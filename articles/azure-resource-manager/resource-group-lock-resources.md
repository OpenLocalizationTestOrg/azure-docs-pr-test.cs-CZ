---
title: "Zamknout prostředky Azure, aby se změny | Microsoft Docs"
description: "Zabráníte uživatelům v aktualizaci nebo odstranění důležité prostředky Azure s použitím zámek pro všechny uživatele a role."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 53c57e8f-741c-4026-80e0-f4c02638c98b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: tomfitz
ms.openlocfilehash: 44c87b00f4fc63dbfd45a07d9a8cddc5eaf1a65c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="lock-resources-to-prevent-unexpected-changes"></a><span data-ttu-id="c1ee4-103">Zamknutí prostředků, aby se zabránilo neočekávané změny</span><span class="sxs-lookup"><span data-stu-id="c1ee4-103">Lock resources to prevent unexpected changes</span></span> 
<span data-ttu-id="c1ee4-104">Jako správce musíte k uzamčení předplatné, skupinu prostředků nebo prostředek zabránit ostatním uživatelům ve vaší organizaci neúmyslnému odstranění nebo úprava důležitých prostředků.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-104">As an administrator, you may need to lock a subscription, resource group, or resource to prevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="c1ee4-105">Můžete nastavit zámek na úrovni **CanNotDelete** nebo **jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-105">You can set the lock level to **CanNotDelete** or **ReadOnly**.</span></span> 

* <span data-ttu-id="c1ee4-106">**CanNotDelete** znamená Autorizovaní uživatelé stále mohou číst a upravovat prostředek, ale jejich nelze odstranit prostředek.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-106">**CanNotDelete** means authorized users can still read and modify a resource, but they can't delete the resource.</span></span> 
* <span data-ttu-id="c1ee4-107">**Jen pro čtení** znamená Autorizovaní uživatelé mohou číst prostředek, ale jejich nelze odstranit nebo aktualizovat prostředek.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-107">**ReadOnly** means authorized users can read a resource, but they can't delete or update the resource.</span></span> <span data-ttu-id="c1ee4-108">Je podobná omezení všem oprávněným uživatelům oprávnění udělují použití této zámku **čtečky** role.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-108">Applying this lock is similar to restricting all authorized users to the permissions granted by the **Reader** role.</span></span> 

## <a name="how-locks-are-applied"></a><span data-ttu-id="c1ee4-109">Jak se používají zámky.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-109">How locks are applied</span></span>

<span data-ttu-id="c1ee4-110">Když použijete zámku v nadřazeném oboru, zdědí všechny prostředky v rámci tohoto oboru stejné zámek.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-110">When you apply a lock at a parent scope, all resources within that scope inherit the same lock.</span></span> <span data-ttu-id="c1ee4-111">I prostředky, které přidáte později zámek dědí z nadřazeného objektu.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-111">Even resources you add later inherit the lock from the parent.</span></span> <span data-ttu-id="c1ee4-112">Nejvíc omezující zámek v dědičnosti přednost.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-112">The most restrictive lock in the inheritance takes precedence.</span></span>

<span data-ttu-id="c1ee4-113">Na rozdíl od řízení přístupu na základě rolí použít zámky správy pro aplikaci omezení ve všech uživatelů a rolí.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-113">Unlike role-based access control, you use management locks to apply a restriction across all users and roles.</span></span> <span data-ttu-id="c1ee4-114">Další informace o nastavení oprávnění pro uživatele a rolí najdete v tématu [řízení přístupu na základě Role v Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="c1ee4-114">To learn about setting permissions for users and roles, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>

<span data-ttu-id="c1ee4-115">Zámky správce prostředků se vztahují pouze na operace, které dojít v rovině řízení, sestávající operací posílá `https://management.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-115">Resource Manager locks apply only to operations that happen in the management plane, which consists of operations sent to `https://management.azure.com`.</span></span> <span data-ttu-id="c1ee4-116">Zámky neomezují jak prostředky provádět vlastní funkce.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-116">The locks do not restrict how resources perform their own functions.</span></span> <span data-ttu-id="c1ee4-117">Změny prostředku jsou s omezeným přístupem, ale operace prostředků nejsou s omezeným přístupem.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-117">Resource changes are restricted, but resource operations are not restricted.</span></span> <span data-ttu-id="c1ee4-118">Například jen pro čtení zámku v databázi SQL zabráníte odstranění nebo úprava databázi, ale nezabrání můžete z vytvoření, aktualizace a odstraňování dat v databázi.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-118">For example, a ReadOnly lock on a SQL Database prevents you from deleting or modifying the database, but it does not prevent you from creating, updating, or deleting data in the database.</span></span> <span data-ttu-id="c1ee4-119">Data transakce jsou povolené, protože tyto operace se neodesílají na `https://management.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-119">Data transactions are permitted because those operations are not sent to `https://management.azure.com`.</span></span>

<span data-ttu-id="c1ee4-120">Použití **jen pro čtení** může vést k neočekávaným výsledkům, protože některé operace, které vypadají podobně jako pro čtení, operace ve skutečnosti vyžadují další akce.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-120">Applying **ReadOnly** can lead to unexpected results because some operations that seem like read operations actually require additional actions.</span></span> <span data-ttu-id="c1ee4-121">Například umístění **jen pro čtení** výpis klíčů všem uživatelům zabrání zámku na účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-121">For example, placing a **ReadOnly** lock on a storage account prevents all users from listing the keys.</span></span> <span data-ttu-id="c1ee4-122">Seznam klíčů operaci je zpracováván prostřednictvím požadavek POST, protože vrácený klíče jsou k dispozici pro operace zápisu.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-122">The list keys operation is handled through a POST request because the returned keys are available for write operations.</span></span> <span data-ttu-id="c1ee4-123">Další příklad umístění **jen pro čtení** zámku na prostředek aplikace služby zabrání zobrazení souborů pro daný prostředek, protože interakce vyžaduje oprávnění k zápisu Průzkumníka serveru Visual Studia.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-123">For another example, placing a **ReadOnly** lock on an App Service resource prevents Visual Studio Server Explorer from displaying files for the resource because that interaction requires write access.</span></span>

## <a name="who-can-create-or-delete-locks-in-your-organization"></a><span data-ttu-id="c1ee4-124">Kdo může vytvářet nebo odstraňovat zámky ve vaší organizaci</span><span class="sxs-lookup"><span data-stu-id="c1ee4-124">Who can create or delete locks in your organization</span></span>
<span data-ttu-id="c1ee4-125">Vytvořit nebo odstranit zámky správy, musíte mít přístup k `Microsoft.Authorization/*` nebo `Microsoft.Authorization/locks/*` akce.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-125">To create or delete management locks, you must have access to `Microsoft.Authorization/*` or `Microsoft.Authorization/locks/*` actions.</span></span> <span data-ttu-id="c1ee4-126">Z předdefinovaných rolí pouze **vlastníka** a **správce přístupu uživatelů** mají tyto akce.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-126">Of the built-in roles, only **Owner** and **User Access Administrator** are granted those actions.</span></span>

## <a name="portal"></a><span data-ttu-id="c1ee4-127">Portál</span><span class="sxs-lookup"><span data-stu-id="c1ee4-127">Portal</span></span>
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a><span data-ttu-id="c1ee4-128">Šablona</span><span class="sxs-lookup"><span data-stu-id="c1ee4-128">Template</span></span>
<span data-ttu-id="c1ee4-129">Následující příklad ukazuje šablonu, která vytvoří zámek na účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-129">The following example shows a template that creates a lock on a storage account.</span></span> <span data-ttu-id="c1ee4-130">Účet úložiště, na kterých chcete použít zámek je zadat jako parametr.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-130">The storage account on which to apply the lock is provided as a parameter.</span></span> <span data-ttu-id="c1ee4-131">Název zámku se vytvoří zřetězením názvu prostředku s **/Microsoft.Authorization/** a název zámku, v tomto případě **myLock**.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-131">The name of the lock is created by concatenating the resource name with **/Microsoft.Authorization/** and the name of the lock, in this case **myLock**.</span></span>

<span data-ttu-id="c1ee4-132">Zadaný typ je specifické pro daný typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-132">The type provided is specific to the resource type.</span></span> <span data-ttu-id="c1ee4-133">Pro úložiště nastavte typ, který má "Microsoft.Storage/storageaccounts/providers/locks".</span><span class="sxs-lookup"><span data-stu-id="c1ee4-133">For storage, set the type to "Microsoft.Storage/storageaccounts/providers/locks".</span></span>

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="powershell"></a><span data-ttu-id="c1ee4-134">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c1ee4-134">PowerShell</span></span>
<span data-ttu-id="c1ee4-135">Zámek můžete nasadit pomocí prostředků pomocí Azure PowerShell [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) příkaz.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-135">You lock deployed resources with Azure PowerShell by using the [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) command.</span></span>

<span data-ttu-id="c1ee4-136">K uzamčení prostředku, zadejte název prostředku, její typ prostředku a jeho název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-136">To lock a resource, provide the name of the resource, its resource type, and its resource group name.</span></span>

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite `
  -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="c1ee4-137">Zamknout skupinu prostředků, zadejte název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-137">To lock a resource group, provide the name of the resource group.</span></span>

```powershell
New-AzureRmResourceLock -LockName LockGroup -LockLevel CanNotDelete `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="c1ee4-138">Chcete-li získat informace o zámek, použijte [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock).</span><span class="sxs-lookup"><span data-stu-id="c1ee4-138">To get information about a lock, use [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock).</span></span> <span data-ttu-id="c1ee4-139">Všechny zámky v rámci vašeho předplatného, použijte:</span><span class="sxs-lookup"><span data-stu-id="c1ee4-139">To get all the locks in your subscription, use:</span></span>

```powershell
Get-AzureRmResourceLock
```

<span data-ttu-id="c1ee4-140">Všechny zámky pro prostředek, použijte:</span><span class="sxs-lookup"><span data-stu-id="c1ee4-140">To get all locks for a resource, use:</span></span>

```powershell
Get-AzureRmResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="c1ee4-141">Všechny zámky pro skupinu prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="c1ee4-141">To get all locks for a resource group, use:</span></span>

```powershell
Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="c1ee4-142">Prostředí Azure PowerShell poskytuje jinými příkazy pro pracovní zámků, jako například [Set-AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) aktualizovat o zámku a [odebrat AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) odstranit zámek.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-142">Azure PowerShell provides other commands for working locks, such as [Set-AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) to update a lock, and [Remove-AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) to delete a lock.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="c1ee4-143">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c1ee4-143">Azure CLI</span></span>

<span data-ttu-id="c1ee4-144">Zámek můžete nasadit pomocí prostředků pomocí Azure CLI [az zámku vytvořit](/cli/azure/lock#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-144">You lock deployed resources with Azure CLI by using the [az lock create](/cli/azure/lock#create) command.</span></span>

<span data-ttu-id="c1ee4-145">K uzamčení prostředku, zadejte název prostředku, její typ prostředku a jeho název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-145">To lock a resource, provide the name of the resource, its resource type, and its resource group name.</span></span>

```azurecli
az lock create --name LockSite --lock-type CanNotDelete \
  --resource-group exampleresourcegroup --resource-name examplesite \
  --resource-type Microsoft.Web/sites
```

<span data-ttu-id="c1ee4-146">Zamknout skupinu prostředků, zadejte název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-146">To lock a resource group, provide the name of the resource group.</span></span>

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete \
  --resource-group exampleresourcegroup
```

<span data-ttu-id="c1ee4-147">Chcete-li získat informace o zámek, použijte [az zámku seznamu](/cli/azure/lock#list).</span><span class="sxs-lookup"><span data-stu-id="c1ee4-147">To get information about a lock, use [az lock list](/cli/azure/lock#list).</span></span> <span data-ttu-id="c1ee4-148">Všechny zámky v rámci vašeho předplatného, použijte:</span><span class="sxs-lookup"><span data-stu-id="c1ee4-148">To get all the locks in your subscription, use:</span></span>

```azurecli
az lock list
```

<span data-ttu-id="c1ee4-149">Všechny zámky pro prostředek, použijte:</span><span class="sxs-lookup"><span data-stu-id="c1ee4-149">To get all locks for a resource, use:</span></span>

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite \
  --namespace Microsoft.Web --resource-type sites --parent ""
```

<span data-ttu-id="c1ee4-150">Všechny zámky pro skupinu prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="c1ee4-150">To get all locks for a resource group, use:</span></span>

```azurecli
az lock list --resource-group exampleresourcegroup
```

<span data-ttu-id="c1ee4-151">Rozhraní příkazového řádku Azure poskytuje jinými příkazy pro pracovní zámků, jako například [az zámek aktualizace](/cli/azure/lock#update) aktualizovat o zámku a [odstranit zámek az](/cli/azure/lock#delete) odstranit zámek.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-151">Azure CLI provides other commands for working locks, such as [az lock update](/cli/azure/lock#update) to update a lock, and [az lock delete](/cli/azure/lock#delete) to delete a lock.</span></span>

## <a name="rest-api"></a><span data-ttu-id="c1ee4-152">REST API</span><span class="sxs-lookup"><span data-stu-id="c1ee4-152">REST API</span></span>
<span data-ttu-id="c1ee4-153">Zamknete nasazené prostředky s [REST API pro správu zámky](https://docs.microsoft.com/rest/api/resources/managementlocks).</span><span class="sxs-lookup"><span data-stu-id="c1ee4-153">You can lock deployed resources with the [REST API for management locks](https://docs.microsoft.com/rest/api/resources/managementlocks).</span></span> <span data-ttu-id="c1ee4-154">Rozhraní REST API umožňuje vytvářet a umožňuje odstranit zámky a načíst informace o existující zámky.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-154">The REST API enables you to create and delete locks, and retrieve information about existing locks.</span></span>

<span data-ttu-id="c1ee4-155">Chcete-li vytvořit zámek, spusťte:</span><span class="sxs-lookup"><span data-stu-id="c1ee4-155">To create a lock, run:</span></span>

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

<span data-ttu-id="c1ee4-156">Obor může být předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-156">The scope could be a subscription, resource group, or resource.</span></span> <span data-ttu-id="c1ee4-157">Název zámku je všechno volání zámek.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-157">The lock-name is whatever you want to call the lock.</span></span> <span data-ttu-id="c1ee4-158">Verze rozhraní api, používat **2015-01-01**.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-158">For api-version, use **2015-01-01**.</span></span>

<span data-ttu-id="c1ee4-159">V žádosti o zahrnují objekt JSON, který určuje vlastnosti pro zámek.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-159">In the request, include a JSON object that specifies the properties for the lock.</span></span>

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a><span data-ttu-id="c1ee4-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c1ee4-160">Next steps</span></span>
* <span data-ttu-id="c1ee4-161">Další informace o práci s uzamčení prostředků najdete v tématu [zámku dolů vaše prostředky Azure](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)</span><span class="sxs-lookup"><span data-stu-id="c1ee4-161">For more information about working with resource locks, see [Lock Down Your Azure Resources](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)</span></span>
* <span data-ttu-id="c1ee4-162">Další informace o logicky organizování vašich prostředků najdete v tématu [pomocí značek k uspořádání prostředků](resource-group-using-tags.md)</span><span class="sxs-lookup"><span data-stu-id="c1ee4-162">To learn about logically organizing your resources, see [Using tags to organize your resources](resource-group-using-tags.md)</span></span>
* <span data-ttu-id="c1ee4-163">Ke změně prostředku se nachází v prostředku skupiny, najdete v části [přesunutím prostředků do nové skupiny prostředků](resource-group-move-resources.md)</span><span class="sxs-lookup"><span data-stu-id="c1ee4-163">To change which resource group a resource resides in, see [Move resources to new resource group](resource-group-move-resources.md)</span></span>
* <span data-ttu-id="c1ee4-164">Můžete použít omezení a pravidla týkající se vašeho předplatného pomocí vlastních zásad.</span><span class="sxs-lookup"><span data-stu-id="c1ee4-164">You can apply restrictions and conventions across your subscription with customized policies.</span></span> <span data-ttu-id="c1ee4-165">Další informace najdete v tématu [Použití zásad ke správě prostředků a řízení přístupu](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="c1ee4-165">For more information, see [Use Policy to manage resources and control access](resource-manager-policy.md).</span></span>
* <span data-ttu-id="c1ee4-166">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="c1ee4-166">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

