---
title: aaaDeploy aplikace .NET v kontejneru tooAzure Service Fabric | Microsoft Docs
description: "Se naučíte, jak toopackage aplikace .NET v sadě Visual Studio v kontejner Docker. Tuto novou aplikaci \"kontejner\" se pak nasadí cluster Service Fabric tooa."
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
ms.openlocfilehash: 094b0e71d76b2e56ffb9b23638dd8154b3aff5fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-net-application-in-a-windows-container-tooazure-service-fabric"></a><span data-ttu-id="9c324-104">Nasazení aplikace .NET v kontejneru tooAzure Windows Service Fabric</span><span class="sxs-lookup"><span data-stu-id="9c324-104">Deploy a .NET application in a Windows container tooAzure Service Fabric</span></span>

<span data-ttu-id="9c324-105">Tento kurz ukazuje, jak toodeploy stávající aplikaci ASP.NET v kontejneru systému Windows v Azure.</span><span class="sxs-lookup"><span data-stu-id="9c324-105">This tutorial shows you how toodeploy an existing ASP.NET application in a Windows container on Azure.</span></span>

<span data-ttu-id="9c324-106">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="9c324-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9c324-107">Vytvoření projektu Docker v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c324-107">Create a Docker project in Visual Studio</span></span>
> * <span data-ttu-id="9c324-108">Containerize stávající aplikaci</span><span class="sxs-lookup"><span data-stu-id="9c324-108">Containerize an existing application</span></span>
> * <span data-ttu-id="9c324-109">Instalační program průběžnou integraci s Visual Studio a služby VSTS</span><span class="sxs-lookup"><span data-stu-id="9c324-109">Setup continuous integration with Visual Studio and VSTS</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c324-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9c324-110">Prerequisites</span></span>

1. <span data-ttu-id="9c324-111">Nainstalujte [CE Docker pro systém Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) tak, aby kontejnery můžete spustit na Windows 10.</span><span class="sxs-lookup"><span data-stu-id="9c324-111">Install [Docker CE for Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) so that you can run containers on Windows 10.</span></span>
2. <span data-ttu-id="9c324-112">Seznamte se s hello [rychlý start pro Windows 10 kontejnery][link-container-quickstart].</span><span class="sxs-lookup"><span data-stu-id="9c324-112">Familiarize yourself with hello [Windows 10 Containers quickstart][link-container-quickstart].</span></span>
3. <span data-ttu-id="9c324-113">Stáhnout hello [Fabrikam Fiber CallCenter] [ link-fabrikam-github] ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c324-113">Download hello [Fabrikam Fiber CallCenter][link-fabrikam-github] sample application.</span></span>
4. <span data-ttu-id="9c324-114">Nainstalujte [prostředí Azure PowerShell][link-azure-powershell-install]</span><span class="sxs-lookup"><span data-stu-id="9c324-114">Install [Azure PowerShell][link-azure-powershell-install]</span></span>
5. <span data-ttu-id="9c324-115">Nainstalujte hello [rozšíření průběžné doručování nástrojů pro Visual Studio 2017][link-visualstudio-cd-extension]</span><span class="sxs-lookup"><span data-stu-id="9c324-115">Install hello [Continuous Delivery Tools extension for Visual Studio 2017][link-visualstudio-cd-extension]</span></span>
6. <span data-ttu-id="9c324-116">Vytvoření [předplatného Azure] [ link-azure-subscription] a [účet Visual Studio Team Services][link-vsts-account].</span><span class="sxs-lookup"><span data-stu-id="9c324-116">Create an [Azure subscription][link-azure-subscription] and a [Visual Studio Team Services account][link-vsts-account].</span></span> 
7. [<span data-ttu-id="9c324-117">Vytvoření clusteru s podporou v Azure</span><span class="sxs-lookup"><span data-stu-id="9c324-117">Create a cluster on Azure</span></span>](service-fabric-tutorial-create-cluster-azure-ps.md)

## <a name="containerize-hello-application"></a><span data-ttu-id="9c324-118">Containerize aplikace hello</span><span class="sxs-lookup"><span data-stu-id="9c324-118">Containerize hello application</span></span>

<span data-ttu-id="9c324-119">Teď, když máte [cluster Service Fabric běží v Azure](service-fabric-tutorial-create-cluster-azure-ps.md) jsou připravené toocreate a nasadit kontejnerizované aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9c324-119">Now that you have a [Service Fabric cluster is running in Azure](service-fabric-tutorial-create-cluster-azure-ps.md) you are ready toocreate and deploy a containerized application.</span></span> <span data-ttu-id="9c324-120">toostart spuštění naše aplikace v kontejneru, potřebujeme tooadd **Docker podporu** toohello projektu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9c324-120">toostart running our application in a container, we need tooadd **Docker Support** toohello project in Visual Studio.</span></span> <span data-ttu-id="9c324-121">Když přidáte **Docker podporu** dojít dvě věci toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c324-121">When you add **Docker support** toohello application, two things happen.</span></span> <span data-ttu-id="9c324-122">První, _soubor Docker_ je přidána toohello projektu.</span><span class="sxs-lookup"><span data-stu-id="9c324-122">First, a _Dockerfile_ is added toohello project.</span></span> <span data-ttu-id="9c324-123">Tohoto nového souboru popisuje, jak hello kontejneru image je toobe vytvořené.</span><span class="sxs-lookup"><span data-stu-id="9c324-123">This new file describes how hello container image is toobe built.</span></span> <span data-ttu-id="9c324-124">Potom druhé, nový _docker compose_ projekt je přidán toohello řešení.</span><span class="sxs-lookup"><span data-stu-id="9c324-124">Then second, a new _docker-compose_ project is added toohello solution.</span></span> <span data-ttu-id="9c324-125">Hello nový projekt obsahuje několik docker compose soubory.</span><span class="sxs-lookup"><span data-stu-id="9c324-125">hello new project contains a few docker-compose files.</span></span> <span data-ttu-id="9c324-126">Docker compose soubory mohou být použité toodescribe jak spusťte kontejner hello.</span><span class="sxs-lookup"><span data-stu-id="9c324-126">Docker-compose files can be used toodescribe how hello container is run.</span></span>

<span data-ttu-id="9c324-127">Další informace o práci s [kontejner nástroje sady Visual Studio][link-visualstudio-container-tools].</span><span class="sxs-lookup"><span data-stu-id="9c324-127">More info on working with [Visual Studio Container Tools][link-visualstudio-container-tools].</span></span>

>[!NOTE]
><span data-ttu-id="9c324-128">Pokud je hello prvním používáte bitových kopií kontejneru systému Windows v počítači, musí Docker CE stahují hello základní Image pro kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="9c324-128">If it is hello first time you are running Windows container images on your computer, Docker CE must pull down hello base images for your containers.</span></span> <span data-ttu-id="9c324-129">Hello obrázků použitých v tomto kurzu jsou 14 GB.</span><span class="sxs-lookup"><span data-stu-id="9c324-129">hello images used in this tutorial are 14 GB.</span></span> <span data-ttu-id="9c324-130">Pokračujte a spusťte následující příkaz terminálu toopull hello základní Image hello:</span><span class="sxs-lookup"><span data-stu-id="9c324-130">Go ahead and run hello following terminal command toopull hello base images:</span></span>
>```cmd
>docker pull microsoft/mssql-server-windows-developer
>docker pull microsoft/aspnet:4.6.2
>```

### <a name="add-docker-support"></a><span data-ttu-id="9c324-131">Přidání podpory Docker</span><span class="sxs-lookup"><span data-stu-id="9c324-131">Add Docker support</span></span>

<span data-ttu-id="9c324-132">Otevřete hello [FabrikamFiber.CallCenter.sln] [ link-fabrikam-github] souborů v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9c324-132">Open hello [FabrikamFiber.CallCenter.sln][link-fabrikam-github] file in Visual Studio.</span></span>

<span data-ttu-id="9c324-133">Klikněte pravým tlačítkem na hello **FabrikamFiber.Web** Projekt > **přidat** > **Docker podporu**.</span><span class="sxs-lookup"><span data-stu-id="9c324-133">Right-click hello **FabrikamFiber.Web** project > **Add** > **Docker Support**.</span></span>

### <a name="add-support-for-sql"></a><span data-ttu-id="9c324-134">Přidání podpory pro SQL</span><span class="sxs-lookup"><span data-stu-id="9c324-134">Add support for SQL</span></span>

<span data-ttu-id="9c324-135">Tato aplikace používá jako hello zprostředkovatele dat, SQL, tak, aby SQL server vyžaduje toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c324-135">This application uses SQL as hello data provider, so a SQL server is required toorun hello application.</span></span> <span data-ttu-id="9c324-136">Referenční bitovou kopii systému SQL Server kontejneru v našem docker compose.override.yml souboru.</span><span class="sxs-lookup"><span data-stu-id="9c324-136">Reference a SQL Server container image in our docker-compose.override.yml file.</span></span>

<span data-ttu-id="9c324-137">V sadě Visual Studio otevřete **Průzkumníku řešení**, Najít **docker compose**a soubor otevřít hello **docker compose.override.yml**.</span><span class="sxs-lookup"><span data-stu-id="9c324-137">In Visual Studio, open **Solution Explorer**, find **docker-compose**, and open hello file **docker-compose.override.yml**.</span></span>

<span data-ttu-id="9c324-138">Přejděte toohello `services:` uzlu přidat uzel s názvem `db:` položka systému SQL Server hello hello kontejneru, který definuje.</span><span class="sxs-lookup"><span data-stu-id="9c324-138">Navigate toohello `services:` node, add a node named `db:` that defines hello SQL Server entry for hello container.</span></span>

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
><span data-ttu-id="9c324-139">Jakýkoli Server SQL dáváte přednost pro místní ladění, můžete použít, dokud je dostupný z vašeho hostitele.</span><span class="sxs-lookup"><span data-stu-id="9c324-139">You can use any SQL Server you prefer for local debugging, as long as it is reachable from your host.</span></span> <span data-ttu-id="9c324-140">Ale **localdb** nepodporuje `container -> host` komunikace.</span><span class="sxs-lookup"><span data-stu-id="9c324-140">However, **localdb** does not support `container -> host` communication.</span></span>

>[!WARNING]
><span data-ttu-id="9c324-141">Zachování dat není podporován systémem SQL Server v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="9c324-141">Running SQL Server in a container does not support persisting data.</span></span> <span data-ttu-id="9c324-142">Když se zastaví kontejner hello, data budou vymazána.</span><span class="sxs-lookup"><span data-stu-id="9c324-142">When hello container stops, your data is erased.</span></span> <span data-ttu-id="9c324-143">Nepoužívejte tuto konfiguraci pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="9c324-143">Do not use this configuration for production.</span></span>

<span data-ttu-id="9c324-144">Přejděte toohello `fabrikamfiber.web:` uzel a přidat podřízený uzel s názvem `depends_on:`.</span><span class="sxs-lookup"><span data-stu-id="9c324-144">Navigate toohello `fabrikamfiber.web:` node and add a child node named `depends_on:`.</span></span> <span data-ttu-id="9c324-145">To zajistí, že hello `db` (kontejner systému SQL Server hello) bude spuštěn před naše webové aplikace (fabrikamfiber.web).</span><span class="sxs-lookup"><span data-stu-id="9c324-145">This ensures that hello `db` service (hello SQL Server container) starts before our web application (fabrikamfiber.web).</span></span>

```yml
  fabrikamfiber.web:
    depends_on:
      - db
```

### <a name="update-hello-web-config"></a><span data-ttu-id="9c324-146">Aktualizovat hello webové konfigurace</span><span class="sxs-lookup"><span data-stu-id="9c324-146">Update hello web config</span></span>

<span data-ttu-id="9c324-147">Zpět v hello **FabrikamFiber.Web** projektu, aktualizace hello připojovací řetězec v hello **web.config** souboru toopoint toohello systému SQL Server v kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="9c324-147">Back in hello **FabrikamFiber.Web** project, update hello connection string in hello **web.config** file, toopoint toohello SQL Server in hello container.</span></span>

```xml
<add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

<add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
```

>[!NOTE]
><span data-ttu-id="9c324-148">Pokud chcete, aby toouse jiný Server SQL při sestavování verze sestavení webové aplikace, přidejte jiný soubor web.release.config tooyour připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="9c324-148">If you want toouse a different SQL Server when building a release build of your web application, add another connection string tooyour web.release.config file.</span></span>

### <a name="test-your-container"></a><span data-ttu-id="9c324-149">Vaše kontejner testů</span><span class="sxs-lookup"><span data-stu-id="9c324-149">Test your container</span></span>

<span data-ttu-id="9c324-150">Stiskněte klávesu **F5** toorun a ladění aplikace hello v vašeho kontejneru.</span><span class="sxs-lookup"><span data-stu-id="9c324-150">Press **F5** toorun and debug hello application in your container.</span></span>

<span data-ttu-id="9c324-151">Hraniční otevře se stránka definované spuštění vaší aplikace pomocí IP adresy hello hello kontejneru na hello interní NAT síti (obvykle 172.x.x.x).</span><span class="sxs-lookup"><span data-stu-id="9c324-151">Edge opens your application's defined launch page using hello IP address of hello container on hello internal NAT network (typically 172.x.x.x).</span></span> <span data-ttu-id="9c324-152">toolearn Další informace o ladění aplikací v kontejnerech pomocí Visual Studio 2017, najdete v části [v tomto článku][link-debug-container].</span><span class="sxs-lookup"><span data-stu-id="9c324-152">toolearn more about debugging applications in containers using Visual Studio 2017, see [this article][link-debug-container].</span></span>

![třeba společnost fabrikam v kontejneru][image-web-preview]

<span data-ttu-id="9c324-154">kontejner Hello je nyní připraven toobe vytvořené a zabalené aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="9c324-154">hello container is now ready toobe built and packaged in a Service Fabric application.</span></span> <span data-ttu-id="9c324-155">Jakmile máte hello kontejneru bitové kopie vytvořené v počítači, můžete poslat ho tooany kontejneru registru a stahují toorun tooany hostitele.</span><span class="sxs-lookup"><span data-stu-id="9c324-155">Once you have hello container image built on your machine, you can push it tooany container registry and pull it down tooany host toorun.</span></span>

## <a name="get-hello-application-ready-for-hello-cloud"></a><span data-ttu-id="9c324-156">Příprava aplikace hello hello cloudu</span><span class="sxs-lookup"><span data-stu-id="9c324-156">Get hello application ready for hello cloud</span></span>

<span data-ttu-id="9c324-157">aplikace hello tooget připraven ke spuštění v Service Fabric v Azure, potřebujeme toocomplete dva kroky:</span><span class="sxs-lookup"><span data-stu-id="9c324-157">tooget hello application ready for running in Service Fabric in Azure, we need toocomplete two steps:</span></span>

1. <span data-ttu-id="9c324-158">Vystavení hello portu, kde Chceme mít tooreach toobe, naše webové aplikace v clusteru Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="9c324-158">Expose hello port where we want toobe able tooreach our web application in hello Service Fabric cluster.</span></span>
2. <span data-ttu-id="9c324-159">Zadejte databázi SQL připravené pro produkční pro naši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9c324-159">Provide a production ready SQL database for our application.</span></span>

### <a name="expose-hello-port-for-hello-app"></a><span data-ttu-id="9c324-160">Vystavení hello port pro aplikace hello</span><span class="sxs-lookup"><span data-stu-id="9c324-160">Expose hello port for hello app</span></span>
<span data-ttu-id="9c324-161">cluster Service Fabric Hello jsme nakonfigurovali, má port *80* otevřete ve výchozím nastavení v hello nástroj pro vyrovnávání zatížení Azure, který vyrovnává příchozí provoz toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="9c324-161">hello Service Fabric cluster we have configured, has port *80* open by default in hello Azure Load Balancer, that balances incoming traffic toohello cluster.</span></span> <span data-ttu-id="9c324-162">Naše kontejneru na port jsme můžou zpřístupnit prostřednictvím našich soubor docker-compose.yml.</span><span class="sxs-lookup"><span data-stu-id="9c324-162">We can expose our container on this port via our docker-compose.yml file.</span></span>

<span data-ttu-id="9c324-163">V sadě Visual Studio otevřete **Průzkumníku řešení**, Najít **docker compose**a soubor otevřít hello **docker compose.override.yml**.</span><span class="sxs-lookup"><span data-stu-id="9c324-163">In Visual Studio, open **Solution Explorer**, find **docker-compose**, and open hello file **docker-compose.override.yml**.</span></span>

<span data-ttu-id="9c324-164">Upravit hello `fabrikamfiber.web:` uzlu přidat podřízený uzel s názvem `ports:`.</span><span class="sxs-lookup"><span data-stu-id="9c324-164">Modify hello `fabrikamfiber.web:` node, add a child node named `ports:`.</span></span>

<span data-ttu-id="9c324-165">Přidat položku řetězec `- "80:80"`.</span><span class="sxs-lookup"><span data-stu-id="9c324-165">Add a string entry `- "80:80"`.</span></span>

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

### <a name="use-a-production-sql-database"></a><span data-ttu-id="9c324-166">Použít provozní databáze SQL</span><span class="sxs-lookup"><span data-stu-id="9c324-166">Use a production SQL database</span></span>
<span data-ttu-id="9c324-167">Při spuštění v produkčním prostředí, potřebujeme naše data v našem databáze.</span><span class="sxs-lookup"><span data-stu-id="9c324-167">When running in production, we need our data persisted in our database.</span></span> <span data-ttu-id="9c324-168">Aktuálně neexistují žádná data trvalé tooguarantee způsob, jak v kontejneru, proto nelze uložit provozních dat v systému SQL Server v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="9c324-168">There is currently no way tooguarantee persistent data in a container, therefore you cannot store production data in SQL Server in a container.</span></span>

<span data-ttu-id="9c324-169">Doporučujeme, abyste že využívat Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="9c324-169">We recommend you utilize an Azure SQL Database.</span></span> <span data-ttu-id="9c324-170">tooset nahoru a spusťte na spravovaný Server SQL Azure, navštivte hello [Azure SQL Database – elementy QuickStart] [ link-azure-sql] článku.</span><span class="sxs-lookup"><span data-stu-id="9c324-170">tooset up and run a managed SQL Server in Azure, visit hello [Azure SQL Database Quickstarts][link-azure-sql] article.</span></span>

>[!NOTE]
><span data-ttu-id="9c324-171">Mějte na paměti, toochange hello připojovací řetězce toohello SQL server v hello **web.release.config** souboru v hello **FabrikamFiber.Web** projektu.</span><span class="sxs-lookup"><span data-stu-id="9c324-171">Remember toochange hello connection strings toohello SQL server in hello **web.release.config** file in hello **FabrikamFiber.Web** project.</span></span>
>
><span data-ttu-id="9c324-172">Tato aplikace se elegantně selže, pokud žádné databáze SQL je dostupný.</span><span class="sxs-lookup"><span data-stu-id="9c324-172">This application fails gracefully if no SQL database is reachable.</span></span> <span data-ttu-id="9c324-173">Můžete zvolit toogo dopředu a nasadit aplikace hello s žádné systému SQL server.</span><span class="sxs-lookup"><span data-stu-id="9c324-173">You can choose toogo ahead and deploy hello application with no SQL server.</span></span>

## <a name="deploy-with-visual-studio-team-services"></a><span data-ttu-id="9c324-174">Nasazení s Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="9c324-174">Deploy with Visual Studio Team Services</span></span>

<span data-ttu-id="9c324-175">tooset až nasazení pomocí sady Visual Studio Team Services, budete potřebovat tooinstall hello [rozšíření průběžné doručování nástrojů pro Visual Studio 2017][link-visualstudio-cd-extension].</span><span class="sxs-lookup"><span data-stu-id="9c324-175">tooset up deployment using Visual Studio Team Services, you need tooinstall hello [Continuous Delivery Tools extension for Visual Studio 2017][link-visualstudio-cd-extension].</span></span> <span data-ttu-id="9c324-176">Toto rozšíření umožňuje snadno toodeploy tooAzure nakonfigurováním Visual Studio Team Services a získat cluster Service Fabric tooyour nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c324-176">This extension makes it easy toodeploy tooAzure by configuring Visual Studio Team Services and get your app deployed tooyour Service Fabric cluster.</span></span>

<span data-ttu-id="9c324-177">tooget spuštění kódu musí být hostované ve správě zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="9c324-177">tooget started, your code must be hosted in source control.</span></span> <span data-ttu-id="9c324-178">Hello zbývající část tohoto oddílu předpokládá **git** je používán.</span><span class="sxs-lookup"><span data-stu-id="9c324-178">hello rest of this section assumes **git** is being used.</span></span>

### <a name="set-up-a-vsts-repo"></a><span data-ttu-id="9c324-179">Nastavení služby VSTS úložišti</span><span class="sxs-lookup"><span data-stu-id="9c324-179">Set up a VSTS repo</span></span>
<span data-ttu-id="9c324-180">V pravém dolním rohu hello sady Visual Studio, klikněte na položku **přidat tooSource řízení** > **Git** (nebo obě tyto možnosti dáváte přednost).</span><span class="sxs-lookup"><span data-stu-id="9c324-180">At hello bottom-right corner of Visual Studio, click **Add tooSource Control** > **Git** (or whichever option you prefer).</span></span>

![tlačítko hello zdroj ovládacího prvku][image-source-control]

<span data-ttu-id="9c324-182">V hello _Team Explorer_ podokně, stiskněte klávesu **publikování úložiště Git**.</span><span class="sxs-lookup"><span data-stu-id="9c324-182">In hello _Team Explorer_ pane, press **Publish Git Repo**.</span></span>

<span data-ttu-id="9c324-183">Vyberte název služby VSTS úložiště a stiskněte klávesu **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="9c324-183">Select your VSTS repository name and press **Repository**.</span></span>

![publikování tooVSTS úložišti][image-publish-repo]

<span data-ttu-id="9c324-185">Teď, když kód synchronizována s zdrojové úložiště služby VSTS, můžete nakonfigurovat průběžnou integraci a nastavené průběžné doručování.</span><span class="sxs-lookup"><span data-stu-id="9c324-185">Now that your code is synchronized with a VSTS source repository, you can configure continuous integration and continuous delivery.</span></span>

### <a name="setup-continuous-delivery"></a><span data-ttu-id="9c324-186">Instalační program nastavené průběžné doručování</span><span class="sxs-lookup"><span data-stu-id="9c324-186">Setup continuous delivery</span></span>

<span data-ttu-id="9c324-187">V _Průzkumníku řešení_, klikněte pravým tlačítkem na hello **řešení** > **konfigurace nastavené průběžné doručování**.</span><span class="sxs-lookup"><span data-stu-id="9c324-187">In _Solution Explorer_, right-click hello **solution** > **Configure Continuous Delivery**.</span></span>

<span data-ttu-id="9c324-188">Vyberte hello předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="9c324-188">Select hello Azure Subscription.</span></span>

<span data-ttu-id="9c324-189">Nastavit **typ hostitele** příliš**Service Fabric Cluster**.</span><span class="sxs-lookup"><span data-stu-id="9c324-189">Set **Host Type** too**Service Fabric Cluster**.</span></span>

<span data-ttu-id="9c324-190">Nastavit **cílového hostitele** cluster service fabric toohello jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="9c324-190">Set **Target Host** toohello service fabric cluster you created in hello previous section.</span></span>

<span data-ttu-id="9c324-191">Vyberte **kontejneru registru** toopublish kontejner tak, aby.</span><span class="sxs-lookup"><span data-stu-id="9c324-191">Choose a **Container Registry** toopublish your container to.</span></span>

>[!TIP]
><span data-ttu-id="9c324-192">Použití hello **upravit** tlačítko toocreate registru kontejneru.</span><span class="sxs-lookup"><span data-stu-id="9c324-192">Use hello **Edit** button toocreate a container registry.</span></span>

<span data-ttu-id="9c324-193">Stiskněte **OK**.</span><span class="sxs-lookup"><span data-stu-id="9c324-193">Press **OK**.</span></span>

![instalace služby fabric průběžnou integraci][image-setup-ci]
   
   <span data-ttu-id="9c324-195">Po dokončení konfigurace hello vaší kontejner je nasazené tooService prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="9c324-195">Once hello configuration is completed, your container is deployed tooService Fabric.</span></span> <span data-ttu-id="9c324-196">Vždy, když nabízená aktualizace toohello úložiště je provést nové sestavení a verze.</span><span class="sxs-lookup"><span data-stu-id="9c324-196">Whenever you push updates toohello repository a new build and release is executed.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="9c324-197">Vytváření hello kontejneru Image trvat přibližně 15 minut.</span><span class="sxs-lookup"><span data-stu-id="9c324-197">Building hello container images take approximately 15 minutes.</span></span>
   ><span data-ttu-id="9c324-198">cluster Service Fabric toohello první nasazení Hello způsobí, že hello základní jádro systému Windows Server kontejneru bitové kopie toobe stáhli.</span><span class="sxs-lookup"><span data-stu-id="9c324-198">hello first deployment toohello Service Fabric cluster causes hello base Windows Server Core container images toobe downloaded.</span></span> <span data-ttu-id="9c324-199">stažení Hello trvá toocomplete dalších 5 až 10 minut.</span><span class="sxs-lookup"><span data-stu-id="9c324-199">hello download takes additional 5-10 minutes toocomplete.</span></span>

<span data-ttu-id="9c324-200">Procházet toohello Fabrikam Call Center aplikace pomocí adresy url hello clusteru: například *http://mycluster.westeurope.cloudapp.azure.com*</span><span class="sxs-lookup"><span data-stu-id="9c324-200">Browse toohello Fabrikam Call Center application using hello url of your cluster: for example, *http://mycluster.westeurope.cloudapp.azure.com*</span></span>

<span data-ttu-id="9c324-201">Teď, když máte kontejnerizované a nasadit řešení hello Fabrikam Call Center, můžete otevřít hello [portál Azure] [ link-azure-portal] a zobrazit hello aplikace běžící v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="9c324-201">Now that you have containerized and deployed hello Fabrikam Call Center solution, you can open hello [Azure portal][link-azure-portal] and see hello application running in Service Fabric.</span></span> <span data-ttu-id="9c324-202">aplikace hello tootry, otevřete webový prohlížeč a přejděte toohello adresu URL clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="9c324-202">tootry hello application, open a web browser and go toohello URL of your Service Fabric cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9c324-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9c324-203">Next steps</span></span>

<span data-ttu-id="9c324-204">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="9c324-204">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9c324-205">Vytvoření projektu Docker v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c324-205">Create a Docker project in Visual Studio</span></span>
> * <span data-ttu-id="9c324-206">Containerize stávající aplikaci</span><span class="sxs-lookup"><span data-stu-id="9c324-206">Containerize an existing application</span></span>
> * <span data-ttu-id="9c324-207">Instalační program průběžnou integraci s Visual Studio a služby VSTS</span><span class="sxs-lookup"><span data-stu-id="9c324-207">Setup continuous integration with Visual Studio and VSTS</span></span>

<!--   NOTE SURE WHAT WE SHOULD DO YET HERE

Advance toohello next tutorial toolearn how toobind a custom SSL certificate tooit.

> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate tooAzure Web Apps](app-service-web-tutorial-custom-ssl.md)

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
