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
# <a name="configure-hbase-cluster-replication-within-virtual-networks"></a>Konfigurace replikace HBase clusteru v rámci virtuální sítě

Zjistěte, jak tooconfigure HBase replikace v rámci jednu virtuální síť (VNet) nebo mezi dvě virtuální sítě.

Clusterová replikace používá metodika nabízené zdroje. HBase cluster může být zdroj nebo cíl, nebo ji můžete najednou splnění obě role. Replikace je asynchronní a hello cílem replikace je konzistence typu případné. Když zdroj hello obdrží rodiny upravit tooa sloupec s povolenou replikací, že úpravy je šířený tooall cílových clusterech. Když data se replikují z jednoho clusteru tooanother, hello zdrojovém clusteru a všechny clustery, které jste již využívat hello data jsou sledovaných tooprevent replikační cykly.

V tomto kurzu nakonfigurujete zdroj cíl replikace. Jiných topologiích clusteru najdete v části [referenční příručka Apache HBase](http://hbase.apache.org/book.html#_cluster_replication).

Případy využití HBase replikace pro jedné virtuální sítě:

* Vyrovnávání zatížení – například spuštění kontroly nebo úloh MapReduce na hello cílový cluster a příjem dat na zdrojovém clusteru hello
* Vysoká dostupnost
* Migrace dat z jednoho clusteru tooanother HBase
* Upgrade clusteru Azure HDInsight z jedné verze tooanother

Případy využití HBase replikace pro dvě virtuální sítě:

* Zotavení po havárii
* Vyrovnávání zatížení a rozdělení do oddílů aplikace hello
* Vysoká dostupnost

Při replikaci clustery pomocí [skript akce](hdinsight-hadoop-customize-cluster-linux.md) skripty nacházející se v [Githubu](https://github.com/Azure/hbase-utils/tree/master/replication).

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="configure-hello-environments"></a>Konfigurace prostředí hello

Existují tři možné konfigurace:

- Dva clustery HBase v jednu virtuální síť Azure
- Dva clustery HBase ve dvou různých virtuálních sítích v hello stejné oblasti
- Dva clustery HBase ve dvou různých virtuálních sítích v různých oblastech dva (geografická replikace)

toomake je snazší tooconfigure hello prostředí, vytvořili jsme některé [šablon Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md). Pokud dáváte přednost tooconfigure hello prostředí pomocí jiné metody, najdete v části:

- [Vytvořit clustery se systémem Linux Hadoop v HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
- [Vytvořit clustery HBase v Azure Virtual Network](hdinsight-hbase-provision-vnet.md)

### <a name="configure-one-virtual-network"></a>Nakonfigurujte jednu virtuální síť

Klikněte na následující obrázek toocreate dvěma HBase clustery v hello hello stejné virtuální síti. Šablona Hello je uložena v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-one-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>

### <a name="configure-two-virtual-networks-in-hello-same-region"></a>Nakonfigurovat dvě virtuální sítě v hello stejné oblasti

Klikněte na následující obrázek toocreate dvě virtuální sítě pomocí virtuální sítě partnerský vztah a dva clustery HBase v hello hello stejné oblasti. Šablona Hello je uložena v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-two-vnets-same-region%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>



Tento scénář vyžaduje [VNet peering](../virtual-network/virtual-network-peering-overview.md). Šablona Hello umožňuje partnerský vztah virtuální sítě.   

Replikace HBase používá IP adresy hello ZooKeeper virtuálních počítačů. Je nutné nakonfigurovat statické IP adresy pro uzly HBase ZooKeeper cílové hello.

**tooconfigure statické IP adresy**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V levé nabídce hello, klikněte na **skupiny prostředků**.
3. Klepněte na skupinu prostředků obsahující cluster HBase cílové hello. Toto je hello skupinu prostředků, který jste zadali při použití hello Resource Manager šablony toocreate hello prostředí. Můžete použít filtr toonarrow hello dolů hello seznamu. Zobrazí seznam prostředků, který obsahuje hello dvě virtuální sítě.
4. Klikněte na tlačítko hello virtuální síť, která obsahuje clusteru HBase cílové hello. Klikněte například na **xxxx-vnet2**. Zobrazí se tří zařízení s názvy, které začínají **seskupování-zookeepermode -**. Tato zařízení jsou ZooKeeper hello tři virtuální počítače.
5. Klikněte na jednu z hello ZooKeeper virtuálních počítačů.
6. Klikněte na tlačítko **konfigurace protokolu IP**.
7. Klikněte na tlačítko **ipConfig1** hello seznamu.
8. Klikněte na tlačítko **statické**a zapište si IP adresu skutečného hello. Když spustíte hello skript akce tooenable replikace, budete potřebovat hello IP adresu.

  ![HDInsight HBase replikace ZooKeeper statickou IP adresu](./media/hdinsight-hbase-replication/hdinsight-hbase-replication-zookeeper-static-ip.png)

9. Zopakujte krok 6 tooset hello statickou IP adresu pro hello další dva uzly ZooKeeper.

Pro scénář hello cross-VNet, je nutné použít hello **- ip** přepínače při volání metody hello **hdi_enable_replication.sh** skript akce.

### <a name="configure-two-virtual-networks-in-two-different-regions"></a>Nakonfigurovat dvě virtuální sítě ve dvou různých oblastech

Klikněte na tlačítko hello následující bitové kopie toocreate dvě virtuální sítě ve dvou různých oblastech. Šablona Hello je uložena ve veřejném kontejneru objektů Blob v Azure.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fdeploy-hbase-geo-replication.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>

Vytvoření brány VPN mezi dvěma virtuálními sítěmi hello. Pokyny najdete v tématu [vytvoření virtuální sítě s připojením site-to-site](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

Replikace HBase používá IP adresy hello ZooKeeper virtuálních počítačů. Je nutné nakonfigurovat statické IP adresy pro uzly HBase ZooKeeper cílové hello. tooconfigure statické IP adresy, najdete v části "hello konfigurace dvou virtuálních sítí ve stejné oblasti" hello v tomto článku.

Pro scénář hello cross-VNet, je nutné použít hello **- ip** přepínače při volání metody hello **hdi_enable_replication.sh** skript akce.

## <a name="load-test-data"></a>Testovací data zatížení

Při replikaci cluster, je nutné zadat tooreplicate tabulky hello. Některá data v této části se načte do clusteru s podporou hello. V další části hello povolíte replikaci mezi dvěma clustery s hello.

Postupujte podle pokynů hello v [kurz HBase: začněte používat Apache HBase se systémem Linux Hadoop v HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) toocreate **kontakty** tabulku a vložte některá data do tabulky hello.

## <a name="enable-replication"></a>Povolení replikace

Hello následující kroky ukazují, jak toocall hello skript akce skriptu z hello portálu Azure. Spuštění skriptu akci pomocí prostředí Azure PowerShell a hello rozhraní příkazového řádku Azure (CLI), najdete v části [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).

**replikace HBase tooenable z hello portálu Azure**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Otevřete cluster HBase hello zdroje.
3. Z nabídky hello clusteru, klikněte na tlačítko **akcí skriptů**.
4. Klikněte na tlačítko **odeslání nové** z hello horní části okna hello.
5. Vyberte nebo zadejte hello následující informace:

  - **Název**: Zadejte **povolit replikaci**.
  - **Adresa URL skriptu bash**: Zadejte **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.
  - **HEAD**: vybraná. Zrušte hello jiné typy uzlů.
  - **Parametry**: hello následující ukázkové parametry povolení replikace pro všechny existující tabulky hello a zkopírovat všechna data hello z hello zdrojového clusteru toohello cílového clusteru:

            -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata

6. Klikněte na možnost **Vytvořit**. skript Hello může trvat nějakou dobu, hlavně když se použije argument - programu copydata hello.

Vyžaduje argumenty:

|Name (Název)|Popis|
|----|-----------|
|-s, – src-cluster | Zadejte název DNS hello cluster HBase hello zdroje. Příklad: hbsrccluster -s, cluster – src = hbsrccluster |
|-d, – dst clusteru | Zadejte název DNS hello hello cíle (replikovaného) HBase cluster. Příklad: dsthbcluster -s, cluster – src = dsthbcluster |
|-sp, – src-ambari heslo | Zadejte heslo správce hello pro Ambari na cluster HBase hello zdroje. |
|-distribučního bodu, – dst-ambari heslo | Zadejte heslo správce hello pro Ambari na cluster HBase cílové hello.|

Nepovinné argumenty:

|Name (Název)|Popis|
|----|-----------|
|-su, – src-ambari-user | Zadejte uživatelské jméno správce hello pro Ambari na cluster HBase hello zdroje. Hello výchozí hodnota je **správce**. |
|-du – dst-ambari-user | Zadejte uživatelské jméno správce hello pro Ambari na cluster HBase cílové hello. Hello výchozí hodnota je **správce**. |
|-t, – seznam tabulek | Zadejte hello tabulky toobe replikovat. Příklad: – seznam tabulek = "tabulky1; tabulky2; Tabulka3". Pokud nezadáte tabulky, replikují se všechny existující tabulky HBase.|
|-m, – počítač | Zadejte hello hlavního uzlu, kde se spustí akce skriptu hello. Hodnota Hello je hn1 nebo hn0. Protože hn0 je obvykle Vytíženější, doporučujeme používat hn1. Tuto možnost použijte, když spouštíte skript hello $0 jako akce skriptu z portálu hello HDInsight nebo Azure PowerShell.|
|-ip | Pokud jste povolení replikace mezi dvěma virtuálními sítěmi, je tento argument povinný. Tento argument funguje jako přepínač toouse hello statické IP adresy ZooKeeper uzly z repliky clusterů namísto názvů plně kvalifikovaný název domény. Hello statické IP adresy musí toobe předkonfigurované před povolením replikace. |
|-prohlášení cp, - programu copydata | Povolte hello migrace existujících dat na hello tabulky, kde je povolená replikace. |
|-ot. / min, - replikovat-phoenix-metadat | Povolení replikace pro Phoenix systémové tabulky. <br><br>*Tuto možnost použijte opatrně.* Doporučujeme, abyste před použitím tohoto skriptu znovu vytvořit Phoenix tabulek v clusterech repliky. |
|-h, – Nápověda | Zobrazí informace o využití. |

Hello print_usage() části hello [skriptu](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) poskytuje podrobné vysvětlení parametrů.

Po akce skriptu hello je úspěšně nasazen, můžete použít cluster HBase cílové toohello tooconnect SSH a ověřte, zda replikoval hello data.

### <a name="replication-scenarios"></a>Scénáře replikace

Hello následující seznam uvádí jste někdy obecné využití a jejich nastavení parametrů:

- **Povolení replikace ve všech tabulkách mezi hello dva clustery**. Tento scénář nevyžaduje hello kopírování nebo migrace existujících dat v tabulkách hello a nepoužívá Phoenix tabulky. Použijte hello následující parametry:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- **Povolit replikaci na konkrétní tabulky**. Použijte následující parametry tooenable replikace na tabulky1 a tabulky2, tabulka3 hello:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- **Povolit replikaci na konkrétní tabulky a zkopírujte existující data hello**. Použijte následující parametry tooenable replikace na tabulky1 a tabulky2, tabulka3 hello:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- **Povolení replikace na všechny tabulky s replikací phoenix metadat ze zdroje toodestination**. Phoenix metadata replikace není úplně bez chyby a by měla být povolená opatrně.

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a>Zkopírujte a migraci dat

Existují dvě samostatné skript akce skripty pro kopírování migraci dat po povolení replikace:

- [Skript pro malé tabulky](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (několik gigabajtů velikost a celkové kopie je očekávané toofinish za méně než jednu hodinu)

- [Skript pro rozsáhlé tabulky](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (očekávaný tootake déle než jednu hodinu toocopy)

Můžete postupovat podle hello stejným postupem v [povolit replikaci](#enable-replication) toocall hello akce skriptu s hello následující parametry:

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

Hello print_usage() části hello [skriptu](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) poskytuje podrobný popis parametrů.

### <a name="scenarios"></a>Scénáře

- **Zkopírujte konkrétní tabulky (test1, test2 a test3) pro všechny řádky upravit do nyní (aktuální časové razítko)**:

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  nebo

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- **Zkopírujte konkrétní tabulky s zadaný časový rozsah**:

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a>Zakázat replikaci

toodisable replikace, použijte jiný skript akce skriptu nacházející se v [Githubu](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh). Můžete postupovat podle hello stejným postupem v [povolit replikaci](#enable-replication) toocall hello akce skriptu s hello následující parametry:

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari Password> <-all|-t "table1;table2;...">  

Hello print_usage() části hello [skriptu](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) poskytuje podrobné vysvětlení parametrů.

### <a name="scenarios"></a>Scénáře

- **Zakázat replikaci ve všech tabulkách**:

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  nebo

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari username> --src-ambari-password=<source cluster Ambari password>

- **Zakázat replikaci na zadaných tabulek (tabulky1, tabulky2 a tabulka3)**:

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili jak replikace HBase tooconfigure přes dvě datových center. toolearn Další informace o HDInsight a HBase, najdete v části:

* [Začínáme s Apache HBase v HDInsight][hdinsight-hbase-get-started]
* [Přehled HDInsight HBase][hdinsight-hbase-overview]
* [Vytvořit clustery HBase v Azure Virtual Network][hdinsight-hbase-provision-vnet]
* [Analýza v reálném čase sentimentu Twitter s HBase][hdinsight-hbase-twitter-sentiment]
* [Analýza dat snímače pomocí Storm a HBase v HDInsight (Hadoop)][hdinsight-sensor-data]

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
