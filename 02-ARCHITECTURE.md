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
LISEZMOI.md                 mode d’emploi (en partie périmé, voir JOURNAL)
construire.py               tableur + fiches -> public/data.js (Python, librairie openpyxl)
package.json                projet Astro : dépendance astro, scripts dev, build, preview
astro.config.mjs            configuration d’Astro (par défaut, vide)
wrangler.jsonc              configuration Cloudflare : lance npm run build, publie ./dist
.gitignore                  ignore node_modules/, dist/, .astro/
donnees/
  frise_art_moderne_base.xlsx
fiches/
  _gabarits/                modèles : gabarit-mouvement.md, gabarit-artiste.md, gabarit-oeuvre.md
  mouvements/  artistes/  oeuvres/     une fiche = un fichier <id>.md
src/
  pages/index.astro         structure de la page de la frise (≈ 60 lignes, dont la visionneuse d’images)
public/                     copié tel quel dans dist/
  style.css                 apparence (≈ 365 lignes)
  app.js                    fonctionnement de la frise (≈ 560 lignes)
  data.js                   GÉNÉRÉ par construire.py : ne jamais le modifier à la main
  images/
  favicon.svg  favicon.ico  logos par défaut d’Astro (à remplacer en phase 3)
docs/                       ancienne V1, figée au 29 octobre, à supprimer (voir JOURNAL)
```

## Le tableur
Une ligne compte comme un élément si sa colonne `ID` est remplie. Colonnes lues par `construire.py` :
- **Mouvements** : ID, Nom, Début, Fin, Lieu, Couleur, Résumé court (1 phrase).
- **Artistes** : ID, Nom complet, Naissance, Décès, Nationalité, Rôle, Mouvement principal, Autres mouvements, Phrase-clé (1 phrase).
  - « Autres mouvements » contient des **identifiants de mouvements séparés par des virgules** (`fauvisme, cubisme`).
- **Œuvres** : ID, Titre, Artiste (ID), Année, Mouvement, Date affichée, Type, Musée, Ville, Majeure (Bible), Lien image, Résumé court (1 phrase).
- **Événements** : ID, Année, Date précise, Titre, Lieu, Mouvement, Description (1-2 phrases).
- **Influences** : ID, De (ID), Vers (ID), Type, Explication (1-2 phrases).
Les autres colonnes (Fichier fiche, Statut fiche, Nb…, Domaine public ?, Notes / corrections) servent au suivi et ne sont pas lues. L’onglet « Lisez-moi » non plus.
Le tableur contient des listes déroulantes Excel qu’openpyxl ne sait pas lire : d’où l’avertissement `Data Validation extension is not supported`, sans danger puisque construire.py n’enregistre jamais le fichier.

## construire.py
1. Lit les cinq onglets.
2. Pour chaque mouvement, artiste et œuvre, lit `fiches/<dossier>/<id>.md`. Il retire l’en-tête YAML, le premier titre `#` et les commentaires `<!-- -->`. Une fiche qui contient moins d’une vingtaine de caractères de vrai texte est considérée comme vide : le site affiche alors « fiche à venir ».
3. Met en forme les textes affichés, jamais les identifiants :
   - `typo` remplace l’apostrophe droite par l’apostrophe typographique ;
   - `majuscule` met une majuscule à la première lettre de la nationalité et du rôle, sans toucher au reste ;
   - `lire_autres` découpe « Autres mouvements » en liste. Une valeur qui n’est pas un identifiant de mouvement est signalée et gardée en texte, avec une majuscule.
4. Vérifie la cohérence et affiche des avertissements : mouvement, autre mouvement ou artiste inconnu (avec l’identifiant suggéré quand le texte ressemble au nom d’un mouvement), fiche manquante, année manquante, identifiant en double.
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
- Lit `window.FRISE`. L’objet `REGLAGES`, en tête du fichier, contient les réglages : années affichées (1860 à 1966, départ en 1870), pixels par an, tailles des cartes et des lignes, marges de la vue mouvement.
- **Trois vues** : « détail » (la frise complète, une carte par œuvre, vue par défaut), « ensemble » (toute la période sans défilement) et « mouvement » (un mouvement en grand, une ligne par artiste).
- **Navigation** : glisser à la souris, flèches du clavier, barre des décennies, sommaire des mouvements, recherche insensible aux accents, case « Œuvres majeures seulement ».
- **Fiches** : une bulle d’aperçu au survol d’une œuvre, et un panneau latéral redimensionnable (poignée, clavier, double-clic pour la taille par défaut). Le contenu du panneau est fabriqué par `ficheOeuvre`, `ficheArtiste`, `ficheMouvement` et `ficheEvenement` :
  - artiste : dates en sous-titre, puis Nationalité, Disciplines, Mouvement, Aussi (liens), le texte, les œuvres et les liens ;
  - mouvement : le texte, puis Artistes majeurs, Moments clés, Œuvres majeures, Autres œuvres (avec le nom court de l’artiste à droite de chaque ligne).
- **Image en grand** : l’image d’une fiche œuvre est dans un bouton (`.agrandir`). Un clic, ou Entrée au clavier, l’ouvre dans le `<dialog id="visionneuse">` d’index.astro avec `showModal()`. Un clic n’importe où la referme ; Échap ne ferme que l’image, pas la fiche.
- **Liens internes** : dans les fiches, `[[id]]` ou `[[id|texte]]` devient un lien `#id`. L’adresse de la page prend la forme `#id` quand une fiche est ouverte, et une adresse en `#id` ouvre directement la fiche.
- Le Markdown est converti dans le navigateur par **marked**, chargé depuis jsDelivr (version 18.0.14).
- **Accessibilité** : attributs ARIA, focus visible, navigation au clavier, respect de `prefers-reduced-motion`. À préserver.

## style.css
- Couleurs et polices définies par des variables CSS sur `:root`.
- Mode sombre par `:root[data-theme="dark"]`. L’attribut est toujours posé par le script du `<head>` d’index.astro : d’après le choix mémorisé (`localStorage`, clé `theme`), sinon d’après le réglage de l’appareil. Le bouton `#bouton-theme` le change (app.js, section « Thème »).
- Version mobile en dessous de 720 px.
- Police Archivo (Google Fonts). La couleur de chaque mouvement vient du tableur.

## Déploiement
À chaque push, Cloudflare installe les dépendances, puis lance la Deploy command `npx wrangler deploy`. Wrangler lit le bloc `build` de `wrangler.jsonc`, lance `npm run build`, puis publie `./dist`. Dans le tableau de bord, la Build command de l’onglet Production reste vide, sans quoi le site serait fabriqué deux fois.
- Branche de production : `main`. Adresse : https://frise-chronologique---art-moderne.tjimblet.workers.dev
- Les autres branches ont une version de test (encadré « Previews »), publiée par `wrangler preview` (commande en bêta) grâce au bloc `"previews": {}`.
- wrangler n’est pas dans les dépendances : `npx` télécharge la dernière version à chaque déploiement (voir JOURNAL).

---

# B. Architecture cible : Astro (en construction)

Les choix marqués « proposé » dans DECISIONS restent à valider. Ce plan sera précisé au début des phases 1 et 2.

## Principes
1. **Les sources ne bougent pas.** Le tableur et les fiches restent la source de vérité et se modifient comme aujourd’hui.
2. **construire.py reste la porte d’entrée des données** et garde toutes ses vérifications. Il écrit désormais `src/data/frise.json` au lieu de `public/data.js`.
3. **Astro génère des pages HTML statiques** : la frise sur la page d’accueil, plus une page par fiche. Pas de serveur ni de base de données.
4. **La frise reste en JavaScript simple** : `app.js` devient un script de la page d’accueil, presque inchangé.
5. **Aucun framework d’interface** (React, Vue…) ni de CSS (Tailwind…).

## Arborescence visée
```
astro.config.mjs  package.json  .gitignore
construire.py                 écrit src/data/frise.json
donnees/  fiches/             inchangés
public/                       copié tel quel : images, favicon
src/
  data/frise.json             GÉNÉRÉ, commité
  layouts/Base.astro          <head>, polices, éléments communs à toutes les pages
  pages/
    index.astro               la frise
    mouvements/[id].astro     une page par mouvement   -> /mouvements/fauvisme/
    artistes/[id].astro       une page par artiste     -> /artistes/matisse/
    oeuvres/[id].astro        une page par œuvre       -> /oeuvres/guernica/
  scripts/frise.js            ancien app.js
  styles/style.css            ancien style.css
  lib/                        fonctions partagées (conversion des liens [[id]], Markdown)
```

## Points à trancher le moment venu
- **Phase 1** : comment `frise.js` reçoit les données (import du JSON ou injection dans la page) ; si `marked` reste chargé depuis le CDN ou si le Markdown est converti au moment du build.
- - **Phase 2** : les liens `[[id]]` doivent mener vers `#id` dans le panneau de la frise et vers `/artistes/id/` sur les pages ; une seule fonction partagée doit gérer les deux cas. Il faut aussi décider si les fiches non rédigées ont une page, et si la page d’accueil reste la frise ou devient une page qui y mène (voir ROADMAP, « Idées à trier ») ; dans ce cas, `index.astro` deviendrait la page d’accueil et la frise aurait sa propre adresse. Enfin, le contenu des fiches est aujourd’hui fabriqué dans app.js (`ficheArtiste`, `ficheMouvement`…) : les pages devront le reprendre sans le dupliquer, par exemple avec des fonctions partagées dans `src/lib/`.
- **Phase 5** : l’organisation des fichiers pour la version anglaise (`/en/…`). À décider avant de commencer, pas pendant.

## Déploiement visé
C’est déjà le cas depuis la phase 1 : Cloudflare lance `npm run build` (via `wrangler.jsonc`) et publie le dossier `./dist`. Comme `frise.json` sera commité, Cloudflare n’aura besoin ni de Python ni du tableur.
