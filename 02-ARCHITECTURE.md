# Architecture

Ce document décrit le site tel qu’il est (partie A) et tel qu’il doit devenir (partie B). Il est mis à jour à la fin de chaque phase de la roadmap.

---

# A. État actuel : V1, statique, fabriquée par Astro (novembre 2026)

## Flux des données
```
donnees/frise_art_moderne_base.xlsx ─┐
                                     ├─> python construire.py ─> public/data.js ─┐
fiches/<type>/<id>.md ───────────────┘                                           ├─> npm run build ─> dist/
src/pages/index.astro + public/ (app.js, style.css, images) ─────────────────────┘
```
Le site lui-même ne lit ni le tableur ni les fiches : il ne lit que `data.js`, produit par `construire.py`. Astro transforme `index.astro` en `dist/index.html` et copie le dossier `public/` tel quel dans `dist/`.

## Arborescence
C’est l’état de la branche `main`, celle qui est en ligne.
```
README.md                   mode d’emploi (installation, travail sur le contenu, publication)
construire.py               tableur + fiches -> public/data.js (Python, librairie openpyxl)
package.json                projet Astro : dépendance astro, scripts dev, build, preview
astro.config.mjs            configuration d’Astro (par défaut, vide)
wrangler.jsonc              configuration Cloudflare : lance npm run build, publie ./dist
.gitignore                  ignore node_modules/, dist/, .astro/
donnees/
  frise_art_moderne_base.xlsx
fiches/
  _gabarits/                modèles : gabarit-mouvement.md, gabarit-artiste.md, gabarit-oeuvre.md
  _en-attente/              fiches gardées en attendant leur ligne dans le tableur (artistes/, oeuvres/)
  mouvements/  artistes/  oeuvres/     une fiche = un fichier <id>.md
src/
  pages/index.astro         structure de la page de la frise (≈ 85 lignes : script du thème, en-tête, panneau, visionneuse)
public/                     copié tel quel dans dist/
  style.css                 apparence (≈ 480 lignes)
  app.js                    fonctionnement de la frise (≈ 840 lignes)
  data.js                   GÉNÉRÉ par construire.py : ne jamais le modifier à la main
  images/
  favicon.svg  favicon.ico  logos par défaut d’Astro (à remplacer en phase 3)
```
Les fiches de `fiches/_en-attente/` n’apparaissent pas sur le site. Quand leur ligne est ajoutée au tableur, il faut les ramener dans `fiches/artistes` ou `fiches/oeuvres`, sans quoi construire.py signale « Fiche manquante ».

## Le tableur
Une ligne compte comme un élément si sa colonne `ID` est remplie. Colonnes lues par `construire.py` :
- **Mouvements** : ID, Nom, Début, Fin, Lieu, Couleur, Résumé court (1 phrase).
- **Artistes** : ID, Nom complet, Naissance, Décès, Nationalité, Rôle, Mouvement principal, Autres mouvements, Phrase-clé (1 phrase).
  - « Autres mouvements » contient des **identifiants de mouvements séparés par des virgules** (`fauvisme, cubisme`).
  - « Nationalité » est attendue au féminin (« française ») : l’étiquette de la vue mouvement affiche « Nationalité française ».
- **Œuvres** : ID, Titre, Artiste (ID), Année, Mouvement, Date affichée, Type, Musée, Ville, Majeure (Bible), Lien image, Résumé court (1 phrase).
- **Événements** : ID, Année, Date précise, Titre, Lieu, Mouvement, Description (1-2 phrases).
- **Influences** : ID, De (ID), Vers (ID), Type, Explication (1-2 phrases).
Les autres colonnes (Fichier fiche, Statut fiche, Nb…, Domaine public ?, Notes / corrections) servent au suivi et ne sont pas lues. L’onglet « Lisez-moi » non plus.
Le tableur contient des listes déroulantes Excel qu’openpyxl ne sait pas lire : d’où l’avertissement `Data Validation extension is not supported`, sans danger puisque construire.py n’enregistre jamais le fichier.

## construire.py
1. Lit les cinq onglets.
2. Pour chaque mouvement, artiste et œuvre, lit `fiches/<dossier>/<id>.md`. Il retire l’en-tête YAML, le premier titre `#` et les commentaires `<!-- -->`. Une fiche qui contient moins d’une vingtaine de caractères de vrai texte est considérée comme vide : le site affiche alors « fiche à venir ».
3. Met en forme les textes affichés, jamais les identifiants :
   - `typo` remplace l’apostrophe droite par l’apostrophe typographique (elle ne s’applique pas au Markdown des fiches) ;
   - `majuscule` met une majuscule à la première lettre de la nationalité et du rôle, sans toucher au reste ;
   - `lire_autres` découpe « Autres mouvements » en liste. Une valeur qui n’est pas un identifiant de mouvement est signalée et gardée en texte, avec une majuscule.
4. Vérifie la cohérence et affiche des avertissements :
   - mouvement, autre mouvement ou artiste inconnu (avec l’identifiant suggéré quand le texte ressemble au nom d’un mouvement) ;
   - fiche manquante, année manquante, identifiant en double ;
   - fiche sans ligne dans le tableur (`verifier_orphelines()`), y compris une fiche rangée dans le mauvais dossier. Les sous-dossiers (`_gabarits`, `_en-attente`) ne sont pas parcourus.
5. Écrit `public/data.js` sous la forme `window.FRISE = { mouvements, artistes, oeuvres, evenements, influences }`.

## Format des données (window.FRISE)
- **mouvement** : id, nom, debut, fin, lieu, couleur, resume, fiche, ficheVide
- **artiste** : id, nom, naissance, deces, nationalite, role, mouvement, autres, phrase, fiche, ficheVide
  - `autres` est une **liste** : des identifiants de mouvements, ou du texte pour une valeur inconnue.
  - `role` est affiché sous le libellé « Disciplines ».
- **oeuvre** : id, titre, artiste, annee, mouvement, dateAffichee, type, musee, ville, majeure (booléen), image, resume, fiche, ficheVide
- **evenement** : id, annee, date, titre, lieu, mouvement, description
- **influence** : de, vers, type, explication

Le champ `fiche` contient le Markdown de la fiche, ou une chaîne vide si elle n’est pas rédigée.

## app.js
- Lit `window.FRISE`. L’objet `REGLAGES`, en tête du fichier, contient les réglages : années affichées (1860 à 1966, départ en 1870), pixels par an et largeur des cartes aux zooms « Détail » et « Moyen », hauteur des lignes, marges de la vue mouvement.
- **Trois vues** (`etat.vue`) :
  - « détail » : la frise complète, un couloir par mouvement, une carte par œuvre (vue par défaut) ;
  - « ensemble » : toute la période sans défilement, une bande par mouvement ;
  - « mouvement » : un mouvement en grand, une ligne par artiste. L’étiquette de chaque artiste affiche son nom, « dates · N œuvres ici », « Nationalité … · Disciplines » et, pour un artiste venu d’un autre mouvement, « Surtout : … » (`etiquetteArtiste()`). Il n’y a plus de ligne de vie.
- **Le zoom « Moyen » n’est pas une quatrième vue** : c’est la vue détail avec `etat.moyen = true`. Seule `fenetre()` en tient compte (28 px par an, cartes de 140 px avec le nom court de l’artiste). `zoomMoyen()` et `niveauAffiche()` calculent le niveau à partir de l’état.
- **Barre du bas** (`#decennies`) :
  - trois boutons de zoom, « Ensemble », « Moyen » et « Détail » (`choisirNiveau()`). Le niveau affiché est en noir ; aucun en vue mouvement ;
  - entre Détail et Moyen, on garde la même année à gauche et le même couloir en haut (`changerZoom()`, `repereVertical()`, `hautRepere()`). Depuis la vue mouvement, Détail, Moyen et « ‹ Toute la frise » ramènent à l’année affichée au bord gauche ;
  - les repères sont refaits à chaque dessin (`remplirReperes()`) : les décennies, ou, en vue mouvement, des années avec un pas de 1, 2, 5 ou 10 ans (au plus 11 cases). Un clic fait défiler la vue ; depuis la vue d’ensemble, il ramène à la frise. Seule la vue détail met une case en noir.
- **Navigation** : glisser à la souris, flèches du clavier, barre du bas, sommaire des mouvements, recherche insensible aux accents, case « Œuvres majeures seulement », bouton « Une œuvre au hasard ». Une fiche ouverte depuis la recherche, le hasard ou un lien centre sa carte à l’écran, aussi verticalement (`hautCentre()`).
- **Fiches** : une bulle d’aperçu au survol d’une œuvre, et un panneau latéral redimensionnable (poignée, clavier, double-clic pour la taille par défaut). Le contenu du panneau est fabriqué par `ficheOeuvre`, `ficheArtiste`, `ficheMouvement` et `ficheEvenement` :
  - artiste : dates en sous-titre, puis Nationalité, Disciplines, Mouvement, Aussi (liens), le texte, les œuvres et les liens ;
  - mouvement : le texte, puis Artistes majeurs, Moments clés, Œuvres majeures, Autres œuvres (avec le nom court de l’artiste à droite de chaque ligne).
- **Index** : le bouton « Index » affiche dans le panneau « Comment lire la frise », les onglets Mouvements, Artistes et Œuvres, une recherche et un compteur (`afficherIndex()`). `etatIndex` retient l’onglet et le texte cherché ; une fiche ouverte depuis l’Index propose « ‹ Retour à l’index ». Pas d’adresse `#index`.
- **Image en grand** : l’image d’une fiche œuvre est dans un bouton (`.agrandir`). Un clic, ou Entrée au clavier, l’ouvre dans le `<dialog id="visionneuse">` d’index.astro avec `showModal()`. Un clic n’importe où la referme ; Échap ne ferme que l’image, pas la fiche.
- **Thème** : le bouton `#bouton-theme` inverse `data-theme` sur `<html>` et mémorise le choix (`localStorage`, clé `theme`). Le thème de départ est posé par le script du `<head>` d’index.astro.
- **Liens internes** : dans les fiches, `[[id]]` ou `[[id|texte]]` devient un lien `#id`. L’adresse de la page prend la forme `#id` quand une fiche est ouverte, et une adresse en `#id` ouvre directement la fiche.
- Le Markdown est converti dans le navigateur par **marked**, chargé depuis jsDelivr (version 18.0.14).
- **Accessibilité** : attributs ARIA, focus visible, navigation au clavier, respect de `prefers-reduced-motion`. Le panneau coupe son `aria-live` pendant l’Index. À préserver.

## style.css
- **Rangé par élément de la page**, dans l’ordre où on les rencontre : variables, base, en-tête, défilement et axe, couloirs et étiquettes, pistes et bandes, cartes et événements, vue d’ensemble, bandeau de la vue mouvement, barre du bas, bulles, panneau, fiches, image en grand, Index. Chaque section réunit la disposition et l’apparence de son élément.
- **Version téléphone** en dessous de 720 px : chaque section a son bloc `@media (max-width: 720px)`, juste après ses règles.
- **Mouvement réduit** : le bloc `prefers-reduced-motion` est en toute fin de fichier. À poids égal, c’est la règle écrite en dernier qui gagne : placé plus haut, il serait battu par les transitions.
- Une nouvelle règle va dans la section de son élément, jamais en fin de fichier.
- Couleurs et polices définies par des variables CSS sur `:root`. Mode sombre par `:root[data-theme="dark"]`. L’attribut est toujours posé par le script du `<head>` d’index.astro : d’après le choix mémorisé, sinon d’après le réglage de l’appareil. Un seul bloc sombre suffit donc.
- Police Archivo (Google Fonts). La couleur de chaque mouvement vient du tableur.

## Déploiement
À chaque push, Cloudflare installe les dépendances, puis lance la Deploy command `npx wrangler deploy`. Wrangler lit le bloc `build` de `wrangler.jsonc`, lance `npm run build`, puis publie `./dist`. Dans le tableau de bord, la Build command de l’onglet Production reste vide, sans quoi le site serait fabriqué deux fois.
- Branche de production : `main`. Adresse : https://frise-chronologique---art-moderne.tjimblet.workers.dev
- Les autres branches ont une version de test (encadré « Previews »), publiée par `wrangler preview` (commande en bêta) grâce au bloc `"previews": {}`.
- wrangler n’est pas dans les dépendances : `npx` télécharge la dernière version à chaque déploiement (voir JOURNAL).
- GitHub Pages est désactivé : l’ancienne adresse en github.io renvoie une erreur 404 (D11).

---

# B. Architecture cible : Astro, une page par fiche (phase 2)

Toutes les décisions de DECISIONS sont tranchées, sauf D10 (droits des images), qui ne touche pas à l’architecture. Ce plan sera précisé au début de la phase 2.

## Principes
1. **Les sources ne bougent pas.** Le tableur et les fiches restent la source de vérité et se modifient comme aujourd’hui.
2. **construire.py reste la porte d’entrée des données** et garde toutes ses vérifications (D7). À partir de la phase 2, il écrira `src/data/frise.json` au lieu de `public/data.js`.
3. **Astro génère des pages HTML statiques** : la frise, plus une page par fiche. Pas de serveur ni de base de données.
4. **La frise reste en JavaScript simple** : `app.js` devient un ou plusieurs scripts de la page de la frise, sans réécriture.
5. **Aucun framework d’interface** (React, Vue…) ni de CSS (Tailwind…).

## Arborescence visée
```
astro.config.mjs  package.json  .gitignore  README.md
construire.py                 écrit src/data/frise.json
donnees/  fiches/             inchangés
public/                       copié tel quel : images, favicon
src/
  data/frise.json             GÉNÉRÉ, commité (D8)
  layouts/Base.astro          <head>, polices, script du thème, éléments communs à toutes les pages
  pages/
    index.astro               la frise (ou la page d’accueil, voir ci-dessous)
    mouvements/[id].astro     une page par mouvement   -> /mouvements/fauvisme/
    artistes/[id].astro       une page par artiste     -> /artistes/matisse/
    oeuvres/[id].astro        une page par œuvre       -> /oeuvres/guernica/
  scripts/                    ancien app.js, découpé en plusieurs fichiers (navigation, vues, fiches, recherche, Index…)
  styles/style.css            ancien style.css
  lib/                        fonctions partagées (conversion des liens [[id]], Markdown, contenu des fiches)
```

## Points à trancher au début de la phase 2
- **Données** : comment les scripts de la frise reçoivent `frise.json` (import du JSON ou injection dans la page).
- **Markdown** : `marked` reste-t-il chargé depuis le CDN, ou le Markdown est-il converti au moment du build ?
- **Liens** : les liens `[[id]]` doivent mener vers `#id` dans le panneau de la frise et vers `/artistes/id/` sur les pages. Une seule fonction partagée doit gérer les deux cas.
- **Contenu des fiches** : il est aujourd’hui fabriqué dans app.js (`ficheArtiste`, `ficheMouvement`…). Les pages devront le reprendre sans le dupliquer, par exemple avec des fonctions partagées dans `src/lib/`.
- **Fiches non rédigées** : ont-elles une page ?
- **Page d’accueil** : reste-t-elle la frise, ou devient-elle une page qui y mène (voir ROADMAP, « Idées à trier ») ? Dans le second cas, `index.astro` devient la page d’accueil et la frise a sa propre adresse, qui devra rester stable (D9).
- **Découpage d’app.js** : en profiter pour ranger les outils partagés (`compter()`, la hauteur de l’axe calculée à deux endroits) et renommer `majDecennies()`, `.decennie` et `#decennies`, qui désignent aussi des années en vue mouvement.

## Phase 5
L’organisation des fichiers pour la version anglaise (`/en/…`) est à décider avant de commencer, pas pendant.

## Déploiement visé
C’est déjà le cas depuis la phase 1 : Cloudflare lance `npm run build` (via `wrangler.jsonc`) et publie le dossier `./dist`. Comme `frise.json` sera commité, Cloudflare n’aura besoin ni de Python ni du tableur.
