---
id: 6j1uw54m05643vm341fjpxr
title: Dégradation temps de mise à jour PPv2
desc: ''
updated: 1665490328570
created: 1665487778425
---
# Dégradation temps de mise à jour PPv2

## Méthodologie

A partir d'un plan de paie PPv2 de base (du 07/10/2022) on génére 2 nouvelles versions avec respectivement 100 et 200 rubriques supplémentaires. 
Les rubriques additionnelles sont générées par clonage d'une rubrique puis en modifiant le GUID et le code de la nouvelle rubrique pour éviter toute "collision" de rubrique.

Un dossier ppv2 en version antérieure à la version de base est restauré sur la version SBCP locale. 
On fait pointer SBCP vers la version de modèle souhaitée (base , +100, +200 ). et on ouvre le dossier.
Le code SBCP a été très légérement instrumenté (sans perturber la mesure) pour effectuer une sortie de la mesure dans un fichier texte

On répéte plusieurs fois la mesure pour chacune des versions en restaurant le dossier de référence entre chaque mesure. 


### biais connus
   - le poste de dev ne peut etre représentatif d'un environnement azure ou LBN (notamment les temps d'accès aux json PPv2)
   - la version de départ du dossier de test est très ancienne (05/2021)
   - les rubriques ajoutées ne sont pas représentatives de la réalité car constituées par clonage.


## résultats (moyenne)

Les mesures sont extrèmement sensibles à la charge CPU, par exemple une conversation teams a dégradé une mesure de la version de base par un facteur 3 !
Ces chiffres sont donc à prendre avec les plus grandes précautions ce que confirme l'écart type des mesures pour un même cas de mise à jour.


| cas            |  mesures |     |      |      |     | moyenne | ecart type| 
| -------------- | ---- | ---- | ---- | ---- | ------ | ------- | --------- |
| base           | 2781 | 3806 | 5521 | 5118 |        | 4306,5  |  1253     | 
| +100 rubriques | 3783 | 4146 | 4797 | 3934 | 3042   | 3940,4  |  634      | 
| +200 rubriques | 3923 | 4255 | 5760 | 4754 | 5750   | 4888,4  |  845      | 



##





| cas          | version PP       | moyenne | dégradation | nombre de rubriques | delta rubriques |
| ------------ | ---------------- | ------- | ----------- | ------------------- | --------------- |
| base         | 10.3.20221007.00 | 4306,5  |             | 2748                |                 |
|+100 rubriques| 10.3.20221007.01 | 3940,4  | -8,50%      | 2848                | +3,64%          |
|+200 rubriques| 10.3.20221007.02 | 4888,4  | +13,51%     | 2948                | +7,28%          |


## Conclusion

La trop grande variance des mesures de temps de mise à jour ne permet pas d'établir de manière sûre toute corrélation entre le nombre de rubriques et les temps de mise à jour PPv2.

