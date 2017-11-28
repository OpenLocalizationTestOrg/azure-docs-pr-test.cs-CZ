---
title: "skripty aaaDevelop U-SQL pomocí nástrojů Data Lake pro Visual Studio | Microsoft Docs"
description: "Zjistěte, jak nástroje tooinstall Data Lake pro Visual Studio a jak toodevelop a testování skriptů U-SQL."
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
ms.openlocfilehash: 7a0c02c275b8cefecbe784ba63969cbf24a150d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a><span data-ttu-id="d9bbb-103">Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d9bbb-103">Develop U-SQL scripts by using Data Lake Tools for Visual Studio</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


<span data-ttu-id="d9bbb-104">Zjistěte, jak účtů toouse Visual Studio toocreate Azure Data Lake Analytics, definování úloh v [U-SQL](data-lake-analytics-u-sql-get-started.md)a odeslání úlohy toohello Data Lake Analytics služby.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-104">Learn how toouse Visual Studio toocreate Azure Data Lake Analytics accounts, define jobs in [U-SQL](data-lake-analytics-u-sql-get-started.md), and submit jobs toohello Data Lake Analytics service.</span></span> <span data-ttu-id="d9bbb-105">Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d9bbb-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="d9bbb-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d9bbb-106">Prerequisites</span></span>

* <span data-ttu-id="d9bbb-107">**Visual Studio:** Podporovány jsou všechny edice kromě Express.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-107">**Visual Studio**: All editions except Express are supported.</span></span>
    * <span data-ttu-id="d9bbb-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="d9bbb-108">Visual Studio 2017</span></span>
    * <span data-ttu-id="d9bbb-109">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="d9bbb-109">Visual Studio 2015</span></span>
    * <span data-ttu-id="d9bbb-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d9bbb-110">Visual Studio 2013</span></span>
* <span data-ttu-id="d9bbb-111">Sada **Microsoft Azure SDK pro .NET** verze 2.7.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-111">**Microsoft Azure SDK for .NET** version 2.7.1 or later.</span></span>  <span data-ttu-id="d9bbb-112">Nainstalujte ji pomocí hello [instalačního programu webové platformy](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="d9bbb-112">Install it by using hello [Web platform installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="d9bbb-113">**Účet Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-113">A **Data Lake Analytics** account.</span></span> <span data-ttu-id="d9bbb-114">toocreate na účet, najdete v části [Začínáme s Azure Data Lake Analytics pomocí portálu Azure](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d9bbb-114">toocreate an account, see [Get Started with Azure Data Lake Analytics using Azure portal](data-lake-analytics-get-started-portal.md).</span></span>

## <a name="install-azure-data-lake-tools-for-visual-studio"></a><span data-ttu-id="d9bbb-115">Instalace nástrojů Azure Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d9bbb-115">Install Azure Data Lake Tools for Visual Studio</span></span> 

<span data-ttu-id="d9bbb-116">Stáhněte a nainstalujte nástroje Azure Data Lake pro Visual Studio [z hello Download Center](http://aka.ms/adltoolsvs).</span><span class="sxs-lookup"><span data-stu-id="d9bbb-116">Download and install Azure Data Lake Tools for Visual Studio [from hello Download Center](http://aka.ms/adltoolsvs).</span></span> <span data-ttu-id="d9bbb-117">Po instalaci si všimněte, že:</span><span class="sxs-lookup"><span data-stu-id="d9bbb-117">After installation, note that:</span></span>
* <span data-ttu-id="d9bbb-118">Hello **Průzkumníka serveru** > **Azure** uzel obsahuje **Data Lake Analytics** uzlu.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-118">hello **Server Explorer** > **Azure** node contains a **Data Lake Analytics** node.</span></span> 
* <span data-ttu-id="d9bbb-119">Hello **nástroje** má nabídky **Data Lake** položky.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-119">hello **Tools** menu has a **Data Lake** item.</span></span>

## <a name="connect-tooan-azure-data-lake-analytics-account"></a><span data-ttu-id="d9bbb-120">Připojit tooan účtu Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="d9bbb-120">Connect tooan Azure Data Lake Analytics account</span></span>

1. <span data-ttu-id="d9bbb-121">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-121">Open Visual Studio.</span></span>
2. <span data-ttu-id="d9bbb-122">Otevřete Průzkumníka serveru výběrem **Zobrazení** > **Průzkumník serveru**.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-122">Open Server Explorer by selecting **View** > **Server Explorer**.</span></span>
3. <span data-ttu-id="d9bbb-123">Klikněte pravým tlačítkem na **Azure**.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-123">Right-click **Azure**.</span></span> <span data-ttu-id="d9bbb-124">Potom vyberte **připojit tooMicrosoft předplatné Azure** a postupujte podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-124">Then select **Connect tooMicrosoft Azure Subscription** and follow hello instructions.</span></span>
4. <span data-ttu-id="d9bbb-125">V Průzkumníku serveru vyberte **Azure** > **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-125">In Server Explorer, select **Azure** > **Data Lake Analytics**.</span></span> <span data-ttu-id="d9bbb-126">Zobrazí se seznam vašich účtů Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-126">You see a list of your Data Lake Analytics accounts.</span></span>


## <a name="write-your-first-u-sql-script"></a><span data-ttu-id="d9bbb-127">Napsání prvního skriptu U-SQL</span><span class="sxs-lookup"><span data-stu-id="d9bbb-127">Write your first U-SQL script</span></span>

<span data-ttu-id="d9bbb-128">Následující text Hello je jednoduchý skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-128">hello following text is a simple U-SQL script.</span></span> <span data-ttu-id="d9bbb-129">Definuje, na malou datovou sadu a zápisů, které volá datovou sadu toohello výchozí Data Lake Store jako soubor `/data.csv`.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-129">It defines a small dataset and writes that dataset toohello default Data Lake Store as a file called `/data.csv`.</span></span>

```
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
```

### <a name="submit-a-data-lake-analytics-job"></a><span data-ttu-id="d9bbb-130">Odeslání úlohy Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="d9bbb-130">Submit a Data Lake Analytics job</span></span>

1. <span data-ttu-id="d9bbb-131">Vyberte **Soubor** > **Nový** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-131">Select **File** > **New** > **Project**.</span></span>

2. <span data-ttu-id="d9bbb-132">Vyberte hello **projekt U-SQL** a klepněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-132">Select hello **U-SQL Project** type, and then click **OK**.</span></span> <span data-ttu-id="d9bbb-133">Visual Studio vytvoří řešení se souborem **Script.usql**.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-133">Visual Studio creates a solution with a **Script.usql** file.</span></span>

3. <span data-ttu-id="d9bbb-134">Vložte skript předchozí hello do hello **Script.usql** okno.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-134">Paste hello previous script into hello **Script.usql** window.</span></span>

4. <span data-ttu-id="d9bbb-135">V levém horním rohu hello hello **Script.usql** okno, zadejte účet Data Lake Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-135">In hello upper-left corner of hello **Script.usql** window, specify hello Data Lake Analytics account.</span></span>

    ![Odeslání projektu U-SQL sady Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. <span data-ttu-id="d9bbb-137">V levém horním rohu hello hello **Script.usql** vyberte **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-137">In hello upper-left corner of hello **Script.usql** window, select **Submit**.</span></span>
6. <span data-ttu-id="d9bbb-138">Ověřte hello **účet Analytics**a potom vyberte **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-138">Verify hello **Analytics Account**, and then select **Submit**.</span></span> <span data-ttu-id="d9bbb-139">Po dokončení odesílání hello jsou k dispozici v hello nástroje Data Lake pro Visual Studio výsledky odeslání výsledky.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-139">Submission results are available in hello Data Lake Tools for Visual Studio Results after hello submission is complete.</span></span>

    ![Odeslání projektu U-SQL sady Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
7. <span data-ttu-id="d9bbb-141">toosee hello nejnovější úlohy stavu a aktualizace úvodní obrazovka, klikněte na tlačítko **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-141">toosee hello latest job status and refresh hello screen, click **Refresh**.</span></span> <span data-ttu-id="d9bbb-142">Pokud je úloha hello úspěšná, zobrazí hello **graf úlohy**, **operací metadat**, **historie stavů**, a **diagnostiky**:</span><span class="sxs-lookup"><span data-stu-id="d9bbb-142">When hello job succeeds, it shows hello **Job Graph**, **MetaData Operations**, **State History**, and **Diagnostics**:</span></span>

    ![Graf výkonu úlohy U-SQL Visual Studio Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * <span data-ttu-id="d9bbb-144">**Souhrn úlohy** ukazuje hello Souhrn úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-144">**Job Summary** shows hello summary of hello job.</span></span>   
   * <span data-ttu-id="d9bbb-145">**Podrobnosti úlohy** uvádí podrobnější informace o hello úlohy, včetně hello skriptu, prostředky a vrcholy.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-145">**Job Details** shows more specific information about hello job, including hello script, resources, and vertices.</span></span>
   * <span data-ttu-id="d9bbb-146">**Graf úlohy** vizualizuje hello průběh úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-146">**Job Graph** visualizes hello progress of hello job.</span></span>
   * <span data-ttu-id="d9bbb-147">**Operace s metadaty** ukazuje všechny hello akce, které byly provedeny v katalogu hello U-SQL.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-147">**MetaData Operations** shows all hello actions that were taken on hello U-SQL catalog.</span></span>
   * <span data-ttu-id="d9bbb-148">**Data** ukazuje všechny hello vstupy a výstupy.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-148">**Data** shows all hello inputs and outputs.</span></span>
   * <span data-ttu-id="d9bbb-149">**Diagnostika** poskytuje pokročilé analýzy spouštění úlohy a optimalizace výkonu.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-149">**Diagnostics** provides an advanced analysis for job execution and performance optimization.</span></span>

### <a name="toocheck-job-state"></a><span data-ttu-id="d9bbb-150">Stav úlohy toocheck</span><span class="sxs-lookup"><span data-stu-id="d9bbb-150">toocheck job state</span></span>

1. <span data-ttu-id="d9bbb-151">V Průzkumníku serveru vyberte **Azure** > **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-151">In Server Explorer, select **Azure** > **Data Lake Analytics**.</span></span> 
2. <span data-ttu-id="d9bbb-152">Rozbalte název účtu Data Lake Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-152">Expand hello Data Lake Analytics account name.</span></span>
3. <span data-ttu-id="d9bbb-153">Dvakrát klikněte na **Úlohy**.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-153">Double-click **Jobs**.</span></span>
4. <span data-ttu-id="d9bbb-154">Vyberte hello úlohu, která již byla odeslána.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-154">Select hello job that you previously submitted.</span></span>

### <a name="toosee-hello-output-of-a-job"></a><span data-ttu-id="d9bbb-155">toosee hello výstup úlohy</span><span class="sxs-lookup"><span data-stu-id="d9bbb-155">toosee hello output of a job</span></span>

1. <span data-ttu-id="d9bbb-156">V Průzkumníku serveru vyhledejte toohello úlohy, které jste odeslali.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-156">In Server Explorer, browse toohello job you submitted.</span></span>
2. <span data-ttu-id="d9bbb-157">Klikněte na tlačítko hello **Data** kartě.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-157">Click hello **Data** tab.</span></span>
3. <span data-ttu-id="d9bbb-158">V hello **výstupy úlohy** karty, vyberte hello `"/data.csv"` souboru.</span><span class="sxs-lookup"><span data-stu-id="d9bbb-158">In hello **Job Outputs** tab, select hello `"/data.csv"` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9bbb-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9bbb-159">Next steps</span></span>

* [<span data-ttu-id="d9bbb-160">Spouštění skriptů U-SQL na vlastní pracovní stanici za účelem testování a ladění</span><span class="sxs-lookup"><span data-stu-id="d9bbb-160">Run U-SQL scripts on your own workstation for testing and debugging</span></span>](data-lake-analytics-data-lake-tools-local-run.md)
* [<span data-ttu-id="d9bbb-161">Ladění kódu C# v úlohách U-SQL</span><span class="sxs-lookup"><span data-stu-id="d9bbb-161">Debug C# code in U-SQL jobs</span></span>](data-lake-analytics-debug-u-sql-jobs.md)
* [<span data-ttu-id="d9bbb-162">Pomocí nástrojů hello Azure Data Lake pro Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d9bbb-162">Use hello Azure Data Lake Tools for Visual Studio Code</span></span>](data-lake-analytics-data-lake-tools-for-vscode.md)
