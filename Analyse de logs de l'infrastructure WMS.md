# Analyse de logs de l'infrastructure WMS

# 1. Objectif

L'objectif de cette analyse est de vérifier le bon fonctionnement de l'infrastructure WMS à travers l'exploitation des journaux système et applicatifs.

L'analyse des logs permet de :

- détecter les incidents ;
- identifier les erreurs ;
- contrôler les accès ;
- vérifier la réplication PostgreSQL ;
- valider les sauvegardes ;
- faciliter le diagnostic en cas de panne.

---

# 2. Sources de logs analysées

## PostgreSQL

Les journaux PostgreSQL permettent de surveiller :

- les connexions utilisateurs ;
- les erreurs SQL ;
- les problèmes de réplication ;
- les opérations d'administration ;
- les erreurs de sauvegarde et de restauration.

Commande utilisée :

```bash
docker logs postgres-primary
```

---

## PostgreSQL Replica

Les journaux du Replica permettent de vérifier :

- la réception des journaux WAL ;
- l'état de la réplication ;
- les éventuelles pertes de synchronisation.

Commande utilisée :

```bash
docker logs postgres-replica
```

---

## Docker

Les journaux Docker permettent de surveiller :

- le démarrage des conteneurs ;
- l'arrêt inattendu d'un service ;
- les erreurs d'exécution.

Commande utilisée :

```bash
docker logs <nom_du_conteneur>
```

---

## Zabbix

Les journaux Zabbix permettent de contrôler :

- la collecte des métriques ;
- les alertes ;
- l'état des agents ;
- les erreurs de supervision.

Commande utilisée :

```bash
docker logs zabbix-server
```

---

# 3. Cas analysés

## Cas n°1 : Vérification de la réplication PostgreSQL

Exemple de log observé :

```text
LOG: started streaming WAL from primary
```

Interprétation :

Le serveur Replica reçoit correctement les journaux WAL du serveur principal.

Conclusion :

La réplication PostgreSQL fonctionne correctement.

---

## Cas n°2 : Connexion utilisateur

Exemple de log observé :

```text
connection authorized: user=wms_admin
```

Interprétation :

L'utilisateur dispose des autorisations nécessaires et la connexion est acceptée.

Conclusion :

Le contrôle d'accès fonctionne correctement.

---

## Cas n°3 : Sauvegarde réussie

Exemple de log observé :

```text
Backup completed successfully
```

Interprétation :

La sauvegarde s'est exécutée sans erreur.

Conclusion :

La stratégie de sauvegarde est opérationnelle.

---

## Cas n°4 : Supervision Zabbix

Exemple de log observé :

```text
Zabbix agent started
```

Interprétation :

L'agent de supervision communique correctement avec le serveur Zabbix.

Conclusion :

La supervision est fonctionnelle.

---

# 4. Procédure de vérification des logs

Les journaux doivent être consultés régulièrement.

Commandes utilisées :

```bash
docker logs postgres-primary --tail 50
```

```bash
docker logs postgres-replica --tail 50
```

```bash
docker logs zabbix-server --tail 50
```

Points à vérifier :

- erreurs critiques ;
- problèmes de réplication ;
- échecs de sauvegarde ;
- connexions suspectes ;
- erreurs système ;
- alertes répétitives.

---

# 5. Actions correctives

En cas d'erreur détectée :

1. Identifier le composant concerné.
2. Analyser le message d'erreur.
3. Vérifier l'état du service.
4. Consulter les journaux complémentaires.
5. Corriger le problème.
6. Contrôler le retour à la normale.

---

# 6. Résultats obtenus

Les analyses réalisées durant le projet ont permis de confirmer :

- le bon fonctionnement de PostgreSQL ;
- le bon fonctionnement de la réplication ;
- la réussite des sauvegardes ;
- le bon fonctionnement de la supervision ;
- la disponibilité des services critiques.

---

# 7. Conclusion

L'analyse régulière des journaux constitue un élément essentiel de l'exploitation de l'infrastructure WMS.

Elle permet d'anticiper les incidents, de faciliter les diagnostics et d'assurer la disponibilité des services.

Les contrôles effectués durant le projet n'ont révélé aucune anomalie critique et confirment le bon fonctionnement global de l'infrastructure PostgreSQL, de la réplication, des sauvegardes et de la supervision.
