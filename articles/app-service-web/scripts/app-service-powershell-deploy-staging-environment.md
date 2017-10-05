---
title: "Ukázkový skript prostředí PowerShell Azure – vytvoření webové aplikace a nasazení kódu do pracovního prostředí | Microsoft Docs"
description: "Ukázkový skript prostředí PowerShell Azure – vytvoření webové aplikace a nasazení kódu do pracovního prostředí"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 27cf0680-c3a9-4a58-9f71-6dec09f6b874
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 55adc13350eb0f4711efa3c901f6e4e7755dfb27
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-and-deploy-code-to-a-staging-environment"></a><span data-ttu-id="775e2-103">Vytvoření webové aplikace a nasazení kódu do pracovního prostředí</span><span class="sxs-lookup"><span data-stu-id="775e2-103">Create a web app and deploy code to a staging environment</span></span>

<span data-ttu-id="775e2-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se další nasazovací slot názvem "přípravy" a pak nasadí ukázkovou aplikaci na "pracovní" slot.</span><span class="sxs-lookup"><span data-stu-id="775e2-104">This sample script creates a web app in App Service with an additional deployment slot called "staging", and then deploys a sample app to the "staging" slot.</span></span>

<span data-ttu-id="775e2-105">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="775e2-105">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="775e2-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="775e2-106">Sample script</span></span>

<span data-ttu-id="775e2-107">[!code-powershell[hlavní](../../../powershell_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.ps1?highlight=1 "vytvoření webové aplikace a nasazení kódu do pracovního prostředí")]</span><span class="sxs-lookup"><span data-stu-id="775e2-107">[!code-powershell[main](../../../powershell_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.ps1?highlight=1 "Create a web app and deploy code to a staging environment")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="775e2-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="775e2-108">Clean up deployment</span></span> 

<span data-ttu-id="775e2-109">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="775e2-109">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="775e2-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="775e2-110">Script explanation</span></span>

<span data-ttu-id="775e2-111">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="775e2-111">This script uses the following commands.</span></span> <span data-ttu-id="775e2-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="775e2-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="775e2-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="775e2-113">Command</span></span> | <span data-ttu-id="775e2-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="775e2-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="775e2-115">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="775e2-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="775e2-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="775e2-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="775e2-117">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="775e2-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="775e2-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="775e2-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="775e2-119">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="775e2-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="775e2-120">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="775e2-120">Creates a web app.</span></span> |
| [<span data-ttu-id="775e2-121">Set-AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="775e2-121">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="775e2-122">Upravuje plán služby App Service změnit jeho cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="775e2-122">Modifies an App Service plan to change its pricing tier.</span></span> |
| [<span data-ttu-id="775e2-123">Nové AzureRmWebAppSlot</span><span class="sxs-lookup"><span data-stu-id="775e2-123">New-AzureRmWebAppSlot</span></span>](/powershell/module/azurerm.websites/new-azurermwebappslot) | <span data-ttu-id="775e2-124">Vytvoří se nasazovací slot pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="775e2-124">Creates a deployment slot for a web app.</span></span> |
| [<span data-ttu-id="775e2-125">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="775e2-125">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="775e2-126">Upravuje prostředků ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="775e2-126">Modifies a resource in a resource group.</span></span> |
| [<span data-ttu-id="775e2-127">Swap AzureRmWebAppSlot</span><span class="sxs-lookup"><span data-stu-id="775e2-127">Swap-AzureRmWebAppSlot</span></span>](/powershell/module/azurerm.websites/swap-azurermwebappslot) | <span data-ttu-id="775e2-128">Zamění nasazovací slot webové aplikace do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="775e2-128">Swaps a web app's deployment slot into production.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="775e2-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="775e2-129">Next steps</span></span>

<span data-ttu-id="775e2-130">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="775e2-130">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="775e2-131">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="775e2-131">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
