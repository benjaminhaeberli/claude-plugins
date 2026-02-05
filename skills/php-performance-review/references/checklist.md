# Checklist Performance PHP / Laravel / Kirby

Best practices for PHP performance auditing from the codebase. Specialized for Laravel, Kirby CMS, Livewire, Vite, Tailwind CSS, and SQL databases.

---

## 1. Requetes SQL & Eloquent (8 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 1 | Pas de requete N+1 | CRITIQUE | Les relations accedees dans des boucles/vues sont eager-loaded (`with()`, `load()`). Verifier les `@foreach` Blade qui accedent a `$model->relation`. Utiliser `Model::preventLazyLoading()` en dev. |
| 2 | Index sur les colonnes filtrees/triees/jointes | CRITIQUE | `WHERE`, `ORDER BY`, `GROUP BY`, `JOIN ON` : les colonnes ont un index. Verifier les migrations pour les `->index()`, `->unique()`, colonnes `_id`. |
| 3 | Select uniquement les colonnes necessaires | HAUTE | `->select(['id', 'name', 'email'])` au lieu de `SELECT *`. Surtout sur les requetes avec beaucoup de colonnes TEXT/JSON. `->without()` pour exclure les relations par defaut. |
| 4 | Pagination des resultats | HAUTE | Les listes utilisent `->paginate()` ou `->cursorPaginate()`. Pas de `->get()` sans limite sur des tables potentiellement larges. API : pagination obligatoire. |
| 5 | Chunking pour les traitements de masse | HAUTE | `->chunk()`, `->chunkById()`, ou `->lazy()` pour traiter de grands volumes. Pas de `->get()` sur 10k+ lignes chargees en memoire. |
| 6 | Requetes optimisees (sous-requetes, exists) | HAUTE | `whereHas()` avec contraintes plutot que charger la relation. `->exists()` plutot que `->count() > 0`. `->value('col')` plutot que `->first()->col`. |
| 7 | Pas de requete dans les boucles | CRITIQUE | Pas de `Model::find()`, `->save()`, ou `DB::` dans un `foreach`/`for`. Utiliser `upsert()`, `insert()`, `update()` en masse. |
| 8 | Utiliser les index composites quand pertinent | MOYENNE | Les requetes avec plusieurs colonnes en WHERE/ORDER BY ont des index composites. L'ordre des colonnes dans l'index correspond a l'ordre d'utilisation. |

## 2. Cache (7 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 9 | Cache sur les donnees frequemment lues | CRITIQUE | Donnees calculees/agregees mises en cache (`Cache::remember()`). Resultats de requetes couteuses caches. TTL adapte a la volatilite. |
| 10 | Cache invalidation coherente | HAUTE | Le cache est invalide quand les donnees changent. Utiliser les model events ou observers. Tags de cache pour l'invalidation groupee. |
| 11 | Driver de cache adapte a la production | HAUTE | Redis ou Memcached en production. Pas de driver `file` ou `array` en production pour les applications a fort trafic. |
| 12 | Config, routes et vues cachees en production | HAUTE | `config:cache`, `route:cache`, `view:cache` executes en deploiement. Event/icon cache si applicable. |
| 13 | Cache HTTP (ETags, Cache-Control) | MOYENNE | Headers de cache sur les reponses statiques et semi-statiques. `Cache-Control` adapte (public vs private, max-age). ETags pour la validation. |
| 14 | Kirby : cache de pages active | HAUTE | Cache des pages active en production (`'cache' => ['pages' => ['active' => true]]`). Exclusions configurees pour les pages dynamiques. |
| 15 | Cache des relations comptees | MOYENNE | `withCount()` cache ou `Counter Cache` pour les compteurs frequemment affiches (nombre de commentaires, likes, etc.). |

## 3. Jobs, queues & traitements asynchrones (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 16 | Operations lourdes dans des jobs | HAUTE | Envoi d'emails, generation de PDF/rapports, traitement d'images, appels API externes : dans un Job/Queue, pas dans le cycle requete/reponse. |
| 17 | Configuration des queues en production | HAUTE | Driver de queue : Redis, SQS, ou database (pas `sync` en production). Workers configures et supervises (Horizon, Supervisor). |
| 18 | Retry et gestion des echecs | MOYENNE | `$tries`, `$backoff`, `$timeout` definis sur les jobs. Failed jobs table configuree. Alerte sur les echecs. |
| 19 | Scheduled tasks optimisees | MOYENNE | Les taches CRON ne dupliquent pas le travail. `withoutOverlapping()` pour les taches longues. `onOneServer()` en multi-serveur. |
| 20 | Batch processing pour les operations multiples | MOYENNE | `Bus::batch()` pour les operations paralleles. Chunk + dispatch pour les gros volumes. Progress tracking si UI. |

## 4. PHP & OPcache (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 21 | OPcache active en production | CRITIQUE | `opcache.enable=1`. `opcache.validate_timestamps=0` en production (redeploy pour invalider). `opcache.memory_consumption` suffisant. |
| 22 | Preloading PHP si applicable | MOYENNE | `opcache.preload` configure avec les classes les plus utilisees. Laravel : `php artisan optimize` genere le fichier de preload. |
| 23 | PHP version recente | HAUTE | PHP 8.2+ pour les gains de performance (readonly, fibers, JIT). Chaque version majeure apporte ~5-15% de perf. |
| 24 | Pas d'operations couteuses repetees | HAUTE | Pas de `file_get_contents()`, `json_decode()`, `preg_match()` sur les memes donnees dans une boucle. Mettre en variable. |
| 25 | Utilisation efficace de la memoire | HAUTE | `ini_get('memory_limit')` adapte. Generators (`yield`) pour les gros jeux de donnees. Pas de tableaux de 100k+ elements en memoire. `unset()` des variables lourdes. |

## 5. Livewire (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 26 | Composants Livewire legers | HAUTE | Les proprietes publiques ne contiennent pas de collections Eloquent entieres. Utiliser des IDs + requete dans le render. Payload de serialisation minimal. |
| 27 | Lazy loading des composants lourds | HAUTE | `<livewire:component lazy />` pour les composants non critiques. Placeholder pendant le chargement. |
| 28 | Polling optimise | HAUTE | Pas de `wire:poll` sans necessite. Intervalle adapte (pas moins de 5s). `wire:poll.visible` pour ne poll que si visible. Preferer les events ou Echo. |
| 29 | Debounce sur les inputs | MOYENNE | `wire:model.debounce.300ms` ou `wire:model.lazy` sur les champs de recherche/filtre. Pas de requete a chaque frappe. |
| 30 | Pagination Livewire | MOYENNE | Utiliser `WithPagination` pour les listes. Pas de chargement complet d'une collection pour la filtrer en PHP. |

## 6. Frontend & assets (7 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 31 | Vite : build de production optimise | HAUTE | `npm run build` genere des assets minifies. Code splitting actif. Tree-shaking fonctionnel. Pas de source maps en production. |
| 32 | Tailwind CSS : purge active | CRITIQUE | `content` configure dans `tailwind.config.js` pour purger les classes inutilisees. CSS final < 20Ko gzippe en general. |
| 33 | Lazy loading images et iframes | HAUTE | `loading="lazy"` sur les images/iframes sous la ligne de flottaison. Images above-the-fold sans lazy loading. |
| 34 | Images optimisees (format, srcset) | HAUTE | Formats modernes (WebP/AVIF). `srcset` pour le responsive. Pas d'image 4000px affichee en 400px. Kirby : thumbs configures. |
| 35 | Fonts optimisees | MOYENNE | WOFF2. `font-display: swap`. Preload des polices critiques. Max 2-3 familles. Preferer les system fonts si possible. |
| 36 | JS et CSS non bloquants | HAUTE | Scripts avec `defer`. CSS critique inline. Le reste en async. Pas de render-blocking dans le `<head>`. |
| 37 | Preconnect / preload strategiques | MOYENNE | `<link rel="preconnect">` vers les origines tierces (CDN, fonts, API). `<link rel="preload">` pour les ressources critiques (hero image, font principale). |

## 7. API & reponses (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 38 | API Resources avec champs selectionnes | HAUTE | `JsonResource` / `ResourceCollection` qui ne renvoient que les champs necessaires. Pas de `$model->toArray()` brut. Relations conditionnelles (`whenLoaded()`). |
| 39 | Pagination sur les endpoints de liste | CRITIQUE | Tout endpoint retournant une collection est pagine. Pas de `->get()` sans limite cote API. Cursor pagination pour les grandes tables. |
| 40 | Compression des reponses | MOYENNE | Gzip/Brotli active sur les reponses JSON et HTML. Verifiable via middleware ou config serveur. |
| 41 | Rate limiting sur les API | HAUTE | `throttle` middleware sur les routes API. Limites adaptees par endpoint. Headers `X-RateLimit-*` retournes. |
| 42 | Pas d'appels API redondants | HAUTE | Pas d'appels en boucle vers des services externes. Batch quand possible. Cache sur les reponses externes. Circuit breaker pour les services instables. |

## 8. Base de donnees & migrations (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 43 | Types de colonnes adaptes | HAUTE | Pas de `TEXT` pour un code postal, pas de `BIGINT` pour un booleen. `UNSIGNED` sur les IDs. `VARCHAR(255)` seulement si necessaire. |
| 44 | Cles etrangeres avec index | HAUTE | Les colonnes `*_id` ont un index. `->constrained()` dans les migrations pour l'integrite referentielle. |
| 45 | Soft deletes indexes | MOYENNE | Si `SoftDeletes` utilise : index sur `deleted_at`. Les requetes frequentes excluent les enregistrements supprimes efficacement. |
| 46 | Transactions pour les operations multiples | HAUTE | `DB::transaction()` pour les operations qui doivent etre atomiques. Pas de risque d'etat inconsistant si une operation echoue. |
| 47 | Database connection pooling | MOYENNE | Connexion persistante ou pooling en production. `MYSQL_ATTR_INIT_COMMAND` pour le charset. Timeouts configures. |

---

## Grille de scoring

- **CRITIQUE** = Goulot d'etranglement majeur, impact direct sur le temps de reponse (x5)
- **HAUTE** = Impact significatif sur la performance sous charge (x4)
- **MOYENNE** = Optimisation utile mais impact modere (x3)

**Calcul du score** : Chaque pratique OK multipliee par son poids. Les pratiques NA sont exclues du total.

```
Score = SUM(pratiques_OK * poids) / SUM(pratiques_applicables * poids) * 100
```

## References

- Laravel Performance Best Practices
- MySQL/MariaDB Query Optimization
- PHP OPcache Documentation
- Livewire Performance Tips
- Kirby Caching Documentation
- Vite Build Optimization
