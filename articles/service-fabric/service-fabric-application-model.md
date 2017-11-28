---
title: Model aplikace Azure Service Fabric | Microsoft Docs
description: "Jak modelu a popisují aplikace a služby v Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: e30482427b88eb3e58d39075c7f0734664b79aa2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="model-an-application-in-service-fabric"></a><span data-ttu-id="fadf5-103">Model aplikace v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="fadf5-103">Model an application in Service Fabric</span></span>
<span data-ttu-id="fadf5-104">Tento článek obsahuje přehled aplikačního modelu Azure Service Fabric a jak definovat aplikace a služby pomocí souborů manifestu.</span><span class="sxs-lookup"><span data-stu-id="fadf5-104">This article provides an overview of the Azure Service Fabric application model and how to define an application and service via manifest files.</span></span>

## <a name="understand-the-application-model"></a><span data-ttu-id="fadf5-105">Pochopení aplikačního modelu</span><span class="sxs-lookup"><span data-stu-id="fadf5-105">Understand the application model</span></span>
<span data-ttu-id="fadf5-106">Aplikace je kolekce základních služeb, které provádějí určité funkce nebo funkce.</span><span class="sxs-lookup"><span data-stu-id="fadf5-106">An application is a collection of constituent services that perform a certain function or functions.</span></span> <span data-ttu-id="fadf5-107">Služba provede kompletní a samostatné funkce a začít a spustit nezávisle na jiné služby.</span><span class="sxs-lookup"><span data-stu-id="fadf5-107">A service performs a complete and standalone function and can start and run independently of other services.</span></span>  <span data-ttu-id="fadf5-108">Služba se skládá z kódu, konfigurace a data.</span><span class="sxs-lookup"><span data-stu-id="fadf5-108">A service is composed of code, configuration, and data.</span></span> <span data-ttu-id="fadf5-109">Pro každou službu kódu se skládá z spustitelné binární soubory, konfigurace se skládá z nastavení služby, které je možné načíst v době běhu a data se skládá z libovolné statických dat, který se má používat služba.</span><span class="sxs-lookup"><span data-stu-id="fadf5-109">For each service, code consists of the executable binaries, configuration consists of service settings that can be loaded at run time, and data consists of arbitrary static data to be consumed by the service.</span></span> <span data-ttu-id="fadf5-110">Jednotlivé komponenty v tomto modelu hierarchické aplikace může být verzí a upgradovali nezávisle.</span><span class="sxs-lookup"><span data-stu-id="fadf5-110">Each component in this hierarchical application model can be versioned and upgraded independently.</span></span>

![Aplikační model Service Fabric][appmodel-diagram]

<span data-ttu-id="fadf5-112">Typ aplikace kategorizaci aplikace a skládá se z sady typy služeb.</span><span class="sxs-lookup"><span data-stu-id="fadf5-112">An application type is a categorization of an application and consists of a bundle of service types.</span></span> <span data-ttu-id="fadf5-113">Typ služby je kategorizaci služby.</span><span class="sxs-lookup"><span data-stu-id="fadf5-113">A service type is a categorization of a service.</span></span> <span data-ttu-id="fadf5-114">Kategorizaci podle služby může mít různá nastavení a konfigurace, ale základní funkce zůstává stejná.</span><span class="sxs-lookup"><span data-stu-id="fadf5-114">The categorization can have different settings and configurations, but the core functionality remains the same.</span></span> <span data-ttu-id="fadf5-115">Instance služby jsou variacím konfigurace jinou službu stejného typu služby.</span><span class="sxs-lookup"><span data-stu-id="fadf5-115">The instances of a service are the different service configuration variations of the same service type.</span></span>  

<span data-ttu-id="fadf5-116">Třídy (nebo "typů") aplikací a služeb, které jsou popsané prostřednictvím souborů XML (manifestů aplikace a služby manifesty).</span><span class="sxs-lookup"><span data-stu-id="fadf5-116">Classes (or "types") of applications and services are described through XML files (application manifests and service manifests).</span></span>  <span data-ttu-id="fadf5-117">Manifesty jsou šablony, kterými může být aplikace vytvořena z úložiště bitových kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="fadf5-117">The manifests are the templates against which applications can be instantiated from the cluster's image store.</span></span> <span data-ttu-id="fadf5-118">Definice schématu pro soubor ServiceManifest.xml a ApplicationManifest.xml je nainstalován pomocí Service Fabric SDK a nástroje k *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="fadf5-118">The schema definition for the ServiceManifest.xml and ApplicationManifest.xml file is installed with the Service Fabric SDK and tools to *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

<span data-ttu-id="fadf5-119">Kód pro jinou aplikaci instance spustit jako samostatné procesy, i když jsou hostované ve stejném uzlu Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="fadf5-119">The code for different application instances run as separate processes even when hosted by the same Service Fabric node.</span></span> <span data-ttu-id="fadf5-120">Kromě toho je možné spravovat životní cyklus každá instance aplikace (například upgradovat) nezávisle.</span><span class="sxs-lookup"><span data-stu-id="fadf5-120">Furthermore, the lifecycle of each application instance can be managed (for example, upgraded) independently.</span></span> <span data-ttu-id="fadf5-121">Následující diagram znázorňuje, jak typy aplikací se skládají z typů služeb, které pak se skládají z kódu, konfigurace a data balíčky.</span><span class="sxs-lookup"><span data-stu-id="fadf5-121">The following diagram shows how application types are composed of service types, which in turn are composed of code, configuration, and data packages.</span></span> <span data-ttu-id="fadf5-122">Pro zjednodušení diagramu, jenom kód/config/data balíčky pro `ServiceType4` se zobrazují, když každý typ služby by mělo zahrnovat některé nebo všechny ty typy balíčků.</span><span class="sxs-lookup"><span data-stu-id="fadf5-122">To simplify the diagram, only the code/config/data packages for `ServiceType4` are shown, though each service type would include some or all those package types.</span></span>

![Typy aplikací Service Fabric a typů služeb][cluster-imagestore-apptypes]

<span data-ttu-id="fadf5-124">K popisu služby a aplikace se používají dva různé soubory manifestu: service manifest a manifest aplikace.</span><span class="sxs-lookup"><span data-stu-id="fadf5-124">Two different manifest files are used to describe applications and services: the service manifest and application manifest.</span></span> <span data-ttu-id="fadf5-125">Manifesty jsou podrobně popsány v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="fadf5-125">Manifests are covered in detail in the following sections.</span></span>

<span data-ttu-id="fadf5-126">Aktivní v clusteru může být jeden nebo více instancí typu služby.</span><span class="sxs-lookup"><span data-stu-id="fadf5-126">There can be one or more instances of a service type active in the cluster.</span></span> <span data-ttu-id="fadf5-127">Například instance stavové služby nebo repliky, dosáhnout vysokou spolehlivostí nastavením stavu replikace mezi replikami, které jsou umístěné na různých uzlech v clusteru.</span><span class="sxs-lookup"><span data-stu-id="fadf5-127">For example, stateful service instances, or replicas, achieve high reliability by replicating state between replicas located on different nodes in the cluster.</span></span> <span data-ttu-id="fadf5-128">Replikace v podstatě poskytuje redundanci pro službu být k dispozici i v případě selhání jednoho uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="fadf5-128">Replication essentially provides redundancy for the service to be available even if one node in a cluster fails.</span></span> <span data-ttu-id="fadf5-129">A [oddíly služby](service-fabric-concepts-partitioning.md) další rozdělí stavu (a vzory přístupu na tento stav) mezi uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="fadf5-129">A [partitioned service](service-fabric-concepts-partitioning.md) further divides its state (and access patterns to that state) across nodes in the cluster.</span></span>

<span data-ttu-id="fadf5-130">Následující diagram znázorňuje vztah mezi aplikací a instance služby, oddíly a repliky.</span><span class="sxs-lookup"><span data-stu-id="fadf5-130">The following diagram shows the relationship between applications and service instances, partitions, and replicas.</span></span>

![Oddíly a repliky v rámci služby][cluster-application-instances]

> [!TIP]
> <span data-ttu-id="fadf5-132">Rozložení aplikace si můžete prohlédnout v clusteru s podporou pomocí nástroje Service Fabric Explorer k dispozici v http://&lt;yourclusteraddress&gt;: 19080/Explorer.</span><span class="sxs-lookup"><span data-stu-id="fadf5-132">You can view the layout of applications in a cluster using the Service Fabric Explorer tool available at http://&lt;yourclusteraddress&gt;:19080/Explorer.</span></span> <span data-ttu-id="fadf5-133">Další informace najdete v tématu [vizualizace vašeho clusteru pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="fadf5-133">For more information, see [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
> 
> 

## <a name="describe-a-service"></a><span data-ttu-id="fadf5-134">Popis služby</span><span class="sxs-lookup"><span data-stu-id="fadf5-134">Describe a service</span></span>
<span data-ttu-id="fadf5-135">Manifest služby deklarativně definuje typ služby a verze.</span><span class="sxs-lookup"><span data-stu-id="fadf5-135">The service manifest declaratively defines the service type and version.</span></span> <span data-ttu-id="fadf5-136">Určuje metadata služby, jako je například typ služby, Vlastnosti stavu, metriky Vyrovnávání zatížení, binární soubory služby a konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="fadf5-136">It specifies service metadata such as service type, health properties, load-balancing metrics, service binaries, and configuration files.</span></span>  <span data-ttu-id="fadf5-137">Jinými slovy, popisuje kód, konfigurace a data balíčky, které tvoří balíček služby pro podporu jeden nebo více typů služeb.</span><span class="sxs-lookup"><span data-stu-id="fadf5-137">Put another way, it describes the code, configuration, and data packages that compose a service package to support one or more service types.</span></span> <span data-ttu-id="fadf5-138">Zde je jednoduchý příklad manifest služby:</span><span class="sxs-lookup"><span data-stu-id="fadf5-138">Here is a simple example service manifest:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
    <EnvironmentVariables>
      <EnvironmentVariable Name="MyEnvVariable" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentVariables>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
```

<span data-ttu-id="fadf5-139">**Verze** atributy jsou nestrukturovaných řetězce a analyzovat není v systému.</span><span class="sxs-lookup"><span data-stu-id="fadf5-139">**Version** attributes are unstructured strings and not parsed by the system.</span></span> <span data-ttu-id="fadf5-140">Verze atributy se používá k verze jednotlivých součástí pro upgrade.</span><span class="sxs-lookup"><span data-stu-id="fadf5-140">Version attributes are used to version each component for upgrades.</span></span>

<span data-ttu-id="fadf5-141">**ServiceTypes** deklaruje, jaké typy služby podporuje **CodePackages** v tomto manifestu.</span><span class="sxs-lookup"><span data-stu-id="fadf5-141">**ServiceTypes** declares what service types are supported by **CodePackages** in this manifest.</span></span> <span data-ttu-id="fadf5-142">Při vytváření instance služby pro jeden z těchto typů služeb aktivují se všechny balíčky kódu, které jsou deklarované v manifestu tento spuštěním jejich vstupní body.</span><span class="sxs-lookup"><span data-stu-id="fadf5-142">When a service is instantiated against one of these service types, all code packages declared in this manifest are activated by running their entry points.</span></span> <span data-ttu-id="fadf5-143">Výsledný procesy se očekává registrace podporovaných typů služeb v době běhu.</span><span class="sxs-lookup"><span data-stu-id="fadf5-143">The resulting processes are expected to register the supported service types at run time.</span></span> <span data-ttu-id="fadf5-144">Typů služeb jsou deklarované v manifestu úroveň a úroveň typu balíček kódu.</span><span class="sxs-lookup"><span data-stu-id="fadf5-144">Service types are declared at the manifest level and not the code package level.</span></span> <span data-ttu-id="fadf5-145">Takže pokud je více balíčků kódu, všechny aktivují se vždy, když systém hledá pro každý typ deklarované služby.</span><span class="sxs-lookup"><span data-stu-id="fadf5-145">So when there are multiple code packages, they are all activated whenever the system looks for any one of the declared service types.</span></span>

<span data-ttu-id="fadf5-146">**SetupEntryPoint** je privilegované vstupního bodu, který běží se stejnými pověřeními, jako Service Fabric (obvykle *LocalSystem* účtu) před další vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="fadf5-146">**SetupEntryPoint** is a privileged entry point that runs with the same credentials as Service Fabric (typically the *LocalSystem* account) before any other entry point.</span></span> <span data-ttu-id="fadf5-147">Spustitelný soubor určený podle **EntryPoint** je obvykle dlouho běžící hostitele služby.</span><span class="sxs-lookup"><span data-stu-id="fadf5-147">The executable specified by **EntryPoint** is typically the long-running service host.</span></span> <span data-ttu-id="fadf5-148">Přítomnost vstupní bod samostatného instalačního zabraňuje nutnosti spuštění hostitele služby s vysokou úrovní oprávnění pro dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="fadf5-148">The presence of a separate setup entry point avoids having to run the service host with high privileges for extended periods of time.</span></span> <span data-ttu-id="fadf5-149">Spustitelný soubor určený podle **EntryPoint** běží **SetupEntryPoint** ukončí úspěšně.</span><span class="sxs-lookup"><span data-stu-id="fadf5-149">The executable specified by **EntryPoint** is run after **SetupEntryPoint** exits successfully.</span></span> <span data-ttu-id="fadf5-150">Pokud proces se někdy ukončí nebo dojde k chybě, výsledná proces monitorovat a restartuje (počínaje znovu **SetupEntryPoint**).</span><span class="sxs-lookup"><span data-stu-id="fadf5-150">If the process ever terminates or crashes, the resulting process is monitored and restarted (beginning again with **SetupEntryPoint**).</span></span>  

<span data-ttu-id="fadf5-151">Typické scénáře použití **SetupEntryPoint** jsou při spustit spustitelný soubor před spuštěním služby nebo k provedení operace se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="fadf5-151">Typical scenarios for using **SetupEntryPoint** are when you run an executable before the service starts or you perform an operation with elevated privileges.</span></span> <span data-ttu-id="fadf5-152">Například:</span><span class="sxs-lookup"><span data-stu-id="fadf5-152">For example:</span></span>

* <span data-ttu-id="fadf5-153">Nastavení a inicializace proměnných prostředí, které potřebuje spustitelný soubor služby.</span><span class="sxs-lookup"><span data-stu-id="fadf5-153">Setting up and initializing environment variables that the service executable needs.</span></span> <span data-ttu-id="fadf5-154">Toto není omezen na napsané pomocí Service Fabric programovací modely pouze spustitelné soubory.</span><span class="sxs-lookup"><span data-stu-id="fadf5-154">This is not limited to only executables written via the Service Fabric programming models.</span></span> <span data-ttu-id="fadf5-155">Například npm.exe musí některé proměnné prostředí nakonfigurované pro nasazení aplikace node.js.</span><span class="sxs-lookup"><span data-stu-id="fadf5-155">For example, npm.exe needs some environment variables configured for deploying a node.js application.</span></span>
* <span data-ttu-id="fadf5-156">Nastavení řízení přístupu nainstalováním certifikáty zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fadf5-156">Setting up access control by installing security certificates.</span></span>

<span data-ttu-id="fadf5-157">Další informace o tom, jak nakonfigurovat **SetupEntryPoint** najdete v části [nakonfigurovat zásady pro bod služby instalační položka](service-fabric-application-runas-security.md)</span><span class="sxs-lookup"><span data-stu-id="fadf5-157">For more details on how to configure the **SetupEntryPoint** see [Configure the policy for a service setup entry point](service-fabric-application-runas-security.md)</span></span>

<span data-ttu-id="fadf5-158">**EnvironmentVariables** poskytuje seznam proměnných prostředí, které jsou nastavené pro tento balíček kódu.</span><span class="sxs-lookup"><span data-stu-id="fadf5-158">**EnvironmentVariables** provides a list of environment variables that are set for this code package.</span></span> <span data-ttu-id="fadf5-159">Environmentment proměnné mohou být přepsána nastaveními v `ApplicationManifest.xml` zajistit různé hodnoty pro instance různé služby.</span><span class="sxs-lookup"><span data-stu-id="fadf5-159">Environmentment variables can be overridden in the `ApplicationManifest.xml` to provide different values for different service instances.</span></span> 

<span data-ttu-id="fadf5-160">**DataPackage** deklaruje do složky s názvem podle **název** atribut, který obsahuje libovolné statických dat, který se má používat v procesu v době běhu.</span><span class="sxs-lookup"><span data-stu-id="fadf5-160">**DataPackage** declares a folder, named by the **Name** attribute, that contains arbitrary static data to be consumed by the process at run time.</span></span>

<span data-ttu-id="fadf5-161">**ConfigPackage** deklaruje do složky s názvem podle **název** atribut, který obsahuje *souborech Settings.xml* souboru.</span><span class="sxs-lookup"><span data-stu-id="fadf5-161">**ConfigPackage** declares a folder, named by the **Name** attribute, that contains a *Settings.xml* file.</span></span> <span data-ttu-id="fadf5-162">Soubor nastavení obsahuje oddíly pár definovaný uživatelem, klíč hodnota nastavení, která čte proces zpět za běhu.</span><span class="sxs-lookup"><span data-stu-id="fadf5-162">The settings file contains sections of user-defined, key-value pair settings that the process reads back at run time.</span></span> <span data-ttu-id="fadf5-163">Během upgradu, pokud je to pouze **ConfigPackage** **verze** došlo ke změně, pak není běžící proces restartovat.</span><span class="sxs-lookup"><span data-stu-id="fadf5-163">During an upgrade, if only the **ConfigPackage** **version** has changed, then the running process is not restarted.</span></span> <span data-ttu-id="fadf5-164">Místo toho zpětné volání upozorní proces, který se změnila nastavení konfigurace, může být dynamicky znovu.</span><span class="sxs-lookup"><span data-stu-id="fadf5-164">Instead, a callback notifies the process that configuration settings have changed so they can be reloaded dynamically.</span></span> <span data-ttu-id="fadf5-165">Tady je příklad *souborech Settings.xml* souboru:</span><span class="sxs-lookup"><span data-stu-id="fadf5-165">Here is an example *Settings.xml* file:</span></span>

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

> [!NOTE]
> <span data-ttu-id="fadf5-166">Manifest služby může obsahovat více kód, konfigurace a data balíčky.</span><span class="sxs-lookup"><span data-stu-id="fadf5-166">A service manifest can contain multiple code, configuration, and data packages.</span></span> <span data-ttu-id="fadf5-167">Každý z nich může být verzí nezávisle.</span><span class="sxs-lookup"><span data-stu-id="fadf5-167">Each of those can be versioned independently.</span></span>
> 
> 

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a><span data-ttu-id="fadf5-168">Popis aplikace</span><span class="sxs-lookup"><span data-stu-id="fadf5-168">Describe an application</span></span>
<span data-ttu-id="fadf5-169">Manifest aplikace deklarativně popisuje typ aplikace a verze.</span><span class="sxs-lookup"><span data-stu-id="fadf5-169">The application manifest declaratively describes the application type and version.</span></span> <span data-ttu-id="fadf5-170">Určuje metadata služby složení například stabilní názvy, vytváření oddílů schématu, faktor počet/replikace instance, zásady zabezpečení nebo izolace, omezení umístění, konfigurace přepsání a typů základní služby.</span><span class="sxs-lookup"><span data-stu-id="fadf5-170">It specifies service composition metadata such as stable names, partitioning scheme, instance count/replication factor, security/isolation policy, placement constraints, configuration overrides, and constituent service types.</span></span> <span data-ttu-id="fadf5-171">Vyrovnávání zatížení domény, do které se umístí aplikace jsou také popsány.</span><span class="sxs-lookup"><span data-stu-id="fadf5-171">The load-balancing domains into which the application is placed are also described.</span></span>

<span data-ttu-id="fadf5-172">Manifest aplikace proto popisuje elementy na úrovni aplikace a odkazuje na jeden nebo více manifesty služby k vytváření typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="fadf5-172">Thus, an application manifest describes elements at the application level and references one or more service manifests to compose an application type.</span></span> <span data-ttu-id="fadf5-173">Zde je jednoduchý příklad manifest aplikace:</span><span class="sxs-lookup"><span data-stu-id="fadf5-173">Here is a simple example application manifest:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
    <ConfigOverrides/>
    <EnvironmentOverrides CodePackageRef="MyCode"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
```

<span data-ttu-id="fadf5-174">Jako služba manifesty **verze** atributy jsou nestrukturovaných řetězce a nejsou analyzovat v systému.</span><span class="sxs-lookup"><span data-stu-id="fadf5-174">Like service manifests, **Version** attributes are unstructured strings and are not parsed by the system.</span></span> <span data-ttu-id="fadf5-175">Verze atributy jsou také použít k verze jednotlivých součástí pro upgrade.</span><span class="sxs-lookup"><span data-stu-id="fadf5-175">Version attributes are also used to version each component for upgrades.</span></span>

<span data-ttu-id="fadf5-176">**ServiceManifestImport** obsahuje odkazy na manifesty služby, které tvoří tento typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="fadf5-176">**ServiceManifestImport** contains references to service manifests that compose this application type.</span></span> <span data-ttu-id="fadf5-177">Manifesty importované služba zjistit, jaké typy služby jsou platné v rámci tento typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="fadf5-177">Imported service manifests determine what service types are valid within this application type.</span></span> <span data-ttu-id="fadf5-178">V rámci ServiceManifestImport můžete přepsat hodnoty konfigurace v souborech Settings.xml a prostředí proměnné v ServiceManifest.xml soubory.</span><span class="sxs-lookup"><span data-stu-id="fadf5-178">Within the ServiceManifestImport, you override configuration values in Settings.xml and environment variables in ServiceManifest.xml files.</span></span> 


<span data-ttu-id="fadf5-179">**DefaultServices** deklaruje instance služby, které se automaticky vytvoří vždy, když aplikace je vytvořena instance pro tento typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="fadf5-179">**DefaultServices** declares service instances that are automatically created whenever an application is instantiated against this application type.</span></span> <span data-ttu-id="fadf5-180">Výchozí služby jsou pouze pro vaše pohodlí a chovají se jako normální služby, každý po jejich vytvoření.</span><span class="sxs-lookup"><span data-stu-id="fadf5-180">Default services are just a convenience and behave like normal services in every respect after they have been created.</span></span> <span data-ttu-id="fadf5-181">Tyto jsou upgradovány spolu s jinými službami v instanci aplikace a může být odebrán také.</span><span class="sxs-lookup"><span data-stu-id="fadf5-181">They are upgraded along with any other services in the application instance and can be removed as well.</span></span>

> [!NOTE]
> <span data-ttu-id="fadf5-182">Manifest aplikace může obsahovat více manifestu importy služby a výchozí služby.</span><span class="sxs-lookup"><span data-stu-id="fadf5-182">An application manifest can contain multiple service manifest imports and default services.</span></span> <span data-ttu-id="fadf5-183">Každý importu manifestu služby může být nezávisle verzí.</span><span class="sxs-lookup"><span data-stu-id="fadf5-183">Each service manifest import can be versioned independently.</span></span>
> 
> 

<span data-ttu-id="fadf5-184">Zjistěte, jak udržovat různé aplikace a služby parametry pro jednotlivá prostředí, najdete v tématu [Správa parametry aplikace pro prostředí s více](service-fabric-manage-multiple-environment-app-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="fadf5-184">To learn how to maintain different application and service parameters for individual environments, see [Managing application parameters for multiple environments](service-fabric-manage-multiple-environment-app-configuration.md).</span></span>

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a><span data-ttu-id="fadf5-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fadf5-185">Next steps</span></span>
<span data-ttu-id="fadf5-186">[Balíček aplikace](service-fabric-package-apps.md) a nastavit jej jako připraveny k nasazení.</span><span class="sxs-lookup"><span data-stu-id="fadf5-186">[Package an application](service-fabric-package-apps.md) and make it ready to deploy.</span></span>

<span data-ttu-id="fadf5-187">[Nasazení a odebírat aplikace] [ 10] popisuje, jak pomocí prostředí PowerShell pro správu instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="fadf5-187">[Deploy and remove applications][10] describes how to use PowerShell to manage application instances.</span></span>

<span data-ttu-id="fadf5-188">[Správa aplikací parametry pro prostředí s více] [ 11] popisuje postup konfigurace parametrů a proměnných prostředí pro instance jinou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fadf5-188">[Managing application parameters for multiple environments][11] describes how to configure parameters and environment variables for different application instances.</span></span>

<span data-ttu-id="fadf5-189">[Konfigurovat zásady zabezpečení pro vaši aplikaci] [ 12] popisuje, jak ke spouštění služeb v rámci zásad zabezpečení, které omezují přístup.</span><span class="sxs-lookup"><span data-stu-id="fadf5-189">[Configure security policies for your application][12] describes how to run services under security policies to restrict access.</span></span>

<span data-ttu-id="fadf5-190">[Aplikace hostující modely] [ 13] popisují vztah mezi nasazená služba a služba hostitelský proces repliky (nebo instance).</span><span class="sxs-lookup"><span data-stu-id="fadf5-190">[Application hosting models][13] describe relationship between replicas (or instances) of a deployed service and service-host process.</span></span>

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
[13]: service-fabric-hosting-model.md
