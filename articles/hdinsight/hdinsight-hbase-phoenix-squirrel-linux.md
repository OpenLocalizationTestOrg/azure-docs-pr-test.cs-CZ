---
title: aaaUse Apache Phoenix a SQuirreL s HBase - Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak toouse Apache Phoenix v HDInsight a jak tooinstall a konfigurace SQuirreL na clusteru pracovní stanice tooconnect tooan HBase v HDInsight."
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
ms.openlocfilehash: a63e8c8212b7a992453ec94fa638ec3863a0ede3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a><span data-ttu-id="4f836-103">Použití nástrojů Apache Phoenix s clustery se systémem Linux HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="4f836-103">Use Apache Phoenix with Linux-based HBase clusters in HDInsight</span></span>
<span data-ttu-id="4f836-104">Zjistěte, jak toouse [Apache Phoenix](http://phoenix.apache.org/) v HDInsight a jak toouse SQLLine.</span><span class="sxs-lookup"><span data-stu-id="4f836-104">Learn how toouse [Apache Phoenix](http://phoenix.apache.org/) in HDInsight, and how toouse SQLLine.</span></span> <span data-ttu-id="4f836-105">Další informace o Phoenix najdete v tématu [Phoenix za 15 minut nebo méně](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span><span class="sxs-lookup"><span data-stu-id="4f836-105">For more information about Phoenix, see [Phoenix in 15 minutes or less](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span></span> <span data-ttu-id="4f836-106">Hello gramatika Phoenix, najdete v části [Phoenix gramatika](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="4f836-106">For hello Phoenix grammar, see [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

> [!NOTE]
> <span data-ttu-id="4f836-107">Hello Phoenix informace o verzi v HDInsight, naleznete v části [co je nového ve verzích clusterů systému Hadoop hello poskytovaných v HDInsight?](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="4f836-107">For hello Phoenix version information in HDInsight, see [What's new in hello Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md).</span></span>
>
>

## <a name="use-sqlline"></a><span data-ttu-id="4f836-108">Použití SQLLine</span><span class="sxs-lookup"><span data-stu-id="4f836-108">Use SQLLine</span></span>
<span data-ttu-id="4f836-109">[SQLLine](http://sqlline.sourceforge.net/) je nástroj příkazového řádku tooexecute SQL.</span><span class="sxs-lookup"><span data-stu-id="4f836-109">[SQLLine](http://sqlline.sourceforge.net/) is a command-line utility tooexecute SQL.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="4f836-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4f836-110">Prerequisites</span></span>
<span data-ttu-id="4f836-111">Než budete moci použít SQLLine, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="4f836-111">Before you can use SQLLine, you must have hello following:</span></span>

* <span data-ttu-id="4f836-112">**Cluster HBase v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="4f836-112">**An HBase cluster in HDInsight**.</span></span> <span data-ttu-id="4f836-113">Informace o zřizování HBase clusteru najdete v tématu [začít pracovat s Apache HBase v HDInsight][hdinsight-hbase-get-started].</span><span class="sxs-lookup"><span data-stu-id="4f836-113">For information on provision HBase cluster, see [Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started].</span></span>
* <span data-ttu-id="4f836-114">**Připojit cluster HBase toohello prostřednictvím protokolu vzdálené plochy hello**.</span><span class="sxs-lookup"><span data-stu-id="4f836-114">**Connect toohello HBase cluster via hello remote desktop protocol**.</span></span> <span data-ttu-id="4f836-115">Pokyny najdete v tématu [hello spravovat Hadoop clusterů v HDInsight pomocí portálu Azure][hdinsight-manage-portal].</span><span class="sxs-lookup"><span data-stu-id="4f836-115">For instructions, see [Manage Hadoop clusters in HDInsight by using hello Azure portal][hdinsight-manage-portal].</span></span>

<span data-ttu-id="4f836-116">Pokud připojíte tooan HBase cluster, je potřeba tooconnect tooone Dobrý den Zookeepers.</span><span class="sxs-lookup"><span data-stu-id="4f836-116">When you connect tooan HBase cluster, you need tooconnect tooone of hello Zookeepers.</span></span> <span data-ttu-id="4f836-117">Každý cluster HDInsight má tři Zookeepers.</span><span class="sxs-lookup"><span data-stu-id="4f836-117">Each HDInsight cluster has three Zookeepers.</span></span>

<span data-ttu-id="4f836-118">**toofind se název hostitele Zookeeper hello**</span><span class="sxs-lookup"><span data-stu-id="4f836-118">**toofind out hello Zookeeper host name**</span></span>

1. <span data-ttu-id="4f836-119">Otevření Ambari procházením příliš**https://<ClusterName>. azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="4f836-119">Open Ambari by browsing too**https://<ClusterName>.azurehdinsight.net**.</span></span>
2. <span data-ttu-id="4f836-120">Zadejte uživatelské jméno a heslo toologin HTTP (cluster) hello.</span><span class="sxs-lookup"><span data-stu-id="4f836-120">Enter hello HTTP (cluster) username and password toologin.</span></span>
3. <span data-ttu-id="4f836-121">Klikněte na tlačítko **ZooKeeper** hello levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="4f836-121">Click **ZooKeeper** from hello left menu.</span></span> <span data-ttu-id="4f836-122">Zobrazí tři **ZooKeeper Server** uvedené.</span><span class="sxs-lookup"><span data-stu-id="4f836-122">You see three **ZooKeeper Server** listed.</span></span>
4. <span data-ttu-id="4f836-123">Klikněte na jednu z hello **ZooKeeper Server** uvedené.</span><span class="sxs-lookup"><span data-stu-id="4f836-123">Click one of hello **ZooKeeper Server** listed.</span></span> <span data-ttu-id="4f836-124">V podokně Souhrn hello najde hello **Hostname**.</span><span class="sxs-lookup"><span data-stu-id="4f836-124">On hello Summary pane, find hello **Hostname**.</span></span> <span data-ttu-id="4f836-125">Je to podobné příliš*zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="4f836-125">It is similar too*zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.</span></span>

<span data-ttu-id="4f836-126">**toouse SQLLine**</span><span class="sxs-lookup"><span data-stu-id="4f836-126">**toouse SQLLine**</span></span>

1. <span data-ttu-id="4f836-127">Připojte toohello clusteru pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="4f836-127">Connect toohello cluster using SSH.</span></span> <span data-ttu-id="4f836-128">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="4f836-128">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="4f836-129">Ze SSH spusťte následující příkazy toorun SQLLine hello:</span><span class="sxs-lookup"><span data-stu-id="4f836-129">From SSH, run hello following commands toorun SQLLine:</span></span>

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure
3. <span data-ttu-id="4f836-130">Spusťte následující příkazy toocreate hello tabulky HBase a vložte některá data:</span><span class="sxs-lookup"><span data-stu-id="4f836-130">Run hello following commands toocreate a HBase table, and insert some data:</span></span>

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

<span data-ttu-id="4f836-131">Další informace najdete v tématu [SQLLine ruční](http://sqlline.sourceforge.net/#manual) a [Phoenix gramatika](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="4f836-131">For more information, see [SQLLine manual](http://sqlline.sourceforge.net/#manual) and [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4f836-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4f836-132">Next steps</span></span>
<span data-ttu-id="4f836-133">V tomto článku jste se naučili jak toouse Apache Phoenix v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4f836-133">In this article, you have learned how toouse Apache Phoenix in HDInsight.</span></span>  <span data-ttu-id="4f836-134">toolearn více, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="4f836-134">toolearn more, see:</span></span>

* <span data-ttu-id="4f836-135">[Přehled HBase ve službě HDInsight][hdinsight-hbase-overview]: HBase je NoSQL open source databáze Apache postavená na systému Hadoop, která poskytuje náhodný přístup a silnou konzistenci pro velké objemy nestrukturovaných a částečně strukturovaných dat.</span><span class="sxs-lookup"><span data-stu-id="4f836-135">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>
* <span data-ttu-id="4f836-136">[Zřídit clustery HBase v Azure Virtual Network][hdinsight-hbase-provision-vnet]: clustery HBase integrace virtuální sítě, může být nasazený toohello stejné virtuální sítě jako aplikace tak, že aplikace může komunikovat s HBase přímo.</span><span class="sxs-lookup"><span data-stu-id="4f836-136">[Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]: With virtual network integration, HBase clusters can be deployed toohello same virtual network as your applications so that applications can communicate with HBase directly.</span></span>
* <span data-ttu-id="4f836-137">[Konfigurace replikace HBase v HDInsight](hdinsight-hbase-replication.md): Zjistěte, jak tooconfigure HBase replikace mezi dvěma datových centrech Azure.</span><span class="sxs-lookup"><span data-stu-id="4f836-137">[Configure HBase replication in HDInsight](hdinsight-hbase-replication.md): Learn how tooconfigure HBase replication across two Azure datacenters.</span></span>
* <span data-ttu-id="4f836-138">[Analýza sentimentu Twitter s HBase v HDInsight][hbase-twitter-sentiment]: Zjistěte, jak v reálném čase toodo [postojích analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) velkých objemů dat pomocí HBase v clusteru Hadoop v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4f836-138">[Analyze Twitter sentiment with HBase in HDInsight][hbase-twitter-sentiment]: Learn how toodo real-time [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) of big data by using HBase in a Hadoop cluster in HDInsight.</span></span>

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
