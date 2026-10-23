# Démarche : créer « L'Oie Néon × Sanrio » avec l'IA

TP d'atelier : concevoir un jeu de l'oie personnalisé en m'appuyant sur une IA générative.
Ce document explique honnêtement comment j'ai travaillé avec l'IA : ce que j'ai décidé, ce que l'IA a proposé ou écrit, comment j'ai testé, et ce que j'ai demandé de modifier.

## Outils IA utilisés

- **Claude (chat)** : mon assistant pour organiser la démarche, me poser des questions de conception, rédiger le cahier des charges à partir de mes idées et rédiger des prompts que je peux modifier.
- **Claude Code** : génération et modification du code du jeu, à partir de mes prompts.

## Ma méthode de travail

Je voulais éviter le « fais-moi un jeu de l'oie » en une phrase, qui donne un résultat générique et incontrôlable. J'ai donc procédé ainsi :

1. **Je me suis fait expliquer le sujet** : ce que le TP demande, comment cloner le jeu exemple `VibeOie` et créer mon propre dépôt GitHub.
2. **Je ne savais pas encore précisément ce que je voulais** : j'ai donc demandé à Claude de **me poser des questions**, une à la fois, pour décider de chaque aspect du jeu (ambiance, plateau, cartes, cases spéciales, pions). Je répondais, parfois avec mes propres idées, parfois en choisissant parmi des options proposées.
3. **J'ai demandé des prompts modifiables** : Claude a rédigé des prompts à copier-coller dans Claude Code, que je pouvais adapter (retirer des lignes, changer des valeurs, ajouter mes demandes). L'objectif était de garder la main sur mes besoins et de faire du « vibe coding » de façon maîtrisée : un prompt précis, étape par étape, avec des précautions pour ne pas casser ce qui marche.
4. **J'ai testé et réajusté** : à chaque résultat, je regardais ce qui ne me convenait pas et je le redemandais.

## Qui a décidé quoi

| Sujet | Choix retenu | Origine |
|---|---|---|
| Ambiance | Sombre : noir avec néons violets | Mon idée |
| Forme du plateau | Spirale de Fibonacci (spirale d'or) | Mon idée |
| Thème Sanrio | Cases, cartes et pions en personnages Sanrio | Mon idée |
| Nombre de cases | 55 (nombre de Fibonacci) | Choisi parmi les options proposées par Claude (55, 63 ou 89) |
| Obtention des cartes | En tombant sur des cases Cadeau | Choisi parmi les options proposées |
| Types de cartes | Mix équilibré : offensives, défensives, bonus | Choisi parmi les options proposées |
| Pions | Oies stylisées en néon | Choisi parmi les options proposées |
| Cases spéciales | Cases classiques réinventées dans l'univers du jeu | Je ne savais pas quoi choisir : Claude m'a fait une proposition, que j'ai validée |
| Carte « On recommence » | Un adversaire de mon choix retourne à la case 0 | Mon idée de carte ; l'effet choisi parmi les options proposées |
| Après test : pions plus lents, costumes plus reconnaissables, quantité de cartes réglable, options inspirées de `VibeOie` | Voir « Modifications demandées » | Mes demandes après avoir joué |
| Images pour les pions | Abandonnées : dessins en code, plus voyants | Ma décision après essai |

Le **cahier des charges** complet est dans [`CAHIER_DES_CHARGES.md`](CAHIER_DES_CHARGES.md). Ce sont mes idées et mes choix, mais il a été **rédigé par l'IA** à partir de ce que je lui ai dit, pas par moi.

## Génération du jeu

J'ai donné mon cahier des charges à Claude Code et lui ai demandé de procéder par étapes (joueurs, plateau, déplacements, cases spéciales, cartes, victoire).

D'après le rapport de Claude Code, la première version comprenait la saisie de 2 à 10 joueurs avec pseudo et costume, le plateau de 55 cases en spirale, les cases spéciales, les 9 cartes actions et l'écran de victoire. Il annonçait aussi 40 vérifications de règles automatiques et 12 parties simulées sans erreur.

Ce rapport vient de l'IA : je ne m'en suis pas contentée et j'ai joué moi-même pour tester.

## Vérifications fonctionnelles

Grille de test utilisée, d'après les cinq questions du sujet :

| Question du sujet | Ce que je devais contrôler |
|---|---|
| Est-ce que ça marche ? | Le jeu s'ouvre, une partie se lance et se termine |
| Toutes les fonctionnalités sont-elles ok ? | Cases spéciales, cartes, rebond sur la case 55, écran de victoire |
| Plusieurs fois de suite ? | « Rejouer » et « Nouvelle partie » enchaînés |
| Actions inattendues ? | Nombre de joueurs invalide, pseudos ou costumes en double, clics rapides, rechargement de la page |
| Ergonomie correcte ? | Lisibilité, boutons clairs, affichage sur petite fenêtre |

**Ce que j'ai observé en jouant à la première version :**

1. Les pions se déplaçaient **trop vite**.
2. Les **costumes** n'étaient pas assez voyants ni reconnaissables.
3. Je voulais pouvoir **choisir la quantité de cartes** en jeu, comme dans le jeu exemple.
4. Un cas limite de mes propres règles : un rebond depuis la case 55 peut tomber sur la case 52 (Flaque de Keroppi), il fallait préciser ce qui se passe.

**Résultat sur la version finale :** j'ai joué à la nouvelle version après les modifications et je n'ai pas constaté de bug : tout fonctionne.

## Modifications demandées à l'IA

Le sujet demande des prompts précis qui préviennent les effets de bord. Claude a rédigé les prompts à partir de mes demandes, en nommant les fonctions concernées du code, en disant ce qu'il ne faut pas changer, et en exigeant de relancer les vérifications après chaque étape. Je les ai adaptés avant de les envoyer à Claude Code.

| # | Modification | Précautions prévues dans le prompt |
|---|---|---|
| 1 | **Ralentir les pions** : pause de 170 → 400 ms par case, durées dans des constantes | Garder `animSpeed` à 0 pour les tests, boutons bloqués pendant les déplacements |
| 2 | **Pions plus voyants** et costumes plus reconnaissables (dessins en code) | Garder `drawGoose` unique pour le plateau et l'aperçu, pions lisibles quand ils partagent une case, 10 costumes sans doublon |
| 3 | **Réglages avant la partie** : quantité de cartes (Peu / Normal / Beaucoup) et vitesse des pions | « Normal » = comportement actuel, limite de 3 cartes et effets des cartes inchangés |
| 4 | **Boutons Règles, Catalogue, « Nouvelle partie avec les mêmes joueurs »**, inspirés du jeu exemple | Fenêtres qui ne perturbent ni les tours ni les modales existantes |
| 5 | **Mécanisme d'ajout d'actions** (`registerCard`) puis nouvelle carte **« On recommence » 🔄** | Les 9 cartes existantes doivent fonctionner à l'identique |

**Carte « On recommence »** (offensive) : le joueur choisit un adversaire qui retourne à la case 0. Règles prévues pour éviter les effets de bord : aucune case spéciale déclenchée, la Mélodie est vérifiée avant le Câlin, une cible bloquée est libérée, une cible déjà en case 0 ne peut pas être choisie, 4 exemplaires dans la pioche. Tests demandés : cible protégée par Mélodie, par Câlin, cible bloquée, cible en case 0, aucun adversaire valide.

**Résultat des modifications :**

- [x] Pions ralentis
- [x] Pions plus reconnaissables
- [x] Réglages avant la partie
- [x] Boutons Règles / Catalogue / Nouvelle partie
- [x] Carte « On recommence » testée

## Ce que l'IA a fait, ce que j'ai fait

**Moi :** les idées et les choix de conception (ambiance, Fibonacci, thème Sanrio, carte « On recommence »), le choix parmi les options proposées, les tests en jouant, les demandes de correction, l'abandon des images, la gestion du dépôt GitHub.

**L'IA :** les questions et propositions de conception, la rédaction du cahier des charges et de ce journal à partir de mes réponses, la rédaction des prompts, l'écriture du code (Claude Code).

## Limites et précautions

- Le code généré par l'IA doit toujours être relu et testé : un rapport de tests automatiques ne remplace pas un test à la main.
- Quand l'IA a dû choisir une règle à ma place (par exemple, si le Sprint de Pochacco fait tomber dans un piège, le tour s'arrête), je l'ai notée pour la valider ou la corriger.
- Les personnages Sanrio sont des marques déposées : le jeu est un projet pédagogique, et les accessoires sont dessinés en code, sans images officielles.
