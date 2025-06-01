# Agent LLM Renault - Cas Pratique

Ce projet a été développé en réponse à un cas pratique d'entretien technique. L'objectif principal est la création d'un agent LLM capable d'interroger une base de connaissances multi-sources (documents Renault, données boursières) pour répondre à des questions spécifiques concernant le groupe Renault, son plan stratégique "Renaulution", ses performances financières et ses indicateurs DPEF.

## Description du Projet

L'agent mis en œuvre utilise une architecture RAG (Retrieval-Augmented Generation). Il s'appuie sur :
* Des documents publics de Renault (Documents d'Enregistrement Universel - DEU, transcriptions de conférences financières, présentations de plans stratégiques).
* Des données boursières récupérées via l'API Yahoo Finance (`yfinance`).
* Des outils spécialisés développés en Python avec LangChain pour interagir avec ces données (recherche sémantique, récupération de cours, génération de graphiques, analyse de corrélation).
* Un modèle de langage (LLM) hébergé sur Hugging Face (Mixtral-8x7B-Instruct-v0.1) pour le raisonnement et la génération des réponses.

## Structure du Notebook

Le projet est intégralement contenu dans le notebook Jupyter `Renault_Agent_GenAI_Solution.ipynb`. Son organisation est la suivante :

* **Partie 0 :** Configuration Globale, Paramètres de Débogage et initialisation du chargement des Variables d'Environnement (notamment la clé API Hugging Face via un fichier `.env`).
* **Partie 1 :** Installation des Dépendances Python et Importations des bibliothèques.
* **Partie 2 :** Acquisition Automatisée des Données (téléchargement des PDFs depuis les URLs fournies, extraction des transcriptions YouTube, récupération des données boursières).
* **Partie 3 :** Traitement des Documents et Création de la Base Vectorielle RAG (chargement des contenus textuels, découpage en chunks, génération des embeddings, et création/sauvegarde ou chargement d'un index FAISS).
* **Partie 4 :** Création des Outils (Tools) Spécifiques pour l'Agent (définition des fonctions LangChain `@tool` pour interagir avec la base de connaissances et les données boursières).
* **Partie 5 :** Création et Utilisation de l'Agent LLM (configuration du LLM, assemblage de l'agent ReAct, et exécution des tests sur les 9 questions de l'entretien avec affichage des réponses).
* **Partie 6 :** Comparaison et Évaluation des Réponses de l'Agent (comparaison des réponses de l'agent avec un jeu de réponses attendues simplifiées, directement intégrées dans le notebook).

## Prérequis et Configuration

* **Environnement d'Exécution :** Conçu et testé pour Google Colab.
* **Dépendances Python :** Toutes les bibliothèques nécessaires sont installées au début du notebook (Partie 1) via des commandes `!pip install`. La bibliothèque `python-dotenv` est utilisée pour la gestion des variables d'environnement.
* **Clé API Hugging Face :** Une clé API Hugging Face valide est **indispensable** pour interroger le modèle LLM.
    * **Méthode d'installation recommandée :**
        1.  Créez un fichier nommé `.env` à la racine de votre environnement de travail Colab (au même niveau que le notebook).
        2.  Dans ce fichier `.env`, ajoutez votre token sous la forme suivante :
            ```
            HUGGINGFACEHUB_API_TOKEN="hf_VOTRE_TOKEN_PERSONNEL_ICI"
            ```
        3.  Le notebook (en Partie 0) chargera automatiquement ce token dans les variables d'environnement au démarrage.
    * **Important :** N'incluez jamais votre fichier `.env` contenant des clés secrètes dans un dépôt Git. Ajoutez `.env` à votre fichier `.gitignore`.
* **Paramètres de Débogage :** En Partie 0 du notebook, vous pouvez ajuster les variables `DEBUG_SHOW_CHUNK_DETAILS` (pour voir le détail des chunks RAG) et `SHOW_AGENT_THOUGHTS` (pour voir le raisonnement interne de l'agent) pour une analyse plus fine.

## Instructions d'Exécution

1.  Ouvrez le fichier `Renault_Agent_GenAI_Solution.ipynb` dans Google Colab.
2.  **Configuration Initiale :** Assurez-vous que votre fichier `.env` est présent à la racine de l'environnement Colab et contient votre `HUGGINGFACEHUB_API_TOKEN` valide.
3.  Exécutez les cellules du notebook séquentiellement, de haut en bas.
    * La Partie 2 téléchargera les documents nécessaires.
    * La première exécution de la Partie 3 (création de l'index FAISS) peut prendre plusieurs minutes (10-30+ min en fonction des ressources Colab). Les exécutions suivantes seront beaucoup plus rapides grâce au chargement de l'index sauvegardé.
4.  Les réponses de l'agent aux questions de l'entretien seront affichées en Partie 5, et la comparaison avec les réponses attendues en Partie 6.

## Sorties Attendues

* Affichage formaté des questions et des réponses de l'agent en Partie 5.
* Génération de fichiers graphiques (ex: `ventes_vehicules_par_an.png`, `renault_vs_cac40_annonces.png`) dans le répertoire Colab pour les questions correspondantes. L'agent confirmera leur création.
* Affichage de la comparaison entre les réponses de l'agent et les réponses de référence en Partie 6.

## Auteur

Samy BEN LEMRID
