# Checklist GDPR/RGPD pour developpeurs web

Curated from the CNIL developer guide (18 fiches), GDPR articles, and Swiss nLPD principles. Focused on what a web developer can verify and fix in code (HTML/CSS/JS/PHP).

---

## 1. Donnees personnelles & minimisation (6 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 1 | Identifier toutes les donnees personnelles collectees | CRITIQUE | Cartographier chaque formulaire, champ, API endpoint qui collecte des donnees personnelles (nom, email, IP, cookies, identifiants). Inclure les donnees indirectement identifiantes. |
| 2 | Ne collecter que le strict necessaire | HAUTE | Chaque champ de formulaire est justifie par une finalite. Pas de champs optionnels "au cas ou". Pas de collecte de donnees sensibles (sante, religion, orientation) sauf si absolument necessaire avec consentement explicite. |
| 3 | Pseudonymiser les donnees quand possible | MOYENNE | Les identifiants internes ne sont pas directement identifiants. Les logs ne contiennent pas de donnees personnelles en clair. Les exports de donnees sont pseudonymises. |
| 4 | Ne pas utiliser de donnees reelles en dev/test | HAUTE | Les environnements de dev et test utilisent des donnees fictives. Pas de dump de production en staging. Jeux de donnees anonymises pour les tests. |
| 5 | Documenter les categories de donnees traitees | MOYENNE | Un registre ou schema decrit quelles donnees sont collectees, pourquoi, combien de temps, et qui y a acces. |
| 6 | Separer les donnees sensibles des donnees courantes | HAUTE | Les donnees sensibles (sante, biometrie, etc.) sont stockees separement avec des controles d'acces renforces. |

## 2. Consentement & base legale (6 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 7 | Obtenir un consentement explicite avant tout traceur | CRITIQUE | Les cookies/traceurs non essentiels sont bloques AVANT le consentement. Pas de scripts analytics/pub charges par defaut. Le consentement est granulaire (par finalite). |
| 8 | Permettre de refuser aussi facilement qu'accepter | CRITIQUE | Le bouton "Refuser" est aussi visible et accessible que "Accepter" dans la banniere cookies. Pas de dark patterns (couleur, taille, position). |
| 9 | Enregistrer la preuve du consentement | HAUTE | Chaque consentement est log avec : timestamp, version de la politique, methode de collecte, finalites acceptees/refusees. |
| 10 | Permettre le retrait du consentement a tout moment | HAUTE | Un lien/icone persistant sur chaque page permet de re-ouvrir le panneau de consentement. Le retrait est effectif immediatement (scripts desactives). |
| 11 | Identifier la base legale de chaque traitement | MOYENNE | Chaque traitement de donnees est justifie : contrat, consentement, interet legitime, obligation legale. Documente dans la politique de confidentialite. |
| 12 | Ne pas coupler le consentement a un service | HAUTE | L'acces au service ne depend pas de l'acceptation des cookies non essentiels. Pas de "cookie wall" bloquant sans alternative. |

## 3. Information & transparence (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 13 | Politique de confidentialite complete et accessible | CRITIQUE | Presente sur le site, lien dans le footer. Contient : identite du responsable, finalites, base legale, destinataires, duree de conservation, droits des personnes, contact DPO, droit de reclamation (CNIL). |
| 14 | Information au moment de la collecte | HAUTE | Chaque formulaire affiche ou lie vers les informations essentielles : pourquoi ces donnees sont collectees, combien de temps conservees, qui y accede. |
| 15 | Langage clair et accessible | MOYENNE | La politique est redigee en langage simple, pas en jargon juridique. Adaptee au public (attention particuliere si mineurs). |
| 16 | Information sur les cookies et traceurs | HAUTE | La banniere cookies liste les finalites, les tiers, la duree de conservation des cookies. Premier et second niveau d'information disponibles. |
| 17 | Notification en cas de violation de donnees | HAUTE | Un mecanisme existe pour notifier les utilisateurs en cas de breach. Template de notification pret. Procedure de notification a l'autorite (72h). |

## 4. Droits des personnes (7 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 18 | Droit d'acces : consulter ses donnees | CRITIQUE | L'utilisateur peut voir toutes les donnees personnelles detenues a son sujet. Fonctionnalite dans le profil ou sur demande avec delai raisonnable. |
| 19 | Droit de rectification : corriger ses donnees | HAUTE | L'utilisateur peut modifier ses informations personnelles (nom, email, adresse, etc.) directement depuis son profil. |
| 20 | Droit a l'effacement : supprimer son compte | CRITIQUE | Fonctionnalite de suppression de compte qui efface toutes les donnees personnelles. L'effacement propage aux sous-traitants. Donnees supprimees aussi des sauvegardes (ou mecanisme pour ne pas les restaurer). |
| 21 | Droit a la portabilite : exporter ses donnees | HAUTE | L'utilisateur peut telecharger ses donnees dans un format standard lisible (CSV, JSON, XML). |
| 22 | Droit d'opposition : s'opposer au traitement | MOYENNE | L'utilisateur peut s'opposer a certains traitements (marketing, profilage). Mecanisme de desinscription pour les emails. |
| 23 | Droit a la limitation : geler le traitement | MOYENNE | Un administrateur peut mettre des donnees en "quarantaine" : ni lues ni modifiees pendant l'examen d'une contestation. |
| 24 | Contact pour exercer ses droits | HAUTE | Une adresse email ou un formulaire dedie est clairement indique pour exercer les droits. Delai de reponse < 1 mois. |

## 5. Securite des donnees (8 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 25 | HTTPS obligatoire partout | CRITIQUE | TLS 1.2+ sur toutes les pages. Redirection HTTP -> HTTPS. HSTS active. Pas de mixed content. |
| 26 | Mots de passe haches avec algorithme robuste | CRITIQUE | Utiliser bcrypt, Argon2, ou scrypt. Jamais MD5, SHA1, ni stockage en clair. Salage unique par mot de passe. |
| 27 | Protection contre les injections (SQL, XSS, CSRF) | CRITIQUE | Requetes preparees (PDO/prepared statements). Echappement des sorties HTML. Tokens CSRF sur les formulaires. Headers de securite (CSP, X-Frame-Options, etc.). |
| 28 | Controle d'acces et moindre privilege | HAUTE | Roles et permissions differencies. Pas de compte admin par defaut. L'acces aux donnees personnelles est restreint au strict necessaire. |
| 29 | Chiffrement des donnees sensibles au repos | HAUTE | Les donnees sensibles (carte bancaire, sante, etc.) sont chiffrees en base de donnees. Les cles de chiffrement sont stockees separement. |
| 30 | Cookies securises | HAUTE | Flags `Secure`, `HttpOnly`, `SameSite=Strict` (ou Lax) sur les cookies d'authentification. Pas de donnees personnelles dans les cookies. |
| 31 | Limitation des tentatives de connexion | HAUTE | Rate limiting sur le login. Blocage apres X tentatives. CAPTCHA ou delai progressif. MFA propose ou impose pour les comptes sensibles. |
| 32 | Messages d'erreur generiques sur l'authentification | MOYENNE | "Identifiant ou mot de passe incorrect" (ne pas reveler si le compte existe). Idem pour la reinitialisation de mot de passe et la creation de compte. |

## 6. Cookies & traceurs (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 33 | Inventaire de tous les cookies et traceurs | HAUTE | Liste exhaustive : cookies first-party, third-party, localStorage, sessionStorage, fingerprinting. Classification par finalite (essentiel, analytics, pub, social). |
| 34 | Blocage des traceurs avant consentement | CRITIQUE | Les scripts de tracking (Google Analytics, Meta Pixel, etc.) ne se chargent pas tant que l'utilisateur n'a pas consenti. Verifiable dans le code source et le reseau. |
| 35 | Analytics respectueux de la vie privee | MOYENNE | Preferer une solution exemptee de consentement (Matomo configure, Plausible, etc.) ou obtenir le consentement pour GA/autres. Pas de transfert hors UE sans garanties. |
| 36 | Duree de vie des cookies limitee | MOYENNE | Cookies de mesure d'audience : max 13 mois. Donnees collectees conservees max 25 mois. Pas de prorogation automatique a chaque visite. |
| 37 | Pas de cookie wall | HAUTE | L'acces au contenu n'est pas conditionne a l'acceptation des cookies non essentiels. Alternative sans traceurs disponible. |

## 7. Conservation & cycle de vie des donnees (4 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 38 | Duree de conservation definie pour chaque type de donnee | HAUTE | Chaque categorie de donnees a une duree de conservation documentee et justifiee. Ex : logs 6 mois, comptes inactifs 3 ans, factures 10 ans. |
| 39 | Purge automatique des donnees expirees | HAUTE | CRON, TTL en BDD, ou autre mecanisme automatique de suppression. Pas de donnees conservees indefiniment "par defaut". |
| 40 | Anonymisation ou suppression effective | MOYENNE | Les donnees purgees sont reellement supprimees (pas juste un soft delete sans nettoyage). Ou anonymisees de facon irreversible. |
| 41 | Journalisation des suppressions | MOYENNE | Les operations de purge/effacement sont tracees (sans stocker les donnees effacees elles-memes). |

## 8. Tiers & sous-traitants (4 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 42 | Inventaire des services tiers qui recoivent des donnees | HAUTE | Liste de tous les tiers : analytics, paiement, emailing, CDN, captcha, social login, etc. Chacun est mentionne dans la politique de confidentialite. |
| 43 | Bibliotheques et SDK respectueux de la vie privee | HAUTE | Les SDK integres ne collectent pas de donnees pour leur propre compte sans consentement. Verifier les politiques de confidentialite des SDK (ex: reCAPTCHA = soumis au consentement). |
| 44 | Pas de transfert hors UE sans garanties | HAUTE | Si des donnees sont envoyees vers des serveurs hors UE (US, etc.), des garanties sont en place (SCC, adequation, BCR). Documente dans la politique. |
| 45 | Contrats de sous-traitance (Art. 28 RGPD) | MOYENNE | [Non verifiable dans le code] Les contrats avec les sous-traitants incluent les clauses RGPD. Marquer comme "a verifier" dans l'audit. |

## 9. Securite du developpement (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 46 | Pas de secrets dans le code source | CRITIQUE | Pas de mots de passe, cles API, ou tokens en dur dans le code. Utiliser des variables d'environnement ou un vault. Verifier l'historique git. .gitignore couvre .env et fichiers sensibles. |
| 47 | Dependances a jour et sans vulnerabilites connues | HAUTE | `npm audit`, `composer audit`, ou equivalent ne rapporte pas de vulnerabilites critiques. Pas de dependance en fin de vie. |
| 48 | Gestion des erreurs sans fuite de donnees | HAUTE | Les pages d'erreur en production ne revelent pas de stack trace, chemins de fichiers, ou requetes SQL. Mode debug desactive en production. |
| 49 | Journaux (logs) sans donnees personnelles sensibles | HAUTE | Les logs ne contiennent pas de mots de passe, tokens, numeros de carte, ni donnees de sante en clair. Les logs ont une duree de retention definie (6 mois recommande). |
| 50 | Tests de securite reguliers | MOYENNE | [Non verifiable dans le code seul] Presence de tests de securite automatises (SAST, DAST). Audits de penetration documentes. Marquer comme "a verifier". |

---

## Grille de scoring

- **CRITIQUE** = Non-conformite pouvant entrainer des sanctions (x5)
- **HAUTE** = Risque important pour les personnes ou l'organisme (x4)
- **MOYENNE** = Amelioration necessaire mais risque modere (x3)

**Calcul du score** : Chaque pratique OK multipliee par son poids. Les pratiques NA sont exclues du total.

```
Score = SUM(pratiques_OK * poids) / SUM(pratiques_applicables * poids) * 100
```

## References reglementaires

- **RGPD** : Reglement (UE) 2016/679
- **nLPD** : Loi federale suisse sur la protection des donnees (2023)
- **ePrivacy** : Directive 2002/58/CE (cookies et traceurs)
- **CNIL** : Guide RGPD de l'equipe de developpement (18 fiches)
- **Articles cles** : Art. 5 (principes), Art. 6 (bases legales), Art. 7 (consentement), Art. 12-22 (droits), Art. 25 (privacy by design), Art. 28 (sous-traitance), Art. 32 (securite), Art. 33-34 (violations)
