# Code Reading Note — TMMLab ICML 2023 Repository

**Related Paper:** *Which is Better for Learning with Noisy Labels: The Semi-supervised Method or Modeling Label Noise?*  

**Source code:** - [TMMLab ICML 2023 Repository](https://github.com/tmllab/2023_ICML_Which-is-Better-for-Learning-with-Noisy-Labels)

**Repository Focus:** code-level understanding of noisy-label learning, transition-matrix estimation, and CDNL-related pseudo-label construction

## Overall Impression

My overall impression is that the repository is **more visibly centered on the model-based side** than on a full SSL training pipeline. The most explicit code path I examined is based on **transition-matrix estimation**, especially through `run_dnl.py`, while the paper-level comparison between SSL-based and model-based methods is conceptually broader than the most visible public training path in the repository.

## Main Files I Looked At

- `main_cifar10.py`
- `main_uci.py`
- `run_dnl.py`
- `kmeans.py`

## My Current Understanding of the Code Structure

```text
main_*.py
→ parse arguments
→ load noisy data
→ choose a method entry point
→ run the experiment

run_dnl.py
→ train a classifier for latent clean-label prediction
→ train a transition matrix T
→ map clean predictions to noisy predictions
→ fit observed noisy labels
→ output an estimated transition matrix

kmeans.py
→ cluster X in feature space
→ convert cluster IDs into pseudo labels Y'
→ estimate P(~Y | Y')
→ support the CDNL logic
```
## What I Learned from run_dnl.py

The DNL path is clearly model-based noisy-label learning rather than sample filtering.
```text
X
→ classifier predicts latent clean-label distribution
→ transition matrix T models clean-to-noisy corruption
→ clean @ T gives predicted noisy-label distribution
→ compare with observed noisy labels
→ jointly update classifier and T
```
This helped me understand that the model does not know which labels are truly clean. Instead, it assumes a latent clean-label structure and learns a corruption process that explains the observed noisy labels.

## What I Learned from kmeans.py

The k-means component is not the main classifier. Instead, it is used to generate clustering-based pseudo labels from the feature space.
```text
X
→ K-means clustering
→ cluster IDs
→ align clusters into pseudo labels Y'
→ estimate P(~Y | Y')
```
This part helped me connect the paper’s CDNL idea to the implementation: one route estimates noisy-label structure from noisy-label modeling, while the other route estimates it from clustering-based pseudo labels.

## My Code-Level Takeaway

The repository helped me see the paper more concretely. The paper compares SSL-based methods, model-based methods, and CDNL as a detector, but the code I read most directly instantiates the model-based / transition-matrix side and the clustering-based pseudo-label side. This made me realize that code organization does not always mirror paper organization one-to-one; reading both together is necessary to understand the full methodological picture.

## My Reflection

This code reading made me more aware of an important research habit: when reading a repository, I should distinguish between the experiment entry files, the data construction layer, the actual training implementation, and the auxiliary estimation modules.
That distinction helped me avoid confusing the main noisy-label learner with the clustering-based estimator used in CDNL.
