# Dossier de conception — Sprint 1

**Projet :** Seasons — version électronique en Java (version débutante, 30 cartes)
**Livraison :** vendredi 9 octobre 2026
**Nature du sprint :** théorique — conception UML, aucun code métier

Les diagrammes sont écrits en **Mermaid** : ils s'affichent directement sur GitHub, dans VS Code (extension « Markdown Preview Mermaid Support ») et dans IntelliJ (plugin Mermaid). Si un rendu pose problème, copier le bloc dans https://mermaid.live pour le corriger.

## Sommaire

| Fichier | Contenu |
|---|---|
| [`conception.md`](conception.md) | Périmètre, vocabulaire, règles à modéliser, choix de conception, hypothèses, **questions pour le client** |
| [`01-cas-utilisation.md`](01-cas-utilisation.md) | Diagramme de cas d'utilisation et scénarios principaux |
| [`02-architecture-packages.md`](02-architecture-packages.md) | Diagramme de packages, dépendances, arborescence Maven |
| [`03-classes-modele.md`](03-classes-modele.md) | Classes : plateau, dés, joueurs, énergies, saisons |
| [`04-classes-cartes.md`](04-classes-cartes.md) | Classes : cartes pouvoir, effets, contexte d'effet, catalogue |
| [`05-classes-moteur.md`](05-classes-moteur.md) | Classes : partie, manche, tour, règles, actions, événements, décompte |
| [`06-classes-robots-simulation.md`](06-classes-robots-simulation.md) | Classes : robots, affichage, simulation |
| [`07-sequences.md`](07-sequences.md) | 8 diagrammes de séquence |
| [`08-etats-et-activite.md`](08-etats-et-activite.md) | Diagrammes d'états (partie, carte) et d'activité (manche) |
| [`catalogue-cartes.md`](catalogue-cartes.md) | Les 30 cartes classées par type d'effet, jeux pré-construits, interactions |
| [`matrice-elements-robots.md`](matrice-elements-robots.md) | Élément du jeu → classe → robot qui l'utilise |
| [`plan-de-tests.md`](plan-de-tests.md) | Tests unitaires et d'intégration prévus |
| [`configuration-maven.md`](configuration-maven.md) | `pom.xml` avec les deux exécutions (`partie`, `simulation`) |
| [`backlog.md`](backlog.md) | User stories des sprints 2 à 8 |

## Liste de contrôle de la livraison

- [ ] Diagramme de cas d'utilisation relu par l'équipe
- [ ] Diagramme de packages sans dépendance circulaire
- [ ] Diagrammes de classes : modèle, cartes, moteur, robots, simulation
- [ ] 8 diagrammes de séquence cohérents avec les diagrammes de classes
- [ ] Diagrammes d'états et d'activité
- [ ] Catalogue des 30 cartes complété avec les **coûts** et les **catégories** relevés sur les cartes
- [ ] Questions Q1 à Q7 de `conception.md` envoyées au client
- [ ] Matrice éléments → robots complète
- [ ] Plan de tests et backlog relus
- [ ] Tout est poussé sur GitHub (branche `main`), tag `sprint-1`
- [ ] Planning mis à jour si besoin (v1.1)

## Répartition suggérée (équipe de 5)

| Personne | Travail |
|---|---|
| 1 | `conception.md`, `01-cas-utilisation.md`, `02-architecture-packages.md`, planning |
| 2 | `03-classes-modele.md` et `maven` |
| 3 | `04-classes-cartes.md` et `catalogue-cartes.md` (relevé des coûts sur les cartes) |
| 4 | `05-classes-moteur.md` et `07-sequences.md` |
| 5 | `06-classes-robots-simulation.md`, `08-etats-et-activite.md`, `plan-de-tests.md`, `matrice-elements-robots.md` |

Le `backlog.md` est relu en équipe à la fin du sprint.

## Points qui nécessitent une décision

Voir la section 8 de [`conception.md`](conception.md). Les trois plus importants :

1. **Balance d'Ishtar** : le texte de la carte (4 énergies, 12 cristaux) et le lexique (3 énergies, 9 cristaux) se contredisent.
2. **Faces des dés** : non décrites dans les règles.
3. **Coûts d'invocation et catégories des cartes** : à relever sur les cartes.
