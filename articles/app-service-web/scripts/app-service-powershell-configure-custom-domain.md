---
title: "aaaAzure ukázkový skript prostředí PowerShell - přiřadit vlastní domény tooa webové aplikace | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - přiřadit vlastní domény tooa webové aplikace"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 356f5af9-f62e-411c-8b24-deba05214103
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 10224e800588019626ef25cbba4a926096779920
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-a-custom-domain-tooa-web-app"></a><span data-ttu-id="de917-103">Přiřadit vlastní domény tooa webové aplikace</span><span class="sxs-lookup"><span data-stu-id="de917-103">Assign a custom domain tooa web app</span></span>

<span data-ttu-id="de917-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a potom mapuje `www.<yourdomain>` tooit.</span><span class="sxs-lookup"><span data-stu-id="de917-104">This sample script creates a web app in App Service with its related resources, and then maps `www.<yourdomain>` tooit.</span></span> 

<span data-ttu-id="de917-105">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="de917-105">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span> <span data-ttu-id="de917-106">Navíc musíte toohave přístup tooyour doménového registrátora na stránku konfigurace služby DNS.</span><span class="sxs-lookup"><span data-stu-id="de917-106">Also, you need toohave access tooyour domain registrar's DNS configuration page.</span></span>

## <a name="sample-script"></a><span data-ttu-id="de917-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="de917-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/map-custom-domain/map-custom-domain.ps1?highlight=1 "Assign a custom domain tooa web app")]

## <a name="clean-up-deployment"></a><span data-ttu-id="de917-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="de917-108">Clean up deployment</span></span> 

<span data-ttu-id="de917-109">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="de917-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="de917-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="de917-110">Script explanation</span></span>

<span data-ttu-id="de917-111">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="de917-111">This script uses hello following commands.</span></span> <span data-ttu-id="de917-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="de917-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="de917-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="de917-113">Command</span></span> | <span data-ttu-id="de917-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="de917-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="de917-115">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="de917-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="de917-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="de917-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="de917-117">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="de917-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="de917-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="de917-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="de917-119">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="de917-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="de917-120">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="de917-120">Creates a web app.</span></span> |
| [<span data-ttu-id="de917-121">Set-AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="de917-121">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="de917-122">Upravuje toochange plán App Service jeho cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="de917-122">Modifies an App Service plan toochange its pricing tier.</span></span> |
| [<span data-ttu-id="de917-123">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="de917-123">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="de917-124">Upraví konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="de917-124">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="de917-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="de917-125">Next steps</span></span>

<span data-ttu-id="de917-126">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="de917-126">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="de917-127">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v hello [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="de917-127">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
