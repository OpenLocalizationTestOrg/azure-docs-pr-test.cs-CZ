---
title: "Azure ukázkový skript prostředí PowerShell - monitorování webové aplikace s protokoly webového serveru | Microsoft Docs"
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
ms.openlocfilehash: 34a3dd318cb9896342fce870922ecd113b3ed08d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-a-web-app-with-web-server-logs"></a><span data-ttu-id="8354c-103">Monitorování webové aplikace s protokoly webového serveru</span><span class="sxs-lookup"><span data-stu-id="8354c-103">Monitor a web app with web server logs</span></span>

<span data-ttu-id="8354c-104">V tomto scénáři vytvoříte skupinu prostředků, plán služby app service, webové aplikace a konfigurace webové aplikace k povolení protokolů webového serveru.</span><span class="sxs-lookup"><span data-stu-id="8354c-104">In this scenario you will create a resource group, app service plan, web app and configure the web app to enable web server logs.</span></span> <span data-ttu-id="8354c-105">Pak si stáhnete souborů protokolu ke kontrole.</span><span class="sxs-lookup"><span data-stu-id="8354c-105">You will then download the log files for review.</span></span>

<span data-ttu-id="8354c-106">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="8354c-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="8354c-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="8354c-107">Sample script</span></span>

<span data-ttu-id="8354c-108">[!code-powershell[hlavní](../../../powershell_scripts/app-service/monitor-with-logs/monitor-with-logs.ps1 "monitorování webové aplikace s protokoly webového serveru")]</span><span class="sxs-lookup"><span data-stu-id="8354c-108">[!code-powershell[main](../../../powershell_scripts/app-service/monitor-with-logs/monitor-with-logs.ps1 "Monitor a web app with web server logs")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="8354c-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="8354c-109">Clean up deployment</span></span> 

<span data-ttu-id="8354c-110">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="8354c-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="8354c-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="8354c-111">Script explanation</span></span>

<span data-ttu-id="8354c-112">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="8354c-112">This script uses the following commands.</span></span> <span data-ttu-id="8354c-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="8354c-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="8354c-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="8354c-114">Command</span></span> | <span data-ttu-id="8354c-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="8354c-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8354c-116">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="8354c-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="8354c-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="8354c-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8354c-118">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="8354c-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="8354c-119">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="8354c-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="8354c-120">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="8354c-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="8354c-121">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8354c-121">Creates a web app.</span></span> |
| [<span data-ttu-id="8354c-122">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="8354c-122">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="8354c-123">Upraví konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="8354c-123">Modifies a web app's configuration.</span></span> |
| [<span data-ttu-id="8354c-124">Get-AzureRMWebAppMetrics</span><span class="sxs-lookup"><span data-stu-id="8354c-124">Get-AzureRMWebAppMetrics</span></span>](/powershell/module/azurerm.websites/get-azurermwebappmetrics) | <span data-ttu-id="8354c-125">Získá metriky webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="8354c-125">Gets a web app's metrics.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8354c-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8354c-126">Next steps</span></span>

<span data-ttu-id="8354c-127">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8354c-127">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="8354c-128">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="8354c-128">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
