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
# <a name="get-started-with-azure-data-lake-analytics-using-azure-powershell"></a><span data-ttu-id="6ad69-103">Začínáme s Azure Data Lake Analytics s využitím Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="6ad69-103">Get started with Azure Data Lake Analytics using Azure PowerShell</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="6ad69-104">Zjistěte, jak toouse toocreate prostředí Azure PowerShell Azure Data Lake Analytics účtů a potom odeslat a spouštění úloh U-SQL.</span><span class="sxs-lookup"><span data-stu-id="6ad69-104">Learn how toouse Azure PowerShell toocreate Azure Data Lake Analytics accounts and then submit and run U-SQL jobs.</span></span> <span data-ttu-id="6ad69-105">Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6ad69-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6ad69-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6ad69-106">Prerequisites</span></span>

<span data-ttu-id="6ad69-107">Než začnete tento kurz, musíte mít hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="6ad69-107">Before you begin this tutorial, you must have hello following information:</span></span>

* <span data-ttu-id="6ad69-108">**Účet služby Azure Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="6ad69-108">**An Azure Data Lake Analytics account**.</span></span> <span data-ttu-id="6ad69-109">Zobrazit téma [Začínáme s Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).</span><span class="sxs-lookup"><span data-stu-id="6ad69-109">See [Get started with Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).</span></span>
* <span data-ttu-id="6ad69-110">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="6ad69-110">**A workstation with Azure PowerShell**.</span></span> <span data-ttu-id="6ad69-111">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6ad69-111">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="6ad69-112">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="6ad69-112">Log in tooAzure</span></span>

<span data-ttu-id="6ad69-113">Tento kurz předpokládá, že jste už s používáním Azure Powershellu obeznámení.</span><span class="sxs-lookup"><span data-stu-id="6ad69-113">This tutorial assumes you are already familiar with using Azure PowerShell.</span></span> <span data-ttu-id="6ad69-114">Konkrétně je nutné tooknow, jak toolog v tooAzure.</span><span class="sxs-lookup"><span data-stu-id="6ad69-114">In particular, you need tooknow how toolog in tooAzure.</span></span> <span data-ttu-id="6ad69-115">V tématu hello [Začínáme s Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps) Pokud potřebujete pomoc.</span><span class="sxs-lookup"><span data-stu-id="6ad69-115">See hello [Get started with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps) if you need help.</span></span>

<span data-ttu-id="6ad69-116">toolog pomocí název odběru:</span><span class="sxs-lookup"><span data-stu-id="6ad69-116">toolog in with a subscription name:</span></span>

```
Login-AzureRmAccount -SubscriptionName "ContosoSubscription"
```

<span data-ttu-id="6ad69-117">Místo názvu hello předplatné můžete použít také toolog id předplatného v:</span><span class="sxs-lookup"><span data-stu-id="6ad69-117">Instead of hello subscription name, you can also use a subscription id toolog in:</span></span>

```
Login-AzureRmAccount -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

<span data-ttu-id="6ad69-118">V případě úspěšného hello výstup tohoto příkazu vypadá hello následující text:</span><span class="sxs-lookup"><span data-stu-id="6ad69-118">If  successful, hello output of this command looks like hello following text:</span></span>

```
Environment           : AzureCloud
Account               : joe@contoso.com
TenantId              : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionId        : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionName      : ContosoSubscription
CurrentStorageAccount :
```

## <a name="preparing-for-hello-tutorial"></a><span data-ttu-id="6ad69-119">Příprava pro kurz hello</span><span class="sxs-lookup"><span data-stu-id="6ad69-119">Preparing for hello tutorial</span></span>

<span data-ttu-id="6ad69-120">fragmenty kódu Hello prostředí PowerShell v tomto kurzu použijte tyto proměnné toostore tyto informace:</span><span class="sxs-lookup"><span data-stu-id="6ad69-120">hello PowerShell snippets in this tutorial use these variables toostore this information:</span></span>

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeStoreAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="get-information-about-a-data-lake-analytics-account"></a><span data-ttu-id="6ad69-121">Získání informací o účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="6ad69-121">Get information about a Data Lake Analytics account</span></span>

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a><span data-ttu-id="6ad69-122">Odeslání úlohy U-SQL</span><span class="sxs-lookup"><span data-stu-id="6ad69-122">Submit a U-SQL job</span></span>

<span data-ttu-id="6ad69-123">Vytvořte skript prostředí PowerShell proměnné toohold hello U-SQL.</span><span class="sxs-lookup"><span data-stu-id="6ad69-123">Create a PowerShell variable toohold hello U-SQL script.</span></span>

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

<span data-ttu-id="6ad69-124">Odešlete skript hello.</span><span class="sxs-lookup"><span data-stu-id="6ad69-124">Submit hello script.</span></span>

```
$job = Submit-AdlJob -AccountName $adla –Script $script
```

<span data-ttu-id="6ad69-125">Alternativně může uložit hello skript jako soubor a odeslat s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6ad69-125">Alternatively, you could save hello script as a file and submit with hello following command:</span></span>

```
$filename = "d:\test.usql"
$script | out-File $filename
$job = Submit-AdlJob -AccountName $adla –ScriptPath $filename
```


<span data-ttu-id="6ad69-126">Získáte stav hello určité úlohy.</span><span class="sxs-lookup"><span data-stu-id="6ad69-126">Get hello status of a specific job.</span></span> <span data-ttu-id="6ad69-127">Tato rutina dál používat, dokud se nezobrazí, že se provádí úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="6ad69-127">Keep using this cmdlet until you see hello job is done.</span></span>

```
$job = Get-AdlJob -AccountName $adla -JobId $job.JobId
```

<span data-ttu-id="6ad69-128">Namísto volání Get-AdlAnalyticsJob opakovaně, dokud nebude dokončeno úlohu, můžete použít rutiny čekání AdlJob hello.</span><span class="sxs-lookup"><span data-stu-id="6ad69-128">Instead of calling Get-AdlAnalyticsJob over and over until a job finishes, you can use hello Wait-AdlJob cmdlet.</span></span>

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

<span data-ttu-id="6ad69-129">Stáhněte si soubor výstup hello.</span><span class="sxs-lookup"><span data-stu-id="6ad69-129">Download hello output file.</span></span>

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "C:\data.csv"
```

## <a name="see-also"></a><span data-ttu-id="6ad69-130">Viz také</span><span class="sxs-lookup"><span data-stu-id="6ad69-130">See also</span></span>
* <span data-ttu-id="6ad69-131">toosee hello stejný kurz pomocí jiných nástrojů, klikněte na selektory karet hello na hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="6ad69-131">toosee hello same tutorial using other tools, click hello tab selectors on hello top of hello page.</span></span>
* <span data-ttu-id="6ad69-132">toolearn U-SQL, najdete v části [Začínáme s jazykem Azure Data Lake Analytics U-SQL](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6ad69-132">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="6ad69-133">Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6ad69-133">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
