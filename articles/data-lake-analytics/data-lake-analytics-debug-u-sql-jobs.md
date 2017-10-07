---
title: "úlohy aaaDebug U-SQL | Microsoft Docs"
description: "Zjistěte, jak toodebug U-SQL se nezdařilo vrchol pomocí sady Visual Studio."
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
ms.openlocfilehash: 092bffa1a59ed91c5837402d0276447480b923fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a><span data-ttu-id="5344e-103">Ladění uživatelem definované C# – kód pro selhání úloh U-SQL</span><span class="sxs-lookup"><span data-stu-id="5344e-103">Debug user-defined C# code for failed U-SQL jobs</span></span>

<span data-ttu-id="5344e-104">U-SQL poskytuje model rozšiřitelnosti pomocí jazyka C#, takže můžete napsat váš kód tooadd funkce například vlastní Extraktor nebo reduktorem.</span><span class="sxs-lookup"><span data-stu-id="5344e-104">U-SQL provides an extensibility model using C#, so you can write your code tooadd functionality such as a custom extractor or reducer.</span></span> <span data-ttu-id="5344e-105">Další, najdete v části toolearn [U-SQL programovatelnosti průvodce](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf).</span><span class="sxs-lookup"><span data-stu-id="5344e-105">toolearn more, see [U-SQL programmability guide](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf).</span></span> <span data-ttu-id="5344e-106">V praxi může potřebovat žádný kód, ladění a systémy velkých objemů dat může poskytnout jenom omezené runtime ladění informace, jako jsou soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="5344e-106">In practice any code may need debugging, and big data systems may only provide limited runtime debugging information such as log files.</span></span>

<span data-ttu-id="5344e-107">Azure nástrojů Data Lake pro Visual Studio poskytuje funkci **se nezdařilo ladění vrchol**, která vám umožňuje klonovat neúspěšnou úlohu z místního počítače tooyour hello cloudu pro ladění.</span><span class="sxs-lookup"><span data-stu-id="5344e-107">Azure Data Lake Tools for Visual Studio provides a feature called **Failed Vertex Debug**, which lets you clone a failed job from hello cloud tooyour local machine for debugging.</span></span> <span data-ttu-id="5344e-108">Vytvořit místní kopii Hello zaznamená hello celého cloudového prostředí, včetně všech vstupních dat a uživatelského kódu.</span><span class="sxs-lookup"><span data-stu-id="5344e-108">hello local clone captures hello entire cloud environment, including any input data and user code.</span></span>

<span data-ttu-id="5344e-109">Hello toto video ukazuje se nezdařilo vrchol ladění ve nástrojů Azure Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5344e-109">hello following video demonstrates Failed Vertex Debug in Azure Data Lake Tools for Visual Studio.</span></span>

> [!VIDEO https://e0d1.wpc.azureedge.net/80E0D1/OfficeMixProdMediaBlobStorage/asset-d3aeab42-6149-4ecc-b044-aa624901ab32/b0fc0373c8f94f1bb8cd39da1310adb8.mp4?sv=2012-02-12&sr=c&si=a91fad76-cfdd-4513-9668-483de39e739c&sig=K%2FR%2FdnIi9S6P%2FBlB3iLAEV5pYu6OJFBDlQy%2FQtZ7E7M%3D&se=2116-07-19T09:27:30Z&rscd=attachment%3B%20filename%3DDebugyourcustomcodeinUSQLADLA.mp4]
>

> [!NOTE]
> <span data-ttu-id="5344e-110">Visual Studio vyžaduje hello následující dvě aktualizace, pokud již nejsou nainstalovány: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) a [Universal C Runtime pro systém Windows](https://www.microsoft.com/download/details.aspx?id=50410).</span><span class="sxs-lookup"><span data-stu-id="5344e-110">Visual Studio requires hello following two updates, if they are not already installed: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) and the [Universal C Runtime for Windows](https://www.microsoft.com/download/details.aspx?id=50410).</span></span>

## <a name="download-failed-vertex-toolocal-machine"></a><span data-ttu-id="5344e-111">Počítač toolocal vrchol stažení se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="5344e-111">Download failed vertex toolocal machine</span></span>

<span data-ttu-id="5344e-112">Při otevření neúspěšnou úlohu v nástrojů Azure Data Lake pro Visual Studio zobrazí žlutý výstrahy pruh s podrobné chybové zprávy v kartě chyby hello.</span><span class="sxs-lookup"><span data-stu-id="5344e-112">When you open a failed job in Azure Data Lake Tools for Visual Studio, you see a yellow alert bar with detailed error messages in hello error tab.</span></span>

1. <span data-ttu-id="5344e-113">Klikněte na tlačítko **Stáhnout** toodownload hello všechny potřebné prostředky a vstupní datové proudy.</span><span class="sxs-lookup"><span data-stu-id="5344e-113">Click **Download** toodownload all hello required resources and input streams.</span></span> <span data-ttu-id="5344e-114">Pokud stahování hello nedokončí, klikněte na tlačítko **opakujte**.</span><span class="sxs-lookup"><span data-stu-id="5344e-114">If hello download doesn't complete, click **Retry**.</span></span>

2. <span data-ttu-id="5344e-115">Klikněte na tlačítko **otevřete** po dokončení stahování hello toogenerate prostředí místní ladění.</span><span class="sxs-lookup"><span data-stu-id="5344e-115">Click **Open** after hello download completes toogenerate a local debugging environment.</span></span> <span data-ttu-id="5344e-116">Nová instance Visual Studio s ladění řešení je automaticky vytvořen a otevřít.</span><span class="sxs-lookup"><span data-stu-id="5344e-116">A new Visual Studio instance with a debugging solution is automatically created and opened.</span></span>

![Azure Data Lake Analytics U-SQL vrchol stažení sady visual studio pro ladění](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

<span data-ttu-id="5344e-118">Úlohy mohou zahrnovat soubory zdrojového kódu nebo registrovaný sestavení a tyto dva typy mají různé scénáře ladění.</span><span class="sxs-lookup"><span data-stu-id="5344e-118">Jobs may include code-behind source files or registered assemblies, and these two types have different debugging scenarios.</span></span>

- [<span data-ttu-id="5344e-119">Ladění úlohy se nezdařilo s kódem v pozadí</span><span class="sxs-lookup"><span data-stu-id="5344e-119">Debug a failed job with code-behind</span></span>](#debug-job-failed-with-code-behind)
- [<span data-ttu-id="5344e-120">Ladění neúspěšnou úlohu se sestavení</span><span class="sxs-lookup"><span data-stu-id="5344e-120">Debug a failed job with assemblies</span></span>](#debug-job-failed-with-assemblies)


## <a name="debug-job-failed-with-code-behind"></a><span data-ttu-id="5344e-121">Ladění úlohy se nezdařilo s kódem v pozadí</span><span class="sxs-lookup"><span data-stu-id="5344e-121">Debug job failed with code-behind</span></span>

<span data-ttu-id="5344e-122">Pokud se nezdaří úlohy U-SQL a hello úlohy zahrnuje uživatelského kódu (obvykle s názvem `Script.usql.cs` v projektu U-SQL), že zdrojový kód je importovat do hello ladění řešení.</span><span class="sxs-lookup"><span data-stu-id="5344e-122">If a U-SQL job fails, and hello job includes user code (typically named `Script.usql.cs` in a U-SQL project), that source code is imported into hello debugging solution.</span></span>  <span data-ttu-id="5344e-123">Odtud můžete hello Visual Studio ladění nástrojů (sledovat, proměnné atd.) tootroubleshoot hello problém.</span><span class="sxs-lookup"><span data-stu-id="5344e-123">From there you can use hello Visual Studio debugging tools (watch, variables, etc.) tootroubleshoot hello problem.</span></span>

> [!NOTE]
> <span data-ttu-id="5344e-124">Před ladění, se že toocheck **výjimky modulu CLR** v okně Nastavení výjimky hello (**Ctrl + Alt + E**).</span><span class="sxs-lookup"><span data-stu-id="5344e-124">Before debugging, be sure toocheck **Common Language Runtime Exceptions** in hello Exception Settings window (**Ctrl + Alt + E**).</span></span>

![Azure Data Lake Analytics U-SQL ladění sady visual studio nastavení](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

1. <span data-ttu-id="5344e-126">Stiskněte klávesu **F5** toorun hello kódu kódu.</span><span class="sxs-lookup"><span data-stu-id="5344e-126">Press **F5** toorun hello code-behind code.</span></span> <span data-ttu-id="5344e-127">Bude fungovat, dokud nebude zastaven výjimkou.</span><span class="sxs-lookup"><span data-stu-id="5344e-127">It will run until it is stopped by an exception.</span></span>

2. <span data-ttu-id="5344e-128">Otevřete hello `ADLTool_Codebehind.usql.cs` souboru a nastavit zarážky, stiskněte **F5** toodebug hello kód krok za krokem.</span><span class="sxs-lookup"><span data-stu-id="5344e-128">Open hello `ADLTool_Codebehind.usql.cs` file and set breakpoints, then press **F5** toodebug hello code step by step.</span></span>

    ![Azure Data Lake Analytics U-SQL ladění výjimek](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

## <a name="debug-job-failed-with-assemblies"></a><span data-ttu-id="5344e-130">Ladění úlohy se nezdařilo s sestavení</span><span class="sxs-lookup"><span data-stu-id="5344e-130">Debug job failed with assemblies</span></span>

<span data-ttu-id="5344e-131">Pokud používáte registrované sestavení ve vašem skriptu U-SQL, hello systému nelze získat hello zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="5344e-131">If you use registered assemblies in your U-SQL script, hello system can't get hello source code automatically.</span></span> <span data-ttu-id="5344e-132">V takovém případě ručně přidejte hello sestavení zdrojového kódu soubory toohello řešení.</span><span class="sxs-lookup"><span data-stu-id="5344e-132">In this case, manually add hello assemblies' source code files toohello solution.</span></span>

### <a name="configure-hello-solution"></a><span data-ttu-id="5344e-133">Konfigurace řešení hello</span><span class="sxs-lookup"><span data-stu-id="5344e-133">Configure hello solution</span></span>

1. <span data-ttu-id="5344e-134">Klikněte pravým tlačítkem na **řešení 'VertexDebug' > Přidat > existující projekt...**  toofind hello se sestavení zdrojového kódu a přidejte toohello projektu hello ladění řešení.</span><span class="sxs-lookup"><span data-stu-id="5344e-134">Right-click **Solution 'VertexDebug' > Add > Existing Project...** toofind hello assemblies' source code and add hello project toohello debugging solution.</span></span>

    ![Přidání projektu Azure Data Lake Analytics U-SQL ladění](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. <span data-ttu-id="5344e-136">Klikněte pravým tlačítkem na **LocalVertexHost > vlastnosti** v řešení a zkopírujte hello hello **pracovní adresář** cesta.</span><span class="sxs-lookup"><span data-stu-id="5344e-136">Right-click **LocalVertexHost > Properties** in hello solution and copy hello **Working Directory** path.</span></span>

3. <span data-ttu-id="5344e-137">Klikněte pravým tlačítkem na **sestavení projektu zdrojového kódu > vlastnosti**, vyberte hello **sestavení** kartě na levé straně a vložte hello zkopírovat cestu jako **výstup > Výstupní cesta**.</span><span class="sxs-lookup"><span data-stu-id="5344e-137">Right-Click **assembly source code project > Properties**, select hello **Build** tab at left, and paste hello copied path as **Output > Output path**.</span></span>

    ![Azure Data Lake Analytics U-SQL ladění, nastavte cestu pdb](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

4. <span data-ttu-id="5344e-139">Stiskněte klávesu **Ctrl + Alt + E**, zkontrolujte **výjimky modulu CLR** v okně Nastavení výjimky.</span><span class="sxs-lookup"><span data-stu-id="5344e-139">Press **Ctrl + Alt + E**, check **Common Language Runtime Exceptions** in Exception Settings window.</span></span>

### <a name="start-debug"></a><span data-ttu-id="5344e-140">Spuštění ladění</span><span class="sxs-lookup"><span data-stu-id="5344e-140">Start debug</span></span>

1. <span data-ttu-id="5344e-141">Klikněte pravým tlačítkem na **sestavení projektu zdrojového kódu > sestavit** toooutput PDB soubory toohello `LocalVertexHost` pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="5344e-141">Right-click **assembly source code project > Rebuild** toooutput .pdb files toohello `LocalVertexHost` working directory.</span></span>

2. <span data-ttu-id="5344e-142">Stiskněte klávesu **F5** a hello projektu se spustí, dokud nebude zastaven výjimkou.</span><span class="sxs-lookup"><span data-stu-id="5344e-142">Press **F5** and hello project will run until it is stopped by an exception.</span></span> <span data-ttu-id="5344e-143">Může se zobrazit hello následující upozornění, které můžete bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="5344e-143">You may see hello following warning message, which you can safely ignore.</span></span> <span data-ttu-id="5344e-144">To může trvat až tooa minut tooget toohello ladění obrazovky.</span><span class="sxs-lookup"><span data-stu-id="5344e-144">It can take up tooa minute tooget toohello debug screen.</span></span>

    ![Azure Data Lake Analytics U-SQL ladění sady visual studio upozornění](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

3. <span data-ttu-id="5344e-146">Otevřete vašeho zdrojového kódu a nastavit zarážky, stiskněte **F5** toodebug hello kód krok za krokem.</span><span class="sxs-lookup"><span data-stu-id="5344e-146">Open your source code and set breakpoints, then press **F5** toodebug hello code step by step.</span></span>

<span data-ttu-id="5344e-147">Můžete také použít hello Visual Studio ladění nástrojů (sledovat, proměnné atd.) tootroubleshoot hello problém.</span><span class="sxs-lookup"><span data-stu-id="5344e-147">You can also use hello Visual Studio debugging tools (watch, variables, etc.) tootroubleshoot hello problem.</span></span>

> [!NOTE]
> <span data-ttu-id="5344e-148">Znovu sestavte projekt hello sestavení zdrojového kódu pokaždé, když po úpravě soubory PDB toogenerate aktualizovat kód hello.</span><span class="sxs-lookup"><span data-stu-id="5344e-148">Rebuild hello assembly source code project each time after you modify hello code toogenerate updated .pdb files.</span></span>

<span data-ttu-id="5344e-149">Po ladění, po úspěšném dokončení projektu hello okno výstup hello ukazuje hello následující zprávou:</span><span class="sxs-lookup"><span data-stu-id="5344e-149">After debugging, if hello project completes successfully hello output window shows hello following message:</span></span>

```
hello Program 'LocalVertexHost.exe' has exited with code 0 (0x0).
```

![Azure Data Lake Analytics U-SQL ladění úspěšné](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

## <a name="resubmit-hello-job"></a><span data-ttu-id="5344e-151">Odešlete znovu úlohu hello</span><span class="sxs-lookup"><span data-stu-id="5344e-151">Resubmit hello job</span></span>

<span data-ttu-id="5344e-152">Po dokončení ladění odešlete znovu hello neúspěšnou úlohu.</span><span class="sxs-lookup"><span data-stu-id="5344e-152">Once you have completed debugging, resubmit hello failed job.</span></span>

1. <span data-ttu-id="5344e-153">Pro úlohy s kódem v pozadí řešení, zkopírujte kódu C# do souboru zdrojového kódu hello (obvykle `Script.usql.cs`).</span><span class="sxs-lookup"><span data-stu-id="5344e-153">For jobs with code-behind solutions, copy your C# code into hello code-behind source file (typically `Script.usql.cs`).</span></span>
2. <span data-ttu-id="5344e-154">Pro úlohy se sestaveními registraci sestavení .dll hello aktualizovat do vaší databáze ADLA:</span><span class="sxs-lookup"><span data-stu-id="5344e-154">For jobs with assemblies, register hello updated .dll assemblies into your ADLA database:</span></span>
    1. <span data-ttu-id="5344e-155">Z Průzkumníka serveru nebo v Průzkumníku cloudu, rozbalte položku hello **ADLA účet > databáze** uzlu.</span><span class="sxs-lookup"><span data-stu-id="5344e-155">From Server Explorer or Cloud Explorer, expand hello **ADLA account > Databases** node.</span></span>
    2. <span data-ttu-id="5344e-156">Klikněte pravým tlačítkem na **sestavení** a registraci vaší nové sestavení .dll s databází ADLA hello: ![Azure Data Lake Analytics U-SQL ladění registrace sestavení](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)</span><span class="sxs-lookup"><span data-stu-id="5344e-156">Right-click **Assemblies** and register your new .dll assemblies with hello ADLA database: ![Azure Data Lake Analytics U-SQL debug register assembly](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)</span></span>
3. <span data-ttu-id="5344e-157">Odešlete znovu úlohu.</span><span class="sxs-lookup"><span data-stu-id="5344e-157">Resubmit your job.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5344e-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5344e-158">Next steps</span></span>

- [<span data-ttu-id="5344e-159">Průvodce programovatelnosti U-SQL</span><span class="sxs-lookup"><span data-stu-id="5344e-159">U-SQL programmability guide</span></span>](data-lake-analytics-u-sql-programmability-guide.md)
- [<span data-ttu-id="5344e-160">Vývoj U-SQL uživatelem definované operátory pro úlohy Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="5344e-160">Develop U-SQL User-defined operators for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [<span data-ttu-id="5344e-161">Kurz: vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5344e-161">Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
