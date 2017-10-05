---
title: Azure Service Fabric Docker Compose Preview | Microsoft Docs
description: "Azure Service Fabric přijme Docker Compose formátu, aby bylo snazší orchestraci kontejnerů exsiting pomocí Service Fabric. Tato podpora je aktuálně ve verzi preview."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: b12ef95add6347621f7d4865fac46568f91a1e12
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="specifying-volume-plugins-and-logging-drivers-for-your-container"></a>Určení svazku modulů plug-in a ovladače protokolování pro váš kontejner

Service Fabric podporuje určení [modulů plug-in svazku Docker](https://docs.docker.com/engine/extend/plugins_volume/) a [Docker protokolování ovladače](https://docs.docker.com/engine/admin/logging/overview/) pro vaši službu kontejneru. Moduly plug-in jsou určené v manifestu aplikace, jak je znázorněno v následujícím manifestu:


```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
    </Parameters>
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
     <Policies>
       <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv"> 
        <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
        <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
        <LogConfig Driver="etwlogs" >
          <DriverOption Name="test" Value="vale"/>
        </LogConfig>
        <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
        <Volume Source="d:\myfolder" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
        <Volume Source="myexternalvolume" Destination="c:\testmountlocation3" Driver="sf" IsReadOnly="true"></Volume>
       </ContainerHostPolicies>
   </Policies>
    </ServiceManifestImport>
    <ServiceTemplates>
        <StatelessService ServiceTypeName="StatelessNodeService" InstanceCount="5">
            <SingletonPartition></SingletonPartition>
        </StatelessService>
    </ServiceTemplates>
</ApplicationManifest>
```

V předchozím příkladu `Source` označení pro `Volume` odkazuje do zdrojové složky. Zdrojová složka může být do složky ve virtuálním počítači, který je hostitelem kontejnery nebo trvalé vzdálené úložiště. `Destination` Značka je umístění, `Source` je namapována na spuštěné kontejneru. 

Při zadávání modulu plug-in svazku, Service Fabric automaticky vytvoří svazku, pomocí zadaných parametrů. `Source` Značka je název svazku a `Driver` značky Určuje modul plug-in ovladač svazku. Možnosti lze zadat pomocí `DriverOption` značky, jak je znázorněno v následujícím fragmentu kódu:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azurefile" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```

Pokud je zadaný ovladač protokolu Docker, je nezbytné pro nasazení agentů (nebo kontejnery) pro zpracování protokoly v clusteru.  `DriverOption` Značky lze zadat také možnosti protokolu ovladačů.

Najdete v těchto článcích nasazení kontejnerů do clusteru Service Fabric:


[Nasazení kontejneru v Service Fabric](service-fabric-deploy-container.md)

