---
title: "aaaUse Tez uživatelského rozhraní s HDInsight se systémem Windows - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello uživatelského rozhraní Tez toodebug Tez úlohy na HDInsight HDInsight se systémem Windows."
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
ms.openlocfilehash: 7ae21242ee1f8dc34a8501bed1ca995480885540
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-tez-ui-toodebug-tez-jobs-on-windows-based-hdinsight"></a><span data-ttu-id="ef682-103">Použít na HDInsight se systémem Windows hello úlohách Tez toodebug Tez uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="ef682-103">Use hello Tez UI toodebug Tez Jobs on Windows-based HDInsight</span></span>
<span data-ttu-id="ef682-104">Hello Tez uživatelského rozhraní je webová stránka, kterou lze použít toounderstand a ladění úlohy, které používají Tez jako modul provádění hello v clusterech HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="ef682-104">hello Tez UI is a web page that can be used toounderstand and debug jobs that use Tez as hello execution engine on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="ef682-105">Hello Tez uživatelského rozhraní umožňuje toovisualize hello úlohy graf připojených položek, přejdete na každou položku a načíst informace o protokolování a statistiky.</span><span class="sxs-lookup"><span data-stu-id="ef682-105">hello Tez UI allows you toovisualize hello job as a graph of connected items, drill into each item, and retrieve statistics and logging information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ef682-106">Hello kroky v tomto dokumentu vyžadují clusteru HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="ef682-106">hello steps in this document require an HDInsight cluster that uses Windows.</span></span> <span data-ttu-id="ef682-107">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ef682-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ef682-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ef682-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef682-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ef682-109">Prerequisites</span></span>
* <span data-ttu-id="ef682-110">Cluster HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="ef682-110">A Windows-based HDInsight cluster.</span></span> <span data-ttu-id="ef682-111">Pokyny týkající se vytvoření nového clusteru, najdete v části [začněte používat HDInsight se systémem Windows](hdinsight-hadoop-tutorial-get-started-windows.md).</span><span class="sxs-lookup"><span data-stu-id="ef682-111">For steps on creating a new cluster, see [Get started using Windows-based HDInsight](hdinsight-hadoop-tutorial-get-started-windows.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="ef682-112">Hello Tez uživatelského rozhraní je dostupná pouze na clustery HDInsight se systémem Windows, které jsou vytvořené po 8. únoru 2016.</span><span class="sxs-lookup"><span data-stu-id="ef682-112">hello Tez UI is only available on Windows-based HDInsight clusters created after February 8th, 2016.</span></span>
  >
  >
* <span data-ttu-id="ef682-113">Klient vzdálené plochy se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="ef682-113">A Windows-based Remote Desktop client.</span></span>

## <a name="understanding-tez"></a><span data-ttu-id="ef682-114">Principy Tez</span><span class="sxs-lookup"><span data-stu-id="ef682-114">Understanding Tez</span></span>
<span data-ttu-id="ef682-115">Tez je rozšiřitelná architektura pro zpracování dat v Hadoop, která poskytuje vyšší rychlosti než tradiční zpracování prostředí MapReduce.</span><span class="sxs-lookup"><span data-stu-id="ef682-115">Tez is an extensible framework for data processing in Hadoop that provides greater speeds than traditional MapReduce processing.</span></span> <span data-ttu-id="ef682-116">Pro clustery HDInsight se systémem Windows je volitelné motoru, kterou můžete povolit pro Hive pomocí hello jako součást dotazu Hive následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ef682-116">For Windows-based HDInsight clusters, it is an optional engine that you can enable for Hive by using hello following command as part of your Hive query:</span></span>

    set hive.execution.engine=tez;

<span data-ttu-id="ef682-117">Odeslaná tooTez po pracovní vytvoří směrované Acyklické grafu (DAG) popisující hello pořadí provádění akcí hello vyžadovanou hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="ef682-117">When work is submitted tooTez, it creates a Directed Acyclic Graph (DAG) that describes hello order of execution of hello actions required by hello job.</span></span> <span data-ttu-id="ef682-118">Jednotlivé akce se nazývají vrcholy a spouštět úsek hello celkové úlohy.</span><span class="sxs-lookup"><span data-stu-id="ef682-118">Individual actions are called vertices, and execute a piece of hello overall job.</span></span> <span data-ttu-id="ef682-119">skutečné provádění Hello popsaného Vrchol pracovní hello je volána úloha a mohou být distribuovány mezi několika uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ef682-119">hello actual execution of hello work described by a vertex is called a task, and may be distributed across multiple nodes in hello cluster.</span></span>

### <a name="understanding-hello-tez-ui"></a><span data-ttu-id="ef682-120">Principy hello Tez uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="ef682-120">Understanding hello Tez UI</span></span>
<span data-ttu-id="ef682-121">Hello Tez uživatelského rozhraní je na webové stránce poskytuje informace o procesech, které jsou spuštěné, nebo byl již spuštěn pomocí Tez.</span><span class="sxs-lookup"><span data-stu-id="ef682-121">hello Tez UI is a web page provides information on processes that are running, or have previously ran using Tez.</span></span> <span data-ttu-id="ef682-122">Umožňuje tooview hello DAG generované Tez, jak je rozdělené mezi clustery, jako je množství paměti používané úlohy a vrcholy a informace o chybě čítače.</span><span class="sxs-lookup"><span data-stu-id="ef682-122">It allows you tooview hello DAG generated by Tez, how it is distributed across clusters, counters such as memory used by tasks and vertices, and error information.</span></span> <span data-ttu-id="ef682-123">Vám může nabídnout užitečné informace v hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="ef682-123">It may offer useful information in hello following scenarios:</span></span>

* <span data-ttu-id="ef682-124">Monitorování dlouho běžící procesy, zobrazení hello průběh mapy a snížit úlohy.</span><span class="sxs-lookup"><span data-stu-id="ef682-124">Monitoring long-running processes, viewing hello progress of map and reduce tasks.</span></span>
* <span data-ttu-id="ef682-125">Analýza historické údaje o úspěšném nebo neúspěšném procesy toolearn, jak lze zlepšit zpracování nebo proč se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="ef682-125">Analyzing historical data for successful or failed processes toolearn how processing could be improved or why it failed.</span></span>

## <a name="generate-a-dag"></a><span data-ttu-id="ef682-126">Generovat DAG</span><span class="sxs-lookup"><span data-stu-id="ef682-126">Generate a DAG</span></span>
<span data-ttu-id="ef682-127">Hello uživatelského rozhraní Tez bude obsahovat pouze data, pokud úlohu, která používá hello Tez modul běží v současné době nebo byl byla spuštěna v posledních hello.</span><span class="sxs-lookup"><span data-stu-id="ef682-127">hello Tez UI will only contain data if a job that uses hello Tez engine is currently running, or has been ran in hello past.</span></span> <span data-ttu-id="ef682-128">Jednoduché dotazů Hive obvykle lze přeložit bez použití Tez, ale složitější dotazy, které provádějí filtrování, seskupování, řazení, atd. spojení se obvykle vyžadují Tez.</span><span class="sxs-lookup"><span data-stu-id="ef682-128">Simple Hive queries can usually be resolved without using Tez, however more complex queries that do filtering, grouping, ordering, joins, etc. will usually require Tez.</span></span>

<span data-ttu-id="ef682-129">Pomocí následujících kroků toorun dotaz Hive, který provede pomocí Tez hello.</span><span class="sxs-lookup"><span data-stu-id="ef682-129">Use hello following steps toorun a Hive query that will execute using Tez.</span></span>

1. <span data-ttu-id="ef682-130">Ve webovém prohlížeči přejděte toohttps://CLUSTERNAME.azurehdinsight.net, kde **CLUSTERNAME** je hello název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ef682-130">In a web browser, navigate toohttps://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is hello name of your HDInsight cluster.</span></span>
2. <span data-ttu-id="ef682-131">Z nabídky hello hello horní části stránky hello vyberte hello **Hive Editor**.</span><span class="sxs-lookup"><span data-stu-id="ef682-131">From hello menu at hello top of hello page, select hello **Hive Editor**.</span></span> <span data-ttu-id="ef682-132">Zobrazí se stránka s hello následující příklad dotazu.</span><span class="sxs-lookup"><span data-stu-id="ef682-132">This will display a page with hello following example query.</span></span>

        Select * from hivesampletable

    <span data-ttu-id="ef682-133">Vymazat hello příklad dotazu a nahraďte ji metodou následující hello.</span><span class="sxs-lookup"><span data-stu-id="ef682-133">Erase hello example query and replace it with hello following.</span></span>

        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. <span data-ttu-id="ef682-134">Vyberte hello **odeslání** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ef682-134">Select hello **Submit** button.</span></span> <span data-ttu-id="ef682-135">Hello **úlohy relace** oddíl hello dolní části stránky hello se zobrazí stav hello hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="ef682-135">hello **Job Session** section at hello bottom of hello page will display hello status of hello query.</span></span> <span data-ttu-id="ef682-136">Jednou hello změny stavu příliš**dokončeno**, vyberte hello **zobrazit podrobnosti** odkaz tooview hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="ef682-136">Once hello status changes too**Completed**, select hello **View Details** link tooview hello results.</span></span> <span data-ttu-id="ef682-137">Hello **výstup úlohy** by měla být podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="ef682-137">hello **Job Output** should be similar toohello following:</span></span>

        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-hello-tez-ui"></a><span data-ttu-id="ef682-138">Použití hello Tez uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="ef682-138">Use hello Tez UI</span></span>
> [!NOTE]
> <span data-ttu-id="ef682-139">Hello Tez uživatelského rozhraní je dostupná pouze z plochy hello hello head uzlů clusteru, takže je nutné použít vzdálené plochy tooconnect toohello hlavních uzlech.</span><span class="sxs-lookup"><span data-stu-id="ef682-139">hello Tez UI is only available from hello desktop of hello cluster head nodes, so you must use Remote Desktop tooconnect toohello head nodes.</span></span>
>
>

1. <span data-ttu-id="ef682-140">Z hello [portál Azure](https://portal.azure.com), vyberte clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ef682-140">From hello [Azure portal](https://portal.azure.com), select your HDInsight cluster.</span></span> <span data-ttu-id="ef682-141">Hello horní části okna hello HDInsight, vyberte hello **vzdálené plochy** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ef682-141">From hello top of hello HDInsight blade, select hello **Remote Desktop** icon.</span></span> <span data-ttu-id="ef682-142">Bude se zobrazovat vzdálené plochy okno hello</span><span class="sxs-lookup"><span data-stu-id="ef682-142">This will display hello remote desktop blade</span></span>

    ![Ikona vzdálené plochy](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. <span data-ttu-id="ef682-144">V okně hello vzdálené plochy, vyberte **Connect** hlavního uzlu clusteru tooconnect toohello.</span><span class="sxs-lookup"><span data-stu-id="ef682-144">From hello Remote Desktop blade, select **Connect** tooconnect toohello cluster head node.</span></span> <span data-ttu-id="ef682-145">Pokud budete vyzváni, použijte hello clusteru připojení ke vzdálené ploše uživatelské jméno a heslo tooauthenticate hello.</span><span class="sxs-lookup"><span data-stu-id="ef682-145">When prompted, use hello cluster Remote Desktop user name and password tooauthenticate hello connection.</span></span>

    ![Ikona připojení vzdálené plochy](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)

   > [!NOTE]
   > <span data-ttu-id="ef682-147">Pokud jste nepovolili připojení vzdálené plochy, zadejte uživatelské jméno, heslo a datum vypršení platnosti a pak vyberte **povolit** tooenable vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="ef682-147">If you have not enabled Remote Desktop connectivity, provide a user name, password, and expiration date, then select **Enable** tooenable Remote Desktop.</span></span> <span data-ttu-id="ef682-148">Jakmile je povoleno, použijte hello předchozí kroky tooconnect.</span><span class="sxs-lookup"><span data-stu-id="ef682-148">Once it has been enabled, use hello previous steps tooconnect.</span></span>
   >
   >
3. <span data-ttu-id="ef682-149">Po připojení otevřete Internet Explorer na hello vzdálené plochy, vyberte hello ozubené kolečko ikonu v hello pravém horním rohu stránky hello prohlížeče a potom vyberte **nastavení kompatibilního zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ef682-149">Once connected, open Internet Explorer on hello remote desktop, select hello gear icon in hello upper right of hello browser, and then select **Compatibility View Settings**.</span></span>
4. <span data-ttu-id="ef682-150">Hello dolní části **nastavení kompatibilního zobrazení**, zrušte hello zaškrtávací políčko pro **zobrazit intranetové servery v kompatibilního zobrazení** a **použití Microsoft kompatibility seznamy**, a pak vyberte **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="ef682-150">From hello bottom of **Compatibility View Settings**, clear hello check box for **Display intranet sites in Compatibility View** and **Use Microsoft compatibility lists**, and then select **Close**.</span></span>
5. <span data-ttu-id="ef682-151">V aplikaci Internet Explorer procházet toohttp://headnodehost:8188/tezui / #/.</span><span class="sxs-lookup"><span data-stu-id="ef682-151">In Internet Explorer, browse toohttp://headnodehost:8188/tezui/#/.</span></span> <span data-ttu-id="ef682-152">Bude se zobrazovat hello Tez uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="ef682-152">This will display hello Tez UI</span></span>

    ![Tez uživatelského rozhraní](./media/hdinsight-debug-tez-ui/tezui.png)

    <span data-ttu-id="ef682-154">Až se načte hello Tez uživatelského rozhraní, zobrazí se, že seznam DAG, které jsou aktuálně spuštěné, nebo byla spuštěna v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ef682-154">When hello Tez UI loads, you will see a list of DAGs that are currently running, or have been ran on hello cluster.</span></span> <span data-ttu-id="ef682-155">Výchozí zobrazení Hello zahrnuje hello Dag název, Id, odesílatel, stav, čas spuštění, čas ukončení, doba trvání, ID aplikace a fronty.</span><span class="sxs-lookup"><span data-stu-id="ef682-155">hello default view includes hello Dag Name, Id, Submitter, Status, Start Time, End Time, Duration, Application ID, and Queue.</span></span> <span data-ttu-id="ef682-156">Pomocí ikony ozubené kolečko hello na hello napravo od stránku hello lze přidat více sloupců.</span><span class="sxs-lookup"><span data-stu-id="ef682-156">More columns can be added using hello gear icon at hello right of hello page.</span></span>

    <span data-ttu-id="ef682-157">Pokud máte pouze jednu položku, bude pro hello dotaz, který jste spustili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="ef682-157">If you have only one entry, it will be for hello query that you ran in hello previous section.</span></span> <span data-ttu-id="ef682-158">Pokud máte více položek, můžete vyhledat tak, že zadáte kritéria hledání v polích hello výše hello DAG a pak stiskněte tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="ef682-158">If you have multiple entries, you can search by entering search criteria in hello fields above hello DAGs, then hit **Enter**.</span></span>
6. <span data-ttu-id="ef682-159">Vyberte hello **Dag název** u hello nejnovější DAG položky.</span><span class="sxs-lookup"><span data-stu-id="ef682-159">Select hello **Dag Name** for hello most recent DAG entry.</span></span> <span data-ttu-id="ef682-160">Tato akce zobrazí informace o hello DAG, jakož i hello možnost toodownload zip soubory JSON, které obsahují informace o hello DAG.</span><span class="sxs-lookup"><span data-stu-id="ef682-160">This will display information about hello DAG, as well as hello option toodownload a zip of JSON files that contain information about hello DAG.</span></span>

    ![Podrobnosti o DAG](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. <span data-ttu-id="ef682-162">Výše hello **DAG podrobnosti** je několik odkazů, které se dají použít toodisplay informace o hello DAG.</span><span class="sxs-lookup"><span data-stu-id="ef682-162">Above hello **DAG Details** are several links that can be used toodisplay information about hello DAG.</span></span>

   * <span data-ttu-id="ef682-163">**Čítače DAG** zobrazí informace o čítačích pro tento DAG.</span><span class="sxs-lookup"><span data-stu-id="ef682-163">**DAG Counters** displays counters information for this DAG.</span></span>
   * <span data-ttu-id="ef682-164">**Grafické zobrazení** zobrazuje grafické reprezentace této DAG.</span><span class="sxs-lookup"><span data-stu-id="ef682-164">**Graphical View** displays a graphical representation of this DAG.</span></span>
   * <span data-ttu-id="ef682-165">**Všechny vrcholy** zobrazí seznam hello vrcholy v této DAG.</span><span class="sxs-lookup"><span data-stu-id="ef682-165">**All Vertices** displays a list of hello vertices in this DAG.</span></span>
   * <span data-ttu-id="ef682-166">**Všechny úlohy** zobrazí seznam hello úloh pro všechny vrcholy v této DAG.</span><span class="sxs-lookup"><span data-stu-id="ef682-166">**All Tasks** displays a list of hello tasks for all vertices in this DAG.</span></span>
   * <span data-ttu-id="ef682-167">**Všechny TaskAttempts** zobrazí informace o hello pokusů o zadání toorun úlohy pro tuto DAG.</span><span class="sxs-lookup"><span data-stu-id="ef682-167">**All TaskAttempts** displays information about hello attempts toorun tasks for this DAG.</span></span>

     > [!NOTE]
     > <span data-ttu-id="ef682-168">Pokud se posunete zobrazení sloupce hello vrcholy, úlohy a TaskAttempts, Všimněte si, že existují odkazy tooview **čítače** a **zobrazení nebo stažení protokolů** pro každý řádek.</span><span class="sxs-lookup"><span data-stu-id="ef682-168">If you scroll hello column display for Vertices, Tasks and TaskAttempts, notice that there are links tooview **counters** and **view or download logs** for each row.</span></span>
     >
     >

     <span data-ttu-id="ef682-169">Pokud došlo k selhání s úlohou hello, hello podrobnosti DAG se zobrazí stav se nezdařilo, spolu s odkazy tooinformation o neúspěšné úloze hello.</span><span class="sxs-lookup"><span data-stu-id="ef682-169">If there was a failure with hello job, hello DAG Details will display a status of FAILED, along with links tooinformation about hello failed task.</span></span> <span data-ttu-id="ef682-170">Diagnostické informace se zobrazí pod hello DAG podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ef682-170">Diagnostics information will be displayed beneath hello DAG details.</span></span>
8. <span data-ttu-id="ef682-171">Vyberte **grafické zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ef682-171">Select **Graphical View**.</span></span> <span data-ttu-id="ef682-172">Zobrazí se grafické reprezentace hello DAG.</span><span class="sxs-lookup"><span data-stu-id="ef682-172">This displays a graphical representation of hello DAG.</span></span> <span data-ttu-id="ef682-173">Každý vrchol hello zobrazení toodisplay informace o tom můžete umístit hello myši.</span><span class="sxs-lookup"><span data-stu-id="ef682-173">You can place hello mouse over each vertex in hello view toodisplay information about it.</span></span>

    ![Grafické zobrazení](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. <span data-ttu-id="ef682-175">Kliknutím na vrchol načte hello **vrchol podrobnosti** pro tuto položku.</span><span class="sxs-lookup"><span data-stu-id="ef682-175">Clicking on a vertex will load hello **Vertex Details** for that item.</span></span> <span data-ttu-id="ef682-176">Klikněte na hello **mapy 1** vrchol toodisplay podrobnosti pro tuto položku.</span><span class="sxs-lookup"><span data-stu-id="ef682-176">Click on hello **Map 1** vertex toodisplay details for this item.</span></span> <span data-ttu-id="ef682-177">Vyberte **potvrdit** tooconfirm hello navigace.</span><span class="sxs-lookup"><span data-stu-id="ef682-177">Select **Confirm** tooconfirm hello navigation.</span></span>

    ![Vrchol podrobnosti](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. <span data-ttu-id="ef682-179">Upozorňujeme ale, nyní se odkazy v horní části hello hello stránky, které jsou související toovertices a úloh.</span><span class="sxs-lookup"><span data-stu-id="ef682-179">Note that you now have links at hello top of hello page that are related toovertices and tasks.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ef682-180">Tato stránka také můžete dospět tak, že přejdete příliš**DAG podrobnosti**, vyberete **vrchol podrobnosti**a potom vyberete hello **mapy 1** vrchol.</span><span class="sxs-lookup"><span data-stu-id="ef682-180">You can also arrive at this page by going back too**DAG Details**, selecting **Vertex Details**, and then selecting hello **Map 1** vertex.</span></span>
    >
    >

    * <span data-ttu-id="ef682-181">**Vrchol čítače** zobrazí čítače informace pro tento vrchol.</span><span class="sxs-lookup"><span data-stu-id="ef682-181">**Vertex Counters** displays counter information for this vertex.</span></span>
    * <span data-ttu-id="ef682-182">**Úlohy** zobrazuje úlohy, které pro tento vrchol.</span><span class="sxs-lookup"><span data-stu-id="ef682-182">**Tasks** displays tasks for this vertex.</span></span>
    * <span data-ttu-id="ef682-183">**Úloha pokusy o** zobrazí informace o úlohách toorun pokusů pro tento vrchol.</span><span class="sxs-lookup"><span data-stu-id="ef682-183">**Task Attempts** displays information about attempts toorun tasks for this vertex.</span></span>
    * <span data-ttu-id="ef682-184">**Zdroje & jímky** zobrazí zdroje dat a pro tento vrchol jímky.</span><span class="sxs-lookup"><span data-stu-id="ef682-184">**Sources & Sinks** displays data sources and sinks for this vertex.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ef682-185">S předchozí nabídce hello při posouvání hello sloupec zobrazení pro úlohy, propojí pokusy o úloh a zdroje & Sinks__ toodisplay toomore informace pro každou položku.</span><span class="sxs-lookup"><span data-stu-id="ef682-185">As with hello previous menu, you can scroll hello column display for Tasks, Task Attempts, and Sources & Sinks__ toodisplay links toomore information for each item.</span></span>
      >
      >
11. <span data-ttu-id="ef682-186">Vyberte **úlohy**, a pak vyberte hello položku s názvem **00_000000**.</span><span class="sxs-lookup"><span data-stu-id="ef682-186">Select **Tasks**, and then select hello item named **00_000000**.</span></span> <span data-ttu-id="ef682-187">Bude se zobrazovat **podrobnosti úlohy** pro tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="ef682-187">This will display **Task Details** for this task.</span></span> <span data-ttu-id="ef682-188">Na této obrazovce můžete zobrazit **úloh čítače** a **úloh pokusy**.</span><span class="sxs-lookup"><span data-stu-id="ef682-188">From this screen, you can view **Task Counters** and **Task Attempts**.</span></span>

    ![Podrobnosti úlohy](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a><span data-ttu-id="ef682-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ef682-190">Next Steps</span></span>
<span data-ttu-id="ef682-191">Teď, když jste se naučili, jak zobrazit toouse hello Tez, další informace o [pomocí Hive v HDInsight](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="ef682-191">Now that you have learned how toouse hello Tez view, learn more about [Using Hive on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="ef682-192">Další podrobné technické informace o Tez naleznete v tématu hello [Tez stránku v Hortonworks](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="ef682-192">For more detailed technical information on Tez, see hello [Tez page at Hortonworks](http://hortonworks.com/hadoop/tez/).</span></span>
