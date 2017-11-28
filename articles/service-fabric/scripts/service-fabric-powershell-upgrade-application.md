---
title: "Upgrade aplikace Service Fabric aaaAzure ukázkový skript prostředí PowerShell - | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - upgradu aplikace Service Fabric."
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
ms.date: 08/23/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 4f4777607bd6b35a76029e09ddb441006565d4cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-service-fabric-application"></a><span data-ttu-id="a3acf-103">Upgrade aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a3acf-103">Upgrade a Service Fabric application</span></span>

<span data-ttu-id="a3acf-104">Tento ukázkový skript provede upgrade spuštěné tooversion instance Service Fabric aplikace 1.3.0.</span><span class="sxs-lookup"><span data-stu-id="a3acf-104">This sample script upgrades a running Service Fabric application instance tooversion 1.3.0.</span></span> <span data-ttu-id="a3acf-105">skript Hello zkopíruje hello nové aplikace balíčku toohello clusteru úložiště image store, zaregistruje typ aplikace hello, spustí monitorovaných upgradu a neustále kontroluje stav upgradu hello až do dokončení upgradu hello nebo vrátí zpět.</span><span class="sxs-lookup"><span data-stu-id="a3acf-105">hello script copies hello new application package toohello cluster image store, registers hello application type, starts a monitored upgrade, and continuously checks hello upgrade status until hello upgrade completes or rolls back.</span></span> <span data-ttu-id="a3acf-106">Parametry hello přizpůsobte podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="a3acf-106">Customize hello parameters as needed.</span></span> 

<span data-ttu-id="a3acf-107">V případě potřeby nainstalujte modul Service Fabric prostředí PowerShell hello s hello [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a3acf-107">If needed, install hello Service Fabric PowerShell module with hello [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="a3acf-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="a3acf-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Upgrade an application")]

## <a name="script-explanation"></a><span data-ttu-id="a3acf-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="a3acf-109">Script explanation</span></span>

<span data-ttu-id="a3acf-110">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="a3acf-110">This script uses hello following commands.</span></span> <span data-ttu-id="a3acf-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="a3acf-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="a3acf-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="a3acf-112">Command</span></span> | <span data-ttu-id="a3acf-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a3acf-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a3acf-114">Get-ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="a3acf-114">Get-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | <span data-ttu-id="a3acf-115">Získá všechny aplikace hello v clusteru Service Fabric hello nebo na konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a3acf-115">Gets all hello applications in hello Service Fabric cluster or a specific application.</span></span>  |
| [<span data-ttu-id="a3acf-116">Get-ServiceFabricApplicationUpgrade</span><span class="sxs-lookup"><span data-stu-id="a3acf-116">Get-ServiceFabricApplicationUpgrade</span></span>](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | <span data-ttu-id="a3acf-117">Získá stav hello upgradu aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a3acf-117">Gets hello status of a Service Fabric application upgrade.</span></span> |
| [<span data-ttu-id="a3acf-118">Get-ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="a3acf-118">Get-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="a3acf-119">Získá typy aplikací Service Fabric hello zaregistrovat u clusteru Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="a3acf-119">Gets hello Service Fabric application types registered on hello Service Fabric cluster.</span></span> |
| [<span data-ttu-id="a3acf-120">Zrušit registraci ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="a3acf-120">Unregister-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="a3acf-121">Zrušení registrace typu aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a3acf-121">Unregisters a Service Fabric application type.</span></span>  |
| [<span data-ttu-id="a3acf-122">Kopírování ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="a3acf-122">Copy-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="a3acf-123">Kopie úložiště bitové kopie toohello balíčku aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a3acf-123">Copies a Service Fabric application package toohello image store.</span></span>  |
| [<span data-ttu-id="a3acf-124">Registrace ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="a3acf-124">Register-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="a3acf-125">Zaregistruje typ aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a3acf-125">Registers a Service Fabric application type.</span></span> |
| [<span data-ttu-id="a3acf-126">Počáteční ServiceFabricApplicationUpgrade</span><span class="sxs-lookup"><span data-stu-id="a3acf-126">Start-ServiceFabricApplicationUpgrade</span></span>](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | <span data-ttu-id="a3acf-127">Upgraduje typ verzi aplikace Service Fabric aplikace toohello zadané aplikace.</span><span class="sxs-lookup"><span data-stu-id="a3acf-127">Upgrades a Service Fabric application toohello specified application type version.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="a3acf-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a3acf-128">Next steps</span></span>

<span data-ttu-id="a3acf-129">Další informace o modulu hello Service Fabric prostředí PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="a3acf-129">For more information on hello Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="a3acf-130">Další ukázky pro Azure Service Fabric Powershell naleznete v hello [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="a3acf-130">Additional Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
