---
title: "Koncové body služby Service Fabric aaaSpecifying | Microsoft Docs"
description: "Jak toodescribe koncový bod prostředků ve službě manifest, včetně jak tooset až koncových bodů HTTPS"
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
ms.openlocfilehash: a4ebee353ce5cf86583673674246094f03f368be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="specify-resources-in-a-service-manifest"></a><span data-ttu-id="a593e-103">Zadání prostředků v manifestu služby</span><span class="sxs-lookup"><span data-stu-id="a593e-103">Specify resources in a service manifest</span></span>
## <a name="overview"></a><span data-ttu-id="a593e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="a593e-104">Overview</span></span>
<span data-ttu-id="a593e-105">manifest služby Hello umožňuje prostředky, které jsou používány toobe služby hello deklarovaný či změnit beze změny kódu hello zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="a593e-105">hello service manifest allows resources that are used by hello service toobe declared/changed without changing hello compiled code.</span></span> <span data-ttu-id="a593e-106">Azure Service Fabric podporuje konfiguraci prostředků koncový bod služby hello.</span><span class="sxs-lookup"><span data-stu-id="a593e-106">Azure Service Fabric supports configuration of endpoint resources for hello service.</span></span> <span data-ttu-id="a593e-107">Hello přístup toohello prostředky, které jsou určené v hello service manifest lze řídit prostřednictvím hello objektu SecurityGroup v manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a593e-107">hello access toohello resources that are specified in hello service manifest can be controlled via hello SecurityGroup in hello application manifest.</span></span> <span data-ttu-id="a593e-108">deklarace Hello prostředků umožňuje tyto prostředky toobe změnit v době nasazení, což znamená, že služba hello nepotřebuje toointroduce mechanismus nové konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a593e-108">hello declaration of resources allows these resources toobe changed at deployment time, meaning hello service doesn't need toointroduce a new configuration mechanism.</span></span> <span data-ttu-id="a593e-109">Hello definice schématu pro soubor ServiceManifest.xml hello se instaluje s hello Service Fabric SDK a nástrojů pro příliš*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="a593e-109">hello schema definition for hello ServiceManifest.xml file is installed with hello Service Fabric SDK and tools too*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

## <a name="endpoints"></a><span data-ttu-id="a593e-110">Koncové body</span><span class="sxs-lookup"><span data-stu-id="a593e-110">Endpoints</span></span>
<span data-ttu-id="a593e-111">Při prostředek koncového bodu je definována v hello service manifest, Service Fabric přiřazuje porty z rozsahu portů aplikace hello vyhrazené, pokud není port určen explicitně.</span><span class="sxs-lookup"><span data-stu-id="a593e-111">When an endpoint resource is defined in hello service manifest, Service Fabric assigns ports from hello reserved application port range when a port isn't specified explicitly.</span></span> <span data-ttu-id="a593e-112">Například, podívejte se na koncový bod hello *ServiceEndpoint1* zadané v manifestu fragment hello zadaný po tomto odstavci.</span><span class="sxs-lookup"><span data-stu-id="a593e-112">For example, look at hello endpoint *ServiceEndpoint1* specified in hello manifest snippet provided after this paragraph.</span></span> <span data-ttu-id="a593e-113">Služby mohou také požadovat specifického portu v prostředku.</span><span class="sxs-lookup"><span data-stu-id="a593e-113">Additionally, services can also request a specific port in a resource.</span></span> <span data-ttu-id="a593e-114">Repliky služby spuštěné v různých uzly lze přiřadit různá čísla portů, zatímco hello repliky služby spuštěné stejný port hello sdílenou složku v uzlu.</span><span class="sxs-lookup"><span data-stu-id="a593e-114">Service replicas running on different cluster nodes can be assigned different port numbers, while replicas of a service running on hello same node share hello port.</span></span> <span data-ttu-id="a593e-115">repliky služby Hello pak můžete použít tyto porty podle potřeby pro replikaci a naslouchá pro požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="a593e-115">hello service replicas can then use these ports as needed for replication and listening for client requests.</span></span>

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

<span data-ttu-id="a593e-116">Odkazovat příliš[konfigurace stavová spolehlivé služby](service-fabric-reliable-services-configuration.md) tooread více informací o ze souboru nastavení konfigurace balíčku hello (souborech settings.xml) odkazující na koncové body.</span><span class="sxs-lookup"><span data-stu-id="a593e-116">Refer too[Configuring stateful Reliable Services](service-fabric-reliable-services-configuration.md) tooread more about referencing endpoints from hello config package settings file (settings.xml).</span></span>

## <a name="example-specifying-an-http-endpoint-for-your-service"></a><span data-ttu-id="a593e-117">Příklad: určení HTTP koncový bod služby</span><span class="sxs-lookup"><span data-stu-id="a593e-117">Example: specifying an HTTP endpoint for your service</span></span>
<span data-ttu-id="a593e-118">Hello následující service manifest definuje jeden prostředek koncový bod TCP a dva prostředky koncový bod HTTP v hello &lt;prostředky&gt; elementu.</span><span class="sxs-lookup"><span data-stu-id="a593e-118">hello following service manifest defines one TCP endpoint resource and two HTTP endpoint resources in hello &lt;Resources&gt; element.</span></span>

<span data-ttu-id="a593e-119">Koncové body protokolu HTTP jsou automaticky že seznamu ACL by pomocí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a593e-119">HTTP endpoints are automatically ACL'd by Service Fabric.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is hello name of your ServiceType.
         This name must match hello string used in hello RegisterServiceType call in Program.cs. -->
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

  <!-- Config package is hello contents of hello Config directoy under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by hello replicator for replicating hello state of your service.
           This endpoint is configured through hello ReplicatorSettings config section in hello Settings.xml
           file under hello ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a><span data-ttu-id="a593e-120">Příklad: určení koncový bod HTTPS pro služby</span><span class="sxs-lookup"><span data-stu-id="a593e-120">Example: specifying an HTTPS endpoint for your service</span></span>
<span data-ttu-id="a593e-121">Hello protokol HTTPS zajišťuje ověřování na serveru a slouží pro komunikaci klienta se serverem šifrování.</span><span class="sxs-lookup"><span data-stu-id="a593e-121">hello HTTPS protocol provides server authentication and is also used for encrypting client-server communication.</span></span> <span data-ttu-id="a593e-122">tooenable HTTPS na službě Service Fabric, zadat protokol hello v hello *prostředků -> Koncové body -> koncový bod* části hello service manifest, jak je uvedeno výše pro koncový bod hello *ServiceEndpoint3* .</span><span class="sxs-lookup"><span data-stu-id="a593e-122">tooenable HTTPS on your Service Fabric service, specify hello protocol in hello *Resources -> Endpoints -> Endpoint* section of hello service manifest, as shown earlier for hello endpoint *ServiceEndpoint3*.</span></span>

> [!NOTE]
> <span data-ttu-id="a593e-123">Protokol služby nelze změnit během upgradu aplikace.</span><span class="sxs-lookup"><span data-stu-id="a593e-123">A service’s protocol cannot be changed during application upgrade.</span></span> <span data-ttu-id="a593e-124">Pokud se změní při upgradu, je narušující změně.</span><span class="sxs-lookup"><span data-stu-id="a593e-124">If it is changed during upgrade, it is a breaking change.</span></span>
> 
> 

<span data-ttu-id="a593e-125">Tady je příklad ApplicationManifest potřebujete tooset pro protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a593e-125">Here is an example ApplicationManifest that you need tooset for HTTPS.</span></span> <span data-ttu-id="a593e-126">je třeba zadat Hello kryptografický otisk certifikátu.</span><span class="sxs-lookup"><span data-stu-id="a593e-126">hello thumbprint for your certificate must be provided.</span></span> <span data-ttu-id="a593e-127">Hello EndpointRef je odkaz tooEndpointResource v ServiceManifest, u kterého jste nastavili hello protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a593e-127">hello EndpointRef is a reference tooEndpointResource in ServiceManifest, for which you set hello HTTPS protocol.</span></span> <span data-ttu-id="a593e-128">Můžete přidat více než jeden EndpointCertificate.</span><span class="sxs-lookup"><span data-stu-id="a593e-128">You can add more than one EndpointCertificate.</span></span>  

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
  <!-- Import hello ServiceManifest from hello ServicePackage. hello ServiceManifestName and ServiceManifestVersion
       should match hello Name and Version attributes of hello ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- hello section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         hello attribute ServiceTypeName below must match hello name defined in hello imported ServiceManifest.xml file. -->
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

## <a name="overriding-endpoints-in-servicemanifestxml"></a><span data-ttu-id="a593e-129">Přepsání koncových bodů v ServiceManifest.xml</span><span class="sxs-lookup"><span data-stu-id="a593e-129">Overriding Endpoints in ServiceManifest.xml</span></span>

<span data-ttu-id="a593e-130">Hello ApplicationManifest přidejte ResourceOverrides oddíl, který bude tooConfigOverrides oddíl na stejné úrovni.</span><span class="sxs-lookup"><span data-stu-id="a593e-130">In hello ApplicationManifest add a ResourceOverrides section which will be a sibling tooConfigOverrides section.</span></span> <span data-ttu-id="a593e-131">V této části můžete zadat hello přepsání pro oddíl hello koncové body v části prostředky hello zadaný v hello Service manifest.</span><span class="sxs-lookup"><span data-stu-id="a593e-131">In this section you can specify hello override for hello Endpoints section in hello resources section specified in hello Service manifest.</span></span>

<span data-ttu-id="a593e-132">V pořadí toooverride koncového bodu v ServiceManifest pomocí ApplicationParameters změnu hello ApplicationManifest jako následující:</span><span class="sxs-lookup"><span data-stu-id="a593e-132">In order toooverride EndPoint in ServiceManifest using ApplicationParameters change hello ApplicationManifest as following:</span></span>

<span data-ttu-id="a593e-133">V části ServiceManifestImport hello přidejte novou část "ResourceOverrides"</span><span class="sxs-lookup"><span data-stu-id="a593e-133">In hello ServiceManifestImport section add a new section "ResourceOverrides"</span></span>

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

<span data-ttu-id="a593e-134">V hello parametry přidat níže:</span><span class="sxs-lookup"><span data-stu-id="a593e-134">In hello Parameters add below:</span></span>

```xml
  <Parameters>
    <Parameter Name="Port" DefaultValue="" />
    <Parameter Name="Protocol" DefaultValue="" />
    <Parameter Name="Type" DefaultValue="" />
    <Parameter Name="Port1" DefaultValue="" />
    <Parameter Name="Protocol1" DefaultValue="" />
  </Parameters>
```

<span data-ttu-id="a593e-135">Při nasazení aplikace hello teď můžete předat tyto hodnoty jako ApplicationParameters například:</span><span class="sxs-lookup"><span data-stu-id="a593e-135">While deploying hello application now you can pass in these values as ApplicationParameters for example:</span></span>

```powershell
PS C:\> New-ServiceFabricApplication -ApplicationName fabric:/myapp -ApplicationTypeName "AppType" -ApplicationTypeVersion "1.0.0" -ApplicationParameter @{Port='1001'; Protocol='https'; Type='Input'; Port1='2001'; Protocol='http'}
```

<span data-ttu-id="a593e-136">Poznámka: Pokud hello hodnoty poskytují pro prázdný hello ApplicationParameters jsme přejděte zpátky toohello výchozí hodnotu podle hello ServiceManifest pro odpovídající EndPointName hello.</span><span class="sxs-lookup"><span data-stu-id="a593e-136">Note: If hello values provide for hello ApplicationParameters is empty we go back toohello default value provided in hello ServiceManifest for hello corresponding EndPointName.</span></span>

<span data-ttu-id="a593e-137">Například:</span><span class="sxs-lookup"><span data-stu-id="a593e-137">For example:</span></span>

<span data-ttu-id="a593e-138">Pokud se v hello ServiceManifest jste zadali</span><span class="sxs-lookup"><span data-stu-id="a593e-138">If in hello ServiceManifest you specified</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint1" Protocol="tcp"/>
    </Endpoints>
  </Resources>
```

<span data-ttu-id="a593e-139">A hello Port1 a Protocol1 hodnotu pro parametry aplikace je null nebo prázdná.</span><span class="sxs-lookup"><span data-stu-id="a593e-139">And hello Port1 and Protocol1 value for Application parameters is null or empty.</span></span> <span data-ttu-id="a593e-140">Hello port je stále rozhodnuto pomocí ServiceFabric.</span><span class="sxs-lookup"><span data-stu-id="a593e-140">hello port is still decided by ServiceFabric.</span></span> <span data-ttu-id="a593e-141">A hello protokol tcp.</span><span class="sxs-lookup"><span data-stu-id="a593e-141">And hello Protocol will tcp.</span></span>

<span data-ttu-id="a593e-142">Předpokládejme, že zadáte chybnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a593e-142">Suppose you specify a wrong value.</span></span> <span data-ttu-id="a593e-143">Například pro Port jste zadali hodnotu řetězce "Foo" namísto typ int.  Nové ServiceFabricApplication příkaz se nezdaří s chybou: Parametr hello přepisu s názvem 'ServiceEndpoint1' atributu "Port1" v části 'ResourceOverrides' je neplatný.</span><span class="sxs-lookup"><span data-stu-id="a593e-143">Like for Port you specified a string value "Foo" instead of an int.  New-ServiceFabricApplication command will fail with an error : hello override parameter with name 'ServiceEndpoint1' attribute 'Port1' in section 'ResourceOverrides' is invalid.</span></span> <span data-ttu-id="a593e-144">Hello hodnota je "Foo" a vyžaduje je 'int'.</span><span class="sxs-lookup"><span data-stu-id="a593e-144">hello value specified is 'Foo' and required is 'int'.</span></span>
