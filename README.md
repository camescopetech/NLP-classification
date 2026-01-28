# **Symptom2Disease – Classification de maladies à partir de symptômes**

  

## **1. Contexte général du projet**

  

Ce projet s’inscrit dans le cadre d’un travail académique en **Traitement Automatique du Langage Naturel (NLP)** et en **prompt engineering avancé**, appliqué à un cas d’usage médical. L’objectif principal est de concevoir et documenter une approche permettant de **prédire une pathologie à partir d’une description textuelle de symptômes**, en utilisant des techniques de classification.

  

Ce type de problématique est central dans de nombreux travaux actuels en intelligence artificielle pour la santé, notamment dans les domaines de l’aide à la décision médicale, du triage automatisé et de la médecine préventive. Le projet ne vise en aucun cas à établir un diagnostic médical, mais à étudier la capacité des modèles de langage à comprendre et structurer des informations cliniques exprimées en langage naturel.


## **2. Dataset choisi**

  

### **2.1 Source du dataset**

  

Le dataset utilisé pour ce projet est **Symptom2Disease**, disponible sur la plateforme Kaggle :

https://www.kaggle.com/datasets/niyarrbarman/symptom2disease

  

Il s’agit d’un dataset public.

----------

### **2.2 Description du dataset**

  

Le dataset est composé de paires **(symptômes, maladie)** :

-   La colonne _symptoms_ contient une description textuelle des symptômes ressentis par un patient. Ces descriptions peuvent prendre la forme de phrases complètes ou de listes de symptômes séparés par des virgules.
    
-   La colonne _disease_ correspond au label de classification, c’est-à-dire la pathologie associée aux symptômes.
    

  

Exemples de maladies présentes dans le dataset :

-   Diabetes
    
-   Hypertension
    
-   Migraine
    
-   Peptic Ulcer Disease
    
-   Arthritis
    
-   Asthma
    
-   Gastroesophageal Reflux Disease
    

  

Le dataset couvre un ensemble varié de pathologies, ce qui en fait un bon support pour étudier un problème de **classification multi-classe**.

----------

### **2.3 Intérêt du dataset pour ce projet**

  

Ce dataset est particulièrement adapté au projet pour plusieurs raisons :

-   Il contient exclusivement des données textuelles, ce qui correspond directement à une problématique NLP.
    
-   Les symptômes sont exprimés en langage naturel, avec une variabilité lexicale proche de descriptions réelles.
    
-   Le nombre de classes (maladies) est suffisant pour rendre la tâche non triviale.
    
-   Le format est simple et exploitable sans prétraitement complexe.
    


## **3. Problématique abordée**

  

La problématique principale du projet est la suivante :

  

**Comment prédire automatiquement une maladie à partir d’une description textuelle de symptômes ?**

  

D’un point de vue formel, le problème est modélisé comme une tâche de **classification supervisée multi-classe**, où :

-   L’entrée est un texte décrivant un ensemble de symptômes
    
-   La sortie est une classe correspondant à une pathologie
    

  

Cette problématique soulève plusieurs défis :

-   Les descriptions de symptômes peuvent être ambiguës ou incomplètes
    
-   Plusieurs maladies peuvent partager des symptômes communs
    
-   Le modèle doit être capable de comprendre le sens médical du texte, et pas uniquement des mots-clés isolés
    

## **4. Solution proposée**

  

### **4.1 Approche générale**

  

La solution mise en place repose sur l’utilisation de **modèles de langage** combinés à des techniques de **prompt engineering** afin de formuler efficacement la tâche de classification.

  

Plutôt que d’entraîner un modèle de classification classique à partir de zéro, nous exploitons la capacité des modèles de langage à effectuer des tâches de classification à partir d’instructions textuelles bien définies.

  

L’approche suit les étapes suivantes :

1.  Définition explicite de la tâche de classification dans le prompt
    
2.  Création de templates de prompts réutilisables
    
3.  Intégration d’exemples (few-shot learning)
    
4.  Ajout d’un raisonnement guidé (Chain-of-Thought)
    
5.  Évaluation systématique des performances
    

----------

### **4.2 Définition de la tâche (Task Definition)**

  

La tâche est formulée explicitement dans le prompt sous la forme :

  

« À partir de la description des symptômes fournie, identifier la maladie la plus probable parmi une liste de maladies connues. »

  

Cette définition claire permet au modèle de comprendre qu’il s’agit d’une tâche de classification, et non de génération libre ou de diagnostic médical détaillé.

----------

### **4.3 Prompt engineering et few-shot learning**

  

Des **templates de prompts** ont été conçus afin d’assurer une structure cohérente pour toutes les prédictions. Ces templates incluent :

-   Une instruction décrivant la tâche
    
-   Une liste de maladies possibles
    
-   Plusieurs exemples annotés (symptômes → maladie)
    
-   Une nouvelle description de symptômes à classer
    

  

Le **few-shot learning** consiste à fournir au modèle quelques exemples représentatifs directement dans le prompt. Cette technique permet d’améliorer significativement les performances, notamment lorsque les descriptions de symptômes sont complexes.

----------

### **4.4 Chain-of-Thought**

  

Une variante des prompts intègre un **raisonnement étape par étape** (Chain-of-Thought). Le modèle est invité à analyser les symptômes, à les relier à des indices cliniques, puis à déduire la maladie la plus probable.

  

Cette approche permet :

-   Une meilleure robustesse face à des descriptions ambiguës
    
-   Une amélioration de la cohérence des prédictions
    
-   Une meilleure interprétabilité du raisonnement du modèle
    

----------

### **4.5 Librairies et outils utilisés**

  

Les principaux outils et librairies utilisés sont :

-   Python 3
    
-   pandas pour la manipulation du dataset
    
-   numpy pour les opérations numériques
    
-   scikit-learn pour le calcul des métriques d’évaluation
    
-   tqdm pour le suivi des expériences
    
-   Google Colab pour l’expérimentation
    

## **5. Résultats et évaluation**

  

### **5.1 Protocole expérimental**

  

L’évaluation des performances a été menée selon un protocole expérimental visant à comparer **plusieurs configurations de prompts**, afin d’analyser l’impact des choix méthodologiques sur la qualité des prédictions.

  

Les expérimentations reposent sur :

-   Un même sous-ensemble de données de test, non utilisé dans les exemples few-shot
    
-   Des prédictions réalisées indépendamment pour chaque configuration
    
-   Un calcul systématique des métriques de classification
    

  

L’objectif n’est pas uniquement d’obtenir le meilleur score, mais de **comprendre pourquoi certaines stratégies de prompting sont plus efficaces que d’autres**.

----------

### **5.2 Métriques d’évaluation**

  

Les métriques utilisées sont celles classiquement employées pour les problèmes de classification multi-classe :

-   **Accuracy** : proportion globale de prédictions correctes.
    
-   **Precision (macro)** : moyenne de la précision calculée indépendamment pour chaque maladie. Cette métrique permet d’évaluer la fiabilité des prédictions positives.
    
-   **Recall (macro)** : moyenne du rappel pour chaque classe, mesurant la capacité du modèle à identifier correctement une maladie lorsqu’elle est présente.
    
-   **F1-score (macro)** : moyenne harmonique entre précision et rappel, fournissant une mesure synthétique et équilibrée.
    

  

Le choix des métriques macro est justifié par la volonté de **ne pas favoriser les maladies les plus fréquentes** au détriment de pathologies moins représentées.

----------

### **5.3 Configurations comparées**

  

Plusieurs configurations ont été comparées afin d’évaluer l’impact des techniques de prompt engineering :

1.  **Prompt zero-shot**
    
    Instruction simple demandant au modèle d’identifier la maladie à partir des symptômes, sans exemples.
    
2.  **Prompt few-shot**
    
    Ajout de plusieurs exemples annotés (symptômes → maladie) dans le prompt.
    
3.  **Prompt few-shot avec Chain-of-Thought**
    
    Le modèle est explicitement invité à analyser les symptômes étape par étape avant de produire la prédiction finale.
    

  

Ces configurations permettent d’isoler l’effet de chaque composant (exemples, raisonnement guidé).

----------

### **5.4 Résultats quantitatifs (test)**

  

L’évaluation finale est réalisée sur le **jeu de test (n = 150)**, dans un cadre **multi-classe** avec **15 maladies**. Le tableau ci-dessous reprend les métriques calculées dans le notebook pour chaque variante, triées par performance (F1 macro).

**Variante**

**Fails**

**Accuracy**

**Precision (macro)**

**Recall (macro)**

**F1 (macro)**

| Variante | Fails | Accuracy | Precision (macro) | Recall (macro) | F1-score (macro) |
|--------|------:|---------:|------------------:|---------------:|-----------------:|
| P2 — Liste + few-shot dynamique (TF-IDF) | 0 | 0.567 | 0.604 | 0.567 | 0.550 |
| P3.3 — CoT + few-shot dynamique (TF-IDF) | 0 | 0.560 | 0.568 | 0.560 | 0.524 |
| P3.2 — CoT + few-shot statique | 0 | 0.447 | 0.369 | 0.447 | 0.370 |
| P3.1 — CoT + liste maladies | 0 | 0.320 | 0.192 | 0.320 | 0.208 |
| P1 — Liste des maladies | 0 | 0.293 | 0.203 | 0.293 | 0.197 |
| P0 — Baseline | 0 | 0.300 | 0.152 | 0.300 | 0.194 |
| P3.0 — CoT seul | 134 | 0.087 | 0.088 | 0.087 | 0.036 |

Remarque : la colonne **Fails** correspond au nombre de sorties du modèle qui n’ont pas pu être converties de manière fiable en un label unique (par exemple réponses non conformes au format attendu). Ces échecs impactent directement les performances mesurées.

----------

### **5.5 Analyse comparative des résultats**

  

#### **Impact du few-shot learning**

  

L’ajout d’exemples annotés dans le prompt permet au modèle :

-   De mieux comprendre la correspondance entre symptômes et maladies
    
-   D’identifier des motifs récurrents dans les descriptions cliniques
    
-   De réduire les erreurs liées à des interprétations trop générales
    

  

Cette amélioration est particulièrement visible pour les maladies partageant des symptômes communs (par exemple fatigue, douleurs, troubles digestifs).


#### **Apport du Chain-of-Thought**

  

L’intégration d’un raisonnement étape par étape apporte plusieurs bénéfices :

-   Le modèle est incité à considérer l’ensemble des symptômes avant de décider
    
-   Les prédictions deviennent plus cohérentes et moins impulsives
    
-   Les erreurs dues à la présence d’un symptôme dominant sont réduites
    

  

Dans les cas ambigus, le Chain-of-Thought permet au modèle de pondérer plusieurs indices cliniques, ce qui explique l’amélioration du F1-score macro.

----------

### **5.6 Analyse qualitative des erreurs**

  

Une analyse qualitative des prédictions incorrectes met en évidence plusieurs types d’erreurs :

-   **Confusion entre maladies aux symptômes proches**, par exemple entre troubles digestifs différents
    
-   **Descriptions de symptômes trop vagues**, ne permettant pas une discrimination claire
    
-   **Absence de symptômes clés** dans certaines entrées du dataset
    

  

Ces erreurs soulignent les limites inhérentes aux données textuelles et justifient l’utilisation de techniques de raisonnement explicite.

----------

### **5.7 Exemple détaillé de prédiction**

  

Entrée (symptômes) :

  

    Frequent urination, excessive thirst, unexplained weight loss, fatigue

  

Raisonnement (Chain-of-Thought) :

-   Présence de symptômes métaboliques
    
-   Association fréquente avec un dérèglement glycémique
    
-   Absence de symptômes respiratoires ou infectieux
    

  

Sortie prédite :

  

    Diabetes

  

Cet exemple illustre comment le raisonnement guidé permet d’aboutir à une prédiction plus fiable.


## **6. Discussion des résultats**

  

### **6.1 Synthèse des tendances observées**

  

Les résultats montrent une tendance très nette : la qualité de la classification dépend fortement de la quantité de structure imposée au modèle (instruction explicite, liste de classes, exemples, puis raisonnement).

-   Les variantes les plus performantes sont **P2** (few-shot dynamique) et **P3.3** (CoT + few-shot dynamique).
    
-   Les variantes sans exemples (P0, P1) restent proches d’un comportement de base, avec une précision macro faible.
    
-   La variante **P3.0 (CoT seul)** obtient des performances très basses et présente un nombre important de sorties non exploitables (fails).
    

  

Cette hiérarchie est cohérente avec l’hypothèse initiale du projet : sur une tâche de classification médicale, l’accès à des exemples pertinents et une structure de réponse contrôlée sont déterminants.

----------

### **6.2 Pourquoi le few-shot dynamique (TF-IDF) fonctionne mieux**

  

La variante **few-shot dynamique** sélectionne, pour chaque exemple de test, des exemples d’entraînement jugés similaires. Dans ce notebook, cette similarité est estimée via une approche de type **TF-IDF**, ce qui présente plusieurs avantages :

1.  **Réduction du décalage sémantique entre exemples et entrée**
    
    Le modèle reçoit des exemples proches lexicalement (et souvent thématiquement) de l’entrée, ce qui facilite l’analogie.
    
2.  **Couverture plus pertinente des cas difficiles**
    
    Lorsque les symptômes sont ambigus (symptômes partagés par plusieurs maladies), les exemples voisins aident le modèle à discriminer.
    
3.  **Meilleure utilisation des tokens disponibles**
    
    Plutôt que d’intégrer des exemples fixes potentiellement hors-sujet, la sélection dynamique alloue le contexte à des exemples informatifs pour l’entrée courante.
    

Ces éléments expliquent que **P2** obtienne le meilleur compromis global (meilleure précision macro et meilleur F1 macro).

----------

### **6.3 CoT : gain réel, mais conditionnel**

  

L’ajout du Chain-of-Thought (CoT) n’améliore pas automatiquement les scores. Dans les résultats, **P3.3** est performant, mais reste légèrement derrière **P2**.

  

Une interprétation cohérente est la suivante :

-   Le CoT peut améliorer la robustesse lorsqu’il est couplé à des exemples pertinents (cas de P3.3).
    
-   En revanche, le CoT augmente aussi la liberté de génération du modèle. Si la sortie attendue doit respecter un format strict, cela peut augmenter les erreurs de format, les réponses trop longues ou les réponses non directement classifiables.
    

  

Dans ce projet, le meilleur usage du CoT est donc **guidé et contraint** : CoT + liste de classes + exemples pertinents.

----------

### **6.4 Interprétation du cas P3.0 (CoT seul) et des fails**

  

La variante **P3.0** présente :

-   Une accuracy très faible
    
-   Un F1 macro très faible
    
-   Et surtout un nombre élevé de **fails** (sorties non parseables / non exploitables)
    

  

Cela suggère que, lorsqu’on demande au modèle de raisonner sans fournir :

-   une liste de classes,
    
-   des exemples,
    
-   et un format de réponse strict,
    

  

le modèle tend à produire des réponses plus libres (explications, hypothèses, plusieurs maladies possibles). Ces sorties deviennent difficiles à convertir de manière fiable en un label unique, ce qui pénalise fortement l’évaluation. Ce résultat illustre une limite importante des approches LLM en classification : **la performance dépend autant de la qualité du raisonnement que de la capacité à produire une sortie structurée**.

----------

### **6.5 Limites et implications**

  

Plusieurs limites doivent être prises en compte dans l’interprétation des résultats :

-   **Chevauchement symptomatique** : certaines maladies partagent des symptômes généraux (fatigue, fièvre, douleur), ce qui limite la séparabilité.
    
-   **Biais de formulation** : la sélection TF-IDF favorise la similarité lexicale ; une similarité sémantique plus profonde pourrait nécessiter des embeddings.
    
-   **Évaluation sur un dataset contrôlé** : les performances ne se généralisent pas directement à des descriptions réelles de patients (variabilité, fautes, contexte médical, comorbidités).
    

  

Malgré ces limites, les résultats confirment un point central : pour une classification médicale, les stratégies de prompt engineering apportent des gains significatifs, surtout lorsque l’on contrôle le contexte (exemples) et le format de sortie.


## **7. Installation et exécution**

  
Le code du projet (`main.ipynb`) peut simplement être téléchargé et ouvert dans un éditeur de code (*par exemple VSCode*). Le notebook contient l'ensemble du code commenté ainsi que les résultats obtenus.

## **8. Limites et perspectives**

 

Ce travail présente plusieurs limites :

-   Le modèle n’est pas entraîné spécifiquement sur des données médicales réelles
    
-   Les descriptions de symptômes restent synthétiques
    
-   L’approche ne remplace pas un modèle entraîné par fine-tuning
    

  

Des pistes d’amélioration incluent :

-   Comparaison avec des approches classiques (TF-IDF + SVM, Logistic Regression)
    
-   Fine-tuning d’un modèle de type BERT
    
-   Extension vers une classification multi-label
    
