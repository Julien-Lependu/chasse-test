# 🎃 Chasse aux Monstres — Parc de la Solitude

Chasse aux monstres géolocalisée façon Pokémon Go, pensée pour un événement Halloween en famille organisé par l'association des parents d'élèves. Les enfants se déplacent dans le parc, un radar les guide vers les monstres cachés, puis une mini-chasse en réalité augmentée (caméra du téléphone) permet de les capturer.

**Aucun compte, aucune inscription** — tout fonctionne directement dans le navigateur, la progression est sauvegardée sur l'appareil.

## ✨ Fonctionnalités

- **Carte en temps réel** (OpenStreetMap / Leaflet) avec la position du joueur et le contour du parc
- **Radar de proximité** qui guide vers le monstre le plus proche, avec vibration et son
- **Capture en réalité augmentée** : le monstre apparaît à l'écran par-dessus la caméra, avec plusieurs comportements d'animation et un mini-défi (taper plusieurs fois dessus) avant capture
- **Carnet du chasseur** : chaque monstre a sa fiche avec une petite anecdote, consultable à tout moment en tapant dessus dans le sac
- **Photobooth souvenir** : photo HD avec cadre, décor Halloween, et le logo de l'association ajouté automatiquement
- **Mode organisateur caché** : appui long de 5 secondes sur le titre pour réinitialiser la partie
- **Aucune donnée envoyée à un serveur** : tout est stocké localement sur le téléphone (`localStorage`)

## 🚀 Mise en ligne (GitHub Pages)

1. Mets ce dépôt en public sur GitHub.
2. Va dans **Settings → Pages**, choisis la branche `main` et le dossier `/ (root)`.
3. GitHub te donne une URL du type `https://ton-compte.github.io/nom-du-depot/` — c'est cette adresse que tu partages aux familles (idéalement via un QR code affiché à l'entrée du parc).

⚠️ **La géolocalisation et la caméra exigent HTTPS.** GitHub Pages le fournit automatiquement, donc pas de configuration à faire de ce côté.

## 🛠️ Configuration avant l'événement

Tout se règle en haut du fichier `index.html`, dans le bloc `CONFIG À PERSONNALISER` du `<script>`.

### 1. Le contour du parc — `PARK_POLYGON`

Liste de points `[latitude, longitude]` qui délimitent la zone où les monstres peuvent apparaître. Pour le relever précisément :
- Sur le terrain : appui long dans Google Maps à chaque coin du parc, note les coordonnées affichées.
- Depuis chez toi : dessine le contour sur [geojson.io](https://geojson.io) et convertis les points obtenus (attention, GeoJSON donne `[longitude, latitude]`, l'app attend l'inverse).

### 2. Les monstres — `MONSTER_TEMPLATES`

Un tableau d'objets, un par monstre :

```js
{ name: "Citrouille Grognon", image: "monsters/citrouille.png", emoji: "🎃", fact: "Une anecdote rigolote sur ce monstre." }
```

- `image` : chemin vers un PNG dans le dossier `monsters/` (voir structure des fichiers ci-dessous). Si l'image ne charge pas, l'emoji prend automatiquement le relais.
- `fact` : le texte affiché à la capture et dans le carnet du chasseur.

Les positions exactes sont générées automatiquement à l'intérieur du `PARK_POLYGON`, avec un espacement minimum réglable via `MIN_DISTANCE_BETWEEN_MONSTERS_M`.

### 3. Le logo de l'association — `ASSOCIATION_LOGO_URL`

Dépose un fichier `logo.png` (fond transparent recommandé) à la racine du dépôt, à côté de `index.html`. Il s'affiche automatiquement sur chaque photo souvenir, sans aucune action des familles pendant l'événement.

### 4. Les distances

| Constante | Rôle | Valeur conseillée |
|---|---|---|
| `RADAR_RADIUS_M` | Distance à laquelle le radar commence à réagir | 25 m |
| `CAMERA_UNLOCK_RADIUS_M` | Distance à laquelle le bouton de capture (caméra) se débloque | 50 m |
| `MIN_DISTANCE_BETWEEN_MONSTERS_M` | Espacement minimum entre deux monstres générés | à adapter à la taille réelle du parc |

## 📁 Structure des fichiers

```
├── index.html          ← l'application (tout est dans ce seul fichier)
├── logo.png             ← logo de l'association (à ajouter, voir ci-dessus)
└── monsters/             ← images des monstres (à ajouter, voir ci-dessus)
    ├── citrouille.png
    ├── fantome.png
    ├── squelette.png
    └── ...
```

Si `logo.png` ou les images de `monsters/` sont absents, l'app fonctionne quand même (repli automatique sur les emojis, pas de logo sur les photos) — rien ne bloque le jeu.

## ✅ À vérifier avant le jour J

- [ ] Le contour vert sur la carte correspond bien aux vraies limites du parc
- [ ] Les 12 monstres ont chacun un nom, une image (ou un emoji) et une anecdote
- [ ] `logo.png` est bien déposé si tu veux le logo sur les photos
- [ ] Tester sur au moins 2 téléphones différents (idéalement un Android et un iPhone), y compris un modèle un peu ancien
- [ ] Vérifier la connexion réseau sur place : la carte a besoin d'internet pour charger les tuiles OpenStreetMap
- [ ] Prévoir que les familles autorisent la géolocalisation *et* la caméra (deux popups différents)

## 🔧 Mode organisateur

Appui long de 5 secondes sur le titre "Chasse aux Monstres" en haut de l'écran → ouvre un panneau permettant de réinitialiser toute la progression enregistrée sur cet appareil. Utile pour remettre un téléphone de prêt à zéro entre deux familles.

## 📄 Licence

Projet réalisé pour un événement associatif — libre d'utilisation et d'adaptation pour d'autres événements du même type.
