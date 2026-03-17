# 🎓 Assistant Virtuel — Inscriptions Université de Djibouti

> Chatbot FAQ multilingue pour les nouveaux bacheliers de l'Université de Djibouti  
> Projet tutoré — Licence Informatique

[![Rasa](https://img.shields.io/badge/Rasa-3.6.21-5C4EE5?style=flat&logo=rasa)](https://rasa.com)
[![Python](https://img.shields.io/badge/Python-3.9.13-3776AB?style=flat&logo=python)](https://python.org)
[![Groq](https://img.shields.io/badge/LLM-Groq%20%2F%20Ollama-orange?style=flat)](https://groq.com)
[![Langues](https://img.shields.io/badge/Langues-5%20(FR%20%7C%20AR%20%7C%20SO%20%7C%20EN%20%7C%20AA)-green?style=flat)](https://github.com)

---

## 📋 Table des matières

1. [Présentation](#présentation)
2. [Fonctionnalités](#fonctionnalités)
3. [Architecture](#architecture)
4. [Technologies utilisées](#technologies-utilisées)
5. [Installation](#installation)
6. [Lancement](#lancement)
7. [Déploiement en ligne](#déploiement-en-ligne)
8. [Structure du projet](#structure-du-projet)
9. [Langues supportées](#langues-supportées)
10. [Dashboard admin](#dashboard-admin)

---

## 🎯 Présentation

L'**Assistant Virtuel UD** est un chatbot intelligent développé pour aider les nouveaux bacheliers de l'Université de Djibouti à obtenir des réponses instantanées sur les inscriptions universitaires.

Il répond aux questions concernant :
- La procédure d'inscription étape par étape
- Les documents requis
- Les frais d'inscription
- Les facultés et formations disponibles
- La plateforme eCampus
- Les contacts et horaires
- Le logement étudiant
- Les bourses disponibles

---

## ✨ Fonctionnalités

| Fonctionnalité | Description |
|---|---|
| 🌍 **Multilingue** | Français, Arabe, Somali, Anglais, Afar |
| 🤖 **LLM hybride** | Groq (cloud rapide) + Ollama (fallback local) |
| 📊 **Dashboard admin** | Statistiques, graphiques, export CSV |
| 💬 **Markdown riche** | Listes, tableaux, liens cliquables |
| 🔄 **Suggestions dynamiques** | Chips contextuelles après chaque réponse |
| 👍 **Système de feedback** | Évaluation 👍/👎 de chaque réponse |
| 🌐 **Accessible en ligne** | Via ngrok + GitHub Pages |
| 📱 **Responsive** | Fonctionne sur mobile et desktop |
| 🔍 **Scraping automatique** | Mise à jour depuis le site officiel UD |
| 💾 **Logging** | Enregistrement de toutes les conversations |

---

## 🏗️ Architecture

```
Utilisateur (téléphone/PC)
        │
        ▼
GitHub Pages (index.html)
        │  HTTPS
        ▼
   ngrok tunnel
        │
        ▼
Rasa Server :5005  ──────►  Rasa Actions :5055
                                    │
              ┌─────────────────────┼──────────────────────┐
              ▼                     ▼                      ▼
    knowledge_base.json     Site officiel UD        Groq API / Ollama
                             (BeautifulSoup)        (LLM multilingue)
                                    │
                                    ▼
                           questions_log.json
                                    │
                                    ▼
                            Dashboard admin
```

---

## 🛠️ Technologies utilisées

| Composant | Technologie | Version |
|---|---|---|
| Framework NLU | Rasa Open Source | 3.6.21 |
| Langage | Python | 3.9.13 |
| LLM Cloud | Groq API (llama-3.3-70b) | - |
| LLM Local | Ollama (llama3.2:3b) | - |
| Scraping | BeautifulSoup4 | 4.14.3 |
| Frontend | HTML / CSS / JavaScript | - |
| Tunnel | ngrok | 3.x |
| Hébergement | GitHub Pages | - |

---

## ⚙️ Installation

### Prérequis

- Python 3.9.13
- Node.js (optionnel)
- Ollama installé (optionnel, pour le mode hors-ligne)
- Clé API Groq gratuite : [console.groq.com](https://console.groq.com)

### Étapes

```powershell
# 1. Cloner le projet
git clone https://github.com/TON_USERNAME/chatbot-ud.git
cd chatbot-ud

# 2. Créer l'environnement virtuel
cd backend
python -m venv env
env\Scripts\activate

# 3. Installer les dépendances
pip install rasa==3.6.21
pip install beautifulsoup4 requests schedule
pip install packaging==20.9 pydantic==1.10.9

# 4. Configurer la clé Groq
# Ouvrir backend/actions/actions.py
# Remplacer GROQ_API_KEY = "VOTRE_CLE_ICI"

# 5. Entraîner le modèle
rasa train
```

---

## 🚀 Lancement

### Option 1 — Démarrage automatique (recommandé)

```powershell
# Double-clic sur start.bat à la racine du projet
start.bat
```

### Option 2 — Démarrage manuel (4 terminaux)

```powershell
# Terminal 1 — Serveur d'actions
cd backend && env\Scripts\activate && rasa run actions

# Terminal 2 — Serveur Rasa
rasa run --enable-api --cors "*" --request-timeout 120

# Terminal 3 — Serveur dashboard
cd backend\data && python -m http.server 8080

# Terminal 4 — Mise à jour base de connaissances (optionnel)
cd backend && python scripts\update_knowledge.py
```

### Accès local

- **Chatbot** : `file:///C:/CHATBOT-INSCRIPTION/frontend/index.html`
- **Dashboard** : `file:///C:/CHATBOT-INSCRIPTION/frontend/dashboard.html`

---

## 🌐 Déploiement en ligne

Le chatbot est accessible depuis n'importe quel appareil via ngrok + GitHub Pages.

### 1. Lancer le tunnel ngrok

```powershell
cd C:\ngrok
.\ngrok http --domain=conceptual-dennis-dashingly.ngrok-free.dev 5005
```

### 2. Accès public

| Interface | URL |
|---|---|
| **Chatbot** | `https://TON_USERNAME.github.io/chatbot-ud` |
| **API Rasa** | `https://conceptual-dennis-dashingly.ngrok-free.dev` |

> **Note** : Le PC doit rester allumé avec ngrok actif pour que le chatbot soit accessible.

---

## 📁 Structure du projet

```
CHATBOT-INSCRIPTION/
│
├── start.bat                          # Démarrage en 1 clic
│
├── frontend/
│   ├── index.html                     # Interface chatbot
│   └── dashboard.html                 # Dashboard admin
│
└── backend/
    ├── config.yml                     # Pipeline NLU Rasa
    ├── domain.yml                     # Intents, actions, réponses
    ├── endpoints.yml                  # Configuration serveurs
    │
    ├── actions/
    │   └── actions.py                 # Actions custom + LLM + multilingue
    │
    ├── data/
    │   ├── nlu.yml                    # Exemples d'entraînement (5 langues)
    │   ├── stories.yml                # Scénarios de conversation
    │   ├── rules.yml                  # Règles de dialogue
    │   ├── knowledge_base.json        # Base de connaissances UD
    │   └── questions_log.json         # Logs des conversations (auto-généré)
    │
    └── scripts/
        ├── update_knowledge.py        # Scraping auto toutes les 24h
        └── analyse_logs.py            # Analyse des logs en terminal
```

---

## 🌍 Langues supportées

| Langue | Code | Détection | Réponses statiques | Réponses LLM |
|---|---|---|---|---|
| Français | `fr` | ✅ | ✅ | ✅ |
| Arabe | `ar` | ✅ Unicode | ✅ | ✅ |
| Somali | `so` | ✅ Score | ✅ | ✅ |
| Anglais | `en` | ✅ Score | ✅ | ✅ |
| Afar | `aa` | ✅ Score | ✅ | ✅ |

La détection de langue utilise un **système de score pondéré** avec mémoire de session — si un utilisateur commence en somali, le bot continue en somali même pour les messages courts ou ambigus.

---

## 📊 Dashboard admin

Le dashboard est accessible sur `http://localhost:8080` (ou via GitHub Pages).

Il affiche en temps réel :
- **Nombre total** de questions posées
- **Taux de satisfaction** (👍/👎)
- **Taux de fallback** (questions non reconnues)
- **Langue principale** des utilisateurs
- **Top 8 intents** les plus demandés
- **Répartition des langues** (donut chart)
- **Activité des 7 derniers jours**
- **50 dernières conversations** avec filtre

Export CSV disponible en un clic.

---

## 👨‍💻 Auteur

Projet tutoré — Université de Djibouti  
Licence Informatique — 2024/2025

---

## 📄 Licence

Projet académique — Université de Djibouti  
Usage éducatif uniquement.
