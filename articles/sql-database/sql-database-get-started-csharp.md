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
# <a name="use-c-to-create-a-sql-database-with-the-sql-database-library-for-net"></a><span data-ttu-id="7e43f-104">Vytvoření databáze SQL pomocí jazyka C# a knihovny SQL Database Library pro .NET</span><span class="sxs-lookup"><span data-stu-id="7e43f-104">Use C# to create a SQL database with the SQL Database Library for .NET</span></span>

<span data-ttu-id="7e43f-105">Zjistěte, jak v jazyce C# vytvořit databázi Azure SQL pomocí knihovny [Microsoft Azure SQL Management Library pro .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).</span><span class="sxs-lookup"><span data-stu-id="7e43f-105">Learn how to use C# to create an Azure SQL database with the [Microsoft Azure SQL Management Library for .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).</span></span> <span data-ttu-id="7e43f-106">Tento článek popisuje, jak vytvořit databázi pomocí jazyka SQL a C#.</span><span class="sxs-lookup"><span data-stu-id="7e43f-106">This article describes how to create a single database with SQL and C#.</span></span> <span data-ttu-id="7e43f-107">Chcete-li vytvořit elastické fondy, přečtěte si článek [Vytvoření elastického fondu](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="7e43f-107">To create elastic pools, see [Create an elastic pool](sql-database-elastic-pool-manage-csharp.md).</span></span>

<span data-ttu-id="7e43f-108">Azure SQL Database Management Library pro .NET poskytuje rozhraní API založené na [Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md), které zabaluje rozhraní [SQL Database REST API založené na Správci prostředků](https://docs.microsoft.com/rest/api/sql/).</span><span class="sxs-lookup"><span data-stu-id="7e43f-108">The Azure SQL Database Management Library for .NET provides an [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)-based API that wraps the [Resource Manager-based SQL Database REST API](https://docs.microsoft.com/rest/api/sql/).</span></span>

> [!NOTE]
> <span data-ttu-id="7e43f-109">Podpora řady nových funkcí služby SQL Database je dostupná jen v případě, že používáte [model nasazení Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md), takže byste měli používat nejnovější knihovnu **Azure SQL Database Management Library pro .NET ([dokumenty](https://docs.microsoft.com/dotnet/api/overview/azure/sql?view=azure-dotnet) | [balíček NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**.</span><span class="sxs-lookup"><span data-stu-id="7e43f-109">Many new features of SQL Database are only supported when you are using the [Azure Resource Manager deployment model](../azure-resource-manager/resource-group-overview.md), so you should always use the latest **Azure SQL Database Management Library for .NET ([docs](https://docs.microsoft.com/dotnet/api/overview/azure/sql?view=azure-dotnet) | [NuGet Package](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**.</span></span> <span data-ttu-id="7e43f-110">Starší [knihovny založené na modelu nasazení Classic](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) jsou podporovány pouze kvůli zpětné kompatibilitě, takže doporučujeme, abyste používali novější knihovny založené na Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="7e43f-110">The older [classic deployment model based libraries](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) are supported for backward compatibility only, so we recommend you use the newer Resource Manager based libraries.</span></span>
> 
> 

<span data-ttu-id="7e43f-111">K dokončení kroků v tomto článku budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="7e43f-111">To complete the steps in this article, you need the following:</span></span>

* <span data-ttu-id="7e43f-112">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="7e43f-112">An Azure subscription.</span></span> <span data-ttu-id="7e43f-113">Pokud potřebujete předplatné Azure, jednoduše klikněte na **Bezplatný účet** v horní části této stránky a poté se vraťte a dokončete tento článek.</span><span class="sxs-lookup"><span data-stu-id="7e43f-113">If you need an Azure subscription simply click **FREE ACCOUNT** at the top of this page, and then come back to finish this article.</span></span>
* <span data-ttu-id="7e43f-114">Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7e43f-114">Visual Studio.</span></span> <span data-ttu-id="7e43f-115">Bezplatnou kopii sady Visual Studio naleznete na stránce [Soubory ke stažení pro Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs).</span><span class="sxs-lookup"><span data-stu-id="7e43f-115">For a free copy of Visual Studio, see the [Visual Studio Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) page.</span></span>

> [!NOTE]
> <span data-ttu-id="7e43f-116">V tomto článku se vytváří nová, prázdná databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="7e43f-116">This article creates a new, blank SQL database.</span></span> <span data-ttu-id="7e43f-117">Upravte metodu *CreateOrUpdateDatabase(...)* v následující ukázce tak, aby kopírovala databáze, škálovala databáze, vytvořila databázi ve fondu apod.</span><span class="sxs-lookup"><span data-stu-id="7e43f-117">Modify the *CreateOrUpdateDatabase(...)* method in the following sample to copy databases, scale databases, create a database in a pool, etc.</span></span>  
> 

## <a name="create-a-console-app-and-install-the-required-libraries"></a><span data-ttu-id="7e43f-118">Vytvoření konzolové aplikace a instalace potřebných knihoven</span><span class="sxs-lookup"><span data-stu-id="7e43f-118">Create a console app and install the required libraries</span></span>
1. <span data-ttu-id="7e43f-119">Spusťte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7e43f-119">Start Visual Studio.</span></span>
2. <span data-ttu-id="7e43f-120">Klikněte na **Soubor**  >  **Nový**  >  **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="7e43f-120">Click **File** > **New** > **Project**.</span></span>
3. <span data-ttu-id="7e43f-121">Vytvořte **Konzolovou aplikaci** v jazyce C# a pojmenujte ji *SqlDbConsoleApp*.</span><span class="sxs-lookup"><span data-stu-id="7e43f-121">Create a C# **Console Application** and name it: *SqlDbConsoleApp*</span></span>

<span data-ttu-id="7e43f-122">Chcete-li vytvořit databázi SQL pomocí jazyka C#, načtěte požadované knihovny správy (pomocí [konzoly správce balíčků](http://docs.nuget.org/Consume/Package-Manager-Console)):</span><span class="sxs-lookup"><span data-stu-id="7e43f-122">To create a SQL database with C#, load the required management libraries (using the [package manager console](http://docs.nuget.org/Consume/Package-Manager-Console)):</span></span>

1. <span data-ttu-id="7e43f-123">Klikněte na **Nástroje**  >  **Správce balíčků NuGet**  >  **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="7e43f-123">Click **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
2. <span data-ttu-id="7e43f-124">Zadejte `Install-Package Microsoft.Azure.Management.Sql -Pre` a nainstalujte tak nejnovější knihovnu [Microsoft Azure SQL Management Library](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).</span><span class="sxs-lookup"><span data-stu-id="7e43f-124">Type `Install-Package Microsoft.Azure.Management.Sql -Pre` to install the latest [Microsoft Azure SQL Management Library](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).</span></span>
3. <span data-ttu-id="7e43f-125">Zadejte `Install-Package Microsoft.Azure.Management.ResourceManager -Pre` a nainstalujte tak knihovnu [Microsoft Azure Resource Manager Library](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).</span><span class="sxs-lookup"><span data-stu-id="7e43f-125">Type `Install-Package Microsoft.Azure.Management.ResourceManager -Pre` to install the [Microsoft Azure Resource Manager Library](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).</span></span>
4. <span data-ttu-id="7e43f-126">Zadejte `Install-Package Microsoft.Azure.Common.Authentication -Pre` a nainstalujte tak knihovnu [Microsoft Azure Common Authentication Library](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication).</span><span class="sxs-lookup"><span data-stu-id="7e43f-126">Type `Install-Package Microsoft.Azure.Common.Authentication -Pre` to install the [Microsoft Azure Common Authentication Library](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication).</span></span> 

> [!NOTE]
> <span data-ttu-id="7e43f-127">Příklady v tomto článku používají synchronní formu každého požadavku rozhraní API a provádění se tedy zablokuje, dokud se nedokončí volání REST základní služby.</span><span class="sxs-lookup"><span data-stu-id="7e43f-127">The examples in this article use a synchronous form of each API request and block until completion of the REST call on the underlying service.</span></span> <span data-ttu-id="7e43f-128">Dostupné jsou i asynchronní metody</span><span class="sxs-lookup"><span data-stu-id="7e43f-128">There are async methods available.</span></span>
> 
> 

## <a name="create-a-sql-database-server-firewall-rule-and-sql-database---c-example"></a><span data-ttu-id="7e43f-129">Vytvoření serveru služby SQL Database, pravidla brány firewall a databáze SQL – ukázka v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="7e43f-129">Create a SQL Database server, firewall rule, and SQL database - C# example</span></span>
<span data-ttu-id="7e43f-130">Následující příklad vytvoří skupinu prostředků, server, pravidlo brány firewall a databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="7e43f-130">The following sample creates a resource group, server, firewall rule, and a SQL database.</span></span> <span data-ttu-id="7e43f-131">Proměnné `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` získáte pomocí postupu v oddílu [Vytvoření instančního objektu pro přístup k prostředkům](#create-a-service-principal-to-access-resources).</span><span class="sxs-lookup"><span data-stu-id="7e43f-131">See, [Create a service principal to access resources](#create-a-service-principal-to-access-resources) to get the `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` variables.</span></span>

<span data-ttu-id="7e43f-132">Nahraďte obsah souboru **Program.cs** následujícím ukázkovým kódem a aktualizujte hodnoty `{variables}` hodnotami vaší aplikace (bez závorek `{}`).</span><span class="sxs-lookup"><span data-stu-id="7e43f-132">Replace the contents of **Program.cs** with the following, and update the `{variables}` with your app values (do not include the `{}`).</span></span>

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





## <a name="create-a-service-principal-to-access-resources"></a><span data-ttu-id="7e43f-133">Vytvoření instančního objektu pro přístup k prostředkům</span><span class="sxs-lookup"><span data-stu-id="7e43f-133">Create a service principal to access resources</span></span>
<span data-ttu-id="7e43f-134">Následující skript prostředí PowerShell vytvoří aplikaci Active Directory (AD) a instanční objekt, který potřebujeme k ověření naší aplikace v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="7e43f-134">The following PowerShell script creates the Active Directory (AD) application and the service principal that we need to authenticate our C# app.</span></span> <span data-ttu-id="7e43f-135">Skript vypíše hodnoty potřebné pro předchozí ukázku v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="7e43f-135">The script outputs values we need for the preceding C# sample.</span></span> <span data-ttu-id="7e43f-136">Podrobné informace najdete v tématu [Vytvoření instančního objektu pro přístup k prostředkům pomocí prostředí Azure PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="7e43f-136">For detailed information, see [Use Azure PowerShell to create a service principal to access resources](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

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



## <a name="next-steps"></a><span data-ttu-id="7e43f-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7e43f-137">Next steps</span></span>
<span data-ttu-id="7e43f-138">Nyní, když jste si vyzkoušeli SQL Database a nastavili databázi pomocí C#, jste připraveni na následující články:</span><span class="sxs-lookup"><span data-stu-id="7e43f-138">Now that you've tried SQL Database and set up a database with C#, you're ready for the following articles:</span></span>

* [<span data-ttu-id="7e43f-139">Připojení k SQL Database přes SQL Server Management Studio a provedení ukázkového dotazu T-SQL</span><span class="sxs-lookup"><span data-stu-id="7e43f-139">Connect to SQL Database with SQL Server Management Studio and perform a sample T-SQL query</span></span>](sql-database-connect-query-ssms.md)

## <a name="additional-resources"></a><span data-ttu-id="7e43f-140">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7e43f-140">Additional Resources</span></span>
* [<span data-ttu-id="7e43f-141">SQL Database</span><span class="sxs-lookup"><span data-stu-id="7e43f-141">SQL Database</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* [<span data-ttu-id="7e43f-142">Třída Database</span><span class="sxs-lookup"><span data-stu-id="7e43f-142">Database Class</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.management.sql.models.database.aspx)

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
