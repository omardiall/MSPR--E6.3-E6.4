# Architecture de la solution WMS

## Vue d'ensemble

L'infrastructure mise en place repose sur une architecture à haute disponibilité destinée à assurer la continuité du service, la sécurisation des données, leur supervision ainsi que leur sauvegarde.

L'environnement est composé de cinq serveurs spécialisés :

* WMS-DB-01 : serveur PostgreSQL principal (Primary)
* WMS-DB-02 : serveur PostgreSQL secondaire (Replica)
* WMS-PGADMIN : serveur d'administration PostgreSQL
* WMS-BACKUP : serveur dédié aux sauvegardes externalisées
* WMS-SUPER01 : serveur de supervision Zabbix

Les services sont déployés à l'aide de conteneurs Docker afin de faciliter leur administration, leur maintenance et leur portabilité.

---

# Schéma logique

```text
                           WMS-SUPER01
                      (Serveur Zabbix)
                       192.168.233.132
                               │
                               │ Supervision
                               │
       ┌───────────────────────┼────────────────────────┐
       │                       │                        │
       ▼                       ▼                        ▼

 WMS-DB-01               WMS-DB-02              WMS-PGADMIN
 PostgreSQL              PostgreSQL             Administration
 Primary                 Replica                PostgreSQL
192.168.233.130       192.168.233.131        192.168.233.134
       │
       │ Réplication PostgreSQL
       └────────────────────────────►

       │
       │ Sauvegardes
       ▼

 WMS-BACKUP
 Sauvegardes externalisées
192.168.233.133
```

---

# Description des serveurs

## WMS-DB-01

Le serveur WMS-DB-01 héberge l'instance PostgreSQL principale.

Ses fonctions sont :

* hébergement de la base de données WMS ;
* gestion des opérations de lecture et d'écriture ;
* génération des sauvegardes automatiques ;
* transmission des journaux WAL vers le serveur de réplication ;
* alimentation du serveur de sauvegarde externe.

---

## WMS-DB-02

Le serveur WMS-DB-02 héberge la réplique PostgreSQL.

Ses fonctions sont :

* réception continue des journaux WAL ;
* maintien d'une copie synchronisée des données ;
* reprise de service en cas d'indisponibilité du serveur principal ;
* réduction du risque de perte de données.

---

## WMS-PGADMIN

Le serveur WMS-PGADMIN héberge l'outil d'administration PostgreSQL.

Ses fonctions sont :

* administration des bases de données ;
* gestion des utilisateurs ;
* exécution des requêtes SQL ;
* supervision fonctionnelle des objets PostgreSQL ;
* séparation entre administration et production.

Cette séparation améliore la sécurité et limite l'exposition du serveur de production.

---

## WMS-BACKUP

Le serveur WMS-BACKUP est dédié au stockage des sauvegardes.

Ses fonctions sont :

* réception des sauvegardes générées par WMS-DB-01 ;
* conservation des sauvegardes externalisées ;
* support aux opérations de restauration ;
* protection contre la perte des sauvegardes locales.

Cette architecture garantit la disponibilité des sauvegardes même en cas de défaillance du serveur principal.

---

## WMS-SUPER01

Le serveur WMS-SUPER01 héberge la plateforme de supervision Zabbix.

Ses fonctions sont :

* surveillance des serveurs ;
* remontée des alertes ;
* suivi des performances CPU, mémoire et stockage ;
* surveillance de la réplication PostgreSQL ;
* surveillance des sauvegardes ;
* centralisation des informations d'exploitation.

---

# Architecture Docker

Les services sont déployés sous forme de conteneurs Docker.

### WMS-DB-01

* postgres-primary

### WMS-DB-02

* postgres-replica

### WMS-PGADMIN

* pgadmin

### WMS-SUPER01

* zabbix-server
* zabbix-web
* zabbix-postgres

### WMS-BACKUP

* stockage des sauvegardes

---

# Mécanismes de disponibilité

Afin de garantir la continuité de service, plusieurs mécanismes ont été mis en œuvre :

* réplication PostgreSQL Primary / Replica ;
* sauvegardes automatiques quotidiennes ;
* externalisation des sauvegardes sur WMS-BACKUP ;
* supervision centralisée via Zabbix ;
* procédures de restauration testées et validées ;
* plan de continuité d'activité (PCA) ;
* plan de reprise d'activité (PRA).

---

# Sécurisation

L'accès à la base de données repose sur un contrôle d'accès basé sur les rôles (RBAC).

Les profils définis sont :

* Administrateur ;
* Superviseur logistique ;
* Opérateur ;
* Invité ;
* Compte de réplication ;
* Compte de sauvegarde.

Chaque utilisateur dispose uniquement des droits nécessaires à ses missions conformément au principe du moindre privilège.

L'administration PostgreSQL est isolée sur un serveur dédié (WMS-PGADMIN) afin de renforcer la sécurité de l'infrastructure.

---

# Justification du choix du SGBD

PostgreSQL a été retenu pour les raisons suivantes :

* logiciel libre et open source ;
* excellente fiabilité ;
* support natif de la réplication ;
* performances élevées ;
* gestion avancée des rôles et permissions ;
* compatibilité avec les environnements Docker ;
* large adoption dans les environnements professionnels.

---

# Conclusion

L'architecture mise en place répond aux besoins de NordTransit Logistics en matière de disponibilité, de sécurité, de supervision et de continuité d'activité.

L'utilisation de serveurs spécialisés pour la production, la réplication, l'administration, la sauvegarde et la supervision permet de rapprocher l'infrastructure des standards observés dans les environnements professionnels tout en garantissant la protection des données critiques de l'entreprise.

# WMS Solution Architecture

## Overview

The implemented infrastructure is based on a high-availability architecture designed to ensure service continuity, data security, monitoring, and backup protection.

The environment consists of five dedicated servers:

* WMS-DB-01: Primary PostgreSQL server
* WMS-DB-02: Replica PostgreSQL server
* WMS-PGADMIN: PostgreSQL administration server
* WMS-BACKUP: Dedicated backup storage server
* WMS-SUPER01: Zabbix monitoring server

Services are deployed using Docker containers to simplify administration, maintenance, and portability.

---

# Logical Architecture

```text
                           WMS-SUPER01
                        (Zabbix Server)
                         192.168.233.132
                                │
                                │ Monitoring
                                │
       ┌────────────────────────┼────────────────────────┐
       │                        │                        │
       ▼                        ▼                        ▼

 WMS-DB-01                WMS-DB-02              WMS-PGADMIN
 PostgreSQL               PostgreSQL             Administration
 Primary                  Replica                PostgreSQL
192.168.233.130        192.168.233.131        192.168.233.134
       │
       │ PostgreSQL Replication
       └──────────────────────────────►

       │
       │ Backups
       ▼

 WMS-BACKUP
 External Backup Storage
192.168.233.133
```

---

# Server Description

## WMS-DB-01

The WMS-DB-01 server hosts the Primary PostgreSQL instance.

Its responsibilities include:

* hosting the WMS database;
* handling read and write operations;
* generating automated backups;
* transmitting WAL logs to the Replica server;
* feeding the external backup server.

---

## WMS-DB-02

The WMS-DB-02 server hosts the PostgreSQL Replica instance.

Its responsibilities include:

* continuous reception of WAL logs;
* maintaining a synchronized copy of the database;
* ensuring service continuity in case of Primary server failure;
* reducing the risk of data loss.

---

## WMS-PGADMIN

The WMS-PGADMIN server hosts the PostgreSQL administration platform.

Its responsibilities include:

* database administration;
* user management;
* SQL query execution;
* PostgreSQL object management and monitoring;
* separation of administration and production environments.

This separation improves security and reduces exposure of the production database server.

---

## WMS-BACKUP

The WMS-BACKUP server is dedicated to backup storage.

Its responsibilities include:

* receiving backups generated by WMS-DB-01;
* storing externalized backups;
* supporting restoration operations;
* protecting backup data against local server failures.

This architecture ensures backup availability even in the event of a Primary server failure.

---

## WMS-SUPER01

The WMS-SUPER01 server hosts the Zabbix monitoring platform.

Its responsibilities include:

* server monitoring;
* alert management;
* CPU, memory, and storage monitoring;
* PostgreSQL replication monitoring;
* backup monitoring;
* centralization of operational information.

---

# Docker Architecture

Services are deployed as Docker containers.

### WMS-DB-01

* postgres-primary

### WMS-DB-02

* postgres-replica

### WMS-PGADMIN

* pgadmin

### WMS-SUPER01

* zabbix-server
* zabbix-web
* zabbix-postgres

### WMS-BACKUP

* backup storage service

---

# Availability Mechanisms

To ensure service continuity, several mechanisms have been implemented:

* PostgreSQL Primary / Replica replication;
* daily automated backups;
* backup externalization to WMS-BACKUP;
* centralized monitoring through Zabbix;
* tested and validated restoration procedures;
* Business Continuity Plan (BCP);
* Disaster Recovery Plan (DRP).

---

# Security

Database access is controlled through a Role-Based Access Control (RBAC) model.

The defined profiles are:

* Administrator;
* Logistics Supervisor;
* Operator;
* Guest;
* Replication Account;
* Backup Account.

Each user is granted only the permissions required to perform their duties, in accordance with the Principle of Least Privilege.

PostgreSQL administration is isolated on a dedicated server (WMS-PGADMIN) to further strengthen infrastructure security.

---

# Justification for Choosing PostgreSQL

PostgreSQL was selected for the following reasons:

* free and open-source software;
* excellent reliability;
* native replication support;
* high performance;
* advanced role and permission management;
* compatibility with Docker environments;
* widespread adoption in professional infrastructures.

---

# Conclusion

The implemented architecture meets NordTransit Logistics' requirements regarding availability, security, monitoring, and business continuity.

The use of dedicated servers for production, replication, administration, backup, and monitoring aligns the infrastructure with industry best practices while ensuring the protection and availability of the company's critical data.

