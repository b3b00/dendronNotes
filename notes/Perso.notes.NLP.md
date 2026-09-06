---
id: Perso.notes.NLP
title: Perso.notes.NLP
desc: NLP
updated: 0
created: 0
---
# Présentation : Concepts de base du NLP Classique

Ce document récapitule les notions fondamentales du Traitement Automatique du Langage Naturel (NLP) classique, basé sur des approches symboliques et statistiques.

---

## 1. Le Précurseur : Le Nettoyage du Texte (Preprocessing)

Avant toute analyse, le texte brut doit être normalisé et nettoyé.

- **Tokenisation** : Découpage du texte en unités de base (*tokens*), généralement des mots ou des signes de ponctuation.
  - *Exemple* : `"J'aime le NLP !"` $\rightarrow$ `["J'", "aime", "le", "NLP", "!"]`
- **Nettoyage (Stopwords & Ponctuation)** : Suppression des mots très fréquents qui n'apportent que peu de valeur sémantique (*mots vides* / *stopwords* comme *le, la, de, et*).
- **Racinisation (*Stemming*) vs Lemmatisation** :
  - **Stemming** : Coupe brutalement la fin des mots pour ne garder que la racine (approche heuristique).
    - *Exemple* : `"mangions"`, `"mangerai"` $\rightarrow$ `"mang"`
  - **Lemmatisation** : Réduit le mot à sa forme canonique du dictionnaire (*lemme*) en s'appuyant sur sa catégorie grammaticale.
    - *Exemple* : `"mangions"` $\rightarrow$ `"manger"`

---

## 2. La Vectorisation : Transformer le Texte en Nombres

Les algorithmes d'apprentissage automatique nécessitent des entrées numériques. Le NLP classique s'appuie sur des méthodes d'extraction de caractéristiques (*feature extraction*).

### Bag-of-Words (BoW)
Un vocabulaire global est créé à partir de tous les mots uniques du corpus. Chaque document est ensuite représenté par un vecteur comptant la fréquence de chaque mot.
> **Limite** : L'ordre des mots et la syntaxe sont totalement perdus. Par exemple, `"Le chat mange la souris"` et `"La souris mange le chat"` partagent exactement la même représentation vectorielle.

### N-Grams
Permet de préserver du contexte local en regroupant les mots contigus par paquets de $N$.
- **Unigramme** ($N=1$) : `["Traitement", "automatique"]`
- **Bigramme** ($N=2$) : `["Traitement automatique"]`
- **Trigramme** ($N=3$) : `["Traitement automatique du"]`

### TF-IDF (*Term Frequency - Inverse Document Frequency*)
Évolution majeure du modèle Bag-of-Words, le TF-IDF évalue l'importance relative d'un mot au sein d'un document donné par rapport à l'ensemble du corpus.

$$\text{TF-IDF}(t, d, D) = \text{TF}(t, d) \times \text{IDF}(t, D)$$

- **TF (Term Frequency)** : Fréquence du mot $t$ dans le document $d$.
- **IDF (Inverse Document Frequency)** : Mesure de la rareté du mot à l'échelle du corpus $D$. Si un mot apparaît dans l'intégralité des documents, son IDF tend vers 0.
- **Résultat** : Les mots spécifiques et représentatifs d'un document reçoivent un score élevé, tandis que les mots génériques ou omniprésents reçoivent un score faible.

---

## 3. Les Tâches d'Analyse Linguistique

Après la préparation du texte, différentes analyses syntaxiques et morphologiques peuvent être appliquées :

- **POS Tagging (*Part-of-Speech Tagging*)** : Étiquetage grammatical attribuant à chaque mot sa nature (Nom, Verbe, Adjectif, etc.).
- **NER (*Named Entity Recognition*)** : Repérage et classification des entités nommées (Personnes, Lieux, Organisations, Dates).
- **Analyse syntaxique (*Parsing*)** : Génération d'un arbre syntaxique explicitant les relations de dépendance structurelle entre les mots.

---

## 4. Comparatif : NLP Classique vs NLP Moderne

| Critère | NLP Classique | NLP Moderne (Transformers / LLMs) |
| :--- | :--- | :--- |
| **Représentation** | Vecteurs creux (*Sparse*) très grands (TF-IDF, BoW) | Plongements denses (*Dense Embeddings*) type Word2Vec, BERT, LLMs |
| **Contexte** | Limité aux $N$-grammes locaux | Prise en compte du contexte global via le mécanisme d'attention |
| **Volume de données** | Efficace sur de petits jeux de données | Nécessite d'imposants volumes de données pour le pré-entraînement |
| **Interprétabilité** | Élevée (les caractéristiques/mots explicites sont directement lisibles) | Plus complexe (modèles paramétriques dits "boîte noire") |

