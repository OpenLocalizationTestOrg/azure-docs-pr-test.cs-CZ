---
title: Konfigurace upgradu aplikace Service Fabric | Microsoft Docs
description: "Zjistěte, jak nakonfigurovat nastavení pro upgrade aplikace Service Fabric pomocí sady Microsoft Visual Studio."
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
ms.openlocfilehash: 314b29a56e4651222822f40a116af97a7372ff2c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a><span data-ttu-id="0282e-103">Konfigurace upgradu aplikace Service Fabric v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0282e-103">Configure the upgrade of a Service Fabric application in Visual Studio</span></span>
<span data-ttu-id="0282e-104">Nástroje sady Visual Studio pro Azure Service Fabric zajištění upgradu podpory pro publikování do místního nebo vzdáleného clusterů.</span><span class="sxs-lookup"><span data-stu-id="0282e-104">Visual Studio tools for Azure Service Fabric provide upgrade support for publishing to local or remote clusters.</span></span> <span data-ttu-id="0282e-105">Existují tři scénáře, ve kterých chcete upgradovat aplikaci na novější verzi místo nahrazení aplikace v průběhu testování a ladění:</span><span class="sxs-lookup"><span data-stu-id="0282e-105">There are three scenarios in which you want to upgrade your application to a newer version instead of replacing the application during testing and debugging:</span></span>

* <span data-ttu-id="0282e-106">Během upgradu nedojde ke ztrátě dat. aplikace.</span><span class="sxs-lookup"><span data-stu-id="0282e-106">Application data won't be lost during the upgrade.</span></span>
* <span data-ttu-id="0282e-107">Dostupnost zůstane vysoké, takže nebudou existovat žádné přerušení služby během upgradu případné že dostatek instance služby rozloženy domén upgradu.</span><span class="sxs-lookup"><span data-stu-id="0282e-107">Availability remains high so there won't be any service interruption during the upgrade, if there are enough service instances spread across upgrade domains.</span></span>
* <span data-ttu-id="0282e-108">Vzhledem aplikaci lze spustit testy, je během upgradu.</span><span class="sxs-lookup"><span data-stu-id="0282e-108">Tests can be run against an application while it's being upgraded.</span></span>

## <a name="parameters-needed-to-upgrade"></a><span data-ttu-id="0282e-109">Parametry, které jsou potřebné pro upgrade</span><span class="sxs-lookup"><span data-stu-id="0282e-109">Parameters needed to upgrade</span></span>
<span data-ttu-id="0282e-110">Můžete vybrat z dva typy nasazení: standardním nebo upgradu.</span><span class="sxs-lookup"><span data-stu-id="0282e-110">You can choose from two types of deployment: regular or upgrade.</span></span> <span data-ttu-id="0282e-111">Regulární nasazení vymaže všechny předchozí informace o nasazení a data v clusteru, při upgradu nasazení se zachová.</span><span class="sxs-lookup"><span data-stu-id="0282e-111">A regular deployment erases any previous deployment information and data on the cluster, while an upgrade deployment preserves it.</span></span> <span data-ttu-id="0282e-112">Při upgradu aplikace Service Fabric v sadě Visual Studio, je třeba zadat, že parametry upgradu aplikace a stavu zkontrolujte zásady.</span><span class="sxs-lookup"><span data-stu-id="0282e-112">When you upgrade a Service Fabric application in Visual Studio, you need to provide application upgrade parameters and health check policies.</span></span> <span data-ttu-id="0282e-113">Parametry upgradu aplikace pomoc při ovládání upgradu, zatímco zásady kontroly stavu určit, zda upgradu byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="0282e-113">Application upgrade parameters help control the upgrade, while health check policies determine whether the upgrade was successful.</span></span> <span data-ttu-id="0282e-114">V tématu [upgradu aplikace Service Fabric: upgrade parametry](service-fabric-application-upgrade-parameters.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="0282e-114">See [Service Fabric application upgrade: upgrade parameters](service-fabric-application-upgrade-parameters.md) for more details.</span></span>

<span data-ttu-id="0282e-115">Existují tři režimy upgradu: *monitorované*, *UnmonitoredAuto*, a *UnmonitoredManual*.</span><span class="sxs-lookup"><span data-stu-id="0282e-115">There are three upgrade modes: *Monitored*, *UnmonitoredAuto*, and *UnmonitoredManual*.</span></span>

* <span data-ttu-id="0282e-116">Upgrade monitorované automatizuje upgradu a kontrola stavu aplikace.</span><span class="sxs-lookup"><span data-stu-id="0282e-116">A Monitored upgrade automates the upgrade and application health check.</span></span>
* <span data-ttu-id="0282e-117">UnmonitoredAuto upgrade automatizuje upgrade, ale přeskočí kontrolu stavu aplikace.</span><span class="sxs-lookup"><span data-stu-id="0282e-117">An UnmonitoredAuto upgrade automates the upgrade, but skips the application health check.</span></span>
* <span data-ttu-id="0282e-118">Když provedete UnmonitoredManual upgrade, je třeba ručně upgradovat každé upgradované domény.</span><span class="sxs-lookup"><span data-stu-id="0282e-118">When you do an UnmonitoredManual upgrade, you need to manually upgrade each upgrade domain.</span></span>

<span data-ttu-id="0282e-119">Každý režimu upgradu vyžaduje různé sady parametrů.</span><span class="sxs-lookup"><span data-stu-id="0282e-119">Each upgrade mode requires different sets of parameters.</span></span> <span data-ttu-id="0282e-120">V tématu [parametry upgradu aplikace](service-fabric-application-upgrade-parameters.md) Další informace o dostupných možnostech upgradu.</span><span class="sxs-lookup"><span data-stu-id="0282e-120">See [Application upgrade parameters](service-fabric-application-upgrade-parameters.md) to learn more about the available upgrade options.</span></span>

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a><span data-ttu-id="0282e-121">Upgrade aplikace Service Fabric v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0282e-121">Upgrade a Service Fabric application in Visual Studio</span></span>
<span data-ttu-id="0282e-122">Pokud používáte nástroje Visual Studio Service Fabric k upgradu aplikace Service Fabric, můžete zadat proces publikování jako upgrade, ne regulární nasazení kontrolou **upgradu aplikace** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="0282e-122">If you’re using the Visual Studio Service Fabric tools to upgrade a Service Fabric application, you can specify a publish process to be an upgrade rather than a regular deployment by checking the **Upgrade the application** check box.</span></span>

### <a name="to-configure-the-upgrade-parameters"></a><span data-ttu-id="0282e-123">Konfigurace upgradu parametrů</span><span class="sxs-lookup"><span data-stu-id="0282e-123">To configure the upgrade parameters</span></span>
1. <span data-ttu-id="0282e-124">Klikněte **nastavení** tlačítko vedle pole.</span><span class="sxs-lookup"><span data-stu-id="0282e-124">Click the **Settings** button next to the check box.</span></span> <span data-ttu-id="0282e-125">**Upravit parametry upgradu** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0282e-125">The **Edit Upgrade Parameters** dialog box appears.</span></span> <span data-ttu-id="0282e-126">**Upravit parametry upgradu** dialogové okno podporuje monitorované UnmonitoredAuto a UnmonitoredManual upgradu režimy.</span><span class="sxs-lookup"><span data-stu-id="0282e-126">The **Edit Upgrade Parameters** dialog box supports the Monitored, UnmonitoredAuto, and UnmonitoredManual upgrade modes.</span></span>
2. <span data-ttu-id="0282e-127">Vyberte režim upgradu, který chcete použít a potom vyplňte parametr mřížky.</span><span class="sxs-lookup"><span data-stu-id="0282e-127">Select the upgrade mode that you want to use and then fill out the parameter grid.</span></span>

    <span data-ttu-id="0282e-128">Každý parametr má výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0282e-128">Each parameter has default values.</span></span> <span data-ttu-id="0282e-129">Volitelný parametr *DefaultServiceTypeHealthPolicy* trvá vstupní tabulky hodnota hash.</span><span class="sxs-lookup"><span data-stu-id="0282e-129">The optional parameter *DefaultServiceTypeHealthPolicy* takes a hash table input.</span></span> <span data-ttu-id="0282e-130">Tady je příklad formát vstupu tabulku hash pro *DefaultServiceTypeHealthPolicy*:</span><span class="sxs-lookup"><span data-stu-id="0282e-130">Here’s an example of the hash table input format for *DefaultServiceTypeHealthPolicy*:</span></span>

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    <span data-ttu-id="0282e-131">*ServiceTypeHealthPolicyMap* je jiný volitelný parametr, který přijímá vstup hash tabulku v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="0282e-131">*ServiceTypeHealthPolicyMap* is another optional parameter that takes a hash table input in the following format:</span></span>

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    <span data-ttu-id="0282e-132">Tady je příklad reálnými:</span><span class="sxs-lookup"><span data-stu-id="0282e-132">Here's a real-life example:</span></span>

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```
3. <span data-ttu-id="0282e-133">Pokud vyberete UnmonitoredManual režimu upgradu, musíte spustit ručně konzole PowerShell chcete pokračovat a dokončit proces upgradu.</span><span class="sxs-lookup"><span data-stu-id="0282e-133">If you select UnmonitoredManual upgrade mode, you must manually start a PowerShell console to continue and finish the upgrade process.</span></span> <span data-ttu-id="0282e-134">Odkazovat na [upgradu aplikace Service Fabric: advanced témata](service-fabric-application-upgrade-advanced.md) další jak ruční upgrade funguje.</span><span class="sxs-lookup"><span data-stu-id="0282e-134">Refer to [Service Fabric application upgrade: advanced topics](service-fabric-application-upgrade-advanced.md) to learn how manual upgrade works.</span></span>

## <a name="upgrade-an-application-by-using-powershell"></a><span data-ttu-id="0282e-135">Upgrade aplikace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="0282e-135">Upgrade an application by using PowerShell</span></span>
<span data-ttu-id="0282e-136">Rutiny prostředí PowerShell můžete použít k upgradu aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="0282e-136">You can use PowerShell cmdlets to upgrade a Service Fabric application.</span></span> <span data-ttu-id="0282e-137">V tématu [kurz upgradu aplikace Service Fabric](service-fabric-application-upgrade-tutorial.md) a [Start-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) podrobné informace.</span><span class="sxs-lookup"><span data-stu-id="0282e-137">See [Service Fabric application upgrade tutorial](service-fabric-application-upgrade-tutorial.md) and [Start-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) for detailed information.</span></span>

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a><span data-ttu-id="0282e-138">Zadejte zásady stavu zkontrolujte v souboru manifestu aplikace</span><span class="sxs-lookup"><span data-stu-id="0282e-138">Specify a health check policy in the application manifest file</span></span>
<span data-ttu-id="0282e-139">Všechny služby v Service Fabric aplikace může mít svůj vlastní parametry zásad stavu, které přepsat výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0282e-139">Every service in a Service Fabric application can have its own health policy parameters that override the default values.</span></span> <span data-ttu-id="0282e-140">Můžete zadat tyto hodnoty parametrů v souboru manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="0282e-140">You can provide these parameter values in the application manifest file.</span></span>

<span data-ttu-id="0282e-141">Následující příklad ukazuje, jak použít zásady kontrola jedinečný stav pro každou službu v manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="0282e-141">The following example shows how to apply a unique health check policy for each service in the application manifest.</span></span>

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
## <a name="next-steps"></a><span data-ttu-id="0282e-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0282e-142">Next steps</span></span>
<span data-ttu-id="0282e-143">Další informace o nasazení aplikace naleznete v tématu [nasadit existující aplikaci v Azure Service Fabric](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="0282e-143">For more information about deploying an application, see [Deploy an existing application in Azure Service Fabric](service-fabric-deploy-existing-app.md).</span></span>