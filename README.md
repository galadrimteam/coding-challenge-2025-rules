# Coding Challenge 2025

## Description du jeu

Le jeu se déroule dans un labyrinthe 2D.

Le labyrinthe est généré aléatoirement.

Les joueurs sont placés aléatoirement dans le labyrinthe.

Chaque joueur commence avec 3 points de vie (PDV).

Le but du jeu est de survivre le plus longtemps possible et de tuer les autres joueurs.

Le jeu se déroule en "tours", à chaque tour, les joueurs choisissent leurs actions.

Tous les 5 tours, la zone jouable rétrécit, les joueurs qui sont encore dans la mauvaise zone perdent 1 PDV par tour.
Vous pouvez savoir quelle est la zone jouable dans la map (les 2 représentent la zone non jouable).

Les joueurs peuvent effectuer les actions suivantes :

- TRAP: pose un piège sur une case (utilisable 1 fois par tour)
- TELEPORT: se déplacer immédiatement sur une case du labyrinthe (1 fois par partie, réinitialisé à chaque kill)
- MOVE: déplace le joueur dans une direction donnée (utilisable 2 fois par tour)
- BUILD: construire un mur dans une direction donnée (utilisable 2 fois par tour)
- SHOOT: tire une balle dans une direction donnée (utilisable 2 fois par tour)
  - si la balle touche un joueur, il perd 1 PDV
  - si la balle touche un mur, il est détruit
  - la balle s'arrête au premier mur touché
 
L'ordre dans lequel est appliqué les actions est celui ci-dessus (e.g. les actions TRAP de tous les joueurs sont appliquées, puis les actions TELEPORT, puis les actions MOVE, etc)

Si le joueur programme une action qui n’est pas autorisée, l'action est ignorée.

A la fin de la partie on compte les points :

- survivre 1 tour = 1 point
- éliminer un joueur = 10 points
- être le dernier survivant = 50 points bonus

## Jouer au jeu

Pour jouer au jeu, il faut coder une fonction `turnActions` qui prend en paramètre un objet `context` et qui renvoie un array d'objet `action`.
e.g.

```ts
return [
  { action: 'TRAP', position: { x: 1, y: 1 } },
  { action: 'MOVE', direction: 'TOP' },
  { action: 'MOVE', direction: 'LEFT' },
  { action: 'BUILD', direction: 'BOTTOM' },
  { action: 'SHOOT', direction: 'LEFT' },
  { action: 'SHOOT', direction: 'RIGHT' },
]
```

Deux joueurs ne peuvent pas se trouver sur la même case.

### Context

L'objet `context` contient les informations suivantes :

- `map`: un tableau à 2 dimensions contenant les cases du labyrinthe

```js
const map = [
  [0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0],
  [0, 0, 1, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0, 1, 2, 0],
  [0, 0, 1, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 0, 0],
  [0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
]
```
Quand la zone rétrécit, l'array reste de la même taille, mais est bordé par des 2, qui représentent la lave (rester dans la lave fait perdre 1 PDV par tour).

si map.length est 10, alors le labyrinthe est de 10 cases de large

si map[0].length est 10, alors le labyrinthe est de 10 cases de long

Voici la signification des valeurs possibles pour chaque case :

- `0`: case vide
- `1`: mur

- `players`: un tableau contenant les joueurs actuels et leur position dans le labyrinthe

```js
const players = [
  {
    id: 1,
    pseudo: 'axel',
    position: { x: 0, y: 0 },
  },
  {
    id: 1,
    pseudo: 'mayo',
    position: { x: 1, y: 2 },
  },
]
```

Le coin le plus en haut à gauche de la carte a les coordonnées x = 0, y = 0

- `traps`: un tableau contenant la position des pièges actuels

```js
const traps = [
  { x: 1, y: 1, strength: 1 },
  { x: 2, y: 1, strength: 2 },
  { x: 3, y: 1, strength: 1 },
]
```

- `you` : le joueur actuel

```js
const you = {
  id: 1
  pseudo: 'axel'
  position: { x: 0, y: 0 }
  health: 2
  turnRemainingActions: {
    SHOOT: 2
    BUILD: 2
    MOVE: 2
    TRAP: 1
  }
}
```

Si votre code ne retourne rien en 3 secondes, il timeout.
