# Comparaison avec le diagramme de classes de l'équipe (`classes.md`)

Ce document compare le diagramme de classes proposé dans `classes.md` (appelé **diagramme A**) avec celui du dossier de conception (fichiers `03` à `06`, appelé **diagramme B**), et liste ce qui a été repris dans le dossier.

## 1. Points forts du diagramme A

| Point fort | Pourquoi c'est utile |
|---|---|
| **Simplicité** : une vingtaine de classes dans un seul diagramme | Facile à lire, à expliquer en soutenance et à implémenter dès le sprint 2 |
| `FaceDe` avec des **champs concrets** (cristaux, énergies, jauge, pioche, cristallisation, points d'avancement) | Se charge directement depuis un fichier, et un robot évalue une face en une ligne. Plus simple que la liste d'actions du diagramme B |
| `TableCristallisation` séparée, `Saison` qui expose énergies abondantes, rares, introuvable | Traduit directement le tableau de rareté de la page 16 des règles |
| `ScoreBilan` et `CalculateurScore` | Le détail du score (cristaux, prestige, malus main, malus bonus) est explicite |
| `Joueur.debloquerCartesAnnee(annee)` | Nom clair pour la gestion des jetons bibliothèque |
| Deux robots « intelligents » aux **heuristiques distinctes** (cristallisation, prestige) | Donne tout de suite des types de robots comparables dans les 500 parties |
| Justification SOLID rédigée | Argumentaire prêt pour le rapport |

## 2. Défauts du diagramme A

| Défaut | Conséquence |
|---|---|
| **Pas de `Manche`, `Tour`, `Action`** : tout est dans `MoteurJeu` | Classe « fourre-tout » ; impossible de modéliser proprement l'ordre des actions d'un tour |
| **Interface `DecisionBot` trop pauvre** : `choisirCarteAInvoquer` ne renvoie qu'une carte | On ne peut pas invoquer plusieurs cartes par tour dans un ordre voulu (Bâton du printemps avant les autres cartes, par exemple), ni enchaîner invocations, activations, bonus et cristallisation |
| `DecisionBot` **manque de décisions** : répartition des 9 cartes en 3 paquets, carte piochée à garder, choix d'adversaire, d'énergie « de votre choix », de carte à sacrifier | Les cartes 2, 9, 10, 12, 16, 17, 21, 24, 25 ne sont pas implémentables |
| Les robots reçoivent `Joueur` et `Plateau` **modifiables** | Un robot peut tricher ou voir la main des adversaires |
| **Pas de mécanisme d'événements** | Les effets permanents (Bâton, Coffret, Corne, Figrim, Sablier, Bourse d'Io) n'ont aucun point d'accroche. L'affichage « tout au long de la partie » n'a pas de source non plus |
| `TypeEffet` à une seule valeur par carte, sans effet de **fin de partie** | Le Grimoire ensorcelé (arrivée en jeu **et** permanent) et le Heaume de Ragfield (fin de partie) ne rentrent pas |
| **Héritage sur le mauvais axe** : `ObjetMagique` / `Familier` en sous-classes **et** `TypeCarte` en énumération | Doublon ; surtout, ce n'est pas la catégorie qui change le comportement mais l'effet. Il faudrait 30 sous-classes ou un gros `switch` |
| `ContexteJeu` est utilisé par `CartePouvoir` mais **n'est défini nulle part** | Diagramme incomplet |
| `ReserveEnergie.CAPACITE_MAX` **constante = 7** | Le Grimoire ensorcelé (capacité 10) est impossible |
| `MoteurJeu "1" o-- "3" Joueur` | Le jeu se joue à 2, 3 ou 4 joueurs |
| `Joueur` annoncé « passif » mais porte `invoquerCarte` et `peutInvoquer()` (sans paramètre) | Contradiction avec la justification ; les règles d'invocation (coût réduit, jauge, effets) ne peuvent pas y tenir |
| `Plateau.avancerTemps` renvoie un simple `boolean` | On ne sait pas s'il y a eu changement de saison ou d'année (nécessaire pour Figrim, Sablier, jetons bibliothèque) |
| Pas de **pioche, défausse, stock d'énergie, catalogue de cartes, jeux pré-construits** | La mise en place du niveau débutant n'est pas modélisée |
| Pas de **package de simulation ni d'affichage** | Les 500 parties et la visualisation textuelle, deux exigences du sujet, n'apparaissent pas |
| Départage d'égalité « puis au plus grand nombre de cristaux » | Cette règle n'existe pas dans le livret (seul le nombre de cartes invoquées compte) : à faire valider par le client (Q8) |
| `Map~Joueur, DecisionBot~` : virgule dans un générique Mermaid | Peut casser le rendu selon la version de Mermaid |

## 3. Points forts du diagramme B (dossier de conception)

- Couvre **toutes les exigences du sujet** : modèle, moteur, robots, simulation, affichage.
- Moteur découpé (`Partie`, `Manche`, `Tour`, `Regles`) ; le robot choisit **une action légale à la fois** parmi celles proposées, sans pouvoir tricher.
- Effets de cartes extensibles (arrivée, permanent, activation, fin de partie) avec événements, adaptés aux 30 cartes y compris les interactions du lexique.
- Prêt pour une **inflexion** du client (`StrategieDistribution` pour un futur draft, capacité de réserve variable, nombre de joueurs libre).

## 4. Points faibles du diagramme B (à surveiller)

| Risque | Mesure |
|---|---|
| Plus de classes (une soixantaine avec les effets) : risque de **sur-conception** pour 8 sprints | Livraison progressive : seules les classes du sprint sont codées (voir `matrice-elements-robots.md` et `backlog.md`) |
| `ContexteEffet` volumineux | Découpé en trois interfaces (`OperationsRessources`, `OperationsCartes`, `OperationsTemps`) |
| `Regles.actionsLegales` est la partie la plus difficile | Implémentée par morceaux : sprint 3 (cristalliser), sprint 4 (invoquer), sprint 5 (activer, bonus) |
| Plus abstrait, moins lisible d'un coup d'œil | Un diagramme par thème, chacun de moins de 15 classes |

## 5. Ce qui a été repris du diagramme A dans le dossier

| Élément repris | Fichiers modifiés |
|---|---|
| `FaceDe` à champs concrets ; suppression de `ActionDe` et `TypeActionDe` | `03-classes-modele.md`, `07-sequences.md`, `backlog.md` |
| `TableCristallisation` ; `Saison` expose énergies courantes, peu courantes, introuvable | `03-classes-modele.md`, `05-classes-moteur.md`, `07-sequences.md`, `backlog.md`, `matrice-elements-robots.md` |
| `Joueur.debloquerCartesAnnee(annee)` | `03-classes-modele.md` |
| Deux heuristiques distinctes pour les robots : `RobotGlouton` = cristallisation, `RobotStrategique` renommé `RobotPrestige` = prestige | `06-classes-robots-simulation.md`, `matrice-elements-robots.md`, `backlog.md`, `02-architecture-packages.md`, `PLANNING.md` |
| Justification d'une partie des choix en termes de principes SOLID | `conception.md`, `04-classes-cartes.md` |
| Question sur le départage supplémentaire par cristaux | `conception.md` (Q8) |

## 6. Ce qui a été volontairement gardé du diagramme B

- Interface `Robot` avec **choix d'une action légale à la fois** et vues en lecture seule.
- `Manche`, `Tour`, `Regles`, `BusEvenements` et les événements.
- Effets de cartes en classes distinctes plutôt qu'en sous-classes de la catégorie.
- Packages `simulation` et `affichage`.
- Capacité de réserve variable, de 2 à 4 joueurs.
