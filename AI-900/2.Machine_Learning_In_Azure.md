# AI-900 — Core ML Concepts (Condensed Notes)

## 1. ML Basics — Training vs Inferencing

- ML model = function:
      y = f(x)
      x = features (inputs)
      y = label (output)
      ŷ (“y-hat”) = predicted label

- **Training phase**:
      Use historical data with:
      - Features (x)
      - Known labels (y)
      Learn f(x) that maps x → y

- **Inferencing phase**:
      Use trained model on new x → get ŷ (prediction)

![alt text](image-1.png)

## 2. Supervised vs Unsupervised Learning

- **Supervised**:
      Training data includes labels (y)
      Goal: learn relationship between x and y
      Types:
      - Regression (numeric y)
      - Classification (categorical y)

- **Unsupervised**:
      Training data has only features (x), no labels
      Goal: find structure/patterns in data
      Type:
      - Clustering (group similar items)

![alt text](image-2.png)

## 3. Supervised Learning Types

### 3.1 Regression (Numeric Prediction)

![alt text](image-4.png)

- Predict **continuous numeric** label (y)
      Examples:
      - Price, sales quantity, fuel efficiency, etc.

- Typical process:
      - Split data into train / validation
      - Train model on training set
      - Predict on validation set
      - Compare predicted ŷ vs actual y
      - Repeat with different features/algorithms/hyperparameters

#### Key Regression Metrics

- **MAE (Mean Absolute Error)**:
      Average of |y - ŷ|
      “On average, how far off are we?”

- **MSE (Mean Squared Error)**:
      Average of (y - ŷ)²
      Penalizes large errors more

- **RMSE (Root Mean Squared Error)**:
      √MSE
      Error in original units (e.g., “ice creams”)

- **R² (Coefficient of Determination)**:
      Proportion of variance explained by model
      Range: 0 → 1
      Closer to 1 = better fit


### 3.2 Classification (Categorical Prediction)

- Predict **category / class** label

#### Binary Classification

- Two possible outcomes (e.g., yes/no, 0/1, positive/negative)
- Model outputs **probability** P(y=1 | x)
- Uses logistic/sigmoid-style function:
      probability between 0 and 1
- **Threshold** (often 0.5):
      - ŷ = 1 if P ≥ threshold
      - ŷ = 0 if P < threshold

![alt text](image-5.png)

##### Confusion Matrix Terms

- TP (True Positive): predicted 1, actual 1
- TN (True Negative): predicted 0, actual 0
- FP (False Positive): predicted 1, actual 0
- FN (False Negative): predicted 0, actual 1

![alt text](image-6.png)

##### Important Metrics

- **Accuracy**:
      (TP + TN) / (TP + TN + FP + FN)
      “Overall, how often correct?”

- **Recall** (Sensitivity, TPR):
      TP / (TP + FN)
      “Of all actual positives, how many did we catch?”

- **Precision**:
      TP / (TP + FP)
      “Of all predicted positives, how many were truly positive?”

- **F1-score**:
      Harmonic mean of precision & recall
      Good when you need balance between precision and recall

- **ROC / AUC**:
      - ROC curve: TPR vs FPR across all thresholds
      - AUC:
            1.0 = perfect
            0.5 = random guessing
            Higher AUC = better classifier overall

## Area Under the Curve (AUC) Example
![alt text](image-7.png)

#### Multiclass Classification

- More than two classes (e.g., species: Adelie, Gentoo, Chinstrap)
- Model estimates probability for **each** class; pick the highest

![alt text](image-9.png)

##### Common Approaches

- **One-vs-Rest (OvR)**:
      Train one binary classifier per class
      f0(x) = P(y=0 | x)
      f1(x) = P(y=1 | x)
      f2(x) = P(y=2 | x)
      Predict class with highest probability

- **Multinomial / Softmax**:
      Single function outputs probability vector:
      f(x) = [P(y=0|x), P(y=1|x), P(y=2|x)]
      Probabilities sum to 1

##### Evaluation

- Multiclass **confusion matrix**:
      Rows = actual classes
      Columns = predicted classes


![alt text](image-10.png)

- Can compute:
      - Accuracy
      - Precision / Recall / F1 for each class
      - Overall precision/recall/F1 (aggregate)


## 4. Unsupervised Learning — Clustering

- No labels; groups items by similarity of features

### Clustering Basics

- **Goal**: assign each data point to a cluster
      Cluster label is learned from structure in data

- **Common algorithm: K-Means**

      1. Choose number of clusters **k**
      2. Initialize k random centroids
      3. Assign each point to nearest centroid
      4. Move centroids to mean of their assigned points
      5. Reassign points to nearest centroid
      6. Repeat 4–5 until clusters stabilize or max iterations reached

### Clustering Evaluation (No Labels)

![alt text](image-11.png)

- Measures how well clusters are separated:

      - Average distance to cluster center
      - Average distance to other cluster centers
      - Maximum distance to cluster center
      - **Silhouette score**:
            -1 to 1
            Closer to 1 = better cluster separation

![alt text](image-12.png)

## 5. Deep Learning — Neural Networks

- **Deep learning** = ML with **deep neural networks (DNNs)**
- Inspired by biological neurons, but implemented as math

![alt text](image-13.png)

### Neural Network Basics

- A network of **layers**:
      - Input layer (features x)
      - Hidden layers
      - Output layer (predicted y or probability vector)

- Each neuron:
      - Takes inputs x with weights w
      - Computes weighted sum
      - Passes result through **activation function**
      - Decides what signal to pass forward

- Used for:
      - Regression
      - Classification
      - NLP
      - Computer vision
      - Many advanced scenarios


## Example - Using deep learning for classification
![alt text](image-14.png)

### DNN as Nested Function

- Overall model: ŷ = f(x)
- Internally: many nested functions with weights (w)
- Training learns the best weights to minimize error (loss)

### Training Neural Networks — Key Ideas

1. **Forward pass**:
      - Feed training features (x) through network
      - Compute outputs ŷ

2. **Compute loss**:
      - Compare ŷ vs actual y with a loss function
      - Higher loss = worse predictions

3. **Backpropagation + Gradient Descent**:
      - Compute how each weight w affects the loss
      - Adjust weights w up/down to reduce loss
      - This is gradient descent-based optimization

4. **Iterate over epochs**:
      - Repeat forward pass + backprop many times
      - Use batches of training data
      - Stop when loss is acceptably low / performance is good

![alt text](image-15.png)

### Hardware Note

- Training DNNs uses **vector/matrix math**
- Often runs on **GPUs** for speed and efficiency


## 6. High-Level Summary (Exam-Speed Recall)

- Model = f(x) → y; training = learn f; inferencing = use f
- Supervised:
      - Regression → numeric y (MAE, MSE, RMSE, R²)
      - Classification → categorical y
            - Binary: confusion matrix, accuracy, precision, recall, F1, AUC
            - Multiclass: OvR or softmax, multiclass confusion matrix
- Unsupervised:
      - Clustering (e.g., K-Means): no labels, group by similarity, silhouette score
- Deep learning:
      - Neural networks with layers & weights
      - Train via forward pass, loss, backprop, gradient descent
      - Great for complex tasks (vision, NLP, etc.)


# AI-900 — ML Lifecycle & Azure ML (Condensed Notes)

## 1. Define the Problem

- Clarify:
      - What output should the model produce?
      - Which ML task fits?
      - How you’ll measure “success” (metrics).

### Common ML Tasks

- **Classification** → predict a **category** (yes/no, class label).
- **Regression** → predict a **numeric** value.
- **Time-series forecasting** → predict **future numeric values over time**.
- **Computer vision** → classify/detect objects in images.
- **NLP** → extract insights from text (classification, sentiment, entities, etc.).

![alt text](image-16.png)

### Example: Diabetes Prediction

- Data: patient health metrics (features).
- Output: diabetic vs not diabetic → **categorical**.
- Task: **classification** (binary).

![alt text](image-17.png)


## 2. Standard ML Workflow (Big-Picture)

1. **Load data** → import / inspect.
2. **Preprocess data** → clean, normalize, handle missing values.
3. **Split data** → train vs test (and sometimes validation).
4. **Choose model** → pick algorithm & settings.
5. **Train model** → learn patterns from training data.
6. **Score model** → generate predictions on test data.
7. **Evaluate** → metrics (accuracy, precision, recall, etc.).

- Training is **iterative**: repeat steps with different features/algorithms/params.


## 3. Get & Prepare Data

### 3.1 Identify Data Source & Format

- **Source examples**:
      - CRM systems
      - SQL / transactional DBs
      - IoT devices / telemetry

- **Formats**:
      - Structured/tabular (tables)
      - Semi-structured (JSON, logs)
      - Unstructured (images, audio, text, video)

### 3.2 Data Ingestion & ETL/ELT

- Common pattern:
      - **Extract Raw Data** data from source
      - **Copy & Transform Data with Azure Synapse Analytics** (clean, aggregate, shape)
      - **Store Data in Azure Blob Storage**
      - **Load to Train Azure Machine Learning** into serving layer (for ML)
- Often called **ETL** or **ELT**.

![alt text](image-18.png)

![alt text](image-19.png)

### 3.3 Azure Data Ingestion Tools

- **Azure Synapse Analytics** → pipelines, data integration, transformation.
- **Azure Databricks** → Spark-based data engineering & prep.
- **Azure Machine Learning** → pipelines & data prep for ML.

### Typical Pipeline Example

- Extract raw data (e.g., from CRM or IoT).
- Copy/transform with **Synapse** or **Databricks**.
- Store prepared data in **Azure Blob Storage** (or Lake).
- Train model with **Azure Machine Learning**.

### Weather Forecast Example (IoT)

- Raw: JSON temperature readings from IoT.
- Convert JSON → table.
- Aggregate to get **average temperature per hour/machine**.
- Result: clean tabular dataset ready for **time-series forecasting**.


## 4. Train the Model — Azure Services

### 4.1 Azure ML-Related Services

- **Azure Machine Learning**
      - Full ML lifecycle: data → train → deploy → manage
      - UI (Studio) or code-first (Python SDK / CLI)

- **Azure Databricks**
      - Spark-based analytics & ML
      - Can integrate with Azure ML for model management/deployment

- **Microsoft Fabric**
      - Unified analytics platform (lakehouse, notebooks, ML, Power BI)
      - End-to-end: prep data, train model, serve predictions, report in Power BI

- **Foundry Tools**
      - Prebuilt ML APIs (e.g., object detection)
      - Easy app integration; some allow custom training with own data

![alt text](image-20.png)

### 4.2 Azure Machine Learning — Capabilities

- Centralized datasets for training/evaluation
- On-demand **compute** for training / jobs
- **AutoML** for automated model selection & tuning
- Visual **pipelines** for training & inferencing workflows
- Integration with frameworks like **MLflow**
- Built-in tooling for **Responsible AI**:
      - explainability, fairness, etc.


## 5. Azure Machine Learning Studio

- Browser-based UI for managing ML resources & jobs.


![alt text](image-21.png)

### With AML Studio You Can:

- Import & explore data
- Create/manage **compute** (CPU/GPU clusters, etc.)
- Run **notebooks**
- Build **pipelines** visually
- Run **Automated ML** experiments
- View trained models, metrics, and responsible AI info
- Deploy models for **real-time** or **batch** inferencing
- Browse / import models from model catalog

### Workspace & Resources

- Core resource = **Azure ML workspace**
- Workspace automatically wires up supporting resources:
      - Storage, container registry, compute, etc.
- Created in the **Azure portal** or via scripts.


## 6. Compute Choices in Azure ML

### 6.1 CPU vs GPU

- **CPU**:
      - Good for small/medium **tabular** datasets
      - Cheaper and often enough for classical ML

- **GPU**:
      - Ideal for **images**, **text** (deep learning), and large datasets
      - Also for very large tabular models when CPU is too slow

### 6.2 General Purpose vs Memory Optimized

- **General Purpose**:
      - Balanced CPU:memory
      - Good for dev, testing, and smaller jobs

- **Memory Optimized**:
      - High memory:CPU
      - Good for large in-memory datasets & heavy notebook workloads

### 6.3 Monitoring & Scaling

- Always **monitor**:
      - Training time
      - CPU/GPU utilization
      - Memory usage

- If training is too slow:
      - Scale up (larger VM)
      - Scale out (distribute with Spark)
      - Consider GPU if appropriate

### 6.4 AutoML Compute

- Using **Azure AutoML**:
      - Compute is automatically managed/assigned for the experiment
      - Auto runs many combinations of algorithms/parameters


## 7. AutoML (Azure Automated ML)

- Automates **algorithm selection**, **feature engineering**, and **hyperparameter tuning**.
- No-code/low-code path in AML Studio (wizard-based).
- Supports multiple tasks:
      - Regression
      - Time-series forecasting
      - Classification
      - Computer vision
      - NLP
- Can deploy best model as a service directly from the UI.


## 8. Integrate & Deploy the Model

- Goal: make model available to apps/services via an **endpoint**.

### 8.1 Endpoint Types

- **Real-time endpoint**:
      - For low-latency, per-request predictions via REST API.
- **Batch endpoint**:
      - For scoring large datasets in bulk, on a schedule or trigger.

### 8.2 Real-Time Predictions — When?

- Need fast predictions **as data arrives**.
- Common for:
      - Web/mobile apps
      - Realtime recommenders
      - Fraud checks at transaction time

- Example:
      - User opens product page (shirt)
      - Model immediately returns recommendations (similar items)
      - Site displays suggestions along with product

![alt text](image-24.png)


### 8.3 Batch Predictions — When?

- Predictions run **periodically** on **bulk data**.
- Good for:
      - Weekly sales forecasts
      - Monthly churn predictions
      - Overnight scoring of customer database

- Example:
      - Orange juice sales forecast
      - Collect weekly sales data
      - Run model once per week
      - Save results in table for reporting

![alt text](image-23.png)

## 9. Real-Time vs Batch — Decision Factors

### Questions to Ask

- How often do I need predictions?
- How quickly must predictions be available?
- Individual or batch inputs?
- How much compute is required?
- Cost sensitivity?

### Frequency & Latency

- **Real-time**:
      - Need predictions immediately when data arrives
      - Example: scoring transactions on each purchase

- **Batch**:
      - OK to score periodically (hourly, daily, weekly)
      - Example: weekly sales or quarterly financial models

### Individual vs Batch Predictions

- **Individual**:
      - Single row / record per call
      - Example: one customer → “will churn?” yes/no

- **Batch**:
      - Many rows in one job
      - Example: entire customer table → churn predictions for all
      - Same idea with images: single image vs folder of images


![alt text](image-25.png)


## 10. Compute & Cost for Deployment

### Real-Time Endpoints

- Typically use:
      - **Azure Container Instances (ACI)**
      - **Azure Kubernetes Service (AKS)**

- Properties:
      - Always-on containers
      - Low-latency responses
      - You pay while service is running (even idle)

### Batch Endpoints

- Typically use:
      - **Compute clusters** that can auto-scale

- Properties:
      - Spin up nodes when batch job starts
      - Auto-scale down to 0 nodes when idle
      - Cost-efficient for infrequent, high-volume scoring

### High-Level Decision

- Need immediate, interactive responses? → **Real-time endpoint**.
- OK with scheduled/triggered jobs on larger datasets? → **Batch endpoint**.
