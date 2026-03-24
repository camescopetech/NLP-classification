# **Symptom2Disease – Disease Classification from Symptoms**



## **1. General Project Context**



This project is part of an academic work in **Natural Language Processing (NLP)** and **advanced prompt engineering**, applied to a medical use case. The main objective is to design and document an approach to **predict a disease from a textual description of symptoms**, using classification techniques.



This type of problem is central to many current works in artificial intelligence for healthcare, particularly in the areas of medical decision support, automated triage, and preventive medicine. The project does not aim to establish a medical diagnosis, but to study the ability of language models to understand and structure clinical information expressed in natural language.

## **2. Installation and Execution**


The project code (`main.ipynb`) can simply be downloaded and opened in a code editor (*e.g. VSCode*). The notebook contains all the commented code as well as the results obtained.


To install the repo in a code editor:

    https://github.com/camescopetech/NLP-classification.git

    cd /NLP-classification

Open the main.ipynb file to view the code and results.

## **3. Chosen Dataset**



### **3.1 Dataset Source**



The dataset used for this project is **Symptom2Disease**, available on the Kaggle platform:

https://www.kaggle.com/datasets/niyarrbarman/symptom2disease



It is a public dataset.

----------

### **3.2 Dataset Description**



The dataset consists of **(symptoms, disease)** pairs:

-   The _symptoms_ column contains a textual description of symptoms experienced by a patient. These descriptions can take the form of complete sentences or lists of symptoms separated by commas.

-   The _disease_ column corresponds to the classification label, i.e. the pathology associated with the symptoms.




Examples of diseases present in the dataset:

-   Diabetes

-   Hypertension

-   Migraine

-   Peptic Ulcer Disease

-   Arthritis

-   Asthma

-   Gastroesophageal Reflux Disease




The dataset covers a varied set of pathologies, making it a good basis for studying a **multi-class classification** problem.

----------

### **3.3 Dataset Relevance for This Project**



This dataset is particularly suited to the project for several reasons:

-   It contains exclusively textual data, which directly corresponds to an NLP problem.

-   Symptoms are expressed in natural language, with lexical variability close to real-world descriptions.

-   The number of classes (diseases) is sufficient to make the task non-trivial.

-   The format is simple and usable without complex preprocessing.



## **4. Problem Statement**



The main problem addressed by the project is the following:



**How to automatically predict a disease from a textual description of symptoms?**



From a formal standpoint, the problem is modeled as a **supervised multi-class classification** task, where:

-   The input is a text describing a set of symptoms

-   The output is a class corresponding to a pathology




This problem raises several challenges:

-   Symptom descriptions can be ambiguous or incomplete

-   Multiple diseases can share common symptoms

-   The model must be able to understand the medical meaning of the text, not just isolated keywords


## **5. Proposed Solution**



### **5.1 General Approach**



The implemented solution relies on the use of **language models** combined with **prompt engineering** techniques in order to effectively formulate the classification task.



Rather than training a classic classification model from scratch, we leverage the ability of language models to perform classification tasks from well-defined textual instructions.



The approach follows these steps:

1.  Explicit definition of the classification task in the prompt

2.  Creation of reusable prompt templates

3.  Integration of examples (few-shot learning)

4.  Addition of guided reasoning (Chain-of-Thought)

5.  Systematic evaluation of performance


----------

### **5.2 Task Definition**



The task is explicitly formulated in the prompt as follows:



"From the provided symptom description, identify the most likely disease from a list of known diseases."



This clear definition allows the model to understand that this is a classification task, and not free generation or detailed medical diagnosis.

----------

### **5.3 Prompt Engineering and Few-Shot Learning**



**Prompt templates** were designed to ensure a consistent structure for all predictions. These templates include:

-   An instruction describing the task

-   A list of possible diseases

-   Several annotated examples (symptoms → disease)

-   A new symptom description to classify




**Few-shot learning** consists of providing the model with a few representative examples directly in the prompt. This technique significantly improves performance, especially when symptom descriptions are complex.

----------

### **5.4 Chain-of-Thought**



A variant of the prompts integrates **step-by-step reasoning** (Chain-of-Thought). The model is prompted to analyze the symptoms, connect them to clinical clues, and then deduce the most likely disease.



This approach enables:

-   Better robustness against ambiguous descriptions

-   Improved consistency of predictions

-   Better interpretability of the model's reasoning


----------

### **5.5 Libraries and Tools Used**



The main tools and libraries used are:

-   Python 3

-   pandas for dataset manipulation

-   numpy for numerical operations

-   scikit-learn for computing evaluation metrics

-   tqdm for experiment tracking

-   Google Colab for experimentation


## **6. Results and Evaluation**



### **6.1 Experimental Protocol**



Performance evaluation was conducted according to an experimental protocol aimed at comparing **several prompt configurations**, in order to analyze the impact of methodological choices on prediction quality.



The experiments rely on:

-   The same subset of test data, not used in the few-shot examples

-   Predictions made independently for each configuration

-   Systematic computation of classification metrics




The goal is not only to achieve the best score, but to **understand why certain prompting strategies are more effective than others**.

----------

### **6.2 Evaluation Metrics**



The metrics used are those classically employed for multi-class classification problems:

-   **Accuracy**: overall proportion of correct predictions.

-   **Precision (macro)**: average of the precision computed independently for each disease. This metric evaluates the reliability of positive predictions.

-   **Recall (macro)**: average recall for each class, measuring the model's ability to correctly identify a disease when it is present.

-   **F1-score (macro)**: harmonic mean between precision and recall, providing a synthetic and balanced measure.




The choice of macro metrics is justified by the desire to **not favor the most frequent diseases** at the expense of less represented pathologies.

----------

### **6.3 Compared Configurations**



Several configurations were compared to evaluate the impact of prompt engineering techniques:

1.  **Zero-shot prompt**

    Simple instruction asking the model to identify the disease from symptoms, without examples.

2.  **Few-shot prompt**

    Addition of several annotated examples (symptoms → disease) in the prompt.

3.  **Few-shot prompt with Chain-of-Thought**

    The model is explicitly prompted to analyze symptoms step by step before producing the final prediction.




These configurations allow isolating the effect of each component (examples, guided reasoning).

----------

### **6.4 Quantitative Results (test)**



The final evaluation is performed on the **test set (n = 150)**, in a **multi-class** setting with **15 diseases**. The table below summarizes the metrics computed in the notebook for each variant, sorted by performance (macro F1).


| Variant | Fails | Accuracy | Precision (macro) | Recall (macro) | F1-score (macro) |
|--------|------:|---------:|------------------:|---------------:|-----------------:|
| P2 — List + dynamic few-shot (TF-IDF) | 0 | 0.567 | 0.604 | 0.567 | 0.550 |
| P3.3 — CoT + dynamic few-shot (TF-IDF) | 0 | 0.560 | 0.568 | 0.560 | 0.524 |
| P3.2 — CoT + static few-shot | 0 | 0.447 | 0.369 | 0.447 | 0.370 |
| P3.1 — CoT + disease list | 0 | 0.320 | 0.192 | 0.320 | 0.208 |
| P1 — Disease list | 0 | 0.293 | 0.203 | 0.293 | 0.197 |
| P0 — Baseline | 0 | 0.300 | 0.152 | 0.300 | 0.194 |
| P3.0 — CoT only | 134 | 0.087 | 0.088 | 0.087 | 0.036 |

Note: the **Fails** column corresponds to the number of model outputs that could not be reliably converted into a single label (e.g. responses not conforming to the expected format). These failures directly impact the measured performance.

----------

### **6.5 Comparative Analysis of Results**



The results highlight very marked differences between the prompting strategies evaluated, both in terms of overall performance and robustness. The analysis of macro metrics (precision, recall and F1-score) allows going beyond simple accuracy and more finely evaluating the ability of models to treat all disease classes fairly, in an imbalanced multi-class classification context.



The best observed configuration is **P2 — List + dynamic few-shot (TF-IDF)**, which achieves an accuracy of 0.567 and especially a macro F1 of 0.550, the highest score among all tested variants. This performance is explained by the combination of an explicitly constrained output space (list of possible diseases) and a dynamic selection of relevant examples, based on TF-IDF similarity between the current input and training set cases. This strategy allows the model to anchor itself in semantically close examples, reducing inter-class confusion and simultaneously improving macro precision (0.604) and macro recall (0.567). The fact that this configuration produces no parsing failures also reinforces its operational robustness.



The variant **P3.3 — Chain-of-Thought + dynamic few-shot (TF-IDF)** achieves slightly lower performance, with a macro F1 of 0.524 and an accuracy of 0.560. Although adding explicit reasoning could theoretically help the model structure its decision, the results here show a slight decrease compared to P2 across all metrics. In particular, the drop in macro precision suggests that step-by-step reasoning can introduce a form of over-interpretation, sometimes leading the model to justify an incorrect prediction in a coherent but wrong manner. In the context of a closed classification task, where the output must belong to a limited and well-defined set of diseases, a more direct prompt strongly guided by relevant examples thus seems more effective than explicit reasoning.



Configurations relying on **static few-shot** or **partial contexts** show a clear degradation in performance. The variant **P3.2 — CoT + static few-shot** achieves a macro F1 of 0.370, which remains above approaches without examples but far below dynamic strategies. This difference underlines the crucial importance of the relevance of provided examples: fixed cases, even well chosen a priori, are not sufficient to cover the semantic diversity of symptoms described in the input data. The model benefits more from contextually close examples than from complex reasoning applied to generic cases.



When context is reduced to a simple **disease list** (P1) or a **baseline prompt without guidance** (P0), performance becomes very similar, with respective macro F1 scores of 0.197 and 0.194. This near-equivalence indicates that the constraint on the output space, taken in isolation, only provides a marginal gain. The model struggles to correctly discriminate between classes without explicit anchoring in concrete examples, resulting in low macro precision and limited recall.



The configuration **P3.1 — Chain-of-Thought + disease list** slightly improves these results (macro F1 of 0.208), but the gain remains modest. Guided reasoning, in the absence of examples, therefore does not allow the model to build sufficiently discriminating decision boundaries between medical classes.



Finally, the variant **P3.0 — Chain-of-Thought only** highlights a major robustness problem. With 134 parsing failures, an accuracy of 0.087 and a macro F1 of 0.036, this configuration is largely unusable in practice. These results show that, without an explicit constraint on the output format or anchoring through examples, the model frequently produces non-conforming responses, making automatic extraction of the prediction impossible. The collapse of metrics is here more related to an interface failure between the prompt and the post-processing than to a purely semantic inability of the model.



Overall, these results show that **the determining factor of performance is neither the complexity of the requested reasoning nor the simple constraint on the output space, but the contextual relevance of the provided examples**. Dynamic few-shot based on TF-IDF appears as the most effective and stable strategy, while Chain-of-Thought, although conceptually interesting, must be used with caution in closed classification tasks, where it can introduce noise rather than signal.

## **7. Critical Discussion and Perspectives**

The results clearly show that the performance of large language models in medical classification depends primarily on the structuring of the context provided to the model. The best results are obtained when the output space is explicitly constrained and examples are dynamically selected by semantic similarity, which allows the model to reason effectively by analogy. Conversely, the addition of explicit Chain-of-Thought reasoning does not lead to a systematic improvement in performance and can even harm robustness when the output format is not strictly controlled.

These results highlight that the observed gains are more the result of fine prompt engineering than a genuine generalization capability of the model, calling for cautious interpretation in sensitive contexts such as healthcare. Dynamic few-shot based on TF-IDF shows itself to be the most robust and effective approach for this task.

From a methodological standpoint, the evaluation remains limited by the absence of a fine-grained analysis of errors and confusions between similar diseases. Macro metrics offer a globally balanced view, but they do not allow distinguishing clinically benign errors from potentially critical ones. Furthermore, since the approach is strictly in-context, the observed performances are very sensitive to the choice of examples, their formulation, and the prompt structure, which raises questions about the stability of results outside the experimental framework.

These observations open several perspectives. The use of richer semantic representations than TF-IDF for example selection could improve robustness and generalization. Multi-label or hierarchical outputs would also better reflect the clinical reality of symptomatic overlaps. Finally, a systematic qualitative evaluation, coupled with out-of-distribution robustness tests, appears indispensable to better characterize the real limits of this type of approach.

## **Conclusion**

This study shows that the performance of large language models in medical classification depends above all on the structuring of the context provided to the model. The best results are obtained when the output space is explicitly constrained and examples are dynamically selected by semantic similarity, allowing the model to reason effectively by analogy. Conversely, adding explicit Chain-of-Thought reasoning does not lead to a systematic improvement in performance and can even harm robustness when the output format is not strictly controlled.

These results underline that the observed gains are more the result of fine prompt engineering than a genuine generalization capability of the model, calling for cautious interpretation in sensitive contexts such as healthcare. This work thus highlights the potential of prompt engineering as an operational lever, while recalling the necessity of rigorous controls, qualitative analyses, and complementary evaluations to envision a reliable and scientifically interpretable use of LLMs in medical classification.
