# L'Oie Néon × Sanrio

Jeu de l'oie personnalisable réalisé pendant l'atelier « Créer un jeu de l'oie avec l'aide de l'IA », en s'inspirant de [VibeOie](https://github.com/tspriet/VibeOie).

![Plateau](capture.png)

## Lancer le jeu
Ouvrez `index.html` dans un navigateur (Chrome, Firefox, Edge, Safari). Il n'y a rien à installer.

## En bref
- 2 à 10 joueurs, chacun avec un pseudo et une oie déguisée en personnage Sanrio (10 costumes dessinés en code).
- Plateau de 55 cases en spirale de Fibonacci, avec l'arrivée au centre (🏰 Sanrio Puroland).
- 2 dés et un rebond si on dépasse la case 55.
- Cases spéciales : 🎀 Nœud de Hello Kitty, ☁️ Nuage de Cinnamoroll, 💀 Piège de Kuromi, 🍳 Sieste de Gudetama, 🐈‍⬛ Labyrinthe de Chococat, 🐸 Flaque de Keroppi, 🎁 Cadeau Sanrio.
- 10 cartes actions à garder en main, comme au Mille Bornes (pioche de 40) : 3 au maximum, 1 jouée par tour avant de lancer les dés. Dernière ajoutée : 🔄 On recommence, qui renvoie un adversaire en case 0.
- **Réglages** avant la partie : quantité de cartes (Peu / Normal / Beaucoup) et vitesse des pions (Lente / Normale / Rapide).
- **Barre d'outils** toujours visible : 📜 Règles, 📖 Catalogue, 🔁 Nouvelle partie avec les mêmes joueurs, ✏️ Changer les joueurs.

Les règles complètes sont dans [CAHIER_DES_CHARGES.md](CAHIER_DES_CHARGES.md) et dans le bouton « 📜 Règles » du jeu.

## Ajouter une carte
Inspiré du `registerGooseAction` de VibeOie, une seule fonction suffit, sans toucher au reste du moteur :

```js
registerCard({
  id: "bisou",                       // identifiant unique
  name: "Bisou de Hello Kitty",
  description: "Avancez de 2 cases.",
  icon: "💋",
  color: "#ff5fa2",                  // facultatif (couleur du type par défaut)
  type: "bonus",                     // "offensive" | "défensive" | "bonus"
  copies: 4,                         // exemplaires dans la pioche (facultatif, 4 par défaut)
  async execute(p, api) {            // p = joueur qui joue la carte
    await api.moveSteps(p, 2, 1);
    await api.resolveCell(p, 2, 1);
    return true;                     // false = annulée, la carte reste en main
  },
  // facultatif : canPlay(p, api) → "" si jouable, sinon la raison ; playableWhenBlocked: true/false
});
```

`api` donne accès à l'état (`api.G`) et aux outils du moteur : `log`, `tag`, `chooseOpponent`, `attack` (vérifie Mélodie puis Câlin), `moveSteps`, `jumpTo`, `resolveCell`, `isBlocked`, `playerAhead`, `showModal`…
La pioche, la main, les Règles et le Catalogue prennent la nouvelle carte en compte automatiquement. Les 10 cartes du jeu sont définies de cette façon, en bas du script (section 10).

## Tests réalisés
Tests automatiques dans un vrai navigateur (Playwright), relancés après chaque étape :
- **Saisie** : nombres refusés (1, 11, 0, -3, 2.5, vide), pseudo vide, pseudos en double, costumes en double.
- **Règles** (40 vérifications) : rebond, victoire pile sur 55, Nœud de Hello Kitty (y compris les enchaînements), Nuage de Cinnamoroll, Piège de Kuromi (2 tours), Sieste de Gudetama (libération par un autre joueur), Labyrinthe de Chococat, Flaque de Keroppi. Pour les cartes : les effets, une carte par tour, l'annulation, les protections (Mélodie puis Câlin) et la défausse quand la main est pleine.
- **Vitesse des pions** (8) : dés et cartes bloqués pendant les déplacements, pauses par case et par saut.
- **Réglages** (16) : cases Cadeau affichées et actives selon Peu / Normal / Beaucoup, aucune sur une case déjà spéciale, vitesses, réglages conservés pour « Rejouer ».
- **Boutons** (27) : Règles et Catalogue ouverts pendant un déplacement sans perturber le tour, par-dessus une fenêtre de choix de cible, fermeture (✕, Échap, clic à côté). « Nouvelle partie avec les mêmes joueurs » : confirmation, remise à zéro, nouvelle pioche, réglages gardés, bouton bloqué pendant une action.
- **Carte « On recommence » et registre** (22) : cas normal, cible protégée par la Mélodie, par le Câlin, cible bloquée (Sieste, Piège + Bâillement) libérée, cible en case 0 grisée, aucun adversaire valide, joueur bloqué qui la joue, refus des cartes mal déclarées, carte ajoutée depuis l'extérieur.
- 12 parties complètes jouées au hasard sans erreur, et affichage vérifié sur téléphone (390 px).

Le jeu expose `window.OIE` (état du jeu, dés forcés, réglages) pour les tests automatiques.
