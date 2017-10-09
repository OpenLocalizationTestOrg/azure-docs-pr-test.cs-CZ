## <a name="obtain-an-azure-resource-manager-token"></a>Získání tokenu Azure Resource Manager
Azure Active Directory musí ověřovat všechny hello úlohy, které můžete provádět na prostředků pomocí Azure Resource Manager hello. Hello zde ukazuje příklad používá ověřování hesla jiné postupy najdete v tématu [požadavků ověřování Azure Resource Manager][lnk-authenticate-arm].

1. Přidejte následující kód toohello hello **hlavní** metoda v souboru Program.cs tooretrieve tokenu z Azure AD pomocí id aplikace hello a hesla.
   
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
2. Vytvoření **ResourceManagementClient** objektu, že používá hello token přidáním následujících toohello kód na konci hello hello **hlavní** metoda:
   
    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```
3. Vytvořit nebo získat odkaz na hello skupinu prostředků, který používáte:
   
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