---
title: "Provedení operace úložiště Azure Table pomocí prostředí PowerShell | Microsoft Docs"
description: "Provedení operace úložiště Azure Table pomocí prostředí PowerShell"
services: cosmos-db
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: robinsh
ms.openlocfilehash: 15a4ed2370598cb98565c48b4563bee3a4445827
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="perform-azure-table-storage-operations-with-azure-powershell"></a>Provedení operace úložiště Azure Table pomocí prostředí Azure PowerShell 

>[!NOTE]
>Rozhraní API tabulky DB Cosmos Azure poskytuje prémiových funkcí pro úložiště table, jako jsou to globální distribuční, nízká latence čtení a zápisy, automatické sekundární indexování a vyhrazenou propustností. Ve většině případů je specifická pro Azure Table storage příkazy prostředí PowerShell v pracovním tohoto článku rozhraní API služby Azure Cosmos DB tabulky a Azure Table storage, ale v tomto článku. Pokud používáte rozhraní API služby Azure Cosmos DB tabulky, přečtěte si téma [operace rozhraní API služby Azure Cosmos DB tabulky s prostředím Azure PowerShell](table-powershell.md).
>

Azure Table storage je úložištěm dat typu NoSQL, který můžete použít k ukládání a dotazování obrovských sad strukturovaných, nerelačních data. Hlavní komponenty služby jsou tabulky, entit a vlastnosti. Tabulka je kolekce entit. Entita je sada vlastností. Každá entita může mít až 252 vlastností, které jsou všechny páry název hodnota. Tento článek předpokládá, že jste již obeznámeni s koncepty služby úložiště Azure Table. Podrobné informace najdete v tématu [Principy datového modelu služby Table](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model) a [Začínáme s Azure Table storage pomocí rozhraní .NET](table-storage-how-to-use-dotnet.md).

Tento článek popisuje běžné operace úložiště Azure Table. Získáte informace o těchto tématech: 

> [!div class="checklist"]
> * Vytvoření tabulky
> * Načíst tabulku
> * Přidání entity tabulky
> * Dotazování tabulky
> * Odstranění entity tabulky
> * Odstranění tabulky

Tento článek ukazuje, jak vytvořit nový účet úložiště v novou skupinu prostředků, takže můžete snadno odebrat ji po dokončení. Pokud byste místo použít stávající účet úložiště, můžete to udělat místo.

Příklady vyžadují prostředí Azure PowerShell verze modulu 4.4.0 nebo novější. V okně prostředí PowerShell, spusťte `Get-Module -ListAvailable AzureRM` najít verzi. Pokud se nezobrazí nebo je nutné upgradovat, najdete v části [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps). 

Po instalaci prostředí Azure PowerShell nebo aktualizaci, musíte nainstalovat modul **AzureRmStorageTable**, která obsahuje příkazy pro správu entity. Chcete-li nainstalovat tento modul, spusťte prostředí PowerShell jako správce a použijte **instalace modulu** příkaz.

```powershell
Install-Module AzureRmStorageTable
```

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k předplatnému Azure pomocí příkazu `Login-AzureRmAccount` a postupujte podle pokynů na obrazovce.

```powershell
Login-AzureRmAccount
```

## <a name="retrieve-list-of-locations"></a>Načíst seznam umístění

Pokud nevíte, jaké umístění máte použít, můžete vypsat všechna dostupná umístění. Po zobrazení seznamu vyhledejte umístění, které chcete použít. Tyto příklady použití **eastus**. Uložení této hodnoty v proměnné **umístění** pro budoucí použití.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/New-AzureRmResourceGroup). 

Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Název skupiny prostředků uložte jako proměnnou pro budoucí použití. V tomto příkladu skupinu prostředků s názvem *pshtablesrg* je vytvořen v *eastus* oblast.

```powershell
$resourceGroup = "pshtablesrg"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Vytvoření účtu úložiště

Vytvořte účet standardního úložiště pro obecné účely s místně redundantní úložiště (LRS) pomocí [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Získá kontext účtu úložiště, který definuje účet úložiště, který se má použít. Když funguje na účet úložiště, můžete odkazovat na kontext místo opakovaně přihlašovací údaje.

```powershell
$storageAccountName = "pshtablestorage"
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```

## <a name="create-a-new-table"></a>Vytvořit novou tabulku

Chcete-li vytvořit tabulku, použijte [New-AzureStorageTable](/powershell/module/azure.storage/New-AzureStorageTable) rutiny. V tomto příkladu je volána v tabulce `pshtesttable`.

```powershell
$tableName = "pshtesttable"
New-AzureStorageTable –Name $tableName –Context $ctx
```

## <a name="retrieve-a-list-of-tables-in-the-storage-account"></a>Načíst seznam tabulek v účtu úložiště

Načíst seznam tabulek v účtu úložiště pomocí [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable).

```powershell
$storageTable = Get-AzureStorageTable –Context $ctx | select Name
```

## <a name="retrieve-a-reference-to-a-specific-table"></a>Načíst odkaz na konkrétní tabulky

K provádění operací v tabulce, potřebujete odkaz na konkrétní tabulky. Získat odkaz na pomocí [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable). 

```powershell
$storageTable = Get-AzureStorageTable –Name $tableName –Context $ctx
```

[!INCLUDE [storage-table-entities-powershell-include](../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Odstranění tabulky

Pokud chcete odstranit tabulku, použijte [odebrat AzureStorageTable](/powershell/module/azure.storage/Remove-AzureStorageTable). Tato rutina odebere tabulky, včetně všech jeho data.

```powershell
Remove-AzureStorageTable –Name $tableName –Context $ctx

# Retrieve the list of tables to verify the table has been removed.
Get-AzureStorageTable –Context $Ctx | select Name
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud jste vytvořili nový účet skupiny a úložiště prostředků na začátku tento postup, můžete odebrat všechny prostředky, které jste vytvořili v tomto cvičení odebráním skupině prostředků. Tento příkaz odstraní všechny prostředky obsažené v rámci skupiny, a také samotnou skupinu prostředků.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Další postup

V tomto postupu článku jste se dozvěděli o běžných operací úložiště Azure Table pomocí prostředí PowerShell, včetně postup: 

> [!div class="checklist"]
> * Vytvoření tabulky
> * Načíst tabulku
> * Přidání entity tabulky
> * Dotazování tabulky
> * Odstranění entity tabulky
> * Odstranění tabulky

Další informace naleznete v následujících článcích

* [Rutiny PowerShellu pro úložiště](/powershell/module/azurerm.storage#storage)

* [Práce s tabulkami úložiště Azure z prostředí PowerShell](https://blogs.technet.microsoft.com/paulomarques/2017/01/17/working-with-azure-storage-tables-from-powershell/)

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.