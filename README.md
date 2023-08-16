# KNAUF_RNA_LAB

Sure, here's a detailed documentation for the provided code:

## Continuous Integration and Continuous Deployment (CI/CD) Workflow Documentation

This documentation provides a breakdown and explanation of the CI/CD workflow defined in the provided YAML file.

### Workflow Overview

This workflow automates the build, validation, export, and deployment process of Azure Data Factory (ADF) resources. It follows the steps below:

1. Checkout the code from the repository.
2. Generate a unique build name using a timestamp.
3. Set up the Node.js environment and install ADF Utilities package dependencies.
4. Cache the Node.js dependencies to improve build speed.
5. Validate Data Factory resources against an ADF instance.
6. Generate ARM templates for Data Factory resources.
7. Upload the generated artifact.
8. Download the artifact in the deployment job.
9. Login to Azure using the Az module.
10. Deploy Data Factory resources using the Azure Data Factory Deploy action.
11. Notify success.

### Workflow Configuration

The workflow is defined using a YAML file. Let's break down the various sections of the YAML file:

1. **Workflow Triggers:**
   - The workflow is triggered manually through the Actions tab in GitHub.

2. **Workflow Permissions:**
   - Specifies the permissions needed for the workflow, including write access to various resources.

3. **Workflow Jobs:**

   a. **Build Job:**
      - **Runs On:** Ubuntu latest version.
      - Steps:
         1. Checkout the repository.
         2. Generate a unique build name using the current timestamp.
         3. Print the generated build name.
         4. Set up the Node.js environment (version 14.x) and install ADF Utilities package dependencies.
         5. Cache Node.js dependencies using the actions/cache action.
         6. Install ADF Utilities package in the build folder.
         7. Validate Data Factory resources against a specified ADF instance using the npm run build validate command.
         8. Generate ARM templates for Data Factory resources using the npm run build export command.
         9. Upload the generated artifact using actions/upload-artifact action.

   b. **DeployToDevEnviornment Job:**
      - **Depends On:** Build Job.
      - **Runs On:** Ubuntu latest version.
      - Steps:
         1. Download the uploaded artifact using actions/download-artifact action.
         2. Log in to Azure using the Az module and provided secrets.
         3. Deploy Data Factory resources using the Azure Data Factory Deploy action.
         4. Notify success if the deployment is successful.

### Parameter Explanations:

1. **BUILD_NAME:** A unique identifier generated using the current timestamp, used throughout the workflow.
2. **AZURE_CLIENT_ID, AZURE_TENANT_ID, AZURE_SUBSCRIPTION_ID:** Azure authentication information stored as GitHub secrets for secure access.
3. **resourceGroupName, dataFactoryName:** Names of the target ADF resource group and Data Factory instance.
4. **ARMTemplateFile:** Path to the ARM template file for deployment.
5. **ARMTemplateParametersFile:** Path to the ARM template parameters file for deployment.

### Usage:

1. To trigger the workflow manually, go to the Actions tab in your GitHub repository and select the workflow named "cicd-dev". Click the "Run workflow" button.
2. The workflow will execute the build steps, perform validation and export, then deploy the generated artifact to the specified Azure Data Factory instance.
3. If all steps succeed, a "success" message will be displayed in the workflow log.

Please ensure that you have set up the necessary secrets and have the required Azure subscriptions and permissions to successfully execute the workflow.

Note: Be cautious when handling secrets, credentials, and sensitive data. Use GitHub secrets to store and manage such information securely. Additionally, review and adapt the workflow to your specific use case and requirements before using it in a production environment.
