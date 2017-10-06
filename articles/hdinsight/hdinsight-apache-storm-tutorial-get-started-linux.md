---
title: "Příklady aaaStorm starter na Apache Storm v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toodo analýzy velkých objemů dat a zpracování dat v reálném čase pomocí Apache Storm a hello počáteční příklady storm v HDInsight."
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
ms.openlocfilehash: bb6d6549e67ca5b557f0692f98c89692a87267b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#<a name="get-started-with-apache-storm-on-hdinsight-using-hello-storm-starter-examples"></a><span data-ttu-id="94a97-104">Začínáme s Apache Storm v HDInsight pomocí hello počáteční příklady storm</span><span class="sxs-lookup"><span data-stu-id="94a97-104">Get started with Apache Storm on HDInsight using hello storm-starter examples</span></span>

<span data-ttu-id="94a97-105">Zjistěte, jak toouse Apache Storm v HDInsight pomocí hello počáteční příklady storm.</span><span class="sxs-lookup"><span data-stu-id="94a97-105">Learn how toouse Apache Storm in HDInsight using hello storm-starter examples.</span></span>

<span data-ttu-id="94a97-106">Apache Storm je škálovatelný výpočetní systém v reálném čase odolný proti chybám, distribuovaný určený pro zpracování datových proudů.</span><span class="sxs-lookup"><span data-stu-id="94a97-106">Apache Storm is a scalable, fault-tolerant, distributed, real-time computation system for processing streams of data.</span></span> <span data-ttu-id="94a97-107">Pomocí Storm v Azure HDInsight můžete vytvořit cloudový cluster Storm, který bude provádět analýzy velkých objemů dat v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="94a97-107">With Storm on Azure HDInsight, you can create a cloud-based Storm cluster that performs big data analytics in real time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="94a97-108">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="94a97-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="94a97-109">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="94a97-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="94a97-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="94a97-110">Prerequisites</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* <span data-ttu-id="94a97-111">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="94a97-111">**An Azure subscription**.</span></span> <span data-ttu-id="94a97-112">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="94a97-112">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="94a97-113">**Znalost SSH a SCP**.</span><span class="sxs-lookup"><span data-stu-id="94a97-113">**Familiarity with SSH and SCP**.</span></span> <span data-ttu-id="94a97-114">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="94a97-114">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="create-a-storm-cluster"></a><span data-ttu-id="94a97-115">Vytvoření clusteru Storm</span><span class="sxs-lookup"><span data-stu-id="94a97-115">Create a Storm cluster</span></span>

<span data-ttu-id="94a97-116">Použijte následující postup toocreate Storm v clusteru HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="94a97-116">Use hello following steps toocreate a Storm on HDInsight cluster:</span></span>

1. <span data-ttu-id="94a97-117">Z hello [portál Azure](https://portal.azure.com), vyberte **+ nový**, **Intelligence + analýzy**a potom vyberte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="94a97-117">From hello [Azure portal](https://portal.azure.com), select **+ NEW**, **Intelligence + Analytics**, and then select **HDInsight**.</span></span>

    ![Vytvoření clusteru HDInsight](./media/hdinsight-apache-storm-tutorial-get-started-linux/create-hdinsight.png)

2. <span data-ttu-id="94a97-119">Z hello **Základy** okno, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="94a97-119">From hello **Basics** blade, enter hello following information:</span></span>

    * <span data-ttu-id="94a97-120">**Název clusteru**: název hello hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="94a97-120">**Cluster Name**: hello name of hello HDInsight cluster.</span></span>
    * <span data-ttu-id="94a97-121">**Předplatné**: Vyberte předplatné toouse hello.</span><span class="sxs-lookup"><span data-stu-id="94a97-121">**Subscription**: Select hello subscription toouse.</span></span>
    * <span data-ttu-id="94a97-122">**Uživatelské jméno přihlášení clusteru** a **clusteru přihlašovacího hesla**: hello přihlášení při přístupu k hello clusteru přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="94a97-122">**Cluster login username** and **Cluster login password**: hello login when accessing hello cluster over HTTPS.</span></span> <span data-ttu-id="94a97-123">Tyto přihlašovací údaje služby tooaccess například hello webové uživatelské rozhraní Ambari nebo REST API používáte.</span><span class="sxs-lookup"><span data-stu-id="94a97-123">You use these credentials tooaccess services such as hello Ambari Web UI or REST API.</span></span>
    * <span data-ttu-id="94a97-124">**Secure Shell (SSH) username**: hello přihlášení, které se používá při získávání hello clusteru prostřednictvím SSH.</span><span class="sxs-lookup"><span data-stu-id="94a97-124">**Secure Shell (SSH) username**: hello login used when accessing hello cluster over SSH.</span></span> <span data-ttu-id="94a97-125">Ve výchozím nastavení je hello hello heslo stejné jako heslo pro přihlášení hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="94a97-125">By default hello password is hello same as hello cluster login password.</span></span>
    * <span data-ttu-id="94a97-126">**Skupina prostředků**: hello prostředků skupiny toocreate hello clusteru v.</span><span class="sxs-lookup"><span data-stu-id="94a97-126">**Resource Group**: hello resource group toocreate hello cluster in.</span></span>
    * <span data-ttu-id="94a97-127">**Umístění**: hello oblast Azure toocreate hello clusteru v.</span><span class="sxs-lookup"><span data-stu-id="94a97-127">**Location**: hello Azure region toocreate hello cluster in.</span></span>

    ![Výběr předplatného](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-basic-configuration.png)

3. <span data-ttu-id="94a97-129">Vyberte **clusteru typ**, a pak sadu hello následující hodnoty na hello **konfigurace clusteru** okno:</span><span class="sxs-lookup"><span data-stu-id="94a97-129">Select **Cluster type**, and then set hello following values on hello **Cluster configuration** blade:</span></span>

    * <span data-ttu-id="94a97-130">**Typ clusteru:** Storm</span><span class="sxs-lookup"><span data-stu-id="94a97-130">**Cluster Type**: Storm</span></span>

    * <span data-ttu-id="94a97-131">**Operační systém:** Linux</span><span class="sxs-lookup"><span data-stu-id="94a97-131">**Operating system**: Linux</span></span>

    * <span data-ttu-id="94a97-132">**Verze:** Storm 1.1.0 (HDI 3.6)</span><span class="sxs-lookup"><span data-stu-id="94a97-132">**Version**: Storm 1.1.0 (HDI 3.6)</span></span>

    * <span data-ttu-id="94a97-133">**Úroveň clusteru:** Standard</span><span class="sxs-lookup"><span data-stu-id="94a97-133">**Cluster Tier**: Standard</span></span>

    <span data-ttu-id="94a97-134">Nakonec použijte hello **vyberte** tlačítko toosave nastavení.</span><span class="sxs-lookup"><span data-stu-id="94a97-134">Finally, use hello **Select** button toosave settings.</span></span>

    ![Výběr typu clusteru](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-cluster-type.png)

4. <span data-ttu-id="94a97-136">Po výběru typu hello clusteru, použijte hello __vyberte__ tooset hello clusteru typ tlačítka.</span><span class="sxs-lookup"><span data-stu-id="94a97-136">After selecting hello cluster type, use hello __Select__ button tooset hello cluster type.</span></span> <span data-ttu-id="94a97-137">Pak pomocí hello __Další__ tlačítko toofinish základní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="94a97-137">Next, use hello __Next__ button toofinish basic configuration.</span></span>

5. <span data-ttu-id="94a97-138">Z hello **úložiště** okně vyberte nebo vytvořte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="94a97-138">From hello **Storage** blade, select or create a Storage account.</span></span> <span data-ttu-id="94a97-139">Hello kroky v tomto dokumentu ponechte hello další pole v tomto okně na hello výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="94a97-139">For hello steps in this document, leave hello other fields on this blade at hello default values.</span></span> <span data-ttu-id="94a97-140">Použití hello __Další__ tlačítko toosave úložiště konfigurace.</span><span class="sxs-lookup"><span data-stu-id="94a97-140">Use hello __Next__ button toosave storage configuration.</span></span>

    ![Nastavit hello nastavení účtu úložiště pro HDInsight](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-storage-account.png)

6. <span data-ttu-id="94a97-142">Z hello **Souhrn** okno, zkontrolujte konfiguraci hello pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="94a97-142">From hello **Summary** blade, review hello configuration for hello cluster.</span></span> <span data-ttu-id="94a97-143">Použití hello __upravit__ odkazy toochange všechna nastavení, která jsou nesprávné.</span><span class="sxs-lookup"><span data-stu-id="94a97-143">Use hello __Edit__ links toochange any settings that are incorrect.</span></span> <span data-ttu-id="94a97-144">Nakonec použijte the__Create__ tlačítko toocreate hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="94a97-144">Finally, use the__Create__ button toocreate hello cluster.</span></span>

    ![Souhrn konfigurace clusteru](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-configuration-summary.png)

    > [!NOTE]
    > <span data-ttu-id="94a97-146">To může trvat až too20 minut toocreate hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="94a97-146">It can take up too20 minutes toocreate hello cluster.</span></span>

## <a name="run-a-storm-starter-sample-on-hdinsight"></a><span data-ttu-id="94a97-147">Spuštění ukázky topologie Storm Starter v HDInsight</span><span class="sxs-lookup"><span data-stu-id="94a97-147">Run a storm-starter sample on HDInsight</span></span>

1. <span data-ttu-id="94a97-148">Připojte toohello clusteru HDInsight pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="94a97-148">Connect toohello HDInsight cluster using SSH:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    <span data-ttu-id="94a97-149">Pokud jste použili toosecure hesla účtu uživatele SSH, jste výzvami tooenter ho.</span><span class="sxs-lookup"><span data-stu-id="94a97-149">If you used a password toosecure your SSH user account, you are prompted tooenter it.</span></span> <span data-ttu-id="94a97-150">Pokud jste použili veřejný klíč, můžete potřebovat použít hello `-i` parametr toospecify hello odpovídající soukromý klíč.</span><span class="sxs-lookup"><span data-stu-id="94a97-150">If you used a public key, you may need use hello `-i` parameter toospecify hello matching private key.</span></span> <span data-ttu-id="94a97-151">Například, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="94a97-151">For example, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span></span>

    <span data-ttu-id="94a97-152">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="94a97-152">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="94a97-153">Použijte následující příkaz toostart ukázkové topologie hello:</span><span class="sxs-lookup"><span data-stu-id="94a97-153">Use hello following command toostart an example topology:</span></span>

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount

    > [!NOTE]
    > <span data-ttu-id="94a97-154">V dřívějších verzích systému HDInsight, název třídy hello hello topologie je `storm.starter.WordCountTopology` místo `org.apache.storm.starter.WordCountTopology`.</span><span class="sxs-lookup"><span data-stu-id="94a97-154">On previous versions of HDInsight, hello class name of hello topology is `storm.starter.WordCountTopology` instead of `org.apache.storm.starter.WordCountTopology`.</span></span>

    <span data-ttu-id="94a97-155">Tento příkaz spustí hello ukázkovou topologii WordCount v clusteru hello s popisným názvem "WordCount".</span><span class="sxs-lookup"><span data-stu-id="94a97-155">This command starts hello example WordCount topology on hello cluster, with a friendly name of 'wordcount'.</span></span> <span data-ttu-id="94a97-156">Generuje náhodně věty a počet hello výskytem jednotlivých slov v hello věty.</span><span class="sxs-lookup"><span data-stu-id="94a97-156">It randomly generates sentences and count hello occurrence of each word in hello sentences.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94a97-157">Při odesílání vlastní topologie toohello cluster, je nutné nejprve zkopírovat soubor jar hello obsahující hello cluster před použitím hello `storm` příkaz.</span><span class="sxs-lookup"><span data-stu-id="94a97-157">When submitting your own topologies toohello cluster, you must first copy hello jar file containing hello cluster before using hello `storm` command.</span></span> <span data-ttu-id="94a97-158">Použití hello `scp` příkaz toocopy hello souboru.</span><span class="sxs-lookup"><span data-stu-id="94a97-158">Use hello `scp` command toocopy hello file.</span></span> <span data-ttu-id="94a97-159">Například `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`.</span><span class="sxs-lookup"><span data-stu-id="94a97-159">For example, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span></span>
    >
    > <span data-ttu-id="94a97-160">Příklad WordCount Hello a další příklady storm starter jsou již zahrnuty v clusteru na `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span><span class="sxs-lookup"><span data-stu-id="94a97-160">hello WordCount example, and other storm-starter examples, are already included on your cluster at `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span></span>

<span data-ttu-id="94a97-161">Pokud vás zajímá v zobrazení zdroje hello hello příklady storm-starter, můžete najít hello kód na [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter).</span><span class="sxs-lookup"><span data-stu-id="94a97-161">If you are interested in viewing hello source for hello storm-starter examples, you can find hello code at [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter).</span></span> <span data-ttu-id="94a97-162">Tento odkaz je pro Storm 1.1.x, který je součástí služby HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="94a97-162">This link is for Storm 1.1.x, which is provided with HDInsight 3.6.</span></span> <span data-ttu-id="94a97-163">U ostatních verzí Storm použít hello __větve__ tlačítko hello horní části stránky tooselect hello jinou verzi Storm.</span><span class="sxs-lookup"><span data-stu-id="94a97-163">For other versions of Storm, use hello __Branch__ button at hello top of hello page tooselect a different Storm version.</span></span>

## <a name="monitor-hello-topology"></a><span data-ttu-id="94a97-164">Monitorování hello topologie</span><span class="sxs-lookup"><span data-stu-id="94a97-164">Monitor hello topology</span></span>

<span data-ttu-id="94a97-165">Hello uživatelské rozhraní Storm poskytuje webové rozhraní pro práci se spuštěnými topologiemi a je součástí clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="94a97-165">hello Storm UI provides a web interface for working with running topologies, and is included on your HDInsight cluster.</span></span>

<span data-ttu-id="94a97-166">Použijte následující postup toomonitor hello topologie pomocí hello uživatelské rozhraní Storm hello:</span><span class="sxs-lookup"><span data-stu-id="94a97-166">Use hello following steps toomonitor hello topology using hello Storm UI:</span></span>

1. <span data-ttu-id="94a97-167">hello toodisplay uživatelské rozhraní Storm, otevřete toohttps://CLUSTERNAME.azurehdinsight.net/stormui webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="94a97-167">toodisplay hello Storm UI, open a web browser toohttps://CLUSTERNAME.azurehdinsight.net/stormui.</span></span> <span data-ttu-id="94a97-168">Nahraďte **CLUSTERNAME** s hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="94a97-168">Replace **CLUSTERNAME** with hello name of your cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94a97-169">Pokud se zobrazí dotaz tooprovide uživatelské jméno a heslo, zadejte Správce clusteru hello (správce) a heslo použité při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="94a97-169">If asked tooprovide a user name and password, enter hello cluster administrator (admin) and password that you used when creating hello cluster.</span></span>

2. <span data-ttu-id="94a97-170">V části **souhrn topologie**, vyberte hello **wordcount** položku v hello **název** sloupce.</span><span class="sxs-lookup"><span data-stu-id="94a97-170">Under **Topology summary**, select hello **wordcount** entry in hello **Name** column.</span></span> <span data-ttu-id="94a97-171">Zobrazí se informace o topologii hello.</span><span class="sxs-lookup"><span data-stu-id="94a97-171">Information about hello topology is displayed.</span></span>

    ![Řídicí panel Storm s informacemi o topologii Storm Starter WordCount.](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)

    <span data-ttu-id="94a97-173">Tato stránka obsahuje hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="94a97-173">This page provides hello following information:</span></span>

    * <span data-ttu-id="94a97-174">**Statistiky topologie** – základní informace o hello výkonu topologie uspořádané do časových oken.</span><span class="sxs-lookup"><span data-stu-id="94a97-174">**Topology stats** - Basic information on hello topology performance, organized into time windows.</span></span>

        > [!NOTE]
        > <span data-ttu-id="94a97-175">Výběr určité časové okno změny hello časové okno informací zobrazených v dalších částech stránky hello.</span><span class="sxs-lookup"><span data-stu-id="94a97-175">Selecting a specific time window changes hello time window for information displayed in other sections of hello page.</span></span>

    * <span data-ttu-id="94a97-176">**Spouts** – základní informace o funkcích spouts, včetně hello poslední chyby vrácené každou funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="94a97-176">**Spouts** - Basic information about spouts, including hello last error returned by each spout.</span></span>

    * <span data-ttu-id="94a97-177">**Funkce Bolts** – základní informace o funkcích bolts.</span><span class="sxs-lookup"><span data-stu-id="94a97-177">**Bolts** - Basic information about bolts.</span></span>

    * <span data-ttu-id="94a97-178">**Topologie konfigurace** -podrobné informace o konfiguraci topologie hello.</span><span class="sxs-lookup"><span data-stu-id="94a97-178">**Topology configuration** - Detailed information about hello topology configuration.</span></span>

    <span data-ttu-id="94a97-179">Tato stránka také obsahuje akce, které můžete provést na topologii hello:</span><span class="sxs-lookup"><span data-stu-id="94a97-179">This page also provides actions that can be taken on hello topology:</span></span>

    * <span data-ttu-id="94a97-180">**Aktivovat** – obnoví zpracování deaktivované topologie.</span><span class="sxs-lookup"><span data-stu-id="94a97-180">**Activate** - Resumes processing of a deactivated topology.</span></span>

    * <span data-ttu-id="94a97-181">**Deaktivovat** – pozastaví spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="94a97-181">**Deactivate** - Pauses a running topology.</span></span>

    * <span data-ttu-id="94a97-182">**Znovu vyvážit** – upraví paralelismus hello hello topologie.</span><span class="sxs-lookup"><span data-stu-id="94a97-182">**Rebalance** - Adjusts hello parallelism of hello topology.</span></span> <span data-ttu-id="94a97-183">Po změně hello počet uzlů v clusteru hello, znovu vyvážit spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="94a97-183">You should rebalance running topologies after you have changed hello number of nodes in hello cluster.</span></span> <span data-ttu-id="94a97-184">Vyrovnává upraví paralelismus toocompensate pro hello zvýšení nebo snížení počtu uzlů v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="94a97-184">Rebalancing adjusts parallelism toocompensate for hello increased/decreased number of nodes in hello cluster.</span></span> <span data-ttu-id="94a97-185">Další informace najdete v tématu [pochopení paralelismu topologie Storm hello](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span><span class="sxs-lookup"><span data-stu-id="94a97-185">For more information, see [Understanding hello parallelism of a Storm topology](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span></span>

    * <span data-ttu-id="94a97-186">**Příkaz kill** – ukončí topologii Storm po hello zadaný časový limit.</span><span class="sxs-lookup"><span data-stu-id="94a97-186">**Kill** - Terminates a Storm topology after hello specified timeout.</span></span>

3. <span data-ttu-id="94a97-187">Z této stránky, vyberte položku z hello **Spouts** nebo **Bolts** části.</span><span class="sxs-lookup"><span data-stu-id="94a97-187">From this page, select an entry from hello **Spouts** or **Bolts** section.</span></span> <span data-ttu-id="94a97-188">Zobrazí se informace o vybrané součásti hello.</span><span class="sxs-lookup"><span data-stu-id="94a97-188">Information about hello selected component is displayed.</span></span>

    ![Řídicí panel Storm s informacemi o vybraných součástech.](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)

    <span data-ttu-id="94a97-190">Tato stránka zobrazuje hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="94a97-190">This page displays hello following information:</span></span>

    * <span data-ttu-id="94a97-191">**Statistiky funkcí spout/Bolt** – základní informace o hello výkonu komponenty uspořádané do časových oken.</span><span class="sxs-lookup"><span data-stu-id="94a97-191">**Spout/Bolt stats** - Basic information on hello component performance, organized into time windows.</span></span>

        > [!NOTE]
        > <span data-ttu-id="94a97-192">Výběr určité časové okno změny hello časové okno informací zobrazených v dalších částech stránky hello.</span><span class="sxs-lookup"><span data-stu-id="94a97-192">Selecting a specific time window changes hello time window for information displayed in other sections of hello page.</span></span>

    * <span data-ttu-id="94a97-193">**Statistiky vstupu** (pouze funkce bolt) – informace o komponentech, které vytváří dat využívaná funkcí hello bolt.</span><span class="sxs-lookup"><span data-stu-id="94a97-193">**Input stats** (bolt only) - Information on components that produce data consumed by hello bolt.</span></span>

    * <span data-ttu-id="94a97-194">**Statististiky výstupu** – informace o datech vysílaných touto funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="94a97-194">**Output stats** - Information on data emitted by this bolt.</span></span>

    * <span data-ttu-id="94a97-195">**Prováděcí moduly** – informace o instancích této komponenty.</span><span class="sxs-lookup"><span data-stu-id="94a97-195">**Executors** - Information on instances of this component.</span></span>

    * <span data-ttu-id="94a97-196">**Chyby** – chyby vytvořené touto komponentou.</span><span class="sxs-lookup"><span data-stu-id="94a97-196">**Errors** - Errors produced by this component.</span></span>

4. <span data-ttu-id="94a97-197">Při zobrazení podrobností hello spout nebo bolt, vyberte položku z hello **Port** sloupec v hello **vykonavatelů** části tooview podrobnosti pro konkrétní instanci komponenty hello.</span><span class="sxs-lookup"><span data-stu-id="94a97-197">When viewing hello details of a spout or bolt, select an entry from hello **Port** column in hello **Executors** section tooview details for a specific instance of hello component.</span></span>

        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]

    <span data-ttu-id="94a97-198">V tomto příkladu hello word **sedm** došlo 1 493 957krát.</span><span class="sxs-lookup"><span data-stu-id="94a97-198">In this example, hello word **seven** has occurred 1493957 times.</span></span> <span data-ttu-id="94a97-199">Je to počet kolikrát hello word došlo od spuštění této topologii.</span><span class="sxs-lookup"><span data-stu-id="94a97-199">This count is how many times hello word has been encountered since this topology was started.</span></span>

## <a name="stop-hello-topology"></a><span data-ttu-id="94a97-200">Zastavení topologie hello</span><span class="sxs-lookup"><span data-stu-id="94a97-200">Stop hello topology</span></span>

<span data-ttu-id="94a97-201">Vrátí toohello **souhrn topologie** pro hello topologii počtu slov a pak vyberte hello **Kill** tlačítko z hello **topologie akce** části.</span><span class="sxs-lookup"><span data-stu-id="94a97-201">Return toohello **Topology summary** page for hello word-count topology, and then select hello **Kill** button from hello **Topology actions** section.</span></span> <span data-ttu-id="94a97-202">Po zobrazení výzvy zadejte 10 sekund toowait hello před zastavením topologie hello.</span><span class="sxs-lookup"><span data-stu-id="94a97-202">When prompted, enter 10 for hello seconds toowait before stopping hello topology.</span></span> <span data-ttu-id="94a97-203">Po hello časového limitu hello topologie již nadále nebude zobrazovat při návštěvě hello **uživatelské rozhraní Storm** část řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="94a97-203">After hello timeout period, hello topology no longer appears when you visit hello **Storm UI** section of hello dashboard.</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="94a97-204">Odstranění clusteru hello</span><span class="sxs-lookup"><span data-stu-id="94a97-204">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="94a97-205">Pokud narazíte na problém s vytvářením clusteru HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="94a97-205">If you run into an issue with creating HDInsight cluster, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <span data-ttu-id="94a97-206"><a id="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="94a97-206"><a id="next"></a>Next steps</span></span>

<span data-ttu-id="94a97-207">V tomto kurzu Apache Storm jste se dozvěděli hello základy práce se Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="94a97-207">In this Apache Storm tutorial, you learned hello basics of working with Storm on HDInsight.</span></span> <span data-ttu-id="94a97-208">Dále se naučíte, jak příliš[založené na jazyce Java vyvíjet topologie pomocí nástroje Maven](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="94a97-208">Next, learn how too[Develop Java-based topologies using Maven](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="94a97-209">Pokud jste již obeznámeni s vývojem topologií založených na jazyce Java a chcete toodeploy existující tooHDInsight topologii, přečtěte si téma [nasazení a správa topologií Apache Storm v HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).</span><span class="sxs-lookup"><span data-stu-id="94a97-209">If you're already familiar with developing Java-based topologies and want toodeploy an existing topology tooHDInsight, see [Deploy and manage Apache Storm topologies on HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).</span></span>

<span data-ttu-id="94a97-210">Pokud jste vývojář .NET, můžete s použitím sady Visual Studio vytvořit topologie C# nebo hybridní topologie C#/Java.</span><span class="sxs-lookup"><span data-stu-id="94a97-210">If you are a .NET developer, you can create C# or hybrid C#/Java topologies using Visual Studio.</span></span> <span data-ttu-id="94a97-211">Další informace najdete v tématu [Vývoj topologií C# pro Apache Storm ve službě HDInsight pomocí nástrojů Hadoop pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="94a97-211">For more information, see [Develop C# topologies for Apache Storm on HDInsight using Hadoop tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

<span data-ttu-id="94a97-212">Topologie, které lze použít s Storm v HDInsight, například zobrazit hello následující příklady:</span><span class="sxs-lookup"><span data-stu-id="94a97-212">For example topologies that can be used with Storm on HDInsight, see hello following examples:</span></span>

* [<span data-ttu-id="94a97-213">Příklad topologií pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="94a97-213">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[preview-portal]: https://portal.azure.com/
