---
title: "Nasazení aplikace .NET v kontejneru do Azure Service Fabric | Microsoft Docs"
description: "Se naučíte, jak zabalit aplikace .NET v sadě Visual Studio v kontejner Docker. Tuto novou aplikaci \"kontejner\" se pak nasadí do clusteru Service Fabric."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: mikhegn
ms.openlocfilehash: 484db494e7975df950543d19bf841a4df7cdd139
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-a-net-application-in-a-windows-container-to-azure-service-fabric"></a><span data-ttu-id="37ca6-104">Nasazení aplikace .NET v kontejneru systému Windows do Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="37ca6-104">Deploy a .NET application in a Windows container to Azure Service Fabric</span></span>

<span data-ttu-id="37ca6-105">V tomto kurzu se dozvíte, jak nasadit stávající aplikaci ASP.NET v kontejneru systému Windows v Azure.</span><span class="sxs-lookup"><span data-stu-id="37ca6-105">This tutorial shows you how to deploy an existing ASP.NET application in a Windows container on Azure.</span></span>

<span data-ttu-id="37ca6-106">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="37ca6-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="37ca6-107">Vytvoření projektu Docker v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37ca6-107">Create a Docker project in Visual Studio</span></span>
> * <span data-ttu-id="37ca6-108">Containerize stávající aplikaci</span><span class="sxs-lookup"><span data-stu-id="37ca6-108">Containerize an existing application</span></span>
> * <span data-ttu-id="37ca6-109">Instalační program průběžnou integraci s Visual Studio a služby VSTS</span><span class="sxs-lookup"><span data-stu-id="37ca6-109">Setup continuous integration with Visual Studio and VSTS</span></span>

## <a name="prerequisites"></a><span data-ttu-id="37ca6-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="37ca6-110">Prerequisites</span></span>

1. <span data-ttu-id="37ca6-111">Nainstalujte [CE Docker pro systém Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) tak, aby kontejnery můžete spustit na Windows 10.</span><span class="sxs-lookup"><span data-stu-id="37ca6-111">Install [Docker CE for Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) so that you can run containers on Windows 10.</span></span>
2. <span data-ttu-id="37ca6-112">Seznamte se s [rychlý start pro Windows 10 kontejnery][link-container-quickstart].</span><span class="sxs-lookup"><span data-stu-id="37ca6-112">Familiarize yourself with the [Windows 10 Containers quickstart][link-container-quickstart].</span></span>
3. <span data-ttu-id="37ca6-113">Stažení [Fabrikam Fiber CallCenter] [ link-fabrikam-github] ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="37ca6-113">Download the [Fabrikam Fiber CallCenter][link-fabrikam-github] sample application.</span></span>
4. <span data-ttu-id="37ca6-114">Nainstalujte [prostředí Azure PowerShell][link-azure-powershell-install]</span><span class="sxs-lookup"><span data-stu-id="37ca6-114">Install [Azure PowerShell][link-azure-powershell-install]</span></span>
5. <span data-ttu-id="37ca6-115">Nainstalujte [rozšíření průběžné doručování nástrojů pro Visual Studio 2017][link-visualstudio-cd-extension]</span><span class="sxs-lookup"><span data-stu-id="37ca6-115">Install the [Continuous Delivery Tools extension for Visual Studio 2017][link-visualstudio-cd-extension]</span></span>
6. <span data-ttu-id="37ca6-116">Vytvoření [předplatného Azure] [ link-azure-subscription] a [účet Visual Studio Team Services][link-vsts-account].</span><span class="sxs-lookup"><span data-stu-id="37ca6-116">Create an [Azure subscription][link-azure-subscription] and a [Visual Studio Team Services account][link-vsts-account].</span></span> 
7. [<span data-ttu-id="37ca6-117">Vytvoření clusteru s podporou v Azure</span><span class="sxs-lookup"><span data-stu-id="37ca6-117">Create a cluster on Azure</span></span>](service-fabric-tutorial-create-cluster-azure-ps.md)

## <a name="containerize-the-application"></a><span data-ttu-id="37ca6-118">Containerize aplikace</span><span class="sxs-lookup"><span data-stu-id="37ca6-118">Containerize the application</span></span>

<span data-ttu-id="37ca6-119">Teď, když máte [cluster Service Fabric běží v Azure](service-fabric-tutorial-create-cluster-azure-ps.md) budete chtít vytvořit a nasadit kontejnerizované aplikaci.</span><span class="sxs-lookup"><span data-stu-id="37ca6-119">Now that you have a [Service Fabric cluster is running in Azure](service-fabric-tutorial-create-cluster-azure-ps.md) you are ready to create and deploy a containerized application.</span></span> <span data-ttu-id="37ca6-120">Spustit aplikaci v kontejneru, je potřeba přidat **Docker podporu** na projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="37ca6-120">To start running our application in a container, we need to add **Docker Support** to the project in Visual Studio.</span></span> <span data-ttu-id="37ca6-121">Když přidáte **Docker podporu** do aplikace, dvě věci dojít.</span><span class="sxs-lookup"><span data-stu-id="37ca6-121">When you add **Docker support** to the application, two things happen.</span></span> <span data-ttu-id="37ca6-122">První, _soubor Docker_ se přidá do projektu.</span><span class="sxs-lookup"><span data-stu-id="37ca6-122">First, a _Dockerfile_ is added to the project.</span></span> <span data-ttu-id="37ca6-123">Tento nový soubor popisuje, jak bitovou kopii kontejneru má být sestaven.</span><span class="sxs-lookup"><span data-stu-id="37ca6-123">This new file describes how the container image is to be built.</span></span> <span data-ttu-id="37ca6-124">Potom druhé, nový _docker compose_ projekt je přidán do řešení.</span><span class="sxs-lookup"><span data-stu-id="37ca6-124">Then second, a new _docker-compose_ project is added to the solution.</span></span> <span data-ttu-id="37ca6-125">Nový projekt obsahuje několik docker compose soubory.</span><span class="sxs-lookup"><span data-stu-id="37ca6-125">The new project contains a few docker-compose files.</span></span> <span data-ttu-id="37ca6-126">Docker compose soubory můžete použít k popisu, jak spouštět kontejneru.</span><span class="sxs-lookup"><span data-stu-id="37ca6-126">Docker-compose files can be used to describe how the container is run.</span></span>

<span data-ttu-id="37ca6-127">Další informace o práci s [kontejner nástroje sady Visual Studio][link-visualstudio-container-tools].</span><span class="sxs-lookup"><span data-stu-id="37ca6-127">More info on working with [Visual Studio Container Tools][link-visualstudio-container-tools].</span></span>

>[!NOTE]
><span data-ttu-id="37ca6-128">Pokud je prvním používáte bitových kopií kontejneru systému Windows v počítači, musí Docker CE stahují základní Image pro kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="37ca6-128">If it is the first time you are running Windows container images on your computer, Docker CE must pull down the base images for your containers.</span></span> <span data-ttu-id="37ca6-129">Obrázků použitých v tomto kurzu jsou 14 GB.</span><span class="sxs-lookup"><span data-stu-id="37ca6-129">The images used in this tutorial are 14 GB.</span></span> <span data-ttu-id="37ca6-130">Pokračujte a spusťte následující příkaz terminálu vyžádat základní bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="37ca6-130">Go ahead and run the following terminal command to pull the base images:</span></span>
>```cmd
>docker pull microsoft/mssql-server-windows-developer
>docker pull microsoft/aspnet:4.6.2
>```

### <a name="add-docker-support"></a><span data-ttu-id="37ca6-131">Přidání podpory Docker</span><span class="sxs-lookup"><span data-stu-id="37ca6-131">Add Docker support</span></span>

<span data-ttu-id="37ca6-132">Otevřete [FabrikamFiber.CallCenter.sln] [ link-fabrikam-github] souborů v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="37ca6-132">Open the [FabrikamFiber.CallCenter.sln][link-fabrikam-github] file in Visual Studio.</span></span>

<span data-ttu-id="37ca6-133">Klikněte pravým tlačítkem myši **FabrikamFiber.Web** Projekt > **přidat** > **Docker podporu**.</span><span class="sxs-lookup"><span data-stu-id="37ca6-133">Right-click the **FabrikamFiber.Web** project > **Add** > **Docker Support**.</span></span>

### <a name="add-support-for-sql"></a><span data-ttu-id="37ca6-134">Přidání podpory pro SQL</span><span class="sxs-lookup"><span data-stu-id="37ca6-134">Add support for SQL</span></span>

<span data-ttu-id="37ca6-135">Tato aplikace používá SQL jako poskytovatel dat, SQL server je vyžadován ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="37ca6-135">This application uses SQL as the data provider, so a SQL server is required to run the application.</span></span> <span data-ttu-id="37ca6-136">Referenční bitovou kopii systému SQL Server kontejneru v našem docker compose.override.yml souboru.</span><span class="sxs-lookup"><span data-stu-id="37ca6-136">Reference a SQL Server container image in our docker-compose.override.yml file.</span></span>

<span data-ttu-id="37ca6-137">V sadě Visual Studio otevřete **Průzkumníku řešení**, Najít **docker compose**a otevřete soubor **docker compose.override.yml**.</span><span class="sxs-lookup"><span data-stu-id="37ca6-137">In Visual Studio, open **Solution Explorer**, find **docker-compose**, and open the file **docker-compose.override.yml**.</span></span>

<span data-ttu-id="37ca6-138">Přejděte na `services:` uzlu přidat uzel s názvem `db:` položce SQL Server pro kontejner, který definuje.</span><span class="sxs-lookup"><span data-stu-id="37ca6-138">Navigate to the `services:` node, add a node named `db:` that defines the SQL Server entry for the container.</span></span>

```yml
  db:
    image: microsoft/mssql-server-windows-developer
    environment:
      sa_password: "Password1"
      ACCEPT_EULA: "Y"
    ports:
      - "1433"
    healthcheck:
      test: [ "CMD", "sqlcmd", "-U", "sa", "-P", "Password1", "-Q", "select 1" ]
      interval: 1s
      retries: 20
```

>[!NOTE]
><span data-ttu-id="37ca6-139">Jakýkoli Server SQL dáváte přednost pro místní ladění, můžete použít, dokud je dostupný z vašeho hostitele.</span><span class="sxs-lookup"><span data-stu-id="37ca6-139">You can use any SQL Server you prefer for local debugging, as long as it is reachable from your host.</span></span> <span data-ttu-id="37ca6-140">Ale **localdb** nepodporuje `container -> host` komunikace.</span><span class="sxs-lookup"><span data-stu-id="37ca6-140">However, **localdb** does not support `container -> host` communication.</span></span>

>[!WARNING]
><span data-ttu-id="37ca6-141">Zachování dat není podporován systémem SQL Server v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="37ca6-141">Running SQL Server in a container does not support persisting data.</span></span> <span data-ttu-id="37ca6-142">Když se zastaví kontejner, data budou vymazána.</span><span class="sxs-lookup"><span data-stu-id="37ca6-142">When the container stops, your data is erased.</span></span> <span data-ttu-id="37ca6-143">Nepoužívejte tuto konfiguraci pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="37ca6-143">Do not use this configuration for production.</span></span>

<span data-ttu-id="37ca6-144">Přejděte na `fabrikamfiber.web:` uzel a přidat podřízený uzel s názvem `depends_on:`.</span><span class="sxs-lookup"><span data-stu-id="37ca6-144">Navigate to the `fabrikamfiber.web:` node and add a child node named `depends_on:`.</span></span> <span data-ttu-id="37ca6-145">To zajistí, že `db` (kontejneru systému SQL Server) bude spuštěn před naše webové aplikace (fabrikamfiber.web).</span><span class="sxs-lookup"><span data-stu-id="37ca6-145">This ensures that the `db` service (the SQL Server container) starts before our web application (fabrikamfiber.web).</span></span>

```yml
  fabrikamfiber.web:
    depends_on:
      - db
```

### <a name="update-the-web-config"></a><span data-ttu-id="37ca6-146">Aktualizace webové konfigurace</span><span class="sxs-lookup"><span data-stu-id="37ca6-146">Update the web config</span></span>

<span data-ttu-id="37ca6-147">Zpět v **FabrikamFiber.Web** projektu, aktualizovat připojovací řetězec **web.config** souboru, do bodu k systému SQL Server v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="37ca6-147">Back in the **FabrikamFiber.Web** project, update the connection string in the **web.config** file, to point to the SQL Server in the container.</span></span>

```xml
<add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

<add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
```

>[!NOTE]
><span data-ttu-id="37ca6-148">Pokud chcete použít jinou konzolu systému SQL Server při vytváření verze sestavení webové aplikace, přidejte další připojovací řetězec do souboru web.release.config.</span><span class="sxs-lookup"><span data-stu-id="37ca6-148">If you want to use a different SQL Server when building a release build of your web application, add another connection string to your web.release.config file.</span></span>

### <a name="test-your-container"></a><span data-ttu-id="37ca6-149">Vaše kontejner testů</span><span class="sxs-lookup"><span data-stu-id="37ca6-149">Test your container</span></span>

<span data-ttu-id="37ca6-150">Stiskněte klávesu **F5** ke spuštění a ladění aplikace v vašeho kontejneru.</span><span class="sxs-lookup"><span data-stu-id="37ca6-150">Press **F5** to run and debug the application in your container.</span></span>

<span data-ttu-id="37ca6-151">Hraniční otevře se stránka definované spuštění vaší aplikace pomocí IP adresy kontejneru v interní síti NAT (obvykle 172.x.x.x).</span><span class="sxs-lookup"><span data-stu-id="37ca6-151">Edge opens your application's defined launch page using the IP address of the container on the internal NAT network (typically 172.x.x.x).</span></span> <span data-ttu-id="37ca6-152">Další informace o ladění aplikací v kontejnerech pomocí Visual Studio 2017 najdete v tématu [v tomto článku][link-debug-container].</span><span class="sxs-lookup"><span data-stu-id="37ca6-152">To learn more about debugging applications in containers using Visual Studio 2017, see [this article][link-debug-container].</span></span>

![třeba společnost fabrikam v kontejneru][image-web-preview]

<span data-ttu-id="37ca6-154">Kontejner je nyní připraven k vytvořené a zabalené aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="37ca6-154">The container is now ready to be built and packaged in a Service Fabric application.</span></span> <span data-ttu-id="37ca6-155">Až budete mít bitovou kopii kontejneru založený na váš počítač, můžete poslat ho přímo žádné registru kontejneru a načítat na každém hostiteli spustit.</span><span class="sxs-lookup"><span data-stu-id="37ca6-155">Once you have the container image built on your machine, you can push it to any container registry and pull it down to any host to run.</span></span>

## <a name="get-the-application-ready-for-the-cloud"></a><span data-ttu-id="37ca6-156">Příprava aplikace pro cloud</span><span class="sxs-lookup"><span data-stu-id="37ca6-156">Get the application ready for the cloud</span></span>

<span data-ttu-id="37ca6-157">Příprava aplikace pro spuštění v Service Fabric v Azure, je potřeba provést dva kroky:</span><span class="sxs-lookup"><span data-stu-id="37ca6-157">To get the application ready for running in Service Fabric in Azure, we need to complete two steps:</span></span>

1. <span data-ttu-id="37ca6-158">Vystavení portu, kde Chceme mít možnost připojení naše webové aplikace v clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="37ca6-158">Expose the port where we want to be able to reach our web application in the Service Fabric cluster.</span></span>
2. <span data-ttu-id="37ca6-159">Zadejte databázi SQL připravené pro produkční pro naši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="37ca6-159">Provide a production ready SQL database for our application.</span></span>

### <a name="expose-the-port-for-the-app"></a><span data-ttu-id="37ca6-160">Vystavení port pro aplikace</span><span class="sxs-lookup"><span data-stu-id="37ca6-160">Expose the port for the app</span></span>
<span data-ttu-id="37ca6-161">Cluster Service Fabric jsme nakonfigurovali, má port *80* otevřete ve výchozím nastavení nástroje pro vyrovnávání zatížení Azure, který vyrovnává příchozí provoz do clusteru.</span><span class="sxs-lookup"><span data-stu-id="37ca6-161">The Service Fabric cluster we have configured, has port *80* open by default in the Azure Load Balancer, that balances incoming traffic to the cluster.</span></span> <span data-ttu-id="37ca6-162">Naše kontejneru na port jsme můžou zpřístupnit prostřednictvím našich soubor docker-compose.yml.</span><span class="sxs-lookup"><span data-stu-id="37ca6-162">We can expose our container on this port via our docker-compose.yml file.</span></span>

<span data-ttu-id="37ca6-163">V sadě Visual Studio otevřete **Průzkumníku řešení**, Najít **docker compose**a otevřete soubor **docker compose.override.yml**.</span><span class="sxs-lookup"><span data-stu-id="37ca6-163">In Visual Studio, open **Solution Explorer**, find **docker-compose**, and open the file **docker-compose.override.yml**.</span></span>

<span data-ttu-id="37ca6-164">Změnit `fabrikamfiber.web:` uzlu přidat podřízený uzel s názvem `ports:`.</span><span class="sxs-lookup"><span data-stu-id="37ca6-164">Modify the `fabrikamfiber.web:` node, add a child node named `ports:`.</span></span>

<span data-ttu-id="37ca6-165">Přidat položku řetězec `- "80:80"`.</span><span class="sxs-lookup"><span data-stu-id="37ca6-165">Add a string entry `- "80:80"`.</span></span>

```yml
  version: '3'

  services:
    fabrikamfiber.web:
      image: fabrikamfiber.web
      build:
        context: .\FabrikamFiber.Web
        dockerfile: Dockerfile
      ports:
        - "80:80"
```

### <a name="use-a-production-sql-database"></a><span data-ttu-id="37ca6-166">Použít provozní databáze SQL</span><span class="sxs-lookup"><span data-stu-id="37ca6-166">Use a production SQL database</span></span>
<span data-ttu-id="37ca6-167">Při spuštění v produkčním prostředí, potřebujeme naše data v našem databáze.</span><span class="sxs-lookup"><span data-stu-id="37ca6-167">When running in production, we need our data persisted in our database.</span></span> <span data-ttu-id="37ca6-168">Aktuálně neexistuje žádný způsob, jak zajistit trvalou data v kontejneru, proto nelze uložit provozních dat v systému SQL Server v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="37ca6-168">There is currently no way to guarantee persistent data in a container, therefore you cannot store production data in SQL Server in a container.</span></span>

<span data-ttu-id="37ca6-169">Doporučujeme, abyste že využívat Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="37ca6-169">We recommend you utilize an Azure SQL Database.</span></span> <span data-ttu-id="37ca6-170">Chcete-li nastavit a spustit na spravovaný Server SQL v Azure, přejděte [Azure SQL Database – elementy QuickStart] [ link-azure-sql] článku.</span><span class="sxs-lookup"><span data-stu-id="37ca6-170">To set up and run a managed SQL Server in Azure, visit the [Azure SQL Database Quickstarts][link-azure-sql] article.</span></span>

>[!NOTE]
><span data-ttu-id="37ca6-171">Mějte na paměti, chcete-li změnit připojovací řetězce k systému SQL server v **web.release.config** v soubor **FabrikamFiber.Web** projektu.</span><span class="sxs-lookup"><span data-stu-id="37ca6-171">Remember to change the connection strings to the SQL server in the **web.release.config** file in the **FabrikamFiber.Web** project.</span></span>
>
><span data-ttu-id="37ca6-172">Tato aplikace se elegantně selže, pokud žádné databáze SQL je dostupný.</span><span class="sxs-lookup"><span data-stu-id="37ca6-172">This application fails gracefully if no SQL database is reachable.</span></span> <span data-ttu-id="37ca6-173">Můžete pokračovat a nasadit aplikace s žádné systému SQL server.</span><span class="sxs-lookup"><span data-stu-id="37ca6-173">You can choose to go ahead and deploy the application with no SQL server.</span></span>

## <a name="deploy-with-visual-studio-team-services"></a><span data-ttu-id="37ca6-174">Nasazení s Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="37ca6-174">Deploy with Visual Studio Team Services</span></span>

<span data-ttu-id="37ca6-175">Chcete-li nastavit nasazení pomocí sady Visual Studio Team Services, je potřeba nainstalovat [rozšíření průběžné doručování nástrojů pro Visual Studio 2017][link-visualstudio-cd-extension].</span><span class="sxs-lookup"><span data-stu-id="37ca6-175">To set up deployment using Visual Studio Team Services, you need to install the [Continuous Delivery Tools extension for Visual Studio 2017][link-visualstudio-cd-extension].</span></span> <span data-ttu-id="37ca6-176">Toto rozšíření umožňuje snadno nasadit do Azure tak, že nakonfigurujete Visual Studio Team Services a získat aplikace nasazené do clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="37ca6-176">This extension makes it easy to deploy to Azure by configuring Visual Studio Team Services and get your app deployed to your Service Fabric cluster.</span></span>

<span data-ttu-id="37ca6-177">Abyste mohli začít, musí být váš kód hostované ve správě zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="37ca6-177">To get started, your code must be hosted in source control.</span></span> <span data-ttu-id="37ca6-178">Zbývající část tohoto oddílu předpokládá **git** je používán.</span><span class="sxs-lookup"><span data-stu-id="37ca6-178">The rest of this section assumes **git** is being used.</span></span>

### <a name="set-up-a-vsts-repo"></a><span data-ttu-id="37ca6-179">Nastavení služby VSTS úložišti</span><span class="sxs-lookup"><span data-stu-id="37ca6-179">Set up a VSTS repo</span></span>
<span data-ttu-id="37ca6-180">V pravém dolním rohu sady Visual Studio, klikněte na položku **přidat do správy zdrojového kódu** > **Git** (nebo obě tyto možnosti dáváte přednost).</span><span class="sxs-lookup"><span data-stu-id="37ca6-180">At the bottom-right corner of Visual Studio, click **Add to Source Control** > **Git** (or whichever option you prefer).</span></span>

![Klikněte na tlačítko Zdroj ovládacího prvku][image-source-control]

<span data-ttu-id="37ca6-182">V _Team Explorer_ podokně, stiskněte klávesu **publikování úložiště Git**.</span><span class="sxs-lookup"><span data-stu-id="37ca6-182">In the _Team Explorer_ pane, press **Publish Git Repo**.</span></span>

<span data-ttu-id="37ca6-183">Vyberte název služby VSTS úložiště a stiskněte klávesu **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="37ca6-183">Select your VSTS repository name and press **Repository**.</span></span>

![publikování do služby VSTS úložišti][image-publish-repo]

<span data-ttu-id="37ca6-185">Teď, když kód synchronizována s zdrojové úložiště služby VSTS, můžete nakonfigurovat průběžnou integraci a nastavené průběžné doručování.</span><span class="sxs-lookup"><span data-stu-id="37ca6-185">Now that your code is synchronized with a VSTS source repository, you can configure continuous integration and continuous delivery.</span></span>

### <a name="setup-continuous-delivery"></a><span data-ttu-id="37ca6-186">Instalační program nastavené průběžné doručování</span><span class="sxs-lookup"><span data-stu-id="37ca6-186">Setup continuous delivery</span></span>

<span data-ttu-id="37ca6-187">V _Průzkumníku řešení_, klikněte pravým tlačítkem myši **řešení** > **konfigurace nastavené průběžné doručování**.</span><span class="sxs-lookup"><span data-stu-id="37ca6-187">In _Solution Explorer_, right-click the **solution** > **Configure Continuous Delivery**.</span></span>

<span data-ttu-id="37ca6-188">Vyberte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="37ca6-188">Select the Azure Subscription.</span></span>

<span data-ttu-id="37ca6-189">Nastavit **hostitele typ** k **služby Fabric Cluster**.</span><span class="sxs-lookup"><span data-stu-id="37ca6-189">Set **Host Type** to **Service Fabric Cluster**.</span></span>

<span data-ttu-id="37ca6-190">Nastavit **cílového hostitele** na cluster service fabric jste vytvořili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="37ca6-190">Set **Target Host** to the service fabric cluster you created in the previous section.</span></span>

<span data-ttu-id="37ca6-191">Vyberte **kontejneru registru** publikovat kontejner tak, aby.</span><span class="sxs-lookup"><span data-stu-id="37ca6-191">Choose a **Container Registry** to publish your container to.</span></span>

>[!TIP]
><span data-ttu-id="37ca6-192">Použití **upravit** tlačítko vytvořte kontejner registru.</span><span class="sxs-lookup"><span data-stu-id="37ca6-192">Use the **Edit** button to create a container registry.</span></span>

<span data-ttu-id="37ca6-193">Stiskněte **OK**.</span><span class="sxs-lookup"><span data-stu-id="37ca6-193">Press **OK**.</span></span>

![instalace služby fabric průběžnou integraci][image-setup-ci]
   
   <span data-ttu-id="37ca6-195">Po dokončení konfigurace vašeho kontejneru se nasadí do Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="37ca6-195">Once the configuration is completed, your container is deployed to Service Fabric.</span></span> <span data-ttu-id="37ca6-196">Vždy, když jste nabízené aktualizace do úložiště nového sestavení a verze je provedena.</span><span class="sxs-lookup"><span data-stu-id="37ca6-196">Whenever you push updates to the repository a new build and release is executed.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="37ca6-197">Vytváření proveďte bitové kopie kontejneru přibližně 15 minut.</span><span class="sxs-lookup"><span data-stu-id="37ca6-197">Building the container images take approximately 15 minutes.</span></span>
   ><span data-ttu-id="37ca6-198">První nasazení tak, aby cluster Service Fabric způsobí, že základní Image kontejneru jádro systému Windows Server ke stažení.</span><span class="sxs-lookup"><span data-stu-id="37ca6-198">The first deployment to the Service Fabric cluster causes the base Windows Server Core container images to be downloaded.</span></span> <span data-ttu-id="37ca6-199">Stahování bude dokončeno další 5 až 10 minut.</span><span class="sxs-lookup"><span data-stu-id="37ca6-199">The download takes additional 5-10 minutes to complete.</span></span>

<span data-ttu-id="37ca6-200">Přejděte do aplikace společnosti Fabrikam Call Center pomocí adresy url clusteru: například *http://mycluster.westeurope.cloudapp.azure.com*</span><span class="sxs-lookup"><span data-stu-id="37ca6-200">Browse to the Fabrikam Call Center application using the url of your cluster: for example, *http://mycluster.westeurope.cloudapp.azure.com*</span></span>

<span data-ttu-id="37ca6-201">Teď, když máte kontejnerizované a nasadit řešení Fabrikam Call Center, můžete otevřít [portál Azure] [ link-azure-portal] a aplikace běžící v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="37ca6-201">Now that you have containerized and deployed the Fabrikam Call Center solution, you can open the [Azure portal][link-azure-portal] and see the application running in Service Fabric.</span></span> <span data-ttu-id="37ca6-202">Pokud chcete vyzkoušet aplikace, otevřete webový prohlížeč a přejděte na adresu URL clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="37ca6-202">To try the application, open a web browser and go to the URL of your Service Fabric cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="37ca6-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="37ca6-203">Next steps</span></span>

<span data-ttu-id="37ca6-204">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="37ca6-204">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="37ca6-205">Vytvoření projektu Docker v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37ca6-205">Create a Docker project in Visual Studio</span></span>
> * <span data-ttu-id="37ca6-206">Containerize stávající aplikaci</span><span class="sxs-lookup"><span data-stu-id="37ca6-206">Containerize an existing application</span></span>
> * <span data-ttu-id="37ca6-207">Instalační program průběžnou integraci s Visual Studio a služby VSTS</span><span class="sxs-lookup"><span data-stu-id="37ca6-207">Setup continuous integration with Visual Studio and VSTS</span></span>

<!--   NOTE SURE WHAT WE SHOULD DO YET HERE

Advance to the next tutorial to learn how to bind a custom SSL certificate to it.

> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)

## Next steps

- [Container Tooling in Visual Studio][link-visualstudio-container-tools]
- [Get started with containers in Service Fabric][link-servicefabric-containers]
- [Creating Service Fabric applications][link-servicefabric-createapp]
-->

[link-debug-container]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-fabrikam-github]: https://aka.ms/fabrikamcontainer
[link-container-quickstart]: /virtualization/windowscontainers/quick-start/quick-start-windows-10
[link-visualstudio-container-tools]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-azure-powershell-install]: /powershell/azure/install-azurerm-ps
[link-servicefabric-create-secure-clusters]: service-fabric-cluster-creation-via-arm.md
[link-visualstudio-cd-extension]: https://aka.ms/cd4vs
[link-servicefabric-containers]: service-fabric-get-started-containers.md
[link-servicefabric-createapp]: service-fabric-create-your-first-application-in-visual-studio.md
[link-azure-portal]: https://portal.azure.com
[link-sf-clustertemplate]: https://aka.ms/securepreviewonelineclustertemplate
[link-azure-pricing-calculator]: https://azure.microsoft.com/en-us/pricing/calculator/
[link-azure-subscription]: https://azure.microsoft.com/en-us/free/
[link-vsts-account]: https://www.visualstudio.com/team-services/pricing/
[link-azure-sql]: /azure/sql-database/

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png
