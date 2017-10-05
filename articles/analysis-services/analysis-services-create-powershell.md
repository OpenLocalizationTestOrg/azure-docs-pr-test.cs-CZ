---
title: "Vytvoření serveru služby Azure Analysis Services pomocí PowerShellu | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit server služby Azure Analysis Services pomocí PowerShellu."
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
ms.openlocfilehash: cb42fd3ed51364cf478848cc51ebbb2f175e96d2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-azure-analysis-services-server-by-using-powershell"></a><span data-ttu-id="01829-103">Vytvoření serveru služby Azure Analysis Services pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="01829-103">Create an Azure Analysis Services server by using PowerShell</span></span>

<span data-ttu-id="01829-104">Tento rychlý start popisuje použití PowerShellu z příkazového řádku k vytvoření serveru Azure Analysis Services ve [skupině prostředků Azure](../azure-resource-manager/resource-group-overview.md) ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="01829-104">This quickstart describes using PowerShell from the command line to create an Azure Analysis Services server in an [Azure resource group](../azure-resource-manager/resource-group-overview.md) in your Azure subscription.</span></span>

<span data-ttu-id="01829-105">Tato úloha vyžaduje modul Azure PowerShell verze 4.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="01829-105">This task requires Azure PowerShell module version 4.0 or later.</span></span> <span data-ttu-id="01829-106">Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="01829-106">To find the version, run ` Get-Module -ListAvailable AzureRM`.</span></span> <span data-ttu-id="01829-107">Pokud chcete provést instalaci nebo upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="01829-107">To install or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span> 

> [!NOTE]
> <span data-ttu-id="01829-108">Vytvoření serveru může znamenat, že se vám začne fakturovat nová služba.</span><span class="sxs-lookup"><span data-stu-id="01829-108">Creating a server might result in a new billable service.</span></span> <span data-ttu-id="01829-109">Další informace najdete v tématu [Ceny služby Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="01829-109">To learn more, see [Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01829-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="01829-110">Prerequisites</span></span>
<span data-ttu-id="01829-111">K dokončení tohoto rychlého startu je potřeba:</span><span class="sxs-lookup"><span data-stu-id="01829-111">To complete this quickstart, you need:</span></span>

* <span data-ttu-id="01829-112">**Předplatné Azure:** Pokud si chcete vytvořit účet, přejděte na stránku [Bezplatný zkušební verze Azure](https://azure.microsoft.com/offers/ms-azr-0044p/).</span><span class="sxs-lookup"><span data-stu-id="01829-112">**Azure subscription**: Visit [Azure Free Trial](https://azure.microsoft.com/offers/ms-azr-0044p/) to create an account.</span></span>
* <span data-ttu-id="01829-113">**Azure Active Directory:** Vaše předplatné musí být přidružené k tenantovi Azure Active Directory a musíte mít účet v tomto adresáři.</span><span class="sxs-lookup"><span data-stu-id="01829-113">**Azure Active Directory**: Your subscription must be associated with an Azure Active Directory tenant and you must have an account in that directory.</span></span> <span data-ttu-id="01829-114">Další informace najdete v tématu [Ověřování a uživatelská oprávnění](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="01829-114">To learn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>

## <a name="import-azurermanalysisservices-module"></a><span data-ttu-id="01829-115">Import modulu AzureRm.AnalysisServices</span><span class="sxs-lookup"><span data-stu-id="01829-115">Import AzureRm.AnalysisServices module</span></span>
<span data-ttu-id="01829-116">K vytvoření serveru ve vašem předplatném můžete použít modul komponenty [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices).</span><span class="sxs-lookup"><span data-stu-id="01829-116">To create a server in your subscription, you use the [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)  component module.</span></span> <span data-ttu-id="01829-117">Načtěte modul AzureRm.AnalysisServices do relace PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="01829-117">Load the AzureRm.AnalysisServices module into your PowerShell session.</span></span>

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-to-azure"></a><span data-ttu-id="01829-118">Přihlášení k Azure</span><span class="sxs-lookup"><span data-stu-id="01829-118">Sign in to Azure</span></span>

<span data-ttu-id="01829-119">Přihlaste se ke svému předplatnému Azure pomocí příkazu [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount).</span><span class="sxs-lookup"><span data-stu-id="01829-119">Sign in to your Azure subscription by using the [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) command.</span></span> <span data-ttu-id="01829-120">Postupujte podle pokynů na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="01829-120">Follow the on-screen directions.</span></span>

```powershell
Add-AzureRmAccount
```

## <a name="create-a-resource-group"></a><span data-ttu-id="01829-121">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="01829-121">Create a resource group</span></span>
 
<span data-ttu-id="01829-122">[Skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) je logický kontejner, ve kterém se nasazují a spravují prostředky Azure jako skupina.</span><span class="sxs-lookup"><span data-stu-id="01829-122">An [Azure resource group](../azure-resource-manager/resource-group-overview.md) is a logical container where Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="01829-123">Při vytváření serveru je potřeba zadat skupinu prostředků ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="01829-123">When you create your server, you must specify a resource group in your subscription.</span></span> <span data-ttu-id="01829-124">Pokud skupinu prostředků ještě nemáte, můžete vytvořit novou pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="01829-124">If you do not already have a resource group, you can create a new one by using the [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) command.</span></span> <span data-ttu-id="01829-125">Následující příklad vytvoří skupinu prostředků `myResourceGroup` v oblasti USA – západ.</span><span class="sxs-lookup"><span data-stu-id="01829-125">The following example creates a resource group named `myResourceGroup` in the West US region.</span></span>

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
```

## <a name="create-a-server"></a><span data-ttu-id="01829-126">Vytvoření serveru</span><span class="sxs-lookup"><span data-stu-id="01829-126">Create a server</span></span>

<span data-ttu-id="01829-127">Vytvořte nový server pomocí příkazu [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver).</span><span class="sxs-lookup"><span data-stu-id="01829-127">Create a new server by using the [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) command.</span></span> <span data-ttu-id="01829-128">Následující příklad vytvoří server myServer ve skupině prostředků myResourceGroup v oblasti USA – západ na úrovni D1 a určí philipc@adventureworks.com jako správce serveru.</span><span class="sxs-lookup"><span data-stu-id="01829-128">The following example creates a server named myServer in myResourceGroup, in the West US region, at the D1 tier, and specifies philipc@adventureworks.com as a server administrator.</span></span>

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myServer" -Location West US -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a><span data-ttu-id="01829-129">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="01829-129">Clean up resources</span></span>

<span data-ttu-id="01829-130">Server můžete z předplatného odebrat pomocí příkazu [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver).</span><span class="sxs-lookup"><span data-stu-id="01829-130">You can remove the server from your subscription by using the [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) command.</span></span> <span data-ttu-id="01829-131">Pokud budete pokračovat dalšími rychlými starty a kurzy v této kolekci, server neodebírejte.</span><span class="sxs-lookup"><span data-stu-id="01829-131">If you continue with other quickstarts and tutorials in this collection, do not remove your server.</span></span> <span data-ttu-id="01829-132">Následující příklad odebere server vytvořený v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="01829-132">The following example removes the server created in the previous step.</span></span>


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myServer" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a><span data-ttu-id="01829-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="01829-133">Next steps</span></span>
<span data-ttu-id="01829-134">[Správa služby Azure Analysis Services pomocí PowerShellu](analysis-services-powershell.md) </span><span class="sxs-lookup"><span data-stu-id="01829-134">[Manage Azure Analysis Services with PowerShell](analysis-services-powershell.md) </span></span>  
<span data-ttu-id="01829-135">[Nasazení modelu ze sady SSDT](analysis-services-deploy.md) </span><span class="sxs-lookup"><span data-stu-id="01829-135">[Deploy a model from SSDT](analysis-services-deploy.md) </span></span>  
[<span data-ttu-id="01829-136">Vytvoření modelu na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="01829-136">Create a model in Azure portal</span></span>](analysis-services-create-model-portal.md)