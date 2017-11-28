---
title: "aaaAzure ukázkový skript prostředí PowerShell - připojení účtu úložiště webové aplikace tooa | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – připojit účtu úložiště tooa webové aplikace"
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
ms.openlocfilehash: 07693366d32fbaefe92c18df67ded81661e1a2df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-storage-account"></a><span data-ttu-id="d782d-103">Připojení účtu úložiště tooa webové aplikace</span><span class="sxs-lookup"><span data-stu-id="d782d-103">Connect a web app tooa storage account</span></span>

<span data-ttu-id="d782d-104">V tomto scénáři se dozvíte, jak toocreate účet úložiště Azure a Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d782d-104">In this scenario you will learn how toocreate an Azure storage account and an Azure web app.</span></span> <span data-ttu-id="d782d-105">Potom propojíte hello úložiště účet toohello webovou aplikaci pomocí nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="d782d-105">Then you will link hello storage account toohello web app using app settings.</span></span>

<span data-ttu-id="d782d-106">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="d782d-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="d782d-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="d782d-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/connect-to-storage/connect-to-storage.ps1 "Connect a web app tooa storage account")]

## <a name="clean-up-deployment"></a><span data-ttu-id="d782d-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="d782d-108">Clean up deployment</span></span> 

<span data-ttu-id="d782d-109">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="d782d-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="d782d-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="d782d-110">Script explanation</span></span>

<span data-ttu-id="d782d-111">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="d782d-111">This script uses hello following commands.</span></span> <span data-ttu-id="d782d-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="d782d-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="d782d-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="d782d-113">Command</span></span> | <span data-ttu-id="d782d-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d782d-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d782d-115">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d782d-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="d782d-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="d782d-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d782d-117">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="d782d-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="d782d-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="d782d-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="d782d-119">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="d782d-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="d782d-120">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d782d-120">Creates a web app.</span></span> |
| [<span data-ttu-id="d782d-121">Nové AzureRMStorageAccount</span><span class="sxs-lookup"><span data-stu-id="d782d-121">New-AzureRMStorageAccount</span></span>](/powershell/module/azurerm.storage/new-azurermstorageaccount) | <span data-ttu-id="d782d-122">Vytvoří účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d782d-122">Creates a Storage account.</span></span> |
| [<span data-ttu-id="d782d-123">Get-AzureRMStorageAccountKey</span><span class="sxs-lookup"><span data-stu-id="d782d-123">Get-AzureRMStorageAccountKey</span></span>](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | <span data-ttu-id="d782d-124">Získá hello přístupové klíče pro účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d782d-124">Gets hello access keys for an Azure Storage account.</span></span> |
| [<span data-ttu-id="d782d-125">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="d782d-125">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="d782d-126">Upraví konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d782d-126">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d782d-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d782d-127">Next steps</span></span>

<span data-ttu-id="d782d-128">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d782d-128">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="d782d-129">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v hello [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d782d-129">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
