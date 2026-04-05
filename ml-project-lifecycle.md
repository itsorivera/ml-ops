# Machine Learning Project Life Cycle

![ML Project Life Cycle](./w1-deployment/assets/ml-lifecycle.png)

*Source: [Machine Learning in production by Andrew Ng - DeepLearning.AI](https://www.deeplearning.ai/courses/machine-learning-in-production/)*  

### 1. Scoping

- **Define the project**: Decide on the specific application (e.g., Speech Recognition for Voice Search).
- **Key Metrics**:
  - **Accuracy**: How well the system performs.
  - **Latency**: Time taken to process (e.g., transcribe speech).
  - **Throughput**: Queries per second (QPS).
- **Resources**: Estimate time, compute budget, and timeline.

### 2. Data

- **Label Consistency**: Crucial for algorithm performance. All transcriptionists/labelers must follow the same conventions (e.g., how to handle stutters or background noise).
- **Data Definition**: Establish clear rules for data (e.g., amount of silence padding, volume normalization).
- **Data-Centric AI**: In production, it is often more effective to improve the data quality (edit training/test sets) rather than treating the dataset as fixed.

### 3. Modeling

- **Core Inputs**: Code (architecture), Hyperparameters, and Data.
- **Model-Centric vs. Data-Centric**:
  - _Model-Centric_: Hold data fixed, vary code/hyperparameters (common in research).
  - _Data-Centric_: Hold code fixed, focus on optimizing data (often more effective for product teams).
- **Targeted Improvement**: Use **Error Analysis** to decide exactly what kind of data to collect or improve, rather than just collecting "more" data blindly.

### 4. Deployment

- **Common Pattern (Edge/Cloud)**:
  - **Edge Device**: Runs **VAD (Voice Activity Detection)** to select relevant audio.
  - **Cloud Server**: Receives audio, runs prediction, and returns results.
- **Challenges**:
  - **Data Drift / Concept Drift**: When the incoming data distribution changes over time (e.g., a system trained on adult voices starts receiving many younger voices), degrading performance.
- **Monitoring**: Implement monitors to spot drift early and maintain the system by feeding live data back into the lifecycle.
