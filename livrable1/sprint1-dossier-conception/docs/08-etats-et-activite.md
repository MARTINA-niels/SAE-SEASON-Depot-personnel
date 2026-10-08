# 08 — Diagrammes d'états et d'activité

## 1. États d'une partie

```mermaid
stateDiagram-v2
    [*] --> MiseEnPlace
    MiseEnPlace --> Tournoi : plateau installé et jeux distribués
    state Tournoi {
        [*] --> DebutManche
        DebutManche --> ChoixDesDes : dés lancés
        ChoixDesDes --> TourDesJoueurs : chaque joueur a un dé
        TourDesJoueurs --> FinDeManche : tous les joueurs ont joué
        FinDeManche --> DebutManche : partie non terminée
        FinDeManche --> [*] : année 3 et case 12 dépassée
    }
    Tournoi --> Decompte
    Decompte --> [*]
```

Correspondance avec `EtatPartie` : `MISE_EN_PLACE`, `TOURNOI`, `DECOMPTE`, `TERMINEE`. Le **prélude** (draft) n'existe pas dans la version débutante : il est remplacé par la distribution des jeux pré-construits.

## 2. États d'une carte pouvoir

```mermaid
stateDiagram-v2
    [*] --> DansLaPioche
    [*] --> SousJetonBibliotheque : paquet 2 ou 3 d'un jeu
    [*] --> EnMain : paquet 1 d'un jeu
    SousJetonBibliotheque --> EnMain : début de l'année 2 ou 3
    DansLaPioche --> EnMain : action piocher et carte gardée
    DansLaPioche --> Defausse : action piocher et carte refusée
    DansLaPioche --> EnJeuRedressee : mise en jeu gratuite (Calice divin)
    EnMain --> EnJeuRedressee : invocation
    EnMain --> Defausse : carte défaussée
    EnJeuRedressee --> EnJeuInclinee : activation
    EnJeuInclinee --> EnJeuRedressee : début de manche
    EnJeuRedressee --> Defausse : sacrifice
    EnJeuInclinee --> Defausse : sacrifice
    EnJeuRedressee --> EnMain : renvoi en main (Amsug Longcoup)
    EnJeuInclinee --> EnMain : renvoi en main (Amsug Longcoup)
    Defausse --> DansLaPioche : pioche vide, la défausse est remélangée
    EnMain --> [*] : fin de partie (-5 points)
    EnJeuRedressee --> [*] : fin de partie (points de prestige)
    EnJeuInclinee --> [*] : fin de partie (points de prestige)
```

## 3. Diagramme d'activité : déroulé d'une manche

```mermaid
flowchart TD
    A(["Début de manche"]) --> B["Le premier joueur lance les N+1 dés de la saison"]
    B --> C["Chaque joueur choisit un dé, dans l'ordre du tour"]
    C --> D{"Reste-t-il un joueur sans dé ?"}
    D -->|oui| C
    D -->|non| E["Le dé restant détermine l'avancée de la saison"]
    E --> F["Tour du joueur suivant"]
    F --> G["Résoudre les actions du dé : pioche, énergies, cristaux, jauge"]
    G --> H{"Réserve au-dessus de la capacité ?"}
    H -->|oui| H1["Le robot choisit les énergies à garder"]
    H1 --> I
    H -->|non| I["Actions légales proposées au robot"]
    I --> J{"Action choisie"}
    J -->|"invoquer"| K["Payer le coût, mettre en jeu, appliquer les effets"]
    J -->|"activer"| L["Incliner, payer le coût d'activation, appliquer l'effet"]
    J -->|"bonus"| M["Appliquer le bonus, avancer la piste des bonus"]
    J -->|"cristalliser"| N["Convertir les énergies en cristaux"]
    K --> I
    L --> I
    M --> I
    N --> I
    J -->|"fin de tour"| O{"Tous les joueurs ont-ils joué ?"}
    O -->|non| F
    O -->|oui| P["Effets de fin de manche : Coffret, Corne"]
    P --> Q["Avancer la roue des saisons"]
    Q --> R{"Changement de saison ?"}
    R -->|oui| R1["Effets : Figrim, Sablier"]
    R -->|non| S
    R1 --> S{"Changement d'année ?"}
    S -->|oui| S1["Ajouter les cartes des jetons bibliothèque"]
    S -->|non| T
    S1 --> T["Redresser les cartes inclinées"]
    T --> U{"Année 3 et case 12 dépassée ?"}
    U -->|oui| V(["Fin de partie et décompte"])
    U -->|non| W["Nouveau premier joueur : voisin de gauche"]
    W --> A
```
