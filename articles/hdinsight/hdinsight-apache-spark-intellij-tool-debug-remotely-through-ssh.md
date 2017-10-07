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
# <a name="debug-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a><span data-ttu-id="a0047-104">Ladění aplikací Spark v clusteru s Azure nástrojů HDInsight pro IntelliJ prostřednictvím SSH</span><span class="sxs-lookup"><span data-stu-id="a0047-104">Debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH</span></span>

<span data-ttu-id="a0047-105">Tento článek obsahuje podrobné pokyny o tom, toouse nástroje HDInsight v Azure nástrojů pro IntelliJ toodebug aplikace vzdáleně v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a0047-105">This article provides step-by-step guidance on how toouse HDInsight Tools in Azure Toolkit for IntelliJ toodebug applications remotely on an HDInsight cluster.</span></span> <span data-ttu-id="a0047-106">toodebug projektu, můžete také zobrazit hello [HDInsight Spark ladění aplikace s Azure Toolkit pro IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) videa.</span><span class="sxs-lookup"><span data-stu-id="a0047-106">toodebug your project, you can also view hello [Debug HDInsight Spark applications with Azure Toolkit for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) video.</span></span>

<span data-ttu-id="a0047-107">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="a0047-107">**Prerequisites**</span></span>

* <span data-ttu-id="a0047-108">**Nástroje HDInsight v Azure nástrojů pro IntelliJ**.</span><span class="sxs-lookup"><span data-stu-id="a0047-108">**HDInsight Tools in Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="a0047-109">Tento nástroj je součástí sady nástrojů Azure pro IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="a0047-109">This tool is part of Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="a0047-110">Další informace najdete v tématu [instalaci nástrojů Azure pro IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).</span><span class="sxs-lookup"><span data-stu-id="a0047-110">For more information, see [Install Azure Toolkit for IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).</span></span>
* <span data-ttu-id="a0047-111">**Azure nástrojů pro IntelliJ**.</span><span class="sxs-lookup"><span data-stu-id="a0047-111">**Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="a0047-112">Tato sada nástrojů toocreate Spark aplikace použijte pro cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a0047-112">Use this toolkit toocreate Spark applications for an HDInsight cluster.</span></span> <span data-ttu-id="a0047-113">Další informace, postupujte podle pokynů hello v [pomocí nástrojů Azure pro IntelliJ toocreate aplikací Spark pro cluster služby HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).</span><span class="sxs-lookup"><span data-stu-id="a0047-113">For more information, follow hello instructions in [Use Azure Toolkit for IntelliJ toocreate Spark applications for an HDInsight cluster](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).</span></span>
* <span data-ttu-id="a0047-114">**HDInsight SSH služby pomocí uživatelského jména a hesla správu**.</span><span class="sxs-lookup"><span data-stu-id="a0047-114">**HDInsight SSH service with username and password management**.</span></span> <span data-ttu-id="a0047-115">Další informace najdete v tématu [připojit tooHDInsight (Hadoop) pomocí protokolu SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) a [použití SSH tunelu tooaccess Ambari webové uživatelské rozhraní, JobHistory, NameNode, Oozie a jiné webové uživatelská](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel).</span><span class="sxs-lookup"><span data-stu-id="a0047-115">For more information, see [Connect tooHDInsight (Hadoop) by using SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) and [Use SSH tunneling tooaccess Ambari web UI, JobHistory, NameNode, Oozie, and other web UIs](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel).</span></span> 
 

## <a name="create-a-spark-scala-application-and-configure-it-for-remote-debugging"></a><span data-ttu-id="a0047-116">Vytvoření aplikací Spark Scala a nakonfigurovat ji pro vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="a0047-116">Create a Spark Scala application and configure it for remote debugging</span></span>

1. <span data-ttu-id="a0047-117">Spusťte IntelliJ IDEA a pak vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="a0047-117">Start IntelliJ IDEA, and then create a project.</span></span> <span data-ttu-id="a0047-118">V hello **nový projekt** dialogové okno pole, hello následující:</span><span class="sxs-lookup"><span data-stu-id="a0047-118">In hello **New Project** dialog box, do hello following:</span></span>

   <span data-ttu-id="a0047-119">a.</span><span class="sxs-lookup"><span data-stu-id="a0047-119">a.</span></span> <span data-ttu-id="a0047-120">Vyberte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="a0047-120">Select **HDInsight**.</span></span> 

   <span data-ttu-id="a0047-121">b.</span><span class="sxs-lookup"><span data-stu-id="a0047-121">b.</span></span> <span data-ttu-id="a0047-122">Vyberte Java nebo Scala šablony založené na vaši volbu.</span><span class="sxs-lookup"><span data-stu-id="a0047-122">Select a Java or Scala template based on your preference.</span></span> <span data-ttu-id="a0047-123">Vyberte mezi hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="a0047-123">Select between hello following options:</span></span>

      - <span data-ttu-id="a0047-124">**Spark v HDInsight (Scala)**</span><span class="sxs-lookup"><span data-stu-id="a0047-124">**Spark on HDInsight (Scala)**</span></span>

      - <span data-ttu-id="a0047-125">**Spark v HDInsight (Java)**</span><span class="sxs-lookup"><span data-stu-id="a0047-125">**Spark on HDInsight (Java)**</span></span>

      - <span data-ttu-id="a0047-126">**Spark v clusteru HDInsight spuštění ukázkové (Scala)**</span><span class="sxs-lookup"><span data-stu-id="a0047-126">**Spark on HDInsight Cluster Run Sample (Scala)**</span></span>

      <span data-ttu-id="a0047-127">Tento příklad používá **Spark v HDInsight clusteru spustit ukázkový (Scala)** šablony.</span><span class="sxs-lookup"><span data-stu-id="a0047-127">This example uses a **Spark on HDInsight Cluster Run Sample (Scala)** template.</span></span>

   <span data-ttu-id="a0047-128">c.</span><span class="sxs-lookup"><span data-stu-id="a0047-128">c.</span></span> <span data-ttu-id="a0047-129">V hello **nástroj pro sestavení** vyberte jednu z následujících, podle potřeby tooyour hello:</span><span class="sxs-lookup"><span data-stu-id="a0047-129">In hello **Build tool** list, select either of hello following, according tooyour need:</span></span>

      - <span data-ttu-id="a0047-130">**Maven**, podpora Průvodce vytvoření projektu Scala</span><span class="sxs-lookup"><span data-stu-id="a0047-130">**Maven**, for Scala project-creation wizard support</span></span>

      -  <span data-ttu-id="a0047-131">**SBT**, pro správu hello závislosti a sestavování pro projekt hello Scala</span><span class="sxs-lookup"><span data-stu-id="a0047-131">**SBT**, for managing hello dependencies and building for hello Scala project</span></span> 

      ![Vytvoření projektu ladění](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-create-projectfor-debug-remotely.png)

   <span data-ttu-id="a0047-133">d.</span><span class="sxs-lookup"><span data-stu-id="a0047-133">d.</span></span> <span data-ttu-id="a0047-134">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="a0047-134">Select **Next**.</span></span>     
 
3. <span data-ttu-id="a0047-135">V hello Další **nový projekt** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="a0047-135">In hello next **New Project** window, do hello following:</span></span>

   ![Vyberte hello Spark SDK](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-new-project.png)

   <span data-ttu-id="a0047-137">a.</span><span class="sxs-lookup"><span data-stu-id="a0047-137">a.</span></span> <span data-ttu-id="a0047-138">Zadejte název projektu a umístění projektu.</span><span class="sxs-lookup"><span data-stu-id="a0047-138">Enter a project name and project location.</span></span>

   <span data-ttu-id="a0047-139">b.</span><span class="sxs-lookup"><span data-stu-id="a0047-139">b.</span></span> <span data-ttu-id="a0047-140">V hello **SDK projektu** rozevíracího seznamu vyberte **Java 1.8** pro **Spark 2.x** clusteru nebo vyberte **Java 1.7** pro **Spark 1. x** clusteru.</span><span class="sxs-lookup"><span data-stu-id="a0047-140">In hello **Project SDK** drop-down list, select **Java 1.8** for **Spark 2.x** cluster or select **Java 1.7** for **Spark 1.x** cluster.</span></span>

   <span data-ttu-id="a0047-141">c.</span><span class="sxs-lookup"><span data-stu-id="a0047-141">c.</span></span> <span data-ttu-id="a0047-142">V hello **Spark verze** rozevíracího seznamu, Průvodce vytvořením projektu Scala hello integruje hello správná verze pro sadu SDK Spark a Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="a0047-142">In hello **Spark Version** drop-down list, hello Scala project creation wizard integrates hello correct version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="a0047-143">Pokud verze clusteru spark hello je starší než 2.0, vyberte **Spark 1.x**.</span><span class="sxs-lookup"><span data-stu-id="a0047-143">If hello spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="a0047-144">Jinak vyberte možnost **Spark 2.x.**</span><span class="sxs-lookup"><span data-stu-id="a0047-144">Otherwise, select **Spark 2.x.**</span></span> <span data-ttu-id="a0047-145">Tento příklad používá **Spark bodu 2.0.2 (Scala 2.11.8)**.</span><span class="sxs-lookup"><span data-stu-id="a0047-145">This example uses **Spark 2.0.2 (Scala 2.11.8)**.</span></span>

   <span data-ttu-id="a0047-146">d.</span><span class="sxs-lookup"><span data-stu-id="a0047-146">d.</span></span> <span data-ttu-id="a0047-147">Vyberte **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="a0047-147">Select **Finish**.</span></span>

4. <span data-ttu-id="a0047-148">Vyberte **src** > **hlavní** > **scala** tooopen kódu v projektu hello.</span><span class="sxs-lookup"><span data-stu-id="a0047-148">Select **src** > **main** > **scala** tooopen your code in hello project.</span></span> <span data-ttu-id="a0047-149">Tento příklad používá hello **SparkCore_wasbloTest** skriptu.</span><span class="sxs-lookup"><span data-stu-id="a0047-149">This example uses hello **SparkCore_wasbloTest** script.</span></span>

5. <span data-ttu-id="a0047-150">tooaccess hello **upravit konfigurace** nabídky, vyberte hello ikonu v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="a0047-150">tooaccess hello **Edit Configurations** menu, select hello icon in hello upper-right corner.</span></span> <span data-ttu-id="a0047-151">Z této nabídky můžete vytvořit nebo upravit hello konfigurace pro vzdálené ladění.</span><span class="sxs-lookup"><span data-stu-id="a0047-151">From this menu, you can create or edit hello configurations for remote debugging.</span></span>

   ![Úprava konfigurací](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-edit-configurations.png) 

6. <span data-ttu-id="a0047-153">V hello **konfigurace spustit/Debug** dialogové okno, vyberte hello – znaménko plus (**+**).</span><span class="sxs-lookup"><span data-stu-id="a0047-153">In hello **Run/Debug Configurations** dialog box, select hello plus sign (**+**).</span></span> <span data-ttu-id="a0047-154">Potom vyberte hello **odeslat úlohu Spark** možnost.</span><span class="sxs-lookup"><span data-stu-id="a0047-154">Then select hello **Submit Spark Job** option.</span></span>

   ![Přidat novou konfiguraci](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-add-new-Configuration.png)
7. <span data-ttu-id="a0047-156">Zadejte informace pro **název**, **clusteru Spark**, a **hlavní název třídy**.</span><span class="sxs-lookup"><span data-stu-id="a0047-156">Enter information for **Name**, **Spark cluster**, and **Main class name**.</span></span> <span data-ttu-id="a0047-157">Potom vyberte **pokročilou konfiguraci**.</span><span class="sxs-lookup"><span data-stu-id="a0047-157">Then select **Advanced configuration**.</span></span> 

   ![Spuštění konfigurace ladění](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-run-debug-configurations.png)

8. <span data-ttu-id="a0047-159">V hello **Spark odeslání Upřesnit konfiguraci** dialogové okno, vyberte **Spark povolení vzdáleného ladění**.</span><span class="sxs-lookup"><span data-stu-id="a0047-159">In hello **Spark Submission Advanced Configuration** dialog box, select **Enable Spark remote debug**.</span></span> <span data-ttu-id="a0047-160">Zadejte uživatelské jméno SSH hello a pak zadejte heslo nebo použít soubor privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="a0047-160">Enter hello SSH username, and then enter a password or use a private key file.</span></span> <span data-ttu-id="a0047-161">toosave hello konfiguraci, vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="a0047-161">toosave hello configuration, select **OK**.</span></span>

   ![Povolení vzdáleného ladění Spark](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-enable-spark-remote-debug.png)

9. <span data-ttu-id="a0047-163">Konfigurace Hello jsou nyní uloženy hello názvem, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="a0047-163">hello configuration is now saved with hello name you provided.</span></span> <span data-ttu-id="a0047-164">tooview hello podrobnosti o konfiguraci, vyberte hello název konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a0047-164">tooview hello configuration details, select hello configuration name.</span></span> <span data-ttu-id="a0047-165">Vyberte změny toomake **upravit konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="a0047-165">toomake changes, select **Edit Configurations**.</span></span> 

10. <span data-ttu-id="a0047-166">Po dokončení konfigurace nastavení hello můžete spustit projekt hello oproti vzdáleném clusteru hello nebo provést vzdálené ladění.</span><span class="sxs-lookup"><span data-stu-id="a0047-166">After you complete hello configurations settings, you can run hello project against hello remote cluster or perform remote debugging.</span></span>

## <a name="learn-how-tooperform-remote-debugging"></a><span data-ttu-id="a0047-167">Zjistěte, jak tooperform vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="a0047-167">Learn how tooperform remote debugging</span></span>
### <a name="scenario-1-perform-remote-run"></a><span data-ttu-id="a0047-168">Scénář 1: Provést vzdálené spuštění</span><span class="sxs-lookup"><span data-stu-id="a0047-168">Scenario 1: Perform remote run</span></span>

<span data-ttu-id="a0047-169">V této části Ukážeme vám jak toodebug ovladače a vykonavatelů.</span><span class="sxs-lookup"><span data-stu-id="a0047-169">In this section, we show you how toodebug drivers and executors.</span></span>

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


1. <span data-ttu-id="a0047-170">Nastavit body ukončování řádků a potom vyberte hello **ladění** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a0047-170">Set up breaking points, and then select hello **Debug** icon.</span></span>

   ![Vyberte ikonu pro ladění hello](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-icon.png)

2. <span data-ttu-id="a0047-172">Při spuštění programu hello dosáhne hello nejnovější bod, uvidíte **ovladač** kartě a dvě **vykonavatele** karty v hello **ladicí program** podokně.</span><span class="sxs-lookup"><span data-stu-id="a0047-172">When hello program execution reaches hello breaking point, you see a **Driver** tab and two **Executor** tabs in hello **Debugger** pane.</span></span> <span data-ttu-id="a0047-173">Vyberte hello **obnovit Program** toocontinue ikona spuštění hello kódu, který pak dosáhne hello další zarážek a se zaměřuje na odpovídající hello **vykonavatele** kartě. Můžete zkontrolovat hello protokoly spouštění na odpovídající hello **konzoly** kartě.</span><span class="sxs-lookup"><span data-stu-id="a0047-173">Select hello **Resume Program** icon toocontinue running hello code, which then reaches hello next breakpoint and focuses on hello corresponding **Executor** tab. You can review hello execution logs on hello corresponding **Console** tab.</span></span>

   ![Karta ladění](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debugger-tab.png)

### <a name="scenario-2-perform-remote-debugging-and-bug-fixing"></a><span data-ttu-id="a0047-175">Scénář 2: Proveďte vzdálené ladění a opravy chyb</span><span class="sxs-lookup"><span data-stu-id="a0047-175">Scenario 2: Perform remote debugging and bug fixing</span></span>
<span data-ttu-id="a0047-176">V této části Ukážeme vám jak toodynamically aktualizace hello hodnota proměnné pomocí hello IntelliJ ladění funkce pro jednoduché opravu.</span><span class="sxs-lookup"><span data-stu-id="a0047-176">In this section, we show you how toodynamically update hello variable value by using hello IntelliJ debugging capability for a simple fix.</span></span> <span data-ttu-id="a0047-177">V hello následující ukázka kódu je vyvolána výjimka, protože hello cílový soubor již existuje.</span><span class="sxs-lookup"><span data-stu-id="a0047-177">In hello following code example, an exception is thrown because hello target file already exists.</span></span>
  
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


#### <a name="tooperform-remote-debugging-and-bug-fixing"></a><span data-ttu-id="a0047-178">vzdálené ladění tooperform a opravy chyb</span><span class="sxs-lookup"><span data-stu-id="a0047-178">tooperform remote debugging and bug fixing</span></span>
1. <span data-ttu-id="a0047-179">Nastavit dva body ukončování řádků a potom vyberte hello **ladění** ikonu toostart hello vzdálené ladění procesu.</span><span class="sxs-lookup"><span data-stu-id="a0047-179">Set up two breaking points, and then select hello **Debug** icon toostart hello remote debugging process.</span></span>

2. <span data-ttu-id="a0047-180">Kód Hello zastaví narušující okamžiku první hello a parametr hello a informace o proměnných jsou uvedeny v hello **proměnné** podokně.</span><span class="sxs-lookup"><span data-stu-id="a0047-180">hello code stops at hello first breaking point, and hello parameter and variable information are shown in hello **Variables** pane.</span></span> 

3. <span data-ttu-id="a0047-181">Vyberte hello **obnovit Program** toocontinue ikonu.</span><span class="sxs-lookup"><span data-stu-id="a0047-181">Select hello **Resume Program** icon toocontinue.</span></span> <span data-ttu-id="a0047-182">Kód Hello zastaví na druhý bod hello.</span><span class="sxs-lookup"><span data-stu-id="a0047-182">hello code stops at hello second point.</span></span> <span data-ttu-id="a0047-183">podle očekávání, je byla zachycena výjimka Hello.</span><span class="sxs-lookup"><span data-stu-id="a0047-183">hello exception is caught as expected.</span></span>

  ![Throw – chyba](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-throw-error.png) 

4. <span data-ttu-id="a0047-185">Vyberte hello **obnovit Program** ikonu znovu.</span><span class="sxs-lookup"><span data-stu-id="a0047-185">Select hello **Resume Program** icon again.</span></span> <span data-ttu-id="a0047-186">Hello **HDInsight Spark odeslání** okně se zobrazí chyba "Úloha spuštění se nepovedlo".</span><span class="sxs-lookup"><span data-stu-id="a0047-186">hello **HDInsight Spark Submission** window displays a "job run failed" error.</span></span>

  ![Chyba odeslání](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-error-submission.png) 

5. <span data-ttu-id="a0047-188">Vyberte toodynamically aktualizace hello hodnota proměnné pomocí funkce ladění hello IntelliJ, **ladění** znovu.</span><span class="sxs-lookup"><span data-stu-id="a0047-188">toodynamically update hello variable value by using hello IntelliJ debugging capability, select **Debug** again.</span></span> <span data-ttu-id="a0047-189">Hello **proměnné** podokně se zobrazí znovu.</span><span class="sxs-lookup"><span data-stu-id="a0047-189">hello **Variables** pane appears again.</span></span> 

6. <span data-ttu-id="a0047-190">Cíl hello klikněte pravým tlačítkem na hello **ladění** a pak vyberte **nastavit hodnotu**.</span><span class="sxs-lookup"><span data-stu-id="a0047-190">Right-click hello target on hello **Debug** tab, and then select **Set Value**.</span></span> <span data-ttu-id="a0047-191">Potom zadejte novou hodnotu pro proměnnou hello.</span><span class="sxs-lookup"><span data-stu-id="a0047-191">Next, enter a new value for hello variable.</span></span> <span data-ttu-id="a0047-192">Potom vyberte **Enter** toosave hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a0047-192">Then select **Enter** toosave hello value.</span></span> 

  ![Nastavte hodnotu](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-set-value.png) 

7. <span data-ttu-id="a0047-194">Vyberte hello **obnovit Program** ikonu toocontinue toorun hello programu.</span><span class="sxs-lookup"><span data-stu-id="a0047-194">Select hello **Resume Program** icon toocontinue toorun hello program.</span></span> <span data-ttu-id="a0047-195">Tentokrát je žádná výjimka zachycena.</span><span class="sxs-lookup"><span data-stu-id="a0047-195">This time, no exception is caught.</span></span> <span data-ttu-id="a0047-196">Uvidíte, že tento projekt hello funguje úspěšně bez jakékoli výjimky.</span><span class="sxs-lookup"><span data-stu-id="a0047-196">You can see that hello project runs successfully without any exceptions.</span></span>

  ![Ladění bez výjimky](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-without-exception.png)

## <span data-ttu-id="a0047-198"><a name="seealso"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="a0047-198"><a name="seealso"></a>Next steps</span></span>
* [<span data-ttu-id="a0047-199">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="a0047-199">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="a0047-200">Ukázka</span><span class="sxs-lookup"><span data-stu-id="a0047-200">Demo</span></span>
* <span data-ttu-id="a0047-201">Vytvoření projektu Scala (video): [vytvoření aplikací Spark Scala](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="a0047-201">Create Scala project (video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="a0047-202">Vzdálené ladění (video): [pomocí nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně v clusteru HDInsight](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="a0047-202">Remote debug (video): [Use Azure Toolkit for IntelliJ toodebug Spark applications remotely on an HDInsight cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="a0047-203">Scénáře</span><span class="sxs-lookup"><span data-stu-id="a0047-203">Scenarios</span></span>
* [<span data-ttu-id="a0047-204">Spark s BI: provádějte interaktivní analýzy dat pomocí Spark v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="a0047-204">Spark with BI: Perform interactive data analysis by using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="a0047-205">Spark s Machine Learning: používejte Spark v HDInsight tooanalyze vytváření teploty pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="a0047-205">Spark with Machine Learning: Use Spark in HDInsight tooanalyze building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="a0047-206">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="a0047-206">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="a0047-207">Datové proudy Spark: Používejte Spark v HDInsight toobuild v reálném čase streamování aplikací</span><span class="sxs-lookup"><span data-stu-id="a0047-207">Spark Streaming: Use Spark in HDInsight toobuild real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="a0047-208">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a0047-208">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="a0047-209">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="a0047-209">Create and run applications</span></span>
* [<span data-ttu-id="a0047-210">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="a0047-210">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="a0047-211">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="a0047-211">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="a0047-212">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="a0047-212">Tools and extensions</span></span>
* [<span data-ttu-id="a0047-213">Použití nástrojů Azure pro IntelliJ toocreate aplikací Spark pro cluster služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="a0047-213">Use Azure Toolkit for IntelliJ toocreate Spark applications for an HDInsight cluster</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="a0047-214">Použití nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně prostřednictvím sítě VPN</span><span class="sxs-lookup"><span data-stu-id="a0047-214">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="a0047-215">Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="a0047-215">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="a0047-216">Použití nástrojů HDInsight v Azure nástrojů Eclipse toocreate Spark aplikací</span><span class="sxs-lookup"><span data-stu-id="a0047-216">Use HDInsight Tools in Azure Toolkit for Eclipse toocreate Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="a0047-217">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a0047-217">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="a0047-218">Jádra dostupná pro poznámkový blok Jupyter v clusteru hello Spark pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="a0047-218">Kernels available for Jupyter notebook in hello Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="a0047-219">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="a0047-219">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="a0047-220">Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="a0047-220">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="a0047-221">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="a0047-221">Manage resources</span></span>
* [<span data-ttu-id="a0047-222">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="a0047-222">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="a0047-223">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a0047-223">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
