# Architecture

Ce document décrit le site tel qu’il est (partie A) et tel qu’il doit devenir (partie B). Il est mis à jour à la fin de chaque phase de la roadmap.

---

# A. État actuel : V1, statique, sans framework (octobre 2026)

## Flux des données
```
donnees/frise_art_moderne_base.xlsx ─┐
                                     ├─> python construire.py ─> docs/data.js ─> docs/index.html
fiches/<type>/<id>.md ───────────────┘                                          + app.js + style.css
```
Le site lui-même ne lit ni le tableur ni les fiches : il ne lit que `docs/data.js`, produit par `construire.py`.

## Arborescence
C’est l’état de la branche `main`, celle qui est en ligne. La branche `astro` ajoute le projet Astro : `package.json`, `astro.config.mjs`, `src/pages/index.astro` et `public/`. Sur cette branche, construire.py écrit `public/data.js` et wrangler publie `./dist` (voir JOURNAL).
```
LISEZMOI.md                 mode d’emploi (en partie périmé, voir JOURNAL)
construire.py               tableur + fiches -> docs/data.js (Python, librairie openpyxl)
wrangler.jsonc              configuration Cloudflare : publie le dossier ./docs
donnees/
  frise_art_moderne_base.xlsx
fiches/
  _gabarits/                modèles : gabarit-mouvement.md, gabarit-artiste.md, gabarit-oeuvre.md
  mouvements/  artistes/  oeuvres/     une fiche = un fichier <id>.md
docs/                       le site publié
  index.html                structure de la page (≈ 50 lignes)
  style.css                 apparence (≈ 330 lignes)
  app.js                    fonctionnement de la frise (≈ 540 lignes)
  data.js                   GÉNÉRÉ par construire.py : ne jamais le modifier à la main
  images/
```

## Le tableur
Une ligne compte comme un élément si sa colonne `ID` est remplie. Colonnes lues par `construire.py` :
- **Mouvements** : ID, Nom, Début, Fin, Lieu, Couleur, Résumé court (1 phrase).
- **Artistes** : ID, Nom complet, Naissance, Décès, Nationalité, Rôle, Mouvement principal, Autres mouvements, Phrase-clé (1 phrase).
- **Œuvres** : ID, Titre, Artiste (ID), Année, Mouvement, Date affichée, Type, Musée, Ville, Majeure (Bible), Lien image, Résumé court (1 phrase).
- **Événements** : ID, Année, Date précise, Titre, Lieu, Mouvement, Description (1-2 phrases).
- **Influences** : ID, De (ID), Vers (ID), Type, Explication (1-2 phrases).
Les autres colonnes (Fichier fiche, Statut fiche, Nb…, Domaine public ?, Notes / corrections) servent au suivi et ne sont pas lues. L’onglet « Lisez-moi » non plus.

## construire.py
1. Lit les cinq onglets.
2. Pour chaque mouvement, artiste et œuvre, lit `fiches/<dossier>/<id>.md`. Il retire l’en-tête YAML, le premier titre `#` et les commentaires `<!-- -->`. Une fiche qui contient moins d’une vingtaine de caractères de vrai texte est considérée comme vide : le site affiche alors « fiche à venir ».
3. Remplace l’apostrophe droite par l’apostrophe typographique dans les textes affichés (fonction `typo`), jamais dans les identifiants.
4. Vérifie la cohérence et affiche des avertissements : mouvement ou artiste inconnu, fiche manquante, année manquante, identifiant en double.
5. Écrit `docs/data.js` sous la forme `window.FRISE = { mouvements, artistes, oeuvres, evenements, influences }`.

## Format des données (window.FRISE)
- **mouvement** : id, nom, debut, fin, lieu, couleur, resume, fiche, ficheVide
- **artiste** : id, nom, naissance, deces, nationalite, role, mouvement, autres, phrase, fiche, ficheVide
- **oeuvre** : id, titre, artiste, annee, mouvement, dateAffichee, type, musee, ville, majeure (booléen), image, resume, fiche, ficheVide
- **evenement** : id, annee, date, titre, lieu, mouvement, description
- **influence** : de, vers, type, explication

Le champ `fiche` contient le Markdown de la fiche, ou une chaîne vide si elle n’est pas rédigée.

## app.js
- Lit `window.FRISE`. L’objet `REGLAGES`, en tête du fichier, contient les réglages : années affichées (1860 à 1966, départ en 1870), pixels par an, tailles des cartes et des lignes, marges de la vue mouvement.
- **Trois vues** : « détail » (la frise complète, une carte par œuvre, vue par défaut), « ensemble » (toute la période sans défilement) et « mouvement » (un mouvement en grand, une ligne par artiste).
- **Navigation** : glisser à la souris, flèches du clavier, barre des décennies, sommaire des mouvements, recherche insensible aux accents, case « Œuvres majeures seulement ».
- **Fiches** : une bulle d’aperçu au survol d’une œuvre, et un panneau latéral redimensionnable (poignée, clavier, double-clic pour la taille par défaut).
- **Liens internes** : dans les fiches, `[[id]]` ou `[[id|texte]]` devient un lien `#id`. L’adresse de la page prend la forme `#id` quand une fiche est ouverte, et une adresse en `#id` ouvre directement la fiche.
- Le Markdown est converti dans le navigateur par **marked**, chargé depuis jsDelivr (version 18.0.14).
- **Accessibilité** : attributs ARIA, focus visible, navigation au clavier, respect de `prefers-reduced-motion`. À préserver.

## style.css
- Couleurs et polices définies par des variables CSS sur `:root`.
- Mode sombre automatique (`prefers-color-scheme`), forçable avec `data-theme`.
- Version mobile en dessous de 720 px.
- Police Archivo (Google Fonts). La couleur de chaque mouvement vient du tableur.

## Déploiement
Cloudflare publie le dossier `./docs` comme site statique (`wrangler.jsonc`, rubrique `assets`). Le site était auparavant publié sur GitHub Pages depuis `/docs` ; la migration vers Cloudflare est en cours.

---

# B. Architecture cible : Astro (en construction)

Les choix marqués « proposé » dans DECISIONS restent à valider. Ce plan sera précisé au début des phases 1 et 2.

## Principes
1. **Les sources ne bougent pas.** Le tableur et les fiches restent la source de vérité et se modifient comme aujourd’hui.
2. **construire.py reste la porte d’entrée des données** et garde toutes ses vérifications. Il écrit désormais `src/data/frise.json` au lieu de `docs/data.js`.
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
- **Phase 2** : les liens `[[id]]` doivent mener vers `#id` dans le panneau de la frise et vers `/artistes/id/` sur les pages ; une seule fonction partagée doit gérer les deux cas. Il faut aussi décider si les fiches non rédigées ont une page.
- **Phase 5** : l’organisation des fichiers pour la version anglaise (`/en/…`). À décider avant de commencer, pas pendant.

## Déploiement visé
Cloudflare lance `npm run build` et publie le dossier `./dist`. Comme `frise.json` est commité, Cloudflare n’a besoin ni de Python ni du tableur.
