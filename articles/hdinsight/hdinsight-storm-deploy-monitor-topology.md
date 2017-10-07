---
title: "aaaDeploy a správa topologií Apache Storm v HDInsight | Microsoft Docs"
description: "Zjistěte, jak toodeploy, monitorování a správa topologií Apache Storm pomocí hello řídicí panel Storm v HDInsight. Pomocí nástroje Hadoop pro sadu Visual Studio."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5e542072-f014-42aa-82d6-2694a76df520
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 05f05fe8dd519fe99fb771d36bfc3d28168ca85f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a><span data-ttu-id="fff8d-104">Nasazení a správa topologií Apache Storm v HDInsight se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="fff8d-104">Deploy and manage Apache Storm topologies on Windows-based HDInsight</span></span>

<span data-ttu-id="fff8d-105">Hello řídicí panel Storm můžete tooeasily nasazení a spuštění clusteru HDInsight tooyour topologií Apache Storm pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="fff8d-105">hello Storm Dashboard allows you tooeasily deploy and run Apache Storm topologies tooyour HDInsight cluster by using your web browser.</span></span> <span data-ttu-id="fff8d-106">Můžete také použít toomonitor hello řídicí panel a správě spuštěných topologií.</span><span class="sxs-lookup"><span data-stu-id="fff8d-106">You can also use hello dashboard toomonitor and manage running topologies.</span></span> <span data-ttu-id="fff8d-107">Pokud používáte Visual Studio, hello nástroje HDInsight pro Visual Studio poskytují podobné funkce v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fff8d-107">If you use Visual Studio, hello HDInsight Tools for Visual Studio provide similar features in Visual Studio.</span></span>

<span data-ttu-id="fff8d-108">Hello řídicí panel Storm a hello Storm funkce hello nástroje HDInsight využívají hello Storm REST API, které lze použít toocreate vlastní řešení pro monitorování a správu.</span><span class="sxs-lookup"><span data-stu-id="fff8d-108">hello Storm Dashboard and hello Storm features in hello HDInsight Tools rely on hello Storm REST API, which can be used toocreate your own monitoring and management solutions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fff8d-109">Hello kroky v tomto dokumentu vyžadují Storm v clusteru HDInsight se systémem Windows jako hello operační systém.</span><span class="sxs-lookup"><span data-stu-id="fff8d-109">hello steps in this document require a Storm on HDInsight cluster that uses Windows as hello operating system.</span></span> <span data-ttu-id="fff8d-110">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="fff8d-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="fff8d-111">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="fff8d-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="fff8d-112">Informace o nasazení a správa topologií Storm pomocí clusteru služby HDInsight, který používá Linux najdete v tématu [nasazení a správa topologií Apache Storm v HDInsight se systémem Linux](hdinsight-storm-deploy-monitor-topology-linux.md)</span><span class="sxs-lookup"><span data-stu-id="fff8d-112">For information on deploying and managing Storm topologies with an HDInsight cluster that uses Linux, see [Deploy and manage Apache Storm topologies on Linux-based HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fff8d-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fff8d-113">Prerequisites</span></span>

* <span data-ttu-id="fff8d-114">**Apache Storm v HDInsight** -najdete v části [začít pracovat s Apache Storm v HDInsight](hdinsight-apache-storm-tutorial-get-started.md) pokyny týkající se vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="fff8d-114">**Apache Storm on HDInsight** - see [Get started with Apache Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started.md) for steps on creating a cluster.</span></span>

* <span data-ttu-id="fff8d-115">Pro hello **řídicí panel Storm**: moderní webový prohlížeč, který podporuje HTML5.</span><span class="sxs-lookup"><span data-stu-id="fff8d-115">For hello **Storm Dashboard**: A modern web browser that supports HTML5.</span></span>

* <span data-ttu-id="fff8d-116">Pro **Visual Studio** -Azure SDK 2.5.1 nebo novější a hello nástroje HDInsight pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fff8d-116">For **Visual Studio** - Azure SDK 2.5.1 or newer and hello HDInsight Tools for Visual Studio.</span></span> <span data-ttu-id="fff8d-117">V tématu [Začínáme pomocí nástrojů HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) tooinstall a nakonfigurujte hello nástroje HDInsight pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fff8d-117">See [Get started using HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) tooinstall and configure hello HDInsight tools for Visual Studio.</span></span>

    <span data-ttu-id="fff8d-118">Jedna z následujících verzí sady Visual Studio hello:</span><span class="sxs-lookup"><span data-stu-id="fff8d-118">One of hello following versions of Visual Studio:</span></span>

  * <span data-ttu-id="fff8d-119">Visual Studio 2012 s aktualizací 4</span><span class="sxs-lookup"><span data-stu-id="fff8d-119">Visual Studio 2012 with Update 4</span></span>

  * <span data-ttu-id="fff8d-120">Visual Studio 2013 s aktualizací 4 nebo Visual Studio 2013 Community</span><span class="sxs-lookup"><span data-stu-id="fff8d-120">Visual Studio 2013 with Update 4 or Visual Studio 2013 Community</span></span>

  * <span data-ttu-id="fff8d-121">Visual Studio 2015 (všechny edice)</span><span class="sxs-lookup"><span data-stu-id="fff8d-121">Visual Studio 2015 (any edition)</span></span>

  * <span data-ttu-id="fff8d-122">Visual Studio 2017 (všechny edice)</span><span class="sxs-lookup"><span data-stu-id="fff8d-122">Visual Studio 2017 (any edition)</span></span>

## <a name="storm-dashboard"></a><span data-ttu-id="fff8d-123">Řídicí panel Storm</span><span class="sxs-lookup"><span data-stu-id="fff8d-123">Storm Dashboard</span></span>

<span data-ttu-id="fff8d-124">Hello řídicí panel Storm je webová stránka, k dispozici v clusteru Storm.</span><span class="sxs-lookup"><span data-stu-id="fff8d-124">hello Storm Dashboard is a web page available on your Storm cluster.</span></span> <span data-ttu-id="fff8d-125">Adresa URL Hello je **https://&lt;clustername >.azurehdinsight.net/**, kde **clustername** je název hello Storm v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="fff8d-125">hello URL is **https://&lt;clustername>.azurehdinsight.net/**, where **clustername** is hello name of your Storm on HDInsight cluster.</span></span>

<span data-ttu-id="fff8d-126">Hello horní části hello řídicí panel Storm, vyberte **odeslání topologie**.</span><span class="sxs-lookup"><span data-stu-id="fff8d-126">From hello top of hello Storm Dashboard, select **Submit Topology**.</span></span> <span data-ttu-id="fff8d-127">Postupujte podle pokynů hello na toorun stránku hello ukázková topologie nebo tooupload a spusťte topologie, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="fff8d-127">Follow hello instructions on hello page toorun a sample topology or tooupload and run a topology that you created.</span></span>

![Hello odeslání stránky topologie][storm-dashboard-submit]

### <a name="storm-ui"></a><span data-ttu-id="fff8d-129">Storm uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="fff8d-129">Storm UI</span></span>

<span data-ttu-id="fff8d-130">Hello řídicí panel Storm, vyberte hello **uživatelské rozhraní Storm** odkaz.</span><span class="sxs-lookup"><span data-stu-id="fff8d-130">From hello Storm Dashboard, select hello **Storm UI** link.</span></span> <span data-ttu-id="fff8d-131">Zobrazí informace o clusteru hello v přidání tooany spuštěnými topologiemi.</span><span class="sxs-lookup"><span data-stu-id="fff8d-131">This displays information about hello cluster, in addition tooany running topologies.</span></span>

![uživatelské rozhraní storm Hello][storm-dashboard-ui]

> [!NOTE]
> <span data-ttu-id="fff8d-133">U některých verzí Internet Explorer může se stát, že hello uživatelské rozhraní Storm neaktualizuje po jste ji nejprve navštívili.</span><span class="sxs-lookup"><span data-stu-id="fff8d-133">With some versions of Internet Explorer, you may discover that hello Storm UI does not refresh after you have first visited it.</span></span> <span data-ttu-id="fff8d-134">Například se nemusí zobrazit nové topologie hello odeslání, nebo se může zobrazit topologii jako aktivní, když je dříve deaktivováno.</span><span class="sxs-lookup"><span data-stu-id="fff8d-134">For example, it may not show hello new topologies you submitted, or it may show a topology as active when you previously deactivated it.</span></span> <span data-ttu-id="fff8d-135">Společnost Microsoft si je vědoma tento problém a pracuje na řešení.</span><span class="sxs-lookup"><span data-stu-id="fff8d-135">Microsoft is aware of this issue and is working on a solution.</span></span>

#### <a name="main-page"></a><span data-ttu-id="fff8d-136">Hlavní stránka</span><span class="sxs-lookup"><span data-stu-id="fff8d-136">Main page</span></span>

<span data-ttu-id="fff8d-137">Hello hlavní stránce hello uživatelské rozhraní Storm poskytuje hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="fff8d-137">hello main page of hello Storm UI provides hello following information:</span></span>

* <span data-ttu-id="fff8d-138">**Souhrn clusteru**: základní informace o clusteru Storm hello.</span><span class="sxs-lookup"><span data-stu-id="fff8d-138">**Cluster summary**: Basic information about hello Storm cluster.</span></span>

* <span data-ttu-id="fff8d-139">**Souhrn topologie**: seznam spuštěných topologií.</span><span class="sxs-lookup"><span data-stu-id="fff8d-139">**Topology summary**: A list of running topologies.</span></span> <span data-ttu-id="fff8d-140">Hello odkazy v této části tooview použijte další informace o konkrétní topologie.</span><span class="sxs-lookup"><span data-stu-id="fff8d-140">Use hello links in this section tooview more information about specific topologies.</span></span>

* <span data-ttu-id="fff8d-141">**Souhrn nadřízeného**: informace o hello Storm nadřízeného.</span><span class="sxs-lookup"><span data-stu-id="fff8d-141">**Supervisor summary**: Information about hello Storm supervisor.</span></span>

* <span data-ttu-id="fff8d-142">**Konfigurace nimbus**: Nimbus konfiguraci pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="fff8d-142">**Nimbus configuration**: Nimbus configuration for hello cluster.</span></span>

#### <a name="topology-summary"></a><span data-ttu-id="fff8d-143">Souhrn topologie</span><span class="sxs-lookup"><span data-stu-id="fff8d-143">Topology summary</span></span>

<span data-ttu-id="fff8d-144">Výběr odkaz z hello **souhrn topologie** části zobrazí následující informace o topologii hello hello:</span><span class="sxs-lookup"><span data-stu-id="fff8d-144">Selecting a link from hello **Topology summary** section displays hello following information about hello topology:</span></span>

* <span data-ttu-id="fff8d-145">**Souhrn topologie**: základní informace o topologii hello.</span><span class="sxs-lookup"><span data-stu-id="fff8d-145">**Topology summary**: Basic information about hello topology.</span></span>

* <span data-ttu-id="fff8d-146">**Topologie akce**: akce správy, které můžete provést pro hello topologie.</span><span class="sxs-lookup"><span data-stu-id="fff8d-146">**Topology actions**: Management actions that you can perform for hello topology.</span></span>

  * <span data-ttu-id="fff8d-147">**Aktivovat**: obnoví zpracování deaktivované topologie.</span><span class="sxs-lookup"><span data-stu-id="fff8d-147">**Activate**: Resumes processing of a deactivated topology.</span></span>

  * <span data-ttu-id="fff8d-148">**Deaktivovat**: Pozastaví spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="fff8d-148">**Deactivate**: Pauses a running topology.</span></span>

  * <span data-ttu-id="fff8d-149">**Znovu vyvážit**: upraví paralelismus hello hello topologie.</span><span class="sxs-lookup"><span data-stu-id="fff8d-149">**Rebalance**: Adjusts hello parallelism of hello topology.</span></span> <span data-ttu-id="fff8d-150">Po změně hello počet uzlů v clusteru hello, znovu vyvážit spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="fff8d-150">You should rebalance running topologies after you have changed hello number of nodes in hello cluster.</span></span> <span data-ttu-id="fff8d-151">To umožňuje hello topologie tooadjust paralelismus toocompensate pro hello zvětšit nebo zmenšit počet uzlů v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="fff8d-151">This allows hello topology tooadjust parallelism toocompensate for hello increased or decreased number of nodes in hello cluster.</span></span>

      <span data-ttu-id="fff8d-152">Další informace najdete v tématu [pochopení paralelismu topologie Storm (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) hello](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span><span class="sxs-lookup"><span data-stu-id="fff8d-152">For more information, see [Understanding hello parallelism of a Storm topology (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span></span>

  * <span data-ttu-id="fff8d-153">**Příkaz kill**: ukončí topologii Storm po hello zadaný časový limit.</span><span class="sxs-lookup"><span data-stu-id="fff8d-153">**Kill**: Terminates a Storm topology after hello specified timeout.</span></span>

* <span data-ttu-id="fff8d-154">**Statistiky topologie**: Statistika hello topologie.</span><span class="sxs-lookup"><span data-stu-id="fff8d-154">**Topology stats**: Statistics about hello topology.</span></span> <span data-ttu-id="fff8d-155">Pomocí odkazů hello v hello **okno** sloupec tooset hello časový rámec hello zbývající položky na stránce hello.</span><span class="sxs-lookup"><span data-stu-id="fff8d-155">Use hello links in hello **Window** column tooset hello timeframe for hello remaining entries on hello page.</span></span>

* <span data-ttu-id="fff8d-156">**Spouts**: hello funkcích spouts používané hello topologie.</span><span class="sxs-lookup"><span data-stu-id="fff8d-156">**Spouts**: hello spouts used by hello topology.</span></span> <span data-ttu-id="fff8d-157">Použijte další informace o konkrétních funkcích spouts hello odkazy v této části tooview.</span><span class="sxs-lookup"><span data-stu-id="fff8d-157">Use hello links in this section tooview more information about specific spouts.</span></span>

* <span data-ttu-id="fff8d-158">**Bolts**: hello funkce bolts používané hello topologie.</span><span class="sxs-lookup"><span data-stu-id="fff8d-158">**Bolts**: hello bolts used by hello topology.</span></span> <span data-ttu-id="fff8d-159">Použijte hello odkazy v této části tooview Další informace o konkrétních funkcích bolts.</span><span class="sxs-lookup"><span data-stu-id="fff8d-159">Use hello links in this section tooview more information about specific bolts.</span></span>

* <span data-ttu-id="fff8d-160">**Topologie konfigurace**: hello konfiguraci topologie hello vybrané.</span><span class="sxs-lookup"><span data-stu-id="fff8d-160">**Topology configuration**: hello configuration of hello selected topology.</span></span>

#### <a name="spout-and-bolt-summary"></a><span data-ttu-id="fff8d-161">Souhrn funkcí Bolt a spout</span><span class="sxs-lookup"><span data-stu-id="fff8d-161">Spout and Bolt summary</span></span>

<span data-ttu-id="fff8d-162">Výběr spout z hello **Spouts** nebo **Bolts** části zobrazí následující informace o položce vybrané hello hello:</span><span class="sxs-lookup"><span data-stu-id="fff8d-162">Selecting a spout from hello **Spouts** or **Bolts** sections displays hello following information about hello selected item:</span></span>

* <span data-ttu-id="fff8d-163">**Souhrn součást**: základní informace o funkcích hello spout nebo bolt.</span><span class="sxs-lookup"><span data-stu-id="fff8d-163">**Component summary**: Basic information about hello spout or bolt.</span></span>

* <span data-ttu-id="fff8d-164">**Statistiky funkcí spout/Bolt**: Statistika hello spout nebo funkce bolt.</span><span class="sxs-lookup"><span data-stu-id="fff8d-164">**Spout/Bolt stats**: Statistics about hello spout or bolt.</span></span> <span data-ttu-id="fff8d-165">Pomocí odkazů hello v hello **okno** sloupec tooset hello časový rámec hello zbývající položky na stránce hello.</span><span class="sxs-lookup"><span data-stu-id="fff8d-165">Use hello links in hello **Window** column tooset hello timeframe for hello remaining entries on hello page.</span></span>

* <span data-ttu-id="fff8d-166">**Statistiky vstupu** (pouze funkce bolt): informace o hello vstupní datové proudy spotřebovávají hello bolt.</span><span class="sxs-lookup"><span data-stu-id="fff8d-166">**Input stats** (bolt only): Information about hello input streams consumed by hello bolt.</span></span>

* <span data-ttu-id="fff8d-167">**Statististiky výstupu**: informace o datových proudů hello vygenerované tímto objektem spout nebo funkce bolt.</span><span class="sxs-lookup"><span data-stu-id="fff8d-167">**Output stats**: Information about hello streams emitted by this spout or bolt.</span></span>

* <span data-ttu-id="fff8d-168">**Vykonavatelů**: informace o instancích hello hello spout nebo bolt.</span><span class="sxs-lookup"><span data-stu-id="fff8d-168">**Executors**: Information about hello instances of hello spout or bolt.</span></span> <span data-ttu-id="fff8d-169">Vyberte hello **Port** položka pro konkrétní vykonavatele tooview vytvořeného protokolu diagnostické informace pro tuto instanci.</span><span class="sxs-lookup"><span data-stu-id="fff8d-169">Select hello **Port** entry for a specific executor tooview a log of diagnostic information produced for this instance.</span></span>

* <span data-ttu-id="fff8d-170">**Chyby**: všechny informace o chybě pro tento spout nebo funkce bolt.</span><span class="sxs-lookup"><span data-stu-id="fff8d-170">**Errors**: Any error information for this spout or bolt.</span></span>

## <a name="hdinsight-tools-for-visual-studio"></a><span data-ttu-id="fff8d-171">Nástroje HDInsight pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fff8d-171">HDInsight Tools for Visual Studio</span></span>

<span data-ttu-id="fff8d-172">Nástroje HDInsight Hello lze použít toosubmit jazyka C# nebo hybridní topologie tooyour cluster Storm.</span><span class="sxs-lookup"><span data-stu-id="fff8d-172">hello HDInsight Tools can be used toosubmit C# or hybrid topologies tooyour Storm cluster.</span></span> <span data-ttu-id="fff8d-173">Hello následující kroky pomocí ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fff8d-173">hello following steps use a sample application.</span></span> <span data-ttu-id="fff8d-174">Informace o vytváření vlastního topologie pomocí nástrojů HDInsight hello najdete v tématu [vývoj topologií C# pomocí hello nástroje HDInsight pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="fff8d-174">For information about creating your own topologies by using hello HDInsight Tools, see [Develop C# topologies using hello HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

<span data-ttu-id="fff8d-175">Použijte následující postup toodeploy ukázka tooyour Storm v clusteru HDInsight hello pak zobrazovat a spravovat topologie hello.</span><span class="sxs-lookup"><span data-stu-id="fff8d-175">Use hello following steps toodeploy a sample tooyour Storm on HDInsight cluster, then view and manage hello topology.</span></span>

1. <span data-ttu-id="fff8d-176">Pokud jste ještě nenainstalovali hello nejnovější verzi hello nástroje HDInsight pro Visual Studio, najdete v části [Začínáme pomocí nástrojů HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="fff8d-176">If you have not already installed hello latest version of hello HDInsight Tools for Visual Studio, see [Get started using HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

2. <span data-ttu-id="fff8d-177">Otevřete Visual Studio, vyberte **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="fff8d-177">Open Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="fff8d-178">V hello **nový projekt** dialogové okno, rozbalte seznam **nainstalovaná** > **šablony**a potom vyberte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="fff8d-178">In hello **New Project** dialog box, expand **Installed** > **Templates**, and then select **HDInsight**.</span></span> <span data-ttu-id="fff8d-179">Hello seznam šablon, vyberte **Storm ukázka**.</span><span class="sxs-lookup"><span data-stu-id="fff8d-179">From hello list of templates, select **Storm Sample**.</span></span> <span data-ttu-id="fff8d-180">V dolní části hello hello dialogového okna zadejte název aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fff8d-180">At hello bottom of hello dialog box, type a name for hello application.</span></span>

    ![Bitové kopie](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

4. <span data-ttu-id="fff8d-182">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **odeslání tooStorm v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="fff8d-182">In **Solution Explorer**, right-click hello project, and select **Submit tooStorm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="fff8d-183">Pokud se zobrazí výzva, zadejte hello přihlašovací údaje pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="fff8d-183">If prompted, enter hello login credentials for your Azure subscription.</span></span> <span data-ttu-id="fff8d-184">Pokud máte více než jedno předplatné, přihlaste se toohello, která obsahuje váš cluster Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="fff8d-184">If you have more than one subscription, log in toohello one that contains your Storm on HDInsight cluster.</span></span>

5. <span data-ttu-id="fff8d-185">Vyberte váš cluster Storm v HDInsight z hello **Storm Cluster** rozevíracího seznamu a potom vyberte **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="fff8d-185">Select your Storm on HDInsight cluster from hello **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="fff8d-186">Můžete sledovat, jestli je pomocí hello úspěšné odeslání hello **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="fff8d-186">You can monitor whether hello submission is successful by using hello **Output** window.</span></span>

6. <span data-ttu-id="fff8d-187">Když hello topologie byl úspěšně odeslán, hello **topologie Storm** pro hello clusteru by se měla objevit.</span><span class="sxs-lookup"><span data-stu-id="fff8d-187">When hello topology has been successfully submitted, hello **Storm Topologies** for hello cluster should appear.</span></span> <span data-ttu-id="fff8d-188">Vyberte topologii hello hello seznamu tooview informace o hello spuštěná topologie.</span><span class="sxs-lookup"><span data-stu-id="fff8d-188">Select hello topology from hello list tooview information about hello running topology.</span></span>

    ![monitorování v sadě Visual studio](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]
   > <span data-ttu-id="fff8d-190">Můžete také zobrazit **topologie Storm** z **Průzkumníka serveru** rozšířením **Azure** > **HDInsight**a potom kliknete pravým tlačítkem cluster Storm v HDInsight a výběr **topologie Storm zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="fff8d-190">You can also view **Storm Topologies** from **Server Explorer** by expanding **Azure** > **HDInsight**, and then right-clicking a Storm on HDInsight cluster, and selecting **View Storm Topologies**.</span></span>

    <span data-ttu-id="fff8d-191">Vyberte hello tvar pro hello spouts nebo bolts tooview informace o těchto součástí.</span><span class="sxs-lookup"><span data-stu-id="fff8d-191">Select hello shape for hello spouts or bolts tooview information about these components.</span></span> <span data-ttu-id="fff8d-192">Otevře se nové okno pro každé vybrané položky.</span><span class="sxs-lookup"><span data-stu-id="fff8d-192">A new window opens for each item selected.</span></span>

   > [!NOTE]
   > <span data-ttu-id="fff8d-193">Název Hello hello topologie je název třídy hello topologie hello (v tomto případě `HelloWord`,) s časovým razítkem připojí.</span><span class="sxs-lookup"><span data-stu-id="fff8d-193">hello name of hello topology is hello class name of hello topology (in this case, `HelloWord`,) with a timestamp appended.</span></span>

7. <span data-ttu-id="fff8d-194">Z hello **souhrn topologie** zobrazit, vyberte možnost **Kill** toostop hello topologie.</span><span class="sxs-lookup"><span data-stu-id="fff8d-194">From hello **Topology Summary** view, select **Kill** toostop hello topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="fff8d-195">Topologie Storm pokračovat, spuštění, dokud jsou zastaveny nebo odstranění clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="fff8d-195">Storm topologies continue running until they are stopped or hello cluster is deleted.</span></span>


## <a name="rest-api"></a><span data-ttu-id="fff8d-196">REST API</span><span class="sxs-lookup"><span data-stu-id="fff8d-196">REST API</span></span>

<span data-ttu-id="fff8d-197">Hello uživatelské rozhraní Storm je postavený na hello rozhraní REST API, takže můžete provádět podobné správy a monitorování funkce pomocí hello REST API.</span><span class="sxs-lookup"><span data-stu-id="fff8d-197">hello Storm UI is built on top of hello REST API, so you can perform similar management and monitoring functionality by using hello REST API.</span></span> <span data-ttu-id="fff8d-198">Můžete vytvořit vlastní nástroje toocreate hello REST API pro správu a monitorování topologie Storm.</span><span class="sxs-lookup"><span data-stu-id="fff8d-198">You can use hello REST API toocreate custom tools for managing and monitoring Storm topologies.</span></span>

<span data-ttu-id="fff8d-199">Další informace najdete v tématu [Storm uživatelského rozhraní REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md).</span><span class="sxs-lookup"><span data-stu-id="fff8d-199">For more information, see [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md).</span></span> <span data-ttu-id="fff8d-200">Hello tyto informace o konkrétní toousing hello REST API s Apache Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="fff8d-200">hello following information is specific toousing hello REST API with Apache Storm on HDInsight.</span></span>

### <a name="base-uri"></a><span data-ttu-id="fff8d-201">Základní identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="fff8d-201">Base URI</span></span>

<span data-ttu-id="fff8d-202">Hello je základní identifikátor URI pro hello rozhraní API REST v clusterech HDInsight **https://&lt;clustername >.azurehdinsight.net/stormui/api/v1/**, kde **clustername** je název hello Storm na HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="fff8d-202">hello base URI for hello REST API on HDInsight clusters is **https://&lt;clustername>.azurehdinsight.net/stormui/api/v1/**, where **clustername** is hello name of your Storm on HDInsight cluster.</span></span>

### <a name="authentication"></a><span data-ttu-id="fff8d-203">Authentication</span><span class="sxs-lookup"><span data-stu-id="fff8d-203">Authentication</span></span>

<span data-ttu-id="fff8d-204">Toohello požadavky musí používat rozhraní REST API **základní ověřování**, takže můžete použít název správce clusteru HDInsight hello a heslo.</span><span class="sxs-lookup"><span data-stu-id="fff8d-204">Requests toohello REST API must use **basic authentication**, so you use hello HDInsight cluster administrator name and password.</span></span>

> [!NOTE]
> <span data-ttu-id="fff8d-205">Protože základní ověřování je odeslána pomocí nešifrovaného textu, měli byste **vždy** používat s clusterem s hello toosecure komunikaci prostřednictvím protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fff8d-205">Because basic authentication is sent by using clear text, you should **always** use HTTPS toosecure communications with hello cluster.</span></span>

### <a name="return-values"></a><span data-ttu-id="fff8d-206">Návratové hodnoty</span><span class="sxs-lookup"><span data-stu-id="fff8d-206">Return values</span></span>

<span data-ttu-id="fff8d-207">Informace, které se vrátí z hello REST API, může být pouze z použitelné v rámci clusteru hello nebo virtuální počítače na hello stejné virtuální síti Azure jako hello cluster.</span><span class="sxs-lookup"><span data-stu-id="fff8d-207">Information that is returned from hello REST API may only be usable from within hello cluster or virtual machines on hello same Azure Virtual Network as hello cluster.</span></span> <span data-ttu-id="fff8d-208">Například hello plně kvalifikovaný název domény (FQDN) pro servery Zookeeper vrátit nejsou být dostupný z Internetu hello.</span><span class="sxs-lookup"><span data-stu-id="fff8d-208">For example, hello fully qualified domain name (FQDN) returned for Zookeeper servers are not be accessible from hello Internet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fff8d-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fff8d-209">Next Steps</span></span>

<span data-ttu-id="fff8d-210">Teď, když jste se naučili, jak toodeploy a sledování topologie pomocí hello řídicí panel Storm, se naučíte, jak:</span><span class="sxs-lookup"><span data-stu-id="fff8d-210">Now that you've learned how toodeploy and monitor topologies by using hello Storm Dashboard, learn how to:</span></span>

* [<span data-ttu-id="fff8d-211">Vývoj topologie C# pomocí hello nástroje HDInsight pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fff8d-211">Develop C# topologies using hello HDInsight Tools for Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [<span data-ttu-id="fff8d-212">Vyvíjet topologie založené na jazyce Java pomocí nástroje Maven</span><span class="sxs-lookup"><span data-stu-id="fff8d-212">Develop Java-based topologies using Maven</span></span>](hdinsight-storm-develop-java-topology.md)

<span data-ttu-id="fff8d-213">Seznam Další příklad topologií najdete v tématu [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="fff8d-213">For a list of more example topologies, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png
