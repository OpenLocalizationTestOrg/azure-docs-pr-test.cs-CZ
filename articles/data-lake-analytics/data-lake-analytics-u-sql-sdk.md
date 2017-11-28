---
title: "místní aaaScale U-SQL spustit a otestovat s Azure Data Lake U-SQL SDK | Microsoft Docs"
description: "Zjistěte, jak místní úlohy tooscale U-SQL Azure Data Lake U-SQL SDK toouse spustit a otestovat pomocí příkazového řádku a programovací rozhraní na místní pracovní stanici."
services: data-lake-analytics
documentationcenter: 
author: 
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: yanacai
ms.openlocfilehash: 2b0a16229789268e333f723ff6fc2c3efdc29905
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-u-sql-local-run-and-test-with-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="b8aaf-103">Škálování U-SQL místní spuštění a testování pomocí Azure Data Lake U-SQL SDK</span><span class="sxs-lookup"><span data-stu-id="b8aaf-103">Scale U-SQL local run and test with Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="b8aaf-104">Při vývoji skript U-SQL, je běžné toorun a testů U-SQL skriptu místně před odešlete ji toocloud.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-104">When developing U-SQL script, it is common toorun and test U-SQL script locally before submit it toocloud.</span></span> <span data-ttu-id="b8aaf-105">Azure Data Lake poskytuje balíček Nuget s názvem SDK Azure Data Lake U-SQL pro tento scénář, pomocí kterých lze snadno škálovat U-SQL místní spuštění a testování.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-105">Azure Data Lake provides a Nuget package called Azure Data Lake U-SQL SDK for this scenario, through which you can easily scale U-SQL local run and test.</span></span> <span data-ttu-id="b8aaf-106">Je také možné toointegrate U-SQL testování s CI (nepřetržité integrace) systému tooautomate hello kompilace a testování.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-106">It is also possible toointegrate this U-SQL test with CI (Continuous Integration) system tooautomate hello compile and test.</span></span>

<span data-ttu-id="b8aaf-107">Pokud se zajímáte o jak toomanually místní spuštění a ladění skriptu U-SQL pomocí nástrojů s grafickým uživatelským rozhraním, můžete pomocí nástroje Azure Data Lake pro Visual Studio pro tento.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-107">If you care about how toomanually local run and debug U-SQL script with GUI tooling, then you can use Azure Data Lake Tools for Visual Studio for that.</span></span> <span data-ttu-id="b8aaf-108">Další informace z [zde](data-lake-analytics-data-lake-tools-local-run.md).</span><span class="sxs-lookup"><span data-stu-id="b8aaf-108">You can learn more from [here](data-lake-analytics-data-lake-tools-local-run.md).</span></span>

## <a name="install-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="b8aaf-109">Instalace Azure Data Lake U-SQL SDK</span><span class="sxs-lookup"><span data-stu-id="b8aaf-109">Install Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="b8aaf-110">Můžete získat hello SDK Azure Data Lake U-SQL [sem](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) na Nuget.org. A před jeho použitím, je třeba toomake se, že máte následující závislosti.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-110">You can get hello Azure Data Lake U-SQL SDK [here](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) on Nuget.org. And before using it, you need toomake sure you have dependencies as follows.</span></span>

### <a name="dependencies"></a><span data-ttu-id="b8aaf-111">Závislosti</span><span class="sxs-lookup"><span data-stu-id="b8aaf-111">Dependencies</span></span>

<span data-ttu-id="b8aaf-112">Hello Data Lake U-SQL SDK vyžaduje hello následující závislosti:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-112">hello Data Lake U-SQL SDK requires hello following dependencies:</span></span>

- <span data-ttu-id="b8aaf-113">[Rozhraní Microsoft .NET Framework 4.6 nebo novější](https://www.microsoft.com/download/details.aspx?id=17851).</span><span class="sxs-lookup"><span data-stu-id="b8aaf-113">[Microsoft .NET Framework 4.6 or newer](https://www.microsoft.com/download/details.aspx?id=17851).</span></span>
- <span data-ttu-id="b8aaf-114">Microsoft Visual C++ 14 a sady Windows SDK 10.0.10240.0 nebo novější (což se označuje jako CppSDK v tomto článku).</span><span class="sxs-lookup"><span data-stu-id="b8aaf-114">Microsoft Visual C++ 14 and Windows SDK 10.0.10240.0 or newer (which is called CppSDK in this article).</span></span> <span data-ttu-id="b8aaf-115">Existují dva způsoby tooget CppSDK:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-115">There are two ways tooget CppSDK:</span></span>

    - <span data-ttu-id="b8aaf-116">Nainstalujte [sady Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou).</span><span class="sxs-lookup"><span data-stu-id="b8aaf-116">Install [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou).</span></span> <span data-ttu-id="b8aaf-117">Budete mít \Windows Kits\10 složku ve složce Program Files hello – například C:\Program Files (x86) \Windows Kits\10\.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-117">You'll have a \Windows Kits\10 folder under hello Program Files folder--for example, C:\Program Files (x86)\Windows Kits\10\.</span></span> <span data-ttu-id="b8aaf-118">Naleznete zde také hello verze Windows 10 SDK v části \Windows Kits\10\Lib.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-118">You'll also find hello Windows 10 SDK version under \Windows Kits\10\Lib.</span></span> <span data-ttu-id="b8aaf-119">Pokud nevidíte tyto složky, přeinstalujte Visual Studio a že tooselect hello Windows 10 SDK se během instalace hello.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-119">If you don’t see these folders, reinstall Visual Studio and be sure tooselect hello Windows 10 SDK during hello installation.</span></span> <span data-ttu-id="b8aaf-120">Pokud je to nainstalované s Visual Studio, místní kompilátoru hello U-SQL najdete je automaticky.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-120">If you have this installed with Visual Studio, hello U-SQL local compiler will find it automatically.</span></span>

    ![Nástroje data Lake pro Visual Studio místní spuštění Windows 10 SDK](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

    - <span data-ttu-id="b8aaf-122">Nainstalujte [nástroje Data Lake pro Visual Studio](http://aka.ms/adltoolsvs).</span><span class="sxs-lookup"><span data-stu-id="b8aaf-122">Install [Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs).</span></span> <span data-ttu-id="b8aaf-123">Můžete najít, že hello balené C:\Program Files (x86) \Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK souborů Visual C++ a sady Windows SDK.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-123">You can find hello prepackaged Visual C++ and Windows SDK files at C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK.</span></span> <span data-ttu-id="b8aaf-124">Místní kompilátoru hello U-SQL v tomto případě nelze najít hello závislosti automaticky.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-124">In this case, hello U-SQL local compiler cannot find hello dependencies automatically.</span></span> <span data-ttu-id="b8aaf-125">Musíte pro něj toospecify hello CppSDK cesta.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-125">You need toospecify hello CppSDK path for it.</span></span> <span data-ttu-id="b8aaf-126">Můžete zkopírovat hello soubory tooanother umístění nebo ho jako je použít.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-126">You can either copy hello files tooanother location or use it as is.</span></span>

## <a name="understand-basic-concepts"></a><span data-ttu-id="b8aaf-127">Pochopit základní koncepty</span><span class="sxs-lookup"><span data-stu-id="b8aaf-127">Understand basic concepts</span></span>

### <a name="data-root"></a><span data-ttu-id="b8aaf-128">Kořenové datové</span><span class="sxs-lookup"><span data-stu-id="b8aaf-128">Data root</span></span>

<span data-ttu-id="b8aaf-129">Hello kořenové datové složce je pro účet místního výpočetní hello "místní úložiště".</span><span class="sxs-lookup"><span data-stu-id="b8aaf-129">hello data-root folder is a "local store" for hello local compute account.</span></span> <span data-ttu-id="b8aaf-130">Je ekvivalentní toohello účtu Azure Data Lake Store účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-130">It's equivalent toohello Azure Data Lake Store account of a Data Lake Analytics account.</span></span> <span data-ttu-id="b8aaf-131">Přepínání tooa různé kořenové datové složce je stejně jako přepínání tooa úložiště jiný účet.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-131">Switching tooa different data-root folder is just like switching tooa different store account.</span></span> <span data-ttu-id="b8aaf-132">Pokud budete chtít tooaccess běžně sdílí data pomocí různých dat kořenové složky, je nutné použít absolutní cesty ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-132">If you want tooaccess commonly shared data with different data-root folders, you must use absolute paths in your scripts.</span></span> <span data-ttu-id="b8aaf-133">Nebo vytvořte symbolické odkazy systému souborů (například **mklink** na systém souborů NTFS) v části hello kořenové datové složce toopoint toohello sdílená data.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-133">Or, create file system symbolic links (for example, **mklink** on NTFS) under hello data-root folder toopoint toohello shared data.</span></span>

<span data-ttu-id="b8aaf-134">Hello kořenové datové složce se používá pro:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-134">hello data-root folder is used to:</span></span>

- <span data-ttu-id="b8aaf-135">Uložení místních metadat, včetně databází, tabulky, funkce vracející tabulku (Tvf) a sestavení.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-135">Store local metadata, including databases, tables, table-valued functions (TVFs), and assemblies.</span></span>
- <span data-ttu-id="b8aaf-136">Vyhledání hello vstupní a výstupní cesty, které jsou definovány jako relativní cesty v U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-136">Look up hello input and output paths that are defined as relative paths in U-SQL.</span></span> <span data-ttu-id="b8aaf-137">Pomocí relativní cesty umožňuje snazší toodeploy vaše projekty tooAzure U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-137">Using relative paths makes it easier toodeploy your U-SQL projects tooAzure.</span></span>

### <a name="file-path-in-u-sql"></a><span data-ttu-id="b8aaf-138">Cesta k souboru v U-SQL</span><span class="sxs-lookup"><span data-stu-id="b8aaf-138">File path in U-SQL</span></span>

<span data-ttu-id="b8aaf-139">Můžete je relativní cesta a místní cestou absolutní v skriptů U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-139">You can use both a relative path and a local absolute path in U-SQL scripts.</span></span> <span data-ttu-id="b8aaf-140">relativní cesta Hello je cesta ke složce relativní toohello zadaný kořenový data.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-140">hello relative path is relative toohello specified data-root folder path.</span></span> <span data-ttu-id="b8aaf-141">Doporučujeme vám, že můžete použít "/" jako hello cesta oddělovače toomake skripty kompatibilní s hello na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-141">We recommend that you use "/" as hello path separator toomake your scripts compatible with hello server side.</span></span> <span data-ttu-id="b8aaf-142">Zde jsou některé příklady relativní cesty a jejich ekvivalent absolutní cesty.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-142">Here are some examples of relative paths and their equivalent absolute paths.</span></span> <span data-ttu-id="b8aaf-143">V těchto příkladech je C:\LocalRunDataRoot hello kořenové datové složce.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-143">In these examples, C:\LocalRunDataRoot is hello data-root folder.</span></span>

|<span data-ttu-id="b8aaf-144">Relativní cesta</span><span class="sxs-lookup"><span data-stu-id="b8aaf-144">Relative path</span></span>|<span data-ttu-id="b8aaf-145">Absolutní cesty</span><span class="sxs-lookup"><span data-stu-id="b8aaf-145">Absolute path</span></span>|
|-------------|-------------|
|<span data-ttu-id="b8aaf-146">/ABC/DEF/Input.csv</span><span class="sxs-lookup"><span data-stu-id="b8aaf-146">/abc/def/input.csv</span></span> |<span data-ttu-id="b8aaf-147">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="b8aaf-147">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="b8aaf-148">ABC/DEF/Input.csv</span><span class="sxs-lookup"><span data-stu-id="b8aaf-148">abc/def/input.csv</span></span>  |<span data-ttu-id="b8aaf-149">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="b8aaf-149">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="b8aaf-150">D:/ABC/DEF/Input.csv</span><span class="sxs-lookup"><span data-stu-id="b8aaf-150">D:/abc/def/input.csv</span></span> |<span data-ttu-id="b8aaf-151">D:\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="b8aaf-151">D:\abc\def\input.csv</span></span>|

### <a name="working-directory"></a><span data-ttu-id="b8aaf-152">Pracovní adresář</span><span class="sxs-lookup"><span data-stu-id="b8aaf-152">Working directory</span></span>

<span data-ttu-id="b8aaf-153">Při místním spuštění skriptu hello U-SQL, vytvoří se během kompilace v aktuálním adresáři spuštěné pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-153">When running hello U-SQL script locally, a working directory is created during compilation under current running directory.</span></span> <span data-ttu-id="b8aaf-154">Kromě toho toohello kompilace výstupy, hello potřebné soubory modulu runtime pro místní spuštění bude stínové zkopírovaný toothis pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-154">In addition toohello compilation outputs, hello needed runtime files for local execution will be shadow copied toothis working directory.</span></span> <span data-ttu-id="b8aaf-155">Hello práce kořenový adresář se označuje jako "ScopeWorkDir" a hello soubory v pracovním adresáři hello jsou následující:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-155">hello working directory root folder is called "ScopeWorkDir" and hello files under hello working directory are as follows:</span></span>

|<span data-ttu-id="b8aaf-156">Adresář nebo soubor</span><span class="sxs-lookup"><span data-stu-id="b8aaf-156">Directory/file</span></span>|<span data-ttu-id="b8aaf-157">Adresář nebo soubor</span><span class="sxs-lookup"><span data-stu-id="b8aaf-157">Directory/file</span></span>|<span data-ttu-id="b8aaf-158">Adresář nebo soubor</span><span class="sxs-lookup"><span data-stu-id="b8aaf-158">Directory/file</span></span>|<span data-ttu-id="b8aaf-159">Definice</span><span class="sxs-lookup"><span data-stu-id="b8aaf-159">Definition</span></span>|<span data-ttu-id="b8aaf-160">Popis</span><span class="sxs-lookup"><span data-stu-id="b8aaf-160">Description</span></span>|
|--------------|--------------|--------------|----------|-----------|
|<span data-ttu-id="b8aaf-161">C6A101DDCB470506</span><span class="sxs-lookup"><span data-stu-id="b8aaf-161">C6A101DDCB470506</span></span>| | |<span data-ttu-id="b8aaf-162">Řetězec hash verze modulu runtime</span><span class="sxs-lookup"><span data-stu-id="b8aaf-162">Hash string of runtime version</span></span>|<span data-ttu-id="b8aaf-163">Soubory modulu runtime, které jsou potřebné pro provedení místní stínové kopie</span><span class="sxs-lookup"><span data-stu-id="b8aaf-163">Shadow copy of runtime files needed for local execution</span></span>|
| |<span data-ttu-id="b8aaf-164">Script_66AE4909AA0ED06C</span><span class="sxs-lookup"><span data-stu-id="b8aaf-164">Script_66AE4909AA0ED06C</span></span>| |<span data-ttu-id="b8aaf-165">Název skriptu + hash řetězec cestu ke skriptu</span><span class="sxs-lookup"><span data-stu-id="b8aaf-165">Script name + hash string of script path</span></span>|<span data-ttu-id="b8aaf-166">Výstupy kompilace a provádění krok protokolování</span><span class="sxs-lookup"><span data-stu-id="b8aaf-166">Compilation outputs and execution step logging</span></span>|
| | |<span data-ttu-id="b8aaf-167">\_skript\_.abr</span><span class="sxs-lookup"><span data-stu-id="b8aaf-167">\_script\_.abr</span></span>|<span data-ttu-id="b8aaf-168">Výstup kompilátoru</span><span class="sxs-lookup"><span data-stu-id="b8aaf-168">Compiler output</span></span>|<span data-ttu-id="b8aaf-169">Soubor algebra</span><span class="sxs-lookup"><span data-stu-id="b8aaf-169">Algebra file</span></span>|
| | |<span data-ttu-id="b8aaf-170">\_ScopeCodeGen\_. *</span><span class="sxs-lookup"><span data-stu-id="b8aaf-170">\_ScopeCodeGen\_.*</span></span>|<span data-ttu-id="b8aaf-171">Výstup kompilátoru</span><span class="sxs-lookup"><span data-stu-id="b8aaf-171">Compiler output</span></span>|<span data-ttu-id="b8aaf-172">Vygenerovaný spravovaného kódu</span><span class="sxs-lookup"><span data-stu-id="b8aaf-172">Generated managed code</span></span>|
| | |<span data-ttu-id="b8aaf-173">\_ScopeCodeGenEngine\_. *</span><span class="sxs-lookup"><span data-stu-id="b8aaf-173">\_ScopeCodeGenEngine\_.*</span></span>|<span data-ttu-id="b8aaf-174">Výstup kompilátoru</span><span class="sxs-lookup"><span data-stu-id="b8aaf-174">Compiler output</span></span>|<span data-ttu-id="b8aaf-175">Vygenerovaný nativního kódu</span><span class="sxs-lookup"><span data-stu-id="b8aaf-175">Generated native code</span></span>|
| | |<span data-ttu-id="b8aaf-176">Odkazovaná sestavení</span><span class="sxs-lookup"><span data-stu-id="b8aaf-176">referenced assemblies</span></span>|<span data-ttu-id="b8aaf-177">Odkaz na sestavení</span><span class="sxs-lookup"><span data-stu-id="b8aaf-177">Assembly reference</span></span>|<span data-ttu-id="b8aaf-178">Soubory odkazované sestavení</span><span class="sxs-lookup"><span data-stu-id="b8aaf-178">Referenced assembly files</span></span>|
| | |<span data-ttu-id="b8aaf-179">deployed_resources</span><span class="sxs-lookup"><span data-stu-id="b8aaf-179">deployed_resources</span></span>|<span data-ttu-id="b8aaf-180">Nasazení prostředků</span><span class="sxs-lookup"><span data-stu-id="b8aaf-180">Resource deployment</span></span>|<span data-ttu-id="b8aaf-181">Soubory nasazení prostředků</span><span class="sxs-lookup"><span data-stu-id="b8aaf-181">Resource deployment files</span></span>|
| | |<span data-ttu-id="b8aaf-182">XXXXXXXX.xxx[1..n]\_\*. *</span><span class="sxs-lookup"><span data-stu-id="b8aaf-182">xxxxxxxx.xxx[1..n]\_\*.*</span></span>|<span data-ttu-id="b8aaf-183">Spuštění protokolu</span><span class="sxs-lookup"><span data-stu-id="b8aaf-183">Execution log</span></span>|<span data-ttu-id="b8aaf-184">Protokol provádění kroků</span><span class="sxs-lookup"><span data-stu-id="b8aaf-184">Log of execution steps</span></span>|


## <a name="use-hello-sdk-from-hello-command-line"></a><span data-ttu-id="b8aaf-185">Použití hello SDK z příkazového řádku hello</span><span class="sxs-lookup"><span data-stu-id="b8aaf-185">Use hello SDK from hello command line</span></span>

### <a name="command-line-interface-of-hello-helper-application"></a><span data-ttu-id="b8aaf-186">Rozhraní příkazového řádku aplikace hello pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="b8aaf-186">Command-line interface of hello helper application</span></span>

<span data-ttu-id="b8aaf-187">V části SDK directory\build\runtime je LocalRunHelper.exe hello nástroj příkazového řádku se aplikace, která poskytuje rozhraní toomost hello běžně používá místní spuštění funkce.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-187">Under SDK directory\build\runtime, LocalRunHelper.exe is hello command-line helper application that provides interfaces toomost of hello commonly used local-run functions.</span></span> <span data-ttu-id="b8aaf-188">Upozorňujeme, že oba hello příkazu a hello argument přepínače jsou malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-188">Note that both hello command and hello argument switches are case-sensitive.</span></span> <span data-ttu-id="b8aaf-189">tooinvoke ho:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-189">tooinvoke it:</span></span>

    LocalRunHelper.exe <command> <Required-Command-Arguments> [Optional-Command-Arguments]

<span data-ttu-id="b8aaf-190">Spustit LocalRunHelper.exe bez argumentů nebo s hello **pomoci** přepínač informace nápovědy tooshow hello:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-190">Run LocalRunHelper.exe without arguments or with hello **help** switch tooshow hello help information:</span></span>

    > LocalRunHelper.exe help

        Command 'help' :  Show usage information
        Command 'compile' :  Compile hello script
        Required Arguments :
            -Script param
                    Script File Path
        Optional Arguments :
            -Shallow [default value 'False']
                    Shallow compile

<span data-ttu-id="b8aaf-191">V hello pomoci informace:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-191">In hello help information:</span></span>

-  <span data-ttu-id="b8aaf-192">**Příkaz** poskytuje hello název příkazu.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-192">**Command** gives hello command’s name.</span></span>  
-  <span data-ttu-id="b8aaf-193">**Vyžaduje Argument** uvádí argumenty, které je nutné zadat.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-193">**Required Argument** lists arguments that must be supplied.</span></span>  
-  <span data-ttu-id="b8aaf-194">**Nepovinný Argument** uvádí argumenty, které jsou volitelné, s výchozími hodnotami.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-194">**Optional Argument** lists arguments that are optional, with default values.</span></span>  <span data-ttu-id="b8aaf-195">Nepovinné argumenty Boolean nemají parametry a jejich vzhled znamená záporná tootheir výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-195">Optional Boolean arguments don’t have parameters, and their appearances mean negative tootheir default value.</span></span>

### <a name="return-value-and-logging"></a><span data-ttu-id="b8aaf-196">Vrátí hodnotu a protokolování</span><span class="sxs-lookup"><span data-stu-id="b8aaf-196">Return value and logging</span></span>

<span data-ttu-id="b8aaf-197">Vrátí Hello podpůrnou aplikací **0** úspěch a **-1** selhání.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-197">hello helper application returns **0** for success and **-1** for failure.</span></span> <span data-ttu-id="b8aaf-198">Ve výchozím nastavení odešle hello pomocná všechny zprávy toohello aktuální konzolu.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-198">By default, hello helper sends all messages toohello current console.</span></span> <span data-ttu-id="b8aaf-199">Ale většina příkazů hello podporují hello **- MessageOut cesta_k_souboru_protokolu** nepovinný argument, který přesměruje hello výstupy tooa souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-199">However, most of hello commands support hello **-MessageOut path_to_log_file** optional argument that redirects hello outputs tooa log file.</span></span>

### <a name="environment-variable-configuring"></a><span data-ttu-id="b8aaf-200">Konfigurace proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="b8aaf-200">Environment variable configuring</span></span>

<span data-ttu-id="b8aaf-201">U-SQL místní spuštění potřebují kořenové zadaná data jako účet místní úložiště, zadaná cesta CppSDK závislosti.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-201">U-SQL local run needs a specified data root as local storage account, as well as a specified CppSDK path for dependencies.</span></span> <span data-ttu-id="b8aaf-202">Argument hello obě sady v proměnné prostředí příkazového řádku, nebo je nastavený můžete pro ně.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-202">You can both set hello argument in command-line or set environment variable for them.</span></span>

- <span data-ttu-id="b8aaf-203">Sada hello **SCOPE_CPP_SDK** proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-203">Set hello **SCOPE_CPP_SDK** environment variable.</span></span>

    <span data-ttu-id="b8aaf-204">Pokud se zobrazí Microsoft Visual C++ a hello Windows SDK prostřednictvím instalace nástrojů Data Lake pro Visual Studio, ověřte, zda máte hello následující složku:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-204">If you get Microsoft Visual C++ and hello Windows SDK by installing Data Lake Tools for Visual Studio, verify that you have hello following folder:</span></span>

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    <span data-ttu-id="b8aaf-205">Zadejte novou proměnnou prostředí s názvem **SCOPE_CPP_SDK** toopoint toothis adresáře.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-205">Define a new environment variable called **SCOPE_CPP_SDK** toopoint toothis directory.</span></span> <span data-ttu-id="b8aaf-206">Nebo zkopírujte složku toohello hello jiného umístění a zadejte **SCOPE_CPP_SDK** jako.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-206">Or copy hello folder toohello other location and specify **SCOPE_CPP_SDK** as that.</span></span>

    <span data-ttu-id="b8aaf-207">Přidání toosetting hello prostředí proměnné, můžete zadat hello **- CppSDK** argument při použití hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-207">In addition toosetting hello environment variable, you can specify hello **-CppSDK** argument when you're using hello command line.</span></span> <span data-ttu-id="b8aaf-208">Tento argument přepíše vaše proměnná prostředí CppSDK výchozí.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-208">This argument overwrites your default CppSDK environment variable.</span></span>

- <span data-ttu-id="b8aaf-209">Sada hello **LOCALRUN_DATAROOT** proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-209">Set hello **LOCALRUN_DATAROOT** environment variable.</span></span>

    <span data-ttu-id="b8aaf-210">Zadejte novou proměnnou prostředí s názvem **LOCALRUN_DATAROOT** , toohello kořenové datové body.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-210">Define a new environment variable called **LOCALRUN_DATAROOT** that points toohello data root.</span></span>

    <span data-ttu-id="b8aaf-211">Přidání toosetting hello prostředí proměnné, můžete zadat hello **- DataRoot** argument s cestou kořenové datové hello při použití příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-211">In addition toosetting hello environment variable, you can specify hello **-DataRoot** argument with hello data-root path when you're using a command line.</span></span> <span data-ttu-id="b8aaf-212">Tento argument přepíše vaše proměnná prostředí kořenové datové výchozí.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-212">This argument overwrites your default data-root environment variable.</span></span> <span data-ttu-id="b8aaf-213">Je nutné tooadd tento argument tooevery příkazového řádku, které používáte, takže můžete přepsat hello výchozí kořenové datové proměnnou prostředí pro všechny operace.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-213">You need tooadd this argument tooevery command line you're running so that you can overwrite hello default data-root environment variable for all operations.</span></span>

### <a name="sdk-command-line-usage-samples"></a><span data-ttu-id="b8aaf-214">Ukázky využití příkazový řádek SDK</span><span class="sxs-lookup"><span data-stu-id="b8aaf-214">SDK command line usage samples</span></span>

#### <a name="compile-and-run"></a><span data-ttu-id="b8aaf-215">Kompilace a spuštění</span><span class="sxs-lookup"><span data-stu-id="b8aaf-215">Compile and run</span></span>

<span data-ttu-id="b8aaf-216">Hello **spustit** pomocí příkazu toocompile hello skript a potom spusťte kompilované výsledky.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-216">hello **run** command is used toocompile hello script and then execute compiled results.</span></span> <span data-ttu-id="b8aaf-217">Jeho argumenty příkazového řádku jsou kombinaci těchto z **zkompilovat** a **provést**.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-217">Its command-line arguments are a combination of those from **compile** and **execute**.</span></span>

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

<span data-ttu-id="b8aaf-218">Hello následují volitelné argumenty pro **spustit**:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-218">hello following are optional arguments for **run**:</span></span>


|<span data-ttu-id="b8aaf-219">Argument</span><span class="sxs-lookup"><span data-stu-id="b8aaf-219">Argument</span></span>|<span data-ttu-id="b8aaf-220">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="b8aaf-220">Default value</span></span>|<span data-ttu-id="b8aaf-221">Popis</span><span class="sxs-lookup"><span data-stu-id="b8aaf-221">Description</span></span>|
|--------|-------------|-----------|
|<span data-ttu-id="b8aaf-222">-CodeBehind</span><span class="sxs-lookup"><span data-stu-id="b8aaf-222">-CodeBehind</span></span>|<span data-ttu-id="b8aaf-223">False</span><span class="sxs-lookup"><span data-stu-id="b8aaf-223">False</span></span>|<span data-ttu-id="b8aaf-224">skript Hello má .cs kódu na pozadí</span><span class="sxs-lookup"><span data-stu-id="b8aaf-224">hello script has .cs code behind</span></span>|
|<span data-ttu-id="b8aaf-225">-CppSDK</span><span class="sxs-lookup"><span data-stu-id="b8aaf-225">-CppSDK</span></span>| |<span data-ttu-id="b8aaf-226">CppSDK adresáře</span><span class="sxs-lookup"><span data-stu-id="b8aaf-226">CppSDK Directory</span></span>|
|<span data-ttu-id="b8aaf-227">-DataRoot</span><span class="sxs-lookup"><span data-stu-id="b8aaf-227">-DataRoot</span></span>| <span data-ttu-id="b8aaf-228">Proměnné prostředí DataRoot</span><span class="sxs-lookup"><span data-stu-id="b8aaf-228">DataRoot environment variable</span></span>|<span data-ttu-id="b8aaf-229">Výchozí DataRoot pro místní spuštění příliš 'LOCALRUN_DATAROOT' – Proměnná prostředí</span><span class="sxs-lookup"><span data-stu-id="b8aaf-229">DataRoot for local run, default too'LOCALRUN_DATAROOT' environment variable</span></span>|
|<span data-ttu-id="b8aaf-230">-MessageOut</span><span class="sxs-lookup"><span data-stu-id="b8aaf-230">-MessageOut</span></span>| |<span data-ttu-id="b8aaf-231">Výpis zprávy v souboru tooa konzoly</span><span class="sxs-lookup"><span data-stu-id="b8aaf-231">Dump messages on console tooa file</span></span>|
|<span data-ttu-id="b8aaf-232">-Paralelní</span><span class="sxs-lookup"><span data-stu-id="b8aaf-232">-Parallel</span></span>|<span data-ttu-id="b8aaf-233">1</span><span class="sxs-lookup"><span data-stu-id="b8aaf-233">1</span></span>|<span data-ttu-id="b8aaf-234">Spustit hello plán s hello zadaný paralelismus</span><span class="sxs-lookup"><span data-stu-id="b8aaf-234">Run hello plan with hello specified parallelism</span></span>|
|<span data-ttu-id="b8aaf-235">-Odkazy</span><span class="sxs-lookup"><span data-stu-id="b8aaf-235">-References</span></span>| |<span data-ttu-id="b8aaf-236">Seznam cest tooextra referenční sestavení nebo datové soubory kódu na pozadí, oddělených ';'</span><span class="sxs-lookup"><span data-stu-id="b8aaf-236">List of paths tooextra reference assemblies or data files of code behind, separated by ';'</span></span>|
|<span data-ttu-id="b8aaf-237">-UdoRedirect</span><span class="sxs-lookup"><span data-stu-id="b8aaf-237">-UdoRedirect</span></span>|<span data-ttu-id="b8aaf-238">False</span><span class="sxs-lookup"><span data-stu-id="b8aaf-238">False</span></span>|<span data-ttu-id="b8aaf-239">Generovat Udo sestavení přesměrování konfigurace</span><span class="sxs-lookup"><span data-stu-id="b8aaf-239">Generate Udo assembly redirect config</span></span>|
|<span data-ttu-id="b8aaf-240">-UseDatabase</span><span class="sxs-lookup"><span data-stu-id="b8aaf-240">-UseDatabase</span></span>|<span data-ttu-id="b8aaf-241">master</span><span class="sxs-lookup"><span data-stu-id="b8aaf-241">master</span></span>|<span data-ttu-id="b8aaf-242">Databáze toouse pro kód za registraci dočasné sestavení</span><span class="sxs-lookup"><span data-stu-id="b8aaf-242">Database toouse for code behind temporary assembly registration</span></span>|
|<span data-ttu-id="b8aaf-243">-Verbose</span><span class="sxs-lookup"><span data-stu-id="b8aaf-243">-Verbose</span></span>|<span data-ttu-id="b8aaf-244">False</span><span class="sxs-lookup"><span data-stu-id="b8aaf-244">False</span></span>|<span data-ttu-id="b8aaf-245">Zobrazit podrobné výstupy z modulu runtime</span><span class="sxs-lookup"><span data-stu-id="b8aaf-245">Show detailed outputs from runtime</span></span>|
|<span data-ttu-id="b8aaf-246">-WorkDir</span><span class="sxs-lookup"><span data-stu-id="b8aaf-246">-WorkDir</span></span>|<span data-ttu-id="b8aaf-247">Aktuální adresář</span><span class="sxs-lookup"><span data-stu-id="b8aaf-247">Current Directory</span></span>|<span data-ttu-id="b8aaf-248">Adresář pro využití kompilátoru a výstupy</span><span class="sxs-lookup"><span data-stu-id="b8aaf-248">Directory for compiler usage and outputs</span></span>|
|<span data-ttu-id="b8aaf-249">-RunScopeCEP</span><span class="sxs-lookup"><span data-stu-id="b8aaf-249">-RunScopeCEP</span></span>|<span data-ttu-id="b8aaf-250">0</span><span class="sxs-lookup"><span data-stu-id="b8aaf-250">0</span></span>|<span data-ttu-id="b8aaf-251">Toouse ScopeCEP režimu</span><span class="sxs-lookup"><span data-stu-id="b8aaf-251">ScopeCEP mode toouse</span></span>|
|<span data-ttu-id="b8aaf-252">-ScopeCEPTempPath</span><span class="sxs-lookup"><span data-stu-id="b8aaf-252">-ScopeCEPTempPath</span></span>|<span data-ttu-id="b8aaf-253">dočasné</span><span class="sxs-lookup"><span data-stu-id="b8aaf-253">temp</span></span>|<span data-ttu-id="b8aaf-254">Dočasná cesta toouse pro datový proud</span><span class="sxs-lookup"><span data-stu-id="b8aaf-254">Temp path toouse for streaming data</span></span>|
|<span data-ttu-id="b8aaf-255">-OptFlags</span><span class="sxs-lookup"><span data-stu-id="b8aaf-255">-OptFlags</span></span>| |<span data-ttu-id="b8aaf-256">Seznam oddělený čárkami Optimalizátor příznaky</span><span class="sxs-lookup"><span data-stu-id="b8aaf-256">Comma-separated list of optimizer flags</span></span>|


<span data-ttu-id="b8aaf-257">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-257">Here's an example:</span></span>

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

<span data-ttu-id="b8aaf-258">Kromě toho kombinování **zkompilovat** a **provést**, můžete zkompilovat a spustit samostatně, spustitelné soubory hello zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-258">Besides combining **compile** and **execute**, you can compile and execute hello compiled executables separately.</span></span>

#### <a name="compile-a-u-sql-script"></a><span data-ttu-id="b8aaf-259">Kompilace skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="b8aaf-259">Compile a U-SQL script</span></span>

<span data-ttu-id="b8aaf-260">Hello **zkompilovat** příkaz je skript tooexecutables použité toocompile U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-260">hello **compile** command is used toocompile a U-SQL script tooexecutables.</span></span>

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

<span data-ttu-id="b8aaf-261">Hello následují volitelné argumenty pro **zkompilovat**:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-261">hello following are optional arguments for **compile**:</span></span>


|<span data-ttu-id="b8aaf-262">Argument</span><span class="sxs-lookup"><span data-stu-id="b8aaf-262">Argument</span></span>|<span data-ttu-id="b8aaf-263">Popis</span><span class="sxs-lookup"><span data-stu-id="b8aaf-263">Description</span></span>|
|--------|-----------|
| <span data-ttu-id="b8aaf-264">-CodeBehind [výchozí hodnotu "False"]</span><span class="sxs-lookup"><span data-stu-id="b8aaf-264">-CodeBehind [default value 'False']</span></span>|<span data-ttu-id="b8aaf-265">skript Hello má .cs kódu na pozadí</span><span class="sxs-lookup"><span data-stu-id="b8aaf-265">hello script has .cs code behind</span></span>|
| <span data-ttu-id="b8aaf-266">-CppSDK [výchozí hodnota:]</span><span class="sxs-lookup"><span data-stu-id="b8aaf-266">-CppSDK [default value '']</span></span>|<span data-ttu-id="b8aaf-267">CppSDK adresáře</span><span class="sxs-lookup"><span data-stu-id="b8aaf-267">CppSDK Directory</span></span>|
| <span data-ttu-id="b8aaf-268">-DataRoot [výchozí hodnota "Proměnné prostředí DataRoot"]</span><span class="sxs-lookup"><span data-stu-id="b8aaf-268">-DataRoot [default value 'DataRoot environment variable']</span></span>|<span data-ttu-id="b8aaf-269">Výchozí DataRoot pro místní spuštění příliš 'LOCALRUN_DATAROOT' – Proměnná prostředí</span><span class="sxs-lookup"><span data-stu-id="b8aaf-269">DataRoot for local run, default too'LOCALRUN_DATAROOT' environment variable</span></span>|
| <span data-ttu-id="b8aaf-270">-MessageOut [výchozí hodnota:]</span><span class="sxs-lookup"><span data-stu-id="b8aaf-270">-MessageOut [default value '']</span></span>|<span data-ttu-id="b8aaf-271">Výpis zprávy v souboru tooa konzoly</span><span class="sxs-lookup"><span data-stu-id="b8aaf-271">Dump messages on console tooa file</span></span>|
| <span data-ttu-id="b8aaf-272">-Odkazuje [výchozí hodnota:]</span><span class="sxs-lookup"><span data-stu-id="b8aaf-272">-References [default value '']</span></span>|<span data-ttu-id="b8aaf-273">Seznam cest tooextra referenční sestavení nebo datové soubory kódu na pozadí, oddělených ';'</span><span class="sxs-lookup"><span data-stu-id="b8aaf-273">List of paths tooextra reference assemblies or data files of code behind, separated by ';'</span></span>|
| <span data-ttu-id="b8aaf-274">-Malou [výchozí hodnotu "False"]</span><span class="sxs-lookup"><span data-stu-id="b8aaf-274">-Shallow [default value 'False']</span></span>|<span data-ttu-id="b8aaf-275">Bez podstruktury kompilace</span><span class="sxs-lookup"><span data-stu-id="b8aaf-275">Shallow compile</span></span>|
| <span data-ttu-id="b8aaf-276">-UdoRedirect [výchozí hodnotu "False"]</span><span class="sxs-lookup"><span data-stu-id="b8aaf-276">-UdoRedirect [default value 'False']</span></span>|<span data-ttu-id="b8aaf-277">Generovat Udo sestavení přesměrování konfigurace</span><span class="sxs-lookup"><span data-stu-id="b8aaf-277">Generate Udo assembly redirect config</span></span>|
| <span data-ttu-id="b8aaf-278">-UseDatabase [výchozí hodnota 'hlavní']</span><span class="sxs-lookup"><span data-stu-id="b8aaf-278">-UseDatabase [default value 'master']</span></span>|<span data-ttu-id="b8aaf-279">Databáze toouse pro kód za registraci dočasné sestavení</span><span class="sxs-lookup"><span data-stu-id="b8aaf-279">Database toouse for code behind temporary assembly registration</span></span>|
| <span data-ttu-id="b8aaf-280">-WorkDir [výchozí hodnotu 'aktuální adresář.]</span><span class="sxs-lookup"><span data-stu-id="b8aaf-280">-WorkDir [default value 'Current Directory']</span></span>|<span data-ttu-id="b8aaf-281">Adresář pro využití kompilátoru a výstupy</span><span class="sxs-lookup"><span data-stu-id="b8aaf-281">Directory for compiler usage and outputs</span></span>|
| <span data-ttu-id="b8aaf-282">-RunScopeCEP [výchozí hodnota '0']</span><span class="sxs-lookup"><span data-stu-id="b8aaf-282">-RunScopeCEP [default value '0']</span></span>|<span data-ttu-id="b8aaf-283">Toouse ScopeCEP režimu</span><span class="sxs-lookup"><span data-stu-id="b8aaf-283">ScopeCEP mode toouse</span></span>|
| <span data-ttu-id="b8aaf-284">-ScopeCEPTempPath [výchozí hodnota 'temp']</span><span class="sxs-lookup"><span data-stu-id="b8aaf-284">-ScopeCEPTempPath [default value 'temp']</span></span>|<span data-ttu-id="b8aaf-285">Dočasná cesta toouse pro datový proud</span><span class="sxs-lookup"><span data-stu-id="b8aaf-285">Temp path toouse for streaming data</span></span>|
| <span data-ttu-id="b8aaf-286">-OptFlags [výchozí hodnota:]</span><span class="sxs-lookup"><span data-stu-id="b8aaf-286">-OptFlags [default value '']</span></span>|<span data-ttu-id="b8aaf-287">Seznam oddělený čárkami Optimalizátor příznaky</span><span class="sxs-lookup"><span data-stu-id="b8aaf-287">Comma-separated list of optimizer flags</span></span>|


<span data-ttu-id="b8aaf-288">Tady jsou některé příklady použití.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-288">Here are some usage examples.</span></span>

<span data-ttu-id="b8aaf-289">Kompilace skript U-SQL:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-289">Compile a U-SQL script:</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql

<span data-ttu-id="b8aaf-290">Kompilace skript U-SQL a nastavte hello kořenové datové složce.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-290">Compile a U-SQL script and set hello data-root folder.</span></span> <span data-ttu-id="b8aaf-291">Všimněte si, že tato akce přepíše proměnnou prostředí sady hello.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-291">Note that this will overwrite hello set environment variable.</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

<span data-ttu-id="b8aaf-292">Kompilace skript U-SQL a nastavte pracovní adresář, referenční sestavení a databáze:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-292">Compile a U-SQL script and set a working directory, reference assembly, and database:</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-results"></a><span data-ttu-id="b8aaf-293">Spuštění kompilované výsledky</span><span class="sxs-lookup"><span data-stu-id="b8aaf-293">Execute compiled results</span></span>

<span data-ttu-id="b8aaf-294">Hello **provést** příkaz je použité tooexecute zkompilovat výsledky.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-294">hello **execute** command is used tooexecute compiled results.</span></span>   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

<span data-ttu-id="b8aaf-295">Hello následují volitelné argumenty pro **provést**:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-295">hello following are optional arguments for **execute**:</span></span>

|<span data-ttu-id="b8aaf-296">Argument</span><span class="sxs-lookup"><span data-stu-id="b8aaf-296">Argument</span></span>|<span data-ttu-id="b8aaf-297">Popis</span><span class="sxs-lookup"><span data-stu-id="b8aaf-297">Description</span></span>|
|--------|-----------|
|<span data-ttu-id="b8aaf-298">-DataRoot [výchozí hodnota:]</span><span class="sxs-lookup"><span data-stu-id="b8aaf-298">-DataRoot [default value '']</span></span>|<span data-ttu-id="b8aaf-299">Kořenové datové pro provedení metadat.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-299">Data root for metadata execution.</span></span> <span data-ttu-id="b8aaf-300">Výchozí hodnota toohello **LOCALRUN_DATAROOT** proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-300">It defaults toohello **LOCALRUN_DATAROOT** environment variable.</span></span>|
|<span data-ttu-id="b8aaf-301">-MessageOut [výchozí hodnota:]</span><span class="sxs-lookup"><span data-stu-id="b8aaf-301">-MessageOut [default value '']</span></span>|<span data-ttu-id="b8aaf-302">Dump zprávy v souboru tooa hello konzoly.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-302">Dump messages on hello console tooa file.</span></span>|
|<span data-ttu-id="b8aaf-303">-Paralelní [výchozí hodnotu 1.]</span><span class="sxs-lookup"><span data-stu-id="b8aaf-303">-Parallel [default value '1']</span></span>|<span data-ttu-id="b8aaf-304">Indikátor toorun hello generované místní spuštění kroky s hello zadat úroveň stupně paralelního zpracování.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-304">Indicator toorun hello generated local-run steps with hello specified parallelism level.</span></span>|
|<span data-ttu-id="b8aaf-305">-Verbose [výchozí hodnotu "False"]</span><span class="sxs-lookup"><span data-stu-id="b8aaf-305">-Verbose [default value 'False']</span></span>|<span data-ttu-id="b8aaf-306">Indikátor tooshow podrobné výstupy z modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-306">Indicator tooshow detailed outputs from runtime.</span></span>|

<span data-ttu-id="b8aaf-307">Tady je příklad použití:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-307">Here's a usage example:</span></span>

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5


## <a name="use-hello-sdk-with-programming-interfaces"></a><span data-ttu-id="b8aaf-308">Hello SDK pomocí programovacího rozhraní</span><span class="sxs-lookup"><span data-stu-id="b8aaf-308">Use hello SDK with programming interfaces</span></span>

<span data-ttu-id="b8aaf-309">programovací rozhraní Hello se nacházejí v hello LocalRunHelper.exe.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-309">hello programming interfaces are all located in hello LocalRunHelper.exe.</span></span> <span data-ttu-id="b8aaf-310">Můžete používat funkce hello toointegrate hello SDK U-SQL a hello C# test framework tooscale svůj test místní skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-310">You can use them toointegrate hello functionality of hello U-SQL SDK and hello C# test framework tooscale your U-SQL script local test.</span></span> <span data-ttu-id="b8aaf-311">V tomto článku použiji hello standardní C# jednotky testovacího projektu tooshow jak toouse těchto rozhraní tootest váš skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-311">In this article, I will use hello standard C# unit test project tooshow how toouse these interfaces tootest your U-SQL script.</span></span>

### <a name="step-1-create-c-unit-test-project-and-configuration"></a><span data-ttu-id="b8aaf-312">Krok 1: Vytvoření projektu testování částí C# a konfigurace</span><span class="sxs-lookup"><span data-stu-id="b8aaf-312">Step 1: Create C# unit test project and configuration</span></span>

- <span data-ttu-id="b8aaf-313">Vytvoření projektu testování částí C# prostřednictvím soubor > Nový > Projekt > Visual C# > testovací > projektu testování částí.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-313">Create a C# unit test project through File > New > Project > Visual C# > Test > Unit Test Project.</span></span>
- <span data-ttu-id="b8aaf-314">Přidejte LocalRunHelper.exe jako odkaz pro projekt hello.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-314">Add LocalRunHelper.exe as a reference for hello project.</span></span> <span data-ttu-id="b8aaf-315">Hello LocalRunHelper.exe se nachází v \build\runtime\LocalRunHelper.exe v balíčku Nuget.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-315">hello LocalRunHelper.exe is located at \build\runtime\LocalRunHelper.exe in Nuget package.</span></span>

    ![Přidat odkaz na sadu SDK Azure Data Lake U-SQL](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-add-reference.png)

- <span data-ttu-id="b8aaf-317">U-SQL SDK **pouze** podporu x64 prostředí, ujistěte se, tooset sestavení Cílová platforma jako x64.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-317">U-SQL SDK **only** support x64 environment, make sure tooset build platform target as x64.</span></span> <span data-ttu-id="b8aaf-318">Můžete nastavit, prostřednictvím vlastnosti projektu > sestavení > Cílová platforma.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-318">You can set that through Project Property > Build > Platform target.</span></span>

    ![Azure Data Lake U-SQL SDK konfigurace x64 projektu](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-x64.png)

- <span data-ttu-id="b8aaf-320">Ujistěte se, že tooset testovacího prostředí jako x64.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-320">Make sure tooset your test environment as x64.</span></span> <span data-ttu-id="b8aaf-321">V sadě Visual Studio, můžete ho nastavit prostřednictvím testovací > Test nastavení > výchozí architekturu procesoru > x64.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-321">In Visual Studio, you can set it through Test > Test Settings > Default Processor Architecture > x64.</span></span>

    ![Azure Data Lake U-SQL SDK konfigurace x64 testovacího prostředí](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-test-x64.png)

- <span data-ttu-id="b8aaf-323">Ujistěte se, že toocopy všechny soubory závislosti v rámci NugetPackage\build\runtime\ tooproject pracovní adresář, který je obvykle pod ProjectFolder\bin\x64\Debug.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-323">Make sure toocopy all dependency files under NugetPackage\build\runtime\ tooproject working directory which is usually under ProjectFolder\bin\x64\Debug.</span></span>

### <a name="step-2-create-u-sql-script-test-case"></a><span data-ttu-id="b8aaf-324">Krok 2: Vytvoření testovacího případu skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="b8aaf-324">Step 2: Create U-SQL script test case</span></span>

<span data-ttu-id="b8aaf-325">Níže je hello ukázkový kód pro test skriptu U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-325">Below is hello sample code for U-SQL script test.</span></span> <span data-ttu-id="b8aaf-326">Pro testování, je nutné tooprepare skripty, soubory očekávaný výstup a vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-326">For testing, you need tooprepare scripts, input files and expected output files.</span></span>

    using System;
    using Microsoft.VisualStudio.TestTools.UnitTesting;
    using System.IO;
    using System.Text;
    using System.Security.Cryptography;
    using Microsoft.Analytics.LocalRun;

    namespace UnitTestProject1
    {
        [TestClass]
        public class USQLUnitTest
        {
            [TestMethod]
            public void TestUSQLScript()
            {
                //Specify hello local run message output path
                StreamWriter MessageOutput = new StreamWriter("../../../log.txt");

                LocalRunHelper localrun = new LocalRunHelper(MessageOutput);

                //Configure hello DateRoot path, Script Path and CPPSDK path
                localrun.DataRoot = "../../../";
                localrun.ScriptPath = "../../../Script/Script.usql";
                localrun.CppSdkDir = "../../../CppSDK";

                //Run U-SQL script
                localrun.DoRun();

                //Script output 
                string Result = Path.Combine(localrun.DataRoot, "Output/result.csv");

                //Expected script output
                string ExpectedResult = "../../../ExpectedOutput/result.csv";

                Test.Helpers.FileAssert.AreEqual(Result, ExpectedResult);

                //Don't forget tooclose MessageOutput tooget logs into file
                MessageOutput.Close();
            }
        }
    }

    namespace Test.Helpers
    {
        public static class FileAssert
        {
            static string GetFileHash(string filename)
            {
                Assert.IsTrue(File.Exists(filename));

                using (var hash = new SHA1Managed())
                {
                    var clearBytes = File.ReadAllBytes(filename);
                    var hashedBytes = hash.ComputeHash(clearBytes);
                    return ConvertBytesToHex(hashedBytes);
                }
            }

            static string ConvertBytesToHex(byte[] bytes)
            {
                var sb = new StringBuilder();

                for (var i = 0; i < bytes.Length; i++)
                {
                    sb.Append(bytes[i].ToString("x"));
                }
                return sb.ToString();
            }

            public static void AreEqual(string filename1, string filename2)
            {
                string hash1 = GetFileHash(filename1);
                string hash2 = GetFileHash(filename2);

                Assert.AreEqual(hash1, hash2);
            }
        }
    }


### <a name="programming-interfaces-in-localrunhelperexe"></a><span data-ttu-id="b8aaf-327">Programování rozhraní LocalRunHelper.exe</span><span class="sxs-lookup"><span data-stu-id="b8aaf-327">Programming interfaces in LocalRunHelper.exe</span></span>

<span data-ttu-id="b8aaf-328">LocalRunHelper.exe poskytuje programovací rozhraní pro místní kompilace U-SQL, spusťte hello, jsou uvedeny v následujícím seznamu rozhraní hello atd.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-328">LocalRunHelper.exe provides hello programming interfaces for U-SQL local compile, run, etc. hello interfaces are listed as follows.</span></span>

<span data-ttu-id="b8aaf-329">**Konstruktor**</span><span class="sxs-lookup"><span data-stu-id="b8aaf-329">**Constructor**</span></span>

<span data-ttu-id="b8aaf-330">veřejné LocalRunHelper ([System.IO.TextWriter messageOutput = null])</span><span class="sxs-lookup"><span data-stu-id="b8aaf-330">public LocalRunHelper([System.IO.TextWriter messageOutput = null])</span></span>

|<span data-ttu-id="b8aaf-331">Parametr</span><span class="sxs-lookup"><span data-stu-id="b8aaf-331">Parameter</span></span>|<span data-ttu-id="b8aaf-332">Typ</span><span class="sxs-lookup"><span data-stu-id="b8aaf-332">Type</span></span>|<span data-ttu-id="b8aaf-333">Popis</span><span class="sxs-lookup"><span data-stu-id="b8aaf-333">Description</span></span>|
|---------|----|-----------|
|<span data-ttu-id="b8aaf-334">messageOutput</span><span class="sxs-lookup"><span data-stu-id="b8aaf-334">messageOutput</span></span>|<span data-ttu-id="b8aaf-335">System.IO.TextWriter</span><span class="sxs-lookup"><span data-stu-id="b8aaf-335">System.IO.TextWriter</span></span>|<span data-ttu-id="b8aaf-336">pro výstup zprávy nastavte toonull toouse konzoly</span><span class="sxs-lookup"><span data-stu-id="b8aaf-336">for output messages, set toonull toouse Console</span></span>|

<span data-ttu-id="b8aaf-337">**Vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="b8aaf-337">**Properties**</span></span>

|<span data-ttu-id="b8aaf-338">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b8aaf-338">Property</span></span>|<span data-ttu-id="b8aaf-339">Typ</span><span class="sxs-lookup"><span data-stu-id="b8aaf-339">Type</span></span>|<span data-ttu-id="b8aaf-340">Popis</span><span class="sxs-lookup"><span data-stu-id="b8aaf-340">Description</span></span>|
|--------|----|-----------|
|<span data-ttu-id="b8aaf-341">AlgebraPath</span><span class="sxs-lookup"><span data-stu-id="b8aaf-341">AlgebraPath</span></span>|<span data-ttu-id="b8aaf-342">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b8aaf-342">string</span></span>|<span data-ttu-id="b8aaf-343">Hello cesta tooalgebra soubor (soubor algebra je jedním z výsledky kompilace hello)</span><span class="sxs-lookup"><span data-stu-id="b8aaf-343">hello path tooalgebra file (algebra file is one of hello compilation results)</span></span>|
|<span data-ttu-id="b8aaf-344">CodeBehindReferences</span><span class="sxs-lookup"><span data-stu-id="b8aaf-344">CodeBehindReferences</span></span>|<span data-ttu-id="b8aaf-345">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b8aaf-345">string</span></span>|<span data-ttu-id="b8aaf-346">Pokud skript hello další kódu na pozadí odkazy, zadejte hello cesty odděleny znakem ";"</span><span class="sxs-lookup"><span data-stu-id="b8aaf-346">If hello script has additional code behind references, specify hello paths separated with ';'</span></span>|
|<span data-ttu-id="b8aaf-347">CppSdkDir</span><span class="sxs-lookup"><span data-stu-id="b8aaf-347">CppSdkDir</span></span>|<span data-ttu-id="b8aaf-348">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b8aaf-348">string</span></span>|<span data-ttu-id="b8aaf-349">CppSDK adresáře</span><span class="sxs-lookup"><span data-stu-id="b8aaf-349">CppSDK directory</span></span>|
|<span data-ttu-id="b8aaf-350">CurrentDir</span><span class="sxs-lookup"><span data-stu-id="b8aaf-350">CurrentDir</span></span>|<span data-ttu-id="b8aaf-351">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b8aaf-351">string</span></span>|<span data-ttu-id="b8aaf-352">Aktuální adresář</span><span class="sxs-lookup"><span data-stu-id="b8aaf-352">Current directory</span></span>|
|<span data-ttu-id="b8aaf-353">DataRoot</span><span class="sxs-lookup"><span data-stu-id="b8aaf-353">DataRoot</span></span>|<span data-ttu-id="b8aaf-354">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b8aaf-354">string</span></span>|<span data-ttu-id="b8aaf-355">Kořenová cesta dat</span><span class="sxs-lookup"><span data-stu-id="b8aaf-355">Data root path</span></span>|
|<span data-ttu-id="b8aaf-356">DebuggerMailPath</span><span class="sxs-lookup"><span data-stu-id="b8aaf-356">DebuggerMailPath</span></span>|<span data-ttu-id="b8aaf-357">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b8aaf-357">string</span></span>|<span data-ttu-id="b8aaf-358">zásuvky Hello cesta toodebugger pošty</span><span class="sxs-lookup"><span data-stu-id="b8aaf-358">hello path toodebugger mailslot</span></span>|
|<span data-ttu-id="b8aaf-359">GenerateUdoRedirect</span><span class="sxs-lookup"><span data-stu-id="b8aaf-359">GenerateUdoRedirect</span></span>|<span data-ttu-id="b8aaf-360">BOOL</span><span class="sxs-lookup"><span data-stu-id="b8aaf-360">bool</span></span>|<span data-ttu-id="b8aaf-361">Pokud chceme načítání sestavení toogenerate přesměrování přepsat konfigurace</span><span class="sxs-lookup"><span data-stu-id="b8aaf-361">If we want toogenerate assembly loading redirection override config</span></span>|
|<span data-ttu-id="b8aaf-362">HasCodeBehind</span><span class="sxs-lookup"><span data-stu-id="b8aaf-362">HasCodeBehind</span></span>|<span data-ttu-id="b8aaf-363">BOOL</span><span class="sxs-lookup"><span data-stu-id="b8aaf-363">bool</span></span>|<span data-ttu-id="b8aaf-364">Pokud má skript hello kódu na pozadí</span><span class="sxs-lookup"><span data-stu-id="b8aaf-364">If hello script has code behind</span></span>|
|<span data-ttu-id="b8aaf-365">InputDir</span><span class="sxs-lookup"><span data-stu-id="b8aaf-365">InputDir</span></span>|<span data-ttu-id="b8aaf-366">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b8aaf-366">string</span></span>|<span data-ttu-id="b8aaf-367">Adresář pro vstupní data</span><span class="sxs-lookup"><span data-stu-id="b8aaf-367">Directory for input data</span></span>|
|<span data-ttu-id="b8aaf-368">MessagePath</span><span class="sxs-lookup"><span data-stu-id="b8aaf-368">MessagePath</span></span>|<span data-ttu-id="b8aaf-369">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b8aaf-369">string</span></span>|<span data-ttu-id="b8aaf-370">Cesta k souboru výpisu zpráv</span><span class="sxs-lookup"><span data-stu-id="b8aaf-370">Message dump file path</span></span>|
|<span data-ttu-id="b8aaf-371">OutputDir</span><span class="sxs-lookup"><span data-stu-id="b8aaf-371">OutputDir</span></span>|<span data-ttu-id="b8aaf-372">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b8aaf-372">string</span></span>|<span data-ttu-id="b8aaf-373">Adresář pro výstupní data</span><span class="sxs-lookup"><span data-stu-id="b8aaf-373">Directory for output data</span></span>|
|<span data-ttu-id="b8aaf-374">Paralelismus</span><span class="sxs-lookup"><span data-stu-id="b8aaf-374">Parallelism</span></span>|<span data-ttu-id="b8aaf-375">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b8aaf-375">int</span></span>|<span data-ttu-id="b8aaf-376">Algebra hello toorun paralelismus</span><span class="sxs-lookup"><span data-stu-id="b8aaf-376">Parallelism toorun hello algebra</span></span>|
|<span data-ttu-id="b8aaf-377">ParentPid</span><span class="sxs-lookup"><span data-stu-id="b8aaf-377">ParentPid</span></span>|<span data-ttu-id="b8aaf-378">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b8aaf-378">int</span></span>|<span data-ttu-id="b8aaf-379">ID nadřazené hello, na které hello služba monitoruje tooexit, too0 sady nebo záporné tooignore</span><span class="sxs-lookup"><span data-stu-id="b8aaf-379">PID of hello parent on which hello service monitors tooexit, set too0 or negative tooignore</span></span>|
|<span data-ttu-id="b8aaf-380">ResultPath</span><span class="sxs-lookup"><span data-stu-id="b8aaf-380">ResultPath</span></span>|<span data-ttu-id="b8aaf-381">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b8aaf-381">string</span></span>|<span data-ttu-id="b8aaf-382">Cesta k souboru výpisu výsledek</span><span class="sxs-lookup"><span data-stu-id="b8aaf-382">Result dump file path</span></span>|
|<span data-ttu-id="b8aaf-383">RuntimeDir</span><span class="sxs-lookup"><span data-stu-id="b8aaf-383">RuntimeDir</span></span>|<span data-ttu-id="b8aaf-384">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b8aaf-384">string</span></span>|<span data-ttu-id="b8aaf-385">Adresář modulu runtime</span><span class="sxs-lookup"><span data-stu-id="b8aaf-385">Runtime directory</span></span>|
|<span data-ttu-id="b8aaf-386">scriptPath</span><span class="sxs-lookup"><span data-stu-id="b8aaf-386">ScriptPath</span></span>|<span data-ttu-id="b8aaf-387">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b8aaf-387">string</span></span>|<span data-ttu-id="b8aaf-388">Kde toofind hello skriptu</span><span class="sxs-lookup"><span data-stu-id="b8aaf-388">Where toofind hello script</span></span>|
|<span data-ttu-id="b8aaf-389">Bez podstruktury</span><span class="sxs-lookup"><span data-stu-id="b8aaf-389">Shallow</span></span>|<span data-ttu-id="b8aaf-390">BOOL</span><span class="sxs-lookup"><span data-stu-id="b8aaf-390">bool</span></span>|<span data-ttu-id="b8aaf-391">Nedávná kompilace, nebo ne</span><span class="sxs-lookup"><span data-stu-id="b8aaf-391">Shallow compile or not</span></span>|
|<span data-ttu-id="b8aaf-392">TempDir</span><span class="sxs-lookup"><span data-stu-id="b8aaf-392">TempDir</span></span>|<span data-ttu-id="b8aaf-393">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b8aaf-393">string</span></span>|<span data-ttu-id="b8aaf-394">Dočasný adresář</span><span class="sxs-lookup"><span data-stu-id="b8aaf-394">Temp directory</span></span>|
|<span data-ttu-id="b8aaf-395">UseDataBase</span><span class="sxs-lookup"><span data-stu-id="b8aaf-395">UseDataBase</span></span>|<span data-ttu-id="b8aaf-396">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b8aaf-396">string</span></span>|<span data-ttu-id="b8aaf-397">Zadejte hello toouse databáze pro kódu na pozadí dočasné sestavení registrace, hlavní ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="b8aaf-397">Specify hello database toouse for code behind temporary assembly registration, master by default</span></span>|
|<span data-ttu-id="b8aaf-398">WorkDir</span><span class="sxs-lookup"><span data-stu-id="b8aaf-398">WorkDir</span></span>|<span data-ttu-id="b8aaf-399">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b8aaf-399">string</span></span>|<span data-ttu-id="b8aaf-400">Upřednostňované pracovní adresář</span><span class="sxs-lookup"><span data-stu-id="b8aaf-400">Preferred working directory</span></span>|


<span data-ttu-id="b8aaf-401">**– Metoda**</span><span class="sxs-lookup"><span data-stu-id="b8aaf-401">**Method**</span></span>

|<span data-ttu-id="b8aaf-402">Metoda</span><span class="sxs-lookup"><span data-stu-id="b8aaf-402">Method</span></span>|<span data-ttu-id="b8aaf-403">Popis</span><span class="sxs-lookup"><span data-stu-id="b8aaf-403">Description</span></span>|<span data-ttu-id="b8aaf-404">Vrátí</span><span class="sxs-lookup"><span data-stu-id="b8aaf-404">Return</span></span>|<span data-ttu-id="b8aaf-405">Parametr</span><span class="sxs-lookup"><span data-stu-id="b8aaf-405">Parameter</span></span>|
|------|-----------|------|---------|
|<span data-ttu-id="b8aaf-406">veřejné bool DoCompile()</span><span class="sxs-lookup"><span data-stu-id="b8aaf-406">public bool DoCompile()</span></span>|<span data-ttu-id="b8aaf-407">Kompilace hello U-SQL skriptů</span><span class="sxs-lookup"><span data-stu-id="b8aaf-407">Compile hello U-SQL script</span></span>|<span data-ttu-id="b8aaf-408">Hodnota TRUE, v případě úspěchu</span><span class="sxs-lookup"><span data-stu-id="b8aaf-408">True on success</span></span>| |
|<span data-ttu-id="b8aaf-409">veřejné bool DoExec()</span><span class="sxs-lookup"><span data-stu-id="b8aaf-409">public bool DoExec()</span></span>|<span data-ttu-id="b8aaf-410">Hello zkompilovat výsledek spuštění</span><span class="sxs-lookup"><span data-stu-id="b8aaf-410">Execute hello compiled result</span></span>|<span data-ttu-id="b8aaf-411">Hodnota TRUE, v případě úspěchu</span><span class="sxs-lookup"><span data-stu-id="b8aaf-411">True on success</span></span>| |
|<span data-ttu-id="b8aaf-412">veřejné bool DoRun()</span><span class="sxs-lookup"><span data-stu-id="b8aaf-412">public bool DoRun()</span></span>|<span data-ttu-id="b8aaf-413">Spusťte skript hello U-SQL (kompilace + spouštění)</span><span class="sxs-lookup"><span data-stu-id="b8aaf-413">Run hello U-SQL script (Compile + Execute)</span></span>|<span data-ttu-id="b8aaf-414">Hodnota TRUE, v případě úspěchu</span><span class="sxs-lookup"><span data-stu-id="b8aaf-414">True on success</span></span>| |
|<span data-ttu-id="b8aaf-415">veřejné bool IsValidRuntimeDir (cesta řetězec)</span><span class="sxs-lookup"><span data-stu-id="b8aaf-415">public bool IsValidRuntimeDir(string path)</span></span>|<span data-ttu-id="b8aaf-416">Zkontrolujte, jestli hello zadána cesta je cesta platná runtime</span><span class="sxs-lookup"><span data-stu-id="b8aaf-416">Check if hello given path is valid runtime path</span></span>|<span data-ttu-id="b8aaf-417">Platí pro platný</span><span class="sxs-lookup"><span data-stu-id="b8aaf-417">True for valid</span></span>|<span data-ttu-id="b8aaf-418">Cesta Hello adresář modulu runtime</span><span class="sxs-lookup"><span data-stu-id="b8aaf-418">hello path of runtime directory</span></span>|


## <a name="faq-about-common-issue"></a><span data-ttu-id="b8aaf-419">Nejčastější dotazy o běžné problémy</span><span class="sxs-lookup"><span data-stu-id="b8aaf-419">FAQ about common issue</span></span>

### <a name="error-1"></a><span data-ttu-id="b8aaf-420">1. Chyba:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-420">Error 1:</span></span>
<span data-ttu-id="b8aaf-421">E_CSC_SYSTEM_INTERNAL: Vnitřní chyba!</span><span class="sxs-lookup"><span data-stu-id="b8aaf-421">E_CSC_SYSTEM_INTERNAL: Internal error!</span></span> <span data-ttu-id="b8aaf-422">Nepodařilo se načíst soubor nebo sestavení 'ScopeEngineManaged.dll nebo jednu ze závislostí.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-422">Could not load file or assembly 'ScopeEngineManaged.dll' or one of its dependencies.</span></span> <span data-ttu-id="b8aaf-423">Hello zadaný modul nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-423">hello specified module could not be found.</span></span>

<span data-ttu-id="b8aaf-424">Zkontrolujte, zda text hello následující:</span><span class="sxs-lookup"><span data-stu-id="b8aaf-424">Please check hello following:</span></span>

- <span data-ttu-id="b8aaf-425">Zajistěte, aby byla x64 prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-425">Make sure you have x64 environment.</span></span> <span data-ttu-id="b8aaf-426">Cílová platforma Hello sestavení a hello testovací prostředí by měl být x64, najdete v příliš**krok 1: vytvoření C# jednotkové testování projektu a konfigurace** výše.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-426">hello build target platform and hello test environment should be x64, refer too**Step 1: Create C# unit test project and configuration** above.</span></span>
- <span data-ttu-id="b8aaf-427">Ujistěte se, že jste zkopírovali všechny soubory závislosti v rámci NugetPackage\build\runtime\ tooproject pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="b8aaf-427">Make sure you have copied all dependency files under NugetPackage\build\runtime\ tooproject working directory.</span></span>


## <a name="next-steps"></a><span data-ttu-id="b8aaf-428">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8aaf-428">Next steps</span></span>

* <span data-ttu-id="b8aaf-429">toolearn U-SQL, najdete v části [Začínáme s jazykem Azure Data Lake Analytics U-SQL](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b8aaf-429">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="b8aaf-430">toolog diagnostické informace, najdete v části [přístup k protokolů diagnostiky pro Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="b8aaf-430">toolog diagnostics information, see [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).</span></span>
* <span data-ttu-id="b8aaf-431">toosee komplexnější dotaz, najdete v části [analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="b8aaf-431">toosee a more complex query, see [Analyze website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="b8aaf-432">Podrobnosti úlohy tooview, najdete v tématu [použití úlohy prohlížeče a zobrazení úloh pro úlohy Azure Data Lake Analytics](data-lake-analytics-data-lake-tools-view-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="b8aaf-432">tooview job details, see [Use Job Browser and Job View for Azure Data Lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md).</span></span>
* <span data-ttu-id="b8aaf-433">zobrazení provádění vrcholů toouse hello, najdete v části [použití hello nebo zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span><span class="sxs-lookup"><span data-stu-id="b8aaf-433">toouse hello vertex execution view, see [Use hello Vertex Execution View in Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span></span>
