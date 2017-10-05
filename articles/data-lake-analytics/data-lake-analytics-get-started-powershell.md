---
title: "Začínáme s Azure Data Lake Analytics pomocí Azure PowerShellu | Dokumentace Microsoftu"
description: "Použijte Azure PowerShell k vytvoření účtu Data Lake Analytics, vytvořte úlohu Data Lake Analytics pomocí U-SQL a úlohu odešlete. "
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
ms.openlocfilehash: 4f73e27c733edae658d1ea3bdabe48076328279b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-powershell"></a><span data-ttu-id="d2027-103">Začínáme s Azure Data Lake Analytics s využitím Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="d2027-103">Get started with Azure Data Lake Analytics using Azure PowerShell</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="d2027-104">Naučíte se, jak pomocí Azure PowerShellu vytvořit účty Azure Data Lake Analytics a následně odeslat a spustit úlohy U-SQL.</span><span class="sxs-lookup"><span data-stu-id="d2027-104">Learn how to use Azure PowerShell to create Azure Data Lake Analytics accounts and then submit and run U-SQL jobs.</span></span> <span data-ttu-id="d2027-105">Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d2027-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d2027-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d2027-106">Prerequisites</span></span>

<span data-ttu-id="d2027-107">Před zahájením tohoto kurzu musíte mít následující informace:</span><span class="sxs-lookup"><span data-stu-id="d2027-107">Before you begin this tutorial, you must have the following information:</span></span>

* <span data-ttu-id="d2027-108">**Účet služby Azure Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="d2027-108">**An Azure Data Lake Analytics account**.</span></span> <span data-ttu-id="d2027-109">Zobrazit téma [Začínáme s Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).</span><span class="sxs-lookup"><span data-stu-id="d2027-109">See [Get started with Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).</span></span>
* <span data-ttu-id="d2027-110">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="d2027-110">**A workstation with Azure PowerShell**.</span></span> <span data-ttu-id="d2027-111">Viz téma [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d2027-111">See [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="d2027-112">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="d2027-112">Log in to Azure</span></span>

<span data-ttu-id="d2027-113">Tento kurz předpokládá, že jste už s používáním Azure Powershellu obeznámení.</span><span class="sxs-lookup"><span data-stu-id="d2027-113">This tutorial assumes you are already familiar with using Azure PowerShell.</span></span> <span data-ttu-id="d2027-114">Konkrétně musíte vědět, jak se k Azure přihlásit.</span><span class="sxs-lookup"><span data-stu-id="d2027-114">In particular, you need to know how to log in to Azure.</span></span> <span data-ttu-id="d2027-115">Pokud potřebujete pomoc, přejděte na téma [Začínáme s Azure PowerShellem](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="d2027-115">See the [Get started with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps) if you need help.</span></span>

<span data-ttu-id="d2027-116">Přihlášení pomocí názvu předplatného:</span><span class="sxs-lookup"><span data-stu-id="d2027-116">To log in with a subscription name:</span></span>

```
Login-AzureRmAccount -SubscriptionName "ContosoSubscription"
```

<span data-ttu-id="d2027-117">Místo názvu předplatného můžete také pro přihlášení použít ID předplatného:</span><span class="sxs-lookup"><span data-stu-id="d2027-117">Instead of the subscription name, you can also use a subscription id to log in:</span></span>

```
Login-AzureRmAccount -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

<span data-ttu-id="d2027-118">Pokud se vám to podaří, výstup tohoto příkazu vypadá jako následující text:</span><span class="sxs-lookup"><span data-stu-id="d2027-118">If  successful, the output of this command looks like the following text:</span></span>

```
Environment           : AzureCloud
Account               : joe@contoso.com
TenantId              : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionId        : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionName      : ContosoSubscription
CurrentStorageAccount :
```

## <a name="preparing-for-the-tutorial"></a><span data-ttu-id="d2027-119">Příprava pro tento kurz</span><span class="sxs-lookup"><span data-stu-id="d2027-119">Preparing for the tutorial</span></span>

<span data-ttu-id="d2027-120">Fragment kódu PowerShellu v tomto kurzu používá následující proměnné k ukládání příslušných informací:</span><span class="sxs-lookup"><span data-stu-id="d2027-120">The PowerShell snippets in this tutorial use these variables to store this information:</span></span>

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeStoreAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="get-information-about-a-data-lake-analytics-account"></a><span data-ttu-id="d2027-121">Získání informací o účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="d2027-121">Get information about a Data Lake Analytics account</span></span>

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a><span data-ttu-id="d2027-122">Odeslání úlohy U-SQL</span><span class="sxs-lookup"><span data-stu-id="d2027-122">Submit a U-SQL job</span></span>

<span data-ttu-id="d2027-123">Vytvořte proměnnou Powershellu, aby uchovala skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="d2027-123">Create a PowerShell variable to hold the U-SQL script.</span></span>

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
    TO "/data.csv"
    USING Outputters.Csv();

"@
```

<span data-ttu-id="d2027-124">Odešlete skript.</span><span class="sxs-lookup"><span data-stu-id="d2027-124">Submit the script.</span></span>

```
$job = Submit-AdlJob -AccountName $adla –Script $script
```

<span data-ttu-id="d2027-125">Nebo můžete tento skript uložit jako soubor a odeslat ho následujícím příkazem:</span><span class="sxs-lookup"><span data-stu-id="d2027-125">Alternatively, you could save the script as a file and submit with the following command:</span></span>

```
$filename = "d:\test.usql"
$script | out-File $filename
$job = Submit-AdlJob -AccountName $adla –ScriptPath $filename
```


<span data-ttu-id="d2027-126">Získejte stav konkrétní úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2027-126">Get the status of a specific job.</span></span> <span data-ttu-id="d2027-127">Tuto rutinu používejte, dokud neuvidíte, že se úloha dokončila.</span><span class="sxs-lookup"><span data-stu-id="d2027-127">Keep using this cmdlet until you see the job is done.</span></span>

```
$job = Get-AdlJob -AccountName $adla -JobId $job.JobId
```

<span data-ttu-id="d2027-128">Namísto opakovaného volání rutiny Get-AdlAnalyticsJob, dokud se úloha nedokončí, můžete použít rutinu Wait-AdlJob.</span><span class="sxs-lookup"><span data-stu-id="d2027-128">Instead of calling Get-AdlAnalyticsJob over and over until a job finishes, you can use the Wait-AdlJob cmdlet.</span></span>

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

<span data-ttu-id="d2027-129">Stáhněte výstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="d2027-129">Download the output file.</span></span>

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "C:\data.csv"
```

## <a name="see-also"></a><span data-ttu-id="d2027-130">Viz také</span><span class="sxs-lookup"><span data-stu-id="d2027-130">See also</span></span>
* <span data-ttu-id="d2027-131">Pokud chcete použít jiné podporované nástroje a zobrazit stejný kurz, klikněte na selektory karet v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="d2027-131">To see the same tutorial using other tools, click the tab selectors on the top of the page.</span></span>
* <span data-ttu-id="d2027-132">Pokud se chcete naučit jazyk U-SQL, informace najdete v tématu [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d2027-132">To learn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="d2027-133">Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d2027-133">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
