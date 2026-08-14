# CLAUDE.md

Guidance pour Claude Code sur ce dépôt.

## Projet

Landing page de la promotion **Les Jardins de St-Oyens** (8 villas Minergie, Chemin du Pré-Rouge 13, 1187 Saint-Oyens).

- Promotion : RealT SA · Morges
- Architecture : Outsider.Architecture SA · Morges
- Vente : PROXIMMO Agence Immobilière · Lausanne

## Branches

- Toujours travailler sur la branche `develop`
- Ne jamais pusher sur `main` sans confirmation explicite de l'utilisateur
- La branche `develop` est uniquement pour les tests locaux — elle n'est jamais déployée
- Le site live sera sur : https://st-oyens.realt.swiss (déployé depuis `main`)

## GitHub Pages

- Source : branche `main`, dossier `/`
- La branche `develop` ne déclenche aucun déploiement automatique
- DNS : CNAME `st-oyens` → `alxtpp.github.io` (zone `realt.swiss` chez Infomaniak)

## Serveur local

```bash
cd ~/st-oyens && python3 -m http.server 3000
```

## Workflow des modifications

1. Faire les modifications sur `develop`
2. Lancer un serveur local sur http://localhost:3000 pour prévisualiser
3. Demander confirmation avant tout push
4. Attendre confirmation explicite avant de merger `develop` → `main`
5. Pour déployer sur `main`, toujours utiliser : `git show develop:index.html > index.html` (ne jamais copier avec `cp` — cela ne fonctionne pas entre branches)

## Structure

- `index.html` — page unique, CSS et JS inline, **images en fichiers** dans `img/` (pas de base64)
- `img/` — rendus en webp, générés depuis les JPG sources haute résolution
- `*.pdf` — brochure et plans d'enquête, servis en téléchargement direct depuis la page
- `CNAME` — domaine GitHub Pages

## Points d'attention

- Le plan interactif des villas (`#villas`) est un calque SVG de 8 polygones au-dessus de `img/plan-villas.webp`.
  Le `viewBox` est `0 0 2750 1790` et correspond au cadrage de `2502_260416_Saint-Oyens_TU.pdf` page 1
  (`pdftoppm -r 200 -x 250 -y 40 -W 2750 -H 1790`). Si l'image du plan change, les polygones sont à recaler.
- Les formulaires (contact + brochure) n'ont **pas de backend** : ils affichent une confirmation mais
  n'envoient rien. Chercher `POINT D'INTÉGRATION LEADS` dans `index.html` pour brancher Formspree/Netlify.
- Le projet est un **4 pièces** (« quatre grandes pièces »), jamais un 5 pièces.
