---
title: "Škálování U-SQL místní spuštění a testování pomocí Azure Data Lake U-SQL SDK | Microsoft Docs"
description: "Zjistěte, jak používat sadu SDK Azure Data Lake U-SQL k místní úlohy škálování U-SQL, spusťte a testování pomocí příkazového řádku a programovací rozhraní na místní pracovní stanici."
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
ms.openlocfilehash: 55242bcf644ca0e7f30cfe7eada2130451c36e64
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scale-u-sql-local-run-and-test-with-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="f7e05-103">Škálování U-SQL místní spuštění a testování pomocí Azure Data Lake U-SQL SDK</span><span class="sxs-lookup"><span data-stu-id="f7e05-103">Scale U-SQL local run and test with Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="f7e05-104">Při vývoji skript U-SQL, je běžné ke spuštění a testů U-SQL skriptu místně před odešlete ji do cloudu.</span><span class="sxs-lookup"><span data-stu-id="f7e05-104">When developing U-SQL script, it is common to run and test U-SQL script locally before submit it to cloud.</span></span> <span data-ttu-id="f7e05-105">Azure Data Lake poskytuje balíček Nuget s názvem SDK Azure Data Lake U-SQL pro tento scénář, pomocí kterých lze snadno škálovat U-SQL místní spuštění a testování.</span><span class="sxs-lookup"><span data-stu-id="f7e05-105">Azure Data Lake provides a Nuget package called Azure Data Lake U-SQL SDK for this scenario, through which you can easily scale U-SQL local run and test.</span></span> <span data-ttu-id="f7e05-106">Je také možné integrovat tento test U-SQL systému CI (nepřetržité integrace) pro automatizaci kompilaci a testování.</span><span class="sxs-lookup"><span data-stu-id="f7e05-106">It is also possible to integrate this U-SQL test with CI (Continuous Integration) system to automate the compile and test.</span></span>

<span data-ttu-id="f7e05-107">Pokud vám záleží, jak chcete ručně místní spuštění a ladění skriptu U-SQL pomocí nástrojů s grafickým uživatelským rozhraním, můžete pomocí nástroje Azure Data Lake pro Visual Studio pro tento.</span><span class="sxs-lookup"><span data-stu-id="f7e05-107">If you care about how to manually local run and debug U-SQL script with GUI tooling, then you can use Azure Data Lake Tools for Visual Studio for that.</span></span> <span data-ttu-id="f7e05-108">Další informace z [zde](data-lake-analytics-data-lake-tools-local-run.md).</span><span class="sxs-lookup"><span data-stu-id="f7e05-108">You can learn more from [here](data-lake-analytics-data-lake-tools-local-run.md).</span></span>

## <a name="install-azure-data-lake-u-sql-sdk"></a><span data-ttu-id="f7e05-109">Instalace Azure Data Lake U-SQL SDK</span><span class="sxs-lookup"><span data-stu-id="f7e05-109">Install Azure Data Lake U-SQL SDK</span></span>

<span data-ttu-id="f7e05-110">Můžete získat sadu SDK Azure Data Lake U-SQL [sem](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) na Nuget.org.</span><span class="sxs-lookup"><span data-stu-id="f7e05-110">You can get the Azure Data Lake U-SQL SDK [here](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) on Nuget.org.</span></span> <span data-ttu-id="f7e05-111">A před jeho použitím, budete muset Ujistěte se, že máte následující závislosti.</span><span class="sxs-lookup"><span data-stu-id="f7e05-111">And before using it, you need to make sure you have dependencies as follows.</span></span>

### <a name="dependencies"></a><span data-ttu-id="f7e05-112">Závislosti</span><span class="sxs-lookup"><span data-stu-id="f7e05-112">Dependencies</span></span>

<span data-ttu-id="f7e05-113">Data Lake U-SQL SDK vyžaduje následující závislosti:</span><span class="sxs-lookup"><span data-stu-id="f7e05-113">The Data Lake U-SQL SDK requires the following dependencies:</span></span>

- <span data-ttu-id="f7e05-114">[Rozhraní Microsoft .NET Framework 4.6 nebo novější](https://www.microsoft.com/download/details.aspx?id=17851).</span><span class="sxs-lookup"><span data-stu-id="f7e05-114">[Microsoft .NET Framework 4.6 or newer](https://www.microsoft.com/download/details.aspx?id=17851).</span></span>
- <span data-ttu-id="f7e05-115">Microsoft Visual C++ 14 a sady Windows SDK 10.0.10240.0 nebo novější (což se označuje jako CppSDK v tomto článku).</span><span class="sxs-lookup"><span data-stu-id="f7e05-115">Microsoft Visual C++ 14 and Windows SDK 10.0.10240.0 or newer (which is called CppSDK in this article).</span></span> <span data-ttu-id="f7e05-116">Existují dva způsoby, jak získat CppSDK:</span><span class="sxs-lookup"><span data-stu-id="f7e05-116">There are two ways to get CppSDK:</span></span>

    - <span data-ttu-id="f7e05-117">Nainstalujte [sady Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou).</span><span class="sxs-lookup"><span data-stu-id="f7e05-117">Install [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou).</span></span> <span data-ttu-id="f7e05-118">Budete mít \Windows Kits\10 složku ve složce Program Files – například C:\Program Files (x86) \Windows Kits\10\.</span><span class="sxs-lookup"><span data-stu-id="f7e05-118">You'll have a \Windows Kits\10 folder under the Program Files folder--for example, C:\Program Files (x86)\Windows Kits\10\.</span></span> <span data-ttu-id="f7e05-119">Naleznete zde také verze Windows 10 SDK v části \Windows Kits\10\Lib.</span><span class="sxs-lookup"><span data-stu-id="f7e05-119">You'll also find the Windows 10 SDK version under \Windows Kits\10\Lib.</span></span> <span data-ttu-id="f7e05-120">Pokud nevidíte tyto složky, přeinstalujte Visual Studio a je nutné vybrat během instalace Windows 10 SDK.</span><span class="sxs-lookup"><span data-stu-id="f7e05-120">If you don’t see these folders, reinstall Visual Studio and be sure to select the Windows 10 SDK during the installation.</span></span> <span data-ttu-id="f7e05-121">Pokud je to nainstalované s Visual Studio, místní kompilátoru U-SQL najdete je automaticky.</span><span class="sxs-lookup"><span data-stu-id="f7e05-121">If you have this installed with Visual Studio, the U-SQL local compiler will find it automatically.</span></span>

    ![Nástroje data Lake pro Visual Studio místní spuštění Windows 10 SDK](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

    - <span data-ttu-id="f7e05-123">Nainstalujte [nástroje Data Lake pro Visual Studio](http://aka.ms/adltoolsvs).</span><span class="sxs-lookup"><span data-stu-id="f7e05-123">Install [Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs).</span></span> <span data-ttu-id="f7e05-124">Můžete najít hotových Visual C++ a Windows SDK soubory v C:\Program Files (x86) \Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK.</span><span class="sxs-lookup"><span data-stu-id="f7e05-124">You can find the prepackaged Visual C++ and Windows SDK files at C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK.</span></span> <span data-ttu-id="f7e05-125">Místní kompilátoru U-SQL v tomto případě nelze najít závislosti automaticky.</span><span class="sxs-lookup"><span data-stu-id="f7e05-125">In this case, the U-SQL local compiler cannot find the dependencies automatically.</span></span> <span data-ttu-id="f7e05-126">Je třeba zadat cestu CppSDK pro ni.</span><span class="sxs-lookup"><span data-stu-id="f7e05-126">You need to specify the CppSDK path for it.</span></span> <span data-ttu-id="f7e05-127">Můžete buď zkopírujte soubory do jiného umístění nebo ho jako je použít.</span><span class="sxs-lookup"><span data-stu-id="f7e05-127">You can either copy the files to another location or use it as is.</span></span>

## <a name="understand-basic-concepts"></a><span data-ttu-id="f7e05-128">Pochopit základní koncepty</span><span class="sxs-lookup"><span data-stu-id="f7e05-128">Understand basic concepts</span></span>

### <a name="data-root"></a><span data-ttu-id="f7e05-129">Kořenové datové</span><span class="sxs-lookup"><span data-stu-id="f7e05-129">Data root</span></span>

<span data-ttu-id="f7e05-130">Data kořenové složky je pro účet místního výpočetní "místní úložiště".</span><span class="sxs-lookup"><span data-stu-id="f7e05-130">The data-root folder is a "local store" for the local compute account.</span></span> <span data-ttu-id="f7e05-131">Je ekvivalentní k účtu Azure Data Lake Store účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="f7e05-131">It's equivalent to the Azure Data Lake Store account of a Data Lake Analytics account.</span></span> <span data-ttu-id="f7e05-132">Přepnutí na jiný data kořenová složka je stejně jako přepnutí na účet jiné úložiště.</span><span class="sxs-lookup"><span data-stu-id="f7e05-132">Switching to a different data-root folder is just like switching to a different store account.</span></span> <span data-ttu-id="f7e05-133">Pokud chcete pro přístup k běžně sdílený data pomocí různých dat kořenové složky, je nutné použít absolutní cesty ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="f7e05-133">If you want to access commonly shared data with different data-root folders, you must use absolute paths in your scripts.</span></span> <span data-ttu-id="f7e05-134">Nebo vytvořte symbolické odkazy systému souborů (například **mklink** na systém souborů NTFS) v kořenové datové složce tak, aby odkazoval sdílená data.</span><span class="sxs-lookup"><span data-stu-id="f7e05-134">Or, create file system symbolic links (for example, **mklink** on NTFS) under the data-root folder to point to the shared data.</span></span>

<span data-ttu-id="f7e05-135">Data kořenové složky se používá pro:</span><span class="sxs-lookup"><span data-stu-id="f7e05-135">The data-root folder is used to:</span></span>

- <span data-ttu-id="f7e05-136">Uložení místních metadat, včetně databází, tabulky, funkce vracející tabulku (Tvf) a sestavení.</span><span class="sxs-lookup"><span data-stu-id="f7e05-136">Store local metadata, including databases, tables, table-valued functions (TVFs), and assemblies.</span></span>
- <span data-ttu-id="f7e05-137">Vyhledání vstupní a výstupní cesty, které jsou definovány jako relativní cesty v U-SQL.</span><span class="sxs-lookup"><span data-stu-id="f7e05-137">Look up the input and output paths that are defined as relative paths in U-SQL.</span></span> <span data-ttu-id="f7e05-138">Pomocí relativní cesty usnadňuje nasazení vašich projektů U-SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="f7e05-138">Using relative paths makes it easier to deploy your U-SQL projects to Azure.</span></span>

### <a name="file-path-in-u-sql"></a><span data-ttu-id="f7e05-139">Cesta k souboru v U-SQL</span><span class="sxs-lookup"><span data-stu-id="f7e05-139">File path in U-SQL</span></span>

<span data-ttu-id="f7e05-140">Můžete je relativní cesta a místní cestou absolutní v skriptů U-SQL.</span><span class="sxs-lookup"><span data-stu-id="f7e05-140">You can use both a relative path and a local absolute path in U-SQL scripts.</span></span> <span data-ttu-id="f7e05-141">Relativní cesta je relativní k cestě zadané kořenové datové složce.</span><span class="sxs-lookup"><span data-stu-id="f7e05-141">The relative path is relative to the specified data-root folder path.</span></span> <span data-ttu-id="f7e05-142">Doporučujeme vám, že používáte "/" jako oddělovač cesty upravit skripty kompatibilní se na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="f7e05-142">We recommend that you use "/" as the path separator to make your scripts compatible with the server side.</span></span> <span data-ttu-id="f7e05-143">Zde jsou některé příklady relativní cesty a jejich ekvivalent absolutní cesty.</span><span class="sxs-lookup"><span data-stu-id="f7e05-143">Here are some examples of relative paths and their equivalent absolute paths.</span></span> <span data-ttu-id="f7e05-144">V těchto příkladech je C:\LocalRunDataRoot kořenové datové složce.</span><span class="sxs-lookup"><span data-stu-id="f7e05-144">In these examples, C:\LocalRunDataRoot is the data-root folder.</span></span>

|<span data-ttu-id="f7e05-145">Relativní cesta</span><span class="sxs-lookup"><span data-stu-id="f7e05-145">Relative path</span></span>|<span data-ttu-id="f7e05-146">Absolutní cesty</span><span class="sxs-lookup"><span data-stu-id="f7e05-146">Absolute path</span></span>|
|-------------|-------------|
|<span data-ttu-id="f7e05-147">/ABC/DEF/Input.csv</span><span class="sxs-lookup"><span data-stu-id="f7e05-147">/abc/def/input.csv</span></span> |<span data-ttu-id="f7e05-148">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="f7e05-148">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="f7e05-149">ABC/DEF/Input.csv</span><span class="sxs-lookup"><span data-stu-id="f7e05-149">abc/def/input.csv</span></span>  |<span data-ttu-id="f7e05-150">C:\LocalRunDataRoot\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="f7e05-150">C:\LocalRunDataRoot\abc\def\input.csv</span></span>|
|<span data-ttu-id="f7e05-151">D:/ABC/DEF/Input.csv</span><span class="sxs-lookup"><span data-stu-id="f7e05-151">D:/abc/def/input.csv</span></span> |<span data-ttu-id="f7e05-152">D:\abc\def\input.csv</span><span class="sxs-lookup"><span data-stu-id="f7e05-152">D:\abc\def\input.csv</span></span>|

### <a name="working-directory"></a><span data-ttu-id="f7e05-153">Pracovní adresář</span><span class="sxs-lookup"><span data-stu-id="f7e05-153">Working directory</span></span>

<span data-ttu-id="f7e05-154">Při místním spuštění skriptu U-SQL, vytvoří se během kompilace v aktuálním adresáři spuštěné pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="f7e05-154">When running the U-SQL script locally, a working directory is created during compilation under current running directory.</span></span> <span data-ttu-id="f7e05-155">Kromě výstupy kompilace soubory potřebné modulu runtime pro místní spuštění bude stínové kopie pro pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="f7e05-155">In addition to the compilation outputs, the needed runtime files for local execution will be shadow copied to this working directory.</span></span> <span data-ttu-id="f7e05-156">Pracovní adresář kořenové složky se označuje jako "ScopeWorkDir" a soubory v pracovní adresář jsou následující:</span><span class="sxs-lookup"><span data-stu-id="f7e05-156">The working directory root folder is called "ScopeWorkDir" and the files under the working directory are as follows:</span></span>

|<span data-ttu-id="f7e05-157">Adresář nebo soubor</span><span class="sxs-lookup"><span data-stu-id="f7e05-157">Directory/file</span></span>|<span data-ttu-id="f7e05-158">Adresář nebo soubor</span><span class="sxs-lookup"><span data-stu-id="f7e05-158">Directory/file</span></span>|<span data-ttu-id="f7e05-159">Adresář nebo soubor</span><span class="sxs-lookup"><span data-stu-id="f7e05-159">Directory/file</span></span>|<span data-ttu-id="f7e05-160">Definice</span><span class="sxs-lookup"><span data-stu-id="f7e05-160">Definition</span></span>|<span data-ttu-id="f7e05-161">Popis</span><span class="sxs-lookup"><span data-stu-id="f7e05-161">Description</span></span>|
|--------------|--------------|--------------|----------|-----------|
|<span data-ttu-id="f7e05-162">C6A101DDCB470506</span><span class="sxs-lookup"><span data-stu-id="f7e05-162">C6A101DDCB470506</span></span>| | |<span data-ttu-id="f7e05-163">Řetězec hash verze modulu runtime</span><span class="sxs-lookup"><span data-stu-id="f7e05-163">Hash string of runtime version</span></span>|<span data-ttu-id="f7e05-164">Soubory modulu runtime, které jsou potřebné pro provedení místní stínové kopie</span><span class="sxs-lookup"><span data-stu-id="f7e05-164">Shadow copy of runtime files needed for local execution</span></span>|
| |<span data-ttu-id="f7e05-165">Script_66AE4909AA0ED06C</span><span class="sxs-lookup"><span data-stu-id="f7e05-165">Script_66AE4909AA0ED06C</span></span>| |<span data-ttu-id="f7e05-166">Název skriptu + hash řetězec cestu ke skriptu</span><span class="sxs-lookup"><span data-stu-id="f7e05-166">Script name + hash string of script path</span></span>|<span data-ttu-id="f7e05-167">Výstupy kompilace a provádění krok protokolování</span><span class="sxs-lookup"><span data-stu-id="f7e05-167">Compilation outputs and execution step logging</span></span>|
| | |<span data-ttu-id="f7e05-168">\_skript\_.abr</span><span class="sxs-lookup"><span data-stu-id="f7e05-168">\_script\_.abr</span></span>|<span data-ttu-id="f7e05-169">Výstup kompilátoru</span><span class="sxs-lookup"><span data-stu-id="f7e05-169">Compiler output</span></span>|<span data-ttu-id="f7e05-170">Soubor algebra</span><span class="sxs-lookup"><span data-stu-id="f7e05-170">Algebra file</span></span>|
| | |<span data-ttu-id="f7e05-171">\_ScopeCodeGen\_. *</span><span class="sxs-lookup"><span data-stu-id="f7e05-171">\_ScopeCodeGen\_.*</span></span>|<span data-ttu-id="f7e05-172">Výstup kompilátoru</span><span class="sxs-lookup"><span data-stu-id="f7e05-172">Compiler output</span></span>|<span data-ttu-id="f7e05-173">Vygenerovaný spravovaného kódu</span><span class="sxs-lookup"><span data-stu-id="f7e05-173">Generated managed code</span></span>|
| | |<span data-ttu-id="f7e05-174">\_ScopeCodeGenEngine\_. *</span><span class="sxs-lookup"><span data-stu-id="f7e05-174">\_ScopeCodeGenEngine\_.*</span></span>|<span data-ttu-id="f7e05-175">Výstup kompilátoru</span><span class="sxs-lookup"><span data-stu-id="f7e05-175">Compiler output</span></span>|<span data-ttu-id="f7e05-176">Vygenerovaný nativního kódu</span><span class="sxs-lookup"><span data-stu-id="f7e05-176">Generated native code</span></span>|
| | |<span data-ttu-id="f7e05-177">Odkazovaná sestavení</span><span class="sxs-lookup"><span data-stu-id="f7e05-177">referenced assemblies</span></span>|<span data-ttu-id="f7e05-178">Odkaz na sestavení</span><span class="sxs-lookup"><span data-stu-id="f7e05-178">Assembly reference</span></span>|<span data-ttu-id="f7e05-179">Soubory odkazované sestavení</span><span class="sxs-lookup"><span data-stu-id="f7e05-179">Referenced assembly files</span></span>|
| | |<span data-ttu-id="f7e05-180">deployed_resources</span><span class="sxs-lookup"><span data-stu-id="f7e05-180">deployed_resources</span></span>|<span data-ttu-id="f7e05-181">Nasazení prostředků</span><span class="sxs-lookup"><span data-stu-id="f7e05-181">Resource deployment</span></span>|<span data-ttu-id="f7e05-182">Soubory nasazení prostředků</span><span class="sxs-lookup"><span data-stu-id="f7e05-182">Resource deployment files</span></span>|
| | |<span data-ttu-id="f7e05-183">XXXXXXXX.xxx[1..n]\_\*. *</span><span class="sxs-lookup"><span data-stu-id="f7e05-183">xxxxxxxx.xxx[1..n]\_\*.*</span></span>|<span data-ttu-id="f7e05-184">Spuštění protokolu</span><span class="sxs-lookup"><span data-stu-id="f7e05-184">Execution log</span></span>|<span data-ttu-id="f7e05-185">Protokol provádění kroků</span><span class="sxs-lookup"><span data-stu-id="f7e05-185">Log of execution steps</span></span>|


## <a name="use-the-sdk-from-the-command-line"></a><span data-ttu-id="f7e05-186">Použití sady SDK z příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="f7e05-186">Use the SDK from the command line</span></span>

### <a name="command-line-interface-of-the-helper-application"></a><span data-ttu-id="f7e05-187">Rozhraní příkazového řádku aplikace pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="f7e05-187">Command-line interface of the helper application</span></span>

<span data-ttu-id="f7e05-188">V části sada SDK directory\build\runtime LocalRunHelper.exe je nástroj příkazového řádku se aplikace, která poskytuje rozhraní pro většinu funkcí pro běžně používané místní spuštění.</span><span class="sxs-lookup"><span data-stu-id="f7e05-188">Under SDK directory\build\runtime, LocalRunHelper.exe is the command-line helper application that provides interfaces to most of the commonly used local-run functions.</span></span> <span data-ttu-id="f7e05-189">Upozorňujeme, že příkaz i argument přepínače jsou malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="f7e05-189">Note that both the command and the argument switches are case-sensitive.</span></span> <span data-ttu-id="f7e05-190">Chcete-li ji použít:</span><span class="sxs-lookup"><span data-stu-id="f7e05-190">To invoke it:</span></span>

    LocalRunHelper.exe <command> <Required-Command-Arguments> [Optional-Command-Arguments]

<span data-ttu-id="f7e05-191">Spustit LocalRunHelper.exe bez argumentů nebo se **pomoci** přepnout na zobrazení informací nápovědy:</span><span class="sxs-lookup"><span data-stu-id="f7e05-191">Run LocalRunHelper.exe without arguments or with the **help** switch to show the help information:</span></span>

    > LocalRunHelper.exe help

        Command 'help' :  Show usage information
        Command 'compile' :  Compile the script
        Required Arguments :
            -Script param
                    Script File Path
        Optional Arguments :
            -Shallow [default value 'False']
                    Shallow compile

<span data-ttu-id="f7e05-192">V informace nápovědy:</span><span class="sxs-lookup"><span data-stu-id="f7e05-192">In the help information:</span></span>

-  <span data-ttu-id="f7e05-193">**Příkaz** poskytuje název příkazu.</span><span class="sxs-lookup"><span data-stu-id="f7e05-193">**Command** gives the command’s name.</span></span>  
-  <span data-ttu-id="f7e05-194">**Vyžaduje Argument** uvádí argumenty, které je nutné zadat.</span><span class="sxs-lookup"><span data-stu-id="f7e05-194">**Required Argument** lists arguments that must be supplied.</span></span>  
-  <span data-ttu-id="f7e05-195">**Nepovinný Argument** uvádí argumenty, které jsou volitelné, s výchozími hodnotami.</span><span class="sxs-lookup"><span data-stu-id="f7e05-195">**Optional Argument** lists arguments that are optional, with default values.</span></span>  <span data-ttu-id="f7e05-196">Nepovinné argumenty Boolean nemají parametry a jejich vzhled znamená záporná na jejich výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f7e05-196">Optional Boolean arguments don’t have parameters, and their appearances mean negative to their default value.</span></span>

### <a name="return-value-and-logging"></a><span data-ttu-id="f7e05-197">Vrátí hodnotu a protokolování</span><span class="sxs-lookup"><span data-stu-id="f7e05-197">Return value and logging</span></span>

<span data-ttu-id="f7e05-198">Vrací podpůrnou aplikací **0** úspěch a **-1** selhání.</span><span class="sxs-lookup"><span data-stu-id="f7e05-198">The helper application returns **0** for success and **-1** for failure.</span></span> <span data-ttu-id="f7e05-199">Ve výchozím nastavení odešle pomocné rutiny všechny zprávy do aktuální konzolu.</span><span class="sxs-lookup"><span data-stu-id="f7e05-199">By default, the helper sends all messages to the current console.</span></span> <span data-ttu-id="f7e05-200">Ale většina příkazů, podporují **- MessageOut cesta_k_souboru_protokolu** nepovinný argument, který přesměruje výstupy do souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="f7e05-200">However, most of the commands support the **-MessageOut path_to_log_file** optional argument that redirects the outputs to a log file.</span></span>

### <a name="environment-variable-configuring"></a><span data-ttu-id="f7e05-201">Konfigurace proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="f7e05-201">Environment variable configuring</span></span>

<span data-ttu-id="f7e05-202">U-SQL místní spuštění potřebují kořenové zadaná data jako účet místní úložiště, zadaná cesta CppSDK závislosti.</span><span class="sxs-lookup"><span data-stu-id="f7e05-202">U-SQL local run needs a specified data root as local storage account, as well as a specified CppSDK path for dependencies.</span></span> <span data-ttu-id="f7e05-203">Můžete i nastavit argument v proměnné prostředí příkazového řádku, nebo je nastavený pro ně.</span><span class="sxs-lookup"><span data-stu-id="f7e05-203">You can both set the argument in command-line or set environment variable for them.</span></span>

- <span data-ttu-id="f7e05-204">Nastavte **SCOPE_CPP_SDK** proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="f7e05-204">Set the **SCOPE_CPP_SDK** environment variable.</span></span>

    <span data-ttu-id="f7e05-205">Pokud se zobrazí Microsoft Visual C++ a sada Windows SDK prostřednictvím instalace nástrojů Data Lake pro Visual Studio, ověřte, zda máte následující složku:</span><span class="sxs-lookup"><span data-stu-id="f7e05-205">If you get Microsoft Visual C++ and the Windows SDK by installing Data Lake Tools for Visual Studio, verify that you have the following folder:</span></span>

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    <span data-ttu-id="f7e05-206">Zadejte novou proměnnou prostředí s názvem **SCOPE_CPP_SDK** tak, aby odkazovaly do tohoto adresáře.</span><span class="sxs-lookup"><span data-stu-id="f7e05-206">Define a new environment variable called **SCOPE_CPP_SDK** to point to this directory.</span></span> <span data-ttu-id="f7e05-207">Zkopírujte složku do jiného umístění a zadejte **SCOPE_CPP_SDK** jako.</span><span class="sxs-lookup"><span data-stu-id="f7e05-207">Or copy the folder to the other location and specify **SCOPE_CPP_SDK** as that.</span></span>

    <span data-ttu-id="f7e05-208">Kromě nastavení proměnné prostředí, můžete zadat **- CppSDK** argument při použití příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f7e05-208">In addition to setting the environment variable, you can specify the **-CppSDK** argument when you're using the command line.</span></span> <span data-ttu-id="f7e05-209">Tento argument přepíše vaše proměnná prostředí CppSDK výchozí.</span><span class="sxs-lookup"><span data-stu-id="f7e05-209">This argument overwrites your default CppSDK environment variable.</span></span>

- <span data-ttu-id="f7e05-210">Nastavte **LOCALRUN_DATAROOT** proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="f7e05-210">Set the **LOCALRUN_DATAROOT** environment variable.</span></span>

    <span data-ttu-id="f7e05-211">Zadejte novou proměnnou prostředí s názvem **LOCALRUN_DATAROOT** který odkazuje na kořenový data.</span><span class="sxs-lookup"><span data-stu-id="f7e05-211">Define a new environment variable called **LOCALRUN_DATAROOT** that points to the data root.</span></span>

    <span data-ttu-id="f7e05-212">Kromě nastavení proměnné prostředí, můžete zadat **- DataRoot** argument data kořenovou cestu při použití příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f7e05-212">In addition to setting the environment variable, you can specify the **-DataRoot** argument with the data-root path when you're using a command line.</span></span> <span data-ttu-id="f7e05-213">Tento argument přepíše vaše proměnná prostředí kořenové datové výchozí.</span><span class="sxs-lookup"><span data-stu-id="f7e05-213">This argument overwrites your default data-root environment variable.</span></span> <span data-ttu-id="f7e05-214">Je nutné přidat tento argument pro každý příkazového řádku, které používáte, takže můžete přepsat proměnnou prostředí kořenové datové výchozí pro všechny operace.</span><span class="sxs-lookup"><span data-stu-id="f7e05-214">You need to add this argument to every command line you're running so that you can overwrite the default data-root environment variable for all operations.</span></span>

### <a name="sdk-command-line-usage-samples"></a><span data-ttu-id="f7e05-215">Ukázky využití příkazový řádek SDK</span><span class="sxs-lookup"><span data-stu-id="f7e05-215">SDK command line usage samples</span></span>

#### <a name="compile-and-run"></a><span data-ttu-id="f7e05-216">Kompilace a spuštění</span><span class="sxs-lookup"><span data-stu-id="f7e05-216">Compile and run</span></span>

<span data-ttu-id="f7e05-217">**Spustit** je příkaz použitý ke kompilaci skript a potom spusťte kompilované výsledky.</span><span class="sxs-lookup"><span data-stu-id="f7e05-217">The **run** command is used to compile the script and then execute compiled results.</span></span> <span data-ttu-id="f7e05-218">Jeho argumenty příkazového řádku jsou kombinaci těchto z **zkompilovat** a **provést**.</span><span class="sxs-lookup"><span data-stu-id="f7e05-218">Its command-line arguments are a combination of those from **compile** and **execute**.</span></span>

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

<span data-ttu-id="f7e05-219">Toto jsou volitelné argumenty pro **spustit**:</span><span class="sxs-lookup"><span data-stu-id="f7e05-219">The following are optional arguments for **run**:</span></span>


|<span data-ttu-id="f7e05-220">Argument</span><span class="sxs-lookup"><span data-stu-id="f7e05-220">Argument</span></span>|<span data-ttu-id="f7e05-221">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="f7e05-221">Default value</span></span>|<span data-ttu-id="f7e05-222">Popis</span><span class="sxs-lookup"><span data-stu-id="f7e05-222">Description</span></span>|
|--------|-------------|-----------|
|<span data-ttu-id="f7e05-223">-CodeBehind</span><span class="sxs-lookup"><span data-stu-id="f7e05-223">-CodeBehind</span></span>|<span data-ttu-id="f7e05-224">False</span><span class="sxs-lookup"><span data-stu-id="f7e05-224">False</span></span>|<span data-ttu-id="f7e05-225">Tento skript má .cs kódu na pozadí</span><span class="sxs-lookup"><span data-stu-id="f7e05-225">The script has .cs code behind</span></span>|
|<span data-ttu-id="f7e05-226">-CppSDK</span><span class="sxs-lookup"><span data-stu-id="f7e05-226">-CppSDK</span></span>| |<span data-ttu-id="f7e05-227">CppSDK adresáře</span><span class="sxs-lookup"><span data-stu-id="f7e05-227">CppSDK Directory</span></span>|
|<span data-ttu-id="f7e05-228">-DataRoot</span><span class="sxs-lookup"><span data-stu-id="f7e05-228">-DataRoot</span></span>| <span data-ttu-id="f7e05-229">Proměnné prostředí DataRoot</span><span class="sxs-lookup"><span data-stu-id="f7e05-229">DataRoot environment variable</span></span>|<span data-ttu-id="f7e05-230">DataRoot pro místní spuštění, výchozí do proměnné prostředí, LOCALRUN_DATAROOT.</span><span class="sxs-lookup"><span data-stu-id="f7e05-230">DataRoot for local run, default to 'LOCALRUN_DATAROOT' environment variable</span></span>|
|<span data-ttu-id="f7e05-231">-MessageOut</span><span class="sxs-lookup"><span data-stu-id="f7e05-231">-MessageOut</span></span>| |<span data-ttu-id="f7e05-232">Výpis zprávy v konzole do souboru</span><span class="sxs-lookup"><span data-stu-id="f7e05-232">Dump messages on console to a file</span></span>|
|<span data-ttu-id="f7e05-233">-Paralelní</span><span class="sxs-lookup"><span data-stu-id="f7e05-233">-Parallel</span></span>|<span data-ttu-id="f7e05-234">1</span><span class="sxs-lookup"><span data-stu-id="f7e05-234">1</span></span>|<span data-ttu-id="f7e05-235">Spustit s plánem s zadaný paralelismus</span><span class="sxs-lookup"><span data-stu-id="f7e05-235">Run the plan with the specified parallelism</span></span>|
|<span data-ttu-id="f7e05-236">-Odkazy</span><span class="sxs-lookup"><span data-stu-id="f7e05-236">-References</span></span>| |<span data-ttu-id="f7e05-237">Seznam cest navíc referenční sestavení nebo datové soubory kódu na pozadí, oddělených ';'</span><span class="sxs-lookup"><span data-stu-id="f7e05-237">List of paths to extra reference assemblies or data files of code behind, separated by ';'</span></span>|
|<span data-ttu-id="f7e05-238">-UdoRedirect</span><span class="sxs-lookup"><span data-stu-id="f7e05-238">-UdoRedirect</span></span>|<span data-ttu-id="f7e05-239">False</span><span class="sxs-lookup"><span data-stu-id="f7e05-239">False</span></span>|<span data-ttu-id="f7e05-240">Generovat Udo sestavení přesměrování konfigurace</span><span class="sxs-lookup"><span data-stu-id="f7e05-240">Generate Udo assembly redirect config</span></span>|
|<span data-ttu-id="f7e05-241">-UseDatabase</span><span class="sxs-lookup"><span data-stu-id="f7e05-241">-UseDatabase</span></span>|<span data-ttu-id="f7e05-242">master</span><span class="sxs-lookup"><span data-stu-id="f7e05-242">master</span></span>|<span data-ttu-id="f7e05-243">Databázi pro použití při kódu na pozadí registrace dočasné sestavení</span><span class="sxs-lookup"><span data-stu-id="f7e05-243">Database to use for code behind temporary assembly registration</span></span>|
|<span data-ttu-id="f7e05-244">-Verbose</span><span class="sxs-lookup"><span data-stu-id="f7e05-244">-Verbose</span></span>|<span data-ttu-id="f7e05-245">False</span><span class="sxs-lookup"><span data-stu-id="f7e05-245">False</span></span>|<span data-ttu-id="f7e05-246">Zobrazit podrobné výstupy z modulu runtime</span><span class="sxs-lookup"><span data-stu-id="f7e05-246">Show detailed outputs from runtime</span></span>|
|<span data-ttu-id="f7e05-247">-WorkDir</span><span class="sxs-lookup"><span data-stu-id="f7e05-247">-WorkDir</span></span>|<span data-ttu-id="f7e05-248">Aktuální adresář</span><span class="sxs-lookup"><span data-stu-id="f7e05-248">Current Directory</span></span>|<span data-ttu-id="f7e05-249">Adresář pro využití kompilátoru a výstupy</span><span class="sxs-lookup"><span data-stu-id="f7e05-249">Directory for compiler usage and outputs</span></span>|
|<span data-ttu-id="f7e05-250">-RunScopeCEP</span><span class="sxs-lookup"><span data-stu-id="f7e05-250">-RunScopeCEP</span></span>|<span data-ttu-id="f7e05-251">0</span><span class="sxs-lookup"><span data-stu-id="f7e05-251">0</span></span>|<span data-ttu-id="f7e05-252">Režim ScopeCEP používat</span><span class="sxs-lookup"><span data-stu-id="f7e05-252">ScopeCEP mode to use</span></span>|
|<span data-ttu-id="f7e05-253">-ScopeCEPTempPath</span><span class="sxs-lookup"><span data-stu-id="f7e05-253">-ScopeCEPTempPath</span></span>|<span data-ttu-id="f7e05-254">dočasné</span><span class="sxs-lookup"><span data-stu-id="f7e05-254">temp</span></span>|<span data-ttu-id="f7e05-255">Dočasná cesta k použití pro datový proud</span><span class="sxs-lookup"><span data-stu-id="f7e05-255">Temp path to use for streaming data</span></span>|
|<span data-ttu-id="f7e05-256">-OptFlags</span><span class="sxs-lookup"><span data-stu-id="f7e05-256">-OptFlags</span></span>| |<span data-ttu-id="f7e05-257">Seznam oddělený čárkami Optimalizátor příznaky</span><span class="sxs-lookup"><span data-stu-id="f7e05-257">Comma-separated list of optimizer flags</span></span>|


<span data-ttu-id="f7e05-258">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="f7e05-258">Here's an example:</span></span>

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

<span data-ttu-id="f7e05-259">Kromě toho kombinování **zkompilovat** a **provést**, můžete zkompilovat a spustit samostatně, kompilované spustitelné soubory.</span><span class="sxs-lookup"><span data-stu-id="f7e05-259">Besides combining **compile** and **execute**, you can compile and execute the compiled executables separately.</span></span>

#### <a name="compile-a-u-sql-script"></a><span data-ttu-id="f7e05-260">Kompilace skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="f7e05-260">Compile a U-SQL script</span></span>

<span data-ttu-id="f7e05-261">**Zkompilovat** příkaz se používá ke kompilaci skript U-SQL pro spustitelné soubory.</span><span class="sxs-lookup"><span data-stu-id="f7e05-261">The **compile** command is used to compile a U-SQL script to executables.</span></span>

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

<span data-ttu-id="f7e05-262">Toto jsou volitelné argumenty pro **zkompilovat**:</span><span class="sxs-lookup"><span data-stu-id="f7e05-262">The following are optional arguments for **compile**:</span></span>


|<span data-ttu-id="f7e05-263">Argument</span><span class="sxs-lookup"><span data-stu-id="f7e05-263">Argument</span></span>|<span data-ttu-id="f7e05-264">Popis</span><span class="sxs-lookup"><span data-stu-id="f7e05-264">Description</span></span>|
|--------|-----------|
| <span data-ttu-id="f7e05-265">-CodeBehind [výchozí hodnotu "False"]</span><span class="sxs-lookup"><span data-stu-id="f7e05-265">-CodeBehind [default value 'False']</span></span>|<span data-ttu-id="f7e05-266">Tento skript má .cs kódu na pozadí</span><span class="sxs-lookup"><span data-stu-id="f7e05-266">The script has .cs code behind</span></span>|
| <span data-ttu-id="f7e05-267">-CppSDK [výchozí hodnota:]</span><span class="sxs-lookup"><span data-stu-id="f7e05-267">-CppSDK [default value '']</span></span>|<span data-ttu-id="f7e05-268">CppSDK adresáře</span><span class="sxs-lookup"><span data-stu-id="f7e05-268">CppSDK Directory</span></span>|
| <span data-ttu-id="f7e05-269">-DataRoot [výchozí hodnota "Proměnné prostředí DataRoot"]</span><span class="sxs-lookup"><span data-stu-id="f7e05-269">-DataRoot [default value 'DataRoot environment variable']</span></span>|<span data-ttu-id="f7e05-270">DataRoot pro místní spuštění, výchozí do proměnné prostředí, LOCALRUN_DATAROOT.</span><span class="sxs-lookup"><span data-stu-id="f7e05-270">DataRoot for local run, default to 'LOCALRUN_DATAROOT' environment variable</span></span>|
| <span data-ttu-id="f7e05-271">-MessageOut [výchozí hodnota:]</span><span class="sxs-lookup"><span data-stu-id="f7e05-271">-MessageOut [default value '']</span></span>|<span data-ttu-id="f7e05-272">Výpis zprávy v konzole do souboru</span><span class="sxs-lookup"><span data-stu-id="f7e05-272">Dump messages on console to a file</span></span>|
| <span data-ttu-id="f7e05-273">-Odkazuje [výchozí hodnota:]</span><span class="sxs-lookup"><span data-stu-id="f7e05-273">-References [default value '']</span></span>|<span data-ttu-id="f7e05-274">Seznam cest navíc referenční sestavení nebo datové soubory kódu na pozadí, oddělených ';'</span><span class="sxs-lookup"><span data-stu-id="f7e05-274">List of paths to extra reference assemblies or data files of code behind, separated by ';'</span></span>|
| <span data-ttu-id="f7e05-275">-Malou [výchozí hodnotu "False"]</span><span class="sxs-lookup"><span data-stu-id="f7e05-275">-Shallow [default value 'False']</span></span>|<span data-ttu-id="f7e05-276">Bez podstruktury kompilace</span><span class="sxs-lookup"><span data-stu-id="f7e05-276">Shallow compile</span></span>|
| <span data-ttu-id="f7e05-277">-UdoRedirect [výchozí hodnotu "False"]</span><span class="sxs-lookup"><span data-stu-id="f7e05-277">-UdoRedirect [default value 'False']</span></span>|<span data-ttu-id="f7e05-278">Generovat Udo sestavení přesměrování konfigurace</span><span class="sxs-lookup"><span data-stu-id="f7e05-278">Generate Udo assembly redirect config</span></span>|
| <span data-ttu-id="f7e05-279">-UseDatabase [výchozí hodnota 'hlavní']</span><span class="sxs-lookup"><span data-stu-id="f7e05-279">-UseDatabase [default value 'master']</span></span>|<span data-ttu-id="f7e05-280">Databázi pro použití při kódu na pozadí registrace dočasné sestavení</span><span class="sxs-lookup"><span data-stu-id="f7e05-280">Database to use for code behind temporary assembly registration</span></span>|
| <span data-ttu-id="f7e05-281">-WorkDir [výchozí hodnotu 'aktuální adresář.]</span><span class="sxs-lookup"><span data-stu-id="f7e05-281">-WorkDir [default value 'Current Directory']</span></span>|<span data-ttu-id="f7e05-282">Adresář pro využití kompilátoru a výstupy</span><span class="sxs-lookup"><span data-stu-id="f7e05-282">Directory for compiler usage and outputs</span></span>|
| <span data-ttu-id="f7e05-283">-RunScopeCEP [výchozí hodnota '0']</span><span class="sxs-lookup"><span data-stu-id="f7e05-283">-RunScopeCEP [default value '0']</span></span>|<span data-ttu-id="f7e05-284">Režim ScopeCEP používat</span><span class="sxs-lookup"><span data-stu-id="f7e05-284">ScopeCEP mode to use</span></span>|
| <span data-ttu-id="f7e05-285">-ScopeCEPTempPath [výchozí hodnota 'temp']</span><span class="sxs-lookup"><span data-stu-id="f7e05-285">-ScopeCEPTempPath [default value 'temp']</span></span>|<span data-ttu-id="f7e05-286">Dočasná cesta k použití pro datový proud</span><span class="sxs-lookup"><span data-stu-id="f7e05-286">Temp path to use for streaming data</span></span>|
| <span data-ttu-id="f7e05-287">-OptFlags [výchozí hodnota:]</span><span class="sxs-lookup"><span data-stu-id="f7e05-287">-OptFlags [default value '']</span></span>|<span data-ttu-id="f7e05-288">Seznam oddělený čárkami Optimalizátor příznaky</span><span class="sxs-lookup"><span data-stu-id="f7e05-288">Comma-separated list of optimizer flags</span></span>|


<span data-ttu-id="f7e05-289">Tady jsou některé příklady použití.</span><span class="sxs-lookup"><span data-stu-id="f7e05-289">Here are some usage examples.</span></span>

<span data-ttu-id="f7e05-290">Kompilace skript U-SQL:</span><span class="sxs-lookup"><span data-stu-id="f7e05-290">Compile a U-SQL script:</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql

<span data-ttu-id="f7e05-291">Kompilace skript U-SQL a nastavte data kořenové složce.</span><span class="sxs-lookup"><span data-stu-id="f7e05-291">Compile a U-SQL script and set the data-root folder.</span></span> <span data-ttu-id="f7e05-292">Všimněte si, že tato akce přepíše nastavená proměnná prostředí.</span><span class="sxs-lookup"><span data-stu-id="f7e05-292">Note that this will overwrite the set environment variable.</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

<span data-ttu-id="f7e05-293">Kompilace skript U-SQL a nastavte pracovní adresář, referenční sestavení a databáze:</span><span class="sxs-lookup"><span data-stu-id="f7e05-293">Compile a U-SQL script and set a working directory, reference assembly, and database:</span></span>

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-results"></a><span data-ttu-id="f7e05-294">Spuštění kompilované výsledky</span><span class="sxs-lookup"><span data-stu-id="f7e05-294">Execute compiled results</span></span>

<span data-ttu-id="f7e05-295">**Provést** příkaz se používá k provedení kompilované výsledky.</span><span class="sxs-lookup"><span data-stu-id="f7e05-295">The **execute** command is used to execute compiled results.</span></span>   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

<span data-ttu-id="f7e05-296">Toto jsou volitelné argumenty pro **provést**:</span><span class="sxs-lookup"><span data-stu-id="f7e05-296">The following are optional arguments for **execute**:</span></span>

|<span data-ttu-id="f7e05-297">Argument</span><span class="sxs-lookup"><span data-stu-id="f7e05-297">Argument</span></span>|<span data-ttu-id="f7e05-298">Popis</span><span class="sxs-lookup"><span data-stu-id="f7e05-298">Description</span></span>|
|--------|-----------|
|<span data-ttu-id="f7e05-299">-DataRoot [výchozí hodnota:]</span><span class="sxs-lookup"><span data-stu-id="f7e05-299">-DataRoot [default value '']</span></span>|<span data-ttu-id="f7e05-300">Kořenové datové pro provedení metadat.</span><span class="sxs-lookup"><span data-stu-id="f7e05-300">Data root for metadata execution.</span></span> <span data-ttu-id="f7e05-301">Výchozí hodnota **LOCALRUN_DATAROOT** proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="f7e05-301">It defaults to the **LOCALRUN_DATAROOT** environment variable.</span></span>|
|<span data-ttu-id="f7e05-302">-MessageOut [výchozí hodnota:]</span><span class="sxs-lookup"><span data-stu-id="f7e05-302">-MessageOut [default value '']</span></span>|<span data-ttu-id="f7e05-303">Dump zprávy v konzole do souboru.</span><span class="sxs-lookup"><span data-stu-id="f7e05-303">Dump messages on the console to a file.</span></span>|
|<span data-ttu-id="f7e05-304">-Paralelní [výchozí hodnotu 1.]</span><span class="sxs-lookup"><span data-stu-id="f7e05-304">-Parallel [default value '1']</span></span>|<span data-ttu-id="f7e05-305">Ukazatel ke spuštění generovaného kroků místní spuštění s úrovní zadaný stupně paralelního zpracování.</span><span class="sxs-lookup"><span data-stu-id="f7e05-305">Indicator to run the generated local-run steps with the specified parallelism level.</span></span>|
|<span data-ttu-id="f7e05-306">-Verbose [výchozí hodnotu "False"]</span><span class="sxs-lookup"><span data-stu-id="f7e05-306">-Verbose [default value 'False']</span></span>|<span data-ttu-id="f7e05-307">Ukazatel zobrazíte podrobné výstupy z modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="f7e05-307">Indicator to show detailed outputs from runtime.</span></span>|

<span data-ttu-id="f7e05-308">Tady je příklad použití:</span><span class="sxs-lookup"><span data-stu-id="f7e05-308">Here's a usage example:</span></span>

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5


## <a name="use-the-sdk-with-programming-interfaces"></a><span data-ttu-id="f7e05-309">Použití sady SDK pomocí rozhraní pro programování</span><span class="sxs-lookup"><span data-stu-id="f7e05-309">Use the SDK with programming interfaces</span></span>

<span data-ttu-id="f7e05-310">Programovací rozhraní jsou umístěné v LocalRunHelper.exe.</span><span class="sxs-lookup"><span data-stu-id="f7e05-310">The programming interfaces are all located in the LocalRunHelper.exe.</span></span> <span data-ttu-id="f7e05-311">Můžete je používat k integraci funkce sady SDK U-SQL a test framework C# škálování svůj test místní skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="f7e05-311">You can use them to integrate the functionality of the U-SQL SDK and the C# test framework to scale your U-SQL script local test.</span></span> <span data-ttu-id="f7e05-312">V tomto článku použiji standardní C# projektu testování částí ukazují, jak používat tyto rozhraní k testování váš skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="f7e05-312">In this article, I will use the standard C# unit test project to show how to use these interfaces to test your U-SQL script.</span></span>

### <a name="step-1-create-c-unit-test-project-and-configuration"></a><span data-ttu-id="f7e05-313">Krok 1: Vytvoření projektu testování částí C# a konfigurace</span><span class="sxs-lookup"><span data-stu-id="f7e05-313">Step 1: Create C# unit test project and configuration</span></span>

- <span data-ttu-id="f7e05-314">Vytvoření projektu testování částí C# prostřednictvím soubor > Nový > Projekt > Visual C# > testovací > projektu testování částí.</span><span class="sxs-lookup"><span data-stu-id="f7e05-314">Create a C# unit test project through File > New > Project > Visual C# > Test > Unit Test Project.</span></span>
- <span data-ttu-id="f7e05-315">Přidejte LocalRunHelper.exe jako odkaz pro projekt.</span><span class="sxs-lookup"><span data-stu-id="f7e05-315">Add LocalRunHelper.exe as a reference for the project.</span></span> <span data-ttu-id="f7e05-316">LocalRunHelper.exe se nachází v \build\runtime\LocalRunHelper.exe v balíčku Nuget.</span><span class="sxs-lookup"><span data-stu-id="f7e05-316">The LocalRunHelper.exe is located at \build\runtime\LocalRunHelper.exe in Nuget package.</span></span>

    ![Přidat odkaz na sadu SDK Azure Data Lake U-SQL](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-add-reference.png)

- <span data-ttu-id="f7e05-318">U-SQL SDK **pouze** podporu x64 prostředí, nezapomeňte nastavit jako x64 Cílová platforma sestavení.</span><span class="sxs-lookup"><span data-stu-id="f7e05-318">U-SQL SDK **only** support x64 environment, make sure to set build platform target as x64.</span></span> <span data-ttu-id="f7e05-319">Můžete nastavit, prostřednictvím vlastnosti projektu > sestavení > Cílová platforma.</span><span class="sxs-lookup"><span data-stu-id="f7e05-319">You can set that through Project Property > Build > Platform target.</span></span>

    ![Azure Data Lake U-SQL SDK konfigurace x64 projektu](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-x64.png)

- <span data-ttu-id="f7e05-321">Ujistěte se, že nastavení testovacího prostředí jako x64.</span><span class="sxs-lookup"><span data-stu-id="f7e05-321">Make sure to set your test environment as x64.</span></span> <span data-ttu-id="f7e05-322">V sadě Visual Studio, můžete ho nastavit prostřednictvím testovací > Test nastavení > výchozí architekturu procesoru > x64.</span><span class="sxs-lookup"><span data-stu-id="f7e05-322">In Visual Studio, you can set it through Test > Test Settings > Default Processor Architecture > x64.</span></span>

    ![Azure Data Lake U-SQL SDK konfigurace x64 testovacího prostředí](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-test-x64.png)

- <span data-ttu-id="f7e05-324">Nezapomeňte zkopírovat všechny soubory závislosti v rámci NugetPackage\build\runtime\ do projektu pracovní adresář, který je obvykle pod ProjectFolder\bin\x64\Debug.</span><span class="sxs-lookup"><span data-stu-id="f7e05-324">Make sure to copy all dependency files under NugetPackage\build\runtime\ to project working directory which is usually under ProjectFolder\bin\x64\Debug.</span></span>

### <a name="step-2-create-u-sql-script-test-case"></a><span data-ttu-id="f7e05-325">Krok 2: Vytvoření testovacího případu skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="f7e05-325">Step 2: Create U-SQL script test case</span></span>

<span data-ttu-id="f7e05-326">Níže je ukázkový kód pro test skriptu U-SQL.</span><span class="sxs-lookup"><span data-stu-id="f7e05-326">Below is the sample code for U-SQL script test.</span></span> <span data-ttu-id="f7e05-327">Pro testování, je nutné připravit skripty, soubory očekávaný výstup a vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="f7e05-327">For testing, you need to prepare scripts, input files and expected output files.</span></span>

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
                //Specify the local run message output path
                StreamWriter MessageOutput = new StreamWriter("../../../log.txt");

                LocalRunHelper localrun = new LocalRunHelper(MessageOutput);

                //Configure the DateRoot path, Script Path and CPPSDK path
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

                //Don't forget to close MessageOutput to get logs into file
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


### <a name="programming-interfaces-in-localrunhelperexe"></a><span data-ttu-id="f7e05-328">Programování rozhraní LocalRunHelper.exe</span><span class="sxs-lookup"><span data-stu-id="f7e05-328">Programming interfaces in LocalRunHelper.exe</span></span>

<span data-ttu-id="f7e05-329">LocalRunHelper.exe poskytuje programovací rozhraní pro místní kompilace U-SQL, spusťte atd. Rozhraní jsou uvedeny v následujícím seznamu.</span><span class="sxs-lookup"><span data-stu-id="f7e05-329">LocalRunHelper.exe provides the programming interfaces for U-SQL local compile, run, etc. The interfaces are listed as follows.</span></span>

<span data-ttu-id="f7e05-330">**Konstruktor**</span><span class="sxs-lookup"><span data-stu-id="f7e05-330">**Constructor**</span></span>

<span data-ttu-id="f7e05-331">veřejné LocalRunHelper ([System.IO.TextWriter messageOutput = null])</span><span class="sxs-lookup"><span data-stu-id="f7e05-331">public LocalRunHelper([System.IO.TextWriter messageOutput = null])</span></span>

|<span data-ttu-id="f7e05-332">Parametr</span><span class="sxs-lookup"><span data-stu-id="f7e05-332">Parameter</span></span>|<span data-ttu-id="f7e05-333">Typ</span><span class="sxs-lookup"><span data-stu-id="f7e05-333">Type</span></span>|<span data-ttu-id="f7e05-334">Popis</span><span class="sxs-lookup"><span data-stu-id="f7e05-334">Description</span></span>|
|---------|----|-----------|
|<span data-ttu-id="f7e05-335">messageOutput</span><span class="sxs-lookup"><span data-stu-id="f7e05-335">messageOutput</span></span>|<span data-ttu-id="f7e05-336">System.IO.TextWriter</span><span class="sxs-lookup"><span data-stu-id="f7e05-336">System.IO.TextWriter</span></span>|<span data-ttu-id="f7e05-337">pro výstup zprávy nastavte na hodnotu null lze použít konzolu</span><span class="sxs-lookup"><span data-stu-id="f7e05-337">for output messages, set to null to use Console</span></span>|

<span data-ttu-id="f7e05-338">**Vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="f7e05-338">**Properties**</span></span>

|<span data-ttu-id="f7e05-339">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f7e05-339">Property</span></span>|<span data-ttu-id="f7e05-340">Typ</span><span class="sxs-lookup"><span data-stu-id="f7e05-340">Type</span></span>|<span data-ttu-id="f7e05-341">Popis</span><span class="sxs-lookup"><span data-stu-id="f7e05-341">Description</span></span>|
|--------|----|-----------|
|<span data-ttu-id="f7e05-342">AlgebraPath</span><span class="sxs-lookup"><span data-stu-id="f7e05-342">AlgebraPath</span></span>|<span data-ttu-id="f7e05-343">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f7e05-343">string</span></span>|<span data-ttu-id="f7e05-344">Cesta k souboru algebra (algebra souboru je jednomu z výsledků kompilace)</span><span class="sxs-lookup"><span data-stu-id="f7e05-344">The path to algebra file (algebra file is one of the compilation results)</span></span>|
|<span data-ttu-id="f7e05-345">CodeBehindReferences</span><span class="sxs-lookup"><span data-stu-id="f7e05-345">CodeBehindReferences</span></span>|<span data-ttu-id="f7e05-346">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f7e05-346">string</span></span>|<span data-ttu-id="f7e05-347">Pokud skript má další kódu na pozadí odkazy, zadejte cesty odděleny znakem ";"</span><span class="sxs-lookup"><span data-stu-id="f7e05-347">If the script has additional code behind references, specify the paths separated with ';'</span></span>|
|<span data-ttu-id="f7e05-348">CppSdkDir</span><span class="sxs-lookup"><span data-stu-id="f7e05-348">CppSdkDir</span></span>|<span data-ttu-id="f7e05-349">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f7e05-349">string</span></span>|<span data-ttu-id="f7e05-350">CppSDK adresáře</span><span class="sxs-lookup"><span data-stu-id="f7e05-350">CppSDK directory</span></span>|
|<span data-ttu-id="f7e05-351">CurrentDir</span><span class="sxs-lookup"><span data-stu-id="f7e05-351">CurrentDir</span></span>|<span data-ttu-id="f7e05-352">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f7e05-352">string</span></span>|<span data-ttu-id="f7e05-353">Aktuální adresář</span><span class="sxs-lookup"><span data-stu-id="f7e05-353">Current directory</span></span>|
|<span data-ttu-id="f7e05-354">DataRoot</span><span class="sxs-lookup"><span data-stu-id="f7e05-354">DataRoot</span></span>|<span data-ttu-id="f7e05-355">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f7e05-355">string</span></span>|<span data-ttu-id="f7e05-356">Kořenová cesta dat</span><span class="sxs-lookup"><span data-stu-id="f7e05-356">Data root path</span></span>|
|<span data-ttu-id="f7e05-357">DebuggerMailPath</span><span class="sxs-lookup"><span data-stu-id="f7e05-357">DebuggerMailPath</span></span>|<span data-ttu-id="f7e05-358">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f7e05-358">string</span></span>|<span data-ttu-id="f7e05-359">Cesta k zásuvky pošty ladicí program</span><span class="sxs-lookup"><span data-stu-id="f7e05-359">The path to debugger mailslot</span></span>|
|<span data-ttu-id="f7e05-360">GenerateUdoRedirect</span><span class="sxs-lookup"><span data-stu-id="f7e05-360">GenerateUdoRedirect</span></span>|<span data-ttu-id="f7e05-361">BOOL</span><span class="sxs-lookup"><span data-stu-id="f7e05-361">bool</span></span>|<span data-ttu-id="f7e05-362">Pokud chcete generovat načítání sestavení přesměrování přepsat konfigurace</span><span class="sxs-lookup"><span data-stu-id="f7e05-362">If we want to generate assembly loading redirection override config</span></span>|
|<span data-ttu-id="f7e05-363">HasCodeBehind</span><span class="sxs-lookup"><span data-stu-id="f7e05-363">HasCodeBehind</span></span>|<span data-ttu-id="f7e05-364">BOOL</span><span class="sxs-lookup"><span data-stu-id="f7e05-364">bool</span></span>|<span data-ttu-id="f7e05-365">Pokud skript má kódu na pozadí</span><span class="sxs-lookup"><span data-stu-id="f7e05-365">If the script has code behind</span></span>|
|<span data-ttu-id="f7e05-366">InputDir</span><span class="sxs-lookup"><span data-stu-id="f7e05-366">InputDir</span></span>|<span data-ttu-id="f7e05-367">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f7e05-367">string</span></span>|<span data-ttu-id="f7e05-368">Adresář pro vstupní data</span><span class="sxs-lookup"><span data-stu-id="f7e05-368">Directory for input data</span></span>|
|<span data-ttu-id="f7e05-369">MessagePath</span><span class="sxs-lookup"><span data-stu-id="f7e05-369">MessagePath</span></span>|<span data-ttu-id="f7e05-370">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f7e05-370">string</span></span>|<span data-ttu-id="f7e05-371">Cesta k souboru výpisu zpráv</span><span class="sxs-lookup"><span data-stu-id="f7e05-371">Message dump file path</span></span>|
|<span data-ttu-id="f7e05-372">OutputDir</span><span class="sxs-lookup"><span data-stu-id="f7e05-372">OutputDir</span></span>|<span data-ttu-id="f7e05-373">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f7e05-373">string</span></span>|<span data-ttu-id="f7e05-374">Adresář pro výstupní data</span><span class="sxs-lookup"><span data-stu-id="f7e05-374">Directory for output data</span></span>|
|<span data-ttu-id="f7e05-375">Paralelismus</span><span class="sxs-lookup"><span data-stu-id="f7e05-375">Parallelism</span></span>|<span data-ttu-id="f7e05-376">celá čísla</span><span class="sxs-lookup"><span data-stu-id="f7e05-376">int</span></span>|<span data-ttu-id="f7e05-377">Ke spuštění algebra paralelismus</span><span class="sxs-lookup"><span data-stu-id="f7e05-377">Parallelism to run the algebra</span></span>|
|<span data-ttu-id="f7e05-378">ParentPid</span><span class="sxs-lookup"><span data-stu-id="f7e05-378">ParentPid</span></span>|<span data-ttu-id="f7e05-379">celá čísla</span><span class="sxs-lookup"><span data-stu-id="f7e05-379">int</span></span>|<span data-ttu-id="f7e05-380">ID nadřazeného objektu, na kterém služba monitoruje ukončíte, nastavena na hodnotu 0 nebo ignorovat záporná hodnota.</span><span class="sxs-lookup"><span data-stu-id="f7e05-380">PID of the parent on which the service monitors to exit, set to 0 or negative to ignore</span></span>|
|<span data-ttu-id="f7e05-381">ResultPath</span><span class="sxs-lookup"><span data-stu-id="f7e05-381">ResultPath</span></span>|<span data-ttu-id="f7e05-382">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f7e05-382">string</span></span>|<span data-ttu-id="f7e05-383">Cesta k souboru výpisu výsledek</span><span class="sxs-lookup"><span data-stu-id="f7e05-383">Result dump file path</span></span>|
|<span data-ttu-id="f7e05-384">RuntimeDir</span><span class="sxs-lookup"><span data-stu-id="f7e05-384">RuntimeDir</span></span>|<span data-ttu-id="f7e05-385">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f7e05-385">string</span></span>|<span data-ttu-id="f7e05-386">Adresář modulu runtime</span><span class="sxs-lookup"><span data-stu-id="f7e05-386">Runtime directory</span></span>|
|<span data-ttu-id="f7e05-387">scriptPath</span><span class="sxs-lookup"><span data-stu-id="f7e05-387">ScriptPath</span></span>|<span data-ttu-id="f7e05-388">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f7e05-388">string</span></span>|<span data-ttu-id="f7e05-389">Kde najít skriptu</span><span class="sxs-lookup"><span data-stu-id="f7e05-389">Where to find the script</span></span>|
|<span data-ttu-id="f7e05-390">Bez podstruktury</span><span class="sxs-lookup"><span data-stu-id="f7e05-390">Shallow</span></span>|<span data-ttu-id="f7e05-391">BOOL</span><span class="sxs-lookup"><span data-stu-id="f7e05-391">bool</span></span>|<span data-ttu-id="f7e05-392">Nedávná kompilace, nebo ne</span><span class="sxs-lookup"><span data-stu-id="f7e05-392">Shallow compile or not</span></span>|
|<span data-ttu-id="f7e05-393">TempDir</span><span class="sxs-lookup"><span data-stu-id="f7e05-393">TempDir</span></span>|<span data-ttu-id="f7e05-394">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f7e05-394">string</span></span>|<span data-ttu-id="f7e05-395">Dočasný adresář</span><span class="sxs-lookup"><span data-stu-id="f7e05-395">Temp directory</span></span>|
|<span data-ttu-id="f7e05-396">UseDataBase</span><span class="sxs-lookup"><span data-stu-id="f7e05-396">UseDataBase</span></span>|<span data-ttu-id="f7e05-397">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f7e05-397">string</span></span>|<span data-ttu-id="f7e05-398">Zadejte databázi, kterou chcete použít pro kódu na pozadí dočasné sestavení registrace, hlavní ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="f7e05-398">Specify the database to use for code behind temporary assembly registration, master by default</span></span>|
|<span data-ttu-id="f7e05-399">WorkDir</span><span class="sxs-lookup"><span data-stu-id="f7e05-399">WorkDir</span></span>|<span data-ttu-id="f7e05-400">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f7e05-400">string</span></span>|<span data-ttu-id="f7e05-401">Upřednostňované pracovní adresář</span><span class="sxs-lookup"><span data-stu-id="f7e05-401">Preferred working directory</span></span>|


<span data-ttu-id="f7e05-402">**– Metoda**</span><span class="sxs-lookup"><span data-stu-id="f7e05-402">**Method**</span></span>

|<span data-ttu-id="f7e05-403">Metoda</span><span class="sxs-lookup"><span data-stu-id="f7e05-403">Method</span></span>|<span data-ttu-id="f7e05-404">Popis</span><span class="sxs-lookup"><span data-stu-id="f7e05-404">Description</span></span>|<span data-ttu-id="f7e05-405">Vrátí</span><span class="sxs-lookup"><span data-stu-id="f7e05-405">Return</span></span>|<span data-ttu-id="f7e05-406">Parametr</span><span class="sxs-lookup"><span data-stu-id="f7e05-406">Parameter</span></span>|
|------|-----------|------|---------|
|<span data-ttu-id="f7e05-407">veřejné bool DoCompile()</span><span class="sxs-lookup"><span data-stu-id="f7e05-407">public bool DoCompile()</span></span>|<span data-ttu-id="f7e05-408">Kompilace skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="f7e05-408">Compile the U-SQL script</span></span>|<span data-ttu-id="f7e05-409">Hodnota TRUE, v případě úspěchu</span><span class="sxs-lookup"><span data-stu-id="f7e05-409">True on success</span></span>| |
|<span data-ttu-id="f7e05-410">veřejné bool DoExec()</span><span class="sxs-lookup"><span data-stu-id="f7e05-410">public bool DoExec()</span></span>|<span data-ttu-id="f7e05-411">Kompilované výsledek spuštění</span><span class="sxs-lookup"><span data-stu-id="f7e05-411">Execute the compiled result</span></span>|<span data-ttu-id="f7e05-412">Hodnota TRUE, v případě úspěchu</span><span class="sxs-lookup"><span data-stu-id="f7e05-412">True on success</span></span>| |
|<span data-ttu-id="f7e05-413">veřejné bool DoRun()</span><span class="sxs-lookup"><span data-stu-id="f7e05-413">public bool DoRun()</span></span>|<span data-ttu-id="f7e05-414">Spusťte skript U-SQL (kompilace + spouštění)</span><span class="sxs-lookup"><span data-stu-id="f7e05-414">Run the U-SQL script (Compile + Execute)</span></span>|<span data-ttu-id="f7e05-415">Hodnota TRUE, v případě úspěchu</span><span class="sxs-lookup"><span data-stu-id="f7e05-415">True on success</span></span>| |
|<span data-ttu-id="f7e05-416">veřejné bool IsValidRuntimeDir (cesta řetězec)</span><span class="sxs-lookup"><span data-stu-id="f7e05-416">public bool IsValidRuntimeDir(string path)</span></span>|<span data-ttu-id="f7e05-417">Zkontrolujte, jestli dané cesty je cesta platná runtime</span><span class="sxs-lookup"><span data-stu-id="f7e05-417">Check if the given path is valid runtime path</span></span>|<span data-ttu-id="f7e05-418">Platí pro platný</span><span class="sxs-lookup"><span data-stu-id="f7e05-418">True for valid</span></span>|<span data-ttu-id="f7e05-419">Cesta adresář modulu runtime</span><span class="sxs-lookup"><span data-stu-id="f7e05-419">The path of runtime directory</span></span>|


## <a name="faq-about-common-issue"></a><span data-ttu-id="f7e05-420">Nejčastější dotazy o běžné problémy</span><span class="sxs-lookup"><span data-stu-id="f7e05-420">FAQ about common issue</span></span>

### <a name="error-1"></a><span data-ttu-id="f7e05-421">1. Chyba:</span><span class="sxs-lookup"><span data-stu-id="f7e05-421">Error 1:</span></span>
<span data-ttu-id="f7e05-422">E_CSC_SYSTEM_INTERNAL: Vnitřní chyba!</span><span class="sxs-lookup"><span data-stu-id="f7e05-422">E_CSC_SYSTEM_INTERNAL: Internal error!</span></span> <span data-ttu-id="f7e05-423">Nepodařilo se načíst soubor nebo sestavení 'ScopeEngineManaged.dll nebo jednu ze závislostí.</span><span class="sxs-lookup"><span data-stu-id="f7e05-423">Could not load file or assembly 'ScopeEngineManaged.dll' or one of its dependencies.</span></span> <span data-ttu-id="f7e05-424">Zadaný modul nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="f7e05-424">The specified module could not be found.</span></span>

<span data-ttu-id="f7e05-425">Zkontrolujte následující:</span><span class="sxs-lookup"><span data-stu-id="f7e05-425">Please check the following:</span></span>

- <span data-ttu-id="f7e05-426">Zajistěte, aby byla x64 prostředí.</span><span class="sxs-lookup"><span data-stu-id="f7e05-426">Make sure you have x64 environment.</span></span> <span data-ttu-id="f7e05-427">Cílové platformy sestavení a testovací prostředí by měl být x64, odkazovat na **krok 1: vytvoření C# jednotkové testování projektu a konfigurace** výše.</span><span class="sxs-lookup"><span data-stu-id="f7e05-427">The build target platform and the test environment should be x64, refer to **Step 1: Create C# unit test project and configuration** above.</span></span>
- <span data-ttu-id="f7e05-428">Ujistěte se, že jste zkopírovali všechny soubory závislosti v rámci NugetPackage\build\runtime\ do projektu pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="f7e05-428">Make sure you have copied all dependency files under NugetPackage\build\runtime\ to project working directory.</span></span>


## <a name="next-steps"></a><span data-ttu-id="f7e05-429">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f7e05-429">Next steps</span></span>

* <span data-ttu-id="f7e05-430">Pokud se chcete naučit jazyk U-SQL, informace najdete v tématu [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f7e05-430">To learn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="f7e05-431">Pokud chcete protokolovat diagnostické informace, přečtěte si téma [přístup k protokolů diagnostiky pro Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="f7e05-431">To log diagnostics information, see [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).</span></span>
* <span data-ttu-id="f7e05-432">Pokud chcete zobrazit komplexnější dotaz, najdete v části [analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="f7e05-432">To see a more complex query, see [Analyze website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="f7e05-433">Chcete-li zobrazit podrobnosti o úlohách, najdete v části [použití úlohy prohlížeče a zobrazení úloh pro úlohy Azure Data Lake Analytics](data-lake-analytics-data-lake-tools-view-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="f7e05-433">To view job details, see [Use Job Browser and Job View for Azure Data Lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md).</span></span>
* <span data-ttu-id="f7e05-434">Chcete-li použít zobrazení provádění vrcholů, přečtěte si téma [použít zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span><span class="sxs-lookup"><span data-stu-id="f7e05-434">To use the vertex execution view, see [Use the Vertex Execution View in Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).</span></span>
