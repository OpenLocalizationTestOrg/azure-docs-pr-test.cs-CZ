---
title: Nainstalujte Presto v clusterech Azure HDInsight Linux | Microsoft Docs
description: "Zjistěte, jak nainstalovat Presto a Airpal na systémem Linux HDInsight Hadoop clustery pomocí akcí skriptů."
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
ms.openlocfilehash: 406ef84e72d253fec51a0b37c48f326dafd511b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a><span data-ttu-id="c4225-103">Na nainstalovat a používat Presto clusterů systému HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="c4225-103">Install and use Presto on HDInsight Hadoop clusters</span></span>

<span data-ttu-id="c4225-104">V tomto tématu se naučíte instalace Presto na clusterů systému HDInsight Hadoop pomocí akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="c4225-104">In this topic, you learn how to install Presto on HDInsight Hadoop clusters by using Script Action.</span></span> <span data-ttu-id="c4225-105">Také zjistíte, jak nainstalovat Airpal na stávající cluster Presto HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c4225-105">You also learn how to install Airpal on an existing Presto HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c4225-106">Kroky v tomto dokumentu vyžadují **clusteru HDInsight 3.5 Hadoop** Linux, který používá.</span><span class="sxs-lookup"><span data-stu-id="c4225-106">The steps in this document require an **HDInsight 3.5 Hadoop cluster** that uses Linux.</span></span> <span data-ttu-id="c4225-107">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="c4225-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="c4225-108">Další informace najdete v tématu [HDInsight verze](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="c4225-108">For more information, see [HDInsight versions](hdinsight-component-versioning.md).</span></span>

## <a name="what-is-presto"></a><span data-ttu-id="c4225-109">Co je Presto?</span><span class="sxs-lookup"><span data-stu-id="c4225-109">What is Presto?</span></span>
<span data-ttu-id="c4225-110">[Presto](https://prestodb.io/overview.html) je rychlé distribuované modul dotazů SQL pro velká data.</span><span class="sxs-lookup"><span data-stu-id="c4225-110">[Presto](https://prestodb.io/overview.html) is a fast distributed SQL query engine for big data.</span></span> <span data-ttu-id="c4225-111">Presto je vhodná pro interaktivní dotazování petabajty dat.</span><span class="sxs-lookup"><span data-stu-id="c4225-111">Presto is suitable for interactive querying of petabytes of data.</span></span> <span data-ttu-id="c4225-112">Další informace o součásti Presto a jak pracují společně, najdete v části [Presto koncepty](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).</span><span class="sxs-lookup"><span data-stu-id="c4225-112">For more information on the components of Presto and how they work together, see [Presto concepts](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).</span></span>

> [!WARNING]
> <span data-ttu-id="c4225-113">Součásti, které jsou součástí clusteru HDInsight jsou plně podporované a Microsoft Support pomůže k izolování a vyřešení problémů týkajících se těchto součástí.</span><span class="sxs-lookup"><span data-stu-id="c4225-113">Components provided with the HDInsight cluster are fully supported and Microsoft Support will help to isolate and resolve issues related to these components.</span></span>
> 
> <span data-ttu-id="c4225-114">Vlastní komponenty, například Presto přijímat vyvineme podporu o pomoci při další řešení problému.</span><span class="sxs-lookup"><span data-stu-id="c4225-114">Custom components, such as Presto, receive commercially reasonable support to help you to further troubleshoot the issue.</span></span> <span data-ttu-id="c4225-115">To může způsobit řešení problému nebo s žádostí o zapojení dostupné kanály pro technologie s otevřeným zdrojem, kterých se nachází hluboké znalosti pro tuto technologii.</span><span class="sxs-lookup"><span data-stu-id="c4225-115">This might result in resolving the issue OR asking you to engage available channels for the open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="c4225-116">Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span><span class="sxs-lookup"><span data-stu-id="c4225-116">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span></span> <span data-ttu-id="c4225-117">Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="c4225-117">Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>
> 
> 


## <a name="install-presto-using-script-action"></a><span data-ttu-id="c4225-118">Nainstalujte Presto pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="c4225-118">Install Presto using script action</span></span>

<span data-ttu-id="c4225-119">Tato část obsahuje pokyny o tom, jak pomocí vzorového skriptu při vytváření nového clusteru pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c4225-119">This section provides instructions on how to use the sample script when creating a new cluster by using the Azure portal.</span></span> 

1. <span data-ttu-id="c4225-120">Spuštění zřizování clusteru pomocí kroků v [clustery HDInsight se systémem Linux](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c4225-120">Start provisioning a cluster by using the steps in [Provision Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span> <span data-ttu-id="c4225-121">Ujistěte se, vytvořit cluster pomocí **vlastní** postup vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4225-121">Make sure you create the cluster using the **Custom** cluster creation flow.</span></span> <span data-ttu-id="c4225-122">Je nutné zajistit, že cluster, který vytvoříte splňuje následující požadavky.</span><span class="sxs-lookup"><span data-stu-id="c4225-122">You must ensure that the cluster you create meets the following requirements.</span></span>

    <span data-ttu-id="c4225-123">a.</span><span class="sxs-lookup"><span data-stu-id="c4225-123">a.</span></span> <span data-ttu-id="c4225-124">Musí to být clusteru Hadoop v prostředí HDInsight verze 3.5.</span><span class="sxs-lookup"><span data-stu-id="c4225-124">It must be a Hadoop cluster with HDInsight version 3.5.</span></span>

    <span data-ttu-id="c4225-125">b.</span><span class="sxs-lookup"><span data-stu-id="c4225-125">b.</span></span> <span data-ttu-id="c4225-126">Jako úložiště dat musí používat úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="c4225-126">It must use Azure Storage as the data store.</span></span> <span data-ttu-id="c4225-127">Použití Presto v clusteru, který používá Azure Data Lake Store jako možnost úložiště není dosud podporováno.</span><span class="sxs-lookup"><span data-stu-id="c4225-127">Using Presto on a cluster that uses Azure Data Lake Store as the storage option is not yet supported.</span></span> 

    ![Vytvoření clusteru HDInsight pomocí vlastních možností](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. <span data-ttu-id="c4225-129">Na **upřesňující nastavení** vyberte **akcí skriptů**a zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="c4225-129">On the **Advanced settings** blade, select **Script Actions**, and provide the information below:</span></span>
   
   * <span data-ttu-id="c4225-130">**NÁZEV**: Zadejte popisný název akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="c4225-130">**NAME**: Enter a friendly name for the script action.</span></span>
   * <span data-ttu-id="c4225-131">**Identifikátor URI skriptu Bash:** `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`</span><span class="sxs-lookup"><span data-stu-id="c4225-131">**Bash script URI**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`</span></span>
   * <span data-ttu-id="c4225-132">**HEAD**: zaškrtnete tuto možnost,</span><span class="sxs-lookup"><span data-stu-id="c4225-132">**HEAD**: Check this option</span></span>
   * <span data-ttu-id="c4225-133">**PRACOVNÍ**: zaškrtnete tuto možnost,</span><span class="sxs-lookup"><span data-stu-id="c4225-133">**WORKER**: Check this option</span></span>
   * <span data-ttu-id="c4225-134">**ZOOKEEPER**: zaškrtnutí tohoto políčka zrušte</span><span class="sxs-lookup"><span data-stu-id="c4225-134">**ZOOKEEPER**: Clear this check box</span></span>
   * <span data-ttu-id="c4225-135">**Parametry**: to pole ponechat prázdné</span><span class="sxs-lookup"><span data-stu-id="c4225-135">**PARAMETERS**: Leave this field blank</span></span>


3. <span data-ttu-id="c4225-136">V dolní části **akcí skriptů** okně klikněte na tlačítko **vyberte** tlačítko Uložit konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="c4225-136">At the bottom of the **Script Actions** blade, click the **Select** button to save the configuration.</span></span> <span data-ttu-id="c4225-137">Nakonec klikněte na **vyberte** tlačítko v dolní části **Upřesnit nastavení** okno a uložte informace o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="c4225-137">Finally, click  the **Select** button at the bottom of the **Advanced Settings** blade to save the configuration information.</span></span>

4. <span data-ttu-id="c4225-138">Pokračovat zřizování clusteru, jak je popsáno v [clustery HDInsight se systémem Linux](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c4225-138">Continue provisioning the cluster as described in [Provision Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="c4225-139">Azure PowerShell, rozhraní příkazového řádku Azure, sady .NET SDK HDInsight nebo šablon Azure Resource Manageru lze také použít skript akce.</span><span class="sxs-lookup"><span data-stu-id="c4225-139">Azure PowerShell, the Azure CLI, the HDInsight .NET SDK, or Azure Resource Manager templates can also be used to apply script actions.</span></span> <span data-ttu-id="c4225-140">Můžete taky použít skript akce již clustery spuštěná.</span><span class="sxs-lookup"><span data-stu-id="c4225-140">You can also apply script actions to already running clusters.</span></span> <span data-ttu-id="c4225-141">Další informace najdete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="c4225-141">For more information, see [Customize HDInsight clusters with Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>
    > 
    > 

## <a name="use-presto-with-hdinsight"></a><span data-ttu-id="c4225-142">Použijte Presto s HDInsight</span><span class="sxs-lookup"><span data-stu-id="c4225-142">Use Presto with HDInsight</span></span>

<span data-ttu-id="c4225-143">Proveďte následující postup použijte Presto v clusteru služby HDInsight po instalaci pomocí výše uvedeného postupu.</span><span class="sxs-lookup"><span data-stu-id="c4225-143">Perform the following steps to use Presto in an HDInsight cluster after you have installed it using the steps described above.</span></span>

1. <span data-ttu-id="c4225-144">Připojte se ke clusteru HDInsight pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="c4225-144">Connect to the HDInsight cluster using SSH:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="c4225-145">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="c4225-145">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>
     

2. <span data-ttu-id="c4225-146">Spusťte Presto prostředí pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="c4225-146">Start the Presto shell using the following command.</span></span>
   
        presto --schema default

3. <span data-ttu-id="c4225-147">Spuštění dotazu v tabulce ukázka **hivesampletable**, která je k dispozici na všech clusterech HDInsight ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="c4225-147">Run a query on a sample table, **hivesampletable**, which is available on all HDInsight clusters by default.</span></span>
   
        select count (*) from hivesampletable;
   
    <span data-ttu-id="c4225-148">Ve výchozím nastavení [Hive](https://prestodb.io/docs/current/connector/hive.html) a [TPCH](https://prestodb.io/docs/current/connector/tpch.html) konektory pro Presto jsou již nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="c4225-148">By default, [Hive](https://prestodb.io/docs/current/connector/hive.html) and [TPCH](https://prestodb.io/docs/current/connector/tpch.html) connectors for Presto are already configured.</span></span> <span data-ttu-id="c4225-149">Hive konektor je nakonfigurovaný na použití výchozí nainstalován Hive instalace, takže všechny tabulky z podregistru budou automaticky viditelné v Presto.</span><span class="sxs-lookup"><span data-stu-id="c4225-149">Hive connector is configured to use the default installed Hive installation, so all the tables from Hive will be automatically visible in Presto.</span></span>

    <span data-ttu-id="c4225-150">Podrobný popis na použití Presto naleznete v tématu [Presto dokumentaci](https://prestodb.io/docs/current/index.html).</span><span class="sxs-lookup"><span data-stu-id="c4225-150">For a detailed description on how you can use Presto, see [Presto documentation](https://prestodb.io/docs/current/index.html).</span></span>

## <a name="use-airpal-with-presto"></a><span data-ttu-id="c4225-151">Pomocí Airpal Presto</span><span class="sxs-lookup"><span data-stu-id="c4225-151">Use Airpal with Presto</span></span>

<span data-ttu-id="c4225-152">[Airpal](https://github.com/airbnb/airpal#airpal) je dotazu open-source webové rozhraní pro Presto.</span><span class="sxs-lookup"><span data-stu-id="c4225-152">[Airpal](https://github.com/airbnb/airpal#airpal) is an open-source web-based query interface for Presto.</span></span> <span data-ttu-id="c4225-153">Další informace o Airpal najdete v tématu [Airpal dokumentaci](https://github.com/airbnb/airpal#airpal).</span><span class="sxs-lookup"><span data-stu-id="c4225-153">For more information on Airpal, see [Airpal documentation](https://github.com/airbnb/airpal#airpal).</span></span>

<span data-ttu-id="c4225-154">V této části se podíváme na postup **nainstalujte Airpal edgenode** clusteru HDInsight Hadoop, kde je již Presto nainstalována.</span><span class="sxs-lookup"><span data-stu-id="c4225-154">In this section, we look at the steps to **install Airpal on the edgenode** of an HDInsight Hadoop cluster, that already has Presto installed.</span></span> <span data-ttu-id="c4225-155">Zajistíte tak, že rozhraní Airpal webové dotazu je k dispozici přes Internet.</span><span class="sxs-lookup"><span data-stu-id="c4225-155">This ensures that the Airpal web query interface is available over the Internet.</span></span>

1. <span data-ttu-id="c4225-156">Pomocí SSH se připojte k headnode clusteru HDInsight, která má nainstalovanou Presto:</span><span class="sxs-lookup"><span data-stu-id="c4225-156">Using SSH, connect to the headnode of the HDInsight cluster that has Presto installed:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="c4225-157">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="c4225-157">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="c4225-158">Jakmile se připojíte, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="c4225-158">Once you are connected, run the following command.</span></span>

        sudo slider registry  --name presto1 --getexp presto 
   
    <span data-ttu-id="c4225-159">Zobrazený výstup by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="c4225-159">You should see an output like the following:</span></span>

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. <span data-ttu-id="c4225-160">Z výstupu, poznamenejte si hodnotu pro **hodnotu** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c4225-160">From the output, note the value for the **value** property.</span></span> <span data-ttu-id="c4225-161">Je nutné při instalaci Airpal na edgenode clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4225-161">You will need this while installing Airpal on the cluster edgenode.</span></span> <span data-ttu-id="c4225-162">Ve výstupu výš, je hodnota, kterou budete potřebovat **10.0.0.12:9090**.</span><span class="sxs-lookup"><span data-stu-id="c4225-162">From the output above, the value that you will need is **10.0.0.12:9090**.</span></span>

4. <span data-ttu-id="c4225-163">Použít šablonu  **[sem](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)**  vytvoření edgenode clusteru HDInsight a zadejte hodnoty, jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="c4225-163">Use the template **[here](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)** to create an HDInsight cluster edgenode and provide the values as shown in the following screenshot.</span></span>

    ![Instalace HDInsight Airpal na Presto clusteru](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. <span data-ttu-id="c4225-165">Klikněte na **Koupit**.</span><span class="sxs-lookup"><span data-stu-id="c4225-165">Click **Purchase**.</span></span>

6. <span data-ttu-id="c4225-166">Jakmile jsou změny v konfiguraci clusteru, můžete přístup k webové rozhraní Airpal pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="c4225-166">Once the changes are applied to the cluster configuration, you can access the Airpal web interface by using the following steps.</span></span>

    <span data-ttu-id="c4225-167">a.</span><span class="sxs-lookup"><span data-stu-id="c4225-167">a.</span></span> <span data-ttu-id="c4225-168">V okně clusteru, klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c4225-168">From the cluster blade, click **Applications**.</span></span>

    ![Spuštění HDInsight Airpal na Presto clusteru](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    <span data-ttu-id="c4225-170">b.</span><span class="sxs-lookup"><span data-stu-id="c4225-170">b.</span></span> <span data-ttu-id="c4225-171">Z **nainstalované aplikace** okně klikněte na tlačítko **portál** proti airpal.</span><span class="sxs-lookup"><span data-stu-id="c4225-171">From the **Installed Apps** blade, click **Portal** against airpal.</span></span>

    ![Spuštění HDInsight Airpal na Presto clusteru](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    <span data-ttu-id="c4225-173">c.</span><span class="sxs-lookup"><span data-stu-id="c4225-173">c.</span></span> <span data-ttu-id="c4225-174">Po zobrazení výzvy zadejte přihlašovací údaje správce, které jste zadali při vytváření clusteru HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="c4225-174">When prompted, enter the admin credentials that you specified while creating the HDInsight Hadoop cluster.</span></span>

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a><span data-ttu-id="c4225-175">Přizpůsobení Presto instalace na clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="c4225-175">Customize a Presto installation on HDInsight cluster</span></span>

<span data-ttu-id="c4225-176">Po instalaci Presto na clusteru HDInsight Hadoop, můžete přizpůsobit instalaci provést změny, jako aktualizace nastavení paměti, změnit konektorů, atd. To uděláte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="c4225-176">After you have installed Presto on an HDInsight Hadoop cluster, you can customize the installation to make changes such as update memory settings, change connectors, etc. Perform the following steps to do so.</span></span>

1. <span data-ttu-id="c4225-177">Pomocí SSH se připojte k headnode clusteru HDInsight, která má nainstalovanou Presto:</span><span class="sxs-lookup"><span data-stu-id="c4225-177">Using SSH, connect to the headnode of the HDInsight cluster that has Presto installed:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="c4225-178">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="c4225-178">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="c4225-179">Provedené změny konfigurace v souboru `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`.</span><span class="sxs-lookup"><span data-stu-id="c4225-179">Make your configuration changes in the file `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`.</span></span> <span data-ttu-id="c4225-180">Další informace o konfiguraci Presto, najdete v části [Presto konfigurace pro clustery založené na YARN](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).</span><span class="sxs-lookup"><span data-stu-id="c4225-180">For more information on Presto configuration, see [Presto configuration for YARN-based clusters](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).</span></span>

3. <span data-ttu-id="c4225-181">Zastavení a ukončení aktuální spuštěnou instanci Presto.</span><span class="sxs-lookup"><span data-stu-id="c4225-181">Stop and kill the current running instance of Presto.</span></span>

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. <span data-ttu-id="c4225-182">Spusťte novou instanci třídy Presto s úpravami.</span><span class="sxs-lookup"><span data-stu-id="c4225-182">Start a new instance of Presto with the customization.</span></span>

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. <span data-ttu-id="c4225-183">Počkejte, než pro nové instance do připravená a poznamenejte si adresu presto coordinator.</span><span class="sxs-lookup"><span data-stu-id="c4225-183">Wait for the new instance to be ready and note presto coordinator address.</span></span>


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a><span data-ttu-id="c4225-184">Generovat srovnávacího testu data pro clustery služby HDInsight se systémem Presto</span><span class="sxs-lookup"><span data-stu-id="c4225-184">Generate benchmark data for HDInsight clusters that run Presto</span></span>

<span data-ttu-id="c4225-185">TPC DS je oborový standard pro měření výkonu systémů mnoho rozhodnutí podpory, včetně systémy velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="c4225-185">TPC-DS is the industry standard for measuring the performance of many decision support systems, including big data systems.</span></span> <span data-ttu-id="c4225-186">Můžete Presto v clusterech HDInsight pro generování dat a vyhodnotit, jak se porovnává s vlastními daty srovnávacího testu HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c4225-186">You can use Presto on HDInsight clusters to generate data and evaluate how it compares with your own HDInsight benchmark data.</span></span> <span data-ttu-id="c4225-187">Další informace najdete v tématu [zde](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="c4225-187">For more information, see [here](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).</span></span>



## <a name="see-also"></a><span data-ttu-id="c4225-188">Viz také</span><span class="sxs-lookup"><span data-stu-id="c4225-188">See also</span></span>
* <span data-ttu-id="c4225-189">[Nainstalovat a používat Hue v clusterech HDInsight](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="c4225-189">[Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span> <span data-ttu-id="c4225-190">HUE je webové uživatelské rozhraní, které usnadňuje vytvoření, spuštění a uložte úlohy Pig a Hive, a také procházet výchozí úložiště pro vaše HDInsight clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4225-190">Hue is a web UI that makes it easy to create, run and save Pig and Hive jobs, as well as browse the default storage for your HDInsight cluster.</span></span>

* <span data-ttu-id="c4225-191">[Nainstalujte Giraph clustery HDInsight](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="c4225-191">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md).</span></span> <span data-ttu-id="c4225-192">Přizpůsobení clusteru použijte k instalaci Giraph clusterů systému HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="c4225-192">Use cluster customization to install Giraph on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="c4225-193">Giraph umožňuje provádět grafu zpracování pomocí Hadoop a lze použít s Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c4225-193">Giraph allows you to perform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span>

* <span data-ttu-id="c4225-194">[Nainstalujte Solr clustery HDInsight](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="c4225-194">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md).</span></span> <span data-ttu-id="c4225-195">Přizpůsobení clusteru použijte k instalaci Solr clusterů systému HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="c4225-195">Use cluster customization to install Solr on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="c4225-196">Solr umožňuje provádět operace výkonné hledání na uložená data.</span><span class="sxs-lookup"><span data-stu-id="c4225-196">Solr allows you to perform powerful search operations on stored data.</span></span>

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
