# 07 — Diagrammes de séquence

Huit scénarios clés. Les noms des participants correspondent aux classes des diagrammes 03 à 06.

## 1. Mise en place de la partie

```mermaid
sequenceDiagram
    participant Main as MainPartie
    participant P as Partie
    participant MEP as MiseEnPlace
    participant Dist as DistributionPreconstruite
    participant Cat as CatalogueCartes
    participant R as Robot
    participant J as Joueur

    Main->>P: new Partie(robots, catalogue, distribution, graine)
    Main->>P: jouer()
    P->>MEP: installer(partie)
    MEP->>MEP: marqueurs année et saison sur la case 1
    MEP->>MEP: placer N+1 dés par couleur
    MEP->>Cat: creerExemplaires()
    Cat-->>MEP: 60 cartes (30 en 2 exemplaires)
    MEP->>Dist: distribuer(participants, catalogue, alea)
    loop pour chaque joueur
        Dist->>J: lui attribuer un jeu pré-construit de 9 cartes
        Dist->>R: repartirCartes(9 cartes, vue)
        R-->>Dist: RepartitionCartes (3 paquets de 3)
        Dist->>J: main = paquet 1, jetons II et III = paquets 2 et 3
    end
    MEP->>MEP: les cartes restantes forment la pioche
    MEP->>MEP: désigner le premier joueur
    MEP-->>P: mise en place terminée
```

## 2. Une manche complète

```mermaid
sequenceDiagram
    participant P as Partie
    participant M as Manche
    participant Pl as Plateau
    participant R as Robot (joueur courant)
    participant T as Tour
    participant Bus as BusEvenements

    P->>M: new Manche(saison, premierJoueur)
    M->>Pl: desDeLaSaison(saison)
    M->>M: lancer les N+1 dés
    M->>Bus: publier(DEBUT_MANCHE)
    loop chaque joueur dans l'ordre du tour
        M->>R: choisirDe(dés restants, vue)
        R-->>M: dé choisi
        M->>Bus: publier(DE_CHOISI)
    end
    Note over M: un dé reste sur la table, il fera avancer la saison
    loop chaque joueur dans l'ordre du tour
        M->>T: new Tour(participant, deChoisi)
        M->>T: jouer()
        T-->>M: tour terminé
    end
    M->>Bus: publier(FIN_DE_MANCHE)
    M->>Pl: roue.avancer(points du dé restant)
    Pl-->>M: ResultatAvancee
    opt saison changée
        M->>Bus: publier(CHANGEMENT_SAISON)
    end
    opt année changée
        M->>Bus: publier(CHANGEMENT_ANNEE)
    end
    M-->>P: ResultatManche (partie terminée ?)
```

Ordre retenu à la fin de manche : 1) événement `FIN_DE_MANCHE` (Coffret merveilleux, Corne du mendiant), 2) avancée de la roue, 3) changements de saison et d'année (Figrim, Sablier du temps), 4) test de fin de partie. Ce choix est à valider (voir `conception.md`).

## 3. Le tour d'un joueur

```mermaid
sequenceDiagram
    participant T as Tour
    participant Rg as Regles
    participant J as Joueur
    participant R as Robot
    participant Bus as BusEvenements

    T->>T: resoudreActionsDuDe()
    Note over T: pioche, énergies, cristaux, jauge sont résolus en premier
    opt le joueur dépasse la capacité de sa réserve
        T->>R: garderEnReserve(toutes, capacite, vue)
        R-->>T: énergies conservées
    end
    opt la face du dé autorise la pioche
        T->>R: garderCartePiochee(carte, vue)
        R-->>T: garder ou défausser
    end
    T->>Bus: publier(ACTION_EXECUTEE)
    loop tant que le robot ne choisit pas ActionFinDeTour
        T->>Rg: actionsLegales(joueur, contexte)
        Rg-->>T: liste d'ActionJoueur
        T->>R: choisirAction(vue, legales)
        R-->>T: action choisie
        T->>T: executer(action)
        T->>Bus: publier(ACTION_EXECUTEE)
    end
    T-->>T: fin du tour
```

## 4. Invocation d'une carte pouvoir

```mermaid
sequenceDiagram
    participant R as Robot
    participant T as Tour
    participant Rg as Regles
    participant J as Joueur
    participant Ef as Effets de la carte
    participant Bus as BusEvenements
    participant Ep as Effets permanents en jeu

    R-->>T: ActionInvoquer(carte, paiement)
    T->>Rg: coutEffectif(joueur, carte)
    Rg->>Ep: modifierCoutInvocation(cout)
    Ep-->>Rg: coût réduit (Main de la fortune)
    Rg-->>T: coût à payer
    T->>Rg: peutInvoquer(joueur, carte)
    Rg-->>T: oui si jauge suffisante et coût payable
    T->>J: payer le coût (énergies et cristaux)
    T->>J: retirer la carte de la main
    T->>J: ajouter une CarteEnJeu
    T->>Bus: publier(CARTE_INVOQUEE)
    Bus->>Ep: surEvenement(CARTE_INVOQUEE)
    Note right of Ep: Bâton du printemps +3 cristaux, Vase d'Yjang +1 énergie
    T->>Ef: appliquer(contexte, joueur) pour les effets d'arrivée
    Note right of Ef: Amulettes, Calice divin, Syllas, Naria...
```

Une carte mise en jeu gratuitement (Calice divin, Potion de rêves) suit le même parcours **sans** paiement et **sans** l'événement `CARTE_INVOQUEE`.

## 5. Activation d'une carte pouvoir

```mermaid
sequenceDiagram
    participant R as Robot
    participant T as Tour
    participant Rg as Regles
    participant C as CarteEnJeu
    participant J as Joueur
    participant Ea as EffetActivation
    participant Ctx as ContexteEffet (Partie)
    participant Bus as BusEvenements

    R-->>T: ActionActiver(carte)
    T->>Rg: peutActiver(joueur, carte)
    Rg->>C: estInclinee()
    Rg->>Ea: peutActiver(contexte, joueur)
    Rg-->>T: oui si carte redressée et coût payable
    T->>C: incliner()
    T->>J: payer coutActivation() (énergies, sacrifice...)
    T->>Ea: activer(contexte, joueur)
    Ea->>Ctx: recevoirEnergies / recevoirCristaux / piocher ...
    Ctx-->>Ea: effets appliqués
    T->>Bus: publier(CARTE_ACTIVEE)
```

La carte reste inclinée jusqu'au début de la manche suivante (redressement de toutes les cartes).

## 6. Cristallisation d'énergies

```mermaid
sequenceDiagram
    participant R as Robot
    participant T as Tour
    participant Rg as Regles
    participant Tc as TableCristallisation
    participant J as Joueur
    participant Pc as PisteDesCristaux
    participant St as StockEnergie
    participant Bus as BusEvenements
    participant Ep as Effets permanents en jeu

    Note over T: la cristallisation est disponible grâce au dé ou à un bonus
    R-->>T: ActionCristalliser(énergies choisies)
    loop chaque énergie choisie
        T->>Rg: tauxCristallisation(saison, énergie, bonus)
        Rg->>Tc: taux(saison, énergie, bonus)
        Tc-->>Rg: 1, 2 ou 3 cristaux (+1 si bonus de cristallisation)
        Rg-->>T: taux
    end
    T->>J: retirer les énergies de la réserve
    T->>St: rendre(énergies défaussées)
    T->>Pc: ajouter(joueur, total de cristaux)
    T->>Bus: publier(ENERGIES_CRISTALLISEES)
    Bus->>Ep: surEvenement(ENERGIES_CRISTALLISEES)
    Note right of Ep: Bourse d'Io +1 cristal par énergie cristallisée
```

L'exemple des règles (hiver, 1 énergie de feu + 2 énergies de terre) donne 2 + 3 + 3 = **8 cristaux** : il sert de test.

## 7. Fin de manche, changement de saison et d'année

```mermaid
sequenceDiagram
    participant M as Manche
    participant Pl as Plateau
    participant Bus as BusEvenements
    participant Ep as Effets permanents
    participant J as Joueurs
    participant P as Partie

    M->>Bus: publier(FIN_DE_MANCHE)
    Bus->>Ep: surEvenement(FIN_DE_MANCHE)
    Note right of Ep: Coffret merveilleux, Corne du mendiant
    M->>Pl: roue.avancer(points du dé restant)
    Pl-->>M: ResultatAvancee
    alt saison changée
        M->>Bus: publier(CHANGEMENT_SAISON)
        Bus->>Ep: surEvenement(CHANGEMENT_SAISON)
        Note right of Ep: Figrim l'avaricieux, Sablier du temps
    end
    alt année changée
        M->>Pl: annees.avancer()
        M->>J: ajouter les cartes du jeton bibliothèque II ou III à la main
        M->>Bus: publier(CHANGEMENT_ANNEE)
    end
    M->>J: redresser toutes les cartes inclinées
    alt année 3 terminée et case 12 dépassée
        M-->>P: partieTerminee = true
    else
        M->>M: premier joueur = voisin de gauche
        M-->>P: partieTerminee = false
    end
```

## 8. Fin de partie et décompte

```mermaid
sequenceDiagram
    participant P as Partie
    participant D as Decompte
    participant J as Joueur
    participant Ef as EffetFinDePartie
    participant Bus as BusEvenements
    participant Aff as Afficheur

    P->>P: etat = DECOMPTE
    loop chaque joueur
        D->>J: cristaux, cartes en jeu, cartes en main, bonus utilisés
        D->>Ef: bonusCristaux(contexte, joueur)
        Ef-->>D: +20 pour le Heaume de Ragfield si conditions remplies
        D->>D: total = cristaux + bonus + prestige - 5 x main - malus bonus
    end
    D->>D: départager (plus grand nombre de cartes invoquées)
    D-->>P: List des ScoreJoueur et vainqueurs
    P->>Bus: publier(FIN_DE_PARTIE)
    Bus->>Aff: surEvenement(FIN_DE_PARTIE)
    P->>Aff: afficherEtatFinal(resultat, vue)
    P->>P: etat = TERMINEE
```
