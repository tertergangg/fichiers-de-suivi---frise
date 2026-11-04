# Décisions

Chaque décision a un statut :
- **Décidé** : on ne revient pas dessus sans en rediscuter explicitement.
- **Proposé** : en attente de validation. Une fois validé, il passe à « Décidé ».

Quand une décision change, on ne l’efface pas : on la marque « Remplacé par Dx » et on en ajoute une nouvelle.

---

**D1 · Construire le site avec Astro** · Décidé (octobre 2026)
Astro génère un site statique, crée facilement une page par fiche (utile pour Google) et permet de garder le HTML, le CSS et le JavaScript existants.

**D2 · Héberger le site sur Cloudflare** · Décidé
Gratuit, avec un compte lié au dépôt GitHub. Remplace GitHub Pages.

**D3 · Garder le design actuel de la frise** · Décidé
La migration ne doit rien changer à l’apparence. Toute évolution visuelle fait l’objet d’une demande à part.

**D4 · Un nom de domaine en .com, sans « github »** · Décidé
Le nom du site reste à choisir.

**D5 · Le tableur et les fiches Markdown sont la source de vérité** · Décidé
Le tableur se modifie dans Excel, les fiches dans VS Code. Tout le reste (data.js, frise.json, pages) est généré à partir d’eux.

**D6 · Numéro non attribué**
Ce numéro a été sauté. On ne renumérote pas, pour que les mentions « Remplacé par Dx » restent justes.

**D7 · construire.py reste la porte d’entrée des données** · Décidé (novembre 2026)
Il produit les données que lisent la frise et les pages : `public/data.js` pendant la phase 1, puis `src/data/frise.json` à partir de la phase 2. On garde ainsi un seul chemin pour les données et ses vérifications existantes.
Alternative écartée : faire lire les fiches directement par Astro (les « content collections »). C’est plus conforme à Astro, mais cela crée un second chemin de données et oblige à réécrire les vérifications.

**D8 · Les données générées sont commitées** · Décidé (novembre 2026)
`public/data.js` (puis `src/data/frise.json` en phase 2) est versionné dans Git. Cloudflare n’a donc besoin ni de Python ni d’openpyxl pour construire le site. Sans ce fichier dans Git, Cloudflare publierait une frise vide. Il faut lancer `python construire.py` avant chaque commit qui touche le tableur ou les fiches.

**D9 · Des identifiants et des adresses stables** · Décidé (novembre 2026)
L’identifiant d’un élément (ex. `matisse`) est aussi le nom de sa fiche et la fin de son adresse (`/artistes/matisse/`). Tant que le site n’est pas public, un identifiant peut encore changer. Une fois le site public, on ne le change plus : cela casserait les liens et le référencement. La version anglaise vivra sous `/en/`. L’organisation des adresses à l’intérieur de `/en/` sera décidée au début de la phase 5.

**D10 · N’afficher que des images dont les droits le permettent** · Proposé, reporté (novembre 2026)
On s’appuie sur la colonne « Domaine public ? » du tableur. Une œuvre encore protégée s’affiche sans image.
À trancher avant d’ajouter d’autres images.

**D11 · Plus de GitHub Pages, sans redirection de l’ancienne adresse** · Décidé (novembre 2026)
GitHub Pages est désactivé et `docs/` supprimé : l’ancienne adresse en github.io renvoie une erreur 404. C’est le choix le plus simple : rien à maintenir jusqu’à la phase 3.
Alternative écartée : garder `docs/` jusqu’à la phase 3 pour y placer une page de redirection vers le .com.