# Plan de Continuité d'Activité (PCA) et Plan de Reprise d'Activité (PRA)

# 1. Objectif

L'objectif du PCA/PRA est de garantir la continuité de service du système WMS en cas d'incident affectant l'infrastructure informatique.

La stratégie retenue repose sur :

* une réplication PostgreSQL Primary / Replica ;
* des sauvegardes automatiques quotidiennes ;
* un serveur dédié au stockage des sauvegardes ;
* une supervision centralisée via Zabbix ;
* un contrôle d'accès basé sur les rôles (RBAC) ;
* des procédures de restauration validées ;
* une séparation des fonctions d'administration, de supervision et de sauvegarde.

L'objectif est de minimiser les interruptions de service et de réduire les pertes de données en cas d'incident.

---

# 2. Analyse des risques

| Risque                                   | Impact                           |
| ---------------------------------------- | -------------------------------- |
| Panne du serveur principal               | Interruption du service          |
| Corruption de la base de données         | Perte de données                 |
| Suppression accidentelle de données      | Altération des opérations        |
| Saturation disque                        | Arrêt des sauvegardes            |
| Défaillance matérielle                   | Indisponibilité de l'application |
| Erreur humaine                           | Dysfonctionnement opérationnel   |
| Perte des sauvegardes locales            | Difficulté de restauration       |
| Compromission d'un compte administrateur | Risque de sécurité               |

---

# 3. Objectifs de reprise

## RTO (Recovery Time Objective)

Temps maximal de reprise :

```text
1 heure
```

L'objectif est de remettre le service en fonctionnement dans un délai maximal d'une heure après la détection d'un incident critique.

---

## RPO (Recovery Point Objective)

Perte maximale de données acceptable :

```text
15 minutes
```

Grâce à la réplication PostgreSQL et aux sauvegardes régulières, la perte potentielle de données reste limitée.

---

# 4. Architecture concernée

L'infrastructure est composée des éléments suivants :

| Serveur     | Fonction                  |
| ----------- | ------------------------- |
| WMS-DB-01   | PostgreSQL Primary        |
| WMS-DB-02   | PostgreSQL Replica        |
| WMS-PGADMIN | Administration PostgreSQL |
| WMS-BACKUP  | Sauvegardes externalisées |
| WMS-SUPER01 | Supervision Zabbix        |

Cette architecture permet de répartir les rôles critiques et d'améliorer la résilience globale du système.

---

# 5. Plan de Continuité d'Activité (PCA)

## Mesures préventives

Afin de maintenir la continuité du service :

* réplication PostgreSQL entre WMS-DB-01 et WMS-DB-02 ;
* supervision permanente via Zabbix ;
* sauvegardes automatiques quotidiennes ;
* stockage externalisé des sauvegardes sur WMS-BACKUP ;
* administration PostgreSQL isolée sur WMS-PGADMIN ;
* contrôle des accès basé sur les rôles (RBAC) ;
* surveillance continue des ressources système.

---

## Surveillance continue

Les éléments surveillés sont :

* disponibilité des serveurs ;
* charge CPU ;
* utilisation mémoire ;
* espace disque ;
* état de la réplication ;
* disponibilité des services PostgreSQL ;
* disponibilité de pgAdmin ;
* disponibilité du serveur de sauvegarde ;
* présence des sauvegardes externalisées.

Toutes les alertes sont centralisées au sein de la plateforme Zabbix.

---

# 6. Plan de Reprise d'Activité (PRA)

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

Commande :

```sql
SELECT pg_promote();
```

5. Reconfigurer les applications vers WMS-DB-02.
6. Contrôler l'accès aux données.

### Résultat attendu

Le serveur secondaire devient le nouveau serveur principal et assure la continuité du service.

---

## Scénario 2 : Suppression accidentelle de données

### Détection

Constat de disparition de données métier.

### Procédure

1. Identifier les données concernées.
2. Récupérer la dernière sauvegarde disponible sur WMS-BACKUP.
3. Restaurer la sauvegarde.
4. Contrôler l'intégrité des données.
5. Vérifier la cohérence applicative.

### Résultat attendu

Les données sont restaurées et les opérations peuvent reprendre normalement.

---

## Scénario 3 : Corruption de la base de données

### Procédure

1. Isoler le serveur concerné.
2. Vérifier les journaux PostgreSQL.
3. Restaurer la dernière sauvegarde valide.
4. Vérifier l'intégrité des données.
5. Reconfigurer la réplication si nécessaire.
6. Contrôler les services applicatifs.

### Résultat attendu

La base de données retrouve un état cohérent et opérationnel.

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

Le service reste disponible grâce au Replica et les données restent récupérables grâce aux sauvegardes externalisées.

---

## Scénario 5 : Indisponibilité de pgAdmin

### Procédure

1. Vérifier le serveur WMS-PGADMIN.
2. Vérifier Docker.
3. Contrôler le conteneur pgAdmin.
4. Vérifier la connectivité avec WMS-DB-01.
5. Redémarrer le service si nécessaire.

### Résultat attendu

L'administration PostgreSQL redevient accessible sans impact sur la production.

---

# 7. Moyens techniques mobilisés

## Sauvegardes

* pg_dump ;
* planification Cron ;
* stockage local temporaire ;
* transfert automatique vers WMS-BACKUP ;
* conservation externalisée des sauvegardes.

---

## Réplication

* PostgreSQL Streaming Replication ;
* compte dédié "replicator" ;
* synchronisation continue des données.

---

## Administration

* pgAdmin ;
* serveur dédié WMS-PGADMIN ;
* séparation administration / production.

---

## Supervision

* Zabbix Server ;
* agents Zabbix ;
* tableau de bord centralisé ;
* alertes automatiques.

---

# 8. Validation du dispositif

Les éléments suivants ont été validés durant le projet :

* fonctionnement de la réplication ;
* génération automatique des sauvegardes ;
* transfert des sauvegardes vers WMS-BACKUP ;
* restauration complète des données ;
* contrôle des accès utilisateurs ;
* supervision de l'infrastructure ;
* surveillance des sauvegardes ;
* disponibilité des services critiques.

---

# 9. Conclusion

Le PCA/PRA mis en place répond aux objectifs de disponibilité, de sécurité et de continuité d'activité définis pour le projet WMS.

Grâce à la combinaison de la réplication PostgreSQL, des sauvegardes externalisées, de la supervision centralisée et des procédures de restauration validées, l'infrastructure est capable de résister à la majorité des incidents pouvant affecter son fonctionnement.

Cette organisation permet de garantir un niveau de service élevé tout en limitant les risques opérationnels pour NordTransit Logistics.
