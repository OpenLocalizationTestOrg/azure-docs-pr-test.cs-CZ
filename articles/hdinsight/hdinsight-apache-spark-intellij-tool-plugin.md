---
title: "Azure nástrojů pro IntelliJ: vytvoření aplikací Spark pro cluster služby HDInsight | Microsoft Docs"
description: "Použijte hello Azure Toolkit pro IntelliJ toodevelop Spark aplikace napsané v jazyce Scala a odesílat je tooan clusteru HDInsight Spark."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 73304272-6c8b-482e-af7c-cd25d95dab4d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: nitinme
ms.openlocfilehash: 22cce014bb848a54e198e77a50bf13448012310e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-intellij-toocreate-spark-applications-for-an-hdinsight-cluster"></a>Použití nástrojů Azure pro IntelliJ toocreate aplikací Spark pro cluster služby HDInsight

Použijte hello Azure Toolkit pro IntelliJ modulu plug-in toodevelop Spark aplikace napsané v jazyce Scala a odesílat je tooan clusteru HDInsight Spark přímo z hello IntelliJ integrované vývojové prostředí (IDE). Můžete použít hello modulu plug-in několika způsoby:

* Vývoj a odesílání aplikací Scala Spark na clusteru HDInsight Spark.
* Přístup k prostředkům clusteru Azure HDInsight Spark.
* Vytvořte a spusťte aplikací Scala Spark místně.

toocreate projektu, zobrazení hello [vytvoření aplikací Spark s hello nástrojů Azure pro IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) videa.

> [!IMPORTANT]
> Můžete použít tento modul plug-in toocreate a odeslání aplikací pouze pro cluster služby HDInsight Spark na systému Linux.
> 

## <a name="prerequisites"></a>Požadavky

- Cluster Apache Spark v HDInsight Linux. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
- Oracle Java Development Kit. Můžete ji nainstalovat ze hello [Oracle webu](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
- IntelliJ IDEA. Tento článek používá verzi 2017.1. Můžete ji nainstalovat ze hello [JetBrains webu](https://www.jetbrains.com/idea/download/).

## <a name="install-azure-toolkit-for-intellij"></a>Instalace Azure Toolkit pro IntelliJ
Pokyny k instalaci naleznete v tématu [instalaci nástrojů Azure pro IntelliJ](../azure-toolkit-for-intellij-installation.md).

## <a name="sign-in-tooyour-azure-subscription"></a>Přihlaste se tooyour předplatného Azure

1. Spusťte hello IntelliJ IDE a otevřete Průzkumník Azure. Na hello **zobrazení** nabídce vyberte možnost **nástroj Windows**a potom vyberte **Azure Explorer**.
       
   ![Průzkumník Azure odkaz Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/show-azure-explorer.png)

2. Klikněte pravým tlačítkem na hello **Azure** uzel a potom vyberte **přihlásit**.

3. V hello **přihlásit k Azure** dialogové okno, vyberte **přihlášení**a pak zadejte přihlašovací údaje Azure.

    ![Hello přihlásit k Azure dialogové okno](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-2.png)

4. Poté, co jste přihlášení, hello **vyberte odběry** zobrazí dialogové okno pole všechny hello předplatná Azure, které jsou přidružené hello přihlašovací údaje. Vyberte hello **vyberte** tlačítko.

    ![Dialogové okno Vybrat odběry Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

5. Na hello **Azure Explorer** rozbalte **HDInsight** tooview hello clustery HDInsight Spark, které jsou v rámci vašeho předplatného.
   
    ![Clustery HDInsight Spark v Azure Explorer](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-3.png)

6. tooview hello prostředky (například účty úložiště) přidružených hello clusteru, můžete dále rozšířit uzlem název clusteru.
   
    ![Rozbalené uzly název clusteru](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Spuštění aplikace na Spark Scala clusteru HDInsight Spark

1. Spusťte IntelliJ IDEA a pak vytvořte projekt. V hello **nový projekt** dialogové okno pole, hello následující: 

   a. Vyberte **HDInsight** > **Spark v HDInsight (Scala)**.

   b. V hello **nástroj pro sestavení** vyberte jednu z následujících, podle potřeby tooyour hello:

      * **Maven**, podpora Průvodce vytvoření projektu Scala
      * **SBT**, pro správu hello závislosti a sestavování pro projekt hello Scala

    ![Dialogové okno Nový projekt Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

2. Vyberte **Další**.

3. Průvodce vytvoření projektu Scala Hello automaticky zjišťuje, zda jste nainstalovali hello Scala modulu plug-in. Vyberte **nainstalovat**.

   ![Kontrola modul plug-in Scala](./media/hdinsight-apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. hello toodownload Scala modul plug-in, vyberte **OK**. Postupujte podle pokynů toorestart hello IntelliJ. 

   ![Dialogové okno modul plug-in Instalace Hello Scala](./media/hdinsight-apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. V hello **nový projekt** okně hello následující:  

    ![Výběr hello Spark SDK](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-new-project.png)

   a. Zadejte název projektu a umístění.

   b. V hello **SDK projektu** rozevíracího seznamu vyberte **Java 1.8** hello Spark 2.x clusteru, nebo vyberte **Java 1.7** hello Spark 1.x clusteru.

   c. V hello **Spark verze** rozevíracího seznamu, Průvodce vytvořením projektu Scala integruje hello správnou verzi sady SDK Spark a Scala SDK. Pokud verze clusteru Spark hello je starší než 2.0, vyberte **Spark 1.x**. Jinak vyberte možnost **Spark2.x**. Tento příklad používá **Spark bodu 2.0.2 (Scala 2.11.8)**.

6. Vyberte **Finish** (Dokončit).

7. projekt Hello Spark pro vás automaticky vytvoří artefakt. tooview hello artefaktů, hello následující:

   a. Na hello **soubor** nabídce vyberte možnost **strukturu projektu**.

   b. V hello **strukturu projektu** dialogové okno, vyberte **artefakty** tooview hello výchozí artefaktů, který je vytvořen. Můžete také vytvořit vlastní artefaktů výběrem hello – znaménko plus (**+**).

      ![Informace o artefaktů v dialogovém okně hello](./media/hdinsight-apache-spark-intellij-tool-plugin/default-artifact.png)
      
8. Přidejte zdrojový kód aplikace pomocí tohoto postupu hello následující:

   a. V prohlížeči projektu klikněte pravým tlačítkem na **src**, bod příliš**nový**a potom vyberte **Scala třída**.
      
      ![Příkazy pro vytvoření třídy Scala z Project Exploreru](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   b. V hello **vytvořte novou třídu Scala** dialogové okno, zadejte název, vyberte **objekt** v hello **druhu** a pak vyberte **OK**.
      
      ![Nová třída Scala dialogové okno vytvořit](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   c. V hello **MyClusterApp.scala** souboru, vložte následující kód hello. Hello kód čte hello data z HVAC.csv (k dispozici na všech clusterech HDInsight Spark), načte hello řádky, které mají jenom jednu číslici hello sedmého sloupce v souboru CSV hello a zapíše výstup hello příliš**/HVACOut** pod výchozí hello Kontejner úložiště pro hello cluster.

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object MyClusterApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find hello rows that have only one digit in hello seventh column in hello CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasb:///HVACOut")
            }
    
        }

9. Spuštění aplikace hello na clusteru HDInsight Spark pomocí tohoto postupu hello následující:

   a. V prohlížeči projektu klikněte pravým tlačítkem na název projektu hello a pak vyberte **odesílání aplikací Spark tooHDInsight**.
      
      ![Hello příkaz tooHDInsight odesílání aplikací Spark](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

   b. Můžete se výzvami tooenter přihlašovací údaje předplatného Azure. V hello **Spark odeslání** dialogové okno, zadejte následující hodnoty hello a pak vyberte **odeslání**.
      
      * Pro **Spark clustery (pouze Linux)**, vyberte hello clusteru HDInsight Spark, na kterém chcete toorun vaší aplikace.

      * Artefakt z hello IntelliJ projektu, nebo vyberte jednu z pevného disku hello.

      * V hello **hlavní název třídy** pole, vyberte hello třemi tečkami (**...** ), vyberte hello hlavní třídy ve zdrojovém kódu aplikace a pak vyberte **OK**.

        ![Dialogové okno Vybrat hlavní třída Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)

      * Protože kód aplikace hello v tomto příkladu nevyžaduje argumenty příkazového řádku ani odkazovat JAR nebo soubory, můžete ponechat hello zbývající polí prázdné. Po zadání všech informací hello dialogové okno hello by měla vypadat přibližně hello následující obrázek.
        
        ![Dialogové okno Spark odeslání Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

   c. Hello **Spark odeslání** kartě v hello dolní části okna hello by se měl spustit zobrazování průběhu hello. Můžete také zastavit aplikace hello výběrem hello červené tlačítko v hello **Spark odeslání** okno.
      
      ![okno Spark odeslání Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      toolearn o tooaccess hello výstup úlohy v tématu hello "přístup a spravovat clustery HDInsight Spark pomocí nástrojů Azure pro IntelliJ" později v tomto článku.

## <a name="run-or-debug-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Spustit nebo ladění aplikací Spark Scala na clusteru HDInsight Spark
Doporučujeme také jiný způsob odesílání hello Spark aplikace toohello clusteru. Můžete tak učinit nastavením hello parametry v hello **konfigurace spustit/Debug** IDE. Další informace najdete v tématu [vzdálené ladění aplikací Spark v clusteru HDInsight pomocí sady Azure Toolkit pro IntelliJ prostřednictvím SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Přístup a spravovat clustery HDInsight Spark pomocí nástrojů Azure pro IntelliJ
Pomocí nástrojů Azure pro IntelliJ můžete provádět různé operace.

### <a name="access-hello-job-view"></a>Zobrazení úloh hello přístup
1. V Průzkumníku Azure rozbalte **HDInsight**, rozbalte název clusteru Spark hello a pak vyberte **úlohy**.  

    ![Úlohy zobrazení uzlu](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. V pravém podokně hello hello **zobrazení úloh Spark** karta zobrazuje všechny hello aplikace, které byly spuštěny v clusteru hello. Vyberte název hello hello aplikace, pro které chcete toosee další podrobnosti.

    ![Podrobnosti o aplikaci](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)

3. toodisplay základní spuštěné úlohy informace, hover přes graf úlohy hello. tooview hello fázích grafu a informace, které generuje každých úlohy, vyberte uzel na graf úlohy hello.

    ![Fáze podrobnosti úlohy](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. tooview často používané protokoly, jako například *ovladač Stderr*, *ovladač Stdout*, a *informací o adresáři*, vyberte hello **protokolu** karta.

    ![Podrobnosti protokolu](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)

5. Můžete také zobrazit hello Spark historie uživatelského rozhraní a hello YARN uživatelského rozhraní (na úrovni aplikace hello) tak, že vyberete odkaz hello horní části okna hello.

### <a name="access-hello-spark-history-server"></a>Přístup k serveru historie Spark hello
1. V Průzkumníku Azure rozbalte **HDInsight**, klikněte pravým tlačítkem na název clusteru Spark a pak vyberte **otevřete uživatelské rozhraní historie Spark**. 

2. Když se zobrazí výzva, zadejte přihlašovací údaje správce hello clusteru, které jste zadali při nastavení hello clusteru.

3. Na hello Spark historie řídicího panelu serveru můžete použít toolook název aplikace hello hello aplikace právě dokončila spuštění. V předchozích kód hello, nastavte název aplikace hello pomocí `val conf = new SparkConf().setAppName("MyClusterApp")`. Proto je název aplikace Spark **MyClusterApp**.

### <a name="start-hello-ambari-portal"></a>Spustit portál Ambari hello
1. V Průzkumníku Azure rozbalte **HDInsight**, klikněte pravým tlačítkem na název clusteru Spark a pak vyberte **otevřete portál pro správu clusteru (Ambari)**. 

2. Když se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru. Tyto přihlašovací údaje jste zadali během procesu instalace clusteru hello.

### <a name="manage-azure-subscriptions"></a>Spravovat předplatná Azure
Ve výchozím nastavení seznam nástrojů Azure pro IntelliJ clustery Spark hello ze všech předplatných Azure. V případě potřeby můžete zadat hello odběry, které chcete tooaccess. 

1. V Průzkumníku Azure, klikněte pravým tlačítkem na hello **Azure** kořenový uzel a potom vyberte **Spravovat odběry**. 

2. V dialogovém okně hello zrušte hello zaškrtávací políčka další toohello odběry, nemáte má tooaccess a potom vyberte **Zavřít**. Můžete také vybrat **Odhlásit** Pokud chcete toosign mimo vašeho předplatného Azure.

## <a name="run-a-spark-scala-application-locally"></a>Místní spuštění aplikace Spark Scala
Sada nástrojů Azure aplikací Spark Scala toorun IntelliJ můžete použít místně na pracovní stanici. Hello aplikace obvykle nebudete potřebovat přístup k toocluster prostředky, například kontejnery úložiště, a můžete spustit a otestovat je místně.

### <a name="prerequisite"></a>Požadavek
Když spouštíte na počítači se systémem Windows hello místních aplikací Spark Scala, může získat výjimku, jak je popsáno v [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356). protože WinUtils.exe chybí v systému Windows došlo k výjimce Hello. 

tooresolve této chybě [stáhnout hello spustitelné](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa umístění, jako **C:\WinUtils\bin**. Pak přidejte proměnnou prostředí hello **HADOOP_HOME**a nastavte hodnotu hello hello proměnné příliš**C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>Spustit místních aplikací Spark Scala
1. Spusťte IntelliJ IDEA a vytvořte projekt. 

2. V hello **nový projekt** dialogové okno pole, hello následující:
   
    a. Vyberte **HDInsight** > **Spark v HDInsight místní spuštění ukázkové (Scala)**.

    b. V hello **nástroj pro sestavení** vyberte jednu z následujících, podle potřeby tooyour hello:

      * **Maven**, podpora Průvodce vytvoření projektu Scala
      * **SBT**, pro správu hello závislosti a sestavování pro projekt hello Scala

    ![Dialogové okno Nový projekt Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run.png)

3. Vyberte **Další**.
 
4. V dalším okně hello hello následující:
   
    a. Zadejte název projektu a umístění.

    b. V hello **SDK projektu** rozevíracího seznamu vyberte verzi jazyka Java, která je novější než verze 1.7.

    c. V hello **Spark verze** rozevíracího seznamu, vyberte hello verze Scala, které chcete toouse: Scala 2.11.x Spark 2.0 nebo Scala 2.10.x pro Spark 1.6.

    ![Dialogové okno Nový projekt Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/Create-local-project.PNG)

5. Vyberte **Finish** (Dokončit).

6. Ukázkový kód přidá šablona Hello (**LogQuery**) v části hello **src** složky, kterou můžete spustit místně v počítači.
   
    ![Umístění LogQuery](./media/hdinsight-apache-spark-intellij-tool-plugin/local-app.png)

7. Klikněte pravým tlačítkem na hello **LogQuery** aplikace a pak vyberte **spustit 'LogQuery'**. Na hello **spustit** karta v dolní části hello, uvidíte výstup jako hello následující:
   
   ![Místní aplikace Spark, výsledek spuštění](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="convert-existing-intellij-idea-applications-toouse-azure-toolkit-for-intellij"></a>Převést stávající IntelliJ IDEA toouse aplikace Azure Toolkit pro IntelliJ
Hello existující Spark Scala aplikace, které jste vytvořili v IntelliJ IDEA toobe kompatibilní s Azure Toolkit pro IntelliJ můžete převést. Potom můžete hello modulu plug-in toosubmit hello aplikace tooan clusteru HDInsight Spark.

1. Pro existující Spark Scala aplikaci, která byla vytvořena prostřednictvím IntelliJ IDEA otevřete soubor přidružené .iml hello.

2. Na hello nejnižší úroveň je **modulu** element jako hello následující:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   Upravit hello element tooadd `UniqueKey="HDInsightTool"` , který hello **modulu** element vypadá hello následující:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

3. Uložte změny hello. Aplikace by teď měly být kompatibilní s Azure nástrojů pro IntelliJ. Můžete otestovat ji kliknutím pravým tlačítkem na název projektu hello v prohlížeči projektu. místní nabídky Hello má teď možnost hello **odesílání aplikací Spark tooHDInsight**.

## <a name="troubleshooting"></a>Řešení potíží

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a>Chyba při místním spuštění: *použijte prosím větší velikost haldy*
V 1.6 Spark Pokud používáte Java SDK 32-bit při místním spuštění, může dojít hello následujícím chybám:

    Exception in thread "main" java.lang.IllegalArgumentException: System memory 259522560 must be at least 4.718592E8. Please use a larger heap size.
        at org.apache.spark.memory.UnifiedMemoryManager$.getMaxMemory(UnifiedMemoryManager.scala:193)
        at org.apache.spark.memory.UnifiedMemoryManager$.apply(UnifiedMemoryManager.scala:175)
        at org.apache.spark.SparkEnv$.create(SparkEnv.scala:354)
        at org.apache.spark.SparkEnv$.createDriverEnv(SparkEnv.scala:193)
        at org.apache.spark.SparkContext.createSparkEnv(SparkContext.scala:288)
        at org.apache.spark.SparkContext.<init>(SparkContext.scala:457)
        at LogQuery$.main(LogQuery.scala:53)
        at LogQuery.main(LogQuery.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:606)
        at com.intellij.rt.execution.application.AppMain.main(AppMain.java:144)

Tyto chyby dojít, protože velikost haldy hello není dostatečně velký pro Spark toorun. Spark vyžaduje alespoň 471 MB. (Další informace najdete v tématu [SPARK 12081](https://issues.apache.org/jira/browse/SPARK-12081).) Jeden jednoduchým řešením je toouse Java SDK 64-bit. Můžete také změnit nastavení JVM hello v IntelliJ přidáním hello následující možnosti:

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Přidání možnosti toohello v IntelliJ pole "Možnosti virtuálního počítače"](./media/hdinsight-apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a>Nejčastější dotazy
Zvolte toosubmit aplikaci tooAzure Data Lake Store, **interaktivní** režimu během procesu hello Azure přihlášení. Pokud vyberete **automatizovaná** režimu, můžete dojde k chybě.

![interaktivní přihlášení](./media/hdinsight-apache-spark-intellij-tool-plugin/interative-signin.png)

Nyní jsme ho vyřešil. Můžete použít Azure Data Lake clusteru toosubmit vaší aplikace pomocí libovolné metody přihlášení.

## <a name="feedback-and-known-issues"></a>Názory a známé problémy
V současné době Spark výstupů zobrazení přímo není podporováno.

Pokud máte jakékoli návrhy či názory, nebo pokud se vyskytnou potíže při použití tento modul plug-in, e-mailu nás na adrese hdivstool@microsoft.com.

## <a name="seealso"></a>Další kroky
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="demo"></a>Ukázka
* Vytvoření projektu Scala (video): [vytvoření aplikací Spark Scala](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Vzdálené ladění (video): [pomocí nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně na clusteru HDInsight](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: provádějte interaktivní analýzy dat pomocí Spark v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: používejte Spark v HDInsight tooanalyze vytváření teploty pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight toobuild v reálném čase streamování aplikací](hdinsight-apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Vytváření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Použití nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně prostřednictvím sítě VPN](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně přes SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Použití nástrojů HDInsight v Azure nástrojů Eclipse toocreate Spark aplikací](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Správa prostředků
* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)

