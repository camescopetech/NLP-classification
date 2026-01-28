# **Symptom2Disease – Classification de maladies à partir de symptômes**

  

## **1. Contexte général du projet**

  

Ce projet s’inscrit dans le cadre d’un travail académique en **Traitement Automatique du Langage Naturel (NLP)** et en **prompt engineering avancé**, appliqué à un cas d’usage médical. L’objectif principal est de concevoir et documenter une approche permettant de **prédire une pathologie à partir d’une description textuelle de symptômes**, en utilisant des techniques de classification.

  

Ce type de problématique est central dans de nombreux travaux actuels en intelligence artificielle pour la santé, notamment dans les domaines de l’aide à la décision médicale, du triage automatisé et de la médecine préventive. Le projet ne vise en aucun cas à établir un diagnostic médical, mais à étudier la capacité des modèles de langage à comprendre et structurer des informations cliniques exprimées en langage naturel.

## **2. Installation et exécution**

  
Le code du projet (`main.ipynb`) peut simplement être téléchargé et ouvert dans un éditeur de code (*par exemple VSCode*). Le notebook contient l'ensemble du code commenté ainsi que les résultats obtenus.

## **3. Dataset choisi**

  

### **3.1 Source du dataset**

  

Le dataset utilisé pour ce projet est **Symptom2Disease**, disponible sur la plateforme Kaggle :

https://www.kaggle.com/datasets/niyarrbarman/symptom2disease

  

Il s’agit d’un dataset public.

----------

### **3.2 Description du dataset**

  

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

### **3.3 Intérêt du dataset pour ce projet**

  

Ce dataset est particulièrement adapté au projet pour plusieurs raisons :

-   Il contient exclusivement des données textuelles, ce qui correspond directement à une problématique NLP.
    
-   Les symptômes sont exprimés en langage naturel, avec une variabilité lexicale proche de descriptions réelles.
    
-   Le nombre de classes (maladies) est suffisant pour rendre la tâche non triviale.
    
-   Le format est simple et exploitable sans prétraitement complexe.
    


## **4. Problématique abordée**

  

La problématique principale du projet est la suivante :

  

**Comment prédire automatiquement une maladie à partir d’une description textuelle de symptômes ?**

  

D’un point de vue formel, le problème est modélisé comme une tâche de **classification supervisée multi-classe**, où :

-   L’entrée est un texte décrivant un ensemble de symptômes
    
-   La sortie est une classe correspondant à une pathologie
    

  

Cette problématique soulève plusieurs défis :

-   Les descriptions de symptômes peuvent être ambiguës ou incomplètes
    
-   Plusieurs maladies peuvent partager des symptômes communs
    
-   Le modèle doit être capable de comprendre le sens médical du texte, et pas uniquement des mots-clés isolés
    

## **5. Solution proposée**

  

### **5.1 Approche générale**

  

La solution mise en place repose sur l’utilisation de **modèles de langage** combinés à des techniques de **prompt engineering** afin de formuler efficacement la tâche de classification.

  

Plutôt que d’entraîner un modèle de classification classique à partir de zéro, nous exploitons la capacité des modèles de langage à effectuer des tâches de classification à partir d’instructions textuelles bien définies.

  

L’approche suit les étapes suivantes :

1.  Définition explicite de la tâche de classification dans le prompt
    
2.  Création de templates de prompts réutilisables
    
3.  Intégration d’exemples (few-shot learning)
    
4.  Ajout d’un raisonnement guidé (Chain-of-Thought)
    
5.  Évaluation systématique des performances
    

----------

### **5.2 Définition de la tâche (Task Definition)**

  

La tâche est formulée explicitement dans le prompt sous la forme :

  

« À partir de la description des symptômes fournie, identifier la maladie la plus probable parmi une liste de maladies connues. »

  

Cette définition claire permet au modèle de comprendre qu’il s’agit d’une tâche de classification, et non de génération libre ou de diagnostic médical détaillé.

----------

### **5.3 Prompt engineering et few-shot learning**

  

Des **templates de prompts** ont été conçus afin d’assurer une structure cohérente pour toutes les prédictions. Ces templates incluent :

-   Une instruction décrivant la tâche
    
-   Une liste de maladies possibles
    
-   Plusieurs exemples annotés (symptômes → maladie)
    
-   Une nouvelle description de symptômes à classer
    

  

Le **few-shot learning** consiste à fournir au modèle quelques exemples représentatifs directement dans le prompt. Cette technique permet d’améliorer significativement les performances, notamment lorsque les descriptions de symptômes sont complexes.

----------

### **5.4 Chain-of-Thought**

  

Une variante des prompts intègre un **raisonnement étape par étape** (Chain-of-Thought). Le modèle est invité à analyser les symptômes, à les relier à des indices cliniques, puis à déduire la maladie la plus probable.

  

Cette approche permet :

-   Une meilleure robustesse face à des descriptions ambiguës
    
-   Une amélioration de la cohérence des prédictions
    
-   Une meilleure interprétabilité du raisonnement du modèle
    

----------

### **5.5 Librairies et outils utilisés**

  

Les principaux outils et librairies utilisés sont :

-   Python 3
    
-   pandas pour la manipulation du dataset
    
-   numpy pour les opérations numériques
    
-   scikit-learn pour le calcul des métriques d’évaluation
    
-   tqdm pour le suivi des expériences
    
-   Google Colab pour l’expérimentation
    

## **6. Résultats et évaluation**

  

### **6.1 Protocole expérimental**

  

L’évaluation des performances a été menée selon un protocole expérimental visant à comparer **plusieurs configurations de prompts**, afin d’analyser l’impact des choix méthodologiques sur la qualité des prédictions.

  

Les expérimentations reposent sur :

-   Un même sous-ensemble de données de test, non utilisé dans les exemples few-shot
    
-   Des prédictions réalisées indépendamment pour chaque configuration
    
-   Un calcul systématique des métriques de classification
    

  

L’objectif n’est pas uniquement d’obtenir le meilleur score, mais de **comprendre pourquoi certaines stratégies de prompting sont plus efficaces que d’autres**.

----------

### **6.2 Métriques d’évaluation**

  

Les métriques utilisées sont celles classiquement employées pour les problèmes de classification multi-classe :

-   **Accuracy** : proportion globale de prédictions correctes.
    
-   **Precision (macro)** : moyenne de la précision calculée indépendamment pour chaque maladie. Cette métrique permet d’évaluer la fiabilité des prédictions positives.
    
-   **Recall (macro)** : moyenne du rappel pour chaque classe, mesurant la capacité du modèle à identifier correctement une maladie lorsqu’elle est présente.
    
-   **F1-score (macro)** : moyenne harmonique entre précision et rappel, fournissant une mesure synthétique et équilibrée.
    

  

Le choix des métriques macro est justifié par la volonté de **ne pas favoriser les maladies les plus fréquentes** au détriment de pathologies moins représentées.

----------

### **6.3 Configurations comparées**

  

Plusieurs configurations ont été comparées afin d’évaluer l’impact des techniques de prompt engineering :

1.  **Prompt zero-shot**
    
    Instruction simple demandant au modèle d’identifier la maladie à partir des symptômes, sans exemples.
    
2.  **Prompt few-shot**
    
    Ajout de plusieurs exemples annotés (symptômes → maladie) dans le prompt.
    
3.  **Prompt few-shot avec Chain-of-Thought**
    
    Le modèle est explicitement invité à analyser les symptômes étape par étape avant de produire la prédiction finale.
    

  

Ces configurations permettent d’isoler l’effet de chaque composant (exemples, raisonnement guidé).

----------

### **6.4 Résultats quantitatifs (test)**

  

L’évaluation finale est réalisée sur le **jeu de test (n = 150)**, dans un cadre **multi-classe** avec **15 maladies**. Le tableau ci-dessous reprend les métriques calculées dans le notebook pour chaque variante, triées par performance (F1 macro).


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

### **6.5 Analyse comparative des résultats**


  

Les résultats obtenus mettent en évidence des différences très marquées entre les stratégies de prompting évaluées, tant en termes de performance globale que de robustesse. L’analyse des métriques macro (précision, rappel et F1-score) permet d’aller au-delà de la simple accuracy et d’évaluer plus finement la capacité des modèles à traiter équitablement l’ensemble des classes de maladies, dans un contexte de classification multi-classes déséquilibrée.

  

La meilleure configuration observée est **P2 — Liste + few-shot dynamique (TF-IDF)**, qui atteint une accuracy de 0.567 et surtout un F1-macro de 0.550, soit le score le plus élevé parmi toutes les variantes testées. Cette performance s’explique par la combinaison d’un espace de sortie explicitement contraint (liste des maladies possibles) et d’une sélection dynamique d’exemples pertinents, basée sur la similarité TF-IDF entre l’entrée courante et les cas du jeu d’entraînement. Cette stratégie permet au modèle de s’ancrer dans des exemples proches sur le plan sémantique, réduisant les confusions inter-classes et améliorant simultanément la précision macro (0.604) et le rappel macro (0.567). Le fait que cette configuration ne produise aucun échec de parsing renforce également sa robustesse opérationnelle.

  

La variante **P3.3 — Chain-of-Thought + few-shot dynamique (TF-IDF)** obtient des performances légèrement inférieures, avec un F1-macro de 0.524 et une accuracy de 0.560. Bien que l’ajout du raisonnement explicite puisse théoriquement aider le modèle à structurer sa décision, les résultats montrent ici un léger recul par rapport à P2 sur l’ensemble des métriques. En particulier, la baisse de la précision macro suggère que le raisonnement étape par étape peut introduire une forme de sur-interprétation, conduisant parfois le modèle à justifier une prédiction incorrecte de manière cohérente mais erronée. Dans le cadre d’une tâche de classification fermée, où la sortie doit appartenir à un ensemble limité et bien défini de maladies, un prompt plus direct, fortement guidé par des exemples pertinents, semble donc plus efficace qu’un raisonnement explicité.

  

Les configurations reposant sur un **few-shot statique** ou sur des **contextes partiels** montrent une dégradation nette des performances. La variante **P3.2 — CoT + few-shot statique** atteint un F1-macro de 0.370, ce qui reste supérieur aux approches sans exemples mais très en-deçà des stratégies dynamiques. Cette différence souligne l’importance cruciale de la pertinence des exemples fournis : des cas fixes, même bien choisis a priori, ne suffisent pas à couvrir la diversité sémantique des symptômes décrits dans les données d’entrée. Le modèle bénéficie davantage d’exemples contextuellement proches que d’un raisonnement complexe appliqué à des cas génériques.

  

Lorsque le contexte est réduit à une simple **liste de maladies** (P1) ou à un **prompt baseline sans guidage** (P0), les performances deviennent très proches, avec des F1-macro respectifs de 0.197 et 0.194. Cette quasi-équivalence indique que la contrainte sur l’espace de sortie, prise isolément, n’apporte qu’un gain marginal. Le modèle peine à discriminer correctement les classes sans ancrage explicite dans des exemples concrets, ce qui se traduit par une faible précision macro et un rappel limité.

  

La configuration **P3.1 — Chain-of-Thought + liste de maladies** améliore légèrement ces résultats (F1-macro de 0.208), mais le gain reste modeste. Le raisonnement guidé, en l’absence d’exemples, ne permet donc pas au modèle de construire des frontières de décision suffisamment discriminantes entre les classes médicales.

  

Enfin, la variante **P3.0 — Chain-of-Thought seul** met en évidence un problème majeur de robustesse. Avec 134 échecs de parsing, une accuracy de 0.087 et un F1-macro de 0.036, cette configuration est largement inexploitable en pratique. Ces résultats montrent que, sans contrainte explicite sur le format de sortie ni ancrage par des exemples, le modèle produit fréquemment des réponses non conformes, rendant l’extraction automatique de la prédiction impossible. L’effondrement des métriques est ici davantage lié à un échec d’interface entre le prompt et le post-traitement qu’à une incapacité purement sémantique du modèle.

  

Dans l’ensemble, ces résultats montrent que **le facteur déterminant de la performance n’est ni la complexité du raisonnement demandé ni la simple contrainte sur l’espace de sortie, mais la pertinence contextuelle des exemples fournis**. Le few-shot dynamique basé sur TF-IDF apparaît comme la stratégie la plus efficace et la plus stable, tandis que le Chain-of-Thought, bien que conceptuellement intéressant, doit être utilisé avec précaution dans les tâches de classification fermée, où il peut introduire du bruit plutôt que du signal.

## **7. Discussion critique et perspectives**

Les résultats obtenus montrent clairement que les performances des modèles dépendent davantage de la structuration du contexte que de la complexité du raisonnement demandé. En particulier, le few-shot dynamique basé sur TF-IDF améliore fortement les métriques macro, mais ce gain doit être interprété avec prudence. Il reflète en grande partie une mise en correspondance efficace entre l’entrée et des exemples proches, plutôt qu’une réelle capacité de généralisation du modèle. Le système raisonne principalement par analogie contextuelle, ce qui limite la portée des conclusions que l’on peut tirer sur ses capacités intrinsèques de compréhension médicale.

Le Chain-of-Thought, souvent présenté comme un levier d’amélioration du raisonnement, ne montre ici aucun bénéfice systématique. Lorsqu’il est correctement encadré, il conduit à des performances légèrement inférieures à un prompt direct enrichi d’exemples pertinents ; lorsqu’il est utilisé sans contraintes fortes, il entraîne une instabilité importante des sorties et des échecs de parsing. Cela met en évidence une tension entre explicabilité apparente et robustesse, particulièrement problématique dans un cadre de classification automatisée où la fiabilité du format de sortie est essentielle.

D’un point de vue méthodologique, l’évaluation reste limitée par l’absence d’analyse fine des erreurs et des confusions entre maladies proches. Les métriques macro offrent une vision globale équilibrée, mais elles ne permettent pas de distinguer les erreurs cliniquement bénignes des erreurs potentiellement critiques. Par ailleurs, l’approche étant strictement in-context, les performances observées sont très sensibles au choix des exemples, à leur formulation et à la structure du prompt, ce qui pose la question de la stabilité des résultats hors du cadre expérimental.

Ces observations ouvrent plusieurs perspectives. L’utilisation de représentations sémantiques plus riches que TF-IDF pour la sélection d’exemples pourrait améliorer la robustesse et la généralisation. Des sorties multi-label ou hiérarchiques permettraient également de mieux refléter la réalité clinique des recouvrements symptomatiques. Enfin, une évaluation qualitative systématique, couplée à des tests de robustesse hors distribution, apparaît indispensable pour mieux caractériser les limites réelles de ce type d’approches.

## **Conclusion**

Cette étude montre que les performances des grands modèles de langage en classification médicale dépendent avant tout de la structuration du contexte fourni au modèle. Les meilleurs résultats sont obtenus lorsque l’espace de sortie est explicitement contraint et que les exemples sont sélectionnés de manière dynamique par similarité sémantique, ce qui permet au modèle de raisonner efficacement par analogie. À l’inverse, l’ajout d’un raisonnement explicite de type Chain-of-Thought ne conduit pas à une amélioration systématique des performances et peut même nuire à la robustesse lorsque le format de sortie n’est pas strictement contrôlé.

Ces résultats soulignent que les gains observés relèvent davantage d’une ingénierie fine du prompt que d’une véritable capacité de généralisation du modèle, invitant à une interprétation prudente dans des contextes sensibles comme la santé. Ce travail met ainsi en évidence le potentiel du prompt engineering comme levier opérationnel, tout en rappelant la nécessité de contrôles rigoureux, d’analyses qualitatives et d’évaluations complémentaires pour envisager une utilisation fiable et scientifiquement interprétable des LLM en classification médicale.
