# Démarche d'optimisation de la base de données PostgreSQL

## 1. Objectif

L'objectif de cette phase était d'améliorer les performances de la base de données WMS afin de garantir des temps de réponse satisfaisants lors des opérations de consultation et de gestion des stocks.

Les optimisations ont porté sur :

* la conception du modèle de données ;
* la mise en place de contraintes d'intégrité ;
* l'utilisation d'index ;
* l'analyse des plans d'exécution PostgreSQL ;
* la séparation des rôles de l'infrastructure afin de limiter les charges inutiles sur le serveur de base de données principal.

---

# 2. Analyse des usages

Les principales opérations métier identifiées sont :

* consultation du stock d'un client ;
* recherche d'un article ;
* consultation de l'historique des mouvements ;
* recherche des localisations d'un article ;
* suivi des mouvements logistiques.

Ces opérations sont susceptibles d'être exécutées fréquemment et nécessitent des performances satisfaisantes.

Afin d'éviter que les tâches d'administration et de sauvegarde n'impactent les performances du serveur principal, l'architecture a été conçue autour de plusieurs serveurs spécialisés :

* WMS-DB-01 : traitement des opérations PostgreSQL ;
* WMS-DB-02 : réplication et reprise d'activité ;
* WMS-PGADMIN : administration graphique ;
* WMS-BACKUP : stockage externe des sauvegardes ;
* WMS-SUPER01 : supervision de l'environnement.

Cette séparation réduit les risques de saturation des ressources du serveur principal et améliore les performances globales de la plateforme.

---

# 7. Résultats obtenus

Les optimisations réalisées permettent :

* une recherche rapide des clients ;
* une recherche rapide des articles ;
* une meilleure cohérence des données ;
* une réduction du risque d'incohérence ;
* une amélioration générale des performances de lecture ;
* une réduction de la charge d'administration sur le serveur PostgreSQL principal ;
* une meilleure répartition des services grâce à l'utilisation de serveurs spécialisés ;
* une amélioration de la résilience globale de l'infrastructure.

---

# 9. Conclusion

La base PostgreSQL a été optimisée en combinant une modélisation rigoureuse, des contraintes d'intégrité, des mécanismes d'indexation et une architecture d'infrastructure adaptée.

Les performances ne reposent pas uniquement sur l'optimisation SQL mais également sur une séparation des rôles entre les différents serveurs de la plateforme. L'utilisation d'une machine dédiée à l'administration PostgreSQL (WMS-PGADMIN), d'un serveur dédié aux sauvegardes (WMS-BACKUP), d'un serveur de réplication (WMS-DB-02) et d'un serveur de supervision (WMS-SUPER01) permet de préserver les ressources du serveur principal et d'améliorer la stabilité de l'ensemble du système.

Ces optimisations permettent d'assurer de bonnes performances tout en garantissant la qualité, la disponibilité et la cohérence des données utilisées par le système WMS.
