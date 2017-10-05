---
title: "Vysoká dostupnost pro Hadoop - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak clustery HDInsight zvyšování spolehlivosti a dostupnosti pomocí dalšího hlavního uzlu. Zjistěte, jak to ovlivní služby Hadoop například Ambari a Hive, jak dobře jednotlivě připojit ke každému hlavního uzlu pomocí protokolu SSH."
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
ms.openlocfilehash: e66ba67a36fc48d1762ba302d708e060489fdc71
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="availability-and-reliability-of-hadoop-clusters-in-hdinsight"></a><span data-ttu-id="40531-105">Dostupnost a spolehlivost clusterů Hadoop ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="40531-105">Availability and reliability of Hadoop clusters in HDInsight</span></span>

<span data-ttu-id="40531-106">Clustery HDInsight poskytují dva uzly head zvýšit dostupnost a spolehlivost Hadoop služeb a spuštěné úlohy.</span><span class="sxs-lookup"><span data-stu-id="40531-106">HDInsight clusters provide two head nodes to increase the availability and reliability of Hadoop services and jobs running.</span></span>

<span data-ttu-id="40531-107">Hadoop dosahuje vysoké dostupnosti a spolehlivosti replikace služeb a dat mezi několika uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="40531-107">Hadoop achieves high availability and reliability by replicating services and data across multiple nodes in a cluster.</span></span> <span data-ttu-id="40531-108">Ale standardní distribuce systému Hadoop obvykle mít pouze jeden hlavní uzel.</span><span class="sxs-lookup"><span data-stu-id="40531-108">However standard distributions of Hadoop typically have only a single head node.</span></span> <span data-ttu-id="40531-109">Všechny výpadku jeden hlavní uzel může způsobit clusteru přestane fungovat.</span><span class="sxs-lookup"><span data-stu-id="40531-109">Any outage of the single head node can cause the cluster to stop working.</span></span> <span data-ttu-id="40531-110">HDInsight nabízí dva headnodes zvýšit dostupnost a spolehlivost na Hadoop.</span><span class="sxs-lookup"><span data-stu-id="40531-110">HDInsight provides two headnodes to improve Hadoop's availability and reliability.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="40531-111">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="40531-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="40531-112">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="40531-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="availability-and-reliability-of-nodes"></a><span data-ttu-id="40531-113">Dostupnost a spolehlivost uzlů</span><span class="sxs-lookup"><span data-stu-id="40531-113">Availability and reliability of nodes</span></span>

<span data-ttu-id="40531-114">Uzly v clusteru HDInsight se implementují pomocí virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="40531-114">Nodes in an HDInsight cluster are implemented using Azure Virtual Machines.</span></span> <span data-ttu-id="40531-115">Následující části popisují typy jednotlivých uzlů použít s HDInsight.</span><span class="sxs-lookup"><span data-stu-id="40531-115">The following sections discuss the individual node types used with HDInsight.</span></span> 

> [!NOTE]
> <span data-ttu-id="40531-116">Ne všechny typy uzlů se používají pro typ clusteru.</span><span class="sxs-lookup"><span data-stu-id="40531-116">Not all node types are used for a cluster type.</span></span> <span data-ttu-id="40531-117">Typ clusteru Hadoop například nemá žádné uzly Nimbus.</span><span class="sxs-lookup"><span data-stu-id="40531-117">For example, a Hadoop cluster type does not have any Nimbus nodes.</span></span> <span data-ttu-id="40531-118">Další informace o uzlech používá typy clusterů HDInsight, najdete v části typy clusteru [vytvořit systémem Linux Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="40531-118">For more information on nodes used by HDInsight cluster types, see the Cluster types section of the [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) document.</span></span>

### <a name="head-nodes"></a><span data-ttu-id="40531-119">hlavních uzlech</span><span class="sxs-lookup"><span data-stu-id="40531-119">Head nodes</span></span>

<span data-ttu-id="40531-120">Zajistit vysokou dostupnost služeb Hadoop HDInsight nabízí dva uzly head.</span><span class="sxs-lookup"><span data-stu-id="40531-120">To ensure high availability of Hadoop services, HDInsight provides two head nodes.</span></span> <span data-ttu-id="40531-121">Obě hlavních uzlech současně jsou aktivní a spuštěné v rámci clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="40531-121">Both head nodes are active and running within the HDInsight cluster simultaneously.</span></span> <span data-ttu-id="40531-122">Některé služby, jako je například HDFS nebo YARN, jsou pouze aktivní jeden hlavního uzlu v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="40531-122">Some services, such as HDFS or YARN, are only 'active' on one head node at any given time.</span></span> <span data-ttu-id="40531-123">Dalšími službami, například HiveServer2 nebo Metaúložiště Hive jsou aktivní v obou uzlech head ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="40531-123">Other services such as HiveServer2 or Hive MetaStore are active on both head nodes at the same time.</span></span>

<span data-ttu-id="40531-124">Uzly HEAD (a jiné uzly v HDInsight) mít číselnou hodnotu v rámci uzlu název hostitele.</span><span class="sxs-lookup"><span data-stu-id="40531-124">Head nodes (and other nodes in HDInsight) have a numeric value as part of the hostname of the node.</span></span> <span data-ttu-id="40531-125">Například `hn0-CLUSTERNAME` nebo `hn4-CLUSTERNAME`.</span><span class="sxs-lookup"><span data-stu-id="40531-125">For example, `hn0-CLUSTERNAME` or `hn4-CLUSTERNAME`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="40531-126">Nepřidružujte číselná hodnota k tom, zda je uzel primární nebo sekundární.</span><span class="sxs-lookup"><span data-stu-id="40531-126">Do not associate the numeric value with whether a node is primary or secondary.</span></span> <span data-ttu-id="40531-127">Číselná hodnota nachází pouze zadejte jedinečný název pro každý uzel.</span><span class="sxs-lookup"><span data-stu-id="40531-127">The numeric value is only present to provide a unique name for each node.</span></span>

### <a name="nimbus-nodes"></a><span data-ttu-id="40531-128">Uzly Nimbus</span><span class="sxs-lookup"><span data-stu-id="40531-128">Nimbus Nodes</span></span>

<span data-ttu-id="40531-129">Jsou k dispozici u clusterů Storm uzly nimbus.</span><span class="sxs-lookup"><span data-stu-id="40531-129">Nimbus nodes are available with Storm clusters.</span></span> <span data-ttu-id="40531-130">Uzly Nimbus poskytují podobné funkce jako Hadoop jobtracker distribuci a monitorování zpracování napříč uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="40531-130">The Nimbus nodes provide similar functionality to the Hadoop JobTracker by distributing and monitoring processing across worker nodes.</span></span> <span data-ttu-id="40531-131">HDInsight nabízí dva uzly Nimbus pro clustery Storm</span><span class="sxs-lookup"><span data-stu-id="40531-131">HDInsight provides two Nimbus nodes for Storm clusters</span></span>

### <a name="zookeeper-nodes"></a><span data-ttu-id="40531-132">Uzly zookeeper</span><span class="sxs-lookup"><span data-stu-id="40531-132">Zookeeper nodes</span></span>

<span data-ttu-id="40531-133">[ZooKeeper](http://zookeeper.apache.org/) uzlech se používají pro vedoucí volba hlavní služeb v hlavních uzlech.</span><span class="sxs-lookup"><span data-stu-id="40531-133">[ZooKeeper](http://zookeeper.apache.org/) nodes are used for leader election of master services on head nodes.</span></span> <span data-ttu-id="40531-134">Používají se také zajistit, aby služby, datové (pracovník) uzly a brány věděli, jaké hlavního uzlu hlavní služby na aktivní.</span><span class="sxs-lookup"><span data-stu-id="40531-134">They are also used to insure that services, data (worker) nodes, and gateways know which head node a master service is active on.</span></span> <span data-ttu-id="40531-135">Ve výchozím nastavení HDInsight poskytuje tři uzly ZooKeeper.</span><span class="sxs-lookup"><span data-stu-id="40531-135">By default, HDInsight provides three ZooKeeper nodes.</span></span>

### <a name="worker-nodes"></a><span data-ttu-id="40531-136">Pracovní uzly</span><span class="sxs-lookup"><span data-stu-id="40531-136">Worker nodes</span></span>

<span data-ttu-id="40531-137">Pracovní uzly provádět analýzy skutečná data, když je úloha odeslána do clusteru.</span><span class="sxs-lookup"><span data-stu-id="40531-137">Worker nodes perform the actual data analysis when a job is submitted to the cluster.</span></span> <span data-ttu-id="40531-138">V případě selhání pracovního uzlu je úloha, která byla provádění odeslán do jiného uzlu pracovníka.</span><span class="sxs-lookup"><span data-stu-id="40531-138">If a worker node fails, the task that it was performing is submitted to another worker node.</span></span> <span data-ttu-id="40531-139">Ve výchozím nastavení vytvoří HDInsight čtyři uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="40531-139">By default, HDInsight creates four worker nodes.</span></span> <span data-ttu-id="40531-140">Toto číslo během a po vytvoření clusteru podle potřeby můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="40531-140">You can change this number to suit your needs both during and after cluster creation.</span></span>

### <a name="edge-node"></a><span data-ttu-id="40531-141">Hraniční uzel</span><span class="sxs-lookup"><span data-stu-id="40531-141">Edge node</span></span>

<span data-ttu-id="40531-142">Hraniční uzel není součástí aktivně analýzu dat v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="40531-142">An edge node does not actively participate in data analysis within the cluster.</span></span> <span data-ttu-id="40531-143">Používá se vývojáři nebo datových vědců při práci s Hadoop.</span><span class="sxs-lookup"><span data-stu-id="40531-143">It is used by developers or data scientists when working with Hadoop.</span></span> <span data-ttu-id="40531-144">Hraničního uzlu je umístěn ve stejné virtuální síti Azure jako ostatní uzly v clusteru a přímý přístup k jiné uzly.</span><span class="sxs-lookup"><span data-stu-id="40531-144">The edge node lives in the same Azure Virtual Network as the other nodes in the cluster, and can directly access all other nodes.</span></span> <span data-ttu-id="40531-145">Hraničního uzlu lze použít bez nutnosti převádět prostředky z úlohy analýzy nebo důležité služby Hadoop.</span><span class="sxs-lookup"><span data-stu-id="40531-145">The edge node can be used without taking resources away from critical Hadoop services or analysis jobs.</span></span>

<span data-ttu-id="40531-146">V současné době R serverem v HDInsight je pouze typ clusteru, který poskytuje hraniční uzel ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="40531-146">Currently, R Server on HDInsight is the only cluster type that provides an edge node by default.</span></span> <span data-ttu-id="40531-147">Pro R serverem v HDInsight, se používá hraničního uzlu testovací R kód místně na uzlu před odesláním do clusteru pro distribuované zpracování.</span><span class="sxs-lookup"><span data-stu-id="40531-147">For R Server on HDInsight, the edge node is used test R code locally on the node before submitting it to the cluster for distributed processing.</span></span>

<span data-ttu-id="40531-148">Informace o používání hraniční uzel s typy clusteru než R Server najdete v tématu [používají uzly okraj v HDInsight](hdinsight-apps-use-edge-node.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="40531-148">For information on using an edge node with cluster types other than R Server, see the [Use edge nodes in HDInsight](hdinsight-apps-use-edge-node.md) document.</span></span>

## <a name="accessing-the-nodes"></a><span data-ttu-id="40531-149">Přístup k uzlu</span><span class="sxs-lookup"><span data-stu-id="40531-149">Accessing the nodes</span></span>

<span data-ttu-id="40531-150">Přístup ke clusteru přes internet je zajišťováno prostřednictvím veřejného brány.</span><span class="sxs-lookup"><span data-stu-id="40531-150">Access to the cluster over the internet is provided through a public gateway.</span></span> <span data-ttu-id="40531-151">Přístup je omezen na připojení k hlavnímu uzlu a (pokud existuje) hraničního uzlu.</span><span class="sxs-lookup"><span data-stu-id="40531-151">Access is limited to connecting to the head nodes and (if one exists) the edge node.</span></span> <span data-ttu-id="40531-152">Přístup ke službám systémem o hlavních uzlech není provedena tak, že více hlavních uzlech.</span><span class="sxs-lookup"><span data-stu-id="40531-152">Access to services running on the head nodes is not effected by having multiple head nodes.</span></span> <span data-ttu-id="40531-153">Veřejné brány směruje požadavky k hlavnímu uzlu, který je hostitelem požadované služby.</span><span class="sxs-lookup"><span data-stu-id="40531-153">The public gateway routes requests to the head node that hosts the requested service.</span></span> <span data-ttu-id="40531-154">Například pokud Ambari je aktuálně hostitelem sekundárního hlavního uzlu, bránu směruje příchozí požadavky pro Ambari k tomuto uzlu.</span><span class="sxs-lookup"><span data-stu-id="40531-154">For example, if Ambari is currently hosted on the secondary head node, the gateway routes incoming requests for Ambari to that node.</span></span>

<span data-ttu-id="40531-155">Přístup prostřednictvím veřejného brány je omezený na portu 443 (HTTPS), 22 a 23.</span><span class="sxs-lookup"><span data-stu-id="40531-155">Access over the public gateway is limited to port 443 (HTTPS), 22, and 23.</span></span>

* <span data-ttu-id="40531-156">Port __443__ se používá pro přístup k Ambari a dalších webových uživatelského rozhraní nebo rozhraní REST API hostované o hlavních uzlech.</span><span class="sxs-lookup"><span data-stu-id="40531-156">Port __443__ is used to access Ambari and other web UI or REST APIs hosted on the head nodes.</span></span>

* <span data-ttu-id="40531-157">Port __22__ se používá pro přístup k primární hlavní uzel nebo okraj uzel s SSH.</span><span class="sxs-lookup"><span data-stu-id="40531-157">Port __22__ is used to access the primary head node or edge node with SSH.</span></span>

* <span data-ttu-id="40531-158">Port __23__ se používá pro přístup sekundárního hlavního uzlu pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="40531-158">Port __23__ is used to access the secondary head node with SSH.</span></span> <span data-ttu-id="40531-159">Například `ssh username@mycluster-ssh.azurehdinsight.net` připojí k primární hlavního uzlu v clusteru s názvem **mycluster**.</span><span class="sxs-lookup"><span data-stu-id="40531-159">For example, `ssh username@mycluster-ssh.azurehdinsight.net` connects to the primary head node of the cluster named **mycluster**.</span></span>

<span data-ttu-id="40531-160">Další informace o používání SSH najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="40531-160">For more information on using SSH, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

### <a name="internal-fully-qualified-domain-names-fqdn"></a><span data-ttu-id="40531-161">Interní plně kvalifikované názvy domény (FQDN)</span><span class="sxs-lookup"><span data-stu-id="40531-161">Internal fully qualified domain names (FQDN)</span></span>

<span data-ttu-id="40531-162">Uzly v clusteru služby HDInsight mít interní IP adresu a plně kvalifikovaný název domény, který lze přistupovat pouze z clusteru.</span><span class="sxs-lookup"><span data-stu-id="40531-162">Nodes in an HDInsight cluster have an internal IP address and FQDN that can only be accessed from the cluster.</span></span> <span data-ttu-id="40531-163">Při přístupu k služby v clusteru pomocí interní plně kvalifikovaný název domény nebo IP adresa, měli byste použít Ambari ověření IP adresu nebo plně kvalifikovaný název domény, který má použít při přístupu ke službě.</span><span class="sxs-lookup"><span data-stu-id="40531-163">When accessing services on the cluster using the internal FQDN or IP address, you should use Ambari to verify the IP or FQDN to use when accessing the service.</span></span>

<span data-ttu-id="40531-164">Například službu Oozie lze spustit jen u jednoho hlavního uzlu a pomocí `oozie` příkaz z relace SSH vyžaduje adresu URL pro službu.</span><span class="sxs-lookup"><span data-stu-id="40531-164">For example, the Oozie service can only run on one head node, and using the `oozie` command from an SSH session requires the URL to the service.</span></span> <span data-ttu-id="40531-165">Tato adresa URL může načíst z Ambari pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="40531-165">This URL can be retrieved from Ambari by using the following command:</span></span>

    curl -u admin:PASSWORD "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url

<span data-ttu-id="40531-166">Tento příkaz vrátí hodnotu podobná následující příkaz, který obsahuje interní adresa URL pro použití s `oozie` příkaz:</span><span class="sxs-lookup"><span data-stu-id="40531-166">This command returns a value similar to the following command, which contains the internal URL to use with the `oozie` command:</span></span>

    "oozie.base.url": "http://hn0-CLUSTERNAME-randomcharacters.cx.internal.cloudapp.net:11000/oozie"

<span data-ttu-id="40531-167">Další informace o práci s Ambari REST API najdete v tématu [monitorování a správě HDInsight pomocí Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="40531-167">For more information on working with the Ambari REST API, see [Monitor and Manage HDInsight using the Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).</span></span>

### <a name="accessing-other-node-types"></a><span data-ttu-id="40531-168">Přístup k jiné typy uzlů</span><span class="sxs-lookup"><span data-stu-id="40531-168">Accessing other node types</span></span>

<span data-ttu-id="40531-169">Můžete připojit k uzlů, které nejsou přímo přístupné přes internet pomocí následujících metod:</span><span class="sxs-lookup"><span data-stu-id="40531-169">You can connect to nodes that are not directly accessible over the internet by using the following methods:</span></span>

* <span data-ttu-id="40531-170">**SSH**: Po připojení k hlavnímu uzlu pomocí protokolu SSH, pak můžete SSH z hlavního uzlu se připojit k jiné uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="40531-170">**SSH**: Once connected to a head node using SSH, you can then use SSH from the head node to connect to other nodes in the cluster.</span></span> <span data-ttu-id="40531-171">Další informace najdete v dokumentu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="40531-171">For more information, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

* <span data-ttu-id="40531-172">**Tunelové propojení SSH**: Pokud potřebujete pro přístup k webové službě hostované na jednom z uzlů, které není přístup k Internetu, musíte použít tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="40531-172">**SSH Tunnel**: If you need to access a web service hosted on one of the nodes that is not exposed to the internet, you must use an SSH tunnel.</span></span> <span data-ttu-id="40531-173">Další informace najdete v tématu [použijte tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="40531-173">For more information, see the [Use an SSH tunnel with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

* <span data-ttu-id="40531-174">**Virtuální síť Azure**: Pokud váš cluster HDInsight je součástí virtuální síť Azure, jakýkoli prostředek na stejné virtuální síti přímo přistupovat k všechny uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="40531-174">**Azure Virtual Network**: If your HDInsight cluster is part of an Azure Virtual Network, any resource on the same Virtual Network can directly access all nodes in the cluster.</span></span> <span data-ttu-id="40531-175">Další informace najdete v tématu [rozšířit HDInsight pomocí Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="40531-175">For more information, see the [Extend HDInsight using Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.</span></span>

## <a name="how-to-check-on-a-service-status"></a><span data-ttu-id="40531-176">Tom, jak zkontrolovat stav služby</span><span class="sxs-lookup"><span data-stu-id="40531-176">How to check on a service status</span></span>

<span data-ttu-id="40531-177">Chcete-li zkontrolovat stav služby, které běží o hlavních uzlech, použijte rozhraní Ambari Web nebo Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="40531-177">To check the status of services that run on the head nodes, use the Ambari Web UI or the Ambari REST API.</span></span>

### <a name="ambari-web-ui"></a><span data-ttu-id="40531-178">Webovému uživatelskému rozhraní Ambari</span><span class="sxs-lookup"><span data-stu-id="40531-178">Ambari Web UI</span></span>

<span data-ttu-id="40531-179">Webové uživatelské rozhraní Ambari jsou viditelná na https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="40531-179">The Ambari Web UI is viewable at https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="40531-180">Nahraďte **CLUSTERNAME** názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="40531-180">Replace **CLUSTERNAME** with the name of your cluster.</span></span> <span data-ttu-id="40531-181">Pokud se zobrazí výzva, zadejte přihlašovací údaje HTTP pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="40531-181">If prompted, enter the HTTP user credentials for your cluster.</span></span> <span data-ttu-id="40531-182">Výchozí uživatelské jméno protokolu HTTP je **správce** a heslo je heslo, které jste zadali při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="40531-182">The default HTTP user name is **admin** and the password is the password you entered when creating the cluster.</span></span>

<span data-ttu-id="40531-183">Až přijedete na stránce Ambari, nainstalovaná services jsou uvedeny na levé straně stránky.</span><span class="sxs-lookup"><span data-stu-id="40531-183">When you arrive on the Ambari page, the installed services are listed on the left of the page.</span></span>

![Nainstalovaná services](./media/hdinsight-high-availability-linux/services.png)

<span data-ttu-id="40531-185">Existuje řada ikony, které mohou být zobrazeny vedle služby, který indikuje stav.</span><span class="sxs-lookup"><span data-stu-id="40531-185">There are a series of icons that may appear next to a service to indicate status.</span></span> <span data-ttu-id="40531-186">Žádné výstrahy týkající se služby lze zobrazit pomocí **výstrahy** odkaz v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="40531-186">Any alerts related to a service can be viewed using the **Alerts** link at the top of the page.</span></span> <span data-ttu-id="40531-187">Můžete vybrat každé služby můžete na něm zobrazit další informace.</span><span class="sxs-lookup"><span data-stu-id="40531-187">You can select each service to view more information on it.</span></span>

<span data-ttu-id="40531-188">Když stránku služby obsahuje informace o stavu a konfiguraci každé služby, neposkytuje informace, které hlavního uzlu je služba spuštěná na.</span><span class="sxs-lookup"><span data-stu-id="40531-188">While the service page provides information on the status and configuration of each service, it does not provide information on which head node the service is running on.</span></span> <span data-ttu-id="40531-189">Chcete-li tyto informace zobrazit, použijte **hostitele** odkaz v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="40531-189">To view this information, use the **Hosts** link at the top of the page.</span></span> <span data-ttu-id="40531-190">Tato stránka zobrazuje hostitelích v clusteru, včetně hlavních uzlech.</span><span class="sxs-lookup"><span data-stu-id="40531-190">This page displays hosts within the cluster, including the head nodes.</span></span>

![seznam hostitelů](./media/hdinsight-high-availability-linux/hosts.png)

<span data-ttu-id="40531-192">Výběr odkaz pro jeden z hlavních uzlech zobrazí služeb a komponent spuštěných na tomto uzlu.</span><span class="sxs-lookup"><span data-stu-id="40531-192">Selecting the link for one of the head nodes displays the services and components running on that node.</span></span>

![Stav součásti](./media/hdinsight-high-availability-linux/nodeservices.png)

<span data-ttu-id="40531-194">Další informace o používání Ambari najdete v tématu [monitorování a správě HDInsight pomocí webového uživatelského rozhraní Ambari](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="40531-194">For more information on using Ambari, see [Monitor and manage HDInsight using the Ambari Web UI](hdinsight-hadoop-manage-ambari.md).</span></span>

### <a name="ambari-rest-api"></a><span data-ttu-id="40531-195">Ambari REST API</span><span class="sxs-lookup"><span data-stu-id="40531-195">Ambari REST API</span></span>

<span data-ttu-id="40531-196">Ambari REST API je dostupná přes internet.</span><span class="sxs-lookup"><span data-stu-id="40531-196">The Ambari REST API is available over the internet.</span></span> <span data-ttu-id="40531-197">Veřejné brány HDInsight zpracovává požadavky na směrování k hlavnímu uzlu, který je aktuálně hostitelem rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="40531-197">The HDInsight public gateway handles routing requests to the head node that is currently hosting the REST API.</span></span>

<span data-ttu-id="40531-198">Chcete-li zkontrolovat stav služby pomocí rozhraní Ambari REST API můžete následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="40531-198">You can use the following command to check the state of a service through the Ambari REST API:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state

* <span data-ttu-id="40531-199">Nahraďte **heslo** pomocí protokolu HTTP (správce) hesla.</span><span class="sxs-lookup"><span data-stu-id="40531-199">Replace **PASSWORD** with the HTTP user (admin) account password.</span></span>
* <span data-ttu-id="40531-200">Místo **CLUSTERNAME** zadejte název vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="40531-200">Replace **CLUSTERNAME** with the name of the cluster.</span></span>
* <span data-ttu-id="40531-201">Nahraďte **SERVICENAME** s názvem služby, které chcete zkontrolovat stav.</span><span class="sxs-lookup"><span data-stu-id="40531-201">Replace **SERVICENAME** with the name of the service you want to check the status of.</span></span>

<span data-ttu-id="40531-202">Například zkontrolovat stav **HDFS** služby v clusteru s názvem **mycluster**, s použitím hesla **heslo**, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="40531-202">For example, to check the status of the **HDFS** service on a cluster named **mycluster**, with a password of **password**, you would use the following command:</span></span>

    curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state

<span data-ttu-id="40531-203">Odpověď je podobná následujícím kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="40531-203">The response is similar to the following JSON:</span></span>

    {
      "href" : "http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
      "ServiceInfo" : {
        "cluster_name" : "mycluster",
        "service_name" : "HDFS",
        "state" : "STARTED"
      }
    }

<span data-ttu-id="40531-204">Adresa URL víme, že služba běží aktuálně hlavního uzlu s názvem **hn0 CLUSTERNAME**.</span><span class="sxs-lookup"><span data-stu-id="40531-204">The URL tells us that the service is currently running on a head node named **hn0-CLUSTERNAME**.</span></span>

<span data-ttu-id="40531-205">Stav víme, že služba je aktuálně spuštěna, nebo **ZAČÍNÁME**.</span><span class="sxs-lookup"><span data-stu-id="40531-205">The state tells us that the service is currently running, or **STARTED**.</span></span>

<span data-ttu-id="40531-206">Pokud si nejste jisti, jaké služby jsou nainstalovány v clusteru, můžete použít následující příkaz pro načtení seznamu:</span><span class="sxs-lookup"><span data-stu-id="40531-206">If you do not know what services are installed on the cluster, you can use the following command to retrieve a list:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services

<span data-ttu-id="40531-207">Další informace o práci s Ambari REST API najdete v tématu [monitorování a správě HDInsight pomocí Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="40531-207">For more information on working with the Ambari REST API, see [Monitor and Manage HDInsight using the Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).</span></span>

#### <a name="service-components"></a><span data-ttu-id="40531-208">Součásti služby</span><span class="sxs-lookup"><span data-stu-id="40531-208">Service components</span></span>

<span data-ttu-id="40531-209">Služby mohou obsahovat součásti, které chcete zkontrolovat stav jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="40531-209">Services may contain components that you wish to check the status of individually.</span></span> <span data-ttu-id="40531-210">Například HDFS obsahuje součást, NameNode.</span><span class="sxs-lookup"><span data-stu-id="40531-210">For example, HDFS contains the NameNode component.</span></span> <span data-ttu-id="40531-211">Chcete-li zobrazit informace o součást, bude příkaz vypadat:</span><span class="sxs-lookup"><span data-stu-id="40531-211">To view information on a component, the command would be:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

<span data-ttu-id="40531-212">Pokud si nejste jisti, jaké součásti jsou poskytovány služby, můžete pro načtení seznamu následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="40531-212">If you do not know what components are provided by a service, you can use the following command to retrieve a list:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

## <a name="how-to-access-log-files-on-the-head-nodes"></a><span data-ttu-id="40531-213">Jak získat přístup k souborům protokolu o hlavních uzlech</span><span class="sxs-lookup"><span data-stu-id="40531-213">How to access log files on the head nodes</span></span>

### <a name="ssh"></a><span data-ttu-id="40531-214">SSH</span><span class="sxs-lookup"><span data-stu-id="40531-214">SSH</span></span>

<span data-ttu-id="40531-215">Při připojení k hlavnímu uzlu prostřednictvím SSH, souborů protokolu najdete v části **/var/log**.</span><span class="sxs-lookup"><span data-stu-id="40531-215">While connected to a head node through SSH, log files can be found under **/var/log**.</span></span> <span data-ttu-id="40531-216">Například **/var/log/hadoop-yarn/yarn** neobsahuje protokoly YARN.</span><span class="sxs-lookup"><span data-stu-id="40531-216">For example, **/var/log/hadoop-yarn/yarn** contain logs for YARN.</span></span>

<span data-ttu-id="40531-217">Každý hlavního uzlu mohou obsahovat položky jedinečný protokolu, takže byste měli zkontrolovat protokoly na obě.</span><span class="sxs-lookup"><span data-stu-id="40531-217">Each head node can have unique log entries, so you should check the logs on both.</span></span>

### <a name="sftp"></a><span data-ttu-id="40531-218">SFTP</span><span class="sxs-lookup"><span data-stu-id="40531-218">SFTP</span></span>

<span data-ttu-id="40531-219">Také můžete připojit k hlavnímu uzlu pomocí SSH protokol FTP nebo rozhraní zabezpečení souboru Transfer Protocol (SFTP) a přímo stáhnout soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="40531-219">You can also connect to the head node using the SSH File Transfer Protocol or Secure File Transfer Protocol (SFTP), and download the log files directly.</span></span>

<span data-ttu-id="40531-220">Podobně jako pomocí klienta SSH, při připojování ke clusteru musíte zadat název účtu uživatele SSH a adresa SSH clusteru.</span><span class="sxs-lookup"><span data-stu-id="40531-220">Similar to using an SSH client, when connecting to the cluster you must provide the SSH user account name and the SSH address of the cluster.</span></span> <span data-ttu-id="40531-221">Například, `sftp username@mycluster-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="40531-221">For example, `sftp username@mycluster-ssh.azurehdinsight.net`.</span></span> <span data-ttu-id="40531-222">Zadejte heslo pro účet po zobrazení výzvy, nebo zadejte veřejný klíč pomocí `-i` parametr.</span><span class="sxs-lookup"><span data-stu-id="40531-222">Provide the password for the account when prompted, or provide a public key using the `-i` parameter.</span></span>

<span data-ttu-id="40531-223">Po připojení se zobrazí `sftp>` řádku.</span><span class="sxs-lookup"><span data-stu-id="40531-223">Once connected, you are presented with a `sftp>` prompt.</span></span> <span data-ttu-id="40531-224">Z příkazového řádku, můžete změnit adresáře, nahrávání a stahování souborů.</span><span class="sxs-lookup"><span data-stu-id="40531-224">From this prompt, you can change directories, upload, and download files.</span></span> <span data-ttu-id="40531-225">Například následující příkazy přejděte do adresáře **/var/log/hadoop/hdfs** adresář a pak stáhnout všechny soubory v adresáři.</span><span class="sxs-lookup"><span data-stu-id="40531-225">For example, the following commands change directories to the **/var/log/hadoop/hdfs** directory and then download all files in the directory.</span></span>

    cd /var/log/hadoop/hdfs
    get *

<span data-ttu-id="40531-226">Seznam dostupné příkazy, zadejte `help` na `sftp>` řádku.</span><span class="sxs-lookup"><span data-stu-id="40531-226">For a list of available commands, enter `help` at the `sftp>` prompt.</span></span>

> [!NOTE]
> <span data-ttu-id="40531-227">Existují také grafické rozhraní, které vám umožní vizualizovat systému souborů při připojení pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="40531-227">There are also graphical interfaces that allow you to visualize the file system when connected using SFTP.</span></span> <span data-ttu-id="40531-228">Například [MobaXTerm](http://mobaxterm.mobatek.net/) umožňuje procházet přes rozhraní podobně jako Průzkumník systému souborů.</span><span class="sxs-lookup"><span data-stu-id="40531-228">For example, [MobaXTerm](http://mobaxterm.mobatek.net/) allows you to browse the file system using an interface similar to Windows Explorer.</span></span>

### <a name="ambari"></a><span data-ttu-id="40531-229">Ambari</span><span class="sxs-lookup"><span data-stu-id="40531-229">Ambari</span></span>

> [!NOTE]
> <span data-ttu-id="40531-230">Pro přístup k souborům protokolu pomocí nástroje Ambari, je nutné použít tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="40531-230">To access log files using Ambari, you must use an SSH tunnel.</span></span> <span data-ttu-id="40531-231">Webové rozhraní pro jednotlivé služby nejsou veřejně přístupné v síti Internet.</span><span class="sxs-lookup"><span data-stu-id="40531-231">The web interfaces for the individual services are not exposed publicly on the Internet.</span></span> <span data-ttu-id="40531-232">Informace o používání tunelového propojení SSH naleznete v tématu [používání tunelového propojení SSH](hdinsight-linux-ambari-ssh-tunnel.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="40531-232">For information on using an SSH tunnel, see the [Use SSH Tunneling](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

<span data-ttu-id="40531-233">Webové uživatelské rozhraní Ambari vyberte službu, kterou chcete zobrazit protokoly pro (například YARN).</span><span class="sxs-lookup"><span data-stu-id="40531-233">From the Ambari Web UI, select the service you wish to view logs for (for example, YARN).</span></span> <span data-ttu-id="40531-234">Potom pomocí **rychlé odkazy** vyberte head uzel k zobrazení protokolů pro.</span><span class="sxs-lookup"><span data-stu-id="40531-234">Then use **Quick Links** to select which head node to view the logs for.</span></span>

![Pokud chcete zobrazit protokoly pomocí rychlé odkazy](./media/hdinsight-high-availability-linux/viewlogs.png)

## <a name="how-to-configure-the-node-size"></a><span data-ttu-id="40531-236">Postup konfigurace velikost uzlu</span><span class="sxs-lookup"><span data-stu-id="40531-236">How to configure the node size</span></span>

<span data-ttu-id="40531-237">Velikost uzlu lze vybrat pouze při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="40531-237">The size of a node can only be selected during cluster creation.</span></span> <span data-ttu-id="40531-238">Můžete najít seznam různých velikosti virtuálních počítačů, která je k dispozici pro HDInsight na [HDInsight stránce s cenami](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="40531-238">You can find a list of the different VM sizes available for HDInsight on the [HDInsight pricing page](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

<span data-ttu-id="40531-239">Při vytváření clusteru, můžete zadat velikost uzlů.</span><span class="sxs-lookup"><span data-stu-id="40531-239">When creating a cluster, you can specify the size of the nodes.</span></span> <span data-ttu-id="40531-240">Následující informace poskytují pokyny o tom, jak určit velikost pomocí [portál Azure][preview-portal], [prostředí Azure PowerShell][azure-powershell]a [rozhraní příkazového řádku Azure][azure-cli]:</span><span class="sxs-lookup"><span data-stu-id="40531-240">The following information provides guidance on how to specify the size using the [Azure portal][preview-portal], [Azure PowerShell][azure-powershell], and the [Azure CLI][azure-cli]:</span></span>

* <span data-ttu-id="40531-241">**Portál Azure**: při vytváření clusteru, můžete nastavit velikost uzlů používaný v clusteru:</span><span class="sxs-lookup"><span data-stu-id="40531-241">**Azure portal**: When creating a cluster, you can set the size of the nodes used by the cluster:</span></span>

    ![Obrázek průvodce vytvořením clusteru s výběrem velikost uzlu](./media/hdinsight-high-availability-linux/headnodesize.png)

* <span data-ttu-id="40531-243">**Rozhraní příkazového řádku Azure**: při použití `azure hdinsight cluster create` příkaz, velikost head, pracovního procesu a uzly ZooKeeper lze nastavit pomocí `--headNodeSize`, `--workerNodeSize`, a `--zookeeperNodeSize` parametry.</span><span class="sxs-lookup"><span data-stu-id="40531-243">**Azure CLI**: When using the `azure hdinsight cluster create` command, you can set the size of the head, worker, and ZooKeeper nodes by using the `--headNodeSize`, `--workerNodeSize`, and `--zookeeperNodeSize` parameters.</span></span>

* <span data-ttu-id="40531-244">**Prostředí Azure PowerShell**: při použití `New-AzureRmHDInsightCluster` rutiny, můžete nastavit velikost head, pracovního procesu a uzly ZooKeeper pomocí `-HeadNodeVMSize`, `-WorkerNodeSize`, a `-ZookeeperNodeSize` parametry.</span><span class="sxs-lookup"><span data-stu-id="40531-244">**Azure PowerShell**: When using the `New-AzureRmHDInsightCluster` cmdlet, you can set the size of the head, worker, and ZooKeeper nodes by using the `-HeadNodeVMSize`, `-WorkerNodeSize`, and `-ZookeeperNodeSize` parameters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="40531-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="40531-245">Next steps</span></span>

<span data-ttu-id="40531-246">Pomocí následujících odkazů na další informace o aspektech, které jsou uvedené v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="40531-246">Use the following links to learn more about things mentioned in this document.</span></span>

* [<span data-ttu-id="40531-247">Ambari REST odkaz</span><span class="sxs-lookup"><span data-stu-id="40531-247">Ambari REST Reference</span></span>](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [<span data-ttu-id="40531-248">Instalace a konfigurace rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="40531-248">Install and configure the Azure CLI</span></span>](../cli-install-nodejs.md)
* [<span data-ttu-id="40531-249">Nainstalujte a nakonfigurujte Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="40531-249">Install and configure Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="40531-250">Spravovat HDInsight pomocí Ambari</span><span class="sxs-lookup"><span data-stu-id="40531-250">Manage HDInsight using Ambari</span></span>](hdinsight-hadoop-manage-ambari.md)
* [<span data-ttu-id="40531-251">Zřizování clusterů HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="40531-251">Provision Linux-based HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)

[preview-portal]: https://portal.azure.com/
[azure-powershell]: /powershell/azureps-cmdlets-docs
[azure-cli]: ../cli-install-nodejs.md
