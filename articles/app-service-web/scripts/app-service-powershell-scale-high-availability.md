---
title: "aaaAzure ukázkový skript prostředí PowerShell - škálování webové aplikace po celém světě s vysokou dostupností architektura | Microsoft Docs"
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
ms.openlocfilehash: 1fcda23250efe4966d63c5dfa744b76c26f3762a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a><span data-ttu-id="3a873-103">Škálování webové aplikace po celém světě s vysokou dostupností architektura</span><span class="sxs-lookup"><span data-stu-id="3a873-103">Scale a web app worldwide with a high-availability architecture</span></span>

<span data-ttu-id="3a873-104">V tomto scénáři vytvoříte skupinu prostředků, dva druhy služeb aplikace, dva webové aplikace, profil správce provozu a dva koncové body správce provozu.</span><span class="sxs-lookup"><span data-stu-id="3a873-104">In this scenario you will create a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="3a873-105">Po dokončení hello cvičení budete mít s vysokou dostupností architektura, která umožňuje poskytuje globální dostupnost webové aplikace založené na nejnižší latenci sítě hello.</span><span class="sxs-lookup"><span data-stu-id="3a873-105">Once hello exercise is complete you will have a high-available architecture which allows provides global availability of your web app based on hello lowest network latency.</span></span>

<span data-ttu-id="3a873-106">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="3a873-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="3a873-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="3a873-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/scale-geographic/scale-geographic.ps1 "Scale a web app worldwide with a high-availability architecture")]

## <a name="clean-up-deployment"></a><span data-ttu-id="3a873-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="3a873-108">Clean up deployment</span></span> 

<span data-ttu-id="3a873-109">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="3a873-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="3a873-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="3a873-110">Script explanation</span></span>

<span data-ttu-id="3a873-111">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="3a873-111">This script uses hello following commands.</span></span> <span data-ttu-id="3a873-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="3a873-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="3a873-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="3a873-113">Command</span></span> | <span data-ttu-id="3a873-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="3a873-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3a873-115">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="3a873-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="3a873-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="3a873-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="3a873-117">Nové AzureRMTrafficManagerProfile</span><span class="sxs-lookup"><span data-stu-id="3a873-117">New-AzureRMTrafficManagerProfile</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | <span data-ttu-id="3a873-118">Vytvoří profil Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="3a873-118">Creates a Traffic Manager profile.</span></span> |
| [<span data-ttu-id="3a873-119">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="3a873-119">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="3a873-120">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="3a873-120">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="3a873-121">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="3a873-121">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="3a873-122">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3a873-122">Creates a web app.</span></span> |
| [<span data-ttu-id="3a873-123">Nové AzureRMTrafficManagerEndpoint</span><span class="sxs-lookup"><span data-stu-id="3a873-123">New-AzureRMTrafficManagerEndpoint</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | <span data-ttu-id="3a873-124">Vytvoří koncový bod v profilu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="3a873-124">Creates an endpoint in a Traffic Manager profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3a873-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3a873-125">Next steps</span></span>

<span data-ttu-id="3a873-126">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3a873-126">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="3a873-127">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v hello [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="3a873-127">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
