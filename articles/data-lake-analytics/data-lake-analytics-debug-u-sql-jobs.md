---
title: "Ladění úloh U-SQL | Microsoft Docs"
description: "Zjistěte, jak k ladění selhání vrchol U-SQL pomocí sady Visual Studio."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/02/2016
ms.author: saveenr
ms.openlocfilehash: 2a77c72d3062272305208934d6406d040266c753
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a><span data-ttu-id="09b0e-103">Ladění uživatelem definované C# – kód pro selhání úloh U-SQL</span><span class="sxs-lookup"><span data-stu-id="09b0e-103">Debug user-defined C# code for failed U-SQL jobs</span></span>

<span data-ttu-id="09b0e-104">U-SQL poskytuje model rozšiřitelnosti pomocí jazyka C#, takže můžete napsat kód přidat další funkce, jako jsou vlastní Extraktor nebo reduktorem.</span><span class="sxs-lookup"><span data-stu-id="09b0e-104">U-SQL provides an extensibility model using C#, so you can write your code to add functionality such as a custom extractor or reducer.</span></span> <span data-ttu-id="09b0e-105">Další informace najdete v tématu [U-SQL programovatelnosti průvodce](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf).</span><span class="sxs-lookup"><span data-stu-id="09b0e-105">To learn more, see [U-SQL programmability guide](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf).</span></span> <span data-ttu-id="09b0e-106">V praxi může potřebovat žádný kód, ladění a systémy velkých objemů dat může poskytnout jenom omezené runtime ladění informace, jako jsou soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="09b0e-106">In practice any code may need debugging, and big data systems may only provide limited runtime debugging information such as log files.</span></span>

<span data-ttu-id="09b0e-107">Azure nástrojů Data Lake pro Visual Studio poskytuje funkci **se nezdařilo ladění vrchol**, která vám umožňuje klonovat neúspěšnou úlohu z cloudu do místního počítače pro ladění.</span><span class="sxs-lookup"><span data-stu-id="09b0e-107">Azure Data Lake Tools for Visual Studio provides a feature called **Failed Vertex Debug**, which lets you clone a failed job from the cloud to your local machine for debugging.</span></span> <span data-ttu-id="09b0e-108">Místní klonu zaznamená celý cloudové prostředí, včetně všech vstupních dat a uživatelského kódu.</span><span class="sxs-lookup"><span data-stu-id="09b0e-108">The local clone captures the entire cloud environment, including any input data and user code.</span></span>

<span data-ttu-id="09b0e-109">Toto video ukazuje se nezdařilo vrchol ladění ve nástrojů Azure Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="09b0e-109">The following video demonstrates Failed Vertex Debug in Azure Data Lake Tools for Visual Studio.</span></span>

> [!VIDEO https://e0d1.wpc.azureedge.net/80E0D1/OfficeMixProdMediaBlobStorage/asset-d3aeab42-6149-4ecc-b044-aa624901ab32/b0fc0373c8f94f1bb8cd39da1310adb8.mp4?sv=2012-02-12&sr=c&si=a91fad76-cfdd-4513-9668-483de39e739c&sig=K%2FR%2FdnIi9S6P%2FBlB3iLAEV5pYu6OJFBDlQy%2FQtZ7E7M%3D&se=2116-07-19T09:27:30Z&rscd=attachment%3B%20filename%3DDebugyourcustomcodeinUSQLADLA.mp4]
>

> [!NOTE]
> <span data-ttu-id="09b0e-110">Visual Studio vyžaduje následující dvě aktualizace, pokud již nejsou nainstalovány: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) a [Universal C Runtime pro systém Windows](https://www.microsoft.com/download/details.aspx?id=50410).</span><span class="sxs-lookup"><span data-stu-id="09b0e-110">Visual Studio requires the following two updates, if they are not already installed: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) and the [Universal C Runtime for Windows](https://www.microsoft.com/download/details.aspx?id=50410).</span></span>

## <a name="download-failed-vertex-to-local-machine"></a><span data-ttu-id="09b0e-111">Vrchol stažení se nezdařilo místním počítači</span><span class="sxs-lookup"><span data-stu-id="09b0e-111">Download failed vertex to local machine</span></span>

<span data-ttu-id="09b0e-112">Při otevření neúspěšnou úlohu v nástrojů Azure Data Lake pro Visual Studio zobrazí žlutý výstrahy pruh s podrobné chybové zprávy v kartě chyby.</span><span class="sxs-lookup"><span data-stu-id="09b0e-112">When you open a failed job in Azure Data Lake Tools for Visual Studio, you see a yellow alert bar with detailed error messages in the error tab.</span></span>

1. <span data-ttu-id="09b0e-113">Klikněte na tlačítko **Stáhnout** a stahovat požadované prostředky a vstupní datové proudy.</span><span class="sxs-lookup"><span data-stu-id="09b0e-113">Click **Download** to download all the required resources and input streams.</span></span> <span data-ttu-id="09b0e-114">Pokud stahování nedokončí, klikněte na tlačítko **opakujte**.</span><span class="sxs-lookup"><span data-stu-id="09b0e-114">If the download doesn't complete, click **Retry**.</span></span>

2. <span data-ttu-id="09b0e-115">Klikněte na tlačítko **otevřete** po dokončení stahování pro generování prostředí místní ladění.</span><span class="sxs-lookup"><span data-stu-id="09b0e-115">Click **Open** after the download completes to generate a local debugging environment.</span></span> <span data-ttu-id="09b0e-116">Nová instance Visual Studio s ladění řešení je automaticky vytvořen a otevřít.</span><span class="sxs-lookup"><span data-stu-id="09b0e-116">A new Visual Studio instance with a debugging solution is automatically created and opened.</span></span>

![Azure Data Lake Analytics U-SQL vrchol stažení sady visual studio pro ladění](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

<span data-ttu-id="09b0e-118">Úlohy mohou zahrnovat soubory zdrojového kódu nebo registrovaný sestavení a tyto dva typy mají různé scénáře ladění.</span><span class="sxs-lookup"><span data-stu-id="09b0e-118">Jobs may include code-behind source files or registered assemblies, and these two types have different debugging scenarios.</span></span>

- [<span data-ttu-id="09b0e-119">Ladění úlohy se nezdařilo s kódem v pozadí</span><span class="sxs-lookup"><span data-stu-id="09b0e-119">Debug a failed job with code-behind</span></span>](#debug-job-failed-with-code-behind)
- [<span data-ttu-id="09b0e-120">Ladění neúspěšnou úlohu se sestavení</span><span class="sxs-lookup"><span data-stu-id="09b0e-120">Debug a failed job with assemblies</span></span>](#debug-job-failed-with-assemblies)


## <a name="debug-job-failed-with-code-behind"></a><span data-ttu-id="09b0e-121">Ladění úlohy se nezdařilo s kódem v pozadí</span><span class="sxs-lookup"><span data-stu-id="09b0e-121">Debug job failed with code-behind</span></span>

<span data-ttu-id="09b0e-122">Pokud se nezdaří úlohy U-SQL a úlohy zahrnuje uživatelského kódu (obvykle s názvem `Script.usql.cs` v projektu U-SQL), že zdrojový kód je importovat do ladění řešení.</span><span class="sxs-lookup"><span data-stu-id="09b0e-122">If a U-SQL job fails, and the job includes user code (typically named `Script.usql.cs` in a U-SQL project), that source code is imported into the debugging solution.</span></span>  <span data-ttu-id="09b0e-123">Odtud můžete použít sady Visual Studio ladicí nástroje (sledovat, proměnné atd.) k vyřešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="09b0e-123">From there you can use the Visual Studio debugging tools (watch, variables, etc.) to troubleshoot the problem.</span></span>

> [!NOTE]
> <span data-ttu-id="09b0e-124">Před ladění, nezapomeňte zkontrolovat **výjimky modulu CLR** v okně Nastavení výjimky (**Ctrl + Alt + E**).</span><span class="sxs-lookup"><span data-stu-id="09b0e-124">Before debugging, be sure to check **Common Language Runtime Exceptions** in the Exception Settings window (**Ctrl + Alt + E**).</span></span>

![Azure Data Lake Analytics U-SQL ladění sady visual studio nastavení](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

1. <span data-ttu-id="09b0e-126">Stiskněte klávesu **F5** spustit kód kódu.</span><span class="sxs-lookup"><span data-stu-id="09b0e-126">Press **F5** to run the code-behind code.</span></span> <span data-ttu-id="09b0e-127">Bude fungovat, dokud nebude zastaven výjimkou.</span><span class="sxs-lookup"><span data-stu-id="09b0e-127">It will run until it is stopped by an exception.</span></span>

2. <span data-ttu-id="09b0e-128">Otevřete `ADLTool_Codebehind.usql.cs` souboru a nastavit zarážky, stiskněte **F5** k ladění kódu krok za krokem.</span><span class="sxs-lookup"><span data-stu-id="09b0e-128">Open the `ADLTool_Codebehind.usql.cs` file and set breakpoints, then press **F5** to debug the code step by step.</span></span>

    ![Azure Data Lake Analytics U-SQL ladění výjimek](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

## <a name="debug-job-failed-with-assemblies"></a><span data-ttu-id="09b0e-130">Ladění úlohy se nezdařilo s sestavení</span><span class="sxs-lookup"><span data-stu-id="09b0e-130">Debug job failed with assemblies</span></span>

<span data-ttu-id="09b0e-131">Pokud používáte registrované sestavení ve vašem skriptu U-SQL, systému nelze načíst zdrojový kód.</span><span class="sxs-lookup"><span data-stu-id="09b0e-131">If you use registered assemblies in your U-SQL script, the system can't get the source code automatically.</span></span> <span data-ttu-id="09b0e-132">Ručně přidejte v tomto případě soubory zdrojového kódu sestavení do řešení.</span><span class="sxs-lookup"><span data-stu-id="09b0e-132">In this case, manually add the assemblies' source code files to the solution.</span></span>

### <a name="configure-the-solution"></a><span data-ttu-id="09b0e-133">Konfigurace řešení</span><span class="sxs-lookup"><span data-stu-id="09b0e-133">Configure the solution</span></span>

1. <span data-ttu-id="09b0e-134">Klikněte pravým tlačítkem na **řešení 'VertexDebug' > Přidat > existující projekt...**  najít zdrojový kód sestavení a přidejte projekt k ladění řešení.</span><span class="sxs-lookup"><span data-stu-id="09b0e-134">Right-click **Solution 'VertexDebug' > Add > Existing Project...** to find the assemblies' source code and add the project to the debugging solution.</span></span>

    ![Přidání projektu Azure Data Lake Analytics U-SQL ladění](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. <span data-ttu-id="09b0e-136">Klikněte pravým tlačítkem na **LocalVertexHost > vlastnosti** v řešení a zkopírujte **pracovní adresář** cesta.</span><span class="sxs-lookup"><span data-stu-id="09b0e-136">Right-click **LocalVertexHost > Properties** in the solution and copy the **Working Directory** path.</span></span>

3. <span data-ttu-id="09b0e-137">Klikněte pravým tlačítkem na **sestavení projektu zdrojového kódu > vlastnosti**, vyberte **sestavení** kartě na levé straně a vložte zkopírovaný cestu jako **výstup > Výstupní cesta**.</span><span class="sxs-lookup"><span data-stu-id="09b0e-137">Right-Click **assembly source code project > Properties**, select the **Build** tab at left, and paste the copied path as **Output > Output path**.</span></span>

    ![Azure Data Lake Analytics U-SQL ladění, nastavte cestu pdb](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

4. <span data-ttu-id="09b0e-139">Stiskněte klávesu **Ctrl + Alt + E**, zkontrolujte **výjimky modulu CLR** v okně Nastavení výjimky.</span><span class="sxs-lookup"><span data-stu-id="09b0e-139">Press **Ctrl + Alt + E**, check **Common Language Runtime Exceptions** in Exception Settings window.</span></span>

### <a name="start-debug"></a><span data-ttu-id="09b0e-140">Spuštění ladění</span><span class="sxs-lookup"><span data-stu-id="09b0e-140">Start debug</span></span>

1. <span data-ttu-id="09b0e-141">Klikněte pravým tlačítkem na **sestavení projektu zdrojového kódu > sestavit** do výstupní soubory PDB do `LocalVertexHost` pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="09b0e-141">Right-click **assembly source code project > Rebuild** to output .pdb files to the `LocalVertexHost` working directory.</span></span>

2. <span data-ttu-id="09b0e-142">Stiskněte klávesu **F5** a projekt se spustí, dokud nebude zastaven výjimkou.</span><span class="sxs-lookup"><span data-stu-id="09b0e-142">Press **F5** and the project will run until it is stopped by an exception.</span></span> <span data-ttu-id="09b0e-143">Může zobrazí následující zprávu upozornění, která můžete bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="09b0e-143">You may see the following warning message, which you can safely ignore.</span></span> <span data-ttu-id="09b0e-144">Ho může trvat několik minut dostali na obrazovku pro ladění.</span><span class="sxs-lookup"><span data-stu-id="09b0e-144">It can take up to a minute to get to the debug screen.</span></span>

    ![Azure Data Lake Analytics U-SQL ladění sady visual studio upozornění](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

3. <span data-ttu-id="09b0e-146">Otevřete vašeho zdrojového kódu a nastavit zarážky, stiskněte **F5** k ladění kódu krok za krokem.</span><span class="sxs-lookup"><span data-stu-id="09b0e-146">Open your source code and set breakpoints, then press **F5** to debug the code step by step.</span></span>

<span data-ttu-id="09b0e-147">Můžete také použít Visual Studio ladicí nástroje (sledovat, proměnné atd.) k vyřešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="09b0e-147">You can also use the Visual Studio debugging tools (watch, variables, etc.) to troubleshoot the problem.</span></span>

> [!NOTE]
> <span data-ttu-id="09b0e-148">Znovu sestavte projekt sestavení zdrojového kódu pokaždé, když po úpravě kód pro vygenerování souborů aktualizované pdb.</span><span class="sxs-lookup"><span data-stu-id="09b0e-148">Rebuild the assembly source code project each time after you modify the code to generate updated .pdb files.</span></span>

<span data-ttu-id="09b0e-149">Po ladění, po úspěšném dokončení projektu ve výstupním okně zobrazí následující zprávu:</span><span class="sxs-lookup"><span data-stu-id="09b0e-149">After debugging, if the project completes successfully the output window shows the following message:</span></span>

```
The Program 'LocalVertexHost.exe' has exited with code 0 (0x0).
```

![Azure Data Lake Analytics U-SQL ladění úspěšné](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

## <a name="resubmit-the-job"></a><span data-ttu-id="09b0e-151">Odešlete znovu úlohu</span><span class="sxs-lookup"><span data-stu-id="09b0e-151">Resubmit the job</span></span>

<span data-ttu-id="09b0e-152">Po dokončení ladění znovu odešlete informace o neúspěšné úloze.</span><span class="sxs-lookup"><span data-stu-id="09b0e-152">Once you have completed debugging, resubmit the failed job.</span></span>

1. <span data-ttu-id="09b0e-153">Pro úlohy s kódem v pozadí řešení, zkopírujte kód C# do zdrojového souboru kódu na pozadí (obvykle `Script.usql.cs`).</span><span class="sxs-lookup"><span data-stu-id="09b0e-153">For jobs with code-behind solutions, copy your C# code into the code-behind source file (typically `Script.usql.cs`).</span></span>
2. <span data-ttu-id="09b0e-154">Pro úlohy se sestaveními registraci sestavení aktualizované .dll do vaší databáze ADLA:</span><span class="sxs-lookup"><span data-stu-id="09b0e-154">For jobs with assemblies, register the updated .dll assemblies into your ADLA database:</span></span>
    1. <span data-ttu-id="09b0e-155">Z Průzkumníka serveru nebo v Průzkumníku cloudu, rozbalte **ADLA účet > databáze** uzlu.</span><span class="sxs-lookup"><span data-stu-id="09b0e-155">From Server Explorer or Cloud Explorer, expand the **ADLA account > Databases** node.</span></span>
    2. <span data-ttu-id="09b0e-156">Klikněte pravým tlačítkem na **sestavení** a registraci vaší nové sestavení .dll s databází ADLA: ![Azure Data Lake Analytics U-SQL ladění registrace sestavení](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)</span><span class="sxs-lookup"><span data-stu-id="09b0e-156">Right-click **Assemblies** and register your new .dll assemblies with the ADLA database: ![Azure Data Lake Analytics U-SQL debug register assembly](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)</span></span>
3. <span data-ttu-id="09b0e-157">Odešlete znovu úlohu.</span><span class="sxs-lookup"><span data-stu-id="09b0e-157">Resubmit your job.</span></span>

## <a name="next-steps"></a><span data-ttu-id="09b0e-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="09b0e-158">Next steps</span></span>

- [<span data-ttu-id="09b0e-159">Průvodce programovatelnosti U-SQL</span><span class="sxs-lookup"><span data-stu-id="09b0e-159">U-SQL programmability guide</span></span>](data-lake-analytics-u-sql-programmability-guide.md)
- [<span data-ttu-id="09b0e-160">Vývoj U-SQL uživatelem definované operátory pro úlohy Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="09b0e-160">Develop U-SQL User-defined operators for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [<span data-ttu-id="09b0e-161">Kurz: vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09b0e-161">Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
