---
title: "Použití Apache Phoenix a SQuirreL s HBase - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak používat Apache Phoenix v HDInsight a jak nainstalovat a nakonfigurovat SQuirreL na pracovní stanici pro připojení k cluster HBase v HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: cda0f33b-a2e8-494c-972f-ae0bb482b818
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/26/2017
ms.author: jgao
ms.openlocfilehash: 13d17083bbe26fa9745ce4c5fef9f56859243c2e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a><span data-ttu-id="e7145-103">Použití nástrojů Apache Phoenix s clustery se systémem Linux HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e7145-103">Use Apache Phoenix with Linux-based HBase clusters in HDInsight</span></span>
<span data-ttu-id="e7145-104">Další informace o použití [Apache Phoenix](http://phoenix.apache.org/) v HDInsight a jak používat SQLLine.</span><span class="sxs-lookup"><span data-stu-id="e7145-104">Learn how to use [Apache Phoenix](http://phoenix.apache.org/) in HDInsight, and how to use SQLLine.</span></span> <span data-ttu-id="e7145-105">Další informace o Phoenix najdete v tématu [Phoenix za 15 minut nebo méně](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span><span class="sxs-lookup"><span data-stu-id="e7145-105">For more information about Phoenix, see [Phoenix in 15 minutes or less](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span></span> <span data-ttu-id="e7145-106">Gramatika Phoenix, najdete v části [Phoenix gramatika](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="e7145-106">For the Phoenix grammar, see [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

> [!NOTE]
> <span data-ttu-id="e7145-107">Informace o verzi Phoenix v HDInsight, naleznete v části [co je nového ve verzích clusterů systému Hadoop poskytovaných prostředím HDInsight?](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="e7145-107">For the Phoenix version information in HDInsight, see [What's new in the Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md).</span></span>
>
>

## <a name="use-sqlline"></a><span data-ttu-id="e7145-108">Použití SQLLine</span><span class="sxs-lookup"><span data-stu-id="e7145-108">Use SQLLine</span></span>
<span data-ttu-id="e7145-109">[SQLLine](http://sqlline.sourceforge.net/) je nástroj příkazového řádku k provedení SQL.</span><span class="sxs-lookup"><span data-stu-id="e7145-109">[SQLLine](http://sqlline.sourceforge.net/) is a command-line utility to execute SQL.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="e7145-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e7145-110">Prerequisites</span></span>
<span data-ttu-id="e7145-111">Než budete moci použít SQLLine, musíte mít následující:</span><span class="sxs-lookup"><span data-stu-id="e7145-111">Before you can use SQLLine, you must have the following:</span></span>

* <span data-ttu-id="e7145-112">**Cluster HBase v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="e7145-112">**An HBase cluster in HDInsight**.</span></span> <span data-ttu-id="e7145-113">Informace o zřizování HBase clusteru najdete v tématu [začít pracovat s Apache HBase v HDInsight][hdinsight-hbase-get-started].</span><span class="sxs-lookup"><span data-stu-id="e7145-113">For information on provision HBase cluster, see [Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started].</span></span>
* <span data-ttu-id="e7145-114">**Připojte se ke clusteru HBase pomocí protokolu vzdálené plochy**.</span><span class="sxs-lookup"><span data-stu-id="e7145-114">**Connect to the HBase cluster via the remote desktop protocol**.</span></span> <span data-ttu-id="e7145-115">Pokyny najdete v tématu [Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure][hdinsight-manage-portal].</span><span class="sxs-lookup"><span data-stu-id="e7145-115">For instructions, see [Manage Hadoop clusters in HDInsight by using the Azure portal][hdinsight-manage-portal].</span></span>

<span data-ttu-id="e7145-116">Jakmile se připojíte ke clusteru HBase, budete muset připojit k Zookeepers.</span><span class="sxs-lookup"><span data-stu-id="e7145-116">When you connect to an HBase cluster, you need to connect to one of the Zookeepers.</span></span> <span data-ttu-id="e7145-117">Každý cluster HDInsight má tři Zookeepers.</span><span class="sxs-lookup"><span data-stu-id="e7145-117">Each HDInsight cluster has three Zookeepers.</span></span>

<span data-ttu-id="e7145-118">**Chcete-li zjistit název hostitele Zookeeper**</span><span class="sxs-lookup"><span data-stu-id="e7145-118">**To find out the Zookeeper host name**</span></span>

1. <span data-ttu-id="e7145-119">Otevření Ambari procházením **https://<ClusterName>. azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="e7145-119">Open Ambari by browsing to **https://<ClusterName>.azurehdinsight.net**.</span></span>
2. <span data-ttu-id="e7145-120">Zadejte uživatelské jméno protokolu HTTP (cluster) a heslo k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="e7145-120">Enter the HTTP (cluster) username and password to login.</span></span>
3. <span data-ttu-id="e7145-121">Klikněte na tlačítko **ZooKeeper** v levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="e7145-121">Click **ZooKeeper** from the left menu.</span></span> <span data-ttu-id="e7145-122">Zobrazí tři **ZooKeeper Server** uvedené.</span><span class="sxs-lookup"><span data-stu-id="e7145-122">You see three **ZooKeeper Server** listed.</span></span>
4. <span data-ttu-id="e7145-123">Klikněte na jednu z **ZooKeeper Server** uvedené.</span><span class="sxs-lookup"><span data-stu-id="e7145-123">Click one of the **ZooKeeper Server** listed.</span></span> <span data-ttu-id="e7145-124">V podokně Souhrn najít **Hostname**.</span><span class="sxs-lookup"><span data-stu-id="e7145-124">On the Summary pane, find the **Hostname**.</span></span> <span data-ttu-id="e7145-125">Je podobná *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="e7145-125">It is similar to *zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.</span></span>

<span data-ttu-id="e7145-126">**Chcete-li použít SQLLine**</span><span class="sxs-lookup"><span data-stu-id="e7145-126">**To use SQLLine**</span></span>

1. <span data-ttu-id="e7145-127">Připojte se ke clusteru pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="e7145-127">Connect to the cluster using SSH.</span></span> <span data-ttu-id="e7145-128">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="e7145-128">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="e7145-129">Ze SSH spusťte následující příkazy ke spuštění SQLLine:</span><span class="sxs-lookup"><span data-stu-id="e7145-129">From SSH, run the following commands to run SQLLine:</span></span>

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure
3. <span data-ttu-id="e7145-130">Spusťte následující příkazy k vytvoření tabulky HBase a vložte některá data:</span><span class="sxs-lookup"><span data-stu-id="e7145-130">Run the following commands to create a HBase table, and insert some data:</span></span>

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

<span data-ttu-id="e7145-131">Další informace najdete v tématu [SQLLine ruční](http://sqlline.sourceforge.net/#manual) a [Phoenix gramatika](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="e7145-131">For more information, see [SQLLine manual](http://sqlline.sourceforge.net/#manual) and [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7145-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e7145-132">Next steps</span></span>
<span data-ttu-id="e7145-133">V tomto článku jste se naučili, jak používat Apache Phoenix v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e7145-133">In this article, you have learned how to use Apache Phoenix in HDInsight.</span></span>  <span data-ttu-id="e7145-134">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="e7145-134">To learn more, see:</span></span>

* <span data-ttu-id="e7145-135">[Přehled HBase ve službě HDInsight][hdinsight-hbase-overview]: HBase je NoSQL open source databáze Apache postavená na systému Hadoop, která poskytuje náhodný přístup a silnou konzistenci pro velké objemy nestrukturovaných a částečně strukturovaných dat.</span><span class="sxs-lookup"><span data-stu-id="e7145-135">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>
* <span data-ttu-id="e7145-136">[Zřídit clustery HBase v Azure Virtual Network][hdinsight-hbase-provision-vnet]: integrace virtuální sítě, clustery HBase můžete nasadit do stejné virtuální síti jako aplikace tak, aby aplikace může komunikovat přímo s HBase.</span><span class="sxs-lookup"><span data-stu-id="e7145-136">[Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]: With virtual network integration, HBase clusters can be deployed to the same virtual network as your applications so that applications can communicate with HBase directly.</span></span>
* <span data-ttu-id="e7145-137">[Konfigurace replikace HBase v HDInsight](hdinsight-hbase-replication.md): Naučte se konfigurovat replikace HBase v rámci dvou datových centrech Azure.</span><span class="sxs-lookup"><span data-stu-id="e7145-137">[Configure HBase replication in HDInsight](hdinsight-hbase-replication.md): Learn how to configure HBase replication across two Azure datacenters.</span></span>
* <span data-ttu-id="e7145-138">[Analýza sentimentu Twitter s HBase v HDInsight][hbase-twitter-sentiment]: Další informace o postupu v reálném čase [postojích analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) velkých objemů dat pomocí HBase v clusteru Hadoop v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e7145-138">[Analyze Twitter sentiment with HBase in HDInsight][hbase-twitter-sentiment]: Learn how to do real-time [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) of big data by using HBase in a Hadoop cluster in HDInsight.</span></span>

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png
