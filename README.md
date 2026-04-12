# AthleteAI — Analyse de 2 ans de données d'entraînement

> **Projet Data Science end-to-end** — de la donnée brute capteur jusqu'au modèle déployé.  
> Données réelles issues de ma montre GPS **Coros Pace 3** — 313 séances, 2 ans d'entraînement demi-fond.

---

## Objectif du projet

Ce projet applique une démarche Data Science complète sur des données d'entraînement sportif personnelles et réelles :

- **Comprendre** mes performances et ma charge d'entraînement sur 2 ans
- **Prédire** mes performances futures à partir de mes données historiques
- **Détecter** les semaines à risque de surmenage avant qu'elles ne surviennent
- **Classifier** automatiquement mes types de séances sans étiquetage manuel
- **Interagir** avec mes données en langage naturel grâce à un coach IA (RAG + LLM)

---

## Dataset

| Indicateur | Valeur |
|---|---|
| Source | Fichiers `.fit` exportés depuis Coros Pace 3 |
| Période | 2 ans |
| Nombre de séances | 313 séances valides |
| Fréquence d'échantillonnage | 1 point par seconde |
| Variables disponibles | `timestamp`, `distance`, `heart_rate`, `speed`, `cadence`, `activity_type` |

> Les fichiers `.fit` bruts ne sont pas versionnés (données personnelles). Seules les données agrégées par séance sont disponibles dans `data/processed/`.

---

## Architecture du projet

```
AthleteAI/
├── data/
│   ├── raw/                    # Fichiers .fit bruts (non versionnés)
│   └── processed/              # Données nettoyées et features engineerées
│       ├── master_sessions.csv # Toutes les séances brutes agrégées
│       ├── sessions_clean.csv  # Séances nettoyées (313 séances)
│       ├── daily_features.csv  # Features journalières (CTL, ATL, TSB)
│       └── weekly_features.csv # Features hebdomadaires (charge, monotonie)
├── notebooks/
│   ├── 01_parsing_fit.ipynb    # Extraction des fichiers .fit
│   ├── 02_EDA.ipynb            # Analyse exploratoire complète
│   ├── 03_feature_engineering.ipynb  # CTL, ATL, TSB, TRIMP, monotonie
│   ├── 04_machine_learning.ipynb     # Prédiction, clustering, anomalie
│   └── 05_coach_IA.ipynb             # RAG + LangChain
├── models/                     # Modèles ML sauvegardés (.pkl)
├── api/                        # API FastAPI
├── dashboard/                  # Dashboard Streamlit
├── requirements.txt
└── README.md
```

---

## Méthodologie — 6 étapes

### 1. Parsing des fichiers `.fit`
Extraction des données brutes depuis les fichiers propriétaires Coros avec la librairie `fitparse`. Chaque fichier représente une séance et contient des milliers de points GPS/cardiaque.

### 2. Analyse Exploratoire (EDA)
Nettoyage, typage, agrégation par séance. Visualisation des distributions (distance, FC, vitesse), de la saisonnalité et des corrélations entre variables.

### 3. Feature Engineering
Calcul des métriques de charge d'entraînement utilisées en sport de haut niveau :
- **TRIMP** (Training Impulse) — charge réelle de chaque séance
- **CTL** (Chronic Training Load) — forme de fond sur 42 jours
- **ATL** (Acute Training Load) — fatigue sur 7 jours
- **TSB** (Training Stress Balance) — forme du jour = CTL − ATL
- **Monotonie** — indicateur de risque de surmenage

### 4. Machine Learning
Trois modèles complémentaires :
- **Random Forest** — prédiction de la performance (allure au km)
- **K-Means** — clustering automatique des types de séances
- **Isolation Forest** — détection des séances anormales / surmenage

### 5. Coach IA (RAG + LangChain)
Système de question-réponse en langage naturel sur mes propres données d'entraînement. Exemple : *"Pourquoi ma FC est-elle plus élevée ces 3 dernières semaines ?"*

### 6. Déploiement
- **API FastAPI** exposant les modèles ML
- **Dashboard Streamlit** interactif
- **GitHub Actions** pour l'intégration continue

---

## Résultats clés

| Métrique | Valeur |
|---|---|
| Distance moyenne par séance | 8,6 km |
| FC moyenne globale | 154 bpm |
| Allure moyenne | ~4'40" /km |
| Cadence moyenne | 81 pas/min |
| Séances détectées à risque | *à compléter après ML* |
| Score R² prédiction performance | *à compléter après ML* |

---

## Stack technique

| Catégorie | Outils |
|---|---|
| Langage | Python 3.14 |
| Manipulation des données | Pandas, NumPy |
| Visualisation | Matplotlib, Seaborn |
| Machine Learning | Scikit-learn |
| IA Générative | LangChain, FAISS |
| API | FastAPI |
| Dashboard | Streamlit |
| Versioning | Git, GitHub |
| Environnement cloud | Google Cloud Platform (BigQuery) |

---

## Installation et utilisation

```bash
# Cloner le projet
git clone https://github.com/hichaavm/AthleteAI.git
cd AthleteAI

# Créer l'environnement virtuel
python -m venv venv
source venv/bin/activate  # Mac/Linux

# Installer les dépendances
pip install -r requirements.txt

# Lancer Jupyter
jupyter notebook
```

---

## Auteur

**Hicham Bouhraznine**  
Alternant Data Analyst — Auchan Retail France  
Master Digital & Data — INSEEC Paris (2026–2028)  
[LinkedIn](https://www.linkedin.com/in/hicbouh) · [Kaggle](https://www.kaggle.com/hichambouhraznine) · [GitHub](https://github.com/hichaavm)

---

## Contexte

Ce projet a été développé dans le cadre de ma montée en compétences Data Science, en parallèle de mon alternance en tant que Manager Commerce chez **Auchan Hypermarché Manosque**. Il illustre ma capacité à mener un projet data end-to-end — de l'extraction de données brutes jusqu'au déploiement d'une solution exploitable — sur des données réelles et personnelles.