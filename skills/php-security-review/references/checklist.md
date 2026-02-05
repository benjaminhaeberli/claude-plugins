# Checklist Securite PHP / Laravel / Kirby

Based on OWASP Top 10 (2021), PHP-specific vulnerabilities, and Laravel/Kirby security best practices. Focused on what a web developer can verify and fix in code.

---

## 1. Injection (OWASP A03) (7 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 1 | Requetes SQL parametrees | CRITIQUE | Pas de concatenation de variables dans les requetes SQL. Utiliser Eloquent, Query Builder, ou PDO prepared statements. Verifier `DB::raw()`, `whereRaw()`, `selectRaw()`, `orderByRaw()` pour l'injection via parametres. |
| 2 | Pas de requetes SQL construites dynamiquement | CRITIQUE | Pas de `$pdo->query("SELECT * FROM users WHERE id = $id")`. Pas de `DB::select("SELECT * FROM users WHERE name = '$name'")`. |
| 3 | Echappement des sorties HTML (XSS) | CRITIQUE | Blade : utiliser `{{ }}` (echappe automatiquement), pas `{!! !!}` sauf pour du contenu HTML de confiance. Vanilla PHP : `htmlspecialchars($var, ENT_QUOTES, 'UTF-8')`. |
| 4 | Protection contre l'injection de commandes OS | CRITIQUE | Pas de `exec()`, `system()`, `shell_exec()`, `passthru()`, `proc_open()` avec des parametres utilisateur. Si necessaire : `escapeshellarg()` + `escapeshellcmd()`. |
| 5 | Protection contre l'injection LDAP/XML/XPath | HAUTE | Si applicable : parametres echappes dans les requetes LDAP/XML. Pas de `simplexml_load_string()` sur des donnees utilisateur sans validation. |
| 6 | Protection contre le Server-Side Template Injection (SSTI) | HAUTE | Pas de rendu dynamique de templates Blade/Twig a partir d'entree utilisateur. Pas de `eval()` ou `Blade::compileString()` sur du contenu non fiable. |
| 7 | Protection contre l'injection dans les headers HTTP | HAUTE | Pas de CRLF injection dans les headers. Les valeurs de redirect ne proviennent pas directement de l'input utilisateur sans validation. |

## 2. Authentification (OWASP A07) (7 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 8 | Mots de passe haches avec bcrypt/Argon2 | CRITIQUE | Laravel : `Hash::make()` (bcrypt par defaut). Jamais `md5()`, `sha1()`, `sha256()` pour les mots de passe. Pas de stockage en clair. |
| 9 | Rate limiting sur le login | CRITIQUE | `ThrottleRequests` middleware ou `RateLimiter` sur les routes d'authentification. Blocage apres X tentatives. |
| 10 | Tokens de session securises | HAUTE | Config session : `secure => true`, `http_only => true`, `same_site => 'lax'`. Regeneration de session apres login (`session()->regenerate()`). |
| 11 | Reinitialisation de mot de passe securisee | HAUTE | Token unique a usage unique avec expiration. Pas de question secrete. Email de reset ne confirme pas l'existence du compte. |
| 12 | MFA propose ou disponible | MOYENNE | Fonctionnalite 2FA disponible (TOTP, SMS). Packages : `laravel/fortify`, `pragmarx/google2fa`. |
| 13 | Messages d'erreur generiques | HAUTE | "Identifiant ou mot de passe incorrect" (pas de distinction). Idem pour la creation de compte et la reinitialisation. Pas d'enumeration d'utilisateurs. |
| 14 | Deconnexion securisee | HAUTE | Invalidation de session cote serveur. Token revoque. Redirection vers une page publique. `session()->invalidate()` + `session()->regenerateToken()`. |

## 3. Controle d'acces (OWASP A01) (7 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 15 | Middleware d'authentification sur les routes protegees | CRITIQUE | Toutes les routes necessitant une connexion ont `auth` middleware. Pas de route admin sans protection. Groupement par middleware. |
| 16 | Autorisation sur chaque action (policies/gates) | CRITIQUE | Laravel : `$this->authorize()`, `Gate::allows()`, `Policy`. Pas de verification par role seulement — verifier aussi l'ownership. Kirby : permissions dans les blueprints. |
| 17 | Pas d'IDOR (Insecure Direct Object Reference) | CRITIQUE | Les endpoints ne permettent pas d'acceder aux ressources d'autres utilisateurs en changeant l'ID. Utiliser `Route Model Binding` + `Policy` ou scope par utilisateur. |
| 18 | Mass assignment protege | HAUTE | Chaque modele Eloquent a `$fillable` (whitelist) defini. Pas de `$guarded = []` (tout autorise). Pas de `Model::create($request->all())` sans validation. |
| 19 | API protegee (tokens, sanctum, passport) | HAUTE | Endpoints API authentifies. Tokens avec expiration. Scopes/abilities definis. Pas de cle API en dur dans le frontend. |
| 20 | Livewire : autorisation sur les actions | HAUTE | Les methodes publiques de composants Livewire verifient les permissions. Les proprietes publiques ne contiennent pas de donnees sensibles. `#[Locked]` sur les proprietes non modifiables. |
| 21 | Kirby : securite du panel et de l'API | HAUTE | Panel accessible uniquement aux utilisateurs autorises. Permissions par role dans les blueprints. API content desactivee ou protegee si non utilisee. |

## 4. Configuration & secrets (OWASP A05) (6 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 22 | Pas de secrets dans le code source | CRITIQUE | Pas de mots de passe, cles API, tokens dans le code PHP, JS, ou les fichiers de config. `.env` pour tout. Verifier l'historique git (`git log -p --all -S 'password'`). |
| 23 | .env non accessible publiquement | CRITIQUE | `.env` dans `.gitignore`. Le serveur web ne sert pas `.env` (test: curl domain.com/.env). `public/` comme document root (pas la racine du projet). |
| 24 | Debug mode desactive en production | CRITIQUE | `APP_DEBUG=false` en production. `APP_ENV=production`. Pas de `dd()`, `dump()`, ou `var_dump()` dans le code commite. Whoops desactive. |
| 25 | APP_KEY unique et non commite | HAUTE | `APP_KEY` present dans `.env`, absent du code et de `.env.example` (valeur vide dans example). Unique par environnement. |
| 26 | Configuration de securite des cookies | HAUTE | `SESSION_SECURE_COOKIE=true`, `SESSION_HTTP_ONLY=true`, `SESSION_SAME_SITE=lax`. En production. |
| 27 | Vite : pas d'exposition de variables sensibles | HAUTE | Seules les variables prefixees `VITE_` sont exposees au frontend. Pas de secrets dans les variables `VITE_*`. Pas de proxy API exposant des endpoints internes. |

## 5. Validation & upload (OWASP A04) (6 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 28 | Validation cote serveur sur tous les inputs | CRITIQUE | Chaque controller/action valide les donnees entrantes (`$request->validate()`, Form Request). Pas de confiance dans la validation JS seule. |
| 29 | Upload de fichiers securise | CRITIQUE | Validation du type MIME reel (pas juste l'extension). Taille maximale definie. Stockage hors du document root (`storage/app/`). Nom de fichier regenere (pas le nom original). |
| 30 | Pas d'inclusion de fichier dynamique | CRITIQUE | Pas de `include($userInput)`, `require($var)`, `file_get_contents($url)` avec des parametres utilisateur. |
| 31 | Validation stricte des types | HAUTE | Utiliser les regles de validation Laravel typees (`integer`, `string`, `email`, `url`). Type hints sur les parametres de methode. Strict types (`declare(strict_types=1)`). |
| 32 | Protection contre le path traversal | HAUTE | Pas de `../` dans les chemins de fichiers. Utiliser `realpath()` et verifier le prefixe. `Storage::` avec des chemins relatifs. |
| 33 | Livewire : validation sur les uploads | HAUTE | `WithFileUploads` avec regles de validation. Taille max, types MIME. Fichiers temporaires nettoyes. |

## 6. Headers & transport (OWASP A02) (6 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 34 | HTTPS obligatoire | CRITIQUE | `URL::forceScheme('https')` ou middleware `\App\Http\Middleware\ForceHttps`. Redirection HTTP -> HTTPS. |
| 35 | CSRF protection active | CRITIQUE | `@csrf` dans chaque formulaire Blade. Middleware `VerifyCsrfToken` actif. Exceptions CSRF justifiees et limitees. Livewire : CSRF auto-gere. |
| 36 | Security headers configures | HAUTE | CSP, X-Content-Type-Options, X-Frame-Options (ou CSP frame-ancestors), Referrer-Policy, Permissions-Policy. Via middleware ou config serveur. |
| 37 | CORS configure correctement | HAUTE | `config/cors.php` : origines specifiques (pas `*` en production). Methods et headers limites au necessaire. |
| 38 | HSTS active | MOYENNE | Header `Strict-Transport-Security` avec `max-age` >= 1 an. `includeSubDomains` si applicable. |
| 39 | Content Security Policy (CSP) | MOYENNE | CSP definie et restrictive. `script-src` sans `unsafe-inline` si possible. Nonces pour les scripts inline. Compatible Vite/Livewire. |

## 7. Dependances & maintenance (OWASP A06) (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 40 | composer audit sans vulnerabilites critiques | CRITIQUE | `composer audit` ne rapporte pas de CVE critique. Dependances majeures a jour. Pas de package abandonne. |
| 41 | npm audit sans vulnerabilites critiques | HAUTE | `npm audit` propre. Dependances frontend (Vite, Tailwind, etc.) a jour. |
| 42 | Version PHP/Laravel supportee | HAUTE | Version PHP et Laravel encore sous support securite. Pas de version en fin de vie. |
| 43 | Packages de sources fiables | MOYENNE | Pas de package provenant de sources non verifiees. Packagist/npmjs.com. Verifier la popularite et la maintenance des packages. |
| 44 | Lock files commites | HAUTE | `composer.lock` et `package-lock.json` (ou equivalent) dans le repo. Builds reproductibles. |

## 8. Logging & monitoring (OWASP A09) (4 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 45 | Journalisation des evenements de securite | HAUTE | Tentatives de connexion (succes/echec), changements de mot de passe, actions admin logues. `Log::warning()`, `Log::critical()`. |
| 46 | Pas de donnees sensibles dans les logs | CRITIQUE | Pas de mots de passe, tokens, cartes bancaires dans les logs. `$hidden` sur les modeles Eloquent. Logger sanitise. |
| 47 | Gestion des erreurs sans fuite d'information | HAUTE | Pages d'erreur generiques en production. Pas de stack trace, chemins de fichiers, requetes SQL visibles. Handler d'exception personnalise. |
| 48 | Rotation et retention des logs | MOYENNE | Logs avec rotation (daily). Retention definie. Pas de logs qui grossissent indefiniment. `LOG_CHANNEL=daily`. |

## 9. PHP specifique (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 49 | Pas de fonctions dangereuses | CRITIQUE | Pas de `eval()`, `assert()` avec des strings, `preg_replace()` avec `/e`, `extract()` sur des donnees utilisateur, `$$var` (variables variables) avec des input utilisateur. |
| 50 | Pas de deserialisation non securisee | CRITIQUE | Pas de `unserialize()` sur des donnees utilisateur. Utiliser `json_decode()`. Si necessaire : `unserialize($data, ['allowed_classes' => false])`. |
| 51 | Fonctions de comparaison strictes | HAUTE | Utiliser `===` au lieu de `==` pour les comparaisons de securite (tokens, mots de passe). Eviter le type juggling. `hash_equals()` pour les comparaisons de hash. |
| 52 | Generateurs de nombres aleatoires securises | HAUTE | `random_bytes()`, `random_int()`, `Str::random()`. Pas de `rand()`, `mt_rand()`, `uniqid()` pour des tokens de securite. |
| 53 | Configuration PHP securisee | MOYENNE | `expose_php = Off`, `display_errors = Off` en production. `open_basedir` configure. `disable_functions` pour les fonctions dangereuses non necessaires. |

---

## Grille de scoring

- **CRITIQUE** = Exploitable, risque de breach ou de compromission (x5)
- **HAUTE** = Risque significatif exploitable sous conditions (x4)
- **MOYENNE** = Defense en profondeur, amelioration recommandee (x3)

**Calcul du score** : Chaque pratique OK multipliee par son poids. Les pratiques NA sont exclues du total.

```
Score = SUM(pratiques_OK * poids) / SUM(pratiques_applicables * poids) * 100
```

## References

- OWASP Top 10 (2021) : https://owasp.org/Top10/
- Laravel Security : https://laravel.com/docs/security
- PHP Security Best Practices
- CWE/SANS Top 25
- Kirby Security : https://getkirby.com/docs/guide/security
