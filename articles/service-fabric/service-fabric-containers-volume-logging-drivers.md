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
# <a name="specifying-volume-plugins-and-logging-drivers-for-your-container"></a><span data-ttu-id="453f2-104">Určení svazku modulů plug-in a ovladače protokolování pro váš kontejner</span><span class="sxs-lookup"><span data-stu-id="453f2-104">Specifying volume plugins and logging drivers for your container</span></span>

<span data-ttu-id="453f2-105">Service Fabric podporuje určení [modulů plug-in svazku Docker](https://docs.docker.com/engine/extend/plugins_volume/) a [Docker protokolování ovladače](https://docs.docker.com/engine/admin/logging/overview/) pro vaši službu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="453f2-105">Service Fabric supports specifying [Docker volume plugins](https://docs.docker.com/engine/extend/plugins_volume/) and [Docker logging drivers](https://docs.docker.com/engine/admin/logging/overview/) for your container service.</span></span> <span data-ttu-id="453f2-106">moduly plug-in Hello jsou určené v manifestu aplikace hello, jak je znázorněno v následujícím manifest hello:</span><span class="sxs-lookup"><span data-stu-id="453f2-106">hello plugins are specified in hello application manifest as shown in hello following manifest:</span></span>


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

<span data-ttu-id="453f2-107">V předchozím příkladu hello, hello `Source` značky pro hello `Volume` odkazuje toohello zdrojové složky.</span><span class="sxs-lookup"><span data-stu-id="453f2-107">In hello preceding example, hello `Source` tag for hello `Volume` refers toohello source folder.</span></span> <span data-ttu-id="453f2-108">Zdrojová složka Hello může být složky v hello virtuálního počítače, který je hostitelem hello kontejnery nebo trvalé vzdálené úložiště.</span><span class="sxs-lookup"><span data-stu-id="453f2-108">hello source folder could be a folder in hello VM that hosts hello containers or a persistent remote store.</span></span> <span data-ttu-id="453f2-109">Hello `Destination` značka je hello umístění, které hello `Source` běží namapované toowithin hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="453f2-109">hello `Destination` tag is hello location that hello `Source` is mapped toowithin hello running container.</span></span> 

<span data-ttu-id="453f2-110">Při zadávání modulu plug-in svazku, Service Fabric automaticky vytvoří hello svazku, pomocí zadaných parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="453f2-110">When specifying a volume plugin, Service Fabric automatically creates hello volume using hello parameters specified.</span></span> <span data-ttu-id="453f2-111">Hello `Source` značka je název hello hello svazku a hello `Driver` značky určuje hello svazku ovladač modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="453f2-111">hello `Source` tag is hello name of hello volume, and hello `Driver` tag specifies hello volume driver plugin.</span></span> <span data-ttu-id="453f2-112">Možnosti lze zadat pomocí hello `DriverOption` značky, jak je znázorněno v následujícím fragmentu kódu hello:</span><span class="sxs-lookup"><span data-stu-id="453f2-112">Options can be specified using hello `DriverOption` tag as shown in hello following snippet:</span></span>

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azurefile" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```

<span data-ttu-id="453f2-113">Pokud je zadaný ovladač Docker protokolu, je nutné toodeploy protokoly hello toohandle agentů (nebo kontejnery) v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="453f2-113">If a Docker log driver is specified, it is necessary toodeploy agents (or containers) toohandle hello logs in hello cluster.</span></span>  <span data-ttu-id="453f2-114">Hello `DriverOption` značky lze použít toospecify protokolu ovladač možnosti také.</span><span class="sxs-lookup"><span data-stu-id="453f2-114">hello `DriverOption` tag can be used toospecify log driver options as well.</span></span>

<span data-ttu-id="453f2-115">Odkažte toohello následující cluster Service Fabric tooa kontejnery toodeploy články:</span><span class="sxs-lookup"><span data-stu-id="453f2-115">Refer toohello following articles toodeploy containers tooa Service Fabric cluster:</span></span>


[<span data-ttu-id="453f2-116">Nasazení kontejneru v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="453f2-116">Deploy a container on Service Fabric</span></span>](service-fabric-deploy-container.md)

