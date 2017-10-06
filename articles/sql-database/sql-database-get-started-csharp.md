---
title: "C#: Začínáme s Azure SQL Database | Dokumentace Microsoftu"
description: "Vyzkoušejte SQL Database při vývoji aplikací SQL a C# a vytvořte Azure SQL Database v C# s použitím hello SQL Database Library pro .NET."
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
ms.openlocfilehash: e880ebabd53546bea37a13186b0f1a13db35b684
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-c-toocreate-a-sql-database-with-hello-sql-database-library-for-net"></a>Použití jazyka C# toocreate databázi SQL s hello SQL Database Library pro .NET

Zjistěte, jak toouse C# toocreate Azure SQL databáze s hello [Microsoft Azure SQL Management Library pro .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Tento článek popisuje, jak toocreate jedné databáze SQL a C#. toocreate elastické fondy, najdete v části [vytvoření fondu elastické databáze](sql-database-elastic-pool-manage-csharp.md).

Hello knihovny správu databáze SQL Azure pro .NET poskytuje [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)– na základě rozhraní API, které zabaluje hello [založené na správci prostředků SQL Database REST API](https://docs.microsoft.com/rest/api/sql/).

> [!NOTE]
> Databáze SQL, mnoha nových funkcí podporují jenom při použití hello [modelu nasazení Azure Resource Manager](../azure-resource-manager/resource-group-overview.md), takže je třeba použít hello nejnovější **knihovny správu databáze SQL Azure pro .NET ([dokumentace](https://docs.microsoft.com/dotnet/api/overview/azure/sql?view=azure-dotnet) | [balíček NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Hello starší [knihovny založené na modelu nasazení classic](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) jsou podporovány pouze z důvodů zpětné kompatibility, proto doporučujeme použít hello novější správce prostředků na základě knihovny.
> 
> 

toocomplete hello kroky v tomto článku, budete potřebovat následující hello:

* Předplatné Azure. Pokud potřebujete předplatné Azure, jednoduše klikněte na tlačítko **bezplatný účet** na začátku hello stránky a pak se vraťte toofinish v tomto článku.
* Visual Studio. Bezplatnou kopii sady Visual Studio, najdete v části hello [Visual Studio stáhne](https://www.visualstudio.com/downloads/download-visual-studio-vs) stránky.

> [!NOTE]
> V tomto článku se vytváří nová, prázdná databáze SQL. Upravit hello *CreateOrUpdateDatabase(...)*  metoda v hello následující ukázkové databáze toocopy, škálování databáze, vytvoření databáze ve fondu, atd.  
> 

## <a name="create-a-console-app-and-install-hello-required-libraries"></a>Vytvořte aplikaci konzoly a nainstalujte požadované hello knihovny
1. Spusťte Visual Studio.
2. Klikněte na **Soubor**  >  **Nový**  >  **Projekt**.
3. Vytvořte **Konzolovou aplikaci** v jazyce C# a pojmenujte ji *SqlDbConsoleApp*.

toocreate databáze SQL pomocí C#, zatížení hello požadované knihovny správy (pomocí hello [konzoly Správce balíčků](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Klikněte na **Nástroje**  >  **Správce balíčků NuGet**  >  **Konzola správce balíčků**.
2. Typ `Install-Package Microsoft.Azure.Management.Sql -Pre` tooinstall hello nejnovější [knihovnu Microsoft Azure SQL Management](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Typ `Install-Package Microsoft.Azure.Management.ResourceManager -Pre` tooinstall hello [knihovnu Microsoft Azure Resource Manager](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Typ `Install-Package Microsoft.Azure.Common.Authentication -Pre` tooinstall hello [knihovnu běžných ověřování Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 

> [!NOTE]
> Hello příklady v tomto článku používají synchronní formu každého požadavku rozhraní API a zároveň se zablokují až do dokončení volání REST hello hello základní služby. Dostupné jsou i asynchronní metody
> 
> 

## <a name="create-a-sql-database-server-firewall-rule-and-sql-database---c-example"></a>Vytvoření serveru služby SQL Database, pravidla brány firewall a databáze SQL – ukázka v jazyce C#
Hello následující ukázka vytvoří skupinu prostředků, server, pravidlo brány firewall a databázi SQL. Zobrazit, [vytvořit hlavní tooaccess služby prostředků](#create-a-service-principal-to-access-resources) tooget hello `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` proměnné.

Nahraďte obsah hello **Program.cs** s následující hello a aktualizace hello `{variables}` hodnotami vaší aplikace (nezahrnují hello `{}`).

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

        // Create management clients for hello Azure resources your app needs toowork with.
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


            Console.WriteLine("Press any key toocontinue...");
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
            // Retrieve hello server that will host this database
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





## <a name="create-a-service-principal-tooaccess-resources"></a>Vytvořit hlavní tooaccess služby prostředků
Hello následující skript prostředí PowerShell vytvoří hello Active Directory (AD) aplikace a služby hello hlavní, že potřebujeme tooauthenticate vaší aplikace C#. Hello skript vypíše hodnoty, které budeme potřebovat pro hello předcházející C# ukázkové. Podrobné informace najdete v tématu [toocreate použití Azure PowerShell objekt služby prostředků tooaccess](../azure-resource-manager/resource-group-authenticate-service-principal.md).

    # Sign in tooAzure.
    Add-AzureRmAccount

    # If you have multiple subscriptions, uncomment and set toohello subscription you want toowork with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is hello display name for your app, must be unique in your directory.
    # $uri does not need toobe a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for hello app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # tooavoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun hello following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output hello values we need for our C# application toosuccessfully authenticate

    Write-Output "Copy these values into hello C# sample app"

    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret



## <a name="next-steps"></a>Další kroky
Teď, když jste si vyzkoušeli SQL Database a nastavili databázi pomocí C#, jste připraveni pro hello následující články:

* [Připojení tooSQL databáze s SQL Server Management Studio a provedení ukázkového dotazu T-SQL](sql-database-connect-query-ssms.md)

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
