# MSPR TPRE623 – Conception, Exploitation et Protection d’une Base de Données via un SGBD Relationnel

## Présentation du projet

Dans le cadre de la MSPR TPRE623 du cursus Administrateur Systèmes, Réseaux et Bases de Données (ASRBD), notre équipe a été missionnée par l’entreprise fictive **NordTransit Logistics (NTL)** afin de concevoir, sécuriser, superviser et exploiter une nouvelle infrastructure de base de données destinée à son système de gestion d’entrepôt (WMS).

L'objectif principal du projet était de mettre en place une infrastructure moderne répondant aux exigences de :

* Haute disponibilité ;
* Continuité d’activité ;
* Sécurisation des accès ;
* Sauvegarde et restauration ;
* Supervision ;
* Exploitabilité ;
* Documentation technique.

Pour répondre à ces besoins, nous avons choisi **PostgreSQL** comme système de gestion de base de données relationnelle et mis en place une architecture reposant sur :

* Un serveur PostgreSQL Principal (Primary) ;
* Un serveur PostgreSQL Secondaire (Replica) ;
* Un serveur dédié à l’administration PostgreSQL ;
* Un serveur dédié aux sauvegardes externalisées ;
* Une plateforme de supervision centralisée ;
* Une gestion des accès basée sur les rôles (RBAC) ;
* Un Plan de Continuité d’Activité (PCA) ;
* Un Plan de Reprise d’Activité (PRA).

---

# Architecture mise en œuvre

L’infrastructure est composée des éléments suivants :

* WMS-DB-01 : PostgreSQL Primary
* WMS-DB-02 : PostgreSQL Replica
* WMS-PGADMIN : Administration PostgreSQL
* WMS-BACKUP : Sauvegardes externalisées
* WMS-SUPER01 : Supervision Zabbix

Cette architecture repose sur une séparation des rôles afin d'améliorer la sécurité, la disponibilité et la résilience de la plateforme.

## Schéma simplifié

```text
WMS-SUPER01 (Zabbix)
        │
        ├───────────────┐
        │               │
        ▼               ▼

WMS-DB-01 ─────► WMS-DB-02
(Primary)        (Replica)

     │
     │ Sauvegardes
     ▼

WMS-BACKUP

WMS-PGADMIN
(Administration PostgreSQL)
```

---

# Technologies utilisées

## Base de données

* PostgreSQL

## Conteneurisation

* Docker
* Docker Compose

## Administration

* pgAdmin

## Supervision

* Zabbix
* Zabbix Agent

## Sauvegarde

* pg_dump
* Cron

---

# Fonctionnalités réalisées

## Base de données

* Modélisation MCD et MLD
* Création du schéma relationnel
* Intégrité référentielle
* Contraintes métier
* Optimisation des accès
* Gestion avancée des rôles utilisateurs

---

## Haute disponibilité

* Architecture Primary / Replica
* Réplication PostgreSQL
* Validation de la synchronisation
* Procédure de bascule vers le Replica

---

## Sauvegarde et restauration

* Sauvegardes automatisées
* Rotation des sauvegardes
* Journalisation
* Sauvegardes externalisées sur WMS-BACKUP
* Tests de restauration validés

---

## Sécurité

* RBAC (Role Based Access Control)
* Principe du moindre privilège
* Comptes dédiés à l’administration
* Comptes dédiés à la réplication
* Comptes dédiés aux sauvegardes
* Séparation entre production et administration

---

## Supervision

* Dashboard Zabbix
* Disponibilité des serveurs
* CPU
* RAM
* Espace disque
* État de la réplication
* Suivi des sauvegardes
* Alertes et supervision continue

---

# Répartition du travail

## Omar Bachir Diallo

### Chef de projet et Administrateur Base de Données

* Pilotage du projet
* Coordination de l'équipe
* Déploiement PostgreSQL Primary
* Mise en œuvre de la réplication PostgreSQL
* Configuration des sauvegardes
* Mise en place du serveur WMS-BACKUP
* Tests de restauration
* Documentation technique
* Validation de l’infrastructure

---

## Filda

### Conception et modélisation de la base de données

* Analyse du besoin métier
* Réalisation du MCD
* Réalisation du MLD
* Création du schéma relationnel
* Mise en place des contraintes métier
* Validation de l'intégrité des données
* Participation à la documentation

---

## Amine

### Supervision et exploitation

* Déploiement du serveur WMS-SUPER01
* Installation de Zabbix
* Configuration des agents
* Création du dashboard
* Mise en place des alertes
* Surveillance CPU, RAM et stockage
* Supervision des sauvegardes
* Analyse des logs
* Validation des indicateurs de supervision

---

## Madgyd

### Sécurité et continuité d’activité

* Mise en œuvre du RBAC
* Création des rôles PostgreSQL
* Gestion des permissions
* Déploiement du serveur WMS-PGADMIN
* Participation au PCA/PRA
* Analyse des risques
* Rédaction des procédures d'exploitation
* Participation aux livrables de gestion de projet

---

# Livrables réalisés

Les livrables produits dans le cadre du projet sont :

* Architecture technique
* MCD et MLD
* Démarche d’optimisation PostgreSQL
* Guide de supervision
* Guide d’administration et d’exploitation
* RunBook d’exploitation
* Analyse de logs
* PCA / PRA
* Gestion de projet et analyse des risques
* Note de direction
* Documentation technique

---

# Compétences mobilisées

Ce projet nous a permis de mettre en œuvre les compétences suivantes :

* Administration PostgreSQL
* Haute disponibilité
* Réplication de bases de données
* Sauvegarde et restauration
* Supervision d’infrastructure
* Sécurisation des accès
* Analyse de logs
* Gestion de projet
* Documentation technique
* Continuité d’activité

---

# Conclusion

Le projet a permis de concevoir une infrastructure de base de données robuste, sécurisée, supervisée et résiliente répondant aux exigences de NordTransit Logistics.

L'architecture finale repose sur cinq serveurs spécialisés :

* WMS-DB-01 : Production PostgreSQL
* WMS-DB-02 : Réplication et continuité d'activité
* WMS-PGADMIN : Administration PostgreSQL
* WMS-BACKUP : Sauvegardes externalisées
* WMS-SUPER01 : Supervision Zabbix

Les mécanismes de haute disponibilité, de réplication, de sauvegarde externalisée, de supervision et de sécurisation des accès garantissent la continuité du service tout en assurant la protection des données critiques de l’entreprise.

Cette MSPR nous a permis de mettre en pratique les compétences attendues d’un Administrateur Systèmes, Réseaux et Bases de Données dans un contexte professionnel réaliste proche d’un environnement de production.
