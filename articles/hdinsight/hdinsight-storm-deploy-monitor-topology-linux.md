---
title: "aaaDeploy a správa topologií Apache Storm v HDInsight se systémem Linux | Microsoft Docs"
description: "Zjistěte, jak toodeploy, monitorování a správa topologií Apache Storm pomocí hello řídicí panel Storm v HDInsight se systémem Linux. Pomocí nástroje Hadoop pro sadu Visual Studio."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 35086e62-d6d8-4ccf-8cae-00073464a1e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: 3a1edb773089cc596fea423710aa88cf83c7b841
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-hdinsight"></a><span data-ttu-id="81f58-104">Nasazení a správa topologií Apache Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="81f58-104">Deploy and manage Apache Storm topologies on HDInsight</span></span>

<span data-ttu-id="81f58-105">V tomto dokumentu zjistěte hello základy správy a monitorování topologií Storm spuštěných na Storm v HDInsight clustery.</span><span class="sxs-lookup"><span data-stu-id="81f58-105">In this document, learn hello basics of managing and monitoring Storm topologies running on Storm on HDInsight clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="81f58-106">Hello kroky v tomto článku vyžadují Storm se systémem Linux v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="81f58-106">hello steps in this article require a Linux-based Storm on HDInsight cluster.</span></span> <span data-ttu-id="81f58-107">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="81f58-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="81f58-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="81f58-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> 
>
> <span data-ttu-id="81f58-109">Informace o nasazení a monitorování topologií v HDInsight se systémem Windows, najdete v části [nasazení a správa topologií Apache Storm v HDInsight se systémem Windows](hdinsight-storm-deploy-monitor-topology.md)</span><span class="sxs-lookup"><span data-stu-id="81f58-109">For information on deploying and monitoring topologies on Windows-based HDInsight, see [Deploy and manage Apache Storm topologies on Windows-based HDInsight](hdinsight-storm-deploy-monitor-topology.md)</span></span>


## <a name="prerequisites"></a><span data-ttu-id="81f58-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="81f58-110">Prerequisites</span></span>

* <span data-ttu-id="81f58-111">**Storm v clusteru HDInsight se systémem Linux**: najdete v části [začít pracovat s Apache Storm v HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) pokyny týkající se vytvoření clusteru</span><span class="sxs-lookup"><span data-stu-id="81f58-111">**A Linux-based Storm on HDInsight cluster**: see [Get started with Apache Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) for steps on creating a cluster</span></span>

* <span data-ttu-id="81f58-112">(Volitelné) **Znalost SSH a SCP**: Další informace najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="81f58-112">(Optional) **Familiarity with SSH and SCP**: For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="81f58-113">(Volitelné) **Visual Studio**: Azure SDK 2.5.1 nebo novější a hello nástrojů Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="81f58-113">(Optional) **Visual Studio**: Azure SDK 2.5.1 or newer and hello Data Lake Tools for Visual Studio.</span></span> <span data-ttu-id="81f58-114">Další informace najdete v tématu [Začínáme pomocí nástrojů Data Lake pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="81f58-114">For more information, see [Get started using Data Lake Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

    <span data-ttu-id="81f58-115">Jedna z následujících verzí sady Visual Studio hello:</span><span class="sxs-lookup"><span data-stu-id="81f58-115">One of hello following versions of Visual Studio:</span></span>

  * <span data-ttu-id="81f58-116">Visual Studio 2012 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="81f58-116">Visual Studio 2012 with [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>

  * <span data-ttu-id="81f58-117">Visual Studio 2013 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=44921) nebo [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span><span class="sxs-lookup"><span data-stu-id="81f58-117">Visual Studio 2013 with [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span></span>
  * [<span data-ttu-id="81f58-118">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="81f58-118">Visual Studio 2015</span></span>](https://www.visualstudio.com/downloads/)

  * <span data-ttu-id="81f58-119">Visual Studio 2015 (všechny edice)</span><span class="sxs-lookup"><span data-stu-id="81f58-119">Visual Studio 2015 (any edition)</span></span>

  * <span data-ttu-id="81f58-120">Visual Studio 2017 (všechny edice).</span><span class="sxs-lookup"><span data-stu-id="81f58-120">Visual Studio 2017 (any edition).</span></span> <span data-ttu-id="81f58-121">Nástroje data Lake pro Visual Studio 2017 se instalují jako součást hello pracovního vytížení Azure.</span><span class="sxs-lookup"><span data-stu-id="81f58-121">Data Lake Tools for Visual Studio 2017 are installed as part of hello Azure Workload.</span></span>

## <a name="submit-a-topology-visual-studio"></a><span data-ttu-id="81f58-122">Odeslání topologii: Visual Studio</span><span class="sxs-lookup"><span data-stu-id="81f58-122">Submit a topology: Visual Studio</span></span>

<span data-ttu-id="81f58-123">Nástroje HDInsight Hello lze použít toosubmit jazyka C# nebo hybridní topologie tooyour cluster Storm.</span><span class="sxs-lookup"><span data-stu-id="81f58-123">hello HDInsight Tools can be used toosubmit C# or hybrid topologies tooyour Storm cluster.</span></span> <span data-ttu-id="81f58-124">Hello následující kroky pomocí ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="81f58-124">hello following steps use a sample application.</span></span> <span data-ttu-id="81f58-125">Informace o vytváření vlastního topologie pomocí nástrojů HDInsight hello najdete v tématu [vývoj topologií C# pomocí hello nástroje HDInsight pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="81f58-125">For information about creating your own topologies by using hello HDInsight Tools, see [Develop C# topologies using hello HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

1. <span data-ttu-id="81f58-126">Pokud jste ještě nenainstalovali hello nejnovější verzi nástroje hello Data Lake pro Visual Studio, najdete v části [Začínáme pomocí nástrojů Data Lake pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="81f58-126">If you have not already installed hello latest version of hello Data Lake tools for Visual Studio, see [Get started using Data Lake Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="81f58-127">Hello nástrojů Data Lake pro Visual Studio se dřív označovaly jako hello nástroje HDInsight pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="81f58-127">hello Data Lake Tools for Visual Studio were formerly called hello HDInsight Tools for Visual Studio.</span></span>
    >
    > <span data-ttu-id="81f58-128">Nástroje data Lake pro Visual Studio jsou součástí hello __pracovního vytížení Azure__ pro Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="81f58-128">Data Lake Tools for Visual Studio are included in hello __Azure Workload__ for Visual Studio 2017.</span></span>

2. <span data-ttu-id="81f58-129">Otevřete Visual Studio, vyberte **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="81f58-129">Open Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="81f58-130">V hello **nový projekt** dialogové okno, rozbalte seznam **nainstalovaná** > **šablony**a potom vyberte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="81f58-130">In hello **New Project** dialog box, expand **Installed** > **Templates**, and then select **HDInsight**.</span></span> <span data-ttu-id="81f58-131">Hello seznam šablon, vyberte **Storm ukázka**.</span><span class="sxs-lookup"><span data-stu-id="81f58-131">From hello list of templates, select **Storm Sample**.</span></span> <span data-ttu-id="81f58-132">V dolní části hello hello dialogového okna zadejte název aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="81f58-132">At hello bottom of hello dialog box, type a name for hello application.</span></span>

    ![Bitové kopie](./media/hdinsight-storm-deploy-monitor-topology-linux/sample.png)

4. <span data-ttu-id="81f58-134">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **odeslání tooStorm v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="81f58-134">In **Solution Explorer**, right-click hello project, and select **Submit tooStorm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="81f58-135">Pokud se zobrazí výzva, zadejte hello přihlašovací údaje pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="81f58-135">If prompted, enter hello login credentials for your Azure subscription.</span></span> <span data-ttu-id="81f58-136">Pokud máte více než jedno předplatné, přihlaste se toohello, která obsahuje váš cluster Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="81f58-136">If you have more than one subscription, log in toohello one that contains your Storm on HDInsight cluster.</span></span>

5. <span data-ttu-id="81f58-137">Vyberte váš cluster Storm v HDInsight z hello **Storm Cluster** rozevíracího seznamu a potom vyberte **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="81f58-137">Select your Storm on HDInsight cluster from hello **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="81f58-138">Můžete sledovat, jestli je pomocí hello úspěšné odeslání hello **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="81f58-138">You can monitor whether hello submission is successful by using hello **Output** window.</span></span>

## <a name="submit-a-topology-ssh-and-hello-storm-command"></a><span data-ttu-id="81f58-139">Odeslání topologii: SSH a hello Storm příkaz</span><span class="sxs-lookup"><span data-stu-id="81f58-139">Submit a topology: SSH and hello Storm command</span></span>

1. <span data-ttu-id="81f58-140">Použití clusteru HDInsight toohello tooconnect SSH.</span><span class="sxs-lookup"><span data-stu-id="81f58-140">Use SSH tooconnect toohello HDInsight cluster.</span></span> <span data-ttu-id="81f58-141">Nahraďte **uživatelské jméno** hello název vaší přihlašování přes SSH.</span><span class="sxs-lookup"><span data-stu-id="81f58-141">Replace **USERNAME** hello name of your SSH login.</span></span> <span data-ttu-id="81f58-142">Nahraďte **CLUSTERNAME** názvem clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="81f58-142">Replace **CLUSTERNAME** with your HDInsight cluster name:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    <span data-ttu-id="81f58-143">Další informace o používání SSH tooconnect tooyour HDInsight clusteru najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="81f58-143">For more information on using SSH tooconnect tooyour HDInsight cluster, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="81f58-144">Použijte následující příkaz toostart ukázkové topologie hello:</span><span class="sxs-lookup"><span data-stu-id="81f58-144">Use hello following command toostart an example topology:</span></span>

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology WordCount

    <span data-ttu-id="81f58-145">Tento příkaz spustí hello ukázkovou topologii WordCount v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="81f58-145">This command starts hello example WordCount topology on hello cluster.</span></span> <span data-ttu-id="81f58-146">Tato topologie náhodně generovat věty a počet hello výskytem jednotlivých slov v hello věty.</span><span class="sxs-lookup"><span data-stu-id="81f58-146">This topology randomly generate sentences and count hello occurrence of each word in hello sentences.</span></span>

   > [!NOTE]
   > <span data-ttu-id="81f58-147">Při odesílání topologie toohello clusteru, je nutné nejprve zkopírovat soubor jar hello obsahující hello cluster před použitím hello `storm` příkaz.</span><span class="sxs-lookup"><span data-stu-id="81f58-147">When submitting topology toohello cluster, you must first copy hello jar file containing hello cluster before using hello `storm` command.</span></span> <span data-ttu-id="81f58-148">toocopy hello souboru toohello clusteru, můžete použít hello `scp` příkaz.</span><span class="sxs-lookup"><span data-stu-id="81f58-148">toocopy hello file toohello cluster, you can use hello `scp` command.</span></span> <span data-ttu-id="81f58-149">Například `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`.</span><span class="sxs-lookup"><span data-stu-id="81f58-149">For example, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span></span>
   >
   > <span data-ttu-id="81f58-150">Příklad WordCount Hello a další příklady starter storm jsou již zahrnuty v clusteru na `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span><span class="sxs-lookup"><span data-stu-id="81f58-150">hello WordCount example, and other storm starter examples, are already included on your cluster at `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span></span>

## <a name="submit-a-topology-programmatically"></a><span data-ttu-id="81f58-151">Odeslání topologii: prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="81f58-151">Submit a topology: programmatically</span></span>

<span data-ttu-id="81f58-152">Topologie tooStorm v HDInsight programově nasadíte komunikaci s hello Nimbus služby hostované v clusteru.</span><span class="sxs-lookup"><span data-stu-id="81f58-152">You can programmatically deploy a topology tooStorm on HDInsight by communicating with hello Nimbus service hosted in your cluster.</span></span> <span data-ttu-id="81f58-153">[https://github.com/Azure-Samples/hdinsight-Java-Deploy-Storm-Topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) poskytuje příklad aplikaci Java, která ukazuje, jak toodeploy a spusťte topologii prostřednictvím hello Nimbus služby.</span><span class="sxs-lookup"><span data-stu-id="81f58-153">[https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) provides an example Java application that demonstrates how toodeploy and start a topology through hello Nimbus service.</span></span>

## <a name="monitor-and-manage-visual-studio"></a><span data-ttu-id="81f58-154">Sledování a správě: Visual Studio</span><span class="sxs-lookup"><span data-stu-id="81f58-154">Monitor and manage: Visual Studio</span></span>

<span data-ttu-id="81f58-155">Když topologii byla úspěšně odeslána pomocí sady Visual Studio, hello **topologie Storm** hello clusteru se zobrazí v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="81f58-155">When a topology has been successfully submitted using Visual Studio, hello **Storm Topologies** view for hello cluster appears.</span></span> <span data-ttu-id="81f58-156">Vyberte topologii hello hello seznamu tooview informace o hello spuštěná topologie.</span><span class="sxs-lookup"><span data-stu-id="81f58-156">Select hello topology from hello list tooview information about hello running topology.</span></span>

![monitorování v sadě Visual studio](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

> [!NOTE]
> <span data-ttu-id="81f58-158">Můžete také zobrazit **topologie Storm** z **Průzkumníka serveru** rozšířením **Azure** > **HDInsight**a potom kliknete pravým tlačítkem cluster Storm v HDInsight a výběr **topologie Storm zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="81f58-158">You can also view **Storm Topologies** from **Server Explorer** by expanding **Azure** > **HDInsight**, and then right-clicking a Storm on HDInsight cluster, and selecting **View Storm Topologies**.</span></span>

<span data-ttu-id="81f58-159">Vyberte hello tvar pro hello spouts nebo bolts tooview informace o těchto součástí.</span><span class="sxs-lookup"><span data-stu-id="81f58-159">Select hello shape for hello spouts or bolts tooview information about these components.</span></span> <span data-ttu-id="81f58-160">Otevře se nové okno pro každé vybrané položky.</span><span class="sxs-lookup"><span data-stu-id="81f58-160">A new window opens for each item selected.</span></span>

### <a name="deactivate-and-reactivate"></a><span data-ttu-id="81f58-161">Deaktivovat a znovu aktivovat</span><span class="sxs-lookup"><span data-stu-id="81f58-161">Deactivate and reactivate</span></span>

<span data-ttu-id="81f58-162">Deaktivace topologii pozastaví ho, dokud nebude ukončen nebo znovu aktivovat.</span><span class="sxs-lookup"><span data-stu-id="81f58-162">Deactivating a topology pauses it until it is killed or reactivated.</span></span> <span data-ttu-id="81f58-163">tooperform těchto operací použít hello __deaktivovat__ a __znovu aktivovat__ tlačítka hello horní části hello __souhrn topologie__.</span><span class="sxs-lookup"><span data-stu-id="81f58-163">tooperform these operations, use hello __Deactivate__ and __Reactivate__ buttons at hello top of hello __Topology Summary__.</span></span>

### <a name="rebalance"></a><span data-ttu-id="81f58-164">Znovu vyvážit</span><span class="sxs-lookup"><span data-stu-id="81f58-164">Rebalance</span></span>

<span data-ttu-id="81f58-165">Vyrovnává topologii umožňuje hello systému toorevise hello paralelismu topologie hello.</span><span class="sxs-lookup"><span data-stu-id="81f58-165">Rebalancing a topology allows hello system toorevise hello parallelism of hello topology.</span></span> <span data-ttu-id="81f58-166">Například pokud jste změnili hello clusteru tooadd další poznámky, vyrovnává umožňuje topologii toosee hello nové uzly.</span><span class="sxs-lookup"><span data-stu-id="81f58-166">For example, if you have resized hello cluster tooadd more notes, rebalancing allows a topology toosee hello new nodes.</span></span>

<span data-ttu-id="81f58-167">toorebalance topologii, použijte hello __znovu vyvážit__ tlačítko hello horní části hello __souhrn topologie__.</span><span class="sxs-lookup"><span data-stu-id="81f58-167">toorebalance a topology, use hello __Rebalance__ button at hello top of hello __Topology Summary__.</span></span>

> [!WARNING]
> <span data-ttu-id="81f58-168">Vyrovnává topologii nejprve deaktivuje hello topologie, pak znovu distribuuje pracovníci rovnoměrně mezi hello clusteru a potom nakonec vrátí hello topologie toohello stavu, ve kterém se nacházel před vyrovnává došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="81f58-168">Rebalancing a topology first deactivates hello topology, then redistributes workers evenly across hello cluster, then finally returns hello topology toohello state it was in before rebalancing occurred.</span></span> <span data-ttu-id="81f58-169">Takže pokud hello topologie byl aktivní, se zase aktivní.</span><span class="sxs-lookup"><span data-stu-id="81f58-169">So if hello topology was active, it becomes active again.</span></span> <span data-ttu-id="81f58-170">Pokud se ji deaktivovala, zůstane deaktivované.</span><span class="sxs-lookup"><span data-stu-id="81f58-170">If it was deactivated, it remains deactivated.</span></span>

### <a name="kill-a-topology"></a><span data-ttu-id="81f58-171">Příkaz kill topologii</span><span class="sxs-lookup"><span data-stu-id="81f58-171">Kill a topology</span></span>

<span data-ttu-id="81f58-172">Topologie Storm pokračovat, spuštění, dokud jsou zastaveny nebo odstranění clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="81f58-172">Storm topologies continue running until they are stopped or hello cluster is deleted.</span></span> <span data-ttu-id="81f58-173">toostop topologii, použijte hello __Kill__ tlačítko hello horní části hello __souhrn topologie__.</span><span class="sxs-lookup"><span data-stu-id="81f58-173">toostop a topology, use hello __Kill__ button at hello top of hello __Topology Summary__.</span></span>

## <a name="monitor-and-manage-ssh-and-hello-storm-command"></a><span data-ttu-id="81f58-174">Sledování a správě: SSH a hello Storm příkaz</span><span class="sxs-lookup"><span data-stu-id="81f58-174">Monitor and manage: SSH and hello Storm command</span></span>

<span data-ttu-id="81f58-175">Hello `storm` nástroje vám umožní toowork se spuštěnými topologiemi z příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="81f58-175">hello `storm` utility allows you toowork with running topologies from hello command line.</span></span> <span data-ttu-id="81f58-176">Použití `storm -h` pro úplný seznam příkazů.</span><span class="sxs-lookup"><span data-stu-id="81f58-176">Use `storm -h` for a full list of commands.</span></span>

### <a name="list-topologies"></a><span data-ttu-id="81f58-177">Seznam topologie</span><span class="sxs-lookup"><span data-stu-id="81f58-177">List topologies</span></span>

<span data-ttu-id="81f58-178">Použijte následující příkaz toolist hello všechny spuštěné topologie:</span><span class="sxs-lookup"><span data-stu-id="81f58-178">Use hello following command toolist all running topologies:</span></span>

    storm list

<span data-ttu-id="81f58-179">Tento příkaz vrátí informace podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="81f58-179">This command returns information similar toohello following text:</span></span>

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a><span data-ttu-id="81f58-180">Deaktivovat a znovu aktivovat</span><span class="sxs-lookup"><span data-stu-id="81f58-180">Deactivate and reactivate</span></span>

<span data-ttu-id="81f58-181">Deaktivace topologii pozastaví ho, dokud nebude ukončen nebo znovu aktivovat.</span><span class="sxs-lookup"><span data-stu-id="81f58-181">Deactivating a topology pauses it until it is killed or reactivated.</span></span> <span data-ttu-id="81f58-182">Použijte následující příkaz toodeactivate hello a opětovná aktivace:</span><span class="sxs-lookup"><span data-stu-id="81f58-182">Use hello following command toodeactivate and reactivate:</span></span>

    storm Deactivate TOPOLOGYNAME

    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a><span data-ttu-id="81f58-183">Příkaz kill spuštěné topologie</span><span class="sxs-lookup"><span data-stu-id="81f58-183">Kill a running topology</span></span>

<span data-ttu-id="81f58-184">Storm topologie, jednou spustit, pokračovat spuštění, dokud se zastavila.</span><span class="sxs-lookup"><span data-stu-id="81f58-184">Storm topologies, once started, continue running until stopped.</span></span> <span data-ttu-id="81f58-185">toostop topologii, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="81f58-185">toostop a topology, use hello following command:</span></span>

    storm kill TOPOLOGYNAME

### <a name="rebalance"></a><span data-ttu-id="81f58-186">Znovu vyvážit</span><span class="sxs-lookup"><span data-stu-id="81f58-186">Rebalance</span></span>

<span data-ttu-id="81f58-187">Vyrovnává topologii umožňuje hello systému toorevise hello paralelismu topologie hello.</span><span class="sxs-lookup"><span data-stu-id="81f58-187">Rebalancing a topology allows hello system toorevise hello parallelism of hello topology.</span></span> <span data-ttu-id="81f58-188">Například pokud jste změnili hello clusteru tooadd další poznámky, vyrovnává umožňuje topologii toosee hello nové uzly.</span><span class="sxs-lookup"><span data-stu-id="81f58-188">For example, if you have resized hello cluster tooadd more notes, rebalancing allows a topology toosee hello new nodes.</span></span>

> [!WARNING]
> <span data-ttu-id="81f58-189">Vyrovnává topologii nejprve deaktivuje hello topologie, pak znovu distribuuje pracovníci rovnoměrně mezi hello clusteru a potom nakonec vrátí hello topologie toohello stavu, ve kterém se nacházel před vyrovnává došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="81f58-189">Rebalancing a topology first deactivates hello topology, then redistributes workers evenly across hello cluster, then finally returns hello topology toohello state it was in before rebalancing occurred.</span></span> <span data-ttu-id="81f58-190">Takže pokud hello topologie byl aktivní, se zase aktivní.</span><span class="sxs-lookup"><span data-stu-id="81f58-190">So if hello topology was active, it becomes active again.</span></span> <span data-ttu-id="81f58-191">Pokud se ji deaktivovala, zůstane deaktivované.</span><span class="sxs-lookup"><span data-stu-id="81f58-191">If it was deactivated, it remains deactivated.</span></span>

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-storm-ui"></a><span data-ttu-id="81f58-192">Sledování a správě: Storm uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="81f58-192">Monitor and manage: Storm UI</span></span>

<span data-ttu-id="81f58-193">Hello uživatelské rozhraní Storm poskytuje webové rozhraní pro práci se spuštěnými topologiemi a je součástí clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="81f58-193">hello Storm UI provides a web interface for working with running topologies, and is included on your HDInsight cluster.</span></span> <span data-ttu-id="81f58-194">hello tooview uživatelské rozhraní Storm, použití webové prohlížeče tooopen **https://CLUSTERNAME.azurehdinsight.net/stormui**, kde **CLUSTERNAME** je hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="81f58-194">tooview hello Storm UI, use a web browser tooopen **https://CLUSTERNAME.azurehdinsight.net/stormui**, where **CLUSTERNAME** is hello name of your cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="81f58-195">Pokud se zobrazí dotaz tooprovide uživatelské jméno a heslo, zadejte Správce clusteru hello (správce) a heslo použité při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="81f58-195">If asked tooprovide a user name and password, enter hello cluster administrator (admin) and password that you used when creating hello cluster.</span></span>

### <a name="main-page"></a><span data-ttu-id="81f58-196">Hlavní stránka</span><span class="sxs-lookup"><span data-stu-id="81f58-196">Main page</span></span>

<span data-ttu-id="81f58-197">Hello hlavní stránce hello uživatelské rozhraní Storm poskytuje hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="81f58-197">hello main page of hello Storm UI provides hello following information:</span></span>

* <span data-ttu-id="81f58-198">**Souhrn clusteru**: základní informace o clusteru Storm hello.</span><span class="sxs-lookup"><span data-stu-id="81f58-198">**Cluster summary**: Basic information about hello Storm cluster.</span></span>
* <span data-ttu-id="81f58-199">**Souhrn topologie**: seznam spuštěných topologií.</span><span class="sxs-lookup"><span data-stu-id="81f58-199">**Topology summary**: A list of running topologies.</span></span> <span data-ttu-id="81f58-200">Hello odkazy v této části tooview použijte další informace o konkrétní topologie.</span><span class="sxs-lookup"><span data-stu-id="81f58-200">Use hello links in this section tooview more information about specific topologies.</span></span>
* <span data-ttu-id="81f58-201">**Souhrn nadřízeného**: informace o hello Storm nadřízeného.</span><span class="sxs-lookup"><span data-stu-id="81f58-201">**Supervisor summary**: Information about hello Storm supervisor.</span></span>
* <span data-ttu-id="81f58-202">**Konfigurace nimbus**: Nimbus konfiguraci pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="81f58-202">**Nimbus configuration**: Nimbus configuration for hello cluster.</span></span>

### <a name="topology-summary"></a><span data-ttu-id="81f58-203">Souhrn topologie</span><span class="sxs-lookup"><span data-stu-id="81f58-203">Topology summary</span></span>

<span data-ttu-id="81f58-204">Výběr odkaz z hello **souhrn topologie** části zobrazí následující informace o topologii hello hello:</span><span class="sxs-lookup"><span data-stu-id="81f58-204">Selecting a link from hello **Topology summary** section displays hello following information about hello topology:</span></span>

* <span data-ttu-id="81f58-205">**Souhrn topologie**: základní informace o topologii hello.</span><span class="sxs-lookup"><span data-stu-id="81f58-205">**Topology summary**: Basic information about hello topology.</span></span>
* <span data-ttu-id="81f58-206">**Topologie akce**: akce správy, které můžete provést pro hello topologie.</span><span class="sxs-lookup"><span data-stu-id="81f58-206">**Topology actions**: Management actions that you can perform for hello topology.</span></span>

  * <span data-ttu-id="81f58-207">**Aktivovat**: obnoví zpracování deaktivované topologie.</span><span class="sxs-lookup"><span data-stu-id="81f58-207">**Activate**: Resumes processing of a deactivated topology.</span></span>
  * <span data-ttu-id="81f58-208">**Deaktivovat**: Pozastaví spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="81f58-208">**Deactivate**: Pauses a running topology.</span></span>
  * <span data-ttu-id="81f58-209">**Znovu vyvážit**: upraví paralelismus hello hello topologie.</span><span class="sxs-lookup"><span data-stu-id="81f58-209">**Rebalance**: Adjusts hello parallelism of hello topology.</span></span> <span data-ttu-id="81f58-210">Po změně hello počet uzlů v clusteru hello, znovu vyvážit spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="81f58-210">You should rebalance running topologies after you have changed hello number of nodes in hello cluster.</span></span> <span data-ttu-id="81f58-211">Tato operace povoluje hello topologie tooadjust paralelismus toocompensate pro hello zvětšit nebo zmenšit počet uzlů v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="81f58-211">This operation allows hello topology tooadjust parallelism toocompensate for hello increased or decreased number of nodes in hello cluster.</span></span>

    <span data-ttu-id="81f58-212">Další informace najdete v tématu <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">pochopení paralelismu topologie Storm hello</a>.</span><span class="sxs-lookup"><span data-stu-id="81f58-212">For more information, see <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Understanding hello parallelism of a Storm topology</a>.</span></span>
  * <span data-ttu-id="81f58-213">**Příkaz kill**: ukončí topologii Storm po hello zadaný časový limit.</span><span class="sxs-lookup"><span data-stu-id="81f58-213">**Kill**: Terminates a Storm topology after hello specified timeout.</span></span>
* <span data-ttu-id="81f58-214">**Statistiky topologie**: Statistika hello topologie.</span><span class="sxs-lookup"><span data-stu-id="81f58-214">**Topology stats**: Statistics about hello topology.</span></span> <span data-ttu-id="81f58-215">tooset hello časový rámec hello zbývající položky na stránce hello, použijte odkazy hello hello **okno** sloupce.</span><span class="sxs-lookup"><span data-stu-id="81f58-215">tooset hello timeframe for hello remaining entries on hello page, use hello links in hello **Window** column.</span></span>
* <span data-ttu-id="81f58-216">**Spouts**: hello funkcích spouts používané hello topologie.</span><span class="sxs-lookup"><span data-stu-id="81f58-216">**Spouts**: hello spouts used by hello topology.</span></span> <span data-ttu-id="81f58-217">Použijte další informace o konkrétních funkcích spouts hello odkazy v této části tooview.</span><span class="sxs-lookup"><span data-stu-id="81f58-217">Use hello links in this section tooview more information about specific spouts.</span></span>
* <span data-ttu-id="81f58-218">**Bolts**: hello funkce bolts používané hello topologie.</span><span class="sxs-lookup"><span data-stu-id="81f58-218">**Bolts**: hello bolts used by hello topology.</span></span> <span data-ttu-id="81f58-219">Použijte hello odkazy v této části tooview Další informace o konkrétních funkcích bolts.</span><span class="sxs-lookup"><span data-stu-id="81f58-219">Use hello links in this section tooview more information about specific bolts.</span></span>
* <span data-ttu-id="81f58-220">**Topologie konfigurace**: hello konfiguraci topologie hello vybrané.</span><span class="sxs-lookup"><span data-stu-id="81f58-220">**Topology configuration**: hello configuration of hello selected topology.</span></span>

### <a name="spout-and-bolt-summary"></a><span data-ttu-id="81f58-221">Souhrn funkcí Bolt a spout</span><span class="sxs-lookup"><span data-stu-id="81f58-221">Spout and Bolt summary</span></span>

<span data-ttu-id="81f58-222">Výběr spout z hello **Spouts** nebo **Bolts** části zobrazí následující informace o položce vybrané hello hello:</span><span class="sxs-lookup"><span data-stu-id="81f58-222">Selecting a spout from hello **Spouts** or **Bolts** sections displays hello following information about hello selected item:</span></span>

* <span data-ttu-id="81f58-223">**Souhrn součást**: základní informace o funkcích hello spout nebo bolt.</span><span class="sxs-lookup"><span data-stu-id="81f58-223">**Component summary**: Basic information about hello spout or bolt.</span></span>
* <span data-ttu-id="81f58-224">**Statistiky funkcí spout/Bolt**: Statistika hello spout nebo funkce bolt.</span><span class="sxs-lookup"><span data-stu-id="81f58-224">**Spout/Bolt stats**: Statistics about hello spout or bolt.</span></span> <span data-ttu-id="81f58-225">tooset hello časový rámec hello zbývající položky na stránce hello, použijte odkazy hello hello **okno** sloupce.</span><span class="sxs-lookup"><span data-stu-id="81f58-225">tooset hello timeframe for hello remaining entries on hello page, use hello links in hello **Window** column.</span></span>
* <span data-ttu-id="81f58-226">**Statistiky vstupu** (pouze funkce bolt): informace o hello vstupní datové proudy spotřebovávají hello bolt.</span><span class="sxs-lookup"><span data-stu-id="81f58-226">**Input stats** (bolt only): Information about hello input streams consumed by hello bolt.</span></span>
* <span data-ttu-id="81f58-227">**Statististiky výstupu**: informace o datových proudů hello vygenerované tímto objektem spout nebo funkce bolt.</span><span class="sxs-lookup"><span data-stu-id="81f58-227">**Output stats**: Information about hello streams emitted by this spout or bolt.</span></span>
* <span data-ttu-id="81f58-228">**Vykonavatelů**: informace o instancích hello hello spout nebo bolt.</span><span class="sxs-lookup"><span data-stu-id="81f58-228">**Executors**: Information about hello instances of hello spout or bolt.</span></span> <span data-ttu-id="81f58-229">Vyberte hello **Port** položka pro konkrétní vykonavatele tooview vytvořeného protokolu diagnostické informace pro tuto instanci.</span><span class="sxs-lookup"><span data-stu-id="81f58-229">Select hello **Port** entry for a specific executor tooview a log of diagnostic information produced for this instance.</span></span>
* <span data-ttu-id="81f58-230">**Chyby**: všechny informace o chybě pro tento spout nebo funkce bolt.</span><span class="sxs-lookup"><span data-stu-id="81f58-230">**Errors**: Any error information for this spout or bolt.</span></span>

## <a name="monitor-and-manage-rest-api"></a><span data-ttu-id="81f58-231">Sledování a správě: REST API</span><span class="sxs-lookup"><span data-stu-id="81f58-231">Monitor and manage: REST API</span></span>

<span data-ttu-id="81f58-232">Hello uživatelské rozhraní Storm je postavený na hello rozhraní REST API, takže můžete provádět podobné správy a monitorování funkce pomocí hello REST API.</span><span class="sxs-lookup"><span data-stu-id="81f58-232">hello Storm UI is built on top of hello REST API, so you can perform similar management and monitoring functionality by using hello REST API.</span></span> <span data-ttu-id="81f58-233">Můžete vytvořit vlastní nástroje toocreate hello REST API pro správu a monitorování topologie Storm.</span><span class="sxs-lookup"><span data-stu-id="81f58-233">You can use hello REST API toocreate custom tools for managing and monitoring Storm topologies.</span></span>

<span data-ttu-id="81f58-234">Další informace najdete v tématu [Storm uživatelského rozhraní REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html).</span><span class="sxs-lookup"><span data-stu-id="81f58-234">For more information, see [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html).</span></span> <span data-ttu-id="81f58-235">Hello tyto informace o konkrétní toousing hello REST API s Apache Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="81f58-235">hello following information is specific toousing hello REST API with Apache Storm on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="81f58-236">Hello Storm REST API není veřejně dostupné přes hello Internetu a musí být přístup pomocí SSH tunel toohello hlavního uzlu clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="81f58-236">hello Storm REST API is not publicly available over hello internet, and must be accessed using an SSH tunnel toohello HDInsight cluster head node.</span></span> <span data-ttu-id="81f58-237">Informace o vytváření a používání tunelového propojení SSH naleznete v tématu [používání tunelového propojení SSH tooaccess Ambari webové uživatelské rozhraní, ResourceManager, JobHistory, NameNode, Oozie a jiné webové uživatelská](hdinsight-linux-ambari-ssh-tunnel.md).</span><span class="sxs-lookup"><span data-stu-id="81f58-237">For information on creating and using an SSH tunnel, see [Use SSH Tunneling tooaccess Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, and other web UIs](hdinsight-linux-ambari-ssh-tunnel.md).</span></span>

### <a name="base-uri"></a><span data-ttu-id="81f58-238">Základní identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="81f58-238">Base URI</span></span>

<span data-ttu-id="81f58-239">Hello základní identifikátor URI pro hello rozhraní API REST v clusterech HDInsight se systémem Linux je k dispozici hello hlavního uzlu v **https://HEADNODEFQDN:8744/api/v1/**.</span><span class="sxs-lookup"><span data-stu-id="81f58-239">hello base URI for hello REST API on Linux-based HDInsight clusters is available on hello head node at **https://HEADNODEFQDN:8744/api/v1/**.</span></span> <span data-ttu-id="81f58-240">název domény Hello hello hlavního uzlu se vygeneruje při vytváření clusteru a nejsou statické.</span><span class="sxs-lookup"><span data-stu-id="81f58-240">hello domain name of hello head node is generated during cluster creation and is not static.</span></span>

<span data-ttu-id="81f58-241">Hello plně kvalifikovaný název domény (FQDN) pro hello hlavního uzlu clusteru můžete najít v několika různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="81f58-241">You can find hello fully qualified domain name (FQDN) for hello cluster head node in several different ways:</span></span>

* <span data-ttu-id="81f58-242">**Z relace SSH**: použijte příkaz hello `headnode -f` z clusteru toohello relace SSH.</span><span class="sxs-lookup"><span data-stu-id="81f58-242">**From an SSH session**: Use hello command `headnode -f` from an SSH session toohello cluster.</span></span>
* <span data-ttu-id="81f58-243">**Z webové Ambari**: vyberte **služby** hello horní části stránky hello, pak vyberte **Storm**.</span><span class="sxs-lookup"><span data-stu-id="81f58-243">**From Ambari Web**: Select **Services** from hello top of hello page, then select **Storm**.</span></span> <span data-ttu-id="81f58-244">Z hello **Souhrn** vyberte **serveru uživatelského rozhraní Storm**.</span><span class="sxs-lookup"><span data-stu-id="81f58-244">From hello **Summary** tab, select **Storm UI Server**.</span></span> <span data-ttu-id="81f58-245">Hello plně kvalifikovaný název domény hello uzlu se systémem hello uživatelské rozhraní Storm a rozhraní API REST je v horní části hello hello stránky.</span><span class="sxs-lookup"><span data-stu-id="81f58-245">hello FQDN of hello node that hello Storm UI and REST API is running is at hello top of hello page.</span></span>
* <span data-ttu-id="81f58-246">**Z rozhraní Ambari REST API**: použijte příkaz hello `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` tooretrieve informace o hello uzlu, který hello uživatelské rozhraní Storm a REST API, které jsou spuštěny na.</span><span class="sxs-lookup"><span data-stu-id="81f58-246">**From Ambari REST API**: Use hello command `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` tooretrieve information about hello node that hello Storm UI and REST API are running on.</span></span> <span data-ttu-id="81f58-247">Nahraďte **heslo** s hello heslo správce pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="81f58-247">Replace **PASSWORD** with hello admin password for hello cluster.</span></span> <span data-ttu-id="81f58-248">Nahraďte **CLUSTERNAME** s názvem clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="81f58-248">Replace **CLUSTERNAME** with hello cluster name.</span></span> <span data-ttu-id="81f58-249">V odpovědi hello obsahuje položku "název_hostitele" hello hello plně kvalifikovaný název domény uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="81f58-249">In hello response, hello "host_name" entry contains hello FQDN of hello node.</span></span>

### <a name="authentication"></a><span data-ttu-id="81f58-250">Authentication</span><span class="sxs-lookup"><span data-stu-id="81f58-250">Authentication</span></span>

<span data-ttu-id="81f58-251">Toohello požadavky musí používat rozhraní REST API **základní ověřování**, takže můžete použít název správce clusteru HDInsight hello a heslo.</span><span class="sxs-lookup"><span data-stu-id="81f58-251">Requests toohello REST API must use **basic authentication**, so you use hello HDInsight cluster administrator name and password.</span></span>

> [!NOTE]
> <span data-ttu-id="81f58-252">Protože základní ověřování je odeslána pomocí nešifrovaného textu, měli byste **vždy** používat s clusterem s hello toosecure komunikaci prostřednictvím protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="81f58-252">Because basic authentication is sent by using clear text, you should **always** use HTTPS toosecure communications with hello cluster.</span></span>

### <a name="return-values"></a><span data-ttu-id="81f58-253">Návratové hodnoty</span><span class="sxs-lookup"><span data-stu-id="81f58-253">Return values</span></span>

<span data-ttu-id="81f58-254">Informace, které se vrátí z hello REST API, může být pouze z použitelné v rámci clusteru hello nebo virtuální počítače na hello stejné virtuální síti Azure jako hello cluster.</span><span class="sxs-lookup"><span data-stu-id="81f58-254">Information that is returned from hello REST API may only be usable from within hello cluster or virtual machines on hello same Azure Virtual Network as hello cluster.</span></span> <span data-ttu-id="81f58-255">Například hello plně kvalifikovaný název domény (FQDN) pro servery Zookeeper vrátit není být dostupný z Internetu hello.</span><span class="sxs-lookup"><span data-stu-id="81f58-255">For example, hello fully qualified domain name (FQDN) returned for Zookeeper servers is not be accessible from hello Internet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="81f58-256">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81f58-256">Next Steps</span></span>

<span data-ttu-id="81f58-257">Teď, když jste se naučili, jak toodeploy a sledování topologie pomocí hello řídicí panel Storm, se naučíte, jak příliš[založené na jazyce Java vyvíjet topologie pomocí nástroje Maven](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="81f58-257">Now that you've learned how toodeploy and monitor topologies by using hello Storm Dashboard, learn how too[Develop Java-based topologies using Maven](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="81f58-258">Seznam Další příklad topologií najdete v tématu [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="81f58-258">For a list of more example topologies, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>
