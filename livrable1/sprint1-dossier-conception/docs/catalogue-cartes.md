# Catalogue des 30 cartes du périmètre

Source : lexique des règles (p. 9 à 12) et résumés sur les cartes. Les **points de prestige** sont ceux lus dans le texte du PDF ; ils sont à recouper avec les cartes. Les **coûts d'invocation** et la **catégorie** (fond violet = objet magique, fond orange = familier) sont à relever sur les cartes pour remplir `cartes.json` (voir Q3 dans `conception.md`).

## 1. Catalogue

| N° | Carte | Prestige | Type d'effet | Résumé de l'effet |
|---|---|---|---|---|
| 1 | Amulette d'air | 6 | Arrivée en jeu | Jauge d'invocation +2 |
| 2 | Amulette de feu | 6 | Arrivée en jeu | Piocher 4 cartes, en garder 1 en main, défausser les 3 autres |
| 3 | Amulette de terre | 6 | Arrivée en jeu | +9 cristaux |
| 4 | Amulette d'eau | 6 | Arrivée en jeu | 4 énergies de son choix posées sur la carte, utilisables comme la réserve (mais hors effets liés à la réserve) |
| 5 | Balance d'Ishtar | 4 | Activation | Défausser des énergies identiques et les cristalliser (3 → 9 cristaux d'après le lexique ; voir Q1) ; affectée par Bourse d'Io et bonus de cristallisation |
| 6 | Bâton du printemps | 9 | Permanent | +3 cristaux à chaque invocation depuis la main |
| 7 | Bottes temporelles | 8 | Arrivée en jeu | Coût d'invocation nul ; avancer ou reculer la saison de 1 à 3 cases (un recul de l'hiver à l'automne recule aussi l'année) |
| 8 | Bourse d'Io | 6 | Permanent | +1 cristal par énergie cristallisée (affecte Balance d'Ishtar et Potion de vie) |
| 9 | Calice divin | 10 | Arrivée en jeu | Piocher 4 cartes, en invoquer 1 gratuitement (jauge requise), défausser les autres |
| 10 | Syllas le fidèle | 14 | Arrivée en jeu | Chaque adversaire sacrifie une carte en jeu |
| 11 | Figrim l'avaricieux | 7 | Permanent | À chaque changement de saison, chaque adversaire donne 1 cristal (coût d'invocation variable selon 2, 3 ou 4 joueurs) |
| 12 | Naria la prophétesse | 8 | Arrivée en jeu | Piocher autant de cartes que de joueurs, en garder 1, donner 1 carte à chaque adversaire |
| 13 | Coffret merveilleux | 4 | Permanent | Fin de manche : avec 4 énergies ou plus en réserve, +3 cristaux |
| 14 | Corne du mendiant | 8 | Permanent | Fin de manche : avec 1 énergie ou moins en réserve, +1 énergie au choix |
| 15 | Dé de la malice | 8 | Activation | Coût d'invocation nul ; relancer son dé avant d'effectuer ses actions, +2 cristaux (avec 2 Dés : 4 cristaux pour les deux relances) |
| 16 | Kairn le destructeur | 9 | Activation | Défausser 1 énergie : chaque adversaire recule de 4 cristaux |
| 17 | Amsug Longcoup | 8 | Arrivée en jeu | Chaque joueur renvoie en main l'un de ses objets magiques en jeu |
| 18 | Grimoire ensorcelé | 8 | Arrivée en jeu + permanent | +2 énergies ; capacité de réserve portée à 10 |
| 19 | Heaume de Ragfield | 10 | Fin de partie | Si strictement le plus de cartes en jeu : +20 cristaux |
| 20 | Main de la fortune | 9 | Permanent | Coût d'invocation des prochaines cartes réduit de 1 énergie (minimum 1 énergie) |
| 21 | Lewis Grisemine | 6 | Arrivée en jeu | Recevoir les mêmes énergies que la réserve d'un adversaire au choix |
| 22 | Cube runique d'Eolis | 30 | Aucun effet | Rapporte uniquement ses 30 points de prestige |
| 23 | Potion de puissance | 0 | Activation | Sacrifice : piocher 1 carte (à garder obligatoirement) et jauge +2 |
| 24 | Potion de rêves | 0 | Activation | Sacrifice + défausser toutes ses énergies : mettre en jeu gratuitement une carte de la main |
| 25 | Potion de savoir | 0 | Activation | Sacrifice : recevoir 5 énergies au choix |
| 26 | Potion de vie | 0 | Activation | Sacrifice : cristalliser chaque énergie de la réserve en 4 cristaux (affectée par Bourse d'Io) |
| 27 | Sablier du temps | 6 | Permanent | À chaque changement de saison, +1 énergie au choix |
| 28 | Sceptre de grandeur | 8 | Arrivée en jeu | +3 cristaux par autre objet magique en jeu |
| 29 | Statue bénie d'Olaf | 0 | Arrivée en jeu | +20 cristaux |
| 30 | Vase oublié d'Yjang | 6 | Permanent | +1 énergie à chaque invocation depuis la main |

Répartition : 12 cartes à effet d'arrivée seul, 8 permanentes seules, 7 à activation, 1 mixte (18), 1 de fin de partie (19), 1 sans effet (22). Total : 30.

## 2. Jeux pré-construits (niveau Apprenti)

| Jeu | Cartes |
|---|---|
| n° 1 | 1, 2, 7, 17, 18, 20, 26, 29, 30 |
| n° 2 | 3, 5, 9, 14, 15, 21, 23, 25, 28 |
| n° 3 | 4, 6, 7, 9, 12, 16, 22, 24, 30 |
| n° 4 | 1, 2, 3, 11, 13, 15, 18, 25, 27 |

- Les cartes **8, 10 et 19** n'apparaissent dans aucun jeu : elles ne viennent que de la pioche.
- Une carte présente dans deux jeux (par exemple la 7 ou la 30) existe en 2 exemplaires : jusqu'à 4 joueurs, il n'y a pas de manque.
- Le reste des 60 cartes forme la pioche.

## 3. Cas d'interaction à traiter (lexique)

| Interaction | Règle |
|---|---|
| Bâton du printemps (6) et Vase d'Yjang (30) | Ne s'appliquent qu'aux cartes invoquées depuis la main, pas à celles mises en jeu gratuitement (Calice divin, Potion de rêves) |
| Bourse d'Io (8) | S'applique à la Balance d'Ishtar et à la Potion de vie, pas aux autres gains de cristaux |
| Main de la fortune (20) | Ne réduit pas les coûts d'activation ; coût minimal de 1 énergie |
| Grimoire ensorcelé (18) | Capacité 10 au maximum, même avec deux Grimoires ; ses énergies comptent comme la réserve |
| Amulette d'eau (4) | Ses énergies sont utilisables comme la réserve mais n'activent ni Coffret merveilleux (13) ni Corne du mendiant (14) ; la Potion de vie (26) et la Potion de rêves (24) ne les touchent pas |
| Bottes temporelles (7) | Un changement de saison provoqué par la carte déclenche immédiatement Figrim (11) et Sablier du temps (27) |
| Calice divin (9), Potion de rêves (24) | La jauge d'invocation doit être suffisante, sinon la carte piochée est défaussée (Calice) |
| Potion de puissance (23) | La carte piochée est obligatoirement gardée en main |
| Potion de rêves (24) | Utilisable même sans énergie |
| Amsug Longcoup (17) | Sans objet magique en jeu, un joueur n'est pas concerné |
| Figrim (11) | Un adversaire sans cristal n'est pas concerné |
| Lewis Grisemine (21) | Copie la réserve (Grimoire compris), pas l'Amulette d'eau ; l'adversaire conserve ses énergies |
| Dé de la malice (15) | Seul le nouveau dé compte ; deux Dés permettent deux relances à la suite (+4 cristaux) |
| Heaume de Ragfield (19) | Le joueur doit avoir strictement plus de cartes en jeu que chaque adversaire |
