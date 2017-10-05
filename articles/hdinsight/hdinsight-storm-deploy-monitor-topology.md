---
title: "Nasazení a správa topologií Apache Storm v HDInsight | Microsoft Docs"
description: "Zjistěte, jak pro nasazení, monitorování a správa topologií Apache Storm pomocí řídicího panelu Storm v HDInsight. Pomocí nástroje Hadoop pro sadu Visual Studio."
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
ms.openlocfilehash: 34072574f83b51280e60e2f8766c6c5d5a33c307
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a><span data-ttu-id="3be15-104">Nasazení a správa topologií Apache Storm v HDInsight se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="3be15-104">Deploy and manage Apache Storm topologies on Windows-based HDInsight</span></span>

<span data-ttu-id="3be15-105">Řídicí panel Storm můžete snadno nasadit a spustit topologií Apache Storm ke svému clusteru HDInsight pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="3be15-105">The Storm Dashboard allows you to easily deploy and run Apache Storm topologies to your HDInsight cluster by using your web browser.</span></span> <span data-ttu-id="3be15-106">Řídicí panel můžete použít také ke sledování a správě spuštěných topologií.</span><span class="sxs-lookup"><span data-stu-id="3be15-106">You can also use the dashboard to monitor and manage running topologies.</span></span> <span data-ttu-id="3be15-107">Pokud používáte Visual Studio, nástroje HDInsight pro Visual Studio poskytují podobné funkce v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3be15-107">If you use Visual Studio, the HDInsight Tools for Visual Studio provide similar features in Visual Studio.</span></span>

<span data-ttu-id="3be15-108">Řídicí panel Storm a funkce Storm v HDInsight nástroje využívají Storm REST API, které lze použít k vytvoření vlastního monitorování a řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="3be15-108">The Storm Dashboard and the Storm features in the HDInsight Tools rely on the Storm REST API, which can be used to create your own monitoring and management solutions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3be15-109">Kroky v tomto dokumentu vyžadují Storm v clusteru HDInsight se systémem Windows jako operační systém.</span><span class="sxs-lookup"><span data-stu-id="3be15-109">The steps in this document require a Storm on HDInsight cluster that uses Windows as the operating system.</span></span> <span data-ttu-id="3be15-110">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="3be15-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3be15-111">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="3be15-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="3be15-112">Informace o nasazení a správa topologií Storm pomocí clusteru služby HDInsight, který používá Linux najdete v tématu [nasazení a správa topologií Apache Storm v HDInsight se systémem Linux](hdinsight-storm-deploy-monitor-topology-linux.md)</span><span class="sxs-lookup"><span data-stu-id="3be15-112">For information on deploying and managing Storm topologies with an HDInsight cluster that uses Linux, see [Deploy and manage Apache Storm topologies on Linux-based HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3be15-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3be15-113">Prerequisites</span></span>

* <span data-ttu-id="3be15-114">**Apache Storm v HDInsight** -najdete v části [začít pracovat s Apache Storm v HDInsight](hdinsight-apache-storm-tutorial-get-started.md) pokyny týkající se vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="3be15-114">**Apache Storm on HDInsight** - see [Get started with Apache Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started.md) for steps on creating a cluster.</span></span>

* <span data-ttu-id="3be15-115">Pro **řídicí panel Storm**: moderní webový prohlížeč, který podporuje HTML5.</span><span class="sxs-lookup"><span data-stu-id="3be15-115">For the **Storm Dashboard**: A modern web browser that supports HTML5.</span></span>

* <span data-ttu-id="3be15-116">Pro **Visual Studio** -Azure SDK 2.5.1 nebo novější a nástroje HDInsight pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3be15-116">For **Visual Studio** - Azure SDK 2.5.1 or newer and the HDInsight Tools for Visual Studio.</span></span> <span data-ttu-id="3be15-117">V tématu [Začínáme pomocí nástrojů HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) k instalaci a konfiguraci nástroje HDInsight pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3be15-117">See [Get started using HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) to install and configure the HDInsight tools for Visual Studio.</span></span>

    <span data-ttu-id="3be15-118">Jedna z následujících verzí sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="3be15-118">One of the following versions of Visual Studio:</span></span>

  * <span data-ttu-id="3be15-119">Visual Studio 2012 s aktualizací 4</span><span class="sxs-lookup"><span data-stu-id="3be15-119">Visual Studio 2012 with Update 4</span></span>

  * <span data-ttu-id="3be15-120">Visual Studio 2013 s aktualizací 4 nebo Visual Studio 2013 Community</span><span class="sxs-lookup"><span data-stu-id="3be15-120">Visual Studio 2013 with Update 4 or Visual Studio 2013 Community</span></span>

  * <span data-ttu-id="3be15-121">Visual Studio 2015 (všechny edice)</span><span class="sxs-lookup"><span data-stu-id="3be15-121">Visual Studio 2015 (any edition)</span></span>

  * <span data-ttu-id="3be15-122">Visual Studio 2017 (všechny edice)</span><span class="sxs-lookup"><span data-stu-id="3be15-122">Visual Studio 2017 (any edition)</span></span>

## <a name="storm-dashboard"></a><span data-ttu-id="3be15-123">Řídicí panel Storm</span><span class="sxs-lookup"><span data-stu-id="3be15-123">Storm Dashboard</span></span>

<span data-ttu-id="3be15-124">Řídicí panel Storm je webová stránka, k dispozici v clusteru Storm.</span><span class="sxs-lookup"><span data-stu-id="3be15-124">The Storm Dashboard is a web page available on your Storm cluster.</span></span> <span data-ttu-id="3be15-125">Adresa URL je **https://&lt;clustername >.azurehdinsight.net/**, kde **clustername** je název vaší cluster Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3be15-125">The URL is **https://&lt;clustername>.azurehdinsight.net/**, where **clustername** is the name of your Storm on HDInsight cluster.</span></span>

<span data-ttu-id="3be15-126">Z horní části řídicího panelu Storm, vyberte **odeslání topologie**.</span><span class="sxs-lookup"><span data-stu-id="3be15-126">From the top of the Storm Dashboard, select **Submit Topology**.</span></span> <span data-ttu-id="3be15-127">Postupujte podle pokynů na stránce ke spuštění ukázkové topologie nebo odeslání a spusťte topologie, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="3be15-127">Follow the instructions on the page to run a sample topology or to upload and run a topology that you created.</span></span>

![odeslání stránky topologie][storm-dashboard-submit]

### <a name="storm-ui"></a><span data-ttu-id="3be15-129">Storm uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="3be15-129">Storm UI</span></span>

<span data-ttu-id="3be15-130">Na řídicím panelu Storm, vyberte **uživatelské rozhraní Storm** odkaz.</span><span class="sxs-lookup"><span data-stu-id="3be15-130">From the Storm Dashboard, select the **Storm UI** link.</span></span> <span data-ttu-id="3be15-131">Zobrazí informace o clusteru, kromě všech spuštěných topologií.</span><span class="sxs-lookup"><span data-stu-id="3be15-131">This displays information about the cluster, in addition to any running topologies.</span></span>

![uživatelské rozhraní storm][storm-dashboard-ui]

> [!NOTE]
> <span data-ttu-id="3be15-133">U některých verzí aplikace Internet Explorer můžete zjistit, že uživatelské rozhraní Storm neaktualizuje po jste ji nejprve navštívili.</span><span class="sxs-lookup"><span data-stu-id="3be15-133">With some versions of Internet Explorer, you may discover that the Storm UI does not refresh after you have first visited it.</span></span> <span data-ttu-id="3be15-134">Například se nemusí zobrazit nové topologie odeslání, nebo se může zobrazit topologii jako aktivní, když je dříve deaktivováno.</span><span class="sxs-lookup"><span data-stu-id="3be15-134">For example, it may not show the new topologies you submitted, or it may show a topology as active when you previously deactivated it.</span></span> <span data-ttu-id="3be15-135">Společnost Microsoft si je vědoma tento problém a pracuje na řešení.</span><span class="sxs-lookup"><span data-stu-id="3be15-135">Microsoft is aware of this issue and is working on a solution.</span></span>

#### <a name="main-page"></a><span data-ttu-id="3be15-136">Hlavní stránka</span><span class="sxs-lookup"><span data-stu-id="3be15-136">Main page</span></span>

<span data-ttu-id="3be15-137">Hlavní stránka uživatelského rozhraní Storm poskytuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="3be15-137">The main page of the Storm UI provides the following information:</span></span>

* <span data-ttu-id="3be15-138">**Souhrn clusteru**: základní informace o clusteru Storm.</span><span class="sxs-lookup"><span data-stu-id="3be15-138">**Cluster summary**: Basic information about the Storm cluster.</span></span>

* <span data-ttu-id="3be15-139">**Souhrn topologie**: seznam spuštěných topologií.</span><span class="sxs-lookup"><span data-stu-id="3be15-139">**Topology summary**: A list of running topologies.</span></span> <span data-ttu-id="3be15-140">Chcete-li zobrazit další informace o konkrétní topologie pomocí odkazů v této části.</span><span class="sxs-lookup"><span data-stu-id="3be15-140">Use the links in this section to view more information about specific topologies.</span></span>

* <span data-ttu-id="3be15-141">**Souhrn nadřízeného**: informace o nadřízeného Storm.</span><span class="sxs-lookup"><span data-stu-id="3be15-141">**Supervisor summary**: Information about the Storm supervisor.</span></span>

* <span data-ttu-id="3be15-142">**Konfigurace nimbus**: Nimbus konfiguraci pro daný cluster.</span><span class="sxs-lookup"><span data-stu-id="3be15-142">**Nimbus configuration**: Nimbus configuration for the cluster.</span></span>

#### <a name="topology-summary"></a><span data-ttu-id="3be15-143">Souhrn topologie</span><span class="sxs-lookup"><span data-stu-id="3be15-143">Topology summary</span></span>

<span data-ttu-id="3be15-144">Výběr odkazu z **souhrn topologie** části zobrazí následující informace o topologii:</span><span class="sxs-lookup"><span data-stu-id="3be15-144">Selecting a link from the **Topology summary** section displays the following information about the topology:</span></span>

* <span data-ttu-id="3be15-145">**Souhrn topologie**: základní informace o topologii.</span><span class="sxs-lookup"><span data-stu-id="3be15-145">**Topology summary**: Basic information about the topology.</span></span>

* <span data-ttu-id="3be15-146">**Topologie akce**: akce správy, které můžete provést pro topologii.</span><span class="sxs-lookup"><span data-stu-id="3be15-146">**Topology actions**: Management actions that you can perform for the topology.</span></span>

  * <span data-ttu-id="3be15-147">**Aktivovat**: obnoví zpracování deaktivované topologie.</span><span class="sxs-lookup"><span data-stu-id="3be15-147">**Activate**: Resumes processing of a deactivated topology.</span></span>

  * <span data-ttu-id="3be15-148">**Deaktivovat**: Pozastaví spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="3be15-148">**Deactivate**: Pauses a running topology.</span></span>

  * <span data-ttu-id="3be15-149">**Znovu vyvážit**: upraví paralelismus topologii.</span><span class="sxs-lookup"><span data-stu-id="3be15-149">**Rebalance**: Adjusts the parallelism of the topology.</span></span> <span data-ttu-id="3be15-150">Po změně počtu uzlů v clusteru musíte znovu vyvážit spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="3be15-150">You should rebalance running topologies after you have changed the number of nodes in the cluster.</span></span> <span data-ttu-id="3be15-151">To umožňuje topologii upravovat paralelismus za účelem kompenzace pro vyšší nebo ke snížení počtu uzlů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="3be15-151">This allows the topology to adjust parallelism to compensate for the increased or decreased number of nodes in the cluster.</span></span>

      <span data-ttu-id="3be15-152">Další informace najdete v tématu [pochopení paralelismu topologie Storm (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span><span class="sxs-lookup"><span data-stu-id="3be15-152">For more information, see [Understanding the parallelism of a Storm topology (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span></span>

  * <span data-ttu-id="3be15-153">**Příkaz kill**: ukončí topologii Storm po zadaný časový limit.</span><span class="sxs-lookup"><span data-stu-id="3be15-153">**Kill**: Terminates a Storm topology after the specified timeout.</span></span>

* <span data-ttu-id="3be15-154">**Statistiky topologie**: Statistika týkající se topologie.</span><span class="sxs-lookup"><span data-stu-id="3be15-154">**Topology stats**: Statistics about the topology.</span></span> <span data-ttu-id="3be15-155">Použijte odkazy v **okno** sloupec nastavit časový rámec pro zbývající záznamy na stránce.</span><span class="sxs-lookup"><span data-stu-id="3be15-155">Use the links in the **Window** column to set the timeframe for the remaining entries on the page.</span></span>

* <span data-ttu-id="3be15-156">**Spouts**: funkcích spouts používané topologii.</span><span class="sxs-lookup"><span data-stu-id="3be15-156">**Spouts**: The spouts used by the topology.</span></span> <span data-ttu-id="3be15-157">Chcete-li zobrazit další informace o konkrétních funkcích spouts pomocí odkazů v této části.</span><span class="sxs-lookup"><span data-stu-id="3be15-157">Use the links in this section to view more information about specific spouts.</span></span>

* <span data-ttu-id="3be15-158">**Bolts**: funkce bolts používané topologii.</span><span class="sxs-lookup"><span data-stu-id="3be15-158">**Bolts**: The bolts used by the topology.</span></span> <span data-ttu-id="3be15-159">Použijte odkazy v této části zobrazíte další informace o konkrétních funkcích bolts.</span><span class="sxs-lookup"><span data-stu-id="3be15-159">Use the links in this section to view more information about specific bolts.</span></span>

* <span data-ttu-id="3be15-160">**Topologie konfigurace**: konfiguraci vybrané topologie.</span><span class="sxs-lookup"><span data-stu-id="3be15-160">**Topology configuration**: The configuration of the selected topology.</span></span>

#### <a name="spout-and-bolt-summary"></a><span data-ttu-id="3be15-161">Souhrn funkcí Bolt a spout</span><span class="sxs-lookup"><span data-stu-id="3be15-161">Spout and Bolt summary</span></span>

<span data-ttu-id="3be15-162">Výběr funkcí spout z **Spouts** nebo **Bolts** části zobrazí následující informace o vybrané položce:</span><span class="sxs-lookup"><span data-stu-id="3be15-162">Selecting a spout from the **Spouts** or **Bolts** sections displays the following information about the selected item:</span></span>

* <span data-ttu-id="3be15-163">**Souhrn součást**: základní informace o funkcích spout nebo bolt.</span><span class="sxs-lookup"><span data-stu-id="3be15-163">**Component summary**: Basic information about the spout or bolt.</span></span>

* <span data-ttu-id="3be15-164">**Statistiky funkcí spout/Bolt**: statistiky o funkcích spout nebo bolt.</span><span class="sxs-lookup"><span data-stu-id="3be15-164">**Spout/Bolt stats**: Statistics about the spout or bolt.</span></span> <span data-ttu-id="3be15-165">Použijte odkazy v **okno** sloupec nastavit časový rámec pro zbývající záznamy na stránce.</span><span class="sxs-lookup"><span data-stu-id="3be15-165">Use the links in the **Window** column to set the timeframe for the remaining entries on the page.</span></span>

* <span data-ttu-id="3be15-166">**Statistiky vstupu** (pouze funkce bolt): informace o vstupní datové proudy využívaná funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="3be15-166">**Input stats** (bolt only): Information about the input streams consumed by the bolt.</span></span>

* <span data-ttu-id="3be15-167">**Statististiky výstupu**: informace o datové proudy vygenerované tímto objektem spout nebo funkce bolt.</span><span class="sxs-lookup"><span data-stu-id="3be15-167">**Output stats**: Information about the streams emitted by this spout or bolt.</span></span>

* <span data-ttu-id="3be15-168">**Vykonavatelů**: informace o funkcích spout nebo bolt instance.</span><span class="sxs-lookup"><span data-stu-id="3be15-168">**Executors**: Information about the instances of the spout or bolt.</span></span> <span data-ttu-id="3be15-169">Vyberte **Port** vytváří záznam pro konkrétní vykonavatele chcete zobrazit protokol diagnostické informace pro tuto instanci.</span><span class="sxs-lookup"><span data-stu-id="3be15-169">Select the **Port** entry for a specific executor to view a log of diagnostic information produced for this instance.</span></span>

* <span data-ttu-id="3be15-170">**Chyby**: všechny informace o chybě pro tento spout nebo funkce bolt.</span><span class="sxs-lookup"><span data-stu-id="3be15-170">**Errors**: Any error information for this spout or bolt.</span></span>

## <a name="hdinsight-tools-for-visual-studio"></a><span data-ttu-id="3be15-171">Nástroje HDInsight pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3be15-171">HDInsight Tools for Visual Studio</span></span>

<span data-ttu-id="3be15-172">Nástroje HDInsight slouží k odeslání jazyka C# nebo hybridní topologie pro váš cluster Storm.</span><span class="sxs-lookup"><span data-stu-id="3be15-172">The HDInsight Tools can be used to submit C# or hybrid topologies to your Storm cluster.</span></span> <span data-ttu-id="3be15-173">Následující postup použijte ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3be15-173">The following steps use a sample application.</span></span> <span data-ttu-id="3be15-174">Informace o vytváření vlastního topologie pomocí nástrojů HDInsight naleznete v tématu [vývoj topologií C# pomocí nástrojů HDInsight pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="3be15-174">For information about creating your own topologies by using the HDInsight Tools, see [Develop C# topologies using the HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

<span data-ttu-id="3be15-175">Použijte následující kroky k nasazení ukázkového pro váš cluster Storm v HDInsight, pak zobrazení a Správa topologie.</span><span class="sxs-lookup"><span data-stu-id="3be15-175">Use the following steps to deploy a sample to your Storm on HDInsight cluster, then view and manage the topology.</span></span>

1. <span data-ttu-id="3be15-176">Pokud jste ještě nenainstalovali nejnovější verzi nástroje HDInsight pro Visual Studio, najdete v části [Začínáme pomocí nástrojů HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3be15-176">If you have not already installed the latest version of the HDInsight Tools for Visual Studio, see [Get started using HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

2. <span data-ttu-id="3be15-177">Otevřete Visual Studio, vyberte **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="3be15-177">Open Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="3be15-178">V **nový projekt** dialogové okno, rozbalte seznam **nainstalovaná** > **šablony**a potom vyberte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="3be15-178">In the **New Project** dialog box, expand **Installed** > **Templates**, and then select **HDInsight**.</span></span> <span data-ttu-id="3be15-179">V seznamu šablon vyberte **Storm ukázka**.</span><span class="sxs-lookup"><span data-stu-id="3be15-179">From the list of templates, select **Storm Sample**.</span></span> <span data-ttu-id="3be15-180">V dolní části dialogových oken zadejte název aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be15-180">At the bottom of the dialog box, type a name for the application.</span></span>

    ![Bitové kopie](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

4. <span data-ttu-id="3be15-182">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **odeslání do Storm v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="3be15-182">In **Solution Explorer**, right-click the project, and select **Submit to Storm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3be15-183">Pokud se zobrazí výzva, zadejte přihlašovací údaje pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="3be15-183">If prompted, enter the login credentials for your Azure subscription.</span></span> <span data-ttu-id="3be15-184">Pokud máte více než jedno předplatné, přihlaste se k ta, která obsahuje váš cluster Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3be15-184">If you have more than one subscription, log in to the one that contains your Storm on HDInsight cluster.</span></span>

5. <span data-ttu-id="3be15-185">Vyberte váš cluster Storm v HDInsight z **Storm Cluster** rozevíracího seznamu a potom vyberte **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="3be15-185">Select your Storm on HDInsight cluster from the **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="3be15-186">Můžete sledovat, jestli je úspěšně odesílání pomocí **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="3be15-186">You can monitor whether the submission is successful by using the **Output** window.</span></span>

6. <span data-ttu-id="3be15-187">Když topologii byl úspěšně odeslán, **topologie Storm** pro cluster by se měla objevit.</span><span class="sxs-lookup"><span data-stu-id="3be15-187">When the topology has been successfully submitted, the **Storm Topologies** for the cluster should appear.</span></span> <span data-ttu-id="3be15-188">Vyberte topologii ze seznamu a zobrazit informace o spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="3be15-188">Select the topology from the list to view information about the running topology.</span></span>

    ![monitorování v sadě Visual studio](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]
   > <span data-ttu-id="3be15-190">Můžete také zobrazit **topologie Storm** z **Průzkumníka serveru** rozšířením **Azure** > **HDInsight**a potom kliknete pravým tlačítkem cluster Storm v HDInsight a výběr **topologie Storm zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="3be15-190">You can also view **Storm Topologies** from **Server Explorer** by expanding **Azure** > **HDInsight**, and then right-clicking a Storm on HDInsight cluster, and selecting **View Storm Topologies**.</span></span>

    <span data-ttu-id="3be15-191">Vyberte tvar funkcích spouts nebo funkce bolts zobrazíte informace o těchto součástí.</span><span class="sxs-lookup"><span data-stu-id="3be15-191">Select the shape for the spouts or bolts to view information about these components.</span></span> <span data-ttu-id="3be15-192">Otevře se nové okno pro každé vybrané položky.</span><span class="sxs-lookup"><span data-stu-id="3be15-192">A new window opens for each item selected.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3be15-193">Název topologie je název třídy topologie (v tomto případě `HelloWord`,) s časovým razítkem připojí.</span><span class="sxs-lookup"><span data-stu-id="3be15-193">The name of the topology is the class name of the topology (in this case, `HelloWord`,) with a timestamp appended.</span></span>

7. <span data-ttu-id="3be15-194">Z **souhrn topologie** zobrazit, vyberte možnost **Kill** k zastavení topologie.</span><span class="sxs-lookup"><span data-stu-id="3be15-194">From the **Topology Summary** view, select **Kill** to stop the topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3be15-195">Topologie Storm pokračovat, spuštění, dokud jsou zastaveny nebo odstranění clusteru.</span><span class="sxs-lookup"><span data-stu-id="3be15-195">Storm topologies continue running until they are stopped or the cluster is deleted.</span></span>


## <a name="rest-api"></a><span data-ttu-id="3be15-196">REST API</span><span class="sxs-lookup"><span data-stu-id="3be15-196">REST API</span></span>

<span data-ttu-id="3be15-197">Uživatelské rozhraní Storm je postavený na rozhraní REST API, takže můžete provádět podobné správy a monitorování funkce pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="3be15-197">The Storm UI is built on top of the REST API, so you can perform similar management and monitoring functionality by using the REST API.</span></span> <span data-ttu-id="3be15-198">Rozhraní REST API můžete použít k vytvoření vlastních nástrojů pro správu a monitorování topologie Storm.</span><span class="sxs-lookup"><span data-stu-id="3be15-198">You can use the REST API to create custom tools for managing and monitoring Storm topologies.</span></span>

<span data-ttu-id="3be15-199">Další informace najdete v tématu [Storm uživatelského rozhraní REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md).</span><span class="sxs-lookup"><span data-stu-id="3be15-199">For more information, see [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md).</span></span> <span data-ttu-id="3be15-200">Tyto informace je specifická pro Apache Storm v HDInsight pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="3be15-200">The following information is specific to using the REST API with Apache Storm on HDInsight.</span></span>

### <a name="base-uri"></a><span data-ttu-id="3be15-201">Základní identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="3be15-201">Base URI</span></span>

<span data-ttu-id="3be15-202">Základní identifikátor URI pro rozhraní API REST v clusterech HDInsight **https://&lt;clustername >.azurehdinsight.net/stormui/api/v1/**, kde **clustername** je název vaší cluster Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3be15-202">The base URI for the REST API on HDInsight clusters is **https://&lt;clustername>.azurehdinsight.net/stormui/api/v1/**, where **clustername** is the name of your Storm on HDInsight cluster.</span></span>

### <a name="authentication"></a><span data-ttu-id="3be15-203">Authentication</span><span class="sxs-lookup"><span data-stu-id="3be15-203">Authentication</span></span>

<span data-ttu-id="3be15-204">Požadavky na rozhraní REST API musí používat **základní ověřování**, takže můžete použít název správce clusteru HDInsight a heslo.</span><span class="sxs-lookup"><span data-stu-id="3be15-204">Requests to the REST API must use **basic authentication**, so you use the HDInsight cluster administrator name and password.</span></span>

> [!NOTE]
> <span data-ttu-id="3be15-205">Protože základní ověřování je odeslána pomocí nešifrovaného textu, měli byste **vždy** používat protokol HTTPS pro zabezpečenou komunikaci s clusterem.</span><span class="sxs-lookup"><span data-stu-id="3be15-205">Because basic authentication is sent by using clear text, you should **always** use HTTPS to secure communications with the cluster.</span></span>

### <a name="return-values"></a><span data-ttu-id="3be15-206">Návratové hodnoty</span><span class="sxs-lookup"><span data-stu-id="3be15-206">Return values</span></span>

<span data-ttu-id="3be15-207">Informace, které se vrátí z rozhraní API REST může být pouze z použitelné v rámci clusteru nebo virtuální počítače ve stejné virtuální síti Azure jako cluster.</span><span class="sxs-lookup"><span data-stu-id="3be15-207">Information that is returned from the REST API may only be usable from within the cluster or virtual machines on the same Azure Virtual Network as the cluster.</span></span> <span data-ttu-id="3be15-208">Například plně kvalifikovaný název domény (FQDN) vrátil pro Zookeeper servery nejsou byly přístupné z Internetu.</span><span class="sxs-lookup"><span data-stu-id="3be15-208">For example, the fully qualified domain name (FQDN) returned for Zookeeper servers are not be accessible from the Internet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3be15-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3be15-209">Next Steps</span></span>

<span data-ttu-id="3be15-210">Teď, když jste se naučili jak nasadit a monitorovat topologie pomocí řídicího panelu Storm, přečtěte si, jak:</span><span class="sxs-lookup"><span data-stu-id="3be15-210">Now that you've learned how to deploy and monitor topologies by using the Storm Dashboard, learn how to:</span></span>

* [<span data-ttu-id="3be15-211">Vývoj topologie C# pomocí nástrojů HDInsight pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3be15-211">Develop C# topologies using the HDInsight Tools for Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [<span data-ttu-id="3be15-212">Vyvíjet topologie založené na jazyce Java pomocí nástroje Maven</span><span class="sxs-lookup"><span data-stu-id="3be15-212">Develop Java-based topologies using Maven</span></span>](hdinsight-storm-develop-java-topology.md)

<span data-ttu-id="3be15-213">Seznam Další příklad topologií najdete v tématu [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="3be15-213">For a list of more example topologies, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png
