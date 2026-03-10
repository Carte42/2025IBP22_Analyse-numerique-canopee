# CLAUDE.md — Projet canopée Le Pouliguen (2025IBP22)

## Contexte
Livrable cartographique du marché 2025IBP22 entre la mairie de Le Pouliguen et CARTE 42.
Analyse numérique de la canopée urbaine par détection automatique sur images aériennes.

## Repo & déploiement
- GitHub : https://github.com/Carte42/2025IBP22_Analyse-numerique-canopee (compte **Carte42**)
- GitHub Pages : https://carte42.github.io/2025IBP22_Analyse-numerique-canopee/
- Auth GitHub : PAT dans `Token.txt` (gitignored), intégré dans le remote URL git

## Stack technique
- `index.html` à la racine — Vanilla JS, pas de build step
- Leaflet 1.9.4 + Chart.js 4.4.0 via CDN
- Déploiement = `git push origin main` (GitHub Pages sur branch main, path /)

## Données GeoJSON
| Fichier | Nb features | Propriétés clés |
|---|---|---|
| `data/geojson/Cimes-contours.geojson` | 7 823 | `id`, `Haut_max`, `Haut_moy`, `Haut_min`, `Surface_2`, `Faux negat` |
| `data/geojson/Faux-positifs.geojson` | 185 | `ID`, `Surface`, `Faux posit` |

## Règles métier à respecter
- **Hauteur = toujours `Haut_max`** (coloration, filtre, KPIs, histogramme). Ne jamais utiliser `Haut_moy` pour les calculs/affichages publics.
- **Surface canopée = 81,3 ha** (valeur du rapport, hardcodée — pas recalculée depuis les données)
- **Indice de canopée = 18,5 %** (valeur du rapport, statique)

## Fonctionnalités actuelles
- Sidebar 310px : KPIs, filtre hauteur (double slider), toggles couches, légende, 2 graphiques
- Carte : cimes colorées par gradient vert selon `Haut_max`, faux positifs en rouge
- Filtre : `applyFilter()` modifie le style des layers sans reconstruire la couche (perf sur 7 823 objets)
- Graphiques : histogramme distribution `Haut_max` + donut faux positifs par catégorie
- Popups : "Hauteur" = `Haut_max`, "Haut. min." = `Haut_min`
