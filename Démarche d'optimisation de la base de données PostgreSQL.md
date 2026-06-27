# Démarche d'optimisation de la base de données PostgreSQL

## 1. Objectif

L'objectif de cette phase est d'améliorer les performances de la base de données WMS afin de garantir des temps de réponse satisfaisants lors des opérations de consultation, de gestion des stocks et de suivi des mouvements logistiques.

Les optimisations ont porté sur :

* la conception du modèle de données ;
* la mise en place de contraintes d'intégrité ;
* l'utilisation d'index ;
* l'analyse des usages métier ;
* l'analyse des plans d'exécution PostgreSQL ;
* la séparation des rôles entre les serveurs de l'infrastructure.

---

# 2. Analyse des usages

Les principales opérations métier identifiées sont :

* consultation du stock d'un client ;
* recherche d'un article par SKU ;
* consultation de l'historique des mouvements ;
* recherche des localisations d'un article ;
* suivi des entrées et sorties de stock ;
* consultation des opérations réalisées par un utilisateur.

Ces opérations sont susceptibles d'être exécutées fréquemment par l'application WMS et nécessitent donc des performances correctes.

Afin de limiter la charge sur le serveur PostgreSQL principal, l'architecture repose sur plusieurs serveurs spécialisés :

* WMS-DB-01 : traitement des opérations PostgreSQL principales ;
* WMS-DB-02 : réplication et continuité d'activité ;
* WMS-PGADMIN : administration PostgreSQL ;
* WMS-BACKUP : stockage externe des sauvegardes ;
* WMS-SUPER01 : supervision de l'environnement.

Cette séparation permet de ne pas concentrer l'administration, les sauvegardes et la supervision sur le serveur de production.

---

# 3. Contraintes mises en œuvre

Afin d'améliorer la cohérence des données et d'éviter les incohérences métier, plusieurs contraintes ont été implémentées.

## Clés primaires

* client_pkey
* article_pkey
* entrepot_pkey
* localisation_pkey
* stock_pkey
* mouvement_stock_pkey
* utilisateur_pkey

## Clés étrangères

* localisation → entrepot
* stock → client
* stock → article
* stock → localisation
* mouvement_stock → client
* mouvement_stock → utilisateur
* mouvement_stock → article
* mouvement_stock → localisation

## Contraintes UNIQUE

* email client
* email utilisateur
* code entrepot
* sku article

## Contraintes CHECK

* quantité positive ;
* dimensions positives ;
* poids positif ;
* type de mouvement contrôlé ;
* rôle utilisateur contrôlé.

Ces contraintes permettent de garantir la qualité des données et d'éviter les erreurs de saisie.

---

# 4. Optimisation par indexation

PostgreSQL crée automatiquement des index sur :

* les clés primaires ;
* les colonnes définies comme UNIQUE.

Ces index permettent d'accélérer les recherches sur les colonnes les plus utilisées.

Exemples :

* recherche d'un client par email ;
* recherche d'un article par SKU ;
* recherche d'un entrepôt par code ;
* accès rapide aux lignes par identifiant.

Des index complémentaires peuvent également être mis en place sur les colonnes utilisées fréquemment dans les filtres ou les jointures, par exemple :

```sql
CREATE INDEX idx_stock_client
ON stock(id_client);
```

```sql
CREATE INDEX idx_stock_article
ON stock(id_article);
```

```sql
CREATE INDEX idx_mouvement_article
ON mouvement_stock(id_article);
```

```sql
CREATE INDEX idx_mouvement_date
ON mouvement_stock(date_mouvement);
```

Ces index permettent d'améliorer les performances des requêtes les plus fréquentes liées au stock et aux mouvements.

---

# 5. Scénarios de test

Plusieurs requêtes métier représentatives ont été utilisées.

## Recherche d'un client par email

```sql
SELECT *
FROM client
WHERE email = 'replication@test.fr';
```

## Consultation du stock d'un client

```sql
SELECT *
FROM stock
WHERE id_client = 1;
```

## Consultation des mouvements d'un article

```sql
SELECT *
FROM mouvement_stock
WHERE id_article = 1;
```

## Consultation des mouvements récents

```sql
SELECT *
FROM mouvement_stock
ORDER BY date_mouvement DESC;
```

Ces requêtes représentent des usages fréquents dans une application WMS.

---

# 6. Analyse du plan d'exécution

L'outil PostgreSQL `EXPLAIN ANALYZE` a été utilisé afin d'étudier le comportement du moteur de base de données.

Exemple :

```sql
EXPLAIN ANALYZE
SELECT *
FROM client
WHERE email = 'replication@test.fr';
```

Le plan d'exécution permet de vérifier si PostgreSQL utilise un index ou effectue un parcours complet de table.

Autre exemple :

```sql
EXPLAIN ANALYZE
SELECT *
FROM stock
WHERE id_client = 1;
```

Après création d'un index sur la colonne `id_client`, PostgreSQL peut accéder plus rapidement aux lignes concernées.

---

# 7. Résultats obtenus

Les optimisations réalisées permettent :

* une recherche rapide des clients ;
* une recherche rapide des articles ;
* une consultation plus efficace des stocks ;
* une consultation plus rapide des mouvements ;
* une meilleure cohérence des données ;
* une réduction du risque d'incohérence ;
* une meilleure répartition des charges entre les serveurs ;
* une réduction de la charge d'administration sur le serveur PostgreSQL principal ;
* une amélioration de la stabilité globale de l'infrastructure.

---

# 8. Bonnes pratiques appliquées

Les bonnes pratiques suivantes ont été respectées :

* normalisation des données ;
* utilisation systématique des clés primaires ;
* utilisation des clés étrangères ;
* mise en place de contraintes d'intégrité ;
* limitation des doublons ;
* utilisation d'index sur les colonnes stratégiques ;
* séparation des rôles entre production, administration, sauvegarde et supervision ;
* supervision des ressources système.

---

# 9. Conclusion

La base PostgreSQL a été optimisée en combinant une modélisation rigoureuse, des contraintes d'intégrité, des mécanismes d'indexation et une architecture d'infrastructure adaptée.

L'optimisation ne repose pas uniquement sur les requêtes SQL. Elle repose également sur une séparation claire des rôles entre les différents serveurs :

* WMS-DB-01 pour la production PostgreSQL ;
* WMS-DB-02 pour la réplication ;
* WMS-PGADMIN pour l'administration ;
* WMS-BACKUP pour les sauvegardes externalisées ;
* WMS-SUPER01 pour la supervision.

Cette approche permet d'améliorer les performances, la disponibilité et la sécurité globale de l'infrastructure WMS.

# PostgreSQL Database Optimization Methodology

## 1. Objective

The objective of this phase was to improve the performance of the WMS database in order to ensure satisfactory response times for inventory management, stock monitoring, and logistics operations.

The optimization process focused on:

* database model design;
* implementation of integrity constraints;
* index utilization;
* business usage analysis;
* PostgreSQL execution plan analysis;
* role separation across infrastructure servers.

---

# 2. Usage Analysis

The main business operations identified are:

* customer inventory consultation;
* product search using SKU;
* inventory movement history consultation;
* product location lookup;
* stock entry and exit tracking;
* review of operations performed by users.

These operations are frequently executed by the WMS application and therefore require efficient performance.

To reduce the workload on the Primary PostgreSQL server, the infrastructure relies on several dedicated servers:

* WMS-DB-01: Primary PostgreSQL operations;
* WMS-DB-02: Replication and business continuity;
* WMS-PGADMIN: PostgreSQL administration;
* WMS-BACKUP: External backup storage;
* WMS-SUPER01: Infrastructure monitoring.

This separation prevents administration, backup, and monitoring activities from impacting the production server.

---

# 3. Implemented Constraints

To improve data consistency and prevent business logic inconsistencies, several constraints were implemented.

## Primary Keys

* client_pkey
* article_pkey
* entrepot_pkey
* localisation_pkey
* stock_pkey
* mouvement_stock_pkey
* utilisateur_pkey

## Foreign Keys

* localisation → entrepot
* stock → client
* stock → article
* stock → localisation
* mouvement_stock → client
* mouvement_stock → utilisateur
* mouvement_stock → article
* mouvement_stock → localisation

## UNIQUE Constraints

* client email
* user email
* warehouse code
* product SKU

## CHECK Constraints

* positive quantity;
* positive dimensions;
* positive weight;
* controlled movement type;
* controlled user role.

These constraints ensure data quality and help prevent data entry errors.

---

# 4. Index Optimization

PostgreSQL automatically creates indexes on:

* primary keys;
* columns defined as UNIQUE.

These indexes accelerate searches on the most frequently used columns.

Examples:

* customer lookup by email;
* product lookup by SKU;
* warehouse lookup by code;
* fast access to records by identifier.

Additional indexes can also be created on columns frequently used in filters and joins.

Example:

```sql
CREATE INDEX idx_stock_client
ON stock(id_client);
```

```sql
CREATE INDEX idx_stock_article
ON stock(id_article);
```

```sql
CREATE INDEX idx_mouvement_article
ON mouvement_stock(id_article);
```

```sql
CREATE INDEX idx_mouvement_date
ON mouvement_stock(date_mouvement);
```

These indexes improve the performance of the most common inventory and logistics queries.

---

# 5. Test Scenarios

Several representative business queries were used.

## Customer Lookup by Email

```sql
SELECT *
FROM client
WHERE email = 'replication@test.fr';
```

## Customer Inventory Consultation

```sql
SELECT *
FROM stock
WHERE id_client = 1;
```

## Product Movement Consultation

```sql
SELECT *
FROM mouvement_stock
WHERE id_article = 1;
```

## Recent Inventory Movements

```sql
SELECT *
FROM mouvement_stock
ORDER BY date_mouvement DESC;
```

These queries represent common operations within a WMS environment.

---

# 6. Execution Plan Analysis

The PostgreSQL `EXPLAIN ANALYZE` tool was used to study database engine behavior.

Example:

```sql
EXPLAIN ANALYZE
SELECT *
FROM client
WHERE email = 'replication@test.fr';
```

The execution plan makes it possible to determine whether PostgreSQL is using an index or performing a full table scan.

Another example:

```sql
EXPLAIN ANALYZE
SELECT *
FROM stock
WHERE id_client = 1;
```

After creating an index on the `id_client` column, PostgreSQL can access the relevant records more efficiently.

---

# 7. Results Achieved

The implemented optimizations provide:

* faster customer searches;
* faster product searches;
* more efficient inventory consultations;
* improved inventory movement retrieval;
* better data consistency;
* reduced risk of inconsistencies;
* improved workload distribution across servers;
* reduced administrative workload on the Primary PostgreSQL server;
* improved overall infrastructure stability.

---

# 8. Applied Best Practices

The following best practices were applied:

* database normalization;
* systematic use of primary keys;
* use of foreign keys;
* implementation of integrity constraints;
* duplicate data prevention;
* strategic indexing of critical columns;
* separation of production, administration, backup, and monitoring functions;
* continuous infrastructure monitoring.

---

# 9. Conclusion

The PostgreSQL database was optimized through a combination of rigorous data modeling, integrity constraints, indexing mechanisms, and an appropriate infrastructure design.

Optimization does not rely solely on SQL query performance. It also relies on a clear separation of responsibilities between the different servers:

* WMS-DB-01 for PostgreSQL production workloads;
* WMS-DB-02 for replication;
* WMS-PGADMIN for administration;
* WMS-BACKUP for externalized backups;
* WMS-SUPER01 for monitoring.

This approach improves performance, availability, and overall security of the WMS infrastructure while ensuring efficient and reliable database operations.
