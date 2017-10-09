---
title: "aaaInstall Presto v Azure HDInsight Linux clusterů | Microsoft Docs"
description: "Zjistěte, jak se tooinstall Presto a Airpal na systémem Linux HDInsight Hadoop clusterů, pomocí akcí skriptů."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2017
ms.author: nitinme
ms.openlocfilehash: 5d54d0efc3e5fdc6f5a8d3a94ad2f61d16df24c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a><span data-ttu-id="ec26c-103">Na nainstalovat a používat Presto clusterů systému HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="ec26c-103">Install and use Presto on HDInsight Hadoop clusters</span></span>

<span data-ttu-id="ec26c-104">V tomto tématu se dozvíte, jak tooinstall Presto na HDInsight Hadoop clusterů pomocí akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="ec26c-104">In this topic, you learn how tooinstall Presto on HDInsight Hadoop clusters by using Script Action.</span></span> <span data-ttu-id="ec26c-105">Také zjistíte, jak tooinstall Airpal na stávající cluster Presto HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ec26c-105">You also learn how tooinstall Airpal on an existing Presto HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ec26c-106">Hello kroky v tomto dokumentu vyžadují **clusteru HDInsight 3.5 Hadoop** Linux, který používá.</span><span class="sxs-lookup"><span data-stu-id="ec26c-106">hello steps in this document require an **HDInsight 3.5 Hadoop cluster** that uses Linux.</span></span> <span data-ttu-id="ec26c-107">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ec26c-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ec26c-108">Další informace najdete v tématu [HDInsight verze](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="ec26c-108">For more information, see [HDInsight versions](hdinsight-component-versioning.md).</span></span>

## <a name="what-is-presto"></a><span data-ttu-id="ec26c-109">Co je Presto?</span><span class="sxs-lookup"><span data-stu-id="ec26c-109">What is Presto?</span></span>
<span data-ttu-id="ec26c-110">[Presto](https://prestodb.io/overview.html) je rychlé distribuované modul dotazů SQL pro velká data.</span><span class="sxs-lookup"><span data-stu-id="ec26c-110">[Presto](https://prestodb.io/overview.html) is a fast distributed SQL query engine for big data.</span></span> <span data-ttu-id="ec26c-111">Presto je vhodná pro interaktivní dotazování petabajty dat.</span><span class="sxs-lookup"><span data-stu-id="ec26c-111">Presto is suitable for interactive querying of petabytes of data.</span></span> <span data-ttu-id="ec26c-112">Další informace o součásti hello Presto a jak pracují společně, najdete v části [Presto koncepty](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).</span><span class="sxs-lookup"><span data-stu-id="ec26c-112">For more information on hello components of Presto and how they work together, see [Presto concepts](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).</span></span>

> [!WARNING]
> <span data-ttu-id="ec26c-113">Součásti, které jsou součástí clusteru HDInsight hello jsou plně podporované a Microsoft Support bude pomoci tooisolate a vyřešit problémy související toothese součásti.</span><span class="sxs-lookup"><span data-stu-id="ec26c-113">Components provided with hello HDInsight cluster are fully supported and Microsoft Support will help tooisolate and resolve issues related toothese components.</span></span>
> 
> <span data-ttu-id="ec26c-114">Vlastní komponenty, například Presto přijímat vyvineme podporu toohelp toofurther můžete vyřešit problém hello.</span><span class="sxs-lookup"><span data-stu-id="ec26c-114">Custom components, such as Presto, receive commercially reasonable support toohelp you toofurther troubleshoot hello issue.</span></span> <span data-ttu-id="ec26c-115">To může způsobit řešení problému hello nebo žádat, že jste tooengage dostupné kanály pro hello otevřít zdroj technologie, kterých se nachází hluboké znalosti pro tuto technologii.</span><span class="sxs-lookup"><span data-stu-id="ec26c-115">This might result in resolving hello issue OR asking you tooengage available channels for hello open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="ec26c-116">Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ec26c-116">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>
> 
> 


## <a name="install-presto-using-script-action"></a><span data-ttu-id="ec26c-117">Nainstalujte Presto pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="ec26c-117">Install Presto using script action</span></span>

<span data-ttu-id="ec26c-118">Tato část obsahuje pokyny jak hello toouse hello ukázkový skript při vytváření nového clusteru pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ec26c-118">This section provides instructions on how toouse hello sample script when creating a new cluster by using hello Azure portal.</span></span> 

1. <span data-ttu-id="ec26c-119">Spuštění zřizování clusteru pomocí kroků hello v [clustery HDInsight se systémem Linux](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ec26c-119">Start provisioning a cluster by using hello steps in [Provision Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span> <span data-ttu-id="ec26c-120">Zajistěte, aby vytvoříte hello clusteru pomocí hello **vlastní** postup vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="ec26c-120">Make sure you create hello cluster using hello **Custom** cluster creation flow.</span></span> <span data-ttu-id="ec26c-121">Je nutné zajistit, že splňuje následující požadavky hello hello clusteru, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="ec26c-121">You must ensure that hello cluster you create meets hello following requirements.</span></span>

    <span data-ttu-id="ec26c-122">a.</span><span class="sxs-lookup"><span data-stu-id="ec26c-122">a.</span></span> <span data-ttu-id="ec26c-123">Musí to být clusteru Hadoop v prostředí HDInsight verze 3.5.</span><span class="sxs-lookup"><span data-stu-id="ec26c-123">It must be a Hadoop cluster with HDInsight version 3.5.</span></span>

    <span data-ttu-id="ec26c-124">b.</span><span class="sxs-lookup"><span data-stu-id="ec26c-124">b.</span></span> <span data-ttu-id="ec26c-125">Azure Storage musí používat jako úložiště dat hello.</span><span class="sxs-lookup"><span data-stu-id="ec26c-125">It must use Azure Storage as hello data store.</span></span> <span data-ttu-id="ec26c-126">Použití Presto v clusteru, který používá Azure Data Lake Store jako možnost úložiště hello není dosud podporováno.</span><span class="sxs-lookup"><span data-stu-id="ec26c-126">Using Presto on a cluster that uses Azure Data Lake Store as hello storage option is not yet supported.</span></span> 

    ![Vytvoření clusteru HDInsight pomocí vlastních možností](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. <span data-ttu-id="ec26c-128">Na hello **upřesňující nastavení** vyberte **akcí skriptů**a zadejte níže uvedené informace hello:</span><span class="sxs-lookup"><span data-stu-id="ec26c-128">On hello **Advanced settings** blade, select **Script Actions**, and provide hello information below:</span></span>
   
   * <span data-ttu-id="ec26c-129">**NÁZEV**: Zadejte popisný název akce skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="ec26c-129">**NAME**: Enter a friendly name for hello script action.</span></span>
   * <span data-ttu-id="ec26c-130">**Identifikátor URI skriptu Bash:**`https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`</span><span class="sxs-lookup"><span data-stu-id="ec26c-130">**Bash script URI**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`</span></span>
   * <span data-ttu-id="ec26c-131">**HEAD**: zaškrtnete tuto možnost,</span><span class="sxs-lookup"><span data-stu-id="ec26c-131">**HEAD**: Check this option</span></span>
   * <span data-ttu-id="ec26c-132">**PRACOVNÍ**: zaškrtnete tuto možnost,</span><span class="sxs-lookup"><span data-stu-id="ec26c-132">**WORKER**: Check this option</span></span>
   * <span data-ttu-id="ec26c-133">**ZOOKEEPER**: zaškrtnutí tohoto políčka zrušte</span><span class="sxs-lookup"><span data-stu-id="ec26c-133">**ZOOKEEPER**: Clear this check box</span></span>
   * <span data-ttu-id="ec26c-134">**Parametry**: to pole ponechat prázdné</span><span class="sxs-lookup"><span data-stu-id="ec26c-134">**PARAMETERS**: Leave this field blank</span></span>


3. <span data-ttu-id="ec26c-135">Na konci hello hello **akcí skriptů** okně klikněte na tlačítko hello **vyberte** tlačítko toosave hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ec26c-135">At hello bottom of hello **Script Actions** blade, click hello **Select** button toosave hello configuration.</span></span> <span data-ttu-id="ec26c-136">Nakonec klikněte na hello **vyberte** tlačítko dole hello hello **Upřesnit nastavení** informace o konfiguraci okna toosave hello.</span><span class="sxs-lookup"><span data-stu-id="ec26c-136">Finally, click  hello **Select** button at hello bottom of hello **Advanced Settings** blade toosave hello configuration information.</span></span>

4. <span data-ttu-id="ec26c-137">Pokračovat zřizování hello clusteru, jak je popsáno v [clustery HDInsight se systémem Linux](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ec26c-137">Continue provisioning hello cluster as described in [Provision Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="ec26c-138">Azure PowerShell, hello rozhraní příkazového řádku Azure, hello HDInsight .NET SDK nebo šablon Azure Resource Manageru může být také použít tooapply akcí skriptů.</span><span class="sxs-lookup"><span data-stu-id="ec26c-138">Azure PowerShell, hello Azure CLI, hello HDInsight .NET SDK, or Azure Resource Manager templates can also be used tooapply script actions.</span></span> <span data-ttu-id="ec26c-139">Můžete také použít skript akce tooalready spuštěné clustery.</span><span class="sxs-lookup"><span data-stu-id="ec26c-139">You can also apply script actions tooalready running clusters.</span></span> <span data-ttu-id="ec26c-140">Další informace najdete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="ec26c-140">For more information, see [Customize HDInsight clusters with Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>
    > 
    > 

## <a name="use-presto-with-hdinsight"></a><span data-ttu-id="ec26c-141">Použijte Presto s HDInsight</span><span class="sxs-lookup"><span data-stu-id="ec26c-141">Use Presto with HDInsight</span></span>

<span data-ttu-id="ec26c-142">Proveďte následující kroky toouse Presto v clusteru služby HDInsight, po instalaci pomocí výše popsané kroky hello hello.</span><span class="sxs-lookup"><span data-stu-id="ec26c-142">Perform hello following steps toouse Presto in an HDInsight cluster after you have installed it using hello steps described above.</span></span>

1. <span data-ttu-id="ec26c-143">Připojte toohello clusteru HDInsight pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="ec26c-143">Connect toohello HDInsight cluster using SSH:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="ec26c-144">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ec26c-144">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>
     

2. <span data-ttu-id="ec26c-145">Spusťte prostředí Presto hello pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="ec26c-145">Start hello Presto shell using hello following command.</span></span>
   
        presto --schema default

3. <span data-ttu-id="ec26c-146">Spuštění dotazu v tabulce ukázka **hivesampletable**, která je k dispozici na všech clusterech HDInsight ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ec26c-146">Run a query on a sample table, **hivesampletable**, which is available on all HDInsight clusters by default.</span></span>
   
        select count (*) from hivesampletable;
   
    <span data-ttu-id="ec26c-147">Ve výchozím nastavení [Hive](https://prestodb.io/docs/current/connector/hive.html) a [TPCH](https://prestodb.io/docs/current/connector/tpch.html) konektory pro Presto jsou již nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="ec26c-147">By default, [Hive](https://prestodb.io/docs/current/connector/hive.html) and [TPCH](https://prestodb.io/docs/current/connector/tpch.html) connectors for Presto are already configured.</span></span> <span data-ttu-id="ec26c-148">Hive konektor je nakonfigurované toouse hello výchozí nainstalován Hive instalace, takže všechny tabulky hello z podregistru budou automaticky viditelné v Presto.</span><span class="sxs-lookup"><span data-stu-id="ec26c-148">Hive connector is configured toouse hello default installed Hive installation, so all hello tables from Hive will be automatically visible in Presto.</span></span>

    <span data-ttu-id="ec26c-149">Podrobný popis na použití Presto naleznete v tématu [Presto dokumentaci](https://prestodb.io/docs/current/index.html).</span><span class="sxs-lookup"><span data-stu-id="ec26c-149">For a detailed description on how you can use Presto, see [Presto documentation](https://prestodb.io/docs/current/index.html).</span></span>

## <a name="use-airpal-with-presto"></a><span data-ttu-id="ec26c-150">Pomocí Airpal Presto</span><span class="sxs-lookup"><span data-stu-id="ec26c-150">Use Airpal with Presto</span></span>

<span data-ttu-id="ec26c-151">[Airpal](https://github.com/airbnb/airpal#airpal) je dotazu open-source webové rozhraní pro Presto.</span><span class="sxs-lookup"><span data-stu-id="ec26c-151">[Airpal](https://github.com/airbnb/airpal#airpal) is an open-source web-based query interface for Presto.</span></span> <span data-ttu-id="ec26c-152">Další informace o Airpal najdete v tématu [Airpal dokumentaci](https://github.com/airbnb/airpal#airpal).</span><span class="sxs-lookup"><span data-stu-id="ec26c-152">For more information on Airpal, see [Airpal documentation](https://github.com/airbnb/airpal#airpal).</span></span>

<span data-ttu-id="ec26c-153">V této části se podíváme na hello kroky příliš**nainstalujte Airpal hello edgenode** clusteru HDInsight Hadoop, kde je již Presto nainstalována.</span><span class="sxs-lookup"><span data-stu-id="ec26c-153">In this section, we look at hello steps too**install Airpal on hello edgenode** of an HDInsight Hadoop cluster, that already has Presto installed.</span></span> <span data-ttu-id="ec26c-154">Tím se zajistí, že tento hello Airpal webové rozhraní je k dispozici prostřednictvím hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="ec26c-154">This ensures that hello Airpal web query interface is available over hello Internet.</span></span>

1. <span data-ttu-id="ec26c-155">Pomocí protokolu SSH, připojte toohello headnode hello clusteru HDInsight, která má nainstalovanou Presto:</span><span class="sxs-lookup"><span data-stu-id="ec26c-155">Using SSH, connect toohello headnode of hello HDInsight cluster that has Presto installed:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="ec26c-156">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ec26c-156">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="ec26c-157">Jakmile se připojíte, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="ec26c-157">Once you are connected, run hello following command.</span></span>

        sudo slider registry  --name presto1 --getexp presto 
   
    <span data-ttu-id="ec26c-158">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="ec26c-158">You should see an output like hello following:</span></span>

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. <span data-ttu-id="ec26c-159">Z výstupu hello, poznamenejte si hodnotu hello pro hello **hodnotu** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ec26c-159">From hello output, note hello value for hello **value** property.</span></span> <span data-ttu-id="ec26c-160">Je nutné při instalaci Airpal na edgenode hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ec26c-160">You will need this while installing Airpal on hello cluster edgenode.</span></span> <span data-ttu-id="ec26c-161">Z výstupu hello výše hello hodnotu, která je nutné je **10.0.0.12:9090**.</span><span class="sxs-lookup"><span data-stu-id="ec26c-161">From hello output above, hello value that you will need is **10.0.0.12:9090**.</span></span>

4. <span data-ttu-id="ec26c-162">Použít šablonu hello  **[sem](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)**  toocreate HDInsight clusteru edgenode a zadejte hodnoty hello, jak ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="ec26c-162">Use hello template **[here](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)** toocreate an HDInsight cluster edgenode and provide hello values as shown in hello following screenshot.</span></span>

    ![Instalace HDInsight Airpal na Presto clusteru](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. <span data-ttu-id="ec26c-164">Klikněte na **Koupit**.</span><span class="sxs-lookup"><span data-stu-id="ec26c-164">Click **Purchase**.</span></span>

6. <span data-ttu-id="ec26c-165">Jakmile jsou změny hello použité toohello konfiguraci clusteru, můžete pomocí následujících kroků hello přístup hello Airpal webové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ec26c-165">Once hello changes are applied toohello cluster configuration, you can access hello Airpal web interface by using hello following steps.</span></span>

    <span data-ttu-id="ec26c-166">a.</span><span class="sxs-lookup"><span data-stu-id="ec26c-166">a.</span></span> <span data-ttu-id="ec26c-167">V okně hello clusteru, klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ec26c-167">From hello cluster blade, click **Applications**.</span></span>

    ![Spuštění HDInsight Airpal na Presto clusteru](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    <span data-ttu-id="ec26c-169">b.</span><span class="sxs-lookup"><span data-stu-id="ec26c-169">b.</span></span> <span data-ttu-id="ec26c-170">Z hello **nainstalované aplikace** okně klikněte na tlačítko **portál** proti airpal.</span><span class="sxs-lookup"><span data-stu-id="ec26c-170">From hello **Installed Apps** blade, click **Portal** against airpal.</span></span>

    ![Spuštění HDInsight Airpal na Presto clusteru](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    <span data-ttu-id="ec26c-172">c.</span><span class="sxs-lookup"><span data-stu-id="ec26c-172">c.</span></span> <span data-ttu-id="ec26c-173">Po zobrazení výzvy zadejte přihlašovací údaje správce hello, které jste zadali při vytváření clusteru HDInsight Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="ec26c-173">When prompted, enter hello admin credentials that you specified while creating hello HDInsight Hadoop cluster.</span></span>

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a><span data-ttu-id="ec26c-174">Přizpůsobení Presto instalace na clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="ec26c-174">Customize a Presto installation on HDInsight cluster</span></span>

<span data-ttu-id="ec26c-175">Po instalaci Presto na clusteru HDInsight Hadoop, můžete přizpůsobit hello instalace toomake změny např. nastavení paměti aktualizace, změnit konektorů, atd. Proveďte následující kroky toodo tak hello.</span><span class="sxs-lookup"><span data-stu-id="ec26c-175">After you have installed Presto on an HDInsight Hadoop cluster, you can customize hello installation toomake changes such as update memory settings, change connectors, etc. Perform hello following steps toodo so.</span></span>

1. <span data-ttu-id="ec26c-176">Pomocí protokolu SSH, připojte toohello headnode hello clusteru HDInsight, která má nainstalovanou Presto:</span><span class="sxs-lookup"><span data-stu-id="ec26c-176">Using SSH, connect toohello headnode of hello HDInsight cluster that has Presto installed:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="ec26c-177">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ec26c-177">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="ec26c-178">Provedené změny konfigurace v souboru hello `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`.</span><span class="sxs-lookup"><span data-stu-id="ec26c-178">Make your configuration changes in hello file `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`.</span></span> <span data-ttu-id="ec26c-179">Další informace o konfiguraci Presto, najdete v části [Presto konfigurace pro clustery založené na YARN](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).</span><span class="sxs-lookup"><span data-stu-id="ec26c-179">For more information on Presto configuration, see [Presto configuration for YARN-based clusters](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).</span></span>

3. <span data-ttu-id="ec26c-180">Zastavení a ukončení hello aktuální spuštěnou instanci Presto.</span><span class="sxs-lookup"><span data-stu-id="ec26c-180">Stop and kill hello current running instance of Presto.</span></span>

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. <span data-ttu-id="ec26c-181">Spusťte novou instanci třídy Presto s hello přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="ec26c-181">Start a new instance of Presto with hello customization.</span></span>

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. <span data-ttu-id="ec26c-182">Počkejte hello nové instance toobe připravené a poznamenejte si adresu presto coordinator.</span><span class="sxs-lookup"><span data-stu-id="ec26c-182">Wait for hello new instance toobe ready and note presto coordinator address.</span></span>


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a><span data-ttu-id="ec26c-183">Generovat srovnávacího testu data pro clustery služby HDInsight se systémem Presto</span><span class="sxs-lookup"><span data-stu-id="ec26c-183">Generate benchmark data for HDInsight clusters that run Presto</span></span>

<span data-ttu-id="ec26c-184">TPC DS je hello oborový standard pro měření výkonu hello mnoho rozhodnutí podpora systémů, včetně systémy velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="ec26c-184">TPC-DS is hello industry standard for measuring hello performance of many decision support systems, including big data systems.</span></span> <span data-ttu-id="ec26c-185">Můžete použít Presto na HDInsight clustery toogenerate data a vyhodnotit, jak se porovnává s vlastními daty srovnávacího testu HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ec26c-185">You can use Presto on HDInsight clusters toogenerate data and evaluate how it compares with your own HDInsight benchmark data.</span></span> <span data-ttu-id="ec26c-186">Další informace najdete v tématu [zde](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="ec26c-186">For more information, see [here](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).</span></span>



## <a name="see-also"></a><span data-ttu-id="ec26c-187">Viz také</span><span class="sxs-lookup"><span data-stu-id="ec26c-187">See also</span></span>
* <span data-ttu-id="ec26c-188">[Nainstalovat a používat Hue v clusterech HDInsight](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="ec26c-188">[Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span> <span data-ttu-id="ec26c-189">HUE je webové uživatelské rozhraní, které umožňuje snadno toocreate, spuštění a uložte úlohy Pig a Hive, stejně jako procházet hello výchozí úložiště pro váš cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ec26c-189">Hue is a web UI that makes it easy toocreate, run and save Pig and Hive jobs, as well as browse hello default storage for your HDInsight cluster.</span></span>

* <span data-ttu-id="ec26c-190">[Nainstalujte Giraph clustery HDInsight](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="ec26c-190">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md).</span></span> <span data-ttu-id="ec26c-191">Použijte vlastní nastavení tooinstall clusteru, které Giraph na HDInsight Hadoop clusterů.</span><span class="sxs-lookup"><span data-stu-id="ec26c-191">Use cluster customization tooinstall Giraph on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="ec26c-192">Giraph můžete graf tooperform zpracování pomocí Hadoop a lze použít s Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ec26c-192">Giraph allows you tooperform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span>

* <span data-ttu-id="ec26c-193">[Nainstalujte Solr clustery HDInsight](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="ec26c-193">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md).</span></span> <span data-ttu-id="ec26c-194">Použijte vlastní nastavení tooinstall clusteru, které Solr na HDInsight Hadoop clusterů.</span><span class="sxs-lookup"><span data-stu-id="ec26c-194">Use cluster customization tooinstall Solr on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="ec26c-195">Solr vám umožní operace výkonné hledání tooperform na uložená data.</span><span class="sxs-lookup"><span data-stu-id="ec26c-195">Solr allows you tooperform powerful search operations on stored data.</span></span>

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
