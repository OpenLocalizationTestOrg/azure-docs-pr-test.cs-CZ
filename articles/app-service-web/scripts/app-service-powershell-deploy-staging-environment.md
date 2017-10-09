---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvoření webové aplikace a nasazení kódu tooa pracovní prostředí | Microsoft Docs"
description: "Ukázkový skript prostředí PowerShell Azure – vytvoření webové aplikace a nasazení kódu tooa pracovní prostředí"
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
ms.openlocfilehash: 5c74b962955770637173f1fd4f49342fec54ae3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-tooa-staging-environment"></a><span data-ttu-id="4b223-103">Vytvoření webové aplikace a nasazení kódu tooa pracovní prostředí</span><span class="sxs-lookup"><span data-stu-id="4b223-103">Create a web app and deploy code tooa staging environment</span></span>

<span data-ttu-id="4b223-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se další nasazovací slot názvem "přípravy" a pak nasadí ukázkové aplikace toohello "pracovní" slot.</span><span class="sxs-lookup"><span data-stu-id="4b223-104">This sample script creates a web app in App Service with an additional deployment slot called "staging", and then deploys a sample app toohello "staging" slot.</span></span>

<span data-ttu-id="4b223-105">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="4b223-105">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="4b223-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="4b223-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.ps1?highlight=1 "Create a web app and deploy code tooa staging environment")]

## <a name="clean-up-deployment"></a><span data-ttu-id="4b223-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="4b223-107">Clean up deployment</span></span> 

<span data-ttu-id="4b223-108">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="4b223-108">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="4b223-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="4b223-109">Script explanation</span></span>

<span data-ttu-id="4b223-110">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="4b223-110">This script uses hello following commands.</span></span> <span data-ttu-id="4b223-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="4b223-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="4b223-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="4b223-112">Command</span></span> | <span data-ttu-id="4b223-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="4b223-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4b223-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4b223-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="4b223-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="4b223-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4b223-116">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="4b223-116">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="4b223-117">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="4b223-117">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="4b223-118">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="4b223-118">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="4b223-119">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4b223-119">Creates a web app.</span></span> |
| [<span data-ttu-id="4b223-120">Set-AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="4b223-120">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="4b223-121">Upravuje toochange plán App Service jeho cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="4b223-121">Modifies an App Service plan toochange its pricing tier.</span></span> |
| [<span data-ttu-id="4b223-122">Nové AzureRmWebAppSlot</span><span class="sxs-lookup"><span data-stu-id="4b223-122">New-AzureRmWebAppSlot</span></span>](/powershell/module/azurerm.websites/new-azurermwebappslot) | <span data-ttu-id="4b223-123">Vytvoří se nasazovací slot pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4b223-123">Creates a deployment slot for a web app.</span></span> |
| [<span data-ttu-id="4b223-124">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="4b223-124">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="4b223-125">Upravuje prostředků ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="4b223-125">Modifies a resource in a resource group.</span></span> |
| [<span data-ttu-id="4b223-126">Swap AzureRmWebAppSlot</span><span class="sxs-lookup"><span data-stu-id="4b223-126">Swap-AzureRmWebAppSlot</span></span>](/powershell/module/azurerm.websites/swap-azurermwebappslot) | <span data-ttu-id="4b223-127">Zamění nasazovací slot webové aplikace do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="4b223-127">Swaps a web app's deployment slot into production.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4b223-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4b223-128">Next steps</span></span>

<span data-ttu-id="4b223-129">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4b223-129">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="4b223-130">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v hello [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4b223-130">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
