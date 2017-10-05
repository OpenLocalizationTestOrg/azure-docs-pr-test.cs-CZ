---
title: "Ladění Apache Spark úlohy spuštěné v Azure HDInsight | Microsoft Docs"
description: "Použít uživatelském rozhraní YARN, Spark uživatelského rozhraní a Spark historie serveru ke sledování a ladění úloh spuštěných na clusteru Spark v Azure HDInsight"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 59af05a7-2bd9-44b0-b55f-2438d294198b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: bf66757cc9439a969c9f28abc0b95055ff697c3b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a><span data-ttu-id="38d88-103">Ladění Apache Spark úlohy spuštěné v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="38d88-103">Debug Apache Spark jobs running on Azure HDInsight</span></span>

<span data-ttu-id="38d88-104">V tomto článku se dozvíte, jak sledovat a ladit spuštěné v clusterech prostředí HDInsight pomocí uživatelského rozhraní YARN, Spark uživatelského rozhraní a serveru Spark historie úlohy Spark.</span><span class="sxs-lookup"><span data-stu-id="38d88-104">In this article you will learn how to track and debug Spark jobs running on HDInsight clusters using the YARN UI, Spark UI, and the Spark History Server.</span></span> <span data-ttu-id="38d88-105">V tomto článku jsme spustí úlohu Spark pomocí poznámkového bloku dostupné s clusterem Spark **strojového učení: prediktivní analýzy dat kontroly potravin pomocí MLLib**.</span><span class="sxs-lookup"><span data-stu-id="38d88-105">For this article, we will start a Spark job using a notebook available with the Spark cluster, **Machine learning: Predictive analysis on food inspection data using MLLib**.</span></span> <span data-ttu-id="38d88-106">Následující postup můžete použít ke sledování aplikace, která jste odeslali pomocí jakékoli jiné přístup také, například **odeslání spark**.</span><span class="sxs-lookup"><span data-stu-id="38d88-106">You can use the steps below to track an application that you submitted using any other approach as well, for example, **spark-submit**.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="38d88-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="38d88-107">Prerequisites</span></span>
<span data-ttu-id="38d88-108">Musíte mít následující:</span><span class="sxs-lookup"><span data-stu-id="38d88-108">You must have the following:</span></span>

* <span data-ttu-id="38d88-109">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="38d88-109">An Azure subscription.</span></span> <span data-ttu-id="38d88-110">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="38d88-110">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="38d88-111">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="38d88-111">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="38d88-112">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="38d88-112">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="38d88-113">Můžete by měl mít spuštění poznámkového bloku,  **[strojového učení: prediktivní analýzy dat kontroly potravin pomocí MLLib](hdinsight-apache-spark-machine-learning-mllib-ipython.md)**.</span><span class="sxs-lookup"><span data-stu-id="38d88-113">You should have started running the notebook, **[Machine learning: Predictive analysis on food inspection data using MLLib](hdinsight-apache-spark-machine-learning-mllib-ipython.md)**.</span></span> <span data-ttu-id="38d88-114">Návod, jak spustit tento poznámkový blok pomocí následujícího odkazu.</span><span class="sxs-lookup"><span data-stu-id="38d88-114">For instructions on how to run this notebook, follow the link.</span></span>  

## <a name="track-an-application-in-the-yarn-ui"></a><span data-ttu-id="38d88-115">Sledování aplikace v uživatelském rozhraní YARN</span><span class="sxs-lookup"><span data-stu-id="38d88-115">Track an application in the YARN UI</span></span>
1. <span data-ttu-id="38d88-116">Spuštění uživatelského rozhraní YARN.</span><span class="sxs-lookup"><span data-stu-id="38d88-116">Launch the YARN UI.</span></span> <span data-ttu-id="38d88-117">V okně clusteru, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **YARN**.</span><span class="sxs-lookup"><span data-stu-id="38d88-117">From the cluster blade, click **Cluster Dashboard**, and then click **YARN**.</span></span>
   
    ![Spustit uživatelské rozhraní YARN](./media/hdinsight-apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]
   > <span data-ttu-id="38d88-119">Alternativně můžete také spustit uživatelské rozhraní YARN z uživatelského rozhraní Ambari.</span><span class="sxs-lookup"><span data-stu-id="38d88-119">Alternatively, you can also launch the YARN UI from the Ambari UI.</span></span> <span data-ttu-id="38d88-120">Chcete-li spustit uživatelské rozhraní Ambari, v okně clusteru, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **řídicí panel clusteru HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="38d88-120">To launch the Ambari UI, from the cluster blade, click **Cluster Dashboard**, and then click **HDInsight Cluster Dashboard**.</span></span> <span data-ttu-id="38d88-121">V uživatelském rozhraní Ambari, klikněte na **YARN**, klikněte na tlačítko **rychlé odkazy**, klikněte na tlačítko Správce prostředků active a pak klikněte na tlačítko **uživatelského rozhraní ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="38d88-121">From the Ambari UI, click **YARN**, click **Quick Links**, click the active resource manager, and then click **ResourceManager UI**.</span></span>    
   > 
   > 
2. <span data-ttu-id="38d88-122">Protože jste spustili úlohy Spark pomocí Jupyter notebooks, aplikace, má název **remotesparkmagics** (to je název pro všechny aplikace, které jsou spuštěné z poznámkových bloků).</span><span class="sxs-lookup"><span data-stu-id="38d88-122">Because you started the Spark job using Jupyter notebooks, the application has the name **remotesparkmagics** (this is the name for all applications that are started from the notebooks).</span></span> <span data-ttu-id="38d88-123">Klikněte na tlačítko ID aplikace proti název aplikace, chcete-li získat další informace o úloze.</span><span class="sxs-lookup"><span data-stu-id="38d88-123">Click the application ID against the application name to get more information about the job.</span></span> <span data-ttu-id="38d88-124">Spustí zobrazení aplikací.</span><span class="sxs-lookup"><span data-stu-id="38d88-124">This launches the application view.</span></span>
   
    ![Vyhledání ID aplikací Spark](./media/hdinsight-apache-spark-job-debugging/find-application-id.png)
   
    <span data-ttu-id="38d88-126">Takové aplikace, které jsou spouštěny z Jupyter notebooks, stav je vždy **systémem** do ukončení poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="38d88-126">For such applications that are launched from the Jupyter notebooks, the status is always **RUNNING** until you exit the notebook.</span></span>
3. <span data-ttu-id="38d88-127">Z pohledu aplikace podrobnostem dál zjistěte kontejnery přidružené aplikace a protokoly (stdout/stderr).</span><span class="sxs-lookup"><span data-stu-id="38d88-127">From the application view, you can drill down further to find out the containers associated with the application and the logs (stdout/stderr).</span></span> <span data-ttu-id="38d88-128">Můžete také spustit uživatelské rozhraní Spark kliknutím serveru linking odpovídající **sledování adresy URL**, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="38d88-128">You can also launch the Spark UI by clicking the linking corresponding to the **Tracking URL**, as shown below.</span></span> 
   
    ![Stažení protokolů o kontejneru](./media/hdinsight-apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-the-spark-ui"></a><span data-ttu-id="38d88-130">Sledování aplikace v uživatelském rozhraní Spark</span><span class="sxs-lookup"><span data-stu-id="38d88-130">Track an application in the Spark UI</span></span>
<span data-ttu-id="38d88-131">V uživatelském rozhraní Spark podrobnostem do úlohy Spark, které jsou v aplikaci, kterou jste spustili dříve vytvořený.</span><span class="sxs-lookup"><span data-stu-id="38d88-131">In the Spark UI, you can drill down into the Spark jobs that are spawned by the application you started earlier.</span></span>

1. <span data-ttu-id="38d88-132">Spuštění rozhraní Spark ze zobrazení aplikací, klikněte na odkaz proti **sledování adresy URL**, jak je uvedeno výše uvedený snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="38d88-132">To launch the Spark UI, from the application view, click the link against the **Tracking URL**, as shown in the screen capture above.</span></span> <span data-ttu-id="38d88-133">Zobrazí se všechny úlohy Spark, které jsou v aplikaci běžící v poznámkového bloku Jupyter spouštěny.</span><span class="sxs-lookup"><span data-stu-id="38d88-133">You can see all the Spark jobs that are launched by the application running in the Jupyter notebook.</span></span>
   
    ![Zobrazit úlohy Spark](./media/hdinsight-apache-spark-job-debugging/view-spark-jobs.png)
2. <span data-ttu-id="38d88-135">Klikněte **vykonavatelů** karty zobrazíte informace o zpracování a úložiště pro každý prováděcího modulu.</span><span class="sxs-lookup"><span data-stu-id="38d88-135">Click the **Executors** tab to see processing and storage information for each executor.</span></span> <span data-ttu-id="38d88-136">Můžete také kliknutím na načíst zásobníku volání **vláken Dump** odkaz.</span><span class="sxs-lookup"><span data-stu-id="38d88-136">You can also retrieve the call stack by clicking on the **Thread Dump** link.</span></span>
   
    ![Zobrazit vykonavatelů Spark](./media/hdinsight-apache-spark-job-debugging/view-spark-executors.png)
3. <span data-ttu-id="38d88-138">Klikněte na tlačítko **fázích** karty zobrazíte fázích přidružené k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="38d88-138">Click the **Stages** tab to see the stages associated with the application.</span></span>
   
    ![Zobrazit Spark fáze](./media/hdinsight-apache-spark-job-debugging/view-spark-stages.png)
   
    <span data-ttu-id="38d88-140">Každá fáze může mít více úloh, pro které je možné zobrazit provádění statistiky, jako vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="38d88-140">Each stage can have multiple tasks for which you can view execution statistics, like shown below.</span></span>
   
    ![Zobrazit Spark fáze](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-details.png) 
4. <span data-ttu-id="38d88-142">Na stránce podrobností fázi můžete spustit DAG vizualizace.</span><span class="sxs-lookup"><span data-stu-id="38d88-142">From the stage details page, you can launch DAG Visualization.</span></span> <span data-ttu-id="38d88-143">Rozbalte **DAG vizualizace** odkaz v horní části stránky, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="38d88-143">Expand the **DAG Visualization** link at the top of the page, as shown below.</span></span>
   
    ![Zobrazit vizualizace Spark fázích DAG](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    <span data-ttu-id="38d88-145">DAG nebo přímé Aclyic grafu představuje různých fázích v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="38d88-145">DAG or Direct Aclyic Graph represents the different stages in the application.</span></span> <span data-ttu-id="38d88-146">Každé pole blue v grafu představuje Spark operace, vyvolané z aplikace.</span><span class="sxs-lookup"><span data-stu-id="38d88-146">Each blue box in the graph represents a Spark operation invoked from the application.</span></span>
5. <span data-ttu-id="38d88-147">Na stránce podrobností fázi můžete také spustit zobrazení časové osy aplikace.</span><span class="sxs-lookup"><span data-stu-id="38d88-147">From the stage details page, you can also launch the application timeline view.</span></span> <span data-ttu-id="38d88-148">Rozbalte **časová osa událostí** odkaz v horní části stránky, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="38d88-148">Expand the **Event Timeline** link at the top of the page, as shown below.</span></span>
   
    ![Zpracuje událost časová osa zobrazení Spark](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    <span data-ttu-id="38d88-150">Zobrazí se události Spark ve formě časové osy.</span><span class="sxs-lookup"><span data-stu-id="38d88-150">This displays the Spark events in the form of a timeline.</span></span> <span data-ttu-id="38d88-151">Zobrazení časové osy je k dispozici ve třech úrovních mezi úlohami v rámci úlohy a v rámci úsek.</span><span class="sxs-lookup"><span data-stu-id="38d88-151">The timeline view is available at three levels, across jobs, within a job, and within a stage.</span></span> <span data-ttu-id="38d88-152">Na obrázku výše zaznamená zobrazení časové osy pro danou fázi.</span><span class="sxs-lookup"><span data-stu-id="38d88-152">The image above captures the timeline view for a given stage.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="38d88-153">Pokud jste vybrali **zapnout zvětšování** zaškrtávací políčko můžete posunete doleva a doprava napříč zobrazení časové osy.</span><span class="sxs-lookup"><span data-stu-id="38d88-153">If you select the **Enable zooming** check box, you can scroll left and right across the timeline view.</span></span>
   > 
   > 
6. <span data-ttu-id="38d88-154">Ostatní karty v uživatelském rozhraní Spark poskytují užitečné informace o instanci Spark také.</span><span class="sxs-lookup"><span data-stu-id="38d88-154">Other tabs in the Spark UI provide useful information about the Spark instance as well.</span></span>
   
   * <span data-ttu-id="38d88-155">Karta úložiště – Pokud vaše aplikace vytvoří RDDs, může najít informace o těch, na kartě úložiště.</span><span class="sxs-lookup"><span data-stu-id="38d88-155">Storage tab - If your application creates an RDDs, you can find information about those in the Storage tab.</span></span>
   * <span data-ttu-id="38d88-156">Karta prostředí – tato karta obsahuje mnoho užitečných informací o instanci Spark, jako</span><span class="sxs-lookup"><span data-stu-id="38d88-156">Environment tab - This tab provides a lot of useful information about your Spark instance such as the</span></span> 
     * <span data-ttu-id="38d88-157">Verze Scala</span><span class="sxs-lookup"><span data-stu-id="38d88-157">Scala version</span></span>
     * <span data-ttu-id="38d88-158">Adresář protokolu událostí související s clusterem</span><span class="sxs-lookup"><span data-stu-id="38d88-158">Event log directory associated with the cluster</span></span>
     * <span data-ttu-id="38d88-159">Počet jader vykonavatele pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="38d88-159">Number of executor cores for the application</span></span>
     * <span data-ttu-id="38d88-160">Atd.</span><span class="sxs-lookup"><span data-stu-id="38d88-160">Etc.</span></span>

## <a name="find-information-about-completed-jobs-using-the-spark-history-server"></a><span data-ttu-id="38d88-161">Najít informace o používání serveru historie Spark dokončené úlohy</span><span class="sxs-lookup"><span data-stu-id="38d88-161">Find information about completed jobs using the Spark History Server</span></span>
<span data-ttu-id="38d88-162">Po dokončení úlohy je v serveru Spark historie jako trvalý informace o úloze.</span><span class="sxs-lookup"><span data-stu-id="38d88-162">Once a job is completed, the information about the job is persisted in the Spark History Server.</span></span>

1. <span data-ttu-id="38d88-163">Ke spuštění serveru Spark historie, v okně clusteru, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **Spark historie serveru**.</span><span class="sxs-lookup"><span data-stu-id="38d88-163">To launch the Spark History Server, from the cluster blade, click **Cluster Dashboard**, and then click **Spark History Server**.</span></span>
   
    ![Spusťte Server historie Spark](./media/hdinsight-apache-spark-job-debugging/launch-spark-history-server.png)
   
   > [!TIP]
   > <span data-ttu-id="38d88-165">Alternativně můžete také spustit rozhraní Spark historie serveru z uživatelského rozhraní Ambari.</span><span class="sxs-lookup"><span data-stu-id="38d88-165">Alternatively, you can also launch the Spark History Server UI from the Ambari UI.</span></span> <span data-ttu-id="38d88-166">Chcete-li spustit uživatelské rozhraní Ambari, v okně clusteru, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **řídicí panel clusteru HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="38d88-166">To launch the Ambari UI, from the cluster blade, click **Cluster Dashboard**, and then click **HDInsight Cluster Dashboard**.</span></span> <span data-ttu-id="38d88-167">V uživatelském rozhraní Ambari, klikněte na **Spark**, klikněte na tlačítko **rychlé odkazy**a potom klikněte na **uživatelské rozhraní serveru Spark historie**.</span><span class="sxs-lookup"><span data-stu-id="38d88-167">From the Ambari UI, click **Spark**, click **Quick Links**, and then click **Spark History Server UI**.</span></span>
   > 
   > 
2. <span data-ttu-id="38d88-168">Zobrazí všechny dokončené aplikace uvedené.</span><span class="sxs-lookup"><span data-stu-id="38d88-168">You will see all the completed applications listed.</span></span> <span data-ttu-id="38d88-169">Klikněte na tlačítko ID aplikací k podrobnostem aplikace pro další informace.</span><span class="sxs-lookup"><span data-stu-id="38d88-169">Click an application ID to drill down into an application for more info.</span></span>
   
    ![Spusťte Server historie Spark](./media/hdinsight-apache-spark-job-debugging/view-completed-applications.png)

## <a name="see-also"></a><span data-ttu-id="38d88-171">Viz také</span><span class="sxs-lookup"><span data-stu-id="38d88-171">See also</span></span>
*  [<span data-ttu-id="38d88-172">Správa prostředků v clusteru Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="38d88-172">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)

### <a name="for-data-analysts"></a><span data-ttu-id="38d88-173">Pro analytiky dat</span><span class="sxs-lookup"><span data-stu-id="38d88-173">For data analysts</span></span>

* [<span data-ttu-id="38d88-174">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="38d88-174">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="38d88-175">Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin</span><span class="sxs-lookup"><span data-stu-id="38d88-175">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="38d88-176">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="38d88-176">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [<span data-ttu-id="38d88-177">Analýza dat telemetrie Application Insights pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="38d88-177">Application Insight telemetry data analysis using Spark in HDInsight</span></span>](hdinsight-spark-analyze-application-insight-logs.md)
* [<span data-ttu-id="38d88-178">Použití Caffe v Azure HDInsight Spark pro distribuované hloubkové learning</span><span class="sxs-lookup"><span data-stu-id="38d88-178">Use Caffe on Azure HDInsight Spark for distributed deep learning</span></span>](hdinsight-deep-learning-caffe-spark.md)

### <a name="for-spark-developers"></a><span data-ttu-id="38d88-179">Pro vývojáře, Spark</span><span class="sxs-lookup"><span data-stu-id="38d88-179">For Spark developers</span></span>

* [<span data-ttu-id="38d88-180">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="38d88-180">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="38d88-181">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="38d88-181">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)
* [<span data-ttu-id="38d88-182">Modul plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="38d88-182">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="38d88-183">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="38d88-183">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="38d88-184">Použití modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vzdálené ladění aplikací Spark</span><span class="sxs-lookup"><span data-stu-id="38d88-184">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="38d88-185">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="38d88-185">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="38d88-186">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="38d88-186">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="38d88-187">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="38d88-187">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="38d88-188">Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="38d88-188">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)


