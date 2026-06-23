# Note de direction

## Objet : Sécurisation et continuité d'activité du système WMS

### Contexte

Dans le cadre de la modernisation de l'infrastructure informatique de gestion logistique, une nouvelle plateforme WMS (Warehouse Management System) a été mise en place afin de centraliser la gestion des stocks, des mouvements de marchandises et des opérations logistiques.

Cette plateforme repose sur une base de données PostgreSQL supervisée en permanence et protégée contre les incidents majeurs pouvant affecter la continuité des activités.

---

## Risques identifiés

Plusieurs risques susceptibles d'impacter l'activité ont été identifiés :

* panne d'un serveur de base de données ;
* perte ou corruption de données ;
* suppression accidentelle d'informations ;
* défaillance matérielle ;
* erreur humaine ;
* saturation des ressources système.

Ces événements peuvent entraîner une interruption du service et des pertes opérationnelles importantes.

---

## Solutions mises en œuvre

Afin de réduire ces risques, plusieurs mécanismes ont été déployés :

### Haute disponibilité

Deux serveurs PostgreSQL ont été mis en place :

* un serveur principal ;
* un serveur secondaire de secours.

Les données sont répliquées automatiquement entre les deux serveurs afin de garantir leur disponibilité.

### Sauvegardes

Des sauvegardes automatiques sont réalisées quotidiennement afin de permettre la restauration rapide des données en cas d'incident.

Des tests de restauration ont également été effectués afin de valider la fiabilité du dispositif.

### Supervision

Une plateforme de supervision Zabbix surveille en permanence :

* la disponibilité des serveurs ;
* l'utilisation du processeur ;
* la mémoire ;
* l'espace disque ;
* l'état général de l'infrastructure.

Les anomalies peuvent être détectées rapidement avant qu'elles n'affectent l'activité.

### Sécurité

Un système de gestion des droits d'accès a été mis en œuvre.

Chaque utilisateur dispose uniquement des permissions nécessaires à ses fonctions conformément au principe du moindre privilège.

---

## Bénéfices pour l'entreprise

La solution mise en place apporte plusieurs avantages :

* amélioration de la disponibilité du système ;
* réduction du risque de perte de données ;
* amélioration de la sécurité ;
* meilleure maîtrise de l'exploitation informatique ;
* réduction des interruptions de service ;
* amélioration de la réactivité face aux incidents.

---

## Conclusion

L'infrastructure déployée répond aux besoins actuels de l'entreprise en matière de disponibilité, de sécurité et de continuité d'activité.

Les mécanismes de réplication, de sauvegarde et de supervision permettent de garantir un niveau de service élevé tout en limitant les risques opérationnels.
