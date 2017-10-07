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
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight-powershell"></a>Generování doporučení pomocí Apache Mahout s Hadoop v HDInsight (PowerShell)

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Zjistěte, jak toouse hello [Apache Mahout](http://mahout.apache.org) knihovny machine learning s Azure HDInsight toogenerate doporučení. Příklad Hello v tomto dokumentu používá prostředí Azure PowerShell toorun Mahout úlohy.

## <a name="prerequisites"></a>Požadavky

* Cluster HDInsight se systémem Linux. Informace o vytváření jeden najdete v tématu [začít používat systémem Linux Hadoop v HDInsight][getstarted].

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [Azure PowerShell](/powershell/azure/overview)

## <a name="recommendations"></a>Generování doporučení pomocí prostředí Azure PowerShell

> [!WARNING]
> Hello úlohy v této části funguje tak, že pomocí Azure PowerShell. Řadu hello třídy součástí Mahout nefungují aktuálně s prostředím Azure PowerShell. Seznam tříd, které nefungují s prostředím Azure PowerShell najdete v tématu hello [Poradce při potížích s](#troubleshooting) části.
>
> Příklad použití SSH tooconnect tooHDInsight a spuštění příkladů Mahout přímo na clusteru hello, naleznete v části [generování doporučení pomocí Mahout a HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).

Jedním z hello funkce, které zajišťuje Mahout je modul doporučení. Tento modul přijímá data ve formátu hello `userID`, `itemId`, a `prefValue` (hello předvoleb uživatele pro položku hello). Mahout používá hello data toodetermine uživatelé s jako položky předvoleb, které můžou být použité toomake doporučení.

Hello následující příklad je zjednodušený návod, jak funguje hello doporučení proces:

* **společné výskyt**: Jan, Alice a Bob všechny líbilo *hvězdičky válek*, *hello Empire stávky zpět*, a *návrat hello Jedi*. Mahout Určuje, že uživatelé, kteří jako některého z těchto filmy chtěl také hello další dvě.

* **společné výskyt**: Bob a Alice také líbilo *hello fiktivní Menace*, *útoku hello klonů*, a *odvety hello Sith*. Mahout Určuje, že uživatelé, kteří líbilo hello předchozí tři filmy také jako tyto filmy.

* **Podobnosti doporučení**: protože Jan líbilo hello první tři filmy, Mahout vypadá na filmy ostatní s líbilo podobných předvolby, ale nebyla Jan sledovaná (líbilo nebo hodnocení). V takovém případě se doporučuje Mahout *hello fiktivní Menace*, *útoku hello klonů*, a *odvety hello Sith*.

### <a name="understanding-hello-data"></a>Principy hello dat

[GroupLens Research] [ movielens] poskytuje hodnocení data pro filmy ve formátu, který je kompatibilní s Mahout. Tato data jsou k dispozici na hello výchozí úložiště pro váš cluster v `/HdiSamples//HdiSamples/MahoutMovieData`.

Existují dva soubory, `moviedb.txt` (informace o filmy hello) a `user-ratings.txt`. Hello `user-ratings.txt` soubor se používá během analýzy. Hello `moviedb.txt` souboru je použité tooprovide popisný text při zobrazení hello výsledky analýzy hello.

Hello data obsažená v ratings.txt uživatel má struktura `userID`, `movieID`, `userRating`, a `timestamp`, který víme, jak vysoce každý uživatel hodnocení filmu. Tady je příklad hello dat:

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

### <a name="run-hello-job"></a>Spustit úlohu hello

Použijte následující toorun skriptu prostředí Windows PowerShell úlohu, která používá modul doporučení Mahout hello s daty film hello hello:

> [!NOTE]
> Tento soubor vyzve k zadání informací, které jsou používané tooconnect cluster HDInsight tooyour a spuštění úlohy. Může trvat několik minut, než toocomplete hello úlohy a stáhněte soubor výstup.txt hello.

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=5-98)]

> [!NOTE]
> Mahout úlohy neodebírejte dočasná data, který se vytvoří při zpracování úlohy hello. Hello `--tempDir` je zadán parametr v hello příklad úlohy tooisolate hello dočasné soubory do konkrétního adresáře.

Úloha Mahout Hello nevrací tooSTDOUT výstup hello. Místo toho je uložený v zadané výstupní adresář hello jako **část r-00000**. skript Hello soubory ke stažení tohoto souboru příliš**výstup.txt** hello aktuálního adresáře na pracovní stanici.

Hello následující text je příkladem hello obsah tohoto souboru:

    1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

první sloupec Hello je hello `userID`. Hello hodnoty obsažené v ' [' a ']' jsou `movieId`:`recommendationScore`.

skript Hello také stáhne hello `moviedb.txt` a `user-ratings.txt` soubory, které jsou potřebné tooformat hello výstup toobe srozumitelnější.

### <a name="view-hello-output"></a>Výstup hello zobrazení

I když hello generovaný výstup může být OK pro použití v aplikaci, není uživatelsky přívětivý. Hello `moviedb.txt` z hello může být server hello použité tooresolve `movieId` tooa film název. Použijte následující doporučení toodisplay skript prostředí PowerShell s názvy film hello:

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=106-180)]

Použijte následující příkaz toodisplay hello doporučení v uživatelsky přívětivý formátu hello: 

```powershell
.\show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt
```

Hello výstup je podobné toohello následující text:

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

## <a name="troubleshooting"></a>Řešení potíží

### <a name="cannot-overwrite-files"></a>Nelze přepsat soubory

Mahout úlohy se vyčistit dočasné soubory, které byly vytvořeny během zpracování. Kromě toho hello úlohy Nepřepisovat existující výstupní soubor.

tooavoid chyby při spuštění úlohy Mahout se odstranit dočasné a výstupní soubory mezi spustí. vytvořené soubory hello tooremove hello starší skripty v tomto dokumentu pomocí hello následující skript prostředí PowerShell:

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

### <a name="nopowershell"></a>Třídy, které nefungují s prostředím Azure PowerShell

Mahout úlohy, které používají následující třídy hello vrátit různé chybové zprávy při použití v prostředí Windows PowerShell:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

toorun úlohy, které používají tyto třídy připojit toohello clusteru HDInsight pomocí SSH a spouštění úloh hello z příkazového řádku hello. Příklad použití SSH toorun Mahout úloh naleznete v části [generování doporučení pomocí Mahout a HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili, jak toouse Mahout, zjišťovat další způsoby, jak práci s daty v HDInsight:

* [Hive s HDInsight](hdinsight-use-hive.md)
* [Pig s HDInsight](hdinsight-use-pig.md)
* [MapReduce s HDInsight](hdinsight-use-mapreduce.md)

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
