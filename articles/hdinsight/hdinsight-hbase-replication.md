---
title: "Konfigurace replikace HBase clusteru v rámci virtuální sítě - Azure | Microsoft Docs"
description: "Postup konfigurace replikace HBase pro vyrovnávání zatížení, vysokou dostupnost, nula. výpadků migrace nebo aktualizaci z jedné verze HDInsight do druhého a zotavení po havárii."
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
ms.openlocfilehash: 895709391486acb4a9d7a54ef046956539913f7b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-hbase-cluster-replication-within-virtual-networks"></a>Konfigurace replikace HBase clusteru v rámci virtuální sítě

Postup konfigurace replikace HBase v rámci jednu virtuální síť (VNet) nebo mezi dvě virtuální sítě.

Clusterová replikace používá metodika nabízené zdroje. HBase cluster může být zdroj nebo cíl, nebo ji můžete najednou splnění obě role. Replikace je asynchronní a cílem replikace je konzistence typu případné. Když zdroj obdrží upravíte k rodin sloupců s povolenou replikací, že úpravy rozšířena do všech cílových clusterech. Pokud data se replikují z jednoho clusteru do druhého, zdrojovém clusteru a všechny clustery, které jste již využívat data jsou sledovány aby replikační cykly.

V tomto kurzu nakonfigurujete zdroj cíl replikace. Jiných topologiích clusteru najdete v části [referenční příručka Apache HBase](http://hbase.apache.org/book.html#_cluster_replication).

Případy využití HBase replikace pro jedné virtuální sítě:

* Vyrovnávání zatížení – například spuštění kontroly nebo úloh MapReduce v cílovém clusteru a příjem dat ve zdrojovém clusteru
* Vysoká dostupnost
* Migrace dat z jednoho clusteru HBase do jiného
* Upgrade clusteru Azure HDInsight z jedné verze do jiného

Případy využití HBase replikace pro dvě virtuální sítě:

* Zotavení po havárii
* Vyrovnávání zatížení a rozdělení do oddílů aplikace
* Vysoká dostupnost

Při replikaci clustery pomocí [skript akce](hdinsight-hadoop-customize-cluster-linux.md) skripty nacházející se v [Githubu](https://github.com/Azure/hbase-utils/tree/master/replication).

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="configure-the-environments"></a>Konfigurace prostředí

Existují tři možné konfigurace:

- Dva clustery HBase v jednu virtuální síť Azure
- Dva clustery HBase ve dvou různých virtuálních sítích ve stejné oblasti
- Dva clustery HBase ve dvou různých virtuálních sítích v různých oblastech dva (geografická replikace)

Aby bylo snazší lze konfigurovat prostředí, vytvořili jsme některé [šablon Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md). Pokud dáváte přednost lze konfigurovat prostředí a použití jiných metod, najdete v části:

- [Vytvořit clustery se systémem Linux Hadoop v HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
- [Vytvořit clustery HBase v Azure Virtual Network](hdinsight-hbase-provision-vnet.md)

### <a name="configure-one-virtual-network"></a>Nakonfigurujte jednu virtuální síť

Kliknutím na následující obrázek vytvořili dva clustery HBase ve stejné virtuální síti. Šablona je uložena v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-one-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

### <a name="configure-two-virtual-networks-in-the-same-region"></a>Konfigurace dvou virtuálních sítí ve stejné oblasti

Kliknutím na následující obrázek vytvořte dvě virtuální sítě pomocí virtuální sítě partnerský vztah a dva clustery HBase ve stejné oblasti. Šablona je uložena v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-two-vnets-same-region%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>



Tento scénář vyžaduje [VNet peering](../virtual-network/virtual-network-peering-overview.md). Šablona umožňuje partnerský vztah virtuální sítě.   

Replikace HBase používá IP adresy virtuálních počítačů ZooKeeper. Je nutné nakonfigurovat statické IP adresy pro cílové uzly HBase ZooKeeper.

**Konfigurace statické IP adresy**

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V levé nabídce klikněte na tlačítko **skupiny prostředků**.
3. Klikněte na tlačítko vaší skupiny prostředků, který obsahuje cílový cluster HBase. Toto je skupina prostředků, které jste zadali při jste použili k vytvoření prostředí šablony Resource Manageru. Filtr můžete použít k upřesnění seznamu. Zobrazí seznam prostředků, který obsahuje dvě virtuální sítě.
4. Klikněte na virtuální síť, která obsahuje cílový cluster HBase. Klikněte například na **xxxx-vnet2**. Zobrazí se tří zařízení s názvy, které začínají **seskupování-zookeepermode -**. Tato zařízení jsou tři ZooKeeper virtuálních počítačů.
5. Klikněte na jednu ZooKeeper virtuálních počítačů.
6. Klikněte na tlačítko **konfigurace protokolu IP**.
7. Klikněte na tlačítko **ipConfig1** ze seznamu.
8. Klikněte na tlačítko **statické**a zapište si IP adresu skutečného. IP adresu budete potřebovat při spuštění akce skriptu k povolení replikace.

  ![HDInsight HBase replikace ZooKeeper statickou IP adresu](./media/hdinsight-hbase-replication/hdinsight-hbase-replication-zookeeper-static-ip.png)

9. Zopakujte krok 6 nastavit statickou IP adresu pro další dva uzly ZooKeeper.

Pro scénář cross-VNet, je nutné použít **- ip** při volání metody přepnout **hdi_enable_replication.sh** skript akce.

### <a name="configure-two-virtual-networks-in-two-different-regions"></a>Nakonfigurovat dvě virtuální sítě ve dvou různých oblastech

Kliknutím na následující obrázek vytvořit dvě virtuální sítě ve dvou různých oblastech. Šablona je uložena ve veřejném kontejneru objektů Blob v Azure.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fdeploy-hbase-geo-replication.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

Vytvoření brány VPN mezi dvěma virtuálními sítěmi. Pokyny najdete v tématu [vytvoření virtuální sítě s připojením site-to-site](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

Replikace HBase používá IP adresy virtuálních počítačů ZooKeeper. Je nutné nakonfigurovat statické IP adresy pro cílové uzly HBase ZooKeeper. Pokud chcete nakonfigurovat statickou IP adresu, najdete v části "Konfigurace dvou virtuálních sítí ve stejné oblasti" v tomto článku.

Pro scénář cross-VNet, je nutné použít **- ip** při volání metody přepnout **hdi_enable_replication.sh** skript akce.

## <a name="load-test-data"></a>Testovací data zatížení

Při replikaci cluster, je nutné zadat tabulky, které chcete replikovat. Některá data v této části se načte do zdrojového clusteru. V další části povolíte replikaci mezi dvěma clustery.

Postupujte podle pokynů v [kurz HBase: začněte používat Apache HBase se systémem Linux Hadoop v HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) k vytvoření **kontakty** tabulku a vložte některá data do tabulky.

## <a name="enable-replication"></a>Povolení replikace

Následující kroky ukazují, jak volat skript akce skriptu z portálu Azure. Spuštění skriptu akci pomocí prostředí Azure PowerShell a rozhraní příkazového řádku Azure (CLI), najdete v části [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).

**Povolení replikace HBase z portálu Azure**

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Otevřete zdrojový cluster HBase.
3. V nabídce clusteru, klikněte na tlačítko **akcí skriptů**.
4. Klikněte na tlačítko **odeslání nové** z horní části okna.
5. Vyberte nebo zadejte následující informace:

  - **Název**: Zadejte **povolit replikaci**.
  - **Adresa URL skriptu bash**: Zadejte **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.
  - **HEAD**: vybraná. Zrušte jiné typy uzlů.
  - **Parametry**: Následující ukázka parametry povolit replikaci pro všechny existující tabulky a zkopírujte všechny do cílového clusteru clusteru data ze zdroje:

            -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata

6. Klikněte na možnost **Vytvořit**. Skript může trvat nějakou dobu, hlavně když se použije argument - programu copydata.

Vyžaduje argumenty:

|Name (Název)|Popis|
|----|-----------|
|-s, – src-cluster | Zadejte název DNS clusteru HBase zdroje. Příklad: hbsrccluster -s, cluster – src = hbsrccluster |
|-d, – dst clusteru | Zadejte název DNS clusteru HBase cílové (replikovaného). Příklad: dsthbcluster -s, cluster – src = dsthbcluster |
|-sp, – src-ambari heslo | Zadejte heslo správce pro Ambari ve zdrojovém clusteru HBase. |
|-distribučního bodu, – dst-ambari heslo | Zadejte heslo správce pro Ambari v cílovém clusteru HBase.|

Nepovinné argumenty:

|Name (Název)|Popis|
|----|-----------|
|-su, – src-ambari-user | Zadejte uživatelské jméno správce pro Ambari ve zdrojovém clusteru HBase. Výchozí hodnota je **správce**. |
|-du – dst-ambari-user | Zadejte uživatelské jméno správce pro Ambari v cílovém clusteru HBase. Výchozí hodnota je **správce**. |
|-t, – seznam tabulek | Zadejte tabulky, které chcete replikovat. Příklad: – seznam tabulek = "tabulky1; tabulky2; Tabulka3". Pokud nezadáte tabulky, replikují se všechny existující tabulky HBase.|
|-m, – počítač | Zadejte hlavního uzlu, kde se spustí akce skriptu. Hodnota je hn1 nebo hn0. Protože hn0 je obvykle Vytíženější, doporučujeme používat hn1. Tuto možnost použijte, když spouštíte skript $0 jako akce skriptu z portálu HDInsight nebo Azure PowerShell.|
|-ip | Pokud jste povolení replikace mezi dvěma virtuálními sítěmi, je tento argument povinný. Tento argument funguje jako přepínač pro použití statické IP adresy ZooKeeper uzlů z clusterů repliky místo názvy plně kvalifikovaný název domény. Statické IP adresy musí být předem nakonfigurované před povolením replikace. |
|-prohlášení cp, - programu copydata | Povolte migraci existujících dat pro tabulky, kde je povolená replikace. |
|-ot. / min, - replikovat-phoenix-metadat | Povolení replikace pro Phoenix systémové tabulky. <br><br>*Tuto možnost použijte opatrně.* Doporučujeme, abyste před použitím tohoto skriptu znovu vytvořit Phoenix tabulek v clusterech repliky. |
|-h, – Nápověda | Zobrazí informace o využití. |

Části print_usage() [skriptu](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) poskytuje podrobné vysvětlení parametrů.

Po úspěšném nasazení akce skriptu, můžete pro připojení k cílový cluster HBase a ověřte, že data se replikují SSH.

### <a name="replication-scenarios"></a>Scénáře replikace

V následujícím seznamu jsou někdy obecné využití a jejich nastavení parametrů:

- **Povolení replikace ve všech tabulkách mezi dvěma clustery**. Tento scénář nevyžaduje kopírování nebo migrace existujících dat v tabulkách a nepoužívá Phoenix tabulky. Použijte následující parametry:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- **Povolit replikaci na konkrétní tabulky**. Povolit replikaci na tabulky1 a tabulky2, tabulka3 pomocí následujících parametrů:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- **Povolit replikaci na konkrétní tabulky a zkopírujte existující data**. Povolit replikaci na tabulky1 a tabulky2, tabulka3 pomocí následujících parametrů:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- **Povolení replikace na všechny tabulky s replikací phoenix metadata ze zdrojového do cílového umístění**. Phoenix metadata replikace není úplně bez chyby a by měla být povolená opatrně.

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a>Zkopírujte a migraci dat

Existují dvě samostatné skript akce skripty pro kopírování migraci dat po povolení replikace:

- [Skript pro malé tabulky](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (několik gigabajtů velikost a celkové kopie očekává se dokončit za méně než jednu hodinu)

- [Skript pro rozsáhlé tabulky](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (očekávaný trvá déle než jednu hodinu kopírování)

Můžete postupujte stejným způsobem v [povolit replikaci](#enable-replication) volání akce skriptu s následujícími parametry:

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

Části print_usage() [skriptu](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) poskytuje podrobný popis parametrů.

### <a name="scenarios"></a>Scénáře

- **Zkopírujte konkrétní tabulky (test1, test2 a test3) pro všechny řádky upravit do nyní (aktuální časové razítko)**:

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  nebo

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- **Zkopírujte konkrétní tabulky s zadaný časový rozsah**:

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a>Zakázat replikaci

Zakázat replikaci, použít jiný skript akce skriptu nacházející se v [Githubu](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh). Můžete postupujte stejným způsobem v [povolit replikaci](#enable-replication) volání akce skriptu s následujícími parametry:

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari Password> <-all|-t "table1;table2;...">  

Části print_usage() [skriptu](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) poskytuje podrobné vysvětlení parametrů.

### <a name="scenarios"></a>Scénáře

- **Zakázat replikaci ve všech tabulkách**:

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  nebo

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari username> --src-ambari-password=<source cluster Ambari password>

- **Zakázat replikaci na zadaných tabulek (tabulky1, tabulky2 a tabulka3)**:

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili konfigurace replikace HBase přes dvě datových center. Další informace o HDInsight a HBase naleznete v tématu:

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
