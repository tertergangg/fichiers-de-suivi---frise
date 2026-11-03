# Journal

L’entrée la plus récente est en haut. Chaque entrée indique ce qui a été fait, ce qui reste à faire et les problèmes ouverts.

---

## 3 novembre 2026 · Lot 3 : couleurs et bandeau du haut (étapes 1 et 2a)
**Fait**
- **Couleurs, mode clair.** Fond papier ambré `#F5EEDF`. Cartes, panneau et recherche en ivoire `#FAF7F0` (`--carte-fond`, `--surface`), un cran plus clair que le fond pour rester lisibles sur les bandes de couleur. `--surface-2` passe à `#EBE1CC`.
- **Couleurs, mode sombre.** Fond brun `#1E1A16`, surfaces `#27221D`, `--surface-2` `#332C25`, texte et bordures blanc cassé `#F2EBDD`, texte doux `#B8AE9F`. Les ombres sont brunes (`#574B3E`), un peu plus claires que le fond, pour garder le relief « affiche ». Les deux blocs du mode sombre ont été modifiés à l’identique.
- **Bandeau compact.** L’en-tête est une grille de deux colonnes : titre | outils, puis sommaire | case « Œuvres majeures seulement ». Le titre passe de 2,6–4,6 rem à 2,2–3 rem. Les dates deviennent un sous-titre (`.sous-titre-site`) : « 1874–1960 · Une histoire en mouvements ». Le texte d’aide est retiré : il reviendra dans le tiroir Index (étape 2b). Le bandeau a ensuite été légèrement agrandi, boutons des mouvements compris. Les règles `.aide` et `.entete h1 span` ont été supprimées.
- **Bouton « Une œuvre au hasard »** (`#hasard`). Il tire une œuvre qui a une carte sur la frise (une année et un mouvement connu), seulement parmi les majeures si la case est cochée, jamais deux fois de suite la même. Il repasse en vue détail si besoin, puis ouvre la fiche.
- **Défilement vertical.** `ouvrir(id, true)` centre désormais aussi la carte verticalement : nouvelle fonction `hautCentre()`, et paramètre `haut` ajouté à `allerAnnee()`. La recherche et les liens entre fiches en profitent aussi.
- Sur téléphone, la liste des résultats de recherche est accrochée à gauche, car accrochée à droite elle risquait de déborder de l’écran.
- La contradiction de la ROADMAP est tranchée : le lot 3 passe avant la phase 0.
- Proposition écartée : des chiffres-clés (« 9 mouvements · 34 artistes · 104 œuvres ») dans le bandeau. La place revient aux boutons prévus, et le compteur ira dans l’Index.
- Branche `corrections-lot3-v1`, testée en local, pas encore fusionnée.

**Reste à faire**
- Lot 3, étapes 2b (tiroir Index), 2c (bouton clair/sombre), 3, 4, 5 et 6 (voir ROADMAP). L’étape 2b se fera dans une nouvelle conversation.
- Ne pas fusionner dans `main` avant l’étape 2b : le texte d’aide a quitté le bandeau et n’est pas encore dans l’Index.

**Problèmes ouverts**
- **Téléphone mis de côté** jusqu’au lot 4 (voir ROADMAP). Les règles téléphone de l’étape 2a sont provisoires.
- **style.css**, à ajouter au rangement de la phase 0 :
  - les deux blocs du mode sombre portent les mêmes valeurs : chaque changement doit être fait deux fois ;
  - `--fond-etiquette` n’est utilisée nulle part ;
  - la taille de `.filtre .pastille` est définie deux fois.
- L’alignement de la case « Œuvres majeures seulement » avec la première rangée de mouvements tient à un `margin-top: 0.3rem` réglé à la main. Si la taille des boutons des mouvements change, il faudra le revoir.
- **app.js** dépasse 570 lignes et va encore grossir avec l’Index. Découpage prévu en phase 2.
- Les problèmes ouverts des entrées précédentes restent valables : wrangler hors des dépendances, `docs/` et GitHub Pages, docstring de construire.py, onglet « Previews Base » de Cloudflare, favicon, avertissement de Seurat, HTML des fiches qui grossit dans app.js.

**Décisions prises** : aucune. Proposition écartée : les chiffres-clés dans le bandeau.

## 3 novembre 2026 · Exploration de design avec Lovable
**Fait**
- Question posée : reconstruire le site avec Lovable ? Réponse : non. Lovable produit du React, du TypeScript et du Tailwind, contraires à D1 et à l’architecture ; le design (D3) serait réécrit ; il pousse vers une base de données en ligne (contraire à D7) ; le référencement et le coût sont incertains ; la phase 1 serait perdue.
- Lovable utilisé comme carnet de croquis : import du site par son adresse, liste d’idées, quelques maquettes (bandeau avec bouton Index, page d’accueil, tiroir Index, galerie d’œuvres, page mouvement, cartel pour les œuvres sans image). Rien de son code n’est repris.
- Tri des idées : le fond, le mode sombre, le bandeau du haut, « Une œuvre au hasard » (validé) et le tiroir Index vont dans le lot 3 ; le reste va dans la nouvelle section « Idées à trier » de la ROADMAP. Idée écartée : remplacer les cartes par des compteurs par décennie pour le zoom intermédiaire.

**Reste à faire**
- Lot 3, dans une nouvelle conversation.
- Une conversation dédiée, avant la phase 2, pour trier les « Idées à trier ».

**Problèmes ouverts**
- **La ROADMAP se contredit** : « le lot 3 passe avant la phase 0 » en haut, mais le lot 3 est annoncé « après le rangement de style.css en phase 0 ». À trancher au début du lot 3. Le lot 3 modifie les variables de style.css, d’où l’intérêt de ranger d’abord.
- Crédits Lovable épuisés : les maquettes du bandeau, des étiquettes d’artistes, du zoom intermédiaire, des pages (accueil, mouvement, artiste, œuvre, À propos) et des sections de contenu n’ont pas été faites. Les trois messages prêts à envoyer sont dans la conversation du 3 novembre.
- Garder les captures des maquettes Lovable en local (hors du dépôt) pour le lot 3 et la conversation de tri.
- Les problèmes ouverts des entrées précédentes restent valables : wrangler hors des dépendances, `docs/` et GitHub Pages, docstring de construire.py, onglet « Previews Base » de Cloudflare, favicon, avertissement de Seurat, HTML des fiches qui grossit dans app.js.

**Décisions prises** : aucune. Proposition écartée : reconstruire le site avec Lovable.

## 2 novembre 2026 · Lot 2 terminé : fiches
**Fait**
- **Fiche artiste.** La nationalité quitte le sous-titre (qui ne garde que les dates) pour une rubrique « Nationalité ». « Rôle » s’affiche « Disciplines » ; la colonne « Rôle » et le champ `role` ne changent pas. Nouvelle fonction `majuscule()` dans construire.py, appliquée à la nationalité et au rôle (et non `capitalize()`, qui mettrait le reste en minuscules).
- **« Aussi » cliquable.** La colonne « Autres mouvements » contient des identifiants séparés par des virgules. Nouvelle fonction `lire_autres()` dans construire.py : elle en fait une liste, signale chaque valeur inconnue (en suggérant l’identifiant quand le texte ressemble au nom d’un mouvement) et la garde en texte simple avec une majuscule. Le champ `autres` de data.js devient une liste. Dans le tableur : `surrealisme` pour tzara et arp ; « surréalisme (proche) » retiré pour picasso ; « néo-impressionnisme » gardé en texte pour seurat, en attendant l’ajout du mouvement.
- **Fiche mouvement.** « Artistes majeurs » ; « Œuvres majeures » puis « Autres œuvres ». Dans ces listes, le nom court de l’artiste est sur la même ligne, calé à droite (classe `avec-artiste`, `listeOeuvres()` devenue une déclaration de fonction).
- **Libellés des fiches** (`.fiche dt`) passés de 0,8 à 0,9 rem.
- **Image en grand.** Un `<dialog id="visionneuse">` dans index.astro, ouvert par `showModal()`. L’image du panneau est dans un bouton invisible (`.agrandir`) pour s’ouvrir aussi au clavier. Un clic n’importe où la ferme ; Échap ne ferme que l’image, pas la fiche.
- Branche `corrections-lot2-v1` testée en local, poussée, fusionnée dans `main`.

**Reste à faire**
- Phase 0, à faire avant le lot 3 : le lot 3 modifiera style.css, qu’il vaut mieux ranger d’abord.

**Problèmes ouverts**
- `python construire.py` garde un avertissement (Seurat, « néo-impressionnisme ») tant que ce mouvement n’est pas dans l’onglet Mouvements. Voir ROADMAP, « Plus tard ».
- openpyxl affiche `UserWarning: Data Validation extension is not supported and will be removed`. C’est sans danger : le tableur contient des listes déroulantes, et construire.py ne fait que lire le fichier, sans jamais l’enregistrer.
- Le champ `role` est affiché sous le libellé « Disciplines » : petit décalage de nom, à garder en tête.
- Le HTML des fiches grossit dans app.js, et la phase 2 devra le reproduire dans les pages Astro. À prévoir dans `src/lib/`.
- style.css : le bloc « Image d’une œuvre en grand » s’ajoute en fin de fichier. Le rangement de la phase 0 devient plus pressant.
- Noms courts : si deux artistes d’un même mouvement partagent un nom de famille, les listes de la fiche mouvement les confondront. `NOMS_COURTS` permet de corriger.
- Les problèmes ouverts des entrées précédentes restent valables : wrangler hors des dépendances, `docs/` et GitHub Pages, docstring de construire.py, onglet « Previews Base » de Cloudflare, favicon.

**Décisions prises** : aucune. Propositions écartées : ajouter `~$*` au `.gitignore` ; signaler dans construire.py une œuvre non majeure dont la fiche est rédigée ; noter dans CONVENTIONS l’exception `rgba` du voile `::backdrop`.

## 2 novembre 2026 · Lot 1 terminé : corrections de la V1
**Fait**
- Bulle d’aperçu : elle disparaît désormais quand on quitte une œuvre. Cause : `.bulle.riche { display: flex }` écrasait `.bulle[hidden] { display: none }` (même spécificité, c’est la dernière règle écrite qui gagne). Correction : ajout du sélecteur `.bulle.riche[hidden]`.
- Poignée du panneau : le panneau ne défile plus. C’est un nouveau div, `#interieur-panneau`, qui défile à sa place : la poignée reste donc visible. `ouvrir()` remet ce div en haut à chaque ouverture.
- Croix du panneau : le carré garde sa taille (1,75 rem). La croix est plus grande et dessinée en CSS avec deux traits, car le « × » de la police Archivo n’était pas centré.
- Branche `corrections-v1` testée en local, puis sur l’aperçu Cloudflare, puis en production après la fusion dans `main`.

**Reste à faire**
- Lot 2 (fiches), avec la phase 0 en parallèle.

**Problèmes ouverts**
- `style.css` : `.fermer` est défini à trois endroits et `.fiche .corps` à deux. Les blocs « Ajouts de la version N » empilés ont causé le bug de la bulle. Un rangement est à prévoir, sans changement visuel.
- Les problèmes ouverts de l’entrée précédente restent valables : wrangler hors des dépendances, `docs/` et GitHub Pages, docstring de construire.py, onglet « Previews Base » de Cloudflare, favicon.

**Décisions prises** : aucune.

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
