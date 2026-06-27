# Gestion du projet et analyse des risques

## 1. Présentation du projet

Le projet MSPR TPRE623 a consisté à concevoir, déployer, sécuriser, superviser et documenter une infrastructure de base de données destinée au système de gestion d'entrepôt (WMS) de l'entreprise fictive NordTransit Logistics (NTL).

L'objectif principal était de mettre en place une infrastructure fiable permettant :

* la gestion des données logistiques ;
* la haute disponibilité du service ;
* la protection des données ;
* la supervision de l'infrastructure ;
* la continuité d'activité ;
* la sécurisation des accès.

L'architecture finale repose sur cinq serveurs spécialisés :

* WMS-DB-01 : PostgreSQL Primary ;
* WMS-DB-02 : PostgreSQL Replica ;
* WMS-PGADMIN : administration PostgreSQL ;
* WMS-BACKUP : stockage des sauvegardes ;
* WMS-SUPER01 : supervision Zabbix.

---

# 2. Organisation du projet

Le projet a été réalisé selon une approche collaborative reposant sur une répartition claire des responsabilités entre les membres de l'équipe.

Les principales activités réalisées ont été :

* conception de l'architecture ;
* modélisation de la base de données ;
* déploiement PostgreSQL ;
* mise en place de la réplication ;
* automatisation des sauvegardes ;
* mise en œuvre du serveur de sauvegarde ;
* déploiement de pgAdmin ;
* mise en place de la supervision ;
* sécurisation des accès ;
* rédaction de la documentation ;
* validation des procédures de restauration.

---

# 3. Répartition des responsabilités

## Omar Bachir Diallo

### Chef de projet et Administrateur Base de Données

Responsabilités :

* pilotage du projet ;
* coordination de l'équipe ;
* déploiement PostgreSQL ;
* mise en œuvre de la réplication ;
* configuration des sauvegardes ;
* mise en place du serveur WMS-BACKUP ;
* validation des restaurations ;
* documentation technique ;
* validation finale de l'infrastructure.

---

## Filda

### Conception et modélisation

Responsabilités :

* analyse des besoins ;
* conception du modèle de données ;
* réalisation du MCD ;
* réalisation du MLD ;
* définition des contraintes métier ;
* validation de la cohérence des données ;
* participation à la documentation.

---

## Amine

### Supervision et exploitation

Responsabilités :

* installation de Zabbix ;
* configuration des agents ;
* création du dashboard ;
* définition des indicateurs de supervision ;
* surveillance CPU, RAM et stockage ;
* supervision des sauvegardes ;
* analyse des journaux ;
* validation des indicateurs.

---

## Madgyd

### Sécurité et continuité d'activité

Responsabilités :

* mise en œuvre du RBAC ;
* création des rôles PostgreSQL ;
* gestion des permissions ;
* déploiement du serveur WMS-PGADMIN ;
* participation au PCA/PRA ;
* analyse des risques ;
* procédures d'exploitation ;
* participation aux livrables de gestion de projet.

---

# 4. Planning des principales étapes

| Étape | Description                       |
| ----- | --------------------------------- |
| 1     | Analyse du cahier des charges     |
| 2     | Conception du modèle de données   |
| 3     | Création du MCD et du MLD         |
| 4     | Déploiement PostgreSQL            |
| 5     | Mise en place de la réplication   |
| 6     | Configuration des sauvegardes     |
| 7     | Déploiement du serveur WMS-BACKUP |
| 8     | Déploiement de pgAdmin            |
| 9     | Mise en place de Zabbix           |
| 10    | Sécurisation RBAC                 |
| 11    | Tests de restauration             |
| 12    | Validation des livrables          |
| 13    | Préparation de la soutenance      |

---

# 5. Analyse des risques

L'identification des risques constitue une étape essentielle afin d'assurer la disponibilité et la continuité d'activité du système.

| Risque                              | Probabilité | Impact     | Mesure mise en œuvre                  |
| ----------------------------------- | ----------- | ---------- | ------------------------------------- |
| Panne du serveur principal          | Moyenne     | Élevé      | Réplication PostgreSQL                |
| Perte de données                    | Faible      | Très élevé | Sauvegardes automatiques + WMS-BACKUP |
| Erreur humaine                      | Moyenne     | Moyen      | RBAC et contrôles d'accès             |
| Saturation du disque                | Moyenne     | Élevé      | Supervision Zabbix                    |
| Défaillance logicielle              | Faible      | Élevé      | Procédures de restauration            |
| Mauvaise manipulation               | Moyenne     | Moyen      | Documentation d'exploitation          |
| Compromission de pgAdmin            | Faible      | Élevé      | Serveur dédié WMS-PGADMIN             |
| Défaillance des sauvegardes locales | Faible      | Très élevé | Sauvegardes externalisées             |

---

# 6. Difficultés rencontrées

Plusieurs difficultés techniques ont été rencontrées durant le projet :

* configuration de la réplication PostgreSQL ;
* paramétrage des accès réseau ;
* configuration des conteneurs Docker ;
* mise en place des sauvegardes ;
* intégration de Zabbix ;
* gestion des permissions PostgreSQL ;
* configuration du RBAC ;
* validation des procédures de restauration.

Ces difficultés ont été résolues grâce à des phases successives de tests, d'analyse et de validation.

---

# 7. Solutions apportées

Afin de répondre aux contraintes du cahier des charges, plusieurs solutions ont été retenues :

* PostgreSQL pour la gestion des données ;
* Docker pour le déploiement des services ;
* Réplication PostgreSQL pour la haute disponibilité ;
* pgAdmin pour l'administration ;
* WMS-BACKUP pour les sauvegardes externalisées ;
* Zabbix pour la supervision ;
* RBAC pour la gestion des accès ;
* PCA/PRA pour la continuité d'activité.

---

# 8. Résultats obtenus

Le projet a permis de mettre en place une infrastructure complète intégrant :

* haute disponibilité ;
* supervision centralisée ;
* sauvegardes externalisées ;
* restauration des données ;
* sécurisation des accès ;
* administration dédiée ;
* continuité d'activité ;
* documentation d'exploitation.

Les objectifs fixés dans le cahier des charges ont été atteints.

---

# 9. Enseignements tirés du projet

Cette MSPR a permis de développer des compétences dans les domaines suivants :

* administration PostgreSQL ;
* réplication de bases de données ;
* sauvegarde et restauration ;
* supervision d'infrastructure ;
* sécurisation des accès ;
* gestion des risques ;
* gestion de projet ;
* documentation technique.

Elle a également permis de mettre en pratique les méthodes de travail collaboratif et les bonnes pratiques utilisées dans les environnements professionnels.

---

# 10. Conclusion

La gestion du projet a permis d'assurer la bonne coordination des différentes tâches nécessaires à la réalisation de l'infrastructure WMS.

Grâce à une répartition claire des responsabilités, à une analyse rigoureuse des risques et à la mise en œuvre de solutions adaptées, l'équipe a pu livrer une infrastructure robuste, sécurisée et conforme aux exigences du cahier des charges.

Le projet constitue une mise en situation professionnelle complète couvrant les principaux domaines de compétence d'un Administrateur Systèmes, Réseaux et Bases de Données.

# Project Management and Risk Analysis

## 1. Project Overview

The MSPR TPRE623 project consisted of designing, deploying, securing, monitoring, and documenting a database infrastructure intended for the Warehouse Management System (WMS) of the fictional company NordTransit Logistics (NTL).

The main objective was to implement a reliable infrastructure capable of providing:

* logistics data management;
* high service availability;
* data protection;
* infrastructure monitoring;
* business continuity;
* secure access control.

The final architecture is based on five specialized servers:

* WMS-DB-01: PostgreSQL Primary;
* WMS-DB-02: PostgreSQL Replica;
* WMS-PGADMIN: PostgreSQL administration server;
* WMS-BACKUP: Backup storage server;
* WMS-SUPER01: Zabbix monitoring server.

---

# 2. Project Organization

The project was carried out using a collaborative approach based on a clear distribution of responsibilities among team members.

The main activities performed included:

* architecture design;
* database modeling;
* PostgreSQL deployment;
* replication implementation;
* backup automation;
* backup server deployment;
* pgAdmin deployment;
* monitoring implementation;
* access security configuration;
* technical documentation;
* validation of restoration procedures.

---

# 3. Roles and Responsibilities

## Omar Bachir Diallo

### Project Manager and Database Administrator

Responsibilities:

* project leadership;
* team coordination;
* PostgreSQL deployment;
* replication implementation;
* backup configuration;
* WMS-BACKUP server deployment;
* restoration validation;
* technical documentation;
* final infrastructure validation.

---

## Filda

### Database Design and Modeling

Responsibilities:

* requirements analysis;
* database model design;
* creation of the Conceptual Data Model (CDM);
* creation of the Logical Data Model (LDM);
* business rule definition;
* data consistency validation;
* contribution to technical documentation.

---

## Amine

### Monitoring and Operations

Responsibilities:

* Zabbix installation;
* agent configuration;
* dashboard creation;
* monitoring KPI definition;
* CPU, RAM, and storage monitoring;
* backup monitoring;
* log analysis;
* monitoring validation.

---

## Madgyd

### Security and Business Continuity

Responsibilities:

* RBAC implementation;
* PostgreSQL role creation;
* permission management;
* deployment of the WMS-PGADMIN server;
* contribution to the BCP/DRP strategy;
* risk analysis;
* operational procedures;
* participation in project management deliverables.

---

# 4. Major Project Milestones

| Step | Description                  |
| ---- | ---------------------------- |
| 1    | Requirements analysis        |
| 2    | Database design              |
| 3    | CDM and LDM creation         |
| 4    | PostgreSQL deployment        |
| 5    | Replication implementation   |
| 6    | Backup configuration         |
| 7    | WMS-BACKUP deployment        |
| 8    | pgAdmin deployment           |
| 9    | Zabbix implementation        |
| 10   | RBAC security implementation |
| 11   | Restoration testing          |
| 12   | Deliverable validation       |
| 13   | Presentation preparation     |

---

# 5. Risk Analysis

Risk identification is a critical step in ensuring system availability and business continuity.

| Risk                   | Probability | Impact    | Mitigation Measure             |
| ---------------------- | ----------- | --------- | ------------------------------ |
| Primary server failure | Medium      | High      | PostgreSQL replication         |
| Data loss              | Low         | Very High | Automated backups + WMS-BACKUP |
| Human error            | Medium      | Medium    | RBAC and access controls       |
| Disk saturation        | Medium      | High      | Zabbix monitoring              |
| Software failure       | Low         | High      | Restoration procedures         |
| Misconfiguration       | Medium      | Medium    | Operational documentation      |
| pgAdmin compromise     | Low         | High      | Dedicated WMS-PGADMIN server   |
| Local backup failure   | Low         | Very High | Externalized backups           |

---

# 6. Challenges Encountered

Several technical challenges were encountered during the project:

* PostgreSQL replication configuration;
* network access configuration;
* Docker container deployment;
* backup implementation;
* Zabbix integration;
* PostgreSQL permission management;
* RBAC configuration;
* restoration procedure validation.

These challenges were successfully resolved through multiple phases of testing, analysis, and validation.

---

# 7. Implemented Solutions

To meet the project requirements, the following solutions were selected:

* PostgreSQL for database management;
* Docker for service deployment;
* PostgreSQL Replication for high availability;
* pgAdmin for administration;
* WMS-BACKUP for externalized backup storage;
* Zabbix for monitoring;
* RBAC for access control management;
* Business Continuity Plan (BCP) and Disaster Recovery Plan (DRP).

---

# 8. Results Achieved

The project resulted in the implementation of a complete infrastructure including:

* high availability;
* centralized monitoring;
* externalized backups;
* data restoration procedures;
* secure access control;
* dedicated administration;
* business continuity mechanisms;
* operational documentation.

All objectives defined in the project requirements were successfully achieved.

---

# 9. Lessons Learned

This MSPR project enabled the development of skills in the following areas:

* PostgreSQL administration;
* database replication;
* backup and recovery;
* infrastructure monitoring;
* access security;
* risk management;
* project management;
* technical documentation.

The project also provided valuable experience in collaborative teamwork and professional best practices commonly used in enterprise environments.

---

# 10. Conclusion

Effective project management ensured the successful coordination of all activities required to build the WMS infrastructure.

Thanks to a clear distribution of responsibilities, thorough risk analysis, and the implementation of appropriate technical solutions, the team successfully delivered a robust, secure, and reliable infrastructure that fully complies with the project requirements.

This project represents a comprehensive professional experience covering the key competencies expected from a Systems, Networks, and Database Administrator in a real-world enterprise environment.
