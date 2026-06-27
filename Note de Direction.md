# Note de direction

## Objet : Sécurisation et continuité d'activité du système WMS

---

# Contexte

Dans le cadre de la modernisation de l'infrastructure informatique de gestion logistique, une nouvelle plateforme WMS (Warehouse Management System) a été mise en place afin de centraliser la gestion des stocks, des mouvements de marchandises et des opérations logistiques.

Cette plateforme repose sur une infrastructure PostgreSQL sécurisée, supervisée et protégée contre les incidents susceptibles d'affecter la continuité des activités de l'entreprise.

L'objectif est de garantir la disponibilité des données, la continuité de service et la protection des informations critiques tout en facilitant l'exploitation quotidienne de l'environnement.

---

# Risques identifiés

Plusieurs risques susceptibles d'impacter l'activité ont été identifiés :

* panne du serveur principal ;
* perte ou corruption de données ;
* suppression accidentelle d'informations ;
* défaillance matérielle ;
* erreur humaine ;
* saturation des ressources système ;
* indisponibilité des sauvegardes ;
* interruption des services critiques.

Ces événements peuvent entraîner une interruption de service, une perte de productivité ou une indisponibilité temporaire des données.

---

# Solutions mises en œuvre

Afin de réduire ces risques, plusieurs mécanismes ont été déployés.

## Haute disponibilité

Deux serveurs PostgreSQL ont été mis en place :

* un serveur principal (WMS-DB-01) ;
* un serveur secondaire de secours (WMS-DB-02).

Les données sont répliquées automatiquement entre les deux serveurs afin de garantir leur disponibilité et de permettre une reprise rapide en cas d'incident.

Cette architecture réduit fortement le risque d'interruption prolongée du service.

---

## Sauvegardes

Des sauvegardes automatiques sont réalisées quotidiennement afin de permettre la restauration rapide des données en cas d'incident.

Afin de renforcer la protection des informations, les sauvegardes sont également transférées vers un serveur dédié (WMS-BACKUP) indépendant du serveur principal.

Cette organisation permet :

* d'éviter la perte simultanée des données et des sauvegardes ;
* d'améliorer la résilience de l'infrastructure ;
* de faciliter les opérations de restauration.

Des tests de restauration ont été réalisés afin de valider la fiabilité du dispositif.

---

## Supervision

Une plateforme de supervision Zabbix surveille en permanence :

* la disponibilité des serveurs ;
* l'utilisation du processeur ;
* la mémoire ;
* l'espace disque ;
* l'état de la réplication PostgreSQL ;
* l'état des sauvegardes ;
* l'état général de l'infrastructure.

Les anomalies peuvent ainsi être détectées rapidement avant qu'elles n'affectent les activités de l'entreprise.

---

## Sécurité

Un système de gestion des droits d'accès basé sur les rôles (RBAC) a été mis en œuvre.

Chaque utilisateur dispose uniquement des permissions nécessaires à ses fonctions conformément au principe du moindre privilège.

Les rôles définis permettent de limiter les risques liés aux erreurs de manipulation et aux accès non autorisés.

Par ailleurs, l'outil d'administration PostgreSQL (pgAdmin) est hébergé sur un serveur dédié (WMS-PGADMIN) afin de séparer les opérations d'administration de l'environnement de production.

Cette séparation améliore la sécurité globale de l'infrastructure.

---

# Architecture retenue

L'infrastructure finale repose sur cinq serveurs spécialisés :

| Serveur     | Fonction                  |
| ----------- | ------------------------- |
| WMS-DB-01   | PostgreSQL Primary        |
| WMS-DB-02   | PostgreSQL Replica        |
| WMS-PGADMIN | Administration PostgreSQL |
| WMS-BACKUP  | Sauvegardes externalisées |
| WMS-SUPER01 | Supervision Zabbix        |

Cette organisation permet une meilleure séparation des responsabilités et facilite l'exploitation de l'environnement.

---

# Bénéfices pour l'entreprise

La solution mise en place apporte plusieurs avantages :

* amélioration de la disponibilité du système ;
* réduction du risque de perte de données ;
* amélioration de la sécurité ;
* meilleure maîtrise de l'exploitation informatique ;
* réduction des interruptions de service ;
* amélioration de la réactivité face aux incidents ;
* protection renforcée des sauvegardes ;
* meilleure résilience de l'infrastructure ;
* simplification des opérations d'administration ;
* amélioration de la continuité d'activité.

---

# Perspectives d'évolution

L'infrastructure mise en place constitue une base solide pouvant être enrichie à l'avenir par :

* l'automatisation avancée des procédures de bascule ;
* l'ajout de mécanismes de haute disponibilité supplémentaires ;
* la supervision applicative avancée ;
* la centralisation des journaux ;
* la mise en place d'une supervision de sécurité renforcée.

---

# Conclusion

L'infrastructure déployée répond aux besoins actuels de NordTransit Logistics en matière de disponibilité, de sécurité, de supervision et de continuité d'activité.

L'architecture repose sur plusieurs serveurs spécialisés assurant respectivement la production des données, la réplication, l'administration, la sauvegarde et la supervision.

Les mécanismes de réplication, de sauvegarde externalisée, de supervision centralisée et de contrôle d'accès permettent de garantir un niveau de service élevé tout en limitant les risques opérationnels.

Cette solution offre à l'entreprise une infrastructure robuste, évolutive et conforme aux bonnes pratiques professionnelles.

# Executive Management Report

## Subject: Security and Business Continuity of the WMS Platform

---

# Background

As part of the modernization of the logistics IT infrastructure, a new Warehouse Management System (WMS) platform has been implemented to centralize inventory management, goods movement tracking, and logistics operations.

This platform relies on a secure, monitored, and resilient PostgreSQL infrastructure designed to protect business operations from incidents that could affect service continuity.

The objective is to ensure data availability, service continuity, and protection of critical business information while simplifying the day-to-day management of the environment.

---

# Identified Risks

Several risks that could impact business operations have been identified:

* Primary server failure;
* Data loss or corruption;
* Accidental deletion of information;
* Hardware failure;
* Human error;
* System resource saturation;
* Backup unavailability;
* Critical service interruption.

These events may result in service disruption, productivity loss, or temporary unavailability of business data.

---

# Implemented Solutions

Several mechanisms have been deployed to mitigate these risks.

## High Availability

Two PostgreSQL servers have been implemented:

* a Primary server (WMS-DB-01);
* a secondary failover server (WMS-DB-02).

Data is automatically replicated between the two servers to ensure availability and enable rapid recovery in the event of an incident affecting the Primary server.

This architecture significantly reduces the risk of prolonged service outages.

---

## Backup Strategy

Automated backups are performed daily to ensure rapid data recovery in the event of an incident.

To further strengthen data protection, backups are transferred to a dedicated server (WMS-BACKUP) independent of the Primary server.

This architecture provides:

* protection against simultaneous loss of production data and backups;
* improved infrastructure resilience;
* simplified restoration operations.

Restoration tests have been successfully conducted to validate the reliability of the backup strategy.

---

## Monitoring

A Zabbix monitoring platform continuously supervises:

* server availability;
* CPU utilization;
* memory usage;
* disk space consumption;
* PostgreSQL replication status;
* backup status;
* overall infrastructure health.

This enables rapid detection of anomalies before they impact business operations.

---

## Security

A Role-Based Access Control (RBAC) model has been implemented.

Each user is granted only the permissions required for their responsibilities, in accordance with the Principle of Least Privilege.

The defined roles reduce the risks associated with accidental errors and unauthorized access.

Additionally, the PostgreSQL administration platform (pgAdmin) is hosted on a dedicated server (WMS-PGADMIN), separating administrative activities from the production environment.

This separation significantly enhances the overall security of the infrastructure.

---

# Selected Architecture

The final infrastructure is based on five specialized servers:

| Server      | Function                  |
| ----------- | ------------------------- |
| WMS-DB-01   | PostgreSQL Primary        |
| WMS-DB-02   | PostgreSQL Replica        |
| WMS-PGADMIN | PostgreSQL Administration |
| WMS-BACKUP  | External Backup Storage   |
| WMS-SUPER01 | Zabbix Monitoring         |

This architecture provides clear separation of responsibilities and simplifies operational management.

---

# Business Benefits

The implemented solution provides several key advantages:

* improved system availability;
* reduced risk of data loss;
* enhanced security;
* improved control over IT operations;
* reduced service interruptions;
* faster incident response;
* stronger backup protection;
* improved infrastructure resilience;
* simplified administration;
* enhanced business continuity.

---

# Future Enhancements

The implemented infrastructure provides a solid foundation that can be extended in the future through:

* advanced failover automation;
* additional high-availability mechanisms;
* advanced application monitoring;
* centralized log management;
* enhanced security monitoring.

---

# Conclusion

The deployed infrastructure fully meets NordTransit Logistics' current requirements in terms of availability, security, monitoring, and business continuity.

The architecture relies on several specialized servers dedicated respectively to production, replication, administration, backup, and monitoring activities.

The combination of replication, externalized backups, centralized monitoring, and role-based access control ensures a high level of service while minimizing operational risks.

This solution provides the company with a robust, scalable, and professional-grade infrastructure aligned with industry best practices and capable of supporting future growth and operational requirements.
