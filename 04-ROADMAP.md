# Roadmap

# Roadmap

**Étape en cours : lot 3.** La phase 1, le lot 1 et le lot 2 sont terminés depuis le 2 novembre 2026. Le lot 3 passe avant la phase 0.

---

## Phase 0 · Remettre de l’ordre (petite)
- Mettre à jour LISEZMOI.md pour qu’il décrive les vrais outils (Excel, VS Code, Cloudflare) et le vrai fonctionnement de app.js.
- Régler le cas des fiches qui n’ont pas de ligne dans le tableur (voir JOURNAL).
- Ajouter à construire.py un avertissement pour ces fiches orphelines, pour que le cas ne se reproduise pas en silence.
- Valider ou modifier les décisions « proposées » de DECISIONS (fait le 2 novembre, sauf D10, reportée).
- Corriger la docstring de construire.py, qui parle encore de `docs/data.js`.
- Décider du sort de `docs/` selon ce que publie GitHub Pages (voir JOURNAL).
- Ranger style.css : regrouper les règles définies plusieurs fois (`.fermer`, `.fiche .corps`) et fondre les blocs « Ajouts de la version N » dans les sections qui leur correspondent, sans changement visuel.

**Terminé quand** : LISEZMOI est exact, chaque fiche correspond à une ligne du tableur, `python construire.py` ne donne aucun avertissement (y compris le nouveau), sauf celui de Seurat, qui attend l’ajout du néo-impressionnisme (voir « Plus tard »), et toutes les décisions sont tranchées, sauf D10, reportée (à trancher avant d’ajouter d’autres images).

## Phase 1 · Migrer vers Astro, à l’identique · Terminée le 2 novembre 2026
Objectif : le même site, reconstruit avec Astro, sans aucun changement visible.
1. Créer une branche `astro`. Installer Node.js et créer un projet Astro minimal qui s’affiche en local. Ajouter un `.gitignore` (node_modules, dist).
2. Déplacer index.html, style.css et app.js dans Astro en changeant le moins de choses possible (essentiellement les chemins).
3. Adapter construire.py pour Astro. Pour la phase 1, il suffit d’écrire `public/data.js` (fait). Le passage à `src/data/frise.json` peut attendre la phase 2, où les pages en auront besoin.
4. Régler Cloudflare : `wrangler.jsonc` publie `./dist` et lance lui-même `npm run build` (bloc `build`), car Cloudflare ignorait la commande saisie dans le tableau de bord. Tester sur l’adresse de prévisualisation de la branche (fait).
5. Fusionner dans `main` (fait le 2 novembre 2026, déploiement de production validé).

**Terminé quand** : `npm run build` passe sans erreur, et chaque point ci-dessous fonctionne comme avant, sur ordinateur et sur téléphone :
- les trois vues (détail, ensemble, mouvement) ;
- le glisser, les flèches du clavier, la barre des décennies, le sommaire ;
- la recherche (avec et sans accents) et la case « Œuvres majeures seulement » ;
- la bulle d’aperçu et le panneau (ouvrir, fermer, redimensionner) ;
- une adresse en `#id` ouvre la bonne fiche ;
- les modes clair et sombre.
Les captures d’écran avant/après prévues au départ ont été abandonnées le 2 novembre 2026 : la liste de contrôle a suffi.

## Lots après la phase 1 (défauts et souhaits relevés sur la V1)
Un lot = une branche. À traiter dans cet ordre.

**Lot 1 · Corrections** · Terminé le 2 novembre 2026
- La bulle d’aperçu ne disparaît pas quand le curseur quitte l’œuvre, tant qu’il ne passe pas sur un autre élément cliquable. Tests à faire : vue, endroit où va le curseur, navigateur.
- La poignée du panneau devient inaccessible quand on fait défiler la fiche (elle défile avec le texte).
- La croix du panneau, un peu plus grande.

**Lot 2 · Fiches** · Terminé le 2 novembre 2026
- « Aussi » : mouvements cliquables et avec majuscule. La colonne « Autres mouvements » contient désormais des identifiants.
- Rubrique « Nationalité » (retirée du sous-titre) ; « Rôle » affiché « Disciplines » ; majuscule initiale à l’affichage.
- Fiche mouvement : « Artistes majeurs » ; « Œuvres » divisée en « Œuvres majeures » et « Autres œuvres », avec le nom de l’artiste sur la même ligne. Seules les œuvres majeures auront une analyse.
- Clic sur l’image d’une œuvre : l’afficher en grand.
- En plus : libellés des fiches (Nationalité, Disciplines, Mouvement…) un peu plus grands.

**Après D10**
- Œuvre protégée : message du type « Nous n’avons pas encore les droits pour afficher cette œuvre, mais vous pouvez la retrouver ici », avec un lien. Piste : le « cartel de musée » (voir « Idées à trier »).

**Lot 3 · Design** (demande explicite, en accord avec D3 ; après le rangement de style.css en phase 0)
- Couleur de fond (pistes : ivoire `#FAF7F0`, papier ambré `#F5EEDF`, kraft `#EFE3CC` ; Lovable propose ivoire `#F8F5EE`) et mode sombre plus doux (brun très foncé, blanc cassé, ombres sombres ; Lovable propose fond `#1A1715`, texte `#F3EEE3`, surfaces `#282320`).
- Bandeau du haut trop vide, surtout à droite. À refaire dans l’esprit de la maquette Lovable du 3 novembre 2026 (titre à gauche, boutons à droite), en gardant la recherche, la case « Œuvres majeures seulement » et le sommaire des mouvements. Il accueille :
  - un bouton « Une œuvre au hasard » (validé), qui ouvre la fiche d’une œuvre tirée au sort ;
  - un bouton « Index », qui ouvre un tiroir sur le modèle du panneau des fiches, avec des onglets Mouvements, Artistes et Œuvres, une recherche et une liste. Le contenu exact du tiroir est à préciser au début du lot.
- Étiquettes d’artistes trop vides dans la vue mouvement.
- Un niveau de zoom intermédiaire entre la vue détail et la vue d’ensemble. Chaque œuvre y reste une carte : l’idée de remplacer les cartes par des compteurs par décennie est écartée.

**Plus tard (phase 2 ou 3)**
- Compléter l’Index avec les liens vers les nouvelles pages et sections, à mesure qu’elles existent (Chronologie, Mouvements, Artistes, Parcours, Lexique, À propos).
- Contenu : ajouter le néo-impressionnisme à l’onglet Mouvements (avec sa fiche), puis remplacer « néo-impressionnisme » par son identifiant dans la colonne « Autres mouvements » de Seurat. `construire.py` suggérera l’identifiant.

## Idées à trier (proposées par Lovable le 3 novembre 2026)
Ces idées ne sont pas validées. Une conversation dédiée, avant le début de la phase 2, décidera de ce qu’on garde, sous quelle forme et dans quelle phase. Pour chaque idée retenue, il faudra aussi dire quel contenu est à rédiger : les sections de contenu demandent surtout de l’écriture.
- **Page d’accueil distincte de la frise** (maquette Lovable) : grand titre, image, grille des mouvements, accès à la frise. C’est un changement d’architecture : aujourd’hui, la frise est la page d’accueil. Meilleur pour Google. À décider au début de la phase 2.
- **Modèles pour les pages de la phase 2** (maquettes Lovable) : galerie d’œuvres filtrée par mouvement, page mouvement avec son texte, ses artistes et ses œuvres.
- **Œuvres sans image** : un « cartel de musée » typographique (titre, date, technique, lieu, lien vers le site du musée). Les initiales proposées par Lovable ne conviennent pas (« Vv » pour van Gogh, et les mêmes initiales sur toutes les œuvres d’un artiste).
- **Fiches de révision** : comparer deux mouvements (contexte, idées, techniques, manifeste, artistes), par exemple fauvisme et expressionnisme.
- **Lexique & manifestes** : 20 à 30 définitions (pointillisme, primitivisme, ready-made…) et des extraits de manifestes (futurisme, 1909…). Droits à vérifier pour chaque extrait, traductions comprises.
- **Chronologie des ruptures** : 10 à 15 repères d’histoire générale (Première Guerre mondiale, photographie, tube de peinture…) mis en regard de la frise, sans la surcharger.
- **Rendre plus visibles deux éléments existants**, que Lovable n’a pas remarqués : le couloir « Contexte » (événements) et la rubrique « Liens » des fiches artistes (influences).

## Phase 2 · Une page par fiche
- Créer un gabarit de page pour les mouvements, les artistes et les œuvres, dans le style du panneau.
- Faire mener les liens `[[id]]` vers les vraies pages, et ajouter dans le panneau de la frise un lien « Ouvrir la page ».
- Mettre en place le référencement de base : titre de page, description (le résumé court du tableur), adresse canonique, sitemap.xml, robots.txt.
- Créer une page « À propos ».
- À décider au début de la phase : les fiches non rédigées ont-elles une page ?
- À décider au début de la phase : la page d’accueil reste-t-elle la frise, ou devient-elle une page qui y mène (voir « Idées à trier ») ? Dans le second cas, il faut aussi choisir l’adresse de la frise, qui devra rester stable (D9).

**Terminé quand** : chaque fiche rédigée a sa page, une vérification automatique confirme qu’aucun lien interne n’est cassé, et le sitemap est généré.

## Phase 3 · Nom de domaine et ouverture au public
- Choisir le nom, acheter le .com et le brancher sur Cloudflare.
- Rediriger l’ancienne adresse GitHub Pages.
- Ajouter une page 404, un favicon et un aperçu pour le partage sur les réseaux (Open Graph).
- Déclarer le site dans Google Search Console.

## Phase 4 · Parcours thématiques
Format à définir avant de commencer. Les fiches de révision, le lexique et la chronologie des ruptures pourraient en faire partie (voir « Idées à trier »).

## Phase 5 · Version anglaise
L’organisation des fichiers et des adresses (`/en/…`) est à décider avant de commencer.