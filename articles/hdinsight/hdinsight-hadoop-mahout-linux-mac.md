---
title: "Generování doporučení pomocí Mahout a HDInsight (SSH) - Azure | Microsoft Docs"
description: "Další informace o použití Apache Mahout strojového učení knihovny pro generování doporučení s HDInsight (Hadoop)."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c78ec37c-9a8c-4bb6-9e38-0bdb9e89fbd7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: 28450d72f19a5467d88bc787d11f6c37c5afbf9a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight-ssh"></a><span data-ttu-id="ab04a-103">Generování doporučení pomocí Apache Mahout se systémem Linux Hadoop v HDInsight (SSH)</span><span class="sxs-lookup"><span data-stu-id="ab04a-103">Generate movie recommendations by using Apache Mahout with Linux-based Hadoop in HDInsight (SSH)</span></span>

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

<span data-ttu-id="ab04a-104">Další informace o použití [Apache Mahout](http://mahout.apache.org) knihovny machine learning s Azure HDInsight ke generování doporučení.</span><span class="sxs-lookup"><span data-stu-id="ab04a-104">Learn how to use the [Apache Mahout](http://mahout.apache.org) machine learning library with Azure HDInsight to generate movie recommendations.</span></span>

<span data-ttu-id="ab04a-105">Je mahout [strojového učení] [ ml] knihovny pro Apache Hadoop.</span><span class="sxs-lookup"><span data-stu-id="ab04a-105">Mahout is a [machine learning][ml] library for Apache Hadoop.</span></span> <span data-ttu-id="ab04a-106">Mahout obsahuje algoritmy pro zpracování dat, jako je například filtrování, klasifikace a clustering.</span><span class="sxs-lookup"><span data-stu-id="ab04a-106">Mahout contains algorithms for processing data, such as filtering, classification, and clustering.</span></span> <span data-ttu-id="ab04a-107">V tomto článku použijete modul doporučení k vygenerování film doporučení, které jsou založeny na filmy, které jste viděli vašich přátel.</span><span class="sxs-lookup"><span data-stu-id="ab04a-107">In this article, you use a recommendation engine to generate movie recommendations that are based on movies your friends have seen.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab04a-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ab04a-108">Prerequisites</span></span>

* <span data-ttu-id="ab04a-109">Cluster HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="ab04a-109">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="ab04a-110">Informace o vytváření jeden najdete v tématu [začít používat systémem Linux Hadoop v HDInsight][getstarted].</span><span class="sxs-lookup"><span data-stu-id="ab04a-110">For information about creating one, see [Get started using Linux-based Hadoop in HDInsight][getstarted].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ab04a-111">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="ab04a-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ab04a-112">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ab04a-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="ab04a-113">Klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="ab04a-113">An SSH client.</span></span> <span data-ttu-id="ab04a-114">Další informace najdete v dokumentu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ab04a-114">For more information, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

## <a name="mahout-versioning"></a><span data-ttu-id="ab04a-115">Správa verzí mahout</span><span class="sxs-lookup"><span data-stu-id="ab04a-115">Mahout versioning</span></span>

<span data-ttu-id="ab04a-116">Další informace o verzi nástroje Mahout v prostředí HDInsight najdete v tématu [HDInsight verze a komponent systému Hadoop](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="ab04a-116">For more information about the version of Mahout in HDInsight, see [HDInsight versions and Hadoop components](hdinsight-component-versioning.md).</span></span>

## <span data-ttu-id="ab04a-117"><a name="recommendations"></a>Vysvětlení doporučení</span><span class="sxs-lookup"><span data-stu-id="ab04a-117"><a name="recommendations"></a>Understanding recommendations</span></span>

<span data-ttu-id="ab04a-118">Jednou z funkcí, které zajišťuje Mahout je modul doporučení.</span><span class="sxs-lookup"><span data-stu-id="ab04a-118">One of the functions that is provided by Mahout is a recommendation engine.</span></span> <span data-ttu-id="ab04a-119">Tento modul přijímá data ve formátu `userID`, `itemId`, a `prefValue` (předvoleb pro položku).</span><span class="sxs-lookup"><span data-stu-id="ab04a-119">This engine accepts data in the format of `userID`, `itemId`, and `prefValue` (the preference for the item).</span></span> <span data-ttu-id="ab04a-120">Mahout pak můžete provádět analýzy společné occurance k určení: *uživatelé, kteří mají předvolbu pro položku také mít předvolbu pro tyto další položky*.</span><span class="sxs-lookup"><span data-stu-id="ab04a-120">Mahout can then perform co-occurance analysis to determine: *users who have a preference for an item also have a preference for these other items*.</span></span> <span data-ttu-id="ab04a-121">Mahout pak určuje uživatelé s předvolby jako položky, které se dají použít tak, aby doporučení.</span><span class="sxs-lookup"><span data-stu-id="ab04a-121">Mahout then determines users with like-item preferences, which can be used to make recommendations.</span></span>

<span data-ttu-id="ab04a-122">Následující pracovní postup je zjednodušená příklad, který používá film data:</span><span class="sxs-lookup"><span data-stu-id="ab04a-122">The following workflow is a simplified example that uses movie data:</span></span>

* <span data-ttu-id="ab04a-123">**Společné occurance**: Jan, Alice a Bob všechny líbilo *hvězdičky válek*, *Empire stávky zpět*, a *návrat Jedi*.</span><span class="sxs-lookup"><span data-stu-id="ab04a-123">**Co-occurance**: Joe, Alice, and Bob all liked *Star Wars*, *The Empire Strikes Back*, and *Return of the Jedi*.</span></span> <span data-ttu-id="ab04a-124">Mahout Určuje, že uživatelé, kteří také jako některého z těchto filmy jako další dvě.</span><span class="sxs-lookup"><span data-stu-id="ab04a-124">Mahout determines that users who like any one of these movies also like the other two.</span></span>

* <span data-ttu-id="ab04a-125">**Společné occurance**: Bob a Alice také líbilo *The Menace fiktivní*, *útoku klonů*, a *odvety Sith*.</span><span class="sxs-lookup"><span data-stu-id="ab04a-125">**Co-occurance**: Bob and Alice also liked *The Phantom Menace*, *Attack of the Clones*, and *Revenge of the Sith*.</span></span> <span data-ttu-id="ab04a-126">Mahout Určuje, že uživatelé, kteří líbilo předchozí tři filmy také jako tyto tři filmy.</span><span class="sxs-lookup"><span data-stu-id="ab04a-126">Mahout determines that users who liked the previous three movies also like these three movies.</span></span>

* <span data-ttu-id="ab04a-127">**Podobnosti doporučení**: protože Jan líbilo první tři filmy, Mahout vypadá na filmy ostatní s líbilo podobných předvolby, ale nebyla Jan sledovaná (líbilo nebo hodnocení).</span><span class="sxs-lookup"><span data-stu-id="ab04a-127">**Similarity recommendation**: Because Joe liked the first three movies, Mahout looks at movies that others with similar preferences liked, but Joe has not watched (liked/rated).</span></span> <span data-ttu-id="ab04a-128">V takovém případě se doporučuje Mahout *The Menace fiktivní*, *útoku klonů*, a *odvety Sith*.</span><span class="sxs-lookup"><span data-stu-id="ab04a-128">In this case, Mahout recommends *The Phantom Menace*, *Attack of the Clones*, and *Revenge of the Sith*.</span></span>

### <a name="understanding-the-data"></a><span data-ttu-id="ab04a-129">Pochopení dat</span><span class="sxs-lookup"><span data-stu-id="ab04a-129">Understanding the data</span></span>

<span data-ttu-id="ab04a-130">Pohodlně [GroupLens Research] [ movielens] poskytuje hodnocení data pro filmy ve formátu, který je kompatibilní s Mahout.</span><span class="sxs-lookup"><span data-stu-id="ab04a-130">Conveniently, [GroupLens Research][movielens] provides rating data for movies in a format that is compatible with Mahout.</span></span> <span data-ttu-id="ab04a-131">Tato data jsou k dispozici na váš cluster výchozí úložiště na `/HdiSamples/HdiSamples/MahoutMovieData`.</span><span class="sxs-lookup"><span data-stu-id="ab04a-131">This data is available on your cluster's default storage at `/HdiSamples/HdiSamples/MahoutMovieData`.</span></span>

<span data-ttu-id="ab04a-132">Existují dva soubory, `moviedb.txt` a `user-ratings.txt`.</span><span class="sxs-lookup"><span data-stu-id="ab04a-132">There are two files, `moviedb.txt` and `user-ratings.txt`.</span></span> <span data-ttu-id="ab04a-133">Soubor ratings.txt uživatele se používá během analýzy, zatímco moviedb.txt slouží k zadání informací popisný text při zobrazení výsledky analýzy.</span><span class="sxs-lookup"><span data-stu-id="ab04a-133">The user-ratings.txt file is used during analysis, while moviedb.txt is used to provide user-friendly text information when displaying the results of the analysis.</span></span>

<span data-ttu-id="ab04a-134">Data obsažená v ratings.txt uživatel má struktura `userID`, `movieID`, `userRating`, a `timestamp`, který víme, jak vysoce každý uživatel hodnocení filmu.</span><span class="sxs-lookup"><span data-stu-id="ab04a-134">The data contained in user-ratings.txt has a structure of `userID`, `movieID`, `userRating`, and `timestamp`, which tells us how highly each user rated a movie.</span></span> <span data-ttu-id="ab04a-135">Tady je příklad dat:</span><span class="sxs-lookup"><span data-stu-id="ab04a-135">Here is an example of the data:</span></span>

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

## <a name="run-the-analysis"></a><span data-ttu-id="ab04a-136">Spuštění analýzy</span><span class="sxs-lookup"><span data-stu-id="ab04a-136">Run the analysis</span></span>

<span data-ttu-id="ab04a-137">Z připojení SSH do clusteru použijte následující příkaz pro spuštění úlohy doporučení:</span><span class="sxs-lookup"><span data-stu-id="ab04a-137">From an SSH connection to the cluster, use the following command to run the recommendation job:</span></span>

```bash
mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp
```

> [!NOTE]
> <span data-ttu-id="ab04a-138">Úloha může trvat několik minut na dokončení a může spustit víc úloh MapReduce.</span><span class="sxs-lookup"><span data-stu-id="ab04a-138">The job may take several minutes to complete, and may run multiple MapReduce jobs.</span></span>

## <a name="view-the-output"></a><span data-ttu-id="ab04a-139">Zobrazit výstup</span><span class="sxs-lookup"><span data-stu-id="ab04a-139">View the output</span></span>

1. <span data-ttu-id="ab04a-140">Po dokončení úlohy pro zobrazení generovaný výstup použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ab04a-140">Once the job completes, use the following command to view the generated output:</span></span>

    ```bash
    hdfs dfs -text /example/data/mahoutout/part-r-00000
    ```

    <span data-ttu-id="ab04a-141">Výstup vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="ab04a-141">The output appears as follows:</span></span>

        1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    <span data-ttu-id="ab04a-142">První sloupec je `userID`.</span><span class="sxs-lookup"><span data-stu-id="ab04a-142">The first column is the `userID`.</span></span> <span data-ttu-id="ab04a-143">Hodnoty obsažené v ' [' a ']' jsou `movieId`:`recommendationScore`.</span><span class="sxs-lookup"><span data-stu-id="ab04a-143">The values contained in '[' and ']' are `movieId`:`recommendationScore`.</span></span>

2. <span data-ttu-id="ab04a-144">Můžete použít výstup, společně s moviedb.txt, poskytovat další informace o doporučení.</span><span class="sxs-lookup"><span data-stu-id="ab04a-144">You can use the output, along with the moviedb.txt, to provide more information on the recommendations.</span></span> <span data-ttu-id="ab04a-145">Nejdřív je potřeba kopírovat soubory místně pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="ab04a-145">First, we need to copy the files locally using the following commands:</span></span>

    ```bash
    hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
    hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .
    ```

    <span data-ttu-id="ab04a-146">Tento příkaz zkopíruje výstupní data do souboru s názvem **recommendations.txt** v aktuálním adresáři, společně s film datových souborů.</span><span class="sxs-lookup"><span data-stu-id="ab04a-146">This command copies the output data to a file named **recommendations.txt** in the current directory, along with the movie data files.</span></span>

3. <span data-ttu-id="ab04a-147">Pomocí následujícího příkazu vytvořte skript jazyka Python, který vyhledá názvy film pro data ve výstupu doporučení:</span><span class="sxs-lookup"><span data-stu-id="ab04a-147">Use the following command to create a Python script that looks up movie names for the data in the recommendations output:</span></span>

    ```bash
    nano show_recommendations.py
    ```

    <span data-ttu-id="ab04a-148">Když se otevře editor, použijte jako obsah souboru následující text:</span><span class="sxs-lookup"><span data-stu-id="ab04a-148">When the editor opens, use the following text as the contents of the file:</span></span>

   ```python
   #!/usr/bin/env python

   import sys

   if len(sys.argv) != 5:
        print "Arguments: userId userDataFilename movieFilename recommendationFilename"
        sys.exit(1)

   userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

   print "Reading Movies Descriptions"
   movieFile = open(movieFilename)
   movieById = {}
   for line in movieFile:
       tokens = line.split("|")
       movieById[tokens[0]] = tokens[1:]
   movieFile.close()

   print "Reading Rated Movies"
   userDataFile = open(userDataFilename)
   ratedMovieIds = []
   for line in userDataFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           ratedMovieIds.append((tokens[1],tokens[2]))
   userDataFile.close()

   print "Reading Recommendations"
   recommendationFile = open(recommendationFilename)
   recommendations = []
   for line in recommendationFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           movieIdAndScores = tokens[1].strip("[]\n").split(",")
           recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
           break
   recommendationFile.close()

   print "Rated Movies"
   print "------------------------"
   for movieId, rating in ratedMovieIds:
       print "%s, rating=%s" % (movieById[movieId][0], rating)
   print "------------------------"

   print "Recommended Movies"
   print "------------------------"
   for movieId, score in recommendations:
       print "%s, score=%s" % (movieById[movieId][0], score)
   print "------------------------"
   ```

    <span data-ttu-id="ab04a-149">Stiskněte klávesu **Ctrl-X**, **Y**a v neposlední řadě **Enter** uložit data.</span><span class="sxs-lookup"><span data-stu-id="ab04a-149">Press **Ctrl-X**, **Y**, and finally **Enter** to save the data.</span></span>

4. <span data-ttu-id="ab04a-150">Spusťte skript Python.</span><span class="sxs-lookup"><span data-stu-id="ab04a-150">Run the Python script.</span></span> <span data-ttu-id="ab04a-151">Tento příkaz předpokládá, že jsou v adresáři, kde byly staženy všechny soubory:</span><span class="sxs-lookup"><span data-stu-id="ab04a-151">The following command assumes you are in the directory where all the files were downloaded:</span></span>

    ```bash
    python show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt
    ```

    <span data-ttu-id="ab04a-152">Tento příkaz zjistí doporučení vygenerované uživatelské ID 4.</span><span class="sxs-lookup"><span data-stu-id="ab04a-152">This command looks at the recommendations generated for user ID 4.</span></span>

    * <span data-ttu-id="ab04a-153">**Uživatele ratings.txt** souboru se používá k načtení filmy, které nemají hodnocení.</span><span class="sxs-lookup"><span data-stu-id="ab04a-153">The **user-ratings.txt** file is used to retrieve movies that have been rated.</span></span>

    * <span data-ttu-id="ab04a-154">**Moviedb.txt** souboru se používá k načtení názvy videa.</span><span class="sxs-lookup"><span data-stu-id="ab04a-154">The **moviedb.txt** file is used to retrieve the names of the movies.</span></span>

    * <span data-ttu-id="ab04a-155">**Recommendations.txt** se používá k načtení film doporučení pro tohoto uživatele.</span><span class="sxs-lookup"><span data-stu-id="ab04a-155">The **recommendations.txt** is used to retrieve the movie recommendations for this user.</span></span>

     <span data-ttu-id="ab04a-156">Výstup tohoto příkazu je podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="ab04a-156">The output from this command is similar to the following text:</span></span>

        <span data-ttu-id="ab04a-157">Sedm let v Tibet (1997), stanovení skóre = 5.0 Indiana Petr a poslední Crusade (1989), stanovení skóre = 5.0 Jaws (1975) skóre = 5.0 smysl a citlivosti (1995), skóre = 5.0 nezávislost den (ID4) (1996), skóre = 5.0 Moje nejlepší přítel svatbě (1997), stanovení skóre = 5.0 Jerry Maguire (1996) skóre = 5.0 Scream 2 (1997) skóre = 5.0 dobu Kill, (1996), stanovení skóre = 5.0</span><span class="sxs-lookup"><span data-stu-id="ab04a-157">Seven Years in Tibet (1997), score=5.0   Indiana Jones and the Last Crusade (1989), score=5.0   Jaws (1975), score=5.0   Sense and Sensibility (1995), score=5.0   Independence Day (ID4) (1996), score=5.0   My Best Friend's Wedding (1997), score=5.0   Jerry Maguire (1996), score=5.0   Scream 2 (1997), score=5.0   Time to Kill, A (1996), score=5.0</span></span>

## <a name="delete-temporary-data"></a><span data-ttu-id="ab04a-158">Odstranit dočasná data</span><span class="sxs-lookup"><span data-stu-id="ab04a-158">Delete temporary data</span></span>

<span data-ttu-id="ab04a-159">Mahout úlohy neodebírejte dočasná data, která je vytvořena při zpracování úlohy.</span><span class="sxs-lookup"><span data-stu-id="ab04a-159">Mahout jobs do not remove temporary data that is created while processing the job.</span></span> <span data-ttu-id="ab04a-160">`--tempDir` v úloze příklad izolovat dočasných souborů do určité cesty pro snadné odstranění je zadán parametr.</span><span class="sxs-lookup"><span data-stu-id="ab04a-160">The `--tempDir` parameter is specified in the example job to isolate the temporary files into a specific path for easy deletion.</span></span> <span data-ttu-id="ab04a-161">Chcete-li odebrat dočasné soubory, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ab04a-161">To remove the temp files, use the following command:</span></span>

```bash
hdfs dfs -rm -f -r /temp/mahouttemp
```

> [!WARNING]
> <span data-ttu-id="ab04a-162">Pokud chcete znovu spustit příkaz, musíte také odstranit výstupnímu adresáři.</span><span class="sxs-lookup"><span data-stu-id="ab04a-162">If you want to run the command again, you must also delete the output directory.</span></span> <span data-ttu-id="ab04a-163">Chcete-li odstranit tento adresář použijte následující:</span><span class="sxs-lookup"><span data-stu-id="ab04a-163">Use the following to delete this directory:</span></span>
>
> `hdfs dfs -rm -f -r /example/data/mahoutout`


## <a name="next-steps"></a><span data-ttu-id="ab04a-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ab04a-164">Next steps</span></span>

<span data-ttu-id="ab04a-165">Teď, když jste se naučili použití Mahout, zjišťovat další způsoby, jak pracovat s daty v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="ab04a-165">Now that you have learned how to use Mahout, discover other ways of working with data on HDInsight:</span></span>

* [<span data-ttu-id="ab04a-166">Hive s HDInsight</span><span class="sxs-lookup"><span data-stu-id="ab04a-166">Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="ab04a-167">Pig s HDInsight</span><span class="sxs-lookup"><span data-stu-id="ab04a-167">Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="ab04a-168">MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="ab04a-168">MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
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
