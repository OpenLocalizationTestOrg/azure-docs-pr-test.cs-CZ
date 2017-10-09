---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvoření webové aplikace a nasazení kódu z Githubu | Microsoft Docs"
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
ms.openlocfilehash: 9a28f9cb01b71c86fa0a3f1d0a6761fc3d45d43b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-from-github"></a><span data-ttu-id="ec97f-103">Vytvoření webové aplikace a nasazení kódu z Githubu</span><span class="sxs-lookup"><span data-stu-id="ec97f-103">Create a web app and deploy code from GitHub</span></span>

<span data-ttu-id="ec97f-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nasadí kódu webové aplikace z veřejného úložiště GitHub (bez průběžné nasazování).</span><span class="sxs-lookup"><span data-stu-id="ec97f-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="ec97f-105">Nasazení Githubu se průběžné nasazování, najdete v tématu [vytvořit webovou aplikaci s průběžné nasazování z Githubu](app-service-powershell-continuous-deployment-github.md).</span><span class="sxs-lookup"><span data-stu-id="ec97f-105">For GitHub deployment with continuous deployment, see [Create a web app with continuous deployment from GitHub](app-service-powershell-continuous-deployment-github.md).</span></span>

<span data-ttu-id="ec97f-106">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ec97f-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="ec97f-107">Navíc musíte úložiště tooGitHub odkaz obsahující kódu webové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="ec97f-107">Also, you need a link tooGitHub repository that contains hello web app code.</span></span>

## <a name="sample-script"></a><span data-ttu-id="ec97f-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="ec97f-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-github/deploy-github.ps1?highlight=1-2 "Create a web app and deploy code from GitHub")]

## <a name="clean-up-deployment"></a><span data-ttu-id="ec97f-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="ec97f-109">Clean up deployment</span></span> 

<span data-ttu-id="ec97f-110">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="ec97f-110">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="ec97f-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="ec97f-111">Script explanation</span></span>

<span data-ttu-id="ec97f-112">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="ec97f-112">This script uses hello following commands.</span></span> <span data-ttu-id="ec97f-113">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="ec97f-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="ec97f-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="ec97f-114">Command</span></span> | <span data-ttu-id="ec97f-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="ec97f-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ec97f-116">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="ec97f-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="ec97f-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="ec97f-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ec97f-118">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="ec97f-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="ec97f-119">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="ec97f-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="ec97f-120">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="ec97f-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="ec97f-121">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ec97f-121">Creates a web app.</span></span> |
| [<span data-ttu-id="ec97f-122">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="ec97f-122">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="ec97f-123">Upravuje prostředků ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="ec97f-123">Modifies a resource in a resource group.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ec97f-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec97f-124">Next steps</span></span>

<span data-ttu-id="ec97f-125">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ec97f-125">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="ec97f-126">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v hello [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ec97f-126">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
