---
title: "aaaTest a ladění úloh U-SQL pomocí místního spuštění a hello Azure Data Lake U-SQL SDK | Microsoft Docs"
description: "Zjistěte, jak úlohy nástroje toouse Azure Data Lake pro Visual Studio a tootest hello SDK Azure Data Lake U-SQL a ladění U-SQL na místní pracovní stanici."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/15/2016
ms.author: yanacai
ms.openlocfilehash: be04558a504acf6a088e207608ee2d4a011d3ffc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="test-and-debug-u-sql-jobs-by-using-local-run-and-hello-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="08114-103">Testování a ladění úloh U-SQL pomocí místního spuštění a hello Azure Data Lake U-SQL SDK</span><span class="sxs-lookup"><span data-stu-id="08114-103">Test and debug U-SQL jobs by using local run and hello Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="08114-104">Nástroje služby Azure Data Lake pro Visual Studio a úloh U-SQL toorun hello SDK Azure Data Lake U-SQL můžete použít na pracovní stanici, stejně jako ve službě Azure Data Lake hello.</span><span class="sxs-lookup"><span data-stu-id="08114-104">You can use Azure Data Lake Tools for Visual Studio and hello Azure Data Lake U-SQL SDK toorun U-SQL jobs on your workstation, just as you can in hello Azure Data Lake service.</span></span> <span data-ttu-id="08114-105">Tyto dvě místně spouštěné funkce vám šetří čas při testování a ladění úloh U-SQL.</span><span class="sxs-lookup"><span data-stu-id="08114-105">These two local-run features save you time in testing and debugging your U-SQL jobs.</span></span>

## <a name="understand-hello-data-root-folder-and-hello-file-path"></a><span data-ttu-id="08114-106">Pochopení hello kořenové datové složce a cesta k souboru hello</span><span class="sxs-lookup"><span data-stu-id="08114-106">Understand hello data-root folder and hello file path</span></span>

<span data-ttu-id="08114-107">Místní spuštění a hello U-SQL sady SDK vyžadovat kořenové datové složce.</span><span class="sxs-lookup"><span data-stu-id="08114-107">Both local run and hello U-SQL SDK require a data-root folder.</span></span> <span data-ttu-id="08114-108">Hello kořenové datové složce je pro účet místního výpočetní hello "místní úložiště".</span><span class="sxs-lookup"><span data-stu-id="08114-108">hello data-root folder is a "local store" for hello local compute account.</span></span> <span data-ttu-id="08114-109">Je ekvivalentní toohello účtu Azure Data Lake Store účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="08114-109">It's equivalent toohello Azure Data Lake Store account of a Data Lake Analytics account.</span></span> <span data-ttu-id="08114-110">Přepínání tooa různé kořenové datové složce je stejně jako přepínání tooa úložiště jiný účet.</span><span class="sxs-lookup"><span data-stu-id="08114-110">Switching tooa different data-root folder is just like switching tooa different store account.</span></span> <span data-ttu-id="08114-111">Pokud budete chtít tooaccess běžně sdílí data pomocí různých dat kořenové složky, je nutné použít absolutní cesty ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="08114-111">If you want tooaccess commonly shared data with different data-root folders, you must use absolute paths in your scripts.</span></span> <span data-ttu-id="08114-112">Nebo vytvořte symbolické odkazy systému souborů (například **mklink** na systém souborů NTFS) v části hello kořenové datové složce toopoint toohello sdílená data.</span><span class="sxs-lookup"><span data-stu-id="08114-112">Or, create file system symbolic links (for example, **mklink** on NTFS) under hello data-root folder toopoint toohello shared data.</span></span>

<span data-ttu-id="08114-113">Hello kořenové datové složce se používá pro:</span><span class="sxs-lookup"><span data-stu-id="08114-113">hello data-root folder is used to:</span></span>

- <span data-ttu-id="08114-114">Ukládání metadat, včetně databází, tabulky, funkce vracející tabulku (Tvf) a sestavení.</span><span class="sxs-lookup"><span data-stu-id="08114-114">Store metadata, including databases, tables, table-valued functions (TVFs), and assemblies.</span></span>
- <span data-ttu-id="08114-115">Vyhledání hello vstupní a výstupní cesty, které jsou definovány jako relativní cesty v U-SQL.</span><span class="sxs-lookup"><span data-stu-id="08114-115">Look up hello input and output paths that are defined as relative paths in U-SQL.</span></span> <span data-ttu-id="08114-116">Pomocí relativní cesty umožňuje snazší toodeploy vaše projekty tooAzure U-SQL.</span><span class="sxs-lookup"><span data-stu-id="08114-116">Using relative paths makes it easier toodeploy your U-SQL projects tooAzure.</span></span>

<span data-ttu-id="08114-117">Můžete je relativní cesta a místní cestou absolutní v skriptů U-SQL.</span><span class="sxs-lookup"><span data-stu-id="08114-117">You can use both a relative path and a local absolute path in U-SQL scripts.</span></span> <span data-ttu-id="08114-118">relativní cesta Hello je cesta ke složce relativní toohello zadaný kořenový data.</span><span class="sxs-lookup"><span data-stu-id="08114-118">hello relative path is relative toohello specified data-root folder path.</span></span> <span data-ttu-id="08114-119">Doporučujeme vám, že můžete použít "/" jako hello cesta oddělovače toomake skripty kompatibilní s hello na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="08114-119">We recommend that you use "/" as hello path separator toomake your scripts compatible with hello server side.</span></span> <span data-ttu-id="08114-120">Zde jsou některé příklady relativní cesty a jejich ekvivalent absolutní cesty.</span><span class="sxs-lookup"><span data-stu-id="08114-120">Here are some examples of relative paths and their equivalent absolute paths.</span></span> <span data-ttu-id="08114-121">V těchto příkladech je C:\LocalRunDataRoot hello kořenové datové složce.</span><span class="sxs-lookup"><span data-stu-id="08114-121">In these examples, C:\LocalRunDataRoot is hello data-root folder.</span></span>

|<span data-ttu-id="08114-122">Relativní cesta</span><span class="sxs-lookup"><span data-stu-id="08114-122">Relative path</span></span>|<span data-ttu-id="08114-123">Absolutní cesty</span><span class="sxs-lookup"><span data-stu-id="08114-123">Absolute path</span></span>|
|-------------|-------------|
|<span data-ttu-id="08114-124">/ABC/DEF/Input.csv</span><span class="sxs-lookup"><span data-stu-id="08114-124">/abc/def/input.csv</span></span> |<span data-ttu-id="08114-125">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="08114-125">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="08114-126">ABC/DEF/Input.csv</span><span class="sxs-lookup"><span data-stu-id="08114-126">abc/def/input.csv</span></span>  |<span data-ttu-id="08114-127">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="08114-127">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="08114-128">D:/ABC/DEF/Input.csv</span><span class="sxs-lookup"><span data-stu-id="08114-128">D:/abc/def/input.csv</span></span> |<span data-ttu-id="08114-129">D:\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="08114-129">D:\abc\def\input.csv</span></span>|

## <a name="use-local-run-from-visual-studio"></a><span data-ttu-id="08114-130">Použití místního spuštění ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08114-130">Use local run from Visual Studio</span></span>

<span data-ttu-id="08114-131">Nástroje data Lake pro Visual Studio poskytuje možnosti místní spuštění U-SQL v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="08114-131">Data Lake Tools for Visual Studio provides a U-SQL local-run experience in Visual Studio.</span></span> <span data-ttu-id="08114-132">Pomocí této funkce můžete:</span><span class="sxs-lookup"><span data-stu-id="08114-132">By using this feature, you can:</span></span>

- <span data-ttu-id="08114-133">Spusťte skript U-SQL, který je místně, společně s sestavení C#.</span><span class="sxs-lookup"><span data-stu-id="08114-133">Run a U-SQL script locally, along with C# assemblies.</span></span>
- <span data-ttu-id="08114-134">Ladění sestavení C# místně.</span><span class="sxs-lookup"><span data-stu-id="08114-134">Debug a C# assembly locally.</span></span>
- <span data-ttu-id="08114-135">Vytvořit, zobrazit a odstranění katalogů U-SQL (místních databází, sestavení, schémat a tabulek) z Průzkumníka serveru.</span><span class="sxs-lookup"><span data-stu-id="08114-135">Create, view, and delete U-SQL catalogs (local databases, assemblies, schemas, and tables) from Server Explorer.</span></span> <span data-ttu-id="08114-136">Můžete také získat místní katalog hello také z Průzkumníka serveru.</span><span class="sxs-lookup"><span data-stu-id="08114-136">You can also find hello local catalog also from Server Explorer.</span></span>

    ![Nástroje data Lake pro Visual Studio místní spuštění místního katalogu](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-local-catalog.png)

<span data-ttu-id="08114-138">Instalační program nástroje Data Lake Hello vytvoří toobe složky C:\LocalRunRoot použít jako hello výchozí kořenové datové složce.</span><span class="sxs-lookup"><span data-stu-id="08114-138">hello Data Lake Tools installer creates a C:\LocalRunRoot folder toobe used as hello default data-root folder.</span></span> <span data-ttu-id="08114-139">paralelismus místní spuštění Hello výchozí je 1.</span><span class="sxs-lookup"><span data-stu-id="08114-139">hello default local-run parallelism is 1.</span></span>

### <a name="tooconfigure-local-run-in-visual-studio"></a><span data-ttu-id="08114-140">tooconfigure místní spuštění v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08114-140">tooconfigure local run in Visual Studio</span></span>

1. <span data-ttu-id="08114-141">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="08114-141">Open Visual Studio.</span></span>
2. <span data-ttu-id="08114-142">Otevřete **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="08114-142">Open **Server Explorer**.</span></span>
3. <span data-ttu-id="08114-143">Rozbalte položku **Azure** > **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="08114-143">Expand **Azure** > **Data Lake Analytics**.</span></span>
4. <span data-ttu-id="08114-144">Klikněte na tlačítko hello **Data Lake** nabídce a pak klikněte na tlačítko **možnosti a nastavení**.</span><span class="sxs-lookup"><span data-stu-id="08114-144">Click hello **Data Lake** menu, and then click **Options and Settings**.</span></span>
5. <span data-ttu-id="08114-145">Rozbalte ve stromu levém hello **Azure Data Lake**a potom rozbalte **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="08114-145">In hello left tree, expand **Azure Data Lake**, and then expand **General**.</span></span>

    ![Konfigurace nástrojů data Lake pro Visual Studio spustit místní nastavení](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-configure.png)

<span data-ttu-id="08114-147">Projekt Visual Studio U-SQL je vyžadována pro provádění místní spuštění.</span><span class="sxs-lookup"><span data-stu-id="08114-147">A Visual Studio U-SQL project is required for performing local run.</span></span> <span data-ttu-id="08114-148">Tato část se liší od spuštění skriptů U-SQL z Azure.</span><span class="sxs-lookup"><span data-stu-id="08114-148">This part is different from running U-SQL scripts from Azure.</span></span>

### <a name="toorun-a-u-sql-script-locally"></a><span data-ttu-id="08114-149">skript U-SQL toorun místně</span><span class="sxs-lookup"><span data-stu-id="08114-149">toorun a U-SQL script locally</span></span>
1. <span data-ttu-id="08114-150">Ze sady Visual Studio otevřete projekt U-SQL.</span><span class="sxs-lookup"><span data-stu-id="08114-150">From Visual Studio, open your U-SQL project.</span></span>   
2. <span data-ttu-id="08114-151">Skript U-SQL v Průzkumníku řešení klikněte pravým tlačítkem myši a pak klikněte na **odeslat skript**.</span><span class="sxs-lookup"><span data-stu-id="08114-151">Right-click a U-SQL script in Solution Explorer, and then click **Submit Script**.</span></span>
3. <span data-ttu-id="08114-152">Vyberte **(místní)** jako hello účtu Analytics toorun váš skript místně.</span><span class="sxs-lookup"><span data-stu-id="08114-152">Select **(Local)** as hello Analytics account toorun your script locally.</span></span>
<span data-ttu-id="08114-153">Můžete také kliknout na hello **(místní)** účet na hello horní části okna skript a potom klikněte na **odeslání** (nebo použijte hello Ctrl + F5 klávesové zkratky).</span><span class="sxs-lookup"><span data-stu-id="08114-153">You can also click hello **(Local)** account on hello top of script window, and then click **Submit** (or use hello Ctrl + F5 keyboard shortcut).</span></span>

    ![Nástroje data Lake pro Visual Studio spustit místní odeslání úlohy](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-submit-job.png)

### <a name="debug-scripts-and-c-assemblies-locally"></a><span data-ttu-id="08114-155">Místní ladění skriptů a sestavení C#</span><span class="sxs-lookup"><span data-stu-id="08114-155">Debug scripts and C# assemblies locally</span></span>

<span data-ttu-id="08114-156">Sestavení C# můžete ladit bez odeslali a zaregistrovali tooAzure služba Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="08114-156">You can debug C# assemblies without submitting and registering it tooAzure Data Lake Analytics Service.</span></span> <span data-ttu-id="08114-157">V obou hello souboru kódu i v odkazovaném projektu C# můžete nastavit zarážky.</span><span class="sxs-lookup"><span data-stu-id="08114-157">You can set breakpoints in both hello code behind file and in a referenced C# project.</span></span>

#### <a name="toodebug-local-code-in-code-behind-file"></a><span data-ttu-id="08114-158">toodebug místního kódu v souboru kódu</span><span class="sxs-lookup"><span data-stu-id="08114-158">toodebug local code in code behind file</span></span>

1. <span data-ttu-id="08114-159">Nastavte zarážky v souboru kódu hello.</span><span class="sxs-lookup"><span data-stu-id="08114-159">Set breakpoints in hello code behind file.</span></span>
2. <span data-ttu-id="08114-160">Stisknutím klávesy F5 toodebug hello skript místně.</span><span class="sxs-lookup"><span data-stu-id="08114-160">Press F5 toodebug hello script locally.</span></span>

> [!NOTE]
   > <span data-ttu-id="08114-161">Následující postup lze použít pouze v sadě Visual Studio 2015 Hello.</span><span class="sxs-lookup"><span data-stu-id="08114-161">hello following procedure only works in Visual Studio 2015.</span></span> <span data-ttu-id="08114-162">Ve starší sadě Visual Studio může potřebujete toomanually přidat soubory pdb hello.</span><span class="sxs-lookup"><span data-stu-id="08114-162">In older Visual Studio you may need toomanually add hello pdb files.</span></span>  
   >
   >

#### <a name="toodebug-local-code-in-a-referenced-c-project"></a><span data-ttu-id="08114-163">toodebug místního kódu v odkazovaném projektu C#</span><span class="sxs-lookup"><span data-stu-id="08114-163">toodebug local code in a referenced C# project</span></span>

1. <span data-ttu-id="08114-164">Vytvořte projekt sestavení C# a sestavte jej toogenerate hello výstupní knihovnu dll.</span><span class="sxs-lookup"><span data-stu-id="08114-164">Create a C# Assembly project, and build it toogenerate hello output dll.</span></span>
2. <span data-ttu-id="08114-165">Zaregistrujte hello knihovny dll pomocí příkazu U-SQL:</span><span class="sxs-lookup"><span data-stu-id="08114-165">Register hello dll using a U-SQL statement:</span></span>

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. <span data-ttu-id="08114-166">Nastavte zarážky v hello kód C#.</span><span class="sxs-lookup"><span data-stu-id="08114-166">Set breakpoints in hello C# code.</span></span>
4. <span data-ttu-id="08114-167">Stisknutím klávesy F5 toodebug hello skriptu s odkazujícím hello C# knihovnu dll.</span><span class="sxs-lookup"><span data-stu-id="08114-167">Press F5 toodebug hello script with referencing hello C# dll locally.</span></span>

## <a name="use-local-run-from-hello-data-lake-u-sql-sdk"></a><span data-ttu-id="08114-168">Použití místního spuštění z hello Data Lake U-SQL SDK</span><span class="sxs-lookup"><span data-stu-id="08114-168">Use local run from hello Data Lake U-SQL SDK</span></span>

<span data-ttu-id="08114-169">Kromě toorunning U-SQL skriptů místně pomocí sady Visual Studio, můžete použít skripty U-SQL toorun SDK Azure Data Lake U-SQL hello místně pomocí příkazového řádku a programovací rozhraní.</span><span class="sxs-lookup"><span data-stu-id="08114-169">In addition toorunning U-SQL scripts locally by using Visual Studio, you can use hello Azure Data Lake U-SQL SDK toorun U-SQL scripts locally with command-line and programming interfaces.</span></span> <span data-ttu-id="08114-170">Pomocí těchto je možné škálovat svůj místní test U-SQL.</span><span class="sxs-lookup"><span data-stu-id="08114-170">Through these, you can scale your U-SQL local test.</span></span>

<span data-ttu-id="08114-171">Další informace o [SDK Azure Data Lake U-SQL](data-lake-analytics-u-sql-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="08114-171">Learn more about [Azure Data Lake U-SQL SDK](data-lake-analytics-u-sql-sdk.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="08114-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="08114-172">Next steps</span></span>

* <span data-ttu-id="08114-173">toosee komplexnější dotaz, najdete v části [analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="08114-173">toosee a more complex query, see [Analyze website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="08114-174">Podrobnosti úlohy tooview, najdete v tématu [použití úlohy prohlížeče a zobrazení úloh pro úlohy Azure Data Lake Analytics](data-lake-analytics-data-lake-tools-view-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="08114-174">tooview job details, see [Use Job Browser and Job View for Azure Data Lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md).</span></span>
* <span data-ttu-id="08114-175">zobrazení provádění vrcholů toouse hello, najdete v části [použití hello nebo zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span><span class="sxs-lookup"><span data-stu-id="08114-175">toouse hello vertex execution view, see [Use hello Vertex Execution View in Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span></span>
