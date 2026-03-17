# Reading Note — ICML 2023  
**Paper:** Which is Better for Learning with Noisy Labels: The Semi-supervised Method or Modeling Label Noise?

**Authors:** Yu Yao, Mingming Gong, Yuxuan Du, Jun Yu, Bo Han, Kun Zhang, Tongliang Liu

**My focus:** noisy-label learning, causal data-generating process, method selection, and code-level understanding.


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
│   │   ├── split data into confident labeled + unconfident unlabeled
│   │   └── use unlabeled data for semi-supervised learning
│   └── Model-based methods
│       ├── estimate transition matrix T
│       └── model clean-to-noisy corruption explicitly
│
├── Main Argument
│   ├── If X -> Y
│   │   ├── P(X) gives little information about P(Y|X)
│   │   └── SSL-based methods are less helpful
│   └── If Y -> X
│       ├── P(X) contains information about P(Y|X)
│       └── SSL-based methods can help more
│
├── Proposed Method
│   └── CDNL
│       ├── estimate P(~Y | Y*)
│       ├── cluster X to get pseudo labels Y'
│       ├── estimate P(~Y | Y')
│       └── compare the gap between the two matrices
│
├── Results
│   ├── causal datasets -> model-based methods work better
│   ├── anticausal datasets -> SSL-based methods work better
│   └── CDNL detects causal structure reasonably well
│
├── Contributions
│   ├── explains noisy-label methods from a causal perspective
│   ├── gives a principle for method selection
│   └── proposes CDNL for causal-structure detection
│
├── Limitations
│   ├── depends on clustering quality
│   ├── threshold is heuristic
│   └── some datasets like Waveform are not fully explained
│
└── My Takeaway
    ├── method choice should follow data-generating assumptions
    ├── SSL is not universally useful
    └── hybrid methods may be a promising next step
