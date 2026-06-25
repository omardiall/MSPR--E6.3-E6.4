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
