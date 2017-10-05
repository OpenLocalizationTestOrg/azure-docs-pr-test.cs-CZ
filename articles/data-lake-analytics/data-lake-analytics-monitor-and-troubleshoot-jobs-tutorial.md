---
title: "Řešení potíží s úlohy Azure Data Lake Analytics pomocí portálu Azure | Microsoft Docs"
description: "Naučte se používat portál Azure k řešení potíží s úloh Data Lake Analytics. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: b9c7453cc0a94f70d0098ed83e5f127832065a62
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-azure-data-lake-analytics-jobs-using-azure-portal"></a><span data-ttu-id="8e690-103">Řešení potíží s úlohy Azure Data Lake Analytics pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8e690-103">Troubleshoot Azure Data Lake Analytics jobs using Azure Portal</span></span>
<span data-ttu-id="8e690-104">Naučte se používat portál Azure k řešení potíží s úloh Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="8e690-104">Learn how to use the Azure Portal to troubleshoot Data Lake Analytics jobs.</span></span>

<span data-ttu-id="8e690-105">V tomto kurzu instalační program chybějící problém zdrojového souboru a pomocí webu Azure Portal k vyřešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="8e690-105">In this tutorial, you will setup a missing source file problem, and use the Azure Portal to troubleshoot the problem.</span></span>

## <a name="submit-a-data-lake-analytics-job"></a><span data-ttu-id="8e690-106">Odeslání úlohy Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="8e690-106">Submit a Data Lake Analytics job</span></span>

<span data-ttu-id="8e690-107">Odeslání následující úlohy U-SQL:</span><span class="sxs-lookup"><span data-stu-id="8e690-107">Submit the following U-SQL job:</span></span>

```
@searchlog =
   EXTRACT UserId          int,
           Start           DateTime,
           Region          string,
           Query           string,
           Duration        int?,
           Urls            string,
           ClickedUrls     string
   FROM "/Samples/Data/SearchLog.tsv1"
   USING Extractors.Tsv();

OUTPUT @searchlog   
   TO "/output/SearchLog-from-adls.csv"
   USING Outputters.Csv();
```
    
<span data-ttu-id="8e690-108">Zdrojový soubor definované ve skriptu je **/Samples/Data/SearchLog.tsv1**, kde by měly být **/Samples/Data/SearchLog.tsv**.</span><span class="sxs-lookup"><span data-stu-id="8e690-108">The source file defined in the script is **/Samples/Data/SearchLog.tsv1**, where it should be **/Samples/Data/SearchLog.tsv**.</span></span>


## <a name="troubleshoot-the-job"></a><span data-ttu-id="8e690-109">Řešení potíží s úlohy</span><span class="sxs-lookup"><span data-stu-id="8e690-109">Troubleshoot the job</span></span>

<span data-ttu-id="8e690-110">**Chcete-li zobrazit všechny úlohy**</span><span class="sxs-lookup"><span data-stu-id="8e690-110">**To see all the jobs**</span></span>

1. <span data-ttu-id="8e690-111">Z portálu Azure klikněte na tlačítko **Microsoft Azure** v levém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="8e690-111">From the Azure portal, click **Microsoft Azure** in the upper left corner.</span></span>
2. <span data-ttu-id="8e690-112">Klikněte na dlaždici s názvem účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="8e690-112">Click the tile with your Data Lake Analytics account name.</span></span>  <span data-ttu-id="8e690-113">Úlohu souhrnu se zobrazí na **úlohy správy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="8e690-113">The job summary is shown on the **Job Management** tile.</span></span>

    ![Správa úloh Azure Data Lake Analytics](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    <span data-ttu-id="8e690-115">Úlohy správy poskytuje přehled o stavu úlohy.</span><span class="sxs-lookup"><span data-stu-id="8e690-115">The job Management gives you a glance of the job status.</span></span> <span data-ttu-id="8e690-116">Všimněte si, že je neúspěšnou úlohu.</span><span class="sxs-lookup"><span data-stu-id="8e690-116">Notice there is a failed job.</span></span>
3. <span data-ttu-id="8e690-117">Klikněte **úlohy správy** dlaždice zobrazíte úlohy.</span><span class="sxs-lookup"><span data-stu-id="8e690-117">Click the **Job Management** tile to see the jobs.</span></span> <span data-ttu-id="8e690-118">Úlohy jsou rozdělené do **systémem**, **zařazeno ve frontě**, a **ukončeno**.</span><span class="sxs-lookup"><span data-stu-id="8e690-118">The jobs are categorized in **Running**, **Queued**, and **Ended**.</span></span> <span data-ttu-id="8e690-119">Zobrazí se vaše neúspěšnou úlohu v **ukončeno** části.</span><span class="sxs-lookup"><span data-stu-id="8e690-119">You shall see your failed job in the **Ended** section.</span></span> <span data-ttu-id="8e690-120">Použije se první z nich v seznamu.</span><span class="sxs-lookup"><span data-stu-id="8e690-120">It shall be first one in the list.</span></span> <span data-ttu-id="8e690-121">Pokud máte mnoho úloh, můžete kliknout na **filtru** můžete vyhledat úlohy.</span><span class="sxs-lookup"><span data-stu-id="8e690-121">When you have a lot of jobs, you can click **Filter** to help you to locate jobs.</span></span>

    ![Filtrujte úlohy Azure Data Lake Analytics](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. <span data-ttu-id="8e690-123">Klikněte na možnost neúspěšné úlohy ze seznamu otevřete nové okno Podrobnosti úlohy:</span><span class="sxs-lookup"><span data-stu-id="8e690-123">Click the failed job from the list to open the job details in a new blade:</span></span>

    ![Azure Data Lake Analytics se nezdařilo úlohy](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    <span data-ttu-id="8e690-125">Upozornění **odešlete znovu** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8e690-125">Notice the **Resubmit** button.</span></span> <span data-ttu-id="8e690-126">Po vyřešení problému znovu odešlete úlohu.</span><span class="sxs-lookup"><span data-stu-id="8e690-126">After you fix the problem, you can resubmit the job.</span></span>
5. <span data-ttu-id="8e690-127">Klikněte na tlačítko zvýrazněná část na předchozím snímku obrazovky Otevřít podrobnosti o chybě.</span><span class="sxs-lookup"><span data-stu-id="8e690-127">Click highlighted part from the previous screenshot to open the error details.</span></span>  <span data-ttu-id="8e690-128">Zobrazí se něco podobného jako:</span><span class="sxs-lookup"><span data-stu-id="8e690-128">You shall see something like:</span></span>

    ![Azure Data Lake Analytics se nezdařilo podrobnosti úlohy](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    <span data-ttu-id="8e690-130">Zde zjistíte, že zdrojová složka nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="8e690-130">It tells you the source folder is not found.</span></span>
6. <span data-ttu-id="8e690-131">Klikněte na tlačítko **duplicitní skriptu**.</span><span class="sxs-lookup"><span data-stu-id="8e690-131">Click **Duplicate Script**.</span></span>
7. <span data-ttu-id="8e690-132">Aktualizace **FROM** cesta pro následující:</span><span class="sxs-lookup"><span data-stu-id="8e690-132">Update the **FROM** path to the following:</span></span>

    <span data-ttu-id="8e690-133">"/ Samples/Data/SearchLog.tsv"</span><span class="sxs-lookup"><span data-stu-id="8e690-133">"/Samples/Data/SearchLog.tsv"</span></span>
8. <span data-ttu-id="8e690-134">Klikněte na **Odeslat úlohu**.</span><span class="sxs-lookup"><span data-stu-id="8e690-134">Click **Submit Job**.</span></span>

## <a name="see-also"></a><span data-ttu-id="8e690-135">Viz také</span><span class="sxs-lookup"><span data-stu-id="8e690-135">See also</span></span>
* [<span data-ttu-id="8e690-136">Přehled Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="8e690-136">Azure Data Lake Analytics overview</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="8e690-137">Začínáme s Azure Data Lake Analytics pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8e690-137">Get started with Azure Data Lake Analytics using Azure PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="8e690-138">Začínáme s Azure Data Lake Analytics a jazykem U-SQL pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e690-138">Get started with Azure Data Lake Analytics and U-SQL using Visual Studio</span></span>](data-lake-analytics-u-sql-get-started.md)
* [<span data-ttu-id="8e690-139">Správa Azure Data Lake Analytics pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8e690-139">Manage Azure Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-manage-use-portal.md)
