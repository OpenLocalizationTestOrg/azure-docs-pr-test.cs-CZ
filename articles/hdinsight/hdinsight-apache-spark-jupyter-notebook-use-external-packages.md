---
title: "vlastní balíčky Maven aaaUse s Jupyter ve Sparku v Azure HDInsight | Microsoft Docs"
description: "Podrobný návod, jak tooconfigure poznámkové bloky Jupyter k dispozici s HDInsight Spark clusterů toouse vlastní Maven balíčky."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2a8bc545-064e-436f-8b5f-e67c26cfbf98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: ba8ac13716bc94ab082a18fe02d4a40b2f1e09e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Použijte externí balíčky s poznámkovými bloky Jupyter v clusterech Apache Spark v HDInsight
> [!div class="op_single_selector"]
> * [Pomocí buňky magic](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [Pomocí akce skriptu](hdinsight-apache-spark-python-package-installation.md)
>
>

Zjistěte, jak tooconfigure poznámkového bloku Jupyter v clusteru Apache Spark v HDInsight toouse externí, komunity podílí **maven** balíčky, které nejsou součástí clusteru hello se na pole. 

Můžete hledat hello [Maven úložiště](http://search.maven.org/) hello úplný seznam balíčků, které jsou k dispozici. Seznam dostupných balíčků můžete také získat z jiných zdrojů. Například je k dispozici úplný seznam balíčků podílí komunity [Spark balíčky](http://spark-packages.org/).

V tomto článku se dozvíte, jak toouse hello [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) balíček s hello Poznámkový blok Jupyter.



## <a name="prerequisites"></a>Požadavky
Musíte mít následující hello:

* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Použijte externí balíčky s poznámkovými bloky Jupyter
1. Z hello [portálu Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel). Můžete také přejít tooyour clusteru pod **Procházet vše** > **clustery HDInsight**.   
2. Z okna clusteru Spark hello, klikněte na tlačítko **rychlé odkazy**a potom z hello **řídicí panel clusteru** okně klikněte na tlačítko **Poznámkový blok Jupyter**. Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.

    > [!NOTE]
    > Může také nedostanete hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči. Nahraďte **CLUSTERNAME** s hello název clusteru:
    > 
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
    > 

   

3. Vytvořte nový poznámkový blok. Klikněte na tlačítko **nový**a potom klikněte na **Spark**.
   
    ![Vytvoření nového poznámkového bloku Jupyter](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Vytvoření nového poznámkového bloku Jupyter")

4. Nový poznámkový blok se vytvoří a otevřít s hello názvem Untitled.pynb. Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název.
   
    ![Zadejte název pro hello notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "zadejte název pro hello Poznámkový blok")

5. Budete používat hello `%%configure` magic tooconfigure hello poznámkového bloku toouse externí balíčku. Ujistěte se, volání hello v poznámkových bloků, které používají externí balíčky, `%%configure` magic v hello první buňky kódu. Tím se zajistí, že tento jádra hello je nakonfigurované toouse hello balíčku před zahájením relace hello.

    >[!IMPORTANT] 
    >Pokud zapomenete tooconfigure hello jádra hello první buňky, můžete použít hello `%%configure` s hello `-f` parametr, ale který restartuje hello relace a všechny průběh budou ztraceny.

    | HDInsight verze | Příkaz |
    |-------------------|---------|
    |Pro HDInsight 3.3 a HDInsight 3.4 | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|
    | Pro HDInsight 3.5 | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}`|

6. výše Hello fragmentu očekává hello maven souřadnice hello externí balíček v centrálním úložišti Maven. V této fragmentu kódu `com.databricks:spark-csv_2.10:1.4.0` je souřadnice maven hello **spark csv** balíčku. Zde je, jak vytvořit hello souřadnice pro balíček.
   
    a. Najděte balíček hello v hello Maven úložiště. V tomto kurzu používáme [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Z úložiště hello shromážděte hello hodnoty pro **GroupId**, **ArtifactId**, a **verze**. Ujistěte se, aby odpovídaly hello hodnoty, které shromáždíte vašeho clusteru. V tomto případě používáme Scala 2.10 a Spark 1.4.0 balíček, ale musíte tooselect různé verze pro příslušnou verzi Scala nebo Spark na hello v clusteru. Můžete zjistit verzi Scala hello v clusteru tak, že spustíte `scala.util.Properties.versionString` na hello Spark Jupyter jádra nebo odeslání Spark. Můžete zjistit hello Spark verze v clusteru tak, že spustíte `sc.version` v poznámkových blocích Jupyter.
   
    ![Použijte externí balíčky s Poznámkový blok Jupyter](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "použijte externí balíčky s poznámkového bloku Jupyter")
   
    c. Řetězení hello tři hodnoty, oddělené dvojtečkou (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

7. Spusťte buňky kódu hello s hello `%%configure` magic. Tím nakonfigurujete hello základní Livy relace toouse hello balíčku, který jste zadali. V hello následné buněk v hello Poznámkový blok můžete nyní používat hello balíček, jak je uvedeno níže.
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

8. Potom můžete spustit hello fragmenty, jako vidíte níže, tooview hello data z hello dataframe jste vytvořili v předchozím kroku hello.
   
        df.show()
   
        df.select("Time").count()

## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Vytvoření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření

* [Používat externí python balíčky s poznámkovými bloky Jupyter v clusterech Apache Spark na HDInsight Linux](hdinsight-apache-spark-python-package-installation.md)
* [Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)

