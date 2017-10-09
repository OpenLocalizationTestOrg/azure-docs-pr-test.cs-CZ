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
# <a name="lock-resources-tooprevent-unexpected-changes"></a>Zamknout prostředky tooprevent neočekávané změny 
Jako správce může být nutné toolock předplatné, skupinu prostředků nebo prostředek tooprevent ostatní uživatelé ve vaší organizaci neúmyslnému odstranění nebo úprava důležitých prostředků. Můžete nastavit úroveň zámku hello také**CanNotDelete** nebo **jen pro čtení**. 

* **CanNotDelete** znamená Autorizovaní uživatelé stále mohou číst a upravovat prostředek, ale jejich nelze odstranit prostředek hello. 
* **Jen pro čtení** znamená Autorizovaní uživatelé mohou číst prostředek, ale jejich nelze odstranit nebo aktualizovat prostředek hello. Použití této zámek je podobné toorestricting všechna oprávnění uživatelé toohello oprávnění udělují hello **čtečky** role. 

## <a name="how-locks-are-applied"></a>Jak se používají zámky.

Když použijete zámku v nadřazeném oboru, všechny prostředky v rámci tohoto oboru dědit hello stejné zámku. I prostředky, které přidáte později dědí z nadřazené hello hello zámku. nejvíc omezující zámku Hello v dědičnosti hello přednost.

Na rozdíl od řízení přístupu na základě rolí použijte správu zámky tooapply omezení ve všech uživatelů a rolí. toolearn o nastavení oprávnění pro uživatele a rolí, najdete v části [řízení přístupu na základě Role v Azure](../active-directory/role-based-access-control-configure.md).

Správce prostředků zámky použít pouze toooperations, který dojít v rovině, správu hello, která se skládá z operace odeslané příliš`https://management.azure.com`. zámky Hello neomezují jak prostředky provádět vlastní funkce. Změny prostředku jsou s omezeným přístupem, ale operace prostředků nejsou s omezeným přístupem. Například jen pro čtení zámku v databázi SQL brání odstranění nebo úprava hello databáze, ale nezabrání vytváření, aktualizaci nebo odstranění dat v databázi hello. Data transakce jsou povolené, protože tyto operace neodešlou příliš`https://management.azure.com`.

Použití **jen pro čtení** může způsobit toounexpected výsledky, protože některé operace, které vypadají podobně jako pro čtení, operace ve skutečnosti vyžadují další akce. Například umístění **jen pro čtení** výpis klíčů hello všem uživatelům zabrání zámku na účet úložiště. Zobrazí seznam Hello operace klíče se zpracovává prostřednictvím požadavek POST, protože hello vrátit klíče jsou k dispozici pro operace zápisu. Další příklad umístění **jen pro čtení** zámku na prostředek aplikace služby zabrání zobrazení souborů pro prostředek hello vzhledem k tomu, že interakce vyžaduje oprávnění k zápisu Průzkumníka serveru Visual Studia.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Kdo může vytvářet nebo odstraňovat zámky ve vaší organizaci
zámky správy toocreate nebo odstranit, musíte mít přístup příliš`Microsoft.Authorization/*` nebo `Microsoft.Authorization/locks/*` akce. Hello předdefinovaných rolí, pouze **vlastníka** a **správce přístupu uživatelů** mají tyto akce.

## <a name="portal"></a>Portál
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a>Šablona
Hello následující příklad ukazuje šablonu, která vytvoří zámek na účet úložiště. účet úložiště Hello, na které tooapply zámku hello je zadat jako parametr. Hello jméno hello zámku se vytvoří zřetězením hello název prostředku s **/Microsoft.Authorization/** a v takovém případě hello název zámku hello **myLock**.

Zadaný typ Hello je toohello konkrétní typ prostředku. Pro úložiště nastavte typ too"Microsoft.Storage/storageaccounts/providers/locks hello".

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

## <a name="powershell"></a>PowerShell
Zámek můžete nasadit prostředků pomocí Azure PowerShell pomocí hello [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) příkaz.

toolock prostředku, zadejte název hello hello prostředku, její typ prostředku a jeho název skupiny prostředků.

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite `
  -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

toolock skupinu prostředků, zadejte hello název skupiny prostředků hello.

```powershell
New-AzureRmResourceLock -LockName LockGroup -LockLevel CanNotDelete `
  -ResourceGroupName exampleresourcegroup
```

tooget informace o uzamčení, použijte [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock). tooget všechny hello zámky v rámci vašeho předplatného, použijte:

```powershell
Get-AzureRmResourceLock
```

tooget všechny zámky pro prostředek, použijte:

```powershell
Get-AzureRmResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

tooget všechny zámky pro skupinu prostředků, použijte:

```powershell
Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup
```

Prostředí Azure PowerShell poskytuje jinými příkazy pro pracovní zámků, jako například [Set-AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) tooupdate zámek, a [odebrat AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) toodelete zámek.

## <a name="azure-cli"></a>Azure CLI

Zámek můžete nasadit prostředků pomocí rozhraní příkazového řádku Azure pomocí hello [vytvoření zámku az](/cli/azure/lock#create) příkaz.

toolock prostředku, zadejte název hello hello prostředku, její typ prostředku a jeho název skupiny prostředků.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete \
  --resource-group exampleresourcegroup --resource-name examplesite \
  --resource-type Microsoft.Web/sites
```

toolock skupinu prostředků, zadejte hello název skupiny prostředků hello.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete \
  --resource-group exampleresourcegroup
```

tooget informace o uzamčení, použijte [az zámku seznamu](/cli/azure/lock#list). tooget všechny hello zámky v rámci vašeho předplatného, použijte:

```azurecli
az lock list
```

tooget všechny zámky pro prostředek, použijte:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite \
  --namespace Microsoft.Web --resource-type sites --parent ""
```

tooget všechny zámky pro skupinu prostředků, použijte:

```azurecli
az lock list --resource-group exampleresourcegroup
```

Rozhraní příkazového řádku Azure poskytuje jinými příkazy pro pracovní zámků, jako například [az zámek aktualizace](/cli/azure/lock#update) tooupdate zámek, a [odstranit zámek az](/cli/azure/lock#delete) toodelete zámek.

## <a name="rest-api"></a>REST API
Zamknete nasazené prostředky s hello [REST API pro správu zámky](https://docs.microsoft.com/rest/api/resources/managementlocks). Hello REST API vám umožňuje toocreate a umožňuje odstranit zámky a načíst informace o existující zámky.

toocreate zámek, spusťte:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

obor Hello může mít předplatné, skupinu prostředků nebo prostředek. Hello název zámku je všechno toocall hello zámku. Verze rozhraní api, používat **2015-01-01**.

V žádosti o hello zahrnují objekt JSON, který určuje vlastnosti hello hello zámek.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a>Další kroky
* Další informace o práci s uzamčení prostředků najdete v tématu [zámku dolů vaše prostředky Azure](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
* toolearn o logicky uspořádání vaše prostředky, najdete v části [pomocí značky tooorganize vašich prostředků](resource-group-using-tags.md)
* toochange, které skupiny prostředků prostředků se nachází v, najdete v části [přesun prostředků toonew prostředků skupiny](resource-group-move-resources.md)
* Můžete použít omezení a pravidla týkající se vašeho předplatného pomocí vlastních zásad. Další informace najdete v tématu [zásady používání toomanage prostředků a řízení přístupu](resource-manager-policy.md).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).

