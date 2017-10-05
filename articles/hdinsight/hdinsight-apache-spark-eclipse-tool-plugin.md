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
# <a name="use-azure-toolkit-for-eclipse-to-create-spark-applications-for-an-hdinsight-cluster"></a><span data-ttu-id="6a5b1-103">Vytvoření aplikací Spark pro cluster služby HDInsight pomocí nástrojů Azure pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="6a5b1-103">Use Azure Toolkit for Eclipse to create Spark applications for an HDInsight cluster</span></span>

<span data-ttu-id="6a5b1-104">Pomocí nástrojů HDInsight v Azure nástrojů pro Eclipse k vývoji aplikací Spark napsané v jazyce Scala a odesílat je na clusteru Azure HDInsight Spark, přímo z integrovaného vývojového prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-104">Use HDInsight Tools in Azure Toolkit for Eclipse to develop Spark applications written in Scala and submit them to an Azure HDInsight Spark cluster, directly from the Eclipse IDE.</span></span> <span data-ttu-id="6a5b1-105">Můžete použít nástroje HDInsight modulu plug-in několika různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="6a5b1-105">You can use the HDInsight Tools plug-in in a few different ways:</span></span>

* <span data-ttu-id="6a5b1-106">K vývoji a odesílání aplikací Scala Spark na clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="6a5b1-106">To develop and submit a Scala Spark application on an HDInsight Spark cluster</span></span>
* <span data-ttu-id="6a5b1-107">K přístupu k prostředkům clusteru Azure HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="6a5b1-107">To access your Azure HDInsight Spark cluster resources</span></span>
* <span data-ttu-id="6a5b1-108">K vývoji a místní spuštění aplikace Scala Spark</span><span class="sxs-lookup"><span data-stu-id="6a5b1-108">To develop and run a Scala Spark application locally</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6a5b1-109">Tento nástroj slouží k vytvoření a odeslání aplikací pouze pro cluster služby HDInsight Spark na systému Linux.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-109">This tool can be used to create and submit applications only for an HDInsight Spark cluster on Linux.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="6a5b1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6a5b1-110">Prerequisites</span></span>

* <span data-ttu-id="6a5b1-111">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-111">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="6a5b1-112">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="6a5b1-112">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="6a5b1-113">Oracle Java Development Kit verze 8, který se používá pro modul runtime Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-113">Oracle Java Development Kit version 8, which is used for the Eclipse IDE runtime.</span></span> <span data-ttu-id="6a5b1-114">Si můžete stáhnout z [Oracle webu](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="6a5b1-114">You can download it from the [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="6a5b1-115">Integrované vývojové prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-115">Eclipse IDE.</span></span> <span data-ttu-id="6a5b1-116">Tento článek používá Neónová Eclipse.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-116">This article uses Eclipse Neon.</span></span> <span data-ttu-id="6a5b1-117">Můžete nainstalovat z [Eclipse webu](https://www.eclipse.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6a5b1-117">You can install it from the [Eclipse website](https://www.eclipse.org/downloads/).</span></span>   
* <span data-ttu-id="6a5b1-118">Spark SDK.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-118">Spark SDK.</span></span> <span data-ttu-id="6a5b1-119">Si můžete stáhnout z [Githubu](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="6a5b1-119">You can download it from [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).</span></span>


## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-scala-plugin"></a><span data-ttu-id="6a5b1-120">Instalace nástrojů HDInsight v Azure nástrojů Eclipse a modulů plug-in Scala</span><span class="sxs-lookup"><span data-stu-id="6a5b1-120">Install HDInsight Tools in Azure Toolkit for Eclipse and Scala Plugin</span></span>
### <a name="install-hdinsight-tools"></a><span data-ttu-id="6a5b1-121">Instalace nástrojů HDInsight</span><span class="sxs-lookup"><span data-stu-id="6a5b1-121">Install HDInsight Tools</span></span>
<span data-ttu-id="6a5b1-122">Nástroje HDInsight pro Eclipse je k dispozici jako součást nástrojů Azure pro prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-122">HDInsight Tools for Eclipse is available as part of Azure Toolkit for Eclipse.</span></span> <span data-ttu-id="6a5b1-123">Pokyny k instalaci naleznete v tématu [instalace nástrojů Azure pro Eclipse](../azure-toolkit-for-eclipse-installation.md).</span><span class="sxs-lookup"><span data-stu-id="6a5b1-123">For installation instructions, see [Installing Azure Toolkit for Eclipse](../azure-toolkit-for-eclipse-installation.md).</span></span>
### <a name="install-scala-plugin"></a><span data-ttu-id="6a5b1-124">Instalace modulu plug-in Scala</span><span class="sxs-lookup"><span data-stu-id="6a5b1-124">Install Scala Plugin</span></span>
<span data-ttu-id="6a5b1-125">Při otevření Intellij nástroje HDInsight automaticky zjistí, zda jste nainstalovali modul plug-in Scala nebo ne.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-125">When you open the Intellij, the HDInsight Tools auto detects whether you installed Scala plugin or not.</span></span> <span data-ttu-id="6a5b1-126">Klikněte na tlačítko **OK** chcete pokračovat a postupujte podle pokynů a nainstalovat tak, že v prostředí Eclipse Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-126">Click **OK** to continue and follow the instructions to install by the Eclipse Marketplace.</span></span>

 ![Modul plug-in Scala automatické instalace](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala.png)

## <a name="sign-in-to-your-azure-subscription"></a><span data-ttu-id="6a5b1-128">Přihlaste se ke svému předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-128">Sign in to your Azure subscription</span></span>
1. <span data-ttu-id="6a5b1-129">Spusťte Eclipse IDE a otevřete Průzkumník Azure.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-129">Start the Eclipse IDE and open Azure Explorer.</span></span> <span data-ttu-id="6a5b1-130">Na **okno** nabídky, klikněte na tlačítko **zobrazit zobrazení**a potom klikněte na **jiných**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-130">On the **Window** menu, click **Show View**, and then click **Other**.</span></span> <span data-ttu-id="6a5b1-131">V dialogovém okně, které se otevře, rozbalte položku **Azure**, klikněte na tlačítko **Azure Explorer**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-131">In the dialog box that opens, expand **Azure**, click **Azure Explorer**, and then click **OK**.</span></span>

    ![Zobrazit dialogové okno zobrazení](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-1.png)
2. <span data-ttu-id="6a5b1-133">Klikněte pravým tlačítkem myši **Azure** uzel a pak klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-133">Right-click the **Azure** node, and then click **Sign in**.</span></span>
3. <span data-ttu-id="6a5b1-134">V **přihlásit k Azure** dialogové okno pole, vyberte metodu ověřování, klikněte na **přihlášení**a zadejte přihlašovací údaje Azure.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-134">In the **Azure Sign In** dialog box, choose the authentication method, click **Sign in**, and enter your Azure credentials.</span></span>
   
    ![Azure přihlašovací dialogové okno](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-2.png)
4. <span data-ttu-id="6a5b1-136">Poté, co jste přihlášení, **vyberte odběry** dialogové okno zobrazí všechna předplatná Azure přidružená pověření.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-136">After you're signed in, the **Select Subscriptions** dialog box lists all the Azure subscriptions associated with the credentials.</span></span> <span data-ttu-id="6a5b1-137">Klikněte na tlačítko **vyberte** zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-137">Click **Select** to close the dialog box.</span></span>

    ![Odběry dialogové okno Vybrat](./media/hdinsight-apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
5. <span data-ttu-id="6a5b1-139">Na **Průzkumník Azure** rozbalte **HDInsight** zobrazíte clusterů HDInsight Spark v rámci svého předplatného.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-139">On the **Azure Explorer** tab, expand **HDInsight** to see the HDInsight Spark clusters under your subscription.</span></span>
   
    ![Clustery HDInsight Spark v Azure Explorer](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-3.png)
6. <span data-ttu-id="6a5b1-141">Název uzlu clusteru zobrazíte prostředky (například účty úložiště) přidružen ke clusteru můžete dále rozšířit.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-141">You can further expand a cluster name node to see the resources (for example, storage accounts) associated with the cluster.</span></span>
   
    ![Rozšiřování název clusteru, který najdete v části prostředky](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-4.png)



## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a><span data-ttu-id="6a5b1-143">Nastavení projektu pro cluster služby HDInsight Spark Spark Scala</span><span class="sxs-lookup"><span data-stu-id="6a5b1-143">Set up a Spark Scala project for an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="6a5b1-144">V pracovním prostoru Eclipse IDE klikněte na tlačítko **soubor**, klikněte na tlačítko **nový**a potom klikněte na **projektu**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-144">In the Eclipse IDE workspace, click **File**, click **New**, and then click **Project**.</span></span> 
2. <span data-ttu-id="6a5b1-145">V průvodci Nový projekt rozbalte **HDInsight**, vyberte **Spark v HDInsight (Scala)**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-145">In the New Project wizard, expand **HDInsight**, select **Spark on HDInsight (Scala)**, and then click **Next**.</span></span>

    ![Výběr Spark v HDInsight (Scala) projektu](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
3. <span data-ttu-id="6a5b1-147">Scala projektu vytvoření průvodce automaticky zjistí, zda jste nainstalovali modul plug-in Scala nebo ne.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-147">The Scala project creation wizard auto detects whether you installed Scala plugin or not.</span></span> <span data-ttu-id="6a5b1-148">Klikněte na tlačítko **OK** pokračovat ve stahování modulu plug-in Scala, pak postupujte podle pokynů k restartování prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-148">Click **OK** to continue downloading the Scala plugin, then follow the instructions to restart Eclipse.</span></span>

    ![Kontrola scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
4. <span data-ttu-id="6a5b1-150">V **nový projekt Scala HDInsight** dialogové okno, zadejte následující hodnoty a pak klikněte na tlačítko **Další**:</span><span class="sxs-lookup"><span data-stu-id="6a5b1-150">In the **New HDInsight Scala Project** dialog box, provide the following values, and then click **Next**:</span></span>
   * <span data-ttu-id="6a5b1-151">Zadejte název projektu.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-151">Enter a name for the project.</span></span>
   * <span data-ttu-id="6a5b1-152">V **prostředí JRE** oblasti, ujistěte se, že **používání spuštění prostředí JRE** je nastaven na **JavaSE 1.7** nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-152">In the **JRE** area, make sure that **Use an execution environment JRE** is set to **JavaSE-1.7** or later.</span></span>
   * <span data-ttu-id="6a5b1-153">Ujistěte se, že Spark SDK je nastavená na umístění, kam jste stáhli sadu SDK.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-153">Make sure that Spark SDK is set to the location where you downloaded the SDK.</span></span> <span data-ttu-id="6a5b1-154">Je součástí odkaz k umístění stahování [požadavky](#prerequisites) výše v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-154">The link to the download location is included in the [prerequisites](#prerequisites) earlier in this article.</span></span> <span data-ttu-id="6a5b1-155">Sady SDK můžete také stáhnout z odkazu součástí dialogu.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-155">You can also download the SDK from the link included in the dialog box.</span></span>

    ![Dialogové okno Nový projekt HDInsight Scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
5.  <span data-ttu-id="6a5b1-157">V dialogovém okně Další klikněte **knihovny** kartě a ponechejte výchozí hodnoty a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-157">In the next dialog box, click the **Libraries** tab and keep the defaults, and then click **Finish**.</span></span> 
   
    ![Karta knihovny](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-4.png)
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a><span data-ttu-id="6a5b1-159">Vytvořit aplikaci pro cluster služby HDInsight Spark Scala</span><span class="sxs-lookup"><span data-stu-id="6a5b1-159">Create a Scala application for an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="6a5b1-160">V integrovaném vývojovém prostředí Eclipse, z Průzkumníka balíčku, rozbalte projekt, který jste předtím vytvořili, klikněte pravým tlačítkem na **src**, přejděte na příkaz **nový**a potom klikněte na **jiných**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-160">In the Eclipse IDE, from Package Explorer, expand the project that you created earlier, right-click **src**, point to **New**, and then click **Other**.</span></span>
2. <span data-ttu-id="6a5b1-161">V **vyberte Průvodce** dialogové okno, rozbalte seznam **Scala průvodců**, klikněte na tlačítko **Scala objekt**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-161">In the **Select a wizard** dialog box, expand **Scala Wizards**, click **Scala Object**, and then click **Next**.</span></span>
   
    ![Vyberte dialogové okno Průvodce](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
3. <span data-ttu-id="6a5b1-163">V **vytvořit nový soubor** dialogové okno, zadejte název pro objekt a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-163">In the **Create New File** dialog box, enter a name for the object, and then click **Finish**.</span></span>
   
    ![Vytvořit nový soubor – dialogové okno](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
4. <span data-ttu-id="6a5b1-165">V textovém editoru, vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="6a5b1-165">Paste the following code in the text editor:</span></span>
   
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
5. <span data-ttu-id="6a5b1-166">Spuštění aplikace na clusteru HDInsight Spark:</span><span class="sxs-lookup"><span data-stu-id="6a5b1-166">Run the application on an HDInsight Spark cluster:</span></span>
   
   1. <span data-ttu-id="6a5b1-167">V Průzkumníku balíčku, klikněte pravým tlačítkem na název projektu a potom vyberte **odesílání aplikací Spark na HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-167">From Package Explorer, right-click the project name, and then select **Submit Spark Application to HDInsight**.</span></span>        
   2. <span data-ttu-id="6a5b1-168">V **Spark odeslání** dialogové okno, zadejte následující hodnoty a pak klikněte na tlačítko **odeslání**:</span><span class="sxs-lookup"><span data-stu-id="6a5b1-168">In the **Spark Submission** dialog box, provide the following values, and then click **Submit**:</span></span>
      
      * <span data-ttu-id="6a5b1-169">Pro **název clusteru**, vyberte cluster HDInsight Spark, na kterém chcete aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-169">For **Cluster Name**, select the HDInsight Spark cluster on which you want to run your application.</span></span>
      * <span data-ttu-id="6a5b1-170">Artefakt z projektu Eclipse, nebo vyberte jednu z pevného disku.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-170">Select an artifact from the Eclipse project, or select one from a hard drive.</span></span> <span data-ttu-id="6a5b1-171">Výchozí hodnota závisí na položku, kterou klikněte pravým tlačítkem z Průzkumníka balíčku.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-171">The default value depends on the item you right-click from package explorer.</span></span>
      * <span data-ttu-id="6a5b1-172">V **hlavní název třídy** rozevírací seznam, odeslání Průvodce zobrazí všechny názvy objektů z vybraného projektu.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-172">In the **Main class name** dropdownlist, submission wizard displays all object names from your selected project.</span></span> <span data-ttu-id="6a5b1-173">Vyberte nebo zadejte ten, který chcete spustit.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-173">Select or input one that you want to run.</span></span> <span data-ttu-id="6a5b1-174">Pokud vyberete artefaktů z pevného disku, je nutné zadat název hlavní třídy sami podle.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-174">If you select artifact from hard disk, you need input main class name by yourself.</span></span> 
      * <span data-ttu-id="6a5b1-175">Protože kód aplikace v tomto příkladu nevyžaduje žádných argumentů příkazového řádku ani odkazovat JAR nebo soubory, můžete zbývající textová pole ponechat prázdné.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-175">Because the application code in this example does not require any command-line arguments or reference JARs or files, you can leave the remaining text boxes empty.</span></span>
        
       ![Dialogové okno Spark odeslání](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
   3. <span data-ttu-id="6a5b1-177">**Spark odeslání** karta by se měl spustit zobrazení průběhu.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-177">The **Spark Submission** tab should start displaying the progress.</span></span> <span data-ttu-id="6a5b1-178">Aplikace můžete ukončit kliknutím na červené tlačítko v **Spark odeslání** okno.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-178">You can stop the application by clicking the red button in the **Spark Submission** window.</span></span> <span data-ttu-id="6a5b1-179">Můžete také zobrazit protokoly pro tuto konkrétní aplikaci spustit kliknutím na ikonu zeměkouli (označen pole blue obrázek).</span><span class="sxs-lookup"><span data-stu-id="6a5b1-179">You can also view the logs for this specific application run by clicking the globe icon (denoted by the blue box in the image).</span></span>
      
       ![Okno Spark odeslání](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a><span data-ttu-id="6a5b1-181">Přístup a spravovat clustery HDInsight Spark pomocí nástrojů HDInsight v Azure nástrojů pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="6a5b1-181">Access and manage HDInsight Spark clusters by using HDInsight Tools in Azure Toolkit for Eclipse</span></span>
<span data-ttu-id="6a5b1-182">Můžete provádět různé operace pomocí nástroje HDInsight, včetně přístupu k výstupu úlohy.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-182">You can perform various operations by using HDInsight Tools, including accessing the job output.</span></span>

### <a name="access-the-job-view"></a><span data-ttu-id="6a5b1-183">Přístup k zobrazení úloh</span><span class="sxs-lookup"><span data-stu-id="6a5b1-183">Access the job view</span></span>
1. <span data-ttu-id="6a5b1-184">V Průzkumníku Azure rozbalte **HDInsight**, rozbalte název clusteru Spark a pak klikněte na tlačítko **úlohy**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-184">In Azure Explorer, expand **HDInsight**, expand the Spark cluster name, and then click **Jobs**.</span></span> 

    ![Úlohy zobrazení uzlu](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. <span data-ttu-id="6a5b1-186">Klikněte na **úlohy** uzlu.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-186">Click on the **Jobs** node.</span></span> <span data-ttu-id="6a5b1-187">Nástroje HDInsight automaticky zjišťuje, zda jste nainstalovali modul plug-in clipse E (fx) nebo ne.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-187">The HDInsight Tools auto-detects whether you installed the E(fx)clipse plugin or not.</span></span> <span data-ttu-id="6a5b1-188">Klikněte na tlačítko **OK** pokračovat a postupujte podle pokynů k instalaci v prostředí Eclipse Marketplace a restartování prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-188">Click **OK** to continue and follow the instructions to install the Eclipse Marketplace and restart Eclipse.</span></span>

    ![Nainstalujte clipse E (fx)](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

3. <span data-ttu-id="6a5b1-190">Otevřete zobrazení úlohy **úlohy** uzlu.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-190">Open the Job View from the **Jobs** node.</span></span> <span data-ttu-id="6a5b1-191">V pravém podokně klikněte **zobrazení úloh Spark** karta zobrazuje všechny aplikace, které byly spuštěny v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-191">In the right pane, the **Spark Job View** tab displays all the applications that were run on the cluster.</span></span> <span data-ttu-id="6a5b1-192">Klikněte na název aplikace, pro který chcete zobrazit další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-192">Click the name of the application for which you want to see more details.</span></span>

    ![Podrobnosti o aplikaci](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)
4. <span data-ttu-id="6a5b1-194">Pokud jste při přechodu myší na graf úlohy, zobrazuje základní informace o spuštěné úlohy.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-194">If you hover on the job graph, it displays basic running job info.</span></span> <span data-ttu-id="6a5b1-195">Kliknutím na graf úlohy zobrazuje graf fázích a informací, které generuje každých úlohy.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-195">Clicking on the job graph shows the stages graph and info that every job generates.</span></span>

    ![Fáze podrobnosti úlohy](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

5. <span data-ttu-id="6a5b1-197">Často používané protokoly, včetně ovladačů Stderr, ovladač Stdout a informací o adresáři, jsou uvedeny v **protokolu** kartě.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-197">Frequently used logs, including Driver Stderr, Driver Stdout, and Directory Info, are listed in the **Log** tab.</span></span>

    ![Podrobnosti protokolu](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)
6. <span data-ttu-id="6a5b1-199">Kliknutím na odpovídající odkaz v horní části okna můžete také otevřít historie Spark uživatelského rozhraní a uživatelské rozhraní YARN (na úrovni aplikace).</span><span class="sxs-lookup"><span data-stu-id="6a5b1-199">You can also open the Spark history UI and the YARN UI (at the application level) by clicking the respective hyperlink at the top of the window.</span></span>

### <a name="access-the-storage-container-for-the-cluster"></a><span data-ttu-id="6a5b1-200">Přístup k kontejner úložiště pro cluster</span><span class="sxs-lookup"><span data-stu-id="6a5b1-200">Access the storage container for the cluster</span></span>
1. <span data-ttu-id="6a5b1-201">V Průzkumníku Azure, rozbalte **HDInsight** kořenový uzel zobrazíte seznam clustery HDInsight Spark, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-201">In Azure Explorer, expand the **HDInsight** root node to see a list of HDInsight Spark clusters that are available.</span></span>
2. <span data-ttu-id="6a5b1-202">Rozbalte název clusteru účet úložiště a výchozí kontejner úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-202">Expand the cluster name to see the storage account and the default storage container for the cluster.</span></span>
   
    ![Kontejner úložiště účet a výchozí úložiště](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-5.png)
3. <span data-ttu-id="6a5b1-204">Klikněte na název kontejneru úložiště, který je přidružen ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-204">Click the storage container name associated with the cluster.</span></span> <span data-ttu-id="6a5b1-205">V pravém podokně dvakrát klikněte na **HVACOut** složky.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-205">In the right pane, double-click the **HVACOut** folder.</span></span> <span data-ttu-id="6a5b1-206">Otevřete jednu z **část -** soubory, které chcete zobrazit výstup aplikace.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-206">Open one of the **part-** files to see the output of the application.</span></span>

### <a name="access-the-spark-history-server"></a><span data-ttu-id="6a5b1-207">Přístup k serveru Spark historie</span><span class="sxs-lookup"><span data-stu-id="6a5b1-207">Access the Spark history server</span></span>
1. <span data-ttu-id="6a5b1-208">V Průzkumníku Azure, klikněte pravým tlačítkem na název clusteru Spark a pak vyberte **otevřete uživatelské rozhraní historie Spark**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-208">In Azure Explorer, right-click your Spark cluster name, and then select **Open Spark History UI**.</span></span> <span data-ttu-id="6a5b1-209">Když se zobrazí výzva, zadejte přihlašovací údaje Správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-209">When you're prompted, enter the admin credentials for the cluster.</span></span> <span data-ttu-id="6a5b1-210">Musí být zadána tyto při zřizování clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-210">You must have specified these while provisioning the cluster.</span></span>
2. <span data-ttu-id="6a5b1-211">V řídicím panelu Spark historie serveru použijte tento název aplikace a Hledat aplikace právě dokončila spuštění.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-211">In the Spark history server dashboard, you use the application name to look for the application that you just finished running.</span></span> <span data-ttu-id="6a5b1-212">V předchozím kódu nastavit název aplikace pomocí `val conf = new SparkConf().setAppName("MyClusterApp")`.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-212">In the preceding code, you set the application name by using `val conf = new SparkConf().setAppName("MyClusterApp")`.</span></span> <span data-ttu-id="6a5b1-213">Proto se název aplikace Spark **MyClusterApp**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-213">Hence, your Spark application name was **MyClusterApp**.</span></span>

### <a name="start-the-ambari-portal"></a><span data-ttu-id="6a5b1-214">Spuštění portálu Ambari</span><span class="sxs-lookup"><span data-stu-id="6a5b1-214">Start the Ambari portal</span></span>
1. <span data-ttu-id="6a5b1-215">V Průzkumníku Azure, klikněte pravým tlačítkem na název clusteru Spark a pak vyberte **otevřete portál pro správu clusteru (Ambari)**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-215">In Azure Explorer, right-click your Spark cluster name, and then select **Open Cluster Management Portal (Ambari)**.</span></span> 
2. <span data-ttu-id="6a5b1-216">Když se zobrazí výzva, zadejte přihlašovací údaje Správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-216">When you're prompted, enter the admin credentials for the cluster.</span></span> <span data-ttu-id="6a5b1-217">Musí být zadána tyto při zřizování clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-217">You must have specified these while provisioning the cluster.</span></span>

### <a name="manage-azure-subscriptions"></a><span data-ttu-id="6a5b1-218">Spravovat předplatná Azure</span><span class="sxs-lookup"><span data-stu-id="6a5b1-218">Manage Azure subscriptions</span></span>
<span data-ttu-id="6a5b1-219">Ve výchozím nastavení seznam nástrojů HDInsight v Azure nástrojů pro Eclipse clustery Spark ze všech předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-219">By default, HDInsight Tools in Azure Toolkit for Eclipse lists the Spark clusters from all your Azure subscriptions.</span></span> <span data-ttu-id="6a5b1-220">V případě potřeby můžete zadat předplatné, pro které chcete pro přístup ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-220">If necessary, you can specify the subscriptions for which you want to access the cluster.</span></span> 

1. <span data-ttu-id="6a5b1-221">V Průzkumníku Azure, klikněte pravým tlačítkem myši **Azure** kořenový uzel a potom klikněte na **Spravovat odběry**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-221">In Azure Explorer, right-click the **Azure** root node, and then click **Manage Subscriptions**.</span></span> 
2. <span data-ttu-id="6a5b1-222">V dialogovém okně, zrušte zaškrtnutí políčka pro předplatné, které nechcete, aby pro přístup a pak klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-222">In the dialog box, clear the check boxes for the subscription that you don't want to access, and then click **Close**.</span></span> <span data-ttu-id="6a5b1-223">Můžete také kliknout na **Odhlásit** Pokud se chcete odhlásit z vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-223">You can also click **Sign Out** if you want to sign out of your Azure subscription.</span></span>

## <a name="run-a-spark-scala-application-locally"></a><span data-ttu-id="6a5b1-224">Místní spuštění aplikace Spark Scala</span><span class="sxs-lookup"><span data-stu-id="6a5b1-224">Run a Spark Scala application locally</span></span>
<span data-ttu-id="6a5b1-225">Nástroje HDInsight v Azure nástrojů pro Eclipse slouží ke spouštění aplikací Spark Scala místně na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-225">You can use HDInsight Tools in Azure Toolkit for Eclipse to run Spark Scala applications locally on your workstation.</span></span> <span data-ttu-id="6a5b1-226">Obvykle se tyto aplikace nepotřebují přístup k prostředkům clusteru, například kontejner úložiště a můžete spustit a otestovat je místně.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-226">Typically, these applications don't need access to cluster resources such as a storage container, and you can run and test them locally.</span></span>

### <a name="prerequisite"></a><span data-ttu-id="6a5b1-227">Požadavek</span><span class="sxs-lookup"><span data-stu-id="6a5b1-227">Prerequisite</span></span>
<span data-ttu-id="6a5b1-228">Při místní aplikací Spark Scala spouštíte na počítači se systémem Windows, může získat výjimku, jak je popsáno v [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356).</span><span class="sxs-lookup"><span data-stu-id="6a5b1-228">While you're running the local Spark Scala application on a Windows computer, you might get an exception as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span></span> <span data-ttu-id="6a5b1-229">K této výjimce, protože **WinUtils.exe** chybí v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-229">This exception occurs because **WinUtils.exe** is missing in Windows.</span></span> 

<span data-ttu-id="6a5b1-230">Chcete-li tuto chybu vyřešit, je potřeba [stáhnout spustitelný soubor](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) do umístění, jako je **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-230">To resolve this error, you must [download the executable](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) to a location like **C:\WinUtils\bin**.</span></span> <span data-ttu-id="6a5b1-231">Pak musíte přidat proměnnou prostředí **HADOOP_HOME** a nastavte hodnotu proměnné na **C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-231">You must then add the environment variable **HADOOP_HOME** and set the value of the variable to **C\WinUtils**.</span></span>

### <a name="run-a-local-spark-scala-application"></a><span data-ttu-id="6a5b1-232">Spustit místních aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="6a5b1-232">Run a local Spark Scala application</span></span>
1. <span data-ttu-id="6a5b1-233">Spusťte Eclipse a vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-233">Start Eclipse and create a project.</span></span> <span data-ttu-id="6a5b1-234">V **nový projekt** dialogové okno, vyberte následující možnosti a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-234">In the **New Project** dialog box, make the following choices, and then click **Next**.</span></span>
   
   * <span data-ttu-id="6a5b1-235">V levém podokně vyberte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-235">In the left pane, select **HDInsight**.</span></span>
   * <span data-ttu-id="6a5b1-236">V pravém podokně vyberte **Spark v ukázkové spustit HDInsight místní (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-236">In the right pane, select **Spark on HDInsight Local Run Sample (Scala)**.</span></span>

    ![Dialogové okno Nový projekt](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
2. <span data-ttu-id="6a5b1-238">K zadání podrobností projektu, postupujte podle kroků 3 až 6 z části starší [nastavení projektu Spark Scala pro cluster služby HDInsight Spark](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).</span><span class="sxs-lookup"><span data-stu-id="6a5b1-238">To provide the project details, follow steps 3 through 6 from the earlier section [Set up a Spark Scala project for an HDInsight Spark cluster](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).</span></span>
3. <span data-ttu-id="6a5b1-239">Ukázkový kód přidá šablona (**LogQuery**) v části **src** složky, kterou můžete spustit místně v počítači.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-239">The template adds a sample code (**LogQuery**) under the **src** folder that you can run locally on your computer.</span></span>
   
    ![Umístění LogQuery](./media/hdinsight-apache-spark-eclipse-tool-plugin/local-app.png)
4. <span data-ttu-id="6a5b1-241">Klikněte pravým tlačítkem myši **LogQuery** aplikace, přejděte na příkaz **spustit jako**a potom klikněte na **aplikace: 1 Scala**.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-241">Right-click the **LogQuery** application, point to **Run As**, and then click **1 Scala Application**.</span></span> <span data-ttu-id="6a5b1-242">Zobrazí se výstup jako to **konzoly** karta v dolní části:</span><span class="sxs-lookup"><span data-stu-id="6a5b1-242">You will see an output like this in the **Console** tab at the bottom:</span></span>
   
   ![Místní aplikace Spark, výsledek spuštění](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="faq"></a><span data-ttu-id="6a5b1-244">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="6a5b1-244">FAQ</span></span>
<span data-ttu-id="6a5b1-245">Chcete-li odeslat aplikace do Azure Data Lake Store, zvolte **interaktivní** režimu během procesu Azure přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-245">To submit an application to Azure Data Lake Store, choose **Interactive** mode during the Azure sign-in process.</span></span> <span data-ttu-id="6a5b1-246">Pokud vyberete **automatizovaná** režimu, můžete dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-246">If you select **Automated** mode, you can get an error.</span></span>

![interaktivní přihlášení](./media/hdinsight-apache-spark-eclipse-tool-plugin/interactive-authentication.png)

<span data-ttu-id="6a5b1-248">Nyní jsme ho vyřešil.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-248">Now, we resolved it.</span></span> <span data-ttu-id="6a5b1-249">Můžete použít Cluster služby Azure Data Lake odeslat vaší aplikace s libovolnou metodu přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-249">You can choose an Azure Data Lake Cluster to submit your application with any sign-in method.</span></span>

## <a name="feedback-and-known-issues"></a><span data-ttu-id="6a5b1-250">Názory a známé problémy</span><span class="sxs-lookup"><span data-stu-id="6a5b1-250">Feedback and known issues</span></span>
<span data-ttu-id="6a5b1-251">V současné době Spark výstupů zobrazení přímo není podporováno.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-251">Currently, viewing Spark outputs directly is not supported.</span></span>

<span data-ttu-id="6a5b1-252">Pokud máte jakékoli návrhy či názory, nebo pokud se vyskytnou potíže při používání tohoto nástroje, neváhejte nám odeslat e-mail na hdivstool@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="6a5b1-252">If you have any suggestions or feedback, or if you encounter any problems when using this tool, feel free to send us an email at hdivstool@microsoft.com.</span></span>

## <span data-ttu-id="6a5b1-253"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="6a5b1-253"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="6a5b1-254">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="6a5b1-254">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="6a5b1-255">Scénáře</span><span class="sxs-lookup"><span data-stu-id="6a5b1-255">Scenarios</span></span>
* [<span data-ttu-id="6a5b1-256">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="6a5b1-256">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="6a5b1-257">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="6a5b1-257">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="6a5b1-258">Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin</span><span class="sxs-lookup"><span data-stu-id="6a5b1-258">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="6a5b1-259">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="6a5b1-259">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="6a5b1-260">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="6a5b1-260">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a><span data-ttu-id="6a5b1-261">Vytváření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="6a5b1-261">Creating and running applications</span></span>
* [<span data-ttu-id="6a5b1-262">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="6a5b1-262">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="6a5b1-263">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="6a5b1-263">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="6a5b1-264">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="6a5b1-264">Tools and extensions</span></span>
* [<span data-ttu-id="6a5b1-265">Použití nástrojů Azure pro IntelliJ k vytvoření a odeslání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="6a5b1-265">Use Azure Toolkit for IntelliJ to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="6a5b1-266">Použití nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně prostřednictvím sítě VPN</span><span class="sxs-lookup"><span data-stu-id="6a5b1-266">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="6a5b1-267">Použití nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně přes SSH</span><span class="sxs-lookup"><span data-stu-id="6a5b1-267">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="6a5b1-268">Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="6a5b1-268">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="6a5b1-269">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="6a5b1-269">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="6a5b1-270">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="6a5b1-270">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="6a5b1-271">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="6a5b1-271">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="6a5b1-272">Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="6a5b1-272">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a><span data-ttu-id="6a5b1-273">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="6a5b1-273">Managing resources</span></span>
* [<span data-ttu-id="6a5b1-274">Správa prostředků v clusteru Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="6a5b1-274">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="6a5b1-275">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="6a5b1-275">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

