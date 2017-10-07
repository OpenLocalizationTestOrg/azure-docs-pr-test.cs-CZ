---
title: "sada Toolkit pro Eclipse – vytvoření Scala aplikací pro HDInsight Spark aaaAzure | Microsoft Docs"
description: "Použití nástrojů HDInsight v Azure nástrojů pro Eclipse toodevelop Spark aplikace napsané v jazyce Scala a odesílat je tooan clusteru HDInsight Spark, přímo z nich hello Eclipse IDE."
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
ms.openlocfilehash: 3ab70857c1e81f591a1c7e29bc1706ec4899ff58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-eclipse-toocreate-spark-applications-for-an-hdinsight-cluster"></a>Použití nástrojů Azure pro Eclipse toocreate aplikací Spark pro cluster služby HDInsight

Pomocí nástrojů HDInsight v Azure nástrojů pro Eclipse toodevelop Spark aplikace napsané v jazyce Scala a odesílat je cluster Azure HDInsight Spark tooan, přímo z hello Eclipse IDE. Můžete použít nástroje HDInsight hello modulu plug-in několika různými způsoby:

* toodevelop a odesílání aplikací Scala Spark na clusteru HDInsight Spark
* tooaccess vaše prostředky clusteru Azure HDInsight Spark
* toodevelop a spustit místně na aplikaci Scala Spark

> [!IMPORTANT]
> Tento nástroj může být použité toocreate a odeslání aplikací pouze pro cluster služby HDInsight Spark na systému Linux.
> 
> 

## <a name="prerequisites"></a>Požadavky

* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java Development Kit verze 8, která se používá k hello Eclipse IDE runtime. Můžete ji stáhnout z hello [Oracle webu](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* Integrované vývojové prostředí Eclipse. Tento článek používá Neónová Eclipse. Můžete ji nainstalovat ze hello [Eclipse webu](https://www.eclipse.org/downloads/).   
* Spark SDK. Si můžete stáhnout z [Githubu](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).


## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-scala-plugin"></a>Instalace nástrojů HDInsight v Azure nástrojů Eclipse a modulů plug-in Scala
### <a name="install-hdinsight-tools"></a>Instalace nástrojů HDInsight
Nástroje HDInsight pro Eclipse je k dispozici jako součást nástrojů Azure pro prostředí Eclipse. Pokyny k instalaci naleznete v tématu [instalace nástrojů Azure pro Eclipse](../azure-toolkit-for-eclipse-installation.md).
### <a name="install-scala-plugin"></a>Instalace modulu plug-in Scala
Při otevření hello Intellij hello nástroje HDInsight automaticky zjistí, zda jste nainstalovali modul plug-in Scala nebo ne. Klikněte na tlačítko **OK** toocontinue a postupujte podle hello tooinstall pokyny podle hello Eclipse Marketplace.

 ![Modul plug-in Scala automatické instalace](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala.png)

## <a name="sign-in-tooyour-azure-subscription"></a>Přihlaste se tooyour předplatného Azure
1. Spusťte hello Eclipse IDE a otevřete Průzkumník Azure. Na hello **okno** nabídky, klikněte na tlačítko **zobrazit zobrazení**a potom klikněte na **jiných**. V poli hello dialogové okno, které se otevře, rozbalte položku **Azure**, klikněte na tlačítko **Azure Explorer**a potom klikněte na **OK**.

    ![Zobrazit dialogové okno zobrazení](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-1.png)
2. Klikněte pravým tlačítkem na hello **Azure** uzel a pak klikněte na tlačítko **přihlášení**.
3. V hello **přihlásit k Azure** dialogové okno pole, vyberte metodu ověřování hello, klikněte na **přihlášení**a zadejte přihlašovací údaje Azure.
   
    ![Azure přihlašovací dialogové okno](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-2.png)
4. Poté, co jste přihlášení, hello **vyberte odběry** zobrazí dialogové okno pole všechny hello předplatná Azure přidružená pověření hello. Klikněte na tlačítko **vyberte** dialogové okno tooclose hello.

    ![Odběry dialogové okno Vybrat](./media/hdinsight-apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
5. Na hello **Azure Explorer** rozbalte **HDInsight** toosee hello clusterů HDInsight Spark v rámci svého předplatného.
   
    ![Clustery HDInsight Spark v Azure Explorer](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-3.png)
6. Můžete dále rozšířit název uzlu toosee hello prostředky clusteru (například účty úložiště) přidruženého k hello clusteru.
   
    ![Rozšiřování prostředky toosee název clusteru](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-4.png)



## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a>Nastavení projektu pro cluster služby HDInsight Spark Spark Scala

1. V pracovním prostoru hello Eclipse IDE, klikněte na tlačítko **soubor**, klikněte na tlačítko **nový**a potom klikněte na **projektu**. 
2. V Průvodci hello nového projektu, rozbalte položku **HDInsight**, vyberte **Spark v HDInsight (Scala)**a potom klikněte na **Další**.

    ![Výběr hello Spark na HDInsight (Scala) projektu](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
3. Hello Scala projektu vytvoření průvodce automaticky zjistí, zda jste nainstalovali modul plug-in Scala nebo ne. Klikněte na tlačítko **OK** toocontinue stahování hello Scala modulů plug-in a potom postupujte podle hello pokyny toorestart Eclipse.

    ![Kontrola scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
4. V hello **nový projekt Scala HDInsight** dialogové okno, zadejte následující hodnoty hello a pak klikněte na tlačítko **Další**:
   * Zadejte název projektu hello.
   * V hello **prostředí JRE** oblasti, ujistěte se, že **používání spuštění prostředí JRE** je nastaven příliš**JavaSE 1.7** nebo novější.
   * Ujistěte se, že Spark SDK je nastavení toohello umístění, kam jste stáhli hello SDK. Hello umístění stahování toohello odkaz je součástí hello [požadavky](#prerequisites) výše v tomto článku. Můžete také stáhnout hello SDK z hello odkaz zahrnuté v dialogovém okně hello.

    ![Dialogové okno Nový projekt HDInsight Scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
5.  V hello další dialogové okno, klikněte na tlačítko hello **knihovny** kartě a zachovat výchozí nastavení hello a pak klikněte na tlačítko **Dokončit**. 
   
    ![Karta knihovny](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-4.png)
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a>Vytvořit aplikaci pro cluster služby HDInsight Spark Scala

1. V hello Eclipse IDE, z Průzkumníka balíčku, rozbalte hello projekt, který jste předtím vytvořili, klikněte pravým tlačítkem na **src**, bod příliš**nový**a potom klikněte na **jiných**.
2. V hello **vyberte Průvodce** dialogové okno, rozbalte seznam **Scala průvodců**, klikněte na tlačítko **Scala objekt**a potom klikněte na **Další**.
   
    ![Vyberte dialogové okno Průvodce](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
3. V hello **vytvořit nový soubor** dialogové okno, zadejte název pro objekt hello a pak klikněte na tlačítko **Dokončit**.
   
    ![Vytvořit nový soubor – dialogové okno](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
4. Vložte následující kód v textovém editoru hello hello:
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        object MyClusterApp{
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find hello rows that have only one digit in hello seventh column in hello CSV
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACOut")
          }        
        }
5. Spuštění aplikace hello na clusteru HDInsight Spark:
   
   1. V Průzkumníku balíčku, klikněte pravým tlačítkem na název projektu hello a potom vyberte **odesílání aplikací Spark tooHDInsight**.        
   2. V hello **Spark odeslání** dialogové okno, zadejte následující hodnoty hello a pak klikněte na tlačítko **odeslání**:
      
      * Pro **název clusteru**, vyberte hello clusteru HDInsight Spark, na kterém chcete toorun vaší aplikace.
      * Vyberte artefakt z projektu Eclipse hello nebo vyberte jednu z pevného disku. Výchozí hodnota Hello závisí na hello položku, kterou klikněte pravým tlačítkem z Průzkumníka balíčku.
      * V hello **hlavní název třídy** rozevírací seznam, odeslání Průvodce zobrazí všechny názvy objektů z vybraného projektu. Vyberte nebo zadejte jednu, které chcete toorun. Pokud vyberete artefaktů z pevného disku, je nutné zadat název hlavní třídy sami podle. 
      * Protože kód aplikace hello v tomto příkladu nevyžaduje žádných argumentů příkazového řádku ani odkazovat JAR nebo soubory, můžete ponechat hello zbývající textová pole prázdná.
        
       ![Dialogové okno Spark odeslání](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
   3. Hello **Spark odeslání** karta by se měl spustit zobrazování průběhu hello. Hello aplikaci můžete ukončit kliknutím na tlačítko hello red v hello **Spark odeslání** okno. Můžete také zobrazit hello protokoly pro tuto konkrétní aplikaci spustit kliknutím na ikonu zeměkouli hello (označen hello modrá značka obrázku hello).
      
       ![Okno Spark odeslání](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a>Přístup a spravovat clustery HDInsight Spark pomocí nástrojů HDInsight v Azure nástrojů pro Eclipse
Můžete provádět různé operace pomocí nástroje HDInsight, včetně přístupu k výstup úlohy hello.

### <a name="access-hello-job-view"></a>Zobrazení úloh hello přístup
1. V Průzkumníku Azure rozbalte **HDInsight**, rozbalte název clusteru Spark hello a pak klikněte na tlačítko **úlohy**. 

    ![Úlohy zobrazení uzlu](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. Klikněte na hello **úlohy** uzlu. Nástroje HDInsight Hello automaticky zjistí zda jste nainstalovali modul plug-in clipse hello E (fx) nebo ne. Klikněte na tlačítko **OK** toocontinue a postupujte podle pokynů hello tooinstall hello Eclipse Marketplace a restartování prostředí Eclipse.

    ![Nainstalujte clipse E (fx)](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

3. Otevřete hello zobrazení úloh z hello **úlohy** uzlu. V pravém podokně hello hello **zobrazení úloh Spark** karta zobrazuje všechny hello aplikace, které byly spuštěny v clusteru hello. Klikněte na název hello hello aplikace, pro které chcete toosee další podrobnosti.

    ![Podrobnosti o aplikaci](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)
4. Pokud jste při přechodu myší na graf úlohy hello, zobrazuje základní informace o spuštěné úlohy. Kliknutím na graf úlohy hello zobrazuje graf fázích hello a informací, které generuje každých úlohy.

    ![Fáze podrobnosti úlohy](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

5. Často používané protokoly, včetně ovladačů Stderr, ovladač Stdout a informací o adresáři, jsou uvedeny v hello **protokolu** kartě.

    ![Podrobnosti protokolu](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)
6. Můžete také otevřít hello Spark historie uživatelského rozhraní a hello YARN uživatelského rozhraní (na úrovni aplikace hello) kliknutím na příslušné hypertextový odkaz hello hello horní části okna hello.

### <a name="access-hello-storage-container-for-hello-cluster"></a>Kontejner úložiště hello přístup pro hello cluster
1. V Průzkumníku Azure rozbalte hello **HDInsight** kořenový uzel toosee seznam clustery HDInsight Spark, které jsou k dispozici.
2. Rozbalte hello clusteru název účtu úložiště toosee hello a hello výchozí kontejner úložiště pro hello cluster.
   
    ![Kontejner úložiště účet a výchozí úložiště](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-5.png)
3. Klikněte na název kontejneru úložiště hello přidruženého k hello clusteru. V pravém podokně hello, klikněte dvakrát na hello **HVACOut** složky. Otevřete ho hello **část -** soubory výstup hello toosee aplikace hello.

### <a name="access-hello-spark-history-server"></a>Přístup k serveru historie Spark hello
1. V Průzkumníku Azure, klikněte pravým tlačítkem na název clusteru Spark a pak vyberte **otevřete uživatelské rozhraní historie Spark**. Když se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru. Musí být zadána tyto při zřizování clusteru hello.
2. V hello Spark historie řídicího panelu serveru použijte toolook název aplikace hello hello aplikace právě dokončila spuštění. V předchozích kód hello, nastavte název aplikace hello pomocí `val conf = new SparkConf().setAppName("MyClusterApp")`. Proto se název aplikace Spark **MyClusterApp**.

### <a name="start-hello-ambari-portal"></a>Spustit portál Ambari hello
1. V Průzkumníku Azure, klikněte pravým tlačítkem na název clusteru Spark a pak vyberte **otevřete portál pro správu clusteru (Ambari)**. 
2. Když se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru. Musí být zadána tyto při zřizování clusteru hello.

### <a name="manage-azure-subscriptions"></a>Spravovat předplatná Azure
Ve výchozím nastavení seznam nástrojů HDInsight v Azure nástrojů pro Eclipse clustery Spark hello ze všech předplatných Azure. V případě potřeby můžete zadat hello odběry, pro které chcete tooaccess hello clusteru. 

1. V Průzkumníku Azure, klikněte pravým tlačítkem na hello **Azure** kořenový uzel a potom klikněte na **Spravovat odběry**. 
2. V dialogovém okně hello, zrušte zaškrtnutí políček hello hello předplatného, nemáte má tooaccess a pak klikněte na tlačítko **Zavřít**. Můžete také kliknout na **Odhlásit** Pokud chcete toosign mimo vašeho předplatného Azure.

## <a name="run-a-spark-scala-application-locally"></a>Místní spuštění aplikace Spark Scala
Můžete použít nástroje HDInsight v Azure nástrojů aplikací Spark Scala toorun Eclipse místně na pracovní stanici. Obvykle se tyto aplikace nepotřebují přístup k prostředkům toocluster například kontejner úložiště a můžete spustit a otestovat je místně.

### <a name="prerequisite"></a>Požadavek
Když spouštíte na počítači se systémem Windows hello místních aplikací Spark Scala, jak je popsáno v může získat výjimku [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356). K této výjimce, protože **WinUtils.exe** chybí v systému Windows. 

tooresolve tato chyba, je nutné [stáhnout hello spustitelné](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa umístění jako **C:\WinUtils\bin**. Pak musíte přidat proměnnou prostředí hello **HADOOP_HOME** a nastavte hodnotu hello hello proměnné příliš**C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>Spustit místních aplikací Spark Scala
1. Spusťte Eclipse a vytvořte projekt. V hello **nový projekt** dialogové okno, ujistěte se, hello následující možnosti a pak klikněte na tlačítko **Další**.
   
   * V levém podokně hello vyberte **HDInsight**.
   * V pravém podokně hello vyberte **Spark v ukázkové spustit HDInsight místní (Scala)**.

    ![Dialogové okno Nový projekt](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
2. hello podrobností projektu tooprovide hello, postupujte podle kroků 3 až 6 z výše uvedené části [nastavení projektu Spark Scala pro cluster služby HDInsight Spark](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).
3. Ukázkový kód přidá šablona Hello (**LogQuery**) v části hello **src** složky, kterou můžete spustit místně v počítači.
   
    ![Umístění LogQuery](./media/hdinsight-apache-spark-eclipse-tool-plugin/local-app.png)
4. Klikněte pravým tlačítkem na hello **LogQuery** aplikace, bod příliš**spustit jako**a potom klikněte na **aplikace: 1 Scala**. Zobrazí se výstup takto v hello **konzoly** kartě dolnímu hello:
   
   ![Místní aplikace Spark, výsledek spuštění](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="faq"></a>Nejčastější dotazy
Zvolte toosubmit aplikaci tooAzure Data Lake Store, **interaktivní** režimu během procesu hello Azure přihlášení. Pokud vyberete **automatizovaná** režimu, můžete dojde k chybě.

![interaktivní přihlášení](./media/hdinsight-apache-spark-eclipse-tool-plugin/interactive-authentication.png)

Nyní jsme ho vyřešil. Můžete použít Azure Data Lake clusteru toosubmit vaší aplikace pomocí libovolné metody přihlášení.

## <a name="feedback-and-known-issues"></a>Názory a známé problémy
V současné době Spark výstupů zobrazení přímo není podporováno.

Pokud máte jakékoli návrhy či názory, nebo pokud se vyskytnou potíže při používání tohoto nástroje, myslíte, že volné toosend nám e-mail na hdivstool@microsoft.com.

## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Vytváření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Použijte sadu nástrojů Azure pro IntelliJ toocreate a odesílání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Použití nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně prostřednictvím sítě VPN](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně přes SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Správa prostředků
* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)

