---
title: "Konfigurace replikace HBase clusteru v rámci virtuální sítě - Azure | Microsoft Docs"
description: "Postup konfigurace replikace HBase pro vyrovnávání zatížení, vysokou dostupnost, nula. výpadků migrace nebo aktualizaci z jedné verze HDInsight do druhého a zotavení po havárii."
services: hdinsight,virtual-network
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 895709391486acb4a9d7a54ef046956539913f7b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-hbase-cluster-replication-within-virtual-networks"></a><span data-ttu-id="121f3-103">Konfigurace replikace HBase clusteru v rámci virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="121f3-103">Configure HBase cluster replication within virtual networks</span></span>

<span data-ttu-id="121f3-104">Postup konfigurace replikace HBase v rámci jednu virtuální síť (VNet) nebo mezi dvě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="121f3-104">Learn how to configure HBase replication within one virtual network (VNet) or between two virtual networks.</span></span>

<span data-ttu-id="121f3-105">Clusterová replikace používá metodika nabízené zdroje.</span><span class="sxs-lookup"><span data-stu-id="121f3-105">Cluster replication uses a source-push methodology.</span></span> <span data-ttu-id="121f3-106">HBase cluster může být zdroj nebo cíl, nebo ji můžete najednou splnění obě role.</span><span class="sxs-lookup"><span data-stu-id="121f3-106">An HBase cluster can be a source or a destination, or it can fulfill both roles at once.</span></span> <span data-ttu-id="121f3-107">Replikace je asynchronní a cílem replikace je konzistence typu případné.</span><span class="sxs-lookup"><span data-stu-id="121f3-107">Replication is asynchronous, and the goal of replication is eventual consistency.</span></span> <span data-ttu-id="121f3-108">Když zdroj obdrží upravíte k rodin sloupců s povolenou replikací, že úpravy rozšířena do všech cílových clusterech.</span><span class="sxs-lookup"><span data-stu-id="121f3-108">When the source receives an edit to a column family with replication enabled, that edit is propagated to all destination clusters.</span></span> <span data-ttu-id="121f3-109">Pokud data se replikují z jednoho clusteru do druhého, zdrojovém clusteru a všechny clustery, které jste již využívat data jsou sledovány aby replikační cykly.</span><span class="sxs-lookup"><span data-stu-id="121f3-109">When data is replicated from one cluster to another, the source cluster and all clusters that have already consumed the data are tracked to prevent replication loops.</span></span>

<span data-ttu-id="121f3-110">V tomto kurzu nakonfigurujete zdroj cíl replikace.</span><span class="sxs-lookup"><span data-stu-id="121f3-110">In this tutorial, you will configure a source-destination replication.</span></span> <span data-ttu-id="121f3-111">Jiných topologiích clusteru najdete v části [referenční příručka Apache HBase](http://hbase.apache.org/book.html#_cluster_replication).</span><span class="sxs-lookup"><span data-stu-id="121f3-111">For other cluster topologies, see [Apache HBase Reference Guide](http://hbase.apache.org/book.html#_cluster_replication).</span></span>

<span data-ttu-id="121f3-112">Případy využití HBase replikace pro jedné virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="121f3-112">HBase replication usage cases for a single virtual network:</span></span>

* <span data-ttu-id="121f3-113">Vyrovnávání zatížení – například spuštění kontroly nebo úloh MapReduce v cílovém clusteru a příjem dat ve zdrojovém clusteru</span><span class="sxs-lookup"><span data-stu-id="121f3-113">Load balancing--for example, running scans or MapReduce jobs on the destination cluster and ingesting data on the source cluster</span></span>
* <span data-ttu-id="121f3-114">Vysoká dostupnost</span><span class="sxs-lookup"><span data-stu-id="121f3-114">High availability</span></span>
* <span data-ttu-id="121f3-115">Migrace dat z jednoho clusteru HBase do jiného</span><span class="sxs-lookup"><span data-stu-id="121f3-115">Migrating data from one HBase cluster to another</span></span>
* <span data-ttu-id="121f3-116">Upgrade clusteru Azure HDInsight z jedné verze do jiného</span><span class="sxs-lookup"><span data-stu-id="121f3-116">Upgrading an Azure HDInsight cluster from one version to another</span></span>

<span data-ttu-id="121f3-117">Případy využití HBase replikace pro dvě virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="121f3-117">HBase replication usage cases for two virtual networks:</span></span>

* <span data-ttu-id="121f3-118">Zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="121f3-118">Disaster recovery</span></span>
* <span data-ttu-id="121f3-119">Vyrovnávání zatížení a rozdělení do oddílů aplikace</span><span class="sxs-lookup"><span data-stu-id="121f3-119">Load balancing and partitioning the application</span></span>
* <span data-ttu-id="121f3-120">Vysoká dostupnost</span><span class="sxs-lookup"><span data-stu-id="121f3-120">High availability</span></span>

<span data-ttu-id="121f3-121">Při replikaci clustery pomocí [skript akce](hdinsight-hadoop-customize-cluster-linux.md) skripty nacházející se v [Githubu](https://github.com/Azure/hbase-utils/tree/master/replication).</span><span class="sxs-lookup"><span data-stu-id="121f3-121">You replicate clusters by using [script action](hdinsight-hadoop-customize-cluster-linux.md) scripts located at [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="121f3-122">Požadavky</span><span class="sxs-lookup"><span data-stu-id="121f3-122">Prerequisites</span></span>
<span data-ttu-id="121f3-123">Než začnete tento kurz, musíte mít předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="121f3-123">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="121f3-124">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="121f3-124">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="configure-the-environments"></a><span data-ttu-id="121f3-125">Konfigurace prostředí</span><span class="sxs-lookup"><span data-stu-id="121f3-125">Configure the environments</span></span>

<span data-ttu-id="121f3-126">Existují tři možné konfigurace:</span><span class="sxs-lookup"><span data-stu-id="121f3-126">There are three possible configurations:</span></span>

- <span data-ttu-id="121f3-127">Dva clustery HBase v jednu virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="121f3-127">Two HBase clusters in one Azure virtual network</span></span>
- <span data-ttu-id="121f3-128">Dva clustery HBase ve dvou různých virtuálních sítích ve stejné oblasti</span><span class="sxs-lookup"><span data-stu-id="121f3-128">Two HBase clusters in two different virtual networks in the same region</span></span>
- <span data-ttu-id="121f3-129">Dva clustery HBase ve dvou různých virtuálních sítích v různých oblastech dva (geografická replikace)</span><span class="sxs-lookup"><span data-stu-id="121f3-129">Two HBase clusters in two different virtual networks in two different regions (geo-replication)</span></span>

<span data-ttu-id="121f3-130">Aby bylo snazší lze konfigurovat prostředí, vytvořili jsme některé [šablon Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="121f3-130">To make it easier to configure the environments, we have created some [Azure Resource Manager templates](../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="121f3-131">Pokud dáváte přednost lze konfigurovat prostředí a použití jiných metod, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="121f3-131">If you prefer to configure the environments by using other methods, see:</span></span>

- [<span data-ttu-id="121f3-132">Vytvořit clustery se systémem Linux Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="121f3-132">Create Linux-based Hadoop clusters in HDInsight</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
- [<span data-ttu-id="121f3-133">Vytvořit clustery HBase v Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="121f3-133">Create HBase clusters in Azure Virtual Network</span></span>](hdinsight-hbase-provision-vnet.md)

### <a name="configure-one-virtual-network"></a><span data-ttu-id="121f3-134">Nakonfigurujte jednu virtuální síť</span><span class="sxs-lookup"><span data-stu-id="121f3-134">Configure one virtual network</span></span>

<span data-ttu-id="121f3-135">Kliknutím na následující obrázek vytvořili dva clustery HBase ve stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="121f3-135">Click the following image to create two HBase clusters in the same virtual network.</span></span> <span data-ttu-id="121f3-136">Šablona je uložena v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).</span><span class="sxs-lookup"><span data-stu-id="121f3-136">The template is stored in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-one-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

### <a name="configure-two-virtual-networks-in-the-same-region"></a><span data-ttu-id="121f3-137">Konfigurace dvou virtuálních sítí ve stejné oblasti</span><span class="sxs-lookup"><span data-stu-id="121f3-137">Configure two virtual networks in the same region</span></span>

<span data-ttu-id="121f3-138">Kliknutím na následující obrázek vytvořte dvě virtuální sítě pomocí virtuální sítě partnerský vztah a dva clustery HBase ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="121f3-138">Click the following image to create two virtual networks with VNet peering and two HBase clusters in the same region.</span></span> <span data-ttu-id="121f3-139">Šablona je uložena v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).</span><span class="sxs-lookup"><span data-stu-id="121f3-139">The template is stored in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-two-vnets-same-region%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>



<span data-ttu-id="121f3-140">Tento scénář vyžaduje [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="121f3-140">This scenario requires [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span> <span data-ttu-id="121f3-141">Šablona umožňuje partnerský vztah virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="121f3-141">The template enables VNet peering.</span></span>   

<span data-ttu-id="121f3-142">Replikace HBase používá IP adresy virtuálních počítačů ZooKeeper.</span><span class="sxs-lookup"><span data-stu-id="121f3-142">HBase replication uses IP addresses of the ZooKeeper VMs.</span></span> <span data-ttu-id="121f3-143">Je nutné nakonfigurovat statické IP adresy pro cílové uzly HBase ZooKeeper.</span><span class="sxs-lookup"><span data-stu-id="121f3-143">You must configure static IP addresses for the destination HBase ZooKeeper nodes.</span></span>

<span data-ttu-id="121f3-144">**Konfigurace statické IP adresy**</span><span class="sxs-lookup"><span data-stu-id="121f3-144">**To configure static IP addresses**</span></span>

1. <span data-ttu-id="121f3-145">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="121f3-145">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="121f3-146">V levé nabídce klikněte na tlačítko **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="121f3-146">From the left menu, click **Resource Groups**.</span></span>
3. <span data-ttu-id="121f3-147">Klikněte na tlačítko vaší skupiny prostředků, který obsahuje cílový cluster HBase.</span><span class="sxs-lookup"><span data-stu-id="121f3-147">Click your resource group that contains the destination HBase cluster.</span></span> <span data-ttu-id="121f3-148">Toto je skupina prostředků, které jste zadali při jste použili k vytvoření prostředí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="121f3-148">This is the resource group that you specified when you used the Resource Manager template to create the environment.</span></span> <span data-ttu-id="121f3-149">Filtr můžete použít k upřesnění seznamu.</span><span class="sxs-lookup"><span data-stu-id="121f3-149">You can use the filter to narrow down the list.</span></span> <span data-ttu-id="121f3-150">Zobrazí seznam prostředků, který obsahuje dvě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="121f3-150">You can see a list of resources that contains the two virtual networks.</span></span>
4. <span data-ttu-id="121f3-151">Klikněte na virtuální síť, která obsahuje cílový cluster HBase.</span><span class="sxs-lookup"><span data-stu-id="121f3-151">Click the virtual network that contains the destination HBase cluster.</span></span> <span data-ttu-id="121f3-152">Klikněte například na **xxxx-vnet2**.</span><span class="sxs-lookup"><span data-stu-id="121f3-152">For example, click **xxxx-vnet2**.</span></span> <span data-ttu-id="121f3-153">Zobrazí se tří zařízení s názvy, které začínají **seskupování-zookeepermode -**.</span><span class="sxs-lookup"><span data-stu-id="121f3-153">You can see three devices with names that start with **nic-zookeepermode-**.</span></span> <span data-ttu-id="121f3-154">Tato zařízení jsou tři ZooKeeper virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="121f3-154">Those devices are the three ZooKeeper VMs.</span></span>
5. <span data-ttu-id="121f3-155">Klikněte na jednu ZooKeeper virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="121f3-155">Click one of the ZooKeeper VMs.</span></span>
6. <span data-ttu-id="121f3-156">Klikněte na tlačítko **konfigurace protokolu IP**.</span><span class="sxs-lookup"><span data-stu-id="121f3-156">Click **IP configurations**.</span></span>
7. <span data-ttu-id="121f3-157">Klikněte na tlačítko **ipConfig1** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="121f3-157">Click **ipConfig1** from the list.</span></span>
8. <span data-ttu-id="121f3-158">Klikněte na tlačítko **statické**a zapište si IP adresu skutečného.</span><span class="sxs-lookup"><span data-stu-id="121f3-158">Click **Static**, and write down the actual IP address.</span></span> <span data-ttu-id="121f3-159">IP adresu budete potřebovat při spuštění akce skriptu k povolení replikace.</span><span class="sxs-lookup"><span data-stu-id="121f3-159">You will need the IP address when you run the script action to enable replication.</span></span>

  ![HDInsight HBase replikace ZooKeeper statickou IP adresu](./media/hdinsight-hbase-replication/hdinsight-hbase-replication-zookeeper-static-ip.png)

9. <span data-ttu-id="121f3-161">Zopakujte krok 6 nastavit statickou IP adresu pro další dva uzly ZooKeeper.</span><span class="sxs-lookup"><span data-stu-id="121f3-161">Repeat step 6 to set the static IP address for the other two ZooKeeper nodes.</span></span>

<span data-ttu-id="121f3-162">Pro scénář cross-VNet, je nutné použít **- ip** při volání metody přepnout **hdi_enable_replication.sh** skript akce.</span><span class="sxs-lookup"><span data-stu-id="121f3-162">For the cross-VNet scenario, you must use the **-ip** switch when calling the **hdi_enable_replication.sh** script action.</span></span>

### <a name="configure-two-virtual-networks-in-two-different-regions"></a><span data-ttu-id="121f3-163">Nakonfigurovat dvě virtuální sítě ve dvou různých oblastech</span><span class="sxs-lookup"><span data-stu-id="121f3-163">Configure two virtual networks in two different regions</span></span>

<span data-ttu-id="121f3-164">Kliknutím na následující obrázek vytvořit dvě virtuální sítě ve dvou různých oblastech.</span><span class="sxs-lookup"><span data-stu-id="121f3-164">Click the following image to create two virtual networks in two different regions.</span></span> <span data-ttu-id="121f3-165">Šablona je uložena ve veřejném kontejneru objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="121f3-165">The template is stored in a public Azure Blob container.</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fdeploy-hbase-geo-replication.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

<span data-ttu-id="121f3-166">Vytvoření brány VPN mezi dvěma virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="121f3-166">Create a VPN gateway between the two virtual networks.</span></span> <span data-ttu-id="121f3-167">Pokyny najdete v tématu [vytvoření virtuální sítě s připojením site-to-site](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="121f3-167">For instructions, see [Create a VNet with a site-to-site connection](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).</span></span>

<span data-ttu-id="121f3-168">Replikace HBase používá IP adresy virtuálních počítačů ZooKeeper.</span><span class="sxs-lookup"><span data-stu-id="121f3-168">HBase replication uses IP addresses of the ZooKeeper VMs.</span></span> <span data-ttu-id="121f3-169">Je nutné nakonfigurovat statické IP adresy pro cílové uzly HBase ZooKeeper.</span><span class="sxs-lookup"><span data-stu-id="121f3-169">You must configure static IP addresses for the destination HBase ZooKeeper nodes.</span></span> <span data-ttu-id="121f3-170">Pokud chcete nakonfigurovat statickou IP adresu, najdete v části "Konfigurace dvou virtuálních sítí ve stejné oblasti" v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="121f3-170">To configure static IP, see the "Configure two virtual networks in the same region" section in this article.</span></span>

<span data-ttu-id="121f3-171">Pro scénář cross-VNet, je nutné použít **- ip** při volání metody přepnout **hdi_enable_replication.sh** skript akce.</span><span class="sxs-lookup"><span data-stu-id="121f3-171">For the cross-VNet scenario, you must use the **-ip** switch when calling the **hdi_enable_replication.sh** script action.</span></span>

## <a name="load-test-data"></a><span data-ttu-id="121f3-172">Testovací data zatížení</span><span class="sxs-lookup"><span data-stu-id="121f3-172">Load test data</span></span>

<span data-ttu-id="121f3-173">Při replikaci cluster, je nutné zadat tabulky, které chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="121f3-173">When you replicate a cluster, you must specify the tables to replicate.</span></span> <span data-ttu-id="121f3-174">Některá data v této části se načte do zdrojového clusteru.</span><span class="sxs-lookup"><span data-stu-id="121f3-174">In this section, you will load some data into the source cluster.</span></span> <span data-ttu-id="121f3-175">V další části povolíte replikaci mezi dvěma clustery.</span><span class="sxs-lookup"><span data-stu-id="121f3-175">In the next section, you will enable replication between the two clusters.</span></span>

<span data-ttu-id="121f3-176">Postupujte podle pokynů v [kurz HBase: začněte používat Apache HBase se systémem Linux Hadoop v HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) k vytvoření **kontakty** tabulku a vložte některá data do tabulky.</span><span class="sxs-lookup"><span data-stu-id="121f3-176">Follow the instructions in [HBase tutorial: Get started using Apache HBase with Linux-based Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) to create a **Contacts** table and insert some data into the table.</span></span>

## <a name="enable-replication"></a><span data-ttu-id="121f3-177">Povolení replikace</span><span class="sxs-lookup"><span data-stu-id="121f3-177">Enable replication</span></span>

<span data-ttu-id="121f3-178">Následující kroky ukazují, jak volat skript akce skriptu z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="121f3-178">The following steps show how to call the script action script from the Azure portal.</span></span> <span data-ttu-id="121f3-179">Spuštění skriptu akci pomocí prostředí Azure PowerShell a rozhraní příkazového řádku Azure (CLI), najdete v části [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="121f3-179">For running a script action by using Azure PowerShell and the Azure command-line interface (CLI), see [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="121f3-180">**Povolení replikace HBase z portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="121f3-180">**To enable HBase replication from the Azure portal**</span></span>

1. <span data-ttu-id="121f3-181">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="121f3-181">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="121f3-182">Otevřete zdrojový cluster HBase.</span><span class="sxs-lookup"><span data-stu-id="121f3-182">Open the source HBase cluster.</span></span>
3. <span data-ttu-id="121f3-183">V nabídce clusteru, klikněte na tlačítko **akcí skriptů**.</span><span class="sxs-lookup"><span data-stu-id="121f3-183">From the cluster menu, click **Script Actions**.</span></span>
4. <span data-ttu-id="121f3-184">Klikněte na tlačítko **odeslání nové** z horní části okna.</span><span class="sxs-lookup"><span data-stu-id="121f3-184">Click **Submit New** from the top of the blade.</span></span>
5. <span data-ttu-id="121f3-185">Vyberte nebo zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="121f3-185">Select or enter the following information:</span></span>

  - <span data-ttu-id="121f3-186">**Název**: Zadejte **povolit replikaci**.</span><span class="sxs-lookup"><span data-stu-id="121f3-186">**Name**: Enter **Enable replication**.</span></span>
  - <span data-ttu-id="121f3-187">**Adresa URL skriptu bash**: Zadejte **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.</span><span class="sxs-lookup"><span data-stu-id="121f3-187">**Bash Script URL**: Enter **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.</span></span>
  - <span data-ttu-id="121f3-188">**HEAD**: vybraná.</span><span class="sxs-lookup"><span data-stu-id="121f3-188">**Head**: Selected.</span></span> <span data-ttu-id="121f3-189">Zrušte jiné typy uzlů.</span><span class="sxs-lookup"><span data-stu-id="121f3-189">Clear the other node types.</span></span>
  - <span data-ttu-id="121f3-190">**Parametry**: Následující ukázka parametry povolit replikaci pro všechny existující tabulky a zkopírujte všechny do cílového clusteru clusteru data ze zdroje:</span><span class="sxs-lookup"><span data-stu-id="121f3-190">**Parameters**: The following sample parameters enable replication for all the existing tables and copy all the data from the source cluster to the destination cluster:</span></span>

            -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata

6. <span data-ttu-id="121f3-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="121f3-191">Click **Create**.</span></span> <span data-ttu-id="121f3-192">Skript může trvat nějakou dobu, hlavně když se použije argument - programu copydata.</span><span class="sxs-lookup"><span data-stu-id="121f3-192">The script can take some time, especially when the -copydata argument is used.</span></span>

<span data-ttu-id="121f3-193">Vyžaduje argumenty:</span><span class="sxs-lookup"><span data-stu-id="121f3-193">Required arguments:</span></span>

|<span data-ttu-id="121f3-194">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="121f3-194">Name</span></span>|<span data-ttu-id="121f3-195">Popis</span><span class="sxs-lookup"><span data-stu-id="121f3-195">Description</span></span>|
|----|-----------|
|<span data-ttu-id="121f3-196">-s, – src-cluster</span><span class="sxs-lookup"><span data-stu-id="121f3-196">-s, --src-cluster</span></span> | <span data-ttu-id="121f3-197">Zadejte název DNS clusteru HBase zdroje.</span><span class="sxs-lookup"><span data-stu-id="121f3-197">Specify the DNS name of the source HBase cluster.</span></span> <span data-ttu-id="121f3-198">Příklad: hbsrccluster -s, cluster – src = hbsrccluster</span><span class="sxs-lookup"><span data-stu-id="121f3-198">For example: -s hbsrccluster, --src-cluster=hbsrccluster</span></span> |
|<span data-ttu-id="121f3-199">-d, – dst clusteru</span><span class="sxs-lookup"><span data-stu-id="121f3-199">-d, --dst-cluster</span></span> | <span data-ttu-id="121f3-200">Zadejte název DNS clusteru HBase cílové (replikovaného).</span><span class="sxs-lookup"><span data-stu-id="121f3-200">Specify the DNS name of the destination (replica) HBase cluster.</span></span> <span data-ttu-id="121f3-201">Příklad: dsthbcluster -s, cluster – src = dsthbcluster</span><span class="sxs-lookup"><span data-stu-id="121f3-201">For example: -s dsthbcluster, --src-cluster=dsthbcluster</span></span> |
|<span data-ttu-id="121f3-202">-sp, – src-ambari heslo</span><span class="sxs-lookup"><span data-stu-id="121f3-202">-sp, --src-ambari-password</span></span> | <span data-ttu-id="121f3-203">Zadejte heslo správce pro Ambari ve zdrojovém clusteru HBase.</span><span class="sxs-lookup"><span data-stu-id="121f3-203">Specify the admin password for Ambari on the source HBase cluster.</span></span> |
|<span data-ttu-id="121f3-204">-distribučního bodu, – dst-ambari heslo</span><span class="sxs-lookup"><span data-stu-id="121f3-204">-dp, --dst-ambari-password</span></span> | <span data-ttu-id="121f3-205">Zadejte heslo správce pro Ambari v cílovém clusteru HBase.</span><span class="sxs-lookup"><span data-stu-id="121f3-205">Specify the admin password for Ambari on the destination HBase cluster.</span></span>|

<span data-ttu-id="121f3-206">Nepovinné argumenty:</span><span class="sxs-lookup"><span data-stu-id="121f3-206">Optional arguments:</span></span>

|<span data-ttu-id="121f3-207">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="121f3-207">Name</span></span>|<span data-ttu-id="121f3-208">Popis</span><span class="sxs-lookup"><span data-stu-id="121f3-208">Description</span></span>|
|----|-----------|
|<span data-ttu-id="121f3-209">-su, – src-ambari-user</span><span class="sxs-lookup"><span data-stu-id="121f3-209">-su, --src-ambari-user</span></span> | <span data-ttu-id="121f3-210">Zadejte uživatelské jméno správce pro Ambari ve zdrojovém clusteru HBase.</span><span class="sxs-lookup"><span data-stu-id="121f3-210">Specify the admin username for Ambari on the source HBase cluster.</span></span> <span data-ttu-id="121f3-211">Výchozí hodnota je **správce**.</span><span class="sxs-lookup"><span data-stu-id="121f3-211">The default value is **admin**.</span></span> |
|<span data-ttu-id="121f3-212">-du – dst-ambari-user</span><span class="sxs-lookup"><span data-stu-id="121f3-212">-du, --dst-ambari-user</span></span> | <span data-ttu-id="121f3-213">Zadejte uživatelské jméno správce pro Ambari v cílovém clusteru HBase.</span><span class="sxs-lookup"><span data-stu-id="121f3-213">Specify the admin username for Ambari on the destination HBase cluster.</span></span> <span data-ttu-id="121f3-214">Výchozí hodnota je **správce**.</span><span class="sxs-lookup"><span data-stu-id="121f3-214">The default value is **admin**.</span></span> |
|<span data-ttu-id="121f3-215">-t, – seznam tabulek</span><span class="sxs-lookup"><span data-stu-id="121f3-215">-t, --table-list</span></span> | <span data-ttu-id="121f3-216">Zadejte tabulky, které chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="121f3-216">Specify the tables to be replicated.</span></span> <span data-ttu-id="121f3-217">Příklad: – seznam tabulek = "tabulky1; tabulky2; Tabulka3".</span><span class="sxs-lookup"><span data-stu-id="121f3-217">For example: --table-list="table1;table2;table3".</span></span> <span data-ttu-id="121f3-218">Pokud nezadáte tabulky, replikují se všechny existující tabulky HBase.</span><span class="sxs-lookup"><span data-stu-id="121f3-218">If you don't specify tables, all existing HBase tables are replicated.</span></span>|
|<span data-ttu-id="121f3-219">-m, – počítač</span><span class="sxs-lookup"><span data-stu-id="121f3-219">-m, --machine</span></span> | <span data-ttu-id="121f3-220">Zadejte hlavního uzlu, kde se spustí akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="121f3-220">Specify the head node where the script action will run.</span></span> <span data-ttu-id="121f3-221">Hodnota je hn1 nebo hn0.</span><span class="sxs-lookup"><span data-stu-id="121f3-221">The value is either hn1 or hn0.</span></span> <span data-ttu-id="121f3-222">Protože hn0 je obvykle Vytíženější, doporučujeme používat hn1.</span><span class="sxs-lookup"><span data-stu-id="121f3-222">Because hn0 is usually busier, we recommend using hn1.</span></span> <span data-ttu-id="121f3-223">Tuto možnost použijte, když spouštíte skript $0 jako akce skriptu z portálu HDInsight nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="121f3-223">You use this option when you're running the $0 script as a script action from the HDInsight portal or Azure PowerShell.</span></span>|
|<span data-ttu-id="121f3-224">-ip</span><span class="sxs-lookup"><span data-stu-id="121f3-224">-ip</span></span> | <span data-ttu-id="121f3-225">Pokud jste povolení replikace mezi dvěma virtuálními sítěmi, je tento argument povinný.</span><span class="sxs-lookup"><span data-stu-id="121f3-225">This argument is required when you're enabling replication between two virtual networks.</span></span> <span data-ttu-id="121f3-226">Tento argument funguje jako přepínač pro použití statické IP adresy ZooKeeper uzlů z clusterů repliky místo názvy plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="121f3-226">This argument acts as a switch to use the static IPs of ZooKeeper nodes from replica clusters instead of FQDN names.</span></span> <span data-ttu-id="121f3-227">Statické IP adresy musí být předem nakonfigurované před povolením replikace.</span><span class="sxs-lookup"><span data-stu-id="121f3-227">The static IPs need to be preconfigured before you enable replication.</span></span> |
|<span data-ttu-id="121f3-228">-prohlášení cp, - programu copydata</span><span class="sxs-lookup"><span data-stu-id="121f3-228">-cp, -copydata</span></span> | <span data-ttu-id="121f3-229">Povolte migraci existujících dat pro tabulky, kde je povolená replikace.</span><span class="sxs-lookup"><span data-stu-id="121f3-229">Enable the migration of existing data on the tables where replication is enabled.</span></span> |
|<span data-ttu-id="121f3-230">-ot. / min, - replikovat-phoenix-metadat</span><span class="sxs-lookup"><span data-stu-id="121f3-230">-rpm, -replicate-phoenix-meta</span></span> | <span data-ttu-id="121f3-231">Povolení replikace pro Phoenix systémové tabulky.</span><span class="sxs-lookup"><span data-stu-id="121f3-231">Enable replication on Phoenix system tables.</span></span> <br><br><span data-ttu-id="121f3-232">*Tuto možnost použijte opatrně.*</span><span class="sxs-lookup"><span data-stu-id="121f3-232">*Use this option with caution.*</span></span> <span data-ttu-id="121f3-233">Doporučujeme, abyste před použitím tohoto skriptu znovu vytvořit Phoenix tabulek v clusterech repliky.</span><span class="sxs-lookup"><span data-stu-id="121f3-233">We recommend that you re-create Phoenix tables on replica clusters before you use this script.</span></span> |
|<span data-ttu-id="121f3-234">-h, – Nápověda</span><span class="sxs-lookup"><span data-stu-id="121f3-234">-h, --help</span></span> | <span data-ttu-id="121f3-235">Zobrazí informace o využití.</span><span class="sxs-lookup"><span data-stu-id="121f3-235">Display usage information.</span></span> |

<span data-ttu-id="121f3-236">Části print_usage() [skriptu](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) poskytuje podrobné vysvětlení parametrů.</span><span class="sxs-lookup"><span data-stu-id="121f3-236">The print_usage() section of the [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) provides a detailed explanation of parameters.</span></span>

<span data-ttu-id="121f3-237">Po úspěšném nasazení akce skriptu, můžete pro připojení k cílový cluster HBase a ověřte, že data se replikují SSH.</span><span class="sxs-lookup"><span data-stu-id="121f3-237">After the script action is successfully deployed, you can use SSH to connect to the destination HBase cluster, and verify the data has been replicated.</span></span>

### <a name="replication-scenarios"></a><span data-ttu-id="121f3-238">Scénáře replikace</span><span class="sxs-lookup"><span data-stu-id="121f3-238">Replication scenarios</span></span>

<span data-ttu-id="121f3-239">V následujícím seznamu jsou někdy obecné využití a jejich nastavení parametrů:</span><span class="sxs-lookup"><span data-stu-id="121f3-239">The following list shows you some general usage cases and their parameter settings:</span></span>

- <span data-ttu-id="121f3-240">**Povolení replikace ve všech tabulkách mezi dvěma clustery**.</span><span class="sxs-lookup"><span data-stu-id="121f3-240">**Enable replication on all tables between the two clusters**.</span></span> <span data-ttu-id="121f3-241">Tento scénář nevyžaduje kopírování nebo migrace existujících dat v tabulkách a nepoužívá Phoenix tabulky.</span><span class="sxs-lookup"><span data-stu-id="121f3-241">This scenario does not require the copy/migration of existing data on the tables, and it does not use Phoenix tables.</span></span> <span data-ttu-id="121f3-242">Použijte následující parametry:</span><span class="sxs-lookup"><span data-stu-id="121f3-242">Use the following parameters:</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- <span data-ttu-id="121f3-243">**Povolit replikaci na konkrétní tabulky**.</span><span class="sxs-lookup"><span data-stu-id="121f3-243">**Enable replication on specific tables**.</span></span> <span data-ttu-id="121f3-244">Povolit replikaci na tabulky1 a tabulky2, tabulka3 pomocí následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="121f3-244">Use the following parameters to enable replication on table1, table2, and table3:</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- <span data-ttu-id="121f3-245">**Povolit replikaci na konkrétní tabulky a zkopírujte existující data**.</span><span class="sxs-lookup"><span data-stu-id="121f3-245">**Enable replication on specific tables and copy the existing data**.</span></span> <span data-ttu-id="121f3-246">Povolit replikaci na tabulky1 a tabulky2, tabulka3 pomocí následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="121f3-246">Use the following parameters to enable replication on table1, table2, and table3:</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- <span data-ttu-id="121f3-247">**Povolení replikace na všechny tabulky s replikací phoenix metadata ze zdrojového do cílového umístění**.</span><span class="sxs-lookup"><span data-stu-id="121f3-247">**Enable replication on all tables with replicating phoenix metadata from source to destination**.</span></span> <span data-ttu-id="121f3-248">Phoenix metadata replikace není úplně bez chyby a by měla být povolená opatrně.</span><span class="sxs-lookup"><span data-stu-id="121f3-248">Phoenix metadata replication is not perfect and should be enabled with caution.</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a><span data-ttu-id="121f3-249">Zkopírujte a migraci dat</span><span class="sxs-lookup"><span data-stu-id="121f3-249">Copy and migrate data</span></span>

<span data-ttu-id="121f3-250">Existují dvě samostatné skript akce skripty pro kopírování migraci dat po povolení replikace:</span><span class="sxs-lookup"><span data-stu-id="121f3-250">There are two separate script action scripts for copying/migrating data after replication is enabled:</span></span>

- <span data-ttu-id="121f3-251">[Skript pro malé tabulky](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (několik gigabajtů velikost a celkové kopie očekává se dokončit za méně než jednu hodinu)</span><span class="sxs-lookup"><span data-stu-id="121f3-251">[Script for small tables](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (a few gigabytes in size, and overall copy is expected to finish in less than one hour)</span></span>

- <span data-ttu-id="121f3-252">[Skript pro rozsáhlé tabulky](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (očekávaný trvá déle než jednu hodinu kopírování)</span><span class="sxs-lookup"><span data-stu-id="121f3-252">[Script for large tables](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (expected to take longer than one hour to copy)</span></span>

<span data-ttu-id="121f3-253">Můžete postupujte stejným způsobem v [povolit replikaci](#enable-replication) volání akce skriptu s následujícími parametry:</span><span class="sxs-lookup"><span data-stu-id="121f3-253">You can follow the same procedure in [Enable replication](#enable-replication) to call the script action with the following parameters:</span></span>

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

<span data-ttu-id="121f3-254">Části print_usage() [skriptu](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) poskytuje podrobný popis parametrů.</span><span class="sxs-lookup"><span data-stu-id="121f3-254">The print_usage() section of the [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) provides a detailed description of parameters.</span></span>

### <a name="scenarios"></a><span data-ttu-id="121f3-255">Scénáře</span><span class="sxs-lookup"><span data-stu-id="121f3-255">Scenarios</span></span>

- <span data-ttu-id="121f3-256">**Zkopírujte konkrétní tabulky (test1, test2 a test3) pro všechny řádky upravit do nyní (aktuální časové razítko)**:</span><span class="sxs-lookup"><span data-stu-id="121f3-256">**Copy specific tables (test1, test2, and test3) for all rows edited till now (current time stamp)**:</span></span>

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  <span data-ttu-id="121f3-257">nebo</span><span class="sxs-lookup"><span data-stu-id="121f3-257">or</span></span>

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- <span data-ttu-id="121f3-258">**Zkopírujte konkrétní tabulky s zadaný časový rozsah**:</span><span class="sxs-lookup"><span data-stu-id="121f3-258">**Copy specific tables with specified time range**:</span></span>

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a><span data-ttu-id="121f3-259">Zakázat replikaci</span><span class="sxs-lookup"><span data-stu-id="121f3-259">Disable replication</span></span>

<span data-ttu-id="121f3-260">Zakázat replikaci, použít jiný skript akce skriptu nacházející se v [Githubu](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh).</span><span class="sxs-lookup"><span data-stu-id="121f3-260">To disable replication, you use another script action script located at [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh).</span></span> <span data-ttu-id="121f3-261">Můžete postupujte stejným způsobem v [povolit replikaci](#enable-replication) volání akce skriptu s následujícími parametry:</span><span class="sxs-lookup"><span data-stu-id="121f3-261">You can follow the same procedure in [Enable replication](#enable-replication) to call the script action with the following parameters:</span></span>

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari Password> <-all|-t "table1;table2;...">  

<span data-ttu-id="121f3-262">Části print_usage() [skriptu](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) poskytuje podrobné vysvětlení parametrů.</span><span class="sxs-lookup"><span data-stu-id="121f3-262">The print_usage() section of the [script](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) provides a detailed explanation of parameters.</span></span>

### <a name="scenarios"></a><span data-ttu-id="121f3-263">Scénáře</span><span class="sxs-lookup"><span data-stu-id="121f3-263">Scenarios</span></span>

- <span data-ttu-id="121f3-264">**Zakázat replikaci ve všech tabulkách**:</span><span class="sxs-lookup"><span data-stu-id="121f3-264">**Disable replication on all tables**:</span></span>

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  <span data-ttu-id="121f3-265">nebo</span><span class="sxs-lookup"><span data-stu-id="121f3-265">or</span></span>

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari username> --src-ambari-password=<source cluster Ambari password>

- <span data-ttu-id="121f3-266">**Zakázat replikaci na zadaných tabulek (tabulky1, tabulky2 a tabulka3)**:</span><span class="sxs-lookup"><span data-stu-id="121f3-266">**Disable replication on specified tables (table1, table2, and table3)**:</span></span>

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a><span data-ttu-id="121f3-267">Další kroky</span><span class="sxs-lookup"><span data-stu-id="121f3-267">Next steps</span></span>

<span data-ttu-id="121f3-268">V tomto kurzu jste se naučili konfigurace replikace HBase přes dvě datových center.</span><span class="sxs-lookup"><span data-stu-id="121f3-268">In this tutorial, you learned how to configure HBase replication across two datacenters.</span></span> <span data-ttu-id="121f3-269">Další informace o HDInsight a HBase naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="121f3-269">To learn more about HDInsight and HBase, see:</span></span>

* <span data-ttu-id="121f3-270">[Začínáme s Apache HBase v HDInsight][hdinsight-hbase-get-started]</span><span class="sxs-lookup"><span data-stu-id="121f3-270">[Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started]</span></span>
* <span data-ttu-id="121f3-271">[Přehled HDInsight HBase][hdinsight-hbase-overview]</span><span class="sxs-lookup"><span data-stu-id="121f3-271">[HDInsight HBase overview][hdinsight-hbase-overview]</span></span>
* <span data-ttu-id="121f3-272">[Vytvořit clustery HBase v Azure Virtual Network][hdinsight-hbase-provision-vnet]</span><span class="sxs-lookup"><span data-stu-id="121f3-272">[Create HBase clusters in Azure Virtual Network][hdinsight-hbase-provision-vnet]</span></span>
* <span data-ttu-id="121f3-273">[Analýza v reálném čase sentimentu Twitter s HBase][hdinsight-hbase-twitter-sentiment]</span><span class="sxs-lookup"><span data-stu-id="121f3-273">[Analyze real-time Twitter sentiment with HBase][hdinsight-hbase-twitter-sentiment]</span></span>
* <span data-ttu-id="121f3-274">[Analýza dat snímače pomocí Storm a HBase v HDInsight (Hadoop)][hdinsight-sensor-data]</span><span class="sxs-lookup"><span data-stu-id="121f3-274">[Analyzing sensor data with Storm and HBase in HDInsight (Hadoop)][hdinsight-sensor-data]</span></span>

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started-linux.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
