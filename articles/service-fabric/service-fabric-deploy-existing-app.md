---
title: "aaaDeploy existující spustitelné tooAzure Service Fabric | Microsoft Docs"
description: "Návod na způsobu nasazení toopackage existující aplikace jako spustitelný hosta, aby ho bylo možné tooa cluster Service Fabric"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/02/2017
ms.author: mfussell;mikhegn
ms.openlocfilehash: 5599802bdb6bda2407a138d77e12148ccb64f437
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-guest-executable-tooservice-fabric"></a><span data-ttu-id="cced8-103">Nasazení spustitelné tooService hosta prostředků infrastruktury</span><span class="sxs-lookup"><span data-stu-id="cced8-103">Deploy a guest executable tooService Fabric</span></span>
<span data-ttu-id="cced8-104">Jakýkoli typ kódu, třeba Node.js, Java nebo C++ v Azure Service Fabric můžete spustit jako služby.</span><span class="sxs-lookup"><span data-stu-id="cced8-104">You can run any type of code, such as Node.js, Java, or C++ in Azure Service Fabric as a service.</span></span> <span data-ttu-id="cced8-105">Service Fabric označuje toothese typů služeb jako hosta spustitelné soubory.</span><span class="sxs-lookup"><span data-stu-id="cced8-105">Service Fabric refers toothese types of services as guest executables.</span></span>

<span data-ttu-id="cced8-106">Spustitelné soubory hosta budou vyhodnocené jako bezstavové služby Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="cced8-106">Guest executables are treated by Service Fabric like stateless services.</span></span> <span data-ttu-id="cced8-107">V důsledku toho jsou umístěny na uzlech v clusteru, na základě dostupnosti a další metriky.</span><span class="sxs-lookup"><span data-stu-id="cced8-107">As a result, they are placed on nodes in a cluster, based on availability and other metrics.</span></span> <span data-ttu-id="cced8-108">Tento článek popisuje, jak toopackage a nasadit cluster hostů spustitelné tooa Service Fabric pomocí Visual Studio nebo nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="cced8-108">This article describes how toopackage and deploy a guest executable tooa Service Fabric cluster, by using Visual Studio or a command-line utility.</span></span>

<span data-ttu-id="cced8-109">V tomto článku jsme zahrnují hello kroky toopackage hosta spustitelný soubor a nasaďte ji tooService prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="cced8-109">In this article, we cover hello steps toopackage a guest executable and deploy it tooService Fabric.</span></span>  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a><span data-ttu-id="cced8-110">Výhody spuštění spustitelného souboru v Service Fabric Host</span><span class="sxs-lookup"><span data-stu-id="cced8-110">Benefits of running a guest executable in Service Fabric</span></span>
<span data-ttu-id="cced8-111">Existuje několik výhod toorunning spustitelný soubor v clusteru Service Fabric Host:</span><span class="sxs-lookup"><span data-stu-id="cced8-111">There are several advantages toorunning a guest executable in a Service Fabric cluster:</span></span>

* <span data-ttu-id="cced8-112">Vysoká dostupnost.</span><span class="sxs-lookup"><span data-stu-id="cced8-112">High availability.</span></span> <span data-ttu-id="cced8-113">Aplikace, které běží v Service Fabric jsou vysoce dostupné.</span><span class="sxs-lookup"><span data-stu-id="cced8-113">Applications that run in Service Fabric are made highly available.</span></span> <span data-ttu-id="cced8-114">Service Fabric zajistí, že instance aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="cced8-114">Service Fabric ensures that instances of an application are running.</span></span>
* <span data-ttu-id="cced8-115">Sledování stavu.</span><span class="sxs-lookup"><span data-stu-id="cced8-115">Health monitoring.</span></span> <span data-ttu-id="cced8-116">Monitorování stavu Service Fabric zjišťuje, zda aplikace běží a poskytuje diagnostické informace, pokud dojde k selhání.</span><span class="sxs-lookup"><span data-stu-id="cced8-116">Service Fabric health monitoring detects if an application is running, and provides diagnostic information if there is a failure.</span></span>   
* <span data-ttu-id="cced8-117">Správa životního cyklu aplikací.</span><span class="sxs-lookup"><span data-stu-id="cced8-117">Application lifecycle management.</span></span> <span data-ttu-id="cced8-118">Kromě toho poskytuje upgrady bez výpadků, Service Fabric nabízí automatického vrácení zpět toohello předchozí verze, pokud existuje událost stavu chybný hlášené během upgradu.</span><span class="sxs-lookup"><span data-stu-id="cced8-118">Besides providing upgrades with no downtime, Service Fabric provides automatic rollback toohello previous version if there is a bad health event reported during an upgrade.</span></span>    
* <span data-ttu-id="cced8-119">Hustotou.</span><span class="sxs-lookup"><span data-stu-id="cced8-119">Density.</span></span> <span data-ttu-id="cced8-120">Více aplikací můžete spustit v clusteru, která eliminuje potřebu hello toorun každou aplikaci na svůj vlastní hardware.</span><span class="sxs-lookup"><span data-stu-id="cced8-120">You can run multiple applications in a cluster, which eliminates hello need for each application toorun on its own hardware.</span></span>
* <span data-ttu-id="cced8-121">Možnosti rozpoznání: Pomocí REST můžete volat hello Service Fabric Naming service toofind další služby v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-121">Discoverability: Using REST you can call hello Service Fabric Naming service toofind other services in hello cluster.</span></span> 

## <a name="samples"></a><span data-ttu-id="cced8-122">Ukázky</span><span class="sxs-lookup"><span data-stu-id="cced8-122">Samples</span></span>
* [<span data-ttu-id="cced8-123">Ukázka pro balení a nasazení spustitelný soubor hosta</span><span class="sxs-lookup"><span data-stu-id="cced8-123">Sample for packaging and deploying a guest executable</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="cced8-124">Ukázka dvěma hosta spustitelné soubory (C# a nodejs) komunikaci přes hello pojmenování službu pomocí REST</span><span class="sxs-lookup"><span data-stu-id="cced8-124">Sample of two guest executables (C# and nodejs) communicating via hello Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a><span data-ttu-id="cced8-125">Přehled aplikace a soubory manifestu služby</span><span class="sxs-lookup"><span data-stu-id="cced8-125">Overview of application and service manifest files</span></span>
<span data-ttu-id="cced8-126">Jako součást nasazení spustitelný soubor hosta, je užitečné toounderstand hello Service Fabric balení a nasazení modelu jak je popsáno v [aplikačního modelu](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="cced8-126">As part of deploying a guest executable, it is useful toounderstand hello Service Fabric packaging and deployment model as described in [application model](service-fabric-application-model.md).</span></span> <span data-ttu-id="cced8-127">model balení Service Fabric Hello spoléhá na dva soubory XML: hello manifestů aplikace a služby.</span><span class="sxs-lookup"><span data-stu-id="cced8-127">hello Service Fabric packaging model relies on two XML files: hello application and service manifests.</span></span> <span data-ttu-id="cced8-128">Hello definice schématu pro hello ApplicationManifest.xml a ServiceManifest.xml soubory se instaluje s hello Service Fabric SDK do *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="cced8-128">hello schema definition for hello ApplicationManifest.xml and ServiceManifest.xml files is installed with hello Service Fabric SDK into *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

* <span data-ttu-id="cced8-129">**Manifest aplikace** manifest aplikace hello je použité toodescribe hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="cced8-129">**Application manifest** hello application manifest is used toodescribe hello application.</span></span> <span data-ttu-id="cced8-130">Vypíše hello služby, které ji tvoří a dalších parametrů, které jsou používané toodefine musí být nasazené jak jednu nebo více služeb, jako je například hello počet instancí.</span><span class="sxs-lookup"><span data-stu-id="cced8-130">It lists hello services that compose it, and other parameters that are used toodefine how one or more services should be deployed, such as hello number of instances.</span></span>

  <span data-ttu-id="cced8-131">Aplikace v Service Fabric je jednotka nasazení a upgrade.</span><span class="sxs-lookup"><span data-stu-id="cced8-131">In Service Fabric, an application is a unit of deployment and upgrade.</span></span> <span data-ttu-id="cced8-132">Aplikace lze upgradovat jako na jednu jednotku, kde se spravují potenciální chyby a potenciální odvolání.</span><span class="sxs-lookup"><span data-stu-id="cced8-132">An application can be upgraded as a single unit where potential failures and potential rollbacks are managed.</span></span> <span data-ttu-id="cced8-133">Service Fabric zaručuje, že proces upgradu hello je buď úspěšné, nebo pokud hello upgrade selže, nenechává aplikace hello v neznámý nebo nestabilním stavu.</span><span class="sxs-lookup"><span data-stu-id="cced8-133">Service Fabric guarantees that hello upgrade process is either successful, or, if hello upgrade fails, does not leave hello application in an unknown or unstable state.</span></span>
* <span data-ttu-id="cced8-134">**Manifest služby** hello service manifest popisuje hello součásti služby.</span><span class="sxs-lookup"><span data-stu-id="cced8-134">**Service manifest** hello service manifest describes hello components of a service.</span></span> <span data-ttu-id="cced8-135">Obsahuje data, jako třeba hello název a typ služby a kódu a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="cced8-135">It includes data, such as hello name and type of service, and its code and configuration.</span></span> <span data-ttu-id="cced8-136">Hello service manifest také zahrnuje některé další parametry, které se dají použít tooconfigure hello služby po jejím nasazení.</span><span class="sxs-lookup"><span data-stu-id="cced8-136">hello service manifest also includes some additional parameters that can be used tooconfigure hello service once it is deployed.</span></span>

## <a name="application-package-file-structure"></a><span data-ttu-id="cced8-137">Struktura souboru balíčku aplikace</span><span class="sxs-lookup"><span data-stu-id="cced8-137">Application package file structure</span></span>
<span data-ttu-id="cced8-138">toodeploy aplikaci tooService prostředků infrastruktury, aplikace hello by mělo vycházet předdefinované adresářovou strukturu.</span><span class="sxs-lookup"><span data-stu-id="cced8-138">toodeploy an application tooService Fabric, hello application should follow a predefined directory structure.</span></span> <span data-ttu-id="cced8-139">Hello následuje příklad této struktury.</span><span class="sxs-lookup"><span data-stu-id="cced8-139">hello following is an example of that structure.</span></span>

```
|-- ApplicationPackageRoot
    |-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```

<span data-ttu-id="cced8-140">Hello ApplicationPackageRoot obsahuje hello ApplicationManifest.xml soubor, který definuje aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-140">hello ApplicationPackageRoot contains hello ApplicationManifest.xml file that defines hello application.</span></span> <span data-ttu-id="cced8-141">Podadresář pro každou službu součástí aplikace hello je použité toocontain všechny hello artefakty, pomocí kterých hello služba vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="cced8-141">A subdirectory for each service included in hello application is used toocontain all hello artifacts that hello service requires.</span></span> <span data-ttu-id="cced8-142">Tyto podadresáře jsou hello ServiceManifest.xml a obvykle hello následující:</span><span class="sxs-lookup"><span data-stu-id="cced8-142">These subdirectories are hello ServiceManifest.xml and, typically, hello following:</span></span>

* <span data-ttu-id="cced8-143">*Kód*.</span><span class="sxs-lookup"><span data-stu-id="cced8-143">*Code*.</span></span> <span data-ttu-id="cced8-144">Tento adresář obsahuje kódu služby hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-144">This directory contains hello service code.</span></span>
* <span data-ttu-id="cced8-145">*Konfigurace*. Tento adresář obsahuje soubor souborech Settings.xml (a další soubory v případě potřeby), služba hello můžete získat přístup v modulu runtime tooretrieve specifické nastavení.</span><span class="sxs-lookup"><span data-stu-id="cced8-145">*Config*. This directory contains a Settings.xml file (and other files if necessary) that hello service can access at runtime tooretrieve specific configuration settings.</span></span>
* <span data-ttu-id="cced8-146">*Data*.</span><span class="sxs-lookup"><span data-stu-id="cced8-146">*Data*.</span></span> <span data-ttu-id="cced8-147">Toto je další adresář toostore další místní data, která může být nutné hello služby.</span><span class="sxs-lookup"><span data-stu-id="cced8-147">This is an additional directory toostore additional local data that hello service may need.</span></span> <span data-ttu-id="cced8-148">Data by měla být pouze dočasné dat použité toostore.</span><span class="sxs-lookup"><span data-stu-id="cced8-148">Data should be used toostore only ephemeral data.</span></span> <span data-ttu-id="cced8-149">Service Fabric nemá zkopírovat nebo replikovat změny toohello datový adresář, pokud služba hello musí toobe přemístění (například během převzetí služeb při selhání).</span><span class="sxs-lookup"><span data-stu-id="cced8-149">Service Fabric does not copy or replicate changes toohello data directory if hello service needs toobe relocated (for example, during failover).</span></span>

> [!NOTE]
> <span data-ttu-id="cced8-150">Nemáte toocreate hello `config` a `data` adresářů, pokud je nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="cced8-150">You don't have toocreate hello `config` and `data` directories if you don't need them.</span></span>
>
>

## <a name="package-an-existing-executable"></a><span data-ttu-id="cced8-151">Balíček existující spustitelný soubor</span><span class="sxs-lookup"><span data-stu-id="cced8-151">Package an existing executable</span></span>
<span data-ttu-id="cced8-152">Při balení spustitelný soubor hosta, můžete zvolit buď toouse šablona projektu sady Visual Studio nebo příliš[ručně vytvořit balíček aplikace hello](#manually).</span><span class="sxs-lookup"><span data-stu-id="cced8-152">When packaging a guest executable, you can choose either toouse a Visual Studio project template or too[create hello application package manually](#manually).</span></span> <span data-ttu-id="cced8-153">Pomocí sady Visual Studio, hello struktura balíček aplikace a soubory manifestu vytvářejí hello nová šablona projektu pro vás.</span><span class="sxs-lookup"><span data-stu-id="cced8-153">Using Visual Studio, hello application package structure and manifest files are created by hello new project template for you.</span></span>

> [!TIP]
> <span data-ttu-id="cced8-154">Nejjednodušší způsob, jak toopackage Hello existující spustitelný soubor do služby systému Windows je toouse Visual Studio a na Linux toouse Yeoman</span><span class="sxs-lookup"><span data-stu-id="cced8-154">hello easiest way toopackage an existing Windows executable into a service is toouse Visual Studio and on Linux toouse Yeoman</span></span>
>

## <a name="use-visual-studio-toopackage-and-deploy-an-existing-executable"></a><span data-ttu-id="cced8-155">Pomocí sady Visual Studio toopackage a nasadit existující spustitelný soubor</span><span class="sxs-lookup"><span data-stu-id="cced8-155">Use Visual Studio toopackage and deploy an existing executable</span></span>
<span data-ttu-id="cced8-156">Visual Studio poskytuje Service Fabric toohelp šablony služby nasadit cluster hostů spustitelné tooa Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="cced8-156">Visual Studio provides a Service Fabric service template toohelp you deploy a guest executable tooa Service Fabric cluster.</span></span>

1. <span data-ttu-id="cced8-157">Zvolte **soubor** > **nový projekt**a vytvářet aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="cced8-157">Choose **File** > **New Project**, and create a Service Fabric application.</span></span>
2. <span data-ttu-id="cced8-158">Zvolte **spustitelný soubor hosta** jako šablonu služby hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-158">Choose **Guest Executable** as hello service template.</span></span>
3. <span data-ttu-id="cced8-159">Klikněte na tlačítko **Procházet** tooselect hello složka se spustitelný soubor a vyplňte hello zbytek hello parametry toocreate hello služby.</span><span class="sxs-lookup"><span data-stu-id="cced8-159">Click **Browse** tooselect hello folder with your executable and fill in hello rest of hello parameters toocreate hello service.</span></span>
   * <span data-ttu-id="cced8-160">*Kód balíčku chování*.</span><span class="sxs-lookup"><span data-stu-id="cced8-160">*Code Package Behavior*.</span></span> <span data-ttu-id="cced8-161">Může být sada toocopy všechny hello obsah vaší složky toohello projekt sady Visual Studio, což je užitečné, pokud hello spustitelný soubor se nemění.</span><span class="sxs-lookup"><span data-stu-id="cced8-161">Can be set toocopy all hello content of your folder toohello Visual Studio Project, which is useful if hello executable does not change.</span></span> <span data-ttu-id="cced8-162">Pokud očekávat toochange hello spustitelný soubor a dynamicky chcete hello možnost toopick si nových sestavení automaticky, můžete toolink toohello složky místo.</span><span class="sxs-lookup"><span data-stu-id="cced8-162">If you expect hello executable toochange and want hello ability toopick up new builds dynamically, you can choose toolink toohello folder instead.</span></span> <span data-ttu-id="cced8-163">Propojené složek můžete použít při vytváření projektu aplikace hello v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cced8-163">You can use linked folders when creating hello application project in Visual Studio.</span></span> <span data-ttu-id="cced8-164">Tím propojí toohello umístění zdroje z v rámci projektu hello, která umožňuje vám tooupdate hello hosta spustitelný soubor v jeho zdroj cíl.</span><span class="sxs-lookup"><span data-stu-id="cced8-164">This links toohello source location from within hello project, making it possible for you tooupdate hello guest executable in its source destination.</span></span> <span data-ttu-id="cced8-165">Tyto aktualizace se stanou součástí balíčku aplikace hello na sestavení.</span><span class="sxs-lookup"><span data-stu-id="cced8-165">Those updates become part of hello application package on build.</span></span>
   * <span data-ttu-id="cced8-166">*Program* určuje hello spustitelný soubor, který by měl být spuštěn toostart hello služby.</span><span class="sxs-lookup"><span data-stu-id="cced8-166">*Program* specifies hello executable that should be run toostart hello service.</span></span>
   * <span data-ttu-id="cced8-167">*Argumenty* určuje hello argumenty, které mají být předány toohello spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="cced8-167">*Arguments* specifies hello arguments that should be passed toohello executable.</span></span> <span data-ttu-id="cced8-168">Může být seznam parametrů s argumenty.</span><span class="sxs-lookup"><span data-stu-id="cced8-168">It can be a list of parameters with arguments.</span></span>
   * <span data-ttu-id="cced8-169">*WorkingFolder* určuje hello pracovní adresář pro hello proces, který se bude toobe spuštěna.</span><span class="sxs-lookup"><span data-stu-id="cced8-169">*WorkingFolder* specifies hello working directory for hello process that is going toobe started.</span></span> <span data-ttu-id="cced8-170">Můžete určit tří hodnot:</span><span class="sxs-lookup"><span data-stu-id="cced8-170">You can specify three values:</span></span>
     * <span data-ttu-id="cced8-171">`CodeBase`Určuje, že pracovní adresář hello přechází toobe nastavit adresář toohello kódu v balíčku aplikace hello (`Code` directory uvedené v předcházející strukturu souborů hello).</span><span class="sxs-lookup"><span data-stu-id="cced8-171">`CodeBase` specifies that hello working directory is going toobe set toohello code directory in hello application package (`Code` directory shown in hello preceding file structure).</span></span>
     * <span data-ttu-id="cced8-172">`CodePackage`Určuje, že pracovní adresář hello přechází toobe nastavit toohello kořenové balíčku aplikace hello (`GuestService1Pkg` uvedené v předcházející strukturu souborů hello).</span><span class="sxs-lookup"><span data-stu-id="cced8-172">`CodePackage` specifies that hello working directory is going toobe set toohello root of hello application package    (`GuestService1Pkg` shown in hello preceding file structure).</span></span>
     * <span data-ttu-id="cced8-173">`Work`Určuje, že hello soubory jsou umístěny v podadresáři volat pracovní.</span><span class="sxs-lookup"><span data-stu-id="cced8-173">`Work` specifies that hello files are placed in a subdirectory called work.</span></span>
4. <span data-ttu-id="cced8-174">Zadejte název služby a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="cced8-174">Give your service a name, and click **OK**.</span></span>
5. <span data-ttu-id="cced8-175">Pokud vaše služba musí koncový bod pro komunikaci, můžete nyní přidat hello protokol, port a typ toohello ServiceManifest.xml souboru.</span><span class="sxs-lookup"><span data-stu-id="cced8-175">If your service needs an endpoint for communication, you can now add hello protocol, port, and type toohello ServiceManifest.xml file.</span></span> <span data-ttu-id="cced8-176">Například: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.</span><span class="sxs-lookup"><span data-stu-id="cced8-176">For example: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.</span></span>
6. <span data-ttu-id="cced8-177">Teď můžete použít balíček hello a publikovat akce s místním clusteru tak, že ladění hello řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cced8-177">You can now use hello package and publish action against your local cluster by debugging hello solution in Visual Studio.</span></span> <span data-ttu-id="cced8-178">Až bude připravený, můžete publikovat hello aplikace tooa vzdálený cluster nebo zkontrolujte v ovládacím prvku toosource řešení hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-178">When ready, you can publish hello application tooa remote cluster or check in hello solution toosource control.</span></span>
7. <span data-ttu-id="cced8-179">Jak přejděte toohello konci tohoto článku toosee tooview hosta spustitelný soubor služby spuštěné v Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="cced8-179">Go toohello end of this article toosee how tooview your guest executable service running in Service Fabric Explorer.</span></span>

## <a name="use-yoeman-toopackage-and-deploy-an-existing-executable-on-linux"></a><span data-ttu-id="cced8-180">Používat Yoeman toopackage a nasaďte existující spustitelný soubor v systému Linux</span><span class="sxs-lookup"><span data-stu-id="cced8-180">Use Yoeman toopackage and deploy an existing executable on Linux</span></span>

<span data-ttu-id="cced8-181">Hello postup pro vytvoření a nasazení hosta spustitelného souboru v systému Linux je hello stejné jako nasazení aplikace csharp nebo java.</span><span class="sxs-lookup"><span data-stu-id="cced8-181">hello procedure for creating and deploying a guest executable on Linux is hello same as deploying a csharp or java application.</span></span>

1. <span data-ttu-id="cced8-182">V terminálu zadejte `yo azuresfguest`.</span><span class="sxs-lookup"><span data-stu-id="cced8-182">In a terminal, type `yo azuresfguest`.</span></span>
2. <span data-ttu-id="cced8-183">Pojmenujte svoji aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cced8-183">Name your application.</span></span>
3. <span data-ttu-id="cced8-184">Název služby a zadejte podrobnosti hello včetně cestu hello spustitelný soubor a hello parametry, které musí být volána s.</span><span class="sxs-lookup"><span data-stu-id="cced8-184">Name your service, and provide hello details including path of hello executable and hello parameters it must be invoked with.</span></span>

<span data-ttu-id="cced8-185">Yeoman vytvoří balíček aplikace hello příslušné aplikace a soubory manifestu spolu s instalaci a odinstalaci skripty.</span><span class="sxs-lookup"><span data-stu-id="cced8-185">Yeoman creates an application package with hello appropriate application and manifest files along with install and uninstall scripts.</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-an-existing-executable"></a><span data-ttu-id="cced8-186">Ručně zabalení a nasazení existující spustitelný soubor</span><span class="sxs-lookup"><span data-stu-id="cced8-186">Manually package and deploy an existing executable</span></span>
<span data-ttu-id="cced8-187">proces Hello ručně balení spustitelný soubor hosta je založena na hello následující obecné kroky:</span><span class="sxs-lookup"><span data-stu-id="cced8-187">hello process of manually packaging a guest executable is based on hello following general steps:</span></span>

1. <span data-ttu-id="cced8-188">Vytvořte strukturu adresáře balíčku hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-188">Create hello package directory structure.</span></span>
2. <span data-ttu-id="cced8-189">Přidejte kód a konfigurační soubory aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-189">Add hello application's code and configuration files.</span></span>
3. <span data-ttu-id="cced8-190">Upravte soubor manifestu služby hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-190">Edit hello service manifest file.</span></span>
4. <span data-ttu-id="cced8-191">Upravte soubor manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-191">Edit hello application manifest file.</span></span>

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you toocreate hello ApplicationPackage automatically. hello tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-hello-package-directory-structure"></a><span data-ttu-id="cced8-192">Vytvořit strukturu adresáře balíčku hello</span><span class="sxs-lookup"><span data-stu-id="cced8-192">Create hello package directory structure</span></span>
<span data-ttu-id="cced8-193">Vytvořením hello adresářovou strukturu, můžete spustit jak je popsáno v předcházející části hello "Aplikace balíčku souboru struktura."</span><span class="sxs-lookup"><span data-stu-id="cced8-193">You can start by creating hello directory structure, as described in hello preceding section, "Application package file structure."</span></span>

### <a name="add-hello-applications-code-and-configuration-files"></a><span data-ttu-id="cced8-194">Přidání kódu a konfigurační soubory aplikace hello</span><span class="sxs-lookup"><span data-stu-id="cced8-194">Add hello application's code and configuration files</span></span>
<span data-ttu-id="cced8-195">Po vytvoření hello adresářovou strukturu, můžete přidat aplikace hello kódu a konfigurační soubory v hello kódu a konfigurace adresářů.</span><span class="sxs-lookup"><span data-stu-id="cced8-195">After you have created hello directory structure, you can add hello application's code and configuration files under hello code and config directories.</span></span> <span data-ttu-id="cced8-196">Můžete také vytvořit další adresáře nebo podadresáře v hello kódu nebo konfiguračního adresáře.</span><span class="sxs-lookup"><span data-stu-id="cced8-196">You can also create additional directories or subdirectories under hello code or config directories.</span></span>

<span data-ttu-id="cced8-197">Service Fabric nemá `xcopy` hello obsahu z hello kořenového adresáře aplikace, takže není k dispozici žádné předdefinované struktura toouse než vytváření dva hlavní adresáře, kód a nastavení.</span><span class="sxs-lookup"><span data-stu-id="cced8-197">Service Fabric does an `xcopy` of hello content of hello application root directory, so there is no predefined structure toouse other than creating two top directories, code and settings.</span></span> <span data-ttu-id="cced8-198">(Můžete si vybrat odlišné názvy podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="cced8-198">(You can pick different names if you want.</span></span> <span data-ttu-id="cced8-199">Další podrobnosti najdete v další části hello.)</span><span class="sxs-lookup"><span data-stu-id="cced8-199">More details are in hello next section.)</span></span>

> [!NOTE]
> <span data-ttu-id="cced8-200">Ujistěte se, jestli jste zahrnuli všechny soubory hello a závislosti, které hello potřebám aplikace.</span><span class="sxs-lookup"><span data-stu-id="cced8-200">Make sure that you include all hello files and dependencies that hello application needs.</span></span> <span data-ttu-id="cced8-201">Service Fabric zkopíruje hello obsahu balíčku aplikace hello na všech uzlech v clusteru hello kde hello aplikační služby jsou toobe probíhající nasazení.</span><span class="sxs-lookup"><span data-stu-id="cced8-201">Service Fabric copies hello content of hello application package on all nodes in hello cluster where hello application's services are going toobe deployed.</span></span> <span data-ttu-id="cced8-202">Hello balíček měl obsahovat všechny hello kódu, že aplikace hello musí toorun.</span><span class="sxs-lookup"><span data-stu-id="cced8-202">hello package should contain all hello code that hello application needs toorun.</span></span> <span data-ttu-id="cced8-203">Nepředpokládejte, že hello závislosti jsou již nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="cced8-203">Do not assume that hello dependencies are already installed.</span></span>
>
>

### <a name="edit-hello-service-manifest-file"></a><span data-ttu-id="cced8-204">Upravit soubor manifestu služby hello</span><span class="sxs-lookup"><span data-stu-id="cced8-204">Edit hello service manifest file</span></span>
<span data-ttu-id="cced8-205">dalším krokem Hello je tooedit hello služby souboru manifestu tooinclude hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="cced8-205">hello next step is tooedit hello service manifest file tooinclude hello following information:</span></span>

* <span data-ttu-id="cced8-206">Hello název typu služby hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-206">hello name of hello service type.</span></span> <span data-ttu-id="cced8-207">Toto je ID, že Service Fabric používá tooidentify služby.</span><span class="sxs-lookup"><span data-stu-id="cced8-207">This is an ID that Service Fabric uses tooidentify a service.</span></span>
* <span data-ttu-id="cced8-208">Hello příkaz toouse toolaunch hello aplikace (ExeHost).</span><span class="sxs-lookup"><span data-stu-id="cced8-208">hello command toouse toolaunch hello application (ExeHost).</span></span>
* <span data-ttu-id="cced8-209">Žádný skript, který potřebuje toobe spustit tooset si aplikace hello (SetupEntrypoint).</span><span class="sxs-lookup"><span data-stu-id="cced8-209">Any script that needs toobe run tooset up hello application (SetupEntrypoint).</span></span>

<span data-ttu-id="cced8-210">Hello tady je příklad `ServiceManifest.xml` souboru:</span><span class="sxs-lookup"><span data-stu-id="cced8-210">hello following is an example of a `ServiceManifest.xml` file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

<span data-ttu-id="cced8-211">Následující části Hello projít různé části hello souboru, je nutné, aby tooupdate hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-211">hello following sections go over hello different parts of hello file that you need tooupdate.</span></span>

#### <a name="update-servicetypes"></a><span data-ttu-id="cced8-212">Aktualizace ServiceTypes</span><span class="sxs-lookup"><span data-stu-id="cced8-212">Update ServiceTypes</span></span>
```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

* <span data-ttu-id="cced8-213">Můžete vybrat libovolný název, který chcete použít pro `ServiceTypeName`.</span><span class="sxs-lookup"><span data-stu-id="cced8-213">You can pick any name that you want for `ServiceTypeName`.</span></span> <span data-ttu-id="cced8-214">Hodnota Hello se používá v hello `ApplicationManifest.xml` tooidentify hello služba souborů.</span><span class="sxs-lookup"><span data-stu-id="cced8-214">hello value is used in hello `ApplicationManifest.xml` file tooidentify hello service.</span></span>
* <span data-ttu-id="cced8-215">Zadejte `UseImplicitHost="true"`.</span><span class="sxs-lookup"><span data-stu-id="cced8-215">Specify `UseImplicitHost="true"`.</span></span> <span data-ttu-id="cced8-216">Tento atribut informuje Service Fabric, že služba hello je založena na samostatná aplikace, takže všechny Service Fabric musí toodo toolaunch ji jako proces a sledovat jeho stav.</span><span class="sxs-lookup"><span data-stu-id="cced8-216">This attribute tells Service Fabric that hello service is based on a self-contained app, so all Service Fabric needs toodo is toolaunch it as a process and monitor its health.</span></span>

#### <a name="update-codepackage"></a><span data-ttu-id="cced8-217">CodePackage aktualizace</span><span class="sxs-lookup"><span data-stu-id="cced8-217">Update CodePackage</span></span>
<span data-ttu-id="cced8-218">Hello CodePackage element určuje umístění hello (a verzi) kódu služby hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-218">hello CodePackage element specifies hello location (and version) of hello service's code.</span></span>

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

<span data-ttu-id="cced8-219">Hello `Name` element je použité toospecify hello název adresáře hello v balíčku aplikace hello, který obsahuje kódu služby hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-219">hello `Name` element is used toospecify hello name of hello directory in hello application package that contains hello service's code.</span></span> <span data-ttu-id="cced8-220">`CodePackage`má také hello `version` atribut.</span><span class="sxs-lookup"><span data-stu-id="cced8-220">`CodePackage` also has hello `version` attribute.</span></span> <span data-ttu-id="cced8-221">To může být použité toospecify hello verze hello kódu a taky potenciálně lze používat kódu služby hello tooupgrade pomocí infrastruktury pro správu životního cyklu aplikace hello v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="cced8-221">This can be used toospecify hello version of hello code, and can also potentially be used tooupgrade hello service's code by using hello application lifecycle management infrastructure in Service Fabric.</span></span>

#### <a name="optional-update-setupentrypoint"></a><span data-ttu-id="cced8-222">Volitelné: Aktualizace SetupEntrypoint</span><span class="sxs-lookup"><span data-stu-id="cced8-222">Optional: Update SetupEntrypoint</span></span>
```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
<span data-ttu-id="cced8-223">Hello SetupEntryPoint element je použité toospecify spustitelný soubor nebo dávkového souboru, který se má provést před spuštění kódu služby hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-223">hello SetupEntryPoint element is used toospecify any executable or batch file that should be executed before hello service's code is launched.</span></span> <span data-ttu-id="cced8-224">Je volitelný krok, takže nemusí toobe zahrnuty, pokud neexistuje žádné inicializace požadována.</span><span class="sxs-lookup"><span data-stu-id="cced8-224">It is an optional step, so it does not need toobe included if there is no initialization required.</span></span> <span data-ttu-id="cced8-225">Hello SetupEntryPoint se spustí pokaždé, když hello restartována.</span><span class="sxs-lookup"><span data-stu-id="cced8-225">hello SetupEntryPoint is executed every time hello service is restarted.</span></span>

<span data-ttu-id="cced8-226">Se jenom jeden SetupEntryPoint, takže skripty instalace potřebovat toobe seskupené do jednoho dávkový soubor, pokud instalace aplikace hello vyžaduje několik skriptů.</span><span class="sxs-lookup"><span data-stu-id="cced8-226">There is only one SetupEntryPoint, so setup scripts need toobe grouped in a single batch file if hello application's setup requires multiple scripts.</span></span> <span data-ttu-id="cced8-227">Hello SetupEntryPoint může spustit libovolný typ souboru: spustitelné soubory, dávkové soubory a rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cced8-227">hello SetupEntryPoint can execute any type of file: executable files, batch files, and PowerShell cmdlets.</span></span> <span data-ttu-id="cced8-228">Další podrobnosti najdete v tématu [konfigurace SetupEntryPoint](service-fabric-application-runas-security.md).</span><span class="sxs-lookup"><span data-stu-id="cced8-228">For more details, see [Configure SetupEntryPoint](service-fabric-application-runas-security.md).</span></span>

<span data-ttu-id="cced8-229">V předchozím příkladu hello, hello SetupEntryPoint běží dávky soubor s názvem `LaunchConfig.cmd` který je umístěn v hello `scripts` podadresáři adresář kódu hello (za předpokladu, že hello WorkingFolder element je nastaven tooCodeBase).</span><span class="sxs-lookup"><span data-stu-id="cced8-229">In hello preceding example, hello SetupEntryPoint runs a batch file called `LaunchConfig.cmd` that is located in hello `scripts` subdirectory of hello code directory (assuming hello WorkingFolder element is set tooCodeBase).</span></span>

#### <a name="update-entrypoint"></a><span data-ttu-id="cced8-230">EntryPoint aktualizace</span><span class="sxs-lookup"><span data-stu-id="cced8-230">Update EntryPoint</span></span>
```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

<span data-ttu-id="cced8-231">Hello `EntryPoint` element v souboru manifestu služby hello je použité toospecify jak toolaunch hello služby.</span><span class="sxs-lookup"><span data-stu-id="cced8-231">hello `EntryPoint` element in hello service manifest file is used toospecify how toolaunch hello service.</span></span> <span data-ttu-id="cced8-232">Hello `ExeHost` element určuje hello spustitelného souboru (a argumenty) který by měl být použit toolaunch hello služby.</span><span class="sxs-lookup"><span data-stu-id="cced8-232">hello `ExeHost` element specifies hello executable (and arguments) that should be used toolaunch hello service.</span></span>

* <span data-ttu-id="cced8-233">`Program`Určuje název hello hello spustitelný soubor, který by měl spustit službu hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-233">`Program` specifies hello name of hello executable that should start hello service.</span></span>
* <span data-ttu-id="cced8-234">`Arguments`Určuje hello argumenty, které mají být předány toohello spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="cced8-234">`Arguments` specifies hello arguments that should be passed toohello executable.</span></span> <span data-ttu-id="cced8-235">Může být seznam parametrů s argumenty.</span><span class="sxs-lookup"><span data-stu-id="cced8-235">It can be a list of parameters with arguments.</span></span>
* <span data-ttu-id="cced8-236">`WorkingFolder`Určuje pracovní adresář hello hello procesu, který se bude toobe spuštěna.</span><span class="sxs-lookup"><span data-stu-id="cced8-236">`WorkingFolder` specifies hello working directory for hello process that is going toobe started.</span></span> <span data-ttu-id="cced8-237">Můžete určit tří hodnot:</span><span class="sxs-lookup"><span data-stu-id="cced8-237">You can specify three values:</span></span>
  * <span data-ttu-id="cced8-238">`CodeBase`Určuje, že pracovní adresář hello přechází toobe nastavit adresář toohello kódu v balíčku aplikace hello (`Code` adresář v hello předcházející strukturu souborů).</span><span class="sxs-lookup"><span data-stu-id="cced8-238">`CodeBase` specifies that hello working directory is going toobe set toohello code directory in hello application package (`Code` directory in hello preceding file structure).</span></span>
  * <span data-ttu-id="cced8-239">`CodePackage`Určuje, že pracovní adresář hello přechází toobe nastavit toohello kořenové balíčku aplikace hello (`GuestService1Pkg` v hello předcházející strukturu souborů).</span><span class="sxs-lookup"><span data-stu-id="cced8-239">`CodePackage` specifies that hello working directory is going toobe set toohello root of hello application package    (`GuestService1Pkg` in hello preceding file structure).</span></span>
    * <span data-ttu-id="cced8-240">`Work`Určuje, že hello soubory jsou umístěny v podadresáři volat pracovní.</span><span class="sxs-lookup"><span data-stu-id="cced8-240">`Work` specifies that hello files are placed in a subdirectory called work.</span></span>

<span data-ttu-id="cced8-241">Hello WorkingFolder je užitečné tooset hello správné pracovní adresář tak, aby relativní cesty mohou být využívána buď hello aplikace nebo inicializace skripty.</span><span class="sxs-lookup"><span data-stu-id="cced8-241">hello WorkingFolder is useful tooset hello correct working directory so that relative paths can be used by either hello application or initialization scripts.</span></span>

#### <a name="update-endpoints-and-register-with-naming-service-for-communication"></a><span data-ttu-id="cced8-242">Aktualizovat koncové body a zaregistrovat u služby DNS pro komunikaci</span><span class="sxs-lookup"><span data-stu-id="cced8-242">Update Endpoints and register with Naming Service for communication</span></span>
```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
<span data-ttu-id="cced8-243">V předchozím příkladu hello, hello `Endpoint` element určuje hello koncových bodů, které může aplikace hello naslouchat.</span><span class="sxs-lookup"><span data-stu-id="cced8-243">In hello preceding example, hello `Endpoint` element specifies hello endpoints that hello application can listen on.</span></span> <span data-ttu-id="cced8-244">V tomto příkladu aplikace Node.js hello naslouchá na protokolu http na portu 3000.</span><span class="sxs-lookup"><span data-stu-id="cced8-244">In this example, hello Node.js application listens on http on port 3000.</span></span>

<span data-ttu-id="cced8-245">Kromě toho můžete pokládat Service Fabric toopublish toohello tento koncový bod služby pojmenování tak další služby můžete zjistit hello koncový bod adresy toothis služby.</span><span class="sxs-lookup"><span data-stu-id="cced8-245">Furthermore you can ask Service Fabric toopublish this endpoint toohello Naming Service so other services can discover hello endpoint address toothis service.</span></span> <span data-ttu-id="cced8-246">To vám umožní mít toocommunicate toobe mezi službami, které jsou hostované spustitelné soubory.</span><span class="sxs-lookup"><span data-stu-id="cced8-246">This enables you toobe able toocommunicate between services that are guest executables.</span></span>
<span data-ttu-id="cced8-247">Hello adresa publikované koncového bodu má hello podobu `UriScheme://IPAddressOrFQDN:Port/PathSuffix`.</span><span class="sxs-lookup"><span data-stu-id="cced8-247">hello published endpoint address is of hello form `UriScheme://IPAddressOrFQDN:Port/PathSuffix`.</span></span> <span data-ttu-id="cced8-248">`UriScheme`a `PathSuffix` jsou volitelných atributů.</span><span class="sxs-lookup"><span data-stu-id="cced8-248">`UriScheme` and `PathSuffix` are optional attributes.</span></span> <span data-ttu-id="cced8-249">`IPAddressOrFQDN`je hello IP adresu nebo plně kvalifikovaný název domény tohoto spustitelného souboru získá umístěny na uzlu hello a se vypočítává za vás.</span><span class="sxs-lookup"><span data-stu-id="cced8-249">`IPAddressOrFQDN` is hello IP address or fully qualified domain name of hello node this executable gets placed on, and it is calculated for you.</span></span>

<span data-ttu-id="cced8-250">V hello následující příklad, jednou hello služby je nasazen, v Service Fabric Explorer zobrazí podobné koncový bod příliš`http://10.1.4.92:3000/myapp/` publikována pro instance služby hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-250">In hello following example, once hello service is deployed, in Service Fabric Explorer you see an endpoint similar too`http://10.1.4.92:3000/myapp/` published for hello service instance.</span></span> <span data-ttu-id="cced8-251">Nebo pokud se jedná se o místní počítač, zobrazí `http://localhost:3000/myapp/`.</span><span class="sxs-lookup"><span data-stu-id="cced8-251">Or if this is a local machine, you see `http://localhost:3000/myapp/`.</span></span>

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
<span data-ttu-id="cced8-252">Můžete použít tyto adresy s [reverse proxy](service-fabric-reverseproxy.md) toocommunicate mezi službami.</span><span class="sxs-lookup"><span data-stu-id="cced8-252">You can use these addresses with [reverse proxy](service-fabric-reverseproxy.md) toocommunicate between services.</span></span>

### <a name="edit-hello-application-manifest-file"></a><span data-ttu-id="cced8-253">Upravte soubor manifestu aplikace hello</span><span class="sxs-lookup"><span data-stu-id="cced8-253">Edit hello application manifest file</span></span>
<span data-ttu-id="cced8-254">Po konfiguraci hello `Servicemanifest.xml` souboru, je nutné toomake některé změny toohello `ApplicationManifest.xml` souboru tooensure, který hello správné, používají typ služby a název.</span><span class="sxs-lookup"><span data-stu-id="cced8-254">Once you have configured hello `Servicemanifest.xml` file, you need toomake some changes toohello `ApplicationManifest.xml` file tooensure that hello correct service type and name are used.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a><span data-ttu-id="cced8-255">ServiceManifestImport</span><span class="sxs-lookup"><span data-stu-id="cced8-255">ServiceManifestImport</span></span>
<span data-ttu-id="cced8-256">V hello `ServiceManifestImport` elementu, můžete zadat jednu nebo více služeb, které chcete tooinclude v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-256">In hello `ServiceManifestImport` element, you can specify one or more services that you want tooinclude in hello app.</span></span> <span data-ttu-id="cced8-257">Služby jsou odkazovány pomocí `ServiceManifestName`, která určuje název hello hello adresáře, kde hello `ServiceManifest.xml` se nachází soubor.</span><span class="sxs-lookup"><span data-stu-id="cced8-257">Services are referenced with `ServiceManifestName`, which specifies hello name of hello directory where hello `ServiceManifest.xml` file is located.</span></span>

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a><span data-ttu-id="cced8-258">Nastavení protokolování</span><span class="sxs-lookup"><span data-stu-id="cced8-258">Set up logging</span></span>
<span data-ttu-id="cced8-259">Pro hosta spustitelné soubory je užitečné toobe možné toosee konzoly protokoly toofind out-li zobrazit všechny chyby hello aplikace a konfigurační skripty.</span><span class="sxs-lookup"><span data-stu-id="cced8-259">For guest executables, it is useful toobe able toosee console logs toofind out if hello application and configuration scripts show any errors.</span></span>
<span data-ttu-id="cced8-260">Přesměrování konzoly se dá nakonfigurovat v hello `ServiceManifest.xml` soubor pomocí hello `ConsoleRedirection` elementu.</span><span class="sxs-lookup"><span data-stu-id="cced8-260">Console redirection can be configured in hello `ServiceManifest.xml` file using hello `ConsoleRedirection` element.</span></span>

> [!WARNING]
> <span data-ttu-id="cced8-261">Nikdy nepoužívejte zásad přesměrování konzoly hello v aplikaci, která je nasazena v produkčním prostředí, protože to může mít vliv hello aplikace převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="cced8-261">Never use hello console redirection policy in an application that is deployed in production because this can affect hello application failover.</span></span> <span data-ttu-id="cced8-262">*Pouze* používejte pro místní vývoj a účely ladění.</span><span class="sxs-lookup"><span data-stu-id="cced8-262">*Only* use this for local development and debugging purposes.</span></span>  
>
>

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

<span data-ttu-id="cced8-263">`ConsoleRedirection`lze použít tooredirect konzoly výstupního (stdout a stderr) tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="cced8-263">`ConsoleRedirection` can be used tooredirect console output (both stdout and stderr) tooa working directory.</span></span> <span data-ttu-id="cced8-264">To poskytuje tooverify hello možnost, že nejsou žádné chyby během hello instalaci nebo spuštění aplikace hello v clusteru Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-264">This provides hello ability tooverify that there are no errors during hello setup or execution of hello application in hello Service Fabric cluster.</span></span>

<span data-ttu-id="cced8-265">`FileRetentionCount`Určuje, kolik souborů se ukládají do hello pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="cced8-265">`FileRetentionCount` determines how many files are saved in hello working directory.</span></span> <span data-ttu-id="cced8-266">Hodnota 5, například znamená, že hello soubory protokolu pro předchozí pět spuštěních hello se ukládají v hello pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="cced8-266">A value of 5, for example, means that hello log files for hello previous five executions are stored in hello working directory.</span></span>

<span data-ttu-id="cced8-267">`FileMaxSizeInKb`Určuje maximální velikost hello hello protokolových souborů.</span><span class="sxs-lookup"><span data-stu-id="cced8-267">`FileMaxSizeInKb` specifies hello maximum size of hello log files.</span></span>

<span data-ttu-id="cced8-268">Ukládání souborů protokolu v jednom z hello služby pracovních adresářích.</span><span class="sxs-lookup"><span data-stu-id="cced8-268">Log files are saved in one of hello service's working directories.</span></span> <span data-ttu-id="cced8-269">toodetermine, kde jsou umístěny, hello soubory pomocí Service Fabric Explorer toodetermine služby hello uzlu, která běží na a pracovní adresář, kterému je používán.</span><span class="sxs-lookup"><span data-stu-id="cced8-269">toodetermine where hello files are located, use Service Fabric Explorer toodetermine which node hello service is running on, and which working directory is being used.</span></span> <span data-ttu-id="cced8-270">Tento proces je popsaná později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="cced8-270">This process is covered later in this article.</span></span>

## <a name="deployment"></a><span data-ttu-id="cced8-271">Nasazení</span><span class="sxs-lookup"><span data-stu-id="cced8-271">Deployment</span></span>
<span data-ttu-id="cced8-272">posledním krokem Hello je příliš[nasazení aplikace](service-fabric-deploy-remove-applications.md).</span><span class="sxs-lookup"><span data-stu-id="cced8-272">hello last step is too[deploy your application](service-fabric-deploy-remove-applications.md).</span></span> <span data-ttu-id="cced8-273">Dobrý den zobrazí skript prostředí PowerShell následující jak toodeploy vaší aplikace toohello místního vývojového clusteru a spusťte novou službu Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="cced8-273">hello following PowerShell script shows how toodeploy your application toohello local development cluster, and start a new Service Fabric service.</span></span>

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```

>[!TIP]
> <span data-ttu-id="cced8-274">[Komprimovat hello balíček](service-fabric-package-apps.md#compress-a-package) před kopírováním toohello úložiště bitové kopie, pokud balíček hello je velký nebo má mnoho souborů.</span><span class="sxs-lookup"><span data-stu-id="cced8-274">[Compress hello package](service-fabric-package-apps.md#compress-a-package) before copying toohello image store if hello package is large or has many files.</span></span> <span data-ttu-id="cced8-275">Další informace [zde](service-fabric-deploy-remove-applications.md#upload-the-application-package).</span><span class="sxs-lookup"><span data-stu-id="cced8-275">Read more [here](service-fabric-deploy-remove-applications.md#upload-the-application-package).</span></span>
>

<span data-ttu-id="cced8-276">Služba Service Fabric se dá nasadit v různých "konfigurace".</span><span class="sxs-lookup"><span data-stu-id="cced8-276">A Service Fabric service can be deployed in various "configurations."</span></span> <span data-ttu-id="cced8-277">Například se můžete nasadit jako jeden nebo více instancí, nebo se můžete nasadit tak, že existuje jedna instance služby hello v každém uzlu clusteru Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-277">For example, it can be deployed as single or multiple instances, or it can be deployed in such a way that there is one instance of hello service on each node of hello Service Fabric cluster.</span></span>

<span data-ttu-id="cced8-278">Hello `InstanceCount` parametr hello `New-ServiceFabricService` rutina je použité toospecify kolik instancí služby hello musí být spuštěna v clusteru Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-278">hello `InstanceCount` parameter of hello `New-ServiceFabricService` cmdlet is used toospecify how many instances of hello service should be launched in hello Service Fabric cluster.</span></span> <span data-ttu-id="cced8-279">Můžete nastavit hello `InstanceCount` hodnotu, v závislosti na typu hello aplikace, kterou nasazujete.</span><span class="sxs-lookup"><span data-stu-id="cced8-279">You can set hello `InstanceCount` value, depending on hello type of application that you are deploying.</span></span> <span data-ttu-id="cced8-280">Hello dva nejběžnější scénáře jsou:</span><span class="sxs-lookup"><span data-stu-id="cced8-280">hello two most common scenarios are:</span></span>

* <span data-ttu-id="cced8-281">`InstanceCount = "1"`.</span><span class="sxs-lookup"><span data-stu-id="cced8-281">`InstanceCount = "1"`.</span></span> <span data-ttu-id="cced8-282">V takovém případě pouze jednu instanci služby hello je nasazen v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-282">In this case, only one instance of hello service is deployed in hello cluster.</span></span> <span data-ttu-id="cced8-283">Scheduler Service Fabric určuje služby, která uzlu hello přechází toobe nasazené na.</span><span class="sxs-lookup"><span data-stu-id="cced8-283">Service Fabric's scheduler determines which node hello service is going toobe deployed on.</span></span>
* <span data-ttu-id="cced8-284">`InstanceCount ="-1"`.</span><span class="sxs-lookup"><span data-stu-id="cced8-284">`InstanceCount ="-1"`.</span></span> <span data-ttu-id="cced8-285">V takovém případě jednu instanci služby hello je nasazen na každý uzel v clusteru Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-285">In this case, one instance of hello service is deployed on every node in hello Service Fabric cluster.</span></span> <span data-ttu-id="cced8-286">výsledek Hello má jeden (a pouze jeden) instanci hello služby pro každý uzel v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-286">hello result is having one (and only one) instance of hello service for each node in hello cluster.</span></span>

<span data-ttu-id="cced8-287">Toto je užitečné konfiguraci pro front-endu aplikace (například koncový bod REST), protože klientské aplikace potřebují příliš "připojit" tooany hello uzlů v hello koncový bod hello toouse clusteru.</span><span class="sxs-lookup"><span data-stu-id="cced8-287">This is a useful configuration for front-end applications (for example, a REST endpoint), because client applications need too"connect" tooany of hello nodes in hello cluster toouse hello endpoint.</span></span> <span data-ttu-id="cced8-288">Tato konfigurace mohou sloužit také při, například všechny uzly clusteru Service Fabric hello jsou připojené tooa nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="cced8-288">This configuration can also be used when, for example, all nodes of hello Service Fabric cluster are connected tooa load balancer.</span></span> <span data-ttu-id="cced8-289">Přenosy klienta lze následně distribuovat mezi hello služba, která běží na všech uzlech v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-289">Client traffic can then be distributed across hello service that is running on all nodes in hello cluster.</span></span>

## <a name="check-your-running-application"></a><span data-ttu-id="cced8-290">Zkontrolujte spuštěné aplikace</span><span class="sxs-lookup"><span data-stu-id="cced8-290">Check your running application</span></span>
<span data-ttu-id="cced8-291">V Service Fabric Exploreru Identifikujte hello uzlu, kde je spuštěna služba hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-291">In Service Fabric Explorer, identify hello node where hello service is running.</span></span> <span data-ttu-id="cced8-292">V tomto příkladu je spuštěna na Uzel1:</span><span class="sxs-lookup"><span data-stu-id="cced8-292">In this example, it runs on Node1:</span></span>

![Uzel, na kterém je spuštěna služba](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

<span data-ttu-id="cced8-294">Pokud přejdete toohello uzlu a procházet toohello aplikace, zobrazí hello uzlu nezbytné informace, včetně jeho umístění na disku.</span><span class="sxs-lookup"><span data-stu-id="cced8-294">If you navigate toohello node and browse toohello application, you see hello essential node information, including its location on disk.</span></span>

![Umístění na disku](./media/service-fabric-deploy-existing-app/locationondisk2.png)

<span data-ttu-id="cced8-296">Pokud adresář toohello pomocí Průzkumníka serveru, najdete hello pracovní adresář a složku protokolu hello služby, jak ukazuje následující snímek obrazovky hello:</span><span class="sxs-lookup"><span data-stu-id="cced8-296">If you browse toohello directory by using Server Explorer, you can find hello working directory and hello service's log folder, as shown in hello following screenshot:</span></span> 

![Umístění protokolu](./media/service-fabric-deploy-existing-app/loglocation.png)

## <a name="next-steps"></a><span data-ttu-id="cced8-298">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cced8-298">Next steps</span></span>
<span data-ttu-id="cced8-299">V tomto článku jste se naučili jak toopackage spustitelný soubor hosta a nasaďte ho tooService prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="cced8-299">In this article, you have learned how toopackage a guest executable and deploy it tooService Fabric.</span></span> <span data-ttu-id="cced8-300">Viz následující články pro související informace a úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="cced8-300">See hello following articles for related information and tasks.</span></span>

* <span data-ttu-id="cced8-301">[Ukázka pro balení a nasazení spustitelný soubor hosta](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), včetně toohello odkaz předběžné verze nástroje balení hello</span><span class="sxs-lookup"><span data-stu-id="cced8-301">[Sample for packaging and deploying a guest executable](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), including a link toohello prerelease of hello packaging tool</span></span>
* [<span data-ttu-id="cced8-302">Ukázka dvěma hosta spustitelné soubory (C# a nodejs) komunikaci přes hello pojmenování službu pomocí REST</span><span class="sxs-lookup"><span data-stu-id="cced8-302">Sample of two guest executables (C# and nodejs) communicating via hello Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
* [<span data-ttu-id="cced8-303">Nasazení několika hostujících spustitelných souborů</span><span class="sxs-lookup"><span data-stu-id="cced8-303">Deploy multiple guest executables</span></span>](service-fabric-deploy-multiple-apps.md)
* [<span data-ttu-id="cced8-304">Vytvoření první aplikace Service Fabric pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cced8-304">Create your first Service Fabric application using Visual Studio</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
