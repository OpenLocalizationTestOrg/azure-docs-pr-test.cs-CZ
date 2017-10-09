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
# <a name="specify-resources-in-a-service-manifest"></a>Zadání prostředků v manifestu služby
## <a name="overview"></a>Přehled
manifest služby Hello umožňuje prostředky, které jsou používány toobe služby hello deklarovaný či změnit beze změny kódu hello zkompilovat. Azure Service Fabric podporuje konfiguraci prostředků koncový bod služby hello. Hello přístup toohello prostředky, které jsou určené v hello service manifest lze řídit prostřednictvím hello objektu SecurityGroup v manifestu aplikace hello. deklarace Hello prostředků umožňuje tyto prostředky toobe změnit v době nasazení, což znamená, že služba hello nepotřebuje toointroduce mechanismus nové konfigurace. Hello definice schématu pro soubor ServiceManifest.xml hello se instaluje s hello Service Fabric SDK a nástrojů pro příliš*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

## <a name="endpoints"></a>Koncové body
Při prostředek koncového bodu je definována v hello service manifest, Service Fabric přiřazuje porty z rozsahu portů aplikace hello vyhrazené, pokud není port určen explicitně. Například, podívejte se na koncový bod hello *ServiceEndpoint1* zadané v manifestu fragment hello zadaný po tomto odstavci. Služby mohou také požadovat specifického portu v prostředku. Repliky služby spuštěné v různých uzly lze přiřadit různá čísla portů, zatímco hello repliky služby spuštěné stejný port hello sdílenou složku v uzlu. repliky služby Hello pak můžete použít tyto porty podle potřeby pro replikaci a naslouchá pro požadavky klientů.

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

Odkazovat příliš[konfigurace stavová spolehlivé služby](service-fabric-reliable-services-configuration.md) tooread více informací o ze souboru nastavení konfigurace balíčku hello (souborech settings.xml) odkazující na koncové body.

## <a name="example-specifying-an-http-endpoint-for-your-service"></a>Příklad: určení HTTP koncový bod služby
Hello následující service manifest definuje jeden prostředek koncový bod TCP a dva prostředky koncový bod HTTP v hello &lt;prostředky&gt; elementu.

Koncové body protokolu HTTP jsou automaticky že seznamu ACL by pomocí Service Fabric.

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

## <a name="example-specifying-an-https-endpoint-for-your-service"></a>Příklad: určení koncový bod HTTPS pro služby
Hello protokol HTTPS zajišťuje ověřování na serveru a slouží pro komunikaci klienta se serverem šifrování. tooenable HTTPS na službě Service Fabric, zadat protokol hello v hello *prostředků -> Koncové body -> koncový bod* části hello service manifest, jak je uvedeno výše pro koncový bod hello *ServiceEndpoint3* .

> [!NOTE]
> Protokol služby nelze změnit během upgradu aplikace. Pokud se změní při upgradu, je narušující změně.
> 
> 

Tady je příklad ApplicationManifest potřebujete tooset pro protokol HTTPS. je třeba zadat Hello kryptografický otisk certifikátu. Hello EndpointRef je odkaz tooEndpointResource v ServiceManifest, u kterého jste nastavili hello protokol HTTPS. Můžete přidat více než jeden EndpointCertificate.  

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

## <a name="overriding-endpoints-in-servicemanifestxml"></a>Přepsání koncových bodů v ServiceManifest.xml

Hello ApplicationManifest přidejte ResourceOverrides oddíl, který bude tooConfigOverrides oddíl na stejné úrovni. V této části můžete zadat hello přepsání pro oddíl hello koncové body v části prostředky hello zadaný v hello Service manifest.

V pořadí toooverride koncového bodu v ServiceManifest pomocí ApplicationParameters změnu hello ApplicationManifest jako následující:

V části ServiceManifestImport hello přidejte novou část "ResourceOverrides"

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

V hello parametry přidat níže:

```xml
  <Parameters>
    <Parameter Name="Port" DefaultValue="" />
    <Parameter Name="Protocol" DefaultValue="" />
    <Parameter Name="Type" DefaultValue="" />
    <Parameter Name="Port1" DefaultValue="" />
    <Parameter Name="Protocol1" DefaultValue="" />
  </Parameters>
```

Při nasazení aplikace hello teď můžete předat tyto hodnoty jako ApplicationParameters například:

```powershell
PS C:\> New-ServiceFabricApplication -ApplicationName fabric:/myapp -ApplicationTypeName "AppType" -ApplicationTypeVersion "1.0.0" -ApplicationParameter @{Port='1001'; Protocol='https'; Type='Input'; Port1='2001'; Protocol='http'}
```

Poznámka: Pokud hello hodnoty poskytují pro prázdný hello ApplicationParameters jsme přejděte zpátky toohello výchozí hodnotu podle hello ServiceManifest pro odpovídající EndPointName hello.

Například:

Pokud se v hello ServiceManifest jste zadali

```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint1" Protocol="tcp"/>
    </Endpoints>
  </Resources>
```

A hello Port1 a Protocol1 hodnotu pro parametry aplikace je null nebo prázdná. Hello port je stále rozhodnuto pomocí ServiceFabric. A hello protokol tcp.

Předpokládejme, že zadáte chybnou hodnotu. Například pro Port jste zadali hodnotu řetězce "Foo" namísto typ int.  Nové ServiceFabricApplication příkaz se nezdaří s chybou: Parametr hello přepisu s názvem 'ServiceEndpoint1' atributu "Port1" v části 'ResourceOverrides' je neplatný. Hello hodnota je "Foo" a vyžaduje je 'int'.
