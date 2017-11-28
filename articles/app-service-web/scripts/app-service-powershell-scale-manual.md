---
title: "aaaAzure ukázkový skript prostředí PowerShell - škálování webové aplikace ručně | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – ruční škálování webové aplikace"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: de5d4285-9c7d-4735-a695-288264047375
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: c749031fbe6c6bcbb25395387b4f32b2ba75cef4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-web-app-manually"></a><span data-ttu-id="5ff9e-103">Ruční škálování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="5ff9e-103">Scale a web app manually</span></span>

<span data-ttu-id="5ff9e-104">V tomto scénáři se dozvíte toocreate skupinu zdrojů, aplikace, služby, plán a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5ff9e-104">In this scenario you will learn toocreate a resource group, app service plan and web app.</span></span> <span data-ttu-id="5ff9e-105">Pak bude škálovat hello plán služby App Service z instancí toomultiple jediné instance.</span><span class="sxs-lookup"><span data-stu-id="5ff9e-105">You will then scale hello App Service Plan from a single instance toomultiple instances.</span></span>

<span data-ttu-id="5ff9e-106">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="5ff9e-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="5ff9e-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="5ff9e-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/scale-manual/scale-manual.ps1 "Scale a web app manually")]

## <a name="clean-up-deployment"></a><span data-ttu-id="5ff9e-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="5ff9e-108">Clean up deployment</span></span> 

<span data-ttu-id="5ff9e-109">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="5ff9e-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="5ff9e-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="5ff9e-110">Script explanation</span></span>

<span data-ttu-id="5ff9e-111">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="5ff9e-111">This script uses hello following commands.</span></span> <span data-ttu-id="5ff9e-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="5ff9e-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="5ff9e-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="5ff9e-113">Command</span></span> | <span data-ttu-id="5ff9e-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="5ff9e-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5ff9e-115">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="5ff9e-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="5ff9e-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="5ff9e-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5ff9e-117">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="5ff9e-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="5ff9e-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="5ff9e-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="5ff9e-119">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="5ff9e-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="5ff9e-120">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5ff9e-120">Creates a web app.</span></span> |
| [<span data-ttu-id="5ff9e-121">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="5ff9e-121">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="5ff9e-122">Upraví konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5ff9e-122">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5ff9e-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5ff9e-123">Next steps</span></span>

<span data-ttu-id="5ff9e-124">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5ff9e-124">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="5ff9e-125">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v hello [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="5ff9e-125">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
