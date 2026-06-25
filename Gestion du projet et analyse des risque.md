# Gestion du projet et analyse des risques

## 1. Présentation du projet

Le projet MSPR TPRE623 a consisté à concevoir, déployer, sécuriser, superviser et documenter une infrastructure de base de données destinée au système de gestion d'entrepôt (WMS) de l'entreprise fictive NordTransit Logistics (NTL).

L'objectif principal était de mettre en place une infrastructure fiable permettant :

* la gestion des données logistiques ;
* la haute disponibilité du service ;
* la protection des données ;
* la supervision de l'infrastructure ;
* la continuité d'activité ;
* la sécurisation des accès.

L'architecture finale repose sur cinq serveurs spécialisés :

* WMS-DB-01 : PostgreSQL Primary ;
* WMS-DB-02 : PostgreSQL Replica ;
* WMS-PGADMIN : administration PostgreSQL ;
* WMS-BACKUP : stockage des sauvegardes ;
* WMS-SUPER01 : supervision Zabbix.

---

# 2. Organisation du projet

Le projet a été réalisé selon une approche collaborative reposant sur une répartition claire des responsabilités entre les membres de l'équipe.

Les principales activités réalisées ont été :

* conception de l'architecture ;
* modélisation de la base de données ;
* déploiement PostgreSQL ;
* mise en place de la réplication ;
* automatisation des sauvegardes ;
* mise en œuvre du serveur de sauvegarde ;
* déploiement de pgAdmin ;
* mise en place de la supervision ;
* sécurisation des accès ;
* rédaction de la documentation ;
* validation des procédures de restauration.

---

# 3. Répartition des responsabilités

## Omar Bachir Diallo

### Chef de projet et Administrateur Base de Données

Responsabilités :

* pilotage du projet ;
* coordination de l'équipe ;
* déploiement PostgreSQL ;
* mise en œuvre de la réplication ;
* configuration des sauvegardes ;
* mise en place du serveur WMS-BACKUP ;
* validation des restaurations ;
* documentation technique ;
* validation finale de l'infrastructure.

---

## Filda

### Conception et modélisation

Responsabilités :

* analyse des besoins ;
* conception du modèle de données ;
* réalisation du MCD ;
* réalisation du MLD ;
* définition des contraintes métier ;
* validation de la cohérence des données ;
* participation à la documentation.

---

## Amine

### Supervision et exploitation

Responsabilités :

* installation de Zabbix ;
* configuration des agents ;
* création du dashboard ;
* définition des indicateurs de supervision ;
* surveillance CPU, RAM et stockage ;
* supervision des sauvegardes ;
* analyse des journaux ;
* validation des indicateurs.

---

## Madgyd

### Sécurité et continuité d'activité

Responsabilités :

* mise en œuvre du RBAC ;
* création des rôles PostgreSQL ;
* gestion des permissions ;
* déploiement du serveur WMS-PGADMIN ;
* participation au PCA/PRA ;
* analyse des risques ;
* procédures d'exploitation ;
* participation aux livrables de gestion de projet.

---

# 4. Planning des principales étapes

| Étape | Description                       |
| ----- | --------------------------------- |
| 1     | Analyse du cahier des charges     |
| 2     | Conception du modèle de données   |
| 3     | Création du MCD et du MLD         |
| 4     | Déploiement PostgreSQL            |
| 5     | Mise en place de la réplication   |
| 6     | Configuration des sauvegardes     |
| 7     | Déploiement du serveur WMS-BACKUP |
| 8     | Déploiement de pgAdmin            |
| 9     | Mise en place de Zabbix           |
| 10    | Sécurisation RBAC                 |
| 11    | Tests de restauration             |
| 12    | Validation des livrables          |
| 13    | Préparation de la soutenance      |

---

# 5. Analyse des risques

L'identification des risques constitue une étape essentielle afin d'assurer la disponibilité et la continuité d'activité du système.

| Risque                              | Probabilité | Impact     | Mesure mise en œuvre                  |
| ----------------------------------- | ----------- | ---------- | ------------------------------------- |
| Panne du serveur principal          | Moyenne     | Élevé      | Réplication PostgreSQL                |
| Perte de données                    | Faible      | Très élevé | Sauvegardes automatiques + WMS-BACKUP |
| Erreur humaine                      | Moyenne     | Moyen      | RBAC et contrôles d'accès             |
| Saturation du disque                | Moyenne     | Élevé      | Supervision Zabbix                    |
| Défaillance logicielle              | Faible      | Élevé      | Procédures de restauration            |
| Mauvaise manipulation               | Moyenne     | Moyen      | Documentation d'exploitation          |
| Compromission de pgAdmin            | Faible      | Élevé      | Serveur dédié WMS-PGADMIN             |
| Défaillance des sauvegardes locales | Faible      | Très élevé | Sauvegardes externalisées             |

---

# 6. Difficultés rencontrées

Plusieurs difficultés techniques ont été rencontrées durant le projet :

* configuration de la réplication PostgreSQL ;
* paramétrage des accès réseau ;
* configuration des conteneurs Docker ;
* mise en place des sauvegardes ;
* intégration de Zabbix ;
* gestion des permissions PostgreSQL ;
* configuration du RBAC ;
* validation des procédures de restauration.

Ces difficultés ont été résolues grâce à des phases successives de tests, d'analyse et de validation.

---

# 7. Solutions apportées

Afin de répondre aux contraintes du cahier des charges, plusieurs solutions ont été retenues :

* PostgreSQL pour la gestion des données ;
* Docker pour le déploiement des services ;
* Réplication PostgreSQL pour la haute disponibilité ;
* pgAdmin pour l'administration ;
* WMS-BACKUP pour les sauvegardes externalisées ;
* Zabbix pour la supervision ;
* RBAC pour la gestion des accès ;
* PCA/PRA pour la continuité d'activité.

---

# 8. Résultats obtenus

Le projet a permis de mettre en place une infrastructure complète intégrant :

* haute disponibilité ;
* supervision centralisée ;
* sauvegardes externalisées ;
* restauration des données ;
* sécurisation des accès ;
* administration dédiée ;
* continuité d'activité ;
* documentation d'exploitation.

Les objectifs fixés dans le cahier des charges ont été atteints.

---

# 9. Enseignements tirés du projet

Cette MSPR a permis de développer des compétences dans les domaines suivants :

* administration PostgreSQL ;
* réplication de bases de données ;
* sauvegarde et restauration ;
* supervision d'infrastructure ;
* sécurisation des accès ;
* gestion des risques ;
* gestion de projet ;
* documentation technique.

Elle a également permis de mettre en pratique les méthodes de travail collaboratif et les bonnes pratiques utilisées dans les environnements professionnels.

---

# 10. Conclusion

La gestion du projet a permis d'assurer la bonne coordination des différentes tâches nécessaires à la réalisation de l'infrastructure WMS.

Grâce à une répartition claire des responsabilités, à une analyse rigoureuse des risques et à la mise en œuvre de solutions adaptées, l'équipe a pu livrer une infrastructure robuste, sécurisée et conforme aux exigences du cahier des charges.

Le projet constitue une mise en situation professionnelle complète couvrant les principaux domaines de compétence d'un Administrateur Systèmes, Réseaux et Bases de Données.
