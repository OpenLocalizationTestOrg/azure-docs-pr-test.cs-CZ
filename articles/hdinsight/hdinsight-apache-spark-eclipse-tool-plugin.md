---
title: "Azure nástrojů pro Eclipse – vytvoření Scala aplikací pro HDInsight Spark | Microsoft Docs"
description: "Použití nástrojů HDInsight v Azure nástrojů pro Eclipse k vývoji aplikací Spark napsané v jazyce Scala a odesílat je na clusteru HDInsight Spark, přímo z nich integrovaného vývojového prostředí Eclipse."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f6c79550-5803-4e13-b541-e86c4abb420b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: nitinme
ms.openlocfilehash: 4bcb1987a62c0b7f4965e6fd257315e820004238
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-toolkit-for-eclipse-to-create-spark-applications-for-an-hdinsight-cluster"></a>Vytvoření aplikací Spark pro cluster služby HDInsight pomocí nástrojů Azure pro Eclipse

Pomocí nástrojů HDInsight v Azure nástrojů pro Eclipse k vývoji aplikací Spark napsané v jazyce Scala a odesílat je na clusteru Azure HDInsight Spark, přímo z integrovaného vývojového prostředí Eclipse. Můžete použít nástroje HDInsight modulu plug-in několika různými způsoby:

* K vývoji a odesílání aplikací Scala Spark na clusteru HDInsight Spark
* K přístupu k prostředkům clusteru Azure HDInsight Spark
* K vývoji a místní spuštění aplikace Scala Spark

> [!IMPORTANT]
> Tento nástroj slouží k vytvoření a odeslání aplikací pouze pro cluster služby HDInsight Spark na systému Linux.
> 
> 

## <a name="prerequisites"></a>Požadavky

* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java Development Kit verze 8, který se používá pro modul runtime Eclipse IDE. Si můžete stáhnout z [Oracle webu](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* Integrované vývojové prostředí Eclipse. Tento článek používá Neónová Eclipse. Můžete nainstalovat z [Eclipse webu](https://www.eclipse.org/downloads/).   
* Spark SDK. Si můžete stáhnout z [Githubu](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).


## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-scala-plugin"></a>Instalace nástrojů HDInsight v Azure nástrojů Eclipse a modulů plug-in Scala
### <a name="install-hdinsight-tools"></a>Instalace nástrojů HDInsight
Nástroje HDInsight pro Eclipse je k dispozici jako součást nástrojů Azure pro prostředí Eclipse. Pokyny k instalaci naleznete v tématu [instalace nástrojů Azure pro Eclipse](../azure-toolkit-for-eclipse-installation.md).
### <a name="install-scala-plugin"></a>Instalace modulu plug-in Scala
Při otevření Intellij nástroje HDInsight automaticky zjistí, zda jste nainstalovali modul plug-in Scala nebo ne. Klikněte na tlačítko **OK** chcete pokračovat a postupujte podle pokynů a nainstalovat tak, že v prostředí Eclipse Marketplace.

 ![Modul plug-in Scala automatické instalace](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala.png)

## <a name="sign-in-to-your-azure-subscription"></a>Přihlaste se ke svému předplatnému Azure.
1. Spusťte Eclipse IDE a otevřete Průzkumník Azure. Na **okno** nabídky, klikněte na tlačítko **zobrazit zobrazení**a potom klikněte na **jiných**. V dialogovém okně, které se otevře, rozbalte položku **Azure**, klikněte na tlačítko **Azure Explorer**a potom klikněte na **OK**.

    ![Zobrazit dialogové okno zobrazení](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-1.png)
2. Klikněte pravým tlačítkem myši **Azure** uzel a pak klikněte na tlačítko **přihlášení**.
3. V **přihlásit k Azure** dialogové okno pole, vyberte metodu ověřování, klikněte na **přihlášení**a zadejte přihlašovací údaje Azure.
   
    ![Azure přihlašovací dialogové okno](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-2.png)
4. Poté, co jste přihlášení, **vyberte odběry** dialogové okno zobrazí všechna předplatná Azure přidružená pověření. Klikněte na tlačítko **vyberte** zavřete dialogové okno.

    ![Odběry dialogové okno Vybrat](./media/hdinsight-apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
5. Na **Průzkumník Azure** rozbalte **HDInsight** zobrazíte clusterů HDInsight Spark v rámci svého předplatného.
   
    ![Clustery HDInsight Spark v Azure Explorer](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-3.png)
6. Název uzlu clusteru zobrazíte prostředky (například účty úložiště) přidružen ke clusteru můžete dále rozšířit.
   
    ![Rozšiřování název clusteru, který najdete v části prostředky](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-4.png)



## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a>Nastavení projektu pro cluster služby HDInsight Spark Spark Scala

1. V pracovním prostoru Eclipse IDE klikněte na tlačítko **soubor**, klikněte na tlačítko **nový**a potom klikněte na **projektu**. 
2. V průvodci Nový projekt rozbalte **HDInsight**, vyberte **Spark v HDInsight (Scala)**a potom klikněte na **Další**.

    ![Výběr Spark v HDInsight (Scala) projektu](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
3. Scala projektu vytvoření průvodce automaticky zjistí, zda jste nainstalovali modul plug-in Scala nebo ne. Klikněte na tlačítko **OK** pokračovat ve stahování modulu plug-in Scala, pak postupujte podle pokynů k restartování prostředí Eclipse.

    ![Kontrola scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
4. V **nový projekt Scala HDInsight** dialogové okno, zadejte následující hodnoty a pak klikněte na tlačítko **Další**:
   * Zadejte název projektu.
   * V **prostředí JRE** oblasti, ujistěte se, že **používání spuštění prostředí JRE** je nastaven na **JavaSE 1.7** nebo novější.
   * Ujistěte se, že Spark SDK je nastavená na umístění, kam jste stáhli sadu SDK. Je součástí odkaz k umístění stahování [požadavky](#prerequisites) výše v tomto článku. Sady SDK můžete také stáhnout z odkazu součástí dialogu.

    ![Dialogové okno Nový projekt HDInsight Scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
5.  V dialogovém okně Další klikněte **knihovny** kartě a ponechejte výchozí hodnoty a pak klikněte na tlačítko **Dokončit**. 
   
    ![Karta knihovny](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-4.png)
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a>Vytvořit aplikaci pro cluster služby HDInsight Spark Scala

1. V integrovaném vývojovém prostředí Eclipse, z Průzkumníka balíčku, rozbalte projekt, který jste předtím vytvořili, klikněte pravým tlačítkem na **src**, přejděte na příkaz **nový**a potom klikněte na **jiných**.
2. V **vyberte Průvodce** dialogové okno, rozbalte seznam **Scala průvodců**, klikněte na tlačítko **Scala objekt**a potom klikněte na **Další**.
   
    ![Vyberte dialogové okno Průvodce](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
3. V **vytvořit nový soubor** dialogové okno, zadejte název pro objekt a pak klikněte na tlačítko **Dokončit**.
   
    ![Vytvořit nový soubor – dialogové okno](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
4. V textovém editoru, vložte následující kód:
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        object MyClusterApp{
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows that have only one digit in the seventh column in the CSV
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACOut")
          }        
        }
5. Spuštění aplikace na clusteru HDInsight Spark:
   
   1. V Průzkumníku balíčku, klikněte pravým tlačítkem na název projektu a potom vyberte **odesílání aplikací Spark na HDInsight**.        
   2. V **Spark odeslání** dialogové okno, zadejte následující hodnoty a pak klikněte na tlačítko **odeslání**:
      
      * Pro **název clusteru**, vyberte cluster HDInsight Spark, na kterém chcete aplikaci spustit.
      * Artefakt z projektu Eclipse, nebo vyberte jednu z pevného disku. Výchozí hodnota závisí na položku, kterou klikněte pravým tlačítkem z Průzkumníka balíčku.
      * V **hlavní název třídy** rozevírací seznam, odeslání Průvodce zobrazí všechny názvy objektů z vybraného projektu. Vyberte nebo zadejte ten, který chcete spustit. Pokud vyberete artefaktů z pevného disku, je nutné zadat název hlavní třídy sami podle. 
      * Protože kód aplikace v tomto příkladu nevyžaduje žádných argumentů příkazového řádku ani odkazovat JAR nebo soubory, můžete zbývající textová pole ponechat prázdné.
        
       ![Dialogové okno Spark odeslání](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
   3. **Spark odeslání** karta by se měl spustit zobrazení průběhu. Aplikace můžete ukončit kliknutím na červené tlačítko v **Spark odeslání** okno. Můžete také zobrazit protokoly pro tuto konkrétní aplikaci spustit kliknutím na ikonu zeměkouli (označen pole blue obrázek).
      
       ![Okno Spark odeslání](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a>Přístup a spravovat clustery HDInsight Spark pomocí nástrojů HDInsight v Azure nástrojů pro Eclipse
Můžete provádět různé operace pomocí nástroje HDInsight, včetně přístupu k výstupu úlohy.

### <a name="access-the-job-view"></a>Přístup k zobrazení úloh
1. V Průzkumníku Azure rozbalte **HDInsight**, rozbalte název clusteru Spark a pak klikněte na tlačítko **úlohy**. 

    ![Úlohy zobrazení uzlu](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. Klikněte na **úlohy** uzlu. Nástroje HDInsight automaticky zjišťuje, zda jste nainstalovali modul plug-in clipse E (fx) nebo ne. Klikněte na tlačítko **OK** pokračovat a postupujte podle pokynů k instalaci v prostředí Eclipse Marketplace a restartování prostředí Eclipse.

    ![Nainstalujte clipse E (fx)](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

3. Otevřete zobrazení úlohy **úlohy** uzlu. V pravém podokně klikněte **zobrazení úloh Spark** karta zobrazuje všechny aplikace, které byly spuštěny v clusteru. Klikněte na název aplikace, pro který chcete zobrazit další podrobnosti.

    ![Podrobnosti o aplikaci](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)
4. Pokud jste při přechodu myší na graf úlohy, zobrazuje základní informace o spuštěné úlohy. Kliknutím na graf úlohy zobrazuje graf fázích a informací, které generuje každých úlohy.

    ![Fáze podrobnosti úlohy](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

5. Často používané protokoly, včetně ovladačů Stderr, ovladač Stdout a informací o adresáři, jsou uvedeny v **protokolu** kartě.

    ![Podrobnosti protokolu](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)
6. Kliknutím na odpovídající odkaz v horní části okna můžete také otevřít historie Spark uživatelského rozhraní a uživatelské rozhraní YARN (na úrovni aplikace).

### <a name="access-the-storage-container-for-the-cluster"></a>Přístup k kontejner úložiště pro cluster
1. V Průzkumníku Azure, rozbalte **HDInsight** kořenový uzel zobrazíte seznam clustery HDInsight Spark, které jsou k dispozici.
2. Rozbalte název clusteru účet úložiště a výchozí kontejner úložiště pro cluster.
   
    ![Kontejner úložiště účet a výchozí úložiště](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-5.png)
3. Klikněte na název kontejneru úložiště, který je přidružen ke clusteru. V pravém podokně dvakrát klikněte na **HVACOut** složky. Otevřete jednu z **část -** soubory, které chcete zobrazit výstup aplikace.

### <a name="access-the-spark-history-server"></a>Přístup k serveru Spark historie
1. V Průzkumníku Azure, klikněte pravým tlačítkem na název clusteru Spark a pak vyberte **otevřete uživatelské rozhraní historie Spark**. Když se zobrazí výzva, zadejte přihlašovací údaje Správce clusteru. Musí být zadána tyto při zřizování clusteru.
2. V řídicím panelu Spark historie serveru použijte tento název aplikace a Hledat aplikace právě dokončila spuštění. V předchozím kódu nastavit název aplikace pomocí `val conf = new SparkConf().setAppName("MyClusterApp")`. Proto se název aplikace Spark **MyClusterApp**.

### <a name="start-the-ambari-portal"></a>Spuštění portálu Ambari
1. V Průzkumníku Azure, klikněte pravým tlačítkem na název clusteru Spark a pak vyberte **otevřete portál pro správu clusteru (Ambari)**. 
2. Když se zobrazí výzva, zadejte přihlašovací údaje Správce clusteru. Musí být zadána tyto při zřizování clusteru.

### <a name="manage-azure-subscriptions"></a>Spravovat předplatná Azure
Ve výchozím nastavení seznam nástrojů HDInsight v Azure nástrojů pro Eclipse clustery Spark ze všech předplatných Azure. V případě potřeby můžete zadat předplatné, pro které chcete pro přístup ke clusteru. 

1. V Průzkumníku Azure, klikněte pravým tlačítkem myši **Azure** kořenový uzel a potom klikněte na **Spravovat odběry**. 
2. V dialogovém okně, zrušte zaškrtnutí políčka pro předplatné, které nechcete, aby pro přístup a pak klikněte na tlačítko **Zavřít**. Můžete také kliknout na **Odhlásit** Pokud se chcete odhlásit z vašeho předplatného Azure.

## <a name="run-a-spark-scala-application-locally"></a>Místní spuštění aplikace Spark Scala
Nástroje HDInsight v Azure nástrojů pro Eclipse slouží ke spouštění aplikací Spark Scala místně na pracovní stanici. Obvykle se tyto aplikace nepotřebují přístup k prostředkům clusteru, například kontejner úložiště a můžete spustit a otestovat je místně.

### <a name="prerequisite"></a>Požadavek
Při místní aplikací Spark Scala spouštíte na počítači se systémem Windows, může získat výjimku, jak je popsáno v [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356). K této výjimce, protože **WinUtils.exe** chybí v systému Windows. 

Chcete-li tuto chybu vyřešit, je potřeba [stáhnout spustitelný soubor](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) do umístění, jako je **C:\WinUtils\bin**. Pak musíte přidat proměnnou prostředí **HADOOP_HOME** a nastavte hodnotu proměnné na **C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>Spustit místních aplikací Spark Scala
1. Spusťte Eclipse a vytvořte projekt. V **nový projekt** dialogové okno, vyberte následující možnosti a pak klikněte na tlačítko **Další**.
   
   * V levém podokně vyberte **HDInsight**.
   * V pravém podokně vyberte **Spark v ukázkové spustit HDInsight místní (Scala)**.

    ![Dialogové okno Nový projekt](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
2. K zadání podrobností projektu, postupujte podle kroků 3 až 6 z části starší [nastavení projektu Spark Scala pro cluster služby HDInsight Spark](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).
3. Ukázkový kód přidá šablona (**LogQuery**) v části **src** složky, kterou můžete spustit místně v počítači.
   
    ![Umístění LogQuery](./media/hdinsight-apache-spark-eclipse-tool-plugin/local-app.png)
4. Klikněte pravým tlačítkem myši **LogQuery** aplikace, přejděte na příkaz **spustit jako**a potom klikněte na **aplikace: 1 Scala**. Zobrazí se výstup jako to **konzoly** karta v dolní části:
   
   ![Místní aplikace Spark, výsledek spuštění](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="faq"></a>Nejčastější dotazy
Chcete-li odeslat aplikace do Azure Data Lake Store, zvolte **interaktivní** režimu během procesu Azure přihlášení. Pokud vyberete **automatizovaná** režimu, můžete dojde k chybě.

![interaktivní přihlášení](./media/hdinsight-apache-spark-eclipse-tool-plugin/interactive-authentication.png)

Nyní jsme ho vyřešil. Můžete použít Cluster služby Azure Data Lake odeslat vaší aplikace s libovolnou metodu přihlášení.

## <a name="feedback-and-known-issues"></a>Názory a známé problémy
V současné době Spark výstupů zobrazení přímo není podporováno.

Pokud máte jakékoli návrhy či názory, nebo pokud se vyskytnou potíže při používání tohoto nástroje, neváhejte nám odeslat e-mail na hdivstool@microsoft.com.

## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Vytváření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Použití nástrojů Azure pro IntelliJ k vytvoření a odeslání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Použití nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně prostřednictvím sítě VPN](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně přes SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Správa prostředků
* [Správa prostředků v clusteru Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)

