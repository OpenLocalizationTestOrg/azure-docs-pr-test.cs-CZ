---
title: aaaAzure Service Fabric Docker Compose Preview | Microsoft Docs
description: "Azure Service Fabric přijímá Docker Compose toomake formát je snazší kontejnery exsiting tooorchestrate pomocí Service Fabric. Tato podpora je aktuálně ve verzi preview."
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
ms.openlocfilehash: 824044fd698f0ed94c4212722bc82187905315dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="specifying-volume-plugins-and-logging-drivers-for-your-container"></a>Určení svazku modulů plug-in a ovladače protokolování pro váš kontejner

Service Fabric podporuje určení [modulů plug-in svazku Docker](https://docs.docker.com/engine/extend/plugins_volume/) a [Docker protokolování ovladače](https://docs.docker.com/engine/admin/logging/overview/) pro vaši službu kontejneru. moduly plug-in Hello jsou určené v manifestu aplikace hello, jak je znázorněno v následujícím manifest hello:


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

V předchozím příkladu hello, hello `Source` značky pro hello `Volume` odkazuje toohello zdrojové složky. Zdrojová složka Hello může být složky v hello virtuálního počítače, který je hostitelem hello kontejnery nebo trvalé vzdálené úložiště. Hello `Destination` značka je hello umístění, které hello `Source` běží namapované toowithin hello kontejneru. 

Při zadávání modulu plug-in svazku, Service Fabric automaticky vytvoří hello svazku, pomocí zadaných parametrů hello. Hello `Source` značka je název hello hello svazku a hello `Driver` značky určuje hello svazku ovladač modulu plug-in. Možnosti lze zadat pomocí hello `DriverOption` značky, jak je znázorněno v následujícím fragmentu kódu hello:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azurefile" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```

Pokud je zadaný ovladač Docker protokolu, je nutné toodeploy protokoly hello toohandle agentů (nebo kontejnery) v clusteru hello.  Hello `DriverOption` značky lze použít toospecify protokolu ovladač možnosti také.

Odkažte toohello následující cluster Service Fabric tooa kontejnery toodeploy články:


[Nasazení kontejneru v Service Fabric](service-fabric-deploy-container.md)

