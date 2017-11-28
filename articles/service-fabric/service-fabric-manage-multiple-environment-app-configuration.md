---
title: "aaaManage prostředí s více v Service Fabric | Microsoft Docs"
description: "Service Fabric aplikace můžete spustit na clustery, které rozsahu velikost z jednoho počítače toothousands počítačů. V některých případech můžete tooconfigure aplikace pro tato rozmanitých prostředí. Tento článek popisuje jak toodefine jinou aplikaci parametry podle prostředí."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: f406eac9-7271-4c37-a0d3-0a2957b60537
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: mikkelhegn
ms.openlocfilehash: 2b3327e0e1a3bbd35a50835e720619f308b1b501
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-application-parameters-for-multiple-environments"></a><span data-ttu-id="0a74a-105">Spravovat aplikace parametry pro prostředí s více</span><span class="sxs-lookup"><span data-stu-id="0a74a-105">Manage application parameters for multiple environments</span></span>
<span data-ttu-id="0a74a-106">Můžete vytvořit clustery Azure Service Fabric pomocí kdekoli z jednoho toomany tisíc počítačů.</span><span class="sxs-lookup"><span data-stu-id="0a74a-106">You can create Azure Service Fabric clusters by using anywhere from one toomany thousands of machines.</span></span> <span data-ttu-id="0a74a-107">Při binární soubory aplikace můžete spustit bez úprav přes tento široké spektrum prostředí, často chcete tooconfigure hello aplikace odlišně, v závislosti na hello počtu počítačů, které nasazujete na.</span><span class="sxs-lookup"><span data-stu-id="0a74a-107">While application binaries can run without modification across this wide spectrum of environments, you often want tooconfigure hello application differently, depending on hello number of machines you're deploying to.</span></span>

<span data-ttu-id="0a74a-108">Jako jednoduchý příklad, zvažte `InstanceCount` bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="0a74a-108">As a simple example, consider `InstanceCount` for a stateless service.</span></span> <span data-ttu-id="0a74a-109">Když aplikace běží v Azure, obvykle chcete tooset tento parametr toohello speciální hodnotu-1.</span><span class="sxs-lookup"><span data-stu-id="0a74a-109">When you are running applications in Azure, you generally want tooset this parameter toohello special value of "-1".</span></span> <span data-ttu-id="0a74a-110">Tato konfigurace zajistí, že vaše služba běží na všech uzlech v clusteru hello (nebo každý uzel v uzlu typu hello Pokud jste nastavili omezení umístění).</span><span class="sxs-lookup"><span data-stu-id="0a74a-110">This configuration ensures that your service is running on every node in hello cluster (or every node in hello node type if you have set a placement constraint).</span></span> <span data-ttu-id="0a74a-111">Tato konfigurace však není vhodný pro cluster jednoho počítače, protože nemůže mít více procesy naslouchání na hello stejný koncový bod na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="0a74a-111">However, this configuration is not suitable for a single-machine cluster since you cannot have multiple processes listening on hello same endpoint on a single machine.</span></span> <span data-ttu-id="0a74a-112">Místo toho je obvykle nastavit `InstanceCount` příliš "1".</span><span class="sxs-lookup"><span data-stu-id="0a74a-112">Instead, you typically set `InstanceCount` too"1".</span></span>

## <a name="specifying-environment-specific-parameters"></a><span data-ttu-id="0a74a-113">Zadání parametrů specifické pro prostředí</span><span class="sxs-lookup"><span data-stu-id="0a74a-113">Specifying environment-specific parameters</span></span>
<span data-ttu-id="0a74a-114">problém s konfigurací toothis řešení Hello je sada parametrizované výchozích služeb a soubory parametrů aplikace, které zadejte tyto hodnoty parametrů pro dané prostředí.</span><span class="sxs-lookup"><span data-stu-id="0a74a-114">hello solution toothis configuration issue is a set of parameterized default services and application parameter files that fill in those parameter values for a given environment.</span></span> <span data-ttu-id="0a74a-115">Výchozí služby a aplikace parametry jsou nakonfigurovaní v hello aplikace a služby manifesty.</span><span class="sxs-lookup"><span data-stu-id="0a74a-115">Default services and application parameters are configured in hello application and service manifests.</span></span> <span data-ttu-id="0a74a-116">Hello definice schématu hello ServiceManifest.xml a ApplicationManifest.xml souborů se instaluje s hello Service Fabric SDK a nástrojů pro příliš*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="0a74a-116">hello schema definition for hello ServiceManifest.xml and ApplicationManifest.xml files is installed with hello Service Fabric SDK and tools too*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

### <a name="default-services"></a><span data-ttu-id="0a74a-117">Výchozí služby</span><span class="sxs-lookup"><span data-stu-id="0a74a-117">Default services</span></span>
<span data-ttu-id="0a74a-118">Aplikace Service Fabric se skládají z kolekce instancí služby.</span><span class="sxs-lookup"><span data-stu-id="0a74a-118">Service Fabric applications are made up of a collection of service instances.</span></span> <span data-ttu-id="0a74a-119">Když je možné, toocreate prázdnou aplikaci a pak vytvořte všechny instance služby dynamicky, většina aplikací mít sadu základní služby, které mají být vytvořeny vždy při vytváření instance aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="0a74a-119">While it is possible for you toocreate an empty application and then create all service instances dynamically, most applications have a set of core services that should always be created when hello application is instantiated.</span></span> <span data-ttu-id="0a74a-120">Jedná se o odkazované tooas "výchozí služby".</span><span class="sxs-lookup"><span data-stu-id="0a74a-120">These are referred tooas "default services".</span></span> <span data-ttu-id="0a74a-121">Jsou uvedené v manifestu aplikace hello se zástupnými symboly pro konfiguraci za prostředí, které jsou součástí hranaté závorky:</span><span class="sxs-lookup"><span data-stu-id="0a74a-121">They are specified in hello application manifest, with placeholders for per-environment configuration included in square brackets:</span></span>

```xml
  <DefaultServices>
      <Service Name="Stateful1">
          <StatefulService
              ServiceTypeName="Stateful1Type"
              TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
              MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

              <UniformInt64Partition
                  PartitionCount="[Stateful1_PartitionCount]"
                  LowKey="-9223372036854775808"
                  HighKey="9223372036854775807"
              />
        </StatefulService>
    </Service>
  </DefaultServices>
```

<span data-ttu-id="0a74a-122">Každý hello pojmenované parametry musí být definován v rámci elementu parametry hello manifest aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="0a74a-122">Each of hello named parameters must be defined within hello Parameters element of hello application manifest:</span></span>

```xml
    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>
```

<span data-ttu-id="0a74a-123">DefaultValue – atribut Hello určuje toobe hodnota hello používá hello neexistence parametr informace specifické pro dané prostředí.</span><span class="sxs-lookup"><span data-stu-id="0a74a-123">hello DefaultValue attribute specifies hello value toobe used in hello absence of a more-specific parameter for a given environment.</span></span>

> [!NOTE]
> <span data-ttu-id="0a74a-124">Všechny parametry instance služby jsou vhodné pro konfiguraci podle prostředí.</span><span class="sxs-lookup"><span data-stu-id="0a74a-124">Not all service instance parameters are suitable for per-environment configuration.</span></span> <span data-ttu-id="0a74a-125">V předchozím příkladu hello hello LowKey a HighKey hodnoty pro schéma rozdělení oddílů hello služby jsou explicitně definovány pro všechny instance služby hello vzhledem k tomu, že rozsah oddílu hello je funkce hello data domény, ne hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="0a74a-125">In hello example above, hello LowKey and HighKey values for hello service's partitioning scheme are explicitly defined for all instances of hello service since hello partition range is a function of hello data domain, not hello environment.</span></span>
> 
> 

### <a name="per-environment-service-configuration-settings"></a><span data-ttu-id="0a74a-126">Nastavení konfigurace služby za prostředí</span><span class="sxs-lookup"><span data-stu-id="0a74a-126">Per-environment service configuration settings</span></span>
<span data-ttu-id="0a74a-127">Hello [model aplikace Service Fabric](service-fabric-application-model.md) umožňuje služby tooinclude konfigurace balíčky, které obsahují vlastní páry klíč hodnota, které jsou v době běhu čitelné.</span><span class="sxs-lookup"><span data-stu-id="0a74a-127">hello [Service Fabric application model](service-fabric-application-model.md) enables services tooinclude configuration packages that contain custom key-value pairs that are readable at run time.</span></span> <span data-ttu-id="0a74a-128">Hello hodnoty těchto nastavení můžete také rozlišené pomocí prostředí tak, že zadáte `ConfigOverride` v manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="0a74a-128">hello values of these settings can also be differentiated by environment by specifying a `ConfigOverride` in hello application manifest.</span></span>

<span data-ttu-id="0a74a-129">Předpokládejme, že máte následující nastavení v souboru Config\Settings.xml hello hello hello `Stateful1` služby:</span><span class="sxs-lookup"><span data-stu-id="0a74a-129">Suppose that you have hello following setting in hello Config\Settings.xml file for hello `Stateful1` service:</span></span>

```xml
  <Section Name="MyConfigSection">
     <Parameter Name="MaxQueueSize" Value="25" />
  </Section>
```
<span data-ttu-id="0a74a-130">vytvoření této hodnoty pro dvojici konkrétní aplikaci nebo prostředí toooverride `ConfigOverride` při importu hello service manifest v manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="0a74a-130">toooverride this value for a specific application/environment pair, create a `ConfigOverride` when you import hello service manifest in hello application manifest.</span></span>

```xml
  <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>
```
<span data-ttu-id="0a74a-131">Tento parametr můžete pak nakonfigurovat prostředí, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="0a74a-131">This parameter can then be configured by environment as shown above.</span></span> <span data-ttu-id="0a74a-132">To provedete tak, že deklarace v části Parametry hello hello manifest aplikace a zadání hodnoty v závislosti na prostředí v soubory parametrů aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="0a74a-132">You can do this by declaring it in hello parameters section of hello application manifest and specifying environment-specific values in hello application parameter files.</span></span>

> [!NOTE]
> <span data-ttu-id="0a74a-133">V případě hello nastavení konfigurace služby jsou tři místa, kde můžete nastavit hello hodnoty klíče: hello balíček konfigurace služby, manifest aplikace hello a soubor parametrů aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="0a74a-133">In hello case of service configuration settings, there are three places where hello value of a key can be set: hello service configuration package, hello application manifest, and hello application parameter file.</span></span> <span data-ttu-id="0a74a-134">Service Fabric se vždycky zvolte ze souboru parametrů aplikace hello nejprve (Pokud je zadána), pak hello manifest aplikace a nakonec hello konfigurační balíček.</span><span class="sxs-lookup"><span data-stu-id="0a74a-134">Service Fabric will always choose from hello application parameter file first (if specified), then hello application manifest, and finally hello configuration package.</span></span>
> 
> 

### <a name="setting-and-using-environment-variables"></a><span data-ttu-id="0a74a-135">Nastavení a použití proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="0a74a-135">Setting and using environment variables</span></span> 
<span data-ttu-id="0a74a-136">Můžete zadat a nastavení proměnných prostředí v souboru ServiceManifest.xml hello a pak přepsat hello ApplicationManifest.xml do souboru na základě za instance.</span><span class="sxs-lookup"><span data-stu-id="0a74a-136">You can specify and set environment variables in hello ServiceManifest.xml file and then override these in hello ApplicationManifest.xml file on a per instance basis.</span></span>
<span data-ttu-id="0a74a-137">Hello níže uvedený příklad dvou proměnných prostředí, jeden s hodnotou nastavte a hello jiných přepsána.</span><span class="sxs-lookup"><span data-stu-id="0a74a-137">hello example below shows two environment variables, one with a value set and hello other is overridden.</span></span> <span data-ttu-id="0a74a-138">Parametry aplikačního proměnné prostředí tooset hodnoty v hello stejný způsobem, že tyto se používaly k přepsání konfigurace můžete použít.</span><span class="sxs-lookup"><span data-stu-id="0a74a-138">You can use application parameters tooset environment variables values in hello same way that these were used for config overrides.</span></span>

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
<span data-ttu-id="0a74a-139">proměnné prostředí hello toooverride v hello ApplicationManifest.xml, odkaz na balíček kódu hello v hello ServiceManifest s hello `EnvironmentOverrides` elementu.</span><span class="sxs-lookup"><span data-stu-id="0a74a-139">toooverride hello environment variables in hello ApplicationManifest.xml, reference hello code package in hello ServiceManifest with hello `EnvironmentOverrides` element.</span></span>

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="MyCode">
      <EnvironmentVariable Name="MyEnvVariable" Value="mydata"/>
    </EnvironmentOverrides>
  </ServiceManifestImport>
 ``` 
 <span data-ttu-id="0a74a-140">Po vytvoření hello s názvem instance služby je přístup k proměnné prostředí hello z kódu.</span><span class="sxs-lookup"><span data-stu-id="0a74a-140">Once hello named service instance is created you can access hello environment variables from code.</span></span> <span data-ttu-id="0a74a-141">například v C# můžete provést následující hello</span><span class="sxs-lookup"><span data-stu-id="0a74a-141">e.g. In C# you can do hello following</span></span>

```csharp
    string EnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

### <a name="service-fabric-environment-variables"></a><span data-ttu-id="0a74a-142">Proměnné prostředí Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0a74a-142">Service Fabric environment variables</span></span>
<span data-ttu-id="0a74a-143">Service Fabric má vestavěné proměnné prostředí, nastavte pro každou instanci služby.</span><span class="sxs-lookup"><span data-stu-id="0a74a-143">Service Fabric has built in environment variables set for each service instance.</span></span> <span data-ttu-id="0a74a-144">Hello úplný seznam proměnných prostředí je níže, kde hello těch, které jsou v tučné jsou hello šablony, které budete používat ve službě, hello jiné se používá modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="0a74a-144">hello full list of environment variables is below, where hello ones in bold are hello ones that you will use in your service, hello other being used by Service Fabric runtime.</span></span> 

* <span data-ttu-id="0a74a-145">Fabric_ApplicationHostId</span><span class="sxs-lookup"><span data-stu-id="0a74a-145">Fabric_ApplicationHostId</span></span>
* <span data-ttu-id="0a74a-146">Fabric_ApplicationHostType</span><span class="sxs-lookup"><span data-stu-id="0a74a-146">Fabric_ApplicationHostType</span></span>
* <span data-ttu-id="0a74a-147">Fabric_ApplicationId</span><span class="sxs-lookup"><span data-stu-id="0a74a-147">Fabric_ApplicationId</span></span>
* <span data-ttu-id="0a74a-148">**Fabric_ApplicationName**</span><span class="sxs-lookup"><span data-stu-id="0a74a-148">**Fabric_ApplicationName**</span></span>
* <span data-ttu-id="0a74a-149">Fabric_CodePackageInstanceId</span><span class="sxs-lookup"><span data-stu-id="0a74a-149">Fabric_CodePackageInstanceId</span></span>
* <span data-ttu-id="0a74a-150">**Fabric_CodePackageName**</span><span class="sxs-lookup"><span data-stu-id="0a74a-150">**Fabric_CodePackageName**</span></span>
* <span data-ttu-id="0a74a-151">**TypeEndpoint Fabric_Endpoint_ [YourServiceName]**</span><span class="sxs-lookup"><span data-stu-id="0a74a-151">**Fabric_Endpoint_[YourServiceName]TypeEndpoint**</span></span>
* <span data-ttu-id="0a74a-152">**Fabric_Folder_App_Log**</span><span class="sxs-lookup"><span data-stu-id="0a74a-152">**Fabric_Folder_App_Log**</span></span>
* <span data-ttu-id="0a74a-153">**Fabric_Folder_App_Temp**</span><span class="sxs-lookup"><span data-stu-id="0a74a-153">**Fabric_Folder_App_Temp**</span></span>
* <span data-ttu-id="0a74a-154">**Fabric_Folder_App_Work**</span><span class="sxs-lookup"><span data-stu-id="0a74a-154">**Fabric_Folder_App_Work**</span></span>
* <span data-ttu-id="0a74a-155">**Fabric_Folder_Application**</span><span class="sxs-lookup"><span data-stu-id="0a74a-155">**Fabric_Folder_Application**</span></span>
* <span data-ttu-id="0a74a-156">Fabric_NodeId</span><span class="sxs-lookup"><span data-stu-id="0a74a-156">Fabric_NodeId</span></span>
* <span data-ttu-id="0a74a-157">**Fabric_NodeIPOrFQDN**</span><span class="sxs-lookup"><span data-stu-id="0a74a-157">**Fabric_NodeIPOrFQDN**</span></span>
* <span data-ttu-id="0a74a-158">**Fabric_NodeName**</span><span class="sxs-lookup"><span data-stu-id="0a74a-158">**Fabric_NodeName**</span></span>
* <span data-ttu-id="0a74a-159">Fabric_RuntimeConnectionAddress</span><span class="sxs-lookup"><span data-stu-id="0a74a-159">Fabric_RuntimeConnectionAddress</span></span>
* <span data-ttu-id="0a74a-160">Fabric_ServicePackageInstanceId</span><span class="sxs-lookup"><span data-stu-id="0a74a-160">Fabric_ServicePackageInstanceId</span></span>
* <span data-ttu-id="0a74a-161">Fabric_ServicePackageName</span><span class="sxs-lookup"><span data-stu-id="0a74a-161">Fabric_ServicePackageName</span></span>
* <span data-ttu-id="0a74a-162">Fabric_ServicePackageVersionInstance</span><span class="sxs-lookup"><span data-stu-id="0a74a-162">Fabric_ServicePackageVersionInstance</span></span>
* <span data-ttu-id="0a74a-163">FabricPackageFileName</span><span class="sxs-lookup"><span data-stu-id="0a74a-163">FabricPackageFileName</span></span>

<span data-ttu-id="0a74a-164">belows Hello kód ukazuje, jak toolist hello proměnné prostředí Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0a74a-164">hello code belows shows how toolist hello Service Fabric environment variables</span></span>
 ```csharp
    foreach (DictionaryEntry de in Environment.GetEnvironmentVariables())
    {
        if (de.Key.ToString().StartsWith("Fabric"))
        {
            Console.WriteLine(" Environment variable {0} = {1}", de.Key, de.Value);
        }
    }
```
<span data-ttu-id="0a74a-165">Hello Následují příklady proměnných prostředí pro typ aplikace volá `GuestExe.Application` volat s typem služby `FrontEndService` při spuštění v místním vývojářském počítači.</span><span class="sxs-lookup"><span data-stu-id="0a74a-165">hello following are examples of environment variables for an application type called `GuestExe.Application` with a service type called `FrontEndService` when run on your local dev machine.</span></span>

* <span data-ttu-id="0a74a-166">**Fabric_ApplicationName = fabric:/GuestExe.Application**</span><span class="sxs-lookup"><span data-stu-id="0a74a-166">**Fabric_ApplicationName = fabric:/GuestExe.Application**</span></span>
* <span data-ttu-id="0a74a-167">**Fabric_CodePackageName = kód**</span><span class="sxs-lookup"><span data-stu-id="0a74a-167">**Fabric_CodePackageName = Code**</span></span>
* <span data-ttu-id="0a74a-168">**Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**</span><span class="sxs-lookup"><span data-stu-id="0a74a-168">**Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**</span></span>
* <span data-ttu-id="0a74a-169">**Fabric_NodeIPOrFQDN = localhost**</span><span class="sxs-lookup"><span data-stu-id="0a74a-169">**Fabric_NodeIPOrFQDN = localhost**</span></span>
* <span data-ttu-id="0a74a-170">**Fabric_NodeName = to uzel _Node_2**</span><span class="sxs-lookup"><span data-stu-id="0a74a-170">**Fabric_NodeName = _Node_2**</span></span>

### <a name="application-parameter-files"></a><span data-ttu-id="0a74a-171">Soubory parametrů aplikace</span><span class="sxs-lookup"><span data-stu-id="0a74a-171">Application parameter files</span></span>
<span data-ttu-id="0a74a-172">projekt aplikace Hello Service Fabric může obsahovat jeden nebo více soubory parametrů aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a74a-172">hello Service Fabric application project can include one or more application parameter files.</span></span> <span data-ttu-id="0a74a-173">Každý z nich definuje hello konkrétní hodnoty pro parametry hello, které jsou definovány v manifestu aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="0a74a-173">Each of them defines hello specific values for hello parameters that are defined in hello application manifest:</span></span>

```xml
    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="3" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>
```
<span data-ttu-id="0a74a-174">Ve výchozím nastavení zahrnuje tři soubory parametrů aplikace s názvem Local.1Node.xml, Local.5Node.xml a Cloud.xml nové aplikace:</span><span class="sxs-lookup"><span data-stu-id="0a74a-174">By default, a new application includes three application parameter files, named Local.1Node.xml, Local.5Node.xml, and Cloud.xml:</span></span>

![Soubory parametrů aplikace v Průzkumníku řešení][app-parameters-solution-explorer]

<span data-ttu-id="0a74a-176">toocreate soubor parametru jednoduše zkopírujte a vložte stávající a dejte mu nový název.</span><span class="sxs-lookup"><span data-stu-id="0a74a-176">toocreate a parameter file, simply copy and paste an existing one and give it a new name.</span></span>

## <a name="identifying-environment-specific-parameters-during-deployment"></a><span data-ttu-id="0a74a-177">Identifikace konkrétní prostředí parametrů během nasazování</span><span class="sxs-lookup"><span data-stu-id="0a74a-177">Identifying environment-specific parameters during deployment</span></span>
<span data-ttu-id="0a74a-178">Při nasazení je nutné toochoose hello odpovídající parametr souboru tooapply s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="0a74a-178">At deployment time, you need toochoose hello appropriate parameter file tooapply with your application.</span></span> <span data-ttu-id="0a74a-179">Můžete to provést prostřednictvím dialogu hello publikovat v sadě Visual Studio nebo pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0a74a-179">You can do this through hello Publish dialog in Visual Studio or through PowerShell.</span></span>

### <a name="deploy-from-visual-studio"></a><span data-ttu-id="0a74a-180">Nasazení z Visual Studia</span><span class="sxs-lookup"><span data-stu-id="0a74a-180">Deploy from Visual Studio</span></span>
<span data-ttu-id="0a74a-181">Můžete zvolit z hello seznam souborů k dispozici parametr při publikování aplikace v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0a74a-181">You can choose from hello list of available parameter files when you publish your application in Visual Studio.</span></span>

![Vyberte soubor s parametry v dialogovém okně Publikovat hello][publishdialog]

### <a name="deploy-from-powershell"></a><span data-ttu-id="0a74a-183">Nasazení z prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="0a74a-183">Deploy from PowerShell</span></span>
<span data-ttu-id="0a74a-184">Hello `Deploy-FabricApplication.ps1` skript prostředí PowerShell, které jsou součástí šablony projektu aplikace hello přijme profil publikování jako parametr a hello PublishProfile obsahuje soubor odkaz na aplikaci toohello parametry.</span><span class="sxs-lookup"><span data-stu-id="0a74a-184">hello `Deploy-FabricApplication.ps1` PowerShell script included in hello application project template accepts a publish profile as a parameter and hello PublishProfile contains a reference toohello application parameters file.</span></span>

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a><span data-ttu-id="0a74a-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0a74a-185">Next steps</span></span>
<span data-ttu-id="0a74a-186">toolearn Další informace o některých hello základní koncepty, které jsou popsané v tomto tématu najdete v části hello [Service Fabric technický přehled](service-fabric-technical-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0a74a-186">toolearn more about some of hello core concepts that are discussed in this topic, see hello [Service Fabric technical overview](service-fabric-technical-overview.md).</span></span> <span data-ttu-id="0a74a-187">Informace o dalším funkcím správy aplikace, které jsou k dispozici v sadě Visual Studio najdete v tématu [spravovat aplikace Service Fabric v sadě Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="0a74a-187">For information about other app management capabilities that are available in Visual Studio, see [Manage your Service Fabric applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span></span>

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
