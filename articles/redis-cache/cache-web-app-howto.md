---
title: "aaaHow toocreate webové aplikace s Redis Cache | Microsoft Docs"
description: "Zjistěte, jak toocreate webové aplikace s Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 454e23d7-a99b-4e6e-8dd7-156451d2da7c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: hero-article
ms.date: 05/09/2017
ms.author: sdanie
ms.openlocfilehash: d3e6df97b06fdf9032570dc360944be4bd7715de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-web-app-with-redis-cache"></a><span data-ttu-id="78d16-103">Jak toocreate webové aplikace s Redis Cache</span><span class="sxs-lookup"><span data-stu-id="78d16-103">How toocreate a Web App with Redis Cache</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="78d16-104">.NET</span><span class="sxs-lookup"><span data-stu-id="78d16-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="78d16-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="78d16-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="78d16-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="78d16-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="78d16-107">Java</span><span class="sxs-lookup"><span data-stu-id="78d16-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="78d16-108">Python</span><span class="sxs-lookup"><span data-stu-id="78d16-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="78d16-109">Tento kurz ukazuje, jak toocreate a nasazení webové aplikace tooa webové aplikace ASP.NET ve službě Azure App Service pomocí Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="78d16-109">This tutorial shows how toocreate and deploy an ASP.NET web application tooa web app in Azure App Service using Visual Studio 2017.</span></span> <span data-ttu-id="78d16-110">Hello ukázková aplikace zobrazí seznam týmových statistik z databáze a uvádí různé způsoby toouse Azure Redis Cache toostore a načtení dat z mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-110">hello sample application displays a list of team statistics from a database and shows different ways toouse Azure Redis Cache toostore and retrieve data from hello cache.</span></span> <span data-ttu-id="78d16-111">Po dokončení kurzu hello budete mít funkční webovou aplikaci, která čte a zapisuje tooa databáze, optimalizované s Azure Redis Cache a je hostovaná v Azure.</span><span class="sxs-lookup"><span data-stu-id="78d16-111">When you complete hello tutorial you'll have a running web app that reads and writes tooa database, optimized with Azure Redis Cache, and hosted in Azure.</span></span>

<span data-ttu-id="78d16-112">Naučíte se:</span><span class="sxs-lookup"><span data-stu-id="78d16-112">You'll learn:</span></span>

* <span data-ttu-id="78d16-113">Jak toocreate ASP.NET MVC 5 webovou aplikaci v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="78d16-113">How toocreate an ASP.NET MVC 5 web application in Visual Studio.</span></span>
* <span data-ttu-id="78d16-114">Jak tooaccess dat z databáze používající rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="78d16-114">How tooaccess data from a database using Entity Framework.</span></span>
* <span data-ttu-id="78d16-115">Jak tooimprove propustnost dat a snížit zatížení databáze díky ukládání a načítání dat pomocí Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="78d16-115">How tooimprove data throughput and reduce database load by storing and retrieving data using Azure Redis Cache.</span></span>
* <span data-ttu-id="78d16-116">Jak toouse Redis seřazené sady tooretrieve hello top 5 týmů.</span><span class="sxs-lookup"><span data-stu-id="78d16-116">How toouse a Redis sorted set tooretrieve hello top 5 teams.</span></span>
* <span data-ttu-id="78d16-117">Jak tooprovision hello prostředků Azure pro aplikace hello pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="78d16-117">How tooprovision hello Azure resources for hello application using a Resource Manager template.</span></span>
* <span data-ttu-id="78d16-118">Jak toopublish hello tooAzure aplikace pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="78d16-118">How toopublish hello application tooAzure using Visual Studio.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="78d16-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="78d16-119">Prerequisites</span></span>
<span data-ttu-id="78d16-120">toocomplete hello kurz, musíte mít hello následující požadavky.</span><span class="sxs-lookup"><span data-stu-id="78d16-120">toocomplete hello tutorial, you must have hello following prerequisites.</span></span>

* [<span data-ttu-id="78d16-121">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="78d16-121">Azure account</span></span>](#azure-account)
* [<span data-ttu-id="78d16-122">Visual Studio 2017 s hello Azure SDK for .NET</span><span class="sxs-lookup"><span data-stu-id="78d16-122">Visual Studio 2017 with hello Azure SDK for .NET</span></span>](#visual-studio-2017-with-the-azure-sdk-for-net)

### <a name="azure-account"></a><span data-ttu-id="78d16-123">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="78d16-123">Azure account</span></span>
<span data-ttu-id="78d16-124">Budete potřebovat účtu Azure toocomplete hello kurzu.</span><span class="sxs-lookup"><span data-stu-id="78d16-124">You need an Azure account toocomplete hello tutorial.</span></span> <span data-ttu-id="78d16-125">Můžete:</span><span class="sxs-lookup"><span data-stu-id="78d16-125">You can:</span></span>

* <span data-ttu-id="78d16-126">[Otevřít bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).</span><span class="sxs-lookup"><span data-stu-id="78d16-126">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).</span></span> <span data-ttu-id="78d16-127">Získáte kredity, které se dají použít tootry na placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="78d16-127">You get credits that can be used tootry out paid Azure services.</span></span> <span data-ttu-id="78d16-128">I po hello kredity vyčerpáte, můžete zachovat hello účet a používat bezplatné služby Azure a funkce.</span><span class="sxs-lookup"><span data-stu-id="78d16-128">Even after hello credits are used up, you can keep hello account and use free Azure services and features.</span></span>
* <span data-ttu-id="78d16-129">[Aktivovat výhody pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero).</span><span class="sxs-lookup"><span data-stu-id="78d16-129">[Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero).</span></span> <span data-ttu-id="78d16-130">Díky předplatnému MSDN každý měsíc získáváte kredity, které můžete použít pro placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="78d16-130">Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

### <a name="visual-studio-2017-with-hello-azure-sdk-for-net"></a><span data-ttu-id="78d16-131">Visual Studio 2017 s hello Azure SDK for .NET</span><span class="sxs-lookup"><span data-stu-id="78d16-131">Visual Studio 2017 with hello Azure SDK for .NET</span></span>
<span data-ttu-id="78d16-132">Hello kurz je napsán pro Visual Studio 2017 s hello [Azure SDK for .NET](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes#azuretools).</span><span class="sxs-lookup"><span data-stu-id="78d16-132">hello tutorial is written for Visual Studio 2017 with hello [Azure SDK for .NET](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes#azuretools).</span></span> <span data-ttu-id="78d16-133">Hello Azure SDK 2.9.5 je součástí instalačního programu sady Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-133">hello Azure SDK 2.9.5 is included with hello Visual Studio installer.</span></span>

<span data-ttu-id="78d16-134">Pokud máte Visual Studio 2015, můžete postupovat podle kurzu hello s hello [Azure SDK for .NET](../dotnet-sdk.md) ve verzi 2.8.2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="78d16-134">If you have Visual Studio 2015, you can follow hello tutorial with hello [Azure SDK for .NET](../dotnet-sdk.md) 2.8.2 or later.</span></span> <span data-ttu-id="78d16-135">[Stažení hello nejnovější sadu Azure SDK pro Visual Studio 2015 tady](http://go.microsoft.com/fwlink/?linkid=518003).</span><span class="sxs-lookup"><span data-stu-id="78d16-135">[Download hello latest Azure SDK for Visual Studio 2015 here](http://go.microsoft.com/fwlink/?linkid=518003).</span></span> <span data-ttu-id="78d16-136">Visual Studio se automaticky nainstaluje s hello SDK, pokud ještě nemáte ho.</span><span class="sxs-lookup"><span data-stu-id="78d16-136">Visual Studio is automatically installed with hello SDK if you don't already have it.</span></span> <span data-ttu-id="78d16-137">Některé obrazovky se mohou lišit od ilustrací hello v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="78d16-137">Some screens may look different from hello illustrations shown in this tutorial.</span></span>

<span data-ttu-id="78d16-138">Pokud máte Visual Studio 2013, můžete [stažení hello nejnovější sadu Azure SDK pro Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322).</span><span class="sxs-lookup"><span data-stu-id="78d16-138">If you have Visual Studio 2013, you can [download hello latest Azure SDK for Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322).</span></span> <span data-ttu-id="78d16-139">Některé obrazovky se mohou lišit od ilustrací hello v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="78d16-139">Some screens may look different from hello illustrations shown in this tutorial.</span></span>

## <a name="create-hello-visual-studio-project"></a><span data-ttu-id="78d16-140">Vytvoření projektu Visual Studio hello</span><span class="sxs-lookup"><span data-stu-id="78d16-140">Create hello Visual Studio project</span></span>
1. <span data-ttu-id="78d16-141">Otevřete sadu Visual Studio a klikněte na **Soubor**, **Nový**, **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="78d16-141">Open Visual Studio and click **File**, **New**, **Project**.</span></span>
2. <span data-ttu-id="78d16-142">Rozbalte hello **Visual C#** uzel v hello **šablony** seznamu, vyberte **cloudu**a klikněte na tlačítko **webové aplikace ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="78d16-142">Expand hello **Visual C#** node in hello **Templates** list, select **Cloud**, and click **ASP.NET Web Application**.</span></span> <span data-ttu-id="78d16-143">Ujistěte se, že je vybrán **.NET Framework 4.5.2** nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="78d16-143">Ensure that **.NET Framework 4.5.2** or higher is selected.</span></span>  <span data-ttu-id="78d16-144">Typ **ContosoTeamStats** do hello **název** textové pole a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="78d16-144">Type **ContosoTeamStats** into hello **Name** textbox and click **OK**.</span></span>
   
    ![Vytvoření projektu][cache-create-project]
3. <span data-ttu-id="78d16-146">Vyberte **MVC** jako typ projektu hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-146">Select **MVC** as hello project type.</span></span> 

    <span data-ttu-id="78d16-147">Ujistěte se, že **bez ověřování** je zadán pro hello **ověřování** nastavení.</span><span class="sxs-lookup"><span data-stu-id="78d16-147">Ensure that **No Authentication** is specified for hello **Authentication** settings.</span></span> <span data-ttu-id="78d16-148">V závislosti na vaší verzi sady Visual Studio může být hello výchozí nastavení toosomething else.</span><span class="sxs-lookup"><span data-stu-id="78d16-148">Depending on your version of Visual Studio, hello default may be set toosomething else.</span></span> <span data-ttu-id="78d16-149">toochange, klikněte na tlačítko **změna ověřování** a vyberte **bez ověřování**.</span><span class="sxs-lookup"><span data-stu-id="78d16-149">toochange it, click **Change Authentication** and select **No Authentication**.</span></span>

    <span data-ttu-id="78d16-150">Pokud postupujete podle společně s Visual Studio 2015, zrušte hello **hostitel v cloudu hello** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="78d16-150">If you are following along with Visual Studio 2015, clear hello **Host in hello cloud** checkbox.</span></span> <span data-ttu-id="78d16-151">Budete [zřídit hello prostředků Azure](#provision-the-azure-resources) a [publikování tooAzure aplikace hello](#publish-the-application-to-azure) v dalších krocích hello kurzu.</span><span class="sxs-lookup"><span data-stu-id="78d16-151">You'll [provision hello Azure resources](#provision-the-azure-resources) and [publish hello application tooAzure](#publish-the-application-to-azure) in subsequent steps in hello tutorial.</span></span> <span data-ttu-id="78d16-152">Příklad zřízení webové aplikace služby App Service ze sady Visual Studio ponecháním **hostitel v cloudu hello** najdete v tématu [Začínáme s webovými aplikacemi ve službě Azure App Service pomocí ASP.NET a Visual Studio](../app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="78d16-152">For an example of provisioning an App Service web app from Visual Studio by leaving **Host in hello cloud** checked, see [Get started with Web Apps in Azure App Service, using ASP.NET and Visual Studio](../app-service-web/app-service-web-get-started-dotnet.md).</span></span>
   
    ![Výběr šablony projektu][cache-select-template]
4. <span data-ttu-id="78d16-154">Klikněte na tlačítko **OK** toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="78d16-154">Click **OK** toocreate hello project.</span></span>

## <a name="create-hello-aspnet-mvc-application"></a><span data-ttu-id="78d16-155">Vytvoření hello aplikace ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="78d16-155">Create hello ASP.NET MVC application</span></span>
<span data-ttu-id="78d16-156">V této části kurzu hello vytvoříte hello základní aplikaci, která načítá a zobrazuje týmové statistiky z databáze.</span><span class="sxs-lookup"><span data-stu-id="78d16-156">In this section of hello tutorial, you'll create hello basic application that reads and displays team statistics from a database.</span></span>

* [<span data-ttu-id="78d16-157">Přidání balíčku Entity Framework NuGet hello</span><span class="sxs-lookup"><span data-stu-id="78d16-157">Add hello Entity Framework NuGet package</span></span>](#add-the-entity-framework-nuget-package)
* [<span data-ttu-id="78d16-158">Přidání modelu hello</span><span class="sxs-lookup"><span data-stu-id="78d16-158">Add hello model</span></span>](#add-the-model)
* [<span data-ttu-id="78d16-159">Přidat řadič hello</span><span class="sxs-lookup"><span data-stu-id="78d16-159">Add hello controller</span></span>](#add-the-controller)
* [<span data-ttu-id="78d16-160">Konfigurace zobrazení hello</span><span class="sxs-lookup"><span data-stu-id="78d16-160">Configure hello views</span></span>](#configure-the-views)

### <a name="add-hello-entity-framework-nuget-package"></a><span data-ttu-id="78d16-161">Přidání balíčku Entity Framework NuGet hello</span><span class="sxs-lookup"><span data-stu-id="78d16-161">Add hello Entity Framework NuGet package</span></span>

1. <span data-ttu-id="78d16-162">Klikněte na tlačítko **Správce balíčků NuGet**, **Konzola správce balíčků** z hello **nástroje** nabídky.</span><span class="sxs-lookup"><span data-stu-id="78d16-162">Click **NuGet Package Manager**, **Package Manager Console** from hello **Tools** menu.</span></span>
2. <span data-ttu-id="78d16-163">Spuštění hello následující příkaz z hello **Konzola správce balíčků** okno.</span><span class="sxs-lookup"><span data-stu-id="78d16-163">Run hello following command from hello **Package Manager Console** window.</span></span>
    
    ```
    Install-Package EntityFramework
    ```

<span data-ttu-id="78d16-164">Další informace o tomto balíčku najdete v tématu hello [EntityFramework](https://www.nuget.org/packages/EntityFramework/) NuGet stránky.</span><span class="sxs-lookup"><span data-stu-id="78d16-164">For more information about this package, see hello [EntityFramework](https://www.nuget.org/packages/EntityFramework/) NuGet page.</span></span>

### <a name="add-hello-model"></a><span data-ttu-id="78d16-165">Přidání modelu hello</span><span class="sxs-lookup"><span data-stu-id="78d16-165">Add hello model</span></span>
1. <span data-ttu-id="78d16-166">V **Průzkumníku řešení** klikněte pravým tlačítkem na **Modely** a vyberte **Přidat**, **Třídu**.</span><span class="sxs-lookup"><span data-stu-id="78d16-166">Right-click **Models** in **Solution Explorer**, and choose **Add**, **Class**.</span></span> 
   
    ![Přidání modelu][cache-model-add-class]
2. <span data-ttu-id="78d16-168">Zadejte `Team` pro název třídy hello a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="78d16-168">Enter `Team` for hello class name and click **Add**.</span></span>
   
    ![Přidání třídy modelu][cache-model-add-class-dialog]
3. <span data-ttu-id="78d16-170">Nahraďte hello `using` příkazy hello horní části hello `Team.cs` soubor s následující hello `using` příkazy.</span><span class="sxs-lookup"><span data-stu-id="78d16-170">Replace hello `using` statements at hello top of hello `Team.cs` file with hello following `using` statements.</span></span>

    ```c#
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Data.Entity.SqlServer;
    ```


1. <span data-ttu-id="78d16-171">Nahraďte definici hello hello `Team` se hello následující fragment kódu, který obsahuje kromě aktualizované `Team` třídy definice, jakož i některé další pomocné třídy Entity Frameworku.</span><span class="sxs-lookup"><span data-stu-id="78d16-171">Replace hello definition of hello `Team` class with hello following code snippet that contains an updated `Team` class definition as well as some other Entity Framework helper classes.</span></span> <span data-ttu-id="78d16-172">Další informace o hello kód první přístup tooEntity Framework, který se používá v tomto kurzu, najdete v části [kód první tooa novou databázi](https://msdn.microsoft.com/data/jj193542).</span><span class="sxs-lookup"><span data-stu-id="78d16-172">For more information on hello code first approach tooEntity Framework that's used in this tutorial, see [Code first tooa new database](https://msdn.microsoft.com/data/jj193542).</span></span>

    ```c#
    public class Team
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public int Wins { get; set; }
        public int Losses { get; set; }
        public int Ties { get; set; }
    
        static public void PlayGames(IEnumerable<Team> teams)
        {
            // Simple random generation of statistics.
            Random r = new Random();
    
            foreach (var t in teams)
            {
                t.Wins = r.Next(33);
                t.Losses = r.Next(33);
                t.Ties = r.Next(0, 5);
            }
        }
    }
    
    public class TeamContext : DbContext
    {
        public TeamContext()
            : base("TeamContext")
        {
        }
    
        public DbSet<Team> Teams { get; set; }
    }
    
    public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
    {
        protected override void Seed(TeamContext context)
        {
            var teams = new List<Team>
            {
                new Team{Name="Adventure Works Cycles"},
                new Team{Name="Alpine Ski House"},
                new Team{Name="Blue Yonder Airlines"},
                new Team{Name="Coho Vineyard"},
                new Team{Name="Contoso, Ltd."},
                new Team{Name="Fabrikam, Inc."},
                new Team{Name="Lucerne Publishing"},
                new Team{Name="Northwind Traders"},
                new Team{Name="Consolidated Messenger"},
                new Team{Name="Fourth Coffee"},
                new Team{Name="Graphic Design Institute"},
                new Team{Name="Nod Publishers"}
            };
    
            Team.PlayGames(teams);
    
            teams.ForEach(t => context.Teams.Add(t));
            context.SaveChanges();
        }
    }
    
    public class TeamConfiguration : DbConfiguration
    {
        public TeamConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
        }
    }
    ```


1. <span data-ttu-id="78d16-173">V **Průzkumníku řešení**, dvakrát klikněte na **web.config** tooopen ho.</span><span class="sxs-lookup"><span data-stu-id="78d16-173">In **Solution Explorer**, double-click **web.config** tooopen it.</span></span>
   
    ![Soubor web.config][cache-web-config]
2. <span data-ttu-id="78d16-175">Přidejte následující hello `connectionStrings` části.</span><span class="sxs-lookup"><span data-stu-id="78d16-175">Add hello following `connectionStrings` section.</span></span> <span data-ttu-id="78d16-176">Název Hello hello připojovací řetězec musí odpovídat názvu hello hello třídy kontextu databáze Entity Framework, který je `TeamContext`.</span><span class="sxs-lookup"><span data-stu-id="78d16-176">hello name of hello connection string must match hello name of hello Entity Framework database context class which is `TeamContext`.</span></span>

    ```xml
    <connectionStrings>
        <add name="TeamContext" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    <span data-ttu-id="78d16-177">Můžete přidat nové hello `connectionStrings` tak, aby postupuje `configSections`, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-177">You can add hello new `connectionStrings` section so that it follows `configSections`, as shown in hello following example.</span></span>

    ```xml
    <configuration>
      <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
      </configSections>
      <connectionStrings>
        <add name="TeamContext" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
      </connectionStrings>
      ...
      ```

    > [!NOTE]
    > <span data-ttu-id="78d16-178">Připojovací řetězec se může lišit v závislosti na verzi hello sady Visual Studio a edice systému SQL Server Express používat toocomplete hello kurzu.</span><span class="sxs-lookup"><span data-stu-id="78d16-178">Your connection string may be different depending on hello version of Visual Studio and SQL Server Express edition used toocomplete hello tutorial.</span></span> <span data-ttu-id="78d16-179">Hello web.config šablona by měla být nakonfigurovaná toomatch instalaci a může obsahovat `Data Source` jako položky `(LocalDB)\v11.0` (z SQL serveru Express 2012) nebo `Data Source=(LocalDB)\MSSQLLocalDB` (z SQL serveru Express 2014 a novější).</span><span class="sxs-lookup"><span data-stu-id="78d16-179">hello web.config template should be configured toomatch your installation, and may contain `Data Source` entries like `(LocalDB)\v11.0` (from SQL Server Express 2012) or `Data Source=(LocalDB)\MSSQLLocalDB` (from SQL Server Express 2014 and newer).</span></span> <span data-ttu-id="78d16-180">Další informace o připojovacích řetězcích a verzích SQL Express najdete v tématu [SQL Server 2016 Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="78d16-180">For more information about connection strings and SQL Express versions, see [SQL Server 2016 Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) .</span></span>

### <a name="add-hello-controller"></a><span data-ttu-id="78d16-181">Přidat řadič hello</span><span class="sxs-lookup"><span data-stu-id="78d16-181">Add hello controller</span></span>
1. <span data-ttu-id="78d16-182">Stiskněte klávesu **F6** toobuild hello projektu.</span><span class="sxs-lookup"><span data-stu-id="78d16-182">Press **F6** toobuild hello project.</span></span> 
2. <span data-ttu-id="78d16-183">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **řadiče** složky a vyberte **přidat**, **řadič**.</span><span class="sxs-lookup"><span data-stu-id="78d16-183">In **Solution Explorer**, right-click hello **Controllers** folder and choose **Add**, **Controller**.</span></span>
   
    ![Přidání kontroleru][cache-add-controller]
3. <span data-ttu-id="78d16-185">Vyberte **Kontroler MVC 5 se zobrazeními, s použitím Entity Frameworku**, a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="78d16-185">Choose **MVC 5 Controller with views, using Entity Framework**, and click **Add**.</span></span> <span data-ttu-id="78d16-186">Pokud dojde k chybě po kliknutí na **přidat**, ujistěte se, že jste vytvořili projekt hello nejdřív.</span><span class="sxs-lookup"><span data-stu-id="78d16-186">If you get an error after clicking **Add**, ensure that you have built hello project first.</span></span>
   
    ![Přidání třídy kontroleru][cache-add-controller-class]
4. <span data-ttu-id="78d16-188">Vyberte **Team (ContosoTeamStats.Models)** z hello **třída modelu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="78d16-188">Select **Team (ContosoTeamStats.Models)** from hello **Model class** drop-down list.</span></span> <span data-ttu-id="78d16-189">Vyberte **TeamContext (ContosoTeamStats.Models)** z hello **třída kontextu dat** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="78d16-189">Select **TeamContext (ContosoTeamStats.Models)** from hello **Data context class** drop-down list.</span></span> <span data-ttu-id="78d16-190">Typ `TeamsController` v hello **řadič** textového pole název (Pokud není vyplněné automaticky).</span><span class="sxs-lookup"><span data-stu-id="78d16-190">Type `TeamsController` in hello **Controller** name textbox (if it is not automatically populated).</span></span> <span data-ttu-id="78d16-191">Klikněte na tlačítko **přidat** toocreate hello třídy kontroleru a přidáte výchozí zobrazení hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-191">Click **Add** toocreate hello controller class and add hello default views.</span></span>
   
    ![Konfigurace kontroleru][cache-configure-controller]
5. <span data-ttu-id="78d16-193">V **Průzkumníku řešení**, rozbalte položku **Global.asax** a dvakrát klikněte na **Global.asax.cs** tooopen ho.</span><span class="sxs-lookup"><span data-stu-id="78d16-193">In **Solution Explorer**, expand **Global.asax** and double-click **Global.asax.cs** tooopen it.</span></span>
   
    ![Soubor Global.asax.cs][cache-global-asax]
6. <span data-ttu-id="78d16-195">Přidejte následující dvě hello `using` příkazy hello horní části souboru hello pod hello jiných `using` příkazy.</span><span class="sxs-lookup"><span data-stu-id="78d16-195">Add hello following two `using` statements at hello top of hello file under hello other `using` statements.</span></span>

    ```c#
    using System.Data.Entity;
    using ContosoTeamStats.Models;
    ```


1. <span data-ttu-id="78d16-196">Přidejte následující řádek kódu na konci hello hello hello `Application_Start` metoda.</span><span class="sxs-lookup"><span data-stu-id="78d16-196">Add hello following line of code at hello end of hello `Application_Start` method.</span></span>

    ```c#
    Database.SetInitializer<TeamContext>(new TeamInitializer());
    ```


1. <span data-ttu-id="78d16-197">V **Průzkumníku řešení** rozbalte `App_Start` a dvakrát klikněte na soubor `RouteConfig.cs`.</span><span class="sxs-lookup"><span data-stu-id="78d16-197">In **Solution Explorer**, expand `App_Start` and double-click `RouteConfig.cs`.</span></span>
   
    ![Soubor RouteConfig.cs][cache-RouteConfig-cs]
2. <span data-ttu-id="78d16-199">Nahraďte `controller = "Home"` v hello následující kód v hello `RegisterRoutes` metoda s `controller = "Teams"` jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-199">Replace `controller = "Home"` in hello following code in hello `RegisterRoutes` method with `controller = "Teams"` as shown in hello following example.</span></span>

    ```c#
    routes.MapRoute(
        name: "Default",
        url: "{controller}/{action}/{id}",
        defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
    );
    ```


### <a name="configure-hello-views"></a><span data-ttu-id="78d16-200">Konfigurace zobrazení hello</span><span class="sxs-lookup"><span data-stu-id="78d16-200">Configure hello views</span></span>
1. <span data-ttu-id="78d16-201">V **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku a potom hello **sdílené** složku a dvojím kliknutím **_Layout.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="78d16-201">In **Solution Explorer**, expand hello **Views** folder and then hello **Shared** folder, and double-click **_Layout.cshtml**.</span></span> 
   
    ![Soubor _Layout.cshtml][cache-layout-cshtml]
2. <span data-ttu-id="78d16-203">Změnit obsah hello hello `title` elementu a nahraďte `My ASP.NET Application` s `Contoso Team Stats` jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-203">Change hello contents of hello `title` element and replace `My ASP.NET Application` with `Contoso Team Stats` as shown in hello following example.</span></span>

    ```html
    <title>@ViewBag.Title - Contoso Team Stats</title>
    ```


1. <span data-ttu-id="78d16-204">V hello `body` část, nejdřív aktualizovat hello `Html.ActionLink` příkaz a nahraďte `Application name` s `Contoso Team Stats` a nahraďte `Home` s `Teams`.</span><span class="sxs-lookup"><span data-stu-id="78d16-204">In hello `body` section, update hello first `Html.ActionLink` statement and replace `Application name` with `Contoso Team Stats` and replace `Home` with `Teams`.</span></span>
   
   * <span data-ttu-id="78d16-205">Před: `@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`</span><span class="sxs-lookup"><span data-stu-id="78d16-205">Before: `@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`</span></span>
   * <span data-ttu-id="78d16-206">Po: `@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`</span><span class="sxs-lookup"><span data-stu-id="78d16-206">After: `@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`</span></span>
     
     ![Změny kódu][cache-layout-cshtml-code]
2. <span data-ttu-id="78d16-208">Stiskněte klávesu **Ctrl + F5** toobuild a spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-208">Press **Ctrl+F5** toobuild and run hello application.</span></span> <span data-ttu-id="78d16-209">Tato verze aplikace hello čte hello výsledky přímo z databáze hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-209">This version of hello application reads hello results directly from hello database.</span></span> <span data-ttu-id="78d16-210">Poznámka: hello **vytvořit nový**, **upravit**, **podrobnosti**, a **odstranit** akce, které byly automaticky přidá toohello aplikace hello **Kontroler MVC 5 se zobrazeními s využitím nástroje Entity Framework** vygenerované uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="78d16-210">Note hello **Create New**, **Edit**, **Details**, and **Delete** actions that were automatically added toohello application by hello **MVC 5 Controller with views, using Entity Framework** scaffold.</span></span> <span data-ttu-id="78d16-211">V další části kurzu hello hello přidáte Redis Cache toooptimize hello data přístup a poskytnutí dodatečných funkcí aplikaci toohello.</span><span class="sxs-lookup"><span data-stu-id="78d16-211">In hello next section of hello tutorial you'll add Redis Cache toooptimize hello data access and provide additional features toohello application.</span></span>

![Startovní aplikace][cache-starter-application]

## <a name="configure-hello-application-toouse-redis-cache"></a><span data-ttu-id="78d16-213">Konfigurace aplikace toouse hello Redis Cache</span><span class="sxs-lookup"><span data-stu-id="78d16-213">Configure hello application toouse Redis Cache</span></span>
<span data-ttu-id="78d16-214">V této části kurzu hello budete konfigurovat hello ukázkové aplikace toostore a načítání týmových statistik Contoso z instance Azure Redis Cache pomocí hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) mezipaměti klienta.</span><span class="sxs-lookup"><span data-stu-id="78d16-214">In this section of hello tutorial, you'll configure hello sample application toostore and retrieve Contoso team statistics from an Azure Redis Cache instance by using hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) cache client.</span></span>

* [<span data-ttu-id="78d16-215">Konfigurace aplikace toouse hello StackExchange.Redis</span><span class="sxs-lookup"><span data-stu-id="78d16-215">Configure hello application toouse StackExchange.Redis</span></span>](#configure-the-application-to-use-stackexchangeredis)
* [<span data-ttu-id="78d16-216">Aktualizaci hello TeamsController třída tooreturn výsledků z mezipaměti hello nebo hello databáze</span><span class="sxs-lookup"><span data-stu-id="78d16-216">Update hello TeamsController class tooreturn results from hello cache or hello database</span></span>](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
* [<span data-ttu-id="78d16-217">Hello vytvořit, upravit, aktualizovat a odstranit toowork metody s mezipamětí hello</span><span class="sxs-lookup"><span data-stu-id="78d16-217">Update hello Create, Edit, and Delete methods toowork with hello cache</span></span>](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
* [<span data-ttu-id="78d16-218">Aktualizovat toowork zobrazení Teams Index hello s mezipamětí hello</span><span class="sxs-lookup"><span data-stu-id="78d16-218">Update hello Teams Index view toowork with hello cache</span></span>](#update-the-teams-index-view-to-work-with-the-cache)

### <a name="configure-hello-application-toouse-stackexchangeredis"></a><span data-ttu-id="78d16-219">Konfigurace aplikace toouse hello StackExchange.Redis</span><span class="sxs-lookup"><span data-stu-id="78d16-219">Configure hello application toouse StackExchange.Redis</span></span>
1. <span data-ttu-id="78d16-220">Klikněte na tlačítko tooconfigure klientskou aplikaci v sadě Visual Studio pomocí balíčku StackExchange.Redis NuGet, hello **Správce balíčků NuGet**, **Konzola správce balíčků** z hello **nástroje** nabídky.</span><span class="sxs-lookup"><span data-stu-id="78d16-220">tooconfigure a client application in Visual Studio using hello StackExchange.Redis NuGet package, click **NuGet Package Manager**, **Package Manager Console** from hello **Tools** menu.</span></span>
2. <span data-ttu-id="78d16-221">Spuštění hello následující příkaz z hello `Package Manager Console` okno.</span><span class="sxs-lookup"><span data-stu-id="78d16-221">Run hello following command from hello `Package Manager Console` window.</span></span>
    
    ```
    Install-Package StackExchange.Redis
    ```
   
    <span data-ttu-id="78d16-222">Hello NuGet balíček stáhne a přidá hello požadované odkazy na sestavení pro vašeho klienta aplikace tooaccess Azure Redis Cache pomocí klienta mezipaměti StackExchange.Redis hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-222">hello NuGet package downloads and adds hello required assembly references for your client application tooaccess Azure Redis Cache with hello StackExchange.Redis cache client.</span></span> <span data-ttu-id="78d16-223">Pokud dáváte přednost toouse silným názvem verzi hello `StackExchange.Redis` klientské knihovny, instalace hello `StackExchange.Redis.StrongName` balíčku.</span><span class="sxs-lookup"><span data-stu-id="78d16-223">If you prefer toouse a strong-named version of hello `StackExchange.Redis` client library, install hello `StackExchange.Redis.StrongName` package.</span></span>
3. <span data-ttu-id="78d16-224">V **Průzkumníku řešení**, rozbalte položku hello **řadiče** složku a dvojím kliknutím **TeamsController.cs** tooopen ho.</span><span class="sxs-lookup"><span data-stu-id="78d16-224">In **Solution Explorer**, expand hello **Controllers** folder and double-click **TeamsController.cs** tooopen it.</span></span>
   
    ![Kontroler Teams][cache-teamscontroller]
4. <span data-ttu-id="78d16-226">Přidejte následující dvě hello `using` příkazy příliš**TeamsController.cs**.</span><span class="sxs-lookup"><span data-stu-id="78d16-226">Add hello following two `using` statements too**TeamsController.cs**.</span></span>

    ```c#   
    using System.Configuration;
    using StackExchange.Redis;
    ```

5. <span data-ttu-id="78d16-227">Přidejte následující dvě vlastnosti toohello hello `TeamsController` třídy.</span><span class="sxs-lookup"><span data-stu-id="78d16-227">Add hello following two properties toohello `TeamsController` class.</span></span>

    ```c#   
    // Redis Connection string info
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
        return ConnectionMultiplexer.Connect(cacheConnection);
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ```

6. <span data-ttu-id="78d16-228">Vytvořte soubor v počítači s názvem `WebAppPlusCacheAppSecrets.config` a jeho následné uložení do umístění, které nebudou navázalo kontakt se hello zdrojový kód ukázkovou aplikaci, musí rozhodnout toocheck ji někam registrovat.</span><span class="sxs-lookup"><span data-stu-id="78d16-228">Create a file on your computer named `WebAppPlusCacheAppSecrets.config` and place it in a location that won't be checked in with hello source code of your sample application, should you decide toocheck it in somewhere.</span></span> <span data-ttu-id="78d16-229">V tento příklad hello `AppSettingsSecrets.config` soubor se nachází v `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.</span><span class="sxs-lookup"><span data-stu-id="78d16-229">In this example hello `AppSettingsSecrets.config` file is located at `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.</span></span>
   
    <span data-ttu-id="78d16-230">Upravit hello `WebAppPlusCacheAppSecrets.config` souboru a přidejte hello následující obsah.</span><span class="sxs-lookup"><span data-stu-id="78d16-230">Edit hello `WebAppPlusCacheAppSecrets.config` file and add hello following contents.</span></span> <span data-ttu-id="78d16-231">Pokud místní spuštění aplikace hello tyto informace jsou použité tooconnect tooyour Azure Redis Cache instance.</span><span class="sxs-lookup"><span data-stu-id="78d16-231">If you run hello application locally this information is used tooconnect tooyour Azure Redis Cache instance.</span></span> <span data-ttu-id="78d16-232">Později v kurzu hello budete zřídíte instanci služby Azure Redis Cache a aktualizovat hello mezipaměti jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="78d16-232">Later in hello tutorial you'll provision an Azure Redis Cache instance and update hello cache name and password.</span></span> <span data-ttu-id="78d16-233">Pokud neplánujete toorun hello ukázkovou aplikaci místně, můžete přeskočit hello vytvoření tohoto souboru a hello následné kroky, které odkazují hello souboru, protože při nasazení aplikace hello tooAzure načítá informace o připojení hello mezipaměti z aplikace hello nastavení pro hello webovou aplikaci a ne z tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="78d16-233">If you don't plan toorun hello sample application locally you can skip hello creation of this file and hello subsequent steps that reference hello file, because when you deploy tooAzure hello application retrieves hello cache connection information from hello app setting for hello Web App and not from this file.</span></span> <span data-ttu-id="78d16-234">Od hello `WebAppPlusCacheAppSecrets.config` není nasazený tooAzure s vaší aplikací, nebudete ho potřebovat, pokud budete toorun hello aplikace místně.</span><span class="sxs-lookup"><span data-stu-id="78d16-234">Since hello `WebAppPlusCacheAppSecrets.config` is not deployed tooAzure with your application, you don't need it unless you are going toorun hello application locally.</span></span>

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. <span data-ttu-id="78d16-235">V **Průzkumníku řešení**, dvakrát klikněte na **web.config** tooopen ho.</span><span class="sxs-lookup"><span data-stu-id="78d16-235">In **Solution Explorer**, double-click **web.config** tooopen it.</span></span>
   
    ![Soubor web.config][cache-web-config]
2. <span data-ttu-id="78d16-237">Přidejte následující hello `file` atribut toohello `appSettings` element.</span><span class="sxs-lookup"><span data-stu-id="78d16-237">Add hello following `file` attribute toohello `appSettings` element.</span></span> <span data-ttu-id="78d16-238">Pokud jste použili jiný název souboru nebo umístění, nahraďte těmito hodnotami hello ty, které jsou v příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-238">If you used a different file name or location, substitute those values for hello ones shown in hello example.</span></span>
   
   * <span data-ttu-id="78d16-239">Před: `<appSettings>`</span><span class="sxs-lookup"><span data-stu-id="78d16-239">Before: `<appSettings>`</span></span>
   * <span data-ttu-id="78d16-240">Po: ` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`</span><span class="sxs-lookup"><span data-stu-id="78d16-240">After: ` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`</span></span>
     
   <span data-ttu-id="78d16-241">Hello ASP.NET runtime sloučí obsah hello hello externího souboru se hello značek v hello `<appSettings>` elementu.</span><span class="sxs-lookup"><span data-stu-id="78d16-241">hello ASP.NET runtime merges hello contents of hello external file with hello markup in hello `<appSettings>` element.</span></span> <span data-ttu-id="78d16-242">Hello runtime ignoruje atribut souboru hello, pokud nelze nalézt zadaný soubor hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-242">hello runtime ignores hello file attribute if hello specified file cannot be found.</span></span> <span data-ttu-id="78d16-243">Vaše tajné klíče (hello připojovací řetězec tooyour mezipaměti) nejsou součástí hello zdrojový kód aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-243">Your secrets (hello connection string tooyour cache) are not included as part of hello source code for hello application.</span></span> <span data-ttu-id="78d16-244">Při nasazení vaší webové aplikace tooAzure hello `WebAppPlusCacheAppSecrests.config` soubor nebude možné nasadit (to znamená, co chcete použít).</span><span class="sxs-lookup"><span data-stu-id="78d16-244">When you deploy your web app tooAzure, hello `WebAppPlusCacheAppSecrests.config` file won't be deployed (that's what you want).</span></span> <span data-ttu-id="78d16-245">Existuje několik způsobů toospecify těchto tajných klíčů v Azure, a v tomto kurzu jsou konfigurovány automaticky za vás při můžete [zřízení prostředků Azure hello](#provision-the-azure-resources) v dalším kroku kurzu.</span><span class="sxs-lookup"><span data-stu-id="78d16-245">There are several ways toospecify these secrets in Azure, and in this tutorial they are configured automatically for you when you [provision hello Azure resources](#provision-the-azure-resources) in a subsequent tutorial step.</span></span> <span data-ttu-id="78d16-246">Další informace o práci s tajnými klíči v Azure najdete v tématu [osvědčené postupy pro nasazování hesel a dalších citlivých dat tooASP.NET a Azure App Service](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).</span><span class="sxs-lookup"><span data-stu-id="78d16-246">For more information about working with secrets in Azure, see [Best practices for deploying passwords and other sensitive data tooASP.NET and Azure App Service](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).</span></span>

### <a name="update-hello-teamscontroller-class-tooreturn-results-from-hello-cache-or-hello-database"></a><span data-ttu-id="78d16-247">Aktualizaci hello TeamsController třída tooreturn výsledků z mezipaměti hello nebo hello databáze</span><span class="sxs-lookup"><span data-stu-id="78d16-247">Update hello TeamsController class tooreturn results from hello cache or hello database</span></span>
<span data-ttu-id="78d16-248">V této ukázce lze týmové statistiky načíst z databáze hello nebo z mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-248">In this sample, team statistics can be retrieved from hello database or from hello cache.</span></span> <span data-ttu-id="78d16-249">Týmové statistiky jsou uloženy v mezipaměti hello jako serializovaný seznam `List<Team>`a také jako seřazená sada pomocí datových typů Redis.</span><span class="sxs-lookup"><span data-stu-id="78d16-249">Team statistics are stored in hello cache as a serialized `List<Team>`, and also as a sorted set using Redis data types.</span></span> <span data-ttu-id="78d16-250">Při načítání položek ze seřazené sady můžete načíst část položek, všechny položky nebo provézt dotaz na určité položky.</span><span class="sxs-lookup"><span data-stu-id="78d16-250">When retrieving items from a sorted set, you can retrieve some, all, or query for certain items.</span></span> <span data-ttu-id="78d16-251">V této ukázce dotazujete hello seřazené sady hello top 5 týmů podle počtu vítězství.</span><span class="sxs-lookup"><span data-stu-id="78d16-251">In this sample you'll query hello sorted set for hello top 5 teams ranked by number of wins.</span></span>

> [!NOTE]
> <span data-ttu-id="78d16-252">Není požadováno toostore hello týmové statistiky ve více formátech v mezipaměti hello v pořadí toouse Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="78d16-252">It is not required toostore hello team statistics in multiple formats in hello cache in order toouse Azure Redis Cache.</span></span> <span data-ttu-id="78d16-253">Tento kurz používá více formátů toodemonstrate některé hello různé způsoby a různé datové typy toocache data můžete použít.</span><span class="sxs-lookup"><span data-stu-id="78d16-253">This tutorial uses multiple formats toodemonstrate some of hello different ways and different data types you can use toocache data.</span></span>
> 
> 

1. <span data-ttu-id="78d16-254">Přidejte následující hello `using` příkazy toohello `TeamsController.cs` souboru v horní části hello s hello jiných `using` příkazy.</span><span class="sxs-lookup"><span data-stu-id="78d16-254">Add hello following `using` statements toohello `TeamsController.cs` file at hello top with hello other `using` statements.</span></span>

    ```c#   
    using System.Diagnostics;
    using Newtonsoft.Json;
    ```

2. <span data-ttu-id="78d16-255">Nahradit aktuální hello `public ActionResult Index()` implementace metod s hello následující implementaci.</span><span class="sxs-lookup"><span data-stu-id="78d16-255">Replace hello current `public ActionResult Index()` method implementation with hello following implementation.</span></span>

    ```c#
    // GET: Teams
    public ActionResult Index(string actionType, string resultType)
    {
        List<Team> teams = null;

        switch(actionType)
        {
            case "playGames": // Play a new season of games.
                PlayGames();
                break;

            case "clearCache": // Clear hello results from hello cache.
                ClearCachedTeams();
                break;

            case "rebuildDB": // Rebuild hello database with sample data.
                RebuildDB();
                break;
        }

        // Measure hello time it takes tooretrieve hello results.
        Stopwatch sw = Stopwatch.StartNew();

        switch(resultType)
        {
            case "teamsSortedSet": // Retrieve teams from sorted set.
                teams = GetFromSortedSet();
                break;

            case "teamsSortedSetTop5": // Retrieve hello top 5 teams from hello sorted set.
                teams = GetFromSortedSetTop5();
                break;

            case "teamsList": // Retrieve teams from hello cached List<Team>.
                teams = GetFromList();
                break;

            case "fromDB": // Retrieve results from hello database.
            default:
                teams = GetFromDB();
                break;
        }

        sw.Stop();
        double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

        // Add hello elapsed time of hello operation toohello ViewBag.msg.
        ViewBag.msg += " MS: " + ms.ToString();

        return View(teams);
    }
    ```


1. <span data-ttu-id="78d16-256">Přidejte následující tři metody toohello hello `TeamsController` třída tooimplement hello `playGames`, `clearCache`, a `rebuildDB` typy akcí z hello switch – příkaz přidali v předchozím fragmentu kódu hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-256">Add hello following three methods toohello `TeamsController` class tooimplement hello `playGames`, `clearCache`, and `rebuildDB` action types from hello switch statement added in hello previous code snippet.</span></span>
   
    <span data-ttu-id="78d16-257">Hello `PlayGames` metoda aktualizací hello týmové statistiky tak, že simuluje sezónu her, uloží hello výsledky toohello databáze a vymaže hello teď zastaralých dat z mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-257">hello `PlayGames` method updates hello team statistics by simulating a season of games, saves hello results toohello database, and clears hello now outdated data from hello cache.</span></span>

    ```c#
    void PlayGames()
    {
        ViewBag.msg += "Updating team statistics. ";
        // Play a "season" of games.
        var teams = from t in db.Teams
                    select t;

        Team.PlayGames(teams);

        db.SaveChanges();

        // Clear any cached results
        ClearCachedTeams();
    }
    ```

    <span data-ttu-id="78d16-258">Hello `RebuildDB` metoda inicializaci hello databáze s hello výchozí sadou týmů, vygeneruje pro ně statistiky, a vymaže hello teď zastaralých dat z mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-258">hello `RebuildDB` method reinitializes hello database with hello default set of teams, generates statistics for them, and clears hello now outdated data from hello cache.</span></span>

    ```c#
    void RebuildDB()
    {
        ViewBag.msg += "Rebuilding DB. ";
        // Delete and re-initialize hello database with sample data.
        db.Database.Delete();
        db.Database.Initialize(true);

        // Clear any cached results
        ClearCachedTeams();
    }
    ```

    <span data-ttu-id="78d16-259">Hello `ClearCachedTeams` metoda odebere všechny uložené týmové statistiky z mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-259">hello `ClearCachedTeams` method removes any cached team statistics from hello cache.</span></span>

    ```c#
    void ClearCachedTeams()
    {
        IDatabase cache = Connection.GetDatabase();
        cache.KeyDelete("teamsList");
        cache.KeyDelete("teamsSortedSet");
        ViewBag.msg += "Team data removed from cache. ";
    } 
    ```


1. <span data-ttu-id="78d16-260">Přidejte následující čtyři metody toohello hello `TeamsController` třída tooimplement hello různých způsobů načítání hello týmové statistiky z mezipaměti hello a hello databáze.</span><span class="sxs-lookup"><span data-stu-id="78d16-260">Add hello following four methods toohello `TeamsController` class tooimplement hello various ways of retrieving hello team statistics from hello cache and hello database.</span></span> <span data-ttu-id="78d16-261">Každá z těchto metod vrací `List<Team>` který se následně zobrazí hello zobrazení.</span><span class="sxs-lookup"><span data-stu-id="78d16-261">Each of these methods returns a `List<Team>` which is then displayed by hello view.</span></span>
   
    <span data-ttu-id="78d16-262">Hello `GetFromDB` metoda čte hello týmové statistiky z databáze hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-262">hello `GetFromDB` method reads hello team statistics from hello database.</span></span>
   
    ```c#
    List<Team> GetFromDB()
    {
        ViewBag.msg += "Results read from DB. ";
        var results = from t in db.Teams
            orderby t.Wins descending
            select t; 

        return results.ToList<Team>();
    }
    ```

    <span data-ttu-id="78d16-263">Hello `GetFromList` metoda čte hello týmové statistiky z mezipaměti jako serializovaný seznam `List<Team>`.</span><span class="sxs-lookup"><span data-stu-id="78d16-263">hello `GetFromList` method reads hello team statistics from cache as a serialized `List<Team>`.</span></span> <span data-ttu-id="78d16-264">Pokud dojde k neúspěšnému přístupu do mezipaměti, týmové statistiky hello jsou čtení z databáze hello a pak uloženy v mezipaměti hello pro další použití.</span><span class="sxs-lookup"><span data-stu-id="78d16-264">If there is a cache miss, hello team statistics are read from hello database and then stored in hello cache for next time.</span></span> <span data-ttu-id="78d16-265">V této ukázce používáme JSON.NET serializace tooserialize hello .NET objekty tooand z mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-265">In this sample we're using JSON.NET serialization tooserialize hello .NET objects tooand from hello cache.</span></span> <span data-ttu-id="78d16-266">Další informace najdete v tématu [jak toowork s .NET objekty ve službě Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).</span><span class="sxs-lookup"><span data-stu-id="78d16-266">For more information, see [How toowork with .NET objects in Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).</span></span>

    ```c#
    List<Team> GetFromList()
    {
        List<Team> teams = null;

        IDatabase cache = Connection.GetDatabase();
        string serializedTeams = cache.StringGet("teamsList");
        if (!String.IsNullOrEmpty(serializedTeams))
        {
            teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

            ViewBag.msg += "List read from cache. ";
        }
        else
        {
            ViewBag.msg += "Teams list cache miss. ";
            // Get from database and store in cache
            teams = GetFromDB();

            ViewBag.msg += "Storing results toocache. ";
            cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
        }
        return teams;
    }
    ```

    <span data-ttu-id="78d16-267">Hello `GetFromSortedSet` metoda přečte hello týmové statistiky ze seřazené sady v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="78d16-267">hello `GetFromSortedSet` method reads hello team statistics from a cached sorted set.</span></span> <span data-ttu-id="78d16-268">Pokud dojde k neúspěšnému přístupu do mezipaměti, týmové statistiky hello jsou čtení z databáze hello a uloženy v hello mezipaměti jako seřazená sada.</span><span class="sxs-lookup"><span data-stu-id="78d16-268">If there is a cache miss, hello team statistics are read from hello database and stored in hello cache as a sorted set.</span></span>

    ```c#
    List<Team> GetFromSortedSet()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();
        // If hello key teamsSortedSet is not present, this method returns a 0 length collection.
        var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
        if (teamsSortedSet.Count() > 0)
        {
            ViewBag.msg += "Reading sorted set from cache. ";
            teams = new List<Team>();
            foreach (var t in teamsSortedSet)
            {
                Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                teams.Add(tt);
            }
        }
        else
        {
            ViewBag.msg += "Teams sorted set cache miss. ";

            // Read from DB
            teams = GetFromDB();

            ViewBag.msg += "Storing results toocache. ";
            foreach (var t in teams)
            {
                Console.WriteLine("Adding toosorted set: {0} - {1}", t.Name, t.Wins);
                cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
            }
        }
        return teams;
    }
    ```

    <span data-ttu-id="78d16-269">Hello `GetFromSortedSetTop5` metoda čte hello top 5 týmů z mezipaměti hello Seřazená sada.</span><span class="sxs-lookup"><span data-stu-id="78d16-269">hello `GetFromSortedSetTop5` method reads hello top 5 teams from hello cached sorted set.</span></span> <span data-ttu-id="78d16-270">Začne tím hello mezipaměti hello existenci hello `teamsSortedSet` klíč.</span><span class="sxs-lookup"><span data-stu-id="78d16-270">It starts by checking hello cache for hello existence of hello `teamsSortedSet` key.</span></span> <span data-ttu-id="78d16-271">Pokud tento klíč neexistuje, hello `GetFromSortedSet` metoda je volána tooread hello týmové statistiky a uložit je do mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-271">If this key is not present, hello `GetFromSortedSet` method is called tooread hello team statistics and store them in hello cache.</span></span> <span data-ttu-id="78d16-272">V dalším kroku hello seřazené sady v mezipaměti je dotazován na hello top 5 týmů, které jsou vráceny.</span><span class="sxs-lookup"><span data-stu-id="78d16-272">Next, hello cached sorted set is queried for hello top 5 teams which are returned.</span></span>

    ```c#
    List<Team> GetFromSortedSetTop5()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();

        // If hello key teamsSortedSet is not present, this method returns a 0 length collection.
        var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        if(teamsSortedSet.Count() == 0)
        {
            // Load hello entire sorted set into hello cache.
            GetFromSortedSet();

            // Retrieve hello top 5 teams.
            teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        }

        ViewBag.msg += "Retrieving top 5 teams from cache. ";
        // Get hello top 5 teams from hello sorted set
        teams = new List<Team>();
        foreach (var team in teamsSortedSet)
        {
            teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
        }
        return teams;
    }
    ```

### <a name="update-hello-create-edit-and-delete-methods-toowork-with-hello-cache"></a><span data-ttu-id="78d16-273">Hello vytvořit, upravit, aktualizovat a odstranit toowork metody s mezipamětí hello</span><span class="sxs-lookup"><span data-stu-id="78d16-273">Update hello Create, Edit, and Delete methods toowork with hello cache</span></span>
<span data-ttu-id="78d16-274">Kód generování uživatelského rozhraní Hello, který byl vytvořen jako součást této ukázky obsahuje metody tooadd, upravovat a odstraňovat týmy.</span><span class="sxs-lookup"><span data-stu-id="78d16-274">hello scaffolding code that was generated as part of this sample includes methods tooadd, edit, and delete teams.</span></span> <span data-ttu-id="78d16-275">Kdykoliv tým přidat, upravit nebo odebrat, hello data v hello mezipaměti stanou zastaralými.</span><span class="sxs-lookup"><span data-stu-id="78d16-275">Anytime a team is added, edited, or removed, hello data in hello cache becomes outdated.</span></span> <span data-ttu-id="78d16-276">V této části, upravte tyto tři metody tooclear hello do mezipaměti týmy tak, aby hello mezipaměť nebude synchronizována s databází hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-276">In this section you'll modify these three methods tooclear hello cached teams so that hello cache won't be out of sync with hello database.</span></span>

1. <span data-ttu-id="78d16-277">Procházet toohello `Create(Team team)` metoda v hello `TeamsController` třídy.</span><span class="sxs-lookup"><span data-stu-id="78d16-277">Browse toohello `Create(Team team)` method in hello `TeamsController` class.</span></span> <span data-ttu-id="78d16-278">Přidejte volání toohello `ClearCachedTeams` metoda, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-278">Add a call toohello `ClearCachedTeams` method, as shown in hello following example.</span></span>

    ```c#
    // POST: Teams/Create
    // tooprotect from overposting attacks, please enable hello specific properties you want toobind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Teams.Add(team);
            db.SaveChanges();
            // When a team is added, hello cache is out of date.
            // Clear hello cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }

        return View(team);
    }
    ```


1. <span data-ttu-id="78d16-279">Procházet toohello `Edit(Team team)` metoda v hello `TeamsController` třídy.</span><span class="sxs-lookup"><span data-stu-id="78d16-279">Browse toohello `Edit(Team team)` method in hello `TeamsController` class.</span></span> <span data-ttu-id="78d16-280">Přidejte volání toohello `ClearCachedTeams` metoda, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-280">Add a call toohello `ClearCachedTeams` method, as shown in hello following example.</span></span>

    ```c#
    // POST: Teams/Edit/5
    // tooprotect from overposting attacks, please enable hello specific properties you want toobind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Entry(team).State = EntityState.Modified;
            db.SaveChanges();
            // When a team is edited, hello cache is out of date.
            // Clear hello cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }
        return View(team);
    }
    ```


1. <span data-ttu-id="78d16-281">Procházet toohello `DeleteConfirmed(int id)` metoda v hello `TeamsController` třídy.</span><span class="sxs-lookup"><span data-stu-id="78d16-281">Browse toohello `DeleteConfirmed(int id)` method in hello `TeamsController` class.</span></span> <span data-ttu-id="78d16-282">Přidejte volání toohello `ClearCachedTeams` metoda, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-282">Add a call toohello `ClearCachedTeams` method, as shown in hello following example.</span></span>

    ```c#
    // POST: Teams/Delete/5
    [HttpPost, ActionName("Delete")]
    [ValidateAntiForgeryToken]
    public ActionResult DeleteConfirmed(int id)
    {
        Team team = db.Teams.Find(id);
        db.Teams.Remove(team);
        db.SaveChanges();
        // When a team is deleted, hello cache is out of date.
        // Clear hello cached teams.
        ClearCachedTeams();
        return RedirectToAction("Index");
    }
    ```


### <a name="update-hello-teams-index-view-toowork-with-hello-cache"></a><span data-ttu-id="78d16-283">Aktualizovat toowork zobrazení Teams Index hello s mezipamětí hello</span><span class="sxs-lookup"><span data-stu-id="78d16-283">Update hello Teams Index view toowork with hello cache</span></span>
1. <span data-ttu-id="78d16-284">V **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku a potom hello **týmy** složku a dvojím kliknutím **Index.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="78d16-284">In **Solution Explorer**, expand hello **Views** folder, then hello **Teams** folder, and double-click **Index.cshtml**.</span></span>
   
    ![Soubor Index.cshtml][cache-views-teams-index-cshtml]
2. <span data-ttu-id="78d16-286">V horní hello hello souboru vyhledejte následující element odstavce hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-286">Near hello top of hello file, look for hello following paragraph element.</span></span>
   
    ![Tabulka akcí][cache-teams-index-table]
   
    <span data-ttu-id="78d16-288">Toto je odkaz toocreate hello nový tým.</span><span class="sxs-lookup"><span data-stu-id="78d16-288">This is hello link toocreate a new team.</span></span> <span data-ttu-id="78d16-289">Nahraďte element odstavce hello hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="78d16-289">Replace hello paragraph element with hello following table.</span></span> <span data-ttu-id="78d16-290">Tato tabulka obsahuje odkazy na akce pro vytvoření nového týmu, hraní nové sezóny her, vymazání mezipaměti hello, načítání hello týmů z mezipaměti hello v různých formátech, načtení týmů hello z hello databáze a znovu sestavit hello databáze s novými vzorovými daty.</span><span class="sxs-lookup"><span data-stu-id="78d16-290">This table has action links for creating a new team, playing a new season of games, clearing hello cache, retrieving hello teams from hello cache in several formats, retrieving hello teams from hello database, and rebuilding hello database with fresh sample data.</span></span>

    ```html
    <table class="table">
        <tr>
            <td>
                @Html.ActionLink("Create New", "Create")
            </td>
            <td>
                @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
            </td>
            <td>
                @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
            </td>
            <td>
                @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
            </td>
            <td>
                @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
            </td>
            <td>
                @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
            </td>
            <td>
                @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
            </td>
            <td>
                @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
            </td>
        </tr>    
    </table>
    ```


1. <span data-ttu-id="78d16-291">Posuv toohello dolní části hello **Index.cshtml** souboru a přidejte následující hello `tr` element tak, aby se hello hello posledním řádkem poslední tabulky v souboru hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-291">Scroll toohello bottom of hello **Index.cshtml** file and add hello following `tr` element so that it is hello last row in hello last table in hello file.</span></span>
   
    ```html
    <tr><td colspan="5">@ViewBag.Msg</td></tr>
    ```
   
    <span data-ttu-id="78d16-292">Tento řádek zobrazuje hodnotu hello `ViewBag.Msg` obsahující zprávu o aktuální operace hello stavu.</span><span class="sxs-lookup"><span data-stu-id="78d16-292">This row displays hello value of `ViewBag.Msg` which contains a status report about hello current operation.</span></span> <span data-ttu-id="78d16-293">Hello `ViewBag.Msg` je nastaven, po kliknutí na tlačítko všechny odkazy na akce hello hello v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="78d16-293">hello `ViewBag.Msg` is set when you click any of hello action links from hello previous step.</span></span>   
   
    ![Zpráva o stavu][cache-status-message]
2. <span data-ttu-id="78d16-295">Stiskněte klávesu **F6** toobuild hello projektu.</span><span class="sxs-lookup"><span data-stu-id="78d16-295">Press **F6** toobuild hello project.</span></span>

## <a name="provision-hello-azure-resources"></a><span data-ttu-id="78d16-296">Zřízení prostředků Azure hello</span><span class="sxs-lookup"><span data-stu-id="78d16-296">Provision hello Azure resources</span></span>
<span data-ttu-id="78d16-297">toohost aplikaci v Azure, musíte nejdříve zřídit služby Azure, které vaše aplikace vyžaduje hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-297">toohost your application in Azure, you must first provision hello Azure services that your application requires.</span></span> <span data-ttu-id="78d16-298">Ukázková aplikace Hello v tomto kurzu používá následující služby Azure hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-298">hello sample application in this tutorial uses hello following Azure services.</span></span>

* <span data-ttu-id="78d16-299">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="78d16-299">Azure Redis Cache</span></span>
* <span data-ttu-id="78d16-300">Webová aplikace App Service</span><span class="sxs-lookup"><span data-stu-id="78d16-300">App Service Web App</span></span>
* <span data-ttu-id="78d16-301">SQL Database</span><span class="sxs-lookup"><span data-stu-id="78d16-301">SQL Database</span></span>

<span data-ttu-id="78d16-302">toodeploy tyto služby tooa nový nebo existující prostředek skupiny podle svého výběru, klikněte na tlačítko hello následující **nasazení tooAzure** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="78d16-302">toodeploy these services tooa new or existing resource group of your choice, click hello following **Deploy tooAzure** button.</span></span>

<span data-ttu-id="78d16-303">[! [Nasazení tooAzure] [deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="78d16-303">[![Deploy tooAzure][deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)</span></span>

<span data-ttu-id="78d16-304">To **nasazení tooAzure** tlačítko používá hello [vytvoření webové aplikace s Redis Cache a SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure Quickstart](https://github.com/Azure/azure-quickstart-templates) šablony tooprovision těchto služeb a sadu hello připojovací řetězec pro hello SQL Database a hello nastavení aplikace pro hello připojovacího řetězce Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="78d16-304">This **Deploy tooAzure** button uses hello [Create a Web App plus Redis Cache plus SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure Quickstart](https://github.com/Azure/azure-quickstart-templates) template tooprovision these services and set hello connection string for hello SQL Database and hello application setting for hello Azure Redis Cache connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="78d16-305">Pokud ještě nemáte účet Azure, můžete si během několika minut [zdarma vytvořit účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).</span><span class="sxs-lookup"><span data-stu-id="78d16-305">If you don't have an Azure account, you can [create a free Azure account](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) in just a couple of minutes.</span></span>
> 
> 

<span data-ttu-id="78d16-306">Kliknutím na hello **nasazení tooAzure** tlačítko přejdete toohello Azure portal a zahájí hello proces vytváření prostředků hello popsaných hello šablony.</span><span class="sxs-lookup"><span data-stu-id="78d16-306">Clicking hello **Deploy tooAzure** button takes you toohello Azure portal and initiates hello process of creating hello resources described by hello template.</span></span>

![Nasazení tooAzure][cache-deploy-to-azure-step-1]

1. <span data-ttu-id="78d16-308">V hello **Základy** části vyberte hello toouse předplatné Azure a vyberte existující skupinu prostředků nebo vytvořte novou a zadejte umístění skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-308">In hello **Basics** section, select hello Azure subscription toouse, and select an existing resource group or create a new one, and specify hello resource group location.</span></span>
2. <span data-ttu-id="78d16-309">V hello **nastavení** , určete **přihlášení správce** (nepoužívejte **správce**), **heslo správce**a  **Název databáze**.</span><span class="sxs-lookup"><span data-stu-id="78d16-309">In hello **Settings** section, specify an **Administrator Login** (don't use **admin**), **Administrator Login Password**, and **Database Name**.</span></span> <span data-ttu-id="78d16-310">Hello další parametry jsou nakonfigurovány pro hostování plán a možnosti nižší náklady hello SQL Database a Azure Redis Cache, které nejsou součástí úrovně free bezplatné služby App Service.</span><span class="sxs-lookup"><span data-stu-id="78d16-310">hello other parameters are configured for a free App Service hosting plan, and lower-cost options for hello SQL Database and Azure Redis Cache, which don't come with a free tier.</span></span>

    ![Nasazení tooAzure][cache-deploy-to-azure-step-2]

3. <span data-ttu-id="78d16-312">Po dokončení konfigurace nastavení hello potřeby, posuňte se konec toohello stránku hello, přečtěte si hello podmínky a ujednání a zkontrolujte hello **souhlasím toohello podmínky a ujednání, které jsou uvedené výše** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="78d16-312">After configuring hello desired settings, scroll toohello end of hello page, read hello terms and conditions, and check hello **I agree toohello terms and conditions stated above** checkbox.</span></span>
4. <span data-ttu-id="78d16-313">Klikněte na tlačítko toobegin zřizování hello prostředky **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="78d16-313">toobegin provisioning hello resources, click **Purchase**.</span></span>

<span data-ttu-id="78d16-314">Klikněte na ikonu oznámení hello tooview hello průběh nasazení a klikněte na tlačítko **nasazení začalo**.</span><span class="sxs-lookup"><span data-stu-id="78d16-314">tooview hello progress of your deployment, click hello notification icon and click **Deployment started**.</span></span>

![Nasazení začalo][cache-deployment-started]

<span data-ttu-id="78d16-316">Hello stav nasazení můžete zobrazit na hello **Microsoft.Template** okno.</span><span class="sxs-lookup"><span data-stu-id="78d16-316">You can view hello status of your deployment on hello **Microsoft.Template** blade.</span></span>

![Nasazení tooAzure][cache-deploy-to-azure-step-3]

<span data-ttu-id="78d16-318">Jakmile je zřizování dokončeno, můžete publikovat tooAzure vaší aplikace z Visual Studia.</span><span class="sxs-lookup"><span data-stu-id="78d16-318">When provisioning is complete, you can publish your application tooAzure from Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="78d16-319">Na hello se zobrazí nějaké chyby, které mohou nastat během procesu zřizování hello **Microsoft.Template** okno.</span><span class="sxs-lookup"><span data-stu-id="78d16-319">Any errors that may occur during hello provisioning process are displayed on hello **Microsoft.Template** blade.</span></span> <span data-ttu-id="78d16-320">Mezi běžné chyby patří příliš mnoho SQL Serverů nebo příliš mnoho plánů hostování služby App Service úrovně Free na předplatné.</span><span class="sxs-lookup"><span data-stu-id="78d16-320">Common errors are too many SQL Servers or too many Free App Service hosting plans per subscription.</span></span> <span data-ttu-id="78d16-321">Vyřešte všechny chyby a restartování procesu hello kliknutím **znovu nasaďte** na hello **Microsoft.Template** okno nebo hello **nasazení tooAzure** tlačítko v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="78d16-321">Resolve any errors and restart hello process by clicking **Redeploy** on hello **Microsoft.Template** blade or hello **Deploy tooAzure** button in this tutorial.</span></span>
> 
> 

## <a name="publish-hello-application-tooazure"></a><span data-ttu-id="78d16-322">Publikování aplikace tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="78d16-322">Publish hello application tooAzure</span></span>
<span data-ttu-id="78d16-323">V tomto kroku kurzu hello budete publikovat tooAzure aplikace hello a spustíte ho v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-323">In this step of hello tutorial, you'll publish hello application tooAzure and run it in hello cloud.</span></span>

1. <span data-ttu-id="78d16-324">Klikněte pravým tlačítkem na hello **ContosoTeamStats** projektu v sadě Visual Studio a zvolte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="78d16-324">Right-click hello **ContosoTeamStats** project in Visual Studio and choose **Publish**.</span></span>
   
    ![Publikování][cache-publish-app]
2. <span data-ttu-id="78d16-326">Klikněte na **Microsoft Azure App Service**, zvolte **Vybrat existující** a klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="78d16-326">Click **Microsoft Azure App Service**, choose **Select Existing**, and click **Publish**.</span></span>
   
    ![Publikování][cache-publish-to-app-service]
3. <span data-ttu-id="78d16-328">Vyberte předplatné hello použité při vytváření hello prostředky Azure, rozbalte skupinu prostředků hello obsahující hello prostředky, a vyberte hello potřeby webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="78d16-328">Select hello subscription used when creating hello Azure resources, expand hello resource group containing hello resources, and select hello desired Web App.</span></span> <span data-ttu-id="78d16-329">Pokud jste použili hello **nasazení tooAzure** tlačítko název vaší webové aplikace spustí s **webu** následuje některé další znaky.</span><span class="sxs-lookup"><span data-stu-id="78d16-329">If you used hello **Deploy tooAzure** button your Web App name starts with **webSite** followed by some additional characters.</span></span>
   
    ![Výběr webové aplikace][cache-select-web-app]
4. <span data-ttu-id="78d16-331">Klikněte na tlačítko **OK** toobegin hello procesu publikování.</span><span class="sxs-lookup"><span data-stu-id="78d16-331">Click **OK** toobegin hello publishing process.</span></span> <span data-ttu-id="78d16-332">Po chvíli hello proces publikování dokončí a spuštění prohlížeče s hello spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="78d16-332">After a few moments hello publishing process completes and a browser is launched with hello running sample application.</span></span> <span data-ttu-id="78d16-333">Pokud dojde chybě DNS při ověřování nebo publikování a hello zřizování pro hello prostředků Azure pro aplikace hello má právě k dokončení, počkejte a akci opakujte.</span><span class="sxs-lookup"><span data-stu-id="78d16-333">If you get a DNS error when validating or publishing, and hello provisioning process for hello Azure resources for hello application has just recently completed, wait a moment and try again.</span></span>
   
    ![Mezipaměť byla přidána][cache-added-to-application]

<span data-ttu-id="78d16-335">Hello následující tabulka popisuje každý odkaz na akci z ukázkové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-335">hello following table describes each action link from hello sample application.</span></span>

| <span data-ttu-id="78d16-336">Akce</span><span class="sxs-lookup"><span data-stu-id="78d16-336">Action</span></span> | <span data-ttu-id="78d16-337">Popis</span><span class="sxs-lookup"><span data-stu-id="78d16-337">Description</span></span> |
| --- | --- |
| <span data-ttu-id="78d16-338">Vytvořit nový</span><span class="sxs-lookup"><span data-stu-id="78d16-338">Create New</span></span> |<span data-ttu-id="78d16-339">Vytvoření nového týmu.</span><span class="sxs-lookup"><span data-stu-id="78d16-339">Create a new Team.</span></span> |
| <span data-ttu-id="78d16-340">Odehrát sezónu</span><span class="sxs-lookup"><span data-stu-id="78d16-340">Play Season</span></span> |<span data-ttu-id="78d16-341">Odehrání sezóny her, aktualizace hello týmových statistik a zrušte všechny zastaralé týmovými daty z mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-341">Play a season of games, update hello team stats, and clear any outdated team data from hello cache.</span></span> |
| <span data-ttu-id="78d16-342">Vymazat mezipaměť</span><span class="sxs-lookup"><span data-stu-id="78d16-342">Clear Cache</span></span> |<span data-ttu-id="78d16-343">Vymazat hello týmových statistik z mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-343">Clear hello team stats from hello cache.</span></span> |
| <span data-ttu-id="78d16-344">Seznam z mezipaměti</span><span class="sxs-lookup"><span data-stu-id="78d16-344">List from Cache</span></span> |<span data-ttu-id="78d16-345">Načíst hello týmových statistik z mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-345">Retrieve hello team stats from hello cache.</span></span> <span data-ttu-id="78d16-346">Pokud dojde k neúspěšnému přístupu do mezipaměti, načtou hello z hello databáze a uloží toohello mezipaměti pro další použití.</span><span class="sxs-lookup"><span data-stu-id="78d16-346">If there is a cache miss, load hello stats from hello database and save toohello cache for next time.</span></span> |
| <span data-ttu-id="78d16-347">Seřazená sada z mezipaměti</span><span class="sxs-lookup"><span data-stu-id="78d16-347">Sorted Set from Cache</span></span> |<span data-ttu-id="78d16-348">Načtení týmových statistik hello z hello mezipaměti jako seřazená sada.</span><span class="sxs-lookup"><span data-stu-id="78d16-348">Retrieve hello team stats from hello cache using a sorted set.</span></span> <span data-ttu-id="78d16-349">Pokud dojde k neúspěšnému přístupu do mezipaměti, načtou hello z hello databáze a uloží toohello mezipaměti jako seřazená sada.</span><span class="sxs-lookup"><span data-stu-id="78d16-349">If there is a cache miss, load hello stats from hello database and save toohello cache using a sorted set.</span></span> |
| <span data-ttu-id="78d16-350">Top 5 týmů z mezipaměti</span><span class="sxs-lookup"><span data-stu-id="78d16-350">Top 5 Teams from Cache</span></span> |<span data-ttu-id="78d16-351">Načtení top 5 týmů hello z hello mezipaměti jako seřazená sada.</span><span class="sxs-lookup"><span data-stu-id="78d16-351">Retrieve hello top 5 teams from hello cache using a sorted set.</span></span> <span data-ttu-id="78d16-352">Pokud dojde k neúspěšnému přístupu do mezipaměti, načtou hello z hello databáze a uloží toohello mezipaměti jako seřazená sada.</span><span class="sxs-lookup"><span data-stu-id="78d16-352">If there is a cache miss, load hello stats from hello database and save toohello cache using a sorted set.</span></span> |
| <span data-ttu-id="78d16-353">Načíst z databáze</span><span class="sxs-lookup"><span data-stu-id="78d16-353">Load from DB</span></span> |<span data-ttu-id="78d16-354">Hello týmových statistik z databáze získat z hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-354">Retrieve hello team stats from hello database.</span></span> |
| <span data-ttu-id="78d16-355">Znovu sestavit databázi</span><span class="sxs-lookup"><span data-stu-id="78d16-355">Rebuild DB</span></span> |<span data-ttu-id="78d16-356">Znovu sestavte hello databáze a se vzorovými týmovými daty.</span><span class="sxs-lookup"><span data-stu-id="78d16-356">Rebuild hello database and reload it with sample team data.</span></span> |
| <span data-ttu-id="78d16-357">Upravit / Podrobnosti / Odstranit</span><span class="sxs-lookup"><span data-stu-id="78d16-357">Edit / Details / Delete</span></span> |<span data-ttu-id="78d16-358">Upravení týmu, zobrazení podrobností o týmu, odstranění týmu.</span><span class="sxs-lookup"><span data-stu-id="78d16-358">Edit a team, view details for a team, delete a team.</span></span> |

<span data-ttu-id="78d16-359">Klikněte na některé hello akce a Experimentujte s načítáním dat hello z různých zdrojů hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-359">Click some of hello actions and experiment with retrieving hello data from hello different sources.</span></span> <span data-ttu-id="78d16-360">Není hello rozdíly v čase hello trvá toocomplete hello různých způsobů načítání hello data z databáze hello a hello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="78d16-360">Not hello differences in hello time it takes toocomplete hello various ways of retrieving hello data from hello database and hello cache.</span></span>

## <a name="delete-hello-resources-when-you-are-finished-with-hello-application"></a><span data-ttu-id="78d16-361">Odstranit hello prostředky, když jste hotovi s aplikací hello</span><span class="sxs-lookup"><span data-stu-id="78d16-361">Delete hello resources when you are finished with hello application</span></span>
<span data-ttu-id="78d16-362">Po skončení hello ukázkové aplikace, můžete odstranit hello Azure používá v pořadí tooconserve náklady a prostředků.</span><span class="sxs-lookup"><span data-stu-id="78d16-362">When you are finished with hello sample tutorial application, you can delete hello Azure resources used in order tooconserve cost and resources.</span></span> <span data-ttu-id="78d16-363">Pokud používáte hello **nasazení tooAzure** tlačítka na hello [hello zřízení prostředků Azure](#provision-the-azure-resources) oddíl a všechny vaše prostředky jsou obsaženy v hello stejnou skupinu prostředků, můžete je odstranit společně v jednom operace odstraněním skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-363">If you use hello **Deploy tooAzure** button in hello [Provision hello Azure resources](#provision-the-azure-resources) section and all of your resources are contained in hello same resource group, you can delete them together in one operation by deleting hello resource group.</span></span>

1. <span data-ttu-id="78d16-364">Přihlaste se toohello [portál Azure](https://portal.azure.com) a klikněte na tlačítko **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="78d16-364">Sign in toohello [Azure portal](https://portal.azure.com) and click **Resource groups**.</span></span>
2. <span data-ttu-id="78d16-365">Název typu hello vaší skupiny prostředků do hello **filtrování položek...**  textové pole.</span><span class="sxs-lookup"><span data-stu-id="78d16-365">Type hello name of your resource group into hello **Filter items...** textbox.</span></span>
3. <span data-ttu-id="78d16-366">Klikněte na tlačítko **...**  toohello napravo od vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="78d16-366">Click **...** toohello right of your resource group.</span></span>
4. <span data-ttu-id="78d16-367">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="78d16-367">Click **Delete**.</span></span>
   
    ![Odstranění][cache-delete-resource-group]
5. <span data-ttu-id="78d16-369">Název typu hello vaší skupiny prostředků a klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="78d16-369">Type hello name of your resource group and click **Delete**.</span></span>
   
    ![Potvrzení odstranění][cache-delete-confirm]

<span data-ttu-id="78d16-371">Za pár chvil hello prostředků skupiny obsažených prostředků odstraněna.</span><span class="sxs-lookup"><span data-stu-id="78d16-371">After a few moments hello resource group and all of its contained resources are deleted.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="78d16-372">Všimněte si, že odstranění skupiny prostředků je nevratné a příslušné skupině prostředků hello a všechny hello prostředky v ní jsou trvale odstraněny.</span><span class="sxs-lookup"><span data-stu-id="78d16-372">Note that deleting a resource group is irreversible and that hello resource group and all hello resources in it are permanently deleted.</span></span> <span data-ttu-id="78d16-373">Ujistěte se, že neodstraníte omylem hello nesprávnou skupinu prostředků nebo prostředky.</span><span class="sxs-lookup"><span data-stu-id="78d16-373">Make sure that you do not accidentally delete hello wrong resource group or resources.</span></span> <span data-ttu-id="78d16-374">Pokud jste vytvořili hello prostředky pro hostování této ukázky ve stávající skupinu prostředků, můžete odstranit jednotlivé prostředky jednotlivě z jejich odpovídajících podoken.</span><span class="sxs-lookup"><span data-stu-id="78d16-374">If you created hello resources for hosting this sample inside an existing resource group, you can delete each resource individually from their respective blades.</span></span>
> 
> 

## <a name="run-hello-sample-application-on-your-local-machine"></a><span data-ttu-id="78d16-375">Spuštění ukázkové aplikace hello na místním počítači</span><span class="sxs-lookup"><span data-stu-id="78d16-375">Run hello sample application on your local machine</span></span>
<span data-ttu-id="78d16-376">aplikace hello toorun místně na počítači, potřebujete Azure Redis Cache instance v které toocache vaše data.</span><span class="sxs-lookup"><span data-stu-id="78d16-376">toorun hello application locally on your machine, you need an Azure Redis Cache instance in which toocache your data.</span></span> 

* <span data-ttu-id="78d16-377">Pokud vaše aplikace tooAzure jste publikovali, jak je popsáno v předchozí části hello, můžete instanci Azure Redis Cache hello, zřízenou během tohoto kroku.</span><span class="sxs-lookup"><span data-stu-id="78d16-377">If you have published your application tooAzure as described in hello previous section, you can use hello Azure Redis Cache instance that was provisioned during that step.</span></span>
* <span data-ttu-id="78d16-378">Pokud máte jinou stávající instanci Azure Redis Cache, můžete tuto toorun této ukázky místně.</span><span class="sxs-lookup"><span data-stu-id="78d16-378">If you have another existing Azure Redis Cache instance, you can use that toorun this sample locally.</span></span>
* <span data-ttu-id="78d16-379">Pokud potřebujete toocreate instanci služby Azure Redis Cache, můžete provést kroky hello v [vytvoření mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span><span class="sxs-lookup"><span data-stu-id="78d16-379">If you need toocreate an Azure Redis Cache instance, you can follow hello steps in [Create a cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span></span>

<span data-ttu-id="78d16-380">Po vybrání nebo vytvoření mezipaměti toouse hello, procházet toohello mezipaměti hello portál Azure a načíst hello [název hostitele](cache-configure.md#properties) a [přístupové klíče](cache-configure.md#access-keys) ke svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="78d16-380">Once you have selected or created hello cache toouse, browse toohello cache in hello Azure portal and retrieve hello [host name](cache-configure.md#properties) and [access keys](cache-configure.md#access-keys) for your cache.</span></span> <span data-ttu-id="78d16-381">Pokyny najdete v oddílu [Konfigurace nastavení mezipaměti Redis](cache-configure.md#configure-redis-cache-settings).</span><span class="sxs-lookup"><span data-stu-id="78d16-381">For instructions, see [Configure Redis cache settings](cache-configure.md#configure-redis-cache-settings).</span></span>

1. <span data-ttu-id="78d16-382">Otevřete hello `WebAppPlusCacheAppSecrets.config` soubor, který jste vytvořili během hello [konfigurace toouse aplikace hello Redis Cache](#configure-the-application-to-use-redis-cache) krok v tomto kurzu pomocí zvoleného editoru hello.</span><span class="sxs-lookup"><span data-stu-id="78d16-382">Open hello `WebAppPlusCacheAppSecrets.config` file that you created during hello [Configure hello application toouse Redis Cache](#configure-the-application-to-use-redis-cache) step of this tutorial using hello editor of your choice.</span></span>
2. <span data-ttu-id="78d16-383">Upravit hello `value` atribut a nahraďte `MyCache.redis.cache.windows.net` s hello [název hostitele](cache-configure.md#properties) vaší mezipaměti a zadejte buď hello [primární nebo sekundární klíč](cache-configure.md#access-keys) svojí mezipaměti jako hello heslo.</span><span class="sxs-lookup"><span data-stu-id="78d16-383">Edit hello `value` attribute and replace `MyCache.redis.cache.windows.net` with hello [host name](cache-configure.md#properties) of your cache, and specify either hello [primary or secondary key](cache-configure.md#access-keys) of your cache as hello password.</span></span>

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. <span data-ttu-id="78d16-384">Stiskněte klávesu **Ctrl + F5** toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="78d16-384">Press **Ctrl+F5** toorun hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="78d16-385">Všimněte si, že protože hello aplikaci, včetně hello databáze, je spuštěn místně a hello Redis Cache je hostovaná v Azure, hello mezipaměti může se zobrazit toounder-provést hello databáze.</span><span class="sxs-lookup"><span data-stu-id="78d16-385">Note that because hello application, including hello database, is running locally and hello Redis Cache is hosted in Azure, hello cache may appear toounder-perform hello database.</span></span> <span data-ttu-id="78d16-386">Pro zajištění nejlepšího výkonu hello klientská aplikace a instance služby Azure Redis Cache by měla být v hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="78d16-386">For best performance, hello client application and Azure Redis Cache instance should be in hello same location.</span></span> 
> 
> 

## <a name="next-steps"></a><span data-ttu-id="78d16-387">Další kroky</span><span class="sxs-lookup"><span data-stu-id="78d16-387">Next steps</span></span>
* <span data-ttu-id="78d16-388">Další informace o [Začínáme s ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) na hello [ASP.NET](http://asp.net/) lokality.</span><span class="sxs-lookup"><span data-stu-id="78d16-388">Learn more about [Getting Started with ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) on hello [ASP.NET](http://asp.net/) site.</span></span>
* <span data-ttu-id="78d16-389">Další příklady vytváření webové aplikace ASP.NET ve službě App Service naleznete v tématu [vytvořit a nasadit webové aplikace ASP.NET ve službě Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) z hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [ukázku](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span><span class="sxs-lookup"><span data-stu-id="78d16-389">For more examples of creating an ASP.NET Web App in App Service, see [Create and deploy an ASP.NET web app in Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) from hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span></span>
  * <span data-ttu-id="78d16-390">Další quickstarts z hello HealthClinic.biz ukázku, najdete v části [Quickstarts nástroje pro vývojáře Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span><span class="sxs-lookup"><span data-stu-id="78d16-390">For more quickstarts from hello HealthClinic.biz demo, see [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span></span>
* <span data-ttu-id="78d16-391">Další informace o hello [kód první tooa novou databázi](https://msdn.microsoft.com/data/jj193542) přístupu tooEntity Framework, který se používá v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="78d16-391">Learn more about hello [Code first tooa new database](https://msdn.microsoft.com/data/jj193542) approach tooEntity Framework that's used in this tutorial.</span></span>
* <span data-ttu-id="78d16-392">Zjistěte více o [webových aplikacích v Azure App Service](../app-service-web/app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="78d16-392">Learn more about [web apps in Azure App Service](../app-service-web/app-service-web-overview.md).</span></span>
* <span data-ttu-id="78d16-393">Zjistěte, jak příliš[monitorování](cache-how-to-monitor.md) svoji mezipaměť hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="78d16-393">Learn how too[monitor](cache-how-to-monitor.md) your cache in hello Azure portal.</span></span>
* <span data-ttu-id="78d16-394">Prozkoumejte prémiové funkce Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="78d16-394">Explore Azure Redis Cache premium features</span></span>
  
  * [<span data-ttu-id="78d16-395">Jak tooconfigure trvalosti pro mezipaměť Azure Redis Cache Premium</span><span class="sxs-lookup"><span data-stu-id="78d16-395">How tooconfigure persistence for a Premium Azure Redis Cache</span></span>](cache-how-to-premium-persistence.md)
  * [<span data-ttu-id="78d16-396">Jak tooconfigure clusteringu pro mezipaměť Azure Redis Cache Premium</span><span class="sxs-lookup"><span data-stu-id="78d16-396">How tooconfigure clustering for a Premium Azure Redis Cache</span></span>](cache-how-to-premium-clustering.md)
  * [<span data-ttu-id="78d16-397">Jak podporuje tooconfigure virtuální sítě pro mezipaměť Azure Redis Cache Premium</span><span class="sxs-lookup"><span data-stu-id="78d16-397">How tooconfigure Virtual Network support for a Premium Azure Redis Cache</span></span>](cache-how-to-premium-vnet.md)
  * <span data-ttu-id="78d16-398">V tématu hello [Azure Redis Cache – nejčastější dotazy](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) další podrobnosti o velikosti, propustnosti a šířky pásma u prémiových mezipamětí.</span><span class="sxs-lookup"><span data-stu-id="78d16-398">See hello [Azure Redis Cache FAQ](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) for more details about size, throughput, and bandwidth with premium caches.</span></span>

<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png

