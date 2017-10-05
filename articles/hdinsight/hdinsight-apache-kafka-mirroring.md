---
title: "Zrcadlení Apache Kafka témata - Azure HDInsight | Microsoft Docs"
description: "Další informace o použití funkce zrcadlení Apache Kafka udržovat zrcadlením témata do clusteru s podporou sekundární repliku Kafka na clusteru HDInsight."
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
ms.openlocfilehash: e418cb01e1a9168e3662e8d6242903e052b6047b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-mirrormaker-to-replicate-apache-kafka-topics-with-kafka-on-hdinsight-preview"></a><span data-ttu-id="0e823-103">Použití MirrorMaker k replikaci Apache Kafka témata s Kafka v HDInsight (preview)</span><span class="sxs-lookup"><span data-stu-id="0e823-103">Use MirrorMaker to replicate Apache Kafka topics with Kafka on HDInsight (preview)</span></span>

<span data-ttu-id="0e823-104">Zjistěte, jak použít funkci zrcadlení Apache Kafka k replikaci témata do clusteru s podporou sekundární.</span><span class="sxs-lookup"><span data-stu-id="0e823-104">Learn how to use Apache Kafka's mirroring feature to replicate topics to a secondary cluster.</span></span> <span data-ttu-id="0e823-105">Zrcadlení lze byla spuštěna jako nepřetržitý proces, nebo použít občas jako metodu migrace dat z jednoho clusteru do druhého.</span><span class="sxs-lookup"><span data-stu-id="0e823-105">Mirroring can be ran as a continuous process, or used intermittently as a method of migrating data from one cluster to another.</span></span>

<span data-ttu-id="0e823-106">V tomto příkladu je zrcadlení používanou k replikaci témata mezi dvěma clustery HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0e823-106">In this example, mirroring is used to replicate topics between two HDInsight clusters.</span></span> <span data-ttu-id="0e823-107">Oba clustery jsou ve virtuální síti Azure ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="0e823-107">Both clusters are in an Azure Virtual Network in the same region.</span></span>

> [!WARNING]
> <span data-ttu-id="0e823-108">Zrcadlení by se neměla považovat jako prostředky k zajištění odolnosti proti chybám.</span><span class="sxs-lookup"><span data-stu-id="0e823-108">Mirroring should not be considered as a means to achieve fault-tolerance.</span></span> <span data-ttu-id="0e823-109">Posun na položky v rámci téma jsou rozdíly mezi zdrojovým a cílovým clustery, takže klienti nemohou použít dva zcela zaměnitelným významem.</span><span class="sxs-lookup"><span data-stu-id="0e823-109">The offset to items within a topic are different between the source and destination clusters, so clients cannot use the two interchangeably.</span></span>
>
> <span data-ttu-id="0e823-110">Pokud máte obavy o odolnost proti chybám, byste měli nastavit replikace pro témata v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-110">If you are concerned about fault tolerance, you should set replication for the topics within your cluster.</span></span> <span data-ttu-id="0e823-111">Další informace najdete v tématu [začít pracovat s Kafka v HDInsight](hdinsight-apache-kafka-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0e823-111">For more information, see [Get started with Kafka on HDInsight](hdinsight-apache-kafka-get-started.md).</span></span>

## <a name="how-kafka-mirroring-works"></a><span data-ttu-id="0e823-112">Jak funguje Kafka zrcadlení</span><span class="sxs-lookup"><span data-stu-id="0e823-112">How Kafka mirroring works</span></span>

<span data-ttu-id="0e823-113">Zrcadlení funguje pomocí nástroje MirrorMaker (součást Apache Kafka) na využívat záznamy ze témata ve zdrojovém clusteru a pak vytvořit místní kopii v cílovém clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-113">Mirroring works by using the MirrorMaker tool (part of Apache Kafka) to consume records from topics on the source cluster and then create a local copy on the destination cluster.</span></span> <span data-ttu-id="0e823-114">MirrorMaker používá (nejméně jeden) *příjemci* který číst ze zdrojového clusteru a *producent* , zapíše do místní (cíl) clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-114">MirrorMaker uses one (or more) *consumers* that read from the source cluster, and a *producer* that writes to the local (destination) cluster.</span></span>

<span data-ttu-id="0e823-115">Následující diagram znázorňuje proces zrcadlení:</span><span class="sxs-lookup"><span data-stu-id="0e823-115">The following diagram illustrates the Mirroring process:</span></span>

![Diagram procesu zrcadlení](./media/hdinsight-apache-kafka-mirroring/kafka-mirroring.png)

<span data-ttu-id="0e823-117">Apache Kafka v HDInsight neposkytuje přístup ke službě Kafka prostřednictvím veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="0e823-117">Apache Kafka on HDInsight does not provide access to the Kafka service over the public internet.</span></span> <span data-ttu-id="0e823-118">Producenti Kafka nebo příjemci musí být ve stejné virtuální síti Azure jako uzly v clusteru Kafka.</span><span class="sxs-lookup"><span data-stu-id="0e823-118">Kafka producers or consumers must be in the same Azure virtual network as the nodes in the Kafka cluster.</span></span> <span data-ttu-id="0e823-119">V tomto příkladu jsou obě Kafka zdrojové a cílové clusterů umístěné v virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="0e823-119">For this example, both the Kafka source and destination clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="0e823-120">Následující diagram znázorňuje tok komunikace mezi clustery:</span><span class="sxs-lookup"><span data-stu-id="0e823-120">The following diagram shows how communication flows between the clusters:</span></span>

![Diagram zdrojové a cílové Kafka clusterů v virtuální sítě Azure](./media/hdinsight-apache-kafka-mirroring/spark-kafka-vnet.png)

<span data-ttu-id="0e823-122">Zdrojové a cílové clustery se může lišit v počtu uzlů a oddíly a odsazení v rámci témata se také liší.</span><span class="sxs-lookup"><span data-stu-id="0e823-122">The source and destination clusters can be different in the number of nodes and partitions, and offsets within the topics are different also.</span></span> <span data-ttu-id="0e823-123">Hodnota klíče, který se používá pro vytváření oddílů, zrcadlení udržuje, takže pořadí záznamů se zachová, i na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="0e823-123">Mirroring maintains the key value that is used for partitioning, so record order is preserved on a per-key basis.</span></span>

### <a name="mirroring-across-network-boundaries"></a><span data-ttu-id="0e823-124">Zrcadlení napříč síťovými hranicemi</span><span class="sxs-lookup"><span data-stu-id="0e823-124">Mirroring across network boundaries</span></span>

<span data-ttu-id="0e823-125">Pokud potřebujete zrcadlení mezi clustery Kafka v jiných sítích, existují následující další aspekty:</span><span class="sxs-lookup"><span data-stu-id="0e823-125">If you need to mirror between Kafka clusters in different networks, there are the following additional considerations:</span></span>

* <span data-ttu-id="0e823-126">**Brány**: sítě musí být schopný komunikovat na úrovni protokolu TCPIP.</span><span class="sxs-lookup"><span data-stu-id="0e823-126">**Gateways**: The networks must be able to communicate at the TCPIP level.</span></span>

* <span data-ttu-id="0e823-127">**Překlad názvů**: The Kafka clustery v každé sítě musí být schopný se připojit k sobě navzájem pomocí názvy hostitelů.</span><span class="sxs-lookup"><span data-stu-id="0e823-127">**Name resolution**: The Kafka clusters in each network must be able to connect to each other by using hostnames.</span></span> <span data-ttu-id="0e823-128">Může to vyžadovat systému DNS (Domain Name) server v každé sítě, který je nakonfigurovaný pro směrování požadavků k jiným sítím.</span><span class="sxs-lookup"><span data-stu-id="0e823-128">This may require a Domain Name System (DNS) server in each network that is configured to forward requests to the other networks.</span></span>

    <span data-ttu-id="0e823-129">Při vytváření virtuální síť Azure, místo použití automatické DNS součástí sítě, je nutné zadat vlastního serveru DNS a IP adresu serveru.</span><span class="sxs-lookup"><span data-stu-id="0e823-129">When creating an Azure Virtual Network, instead of using the automatic DNS provided with the network, you must specify a custom DNS server and the IP address for the server.</span></span> <span data-ttu-id="0e823-130">Po vytvoření virtuální sítě, můžete musí poté vytvořte virtuální počítač Azure, který používá IP adresu, pak nainstalujte a nakonfigurujte DNS software na něm.</span><span class="sxs-lookup"><span data-stu-id="0e823-130">After the Virtual Network has been created, you must then create an Azure Virtual Machine that uses that IP address, then install and configure DNS software on it.</span></span>

    > [!WARNING]
    > <span data-ttu-id="0e823-131">Vytvoření a konfigurace vlastního serveru DNS před instalací HDInsight do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="0e823-131">Create and configure the custom DNS server before installing HDInsight into the Virtual Network.</span></span> <span data-ttu-id="0e823-132">Neexistuje žádná další konfigurace požadované pro HDInsight použít server DNS nakonfigurovaný pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="0e823-132">There is no additional configuration required for HDInsight to use the DNS server configured for the Virtual Network.</span></span>

<span data-ttu-id="0e823-133">Další informace o připojení dvou virtuálních sítí Azure najdete v tématu [konfigurace připojení typu VNet-to-VNet](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="0e823-133">For more information on connecting two Azure Virtual Networks, see [Configure a VNet-to-VNet connection](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span></span>

## <a name="create-kafka-clusters"></a><span data-ttu-id="0e823-134">Vytvoření Kafka clusterů</span><span class="sxs-lookup"><span data-stu-id="0e823-134">Create Kafka clusters</span></span>

<span data-ttu-id="0e823-135">Když vytvoříte virtuální síť Azure a Kafka clusterů ručně, je jednodušší použít šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0e823-135">While you can create an Azure virtual network and Kafka clusters manually, it's easier to use an Azure Resource Manager template.</span></span> <span data-ttu-id="0e823-136">Použijte následující kroky k nasazení virtuální sítě Azure a dva clustery Kafka k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="0e823-136">Use the following steps to deploy an Azure virtual network and two Kafka clusters to your Azure subscription.</span></span>

1. <span data-ttu-id="0e823-137">Na následující tlačítko použijte pro přihlášení do Azure a otevřete šablonu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0e823-137">Use the following button to sign in to Azure and open the template in the Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-kafka-mirroring/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    <span data-ttu-id="0e823-138">Šablona Azure Resource Manager je umístěna ve **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.</span><span class="sxs-lookup"><span data-stu-id="0e823-138">The Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="0e823-139">Pokud chcete zajistit dostupnost Kafka v HDInsightu, musí cluster obsahovat aspoň tři pracovní uzly.</span><span class="sxs-lookup"><span data-stu-id="0e823-139">To guarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="0e823-140">Tato šablona vytvoří cluster Kafka, který obsahuje tři uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="0e823-140">This template creates a Kafka cluster that contains three worker nodes.</span></span>

2. <span data-ttu-id="0e823-141">Následující informace slouží k naplnění položek na **vlastní nasazení** okno:</span><span class="sxs-lookup"><span data-stu-id="0e823-141">Use the following information to populate the entries on the **Custom deployment** blade:</span></span>
    
    ![HDInsight vlastní nasazení](./media/hdinsight-apache-kafka-mirroring/parameters.png)
    
    * <span data-ttu-id="0e823-143">**Skupina prostředků**: vytvoření skupiny nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="0e823-143">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="0e823-144">Tato skupina obsahuje clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0e823-144">This group contains the HDInsight cluster.</span></span>

    * <span data-ttu-id="0e823-145">**Umístění**: Vyberte umístění geograficky blízko vás.</span><span class="sxs-lookup"><span data-stu-id="0e823-145">**Location**: Select a location geographically close to you.</span></span>
     
    * <span data-ttu-id="0e823-146">**Základní název clusteru**: Tato hodnota se používá jako základní název pro clustery Kafka.</span><span class="sxs-lookup"><span data-stu-id="0e823-146">**Base Cluster Name**: This value is used as the base name for the Kafka clusters.</span></span> <span data-ttu-id="0e823-147">Například zadáním **hdi** vytvoří clustery s názvem **zdroj hdi** a **cíle hdi**.</span><span class="sxs-lookup"><span data-stu-id="0e823-147">For example, entering **hdi** creates clusters named **source-hdi** and **dest-hdi**.</span></span>

    * <span data-ttu-id="0e823-148">**Uživatelské jméno přihlášení clusteru**: uživatelské jméno správce pro zdrojové a cílové Kafka clusterů.</span><span class="sxs-lookup"><span data-stu-id="0e823-148">**Cluster Login User Name**: The admin user name for the source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="0e823-149">**Heslo pro přihlášení clusteru**: uživatelské heslo správce pro zdrojové a cílové Kafka clusterů.</span><span class="sxs-lookup"><span data-stu-id="0e823-149">**Cluster Login Password**: The admin user password for the source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="0e823-150">**Uživatelské jméno SSH**: SSH, aby uživatel vytvořil pro zdrojové a cílové Kafka clustery.</span><span class="sxs-lookup"><span data-stu-id="0e823-150">**SSH User Name**: The SSH user to create for the source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="0e823-151">**Heslo SSH**: heslo pro uživatele SSH pro zdrojové a cílové Kafka clusterů.</span><span class="sxs-lookup"><span data-stu-id="0e823-151">**SSH Password**: The password for the SSH user for the source and destination Kafka clusters.</span></span>

3. <span data-ttu-id="0e823-152">Pro čtení **podmínky a ujednání**a potom vyberte **souhlasím s podmínkami a ujednáními výše uvedených**.</span><span class="sxs-lookup"><span data-stu-id="0e823-152">Read the **Terms and Conditions**, and then select **I agree to the terms and conditions stated above**.</span></span>

4. <span data-ttu-id="0e823-153">Nakonec zkontrolujte **připnout na řídicí panel** a pak vyberte **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="0e823-153">Finally, check **Pin to dashboard** and then select **Purchase**.</span></span> <span data-ttu-id="0e823-154">Chcete-li vytvořit clustery trvá asi 20 minut.</span><span class="sxs-lookup"><span data-stu-id="0e823-154">It takes about 20 minutes to create the clusters.</span></span>

<span data-ttu-id="0e823-155">Po vytvoření prostředky budete přesměrováni do okna pro skupinu prostředků, která obsahuje clustery a webový řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="0e823-155">Once the resources have been created, you are redirected to a blade for the resource group that contains the clusters and web dashboard.</span></span>

![Okno skupiny prostředků pro virtuální síť a clustery](./media/hdinsight-apache-kafka-mirroring/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="0e823-157">Všimněte si, že jsou názvy clusterů HDInsight **zdroj BASENAME** a **cíle BASENAME**, kde BASENAME je jméno, které jste zadali v šabloně.</span><span class="sxs-lookup"><span data-stu-id="0e823-157">Notice that the names of the HDInsight clusters are **source-BASENAME** and **dest-BASENAME**, where BASENAME is the name you provided to the template.</span></span> <span data-ttu-id="0e823-158">Názvy těchto používat v dalších krocích při připojování k clustery.</span><span class="sxs-lookup"><span data-stu-id="0e823-158">You use these names in later steps when connecting to the clusters.</span></span>

## <a name="create-topics"></a><span data-ttu-id="0e823-159">Vytvoření témata</span><span class="sxs-lookup"><span data-stu-id="0e823-159">Create topics</span></span>

1. <span data-ttu-id="0e823-160">Připojení k **zdroj** clusteru pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="0e823-160">Connect to the **source** cluster using SSH:</span></span>

    ```bash
    ssh sshuser@source-BASENAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="0e823-161">Nahraďte **sshuser** s uživatelským jménem SSH použít při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-161">Replace **sshuser** with the SSH user name used when creating the cluster.</span></span> <span data-ttu-id="0e823-162">Nahraďte **BASENAME** s základní název použít při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-162">Replace **BASENAME** with the base name used when creating the cluster.</span></span>

    <span data-ttu-id="0e823-163">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="0e823-163">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="0e823-164">Pokud chcete najít hostitele Zookeeper pro zdrojový cluster, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0e823-164">Use the following commands to find the Zookeeper hosts for the source cluster:</span></span>

    ```bash
    # Install jq if it is not installed
    sudo apt -y install jq
    # get the zookeeper hosts for the source cluster
    export SOURCE_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    
    Replace `$PASSWORD` with the password for the cluster.

    Replace `$CLUSTERNAME` with the name of the source cluster.

3. To create a topic named `testtopic`, use the following command:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SOURCE_ZKHOSTS
    ```

3. <span data-ttu-id="0e823-165">Chcete-li ověřit, zda byl vytvořen v tématu použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0e823-165">Use the following command to verify that the topic was created:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $SOURCE_ZKHOSTS
    ```

    <span data-ttu-id="0e823-166">Odpověď obsahuje `testtopic`.</span><span class="sxs-lookup"><span data-stu-id="0e823-166">The response contains `testtopic`.</span></span>

4. <span data-ttu-id="0e823-167">Použijte následující zobrazíte informace o hostiteli Zookeeper pro tento ( **zdroj**) clusteru:</span><span class="sxs-lookup"><span data-stu-id="0e823-167">Use the following to view the Zookeeper host information for this (the **source**) cluster:</span></span>

    ```bash
    echo $SOURCE_ZKHOSTS
    ```

    <span data-ttu-id="0e823-168">Vrátí informace podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="0e823-168">This returns information similar to the following text:</span></span>

    `zk0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181,zk1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181`

    <span data-ttu-id="0e823-169">Tyto informace uložte.</span><span class="sxs-lookup"><span data-stu-id="0e823-169">Save this information.</span></span> <span data-ttu-id="0e823-170">Používá se v další části.</span><span class="sxs-lookup"><span data-stu-id="0e823-170">It is used in the next section.</span></span>

## <a name="configure-mirroring"></a><span data-ttu-id="0e823-171">Konfigurace zrcadlení</span><span class="sxs-lookup"><span data-stu-id="0e823-171">Configure mirroring</span></span>

1. <span data-ttu-id="0e823-172">Připojení k **cílové** clusteru pomocí jiné relace SSH:</span><span class="sxs-lookup"><span data-stu-id="0e823-172">Connect to the **destination** cluster using a different SSH session:</span></span>

    ```bash
    ssh sshuser@dest-BASENAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="0e823-173">Nahraďte **sshuser** s uživatelským jménem SSH použít při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-173">Replace **sshuser** with the SSH user name used when creating the cluster.</span></span> <span data-ttu-id="0e823-174">Nahraďte **BASENAME** s základní název použít při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-174">Replace **BASENAME** with the base name used when creating the cluster.</span></span>

    <span data-ttu-id="0e823-175">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="0e823-175">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="0e823-176">Použijte následující příkaz k vytvoření `consumer.properties` soubor, který popisuje, jak ke komunikaci s **zdroj** clusteru:</span><span class="sxs-lookup"><span data-stu-id="0e823-176">Use the following command to create a `consumer.properties` file that describes how to communicate with the **source** cluster:</span></span>

    ```bash
    nano consumer.properties
    ```

    <span data-ttu-id="0e823-177">Použít následující text jako obsah `consumer.properties` souboru:</span><span class="sxs-lookup"><span data-stu-id="0e823-177">Use the following text as the contents of the `consumer.properties` file:</span></span>

    ```yaml
    zookeeper.connect=SOURCE_ZKHOSTS
    group.id=mirrorgroup
    ```

    <span data-ttu-id="0e823-178">Nahraďte **SOURCE_ZKHOSTS** informacemi Zookeeper hostitele z **zdroj** clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-178">Replace **SOURCE_ZKHOSTS** with the Zookeeper hosts information from the **source** cluster.</span></span>

    <span data-ttu-id="0e823-179">Tento soubor popisuje příjemce informace používat při čtení ze zdroje Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-179">This file describes the consumer information to use when reading from the source Kafka cluster.</span></span> <span data-ttu-id="0e823-180">Další informace o uživatelských nastavení, naleznete v tématu [příjemce konfigurací](https://kafka.apache.org/documentation#consumerconfigs) v kafka.apache.org.</span><span class="sxs-lookup"><span data-stu-id="0e823-180">For more information consumer configuration, see [Consumer Configs](https://kafka.apache.org/documentation#consumerconfigs) at kafka.apache.org.</span></span>

    <span data-ttu-id="0e823-181">Chcete-li uložit soubor, použijte **kombinaci kláves Ctrl + X**, **Y**a potom **Enter**.</span><span class="sxs-lookup"><span data-stu-id="0e823-181">To save the file, use **Ctrl + X**, **Y**, and then **Enter**.</span></span>

3. <span data-ttu-id="0e823-182">Před konfigurací producent, který komunikuje s cílový cluster, musíte vyhledat zprostředkovatele hostitelů **cílové** clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-182">Before configuring the producer that communicates with the destination cluster, you must find the broker hosts for the **destination** cluster.</span></span> <span data-ttu-id="0e823-183">K načtení těchto informací použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0e823-183">Use the following commands to retrieve this information:</span></span>

    ```bash
    sudo apt -y install jq
    DEST_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $DEST_BROKERHOSTS
    ```

    <span data-ttu-id="0e823-184">Nahraďte `$PASSWORD` s heslo pro účet (správce správce) přihlášení pro cluster.</span><span class="sxs-lookup"><span data-stu-id="0e823-184">Replace `$PASSWORD` with the login account (admin) password for the cluster.</span></span>

    <span data-ttu-id="0e823-185">Nahraďte `$CLUSTERNAME` s názvem cílového clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-185">Replace `$CLUSTERNAME` with the name of the destination cluster.</span></span>

    <span data-ttu-id="0e823-186">Tyto příkazy vrátit informace podobná této:</span><span class="sxs-lookup"><span data-stu-id="0e823-186">These commands return information similar to the following:</span></span>

        wn0-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn1-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092

4. <span data-ttu-id="0e823-187">Následující informace vám pomůžou vytvořit `producer.properties` soubor, který popisuje, jak ke komunikaci s **cílové** clusteru:</span><span class="sxs-lookup"><span data-stu-id="0e823-187">Use the following to create a `producer.properties` file that describes how to communicate with the **destination** cluster:</span></span>

    ```bash
    nano producer.properties
    ```

    <span data-ttu-id="0e823-188">Použít následující text jako obsah `producer.properties` souboru:</span><span class="sxs-lookup"><span data-stu-id="0e823-188">Use the following text as the contents of the `producer.properties` file:</span></span>

    ```yaml
    bootstrap.servers=DEST_BROKERS
    compression.type=none
    ```

    <span data-ttu-id="0e823-189">Nahraďte **DEST_BROKERS** s informacemi o zprostředkovatele z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="0e823-189">Replace **DEST_BROKERS** with the broker information from the previous step.</span></span>

    <span data-ttu-id="0e823-190">Další informace o producent konfigurace, najdete v části [producent konfigurací](https://kafka.apache.org/documentation#producerconfigs) v kafka.apache.org.</span><span class="sxs-lookup"><span data-stu-id="0e823-190">For more information producer configuration, see [Producer Configs](https://kafka.apache.org/documentation#producerconfigs) at kafka.apache.org.</span></span>

## <a name="start-mirrormaker"></a><span data-ttu-id="0e823-191">Spustit MirrorMaker</span><span class="sxs-lookup"><span data-stu-id="0e823-191">Start MirrorMaker</span></span>

1. <span data-ttu-id="0e823-192">Připojení SSH ke **cílové** clusteru, použijte následující příkaz ke spuštění procesu MirrorMaker:</span><span class="sxs-lookup"><span data-stu-id="0e823-192">From the SSH connection to the **destination** cluster, use the following command to start the MirrorMaker process:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    <span data-ttu-id="0e823-193">Parametry použité v tomto příkladu jsou:</span><span class="sxs-lookup"><span data-stu-id="0e823-193">The parameters used in this example are:</span></span>

    * <span data-ttu-id="0e823-194">**--consumer.config**: Určuje soubor, který obsahuje příjemce vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0e823-194">**--consumer.config**: Specifies the file that contains consumer properties.</span></span> <span data-ttu-id="0e823-195">Tyto vlastnosti se používají k vytvoření příjemce, který čte z *zdroj* Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-195">These properties are used to create a consumer that reads from the *source* Kafka cluster.</span></span>

    * <span data-ttu-id="0e823-196">**--producer.config**: Určuje soubor, který obsahuje producent vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0e823-196">**--producer.config**: Specifies the file that contains producer properties.</span></span> <span data-ttu-id="0e823-197">Tyto vlastnosti se používají k vytvoření producent, který zapíše do *cílové* Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-197">These properties are used to create a producer that writes to the *destination* Kafka cluster.</span></span>

    * <span data-ttu-id="0e823-198">**seznam povolených adres –**: seznam témat, která replikuje MirrorMaker ze zdrojového clusteru do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="0e823-198">**--whitelist**: A list of topics that MirrorMaker replicates from the source cluster to the destination.</span></span>

    * <span data-ttu-id="0e823-199">**--num.streams**: počet vláken příjemce k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="0e823-199">**--num.streams**: The number of consumer threads to create.</span></span>

 <span data-ttu-id="0e823-200">Při spuštění MirrorMaker vrátí informace podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="0e823-200">On startup, MirrorMaker returns information similar to the following text:</span></span>

    ```json
    {metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-3, security.protocol=PLAINTEXT}{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-0, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-kafka.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-2, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-1, security.protocol=PLAINTEXT}
    ```

2. <span data-ttu-id="0e823-201">Připojení SSH ke **zdroj** clusteru, použijte následující příkaz ke spuštění producent a odesílání zpráv do tématu:</span><span class="sxs-lookup"><span data-stu-id="0e823-201">From the SSH connection to the **source** cluster, use the following command to start a producer and send messages to the topic:</span></span>

    ```bash
    SOURCE_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

    <span data-ttu-id="0e823-202">Nahraďte `$PASSWORD` s heslo pro přihlášení (správce) pro zdrojový cluster.</span><span class="sxs-lookup"><span data-stu-id="0e823-202">Replace `$PASSWORD` with the login (admin) password for the source cluster.</span></span>

    <span data-ttu-id="0e823-203">Nahraďte `$CLUSTERNAME` s názvem zdrojového clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-203">Replace `$CLUSTERNAME` with the name of the source cluster.</span></span>

     <span data-ttu-id="0e823-204">Až přijedete do prázdný řádek s kurzoru, zadejte několik textové zprávy.</span><span class="sxs-lookup"><span data-stu-id="0e823-204">When you arrive at a blank line with a cursor, type in a few text messages.</span></span> <span data-ttu-id="0e823-205">Tyto jsou odeslány do tématu **zdroj** clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-205">These are sent to the topic on the **source** cluster.</span></span> <span data-ttu-id="0e823-206">Až budete hotoví, použijte **kombinaci kláves Ctrl + C** ukončit proces producent.</span><span class="sxs-lookup"><span data-stu-id="0e823-206">When done, use **Ctrl + C** to end the producer process.</span></span>

3. <span data-ttu-id="0e823-207">Připojení SSH ke **cílové** clusteru, použijte **kombinaci kláves Ctrl + C** ukončit proces MirrorMaker.</span><span class="sxs-lookup"><span data-stu-id="0e823-207">From the SSH connection to the **destination** cluster, use **Ctrl + C** to end the MirrorMaker process.</span></span> <span data-ttu-id="0e823-208">Pak pomocí následujících příkazů ověřte, zda `testtopic` tématu byl vytvořen a tato data v tomto tématu se replikují do této zrcadlení:</span><span class="sxs-lookup"><span data-stu-id="0e823-208">Then use the following commands to verify that the `testtopic` topic was created, and that data in the topic was replicated to this mirror:</span></span>

    ```bash
    DEST_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $DEST_ZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $DEST_ZKHOSTS --topic testtopic --from-beginning
    ```

    <span data-ttu-id="0e823-209">Nahraďte `$PASSWORD` s pro cílový cluster heslo pro přihlášení (správce).</span><span class="sxs-lookup"><span data-stu-id="0e823-209">Replace `$PASSWORD` with the login (admin) password for the destination cluster.</span></span>

    <span data-ttu-id="0e823-210">Nahraďte `$CLUSTERNAME` s názvem cílového clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-210">Replace `$CLUSTERNAME` with the name of the destination cluster.</span></span>

    <span data-ttu-id="0e823-211">Seznam témat nyní zahrnuje `testtopic`, který se vytvoří při MirrorMaster zrcadlí tématu ze zdrojového clusteru do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="0e823-211">The list of topics now includes `testtopic`, which is created when MirrorMaster mirrors the topic from the source cluster to the destination.</span></span> <span data-ttu-id="0e823-212">Zprávy přijaté z tématu jsou stejné, jako je zadaný ve zdrojovém clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-212">The messages retrieved from the topic are the same as entered on the source cluster.</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="0e823-213">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="0e823-213">Delete the cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="0e823-214">Vzhledem k tomu, že kroky v tomto dokumentu vytvořit oba clustery ve stejné skupině prostředků Azure, můžete odstranit skupinu prostředků na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0e823-214">Since the steps in this document create both clusters in the same Azure resource group, you can delete the resource group in the Azure portal.</span></span> <span data-ttu-id="0e823-215">Odstraňuje se skupina prostředků odebere všechny prostředky, které jsou vytvořené pomocí následujících tento dokument, Azure Virtual Network a účet úložiště, které jsou používané clustery.</span><span class="sxs-lookup"><span data-stu-id="0e823-215">Deleting the resource group removes all resources created by following this document, the Azure Virtual Network, and storage account used by the clusters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0e823-216">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0e823-216">Next Steps</span></span>

<span data-ttu-id="0e823-217">V tomto dokumentu jste zjistili, jak používat MirrorMaker k vytvoření repliky Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e823-217">In this document, you learned how to use MirrorMaker to create a replica of a Kafka cluster.</span></span> <span data-ttu-id="0e823-218">Chcete-li zjistit další způsoby, jak pracovat s Kafka pomocí následujících odkazů:</span><span class="sxs-lookup"><span data-stu-id="0e823-218">Use the following links to discover other ways to work with Kafka:</span></span>

* <span data-ttu-id="0e823-219">[Dokumentaci Apache Kafka MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) v cwiki.apache.org.</span><span class="sxs-lookup"><span data-stu-id="0e823-219">[Apache Kafka MirrorMaker documentation](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) at cwiki.apache.org.</span></span>
* [<span data-ttu-id="0e823-220">Začínáme s Apache Kafka v HDInsight</span><span class="sxs-lookup"><span data-stu-id="0e823-220">Get started with Apache Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="0e823-221">Použití Apache Sparku se systémem Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="0e823-221">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)
* [<span data-ttu-id="0e823-222">Použití Apache Stormu se systémem Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="0e823-222">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)
* [<span data-ttu-id="0e823-223">Připojení k systému Kafka přes virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="0e823-223">Connect to Kafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)
