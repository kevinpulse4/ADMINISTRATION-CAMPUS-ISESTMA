# Administration de l'accès internet du campus ISESTMA

> Étude du réseau existant d'un campus universitaire et conception d'une solution d'administration de l'accès internet fondée sur un **annuaire d'identités** (LDAP), une **authentification réseau et Wi-Fi** (RADIUS / 802.1X) et un **contrôle d'accès par groupe** (ACL, pare-feu, VLAN).

Projet de **BTS Réseaux et Sécurité - Niveau 2** réalisé à l'**ISESTMA** (Institut Supérieur d'Études Scientifiques, Technologiques et Managériales), année académique 2026-2027, dans le cadre du cours *Authentification des services d'annuaire et administration des SE réseaux*.

## Sommaire

1. [De quoi parle ce projet ?](#de-quoi-parle-ce-projet-)
2. [Équipe et encadrement](#équipe-et-encadrement)
3. [Organisation du projet en deux phases](#organisation-du-projet-en-deux-phases)
4. [Phase 1 : étude de l'existant](#phase-1--étude-de-lexistant)
5. [Phase 2 : conception de la solution](#phase-2--conception-de-la-solution)
6. [Contenu du dépôt](#contenu-du-dépôt)
7. [Maquette Packet Tracer](#maquette-packet-tracer)
8. [État d'avancement](#état-davancement)

## De quoi parle ce projet ?

Le campus ISESTMA compte deux bâtiments. Sa connexion internet arrive par une antenne de l'opérateur CAMTEL et traverse le campus sans véritable administration : aucune authentification individuelle, aucune segmentation du réseau et aucune journalisation n'ont été relevées. Les étudiants, eux, constatent que la connexion n'arrive pas dans certaines zones, que le signal Wi-Fi est faible et que certaines zones ne sont pas couvertes.

Le projet répond à deux questions :

1. **Où en est le campus ?** Cartographier le réseau, représenter sa topologie, étudier l'existant et recueillir l'avis des utilisateurs.
2. **Comment l'administrer et le sécuriser ?** Identifier les besoins en administration et en sécurité, puis concevoir une infrastructure d'annuaire qui permet de savoir *qui* se connecte, avec *quels droits*, et de garder une trace des connexions.

Un visiteur de ce dépôt y trouve donc à la fois un **diagnostic** (réseau existant, enquête, maquette) et une **solution proposée** (annuaire, authentification, contrôle d'accès).

## Équipe et encadrement

| Rôle | Nom |
|---|---|
| Membres du groupe | FEUDJEU ROSTINE KEVIN, MAKOUGANG GENY ORCHELLE |
| Encadreur | M. Daniel MOUNE |
| Établissement | ISESTMA - BTS Niveau 2 Réseaux et Sécurité |
| Année académique | 2026-2027 |

## Organisation du projet en deux phases

| Phase | Objectif | Livrables |
|---|---|---|
| **Phase 1** | Connaître l'existant et les besoins | Cartographie, topologie, maquette de l'existant, étude de l'existant, enquête auprès de 50 étudiants, analyse des besoins |
| **Phase 2** | Concevoir et sécuriser la solution | Vision produit, architecture d'annuaire, annuaire LDAP opérationnel, pare-feu et ACL, intégration AD / Linux, démonstration finale |

## Phase 1 : étude de l'existant

### Le réseau actuel

- Le campus est constitué du **bâtiment B** (rez-de-chaussée + 2 niveaux) et du **bâtiment A** (rez-de-chaussée + 4 niveaux).
- La connexion internet arrive par une **antenne réseau CAMTEL** reçue au bâtiment B, puis envoyée vers la **baie technique**, qui alimente certains services de ce bâtiment.
- **Aucun câble ne relie les deux bâtiments** : ils communiquent par **liaison radio entre deux NanoStations**.
- Au bâtiment A, la NanoStation envoie la connexion à un **switch central**, qui distribue les services par câble Ethernet. D'autres zones sont couvertes en Wi-Fi par un **routeur Wi-Fi**.

```
Internet > Antenne CAMTEL > Baie technique (bât. B) > NanoStation B
        ~ liaison radio ~ NanoStation A > Switch central (bât. A) > Services câblés + Wi-Fi
```

### Limites identifiées

- Un seul lien opérateur, une seule liaison radio et un seul switch central : plusieurs points de défaillance uniques.
- Aucun pare-feu dédié, aucune segmentation du réseau ni supervision n'a été relevé.
- Wi-Fi assuré par un seul routeur, sans authentification individuelle ni contrôle de la bande passante relevés.

### Résultats de l'enquête (50 étudiants)

Les réponses des 50 étudiants convergent vers trois constats :

1. la connexion internet **n'arrive pas dans certaines zones** du campus, à cause de l'épaisseur des murs ;
2. le **signal Wi-Fi est faible** ;
3. **certaines zones ne sont pas couvertes** en Wi-Fi.

L'interprétation en conclut qu'il s'agit d'un problème structurel de **couverture** plus que de débit, cohérent avec l'insuffisance de points de diffusion relevée dans l'étude de l'existant.

## Phase 2 : conception de la solution

La solution proposée repose sur trois services d'annuaire à administrer :

| Service | Rôle | Technologie |
|---|---|---|
| **Annuaire central des identités** | Comptes et groupes (étudiants, enseignants, administration, invités) : source unique de vérité | LDAP / LDAPS (OpenLDAP) |
| **Authentification réseau et Wi-Fi** | Connexion nominative au Wi-Fi et au réseau, adossée à l'annuaire | RADIUS, 802.1X |
| **Contrôle d'accès par groupe** | Droits différenciés selon la catégorie d'utilisateurs et segmentation du réseau | ACL, pare-feu, VLAN |

**Ce que cela apporte en sécurité** : confidentialité (échanges chiffrés par TLS), intégrité (ACL sur l'annuaire), disponibilité (réplication prévue dès la conception), traçabilité (journalisation des authentifications, un compte nominatif par personne) et moindre privilège.

### Jalons

| Séance | Jalon |
|---|---|
| 1 | Vision produit |
| 4 | Architecture d'annuaire |
| 8 | LDAP opérationnel |
| 12 | Pare-feu et ACL en place |
| 16 | Intégration AD / Linux |
| 20 | Démonstration finale |

## Contenu du dépôt

```
ADMINISTRATION-CAMPUS-ISESTMA/
├── README.md
├── cartographie/
│   └── topologie-campus.png               Cartographie du réseau existant
├── maquette/
│   └── existant.pkt                       Maquette Cisco Packet Tracer de l'existant
├── configuration/
│   └── configuration-finale-campus.txt    Configuration CLI des équipements de la maquette
└── docs/
    ├── etude-de-l-existant.pdf            Étude de l'existant
    ├── resultats-enquete-50-etudiants.pdf Résultats et interprétation de l'enquête
    ├── fiche-vision-produit.pdf           Vision produit de la solution (Phase 2)
    └── analyse-des-besoins.pdf            Analyse des besoins (à venir)
```

## Maquette Packet Tracer

Le fichier `maquette/existant.pkt` reproduit l'existant. Les NanoStations et leur liaison radio sont simulées par des équipements de liaison et un pont.

| Réalité | Maquette |
|---|---|
| Antenne CAMTEL | Cloud-PT |
| Routage et sortie internet | R0 (2911) et R1 (2901) |
| Baie technique | SW-CORE (2960-24TT) + Serveur0 |
| Services du bâtiment B | SW-BAT-B + PC0 à PC3 |
| NanoStation B / NanoStation A | Équipements de liaison + Bridge0 |
| Switch central du bâtiment A | SW-BAT-A + PC4 à PC7 |
| Routeur Wi-Fi | Access Point + Laptop0 et Laptop1 |

**Plan d'adressage de la maquette**

| Réseau | Plage |
|---|---|
| Liaison opérateur (CAMTEL) | 203.0.113.0/30 |
| Liaison R0 - R1 | 10.0.0.0/30 |
| LAN du campus | 192.168.10.0/24 |

### Utiliser la maquette

1. Installer **Cisco Packet Tracer**.
2. Ouvrir `maquette/existant.pkt`.
3. Si besoin, appliquer les configurations de `configuration/configuration-finale-campus.txt` (onglet CLI de chaque équipement).
4. Tester la connectivité entre les deux bâtiments, par exemple un ping entre PC0 (bâtiment B) et PC7 (bâtiment A).

## État d'avancement

**Phase 1**
- [x] Relevé terrain et étude de l'existant
- [x] Topologie de l'existant (Packet Tracer) et configuration finale
- [x] Cartographie du réseau
- [x] Enquête auprès de 50 étudiants
- [ ] Analyse des besoins en administration et en sécurité

**Phase 2**
- [x] Vision produit
- [ ] Architecture d'annuaire
- [ ] Annuaire LDAP opérationnel
- [ ] Pare-feu et ACL
- [ ] Intégration AD / Linux
- [ ] Démonstration finale

# Planning de la Phase 2 : subdivision du projet et dépôts hebdomadaires

Ce planning découpe la **Phase 2** (conception et sécurisation de la solution) en **20 séances**, à raison d'**une séance par semaine** (S1 à S20). Il suit les jalons du cours *Authentification des services d'annuaire et administration des SE réseaux* (ISESTMA, M. Daniel MOUNE).

> La Phase 1 (cartographie, topologie, étude de l'existant, enquête) est déjà déposée sur le dépôt. La Phase 2 s'appuie dessus.

## 1. Les cinq blocs du projet

| Bloc | Séances | Objectif | Jalon |
|---|---|---|---|
| A. Vision et architecture | S1 à S4 | Définir la vision et concevoir l'architecture d'annuaire | **S4** : architecture d'annuaire |
| B. Annuaire LDAP | S5 à S8 | Installer, remplir et sécuriser l'annuaire | **S8** : LDAP opérationnel |
| C. Pare-feu et ACL | S9 à S12 | Segmenter le réseau et filtrer les accès par groupe | **S12** : pare-feu et ACL en place |
| D. Authentification réseau et intégration AD / Linux | S13 à S16 | Brancher le Wi-Fi sur l'annuaire et intégrer AD et Linux | **S16** : intégration AD / Linux |
| E. Finalisation et démonstration | S17 à S20 | Journaliser, tester, documenter et présenter | **S20** : démo finale |

## 2. Plan détaillé : travail et dépôt de chaque semaine

| Semaine | Travail à réaliser | À déposer sur GitHub |
|---|---|---|
| **S1** | Vision produit et activités de la séance : définitions comparées de l'annuaire, scénarios d'authentification, compétences, modèle hybride | `phase2/01-vision-produit/` : fiche de vision produit, tableau comparatif des définitions, fiches d'analyse des scénarios, carte mentale des compétences, proposition de modèle hybride |
| **S2** | Concepts LDAP (objets, DN, hiérarchie, schémas) ; lecture de la RFC 4510 et du guide ANSSI ; analyse des besoins si elle n'est pas encore déposée | `phase2/02-architecture/` : fiche de synthèse LDAP (réponses aux questions guides) ; `docs/analyse-des-besoins.pdf` |
| **S3** | Vision produit affinée ; politique d'identité (catégories d'utilisateurs, cycle de vie des comptes) ; conception de l'arborescence de l'annuaire | Vision produit v2 ; `politique-identite.md` ; schéma de l'arborescence |
| **S4** | **Jalon.** Document d'architecture : annuaire, RADIUS, VLAN, ACL, plan d'adressage cible | `docs/architecture-annuaire.pdf` ; schéma d'architecture (PNG) |
| **S5** | Installation et configuration de base du serveur OpenLDAP (machine virtuelle Linux) | `phase2/03-ldap/` : procédure d'installation, captures d'écran |
| **S6** | Création de l'arborescence, des groupes et des utilisateurs | Fichiers `.ldif`, script d'import |
| **S7** | Sécurisation : chiffrement TLS (LDAPS), ACL de l'annuaire, comptes de service | Configuration (sans mots de passe), tests de chiffrement |
| **S8** | **Jalon.** Tests de recherche et d'authentification (`ldapsearch`, `ldapwhoami`) ; recette | Rapport de tests, export de la configuration |
| **S9** | Politique de sécurité réseau : matrice des flux (qui accède à quoi), VLAN par catégorie d'utilisateurs | `phase2/04-securite/` : matrice des flux, plan de VLAN |
| **S10** | Mise en place du pare-feu et de ses règles | Règles du pare-feu, procédure |
| **S11** | ACL sur routeurs et switchs ; maquette Packet Tracer de la solution (VLAN, ACL) | `maquette/solution-proposee.pkt` (v1), configuration CLI |
| **S12** | **Jalon.** Tests de filtrage par catégorie d'utilisateurs ; recette | Rapport de tests de filtrage, maquette mise à jour |
| **S13** | Installation de FreeRADIUS et liaison à l'annuaire LDAP | `phase2/05-integration/` : procédure et configuration RADIUS |
| **S14** | Authentification du Wi-Fi en 802.1X (WPA2 Enterprise) via RADIUS | Configuration du point d'accès, tests de connexion |
| **S15** | Intégration AD / Linux (authentification des postes Linux sur l'annuaire) | Procédure d'intégration, configuration |
| **S16** | **Jalon.** Tests d'authentification croisée ; recette | Rapport de recette, maquette mise à jour |
| **S17** | Journalisation centralisée ; sauvegarde et restauration ; test de résilience (panne, reprise) | `phase2/06-finalisation/` : configuration des logs, rapport de résilience |
| **S18** | Documentation finale : guide d'administration ; mise à jour du README | `docs/guide-administration.pdf`, README à jour |
| **S19** | Répétition de la démo, corrections, scénario de démonstration | Scénario de démo, captures des tests finaux |
| **S20** | **Démo finale.** Dépôt de la version finale | Version finale complète, release `v1.0` |

*Les séances S17 à S19 ne sont pas détaillées dans le cours : leur contenu ci-dessus est une proposition à valider avec l'encadreur.*

## 3. Structure du dépôt pour la Phase 2

```
ADMINISTRATION-CAMPUS-ISESTMA/
├── README.md
├── cartographie/
├── maquette/
│   ├── existant.pkt
│   └── solution-proposee.pkt
├── configuration/
├── docs/
│   ├── etude-de-l-existant.pdf
│   ├── resultats-enquete-50-etudiants.pdf
│   ├── analyse-des-besoins.pdf
│   ├── architecture-annuaire.pdf
│   └── guide-administration.pdf
├── phase2/
│   ├── 01-vision-produit/
│   ├── 02-architecture/
│   ├── 03-ldap/
│   ├── 04-securite/
│   ├── 05-integration/
│   └── 06-finalisation/
└── suivi/
    └── journal-de-bord.md
```

## 4. Règles de dépôt hebdomadaire

1. **Un dépôt à la fin de chaque séance**, avec un message de commit qui commence par le numéro de séance, par exemple `S05 - Installation de base d'OpenLDAP`.
2. **Un livrable par semaine au minimum**, rangé dans le dossier du bloc concerné.
3. **Une entrée dans `suivi/journal-de-bord.md`** : ce qui a été fait, les difficultés rencontrées et ce qui est prévu pour la semaine suivante.
4. **Mise à jour des cases d'avancement du README** à chaque jalon (S4, S8, S12, S16, S20).
5. **Aucun mot de passe réel** dans le dépôt : utiliser des exemples génériques.
6. Les **jalons** (S4, S8, S12, S16, S20) correspondent aux dates d'évaluation du projet, qui compte pour 40 % de la note du cours.
