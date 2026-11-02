# Modèles de demandes
<!-- Pour toi : messages à copier-coller dans les conversations du projet. Inutile de l’ajouter aux fichiers du projet. -->

## Ouvrir une nouvelle conversation
> Lis le JOURNAL et la ROADMAP. On attaque : [étape, ex. « Phase 1, étape 2 »]. Avant toute chose, résume-moi en trois lignes où on en est et ce que tu comptes faire, puis pose-moi tes questions.

## Demander une fonctionnalité ou une modification
> **Objectif** : [ce que je veux voir à l’écran ou pouvoir faire]
> **Fichiers concernés** : [chemins, et je colle leur contenu actuel ci-dessous]
> **Contraintes** : [ex. « ne touche pas au style », « sans nouvelle dépendance »]
> **Hors sujet pour l’instant** : [ce qu’il ne faut pas faire]
> Commence par le plan, ne code pas encore.

## Signaler un bug
> **Ce que je fais** : [les étapes pour reproduire le bug]
> **Ce que j’attends** :
> **Ce qui se passe** :
> **Message d’erreur exact** : [terminal ou console du navigateur, F12 > Console]
> **Dernier changement avant le bug** : [commit ou modification]
> Donne-moi d’abord ton hypothèse sur la cause, sans corriger.

## Faire relire le code par un regard neuf
À faire dans une nouvelle conversation, pour que Claude n’ait pas le contexte de celle qui a écrit le code :
> Relis ce code comme un développeur senior exigeant. Vérifie sa conformité avec ARCHITECTURE et CONVENTIONS, cherche les bugs possibles, les cas oubliés (mobile, clavier, mode sombre, données manquantes) et ce qui sera difficile à maintenir. Classe tes remarques par gravité. Ne réécris rien.

## Forcer la prise de recul
> Stop. Avant d’aller plus loin : quelles hypothèses fais-tu ? Quelle autre approche était possible, et pourquoi ne l’as-tu pas choisie ? Est-ce qu’on reste dans l’étape en cours de la roadmap ?

## Quand ça part dans tous les sens
> On a fait deux corrections sans succès. Repars de zéro : résume le problème, liste ce qu’on sait et ce qu’on ignore, et dis-moi s’il vaut mieux revenir au dernier commit qui marchait.

## Clore une conversation
> On clôture.

Ensuite, colle l’entrée proposée en haut du JOURNAL et applique les changements proposés à DECISIONS et à la ROADMAP. Remplace les anciens fichiers dans le projet par les nouvelles versions.
