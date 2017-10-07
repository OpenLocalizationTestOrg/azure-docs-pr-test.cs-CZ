---
title: "Azure nástrojů pro IntelliJ: ladění Spark aplikací vzdáleně přes SSH | Microsoft Docs"
description: "Podrobný návod jak clusterů toouse nástroje HDInsight v Azure nástrojů pro IntelliJ toodebug aplikace vzdáleně v HDInsight pomocí SSH"
keywords: "vzdálené ladění intellij, vzdálené ladění intellij, ssh, intellij, hdinsight, ladění intellij, ladění"
services: hdinsight
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 08/24/2017
ms.author: Jenny Jiang
ms.openlocfilehash: bf3ab9d04c2ff9fcb6bbbdeefb11f55a12fbd845
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a>Ladění aplikací Spark v clusteru s Azure nástrojů HDInsight pro IntelliJ prostřednictvím SSH

Tento článek obsahuje podrobné pokyny o tom, toouse nástroje HDInsight v Azure nástrojů pro IntelliJ toodebug aplikace vzdáleně v clusteru HDInsight. toodebug projektu, můžete také zobrazit hello [HDInsight Spark ladění aplikace s Azure Toolkit pro IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) videa.

**Požadavky**

* **Nástroje HDInsight v Azure nástrojů pro IntelliJ**. Tento nástroj je součástí sady nástrojů Azure pro IntelliJ. Další informace najdete v tématu [instalaci nástrojů Azure pro IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).
* **Azure nástrojů pro IntelliJ**. Tato sada nástrojů toocreate Spark aplikace použijte pro cluster služby HDInsight. Další informace, postupujte podle pokynů hello v [pomocí nástrojů Azure pro IntelliJ toocreate aplikací Spark pro cluster služby HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).
* **HDInsight SSH služby pomocí uživatelského jména a hesla správu**. Další informace najdete v tématu [připojit tooHDInsight (Hadoop) pomocí protokolu SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) a [použití SSH tunelu tooaccess Ambari webové uživatelské rozhraní, JobHistory, NameNode, Oozie a jiné webové uživatelská](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel). 
 

## <a name="create-a-spark-scala-application-and-configure-it-for-remote-debugging"></a>Vytvoření aplikací Spark Scala a nakonfigurovat ji pro vzdálené ladění

1. Spusťte IntelliJ IDEA a pak vytvořte projekt. V hello **nový projekt** dialogové okno pole, hello následující:

   a. Vyberte **HDInsight**. 

   b. Vyberte Java nebo Scala šablony založené na vaši volbu. Vyberte mezi hello následující možnosti:

      - **Spark v HDInsight (Scala)**

      - **Spark v HDInsight (Java)**

      - **Spark v clusteru HDInsight spuštění ukázkové (Scala)**

      Tento příklad používá **Spark v HDInsight clusteru spustit ukázkový (Scala)** šablony.

   c. V hello **nástroj pro sestavení** vyberte jednu z následujících, podle potřeby tooyour hello:

      - **Maven**, podpora Průvodce vytvoření projektu Scala

      -  **SBT**, pro správu hello závislosti a sestavování pro projekt hello Scala 

      ![Vytvoření projektu ladění](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-create-projectfor-debug-remotely.png)

   d. Vyberte **Další**.     
 
3. V hello Další **nový projekt** okně hello následující:

   ![Vyberte hello Spark SDK](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-new-project.png)

   a. Zadejte název projektu a umístění projektu.

   b. V hello **SDK projektu** rozevíracího seznamu vyberte **Java 1.8** pro **Spark 2.x** clusteru nebo vyberte **Java 1.7** pro **Spark 1. x** clusteru.

   c. V hello **Spark verze** rozevíracího seznamu, Průvodce vytvořením projektu Scala hello integruje hello správná verze pro sadu SDK Spark a Scala SDK. Pokud verze clusteru spark hello je starší než 2.0, vyberte **Spark 1.x**. Jinak vyberte možnost **Spark 2.x.** Tento příklad používá **Spark bodu 2.0.2 (Scala 2.11.8)**.

   d. Vyberte **Finish** (Dokončit).

4. Vyberte **src** > **hlavní** > **scala** tooopen kódu v projektu hello. Tento příklad používá hello **SparkCore_wasbloTest** skriptu.

5. tooaccess hello **upravit konfigurace** nabídky, vyberte hello ikonu v pravém horním rohu hello. Z této nabídky můžete vytvořit nebo upravit hello konfigurace pro vzdálené ladění.

   ![Úprava konfigurací](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-edit-configurations.png) 

6. V hello **konfigurace spustit/Debug** dialogové okno, vyberte hello – znaménko plus (**+**). Potom vyberte hello **odeslat úlohu Spark** možnost.

   ![Přidat novou konfiguraci](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-add-new-Configuration.png)
7. Zadejte informace pro **název**, **clusteru Spark**, a **hlavní název třídy**. Potom vyberte **pokročilou konfiguraci**. 

   ![Spuštění konfigurace ladění](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-run-debug-configurations.png)

8. V hello **Spark odeslání Upřesnit konfiguraci** dialogové okno, vyberte **Spark povolení vzdáleného ladění**. Zadejte uživatelské jméno SSH hello a pak zadejte heslo nebo použít soubor privátního klíče. toosave hello konfiguraci, vyberte **OK**.

   ![Povolení vzdáleného ladění Spark](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-enable-spark-remote-debug.png)

9. Konfigurace Hello jsou nyní uloženy hello názvem, který jste zadali. tooview hello podrobnosti o konfiguraci, vyberte hello název konfigurace. Vyberte změny toomake **upravit konfigurace**. 

10. Po dokončení konfigurace nastavení hello můžete spustit projekt hello oproti vzdáleném clusteru hello nebo provést vzdálené ladění.

## <a name="learn-how-tooperform-remote-debugging"></a>Zjistěte, jak tooperform vzdálené ladění
### <a name="scenario-1-perform-remote-run"></a>Scénář 1: Provést vzdálené spuštění

V této části Ukážeme vám jak toodebug ovladače a vykonavatelů.

    import org.apache.spark.{SparkConf, SparkContext}

    object LogQuery {
      val exampleApacheLogs = List(
        """10.10.10.10 - "FRED" [18/Jan/2013:17:56:07 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 315 "http://referall.com/" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.350 "-" - "" 265 923 934 ""
          | 62.24.11.25 images.com 1358492167 - Whatup""".stripMargin.lines.mkString,
        """10.10.10.10 - "FRED" [18/Jan/2013:18:02:37 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 306 "http:/referall.com" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.352 "-" - "" 256 977 988 ""
          | 0 73.23.2.15 images.com 1358492557 - Whatup""".stripMargin.lines.mkString
      )
      def main(args: Array[String]) {
        val sparkconf = new SparkConf().setAppName("Log Query")
        val sc = new SparkContext(sparkconf)
        val dataSet = sc.parallelize(exampleApacheLogs)
        // scalastyle:off
        val apacheLogRegex =
          """^([\d.]+) (\S+) (\S+) \[([\w\d:/]+\s[+\-]\d{4})\] "(.+?)" (\d{3}) ([\d\-]+) "([^"]+)" "([^"]+)".*""".r
        // scalastyle:on
        /** Tracks hello total query count and number of aggregate bytes for a particular group. */
        class Stats(val count: Int, val numBytes: Int) extends Serializable {
          def merge(other: Stats): Stats = new Stats(count + other.count, numBytes + other.numBytes)
          override def toString: String = "bytes=%s\tn=%s".format(numBytes, count)
        }
        def extractKey(line: String): (String, String, String) = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              if (user != "\"-\"") (ip, user, query)
              else (null, null, null)
            case _ => (null, null, null)
          }
        }
        def extractStats(line: String): Stats = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              new Stats(1, bytes.toInt)
            case _ => new Stats(1, 0)
          }
        }
        
        dataSet.map(line => (extractKey(line), extractStats(line)))
          .reduceByKey((a, b) => a.merge(b))
          .collect().foreach{
          case (user, query) => println("%s\t%s".format(user, query))}

        sc.stop()
      }
    }


1. Nastavit body ukončování řádků a potom vyberte hello **ladění** ikonu.

   ![Vyberte ikonu pro ladění hello](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-icon.png)

2. Při spuštění programu hello dosáhne hello nejnovější bod, uvidíte **ovladač** kartě a dvě **vykonavatele** karty v hello **ladicí program** podokně. Vyberte hello **obnovit Program** toocontinue ikona spuštění hello kódu, který pak dosáhne hello další zarážek a se zaměřuje na odpovídající hello **vykonavatele** kartě. Můžete zkontrolovat hello protokoly spouštění na odpovídající hello **konzoly** kartě.

   ![Karta ladění](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debugger-tab.png)

### <a name="scenario-2-perform-remote-debugging-and-bug-fixing"></a>Scénář 2: Proveďte vzdálené ladění a opravy chyb
V této části Ukážeme vám jak toodynamically aktualizace hello hodnota proměnné pomocí hello IntelliJ ladění funkce pro jednoduché opravu. V hello následující ukázka kódu je vyvolána výjimka, protože hello cílový soubor již existuje.
  
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext

        object SparkCore_WasbIOTest {
          def main(arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkCore_WasbIOTest")
            val sc = new SparkContext(conf)
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            // Find hello rows that have only one digit in hello sixth column.
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)

            try {
              var target = "wasb:///HVACout2_testdebug1";
              rdd1.saveAsTextFile(target);
            } catch {
              case ex: Exception => {
                throw ex;
              }
            }
          }
        }


#### <a name="tooperform-remote-debugging-and-bug-fixing"></a>vzdálené ladění tooperform a opravy chyb
1. Nastavit dva body ukončování řádků a potom vyberte hello **ladění** ikonu toostart hello vzdálené ladění procesu.

2. Kód Hello zastaví narušující okamžiku první hello a parametr hello a informace o proměnných jsou uvedeny v hello **proměnné** podokně. 

3. Vyberte hello **obnovit Program** toocontinue ikonu. Kód Hello zastaví na druhý bod hello. podle očekávání, je byla zachycena výjimka Hello.

  ![Throw – chyba](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-throw-error.png) 

4. Vyberte hello **obnovit Program** ikonu znovu. Hello **HDInsight Spark odeslání** okně se zobrazí chyba "Úloha spuštění se nepovedlo".

  ![Chyba odeslání](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-error-submission.png) 

5. Vyberte toodynamically aktualizace hello hodnota proměnné pomocí funkce ladění hello IntelliJ, **ladění** znovu. Hello **proměnné** podokně se zobrazí znovu. 

6. Cíl hello klikněte pravým tlačítkem na hello **ladění** a pak vyberte **nastavit hodnotu**. Potom zadejte novou hodnotu pro proměnnou hello. Potom vyberte **Enter** toosave hello hodnotu. 

  ![Nastavte hodnotu](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-set-value.png) 

7. Vyberte hello **obnovit Program** ikonu toocontinue toorun hello programu. Tentokrát je žádná výjimka zachycena. Uvidíte, že tento projekt hello funguje úspěšně bez jakékoli výjimky.

  ![Ladění bez výjimky](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-without-exception.png)

## <a name="seealso"></a>Další kroky
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="demo"></a>Ukázka
* Vytvoření projektu Scala (video): [vytvoření aplikací Spark Scala](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Vzdálené ladění (video): [pomocí nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně v clusteru HDInsight](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: provádějte interaktivní analýzy dat pomocí Spark v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: používejte Spark v HDInsight tooanalyze vytváření teploty pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight toobuild v reálném čase streamování aplikací](hdinsight-apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Vytvoření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Použití nástrojů Azure pro IntelliJ toocreate aplikací Spark pro cluster služby HDInsight](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Použití nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně prostřednictvím sítě VPN](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Použití nástrojů HDInsight v Azure nástrojů Eclipse toocreate Spark aplikací](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru hello Spark pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
