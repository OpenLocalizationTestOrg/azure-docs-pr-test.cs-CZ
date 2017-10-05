---
title: "Určení koncové body služby Service Fabric | Microsoft Docs"
description: "Postupy popisují koncový bod prostředků v service manifest, včetně toho, jak nastavit koncové body HTTPS"
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: da36cbdb-6531-4dae-88e8-a311ab71520d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: subramar
ms.openlocfilehash: 08141edfbc8be9bf7bf303419e1e482d5f884860
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="specify-resources-in-a-service-manifest"></a><span data-ttu-id="531b5-103">Zadání prostředků v manifestu služby</span><span class="sxs-lookup"><span data-stu-id="531b5-103">Specify resources in a service manifest</span></span>
## <a name="overview"></a><span data-ttu-id="531b5-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="531b5-104">Overview</span></span>
<span data-ttu-id="531b5-105">Manifest služby umožňuje prostředky, které používají službu být deklarován či změnit beze změny zkompilovaný kód.</span><span class="sxs-lookup"><span data-stu-id="531b5-105">The service manifest allows resources that are used by the service to be declared/changed without changing the compiled code.</span></span> <span data-ttu-id="531b5-106">Azure Service Fabric podporuje konfiguraci prostředků koncový bod pro službu.</span><span class="sxs-lookup"><span data-stu-id="531b5-106">Azure Service Fabric supports configuration of endpoint resources for the service.</span></span> <span data-ttu-id="531b5-107">Přístup k prostředkům, které jsou určené v service manifest lze řídit prostřednictvím objektu SecurityGroup v manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="531b5-107">The access to the resources that are specified in the service manifest can be controlled via the SecurityGroup in the application manifest.</span></span> <span data-ttu-id="531b5-108">Prohlášení o prostředky umožňuje tyto prostředky se musí změnit v době nasazení, což znamená, že služba nepotřebuje zavést nové mechanismus konfigurace.</span><span class="sxs-lookup"><span data-stu-id="531b5-108">The declaration of resources allows these resources to be changed at deployment time, meaning the service doesn't need to introduce a new configuration mechanism.</span></span> <span data-ttu-id="531b5-109">Definice schématu pro soubor ServiceManifest.xml je nainstalován pomocí Service Fabric SDK a nástroje k *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="531b5-109">The schema definition for the ServiceManifest.xml file is installed with the Service Fabric SDK and tools to *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

## <a name="endpoints"></a><span data-ttu-id="531b5-110">Koncové body</span><span class="sxs-lookup"><span data-stu-id="531b5-110">Endpoints</span></span>
<span data-ttu-id="531b5-111">Pokud prostředek koncového bodu je definován v service manifest, Service Fabric přiřazuje porty z rozsahu portů vyhrazené aplikace, pokud není port určen explicitně.</span><span class="sxs-lookup"><span data-stu-id="531b5-111">When an endpoint resource is defined in the service manifest, Service Fabric assigns ports from the reserved application port range when a port isn't specified explicitly.</span></span> <span data-ttu-id="531b5-112">Například, podívejte se na koncový bod *ServiceEndpoint1* zadané v manifestu fragmentu kódu zadaný po tomto odstavci.</span><span class="sxs-lookup"><span data-stu-id="531b5-112">For example, look at the endpoint *ServiceEndpoint1* specified in the manifest snippet provided after this paragraph.</span></span> <span data-ttu-id="531b5-113">Služby mohou také požadovat specifického portu v prostředku.</span><span class="sxs-lookup"><span data-stu-id="531b5-113">Additionally, services can also request a specific port in a resource.</span></span> <span data-ttu-id="531b5-114">Repliky služby spuštěné v různých uzly lze přiřadit různá čísla portů, zatímco repliky služby spuštěné na stejném uzlu sdílet port.</span><span class="sxs-lookup"><span data-stu-id="531b5-114">Service replicas running on different cluster nodes can be assigned different port numbers, while replicas of a service running on the same node share the port.</span></span> <span data-ttu-id="531b5-115">Repliky služby pak můžete použít tyto porty podle potřeby pro replikaci a naslouchá pro požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="531b5-115">The service replicas can then use these ports as needed for replication and listening for client requests.</span></span>

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

<span data-ttu-id="531b5-116">Odkazovat na [konfigurace stavová spolehlivé služby](service-fabric-reliable-services-configuration.md) číst informace o odkazování na koncové body z nastavení souboru config balíčku souboru (souborech settings.xml).</span><span class="sxs-lookup"><span data-stu-id="531b5-116">Refer to [Configuring stateful Reliable Services](service-fabric-reliable-services-configuration.md) to read more about referencing endpoints from the config package settings file (settings.xml).</span></span>

## <a name="example-specifying-an-http-endpoint-for-your-service"></a><span data-ttu-id="531b5-117">Příklad: určení HTTP koncový bod služby</span><span class="sxs-lookup"><span data-stu-id="531b5-117">Example: specifying an HTTP endpoint for your service</span></span>
<span data-ttu-id="531b5-118">Následující service manifest definuje jeden prostředek koncový bod TCP a dva prostředky koncový bod HTTP v &lt;prostředky&gt; elementu.</span><span class="sxs-lookup"><span data-stu-id="531b5-118">The following service manifest defines one TCP endpoint resource and two HTTP endpoint resources in the &lt;Resources&gt; element.</span></span>

<span data-ttu-id="531b5-119">Koncové body protokolu HTTP jsou automaticky že seznamu ACL by pomocí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="531b5-119">HTTP endpoints are automatically ACL'd by Service Fabric.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         This name must match the string used in the RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by the replicator for replicating the state of your service.
           This endpoint is configured through the ReplicatorSettings config section in the Settings.xml
           file under the ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a><span data-ttu-id="531b5-120">Příklad: určení koncový bod HTTPS pro služby</span><span class="sxs-lookup"><span data-stu-id="531b5-120">Example: specifying an HTTPS endpoint for your service</span></span>
<span data-ttu-id="531b5-121">Protokol HTTPS zajišťuje ověřování na serveru a slouží pro komunikaci klienta se serverem šifrování.</span><span class="sxs-lookup"><span data-stu-id="531b5-121">The HTTPS protocol provides server authentication and is also used for encrypting client-server communication.</span></span> <span data-ttu-id="531b5-122">Pokud chcete povolit protokol HTTPS na služby Service Fabric, zadat protokol v *prostředků -> Koncové body -> koncový bod* oddílu manifest služby, jak je uvedeno výše pro koncový bod *ServiceEndpoint3*.</span><span class="sxs-lookup"><span data-stu-id="531b5-122">To enable HTTPS on your Service Fabric service, specify the protocol in the *Resources -> Endpoints -> Endpoint* section of the service manifest, as shown earlier for the endpoint *ServiceEndpoint3*.</span></span>

> [!NOTE]
> <span data-ttu-id="531b5-123">Protokol služby nelze změnit během upgradu aplikace.</span><span class="sxs-lookup"><span data-stu-id="531b5-123">A service’s protocol cannot be changed during application upgrade.</span></span> <span data-ttu-id="531b5-124">Pokud se změní při upgradu, je narušující změně.</span><span class="sxs-lookup"><span data-stu-id="531b5-124">If it is changed during upgrade, it is a breaking change.</span></span>
> 
> 

<span data-ttu-id="531b5-125">Tady je příklad ApplicationManifest, který je nutné nastavit pro protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="531b5-125">Here is an example ApplicationManifest that you need to set for HTTPS.</span></span> <span data-ttu-id="531b5-126">Kryptografický otisk certifikátu musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="531b5-126">The thumbprint for your certificate must be provided.</span></span> <span data-ttu-id="531b5-127">EndpointRef je odkaz na EndpointResource v ServiceManifest, u kterého jste nastavili protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="531b5-127">The EndpointRef is a reference to EndpointResource in ServiceManifest, for which you set the HTTPS protocol.</span></span> <span data-ttu-id="531b5-128">Můžete přidat více než jeden EndpointCertificate.</span><span class="sxs-lookup"><span data-stu-id="531b5-128">You can add more than one EndpointCertificate.</span></span>  

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_ ]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```

## <a name="overriding-endpoints-in-servicemanifestxml"></a><span data-ttu-id="531b5-129">Přepsání koncových bodů v ServiceManifest.xml</span><span class="sxs-lookup"><span data-stu-id="531b5-129">Overriding Endpoints in ServiceManifest.xml</span></span>

<span data-ttu-id="531b5-130">ApplicationManifest přidejte ResourceOverrides část, která bude na stejné úrovni ConfigOverrides sekci.</span><span class="sxs-lookup"><span data-stu-id="531b5-130">In the ApplicationManifest add a ResourceOverrides section which will be a sibling to ConfigOverrides section.</span></span> <span data-ttu-id="531b5-131">V této části můžete zadat přepsání pro oddíl koncové body v části prostředky zadané v Service manifest.</span><span class="sxs-lookup"><span data-stu-id="531b5-131">In this section you can specify the override for the Endpoints section in the resources section specified in the Service manifest.</span></span>

<span data-ttu-id="531b5-132">Chcete-li přepsat koncového bodu v ServiceManifest pomocí ApplicationParameters změn ApplicationManifest jako následující:</span><span class="sxs-lookup"><span data-stu-id="531b5-132">In order to override EndPoint in ServiceManifest using ApplicationParameters change the ApplicationManifest as following:</span></span>

<span data-ttu-id="531b5-133">V části ServiceManifestImport přidejte novou část "ResourceOverrides"</span><span class="sxs-lookup"><span data-stu-id="531b5-133">In the ServiceManifestImport section add a new section "ResourceOverrides"</span></span>

```xml
<ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <ResourceOverrides>
      <Endpoints>
        <Endpoint Name="ServiceEndpoint" Port="[Port]" Protocol="[Protocol]" Type="[Type]" />
        <Endpoint Name="ServiceEndpoint1" Port="[Port1]" Protocol="[Protocol1] "/>
      </Endpoints>
    </ResourceOverrides>
        <Policies>
           <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint"/>
        </Policies>
  </ServiceManifestImport>
```

<span data-ttu-id="531b5-134">V parametrech přidat níže:</span><span class="sxs-lookup"><span data-stu-id="531b5-134">In the Parameters add below:</span></span>

```xml
  <Parameters>
    <Parameter Name="Port" DefaultValue="" />
    <Parameter Name="Protocol" DefaultValue="" />
    <Parameter Name="Type" DefaultValue="" />
    <Parameter Name="Port1" DefaultValue="" />
    <Parameter Name="Protocol1" DefaultValue="" />
  </Parameters>
```

<span data-ttu-id="531b5-135">Při nasazování aplikace teď můžete předat tyto hodnoty jako ApplicationParameters například:</span><span class="sxs-lookup"><span data-stu-id="531b5-135">While deploying the application now you can pass in these values as ApplicationParameters for example:</span></span>

```powershell
PS C:\> New-ServiceFabricApplication -ApplicationName fabric:/myapp -ApplicationTypeName "AppType" -ApplicationTypeVersion "1.0.0" -ApplicationParameter @{Port='1001'; Protocol='https'; Type='Input'; Port1='2001'; Protocol='http'}
```

<span data-ttu-id="531b5-136">Poznámka: Pokud příslušná hodnota poskytuje pro prázdný ApplicationParameters jsme přejděte zpět na výchozí hodnotu dostupné ServiceManifest pro odpovídající EndPointName.</span><span class="sxs-lookup"><span data-stu-id="531b5-136">Note: If the values provide for the ApplicationParameters is empty we go back to the default value provided in the ServiceManifest for the corresponding EndPointName.</span></span>

<span data-ttu-id="531b5-137">Například:</span><span class="sxs-lookup"><span data-stu-id="531b5-137">For example:</span></span>

<span data-ttu-id="531b5-138">Pokud se v ServiceManifest jste zadali</span><span class="sxs-lookup"><span data-stu-id="531b5-138">If in the ServiceManifest you specified</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint1" Protocol="tcp"/>
    </Endpoints>
  </Resources>
```

<span data-ttu-id="531b5-139">A Port1 a Protocol1 hodnotu pro parametry aplikace je null nebo prázdná.</span><span class="sxs-lookup"><span data-stu-id="531b5-139">And the Port1 and Protocol1 value for Application parameters is null or empty.</span></span> <span data-ttu-id="531b5-140">Port je stále rozhodnuto pomocí ServiceFabric.</span><span class="sxs-lookup"><span data-stu-id="531b5-140">The port is still decided by ServiceFabric.</span></span> <span data-ttu-id="531b5-141">A bude protokol tcp.</span><span class="sxs-lookup"><span data-stu-id="531b5-141">And the Protocol will tcp.</span></span>

<span data-ttu-id="531b5-142">Předpokládejme, že zadáte chybnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="531b5-142">Suppose you specify a wrong value.</span></span> <span data-ttu-id="531b5-143">Například pro Port jste zadali hodnotu řetězce "Foo" namísto typ int.  Nové ServiceFabricApplication příkaz se nezdaří s chybou: Parametr přepisu s názvem 'ServiceEndpoint1' atributu "Port1" v části 'ResourceOverrides' je neplatný.</span><span class="sxs-lookup"><span data-stu-id="531b5-143">Like for Port you specified a string value "Foo" instead of an int.  New-ServiceFabricApplication command will fail with an error : The override parameter with name 'ServiceEndpoint1' attribute 'Port1' in section 'ResourceOverrides' is invalid.</span></span> <span data-ttu-id="531b5-144">Zadaná hodnota je "Foo" a vyžaduje je 'int'.</span><span class="sxs-lookup"><span data-stu-id="531b5-144">The value specified is 'Foo' and required is 'int'.</span></span>
