# MSPR TPRE623 – Conception, Exploitation et Protection d’une Base de Données via un SGBD Relationnel

## Présentation du projet

Dans le cadre de la MSPR TPRE623 du cursus Administrateur Systèmes, Réseaux et Bases de Données (ASRBD), notre équipe a été missionnée par l’entreprise fictive **NordTransit Logistics (NTL)** afin de concevoir, sécuriser, superviser et exploiter une nouvelle infrastructure de base de données destinée à son système de gestion d’entrepôt (WMS).

L'objectif principal du projet était de remplacer l'infrastructure existante par une solution moderne répondant aux exigences de :

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
* Une réplication PostgreSQL en temps réel ;
* Des sauvegardes automatisées ;
* Une supervision centralisée via Zabbix ;
* Une gestion des accès basée sur les rôles (RBAC) ;
* Un Plan de Continuité d’Activité (PCA) ;
* Un Plan de Reprise d’Activité (PRA).

---

Architecture mise en œuvre

L’infrastructure est composée des éléments suivants :

WMS-DB-01 : PostgreSQL Primary
WMS-DB-02 : PostgreSQL Replica
WMS-PGADMIN : Serveur dédié à l’administration PostgreSQL
WMS-BACKUP : Serveur dédié au stockage des sauvegardes
WMS-SUPER01 : Serveur de supervision Zabbix

Cette architecture repose sur une séparation des rôles afin d'améliorer la sécurité, la disponibilité et la résilience de la plateforme.

Les principaux mécanismes mis en place sont :

Réplication PostgreSQL ;
Sauvegardes automatiques ;
Sauvegardes externalisées ;
Tests de restauration ;
Supervision CPU, RAM, Disque et disponibilité ;
Gestion des droits par rôles ;
Analyse des journaux système ;
Administration PostgreSQL dédiée.
Fonctionnalités réalisées
Base de données
Modélisation MCD et MLD
Création du schéma relationnel
Intégrité référentielle
Contraintes métier
Optimisation des accès
Gestion avancée des rôles utilisateurs
Haute disponibilité
Architecture Primary / Replica
Réplication PostgreSQL
Validation de la synchronisation
Procédure de bascule vers le Replica
Sauvegarde et restauration
Sauvegardes automatisées
Rotation des sauvegardes
Journalisation
Stockage externalisé sur WMS-BACKUP
Tests de restauration validés
Sécurité
RBAC (Role Based Access Control)
Principe du moindre privilège
Comptes dédiés à l’administration, la supervision, la réplication et la sauvegarde
Séparation entre production et administration
Supervision
Dashboard Zabbix
Disponibilité des serveurs
CPU
RAM
Espace disque
État de la réplication
Suivi des sauvegardes
Alertes et supervision continue

---

# Répartition du travail

## Omar Bachir Diallo

* Chef de projet
* Déploiement PostgreSQL Primary
* Mise en place de la réplication PostgreSQL
* Configuration des sauvegardes
* Tests de restauration
* Documentation technique
* Validation de l’infrastructure

---

## Filda

* Modélisation de la base de données
* Élaboration du MCD
* Élaboration du MLD
* Définition des contraintes métier
* Vérification de l’intégrité des données
* Participation à la documentation

---

## Amine

* Mise en œuvre de la supervision Zabbix
* Configuration des agents
* Création du dashboard
* Définition des indicateurs de supervision
* Analyse des journaux
* Participation aux tests de validation

---

## Madgyd

* Sécurisation de l’infrastructure
* Mise en œuvre du RBAC
* Gestion des rôles et permissions
* Contribution au PCA/PRA
* Analyse des risques
* Participation aux livrables de gestion de projet

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

WMS-DB-01 : Production PostgreSQL
WMS-DB-02 : Réplication et continuité d'activité
WMS-PGADMIN : Administration PostgreSQL
WMS-BACKUP : Sauvegardes externalisées
WMS-SUPER01 : Supervision Zabbix

Les mécanismes de haute disponibilité, de réplication, de sauvegarde externalisée, de supervision et de sécurisation des accès garantissent la continuité du service tout en assurant la protection des données critiques de l’entreprise.

Cette MSPR nous a permis de mettre en pratique les compétences attendues d’un Administrateur Systèmes, Réseaux et Bases de Données dans un contexte professionnel réaliste proche d’un environnement de production.