---
title: "aaaAzure ukázkový skript prostředí PowerShell – nasazení aplikace tooa clusteru | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – cluster Service Fabric tooa aplikaci nasadit."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: b417c9908c72f016e930c43ff2d13e0cc5451f46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-tooa-service-fabric-cluster"></a><span data-ttu-id="b30ab-103">Nasazení clusteru Service Fabric tooa aplikace</span><span class="sxs-lookup"><span data-stu-id="b30ab-103">Deploy an application tooa Service Fabric cluster</span></span>

<span data-ttu-id="b30ab-104">Tento ukázkový skript zkopíruje úložišti aplikace clusteru tooa balíček bitové kopie, zaregistruje typ aplikace hello v clusteru hello a vytvoří instanci aplikace z typu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b30ab-104">This sample script copies an application package tooa cluster image store, registers hello application type in hello cluster, and creates an application instance from hello application type.</span></span>  <span data-ttu-id="b30ab-105">Pokud žádné výchozí služby byly definovány v manifestu aplikace hello typu hello cílové aplikace, vytvoří se v tuto chvíli těchto služeb.</span><span class="sxs-lookup"><span data-stu-id="b30ab-105">If any default services were defined in hello application manifest of hello target application type, then those services are created at this time.</span></span> <span data-ttu-id="b30ab-106">Parametry hello přizpůsobte podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="b30ab-106">Customize hello parameters as needed.</span></span> 

<span data-ttu-id="b30ab-107">V případě potřeby nainstalujte modul Service Fabric prostředí PowerShell hello s hello [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b30ab-107">If needed, install hello Service Fabric PowerShell module with hello [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="b30ab-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="b30ab-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application tooa cluster")]

## <a name="clean-up-deployment"></a><span data-ttu-id="b30ab-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="b30ab-109">Clean up deployment</span></span> 

<span data-ttu-id="b30ab-110">Po spuštění ukázka skriptu hello hello skript v [odebrání aplikace](service-fabric-powershell-remove-application.md) může být instanci aplikace hello použité tooremove, zrušení registrace typu aplikace hello a odstranit balíček aplikace hello z úložiště bitových kopií hello.</span><span class="sxs-lookup"><span data-stu-id="b30ab-110">After hello script sample has been run, hello script in [Remove an application](service-fabric-powershell-remove-application.md) can be used tooremove hello application instance, unregister hello application type, and delete hello application package from hello image store.</span></span>

## <a name="script-explanation"></a><span data-ttu-id="b30ab-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="b30ab-111">Script explanation</span></span>

<span data-ttu-id="b30ab-112">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="b30ab-112">This script uses hello following commands.</span></span> <span data-ttu-id="b30ab-113">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="b30ab-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="b30ab-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="b30ab-114">Command</span></span> | <span data-ttu-id="b30ab-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b30ab-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b30ab-116">Kopírování ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="b30ab-116">Copy-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="b30ab-117">Zkopírujte úložišti aplikace clusteru toohello balíček bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="b30ab-117">Copy an application package toohello cluster image store.</span></span>  |
|[<span data-ttu-id="b30ab-118">Registrace ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="b30ab-118">Register-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| <span data-ttu-id="b30ab-119">Zaregistruje typ a verze aplikace v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b30ab-119">Registers an application type and version on hello cluster.</span></span> |
|[<span data-ttu-id="b30ab-120">Nové ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="b30ab-120">New-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| <span data-ttu-id="b30ab-121">Vytvoří aplikace z typu zaregistrovanou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b30ab-121">Creates an application from a registered application type.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b30ab-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b30ab-122">Next steps</span></span>

<span data-ttu-id="b30ab-123">Další informace o modulu hello Service Fabric prostředí PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="b30ab-123">For more information on hello Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="b30ab-124">Další ukázky pro Azure Service Fabric Powershell naleznete v hello [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b30ab-124">Additional Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
