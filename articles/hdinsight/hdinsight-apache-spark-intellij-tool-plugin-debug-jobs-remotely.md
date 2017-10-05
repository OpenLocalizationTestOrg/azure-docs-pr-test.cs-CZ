---
title: "Azure nástrojů pro IntelliJ - ladění aplikace vzdáleně na HDInsight Spark | Microsoft Docs"
description: "Další informace o použití nástrojů HDInsight v Azure nástrojů pro IntelliJ pro vzdálené ladění aplikací běžících na HDInsight Spark clusterů prostřednictvím sítě vpn."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 55fb454f-c7dc-46de-a978-e242e9a94f4c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 5ce282aac94d0f22ea587cbe4005819310e23b1f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-toolkit-for-intellij-to-debug-applications-remotely-on-hdinsight-spark-through-vpn"></a>Použití nástrojů Azure pro IntelliJ k ladění aplikací vzdáleně na HDInsight Spark prostřednictvím sítě VPN

Doporučujeme, abyste způsob ladění spark applicaltion vzdáleně přes ssh. Pokyny najdete v tématu [vzdálené ladění aplikací Spark v clusteru HDInsight pomocí sady Azure Toolkit pro IntelliJ prostřednictvím SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).

Tento článek obsahuje podrobné pokyny o tom, jak používat nástroje HDInsight v Azure nástrojů pro IntelliJ odeslat úlohu Spark v HDInsight Spark clusteru a pak ho vzdáleně ladit ze stolního počítače. Chcete-li to provést, musíte provést následující postup vysoké úrovně:

1. Vytvoření site-to-site nebo point-to-site virtuální sítě Azure. Kroky v tomto dokumentu předpokládají, že používáte síť site-to-site.
2. Vytvořte Spark cluster v Azure HDInsight, který je součástí virtuální sítě Azure site-to-site.
3. Zkontrolujte připojení mezi headnode clusteru a pracovní ploše.
4. Vytvoření aplikace Scala v IntelliJ IDEA a nakonfigurovat ji pro vzdálené ladění.
5. Spuštění a ladění aplikace.

## <a name="prerequisites"></a>Požadavky
* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Java Development kit Oracle. Můžete nainstalovat z [zde](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* IntelliJ IDEA. Tento článek používá verzi 2017.1. Můžete nainstalovat z [zde](https://www.jetbrains.com/idea/download/).
* Nástroje HDInsight v Azure nástrojů pro IntelliJ. Nástroje HDInsight pro IntelliJ jsou k dispozici jako součást sady nástrojů Azure pro IntelliJ. Pokyny k instalaci sady nástrojů Azure najdete v tématu [instalace sady Toolkit Azure pro IntelliJ](../azure-toolkit-for-intellij-installation.md).
* Přihlaste se k předplatnému Azure z IntelliJ IDEA. Postupujte podle pokynů [zde](hdinsight-apache-spark-intellij-tool-plugin.md).
* Při spouštění aplikací Spark Scala pro vzdálené ladění na počítači se systémem Windows, může získat výjimku, jak je popsáno v [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356) k tomu dojde z důvodu chybějící WinUtils.exe v systému Windows. Chcete-li tuto chybu vyřešit, je potřeba [spustitelný soubor stáhnout odsud](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) do umístění, jako je **C:\WinUtils\bin**. Pak musíte přidat proměnnou prostředí **HADOOP_HOME** a nastavte hodnotu proměnné na **C\WinUtils**.

## <a name="step-1-create-an-azure-virtual-network"></a>Krok 1: Vytvoření virtuální sítě Azure
Postupujte podle pokynů z následujících odkazů můžete vytvořit virtuální síť Azure a pak ověřte připojení mezi desktopem a virtuální síti Azure.

* [Vytvoření virtuální sítě s připojením VPN typu site-to-site pomocí portálu Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [Vytvoření virtuální sítě s připojením VPN typu site-to-site pomocí prostředí PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [Konfigurace připojení typu point-to-site k virtuální síti pomocí prostředí PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="step-2-create-an-hdinsight-spark-cluster"></a>Krok 2: Vytvoření clusteru HDInsight Spark
Měli byste také vytvořit cluster Apache Spark v Azure HDInsight, který je součástí virtuální sítě Azure, kterou jste vytvořili. Použijte informace k dispozici na [vytvořit systémem Linux clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Jako součást volitelné konfigurace vyberte virtuální síť Azure, kterou jste vytvořili v předchozím kroku.

## <a name="step-3-verify-the-connectivity-between-the-cluster-headnode-and-your-desktop"></a>Krok 3: Ověření připojení mezi headnode clusteru a pracovní plochy
1. Získáte IP adresu headnode. Otevřete uživatelské rozhraní Ambari pro cluster. V okně clusteru, klikněte na tlačítko **řídicí panel**.

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/launch-ambari-ui.png)
2. V uživatelském rozhraní Ambari, v pravém horním rohu, klikněte na **hostitele**.

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/ambari-hosts.png)
3. Zobrazí seznam headnodes, pracovní uzly a uzly zookeeper. Máte headnodes **hn*** předponu. Klikněte na první headnode.

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/cluster-headnodes.png)
4. V dolní části stránky, které se otevře z **Souhrn** pole, zkopírovat IP adresu headnode a název hostitele.

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/headnode-ip-address.png)
5. Zahrnují IP adresu a název hostitele headnode k **hostitele** soubor v počítači ze kterých chcete spustit a vzdáleně ladit úlohy Spark. To vám umožní komunikovat s headnode používat IP adresu, stejně jako název hostitele.

   1. Otevřete Poznámkový blok se zvýšenými oprávněními. V nabídce Soubor klikněte na tlačítko **otevřete** a pak přejděte do umístění souboru hostitelů. Na počítači s Windows, je `C:\Windows\System32\Drivers\etc\hosts`.
   2. Přidejte následující **hostitele** souboru.

           # For headnode0
           192.xxx.xx.xx hn0-nitinp
           192.xxx.xx.xx hn0-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net

           # For headnode1
           192.xxx.xx.xx hn1-nitinp
           192.xxx.xx.xx hn1-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
6. Z počítače, který jste připojeni k virtuální síti Azure, který je používán clusteru HDInsight ověřte, že může odeslat příkaz ping i headnodes používat IP adresu, stejně jako název hostitele.
7. SSH do clusteru headnode, postupujte podle pokynů v [připojit ke clusteru HDInsight pomocí protokolu SSH](hdinsight-hadoop-linux-use-ssh-unix.md). Z clusteru headnode příkazem ping otestovat adresu IP stolní počítač. Měli byste otestovat připojení k obě IP adresy přiřazené do počítače, jeden pro síťové připojení a druhou pro virtuální síť Azure, která je počítač připojen k.
8. Opakujte kroky pro ostatní headnode také.

## <a name="step-4-create-a-spark-scala-application-using-the-hdinsight-tools-in-azure-toolkit-for-intellij-and-configure-it-for-remote-debugging"></a>Krok 4: Vytvoření Spark Scala aplikace pomocí nástrojů HDInsight pro IntelliJ v nástrojů Azure a nakonfigurovat ji pro vzdálené ladění
1. Spusťte IntelliJ IDEA a vytvoření nového projektu. V dialogovém okně Nový projekt vyberte následující možnosti a pak klikněte na tlačítko **Další**.

    ![Vytvoření aplikací Spark Scala](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-hdi-scala-app.png)

   * V levém podokně vyberte **HDInsight**.
   * V pravém podokně vyberte **Spark v HDInsight (Scala)**.
   * Klikněte na **Další**.
2. V okně Další zadejte následující podrobnosti projektu a pak klikněte na tlačítko **Dokončit**.  
   - Zadejte název projektu a umístění projektu.
   - Pro **SDK projektu**, použijte Java 1.8 pro cluster spark 2.x, Java 1.7 pro 1.x clusteru spark.
   - Pro **Spark verze**, Průvodce vytvořením projektu Scala integruje správnou verzi sady SDK Spark a Scala SDK. Pokud je verze clusteru spark nižší 2.0, zvolte spark 1.x. Jinak měli byste vybrat spark2.x. Tento příklad používá Spark2.0.2 (Scala 2.11.8).
       ![Vytvoření aplikací Spark Scala](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-scala-project-details.png)
  
3. Projekt Spark automaticky vytvoří artefakt. Informace o artefaktu, postupujte podle těchto kroků.

   1. Z **soubor** nabídky, klikněte na tlačítko **strukturu projektu**.
   2. V **strukturu projektu** dialogové okno, klikněte na tlačítko **artefakty** zobrazíte výchozí artefaktu, který je vytvořen.
   ![Vytvoření JAR](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/default-artifact.png)

      Můžete také vytvořit vlastní artefaktů bly kliknutím na  **+**  ikonu, vyznačené na obrázku výše.

4. Do projektu přidejte knihovny. Pokud chcete přidat do knihovny, klikněte pravým tlačítkem na název projektu ve stromové struktuře projektu a pak klikněte na tlačítko **otevřete nastavení modulu**. V **strukturu projektu** dialogové okno, v levém podokně klikněte na tlačítko **knihovny**, klikněte na tlačítko (+) symbolů a potom klikněte na **z Maven**.

    ![Přidání knihovny](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/add-library.png)

    V **knihovny stáhnout z úložiště Maven** dialogové okno, vyhledávání a přidejte následující knihovny.

   * `org.scalatest:scalatest_2.10:2.2.1`
   * `org.apache.hadoop:hadoop-azure:2.7.1`
5. Kopírování `yarn-site.xml` a `core-site.xml` z headnode clusteru a přidejte ji do projektu. Použijte následující příkazy pro kopírování souborů. Můžete použít [emulaci](https://cygwin.com/install.html) spustit následující `scp` příkazy pro kopírování souborů z headnodes clusteru.

        scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .

    Protože jsme již přidána clusteru headnode IP adresy a názvy hostitelů fo souboru hostitelů na ploše, můžeme použít **spojovací bod služby** příkazy následujícím způsobem.

        scp sshuser@hn0-nitinp:/etc/hadoop/conf/core-site.xml .
        scp sshuser@hn0-nitinp:/etc/hadoop/conf/yarn-site.xml .

    Do projektu přidejte tyto soubory tak, že je v části zkopírujete **/src** složky ve stromu vašeho projektu, například `<your project directory>\src`.
6. Aktualizace `core-site.xml` provést následující změny.

   1. `core-site.xml`obsahuje šifrovaný klíč k účtu úložiště, který je přidružen ke clusteru. V `core-site.xml` , že jste přidali do projektu, nahraďte skutečným úložiště klíč zašifrovaný klíč přidružený výchozí účet úložiště. V tématu [Správa přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).

           <property>
                 <name>fs.azure.account.key.hdistoragecentral.blob.core.windows.net</name>
                 <value>access-key-associated-with-the-account</value>
           </property>
   2. Odebrat následující položky z `core-site.xml`.

           <property>
                 <name>fs.azure.account.keyprovider.hdistoragecentral.blob.core.windows.net</name>
                 <value>org.apache.hadoop.fs.azure.ShellDecryptionKeyProvider</value>
           </property>

           <property>
                 <name>fs.azure.shellkeyprovider.script</name>
                 <value>/usr/lib/python2.7/dist-packages/hdinsight_common/decrypt.sh</value>
           </property>

           <property>
                 <name>net.topology.script.file.name</name>
                 <value>/etc/hadoop/conf/topology_script.py</value>
           </property>
   3. Uložte soubor.
7. Přidání třídy hlavní pro vaši aplikaci. Z **Project Exploreru**, klikněte pravým tlačítkem na **src**, přejděte na **nový**a potom klikněte na **Scala třída**.

    ![Přidejte zdrojový kód](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code.png)
8. V **vytvořte novou třídu Scala** dialogovém okně zadejte název, **druhu** vyberte **objekt**a potom klikněte na **OK**.

    ![Přidejte zdrojový kód](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code-object.png)
9. V `MyClusterAppMain.scala` souboru, vložte následující kód. Tento kód vytvoří Spark kontextu a spustí `executeJob` metoda z `SparkSample` objektu.

        import org.apache.spark.{SparkConf, SparkContext}

        object SparkSampleMain {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkSample")
                                      .set("spark.hadoop.validateOutputSpecs", "false")
            val sc = new SparkContext(conf)

            SparkSample.executeJob(sc,
                                   "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
                                   "wasb:///HVACOut")
          }
        }

10. Opakujte kroky 8 a 9 výše a přidejte nový objekt Scala názvem `SparkSample`. Pro tuto třídu přidejte následující kód. Tento kód čte data z HVAC.csv (k dispozici na všech clusterech HDInsight Spark), načte řádky, které mají pouze jednu číslici ve sloupci sedmého ve sdíleném svazku clusteru a zapíše výstup do **/HVACOut** v kontejneru výchozí úložiště pro cluster.

        import org.apache.spark.SparkContext

        object SparkSample {
         def executeJob (sc: SparkContext, input: String, output: String): Unit = {
           val rdd = sc.textFile(input)

           //find the rows which have only one digit in the 7th column in the CSV
           val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

           val s = sc.parallelize(rdd.take(5)).cartesian(rdd).count()
           println(s)

           rdd1.saveAsTextFile(output)
           //rdd1.collect().foreach(println)
         }
        }
11. Opakujte kroky 8 a 9 výše a přidejte novou třídu s názvem `RemoteClusterDebugging`. Tato třída implementuje rozhraní Spark test, který slouží k ladění aplikací. Přidejte následující kód, který `RemoteClusterDebugging` třídy.

        import org.apache.spark.{SparkConf, SparkContext}
        import org.scalatest.FunSuite

        class RemoteClusterDebugging extends FunSuite {

         test("Remote run") {
           val conf = new SparkConf().setAppName("SparkSample")
                                     .setMaster("yarn-client")
                                     .set("spark.yarn.am.extraJavaOptions", "-Dhdp.version=2.4")
                                     .set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")
                                     .setJars(Seq("""C:\workspace\IdeaProjects\MyClusterApp\out\artifacts\MyClusterApp_DefaultArtifact\default_artifact.jar"""))
                                     .set("spark.hadoop.validateOutputSpecs", "false")
           val sc = new SparkContext(conf)

           SparkSample.executeJob(sc,
             "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
             "wasb:///HVACOut")
         }
        }

     Několik důležitých věcí si zde:

   * Pro `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, zkontrolujte, zda je sestavení Spark JAR k dispozici v úložišti clusteru v zadané cestě.
   * Pro `setJars`, zadejte umístění, kde bude vytvořen jar artefaktů. Obvykle je `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.
12. V `RemoteClusterDebugging` třídy, klikněte pravým tlačítkem myši `test` – klíčové slovo a vyberte **vytvořit konfigurace RemoteClusterDebugging**.

    ![Vytvoření konfigurace vzdáleného](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-remote-config.png)

13. V dialogovém okně zadejte název pro konfiguraci a vyberte **testovací druh** jako **název testovacího**. Nechat všechny ostatní hodnoty jako výchozí, klikněte na **použít**a potom klikněte na **OK**.

    ![Vytvoření konfigurace vzdáleného](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/provide-config-value.png)
14. Teď byste měli vidět **vzdálené spuštění** konfigurace rozevírací seznam v panelu nabídek.

    ![Vytvoření konfigurace vzdáleného](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/config-run.png)

## <a name="step-5-run-the-application-in-debug-mode"></a>Krok 5: Spuštění aplikace v režimu ladění
1. Otevřete v projektu IntelliJ IDEA `SparkSample.scala` a vytvořit zarážku vedle 'val rdd1'. V nabídce automaticky otevírané okno pro vytvoření zarážku vyberte **řádku funkce executeJob**.

    ![Přidat zarážky](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-breakpoint.png)
2. Klikněte na tlačítko **ladění spustit** vedle položky **vzdálené spuštění** konfigurace rozevíracího seznamu spustit aplikace.

    ![Spusťte program v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-run-mode.png)
3. Při spuštění programu dosáhne zarážky, měli byste vidět **ladicí program** karty v dolním podokně.

    ![Spusťte program v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch.png)
4. Klikněte na (**+**) ikonu, čímž přidáte sledovat, jak je znázorněno na obrázku níže.

    ![Spusťte program v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable.png)

    Sem, protože aplikace překročila před proměnnou `rdd1` byla vytvořena pomocí tohoto sledování, uvidíte, jaké jsou prvních 5 řádky v proměnné `rdd`. Stiskněte **ENTER**.

    ![Spusťte program v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable-value.png)

    Co se zobrazí v na obrázku výše je, že v době běhu můžete dotazovat terrabytes dat a ladění jak postupuje vaší aplikace. Ve výstupu znázorněno na obrázku výše, například uvidíte, že první řádek výstupu je záhlaví. Na základě, můžete upravit kód aplikace tak, aby přeskočil řádek záhlaví, pokud je to nutné.
5. Nyní můžete kliknout na **obnovit Program** ikonu pokračujte aplikace spustit.

    ![Spusťte program v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-continue-run.png)
6. Pokud se aplikace úspěšně dokončí, měli byste vidět výstup jako následující.

    ![Spusťte program v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-complete.png)

## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="demo"></a>Ukázka
* Vytvoření projektu Scala (Video): [vytvoření aplikací Spark Scala](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Vzdálené ladění (Video): [pomocí nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně na clusteru HDInsight](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Vytvoření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Pomocí nástrojů HDInsight v Azure nástrojů pro IntelliJ můžete vytvořit a odesílání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Použití nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně přes SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Používat nástroje HDInsight pro vytvoření aplikací Spark v Azure nástrojů pro Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků v clusteru Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
