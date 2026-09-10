# AI Business Assistant — Atelier Prompt Engineering

Ce dépôt présente la réalisation de l'atelier Prompt Engineering : 
construction, test et évaluation de prompts pour un assistant IA métier.

## Partie 1 — Anatomie d'un prompt

**Objectif :** analyser les retours clients d'une entreprise.

### Prompt construit

*Rôle : Tu es un analyste customer insights expérimenté.

Contexte : Une entreprise de e-commerce reçoit régulièrement des avis clients
suite à ses livraisons et son service client.

Tâche : Analyse les avis clients ci-dessous et identifie les tendances 
principales (points positifs récurrents, problèmes récurrents).

Données d'entrée :
"""
1. "Livraison rapide mais l'emballage était abîmé."
2. "Service client très réactif, problème résolu en 10 minutes."
3. "J'ai attendu 3 jours de plus que prévu, aucune communication."
4. "Produit conforme à la description, très satisfait."
5. "Application qui plante souvent au moment du paiement."
"""

Contraintes : Base-toi uniquement sur les avis fournis, ne pas inventer 
d'informations non mentionnées, rester factuel.

Format de sortie : 
- Liste des points positifs récurrents
- Liste des problèmes récurrents
- Une recommandation générale en une phrase*


### Réponse obtenue

![Réponse Partie 1](images/p1_anatomie_prompt.png)


### Analyse

Le LLM a respecté l'ensemble des contraintes du prompt : chaque point cité 
(positif ou négatif) est directement rattaché à un avis précis, sans 
information inventée. Le format demandé (points positifs / problèmes / 
recommandation) est bien suivi. Point notable : le LLM a spontanément 
ajouté une remarque méthodologique sur la faible représentativité 
statistique de l'échantillon (5 avis), ce qui n'était pas explicitement 
demandé mais montre une bonne prise de recul sur la fiabilité de l'analyse.
