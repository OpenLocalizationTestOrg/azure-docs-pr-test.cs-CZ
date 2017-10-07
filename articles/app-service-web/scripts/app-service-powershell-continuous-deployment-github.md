---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvořit webovou aplikaci s průběžné nasazování z Githubu | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření webové aplikace s průběžné nasazování z Githubu"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 42f901f8-02f7-4869-b22d-d99ef59f874c
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: c2f260a06bce9af6d11ad4033931d3dc18da8f49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a><span data-ttu-id="cfab0-103">Vytvoření webové aplikace s průběžné nasazování z Githubu</span><span class="sxs-lookup"><span data-stu-id="cfab0-103">Create a web app with continuous deployment from GitHub</span></span>

<span data-ttu-id="cfab0-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nastaví průběžné nasazování z úložiště Githubu.</span><span class="sxs-lookup"><span data-stu-id="cfab0-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a GitHub repository.</span></span> <span data-ttu-id="cfab0-105">GitHub nasazení bez průběžné nasazování, najdete v tématu [vytvoření webové aplikace a nasazení kódu z Githubu](app-service-powershell-deploy-github.md).</span><span class="sxs-lookup"><span data-stu-id="cfab0-105">For GitHub deployment without continuous deployment, see [Create a web app and deploy code from GitHub](app-service-powershell-deploy-github.md).</span></span>

<span data-ttu-id="cfab0-106">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cfab0-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="cfab0-107">Také zajistěte, aby:</span><span class="sxs-lookup"><span data-stu-id="cfab0-107">Also, ensure that:</span></span>

- <span data-ttu-id="cfab0-108">Připojení s Azure byl vytvořen pomocí hello `az login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="cfab0-108">A connection with Azure has been created using hello `az login` command.</span></span>
- <span data-ttu-id="cfab0-109">kód aplikace Hello je v úložišti GitHub veřejných nebo privátních, který vlastníte.</span><span class="sxs-lookup"><span data-stu-id="cfab0-109">hello application code is in a public or private GitHub repository that you own.</span></span>
- <span data-ttu-id="cfab0-110">Máte [vytvoří token přístupu ve vašem účtu GitHub](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).</span><span class="sxs-lookup"><span data-stu-id="cfab0-110">You have [created an access token in your GitHub account](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).</span></span>

## <a name="sample-script"></a><span data-ttu-id="cfab0-111">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="cfab0-111">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-github-continuous/deploy-github-continuous.ps1?highlight=1-2 "Create a web app with continuous deployment from GitHub")]

## <a name="clean-up-deployment"></a><span data-ttu-id="cfab0-112">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="cfab0-112">Clean up deployment</span></span> 

<span data-ttu-id="cfab0-113">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="cfab0-113">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="cfab0-114">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="cfab0-114">Script explanation</span></span>

<span data-ttu-id="cfab0-115">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="cfab0-115">This script uses hello following commands.</span></span> <span data-ttu-id="cfab0-116">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="cfab0-116">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="cfab0-117">Příkaz</span><span class="sxs-lookup"><span data-stu-id="cfab0-117">Command</span></span> | <span data-ttu-id="cfab0-118">Poznámky</span><span class="sxs-lookup"><span data-stu-id="cfab0-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cfab0-119">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="cfab0-119">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="cfab0-120">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="cfab0-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="cfab0-121">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="cfab0-121">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="cfab0-122">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="cfab0-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="cfab0-123">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="cfab0-123">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="cfab0-124">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cfab0-124">Creates a web app.</span></span> |
| [<span data-ttu-id="cfab0-125">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="cfab0-125">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="cfab0-126">Upravuje prostředků ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="cfab0-126">Modifies a resource in a resource group.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="cfab0-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cfab0-127">Next steps</span></span>

<span data-ttu-id="cfab0-128">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cfab0-128">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="cfab0-129">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v hello [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="cfab0-129">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
