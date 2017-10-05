---
title: "Azure skript prostředí PowerShell ukázkový – webovou aplikaci připojit k účtu úložiště | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – webovou aplikaci připojit k účtu úložiště"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: e4831bdc-2068-4883-9474-0b34c2e3e255
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 481f3efdb1cbbeba328183da7e320c7e5b819b3a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-a-web-app-to-a-storage-account"></a><span data-ttu-id="e2d94-103">Webovou aplikaci připojit k účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="e2d94-103">Connect a web app to a storage account</span></span>

<span data-ttu-id="e2d94-104">V tomto scénáři se dozvíte, jak vytvořit účet úložiště Azure a webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="e2d94-104">In this scenario you will learn how to create an Azure storage account and an Azure web app.</span></span> <span data-ttu-id="e2d94-105">Potom propojíte účet úložiště pro webovou aplikaci pomocí nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2d94-105">Then you will link the storage account to the web app using app settings.</span></span>

<span data-ttu-id="e2d94-106">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="e2d94-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="e2d94-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="e2d94-107">Sample script</span></span>

<span data-ttu-id="e2d94-108">[!code-powershell[hlavní](../../../powershell_scripts/app-service/connect-to-storage/connect-to-storage.ps1 "webovou aplikaci připojit k účtu úložiště")]</span><span class="sxs-lookup"><span data-stu-id="e2d94-108">[!code-powershell[main](../../../powershell_scripts/app-service/connect-to-storage/connect-to-storage.ps1 "Connect a web app to a storage account")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="e2d94-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="e2d94-109">Clean up deployment</span></span> 

<span data-ttu-id="e2d94-110">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="e2d94-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="e2d94-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="e2d94-111">Script explanation</span></span>

<span data-ttu-id="e2d94-112">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="e2d94-112">This script uses the following commands.</span></span> <span data-ttu-id="e2d94-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="e2d94-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="e2d94-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="e2d94-114">Command</span></span> | <span data-ttu-id="e2d94-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="e2d94-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e2d94-116">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="e2d94-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="e2d94-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="e2d94-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="e2d94-118">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="e2d94-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="e2d94-119">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="e2d94-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="e2d94-120">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="e2d94-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="e2d94-121">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e2d94-121">Creates a web app.</span></span> |
| [<span data-ttu-id="e2d94-122">Nové AzureRMStorageAccount</span><span class="sxs-lookup"><span data-stu-id="e2d94-122">New-AzureRMStorageAccount</span></span>](/powershell/module/azurerm.storage/new-azurermstorageaccount) | <span data-ttu-id="e2d94-123">Vytvoří účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e2d94-123">Creates a Storage account.</span></span> |
| [<span data-ttu-id="e2d94-124">Get-AzureRMStorageAccountKey</span><span class="sxs-lookup"><span data-stu-id="e2d94-124">Get-AzureRMStorageAccountKey</span></span>](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | <span data-ttu-id="e2d94-125">Získá přístupové klíče pro účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="e2d94-125">Gets the access keys for an Azure Storage account.</span></span> |
| [<span data-ttu-id="e2d94-126">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="e2d94-126">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="e2d94-127">Upraví konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2d94-127">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e2d94-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e2d94-128">Next steps</span></span>

<span data-ttu-id="e2d94-129">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e2d94-129">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="e2d94-130">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e2d94-130">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
