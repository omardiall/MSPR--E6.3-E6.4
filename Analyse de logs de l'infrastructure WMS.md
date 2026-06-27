# Analyse de logs de l'infrastructure WMS

# 1. Objectif

L'objectif de cette analyse est de vérifier le bon fonctionnement de l'infrastructure WMS à travers l'exploitation des journaux système et applicatifs.

L'analyse des logs permet de :

- détecter les incidents ;
- identifier les erreurs ;
- contrôler les accès ;
- vérifier la réplication PostgreSQL ;
- valider les sauvegardes ;
- faciliter le diagnostic en cas de panne.

---

# 2. Sources de logs analysées

## PostgreSQL

Les journaux PostgreSQL permettent de surveiller :

- les connexions utilisateurs ;
- les erreurs SQL ;
- les problèmes de réplication ;
- les opérations d'administration ;
- les erreurs de sauvegarde et de restauration.

Commande utilisée :

```bash
docker logs postgres-primary
```

---

## PostgreSQL Replica

Les journaux du Replica permettent de vérifier :

- la réception des journaux WAL ;
- l'état de la réplication ;
- les éventuelles pertes de synchronisation.

Commande utilisée :

```bash
docker logs postgres-replica
```

---

## Docker

Les journaux Docker permettent de surveiller :

- le démarrage des conteneurs ;
- l'arrêt inattendu d'un service ;
- les erreurs d'exécution.

Commande utilisée :

```bash
docker logs <nom_du_conteneur>
```

---

## Zabbix

Les journaux Zabbix permettent de contrôler :

- la collecte des métriques ;
- les alertes ;
- l'état des agents ;
- les erreurs de supervision.

Commande utilisée :

```bash
docker logs zabbix-server
```

---

# 3. Cas analysés

## Cas n°1 : Vérification de la réplication PostgreSQL

Exemple de log observé :

```text
LOG: started streaming WAL from primary
```

Interprétation :

Le serveur Replica reçoit correctement les journaux WAL du serveur principal.

Conclusion :

La réplication PostgreSQL fonctionne correctement.

---

## Cas n°2 : Connexion utilisateur

Exemple de log observé :

```text
connection authorized: user=wms_admin
```

Interprétation :

L'utilisateur dispose des autorisations nécessaires et la connexion est acceptée.

Conclusion :

Le contrôle d'accès fonctionne correctement.

---

## Cas n°3 : Sauvegarde réussie

Exemple de log observé :

```text
Backup completed successfully
```

Interprétation :

La sauvegarde s'est exécutée sans erreur.

Conclusion :

La stratégie de sauvegarde est opérationnelle.

---

## Cas n°4 : Supervision Zabbix

Exemple de log observé :

```text
Zabbix agent started
```

Interprétation :

L'agent de supervision communique correctement avec le serveur Zabbix.

Conclusion :

La supervision est fonctionnelle.

---

# 4. Procédure de vérification des logs

Les journaux doivent être consultés régulièrement.

Commandes utilisées :

```bash
docker logs postgres-primary --tail 50
```

```bash
docker logs postgres-replica --tail 50
```

```bash
docker logs zabbix-server --tail 50
```

Points à vérifier :

- erreurs critiques ;
- problèmes de réplication ;
- échecs de sauvegarde ;
- connexions suspectes ;
- erreurs système ;
- alertes répétitives.

---

# 5. Actions correctives

En cas d'erreur détectée :

1. Identifier le composant concerné.
2. Analyser le message d'erreur.
3. Vérifier l'état du service.
4. Consulter les journaux complémentaires.
5. Corriger le problème.
6. Contrôler le retour à la normale.

---

# 6. Résultats obtenus

Les analyses réalisées durant le projet ont permis de confirmer :

- le bon fonctionnement de PostgreSQL ;
- le bon fonctionnement de la réplication ;
- la réussite des sauvegardes ;
- le bon fonctionnement de la supervision ;
- la disponibilité des services critiques.

---

# 7. Conclusion

L'analyse régulière des journaux constitue un élément essentiel de l'exploitation de l'infrastructure WMS.

Elle permet d'anticiper les incidents, de faciliter les diagnostics et d'assurer la disponibilité des services.

Les contrôles effectués durant le projet n'ont révélé aucune anomalie critique et confirment le bon fonctionnement global de l'infrastructure PostgreSQL, de la réplication, des sauvegardes et de la supervision.

# WMS Infrastructure Log Analysis

# 1. Purpose

The purpose of this analysis is to verify the proper operation of the WMS infrastructure through the examination of system and application logs.

Log analysis helps to:

* detect incidents;
* identify errors;
* monitor access attempts;
* verify PostgreSQL replication;
* validate backup operations;
* facilitate troubleshooting and incident diagnosis.

---

# 2. Log Sources Analyzed

## PostgreSQL

PostgreSQL logs are used to monitor:

* user connections;
* SQL errors;
* replication issues;
* administrative operations;
* backup and restore errors.

Command used:

```bash
docker logs postgres-primary
```

---

## PostgreSQL Replica

Replica logs are used to verify:

* WAL log reception;
* replication status;
* potential synchronization issues.

Command used:

```bash
docker logs postgres-replica
```

---

## Docker

Docker logs are used to monitor:

* container startup events;
* unexpected service shutdowns;
* runtime errors.

Command used:

```bash
docker logs <container_name>
```

---

## Zabbix

Zabbix logs are used to monitor:

* metric collection;
* alert generation;
* agent status;
* monitoring errors.

Command used:

```bash
docker logs zabbix-server
```

---

# 3. Analyzed Cases

## Case #1: PostgreSQL Replication Verification

Example log entry:

```text
LOG: started streaming WAL from primary
```

Interpretation:

The Replica server is successfully receiving WAL logs from the Primary server.

Conclusion:

PostgreSQL replication is operating correctly.

---

## Case #2: User Connection

Example log entry:

```text
connection authorized: user=wms_admin
```

Interpretation:

The user has the required permissions and the connection has been successfully established.

Conclusion:

Access control is functioning correctly.

---

## Case #3: Successful Backup

Example log entry:

```text
Backup completed successfully
```

Interpretation:

The backup operation completed successfully without any errors.

Conclusion:

The backup strategy is operational.

---

## Case #4: Zabbix Monitoring

Example log entry:

```text
Zabbix agent started
```

Interpretation:

The monitoring agent is successfully communicating with the Zabbix server.

Conclusion:

The monitoring infrastructure is functioning properly.

---

# 4. Log Verification Procedure

Logs should be reviewed regularly.

Commands used:

```bash
docker logs postgres-primary --tail 50
```

```bash
docker logs postgres-replica --tail 50
```

```bash
docker logs zabbix-server --tail 50
```

Items to verify:

* critical errors;
* replication issues;
* backup failures;
* suspicious connections;
* system errors;
* recurring alerts.

---

# 5. Corrective Actions

If an error is detected:

1. Identify the affected component.
2. Analyze the error message.
3. Verify the service status.
4. Review additional logs.
5. Apply the necessary correction.
6. Confirm service recovery.

---

# 6. Results Obtained

The analyses performed during the project confirmed:

* proper PostgreSQL operation;
* successful replication;
* successful backup execution;
* proper monitoring operation;
* availability of critical services.

---

# 7. Conclusion

Regular log analysis is a key component of WMS infrastructure operations.

It enables proactive incident detection, facilitates troubleshooting, and helps ensure service availability.

The checks performed throughout the project did not reveal any critical issues and confirmed the proper operation of PostgreSQL, replication, backup processes, and the monitoring infrastructure.

