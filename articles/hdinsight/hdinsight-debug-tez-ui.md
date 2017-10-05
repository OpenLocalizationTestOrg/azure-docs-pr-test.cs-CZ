---
title: "Použijte uživatelské rozhraní Tez s HDInsight se systémem Windows - Azure | Microsoft Docs"
description: "Zjistěte, jak pomocí uživatelského rozhraní Tez k ladění úlohách Tez na HDInsight HDInsight se systémem Windows."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a55bccb9-7c32-4ff2-b654-213a2354bd5c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 3889fa1c3523eb0330cbe3b7640fd8590a5ceadf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-tez-ui-to-debug-tez-jobs-on-windows-based-hdinsight"></a><span data-ttu-id="9ff93-103">Chcete-li ladit úlohách Tez na HDInsight se systémem Windows pomocí uživatelského rozhraní Tez</span><span class="sxs-lookup"><span data-stu-id="9ff93-103">Use the Tez UI to debug Tez Jobs on Windows-based HDInsight</span></span>
<span data-ttu-id="9ff93-104">Rozhraní Tez je webová stránka, která můžete použít k pochopení a ladění úlohy, které používají Tez jako modul spouštění v clusterech HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="9ff93-104">The Tez UI is a web page that can be used to understand and debug jobs that use Tez as the execution engine on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="9ff93-105">Rozhraní Tez umožňuje vizualizovat úlohu jako graf připojených položek, přejdete na každou položku a načíst informace o protokolování a statistiky.</span><span class="sxs-lookup"><span data-stu-id="9ff93-105">The Tez UI allows you to visualize the job as a graph of connected items, drill into each item, and retrieve statistics and logging information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9ff93-106">Kroky v tomto dokumentu vyžadují clusteru HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="9ff93-106">The steps in this document require an HDInsight cluster that uses Windows.</span></span> <span data-ttu-id="9ff93-107">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="9ff93-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="9ff93-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="9ff93-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ff93-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9ff93-109">Prerequisites</span></span>
* <span data-ttu-id="9ff93-110">Cluster HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="9ff93-110">A Windows-based HDInsight cluster.</span></span> <span data-ttu-id="9ff93-111">Pokyny týkající se vytvoření nového clusteru, najdete v části [začněte používat HDInsight se systémem Windows](hdinsight-hadoop-tutorial-get-started-windows.md).</span><span class="sxs-lookup"><span data-stu-id="9ff93-111">For steps on creating a new cluster, see [Get started using Windows-based HDInsight](hdinsight-hadoop-tutorial-get-started-windows.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="9ff93-112">Rozhraní Tez je dostupná pouze na clustery HDInsight se systémem Windows, které jsou vytvořené po 8. únoru 2016.</span><span class="sxs-lookup"><span data-stu-id="9ff93-112">The Tez UI is only available on Windows-based HDInsight clusters created after February 8th, 2016.</span></span>
  >
  >
* <span data-ttu-id="9ff93-113">Klient vzdálené plochy se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="9ff93-113">A Windows-based Remote Desktop client.</span></span>

## <a name="understanding-tez"></a><span data-ttu-id="9ff93-114">Principy Tez</span><span class="sxs-lookup"><span data-stu-id="9ff93-114">Understanding Tez</span></span>
<span data-ttu-id="9ff93-115">Tez je rozšiřitelná architektura pro zpracování dat v Hadoop, která poskytuje vyšší rychlosti než tradiční zpracování prostředí MapReduce.</span><span class="sxs-lookup"><span data-stu-id="9ff93-115">Tez is an extensible framework for data processing in Hadoop that provides greater speeds than traditional MapReduce processing.</span></span> <span data-ttu-id="9ff93-116">Pro clustery HDInsight se systémem Windows je volitelné motoru, kterou můžete povolit pro Hive pomocí následujícího příkazu v rámci dotazu Hive:</span><span class="sxs-lookup"><span data-stu-id="9ff93-116">For Windows-based HDInsight clusters, it is an optional engine that you can enable for Hive by using the following command as part of your Hive query:</span></span>

    set hive.execution.engine=tez;

<span data-ttu-id="9ff93-117">Při odeslání pracovní Tez, vytvoří směrované Acyklické grafu (DAG) popisující pořadí provádění akcí požadovaných úlohou.</span><span class="sxs-lookup"><span data-stu-id="9ff93-117">When work is submitted to Tez, it creates a Directed Acyclic Graph (DAG) that describes the order of execution of the actions required by the job.</span></span> <span data-ttu-id="9ff93-118">Jednotlivé akce se nazývají vrcholy a provést část celkového úlohy.</span><span class="sxs-lookup"><span data-stu-id="9ff93-118">Individual actions are called vertices, and execute a piece of the overall job.</span></span> <span data-ttu-id="9ff93-119">Skutečné provádění pracovní popsaného vrchol je volána úloha a mohou být distribuovány mezi několika uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="9ff93-119">The actual execution of the work described by a vertex is called a task, and may be distributed across multiple nodes in the cluster.</span></span>

### <a name="understanding-the-tez-ui"></a><span data-ttu-id="9ff93-120">Vysvětlení rozhraní Tez</span><span class="sxs-lookup"><span data-stu-id="9ff93-120">Understanding the Tez UI</span></span>
<span data-ttu-id="9ff93-121">Rozhraní Tez je na webové stránce poskytuje informace o procesech, které jsou spuštěné, nebo byl již spuštěn pomocí Tez.</span><span class="sxs-lookup"><span data-stu-id="9ff93-121">The Tez UI is a web page provides information on processes that are running, or have previously ran using Tez.</span></span> <span data-ttu-id="9ff93-122">Umožňuje zobrazit DAG generované Tez, jak je rozdělené mezi clustery, jako je množství paměti používané úlohy a vrcholy a informace o chybě čítače.</span><span class="sxs-lookup"><span data-stu-id="9ff93-122">It allows you to view the DAG generated by Tez, how it is distributed across clusters, counters such as memory used by tasks and vertices, and error information.</span></span> <span data-ttu-id="9ff93-123">Vám může nabídnout užitečné informace v následujících scénářích:</span><span class="sxs-lookup"><span data-stu-id="9ff93-123">It may offer useful information in the following scenarios:</span></span>

* <span data-ttu-id="9ff93-124">Monitorování dlouho běžící procesy, zobrazení průběhu mapy a snížit úlohy.</span><span class="sxs-lookup"><span data-stu-id="9ff93-124">Monitoring long-running processes, viewing the progress of map and reduce tasks.</span></span>
* <span data-ttu-id="9ff93-125">Analýza historické údaje o úspěšném nebo neúspěšném procesy, které se dozvíte, jak lze zlepšit zpracování nebo proč se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="9ff93-125">Analyzing historical data for successful or failed processes to learn how processing could be improved or why it failed.</span></span>

## <a name="generate-a-dag"></a><span data-ttu-id="9ff93-126">Generovat DAG</span><span class="sxs-lookup"><span data-stu-id="9ff93-126">Generate a DAG</span></span>
<span data-ttu-id="9ff93-127">Rozhraní Tez bude obsahovat pouze data, pokud úlohu, která používá modul Tez běží v současné době nebo byl byla spuštěna v minulosti.</span><span class="sxs-lookup"><span data-stu-id="9ff93-127">The Tez UI will only contain data if a job that uses the Tez engine is currently running, or has been ran in the past.</span></span> <span data-ttu-id="9ff93-128">Jednoduché dotazů Hive obvykle lze přeložit bez použití Tez, ale složitější dotazy, které provádějí filtrování, seskupování, řazení, atd. spojení se obvykle vyžadují Tez.</span><span class="sxs-lookup"><span data-stu-id="9ff93-128">Simple Hive queries can usually be resolved without using Tez, however more complex queries that do filtering, grouping, ordering, joins, etc. will usually require Tez.</span></span>

<span data-ttu-id="9ff93-129">Použijte následující postup ke spuštění dotazu Hive, která se spustí pomocí Tez.</span><span class="sxs-lookup"><span data-stu-id="9ff93-129">Use the following steps to run a Hive query that will execute using Tez.</span></span>

1. <span data-ttu-id="9ff93-130">Ve webovém prohlížeči, přejděte na https://CLUSTERNAME.azurehdinsight.net, kde **CLUSTERNAME** je název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9ff93-130">In a web browser, navigate to https://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is the name of your HDInsight cluster.</span></span>
2. <span data-ttu-id="9ff93-131">V nabídce v horní části stránky, vyberte **Hive Editor**.</span><span class="sxs-lookup"><span data-stu-id="9ff93-131">From the menu at the top of the page, select the **Hive Editor**.</span></span> <span data-ttu-id="9ff93-132">Zobrazí se stránka s následující příklad dotazu.</span><span class="sxs-lookup"><span data-stu-id="9ff93-132">This will display a page with the following example query.</span></span>

        Select * from hivesampletable

    <span data-ttu-id="9ff93-133">Erase – příklad dotazu a nahraďte ji následujícím textem.</span><span class="sxs-lookup"><span data-stu-id="9ff93-133">Erase the example query and replace it with the following.</span></span>

        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. <span data-ttu-id="9ff93-134">Vyberte **odeslání** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ff93-134">Select the **Submit** button.</span></span> <span data-ttu-id="9ff93-135">**Úlohy relace** v dolní části stránky se zobrazí stav dotazu.</span><span class="sxs-lookup"><span data-stu-id="9ff93-135">The **Job Session** section at the bottom of the page will display the status of the query.</span></span> <span data-ttu-id="9ff93-136">Jakmile se stav změní na **dokončeno**, vyberte **zobrazit podrobnosti** odkaz zobrazíte výsledky.</span><span class="sxs-lookup"><span data-stu-id="9ff93-136">Once the status changes to **Completed**, select the **View Details** link to view the results.</span></span> <span data-ttu-id="9ff93-137">**Výstup úlohy** by mělo být podobné následujícímu:</span><span class="sxs-lookup"><span data-stu-id="9ff93-137">The **Job Output** should be similar to the following:</span></span>

        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-the-tez-ui"></a><span data-ttu-id="9ff93-138">Pomocí uživatelského rozhraní Tez</span><span class="sxs-lookup"><span data-stu-id="9ff93-138">Use the Tez UI</span></span>
> [!NOTE]
> <span data-ttu-id="9ff93-139">Rozhraní Tez je dostupná pouze z plochy hlavních uzlech clusteru, je nutné použít vzdálené plochy pro připojení k hlavnímu uzlu.</span><span class="sxs-lookup"><span data-stu-id="9ff93-139">The Tez UI is only available from the desktop of the cluster head nodes, so you must use Remote Desktop to connect to the head nodes.</span></span>
>
>

1. <span data-ttu-id="9ff93-140">Z [portál Azure](https://portal.azure.com), vyberte clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9ff93-140">From the [Azure portal](https://portal.azure.com), select your HDInsight cluster.</span></span> <span data-ttu-id="9ff93-141">Z horní části okna HDInsight, vyberte **vzdálené plochy** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9ff93-141">From the top of the HDInsight blade, select the **Remote Desktop** icon.</span></span> <span data-ttu-id="9ff93-142">Tato akce zobrazí okno Vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="9ff93-142">This will display the remote desktop blade</span></span>

    ![Ikona vzdálené plochy](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. <span data-ttu-id="9ff93-144">V okně připojení ke vzdálené ploše vyberte **Connect** pro připojení k hlavnímu uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="9ff93-144">From the Remote Desktop blade, select **Connect** to connect to the cluster head node.</span></span> <span data-ttu-id="9ff93-145">Pokud budete vyzváni, použijte clusteru vzdálené plochy uživatelské jméno a heslo k ověření připojení.</span><span class="sxs-lookup"><span data-stu-id="9ff93-145">When prompted, use the cluster Remote Desktop user name and password to authenticate the connection.</span></span>

    ![Ikona připojení vzdálené plochy](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)

   > [!NOTE]
   > <span data-ttu-id="9ff93-147">Pokud jste nepovolili připojení vzdálené plochy, zadejte uživatelské jméno, heslo a datum vypršení platnosti a pak vyberte **povolit** k povolení služby Vzdálená plocha.</span><span class="sxs-lookup"><span data-stu-id="9ff93-147">If you have not enabled Remote Desktop connectivity, provide a user name, password, and expiration date, then select **Enable** to enable Remote Desktop.</span></span> <span data-ttu-id="9ff93-148">Poté, co byla povolena, použijte předchozí kroky pro připojení.</span><span class="sxs-lookup"><span data-stu-id="9ff93-148">Once it has been enabled, use the previous steps to connect.</span></span>
   >
   >
3. <span data-ttu-id="9ff93-149">Po připojení otevřete Internet Explorer na vzdálenou plochu, vyberte ikonu ozubené kolečko v pravém horním rohu stránky prohlížeče a pak vyberte **nastavení kompatibilního zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="9ff93-149">Once connected, open Internet Explorer on the remote desktop, select the gear icon in the upper right of the browser, and then select **Compatibility View Settings**.</span></span>
4. <span data-ttu-id="9ff93-150">V dolní části **nastavení kompatibilního zobrazení**, zrušte zaškrtnutí políčka pro **zobrazit intranetové servery v kompatibilního zobrazení** a **použití Microsoft kompatibility seznamy**, a potom vyberte **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="9ff93-150">From the bottom of **Compatibility View Settings**, clear the check box for **Display intranet sites in Compatibility View** and **Use Microsoft compatibility lists**, and then select **Close**.</span></span>
5. <span data-ttu-id="9ff93-151">V Internet Exploreru přejděte do http://headnodehost:8188/tezui / #/.</span><span class="sxs-lookup"><span data-stu-id="9ff93-151">In Internet Explorer, browse to http://headnodehost:8188/tezui/#/.</span></span> <span data-ttu-id="9ff93-152">Bude se zobrazovat Tez uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="9ff93-152">This will display the Tez UI</span></span>

    ![Tez uživatelského rozhraní](./media/hdinsight-debug-tez-ui/tezui.png)

    <span data-ttu-id="9ff93-154">Až se načte Tez uživatelského rozhraní, zobrazí se, že seznam DAG, které jsou aktuálně spuštěné, nebo byla spuštěna v clusteru.</span><span class="sxs-lookup"><span data-stu-id="9ff93-154">When the Tez UI loads, you will see a list of DAGs that are currently running, or have been ran on the cluster.</span></span> <span data-ttu-id="9ff93-155">Výchozí zobrazení zahrnuje Dag název, Id, odesílatel, stav, čas spuštění, čas ukončení, doba trvání, ID aplikace a fronty.</span><span class="sxs-lookup"><span data-stu-id="9ff93-155">The default view includes the Dag Name, Id, Submitter, Status, Start Time, End Time, Duration, Application ID, and Queue.</span></span> <span data-ttu-id="9ff93-156">Pomocí ikony ozubené kolečko na pravé straně stránky lze přidat více sloupců.</span><span class="sxs-lookup"><span data-stu-id="9ff93-156">More columns can be added using the gear icon at the right of the page.</span></span>

    <span data-ttu-id="9ff93-157">Pokud máte pouze jednu položku, bude pro dotaz, který jste spustili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="9ff93-157">If you have only one entry, it will be for the query that you ran in the previous section.</span></span> <span data-ttu-id="9ff93-158">Pokud máte více položek, můžete vyhledat tak, že zadáte kritéria hledání v polích nad DAG a pak stiskněte tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="9ff93-158">If you have multiple entries, you can search by entering search criteria in the fields above the DAGs, then hit **Enter**.</span></span>
6. <span data-ttu-id="9ff93-159">Vyberte **Dag název** u nejnovější položky DAG.</span><span class="sxs-lookup"><span data-stu-id="9ff93-159">Select the **Dag Name** for the most recent DAG entry.</span></span> <span data-ttu-id="9ff93-160">Tato akce zobrazí informace o DAG, a také možnost stáhnout zip soubory JSON, které obsahují informace o DAG.</span><span class="sxs-lookup"><span data-stu-id="9ff93-160">This will display information about the DAG, as well as the option to download a zip of JSON files that contain information about the DAG.</span></span>

    ![Podrobnosti o DAG](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. <span data-ttu-id="9ff93-162">Výše **DAG podrobnosti** je několik odkazů, které lze použít k zobrazení informací o DAG.</span><span class="sxs-lookup"><span data-stu-id="9ff93-162">Above the **DAG Details** are several links that can be used to display information about the DAG.</span></span>

   * <span data-ttu-id="9ff93-163">**Čítače DAG** zobrazí informace o čítačích pro tento DAG.</span><span class="sxs-lookup"><span data-stu-id="9ff93-163">**DAG Counters** displays counters information for this DAG.</span></span>
   * <span data-ttu-id="9ff93-164">**Grafické zobrazení** zobrazuje grafické reprezentace této DAG.</span><span class="sxs-lookup"><span data-stu-id="9ff93-164">**Graphical View** displays a graphical representation of this DAG.</span></span>
   * <span data-ttu-id="9ff93-165">**Všechny vrcholy** zobrazí seznam vrcholy v této DAG.</span><span class="sxs-lookup"><span data-stu-id="9ff93-165">**All Vertices** displays a list of the vertices in this DAG.</span></span>
   * <span data-ttu-id="9ff93-166">**Všechny úlohy** zobrazí seznam úloh pro všechny vrcholy v této DAG.</span><span class="sxs-lookup"><span data-stu-id="9ff93-166">**All Tasks** displays a list of the tasks for all vertices in this DAG.</span></span>
   * <span data-ttu-id="9ff93-167">**Všechny TaskAttempts** zobrazí informace o pokusí spustit úlohy pro tuto DAG.</span><span class="sxs-lookup"><span data-stu-id="9ff93-167">**All TaskAttempts** displays information about the attempts to run tasks for this DAG.</span></span>

     > [!NOTE]
     > <span data-ttu-id="9ff93-168">Pokud se posunete zobrazení sloupce pro vrcholy, úlohy a TaskAttempts, Všimněte si, že jsou odkazů pro zobrazení **čítače** a **zobrazení nebo stažení protokolů** pro každý řádek.</span><span class="sxs-lookup"><span data-stu-id="9ff93-168">If you scroll the column display for Vertices, Tasks and TaskAttempts, notice that there are links to view **counters** and **view or download logs** for each row.</span></span>
     >
     >

     <span data-ttu-id="9ff93-169">Pokud došlo k selhání s úlohou, podrobnosti DAG se zobrazí stav se nezdařilo, spolu s odkazy na informace o neúspěšné úloze.</span><span class="sxs-lookup"><span data-stu-id="9ff93-169">If there was a failure with the job, the DAG Details will display a status of FAILED, along with links to information about the failed task.</span></span> <span data-ttu-id="9ff93-170">Diagnostické informace se zobrazí pod podrobnosti DAG.</span><span class="sxs-lookup"><span data-stu-id="9ff93-170">Diagnostics information will be displayed beneath the DAG details.</span></span>
8. <span data-ttu-id="9ff93-171">Vyberte **grafické zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="9ff93-171">Select **Graphical View**.</span></span> <span data-ttu-id="9ff93-172">Zobrazí se grafické reprezentace DAG.</span><span class="sxs-lookup"><span data-stu-id="9ff93-172">This displays a graphical representation of the DAG.</span></span> <span data-ttu-id="9ff93-173">Myši můžete umístit každý vrchol, v zobrazení pro zobrazení informací o něm.</span><span class="sxs-lookup"><span data-stu-id="9ff93-173">You can place the mouse over each vertex in the view to display information about it.</span></span>

    ![Grafické zobrazení](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. <span data-ttu-id="9ff93-175">Kliknutím na vrchol načte **vrchol podrobnosti** pro tuto položku.</span><span class="sxs-lookup"><span data-stu-id="9ff93-175">Clicking on a vertex will load the **Vertex Details** for that item.</span></span> <span data-ttu-id="9ff93-176">Klikněte na **mapy 1** vrchol si můžete zobrazit podrobnosti pro tuto položku.</span><span class="sxs-lookup"><span data-stu-id="9ff93-176">Click on the **Map 1** vertex to display details for this item.</span></span> <span data-ttu-id="9ff93-177">Vyberte **potvrdit** potvrďte navigaci.</span><span class="sxs-lookup"><span data-stu-id="9ff93-177">Select **Confirm** to confirm the navigation.</span></span>

    ![Vrchol podrobnosti](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. <span data-ttu-id="9ff93-179">Upozorňujeme ale, nyní se odkazy v horní části stránky, která se vztahují k vrcholy a úloh.</span><span class="sxs-lookup"><span data-stu-id="9ff93-179">Note that you now have links at the top of the page that are related to vertices and tasks.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9ff93-180">Můžete se na tuto stránku tak, že přejdete zpět na také doručení **DAG podrobnosti**, vyberete **vrchol podrobnosti**a potom výběrem **mapy 1** vrchol.</span><span class="sxs-lookup"><span data-stu-id="9ff93-180">You can also arrive at this page by going back to **DAG Details**, selecting **Vertex Details**, and then selecting the **Map 1** vertex.</span></span>
    >
    >

    * <span data-ttu-id="9ff93-181">**Vrchol čítače** zobrazí čítače informace pro tento vrchol.</span><span class="sxs-lookup"><span data-stu-id="9ff93-181">**Vertex Counters** displays counter information for this vertex.</span></span>
    * <span data-ttu-id="9ff93-182">**Úlohy** zobrazuje úlohy, které pro tento vrchol.</span><span class="sxs-lookup"><span data-stu-id="9ff93-182">**Tasks** displays tasks for this vertex.</span></span>
    * <span data-ttu-id="9ff93-183">**Úloha pokusy o** zobrazí informace o pokusí spustit úlohy pro tuto vrchol.</span><span class="sxs-lookup"><span data-stu-id="9ff93-183">**Task Attempts** displays information about attempts to run tasks for this vertex.</span></span>
    * <span data-ttu-id="9ff93-184">**Zdroje & jímky** zobrazí zdroje dat a pro tento vrchol jímky.</span><span class="sxs-lookup"><span data-stu-id="9ff93-184">**Sources & Sinks** displays data sources and sinks for this vertex.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9ff93-185">Jako s předchozí nabídky můžete posuňte zobrazení sloupce pro úlohy, pokusy o úloh a zdroje & Sinks__ zobrazíte odkazy na další informace pro každou položku.</span><span class="sxs-lookup"><span data-stu-id="9ff93-185">As with the previous menu, you can scroll the column display for Tasks, Task Attempts, and Sources & Sinks__ to display links to more information for each item.</span></span>
      >
      >
11. <span data-ttu-id="9ff93-186">Vyberte **úlohy**a potom vyberte položku s názvem **00_000000**.</span><span class="sxs-lookup"><span data-stu-id="9ff93-186">Select **Tasks**, and then select the item named **00_000000**.</span></span> <span data-ttu-id="9ff93-187">Bude se zobrazovat **podrobnosti úlohy** pro tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="9ff93-187">This will display **Task Details** for this task.</span></span> <span data-ttu-id="9ff93-188">Na této obrazovce můžete zobrazit **úloh čítače** a **úloh pokusy**.</span><span class="sxs-lookup"><span data-stu-id="9ff93-188">From this screen, you can view **Task Counters** and **Task Attempts**.</span></span>

    ![Podrobnosti úlohy](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a><span data-ttu-id="9ff93-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ff93-190">Next Steps</span></span>
<span data-ttu-id="9ff93-191">Teď, když jste se naučili použití Tez zobrazení, další informace o [pomocí Hive v HDInsight](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="9ff93-191">Now that you have learned how to use the Tez view, learn more about [Using Hive on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="9ff93-192">Další podrobné technické informace o Tez naleznete v tématu [Tez stránku v Hortonworks](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="9ff93-192">For more detailed technical information on Tez, see the [Tez page at Hortonworks](http://hortonworks.com/hadoop/tez/).</span></span>
