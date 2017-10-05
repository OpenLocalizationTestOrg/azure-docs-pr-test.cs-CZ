---
title: "Nasazení a správa topologií Apache Storm v HDInsight se systémem Linux | Microsoft Docs"
description: "Zjistěte, jak pro nasazení, monitorování a správa topologií Apache Storm pomocí řídicího panelu Storm v HDInsight se systémem Linux. Pomocí nástroje Hadoop pro sadu Visual Studio."
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
ms.openlocfilehash: b9e82463030807d2674594e73f762fe93515d423
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-hdinsight"></a><span data-ttu-id="d73af-104">Nasazení a správa topologií Apache Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d73af-104">Deploy and manage Apache Storm topologies on HDInsight</span></span>

<span data-ttu-id="d73af-105">V tomto dokumentu seznámíte se základy správy a monitorování topologií Storm spuštěných na Storm v HDInsight clustery.</span><span class="sxs-lookup"><span data-stu-id="d73af-105">In this document, learn the basics of managing and monitoring Storm topologies running on Storm on HDInsight clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d73af-106">Kroky v tomto článku vyžadují Storm se systémem Linux v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d73af-106">The steps in this article require a Linux-based Storm on HDInsight cluster.</span></span> <span data-ttu-id="d73af-107">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="d73af-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d73af-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d73af-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> 
>
> <span data-ttu-id="d73af-109">Informace o nasazení a monitorování topologií v HDInsight se systémem Windows, najdete v části [nasazení a správa topologií Apache Storm v HDInsight se systémem Windows](hdinsight-storm-deploy-monitor-topology.md)</span><span class="sxs-lookup"><span data-stu-id="d73af-109">For information on deploying and monitoring topologies on Windows-based HDInsight, see [Deploy and manage Apache Storm topologies on Windows-based HDInsight](hdinsight-storm-deploy-monitor-topology.md)</span></span>


## <a name="prerequisites"></a><span data-ttu-id="d73af-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d73af-110">Prerequisites</span></span>

* <span data-ttu-id="d73af-111">**Storm v clusteru HDInsight se systémem Linux**: najdete v části [začít pracovat s Apache Storm v HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) pokyny týkající se vytvoření clusteru</span><span class="sxs-lookup"><span data-stu-id="d73af-111">**A Linux-based Storm on HDInsight cluster**: see [Get started with Apache Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) for steps on creating a cluster</span></span>

* <span data-ttu-id="d73af-112">(Volitelné) **Znalost SSH a SCP**: Další informace najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="d73af-112">(Optional) **Familiarity with SSH and SCP**: For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="d73af-113">(Volitelné) **Visual Studio**: Azure SDK 2.5.1 nebo novější a nástrojů Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d73af-113">(Optional) **Visual Studio**: Azure SDK 2.5.1 or newer and the Data Lake Tools for Visual Studio.</span></span> <span data-ttu-id="d73af-114">Další informace najdete v tématu [Začínáme pomocí nástrojů Data Lake pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d73af-114">For more information, see [Get started using Data Lake Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

    <span data-ttu-id="d73af-115">Jedna z následujících verzí sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d73af-115">One of the following versions of Visual Studio:</span></span>

  * <span data-ttu-id="d73af-116">Visual Studio 2012 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="d73af-116">Visual Studio 2012 with [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>

  * <span data-ttu-id="d73af-117">Visual Studio 2013 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=44921) nebo [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span><span class="sxs-lookup"><span data-stu-id="d73af-117">Visual Studio 2013 with [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span></span>
  * [<span data-ttu-id="d73af-118">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="d73af-118">Visual Studio 2015</span></span>](https://www.visualstudio.com/downloads/)

  * <span data-ttu-id="d73af-119">Visual Studio 2015 (všechny edice)</span><span class="sxs-lookup"><span data-stu-id="d73af-119">Visual Studio 2015 (any edition)</span></span>

  * <span data-ttu-id="d73af-120">Visual Studio 2017 (všechny edice).</span><span class="sxs-lookup"><span data-stu-id="d73af-120">Visual Studio 2017 (any edition).</span></span> <span data-ttu-id="d73af-121">Nástroje data Lake pro Visual Studio 2017 se instalují jako součást pracovního vytížení Azure.</span><span class="sxs-lookup"><span data-stu-id="d73af-121">Data Lake Tools for Visual Studio 2017 are installed as part of the Azure Workload.</span></span>

## <a name="submit-a-topology-visual-studio"></a><span data-ttu-id="d73af-122">Odeslání topologii: Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d73af-122">Submit a topology: Visual Studio</span></span>

<span data-ttu-id="d73af-123">Nástroje HDInsight slouží k odeslání jazyka C# nebo hybridní topologie pro váš cluster Storm.</span><span class="sxs-lookup"><span data-stu-id="d73af-123">The HDInsight Tools can be used to submit C# or hybrid topologies to your Storm cluster.</span></span> <span data-ttu-id="d73af-124">Následující postup použijte ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d73af-124">The following steps use a sample application.</span></span> <span data-ttu-id="d73af-125">Informace o vytváření vlastního topologie pomocí nástrojů HDInsight naleznete v tématu [vývoj topologií C# pomocí nástrojů HDInsight pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="d73af-125">For information about creating your own topologies by using the HDInsight Tools, see [Develop C# topologies using the HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

1. <span data-ttu-id="d73af-126">Pokud jste ještě nenainstalovali nejnovější verzi nástroje Data Lake pro Visual Studio, najdete v části [Začínáme pomocí nástrojů Data Lake pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d73af-126">If you have not already installed the latest version of the Data Lake tools for Visual Studio, see [Get started using Data Lake Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="d73af-127">Nástroje Data Lake pro Visual Studio se dřív označovaly jako nástroje HDInsight pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d73af-127">The Data Lake Tools for Visual Studio were formerly called the HDInsight Tools for Visual Studio.</span></span>
    >
    > <span data-ttu-id="d73af-128">Nástroje data Lake pro Visual Studio jsou součástí __pracovního vytížení Azure__ pro Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d73af-128">Data Lake Tools for Visual Studio are included in the __Azure Workload__ for Visual Studio 2017.</span></span>

2. <span data-ttu-id="d73af-129">Otevřete Visual Studio, vyberte **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="d73af-129">Open Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="d73af-130">V **nový projekt** dialogové okno, rozbalte seznam **nainstalovaná** > **šablony**a potom vyberte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="d73af-130">In the **New Project** dialog box, expand **Installed** > **Templates**, and then select **HDInsight**.</span></span> <span data-ttu-id="d73af-131">V seznamu šablon vyberte **Storm ukázka**.</span><span class="sxs-lookup"><span data-stu-id="d73af-131">From the list of templates, select **Storm Sample**.</span></span> <span data-ttu-id="d73af-132">V dolní části dialogových oken zadejte název aplikace.</span><span class="sxs-lookup"><span data-stu-id="d73af-132">At the bottom of the dialog box, type a name for the application.</span></span>

    ![Bitové kopie](./media/hdinsight-storm-deploy-monitor-topology-linux/sample.png)

4. <span data-ttu-id="d73af-134">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **odeslání do Storm v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="d73af-134">In **Solution Explorer**, right-click the project, and select **Submit to Storm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d73af-135">Pokud se zobrazí výzva, zadejte přihlašovací údaje pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="d73af-135">If prompted, enter the login credentials for your Azure subscription.</span></span> <span data-ttu-id="d73af-136">Pokud máte více než jedno předplatné, přihlaste se k ta, která obsahuje váš cluster Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d73af-136">If you have more than one subscription, log in to the one that contains your Storm on HDInsight cluster.</span></span>

5. <span data-ttu-id="d73af-137">Vyberte váš cluster Storm v HDInsight z **Storm Cluster** rozevíracího seznamu a potom vyberte **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="d73af-137">Select your Storm on HDInsight cluster from the **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="d73af-138">Můžete sledovat, jestli je úspěšně odesílání pomocí **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="d73af-138">You can monitor whether the submission is successful by using the **Output** window.</span></span>

## <a name="submit-a-topology-ssh-and-the-storm-command"></a><span data-ttu-id="d73af-139">Odeslání topologii: SSH a příkaz Storm</span><span class="sxs-lookup"><span data-stu-id="d73af-139">Submit a topology: SSH and the Storm command</span></span>

1. <span data-ttu-id="d73af-140">Použití SSH se připojit ke clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d73af-140">Use SSH to connect to the HDInsight cluster.</span></span> <span data-ttu-id="d73af-141">Nahraďte **uživatelské jméno** název vaší přihlašování přes SSH.</span><span class="sxs-lookup"><span data-stu-id="d73af-141">Replace **USERNAME** the name of your SSH login.</span></span> <span data-ttu-id="d73af-142">Nahraďte **CLUSTERNAME** názvem clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="d73af-142">Replace **CLUSTERNAME** with your HDInsight cluster name:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    <span data-ttu-id="d73af-143">Další informace o používání SSH připojit ke svému clusteru HDInsight najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="d73af-143">For more information on using SSH to connect to your HDInsight cluster, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="d73af-144">Následující příkaz použijte ke spuštění ukázkové topologie:</span><span class="sxs-lookup"><span data-stu-id="d73af-144">Use the following command to start an example topology:</span></span>

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology WordCount

    <span data-ttu-id="d73af-145">Tento příkaz spustí ukázkovou topologii WordCount v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d73af-145">This command starts the example WordCount topology on the cluster.</span></span> <span data-ttu-id="d73af-146">Tato topologie náhodně generovat věty a určení počtu výskytů jednotlivých slov ve větě.</span><span class="sxs-lookup"><span data-stu-id="d73af-146">This topology randomly generate sentences and count the occurrence of each word in the sentences.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d73af-147">Při odesílání topologie do clusteru je nutné nejprve zkopírovat soubor jar obsahující cluster před použitím příkazu `storm`.</span><span class="sxs-lookup"><span data-stu-id="d73af-147">When submitting topology to the cluster, you must first copy the jar file containing the cluster before using the `storm` command.</span></span> <span data-ttu-id="d73af-148">Při kopírování souboru do clusteru, můžete použít `scp` příkaz.</span><span class="sxs-lookup"><span data-stu-id="d73af-148">To copy the file to the cluster, you can use the `scp` command.</span></span> <span data-ttu-id="d73af-149">Například `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`.</span><span class="sxs-lookup"><span data-stu-id="d73af-149">For example, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span></span>
   >
   > <span data-ttu-id="d73af-150">Příklad WordCount a další příklady starter storm jsou již zahrnuty v clusteru na `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span><span class="sxs-lookup"><span data-stu-id="d73af-150">The WordCount example, and other storm starter examples, are already included on your cluster at `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span></span>

## <a name="submit-a-topology-programmatically"></a><span data-ttu-id="d73af-151">Odeslání topologii: prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="d73af-151">Submit a topology: programmatically</span></span>

<span data-ttu-id="d73af-152">Prostřednictvím kódu programu můžete nasadit s topologií pro Storm v HDInsight komunikaci se službou Nimbus hostovaná v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d73af-152">You can programmatically deploy a topology to Storm on HDInsight by communicating with the Nimbus service hosted in your cluster.</span></span> <span data-ttu-id="d73af-153">[https://github.com/Azure-Samples/hdinsight-Java-Deploy-Storm-Topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) poskytuje příklad aplikaci Java, která ukazuje, jak nasadit a spustit topologii prostřednictvím služby Nimbus.</span><span class="sxs-lookup"><span data-stu-id="d73af-153">[https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) provides an example Java application that demonstrates how to deploy and start a topology through the Nimbus service.</span></span>

## <a name="monitor-and-manage-visual-studio"></a><span data-ttu-id="d73af-154">Sledování a správě: Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d73af-154">Monitor and manage: Visual Studio</span></span>

<span data-ttu-id="d73af-155">Když topologii byla úspěšně odeslána pomocí sady Visual Studio **topologie Storm** clusteru se zobrazí v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d73af-155">When a topology has been successfully submitted using Visual Studio, the **Storm Topologies** view for the cluster appears.</span></span> <span data-ttu-id="d73af-156">Vyberte topologii ze seznamu a zobrazit informace o spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="d73af-156">Select the topology from the list to view information about the running topology.</span></span>

![monitorování v sadě Visual studio](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

> [!NOTE]
> <span data-ttu-id="d73af-158">Můžete také zobrazit **topologie Storm** z **Průzkumníka serveru** rozšířením **Azure** > **HDInsight**a potom kliknete pravým tlačítkem cluster Storm v HDInsight a výběr **topologie Storm zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="d73af-158">You can also view **Storm Topologies** from **Server Explorer** by expanding **Azure** > **HDInsight**, and then right-clicking a Storm on HDInsight cluster, and selecting **View Storm Topologies**.</span></span>

<span data-ttu-id="d73af-159">Vyberte tvar funkcích spouts nebo funkce bolts zobrazíte informace o těchto součástí.</span><span class="sxs-lookup"><span data-stu-id="d73af-159">Select the shape for the spouts or bolts to view information about these components.</span></span> <span data-ttu-id="d73af-160">Otevře se nové okno pro každé vybrané položky.</span><span class="sxs-lookup"><span data-stu-id="d73af-160">A new window opens for each item selected.</span></span>

### <a name="deactivate-and-reactivate"></a><span data-ttu-id="d73af-161">Deaktivovat a znovu aktivovat</span><span class="sxs-lookup"><span data-stu-id="d73af-161">Deactivate and reactivate</span></span>

<span data-ttu-id="d73af-162">Deaktivace topologii pozastaví ho, dokud nebude ukončen nebo znovu aktivovat.</span><span class="sxs-lookup"><span data-stu-id="d73af-162">Deactivating a topology pauses it until it is killed or reactivated.</span></span> <span data-ttu-id="d73af-163">Pokud chcete provést tyto operace, použijte __deaktivovat__ a __znovu aktivovat__ tlačítka v horní části __souhrn topologie__.</span><span class="sxs-lookup"><span data-stu-id="d73af-163">To perform these operations, use the __Deactivate__ and __Reactivate__ buttons at the top of the __Topology Summary__.</span></span>

### <a name="rebalance"></a><span data-ttu-id="d73af-164">Znovu vyvážit</span><span class="sxs-lookup"><span data-stu-id="d73af-164">Rebalance</span></span>

<span data-ttu-id="d73af-165">Vyrovnává topologii umožňuje systému zkontrolovat, jestli paralelismus topologii.</span><span class="sxs-lookup"><span data-stu-id="d73af-165">Rebalancing a topology allows the system to revise the parallelism of the topology.</span></span> <span data-ttu-id="d73af-166">Například pokud jste změnili velikost clusteru přidat další poznámky, vyrovnává umožňuje topologii zobrazíte nové uzly.</span><span class="sxs-lookup"><span data-stu-id="d73af-166">For example, if you have resized the cluster to add more notes, rebalancing allows a topology to see the new nodes.</span></span>

<span data-ttu-id="d73af-167">Chcete-li znovu vyvážit topologii, použijte __znovu vyvážit__ tlačítka v horní části __souhrn topologie__.</span><span class="sxs-lookup"><span data-stu-id="d73af-167">To rebalance a topology, use the __Rebalance__ button at the top of the __Topology Summary__.</span></span>

> [!WARNING]
> <span data-ttu-id="d73af-168">Vyrovnává topologii nejprve deaktivuje topologii, pak znovu rovnoměrně distribuuje pracovních procesů v clusteru a potom nakonec vrátí topologie do stavu, ve kterém se nacházel před vyrovnává došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="d73af-168">Rebalancing a topology first deactivates the topology, then redistributes workers evenly across the cluster, then finally returns the topology to the state it was in before rebalancing occurred.</span></span> <span data-ttu-id="d73af-169">Takže pokud topologii byl aktivní, se zase aktivní.</span><span class="sxs-lookup"><span data-stu-id="d73af-169">So if the topology was active, it becomes active again.</span></span> <span data-ttu-id="d73af-170">Pokud se ji deaktivovala, zůstane deaktivované.</span><span class="sxs-lookup"><span data-stu-id="d73af-170">If it was deactivated, it remains deactivated.</span></span>

### <a name="kill-a-topology"></a><span data-ttu-id="d73af-171">Příkaz kill topologii</span><span class="sxs-lookup"><span data-stu-id="d73af-171">Kill a topology</span></span>

<span data-ttu-id="d73af-172">Topologie Storm pokračovat, spuštění, dokud jsou zastaveny nebo odstranění clusteru.</span><span class="sxs-lookup"><span data-stu-id="d73af-172">Storm topologies continue running until they are stopped or the cluster is deleted.</span></span> <span data-ttu-id="d73af-173">Chcete-li zastavit topologii, použijte __Kill__ tlačítka v horní části __souhrn topologie__.</span><span class="sxs-lookup"><span data-stu-id="d73af-173">To stop a topology, use the __Kill__ button at the top of the __Topology Summary__.</span></span>

## <a name="monitor-and-manage-ssh-and-the-storm-command"></a><span data-ttu-id="d73af-174">Sledování a správě: SSH a příkaz Storm</span><span class="sxs-lookup"><span data-stu-id="d73af-174">Monitor and manage: SSH and the Storm command</span></span>

<span data-ttu-id="d73af-175">`storm` Nástroj umožňuje pracovat se spuštěnými topologiemi z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d73af-175">The `storm` utility allows you to work with running topologies from the command line.</span></span> <span data-ttu-id="d73af-176">Použití `storm -h` pro úplný seznam příkazů.</span><span class="sxs-lookup"><span data-stu-id="d73af-176">Use `storm -h` for a full list of commands.</span></span>

### <a name="list-topologies"></a><span data-ttu-id="d73af-177">Seznam topologie</span><span class="sxs-lookup"><span data-stu-id="d73af-177">List topologies</span></span>

<span data-ttu-id="d73af-178">Použijte následující příkaz, který seznam všech spuštěnými topologiemi:</span><span class="sxs-lookup"><span data-stu-id="d73af-178">Use the following command to list all running topologies:</span></span>

    storm list

<span data-ttu-id="d73af-179">Tento příkaz by měl vrátit informace podobné následujícímu textu:</span><span class="sxs-lookup"><span data-stu-id="d73af-179">This command returns information similar to the following text:</span></span>

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a><span data-ttu-id="d73af-180">Deaktivovat a znovu aktivovat</span><span class="sxs-lookup"><span data-stu-id="d73af-180">Deactivate and reactivate</span></span>

<span data-ttu-id="d73af-181">Deaktivace topologii pozastaví ho, dokud nebude ukončen nebo znovu aktivovat.</span><span class="sxs-lookup"><span data-stu-id="d73af-181">Deactivating a topology pauses it until it is killed or reactivated.</span></span> <span data-ttu-id="d73af-182">Znovu aktivovat a deaktivovat použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d73af-182">Use the following command to deactivate and reactivate:</span></span>

    storm Deactivate TOPOLOGYNAME

    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a><span data-ttu-id="d73af-183">Příkaz kill spuštěné topologie</span><span class="sxs-lookup"><span data-stu-id="d73af-183">Kill a running topology</span></span>

<span data-ttu-id="d73af-184">Storm topologie, jednou spustit, pokračovat spuštění, dokud se zastavila.</span><span class="sxs-lookup"><span data-stu-id="d73af-184">Storm topologies, once started, continue running until stopped.</span></span> <span data-ttu-id="d73af-185">Chcete-li zastavit topologii, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d73af-185">To stop a topology, use the following command:</span></span>

    storm kill TOPOLOGYNAME

### <a name="rebalance"></a><span data-ttu-id="d73af-186">Znovu vyvážit</span><span class="sxs-lookup"><span data-stu-id="d73af-186">Rebalance</span></span>

<span data-ttu-id="d73af-187">Vyrovnává topologii umožňuje systému zkontrolovat, jestli paralelismus topologii.</span><span class="sxs-lookup"><span data-stu-id="d73af-187">Rebalancing a topology allows the system to revise the parallelism of the topology.</span></span> <span data-ttu-id="d73af-188">Například pokud jste změnili velikost clusteru přidat další poznámky, vyrovnává umožňuje topologii zobrazíte nové uzly.</span><span class="sxs-lookup"><span data-stu-id="d73af-188">For example, if you have resized the cluster to add more notes, rebalancing allows a topology to see the new nodes.</span></span>

> [!WARNING]
> <span data-ttu-id="d73af-189">Vyrovnává topologii nejprve deaktivuje topologii, pak znovu rovnoměrně distribuuje pracovních procesů v clusteru a potom nakonec vrátí topologie do stavu, ve kterém se nacházel před vyrovnává došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="d73af-189">Rebalancing a topology first deactivates the topology, then redistributes workers evenly across the cluster, then finally returns the topology to the state it was in before rebalancing occurred.</span></span> <span data-ttu-id="d73af-190">Takže pokud topologii byl aktivní, se zase aktivní.</span><span class="sxs-lookup"><span data-stu-id="d73af-190">So if the topology was active, it becomes active again.</span></span> <span data-ttu-id="d73af-191">Pokud se ji deaktivovala, zůstane deaktivované.</span><span class="sxs-lookup"><span data-stu-id="d73af-191">If it was deactivated, it remains deactivated.</span></span>

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-storm-ui"></a><span data-ttu-id="d73af-192">Sledování a správě: Storm uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d73af-192">Monitor and manage: Storm UI</span></span>

<span data-ttu-id="d73af-193">Uživatelské rozhraní Storm poskytuje webové rozhraní pro práci se spuštěnými topologiemi a je součástí clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d73af-193">The Storm UI provides a web interface for working with running topologies, and is included on your HDInsight cluster.</span></span> <span data-ttu-id="d73af-194">Chcete-li zobrazit uživatelské rozhraní Storm, otevřete pomocí webového prohlížeče **https://CLUSTERNAME.azurehdinsight.net/stormui**, kde **CLUSTERNAME** je název clusteru.</span><span class="sxs-lookup"><span data-stu-id="d73af-194">To view the Storm UI, use a web browser to open **https://CLUSTERNAME.azurehdinsight.net/stormui**, where **CLUSTERNAME** is the name of your cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="d73af-195">Pokud budete vyzváni k zadání uživatelského jména a hesla, zadejte správce clusteru (admin) a heslo použité při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="d73af-195">If asked to provide a user name and password, enter the cluster administrator (admin) and password that you used when creating the cluster.</span></span>

### <a name="main-page"></a><span data-ttu-id="d73af-196">Hlavní stránka</span><span class="sxs-lookup"><span data-stu-id="d73af-196">Main page</span></span>

<span data-ttu-id="d73af-197">Hlavní stránka uživatelského rozhraní Storm poskytuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="d73af-197">The main page of the Storm UI provides the following information:</span></span>

* <span data-ttu-id="d73af-198">**Souhrn clusteru**: základní informace o clusteru Storm.</span><span class="sxs-lookup"><span data-stu-id="d73af-198">**Cluster summary**: Basic information about the Storm cluster.</span></span>
* <span data-ttu-id="d73af-199">**Souhrn topologie**: seznam spuštěných topologií.</span><span class="sxs-lookup"><span data-stu-id="d73af-199">**Topology summary**: A list of running topologies.</span></span> <span data-ttu-id="d73af-200">Chcete-li zobrazit další informace o konkrétní topologie pomocí odkazů v této části.</span><span class="sxs-lookup"><span data-stu-id="d73af-200">Use the links in this section to view more information about specific topologies.</span></span>
* <span data-ttu-id="d73af-201">**Souhrn nadřízeného**: informace o nadřízeného Storm.</span><span class="sxs-lookup"><span data-stu-id="d73af-201">**Supervisor summary**: Information about the Storm supervisor.</span></span>
* <span data-ttu-id="d73af-202">**Konfigurace nimbus**: Nimbus konfiguraci pro daný cluster.</span><span class="sxs-lookup"><span data-stu-id="d73af-202">**Nimbus configuration**: Nimbus configuration for the cluster.</span></span>

### <a name="topology-summary"></a><span data-ttu-id="d73af-203">Souhrn topologie</span><span class="sxs-lookup"><span data-stu-id="d73af-203">Topology summary</span></span>

<span data-ttu-id="d73af-204">Výběr odkazu z **souhrn topologie** části zobrazí následující informace o topologii:</span><span class="sxs-lookup"><span data-stu-id="d73af-204">Selecting a link from the **Topology summary** section displays the following information about the topology:</span></span>

* <span data-ttu-id="d73af-205">**Souhrn topologie**: základní informace o topologii.</span><span class="sxs-lookup"><span data-stu-id="d73af-205">**Topology summary**: Basic information about the topology.</span></span>
* <span data-ttu-id="d73af-206">**Topologie akce**: akce správy, které můžete provést pro topologii.</span><span class="sxs-lookup"><span data-stu-id="d73af-206">**Topology actions**: Management actions that you can perform for the topology.</span></span>

  * <span data-ttu-id="d73af-207">**Aktivovat**: obnoví zpracování deaktivované topologie.</span><span class="sxs-lookup"><span data-stu-id="d73af-207">**Activate**: Resumes processing of a deactivated topology.</span></span>
  * <span data-ttu-id="d73af-208">**Deaktivovat**: Pozastaví spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="d73af-208">**Deactivate**: Pauses a running topology.</span></span>
  * <span data-ttu-id="d73af-209">**Znovu vyvážit**: upraví paralelismus topologii.</span><span class="sxs-lookup"><span data-stu-id="d73af-209">**Rebalance**: Adjusts the parallelism of the topology.</span></span> <span data-ttu-id="d73af-210">Po změně počtu uzlů v clusteru musíte znovu vyvážit spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="d73af-210">You should rebalance running topologies after you have changed the number of nodes in the cluster.</span></span> <span data-ttu-id="d73af-211">Tato operace umožňuje topologii upravovat paralelismus za účelem kompenzace pro vyšší nebo ke snížení počtu uzlů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d73af-211">This operation allows the topology to adjust parallelism to compensate for the increased or decreased number of nodes in the cluster.</span></span>

    <span data-ttu-id="d73af-212">Další informace naleznete v části <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Pochopení paralelismu topologie Storm</a>.</span><span class="sxs-lookup"><span data-stu-id="d73af-212">For more information, see <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Understanding the parallelism of a Storm topology</a>.</span></span>
  * <span data-ttu-id="d73af-213">**Příkaz kill**: ukončí topologii Storm po zadaný časový limit.</span><span class="sxs-lookup"><span data-stu-id="d73af-213">**Kill**: Terminates a Storm topology after the specified timeout.</span></span>
* <span data-ttu-id="d73af-214">**Statistiky topologie**: Statistika týkající se topologie.</span><span class="sxs-lookup"><span data-stu-id="d73af-214">**Topology stats**: Statistics about the topology.</span></span> <span data-ttu-id="d73af-215">Pokud chcete nastavit časový rámec pro zbývající záznamy na stránce, použijte odkazy v **okno** sloupce.</span><span class="sxs-lookup"><span data-stu-id="d73af-215">To set the timeframe for the remaining entries on the page, use the links in the **Window** column.</span></span>
* <span data-ttu-id="d73af-216">**Spouts**: funkcích spouts používané topologii.</span><span class="sxs-lookup"><span data-stu-id="d73af-216">**Spouts**: The spouts used by the topology.</span></span> <span data-ttu-id="d73af-217">Chcete-li zobrazit další informace o konkrétních funkcích spouts pomocí odkazů v této části.</span><span class="sxs-lookup"><span data-stu-id="d73af-217">Use the links in this section to view more information about specific spouts.</span></span>
* <span data-ttu-id="d73af-218">**Bolts**: funkce bolts používané topologii.</span><span class="sxs-lookup"><span data-stu-id="d73af-218">**Bolts**: The bolts used by the topology.</span></span> <span data-ttu-id="d73af-219">Použijte odkazy v této části zobrazíte další informace o konkrétních funkcích bolts.</span><span class="sxs-lookup"><span data-stu-id="d73af-219">Use the links in this section to view more information about specific bolts.</span></span>
* <span data-ttu-id="d73af-220">**Topologie konfigurace**: konfiguraci vybrané topologie.</span><span class="sxs-lookup"><span data-stu-id="d73af-220">**Topology configuration**: The configuration of the selected topology.</span></span>

### <a name="spout-and-bolt-summary"></a><span data-ttu-id="d73af-221">Souhrn funkcí Bolt a spout</span><span class="sxs-lookup"><span data-stu-id="d73af-221">Spout and Bolt summary</span></span>

<span data-ttu-id="d73af-222">Výběr funkcí spout z **Spouts** nebo **Bolts** části zobrazí následující informace o vybrané položce:</span><span class="sxs-lookup"><span data-stu-id="d73af-222">Selecting a spout from the **Spouts** or **Bolts** sections displays the following information about the selected item:</span></span>

* <span data-ttu-id="d73af-223">**Souhrn součást**: základní informace o funkcích spout nebo bolt.</span><span class="sxs-lookup"><span data-stu-id="d73af-223">**Component summary**: Basic information about the spout or bolt.</span></span>
* <span data-ttu-id="d73af-224">**Statistiky funkcí spout/Bolt**: statistiky o funkcích spout nebo bolt.</span><span class="sxs-lookup"><span data-stu-id="d73af-224">**Spout/Bolt stats**: Statistics about the spout or bolt.</span></span> <span data-ttu-id="d73af-225">Pokud chcete nastavit časový rámec pro zbývající záznamy na stránce, použijte odkazy v **okno** sloupce.</span><span class="sxs-lookup"><span data-stu-id="d73af-225">To set the timeframe for the remaining entries on the page, use the links in the **Window** column.</span></span>
* <span data-ttu-id="d73af-226">**Statistiky vstupu** (pouze funkce bolt): informace o vstupní datové proudy využívaná funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="d73af-226">**Input stats** (bolt only): Information about the input streams consumed by the bolt.</span></span>
* <span data-ttu-id="d73af-227">**Statististiky výstupu**: informace o datové proudy vygenerované tímto objektem spout nebo funkce bolt.</span><span class="sxs-lookup"><span data-stu-id="d73af-227">**Output stats**: Information about the streams emitted by this spout or bolt.</span></span>
* <span data-ttu-id="d73af-228">**Vykonavatelů**: informace o funkcích spout nebo bolt instance.</span><span class="sxs-lookup"><span data-stu-id="d73af-228">**Executors**: Information about the instances of the spout or bolt.</span></span> <span data-ttu-id="d73af-229">Vyberte **Port** vytváří záznam pro konkrétní vykonavatele chcete zobrazit protokol diagnostické informace pro tuto instanci.</span><span class="sxs-lookup"><span data-stu-id="d73af-229">Select the **Port** entry for a specific executor to view a log of diagnostic information produced for this instance.</span></span>
* <span data-ttu-id="d73af-230">**Chyby**: všechny informace o chybě pro tento spout nebo funkce bolt.</span><span class="sxs-lookup"><span data-stu-id="d73af-230">**Errors**: Any error information for this spout or bolt.</span></span>

## <a name="monitor-and-manage-rest-api"></a><span data-ttu-id="d73af-231">Sledování a správě: REST API</span><span class="sxs-lookup"><span data-stu-id="d73af-231">Monitor and manage: REST API</span></span>

<span data-ttu-id="d73af-232">Uživatelské rozhraní Storm je postavený na rozhraní REST API, takže můžete provádět podobné správy a monitorování funkce pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="d73af-232">The Storm UI is built on top of the REST API, so you can perform similar management and monitoring functionality by using the REST API.</span></span> <span data-ttu-id="d73af-233">Rozhraní REST API můžete použít k vytvoření vlastních nástrojů pro správu a monitorování topologie Storm.</span><span class="sxs-lookup"><span data-stu-id="d73af-233">You can use the REST API to create custom tools for managing and monitoring Storm topologies.</span></span>

<span data-ttu-id="d73af-234">Další informace najdete v tématu [Storm uživatelského rozhraní REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html).</span><span class="sxs-lookup"><span data-stu-id="d73af-234">For more information, see [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html).</span></span> <span data-ttu-id="d73af-235">Tyto informace je specifická pro Apache Storm v HDInsight pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="d73af-235">The following information is specific to using the REST API with Apache Storm on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d73af-236">Rozhraní API REST Storm není veřejně dostupné přes internet a musí být přístup pomocí tunelového propojení SSH k hlavnímu uzlu clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d73af-236">The Storm REST API is not publicly available over the internet, and must be accessed using an SSH tunnel to the HDInsight cluster head node.</span></span> <span data-ttu-id="d73af-237">Informace o vytváření a používání tunelového propojení SSH naleznete v tématu [používání tunelového propojení SSH pro přístup k webovému uživatelskému rozhraní Ambari, ResourceManager, JobHistory, NameNode, Oozie a jiným webovým uživatelská rozhraní](hdinsight-linux-ambari-ssh-tunnel.md).</span><span class="sxs-lookup"><span data-stu-id="d73af-237">For information on creating and using an SSH tunnel, see [Use SSH Tunneling to access Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, and other web UIs](hdinsight-linux-ambari-ssh-tunnel.md).</span></span>

### <a name="base-uri"></a><span data-ttu-id="d73af-238">Základní identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="d73af-238">Base URI</span></span>

<span data-ttu-id="d73af-239">Základní identifikátor URI pro rozhraní API REST v clusterech HDInsight se systémem Linux je dostupné z hlavního uzlu v **https://HEADNODEFQDN:8744/api/v1/**.</span><span class="sxs-lookup"><span data-stu-id="d73af-239">The base URI for the REST API on Linux-based HDInsight clusters is available on the head node at **https://HEADNODEFQDN:8744/api/v1/**.</span></span> <span data-ttu-id="d73af-240">Název domény hlavního uzlu se vygeneruje při vytváření clusteru a nejsou statické.</span><span class="sxs-lookup"><span data-stu-id="d73af-240">The domain name of the head node is generated during cluster creation and is not static.</span></span>

<span data-ttu-id="d73af-241">Plně kvalifikovaný název domény (FQDN) pro hlavního uzlu clusteru najdete v několika různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="d73af-241">You can find the fully qualified domain name (FQDN) for the cluster head node in several different ways:</span></span>

* <span data-ttu-id="d73af-242">**Z relace SSH**: použijte příkaz `headnode -f` z relace SSH do clusteru.</span><span class="sxs-lookup"><span data-stu-id="d73af-242">**From an SSH session**: Use the command `headnode -f` from an SSH session to the cluster.</span></span>
* <span data-ttu-id="d73af-243">**Z webové Ambari**: vyberte **služby** z horní části stránky, pak vyberte **Storm**.</span><span class="sxs-lookup"><span data-stu-id="d73af-243">**From Ambari Web**: Select **Services** from the top of the page, then select **Storm**.</span></span> <span data-ttu-id="d73af-244">Z **Souhrn** vyberte **serveru uživatelského rozhraní Storm**.</span><span class="sxs-lookup"><span data-stu-id="d73af-244">From the **Summary** tab, select **Storm UI Server**.</span></span> <span data-ttu-id="d73af-245">Plně kvalifikovaný název domény uzlu, který běží uživatelské rozhraní Storm a rozhraní API REST je v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="d73af-245">The FQDN of the node that the Storm UI and REST API is running is at the top of the page.</span></span>
* <span data-ttu-id="d73af-246">**Z rozhraní Ambari REST API**: použijte příkaz `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` k načtení informací o uzlu, který na běží uživatelské rozhraní Storm a REST API.</span><span class="sxs-lookup"><span data-stu-id="d73af-246">**From Ambari REST API**: Use the command `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` to retrieve information about the node that the Storm UI and REST API are running on.</span></span> <span data-ttu-id="d73af-247">Nahraďte **heslo** s heslo správce pro cluster.</span><span class="sxs-lookup"><span data-stu-id="d73af-247">Replace **PASSWORD** with the admin password for the cluster.</span></span> <span data-ttu-id="d73af-248">Nahraďte **CLUSTERNAME** se název clusteru.</span><span class="sxs-lookup"><span data-stu-id="d73af-248">Replace **CLUSTERNAME** with the cluster name.</span></span> <span data-ttu-id="d73af-249">V odpovědi na položku "název_hostitele" obsahuje plně kvalifikovaný název domény uzlu.</span><span class="sxs-lookup"><span data-stu-id="d73af-249">In the response, the "host_name" entry contains the FQDN of the node.</span></span>

### <a name="authentication"></a><span data-ttu-id="d73af-250">Authentication</span><span class="sxs-lookup"><span data-stu-id="d73af-250">Authentication</span></span>

<span data-ttu-id="d73af-251">Požadavky na rozhraní REST API musí používat **základní ověřování**, takže můžete použít název správce clusteru HDInsight a heslo.</span><span class="sxs-lookup"><span data-stu-id="d73af-251">Requests to the REST API must use **basic authentication**, so you use the HDInsight cluster administrator name and password.</span></span>

> [!NOTE]
> <span data-ttu-id="d73af-252">Protože základní ověřování je odeslána pomocí nešifrovaného textu, měli byste **vždy** používat protokol HTTPS pro zabezpečenou komunikaci s clusterem.</span><span class="sxs-lookup"><span data-stu-id="d73af-252">Because basic authentication is sent by using clear text, you should **always** use HTTPS to secure communications with the cluster.</span></span>

### <a name="return-values"></a><span data-ttu-id="d73af-253">Návratové hodnoty</span><span class="sxs-lookup"><span data-stu-id="d73af-253">Return values</span></span>

<span data-ttu-id="d73af-254">Informace, které se vrátí z rozhraní API REST může být pouze z použitelné v rámci clusteru nebo virtuální počítače ve stejné virtuální síti Azure jako cluster.</span><span class="sxs-lookup"><span data-stu-id="d73af-254">Information that is returned from the REST API may only be usable from within the cluster or virtual machines on the same Azure Virtual Network as the cluster.</span></span> <span data-ttu-id="d73af-255">Plně kvalifikovaný název domény (FQDN) vrátil pro servery Zookeeper se není třeba přístupný z Internetu.</span><span class="sxs-lookup"><span data-stu-id="d73af-255">For example, the fully qualified domain name (FQDN) returned for Zookeeper servers is not be accessible from the Internet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d73af-256">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d73af-256">Next Steps</span></span>

<span data-ttu-id="d73af-257">Teď, když jste se naučili jak nasadit a monitorovat topologie pomocí řídicího panelu Storm, přečtěte si, jak [založené na jazyce Java vyvíjet topologie pomocí nástroje Maven](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="d73af-257">Now that you've learned how to deploy and monitor topologies by using the Storm Dashboard, learn how to [Develop Java-based topologies using Maven](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="d73af-258">Seznam Další příklad topologií najdete v tématu [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="d73af-258">For a list of more example topologies, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>
