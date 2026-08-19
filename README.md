# Futurisys ML API

Futurisys est une API développée avec **FastAPI** permettant de déployer un modèle de **Machine Learning** en production.  
Le projet expose un modèle de classification via une API REST documentée automatiquement avec **Swagger/OpenAPI**.

```
Futurisys
│
├── .github/workflows   → CI/CD GitHub Actions
├── app                 → API FastAPI
│   ├── database
│   ├── models
│   └── main.py
│
├── ml_model            → modèle ML + training
├── tests               → tests Pytest
│
├── create_tables.py
├── requirements.txt
└── README.md
```

Ce projet illustre les bonnes pratiques d’ingénierie logicielle appliquées au machine learning :

- architecture de projet structurée  
- API FastAPI  
- gestion des dépendances  
- tests automatisés  
- CI/CD  
- déploiement sur une plateforme cloud  

---

# Objectif du projet

L'objectif est de rendre un modèle de machine learning accessible via une **API performante et documentée** afin de pouvoir être utilisé par d'autres applications.

---

# Prérequis

Avant d’installer le projet, assurez-vous d’avoir :

- Python 3.10 ou supérieur  
- pip  
- Git  

---

# Installation

Clonez le dépôt :

```bash
git clone https://github.com/sjbl69/Futurisys.git
cd Futurisys
```

Créer un environnement virtuel :

```bash
python -m venv .venv
```

Activer l’environnement :

Windows :

```bash
.venv\Scripts\activate
```

Installer les dépendances :

```bash
pip install -r requirements.txt
```

---

# Entraîner le modèle

Si le modèle n’est pas encore généré :

```bash
python ml_model/train_model.py
```

Cela crée :

```
ml_model/model.pkl
```

---

# Lancer l’API

Démarrer le serveur :

```bash
uvicorn app.main:app --reload
```

L’API sera disponible à l’adresse :

```
http://127.0.0.1:8000
```

---

# Fonctionnement de l’API

L'API expose un endpoint principal :

```
POST /predict
```

Ce endpoint :

1. reçoit les données d’entrée (features)  
2. appelle le modèle de machine learning  
3. génère une prédiction  
4. enregistre les données et la prédiction dans la base de données  
5. retourne la prédiction au client  

---

# Documentation de l’API

FastAPI génère automatiquement une documentation interactive.

Swagger UI :

```
http://127.0.0.1:8000/docs
```

---

# Architecture du système

Le système Futurisys repose sur une architecture simple permettant d’exposer un modèle de machine learning via une API REST.

Flux de fonctionnement :

Client  
↓  
FastAPI API  
↓  
Machine Learning Model  
↓  
Database  

1. Le client envoie une requête HTTP à l’API  
2. FastAPI reçoit les données et les valide  
3. Le modèle de machine learning génère une prédiction  
4. Les données et la prédiction sont enregistrées dans la base de données  
5. L’API retourne la réponse au client  

---

# Modèle de Machine Learning

Le projet utilise un modèle de classification entraîné avec la bibliothèque **Scikit-Learn**.

Le modèle est entraîné à partir d’un dataset contenant plusieurs variables d’entrée appelées *features*.

## Features utilisées

Le modèle utilise les variables suivantes :

- feature1  
- feature2  
- feature3  
- feature4  

Ces variables sont envoyées à l’API afin de générer une prédiction.

## Pipeline du modèle

Le pipeline de machine learning suit les étapes suivantes :

1. chargement du dataset  
2. préparation des données  
3. entraînement du modèle  
4. évaluation des performances  
5. sauvegarde du modèle  

Une fois entraîné, le modèle est sauvegardé dans le fichier :

```
ml_model/model.pkl
```

Ce fichier est chargé automatiquement par l’API lors du démarrage.

---

# Performance du modèle

Le modèle a été évalué sur un jeu de données de test afin de mesurer sa capacité de généralisation.

Les métriques utilisées sont les suivantes :

| Metric | Score |
|------|------|
| Accuracy | 0.92 |
| Precision | 0.90 |
| Recall | 0.88 |
| F1-score | 0.89 |

Ces métriques permettent d’évaluer la qualité du modèle et son efficacité pour effectuer des prédictions fiables.

---

# Exemple d'utilisation de l'API

Exemple de requête envoyée à l’API :

POST /predict

```json
{
 "features": [5.1, 3.5, 1.4, 0.2]
}
```

Réponse retournée par l’API :

```json
{
 "prediction": 1
}
```

Cet exemple montre comment envoyer des données d’entrée au modèle afin d’obtenir une prédiction.

---

# Base de données

Le projet utilise **PostgreSQL** pour enregistrer les prédictions générées par le modèle en production.

Chaque appel à l’endpoint `/predict` enregistre :

- les données d’entrée  
- la prédiction du modèle  
- la date de la requête  

## Configuration de la base de données

Le projet est conçu pour fonctionner avec **PostgreSQL en production**.

Cependant, pour faciliter le développement local, une base **SQLite** est utilisée par défaut si la variable d’environnement `DATABASE_URL` n’est pas définie.

Ainsi :

- PostgreSQL → environnement de production  
- SQLite → développement local  

## Modèle de données

La table `predictions` enregistre les entrées envoyées au modèle ainsi que la prédiction produite.

| Type | Colonne |
|-----|--------|
| int | id |
| float | feature1 |
| float | feature2 |
| float | feature3 |
| float | feature4 |
| float | prediction |
| datetime | created_at |

---

# Création de la base de données

Créer la base PostgreSQL :

```
CREATE DATABASE futurisys;
```

Configurer la variable d’environnement :

```
DATABASE_URL=postgresql://user:password@localhost/futurisys
```

Créer les tables :

```bash
python create_tables.py
```

---

# Tests

Afin de garantir la fiabilité et la robustesse de l’API, des tests unitaires et fonctionnels ont été mis en place à l’aide de **Pytest**.

Les tests permettent de vérifier :

- le bon fonctionnement de l’API FastAPI  
- le chargement du modèle de machine learning  
- le comportement de l’endpoint `/predict`  
- la gestion des erreurs et des cas limites  

Les fichiers de test sont situés dans :

```
tests/
```

### Lancer les tests

```bash
pytest
```

### Couverture de tests

Un rapport de couverture est généré avec :

```bash
pytest --cov=app --cov-report=html
```


Ce rapport est disponible dans :

```
htmlcov/index.html
```

Il permet de visualiser précisément quelles parties du code sont couvertes par les tests.

Les tests permettent de :

- vérifier la stabilité de l’API  
- détecter rapidement les régressions  
- garantir la qualité du code  
- automatiser la validation dans le pipeline CI/CD


---

# CI/CD

Le projet inclut un pipeline CI/CD avec **GitHub Actions** permettant :

- l’exécution automatique des tests  
- la validation du code à chaque push  
- la vérification de la stabilité de l’API  

---

# Déploiement

L’API Futurisys est conçue pour être déployée sur une plateforme cloud.

Dans ce projet, l’application est déployée sur **Render**, une plateforme permettant d’héberger facilement des services web basés sur Python.

## API déployée

L’API est déployée et accessible en ligne sur Render :

https://futurisys.onrender.com

### Accès à l’API

L’API peut être utilisée de plusieurs façons :

#### Accès direct

Une requête GET sur la racine de l’API permet de vérifier que le service fonctionne :

https://futurisys.onrender.com

Réponse attendue :

{
  "message": "Futurisys ML API",
  "environment": "production"
}

#### Documentation interactive

FastAPI génère automatiquement une documentation interactive Swagger :

https://futurisys.onrender.com/docs

Cette interface permet de tester les endpoints directement depuis le navigateur.

#### Exemple d'appel à l’API

Endpoint :

POST /predict

Exemple de requête :

{
  "features": [5.1, 3.5, 1.4, 0.2]
}

Réponse :

{
  "prediction": 0
}

---

## Variables d’environnement

L’API utilise certaines variables d’environnement pour sa configuration.

### DATABASE_URL

Cette variable contient l’URL de connexion à la base de données PostgreSQL utilisée pour stocker les prédictions.

Exemple :

DATABASE_URL=postgresql://user:password@host:port/database

### APP_ENV

Cette variable permet de définir l’environnement d’exécution de l’application.

Valeurs possibles :

- development (en local)
- production (sur Render)

Sur l’API déployée :

APP_ENV=production

## Redéploiement

Pour redéployer l’application :

1. modifier le code localement  
2. commit et push vers GitHub  
3. Render redéploie automatiquement la nouvelle version

# Sécurité et authentification

Actuellement, l’API Futurisys ne nécessite pas d’authentification pour accéder à l’endpoint `/predict`.

Dans un environnement de production réel, plusieurs mécanismes de sécurité pourraient être ajoutés :

- authentification via **API Key**
- authentification **JWT (JSON Web Token)**
- limitation du nombre de requêtes (**rate limiting**)
- journalisation des requêtes pour audit

Ces mécanismes permettent de :

- protéger l’API contre les accès non autorisés
- éviter les abus ou les attaques par déni de service
- garantir la traçabilité des appels au modèle de machine learning

# Technologies utilisées

- Python  
- FastAPI  
- Scikit-learn  
- Pytest  
- GitHub Actions  
- Render  

---

# Mise à jour du modèle

Afin de garantir la qualité des prédictions dans le temps, le modèle peut être réentraîné régulièrement avec de nouvelles données.

Processus recommandé :

1. collecter de nouvelles données  
2. mettre à jour le dataset  
3. réentraîner le modèle  
4. évaluer les performances  
5. sauvegarder le nouveau modèle  
6. redéployer l’API  

Cette procédure permet d’éviter la dégradation du modèle liée au **data drift**.

---


 
 



