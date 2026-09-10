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


## Partie 2 — Comparer les techniques de prompting

### 2.1 Zero-shot

**Prompt :**
Classe le commentaire suivant en positif, négatif ou neutre :
"Le service est rapide mais l'application plante régulièrement."

**Réponse obtenue :**

![Réponse zero-shot](images/p2_zeroshot.png)


### Analyse 

Le LLM hésite entre deux classes : il propose d'abord "neutre/mitigé" 
(catégorie non prévue dans les 3 classes demandées), avant de préciser 
que s'il doit choisir strictement parmi positif/négatif/neutre, il 
pencherait pour "négatif". Le prompt zero-shot n'imposant aucune 
contrainte de format, le LLM se permet de sortir du cadre des 3 classes 
demandées.


### 2.2 One-shot

**Prompt :**
Voici un exemple de classification :
Commentaire : "Livraison en retard, produit endommagé."
Classe : négatif

Classe maintenant ce commentaire en positif, négatif ou neutre :
"Le service est rapide mais l'application plante régulièrement."

**Réponse obtenue :**

![Réponse one-shot](images/p2_oneshot.png)


### Analyse

Contrairement au zero-shot qui hésitait entre "neutre/mitigé" et "négatif", 
l'ajout d'un seul exemple suffit à faire trancher le LLM de façon nette 
pour "négatif", en respectant strictement les 3 classes demandées. 
L'exemple fourni (qui associe un problème concret à la classe "négatif") 
semble avoir cadré le format de réponse attendu.