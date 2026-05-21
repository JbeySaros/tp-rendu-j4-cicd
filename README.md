# TP Rendu J4 — Pipeline CI/CD complet

Pipeline Jenkins qui déploie une application Flask sur 2 cibles Docker
en stratégie **Rolling Update**.

## Architecture

```
GitHub → Jenkins → Ansible (Rolling) → target1 + target2
```

## Stack

- Python 3 + Flask (application)
- Pytest (tests unitaires)
- Ansible (déploiement)
- Jenkins (CI/CD)
- Docker (cibles)

## Lancement local

```bash
# Tests
cd app && pytest -v

# Déploiement manuel
cd ansible && ansible-playbook -i inventory.ini site.yml
```

## Auteur

JbeySaros - Promotion B3 - Formation InfoSoftware
