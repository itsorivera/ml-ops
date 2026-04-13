# A Framework for ML Projects: The 2x2 Grid

Machine learning best practices are not universal; they vary significantly depending on the project type. This lecture introduces a framework to categorize projects based on data type and dataset size.

## 1. The 2x2 Categorization Grid

ML projects can be mapped along two primary axes:

![2x2 Grid](/w3-data/lectures/assets/2x2-grid.png)

_Source: [Machine Learning in production by Andrew Ng - DeepLearning.AI](https://www.deeplearning.ai/courses/machine-learning-in-production/)_

- **Data Type:** Unstructured (Images, Audio, Text) vs. Structured (Database records, spreadsheets).
- **Dataset Size:** Small (e.g., < 10,000 examples) vs. Large (e.g., > 10,000 examples).

The 10,000-example threshold is a practical limit where it becomes difficult for a single person to manually inspect every data point.

## 2. Unstructured vs. Structured Data

- **Unstructured Data:** Humans are naturally adept at processing these inputs. Techniques like **data augmentation** (synthesizing new images/audio) are highly effective and widely used.
- **Structured Data:** These are harder for humans to interpret and significantly harder to augment. You are generally restricted to the data you can naturally collect (e.g., number of houses sold or users acquired).

## 3. Small Data vs. Big Data

- **Small Data (Criticality of Clean Labels):** When you have 100 examples, one mislabeled point represents 1% of your data. Meticulous, manual cleaning and ensuring labeling consistency among a small team is the highest priority.
- **Big Data (Emphasis on Data Process):** With millions of examples, manual review is impossible. The focus shifts to **scalable data processes**: robust collection methods, rigorous storage standards, and clear, standardized instructions for large crowdsourced labeling teams.

## 4. Key Takeaways for ML Engineers

- **Advice is Quadrant-Specific:** Practical tips for a "Big Data / Structured" problem (like ad recommendations) rarely apply to a "Small Data / Unstructured" problem (like rare disease detection).
- **Generalization of Skills:** Expertise usually generalizes better within the same quadrant. When hiring or seeking advice, look for experience in the specific quadrant of your project.

## Summary

Identifying where your project sits on this grid helps you choose the right tools and strategies. Whether you should focus on manual data cleaning or scalable process engineering depends entirely on your data type and volume.

_In the next lecture, we will dive deeper into why clean labels are especially vital for small data problems._
