# Reading Note — ICML 2023

**Title:** *Which is Better for Learning with Noisy Labels: The Semi-supervised Method or Modeling Label Noise?*  
**Authors:** Yu Yao, Mingming Gong, Yuxuan Du, Jun Yu, Bo Han, Kun Zhang, Tongliang Liu  
**Keywords:** noisy-label learning, semi-supervised learning, model-based methods, transition matrix, causal data-generating process, anticausal learning, CDNL

## What I Focused on Most

My main focus in this paper is the three-way structure formed by **CDNL as a causal-structure detector**, **SSL-based methods**, and **model-based noisy-label learning** The paper argues that the relative usefulness of SSL-based methods and model-based methods depends on the underlying causal data-generating process. In particular, SSL benefits from unlabeled data only when the feature distribution \(P(X)\) contains useful information about \(P(Y \mid X)\), which is more likely under an anticausal structure \(Y \to X\). 

### Three Lines I Learned from This Paper

```text
1.CDNL
X + noisy labels
→ estimate P(~Y | Y*)
→ cluster X to obtain pseudo labels Y'
→ estimate P(~Y | Y')
→ compare the gap between the two matrices
→ infer whether the dataset is more likely X -> Y or Y -> X

2. SSL-based noisy-label learning
noisy data
→ split into confident labeled set + unconfident unlabeled set
→ keep labels for confident samples
→ discard noisy labels for unconfident samples
→ use SSL techniques on unlabeled X
→ improve generalization if P(X) contains useful class information

3. Model-based noisy-label learning
X
→ classifier predicts latent clean-label distribution
→ transition matrix T maps clean labels to noisy labels
→ predicted noisy labels are matched with observed noisy labels
→ jointly update classifier + T
→ recover a classifier closer to the clean decision rule
```
## My Takeaway

My biggest conceptual breakthrough from this paper is that method choice should follow data-generating assumptions rather than benchmark habit. Before reading it, I tended to think of noisy-label learning as a competition between methods. After reading it, I see a more structural question: when does unlabeled feature structure actually help classification, and when is it better to explicitly model the corruption process? This paper made me understand that SSL is not universally useful, and that causal direction can explain why different noisy-label methods succeed under different conditions.

## Mind Map

```text
Which is Better for Learning with Noisy Labels?
├── Research Purpose
│   ├── Compare SSL-based methods and model-based methods
│   └── Ask when each family is more suitable
│
├── Core Idea
│   ├── The answer depends on the causal data-generating process
│   ├── Causal: X -> Y
│   └── Anticausal: Y -> X
│
├── Two Method Families
│   ├── SSL-based methods
│   │   ├── split data into confident labeled + unconfident unlabeled sets
│   │   └── use unlabeled data for semi-supervised learning
│   └── Model-based methods
│       ├── estimate a transition matrix T
│       └── model clean-to-noisy corruption explicitly
│
├── Proposed Method
│   └── CDNL
│       ├── estimate P(~Y | Y*)
│       ├── cluster X to get pseudo labels Y'
│       ├── estimate P(~Y | Y')
│       └── compare the gap between the two matrices
│
├── Main Result
│   ├── causal datasets -> model-based methods work better
│   ├── anticausal datasets -> SSL-based methods work better
│   └── CDNL helps detect the underlying structure
│
└── My Insight
    ├── method choice should follow assumptions
    ├── SSL is not universally beneficial
    └── hybrid approaches may be promising
