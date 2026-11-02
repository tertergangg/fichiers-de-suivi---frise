# Conventions

Ces règles décrivent le style déjà en place dans le code. Le nouveau code doit leur ressembler.

## Langue
- Les noms de variables, de fonctions et de classes CSS sont en français, sans accents : `dessiner`, `oeuvres`, `REGLAGES`, `.panneau`.
- Les commentaires sont en français.
- Les mots imposés par un outil restent en anglais : Astro, npm, les noms de propriétés HTML et CSS.

## JavaScript
- JavaScript moderne et sans framework : `const` par défaut, `let` si la valeur change, jamais `var`.
- Guillemets doubles, points-virgules, indentation de deux espaces.
- Des fonctions courtes qui font une seule chose, avec un nom qui dit ce qu’elles font.
- Pas de nombre magique : un réglage va dans `REGLAGES`, une valeur fixe dans une constante nommée.
- Un commentaire au-dessus de chaque bloc dont l’intention n’est pas évidente. On explique pourquoi, pas ce que dit déjà le code.
- Tout texte venant des données et inséré dans du HTML passe par `esc()`.

## CSS
- Les couleurs, polices et tailles passent par les variables de `:root`. Chaque nouvelle couleur doit avoir sa valeur pour le mode sombre.
- Pas de style écrit en dur dans index.html. Dans app.js, l’attribut `style` ne sert qu’aux valeurs calculées : positions et largeurs (`left`, `width`) et couleur du mouvement (`--c`). Tout le reste va dans style.css.
- La version mobile se gère avec les blocs `@media (max-width: 720px)` existants.
- On garde le focus visible et le respect de `prefers-reduced-motion`.

## Python (construire.py)
- Indentation de quatre espaces, noms en français.
- Toute nouvelle règle de cohérence passe par `avertir()`, pour être affichée avec les autres.

## Identifiants
- En minuscules, sans accents, avec des tirets : `van-gogh`, `les-demoiselles-d-avignon`.
- L’identifiant est le même dans le tableur (colonne ID), dans le nom de la fiche (`<id>.md`) et dans l’adresse de la page.
- Il est unique sur tout le site, tous types confondus.
- On ne le change plus une fois le site public (D9).

## Fiches
- Un en-tête YAML avec `id` et `type`, puis un titre `#`, puis les rubriques du gabarit correspondant dans `fiches/_gabarits/`.
- Pour lier une autre fiche : `[[id]]` ou `[[id|texte du lien]]`.
- Les commentaires `<!-- -->` des gabarits ne s’affichent pas.
- Les fiches mouvements et artistes n’ont pas de section « Sources ».

## Typographie des textes affichés
- Apostrophe typographique (’), guillemets français (« ») et tiret demi-cadratin entre deux dates (1874–1960).
- Dans le code, on garde les guillemets et apostrophes droits (' et ").

## Git
- Une branche par tâche, avec un nom court : `astro`, `pages-fiches`, `correction-recherche`.
- On fait un commit avant de demander une modification à Claude, puis un commit par étape qui fonctionne.
- Messages courts en français, qui décrivent le changement : « Recherche insensible aux accents ».
- On relit le diff dans VS Code avant chaque commit.
- On lance `python construire.py` avant tout commit qui touche le tableur ou les fiches.
