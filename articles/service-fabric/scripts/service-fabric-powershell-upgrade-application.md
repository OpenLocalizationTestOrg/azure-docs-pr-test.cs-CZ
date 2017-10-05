---
title: "Azure ukázkový skript prostředí PowerShell - upgradu aplikace Service Fabric | Microsoft Docs"
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
ms.openlocfilehash: 454849f82ddb23ddb9d71459f86e3cf5a1589254
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="upgrade-a-service-fabric-application"></a><span data-ttu-id="945ad-103">Upgrade aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="945ad-103">Upgrade a Service Fabric application</span></span>

<span data-ttu-id="945ad-104">Tento ukázkový skript upgraduje na verzi 1.3.0 spuštěné instance aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="945ad-104">This sample script upgrades a running Service Fabric application instance to version 1.3.0.</span></span> <span data-ttu-id="945ad-105">Skript zkopíruje nový balíček aplikace do úložiště bitových kopií clusteru, se zaregistruje typ aplikace, spustí monitorovaných upgradu a neustále kontroluje stav upgradu, až do dokončení upgradu nebo vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="945ad-105">The script copies the new application package to the cluster image store, registers the application type, starts a monitored upgrade, and continuously checks the upgrade status until the upgrade completes or rolls back.</span></span> <span data-ttu-id="945ad-106">Podle potřeby upravte požadované parametry.</span><span class="sxs-lookup"><span data-stu-id="945ad-106">Customize the parameters as needed.</span></span> 

<span data-ttu-id="945ad-107">V případě potřeby nainstalujte modul Service Fabric prostředí PowerShell s [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="945ad-107">If needed, install the Service Fabric PowerShell module with the [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="945ad-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="945ad-108">Sample script</span></span>

<span data-ttu-id="945ad-109">[!code-powershell[hlavní](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Upgrade aplikace")]</span><span class="sxs-lookup"><span data-stu-id="945ad-109">[!code-powershell[main](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Upgrade an application")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="945ad-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="945ad-110">Script explanation</span></span>

<span data-ttu-id="945ad-111">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="945ad-111">This script uses the following commands.</span></span> <span data-ttu-id="945ad-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="945ad-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="945ad-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="945ad-113">Command</span></span> | <span data-ttu-id="945ad-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="945ad-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="945ad-115">Get-ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="945ad-115">Get-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | <span data-ttu-id="945ad-116">Získá všechny aplikace v clusteru Service Fabric nebo na konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="945ad-116">Gets all the applications in the Service Fabric cluster or a specific application.</span></span>  |
| [<span data-ttu-id="945ad-117">Get-ServiceFabricApplicationUpgrade</span><span class="sxs-lookup"><span data-stu-id="945ad-117">Get-ServiceFabricApplicationUpgrade</span></span>](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | <span data-ttu-id="945ad-118">Získá stav upgradu aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="945ad-118">Gets the status of a Service Fabric application upgrade.</span></span> |
| [<span data-ttu-id="945ad-119">Get-ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="945ad-119">Get-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="945ad-120">Získá typy aplikací Service Fabric zaregistrovat u clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="945ad-120">Gets the Service Fabric application types registered on the Service Fabric cluster.</span></span> |
| [<span data-ttu-id="945ad-121">Zrušit registraci ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="945ad-121">Unregister-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="945ad-122">Zrušení registrace typu aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="945ad-122">Unregisters a Service Fabric application type.</span></span>  |
| [<span data-ttu-id="945ad-123">Kopírování ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="945ad-123">Copy-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="945ad-124">Balíček aplikace Service Fabric se zkopíruje do úložiště bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="945ad-124">Copies a Service Fabric application package to the image store.</span></span>  |
| [<span data-ttu-id="945ad-125">Registrace ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="945ad-125">Register-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="945ad-126">Zaregistruje typ aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="945ad-126">Registers a Service Fabric application type.</span></span> |
| [<span data-ttu-id="945ad-127">Počáteční ServiceFabricApplicationUpgrade</span><span class="sxs-lookup"><span data-stu-id="945ad-127">Start-ServiceFabricApplicationUpgrade</span></span>](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | <span data-ttu-id="945ad-128">Upgraduje aplikace Service Fabric na verzi typ zadané aplikace.</span><span class="sxs-lookup"><span data-stu-id="945ad-128">Upgrades a Service Fabric application to the specified application type version.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="945ad-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="945ad-129">Next steps</span></span>

<span data-ttu-id="945ad-130">Další informace o modulu Service Fabric prostředí PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="945ad-130">For more information on the Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="945ad-131">Další ukázky pro Azure Service Fabric Powershell najdete v [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="945ad-131">Additional Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
