# 05 — Diagramme de classes : moteur de jeu

Package `fr.seasons.moteur`. Le moteur applique les règles et arbitre : il propose aux robots **uniquement des actions légales** (donc un robot ne peut pas tricher) et publie des **événements** pour l'affichage et les effets permanents.

## 1. Déroulement : Partie, Manche, Tour

```mermaid
classDiagram
    class Partie {
        -Plateau plateau
        -List~Participant~ participants
        -EtatPartie etat
        -BusEvenements bus
        -Regles regles
        -Random alea
        +Partie(List~Robot~ robots, CatalogueCartes cat, StrategieDistribution distrib, long graine)
        +jouer() ResultatPartie
        +vue() VuePartie
        +bus() BusEvenements
    }
    class EtatPartie {
        <<enumeration>>
        MISE_EN_PLACE
        TOURNOI
        DECOMPTE
        TERMINEE
    }
    class Participant {
        <<record>>
        Joueur joueur
        Robot robot
    }
    class MiseEnPlace {
        +installer(Partie p)
    }
    class StrategieDistribution {
        <<interface>>
        +distribuer(List~Participant~ ps, CatalogueCartes cat, Random r)
    }
    class DistributionPreconstruite {
        +distribuer(List~Participant~ ps, CatalogueCartes cat, Random r)
    }
    class Manche {
        -Saison saison
        -Participant premierJoueur
        -List~De~ des
        +jouer() ResultatManche
    }
    class Tour {
        -Participant participant
        -De deChoisi
        +jouer()
        -resoudreActionsDuDe()
        -boucleActions()
    }
    class ResultatManche {
        <<record>>
        ResultatAvancee avancee
        boolean partieTerminee
    }

    Partie "1" *-- "2..4" Participant
    Partie ..> EtatPartie
    Partie ..> MiseEnPlace
    Partie "1" *-- "*" Manche : crée
    Manche "1" *-- "*" Tour : crée
    Manche ..> ResultatManche
    MiseEnPlace ..> StrategieDistribution
    StrategieDistribution <|.. DistributionPreconstruite
```

`StrategieDistribution` isole la façon de distribuer les cartes : aujourd'hui **jeux pré-construits** (niveau Apprenti) ; un futur **draft** (si le client le demande) se brancherait ici sans modifier le reste.

## 2. Règles, actions, vues

```mermaid
classDiagram
    class Regles {
        +actionsLegales(Joueur j, ContextTour t) List~ActionJoueur~
        +coutEffectif(Joueur j, CartePouvoir c) Cout
        +peutInvoquer(Joueur j, CartePouvoir c) boolean
        +peutActiver(Joueur j, CarteEnJeu c) boolean
        +valeurCristallisation(Saison s, Energie e, boolean bonus) int
        +ordrePremierJoueur(List~Participant~ ps) Participant
        +anneeFinale() int
    }
    class ContextTour {
        <<record>>
        boolean cristallisationDisponible
        int bonusRestants
    }
    class ActionJoueur {
        <<interface>>
        +description() String
    }
    class ActionInvoquer {
        <<record>>
        CartePouvoir carte
        CompteurEnergies paiement
    }
    class ActionActiver {
        <<record>>
        CarteEnJeu carte
    }
    class ActionBonus {
        <<record>>
        TypeBonus bonus
    }
    class ActionCristalliser {
        <<record>>
        CompteurEnergies energies
    }
    class ActionFinDeTour {
        <<record>>
    }
    class VuePartie {
        <<interface>>
        +saison() Saison
        +annee() int
        +positionRoue() int
        +nbJoueurs() int
        +moi() VueJoueur
        +adversaires() List~VueJoueur~
        +desRestants() List~De~
        +taillePioche() int
    }
    class VueJoueur {
        <<interface>>
        +nom() String
        +cristaux() int
        +reserve() CompteurEnergies
        +jauge() int
        +cartesEnJeu() List~CartePouvoir~
        +nbCartesEnMain() int
        +nbBonusUtilises() int
    }

    Regles ..> ActionJoueur
    Regles ..> ContextTour
    ActionJoueur <|.. ActionInvoquer
    ActionJoueur <|.. ActionActiver
    ActionJoueur <|.. ActionBonus
    ActionJoueur <|.. ActionCristalliser
    ActionJoueur <|.. ActionFinDeTour
    VuePartie "1" --> "1" VueJoueur : moi
    VuePartie "1" --> "*" VueJoueur : adversaires
```

- `VuePartie` et `VueJoueur` sont **en lecture seule** : un robot voit tout ce qui est public (cristaux, cartes en jeu, réserves) mais pas la main des adversaires.
- `ActionJoueur` suit le patron **Command** : l'action est un objet décrivant ce que le robot veut faire ; le `Tour` l'exécute après vérification.

## 3. Robot (port), événements, décompte

```mermaid
classDiagram
    class Robot {
        <<interface>>
        +nom() String
        +repartirCartes(List~CartePouvoir~ neuf, VuePartie v) RepartitionCartes
        +choisirDe(List~De~ disponibles, VuePartie v) De
        +garderEnReserve(CompteurEnergies toutes, int capacite, VuePartie v) CompteurEnergies
        +garderCartePiochee(CartePouvoir c, VuePartie v) boolean
        +choisirAction(VuePartie v, List~ActionJoueur~ legales) ActionJoueur
    }
    class RepartitionCartes {
        <<record>>
        List~CartePouvoir~ main
        List~CartePouvoir~ annee2
        List~CartePouvoir~ annee3
    }
    class BusEvenements {
        +abonner(EcouteurEvenement e)
        +publier(EvenementJeu ev)
    }
    class EcouteurEvenement {
        <<interface>>
        +surEvenement(EvenementJeu ev)
    }
    class EvenementJeu {
        <<interface>>
        +type() TypeEvenement
    }
    class TypeEvenement {
        <<enumeration>>
        PARTIE_COMMENCEE
        DEBUT_MANCHE
        DE_CHOISI
        ACTION_EXECUTEE
        CARTE_INVOQUEE
        CARTE_ACTIVEE
        ENERGIES_CRISTALLISEES
        FIN_DE_MANCHE
        CHANGEMENT_SAISON
        CHANGEMENT_ANNEE
        FIN_DE_PARTIE
    }
    class Decompte {
        +calculer(Partie p) List~ScoreJoueur~
        +departager(List~ScoreJoueur~ s) List~Joueur~
    }
    class ScoreJoueur {
        <<record>>
        Joueur joueur
        int cristaux
        int bonusFinDePartie
        int prestigeCartes
        int penaliteMain
        int penaliteBonus
        +total() int
    }
    class ResultatPartie {
        <<record>>
        List~ScoreJoueur~ scores
        List~Joueur~ vainqueurs
        int nbManches
    }

    BusEvenements "1" o-- "*" EcouteurEvenement
    BusEvenements ..> EvenementJeu
    EvenementJeu ..> TypeEvenement
    Decompte ..> ScoreJoueur
    ResultatPartie "1" *-- "*" ScoreJoueur
    Robot ..> RepartitionCartes
```

**Calcul du score** (`ScoreJoueur.total`) :

```text
total = cristaux
      + bonus de fin de partie (cartes à effet de fin de partie, ex. Heaume de Ragfield : +20 cristaux)
      + points de prestige des cartes en jeu
      - 5 x nombre de cartes encore en main
      - malus de la piste des bonus (0, 5, 12 ou 20)
```

Les énergies non utilisées en fin de partie ne rapportent rien. En cas d'égalité, le vainqueur est le joueur qui a invoqué le plus de cartes ; s'il y a encore égalité, la victoire est **partagée** (comptée comme « ex æquo » dans les statistiques).
