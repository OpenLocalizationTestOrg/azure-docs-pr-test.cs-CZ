---
title: "Spravovat řešení Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Ke správě prostředků pomocí Azure PowerShell a správce prostředků."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: ff42e5cb29005c5f4b97babdae58bef9382071f0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="manage-resources-with-azure-powershell-and-resource-manager"></a><span data-ttu-id="5ca64-103">Správa prostředků pomocí Azure PowerShell a správce prostředků</span><span class="sxs-lookup"><span data-stu-id="5ca64-103">Manage resources with Azure PowerShell and Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5ca64-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5ca64-104">Portal</span></span>](resource-group-portal.md)
> * [<span data-ttu-id="5ca64-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5ca64-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="5ca64-106">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="5ca64-106">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="5ca64-107">REST API</span><span class="sxs-lookup"><span data-stu-id="5ca64-107">REST API</span></span>](resource-manager-rest-api.md)
>
>

<span data-ttu-id="5ca64-108">V tomto článku zjistěte, jak spravovat vaše řešení pomocí Azure PowerShell a Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5ca64-108">In this article, you learn how to manage your solutions with Azure PowerShell and Azure Resource Manager.</span></span> <span data-ttu-id="5ca64-109">Pokud nejste obeznámeni s Resource Managerem, přečtěte si téma [Přehled služby Správce prostředků](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5ca64-109">If you are not familiar with Resource Manager, see [Resource Manager Overview](resource-group-overview.md).</span></span> <span data-ttu-id="5ca64-110">Toto téma se zaměřuje na úlohy správy.</span><span class="sxs-lookup"><span data-stu-id="5ca64-110">This topic focuses on management tasks.</span></span> <span data-ttu-id="5ca64-111">Vaším úkolem je:</span><span class="sxs-lookup"><span data-stu-id="5ca64-111">You will:</span></span>

1. <span data-ttu-id="5ca64-112">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5ca64-112">Create a resource group</span></span>
2. <span data-ttu-id="5ca64-113">Přidání prostředku do skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5ca64-113">Add a resource to the resource group</span></span>
3. <span data-ttu-id="5ca64-114">Přidání značky k prostředku</span><span class="sxs-lookup"><span data-stu-id="5ca64-114">Add a tag to the resource</span></span>
4. <span data-ttu-id="5ca64-115">Zadat dotaz na prostředky na základě názvy nebo hodnoty značky</span><span class="sxs-lookup"><span data-stu-id="5ca64-115">Query resources based on names or tag values</span></span>
5. <span data-ttu-id="5ca64-116">Použít a odebere se zámek na prostředek</span><span class="sxs-lookup"><span data-stu-id="5ca64-116">Apply and remove a lock on the resource</span></span>
6. <span data-ttu-id="5ca64-117">Odstranit skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="5ca64-117">Delete a resource group</span></span>

<span data-ttu-id="5ca64-118">Tento článek nezobrazuje postup nasazení šablony Resource Manageru do vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="5ca64-118">This article does not show how to deploy a Resource Manager template to your subscription.</span></span> <span data-ttu-id="5ca64-119">Podrobnosti naleznete v tématu [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="5ca64-119">For that information, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md).</span></span>

## <a name="get-started-with-azure-powershell"></a><span data-ttu-id="5ca64-120">Začínáme s Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="5ca64-120">Get started with Azure PowerShell</span></span>

<span data-ttu-id="5ca64-121">Pokud jste prostředí Azure PowerShell nenainstalovali, přečtěte si téma [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5ca64-121">If you have not installed Azure PowerShell, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="5ca64-122">Pokud máte nainstalovaný Azure PowerShell v minulosti, ale nebyly aktualizovány nedávno, zvažte instalaci nejnovější verze.</span><span class="sxs-lookup"><span data-stu-id="5ca64-122">If you have installed Azure PowerShell in the past but have not updated it recently, consider installing the latest version.</span></span> <span data-ttu-id="5ca64-123">Verze můžete aktualizovat pomocí stejného postupu, které jste použili k jeho instalaci.</span><span class="sxs-lookup"><span data-stu-id="5ca64-123">You can update the version through the same method you used to install it.</span></span> <span data-ttu-id="5ca64-124">Například pokud jste použili služby instalace webové platformy, spusťte znovu a vyhledejte aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="5ca64-124">For example, if you used the Web Platform Installer, launch it again and look for an update.</span></span>

<span data-ttu-id="5ca64-125">Pokud chcete zkontrolovat vaší verzi modulu prostředky Azure, použijte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="5ca64-125">To check your version of the Azure Resources module, use the following cmdlet:</span></span>

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

<span data-ttu-id="5ca64-126">Toto téma bylo aktualizováno pro verzi 3.3.0.</span><span class="sxs-lookup"><span data-stu-id="5ca64-126">This topic was updated for version 3.3.0.</span></span> <span data-ttu-id="5ca64-127">Pokud máte starší verzi, nemusí odpovídat prostředí postupy v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="5ca64-127">If you have an earlier version, your experience might not match the steps shown in this topic.</span></span> <span data-ttu-id="5ca64-128">Dokumentaci o rutinách v této verzi najdete v tématu [AzureRM.Resources modulu](/powershell/module/azurerm.resources).</span><span class="sxs-lookup"><span data-stu-id="5ca64-128">For documentation about the cmdlets in this version, see [AzureRM.Resources Module](/powershell/module/azurerm.resources).</span></span>

## <a name="log-in-to-your-azure-account"></a><span data-ttu-id="5ca64-129">Přihlaste se k účtu Azure</span><span class="sxs-lookup"><span data-stu-id="5ca64-129">Log in to your Azure account</span></span>
<span data-ttu-id="5ca64-130">Před zahájením práce na řešení, musíte být přihlášení ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="5ca64-130">Before working on your solution, you must log in to your account.</span></span>

<span data-ttu-id="5ca64-131">Přihlásit se k účtu Azure, použijte **Login-AzureRmAccount** rutiny.</span><span class="sxs-lookup"><span data-stu-id="5ca64-131">To log in to your Azure account, use the **Login-AzureRmAccount** cmdlet.</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="5ca64-132">Tato rutina vás vyzve k zadání přihlašovacích údajů k vašemu účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="5ca64-132">The cmdlet prompts you for the login credentials for your Azure account.</span></span> <span data-ttu-id="5ca64-133">Po přihlášení se stáhne nastavení účtu, aby bylo dostupné v Azure PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="5ca64-133">After logging in, it downloads your account settings so they are available to Azure PowerShell.</span></span>

<span data-ttu-id="5ca64-134">Rutina vrátí informace o váš účet a předplatné pro použití pro úlohy.</span><span class="sxs-lookup"><span data-stu-id="5ca64-134">The cmdlet returns information about your account and the subscription to use for the tasks.</span></span>

```powershell
Environment           : AzureCloud
Account               : example@contoso.com
TenantId              : {guid}
SubscriptionId        : {guid}
SubscriptionName      : Example Subscription One
CurrentStorageAccount :

```

<span data-ttu-id="5ca64-135">Pokud máte více než jedno předplatné, můžete přepnout do jiného předplatného.</span><span class="sxs-lookup"><span data-stu-id="5ca64-135">If you have more than one subscription, you can switch to a different subscription.</span></span> <span data-ttu-id="5ca64-136">První Podíváme se, Všechna předplatná pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="5ca64-136">First, let's see all the subscriptions for your account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="5ca64-137">Vrátí povolených a zakázaných odběrů.</span><span class="sxs-lookup"><span data-stu-id="5ca64-137">It returns enabled and disabled subscriptions.</span></span>

```powershell
SubscriptionName : Example Subscription One
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Two
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Three
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Disabled
```

<span data-ttu-id="5ca64-138">Chcete-li přepnout do jiného předplatného, zadejte název odběru s **Set-AzureRmContext** rutiny.</span><span class="sxs-lookup"><span data-stu-id="5ca64-138">To switch to a different subscription, provide the subscription name with the **Set-AzureRmContext** cmdlet.</span></span>

```powershell
Set-AzureRmContext -SubscriptionName "Example Subscription Two"
```

## <a name="create-a-resource-group"></a><span data-ttu-id="5ca64-139">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5ca64-139">Create a resource group</span></span>
<span data-ttu-id="5ca64-140">Před nasazením žádné prostředky k vašemu předplatnému, musíte vytvořit skupinu prostředků, která bude obsahovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="5ca64-140">Before deploying any resources to your subscription, you must create a resource group that will contain the resources.</span></span>

<span data-ttu-id="5ca64-141">Chcete-li vytvořit skupinu prostředků, použijte **New-AzureRmResourceGroup** rutiny.</span><span class="sxs-lookup"><span data-stu-id="5ca64-141">To create a resource group, use the **New-AzureRmResourceGroup** cmdlet.</span></span> <span data-ttu-id="5ca64-142">Příkaz používá **název** parametr zadejte název pro skupinu prostředků a **umístění** parametr k určení jeho umístění.</span><span class="sxs-lookup"><span data-stu-id="5ca64-142">The command uses the **Name** parameter to specify a name for the resource group and the **Location** parameter to specify its location.</span></span>

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location "South Central US"
```

<span data-ttu-id="5ca64-143">Výstup je v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="5ca64-143">The output is in the following format:</span></span>

```powershell
ResourceGroupName : TestRG1
Location          : southcentralus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1
```

<span data-ttu-id="5ca64-144">Pokud budete potřebovat později načíst skupinu prostředků, použijte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="5ca64-144">If you need to retrieve the resource group later, use the following cmdlet:</span></span>

```powershell
Get-AzureRmResourceGroup -ResourceGroupName TestRG1
```

<span data-ttu-id="5ca64-145">Chcete-li získat všechny skupiny prostředků v rámci vašeho předplatného, nezadávejte název:</span><span class="sxs-lookup"><span data-stu-id="5ca64-145">To get all the resource groups in your subscription, do not specify a name:</span></span>

```powershell
Get-AzureRmResourceGroup
```

## <a name="add-resources-to-a-resource-group"></a><span data-ttu-id="5ca64-146">Přidat prostředky do skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5ca64-146">Add resources to a resource group</span></span>
<span data-ttu-id="5ca64-147">Chcete-li přidat prostředek do skupiny prostředků, můžete použít **New-AzureRmResource** rutina nebo rutiny, které jsou specifické pro typ prostředku, kterou vytváříte (jako **AzureRmStorageAccount nový**).</span><span class="sxs-lookup"><span data-stu-id="5ca64-147">To add a resource to the resource group, you can use the **New-AzureRmResource** cmdlet or a cmdlet that is specific to the type of resource you are creating (like **New-AzureRmStorageAccount**).</span></span> <span data-ttu-id="5ca64-148">Vám může být snadněji použít rutinu, která je specifická pro typ prostředku, protože obsahuje parametry pro vlastnosti, které jsou potřebné pro nový prostředek.</span><span class="sxs-lookup"><span data-stu-id="5ca64-148">You might find it easier to use a cmdlet that is specific to a resource type because it includes parameters for the properties that are needed for the new resource.</span></span> <span data-ttu-id="5ca64-149">Použít **New-AzureRmResource**, musíte znát všechny vlastnosti, které chcete nastavit bez výzvy pro ně.</span><span class="sxs-lookup"><span data-stu-id="5ca64-149">To use **New-AzureRmResource**, you must know all the properties to set without being prompted for them.</span></span>

<span data-ttu-id="5ca64-150">Přidání prostředku pomocí rutin však může být matoucí budoucí protože nový prostředek neexistuje v šabloně Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5ca64-150">However, adding a resource through cmdlets might cause future confusion because the new resource does not exist in a Resource Manager template.</span></span> <span data-ttu-id="5ca64-151">Společnost Microsoft doporučuje definování infrastrukturu pro vaše řešení Azure v šabloně Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5ca64-151">Microsoft recommends defining the infrastructure for your Azure solution in a Resource Manager template.</span></span> <span data-ttu-id="5ca64-152">Šablony umožňují spolehlivě a opakovaného nasazování svého řešení.</span><span class="sxs-lookup"><span data-stu-id="5ca64-152">Templates enable you to reliably and repeatedly deploy your solution.</span></span> <span data-ttu-id="5ca64-153">Pro toto téma vytvoříte účet úložiště s rutiny prostředí PowerShell, ale později generování šablonu z vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5ca64-153">For this topic, you create a storage account with a PowerShell cmdlet, but later you generate a template from your resource group.</span></span>

<span data-ttu-id="5ca64-154">Následující rutina vytvoří účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="5ca64-154">The following cmdlet creates a storage account.</span></span> <span data-ttu-id="5ca64-155">Místo použití názvu v příkladu, zadejte jedinečný název pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="5ca64-155">Instead of using the name shown in the example, provide a unique name for the storage account.</span></span> <span data-ttu-id="5ca64-156">Název musí být v rozmezí 3 až 24 znaků a použít pouze čísla a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="5ca64-156">The name must be between 3 and 24 characters in length, and use only numbers and lower-case letters.</span></span> <span data-ttu-id="5ca64-157">Pokud použijete název v příkladu, zobrazí chybu, protože tento název je již používán.</span><span class="sxs-lookup"><span data-stu-id="5ca64-157">If you use the name shown in the example, you receive an error because that name is already in use.</span></span>

```powershell
New-AzureRmStorageAccount -ResourceGroupName TestRG1 -AccountName mystoragename -Type "Standard_LRS" -Location "South Central US"
```

<span data-ttu-id="5ca64-158">Pokud budete potřebovat později načíst tento prostředek, použijte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="5ca64-158">If you need to retrieve this resource later, use the following cmdlet:</span></span>

```powershell
Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1
```

## <a name="add-a-tag"></a><span data-ttu-id="5ca64-159">Přidání značky</span><span class="sxs-lookup"><span data-stu-id="5ca64-159">Add a tag</span></span>

<span data-ttu-id="5ca64-160">Značky umožňují uspořádání prostředků podle jiné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5ca64-160">Tags enable you to organize your resources according to different properties.</span></span> <span data-ttu-id="5ca64-161">Například může mít několik prostředků v různých prostředků skupiny, které patří do stejného oddělení.</span><span class="sxs-lookup"><span data-stu-id="5ca64-161">For example, you may have several resources in different resource groups that belong to the same department.</span></span> <span data-ttu-id="5ca64-162">Můžete použít oddělení značky a hodnoty pro tyto prostředky označit je jako náležící do stejné kategorii.</span><span class="sxs-lookup"><span data-stu-id="5ca64-162">You can apply a department tag and value to those resources to mark them as belonging to the same category.</span></span> <span data-ttu-id="5ca64-163">Nebo můžete označit, zda je prostředek použít v produkčním i testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5ca64-163">Or, you can mark whether a resource is used in a production or test environment.</span></span> <span data-ttu-id="5ca64-164">V tomto tématu Použití značek k jen jeden prostředek, ale ve vašem prostředí nejpravděpodobnější má smysl použití značek ke všem prostředkům.</span><span class="sxs-lookup"><span data-stu-id="5ca64-164">In this topic, you apply tags to only one resource, but in your environment it most likely makes sense to apply tags to all your resources.</span></span>

<span data-ttu-id="5ca64-165">Následující rutiny platí dvě značky pro váš účet úložiště:</span><span class="sxs-lookup"><span data-stu-id="5ca64-165">The following cmdlet applies two tags to your storage account:</span></span>

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
 ```

<span data-ttu-id="5ca64-166">Značky jsou aktualizovány jako jednoho objektu.</span><span class="sxs-lookup"><span data-stu-id="5ca64-166">Tags are updated as a single object.</span></span> <span data-ttu-id="5ca64-167">Postup přidání značky k prostředku, který již obsahuje značky, nejdřív načtěte stávající značky.</span><span class="sxs-lookup"><span data-stu-id="5ca64-167">To add a tag to a resource that already includes tags, first retrieve the existing tags.</span></span> <span data-ttu-id="5ca64-168">Přidejte novou značku k objektu, který obsahuje stávající značky a znovu použít všechny značky k prostředku.</span><span class="sxs-lookup"><span data-stu-id="5ca64-168">Add the new tag to the object that contains the existing tags, and reapply all the tags to the resource.</span></span>

```powershell
$tags = (Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
```

## <a name="search-for-resources"></a><span data-ttu-id="5ca64-169">Hledat prostředky</span><span class="sxs-lookup"><span data-stu-id="5ca64-169">Search for resources</span></span>

<span data-ttu-id="5ca64-170">Použití **najít AzureRmResource** rutiny k načtení prostředků pro různé vyhledávací podmínky.</span><span class="sxs-lookup"><span data-stu-id="5ca64-170">Use the **Find-AzureRmResource** cmdlet to retrieve resources for different search conditions.</span></span>

* <span data-ttu-id="5ca64-171">Chcete-li získat prostředek podle názvu, zadejte **ResourceNameContains** parametr:</span><span class="sxs-lookup"><span data-stu-id="5ca64-171">To get a resource by name, provide the **ResourceNameContains** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceNameContains mystoragename
  ```

* <span data-ttu-id="5ca64-172">Chcete-li získat všechny prostředky ve skupině prostředků, zadejte **ResourceGroupNameContains** parametr:</span><span class="sxs-lookup"><span data-stu-id="5ca64-172">To get all the resources in a resource group, provide the **ResourceGroupNameContains** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceGroupNameContains TestRG1
  ```

* <span data-ttu-id="5ca64-173">Chcete-li získat všechny prostředky se název značky a hodnotou, zadat **TagName** a **TagValue** parametry:</span><span class="sxs-lookup"><span data-stu-id="5ca64-173">To get all the resources with a tag name and value, provide the **TagName** and **TagValue** parameters:</span></span>

  ```powershell
  Find-AzureRmResource -TagName Dept -TagValue IT
  ```

* <span data-ttu-id="5ca64-174">Na všechny prostředky s konkrétní typ prostředku, zadejte **ResourceType** parametr:</span><span class="sxs-lookup"><span data-stu-id="5ca64-174">To all the resources with a particular resource type, provide the **ResourceType** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceType Microsoft.Storage/storageAccounts
  ```

## <a name="lock-a-resource"></a><span data-ttu-id="5ca64-175">Zamknout prostředku</span><span class="sxs-lookup"><span data-stu-id="5ca64-175">Lock a resource</span></span>

<span data-ttu-id="5ca64-176">Pokud potřebujete zajistit kritické prostředků je náhodně odstraněna nebo upravena, použije zámek k prostředku.</span><span class="sxs-lookup"><span data-stu-id="5ca64-176">When you need to make sure a critical resource is not accidentally deleted or modified, apply a lock to the resource.</span></span> <span data-ttu-id="5ca64-177">Můžete buď zadat **CanNotDelete** nebo **jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="5ca64-177">You can specify either a **CanNotDelete** or **ReadOnly**.</span></span>

<span data-ttu-id="5ca64-178">Vytvořit nebo odstranit zámky správy, musíte mít přístup k `Microsoft.Authorization/*` nebo `Microsoft.Authorization/locks/*` akce.</span><span class="sxs-lookup"><span data-stu-id="5ca64-178">To create or delete management locks, you must have access to `Microsoft.Authorization/*` or `Microsoft.Authorization/locks/*` actions.</span></span> <span data-ttu-id="5ca64-179">Z předdefinovaných rolí pouze vlastník a správce přístupu uživatelů mají tyto akce.</span><span class="sxs-lookup"><span data-stu-id="5ca64-179">Of the built-in roles, only Owner and User Access Administrator are granted those actions.</span></span>

<span data-ttu-id="5ca64-180">Použít zámek, použijte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="5ca64-180">To apply a lock, use the following cmdlet:</span></span>

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

<span data-ttu-id="5ca64-181">Uzamčení prostředků v předchozím příkladu nelze odstranit, dokud se neodstraní zámek.</span><span class="sxs-lookup"><span data-stu-id="5ca64-181">The locked resource in the preceding example cannot be deleted until the lock is removed.</span></span> <span data-ttu-id="5ca64-182">Chcete-li odebrat zámek, použijte:</span><span class="sxs-lookup"><span data-stu-id="5ca64-182">To remove a lock, use:</span></span>

```powershell
Remove-AzureRmResourceLock -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

<span data-ttu-id="5ca64-183">Další informace o nastavení zámky najdete v tématu [zamknutí prostředků pomocí Azure Resource Manageru](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="5ca64-183">For more information about setting locks, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

## <a name="remove-resources-or-resource-group"></a><span data-ttu-id="5ca64-184">Odebrat prostředky nebo skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="5ca64-184">Remove resources or resource group</span></span>
<span data-ttu-id="5ca64-185">Můžete odebrat prostředek nebo skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="5ca64-185">You can remove a resource or resource group.</span></span> <span data-ttu-id="5ca64-186">Když odeberete skupinu prostředků, je také odstranit všechny prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="5ca64-186">When you remove a resource group, you also remove all the resources within that resource group.</span></span>

* <span data-ttu-id="5ca64-187">Pokud chcete odstranit prostředek ze skupiny prostředků, použijte **odebrat AzureRmResource** rutiny.</span><span class="sxs-lookup"><span data-stu-id="5ca64-187">To delete a resource from the resource group, use the **Remove-AzureRmResource** cmdlet.</span></span> <span data-ttu-id="5ca64-188">Tato rutina Odstraní prostředek, ale nedojde k odstranění skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5ca64-188">This cmdlet deletes the resource, but does not delete the resource group.</span></span>

  ```powershell
  Remove-AzureRmResource -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
  ```

* <span data-ttu-id="5ca64-189">Chcete-li odstranit skupinu prostředků a všechny její prostředky, použijte **Remove-AzureRmResourceGroup** rutiny.</span><span class="sxs-lookup"><span data-stu-id="5ca64-189">To delete a resource group and all its resources, use the **Remove-AzureRmResourceGroup** cmdlet.</span></span>

  ```powershell
  Remove-AzureRmResourceGroup -Name TestRG1
  ```

<span data-ttu-id="5ca64-190">Obě rutin, zobrazí se výzva k potvrzení, že chcete odebrat prostředek nebo skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="5ca64-190">For both cmdlets, you are asked to confirm that you wish to remove the resource or resource group.</span></span> <span data-ttu-id="5ca64-191">Pokud se operace úspěšně Odstraní prostředek nebo skupina prostředků, vrátí **True**.</span><span class="sxs-lookup"><span data-stu-id="5ca64-191">If the operation successfully deletes the resource or resource group, it returns **True**.</span></span>

## <a name="run-resource-manager-scripts-with-azure-automation"></a><span data-ttu-id="5ca64-192">Spusťte skripty správce prostředků se Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5ca64-192">Run Resource Manager scripts with Azure Automation</span></span>

<span data-ttu-id="5ca64-193">Toto téma ukazuje, jak provádět základní operace na vašich prostředků Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5ca64-193">This topic shows you how to perform basic operations on your resources with Azure PowerShell.</span></span> <span data-ttu-id="5ca64-194">Pro pokročilejší scénáře správy obvykle chcete vytvořit skript a znovu použít tento skript podle potřeby nebo podle plánu.</span><span class="sxs-lookup"><span data-stu-id="5ca64-194">For more advanced management scenarios, you typically want to create a script, and reuse that script as needed or on a schedule.</span></span> <span data-ttu-id="5ca64-195">[Služby Azure Automation](../automation/automation-intro.md) poskytuje způsob, jak často automatizaci použít skripty, které spravují řešení Azure.</span><span class="sxs-lookup"><span data-stu-id="5ca64-195">[Azure Automation](../automation/automation-intro.md) provides a way for you to automate frequently used scripts that manage your Azure solutions.</span></span>

<span data-ttu-id="5ca64-196">Následující témata ukazují, jak používat Azure Automation, Resource Manageru a prostředí PowerShell k efektivní provádění úloh správy:</span><span class="sxs-lookup"><span data-stu-id="5ca64-196">The following topics show you how to use Azure Automation, Resource Manager, and PowerShell to effectively perform management tasks:</span></span>

- <span data-ttu-id="5ca64-197">Informace o vytvoření sady runbook najdete v tématu [Můj první Powershellový runbook](../automation/automation-first-runbook-textual-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="5ca64-197">For information about creating a runbook, see [My first PowerShell runbook](../automation/automation-first-runbook-textual-powershell.md).</span></span>
- <span data-ttu-id="5ca64-198">Informace o práci s galerií skriptů, najdete v tématu [Galerie Runbooků a modulů pro Azure Automation](../automation/automation-runbook-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="5ca64-198">For information about working with galleries of scripts, see [Runbook and module galleries for Azure Automation](../automation/automation-runbook-gallery.md).</span></span>
- <span data-ttu-id="5ca64-199">Sady runbook, které spuštění a zastavení virtuálních počítačů, najdete v části [scénáře Azure Automation: formátu JSON pomocí značky se mají vytvořit plán pro virtuální počítač Azure spuštění a vypnutí](../automation/automation-scenario-start-stop-vm-wjson-tags.md).</span><span class="sxs-lookup"><span data-stu-id="5ca64-199">For runbooks that start and stop virtual machines, see [Azure Automation scenario: Using JSON-formatted tags to create a schedule for Azure VM startup and shutdown](../automation/automation-scenario-start-stop-vm-wjson-tags.md).</span></span>
- <span data-ttu-id="5ca64-200">Sady runbook, které spuštění a zastavení počítačem nepracujete virtuální počítače, naleznete v části [spuštění a zastavení virtuálních počítačů během řešení počítačem nepracujete v automatizaci](../automation/automation-solution-vm-management.md).</span><span class="sxs-lookup"><span data-stu-id="5ca64-200">For runbooks that start and stop virtual machines off-hours, see [Start/Stop VMs during off-hours solution in Automation](../automation/automation-solution-vm-management.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5ca64-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5ca64-201">Next steps</span></span>
* <span data-ttu-id="5ca64-202">Další informace o vytváření šablon Resource Manageru, najdete v části [vytváření šablon Azure Resource Manager](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="5ca64-202">To learn about creating Resource Manager templates, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="5ca64-203">Další informace o nasazení šablony najdete v tématu [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="5ca64-203">To learn about deploying templates, see [Deploy an application with Azure Resource Manager Template](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="5ca64-204">Existující prostředky můžete přesunout do nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5ca64-204">You can move existing resources to a new resource group.</span></span> <span data-ttu-id="5ca64-205">Příklady najdete v tématu [přesunutí prostředků do nové skupiny prostředků nebo předplatného](resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="5ca64-205">For examples, see [Move Resources to New Resource Group or Subscription](resource-group-move-resources.md).</span></span>
* <span data-ttu-id="5ca64-206">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="5ca64-206">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

