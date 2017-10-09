---
title: "replikace clusteru HBase aaaConfigure v rámci virtuální sítě - Azure | Microsoft Docs"
description: "Zjistěte, jak tooconfigure replikace HBase pro vyrovnávání zatížení, vysokou dostupnost, nula. výpadků migrace nebo aktualizace z jedné verze tooanother HDInsight a zotavení po havárii."
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
ms.openlocfilehash: ba1c44f26b7cbf4a7a88159b12b3e064ea9f9a20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hbase-cluster-replication-within-virtual-networks"></a><span data-ttu-id="997a2-103">Konfigurace replikace HBase clusteru v rámci virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="997a2-103">Configure HBase cluster replication within virtual networks</span></span>

<span data-ttu-id="997a2-104">Zjistěte, jak tooconfigure HBase replikace v rámci jednu virtuální síť (VNet) nebo mezi dvě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="997a2-104">Learn how tooconfigure HBase replication within one virtual network (VNet) or between two virtual networks.</span></span>

<span data-ttu-id="997a2-105">Clusterová replikace používá metodika nabízené zdroje.</span><span class="sxs-lookup"><span data-stu-id="997a2-105">Cluster replication uses a source-push methodology.</span></span> <span data-ttu-id="997a2-106">HBase cluster může být zdroj nebo cíl, nebo ji můžete najednou splnění obě role.</span><span class="sxs-lookup"><span data-stu-id="997a2-106">An HBase cluster can be a source or a destination, or it can fulfill both roles at once.</span></span> <span data-ttu-id="997a2-107">Replikace je asynchronní a hello cílem replikace je konzistence typu případné.</span><span class="sxs-lookup"><span data-stu-id="997a2-107">Replication is asynchronous, and hello goal of replication is eventual consistency.</span></span> <span data-ttu-id="997a2-108">Když zdroj hello obdrží rodiny upravit tooa sloupec s povolenou replikací, že úpravy je šířený tooall cílových clusterech.</span><span class="sxs-lookup"><span data-stu-id="997a2-108">When hello source receives an edit tooa column family with replication enabled, that edit is propagated tooall destination clusters.</span></span> <span data-ttu-id="997a2-109">Když data se replikují z jednoho clusteru tooanother, hello zdrojovém clusteru a všechny clustery, které jste již využívat hello data jsou sledovaných tooprevent replikační cykly.</span><span class="sxs-lookup"><span data-stu-id="997a2-109">When data is replicated from one cluster tooanother, hello source cluster and all clusters that have already consumed hello data are tracked tooprevent replication loops.</span></span>

<span data-ttu-id="997a2-110">V tomto kurzu nakonfigurujete zdroj cíl replikace.</span><span class="sxs-lookup"><span data-stu-id="997a2-110">In this tutorial, you will configure a source-destination replication.</span></span> <span data-ttu-id="997a2-111">Jiných topologiích clusteru najdete v části [referenční příručka Apache HBase](http://hbase.apache.org/book.html#_cluster_replication).</span><span class="sxs-lookup"><span data-stu-id="997a2-111">For other cluster topologies, see [Apache HBase Reference Guide](http://hbase.apache.org/book.html#_cluster_replication).</span></span>

<span data-ttu-id="997a2-112">Případy využití HBase replikace pro jedné virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="997a2-112">HBase replication usage cases for a single virtual network:</span></span>

* <span data-ttu-id="997a2-113">Vyrovnávání zatížení – například spuštění kontroly nebo úloh MapReduce na hello cílový cluster a příjem dat na zdrojovém clusteru hello</span><span class="sxs-lookup"><span data-stu-id="997a2-113">Load balancing--for example, running scans or MapReduce jobs on hello destination cluster and ingesting data on hello source cluster</span></span>
* <span data-ttu-id="997a2-114">Vysoká dostupnost</span><span class="sxs-lookup"><span data-stu-id="997a2-114">High availability</span></span>
* <span data-ttu-id="997a2-115">Migrace dat z jednoho clusteru tooanother HBase</span><span class="sxs-lookup"><span data-stu-id="997a2-115">Migrating data from one HBase cluster tooanother</span></span>
* <span data-ttu-id="997a2-116">Upgrade clusteru Azure HDInsight z jedné verze tooanother</span><span class="sxs-lookup"><span data-stu-id="997a2-116">Upgrading an Azure HDInsight cluster from one version tooanother</span></span>

<span data-ttu-id="997a2-117">Případy využití HBase replikace pro dvě virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="997a2-117">HBase replication usage cases for two virtual networks:</span></span>

* <span data-ttu-id="997a2-118">Zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="997a2-118">Disaster recovery</span></span>
* <span data-ttu-id="997a2-119">Vyrovnávání zatížení a rozdělení do oddílů aplikace hello</span><span class="sxs-lookup"><span data-stu-id="997a2-119">Load balancing and partitioning hello application</span></span>
* <span data-ttu-id="997a2-120">Vysoká dostupnost</span><span class="sxs-lookup"><span data-stu-id="997a2-120">High availability</span></span>

<span data-ttu-id="997a2-121">Při replikaci clustery pomocí [skript akce](hdinsight-hadoop-customize-cluster-linux.md) skripty nacházející se v [Githubu](https://github.com/Azure/hbase-utils/tree/master/replication).</span><span class="sxs-lookup"><span data-stu-id="997a2-121">You replicate clusters by using [script action](hdinsight-hadoop-customize-cluster-linux.md) scripts located at [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="997a2-122">Požadavky</span><span class="sxs-lookup"><span data-stu-id="997a2-122">Prerequisites</span></span>
<span data-ttu-id="997a2-123">Než začnete tento kurz, musíte mít předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="997a2-123">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="997a2-124">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="997a2-124">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="configure-hello-environments"></a><span data-ttu-id="997a2-125">Konfigurace prostředí hello</span><span class="sxs-lookup"><span data-stu-id="997a2-125">Configure hello environments</span></span>

<span data-ttu-id="997a2-126">Existují tři možné konfigurace:</span><span class="sxs-lookup"><span data-stu-id="997a2-126">There are three possible configurations:</span></span>

- <span data-ttu-id="997a2-127">Dva clustery HBase v jednu virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="997a2-127">Two HBase clusters in one Azure virtual network</span></span>
- <span data-ttu-id="997a2-128">Dva clustery HBase ve dvou různých virtuálních sítích v hello stejné oblasti</span><span class="sxs-lookup"><span data-stu-id="997a2-128">Two HBase clusters in two different virtual networks in hello same region</span></span>
- <span data-ttu-id="997a2-129">Dva clustery HBase ve dvou různých virtuálních sítích v různých oblastech dva (geografická replikace)</span><span class="sxs-lookup"><span data-stu-id="997a2-129">Two HBase clusters in two different virtual networks in two different regions (geo-replication)</span></span>

<span data-ttu-id="997a2-130">toomake je snazší tooconfigure hello prostředí, vytvořili jsme některé [šablon Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="997a2-130">toomake it easier tooconfigure hello environments, we have created some [Azure Resource Manager templates](../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="997a2-131">Pokud dáváte přednost tooconfigure hello prostředí pomocí jiné metody, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="997a2-131">If you prefer tooconfigure hello environments by using other methods, see:</span></span>

- [<span data-ttu-id="997a2-132">Vytvořit clustery se systémem Linux Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="997a2-132">Create Linux-based Hadoop clusters in HDInsight</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
- [<span data-ttu-id="997a2-133">Vytvořit clustery HBase v Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="997a2-133">Create HBase clusters in Azure Virtual Network</span></span>](hdinsight-hbase-provision-vnet.md)

### <a name="configure-one-virtual-network"></a><span data-ttu-id="997a2-134">Nakonfigurujte jednu virtuální síť</span><span class="sxs-lookup"><span data-stu-id="997a2-134">Configure one virtual network</span></span>

<span data-ttu-id="997a2-135">Klikněte na následující obrázek toocreate dvěma HBase clustery v hello hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="997a2-135">Click hello following image toocreate two HBase clusters in hello same virtual network.</span></span> <span data-ttu-id="997a2-136">Šablona Hello je uložena v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).</span><span class="sxs-lookup"><span data-stu-id="997a2-136">hello template is stored in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-one-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>

### <a name="configure-two-virtual-networks-in-hello-same-region"></a><span data-ttu-id="997a2-137">Nakonfigurovat dvě virtuální sítě v hello stejné oblasti</span><span class="sxs-lookup"><span data-stu-id="997a2-137">Configure two virtual networks in hello same region</span></span>

<span data-ttu-id="997a2-138">Klikněte na následující obrázek toocreate dvě virtuální sítě pomocí virtuální sítě partnerský vztah a dva clustery HBase v hello hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="997a2-138">Click hello following image toocreate two virtual networks with VNet peering and two HBase clusters in hello same region.</span></span> <span data-ttu-id="997a2-139">Šablona Hello je uložena v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).</span><span class="sxs-lookup"><span data-stu-id="997a2-139">hello template is stored in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-two-vnets-same-region%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>



<span data-ttu-id="997a2-140">Tento scénář vyžaduje [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="997a2-140">This scenario requires [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span> <span data-ttu-id="997a2-141">Šablona Hello umožňuje partnerský vztah virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="997a2-141">hello template enables VNet peering.</span></span>   

<span data-ttu-id="997a2-142">Replikace HBase používá IP adresy hello ZooKeeper virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="997a2-142">HBase replication uses IP addresses of hello ZooKeeper VMs.</span></span> <span data-ttu-id="997a2-143">Je nutné nakonfigurovat statické IP adresy pro uzly HBase ZooKeeper cílové hello.</span><span class="sxs-lookup"><span data-stu-id="997a2-143">You must configure static IP addresses for hello destination HBase ZooKeeper nodes.</span></span>

<span data-ttu-id="997a2-144">**tooconfigure statické IP adresy**</span><span class="sxs-lookup"><span data-stu-id="997a2-144">**tooconfigure static IP addresses**</span></span>

1. <span data-ttu-id="997a2-145">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="997a2-145">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="997a2-146">V levé nabídce hello, klikněte na **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="997a2-146">From hello left menu, click **Resource Groups**.</span></span>
3. <span data-ttu-id="997a2-147">Klepněte na skupinu prostředků obsahující cluster HBase cílové hello.</span><span class="sxs-lookup"><span data-stu-id="997a2-147">Click your resource group that contains hello destination HBase cluster.</span></span> <span data-ttu-id="997a2-148">Toto je hello skupinu prostředků, který jste zadali při použití hello Resource Manager šablony toocreate hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="997a2-148">This is hello resource group that you specified when you used hello Resource Manager template toocreate hello environment.</span></span> <span data-ttu-id="997a2-149">Můžete použít filtr toonarrow hello dolů hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="997a2-149">You can use hello filter toonarrow down hello list.</span></span> <span data-ttu-id="997a2-150">Zobrazí seznam prostředků, který obsahuje hello dvě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="997a2-150">You can see a list of resources that contains hello two virtual networks.</span></span>
4. <span data-ttu-id="997a2-151">Klikněte na tlačítko hello virtuální síť, která obsahuje clusteru HBase cílové hello.</span><span class="sxs-lookup"><span data-stu-id="997a2-151">Click hello virtual network that contains hello destination HBase cluster.</span></span> <span data-ttu-id="997a2-152">Klikněte například na **xxxx-vnet2**.</span><span class="sxs-lookup"><span data-stu-id="997a2-152">For example, click **xxxx-vnet2**.</span></span> <span data-ttu-id="997a2-153">Zobrazí se tří zařízení s názvy, které začínají **seskupování-zookeepermode -**.</span><span class="sxs-lookup"><span data-stu-id="997a2-153">You can see three devices with names that start with **nic-zookeepermode-**.</span></span> <span data-ttu-id="997a2-154">Tato zařízení jsou ZooKeeper hello tři virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="997a2-154">Those devices are hello three ZooKeeper VMs.</span></span>
5. <span data-ttu-id="997a2-155">Klikněte na jednu z hello ZooKeeper virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="997a2-155">Click one of hello ZooKeeper VMs.</span></span>
6. <span data-ttu-id="997a2-156">Klikněte na tlačítko **konfigurace protokolu IP**.</span><span class="sxs-lookup"><span data-stu-id="997a2-156">Click **IP configurations**.</span></span>
7. <span data-ttu-id="997a2-157">Klikněte na tlačítko **ipConfig1** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="997a2-157">Click **ipConfig1** from hello list.</span></span>
8. <span data-ttu-id="997a2-158">Klikněte na tlačítko **statické**a zapište si IP adresu skutečného hello.</span><span class="sxs-lookup"><span data-stu-id="997a2-158">Click **Static**, and write down hello actual IP address.</span></span> <span data-ttu-id="997a2-159">Když spustíte hello skript akce tooenable replikace, budete potřebovat hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="997a2-159">You will need hello IP address when you run hello script action tooenable replication.</span></span>

  ![HDInsight HBase replikace ZooKeeper statickou IP adresu](./media/hdinsight-hbase-replication/hdinsight-hbase-replication-zookeeper-static-ip.png)

9. <span data-ttu-id="997a2-161">Zopakujte krok 6 tooset hello statickou IP adresu pro hello další dva uzly ZooKeeper.</span><span class="sxs-lookup"><span data-stu-id="997a2-161">Repeat step 6 tooset hello static IP address for hello other two ZooKeeper nodes.</span></span>

<span data-ttu-id="997a2-162">Pro scénář hello cross-VNet, je nutné použít hello **- ip** přepínače při volání metody hello **hdi_enable_replication.sh** skript akce.</span><span class="sxs-lookup"><span data-stu-id="997a2-162">For hello cross-VNet scenario, you must use hello **-ip** switch when calling hello **hdi_enable_replication.sh** script action.</span></span>

### <a name="configure-two-virtual-networks-in-two-different-regions"></a><span data-ttu-id="997a2-163">Nakonfigurovat dvě virtuální sítě ve dvou různých oblastech</span><span class="sxs-lookup"><span data-stu-id="997a2-163">Configure two virtual networks in two different regions</span></span>

<span data-ttu-id="997a2-164">Klikněte na tlačítko hello následující bitové kopie toocreate dvě virtuální sítě ve dvou různých oblastech.</span><span class="sxs-lookup"><span data-stu-id="997a2-164">Click hello following image toocreate two virtual networks in two different regions.</span></span> <span data-ttu-id="997a2-165">Šablona Hello je uložena ve veřejném kontejneru objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="997a2-165">hello template is stored in a public Azure Blob container.</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fdeploy-hbase-geo-replication.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>

<span data-ttu-id="997a2-166">Vytvoření brány VPN mezi dvěma virtuálními sítěmi hello.</span><span class="sxs-lookup"><span data-stu-id="997a2-166">Create a VPN gateway between hello two virtual networks.</span></span> <span data-ttu-id="997a2-167">Pokyny najdete v tématu [vytvoření virtuální sítě s připojením site-to-site](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="997a2-167">For instructions, see [Create a VNet with a site-to-site connection](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).</span></span>

<span data-ttu-id="997a2-168">Replikace HBase používá IP adresy hello ZooKeeper virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="997a2-168">HBase replication uses IP addresses of hello ZooKeeper VMs.</span></span> <span data-ttu-id="997a2-169">Je nutné nakonfigurovat statické IP adresy pro uzly HBase ZooKeeper cílové hello.</span><span class="sxs-lookup"><span data-stu-id="997a2-169">You must configure static IP addresses for hello destination HBase ZooKeeper nodes.</span></span> <span data-ttu-id="997a2-170">tooconfigure statické IP adresy, najdete v části "hello konfigurace dvou virtuálních sítí ve stejné oblasti" hello v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="997a2-170">tooconfigure static IP, see hello "Configure two virtual networks in hello same region" section in this article.</span></span>

<span data-ttu-id="997a2-171">Pro scénář hello cross-VNet, je nutné použít hello **- ip** přepínače při volání metody hello **hdi_enable_replication.sh** skript akce.</span><span class="sxs-lookup"><span data-stu-id="997a2-171">For hello cross-VNet scenario, you must use hello **-ip** switch when calling hello **hdi_enable_replication.sh** script action.</span></span>

## <a name="load-test-data"></a><span data-ttu-id="997a2-172">Testovací data zatížení</span><span class="sxs-lookup"><span data-stu-id="997a2-172">Load test data</span></span>

<span data-ttu-id="997a2-173">Při replikaci cluster, je nutné zadat tooreplicate tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="997a2-173">When you replicate a cluster, you must specify hello tables tooreplicate.</span></span> <span data-ttu-id="997a2-174">Některá data v této části se načte do clusteru s podporou hello.</span><span class="sxs-lookup"><span data-stu-id="997a2-174">In this section, you will load some data into hello source cluster.</span></span> <span data-ttu-id="997a2-175">V další části hello povolíte replikaci mezi dvěma clustery s hello.</span><span class="sxs-lookup"><span data-stu-id="997a2-175">In hello next section, you will enable replication between hello two clusters.</span></span>

<span data-ttu-id="997a2-176">Postupujte podle pokynů hello v [kurz HBase: začněte používat Apache HBase se systémem Linux Hadoop v HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) toocreate **kontakty** tabulku a vložte některá data do tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="997a2-176">Follow hello instructions in [HBase tutorial: Get started using Apache HBase with Linux-based Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) toocreate a **Contacts** table and insert some data into hello table.</span></span>

## <a name="enable-replication"></a><span data-ttu-id="997a2-177">Povolení replikace</span><span class="sxs-lookup"><span data-stu-id="997a2-177">Enable replication</span></span>

<span data-ttu-id="997a2-178">Hello následující kroky ukazují, jak toocall hello skript akce skriptu z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="997a2-178">hello following steps show how toocall hello script action script from hello Azure portal.</span></span> <span data-ttu-id="997a2-179">Spuštění skriptu akci pomocí prostředí Azure PowerShell a hello rozhraní příkazového řádku Azure (CLI), najdete v části [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="997a2-179">For running a script action by using Azure PowerShell and hello Azure command-line interface (CLI), see [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="997a2-180">**replikace HBase tooenable z hello portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="997a2-180">**tooenable HBase replication from hello Azure portal**</span></span>

1. <span data-ttu-id="997a2-181">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="997a2-181">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="997a2-182">Otevřete cluster HBase hello zdroje.</span><span class="sxs-lookup"><span data-stu-id="997a2-182">Open hello source HBase cluster.</span></span>
3. <span data-ttu-id="997a2-183">Z nabídky hello clusteru, klikněte na tlačítko **akcí skriptů**.</span><span class="sxs-lookup"><span data-stu-id="997a2-183">From hello cluster menu, click **Script Actions**.</span></span>
4. <span data-ttu-id="997a2-184">Klikněte na tlačítko **odeslání nové** z hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="997a2-184">Click **Submit New** from hello top of hello blade.</span></span>
5. <span data-ttu-id="997a2-185">Vyberte nebo zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="997a2-185">Select or enter hello following information:</span></span>

  - <span data-ttu-id="997a2-186">**Název**: Zadejte **povolit replikaci**.</span><span class="sxs-lookup"><span data-stu-id="997a2-186">**Name**: Enter **Enable replication**.</span></span>
  - <span data-ttu-id="997a2-187">**Adresa URL skriptu bash**: Zadejte **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.</span><span class="sxs-lookup"><span data-stu-id="997a2-187">**Bash Script URL**: Enter **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.</span></span>
  - <span data-ttu-id="997a2-188">**HEAD**: vybraná.</span><span class="sxs-lookup"><span data-stu-id="997a2-188">**Head**: Selected.</span></span> <span data-ttu-id="997a2-189">Zrušte hello jiné typy uzlů.</span><span class="sxs-lookup"><span data-stu-id="997a2-189">Clear hello other node types.</span></span>
  - <span data-ttu-id="997a2-190">**Parametry**: hello následující ukázkové parametry povolení replikace pro všechny existující tabulky hello a zkopírovat všechna data hello z hello zdrojového clusteru toohello cílového clusteru:</span><span class="sxs-lookup"><span data-stu-id="997a2-190">**Parameters**: hello following sample parameters enable replication for all hello existing tables and copy all hello data from hello source cluster toohello destination cluster:</span></span>

            -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata

6. <span data-ttu-id="997a2-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="997a2-191">Click **Create**.</span></span> <span data-ttu-id="997a2-192">skript Hello může trvat nějakou dobu, hlavně když se použije argument - programu copydata hello.</span><span class="sxs-lookup"><span data-stu-id="997a2-192">hello script can take some time, especially when hello -copydata argument is used.</span></span>

<span data-ttu-id="997a2-193">Vyžaduje argumenty:</span><span class="sxs-lookup"><span data-stu-id="997a2-193">Required arguments:</span></span>

|<span data-ttu-id="997a2-194">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="997a2-194">Name</span></span>|<span data-ttu-id="997a2-195">Popis</span><span class="sxs-lookup"><span data-stu-id="997a2-195">Description</span></span>|
|----|-----------|
|<span data-ttu-id="997a2-196">-s, – src-cluster</span><span class="sxs-lookup"><span data-stu-id="997a2-196">-s, --src-cluster</span></span> | <span data-ttu-id="997a2-197">Zadejte název DNS hello cluster HBase hello zdroje.</span><span class="sxs-lookup"><span data-stu-id="997a2-197">Specify hello DNS name of hello source HBase cluster.</span></span> <span data-ttu-id="997a2-198">Příklad: hbsrccluster -s, cluster – src = hbsrccluster</span><span class="sxs-lookup"><span data-stu-id="997a2-198">For example: -s hbsrccluster, --src-cluster=hbsrccluster</span></span> |
|<span data-ttu-id="997a2-199">-d, – dst clusteru</span><span class="sxs-lookup"><span data-stu-id="997a2-199">-d, --dst-cluster</span></span> | <span data-ttu-id="997a2-200">Zadejte název DNS hello hello cíle (replikovaného) HBase cluster.</span><span class="sxs-lookup"><span data-stu-id="997a2-200">Specify hello DNS name of hello destination (replica) HBase cluster.</span></span> <span data-ttu-id="997a2-201">Příklad: dsthbcluster -s, cluster – src = dsthbcluster</span><span class="sxs-lookup"><span data-stu-id="997a2-201">For example: -s dsthbcluster, --src-cluster=dsthbcluster</span></span> |
|<span data-ttu-id="997a2-202">-sp, – src-ambari heslo</span><span class="sxs-lookup"><span data-stu-id="997a2-202">-sp, --src-ambari-password</span></span> | <span data-ttu-id="997a2-203">Zadejte heslo správce hello pro Ambari na cluster HBase hello zdroje.</span><span class="sxs-lookup"><span data-stu-id="997a2-203">Specify hello admin password for Ambari on hello source HBase cluster.</span></span> |
|<span data-ttu-id="997a2-204">-distribučního bodu, – dst-ambari heslo</span><span class="sxs-lookup"><span data-stu-id="997a2-204">-dp, --dst-ambari-password</span></span> | <span data-ttu-id="997a2-205">Zadejte heslo správce hello pro Ambari na cluster HBase cílové hello.</span><span class="sxs-lookup"><span data-stu-id="997a2-205">Specify hello admin password for Ambari on hello destination HBase cluster.</span></span>|

<span data-ttu-id="997a2-206">Nepovinné argumenty:</span><span class="sxs-lookup"><span data-stu-id="997a2-206">Optional arguments:</span></span>

|<span data-ttu-id="997a2-207">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="997a2-207">Name</span></span>|<span data-ttu-id="997a2-208">Popis</span><span class="sxs-lookup"><span data-stu-id="997a2-208">Description</span></span>|
|----|-----------|
|<span data-ttu-id="997a2-209">-su, – src-ambari-user</span><span class="sxs-lookup"><span data-stu-id="997a2-209">-su, --src-ambari-user</span></span> | <span data-ttu-id="997a2-210">Zadejte uživatelské jméno správce hello pro Ambari na cluster HBase hello zdroje.</span><span class="sxs-lookup"><span data-stu-id="997a2-210">Specify hello admin username for Ambari on hello source HBase cluster.</span></span> <span data-ttu-id="997a2-211">Hello výchozí hodnota je **správce**.</span><span class="sxs-lookup"><span data-stu-id="997a2-211">hello default value is **admin**.</span></span> |
|<span data-ttu-id="997a2-212">-du – dst-ambari-user</span><span class="sxs-lookup"><span data-stu-id="997a2-212">-du, --dst-ambari-user</span></span> | <span data-ttu-id="997a2-213">Zadejte uživatelské jméno správce hello pro Ambari na cluster HBase cílové hello.</span><span class="sxs-lookup"><span data-stu-id="997a2-213">Specify hello admin username for Ambari on hello destination HBase cluster.</span></span> <span data-ttu-id="997a2-214">Hello výchozí hodnota je **správce**.</span><span class="sxs-lookup"><span data-stu-id="997a2-214">hello default value is **admin**.</span></span> |
|<span data-ttu-id="997a2-215">-t, – seznam tabulek</span><span class="sxs-lookup"><span data-stu-id="997a2-215">-t, --table-list</span></span> | <span data-ttu-id="997a2-216">Zadejte hello tabulky toobe replikovat.</span><span class="sxs-lookup"><span data-stu-id="997a2-216">Specify hello tables toobe replicated.</span></span> <span data-ttu-id="997a2-217">Příklad: – seznam tabulek = "tabulky1; tabulky2; Tabulka3".</span><span class="sxs-lookup"><span data-stu-id="997a2-217">For example: --table-list="table1;table2;table3".</span></span> <span data-ttu-id="997a2-218">Pokud nezadáte tabulky, replikují se všechny existující tabulky HBase.</span><span class="sxs-lookup"><span data-stu-id="997a2-218">If you don't specify tables, all existing HBase tables are replicated.</span></span>|
|<span data-ttu-id="997a2-219">-m, – počítač</span><span class="sxs-lookup"><span data-stu-id="997a2-219">-m, --machine</span></span> | <span data-ttu-id="997a2-220">Zadejte hello hlavního uzlu, kde se spustí akce skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="997a2-220">Specify hello head node where hello script action will run.</span></span> <span data-ttu-id="997a2-221">Hodnota Hello je hn1 nebo hn0.</span><span class="sxs-lookup"><span data-stu-id="997a2-221">hello value is either hn1 or hn0.</span></span> <span data-ttu-id="997a2-222">Protože hn0 je obvykle Vytíženější, doporučujeme používat hn1.</span><span class="sxs-lookup"><span data-stu-id="997a2-222">Because hn0 is usually busier, we recommend using hn1.</span></span> <span data-ttu-id="997a2-223">Tuto možnost použijte, když spouštíte skript hello $0 jako akce skriptu z portálu hello HDInsight nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="997a2-223">You use this option when you're running hello $0 script as a script action from hello HDInsight portal or Azure PowerShell.</span></span>|
|<span data-ttu-id="997a2-224">-ip</span><span class="sxs-lookup"><span data-stu-id="997a2-224">-ip</span></span> | <span data-ttu-id="997a2-225">Pokud jste povolení replikace mezi dvěma virtuálními sítěmi, je tento argument povinný.</span><span class="sxs-lookup"><span data-stu-id="997a2-225">This argument is required when you're enabling replication between two virtual networks.</span></span> <span data-ttu-id="997a2-226">Tento argument funguje jako přepínač toouse hello statické IP adresy ZooKeeper uzly z repliky clusterů namísto názvů plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="997a2-226">This argument acts as a switch toouse hello static IPs of ZooKeeper nodes from replica clusters instead of FQDN names.</span></span> <span data-ttu-id="997a2-227">Hello statické IP adresy musí toobe předkonfigurované před povolením replikace.</span><span class="sxs-lookup"><span data-stu-id="997a2-227">hello static IPs need toobe preconfigured before you enable replication.</span></span> |
|<span data-ttu-id="997a2-228">-prohlášení cp, - programu copydata</span><span class="sxs-lookup"><span data-stu-id="997a2-228">-cp, -copydata</span></span> | <span data-ttu-id="997a2-229">Povolte hello migrace existujících dat na hello tabulky, kde je povolená replikace.</span><span class="sxs-lookup"><span data-stu-id="997a2-229">Enable hello migration of existing data on hello tables where replication is enabled.</span></span> |
|<span data-ttu-id="997a2-230">-ot. / min, - replikovat-phoenix-metadat</span><span class="sxs-lookup"><span data-stu-id="997a2-230">-rpm, -replicate-phoenix-meta</span></span> | <span data-ttu-id="997a2-231">Povolení replikace pro Phoenix systémové tabulky.</span><span class="sxs-lookup"><span data-stu-id="997a2-231">Enable replication on Phoenix system tables.</span></span> <br><br><span data-ttu-id="997a2-232">*Tuto možnost použijte opatrně.*</span><span class="sxs-lookup"><span data-stu-id="997a2-232">*Use this option with caution.*</span></span> <span data-ttu-id="997a2-233">Doporučujeme, abyste před použitím tohoto skriptu znovu vytvořit Phoenix tabulek v clusterech repliky.</span><span class="sxs-lookup"><span data-stu-id="997a2-233">We recommend that you re-create Phoenix tables on replica clusters before you use this script.</span></span> |
|<span data-ttu-id="997a2-234">-h, – Nápověda</span><span class="sxs-lookup"><span data-stu-id="997a2-234">-h, --help</span></span> | <span data-ttu-id="997a2-235">Zobrazí informace o využití.</span><span class="sxs-lookup"><span data-stu-id="997a2-235">Display usage information.</span></span> |

<span data-ttu-id="997a2-236">Hello print_usage() části hello [skriptu](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) poskytuje podrobné vysvětlení parametrů.</span><span class="sxs-lookup"><span data-stu-id="997a2-236">hello print_usage() section of hello [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) provides a detailed explanation of parameters.</span></span>

<span data-ttu-id="997a2-237">Po akce skriptu hello je úspěšně nasazen, můžete použít cluster HBase cílové toohello tooconnect SSH a ověřte, zda replikoval hello data.</span><span class="sxs-lookup"><span data-stu-id="997a2-237">After hello script action is successfully deployed, you can use SSH tooconnect toohello destination HBase cluster, and verify hello data has been replicated.</span></span>

### <a name="replication-scenarios"></a><span data-ttu-id="997a2-238">Scénáře replikace</span><span class="sxs-lookup"><span data-stu-id="997a2-238">Replication scenarios</span></span>

<span data-ttu-id="997a2-239">Hello následující seznam uvádí jste někdy obecné využití a jejich nastavení parametrů:</span><span class="sxs-lookup"><span data-stu-id="997a2-239">hello following list shows you some general usage cases and their parameter settings:</span></span>

- <span data-ttu-id="997a2-240">**Povolení replikace ve všech tabulkách mezi hello dva clustery**.</span><span class="sxs-lookup"><span data-stu-id="997a2-240">**Enable replication on all tables between hello two clusters**.</span></span> <span data-ttu-id="997a2-241">Tento scénář nevyžaduje hello kopírování nebo migrace existujících dat v tabulkách hello a nepoužívá Phoenix tabulky.</span><span class="sxs-lookup"><span data-stu-id="997a2-241">This scenario does not require hello copy/migration of existing data on hello tables, and it does not use Phoenix tables.</span></span> <span data-ttu-id="997a2-242">Použijte hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="997a2-242">Use hello following parameters:</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- <span data-ttu-id="997a2-243">**Povolit replikaci na konkrétní tabulky**.</span><span class="sxs-lookup"><span data-stu-id="997a2-243">**Enable replication on specific tables**.</span></span> <span data-ttu-id="997a2-244">Použijte následující parametry tooenable replikace na tabulky1 a tabulky2, tabulka3 hello:</span><span class="sxs-lookup"><span data-stu-id="997a2-244">Use hello following parameters tooenable replication on table1, table2, and table3:</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- <span data-ttu-id="997a2-245">**Povolit replikaci na konkrétní tabulky a zkopírujte existující data hello**.</span><span class="sxs-lookup"><span data-stu-id="997a2-245">**Enable replication on specific tables and copy hello existing data**.</span></span> <span data-ttu-id="997a2-246">Použijte následující parametry tooenable replikace na tabulky1 a tabulky2, tabulka3 hello:</span><span class="sxs-lookup"><span data-stu-id="997a2-246">Use hello following parameters tooenable replication on table1, table2, and table3:</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- <span data-ttu-id="997a2-247">**Povolení replikace na všechny tabulky s replikací phoenix metadat ze zdroje toodestination**.</span><span class="sxs-lookup"><span data-stu-id="997a2-247">**Enable replication on all tables with replicating phoenix metadata from source toodestination**.</span></span> <span data-ttu-id="997a2-248">Phoenix metadata replikace není úplně bez chyby a by měla být povolená opatrně.</span><span class="sxs-lookup"><span data-stu-id="997a2-248">Phoenix metadata replication is not perfect and should be enabled with caution.</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a><span data-ttu-id="997a2-249">Zkopírujte a migraci dat</span><span class="sxs-lookup"><span data-stu-id="997a2-249">Copy and migrate data</span></span>

<span data-ttu-id="997a2-250">Existují dvě samostatné skript akce skripty pro kopírování migraci dat po povolení replikace:</span><span class="sxs-lookup"><span data-stu-id="997a2-250">There are two separate script action scripts for copying/migrating data after replication is enabled:</span></span>

- <span data-ttu-id="997a2-251">[Skript pro malé tabulky](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (několik gigabajtů velikost a celkové kopie je očekávané toofinish za méně než jednu hodinu)</span><span class="sxs-lookup"><span data-stu-id="997a2-251">[Script for small tables](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (a few gigabytes in size, and overall copy is expected toofinish in less than one hour)</span></span>

- <span data-ttu-id="997a2-252">[Skript pro rozsáhlé tabulky](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (očekávaný tootake déle než jednu hodinu toocopy)</span><span class="sxs-lookup"><span data-stu-id="997a2-252">[Script for large tables](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (expected tootake longer than one hour toocopy)</span></span>

<span data-ttu-id="997a2-253">Můžete postupovat podle hello stejným postupem v [povolit replikaci](#enable-replication) toocall hello akce skriptu s hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="997a2-253">You can follow hello same procedure in [Enable replication](#enable-replication) toocall hello script action with hello following parameters:</span></span>

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

<span data-ttu-id="997a2-254">Hello print_usage() části hello [skriptu](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) poskytuje podrobný popis parametrů.</span><span class="sxs-lookup"><span data-stu-id="997a2-254">hello print_usage() section of hello [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) provides a detailed description of parameters.</span></span>

### <a name="scenarios"></a><span data-ttu-id="997a2-255">Scénáře</span><span class="sxs-lookup"><span data-stu-id="997a2-255">Scenarios</span></span>

- <span data-ttu-id="997a2-256">**Zkopírujte konkrétní tabulky (test1, test2 a test3) pro všechny řádky upravit do nyní (aktuální časové razítko)**:</span><span class="sxs-lookup"><span data-stu-id="997a2-256">**Copy specific tables (test1, test2, and test3) for all rows edited till now (current time stamp)**:</span></span>

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  <span data-ttu-id="997a2-257">nebo</span><span class="sxs-lookup"><span data-stu-id="997a2-257">or</span></span>

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- <span data-ttu-id="997a2-258">**Zkopírujte konkrétní tabulky s zadaný časový rozsah**:</span><span class="sxs-lookup"><span data-stu-id="997a2-258">**Copy specific tables with specified time range**:</span></span>

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a><span data-ttu-id="997a2-259">Zakázat replikaci</span><span class="sxs-lookup"><span data-stu-id="997a2-259">Disable replication</span></span>

<span data-ttu-id="997a2-260">toodisable replikace, použijte jiný skript akce skriptu nacházející se v [Githubu](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh).</span><span class="sxs-lookup"><span data-stu-id="997a2-260">toodisable replication, you use another script action script located at [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh).</span></span> <span data-ttu-id="997a2-261">Můžete postupovat podle hello stejným postupem v [povolit replikaci](#enable-replication) toocall hello akce skriptu s hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="997a2-261">You can follow hello same procedure in [Enable replication](#enable-replication) toocall hello script action with hello following parameters:</span></span>

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari Password> <-all|-t "table1;table2;...">  

<span data-ttu-id="997a2-262">Hello print_usage() části hello [skriptu](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) poskytuje podrobné vysvětlení parametrů.</span><span class="sxs-lookup"><span data-stu-id="997a2-262">hello print_usage() section of hello [script](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) provides a detailed explanation of parameters.</span></span>

### <a name="scenarios"></a><span data-ttu-id="997a2-263">Scénáře</span><span class="sxs-lookup"><span data-stu-id="997a2-263">Scenarios</span></span>

- <span data-ttu-id="997a2-264">**Zakázat replikaci ve všech tabulkách**:</span><span class="sxs-lookup"><span data-stu-id="997a2-264">**Disable replication on all tables**:</span></span>

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  <span data-ttu-id="997a2-265">nebo</span><span class="sxs-lookup"><span data-stu-id="997a2-265">or</span></span>

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari username> --src-ambari-password=<source cluster Ambari password>

- <span data-ttu-id="997a2-266">**Zakázat replikaci na zadaných tabulek (tabulky1, tabulky2 a tabulka3)**:</span><span class="sxs-lookup"><span data-stu-id="997a2-266">**Disable replication on specified tables (table1, table2, and table3)**:</span></span>

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a><span data-ttu-id="997a2-267">Další kroky</span><span class="sxs-lookup"><span data-stu-id="997a2-267">Next steps</span></span>

<span data-ttu-id="997a2-268">V tomto kurzu jste se naučili jak replikace HBase tooconfigure přes dvě datových center.</span><span class="sxs-lookup"><span data-stu-id="997a2-268">In this tutorial, you learned how tooconfigure HBase replication across two datacenters.</span></span> <span data-ttu-id="997a2-269">toolearn Další informace o HDInsight a HBase, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="997a2-269">toolearn more about HDInsight and HBase, see:</span></span>

* <span data-ttu-id="997a2-270">[Začínáme s Apache HBase v HDInsight][hdinsight-hbase-get-started]</span><span class="sxs-lookup"><span data-stu-id="997a2-270">[Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started]</span></span>
* <span data-ttu-id="997a2-271">[Přehled HDInsight HBase][hdinsight-hbase-overview]</span><span class="sxs-lookup"><span data-stu-id="997a2-271">[HDInsight HBase overview][hdinsight-hbase-overview]</span></span>
* <span data-ttu-id="997a2-272">[Vytvořit clustery HBase v Azure Virtual Network][hdinsight-hbase-provision-vnet]</span><span class="sxs-lookup"><span data-stu-id="997a2-272">[Create HBase clusters in Azure Virtual Network][hdinsight-hbase-provision-vnet]</span></span>
* <span data-ttu-id="997a2-273">[Analýza v reálném čase sentimentu Twitter s HBase][hdinsight-hbase-twitter-sentiment]</span><span class="sxs-lookup"><span data-stu-id="997a2-273">[Analyze real-time Twitter sentiment with HBase][hdinsight-hbase-twitter-sentiment]</span></span>
* <span data-ttu-id="997a2-274">[Analýza dat snímače pomocí Storm a HBase v HDInsight (Hadoop)][hdinsight-sensor-data]</span><span class="sxs-lookup"><span data-stu-id="997a2-274">[Analyzing sensor data with Storm and HBase in HDInsight (Hadoop)][hdinsight-sensor-data]</span></span>

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
