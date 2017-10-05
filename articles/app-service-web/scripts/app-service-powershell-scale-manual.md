---
title: "Azure skript prostředí PowerShell ukázkový – škálování webové aplikace ručně | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – ruční škálování webové aplikace"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: de5d4285-9c7d-4735-a695-288264047375
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: e99dfc02b6ab4123cd5f95997285dca5cb686380
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="scale-a-web-app-manually"></a><span data-ttu-id="82a10-103">Ruční škálování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="82a10-103">Scale a web app manually</span></span>

<span data-ttu-id="82a10-104">V tomto scénáři se dozvíte, pro vytvoření skupiny prostředků, aplikace, služby, plán a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="82a10-104">In this scenario you will learn to create a resource group, app service plan and web app.</span></span> <span data-ttu-id="82a10-105">Pak bude škálovat, plán služby App Service z jediné instance na více instancí.</span><span class="sxs-lookup"><span data-stu-id="82a10-105">You will then scale the App Service Plan from a single instance to multiple instances.</span></span>

<span data-ttu-id="82a10-106">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="82a10-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="82a10-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="82a10-107">Sample script</span></span>

<span data-ttu-id="82a10-108">[!code-powershell[hlavní](../../../powershell_scripts/app-service/scale-manual/scale-manual.ps1 "ručně škálování webové aplikace")]</span><span class="sxs-lookup"><span data-stu-id="82a10-108">[!code-powershell[main](../../../powershell_scripts/app-service/scale-manual/scale-manual.ps1 "Scale a web app manually")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="82a10-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="82a10-109">Clean up deployment</span></span> 

<span data-ttu-id="82a10-110">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="82a10-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="82a10-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="82a10-111">Script explanation</span></span>

<span data-ttu-id="82a10-112">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="82a10-112">This script uses the following commands.</span></span> <span data-ttu-id="82a10-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="82a10-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="82a10-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="82a10-114">Command</span></span> | <span data-ttu-id="82a10-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="82a10-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="82a10-116">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="82a10-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="82a10-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="82a10-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="82a10-118">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="82a10-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="82a10-119">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="82a10-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="82a10-120">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="82a10-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="82a10-121">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="82a10-121">Creates a web app.</span></span> |
| [<span data-ttu-id="82a10-122">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="82a10-122">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="82a10-123">Upraví konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="82a10-123">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="82a10-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="82a10-124">Next steps</span></span>

<span data-ttu-id="82a10-125">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="82a10-125">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="82a10-126">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="82a10-126">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
