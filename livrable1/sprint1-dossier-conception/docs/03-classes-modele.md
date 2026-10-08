# 03 — Diagramme de classes : modèle du jeu

Package `fr.seasons.modele`. Ces classes représentent **l'état** du jeu (plateau, dés, joueurs, pions). Elles ne contiennent que les règles locales (par exemple : une réserve ne dépasse pas sa capacité). Les règles de déroulement sont dans le moteur.

## 1. Énumérations et valeurs

```mermaid
classDiagram
    class Energie {
        <<enumeration>>
        AIR
        EAU
        FEU
        TERRE
    }
    class Saison {
        <<enumeration>>
        HIVER
        PRINTEMPS
        ETE
        AUTOMNE
        +rarete(Energie e) Rarete
        +energiesCourantes() List~Energie~
        +energiesPeuCourantes() List~Energie~
        +energieIntrouvable() Energie
        +suivante() Saison
    }
    class TableCristallisation {
        +taux(Saison s, Energie e, boolean bonusPlusUn) int
    }
    class Rarete {
        <<enumeration>>
        COURANT
        PEU_COURANT
        INTROUVABLE
        +cristaux() int
    }
    class TypeBonus {
        <<enumeration>>
        ECHANGE_ENERGIES
        CRISTALLISATION_PLUS_UN
        JAUGE_PLUS_UN
        PIOCHE_DOUBLE
    }
    Saison ..> Energie : rareté par énergie
    Saison ..> Rarete
    TableCristallisation ..> Saison
    TableCristallisation ..> Rarete
```

**Cours des énergies** (page 16 des règles, calculé par `TableCristallisation`) : `COURANT` = 1 cristal, `PEU_COURANT` = 2, `INTROUVABLE` = 3.

| Saison | Courant (1) | Peu courant (2) | Introuvable (3) |
|---|---|---|---|
| Hiver | Eau, Air | Feu | Terre |
| Printemps | Terre, Eau | Air | Feu |
| Été | Terre, Feu | Eau | Air |
| Automne | Feu, Air | Terre | Eau |

## 2. Plateau, dés, pioche

```mermaid
classDiagram
    class Plateau {
        +RoueDesSaisons roue
        +EchelleDesAnnees annees
        +PisteDesCristaux cristaux
        +StockEnergie stock
        +Pioche pioche
        +Defausse defausse
        +des() List~De~
        +desDeLaSaison(Saison s) List~De~
    }
    class RoueDesSaisons {
        -int position
        +saisonCourante() Saison
        +position() int
        +avancer(int cases) ResultatAvancee
        +reculer(int cases) ResultatAvancee
    }
    class ResultatAvancee {
        <<record>>
        Saison nouvelleSaison
        boolean saisonChangee
        boolean anneeChangee
        boolean finDepassee
    }
    class EchelleDesAnnees {
        -int annee
        +anneeCourante() int
        +avancer()
        +reculer()
    }
    class PisteDesCristaux {
        -Map cristauxParJoueur
        +cristaux(Joueur j) int
        +ajouter(Joueur j, int n)
        +retirer(Joueur j, int n) int
    }
    class CompteurEnergies {
        -int air
        -int eau
        -int feu
        -int terre
        +get(Energie e) int
        +ajouter(Energie e, int n)
        +retirer(Energie e, int n)
        +total() int
        +contient(CompteurEnergies c) boolean
    }
    class StockEnergie {
        +prendre(Energie e, int n) CompteurEnergies
        +rendre(CompteurEnergies c)
    }
    class Pioche {
        +piocher() Optional~CartePouvoir~
        +regarderDessus() Optional~CartePouvoir~
        +remettreDessus(CartePouvoir c)
        +estVide() boolean
    }
    class Defausse {
        +ajouter(CartePouvoir c)
        +vider() List~CartePouvoir~
    }
    class De {
        -Saison couleur
        -FaceDe faceCourante
        +lancer(Random r)
        +face() FaceDe
        +couleur() Saison
    }
    class FaceDe {
        <<record>>
        int cristaux
        List~Energie~ energies
        int augmentationJauge
        boolean pioche
        boolean cristallisation
        int pointsAvancement
    }

    Plateau "1" *-- "1" RoueDesSaisons
    Plateau "1" *-- "1" EchelleDesAnnees
    Plateau "1" *-- "1" PisteDesCristaux
    Plateau "1" *-- "1" StockEnergie
    Plateau "1" *-- "1" Pioche
    Plateau "1" *-- "1" Defausse
    Plateau "1" o-- "*" De
    RoueDesSaisons ..> ResultatAvancee
    StockEnergie ..> CompteurEnergies
    De "1" *-- "6" FaceDe
    De ..> Saison
    FaceDe ..> Energie
```

Remarques :

- Il y a **20 dés** : 5 par saison. Une partie à N joueurs utilise **N + 1 dés par couleur** (3, 4 ou 5).
- Chaque dé a 6 faces. Une `FaceDe` est décrite par des champs concrets (cristaux reçus, énergies reçues, augmentation de la jauge, pioche, autorisation de cristalliser) et par un nombre de points d'avancement (1, 2 ou 3) qui fait avancer la roue quand le dé n'est pas choisi. Ce format, plus simple qu'une liste d'actions, se lit directement dans `des.json` et permet aux robots d'évaluer une face en une ligne. Sur un dé : 2 faces à 1 point, 2 faces à 2 points, 2 faces à 3 points.
- **Le contenu exact des faces n'est pas décrit dans les règles** : il est chargé depuis `des.json` (voir `conception.md`, points à valider).
- `RoueDesSaisons` : cases 1 à 12 ; hiver = 1-3, printemps = 4-6, été = 7-9, automne = 10-12. Le passage de la case 12 à la case 1 déclenche un changement d'année.
- `PisteDesCristaux.retirer` ne descend jamais sous 0 et retourne le nombre de cristaux réellement retirés (utile pour Figrim, Kairn).

## 3. Joueur et plateau individuel

```mermaid
classDiagram
    class Joueur {
        -String nom
        -int age
        -List~CartePouvoir~ main
        -List~CarteEnJeu~ cartesEnJeu
        +plateauIndividuel() PlateauIndividuel
        +main() List~CartePouvoir~
        +cartesEnJeu() List~CarteEnJeu~
        +energiesDisponibles() CompteurEnergies
        +nbCartesInvoquees() int
        +debloquerCartesAnnee(int annee)
    }
    class PlateauIndividuel {
        +ReserveEnergie reserve
        +JaugeInvocation jauge
        +PisteDesBonus bonus
    }
    class ReserveEnergie {
        -CompteurEnergies energies
        -int capacite
        +capacite() int
        +definirCapacite(int c)
        +ajouter(Energie e, int n)
        +retirer(Energie e, int n)
        +contenu() CompteurEnergies
        +depassement() int
    }
    class JaugeInvocation {
        -int niveau
        +niveau() int
        +augmenter(int n)
        +diminuer(int n)
    }
    class PisteDesBonus {
        -List~TypeBonus~ utilises
        +peutUtiliser() boolean
        +utiliser(TypeBonus b)
        +nbUtilises() int
        +malus() int
    }
    class JetonBibliotheque {
        <<record>>
        int annee
        List~CartePouvoir~ cartes
    }

    Joueur "1" *-- "1" PlateauIndividuel
    Joueur "1" o-- "2" JetonBibliotheque : II et III
    PlateauIndividuel "1" *-- "1" ReserveEnergie
    PlateauIndividuel "1" *-- "1" JaugeInvocation
    PlateauIndividuel "1" *-- "1" PisteDesBonus
    ReserveEnergie ..> CompteurEnergies
    PisteDesBonus ..> TypeBonus
```

Règles locales portées par ces classes :

| Classe | Règle |
|---|---|
| `ReserveEnergie` | Capacité 7 par défaut, 10 avec le Grimoire ensorcelé (carte 18). `depassement()` indique combien d'énergies sont en trop quand une action en fait recevoir trop : le joueur doit garder 7 énergies avant toute autre action. |
| `JaugeInvocation` | Entre 0 et 15 ; un joueur peut avoir autant de cartes en jeu que le niveau de sa jauge |
| `PisteDesBonus` | 3 bonus maximum par partie ; malus de fin de partie : 0 → 0, 1 → 5, 2 → 12, 3 → 20 points |
| `Joueur.energiesDisponibles()` | Réserve + énergies posées sur l'Amulette d'eau (utilisables comme la réserve) |
| `Joueur.nbCartesInvoquees()` | Utilisé pour départager une égalité de points |

Les marqueurs de centaines de la piste des cristaux ne sont pas modélisés : le score est un entier.
