---
title: dostupnost aaaHigh pro Hadoop - Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak clustery HDInsight zvyšování spolehlivosti a dostupnosti pomocí dalšího hlavního uzlu. Zjistěte, jak to ovlivní služby Hadoop, jako je například Ambari a Hive, a jak tooindividually připojit tooeach hlavního uzlu pomocí protokolu SSH."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: Blackmist
documentationcenter: 
tags: azure-portal
keywords: hadoop vysokou dostupnost
ms.assetid: 99c9f59c-cf6b-4529-99d1-bf060435e8d4
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 07/28/2017
ms.author: larryfr
ms.openlocfilehash: 9ff62afe6b63b241cb984225233157219f8d7411
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="availability-and-reliability-of-hadoop-clusters-in-hdinsight"></a><span data-ttu-id="4f3d1-105">Dostupnost a spolehlivost clusterů Hadoop ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="4f3d1-105">Availability and reliability of Hadoop clusters in HDInsight</span></span>

<span data-ttu-id="4f3d1-106">Clustery HDInsight poskytují dvou hlavních uzlech tooincrease hello dostupnost a spolehlivost Hadoop služeb a spuštěné úlohy.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-106">HDInsight clusters provide two head nodes tooincrease hello availability and reliability of Hadoop services and jobs running.</span></span>

<span data-ttu-id="4f3d1-107">Hadoop dosahuje vysoké dostupnosti a spolehlivosti replikace služeb a dat mezi několika uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-107">Hadoop achieves high availability and reliability by replicating services and data across multiple nodes in a cluster.</span></span> <span data-ttu-id="4f3d1-108">Ale standardní distribuce systému Hadoop obvykle mít pouze jeden hlavní uzel.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-108">However standard distributions of Hadoop typically have only a single head node.</span></span> <span data-ttu-id="4f3d1-109">Všechny výpadku hello jeden hlavní uzel může způsobit hello clusteru toostop práci.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-109">Any outage of hello single head node can cause hello cluster toostop working.</span></span> <span data-ttu-id="4f3d1-110">HDInsight poskytuje dostupnost a spolehlivost Hadoop tooimprove dva headnodes.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-110">HDInsight provides two headnodes tooimprove Hadoop's availability and reliability.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4f3d1-111">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="4f3d1-112">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="4f3d1-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="availability-and-reliability-of-nodes"></a><span data-ttu-id="4f3d1-113">Dostupnost a spolehlivost uzlů</span><span class="sxs-lookup"><span data-stu-id="4f3d1-113">Availability and reliability of nodes</span></span>

<span data-ttu-id="4f3d1-114">Uzly v clusteru HDInsight se implementují pomocí virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-114">Nodes in an HDInsight cluster are implemented using Azure Virtual Machines.</span></span> <span data-ttu-id="4f3d1-115">Hello následující části popisují typy jednotlivých uzlů hello používá s HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-115">hello following sections discuss hello individual node types used with HDInsight.</span></span> 

> [!NOTE]
> <span data-ttu-id="4f3d1-116">Ne všechny typy uzlů se používají pro typ clusteru.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-116">Not all node types are used for a cluster type.</span></span> <span data-ttu-id="4f3d1-117">Typ clusteru Hadoop například nemá žádné uzly Nimbus.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-117">For example, a Hadoop cluster type does not have any Nimbus nodes.</span></span> <span data-ttu-id="4f3d1-118">Další informace o uzlech používá typy clusterů HDInsight, najdete v oddílu typy clusteru hello hello [vytvořit systémem Linux Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-118">For more information on nodes used by HDInsight cluster types, see hello Cluster types section of hello [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) document.</span></span>

### <a name="head-nodes"></a><span data-ttu-id="4f3d1-119">hlavních uzlech</span><span class="sxs-lookup"><span data-stu-id="4f3d1-119">Head nodes</span></span>

<span data-ttu-id="4f3d1-120">tooensure vysokou dostupnost služby Hadoop, HDInsight nabízí dva uzly head.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-120">tooensure high availability of Hadoop services, HDInsight provides two head nodes.</span></span> <span data-ttu-id="4f3d1-121">Obě hlavních uzlech jsou aktivní a jsou spuštěné v rámci clusteru HDInsight hello současně.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-121">Both head nodes are active and running within hello HDInsight cluster simultaneously.</span></span> <span data-ttu-id="4f3d1-122">Některé služby, jako je například HDFS nebo YARN, jsou pouze aktivní jeden hlavního uzlu v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-122">Some services, such as HDFS or YARN, are only 'active' on one head node at any given time.</span></span> <span data-ttu-id="4f3d1-123">Dalšími službami, například HiveServer2 nebo Metaúložiště Hive jsou aktivní v obou uzlech head v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-123">Other services such as HiveServer2 or Hive MetaStore are active on both head nodes at hello same time.</span></span>

<span data-ttu-id="4f3d1-124">Uzly HEAD (a jiné uzly v HDInsight) mít číselnou hodnotu v rámci hello hostname hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-124">Head nodes (and other nodes in HDInsight) have a numeric value as part of hello hostname of hello node.</span></span> <span data-ttu-id="4f3d1-125">Například `hn0-CLUSTERNAME` nebo `hn4-CLUSTERNAME`.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-125">For example, `hn0-CLUSTERNAME` or `hn4-CLUSTERNAME`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4f3d1-126">Nepřidružujte hello číselná hodnota k tom, zda je uzel primární nebo sekundární.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-126">Do not associate hello numeric value with whether a node is primary or secondary.</span></span> <span data-ttu-id="4f3d1-127">Číselná hodnota Hello je pouze přítomen tooprovide jedinečný název pro každý uzel.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-127">hello numeric value is only present tooprovide a unique name for each node.</span></span>

### <a name="nimbus-nodes"></a><span data-ttu-id="4f3d1-128">Uzly Nimbus</span><span class="sxs-lookup"><span data-stu-id="4f3d1-128">Nimbus Nodes</span></span>

<span data-ttu-id="4f3d1-129">Jsou k dispozici u clusterů Storm uzly nimbus.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-129">Nimbus nodes are available with Storm clusters.</span></span> <span data-ttu-id="4f3d1-130">uzly Nimbus Hello zadejte toohello podobné funkce jako Hadoop JobTracker distribuci a monitorování zpracování napříč uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-130">hello Nimbus nodes provide similar functionality toohello Hadoop JobTracker by distributing and monitoring processing across worker nodes.</span></span> <span data-ttu-id="4f3d1-131">HDInsight nabízí dva uzly Nimbus pro clustery Storm</span><span class="sxs-lookup"><span data-stu-id="4f3d1-131">HDInsight provides two Nimbus nodes for Storm clusters</span></span>

### <a name="zookeeper-nodes"></a><span data-ttu-id="4f3d1-132">Uzly zookeeper</span><span class="sxs-lookup"><span data-stu-id="4f3d1-132">Zookeeper nodes</span></span>

<span data-ttu-id="4f3d1-133">[ZooKeeper](http://zookeeper.apache.org/) uzlech se používají pro vedoucí volba hlavní služeb v hlavních uzlech.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-133">[ZooKeeper](http://zookeeper.apache.org/) nodes are used for leader election of master services on head nodes.</span></span> <span data-ttu-id="4f3d1-134">Jsou také použít tooinsure, služby, datové (pracovník) uzly a bran vědět, které hlavního uzlu je aktivní na hlavní služby.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-134">They are also used tooinsure that services, data (worker) nodes, and gateways know which head node a master service is active on.</span></span> <span data-ttu-id="4f3d1-135">Ve výchozím nastavení HDInsight poskytuje tři uzly ZooKeeper.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-135">By default, HDInsight provides three ZooKeeper nodes.</span></span>

### <a name="worker-nodes"></a><span data-ttu-id="4f3d1-136">Pracovní uzly</span><span class="sxs-lookup"><span data-stu-id="4f3d1-136">Worker nodes</span></span>

<span data-ttu-id="4f3d1-137">Pracovní uzly analýzám hello skutečná data odeslaná toohello clusteru po úlohu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-137">Worker nodes perform hello actual data analysis when a job is submitted toohello cluster.</span></span> <span data-ttu-id="4f3d1-138">V případě selhání pracovního uzlu hello úkol, který byl výkon je odeslaná tooanother pracovního uzlu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-138">If a worker node fails, hello task that it was performing is submitted tooanother worker node.</span></span> <span data-ttu-id="4f3d1-139">Ve výchozím nastavení vytvoří HDInsight čtyři uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-139">By default, HDInsight creates four worker nodes.</span></span> <span data-ttu-id="4f3d1-140">Toto číslo toosuit můžete změnit vašim potřebám, během a po vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-140">You can change this number toosuit your needs both during and after cluster creation.</span></span>

### <a name="edge-node"></a><span data-ttu-id="4f3d1-141">Hraniční uzel</span><span class="sxs-lookup"><span data-stu-id="4f3d1-141">Edge node</span></span>

<span data-ttu-id="4f3d1-142">Hraniční uzel analýzu dat v rámci clusteru hello aktivně neúčastní.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-142">An edge node does not actively participate in data analysis within hello cluster.</span></span> <span data-ttu-id="4f3d1-143">Používá se vývojáři nebo datových vědců při práci s Hadoop.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-143">It is used by developers or data scientists when working with Hadoop.</span></span> <span data-ttu-id="4f3d1-144">Hello hraniční uzel život v hello stejné virtuální síti Azure jako hello jiné uzly v clusteru hello a přímý přístup k všechny ostatní uzly.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-144">hello edge node lives in hello same Azure Virtual Network as hello other nodes in hello cluster, and can directly access all other nodes.</span></span> <span data-ttu-id="4f3d1-145">Hello hraniční uzel lze použít bez nutnosti převádět prostředky z úlohy analýzy nebo důležité služby Hadoop.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-145">hello edge node can be used without taking resources away from critical Hadoop services or analysis jobs.</span></span>

<span data-ttu-id="4f3d1-146">V současné době R serverem v HDInsight je hello pouze clusteru typ, který poskytuje hraniční uzel ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-146">Currently, R Server on HDInsight is hello only cluster type that provides an edge node by default.</span></span> <span data-ttu-id="4f3d1-147">Pro R serverem v HDInsight, se používá hello hraniční uzel, na testovací R kód místně na uzlu hello před odesláním ji toohello clusteru pro distribuované zpracování.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-147">For R Server on HDInsight, hello edge node is used test R code locally on hello node before submitting it toohello cluster for distributed processing.</span></span>

<span data-ttu-id="4f3d1-148">Informace o používání hraniční uzel s typy clusteru než R Server najdete v tématu hello [používají uzly okraj v HDInsight](hdinsight-apps-use-edge-node.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-148">For information on using an edge node with cluster types other than R Server, see hello [Use edge nodes in HDInsight](hdinsight-apps-use-edge-node.md) document.</span></span>

## <a name="accessing-hello-nodes"></a><span data-ttu-id="4f3d1-149">Přístup k hello uzly</span><span class="sxs-lookup"><span data-stu-id="4f3d1-149">Accessing hello nodes</span></span>

<span data-ttu-id="4f3d1-150">Clusteru toohello přístupu přes hello Internetu je zajišťováno prostřednictvím veřejného brány.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-150">Access toohello cluster over hello internet is provided through a public gateway.</span></span> <span data-ttu-id="4f3d1-151">Přístup je omezená tooconnecting toohello hlavních uzlech a (pokud existuje) hello hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-151">Access is limited tooconnecting toohello head nodes and (if one exists) hello edge node.</span></span> <span data-ttu-id="4f3d1-152">Přístup k tooservices systémem hlavních uzlech hello není provedena tak, že více hlavních uzlech.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-152">Access tooservices running on hello head nodes is not effected by having multiple head nodes.</span></span> <span data-ttu-id="4f3d1-153">Hello veřejné brány trasy požadavky toohello hlavního uzlu, který hostuje hello požadované služby.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-153">hello public gateway routes requests toohello head node that hosts hello requested service.</span></span> <span data-ttu-id="4f3d1-154">Například pokud Ambari je aktuálně hostitelem hello sekundárního hlavního uzlu, brána hello směruje příchozí požadavky pro Ambari toothat uzel.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-154">For example, if Ambari is currently hosted on hello secondary head node, hello gateway routes incoming requests for Ambari toothat node.</span></span>

<span data-ttu-id="4f3d1-155">Přístup prostřednictvím brány veřejné hello je omezená tooport 443 (HTTPS), 22 a 23.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-155">Access over hello public gateway is limited tooport 443 (HTTPS), 22, and 23.</span></span>

* <span data-ttu-id="4f3d1-156">Port __443__ je použité tooaccess Ambari a další webového uživatelského rozhraní nebo rozhraní REST API hostované v uzlech head hello.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-156">Port __443__ is used tooaccess Ambari and other web UI or REST APIs hosted on hello head nodes.</span></span>

* <span data-ttu-id="4f3d1-157">Port __22__ je použité tooaccess hello primární hlavní uzel nebo okraj uzlu pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-157">Port __22__ is used tooaccess hello primary head node or edge node with SSH.</span></span>

* <span data-ttu-id="4f3d1-158">Port __23__ je použité tooaccess hello sekundárního hlavního uzlu pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-158">Port __23__ is used tooaccess hello secondary head node with SSH.</span></span> <span data-ttu-id="4f3d1-159">Například `ssh username@mycluster-ssh.azurehdinsight.net` připojí toohello primární hlavního uzlu clusteru hello s názvem **mycluster**.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-159">For example, `ssh username@mycluster-ssh.azurehdinsight.net` connects toohello primary head node of hello cluster named **mycluster**.</span></span>

<span data-ttu-id="4f3d1-160">Další informace o používání SSH najdete v tématu hello [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-160">For more information on using SSH, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

### <a name="internal-fully-qualified-domain-names-fqdn"></a><span data-ttu-id="4f3d1-161">Interní plně kvalifikované názvy domény (FQDN)</span><span class="sxs-lookup"><span data-stu-id="4f3d1-161">Internal fully qualified domain names (FQDN)</span></span>

<span data-ttu-id="4f3d1-162">Uzly v clusteru služby HDInsight mít interní IP adresu a plně kvalifikovaný název domény, který lze přistupovat pouze z clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-162">Nodes in an HDInsight cluster have an internal IP address and FQDN that can only be accessed from hello cluster.</span></span> <span data-ttu-id="4f3d1-163">Pokud přístup ke službám v clusteru hello pomocí hello interní plně kvalifikovaný název domény nebo IP adresa, měli byste použít toouse Ambari tooverify hello IP adresu nebo plně kvalifikovaný název domény při přístupu k hello služby.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-163">When accessing services on hello cluster using hello internal FQDN or IP address, you should use Ambari tooverify hello IP or FQDN toouse when accessing hello service.</span></span>

<span data-ttu-id="4f3d1-164">Například hello Oozie služby lze spustit jen u jednoho hlavního uzlu a pomocí hello `oozie` příkaz z relace SSH vyžaduje službu toohello hello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-164">For example, hello Oozie service can only run on one head node, and using hello `oozie` command from an SSH session requires hello URL toohello service.</span></span> <span data-ttu-id="4f3d1-165">Tato adresa URL může být načítat z Ambari pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4f3d1-165">This URL can be retrieved from Ambari by using hello following command:</span></span>

    curl -u admin:PASSWORD "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url

<span data-ttu-id="4f3d1-166">Tento příkaz vrátí hodnotu podobné toohello následující příkaz, který obsahuje hello interní adresa URL toouse s hello `oozie` příkaz:</span><span class="sxs-lookup"><span data-stu-id="4f3d1-166">This command returns a value similar toohello following command, which contains hello internal URL toouse with hello `oozie` command:</span></span>

    "oozie.base.url": "http://hn0-CLUSTERNAME-randomcharacters.cx.internal.cloudapp.net:11000/oozie"

<span data-ttu-id="4f3d1-167">Další informace o práci s hello Ambari REST API najdete v tématu [monitorování a správě HDInsight pomocí Ambari REST API hello](hdinsight-hadoop-manage-ambari-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="4f3d1-167">For more information on working with hello Ambari REST API, see [Monitor and Manage HDInsight using hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).</span></span>

### <a name="accessing-other-node-types"></a><span data-ttu-id="4f3d1-168">Přístup k jiné typy uzlů</span><span class="sxs-lookup"><span data-stu-id="4f3d1-168">Accessing other node types</span></span>

<span data-ttu-id="4f3d1-169">Můžete připojit toonodes, které nejsou přímo přístupné přes internet hello pomocí hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="4f3d1-169">You can connect toonodes that are not directly accessible over hello internet by using hello following methods:</span></span>

* <span data-ttu-id="4f3d1-170">**SSH**: Po připojení tooa hlavního uzlu pomocí protokolu SSH, potom můžete pomocí SSH z hello hlavního uzlu tooconnect tooother uzlů v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-170">**SSH**: Once connected tooa head node using SSH, you can then use SSH from hello head node tooconnect tooother nodes in hello cluster.</span></span> <span data-ttu-id="4f3d1-171">Další informace najdete v tématu hello [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-171">For more information, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

* <span data-ttu-id="4f3d1-172">**Tunelové propojení SSH**: Pokud potřebujete tooaccess webovou službu hostovanou na jednom z uzlů hello, které není vystavený toohello internet, je nutné použít tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-172">**SSH Tunnel**: If you need tooaccess a web service hosted on one of hello nodes that is not exposed toohello internet, you must use an SSH tunnel.</span></span> <span data-ttu-id="4f3d1-173">Další informace najdete v tématu hello [použijte tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-173">For more information, see hello [Use an SSH tunnel with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

* <span data-ttu-id="4f3d1-174">**Virtuální síť Azure**: Pokud váš HDInsight clusteru je součástí virtuální síť Azure, jakýkoli prostředek na hello stejné virtuální sítě můžete přímý přístup k všechny uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-174">**Azure Virtual Network**: If your HDInsight cluster is part of an Azure Virtual Network, any resource on hello same Virtual Network can directly access all nodes in hello cluster.</span></span> <span data-ttu-id="4f3d1-175">Další informace najdete v tématu hello [rozšířit HDInsight pomocí Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-175">For more information, see hello [Extend HDInsight using Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.</span></span>

## <a name="how-toocheck-on-a-service-status"></a><span data-ttu-id="4f3d1-176">Jak toocheck na stav služby</span><span class="sxs-lookup"><span data-stu-id="4f3d1-176">How toocheck on a service status</span></span>

<span data-ttu-id="4f3d1-177">toocheck hello stav služby, které běží na hello hlavních uzlů se použít hello webové uživatelské rozhraní Ambari nebo hello Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-177">toocheck hello status of services that run on hello head nodes, use hello Ambari Web UI or hello Ambari REST API.</span></span>

### <a name="ambari-web-ui"></a><span data-ttu-id="4f3d1-178">Webovému uživatelskému rozhraní Ambari</span><span class="sxs-lookup"><span data-stu-id="4f3d1-178">Ambari Web UI</span></span>

<span data-ttu-id="4f3d1-179">Hello webové uživatelské rozhraní Ambari jsou viditelná na https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-179">hello Ambari Web UI is viewable at https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="4f3d1-180">Nahraďte **CLUSTERNAME** s hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-180">Replace **CLUSTERNAME** with hello name of your cluster.</span></span> <span data-ttu-id="4f3d1-181">Pokud se zobrazí výzva, zadejte přihlašovací údaje uživatele hello HTTP pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-181">If prompted, enter hello HTTP user credentials for your cluster.</span></span> <span data-ttu-id="4f3d1-182">Hello výchozí HTTP uživatelské jméno je **správce** a hello heslo je hello heslo, které jste zadali při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-182">hello default HTTP user name is **admin** and hello password is hello password you entered when creating hello cluster.</span></span>

<span data-ttu-id="4f3d1-183">Až přijedete na stránce Ambari hello, jsou uvedené služby hello nainstalovaná na hello nalevo od stránku hello.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-183">When you arrive on hello Ambari page, hello installed services are listed on hello left of hello page.</span></span>

![Nainstalovaná services](./media/hdinsight-high-availability-linux/services.png)

<span data-ttu-id="4f3d1-185">Existuje řada ikony, které se mohou zobrazit další stav tooindicate tooa služby.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-185">There are a series of icons that may appear next tooa service tooindicate status.</span></span> <span data-ttu-id="4f3d1-186">Žádné výstrahy týkající se služby tooa lze zobrazit pomocí hello **výstrahy** odkaz hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-186">Any alerts related tooa service can be viewed using hello **Alerts** link at hello top of hello page.</span></span> <span data-ttu-id="4f3d1-187">Další informace o jeho můžete vybrat každou tooview služby.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-187">You can select each service tooview more information on it.</span></span>

<span data-ttu-id="4f3d1-188">Když stránku hello služby obsahuje informace o stavu hello a konfigurace jednotlivých služeb, neposkytuje informace, na kterém je spuštěna služba hello hlavního uzlu na.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-188">While hello service page provides information on hello status and configuration of each service, it does not provide information on which head node hello service is running on.</span></span> <span data-ttu-id="4f3d1-189">tooview tento informace, použijte hello **hostitele** odkaz hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-189">tooview this information, use hello **Hosts** link at hello top of hello page.</span></span> <span data-ttu-id="4f3d1-190">Tato stránka zobrazuje hostitelů v clusteru hello, včetně hlavních uzlech hello.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-190">This page displays hosts within hello cluster, including hello head nodes.</span></span>

![seznam hostitelů](./media/hdinsight-high-availability-linux/hosts.png)

<span data-ttu-id="4f3d1-192">Výběr hello odkaz pro jeden z hlavních uzlech hello zobrazí hello služeb a komponent spuštěných na tomto uzlu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-192">Selecting hello link for one of hello head nodes displays hello services and components running on that node.</span></span>

![Stav součásti](./media/hdinsight-high-availability-linux/nodeservices.png)

<span data-ttu-id="4f3d1-194">Další informace o používání Ambari najdete v tématu [monitorování a správě HDInsight pomocí hello webové uživatelské rozhraní Ambari](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="4f3d1-194">For more information on using Ambari, see [Monitor and manage HDInsight using hello Ambari Web UI](hdinsight-hadoop-manage-ambari.md).</span></span>

### <a name="ambari-rest-api"></a><span data-ttu-id="4f3d1-195">Ambari REST API</span><span class="sxs-lookup"><span data-stu-id="4f3d1-195">Ambari REST API</span></span>

<span data-ttu-id="4f3d1-196">Hello Ambari REST API je k dispozici prostřednictvím hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-196">hello Ambari REST API is available over hello internet.</span></span> <span data-ttu-id="4f3d1-197">veřejné brány HDInsight Hello zpracovává směrování požadavků uzlu head toohello, která je aktuálně hostitelem hello REST API.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-197">hello HDInsight public gateway handles routing requests toohello head node that is currently hosting hello REST API.</span></span>

<span data-ttu-id="4f3d1-198">Můžete použít následující příkaz toocheck hello stavu služby prostřednictvím hello Ambari REST API hello:</span><span class="sxs-lookup"><span data-stu-id="4f3d1-198">You can use hello following command toocheck hello state of a service through hello Ambari REST API:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state

* <span data-ttu-id="4f3d1-199">Nahraďte **heslo** s hello HTTP heslo účtu uživatele (správce).</span><span class="sxs-lookup"><span data-stu-id="4f3d1-199">Replace **PASSWORD** with hello HTTP user (admin) account password.</span></span>
* <span data-ttu-id="4f3d1-200">Nahraďte **CLUSTERNAME** s názvem hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-200">Replace **CLUSTERNAME** with hello name of hello cluster.</span></span>
* <span data-ttu-id="4f3d1-201">Nahraďte **SERVICENAME** s názvem hello služby hello chcete toocheck hello stav.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-201">Replace **SERVICENAME** with hello name of hello service you want toocheck hello status of.</span></span>

<span data-ttu-id="4f3d1-202">Například stav hello toocheck hello **HDFS** služby v clusteru s názvem **mycluster**, s použitím hesla **heslo**, využije hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4f3d1-202">For example, toocheck hello status of hello **HDFS** service on a cluster named **mycluster**, with a password of **password**, you would use hello following command:</span></span>

    curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state

<span data-ttu-id="4f3d1-203">odpověď Hello je podobné toohello následující JSON:</span><span class="sxs-lookup"><span data-stu-id="4f3d1-203">hello response is similar toohello following JSON:</span></span>

    {
      "href" : "http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
      "ServiceInfo" : {
        "cluster_name" : "mycluster",
        "service_name" : "HDFS",
        "state" : "STARTED"
      }
    }

<span data-ttu-id="4f3d1-204">Hello URL víme, že je aktuálně spuštěna služba hello hlavního uzlu s názvem **hn0 CLUSTERNAME**.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-204">hello URL tells us that hello service is currently running on a head node named **hn0-CLUSTERNAME**.</span></span>

<span data-ttu-id="4f3d1-205">Hello stavu víme, že je služba hello aktuálně spuštěna, nebo **ZAČÍNÁME**.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-205">hello state tells us that hello service is currently running, or **STARTED**.</span></span>

<span data-ttu-id="4f3d1-206">Pokud si nejste jisti, jaké služby jsou nainstalovány na hello clusteru, můžete použít následující příkaz tooretrieve seznam hello:</span><span class="sxs-lookup"><span data-stu-id="4f3d1-206">If you do not know what services are installed on hello cluster, you can use hello following command tooretrieve a list:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services

<span data-ttu-id="4f3d1-207">Další informace o práci s hello Ambari REST API najdete v tématu [monitorování a správě HDInsight pomocí Ambari REST API hello](hdinsight-hadoop-manage-ambari-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="4f3d1-207">For more information on working with hello Ambari REST API, see [Monitor and Manage HDInsight using hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).</span></span>

#### <a name="service-components"></a><span data-ttu-id="4f3d1-208">Součásti služby</span><span class="sxs-lookup"><span data-stu-id="4f3d1-208">Service components</span></span>

<span data-ttu-id="4f3d1-209">Služby mohou obsahovat součásti, které chcete stav hello toocheck jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-209">Services may contain components that you wish toocheck hello status of individually.</span></span> <span data-ttu-id="4f3d1-210">Například HDFS obsahuje součást NameNode hello.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-210">For example, HDFS contains hello NameNode component.</span></span> <span data-ttu-id="4f3d1-211">tooview informací na komponentu, bude příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="4f3d1-211">tooview information on a component, hello command would be:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

<span data-ttu-id="4f3d1-212">Pokud si nejste jisti, jaké součásti jsou poskytovány služby, můžete použít následující příkaz tooretrieve seznam hello:</span><span class="sxs-lookup"><span data-stu-id="4f3d1-212">If you do not know what components are provided by a service, you can use hello following command tooretrieve a list:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

## <a name="how-tooaccess-log-files-on-hello-head-nodes"></a><span data-ttu-id="4f3d1-213">Jak tooaccess soubory protokolu na hlavních uzlech hello</span><span class="sxs-lookup"><span data-stu-id="4f3d1-213">How tooaccess log files on hello head nodes</span></span>

### <a name="ssh"></a><span data-ttu-id="4f3d1-214">SSH</span><span class="sxs-lookup"><span data-stu-id="4f3d1-214">SSH</span></span>

<span data-ttu-id="4f3d1-215">Při připojené tooa hlavního uzlu prostřednictvím SSH, souborů protokolu najdete v části **/var/log**.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-215">While connected tooa head node through SSH, log files can be found under **/var/log**.</span></span> <span data-ttu-id="4f3d1-216">Například **/var/log/hadoop-yarn/yarn** neobsahuje protokoly YARN.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-216">For example, **/var/log/hadoop-yarn/yarn** contain logs for YARN.</span></span>

<span data-ttu-id="4f3d1-217">Každý hlavního uzlu mohou obsahovat položky jedinečný protokolu, takže byste měli zkontrolovat, že hello protokolů v obou.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-217">Each head node can have unique log entries, so you should check hello logs on both.</span></span>

### <a name="sftp"></a><span data-ttu-id="4f3d1-218">SFTP</span><span class="sxs-lookup"><span data-stu-id="4f3d1-218">SFTP</span></span>

<span data-ttu-id="4f3d1-219">Můžete také připojit toohello hlavního uzlu pomocí hello SSH protokol FTP nebo rozhraní zabezpečení souboru Transfer Protocol (SFTP) a stahování souborů protokolu hello přímo.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-219">You can also connect toohello head node using hello SSH File Transfer Protocol or Secure File Transfer Protocol (SFTP), and download hello log files directly.</span></span>

<span data-ttu-id="4f3d1-220">Podobně jako toousing klient SSH, pokud se připojujete toohello clusteru, je třeba zadat hello uživatele účtu název a hello SSH adresa SSH hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-220">Similar toousing an SSH client, when connecting toohello cluster you must provide hello SSH user account name and hello SSH address of hello cluster.</span></span> <span data-ttu-id="4f3d1-221">Například, `sftp username@mycluster-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-221">For example, `sftp username@mycluster-ssh.azurehdinsight.net`.</span></span> <span data-ttu-id="4f3d1-222">Zadejte hello heslo pro účet hello po zobrazení výzvy, nebo zadejte veřejný klíč pomocí hello `-i` parametr.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-222">Provide hello password for hello account when prompted, or provide a public key using hello `-i` parameter.</span></span>

<span data-ttu-id="4f3d1-223">Po připojení se zobrazí `sftp>` řádku.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-223">Once connected, you are presented with a `sftp>` prompt.</span></span> <span data-ttu-id="4f3d1-224">Z příkazového řádku, můžete změnit adresáře, nahrávání a stahování souborů.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-224">From this prompt, you can change directories, upload, and download files.</span></span> <span data-ttu-id="4f3d1-225">Můžete například změnit hello následující příkazy adresáře toohello **/var/log/hadoop/hdfs** adresář a pak stáhnout všechny soubory v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-225">For example, hello following commands change directories toohello **/var/log/hadoop/hdfs** directory and then download all files in hello directory.</span></span>

    cd /var/log/hadoop/hdfs
    get *

<span data-ttu-id="4f3d1-226">Seznam dostupné příkazy, zadejte `help` v hello `sftp>` řádku.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-226">For a list of available commands, enter `help` at hello `sftp>` prompt.</span></span>

> [!NOTE]
> <span data-ttu-id="4f3d1-227">Existují také grafické rozhraní, které vám umožňují toovisualize hello systému souborů při připojení pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-227">There are also graphical interfaces that allow you toovisualize hello file system when connected using SFTP.</span></span> <span data-ttu-id="4f3d1-228">Například [MobaXTerm](http://mobaxterm.mobatek.net/) vám umožní systému souborů hello toobrowse pomocí podobné tooWindows rozhraní Průzkumníka.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-228">For example, [MobaXTerm](http://mobaxterm.mobatek.net/) allows you toobrowse hello file system using an interface similar tooWindows Explorer.</span></span>

### <a name="ambari"></a><span data-ttu-id="4f3d1-229">Ambari</span><span class="sxs-lookup"><span data-stu-id="4f3d1-229">Ambari</span></span>

> [!NOTE]
> <span data-ttu-id="4f3d1-230">soubory pomocí nástroje Ambari protokolu tooaccess, je nutné použít tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-230">tooaccess log files using Ambari, you must use an SSH tunnel.</span></span> <span data-ttu-id="4f3d1-231">Hello webové rozhraní pro jednotlivé služby hello nejsou veřejně přístupné na hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-231">hello web interfaces for hello individual services are not exposed publicly on hello Internet.</span></span> <span data-ttu-id="4f3d1-232">Informace o používání tunelového propojení SSH naleznete v tématu hello [používání tunelového propojení SSH](hdinsight-linux-ambari-ssh-tunnel.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-232">For information on using an SSH tunnel, see hello [Use SSH Tunneling](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

<span data-ttu-id="4f3d1-233">Hello webové uživatelské rozhraní Ambari vyberte službu hello chcete tooview protokoly pro (například YARN).</span><span class="sxs-lookup"><span data-stu-id="4f3d1-233">From hello Ambari Web UI, select hello service you wish tooview logs for (for example, YARN).</span></span> <span data-ttu-id="4f3d1-234">Potom pomocí **rychlé odkazy** tooselect které hello tooview hlavního uzlu v protokolech.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-234">Then use **Quick Links** tooselect which head node tooview hello logs for.</span></span>

![Pomocí rychlé odkazy tooview protokoly](./media/hdinsight-high-availability-linux/viewlogs.png)

## <a name="how-tooconfigure-hello-node-size"></a><span data-ttu-id="4f3d1-236">Jak tooconfigure hello velikost uzlu</span><span class="sxs-lookup"><span data-stu-id="4f3d1-236">How tooconfigure hello node size</span></span>

<span data-ttu-id="4f3d1-237">Hello velikost uzlu lze vybrat pouze při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-237">hello size of a node can only be selected during cluster creation.</span></span> <span data-ttu-id="4f3d1-238">Můžete najít seznam hello různé velikosti virtuálních počítačů k dispozici pro HDInsight na hello [HDInsight stránce s cenami](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="4f3d1-238">You can find a list of hello different VM sizes available for HDInsight on hello [HDInsight pricing page](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

<span data-ttu-id="4f3d1-239">Při vytváření clusteru, můžete zadat velikost hello hello uzlů.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-239">When creating a cluster, you can specify hello size of hello nodes.</span></span> <span data-ttu-id="4f3d1-240">Hello následující informace poskytují pokyny o tom, jak pomocí velikost hello toospecify hello [portál Azure][preview-portal], [prostředí Azure PowerShell][azure-powershell], a hello [rozhraní příkazového řádku Azure][azure-cli]:</span><span class="sxs-lookup"><span data-stu-id="4f3d1-240">hello following information provides guidance on how toospecify hello size using hello [Azure portal][preview-portal], [Azure PowerShell][azure-powershell], and hello [Azure CLI][azure-cli]:</span></span>

* <span data-ttu-id="4f3d1-241">**Portál Azure**: při vytváření clusteru, můžete nastavit hello velikost uzlů hello používá hello cluster:</span><span class="sxs-lookup"><span data-stu-id="4f3d1-241">**Azure portal**: When creating a cluster, you can set hello size of hello nodes used by hello cluster:</span></span>

    ![Obrázek průvodce vytvořením clusteru s výběrem velikost uzlu](./media/hdinsight-high-availability-linux/headnodesize.png)

* <span data-ttu-id="4f3d1-243">**Rozhraní příkazového řádku Azure**: při použití hello `azure hdinsight cluster create` příkaz hello velikost hello head, pracovního procesu a uzly ZooKeeper lze nastavit pomocí hello `--headNodeSize`, `--workerNodeSize`, a `--zookeeperNodeSize` parametry.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-243">**Azure CLI**: When using hello `azure hdinsight cluster create` command, you can set hello size of hello head, worker, and ZooKeeper nodes by using hello `--headNodeSize`, `--workerNodeSize`, and `--zookeeperNodeSize` parameters.</span></span>

* <span data-ttu-id="4f3d1-244">**Prostředí Azure PowerShell**: při použití hello `New-AzureRmHDInsightCluster` rutiny, můžete nastavit velikost hello hello head, pracovního procesu a uzly ZooKeeper pomocí hello `-HeadNodeVMSize`, `-WorkerNodeSize`, a `-ZookeeperNodeSize` parametry.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-244">**Azure PowerShell**: When using hello `New-AzureRmHDInsightCluster` cmdlet, you can set hello size of hello head, worker, and ZooKeeper nodes by using hello `-HeadNodeVMSize`, `-WorkerNodeSize`, and `-ZookeeperNodeSize` parameters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4f3d1-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4f3d1-245">Next steps</span></span>

<span data-ttu-id="4f3d1-246">Použijte následující odkazy toolearn více informací o věcí uvedených v tomto dokumentu hello.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-246">Use hello following links toolearn more about things mentioned in this document.</span></span>

* [<span data-ttu-id="4f3d1-247">Ambari REST odkaz</span><span class="sxs-lookup"><span data-stu-id="4f3d1-247">Ambari REST Reference</span></span>](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [<span data-ttu-id="4f3d1-248">Instalace a konfigurace hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="4f3d1-248">Install and configure hello Azure CLI</span></span>](../cli-install-nodejs.md)
* [<span data-ttu-id="4f3d1-249">Nainstalujte a nakonfigurujte Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4f3d1-249">Install and configure Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="4f3d1-250">Spravovat HDInsight pomocí Ambari</span><span class="sxs-lookup"><span data-stu-id="4f3d1-250">Manage HDInsight using Ambari</span></span>](hdinsight-hadoop-manage-ambari.md)
* [<span data-ttu-id="4f3d1-251">Zřizování clusterů HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="4f3d1-251">Provision Linux-based HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)

[preview-portal]: https://portal.azure.com/
[azure-powershell]: /powershell/azureps-cmdlets-docs
[azure-cli]: ../cli-install-nodejs.md
