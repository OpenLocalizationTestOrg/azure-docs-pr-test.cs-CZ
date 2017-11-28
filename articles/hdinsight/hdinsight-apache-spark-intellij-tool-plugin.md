---
title: "Azure nástrojů pro IntelliJ: vytvoření aplikací Spark pro cluster služby HDInsight | Microsoft Docs"
description: "Použití sady nástrojů Azure pro IntelliJ k vývoji aplikací Spark napsané v jazyce Scala a odesílat je na clusteru HDInsight Spark."
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
ms.openlocfilehash: 19cb8f436fa4d86f323013a5d4b3b50bf6c80a1a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-toolkit-for-intellij-to-create-spark-applications-for-an-hdinsight-cluster"></a><span data-ttu-id="88334-103">Vytvoření aplikací Spark pro cluster služby HDInsight pomocí nástrojů Azure pro IntelliJ</span><span class="sxs-lookup"><span data-stu-id="88334-103">Use Azure Toolkit for IntelliJ to create Spark applications for an HDInsight cluster</span></span>

<span data-ttu-id="88334-104">Použití nástrojů Azure pro IntelliJ modulu plug-in k vývoji aplikací Spark napsané v jazyce Scala a odesílat je na clusteru HDInsight Spark přímo z IntelliJ integrované vývojové prostředí (IDE).</span><span class="sxs-lookup"><span data-stu-id="88334-104">Use the Azure Toolkit for IntelliJ plug-in to develop Spark applications written in Scala, and then submit them to an HDInsight Spark cluster directly from the IntelliJ integrated development environment (IDE).</span></span> <span data-ttu-id="88334-105">Modul plug-in můžete použít několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="88334-105">You can use the plug-in in a few ways:</span></span>

* <span data-ttu-id="88334-106">Vývoj a odesílání aplikací Scala Spark na clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="88334-106">Develop and submit a Scala Spark application on an HDInsight Spark cluster.</span></span>
* <span data-ttu-id="88334-107">Přístup k prostředkům clusteru Azure HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="88334-107">Access your Azure HDInsight Spark cluster resources.</span></span>
* <span data-ttu-id="88334-108">Vytvořte a spusťte aplikací Scala Spark místně.</span><span class="sxs-lookup"><span data-stu-id="88334-108">Develop and run a Scala Spark application locally.</span></span>

<span data-ttu-id="88334-109">Chcete-li vytvořit projekt, podívejte se [vytvoření aplikací Spark pomocí sady nástrojů Azure pro IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) videa.</span><span class="sxs-lookup"><span data-stu-id="88334-109">To create your project, view the [Create Spark Applications with the Azure Toolkit for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) video.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="88334-110">Tento modul plug-in můžete použít k vytvoření a odeslání aplikací pouze pro cluster služby HDInsight Spark na systému Linux.</span><span class="sxs-lookup"><span data-stu-id="88334-110">You can use this plug-in to create and submit applications only for an HDInsight Spark cluster on Linux.</span></span>
> 

## <a name="prerequisites"></a><span data-ttu-id="88334-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="88334-111">Prerequisites</span></span>

- <span data-ttu-id="88334-112">Cluster Apache Spark v HDInsight Linux.</span><span class="sxs-lookup"><span data-stu-id="88334-112">An Apache Spark cluster on HDInsight Linux.</span></span> <span data-ttu-id="88334-113">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="88334-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
- <span data-ttu-id="88334-114">Oracle Java Development Kit.</span><span class="sxs-lookup"><span data-stu-id="88334-114">Oracle Java Development Kit.</span></span> <span data-ttu-id="88334-115">Můžete nainstalovat z [Oracle webu](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="88334-115">You can install it from the [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
- <span data-ttu-id="88334-116">IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="88334-116">IntelliJ IDEA.</span></span> <span data-ttu-id="88334-117">Tento článek používá verzi 2017.1.</span><span class="sxs-lookup"><span data-stu-id="88334-117">This article uses version 2017.1.</span></span> <span data-ttu-id="88334-118">Můžete nainstalovat z [JetBrains webu](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="88334-118">You can install it from the [JetBrains website](https://www.jetbrains.com/idea/download/).</span></span>

## <a name="install-azure-toolkit-for-intellij"></a><span data-ttu-id="88334-119">Instalace Azure Toolkit pro IntelliJ</span><span class="sxs-lookup"><span data-stu-id="88334-119">Install Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="88334-120">Pokyny k instalaci naleznete v tématu [instalaci nástrojů Azure pro IntelliJ](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="88334-120">For installation instructions, see [Install Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>

## <a name="sign-in-to-your-azure-subscription"></a><span data-ttu-id="88334-121">Přihlaste se ke svému předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="88334-121">Sign in to your Azure subscription</span></span>

1. <span data-ttu-id="88334-122">Spuštění rozhraní IDE IntelliJ a otevřete Průzkumník Azure.</span><span class="sxs-lookup"><span data-stu-id="88334-122">Start the IntelliJ IDE, and open Azure Explorer.</span></span> <span data-ttu-id="88334-123">Na **zobrazení** nabídce vyberte možnost **nástroj Windows**a potom vyberte **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="88334-123">On the **View** menu, select **Tool Windows**, and then select **Azure Explorer**.</span></span>
       
   ![Průzkumník Azure odkaz](./media/hdinsight-apache-spark-intellij-tool-plugin/show-azure-explorer.png)

2. <span data-ttu-id="88334-125">Klikněte pravým tlačítkem myši **Azure** uzel a potom vyberte **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="88334-125">Right-click the **Azure** node, and then select **Sign In**.</span></span>

3. <span data-ttu-id="88334-126">V **přihlásit k Azure** dialogové okno, vyberte **přihlášení**a pak zadejte přihlašovací údaje Azure.</span><span class="sxs-lookup"><span data-stu-id="88334-126">In the **Azure Sign In** dialog box, select **Sign in**, and then enter your Azure credentials.</span></span>

    ![Dialogové okno přihlášení k Azure](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-2.png)

4. <span data-ttu-id="88334-128">Poté, co jste přihlášení, **vyberte odběry** dialogové okno zobrazí všechna předplatná Azure, které jsou spojeny s přihlašovacími údaji.</span><span class="sxs-lookup"><span data-stu-id="88334-128">After you're signed in, the **Select Subscriptions** dialog box lists all the Azure subscriptions that are associated with the credentials.</span></span> <span data-ttu-id="88334-129">Vyberte **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="88334-129">Select the **Select** button.</span></span>

    ![Dialogové okno Vybrat odběrů](./media/hdinsight-apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

5. <span data-ttu-id="88334-131">Na **Průzkumník Azure** rozbalte **HDInsight** zobrazíte clustery HDInsight Spark, které jsou v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="88334-131">On the **Azure Explorer** tab, expand **HDInsight** to view the HDInsight Spark clusters that are in your subscription.</span></span>
   
    ![Clustery HDInsight Spark v Azure Explorer](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-3.png)

6. <span data-ttu-id="88334-133">Chcete-li zobrazit prostředky (například účty úložiště), které jsou přidružené ke clusteru, můžete dále rozšířit uzlem název clusteru.</span><span class="sxs-lookup"><span data-stu-id="88334-133">To view the resources (for example, storage accounts) that are associated with the cluster, you can further expand a cluster-name node.</span></span>
   
    ![Rozbalené uzly název clusteru](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="88334-135">Spuštění aplikace na Spark Scala clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="88334-135">Run a Spark Scala application on an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="88334-136">Spusťte IntelliJ IDEA a pak vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="88334-136">Start IntelliJ IDEA, and then create a project.</span></span> <span data-ttu-id="88334-137">V **nový projekt** dialogové okno pole, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="88334-137">In the **New Project** dialog box, do the following:</span></span> 

   <span data-ttu-id="88334-138">a.</span><span class="sxs-lookup"><span data-stu-id="88334-138">a.</span></span> <span data-ttu-id="88334-139">Vyberte **HDInsight** > **Spark v HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="88334-139">Select **HDInsight** > **Spark on HDInsight (Scala)**.</span></span>

   <span data-ttu-id="88334-140">b.</span><span class="sxs-lookup"><span data-stu-id="88334-140">b.</span></span> <span data-ttu-id="88334-141">V **nástroj pro sestavení** vyberte některý z následujících akcí podle potřeby vašeho:</span><span class="sxs-lookup"><span data-stu-id="88334-141">In the **Build tool** list, select either of the following, according to your need:</span></span>

      * <span data-ttu-id="88334-142">**Maven**, podpora Průvodce vytvoření projektu Scala</span><span class="sxs-lookup"><span data-stu-id="88334-142">**Maven**, for Scala project-creation wizard support</span></span>
      * <span data-ttu-id="88334-143">**SBT**, ke správě závislosti a vytváření projektu Scala</span><span class="sxs-lookup"><span data-stu-id="88334-143">**SBT**, for managing the dependencies and building for the Scala project</span></span>

    ![Dialogové okno Nový projekt](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

2. <span data-ttu-id="88334-145">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="88334-145">Select **Next**.</span></span>

3. <span data-ttu-id="88334-146">Průvodce vytvoření projektu Scala automaticky zjišťuje, zda jste nainstalovali Scala modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="88334-146">The Scala project-creation wizard automatically detects whether you've installed the Scala plug-in.</span></span> <span data-ttu-id="88334-147">Vyberte **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="88334-147">Select **Install**.</span></span>

   ![Kontrola modul plug-in Scala](./media/hdinsight-apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. <span data-ttu-id="88334-149">Chcete-li stáhnout modul plug-in Scala, vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="88334-149">To download the Scala plug-in, select **OK**.</span></span> <span data-ttu-id="88334-150">Postupujte podle pokynů k restartování IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="88334-150">Follow the instructions to restart IntelliJ.</span></span> 

   ![Dialogové okno instalace modulu plug-in Scala](./media/hdinsight-apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. <span data-ttu-id="88334-152">V **nový projekt** okno, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="88334-152">In the **New Project** window, do the following:</span></span>  

    ![Výběr Spark SDK](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-new-project.png)

   <span data-ttu-id="88334-154">a.</span><span class="sxs-lookup"><span data-stu-id="88334-154">a.</span></span> <span data-ttu-id="88334-155">Zadejte název projektu a umístění.</span><span class="sxs-lookup"><span data-stu-id="88334-155">Enter a project name and location.</span></span>

   <span data-ttu-id="88334-156">b.</span><span class="sxs-lookup"><span data-stu-id="88334-156">b.</span></span> <span data-ttu-id="88334-157">V **SDK projektu** rozevíracího seznamu vyberte **Java 1.8** pro Spark 2.x cluster nebo vyberte **Java 1.7** 1.x clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="88334-157">In the **Project SDK** drop-down list, select **Java 1.8** for the Spark 2.x cluster, or select **Java 1.7** for the Spark 1.x cluster.</span></span>

   <span data-ttu-id="88334-158">c.</span><span class="sxs-lookup"><span data-stu-id="88334-158">c.</span></span> <span data-ttu-id="88334-159">V **Spark verze** rozevíracího seznamu, Průvodce vytvořením projektu Scala integruje správnou verzi sady SDK Spark a Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="88334-159">In the **Spark version** drop-down list, Scala project creation wizard integrates the proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="88334-160">Pokud verze clusteru Spark je starší než 2.0, vyberte **Spark 1.x**.</span><span class="sxs-lookup"><span data-stu-id="88334-160">If the Spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="88334-161">Jinak vyberte možnost **Spark2.x**.</span><span class="sxs-lookup"><span data-stu-id="88334-161">Otherwise, select **Spark2.x**.</span></span> <span data-ttu-id="88334-162">Tento příklad používá **Spark bodu 2.0.2 (Scala 2.11.8)**.</span><span class="sxs-lookup"><span data-stu-id="88334-162">This example uses **Spark 2.0.2 (Scala 2.11.8)**.</span></span>

6. <span data-ttu-id="88334-163">Vyberte **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="88334-163">Select **Finish**.</span></span>

7. <span data-ttu-id="88334-164">Projekt Spark pro vás automaticky vytvoří artefakt.</span><span class="sxs-lookup"><span data-stu-id="88334-164">The Spark project automatically creates an artifact for you.</span></span> <span data-ttu-id="88334-165">Chcete-li zobrazit artefaktu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="88334-165">To view the artifact, do the following:</span></span>

   <span data-ttu-id="88334-166">a.</span><span class="sxs-lookup"><span data-stu-id="88334-166">a.</span></span> <span data-ttu-id="88334-167">Na **soubor** nabídce vyberte možnost **strukturu projektu**.</span><span class="sxs-lookup"><span data-stu-id="88334-167">On the **File** menu, select **Project Structure**.</span></span>

   <span data-ttu-id="88334-168">b.</span><span class="sxs-lookup"><span data-stu-id="88334-168">b.</span></span> <span data-ttu-id="88334-169">V **strukturu projektu** dialogové okno, vyberte **artefakty** zobrazíte výchozí artefaktu, který je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="88334-169">In the **Project Structure** dialog box, select **Artifacts** to view the default artifact that is created.</span></span> <span data-ttu-id="88334-170">Můžete také vytvořit vlastní artefaktů výběrem na symbol plus (**+**).</span><span class="sxs-lookup"><span data-stu-id="88334-170">You can also create your own artifact by selecting the plus sign (**+**).</span></span>

      ![Informace o artefaktů v dialogovém okně](./media/hdinsight-apache-spark-intellij-tool-plugin/default-artifact.png)
      
8. <span data-ttu-id="88334-172">Přidejte zdrojový kód aplikace následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="88334-172">Add your application source code by doing the following:</span></span>

   <span data-ttu-id="88334-173">a.</span><span class="sxs-lookup"><span data-stu-id="88334-173">a.</span></span> <span data-ttu-id="88334-174">V prohlížeči projektu klikněte pravým tlačítkem na **src**, přejděte na příkaz **nový**a potom vyberte **Scala třída**.</span><span class="sxs-lookup"><span data-stu-id="88334-174">In Project Explorer, right-click **src**, point to **New**, and then select **Scala Class**.</span></span>
      
      ![Příkazy pro vytvoření třídy Scala z Project Exploreru](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   <span data-ttu-id="88334-176">b.</span><span class="sxs-lookup"><span data-stu-id="88334-176">b.</span></span> <span data-ttu-id="88334-177">V **vytvořte novou třídu Scala** dialogové okno, zadejte název, vyberte **objekt** v **druhu** a pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="88334-177">In the **Create New Scala Class** dialog box, provide a name, select **Object** in the **Kind** box, and then select **OK**.</span></span>
      
      ![Nová třída Scala dialogové okno vytvořit](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   <span data-ttu-id="88334-179">c.</span><span class="sxs-lookup"><span data-stu-id="88334-179">c.</span></span> <span data-ttu-id="88334-180">V **MyClusterApp.scala** souboru, vložte následující kód.</span><span class="sxs-lookup"><span data-stu-id="88334-180">In the **MyClusterApp.scala** file, paste the following code.</span></span> <span data-ttu-id="88334-181">Kód čte data z HVAC.csv (k dispozici na všech clusterech HDInsight Spark), načte řádky, které mají jenom jednu číslici ve sloupci sedmého v souboru CSV a zapíše výstup do **/HVACOut** v kontejneru výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="88334-181">The code reads the data from HVAC.csv (available on all HDInsight Spark clusters), retrieves the rows that have only one digit in the seventh column in the CSV file, and writes the output to **/HVACOut** under the default storage container for the cluster.</span></span>

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object MyClusterApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find the rows that have only one digit in the seventh column in the CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasb:///HVACOut")
            }
    
        }

9. <span data-ttu-id="88334-182">Spuštění aplikace na clusteru HDInsight Spark následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="88334-182">Run the application on an HDInsight Spark cluster by doing the following:</span></span>

   <span data-ttu-id="88334-183">a.</span><span class="sxs-lookup"><span data-stu-id="88334-183">a.</span></span> <span data-ttu-id="88334-184">V prohlížeči projektu klikněte pravým tlačítkem na název projektu a pak vyberte **odesílání aplikací Spark na HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="88334-184">In Project Explorer, right-click the project name, and then select **Submit Spark Application to HDInsight**.</span></span>
      
      ![Odeslání aplikací Spark na HDInsight příkaz](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

   <span data-ttu-id="88334-186">b.</span><span class="sxs-lookup"><span data-stu-id="88334-186">b.</span></span> <span data-ttu-id="88334-187">Zobrazí se výzva k zadání přihlašovacích údajů předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="88334-187">You are prompted to enter your Azure subscription credentials.</span></span> <span data-ttu-id="88334-188">V **Spark odeslání** dialogové okno, zadejte následující hodnoty a potom vyberte **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="88334-188">In the **Spark Submission** dialog box, provide the following values, and then select **Submit**.</span></span>
      
      * <span data-ttu-id="88334-189">Pro **Spark clustery (pouze Linux)**, vyberte cluster HDInsight Spark, na kterém chcete aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="88334-189">For **Spark clusters (Linux only)**, select the HDInsight Spark cluster on which you want to run your application.</span></span>

      * <span data-ttu-id="88334-190">Vyberte artefakt z projektu IntelliJ nebo vyberte jednu z pevného disku.</span><span class="sxs-lookup"><span data-stu-id="88334-190">Select an artifact from the IntelliJ project, or select one from the hard drive.</span></span>

      * <span data-ttu-id="88334-191">V **hlavní název třídy** vyberte se třemi tečkami (**...** ), vyberte hlavní třídy ve zdrojovém kódu aplikace a pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="88334-191">In the **Main class name** box, select the ellipsis (**...**), select the main class in your application source code, and then select **OK**.</span></span>

        ![Dialogové okno Vybrat hlavní – třída](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)

      * <span data-ttu-id="88334-193">Protože kódu aplikace v tomto příkladu nevyžaduje argumenty příkazového řádku ani odkazovat JAR nebo soubory, můžete zbývající pole ponechat prázdné.</span><span class="sxs-lookup"><span data-stu-id="88334-193">Because the application code in this example does not require command-line arguments or reference JARs or files, you can leave the remaining boxes empty.</span></span> <span data-ttu-id="88334-194">Po zadání všech informací, dialogové okno by měla vypadat přibližně na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="88334-194">After you provide all the information, the dialog box should resemble the following image.</span></span>
        
        ![Dialogové okno Spark odeslání](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

   <span data-ttu-id="88334-196">c.</span><span class="sxs-lookup"><span data-stu-id="88334-196">c.</span></span> <span data-ttu-id="88334-197">**Spark odeslání** karta v dolní části okna by se měl spustit zobrazení průběhu.</span><span class="sxs-lookup"><span data-stu-id="88334-197">The **Spark Submission** tab at the bottom of the window should start displaying the progress.</span></span> <span data-ttu-id="88334-198">Můžete také zastavit aplikaci tak, že vyberete červené tlačítko v **Spark odeslání** okno.</span><span class="sxs-lookup"><span data-stu-id="88334-198">You can also stop the application by selecting the red button in the **Spark Submission** window.</span></span>
      
      ![Okno Spark odeslání](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      <span data-ttu-id="88334-200">Další informace o přístupu k výstup úlohy najdete v tématu "přístup a spravovat clustery HDInsight Spark pomocí nástrojů Azure pro IntelliJ" později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="88334-200">To learn how to access the job output, see the "Access and manage HDInsight Spark clusters by using Azure Toolkit for IntelliJ" section later in this article.</span></span>

## <a name="run-or-debug-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="88334-201">Spustit nebo ladění aplikací Spark Scala na clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="88334-201">Run or debug a Spark Scala application on an HDInsight Spark cluster</span></span>
<span data-ttu-id="88334-202">Doporučujeme také jiný způsob odesílání aplikací Spark pro cluster.</span><span class="sxs-lookup"><span data-stu-id="88334-202">We also recommend another way of submitting the Spark application to the cluster.</span></span> <span data-ttu-id="88334-203">Můžete tak učinit pomocí nastavení v parametrech **konfigurace spustit/Debug** IDE.</span><span class="sxs-lookup"><span data-stu-id="88334-203">You can do so by setting the parameters in the **Run/Debug configurations** IDE.</span></span> <span data-ttu-id="88334-204">Další informace najdete v tématu [vzdálené ladění aplikací Spark v clusteru HDInsight pomocí sady Azure Toolkit pro IntelliJ prostřednictvím SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span><span class="sxs-lookup"><span data-stu-id="88334-204">For more information, see [Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span></span>

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a><span data-ttu-id="88334-205">Přístup a spravovat clustery HDInsight Spark pomocí nástrojů Azure pro IntelliJ</span><span class="sxs-lookup"><span data-stu-id="88334-205">Access and manage HDInsight Spark clusters by using Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="88334-206">Pomocí nástrojů Azure pro IntelliJ můžete provádět různé operace.</span><span class="sxs-lookup"><span data-stu-id="88334-206">You can perform various operations by using Azure Toolkit for IntelliJ.</span></span>

### <a name="access-the-job-view"></a><span data-ttu-id="88334-207">Přístup k zobrazení úloh</span><span class="sxs-lookup"><span data-stu-id="88334-207">Access the job view</span></span>
1. <span data-ttu-id="88334-208">V Průzkumníku Azure rozbalte **HDInsight**, rozbalte název clusteru Spark a pak vyberte **úlohy**.</span><span class="sxs-lookup"><span data-stu-id="88334-208">In Azure Explorer, expand **HDInsight**, expand the Spark cluster name, and then select **Jobs**.</span></span>  

    ![Úlohy zobrazení uzlu](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. <span data-ttu-id="88334-210">V pravém podokně klikněte **zobrazení úloh Spark** karta zobrazuje všechny aplikace, které byly spuštěny v clusteru.</span><span class="sxs-lookup"><span data-stu-id="88334-210">In the right pane, the **Spark Job View** tab displays all the applications that were run on the cluster.</span></span> <span data-ttu-id="88334-211">Vyberte název aplikace, pro který chcete zobrazit další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="88334-211">Select the name of the application for which you want to see more details.</span></span>

    ![Podrobnosti o aplikaci](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)

3. <span data-ttu-id="88334-213">Chcete-li zobrazit základní informace o spuštěné úlohy, pozastavte ukazatel myši nad graf úlohy.</span><span class="sxs-lookup"><span data-stu-id="88334-213">To display basic running job information, hover over the job graph.</span></span> <span data-ttu-id="88334-214">Chcete-li zobrazit graf fázích a informace, které generuje každých úlohy, vyberte uzel na graf úlohy.</span><span class="sxs-lookup"><span data-stu-id="88334-214">To view the stages graph and information that every job generates, select a node on the job graph.</span></span>

    ![Fáze podrobnosti úlohy](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. <span data-ttu-id="88334-216">Zobrazení často používat protokoly, jako např *ovladač Stderr*, *ovladač Stdout*, a *informací o adresáři*, vyberte **protokolu** karta.</span><span class="sxs-lookup"><span data-stu-id="88334-216">To view frequently used logs, such as *Driver Stderr*, *Driver Stdout*, and *Directory Info*, select the **Log** tab.</span></span>

    ![Podrobnosti protokolu](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)

5. <span data-ttu-id="88334-218">Výběrem odkaz v horní části okna můžete také zobrazit historii Spark uživatelského rozhraní a uživatelské rozhraní YARN (na úrovni aplikace).</span><span class="sxs-lookup"><span data-stu-id="88334-218">You can also view the Spark history UI and the YARN UI (at the application level) by selecting a link at the top of the window.</span></span>

### <a name="access-the-spark-history-server"></a><span data-ttu-id="88334-219">Přístup k serveru Spark historie</span><span class="sxs-lookup"><span data-stu-id="88334-219">Access the Spark history server</span></span>
1. <span data-ttu-id="88334-220">V Průzkumníku Azure rozbalte **HDInsight**, klikněte pravým tlačítkem na název clusteru Spark a pak vyberte **otevřete uživatelské rozhraní historie Spark**.</span><span class="sxs-lookup"><span data-stu-id="88334-220">In Azure Explorer, expand **HDInsight**, right-click your Spark cluster name, and then select **Open Spark History UI**.</span></span> 

2. <span data-ttu-id="88334-221">Když se zobrazí výzva, zadejte přihlašovací údaje Správce clusteru, které jste zadali při nastavování clusteru.</span><span class="sxs-lookup"><span data-stu-id="88334-221">When you're prompted, enter the cluster's admin credentials, which you specified when you set up the cluster.</span></span>

3. <span data-ttu-id="88334-222">Na řídicím panelu serveru historie Spark můžete použít název aplikace a Hledat aplikace právě dokončila spuštění.</span><span class="sxs-lookup"><span data-stu-id="88334-222">On the Spark history server dashboard, you can use the application name to look for the application that you just finished running.</span></span> <span data-ttu-id="88334-223">V předchozím kódu nastavit název aplikace pomocí `val conf = new SparkConf().setAppName("MyClusterApp")`.</span><span class="sxs-lookup"><span data-stu-id="88334-223">In the preceding code, you set the application name by using `val conf = new SparkConf().setAppName("MyClusterApp")`.</span></span> <span data-ttu-id="88334-224">Proto je název aplikace Spark **MyClusterApp**.</span><span class="sxs-lookup"><span data-stu-id="88334-224">Therefore, your Spark application name is **MyClusterApp**.</span></span>

### <a name="start-the-ambari-portal"></a><span data-ttu-id="88334-225">Spuštění portálu Ambari</span><span class="sxs-lookup"><span data-stu-id="88334-225">Start the Ambari portal</span></span>
1. <span data-ttu-id="88334-226">V Průzkumníku Azure rozbalte **HDInsight**, klikněte pravým tlačítkem na název clusteru Spark a pak vyberte **otevřete portál pro správu clusteru (Ambari)**.</span><span class="sxs-lookup"><span data-stu-id="88334-226">In Azure Explorer, expand **HDInsight**, right-click your Spark cluster name, and then select **Open Cluster Management Portal (Ambari)**.</span></span> 

2. <span data-ttu-id="88334-227">Když se zobrazí výzva, zadejte přihlašovací údaje Správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="88334-227">When you're prompted, enter the admin credentials for the cluster.</span></span> <span data-ttu-id="88334-228">Tyto přihlašovací údaje jste zadali během procesu instalace clusteru.</span><span class="sxs-lookup"><span data-stu-id="88334-228">You specified these credentials during the cluster setup process.</span></span>

### <a name="manage-azure-subscriptions"></a><span data-ttu-id="88334-229">Spravovat předplatná Azure</span><span class="sxs-lookup"><span data-stu-id="88334-229">Manage Azure subscriptions</span></span>
<span data-ttu-id="88334-230">Ve výchozím nastavení seznam nástrojů Azure pro IntelliJ clustery Spark ze všech předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="88334-230">By default, Azure Toolkit for IntelliJ lists the Spark clusters from all your Azure subscriptions.</span></span> <span data-ttu-id="88334-231">V případě potřeby můžete zadat odběry, které chcete získat přístup.</span><span class="sxs-lookup"><span data-stu-id="88334-231">If necessary, you can specify the subscriptions that you want to access.</span></span> 

1. <span data-ttu-id="88334-232">V Průzkumníku Azure, klikněte pravým tlačítkem myši **Azure** kořenový uzel a potom vyberte **Spravovat odběry**.</span><span class="sxs-lookup"><span data-stu-id="88334-232">In Azure Explorer, right-click the **Azure** root node, and then select **Manage Subscriptions**.</span></span> 

2. <span data-ttu-id="88334-233">V dialogovém okně, zrušte zaškrtnutí políček vedle odběry, které nechcete použít pro přístup a potom vyberte **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="88334-233">In the dialog box, clear the check boxes next to the subscriptions that you don't want to access, and then select **Close**.</span></span> <span data-ttu-id="88334-234">Můžete také vybrat **Odhlásit** Pokud se chcete odhlásit z vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="88334-234">You can also select **Sign Out** if you want to sign out of your Azure subscription.</span></span>

## <a name="run-a-spark-scala-application-locally"></a><span data-ttu-id="88334-235">Místní spuštění aplikace Spark Scala</span><span class="sxs-lookup"><span data-stu-id="88334-235">Run a Spark Scala application locally</span></span>
<span data-ttu-id="88334-236">Sada nástrojů Azure pro IntelliJ můžete použít ke spouštění aplikací Spark Scala místně na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="88334-236">You can use Azure Toolkit for IntelliJ to run Spark Scala applications locally on your workstation.</span></span> <span data-ttu-id="88334-237">Aplikace obvykle nepotřebují přístup k prostředkům clusteru, jako je například úložiště kontejnery, a můžete spustit a otestovat je místně.</span><span class="sxs-lookup"><span data-stu-id="88334-237">The applications usually don't need access to cluster resources, such as storage containers, and you can run and test them locally.</span></span>

### <a name="prerequisite"></a><span data-ttu-id="88334-238">Požadavek</span><span class="sxs-lookup"><span data-stu-id="88334-238">Prerequisite</span></span>
<span data-ttu-id="88334-239">Když používáte místní aplikací Spark Scala na počítači se systémem Windows, může získat výjimku, jak je popsáno v [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356).</span><span class="sxs-lookup"><span data-stu-id="88334-239">While you're running the local Spark Scala application on a Windows computer, you might get an exception, as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span></span> <span data-ttu-id="88334-240">Výjimka nastává, protože WinUtils.exe chybí v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="88334-240">The exception occurs because WinUtils.exe is missing on Windows.</span></span> 

<span data-ttu-id="88334-241">Chcete tuto chybu vyřešit [stáhnout spustitelný soubor](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) do umístění, jako **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="88334-241">To resolve this error, [download the executable](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) to a location such as **C:\WinUtils\bin**.</span></span> <span data-ttu-id="88334-242">Pak přidejte proměnnou prostředí **HADOOP_HOME**a nastavte hodnotu proměnné na **C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="88334-242">Then, add the environment variable **HADOOP_HOME**, and set the value of the variable to **C\WinUtils**.</span></span>

### <a name="run-a-local-spark-scala-application"></a><span data-ttu-id="88334-243">Spustit místních aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="88334-243">Run a local Spark Scala application</span></span>
1. <span data-ttu-id="88334-244">Spusťte IntelliJ IDEA a vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="88334-244">Start IntelliJ IDEA, and create a project.</span></span> 

2. <span data-ttu-id="88334-245">V **nový projekt** dialogové okno pole, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="88334-245">In the **New Project** dialog box, do the following:</span></span>
   
    <span data-ttu-id="88334-246">a.</span><span class="sxs-lookup"><span data-stu-id="88334-246">a.</span></span> <span data-ttu-id="88334-247">Vyberte **HDInsight** > **Spark v HDInsight místní spuštění ukázkové (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="88334-247">Select **HDInsight** > **Spark on HDInsight Local Run Sample (Scala)**.</span></span>

    <span data-ttu-id="88334-248">b.</span><span class="sxs-lookup"><span data-stu-id="88334-248">b.</span></span> <span data-ttu-id="88334-249">V **nástroj pro sestavení** vyberte některý z následujících akcí podle potřeby vašeho:</span><span class="sxs-lookup"><span data-stu-id="88334-249">In the **Build tool** list, select either of the following, according to your need:</span></span>

      * <span data-ttu-id="88334-250">**Maven**, podpora Průvodce vytvoření projektu Scala</span><span class="sxs-lookup"><span data-stu-id="88334-250">**Maven**, for Scala project-creation wizard support</span></span>
      * <span data-ttu-id="88334-251">**SBT**, ke správě závislosti a vytváření projektu Scala</span><span class="sxs-lookup"><span data-stu-id="88334-251">**SBT**, for managing the dependencies and building for the Scala project</span></span>

    ![Dialogové okno Nový projekt](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run.png)

3. <span data-ttu-id="88334-253">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="88334-253">Select **Next**.</span></span>
 
4. <span data-ttu-id="88334-254">V dalším intervalu postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="88334-254">In the next window, do the following:</span></span>
   
    <span data-ttu-id="88334-255">a.</span><span class="sxs-lookup"><span data-stu-id="88334-255">a.</span></span> <span data-ttu-id="88334-256">Zadejte název projektu a umístění.</span><span class="sxs-lookup"><span data-stu-id="88334-256">Enter a project name and location.</span></span>

    <span data-ttu-id="88334-257">b.</span><span class="sxs-lookup"><span data-stu-id="88334-257">b.</span></span> <span data-ttu-id="88334-258">V **SDK projektu** rozevíracího seznamu vyberte verzi jazyka Java, která je novější než verze 1.7.</span><span class="sxs-lookup"><span data-stu-id="88334-258">In the **Project SDK** drop-down list, select a Java version that's later than version 1.7.</span></span>

    <span data-ttu-id="88334-259">c.</span><span class="sxs-lookup"><span data-stu-id="88334-259">c.</span></span> <span data-ttu-id="88334-260">V **Spark verze** rozevíracího seznamu vyberte verzi Scala, který chcete použít: Scala 2.11.x Spark 2.0 nebo Scala 2.10.x pro Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="88334-260">In the **Spark Version** drop-down list, select the version of Scala that you want to use: Scala 2.11.x for Spark 2.0 or Scala 2.10.x for Spark 1.6.</span></span>

    ![Dialogové okno Nový projekt](./media/hdinsight-apache-spark-intellij-tool-plugin/Create-local-project.PNG)

5. <span data-ttu-id="88334-262">Vyberte **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="88334-262">Select **Finish**.</span></span>

6. <span data-ttu-id="88334-263">Ukázkový kód přidá šablona (**LogQuery**) v části **src** složky, kterou můžete spustit místně v počítači.</span><span class="sxs-lookup"><span data-stu-id="88334-263">The template adds a sample code (**LogQuery**) under the **src** folder that you can run locally on your computer.</span></span>
   
    ![Umístění LogQuery](./media/hdinsight-apache-spark-intellij-tool-plugin/local-app.png)

7. <span data-ttu-id="88334-265">Klikněte pravým tlačítkem myši **LogQuery** aplikace a pak vyberte **spustit 'LogQuery'**.</span><span class="sxs-lookup"><span data-stu-id="88334-265">Right-click the **LogQuery** application, and then select **Run 'LogQuery'**.</span></span> <span data-ttu-id="88334-266">Na **spustit** karta v dolní části, uvidíte výstup takto:</span><span class="sxs-lookup"><span data-stu-id="88334-266">On the **Run** tab at the bottom, you see an output like the following:</span></span>
   
   ![Místní aplikace Spark, výsledek spuštění](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a><span data-ttu-id="88334-268">Převést stávající aplikace IntelliJ IDEA používat sadu nástrojů Azure pro IntelliJ</span><span class="sxs-lookup"><span data-stu-id="88334-268">Convert existing IntelliJ IDEA applications to use Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="88334-269">Můžete převést stávající Spark Scala aplikace, který jste vytvořili v IntelliJ IDEA, aby byl kompatibilní s Azure nástrojů pro IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="88334-269">You can convert the existing Spark Scala applications that you created in IntelliJ IDEA to be compatible with Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="88334-270">Potom můžete modul plug-in odeslání aplikací do clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="88334-270">You can then use the plug-in to submit the applications to an HDInsight Spark cluster.</span></span>

1. <span data-ttu-id="88334-271">Pro existující Spark Scala aplikaci, která byla vytvořena prostřednictvím IntelliJ IDEA otevřete soubor přidružené .iml.</span><span class="sxs-lookup"><span data-stu-id="88334-271">For an existing Spark Scala application that was created through IntelliJ IDEA, open the associated .iml file.</span></span>

2. <span data-ttu-id="88334-272">V kořenovém adresáři úroveň je **modulu** element takto:</span><span class="sxs-lookup"><span data-stu-id="88334-272">At the root level is a **module** element like the following:</span></span>
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   <span data-ttu-id="88334-273">Upravit elementu, který chcete přidat `UniqueKey="HDInsightTool"` tak, aby **modulu** element vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="88334-273">Edit the element to add `UniqueKey="HDInsightTool"` so that the **module** element looks like the following:</span></span>
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

3. <span data-ttu-id="88334-274">Uložte změny.</span><span class="sxs-lookup"><span data-stu-id="88334-274">Save the changes.</span></span> <span data-ttu-id="88334-275">Aplikace by teď měly být kompatibilní s Azure nástrojů pro IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="88334-275">Your application should now be compatible with Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="88334-276">Můžete otestovat ji kliknutím pravým tlačítkem na název projektu v prohlížeči projektu.</span><span class="sxs-lookup"><span data-stu-id="88334-276">You can test it by right-clicking the project name in Project Explorer.</span></span> <span data-ttu-id="88334-277">V rozbalovacím má teď možnost **odesílání aplikací Spark na HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="88334-277">The pop-up menu now has the option **Submit Spark Application to HDInsight**.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="88334-278">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="88334-278">Troubleshooting</span></span>

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a><span data-ttu-id="88334-279">Chyba při místním spuštění: *použijte prosím větší velikost haldy*</span><span class="sxs-lookup"><span data-stu-id="88334-279">Error in local run: *Please use a larger heap size*</span></span>
<span data-ttu-id="88334-280">V 1.6 Spark Pokud používáte Java SDK 32-bit při místním spuštění, může dojít k následujícím chybám:</span><span class="sxs-lookup"><span data-stu-id="88334-280">In Spark 1.6, if you're using a 32-bit Java SDK during local run, you might encounter the following errors:</span></span>

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

<span data-ttu-id="88334-281">Tyto chyby dojít, protože velikost haldy není dostatečně velký pro Spark ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="88334-281">These errors happen because the heap size is not large enough for Spark to run.</span></span> <span data-ttu-id="88334-282">Spark vyžaduje alespoň 471 MB.</span><span class="sxs-lookup"><span data-stu-id="88334-282">Spark requires at least 471 MB.</span></span> <span data-ttu-id="88334-283">(Další informace najdete v tématu [SPARK 12081](https://issues.apache.org/jira/browse/SPARK-12081).) Jeden jednoduchým řešením je použití sady Java SDK 64-bit.</span><span class="sxs-lookup"><span data-stu-id="88334-283">(For more information, see [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081).) One simple solution is to use a 64-bit Java SDK.</span></span> <span data-ttu-id="88334-284">Můžete také změnit nastavení JVM v IntelliJ přidáním následujících možností:</span><span class="sxs-lookup"><span data-stu-id="88334-284">You can also change the JVM settings in IntelliJ by adding the following options:</span></span>

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Přidání možnosti do pole "Možnosti virtuálního počítače" v IntelliJ](./media/hdinsight-apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a><span data-ttu-id="88334-286">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="88334-286">FAQ</span></span>
<span data-ttu-id="88334-287">Chcete-li odeslat aplikace do Azure Data Lake Store, zvolte **interaktivní** režimu během procesu Azure přihlášení.</span><span class="sxs-lookup"><span data-stu-id="88334-287">To submit an application to Azure Data Lake Store, choose **Interactive** mode during the Azure sign-in process.</span></span> <span data-ttu-id="88334-288">Pokud vyberete **automatizovaná** režimu, můžete dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="88334-288">If you select **Automated** mode, you can get an error.</span></span>

![interaktivní přihlášení](./media/hdinsight-apache-spark-intellij-tool-plugin/interative-signin.png)

<span data-ttu-id="88334-290">Nyní jsme ho vyřešil.</span><span class="sxs-lookup"><span data-stu-id="88334-290">Now, we resolved it.</span></span> <span data-ttu-id="88334-291">Můžete použít Cluster služby Azure Data Lake odeslat vaší aplikace s libovolnou metodu přihlášení.</span><span class="sxs-lookup"><span data-stu-id="88334-291">You can choose an Azure Data Lake Cluster to submit your application with any sign-in method.</span></span>

## <a name="feedback-and-known-issues"></a><span data-ttu-id="88334-292">Názory a známé problémy</span><span class="sxs-lookup"><span data-stu-id="88334-292">Feedback and known issues</span></span>
<span data-ttu-id="88334-293">V současné době Spark výstupů zobrazení přímo není podporováno.</span><span class="sxs-lookup"><span data-stu-id="88334-293">Currently, viewing Spark outputs directly is not supported.</span></span>

<span data-ttu-id="88334-294">Pokud máte jakékoli návrhy či názory, nebo pokud se vyskytnou potíže při použití tento modul plug-in, e-mailu nás na adrese hdivstool@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="88334-294">If you have any suggestions or feedback, or if you encounter any problems when you use this plug-in, email us at hdivstool@microsoft.com.</span></span>

## <span data-ttu-id="88334-295"><a name="seealso"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="88334-295"><a name="seealso"></a>Next steps</span></span>
* [<span data-ttu-id="88334-296">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="88334-296">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="88334-297">Ukázka</span><span class="sxs-lookup"><span data-stu-id="88334-297">Demo</span></span>
* <span data-ttu-id="88334-298">Vytvoření projektu Scala (video): [vytvoření aplikací Spark Scala](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="88334-298">Create Scala project (video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="88334-299">Vzdálené ladění (video): [pomocí nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně na clusteru HDInsight](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="88334-299">Remote debug (video): [Use Azure Toolkit for IntelliJ to debug Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="88334-300">Scénáře</span><span class="sxs-lookup"><span data-stu-id="88334-300">Scenarios</span></span>
* [<span data-ttu-id="88334-301">Spark s BI: provádějte interaktivní analýzy dat pomocí Spark v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="88334-301">Spark with BI: Perform interactive data analysis by using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="88334-302">Spark s Machine Learning: používejte Spark v HDInsight pro analýzu stavební teploty pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="88334-302">Spark with Machine Learning: Use Spark in HDInsight to analyze building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="88334-303">Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin</span><span class="sxs-lookup"><span data-stu-id="88334-303">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="88334-304">Datové proudy Spark: Používejte Spark v HDInsight k sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="88334-304">Spark Streaming: Use Spark in HDInsight to build real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="88334-305">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="88334-305">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a><span data-ttu-id="88334-306">Vytváření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="88334-306">Creating and running applications</span></span>
* [<span data-ttu-id="88334-307">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="88334-307">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="88334-308">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="88334-308">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="88334-309">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="88334-309">Tools and extensions</span></span>
* [<span data-ttu-id="88334-310">Použití nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně prostřednictvím sítě VPN</span><span class="sxs-lookup"><span data-stu-id="88334-310">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="88334-311">Použití nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně přes SSH</span><span class="sxs-lookup"><span data-stu-id="88334-311">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="88334-312">Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="88334-312">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="88334-313">Používat nástroje HDInsight pro vytvoření aplikací Spark v Azure nástrojů pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="88334-313">Use HDInsight Tools in Azure Toolkit for Eclipse to create Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="88334-314">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="88334-314">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="88334-315">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="88334-315">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="88334-316">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="88334-316">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="88334-317">Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="88334-317">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a><span data-ttu-id="88334-318">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="88334-318">Managing resources</span></span>
* [<span data-ttu-id="88334-319">Správa prostředků v clusteru Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="88334-319">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="88334-320">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="88334-320">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

