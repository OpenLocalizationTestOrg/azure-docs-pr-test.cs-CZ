---
title: "doporučení aaaGenerate pomocí Mahout a HDInsight (SSH) - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello Apache Mahout strojového učení knihovny toogenerate doporučení s HDInsight (Hadoop)."
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
ms.openlocfilehash: fedac9ceb4268f8421bce4623a5ad271041b8b3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight-ssh"></a><span data-ttu-id="eb4ee-103">Generování doporučení pomocí Apache Mahout se systémem Linux Hadoop v HDInsight (SSH)</span><span class="sxs-lookup"><span data-stu-id="eb4ee-103">Generate movie recommendations by using Apache Mahout with Linux-based Hadoop in HDInsight (SSH)</span></span>

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

<span data-ttu-id="eb4ee-104">Zjistěte, jak toouse hello [Apache Mahout](http://mahout.apache.org) knihovny machine learning s Azure HDInsight toogenerate doporučení.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-104">Learn how toouse hello [Apache Mahout](http://mahout.apache.org) machine learning library with Azure HDInsight toogenerate movie recommendations.</span></span>

<span data-ttu-id="eb4ee-105">Je mahout [strojového učení] [ ml] knihovny pro Apache Hadoop.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-105">Mahout is a [machine learning][ml] library for Apache Hadoop.</span></span> <span data-ttu-id="eb4ee-106">Mahout obsahuje algoritmy pro zpracování dat, jako je například filtrování, klasifikace a clustering.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-106">Mahout contains algorithms for processing data, such as filtering, classification, and clustering.</span></span> <span data-ttu-id="eb4ee-107">V tomto článku použijete doporučení film toogenerate modul doporučení, které jsou založeny na filmy, které jste viděli vašich přátel.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-107">In this article, you use a recommendation engine toogenerate movie recommendations that are based on movies your friends have seen.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eb4ee-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="eb4ee-108">Prerequisites</span></span>

* <span data-ttu-id="eb4ee-109">Cluster HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-109">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="eb4ee-110">Informace o vytváření jeden najdete v tématu [začít používat systémem Linux Hadoop v HDInsight][getstarted].</span><span class="sxs-lookup"><span data-stu-id="eb4ee-110">For information about creating one, see [Get started using Linux-based Hadoop in HDInsight][getstarted].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="eb4ee-111">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="eb4ee-112">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="eb4ee-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="eb4ee-113">Klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-113">An SSH client.</span></span> <span data-ttu-id="eb4ee-114">Další informace najdete v tématu hello [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-114">For more information, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

## <a name="mahout-versioning"></a><span data-ttu-id="eb4ee-115">Správa verzí mahout</span><span class="sxs-lookup"><span data-stu-id="eb4ee-115">Mahout versioning</span></span>

<span data-ttu-id="eb4ee-116">Další informace o verzi hello Mahout v prostředí HDInsight najdete v tématu [HDInsight verze a komponent systému Hadoop](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="eb4ee-116">For more information about hello version of Mahout in HDInsight, see [HDInsight versions and Hadoop components](hdinsight-component-versioning.md).</span></span>

## <span data-ttu-id="eb4ee-117"><a name="recommendations"></a>Vysvětlení doporučení</span><span class="sxs-lookup"><span data-stu-id="eb4ee-117"><a name="recommendations"></a>Understanding recommendations</span></span>

<span data-ttu-id="eb4ee-118">Jedním z hello funkce, které zajišťuje Mahout je modul doporučení.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-118">One of hello functions that is provided by Mahout is a recommendation engine.</span></span> <span data-ttu-id="eb4ee-119">Tento modul přijímá data ve formátu hello `userID`, `itemId`, a `prefValue` (hello předvoleb pro položku hello).</span><span class="sxs-lookup"><span data-stu-id="eb4ee-119">This engine accepts data in hello format of `userID`, `itemId`, and `prefValue` (hello preference for hello item).</span></span> <span data-ttu-id="eb4ee-120">Mahout pak můžete provádět analýzy toodetermine společné occurance: *uživatelé, kteří mají předvolbu pro položku také mít předvolbu pro tyto další položky*.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-120">Mahout can then perform co-occurance analysis toodetermine: *users who have a preference for an item also have a preference for these other items*.</span></span> <span data-ttu-id="eb4ee-121">Mahout pak určuje uživatelé s jako položky předvoleb, které můžou být použité toomake doporučení.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-121">Mahout then determines users with like-item preferences, which can be used toomake recommendations.</span></span>

<span data-ttu-id="eb4ee-122">Hello následující pracovní postup je zjednodušená příklad, který používá film data:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-122">hello following workflow is a simplified example that uses movie data:</span></span>

* <span data-ttu-id="eb4ee-123">**Společné occurance**: Jan, Alice a Bob všechny líbilo *hvězdičky válek*, *hello Empire stávky zpět*, a *návrat hello Jedi*.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-123">**Co-occurance**: Joe, Alice, and Bob all liked *Star Wars*, *hello Empire Strikes Back*, and *Return of hello Jedi*.</span></span> <span data-ttu-id="eb4ee-124">Mahout Určuje, že uživatelé, kteří jako některého z těchto filmy chtěl také hello další dvě.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-124">Mahout determines that users who like any one of these movies also like hello other two.</span></span>

* <span data-ttu-id="eb4ee-125">**Společné occurance**: Bob a Alice také líbilo *hello fiktivní Menace*, *útoku hello klonů*, a *odvety hello Sith*.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-125">**Co-occurance**: Bob and Alice also liked *hello Phantom Menace*, *Attack of hello Clones*, and *Revenge of hello Sith*.</span></span> <span data-ttu-id="eb4ee-126">Mahout Určuje, že uživatelé, kteří líbilo hello předchozí tři filmy také jako tyto tři filmy.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-126">Mahout determines that users who liked hello previous three movies also like these three movies.</span></span>

* <span data-ttu-id="eb4ee-127">**Podobnosti doporučení**: protože Jan líbilo hello první tři filmy, Mahout vypadá na filmy ostatní s líbilo podobných předvolby, ale nebyla Jan sledovaná (líbilo nebo hodnocení).</span><span class="sxs-lookup"><span data-stu-id="eb4ee-127">**Similarity recommendation**: Because Joe liked hello first three movies, Mahout looks at movies that others with similar preferences liked, but Joe has not watched (liked/rated).</span></span> <span data-ttu-id="eb4ee-128">V takovém případě se doporučuje Mahout *hello fiktivní Menace*, *útoku hello klonů*, a *odvety hello Sith*.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-128">In this case, Mahout recommends *hello Phantom Menace*, *Attack of hello Clones*, and *Revenge of hello Sith*.</span></span>

### <a name="understanding-hello-data"></a><span data-ttu-id="eb4ee-129">Principy hello dat</span><span class="sxs-lookup"><span data-stu-id="eb4ee-129">Understanding hello data</span></span>

<span data-ttu-id="eb4ee-130">Pohodlně [GroupLens Research] [ movielens] poskytuje hodnocení data pro filmy ve formátu, který je kompatibilní s Mahout.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-130">Conveniently, [GroupLens Research][movielens] provides rating data for movies in a format that is compatible with Mahout.</span></span> <span data-ttu-id="eb4ee-131">Tato data jsou k dispozici na váš cluster výchozí úložiště na `/HdiSamples/HdiSamples/MahoutMovieData`.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-131">This data is available on your cluster's default storage at `/HdiSamples/HdiSamples/MahoutMovieData`.</span></span>

<span data-ttu-id="eb4ee-132">Existují dva soubory, `moviedb.txt` a `user-ratings.txt`.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-132">There are two files, `moviedb.txt` and `user-ratings.txt`.</span></span> <span data-ttu-id="eb4ee-133">Hello uživatele ratings.txt soubor se používá během analýzy, zatímco moviedb.txt je použité tooprovide popisný text informace při zobrazení hello výsledky analýzy hello.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-133">hello user-ratings.txt file is used during analysis, while moviedb.txt is used tooprovide user-friendly text information when displaying hello results of hello analysis.</span></span>

<span data-ttu-id="eb4ee-134">Hello data obsažená v ratings.txt uživatel má struktura `userID`, `movieID`, `userRating`, a `timestamp`, který víme, jak vysoce každý uživatel hodnocení filmu.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-134">hello data contained in user-ratings.txt has a structure of `userID`, `movieID`, `userRating`, and `timestamp`, which tells us how highly each user rated a movie.</span></span> <span data-ttu-id="eb4ee-135">Tady je příklad hello dat:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-135">Here is an example of hello data:</span></span>

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

## <a name="run-hello-analysis"></a><span data-ttu-id="eb4ee-136">Spuštění hello analýzy</span><span class="sxs-lookup"><span data-stu-id="eb4ee-136">Run hello analysis</span></span>

<span data-ttu-id="eb4ee-137">Z clusteru toohello připojení SSH použijte následující příkaz toorun hello doporučení úlohy hello:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-137">From an SSH connection toohello cluster, use hello following command toorun hello recommendation job:</span></span>

```bash
mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp
```

> [!NOTE]
> <span data-ttu-id="eb4ee-138">Úloha Hello může trvat několik minut toocomplete a může spustit víc úloh MapReduce.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-138">hello job may take several minutes toocomplete, and may run multiple MapReduce jobs.</span></span>

## <a name="view-hello-output"></a><span data-ttu-id="eb4ee-139">Výstup hello zobrazení</span><span class="sxs-lookup"><span data-stu-id="eb4ee-139">View hello output</span></span>

1. <span data-ttu-id="eb4ee-140">Po dokončení úlohy hello, použijte následující příkaz tooview hello generované výstup hello:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-140">Once hello job completes, use hello following command tooview hello generated output:</span></span>

    ```bash
    hdfs dfs -text /example/data/mahoutout/part-r-00000
    ```

    <span data-ttu-id="eb4ee-141">Zobrazí se následující výstup Hello:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-141">hello output appears as follows:</span></span>

        1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    <span data-ttu-id="eb4ee-142">první sloupec Hello je hello `userID`.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-142">hello first column is hello `userID`.</span></span> <span data-ttu-id="eb4ee-143">Hello hodnoty obsažené v ' [' a ']' jsou `movieId`:`recommendationScore`.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-143">hello values contained in '[' and ']' are `movieId`:`recommendationScore`.</span></span>

2. <span data-ttu-id="eb4ee-144">Další informace můžete použít výstup hello, společně s hello moviedb.txt, tooprovide na hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-144">You can use hello output, along with hello moviedb.txt, tooprovide more information on hello recommendations.</span></span> <span data-ttu-id="eb4ee-145">Nejdřív potřebujeme toocopy hello soubory místně pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-145">First, we need toocopy hello files locally using hello following commands:</span></span>

    ```bash
    hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
    hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .
    ```

    <span data-ttu-id="eb4ee-146">Tento příkaz kopie hello výstupní datový tooa soubor s názvem **recommendations.txt** v aktuálním adresáři hello, společně s hello film datových souborů.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-146">This command copies hello output data tooa file named **recommendations.txt** in hello current directory, along with hello movie data files.</span></span>

3. <span data-ttu-id="eb4ee-147">Použijte následující příkaz toocreate Python skript, který vyhledá názvy film hello dat v hello doporučení výstup hello:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-147">Use hello following command toocreate a Python script that looks up movie names for hello data in hello recommendations output:</span></span>

    ```bash
    nano show_recommendations.py
    ```

    <span data-ttu-id="eb4ee-148">Když se otevře hello editor, použijte následující text jako hello obsah souboru hello hello:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-148">When hello editor opens, use hello following text as hello contents of hello file:</span></span>

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

    <span data-ttu-id="eb4ee-149">Stiskněte klávesu **Ctrl-X**, **Y**a v neposlední řadě **Enter** toosave hello data.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-149">Press **Ctrl-X**, **Y**, and finally **Enter** toosave hello data.</span></span>

4. <span data-ttu-id="eb4ee-150">Spusťte skript v jazyce Python hello.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-150">Run hello Python script.</span></span> <span data-ttu-id="eb4ee-151">Hello následujícího příkazu se předpokládá, že jsou v adresáři hello, které byly staženy všechny soubory hello:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-151">hello following command assumes you are in hello directory where all hello files were downloaded:</span></span>

    ```bash
    python show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt
    ```

    <span data-ttu-id="eb4ee-152">Tento příkaz zjistí hello doporučení vygenerované uživatelské ID 4.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-152">This command looks at hello recommendations generated for user ID 4.</span></span>

    * <span data-ttu-id="eb4ee-153">Hello **uživatele ratings.txt** je použité tooretrieve filmy, které byly vyhodnoceny jako soubor.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-153">hello **user-ratings.txt** file is used tooretrieve movies that have been rated.</span></span>

    * <span data-ttu-id="eb4ee-154">Hello **moviedb.txt** soubor je použité tooretrieve hello názvy filmy hello.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-154">hello **moviedb.txt** file is used tooretrieve hello names of hello movies.</span></span>

    * <span data-ttu-id="eb4ee-155">Hello **recommendations.txt** je použité tooretrieve hello doporučení pro tohoto uživatele.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-155">hello **recommendations.txt** is used tooretrieve hello movie recommendations for this user.</span></span>

     <span data-ttu-id="eb4ee-156">Hello výstup z tohoto příkazu je podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-156">hello output from this command is similar toohello following text:</span></span>

        <span data-ttu-id="eb4ee-157">Stanovení skóre sedm let v Tibet (1997) = 5.0 Indiana Petr a hello poslední Crusade (1989), stanovení skóre = 5.0 Jaws (1975) skóre = 5.0 smysl a citlivosti (1995), skóre = 5.0 nezávislost den (ID4) (1996), skóre = 5.0 Moje nejlepší přítel svatbě (1997), stanovení skóre = 5.0 Jerry Maguire (1996 ), skóre = 5.0 Scream 2 (1997) skóre = 5.0 čas tooKill, (1996), stanovení skóre = 5.0</span><span class="sxs-lookup"><span data-stu-id="eb4ee-157">Seven Years in Tibet (1997), score=5.0   Indiana Jones and hello Last Crusade (1989), score=5.0   Jaws (1975), score=5.0   Sense and Sensibility (1995), score=5.0   Independence Day (ID4) (1996), score=5.0   My Best Friend's Wedding (1997), score=5.0   Jerry Maguire (1996), score=5.0   Scream 2 (1997), score=5.0   Time tooKill, A (1996), score=5.0</span></span>

## <a name="delete-temporary-data"></a><span data-ttu-id="eb4ee-158">Odstranit dočasná data</span><span class="sxs-lookup"><span data-stu-id="eb4ee-158">Delete temporary data</span></span>

<span data-ttu-id="eb4ee-159">Mahout úlohy neodebírejte dočasná data, který se vytvoří při zpracování úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-159">Mahout jobs do not remove temporary data that is created while processing hello job.</span></span> <span data-ttu-id="eb4ee-160">Hello `--tempDir` je zadán parametr v hello příklad úlohy tooisolate hello dočasné soubory do určité cesty pro snadné odstranění.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-160">hello `--tempDir` parameter is specified in hello example job tooisolate hello temporary files into a specific path for easy deletion.</span></span> <span data-ttu-id="eb4ee-161">tooremove hello dočasné soubory, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-161">tooremove hello temp files, use hello following command:</span></span>

```bash
hdfs dfs -rm -f -r /temp/mahouttemp
```

> [!WARNING]
> <span data-ttu-id="eb4ee-162">Pokud chcete toorun hello příkaz znovu, musíte také odstranit hello výstupního adresáře.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-162">If you want toorun hello command again, you must also delete hello output directory.</span></span> <span data-ttu-id="eb4ee-163">Použijte následující toodelete hello tento adresář:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-163">Use hello following toodelete this directory:</span></span>
>
> `hdfs dfs -rm -f -r /example/data/mahoutout`


## <a name="next-steps"></a><span data-ttu-id="eb4ee-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eb4ee-164">Next steps</span></span>

<span data-ttu-id="eb4ee-165">Teď, když jste se naučili, jak toouse Mahout, zjišťovat další způsoby, jak práci s daty v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-165">Now that you have learned how toouse Mahout, discover other ways of working with data on HDInsight:</span></span>

* [<span data-ttu-id="eb4ee-166">Hive s HDInsight</span><span class="sxs-lookup"><span data-stu-id="eb4ee-166">Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="eb4ee-167">Pig s HDInsight</span><span class="sxs-lookup"><span data-stu-id="eb4ee-167">Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="eb4ee-168">MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="eb4ee-168">MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

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
