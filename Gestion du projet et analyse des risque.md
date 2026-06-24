# Gestion de projet et analyse des risques

## Organisation du projet

Le projet a été réalisé selon une approche collaborative reposant sur une répartition des responsabilités entre les membres de l'équipe.

Les principales activités réalisées ont été :

* conception de l'architecture ;
* déploiement de PostgreSQL ;
* mise en place de la réplication ;
* automatisation des sauvegardes ;
* mise en œuvre de la supervision ;
* sécurisation des accès ;
* validation des procédures de restauration.

---

## Analyse des risques

| Risque                     | Probabilité | Impact | Mesure mise en œuvre         |
| -------------------------- | ----------- | ------ | ---------------------------- |
| Panne du serveur principal | Moyenne     | Élevé  | Réplication PostgreSQL       |
| Perte de données           | Faible      | Élevé  | Sauvegardes automatiques     |
| Erreur humaine             | Moyenne     | Moyen  | RBAC et contrôles d'accès    |
| Saturation du disque       | Moyenne     | Élevé  | Supervision Zabbix           |
| Défaillance logicielle     | Faible      | Élevé  | Procédures de restauration   |
| Mauvaise manipulation      | Moyenne     | Moyen  | Documentation d'exploitation |

---

## Difficultés rencontrées

Plusieurs difficultés techniques ont été rencontrées :

* configuration de la réplication PostgreSQL ;
* paramétrage des accès réseau ;
* configuration des conteneurs Docker ;
* intégration de la supervision Zabbix ;
* gestion des permissions PostgreSQL.

Ces difficultés ont été résolues grâce à des phases de tests et de validation successives.

---

## Résultats obtenus

Le projet a permis de mettre en place une infrastructure complète intégrant :

* haute disponibilité ;
* supervision ;
* sauvegarde ;
* restauration ;
* sécurisation des accès ;
* continuité d'activité.

Les objectifs fixés dans le cahier des charges ont été atteints.
