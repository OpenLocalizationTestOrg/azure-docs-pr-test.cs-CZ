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
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight-ssh"></a>Generování doporučení pomocí Apache Mahout se systémem Linux Hadoop v HDInsight (SSH)

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Zjistěte, jak toouse hello [Apache Mahout](http://mahout.apache.org) knihovny machine learning s Azure HDInsight toogenerate doporučení.

Je mahout [strojového učení] [ ml] knihovny pro Apache Hadoop. Mahout obsahuje algoritmy pro zpracování dat, jako je například filtrování, klasifikace a clustering. V tomto článku použijete doporučení film toogenerate modul doporučení, které jsou založeny na filmy, které jste viděli vašich přátel.

## <a name="prerequisites"></a>Požadavky

* Cluster HDInsight se systémem Linux. Informace o vytváření jeden najdete v tématu [začít používat systémem Linux Hadoop v HDInsight][getstarted].

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Klientem SSH. Další informace najdete v tématu hello [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentu.

## <a name="mahout-versioning"></a>Správa verzí mahout

Další informace o verzi hello Mahout v prostředí HDInsight najdete v tématu [HDInsight verze a komponent systému Hadoop](hdinsight-component-versioning.md).

## <a name="recommendations"></a>Vysvětlení doporučení

Jedním z hello funkce, které zajišťuje Mahout je modul doporučení. Tento modul přijímá data ve formátu hello `userID`, `itemId`, a `prefValue` (hello předvoleb pro položku hello). Mahout pak můžete provádět analýzy toodetermine společné occurance: *uživatelé, kteří mají předvolbu pro položku také mít předvolbu pro tyto další položky*. Mahout pak určuje uživatelé s jako položky předvoleb, které můžou být použité toomake doporučení.

Hello následující pracovní postup je zjednodušená příklad, který používá film data:

* **Společné occurance**: Jan, Alice a Bob všechny líbilo *hvězdičky válek*, *hello Empire stávky zpět*, a *návrat hello Jedi*. Mahout Určuje, že uživatelé, kteří jako některého z těchto filmy chtěl také hello další dvě.

* **Společné occurance**: Bob a Alice také líbilo *hello fiktivní Menace*, *útoku hello klonů*, a *odvety hello Sith*. Mahout Určuje, že uživatelé, kteří líbilo hello předchozí tři filmy také jako tyto tři filmy.

* **Podobnosti doporučení**: protože Jan líbilo hello první tři filmy, Mahout vypadá na filmy ostatní s líbilo podobných předvolby, ale nebyla Jan sledovaná (líbilo nebo hodnocení). V takovém případě se doporučuje Mahout *hello fiktivní Menace*, *útoku hello klonů*, a *odvety hello Sith*.

### <a name="understanding-hello-data"></a>Principy hello dat

Pohodlně [GroupLens Research] [ movielens] poskytuje hodnocení data pro filmy ve formátu, který je kompatibilní s Mahout. Tato data jsou k dispozici na váš cluster výchozí úložiště na `/HdiSamples/HdiSamples/MahoutMovieData`.

Existují dva soubory, `moviedb.txt` a `user-ratings.txt`. Hello uživatele ratings.txt soubor se používá během analýzy, zatímco moviedb.txt je použité tooprovide popisný text informace při zobrazení hello výsledky analýzy hello.

Hello data obsažená v ratings.txt uživatel má struktura `userID`, `movieID`, `userRating`, a `timestamp`, který víme, jak vysoce každý uživatel hodnocení filmu. Tady je příklad hello dat:

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

## <a name="run-hello-analysis"></a>Spuštění hello analýzy

Z clusteru toohello připojení SSH použijte následující příkaz toorun hello doporučení úlohy hello:

```bash
mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp
```

> [!NOTE]
> Úloha Hello může trvat několik minut toocomplete a může spustit víc úloh MapReduce.

## <a name="view-hello-output"></a>Výstup hello zobrazení

1. Po dokončení úlohy hello, použijte následující příkaz tooview hello generované výstup hello:

    ```bash
    hdfs dfs -text /example/data/mahoutout/part-r-00000
    ```

    Zobrazí se následující výstup Hello:

        1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    první sloupec Hello je hello `userID`. Hello hodnoty obsažené v ' [' a ']' jsou `movieId`:`recommendationScore`.

2. Další informace můžete použít výstup hello, společně s hello moviedb.txt, tooprovide na hello doporučení. Nejdřív potřebujeme toocopy hello soubory místně pomocí hello následující příkazy:

    ```bash
    hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
    hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .
    ```

    Tento příkaz kopie hello výstupní datový tooa soubor s názvem **recommendations.txt** v aktuálním adresáři hello, společně s hello film datových souborů.

3. Použijte následující příkaz toocreate Python skript, který vyhledá názvy film hello dat v hello doporučení výstup hello:

    ```bash
    nano show_recommendations.py
    ```

    Když se otevře hello editor, použijte následující text jako hello obsah souboru hello hello:

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

    Stiskněte klávesu **Ctrl-X**, **Y**a v neposlední řadě **Enter** toosave hello data.

4. Spusťte skript v jazyce Python hello. Hello následujícího příkazu se předpokládá, že jsou v adresáři hello, které byly staženy všechny soubory hello:

    ```bash
    python show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt
    ```

    Tento příkaz zjistí hello doporučení vygenerované uživatelské ID 4.

    * Hello **uživatele ratings.txt** je použité tooretrieve filmy, které byly vyhodnoceny jako soubor.

    * Hello **moviedb.txt** soubor je použité tooretrieve hello názvy filmy hello.

    * Hello **recommendations.txt** je použité tooretrieve hello doporučení pro tohoto uživatele.

     Hello výstup z tohoto příkazu je podobné toohello následující text:

        Stanovení skóre sedm let v Tibet (1997) = 5.0 Indiana Petr a hello poslední Crusade (1989), stanovení skóre = 5.0 Jaws (1975) skóre = 5.0 smysl a citlivosti (1995), skóre = 5.0 nezávislost den (ID4) (1996), skóre = 5.0 Moje nejlepší přítel svatbě (1997), stanovení skóre = 5.0 Jerry Maguire (1996 ), skóre = 5.0 Scream 2 (1997) skóre = 5.0 čas tooKill, (1996), stanovení skóre = 5.0

## <a name="delete-temporary-data"></a>Odstranit dočasná data

Mahout úlohy neodebírejte dočasná data, který se vytvoří při zpracování úlohy hello. Hello `--tempDir` je zadán parametr v hello příklad úlohy tooisolate hello dočasné soubory do určité cesty pro snadné odstranění. tooremove hello dočasné soubory, použijte následující příkaz hello:

```bash
hdfs dfs -rm -f -r /temp/mahouttemp
```

> [!WARNING]
> Pokud chcete toorun hello příkaz znovu, musíte také odstranit hello výstupního adresáře. Použijte následující toodelete hello tento adresář:
>
> `hdfs dfs -rm -f -r /example/data/mahoutout`


## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili, jak toouse Mahout, zjišťovat další způsoby, jak práci s daty v HDInsight:

* [Hive s HDInsight](hdinsight-use-hive.md)
* [Pig s HDInsight](hdinsight-use-pig.md)
* [MapReduce s HDInsight](hdinsight-use-mapreduce.md)

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
