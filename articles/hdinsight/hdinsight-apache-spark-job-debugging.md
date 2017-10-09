---
title: "aaaDebug Apache Spark úlohy spuštěné v Azure HDInsight | Microsoft Docs"
description: "Pomocí uživatelského rozhraní YARN, Spark uživatelského rozhraní a Spark historie serveru tootrack a ladění úloh spuštěných v clusteru Spark v Azure HDInsight"
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
ms.openlocfilehash: 33d352a5773920735aa4e5e8532b78122f381377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a><span data-ttu-id="02a2c-103">Ladění Apache Spark úlohy spuštěné v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="02a2c-103">Debug Apache Spark jobs running on Azure HDInsight</span></span>

<span data-ttu-id="02a2c-104">V tomto článku se dozvíte, jak tootrack a ladění úloh spuštěných na clusterů HDInsight pomocí hello uživatelském rozhraní YARN, Spark uživatelského rozhraní, Spark a hello Spark historie serveru.</span><span class="sxs-lookup"><span data-stu-id="02a2c-104">In this article you will learn how tootrack and debug Spark jobs running on HDInsight clusters using hello YARN UI, Spark UI, and hello Spark History Server.</span></span> <span data-ttu-id="02a2c-105">V tomto článku jsme spustí úlohu Spark pomocí poznámkového bloku dostupné s clusterem Spark hello, **strojového učení: prediktivní analýzy dat kontroly potravin pomocí MLLib**.</span><span class="sxs-lookup"><span data-stu-id="02a2c-105">For this article, we will start a Spark job using a notebook available with hello Spark cluster, **Machine learning: Predictive analysis on food inspection data using MLLib**.</span></span> <span data-ttu-id="02a2c-106">Můžete použít následující tootrack aplikace, která jste odeslali pomocí jakékoli jiné přístup také, například postup hello **odeslání spark**.</span><span class="sxs-lookup"><span data-stu-id="02a2c-106">You can use hello steps below tootrack an application that you submitted using any other approach as well, for example, **spark-submit**.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="02a2c-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="02a2c-107">Prerequisites</span></span>
<span data-ttu-id="02a2c-108">Musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="02a2c-108">You must have hello following:</span></span>

* <span data-ttu-id="02a2c-109">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="02a2c-109">An Azure subscription.</span></span> <span data-ttu-id="02a2c-110">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="02a2c-110">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="02a2c-111">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="02a2c-111">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="02a2c-112">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="02a2c-112">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="02a2c-113">Můžete by měl mít spuštění hello Poznámkový blok,  **[strojového učení: prediktivní analýzy dat kontroly potravin pomocí MLLib](hdinsight-apache-spark-machine-learning-mllib-ipython.md)**.</span><span class="sxs-lookup"><span data-stu-id="02a2c-113">You should have started running hello notebook, **[Machine learning: Predictive analysis on food inspection data using MLLib](hdinsight-apache-spark-machine-learning-mllib-ipython.md)**.</span></span> <span data-ttu-id="02a2c-114">Návod, jak toorun tento poznámkový blok, postupujte podle hello odkaz.</span><span class="sxs-lookup"><span data-stu-id="02a2c-114">For instructions on how toorun this notebook, follow hello link.</span></span>  

## <a name="track-an-application-in-hello-yarn-ui"></a><span data-ttu-id="02a2c-115">Sledování aplikace v uživatelském rozhraní YARN hello</span><span class="sxs-lookup"><span data-stu-id="02a2c-115">Track an application in hello YARN UI</span></span>
1. <span data-ttu-id="02a2c-116">Spusťte hello uživatelském rozhraní YARN.</span><span class="sxs-lookup"><span data-stu-id="02a2c-116">Launch hello YARN UI.</span></span> <span data-ttu-id="02a2c-117">V okně hello clusteru, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **YARN**.</span><span class="sxs-lookup"><span data-stu-id="02a2c-117">From hello cluster blade, click **Cluster Dashboard**, and then click **YARN**.</span></span>
   
    ![Spustit uživatelské rozhraní YARN](./media/hdinsight-apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]
   > <span data-ttu-id="02a2c-119">Alternativně můžete také spustit hello uživatelském rozhraní YARN z hello uživatelského rozhraní Ambari.</span><span class="sxs-lookup"><span data-stu-id="02a2c-119">Alternatively, you can also launch hello YARN UI from hello Ambari UI.</span></span> <span data-ttu-id="02a2c-120">Klikněte na tlačítko toolaunch hello uživatelského rozhraní Ambari, v okně clusteru hello, **řídicí panel clusteru**a potom klikněte na **řídicí panel clusteru HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="02a2c-120">toolaunch hello Ambari UI, from hello cluster blade, click **Cluster Dashboard**, and then click **HDInsight Cluster Dashboard**.</span></span> <span data-ttu-id="02a2c-121">Z hello uživatelského rozhraní Ambari, klikněte na tlačítko **YARN**, klikněte na tlačítko **rychlé odkazy**, klikněte na tlačítko hello active resource Manageru a pak klikněte na **uživatelského rozhraní ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="02a2c-121">From hello Ambari UI, click **YARN**, click **Quick Links**, click hello active resource manager, and then click **ResourceManager UI**.</span></span>    
   > 
   > 
2. <span data-ttu-id="02a2c-122">Protože jste spustili úlohy Spark hello pomocí Jupyter notebooks, aplikace hello má název hello **remotesparkmagics** (to je hello název pro všechny aplikace, které jsou spuštěné z poznámkových bloků hello).</span><span class="sxs-lookup"><span data-stu-id="02a2c-122">Because you started hello Spark job using Jupyter notebooks, hello application has hello name **remotesparkmagics** (this is hello name for all applications that are started from hello notebooks).</span></span> <span data-ttu-id="02a2c-123">Další informace o úloze hello klikněte na tlačítko ID aplikace hello proti tooget název aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="02a2c-123">Click hello application ID against hello application name tooget more information about hello job.</span></span> <span data-ttu-id="02a2c-124">Spustí zobrazení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="02a2c-124">This launches hello application view.</span></span>
   
    ![Vyhledání ID aplikací Spark](./media/hdinsight-apache-spark-job-debugging/find-application-id.png)
   
    <span data-ttu-id="02a2c-126">Pro tyto aplikace, které jsou spouštěny z poznámkových bloků Jupyter hello hello stav je vždy **systémem** do ukončení hello Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="02a2c-126">For such applications that are launched from hello Jupyter notebooks, hello status is always **RUNNING** until you exit hello notebook.</span></span>
3. <span data-ttu-id="02a2c-127">Z hlediska aplikace hello podrobnostem další toofind out hello kontejnery přidružené aplikace hello a protokoly hello (stdout/stderr).</span><span class="sxs-lookup"><span data-stu-id="02a2c-127">From hello application view, you can drill down further toofind out hello containers associated with hello application and hello logs (stdout/stderr).</span></span> <span data-ttu-id="02a2c-128">Hello Spark uživatelského rozhraní můžete také spustit kliknutím hello propojení odpovídající toohello **sledování adresy URL**, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="02a2c-128">You can also launch hello Spark UI by clicking hello linking corresponding toohello **Tracking URL**, as shown below.</span></span> 
   
    ![Stažení protokolů o kontejneru](./media/hdinsight-apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-hello-spark-ui"></a><span data-ttu-id="02a2c-130">Sledování aplikace hello Spark uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="02a2c-130">Track an application in hello Spark UI</span></span>
<span data-ttu-id="02a2c-131">V hello Spark uživatelského rozhraní můžete se přejděte do hello Spark úlohy, které jsou vytvořený službou hello aplikaci, kterou jste spustili dříve.</span><span class="sxs-lookup"><span data-stu-id="02a2c-131">In hello Spark UI, you can drill down into hello Spark jobs that are spawned by hello application you started earlier.</span></span>

1. <span data-ttu-id="02a2c-132">toolaunch hello Spark uživatelského rozhraní, ze zobrazení aplikace hello, klikněte na odkaz hello proti hello **sledování adresy URL**, jak ukazuje snímek obrazovky hello výše.</span><span class="sxs-lookup"><span data-stu-id="02a2c-132">toolaunch hello Spark UI, from hello application view, click hello link against hello **Tracking URL**, as shown in hello screen capture above.</span></span> <span data-ttu-id="02a2c-133">Zobrazí se všechny úlohy Spark hello, které jsou spouštěny podle hello aplikace běžící v hello Poznámkový blok Jupyter.</span><span class="sxs-lookup"><span data-stu-id="02a2c-133">You can see all hello Spark jobs that are launched by hello application running in hello Jupyter notebook.</span></span>
   
    ![Zobrazit úlohy Spark](./media/hdinsight-apache-spark-job-debugging/view-spark-jobs.png)
2. <span data-ttu-id="02a2c-135">Klikněte na tlačítko hello **vykonavatelů** kartě toosee zpracování a ukládání informací pro každý prováděcího modulu.</span><span class="sxs-lookup"><span data-stu-id="02a2c-135">Click hello **Executors** tab toosee processing and storage information for each executor.</span></span> <span data-ttu-id="02a2c-136">Můžete také načíst zásobník volání hello kliknutím na hello **vláken Dump** odkaz.</span><span class="sxs-lookup"><span data-stu-id="02a2c-136">You can also retrieve hello call stack by clicking on hello **Thread Dump** link.</span></span>
   
    ![Zobrazit vykonavatelů Spark](./media/hdinsight-apache-spark-job-debugging/view-spark-executors.png)
3. <span data-ttu-id="02a2c-138">Klikněte na tlačítko hello **fázích** kartě toosee hello fázemi, přidruženou k aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="02a2c-138">Click hello **Stages** tab toosee hello stages associated with hello application.</span></span>
   
    ![Zobrazit Spark fáze](./media/hdinsight-apache-spark-job-debugging/view-spark-stages.png)
   
    <span data-ttu-id="02a2c-140">Každá fáze může mít více úloh, pro které je možné zobrazit provádění statistiky, jako vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="02a2c-140">Each stage can have multiple tasks for which you can view execution statistics, like shown below.</span></span>
   
    ![Zobrazit Spark fáze](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-details.png) 
4. <span data-ttu-id="02a2c-142">Na stránce Podrobnosti hello fáze můžete spustit DAG vizualizace.</span><span class="sxs-lookup"><span data-stu-id="02a2c-142">From hello stage details page, you can launch DAG Visualization.</span></span> <span data-ttu-id="02a2c-143">Rozbalte hello **DAG vizualizace** odkaz v horní části hello hello stránky, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="02a2c-143">Expand hello **DAG Visualization** link at hello top of hello page, as shown below.</span></span>
   
    ![Zobrazit vizualizace Spark fázích DAG](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    <span data-ttu-id="02a2c-145">DAG nebo přímé Aclyic grafu představuje hello různých fázích v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="02a2c-145">DAG or Direct Aclyic Graph represents hello different stages in hello application.</span></span> <span data-ttu-id="02a2c-146">Každé pole blue v grafu hello představuje Spark operace, vyvolané z aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="02a2c-146">Each blue box in hello graph represents a Spark operation invoked from hello application.</span></span>
5. <span data-ttu-id="02a2c-147">Na stránce podrobnosti fáze hello můžete také spustit zobrazení časové osy aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="02a2c-147">From hello stage details page, you can also launch hello application timeline view.</span></span> <span data-ttu-id="02a2c-148">Rozbalte hello **časová osa událostí** odkaz v horní části hello hello stránky, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="02a2c-148">Expand hello **Event Timeline** link at hello top of hello page, as shown below.</span></span>
   
    ![Zpracuje událost časová osa zobrazení Spark](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    <span data-ttu-id="02a2c-150">Zobrazí se události hello Spark v podobě hello časové osy.</span><span class="sxs-lookup"><span data-stu-id="02a2c-150">This displays hello Spark events in hello form of a timeline.</span></span> <span data-ttu-id="02a2c-151">Časová osa zobrazení Hello je k dispozici ve třech úrovních mezi úlohami v rámci úlohy a v rámci úsek.</span><span class="sxs-lookup"><span data-stu-id="02a2c-151">hello timeline view is available at three levels, across jobs, within a job, and within a stage.</span></span> <span data-ttu-id="02a2c-152">Hello obrázku výše zaznamená hello časová osa zobrazení pro danou fázi.</span><span class="sxs-lookup"><span data-stu-id="02a2c-152">hello image above captures hello timeline view for a given stage.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="02a2c-153">Pokud vyberete hello **zapnout zvětšování** zaškrtávací políčko můžete posunete doleva a doprava napříč hello časová osa zobrazení.</span><span class="sxs-lookup"><span data-stu-id="02a2c-153">If you select hello **Enable zooming** check box, you can scroll left and right across hello timeline view.</span></span>
   > 
   > 
6. <span data-ttu-id="02a2c-154">Ostatní karty v hello uživatelského rozhraní Spark poskytují užitečné informace o hello Spark také instanci.</span><span class="sxs-lookup"><span data-stu-id="02a2c-154">Other tabs in hello Spark UI provide useful information about hello Spark instance as well.</span></span>
   
   * <span data-ttu-id="02a2c-155">Karta úložiště – Pokud vaše aplikace vytvoří RDDs, může najít informace o těch, na kartě úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="02a2c-155">Storage tab - If your application creates an RDDs, you can find information about those in hello Storage tab.</span></span>
   * <span data-ttu-id="02a2c-156">Karta prostředí – tato karta obsahuje mnoho užitečných informací o vaší instanci Spark, jako je například hello</span><span class="sxs-lookup"><span data-stu-id="02a2c-156">Environment tab - This tab provides a lot of useful information about your Spark instance such as hello</span></span> 
     * <span data-ttu-id="02a2c-157">Verze Scala</span><span class="sxs-lookup"><span data-stu-id="02a2c-157">Scala version</span></span>
     * <span data-ttu-id="02a2c-158">Adresář protokolu událostí související s clusterem hello</span><span class="sxs-lookup"><span data-stu-id="02a2c-158">Event log directory associated with hello cluster</span></span>
     * <span data-ttu-id="02a2c-159">Počet jader vykonavatele pro aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="02a2c-159">Number of executor cores for hello application</span></span>
     * <span data-ttu-id="02a2c-160">Atd.</span><span class="sxs-lookup"><span data-stu-id="02a2c-160">Etc.</span></span>

## <a name="find-information-about-completed-jobs-using-hello-spark-history-server"></a><span data-ttu-id="02a2c-161">Najít informace o dokončené úlohy pomocí Spark historie Server hello</span><span class="sxs-lookup"><span data-stu-id="02a2c-161">Find information about completed jobs using hello Spark History Server</span></span>
<span data-ttu-id="02a2c-162">Po dokončení úlohy je v hello Spark historie serveru jako trvalý hello informace o úloze hello.</span><span class="sxs-lookup"><span data-stu-id="02a2c-162">Once a job is completed, hello information about hello job is persisted in hello Spark History Server.</span></span>

1. <span data-ttu-id="02a2c-163">Klikněte na tlačítko toolaunch hello serveru Spark historie, v okně clusteru hello, **řídicí panel clusteru**a potom klikněte na **Spark historie serveru**.</span><span class="sxs-lookup"><span data-stu-id="02a2c-163">toolaunch hello Spark History Server, from hello cluster blade, click **Cluster Dashboard**, and then click **Spark History Server**.</span></span>
   
    ![Spusťte Server historie Spark](./media/hdinsight-apache-spark-job-debugging/launch-spark-history-server.png)
   
   > [!TIP]
   > <span data-ttu-id="02a2c-165">Alternativně můžete také spustit hello uživatelské rozhraní serveru Spark historie z hello uživatelského rozhraní Ambari.</span><span class="sxs-lookup"><span data-stu-id="02a2c-165">Alternatively, you can also launch hello Spark History Server UI from hello Ambari UI.</span></span> <span data-ttu-id="02a2c-166">Klikněte na tlačítko toolaunch hello uživatelského rozhraní Ambari, v okně clusteru hello, **řídicí panel clusteru**a potom klikněte na **řídicí panel clusteru HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="02a2c-166">toolaunch hello Ambari UI, from hello cluster blade, click **Cluster Dashboard**, and then click **HDInsight Cluster Dashboard**.</span></span> <span data-ttu-id="02a2c-167">Z hello uživatelského rozhraní Ambari, klikněte na tlačítko **Spark**, klikněte na tlačítko **rychlé odkazy**a potom klikněte na **uživatelské rozhraní serveru Spark historie**.</span><span class="sxs-lookup"><span data-stu-id="02a2c-167">From hello Ambari UI, click **Spark**, click **Quick Links**, and then click **Spark History Server UI**.</span></span>
   > 
   > 
2. <span data-ttu-id="02a2c-168">Zobrazí se všechny aplikace hello dokončit uvedené.</span><span class="sxs-lookup"><span data-stu-id="02a2c-168">You will see all hello completed applications listed.</span></span> <span data-ttu-id="02a2c-169">Klikněte na tlačítko toodrill ID aplikace dolů do aplikace pro další informace.</span><span class="sxs-lookup"><span data-stu-id="02a2c-169">Click an application ID toodrill down into an application for more info.</span></span>
   
    ![Spusťte Server historie Spark](./media/hdinsight-apache-spark-job-debugging/view-completed-applications.png)

## <a name="see-also"></a><span data-ttu-id="02a2c-171">Viz také</span><span class="sxs-lookup"><span data-stu-id="02a2c-171">See also</span></span>
*  [<span data-ttu-id="02a2c-172">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="02a2c-172">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)

### <a name="for-data-analysts"></a><span data-ttu-id="02a2c-173">Pro analytiky dat</span><span class="sxs-lookup"><span data-stu-id="02a2c-173">For data analysts</span></span>

* [<span data-ttu-id="02a2c-174">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="02a2c-174">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="02a2c-175">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="02a2c-175">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="02a2c-176">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="02a2c-176">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [<span data-ttu-id="02a2c-177">Analýza dat telemetrie Application Insights pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="02a2c-177">Application Insight telemetry data analysis using Spark in HDInsight</span></span>](hdinsight-spark-analyze-application-insight-logs.md)
* [<span data-ttu-id="02a2c-178">Použití Caffe v Azure HDInsight Spark pro distribuované hloubkové learning</span><span class="sxs-lookup"><span data-stu-id="02a2c-178">Use Caffe on Azure HDInsight Spark for distributed deep learning</span></span>](hdinsight-deep-learning-caffe-spark.md)

### <a name="for-spark-developers"></a><span data-ttu-id="02a2c-179">Pro vývojáře, Spark</span><span class="sxs-lookup"><span data-stu-id="02a2c-179">For Spark developers</span></span>

* [<span data-ttu-id="02a2c-180">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="02a2c-180">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="02a2c-181">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="02a2c-181">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)
* [<span data-ttu-id="02a2c-182">Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="02a2c-182">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="02a2c-183">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="02a2c-183">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="02a2c-184">Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace</span><span class="sxs-lookup"><span data-stu-id="02a2c-184">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="02a2c-185">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="02a2c-185">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="02a2c-186">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="02a2c-186">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="02a2c-187">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="02a2c-187">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="02a2c-188">Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="02a2c-188">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)


