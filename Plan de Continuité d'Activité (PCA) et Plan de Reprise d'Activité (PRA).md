# Plan de Continuité d'Activité (PCA) et Plan de Reprise d'Activité (PRA)

# 1. Objectif

L'objectif du PCA/PRA est de garantir la continuité de service de l'application WMS en cas d'incident affectant l'infrastructure informatique.

La stratégie retenue repose sur :

* une réplication PostgreSQL Primary / Replica ;
* des sauvegardes automatiques quotidiennes ;
* une supervision centralisée avec Zabbix ;
* des procédures de restauration validées.

---

# 2. Analyse des risques

| Risque                              | Impact                           |
| ----------------------------------- | -------------------------------- |
| Panne du serveur principal          | Interruption du service          |
| Corruption de la base de données    | Perte de données                 |
| Suppression accidentelle de données | Altération des opérations        |
| Saturation disque                   | Arrêt des sauvegardes            |
| Défaillance matérielle              | Indisponibilité de l'application |
| Erreur humaine                      | Dysfonctionnement opérationnel   |

---

# 3. Objectifs de reprise

## RTO (Recovery Time Objective)

Temps maximal de reprise :

```text
1 heure
```

L'objectif est de remettre le service en fonctionnement dans un délai maximal d'une heure après la détection de l'incident.

---

## RPO (Recovery Point Objective)

Perte maximale de données acceptable :

```text
15 minutes
```

La réplication PostgreSQL permet de limiter fortement les pertes potentielles de données.

---

# 4. Plan de Continuité d'Activité (PCA)

## Mesures préventives

Afin de maintenir la continuité du service :

* réplication PostgreSQL entre WMS-DB-01 et WMS-DB-02 ;
* supervision permanente via Zabbix ;
* sauvegardes automatiques quotidiennes ;
* contrôle des accès basé sur les rôles (RBAC) ;
* surveillance des ressources système.

---

## Surveillance continue

Les éléments surveillés sont :

* disponibilité des serveurs ;
* charge CPU ;
* utilisation mémoire ;
* espace disque ;
* état de la réplication ;
* disponibilité des services PostgreSQL.

Les alertes sont centralisées dans Zabbix.

---

# 5. Plan de Reprise d'Activité (PRA)

## Scénario 1 : Panne du serveur principal

### Détection

L'incident est détecté par :

* les alertes Zabbix ;
* l'indisponibilité de PostgreSQL ;
* l'impossibilité de connexion à la base.

### Procédure

1. Vérifier l'état du serveur WMS-DB-01.
2. Contrôler l'intégrité du serveur WMS-DB-02.
3. Vérifier l'état de la réplication.
4. Promouvoir le Replica en serveur principal.

Commande PostgreSQL :

```sql
SELECT pg_promote();
```

5. Reconfigurer les applications pour utiliser WMS-DB-02.
6. Contrôler l'accès aux données.

### Résultat attendu

Le serveur secondaire devient le nouveau serveur principal et assure la continuité du service.

---

## Scénario 2 : Suppression accidentelle de données

### Détection

Constat de disparition de données métier.

### Procédure

1. Identifier les données supprimées.
2. Restaurer la sauvegarde la plus récente.
3. Contrôler l'intégrité des données restaurées.
4. Vérifier la cohérence applicative.

### Validation

Le test de restauration a été réalisé avec succès durant le projet.

---

## Scénario 3 : Corruption de la base de données

### Procédure

1. Isoler le serveur concerné.
2. Vérifier les journaux PostgreSQL.
3. Restaurer la dernière sauvegarde valide.
4. Vérifier la réplication.
5. Contrôler les données restaurées.

---

# 6. Moyens techniques mobilisés

## Sauvegardes

* pg_dump ;
* planification Cron ;
* stockage local des sauvegardes.

## Réplication

* PostgreSQL Streaming Replication ;
* compte dédié "replicator".

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
* restauration complète des données ;
* contrôle des accès utilisateurs.

Le PCA/PRA répond ainsi aux objectifs de disponibilité et de sécurité définis pour le projet WMS.
