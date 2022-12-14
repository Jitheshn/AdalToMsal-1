# Project structure 
- The first folder [1.Before-webapp-webapi-ADAL](https://github.com/idaceappdev/AdalToMsal/tree/main/Asp.NetCore-ADAL-To-Identity.Web/1.Before-webapp-webapi-ADAL) has the ASP.NET 2.2 core project code which uses ADAL to sign-in and acquire the token to call an API.
- The second folder [2. Intermediate-NETMigration-webapp-webapi](https://github.com/idaceappdev/AdalToMsal/tree/main/Asp.NetCore-ADAL-To-Identity.Web/2.%20Intermediate-NETMigration-webapp-webapi) has the project which is built on top of the above project and walks you through the steps involved to migrate the ASP.NET core 2.2 to .NET 6 which is a minimum .NET supported version. The project still uses ADAL. 
- The third folder [3. After-migration-webapp-webapi-MSAL](https://github.com/idaceappdev/AdalToMsal/tree/main/Asp.NetCore-ADAL-To-Identity.Web/3.%20After-migration-webapp-webapi-MSAL) has the project which is built on top of the above project and here you will see the changes related to the code where actual migrationhappens from ADAL to Identity.Web.   