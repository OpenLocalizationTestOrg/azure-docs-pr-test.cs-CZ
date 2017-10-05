---
title: "Ukázkový skript prostředí PowerShell Azure – vytvoření webové aplikace a nasazení kódu z Githubu | Microsoft Docs"
description: "Ukázkový skript prostředí PowerShell Azure – vytvoření webové aplikace a nasazení kódu z Githubu"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 1f7fc21dc12c334f5d347ae9918bd62945bede64
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-and-deploy-code-from-github"></a><span data-ttu-id="d19fe-103">Vytvoření webové aplikace a nasazení kódu z Githubu</span><span class="sxs-lookup"><span data-stu-id="d19fe-103">Create a web app and deploy code from GitHub</span></span>

<span data-ttu-id="d19fe-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nasadí kódu webové aplikace z veřejného úložiště GitHub (bez průběžné nasazování).</span><span class="sxs-lookup"><span data-stu-id="d19fe-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="d19fe-105">Nasazení Githubu se průběžné nasazování, najdete v tématu [vytvořit webovou aplikaci s průběžné nasazování z Githubu](app-service-powershell-continuous-deployment-github.md).</span><span class="sxs-lookup"><span data-stu-id="d19fe-105">For GitHub deployment with continuous deployment, see [Create a web app with continuous deployment from GitHub](app-service-powershell-continuous-deployment-github.md).</span></span>

<span data-ttu-id="d19fe-106">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d19fe-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="d19fe-107">Také je třeba odkaz na úložiště GitHub obsahující kódu webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d19fe-107">Also, you need a link to GitHub repository that contains the web app code.</span></span>

## <a name="sample-script"></a><span data-ttu-id="d19fe-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="d19fe-108">Sample script</span></span>

<span data-ttu-id="d19fe-109">[!code-powershell[hlavní](../../../powershell_scripts/app-service/deploy-github/deploy-github.ps1?highlight=1-2 "vytvoření webové aplikace a nasazení kódu z Githubu")]</span><span class="sxs-lookup"><span data-stu-id="d19fe-109">[!code-powershell[main](../../../powershell_scripts/app-service/deploy-github/deploy-github.ps1?highlight=1-2 "Create a web app and deploy code from GitHub")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="d19fe-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="d19fe-110">Clean up deployment</span></span> 

<span data-ttu-id="d19fe-111">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="d19fe-111">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="d19fe-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="d19fe-112">Script explanation</span></span>

<span data-ttu-id="d19fe-113">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="d19fe-113">This script uses the following commands.</span></span> <span data-ttu-id="d19fe-114">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="d19fe-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="d19fe-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="d19fe-115">Command</span></span> | <span data-ttu-id="d19fe-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d19fe-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d19fe-117">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d19fe-117">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="d19fe-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="d19fe-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d19fe-119">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="d19fe-119">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="d19fe-120">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="d19fe-120">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="d19fe-121">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="d19fe-121">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="d19fe-122">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d19fe-122">Creates a web app.</span></span> |
| [<span data-ttu-id="d19fe-123">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="d19fe-123">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="d19fe-124">Upravuje prostředků ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="d19fe-124">Modifies a resource in a resource group.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d19fe-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d19fe-125">Next steps</span></span>

<span data-ttu-id="d19fe-126">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d19fe-126">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="d19fe-127">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d19fe-127">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
