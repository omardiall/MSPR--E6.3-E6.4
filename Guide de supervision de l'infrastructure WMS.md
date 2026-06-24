# Guide de supervision de l'infrastructure WMS

# 1. Objectif

La supervision de l'infrastructure WMS est assurée par la solution Zabbix.

L'objectif est de :

* surveiller l'état des serveurs ;
* détecter les incidents ;
* suivre les performances ;
* anticiper les défaillances ;
* garantir la disponibilité des services.

---

# 2. Architecture de supervision

Le serveur de supervision est :

| Serveur     | Adresse IP      | Fonction      |
| ----------- | --------------- | ------------- |
| WMS-SUPER01 | 192.168.233.132 | Zabbix Server |

Les serveurs supervisés sont :

| Serveur   | Fonction           |
| --------- | ------------------ |
| WMS-DB-01 | PostgreSQL Primary |
| WMS-DB-02 | PostgreSQL Replica |

Chaque serveur dispose d'un agent Zabbix chargé de transmettre les informations de supervision.

---

# 3. Indicateurs surveillés## 2. Architecture de supervision

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
| WMS-BACKUP  | Stockage des sauvegardes  |
| WMS-SUPER01 | Serveur de supervision    |

Chaque serveur dispose d'un agent Zabbix chargé de transmettre les informations de supervision.

La supervision couvre l'ensemble des composants critiques de l'infrastructure afin de garantir la disponibilité des services et la détection rapide des incidents.

---

## Utilisation disque

Objectif :

Prévenir la saturation du stockage.

Une attention particulière est portée au serveur WMS-BACKUP qui héberge les sauvegardes externalisées de la base PostgreSQL.

Seuils :

| Niveau       | Valeur    |
| ------------ | --------- |
| Normal       | < 80 %    |
| Surveillance | 80 à 90 % |
| Critique     | > 90 %    |

Action :

* Supprimer les fichiers inutiles.
* Vérifier les sauvegardes.
* Archiver les journaux.
* Contrôler l'espace disponible sur WMS-BACKUP.

---

## État de la réplication PostgreSQL

Objectif :

Vérifier la synchronisation permanente entre WMS-DB-01 et WMS-DB-02.

Indicateur surveillé :

```text
state = streaming
```

Seuil d'alerte :

```text
state != streaming
```

Action :

* Vérifier le serveur Replica.
* Vérifier la connectivité réseau.
* Contrôler les journaux PostgreSQL.
* Vérifier la configuration de réplication.

---

## État des sauvegardes

Objectif :

Vérifier que les sauvegardes sont correctement générées puis transférées vers WMS-BACKUP.

Seuil d'alerte :

```text
Absence de sauvegarde quotidienne
```

Action :

* Vérifier le script de sauvegarde.
* Vérifier Cron.
* Vérifier le transfert vers WMS-BACKUP.
* Contrôler l'espace disque disponible.

---

## Disponibilité de pgAdmin

Objectif :

Vérifier l'accessibilité de la plateforme d'administration PostgreSQL.

Serveur surveillé :

```text
WMS-PGADMIN
```

Seuil d'alerte :

```text
Service pgAdmin indisponible
```

Action :

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
* la supervision du serveur WMS-BACKUP.

La solution de supervision répond ainsi aux exigences de disponibilité, de sécurité et d'exploitation du projet WMS tout en assurant la surveillance de l'ensemble des composants de l'infrastructure.
