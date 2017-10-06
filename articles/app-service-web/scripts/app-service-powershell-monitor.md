---
title: "Ukázka skriptu prostředí PowerShell - aaaAzure monitorování webové aplikace s protokoly webového serveru | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - monitorování webové aplikace s protokoly webového serveru"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 5805d7cd-9e56-4eba-bd85-75b013690ff5
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: efaae1e19f5153e33d1f5d5decadb9f6c4649f8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-web-app-with-web-server-logs"></a><span data-ttu-id="350f9-103">Monitorování webové aplikace s protokoly webového serveru</span><span class="sxs-lookup"><span data-stu-id="350f9-103">Monitor a web app with web server logs</span></span>

<span data-ttu-id="350f9-104">V tomto scénáři vytvoříte skupinu prostředků, plán služby app service, webové aplikace a konfiguraci protokolů hello webové aplikace tooenable webového serveru.</span><span class="sxs-lookup"><span data-stu-id="350f9-104">In this scenario you will create a resource group, app service plan, web app and configure hello web app tooenable web server logs.</span></span> <span data-ttu-id="350f9-105">Pak si stáhnete hello souborů protokolu ke kontrole.</span><span class="sxs-lookup"><span data-stu-id="350f9-105">You will then download hello log files for review.</span></span>

<span data-ttu-id="350f9-106">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="350f9-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="350f9-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="350f9-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/monitor-with-logs/monitor-with-logs.ps1 "Monitor a web app with web server logs")]

## <a name="clean-up-deployment"></a><span data-ttu-id="350f9-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="350f9-108">Clean up deployment</span></span> 

<span data-ttu-id="350f9-109">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="350f9-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="350f9-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="350f9-110">Script explanation</span></span>

<span data-ttu-id="350f9-111">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="350f9-111">This script uses hello following commands.</span></span> <span data-ttu-id="350f9-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="350f9-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="350f9-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="350f9-113">Command</span></span> | <span data-ttu-id="350f9-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="350f9-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="350f9-115">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="350f9-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="350f9-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="350f9-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="350f9-117">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="350f9-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="350f9-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="350f9-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="350f9-119">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="350f9-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="350f9-120">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="350f9-120">Creates a web app.</span></span> |
| [<span data-ttu-id="350f9-121">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="350f9-121">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="350f9-122">Upraví konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="350f9-122">Modifies a web app's configuration.</span></span> |
| [<span data-ttu-id="350f9-123">Get-AzureRMWebAppMetrics</span><span class="sxs-lookup"><span data-stu-id="350f9-123">Get-AzureRMWebAppMetrics</span></span>](/powershell/module/azurerm.websites/get-azurermwebappmetrics) | <span data-ttu-id="350f9-124">Získá metriky webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="350f9-124">Gets a web app's metrics.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="350f9-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="350f9-125">Next steps</span></span>

<span data-ttu-id="350f9-126">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="350f9-126">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="350f9-127">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v hello [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="350f9-127">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
