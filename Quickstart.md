# Quickstart | RagaAI Catalyst

**This quickstart guide walks you through the installation and setup of RagaAI Catalyst.**

---

## **1. Install RagaAI Catalyst**

To install the RagaAI Catalyst package, run the following command in your terminal:

```bash
pip install ragaai-catalyst
```

---

## **2. Set Up Authentication Keys**

### **Obtain API Keys:**
1. Log in to your account at [RagaAI Catalyst](https://catalyst.raga.ai/).
2. Navigate to **Profile Settings** → **Authentication**.
3. Click **Generate New Key** to obtain your **Access Key** and **Secret Key**.

### **Initialize the SDK**

To begin using Catalyst, initialize it as follows:

```python
from ragaai_catalyst import RagaAICatalyst

catalyst = RagaAICatalyst(
    access_key="YOUR_ACCESS_KEY",  # Replace with your access key
    secret_key="YOUR_SECRET_KEY",  # Replace with your secret key
    base_url="BASE_URL"  # Instance URL
)
```

---

## **3. Create Your First Project**

Create a new project and choose a use case from the available options:

```python
# Create a new project
project = catalyst.create_project(
    project_name="Test-RAG-App-1",
    usecase="Chatbot"  # Options : Chatbot, Q/A, Others, Agentic Application
)

# List available use cases
print(catalyst.project_use_cases())
```

### **Add a Dataset**
Use the project name to initialize the dataset manager and create a dataset from a CSV file, DataFrame, or JSON file.

Define a **schema mapping** for the dataset.

```python
from ragaai_catalyst import Dataset

# Initialize dataset manager
dataset_manager = Dataset(project_name="Test-RAG-App-1")

# Create dataset from a CSV file
dataset_manager.create_from_csv(
    csv_path="path/to/your.csv",
    dataset_name="MyDataset",
    schema_mapping={
        'column1': 'schema_element1',
        'column2': 'schema_element2'
    }
)

# View dataset schema
print(dataset_manager.get_schema_mapping())
```

---

## **4. Trace Your Application**

You can enable real-time observability and tracing in two ways:

1. Using the `with tracer()` function.
2. Manually starting and stopping the tracer with `tracer.start()` and `tracer.stop()`.


```python
from ragaai_catalyst import Tracer

# Initialize production tracer
tracer = Tracer(
    project_name="Test-RAG-App-1",
    dataset_name="tracer_dataset_name",
    tracer_type="tracer_type"
)

# Start a trace recording (Option 1)
with tracer():
    # Your code here

# Start a trace recording (Option 2)
tracer.start()

# Your code here

# Stop the trace recording
tracer.stop()

# Verify data capture
print(tracer.get_upload_status())
```

---

## **5. Evaluation Framework**

Follow these steps to configure evaluation:
1. Import `Evaluation` from `ragaai_catalyst`.
2. Configure evaluation metrics.
3. Add metrics from the available options.
4. Check the status and retrieve results after running the evaluation.

```python
from ragaai_catalyst import Evaluation

# Initialize evaluation engine
evaluation = Evaluation(
    project_name="Test-RAG-App-1",
    dataset_name="MyDataset"
)

# Configure evaluation metrics
schema_mapping = {
    'Query': 'prompt',
    'response': 'response',
    'Context': 'context',
    'expectedResponse': 'expected_response'
}

evaluation.add_metrics(
    metrics=[
        {
            "name": "Faithfulness",
            "config": {"model": "gpt-4o-mini", "provider": "openai", "threshold": {"gte": 0.232323}},
            "column_name": "Faithfulness_v1",
            "schema_mapping": schema_mapping
        }
    ]
)

# Monitor evaluation
print(f"Status: {evaluation.get_status()}")
print(f"Results: {evaluation.get_results()}")
```

---

## **Next Steps**
- **Explore the Dashboard:** Visualize metrics and insights in the RagaAI Web UI.

---

**Version:** 1.0.0 | **Last Updated:** Mar 2025
