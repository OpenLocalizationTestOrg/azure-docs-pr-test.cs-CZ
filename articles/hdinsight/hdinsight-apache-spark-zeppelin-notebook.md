---
title: "clusteru aaaUse poznámkových bloků Zeppelin s Apache Spark v Azure HDInsight | Microsoft Docs"
description: "Podrobný návod, jak clusterů toouse poznámkových bloků Zeppelin s Apache Spark v Azure HDInsight."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: df489d70-7788-4efa-a089-e5e5006421e2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 3ab479cfccc7fd38a9bf6a9fb4f5928beec8ff7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Použití poznámkových bloků Zeppelin s clusterem Apache Spark v Azure HDInsight

Clustery HDInsight Spark zahrnují poznámkových bloků Zeppelin, které můžete použít toorun Spark úlohy. V tomto článku se dozvíte, jak toouse hello Zeppelin poznámkového bloku v clusteru HDInsight.

> [!NOTE]
> Poznámkových bloků Zeppelin jsou dostupné pouze pro 1.6.3 Spark v HDInsight 3.5 a 2.1.0 Spark v HDInsight 3.6.
>

**Požadavky:**

* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Spusťte Zeppelin Poznámkový blok
1. Z okna clusteru Spark hello, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **Zeppelin Poznámkový blok**. Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.
   
   > [!NOTE]
   > Může také nedostanete hello Zeppelin Poznámkový blok pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči. Nahraďte **CLUSTERNAME** s hello název clusteru:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`
   > 
   > 
2. Vytvořte nový poznámkový blok. Z hello podokno záhlaví, klikněte na tlačítko **poznámkového bloku**a potom klikněte na **vytvořit novou poznámku**.
   
    ![Vytvořte nový poznámkový blok Zeppelin](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "vytvořte nový poznámkový blok Zeppelin")
   
    Zadejte název pro hello Poznámkový blok a pak klikněte na tlačítko **vytvořit Poznámka**.
3. Také zkontrolujte, zda záhlaví poznámkového bloku hello zobrazí připojené stav. Označuje zelená tečky v pravém horním rohu hello.
   
    ![Stav poznámkového bloku Zeppelin](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin poznámkového bloku stav")
4. Načtěte vzorová data do dočasné tabulky. Když vytvoříte Spark cluster v HDInsight, ukázkový datový soubor hello, **hvac.csv**, je zkopírovaný toohello přidruženého účtu úložiště **\HdiSamples\SensorSampleData\hvac**.
   
    V prázdné odstavci hello, který se vytvoří ve výchozím nastavení v hello nový poznámkový blok vložte následující fragment kódu hello.
   
        %livy.spark
        //hello above magic instructs Zeppelin toouse hello Livy Scala interpreter
   
        // Create an RDD using hello default Spark context, sc
        val hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
   
        // Map hello values in hello .csv file toohello schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
   
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
   
    Stiskněte klávesu **SHIFT + ENTER** nebo klikněte na tlačítko hello **přehrání** tlačítko hello odstavce toorun hello fragment kódu. Stav Hello na hello pravém rohu hello odstavce by měl průběhu z PŘIPRAVENÝ, čeká na vyřízení, tooFINISHED SPUŠTĚNÁ. výstup Hello objeví dole hello hello stejné odstavce. snímek obrazovky Hello vypadá hello následující:
   
    ![Vytvořte dočasnou tabulku z nezpracovaná data](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "vytvořit dočasnou tabulku od nezpracovaných dat")
   
    Můžete zadat také název tooeach odstavec. V pravém rohu hello, klikněte na hello **nastavení** ikonu a pak klikněte na tlačítko **zobrazit nadpis**.
5. Teď můžete spustit příkazy Spark SQL na hello **TVK** tabulky. Vložte hello následující dotaz nový odstavec. Hello dotaz načte ID budovy hello a hello rozdíl mezi hello cíl a skutečný teploty pro každé sestavení v určitém dni. Stiskněte klávesu **SHIFT + ENTER**.
   
        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
   
    Hello **% sql** příkaz od začátku hello sděluje hello poznámkového bloku toouse hello Livy Scala překladač.
   
    Hello následující snímek obrazovky ukazuje výstup hello.
   
    ![Spustit příkaz Spark SQL pomocí poznámkového bloku hello](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "spustit příkaz Spark SQL pomocí poznámkového bloku hello")
   
     Klikněte na tlačítko hello zobrazení možnosti (zvýraznit v obdélníku) tooswitch mezi různé reprezentace pro hello stejný výstup. Klikněte na tlačítko **nastavení** toochoose co consitutes hello klíče a hodnoty ve výstupu hello. snímek obrazovky výše používá Hello **buildingID** jako klíč hello a průměr hello **temp_diff** jako hodnota hello.
6. Můžete také spustit příkazy Spark SQL pomocí proměnných v dotazu hello. Hello další fragment kódu ukazuje jak toodefine proměnnou, **Temp**, hello dotaz s hello možné hodnoty chcete tooquery s. Při prvním spuštění dotazu hello, rozevírací seznam se automaticky zadá hello hodnoty, které jste zadali pro proměnnou hello.
   
        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 
   
    Vložte tento fragment kódu nový odstavec a stiskněte klávesu **SHIFT + ENTER**. Hello následující snímek obrazovky ukazuje výstup hello.
   
    ![Spustit příkaz Spark SQL pomocí poznámkového bloku hello](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "spustit příkaz Spark SQL pomocí poznámkového bloku hello")
   
    Pro následné dotazy můžete vybrat novou hodnotu z rozevíracího seznamu hello a znovu spusťte dotaz hello. Klikněte na tlačítko **nastavení** toochoose co consitutes hello klíče a hodnoty ve výstupu hello. snímek obrazovky výše používá Hello **buildingID** jako klíč hello hello průměrný počet **temp_diff** jako hello hodnotu, a **targettemp** jako skupina hello.
7. Restartujte hello Livy překladač tooexit hello aplikace. toodo tak, že otevřete nastavení překladač kliknutím hello přihlášení uživatelské jméno ze hello pravém horním rohu a pak klikněte na **překladač**.
   
    ![Spustí překladač](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "výstupu podregistru")
8. Posuňte se tooLivy překladač nastavení a pak klikněte na tlačítko **restartujte**.
   
    ![Restartujte hello Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "restartujte hello Zeppelin intepreter")

## <a name="how-do-i-use-external-packages-with-hello-notebook"></a>Jak používat externí balíčky s hello Poznámkový blok?
V clusteru Apache Spark v HDInsight (Linux) toouse externí, komunity podílí balíčky, které nejsou zahrnuté out-of-the-box v hello clusteru můžete nakonfigurovat hello Zeppelin poznámkového bloku. Můžete hledat hello [Maven úložiště](http://search.maven.org/) hello úplný seznam balíčků, které jsou k dispozici. Seznam dostupných balíčků můžete také získat z jiných zdrojů. Například je k dispozici úplný seznam balíčků podílí komunity [Spark balíčky](http://spark-packages.org/).

V tomto článku se zobrazí jak toouse hello [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) balíček s hello Poznámkový blok Jupyter.

1. Otevřete nastavení překladač. V pravém horním rohu hello, klikněte hello přihlášení uživatelské jméno a potom klikněte na **překladač**.
   
    ![Spustí překladač](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "výstupu podregistru")
2. Posuňte se tooLivy překladač nastavení a pak klikněte na tlačítko **upravit**.
   
    ![Změna nastavení překladač](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "změnit nastavení překladač")
3. Přidejte nový klíč, nazývá **livy.spark.jars.packages** a nastavení jeho hodnoty ve formátu hello `group:id:version`. Pokud chcete, aby toouse hello tedy [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) balíčku, je nutné nastavit hello hodnotu klíče hello příliš`com.databricks:spark-csv_2.10:1.4.0`.
   
    ![Změna nastavení překladač](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "změnit nastavení překladač")
   
    Klikněte na tlačítko **Uložit** a pak restartujte hello Livy překladač.
4. **Tip**: Pokud chcete, aby toounderstand jak tooarrive v hodnotě hello hello klíče zadaná výše, zde uvádíme jak.
   
    a. Najděte balíček hello v hello Maven úložiště. V tomto kurzu jsme použili [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Z úložiště hello shromážděte hello hodnoty pro **GroupId**, **ArtifactId**, a **verze**.
   
    ![Použijte externí balíčky s Poznámkový blok Jupyter](./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "použijte externí balíčky s poznámkového bloku Jupyter")
   
    c. Řetězení hello tři hodnoty, oddělené dvojtečkou (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-hello-zeppelin-notebooks-saved"></a>Kde jsou hello poznámkových bloků Zeppelin uložit?
poznámkových bloků Zeppelin Hello ukládají headnodes toohello clusteru. Takže pokud odstraníte hello clusteru, se odstraní také hello poznámkových bloků. Pokud chcete, toopreserve poznámkové bloky pro pozdější použití v jiných clusterech, je nutné je po dokončení probíhajících úloh hello exportovat. tooexport Poznámkový blok, klikněte na tlačítko hello **exportovat** ikonu, jak ukazuje následující obrázek hello.

![Stáhnout poznámkového bloku](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "stažení hello Poznámkový blok")

To umožňuje ušetřit hello Poznámkový blok jako soubor JSON ve vašem umístění stahování.

## <a name="livy-session-management"></a>Správa relací Livy
Když spustíte hello prvním odstavci kódu v poznámkovém Zeppelin, je vytvořit novou relaci Livy v clusteru HDInsight Spark. Tuto relaci je sdílen na všech poznámkových bloků Zeppelin, které následně vytvoříte. Pokud pro některé hello důvod Livy relace je ukončená (restartování clusteru atd.) a nebude ji již možné toorun úlohy z hello Zeppelin poznámkového bloku.

V takovém případě je nutné provést následující kroky předtím, než můžete začít spouštět úlohy z Poznámkový blok Zeppelin hello. 

1. Restartujte hello Livy překladač z hello Zeppelin poznámkového bloku. toodo tak, že otevřete nastavení překladač kliknutím hello přihlášení uživatelské jméno ze hello pravém horním rohu a pak klikněte na **překladač**.
   
    ![Spustí překladač](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "výstupu podregistru")
2. Posuňte se tooLivy překladač nastavení a pak klikněte na tlačítko **restartujte**.
   
    ![Restartujte hello Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "restartujte hello Zeppelin intepreter")
3. Spusťte buňky kódu z existujícího Zeppelin poznámkového bloku. Tím se vytvoří novou relaci Livy v clusteru HDInsight hello.

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
* [Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md 







