---
title: "Provádění operací rozhraní API služby Azure Cosmos DB tabulky pomocí prostředí PowerShell | Microsoft Docs"
description: "Jak provádět operace rozhraní API služby Azure Cosmos DB tabulky pomocí prostředí PowerShell"
services: storage
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 11/15/2017
ms.author: robinsh
ms.openlocfilehash: d0f835db8a9fbe3833a9c7931ad1d8b4a778f016
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="perform-azure-cosmos-db-table-api-operations-with-azure-powershell"></a>Provádění operací rozhraní API služby Azure Cosmos DB tabulky s prostředím Azure PowerShell 

>[!NOTE]
>Rozhraní API tabulky DB Cosmos Azure poskytuje prémiových funkcí pro úložiště table, jako jsou to globální distribuční, nízká latence čtení a zápisy, automatické sekundární indexování a vyhrazenou propustností. Ve většině případů je specifické pro rozhraní API služby Azure Cosmos DB tabulky příkazy prostředí PowerShell v pracovním tohoto článku rozhraní API služby Azure Cosmos DB tabulky a Azure Table storage, ale v tomto článku. Pokud používáte Azure Table storage, najdete v části [operace úložiště Azure Table provést v prostředí Azure PowerShell](table-storage-how-to-use-powershell.md).
>

Rozhraní API tabulky databáze Azure Cosmos umožňuje ukládání a dotazování obrovských sad strukturovaných, nerelačních data. Hlavní komponenty služby jsou tabulky, entit a vlastnosti. Tabulka je kolekce entit. Entita je sada vlastností. Každá entita může mít až 252 vlastností, které jsou všechny páry název hodnota. Tento článek předpokládá, že jste již obeznámeni s koncepty rozhraní API služby Azure Cosmos DB tabulky. Podrobné informace najdete v tématu [Úvod do rozhraní API služby Azure Cosmos DB tabulky](table-introduction.md) a [vytvoření aplikace .NET pomocí rozhraní API tabulky](create-table-dotnet.md).

Tento článek s postupy popisuje běžné operace rozhraní API tabulky. Získáte informace o těchto tématech: 

> [!div class="checklist"]
> * Vytvoření tabulky
> * Načíst tabulku
> * Přidání entity tabulky
> * Dotazování tabulky
> * Odstranění entity tabulky

## <a name="prerequisites"></a>Požadavky

Příklady vyžadují prostředí Azure PowerShell verze modulu 4.4.0 nebo novější. V okně prostředí PowerShell, spusťte `Get-Module -ListAvailable AzureRM` najít verzi. Pokud se nezobrazí nebo je nutné upgradovat, najdete v části [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps). 

Po instalaci prostředí Azure PowerShell nebo aktualizaci, musíte nainstalovat modul **AzureRmStorageTable**, která obsahuje příkazy pro správu entity. Chcete-li nainstalovat tento modul, spusťte prostředí PowerShell jako správce a použijte **instalace modulu** příkaz.

```powershell
Install-Module AzureRmStorageTable
```

Potom nainstalujte Azure Cosmos DB sestavení místně, aby bylo možné používat tyto rutiny prostředí PowerShell. Pokyny o tom, jak to udělat najdete v tématu [modul Azure RM úložiště tabulek PowerShell pro tabulky DB Cosmos](https://blogs.technet.microsoft.com/paulomarques/2017/05/23/azure-rm-storage-tables-powershell-module-now-includes-support-for-cosmos-db-tables/).

Můžete vyzkoušet na následující cvičení, potřebujete účet Azure Cosmos DB databáze. Pokud jste ještě nemáte, vytvořte novou Cosmos Azure DB účtu pomocí [portál Azure](https://portal.azure.com). Nápovědu k vytvoření nového účtu databáze najdete v tématu [Cosmos databázi Azure: vytvoření databázového účtu](create-table-dotnet.md#create-a-database-account).

Získat účet databáze skupina název a prostředků z portálu. je nutné tyto hodnoty uvést do vašeho skriptu k přístupu k tabulce. 

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k předplatnému Azure pomocí příkazu `Login-AzureRmAccount` a postupujte podle pokynů na obrazovce.

```powershell
Login-AzureRmAccount
```

## <a name="create-a-table-or-reference-a-table"></a>Vytvoří tabulku nebo zadejte odkaz na tabulku

Chcete-li vytvořit tabulku nebo získat odkaz na tabulku, použijte **Get-AzureStorageTableTable**. Když zavoláte tuto rutinu s názvem tabulky, který neexistuje, vytvoří novou tabulku s tímto názvem a vrátí odkaz na novou tabulku. Pokud tabulka neexistuje, vrátí odkaz na existující tabulce.

```powershell
# set the name of the resource group in which your Cosmos DB Account resides.
$resourceGroup = "contosocosmosrg"
# if you want to make sure the resource group is valid, try this command
Get-AzureRmResourceGroup -Name $resourceGroup

# set the Cosmos DB account name 
$cosmosDBAccountName = "contosocosmostbl" 

# set the table name 
$tableName = "contosotable1"

# Get a reference to a table. This creates the table if it doesn't exist.
$storageTable = Get-AzureStorageTableTable `
  -resourceGroup $resourceGroup `
  -tableName $tableName `
  -cosmosDbAccount $cosmosDBAccountName 
```

Nelze seznam tabulek v účtu Azure Cosmos DB pomocí prostředí PowerShell, ale může přihlásit k portálu a podívejte se na tabulku. Nyní podíváme, jak spravovat entity v tabulce.

[!INCLUDE [storage-table-entities-powershell-include](../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Odstranění tabulky 

PowerShell nepodporuje odstraňování tabulek z databáze Azure Cosmos. Chcete-li odstranit tabulku, přejděte na [portál Azure](https://azure.portal.com), vyhledejte účet Azure Cosmos DB používáte, pak najít a odstranit tabulku. 

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud jste vytvořili novou skupinu prostředků a vytvořit nový účet Azure Cosmos DB v dané skupině, můžete odebrat všechny prostředky jste vytvořili v tomto cvičení odebráním skupině prostředků. Tento příkaz odstraní všechny prostředky obsažené v rámci skupiny, a také samotnou skupinu prostředků.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Další kroky

V tomto postupu článku jste se dozvěděli o běžných operací rozhraní API tabulky pomocí prostředí PowerShell, včetně postup: 

> [!div class="checklist"]
> * Vytvoření tabulky
> * Načíst tabulku
> * Přidání entity tabulky
> * Dotazování tabulky
> * Odstranění entity tabulky

Další informace najdete v následujících článcích:

* [Práce s tabulkami úložiště Azure z prostředí PowerShell](https://blogs.technet.microsoft.com/paulomarques/2017/01/17/working-with-azure-storage-tables-from-powershell/)

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.