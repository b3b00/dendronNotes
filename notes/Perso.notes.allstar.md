---
id: Perso.notes.allstar
title: Perso.notes.allstar
desc: ALL*
updated: 1765965811805
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

## 6. Le DFA de lookahead dans ALL(*)

### 6.1 Qu’est-ce que le DFA en ALL(*) ?

Dans ALL(*), un **DFA de lookahead** (*Lookahead Deterministic Finite Automaton*) est une **structure de mémorisation des décisions de parsing**.

👉 Il ne reconnaît pas le langage, mais **prédit quelle production choisir** à un point de décision donné.

Caractéristiques essentielles :

* Un **DFA par point de décision** (non-terminal avec plusieurs productions)
* Les **états du DFA représentent des ensembles de configurations ATN**
* Les **transitions sont étiquetées par des terminaux** (tokens)
* Les **états finaux correspondent à une production unique**

En résumé :

> **Le DFA encode la connaissance acquise dynamiquement sur les séquences d’entrée déjà rencontrées.**

---

### 6.2 États du DFA : configurations ATN

Un état du DFA est un ensemble de **configurations ATN** de la forme :

```text
(p, i, Γ)
```

Où :

* `p` : état courant dans l’ATN
* `i` : numéro de la production candidate
* `Γ` : pile d’appels (ou # en mode SLL)

➡️ Chaque état du DFA représente **toutes les situations possibles du parseur** après avoir lu un certain préfixe d’entrée.

---

### 6.3 Construction du DFA (à la volée)

Le DFA n’est **jamais construit entièrement à l’avance**.
Il est **étendu dynamiquement** uniquement quand une séquence inconnue apparaît.

#### Étape 1 : état initial D₀

Pour une règle `A → α₁ | α₂ | ... | αₙ` :

* On crée un état initial `D₀`
* `D₀` contient les configurations correspondant **au début de chaque production**
* On applique la **closure ε** (transitions sans consommer de token)

---

#### Étape 2 : transition sur un token

À partir d’un état DFA `D` et d’un token `t` :

1. On applique `move(D, t)`

   * avance les configurations ATN consommant `t`
2. On applique `closure` sur le résultat
3. On obtient un nouvel état `D'`

Cas possibles :

* `D'` est vide → **erreur syntaxique**
* Toutes les configurations de `D'` prédisent la même production → **état final**
* Sinon → **état intermédiaire**, potentiellement conflictuel

---

### 6.4 États finaux du DFA

Un état DFA est **final** si :

```text
{ i | (–, i, –) ∈ D } = { k }
```

➡️ Toutes les configurations pointent vers **la même production `k`**.

Dans ce cas :

* Le DFA retourne immédiatement `k`
* La prédiction est terminée

---

### 6.5 Gestion des conflits

Un état DFA est dit **conflictuel** si :

* Plusieurs productions sont encore possibles
* Et qu’aucune n’est clairement viable sans pile

Deux cas :

| Situation                   | Action                                  |
| --------------------------- | --------------------------------------- |
| Mode SLL, conflit           | Fallback en LL                          |
| Mode LL, conflit persistant | Ambiguïté (priorité à la prod minimale) |

Les états conflictueux sont **marqués comme stack-sensitive**.

---

### 6.6 Utilisation du DFA pendant le parsing

Lors d’une prédiction ultérieure :

1. Le parseur rejoue les tokens dans le DFA
2. S’il atteint un état final → décision immédiate (O(1) par token)
3. S’il manque une transition → extension dynamique du DFA

➡️ Plus le parser avance, plus le DFA devient complet et rapide.

---

## 7. Schéma logique de ALL(*)

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

