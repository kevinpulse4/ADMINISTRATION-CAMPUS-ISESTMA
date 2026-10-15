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
