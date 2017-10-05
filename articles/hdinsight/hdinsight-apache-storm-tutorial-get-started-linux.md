---
title: "Příklady topologie Storm Starter v systému Apache Storm v HDInsight – Azure | Dokumentace Microsoftu"
description: "Přečtěte si, jak provádět analýzu velkých objemů dat a zpracovávat data v reálném časem pomocí Apache Storm a příkladů topologie Storm Starter ve službě HDInsight."
keywords: "Storm Starter, příklad Apache Storm"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: d710dcac-35d1-4c27-a8d6-acaf8146b485
ms.service: hdinsight
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 83fc6db1ddb43eb87e7c58684505d7196c1e53d0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
#<a name="get-started-with-apache-storm-on-hdinsight-using-the-storm-starter-examples"></a><span data-ttu-id="2f42f-104">Začínáme s Apache Storm v HDInsight pomocí příkladů topologie Storm Starter</span><span class="sxs-lookup"><span data-stu-id="2f42f-104">Get started with Apache Storm on HDInsight using the storm-starter examples</span></span>

<span data-ttu-id="2f42f-105">Naučte se používat systém Apache Storm v HDInsight pomocí příkladů topologie Storm Starter.</span><span class="sxs-lookup"><span data-stu-id="2f42f-105">Learn how to use Apache Storm in HDInsight using the storm-starter examples.</span></span>

<span data-ttu-id="2f42f-106">Apache Storm je škálovatelný výpočetní systém v reálném čase odolný proti chybám, distribuovaný určený pro zpracování datových proudů.</span><span class="sxs-lookup"><span data-stu-id="2f42f-106">Apache Storm is a scalable, fault-tolerant, distributed, real-time computation system for processing streams of data.</span></span> <span data-ttu-id="2f42f-107">Pomocí Storm v Azure HDInsight můžete vytvořit cloudový cluster Storm, který bude provádět analýzy velkých objemů dat v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="2f42f-107">With Storm on Azure HDInsight, you can create a cloud-based Storm cluster that performs big data analytics in real time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2f42f-108">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="2f42f-108">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="2f42f-109">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="2f42f-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2f42f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2f42f-110">Prerequisites</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* <span data-ttu-id="2f42f-111">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="2f42f-111">**An Azure subscription**.</span></span> <span data-ttu-id="2f42f-112">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="2f42f-112">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="2f42f-113">**Znalost SSH a SCP**.</span><span class="sxs-lookup"><span data-stu-id="2f42f-113">**Familiarity with SSH and SCP**.</span></span> <span data-ttu-id="2f42f-114">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="2f42f-114">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="create-a-storm-cluster"></a><span data-ttu-id="2f42f-115">Vytvoření clusteru Storm</span><span class="sxs-lookup"><span data-stu-id="2f42f-115">Create a Storm cluster</span></span>

<span data-ttu-id="2f42f-116">Pomocí následujících kroků můžete vytvořit Storm na clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="2f42f-116">Use the following steps to create a Storm on HDInsight cluster:</span></span>

1. <span data-ttu-id="2f42f-117">Na webu [Azure Portal](https://portal.azure.com) vyberte **+ NOVÉ**, **Inteligentní funkce a analýzy** a pak **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="2f42f-117">From the [Azure portal](https://portal.azure.com), select **+ NEW**, **Intelligence + Analytics**, and then select **HDInsight**.</span></span>

    ![Vytvoření clusteru HDInsight](./media/hdinsight-apache-storm-tutorial-get-started-linux/create-hdinsight.png)

2. <span data-ttu-id="2f42f-119">V okně **Základy** zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="2f42f-119">From the **Basics** blade, enter the following information:</span></span>

    * <span data-ttu-id="2f42f-120">**Název clusteru:** Název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2f42f-120">**Cluster Name**: The name of the HDInsight cluster.</span></span>
    * <span data-ttu-id="2f42f-121">**Předplatné:** Vyberte předplatné, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="2f42f-121">**Subscription**: Select the subscription to use.</span></span>
    * <span data-ttu-id="2f42f-122">**Uživatelské jméno přihlášení clusteru** a **Heslo přihlášení clusteru**: Přihlašovací údaje pro přístup ke clusteru pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2f42f-122">**Cluster login username** and **Cluster login password**: The login when accessing the cluster over HTTPS.</span></span> <span data-ttu-id="2f42f-123">Tyto přihlašovací údaje se používají i pro přístup ke službám, jako jsou webové uživatelské rozhraní Ambari nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="2f42f-123">You use these credentials to access services such as the Ambari Web UI or REST API.</span></span>
    * <span data-ttu-id="2f42f-124">**Uživatelské jméno Secure Shell (SSH:)** Přihlašovací údaje používané pro přístup ke clusteru přes SSH.</span><span class="sxs-lookup"><span data-stu-id="2f42f-124">**Secure Shell (SSH) username**: The login used when accessing the cluster over SSH.</span></span> <span data-ttu-id="2f42f-125">Ve výchozím nastavení je heslo stejné jako pro přihlášení ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="2f42f-125">By default the password is the same as the cluster login password.</span></span>
    * <span data-ttu-id="2f42f-126">**Skupina prostředků:** Skupina prostředků, ve které se cluster vytváří.</span><span class="sxs-lookup"><span data-stu-id="2f42f-126">**Resource Group**: The resource group to create the cluster in.</span></span>
    * <span data-ttu-id="2f42f-127">**Umístění:** Oblast Azure, ve které se cluster vytváří.</span><span class="sxs-lookup"><span data-stu-id="2f42f-127">**Location**: The Azure region to create the cluster in.</span></span>

    ![Výběr předplatného](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-basic-configuration.png)

3. <span data-ttu-id="2f42f-129">Vyberte **Typ clusteru** a pak v okně **Konfigurace clusteru** zadejte tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="2f42f-129">Select **Cluster type**, and then set the following values on the **Cluster configuration** blade:</span></span>

    * <span data-ttu-id="2f42f-130">**Typ clusteru:** Storm</span><span class="sxs-lookup"><span data-stu-id="2f42f-130">**Cluster Type**: Storm</span></span>

    * <span data-ttu-id="2f42f-131">**Operační systém:** Linux</span><span class="sxs-lookup"><span data-stu-id="2f42f-131">**Operating system**: Linux</span></span>

    * <span data-ttu-id="2f42f-132">**Verze:** Storm 1.1.0 (HDI 3.6)</span><span class="sxs-lookup"><span data-stu-id="2f42f-132">**Version**: Storm 1.1.0 (HDI 3.6)</span></span>

    * <span data-ttu-id="2f42f-133">**Úroveň clusteru:** Standard</span><span class="sxs-lookup"><span data-stu-id="2f42f-133">**Cluster Tier**: Standard</span></span>

    <span data-ttu-id="2f42f-134">Nakonec uložte nastavení tlačítkem **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="2f42f-134">Finally, use the **Select** button to save settings.</span></span>

    ![Výběr typu clusteru](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-cluster-type.png)

4. <span data-ttu-id="2f42f-136">Po výběru typu clusteru použijte tlačítko __Vybrat__ k výběru typu clusteru.</span><span class="sxs-lookup"><span data-stu-id="2f42f-136">After selecting the cluster type, use the __Select__ button to set the cluster type.</span></span> <span data-ttu-id="2f42f-137">Dále stisknutím tlačítka __Další__ dokončete základní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2f42f-137">Next, use the __Next__ button to finish basic configuration.</span></span>

5. <span data-ttu-id="2f42f-138">V okně **Úložiště** vyberte nebo vytvořte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="2f42f-138">From the **Storage** blade, select or create a Storage account.</span></span> <span data-ttu-id="2f42f-139">Pro ukázkový postup v tomto dokumentu ponechte všechna ostatní pole v tomto okně na výchozích hodnotách.</span><span class="sxs-lookup"><span data-stu-id="2f42f-139">For the steps in this document, leave the other fields on this blade at the default values.</span></span> <span data-ttu-id="2f42f-140">Stisknutím tlačítka __Další__ uložte konfiguraci úložiště.</span><span class="sxs-lookup"><span data-stu-id="2f42f-140">Use the __Next__ button to save storage configuration.</span></span>

    ![Nastavení účtu úložiště pro HDInsight](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-storage-account.png)

6. <span data-ttu-id="2f42f-142">V okně **Souhrn** zkontrolujte konfiguraci clusteru.</span><span class="sxs-lookup"><span data-stu-id="2f42f-142">From the **Summary** blade, review the configuration for the cluster.</span></span> <span data-ttu-id="2f42f-143">Pomocí odkazů __Upravit__ opravte případná chybná nastavení.</span><span class="sxs-lookup"><span data-stu-id="2f42f-143">Use the __Edit__ links to change any settings that are incorrect.</span></span> <span data-ttu-id="2f42f-144">Nakonec stisknutím tlačítka Vytvořit cluster vytvořte.</span><span class="sxs-lookup"><span data-stu-id="2f42f-144">Finally, use the__Create__ button to create the cluster.</span></span>

    ![Souhrn konfigurace clusteru](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-configuration-summary.png)

    > [!NOTE]
    > <span data-ttu-id="2f42f-146">Vytvoření clusteru trvá přibližně 20 minut.</span><span class="sxs-lookup"><span data-stu-id="2f42f-146">It can take up to 20 minutes to create the cluster.</span></span>

## <a name="run-a-storm-starter-sample-on-hdinsight"></a><span data-ttu-id="2f42f-147">Spuštění ukázky topologie Storm Starter v HDInsight</span><span class="sxs-lookup"><span data-stu-id="2f42f-147">Run a storm-starter sample on HDInsight</span></span>

1. <span data-ttu-id="2f42f-148">Připojte se ke clusteru HDInsight pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="2f42f-148">Connect to the HDInsight cluster using SSH:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    <span data-ttu-id="2f42f-149">Pokud jste použili heslo k zabezpečení uživatelského účtu SSH, zobrazí se výzva k jeho zadání.</span><span class="sxs-lookup"><span data-stu-id="2f42f-149">If you used a password to secure your SSH user account, you are prompted to enter it.</span></span> <span data-ttu-id="2f42f-150">Pokud jste použili veřejný klíč, bude pravděpodobně muset použít parametr `-i` k určení odpovídajícího privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="2f42f-150">If you used a public key, you may need use the `-i` parameter to specify the matching private key.</span></span> <span data-ttu-id="2f42f-151">Například, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="2f42f-151">For example, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span></span>

    <span data-ttu-id="2f42f-152">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="2f42f-152">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="2f42f-153">Následující příkaz použijte ke spuštění ukázkové topologie:</span><span class="sxs-lookup"><span data-stu-id="2f42f-153">Use the following command to start an example topology:</span></span>

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount

    > [!NOTE]
    > <span data-ttu-id="2f42f-154">V dřívějších verzích služby HDInsight je název třídy topologie `storm.starter.WordCountTopology` místo `org.apache.storm.starter.WordCountTopology`.</span><span class="sxs-lookup"><span data-stu-id="2f42f-154">On previous versions of HDInsight, the class name of the topology is `storm.starter.WordCountTopology` instead of `org.apache.storm.starter.WordCountTopology`.</span></span>

    <span data-ttu-id="2f42f-155">Tento příkaz spustí ukázkovou topologii WordCount v clusteru s popisným názvem „wordcount“.</span><span class="sxs-lookup"><span data-stu-id="2f42f-155">This command starts the example WordCount topology on the cluster, with a friendly name of 'wordcount'.</span></span> <span data-ttu-id="2f42f-156">Bude náhodně generovat věty a počítat výskyt jednotlivých slov v těchto větách.</span><span class="sxs-lookup"><span data-stu-id="2f42f-156">It randomly generates sentences and count the occurrence of each word in the sentences.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2f42f-157">Při odesílání vlastních topologií do clusteru je před použitím příkazu `storm` nutné nejprve zkopírovat soubor jar obsahující cluster.</span><span class="sxs-lookup"><span data-stu-id="2f42f-157">When submitting your own topologies to the cluster, you must first copy the jar file containing the cluster before using the `storm` command.</span></span> <span data-ttu-id="2f42f-158">Ke zkopírování tohoto souboru použijte příkaz `scp`.</span><span class="sxs-lookup"><span data-stu-id="2f42f-158">Use the `scp` command to copy the file.</span></span> <span data-ttu-id="2f42f-159">Například `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`.</span><span class="sxs-lookup"><span data-stu-id="2f42f-159">For example, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span></span>
    >
    > <span data-ttu-id="2f42f-160">Příklad WordCount a další příklady topologie Storm Starter jsou již zahrnuty v clusteru na `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span><span class="sxs-lookup"><span data-stu-id="2f42f-160">The WordCount example, and other storm-starter examples, are already included on your cluster at `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span></span>

<span data-ttu-id="2f42f-161">Pokud si chcete prohlédnout zdrojové kódy příkladů topologie Storm Starter, najdete je na [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter).</span><span class="sxs-lookup"><span data-stu-id="2f42f-161">If you are interested in viewing the source for the storm-starter examples, you can find the code at [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter).</span></span> <span data-ttu-id="2f42f-162">Tento odkaz je pro Storm 1.1.x, který je součástí služby HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="2f42f-162">This link is for Storm 1.1.x, which is provided with HDInsight 3.6.</span></span> <span data-ttu-id="2f42f-163">Pro ostatní verze Stormu použijte tlačítko __Větev__ v horní části stránky a vyberte jinou verzi Stormu.</span><span class="sxs-lookup"><span data-stu-id="2f42f-163">For other versions of Storm, use the __Branch__ button at the top of the page to select a different Storm version.</span></span>

## <a name="monitor-the-topology"></a><span data-ttu-id="2f42f-164">Monitorování topologie</span><span class="sxs-lookup"><span data-stu-id="2f42f-164">Monitor the topology</span></span>

<span data-ttu-id="2f42f-165">Uživatelské rozhraní Storm poskytuje webové rozhraní pro práci se spuštěnými topologiemi a je součástí clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2f42f-165">The Storm UI provides a web interface for working with running topologies, and is included on your HDInsight cluster.</span></span>

<span data-ttu-id="2f42f-166">Ke sledování topologie pomocí uživatelského rozhraní Storm použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2f42f-166">Use the following steps to monitor the topology using the Storm UI:</span></span>

1. <span data-ttu-id="2f42f-167">Pokud chcete zobrazit uživatelské rozhraní Storm, ve webovém prohlížeči otevřete stránku https://CLUSTERNAME.azurehdinsight.net/stormui.</span><span class="sxs-lookup"><span data-stu-id="2f42f-167">To display the Storm UI, open a web browser to https://CLUSTERNAME.azurehdinsight.net/stormui.</span></span> <span data-ttu-id="2f42f-168">Nahraďte **CLUSTERNAME** názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="2f42f-168">Replace **CLUSTERNAME** with the name of your cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2f42f-169">Pokud budete vyzváni k zadání uživatelského jména a hesla, zadejte správce clusteru (admin) a heslo použité při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="2f42f-169">If asked to provide a user name and password, enter the cluster administrator (admin) and password that you used when creating the cluster.</span></span>

2. <span data-ttu-id="2f42f-170">V části **Souhrn topologie**, vyberte položku **wordcount** ve sloupci **Název**.</span><span class="sxs-lookup"><span data-stu-id="2f42f-170">Under **Topology summary**, select the **wordcount** entry in the **Name** column.</span></span> <span data-ttu-id="2f42f-171">Zobrazí se další informace o topologii.</span><span class="sxs-lookup"><span data-stu-id="2f42f-171">Information about the topology is displayed.</span></span>

    ![Řídicí panel Storm s informacemi o topologii Storm Starter WordCount.](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)

    <span data-ttu-id="2f42f-173">Tato stránka přináší následující informace:</span><span class="sxs-lookup"><span data-stu-id="2f42f-173">This page provides the following information:</span></span>

    * <span data-ttu-id="2f42f-174">**Statistiky topologie** – základní informace o výkonu topologie uspořádané do časových oken.</span><span class="sxs-lookup"><span data-stu-id="2f42f-174">**Topology stats** - Basic information on the topology performance, organized into time windows.</span></span>

        > [!NOTE]
        > <span data-ttu-id="2f42f-175">Výběrem konkrétního časového okna změníte časové okno informací zobrazených v dalších částech stránky.</span><span class="sxs-lookup"><span data-stu-id="2f42f-175">Selecting a specific time window changes the time window for information displayed in other sections of the page.</span></span>

    * <span data-ttu-id="2f42f-176">**Funkce Spouts** – základní informace o funkcích spouts, včetně poslední chyby vrácené každou funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="2f42f-176">**Spouts** - Basic information about spouts, including the last error returned by each spout.</span></span>

    * <span data-ttu-id="2f42f-177">**Funkce Bolts** – základní informace o funkcích bolts.</span><span class="sxs-lookup"><span data-stu-id="2f42f-177">**Bolts** - Basic information about bolts.</span></span>

    * <span data-ttu-id="2f42f-178">**Topologie konfigurace** – podrobné informace o konfiguraci topologie.</span><span class="sxs-lookup"><span data-stu-id="2f42f-178">**Topology configuration** - Detailed information about the topology configuration.</span></span>

    <span data-ttu-id="2f42f-179">Tato stránka také obsahuje akce, které můžete provést na topologii:</span><span class="sxs-lookup"><span data-stu-id="2f42f-179">This page also provides actions that can be taken on the topology:</span></span>

    * <span data-ttu-id="2f42f-180">**Aktivovat** – obnoví zpracování deaktivované topologie.</span><span class="sxs-lookup"><span data-stu-id="2f42f-180">**Activate** - Resumes processing of a deactivated topology.</span></span>

    * <span data-ttu-id="2f42f-181">**Deaktivovat** – pozastaví spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="2f42f-181">**Deactivate** - Pauses a running topology.</span></span>

    * <span data-ttu-id="2f42f-182">**Znovu vyvážit** – upraví paralelismus topologii.</span><span class="sxs-lookup"><span data-stu-id="2f42f-182">**Rebalance** - Adjusts the parallelism of the topology.</span></span> <span data-ttu-id="2f42f-183">Po změně počtu uzlů v clusteru musíte znovu vyvážit spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="2f42f-183">You should rebalance running topologies after you have changed the number of nodes in the cluster.</span></span> <span data-ttu-id="2f42f-184">Nové vyvážení upraví paralelismus, aby se vykompenzovalo zvýšení nebo snížení počtu uzlů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="2f42f-184">Rebalancing adjusts parallelism to compensate for the increased/decreased number of nodes in the cluster.</span></span> <span data-ttu-id="2f42f-185">Další informace naleznete v části [Pochopení paralelismu topologie Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span><span class="sxs-lookup"><span data-stu-id="2f42f-185">For more information, see [Understanding the parallelism of a Storm topology](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span></span>

    * <span data-ttu-id="2f42f-186">**Ukončit** – ukončí topologii Storm po zadaném časovém limitu.</span><span class="sxs-lookup"><span data-stu-id="2f42f-186">**Kill** - Terminates a Storm topology after the specified timeout.</span></span>

3. <span data-ttu-id="2f42f-187">Na této stránce vyberte položku z oddílu **Spouts** nebo **Bolts**.</span><span class="sxs-lookup"><span data-stu-id="2f42f-187">From this page, select an entry from the **Spouts** or **Bolts** section.</span></span> <span data-ttu-id="2f42f-188">Zobrazí se informace o vybrané komponentě.</span><span class="sxs-lookup"><span data-stu-id="2f42f-188">Information about the selected component is displayed.</span></span>

    ![Řídicí panel Storm s informacemi o vybraných součástech.](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)

    <span data-ttu-id="2f42f-190">Tato stránka obsahuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="2f42f-190">This page displays the following information:</span></span>

    * <span data-ttu-id="2f42f-191">**Statistiky funkcí Spout/Bolt** – základní informace o výkonu komponenty uspořádané do časových oken.</span><span class="sxs-lookup"><span data-stu-id="2f42f-191">**Spout/Bolt stats** - Basic information on the component performance, organized into time windows.</span></span>

        > [!NOTE]
        > <span data-ttu-id="2f42f-192">Výběrem konkrétního časového okna změníte časové okno informací zobrazených v dalších částech stránky.</span><span class="sxs-lookup"><span data-stu-id="2f42f-192">Selecting a specific time window changes the time window for information displayed in other sections of the page.</span></span>

    * <span data-ttu-id="2f42f-193">**Statistiky vstupu** (pouze funkce bolt) – informace o komponentech, které vytváří dat využívaná funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="2f42f-193">**Input stats** (bolt only) - Information on components that produce data consumed by the bolt.</span></span>

    * <span data-ttu-id="2f42f-194">**Statististiky výstupu** – informace o datech vysílaných touto funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="2f42f-194">**Output stats** - Information on data emitted by this bolt.</span></span>

    * <span data-ttu-id="2f42f-195">**Prováděcí moduly** – informace o instancích této komponenty.</span><span class="sxs-lookup"><span data-stu-id="2f42f-195">**Executors** - Information on instances of this component.</span></span>

    * <span data-ttu-id="2f42f-196">**Chyby** – chyby vytvořené touto komponentou.</span><span class="sxs-lookup"><span data-stu-id="2f42f-196">**Errors** - Errors produced by this component.</span></span>

4. <span data-ttu-id="2f42f-197">Chcete-li zobrazit podrobnosti pro konkrétní instanci komponenty, při zobrazení podrobností o funkcích spout nebo bolt vyberte položku ze sloupce **Port** v oddílu **Vykonavatelé**.</span><span class="sxs-lookup"><span data-stu-id="2f42f-197">When viewing the details of a spout or bolt, select an entry from the **Port** column in the **Executors** section to view details for a specific instance of the component.</span></span>

        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]

    <span data-ttu-id="2f42f-198">V tomto příkladu se slovo **seven** vyskytlo 1493957krát.</span><span class="sxs-lookup"><span data-stu-id="2f42f-198">In this example, the word **seven** has occurred 1493957 times.</span></span> <span data-ttu-id="2f42f-199">Tolikrát bylo toto slovo zjištěno od spuštění této topologie.</span><span class="sxs-lookup"><span data-stu-id="2f42f-199">This count is how many times the word has been encountered since this topology was started.</span></span>

## <a name="stop-the-topology"></a><span data-ttu-id="2f42f-200">Zastavení topologie</span><span class="sxs-lookup"><span data-stu-id="2f42f-200">Stop the topology</span></span>

<span data-ttu-id="2f42f-201">Vraťte se na stránku **Souhrn topologie**, kde naleznete topologii počtu slov a pak vyberte tlačítko **Zastavit** z oddílu **Topologie akce**.</span><span class="sxs-lookup"><span data-stu-id="2f42f-201">Return to the **Topology summary** page for the word-count topology, and then select the **Kill** button from the **Topology actions** section.</span></span> <span data-ttu-id="2f42f-202">Po zobrazení výzvy zadejte hodnotu 10 jako počet sekund, po které se má počkat před zastavením topologie.</span><span class="sxs-lookup"><span data-stu-id="2f42f-202">When prompted, enter 10 for the seconds to wait before stopping the topology.</span></span> <span data-ttu-id="2f42f-203">Po uplynutí časového limitu se topologie už při návštěvě oddílu **Uživatelské rozhraní Storm** řídicího panelu nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="2f42f-203">After the timeout period, the topology no longer appears when you visit the **Storm UI** section of the dashboard.</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="2f42f-204">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="2f42f-204">Delete the cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="2f42f-205">Pokud narazíte na problém s vytvářením clusteru HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="2f42f-205">If you run into an issue with creating HDInsight cluster, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <span data-ttu-id="2f42f-206"><a id="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="2f42f-206"><a id="next"></a>Next steps</span></span>

<span data-ttu-id="2f42f-207">V tomto kurzu Apache Storm jste se naučili základy práce se Stormem v HDInsightu.</span><span class="sxs-lookup"><span data-stu-id="2f42f-207">In this Apache Storm tutorial, you learned the basics of working with Storm on HDInsight.</span></span> <span data-ttu-id="2f42f-208">Dále se naučíte, jak [Vyvíjet topologie založené na jazyce Java pomocí nástroje Maven](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="2f42f-208">Next, learn how to [Develop Java-based topologies using Maven](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="2f42f-209">Pokud jste již obeznámeni s vývojem topologií založených na jazyce Java a chcete nasadit existující topologie do HDInsight, naleznete postup v části [Nasazení a správa topologií Apache Storm v HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).</span><span class="sxs-lookup"><span data-stu-id="2f42f-209">If you're already familiar with developing Java-based topologies and want to deploy an existing topology to HDInsight, see [Deploy and manage Apache Storm topologies on HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).</span></span>

<span data-ttu-id="2f42f-210">Pokud jste vývojář .NET, můžete s použitím sady Visual Studio vytvořit topologie C# nebo hybridní topologie C#/Java.</span><span class="sxs-lookup"><span data-stu-id="2f42f-210">If you are a .NET developer, you can create C# or hybrid C#/Java topologies using Visual Studio.</span></span> <span data-ttu-id="2f42f-211">Další informace najdete v tématu [Vývoj topologií C# pro Apache Storm ve službě HDInsight pomocí nástrojů Hadoop pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="2f42f-211">For more information, see [Develop C# topologies for Apache Storm on HDInsight using Hadoop tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

<span data-ttu-id="2f42f-212">Příklady topologií, které se dají použít se systémem Storm ve službě HDInsight:</span><span class="sxs-lookup"><span data-stu-id="2f42f-212">For example topologies that can be used with Storm on HDInsight, see the following examples:</span></span>

* [<span data-ttu-id="2f42f-213">Příklad topologií pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="2f42f-213">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[preview-portal]: https://portal.azure.com/
