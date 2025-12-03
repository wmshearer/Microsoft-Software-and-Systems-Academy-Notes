# Explore Automated Machine Learning in Azure Machine Learning

## Windows & Azure Login

- [ ] Sign in to Windows as:
      Student / Pa55w.rd

- [ ] Sign in to Azure with:
      Username: LabUser-57181871@LODSPRODMCA.onmicrosoft.com
      TAP: =ZfFauzb

- [ ] Use **ResourceGroup1** for all Azure resources


## Create a Workspace

- [ ] Open one of the portals:
      Azure ML Studio → https://ml.azure.com
      ML Lab App → https://aka.ms/ml-lab

- [ ] If Azure ML Studio opens an existing workspace:
      Go to **All workspaces**

- [ ] Create a new workspace:
      Name: Project57181871
      Subscription: provided subscription
      Resource group: ResourceGroup1
      Region: westus

- [ ] After creation, open the workspace **Home** page


## Download Data

- [ ] Download:
      https://aka.ms/mslearn-ml-data

- [ ] Extract **ml-data.zip**
- [ ] Locate **ice-cream.csv**


## Train a Model with Automated Machine Learning

- [ ] In the ML portal, open:
      Automated ML (under Authoring)

- [ ] Create a new Automated ML job:
      Assign a unique job name


### Task Type & Data

- [ ] Task type:
      Regression

- [ ] Create a new tabular dataset:
      Name: ice-cream
      Upload: ice-cream.csv

- [ ] Include ONLY:
      DayOfWeek
      Month
      Temperature
      Rainfall
      IceCreamsSold

- [ ] Ensure **ice-cream** data asset is selected


### Task Settings

- [ ] Target column:
      IceCreamsSold

- [ ] Primary metric:
      R2 score

- [ ] Algorithms:
      Leave all selected or choose preferred ones


### Featurization

- [ ] Adjust settings as needed (defaults acceptable)


### Limits

- [ ] Metric score threshold: 0.9
- [ ] Experiment timeout: 15 minutes


### Compute

- [ ] Use **Serverless compute**


### Submit Training Job

- [ ] Review settings
- [ ] Submit the job
- [ ] Wait until completed


## Review the Best Model

- [ ] On the job’s **Overview** tab:
      Review the best model summary

- [ ] Select the best model’s **Algorithm name**

- [ ] Review:
      Overview
      Model
      Metrics
      Outputs + logs


## Deploy the Model

- [ ] On the **Model** tab:
      Select **Deploy** → Real-time endpoint

- [ ] Choose compute options appropriate for your available quota
- [ ] Provide endpoint and deployment names
- [ ] Wait for deployment to complete (5–10 mins)


## Test the Deployed Service

- [ ] Go to:
      Endpoints → your real-time endpoint

- [ ] Open the **Test** tab

- [ ] Replace template JSON with:

```json
{
 "input_data": {
    "columns": [
        "DayOfWeek",
        "Month",
        "Temperature",
        "Rainfall"
    ],
    "index": [0],
    "data": [["Wednesday","June",70.5,0.05]]
 }
}
```