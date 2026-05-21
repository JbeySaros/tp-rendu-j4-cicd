# Réponses – TP CI/CD Jenkins / Ansible

## Question 1 (0.5 pt)

Dans le playbook, si on retire `serial: 1`, Ansible n’exécute plus le déploiement de manière progressive hôte par hôte.  
Les hôtes du groupe `web` (par exemple target1 et target2) seront traités en parallèle ou en une seule vague, selon la configuration d’Ansible.

Cela signifie qu’il n’y a plus de déploiement contrôlé par lots : toutes les machines reçoivent les modifications en même temps.

---

## Question 2 (1 pt)

target1 reste en version v2 car il a déjà terminé son déploiement avant que target2 ne rencontre une erreur.

Avec `serial: 1`, chaque hôte est déployé indépendamment. Ainsi, un échec sur target2 n’annule pas les changements déjà appliqués à target1.

Cela est préférable à un déploiement “Big Bang” car :
- la zone d’impact d’un échec est réduite (blast radius limité),
- une seule machine peut échouer sans bloquer l’ensemble du système,
- cela améliore la disponibilité globale pendant le déploiement.

---

## Question 3 (1 pt)

Le stage de tests unitaires permet de détecter les bugs avant le déploiement en production.

Cela est préférable car :
- les erreurs sont détectées tôt dans le pipeline,
- on évite de déployer du code instable en production,
- on réduit les incidents utilisateurs.

Le principe de “fail fast” signifie que le pipeline s’arrête immédiatement dès qu’une erreur est détectée, empêchant la propagation du problème aux étapes suivantes.

---

## Question 4 (1 pt)

Deux améliorations possibles pour rendre le pipeline plus production-ready :

1. Ajouter un scan de sécurité (SAST ou analyse des dépendances Python avec pip-audit ou Trivy) afin de détecter les vulnérabilités avant déploiement.

2. Mettre en place un mécanisme de rollback automatique en cas d’échec du healthcheck après déploiement, afin de restaurer rapidement la version précédente stable.

---

## Question 5 (0.5 pt)

La métrique DORA la plus directement améliorée est la **Deployment Frequency**.

Le pipeline CI/CD automatisé permet de déployer plus souvent et de manière reproductible sans intervention manuelle. Cela augmente la fréquence des livraisons et réduit la friction entre développement et production.
