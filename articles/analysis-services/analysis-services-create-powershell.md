---
title: "aaaCreate serveru Azure Analysis Services pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak server toocreate Azure Analysis Services serveru pomocí prostředí PowerShell"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/01/2017
ms.author: owend
ms.custom: mvc
ms.openlocfilehash: 269b78983410f773d47c4cea34d6d353b19f9e91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-analysis-services-server-by-using-powershell"></a><span data-ttu-id="8a528-103">Vytvoření serveru služby Azure Analysis Services pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="8a528-103">Create an Azure Analysis Services server by using PowerShell</span></span>

<span data-ttu-id="8a528-104">Tento rychlý start popisuje pomocí serveru Azure Analysis Services v prostředí PowerShell z příkazového řádku toocreate hello [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="8a528-104">This quickstart describes using PowerShell from hello command line toocreate an Azure Analysis Services server in an [Azure resource group](../azure-resource-manager/resource-group-overview.md) in your Azure subscription.</span></span>

<span data-ttu-id="8a528-105">Tato úloha vyžaduje modul Azure PowerShell verze 4.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="8a528-105">This task requires Azure PowerShell module version 4.0 or later.</span></span> <span data-ttu-id="8a528-106">verze hello toofind, spusťte ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="8a528-106">toofind hello version, run ` Get-Module -ListAvailable AzureRM`.</span></span> <span data-ttu-id="8a528-107">tooinstall nebo aktualizace, najdete v části [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="8a528-107">tooinstall or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span> 

> [!NOTE]
> <span data-ttu-id="8a528-108">Vytvoření serveru může znamenat, že se vám začne fakturovat nová služba.</span><span class="sxs-lookup"><span data-stu-id="8a528-108">Creating a server might result in a new billable service.</span></span> <span data-ttu-id="8a528-109">Další, najdete v části toolearn [ceny služby Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="8a528-109">toolearn more, see [Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8a528-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8a528-110">Prerequisites</span></span>
<span data-ttu-id="8a528-111">toocomplete tento rychlý start, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="8a528-111">toocomplete this quickstart, you need:</span></span>

* <span data-ttu-id="8a528-112">**Předplatné Azure**: navštivte [bezplatná zkušební verze Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate účet.</span><span class="sxs-lookup"><span data-stu-id="8a528-112">**Azure subscription**: Visit [Azure Free Trial](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate an account.</span></span>
* <span data-ttu-id="8a528-113">**Azure Active Directory:** Vaše předplatné musí být přidružené k tenantovi Azure Active Directory a musíte mít účet v tomto adresáři.</span><span class="sxs-lookup"><span data-stu-id="8a528-113">**Azure Active Directory**: Your subscription must be associated with an Azure Active Directory tenant and you must have an account in that directory.</span></span> <span data-ttu-id="8a528-114">Další, najdete v části toolearn [ověřování a uživatel oprávnění](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="8a528-114">toolearn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>

## <a name="import-azurermanalysisservices-module"></a><span data-ttu-id="8a528-115">Import modulu AzureRm.AnalysisServices</span><span class="sxs-lookup"><span data-stu-id="8a528-115">Import AzureRm.AnalysisServices module</span></span>
<span data-ttu-id="8a528-116">toocreate serveru v rámci vašeho předplatného, použijte hello [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) součást modulu.</span><span class="sxs-lookup"><span data-stu-id="8a528-116">toocreate a server in your subscription, you use hello [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)  component module.</span></span> <span data-ttu-id="8a528-117">Načtení modulu AzureRm.AnalysisServices hello do relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8a528-117">Load hello AzureRm.AnalysisServices module into your PowerShell session.</span></span>

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-tooazure"></a><span data-ttu-id="8a528-118">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="8a528-118">Sign in tooAzure</span></span>

<span data-ttu-id="8a528-119">Přihlaste se pomocí hello tooyour předplatného Azure [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) příkaz.</span><span class="sxs-lookup"><span data-stu-id="8a528-119">Sign in tooyour Azure subscription by using hello [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) command.</span></span> <span data-ttu-id="8a528-120">Postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="8a528-120">Follow hello on-screen directions.</span></span>

```powershell
Add-AzureRmAccount
```

## <a name="create-a-resource-group"></a><span data-ttu-id="8a528-121">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="8a528-121">Create a resource group</span></span>
 
<span data-ttu-id="8a528-122">[Skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) je logický kontejner, ve kterém se nasazují a spravují prostředky Azure jako skupina.</span><span class="sxs-lookup"><span data-stu-id="8a528-122">An [Azure resource group](../azure-resource-manager/resource-group-overview.md) is a logical container where Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="8a528-123">Při vytváření serveru je potřeba zadat skupinu prostředků ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="8a528-123">When you create your server, you must specify a resource group in your subscription.</span></span> <span data-ttu-id="8a528-124">Pokud již jste skupinu prostředků, můžete vytvořit novou pomocí hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) příkaz.</span><span class="sxs-lookup"><span data-stu-id="8a528-124">If you do not already have a resource group, you can create a new one by using hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) command.</span></span> <span data-ttu-id="8a528-125">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v oblasti západní USA hello.</span><span class="sxs-lookup"><span data-stu-id="8a528-125">hello following example creates a resource group named `myResourceGroup` in hello West US region.</span></span>

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
```

## <a name="create-a-server"></a><span data-ttu-id="8a528-126">Vytvoření serveru</span><span class="sxs-lookup"><span data-stu-id="8a528-126">Create a server</span></span>

<span data-ttu-id="8a528-127">Vytvoření nového serveru pomocí hello [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) příkaz.</span><span class="sxs-lookup"><span data-stu-id="8a528-127">Create a new server by using hello [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) command.</span></span> <span data-ttu-id="8a528-128">Hello následující příklad vytvoří server s názvem myServer ve myResourceGroup, v oblasti západní USA hello na úroveň hello D1 a určuje philipc@adventureworks.com jako správce serveru.</span><span class="sxs-lookup"><span data-stu-id="8a528-128">hello following example creates a server named myServer in myResourceGroup, in hello West US region, at hello D1 tier, and specifies philipc@adventureworks.com as a server administrator.</span></span>

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myServer" -Location West US -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a><span data-ttu-id="8a528-129">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="8a528-129">Clean up resources</span></span>

<span data-ttu-id="8a528-130">Hello server můžete odebrat ze svého předplatného pomocí hello [odebrat AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) příkaz.</span><span class="sxs-lookup"><span data-stu-id="8a528-130">You can remove hello server from your subscription by using hello [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) command.</span></span> <span data-ttu-id="8a528-131">Pokud budete pokračovat dalšími rychlými starty a kurzy v této kolekci, server neodebírejte.</span><span class="sxs-lookup"><span data-stu-id="8a528-131">If you continue with other quickstarts and tutorials in this collection, do not remove your server.</span></span> <span data-ttu-id="8a528-132">Hello následující příklad odebere server hello vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="8a528-132">hello following example removes hello server created in hello previous step.</span></span>


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myServer" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a><span data-ttu-id="8a528-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a528-133">Next steps</span></span>
<span data-ttu-id="8a528-134">[Správa služby Azure Analysis Services pomocí PowerShellu](analysis-services-powershell.md) </span><span class="sxs-lookup"><span data-stu-id="8a528-134">[Manage Azure Analysis Services with PowerShell](analysis-services-powershell.md) </span></span>  
<span data-ttu-id="8a528-135">[Nasazení modelu ze sady SSDT](analysis-services-deploy.md) </span><span class="sxs-lookup"><span data-stu-id="8a528-135">[Deploy a model from SSDT](analysis-services-deploy.md) </span></span>  
[<span data-ttu-id="8a528-136">Vytvoření modelu na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8a528-136">Create a model in Azure portal</span></span>](analysis-services-create-model-portal.md)