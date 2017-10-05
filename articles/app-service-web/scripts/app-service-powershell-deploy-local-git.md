---
title: "Ukázkový skript prostředí PowerShell Azure – vytvoření webové aplikace a nasazení kódu z místního úložiště Git | Microsoft Docs"
description: "Ukázkový skript prostředí PowerShell Azure – vytvoření webové aplikace a nasazení kódu z místního úložiště Git"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 5a927f23-8e70-45fd-9aae-980d4e7a007d
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 855b8c643bf2a742e763bda2e2c21c6a86331aac
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a><span data-ttu-id="284c0-103">Vytvoření webové aplikace a nasazení kódu z místního úložiště Git</span><span class="sxs-lookup"><span data-stu-id="284c0-103">Create a web app and deploy code from a local Git repository</span></span>

<span data-ttu-id="284c0-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nasadí kódu webové aplikace v místní úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="284c0-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code in a local Git repository.</span></span>

<span data-ttu-id="284c0-105">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="284c0-105">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span> <span data-ttu-id="284c0-106">Kód aplikace také musí být zapsána do místního úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="284c0-106">Also, your application code needs to be committed into a local Git repository.</span></span>

## <a name="sample-script"></a><span data-ttu-id="284c0-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="284c0-107">Sample script</span></span>

<span data-ttu-id="284c0-108">[!code-powershell[hlavní](../../../powershell_scripts/app-service/deploy-local-git/deploy-local-git.ps1?highlight=1 "vytvoření webové aplikace a nasazení kódu z místního úložiště Git")]</span><span class="sxs-lookup"><span data-stu-id="284c0-108">[!code-powershell[main](../../../powershell_scripts/app-service/deploy-local-git/deploy-local-git.ps1?highlight=1 "Create a web app and deploy code from a local Git repository")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="284c0-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="284c0-109">Clean up deployment</span></span> 

<span data-ttu-id="284c0-110">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="284c0-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="284c0-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="284c0-111">Script explanation</span></span>

<span data-ttu-id="284c0-112">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="284c0-112">This script uses the following commands.</span></span> <span data-ttu-id="284c0-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="284c0-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="284c0-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="284c0-114">Command</span></span> | <span data-ttu-id="284c0-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="284c0-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="284c0-116">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="284c0-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="284c0-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="284c0-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="284c0-118">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="284c0-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="284c0-119">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="284c0-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="284c0-120">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="284c0-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="284c0-121">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="284c0-121">Creates a web app.</span></span> |
| [<span data-ttu-id="284c0-122">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="284c0-122">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="284c0-123">Upravuje prostředků ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="284c0-123">Modifies a resource in a resource group.</span></span> |
| [<span data-ttu-id="284c0-124">Get-AzureRmWebAppPublishingProfile</span><span class="sxs-lookup"><span data-stu-id="284c0-124">Get-AzureRmWebAppPublishingProfile</span></span>](/powershell/module/azurerm.websites/get-azurermwebapppublishingprofile) | <span data-ttu-id="284c0-125">Získáte profil publikování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="284c0-125">Get a web app's publishing profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="284c0-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="284c0-126">Next steps</span></span>

<span data-ttu-id="284c0-127">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="284c0-127">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="284c0-128">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="284c0-128">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
