[CAHIER_DES_CHARGES.md](https://github.com/user-attachments/files/33814407/CAHIER_DES_CHARGES.md)
# Cahier des charges — L'Oie Néon × Sanrio

Jeu de l'oie en spirale de Fibonacci.

## 1. Fonctionnalités

### Mise en place

- Saisie du nombre de joueurs (2 à 10). Tout nombre hors de cette plage est refusé, avec un message d'erreur.
- Chaque joueur saisit son pseudo (obligatoire, 16 caractères maximum, pas de doublon) et choisit le costume Sanrio de son oie (un costume par joueur).
- Un bouton « Lancer la partie ».

### Déroulement

- Plateau de 55 cases (nombre de Fibonacci). Départ en case 0, arrivée en case 55, au centre de la spirale.
- On lance 2 dés (2 à 12) et le pion se déplace case par case, avec une animation.
- **Rebond** : si on dépasse 55, on recule du nombre de cases en trop (exemple : on est en 50 et on fait 8, on va jusqu'à 55, puis on revient en 52).
- Plusieurs pions peuvent occuper la même case.

### Cases spéciales

| Case | Effet | Numéros |
|---|---|---|
| 🎀 Nœud de Hello Kitty (oie) | On avance encore du même nombre de cases | 10, 23, 29, 41, 47 (irréguliers, pour éviter un enchaînement automatique avec un seul lancer) |
| ☁️ Nuage de Cinnamoroll (pont) | On s'envole de la case 6 à la case 12 | 6 |
| 💀 Piège de Kuromi (prison) | On passe 2 tours | 19 |
| 🍳 Sieste de Gudetama (puits) | Bloqué jusqu'à ce qu'un autre joueur tombe dedans (il prend alors la place) | 31 |
| 🐈‍⬛ Labyrinthe de Chococat | On recule à la case 30 | 42 |
| 🐸 Flaque de Keroppi (mort) | On glisse jusqu'à la case départ | 52 |
| 🎁 Cadeau Sanrio | On pioche une carte action | 5, 8, 13, 21, 34 |

### Cartes actions (style Mille Bornes)

- On les obtient uniquement en tombant sur une case Carte.
- On peut avoir 3 cartes au maximum en main. Si on en pioche une quatrième, on doit choisir laquelle défausser.
- On peut jouer 1 carte par tour, avant de lancer les dés. Tout se joue à la main, y compris le bouclier, qu'on active à l'avance.
- Pioche de 36 cartes (4 exemplaires de chacune des 9 cartes), remélangée quand elle est vide.

| Type | Carte | Effet |
|---|---|---|
| Offensive | 😈 Farce de Kuromi | Un adversaire recule de 5 cases |
| Offensive | 🥱 Bâillement de Gudetama | Un adversaire passe son prochain tour |
| Offensive | 🌠 Vœu de Kiki & Lala | On échange sa place avec le joueur juste devant |
| Défensive | 🤍 Câlin de Cinnamoroll (bouclier) | Absorbe la prochaine attaque ou le prochain piège (Kuromi, Gudetama, Chococat, Keroppi) |
| Défensive | 🍮 Pudding de Pompompurin | Libère immédiatement d'un blocage : piège, sieste ou bâillement (seulement quand on est bloqué) |
| Défensive | 🎵 Mélodie de My Melody | Immunisé contre les cartes adverses pendant 2 tours |
| Bonus | 🏀 Sprint de Pochacco | Avancer immédiatement de 4 cases |
| Bonus | 🍎 Pomme de Hello Kitty | Relancer les dés après son déplacement |
| Bonus | 🎲 Triche de Badtz-Maru | Choisir la valeur d'un des deux dés |

**Règles particulières**

- Quand on est bloqué, on peut seulement jouer une carte offensive, le Câlin, la Mélodie ou le Pudding.
- Les déplacements imposés par une carte (Farce, Vœu) ne déclenchent pas les cases spéciales.
- Un joueur qu'on fait sortir de la Sieste de Gudetama est libéré.
- La Mélodie est vérifiée avant le Câlin.
- Une carte bloquée est perdue.

### Fin de partie

- Le premier joueur qui arrive exactement sur la case 55 (🏰 Sanrio Puroland) gagne.
- Un écran de victoire s'affiche, avec un bouton « Rejouer ».

## 2. Conditions techniques

- Jeu web en HTML / CSS / JavaScript, dans un seul fichier `index.html`, sans installation ni serveur.
- Fonctionne dans un navigateur récent (Chrome, Firefox, Edge, Safari), sur Windows, macOS et Linux.
- Pourra être publié sur GitHub Pages.

## 3. Aspect graphique

- Ambiance Kuromi néon : fond noir, néons violets avec des touches de rose (effets lumineux, `text-shadow` / `box-shadow`). Références Sanrio partout (cases, cartes, pions).
- Plateau dessiné en spirale de Fibonacci (spirale d'or), de l'extérieur (départ) vers le centre (case 55).
- Chaque case spéciale a son icône et sa couleur néon.
- Pions : petites oies néon déguisées en personnages Sanrio (nœud de Hello Kitty, chapeau de Kuromi, capuche de My Melody, oreilles de Cinnamoroll, béret de Pompompurin, oreilles de Pochacco, yeux de Keroppi, épis de Badtz-Maru, œuf de Gudetama, étoile de Little Twin Stars), chacune avec sa couleur.
- Un panneau latéral affiche le joueur dont c'est le tour, les dés, la main de cartes et un journal des événements.

## 4. Méthode

Développement par étapes, chacune testée avant de passer à la suivante :

1. Saisie des joueurs (nombre, pseudos, costumes)
2. Plateau en spirale
3. Dés et déplacements (avec le rebond)
4. Cases spéciales
5. Cartes actions
6. Victoire et bouton « Rejouer »
