---
title: "změny tooprevent aaaLock prostředky Azure | Microsoft Docs"
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
ms.openlocfilehash: 1f0d8911b2b129069bd2f01a9a4ec0a3cf700118
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="lock-resources-tooprevent-unexpected-changes"></a><span data-ttu-id="6c6d0-103">Zamknout prostředky tooprevent neočekávané změny</span><span class="sxs-lookup"><span data-stu-id="6c6d0-103">Lock resources tooprevent unexpected changes</span></span> 
<span data-ttu-id="6c6d0-104">Jako správce může být nutné toolock předplatné, skupinu prostředků nebo prostředek tooprevent ostatní uživatelé ve vaší organizaci neúmyslnému odstranění nebo úprava důležitých prostředků.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-104">As an administrator, you may need toolock a subscription, resource group, or resource tooprevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="6c6d0-105">Můžete nastavit úroveň zámku hello také**CanNotDelete** nebo **jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-105">You can set hello lock level too**CanNotDelete** or **ReadOnly**.</span></span> 

* <span data-ttu-id="6c6d0-106">**CanNotDelete** znamená Autorizovaní uživatelé stále mohou číst a upravovat prostředek, ale jejich nelze odstranit prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-106">**CanNotDelete** means authorized users can still read and modify a resource, but they can't delete hello resource.</span></span> 
* <span data-ttu-id="6c6d0-107">**Jen pro čtení** znamená Autorizovaní uživatelé mohou číst prostředek, ale jejich nelze odstranit nebo aktualizovat prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-107">**ReadOnly** means authorized users can read a resource, but they can't delete or update hello resource.</span></span> <span data-ttu-id="6c6d0-108">Použití této zámek je podobné toorestricting všechna oprávnění uživatelé toohello oprávnění udělují hello **čtečky** role.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-108">Applying this lock is similar toorestricting all authorized users toohello permissions granted by hello **Reader** role.</span></span> 

## <a name="how-locks-are-applied"></a><span data-ttu-id="6c6d0-109">Jak se používají zámky.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-109">How locks are applied</span></span>

<span data-ttu-id="6c6d0-110">Když použijete zámku v nadřazeném oboru, všechny prostředky v rámci tohoto oboru dědit hello stejné zámku.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-110">When you apply a lock at a parent scope, all resources within that scope inherit hello same lock.</span></span> <span data-ttu-id="6c6d0-111">I prostředky, které přidáte později dědí z nadřazené hello hello zámku.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-111">Even resources you add later inherit hello lock from hello parent.</span></span> <span data-ttu-id="6c6d0-112">nejvíc omezující zámku Hello v dědičnosti hello přednost.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-112">hello most restrictive lock in hello inheritance takes precedence.</span></span>

<span data-ttu-id="6c6d0-113">Na rozdíl od řízení přístupu na základě rolí použijte správu zámky tooapply omezení ve všech uživatelů a rolí.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-113">Unlike role-based access control, you use management locks tooapply a restriction across all users and roles.</span></span> <span data-ttu-id="6c6d0-114">toolearn o nastavení oprávnění pro uživatele a rolí, najdete v části [řízení přístupu na základě Role v Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="6c6d0-114">toolearn about setting permissions for users and roles, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>

<span data-ttu-id="6c6d0-115">Správce prostředků zámky použít pouze toooperations, který dojít v rovině, správu hello, která se skládá z operace odeslané příliš`https://management.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-115">Resource Manager locks apply only toooperations that happen in hello management plane, which consists of operations sent too`https://management.azure.com`.</span></span> <span data-ttu-id="6c6d0-116">zámky Hello neomezují jak prostředky provádět vlastní funkce.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-116">hello locks do not restrict how resources perform their own functions.</span></span> <span data-ttu-id="6c6d0-117">Změny prostředku jsou s omezeným přístupem, ale operace prostředků nejsou s omezeným přístupem.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-117">Resource changes are restricted, but resource operations are not restricted.</span></span> <span data-ttu-id="6c6d0-118">Například jen pro čtení zámku v databázi SQL brání odstranění nebo úprava hello databáze, ale nezabrání vytváření, aktualizaci nebo odstranění dat v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-118">For example, a ReadOnly lock on a SQL Database prevents you from deleting or modifying hello database, but it does not prevent you from creating, updating, or deleting data in hello database.</span></span> <span data-ttu-id="6c6d0-119">Data transakce jsou povolené, protože tyto operace neodešlou příliš`https://management.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-119">Data transactions are permitted because those operations are not sent too`https://management.azure.com`.</span></span>

<span data-ttu-id="6c6d0-120">Použití **jen pro čtení** může způsobit toounexpected výsledky, protože některé operace, které vypadají podobně jako pro čtení, operace ve skutečnosti vyžadují další akce.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-120">Applying **ReadOnly** can lead toounexpected results because some operations that seem like read operations actually require additional actions.</span></span> <span data-ttu-id="6c6d0-121">Například umístění **jen pro čtení** výpis klíčů hello všem uživatelům zabrání zámku na účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-121">For example, placing a **ReadOnly** lock on a storage account prevents all users from listing hello keys.</span></span> <span data-ttu-id="6c6d0-122">Zobrazí seznam Hello operace klíče se zpracovává prostřednictvím požadavek POST, protože hello vrátit klíče jsou k dispozici pro operace zápisu.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-122">hello list keys operation is handled through a POST request because hello returned keys are available for write operations.</span></span> <span data-ttu-id="6c6d0-123">Další příklad umístění **jen pro čtení** zámku na prostředek aplikace služby zabrání zobrazení souborů pro prostředek hello vzhledem k tomu, že interakce vyžaduje oprávnění k zápisu Průzkumníka serveru Visual Studia.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-123">For another example, placing a **ReadOnly** lock on an App Service resource prevents Visual Studio Server Explorer from displaying files for hello resource because that interaction requires write access.</span></span>

## <a name="who-can-create-or-delete-locks-in-your-organization"></a><span data-ttu-id="6c6d0-124">Kdo může vytvářet nebo odstraňovat zámky ve vaší organizaci</span><span class="sxs-lookup"><span data-stu-id="6c6d0-124">Who can create or delete locks in your organization</span></span>
<span data-ttu-id="6c6d0-125">zámky správy toocreate nebo odstranit, musíte mít přístup příliš`Microsoft.Authorization/*` nebo `Microsoft.Authorization/locks/*` akce.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-125">toocreate or delete management locks, you must have access too`Microsoft.Authorization/*` or `Microsoft.Authorization/locks/*` actions.</span></span> <span data-ttu-id="6c6d0-126">Hello předdefinovaných rolí, pouze **vlastníka** a **správce přístupu uživatelů** mají tyto akce.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-126">Of hello built-in roles, only **Owner** and **User Access Administrator** are granted those actions.</span></span>

## <a name="portal"></a><span data-ttu-id="6c6d0-127">Portál</span><span class="sxs-lookup"><span data-stu-id="6c6d0-127">Portal</span></span>
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a><span data-ttu-id="6c6d0-128">Šablona</span><span class="sxs-lookup"><span data-stu-id="6c6d0-128">Template</span></span>
<span data-ttu-id="6c6d0-129">Hello následující příklad ukazuje šablonu, která vytvoří zámek na účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-129">hello following example shows a template that creates a lock on a storage account.</span></span> <span data-ttu-id="6c6d0-130">účet úložiště Hello, na které tooapply zámku hello je zadat jako parametr.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-130">hello storage account on which tooapply hello lock is provided as a parameter.</span></span> <span data-ttu-id="6c6d0-131">Hello jméno hello zámku se vytvoří zřetězením hello název prostředku s **/Microsoft.Authorization/** a v takovém případě hello název zámku hello **myLock**.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-131">hello name of hello lock is created by concatenating hello resource name with **/Microsoft.Authorization/** and hello name of hello lock, in this case **myLock**.</span></span>

<span data-ttu-id="6c6d0-132">Zadaný typ Hello je toohello konkrétní typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-132">hello type provided is specific toohello resource type.</span></span> <span data-ttu-id="6c6d0-133">Pro úložiště nastavte typ too"Microsoft.Storage/storageaccounts/providers/locks hello".</span><span class="sxs-lookup"><span data-stu-id="6c6d0-133">For storage, set hello type too"Microsoft.Storage/storageaccounts/providers/locks".</span></span>

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

## <a name="powershell"></a><span data-ttu-id="6c6d0-134">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6c6d0-134">PowerShell</span></span>
<span data-ttu-id="6c6d0-135">Zámek můžete nasadit prostředků pomocí Azure PowerShell pomocí hello [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) příkaz.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-135">You lock deployed resources with Azure PowerShell by using hello [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) command.</span></span>

<span data-ttu-id="6c6d0-136">toolock prostředku, zadejte název hello hello prostředku, její typ prostředku a jeho název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-136">toolock a resource, provide hello name of hello resource, its resource type, and its resource group name.</span></span>

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite `
  -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="6c6d0-137">toolock skupinu prostředků, zadejte hello název skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-137">toolock a resource group, provide hello name of hello resource group.</span></span>

```powershell
New-AzureRmResourceLock -LockName LockGroup -LockLevel CanNotDelete `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="6c6d0-138">tooget informace o uzamčení, použijte [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock).</span><span class="sxs-lookup"><span data-stu-id="6c6d0-138">tooget information about a lock, use [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock).</span></span> <span data-ttu-id="6c6d0-139">tooget všechny hello zámky v rámci vašeho předplatného, použijte:</span><span class="sxs-lookup"><span data-stu-id="6c6d0-139">tooget all hello locks in your subscription, use:</span></span>

```powershell
Get-AzureRmResourceLock
```

<span data-ttu-id="6c6d0-140">tooget všechny zámky pro prostředek, použijte:</span><span class="sxs-lookup"><span data-stu-id="6c6d0-140">tooget all locks for a resource, use:</span></span>

```powershell
Get-AzureRmResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="6c6d0-141">tooget všechny zámky pro skupinu prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="6c6d0-141">tooget all locks for a resource group, use:</span></span>

```powershell
Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="6c6d0-142">Prostředí Azure PowerShell poskytuje jinými příkazy pro pracovní zámků, jako například [Set-AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) tooupdate zámek, a [odebrat AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) toodelete zámek.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-142">Azure PowerShell provides other commands for working locks, such as [Set-AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) tooupdate a lock, and [Remove-AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) toodelete a lock.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="6c6d0-143">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6c6d0-143">Azure CLI</span></span>

<span data-ttu-id="6c6d0-144">Zámek můžete nasadit prostředků pomocí rozhraní příkazového řádku Azure pomocí hello [vytvoření zámku az](/cli/azure/lock#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-144">You lock deployed resources with Azure CLI by using hello [az lock create](/cli/azure/lock#create) command.</span></span>

<span data-ttu-id="6c6d0-145">toolock prostředku, zadejte název hello hello prostředku, její typ prostředku a jeho název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-145">toolock a resource, provide hello name of hello resource, its resource type, and its resource group name.</span></span>

```azurecli
az lock create --name LockSite --lock-type CanNotDelete \
  --resource-group exampleresourcegroup --resource-name examplesite \
  --resource-type Microsoft.Web/sites
```

<span data-ttu-id="6c6d0-146">toolock skupinu prostředků, zadejte hello název skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-146">toolock a resource group, provide hello name of hello resource group.</span></span>

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete \
  --resource-group exampleresourcegroup
```

<span data-ttu-id="6c6d0-147">tooget informace o uzamčení, použijte [az zámku seznamu](/cli/azure/lock#list).</span><span class="sxs-lookup"><span data-stu-id="6c6d0-147">tooget information about a lock, use [az lock list](/cli/azure/lock#list).</span></span> <span data-ttu-id="6c6d0-148">tooget všechny hello zámky v rámci vašeho předplatného, použijte:</span><span class="sxs-lookup"><span data-stu-id="6c6d0-148">tooget all hello locks in your subscription, use:</span></span>

```azurecli
az lock list
```

<span data-ttu-id="6c6d0-149">tooget všechny zámky pro prostředek, použijte:</span><span class="sxs-lookup"><span data-stu-id="6c6d0-149">tooget all locks for a resource, use:</span></span>

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite \
  --namespace Microsoft.Web --resource-type sites --parent ""
```

<span data-ttu-id="6c6d0-150">tooget všechny zámky pro skupinu prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="6c6d0-150">tooget all locks for a resource group, use:</span></span>

```azurecli
az lock list --resource-group exampleresourcegroup
```

<span data-ttu-id="6c6d0-151">Rozhraní příkazového řádku Azure poskytuje jinými příkazy pro pracovní zámků, jako například [az zámek aktualizace](/cli/azure/lock#update) tooupdate zámek, a [odstranit zámek az](/cli/azure/lock#delete) toodelete zámek.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-151">Azure CLI provides other commands for working locks, such as [az lock update](/cli/azure/lock#update) tooupdate a lock, and [az lock delete](/cli/azure/lock#delete) toodelete a lock.</span></span>

## <a name="rest-api"></a><span data-ttu-id="6c6d0-152">REST API</span><span class="sxs-lookup"><span data-stu-id="6c6d0-152">REST API</span></span>
<span data-ttu-id="6c6d0-153">Zamknete nasazené prostředky s hello [REST API pro správu zámky](https://docs.microsoft.com/rest/api/resources/managementlocks).</span><span class="sxs-lookup"><span data-stu-id="6c6d0-153">You can lock deployed resources with hello [REST API for management locks](https://docs.microsoft.com/rest/api/resources/managementlocks).</span></span> <span data-ttu-id="6c6d0-154">Hello REST API vám umožňuje toocreate a umožňuje odstranit zámky a načíst informace o existující zámky.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-154">hello REST API enables you toocreate and delete locks, and retrieve information about existing locks.</span></span>

<span data-ttu-id="6c6d0-155">toocreate zámek, spusťte:</span><span class="sxs-lookup"><span data-stu-id="6c6d0-155">toocreate a lock, run:</span></span>

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

<span data-ttu-id="6c6d0-156">obor Hello může mít předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-156">hello scope could be a subscription, resource group, or resource.</span></span> <span data-ttu-id="6c6d0-157">Hello název zámku je všechno toocall hello zámku.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-157">hello lock-name is whatever you want toocall hello lock.</span></span> <span data-ttu-id="6c6d0-158">Verze rozhraní api, používat **2015-01-01**.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-158">For api-version, use **2015-01-01**.</span></span>

<span data-ttu-id="6c6d0-159">V žádosti o hello zahrnují objekt JSON, který určuje vlastnosti hello hello zámek.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-159">In hello request, include a JSON object that specifies hello properties for hello lock.</span></span>

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a><span data-ttu-id="6c6d0-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6c6d0-160">Next steps</span></span>
* <span data-ttu-id="6c6d0-161">Další informace o práci s uzamčení prostředků najdete v tématu [zámku dolů vaše prostředky Azure](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)</span><span class="sxs-lookup"><span data-stu-id="6c6d0-161">For more information about working with resource locks, see [Lock Down Your Azure Resources](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)</span></span>
* <span data-ttu-id="6c6d0-162">toolearn o logicky uspořádání vaše prostředky, najdete v části [pomocí značky tooorganize vašich prostředků](resource-group-using-tags.md)</span><span class="sxs-lookup"><span data-stu-id="6c6d0-162">toolearn about logically organizing your resources, see [Using tags tooorganize your resources](resource-group-using-tags.md)</span></span>
* <span data-ttu-id="6c6d0-163">toochange, které skupiny prostředků prostředků se nachází v, najdete v části [přesun prostředků toonew prostředků skupiny](resource-group-move-resources.md)</span><span class="sxs-lookup"><span data-stu-id="6c6d0-163">toochange which resource group a resource resides in, see [Move resources toonew resource group](resource-group-move-resources.md)</span></span>
* <span data-ttu-id="6c6d0-164">Můžete použít omezení a pravidla týkající se vašeho předplatného pomocí vlastních zásad.</span><span class="sxs-lookup"><span data-stu-id="6c6d0-164">You can apply restrictions and conventions across your subscription with customized policies.</span></span> <span data-ttu-id="6c6d0-165">Další informace najdete v tématu [zásady používání toomanage prostředků a řízení přístupu](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="6c6d0-165">For more information, see [Use Policy toomanage resources and control access](resource-manager-policy.md).</span></span>
* <span data-ttu-id="6c6d0-166">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="6c6d0-166">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

