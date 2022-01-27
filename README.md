# AzurePolicyCompliance
Azure Policy Compliance Scan
1) Go to Your Azure CLI in your Azure Portal
```
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/{subscription-id} --sdk-auth
                            
  # Replace {subscription-id} with the subscription identifiers
  
  # The command should output a JSON object similar to this:

  {
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
  }
  
```
2) Go to Actions tab in GitHub create yml file.
```
# File: .github/workflows/workflow.yml

on: push

jobs:
  assess-policy-compliance:    
    runs-on: ubuntu-latest
    steps:
    # Azure Login       
    - name: Login to Azure
      uses: azure/login@v1
      with:
        creds: ${{secrets.AZURE_CREDENTIALS}} 
    
    - name: Check for resource compliance
      uses: azure/policy-compliance-scan@v0
      with:
        scopes: |
          /subscriptions/{subscription-id}
          
           # Replace {subscription-id} with the subscription identifiers
        
```
3) Copy the JSON File from Step 1 to the Settings Tab > Secrets > Actions > New Repository Secret > Enter the Name "AZURE_CREDENTIALS" and paste the JSON file and Save.
4) Result: Get the resut from Actions Tab workflow even the workflow is error, you will get the csv file (ScanReport_xxxxxxxxx.csv). 

** Reference **
1) https://github.com/azure/policy-compliance-scan
