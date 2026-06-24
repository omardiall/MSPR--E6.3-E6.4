# Plan de Continuité d'Activité (PCA) et Plan de Reprise d'Activité (PRA)

## 1. Objectif

L'objectif du PCA/PRA est de garantir la continuité de service de l'application WMS en cas d'incident affectant l'infrastructure informatique.

La stratégie retenue repose sur :

* une réplication PostgreSQL Primary / Replica ;
* des sauvegardes automatiques quotidiennes ;
* un serveur dédié au stockage des sauvegardes ;
* une supervision centralisée avec Zabbix ;
* des procédures de restauration validées ;
* une séparation des rôles entre les différents composants de l'infrastructure.

---

# 4. Plan de Continuité d'Activité (PCA)

## Mesures préventives

Afin de maintenir la continuité du service :

* réplication PostgreSQL entre WMS-DB-01 et WMS-DB-02 ;
* supervision permanente via Zabbix ;
* sauvegardes automatiques quotidiennes ;
* stockage externalisé des sauvegardes sur WMS-BACKUP ;
* contrôle des accès basé sur les rôles (RBAC) ;
* administration PostgreSQL isolée sur WMS-PGADMIN ;
* surveillance des ressources système.

---

## Surveillance continue

Les éléments surveillés sont :

* disponibilité des serveurs ;
* charge CPU ;
* utilisation mémoire ;
* espace disque ;
* état de la réplication ;
* disponibilité des services PostgreSQL ;
* disponibilité de WMS-PGADMIN ;
* disponibilité de WMS-BACKUP ;
* présence des sauvegardes externalisées.

Les alertes sont centralisées dans Zabbix.

---

## Scénario 2 : Suppression accidentelle de données

### Détection

Constat de disparition de données métier.

### Procédure

1. Identifier les données supprimées.
2. Récupérer la dernière sauvegarde disponible sur WMS-BACKUP.
3. Restaurer la sauvegarde.
4. Contrôler l'intégrité des données restaurées.
5. Vérifier la cohérence applicative.

### Validation

Le test de restauration a été réalisé avec succès durant le projet.

La présence du serveur WMS-BACKUP permet de disposer d'une copie indépendante des sauvegardes en cas de défaillance du serveur principal.

---

## Scénario 4 : Perte du serveur principal et de son stockage local

### Détection

Le serveur WMS-DB-01 devient indisponible et les sauvegardes locales ne sont plus accessibles.

### Procédure

1. Basculer les applications vers WMS-DB-02.
2. Vérifier l'intégrité des données répliquées.
3. Récupérer les sauvegardes stockées sur WMS-BACKUP.
4. Préparer un nouveau serveur PostgreSQL.
5. Restaurer les sauvegardes si nécessaire.
6. Reconfigurer la réplication.

### Résultat attendu

La continuité de service est assurée grâce au serveur Replica et les données restent disponibles grâce aux sauvegardes externalisées.

---

# 6. Moyens techniques mobilisés

## Sauvegardes

* pg_dump ;
* planification Cron ;
* stockage local temporaire ;
* transfert automatique vers WMS-BACKUP ;
* conservation externalisée des sauvegardes.

## Réplication

* PostgreSQL Streaming Replication ;
* compte dédié "replicator".

## Administration

* serveur dédié WMS-PGADMIN ;
* séparation entre administration et production.

## Supervision

* Zabbix Server ;
* agents Zabbix ;
* tableau de bord de supervision.

---

# 7. Validation du dispositif

Les éléments suivants ont été validés durant le projet :

* fonctionnement de la réplication ;
* surveillance de l'infrastructure ;
* génération automatique des sauvegardes ;
* transfert des sauvegardes vers WMS-BACKUP ;
* restauration complète des données ;
* contrôle des accès utilisateurs ;
* supervision des serveurs critiques ;
* continuité d'activité en cas de panne du serveur principal.

Le PCA/PRA répond ainsi aux objectifs de disponibilité, de sécurité et de résilience définis pour le projet WMS.
