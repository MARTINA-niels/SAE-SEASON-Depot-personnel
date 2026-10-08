# 04 — Diagramme de classes : cartes pouvoir et effets

Les cartes se décomposent en **données** (nom, coût, points de prestige, type d'effet : fichier `cartes.json`) et en **comportement** (classes d'effet, une par carte qui a un effet). Ajouter ou modifier une carte ne demande pas de toucher au moteur.

## 1. Cartes et effets

```mermaid
classDiagram
    class CartePouvoir {
        -int numero
        -String nom
        -CategorieCarte categorie
        -int pointsPrestige
        -CoutInvocation cout
        -List~Effet~ effets
        +numero() int
        +estObjetMagique() boolean
        +effetsDeType(TypeEffet t) List~Effet~
    }
    class CategorieCarte {
        <<enumeration>>
        OBJET_MAGIQUE
        FAMILIER
    }
    class CarteEnJeu {
        -CartePouvoir carte
        -boolean inclinee
        -CompteurEnergies energiesPosees
        +incliner()
        +redresser()
        +estInclinee() boolean
        +energiesPosees() CompteurEnergies
    }
    class Cout {
        <<record>>
        CompteurEnergies energies
        int cristaux
    }
    class CoutInvocation {
        -Cout parDefaut
        -Map variantes
        +pour(int nbJoueurs) Cout
    }
    class TypeEffet {
        <<enumeration>>
        ARRIVEE_EN_JEU
        PERMANENT
        ACTIVATION
        FIN_DE_PARTIE
    }
    class Effet {
        <<interface>>
        +type() TypeEffet
    }
    class EffetArrivee {
        <<interface>>
        +appliquer(ContexteEffet ctx, Joueur proprietaire)
    }
    class EffetPermanent {
        <<interface>>
        +evenementsEcoutes() Set~TypeEvenement~
        +surEvenement(EvenementJeu e, ContexteEffet ctx, Joueur proprietaire)
        +modifierCoutInvocation(Cout c) Cout
        +bonusCapaciteReserve() int
    }
    class EffetActivation {
        <<interface>>
        +coutActivation() Cout
        +peutActiver(ContexteEffet ctx, Joueur proprietaire) boolean
        +activer(ContexteEffet ctx, Joueur proprietaire)
    }
    class EffetFinDePartie {
        <<interface>>
        +bonusCristaux(ContexteEffet ctx, Joueur proprietaire) int
    }

    CartePouvoir "1" *-- "1" CoutInvocation
    CartePouvoir "1" *-- "*" Effet
    CartePouvoir ..> CategorieCarte
    CoutInvocation "1" *-- "1..*" Cout
    CarteEnJeu "1" --> "1" CartePouvoir
    Effet ..> TypeEffet
    Effet <|-- EffetArrivee
    Effet <|-- EffetPermanent
    Effet <|-- EffetActivation
    Effet <|-- EffetFinDePartie
```

Précisions :

- `CoutInvocation.pour(nbJoueurs)` : dans le périmètre des 30 cartes, seule **Figrim l'avaricieux (n° 11)** a un coût qui dépend du nombre de joueurs (2, 3 ou 4).
- Les cartes **Bottes temporelles (7)** et **Dé de la malice (15)** n'ont aucun coût d'invocation.
- `EffetPermanent.modifierCoutInvocation` sert à la Main de la fortune (20) ; `bonusCapaciteReserve` au Grimoire ensorcelé (18).
- Une carte peut porter plusieurs effets : le Grimoire ensorcelé a un effet d'arrivée (recevoir 2 énergies) et un effet permanent (réserve de 10).

## 2. Contexte d'exécution des effets et décisions

Un effet agit sur le jeu uniquement à travers `ContexteEffet` ; quand il faut un choix (énergie « de votre choix », adversaire, carte à sacrifier…), il passe par `Decideur`, que le robot implémente.

```mermaid
classDiagram
    class ContexteEffet {
        <<interface>>
        +nbJoueurs() int
        +saisonCourante() Saison
        +adversaires(Joueur j) List~Joueur~
        +recevoirEnergies(Joueur j, Energie e, int n)
        +defausserEnergies(Joueur j, CompteurEnergies c)
        +recevoirCristaux(Joueur j, int n)
        +perdreCristaux(Joueur j, int n) int
        +augmenterJauge(Joueur j, int n)
        +diminuerJauge(Joueur j, int n)
        +piocher(Joueur j) Optional~CartePouvoir~
        +defausserCarte(CartePouvoir c)
        +sacrifier(Joueur j, CarteEnJeu c)
        +renvoyerEnMain(Joueur j, CarteEnJeu c)
        +mettreEnJeuGratuitement(Joueur j, CartePouvoir c) boolean
        +cristalliser(Joueur j, CompteurEnergies c, int cristauxParEnergie)
        +avancerSaison(int cases)
        +reculerSaison(int cases)
        +relancerDe(Joueur j)
        +decideur(Joueur j) Decideur
    }
    class Decideur {
        <<interface>>
        +choisirEnergie(Joueur j, String raison) Energie
        +choisirEnergies(Joueur j, int n, String raison) CompteurEnergies
        +choisirCarte(Joueur j, List~CartePouvoir~ options, String raison) CartePouvoir
        +choisirCarteEnJeu(Joueur j, List~CarteEnJeu~ options, String raison) CarteEnJeu
        +choisirAdversaire(Joueur j, List~Joueur~ options) Joueur
        +choisirOuiNon(Joueur j, String question) boolean
    }
    class Partie {
        <<moteur>>
    }
    class Robot {
        <<interface>>
    }
    ContexteEffet <|.. Partie
    Decideur <|-- Robot
    ContexteEffet ..> Decideur
```

## 3. Catalogue et chargement

```mermaid
classDiagram
    class CatalogueCartes {
        +charger(String ressource) CatalogueCartes
        +carte(int numero) CartePouvoir
        +creerExemplaires() List~CartePouvoir~
        +jeuxPreconstruits() List~JeuPreconstruit~
    }
    class RegistreEffets {
        +effetsPour(int numero) List~Effet~
    }
    class JeuPreconstruit {
        <<record>>
        int numero
        List~Integer~ numerosCartes
    }
    class EffetAmuletteDAir
    class EffetBatonDuPrintemps
    class EffetMainDeLaFortune
    class EffetBalanceDIshtar
    class EffetPotionDeVie
    class EffetHeaumeDeRagfield

    CatalogueCartes ..> RegistreEffets
    CatalogueCartes "1" o-- "30" CartePouvoir
    CatalogueCartes "1" o-- "4" JeuPreconstruit
    RegistreEffets ..> Effet
    EffetAmuletteDAir ..|> EffetArrivee
    EffetBatonDuPrintemps ..|> EffetPermanent
    EffetMainDeLaFortune ..|> EffetPermanent
    EffetBalanceDIshtar ..|> EffetActivation
    EffetPotionDeVie ..|> EffetActivation
    EffetHeaumeDeRagfield ..|> EffetFinDePartie
```

- `creerExemplaires()` crée les **60 cartes** (30 cartes en 2 exemplaires).
- Les classes d'effet (une par carte à effet) sont dans `fr.seasons.cartes.effets`. Le tableau complet est dans `catalogue-cartes.md`.
- Les cartes mises en jeu **gratuitement** (Calice divin, Potion de rêves) passent par `mettreEnJeuGratuitement` : elles ne publient pas l'événement `CARTE_INVOQUEE`, donc ne déclenchent ni le Bâton du printemps ni le Vase oublié d'Yjang.
