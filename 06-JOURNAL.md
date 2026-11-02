# Journal

L’entrée la plus récente est en haut. Chaque entrée indique ce qui a été fait, ce qui reste à faire et les problèmes ouverts.

---

## 2 novembre 2026 · Phase 1 terminée : fusion de astro dans main
**Fait**
- Vérifications avant la fusion : `astro` propre et à jour avec GitHub, `main` inchangée depuis le 30 octobre, `public/data.js` bien suivi par Git.
- Réglages Cloudflare vérifiés. Onglet Production : branche `main`, Build command vide, Deploy command `npx wrangler deploy`. Onglet « Previews Base » inchangé.
- Fusion de `astro` dans `main` (commit de fusion), puis push.
- Premier déploiement de production réussi : `npx wrangler deploy` (wrangler 4.156.2) lance bien le bloc `build` de `wrangler.jsonc` (lignes `[custom build]` dans le journal de Cloudflare). Aucun fichier n’a été renvoyé, sans doute parce qu’ils étaient identiques à ceux de l’aperçu.
- Liste de contrôle de la phase 1 validée en production.

**Reste à faire**
- Lot 1 (corrections de la V1), puis la phase 0 en parallèle ou juste après.

**Problèmes ouverts**
- **wrangler n’est pas dans les dépendances.** `npx` télécharge la dernière version à chaque déploiement, et une nouvelle version pourrait changer le comportement du bloc `build`. Piste : l’ajouter en `devDependencies` à une version fixe. C’est à décider, puisque cela ajoute une dépendance.
- **GitHub Pages et `docs/`.** Si GitHub Pages publie encore depuis `/docs`, supprimer ce dossier mettra l’ancienne adresse hors ligne, alors que la phase 3 prévoit d’y installer une redirection. À trancher avant de supprimer `docs/`.
- Docstring de construire.py périmée (« écrit docs/data.js »).
- Cloudflare, onglet « Previews Base » : la Build command et la Preview command sont toujours ignorées.
- Cloudflare, champ « Production branch » : il doit rester sur `main`.
- Habitude Git : lancer `git status` avant chaque commit et ajouter les fichiers un par un.

**Décisions prises** : abandon des captures d’écran avant/après prévues pour la phase 1, car la liste de contrôle a suffi.

## 2 novembre 2026 · Phase 1 : décisions tranchées, aperçu Cloudflare fonctionnel
**Fait**
- Décisions : D7, D8 et D9 validées et précisées, D6 marquée « numéro non attribué », D10 reportée.
- `npm run build` et `npm run preview` fonctionnent en local.
- La version de test de la branche `astro` se construit et s’affiche sur Cloudflare. Pour y arriver, `wrangler.jsonc` contient désormais :
  - un bloc `"previews": {}`, exigé par `wrangler preview` (commande en bêta) ;
  - un bloc `"build": { "command": "npm run build" }`, car Cloudflare ignorait la commande de build saisie dans l’onglet « Previews Base » (cause inconnue).
- Retrait de deux commandes Git collées par erreur dans `src/pages/index.astro`.
- Liste de contrôle de la phase 1 validée sur l’aperçu : ordinateur, téléphone, modes clair et sombre. Les captures avant/après n’ont pas été faites.

**Reste à faire**
- Fusionner `astro` dans `main` (phase 1, étape 5), puis vérifier le premier déploiement de production. C’est le bloc `build` de wrangler.jsonc qui doit fabriquer dist/ ; la « Build command » de l’onglet Production reste vide.

**Problèmes ouverts**
- Cloudflare, onglet « Previews Base » : la Build command (`npm run build`) et la Preview command (`npm run build && npx wrangler preview`) sont saisies mais ignorées. Si Cloudflare se met à les appliquer, le site sera fabriqué plusieurs fois : sans danger, mais à nettoyer.
- Cloudflare, champ « Production branch » : il a été passé par erreur sur `astro`, puis remis sur `main`. Il doit rester sur `main`.
- `docs/` : inutile après la fusion. À supprimer dans un commit à part, après avoir vérifié si GitHub Pages publie encore depuis `/docs` (GitHub > Settings > Pages). Mettre alors à jour LISEZMOI et ARCHITECTURE.
- Docstring de construire.py périmée (« écrit docs/data.js »).
- Favicon : logo Astro par défaut.
- Défauts et souhaits relevés sur la V1, à traiter après la fusion (voir ROADMAP, « Lots après la phase 1 »).
- Habitude Git : lancer `git status` avant chaque commit et ajouter les fichiers un par un.

## 30 octobre 2026 · Branche astro poussée sur GitHub
**Fait**
- La branche `astro` est créée et poussée sur GitHub. Elle contient un seul commit, « Projet Astro minimal, frise copiée à l’identique ». `main` n’a pas changé.
- `public/` contient app.js, data.js et style.css, identiques à ceux de `docs/`, ainsi que les images, `favicon.svg` et `favicon.ico`.
- `construire.py` écrit désormais `public/data.js`. `docs/data.js` n’est donc plus mis à jour : il reste figé dans son état du 29 octobre, ce qui ne pose pas de problème puisque c’est la version en ligne.
- `wrangler.jsonc` publie désormais `./dist` au lieu de `./docs`.
- Aucun fichier inutile n’a été envoyé : node_modules, .astro et dist sont bien ignorés.

**Problèmes ouverts**
- **Réglages Cloudflare (Settings > Build).** La commande de build doit être `npm run build`, car `./dist` n’est pas dans Git. Les builds de la branche `astro` (« Builds for non-production branches ») doivent être activés pour obtenir une adresse de prévisualisation. Attention : la commande de build est commune à toutes les branches. Une fois réglée, les builds de `main` échoueront tant que `astro` n’y est pas fusionnée. Le site en ligne n’est pas cassé pour autant, car il garde son dernier déploiement réussi. Il ne faut simplement plus pousser sur `main` d’ici la fusion.
- **Favicon.** `favicon.svg` et `favicon.ico` sont les logos par défaut d’Astro. Le navigateur affichera le logo Astro dans l’onglet, alors que la V1 n’avait pas d’icône. À remplacer en phase 3, ou à supprimer d’ici là.
- **Liste de contrôle.** La vérification de la phase 1 n’a pas encore été faite sur `npm run dev` ni sur l’aperçu Cloudflare.

**À faire ensuite** : la liste de contrôle de la phase 1, l’aperçu Cloudflare de la branche `astro`, puis la phase 0 en parallèle.

---

## 30 octobre 2026 · État des lieux avant la migration
**Fait**
- La V1 de la frise est en ligne. Le dernier commit sur `main` s’intitule « Œuvres fondatrices et aperçu au survol ».
- Cloudflare est configuré pour publier `./docs` (`wrangler.jsonc`).
- `python construire.py` fonctionne et ne donne aucun avertissement. Il compte 9 mouvements, 34 artistes, 104 œuvres, 14 événements et 12 influences.
- 43 fiches sont rédigées sur 147 : les 9 mouvements, les 34 artistes, et aucune œuvre pour l’instant.
- Une seule œuvre a une image (`impression-soleil-levant.jpg`).
- **Migration Astro commencée en local, sur `main`, rien n’est encore poussé sur GitHub.**
  - `package.json` : projet « site-astro », Astro ^7.4.4, Node ≥ 22.23.4, scripts dev, build et preview.
  - `astro.config.mjs` : configuration par défaut, vide.
  - `.gitignore` : ignore bien `dist/`, `.astro/` et `node_modules/`.
  - `src/pages/index.astro` : copie fidèle de `docs/index.html`. Les chemins pointent vers la racine (`/style.css`, `/data.js`, `/app.js`) et les trois scripts sont en `is:inline`. Son commentaire indique que style.css, app.js, data.js et images doivent se trouver dans `public/`.
  - `docs/` est identique au dernier commit GitHub : le site en ligne n’est pas touché.

**Problèmes ouverts**
- **LISEZMOI.md est en partie périmé.** Il parle de Google Sheets, d’Obsidian et de GitHub Pages, et renvoie à un bloc `new vis.Timeline` qui n’existe plus dans app.js.
- **Fiches sans ligne dans le tableur.** Ces fiches existent mais ne s’affichent pas, et construire.py ne le signale pas. Les quatre fiches artistes sont des gabarits vides créés avec la première version de la frise (on y trouve encore une section « Sources ») : ce sont sans doute des restes d’éléments retirés du tableur. À confirmer avant de les supprimer.
  - artistes : ball, eluard, manet, miro ;
  - œuvres : capitale-de-la-douleur, karawane, l-amour-la-poesie, la-fuite-hors-du-temps, le-carnaval-d-arlequin, le-dejeuner-sur-l-herbe, liberte, manifeste-dada.
- **Le gabarit œuvre contient une section « Sources »**, contrairement aux fiches mouvements et artistes. À confirmer : est-ce voulu ?

**À faire ensuite** : la phase 0 de la roadmap.

---

<!-- Modèle d’entrée :
## JJ mois AAAA · Titre de la tâche
**Fait** :
**Reste à faire** :
**Problèmes ouverts** :
**Décisions prises** : (à reporter dans DECISIONS)
-->
