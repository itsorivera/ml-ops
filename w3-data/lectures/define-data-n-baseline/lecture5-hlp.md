# Human-Level Performance (HLP)

Human-Level Performance (HLP) is a powerful tool for establishing baselines, but it is often misunderstood or misused. This lecture explores its true utility and the pitfalls of using it as a sole metric for success.

## 1. The Real Utility of HLP

- **Estimating Bayes' Error:** HLP serves as a proxy for irreducible error, especially in unstructured data tasks like image or speech recognition.
- **Establishing Feasibility:** It provides a reality check for business requirements. If human experts are only 67% accurate on a task, it is unrealistic to expect a model to reach 99% without major changes to the input data.
- **Prioritizing Efforts:** Understanding HLP helps teams focus on areas where the gap between current model performance and the baseline is largest.

## 2. Common Pitfalls and Misuses

- **Academic Benchmarking:** While "beating HLP" is a popular goal for publishing papers, it doesn't always translate to real-world value.
- **Proving Superiority:** Using HLP to prove that AI is "better than humans" is often counterproductive. Businesses require more than just average accuracy (e.g., handling edge cases, interpretability, and reliability).
- **The "Unfair Advantage" Trap:** Inconsistent labels can give models a "fake" lead over humans. For example, if two labeling conventions are equally valid but one is used 70% of the time, a model that consistently picks the majority choice will appear to "outperform" human agreement, even if its outputs aren't actually better.

## 3. The Masking Effect

A major danger of focusing solely on HLP is that strong performance on ambiguous, low-impact tasks can mathematically "mask" critical failures in other areas. A model might appear superior on average while actually producing lower-quality results for users on high-stakes inputs.

## Summary: Building vs. Benchmarking

Instead of obsessing over beating HLP, practical ML developers should focus on **raising HLP**. By improving label consistency and instructions, you provide the model with a cleaner signal, which ultimately drives real-world performance higher.

_In the next lecture, we will dive deeper into how raising HLP through better data definitions can lead to superior model performance._
