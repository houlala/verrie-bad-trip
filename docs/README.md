# Site (dossier docs/)

Ce dossier contient le site statique, généré par Jekyll et publié via GitHub Pages.

## Comment publier

1. Pousser ce dépôt sur GitHub.
2. Dans **Settings → Pages**, choisir la source :
   - Branch : `main`
   - Folder : `/docs`
3. Attendre quelques instants : le site est en ligne.

## Structure

- `_config.yml` — configuration Jekyll
- `_layouts/default.html` — gabarit commun (structure de page)
- `_includes/header.html` / `footer.html` — en-tête et pied de page réutilisés partout
- `assets/style.css` — styles
- `assets/images/` — images
- `index.md`, `a-propos.md` — pages, écrites en Markdown

## Ajouter une page

Créer un fichier `.md` avec cet en-tête, puis écrire en Markdown :

```markdown
---
layout: default
title: Titre de la page
---

Contenu en **markdown**.
```
