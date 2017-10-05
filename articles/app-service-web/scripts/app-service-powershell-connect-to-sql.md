---
title: "Azure skript prostředí PowerShell ukázkový – webovou aplikaci připojit k databázi SQL | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – webovou aplikaci připojit k databázi SQL"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 055440a9-fff1-49b2-b964-9c95b364e533
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 5312bf6b81d1cc48490b71c3f77323cca23e1559
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="connect-a-web-app-to-a-sql-database"></a><span data-ttu-id="b551d-103">Webovou aplikaci připojit k databázi SQL</span><span class="sxs-lookup"><span data-stu-id="b551d-103">Connect a web app to a SQL database</span></span>

<span data-ttu-id="b551d-104">V tomto scénáři se dozvíte, jak vytvářet Azure SQL database a webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="b551d-104">In this scenario you will learn how to create an Azure SQL database and an Azure web app.</span></span> <span data-ttu-id="b551d-105">Potom propojíte databáze SQL pro webovou aplikaci pomocí nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="b551d-105">Then you will link the SQL database to the web app using app settings.</span></span>

<span data-ttu-id="b551d-106">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="b551d-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="b551d-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="b551d-107">Sample script</span></span>

<span data-ttu-id="b551d-108">[!code-powershell[hlavní](../../../powershell_scripts/app-service/connect-to-sql/connect-to-sql.ps1?highlight=13 "webovou aplikaci připojit k databázi SQL")]</span><span class="sxs-lookup"><span data-stu-id="b551d-108">[!code-powershell[main](../../../powershell_scripts/app-service/connect-to-sql/connect-to-sql.ps1?highlight=13 "Connect a web app to a SQL database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="b551d-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="b551d-109">Clean up deployment</span></span> 

<span data-ttu-id="b551d-110">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="b551d-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="b551d-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="b551d-111">Script explanation</span></span>

<span data-ttu-id="b551d-112">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="b551d-112">This script uses the following commands.</span></span> <span data-ttu-id="b551d-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="b551d-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="b551d-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="b551d-114">Command</span></span> | <span data-ttu-id="b551d-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b551d-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b551d-116">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="b551d-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="b551d-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="b551d-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b551d-118">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="b551d-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="b551d-119">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="b551d-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="b551d-120">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="b551d-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="b551d-121">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b551d-121">Creates a web app.</span></span> |
| [<span data-ttu-id="b551d-122">Nový AzureRMSQLServer</span><span class="sxs-lookup"><span data-stu-id="b551d-122">New-AzureRMSQLServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="b551d-123">Vytvoří databáze SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="b551d-123">Creates a SQL Database server.</span></span> |
| [<span data-ttu-id="b551d-124">New-AzureRmSqlServerFirewallRule</span><span class="sxs-lookup"><span data-stu-id="b551d-124">New-AzureRmSqlServerFirewallRule</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | <span data-ttu-id="b551d-125">Vytvoří pravidlo brány firewall pro server databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="b551d-125">Creates a firewall rule for a SQL Database server.</span></span> |
| [<span data-ttu-id="b551d-126">Nový AzureRMSQLDatabase</span><span class="sxs-lookup"><span data-stu-id="b551d-126">New-AzureRMSQLDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="b551d-127">Vytvoří databázi nebo elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="b551d-127">Creates a database or an elastic database.</span></span> |
| [<span data-ttu-id="b551d-128">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="b551d-128">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="b551d-129">Upraví konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b551d-129">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b551d-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b551d-130">Next steps</span></span>

<span data-ttu-id="b551d-131">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b551d-131">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="b551d-132">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b551d-132">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
