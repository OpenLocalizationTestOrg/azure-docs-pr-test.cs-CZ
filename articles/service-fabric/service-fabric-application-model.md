---
title: model aplikace Service Fabric aaaAzure | Microsoft Docs
description: "Jak toomodel a popisují aplikace a služby v Service Fabric."
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
ms.openlocfilehash: 54c4d026e7d556be5f697d4a6f2ee886687e1c35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="model-an-application-in-service-fabric"></a><span data-ttu-id="bd6d7-103">Model aplikace v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="bd6d7-103">Model an application in Service Fabric</span></span>
<span data-ttu-id="bd6d7-104">Tento článek obsahuje přehled aplikačního modelu hello Azure Service Fabric a jak toodefine aplikace a služby prostřednictvím manifest soubory.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-104">This article provides an overview of hello Azure Service Fabric application model and how toodefine an application and service via manifest files.</span></span>

## <a name="understand-hello-application-model"></a><span data-ttu-id="bd6d7-105">Pochopení hello aplikačního modelu</span><span class="sxs-lookup"><span data-stu-id="bd6d7-105">Understand hello application model</span></span>
<span data-ttu-id="bd6d7-106">Aplikace je kolekce základních služeb, které provádějí určité funkce nebo funkce.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-106">An application is a collection of constituent services that perform a certain function or functions.</span></span> <span data-ttu-id="bd6d7-107">Služba provede kompletní a samostatné funkce a začít a spustit nezávisle na jiné služby.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-107">A service performs a complete and standalone function and can start and run independently of other services.</span></span>  <span data-ttu-id="bd6d7-108">Služba se skládá z kódu, konfigurace a data.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-108">A service is composed of code, configuration, and data.</span></span> <span data-ttu-id="bd6d7-109">Pro každou službu kódu se skládá z binární hello spustitelné soubory, konfigurace se skládá z nastavení služby, které je možné načíst v době běhu a data se skládá z libovolné statických dat toobe spotřebovávají hello služby.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-109">For each service, code consists of hello executable binaries, configuration consists of service settings that can be loaded at run time, and data consists of arbitrary static data toobe consumed by hello service.</span></span> <span data-ttu-id="bd6d7-110">Jednotlivé komponenty v tomto modelu hierarchické aplikace může být verzí a upgradovali nezávisle.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-110">Each component in this hierarchical application model can be versioned and upgraded independently.</span></span>

![model aplikace Service Fabric Hello][appmodel-diagram]

<span data-ttu-id="bd6d7-112">Typ aplikace kategorizaci aplikace a skládá se z sady typy služeb.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-112">An application type is a categorization of an application and consists of a bundle of service types.</span></span> <span data-ttu-id="bd6d7-113">Typ služby je kategorizaci služby.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-113">A service type is a categorization of a service.</span></span> <span data-ttu-id="bd6d7-114">Hello kategorizaci může mít různá nastavení a konfigurace, ale hello základní funkce zůstane hello stejné.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-114">hello categorization can have different settings and configurations, but hello core functionality remains hello same.</span></span> <span data-ttu-id="bd6d7-115">Hello instancí služby jsou variacím konfigurace hello jinou službu systému hello stejný typ služby.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-115">hello instances of a service are hello different service configuration variations of hello same service type.</span></span>  

<span data-ttu-id="bd6d7-116">Třídy (nebo "typů") aplikací a služeb, které jsou popsané prostřednictvím souborů XML (manifestů aplikace a služby manifesty).</span><span class="sxs-lookup"><span data-stu-id="bd6d7-116">Classes (or "types") of applications and services are described through XML files (application manifests and service manifests).</span></span>  <span data-ttu-id="bd6d7-117">manifesty Hello jsou hello šablony, kterými může být aplikace vytvořena z úložiště bitových kopií hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-117">hello manifests are hello templates against which applications can be instantiated from hello cluster's image store.</span></span> <span data-ttu-id="bd6d7-118">Hello definice schématu pro soubor ServiceManifest.xml a ApplicationManifest.xml hello se instaluje s hello Service Fabric SDK a nástrojů pro příliš*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-118">hello schema definition for hello ServiceManifest.xml and ApplicationManifest.xml file is installed with hello Service Fabric SDK and tools too*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

<span data-ttu-id="bd6d7-119">Hello kód pro spuštění jako samostatné procesy, i když jsou hostované pomocí hello stejného uzlu Service Fabric instance jinou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-119">hello code for different application instances run as separate processes even when hosted by hello same Service Fabric node.</span></span> <span data-ttu-id="bd6d7-120">Kromě toho je možné spravovat životní cyklus hello každá instance aplikace (například upgradovat) nezávisle.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-120">Furthermore, hello lifecycle of each application instance can be managed (for example, upgraded) independently.</span></span> <span data-ttu-id="bd6d7-121">Hello následující diagram ukazuje, jak typy aplikací se skládají z typů služeb, které pak se skládají z kódu, konfigurace a data balíčky.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-121">hello following diagram shows how application types are composed of service types, which in turn are composed of code, configuration, and data packages.</span></span> <span data-ttu-id="bd6d7-122">diagram hello toosimplify, hello balíčků kódu/config/dat pouze pro `ServiceType4` se zobrazují, když každý typ služby by mělo zahrnovat některé nebo všechny ty typy balíčků.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-122">toosimplify hello diagram, only hello code/config/data packages for `ServiceType4` are shown, though each service type would include some or all those package types.</span></span>

![Typy aplikací Service Fabric a typů služeb][cluster-imagestore-apptypes]

<span data-ttu-id="bd6d7-124">Jsou dva různé soubory manifestu použité toodescribe aplikací a služeb: hello service manifest a manifest aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-124">Two different manifest files are used toodescribe applications and services: hello service manifest and application manifest.</span></span> <span data-ttu-id="bd6d7-125">Manifesty jsou podrobně popsány v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-125">Manifests are covered in detail in hello following sections.</span></span>

<span data-ttu-id="bd6d7-126">V clusteru hello aktivní může být jeden nebo více instancí typu služby.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-126">There can be one or more instances of a service type active in hello cluster.</span></span> <span data-ttu-id="bd6d7-127">Například instance stavové služby nebo repliky, dosáhnout nastavením stavu replikace mezi replikami, které jsou umístěné na různých uzlech v clusteru hello vysokou spolehlivostí.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-127">For example, stateful service instances, or replicas, achieve high reliability by replicating state between replicas located on different nodes in hello cluster.</span></span> <span data-ttu-id="bd6d7-128">Replikace v podstatě poskytuje redundanci pro toobe služby hello k dispozici i v případě selhání jednoho uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-128">Replication essentially provides redundancy for hello service toobe available even if one node in a cluster fails.</span></span> <span data-ttu-id="bd6d7-129">A [oddíly služby](service-fabric-concepts-partitioning.md) další vydělí jeho stav (a přístup vzory toothat stav) mezi uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-129">A [partitioned service](service-fabric-concepts-partitioning.md) further divides its state (and access patterns toothat state) across nodes in hello cluster.</span></span>

<span data-ttu-id="bd6d7-130">Hello následující diagram znázorňuje hello vztah mezi aplikací a instance služby, oddíly a repliky.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-130">hello following diagram shows hello relationship between applications and service instances, partitions, and replicas.</span></span>

![Oddíly a repliky v rámci služby][cluster-application-instances]

> [!TIP]
> <span data-ttu-id="bd6d7-132">Rozložení hello aplikací si můžete prohlédnout v clusteru pomocí nástroje Service Fabric Explorer hello k dispozici v http://&lt;yourclusteraddress&gt;: 19080/Explorer.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-132">You can view hello layout of applications in a cluster using hello Service Fabric Explorer tool available at http://&lt;yourclusteraddress&gt;:19080/Explorer.</span></span> <span data-ttu-id="bd6d7-133">Další informace najdete v tématu [vizualizace vašeho clusteru pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="bd6d7-133">For more information, see [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
> 
> 

## <a name="describe-a-service"></a><span data-ttu-id="bd6d7-134">Popis služby</span><span class="sxs-lookup"><span data-stu-id="bd6d7-134">Describe a service</span></span>
<span data-ttu-id="bd6d7-135">manifest služby Hello deklarativně definuje hello služby typu a verzi.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-135">hello service manifest declaratively defines hello service type and version.</span></span> <span data-ttu-id="bd6d7-136">Určuje metadata služby, jako je například typ služby, Vlastnosti stavu, metriky Vyrovnávání zatížení, binární soubory služby a konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-136">It specifies service metadata such as service type, health properties, load-balancing metrics, service binaries, and configuration files.</span></span>  <span data-ttu-id="bd6d7-137">Jinými slovy, popisuje hello kód, konfigurace a data balíčky, které tvoří toosupport balíček služby jeden nebo více typů služeb.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-137">Put another way, it describes hello code, configuration, and data packages that compose a service package toosupport one or more service types.</span></span> <span data-ttu-id="bd6d7-138">Zde je jednoduchý příklad manifest služby:</span><span class="sxs-lookup"><span data-stu-id="bd6d7-138">Here is a simple example service manifest:</span></span>

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

<span data-ttu-id="bd6d7-139">**Verze** atributy jsou nestrukturovaných řetězce a analyzovat není systémem hello.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-139">**Version** attributes are unstructured strings and not parsed by hello system.</span></span> <span data-ttu-id="bd6d7-140">Verze atributy jsou použité tooversion jednotlivé komponenty pro upgrade.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-140">Version attributes are used tooversion each component for upgrades.</span></span>

<span data-ttu-id="bd6d7-141">**ServiceTypes** deklaruje, jaké typy služby podporuje **CodePackages** v tomto manifestu.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-141">**ServiceTypes** declares what service types are supported by **CodePackages** in this manifest.</span></span> <span data-ttu-id="bd6d7-142">Při vytváření instance služby pro jeden z těchto typů služeb aktivují se všechny balíčky kódu, které jsou deklarované v manifestu tento spuštěním jejich vstupní body.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-142">When a service is instantiated against one of these service types, all code packages declared in this manifest are activated by running their entry points.</span></span> <span data-ttu-id="bd6d7-143">Výsledný procesy Hello jsou očekávané tooregister hello podporované služby typy za běhu.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-143">hello resulting processes are expected tooregister hello supported service types at run time.</span></span> <span data-ttu-id="bd6d7-144">Typů služeb jsou deklarované v manifestu úroveň hello a není hello úrovni balíčku kódu.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-144">Service types are declared at hello manifest level and not hello code package level.</span></span> <span data-ttu-id="bd6d7-145">Takže pokud je více balíčků kódu, všechny aktivují se vždy, když systém hello hledá některého hello deklarovaný typů služeb.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-145">So when there are multiple code packages, they are all activated whenever hello system looks for any one of hello declared service types.</span></span>

<span data-ttu-id="bd6d7-146">**SetupEntryPoint** je privilegované vstupního bodu, který je spuštěn s hello stejné přihlašovací údaje jako Service Fabric (obvykle hello *LocalSystem* účtu) před další vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-146">**SetupEntryPoint** is a privileged entry point that runs with hello same credentials as Service Fabric (typically hello *LocalSystem* account) before any other entry point.</span></span> <span data-ttu-id="bd6d7-147">spustitelný soubor Hello určeného **EntryPoint** je obvykle hostitele služby dlouho běžící hello.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-147">hello executable specified by **EntryPoint** is typically hello long-running service host.</span></span> <span data-ttu-id="bd6d7-148">Hello přítomnost vstupní bod samostatného instalačního zabraňuje s toorun hello hostitele služby s vysokou úrovní oprávnění pro dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-148">hello presence of a separate setup entry point avoids having toorun hello service host with high privileges for extended periods of time.</span></span> <span data-ttu-id="bd6d7-149">spustitelný soubor Hello určeného **EntryPoint** běží **SetupEntryPoint** ukončí úspěšně.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-149">hello executable specified by **EntryPoint** is run after **SetupEntryPoint** exits successfully.</span></span> <span data-ttu-id="bd6d7-150">Pokud proces hello někdy ukončí nebo dojde k chybě, výsledná proces hello monitorovat a restartování (počínaje znovu **SetupEntryPoint**).</span><span class="sxs-lookup"><span data-stu-id="bd6d7-150">If hello process ever terminates or crashes, hello resulting process is monitored and restarted (beginning again with **SetupEntryPoint**).</span></span>  

<span data-ttu-id="bd6d7-151">Typické scénáře použití **SetupEntryPoint** jsou při spustit spustitelný soubor před spuštěním služby hello nebo k provedení operace se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-151">Typical scenarios for using **SetupEntryPoint** are when you run an executable before hello service starts or you perform an operation with elevated privileges.</span></span> <span data-ttu-id="bd6d7-152">Například:</span><span class="sxs-lookup"><span data-stu-id="bd6d7-152">For example:</span></span>

* <span data-ttu-id="bd6d7-153">Nastavení a inicializace proměnné prostředí, které hello potřebám spustitelný soubor služby.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-153">Setting up and initializing environment variables that hello service executable needs.</span></span> <span data-ttu-id="bd6d7-154">Toto není omezený tooonly spustitelné soubory zapisovat prostřednictvím programovací modely hello Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-154">This is not limited tooonly executables written via hello Service Fabric programming models.</span></span> <span data-ttu-id="bd6d7-155">Například npm.exe musí některé proměnné prostředí nakonfigurované pro nasazení aplikace node.js.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-155">For example, npm.exe needs some environment variables configured for deploying a node.js application.</span></span>
* <span data-ttu-id="bd6d7-156">Nastavení řízení přístupu nainstalováním certifikáty zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-156">Setting up access control by installing security certificates.</span></span>

<span data-ttu-id="bd6d7-157">Další podrobnosti o tom tooconfigure hello **SetupEntryPoint** najdete v části [konfigurace hello zásady pro bod služby instalační položka](service-fabric-application-runas-security.md)</span><span class="sxs-lookup"><span data-stu-id="bd6d7-157">For more details on how tooconfigure hello **SetupEntryPoint** see [Configure hello policy for a service setup entry point](service-fabric-application-runas-security.md)</span></span>

<span data-ttu-id="bd6d7-158">**EnvironmentVariables** poskytuje seznam proměnných prostředí, které jsou nastavené pro tento balíček kódu.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-158">**EnvironmentVariables** provides a list of environment variables that are set for this code package.</span></span> <span data-ttu-id="bd6d7-159">Environmentment proměnné mohou být přepsána nastaveními v hello `ApplicationManifest.xml` tooprovide různé hodnoty pro různé služby instance.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-159">Environmentment variables can be overridden in hello `ApplicationManifest.xml` tooprovide different values for different service instances.</span></span> 

<span data-ttu-id="bd6d7-160">**DataPackage** deklaruje do složky s názvem podle hello **název** atribut, který obsahuje libovolné statických dat toobe spotřebovávají hello procesu v době běhu.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-160">**DataPackage** declares a folder, named by hello **Name** attribute, that contains arbitrary static data toobe consumed by hello process at run time.</span></span>

<span data-ttu-id="bd6d7-161">**ConfigPackage** deklaruje do složky s názvem podle hello **název** atribut, který obsahuje *souborech Settings.xml* souboru.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-161">**ConfigPackage** declares a folder, named by hello **Name** attribute, that contains a *Settings.xml* file.</span></span> <span data-ttu-id="bd6d7-162">soubor nastavení Hello obsahuje oddíly pár definovaný uživatelem, klíč hodnota nastavení, která čte hello proces zpět za běhu.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-162">hello settings file contains sections of user-defined, key-value pair settings that hello process reads back at run time.</span></span> <span data-ttu-id="bd6d7-163">Během upgradu, pokud pouze hello **ConfigPackage** **verze** došlo ke změně, pak není hello spuštěn proces restartovat.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-163">During an upgrade, if only hello **ConfigPackage** **version** has changed, then hello running process is not restarted.</span></span> <span data-ttu-id="bd6d7-164">Místo toho zpětné volání upozorní hello procesu, které mění nastavení konfigurace, může být dynamicky znovu.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-164">Instead, a callback notifies hello process that configuration settings have changed so they can be reloaded dynamically.</span></span> <span data-ttu-id="bd6d7-165">Tady je příklad *souborech Settings.xml* souboru:</span><span class="sxs-lookup"><span data-stu-id="bd6d7-165">Here is an example *Settings.xml* file:</span></span>

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

> [!NOTE]
> <span data-ttu-id="bd6d7-166">Manifest služby může obsahovat více kód, konfigurace a data balíčky.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-166">A service manifest can contain multiple code, configuration, and data packages.</span></span> <span data-ttu-id="bd6d7-167">Každý z nich může být verzí nezávisle.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-167">Each of those can be versioned independently.</span></span>
> 
> 

<!--
For more information about other features supported by service manifests, refer toohello following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a><span data-ttu-id="bd6d7-168">Popis aplikace</span><span class="sxs-lookup"><span data-stu-id="bd6d7-168">Describe an application</span></span>
<span data-ttu-id="bd6d7-169">manifest aplikace Hello deklarativně popisuje hello typ a verze aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-169">hello application manifest declaratively describes hello application type and version.</span></span> <span data-ttu-id="bd6d7-170">Určuje metadata služby složení například stabilní názvy, vytváření oddílů schématu, faktor počet/replikace instance, zásady zabezpečení nebo izolace, omezení umístění, konfigurace přepsání a typů základní služby.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-170">It specifies service composition metadata such as stable names, partitioning scheme, instance count/replication factor, security/isolation policy, placement constraints, configuration overrides, and constituent service types.</span></span> <span data-ttu-id="bd6d7-171">Hello Vyrovnávání zatížení domén do které se umístí hello aplikace jsou také popsány.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-171">hello load-balancing domains into which hello application is placed are also described.</span></span>

<span data-ttu-id="bd6d7-172">Proto manifest aplikace popisuje elementy na úrovni aplikace hello a odkazuje na jeden nebo více služby manifesty toocompose typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-172">Thus, an application manifest describes elements at hello application level and references one or more service manifests toocompose an application type.</span></span> <span data-ttu-id="bd6d7-173">Zde je jednoduchý příklad manifest aplikace:</span><span class="sxs-lookup"><span data-stu-id="bd6d7-173">Here is a simple example application manifest:</span></span>

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

<span data-ttu-id="bd6d7-174">Jako služba manifesty **verze** atributy jsou nestrukturovaných řetězce a nejsou analyzovat hello systému.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-174">Like service manifests, **Version** attributes are unstructured strings and are not parsed by hello system.</span></span> <span data-ttu-id="bd6d7-175">Verze atributy jsou také použít tooversion jednotlivé komponenty pro upgrade.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-175">Version attributes are also used tooversion each component for upgrades.</span></span>

<span data-ttu-id="bd6d7-176">**ServiceManifestImport** obsahuje manifesty tooservice odkazy, které tvoří tento typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-176">**ServiceManifestImport** contains references tooservice manifests that compose this application type.</span></span> <span data-ttu-id="bd6d7-177">Manifesty importované služba zjistit, jaké typy služby jsou platné v rámci tento typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-177">Imported service manifests determine what service types are valid within this application type.</span></span> <span data-ttu-id="bd6d7-178">V rámci hello ServiceManifestImport přepsat hodnoty konfigurace v souborech Settings.xml a prostředí proměnné v ServiceManifest.xml soubory.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-178">Within hello ServiceManifestImport, you override configuration values in Settings.xml and environment variables in ServiceManifest.xml files.</span></span> 


<span data-ttu-id="bd6d7-179">**DefaultServices** deklaruje instance služby, které se automaticky vytvoří vždy, když aplikace je vytvořena instance pro tento typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-179">**DefaultServices** declares service instances that are automatically created whenever an application is instantiated against this application type.</span></span> <span data-ttu-id="bd6d7-180">Výchozí služby jsou pouze pro vaše pohodlí a chovají se jako normální služby, každý po jejich vytvoření.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-180">Default services are just a convenience and behave like normal services in every respect after they have been created.</span></span> <span data-ttu-id="bd6d7-181">Tyto jsou upgradovány spolu s jinými službami v instanci aplikace hello a může být odebrán také.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-181">They are upgraded along with any other services in hello application instance and can be removed as well.</span></span>

> [!NOTE]
> <span data-ttu-id="bd6d7-182">Manifest aplikace může obsahovat více manifestu importy služby a výchozí služby.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-182">An application manifest can contain multiple service manifest imports and default services.</span></span> <span data-ttu-id="bd6d7-183">Každý importu manifestu služby může být nezávisle verzí.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-183">Each service manifest import can be versioned independently.</span></span>
> 
> 

<span data-ttu-id="bd6d7-184">jak toomaintain jinou aplikaci a parametry služby pro jednotlivá prostředí, najdete v části toolearn [Správa parametry aplikace pro prostředí s více](service-fabric-manage-multiple-environment-app-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="bd6d7-184">toolearn how toomaintain different application and service parameters for individual environments, see [Managing application parameters for multiple environments](service-fabric-manage-multiple-environment-app-configuration.md).</span></span>

<!--
For more information about other features supported by application manifests, refer toohello following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a><span data-ttu-id="bd6d7-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bd6d7-185">Next steps</span></span>
<span data-ttu-id="bd6d7-186">[Balíček aplikace](service-fabric-package-apps.md) a nastavit jej připraveni toodeploy.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-186">[Package an application](service-fabric-package-apps.md) and make it ready toodeploy.</span></span>

<span data-ttu-id="bd6d7-187">[Nasazení a odebírat aplikace] [ 10] popisuje, jak instancemi aplikace toomanage toouse prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-187">[Deploy and remove applications][10] describes how toouse PowerShell toomanage application instances.</span></span>

<span data-ttu-id="bd6d7-188">[Správa aplikací parametry pro prostředí s více] [ 11] popisuje, jak tooconfigure parametrů a proměnných prostředí pro instance jinou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-188">[Managing application parameters for multiple environments][11] describes how tooconfigure parameters and environment variables for different application instances.</span></span>

<span data-ttu-id="bd6d7-189">[Konfigurovat zásady zabezpečení pro vaši aplikaci] [ 12] popisuje, jak toorun služeb v rámci přístup toorestrict zásady zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="bd6d7-189">[Configure security policies for your application][12] describes how toorun services under security policies toorestrict access.</span></span>

<span data-ttu-id="bd6d7-190">[Aplikace hostující modely] [ 13] popisují vztah mezi nasazená služba a služba hostitelský proces repliky (nebo instance).</span><span class="sxs-lookup"><span data-stu-id="bd6d7-190">[Application hosting models][13] describe relationship between replicas (or instances) of a deployed service and service-host process.</span></span>

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
[13]: service-fabric-hosting-model.md
