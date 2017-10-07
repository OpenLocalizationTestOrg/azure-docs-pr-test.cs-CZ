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
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Použití nástrojů Apache Phoenix s clustery se systémem Linux HBase v HDInsight
Zjistěte, jak toouse [Apache Phoenix](http://phoenix.apache.org/) v HDInsight a jak toouse SQLLine. Další informace o Phoenix najdete v tématu [Phoenix za 15 minut nebo méně](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Hello gramatika Phoenix, najdete v části [Phoenix gramatika](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Hello Phoenix informace o verzi v HDInsight, naleznete v části [co je nového ve verzích clusterů systému Hadoop hello poskytovaných v HDInsight?](hdinsight-component-versioning.md).
>
>

## <a name="use-sqlline"></a>Použití SQLLine
[SQLLine](http://sqlline.sourceforge.net/) je nástroj příkazového řádku tooexecute SQL.

### <a name="prerequisites"></a>Požadavky
Než budete moci použít SQLLine, musíte mít následující hello:

* **Cluster HBase v HDInsight**. Informace o zřizování HBase clusteru najdete v tématu [začít pracovat s Apache HBase v HDInsight][hdinsight-hbase-get-started].
* **Připojit cluster HBase toohello prostřednictvím protokolu vzdálené plochy hello**. Pokyny najdete v tématu [hello spravovat Hadoop clusterů v HDInsight pomocí portálu Azure][hdinsight-manage-portal].

Pokud připojíte tooan HBase cluster, je potřeba tooconnect tooone Dobrý den Zookeepers. Každý cluster HDInsight má tři Zookeepers.

**toofind se název hostitele Zookeeper hello**

1. Otevření Ambari procházením příliš**https://<ClusterName>. azurehdinsight.net**.
2. Zadejte uživatelské jméno a heslo toologin HTTP (cluster) hello.
3. Klikněte na tlačítko **ZooKeeper** hello levé nabídce. Zobrazí tři **ZooKeeper Server** uvedené.
4. Klikněte na jednu z hello **ZooKeeper Server** uvedené. V podokně Souhrn hello najde hello **Hostname**. Je to podobné příliš*zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**toouse SQLLine**

1. Připojte toohello clusteru pomocí protokolu SSH. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Ze SSH spusťte následující příkazy toorun SQLLine hello:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure
3. Spusťte následující příkazy toocreate hello tabulky HBase a vložte některá data:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

Další informace najdete v tématu [SQLLine ruční](http://sqlline.sourceforge.net/#manual) a [Phoenix gramatika](http://phoenix.apache.org/language/index.html).

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili jak toouse Apache Phoenix v HDInsight.  toolearn více, najdete v části:

* [Přehled HBase ve službě HDInsight][hdinsight-hbase-overview]: HBase je NoSQL open source databáze Apache postavená na systému Hadoop, která poskytuje náhodný přístup a silnou konzistenci pro velké objemy nestrukturovaných a částečně strukturovaných dat.
* [Zřídit clustery HBase v Azure Virtual Network][hdinsight-hbase-provision-vnet]: clustery HBase integrace virtuální sítě, může být nasazený toohello stejné virtuální sítě jako aplikace tak, že aplikace může komunikovat s HBase přímo.
* [Konfigurace replikace HBase v HDInsight](hdinsight-hbase-replication.md): Zjistěte, jak tooconfigure HBase replikace mezi dvěma datových centrech Azure.
* [Analýza sentimentu Twitter s HBase v HDInsight][hbase-twitter-sentiment]: Zjistěte, jak v reálném čase toodo [postojích analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) velkých objemů dat pomocí HBase v clusteru Hadoop v HDInsight.

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
