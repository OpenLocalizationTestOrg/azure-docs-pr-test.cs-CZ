---
title: "Azure nástrojů pro IntelliJ: ladění Spark aplikací vzdáleně přes SSH | Microsoft Docs"
description: "Podrobné pokyny o tom, jak používat nástroje HDInsight v Azure nástrojů pro IntelliJ k ladění aplikací vzdáleně na HDInsight clustery prostřednictvím SSH"
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
ms.openlocfilehash: 19053e31d6eb097bc91a04ef9c6af5772aaa16da
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="debug-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a><span data-ttu-id="06612-104">Ladění aplikací Spark v clusteru s Azure nástrojů HDInsight pro IntelliJ prostřednictvím SSH</span><span class="sxs-lookup"><span data-stu-id="06612-104">Debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH</span></span>

<span data-ttu-id="06612-105">Tento článek obsahuje podrobné pokyny o tom, jak používat nástroje HDInsight v Azure nástrojů pro IntelliJ k ladění aplikací vzdáleně v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="06612-105">This article provides step-by-step guidance on how to use HDInsight Tools in Azure Toolkit for IntelliJ to debug applications remotely on an HDInsight cluster.</span></span> <span data-ttu-id="06612-106">K ladění projektu, můžete také zobrazit [HDInsight Spark ladění aplikace s Azure Toolkit pro IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) videa.</span><span class="sxs-lookup"><span data-stu-id="06612-106">To debug your project, you can also view the [Debug HDInsight Spark applications with Azure Toolkit for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) video.</span></span>

<span data-ttu-id="06612-107">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="06612-107">**Prerequisites**</span></span>

* <span data-ttu-id="06612-108">**Nástroje HDInsight v Azure nástrojů pro IntelliJ**.</span><span class="sxs-lookup"><span data-stu-id="06612-108">**HDInsight Tools in Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="06612-109">Tento nástroj je součástí sady nástrojů Azure pro IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="06612-109">This tool is part of Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="06612-110">Další informace najdete v tématu [instalaci nástrojů Azure pro IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).</span><span class="sxs-lookup"><span data-stu-id="06612-110">For more information, see [Install Azure Toolkit for IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).</span></span>
* <span data-ttu-id="06612-111">**Azure nástrojů pro IntelliJ**.</span><span class="sxs-lookup"><span data-stu-id="06612-111">**Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="06612-112">Tato sada nástrojů použijte k vytvoření aplikací Spark pro cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="06612-112">Use this toolkit to create Spark applications for an HDInsight cluster.</span></span> <span data-ttu-id="06612-113">Další informace, postupujte podle pokynů v [pomocí nástrojů Azure pro IntelliJ k vytvoření aplikací Spark pro cluster služby HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).</span><span class="sxs-lookup"><span data-stu-id="06612-113">For more information, follow the instructions in [Use Azure Toolkit for IntelliJ to create Spark applications for an HDInsight cluster](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).</span></span>
* <span data-ttu-id="06612-114">**HDInsight SSH služby pomocí uživatelského jména a hesla správu**.</span><span class="sxs-lookup"><span data-stu-id="06612-114">**HDInsight SSH service with username and password management**.</span></span> <span data-ttu-id="06612-115">Další informace najdete v tématu [připojení k HDInsight (Hadoop) pomocí protokolu SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) a [použití SSH tunelového propojení pro přístup k Ambari webové uživatelské rozhraní, JobHistory, NameNode, Oozie a jiné webové uživatelská](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel).</span><span class="sxs-lookup"><span data-stu-id="06612-115">For more information, see [Connect to HDInsight (Hadoop) by using SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) and [Use SSH tunneling to access Ambari web UI, JobHistory, NameNode, Oozie, and other web UIs](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel).</span></span> 
 

## <a name="create-a-spark-scala-application-and-configure-it-for-remote-debugging"></a><span data-ttu-id="06612-116">Vytvoření aplikací Spark Scala a nakonfigurovat ji pro vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="06612-116">Create a Spark Scala application and configure it for remote debugging</span></span>

1. <span data-ttu-id="06612-117">Spusťte IntelliJ IDEA a pak vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="06612-117">Start IntelliJ IDEA, and then create a project.</span></span> <span data-ttu-id="06612-118">V **nový projekt** dialogové okno pole, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="06612-118">In the **New Project** dialog box, do the following:</span></span>

   <span data-ttu-id="06612-119">a.</span><span class="sxs-lookup"><span data-stu-id="06612-119">a.</span></span> <span data-ttu-id="06612-120">Vyberte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="06612-120">Select **HDInsight**.</span></span> 

   <span data-ttu-id="06612-121">b.</span><span class="sxs-lookup"><span data-stu-id="06612-121">b.</span></span> <span data-ttu-id="06612-122">Vyberte Java nebo Scala šablony založené na vaši volbu.</span><span class="sxs-lookup"><span data-stu-id="06612-122">Select a Java or Scala template based on your preference.</span></span> <span data-ttu-id="06612-123">Vyberte jednu z těchto možností:</span><span class="sxs-lookup"><span data-stu-id="06612-123">Select between the following options:</span></span>

      - <span data-ttu-id="06612-124">**Spark v HDInsight (Scala)**</span><span class="sxs-lookup"><span data-stu-id="06612-124">**Spark on HDInsight (Scala)**</span></span>

      - <span data-ttu-id="06612-125">**Spark v HDInsight (Java)**</span><span class="sxs-lookup"><span data-stu-id="06612-125">**Spark on HDInsight (Java)**</span></span>

      - <span data-ttu-id="06612-126">**Spark v clusteru HDInsight spuštění ukázkové (Scala)**</span><span class="sxs-lookup"><span data-stu-id="06612-126">**Spark on HDInsight Cluster Run Sample (Scala)**</span></span>

      <span data-ttu-id="06612-127">Tento příklad používá **Spark v HDInsight clusteru spustit ukázkový (Scala)** šablony.</span><span class="sxs-lookup"><span data-stu-id="06612-127">This example uses a **Spark on HDInsight Cluster Run Sample (Scala)** template.</span></span>

   <span data-ttu-id="06612-128">c.</span><span class="sxs-lookup"><span data-stu-id="06612-128">c.</span></span> <span data-ttu-id="06612-129">V **nástroj pro sestavení** vyberte některý z následujících akcí podle potřeby vašeho:</span><span class="sxs-lookup"><span data-stu-id="06612-129">In the **Build tool** list, select either of the following, according to your need:</span></span>

      - <span data-ttu-id="06612-130">**Maven**, podpora Průvodce vytvoření projektu Scala</span><span class="sxs-lookup"><span data-stu-id="06612-130">**Maven**, for Scala project-creation wizard support</span></span>

      -  <span data-ttu-id="06612-131">**SBT**, ke správě závislosti a vytváření projektu Scala</span><span class="sxs-lookup"><span data-stu-id="06612-131">**SBT**, for managing the dependencies and building for the Scala project</span></span> 

      ![Vytvoření projektu ladění](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-create-projectfor-debug-remotely.png)

   <span data-ttu-id="06612-133">d.</span><span class="sxs-lookup"><span data-stu-id="06612-133">d.</span></span> <span data-ttu-id="06612-134">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="06612-134">Select **Next**.</span></span>     
 
3. <span data-ttu-id="06612-135">V dalším **nový projekt** okno, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="06612-135">In the next **New Project** window, do the following:</span></span>

   ![Vyberte Spark SDK](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-new-project.png)

   <span data-ttu-id="06612-137">a.</span><span class="sxs-lookup"><span data-stu-id="06612-137">a.</span></span> <span data-ttu-id="06612-138">Zadejte název projektu a umístění projektu.</span><span class="sxs-lookup"><span data-stu-id="06612-138">Enter a project name and project location.</span></span>

   <span data-ttu-id="06612-139">b.</span><span class="sxs-lookup"><span data-stu-id="06612-139">b.</span></span> <span data-ttu-id="06612-140">V **SDK projektu** rozevíracího seznamu vyberte **Java 1.8** pro **Spark 2.x** clusteru nebo vyberte **Java 1.7** pro **Spark 1.x**  clusteru.</span><span class="sxs-lookup"><span data-stu-id="06612-140">In the **Project SDK** drop-down list, select **Java 1.8** for **Spark 2.x** cluster or select **Java 1.7** for **Spark 1.x** cluster.</span></span>

   <span data-ttu-id="06612-141">c.</span><span class="sxs-lookup"><span data-stu-id="06612-141">c.</span></span> <span data-ttu-id="06612-142">V **Spark verze** rozevíracího seznamu, Průvodce vytvořením projektu Scala integruje správná verze pro sadu SDK Spark a Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="06612-142">In the **Spark Version** drop-down list, the Scala project creation wizard integrates the correct version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="06612-143">Pokud verze clusteru spark je starší než 2.0, vyberte **Spark 1.x**.</span><span class="sxs-lookup"><span data-stu-id="06612-143">If the spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="06612-144">Jinak vyberte možnost **Spark 2.x.**</span><span class="sxs-lookup"><span data-stu-id="06612-144">Otherwise, select **Spark 2.x.**</span></span> <span data-ttu-id="06612-145">Tento příklad používá **Spark bodu 2.0.2 (Scala 2.11.8)**.</span><span class="sxs-lookup"><span data-stu-id="06612-145">This example uses **Spark 2.0.2 (Scala 2.11.8)**.</span></span>

   <span data-ttu-id="06612-146">d.</span><span class="sxs-lookup"><span data-stu-id="06612-146">d.</span></span> <span data-ttu-id="06612-147">Vyberte **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="06612-147">Select **Finish**.</span></span>

4. <span data-ttu-id="06612-148">Vyberte **src** > **hlavní** > **scala** otevřete kódu v projektu.</span><span class="sxs-lookup"><span data-stu-id="06612-148">Select **src** > **main** > **scala** to open your code in the project.</span></span> <span data-ttu-id="06612-149">Tento příklad používá **SparkCore_wasbloTest** skriptu.</span><span class="sxs-lookup"><span data-stu-id="06612-149">This example uses the **SparkCore_wasbloTest** script.</span></span>

5. <span data-ttu-id="06612-150">Pro přístup k **upravit konfigurace** nabídce vyberte ikonu v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="06612-150">To access the **Edit Configurations** menu, select the icon in the upper-right corner.</span></span> <span data-ttu-id="06612-151">Z této nabídky můžete vytvořit nebo upravit konfiguraci pro vzdálené ladění.</span><span class="sxs-lookup"><span data-stu-id="06612-151">From this menu, you can create or edit the configurations for remote debugging.</span></span>

   ![Úprava konfigurací](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-edit-configurations.png) 

6. <span data-ttu-id="06612-153">V **konfigurace spustit/Debug** dialogovém okně vyberte znaménko plus (**+**).</span><span class="sxs-lookup"><span data-stu-id="06612-153">In the **Run/Debug Configurations** dialog box, select the plus sign (**+**).</span></span> <span data-ttu-id="06612-154">Vyberte **odeslat úlohu Spark** možnost.</span><span class="sxs-lookup"><span data-stu-id="06612-154">Then select the **Submit Spark Job** option.</span></span>

   ![Přidat novou konfiguraci](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-add-new-Configuration.png)
7. <span data-ttu-id="06612-156">Zadejte informace pro **název**, **clusteru Spark**, a **hlavní název třídy**.</span><span class="sxs-lookup"><span data-stu-id="06612-156">Enter information for **Name**, **Spark cluster**, and **Main class name**.</span></span> <span data-ttu-id="06612-157">Potom vyberte **pokročilou konfiguraci**.</span><span class="sxs-lookup"><span data-stu-id="06612-157">Then select **Advanced configuration**.</span></span> 

   ![Spuštění konfigurace ladění](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-run-debug-configurations.png)

8. <span data-ttu-id="06612-159">V **Spark odeslání Upřesnit konfiguraci** dialogové okno, vyberte **Spark povolení vzdáleného ladění**.</span><span class="sxs-lookup"><span data-stu-id="06612-159">In the **Spark Submission Advanced Configuration** dialog box, select **Enable Spark remote debug**.</span></span> <span data-ttu-id="06612-160">Zadejte uživatelské jméno SSH a pak zadejte heslo nebo použít soubor privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="06612-160">Enter the SSH username, and then enter a password or use a private key file.</span></span> <span data-ttu-id="06612-161">Chcete-li uložit konfiguraci, vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="06612-161">To save the configuration, select **OK**.</span></span>

   ![Povolení vzdáleného ladění Spark](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-enable-spark-remote-debug.png)

9. <span data-ttu-id="06612-163">Konfigurace je nyní uložit s názvem, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="06612-163">The configuration is now saved with the name you provided.</span></span> <span data-ttu-id="06612-164">Chcete-li zobrazit podrobnosti o konfiguraci, vyberte název konfigurace.</span><span class="sxs-lookup"><span data-stu-id="06612-164">To view the configuration details, select the configuration name.</span></span> <span data-ttu-id="06612-165">Chcete-li provést změny, vyberte **upravit konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="06612-165">To make changes, select **Edit Configurations**.</span></span> 

10. <span data-ttu-id="06612-166">Po dokončení nastavení konfigurace, můžete spustit projekt na vzdálený cluster nebo provést vzdálené ladění.</span><span class="sxs-lookup"><span data-stu-id="06612-166">After you complete the configurations settings, you can run the project against the remote cluster or perform remote debugging.</span></span>

## <a name="learn-how-to-perform-remote-debugging"></a><span data-ttu-id="06612-167">Zjistěte, jak provést vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="06612-167">Learn how to perform remote debugging</span></span>
### <a name="scenario-1-perform-remote-run"></a><span data-ttu-id="06612-168">Scénář 1: Provést vzdálené spuštění</span><span class="sxs-lookup"><span data-stu-id="06612-168">Scenario 1: Perform remote run</span></span>

<span data-ttu-id="06612-169">V této části Ukážeme vám ladění ovladače a vykonavatelů.</span><span class="sxs-lookup"><span data-stu-id="06612-169">In this section, we show you how to debug drivers and executors.</span></span>

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
        /** Tracks the total query count and number of aggregate bytes for a particular group. */
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


1. <span data-ttu-id="06612-170">Nastavit body ukončování řádků a pak vyberte **ladění** ikonu.</span><span class="sxs-lookup"><span data-stu-id="06612-170">Set up breaking points, and then select the **Debug** icon.</span></span>

   ![Vyberte ikonu pro ladění](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-icon.png)

2. <span data-ttu-id="06612-172">Při spuštění programu dosáhne bodem ukončování řádků, uvidíte **ovladač** kartě a dvě **vykonavatele** karty v **ladicí program** podokně.</span><span class="sxs-lookup"><span data-stu-id="06612-172">When the program execution reaches the breaking point, you see a **Driver** tab and two **Executor** tabs in the **Debugger** pane.</span></span> <span data-ttu-id="06612-173">Vyberte **obnovit Program** ikonu dál spuštěný kód, který pak dosáhne další zarážek a se zaměřuje na odpovídající **vykonavatele** kartě. Můžete zkontrolovat protokoly spouštění na odpovídající **konzoly** kartě.</span><span class="sxs-lookup"><span data-stu-id="06612-173">Select the **Resume Program** icon to continue running the code, which then reaches the next breakpoint and focuses on the corresponding **Executor** tab. You can review the execution logs on the corresponding **Console** tab.</span></span>

   ![Karta ladění](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debugger-tab.png)

### <a name="scenario-2-perform-remote-debugging-and-bug-fixing"></a><span data-ttu-id="06612-175">Scénář 2: Proveďte vzdálené ladění a opravy chyb</span><span class="sxs-lookup"><span data-stu-id="06612-175">Scenario 2: Perform remote debugging and bug fixing</span></span>
<span data-ttu-id="06612-176">V této části jsme ukazují, jak pomocí možnosti ladění IntelliJ pro jednoduché opravu dynamicky aktualizovat hodnotu proměnné.</span><span class="sxs-lookup"><span data-stu-id="06612-176">In this section, we show you how to dynamically update the variable value by using the IntelliJ debugging capability for a simple fix.</span></span> <span data-ttu-id="06612-177">V následujícím příkladu kódu je vyvolána výjimka, protože cílový soubor již existuje.</span><span class="sxs-lookup"><span data-stu-id="06612-177">In the following code example, an exception is thrown because the target file already exists.</span></span>
  
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext

        object SparkCore_WasbIOTest {
          def main(arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkCore_WasbIOTest")
            val sc = new SparkContext(conf)
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            // Find the rows that have only one digit in the sixth column.
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


#### <a name="to-perform-remote-debugging-and-bug-fixing"></a><span data-ttu-id="06612-178">K provedení vzdálené ladění a opravy chyb</span><span class="sxs-lookup"><span data-stu-id="06612-178">To perform remote debugging and bug fixing</span></span>
1. <span data-ttu-id="06612-179">Nastavit dva body ukončování řádků a pak vyberte **ladění** ikonu pro spuštění vzdáleného ladění procesu.</span><span class="sxs-lookup"><span data-stu-id="06612-179">Set up two breaking points, and then select the **Debug** icon to start the remote debugging process.</span></span>

2. <span data-ttu-id="06612-180">Kód zastaví v prvním bodu ukončování řádků, a parametr a informace o proměnných jsou uvedeny v **proměnné** podokně.</span><span class="sxs-lookup"><span data-stu-id="06612-180">The code stops at the first breaking point, and the parameter and variable information are shown in the **Variables** pane.</span></span> 

3. <span data-ttu-id="06612-181">Vyberte **obnovit Program** ikonu pokračovat.</span><span class="sxs-lookup"><span data-stu-id="06612-181">Select the **Resume Program** icon to continue.</span></span> <span data-ttu-id="06612-182">Kód zastaví v druhém bodu.</span><span class="sxs-lookup"><span data-stu-id="06612-182">The code stops at the second point.</span></span> <span data-ttu-id="06612-183">Výjimka je zachycena podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="06612-183">The exception is caught as expected.</span></span>

  ![Throw – chyba](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-throw-error.png) 

4. <span data-ttu-id="06612-185">Vyberte **obnovit Program** ikonu znovu.</span><span class="sxs-lookup"><span data-stu-id="06612-185">Select the **Resume Program** icon again.</span></span> <span data-ttu-id="06612-186">**HDInsight Spark odeslání** okně se zobrazí chyba "Úloha spuštění se nepovedlo".</span><span class="sxs-lookup"><span data-stu-id="06612-186">The **HDInsight Spark Submission** window displays a "job run failed" error.</span></span>

  ![Chyba odeslání](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-error-submission.png) 

5. <span data-ttu-id="06612-188">Chcete-li dynamicky aktualizovat hodnotu proměnné pomocí IntelliJ ladění funkce, vyberte **ladění** znovu.</span><span class="sxs-lookup"><span data-stu-id="06612-188">To dynamically update the variable value by using the IntelliJ debugging capability, select **Debug** again.</span></span> <span data-ttu-id="06612-189">**Proměnné** podokně se zobrazí znovu.</span><span class="sxs-lookup"><span data-stu-id="06612-189">The **Variables** pane appears again.</span></span> 

6. <span data-ttu-id="06612-190">Klikněte pravým tlačítkem na cíl na **ladění** a pak vyberte **nastavit hodnotu**.</span><span class="sxs-lookup"><span data-stu-id="06612-190">Right-click the target on the **Debug** tab, and then select **Set Value**.</span></span> <span data-ttu-id="06612-191">Potom zadejte novou hodnotu proměnné.</span><span class="sxs-lookup"><span data-stu-id="06612-191">Next, enter a new value for the variable.</span></span> <span data-ttu-id="06612-192">Potom vyberte **Enter** uložte hodnotu.</span><span class="sxs-lookup"><span data-stu-id="06612-192">Then select **Enter** to save the value.</span></span> 

  ![Nastavte hodnotu](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-set-value.png) 

7. <span data-ttu-id="06612-194">Vyberte **obnovit Program** ikonu pokračujte ke spuštění programu.</span><span class="sxs-lookup"><span data-stu-id="06612-194">Select the **Resume Program** icon to continue to run the program.</span></span> <span data-ttu-id="06612-195">Tentokrát je žádná výjimka zachycena.</span><span class="sxs-lookup"><span data-stu-id="06612-195">This time, no exception is caught.</span></span> <span data-ttu-id="06612-196">Uvidíte, že projekt úspěšně běží bez jakékoli výjimky.</span><span class="sxs-lookup"><span data-stu-id="06612-196">You can see that the project runs successfully without any exceptions.</span></span>

  ![Ladění bez výjimky](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-without-exception.png)

## <span data-ttu-id="06612-198"><a name="seealso"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="06612-198"><a name="seealso"></a>Next steps</span></span>
* [<span data-ttu-id="06612-199">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="06612-199">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="06612-200">Ukázka</span><span class="sxs-lookup"><span data-stu-id="06612-200">Demo</span></span>
* <span data-ttu-id="06612-201">Vytvoření projektu Scala (video): [vytvoření aplikací Spark Scala](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="06612-201">Create Scala project (video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="06612-202">Vzdálené ladění (video): [pomocí nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně v clusteru HDInsight](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="06612-202">Remote debug (video): [Use Azure Toolkit for IntelliJ to debug Spark applications remotely on an HDInsight cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="06612-203">Scénáře</span><span class="sxs-lookup"><span data-stu-id="06612-203">Scenarios</span></span>
* [<span data-ttu-id="06612-204">Spark s BI: provádějte interaktivní analýzy dat pomocí Spark v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="06612-204">Spark with BI: Perform interactive data analysis by using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="06612-205">Spark s Machine Learning: používejte Spark v HDInsight pro analýzu stavební teploty pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="06612-205">Spark with Machine Learning: Use Spark in HDInsight to analyze building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="06612-206">Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin</span><span class="sxs-lookup"><span data-stu-id="06612-206">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="06612-207">Datové proudy Spark: Používejte Spark v HDInsight k sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="06612-207">Spark Streaming: Use Spark in HDInsight to build real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="06612-208">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="06612-208">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="06612-209">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="06612-209">Create and run applications</span></span>
* [<span data-ttu-id="06612-210">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="06612-210">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="06612-211">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="06612-211">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="06612-212">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="06612-212">Tools and extensions</span></span>
* [<span data-ttu-id="06612-213">Vytvoření aplikací Spark pro cluster služby HDInsight pomocí nástrojů Azure pro IntelliJ</span><span class="sxs-lookup"><span data-stu-id="06612-213">Use Azure Toolkit for IntelliJ to create Spark applications for an HDInsight cluster</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="06612-214">Použití nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně prostřednictvím sítě VPN</span><span class="sxs-lookup"><span data-stu-id="06612-214">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="06612-215">Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="06612-215">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="06612-216">Používat nástroje HDInsight pro vytvoření aplikací Spark v Azure nástrojů pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="06612-216">Use HDInsight Tools in Azure Toolkit for Eclipse to create Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="06612-217">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="06612-217">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="06612-218">Jádra dostupná pro poznámkový blok Jupyter v clusteru Spark pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="06612-218">Kernels available for Jupyter notebook in the Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="06612-219">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="06612-219">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="06612-220">Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="06612-220">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="06612-221">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="06612-221">Manage resources</span></span>
* [<span data-ttu-id="06612-222">Správa prostředků v clusteru Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="06612-222">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="06612-223">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="06612-223">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
