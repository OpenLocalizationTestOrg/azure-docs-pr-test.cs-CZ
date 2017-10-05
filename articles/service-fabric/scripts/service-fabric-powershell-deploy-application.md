---
title: "Azure skript prostředí PowerShell ukázkový – nasazení aplikace do clusteru s podporou | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – nasazení aplikace do clusteru Service Fabric."
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
ms.openlocfilehash: 2863823205dbd70f63948ecd4af8898220fe1ff8
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a><span data-ttu-id="1cd9a-103">Nasazení aplikace do clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1cd9a-103">Deploy an application to a Service Fabric cluster</span></span>

<span data-ttu-id="1cd9a-104">Tento ukázkový skript zkopíruje balíček aplikace do úložiště bitové kopie clusteru, zaregistruje typ aplikace v clusteru a vytvoří instanci aplikace z typu aplikace.</span><span class="sxs-lookup"><span data-stu-id="1cd9a-104">This sample script copies an application package to a cluster image store, registers the application type in the cluster, and creates an application instance from the application type.</span></span>  <span data-ttu-id="1cd9a-105">Pokud žádné výchozí služby byly definovány v manifestu aplikace cílového typu aplikace, vytvoří se v tuto chvíli těchto služeb.</span><span class="sxs-lookup"><span data-stu-id="1cd9a-105">If any default services were defined in the application manifest of the target application type, then those services are created at this time.</span></span> <span data-ttu-id="1cd9a-106">Podle potřeby upravte požadované parametry.</span><span class="sxs-lookup"><span data-stu-id="1cd9a-106">Customize the parameters as needed.</span></span> 

<span data-ttu-id="1cd9a-107">V případě potřeby nainstalujte modul Service Fabric prostředí PowerShell s [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1cd9a-107">If needed, install the Service Fabric PowerShell module with the [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="1cd9a-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="1cd9a-108">Sample script</span></span>

<span data-ttu-id="1cd9a-109">[!code-powershell[hlavní](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "nasazení aplikace do clusteru")]</span><span class="sxs-lookup"><span data-stu-id="1cd9a-109">[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application to a cluster")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="1cd9a-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="1cd9a-110">Clean up deployment</span></span> 

<span data-ttu-id="1cd9a-111">Po ukázka skriptu spuštění, skript [odebrání aplikace](service-fabric-powershell-remove-application.md) slouží k odebrání instance aplikace, zrušení registrace typu aplikace a odstranění balíčku aplikace z úložiště bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="1cd9a-111">After the script sample has been run, the script in [Remove an application](service-fabric-powershell-remove-application.md) can be used to remove the application instance, unregister the application type, and delete the application package from the image store.</span></span>

## <a name="script-explanation"></a><span data-ttu-id="1cd9a-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="1cd9a-112">Script explanation</span></span>

<span data-ttu-id="1cd9a-113">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="1cd9a-113">This script uses the following commands.</span></span> <span data-ttu-id="1cd9a-114">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="1cd9a-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="1cd9a-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="1cd9a-115">Command</span></span> | <span data-ttu-id="1cd9a-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1cd9a-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1cd9a-117">Kopírování ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="1cd9a-117">Copy-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="1cd9a-118">Zkopírujte balíček aplikace do úložiště bitových kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="1cd9a-118">Copy an application package to the cluster image store.</span></span>  |
|[<span data-ttu-id="1cd9a-119">Registrace ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="1cd9a-119">Register-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| <span data-ttu-id="1cd9a-120">Zaregistruje typ a verze aplikace v clusteru.</span><span class="sxs-lookup"><span data-stu-id="1cd9a-120">Registers an application type and version on the cluster.</span></span> |
|[<span data-ttu-id="1cd9a-121">Nové ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="1cd9a-121">New-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| <span data-ttu-id="1cd9a-122">Vytvoří aplikace z typu zaregistrovanou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1cd9a-122">Creates an application from a registered application type.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1cd9a-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1cd9a-123">Next steps</span></span>

<span data-ttu-id="1cd9a-124">Další informace o modulu Service Fabric prostředí PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="1cd9a-124">For more information on the Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="1cd9a-125">Další ukázky pro Azure Service Fabric Powershell najdete v [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="1cd9a-125">Additional Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
