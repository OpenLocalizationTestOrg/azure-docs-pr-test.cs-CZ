---
title: "aaaUse zobrazení Ambari Tez s HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello Ambari Tez zobrazit úlohy Tez toodebug v HDInsight."
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
ms.openlocfilehash: 5d61bd0403c98284c86982073af91468ae62ac60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-ambari-views-toodebug-tez-jobs-on-hdinsight"></a><span data-ttu-id="3a153-103">Pomocí zobrazení Ambari toodebug Tez úloh v HDInsight</span><span class="sxs-lookup"><span data-stu-id="3a153-103">Use Ambari Views toodebug Tez Jobs on HDInsight</span></span>

<span data-ttu-id="3a153-104">Hello webové uživatelské rozhraní Ambari pro HDInsight obsahuje Tez zobrazení, které lze použít toounderstand a ladění úlohy, které používají Tez.</span><span class="sxs-lookup"><span data-stu-id="3a153-104">hello Ambari Web UI for HDInsight contains a Tez view that can be used toounderstand and debug jobs that use Tez.</span></span> <span data-ttu-id="3a153-105">zobrazení Tez Hello umožňuje toovisualize hello úlohy graf připojených položek, přejdete na každou položku a načíst informace o protokolování a statistiky.</span><span class="sxs-lookup"><span data-stu-id="3a153-105">hello Tez view allows you toovisualize hello job as a graph of connected items, drill into each item, and retrieve statistics and logging information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3a153-106">Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="3a153-106">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="3a153-107">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="3a153-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3a153-108">Další informace najdete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="3a153-108">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3a153-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3a153-109">Prerequisites</span></span>

* <span data-ttu-id="3a153-110">Cluster HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="3a153-110">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="3a153-111">Pokyny týkající se vytvoření clusteru, najdete v části [začněte používat HDInsight se systémem Linux](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3a153-111">For steps on creating a cluster, see [Get started using Linux-based HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
* <span data-ttu-id="3a153-112">Moderní webový prohlížeč, který podporuje HTML5.</span><span class="sxs-lookup"><span data-stu-id="3a153-112">A modern web browser that supports HTML5.</span></span>

## <a name="understanding-tez"></a><span data-ttu-id="3a153-113">Principy Tez</span><span class="sxs-lookup"><span data-stu-id="3a153-113">Understanding Tez</span></span>

<span data-ttu-id="3a153-114">Tez je rozšiřitelná architektura pro zpracování dat v Hadoop, která poskytuje vyšší rychlosti než tradiční zpracování prostředí MapReduce.</span><span class="sxs-lookup"><span data-stu-id="3a153-114">Tez is an extensible framework for data processing in Hadoop that provides greater speeds than traditional MapReduce processing.</span></span> <span data-ttu-id="3a153-115">Clustery HDInsight se systémem Linux je hello výchozí modul pro Hive.</span><span class="sxs-lookup"><span data-stu-id="3a153-115">For Linux-based HDInsight clusters, it is hello default engine for Hive.</span></span>

<span data-ttu-id="3a153-116">Tez vytvoří směrované Acyklické grafu (DAG) popisující hello pořadí akce požadované úlohami.</span><span class="sxs-lookup"><span data-stu-id="3a153-116">Tez creates a Directed Acyclic Graph (DAG) that describes hello order of actions required by jobs.</span></span> <span data-ttu-id="3a153-117">Jednotlivé akce se nazývají vrcholy a spouštět úsek hello celkové úlohy.</span><span class="sxs-lookup"><span data-stu-id="3a153-117">Individual actions are called vertices, and execute a piece of hello overall job.</span></span> <span data-ttu-id="3a153-118">skutečné provádění Hello popsaného Vrchol pracovní hello je volána úloha a mohou být distribuovány mezi několika uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="3a153-118">hello actual execution of hello work described by a vertex is called a task, and may be distributed across multiple nodes in hello cluster.</span></span>

### <a name="understanding-hello-tez-view"></a><span data-ttu-id="3a153-119">Principy hello Tez zobrazení</span><span class="sxs-lookup"><span data-stu-id="3a153-119">Understanding hello Tez view</span></span>

<span data-ttu-id="3a153-120">Hello Tez zobrazení poskytuje historické informace a informace na procesy, které jsou spuštěné.</span><span class="sxs-lookup"><span data-stu-id="3a153-120">hello Tez view provides both historical information and information on processes that are running.</span></span> <span data-ttu-id="3a153-121">Tyto informace ukazuje, jak je úlohu rozdělené mezi clustery.</span><span class="sxs-lookup"><span data-stu-id="3a153-121">This information shows how a job is distributed across clusters.</span></span> <span data-ttu-id="3a153-122">Také zobrazuje čítače používané úlohy a vrcholy a informace o chybě související s toohello úlohy.</span><span class="sxs-lookup"><span data-stu-id="3a153-122">It also displays counters used by tasks and vertices, and error information related toohello job.</span></span> <span data-ttu-id="3a153-123">Vám může nabídnout užitečné informace v hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="3a153-123">It may offer useful information in hello following scenarios:</span></span>

* <span data-ttu-id="3a153-124">Monitorování dlouho běžící procesy, zobrazení hello průběh mapy a snížit úlohy.</span><span class="sxs-lookup"><span data-stu-id="3a153-124">Monitoring long-running processes, viewing hello progress of map and reduce tasks.</span></span>
* <span data-ttu-id="3a153-125">Analýza historické údaje o úspěšném nebo neúspěšném procesy toolearn, jak lze zlepšit zpracování nebo proč se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="3a153-125">Analyzing historical data for successful or failed processes toolearn how processing could be improved or why it failed.</span></span>

## <a name="generate-a-dag"></a><span data-ttu-id="3a153-126">Generovat DAG</span><span class="sxs-lookup"><span data-stu-id="3a153-126">Generate a DAG</span></span>

<span data-ttu-id="3a153-127">Hello Tez zobrazení obsahuje data pouze, pokud úlohu, která používá hello modulu Tez je aktuálně spuštěna, nebo má již dříve spustil.</span><span class="sxs-lookup"><span data-stu-id="3a153-127">hello Tez view only contains data if a job that uses hello Tez engine is currently running, or has been ran previously.</span></span> <span data-ttu-id="3a153-128">Jednoduché dotazů Hive, bez použití Tez lze vyřešit.</span><span class="sxs-lookup"><span data-stu-id="3a153-128">Simple Hive queries can be resolved without using Tez.</span></span> <span data-ttu-id="3a153-129">Složitější dotazy tento filtrování proveďte seskupení, řazení, spojení, atd.</span><span class="sxs-lookup"><span data-stu-id="3a153-129">More complex queries that do filtering, grouping, ordering, joins, etc.</span></span> <span data-ttu-id="3a153-130">pomocí modulu Tez hello.</span><span class="sxs-lookup"><span data-stu-id="3a153-130">use hello Tez engine.</span></span>

<span data-ttu-id="3a153-131">Použijte následující postup toorun dotaz Hive, který používá Tez hello:</span><span class="sxs-lookup"><span data-stu-id="3a153-131">Use hello following steps toorun a Hive query that uses Tez:</span></span>

1. <span data-ttu-id="3a153-132">Ve webovém prohlížeči přejděte toohttps://CLUSTERNAME.azurehdinsight.net, kde **CLUSTERNAME** je hello název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3a153-132">In a web browser, navigate toohttps://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is hello name of your HDInsight cluster.</span></span>

2. <span data-ttu-id="3a153-133">Z nabídky hello hello horní části stránky hello vyberte hello **zobrazení** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3a153-133">From hello menu at hello top of hello page, select hello **Views** icon.</span></span> <span data-ttu-id="3a153-134">Tato ikona vypadá jako řadu kvadratických hodnot.</span><span class="sxs-lookup"><span data-stu-id="3a153-134">This icon looks like a series of squares.</span></span> <span data-ttu-id="3a153-135">V rozevírací nabídce hello, který se zobrazí, vyberte **Hive zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="3a153-135">In hello dropdown that appears, select **Hive view**.</span></span>

    ![Výběr zobrazení Hive](./media/hdinsight-debug-ambari-tez-view/selecthive.png)

3. <span data-ttu-id="3a153-137">Když hello zobrazení Hive načte, vložte následující hello dotaz do hello Editor dotazů a pak klikněte na **provést**.</span><span class="sxs-lookup"><span data-stu-id="3a153-137">When hello Hive view loads, paste hello following query into hello Query Editor, and then click **execute**.</span></span>

        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;

    <span data-ttu-id="3a153-138">Po dokončení úlohy hello byste měli vidět výstup hello zobrazí v hello **výsledky zpracování dotazu** části.</span><span class="sxs-lookup"><span data-stu-id="3a153-138">Once hello job has completed, you should see hello output displayed in hello **Query Process Results** section.</span></span> <span data-ttu-id="3a153-139">výsledky Hello by měl vypadat podobně jako toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="3a153-139">hello results should be similar toohello following text:</span></span>

        market  state       country
        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica

4. <span data-ttu-id="3a153-140">Vyberte hello **protokolu** kartě. Zobrazí informace o podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="3a153-140">Select hello **Log** tab. You see information similar toohello following text:</span></span>

        INFO : Session is already open
        INFO :

        INFO : Status: Running (Executing on YARN cluster with App id application_1454546500517_0063)

    <span data-ttu-id="3a153-141">Uložit hello **id aplikace** hodnota, protože tato hodnota se používá v další části hello.</span><span class="sxs-lookup"><span data-stu-id="3a153-141">Save hello **App id** value, as this value is used in hello next section.</span></span>

## <a name="use-hello-tez-view"></a><span data-ttu-id="3a153-142">Použití hello Tez zobrazení</span><span class="sxs-lookup"><span data-stu-id="3a153-142">Use hello Tez View</span></span>

1. <span data-ttu-id="3a153-143">Z nabídky hello hello horní části stránky hello vyberte hello **zobrazení** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3a153-143">From hello menu at hello top of hello page, select hello **Views** icon.</span></span> <span data-ttu-id="3a153-144">V rozevírací nabídce hello, který se zobrazí, vyberte **Tez zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="3a153-144">In hello dropdown that appears, select **Tez view**.</span></span>

    ![Výběr zobrazení Tez](./media/hdinsight-debug-ambari-tez-view/selecttez.png)

2. <span data-ttu-id="3a153-146">Při načtení hello Tez zobrazení uvidíte, že seznam dotazů hive, které jsou aktuálně spuštěné, nebo byla spuštěna v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="3a153-146">When hello Tez view loads, you see a list of hive queries that are currently running, or have been ran on hello cluster.</span></span>

    ![Všechny DAG](./media/hdinsight-debug-ambari-tez-view/tez-view-home.png)

3. <span data-ttu-id="3a153-148">Pokud máte pouze jednu položku, je pro hello dotaz, který jste spustili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="3a153-148">If you have only one entry, it is for hello query that you ran in hello previous section.</span></span> <span data-ttu-id="3a153-149">Pokud máte více položek, může hledat pomocí hello polí v hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="3a153-149">If you have multiple entries, you can search by using hello fields at hello top of hello page.</span></span>

4. <span data-ttu-id="3a153-150">Vyberte hello **ID dotazu** pro dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="3a153-150">Select hello **Query ID** for a Hive query.</span></span> <span data-ttu-id="3a153-151">Zobrazí se informace o dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="3a153-151">Information about hello query is displayed.</span></span>

    ![Podrobnosti o DAG](./media/hdinsight-debug-ambari-tez-view/query-details.png)

5. <span data-ttu-id="3a153-153">Hello karty na této stránce povolit tooview hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="3a153-153">hello tabs on this page allow you tooview hello following information:</span></span>

    * <span data-ttu-id="3a153-154">**Dotaz na podrobnosti**: Podrobnosti o dotaz Hive hello.</span><span class="sxs-lookup"><span data-stu-id="3a153-154">**Query Details**: Details about hello Hive query.</span></span>
    * <span data-ttu-id="3a153-155">**Časová osa**: informace o tom, jak dlouho trvalo každé fáze zpracování.</span><span class="sxs-lookup"><span data-stu-id="3a153-155">**Timeline**: Information about how long each stage of processing took.</span></span>
    * <span data-ttu-id="3a153-156">**Konfigurace**: Konfigurace hello použitá pro tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="3a153-156">**Configurations**: hello configuration used for this query.</span></span>

    <span data-ttu-id="3a153-157">Z __dotazu podrobnosti__ můžete hello odkazy toofind informací o hello __aplikace__ nebo hello __DAG__ pro tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="3a153-157">From __Query Details__ you can use hello links toofind information about hello __Application__ or hello __DAG__ for this query.</span></span>
    
    * <span data-ttu-id="3a153-158">Hello __aplikace__ odkaz zobrazí informace o hello YARN aplikace pro tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="3a153-158">hello __Application__ link displays information about hello YARN application for this query.</span></span> <span data-ttu-id="3a153-159">Odsud můžete přistupovat hello YARN aplikační protokoly.</span><span class="sxs-lookup"><span data-stu-id="3a153-159">From here you can access hello YARN application logs.</span></span>
    * <span data-ttu-id="3a153-160">Hello __DAG__ odkaz zobrazí informace o hello necyklicky pro tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="3a153-160">hello __DAG__ link displays information about hello directed acyclic graph for this query.</span></span> <span data-ttu-id="3a153-161">Odtud můžete zobrazit grafické reprezentace hello DAG.</span><span class="sxs-lookup"><span data-stu-id="3a153-161">From here you can view a graphical representation of hello DAG.</span></span> <span data-ttu-id="3a153-162">Můžete také získat informace o hello vrcholy v rámci hello DAG.</span><span class="sxs-lookup"><span data-stu-id="3a153-162">You can also find information on hello vertices within hello DAG.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a153-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3a153-163">Next Steps</span></span>

<span data-ttu-id="3a153-164">Teď, když jste se naučili, jak zobrazit toouse hello Tez, další informace o [pomocí Hive v HDInsight](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="3a153-164">Now that you have learned how toouse hello Tez view, learn more about [Using Hive on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="3a153-165">Další podrobné technické informace o Tez naleznete v tématu hello [Tez stránku v Hortonworks](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="3a153-165">For more detailed technical information on Tez, see hello [Tez page at Hortonworks](http://hortonworks.com/hadoop/tez/).</span></span>

<span data-ttu-id="3a153-166">Další informace o používání Ambari s HDInsight najdete v tématu [clusterů HDInsight spravovat pomocí hello webové uživatelské rozhraní Ambari](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="3a153-166">For more information on using Ambari with HDInsight, see [Manage HDInsight clusters using hello Ambari Web UI](hdinsight-hadoop-manage-ambari.md)</span></span>
