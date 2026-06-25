# Conclusion générale

Dans le cadre de ce projet MSPR, nous avons conçu, déployé, sécurisé et documenté une infrastructure de base de données complète répondant aux exigences de disponibilité, de sécurité, de supervision et de continuité d'activité définies dans le cahier des charges de NordTransit Logistics.

La solution mise en œuvre repose sur PostgreSQL et s'appuie sur une architecture distribuée composée de cinq serveurs spécialisés :

* WMS-DB-01 : serveur PostgreSQL principal ;
* WMS-DB-02 : serveur PostgreSQL secondaire assurant la réplication ;
* WMS-PGADMIN : serveur dédié à l'administration de la base de données ;
* WMS-BACKUP : serveur dédié au stockage externalisé des sauvegardes ;
* WMS-SUPER01 : serveur de supervision Zabbix.

Cette organisation permet de séparer les rôles critiques de l'infrastructure afin d'améliorer la sécurité, la disponibilité des services et la résilience globale du système.

Afin de garantir la continuité d'activité, une réplication PostgreSQL de type Primary / Replica a été mise en œuvre et validée par des tests de synchronisation. Cette réplication permet d'assurer la disponibilité des données et facilite les opérations de reprise en cas d'incident affectant le serveur principal.

La protection des données repose sur une stratégie de sauvegarde automatisée associée à un stockage externalisé sur le serveur WMS-BACKUP. Des procédures de restauration ont été testées avec succès afin de vérifier la capacité de récupération des données en cas de suppression accidentelle, de corruption ou de défaillance matérielle.

La supervision de l'ensemble de l'infrastructure est assurée par Zabbix. Les principaux indicateurs surveillés sont la disponibilité des serveurs, l'utilisation du processeur, la mémoire, l'espace disque, l'état de la réplication PostgreSQL ainsi que le suivi des sauvegardes. Cette supervision permet une détection rapide des incidents et contribue à améliorer la qualité de service.

Sur le plan de la sécurité, un modèle de contrôle d'accès basé sur les rôles (RBAC) a été déployé conformément au principe du moindre privilège. Plusieurs profils utilisateurs ont été définis afin de limiter les permissions accordées aux seules opérations nécessaires à chaque fonction.

Le projet a également permis la réalisation de nombreux livrables professionnels comprenant notamment :

* l'architecture technique ;
* le MCD et le MLD ;
* le guide de supervision ;
* le guide d'administration et d'exploitation ;
* le RunBook d'exploitation ;
* le PCA/PRA ;
* l'analyse des logs ;
* la démarche d'optimisation de la base de données ;
* la gestion de projet et l'analyse des risques ;
* la note de direction.

Au-delà des aspects techniques, cette MSPR nous a permis de développer des compétences en administration PostgreSQL, en haute disponibilité, en sauvegarde et restauration, en supervision d'infrastructure, en sécurisation des accès, en gestion de projet ainsi qu'en documentation technique.

L'ensemble des objectifs définis dans le cahier des charges a été atteint. L'infrastructure réalisée constitue une solution robuste, sécurisée, supervisée et évolutive, conforme aux bonnes pratiques professionnelles et capable de répondre aux besoins opérationnels de NordTransit Logistics.

Cette expérience nous a permis de mettre en pratique les compétences attendues d'un Administrateur Systèmes, Réseaux et Bases de Données dans un contexte proche des environnements de production rencontrés en entreprise.
