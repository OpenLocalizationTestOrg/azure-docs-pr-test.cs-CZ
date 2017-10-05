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
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Použití nástrojů Apache Phoenix s clustery se systémem Linux HBase v HDInsight
Další informace o použití [Apache Phoenix](http://phoenix.apache.org/) v HDInsight a jak používat SQLLine. Další informace o Phoenix najdete v tématu [Phoenix za 15 minut nebo méně](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Gramatika Phoenix, najdete v části [Phoenix gramatika](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Informace o verzi Phoenix v HDInsight, naleznete v části [co je nového ve verzích clusterů systému Hadoop poskytovaných prostředím HDInsight?](hdinsight-component-versioning.md).
>
>

## <a name="use-sqlline"></a>Použití SQLLine
[SQLLine](http://sqlline.sourceforge.net/) je nástroj příkazového řádku k provedení SQL.

### <a name="prerequisites"></a>Požadavky
Než budete moci použít SQLLine, musíte mít následující:

* **Cluster HBase v HDInsight**. Informace o zřizování HBase clusteru najdete v tématu [začít pracovat s Apache HBase v HDInsight][hdinsight-hbase-get-started].
* **Připojte se ke clusteru HBase pomocí protokolu vzdálené plochy**. Pokyny najdete v tématu [Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure][hdinsight-manage-portal].

Jakmile se připojíte ke clusteru HBase, budete muset připojit k Zookeepers. Každý cluster HDInsight má tři Zookeepers.

**Chcete-li zjistit název hostitele Zookeeper**

1. Otevření Ambari procházením **https://<ClusterName>. azurehdinsight.net**.
2. Zadejte uživatelské jméno protokolu HTTP (cluster) a heslo k přihlášení.
3. Klikněte na tlačítko **ZooKeeper** v levé nabídce. Zobrazí tři **ZooKeeper Server** uvedené.
4. Klikněte na jednu z **ZooKeeper Server** uvedené. V podokně Souhrn najít **Hostname**. Je podobná *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**Chcete-li použít SQLLine**

1. Připojte se ke clusteru pomocí protokolu SSH. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Ze SSH spusťte následující příkazy ke spuštění SQLLine:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure
3. Spusťte následující příkazy k vytvoření tabulky HBase a vložte některá data:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

Další informace najdete v tématu [SQLLine ruční](http://sqlline.sourceforge.net/#manual) a [Phoenix gramatika](http://phoenix.apache.org/language/index.html).

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili, jak používat Apache Phoenix v HDInsight.  Další informace naleznete v tématu:

* [Přehled HBase ve službě HDInsight][hdinsight-hbase-overview]: HBase je NoSQL open source databáze Apache postavená na systému Hadoop, která poskytuje náhodný přístup a silnou konzistenci pro velké objemy nestrukturovaných a částečně strukturovaných dat.
* [Zřídit clustery HBase v Azure Virtual Network][hdinsight-hbase-provision-vnet]: integrace virtuální sítě, clustery HBase můžete nasadit do stejné virtuální síti jako aplikace tak, aby aplikace může komunikovat přímo s HBase.
* [Konfigurace replikace HBase v HDInsight](hdinsight-hbase-replication.md): Naučte se konfigurovat replikace HBase v rámci dvou datových centrech Azure.
* [Analýza sentimentu Twitter s HBase v HDInsight][hbase-twitter-sentiment]: Další informace o postupu v reálném čase [postojích analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) velkých objemů dat pomocí HBase v clusteru Hadoop v HDInsight.

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
