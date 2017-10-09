---
title: "sada Toolkit pro IntelliJ - ladění aplikace vzdáleně na HDInsight Spark aaaAzure | Microsoft Docs"
description: "Další informace o použití nástrojů HDInsight v Azure nástrojů IntelliJ tooremotely ladění aplikací spuštěných na clustery HDInsight Spark prostřednictvím sítě vpn."
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
ms.openlocfilehash: ad67d23bf609d0a7afb38b2acb110062f8b27b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-intellij-toodebug-applications-remotely-on-hdinsight-spark-through-vpn"></a>Použijte sadu nástrojů Azure pro aplikace toodebug IntelliJ vzdáleně na HDInsight Spark prostřednictvím sítě VPN

Doporučujeme, abyste hello způsob ladění spark applicaltion vzdáleně přes ssh. Pokyny najdete v tématu [vzdálené ladění aplikací Spark v clusteru HDInsight pomocí sady Azure Toolkit pro IntelliJ prostřednictvím SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).

Tento článek obsahuje podrobné pokyny k jak toouse hello nástroje HDInsight v Azure nástrojů pro IntelliJ toosubmit úlohy Spark v HDInsight Spark clusteru a pak ho vzdáleně ladit ze stolního počítače. toodo Ano, je třeba provést následující postup vysoké úrovně hello:

1. Vytvoření site-to-site nebo point-to-site virtuální sítě Azure. Hello kroky v tomto dokumentu předpokládají, že používáte síť site-to-site.
2. Vytvořte Spark cluster v Azure HDInsight, který je součástí hello site-to-site virtuální síť Azure.
3. Ověřte připojení hello mezi headnode hello clusteru a pracovní ploše.
4. Vytvoření aplikace Scala v IntelliJ IDEA a nakonfigurovat ji pro vzdálené ladění.
5. Spuštění a ladění aplikace hello.

## <a name="prerequisites"></a>Požadavky
* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Java Development kit Oracle. Můžete nainstalovat z [zde](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* IntelliJ IDEA. Tento článek používá verzi 2017.1. Můžete nainstalovat z [zde](https://www.jetbrains.com/idea/download/).
* Nástroje HDInsight v Azure nástrojů pro IntelliJ. Nástroje HDInsight pro IntelliJ jsou k dispozici jako součást hello nástrojů Azure pro IntelliJ. Pokyny jak tooinstall hello nástrojů Azure, najdete v části [hello instalace nástrojů Azure pro IntelliJ](../azure-toolkit-for-intellij-installation.md).
* Přihlaste se k předplatnému Azure z IntelliJ IDEA. Postupujte podle pokynů hello [zde](hdinsight-apache-spark-intellij-tool-plugin.md).
* Při spouštění aplikací Spark Scala pro vzdálené ladění na počítači se systémem Windows, může získat výjimku, jak je popsáno v [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356) k tomu dojde kvůli chybějící tooa WinUtils.exe v systému Windows. toowork vyřešit tuto chybu, musíte [hello spustitelný soubor stáhnout odsud](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa umístění jako **C:\WinUtils\bin**. Pak musíte přidat proměnnou prostředí **HADOOP_HOME** a nastavte hodnotu hello hello proměnné příliš**C\WinUtils**.

## <a name="step-1-create-an-azure-virtual-network"></a>Krok 1: Vytvoření virtuální sítě Azure
Postupujte podle pokynů hello z hello pod odkazy toocreate virtuální síť Azure a pak ověřte hello připojení mezi hello plochy a virtuální síti Azure.

* [Vytvoření virtuální sítě s připojením VPN typu site-to-site pomocí portálu Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [Vytvoření virtuální sítě s připojením VPN typu site-to-site pomocí prostředí PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [Konfigurace připojení point-to-site tooa virtuální sítě pomocí prostředí PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="step-2-create-an-hdinsight-spark-cluster"></a>Krok 2: Vytvoření clusteru HDInsight Spark
Měli byste také vytvořit cluster Apache Spark v Azure HDInsight, který je součástí hello virtuální síť Azure, kterou jste vytvořili. Použijte hello informace, které jsou k dispozici na [vytvořit systémem Linux clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Jako součást volitelné konfigurace vyberte hello virtuální síť Azure, kterou jste vytvořili v předchozím kroku hello.

## <a name="step-3-verify-hello-connectivity-between-hello-cluster-headnode-and-your-desktop"></a>Krok 3: Ověření hello připojení mezi headnode hello clusteru a pracovní plochy
1. Získáte IP adresu hello hello headnode. Otevřete uživatelské rozhraní Ambari pro hello cluster. V okně hello clusteru, klikněte na tlačítko **řídicí panel**.

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/launch-ambari-ui.png)
2. Z hello uživatelského rozhraní Ambari, v pravém horním rohu hello, klikněte na tlačítko **hostitele**.

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/ambari-hosts.png)
3. Zobrazí seznam headnodes, pracovní uzly a uzly zookeeper. Hello headnodes mít hello **hn*** předponu. Klikněte na první headnode hello.

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/cluster-headnodes.png)
4. V dolní části hello hello stránky, které se otevře z hello **Souhrn** pole kopie hello IP adresu hello headnode a název hostitele hello.

    ![Hledání headnode IP adresy](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/headnode-ip-address.png)
5. Zahrnout hello IP adresu a název hostitele hello hello headnode toohello **hostitele** soubor na počítači hello z kam chcete toorun a vzdáleně ladit úlohy Spark hello. To vám umožní toocommunicate s headnode hello pomocí hello IP adresu a také hello název hostitele.

   1. Otevřete Poznámkový blok se zvýšenými oprávněními. Z nabídky Soubor hello, klikněte na tlačítko **otevřete** a pak přejděte toohello umístění souboru hostitelů hello. Na počítači s Windows, je `C:\Windows\System32\Drivers\etc\hosts`.
   2. Přidejte následující toohello hello **hostitele** souboru.

           # For headnode0
           192.xxx.xx.xx hn0-nitinp
           192.xxx.xx.xx hn0-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net

           # For headnode1
           192.xxx.xx.xx hn1-nitinp
           192.xxx.xx.xx hn1-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
6. Z hello počítači, který je připojen toohello Azure Virtual Network, který je používán hello clusteru HDInsight ověřte, že může odeslat příkaz ping obou hello headnodes pomocí hello IP adresu a také hello název hostitele.
7. SSH do headnode hello clusteru pomocí pokynů hello [clusteru HDInsight tooan připojit pomocí protokolu SSH](hdinsight-hadoop-linux-use-ssh-unix.md). Z clusteru headnode hello odeslat příkaz ping hello IP adresu hello stolní počítač. Měli byste otestovat připojení tooboth hello IP adresy přiřazené toohello počítače, jeden pro hello síťové připojení a hello jiné pro hello virtuální síť Azure, která hello počítač je připojený k.
8. Opakujte kroky hello pro hello také jiné headnode.

## <a name="step-4-create-a-spark-scala-application-using-hello-hdinsight-tools-in-azure-toolkit-for-intellij-and-configure-it-for-remote-debugging"></a>Krok 4: Vytvoření aplikace Spark Scala pomocí hello nástroje HDInsight pro IntelliJ v Azure nástrojů a nakonfigurovat ji pro vzdálené ladění
1. Spusťte IntelliJ IDEA a vytvoření nového projektu. V hello projektu dialogové okno Nový, zkontrolujte hello následující možnosti a pak klikněte na **Další**.

    ![Vytvoření aplikací Spark Scala](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-hdi-scala-app.png)

   * V levém podokně hello, vyberte **HDInsight**.
   * V pravém podokně hello vyberte **Spark v HDInsight (Scala)**.
   * Klikněte na **Další**.
2. V dalším okně hello zadejte hello následujících podrobností projektu a pak klikněte na tlačítko **Dokončit**.  
   - Zadejte název projektu a umístění projektu.
   - Pro **SDK projektu**, použijte Java 1.8 pro cluster spark 2.x, Java 1.7 pro 1.x clusteru spark.
   - Pro **Spark verze**, Průvodce vytvořením projektu Scala integruje správnou verzi sady SDK Spark a Scala SDK. Pokud verze clusteru spark hello nižší 2.0, zvolte spark 1.x. Jinak měli byste vybrat spark2.x. Tento příklad používá Spark2.0.2 (Scala 2.11.8).
       ![Vytvoření aplikací Spark Scala](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-scala-project-details.png)
  
3. Hello Spark projektu automaticky vytvoří artefakt. toosee hello artefaktů, postupujte podle těchto kroků.

   1. Z hello **soubor** nabídky, klikněte na tlačítko **strukturu projektu**.
   2. V hello **strukturu projektu** dialogové okno, klikněte na tlačítko **artefakty** toosee hello výchozí artefaktů, který je vytvořen.
   ![Vytvoření JAR](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/default-artifact.png)

      Můžete také vytvořit vlastní artefaktů bly kliknutím na hello  **+**  ikonu, vyznačené na obrázku hello výše.

4. Přidání knihovny tooyour projektu. Klikněte pravým tlačítkem na název projektu hello ve stromu projektu hello tooadd knihovny a pak klikněte na **otevřete nastavení modulu**. V hello **strukturu projektu** dialogové okno, v levém podokně hello, klikněte na tlačítko **knihovny**, klikněte na symbol hello (+) a pak klikněte na tlačítko **z Maven**.

    ![Přidání knihovny](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/add-library.png)

    V hello **knihovny stáhnout z úložiště Maven** dialogové okno, vyhledávání a přidejte následující knihovny hello.

   * `org.scalatest:scalatest_2.10:2.2.1`
   * `org.apache.hadoop:hadoop-azure:2.7.1`
5. Kopírování `yarn-site.xml` a `core-site.xml` z hello clusteru headnode a přidejte ji toohello projektu. Použijte následující příkazy toocopy hello soubory hello. Můžete použít [emulaci](https://cygwin.com/install.html) toorun hello následující `scp` příkazy toocopy hello soubory z clusteru headnodes hello.

        scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .

    Protože jsme již přidali hello clusteru headnode IP adresy a názvy hostitelů fo hello souboru hostitelů na ploše hello, můžeme použít hello **spojovací bod služby** příkazy v hello následujícím způsobem.

        scp sshuser@hn0-nitinp:/etc/hadoop/conf/core-site.xml .
        scp sshuser@hn0-nitinp:/etc/hadoop/conf/yarn-site.xml .

    Přidat tyto soubory tooyour projekt zkopírováním pod hello **/src** složky ve stromu vašeho projektu, například `<your project directory>\src`.
6. Aktualizace hello `core-site.xml` toomake hello následující změny.

   1. `core-site.xml`zahrnuje hello šifrované klíče toohello úložiště účet přidružený ke clusteru hello. V hello `core-site.xml` , že jste přidali toohello projektu, nahraďte hello zašifrovaný klíč s hello skutečné úložiště klíč přidružený k hello výchozí účet úložiště. V tématu [Správa přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).

           <property>
                 <name>fs.azure.account.key.hdistoragecentral.blob.core.windows.net</name>
                 <value>access-key-associated-with-the-account</value>
           </property>
   2. Odebrat hello následujících položek z hello `core-site.xml`.

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
   3. Uložte soubor hello.
7. Přidání hello hlavní třídy pro vaši aplikaci. Z hello **Project Exploreru**, klikněte pravým tlačítkem na **src**, bod příliš**nový**a potom klikněte na **Scala třída**.

    ![Přidejte zdrojový kód](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code.png)
8. V hello **vytvořte novou třídu Scala** dialogovém okně zadejte název, **druhu** vyberte **objekt**a potom klikněte na **OK**.

    ![Přidejte zdrojový kód](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code-object.png)
9. V hello `MyClusterAppMain.scala` souboru, vložte následující kód hello. Tento kód vytvoří hello Spark kontextu a spustí `executeJob` metoda z hello `SparkSample` objektu.

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

10. Opakujte kroky 8 a 9 výše tooadd názvem nového objektu Scala `SparkSample`. Třída toothis přidejte následující kód hello. Tento kód čte hello data z hello HVAC.csv (k dispozici na všech clusterech HDInsight Spark), načte hello řádky, které mají pouze jednu číslici v hello sedmého sloupci hello sdíleného svazku clusteru a zapíše výstup hello příliš**/HVACOut** pod výchozí hello Kontejner úložiště pro hello cluster.

        import org.apache.spark.SparkContext

        object SparkSample {
         def executeJob (sc: SparkContext, input: String, output: String): Unit = {
           val rdd = sc.textFile(input)

           //find hello rows which have only one digit in hello 7th column in hello CSV
           val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

           val s = sc.parallelize(rdd.take(5)).cartesian(rdd).count()
           println(s)

           rdd1.saveAsTextFile(output)
           //rdd1.collect().foreach(println)
         }
        }
11. Opakujte kroky 8 a 9 výše tooadd novou třídu s názvem `RemoteClusterDebugging`. Tato třída implementuje hello Spark test framework, který slouží k ladění aplikací. Přidejte následující kód toohello hello `RemoteClusterDebugging` třídy.

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

     Několik důležitých věcí toonote tady:

   * Pro `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, zkontrolujte, zda je k dispozici v úložišti clusteru hello v zadané cestě hello hello Spark sestavení JAR.
   * Pro `setJars`, zadejte hello umístění, kde bude vytvořen jar artefaktů hello. Obvykle je `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.
12. V hello `RemoteClusterDebugging` třídy, klikněte pravým tlačítkem na hello `test` – klíčové slovo a vyberte **vytvořit konfigurace RemoteClusterDebugging**.

    ![Vytvoření konfigurace vzdáleného](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-remote-config.png)

13. V dialogovém okně hello, zadejte název pro konfiguraci hello a vyberte hello **testovací druh** jako **název testovacího**. Nechat všechny ostatní hodnoty jako výchozí, klikněte na **použít**a potom klikněte na **OK**.

    ![Vytvoření konfigurace vzdáleného](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/provide-config-value.png)
14. Teď byste měli vidět **vzdálené spuštění** konfigurace rozevírací seznam v řádku nabídek hello.

    ![Vytvoření konfigurace vzdáleného](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/config-run.png)

## <a name="step-5-run-hello-application-in-debug-mode"></a>Krok 5: Hello aplikaci spustit v režimu ladění
1. Otevřete v projektu IntelliJ IDEA `SparkSample.scala` a vytvořit další rdd1 too'val zarážek '. V místní nabídce hello pro vytvoření zarážku vyberte **řádku funkce executeJob**.

    ![Přidat zarážky](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-breakpoint.png)
2. Klikněte na tlačítko hello **ladění spustit** tlačítko Další toohello **vzdálené spuštění** toostart rozevíracího seznamu configuration spuštěna aplikace hello.

    ![Spuštění programu hello v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-run-mode.png)
3. Při spuštění programu hello dosáhne hello zarážek, měli byste vidět **ladicí program** ve spodním podokně hello.

    ![Spuštění programu hello v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch.png)
4. Klikněte na tlačítko hello (**+**) ikona tooadd sledovat, jak ukazuje následující obrázek hello.

    ![Spuštění programu hello v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable.png)

    Sem, protože aplikace hello překročila před proměnnou hello `rdd1` byla vytvořena pomocí tohoto sledování, uvidíte, jaké jsou hello prvních 5 řádků v proměnné hello `rdd`. Stiskněte **ENTER**.

    ![Spuštění programu hello v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable-value.png)

    Co se zobrazí v hello obrázku výše je, že v době běhu můžete dotazovat terrabytes dat a ladění jak postupuje vaší aplikace. Například v hello výstup hello obrázku výše, uvidíte této hello první řádek výstupu hello je záhlaví. Řádek záhlaví tooskip hello vaší aplikace kódu můžete upravit na základě toho se v případě potřeby.
5. Nyní můžete kliknout na hello **obnovit Program** tooproceed ikona s vaší aplikací spustit.

    ![Spuštění programu hello v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-continue-run.png)
6. Pokud aplikace hello úspěšně dokončí, měli byste vidět výstup jako následující hello.

    ![Spuštění programu hello v režimu ladění](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-complete.png)

## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="demo"></a>Ukázka
* Vytvoření projektu Scala (Video): [vytvoření aplikací Spark Scala](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Vzdálené ladění (Video): [pomocí nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně na clusteru HDInsight](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

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
* [Používat nástroje HDInsight pro IntelliJ toocreate v nástrojů Azure a odesílání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Použití nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně přes SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Použití nástrojů HDInsight v Azure nástrojů Eclipse toocreate Spark aplikací](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
