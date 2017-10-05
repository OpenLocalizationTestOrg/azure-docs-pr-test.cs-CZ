---
title: "Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio | Dokumentace Microsoftu"
description: "Naučte se nainstalovat nástroje Data Lake pro Visual Studio a vyvíjet a testovat skripty U-SQL."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/28/2017
ms.author: saveenr, yanacai
ms.openlocfilehash: 7bbbb08ff635477a88403a3ae6bd3486d31838ef
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a><span data-ttu-id="a9bc1-103">Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9bc1-103">Develop U-SQL scripts by using Data Lake Tools for Visual Studio</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


<span data-ttu-id="a9bc1-104">Naučte se používat sadu Visual Studio k vytváření účtů Azure Data Lake Analytics, definování úloh v [U-SQL](data-lake-analytics-u-sql-get-started.md) a odesílání úloh do služby Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-104">Learn how to use Visual Studio to create Azure Data Lake Analytics accounts, define jobs in [U-SQL](data-lake-analytics-u-sql-get-started.md), and submit jobs to the Data Lake Analytics service.</span></span> <span data-ttu-id="a9bc1-105">Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a9bc1-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="a9bc1-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a9bc1-106">Prerequisites</span></span>

* <span data-ttu-id="a9bc1-107">**Visual Studio:** Podporovány jsou všechny edice kromě Express.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-107">**Visual Studio**: All editions except Express are supported.</span></span>
    * <span data-ttu-id="a9bc1-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="a9bc1-108">Visual Studio 2017</span></span>
    * <span data-ttu-id="a9bc1-109">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="a9bc1-109">Visual Studio 2015</span></span>
    * <span data-ttu-id="a9bc1-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a9bc1-110">Visual Studio 2013</span></span>
* <span data-ttu-id="a9bc1-111">Sada **Microsoft Azure SDK pro .NET** verze 2.7.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-111">**Microsoft Azure SDK for .NET** version 2.7.1 or later.</span></span>  <span data-ttu-id="a9bc1-112">Nainstalujte ji pomocí [Instalačního programu webové platformy](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9bc1-112">Install it by using the [Web platform installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="a9bc1-113">**Účet Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-113">A **Data Lake Analytics** account.</span></span> <span data-ttu-id="a9bc1-114">Informace o vytvoření účtu najdete v tématu [Začínáme s Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a9bc1-114">To create an account, see [Get Started with Azure Data Lake Analytics using Azure portal](data-lake-analytics-get-started-portal.md).</span></span>

## <a name="install-azure-data-lake-tools-for-visual-studio"></a><span data-ttu-id="a9bc1-115">Instalace nástrojů Azure Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9bc1-115">Install Azure Data Lake Tools for Visual Studio</span></span> 

<span data-ttu-id="a9bc1-116">Stáhněte nástroje Azure Data Lake pro Visual Studio z webu [Download Center](http://aka.ms/adltoolsvs) a nainstalujte je.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-116">Download and install Azure Data Lake Tools for Visual Studio [from the Download Center](http://aka.ms/adltoolsvs).</span></span> <span data-ttu-id="a9bc1-117">Po instalaci si všimněte, že:</span><span class="sxs-lookup"><span data-stu-id="a9bc1-117">After installation, note that:</span></span>
* <span data-ttu-id="a9bc1-118">Uzel **Průzkumník serveru** > **Azure** obsahuje uzel **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-118">The **Server Explorer** > **Azure** node contains a **Data Lake Analytics** node.</span></span> 
* <span data-ttu-id="a9bc1-119">Nabídka **Nástroje** obsahuje položku **Data Lake**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-119">The **Tools** menu has a **Data Lake** item.</span></span>

## <a name="connect-to-an-azure-data-lake-analytics-account"></a><span data-ttu-id="a9bc1-120">Připojení k účtu Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="a9bc1-120">Connect to an Azure Data Lake Analytics account</span></span>

1. <span data-ttu-id="a9bc1-121">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-121">Open Visual Studio.</span></span>
2. <span data-ttu-id="a9bc1-122">Otevřete Průzkumníka serveru výběrem **Zobrazení** > **Průzkumník serveru**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-122">Open Server Explorer by selecting **View** > **Server Explorer**.</span></span>
3. <span data-ttu-id="a9bc1-123">Klikněte pravým tlačítkem na **Azure**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-123">Right-click **Azure**.</span></span> <span data-ttu-id="a9bc1-124">Pak vyberte **Připojit k předplatnému Microsoft Azure** a postupujte podle pokynů.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-124">Then select **Connect to Microsoft Azure Subscription** and follow the instructions.</span></span>
4. <span data-ttu-id="a9bc1-125">V Průzkumníku serveru vyberte **Azure** > **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-125">In Server Explorer, select **Azure** > **Data Lake Analytics**.</span></span> <span data-ttu-id="a9bc1-126">Zobrazí se seznam vašich účtů Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-126">You see a list of your Data Lake Analytics accounts.</span></span>


## <a name="write-your-first-u-sql-script"></a><span data-ttu-id="a9bc1-127">Napsání prvního skriptu U-SQL</span><span class="sxs-lookup"><span data-stu-id="a9bc1-127">Write your first U-SQL script</span></span>

<span data-ttu-id="a9bc1-128">Následující text je jednoduchý skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-128">The following text is a simple U-SQL script.</span></span> <span data-ttu-id="a9bc1-129">Definuje malou datovou sadu a zapíše ji do výchozího úložiště Data Lake Store jako soubor s názvem `/data.csv`.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-129">It defines a small dataset and writes that dataset to the default Data Lake Store as a file called `/data.csv`.</span></span>

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

### <a name="submit-a-data-lake-analytics-job"></a><span data-ttu-id="a9bc1-130">Odeslání úlohy Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="a9bc1-130">Submit a Data Lake Analytics job</span></span>

1. <span data-ttu-id="a9bc1-131">Vyberte **Soubor** > **Nový** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-131">Select **File** > **New** > **Project**.</span></span>

2. <span data-ttu-id="a9bc1-132">Vyberte typ **Projekt U-SQL** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-132">Select the **U-SQL Project** type, and then click **OK**.</span></span> <span data-ttu-id="a9bc1-133">Visual Studio vytvoří řešení se souborem **Script.usql**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-133">Visual Studio creates a solution with a **Script.usql** file.</span></span>

3. <span data-ttu-id="a9bc1-134">Vložte předchozí skript do okna **Script.usql**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-134">Paste the previous script into the **Script.usql** window.</span></span>

4. <span data-ttu-id="a9bc1-135">V levém horním rohu okna **Script.usql** zadejte účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-135">In the upper-left corner of the **Script.usql** window, specify the Data Lake Analytics account.</span></span>

    ![Odeslání projektu U-SQL sady Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. <span data-ttu-id="a9bc1-137">V levém horním rohu okna **Script.usql** vyberte **Odeslat**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-137">In the upper-left corner of the **Script.usql** window, select **Submit**.</span></span>
6. <span data-ttu-id="a9bc1-138">Zkontrolujte **Účet Analytics** a pak vyberte **Odeslat**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-138">Verify the **Analytics Account**, and then select **Submit**.</span></span> <span data-ttu-id="a9bc1-139">Po dokončení odeslání jsou výsledky odeslání dostupné ve výsledcích nástrojů Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-139">Submission results are available in the Data Lake Tools for Visual Studio Results after the submission is complete.</span></span>

    ![Odeslání projektu U-SQL sady Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
7. <span data-ttu-id="a9bc1-141">Pokud chcete zobrazit nejnovější stav úlohy a aktualizovat obrazovku, klikněte na **Aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-141">To see the latest job status and refresh the screen, click **Refresh**.</span></span> <span data-ttu-id="a9bc1-142">Když se úloha úspěšně dokončí, zobrazí se **Graf úlohy**, **Operace s metadaty**, **Historie stavů**, **Diagnostika**:</span><span class="sxs-lookup"><span data-stu-id="a9bc1-142">When the job succeeds, it shows the **Job Graph**, **MetaData Operations**, **State History**, and **Diagnostics**:</span></span>

    ![Graf výkonu úlohy U-SQL Visual Studio Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * <span data-ttu-id="a9bc1-144">**Souhrn úlohy** zobrazuje souhrn úlohy.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-144">**Job Summary** shows the summary of the job.</span></span>   
   * <span data-ttu-id="a9bc1-145">**Podrobnosti o úloze** zobrazují konkrétnější informace o úloze, včetně skriptu, prostředků a vrcholů.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-145">**Job Details** shows more specific information about the job, including the script, resources, and vertices.</span></span>
   * <span data-ttu-id="a9bc1-146">**Graf úlohy** vizualizuje průběh úlohy.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-146">**Job Graph** visualizes the progress of the job.</span></span>
   * <span data-ttu-id="a9bc1-147">**Operace s metadaty** zobrazují všechny akce provedené s katalogem U-SQL.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-147">**MetaData Operations** shows all the actions that were taken on the U-SQL catalog.</span></span>
   * <span data-ttu-id="a9bc1-148">**Data** zobrazují všechny vstupy a výstupy.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-148">**Data** shows all the inputs and outputs.</span></span>
   * <span data-ttu-id="a9bc1-149">**Diagnostika** poskytuje pokročilé analýzy spouštění úlohy a optimalizace výkonu.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-149">**Diagnostics** provides an advanced analysis for job execution and performance optimization.</span></span>

### <a name="to-check-job-state"></a><span data-ttu-id="a9bc1-150">Postup kontroly stavu úlohy</span><span class="sxs-lookup"><span data-stu-id="a9bc1-150">To check job state</span></span>

1. <span data-ttu-id="a9bc1-151">V Průzkumníku serveru vyberte **Azure** > **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-151">In Server Explorer, select **Azure** > **Data Lake Analytics**.</span></span> 
2. <span data-ttu-id="a9bc1-152">Rozbalte název účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-152">Expand the Data Lake Analytics account name.</span></span>
3. <span data-ttu-id="a9bc1-153">Dvakrát klikněte na **Úlohy**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-153">Double-click **Jobs**.</span></span>
4. <span data-ttu-id="a9bc1-154">Vyberte úlohu, kterou jste dříve odeslali.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-154">Select the job that you previously submitted.</span></span>

### <a name="to-see-the-output-of-a-job"></a><span data-ttu-id="a9bc1-155">Zobrazení výstupu úlohy</span><span class="sxs-lookup"><span data-stu-id="a9bc1-155">To see the output of a job</span></span>

1. <span data-ttu-id="a9bc1-156">V Průzkumníku serveru přejděte na úlohu, kterou jste odeslali.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-156">In Server Explorer, browse to the job you submitted.</span></span>
2. <span data-ttu-id="a9bc1-157">Klikněte na kartu **Data**.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-157">Click the **Data** tab.</span></span>
3. <span data-ttu-id="a9bc1-158">Na kartě **Výstupy úlohy** vyberte soubor `"/data.csv"`.</span><span class="sxs-lookup"><span data-stu-id="a9bc1-158">In the **Job Outputs** tab, select the `"/data.csv"` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9bc1-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a9bc1-159">Next steps</span></span>

* [<span data-ttu-id="a9bc1-160">Spouštění skriptů U-SQL na vlastní pracovní stanici za účelem testování a ladění</span><span class="sxs-lookup"><span data-stu-id="a9bc1-160">Run U-SQL scripts on your own workstation for testing and debugging</span></span>](data-lake-analytics-data-lake-tools-local-run.md)
* [<span data-ttu-id="a9bc1-161">Ladění kódu C# v úlohách U-SQL</span><span class="sxs-lookup"><span data-stu-id="a9bc1-161">Debug C# code in U-SQL jobs</span></span>](data-lake-analytics-debug-u-sql-jobs.md)
* [<span data-ttu-id="a9bc1-162">Použití nástrojů Azure Data Lake pro Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9bc1-162">Use the Azure Data Lake Tools for Visual Studio Code</span></span>](data-lake-analytics-data-lake-tools-for-vscode.md)
