---
title: "Azure skript prostředí PowerShell ukázkový – vytvoření webové aplikace s průběžné nasazování z Githubu | Microsoft Docs"
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
ms.openlocfilehash: fc594a94bb64ceb88370be8617036a73402ade4e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a><span data-ttu-id="68abf-103">Vytvoření webové aplikace s průběžné nasazování z Githubu</span><span class="sxs-lookup"><span data-stu-id="68abf-103">Create a web app with continuous deployment from GitHub</span></span>

<span data-ttu-id="68abf-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nastaví průběžné nasazování z úložiště Githubu.</span><span class="sxs-lookup"><span data-stu-id="68abf-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a GitHub repository.</span></span> <span data-ttu-id="68abf-105">GitHub nasazení bez průběžné nasazování, najdete v tématu [vytvoření webové aplikace a nasazení kódu z Githubu](app-service-powershell-deploy-github.md).</span><span class="sxs-lookup"><span data-stu-id="68abf-105">For GitHub deployment without continuous deployment, see [Create a web app and deploy code from GitHub](app-service-powershell-deploy-github.md).</span></span>

<span data-ttu-id="68abf-106">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="68abf-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="68abf-107">Také zajistěte, aby:</span><span class="sxs-lookup"><span data-stu-id="68abf-107">Also, ensure that:</span></span>

- <span data-ttu-id="68abf-108">Připojení s Azure byla vytvořena pomocí `az login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="68abf-108">A connection with Azure has been created using the `az login` command.</span></span>
- <span data-ttu-id="68abf-109">Kód aplikace je v úložišti GitHub veřejných nebo privátních, který vlastníte.</span><span class="sxs-lookup"><span data-stu-id="68abf-109">The application code is in a public or private GitHub repository that you own.</span></span>
- <span data-ttu-id="68abf-110">Máte [vytvoří token přístupu ve vašem účtu GitHub](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).</span><span class="sxs-lookup"><span data-stu-id="68abf-110">You have [created an access token in your GitHub account](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).</span></span>

## <a name="sample-script"></a><span data-ttu-id="68abf-111">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="68abf-111">Sample script</span></span>

<span data-ttu-id="68abf-112">[!code-powershell[hlavní](../../../powershell_scripts/app-service/deploy-github-continuous/deploy-github-continuous.ps1?highlight=1-2 "vytvořit webovou aplikaci s průběžné nasazování z Githubu")]</span><span class="sxs-lookup"><span data-stu-id="68abf-112">[!code-powershell[main](../../../powershell_scripts/app-service/deploy-github-continuous/deploy-github-continuous.ps1?highlight=1-2 "Create a web app with continuous deployment from GitHub")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="68abf-113">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="68abf-113">Clean up deployment</span></span> 

<span data-ttu-id="68abf-114">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="68abf-114">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="68abf-115">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="68abf-115">Script explanation</span></span>

<span data-ttu-id="68abf-116">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="68abf-116">This script uses the following commands.</span></span> <span data-ttu-id="68abf-117">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="68abf-117">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="68abf-118">Příkaz</span><span class="sxs-lookup"><span data-stu-id="68abf-118">Command</span></span> | <span data-ttu-id="68abf-119">Poznámky</span><span class="sxs-lookup"><span data-stu-id="68abf-119">Notes</span></span> |
|---|---|
| [<span data-ttu-id="68abf-120">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="68abf-120">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="68abf-121">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="68abf-121">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="68abf-122">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="68abf-122">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="68abf-123">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="68abf-123">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="68abf-124">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="68abf-124">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="68abf-125">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="68abf-125">Creates a web app.</span></span> |
| [<span data-ttu-id="68abf-126">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="68abf-126">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="68abf-127">Upravuje prostředků ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="68abf-127">Modifies a resource in a resource group.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="68abf-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="68abf-128">Next steps</span></span>

<span data-ttu-id="68abf-129">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="68abf-129">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="68abf-130">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="68abf-130">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
