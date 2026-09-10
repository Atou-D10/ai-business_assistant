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


### 2.3 Few-shot

**Prompt :**
Voici des exemples de classification :
Commentaire : "Livraison en retard, produit endommagé." → négatif
Commentaire : "Très satisfait, produit conforme et rapide." → positif
Commentaire : "Produit reçu, rien de particulier à signaler." → neutre

Classe maintenant ce commentaire en positif, négatif ou neutre :
"Le service est rapide mais l'application plante régulièrement."

**Réponse obtenue :**

![Réponse few-shot](images/p2_fewshot.png)

### Analyse

Le few-shot confirme le résultat du one-shot ("négatif"), avec une 
justification quasi identique. L'ajout de deux exemples supplémentaires 
(dont un cas "neutre" et un cas "positif") n'a pas fait varier le résultat 
ni significativement enrichi le raisonnement par rapport au one-shot, ce 
qui suggère qu'un seul exemple bien choisi suffisait déjà à cadrer la 
réponse pour cette tâche de classification simple.


### 2.4 Prompt structuré

**Prompt :**
Rôle : Tu es un système de classification de sentiment client.

Contexte : Une entreprise analyse les commentaires clients pour détecter 
les avis mitigés (contenant à la fois du positif et du négatif).

Tâche : Classe le commentaire suivant en une seule classe parmi : 
positif, négatif, neutre.

Commentaire : "Le service est rapide mais l'application plante régulièrement."

Contraintes : Si le commentaire contient à la fois un point positif et 
un point négatif, choisis la classe qui reflète l'impact principal sur 
l'expérience utilisateur. Justifie brièvement ton choix.

Format de sortie : 
Classe : [positif/négatif/neutre]
Justification : [une phrase]

**Réponse obtenue :**

![Réponse prompt structuré](images/p2_structure.png)

### Analyse 

Le prompt structuré confirme "négatif", avec une justification similaire 
aux deux techniques précédentes, mais cette fois le format de sortie est 
strictement respecté ("Classe : ..." / "Justification : ..."), ce qui 
facilite l'exploitation automatique de la réponse par une application.


### 2.5 Comparaison des 4 techniques

| Technique | Classe obtenue | Respect du format demandé | Qualité de justification |
|---|---|---|---|
| Zero-shot | Hésite entre "neutre/mitigé" et "négatif" | Non (sort du cadre des 3 classes) | Bonne, mais réponse ambiguë |
| One-shot | Négatif | Oui | Nette et directe |
| Few-shot | Négatif | Oui | Similaire au one-shot |
| Prompt structuré | Négatif | Oui, format strict respecté | Nette, format exploitable |

**Conclusion :** le zero-shot est la seule technique à ne pas trancher 
clairement et à sortir du cadre des classes demandées. Dès qu'un exemple 
est fourni (one-shot), le LLM se stabilise sur "négatif" et respecte le 
format — ajouter plus d'exemples (few-shot) n'apporte pas de gain 
supplémentaire sur ce cas précis. Le prompt structuré est le plus fiable 
pour un usage en production, car il impose un format de sortie exploitable 
directement par une application, sans avoir besoin d'exemples.