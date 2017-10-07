---
title: "doporučení aaaGenerate pomocí Mahout HDInsight z prostředí PowerShell – Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello Apache Mahout strojového učení knihovny toogenerate doporučení s HDInsight (Hadoop) ze skriptu prostředí PowerShell systémem vašeho klienta."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 07b57208-32aa-4e59-900a-6c934fa1b7a7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: 675a2cd8ecaf7fc797d6cd094e4e58f9aca7ed92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight-powershell"></a><span data-ttu-id="20a50-103">Generování doporučení pomocí Apache Mahout s Hadoop v HDInsight (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="20a50-103">Generate movie recommendations by using Apache Mahout with Hadoop in HDInsight (PowerShell)</span></span>

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

<span data-ttu-id="20a50-104">Zjistěte, jak toouse hello [Apache Mahout](http://mahout.apache.org) knihovny machine learning s Azure HDInsight toogenerate doporučení.</span><span class="sxs-lookup"><span data-stu-id="20a50-104">Learn how toouse hello [Apache Mahout](http://mahout.apache.org) machine learning library with Azure HDInsight toogenerate movie recommendations.</span></span> <span data-ttu-id="20a50-105">Příklad Hello v tomto dokumentu používá prostředí Azure PowerShell toorun Mahout úlohy.</span><span class="sxs-lookup"><span data-stu-id="20a50-105">hello example in this document uses Azure PowerShell toorun Mahout jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="20a50-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="20a50-106">Prerequisites</span></span>

* <span data-ttu-id="20a50-107">Cluster HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="20a50-107">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="20a50-108">Informace o vytváření jeden najdete v tématu [začít používat systémem Linux Hadoop v HDInsight][getstarted].</span><span class="sxs-lookup"><span data-stu-id="20a50-108">For information about creating one, see [Get started using Linux-based Hadoop in HDInsight][getstarted].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="20a50-109">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="20a50-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="20a50-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="20a50-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* [<span data-ttu-id="20a50-111">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="20a50-111">Azure PowerShell</span></span>](/powershell/azure/overview)

## <span data-ttu-id="20a50-112"><a name="recommendations"></a>Generování doporučení pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="20a50-112"><a name="recommendations"></a>Generate recommendations by using Azure PowerShell</span></span>

> [!WARNING]
> <span data-ttu-id="20a50-113">Hello úlohy v této části funguje tak, že pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="20a50-113">hello job in this section works by using Azure PowerShell.</span></span> <span data-ttu-id="20a50-114">Řadu hello třídy součástí Mahout nefungují aktuálně s prostředím Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="20a50-114">Many of hello classes provided with Mahout do not currently work with Azure PowerShell.</span></span> <span data-ttu-id="20a50-115">Seznam tříd, které nefungují s prostředím Azure PowerShell najdete v tématu hello [Poradce při potížích s](#troubleshooting) části.</span><span class="sxs-lookup"><span data-stu-id="20a50-115">For a list of classes that do not work with Azure PowerShell, see hello [Troubleshooting](#troubleshooting) section.</span></span>
>
> <span data-ttu-id="20a50-116">Příklad použití SSH tooconnect tooHDInsight a spuštění příkladů Mahout přímo na clusteru hello, naleznete v části [generování doporučení pomocí Mahout a HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).</span><span class="sxs-lookup"><span data-stu-id="20a50-116">For an example of using SSH tooconnect tooHDInsight and run Mahout examples directly on hello cluster, see [Generate movie recommendations using Mahout and HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).</span></span>

<span data-ttu-id="20a50-117">Jedním z hello funkce, které zajišťuje Mahout je modul doporučení.</span><span class="sxs-lookup"><span data-stu-id="20a50-117">One of hello functions that is provided by Mahout is a recommendation engine.</span></span> <span data-ttu-id="20a50-118">Tento modul přijímá data ve formátu hello `userID`, `itemId`, a `prefValue` (hello předvoleb uživatele pro položku hello).</span><span class="sxs-lookup"><span data-stu-id="20a50-118">This engine accepts data in hello format of `userID`, `itemId`, and `prefValue` (hello users preference for hello item).</span></span> <span data-ttu-id="20a50-119">Mahout používá hello data toodetermine uživatelé s jako položky předvoleb, které můžou být použité toomake doporučení.</span><span class="sxs-lookup"><span data-stu-id="20a50-119">Mahout uses hello data toodetermine users with like-item preferences, which can be used toomake recommendations.</span></span>

<span data-ttu-id="20a50-120">Hello následující příklad je zjednodušený návod, jak funguje hello doporučení proces:</span><span class="sxs-lookup"><span data-stu-id="20a50-120">hello following example is a simplified walk-through of how hello recommendation process works:</span></span>

* <span data-ttu-id="20a50-121">**společné výskyt**: Jan, Alice a Bob všechny líbilo *hvězdičky válek*, *hello Empire stávky zpět*, a *návrat hello Jedi*.</span><span class="sxs-lookup"><span data-stu-id="20a50-121">**co-occurrence**: Joe, Alice, and Bob all liked *Star Wars*, *hello Empire Strikes Back*, and *Return of hello Jedi*.</span></span> <span data-ttu-id="20a50-122">Mahout Určuje, že uživatelé, kteří jako některého z těchto filmy chtěl také hello další dvě.</span><span class="sxs-lookup"><span data-stu-id="20a50-122">Mahout determines that users who like any one of these movies also like hello other two.</span></span>

* <span data-ttu-id="20a50-123">**společné výskyt**: Bob a Alice také líbilo *hello fiktivní Menace*, *útoku hello klonů*, a *odvety hello Sith*.</span><span class="sxs-lookup"><span data-stu-id="20a50-123">**co-occurrence**: Bob and Alice also liked *hello Phantom Menace*, *Attack of hello Clones*, and *Revenge of hello Sith*.</span></span> <span data-ttu-id="20a50-124">Mahout Určuje, že uživatelé, kteří líbilo hello předchozí tři filmy také jako tyto filmy.</span><span class="sxs-lookup"><span data-stu-id="20a50-124">Mahout determines that users who liked hello previous three movies also like these movies.</span></span>

* <span data-ttu-id="20a50-125">**Podobnosti doporučení**: protože Jan líbilo hello první tři filmy, Mahout vypadá na filmy ostatní s líbilo podobných předvolby, ale nebyla Jan sledovaná (líbilo nebo hodnocení).</span><span class="sxs-lookup"><span data-stu-id="20a50-125">**Similarity recommendation**: Because Joe liked hello first three movies, Mahout looks at movies that others with similar preferences liked, but Joe has not watched (liked/rated).</span></span> <span data-ttu-id="20a50-126">V takovém případě se doporučuje Mahout *hello fiktivní Menace*, *útoku hello klonů*, a *odvety hello Sith*.</span><span class="sxs-lookup"><span data-stu-id="20a50-126">In this case, Mahout recommends *hello Phantom Menace*, *Attack of hello Clones*, and *Revenge of hello Sith*.</span></span>

### <a name="understanding-hello-data"></a><span data-ttu-id="20a50-127">Principy hello dat</span><span class="sxs-lookup"><span data-stu-id="20a50-127">Understanding hello data</span></span>

<span data-ttu-id="20a50-128">[GroupLens Research] [ movielens] poskytuje hodnocení data pro filmy ve formátu, který je kompatibilní s Mahout.</span><span class="sxs-lookup"><span data-stu-id="20a50-128">[GroupLens Research][movielens] provides rating data for movies in a format that is compatible with Mahout.</span></span> <span data-ttu-id="20a50-129">Tato data jsou k dispozici na hello výchozí úložiště pro váš cluster v `/HdiSamples//HdiSamples/MahoutMovieData`.</span><span class="sxs-lookup"><span data-stu-id="20a50-129">This data is available on hello default storage for your cluster at `/HdiSamples//HdiSamples/MahoutMovieData`.</span></span>

<span data-ttu-id="20a50-130">Existují dva soubory, `moviedb.txt` (informace o filmy hello) a `user-ratings.txt`.</span><span class="sxs-lookup"><span data-stu-id="20a50-130">There are two files, `moviedb.txt` (information about hello movies) and `user-ratings.txt`.</span></span> <span data-ttu-id="20a50-131">Hello `user-ratings.txt` soubor se používá během analýzy.</span><span class="sxs-lookup"><span data-stu-id="20a50-131">hello `user-ratings.txt` file is used during analysis.</span></span> <span data-ttu-id="20a50-132">Hello `moviedb.txt` souboru je použité tooprovide popisný text při zobrazení hello výsledky analýzy hello.</span><span class="sxs-lookup"><span data-stu-id="20a50-132">hello `moviedb.txt` file is used tooprovide user-friendly text when displaying hello results of hello analysis.</span></span>

<span data-ttu-id="20a50-133">Hello data obsažená v ratings.txt uživatel má struktura `userID`, `movieID`, `userRating`, a `timestamp`, který víme, jak vysoce každý uživatel hodnocení filmu.</span><span class="sxs-lookup"><span data-stu-id="20a50-133">hello data contained in user-ratings.txt has a structure of `userID`, `movieID`, `userRating`, and `timestamp`, which tells us how highly each user rated a movie.</span></span> <span data-ttu-id="20a50-134">Tady je příklad hello dat:</span><span class="sxs-lookup"><span data-stu-id="20a50-134">Here is an example of hello data:</span></span>

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

### <a name="run-hello-job"></a><span data-ttu-id="20a50-135">Spustit úlohu hello</span><span class="sxs-lookup"><span data-stu-id="20a50-135">Run hello job</span></span>

<span data-ttu-id="20a50-136">Použijte následující toorun skriptu prostředí Windows PowerShell úlohu, která používá modul doporučení Mahout hello s daty film hello hello:</span><span class="sxs-lookup"><span data-stu-id="20a50-136">Use hello following Windows PowerShell script toorun a job that uses hello Mahout recommendation engine with hello movie data:</span></span>

> [!NOTE]
> <span data-ttu-id="20a50-137">Tento soubor vyzve k zadání informací, které jsou používané tooconnect cluster HDInsight tooyour a spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="20a50-137">This file prompts you for information that is used tooconnect tooyour HDInsight cluster and run jobs.</span></span> <span data-ttu-id="20a50-138">Může trvat několik minut, než toocomplete hello úlohy a stáhněte soubor výstup.txt hello.</span><span class="sxs-lookup"><span data-stu-id="20a50-138">It may take several minutes for hello jobs toocomplete and download hello output.txt file.</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=5-98)]

> [!NOTE]
> <span data-ttu-id="20a50-139">Mahout úlohy neodebírejte dočasná data, který se vytvoří při zpracování úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="20a50-139">Mahout jobs do not remove temporary data that is created while processing hello job.</span></span> <span data-ttu-id="20a50-140">Hello `--tempDir` je zadán parametr v hello příklad úlohy tooisolate hello dočasné soubory do konkrétního adresáře.</span><span class="sxs-lookup"><span data-stu-id="20a50-140">hello `--tempDir` parameter is specified in hello example job tooisolate hello temporary files into a specific directory.</span></span>

<span data-ttu-id="20a50-141">Úloha Mahout Hello nevrací tooSTDOUT výstup hello.</span><span class="sxs-lookup"><span data-stu-id="20a50-141">hello Mahout job does not return hello output tooSTDOUT.</span></span> <span data-ttu-id="20a50-142">Místo toho je uložený v zadané výstupní adresář hello jako **část r-00000**.</span><span class="sxs-lookup"><span data-stu-id="20a50-142">Instead, it stores it in hello specified output directory as **part-r-00000**.</span></span> <span data-ttu-id="20a50-143">skript Hello soubory ke stažení tohoto souboru příliš**výstup.txt** hello aktuálního adresáře na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="20a50-143">hello script downloads this file too**output.txt** in hello current directory on your workstation.</span></span>

<span data-ttu-id="20a50-144">Hello následující text je příkladem hello obsah tohoto souboru:</span><span class="sxs-lookup"><span data-stu-id="20a50-144">hello following text is an example of hello content of this file:</span></span>

    1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

<span data-ttu-id="20a50-145">první sloupec Hello je hello `userID`.</span><span class="sxs-lookup"><span data-stu-id="20a50-145">hello first column is hello `userID`.</span></span> <span data-ttu-id="20a50-146">Hello hodnoty obsažené v ' [' a ']' jsou `movieId`:`recommendationScore`.</span><span class="sxs-lookup"><span data-stu-id="20a50-146">hello values contained in '[' and ']' are `movieId`:`recommendationScore`.</span></span>

<span data-ttu-id="20a50-147">skript Hello také stáhne hello `moviedb.txt` a `user-ratings.txt` soubory, které jsou potřebné tooformat hello výstup toobe srozumitelnější.</span><span class="sxs-lookup"><span data-stu-id="20a50-147">hello script also downloads hello `moviedb.txt` and `user-ratings.txt` files, which are needed tooformat hello output toobe more readable.</span></span>

### <a name="view-hello-output"></a><span data-ttu-id="20a50-148">Výstup hello zobrazení</span><span class="sxs-lookup"><span data-stu-id="20a50-148">View hello output</span></span>

<span data-ttu-id="20a50-149">I když hello generovaný výstup může být OK pro použití v aplikaci, není uživatelsky přívětivý.</span><span class="sxs-lookup"><span data-stu-id="20a50-149">Although hello generated output might be OK for use in an application, it's not user-friendly.</span></span> <span data-ttu-id="20a50-150">Hello `moviedb.txt` z hello může být server hello použité tooresolve `movieId` tooa film název.</span><span class="sxs-lookup"><span data-stu-id="20a50-150">hello `moviedb.txt` from hello server can be used tooresolve hello `movieId` tooa movie name.</span></span> <span data-ttu-id="20a50-151">Použijte následující doporučení toodisplay skript prostředí PowerShell s názvy film hello:</span><span class="sxs-lookup"><span data-stu-id="20a50-151">Use hello following PowerShell script toodisplay recommendations with movie names:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=106-180)]

<span data-ttu-id="20a50-152">Použijte následující příkaz toodisplay hello doporučení v uživatelsky přívětivý formátu hello:</span><span class="sxs-lookup"><span data-stu-id="20a50-152">Use hello following command toodisplay hello recommendations in a user-friendly format:</span></span> 

```powershell
.\show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt
```

<span data-ttu-id="20a50-153">Hello výstup je podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="20a50-153">hello output is similar toohello following text:</span></span>

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, hello (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of hello Dove, hello (1997)            4.6666665
    People vs. Larry Flynt, hello (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237

## <span data-ttu-id="20a50-154"><a name="troubleshooting"></a>Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="20a50-154"><a name="troubleshooting"></a>Troubleshooting</span></span>

### <a name="cannot-overwrite-files"></a><span data-ttu-id="20a50-155">Nelze přepsat soubory</span><span class="sxs-lookup"><span data-stu-id="20a50-155">Cannot overwrite files</span></span>

<span data-ttu-id="20a50-156">Mahout úlohy se vyčistit dočasné soubory, které byly vytvořeny během zpracování.</span><span class="sxs-lookup"><span data-stu-id="20a50-156">Mahout jobs do not clean up temporary files that were created during processing.</span></span> <span data-ttu-id="20a50-157">Kromě toho hello úlohy Nepřepisovat existující výstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="20a50-157">In addition, hello jobs do not overwrite existing output file.</span></span>

<span data-ttu-id="20a50-158">tooavoid chyby při spuštění úlohy Mahout se odstranit dočasné a výstupní soubory mezi spustí.</span><span class="sxs-lookup"><span data-stu-id="20a50-158">tooavoid errors when running Mahout jobs, delete temporary and output files between runs.</span></span> <span data-ttu-id="20a50-159">vytvořené soubory hello tooremove hello starší skripty v tomto dokumentu pomocí hello následující skript prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="20a50-159">tooremove hello files created by hello earlier scripts in this document, use hello following PowerShell script:</span></span>

```powershell
# Login tooyour Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter hello HDInsight cluster name"
$creds=Get-Credential -Message "Enter hello login for hello cluster"

#Get hello cluster info so we can get hello resource group, storage, etc.
$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
$container = $clusterInfo.DefaultStorageContainer
$storageAccountKey = (Get-AzureRmStorageAccountKey `
    -Name $storageAccountName `
-ResourceGroupName $resourceGroup)[0].Value

#Create a storage context and upload hello file
$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

#Azure PowerShell can't delete blobs using wildcard,
#so have tooget a list and delete one at a time
# Start with hello output
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/out"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
# Next hello temp files
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/temp"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
```

### <span data-ttu-id="20a50-160"><a name="nopowershell"></a>Třídy, které nefungují s prostředím Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="20a50-160"><a name="nopowershell"></a>Classes that do not work with Azure PowerShell</span></span>

<span data-ttu-id="20a50-161">Mahout úlohy, které používají následující třídy hello vrátit různé chybové zprávy při použití v prostředí Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="20a50-161">Mahout jobs that use hello following classes return various error messages when used from Windows PowerShell:</span></span>

* <span data-ttu-id="20a50-162">org.apache.mahout.utils.clustering.ClusterDumper</span><span class="sxs-lookup"><span data-stu-id="20a50-162">org.apache.mahout.utils.clustering.ClusterDumper</span></span>
* <span data-ttu-id="20a50-163">org.apache.mahout.utils.SequenceFileDumper</span><span class="sxs-lookup"><span data-stu-id="20a50-163">org.apache.mahout.utils.SequenceFileDumper</span></span>
* <span data-ttu-id="20a50-164">org.apache.mahout.utils.vectors.lucene.Driver</span><span class="sxs-lookup"><span data-stu-id="20a50-164">org.apache.mahout.utils.vectors.lucene.Driver</span></span>
* <span data-ttu-id="20a50-165">org.apache.mahout.utils.vectors.arff.Driver</span><span class="sxs-lookup"><span data-stu-id="20a50-165">org.apache.mahout.utils.vectors.arff.Driver</span></span>
* <span data-ttu-id="20a50-166">org.apache.mahout.text.WikipediaToSequenceFile</span><span class="sxs-lookup"><span data-stu-id="20a50-166">org.apache.mahout.text.WikipediaToSequenceFile</span></span>
* <span data-ttu-id="20a50-167">org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles</span><span class="sxs-lookup"><span data-stu-id="20a50-167">org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles</span></span>
* <span data-ttu-id="20a50-168">org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer</span><span class="sxs-lookup"><span data-stu-id="20a50-168">org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer</span></span>
* <span data-ttu-id="20a50-169">org.apache.mahout.classifier.sgd.TrainLogistic</span><span class="sxs-lookup"><span data-stu-id="20a50-169">org.apache.mahout.classifier.sgd.TrainLogistic</span></span>
* <span data-ttu-id="20a50-170">org.apache.mahout.classifier.sgd.RunLogistic</span><span class="sxs-lookup"><span data-stu-id="20a50-170">org.apache.mahout.classifier.sgd.RunLogistic</span></span>
* <span data-ttu-id="20a50-171">org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic</span><span class="sxs-lookup"><span data-stu-id="20a50-171">org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic</span></span>
* <span data-ttu-id="20a50-172">org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic</span><span class="sxs-lookup"><span data-stu-id="20a50-172">org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic</span></span>
* <span data-ttu-id="20a50-173">org.apache.mahout.classifier.sgd.RunAdaptiveLogistic</span><span class="sxs-lookup"><span data-stu-id="20a50-173">org.apache.mahout.classifier.sgd.RunAdaptiveLogistic</span></span>
* <span data-ttu-id="20a50-174">org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer</span><span class="sxs-lookup"><span data-stu-id="20a50-174">org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer</span></span>
* <span data-ttu-id="20a50-175">org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator</span><span class="sxs-lookup"><span data-stu-id="20a50-175">org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator</span></span>
* <span data-ttu-id="20a50-176">org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator</span><span class="sxs-lookup"><span data-stu-id="20a50-176">org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator</span></span>
* <span data-ttu-id="20a50-177">org.apache.mahout.classifier.df.tools.Describe</span><span class="sxs-lookup"><span data-stu-id="20a50-177">org.apache.mahout.classifier.df.tools.Describe</span></span>

<span data-ttu-id="20a50-178">toorun úlohy, které používají tyto třídy připojit toohello clusteru HDInsight pomocí SSH a spouštění úloh hello z příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="20a50-178">toorun jobs that use these classes, connect toohello HDInsight cluster using SSH and run hello jobs from hello command line.</span></span> <span data-ttu-id="20a50-179">Příklad použití SSH toorun Mahout úloh naleznete v části [generování doporučení pomocí Mahout a HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).</span><span class="sxs-lookup"><span data-stu-id="20a50-179">For an example of using SSH toorun Mahout jobs, see [Generate movie recommendations using Mahout and HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="20a50-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="20a50-180">Next steps</span></span>

<span data-ttu-id="20a50-181">Teď, když jste se naučili, jak toouse Mahout, zjišťovat další způsoby, jak práci s daty v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="20a50-181">Now that you have learned how toouse Mahout, discover other ways of working with data on HDInsight:</span></span>

* [<span data-ttu-id="20a50-182">Hive s HDInsight</span><span class="sxs-lookup"><span data-stu-id="20a50-182">Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="20a50-183">Pig s HDInsight</span><span class="sxs-lookup"><span data-stu-id="20a50-183">Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="20a50-184">MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="20a50-184">MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: /powershell/azureps-cmdlets-docs
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
