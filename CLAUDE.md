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

- Le plan interactif des villas (`#villas`) est un calque SVG de 8 polygones au-dessus de
  `img/plan-villas-axo.webp` — l'axonométrie fournie par le client (`schem _bw.jpg`, recadrée
  puis convertie en webp 1600x887). Le `viewBox` est `0 0 1600 887`.
  Les polygones couvrent les toitures. Ils ne sont pas tracés à la main : ils sont générés à
  partir du profil de pignon de la première villa de chaque rang (avant-toit bas, faîte,
  avant-toit haut) répété le long du rang. Attention, **le pas n'est pas uniforme** : les
  villas d'extrémité font 6,65 m et celles du milieu 6,40 m.
  Rang A = bâtiment au premier plan, A1 en haut à gauche vers A4 en bas à droite.
  Rang B = bâtiment à l'arrière, B1 en bas à gauche vers B4 en haut à droite.
  Si l'image change, tout se recalcule : voir le commit « Plan interactif : axonométrie ».
- L'image du plan ne doit **pas** être en `loading="lazy"` : dans un conteneur en `position:relative`
  sans hauteur propre, le lazy-load ne se déclenche jamais et l'image reste invisible.
  Elle garde ses attributs `width`/`height` et un `aspect-ratio` pour réserver sa place.
- Les formulaires (contact + brochure) n'ont **pas de backend** : ils affichent une confirmation mais
  n'envoient rien. Chercher `POINT D'INTÉGRATION LEADS` dans `index.html` pour brancher Formspree/Netlify.
- Le projet est un **4 pièces** (« quatre grandes pièces »), jamais un 5 pièces.
