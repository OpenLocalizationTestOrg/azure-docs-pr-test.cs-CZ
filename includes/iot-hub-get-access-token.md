## <a name="obtain-an-azure-resource-manager-token"></a><span data-ttu-id="d259a-101">Získání tokenu Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d259a-101">Obtain an Azure Resource Manager token</span></span>
<span data-ttu-id="d259a-102">Azure Active Directory musí ověřovat všechny úlohy, které můžete provádět na prostředků pomocí Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d259a-102">Azure Active Directory must authenticate all the tasks that you perform on resources using the Azure Resource Manager.</span></span> <span data-ttu-id="d259a-103">Z příkladu používá ověřování hesla, naleznete v dalších přístupy [požadavků ověřování Azure Resource Manager][lnk-authenticate-arm].</span><span class="sxs-lookup"><span data-stu-id="d259a-103">The example shown here uses password authentication, for other approaches see [Authenticating Azure Resource Manager requests][lnk-authenticate-arm].</span></span>

1. <span data-ttu-id="d259a-104">Přidejte následující kód, který **hlavní** metoda do souboru Program.cs k načtení tokenu z Azure AD pomocí id aplikace a hesla.</span><span class="sxs-lookup"><span data-stu-id="d259a-104">Add the following code to the **Main** method in Program.cs to retrieve a token from Azure AD using the application id and password.</span></span>
   
    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.microsoftonline.com/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
   
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```
2. <span data-ttu-id="d259a-105">Vytvoření **ResourceManagementClient** objekt, který používá token přidáním následující kód do konce **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="d259a-105">Create a **ResourceManagementClient** object that uses the token by adding the following code to the end of the **Main** method:</span></span>
   
    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```
3. <span data-ttu-id="d259a-106">Vytvořit nebo získat odkaz na skupinu prostředků, kterou používáte:</span><span class="sxs-lookup"><span data-stu-id="d259a-106">Create, or obtain a reference to, the resource group you are using:</span></span>
   
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