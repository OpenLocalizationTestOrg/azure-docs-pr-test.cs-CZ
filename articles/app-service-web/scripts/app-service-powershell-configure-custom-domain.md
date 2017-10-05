---
title: "Azure ukázkový skript prostředí PowerShell - přiřadit vlastní doménu do webové aplikace | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - přiřadit vlastní doménu do webové aplikace"
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
ms.openlocfilehash: 6d25fe8098848fc69470c77e3200bee554c1f875
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="assign-a-custom-domain-to-a-web-app"></a><span data-ttu-id="c3aae-103">Přiřadit vlastní doménu do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c3aae-103">Assign a custom domain to a web app</span></span>

<span data-ttu-id="c3aae-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a potom mapuje `www.<yourdomain>` k němu.</span><span class="sxs-lookup"><span data-stu-id="c3aae-104">This sample script creates a web app in App Service with its related resources, and then maps `www.<yourdomain>` to it.</span></span> 

<span data-ttu-id="c3aae-105">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="c3aae-105">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span> <span data-ttu-id="c3aae-106">Navíc musíte mít přístup na stránku konfigurace DNS doménového registrátora.</span><span class="sxs-lookup"><span data-stu-id="c3aae-106">Also, you need to have access to your domain registrar's DNS configuration page.</span></span>

## <a name="sample-script"></a><span data-ttu-id="c3aae-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="c3aae-107">Sample script</span></span>

<span data-ttu-id="c3aae-108">[!code-powershell[hlavní](../../../powershell_scripts/app-service/map-custom-domain/map-custom-domain.ps1?highlight=1 "přiřadit vlastní doménu do webové aplikace")]</span><span class="sxs-lookup"><span data-stu-id="c3aae-108">[!code-powershell[main](../../../powershell_scripts/app-service/map-custom-domain/map-custom-domain.ps1?highlight=1 "Assign a custom domain to a web app")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="c3aae-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="c3aae-109">Clean up deployment</span></span> 

<span data-ttu-id="c3aae-110">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="c3aae-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="c3aae-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="c3aae-111">Script explanation</span></span>

<span data-ttu-id="c3aae-112">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="c3aae-112">This script uses the following commands.</span></span> <span data-ttu-id="c3aae-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="c3aae-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="c3aae-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="c3aae-114">Command</span></span> | <span data-ttu-id="c3aae-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="c3aae-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c3aae-116">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c3aae-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="c3aae-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="c3aae-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c3aae-118">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="c3aae-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="c3aae-119">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="c3aae-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="c3aae-120">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="c3aae-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="c3aae-121">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c3aae-121">Creates a web app.</span></span> |
| [<span data-ttu-id="c3aae-122">Set-AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="c3aae-122">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="c3aae-123">Upravuje plán služby App Service změnit jeho cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="c3aae-123">Modifies an App Service plan to change its pricing tier.</span></span> |
| [<span data-ttu-id="c3aae-124">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="c3aae-124">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="c3aae-125">Upraví konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c3aae-125">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c3aae-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c3aae-126">Next steps</span></span>

<span data-ttu-id="c3aae-127">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c3aae-127">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="c3aae-128">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c3aae-128">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
