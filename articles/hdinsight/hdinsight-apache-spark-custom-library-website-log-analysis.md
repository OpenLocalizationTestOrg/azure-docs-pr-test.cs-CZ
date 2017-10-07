---
title: "aaaAnalyze webových protokolů pomocí knihovny Python ve Sparku - Azure | Microsoft Docs"
description: "Tento poznámkový blok ukazuje, jak tooanalyze protokolu dat pomocí Spark v Azure HDInsight vlastní knihovny."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8c61c70f-fe7f-4f0f-a4ab-0cccee5668c9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 29e4308b2a359aee6d69494a98307d4da07f7909
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-website-logs-using-a-custom-python-library-with-spark-cluster-on-hdinsight"></a>Analýza webových protokolů pomocí vlastní knihovna Python s clusterem Spark v HDInsight

Tento poznámkový blok ukazuje, jak tooanalyze protokolu dat pomocí Spark v HDInsight vlastní knihovny. Vlastní knihovna Hello používáme je knihovna Python názvem **iislogparser.py**.

> [!TIP]
> V tomto kurzu je také k dispozici jako poznámkový blok Jupyter v clusteru Spark (Linux), který vytvoříte v HDInsight. Hello poznámkového bloku prostředí vám umožní spustit hello Python fragmenty z Poznámkový blok hello sám sebe. kurz hello tooperform z v rámci Poznámkový blok, vytvořte Spark cluster, spusťte poznámkového bloku Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), a poté spusťte Poznámkový blok hello **Analýza protokolů s Spark pomocí vlastní library.ipynb** pod hello  **PySpark** složky.
>
>

**Požadavky:**

Musíte mít následující hello:

* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Uložit jako RDD nezpracovaná data
V této části používáme hello [Jupyter](https://jupyter.org) poznámkového bloku přidružené cluster Apache Spark v HDInsight toorun úlohy, které zpracování nezpracovaných ukázkových dat a uložte ho jako tabulku Hive. Hello ukázkových dat je soubor .csv (hvac.csv) k dispozici na všech clusterech ve výchozím nastavení.

Vaše data uložena jako tabulku Hive, v další části hello jsme se připojí toohello tabulku Hive pomocí nástrojů BI, například Power BI a Tableau.

1. Z hello [portál Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel). Můžete také přejít tooyour clusteru pod **Procházet vše** > **clustery HDInsight**.   
2. Z okna clusteru Spark hello, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **Poznámkový blok Jupyter**. Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.

   > [!NOTE]
   > Může také nedostanete hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči. Nahraďte **CLUSTERNAME** s hello název clusteru:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Vytvořte nový poznámkový blok. Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.

    ![Vytvoření nového poznámkového bloku Jupyter](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-create-jupyter-notebook.png "Vytvoření nového poznámkového bloku Jupyter")
4. Nový poznámkový blok se vytvoří a otevřít s hello názvem Untitled.pynb. Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název.

    ![Zadejte název pro hello notebook](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-name-jupyter-notebook.png "zadejte název pro hello Poznámkový blok")
5. Vzhledem k tomu, že jste vytvořili pomocí jádra PySpark hello Poznámkový blok, není nutné toocreate tvořit kontexty explicitně. Hello kontexty Spark a Hive se automaticky vytvoří za vás při spuštění první buňky kódu hello. Můžete začít importem hello typy, které jsou požadovány pro tento scénář. Vložte následující fragment kódu do prázdné buňky hello a potom stiskněte klávesu **SHIFT + ENTER**.

        from pyspark.sql import Row
        from pyspark.sql.types import *


1. Vytvoření RDD pomocí hello ukázková protokolu data již k dispozici v clusteru hello. Můžou k datům hello ve hello výchozí účet úložiště přidruženého k hello clusteru na **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.

        logs = sc.textFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


1. Načtěte ukázkové sady tooverify protokolu, který hello předchozího kroku byla úspěšně dokončena.

        logs.take(5)

    Měli byste vidět výstup podobný toohello následující:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Analyzovat data protokolu pomocí vlastní knihovna Python
1. Ve výstupu hello výše hello prvních několika řádků obsahovat informace hlavičky hello a každý zbývající řádek odpovídá schématu hello popsané v této hlavičce. Analýza tyto protokoly může být složité. Ano, používáme vlastní knihovna Python (**iislogparser.py**), díky analýze tyto protokoly mnohem jednodušší. Ve výchozím nastavení, je součástí clusteru Spark v HDInsight v této knihovně **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.

    Ale v této knihovně se nenachází v hello `PYTHONPATH` tak jsme nelze použít pomocí příkazu import jako `import iislogparser`. toouse tuto knihovnu jsme ji musíte distribuovat tooall hello pracovním uzlům. Spusťte hello následující fragment kódu.

        sc.addPyFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


1. `iislogparser`poskytuje funkci `parse_log_line` , který vrací `None` Pokud protokolu řádek je řádek záhlaví a vrací instanci třídy hello `LogLine` třídy v případě nalezení řádek protokolu. Použití hello `LogLine` třída tooextract pouze hello protokolu řádky z hello RDD:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()
2. Načtení pár tooverify extrahované protokolu řádky, které hello krok byla úspěšně dokončena.

       logLines.take(2)

   výstup Hello by měl vypadat podobně jako toohello následující:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]
3. Hello `LogLine` třída, pak má některé užitečné metody, jako je třeba `is_error()`, který vrátí, zda položka protokolu má chybový kód. Pomocí této toocompute hello počet chyb v hello extrahovat protokolu řádky a potom všechny hello chyby tooa jiný soubor protokolu.

       errors = logLines.filter(lambda p: p.is_error())
       numLines = logLines.count()
       numErrors = errors.count()
       print 'There are', numErrors, 'errors and', numLines, 'log entries'
       errors.map(lambda p: str(p)).saveAsTextFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

   Měli byste vidět výstup jako hello následující:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       There are 30 errors and 646 log entries
4. Můžete také použít **Matplotlib** tooconstruct vizualizace dat hello. Například pokud chcete tooisolate hello příčinu požadavků, které spustit po dlouhou dobu, můžete toofind hello soubory, které provést hello většinu času tooserve v průměru.
   Následující fragment Hello načte hello nejvyšší 25 zdroje, které trvalo tooserve většinu času požadavku.

       def avgTimeTakenByKey(rdd):
           return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                   lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                   lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                     .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))

       avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

   Měli byste vidět výstup jako hello následující:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [(u'/blogposts/mvc4/step13.png', 197.5),
        (u'/blogposts/mvc2/step10.jpg', 179.5),
        (u'/blogposts/extractusercontrol/step5.png', 170.0),
        (u'/blogposts/mvc4/step8.png', 159.0),
        (u'/blogposts/mvcrouting/step22.jpg', 155.0),
        (u'/blogposts/mvcrouting/step3.jpg', 152.0),
        (u'/blogposts/linqsproc1/step16.jpg', 138.75),
        (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
        (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
        (u'/blogposts/nested/step2.jpg', 126.0),
        (u'/blogposts/adminpack/step1.png', 124.0),
        (u'/BlogPosts/datalistpaging/step2.png', 118.0),
        (u'/blogposts/mvc4/step35.png', 117.0),
        (u'/blogposts/mvcrouting/step2.jpg', 116.5),
        (u'/blogposts/aboutme/basketball.jpg', 109.0),
        (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
        (u'/blogposts/mvc4/step12.png', 106.0),
        (u'/blogposts/linq8/step0.jpg', 105.5),
        (u'/blogposts/mvc2/step18.jpg', 104.0),
        (u'/blogposts/mvc2/step11.jpg', 104.0),
        (u'/blogposts/mvcrouting/step1.jpg', 104.0),
        (u'/blogposts/extractusercontrol/step1.png', 103.0),
        (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
        (u'/blogposts/mvcrouting/step21.jpg', 101.0),
        (u'/blogposts/mvc4/step1.png', 98.0)]
5. Můžete také prezentuje tyto informace v hello formátu vykreslení. Jako první krok toocreate vykreslení. dejte nám nejdřív vytvořit dočasné tabulky **AverageTime**. Hello tabulka skupiny hello zaznamená podle času toosee, pokud v kterémkoli okamžiku nebyly žádné neobvyklou latence špičky.

       avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
       schema = StructType([StructField('Minutes', IntegerType(), True),
                            StructField('Time', FloatType(), True)])

       avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
       avgTimeTakenByMinuteDF.registerTempTable('AverageTime')
6. Potom můžete spustit následující tooget dotazu SQL hello všechny záznamy hello v hello **AverageTime** tabulky.

       %%sql -o averagetime
       SELECT * FROM AverageTime

   Hello `%%sql` magic následuje `-o averagetime` zajistí, že výstup hello hello dotazu je trvalé místně na serveru Jupyter hello (obvykle hello headnode hello clusteru). výstup Hello je uchován jako [Pandas](http://pandas.pydata.org/) dataframe s hello zadaný název **averagetime**.

   Měli byste vidět výstup jako hello následující:

   ![Výstup dotazu SQL](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-jupyter-sql-qyery-output.png "výstupu dotazu SQL")

   Další informace o hello `%%sql` magic, viz [parametry podporovány s hello %% sql magic](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).
7. Teď můžete použít Matplotlib, knihovny použita tooconstruct vizualizaci dat, toocreate vykreslení. Protože hello výkresu musí být vytvořeny z místně hello trvalé **averagetime** dataframe, fragment kódu hello musí začínat řetězcem hello `%%local` magic. To zajistí hello kód je místně na serveru Jupyter hello.

       %%local
       %matplotlib inline
       import matplotlib.pyplot as plt

       plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
       plt.xlabel('Time (min)')
       plt.ylabel('Average time taken for request (ms)')

   Měli byste vidět výstup jako hello následující:

   ![Výstup Matplotlib](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-apache-spark-web-log-analysis-plot.png "Matplotlib výstup")
8. Po dokončení spuštění aplikace hello, měli byste vypnout hello poznámkového bloku toorelease hello prostředky. toodo Ano, z hello **soubor** nabídce hello Poznámkový blok, klikněte na tlačítko **zavřít a zastavit**. Dojde k vypnutí a zavřít hello Poznámkový blok.

## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)

### <a name="create-and-run-applications"></a>Vytvoření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
