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
