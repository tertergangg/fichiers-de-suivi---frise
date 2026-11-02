# Marche à suivre
<!-- Pour toi. Inutile de l’ajouter aux fichiers du projet. -->

## Étape 1 · Installer le projet Claude (une seule fois)
1. Ouvre ton projet Claude dédié. Copie tout le contenu de **00-INSTRUCTIONS-PROJET** dans le champ « Instructions ».
2. Ajoute aux fichiers du projet les six documents : **01-CONTEXTE, 02-ARCHITECTURE, 03-DECISIONS, 04-ROADMAP, 05-CONVENTIONS et 06-JOURNAL**.
3. Ajoute aussi tes fichiers de code depuis la branche `astro` : `src/pages/index.astro`, `public/app.js`, `public/style.css`, `construire.py`, `wrangler.jsonc`, `package.json` et `astro.config.mjs`.
4. N’ajoute pas `data.js`, les fiches ni le tableur. Les fichiers 07 et 08 sont pour toi.

## Étape 2 · Lancer la première conversation de travail
Ouvre une nouvelle conversation dans le projet et colle ce message :

> Lis le JOURNAL et la ROADMAP. Avant toute chose, résume-moi en trois lignes où on en est. Ensuite, on commence par deux choses : valider ou modifier les décisions « Proposé » de DECISIONS (D6 à D10), puis finir la phase 1. Guide-moi pas à pas, une étape à la fois.

Claude reprendra alors les points ouverts du JOURNAL, dans l’ordre.

## Étape 3 · Travailler, une tâche par conversation
- Pour chaque demande, utilise les modèles du fichier 07 : nouvelle tâche, bug, relecture, prise de recul.
- Laisse Claude proposer un plan, puis valide-le avant qu’il code.
- Après chaque changement qui fonctionne, fais un commit sur la branche en cours.
- Si ça part dans tous les sens, utilise le modèle « Quand ça part dans tous les sens », ou reviens au dernier commit.

## Étape 4 · Clore chaque conversation
1. Écris « on clôture ».
2. Remplace le JOURNAL dans les fichiers du projet par la nouvelle version, avec l’entrée proposée en haut. Fais de même pour DECISIONS ou la ROADMAP si elles ont changé.
3. Remplace aussi les fichiers de code modifiés pendant la conversation.
4. Ouvre une nouvelle conversation pour la tâche suivante.

## L’ordre des prochaines tâches
Chaque tâche correspond à une conversation.
1. **Décisions** : valider D6 à D10.
2. **Cloudflare** : régler la commande de build et les builds de branches, puis obtenir l’adresse de prévisualisation de `astro`.
3. **Vérification** : passer la liste de contrôle de la phase 1, en local puis sur la prévisualisation.
4. **Fusion** : fusionner `astro` dans `main`. La phase 1 est terminée.
5. **Phase 0** : mettre à jour LISEZMOI, régler les fiches sans ligne dans le tableur et ajouter leur avertissement dans construire.py.
6. **Phase 2** : une page par fiche. Claude la découpera en tâches au moment voulu.

## Règles de sécurité à garder en tête
- Tant que `astro` n’est pas fusionnée, ne pousse rien sur `main`.
- Fais un commit avant de demander une modification à Claude.
- Relis le diff dans VS Code avant chaque commit.
