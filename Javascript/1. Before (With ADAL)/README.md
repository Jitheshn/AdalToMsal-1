---
services: active-directory
platforms: javascript
author: jmprieur
---

# Azure AD Javascript Getting Started

> There's a newer version of this sample! Check it out: https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2
>
> This newer sample takes advantage of the Microsoft identity platform (formerly Azure AD v2.0).
>
> While still in public preview, every component is supported in production environments

This sample demonstrates the use of ADAL for JavaScript for securing an single page app written independently of any frameworks, implemented with an ASP.NET Web API backend.

ADAL for Javascript is an open source library. For distribution options, source code, and contributions, check out the ADAL JS repo at https://github.com/AzureAD/azure-activedirectory-library-for-js.

For more information about how the protocols work in this scenario and other scenarios, see [Authentication Scenarios for Azure AD](http://go.microsoft.com/fwlink/?LinkId=394414).

## How To Run This Sample

Getting started is simple! To run this sample you will need:

- Visual Studio 2017
- An Internet connection
- An Azure Active Directory (Azure AD) tenant. For more information on how to get an Azure AD tenant, please see [How to get an Azure AD tenant](https://azure.microsoft.com/en-us/documentation/articles/active-directory-howto-tenant/)
- A user account in your Azure AD tenant. This sample will not work with a Microsoft account, so if you signed in to the Azure portal with a Microsoft account and have never created a user account in your directory before, you need to do that now.

### Step 1: Clone or download this repository

From your shell or command line:
`git clone https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi.git`

### Step 2: Register the sample with your Azure Active Directory tenant

1. Sign in to the [Azure portal](https://portal.azure.com).
2. On the top bar, click on your account and under the **Directory** list, choose the Active Directory tenant where you wish to register your application.
3. From the left hand navigation pane, choose **Azure Active Directory**.
4. Click on **[App registrations](https://go.microsoft.com/fwlink/?linkid=2083908)** and choose **New registration**.
5. When the Register an application page appears, enter your application's registration information:
   - Enter a friendly name for the application, for example 'SinglePageApp-jQuery-DotNet'.
   - Leave **Supported account types** on **Accounts in this organizational directory only**
   - For the **Redirect URI (optional)**, enter the base URL for the sample, which is by default `https://localhost:44302/`.
6. Click on **Register** to create the application.
7. On the following app **Overview** page, find the **Application (client) ID** value and record it for later. You'll need it to configure the Visual Studio configuration file for this project.
8. Select **Expose an API**, and select _Set_ For the App ID URI. When prompted, enter `https://<your_tenant_name>/SinglePageApp-jQuery-DotNet`, replacing `<your_tenant_name>` with the name of your Azure AD tenant.
9. [Optional] Next, if you are tenant admin, you can grant permissions across your tenant for your application. Go to **API Permissions**, and then choose **Grant admin consent for <tenant_name>** button. Click _Yes_ to confirm.

### Step 3: Enable the OAuth2 implicit grant for your application

By default, applications provisioned in Azure AD are not enabled to use the OAuth2 implicit grant. In order to run this sample, you need to explicitly opt in.

1. From the former steps, your browser should still be on the Azure portal.
2. From the application page, click on **Authentication**, and under **Advanced Settings**, select the checkboxes next to `Access tokens` and `ID tokens` to enable OAuth2 implicit grant for the application

### Step 4: Configure the sample to use your Azure Active Directory tenant

1. Open the solution in Visual Studio 2017.
2. Open the `web.config` file.
3. Find the app key `ida:Tenant` and replace the value with your AAD tenant name.
4. Find the app key `ida:Audience` and replace the value with the Application (client) ID from the Azure portal.
5. Open the file `App/Scripts/App.js` and locate the line `window.config = ...`.
6. Replace the value of `tenant` with your AAD tenant name.
7. Replace the value of `clientId` with the Client ID from the Azure portal.

### Step 5: Run the sample

Clean the solution, rebuild the solution, and run it.

You can trigger the sign in experience by either clicking on the sign in link on the top right corner, or by clicking directly on the Todo List tab.
Explore the sample by signing in, adding items to the To Do list, removing the user account, and starting again.

- **[Note]**: The [base sample](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi/tree/master) TodoList api uses localSQLDB to store the TodoList items. If you are using the base sample for migration and if the localSQLDB fails to work, we have provided the solution to store the TodoList items in the memory. Please follow the instructions below to setup the TodoList API sample to run using the memory.

1. Locate `TodoListServiceContext.cs` file under the `DAL` folder and replace the TodoListServiceContext Class definition with:

```
public class TodoListServiceContext
    {
        public TodoListServiceContext()

        { Todoes = new List<Todo>(); }
        public List<Todo> Todoes { get; set; }

        internal void SaveChanges()
        {

        }
    }
```

2. Locate the `TodoListServiceInitializer.cs` file under the `DAL` folder and comment the following section under the `TodoSPA.DAL` namespace.

```
class TodoListServiceInitializer : DropCreateDatabaseIfModelChanges<TodoListServiceContext>
{}
```

3. Locate the `TodoListController.cs` file under the `Controllers` folder and update the TodoListServiceContext object with `null`

```
private static TodoListServiceContext db = null;
```

4. Add the following constructor on the next line.

```
public TodoListController()
    {
        if (db == null)
        db = new TodoListServiceContext();
    }
```

5. Locate the Post method in the `TodoListController.cs` file and add the following snippet at before `db.Todoes.Add(todo);`

```
// Adding unique IDs to each item in the todo list
            int newId = 0;
            if (db.Todoes.Count() > 0)
            {
                newId = db.Todoes.Max(i => i.ID);
            }

            todo.ID = newId + 1;
            //----------------------------------
```

## About the Code

The key files containing authentication logic are the following:

**App.js** - Provides the app configuration values used by ADAL for driving protocol interactions with AAD, indicates which routes should not be accessed without previous authentication, issues login and logout requests to Azure AD, handles both successful and failed authentication callbacks from Azure AD, and displays information about the user received in the id_token.

**index.html** - contains a reference to adal.js.

**todoListCtrl.js**- shows how to take advantage of the acquireToken() method in ADAL to get a token for accessing a resource.

**userDataCtrl.js** - shows how to extract user information from the cached id_token.
