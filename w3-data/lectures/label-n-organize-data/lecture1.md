# Data Acquisition and labeling Best Practices

Obtaining the right data efficiently is a cornerstone of successful ML projects. This lecture emphasizes speed of iteration, careful sourcing, and incremental growth.

## 1. Prioritize Iteration over Volume

Machine learning is inherently iterative. Instead of spending weeks on initial data collection, aim to enter the **Loop (Collect → Train → Error Analysis)** as quickly as possible.

- **Rule of Thumb:** Try to limit initial collection to a few days.
- **The Value of Scrappiness:** Small amounts of "scrappy" data let you use error analysis to determine exactly what kind of _more_ data you actually need.
- **Exception:** If you have deep experience in a domain (e.g., speech recognition) and know exactly what volume is required, a larger upfront investment may be justified.

## 2. Taking a Data Source Inventory

When planning data acquisition, brainstorm all sources and evaluate them across four criteria:

- **Financial Cost:** Price per hour or per label.
- **Time Cost:** How long it takes to integrate the software or execute the purchase order.
- **Data Quality:** Whether the data is natural or synthetic (e.g., people reading text vs. spontaneous speech).
- **Privacy & Regulation:** Ensuring absolute compliance with user privacy and legal standards.

## 3. Labeling Strategies

Choosing the right labelers depends on the complexity of the task:

- **In-House:** Best for early stages. ML engineers labeling their own data build critical intuition about the problem.
- **Outsource/Crowdsource:** Scalable options for general tasks (e.g., speech-to-text), but require rigorous management.
- **Subject Matter Experts (SMEs):** Essential for specialized domains like medical diagnosis or factory inspection where general labelers lack the necessary knowledge.
- **Automated/Purchase:** For tasks like product recommendations, user interaction data is often more reliable than human judgment.

## 4. The 10x Growth Rule

When expanding your dataset, avoid increasing it by more than **10x** in a single jump.

- Massive increases in data volume change the training dynamics significantly.
- It is often more efficient to grow by 2x or 5x, re-evaluate through error analysis, and then decide if further investment is necessary.

## Summary

The goal is to be as efficient as possible with both your time and budget. By starting small and growing incrementally, you avoid over-investing in data that might not be the highest priority for your model’s success.

_In the next lecture, we will explore best practices for building robust data pipelines to handle multi-step pre-processing._
