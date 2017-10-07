---
title: "aaaAzure ukázkový skript prostředí PowerShell - připojit databázi SQL webové aplikace tooa | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – připojit databázi SQL tooa webové aplikace"
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
ms.openlocfilehash: 8f80b940378d020cbcaec2c1bbc28bae1a3ef35a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-sql-database"></a><span data-ttu-id="f2390-103">Připojit databáze SQL tooa webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f2390-103">Connect a web app tooa SQL database</span></span>

<span data-ttu-id="f2390-104">V tomto scénáři se dozvíte, jak toocreate Azure SQL database a Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f2390-104">In this scenario you will learn how toocreate an Azure SQL database and an Azure web app.</span></span> <span data-ttu-id="f2390-105">Potom propojíte hello SQL databáze toohello webovou aplikaci pomocí nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="f2390-105">Then you will link hello SQL database toohello web app using app settings.</span></span>

<span data-ttu-id="f2390-106">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="f2390-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="f2390-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="f2390-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/connect-to-sql/connect-to-sql.ps1?highlight=13 "Connect a web app tooa SQL database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="f2390-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="f2390-108">Clean up deployment</span></span> 

<span data-ttu-id="f2390-109">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="f2390-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="f2390-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="f2390-110">Script explanation</span></span>

<span data-ttu-id="f2390-111">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="f2390-111">This script uses hello following commands.</span></span> <span data-ttu-id="f2390-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="f2390-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="f2390-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="f2390-113">Command</span></span> | <span data-ttu-id="f2390-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f2390-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f2390-115">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f2390-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="f2390-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="f2390-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f2390-117">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="f2390-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="f2390-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="f2390-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="f2390-119">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="f2390-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="f2390-120">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f2390-120">Creates a web app.</span></span> |
| [<span data-ttu-id="f2390-121">Nový AzureRMSQLServer</span><span class="sxs-lookup"><span data-stu-id="f2390-121">New-AzureRMSQLServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="f2390-122">Vytvoří databáze SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="f2390-122">Creates a SQL Database server.</span></span> |
| [<span data-ttu-id="f2390-123">New-AzureRmSqlServerFirewallRule</span><span class="sxs-lookup"><span data-stu-id="f2390-123">New-AzureRmSqlServerFirewallRule</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | <span data-ttu-id="f2390-124">Vytvoří pravidlo brány firewall pro server databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="f2390-124">Creates a firewall rule for a SQL Database server.</span></span> |
| [<span data-ttu-id="f2390-125">Nový AzureRMSQLDatabase</span><span class="sxs-lookup"><span data-stu-id="f2390-125">New-AzureRMSQLDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="f2390-126">Vytvoří databázi nebo elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="f2390-126">Creates a database or an elastic database.</span></span> |
| [<span data-ttu-id="f2390-127">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="f2390-127">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="f2390-128">Upraví konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f2390-128">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f2390-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f2390-129">Next steps</span></span>

<span data-ttu-id="f2390-130">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f2390-130">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="f2390-131">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v hello [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f2390-131">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
