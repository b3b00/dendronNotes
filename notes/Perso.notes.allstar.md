---
id: Perso.notes.allstar
title: Perso.notes.allstar
desc: All*
updated: 0
created: 0
---
# Synthèse de l’algorithme **ALL(*) (Adaptive LL*)**

## 1. Objectif

**ALL(*)** est un algorithme de *parsing syntaxique top‑down adaptatif* utilisé par **ANTLR 4**.
Il permet de reconnaître **toute grammaire hors récursion gauche**, sans analyse grammaticale statique préalable.

Son objectif principal est de :

* conserver la **simplicité et la prédictibilité du LL**,
* tout en atteignant la **puissance d’un parseur général (GLR/GLL)**,
* avec de **bonnes performances en pratique**.

---

## 2. Problème traité

Les stratégies de parsing classiques présentent plusieurs limites :

| Algorithme | Limites principales                       |
| ---------- | ----------------------------------------- |
| LL(k)      | Lookahead fixe, grammaires contraignantes |
| LL(*)      | Analyse statique parfois indécidable      |
| PEG        | Choix glouton, ambiguïtés silencieuses    |
| GLR / GLL  | Lents, mémoire élevée, forêts d’arbres    |

➡️ **ALL(*)** contourne ces limites en déplaçant **l’analyse grammaticale au moment du parsing**.

---

## 3. Idées clés de ALL(*)

### 3.1 Analyse dynamique

* Aucune analyse complète de la grammaire à la génération
* L’algorithme n’analyse **que les séquences réellement rencontrées**

### 3.2 Simulation parallèle des alternatives

* À chaque point de décision, une **sous‑analyse par production possible**
* Les chemins invalides sont éliminés progressivement

### 3.3 Lookahead minimal

* Le parsing s’arrête dès qu’une **alternative devient unique**
* Pas de lookahead inutilement long

### 3.4 Mémorisation (DFA adaptatifs)

* Les décisions déjà prises sont stockées dans des **DFA de lookahead**
* Les prochaines occurrences sont résolues rapidement

### 3.5 Fallback intelligent

* Deux modes :

  * **SLL** (rapide, sans pile d’appels)
  * **LL** (complet, sensible à la pile)
* Passage automatique en LL uniquement en cas de conflit

---

## 4. Fonctionnement global

À chaque point de décision (non‑terminal avec plusieurs productions) :

1. Tentative de prédiction via le **DFA existant**
2. Si la décision est inconnue :

   * simulation dynamique de l’**ATN** (automate de la grammaire)
3. Les alternatives incompatibles sont éliminées
4. Si une seule alternative subsiste → choix immédiat
5. En cas de conflit :

   * fallback en LL (prise en compte de la pile)
   * ou résolution par priorité

---

## 5. Pseudo‑code simplifié

### 5.1 Prédiction adaptative (SLL)

```pseudo
function adaptivePredict(rule A, parser_stack γ):
    if DFA[A] existe:
        result ← DFA[A].simulate(input)
        if result trouvé:
            return result

    configs ← startConfigurations(A, γ)

    while true:
        if toutes configs prédisent la même production:
            mémoriser dans DFA
            return production

        if conflit détecté:
            return adaptivePredict_LL(A, γ)

        symbol ← input.peek()
        configs ← avancer_ATN(configs, symbol)
```

---

### 5.2 Fallback LL (avec pile)

```pseudo
function adaptivePredict_LL(A, γ):
    configs ← startConfigurations(A, γ)

    while true:
        if prédiction unique:
            return production

        if conflit persistant:
            signaler ambiguïté
            return production_prioritaire

        avancer avec la pile γ
```

---

## 6. Schéma logique de ALL(*)

```text
          ┌─────────────────────┐
          │ Point de décision A │
          └──────────┬──────────┘
                     │
           ┌─────────▼─────────┐
           │ DFA existant ?     │
           └──────┬─────┬──────┘
                  │oui  │non
                  ▼     ▼
          ┌──────────┐  ┌────────────────────┐
          │ Prédire  │  │ Simulation ATN     │
          │ via DFA  │  │ (alternatives)     │
          └────┬─────┘  └─────────┬──────────┘
               │                  │
               ▼                  ▼
        ┌─────────────┐    ┌─────────────────┐
        │ Succès ?    │    │ Alternatives    │
        └────┬────────┘    │ éliminées       │
             │             └──────┬──────────┘
             ▼                    ▼
      Production choisie   ┌───────────────┐
                            │ Unique ?      │
                            └────┬──────────┘
                                 │
                 ┌───────────────┴───────────────┐
                 ▼                               ▼
        Enregistrer DFA                Fallback LL / conflit
```

---

## 7. Résumé en une phrase

> **ALL(*) est un algorithme de parsing adaptatif qui combine LL et GLR en effectuant une analyse grammaticale dynamique, mémorisée et optimisée par DFA, garantissant puissance et performances en pratique.**

