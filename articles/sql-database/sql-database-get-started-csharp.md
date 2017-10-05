---
title: "C#: Začínáme s Azure SQL Database | Dokumentace Microsoftu"
description: "Vyzkoušejte SQL Database při vývoji aplikací v jazyce C# využívajících SQL a vytvořte Azure SQL Database v C# s použitím knihovny SQL Database Library pro .NET."
keywords: Zkuste sql, sql c#
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: cfff2299-a474-4054-8d99-759af1ae5188
ms.service: sql-database
ms.custom: develop apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: csharp
ms.workload: data-management
ms.date: 10/04/2016
ms.author: sstein
ms.openlocfilehash: c8a2703da1ee3687f8d134e768dd8d31dc4f316b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-c-to-create-a-sql-database-with-the-sql-database-library-for-net"></a>Vytvoření databáze SQL pomocí jazyka C# a knihovny SQL Database Library pro .NET

Zjistěte, jak v jazyce C# vytvořit databázi Azure SQL pomocí knihovny [Microsoft Azure SQL Management Library pro .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Tento článek popisuje, jak vytvořit databázi pomocí jazyka SQL a C#. Chcete-li vytvořit elastické fondy, přečtěte si článek [Vytvoření elastického fondu](sql-database-elastic-pool-manage-csharp.md).

Azure SQL Database Management Library pro .NET poskytuje rozhraní API založené na [Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md), které zabaluje rozhraní [SQL Database REST API založené na Správci prostředků](https://docs.microsoft.com/rest/api/sql/).

> [!NOTE]
> Podpora řady nových funkcí služby SQL Database je dostupná jen v případě, že používáte [model nasazení Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md), takže byste měli používat nejnovější knihovnu **Azure SQL Database Management Library pro .NET ([dokumenty](https://docs.microsoft.com/dotnet/api/overview/azure/sql?view=azure-dotnet) | [balíček NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Starší [knihovny založené na modelu nasazení Classic](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) jsou podporovány pouze kvůli zpětné kompatibilitě, takže doporučujeme, abyste používali novější knihovny založené na Resource Manageru.
> 
> 

K dokončení kroků v tomto článku budete potřebovat následující:

* Předplatné Azure. Pokud potřebujete předplatné Azure, jednoduše klikněte na **Bezplatný účet** v horní části této stránky a poté se vraťte a dokončete tento článek.
* Visual Studio. Bezplatnou kopii sady Visual Studio naleznete na stránce [Soubory ke stažení pro Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs).

> [!NOTE]
> V tomto článku se vytváří nová, prázdná databáze SQL. Upravte metodu *CreateOrUpdateDatabase(...)* v následující ukázce tak, aby kopírovala databáze, škálovala databáze, vytvořila databázi ve fondu apod.  
> 

## <a name="create-a-console-app-and-install-the-required-libraries"></a>Vytvoření konzolové aplikace a instalace potřebných knihoven
1. Spusťte Visual Studio.
2. Klikněte na **Soubor**  >  **Nový**  >  **Projekt**.
3. Vytvořte **Konzolovou aplikaci** v jazyce C# a pojmenujte ji *SqlDbConsoleApp*.

Chcete-li vytvořit databázi SQL pomocí jazyka C#, načtěte požadované knihovny správy (pomocí [konzoly správce balíčků](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Klikněte na **Nástroje**  >  **Správce balíčků NuGet**  >  **Konzola správce balíčků**.
2. Zadejte `Install-Package Microsoft.Azure.Management.Sql -Pre` a nainstalujte tak nejnovější knihovnu [Microsoft Azure SQL Management Library](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Zadejte `Install-Package Microsoft.Azure.Management.ResourceManager -Pre` a nainstalujte tak knihovnu [Microsoft Azure Resource Manager Library](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Zadejte `Install-Package Microsoft.Azure.Common.Authentication -Pre` a nainstalujte tak knihovnu [Microsoft Azure Common Authentication Library](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 

> [!NOTE]
> Příklady v tomto článku používají synchronní formu každého požadavku rozhraní API a provádění se tedy zablokuje, dokud se nedokončí volání REST základní služby. Dostupné jsou i asynchronní metody
> 
> 

## <a name="create-a-sql-database-server-firewall-rule-and-sql-database---c-example"></a>Vytvoření serveru služby SQL Database, pravidla brány firewall a databáze SQL – ukázka v jazyce C#
Následující příklad vytvoří skupinu prostředků, server, pravidlo brány firewall a databázi SQL. Proměnné `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` získáte pomocí postupu v oddílu [Vytvoření instančního objektu pro přístup k prostředkům](#create-a-service-principal-to-access-resources).

Nahraďte obsah souboru **Program.cs** následujícím ukázkovým kódem a aktualizujte hodnoty `{variables}` hodnotami vaší aplikace (bez závorek `{}`).

    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;

    namespace SqlDbConsoleApp
    {
    class Program
       {
        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _databaseName = "{dbfromcsarticle}";
        static string _databaseEdition = DatabaseEditions.Basic;
        static string _databasePerfLevel = ""; // "S0", "S1", and so on here for other tiers


        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken)) { SubscriptionId = _subscriptionId };

            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            Server sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Id);

            Console.WriteLine("Server firewall...");
            FirewallRule fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.Id);

            Console.WriteLine("Database...");
            Database dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _databaseEdition, _databasePerfLevel);
            Console.WriteLine("Database: " + dbr.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static Server CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            Server serverParameters = new Server()
            {
                Location = serverLocation,
                AdministratorLogin = serverAdmin,
                AdministratorLoginPassword = serverAdminPassword,
                Version = "12.0"
            };
            Server serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }

        static FirewallRule CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRule firewallParameters = new FirewallRule()
            {
                StartIpAddress = startIpAddress,
                EndIpAddress = endIpAddress
            };
            FirewallRule firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }

        static Database CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string databaseEdition, string databasePerfLevel)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName);

            // Create a database: configure create or update parameters and properties explicitly
            Database newDatabaseParameters = new Database()
            {
                Location = currentServer.Location,
                CreateMode = CreateMode.Default,
                Edition = databaseEdition,
                RequestedServiceObjectiveName = databasePerfLevel

            };
            Database dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
      }
    }





## <a name="create-a-service-principal-to-access-resources"></a>Vytvoření instančního objektu pro přístup k prostředkům
Následující skript prostředí PowerShell vytvoří aplikaci Active Directory (AD) a instanční objekt, který potřebujeme k ověření naší aplikace v jazyce C#. Skript vypíše hodnoty potřebné pro předchozí ukázku v jazyce C#. Podrobné informace najdete v tématu [Vytvoření instančního objektu pro přístup k prostředkům pomocí prostředí Azure PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md).

    # Sign in to Azure.
    Add-AzureRmAccount

    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output the values we need for our C# application to successfully authenticate

    Write-Output "Copy these values into the C# sample app"

    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret



## <a name="next-steps"></a>Další kroky
Nyní, když jste si vyzkoušeli SQL Database a nastavili databázi pomocí C#, jste připraveni na následující články:

* [Připojení k SQL Database přes SQL Server Management Studio a provedení ukázkového dotazu T-SQL](sql-database-connect-query-ssms.md)

## <a name="additional-resources"></a>Další zdroje
* [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)
* [Třída Database](https://msdn.microsoft.com/library/azure/microsoft.azure.management.sql.models.database.aspx)

<!--Image references-->
[1]: ./media/sql-database-get-started-csharp/aad.png
[2]: ./media/sql-database-get-started-csharp/permissions.png
[3]: ./media/sql-database-get-started-csharp/getdomain.png
[4]: ./media/sql-database-get-started-csharp/aad2.png
[5]: ./media/sql-database-get-started-csharp/aad-applications.png
[6]: ./media/sql-database-get-started-csharp/add.png
[7]: ./media/sql-database-get-started-csharp/add-application.png
[8]: ./media/sql-database-get-started-csharp/add-application2.png
[9]: ./media/sql-database-get-started-csharp/clientid.png
