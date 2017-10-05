---
title: "Správa prostředí s více v Service Fabric | Microsoft Docs"
description: "Service Fabric aplikace můžete spustit na clustery, které rozsahu velikost z jednoho počítače tisíce počítačů. V některých případech můžete ke konfiguraci vaší aplikace pro tato rozmanitých prostředí. Tento článek vysvětluje postup definujte parametry jinou aplikaci na prostředí."
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
ms.openlocfilehash: 9317b3f0b7984e795c4205360ed58e2c4f3fbcb1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="manage-application-parameters-for-multiple-environments"></a><span data-ttu-id="14258-105">Spravovat aplikace parametry pro prostředí s více</span><span class="sxs-lookup"><span data-stu-id="14258-105">Manage application parameters for multiple environments</span></span>
<span data-ttu-id="14258-106">Můžete vytvořit clustery Azure Service Fabric pomocí kdekoli 1 až několika tisíc počítačů.</span><span class="sxs-lookup"><span data-stu-id="14258-106">You can create Azure Service Fabric clusters by using anywhere from one to many thousands of machines.</span></span> <span data-ttu-id="14258-107">Binární soubory aplikace můžete spustit bez úprav přes tento široké spektrum prostředí, ale často chcete nakonfigurovat aplikaci, jinak, v závislosti na počtu počítačů, které nasazujete na.</span><span class="sxs-lookup"><span data-stu-id="14258-107">While application binaries can run without modification across this wide spectrum of environments, you often want to configure the application differently, depending on the number of machines you're deploying to.</span></span>

<span data-ttu-id="14258-108">Jako jednoduchý příklad, zvažte `InstanceCount` bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="14258-108">As a simple example, consider `InstanceCount` for a stateless service.</span></span> <span data-ttu-id="14258-109">Když aplikace běží v Azure, obvykle chcete nastavte tento parametr zvláštní hodnotu-1.</span><span class="sxs-lookup"><span data-stu-id="14258-109">When you are running applications in Azure, you generally want to set this parameter to the special value of "-1".</span></span> <span data-ttu-id="14258-110">Tato konfigurace zajistí, že vaše služba běží na všech uzlech v clusteru (nebo každý uzel v uzlu typu, pokud jste nastavili omezení umístění).</span><span class="sxs-lookup"><span data-stu-id="14258-110">This configuration ensures that your service is running on every node in the cluster (or every node in the node type if you have set a placement constraint).</span></span> <span data-ttu-id="14258-111">Tato konfigurace však není vhodný pro cluster s podporou jeden počítač, protože nemůže mít více procesy naslouchání na stejný koncový bod na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="14258-111">However, this configuration is not suitable for a single-machine cluster since you cannot have multiple processes listening on the same endpoint on a single machine.</span></span> <span data-ttu-id="14258-112">Místo toho je obvykle nastavit `InstanceCount` "1".</span><span class="sxs-lookup"><span data-stu-id="14258-112">Instead, you typically set `InstanceCount` to "1".</span></span>

## <a name="specifying-environment-specific-parameters"></a><span data-ttu-id="14258-113">Zadání parametrů specifické pro prostředí</span><span class="sxs-lookup"><span data-stu-id="14258-113">Specifying environment-specific parameters</span></span>
<span data-ttu-id="14258-114">Řešení, aby tento problém je sada parametrizované výchozích služeb a soubory parametrů aplikace, které zadejte tyto hodnoty parametrů pro dané prostředí.</span><span class="sxs-lookup"><span data-stu-id="14258-114">The solution to this configuration issue is a set of parameterized default services and application parameter files that fill in those parameter values for a given environment.</span></span> <span data-ttu-id="14258-115">Výchozí služby a aplikace parametry jsou nastaveny v manifestů aplikace a služby.</span><span class="sxs-lookup"><span data-stu-id="14258-115">Default services and application parameters are configured in the application and service manifests.</span></span> <span data-ttu-id="14258-116">Definice schématu pro ServiceManifest.xml a ApplicationManifest.xml soubory se instaluje s Service Fabric SDK a nástroje k *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="14258-116">The schema definition for the ServiceManifest.xml and ApplicationManifest.xml files is installed with the Service Fabric SDK and tools to *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

### <a name="default-services"></a><span data-ttu-id="14258-117">Výchozí služby</span><span class="sxs-lookup"><span data-stu-id="14258-117">Default services</span></span>
<span data-ttu-id="14258-118">Aplikace Service Fabric se skládají z kolekce instancí služby.</span><span class="sxs-lookup"><span data-stu-id="14258-118">Service Fabric applications are made up of a collection of service instances.</span></span> <span data-ttu-id="14258-119">I když je možné vytvořit prázdnou aplikaci a pak vytvořit všechny instance služby dynamicky, většina aplikací mít sadu základní služby, které mají být vytvořeny vždy při vytváření instance aplikace.</span><span class="sxs-lookup"><span data-stu-id="14258-119">While it is possible for you to create an empty application and then create all service instances dynamically, most applications have a set of core services that should always be created when the application is instantiated.</span></span> <span data-ttu-id="14258-120">Tyto jsou označovány jako "výchozí služby".</span><span class="sxs-lookup"><span data-stu-id="14258-120">These are referred to as "default services".</span></span> <span data-ttu-id="14258-121">Jsou uvedené v manifestu aplikace, se zástupnými symboly pro konfiguraci za prostředí, které jsou součástí hranaté závorky:</span><span class="sxs-lookup"><span data-stu-id="14258-121">They are specified in the application manifest, with placeholders for per-environment configuration included in square brackets:</span></span>

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

<span data-ttu-id="14258-122">Všechny pojmenované parametry musí být definován v rámci elementu parametry manifestu aplikace:</span><span class="sxs-lookup"><span data-stu-id="14258-122">Each of the named parameters must be defined within the Parameters element of the application manifest:</span></span>

```xml
    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>
```

<span data-ttu-id="14258-123">Atribut DefaultValue Určuje hodnotu, který se má použít při absenci dalších konkrétní parametr pro dané prostředí.</span><span class="sxs-lookup"><span data-stu-id="14258-123">The DefaultValue attribute specifies the value to be used in the absence of a more-specific parameter for a given environment.</span></span>

> [!NOTE]
> <span data-ttu-id="14258-124">Všechny parametry instance služby jsou vhodné pro konfiguraci podle prostředí.</span><span class="sxs-lookup"><span data-stu-id="14258-124">Not all service instance parameters are suitable for per-environment configuration.</span></span> <span data-ttu-id="14258-125">V předchozím příkladu jsou hodnoty LowKey a HighKey pro schéma rozdělení oddílů služby explicitně definovány pro všechny instance služby od rozsahu oddílu je funkce domény data, není v prostředí.</span><span class="sxs-lookup"><span data-stu-id="14258-125">In the example above, the LowKey and HighKey values for the service's partitioning scheme are explicitly defined for all instances of the service since the partition range is a function of the data domain, not the environment.</span></span>
> 
> 

### <a name="per-environment-service-configuration-settings"></a><span data-ttu-id="14258-126">Nastavení konfigurace služby za prostředí</span><span class="sxs-lookup"><span data-stu-id="14258-126">Per-environment service configuration settings</span></span>
<span data-ttu-id="14258-127">[Model aplikace Service Fabric](service-fabric-application-model.md) umožňuje služby zahrnuté balíčky konfigurace, které obsahují vlastní páry klíč hodnota, které jsou v době běhu čitelné.</span><span class="sxs-lookup"><span data-stu-id="14258-127">The [Service Fabric application model](service-fabric-application-model.md) enables services to include configuration packages that contain custom key-value pairs that are readable at run time.</span></span> <span data-ttu-id="14258-128">Hodnoty těchto nastavení můžete také rozlišené pomocí prostředí tak, že zadáte `ConfigOverride` v manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="14258-128">The values of these settings can also be differentiated by environment by specifying a `ConfigOverride` in the application manifest.</span></span>

<span data-ttu-id="14258-129">Předpokládejme, že máte následující nastavení v souboru Config\Settings.xml `Stateful1` služby:</span><span class="sxs-lookup"><span data-stu-id="14258-129">Suppose that you have the following setting in the Config\Settings.xml file for the `Stateful1` service:</span></span>

```xml
  <Section Name="MyConfigSection">
     <Parameter Name="MaxQueueSize" Value="25" />
  </Section>
```
<span data-ttu-id="14258-130">Chcete-li přepsat tuto hodnotu pro konkrétní prostředí aplikace pair, vytvořte `ConfigOverride` při importu service manifest v manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="14258-130">To override this value for a specific application/environment pair, create a `ConfigOverride` when you import the service manifest in the application manifest.</span></span>

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
<span data-ttu-id="14258-131">Tento parametr můžete pak nakonfigurovat prostředí, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="14258-131">This parameter can then be configured by environment as shown above.</span></span> <span data-ttu-id="14258-132">To provedete tak, že deklarace v sekci parametrů manifestu aplikace a zadání hodnoty v závislosti na prostředí v soubory parametrů aplikace.</span><span class="sxs-lookup"><span data-stu-id="14258-132">You can do this by declaring it in the parameters section of the application manifest and specifying environment-specific values in the application parameter files.</span></span>

> [!NOTE]
> <span data-ttu-id="14258-133">V případě nastavení konfigurace služby, jsou tři místa, kde můžete nastavit hodnoty klíče: konfigurační balíček service manifest aplikace a parametr souboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="14258-133">In the case of service configuration settings, there are three places where the value of a key can be set: the service configuration package, the application manifest, and the application parameter file.</span></span> <span data-ttu-id="14258-134">Service Fabric ze souboru aplikace parametr vždycky vybere první (Pokud je zadána), pak manifest aplikace a nakonec konfigurační balíček.</span><span class="sxs-lookup"><span data-stu-id="14258-134">Service Fabric will always choose from the application parameter file first (if specified), then the application manifest, and finally the configuration package.</span></span>
> 
> 

### <a name="setting-and-using-environment-variables"></a><span data-ttu-id="14258-135">Nastavení a použití proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="14258-135">Setting and using environment variables</span></span> 
<span data-ttu-id="14258-136">Můžete zadat a nastavení proměnných prostředí v souboru ServiceManifest.xml a pak přepsání ApplicationManifest.xml souboru na základě za instance.</span><span class="sxs-lookup"><span data-stu-id="14258-136">You can specify and set environment variables in the ServiceManifest.xml file and then override these in the ApplicationManifest.xml file on a per instance basis.</span></span>
<span data-ttu-id="14258-137">Následující příklad ukazuje dvou proměnných prostředí, jeden s nastavenou hodnotu a dalších přepsána.</span><span class="sxs-lookup"><span data-stu-id="14258-137">The example below shows two environment variables, one with a value set and the other is overridden.</span></span> <span data-ttu-id="14258-138">Parametry aplikačního slouží k nastavení hodnot proměnných prostředí stejným způsobem, že tyto se používaly k přepsání konfigurace.</span><span class="sxs-lookup"><span data-stu-id="14258-138">You can use application parameters to set environment variables values in the same way that these were used for config overrides.</span></span>

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
<span data-ttu-id="14258-139">Pokud chcete přepsat proměnné prostředí v ApplicationManifest.xml, odkazují na balíček kódu v ServiceManifest s `EnvironmentOverrides` elementu.</span><span class="sxs-lookup"><span data-stu-id="14258-139">To override the environment variables in the ApplicationManifest.xml, reference the code package in the ServiceManifest with the `EnvironmentOverrides` element.</span></span>

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="MyCode">
      <EnvironmentVariable Name="MyEnvVariable" Value="mydata"/>
    </EnvironmentOverrides>
  </ServiceManifestImport>
 ``` 
 <span data-ttu-id="14258-140">Po vytvoření instance služby s názvem dostanete z kódu proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="14258-140">Once the named service instance is created you can access the environment variables from code.</span></span> <span data-ttu-id="14258-141">například v jazyce C# můžete provést následující</span><span class="sxs-lookup"><span data-stu-id="14258-141">e.g. In C# you can do the following</span></span>

```csharp
    string EnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

### <a name="service-fabric-environment-variables"></a><span data-ttu-id="14258-142">Proměnné prostředí Service Fabric</span><span class="sxs-lookup"><span data-stu-id="14258-142">Service Fabric environment variables</span></span>
<span data-ttu-id="14258-143">Service Fabric má vestavěné proměnné prostředí, nastavte pro každou instanci služby.</span><span class="sxs-lookup"><span data-stu-id="14258-143">Service Fabric has built in environment variables set for each service instance.</span></span> <span data-ttu-id="14258-144">Úplný seznam proměnných prostředí je níže, kde těm, které jsou v bold jsou ty, které budete používat ve službě, druhé používá modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="14258-144">The full list of environment variables is below, where the ones in bold are the ones that you will use in your service, the other being used by Service Fabric runtime.</span></span> 

* <span data-ttu-id="14258-145">Fabric_ApplicationHostId</span><span class="sxs-lookup"><span data-stu-id="14258-145">Fabric_ApplicationHostId</span></span>
* <span data-ttu-id="14258-146">Fabric_ApplicationHostType</span><span class="sxs-lookup"><span data-stu-id="14258-146">Fabric_ApplicationHostType</span></span>
* <span data-ttu-id="14258-147">Fabric_ApplicationId</span><span class="sxs-lookup"><span data-stu-id="14258-147">Fabric_ApplicationId</span></span>
* <span data-ttu-id="14258-148">**Fabric_ApplicationName**</span><span class="sxs-lookup"><span data-stu-id="14258-148">**Fabric_ApplicationName**</span></span>
* <span data-ttu-id="14258-149">Fabric_CodePackageInstanceId</span><span class="sxs-lookup"><span data-stu-id="14258-149">Fabric_CodePackageInstanceId</span></span>
* <span data-ttu-id="14258-150">**Fabric_CodePackageName**</span><span class="sxs-lookup"><span data-stu-id="14258-150">**Fabric_CodePackageName**</span></span>
* <span data-ttu-id="14258-151">**TypeEndpoint Fabric_Endpoint_ [YourServiceName]**</span><span class="sxs-lookup"><span data-stu-id="14258-151">**Fabric_Endpoint_[YourServiceName]TypeEndpoint**</span></span>
* <span data-ttu-id="14258-152">**Fabric_Folder_App_Log**</span><span class="sxs-lookup"><span data-stu-id="14258-152">**Fabric_Folder_App_Log**</span></span>
* <span data-ttu-id="14258-153">**Fabric_Folder_App_Temp**</span><span class="sxs-lookup"><span data-stu-id="14258-153">**Fabric_Folder_App_Temp**</span></span>
* <span data-ttu-id="14258-154">**Fabric_Folder_App_Work**</span><span class="sxs-lookup"><span data-stu-id="14258-154">**Fabric_Folder_App_Work**</span></span>
* <span data-ttu-id="14258-155">**Fabric_Folder_Application**</span><span class="sxs-lookup"><span data-stu-id="14258-155">**Fabric_Folder_Application**</span></span>
* <span data-ttu-id="14258-156">Fabric_NodeId</span><span class="sxs-lookup"><span data-stu-id="14258-156">Fabric_NodeId</span></span>
* <span data-ttu-id="14258-157">**Fabric_NodeIPOrFQDN**</span><span class="sxs-lookup"><span data-stu-id="14258-157">**Fabric_NodeIPOrFQDN**</span></span>
* <span data-ttu-id="14258-158">**Fabric_NodeName**</span><span class="sxs-lookup"><span data-stu-id="14258-158">**Fabric_NodeName**</span></span>
* <span data-ttu-id="14258-159">Fabric_RuntimeConnectionAddress</span><span class="sxs-lookup"><span data-stu-id="14258-159">Fabric_RuntimeConnectionAddress</span></span>
* <span data-ttu-id="14258-160">Fabric_ServicePackageInstanceId</span><span class="sxs-lookup"><span data-stu-id="14258-160">Fabric_ServicePackageInstanceId</span></span>
* <span data-ttu-id="14258-161">Fabric_ServicePackageName</span><span class="sxs-lookup"><span data-stu-id="14258-161">Fabric_ServicePackageName</span></span>
* <span data-ttu-id="14258-162">Fabric_ServicePackageVersionInstance</span><span class="sxs-lookup"><span data-stu-id="14258-162">Fabric_ServicePackageVersionInstance</span></span>
* <span data-ttu-id="14258-163">FabricPackageFileName</span><span class="sxs-lookup"><span data-stu-id="14258-163">FabricPackageFileName</span></span>

<span data-ttu-id="14258-164">Belows kód ukazuje, jak do seznamu proměnných prostředí Service Fabric</span><span class="sxs-lookup"><span data-stu-id="14258-164">The code belows shows how to list the Service Fabric environment variables</span></span>
 ```csharp
    foreach (DictionaryEntry de in Environment.GetEnvironmentVariables())
    {
        if (de.Key.ToString().StartsWith("Fabric"))
        {
            Console.WriteLine(" Environment variable {0} = {1}", de.Key, de.Value);
        }
    }
```
<span data-ttu-id="14258-165">Tady jsou příklady proměnných prostředí pro typ aplikace volá `GuestExe.Application` volat s typem služby `FrontEndService` při spuštění v místním vývojářském počítači.</span><span class="sxs-lookup"><span data-stu-id="14258-165">The following are examples of environment variables for an application type called `GuestExe.Application` with a service type called `FrontEndService` when run on your local dev machine.</span></span>

* <span data-ttu-id="14258-166">**Fabric_ApplicationName = fabric:/GuestExe.Application**</span><span class="sxs-lookup"><span data-stu-id="14258-166">**Fabric_ApplicationName = fabric:/GuestExe.Application**</span></span>
* <span data-ttu-id="14258-167">**Fabric_CodePackageName = kód**</span><span class="sxs-lookup"><span data-stu-id="14258-167">**Fabric_CodePackageName = Code**</span></span>
* <span data-ttu-id="14258-168">**Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**</span><span class="sxs-lookup"><span data-stu-id="14258-168">**Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**</span></span>
* <span data-ttu-id="14258-169">**Fabric_NodeIPOrFQDN = localhost**</span><span class="sxs-lookup"><span data-stu-id="14258-169">**Fabric_NodeIPOrFQDN = localhost**</span></span>
* <span data-ttu-id="14258-170">**Fabric_NodeName = to uzel _Node_2**</span><span class="sxs-lookup"><span data-stu-id="14258-170">**Fabric_NodeName = _Node_2**</span></span>

### <a name="application-parameter-files"></a><span data-ttu-id="14258-171">Soubory parametrů aplikace</span><span class="sxs-lookup"><span data-stu-id="14258-171">Application parameter files</span></span>
<span data-ttu-id="14258-172">Projekt aplikace Service Fabric může obsahovat jeden nebo více soubory parametrů aplikace.</span><span class="sxs-lookup"><span data-stu-id="14258-172">The Service Fabric application project can include one or more application parameter files.</span></span> <span data-ttu-id="14258-173">Každý z nich definuje konkrétní hodnoty pro parametry, které jsou definovány v manifestu aplikace:</span><span class="sxs-lookup"><span data-stu-id="14258-173">Each of them defines the specific values for the parameters that are defined in the application manifest:</span></span>

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
<span data-ttu-id="14258-174">Ve výchozím nastavení zahrnuje tři soubory parametrů aplikace s názvem Local.1Node.xml, Local.5Node.xml a Cloud.xml nové aplikace:</span><span class="sxs-lookup"><span data-stu-id="14258-174">By default, a new application includes three application parameter files, named Local.1Node.xml, Local.5Node.xml, and Cloud.xml:</span></span>

![Soubory parametrů aplikace v Průzkumníku řešení][app-parameters-solution-explorer]

<span data-ttu-id="14258-176">Pokud chcete vytvořit soubor s parametry, jednoduše zkopírujte a vložte stávající a dejte mu nový název.</span><span class="sxs-lookup"><span data-stu-id="14258-176">To create a parameter file, simply copy and paste an existing one and give it a new name.</span></span>

## <a name="identifying-environment-specific-parameters-during-deployment"></a><span data-ttu-id="14258-177">Identifikace konkrétní prostředí parametrů během nasazování</span><span class="sxs-lookup"><span data-stu-id="14258-177">Identifying environment-specific parameters during deployment</span></span>
<span data-ttu-id="14258-178">Při nasazení budete muset zvolit soubor odpovídající parametru pro použití s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="14258-178">At deployment time, you need to choose the appropriate parameter file to apply with your application.</span></span> <span data-ttu-id="14258-179">Můžete to provést prostřednictvím dialogové okno publikování v sadě Visual Studio nebo pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="14258-179">You can do this through the Publish dialog in Visual Studio or through PowerShell.</span></span>

### <a name="deploy-from-visual-studio"></a><span data-ttu-id="14258-180">Nasazení z Visual Studia</span><span class="sxs-lookup"><span data-stu-id="14258-180">Deploy from Visual Studio</span></span>
<span data-ttu-id="14258-181">Seznam dostupných parametrů souborů, které lze vybírat při publikování aplikace v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="14258-181">You can choose from the list of available parameter files when you publish your application in Visual Studio.</span></span>

![Vyberte soubor s parametry v dialogovém okně publikování][publishdialog]

### <a name="deploy-from-powershell"></a><span data-ttu-id="14258-183">Nasazení z prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="14258-183">Deploy from PowerShell</span></span>
<span data-ttu-id="14258-184">`Deploy-FabricApplication.ps1` Skript prostředí PowerShell, které jsou součástí šablony projektu aplikace přijímá profil publikování jako parametr a PublishProfile obsahuje odkaz na soubor parametrů aplikace.</span><span class="sxs-lookup"><span data-stu-id="14258-184">The `Deploy-FabricApplication.ps1` PowerShell script included in the application project template accepts a publish profile as a parameter and the PublishProfile contains a reference to the application parameters file.</span></span>

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a><span data-ttu-id="14258-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="14258-185">Next steps</span></span>
<span data-ttu-id="14258-186">Další informace o některých základní koncepty, které jsou popsané v tomto tématu najdete v tématu [Service Fabric technický přehled](service-fabric-technical-overview.md).</span><span class="sxs-lookup"><span data-stu-id="14258-186">To learn more about some of the core concepts that are discussed in this topic, see the [Service Fabric technical overview](service-fabric-technical-overview.md).</span></span> <span data-ttu-id="14258-187">Informace o dalším funkcím správy aplikace, které jsou k dispozici v sadě Visual Studio najdete v tématu [spravovat aplikace Service Fabric v sadě Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="14258-187">For information about other app management capabilities that are available in Visual Studio, see [Manage your Service Fabric applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span></span>

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
