---
title: "Azure ukázkový skript prostředí PowerShell - škálování webové aplikace po celém světě s vysokou dostupností architektura | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - škálování po celém světě s architekturou vysoké dostupnosti webové aplikace"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 470f0129-1efe-462c-a029-5c66e04158a8
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 9acd1cf4d1a5705811c4dedc545505ec0ac55fc7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a><span data-ttu-id="e83fb-103">Škálování webové aplikace po celém světě s vysokou dostupností architektura</span><span class="sxs-lookup"><span data-stu-id="e83fb-103">Scale a web app worldwide with a high-availability architecture</span></span>

<span data-ttu-id="e83fb-104">V tomto scénáři vytvoříte skupinu prostředků, dva druhy služeb aplikace, dva webové aplikace, profil správce provozu a dva koncové body správce provozu.</span><span class="sxs-lookup"><span data-stu-id="e83fb-104">In this scenario you will create a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="e83fb-105">Po dokončení výkonu bude mít s vysokou dostupností architektura, která umožňuje poskytuje globální dostupnost webové aplikace založené na nejnižší latenci sítě.</span><span class="sxs-lookup"><span data-stu-id="e83fb-105">Once the exercise is complete you will have a high-available architecture which allows provides global availability of your web app based on the lowest network latency.</span></span>

<span data-ttu-id="e83fb-106">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="e83fb-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="e83fb-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="e83fb-107">Sample script</span></span>

<span data-ttu-id="e83fb-108">[!code-powershell[hlavní](../../../powershell_scripts/app-service/scale-geographic/scale-geographic.ps1 "škálování webové aplikace po celém světě s vysokou dostupností architektura")]</span><span class="sxs-lookup"><span data-stu-id="e83fb-108">[!code-powershell[main](../../../powershell_scripts/app-service/scale-geographic/scale-geographic.ps1 "Scale a web app worldwide with a high-availability architecture")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="e83fb-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="e83fb-109">Clean up deployment</span></span> 

<span data-ttu-id="e83fb-110">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="e83fb-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="e83fb-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="e83fb-111">Script explanation</span></span>

<span data-ttu-id="e83fb-112">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="e83fb-112">This script uses the following commands.</span></span> <span data-ttu-id="e83fb-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="e83fb-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="e83fb-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="e83fb-114">Command</span></span> | <span data-ttu-id="e83fb-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="e83fb-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e83fb-116">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="e83fb-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="e83fb-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="e83fb-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="e83fb-118">Nové AzureRMTrafficManagerProfile</span><span class="sxs-lookup"><span data-stu-id="e83fb-118">New-AzureRMTrafficManagerProfile</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | <span data-ttu-id="e83fb-119">Vytvoří profil Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="e83fb-119">Creates a Traffic Manager profile.</span></span> |
| [<span data-ttu-id="e83fb-120">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="e83fb-120">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="e83fb-121">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="e83fb-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="e83fb-122">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="e83fb-122">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="e83fb-123">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e83fb-123">Creates a web app.</span></span> |
| [<span data-ttu-id="e83fb-124">Nové AzureRMTrafficManagerEndpoint</span><span class="sxs-lookup"><span data-stu-id="e83fb-124">New-AzureRMTrafficManagerEndpoint</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | <span data-ttu-id="e83fb-125">Vytvoří koncový bod v profilu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="e83fb-125">Creates an endpoint in a Traffic Manager profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e83fb-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e83fb-126">Next steps</span></span>

<span data-ttu-id="e83fb-127">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e83fb-127">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="e83fb-128">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e83fb-128">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
