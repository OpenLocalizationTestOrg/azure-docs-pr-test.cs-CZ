---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvoření webové aplikace a nasazení kódu z místního úložiště Git | Microsoft Docs"
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
ms.openlocfilehash: a90996658bf0b609315460324d0dcd3a411a6512
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a><span data-ttu-id="9d383-103">Vytvoření webové aplikace a nasazení kódu z místního úložiště Git</span><span class="sxs-lookup"><span data-stu-id="9d383-103">Create a web app and deploy code from a local Git repository</span></span>

<span data-ttu-id="9d383-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nasadí kódu webové aplikace v místní úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="9d383-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code in a local Git repository.</span></span>

<span data-ttu-id="9d383-105">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="9d383-105">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span> <span data-ttu-id="9d383-106">Kód aplikace také musí toobe zapsána do místního úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="9d383-106">Also, your application code needs toobe committed into a local Git repository.</span></span>

## <a name="sample-script"></a><span data-ttu-id="9d383-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="9d383-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-local-git/deploy-local-git.ps1?highlight=1 "Create a web app and deploy code from a local Git repository")]

## <a name="clean-up-deployment"></a><span data-ttu-id="9d383-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="9d383-108">Clean up deployment</span></span> 

<span data-ttu-id="9d383-109">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="9d383-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="9d383-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="9d383-110">Script explanation</span></span>

<span data-ttu-id="9d383-111">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="9d383-111">This script uses hello following commands.</span></span> <span data-ttu-id="9d383-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="9d383-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="9d383-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="9d383-113">Command</span></span> | <span data-ttu-id="9d383-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9d383-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9d383-115">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="9d383-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="9d383-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="9d383-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="9d383-117">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="9d383-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="9d383-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="9d383-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="9d383-119">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="9d383-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="9d383-120">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9d383-120">Creates a web app.</span></span> |
| [<span data-ttu-id="9d383-121">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="9d383-121">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="9d383-122">Upravuje prostředků ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="9d383-122">Modifies a resource in a resource group.</span></span> |
| [<span data-ttu-id="9d383-123">Get-AzureRmWebAppPublishingProfile</span><span class="sxs-lookup"><span data-stu-id="9d383-123">Get-AzureRmWebAppPublishingProfile</span></span>](/powershell/module/azurerm.websites/get-azurermwebapppublishingprofile) | <span data-ttu-id="9d383-124">Získáte profil publikování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d383-124">Get a web app's publishing profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9d383-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9d383-125">Next steps</span></span>

<span data-ttu-id="9d383-126">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9d383-126">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="9d383-127">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v hello [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9d383-127">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
