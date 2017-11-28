---
title: "Použijte zobrazení Ambari Tez s HDInsight - Azure | Microsoft Docs"
description: "Další informace o použití zobrazení Ambari Tez k ladění úlohách Tez v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 9c39ea56-670b-4699-aba0-0f64c261e411
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 65d89309b9eea8544b85d16687baa90d49688d77
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-ambari-views-to-debug-tez-jobs-on-hdinsight"></a><span data-ttu-id="33e33-103">Použití zobrazení Ambari k ladění úlohách Tez v HDInsight</span><span class="sxs-lookup"><span data-stu-id="33e33-103">Use Ambari Views to debug Tez Jobs on HDInsight</span></span>

<span data-ttu-id="33e33-104">Webové uživatelské rozhraní Ambari pro HDInsight obsahuje Tez zobrazení, které můžete použít k pochopení a ladění úlohy, které používají Tez.</span><span class="sxs-lookup"><span data-stu-id="33e33-104">The Ambari Web UI for HDInsight contains a Tez view that can be used to understand and debug jobs that use Tez.</span></span> <span data-ttu-id="33e33-105">Zobrazení Tez umožňuje vizualizovat úlohu jako graf připojených položek, přejdete na každou položku a načíst informace o protokolování a statistiky.</span><span class="sxs-lookup"><span data-stu-id="33e33-105">The Tez view allows you to visualize the job as a graph of connected items, drill into each item, and retrieve statistics and logging information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="33e33-106">Kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="33e33-106">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="33e33-107">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="33e33-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="33e33-108">Další informace najdete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="33e33-108">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33e33-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="33e33-109">Prerequisites</span></span>

* <span data-ttu-id="33e33-110">Cluster HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="33e33-110">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="33e33-111">Pokyny týkající se vytvoření clusteru, najdete v části [začněte používat HDInsight se systémem Linux](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="33e33-111">For steps on creating a cluster, see [Get started using Linux-based HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
* <span data-ttu-id="33e33-112">Moderní webový prohlížeč, který podporuje HTML5.</span><span class="sxs-lookup"><span data-stu-id="33e33-112">A modern web browser that supports HTML5.</span></span>

## <a name="understanding-tez"></a><span data-ttu-id="33e33-113">Principy Tez</span><span class="sxs-lookup"><span data-stu-id="33e33-113">Understanding Tez</span></span>

<span data-ttu-id="33e33-114">Tez je rozšiřitelná architektura pro zpracování dat v Hadoop, která poskytuje vyšší rychlosti než tradiční zpracování prostředí MapReduce.</span><span class="sxs-lookup"><span data-stu-id="33e33-114">Tez is an extensible framework for data processing in Hadoop that provides greater speeds than traditional MapReduce processing.</span></span> <span data-ttu-id="33e33-115">Clustery HDInsight se systémem Linux je výchozí modul pro Hive.</span><span class="sxs-lookup"><span data-stu-id="33e33-115">For Linux-based HDInsight clusters, it is the default engine for Hive.</span></span>

<span data-ttu-id="33e33-116">Tez vytvoří směrované Acyklické grafu (DAG) popisující pořadí akce požadované úlohami.</span><span class="sxs-lookup"><span data-stu-id="33e33-116">Tez creates a Directed Acyclic Graph (DAG) that describes the order of actions required by jobs.</span></span> <span data-ttu-id="33e33-117">Jednotlivé akce se nazývají vrcholy a provést část celkového úlohy.</span><span class="sxs-lookup"><span data-stu-id="33e33-117">Individual actions are called vertices, and execute a piece of the overall job.</span></span> <span data-ttu-id="33e33-118">Skutečné provádění pracovní popsaného vrchol je volána úloha a mohou být distribuovány mezi několika uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="33e33-118">The actual execution of the work described by a vertex is called a task, and may be distributed across multiple nodes in the cluster.</span></span>

### <a name="understanding-the-tez-view"></a><span data-ttu-id="33e33-119">Principy zobrazení Tez</span><span class="sxs-lookup"><span data-stu-id="33e33-119">Understanding the Tez view</span></span>

<span data-ttu-id="33e33-120">Zobrazení Tez poskytuje historické informace a informace na procesy, které jsou spuštěné.</span><span class="sxs-lookup"><span data-stu-id="33e33-120">The Tez view provides both historical information and information on processes that are running.</span></span> <span data-ttu-id="33e33-121">Tyto informace ukazuje, jak je úlohu rozdělené mezi clustery.</span><span class="sxs-lookup"><span data-stu-id="33e33-121">This information shows how a job is distributed across clusters.</span></span> <span data-ttu-id="33e33-122">Zobrazí se také čítače používané úlohy a vrcholy a informace o chybě související s úlohy.</span><span class="sxs-lookup"><span data-stu-id="33e33-122">It also displays counters used by tasks and vertices, and error information related to the job.</span></span> <span data-ttu-id="33e33-123">Vám může nabídnout užitečné informace v následujících scénářích:</span><span class="sxs-lookup"><span data-stu-id="33e33-123">It may offer useful information in the following scenarios:</span></span>

* <span data-ttu-id="33e33-124">Monitorování dlouho běžící procesy, zobrazení průběhu mapy a snížit úlohy.</span><span class="sxs-lookup"><span data-stu-id="33e33-124">Monitoring long-running processes, viewing the progress of map and reduce tasks.</span></span>
* <span data-ttu-id="33e33-125">Analýza historické údaje o úspěšném nebo neúspěšném procesy, které se dozvíte, jak lze zlepšit zpracování nebo proč se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="33e33-125">Analyzing historical data for successful or failed processes to learn how processing could be improved or why it failed.</span></span>

## <a name="generate-a-dag"></a><span data-ttu-id="33e33-126">Generovat DAG</span><span class="sxs-lookup"><span data-stu-id="33e33-126">Generate a DAG</span></span>

<span data-ttu-id="33e33-127">Zobrazení Tez obsahuje pouze data, pokud úlohu, která používá modul Tez běží v současné době nebo byl byla dříve spuštěna.</span><span class="sxs-lookup"><span data-stu-id="33e33-127">The Tez view only contains data if a job that uses the Tez engine is currently running, or has been ran previously.</span></span> <span data-ttu-id="33e33-128">Jednoduché dotazů Hive, bez použití Tez lze vyřešit.</span><span class="sxs-lookup"><span data-stu-id="33e33-128">Simple Hive queries can be resolved without using Tez.</span></span> <span data-ttu-id="33e33-129">Složitější dotazy tento filtrování proveďte seskupení, řazení, spojení, atd.</span><span class="sxs-lookup"><span data-stu-id="33e33-129">More complex queries that do filtering, grouping, ordering, joins, etc.</span></span> <span data-ttu-id="33e33-130">Použití modulu Tez.</span><span class="sxs-lookup"><span data-stu-id="33e33-130">use the Tez engine.</span></span>

<span data-ttu-id="33e33-131">Pro spouštění dotazů Hive, který používá Tez použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="33e33-131">Use the following steps to run a Hive query that uses Tez:</span></span>

1. <span data-ttu-id="33e33-132">Ve webovém prohlížeči, přejděte na https://CLUSTERNAME.azurehdinsight.net, kde **CLUSTERNAME** je název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="33e33-132">In a web browser, navigate to https://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is the name of your HDInsight cluster.</span></span>

2. <span data-ttu-id="33e33-133">V nabídce v horní části stránky, vyberte **zobrazení** ikonu.</span><span class="sxs-lookup"><span data-stu-id="33e33-133">From the menu at the top of the page, select the **Views** icon.</span></span> <span data-ttu-id="33e33-134">Tato ikona vypadá jako řadu kvadratických hodnot.</span><span class="sxs-lookup"><span data-stu-id="33e33-134">This icon looks like a series of squares.</span></span> <span data-ttu-id="33e33-135">V rozevírací nabídce, která se zobrazí, vyberte **Hive zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="33e33-135">In the dropdown that appears, select **Hive view**.</span></span>

    ![Výběr zobrazení Hive](./media/hdinsight-debug-ambari-tez-view/selecthive.png)

3. <span data-ttu-id="33e33-137">Při načtení zobrazení Hive, vložte následující dotaz do editoru dotazů a pak klikněte na tlačítko **provést**.</span><span class="sxs-lookup"><span data-stu-id="33e33-137">When the Hive view loads, paste the following query into the Query Editor, and then click **execute**.</span></span>

        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;

    <span data-ttu-id="33e33-138">Po dokončení úlohy byste měli vidět výstup v zobrazí **výsledky zpracování dotazu** části.</span><span class="sxs-lookup"><span data-stu-id="33e33-138">Once the job has completed, you should see the output displayed in the **Query Process Results** section.</span></span> <span data-ttu-id="33e33-139">Výsledky by mělo být podobné následujícímu:</span><span class="sxs-lookup"><span data-stu-id="33e33-139">The results should be similar to the following text:</span></span>

        market  state       country
        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica

4. <span data-ttu-id="33e33-140">Vyberte **protokolu** kartě.</span><span class="sxs-lookup"><span data-stu-id="33e33-140">Select the **Log** tab.</span></span> <span data-ttu-id="33e33-141">Zobrazí informace podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="33e33-141">You see information similar to the following text:</span></span>

        INFO : Session is already open
        INFO :

        INFO : Status: Running (Executing on YARN cluster with App id application_1454546500517_0063)

    <span data-ttu-id="33e33-142">Uložit **id aplikace** hodnota, protože tato hodnota se používá v další části.</span><span class="sxs-lookup"><span data-stu-id="33e33-142">Save the **App id** value, as this value is used in the next section.</span></span>

## <a name="use-the-tez-view"></a><span data-ttu-id="33e33-143">Pomocí zobrazení Tez</span><span class="sxs-lookup"><span data-stu-id="33e33-143">Use the Tez View</span></span>

1. <span data-ttu-id="33e33-144">V nabídce v horní části stránky, vyberte **zobrazení** ikonu.</span><span class="sxs-lookup"><span data-stu-id="33e33-144">From the menu at the top of the page, select the **Views** icon.</span></span> <span data-ttu-id="33e33-145">V rozevírací nabídce, která se zobrazí, vyberte **Tez zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="33e33-145">In the dropdown that appears, select **Tez view**.</span></span>

    ![Výběr zobrazení Tez](./media/hdinsight-debug-ambari-tez-view/selecttez.png)

2. <span data-ttu-id="33e33-147">Při načtení zobrazení Tez, uvidíte, že seznam dotazů hive, které jsou aktuálně spuštěné, nebo byla spuštěna v clusteru.</span><span class="sxs-lookup"><span data-stu-id="33e33-147">When the Tez view loads, you see a list of hive queries that are currently running, or have been ran on the cluster.</span></span>

    ![Všechny DAG](./media/hdinsight-debug-ambari-tez-view/tez-view-home.png)

3. <span data-ttu-id="33e33-149">Pokud máte pouze jednu položku, je pro dotaz, který jste spustili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="33e33-149">If you have only one entry, it is for the query that you ran in the previous section.</span></span> <span data-ttu-id="33e33-150">Pokud máte více položek, může hledat pomocí pole v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="33e33-150">If you have multiple entries, you can search by using the fields at the top of the page.</span></span>

4. <span data-ttu-id="33e33-151">Vyberte **ID dotazu** pro dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="33e33-151">Select the **Query ID** for a Hive query.</span></span> <span data-ttu-id="33e33-152">Zobrazí se informace o dotazu.</span><span class="sxs-lookup"><span data-stu-id="33e33-152">Information about the query is displayed.</span></span>

    ![Podrobnosti o DAG](./media/hdinsight-debug-ambari-tez-view/query-details.png)

5. <span data-ttu-id="33e33-154">Karty na této stránce můžete zobrazit následující informace:</span><span class="sxs-lookup"><span data-stu-id="33e33-154">The tabs on this page allow you to view the following information:</span></span>

    * <span data-ttu-id="33e33-155">**Dotaz na podrobnosti**: Podrobnosti o dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="33e33-155">**Query Details**: Details about the Hive query.</span></span>
    * <span data-ttu-id="33e33-156">**Časová osa**: informace o tom, jak dlouho trvalo každé fáze zpracování.</span><span class="sxs-lookup"><span data-stu-id="33e33-156">**Timeline**: Information about how long each stage of processing took.</span></span>
    * <span data-ttu-id="33e33-157">**Konfigurace**: Konfigurace použitá pro tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="33e33-157">**Configurations**: The configuration used for this query.</span></span>

    <span data-ttu-id="33e33-158">Z __dotazu podrobnosti__ můžete pomocí odkazů můžete získat informace o __aplikace__ nebo __DAG__ pro tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="33e33-158">From __Query Details__ you can use the links to find information about the __Application__ or the __DAG__ for this query.</span></span>
    
    * <span data-ttu-id="33e33-159">__Aplikace__ odkaz zobrazí informace o aplikaci YARN pro tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="33e33-159">The __Application__ link displays information about the YARN application for this query.</span></span> <span data-ttu-id="33e33-160">Odsud můžete přejít do aplikačních protokolů YARN.</span><span class="sxs-lookup"><span data-stu-id="33e33-160">From here you can access the YARN application logs.</span></span>
    * <span data-ttu-id="33e33-161">__DAG__ odkaz zobrazí informace o necyklicky pro tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="33e33-161">The __DAG__ link displays information about the directed acyclic graph for this query.</span></span> <span data-ttu-id="33e33-162">Odtud můžete zobrazit grafické reprezentace DAG.</span><span class="sxs-lookup"><span data-stu-id="33e33-162">From here you can view a graphical representation of the DAG.</span></span> <span data-ttu-id="33e33-163">Můžete také získat informace o vrcholy v rámci DAG.</span><span class="sxs-lookup"><span data-stu-id="33e33-163">You can also find information on the vertices within the DAG.</span></span>

## <a name="next-steps"></a><span data-ttu-id="33e33-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="33e33-164">Next Steps</span></span>

<span data-ttu-id="33e33-165">Teď, když jste se naučili použití Tez zobrazení, další informace o [pomocí Hive v HDInsight](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="33e33-165">Now that you have learned how to use the Tez view, learn more about [Using Hive on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="33e33-166">Další podrobné technické informace o Tez naleznete v tématu [Tez stránku v Hortonworks](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="33e33-166">For more detailed technical information on Tez, see the [Tez page at Hortonworks](http://hortonworks.com/hadoop/tez/).</span></span>

<span data-ttu-id="33e33-167">Další informace o používání Ambari s HDInsight najdete v tématu [Správa clusterů HDInsight pomocí Ambari webového uživatelského rozhraní](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="33e33-167">For more information on using Ambari with HDInsight, see [Manage HDInsight clusters using the Ambari Web UI](hdinsight-hadoop-manage-ambari.md)</span></span>
