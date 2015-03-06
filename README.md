# O365-CORS-VisualStudio
This sample shows how to build a AngularJS Single Page Application using Visual Studio and ADAL for JavaScript to demonstrate the Office 365 CORS support.

ADAL for Javascript is an open source library.  For distribution options, source code, and contributions, check out the ADAL JS repo at [https://github.com/AzureAD/azure-activedirectory-library-for-js](https://github.com/AzureAD/azure-activedirectory-library-for-js).

## How To Run This Sample
To run this sample, you need:

1. Visual Studio 2013
2. [Office Developer Tools for Visual Studio 2013](http://aka.ms/OfficeDevToolsForVS2013)
3. [Office 365 Developer Subscription](https://portal.office.com/Signup/Signup.aspx?OfferId=6881A1CB-F4EB-4db3-9F18-388898DAF510&DL=DEVELOPERPACK&ali=1)

## Step 1: Clone or download this repository
From your Git Shell or command line:

`git clone https://github.com/OfficeDev/O365-CORS-VisualStudio.git`

## Step 2: Build the Project
1. Open the project in Visual Studio 2013.
2. Simply Build the project to restore NuGet packages.
3. Ignore any build errors for now as we will configure the project in the next steps.

## Step 3: Configure the sample
Once downloaded, open the sample in Visual Studio.

### Register Azure AD application to consume Office 365 APIs
Office 365 applications use Azure Active Directory (Azure AD) to authenticate and authorize users and applications respectively. All users, application registrations, permissions are stored in Azure AD.

Using the Office 365 API Tool for Visual Studio you can configure your web application to consume Office 365 APIs.

1. In the Solution Explorer window, **right click your project -> Add -> Connected Service**.
2. A Services Manager dialog box will appear. Choose **Office 365 -> Office 365 API** and click **Register your app**.
3. On the sign-in dialog box, enter the username and password for your Office 365 tenant.
4. After you're signed in, you will see a list of all the services.
5. Initially, no permissions will be selected, as the app is not registered to consume any services yet.
6. Select **Users and Groups** and then click **Permissions**
7. In the **Users and Groups Permissions** dialog, select **Enable sign-on and read users profiles'** and click **Apply**
8. Select **My Files** and then click **Permissions**
9. In the **My Files Permissions** dialog, select both **Read users' files** and **Edit or delete users' files** then click **Apply**
10. Click **Ok**

After clicking OK, Office 365 client libraries (in the form of NuGet packages) for connecting to Office 365 APIs will be added to your project.

In this process, Office 365 API tool registered an Azure AD Application in the Office 365 tenant that you signed in the wizard and added the Azure AD application details to web.config.

### Update web.config with your Tenant ID
In your web.config, update the **TenantId** value to your **Office 365 tenant Id** where the application is deployed.

To get the tenant Id of your Office 365 tenant:
- Log in to your Azure Portal and select your Office 365 domain directory.

**Note:** If you are unable to login to [Azure Portal](https://manage.windowsazure.com) using your Office 365 credentials, You can also access your Office 365’s Azure Portal directly from your [Office 365 Admin Center](http://chakkaradeep.com/index.php/access-azure-active-directory-portal-from-your-office-365-subscription/)

- Now, in the browser URL, locate the GUID. This will be your Office 365 tenant Id.
- Copy and paste it in the web.config where it says “paste-your-tenant-guid-here“ : 
<add key=“ida:TenantId“ value=“paste-your-tenant-guid-here“ />

**Note:** If you are deploying to a production tenant, you will need to ask your tenant admin for the tenant identifier.

### Update app.js with your Client ID
1. Open the web.config file.
2. Find the app key `ida:ClientID` and copy the value.
3. Open the file `App/Scripts/app.js` and locate the line `adalProvider.init(`.
4. Replace the value of `clientId` with the ClientId from web.config.

## Step 4: Enable the OAuth2 implicit grant for your application
By default, applications provisioned in Azure AD are not enabled to use the OAuth2 implicit grant. In order to run this sample, you need to explicitly opt in.

1. Log in to your Azure Portal and select your Office 365 domain directory.
2. Click on the Applications tab.
3. Paste the `ClientID` which was copied from web.config into the search box and trigger search. Now select "Office365CORS.Office365App" application from the results list. 
4. Using the Manage Manifest button in the drawer, download the manifest file for the application and save it to disk.
5. Open the manifest file with a text editor. Search for the `oauth2AllowImplicitFlow` property. You will find that it is set to `false`; change it to `true` and save the file.
6. Using the Manage Manifest button, upload the updated manifest file. Save the configuration of the app.

## Step 5: Run the sample
Clean the solution, rebuild the solution, and run it. 

You can trigger the sign in experience by either clicking on the Login link on the top right corner, or by clicking directly on the My Files tab. To see a list of files stored on OneDrive click on the My Files tab. To view the properties of a file or folder, click on the properties link displayed right next to the file name and the properties will be displayed at the bottom of MyFiles list.

## How To Deploy This Sample To Azure Website
To deploy this sample application to azure website, we will first create a new website on Azure then import the publish profile

1. Sign in to the [Azure management portal](https://manage.windowsazure.com).
2. In the drawer, click **+New**.
3. Select **Compute** -> **Website** -> **Quick Create**.
4. Enter the URL, for example "Office365CORS" and from the web hosting plan either select an existing plan or create a new web hosting plan.
5. Click **Create Website**.
6. Once the website is created, select the newly created website and click on **Configure** tab.
7. Scroll down to **Authentication / Authorization** and click Configure.
8. On the "Configure Azure Wedsites Authentication / Authorization" dialog, select the **Directory** in which the sample app was registered and from the **AAD Application** drop down list, select "Office365CORS.Office365App (https://localhost:44304/)". Click **OK**.
9. Once authentication / authorization configuration is completed, click on **Dashboard** tab.
10. In the quick glance section, click **Download the publish profile** and save it to disk.
11. Go back to Visual Studio, right click on "Office365CORS" project node in solution explorer and select **Publish**.
12. Click **Import**.
13. **Browse** to the publish profile that we saved to disk and click **OK**.
14. In Connection tab, make sure that **Publish method** is "Web Deploy". Click **Next**.
15. In Settings tab, **un-check** the **Enable Organizational Authentication** checkbox to disable it.
16. Click **Publish**.

The sample application will now be published to "[websiteURL].azurewebsites.net".