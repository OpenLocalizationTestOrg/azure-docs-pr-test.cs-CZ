## <a name="obtain-an-azure-resource-manager-token"></a><span data-ttu-id="58152-101">Získání tokenu Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="58152-101">Obtain an Azure Resource Manager token</span></span>
<span data-ttu-id="58152-102">Azure Active Directory musí ověřovat všechny hello úlohy, které můžete provádět na prostředků pomocí Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="58152-102">Azure Active Directory must authenticate all hello tasks that you perform on resources using hello Azure Resource Manager.</span></span> <span data-ttu-id="58152-103">Hello zde ukazuje příklad používá ověřování hesla jiné postupy najdete v tématu [požadavků ověřování Azure Resource Manager][lnk-authenticate-arm].</span><span class="sxs-lookup"><span data-stu-id="58152-103">hello example shown here uses password authentication, for other approaches see [Authenticating Azure Resource Manager requests][lnk-authenticate-arm].</span></span>

1. <span data-ttu-id="58152-104">Přidejte následující kód toohello hello **hlavní** metoda v souboru Program.cs tooretrieve tokenu z Azure AD pomocí id aplikace hello a hesla.</span><span class="sxs-lookup"><span data-stu-id="58152-104">Add hello following code toohello **Main** method in Program.cs tooretrieve a token from Azure AD using hello application id and password.</span></span>
   
    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.microsoftonline.com/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
   
    if (token == null)
    {
      Console.WriteLine("Failed tooobtain hello token");
      return;
    }
    ```
2. <span data-ttu-id="58152-105">Vytvoření **ResourceManagementClient** objektu, že používá hello token přidáním následujících toohello kód na konci hello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="58152-105">Create a **ResourceManagementClient** object that uses hello token by adding hello following code toohello end of hello **Main** method:</span></span>
   
    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```
3. <span data-ttu-id="58152-106">Vytvořit nebo získat odkaz na hello skupinu prostředků, který používáte:</span><span class="sxs-lookup"><span data-stu-id="58152-106">Create, or obtain a reference to, hello resource group you are using:</span></span>
   
    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx