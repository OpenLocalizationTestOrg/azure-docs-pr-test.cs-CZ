---
title: "aaaGet začít s Azure Data Lake Analytics pomocí Azure PowerShell | Microsoft Docs"
description: "Použití Azure PowerShell toocreate účtu Data Lake Analytics, vytvoření úlohy Data Lake Analytics pomocí U-SQL a odeslání úlohy hello. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 8a4e901e-9656-4a60-90d0-d78ff2f00656
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: edmaca
ms.openlocfilehash: cb9b35352d1cc9a78337448b1d6835875a212e08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Začínáme s Azure Data Lake Analytics s využitím Azure PowerShellu
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Zjistěte, jak toouse toocreate prostředí Azure PowerShell Azure Data Lake Analytics účtů a potom odeslat a spouštění úloh U-SQL. Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Požadavky

Než začnete tento kurz, musíte mít hello následující informace:

* **Účet služby Azure Data Lake Analytics**. Zobrazit téma [Začínáme s Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).
* **Pracovní stanice s prostředím Azure PowerShell**. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Tento kurz předpokládá, že jste už s používáním Azure Powershellu obeznámení. Konkrétně je nutné tooknow, jak toolog v tooAzure. V tématu hello [Začínáme s Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps) Pokud potřebujete pomoc.

toolog pomocí název odběru:

```
Login-AzureRmAccount -SubscriptionName "ContosoSubscription"
```

Místo názvu hello předplatné můžete použít také toolog id předplatného v:

```
Login-AzureRmAccount -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

V případě úspěšného hello výstup tohoto příkazu vypadá hello následující text:

```
Environment           : AzureCloud
Account               : joe@contoso.com
TenantId              : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionId        : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionName      : ContosoSubscription
CurrentStorageAccount :
```

## <a name="preparing-for-hello-tutorial"></a>Příprava pro kurz hello

fragmenty kódu Hello prostředí PowerShell v tomto kurzu použijte tyto proměnné toostore tyto informace:

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeStoreAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="get-information-about-a-data-lake-analytics-account"></a>Získání informací o účtu Data Lake Analytics

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a>Odeslání úlohy U-SQL

Vytvořte skript prostředí PowerShell proměnné toohold hello U-SQL.

```
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();

"@
```

Odešlete skript hello.

```
$job = Submit-AdlJob -AccountName $adla –Script $script
```

Alternativně může uložit hello skript jako soubor a odeslat s hello následující příkaz:

```
$filename = "d:\test.usql"
$script | out-File $filename
$job = Submit-AdlJob -AccountName $adla –ScriptPath $filename
```


Získáte stav hello určité úlohy. Tato rutina dál používat, dokud se nezobrazí, že se provádí úlohy hello.

```
$job = Get-AdlJob -AccountName $adla -JobId $job.JobId
```

Namísto volání Get-AdlAnalyticsJob opakovaně, dokud nebude dokončeno úlohu, můžete použít rutiny čekání AdlJob hello.

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

Stáhněte si soubor výstup hello.

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "C:\data.csv"
```

## <a name="see-also"></a>Viz také
* toosee hello stejný kurz pomocí jiných nástrojů, klikněte na selektory karet hello na hello horní části stránky hello.
* toolearn U-SQL, najdete v části [Začínáme s jazykem Azure Data Lake Analytics U-SQL](data-lake-analytics-u-sql-get-started.md).
* Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).
