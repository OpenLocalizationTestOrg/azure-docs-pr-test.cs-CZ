---
title: "Azure ukázkový skript prostředí PowerShell - aplikaci odebrat z clusteru s podporou | Microsoft Docs"
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
ms.openlocfilehash: 05851132c7e5e5877884d29f04bce6c0717ce411
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a><span data-ttu-id="43e87-103">Odebrání aplikace z clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="43e87-103">Remove an application from a Service Fabric cluster</span></span>

<span data-ttu-id="43e87-104">Tento ukázkový skript odstraní spuštěné instance aplikace Service Fabric, zrušení registrace typ a verze aplikace z clusteru a odstranění balíčku aplikace z úložiště imagí clusteru.</span><span class="sxs-lookup"><span data-stu-id="43e87-104">This sample script deletes a running Service Fabric application instance, unregisters an application type and version from the cluster, and deletes the application package from the cluster image store.</span></span>  <span data-ttu-id="43e87-105">Odstranění instance aplikace na také odstraní všechny spuštěné služby instance přidružené s touto aplikací.</span><span class="sxs-lookup"><span data-stu-id="43e87-105">Deleting the application instance also deletes all the running service instances associated with that application.</span></span> <span data-ttu-id="43e87-106">Podle potřeby upravte požadované parametry.</span><span class="sxs-lookup"><span data-stu-id="43e87-106">Customize the parameters as needed.</span></span> 

<span data-ttu-id="43e87-107">V případě potřeby nainstalujte modul Service Fabric prostředí PowerShell s [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="43e87-107">If needed, install the Service Fabric PowerShell module with the [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="43e87-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="43e87-108">Sample script</span></span>

<span data-ttu-id="43e87-109">[!code-powershell[hlavní](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "odebrání aplikace z clusteru")]</span><span class="sxs-lookup"><span data-stu-id="43e87-109">[!code-powershell[main](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "Remove an application from a cluster")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="43e87-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="43e87-110">Script explanation</span></span>

<span data-ttu-id="43e87-111">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="43e87-111">This script uses the following commands.</span></span> <span data-ttu-id="43e87-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="43e87-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="43e87-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="43e87-113">Command</span></span> | <span data-ttu-id="43e87-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="43e87-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="43e87-115">Odebrat ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="43e87-115">Remove-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) | <span data-ttu-id="43e87-116">Odebere spuštěné instance aplikace Service Fabric z clusteru.</span><span class="sxs-lookup"><span data-stu-id="43e87-116">Removes a running Service Fabric application instance from the cluster.</span></span>  |
| [<span data-ttu-id="43e87-117">Zrušit registraci ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="43e87-117">Unregister-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="43e87-118">Zrušení registrace typu aplikace Service Fabric a verze z clusteru.</span><span class="sxs-lookup"><span data-stu-id="43e87-118">Unregisters a Service Fabric application type and version from the cluster.</span></span> |
| [<span data-ttu-id="43e87-119">Odebrat ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="43e87-119">Remove-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="43e87-120">Balíček aplikace Service Fabric se odebere z úložiště bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="43e87-120">Removes a Service Fabric application package from the image store.</span></span>|

## <a name="next-steps"></a><span data-ttu-id="43e87-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="43e87-121">Next steps</span></span>

<span data-ttu-id="43e87-122">Další informace o modulu Service Fabric prostředí PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="43e87-122">For more information on the Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="43e87-123">Další ukázky pro Azure Service Fabric Powershell najdete v [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="43e87-123">Additional Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
