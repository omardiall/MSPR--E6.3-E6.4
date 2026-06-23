# Architecture de la solution WMS

## Vue d'ensemble

L'infrastructure mise en place repose sur une architecture à haute disponibilité destinée à assurer la continuité du service, la sécurisation des données et leur supervision.

L'environnement est composé de trois serveurs principaux :

* WMS-DB-01 : serveur PostgreSQL principal (Primary)
* WMS-DB-02 : serveur PostgreSQL secondaire (Replica)
* WMS-SUPER01 : serveur de supervision Zabbix

Les services sont déployés à l'aide de conteneurs Docker afin de faciliter leur administration, leur maintenance et leur portabilité.

## Schéma logique

```
                     WMS-SUPER01
                (Serveur de supervision)
                       Zabbix
                 192.168.233.132
                           │
                           │ Supervision
                           │
         ┌─────────────────┴─────────────────┐
         │                                   │
         ▼                                   ▼

  WMS-DB-01                         WMS-DB-02
```

PostgreSQL Primary               PostgreSQL Replica
192.168.233.130                  192.168.233.131

```
         │
         │ Réplication PostgreSQL
         └──────────────────────────────►

         │
         ▼

      pgAdmin
```

Administration des bases

## Description des serveurs

### WMS-DB-01

Le serveur WMS-DB-01 héberge l'instance PostgreSQL principale.

Ses fonctions sont :

* hébergement de la base de données WMS ;
* gestion des opérations de lecture et d'écriture ;
* génération des sauvegardes automatiques ;
* transmission des journaux WAL vers le serveur de réplication.

### WMS-DB-02

Le serveur WMS-DB-02 héberge la réplique PostgreSQL.

Ses fonctions sont :

* réception continue des journaux WAL ;
* maintien d'une copie synchronisée des données ;
* reprise de service en cas d'indisponibilité du serveur principal.

### WMS-SUPER01

Le serveur WMS-SUPER01 héberge la plateforme de supervision Zabbix.

Ses fonctions sont :

* surveillance des serveurs ;
* remontée des alertes ;
* suivi des performances CPU, mémoire et stockage ;
* centralisation des informations d'exploitation.

## Mécanismes de disponibilité

Afin de garantir la continuité de service, plusieurs mécanismes ont été mis en œuvre :

* réplication PostgreSQL Primary/Replica ;
* sauvegardes automatiques quotidiennes ;
* supervision centralisée via Zabbix ;
* procédures de restauration testées et validées.

## Sécurisation

L'accès à la base de données repose sur un contrôle d'accès basé sur les rôles (RBAC).

Les profils définis sont :

* Administrateur ;
* Superviseur logistique ;
* Opérateur ;
* Invité ;
* Compte de réplication.

Chaque utilisateur dispose uniquement des droits nécessaires à ses missions conformément au principe du moindre privilège.
