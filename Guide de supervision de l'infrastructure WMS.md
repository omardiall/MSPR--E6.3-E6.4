# Guide de supervision de l'infrastructure WMS

# 1. Objectif

La supervision de l'infrastructure WMS est assurée par la solution Zabbix.

L'objectif est de :

* surveiller l'état des serveurs ;
* détecter les incidents ;
* suivre les performances ;
* anticiper les défaillances ;
* garantir la disponibilité des services ;
* surveiller la réplication PostgreSQL ;
* contrôler les sauvegardes ;
* faciliter l'exploitation quotidienne.

---

# 2. Architecture de supervision

Le serveur de supervision est :

| Serveur     | Adresse IP      | Fonction      |
| ----------- | --------------- | ------------- |
| WMS-SUPER01 | 192.168.233.132 | Zabbix Server |

Les serveurs supervisés sont :

| Serveur     | Fonction                  |
| ----------- | ------------------------- |
| WMS-DB-01   | PostgreSQL Primary        |
| WMS-DB-02   | PostgreSQL Replica        |
| WMS-PGADMIN | Administration PostgreSQL |
| WMS-BACKUP  | Sauvegardes externalisées |
| WMS-SUPER01 | Supervision Zabbix        |

Chaque serveur dispose d'un agent Zabbix chargé de transmettre les informations de supervision.

La supervision couvre l'ensemble des composants critiques de l'infrastructure afin de garantir la disponibilité des services et la détection rapide des incidents.

---

# 3. Indicateurs surveillés

## Disponibilité

### Objectif

Vérifier que les serveurs sont joignables et opérationnels.

### Serveurs surveillés

* WMS-DB-01
* WMS-DB-02
* WMS-PGADMIN
* WMS-BACKUP
* WMS-SUPER01

### Seuil d'alerte

```text
Serveur indisponible
```

### Action

* Vérifier le réseau.
* Vérifier l'état du serveur.
* Vérifier Docker.
* Vérifier les journaux système.

---

## Utilisation CPU

### Objectif

Mesurer la charge processeur.

### Seuils

| Niveau       | Valeur    |
| ------------ | --------- |
| Normal       | < 70 %    |
| Surveillance | 70 à 85 % |
| Critique     | > 85 %    |

### Action

* Identifier le processus responsable.
* Vérifier les requêtes PostgreSQL.
* Contrôler les conteneurs Docker.
* Contrôler les services système.

---

## Utilisation mémoire

### Objectif

Mesurer la consommation RAM.

### Seuils

| Niveau       | Valeur    |
| ------------ | --------- |
| Normal       | < 75 %    |
| Surveillance | 75 à 90 % |
| Critique     | > 90 %    |

### Action

* Identifier les processus consommateurs.
* Vérifier PostgreSQL.
* Contrôler Docker.
* Vérifier les applications connectées.

---

## Utilisation disque

### Objectif

Prévenir la saturation du stockage.

Une attention particulière est portée au serveur WMS-BACKUP qui héberge les sauvegardes externalisées.

### Seuils

| Niveau       | Valeur    |
| ------------ | --------- |
| Normal       | < 80 %    |
| Surveillance | 80 à 90 % |
| Critique     | > 90 %    |

### Action

* Supprimer les fichiers inutiles.
* Vérifier les sauvegardes.
* Archiver les journaux.
* Contrôler l'espace disponible sur WMS-BACKUP.

---

## État de la réplication PostgreSQL

### Objectif

Vérifier la synchronisation permanente entre WMS-DB-01 et WMS-DB-02.

### Indicateur surveillé

```text
state = streaming
```

### Seuil d'alerte

```text
state != streaming
```

### Action

* Vérifier le serveur Replica.
* Vérifier la connectivité réseau.
* Contrôler les journaux PostgreSQL.
* Vérifier la configuration de réplication.

---

## État des sauvegardes

### Objectif

Vérifier que les sauvegardes sont correctement générées puis transférées vers WMS-BACKUP.

### Seuil d'alerte

```text
Absence de sauvegarde quotidienne
```

### Action

* Vérifier le script de sauvegarde.
* Vérifier Cron.
* Vérifier le transfert vers WMS-BACKUP.
* Contrôler l'espace disque disponible.

---

## Disponibilité de pgAdmin

### Objectif

Vérifier l'accessibilité de la plateforme d'administration PostgreSQL.

### Serveur surveillé

```text
WMS-PGADMIN
```

### Seuil d'alerte

```text
Service pgAdmin indisponible
```

### Action

* Vérifier Docker.
* Vérifier le conteneur pgAdmin.
* Vérifier la connectivité avec WMS-DB-01.
* Contrôler les journaux applicatifs.

---

# 4. Tableau de bord

Le tableau de bord Zabbix centralise :

* disponibilité des serveurs ;
* utilisation CPU ;
* utilisation mémoire ;
* utilisation disque ;
* état de la réplication PostgreSQL ;
* état des sauvegardes ;
* disponibilité de pgAdmin ;
* alertes système.

Le dashboard constitue le point principal de supervision de l'infrastructure.

---

# 5. Procédure de contrôle quotidien

Chaque jour :

1. Vérifier le dashboard Zabbix.
2. Contrôler les alertes.
3. Vérifier l'espace disque.
4. Vérifier l'état de la réplication PostgreSQL.
5. Vérifier les sauvegardes sur WMS-BACKUP.
6. Vérifier l'accessibilité de WMS-PGADMIN.
7. Contrôler les indicateurs CPU et mémoire.

---

# 6. Procédure de contrôle hebdomadaire

Chaque semaine :

1. Contrôler les performances PostgreSQL.
2. Vérifier les journaux système.
3. Vérifier les journaux de sauvegarde.
4. Contrôler les utilisateurs et permissions.
5. Vérifier l'état des conteneurs Docker.
6. Vérifier le bon fonctionnement de WMS-PGADMIN.
7. Vérifier la capacité de stockage de WMS-BACKUP.
8. Vérifier les historiques d'alertes Zabbix.

---

# 7. Gestion des alertes

## Alerte CPU

### Actions

* Identifier le processus responsable.
* Vérifier les requêtes PostgreSQL.
* Contrôler l'activité Docker.
* Vérifier les ressources système.

---

## Alerte mémoire

### Actions

* Vérifier PostgreSQL.
* Vérifier les conteneurs.
* Contrôler les applications connectées.
* Vérifier les consommations anormales.

---

## Alerte disque

### Actions

* Identifier les fichiers volumineux.
* Contrôler les sauvegardes.
* Vérifier les journaux.
* Contrôler l'espace disponible sur WMS-BACKUP.

---

## Alerte réplication

### Actions

1. Vérifier l'état du Replica.
2. Vérifier le réseau.
3. Contrôler PostgreSQL.
4. Vérifier les journaux PostgreSQL.
5. Contrôler la vue pg_stat_replication.

---

## Alerte sauvegarde

### Actions

1. Vérifier Cron.
2. Vérifier le script de sauvegarde.
3. Vérifier le stockage sur WMS-BACKUP.
4. Contrôler les journaux de sauvegarde.

---

## Alerte indisponibilité serveur

### Actions

1. Vérifier la connectivité réseau.
2. Vérifier Docker.
3. Vérifier PostgreSQL ou le service concerné.
4. Vérifier les journaux système.
5. Contrôler les ressources matérielles.

---

# 8. Validation

Les tests réalisés durant le projet ont permis de valider :

* la remontée des métriques ;
* la disponibilité des agents ;
* le fonctionnement du dashboard ;
* la supervision des ressources critiques ;
* le suivi de la réplication PostgreSQL ;
* le suivi des sauvegardes externalisées ;
* la supervision du serveur WMS-PGADMIN ;
* la supervision du serveur WMS-BACKUP ;
* la détection des alertes critiques.

---

# 9. Bénéfices de la supervision

La supervision mise en place permet :

* une détection rapide des incidents ;
* une meilleure disponibilité des services ;
* un suivi continu des performances ;
* une anticipation des problèmes de capacité ;
* une amélioration de la réactivité des équipes d'exploitation ;
* une réduction des risques d'interruption de service.

---

# 10. Conclusion

La solution Zabbix mise en œuvre répond aux exigences de supervision de l'infrastructure WMS.

Grâce à la surveillance centralisée de l'ensemble des serveurs, de la réplication PostgreSQL, des sauvegardes externalisées et des ressources système, l'équipe d'exploitation dispose d'une visibilité complète sur l'état de la plateforme.

Cette supervision contribue directement à la disponibilité, à la sécurité et à la continuité d'activité du système WMS.
