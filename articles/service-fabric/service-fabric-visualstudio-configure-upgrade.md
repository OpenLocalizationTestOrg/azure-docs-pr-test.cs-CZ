---
title: upgrade hello aaaConfigure aplikace Service Fabric | Microsoft Docs
description: "Zjistěte, jak tooconfigure hello nastavení pro upgrade aplikace Service Fabric pomocí sady Microsoft Visual Studio."
services: service-fabric
documentationcenter: na
author: mikkelhegn
manager: mfussell
editor: tglee
ms.assetid: 1757ba85-0b7b-4f16-8a23-2ddaa61c86c6
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/29/2017
ms.author: mikkelhegn
ms.openlocfilehash: 8ca50aa9d911f3c98f017490c8fe29011e8d80cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-upgrade-of-a-service-fabric-application-in-visual-studio"></a><span data-ttu-id="71f94-103">Konfigurovat hello upgrade aplikace Service Fabric v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71f94-103">Configure hello upgrade of a Service Fabric application in Visual Studio</span></span>
<span data-ttu-id="71f94-104">Nástroje sady Visual Studio pro Azure Service Fabric upgradu podporují publikování toolocal nebo vzdálené clustery.</span><span class="sxs-lookup"><span data-stu-id="71f94-104">Visual Studio tools for Azure Service Fabric provide upgrade support for publishing toolocal or remote clusters.</span></span> <span data-ttu-id="71f94-105">Existují tři scénáře, ve kterých má tooupgrade vaší aplikace tooa novější verzi místo nahrazení aplikace hello během testování a ladění:</span><span class="sxs-lookup"><span data-stu-id="71f94-105">There are three scenarios in which you want tooupgrade your application tooa newer version instead of replacing hello application during testing and debugging:</span></span>

* <span data-ttu-id="71f94-106">Během upgradu hello nedojde ke ztrátě dat. aplikace.</span><span class="sxs-lookup"><span data-stu-id="71f94-106">Application data won't be lost during hello upgrade.</span></span>
* <span data-ttu-id="71f94-107">Dostupnosti zůstane vysoké, takže nebudou existovat žádné přerušení služby během upgradu hello případné že dostatek instance služby rozloženy domén upgradu.</span><span class="sxs-lookup"><span data-stu-id="71f94-107">Availability remains high so there won't be any service interruption during hello upgrade, if there are enough service instances spread across upgrade domains.</span></span>
* <span data-ttu-id="71f94-108">Vzhledem aplikaci lze spustit testy, je během upgradu.</span><span class="sxs-lookup"><span data-stu-id="71f94-108">Tests can be run against an application while it's being upgraded.</span></span>

## <a name="parameters-needed-tooupgrade"></a><span data-ttu-id="71f94-109">Parametry potřeby tooupgrade</span><span class="sxs-lookup"><span data-stu-id="71f94-109">Parameters needed tooupgrade</span></span>
<span data-ttu-id="71f94-110">Můžete vybrat z dva typy nasazení: standardním nebo upgradu.</span><span class="sxs-lookup"><span data-stu-id="71f94-110">You can choose from two types of deployment: regular or upgrade.</span></span> <span data-ttu-id="71f94-111">Regulární nasazení vymaže všechny předchozí informace o nasazení a data v clusteru hello při upgradu nasazení se zachová.</span><span class="sxs-lookup"><span data-stu-id="71f94-111">A regular deployment erases any previous deployment information and data on hello cluster, while an upgrade deployment preserves it.</span></span> <span data-ttu-id="71f94-112">Při upgradu aplikace Service Fabric v sadě Visual Studio, musíte parametry upgradu aplikace tooprovide a Kontrola zásad stavu.</span><span class="sxs-lookup"><span data-stu-id="71f94-112">When you upgrade a Service Fabric application in Visual Studio, you need tooprovide application upgrade parameters and health check policies.</span></span> <span data-ttu-id="71f94-113">Aplikace upgradu parametry nápovědy řízení hello upgrade, zatímco zásady kontroly stavu určit, zda text hello upgradu byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="71f94-113">Application upgrade parameters help control hello upgrade, while health check policies determine whether hello upgrade was successful.</span></span> <span data-ttu-id="71f94-114">V tématu [upgradu aplikace Service Fabric: upgrade parametry](service-fabric-application-upgrade-parameters.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="71f94-114">See [Service Fabric application upgrade: upgrade parameters](service-fabric-application-upgrade-parameters.md) for more details.</span></span>

<span data-ttu-id="71f94-115">Existují tři režimy upgradu: *monitorované*, *UnmonitoredAuto*, a *UnmonitoredManual*.</span><span class="sxs-lookup"><span data-stu-id="71f94-115">There are three upgrade modes: *Monitored*, *UnmonitoredAuto*, and *UnmonitoredManual*.</span></span>

* <span data-ttu-id="71f94-116">Upgrade monitorované automatizuje hello upgradu a aplikace kontrolu stavu.</span><span class="sxs-lookup"><span data-stu-id="71f94-116">A Monitored upgrade automates hello upgrade and application health check.</span></span>
* <span data-ttu-id="71f94-117">UnmonitoredAuto upgrade automatizuje hello upgrade, ale přeskočí hello Kontrola stavu aplikace.</span><span class="sxs-lookup"><span data-stu-id="71f94-117">An UnmonitoredAuto upgrade automates hello upgrade, but skips hello application health check.</span></span>
* <span data-ttu-id="71f94-118">Když provedete UnmonitoredManual upgrade, je nutné toomanually upgrade každé upgradované domény.</span><span class="sxs-lookup"><span data-stu-id="71f94-118">When you do an UnmonitoredManual upgrade, you need toomanually upgrade each upgrade domain.</span></span>

<span data-ttu-id="71f94-119">Každý režimu upgradu vyžaduje různé sady parametrů.</span><span class="sxs-lookup"><span data-stu-id="71f94-119">Each upgrade mode requires different sets of parameters.</span></span> <span data-ttu-id="71f94-120">V tématu [parametry upgradu aplikace](service-fabric-application-upgrade-parameters.md) toolearn Další informace o dostupných možnostech upgradu hello.</span><span class="sxs-lookup"><span data-stu-id="71f94-120">See [Application upgrade parameters](service-fabric-application-upgrade-parameters.md) toolearn more about hello available upgrade options.</span></span>

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a><span data-ttu-id="71f94-121">Upgrade aplikace Service Fabric v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71f94-121">Upgrade a Service Fabric application in Visual Studio</span></span>
<span data-ttu-id="71f94-122">Pokud používáte tooupgrade nástroje Visual Studio Service Fabric hello aplikace Service Fabric, můžete zadat, toobe proces publikování upgrade spíše než regulární nasazení kontrolou hello **upgradu aplikace hello** zkontrolujte pole.</span><span class="sxs-lookup"><span data-stu-id="71f94-122">If you’re using hello Visual Studio Service Fabric tools tooupgrade a Service Fabric application, you can specify a publish process toobe an upgrade rather than a regular deployment by checking hello **Upgrade hello application** check box.</span></span>

### <a name="tooconfigure-hello-upgrade-parameters"></a><span data-ttu-id="71f94-123">Parametry upgradu tooconfigure hello</span><span class="sxs-lookup"><span data-stu-id="71f94-123">tooconfigure hello upgrade parameters</span></span>
1. <span data-ttu-id="71f94-124">Klikněte na tlačítko hello **nastavení** tlačítko Další toohello zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="71f94-124">Click hello **Settings** button next toohello check box.</span></span> <span data-ttu-id="71f94-125">Hello **upravit parametry upgradu** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="71f94-125">hello **Edit Upgrade Parameters** dialog box appears.</span></span> <span data-ttu-id="71f94-126">Hello **upravit parametry upgradu** dialogové okno podporuje hello monitorované UnmonitoredAuto a UnmonitoredManual upgradu režimy.</span><span class="sxs-lookup"><span data-stu-id="71f94-126">hello **Edit Upgrade Parameters** dialog box supports hello Monitored, UnmonitoredAuto, and UnmonitoredManual upgrade modes.</span></span>
2. <span data-ttu-id="71f94-127">Vyberte režim upgradu hello má toouse a potom vyplňte hello parametr mřížky.</span><span class="sxs-lookup"><span data-stu-id="71f94-127">Select hello upgrade mode that you want toouse and then fill out hello parameter grid.</span></span>

    <span data-ttu-id="71f94-128">Každý parametr má výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="71f94-128">Each parameter has default values.</span></span> <span data-ttu-id="71f94-129">Hello volitelný parametr *DefaultServiceTypeHealthPolicy* trvá vstupní tabulky hodnota hash.</span><span class="sxs-lookup"><span data-stu-id="71f94-129">hello optional parameter *DefaultServiceTypeHealthPolicy* takes a hash table input.</span></span> <span data-ttu-id="71f94-130">Tady je příklad hello hash tabulku formát vstupu pro *DefaultServiceTypeHealthPolicy*:</span><span class="sxs-lookup"><span data-stu-id="71f94-130">Here’s an example of hello hash table input format for *DefaultServiceTypeHealthPolicy*:</span></span>

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    <span data-ttu-id="71f94-131">*ServiceTypeHealthPolicyMap* je jiný volitelný parametr, který přebírá vstupní tabulky hash hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="71f94-131">*ServiceTypeHealthPolicyMap* is another optional parameter that takes a hash table input in hello following format:</span></span>

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    <span data-ttu-id="71f94-132">Tady je příklad reálnými:</span><span class="sxs-lookup"><span data-stu-id="71f94-132">Here's a real-life example:</span></span>

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```
3. <span data-ttu-id="71f94-133">Pokud vyberete UnmonitoredManual režimu upgradu, musíte ručně spustit toocontinue konzoly prostředí PowerShell a dokončení procesu upgradu hello.</span><span class="sxs-lookup"><span data-stu-id="71f94-133">If you select UnmonitoredManual upgrade mode, you must manually start a PowerShell console toocontinue and finish hello upgrade process.</span></span> <span data-ttu-id="71f94-134">Odkazovat příliš[upgradu aplikace Service Fabric: advanced témata](service-fabric-application-upgrade-advanced.md) toolearn jak ruční upgrade funguje.</span><span class="sxs-lookup"><span data-stu-id="71f94-134">Refer too[Service Fabric application upgrade: advanced topics](service-fabric-application-upgrade-advanced.md) toolearn how manual upgrade works.</span></span>

## <a name="upgrade-an-application-by-using-powershell"></a><span data-ttu-id="71f94-135">Upgrade aplikace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="71f94-135">Upgrade an application by using PowerShell</span></span>
<span data-ttu-id="71f94-136">Můžete použít tooupgrade rutiny prostředí PowerShell aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="71f94-136">You can use PowerShell cmdlets tooupgrade a Service Fabric application.</span></span> <span data-ttu-id="71f94-137">V tématu [kurz upgradu aplikace Service Fabric](service-fabric-application-upgrade-tutorial.md) a [Start-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) podrobné informace.</span><span class="sxs-lookup"><span data-stu-id="71f94-137">See [Service Fabric application upgrade tutorial](service-fabric-application-upgrade-tutorial.md) and [Start-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) for detailed information.</span></span>

## <a name="specify-a-health-check-policy-in-hello-application-manifest-file"></a><span data-ttu-id="71f94-138">Zadejte zásady stavu zkontrolujte v souboru manifestu aplikace hello</span><span class="sxs-lookup"><span data-stu-id="71f94-138">Specify a health check policy in hello application manifest file</span></span>
<span data-ttu-id="71f94-139">Všechny služby v Service Fabric aplikace může mít svůj vlastní parametry zásad stavu, které hello výchozí hodnotu přepsat.</span><span class="sxs-lookup"><span data-stu-id="71f94-139">Every service in a Service Fabric application can have its own health policy parameters that override hello default values.</span></span> <span data-ttu-id="71f94-140">Můžete zadat tyto hodnoty parametrů v souboru manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="71f94-140">You can provide these parameter values in hello application manifest file.</span></span>

<span data-ttu-id="71f94-141">Hello následující příklad ukazuje, jak zkontrolovat tooapply jedinečný stav zásad u každé služby v manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="71f94-141">hello following example shows how tooapply a unique health check policy for each service in hello application manifest.</span></span>

```xml
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a><span data-ttu-id="71f94-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="71f94-142">Next steps</span></span>
<span data-ttu-id="71f94-143">Další informace o nasazení aplikace naleznete v tématu [nasadit existující aplikaci v Azure Service Fabric](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="71f94-143">For more information about deploying an application, see [Deploy an existing application in Azure Service Fabric](service-fabric-deploy-existing-app.md).</span></span>