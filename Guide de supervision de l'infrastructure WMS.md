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

# 3. Indicateurs surveillés

## Disponibilité

Objectif :

Vérifier que les serveurs sont joignables.

Seuil d'alerte :

```text
Serveur indisponible
```

Action :

* Vérifier le réseau.
* Vérifier l'état du serveur.
* Vérifier les services Docker.

---

## Utilisation CPU

Objectif :

Mesurer la charge processeur.

Seuils :

| Niveau       | Valeur    |
| ------------ | --------- |
| Normal       | < 70 %    |
| Surveillance | 70 à 85 % |
| Critique     | > 85 %    |

Action :

* Identifier le processus responsable.
* Vérifier les requêtes PostgreSQL.
* Contrôler les conteneurs Docker.

---

## Utilisation mémoire

Objectif :

Mesurer la consommation RAM.

Seuils :

| Niveau       | Valeur    |
| ------------ | --------- |
| Normal       | < 75 %    |
| Surveillance | 75 à 90 % |
| Critique     | > 90 %    |

Action :

* Identifier les processus consommateurs.
* Vérifier PostgreSQL.
* Contrôler Docker.

---

## Utilisation disque

Objectif :

Prévenir la saturation du stockage.

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

---

# 4. Tableau de bord

Le tableau de bord Zabbix centralise :

* disponibilité des serveurs ;
* utilisation CPU ;
* utilisation mémoire ;
* utilisation disque ;
* alertes système.

Le dashboard constitue le point principal de supervision de l'infrastructure.

---

# 5. Procédure de contrôle quotidien

Chaque jour :

1. Vérifier le dashboard Zabbix.
2. Contrôler les alertes.
3. Vérifier l'espace disque.
4. Vérifier l'état de la réplication PostgreSQL.
5. Vérifier les sauvegardes.

---

# 6. Procédure de contrôle hebdomadaire

Chaque semaine :

1. Contrôler les performances PostgreSQL.
2. Vérifier les journaux système.
3. Vérifier les journaux de sauvegarde.
4. Contrôler les utilisateurs et permissions.
5. Vérifier l'état des conteneurs Docker.

---

# 7. Gestion des alertes

## Alerte CPU

Actions :

* Identifier le processus responsable.
* Vérifier les requêtes PostgreSQL.
* Contrôler l'activité Docker.

---

## Alerte mémoire

Actions :

* Vérifier PostgreSQL.
* Vérifier les conteneurs.
* Contrôler les applications connectées.

---

## Alerte disque

Actions :

* Identifier les fichiers volumineux.
* Contrôler les sauvegardes.
* Vérifier les journaux.

---

## Alerte indisponibilité serveur

Actions :

1. Vérifier la connectivité réseau.
2. Vérifier Docker.
3. Vérifier PostgreSQL.
4. Vérifier les journaux système.

---

# 8. Validation

Les tests réalisés durant le projet ont permis de valider :

* la remontée des métriques ;
* la disponibilité des agents ;
* le fonctionnement du dashboard ;
* la supervision des ressources critiques.

La solution de supervision répond ainsi aux exigences de disponibilité et d'exploitation du projet WMS.
