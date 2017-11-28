---
title: "aaaAzure ukázka skriptu prostředí PowerShell - aplikaci odebrat z clusteru s podporou | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - odebrat aplikaci z clusteru Service Fabric."
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
ms.openlocfilehash: 3fe2082c2fbeffbff1622f206021d4d907197d19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a><span data-ttu-id="783a2-103">Odebrání aplikace z clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="783a2-103">Remove an application from a Service Fabric cluster</span></span>

<span data-ttu-id="783a2-104">Tento ukázkový skript odstraní spuštěné instance aplikace Service Fabric, zrušení registrace typ a verze aplikace z hello clusteru a balíček aplikace hello z úložiště bitových kopií clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="783a2-104">This sample script deletes a running Service Fabric application instance, unregisters an application type and version from hello cluster, and deletes hello application package from hello cluster image store.</span></span>  <span data-ttu-id="783a2-105">Odstraňování instance aplikace hello také odstraní všechny hello spuštění instance služby, které jsou přidružené s touto aplikací.</span><span class="sxs-lookup"><span data-stu-id="783a2-105">Deleting hello application instance also deletes all hello running service instances associated with that application.</span></span> <span data-ttu-id="783a2-106">Parametry hello přizpůsobte podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="783a2-106">Customize hello parameters as needed.</span></span> 

<span data-ttu-id="783a2-107">V případě potřeby nainstalujte modul Service Fabric prostředí PowerShell hello s hello [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="783a2-107">If needed, install hello Service Fabric PowerShell module with hello [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="783a2-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="783a2-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "Remove an application from a cluster")]

## <a name="script-explanation"></a><span data-ttu-id="783a2-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="783a2-109">Script explanation</span></span>

<span data-ttu-id="783a2-110">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="783a2-110">This script uses hello following commands.</span></span> <span data-ttu-id="783a2-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="783a2-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="783a2-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="783a2-112">Command</span></span> | <span data-ttu-id="783a2-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="783a2-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="783a2-114">Odebrat ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="783a2-114">Remove-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) | <span data-ttu-id="783a2-115">Odebere z clusteru hello spuštěné instance aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="783a2-115">Removes a running Service Fabric application instance from hello cluster.</span></span>  |
| [<span data-ttu-id="783a2-116">Zrušit registraci ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="783a2-116">Unregister-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="783a2-117">Zrušení registrace typu aplikace Service Fabric a verze z clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="783a2-117">Unregisters a Service Fabric application type and version from hello cluster.</span></span> |
| [<span data-ttu-id="783a2-118">Odebrat ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="783a2-118">Remove-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="783a2-119">Balíček aplikace Service Fabric se odebere z úložiště bitových kopií hello.</span><span class="sxs-lookup"><span data-stu-id="783a2-119">Removes a Service Fabric application package from hello image store.</span></span>|

## <a name="next-steps"></a><span data-ttu-id="783a2-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="783a2-120">Next steps</span></span>

<span data-ttu-id="783a2-121">Další informace o modulu hello Service Fabric prostředí PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="783a2-121">For more information on hello Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="783a2-122">Další ukázky pro Azure Service Fabric Powershell naleznete v hello [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="783a2-122">Additional Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
