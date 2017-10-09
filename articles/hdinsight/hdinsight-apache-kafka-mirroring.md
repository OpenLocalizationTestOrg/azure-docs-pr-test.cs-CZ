---
title: "témata Apache Kafka aaaMirror - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse Apache Kafka zrcadlení funkci toomaintain repliku Kafka na clusteru HDInsight pomocí zrcadlení témata tooa sekundární clusteru."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 015d276e-f678-4f2b-9572-75553c56625b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/13/2017
ms.author: larryfr
ms.openlocfilehash: 5ace0251d7402d4d7d9b28726e253ce7091a87ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-mirrormaker-tooreplicate-apache-kafka-topics-with-kafka-on-hdinsight-preview"></a><span data-ttu-id="8d5b0-103">Použití MirrorMaker tooreplicate Apache Kafka témata s Kafka v HDInsight (preview)</span><span class="sxs-lookup"><span data-stu-id="8d5b0-103">Use MirrorMaker tooreplicate Apache Kafka topics with Kafka on HDInsight (preview)</span></span>

<span data-ttu-id="8d5b0-104">Zjistěte, jak je toouse Apache Kafka zrcadlení funkce tooreplicate témata tooa sekundární clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-104">Learn how toouse Apache Kafka's mirroring feature tooreplicate topics tooa secondary cluster.</span></span> <span data-ttu-id="8d5b0-105">Zrcadlení lze byla spuštěna jako nepřetržitý proces, nebo použít občas jako metodu migrace dat z jednoho clusteru tooanother.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-105">Mirroring can be ran as a continuous process, or used intermittently as a method of migrating data from one cluster tooanother.</span></span>

<span data-ttu-id="8d5b0-106">V tomto příkladu je zrcadlení použité tooreplicate témata mezi dvěma clustery HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-106">In this example, mirroring is used tooreplicate topics between two HDInsight clusters.</span></span> <span data-ttu-id="8d5b0-107">Oba clustery jsou ve virtuální síti Azure v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-107">Both clusters are in an Azure Virtual Network in hello same region.</span></span>

> [!WARNING]
> <span data-ttu-id="8d5b0-108">Zrcadlení by se neměla považovat jako znamená tooachieve odolnosti proti chybám.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-108">Mirroring should not be considered as a means tooachieve fault-tolerance.</span></span> <span data-ttu-id="8d5b0-109">Posunutí tooitems Hello v tématu se liší mezi zdrojovým a cílovým clustery hello, takže klienti nemohou použít hello dva zcela zaměnitelným významem.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-109">hello offset tooitems within a topic are different between hello source and destination clusters, so clients cannot use hello two interchangeably.</span></span>
>
> <span data-ttu-id="8d5b0-110">Pokud máte obavy o odolnost proti chybám, byste měli nastavit replikace pro hello témata v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-110">If you are concerned about fault tolerance, you should set replication for hello topics within your cluster.</span></span> <span data-ttu-id="8d5b0-111">Další informace najdete v tématu [začít pracovat s Kafka v HDInsight](hdinsight-apache-kafka-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="8d5b0-111">For more information, see [Get started with Kafka on HDInsight](hdinsight-apache-kafka-get-started.md).</span></span>

## <a name="how-kafka-mirroring-works"></a><span data-ttu-id="8d5b0-112">Jak funguje Kafka zrcadlení</span><span class="sxs-lookup"><span data-stu-id="8d5b0-112">How Kafka mirroring works</span></span>

<span data-ttu-id="8d5b0-113">Zrcadlení funguje s použitím hello MirrorMaker nástroj (součást Apache Kafka) tooconsume od témata na hello zdrojovém clusteru a pak vytvořit místní kopii na hello cílový cluster.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-113">Mirroring works by using hello MirrorMaker tool (part of Apache Kafka) tooconsume records from topics on hello source cluster and then create a local copy on hello destination cluster.</span></span> <span data-ttu-id="8d5b0-114">MirrorMaker používá (nejméně jeden) *příjemci* který číst z hello zdrojovém clusteru a *producent* , zapíše toohello místní (cíl) clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-114">MirrorMaker uses one (or more) *consumers* that read from hello source cluster, and a *producer* that writes toohello local (destination) cluster.</span></span>

<span data-ttu-id="8d5b0-115">Hello následující diagram znázorňuje proces zrcadlení hello:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-115">hello following diagram illustrates hello Mirroring process:</span></span>

![Diagram hello zrcadlení procesu](./media/hdinsight-apache-kafka-mirroring/kafka-mirroring.png)

<span data-ttu-id="8d5b0-117">Apache Kafka v HDInsight neposkytuje oproti hello přístup toohello Kafka služby veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-117">Apache Kafka on HDInsight does not provide access toohello Kafka service over hello public internet.</span></span> <span data-ttu-id="8d5b0-118">Producenti Kafka nebo příjemci musí být v hello stejné virtuální síti Azure jako hello uzly v clusteru Kafka hello.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-118">Kafka producers or consumers must be in hello same Azure virtual network as hello nodes in hello Kafka cluster.</span></span> <span data-ttu-id="8d5b0-119">V tomto příkladu hello Kafka zdroje a cílových clusterech jsou umístěny ve virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-119">For this example, both hello Kafka source and destination clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="8d5b0-120">Hello následující diagram znázorňuje tok komunikace mezi clustery hello:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-120">hello following diagram shows how communication flows between hello clusters:</span></span>

![Diagram zdrojové a cílové Kafka clusterů v virtuální sítě Azure](./media/hdinsight-apache-kafka-mirroring/spark-kafka-vnet.png)

<span data-ttu-id="8d5b0-122">Hello zdrojové a cílové clustery mohou být různé hello počtu uzlů a oddíly a odsazení v rámci témata hello se také liší.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-122">hello source and destination clusters can be different in hello number of nodes and partitions, and offsets within hello topics are different also.</span></span> <span data-ttu-id="8d5b0-123">Zrcadlení udržuje hello hodnotu klíče, který se používá pro vytváření oddílů, takže pořadí záznamů se zachová, i na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-123">Mirroring maintains hello key value that is used for partitioning, so record order is preserved on a per-key basis.</span></span>

### <a name="mirroring-across-network-boundaries"></a><span data-ttu-id="8d5b0-124">Zrcadlení napříč síťovými hranicemi</span><span class="sxs-lookup"><span data-stu-id="8d5b0-124">Mirroring across network boundaries</span></span>

<span data-ttu-id="8d5b0-125">Pokud potřebujete toomirror mezi clustery Kafka v jiných sítích, existují hello následující další důležité informace:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-125">If you need toomirror between Kafka clusters in different networks, there are hello following additional considerations:</span></span>

* <span data-ttu-id="8d5b0-126">**Brány**: hello sítě musí být schopný toocommunicate v hello TCPIP úroveň.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-126">**Gateways**: hello networks must be able toocommunicate at hello TCPIP level.</span></span>

* <span data-ttu-id="8d5b0-127">**Překlad názvů**: hello Kafka clusterů v každé sítě musí být schopný tooconnect tooeach jiných pomocí názvy hostitelů.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-127">**Name resolution**: hello Kafka clusters in each network must be able tooconnect tooeach other by using hostnames.</span></span> <span data-ttu-id="8d5b0-128">To může vyžadovat, že server systému DNS (Domain Name) v každé sítě, která je nakonfigurovaná tooforward požadavky toohello další sítě.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-128">This may require a Domain Name System (DNS) server in each network that is configured tooforward requests toohello other networks.</span></span>

    <span data-ttu-id="8d5b0-129">Při vytváření virtuální síť Azure, místo použití hello poskytnuté automatické DNS s hello síť, musíte zadat vlastní DNS serveru a hello IP adresu pro hello server.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-129">When creating an Azure Virtual Network, instead of using hello automatic DNS provided with hello network, you must specify a custom DNS server and hello IP address for hello server.</span></span> <span data-ttu-id="8d5b0-130">Po hello, který byl vytvořen virtuální sítě musí pak vytvořit virtuální počítač Azure, který používá IP adresu, pak nainstalujte a nakonfigurujte DNS software na něm.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-130">After hello Virtual Network has been created, you must then create an Azure Virtual Machine that uses that IP address, then install and configure DNS software on it.</span></span>

    > [!WARNING]
    > <span data-ttu-id="8d5b0-131">Vytvořit a nakonfigurovat hello vlastního serveru DNS před instalací HDInsight do hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-131">Create and configure hello custom DNS server before installing HDInsight into hello Virtual Network.</span></span> <span data-ttu-id="8d5b0-132">Neexistuje žádná další konfigurace požadované pro HDInsight toouse hello server DNS nakonfigurovaný pro hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-132">There is no additional configuration required for HDInsight toouse hello DNS server configured for hello Virtual Network.</span></span>

<span data-ttu-id="8d5b0-133">Další informace o připojení dvou virtuálních sítí Azure najdete v tématu [konfigurace připojení typu VNet-to-VNet](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="8d5b0-133">For more information on connecting two Azure Virtual Networks, see [Configure a VNet-to-VNet connection](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span></span>

## <a name="create-kafka-clusters"></a><span data-ttu-id="8d5b0-134">Vytvoření Kafka clusterů</span><span class="sxs-lookup"><span data-stu-id="8d5b0-134">Create Kafka clusters</span></span>

<span data-ttu-id="8d5b0-135">Když vytvoříte virtuální síť Azure a Kafka clusterů ručně, je snazší toouse šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-135">While you can create an Azure virtual network and Kafka clusters manually, it's easier toouse an Azure Resource Manager template.</span></span> <span data-ttu-id="8d5b0-136">Použijte následující kroky toodeploy hello virtuální sítě Azure a dvě Kafka clustery tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-136">Use hello following steps toodeploy an Azure virtual network and two Kafka clusters tooyour Azure subscription.</span></span>

1. <span data-ttu-id="8d5b0-137">Použijte hello tlačítko toosign v tooAzure a otevřete hello šablony v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-137">Use hello following button toosign in tooAzure and open hello template in hello Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-kafka-mirroring/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   
    <span data-ttu-id="8d5b0-138">Hello šablony Azure Resource Manageru se nachází v **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-138">hello Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="8d5b0-139">dostupnost tooguarantee Kafka v HDInsight, cluster musí obsahovat aspoň tři uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-139">tooguarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="8d5b0-140">Tato šablona vytvoří cluster Kafka, který obsahuje tři uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-140">This template creates a Kafka cluster that contains three worker nodes.</span></span>

2. <span data-ttu-id="8d5b0-141">Hello použijte následující informace toopopulate hello položky na hello **vlastní nasazení** okno:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-141">Use hello following information toopopulate hello entries on hello **Custom deployment** blade:</span></span>
    
    ![HDInsight vlastní nasazení](./media/hdinsight-apache-kafka-mirroring/parameters.png)
    
    * <span data-ttu-id="8d5b0-143">**Skupina prostředků**: vytvoření skupiny nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-143">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="8d5b0-144">Tato skupina obsahuje hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-144">This group contains hello HDInsight cluster.</span></span>

    * <span data-ttu-id="8d5b0-145">**Umístění**: Vyberte tooyou geograficky zavřít umístění.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-145">**Location**: Select a location geographically close tooyou.</span></span>
     
    * <span data-ttu-id="8d5b0-146">**Základní název clusteru**: Tato hodnota se používá jako hello základní název pro hello Kafka clusterů.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-146">**Base Cluster Name**: This value is used as hello base name for hello Kafka clusters.</span></span> <span data-ttu-id="8d5b0-147">Například zadáním **hdi** vytvoří clustery s názvem **zdroj hdi** a **cíle hdi**.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-147">For example, entering **hdi** creates clusters named **source-hdi** and **dest-hdi**.</span></span>

    * <span data-ttu-id="8d5b0-148">**Uživatelské jméno přihlášení clusteru**: hello uživatelské jméno správce pro hello zdrojové a cílové Kafka clusterů.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-148">**Cluster Login User Name**: hello admin user name for hello source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="8d5b0-149">**Heslo pro přihlášení clusteru**: heslo uživatele správce hello k hello zdrojové a cílové Kafka clusterů.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-149">**Cluster Login Password**: hello admin user password for hello source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="8d5b0-150">**Uživatelské jméno SSH**: hello SSH uživatele toocreate pro hello zdrojové a cílové Kafka clusterů.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-150">**SSH User Name**: hello SSH user toocreate for hello source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="8d5b0-151">**Heslo SSH**: hello heslo pro uživatele SSH hello hello zdrojové a cílové Kafka clusterů.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-151">**SSH Password**: hello password for hello SSH user for hello source and destination Kafka clusters.</span></span>

3. <span data-ttu-id="8d5b0-152">Čtení hello **podmínky a ujednání**a potom vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-152">Read hello **Terms and Conditions**, and then select **I agree toohello terms and conditions stated above**.</span></span>

4. <span data-ttu-id="8d5b0-153">Nakonec zkontrolujte **Pin toodashboard** a pak vyberte **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-153">Finally, check **Pin toodashboard** and then select **Purchase**.</span></span> <span data-ttu-id="8d5b0-154">Trvá přibližně 20 minut toocreate hello clustery.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-154">It takes about 20 minutes toocreate hello clusters.</span></span>

<span data-ttu-id="8d5b0-155">Po vytvoření hello prostředky jste přesměrovaného tooa okně hello skupinu prostředků, která obsahuje hello clustery a webové řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-155">Once hello resources have been created, you are redirected tooa blade for hello resource group that contains hello clusters and web dashboard.</span></span>

![Okno skupiny prostředků pro virtuální síť hello a clustery](./media/hdinsight-apache-kafka-mirroring/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="8d5b0-157">Všimněte si, že jsou názvy hello clusterů HDInsight hello **zdroj BASENAME** a **cíle BASENAME**, kde BASENAME je hello jméno, které jste zadali toohello šablony.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-157">Notice that hello names of hello HDInsight clusters are **source-BASENAME** and **dest-BASENAME**, where BASENAME is hello name you provided toohello template.</span></span> <span data-ttu-id="8d5b0-158">Použít tyto názvy v dalších krocích při připojení toohello clustery.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-158">You use these names in later steps when connecting toohello clusters.</span></span>

## <a name="create-topics"></a><span data-ttu-id="8d5b0-159">Vytvoření témata</span><span class="sxs-lookup"><span data-stu-id="8d5b0-159">Create topics</span></span>

1. <span data-ttu-id="8d5b0-160">Připojit toohello **zdroj** clusteru pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-160">Connect toohello **source** cluster using SSH:</span></span>

    ```bash
    ssh sshuser@source-BASENAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="8d5b0-161">Nahraďte **sshuser** s uživatelským jménem SSH hello používá při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-161">Replace **sshuser** with hello SSH user name used when creating hello cluster.</span></span> <span data-ttu-id="8d5b0-162">Nahraďte **BASENAME** základní názvem hello používá při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-162">Replace **BASENAME** with hello base name used when creating hello cluster.</span></span>

    <span data-ttu-id="8d5b0-163">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="8d5b0-163">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="8d5b0-164">Použití hello následující příkazy toofind hello Zookeeper hostitele pro zdrojový cluster hello:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-164">Use hello following commands toofind hello Zookeeper hosts for hello source cluster:</span></span>

    ```bash
    # Install jq if it is not installed
    sudo apt -y install jq
    # get hello zookeeper hosts for hello source cluster
    export SOURCE_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    
    Replace `$PASSWORD` with hello password for hello cluster.

    Replace `$CLUSTERNAME` with hello name of hello source cluster.

3. toocreate a topic named `testtopic`, use hello following command:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SOURCE_ZKHOSTS
    ```

3. <span data-ttu-id="8d5b0-165">Byla vytvořena hello použijte následující příkaz tooverify, který hello tématu:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-165">Use hello following command tooverify that hello topic was created:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $SOURCE_ZKHOSTS
    ```

    <span data-ttu-id="8d5b0-166">Hello odpovědi obsahuje `testtopic`.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-166">hello response contains `testtopic`.</span></span>

4. <span data-ttu-id="8d5b0-167">Použití hello následující tooview hello Zookeeper hostitele informace pro tento (hello **zdroj**) clusteru:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-167">Use hello following tooview hello Zookeeper host information for this (hello **source**) cluster:</span></span>

    ```bash
    echo $SOURCE_ZKHOSTS
    ```

    <span data-ttu-id="8d5b0-168">Tento příkaz vrátí informace podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-168">This returns information similar toohello following text:</span></span>

    `zk0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181,zk1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181`

    <span data-ttu-id="8d5b0-169">Tyto informace uložte.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-169">Save this information.</span></span> <span data-ttu-id="8d5b0-170">Používá se v další části hello.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-170">It is used in hello next section.</span></span>

## <a name="configure-mirroring"></a><span data-ttu-id="8d5b0-171">Konfigurace zrcadlení</span><span class="sxs-lookup"><span data-stu-id="8d5b0-171">Configure mirroring</span></span>

1. <span data-ttu-id="8d5b0-172">Připojit toohello **cílové** clusteru pomocí jiné relace SSH:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-172">Connect toohello **destination** cluster using a different SSH session:</span></span>

    ```bash
    ssh sshuser@dest-BASENAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="8d5b0-173">Nahraďte **sshuser** s uživatelským jménem SSH hello používá při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-173">Replace **sshuser** with hello SSH user name used when creating hello cluster.</span></span> <span data-ttu-id="8d5b0-174">Nahraďte **BASENAME** základní názvem hello používá při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-174">Replace **BASENAME** with hello base name used when creating hello cluster.</span></span>

    <span data-ttu-id="8d5b0-175">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="8d5b0-175">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="8d5b0-176">Použití hello následující příkaz toocreate `consumer.properties` soubor, který popisuje, jak toocommunicate s hello **zdroj** clusteru:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-176">Use hello following command toocreate a `consumer.properties` file that describes how toocommunicate with hello **source** cluster:</span></span>

    ```bash
    nano consumer.properties
    ```

    <span data-ttu-id="8d5b0-177">Použití hello následující text jako hello obsah hello `consumer.properties` souboru:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-177">Use hello following text as hello contents of hello `consumer.properties` file:</span></span>

    ```yaml
    zookeeper.connect=SOURCE_ZKHOSTS
    group.id=mirrorgroup
    ```

    <span data-ttu-id="8d5b0-178">Nahraďte **SOURCE_ZKHOSTS** s hello Zookeeper hostitelem informace z hello **zdroj** clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-178">Replace **SOURCE_ZKHOSTS** with hello Zookeeper hosts information from hello **source** cluster.</span></span>

    <span data-ttu-id="8d5b0-179">Tento soubor popisuje hello příjemce informace toouse při čtení ze zdroje hello Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-179">This file describes hello consumer information toouse when reading from hello source Kafka cluster.</span></span> <span data-ttu-id="8d5b0-180">Další informace o uživatelských nastavení, naleznete v tématu [příjemce konfigurací](https://kafka.apache.org/documentation#consumerconfigs) v kafka.apache.org.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-180">For more information consumer configuration, see [Consumer Configs](https://kafka.apache.org/documentation#consumerconfigs) at kafka.apache.org.</span></span>

    <span data-ttu-id="8d5b0-181">toosave hello soubor, použijte **kombinaci kláves Ctrl + X**, **Y**a potom **Enter**.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-181">toosave hello file, use **Ctrl + X**, **Y**, and then **Enter**.</span></span>

3. <span data-ttu-id="8d5b0-182">Před konfigurací hello producent, který komunikuje s hello cílový cluster, musíte vyhledat hello zprostředkovatele hostitele pro hello **cílové** clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-182">Before configuring hello producer that communicates with hello destination cluster, you must find hello broker hosts for hello **destination** cluster.</span></span> <span data-ttu-id="8d5b0-183">Použijte následující příkazy tooretrieve hello tyto informace:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-183">Use hello following commands tooretrieve this information:</span></span>

    ```bash
    sudo apt -y install jq
    DEST_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $DEST_BROKERHOSTS
    ```

    <span data-ttu-id="8d5b0-184">Nahraďte `$PASSWORD` s hello přihlášení heslo účtu (správce) pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-184">Replace `$PASSWORD` with hello login account (admin) password for hello cluster.</span></span>

    <span data-ttu-id="8d5b0-185">Nahraďte `$CLUSTERNAME` s názvem hello hello cílový cluster.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-185">Replace `$CLUSTERNAME` with hello name of hello destination cluster.</span></span>

    <span data-ttu-id="8d5b0-186">Tyto příkazy vrátit podobné toohello následující informace:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-186">These commands return information similar toohello following:</span></span>

        wn0-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn1-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092

4. <span data-ttu-id="8d5b0-187">Použití hello následující toocreate `producer.properties` soubor, který popisuje, jak toocommunicate s hello **cílové** clusteru:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-187">Use hello following toocreate a `producer.properties` file that describes how toocommunicate with hello **destination** cluster:</span></span>

    ```bash
    nano producer.properties
    ```

    <span data-ttu-id="8d5b0-188">Použití hello následující text jako hello obsah hello `producer.properties` souboru:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-188">Use hello following text as hello contents of hello `producer.properties` file:</span></span>

    ```yaml
    bootstrap.servers=DEST_BROKERS
    compression.type=none
    ```

    <span data-ttu-id="8d5b0-189">Nahraďte **DEST_BROKERS** s informacemi o zprostředkovatele hello hello v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-189">Replace **DEST_BROKERS** with hello broker information from hello previous step.</span></span>

    <span data-ttu-id="8d5b0-190">Další informace o producent konfigurace, najdete v části [producent konfigurací](https://kafka.apache.org/documentation#producerconfigs) v kafka.apache.org.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-190">For more information producer configuration, see [Producer Configs](https://kafka.apache.org/documentation#producerconfigs) at kafka.apache.org.</span></span>

## <a name="start-mirrormaker"></a><span data-ttu-id="8d5b0-191">Spustit MirrorMaker</span><span class="sxs-lookup"><span data-stu-id="8d5b0-191">Start MirrorMaker</span></span>

1. <span data-ttu-id="8d5b0-192">Z toohello připojení SSH hello **cílové** clusteru, použijte následující příkaz toostart hello MirrorMaker proces hello:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-192">From hello SSH connection toohello **destination** cluster, use hello following command toostart hello MirrorMaker process:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    <span data-ttu-id="8d5b0-193">Hello parametry použité v tomto příkladu jsou:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-193">hello parameters used in this example are:</span></span>

    * <span data-ttu-id="8d5b0-194">**--consumer.config**: Určuje hello soubor, který obsahuje příjemce vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-194">**--consumer.config**: Specifies hello file that contains consumer properties.</span></span> <span data-ttu-id="8d5b0-195">Tyto vlastnosti jsou použité toocreate příjemce, který čte z hello *zdroj* Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-195">These properties are used toocreate a consumer that reads from hello *source* Kafka cluster.</span></span>

    * <span data-ttu-id="8d5b0-196">**--producer.config**: Určuje hello soubor, který obsahuje producent vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-196">**--producer.config**: Specifies hello file that contains producer properties.</span></span> <span data-ttu-id="8d5b0-197">Tyto vlastnosti jsou použité toocreate producent, který zapíše toohello *cílové* Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-197">These properties are used toocreate a producer that writes toohello *destination* Kafka cluster.</span></span>

    * <span data-ttu-id="8d5b0-198">**seznam povolených adres –**: seznam témat, která replikuje MirrorMaker z hello zdroj clusteru toohello cíl.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-198">**--whitelist**: A list of topics that MirrorMaker replicates from hello source cluster toohello destination.</span></span>

    * <span data-ttu-id="8d5b0-199">**--num.streams**: hello počet vláken toocreate příjemce.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-199">**--num.streams**: hello number of consumer threads toocreate.</span></span>

 <span data-ttu-id="8d5b0-200">Při spuštění vrátí MirrorMaker informace podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-200">On startup, MirrorMaker returns information similar toohello following text:</span></span>

    ```json
    {metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-3, security.protocol=PLAINTEXT}{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-0, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-kafka.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-2, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-1, security.protocol=PLAINTEXT}
    ```

2. <span data-ttu-id="8d5b0-201">Z toohello připojení SSH hello **zdroj** clusteru, použijte následující příkaz toostart producent hello a odesílat zprávy toohello tématu:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-201">From hello SSH connection toohello **source** cluster, use hello following command toostart a producer and send messages toohello topic:</span></span>

    ```bash
    SOURCE_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

    <span data-ttu-id="8d5b0-202">Nahraďte `$PASSWORD` s hello heslo pro přihlášení (správce) pro zdrojový cluster hello.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-202">Replace `$PASSWORD` with hello login (admin) password for hello source cluster.</span></span>

    <span data-ttu-id="8d5b0-203">Nahraďte `$CLUSTERNAME` s názvem hello hello zdrojového clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-203">Replace `$CLUSTERNAME` with hello name of hello source cluster.</span></span>

     <span data-ttu-id="8d5b0-204">Až přijedete do prázdný řádek s kurzoru, zadejte několik textové zprávy.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-204">When you arrive at a blank line with a cursor, type in a few text messages.</span></span> <span data-ttu-id="8d5b0-205">Tyto jsou odesílány toohello tématu na hello **zdroj** clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-205">These are sent toohello topic on hello **source** cluster.</span></span> <span data-ttu-id="8d5b0-206">Až budete hotoví, použijte **kombinaci kláves Ctrl + C** tooend hello producent procesu.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-206">When done, use **Ctrl + C** tooend hello producer process.</span></span>

3. <span data-ttu-id="8d5b0-207">Z toohello připojení SSH hello **cílové** clusteru, použijte **kombinaci kláves Ctrl + C** tooend hello MirrorMaker procesu.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-207">From hello SSH connection toohello **destination** cluster, use **Ctrl + C** tooend hello MirrorMaker process.</span></span> <span data-ttu-id="8d5b0-208">Pak použijte hello následující příkazy tooverify této hello `testtopic` tématu byl vytvořen a tato data v tématu hello byl replikované toothis zrcadlení:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-208">Then use hello following commands tooverify that hello `testtopic` topic was created, and that data in hello topic was replicated toothis mirror:</span></span>

    ```bash
    DEST_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $DEST_ZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $DEST_ZKHOSTS --topic testtopic --from-beginning
    ```

    <span data-ttu-id="8d5b0-209">Nahraďte `$PASSWORD` s hello heslo pro přihlášení (správce) pro hello cílový cluster.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-209">Replace `$PASSWORD` with hello login (admin) password for hello destination cluster.</span></span>

    <span data-ttu-id="8d5b0-210">Nahraďte `$CLUSTERNAME` s názvem hello hello cílový cluster.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-210">Replace `$CLUSTERNAME` with hello name of hello destination cluster.</span></span>

    <span data-ttu-id="8d5b0-211">Hello seznam témat nyní zahrnuje `testtopic`, který se vytvoří při MirrorMaster zrcadlí hello téma z hello zdroj clusteru toohello cíl.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-211">hello list of topics now includes `testtopic`, which is created when MirrorMaster mirrors hello topic from hello source cluster toohello destination.</span></span> <span data-ttu-id="8d5b0-212">zprávy Hello načtena z tématu hello jsou, stejně jako na zdrojovém clusteru hello zadán hello.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-212">hello messages retrieved from hello topic are hello same as entered on hello source cluster.</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="8d5b0-213">Odstranění clusteru hello</span><span class="sxs-lookup"><span data-stu-id="8d5b0-213">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="8d5b0-214">Vzhledem k tomu, že hello kroky v tomto dokumentu vytvořte oba clustery v hello stejnou skupinu prostředků Azure, můžete odstranit skupinu prostředků hello hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-214">Since hello steps in this document create both clusters in hello same Azure resource group, you can delete hello resource group in hello Azure portal.</span></span> <span data-ttu-id="8d5b0-215">Odstraněním skupiny prostředků hello odebere všechny prostředky, které jsou vytvořené pomocí následujících tento dokument, hello virtuální sítě Azure a účet úložiště, které jsou používané clustery hello.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-215">Deleting hello resource group removes all resources created by following this document, hello Azure Virtual Network, and storage account used by hello clusters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d5b0-216">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8d5b0-216">Next Steps</span></span>

<span data-ttu-id="8d5b0-217">V tomto dokumentu jste zjistili, jak toouse MirrorMaker toocreate repliku Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-217">In this document, you learned how toouse MirrorMaker toocreate a replica of a Kafka cluster.</span></span> <span data-ttu-id="8d5b0-218">Použijte následující odkazy toodiscover hello jiné způsoby toowork s Kafka:</span><span class="sxs-lookup"><span data-stu-id="8d5b0-218">Use hello following links toodiscover other ways toowork with Kafka:</span></span>

* <span data-ttu-id="8d5b0-219">[Dokumentaci Apache Kafka MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) v cwiki.apache.org.</span><span class="sxs-lookup"><span data-stu-id="8d5b0-219">[Apache Kafka MirrorMaker documentation](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) at cwiki.apache.org.</span></span>
* [<span data-ttu-id="8d5b0-220">Začínáme s Apache Kafka v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8d5b0-220">Get started with Apache Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="8d5b0-221">Použití Apache Sparku se systémem Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="8d5b0-221">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)
* [<span data-ttu-id="8d5b0-222">Použití Apache Stormu se systémem Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="8d5b0-222">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)
* [<span data-ttu-id="8d5b0-223">Připojení tooKafka přes virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="8d5b0-223">Connect tooKafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)
