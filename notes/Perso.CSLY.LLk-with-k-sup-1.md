---
id: eafo36iywegvpr8pq9p7vd4
title: LL(k) where k > 1
desc: 'greater lookahead if grammar is LL(k) with k > 1'
updated: 1660139784094
created: 1660120066934
---



## motivation

For now CSLY is LL(1) which mean it requires backtracking if needed prediction is > 1.

## LL(k) detection

detect if the grammar is LL(k) with k > 1

## Implement LL(k) in CSLY parsing algorythm

If LL(k) detection returns k > 1, use k lookahead tokens to predict next production rule.



