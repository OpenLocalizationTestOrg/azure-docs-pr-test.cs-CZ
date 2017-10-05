---
title: "Začínáme s Azure Data Lake Analytics pomocí portálu Azure Portal | Dokumentace Microsoftu"
description: "Naučte se používat Azure Portal k vytvoření účtu Data Lake Analytics, vytvoření úlohy Data Lake Analytics pomocí U-SQL a odeslání úlohy. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: edmaca
ms.openlocfilehash: 2722a2d72ed90ea0005362563ecaee30750c040a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-portal"></a><span data-ttu-id="17095-103">Začínáme s Azure Data Lake Analytics s využitím webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="17095-103">Get started with Azure Data Lake Analytics using Azure portal</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="17095-104">Naučte se používat Azure Portal k vytváření účtů Azure Data Lake Analytics, definování úloh v [U-SQL](data-lake-analytics-u-sql-get-started.md) a odesílání úloh do služby Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="17095-104">Learn how to use the Azure portal to create Azure Data Lake Analytics accounts, define jobs in [U-SQL](data-lake-analytics-u-sql-get-started.md), and submit jobs to the Data Lake Analytics service.</span></span> <span data-ttu-id="17095-105">Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="17095-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="17095-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="17095-106">Prerequisites</span></span>

<span data-ttu-id="17095-107">Než začnete tento kurz, musíte mít **předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="17095-107">Before you begin this tutorial, you must have an **Azure subscription**.</span></span> <span data-ttu-id="17095-108">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="17095-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="17095-109">Vytvoření účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="17095-109">Create a Data Lake Analytics account</span></span>

<span data-ttu-id="17095-110">Teď zároveň vytvoříte účet Data Lake Analytics a Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="17095-110">Now, you will create a Data Lake Analytics and a Data Lake Store account at the same time.</span></span>  <span data-ttu-id="17095-111">Tento krok je jednoduchý a trvá jen asi 60 vteřin.</span><span class="sxs-lookup"><span data-stu-id="17095-111">This step is simple and only takes about 60 seconds to finish.</span></span>

1. <span data-ttu-id="17095-112">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="17095-112">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="17095-113">Klikněte na **Nový** >  **Data a analýzy** > **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="17095-113">Click **New** >  **Data + Analytics** > **Data Lake Analytics**.</span></span>
3. <span data-ttu-id="17095-114">Vyberte hodnoty pro následující položky:</span><span class="sxs-lookup"><span data-stu-id="17095-114">Select values for the following items:</span></span>
   * <span data-ttu-id="17095-115">**Název:** Pojmenujte svůj účet Data Lake Analytics (povolena jsou pouze malá písmena a číslice).</span><span class="sxs-lookup"><span data-stu-id="17095-115">**Name**: Name your Data Lake Analytics account (Only lower case letters and numbers allowed).</span></span>
   * <span data-ttu-id="17095-116">**Předplatné**: Zvolte předplatné Azure použité pro účet Analytics.</span><span class="sxs-lookup"><span data-stu-id="17095-116">**Subscription**: Choose the Azure subscription used for the Analytics account.</span></span>
   * <span data-ttu-id="17095-117">**Skupina prostředků**.</span><span class="sxs-lookup"><span data-stu-id="17095-117">**Resource Group**.</span></span> <span data-ttu-id="17095-118">Vyberte některou z existujících skupin prostředků Azure nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="17095-118">Select an existing Azure Resource Group or create a new one.</span></span>
   * <span data-ttu-id="17095-119">**Umístění**.</span><span class="sxs-lookup"><span data-stu-id="17095-119">**Location**.</span></span> <span data-ttu-id="17095-120">Vyberte datové centrum Azure pro účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="17095-120">Select an Azure data center for the Data Lake Analytics account.</span></span>
   * <span data-ttu-id="17095-121">**Data Lake Store**: Postupujte podle pokynů a vytvořte nový účet Data Lake Store nebo vyberte některý z existujících.</span><span class="sxs-lookup"><span data-stu-id="17095-121">**Data Lake Store**: Follow the instruction to create a new Data Lake Store account, or select an existing one.</span></span> 
4. <span data-ttu-id="17095-122">Volitelně vyberte cenovou úroveň pro svůj účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="17095-122">Optionally, select a pricing tier for your Data Lake Analytics account.</span></span>
5. <span data-ttu-id="17095-123">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="17095-123">Click **Create**.</span></span> 


## <a name="your-first-u-sql-script"></a><span data-ttu-id="17095-124">Váš první skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="17095-124">Your first U-SQL script</span></span>

<span data-ttu-id="17095-125">Následující text je velmi jednoduchý skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="17095-125">The following text is a very simple U-SQL script.</span></span> <span data-ttu-id="17095-126">Celá jeho úloha spočívá v definování malé datové sady v rámci skriptu a následném zapsání této datové sady do výchozího úložiště Data Lake Store jako soubor s názvem `/data.csv`.</span><span class="sxs-lookup"><span data-stu-id="17095-126">All it does is define a small dataset within the script and then write that dataset out to the default Data Lake Store as a file called `/data.csv`.</span></span>

```
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
```

## <a name="submit-a-u-sql-job"></a><span data-ttu-id="17095-127">Odeslání úlohy U-SQL</span><span class="sxs-lookup"><span data-stu-id="17095-127">Submit a U-SQL job</span></span>

1. <span data-ttu-id="17095-128">V účtu Data Lake Analytics klikněte na možnost **Nová úloha**.</span><span class="sxs-lookup"><span data-stu-id="17095-128">From the Data Lake Analytics account, click **New Job**.</span></span>
2. <span data-ttu-id="17095-129">Vložte výše uvedený text skriptu U-SQL.</span><span class="sxs-lookup"><span data-stu-id="17095-129">Paste in the text of the U-SQL script shown above.</span></span> 
3. <span data-ttu-id="17095-130">Klikněte na **Odeslat úlohu**.</span><span class="sxs-lookup"><span data-stu-id="17095-130">Click **Submit Job**.</span></span>   
4. <span data-ttu-id="17095-131">Počkejte, až se stav úlohy změní na **Úspěch**.</span><span class="sxs-lookup"><span data-stu-id="17095-131">Wait until the job status changes to **Succeeded**.</span></span>
5. <span data-ttu-id="17095-132">Pokud úloha nebyla úspěšná, přejděte na téma [Sledování úloh Data Lake Analytics a odstraňování potíží](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="17095-132">If the job failed, see [Monitor and troubleshoot Data Lake Analytics jobs](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span></span>
6. <span data-ttu-id="17095-133">Klikněte na kartu **Výstup** a potom na `data.csv`.</span><span class="sxs-lookup"><span data-stu-id="17095-133">Click the **Output** tab, and then click `data.csv`.</span></span> 

## <a name="see-also"></a><span data-ttu-id="17095-134">Viz také</span><span class="sxs-lookup"><span data-stu-id="17095-134">See also</span></span>

* <span data-ttu-id="17095-135">Pokud chcete začít s vývojem aplikací U-SQL, přejděte k tématu [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="17095-135">To get started developing U-SQL applications, see [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="17095-136">Pokud se chcete naučit jazyk U-SQL, informace najdete v tématu [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="17095-136">To learn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="17095-137">Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="17095-137">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
