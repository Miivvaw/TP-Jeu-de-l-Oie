# Cahier des charges — L'Oie Néon × Sanrio (jeu de l'oie en spirale de Fibonacci)

## 1. Fonctionnalités

### Mise en place
- Saisie du nombre de joueurs (2 à 10). Tout nombre hors de cette plage est refusé, avec un message d'erreur.
- Chaque joueur saisit son **pseudo** (obligatoire, 16 caractères maximum, pas de doublon) et choisit le **costume Sanrio** de son oie (un costume par joueur). Un aperçu de l'oie déguisée, avec le nom du personnage, s'affiche à côté.
- Une zone **Réglages** :

| Réglage | Choix | Effet |
|---------|-------|-------|
| Quantité de cartes | Peu | 3 cases Cadeau : 5, 13, 34 |
| | **Normal** (par défaut) | 5 cases Cadeau : 5, 8, 13, 21, 34 |
| | Beaucoup | 8 cases Cadeau : les 5 normales + 16, 37, 49 (cases qui n'étaient pas spéciales) |
| Vitesse des pions | Lente | 600 ms par case, 1 100 ms par saut |
| | **Normale** (par défaut) | 400 ms par case, 800 ms par saut |
| | Rapide | 200 ms par case, 450 ms par saut |

- Les réglages sont conservés pour « Rejouer » et « Nouvelle partie avec les mêmes joueurs ». Ils ne changent ni la limite de 3 cartes en main, ni la règle d'une carte par tour, ni les effets des cartes, ni les autres cases spéciales.
- Un bouton « Lancer la partie ».

### Déroulement
- Plateau de **55 cases** (nombre de Fibonacci). Départ en case 0, arrivée en case 55, au centre de la spirale.
- On lance **2 dés** (2 à 12) et le pion se déplace case par case, avec une animation.
- **Rebond** : si on dépasse 55, on recule du nombre de cases en trop (exemple : on est en 50 et on fait 8, on va jusqu'à 55, puis on revient en 52).
- Plusieurs pions peuvent occuper la même case.

### Cases spéciales
| Case | Effet | Numéros |
|------|-------|---------|
| 🎀 Nœud de Hello Kitty (oie) | On avance encore du même nombre de cases | 10, 23, 29, 41, 47 (irréguliers, pour éviter un enchaînement automatique avec un seul lancer) |
| ☁️ Nuage de Cinnamoroll (pont) | On s'envole de la case 6 à la case 12 | 6 |
| 💀 Piège de Kuromi (prison) | On passe 2 tours | 19 |
| 🍳 Sieste de Gudetama (puits) | Bloqué jusqu'à ce qu'un autre joueur tombe dedans (il prend alors la place) | 31 |
| 🐈‍⬛ Labyrinthe de Chococat | On recule à la case 30 | 42 |
| 🐸 Flaque de Keroppi (mort) | On glisse jusqu'à la case départ | 52 |
| 🎁 Cadeau Sanrio | On pioche une carte action | 5, 8, 13, 21, 34 (selon le réglage « Quantité de cartes ») |

### Cartes actions (style Mille Bornes)
- On les obtient **uniquement** en tombant sur une case Carte.
- On peut avoir **3 cartes au maximum** en main. Si on en pioche une quatrième, on doit choisir laquelle défausser.
- On peut jouer **1 carte par tour, avant de lancer les dés**. Tout se joue à la main, y compris le bouclier, qu'on active à l'avance.
- Pioche de **40 cartes** (4 exemplaires de chacune des 10 cartes), remélangée quand elle est vide.

| Type | Carte | Effet |
|------|-------|-------|
| Offensive | 😈 Farce de Kuromi | Un adversaire recule de 5 cases |
| Offensive | 🥱 Bâillement de Gudetama | Un adversaire passe son prochain tour |
| Offensive | 🌠 Vœu de Kiki & Lala | On échange sa place avec le joueur juste devant |
| Défensive | 🤍 Câlin de Cinnamoroll (bouclier) | Absorbe la prochaine attaque ou le prochain piège (Kuromi, Gudetama, Chococat, Keroppi) |
| Défensive | 🍮 Pudding de Pompompurin | Libère immédiatement d'un blocage : piège, sieste ou bâillement (seulement quand on est bloqué) |
| Défensive | 🎵 Mélodie de My Melody | Immunisé contre les cartes adverses pendant 2 tours |
| Bonus | 🏀 Sprint de Pochacco | Avancer immédiatement de 4 cases |
| Bonus | 🍎 Pomme de Hello Kitty | Relancer les dés après son déplacement |
| Bonus | 🎲 Triche de Badtz-Maru | Choisir la valeur d'un des deux dés |
| Offensive | 🔄 On recommence | Un adversaire retourne immédiatement à la case 0 |

- Quand on est bloqué, on peut seulement jouer la Farce, le Bâillement, On recommence, le Câlin, la Mélodie ou le Pudding (pas le Vœu ni les cartes bonus).
- Les déplacements imposés par une carte (Farce, Vœu, On recommence) ne déclenchent pas les cases spéciales. Un joueur qu'on fait sortir de la Sieste de Gudetama est libéré.
- La Mélodie est vérifiée avant le Câlin. Une carte bloquée est perdue.
- Si une carte mène le joueur dans un piège (par exemple le Sprint), son tour s'arrête.

**Détails de « On recommence »**
- Le joueur choisit un adversaire, qui retourne immédiatement en case 0, sans déclencher de case spéciale.
- Si la cible était bloquée (Sieste, Piège, Bâillement), elle est libérée.
- La Mélodie protège en premier, puis le Câlin absorbe l'attaque, comme pour les autres cartes offensives.
- Un adversaire déjà en case 0 ne peut pas être choisi. S'il n'y a aucune cible valide, la carte ne peut pas être jouée.
- Un joueur bloqué peut la jouer, comme la Farce et le Bâillement.

### Boutons toujours disponibles
Une barre en haut de l'écran reste visible, même par-dessus une fenêtre de jeu :
- **📜 Règles** : déroulement d'un tour, rebond, cases spéciales et cartes. S'ouvre et se ferme (✕, Échap ou clic à côté) sans interrompre la partie.
- **📖 Catalogue** : toutes les cases spéciales (avec leurs numéros) et toutes les cartes (icône, nom, type, nombre d'exemplaires, effet).
- **🔁 Nouvelle partie avec les mêmes joueurs** : mêmes pseudos, costumes et réglages ; plateau, pioche et mains remis à zéro. Une confirmation est demandée si la partie est en cours.
- **✏️ Changer les joueurs** : retour à la mise en place.
- Les deux boutons de relance sont bloqués pendant un déplacement ou une action en cours.

### Fin de partie
- Le premier joueur qui arrive exactement sur la case 55 (🏰 Sanrio Puroland) gagne.
- Un écran de victoire s'affiche avec le classement et les boutons « Rejouer avec les mêmes joueurs » et « Nouvelle partie (changer les joueurs) ».

## 2. Conditions techniques
- Jeu web en **HTML / CSS / JavaScript**, dans un seul fichier `index.html`, sans installation ni serveur.
- Fonctionne dans un navigateur récent (Chrome, Firefox, Edge, Safari), sur Windows, macOS et Linux.
- Pourra être publié sur GitHub Pages.
- **Ajouter une carte** : appeler `registerCard({ id, name, description, icon, color, type, copies, execute })`, sans modifier le reste du moteur. `type` vaut `"offensive"`, `"défensive"` ou `"bonus"`. `execute(p, api)` renvoie `true` si la carte est jouée, `false` si le joueur annule. Deux champs sont facultatifs : `canPlay(p, api)` (raison pour laquelle la carte est injouable, ou `""`) et `playableWhenBlocked`. La pioche, la main, les Règles et le Catalogue prennent la carte en compte automatiquement. Les 10 cartes du jeu sont définies de cette façon.

## 3. Aspect graphique
- Ambiance **Kuromi néon** : fond noir, néons violets avec des touches de rose (effets lumineux, `text-shadow` / `box-shadow`). Références Sanrio partout (cases, cartes, pions).
- Plateau dessiné en **spirale de Fibonacci (spirale d'or)**, de l'extérieur (départ) vers le centre (case 55).
- Chaque case spéciale a son icône et sa couleur néon.
- Pions : oies **déguisées en personnages Sanrio**, dessinées en code (sans image), avec un corps de couleur claire, un contour néon et un halo pour le joueur actif. Accessoires : gros nœud rouge (Hello Kitty), capuche noire à oreilles pointues et tête de mort rose (Kuromi), capuche rose à longues oreilles et fleur (My Melody), longues oreilles tombantes et queue en roulé (Cinnamoroll), béret marron (Pompompurin), oreilles noires et ballon de basket (Pochacco), gros yeux de grenouille (Keroppi), épis noirs (Badtz-Maru), œuf au plat et bacon (Gudetama), deux étoiles jaune et rose (Little Twin Stars).
- Les pions sont environ 1,5 fois plus grands qu'au départ, sans déborder des cases, et restent lisibles quand plusieurs sont sur la même case. La même fonction de dessin sert pour le plateau, l'aperçu de l'écran de choix et le petit pion à côté du pseudo dans le panneau.
- Un panneau latéral affiche le joueur dont c'est le tour, les dés, la main de cartes et un journal des événements.

## 4. Méthode
Développement par étapes, chacune testée avant de passer à la suivante :
1. Saisie des joueurs (nombre, pseudos, costumes)
2. Plateau en spirale
3. Dés et déplacements (avec le rebond)
4. Cases spéciales
5. Cartes actions
6. Victoire et bouton « Rejouer »

Puis modifications, chacune validée par toutes les vérifications avant la suivante :

7. Pions plus lents (`STEP_DELAY`, `JUMP_DELAY`)
8. Pions plus voyants et reconnaissables
9. Réglages (quantité de cartes, vitesse des pions)
10. Boutons Règles, Catalogue et « Nouvelle partie avec les mêmes joueurs »
11. Registre `registerCard` et carte « On recommence »
