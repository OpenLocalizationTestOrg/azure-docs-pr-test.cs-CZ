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
# <a name="specifying-volume-plugins-and-logging-drivers-for-your-container"></a><span data-ttu-id="613a1-104">Určení svazku modulů plug-in a ovladače protokolování pro váš kontejner</span><span class="sxs-lookup"><span data-stu-id="613a1-104">Specifying volume plugins and logging drivers for your container</span></span>

<span data-ttu-id="613a1-105">Service Fabric podporuje určení [modulů plug-in svazku Docker](https://docs.docker.com/engine/extend/plugins_volume/) a [Docker protokolování ovladače](https://docs.docker.com/engine/admin/logging/overview/) pro vaši službu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="613a1-105">Service Fabric supports specifying [Docker volume plugins](https://docs.docker.com/engine/extend/plugins_volume/) and [Docker logging drivers](https://docs.docker.com/engine/admin/logging/overview/) for your container service.</span></span> <span data-ttu-id="613a1-106">Moduly plug-in jsou určené v manifestu aplikace, jak je znázorněno v následujícím manifestu:</span><span class="sxs-lookup"><span data-stu-id="613a1-106">The plugins are specified in the application manifest as shown in the following manifest:</span></span>


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

<span data-ttu-id="613a1-107">V předchozím příkladu `Source` označení pro `Volume` odkazuje do zdrojové složky.</span><span class="sxs-lookup"><span data-stu-id="613a1-107">In the preceding example, the `Source` tag for the `Volume` refers to the source folder.</span></span> <span data-ttu-id="613a1-108">Zdrojová složka může být do složky ve virtuálním počítači, který je hostitelem kontejnery nebo trvalé vzdálené úložiště.</span><span class="sxs-lookup"><span data-stu-id="613a1-108">The source folder could be a folder in the VM that hosts the containers or a persistent remote store.</span></span> <span data-ttu-id="613a1-109">`Destination` Značka je umístění, `Source` je namapována na spuštěné kontejneru.</span><span class="sxs-lookup"><span data-stu-id="613a1-109">The `Destination` tag is the location that the `Source` is mapped to within the running container.</span></span> 

<span data-ttu-id="613a1-110">Při zadávání modulu plug-in svazku, Service Fabric automaticky vytvoří svazku, pomocí zadaných parametrů.</span><span class="sxs-lookup"><span data-stu-id="613a1-110">When specifying a volume plugin, Service Fabric automatically creates the volume using the parameters specified.</span></span> <span data-ttu-id="613a1-111">`Source` Značka je název svazku a `Driver` značky Určuje modul plug-in ovladač svazku.</span><span class="sxs-lookup"><span data-stu-id="613a1-111">The `Source` tag is the name of the volume, and the `Driver` tag specifies the volume driver plugin.</span></span> <span data-ttu-id="613a1-112">Možnosti lze zadat pomocí `DriverOption` značky, jak je znázorněno v následujícím fragmentu kódu:</span><span class="sxs-lookup"><span data-stu-id="613a1-112">Options can be specified using the `DriverOption` tag as shown in the following snippet:</span></span>

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azurefile" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```

<span data-ttu-id="613a1-113">Pokud je zadaný ovladač protokolu Docker, je nezbytné pro nasazení agentů (nebo kontejnery) pro zpracování protokoly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="613a1-113">If a Docker log driver is specified, it is necessary to deploy agents (or containers) to handle the logs in the cluster.</span></span>  <span data-ttu-id="613a1-114">`DriverOption` Značky lze zadat také možnosti protokolu ovladačů.</span><span class="sxs-lookup"><span data-stu-id="613a1-114">The `DriverOption` tag can be used to specify log driver options as well.</span></span>

<span data-ttu-id="613a1-115">Najdete v těchto článcích nasazení kontejnerů do clusteru Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="613a1-115">Refer to the following articles to deploy containers to a Service Fabric cluster:</span></span>


[<span data-ttu-id="613a1-116">Nasazení kontejneru v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="613a1-116">Deploy a container on Service Fabric</span></span>](service-fabric-deploy-container.md)

