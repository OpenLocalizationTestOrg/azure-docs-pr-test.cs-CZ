---
title: "Nasadit existující spustitelný soubor do Azure Service Fabric | Microsoft Docs"
description: "Návod o tom, jak balíček stávající aplikace jako Host spustitelného souboru, aby ji můžete nasadit na cluster Service Fabric"
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
ms.openlocfilehash: a1db3dda674ffe43587333d88f3816549af3019c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-guest-executable-to-service-fabric"></a><span data-ttu-id="d0c42-103">Nasazení do Service Fabric Host spustitelný soubor</span><span class="sxs-lookup"><span data-stu-id="d0c42-103">Deploy a guest executable to Service Fabric</span></span>
<span data-ttu-id="d0c42-104">Jakýkoli typ kódu, třeba Node.js, Java nebo C++ v Azure Service Fabric můžete spustit jako služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-104">You can run any type of code, such as Node.js, Java, or C++ in Azure Service Fabric as a service.</span></span> <span data-ttu-id="d0c42-105">Service Fabric odkazuje na těchto typů služeb jako hosta spustitelné soubory.</span><span class="sxs-lookup"><span data-stu-id="d0c42-105">Service Fabric refers to these types of services as guest executables.</span></span>

<span data-ttu-id="d0c42-106">Spustitelné soubory hosta budou vyhodnocené jako bezstavové služby Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d0c42-106">Guest executables are treated by Service Fabric like stateless services.</span></span> <span data-ttu-id="d0c42-107">V důsledku toho jsou umístěny na uzlech v clusteru, na základě dostupnosti a další metriky.</span><span class="sxs-lookup"><span data-stu-id="d0c42-107">As a result, they are placed on nodes in a cluster, based on availability and other metrics.</span></span> <span data-ttu-id="d0c42-108">Tento článek popisuje, jak pro zabalení a nasazení hosta spustitelný soubor do clusteru Service Fabric pomocí Visual Studio nebo nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d0c42-108">This article describes how to package and deploy a guest executable to a Service Fabric cluster, by using Visual Studio or a command-line utility.</span></span>

<span data-ttu-id="d0c42-109">V tomto článku jsme zahrnují kroky k balíčku hosta spustitelný soubor a nasadíte ho do Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d0c42-109">In this article, we cover the steps to package a guest executable and deploy it to Service Fabric.</span></span>  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a><span data-ttu-id="d0c42-110">Výhody spuštění spustitelného souboru v Service Fabric Host</span><span class="sxs-lookup"><span data-stu-id="d0c42-110">Benefits of running a guest executable in Service Fabric</span></span>
<span data-ttu-id="d0c42-111">Existuje několik výhod pro spuštění spustitelného souboru v clusteru Service Fabric Host:</span><span class="sxs-lookup"><span data-stu-id="d0c42-111">There are several advantages to running a guest executable in a Service Fabric cluster:</span></span>

* <span data-ttu-id="d0c42-112">Vysoká dostupnost.</span><span class="sxs-lookup"><span data-stu-id="d0c42-112">High availability.</span></span> <span data-ttu-id="d0c42-113">Aplikace, které běží v Service Fabric jsou vysoce dostupné.</span><span class="sxs-lookup"><span data-stu-id="d0c42-113">Applications that run in Service Fabric are made highly available.</span></span> <span data-ttu-id="d0c42-114">Service Fabric zajistí, že instance aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="d0c42-114">Service Fabric ensures that instances of an application are running.</span></span>
* <span data-ttu-id="d0c42-115">Sledování stavu.</span><span class="sxs-lookup"><span data-stu-id="d0c42-115">Health monitoring.</span></span> <span data-ttu-id="d0c42-116">Monitorování stavu Service Fabric zjišťuje, zda aplikace běží a poskytuje diagnostické informace, pokud dojde k selhání.</span><span class="sxs-lookup"><span data-stu-id="d0c42-116">Service Fabric health monitoring detects if an application is running, and provides diagnostic information if there is a failure.</span></span>   
* <span data-ttu-id="d0c42-117">Správa životního cyklu aplikací.</span><span class="sxs-lookup"><span data-stu-id="d0c42-117">Application lifecycle management.</span></span> <span data-ttu-id="d0c42-118">Kromě toho poskytuje upgrady bez výpadků, Service Fabric nabízí automatického vrácení zpět na předchozí verzi, pokud existuje událost stavu chybný hlášené během upgradu.</span><span class="sxs-lookup"><span data-stu-id="d0c42-118">Besides providing upgrades with no downtime, Service Fabric provides automatic rollback to the previous version if there is a bad health event reported during an upgrade.</span></span>    
* <span data-ttu-id="d0c42-119">Hustotou.</span><span class="sxs-lookup"><span data-stu-id="d0c42-119">Density.</span></span> <span data-ttu-id="d0c42-120">Více aplikací můžete spustit v clusteru, není potřeba pro každou aplikaci, aby běžela na svůj vlastní hardware.</span><span class="sxs-lookup"><span data-stu-id="d0c42-120">You can run multiple applications in a cluster, which eliminates the need for each application to run on its own hardware.</span></span>
* <span data-ttu-id="d0c42-121">Možnosti rozpoznání: Pomocí REST můžete volat službu Service Fabric Naming, kterou chcete najít další služby v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d0c42-121">Discoverability: Using REST you can call the Service Fabric Naming service to find other services in the cluster.</span></span> 

## <a name="samples"></a><span data-ttu-id="d0c42-122">Ukázky</span><span class="sxs-lookup"><span data-stu-id="d0c42-122">Samples</span></span>
* [<span data-ttu-id="d0c42-123">Ukázka pro balení a nasazení spustitelný soubor hosta</span><span class="sxs-lookup"><span data-stu-id="d0c42-123">Sample for packaging and deploying a guest executable</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="d0c42-124">Ukázka dvěma hosta spustitelné soubory (C# a nodejs) komunikaci přes službu Naming pomocí REST</span><span class="sxs-lookup"><span data-stu-id="d0c42-124">Sample of two guest executables (C# and nodejs) communicating via the Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a><span data-ttu-id="d0c42-125">Přehled aplikace a soubory manifestu služby</span><span class="sxs-lookup"><span data-stu-id="d0c42-125">Overview of application and service manifest files</span></span>
<span data-ttu-id="d0c42-126">Jako součást nasazení spustitelný soubor hosta, je užitečné Service Fabric balení a nasazení modelu pochopit, jak je popsáno v [aplikačního modelu](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="d0c42-126">As part of deploying a guest executable, it is useful to understand the Service Fabric packaging and deployment model as described in [application model](service-fabric-application-model.md).</span></span> <span data-ttu-id="d0c42-127">Balení modelu Service Fabric spoléhá na dva soubory XML: manifestů aplikace a služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-127">The Service Fabric packaging model relies on two XML files: the application and service manifests.</span></span> <span data-ttu-id="d0c42-128">Definice schématu pro ApplicationManifest.xml a ServiceManifest.xml soubory se instaluje s Service Fabric SDK do *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="d0c42-128">The schema definition for the ApplicationManifest.xml and ServiceManifest.xml files is installed with the Service Fabric SDK into *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

* <span data-ttu-id="d0c42-129">**Manifest aplikace** manifest aplikace se používá k popisu aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0c42-129">**Application manifest** The application manifest is used to describe the application.</span></span> <span data-ttu-id="d0c42-130">Zobrazí seznam služeb, které ji tvoří a musí být nasazené dalších parametrů, které slouží k určení jak jednu nebo více služeb, jako je počet instancí.</span><span class="sxs-lookup"><span data-stu-id="d0c42-130">It lists the services that compose it, and other parameters that are used to define how one or more services should be deployed, such as the number of instances.</span></span>

  <span data-ttu-id="d0c42-131">Aplikace v Service Fabric je jednotka nasazení a upgrade.</span><span class="sxs-lookup"><span data-stu-id="d0c42-131">In Service Fabric, an application is a unit of deployment and upgrade.</span></span> <span data-ttu-id="d0c42-132">Aplikace lze upgradovat jako na jednu jednotku, kde se spravují potenciální chyby a potenciální odvolání.</span><span class="sxs-lookup"><span data-stu-id="d0c42-132">An application can be upgraded as a single unit where potential failures and potential rollbacks are managed.</span></span> <span data-ttu-id="d0c42-133">Service Fabric zaručuje, že proces upgradu je buď úspěšné, nebo, pokud se upgrade nezdaří, nenechává aplikace v neznámý nebo nestabilním stavu.</span><span class="sxs-lookup"><span data-stu-id="d0c42-133">Service Fabric guarantees that the upgrade process is either successful, or, if the upgrade fails, does not leave the application in an unknown or unstable state.</span></span>
* <span data-ttu-id="d0c42-134">**Manifest služby** service manifest popisuje součásti služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-134">**Service manifest** The service manifest describes the components of a service.</span></span> <span data-ttu-id="d0c42-135">Obsahuje data, jako třeba název a typ služby a jeho kódu a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d0c42-135">It includes data, such as the name and type of service, and its code and configuration.</span></span> <span data-ttu-id="d0c42-136">Manifest služby zahrnuje také některé další parametry, které lze použít ke konfiguraci služby po jejím nasazení.</span><span class="sxs-lookup"><span data-stu-id="d0c42-136">The service manifest also includes some additional parameters that can be used to configure the service once it is deployed.</span></span>

## <a name="application-package-file-structure"></a><span data-ttu-id="d0c42-137">Struktura souboru balíčku aplikace</span><span class="sxs-lookup"><span data-stu-id="d0c42-137">Application package file structure</span></span>
<span data-ttu-id="d0c42-138">Pokud chcete nasadit aplikace do Service Fabric, by mělo vycházet aplikace předdefinované adresářovou strukturu.</span><span class="sxs-lookup"><span data-stu-id="d0c42-138">To deploy an application to Service Fabric, the application should follow a predefined directory structure.</span></span> <span data-ttu-id="d0c42-139">Následuje příklad této struktury.</span><span class="sxs-lookup"><span data-stu-id="d0c42-139">The following is an example of that structure.</span></span>

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

<span data-ttu-id="d0c42-140">ApplicationPackageRoot obsahuje ApplicationManifest.xml soubor, který definuje aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0c42-140">The ApplicationPackageRoot contains the ApplicationManifest.xml file that defines the application.</span></span> <span data-ttu-id="d0c42-141">Tak, aby obsahovala všechny artefakty, které služba vyžaduje, aby se používá podadresáři pro každou službu obsažené v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d0c42-141">A subdirectory for each service included in the application is used to contain all the artifacts that the service requires.</span></span> <span data-ttu-id="d0c42-142">Tyto podadresáře jsou ServiceManifest.xml a obvykle následující:</span><span class="sxs-lookup"><span data-stu-id="d0c42-142">These subdirectories are the ServiceManifest.xml and, typically, the following:</span></span>

* <span data-ttu-id="d0c42-143">*Kód*.</span><span class="sxs-lookup"><span data-stu-id="d0c42-143">*Code*.</span></span> <span data-ttu-id="d0c42-144">Tento adresář obsahuje kód služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-144">This directory contains the service code.</span></span>
* <span data-ttu-id="d0c42-145">*Konfigurace*.</span><span class="sxs-lookup"><span data-stu-id="d0c42-145">*Config*.</span></span> <span data-ttu-id="d0c42-146">Tento adresář obsahuje soubor souborech Settings.xml (a další soubory v případě potřeby), že služba přístup za běhu k načtení specifické nastavení.</span><span class="sxs-lookup"><span data-stu-id="d0c42-146">This directory contains a Settings.xml file (and other files if necessary) that the service can access at runtime to retrieve specific configuration settings.</span></span>
* <span data-ttu-id="d0c42-147">*Data*.</span><span class="sxs-lookup"><span data-stu-id="d0c42-147">*Data*.</span></span> <span data-ttu-id="d0c42-148">To je další adresář k uložení další místní data, která může být nutné službu.</span><span class="sxs-lookup"><span data-stu-id="d0c42-148">This is an additional directory to store additional local data that the service may need.</span></span> <span data-ttu-id="d0c42-149">Data slouží k ukládání pouze dočasných dat.</span><span class="sxs-lookup"><span data-stu-id="d0c42-149">Data should be used to store only ephemeral data.</span></span> <span data-ttu-id="d0c42-150">Service Fabric kopírování nebo replikovat změny do adresáře dat, pokud služba musí být přemístění (například během převzetí služeb při selhání).</span><span class="sxs-lookup"><span data-stu-id="d0c42-150">Service Fabric does not copy or replicate changes to the data directory if the service needs to be relocated (for example, during failover).</span></span>

> [!NOTE]
> <span data-ttu-id="d0c42-151">Není nutné vytvářet `config` a `data` adresářů, pokud je nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="d0c42-151">You don't have to create the `config` and `data` directories if you don't need them.</span></span>
>
>

## <a name="package-an-existing-executable"></a><span data-ttu-id="d0c42-152">Balíček existující spustitelný soubor</span><span class="sxs-lookup"><span data-stu-id="d0c42-152">Package an existing executable</span></span>
<span data-ttu-id="d0c42-153">Při balení spustitelný soubor hosta, můžete buď používat šablony projektů Visual Studio nebo [ručně vytvořit balíček aplikace](#manually).</span><span class="sxs-lookup"><span data-stu-id="d0c42-153">When packaging a guest executable, you can choose either to use a Visual Studio project template or to [create the application package manually](#manually).</span></span> <span data-ttu-id="d0c42-154">Pomocí sady Visual Studio, struktury balíček aplikace a soubory manifestu vytvářejí nové šablona projektu pro vás.</span><span class="sxs-lookup"><span data-stu-id="d0c42-154">Using Visual Studio, the application package structure and manifest files are created by the new project template for you.</span></span>

> [!TIP]
> <span data-ttu-id="d0c42-155">Nejjednodušší způsob, jak spustitelný soubor do služby Windows pro existující balíček je pomocí sady Visual Studio a v systému Linux používat Yeoman</span><span class="sxs-lookup"><span data-stu-id="d0c42-155">The easiest way to package an existing Windows executable into a service is to use Visual Studio and on Linux to use Yeoman</span></span>
>

## <a name="use-visual-studio-to-package-and-deploy-an-existing-executable"></a><span data-ttu-id="d0c42-156">Použijte sadu Visual Studio pro zabalení a nasazení existující spustitelný soubor</span><span class="sxs-lookup"><span data-stu-id="d0c42-156">Use Visual Studio to package and deploy an existing executable</span></span>
<span data-ttu-id="d0c42-157">Visual Studio poskytuje šablony služby Service Fabric vám pomůžou nasadit do clusteru Service Fabric Host spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="d0c42-157">Visual Studio provides a Service Fabric service template to help you deploy a guest executable to a Service Fabric cluster.</span></span>

1. <span data-ttu-id="d0c42-158">Zvolte **soubor** > **nový projekt**a vytvářet aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d0c42-158">Choose **File** > **New Project**, and create a Service Fabric application.</span></span>
2. <span data-ttu-id="d0c42-159">Zvolte **spustitelný soubor hosta** jako šablonu služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-159">Choose **Guest Executable** as the service template.</span></span>
3. <span data-ttu-id="d0c42-160">Klikněte na tlačítko **Procházet** vyberte složku s vaší spustitelný soubor a vyplňte zbytek parametrů k vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-160">Click **Browse** to select the folder with your executable and fill in the rest of the parameters to create the service.</span></span>
   * <span data-ttu-id="d0c42-161">*Kód balíčku chování*.</span><span class="sxs-lookup"><span data-stu-id="d0c42-161">*Code Package Behavior*.</span></span> <span data-ttu-id="d0c42-162">Může být nastaven na veškerý obsah složky zkopírujte do projektu Visual Studio, což je užitečné, pokud spustitelný soubor se nemění.</span><span class="sxs-lookup"><span data-stu-id="d0c42-162">Can be set to copy all the content of your folder to the Visual Studio Project, which is useful if the executable does not change.</span></span> <span data-ttu-id="d0c42-163">Pokud očekáváte, spustitelný soubor změnit a chcete mít možnost dynamicky vyzvednutí nových sestavení automaticky, můžete místo toho odkaz ke složce.</span><span class="sxs-lookup"><span data-stu-id="d0c42-163">If you expect the executable to change and want the ability to pick up new builds dynamically, you can choose to link to the folder instead.</span></span> <span data-ttu-id="d0c42-164">Propojené složek můžete použít při vytváření projektu aplikace v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d0c42-164">You can use linked folders when creating the application project in Visual Studio.</span></span> <span data-ttu-id="d0c42-165">Tato sestava odkazuje na umístění zdroje z v projektu, což vám umožní aktualizovat Host spustitelný soubor v jeho zdroj cíl.</span><span class="sxs-lookup"><span data-stu-id="d0c42-165">This links to the source location from within the project, making it possible for you to update the guest executable in its source destination.</span></span> <span data-ttu-id="d0c42-166">Tyto aktualizace se stanou součástí balíčku aplikace při sestavování.</span><span class="sxs-lookup"><span data-stu-id="d0c42-166">Those updates become part of the application package on build.</span></span>
   * <span data-ttu-id="d0c42-167">*Program* Určuje spustitelný soubor, který by měl být spuštěn pro spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-167">*Program* specifies the executable that should be run to start the service.</span></span>
   * <span data-ttu-id="d0c42-168">*Argumenty* Určuje argumenty, které by měla být předána spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="d0c42-168">*Arguments* specifies the arguments that should be passed to the executable.</span></span> <span data-ttu-id="d0c42-169">Může být seznam parametrů s argumenty.</span><span class="sxs-lookup"><span data-stu-id="d0c42-169">It can be a list of parameters with arguments.</span></span>
   * <span data-ttu-id="d0c42-170">*WorkingFolder* určuje pracovní adresář pro proces, který chcete spustit.</span><span class="sxs-lookup"><span data-stu-id="d0c42-170">*WorkingFolder* specifies the working directory for the process that is going to be started.</span></span> <span data-ttu-id="d0c42-171">Můžete určit tří hodnot:</span><span class="sxs-lookup"><span data-stu-id="d0c42-171">You can specify three values:</span></span>
     * <span data-ttu-id="d0c42-172">`CodeBase`Určuje, že pracovní adresář bude být nastavena na adresář kódu v balíčku aplikace (`Code` directory uvedené v předchozí strukturu souborů).</span><span class="sxs-lookup"><span data-stu-id="d0c42-172">`CodeBase` specifies that the working directory is going to be set to the code directory in the application package (`Code` directory shown in the preceding file structure).</span></span>
     * <span data-ttu-id="d0c42-173">`CodePackage`Určuje, že pracovní adresář bude se nastaví na kořen balíčku aplikace (`GuestService1Pkg` uvedené v předchozí strukturu souborů).</span><span class="sxs-lookup"><span data-stu-id="d0c42-173">`CodePackage` specifies that the working directory is going to be set to the root of the application package    (`GuestService1Pkg` shown in the preceding file structure).</span></span>
     * <span data-ttu-id="d0c42-174">`Work`Určuje, že soubory jsou umístěny v podadresáři volat pracovní.</span><span class="sxs-lookup"><span data-stu-id="d0c42-174">`Work` specifies that the files are placed in a subdirectory called work.</span></span>
4. <span data-ttu-id="d0c42-175">Zadejte název služby a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="d0c42-175">Give your service a name, and click **OK**.</span></span>
5. <span data-ttu-id="d0c42-176">Pokud vaše služba musí koncový bod pro komunikaci, můžete nyní přidat protokol, port a typ souboru ServiceManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="d0c42-176">If your service needs an endpoint for communication, you can now add the protocol, port, and type to the ServiceManifest.xml file.</span></span> <span data-ttu-id="d0c42-177">Například: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.</span><span class="sxs-lookup"><span data-stu-id="d0c42-177">For example: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.</span></span>
6. <span data-ttu-id="d0c42-178">Teď můžete použít balíček a publikovat akce s místním clusteru tak, že ladění řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d0c42-178">You can now use the package and publish action against your local cluster by debugging the solution in Visual Studio.</span></span> <span data-ttu-id="d0c42-179">Až bude připravený, můžete publikovat aplikaci do vzdáleného clusteru nebo řešení správy zdrojového kódu se změnami.</span><span class="sxs-lookup"><span data-stu-id="d0c42-179">When ready, you can publish the application to a remote cluster or check in the solution to source control.</span></span>
7. <span data-ttu-id="d0c42-180">Přejděte na konci tohoto článku chcete zjistit, jak chcete-li zobrazit hosta spustitelný soubor služby spuštěné v Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="d0c42-180">Go to the end of this article to see how to view your guest executable service running in Service Fabric Explorer.</span></span>

## <a name="use-yoeman-to-package-and-deploy-an-existing-executable-on-linux"></a><span data-ttu-id="d0c42-181">Použít Yoeman pro balíček a nasazení existující spustitelný soubor v systému Linux</span><span class="sxs-lookup"><span data-stu-id="d0c42-181">Use Yoeman to package and deploy an existing executable on Linux</span></span>

<span data-ttu-id="d0c42-182">Postup pro vytvoření a nasazení hosta spustitelného souboru v systému Linux je stejný jako nasazení aplikace csharp nebo java.</span><span class="sxs-lookup"><span data-stu-id="d0c42-182">The procedure for creating and deploying a guest executable on Linux is the same as deploying a csharp or java application.</span></span>

1. <span data-ttu-id="d0c42-183">V terminálu zadejte `yo azuresfguest`.</span><span class="sxs-lookup"><span data-stu-id="d0c42-183">In a terminal, type `yo azuresfguest`.</span></span>
2. <span data-ttu-id="d0c42-184">Pojmenujte svoji aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d0c42-184">Name your application.</span></span>
3. <span data-ttu-id="d0c42-185">Název služby a zadejte podrobnosti, včetně cesta ke spustitelnému souboru a parametry, které musí být volána s.</span><span class="sxs-lookup"><span data-stu-id="d0c42-185">Name your service, and provide the details including path of the executable and the parameters it must be invoked with.</span></span>

<span data-ttu-id="d0c42-186">Yeoman vytvoří balíček aplikace s příslušnou aplikaci a souborů manifestu spolu s instalaci a odinstalaci skripty.</span><span class="sxs-lookup"><span data-stu-id="d0c42-186">Yeoman creates an application package with the appropriate application and manifest files along with install and uninstall scripts.</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-an-existing-executable"></a><span data-ttu-id="d0c42-187">Ručně zabalení a nasazení existující spustitelný soubor</span><span class="sxs-lookup"><span data-stu-id="d0c42-187">Manually package and deploy an existing executable</span></span>
<span data-ttu-id="d0c42-188">Proces ručně balení spustitelný soubor hosta je založena na následujících obecných kroků:</span><span class="sxs-lookup"><span data-stu-id="d0c42-188">The process of manually packaging a guest executable is based on the following general steps:</span></span>

1. <span data-ttu-id="d0c42-189">Vytvořte strukturu adresáře balíčku.</span><span class="sxs-lookup"><span data-stu-id="d0c42-189">Create the package directory structure.</span></span>
2. <span data-ttu-id="d0c42-190">Přidejte kód a konfigurační soubory aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0c42-190">Add the application's code and configuration files.</span></span>
3. <span data-ttu-id="d0c42-191">Upravte soubor manifestu služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-191">Edit the service manifest file.</span></span>
4. <span data-ttu-id="d0c42-192">Upravte soubor manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0c42-192">Edit the application manifest file.</span></span>

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a><span data-ttu-id="d0c42-193">Vytvořit strukturu adresáře balíčku</span><span class="sxs-lookup"><span data-stu-id="d0c42-193">Create the package directory structure</span></span>
<span data-ttu-id="d0c42-194">Můžete spustit tak, že vytvoříte strukturu adresáře, jak je popsáno v předchozí části "Aplikace balíčku souboru struktura."</span><span class="sxs-lookup"><span data-stu-id="d0c42-194">You can start by creating the directory structure, as described in the preceding section, "Application package file structure."</span></span>

### <a name="add-the-applications-code-and-configuration-files"></a><span data-ttu-id="d0c42-195">Přidání kódu a konfigurační soubory aplikace</span><span class="sxs-lookup"><span data-stu-id="d0c42-195">Add the application's code and configuration files</span></span>
<span data-ttu-id="d0c42-196">Po vytvoření strukturu adresáře, můžete přidat kód aplikace a konfigurační soubory v adresáři kódu a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d0c42-196">After you have created the directory structure, you can add the application's code and configuration files under the code and config directories.</span></span> <span data-ttu-id="d0c42-197">Můžete také vytvořit další adresáře nebo podadresáře v adresáři kódu nebo konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d0c42-197">You can also create additional directories or subdirectories under the code or config directories.</span></span>

<span data-ttu-id="d0c42-198">Service Fabric nemá `xcopy` obsahu kořenového adresáře aplikace, takže není k dispozici žádné předdefinované struktura používat jiné než vytváření dva hlavní adresáře, kód a nastavení.</span><span class="sxs-lookup"><span data-stu-id="d0c42-198">Service Fabric does an `xcopy` of the content of the application root directory, so there is no predefined structure to use other than creating two top directories, code and settings.</span></span> <span data-ttu-id="d0c42-199">(Můžete si vybrat odlišné názvy podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-199">(You can pick different names if you want.</span></span> <span data-ttu-id="d0c42-200">Další podrobnosti najdete v další části.)</span><span class="sxs-lookup"><span data-stu-id="d0c42-200">More details are in the next section.)</span></span>

> [!NOTE]
> <span data-ttu-id="d0c42-201">Ujistěte se, jestli jste zahrnuli všechny soubory a závislosti, které aplikace potřebuje.</span><span class="sxs-lookup"><span data-stu-id="d0c42-201">Make sure that you include all the files and dependencies that the application needs.</span></span> <span data-ttu-id="d0c42-202">Service Fabric zkopíruje obsah balíčku aplikace na všech uzlech v clusteru, kde se chystáte služby aplikace nasadit.</span><span class="sxs-lookup"><span data-stu-id="d0c42-202">Service Fabric copies the content of the application package on all nodes in the cluster where the application's services are going to be deployed.</span></span> <span data-ttu-id="d0c42-203">Tento balíček měl obsahovat všechny kód, který aplikace je potřeba spustit.</span><span class="sxs-lookup"><span data-stu-id="d0c42-203">The package should contain all the code that the application needs to run.</span></span> <span data-ttu-id="d0c42-204">Nepředpokládejte, že závislosti jsou již nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="d0c42-204">Do not assume that the dependencies are already installed.</span></span>
>
>

### <a name="edit-the-service-manifest-file"></a><span data-ttu-id="d0c42-205">Upravit soubor manifestu služby</span><span class="sxs-lookup"><span data-stu-id="d0c42-205">Edit the service manifest file</span></span>
<span data-ttu-id="d0c42-206">Dalším krokem je upravit soubor manifestu služby zahrnout tyto informace:</span><span class="sxs-lookup"><span data-stu-id="d0c42-206">The next step is to edit the service manifest file to include the following information:</span></span>

* <span data-ttu-id="d0c42-207">Název typu služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-207">The name of the service type.</span></span> <span data-ttu-id="d0c42-208">Toto je ID, které Service Fabric používá k identifikaci služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-208">This is an ID that Service Fabric uses to identify a service.</span></span>
* <span data-ttu-id="d0c42-209">Příkaz sloužící ke spuštění aplikace (ExeHost).</span><span class="sxs-lookup"><span data-stu-id="d0c42-209">The command to use to launch the application (ExeHost).</span></span>
* <span data-ttu-id="d0c42-210">Žádný skript, který je potřeba spustit k nastavení aplikace (SetupEntrypoint).</span><span class="sxs-lookup"><span data-stu-id="d0c42-210">Any script that needs to be run to set up the application (SetupEntrypoint).</span></span>

<span data-ttu-id="d0c42-211">Tady je příklad `ServiceManifest.xml` souboru:</span><span class="sxs-lookup"><span data-stu-id="d0c42-211">The following is an example of a `ServiceManifest.xml` file:</span></span>

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

<span data-ttu-id="d0c42-212">V následujících částech projít různé části souboru, který je potřeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="d0c42-212">The following sections go over the different parts of the file that you need to update.</span></span>

#### <a name="update-servicetypes"></a><span data-ttu-id="d0c42-213">Aktualizace ServiceTypes</span><span class="sxs-lookup"><span data-stu-id="d0c42-213">Update ServiceTypes</span></span>
```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

* <span data-ttu-id="d0c42-214">Můžete vybrat libovolný název, který chcete použít pro `ServiceTypeName`.</span><span class="sxs-lookup"><span data-stu-id="d0c42-214">You can pick any name that you want for `ServiceTypeName`.</span></span> <span data-ttu-id="d0c42-215">Hodnota se používá v `ApplicationManifest.xml` souboru k identifikaci služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-215">The value is used in the `ApplicationManifest.xml` file to identify the service.</span></span>
* <span data-ttu-id="d0c42-216">Zadejte `UseImplicitHost="true"`.</span><span class="sxs-lookup"><span data-stu-id="d0c42-216">Specify `UseImplicitHost="true"`.</span></span> <span data-ttu-id="d0c42-217">Tento atribut informuje Service Fabric, že služba je založen na samostatná aplikace, tak, aby všechny musí Service Fabric se spustí jako proces a monitorovat jeho stav.</span><span class="sxs-lookup"><span data-stu-id="d0c42-217">This attribute tells Service Fabric that the service is based on a self-contained app, so all Service Fabric needs to do is to launch it as a process and monitor its health.</span></span>

#### <a name="update-codepackage"></a><span data-ttu-id="d0c42-218">CodePackage aktualizace</span><span class="sxs-lookup"><span data-stu-id="d0c42-218">Update CodePackage</span></span>
<span data-ttu-id="d0c42-219">CodePackage element určuje umístění (a verzi) kódu služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-219">The CodePackage element specifies the location (and version) of the service's code.</span></span>

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

<span data-ttu-id="d0c42-220">`Name` Element se používá k určení názvu adresáře v balíčku aplikace, která obsahuje kód služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-220">The `Name` element is used to specify the name of the directory in the application package that contains the service's code.</span></span> <span data-ttu-id="d0c42-221">`CodePackage`má také `version` atribut.</span><span class="sxs-lookup"><span data-stu-id="d0c42-221">`CodePackage` also has the `version` attribute.</span></span> <span data-ttu-id="d0c42-222">To lze použít k určení verze kódu a můžete také potenciálně použít k upgradu služby kódu s použitím infrastruktury pro správu životního cyklu aplikace v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d0c42-222">This can be used to specify the version of the code, and can also potentially be used to upgrade the service's code by using the application lifecycle management infrastructure in Service Fabric.</span></span>

#### <a name="optional-update-setupentrypoint"></a><span data-ttu-id="d0c42-223">Volitelné: Aktualizace SetupEntrypoint</span><span class="sxs-lookup"><span data-stu-id="d0c42-223">Optional: Update SetupEntrypoint</span></span>
```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
<span data-ttu-id="d0c42-224">SetupEntryPoint element slouží k určení spustitelného souboru nebo v dávkovém souboru, který se má provést před spuštění kódu služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-224">The SetupEntryPoint element is used to specify any executable or batch file that should be executed before the service's code is launched.</span></span> <span data-ttu-id="d0c42-225">Je volitelný krok, takže nemusí být zahrnuty, pokud není žádná inicializace požadována.</span><span class="sxs-lookup"><span data-stu-id="d0c42-225">It is an optional step, so it does not need to be included if there is no initialization required.</span></span> <span data-ttu-id="d0c42-226">SetupEntryPoint je proveden při každém restartování služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-226">The SetupEntryPoint is executed every time the service is restarted.</span></span>

<span data-ttu-id="d0c42-227">Se jenom jeden SetupEntryPoint, takže skripty instalace musí být seskupeny do jednoho dávkový soubor, pokud instalační program aplikace vyžaduje několik skriptů.</span><span class="sxs-lookup"><span data-stu-id="d0c42-227">There is only one SetupEntryPoint, so setup scripts need to be grouped in a single batch file if the application's setup requires multiple scripts.</span></span> <span data-ttu-id="d0c42-228">SetupEntryPoint může spustit libovolný typ souboru: spustitelné soubory, dávkové soubory a rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d0c42-228">The SetupEntryPoint can execute any type of file: executable files, batch files, and PowerShell cmdlets.</span></span> <span data-ttu-id="d0c42-229">Další podrobnosti najdete v tématu [konfigurace SetupEntryPoint](service-fabric-application-runas-security.md).</span><span class="sxs-lookup"><span data-stu-id="d0c42-229">For more details, see [Configure SetupEntryPoint](service-fabric-application-runas-security.md).</span></span>

<span data-ttu-id="d0c42-230">V předchozím příkladu běží SetupEntryPoint batch soubor s názvem `LaunchConfig.cmd` který je umístěný v `scripts` podadresáři adresáři kódu (za předpokladu, že WorkingFolder element je nastaven na hodnotu základu kódu).</span><span class="sxs-lookup"><span data-stu-id="d0c42-230">In the preceding example, the SetupEntryPoint runs a batch file called `LaunchConfig.cmd` that is located in the `scripts` subdirectory of the code directory (assuming the WorkingFolder element is set to CodeBase).</span></span>

#### <a name="update-entrypoint"></a><span data-ttu-id="d0c42-231">EntryPoint aktualizace</span><span class="sxs-lookup"><span data-stu-id="d0c42-231">Update EntryPoint</span></span>
```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

<span data-ttu-id="d0c42-232">`EntryPoint` Element v souboru manifestu služby se používá k určení způsobu spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-232">The `EntryPoint` element in the service manifest file is used to specify how to launch the service.</span></span> <span data-ttu-id="d0c42-233">`ExeHost` Element Určuje spustitelný soubor (a argumentů), by měl být použitý ke spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-233">The `ExeHost` element specifies the executable (and arguments) that should be used to launch the service.</span></span>

* <span data-ttu-id="d0c42-234">`Program`Určuje název spustitelného souboru, který by měl spustit službu.</span><span class="sxs-lookup"><span data-stu-id="d0c42-234">`Program` specifies the name of the executable that should start the service.</span></span>
* <span data-ttu-id="d0c42-235">`Arguments`Určuje argumenty, které by měla být předána spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="d0c42-235">`Arguments` specifies the arguments that should be passed to the executable.</span></span> <span data-ttu-id="d0c42-236">Může být seznam parametrů s argumenty.</span><span class="sxs-lookup"><span data-stu-id="d0c42-236">It can be a list of parameters with arguments.</span></span>
* <span data-ttu-id="d0c42-237">`WorkingFolder`Určuje pracovní adresář pro proces, který chcete spustit.</span><span class="sxs-lookup"><span data-stu-id="d0c42-237">`WorkingFolder` specifies the working directory for the process that is going to be started.</span></span> <span data-ttu-id="d0c42-238">Můžete určit tří hodnot:</span><span class="sxs-lookup"><span data-stu-id="d0c42-238">You can specify three values:</span></span>
  * <span data-ttu-id="d0c42-239">`CodeBase`Určuje, že pracovní adresář bude být nastavena na adresář kódu v balíčku aplikace (`Code` adresář v předchozím struktuře souborů).</span><span class="sxs-lookup"><span data-stu-id="d0c42-239">`CodeBase` specifies that the working directory is going to be set to the code directory in the application package (`Code` directory in the preceding file structure).</span></span>
  * <span data-ttu-id="d0c42-240">`CodePackage`Určuje, že pracovní adresář bude se nastaví na kořen balíčku aplikace (`GuestService1Pkg` v předchozí struktuře souborů).</span><span class="sxs-lookup"><span data-stu-id="d0c42-240">`CodePackage` specifies that the working directory is going to be set to the root of the application package    (`GuestService1Pkg` in the preceding file structure).</span></span>
    * <span data-ttu-id="d0c42-241">`Work`Určuje, že soubory jsou umístěny v podadresáři volat pracovní.</span><span class="sxs-lookup"><span data-stu-id="d0c42-241">`Work` specifies that the files are placed in a subdirectory called work.</span></span>

<span data-ttu-id="d0c42-242">WorkingFolder je vhodné nastavit správné pracovní adresář tak, aby relativní cesty mohou být využívána aplikace nebo inicializace skripty.</span><span class="sxs-lookup"><span data-stu-id="d0c42-242">The WorkingFolder is useful to set the correct working directory so that relative paths can be used by either the application or initialization scripts.</span></span>

#### <a name="update-endpoints-and-register-with-naming-service-for-communication"></a><span data-ttu-id="d0c42-243">Aktualizovat koncové body a zaregistrovat u služby DNS pro komunikaci</span><span class="sxs-lookup"><span data-stu-id="d0c42-243">Update Endpoints and register with Naming Service for communication</span></span>
```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
<span data-ttu-id="d0c42-244">V předchozím příkladu `Endpoint` element určuje koncové body, které aplikace může naslouchat na portu.</span><span class="sxs-lookup"><span data-stu-id="d0c42-244">In the preceding example, the `Endpoint` element specifies the endpoints that the application can listen on.</span></span> <span data-ttu-id="d0c42-245">V tomto příkladu aplikace Node.js naslouchá na protokolu http na portu 3000.</span><span class="sxs-lookup"><span data-stu-id="d0c42-245">In this example, the Node.js application listens on http on port 3000.</span></span>

<span data-ttu-id="d0c42-246">Kromě toho můžete pokládat Service Fabric publikovat tento koncový bod do služby DNS, tak další služby můžete zjistit adresa koncového bodu pro tuto službu.</span><span class="sxs-lookup"><span data-stu-id="d0c42-246">Furthermore you can ask Service Fabric to publish this endpoint to the Naming Service so other services can discover the endpoint address to this service.</span></span> <span data-ttu-id="d0c42-247">To vám umožňuje komunikovat mezi službami, které jsou hostované spustitelné soubory.</span><span class="sxs-lookup"><span data-stu-id="d0c42-247">This enables you to be able to communicate between services that are guest executables.</span></span>
<span data-ttu-id="d0c42-248">Adresa publikované koncového bodu je ve formátu `UriScheme://IPAddressOrFQDN:Port/PathSuffix`.</span><span class="sxs-lookup"><span data-stu-id="d0c42-248">The published endpoint address is of the form `UriScheme://IPAddressOrFQDN:Port/PathSuffix`.</span></span> <span data-ttu-id="d0c42-249">`UriScheme`a `PathSuffix` jsou volitelných atributů.</span><span class="sxs-lookup"><span data-stu-id="d0c42-249">`UriScheme` and `PathSuffix` are optional attributes.</span></span> <span data-ttu-id="d0c42-250">`IPAddressOrFQDN`je IP adresa nebo plně kvalifikovaný název domény tohoto spustitelného souboru získá umístěny na uzlu, a se vypočítává za vás.</span><span class="sxs-lookup"><span data-stu-id="d0c42-250">`IPAddressOrFQDN` is the IP address or fully qualified domain name of the node this executable gets placed on, and it is calculated for you.</span></span>

<span data-ttu-id="d0c42-251">V následujícím příkladu, jakmile nasazení služby v Service Fabric Explorer zobrazí podobná koncový bod `http://10.1.4.92:3000/myapp/` publikována pro instance služby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-251">In the following example, once the service is deployed, in Service Fabric Explorer you see an endpoint similar to `http://10.1.4.92:3000/myapp/` published for the service instance.</span></span> <span data-ttu-id="d0c42-252">Nebo pokud se jedná se o místní počítač, zobrazí `http://localhost:3000/myapp/`.</span><span class="sxs-lookup"><span data-stu-id="d0c42-252">Or if this is a local machine, you see `http://localhost:3000/myapp/`.</span></span>

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
<span data-ttu-id="d0c42-253">Můžete použít tyto adresy s [reverse proxy](service-fabric-reverseproxy.md) ke komunikaci mezi službami.</span><span class="sxs-lookup"><span data-stu-id="d0c42-253">You can use these addresses with [reverse proxy](service-fabric-reverseproxy.md) to communicate between services.</span></span>

### <a name="edit-the-application-manifest-file"></a><span data-ttu-id="d0c42-254">Upravit soubor manifestu aplikace</span><span class="sxs-lookup"><span data-stu-id="d0c42-254">Edit the application manifest file</span></span>
<span data-ttu-id="d0c42-255">Po konfiguraci `Servicemanifest.xml` souboru, budete muset udělat některé změny `ApplicationManifest.xml` zajistíte, že jsou použité správné služby typu a název souboru.</span><span class="sxs-lookup"><span data-stu-id="d0c42-255">Once you have configured the `Servicemanifest.xml` file, you need to make some changes to the `ApplicationManifest.xml` file to ensure that the correct service type and name are used.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a><span data-ttu-id="d0c42-256">ServiceManifestImport</span><span class="sxs-lookup"><span data-stu-id="d0c42-256">ServiceManifestImport</span></span>
<span data-ttu-id="d0c42-257">V `ServiceManifestImport` elementu, můžete zadat jednu nebo více služeb, které chcete zahrnout do aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0c42-257">In the `ServiceManifestImport` element, you can specify one or more services that you want to include in the app.</span></span> <span data-ttu-id="d0c42-258">Služby jsou odkazovány pomocí `ServiceManifestName`, která určuje název adresáře, kde `ServiceManifest.xml` se nachází soubor.</span><span class="sxs-lookup"><span data-stu-id="d0c42-258">Services are referenced with `ServiceManifestName`, which specifies the name of the directory where the `ServiceManifest.xml` file is located.</span></span>

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a><span data-ttu-id="d0c42-259">Nastavení protokolování</span><span class="sxs-lookup"><span data-stu-id="d0c42-259">Set up logging</span></span>
<span data-ttu-id="d0c42-260">Pro spustitelné soubory hosta je vhodné zobrazit protokoly konzoly a zjistěte, pokud aplikace a konfigurační skripty zobrazit všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="d0c42-260">For guest executables, it is useful to be able to see console logs to find out if the application and configuration scripts show any errors.</span></span>
<span data-ttu-id="d0c42-261">Přesměrování konzoly se dá nakonfigurovat v `ServiceManifest.xml` souboru pomocí `ConsoleRedirection` elementu.</span><span class="sxs-lookup"><span data-stu-id="d0c42-261">Console redirection can be configured in the `ServiceManifest.xml` file using the `ConsoleRedirection` element.</span></span>

> [!WARNING]
> <span data-ttu-id="d0c42-262">Nikdy nepoužívejte konzolu Zásady přesměrování v aplikaci, která je nasazena v produkčním prostředí, protože to může mít vliv převzetí služeb při selhání aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d0c42-262">Never use the console redirection policy in an application that is deployed in production because this can affect the application failover.</span></span> <span data-ttu-id="d0c42-263">*Pouze* používejte pro místní vývoj a účely ladění.</span><span class="sxs-lookup"><span data-stu-id="d0c42-263">*Only* use this for local development and debugging purposes.</span></span>  
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

<span data-ttu-id="d0c42-264">`ConsoleRedirection`slouží k přesměrování výstup konzoly (stdout a stderr) do pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="d0c42-264">`ConsoleRedirection` can be used to redirect console output (both stdout and stderr) to a working directory.</span></span> <span data-ttu-id="d0c42-265">To umožňuje ověřit, zda nejsou žádné chyby při instalaci nebo spuštění aplikace v clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d0c42-265">This provides the ability to verify that there are no errors during the setup or execution of the application in the Service Fabric cluster.</span></span>

<span data-ttu-id="d0c42-266">`FileRetentionCount`Určuje, kolik souborů se ukládají do pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="d0c42-266">`FileRetentionCount` determines how many files are saved in the working directory.</span></span> <span data-ttu-id="d0c42-267">Hodnota 5, například znamená, že soubory protokolu pro předchozí pět spuštěních se ukládají v pracovním adresáři.</span><span class="sxs-lookup"><span data-stu-id="d0c42-267">A value of 5, for example, means that the log files for the previous five executions are stored in the working directory.</span></span>

<span data-ttu-id="d0c42-268">`FileMaxSizeInKb`Určuje maximální velikost protokolových souborů.</span><span class="sxs-lookup"><span data-stu-id="d0c42-268">`FileMaxSizeInKb` specifies the maximum size of the log files.</span></span>

<span data-ttu-id="d0c42-269">Ukládání souborů protokolu v jednom ze služby pracovních adresářích.</span><span class="sxs-lookup"><span data-stu-id="d0c42-269">Log files are saved in one of the service's working directories.</span></span> <span data-ttu-id="d0c42-270">Chcete-li určit, kde jsou umístěny soubory, pomocí Service Fabric Explorer rozhodnout, který uzel služby běží na a pracovní adresář, kterému je používán.</span><span class="sxs-lookup"><span data-stu-id="d0c42-270">To determine where the files are located, use Service Fabric Explorer to determine which node the service is running on, and which working directory is being used.</span></span> <span data-ttu-id="d0c42-271">Tento proces je popsaná později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="d0c42-271">This process is covered later in this article.</span></span>

## <a name="deployment"></a><span data-ttu-id="d0c42-272">Nasazení</span><span class="sxs-lookup"><span data-stu-id="d0c42-272">Deployment</span></span>
<span data-ttu-id="d0c42-273">Posledním krokem je [nasazení aplikace](service-fabric-deploy-remove-applications.md).</span><span class="sxs-lookup"><span data-stu-id="d0c42-273">The last step is to [deploy your application](service-fabric-deploy-remove-applications.md).</span></span> <span data-ttu-id="d0c42-274">Následující skript prostředí PowerShell ukazuje, jak nasadit aplikace do místního vývojového clusteru a spustit novou službu Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d0c42-274">The following PowerShell script shows how to deploy your application to the local development cluster, and start a new Service Fabric service.</span></span>

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
> <span data-ttu-id="d0c42-275">[Komprimovat balíček](service-fabric-package-apps.md#compress-a-package) před kopírování do úložiště bitové kopie, pokud tento balíček je velký nebo má mnoho souborů.</span><span class="sxs-lookup"><span data-stu-id="d0c42-275">[Compress the package](service-fabric-package-apps.md#compress-a-package) before copying to the image store if the package is large or has many files.</span></span> <span data-ttu-id="d0c42-276">Další informace [zde](service-fabric-deploy-remove-applications.md#upload-the-application-package).</span><span class="sxs-lookup"><span data-stu-id="d0c42-276">Read more [here](service-fabric-deploy-remove-applications.md#upload-the-application-package).</span></span>
>

<span data-ttu-id="d0c42-277">Služba Service Fabric se dá nasadit v různých "konfigurace".</span><span class="sxs-lookup"><span data-stu-id="d0c42-277">A Service Fabric service can be deployed in various "configurations."</span></span> <span data-ttu-id="d0c42-278">Například se můžete nasadit jako jeden nebo více instancí, nebo se můžete nasadit tak, že existuje jedna instance služby v každém uzlu clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d0c42-278">For example, it can be deployed as single or multiple instances, or it can be deployed in such a way that there is one instance of the service on each node of the Service Fabric cluster.</span></span>

<span data-ttu-id="d0c42-279">`InstanceCount` Parametr `New-ServiceFabricService` rutiny slouží k určení, kolik instancí služby musí být spuštěna v clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d0c42-279">The `InstanceCount` parameter of the `New-ServiceFabricService` cmdlet is used to specify how many instances of the service should be launched in the Service Fabric cluster.</span></span> <span data-ttu-id="d0c42-280">Můžete nastavit `InstanceCount` hodnotu, v závislosti na typu aplikace, kterou nasazujete.</span><span class="sxs-lookup"><span data-stu-id="d0c42-280">You can set the `InstanceCount` value, depending on the type of application that you are deploying.</span></span> <span data-ttu-id="d0c42-281">Dva nejběžnější scénáře jsou:</span><span class="sxs-lookup"><span data-stu-id="d0c42-281">The two most common scenarios are:</span></span>

* <span data-ttu-id="d0c42-282">`InstanceCount = "1"`.</span><span class="sxs-lookup"><span data-stu-id="d0c42-282">`InstanceCount = "1"`.</span></span> <span data-ttu-id="d0c42-283">V takovém případě je pouze jedna instance služby nasadit v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d0c42-283">In this case, only one instance of the service is deployed in the cluster.</span></span> <span data-ttu-id="d0c42-284">Scheduler Service Fabric Určuje, který uzel služby se chystáte nasadit na.</span><span class="sxs-lookup"><span data-stu-id="d0c42-284">Service Fabric's scheduler determines which node the service is going to be deployed on.</span></span>
* <span data-ttu-id="d0c42-285">`InstanceCount ="-1"`.</span><span class="sxs-lookup"><span data-stu-id="d0c42-285">`InstanceCount ="-1"`.</span></span> <span data-ttu-id="d0c42-286">V takovém případě jedna instance služby je nasazen na každý uzel v clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d0c42-286">In this case, one instance of the service is deployed on every node in the Service Fabric cluster.</span></span> <span data-ttu-id="d0c42-287">Výsledek má jeden (a pouze jeden) instance služby pro každý uzel v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d0c42-287">The result is having one (and only one) instance of the service for each node in the cluster.</span></span>

<span data-ttu-id="d0c42-288">Toto je užitečné konfiguraci pro front-endu aplikace (například koncový bod REST), protože klientské aplikace potřebují "připojit" pro všechny uzly v clusteru, aby používaly koncový bod.</span><span class="sxs-lookup"><span data-stu-id="d0c42-288">This is a useful configuration for front-end applications (for example, a REST endpoint), because client applications need to "connect" to any of the nodes in the cluster to use the endpoint.</span></span> <span data-ttu-id="d0c42-289">Tato konfigurace mohou sloužit také když například jsou všechny uzly clusteru Service Fabric připojeni ke službě Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="d0c42-289">This configuration can also be used when, for example, all nodes of the Service Fabric cluster are connected to a load balancer.</span></span> <span data-ttu-id="d0c42-290">Přenosy klienta lze následně distribuovat napříč službou, která běží na všech uzlech v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d0c42-290">Client traffic can then be distributed across the service that is running on all nodes in the cluster.</span></span>

## <a name="check-your-running-application"></a><span data-ttu-id="d0c42-291">Zkontrolujte spuštěné aplikace</span><span class="sxs-lookup"><span data-stu-id="d0c42-291">Check your running application</span></span>
<span data-ttu-id="d0c42-292">V Service Fabric Exploreru Identifikujte uzlu, kde je služba spuštěná.</span><span class="sxs-lookup"><span data-stu-id="d0c42-292">In Service Fabric Explorer, identify the node where the service is running.</span></span> <span data-ttu-id="d0c42-293">V tomto příkladu je spuštěna na Uzel1:</span><span class="sxs-lookup"><span data-stu-id="d0c42-293">In this example, it runs on Node1:</span></span>

![Uzel, na kterém je spuštěna služba](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

<span data-ttu-id="d0c42-295">Přejděte na uzel a přejděte do aplikace, zobrazí informace o základní uzlu, včetně jeho umístění na disku.</span><span class="sxs-lookup"><span data-stu-id="d0c42-295">If you navigate to the node and browse to the application, you see the essential node information, including its location on disk.</span></span>

![Umístění na disku](./media/service-fabric-deploy-existing-app/locationondisk2.png)

<span data-ttu-id="d0c42-297">Pokud přejdete do adresáře pomocí Průzkumníku serveru, můžete najít pracovní adresář a služby do složky protokolů, jak je znázorněno na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="d0c42-297">If you browse to the directory by using Server Explorer, you can find the working directory and the service's log folder, as shown in the following screenshot:</span></span> 

![Umístění protokolu](./media/service-fabric-deploy-existing-app/loglocation.png)

## <a name="next-steps"></a><span data-ttu-id="d0c42-299">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d0c42-299">Next steps</span></span>
<span data-ttu-id="d0c42-300">V tomto článku jste se naučili, jak zabalit spustitelný soubor hosta a nasadíte ho do Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d0c42-300">In this article, you have learned how to package a guest executable and deploy it to Service Fabric.</span></span> <span data-ttu-id="d0c42-301">Najdete v následujících článcích související informace a úlohy.</span><span class="sxs-lookup"><span data-stu-id="d0c42-301">See the following articles for related information and tasks.</span></span>

* <span data-ttu-id="d0c42-302">[Ukázka pro balení a nasazení spustitelný soubor hosta](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), včetně odkaz na zkušební verzi nástroje balení</span><span class="sxs-lookup"><span data-stu-id="d0c42-302">[Sample for packaging and deploying a guest executable](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), including a link to the prerelease of the packaging tool</span></span>
* [<span data-ttu-id="d0c42-303">Ukázka dvěma hosta spustitelné soubory (C# a nodejs) komunikaci přes službu Naming pomocí REST</span><span class="sxs-lookup"><span data-stu-id="d0c42-303">Sample of two guest executables (C# and nodejs) communicating via the Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
* [<span data-ttu-id="d0c42-304">Nasazení několika hostujících spustitelných souborů</span><span class="sxs-lookup"><span data-stu-id="d0c42-304">Deploy multiple guest executables</span></span>](service-fabric-deploy-multiple-apps.md)
* [<span data-ttu-id="d0c42-305">Vytvoření první aplikace Service Fabric pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d0c42-305">Create your first Service Fabric application using Visual Studio</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
