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
# <a name="use-azure-toolkit-for-intellij-toocreate-spark-applications-for-an-hdinsight-cluster"></a><span data-ttu-id="84a29-103">Použití nástrojů Azure pro IntelliJ toocreate aplikací Spark pro cluster služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="84a29-103">Use Azure Toolkit for IntelliJ toocreate Spark applications for an HDInsight cluster</span></span>

<span data-ttu-id="84a29-104">Použijte hello Azure Toolkit pro IntelliJ modulu plug-in toodevelop Spark aplikace napsané v jazyce Scala a odesílat je tooan clusteru HDInsight Spark přímo z hello IntelliJ integrované vývojové prostředí (IDE).</span><span class="sxs-lookup"><span data-stu-id="84a29-104">Use hello Azure Toolkit for IntelliJ plug-in toodevelop Spark applications written in Scala, and then submit them tooan HDInsight Spark cluster directly from hello IntelliJ integrated development environment (IDE).</span></span> <span data-ttu-id="84a29-105">Můžete použít hello modulu plug-in několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="84a29-105">You can use hello plug-in in a few ways:</span></span>

* <span data-ttu-id="84a29-106">Vývoj a odesílání aplikací Scala Spark na clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="84a29-106">Develop and submit a Scala Spark application on an HDInsight Spark cluster.</span></span>
* <span data-ttu-id="84a29-107">Přístup k prostředkům clusteru Azure HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="84a29-107">Access your Azure HDInsight Spark cluster resources.</span></span>
* <span data-ttu-id="84a29-108">Vytvořte a spusťte aplikací Scala Spark místně.</span><span class="sxs-lookup"><span data-stu-id="84a29-108">Develop and run a Scala Spark application locally.</span></span>

<span data-ttu-id="84a29-109">toocreate projektu, zobrazení hello [vytvoření aplikací Spark s hello nástrojů Azure pro IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) videa.</span><span class="sxs-lookup"><span data-stu-id="84a29-109">toocreate your project, view hello [Create Spark Applications with hello Azure Toolkit for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) video.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="84a29-110">Můžete použít tento modul plug-in toocreate a odeslání aplikací pouze pro cluster služby HDInsight Spark na systému Linux.</span><span class="sxs-lookup"><span data-stu-id="84a29-110">You can use this plug-in toocreate and submit applications only for an HDInsight Spark cluster on Linux.</span></span>
> 

## <a name="prerequisites"></a><span data-ttu-id="84a29-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="84a29-111">Prerequisites</span></span>

- <span data-ttu-id="84a29-112">Cluster Apache Spark v HDInsight Linux.</span><span class="sxs-lookup"><span data-stu-id="84a29-112">An Apache Spark cluster on HDInsight Linux.</span></span> <span data-ttu-id="84a29-113">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="84a29-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
- <span data-ttu-id="84a29-114">Oracle Java Development Kit.</span><span class="sxs-lookup"><span data-stu-id="84a29-114">Oracle Java Development Kit.</span></span> <span data-ttu-id="84a29-115">Můžete ji nainstalovat ze hello [Oracle webu](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="84a29-115">You can install it from hello [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
- <span data-ttu-id="84a29-116">IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="84a29-116">IntelliJ IDEA.</span></span> <span data-ttu-id="84a29-117">Tento článek používá verzi 2017.1.</span><span class="sxs-lookup"><span data-stu-id="84a29-117">This article uses version 2017.1.</span></span> <span data-ttu-id="84a29-118">Můžete ji nainstalovat ze hello [JetBrains webu](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="84a29-118">You can install it from hello [JetBrains website](https://www.jetbrains.com/idea/download/).</span></span>

## <a name="install-azure-toolkit-for-intellij"></a><span data-ttu-id="84a29-119">Instalace Azure Toolkit pro IntelliJ</span><span class="sxs-lookup"><span data-stu-id="84a29-119">Install Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="84a29-120">Pokyny k instalaci naleznete v tématu [instalaci nástrojů Azure pro IntelliJ](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="84a29-120">For installation instructions, see [Install Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>

## <a name="sign-in-tooyour-azure-subscription"></a><span data-ttu-id="84a29-121">Přihlaste se tooyour předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="84a29-121">Sign in tooyour Azure subscription</span></span>

1. <span data-ttu-id="84a29-122">Spusťte hello IntelliJ IDE a otevřete Průzkumník Azure.</span><span class="sxs-lookup"><span data-stu-id="84a29-122">Start hello IntelliJ IDE, and open Azure Explorer.</span></span> <span data-ttu-id="84a29-123">Na hello **zobrazení** nabídce vyberte možnost **nástroj Windows**a potom vyberte **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="84a29-123">On hello **View** menu, select **Tool Windows**, and then select **Azure Explorer**.</span></span>
       
   ![Průzkumník Azure odkaz Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/show-azure-explorer.png)

2. <span data-ttu-id="84a29-125">Klikněte pravým tlačítkem na hello **Azure** uzel a potom vyberte **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="84a29-125">Right-click hello **Azure** node, and then select **Sign In**.</span></span>

3. <span data-ttu-id="84a29-126">V hello **přihlásit k Azure** dialogové okno, vyberte **přihlášení**a pak zadejte přihlašovací údaje Azure.</span><span class="sxs-lookup"><span data-stu-id="84a29-126">In hello **Azure Sign In** dialog box, select **Sign in**, and then enter your Azure credentials.</span></span>

    ![Hello přihlásit k Azure dialogové okno](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-2.png)

4. <span data-ttu-id="84a29-128">Poté, co jste přihlášení, hello **vyberte odběry** zobrazí dialogové okno pole všechny hello předplatná Azure, které jsou přidružené hello přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="84a29-128">After you're signed in, hello **Select Subscriptions** dialog box lists all hello Azure subscriptions that are associated with hello credentials.</span></span> <span data-ttu-id="84a29-129">Vyberte hello **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="84a29-129">Select hello **Select** button.</span></span>

    ![Dialogové okno Vybrat odběry Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

5. <span data-ttu-id="84a29-131">Na hello **Azure Explorer** rozbalte **HDInsight** tooview hello clustery HDInsight Spark, které jsou v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="84a29-131">On hello **Azure Explorer** tab, expand **HDInsight** tooview hello HDInsight Spark clusters that are in your subscription.</span></span>
   
    ![Clustery HDInsight Spark v Azure Explorer](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-3.png)

6. <span data-ttu-id="84a29-133">tooview hello prostředky (například účty úložiště) přidružených hello clusteru, můžete dále rozšířit uzlem název clusteru.</span><span class="sxs-lookup"><span data-stu-id="84a29-133">tooview hello resources (for example, storage accounts) that are associated with hello cluster, you can further expand a cluster-name node.</span></span>
   
    ![Rozbalené uzly název clusteru](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="84a29-135">Spuštění aplikace na Spark Scala clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="84a29-135">Run a Spark Scala application on an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="84a29-136">Spusťte IntelliJ IDEA a pak vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="84a29-136">Start IntelliJ IDEA, and then create a project.</span></span> <span data-ttu-id="84a29-137">V hello **nový projekt** dialogové okno pole, hello následující:</span><span class="sxs-lookup"><span data-stu-id="84a29-137">In hello **New Project** dialog box, do hello following:</span></span> 

   <span data-ttu-id="84a29-138">a.</span><span class="sxs-lookup"><span data-stu-id="84a29-138">a.</span></span> <span data-ttu-id="84a29-139">Vyberte **HDInsight** > **Spark v HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="84a29-139">Select **HDInsight** > **Spark on HDInsight (Scala)**.</span></span>

   <span data-ttu-id="84a29-140">b.</span><span class="sxs-lookup"><span data-stu-id="84a29-140">b.</span></span> <span data-ttu-id="84a29-141">V hello **nástroj pro sestavení** vyberte jednu z následujících, podle potřeby tooyour hello:</span><span class="sxs-lookup"><span data-stu-id="84a29-141">In hello **Build tool** list, select either of hello following, according tooyour need:</span></span>

      * <span data-ttu-id="84a29-142">**Maven**, podpora Průvodce vytvoření projektu Scala</span><span class="sxs-lookup"><span data-stu-id="84a29-142">**Maven**, for Scala project-creation wizard support</span></span>
      * <span data-ttu-id="84a29-143">**SBT**, pro správu hello závislosti a sestavování pro projekt hello Scala</span><span class="sxs-lookup"><span data-stu-id="84a29-143">**SBT**, for managing hello dependencies and building for hello Scala project</span></span>

    ![Dialogové okno Nový projekt Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

2. <span data-ttu-id="84a29-145">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="84a29-145">Select **Next**.</span></span>

3. <span data-ttu-id="84a29-146">Průvodce vytvoření projektu Scala Hello automaticky zjišťuje, zda jste nainstalovali hello Scala modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="84a29-146">hello Scala project-creation wizard automatically detects whether you've installed hello Scala plug-in.</span></span> <span data-ttu-id="84a29-147">Vyberte **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="84a29-147">Select **Install**.</span></span>

   ![Kontrola modul plug-in Scala](./media/hdinsight-apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. <span data-ttu-id="84a29-149">hello toodownload Scala modul plug-in, vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="84a29-149">toodownload hello Scala plug-in, select **OK**.</span></span> <span data-ttu-id="84a29-150">Postupujte podle pokynů toorestart hello IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="84a29-150">Follow hello instructions toorestart IntelliJ.</span></span> 

   ![Dialogové okno modul plug-in Instalace Hello Scala](./media/hdinsight-apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. <span data-ttu-id="84a29-152">V hello **nový projekt** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="84a29-152">In hello **New Project** window, do hello following:</span></span>  

    ![Výběr hello Spark SDK](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-new-project.png)

   <span data-ttu-id="84a29-154">a.</span><span class="sxs-lookup"><span data-stu-id="84a29-154">a.</span></span> <span data-ttu-id="84a29-155">Zadejte název projektu a umístění.</span><span class="sxs-lookup"><span data-stu-id="84a29-155">Enter a project name and location.</span></span>

   <span data-ttu-id="84a29-156">b.</span><span class="sxs-lookup"><span data-stu-id="84a29-156">b.</span></span> <span data-ttu-id="84a29-157">V hello **SDK projektu** rozevíracího seznamu vyberte **Java 1.8** hello Spark 2.x clusteru, nebo vyberte **Java 1.7** hello Spark 1.x clusteru.</span><span class="sxs-lookup"><span data-stu-id="84a29-157">In hello **Project SDK** drop-down list, select **Java 1.8** for hello Spark 2.x cluster, or select **Java 1.7** for hello Spark 1.x cluster.</span></span>

   <span data-ttu-id="84a29-158">c.</span><span class="sxs-lookup"><span data-stu-id="84a29-158">c.</span></span> <span data-ttu-id="84a29-159">V hello **Spark verze** rozevíracího seznamu, Průvodce vytvořením projektu Scala integruje hello správnou verzi sady SDK Spark a Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="84a29-159">In hello **Spark version** drop-down list, Scala project creation wizard integrates hello proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="84a29-160">Pokud verze clusteru Spark hello je starší než 2.0, vyberte **Spark 1.x**.</span><span class="sxs-lookup"><span data-stu-id="84a29-160">If hello Spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="84a29-161">Jinak vyberte možnost **Spark2.x**.</span><span class="sxs-lookup"><span data-stu-id="84a29-161">Otherwise, select **Spark2.x**.</span></span> <span data-ttu-id="84a29-162">Tento příklad používá **Spark bodu 2.0.2 (Scala 2.11.8)**.</span><span class="sxs-lookup"><span data-stu-id="84a29-162">This example uses **Spark 2.0.2 (Scala 2.11.8)**.</span></span>

6. <span data-ttu-id="84a29-163">Vyberte **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="84a29-163">Select **Finish**.</span></span>

7. <span data-ttu-id="84a29-164">projekt Hello Spark pro vás automaticky vytvoří artefakt.</span><span class="sxs-lookup"><span data-stu-id="84a29-164">hello Spark project automatically creates an artifact for you.</span></span> <span data-ttu-id="84a29-165">tooview hello artefaktů, hello následující:</span><span class="sxs-lookup"><span data-stu-id="84a29-165">tooview hello artifact, do hello following:</span></span>

   <span data-ttu-id="84a29-166">a.</span><span class="sxs-lookup"><span data-stu-id="84a29-166">a.</span></span> <span data-ttu-id="84a29-167">Na hello **soubor** nabídce vyberte možnost **strukturu projektu**.</span><span class="sxs-lookup"><span data-stu-id="84a29-167">On hello **File** menu, select **Project Structure**.</span></span>

   <span data-ttu-id="84a29-168">b.</span><span class="sxs-lookup"><span data-stu-id="84a29-168">b.</span></span> <span data-ttu-id="84a29-169">V hello **strukturu projektu** dialogové okno, vyberte **artefakty** tooview hello výchozí artefaktů, který je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="84a29-169">In hello **Project Structure** dialog box, select **Artifacts** tooview hello default artifact that is created.</span></span> <span data-ttu-id="84a29-170">Můžete také vytvořit vlastní artefaktů výběrem hello – znaménko plus (**+**).</span><span class="sxs-lookup"><span data-stu-id="84a29-170">You can also create your own artifact by selecting hello plus sign (**+**).</span></span>

      ![Informace o artefaktů v dialogovém okně hello](./media/hdinsight-apache-spark-intellij-tool-plugin/default-artifact.png)
      
8. <span data-ttu-id="84a29-172">Přidejte zdrojový kód aplikace pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="84a29-172">Add your application source code by doing hello following:</span></span>

   <span data-ttu-id="84a29-173">a.</span><span class="sxs-lookup"><span data-stu-id="84a29-173">a.</span></span> <span data-ttu-id="84a29-174">V prohlížeči projektu klikněte pravým tlačítkem na **src**, bod příliš**nový**a potom vyberte **Scala třída**.</span><span class="sxs-lookup"><span data-stu-id="84a29-174">In Project Explorer, right-click **src**, point too**New**, and then select **Scala Class**.</span></span>
      
      ![Příkazy pro vytvoření třídy Scala z Project Exploreru](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   <span data-ttu-id="84a29-176">b.</span><span class="sxs-lookup"><span data-stu-id="84a29-176">b.</span></span> <span data-ttu-id="84a29-177">V hello **vytvořte novou třídu Scala** dialogové okno, zadejte název, vyberte **objekt** v hello **druhu** a pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="84a29-177">In hello **Create New Scala Class** dialog box, provide a name, select **Object** in hello **Kind** box, and then select **OK**.</span></span>
      
      ![Nová třída Scala dialogové okno vytvořit](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   <span data-ttu-id="84a29-179">c.</span><span class="sxs-lookup"><span data-stu-id="84a29-179">c.</span></span> <span data-ttu-id="84a29-180">V hello **MyClusterApp.scala** souboru, vložte následující kód hello.</span><span class="sxs-lookup"><span data-stu-id="84a29-180">In hello **MyClusterApp.scala** file, paste hello following code.</span></span> <span data-ttu-id="84a29-181">Hello kód čte hello data z HVAC.csv (k dispozici na všech clusterech HDInsight Spark), načte hello řádky, které mají jenom jednu číslici hello sedmého sloupce v souboru CSV hello a zapíše výstup hello příliš**/HVACOut** pod výchozí hello Kontejner úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="84a29-181">hello code reads hello data from HVAC.csv (available on all HDInsight Spark clusters), retrieves hello rows that have only one digit in hello seventh column in hello CSV file, and writes hello output too**/HVACOut** under hello default storage container for hello cluster.</span></span>

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

9. <span data-ttu-id="84a29-182">Spuštění aplikace hello na clusteru HDInsight Spark pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="84a29-182">Run hello application on an HDInsight Spark cluster by doing hello following:</span></span>

   <span data-ttu-id="84a29-183">a.</span><span class="sxs-lookup"><span data-stu-id="84a29-183">a.</span></span> <span data-ttu-id="84a29-184">V prohlížeči projektu klikněte pravým tlačítkem na název projektu hello a pak vyberte **odesílání aplikací Spark tooHDInsight**.</span><span class="sxs-lookup"><span data-stu-id="84a29-184">In Project Explorer, right-click hello project name, and then select **Submit Spark Application tooHDInsight**.</span></span>
      
      ![Hello příkaz tooHDInsight odesílání aplikací Spark](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

   <span data-ttu-id="84a29-186">b.</span><span class="sxs-lookup"><span data-stu-id="84a29-186">b.</span></span> <span data-ttu-id="84a29-187">Můžete se výzvami tooenter přihlašovací údaje předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="84a29-187">You are prompted tooenter your Azure subscription credentials.</span></span> <span data-ttu-id="84a29-188">V hello **Spark odeslání** dialogové okno, zadejte následující hodnoty hello a pak vyberte **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="84a29-188">In hello **Spark Submission** dialog box, provide hello following values, and then select **Submit**.</span></span>
      
      * <span data-ttu-id="84a29-189">Pro **Spark clustery (pouze Linux)**, vyberte hello clusteru HDInsight Spark, na kterém chcete toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="84a29-189">For **Spark clusters (Linux only)**, select hello HDInsight Spark cluster on which you want toorun your application.</span></span>

      * <span data-ttu-id="84a29-190">Artefakt z hello IntelliJ projektu, nebo vyberte jednu z pevného disku hello.</span><span class="sxs-lookup"><span data-stu-id="84a29-190">Select an artifact from hello IntelliJ project, or select one from hello hard drive.</span></span>

      * <span data-ttu-id="84a29-191">V hello **hlavní název třídy** pole, vyberte hello třemi tečkami (**...** ), vyberte hello hlavní třídy ve zdrojovém kódu aplikace a pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="84a29-191">In hello **Main class name** box, select hello ellipsis (**...**), select hello main class in your application source code, and then select **OK**.</span></span>

        ![Dialogové okno Vybrat hlavní třída Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)

      * <span data-ttu-id="84a29-193">Protože kód aplikace hello v tomto příkladu nevyžaduje argumenty příkazového řádku ani odkazovat JAR nebo soubory, můžete ponechat hello zbývající polí prázdné.</span><span class="sxs-lookup"><span data-stu-id="84a29-193">Because hello application code in this example does not require command-line arguments or reference JARs or files, you can leave hello remaining boxes empty.</span></span> <span data-ttu-id="84a29-194">Po zadání všech informací hello dialogové okno hello by měla vypadat přibližně hello následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="84a29-194">After you provide all hello information, hello dialog box should resemble hello following image.</span></span>
        
        ![Dialogové okno Spark odeslání Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

   <span data-ttu-id="84a29-196">c.</span><span class="sxs-lookup"><span data-stu-id="84a29-196">c.</span></span> <span data-ttu-id="84a29-197">Hello **Spark odeslání** kartě v hello dolní části okna hello by se měl spustit zobrazování průběhu hello.</span><span class="sxs-lookup"><span data-stu-id="84a29-197">hello **Spark Submission** tab at hello bottom of hello window should start displaying hello progress.</span></span> <span data-ttu-id="84a29-198">Můžete také zastavit aplikace hello výběrem hello červené tlačítko v hello **Spark odeslání** okno.</span><span class="sxs-lookup"><span data-stu-id="84a29-198">You can also stop hello application by selecting hello red button in hello **Spark Submission** window.</span></span>
      
      ![okno Spark odeslání Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      <span data-ttu-id="84a29-200">toolearn o tooaccess hello výstup úlohy v tématu hello "přístup a spravovat clustery HDInsight Spark pomocí nástrojů Azure pro IntelliJ" později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="84a29-200">toolearn how tooaccess hello job output, see hello "Access and manage HDInsight Spark clusters by using Azure Toolkit for IntelliJ" section later in this article.</span></span>

## <a name="run-or-debug-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="84a29-201">Spustit nebo ladění aplikací Spark Scala na clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="84a29-201">Run or debug a Spark Scala application on an HDInsight Spark cluster</span></span>
<span data-ttu-id="84a29-202">Doporučujeme také jiný způsob odesílání hello Spark aplikace toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="84a29-202">We also recommend another way of submitting hello Spark application toohello cluster.</span></span> <span data-ttu-id="84a29-203">Můžete tak učinit nastavením hello parametry v hello **konfigurace spustit/Debug** IDE.</span><span class="sxs-lookup"><span data-stu-id="84a29-203">You can do so by setting hello parameters in hello **Run/Debug configurations** IDE.</span></span> <span data-ttu-id="84a29-204">Další informace najdete v tématu [vzdálené ladění aplikací Spark v clusteru HDInsight pomocí sady Azure Toolkit pro IntelliJ prostřednictvím SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span><span class="sxs-lookup"><span data-stu-id="84a29-204">For more information, see [Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span></span>

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a><span data-ttu-id="84a29-205">Přístup a spravovat clustery HDInsight Spark pomocí nástrojů Azure pro IntelliJ</span><span class="sxs-lookup"><span data-stu-id="84a29-205">Access and manage HDInsight Spark clusters by using Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="84a29-206">Pomocí nástrojů Azure pro IntelliJ můžete provádět různé operace.</span><span class="sxs-lookup"><span data-stu-id="84a29-206">You can perform various operations by using Azure Toolkit for IntelliJ.</span></span>

### <a name="access-hello-job-view"></a><span data-ttu-id="84a29-207">Zobrazení úloh hello přístup</span><span class="sxs-lookup"><span data-stu-id="84a29-207">Access hello job view</span></span>
1. <span data-ttu-id="84a29-208">V Průzkumníku Azure rozbalte **HDInsight**, rozbalte název clusteru Spark hello a pak vyberte **úlohy**.</span><span class="sxs-lookup"><span data-stu-id="84a29-208">In Azure Explorer, expand **HDInsight**, expand hello Spark cluster name, and then select **Jobs**.</span></span>  

    ![Úlohy zobrazení uzlu](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. <span data-ttu-id="84a29-210">V pravém podokně hello hello **zobrazení úloh Spark** karta zobrazuje všechny hello aplikace, které byly spuštěny v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="84a29-210">In hello right pane, hello **Spark Job View** tab displays all hello applications that were run on hello cluster.</span></span> <span data-ttu-id="84a29-211">Vyberte název hello hello aplikace, pro které chcete toosee další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="84a29-211">Select hello name of hello application for which you want toosee more details.</span></span>

    ![Podrobnosti o aplikaci](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)

3. <span data-ttu-id="84a29-213">toodisplay základní spuštěné úlohy informace, hover přes graf úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="84a29-213">toodisplay basic running job information, hover over hello job graph.</span></span> <span data-ttu-id="84a29-214">tooview hello fázích grafu a informace, které generuje každých úlohy, vyberte uzel na graf úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="84a29-214">tooview hello stages graph and information that every job generates, select a node on hello job graph.</span></span>

    ![Fáze podrobnosti úlohy](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. <span data-ttu-id="84a29-216">tooview často používané protokoly, jako například *ovladač Stderr*, *ovladač Stdout*, a *informací o adresáři*, vyberte hello **protokolu** karta.</span><span class="sxs-lookup"><span data-stu-id="84a29-216">tooview frequently used logs, such as *Driver Stderr*, *Driver Stdout*, and *Directory Info*, select hello **Log** tab.</span></span>

    ![Podrobnosti protokolu](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)

5. <span data-ttu-id="84a29-218">Můžete také zobrazit hello Spark historie uživatelského rozhraní a hello YARN uživatelského rozhraní (na úrovni aplikace hello) tak, že vyberete odkaz hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="84a29-218">You can also view hello Spark history UI and hello YARN UI (at hello application level) by selecting a link at hello top of hello window.</span></span>

### <a name="access-hello-spark-history-server"></a><span data-ttu-id="84a29-219">Přístup k serveru historie Spark hello</span><span class="sxs-lookup"><span data-stu-id="84a29-219">Access hello Spark history server</span></span>
1. <span data-ttu-id="84a29-220">V Průzkumníku Azure rozbalte **HDInsight**, klikněte pravým tlačítkem na název clusteru Spark a pak vyberte **otevřete uživatelské rozhraní historie Spark**.</span><span class="sxs-lookup"><span data-stu-id="84a29-220">In Azure Explorer, expand **HDInsight**, right-click your Spark cluster name, and then select **Open Spark History UI**.</span></span> 

2. <span data-ttu-id="84a29-221">Když se zobrazí výzva, zadejte přihlašovací údaje správce hello clusteru, které jste zadali při nastavení hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="84a29-221">When you're prompted, enter hello cluster's admin credentials, which you specified when you set up hello cluster.</span></span>

3. <span data-ttu-id="84a29-222">Na hello Spark historie řídicího panelu serveru můžete použít toolook název aplikace hello hello aplikace právě dokončila spuštění.</span><span class="sxs-lookup"><span data-stu-id="84a29-222">On hello Spark history server dashboard, you can use hello application name toolook for hello application that you just finished running.</span></span> <span data-ttu-id="84a29-223">V předchozích kód hello, nastavte název aplikace hello pomocí `val conf = new SparkConf().setAppName("MyClusterApp")`.</span><span class="sxs-lookup"><span data-stu-id="84a29-223">In hello preceding code, you set hello application name by using `val conf = new SparkConf().setAppName("MyClusterApp")`.</span></span> <span data-ttu-id="84a29-224">Proto je název aplikace Spark **MyClusterApp**.</span><span class="sxs-lookup"><span data-stu-id="84a29-224">Therefore, your Spark application name is **MyClusterApp**.</span></span>

### <a name="start-hello-ambari-portal"></a><span data-ttu-id="84a29-225">Spustit portál Ambari hello</span><span class="sxs-lookup"><span data-stu-id="84a29-225">Start hello Ambari portal</span></span>
1. <span data-ttu-id="84a29-226">V Průzkumníku Azure rozbalte **HDInsight**, klikněte pravým tlačítkem na název clusteru Spark a pak vyberte **otevřete portál pro správu clusteru (Ambari)**.</span><span class="sxs-lookup"><span data-stu-id="84a29-226">In Azure Explorer, expand **HDInsight**, right-click your Spark cluster name, and then select **Open Cluster Management Portal (Ambari)**.</span></span> 

2. <span data-ttu-id="84a29-227">Když se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="84a29-227">When you're prompted, enter hello admin credentials for hello cluster.</span></span> <span data-ttu-id="84a29-228">Tyto přihlašovací údaje jste zadali během procesu instalace clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="84a29-228">You specified these credentials during hello cluster setup process.</span></span>

### <a name="manage-azure-subscriptions"></a><span data-ttu-id="84a29-229">Spravovat předplatná Azure</span><span class="sxs-lookup"><span data-stu-id="84a29-229">Manage Azure subscriptions</span></span>
<span data-ttu-id="84a29-230">Ve výchozím nastavení seznam nástrojů Azure pro IntelliJ clustery Spark hello ze všech předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="84a29-230">By default, Azure Toolkit for IntelliJ lists hello Spark clusters from all your Azure subscriptions.</span></span> <span data-ttu-id="84a29-231">V případě potřeby můžete zadat hello odběry, které chcete tooaccess.</span><span class="sxs-lookup"><span data-stu-id="84a29-231">If necessary, you can specify hello subscriptions that you want tooaccess.</span></span> 

1. <span data-ttu-id="84a29-232">V Průzkumníku Azure, klikněte pravým tlačítkem na hello **Azure** kořenový uzel a potom vyberte **Spravovat odběry**.</span><span class="sxs-lookup"><span data-stu-id="84a29-232">In Azure Explorer, right-click hello **Azure** root node, and then select **Manage Subscriptions**.</span></span> 

2. <span data-ttu-id="84a29-233">V dialogovém okně hello zrušte hello zaškrtávací políčka další toohello odběry, nemáte má tooaccess a potom vyberte **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="84a29-233">In hello dialog box, clear hello check boxes next toohello subscriptions that you don't want tooaccess, and then select **Close**.</span></span> <span data-ttu-id="84a29-234">Můžete také vybrat **Odhlásit** Pokud chcete toosign mimo vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="84a29-234">You can also select **Sign Out** if you want toosign out of your Azure subscription.</span></span>

## <a name="run-a-spark-scala-application-locally"></a><span data-ttu-id="84a29-235">Místní spuštění aplikace Spark Scala</span><span class="sxs-lookup"><span data-stu-id="84a29-235">Run a Spark Scala application locally</span></span>
<span data-ttu-id="84a29-236">Sada nástrojů Azure aplikací Spark Scala toorun IntelliJ můžete použít místně na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="84a29-236">You can use Azure Toolkit for IntelliJ toorun Spark Scala applications locally on your workstation.</span></span> <span data-ttu-id="84a29-237">Hello aplikace obvykle nebudete potřebovat přístup k toocluster prostředky, například kontejnery úložiště, a můžete spustit a otestovat je místně.</span><span class="sxs-lookup"><span data-stu-id="84a29-237">hello applications usually don't need access toocluster resources, such as storage containers, and you can run and test them locally.</span></span>

### <a name="prerequisite"></a><span data-ttu-id="84a29-238">Požadavek</span><span class="sxs-lookup"><span data-stu-id="84a29-238">Prerequisite</span></span>
<span data-ttu-id="84a29-239">Když spouštíte na počítači se systémem Windows hello místních aplikací Spark Scala, může získat výjimku, jak je popsáno v [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356).</span><span class="sxs-lookup"><span data-stu-id="84a29-239">While you're running hello local Spark Scala application on a Windows computer, you might get an exception, as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span></span> <span data-ttu-id="84a29-240">protože WinUtils.exe chybí v systému Windows došlo k výjimce Hello.</span><span class="sxs-lookup"><span data-stu-id="84a29-240">hello exception occurs because WinUtils.exe is missing on Windows.</span></span> 

<span data-ttu-id="84a29-241">tooresolve této chybě [stáhnout hello spustitelné](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa umístění, jako **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="84a29-241">tooresolve this error, [download hello executable](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa location such as **C:\WinUtils\bin**.</span></span> <span data-ttu-id="84a29-242">Pak přidejte proměnnou prostředí hello **HADOOP_HOME**a nastavte hodnotu hello hello proměnné příliš**C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="84a29-242">Then, add hello environment variable **HADOOP_HOME**, and set hello value of hello variable too**C\WinUtils**.</span></span>

### <a name="run-a-local-spark-scala-application"></a><span data-ttu-id="84a29-243">Spustit místních aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="84a29-243">Run a local Spark Scala application</span></span>
1. <span data-ttu-id="84a29-244">Spusťte IntelliJ IDEA a vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="84a29-244">Start IntelliJ IDEA, and create a project.</span></span> 

2. <span data-ttu-id="84a29-245">V hello **nový projekt** dialogové okno pole, hello následující:</span><span class="sxs-lookup"><span data-stu-id="84a29-245">In hello **New Project** dialog box, do hello following:</span></span>
   
    <span data-ttu-id="84a29-246">a.</span><span class="sxs-lookup"><span data-stu-id="84a29-246">a.</span></span> <span data-ttu-id="84a29-247">Vyberte **HDInsight** > **Spark v HDInsight místní spuštění ukázkové (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="84a29-247">Select **HDInsight** > **Spark on HDInsight Local Run Sample (Scala)**.</span></span>

    <span data-ttu-id="84a29-248">b.</span><span class="sxs-lookup"><span data-stu-id="84a29-248">b.</span></span> <span data-ttu-id="84a29-249">V hello **nástroj pro sestavení** vyberte jednu z následujících, podle potřeby tooyour hello:</span><span class="sxs-lookup"><span data-stu-id="84a29-249">In hello **Build tool** list, select either of hello following, according tooyour need:</span></span>

      * <span data-ttu-id="84a29-250">**Maven**, podpora Průvodce vytvoření projektu Scala</span><span class="sxs-lookup"><span data-stu-id="84a29-250">**Maven**, for Scala project-creation wizard support</span></span>
      * <span data-ttu-id="84a29-251">**SBT**, pro správu hello závislosti a sestavování pro projekt hello Scala</span><span class="sxs-lookup"><span data-stu-id="84a29-251">**SBT**, for managing hello dependencies and building for hello Scala project</span></span>

    ![Dialogové okno Nový projekt Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run.png)

3. <span data-ttu-id="84a29-253">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="84a29-253">Select **Next**.</span></span>
 
4. <span data-ttu-id="84a29-254">V dalším okně hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="84a29-254">In hello next window, do hello following:</span></span>
   
    <span data-ttu-id="84a29-255">a.</span><span class="sxs-lookup"><span data-stu-id="84a29-255">a.</span></span> <span data-ttu-id="84a29-256">Zadejte název projektu a umístění.</span><span class="sxs-lookup"><span data-stu-id="84a29-256">Enter a project name and location.</span></span>

    <span data-ttu-id="84a29-257">b.</span><span class="sxs-lookup"><span data-stu-id="84a29-257">b.</span></span> <span data-ttu-id="84a29-258">V hello **SDK projektu** rozevíracího seznamu vyberte verzi jazyka Java, která je novější než verze 1.7.</span><span class="sxs-lookup"><span data-stu-id="84a29-258">In hello **Project SDK** drop-down list, select a Java version that's later than version 1.7.</span></span>

    <span data-ttu-id="84a29-259">c.</span><span class="sxs-lookup"><span data-stu-id="84a29-259">c.</span></span> <span data-ttu-id="84a29-260">V hello **Spark verze** rozevíracího seznamu, vyberte hello verze Scala, které chcete toouse: Scala 2.11.x Spark 2.0 nebo Scala 2.10.x pro Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="84a29-260">In hello **Spark Version** drop-down list, select hello version of Scala that you want toouse: Scala 2.11.x for Spark 2.0 or Scala 2.10.x for Spark 1.6.</span></span>

    ![Dialogové okno Nový projekt Hello](./media/hdinsight-apache-spark-intellij-tool-plugin/Create-local-project.PNG)

5. <span data-ttu-id="84a29-262">Vyberte **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="84a29-262">Select **Finish**.</span></span>

6. <span data-ttu-id="84a29-263">Ukázkový kód přidá šablona Hello (**LogQuery**) v části hello **src** složky, kterou můžete spustit místně v počítači.</span><span class="sxs-lookup"><span data-stu-id="84a29-263">hello template adds a sample code (**LogQuery**) under hello **src** folder that you can run locally on your computer.</span></span>
   
    ![Umístění LogQuery](./media/hdinsight-apache-spark-intellij-tool-plugin/local-app.png)

7. <span data-ttu-id="84a29-265">Klikněte pravým tlačítkem na hello **LogQuery** aplikace a pak vyberte **spustit 'LogQuery'**.</span><span class="sxs-lookup"><span data-stu-id="84a29-265">Right-click hello **LogQuery** application, and then select **Run 'LogQuery'**.</span></span> <span data-ttu-id="84a29-266">Na hello **spustit** karta v dolní části hello, uvidíte výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="84a29-266">On hello **Run** tab at hello bottom, you see an output like hello following:</span></span>
   
   ![Místní aplikace Spark, výsledek spuštění](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="convert-existing-intellij-idea-applications-toouse-azure-toolkit-for-intellij"></a><span data-ttu-id="84a29-268">Převést stávající IntelliJ IDEA toouse aplikace Azure Toolkit pro IntelliJ</span><span class="sxs-lookup"><span data-stu-id="84a29-268">Convert existing IntelliJ IDEA applications toouse Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="84a29-269">Hello existující Spark Scala aplikace, které jste vytvořili v IntelliJ IDEA toobe kompatibilní s Azure Toolkit pro IntelliJ můžete převést.</span><span class="sxs-lookup"><span data-stu-id="84a29-269">You can convert hello existing Spark Scala applications that you created in IntelliJ IDEA toobe compatible with Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="84a29-270">Potom můžete hello modulu plug-in toosubmit hello aplikace tooan clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="84a29-270">You can then use hello plug-in toosubmit hello applications tooan HDInsight Spark cluster.</span></span>

1. <span data-ttu-id="84a29-271">Pro existující Spark Scala aplikaci, která byla vytvořena prostřednictvím IntelliJ IDEA otevřete soubor přidružené .iml hello.</span><span class="sxs-lookup"><span data-stu-id="84a29-271">For an existing Spark Scala application that was created through IntelliJ IDEA, open hello associated .iml file.</span></span>

2. <span data-ttu-id="84a29-272">Na hello nejnižší úroveň je **modulu** element jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="84a29-272">At hello root level is a **module** element like hello following:</span></span>
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   <span data-ttu-id="84a29-273">Upravit hello element tooadd `UniqueKey="HDInsightTool"` , který hello **modulu** element vypadá hello následující:</span><span class="sxs-lookup"><span data-stu-id="84a29-273">Edit hello element tooadd `UniqueKey="HDInsightTool"` so that hello **module** element looks like hello following:</span></span>
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

3. <span data-ttu-id="84a29-274">Uložte změny hello.</span><span class="sxs-lookup"><span data-stu-id="84a29-274">Save hello changes.</span></span> <span data-ttu-id="84a29-275">Aplikace by teď měly být kompatibilní s Azure nástrojů pro IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="84a29-275">Your application should now be compatible with Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="84a29-276">Můžete otestovat ji kliknutím pravým tlačítkem na název projektu hello v prohlížeči projektu.</span><span class="sxs-lookup"><span data-stu-id="84a29-276">You can test it by right-clicking hello project name in Project Explorer.</span></span> <span data-ttu-id="84a29-277">místní nabídky Hello má teď možnost hello **odesílání aplikací Spark tooHDInsight**.</span><span class="sxs-lookup"><span data-stu-id="84a29-277">hello pop-up menu now has hello option **Submit Spark Application tooHDInsight**.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="84a29-278">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="84a29-278">Troubleshooting</span></span>

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a><span data-ttu-id="84a29-279">Chyba při místním spuštění: *použijte prosím větší velikost haldy*</span><span class="sxs-lookup"><span data-stu-id="84a29-279">Error in local run: *Please use a larger heap size*</span></span>
<span data-ttu-id="84a29-280">V 1.6 Spark Pokud používáte Java SDK 32-bit při místním spuštění, může dojít hello následujícím chybám:</span><span class="sxs-lookup"><span data-stu-id="84a29-280">In Spark 1.6, if you're using a 32-bit Java SDK during local run, you might encounter hello following errors:</span></span>

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

<span data-ttu-id="84a29-281">Tyto chyby dojít, protože velikost haldy hello není dostatečně velký pro Spark toorun.</span><span class="sxs-lookup"><span data-stu-id="84a29-281">These errors happen because hello heap size is not large enough for Spark toorun.</span></span> <span data-ttu-id="84a29-282">Spark vyžaduje alespoň 471 MB.</span><span class="sxs-lookup"><span data-stu-id="84a29-282">Spark requires at least 471 MB.</span></span> <span data-ttu-id="84a29-283">(Další informace najdete v tématu [SPARK 12081](https://issues.apache.org/jira/browse/SPARK-12081).) Jeden jednoduchým řešením je toouse Java SDK 64-bit.</span><span class="sxs-lookup"><span data-stu-id="84a29-283">(For more information, see [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081).) One simple solution is toouse a 64-bit Java SDK.</span></span> <span data-ttu-id="84a29-284">Můžete také změnit nastavení JVM hello v IntelliJ přidáním hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="84a29-284">You can also change hello JVM settings in IntelliJ by adding hello following options:</span></span>

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Přidání možnosti toohello v IntelliJ pole "Možnosti virtuálního počítače"](./media/hdinsight-apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a><span data-ttu-id="84a29-286">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="84a29-286">FAQ</span></span>
<span data-ttu-id="84a29-287">Zvolte toosubmit aplikaci tooAzure Data Lake Store, **interaktivní** režimu během procesu hello Azure přihlášení.</span><span class="sxs-lookup"><span data-stu-id="84a29-287">toosubmit an application tooAzure Data Lake Store, choose **Interactive** mode during hello Azure sign-in process.</span></span> <span data-ttu-id="84a29-288">Pokud vyberete **automatizovaná** režimu, můžete dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="84a29-288">If you select **Automated** mode, you can get an error.</span></span>

![interaktivní přihlášení](./media/hdinsight-apache-spark-intellij-tool-plugin/interative-signin.png)

<span data-ttu-id="84a29-290">Nyní jsme ho vyřešil.</span><span class="sxs-lookup"><span data-stu-id="84a29-290">Now, we resolved it.</span></span> <span data-ttu-id="84a29-291">Můžete použít Azure Data Lake clusteru toosubmit vaší aplikace pomocí libovolné metody přihlášení.</span><span class="sxs-lookup"><span data-stu-id="84a29-291">You can choose an Azure Data Lake Cluster toosubmit your application with any sign-in method.</span></span>

## <a name="feedback-and-known-issues"></a><span data-ttu-id="84a29-292">Názory a známé problémy</span><span class="sxs-lookup"><span data-stu-id="84a29-292">Feedback and known issues</span></span>
<span data-ttu-id="84a29-293">V současné době Spark výstupů zobrazení přímo není podporováno.</span><span class="sxs-lookup"><span data-stu-id="84a29-293">Currently, viewing Spark outputs directly is not supported.</span></span>

<span data-ttu-id="84a29-294">Pokud máte jakékoli návrhy či názory, nebo pokud se vyskytnou potíže při použití tento modul plug-in, e-mailu nás na adrese hdivstool@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="84a29-294">If you have any suggestions or feedback, or if you encounter any problems when you use this plug-in, email us at hdivstool@microsoft.com.</span></span>

## <span data-ttu-id="84a29-295"><a name="seealso"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="84a29-295"><a name="seealso"></a>Next steps</span></span>
* [<span data-ttu-id="84a29-296">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="84a29-296">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="84a29-297">Ukázka</span><span class="sxs-lookup"><span data-stu-id="84a29-297">Demo</span></span>
* <span data-ttu-id="84a29-298">Vytvoření projektu Scala (video): [vytvoření aplikací Spark Scala](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="84a29-298">Create Scala project (video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="84a29-299">Vzdálené ladění (video): [pomocí nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně na clusteru HDInsight](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="84a29-299">Remote debug (video): [Use Azure Toolkit for IntelliJ toodebug Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="84a29-300">Scénáře</span><span class="sxs-lookup"><span data-stu-id="84a29-300">Scenarios</span></span>
* [<span data-ttu-id="84a29-301">Spark s BI: provádějte interaktivní analýzy dat pomocí Spark v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="84a29-301">Spark with BI: Perform interactive data analysis by using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="84a29-302">Spark s Machine Learning: používejte Spark v HDInsight tooanalyze vytváření teploty pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="84a29-302">Spark with Machine Learning: Use Spark in HDInsight tooanalyze building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="84a29-303">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="84a29-303">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="84a29-304">Datové proudy Spark: Používejte Spark v HDInsight toobuild v reálném čase streamování aplikací</span><span class="sxs-lookup"><span data-stu-id="84a29-304">Spark Streaming: Use Spark in HDInsight toobuild real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="84a29-305">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="84a29-305">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a><span data-ttu-id="84a29-306">Vytváření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="84a29-306">Creating and running applications</span></span>
* [<span data-ttu-id="84a29-307">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="84a29-307">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="84a29-308">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="84a29-308">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="84a29-309">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="84a29-309">Tools and extensions</span></span>
* [<span data-ttu-id="84a29-310">Použití nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně prostřednictvím sítě VPN</span><span class="sxs-lookup"><span data-stu-id="84a29-310">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="84a29-311">Použití nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně přes SSH</span><span class="sxs-lookup"><span data-stu-id="84a29-311">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="84a29-312">Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="84a29-312">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="84a29-313">Použití nástrojů HDInsight v Azure nástrojů Eclipse toocreate Spark aplikací</span><span class="sxs-lookup"><span data-stu-id="84a29-313">Use HDInsight Tools in Azure Toolkit for Eclipse toocreate Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="84a29-314">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="84a29-314">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="84a29-315">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="84a29-315">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="84a29-316">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="84a29-316">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="84a29-317">Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="84a29-317">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a><span data-ttu-id="84a29-318">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="84a29-318">Managing resources</span></span>
* [<span data-ttu-id="84a29-319">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="84a29-319">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="84a29-320">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="84a29-320">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

