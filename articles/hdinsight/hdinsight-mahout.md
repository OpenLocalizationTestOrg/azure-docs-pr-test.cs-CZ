---
title: "Generování doporučení pomocí Mahout HDInsight z prostředí PowerShell – Azure | Microsoft Docs"
description: "Další informace o použití Apache Mahout strojového učení knihovny pro generování doporučení s HDInsight (Hadoop) ze skriptu prostředí PowerShell systémem vašeho klienta."
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
ms.openlocfilehash: 934de9ca2df48b29ef7a56d5729d59d77875ea7b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight-powershell"></a><span data-ttu-id="cea7f-103">Generování doporučení pomocí Apache Mahout s Hadoop v HDInsight (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="cea7f-103">Generate movie recommendations by using Apache Mahout with Hadoop in HDInsight (PowerShell)</span></span>

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

<span data-ttu-id="cea7f-104">Další informace o použití [Apache Mahout](http://mahout.apache.org) knihovny machine learning s Azure HDInsight ke generování doporučení.</span><span class="sxs-lookup"><span data-stu-id="cea7f-104">Learn how to use the [Apache Mahout](http://mahout.apache.org) machine learning library with Azure HDInsight to generate movie recommendations.</span></span> <span data-ttu-id="cea7f-105">V příkladu v tomto dokumentu používá ke spuštění úlohy Mahout prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cea7f-105">The example in this document uses Azure PowerShell to run Mahout jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cea7f-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cea7f-106">Prerequisites</span></span>

* <span data-ttu-id="cea7f-107">Cluster HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="cea7f-107">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="cea7f-108">Informace o vytváření jeden najdete v tématu [začít používat systémem Linux Hadoop v HDInsight][getstarted].</span><span class="sxs-lookup"><span data-stu-id="cea7f-108">For information about creating one, see [Get started using Linux-based Hadoop in HDInsight][getstarted].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cea7f-109">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="cea7f-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="cea7f-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="cea7f-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* [<span data-ttu-id="cea7f-111">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="cea7f-111">Azure PowerShell</span></span>](/powershell/azure/overview)

## <span data-ttu-id="cea7f-112"><a name="recommendations"></a>Generování doporučení pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="cea7f-112"><a name="recommendations"></a>Generate recommendations by using Azure PowerShell</span></span>

> [!WARNING]
> <span data-ttu-id="cea7f-113">Úlohy v této části funguje tak, že pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cea7f-113">The job in this section works by using Azure PowerShell.</span></span> <span data-ttu-id="cea7f-114">Řadu třídy součástí Mahout nefungují aktuálně s prostředím Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cea7f-114">Many of the classes provided with Mahout do not currently work with Azure PowerShell.</span></span> <span data-ttu-id="cea7f-115">Seznam tříd, které nefungují s prostředím Azure PowerShell najdete v tématu [Poradce při potížích s](#troubleshooting) části.</span><span class="sxs-lookup"><span data-stu-id="cea7f-115">For a list of classes that do not work with Azure PowerShell, see the [Troubleshooting](#troubleshooting) section.</span></span>
>
> <span data-ttu-id="cea7f-116">Příklad použití SSH se připojit k HDInsight a spuštění příklady Mahout přímo na clusteru, naleznete v části [generování doporučení pomocí Mahout a HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).</span><span class="sxs-lookup"><span data-stu-id="cea7f-116">For an example of using SSH to connect to HDInsight and run Mahout examples directly on the cluster, see [Generate movie recommendations using Mahout and HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).</span></span>

<span data-ttu-id="cea7f-117">Jednou z funkcí, které zajišťuje Mahout je modul doporučení.</span><span class="sxs-lookup"><span data-stu-id="cea7f-117">One of the functions that is provided by Mahout is a recommendation engine.</span></span> <span data-ttu-id="cea7f-118">Tento modul přijímá data ve formátu `userID`, `itemId`, a `prefValue` (předvolba uživatele pro položku).</span><span class="sxs-lookup"><span data-stu-id="cea7f-118">This engine accepts data in the format of `userID`, `itemId`, and `prefValue` (the users preference for the item).</span></span> <span data-ttu-id="cea7f-119">Mahout data používá k určení uživatelů s předvolby jako položky, které se dají použít tak, aby doporučení.</span><span class="sxs-lookup"><span data-stu-id="cea7f-119">Mahout uses the data to determine users with like-item preferences, which can be used to make recommendations.</span></span>

<span data-ttu-id="cea7f-120">Následující příklad je zjednodušený návod, jak funguje proces doporučení:</span><span class="sxs-lookup"><span data-stu-id="cea7f-120">The following example is a simplified walk-through of how the recommendation process works:</span></span>

* <span data-ttu-id="cea7f-121">**společné výskyt**: Jan, Alice a Bob všechny líbilo *hvězdičky válek*, *Empire stávky zpět*, a *návrat Jedi*.</span><span class="sxs-lookup"><span data-stu-id="cea7f-121">**co-occurrence**: Joe, Alice, and Bob all liked *Star Wars*, *The Empire Strikes Back*, and *Return of the Jedi*.</span></span> <span data-ttu-id="cea7f-122">Mahout Určuje, že uživatelé, kteří také jako některého z těchto filmy jako další dvě.</span><span class="sxs-lookup"><span data-stu-id="cea7f-122">Mahout determines that users who like any one of these movies also like the other two.</span></span>

* <span data-ttu-id="cea7f-123">**společné výskyt**: Bob a Alice také líbilo *The Menace fiktivní*, *útoku klonů*, a *odvety Sith*.</span><span class="sxs-lookup"><span data-stu-id="cea7f-123">**co-occurrence**: Bob and Alice also liked *The Phantom Menace*, *Attack of the Clones*, and *Revenge of the Sith*.</span></span> <span data-ttu-id="cea7f-124">Mahout Určuje, že uživatelé, kteří líbilo předchozí tři filmy také jako tyto filmy.</span><span class="sxs-lookup"><span data-stu-id="cea7f-124">Mahout determines that users who liked the previous three movies also like these movies.</span></span>

* <span data-ttu-id="cea7f-125">**Podobnosti doporučení**: protože Jan líbilo první tři filmy, Mahout vypadá na filmy ostatní s líbilo podobných předvolby, ale nebyla Jan sledovaná (líbilo nebo hodnocení).</span><span class="sxs-lookup"><span data-stu-id="cea7f-125">**Similarity recommendation**: Because Joe liked the first three movies, Mahout looks at movies that others with similar preferences liked, but Joe has not watched (liked/rated).</span></span> <span data-ttu-id="cea7f-126">V takovém případě se doporučuje Mahout *The Menace fiktivní*, *útoku klonů*, a *odvety Sith*.</span><span class="sxs-lookup"><span data-stu-id="cea7f-126">In this case, Mahout recommends *The Phantom Menace*, *Attack of the Clones*, and *Revenge of the Sith*.</span></span>

### <a name="understanding-the-data"></a><span data-ttu-id="cea7f-127">Pochopení dat</span><span class="sxs-lookup"><span data-stu-id="cea7f-127">Understanding the data</span></span>

<span data-ttu-id="cea7f-128">[GroupLens Research] [ movielens] poskytuje hodnocení data pro filmy ve formátu, který je kompatibilní s Mahout.</span><span class="sxs-lookup"><span data-stu-id="cea7f-128">[GroupLens Research][movielens] provides rating data for movies in a format that is compatible with Mahout.</span></span> <span data-ttu-id="cea7f-129">Tato data jsou k dispozici na výchozí úložiště pro váš cluster v `/HdiSamples//HdiSamples/MahoutMovieData`.</span><span class="sxs-lookup"><span data-stu-id="cea7f-129">This data is available on the default storage for your cluster at `/HdiSamples//HdiSamples/MahoutMovieData`.</span></span>

<span data-ttu-id="cea7f-130">Existují dva soubory, `moviedb.txt` (informace o videa) a `user-ratings.txt`.</span><span class="sxs-lookup"><span data-stu-id="cea7f-130">There are two files, `moviedb.txt` (information about the movies) and `user-ratings.txt`.</span></span> <span data-ttu-id="cea7f-131">`user-ratings.txt` Soubor se používá během analýzy.</span><span class="sxs-lookup"><span data-stu-id="cea7f-131">The `user-ratings.txt` file is used during analysis.</span></span> <span data-ttu-id="cea7f-132">`moviedb.txt` Soubor se používá k zadejte popisný text při zobrazení výsledky analýzy.</span><span class="sxs-lookup"><span data-stu-id="cea7f-132">The `moviedb.txt` file is used to provide user-friendly text when displaying the results of the analysis.</span></span>

<span data-ttu-id="cea7f-133">Data obsažená v ratings.txt uživatel má struktura `userID`, `movieID`, `userRating`, a `timestamp`, který víme, jak vysoce každý uživatel hodnocení filmu.</span><span class="sxs-lookup"><span data-stu-id="cea7f-133">The data contained in user-ratings.txt has a structure of `userID`, `movieID`, `userRating`, and `timestamp`, which tells us how highly each user rated a movie.</span></span> <span data-ttu-id="cea7f-134">Tady je příklad dat:</span><span class="sxs-lookup"><span data-stu-id="cea7f-134">Here is an example of the data:</span></span>

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

### <a name="run-the-job"></a><span data-ttu-id="cea7f-135">Spustit úlohu</span><span class="sxs-lookup"><span data-stu-id="cea7f-135">Run the job</span></span>

<span data-ttu-id="cea7f-136">Spustit úlohu, která používá modul doporučení Mahout s daty film pomocí následujícího skriptu prostředí Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="cea7f-136">Use the following Windows PowerShell script to run a job that uses the Mahout recommendation engine with the movie data:</span></span>

> [!NOTE]
> <span data-ttu-id="cea7f-137">Tento soubor vyzve k zadání informace, které se používá k připojení ke svému clusteru HDInsight a spuštění úloh.</span><span class="sxs-lookup"><span data-stu-id="cea7f-137">This file prompts you for information that is used to connect to your HDInsight cluster and run jobs.</span></span> <span data-ttu-id="cea7f-138">To může trvat několik minut na dokončení a stáhněte si soubor výstup.txt úloh.</span><span class="sxs-lookup"><span data-stu-id="cea7f-138">It may take several minutes for the jobs to complete and download the output.txt file.</span></span>

<span data-ttu-id="cea7f-139">[!code-powershell[hlavní](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=5-98)]</span><span class="sxs-lookup"><span data-stu-id="cea7f-139">[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=5-98)]</span></span>

> [!NOTE]
> <span data-ttu-id="cea7f-140">Mahout úlohy neodebírejte dočasná data, která je vytvořena při zpracování úlohy.</span><span class="sxs-lookup"><span data-stu-id="cea7f-140">Mahout jobs do not remove temporary data that is created while processing the job.</span></span> <span data-ttu-id="cea7f-141">`--tempDir` v úloze příklad izolovat dočasné soubory do konkrétního adresáře je zadán parametr.</span><span class="sxs-lookup"><span data-stu-id="cea7f-141">The `--tempDir` parameter is specified in the example job to isolate the temporary files into a specific directory.</span></span>

<span data-ttu-id="cea7f-142">Úloha Mahout nevrátí výstup STDOUT.</span><span class="sxs-lookup"><span data-stu-id="cea7f-142">The Mahout job does not return the output to STDOUT.</span></span> <span data-ttu-id="cea7f-143">Místo toho je uložený v zadané výstupní adresář jako **část r-00000**.</span><span class="sxs-lookup"><span data-stu-id="cea7f-143">Instead, it stores it in the specified output directory as **part-r-00000**.</span></span> <span data-ttu-id="cea7f-144">Skript stáhne tento soubor do **výstup.txt** v aktuálním adresáři na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="cea7f-144">The script downloads this file to **output.txt** in the current directory on your workstation.</span></span>

<span data-ttu-id="cea7f-145">Tento text je příkladem obsah tohoto souboru:</span><span class="sxs-lookup"><span data-stu-id="cea7f-145">The following text is an example of the content of this file:</span></span>

    1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

<span data-ttu-id="cea7f-146">První sloupec je `userID`.</span><span class="sxs-lookup"><span data-stu-id="cea7f-146">The first column is the `userID`.</span></span> <span data-ttu-id="cea7f-147">Hodnoty obsažené v ' [' a ']' jsou `movieId`:`recommendationScore`.</span><span class="sxs-lookup"><span data-stu-id="cea7f-147">The values contained in '[' and ']' are `movieId`:`recommendationScore`.</span></span>

<span data-ttu-id="cea7f-148">Skript také stáhne `moviedb.txt` a `user-ratings.txt` soubory, které jsou potřebné k formátování výstupu být srozumitelnější.</span><span class="sxs-lookup"><span data-stu-id="cea7f-148">The script also downloads the `moviedb.txt` and `user-ratings.txt` files, which are needed to format the output to be more readable.</span></span>

### <a name="view-the-output"></a><span data-ttu-id="cea7f-149">Zobrazit výstup</span><span class="sxs-lookup"><span data-stu-id="cea7f-149">View the output</span></span>

<span data-ttu-id="cea7f-150">I když generovaný výstup může být OK pro použití v aplikaci, není uživatelsky přívětivý.</span><span class="sxs-lookup"><span data-stu-id="cea7f-150">Although the generated output might be OK for use in an application, it's not user-friendly.</span></span> <span data-ttu-id="cea7f-151">`moviedb.txt` Ze serveru, můžete použít k vyřešení `movieId` film název.</span><span class="sxs-lookup"><span data-stu-id="cea7f-151">The `moviedb.txt` from the server can be used to resolve the `movieId` to a movie name.</span></span> <span data-ttu-id="cea7f-152">Pomocí následujícího skriptu prostředí PowerShell zobrazíte doporučení s názvy film:</span><span class="sxs-lookup"><span data-stu-id="cea7f-152">Use the following PowerShell script to display recommendations with movie names:</span></span>

<span data-ttu-id="cea7f-153">[!code-powershell[hlavní](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=106-180)]</span><span class="sxs-lookup"><span data-stu-id="cea7f-153">[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=106-180)]</span></span>

<span data-ttu-id="cea7f-154">Pokud chcete zobrazit doporučení v uživatelsky přívětivý formátu použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cea7f-154">Use the following command to display the recommendations in a user-friendly format:</span></span> 

```powershell
.\show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt
```

<span data-ttu-id="cea7f-155">Výstup se bude podobat následujícímu:</span><span class="sxs-lookup"><span data-stu-id="cea7f-155">The output is similar to the following text:</span></span>

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
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
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237

## <span data-ttu-id="cea7f-156"><a name="troubleshooting"></a>Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="cea7f-156"><a name="troubleshooting"></a>Troubleshooting</span></span>

### <a name="cannot-overwrite-files"></a><span data-ttu-id="cea7f-157">Nelze přepsat soubory</span><span class="sxs-lookup"><span data-stu-id="cea7f-157">Cannot overwrite files</span></span>

<span data-ttu-id="cea7f-158">Mahout úlohy se vyčistit dočasné soubory, které byly vytvořeny během zpracování.</span><span class="sxs-lookup"><span data-stu-id="cea7f-158">Mahout jobs do not clean up temporary files that were created during processing.</span></span> <span data-ttu-id="cea7f-159">Kromě toho úlohy Nepřepisovat existující soubor výstupu.</span><span class="sxs-lookup"><span data-stu-id="cea7f-159">In addition, the jobs do not overwrite existing output file.</span></span>

<span data-ttu-id="cea7f-160">Aby nedocházelo k chybám při spuštění úlohy Mahout, odstraňte dočasné a výstupní soubory mezi spustí.</span><span class="sxs-lookup"><span data-stu-id="cea7f-160">To avoid errors when running Mahout jobs, delete temporary and output files between runs.</span></span> <span data-ttu-id="cea7f-161">Chcete-li odebrat soubory vytvořené pomocí dřívějších skripty v tomto dokumentu, použijte následující skript prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="cea7f-161">To remove the files created by the earlier scripts in this document, use the following PowerShell script:</span></span>

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -Message "Enter the login for the cluster"

#Get the cluster info so we can get the resource group, storage, etc.
$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
$container = $clusterInfo.DefaultStorageContainer
$storageAccountKey = (Get-AzureRmStorageAccountKey `
    -Name $storageAccountName `
-ResourceGroupName $resourceGroup)[0].Value

#Create a storage context and upload the file
$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

#Azure PowerShell can't delete blobs using wildcard,
#so have to get a list and delete one at a time
# Start with the output
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/out"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
# Next the temp files
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/temp"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
```

### <span data-ttu-id="cea7f-162"><a name="nopowershell"></a>Třídy, které nefungují s prostředím Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="cea7f-162"><a name="nopowershell"></a>Classes that do not work with Azure PowerShell</span></span>

<span data-ttu-id="cea7f-163">Mahout úlohy, které používají následující třídy vrátit různé chybové zprávy při použití v prostředí Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="cea7f-163">Mahout jobs that use the following classes return various error messages when used from Windows PowerShell:</span></span>

* <span data-ttu-id="cea7f-164">org.apache.mahout.utils.clustering.ClusterDumper</span><span class="sxs-lookup"><span data-stu-id="cea7f-164">org.apache.mahout.utils.clustering.ClusterDumper</span></span>
* <span data-ttu-id="cea7f-165">org.apache.mahout.utils.SequenceFileDumper</span><span class="sxs-lookup"><span data-stu-id="cea7f-165">org.apache.mahout.utils.SequenceFileDumper</span></span>
* <span data-ttu-id="cea7f-166">org.apache.mahout.utils.vectors.lucene.Driver</span><span class="sxs-lookup"><span data-stu-id="cea7f-166">org.apache.mahout.utils.vectors.lucene.Driver</span></span>
* <span data-ttu-id="cea7f-167">org.apache.mahout.utils.vectors.arff.Driver</span><span class="sxs-lookup"><span data-stu-id="cea7f-167">org.apache.mahout.utils.vectors.arff.Driver</span></span>
* <span data-ttu-id="cea7f-168">org.apache.mahout.text.WikipediaToSequenceFile</span><span class="sxs-lookup"><span data-stu-id="cea7f-168">org.apache.mahout.text.WikipediaToSequenceFile</span></span>
* <span data-ttu-id="cea7f-169">org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles</span><span class="sxs-lookup"><span data-stu-id="cea7f-169">org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles</span></span>
* <span data-ttu-id="cea7f-170">org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer</span><span class="sxs-lookup"><span data-stu-id="cea7f-170">org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer</span></span>
* <span data-ttu-id="cea7f-171">org.apache.mahout.classifier.sgd.TrainLogistic</span><span class="sxs-lookup"><span data-stu-id="cea7f-171">org.apache.mahout.classifier.sgd.TrainLogistic</span></span>
* <span data-ttu-id="cea7f-172">org.apache.mahout.classifier.sgd.RunLogistic</span><span class="sxs-lookup"><span data-stu-id="cea7f-172">org.apache.mahout.classifier.sgd.RunLogistic</span></span>
* <span data-ttu-id="cea7f-173">org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic</span><span class="sxs-lookup"><span data-stu-id="cea7f-173">org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic</span></span>
* <span data-ttu-id="cea7f-174">org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic</span><span class="sxs-lookup"><span data-stu-id="cea7f-174">org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic</span></span>
* <span data-ttu-id="cea7f-175">org.apache.mahout.classifier.sgd.RunAdaptiveLogistic</span><span class="sxs-lookup"><span data-stu-id="cea7f-175">org.apache.mahout.classifier.sgd.RunAdaptiveLogistic</span></span>
* <span data-ttu-id="cea7f-176">org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer</span><span class="sxs-lookup"><span data-stu-id="cea7f-176">org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer</span></span>
* <span data-ttu-id="cea7f-177">org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator</span><span class="sxs-lookup"><span data-stu-id="cea7f-177">org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator</span></span>
* <span data-ttu-id="cea7f-178">org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator</span><span class="sxs-lookup"><span data-stu-id="cea7f-178">org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator</span></span>
* <span data-ttu-id="cea7f-179">org.apache.mahout.classifier.df.tools.Describe</span><span class="sxs-lookup"><span data-stu-id="cea7f-179">org.apache.mahout.classifier.df.tools.Describe</span></span>

<span data-ttu-id="cea7f-180">Ke spuštění úloh, které používají tyto třídy, připojte se ke clusteru HDInsight pomocí protokolu SSH a spuštění úloh z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="cea7f-180">To run jobs that use these classes, connect to the HDInsight cluster using SSH and run the jobs from the command line.</span></span> <span data-ttu-id="cea7f-181">Příklad použití SSH ke spuštění úloh Mahout, naleznete v části [generování doporučení pomocí Mahout a HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).</span><span class="sxs-lookup"><span data-stu-id="cea7f-181">For an example of using SSH to run Mahout jobs, see [Generate movie recommendations using Mahout and HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cea7f-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cea7f-182">Next steps</span></span>

<span data-ttu-id="cea7f-183">Teď, když jste se naučili použití Mahout, zjišťovat další způsoby, jak pracovat s daty v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="cea7f-183">Now that you have learned how to use Mahout, discover other ways of working with data on HDInsight:</span></span>

* [<span data-ttu-id="cea7f-184">Hive s HDInsight</span><span class="sxs-lookup"><span data-stu-id="cea7f-184">Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="cea7f-185">Pig s HDInsight</span><span class="sxs-lookup"><span data-stu-id="cea7f-185">Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="cea7f-186">MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="cea7f-186">MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

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
