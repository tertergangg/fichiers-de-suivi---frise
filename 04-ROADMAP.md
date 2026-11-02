# Roadmap

**Étape en cours : Lot 1 (corrections).** La phase 1 est terminée depuis le 2 novembre 2026. La phase 0 peut se faire en parallèle.

---

## Phase 0 · Remettre de l’ordre (petite)
- Mettre à jour LISEZMOI.md pour qu’il décrive les vrais outils (Excel, VS Code, Cloudflare) et le vrai fonctionnement de app.js.
- Régler le cas des fiches qui n’ont pas de ligne dans le tableur (voir JOURNAL).
- Ajouter à construire.py un avertissement pour ces fiches orphelines, pour que le cas ne se reproduise pas en silence.
- Valider ou modifier les décisions « proposées » de DECISIONS (fait le 2 novembre, sauf D10, reportée).
- Corriger la docstring de construire.py, qui parle encore de `docs/data.js`.
- Décider du sort de `docs/` selon ce que publie GitHub Pages (voir JOURNAL).

**Terminé quand** : LISEZMOI est exact, chaque fiche correspond à une ligne du tableur, `python construire.py` ne donne aucun avertissement (y compris le nouveau), et toutes les décisions sont tranchées, sauf D10, reportée (à trancher avant d’ajouter d’autres images).

## Phase 1 · Migrer vers Astro, à l’identique
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

**Lot 1 · Corrections**
- La bulle d’aperçu ne disparaît pas quand le curseur quitte l’œuvre, tant qu’il ne passe pas sur un autre élément cliquable. Tests à faire : vue, endroit où va le curseur, navigateur.
- La poignée du panneau devient inaccessible quand on fait défiler la fiche (elle défile avec le texte).
- La croix du panneau, un peu plus grande.

**Lot 2 · Fiches** (avant la phase 2, dont les pages reprendront le contenu du panneau)
- « Aussi » : mouvements non cliquables et sans majuscule. La colonne « Autres mouvements » devra contenir des identifiants.
- Rubrique « Nationalité » ; « Rôle » renommé, par exemple « Pratiques » ; majuscule initiale à l’affichage.
- Fiche mouvement : « Artistes majeurs » ; « Œuvres » divisée en œuvres majeures et autres œuvres. Seules les œuvres majeures auront une analyse.
- Clic sur l’image d’une œuvre : l’afficher en grand.

**Après D10**
- Œuvre protégée : message du type « Nous n’avons pas encore les droits pour afficher cette œuvre, mais vous pouvez la retrouver ici », avec un lien.

**Lot 3 · Design** (demande explicite, en accord avec D3)
- Couleur de fond (pistes : ivoire `#FAF7F0`, papier ambré `#F5EEDF`, kraft `#EFE3CC`) et mode sombre plus doux (brun très foncé, blanc cassé, ombres sombres).
- Bandeau du haut trop vide, surtout à droite.
- Étiquettes d’artistes trop vides dans la vue mouvement.
- Un niveau de zoom intermédiaire entre la vue détail et la vue d’ensemble.

**Plus tard (phase 2 ou 3)**
- Un menu pour accéder aux différentes sections du site.

## Phase 2 · Une page par fiche
- Créer un gabarit de page pour les mouvements, les artistes et les œuvres, dans le style du panneau.
- Faire mener les liens `[[id]]` vers les vraies pages, et ajouter dans le panneau de la frise un lien « Ouvrir la page ».
- Mettre en place le référencement de base : titre de page, description (le résumé court du tableur), adresse canonique, sitemap.xml, robots.txt.
- Créer une page « À propos ».
- À décider au début de la phase : les fiches non rédigées ont-elles une page ?

**Terminé quand** : chaque fiche rédigée a sa page, une vérification automatique confirme qu’aucun lien interne n’est cassé, et le sitemap est généré.

## Phase 3 · Nom de domaine et ouverture au public
- Choisir le nom, acheter le .com et le brancher sur Cloudflare.
- Rediriger l’ancienne adresse GitHub Pages.
- Ajouter une page 404, un favicon et un aperçu pour le partage sur les réseaux (Open Graph).
- Déclarer le site dans Google Search Console.

## Phase 4 · Parcours thématiques
Format à définir avant de commencer.

## Phase 5 · Version anglaise
L’organisation des fichiers et des adresses (`/en/…`) est à décider avant de commencer.
