---
id: Perso.projects.liste.copilot
title: Perso.projects.liste.copilot
desc: working with copilot
updated: 1767773421823
created: 0
---
# comment fonctionner avec copilot

Aussi souvent que possible demander une doc (format md). Ceci permettra de mitiger le code qui n'est pas forcément d'une top qualité.

## strategy

**documentation/feature_strategy.md (description de la stratégie d'implémentation d'une feature)** :

features documentation are defined in documentation/feature/<NAME_OF_THE_FEATURE>
feature folders will contains :
 - description.md : the feature description, defined by me.
 - plan.md : the implementation plan. copilot field. the plan should not get too much implementation details but keep to technical or functional constraints.
 - implementation.md : by copilot, will contains implementation details, specifically if important technical reword is needed

copilot must not starts its  implementation without a explicit order after planning step so I could rview and modify plan if needed

exemple : https://github.com/b3b00/liste/tree/feature/multi-list

### avantages : 
 - documentation gratuite ! 
 - sécurisation des dev : on peut éviter certaines mécompréhensions, mauvais choix technique, overengeenering....
 - la documentation viendra s'ajouter au contexte de copilot pour les développement ultérieurs
