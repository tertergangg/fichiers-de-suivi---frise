# Journal

L’entrée la plus récente est en haut. Chaque entrée indique ce qui a été fait, ce qui reste à faire et les problèmes ouverts.

---

## 4 novembre 2026 · Lot 3 terminé : barre du bas en vue mouvement (étape 6)
**Fait**
- **Repères adaptés à la vue mouvement.** La liste de la barre du bas n’est plus fabriquée une seule fois au démarrage : `remplirReperes(f)`, appelée par `dessiner()`, la remplit selon la vue.
  - En vue détail et en vue d’ensemble, rien ne change : les 11 décennies.
  - En vue mouvement, des années, avec le pas le plus fin parmi 1, 2, 5 et 10 ans qui donne au plus 11 cases (constantes `PAS_REPERES` et `REPERES_MAX`, fonctions `anneesReperes()` et `pasReperes()`).
  - La liste n’est refaite que si ses années changent (`clefReperes`), pour qu’un redimensionnement ne fasse pas perdre le focus clavier.
  - Le libellé de la barre devient « Aller à une année » en vue mouvement.
- **Clic sur une case** : en vue mouvement, il fait défiler cette vue (`allerAnnee()`) au lieu de ramener à la frise. Seule la vue d’ensemble renvoie encore à la frise.
- **Case en noir** : seulement en vue détail, celle du bord gauche (dernière case déjà passée, ou la première). Aucune en vue mouvement ni en vue d’ensemble.
- **Retour à la frise depuis la vue mouvement** : les boutons Détail et Moyen, comme « ‹ Toute la frise », ramènent à l’année affichée au bord gauche (`anneeGauche()`, lue avant le changement de vue), et non plus au début du mouvement. Les trois boutons de zoom restent affichés, aucun en noir.
- **style.css** : la grille de la barre crée une colonne par case (`grid-auto-flow: column; grid-auto-columns: 1fr`, et `minmax(2.2rem, 1fr)` sur téléphone) au lieu d’en imposer 11. Rendu identique en vue détail.
- **En plus** : la ligne de vie des artistes est retirée de la vue mouvement (app.js, `vueMouvement()` ; règle `.vie` supprimée de style.css). Les dates restent dans l’étiquette.
- Propositions écartées :
  - un gros bouton unique « Toute la frise » dans la barre, qui aurait doublé celui du bandeau et changé la forme de la barre ;
  - mettre en noir toutes les cases visibles, comme le curseur d’une barre de défilement, essayé puis retiré parce que trop appuyé.
- Branche `corrections-lot3-suite`, testée en local.

**Reste à faire**
- Tester `corrections-lot3-suite` sur l’aperçu Cloudflare, puis la fusionner dans `main`.
- Puis la phase 0.

**Problèmes ouverts**
- Un mouvement court tient presque entier à l’écran : la vue ne défile pas, et un clic sur une case ne fait rien de visible. La barre sert alors surtout d’échelle.
- En vue détail, la case noire suit le bord gauche : en fin de frise, les dernières décennies (1950, 1960) ne passent sans doute jamais en noir. Ce défaut existait déjà.
- Les noms `majDecennies()`, `.decennie` et `#decennies` désignent aussi des années en vue mouvement. À renommer au découpage de la phase 2.
- LISEZMOI et ARCHITECTURE ne décrivent ni les trois boutons de zoom, ni les repères de la vue mouvement, ni le retrait de la ligne de vie.
- Les problèmes ouverts de l’entrée précédente restent valables, sauf `choisirNiveau()`, revue à cette étape :
  - centrage d’une fiche compté en années (au zoom Moyen) ;
  - hauteur de l’axe calculée à deux endroits ;
  - `changerZoom()` qui suppose les mêmes couloirs dans le même ordre ;
  - clic sur une décennie depuis la vue d’ensemble qui ramène au dernier zoom ;
  - barre du bas serrée sur téléphone ;
  - app.js qui grossit ;
  - `compter()` hors de l’Index ;
  - nationalité supposée au féminin ;
  - disciplines longues sur deux lignes ;
  - HTML des fiches dans app.js ;
  - ligne des outils sur téléphone ;
  - position de l’Index non gardée, pas d’adresse `#index` ;
  - `aria-live` non testé ;
  - flèche ↗ ;
  - blocs en fin de style.css ;
  - clé `theme` écrite à deux endroits ;
  - hauteur du bouton de thème ;
  - libellé de la bulle du thème ;
  - wrangler hors des dépendances ;
  - `docs/` et GitHub Pages ;
  - docstring de construire.py ;
  - « Previews Base » ;
  - favicon ;
  - Seurat ;
  - `--fond-etiquette` ;
  - `.filtre .pastille`.

**Décisions prises** : aucune.

## 4 novembre 2026 · Lot 3 : zoom intermédiaire (étape 4), croix gardée (étape 5)
**Fait**
- **Fusion** de `corrections-lot3-v1` (étapes 1 à 3) dans `main`. Nouvelle branche `corrections-lot3-suite` pour la suite du lot 3.
- **Trois niveaux dans la barre du bas** : les boutons « Ensemble », « Moyen » et « Détail » (groupe `.niveaux`, boutons `.niveau` en `aria-pressed`) remplacent le bouton « Vue d’ensemble ». Le niveau affiché est en noir ; aucun ne l’est en vue mouvement.
- **Le zoom « Moyen » n’est pas une nouvelle vue** : c’est la vue détail avec `etat.moyen = true`. Seule `fenetre()` en tient compte (nouveaux réglages `pxParAnMoyen: 28` et `largeurCarteMoyenne: 140`, mêmes coefficients sur téléphone). Recherche, hasard, décennies, centrage et bandes cliquables marchent donc sans retouche. Alternative écartée : une quatrième vue, qui aurait obligé à modifier une dizaine de tests `etat.vue === "detail"`.
- **Cartes resserrées** : au zoom Moyen, elles affichent le nom court de l’artiste (`artisteCarte()`). L’aperçu au survol garde le nom complet.
- **On garde sa place** entre Détail et Moyen : la même année à gauche, et le même couloir en haut, à la même hauteur relative (`repereVertical()`, `hautRepere()`, `changerZoom()`). Depuis la vue d’ensemble ou la vue mouvement, on arrive en haut de la frise (au début du mouvement depuis la vue mouvement).
- **Nouvelles fonctions** : `zoomMoyen()` et `niveauAffiche()`, qui calculent le niveau à partir de l’état au lieu de le stocker, et `choisirNiveau()`. Le zoom choisi est gardé quand on change de vue, mais pas d’une visite à l’autre.
- **style.css** : les règles `.decennies .ensemble` sont remplacées sur place par les règles `.niveaux` et `.niveau` (ordinateur et téléphone). Au passage, la règle `.fleche:first-of-type`, qui ne s’appliquait jamais, s’applique désormais : le trait apparaît entre ‹ et « 1860 », et le trait double qui précédait ‹ disparaît.
- **Choix** : les repères de l’axe restent tous les 5 ans au zoom Moyen (140 px d’écart, lisible).
- **Propositions écartées** : zoomer avec Ctrl + molette ou en pinçant l’écran (conflit avec le zoom du navigateur, accessibilité) ; mémoriser le zoom.
- **Étape 5 abandonnée** : la croix de fermeture actuelle est gardée, dans le panneau comme dans l’image en grand.
- Branche `corrections-lot3-suite`, testée en local, commitée.

**Reste à faire**
- Lot 3, étape 6 (barre du bas en vue mouvement), dans une nouvelle conversation. Elle devra revoir `choisirNiveau()`.
- Décider de la fusion de `corrections-lot3-suite` dans `main` : maintenant (après un test sur l’aperçu Cloudflare) ou après l’étape 6.
- Puis la phase 0.

**Problèmes ouverts**
- Le centrage d’une fiche ouverte (`allerAnnee(annee - 3, …)`) est compté en années : au zoom Moyen, la carte arrive plus près du bord gauche.
- `toile.querySelector(".axe").offsetHeight` est calculé à deux endroits (`repereVertical()` et `hautRepere()`). À ranger au découpage de la phase 2.
- `changerZoom()` repère les couloirs par leur position dans la liste. Cela suppose que Détail et Moyen dessinent les mêmes couloirs dans le même ordre, ce qui est le cas aujourd’hui.
- Depuis la vue d’ensemble, un clic sur une décennie ramène au dernier zoom choisi, pas forcément à « Détail ».
- **Téléphone** : la barre du bas, avec ses trois boutons, est serrée. À revoir au lot 4.
- LISEZMOI et ARCHITECTURE parlent encore du bouton « Vue d’ensemble ».
- app.js grossit encore d’une quarantaine de lignes.
- Les problèmes ouverts de l’entrée précédente restent valables, sauf la décision de fusion de `corrections-lot3-v1`, réglée :
  - `compter()` utilisée hors de l’Index ;
  - nationalité supposée au féminin ;
  - disciplines longues sur deux lignes ;
  - HTML des fiches qui grossit dans app.js ;
  - ligne des outils sur téléphone ;
  - position de l’Index non gardée, pas d’adresse `#index` ;
  - `aria-live` non testé avec un lecteur d’écran ;
  - rendu de la flèche ↗ ;
  - blocs ajoutés en fin de style.css ;
  - clé `theme` écrite à deux endroits ;
  - hauteur du bouton de thème calée à la main ;
  - libellé de la bulle du thème mis à jour au survol suivant ;
  - wrangler hors des dépendances ;
  - `docs/` et GitHub Pages ;
  - docstring de construire.py ;
  - onglet « Previews Base » de Cloudflare ;
  - favicon ;
  - avertissement de Seurat ;
  - `--fond-etiquette` inutilisée ;
  - `.filtre .pastille` définie deux fois.

**Décisions prises** : aucune. L’abandon de l’étape 5 est noté dans la ROADMAP.

## 3 novembre 2026 · Lot 3 : étiquettes d’artistes (étape 3)
**Fait**
- **Étiquettes de la vue mouvement plus complètes.** Nouvelle fonction `etiquetteArtiste(a, m, nombre)` dans app.js, appelée par `vueMouvement()`. Chaque étiquette affiche :
  - le nom (inchangé : un clic ouvre la fiche, la phrase-clé reste en bulle au survol) ;
  - « dates · N œuvres ici ». Le nombre compte les œuvres de l’artiste visibles dans la vue ; il est masqué s’il vaut zéro. L’accord (« 1 œuvre », « 2 œuvres ») réutilise `compter()`, la fonction de l’Index ;
  - « Nationalité … · Disciplines », chaque morceau seulement s’il existe ;
  - « Surtout : … » pour un artiste « invité », c’est-à-dire dont le mouvement principal est un autre.
- **Nationalité courte** : nouvelle fonction `nationaliteCourte()`. Elle garde le premier mot (coupé au premier espace, à la première virgule ou à la première parenthèse) et le met en minuscule : « Roumaine (puis française) » donne « Nationalité roumaine ». Le texte complet reste dans la fiche.
- **style.css** : les nouvelles lignes reprennent le style des `<small>` existants. Seul ajout : la classe `.complement`, cachée sous 720 px dans le bloc téléphone des « Ajouts de la version 3 ». Sur téléphone, l’étiquette reste donc comme avant (nom et dates).
- Propositions écartées :
  - afficher la phrase-clé dans l’étiquette ;
  - « N œuvres dans la frise », faux, puisque seul ce mouvement est compté ;
  - « N œuvres dans ce mouvement », trop long pour tenir sur la ligne des dates.
- Branche `corrections-lot3-v1`, testée en local, commitée.

**Reste à faire**
- Lot 3, étapes 4 (zoom intermédiaire), 5 (croix de fermeture) et 6 (barre du bas en vue mouvement), dans de nouvelles conversations.
- Décider de la fusion dans `main` : maintenant (après un test sur l’aperçu Cloudflare) ou à la fin du lot 3.

**Problèmes ouverts**
- `compter()` sert maintenant hors de l’Index. À ranger avec les outils partagés au découpage de la phase 2.
- « Nationalité … » suppose que la colonne « Nationalité » du tableur est au féminin (« française »). Rien ne signale une forme masculine.
- Des disciplines longues passent sur deux lignes et agrandissent un peu la ligne de l’artiste.
- app.js continue de grossir, comme le HTML fabriqué dans les fonctions de vue et de fiche.
- Les problèmes ouverts de l’entrée précédente restent valables : ligne des outils sur téléphone, position de l’Index non gardée, pas d’adresse `#index`, `aria-live` non testé avec un lecteur d’écran, rendu de la flèche ↗, blocs ajoutés en fin de style.css, clé `theme` écrite à deux endroits, hauteur du bouton de thème calée à la main, libellé de la bulle du thème mis à jour au survol suivant, wrangler hors des dépendances, `docs/` et GitHub Pages, docstring de construire.py, onglet « Previews Base » de Cloudflare, favicon, avertissement de Seurat, `--fond-etiquette` inutilisée, `.filtre .pastille` définie deux fois.

**Décisions prises** : aucune.

## 3 novembre 2026 · Lot 3 : bouton clair/sombre (étape 2c)
**Fait**
- **Script dans le `<head>`** d’index.astro (`is:inline`). Il lit le choix mémorisé (`localStorage`, clé `theme`, valeurs `light` ou `dark`). Sans choix, il suit le réglage de l’appareil (`matchMedia`). Il pose toujours `data-theme` sur `<html>` avant l’affichage : pas de flash du mauvais thème. Il est écrit comme une fonction immédiatement appelée, pour que ses variables n’entrent pas en conflit avec celles d’app.js.
- **Bouton de thème** (`#bouton-theme`), sous « Index », sur la deuxième ligne de l’en-tête, à droite de la case « Œuvres majeures seulement ». Les deux sont regroupées dans un nouveau bloc `.reglages`. Le bouton est carré, de la hauteur des boutons des mouvements, et aligné sur leur première rangée.
  - Les icônes sont en SVG `currentColor` : une lune en mode clair, un soleil en mode sombre. Le CSS n’affiche que la bonne.
  - Le style est partagé avec `.hasard` et `.bouton-index`, sans copie.
- **app.js**, nouvelle section « Thème » avant « Démarrage ». Un clic inverse `data-theme` et mémorise le choix. Le libellé décrit l’action (« Passer en mode sombre » ou « Passer en mode clair ») ; il sert d’`aria-label` et de bulle au survol (`data-bulle`).
- **Un seul bloc pour le mode sombre** dans style.css : le bloc `@media (prefers-color-scheme: dark)` est supprimé, puisque `data-theme` est toujours posé.
- Choix : si l’appareil change de thème pendant la visite, la page ne suit qu’au prochain chargement. Pas de troisième état « suivre l’appareil ».
- Branche `corrections-lot3-v1`, testée en local, commitée en deux commits.

**Reste à faire**
- Lot 3, étapes 3 à 6, dans une nouvelle conversation.
- Décider de la fusion dans `main` : maintenant (après un test sur l’aperçu Cloudflare) ou à la fin du lot 3.

**Problèmes ouverts**
- Sans JavaScript, le site reste en mode clair. C’est sans conséquence, puisque la frise a besoin de JavaScript.
- La clé `theme` et les valeurs `light`/`dark` sont écrites à deux endroits : le script du `<head>` et app.js.
- La hauteur du bouton (`2.25rem`) est calée sur celle des boutons des mouvements, comme le `margin-top` de la case. Si ces boutons changent de taille, il faudra revoir les deux.
- Après un clic, la bulle garde l’ancien libellé jusqu’au survol suivant.
- Les problèmes ouverts de l’entrée précédente restent valables, sauf le double bloc du mode sombre, réglé : app.js qui grossit (découpage en phase 2), ligne des outils sur téléphone, position de l’Index non gardée, pas d’adresse `#index`, `aria-live` non testé avec un lecteur d’écran, rendu de la flèche ↗, blocs ajoutés en fin de style.css, wrangler hors des dépendances, `docs/` et GitHub Pages, docstring de construire.py, onglet « Previews Base » de Cloudflare, favicon, avertissement de Seurat, HTML des fiches qui grossit dans app.js, `--fond-etiquette` inutilisée, `.filtre .pastille` définie deux fois.

**Décisions prises** : aucune.

## 3 novembre 2026 · Lot 3 : tiroir Index (étape 2b)
**Fait**
- **Bouton « Index »** dans `.outils`, à droite de « Une œuvre au hasard ». Les trois traits de son icône sont dessinés en CSS (trois `linear-gradient` en `currentColor`) : ils changent de couleur avec le texte au survol.
- **L’Index s’affiche dans le panneau des fiches** (`#panneau`) et garde donc la croix, la poignée, Échap et la version téléphone. Alternative écartée : un tiroir séparé, qui aurait dupliqué tout cela.
- **Contenu** :
  - le titre « Index. » ;
  - le bloc repliable « Comment lire la frise » (`<details>`), qui reprend le texte d’aide retiré du bandeau à l’étape 2a (constante `AIDE_FRISE`) ;
  - les onglets Mouvements, Artistes et Œuvres (boutons `aria-pressed`) ;
  - une recherche insensible aux accents (`sansAccent`) et un compteur (« 1 œuvre », « 9 mouvements ») ;
  - la liste, avec une flèche ↗ sur chaque ligne.
- **Tri** : mouvements par date de début ; artistes de A à Z par nom de famille (`nomCourt` + `localeCompare("fr")`) ; œuvres par année, puis par titre, celles sans année en fin de liste.
- **Lignes** : mouvement « dates · lieu », artiste « dates · mouvement principal », œuvre « année · artiste ». L’Index liste toutes les œuvres, même quand la case « Œuvres majeures seulement » est cochée.
- **Écarts volontaires avec la maquette Lovable** : la croix actuelle est gardée (nouveau style prévu à l’étape 5), tiret demi-cadratin entre les dates, mouvements triés par date.
- **« ‹ Retour à l’index »** en haut des fiches ouvertes pendant que l’Index était ouvert. C’est le drapeau `etatIndex.actif` : `afficherIndex()` le met à `true`, `fermer()` à `false`. Le retour retrouve l’onglet et le texte cherché.
- **Accessibilité** :
  - `aria-live` du panneau coupé pendant l’Index, puis remis par `ouvrir()` ;
  - libellés « Fermer le panneau » et « Redimensionner le panneau » ;
  - à l’ouverture, le focus va dans le champ de recherche sur ordinateur, pas sur téléphone.
- **style.css** : `.bouton-index` et `.onglet` partagent les règles de `.hasard`, et `.champ-index` celles de `.recherche input`, sans copie. Nouveau bloc « Index » en fin de fichier.
- `app.js` (nouvelle section « Index » après `fermer()`) et `index.astro` modifiés. Branche `corrections-lot3-v1`, testée en local, commitée.

**Reste à faire**
- Lot 3, étape 2c (bouton clair/sombre), dans une nouvelle conversation, puis les étapes 3 à 6.
- La fusion dans `main` est de nouveau possible, puisque le texte d’aide est dans l’Index. À décider : fusionner maintenant (après un test sur l’aperçu Cloudflare) ou à la fin du lot 3.

**Problèmes ouverts**
- **app.js** atteint environ 705 lignes. Le découpage de la phase 2 devient pressant.
- **Téléphone** : la ligne des outils (recherche, « Au hasard », « Index ») déborde sans doute. À traiter au lot 4.
- Au retour dans l’Index, la liste revient en haut : la position de défilement n’est pas gardée.
- Pas d’adresse `#index` : un lien ne peut pas ouvrir directement l’Index.
- La coupure d’`aria-live` n’a pas été testée avec un vrai lecteur d’écran.
- La flèche ↗ n’existe peut-être pas dans Archivo (le navigateur prend alors une autre police) : vérifier son rendu sur plusieurs navigateurs.
- **style.css** : le bloc « Index » s’ajoute en fin de fichier, comme les « Ajouts de la version N ». À ranger en phase 0.
- Les problèmes ouverts des entrées précédentes restent valables : wrangler hors des dépendances, `docs/` et GitHub Pages, docstring de construire.py, onglet « Previews Base » de Cloudflare, favicon, avertissement de Seurat, HTML des fiches qui grossit dans app.js, double bloc du mode sombre, `--fond-etiquette` inutilisée, `.filtre .pastille` définie deux fois, alignement de la case « Œuvres majeures seulement » réglé à la main.

**Décisions prises** : aucune.

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
