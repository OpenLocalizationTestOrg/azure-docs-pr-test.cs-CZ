---
title: "aaaTroubleshoot vaše místní nastavení clusteru Service Fabric | Microsoft Docs"
description: "Tento článek popisuje sadu doporučení pro řešení potíží s místní vývojový cluster"
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 97f4feaa-bba0-47af-8fdd-07f811fe2202
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: mikkelhegn
ms.openlocfilehash: ce36f62a4bc69d2cd5b6c3df4abda6ca88fa84f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-your-local-development-cluster-setup"></a><span data-ttu-id="5c815-103">Řešení potíží s instalací vašeho místního vývojového clusteru</span><span class="sxs-lookup"><span data-stu-id="5c815-103">Troubleshoot your local development cluster setup</span></span>
<span data-ttu-id="5c815-104">Pokud narazíte na problém při interakci s místním clusteru vývoj Azure Service Fabric, přečtěte si následující návrhy možná řešení hello.</span><span class="sxs-lookup"><span data-stu-id="5c815-104">If you run into an issue while interacting with your local Azure Service Fabric development cluster, review hello following suggestions for potential solutions.</span></span>

## <a name="cluster-setup-failures"></a><span data-ttu-id="5c815-105">Selhání instalace clusteru</span><span class="sxs-lookup"><span data-stu-id="5c815-105">Cluster setup failures</span></span>
### <a name="cannot-clean-up-service-fabric-logs"></a><span data-ttu-id="5c815-106">Nelze vyčistit protokoly Service Fabric</span><span class="sxs-lookup"><span data-stu-id="5c815-106">Cannot clean up Service Fabric logs</span></span>
#### <a name="problem"></a><span data-ttu-id="5c815-107">Problém</span><span class="sxs-lookup"><span data-stu-id="5c815-107">Problem</span></span>
<span data-ttu-id="5c815-108">Při spouštění skriptu DevClusterSetup hello, zobrazí chybu takto:</span><span class="sxs-lookup"><span data-stu-id="5c815-108">While running hello DevClusterSetup script, you see an error like this:</span></span>

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held tooitems in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a><span data-ttu-id="5c815-109">Řešení</span><span class="sxs-lookup"><span data-stu-id="5c815-109">Solution</span></span>
<span data-ttu-id="5c815-110">Zavřete aktuální okno PowerShell text hello a otevřete nové okno prostředí PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="5c815-110">Close hello current PowerShell window and open a new PowerShell window as an administrator.</span></span> <span data-ttu-id="5c815-111">Nyní byste měli mít toosuccessfully spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="5c815-111">You should now be able toosuccessfully run hello script.</span></span>

## <a name="cluster-connection-failures"></a><span data-ttu-id="5c815-112">Selhání připojení ke clusteru</span><span class="sxs-lookup"><span data-stu-id="5c815-112">Cluster connection failures</span></span>
### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a><span data-ttu-id="5c815-113">Rutiny prostředí Service Fabric PowerShell nejsou rozpoznány v prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="5c815-113">Service Fabric PowerShell cmdlets are not recognized in Azure PowerShell</span></span>
#### <a name="problem"></a><span data-ttu-id="5c815-114">Problém</span><span class="sxs-lookup"><span data-stu-id="5c815-114">Problem</span></span>
<span data-ttu-id="5c815-115">Pokud můžete zkusit toorun kterýkoli z hello rutiny prostředí Service Fabric PowerShell, jako například `Connect-ServiceFabricCluster` v okně prostředí Azure PowerShell, se nezdaří, oznamující tuto rutinu hello nebyl rozpoznán.</span><span class="sxs-lookup"><span data-stu-id="5c815-115">If you try toorun any of hello Service Fabric PowerShell cmdlets, such as `Connect-ServiceFabricCluster` in an Azure PowerShell window, it fails, saying that hello cmdlet is not recognized.</span></span> <span data-ttu-id="5c815-116">Hello důvodem je, že prostředí Azure PowerShell používá hello 32bitová verze prostředí Windows PowerShell (i na 64bitové verze operačního systému), zatímco hello Service Fabric rutiny fungovat pouze v prostředí 64-bit.</span><span class="sxs-lookup"><span data-stu-id="5c815-116">hello reason for this is that Azure PowerShell uses hello 32-bit version of Windows PowerShell (even on 64-bit OS versions), whereas hello Service Fabric cmdlets only work in 64-bit environments.</span></span>

#### <a name="solution"></a><span data-ttu-id="5c815-117">Řešení</span><span class="sxs-lookup"><span data-stu-id="5c815-117">Solution</span></span>
<span data-ttu-id="5c815-118">Vždy spustit rutiny Service Fabric přímo z prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5c815-118">Always run Service Fabric cmdlets directly from Windows PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="5c815-119">Hello nejnovější verzi prostředí Azure PowerShell nevytvoří speciální zástupce, takže to už byste měli udělat.</span><span class="sxs-lookup"><span data-stu-id="5c815-119">hello latest version of Azure PowerShell does not create a special shortcut, so this should no longer occur.</span></span>
> 
> 

### <a name="type-initialization-exception"></a><span data-ttu-id="5c815-120">Typ výjimky inicializace</span><span class="sxs-lookup"><span data-stu-id="5c815-120">Type Initialization exception</span></span>
#### <a name="problem"></a><span data-ttu-id="5c815-121">Problém</span><span class="sxs-lookup"><span data-stu-id="5c815-121">Problem</span></span>
<span data-ttu-id="5c815-122">Pokud se připojujete toohello clusteru v prostředí PowerShell, zobrazí chyba hello typeinitializationexception – pro System.Fabric.Common.AppTrace.</span><span class="sxs-lookup"><span data-stu-id="5c815-122">When you are connecting toohello cluster in PowerShell, you see hello error TypeInitializationException for System.Fabric.Common.AppTrace.</span></span>

#### <a name="solution"></a><span data-ttu-id="5c815-123">Řešení</span><span class="sxs-lookup"><span data-stu-id="5c815-123">Solution</span></span>
<span data-ttu-id="5c815-124">Během instalace nebyl správně nastaven vaše proměnná path.</span><span class="sxs-lookup"><span data-stu-id="5c815-124">Your path variable was not correctly set during installation.</span></span> <span data-ttu-id="5c815-125">Odhlásit se ze systému Windows a přihlaste se zpět.</span><span class="sxs-lookup"><span data-stu-id="5c815-125">Sign out of Windows and sign back in.</span></span> <span data-ttu-id="5c815-126">To obnoví cestu.</span><span class="sxs-lookup"><span data-stu-id="5c815-126">This refreshes your path.</span></span>

### <a name="cluster-connection-fails-with-object-is-closed"></a><span data-ttu-id="5c815-127">Připojení clusteru selže s "Objekt je zavřený."</span><span class="sxs-lookup"><span data-stu-id="5c815-127">Cluster connection fails with "Object is closed"</span></span>
#### <a name="problem"></a><span data-ttu-id="5c815-128">Problém</span><span class="sxs-lookup"><span data-stu-id="5c815-128">Problem</span></span>
<span data-ttu-id="5c815-129">Volání tooConnect-ServiceFabricCluster selže s chybou takto:</span><span class="sxs-lookup"><span data-stu-id="5c815-129">A call tooConnect-ServiceFabricCluster fails with an error like this:</span></span>

    Connect-ServiceFabricCluster : hello object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a><span data-ttu-id="5c815-130">Řešení</span><span class="sxs-lookup"><span data-stu-id="5c815-130">Solution</span></span>
<span data-ttu-id="5c815-131">Zavřete aktuální okno PowerShell text hello a otevřete nové okno prostředí PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="5c815-131">Close hello current PowerShell window and open a new PowerShell window as an administrator.</span></span> <span data-ttu-id="5c815-132">Teď mělo být možné toosuccessfully připojení.</span><span class="sxs-lookup"><span data-stu-id="5c815-132">You should now be able toosuccessfully connect.</span></span>

### <a name="fabric-connection-denied-exception"></a><span data-ttu-id="5c815-133">Výjimka připojení odepřen prostředků infrastruktury</span><span class="sxs-lookup"><span data-stu-id="5c815-133">Fabric Connection Denied exception</span></span>
#### <a name="problem"></a><span data-ttu-id="5c815-134">Problém</span><span class="sxs-lookup"><span data-stu-id="5c815-134">Problem</span></span>
<span data-ttu-id="5c815-135">Při ladění ze sady Visual Studio, dojde k chybě FabricConnectionDeniedException.</span><span class="sxs-lookup"><span data-stu-id="5c815-135">When debugging from Visual Studio, you get a FabricConnectionDeniedException error.</span></span>

#### <a name="solution"></a><span data-ttu-id="5c815-136">Řešení</span><span class="sxs-lookup"><span data-stu-id="5c815-136">Solution</span></span>
<span data-ttu-id="5c815-137">Této chybě obvykle dochází, když zkusíte toostart hostitelský proces služby ručně, nepřidělujte toostart modulu runtime Service Fabric hello ho za vás.</span><span class="sxs-lookup"><span data-stu-id="5c815-137">This error usually occurs when you try toostart a service host process manually, rather than allowing hello Service Fabric runtime toostart it for you.</span></span>

<span data-ttu-id="5c815-138">Ujistěte se, že nemáte žádné projekty služeb nastavit jako spouštěné projekty v řešení.</span><span class="sxs-lookup"><span data-stu-id="5c815-138">Ensure that you do not have any service projects set as startup projects in your solution.</span></span> <span data-ttu-id="5c815-139">Pouze projekty aplikací Service Fabric musí být nastavená jako projektů po spuštění.</span><span class="sxs-lookup"><span data-stu-id="5c815-139">Only Service Fabric application projects should be set as startup projects.</span></span>

> [!TIP]
> <span data-ttu-id="5c815-140">Pokud po instalaci, místní cluster začne toobehave neobvyklým způsobem, můžete obnovit pomocí aplikace hello místní cluster správce systému.</span><span class="sxs-lookup"><span data-stu-id="5c815-140">If, following setup, your local cluster begins toobehave abnormally, you can reset it using hello local cluster manager system tray application.</span></span> <span data-ttu-id="5c815-141">Tato odebere hello stávajícího clusteru a nastavit nový.</span><span class="sxs-lookup"><span data-stu-id="5c815-141">This removes hello existing cluster and set up a new one.</span></span> <span data-ttu-id="5c815-142">Všimněte si, že všechny nasazené aplikace a související data se odeberou.</span><span class="sxs-lookup"><span data-stu-id="5c815-142">Note that all deployed applications and associated data is removed.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="5c815-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5c815-143">Next steps</span></span>
* [<span data-ttu-id="5c815-144">Rady pro pochopení a řešení potíží s clusteru pomocí sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="5c815-144">Understand and troubleshoot your cluster with system health reports</span></span>](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [<span data-ttu-id="5c815-145">Vizualizace clusteru pomocí nástroje Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="5c815-145">Visualize your cluster with Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)

