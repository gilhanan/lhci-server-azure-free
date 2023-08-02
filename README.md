# Free Lighthouse CI Server on Azure

## Description

This repository provides everything needed to create and maintain a free [Lighthouse CI Server](https://github.com/GoogleChrome/lighthouse-ci) hosted on a  Microsoft's Azure platform, offering a seamless integration with GitHub Actions for continuous integration and deployment processes.
In this free solution, the Lighthouse data is stored in the App Service file system, which is ephemeral. To ensure that your Lighthouse CI Server data is not lost, you should use a persistent solution, see [Lighthouse CI Server Azure Database MySQL](https://github.com/gilhanan/lhci-server-azure).


### Components

- **Azure Web App**: Free Linux-based web application based on the `patrickhulce/lhci-server:latest`.

## Prerequisites

- [Azure Account](https://azure.microsoft.com)
- [GitHub Account](https://github.com)

## Setup

### 2. Create an Azure Subscription

If you do not have an Azure subscription, create a new one:

1. Log in to the [Azure Portal](https://portal.azure.com).
2. Click on **Subscriptions** in the left sidebar.
3. Click on **Add** at the top of the page.
4. Follow the steps to create a new subscription.
5. Note down the subscription id.

### 3. Create an Azure Active Directory (AAD) Application

The AAD Application acts as an identity for the GitHub Actions workflow to interact with Azure services.

1. Log in to the [Azure Portal](https://portal.azure.com).
2. Search for **Azure Active Directory** in the search bar at the top and select it.
3. Click on **App registrations** and then **New registration**.
4. Enter a name for the application, and then click **Register**.
5. Once the application is created, note down the **Application (client) ID** and **Directory (tenant) ID**.

### 4. Create a new AAD Application secret

This secret acts as a key for authenticating and allowing the GitHub Actions workflow to interact securely with Azure services.

1. Search for and select **Azure Active Directory**.
2. Select **App registrations** and select your application from the list.
3. Select **Certificates & secrets**.
4. Select **Client secrets**, and then select **New client secret**.
5. Provide a description of the secret, and a duration.
6. Select Add.
7. Note down the secret value.

### 4. Assign Contributor Role to the AAD Application

Allows the AAD Application to have the necessary permissions to create, manage, and delete resources within the specified Azure subscription.

1. Navigate to your Azure subscription.
2. Select **Access control (IAM)**, then click **Add role assignment**.
3. In the **Role** pane, select **Privileged administrator role** and select _Contributor_ as the role
4. Click **Next** or **Members**, select **Assign access to** to **User, group, or service principal**, and select the AAD application that you created earlier.
5. Click **Assign**.

### 5. Set GitHub Secrets

These secrets enable the GitHub Actions workflow to authenticate and interact with Azure, maintaining the security and integrity of sensitive information.
In your GitHub repository, go to **Settings** -> **Secrets and variables** -> **Actions** -> **New repository secret** and add the following secrets:

- `AZURE_APPLICATION_ID`: The Application (client) ID of the AAD application.
- `AZURE_APPLICATION_ID_PASSWORD`: The secret value of the AAD application.
- `AZURE_TENANT_ID`: The Directory (tenant) ID of the AAD application.
- `AZURE_SUBSCRIPTION_ID`: The Subscription ID where you want to deploy the resources.
- `AZURE_RESOURCE_GROUP`: Generate a name for the Azure resource group where you want to deploy the resources.
- `AZURE_WEB_APP_NAME`: Generate a name for the Web App service. Must be unique across the internet

### 6. Run the GitHub Action Workflow

Once everything is set up, you can run the workflow manually from the **Actions** tab in your GitHub repository, or it will run automatically whenever there's a push to the `main` branch.

## Official documentations

- [Azure Subscription](https://learn.microsoft.com/azure/cost-management-billing/manage/create-subscription)
- [Azure AAD Application](https://learn.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)
- [Azure AAD Application secret](https://learn.microsoft.com/azure/active-directory/develop/quickstart-register-app#add-a-client-secret)
- [Azure AAD assign Azure roles](https://learn.microsoft.com/azure/role-based-access-control/role-assignments-portal)
- [GitHub Actions Secrets](https://docs.github.com/rest/actions/secrets)
- [GitHub Actions](https://docs.github.com/actions/using-workflows/about-workflows)

## Pricing

For information on pricing, please refer to the following links:

- [Azure Web App](https://azure.microsoft.com/pricing/details/app-service/)
