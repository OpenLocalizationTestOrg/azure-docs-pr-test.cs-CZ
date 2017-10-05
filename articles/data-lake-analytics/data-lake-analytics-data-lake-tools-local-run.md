---
title: "Testování a ladění úloh U-SQL pomocí místní spuštění a sadu SDK Azure Data Lake U-SQL | Microsoft Docs"
description: "Další informace o použití nástroje Azure Data Lake pro Visual Studio a sadu SDK Azure Data Lake U-SQL pro testování a ladění úloh U-SQL na místní pracovní stanici."
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
ms.openlocfilehash: 771a96df5cc66bac46e7144785be8cc072b57b31
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="test-and-debug-u-sql-jobs-by-using-local-run-and-the-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="0258e-103">Testování a ladění úloh U-SQL pomocí místního spuštění a sadu SDK Azure Data Lake U-SQL</span><span class="sxs-lookup"><span data-stu-id="0258e-103">Test and debug U-SQL jobs by using local run and the Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="0258e-104">Nástroje Azure Data Lake pro Visual Studio a sadu Azure Data Lake U-SQL SDK můžete použít k místnímu spouštění úloh U-SQL na pracovní stanici, stejně jako byste je spouštěli ve službě Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="0258e-104">You can use Azure Data Lake Tools for Visual Studio and the Azure Data Lake U-SQL SDK to run U-SQL jobs on your workstation, just as you can in the Azure Data Lake service.</span></span> <span data-ttu-id="0258e-105">Tyto dvě místně spouštěné funkce vám šetří čas při testování a ladění úloh U-SQL.</span><span class="sxs-lookup"><span data-stu-id="0258e-105">These two local-run features save you time in testing and debugging your U-SQL jobs.</span></span>

## <a name="understand-the-data-root-folder-and-the-file-path"></a><span data-ttu-id="0258e-106">Pochopení dat kořenové složky a cesta k souboru</span><span class="sxs-lookup"><span data-stu-id="0258e-106">Understand the data-root folder and the file path</span></span>

<span data-ttu-id="0258e-107">Místní spuštění i U-SQL sady SDK vyžadovat kořenové datové složce.</span><span class="sxs-lookup"><span data-stu-id="0258e-107">Both local run and the U-SQL SDK require a data-root folder.</span></span> <span data-ttu-id="0258e-108">Data kořenové složky je pro účet místního výpočetní "místní úložiště".</span><span class="sxs-lookup"><span data-stu-id="0258e-108">The data-root folder is a "local store" for the local compute account.</span></span> <span data-ttu-id="0258e-109">Je ekvivalentní k účtu Azure Data Lake Store účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="0258e-109">It's equivalent to the Azure Data Lake Store account of a Data Lake Analytics account.</span></span> <span data-ttu-id="0258e-110">Přepnutí na jiný data kořenová složka je stejně jako přepnutí na účet jiné úložiště.</span><span class="sxs-lookup"><span data-stu-id="0258e-110">Switching to a different data-root folder is just like switching to a different store account.</span></span> <span data-ttu-id="0258e-111">Pokud chcete pro přístup k běžně sdílený data pomocí různých dat kořenové složky, je nutné použít absolutní cesty ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="0258e-111">If you want to access commonly shared data with different data-root folders, you must use absolute paths in your scripts.</span></span> <span data-ttu-id="0258e-112">Nebo vytvořte symbolické odkazy systému souborů (například **mklink** na systém souborů NTFS) v kořenové datové složce tak, aby odkazoval sdílená data.</span><span class="sxs-lookup"><span data-stu-id="0258e-112">Or, create file system symbolic links (for example, **mklink** on NTFS) under the data-root folder to point to the shared data.</span></span>

<span data-ttu-id="0258e-113">Data kořenové složky se používá pro:</span><span class="sxs-lookup"><span data-stu-id="0258e-113">The data-root folder is used to:</span></span>

- <span data-ttu-id="0258e-114">Ukládání metadat, včetně databází, tabulky, funkce vracející tabulku (Tvf) a sestavení.</span><span class="sxs-lookup"><span data-stu-id="0258e-114">Store metadata, including databases, tables, table-valued functions (TVFs), and assemblies.</span></span>
- <span data-ttu-id="0258e-115">Vyhledání vstupní a výstupní cesty, které jsou definovány jako relativní cesty v U-SQL.</span><span class="sxs-lookup"><span data-stu-id="0258e-115">Look up the input and output paths that are defined as relative paths in U-SQL.</span></span> <span data-ttu-id="0258e-116">Pomocí relativní cesty usnadňuje nasazení vašich projektů U-SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="0258e-116">Using relative paths makes it easier to deploy your U-SQL projects to Azure.</span></span>

<span data-ttu-id="0258e-117">Můžete je relativní cesta a místní cestou absolutní v skriptů U-SQL.</span><span class="sxs-lookup"><span data-stu-id="0258e-117">You can use both a relative path and a local absolute path in U-SQL scripts.</span></span> <span data-ttu-id="0258e-118">Relativní cesta je relativní k cestě zadané kořenové datové složce.</span><span class="sxs-lookup"><span data-stu-id="0258e-118">The relative path is relative to the specified data-root folder path.</span></span> <span data-ttu-id="0258e-119">Doporučujeme vám, že používáte "/" jako oddělovač cesty upravit skripty kompatibilní se na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="0258e-119">We recommend that you use "/" as the path separator to make your scripts compatible with the server side.</span></span> <span data-ttu-id="0258e-120">Zde jsou některé příklady relativní cesty a jejich ekvivalent absolutní cesty.</span><span class="sxs-lookup"><span data-stu-id="0258e-120">Here are some examples of relative paths and their equivalent absolute paths.</span></span> <span data-ttu-id="0258e-121">V těchto příkladech je C:\LocalRunDataRoot kořenové datové složce.</span><span class="sxs-lookup"><span data-stu-id="0258e-121">In these examples, C:\LocalRunDataRoot is the data-root folder.</span></span>

|<span data-ttu-id="0258e-122">Relativní cesta</span><span class="sxs-lookup"><span data-stu-id="0258e-122">Relative path</span></span>|<span data-ttu-id="0258e-123">Absolutní cesty</span><span class="sxs-lookup"><span data-stu-id="0258e-123">Absolute path</span></span>|
|-------------|-------------|
|<span data-ttu-id="0258e-124">/ABC/DEF/Input.csv</span><span class="sxs-lookup"><span data-stu-id="0258e-124">/abc/def/input.csv</span></span> |<span data-ttu-id="0258e-125">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="0258e-125">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="0258e-126">ABC/DEF/Input.csv</span><span class="sxs-lookup"><span data-stu-id="0258e-126">abc/def/input.csv</span></span>  |<span data-ttu-id="0258e-127">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="0258e-127">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="0258e-128">D:/ABC/DEF/Input.csv</span><span class="sxs-lookup"><span data-stu-id="0258e-128">D:/abc/def/input.csv</span></span> |<span data-ttu-id="0258e-129">D:\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="0258e-129">D:\abc\def\input.csv</span></span>|

## <a name="use-local-run-from-visual-studio"></a><span data-ttu-id="0258e-130">Použití místního spuštění ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0258e-130">Use local run from Visual Studio</span></span>

<span data-ttu-id="0258e-131">Nástroje data Lake pro Visual Studio poskytuje možnosti místní spuštění U-SQL v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0258e-131">Data Lake Tools for Visual Studio provides a U-SQL local-run experience in Visual Studio.</span></span> <span data-ttu-id="0258e-132">Pomocí této funkce můžete:</span><span class="sxs-lookup"><span data-stu-id="0258e-132">By using this feature, you can:</span></span>

- <span data-ttu-id="0258e-133">Spusťte skript U-SQL, který je místně, společně s sestavení C#.</span><span class="sxs-lookup"><span data-stu-id="0258e-133">Run a U-SQL script locally, along with C# assemblies.</span></span>
- <span data-ttu-id="0258e-134">Ladění sestavení C# místně.</span><span class="sxs-lookup"><span data-stu-id="0258e-134">Debug a C# assembly locally.</span></span>
- <span data-ttu-id="0258e-135">Vytvořit, zobrazit a odstranění katalogů U-SQL (místních databází, sestavení, schémat a tabulek) z Průzkumníka serveru.</span><span class="sxs-lookup"><span data-stu-id="0258e-135">Create, view, and delete U-SQL catalogs (local databases, assemblies, schemas, and tables) from Server Explorer.</span></span> <span data-ttu-id="0258e-136">Můžete také získat místní katalog také z Průzkumníka serveru.</span><span class="sxs-lookup"><span data-stu-id="0258e-136">You can also find the local catalog also from Server Explorer.</span></span>

    ![Nástroje data Lake pro Visual Studio místní spuštění místního katalogu](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-local-catalog.png)

<span data-ttu-id="0258e-138">Instalační program nástroje Data Lake vytvoří složku C:\LocalRunRoot má být použit jako výchozí kořenové datové složce.</span><span class="sxs-lookup"><span data-stu-id="0258e-138">The Data Lake Tools installer creates a C:\LocalRunRoot folder to be used as the default data-root folder.</span></span> <span data-ttu-id="0258e-139">Místní spuštění paralelismus výchozí je 1.</span><span class="sxs-lookup"><span data-stu-id="0258e-139">The default local-run parallelism is 1.</span></span>

### <a name="to-configure-local-run-in-visual-studio"></a><span data-ttu-id="0258e-140">Konfigurace místního spuštění v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0258e-140">To configure local run in Visual Studio</span></span>

1. <span data-ttu-id="0258e-141">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0258e-141">Open Visual Studio.</span></span>
2. <span data-ttu-id="0258e-142">Otevřete **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="0258e-142">Open **Server Explorer**.</span></span>
3. <span data-ttu-id="0258e-143">Rozbalte položku **Azure** > **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="0258e-143">Expand **Azure** > **Data Lake Analytics**.</span></span>
4. <span data-ttu-id="0258e-144">Klikněte **Data Lake** nabídce a pak klikněte na tlačítko **možnosti a nastavení**.</span><span class="sxs-lookup"><span data-stu-id="0258e-144">Click the **Data Lake** menu, and then click **Options and Settings**.</span></span>
5. <span data-ttu-id="0258e-145">Ve stromu vlevo rozbalte **Azure Data Lake**a potom rozbalte **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="0258e-145">In the left tree, expand **Azure Data Lake**, and then expand **General**.</span></span>

    ![Konfigurace nástrojů data Lake pro Visual Studio spustit místní nastavení](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-configure.png)

<span data-ttu-id="0258e-147">Projekt Visual Studio U-SQL je vyžadována pro provádění místní spuštění.</span><span class="sxs-lookup"><span data-stu-id="0258e-147">A Visual Studio U-SQL project is required for performing local run.</span></span> <span data-ttu-id="0258e-148">Tato část se liší od spuštění skriptů U-SQL z Azure.</span><span class="sxs-lookup"><span data-stu-id="0258e-148">This part is different from running U-SQL scripts from Azure.</span></span>

### <a name="to-run-a-u-sql-script-locally"></a><span data-ttu-id="0258e-149">Místně spustíte skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="0258e-149">To run a U-SQL script locally</span></span>
1. <span data-ttu-id="0258e-150">Ze sady Visual Studio otevřete projekt U-SQL.</span><span class="sxs-lookup"><span data-stu-id="0258e-150">From Visual Studio, open your U-SQL project.</span></span>   
2. <span data-ttu-id="0258e-151">Skript U-SQL v Průzkumníku řešení klikněte pravým tlačítkem myši a pak klikněte na **odeslat skript**.</span><span class="sxs-lookup"><span data-stu-id="0258e-151">Right-click a U-SQL script in Solution Explorer, and then click **Submit Script**.</span></span>
3. <span data-ttu-id="0258e-152">Vyberte **(místní)** jako účet Analytics, který chcete spustit skript místně.</span><span class="sxs-lookup"><span data-stu-id="0258e-152">Select **(Local)** as the Analytics account to run your script locally.</span></span>
<span data-ttu-id="0258e-153">Můžete také kliknutím **(místní)** účet horní okně Skript a potom klikněte na **odeslání** (nebo použít kombinaci kláves Ctrl + F5 klávesové zkratky).</span><span class="sxs-lookup"><span data-stu-id="0258e-153">You can also click the **(Local)** account on the top of script window, and then click **Submit** (or use the Ctrl + F5 keyboard shortcut).</span></span>

    ![Nástroje data Lake pro Visual Studio spustit místní odeslání úlohy](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-submit-job.png)

### <a name="debug-scripts-and-c-assemblies-locally"></a><span data-ttu-id="0258e-155">Místní ladění skriptů a sestavení C#</span><span class="sxs-lookup"><span data-stu-id="0258e-155">Debug scripts and C# assemblies locally</span></span>

<span data-ttu-id="0258e-156">Sestavení C# můžete ladit bez odeslali a zaregistrovali k službě Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="0258e-156">You can debug C# assemblies without submitting and registering it to Azure Data Lake Analytics Service.</span></span> <span data-ttu-id="0258e-157">V souboru kódu i v odkazovaném projektu C# můžete nastavit zarážky.</span><span class="sxs-lookup"><span data-stu-id="0258e-157">You can set breakpoints in both the code behind file and in a referenced C# project.</span></span>

#### <a name="to-debug-local-code-in-code-behind-file"></a><span data-ttu-id="0258e-158">Postup ladění místního kódu v souboru kódu</span><span class="sxs-lookup"><span data-stu-id="0258e-158">To debug local code in code behind file</span></span>

1. <span data-ttu-id="0258e-159">Nastavte zarážky v souboru kódu.</span><span class="sxs-lookup"><span data-stu-id="0258e-159">Set breakpoints in the code behind file.</span></span>
2. <span data-ttu-id="0258e-160">Stisknutím klávesy F5 místně laďte skript.</span><span class="sxs-lookup"><span data-stu-id="0258e-160">Press F5 to debug the script locally.</span></span>

> [!NOTE]
   > <span data-ttu-id="0258e-161">Následující postup funguje pouze v sadě Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="0258e-161">The following procedure only works in Visual Studio 2015.</span></span> <span data-ttu-id="0258e-162">Ve starší sadě Visual Studio je pravděpodobně nutné ručně přidat soubory PDB.</span><span class="sxs-lookup"><span data-stu-id="0258e-162">In older Visual Studio you may need to manually add the pdb files.</span></span>  
   >
   >

#### <a name="to-debug-local-code-in-a-referenced-c-project"></a><span data-ttu-id="0258e-163">Postup ladění místního kódu v odkazovaném projektu C#</span><span class="sxs-lookup"><span data-stu-id="0258e-163">To debug local code in a referenced C# project</span></span>

1. <span data-ttu-id="0258e-164">Vytvořte projekt sestavení C# a sestavte jej tak, aby generoval výstupní knihovnu DLL.</span><span class="sxs-lookup"><span data-stu-id="0258e-164">Create a C# Assembly project, and build it to generate the output dll.</span></span>
2. <span data-ttu-id="0258e-165">Zaregistruje knihovnu DLL pomocí příkazu U-SQL:</span><span class="sxs-lookup"><span data-stu-id="0258e-165">Register the dll using a U-SQL statement:</span></span>

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. <span data-ttu-id="0258e-166">Nastavte zarážky v kódu C#.</span><span class="sxs-lookup"><span data-stu-id="0258e-166">Set breakpoints in the C# code.</span></span>
4. <span data-ttu-id="0258e-167">Stisknutím klávesy F5 a laďte skript s odkazujícím C# knihovny dll místně.</span><span class="sxs-lookup"><span data-stu-id="0258e-167">Press F5 to debug the script with referencing the C# dll locally.</span></span>

## <a name="use-local-run-from-the-data-lake-u-sql-sdk"></a><span data-ttu-id="0258e-168">Použití místní spuštění ze sady SDK pro Data Lake U-SQL</span><span class="sxs-lookup"><span data-stu-id="0258e-168">Use local run from the Data Lake U-SQL SDK</span></span>

<span data-ttu-id="0258e-169">Kromě spuštění skriptů U-SQL místně pomocí sady Visual Studio, můžete použít sadu SDK Azure Data Lake U-SQL ke spouštění skriptů U-SQL místně pomocí příkazového řádku a programovací rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0258e-169">In addition to running U-SQL scripts locally by using Visual Studio, you can use the Azure Data Lake U-SQL SDK to run U-SQL scripts locally with command-line and programming interfaces.</span></span> <span data-ttu-id="0258e-170">Pomocí těchto je možné škálovat svůj místní test U-SQL.</span><span class="sxs-lookup"><span data-stu-id="0258e-170">Through these, you can scale your U-SQL local test.</span></span>

<span data-ttu-id="0258e-171">Další informace o [SDK Azure Data Lake U-SQL](data-lake-analytics-u-sql-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="0258e-171">Learn more about [Azure Data Lake U-SQL SDK](data-lake-analytics-u-sql-sdk.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="0258e-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0258e-172">Next steps</span></span>

* <span data-ttu-id="0258e-173">Pokud chcete zobrazit komplexnější dotaz, najdete v části [analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="0258e-173">To see a more complex query, see [Analyze website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="0258e-174">Chcete-li zobrazit podrobnosti o úlohách, najdete v části [použití úlohy prohlížeče a zobrazení úloh pro úlohy Azure Data Lake Analytics](data-lake-analytics-data-lake-tools-view-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="0258e-174">To view job details, see [Use Job Browser and Job View for Azure Data Lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md).</span></span>
* <span data-ttu-id="0258e-175">Chcete-li použít zobrazení provádění vrcholů, přečtěte si téma [použít zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span><span class="sxs-lookup"><span data-stu-id="0258e-175">To use the vertex execution view, see [Use the Vertex Execution View in Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span></span>
