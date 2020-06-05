
***
## Prerequisites

To begin, you will need:
* An Azure subscription where you can create the following kinds of resources:
    * Azure Logic App
    * App service
    * App service plan
    * Bot channels registration
    * Azure Cosmos DB account
    * Application Insights
* A copy of the Icebreaker app GitHub repo (https://github.com/officedev/microsoft-teams-icebreaker-app)
* This is not a pre-requisite per se, but here is a video walkthrough of this deployment if you'd like to follow along as you go through your own deployment. [Icebreaker video walkthrough](https://www.youtube.com/watch?v=BkoLT3MEtZg)

## Step 1: Use Registered Microsoft Azure AD application

When prompted for More information required click on the Skip for now(14 days until this is required link)

![](images/skip.png)

1. When the app is registered, you'll be taken to the app's "Overview" page. Copy the **Application (client) ID**; we will need it later. Verify that the "Supported account types" is set to **Multiple organizations**.

    ![Azure AD app overview page](images/app_overview2.png)  
    
Select **App Registrations** on the left-hand side and then select **Teams App Challenge**.

![](images/teamsappschallenge.png)  

On the Overview page copy and paste the **Application(client) ID** for use later

![](images/overview.png)  

Then click **Certificates & secrets** on the left navigation

Click on the New client secret button and create a new secret using your initials at the beginning

![](images/newsecret.png)  

Choose Never Expire

![](images/neverexpire.png)

When the dialog closes copy the value in the Client secret section. It will never be available again.

![](images/new%20secret.png)


## Step 2: Deploy to your Azure subscription

1. Click on the "Deploy to Azure" button below.

    [![Deploy to Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FOfficeDev%2Fmicrosoft-teams-icebreaker-app%2Fmaster%2FDeployment%2Fazuredeploy.json)

1. When prompted, log in to your Azure subscription.

1. Azure will create a "Custom deployment" based on the ARM template and ask you to fill in the template parameters.

    ![Custom deployment page](images/custom_deployment.png)

1. Select a subscription and choose the **Icebreaker** resource group.

**For the App name prefix Icebreaker with your initials**

1. Fill in the various IDs in the template:
    * **Bot App ID**: The Application (client) ID from the Azure AD application created above
    * **Bot App Password**: The client secret from the Azure AD application created above

    Make sure that the values are copied as-is, with no extra spaces. The template checks that the GUID is exactly 36 characters.

1. Agree to the Azure terms and conditions by clicking on the check box “I agree to the terms and conditions stated above” located at the bottom of the page.

1. Click on “Purchase” to start the deployment.

1. Wait for the deployment to finish. You can check the progress of the deployment from the "Notifications" pane of the Azure Portal.

1. Once completed, navigate to the App Service you have created (it should be of type Microsoft.Web/sites with a name similar to "icebreaker-XXXXXXXXXXXXX"). Copy its URL; we will need it later. It should be similar to "https://icebreaker-XXXXXXXXXXXXX.azurewebsites.net" where the X's are the hash.

    ![App service URL](images/app_service_url.png)

## Step 3: Create the Teams app package

1. Open the `Manifest\manifest.json` file in a text editor.

1. Change the placeholder fields in the manifest to values appropriate for your organization.
    * `developer.name` ([What's this?](https://docs.microsoft.com/en-us/microsoftteams/platform/resources/schema/manifest-schema#developer))
    * `developer.websiteUrl`
    * `developer.privacyUrl`
    * `developer.termsOfUseUrl`

1. Change the “botId” placeholder to your Azure AD application's ID from above. This is the same GUID that you entered in the template under “Bot App ID”.

1. In the "validDomains" section, replace the placeholder with your App Service's domain. This is your App Service's URL you copied above **WITHOUT** the "https://" e.g. "icebreaker-XXXXXXXXXXXXX.azurewebsites.net".

1. Create a ZIP package with `manifest.json`, `color.png`, and `outline.png`. The two image files are the icons for your app in Teams.
    * Make sure that the 3 files are the *top level* of the ZIP package, with no nested folders.
    ![Teams app package ZIP file](images/app_package_zip.png)

## Step 4: Run the app in Microsoft Teams

1.	If your tenant has sideloading apps enabled, you can install your app to a team by following the instructions below.
    * Upload package to a team using the Apps tab: https://docs.microsoft.com/en-us/microsoftteams/platform/concepts/apps/apps-upload#upload-your-package-into-a-team-using-the-apps-tab
    * Upload package to a team using the Store: https://docs.microsoft.com/en-us/microsoftteams/platform/concepts/apps/apps-upload#upload-your-package-into-a-team-or-conversation-using-the-store

1.	You can also upload it to your tenant's app catalog, so that it can be available for everyone in your tenant to install: https://docs.microsoft.com/en-us/microsoftteams/tenant-apps-catalog-teams

## Troubleshooting

Please see our [Troubleshooting](Troubleshooting) page.

***
