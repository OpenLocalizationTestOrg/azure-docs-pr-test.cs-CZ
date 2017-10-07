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
# <a name="availability-and-reliability-of-hadoop-clusters-in-hdinsight"></a>Dostupnost a spolehlivost clusterů Hadoop ve službě HDInsight

Clustery HDInsight poskytují dvou hlavních uzlech tooincrease hello dostupnost a spolehlivost Hadoop služeb a spuštěné úlohy.

Hadoop dosahuje vysoké dostupnosti a spolehlivosti replikace služeb a dat mezi několika uzly v clusteru. Ale standardní distribuce systému Hadoop obvykle mít pouze jeden hlavní uzel. Všechny výpadku hello jeden hlavní uzel může způsobit hello clusteru toostop práci. HDInsight poskytuje dostupnost a spolehlivost Hadoop tooimprove dva headnodes.

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="availability-and-reliability-of-nodes"></a>Dostupnost a spolehlivost uzlů

Uzly v clusteru HDInsight se implementují pomocí virtuálních počítačů Azure. Hello následující části popisují typy jednotlivých uzlů hello používá s HDInsight. 

> [!NOTE]
> Ne všechny typy uzlů se používají pro typ clusteru. Typ clusteru Hadoop například nemá žádné uzly Nimbus. Další informace o uzlech používá typy clusterů HDInsight, najdete v oddílu typy clusteru hello hello [vytvořit systémem Linux Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) dokumentu.

### <a name="head-nodes"></a>hlavních uzlech

tooensure vysokou dostupnost služby Hadoop, HDInsight nabízí dva uzly head. Obě hlavních uzlech jsou aktivní a jsou spuštěné v rámci clusteru HDInsight hello současně. Některé služby, jako je například HDFS nebo YARN, jsou pouze aktivní jeden hlavního uzlu v daném okamžiku. Dalšími službami, například HiveServer2 nebo Metaúložiště Hive jsou aktivní v obou uzlech head v hello stejnou dobu.

Uzly HEAD (a jiné uzly v HDInsight) mít číselnou hodnotu v rámci hello hostname hello uzlu. Například `hn0-CLUSTERNAME` nebo `hn4-CLUSTERNAME`.

> [!IMPORTANT]
> Nepřidružujte hello číselná hodnota k tom, zda je uzel primární nebo sekundární. Číselná hodnota Hello je pouze přítomen tooprovide jedinečný název pro každý uzel.

### <a name="nimbus-nodes"></a>Uzly Nimbus

Jsou k dispozici u clusterů Storm uzly nimbus. uzly Nimbus Hello zadejte toohello podobné funkce jako Hadoop JobTracker distribuci a monitorování zpracování napříč uzly pracovního procesu. HDInsight nabízí dva uzly Nimbus pro clustery Storm

### <a name="zookeeper-nodes"></a>Uzly zookeeper

[ZooKeeper](http://zookeeper.apache.org/) uzlech se používají pro vedoucí volba hlavní služeb v hlavních uzlech. Jsou také použít tooinsure, služby, datové (pracovník) uzly a bran vědět, které hlavního uzlu je aktivní na hlavní služby. Ve výchozím nastavení HDInsight poskytuje tři uzly ZooKeeper.

### <a name="worker-nodes"></a>Pracovní uzly

Pracovní uzly analýzám hello skutečná data odeslaná toohello clusteru po úlohu. V případě selhání pracovního uzlu hello úkol, který byl výkon je odeslaná tooanother pracovního uzlu. Ve výchozím nastavení vytvoří HDInsight čtyři uzly pracovního procesu. Toto číslo toosuit můžete změnit vašim potřebám, během a po vytvoření clusteru.

### <a name="edge-node"></a>Hraniční uzel

Hraniční uzel analýzu dat v rámci clusteru hello aktivně neúčastní. Používá se vývojáři nebo datových vědců při práci s Hadoop. Hello hraniční uzel život v hello stejné virtuální síti Azure jako hello jiné uzly v clusteru hello a přímý přístup k všechny ostatní uzly. Hello hraniční uzel lze použít bez nutnosti převádět prostředky z úlohy analýzy nebo důležité služby Hadoop.

V současné době R serverem v HDInsight je hello pouze clusteru typ, který poskytuje hraniční uzel ve výchozím nastavení. Pro R serverem v HDInsight, se používá hello hraniční uzel, na testovací R kód místně na uzlu hello před odesláním ji toohello clusteru pro distribuované zpracování.

Informace o používání hraniční uzel s typy clusteru než R Server najdete v tématu hello [používají uzly okraj v HDInsight](hdinsight-apps-use-edge-node.md) dokumentu.

## <a name="accessing-hello-nodes"></a>Přístup k hello uzly

Clusteru toohello přístupu přes hello Internetu je zajišťováno prostřednictvím veřejného brány. Přístup je omezená tooconnecting toohello hlavních uzlech a (pokud existuje) hello hraniční uzel. Přístup k tooservices systémem hlavních uzlech hello není provedena tak, že více hlavních uzlech. Hello veřejné brány trasy požadavky toohello hlavního uzlu, který hostuje hello požadované služby. Například pokud Ambari je aktuálně hostitelem hello sekundárního hlavního uzlu, brána hello směruje příchozí požadavky pro Ambari toothat uzel.

Přístup prostřednictvím brány veřejné hello je omezená tooport 443 (HTTPS), 22 a 23.

* Port __443__ je použité tooaccess Ambari a další webového uživatelského rozhraní nebo rozhraní REST API hostované v uzlech head hello.

* Port __22__ je použité tooaccess hello primární hlavní uzel nebo okraj uzlu pomocí protokolu SSH.

* Port __23__ je použité tooaccess hello sekundárního hlavního uzlu pomocí protokolu SSH. Například `ssh username@mycluster-ssh.azurehdinsight.net` připojí toohello primární hlavního uzlu clusteru hello s názvem **mycluster**.

Další informace o používání SSH najdete v tématu hello [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentu.

### <a name="internal-fully-qualified-domain-names-fqdn"></a>Interní plně kvalifikované názvy domény (FQDN)

Uzly v clusteru služby HDInsight mít interní IP adresu a plně kvalifikovaný název domény, který lze přistupovat pouze z clusteru hello. Pokud přístup ke službám v clusteru hello pomocí hello interní plně kvalifikovaný název domény nebo IP adresa, měli byste použít toouse Ambari tooverify hello IP adresu nebo plně kvalifikovaný název domény při přístupu k hello služby.

Například hello Oozie služby lze spustit jen u jednoho hlavního uzlu a pomocí hello `oozie` příkaz z relace SSH vyžaduje službu toohello hello adresy URL. Tato adresa URL může být načítat z Ambari pomocí hello následující příkaz:

    curl -u admin:PASSWORD "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url

Tento příkaz vrátí hodnotu podobné toohello následující příkaz, který obsahuje hello interní adresa URL toouse s hello `oozie` příkaz:

    "oozie.base.url": "http://hn0-CLUSTERNAME-randomcharacters.cx.internal.cloudapp.net:11000/oozie"

Další informace o práci s hello Ambari REST API najdete v tématu [monitorování a správě HDInsight pomocí Ambari REST API hello](hdinsight-hadoop-manage-ambari-rest-api.md).

### <a name="accessing-other-node-types"></a>Přístup k jiné typy uzlů

Můžete připojit toonodes, které nejsou přímo přístupné přes internet hello pomocí hello následující metody:

* **SSH**: Po připojení tooa hlavního uzlu pomocí protokolu SSH, potom můžete pomocí SSH z hello hlavního uzlu tooconnect tooother uzlů v clusteru hello. Další informace najdete v tématu hello [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentu.

* **Tunelové propojení SSH**: Pokud potřebujete tooaccess webovou službu hostovanou na jednom z uzlů hello, které není vystavený toohello internet, je nutné použít tunelového propojení SSH. Další informace najdete v tématu hello [použijte tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentu.

* **Virtuální síť Azure**: Pokud váš HDInsight clusteru je součástí virtuální síť Azure, jakýkoli prostředek na hello stejné virtuální sítě můžete přímý přístup k všechny uzly v clusteru hello. Další informace najdete v tématu hello [rozšířit HDInsight pomocí Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentu.

## <a name="how-toocheck-on-a-service-status"></a>Jak toocheck na stav služby

toocheck hello stav služby, které běží na hello hlavních uzlů se použít hello webové uživatelské rozhraní Ambari nebo hello Ambari REST API.

### <a name="ambari-web-ui"></a>Webovému uživatelskému rozhraní Ambari

Hello webové uživatelské rozhraní Ambari jsou viditelná na https://CLUSTERNAME.azurehdinsight.net. Nahraďte **CLUSTERNAME** s hello názvem vašeho clusteru. Pokud se zobrazí výzva, zadejte přihlašovací údaje uživatele hello HTTP pro váš cluster. Hello výchozí HTTP uživatelské jméno je **správce** a hello heslo je hello heslo, které jste zadali při vytváření clusteru hello.

Až přijedete na stránce Ambari hello, jsou uvedené služby hello nainstalovaná na hello nalevo od stránku hello.

![Nainstalovaná services](./media/hdinsight-high-availability-linux/services.png)

Existuje řada ikony, které se mohou zobrazit další stav tooindicate tooa služby. Žádné výstrahy týkající se služby tooa lze zobrazit pomocí hello **výstrahy** odkaz hello horní části stránky hello. Další informace o jeho můžete vybrat každou tooview služby.

Když stránku hello služby obsahuje informace o stavu hello a konfigurace jednotlivých služeb, neposkytuje informace, na kterém je spuštěna služba hello hlavního uzlu na. tooview tento informace, použijte hello **hostitele** odkaz hello horní části stránky hello. Tato stránka zobrazuje hostitelů v clusteru hello, včetně hlavních uzlech hello.

![seznam hostitelů](./media/hdinsight-high-availability-linux/hosts.png)

Výběr hello odkaz pro jeden z hlavních uzlech hello zobrazí hello služeb a komponent spuštěných na tomto uzlu.

![Stav součásti](./media/hdinsight-high-availability-linux/nodeservices.png)

Další informace o používání Ambari najdete v tématu [monitorování a správě HDInsight pomocí hello webové uživatelské rozhraní Ambari](hdinsight-hadoop-manage-ambari.md).

### <a name="ambari-rest-api"></a>Ambari REST API

Hello Ambari REST API je k dispozici prostřednictvím hello Internetu. veřejné brány HDInsight Hello zpracovává směrování požadavků uzlu head toohello, která je aktuálně hostitelem hello REST API.

Můžete použít následující příkaz toocheck hello stavu služby prostřednictvím hello Ambari REST API hello:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state

* Nahraďte **heslo** s hello HTTP heslo účtu uživatele (správce).
* Nahraďte **CLUSTERNAME** s názvem hello hello clusteru.
* Nahraďte **SERVICENAME** s názvem hello služby hello chcete toocheck hello stav.

Například stav hello toocheck hello **HDFS** služby v clusteru s názvem **mycluster**, s použitím hesla **heslo**, využije hello následující příkaz:

    curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state

odpověď Hello je podobné toohello následující JSON:

    {
      "href" : "http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
      "ServiceInfo" : {
        "cluster_name" : "mycluster",
        "service_name" : "HDFS",
        "state" : "STARTED"
      }
    }

Hello URL víme, že je aktuálně spuštěna služba hello hlavního uzlu s názvem **hn0 CLUSTERNAME**.

Hello stavu víme, že je služba hello aktuálně spuštěna, nebo **ZAČÍNÁME**.

Pokud si nejste jisti, jaké služby jsou nainstalovány na hello clusteru, můžete použít následující příkaz tooretrieve seznam hello:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services

Další informace o práci s hello Ambari REST API najdete v tématu [monitorování a správě HDInsight pomocí Ambari REST API hello](hdinsight-hadoop-manage-ambari-rest-api.md).

#### <a name="service-components"></a>Součásti služby

Služby mohou obsahovat součásti, které chcete stav hello toocheck jednotlivě. Například HDFS obsahuje součást NameNode hello. tooview informací na komponentu, bude příkaz hello:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

Pokud si nejste jisti, jaké součásti jsou poskytovány služby, můžete použít následující příkaz tooretrieve seznam hello:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

## <a name="how-tooaccess-log-files-on-hello-head-nodes"></a>Jak tooaccess soubory protokolu na hlavních uzlech hello

### <a name="ssh"></a>SSH

Při připojené tooa hlavního uzlu prostřednictvím SSH, souborů protokolu najdete v části **/var/log**. Například **/var/log/hadoop-yarn/yarn** neobsahuje protokoly YARN.

Každý hlavního uzlu mohou obsahovat položky jedinečný protokolu, takže byste měli zkontrolovat, že hello protokolů v obou.

### <a name="sftp"></a>SFTP

Můžete také připojit toohello hlavního uzlu pomocí hello SSH protokol FTP nebo rozhraní zabezpečení souboru Transfer Protocol (SFTP) a stahování souborů protokolu hello přímo.

Podobně jako toousing klient SSH, pokud se připojujete toohello clusteru, je třeba zadat hello uživatele účtu název a hello SSH adresa SSH hello clusteru. Například, `sftp username@mycluster-ssh.azurehdinsight.net`. Zadejte hello heslo pro účet hello po zobrazení výzvy, nebo zadejte veřejný klíč pomocí hello `-i` parametr.

Po připojení se zobrazí `sftp>` řádku. Z příkazového řádku, můžete změnit adresáře, nahrávání a stahování souborů. Můžete například změnit hello následující příkazy adresáře toohello **/var/log/hadoop/hdfs** adresář a pak stáhnout všechny soubory v adresáři hello.

    cd /var/log/hadoop/hdfs
    get *

Seznam dostupné příkazy, zadejte `help` v hello `sftp>` řádku.

> [!NOTE]
> Existují také grafické rozhraní, které vám umožňují toovisualize hello systému souborů při připojení pomocí protokolu SFTP. Například [MobaXTerm](http://mobaxterm.mobatek.net/) vám umožní systému souborů hello toobrowse pomocí podobné tooWindows rozhraní Průzkumníka.

### <a name="ambari"></a>Ambari

> [!NOTE]
> soubory pomocí nástroje Ambari protokolu tooaccess, je nutné použít tunelového propojení SSH. Hello webové rozhraní pro jednotlivé služby hello nejsou veřejně přístupné na hello Internetu. Informace o používání tunelového propojení SSH naleznete v tématu hello [používání tunelového propojení SSH](hdinsight-linux-ambari-ssh-tunnel.md) dokumentu.

Hello webové uživatelské rozhraní Ambari vyberte službu hello chcete tooview protokoly pro (například YARN). Potom pomocí **rychlé odkazy** tooselect které hello tooview hlavního uzlu v protokolech.

![Pomocí rychlé odkazy tooview protokoly](./media/hdinsight-high-availability-linux/viewlogs.png)

## <a name="how-tooconfigure-hello-node-size"></a>Jak tooconfigure hello velikost uzlu

Hello velikost uzlu lze vybrat pouze při vytváření clusteru. Můžete najít seznam hello různé velikosti virtuálních počítačů k dispozici pro HDInsight na hello [HDInsight stránce s cenami](https://azure.microsoft.com/pricing/details/hdinsight/).

Při vytváření clusteru, můžete zadat velikost hello hello uzlů. Hello následující informace poskytují pokyny o tom, jak pomocí velikost hello toospecify hello [portál Azure][preview-portal], [prostředí Azure PowerShell][azure-powershell], a hello [rozhraní příkazového řádku Azure][azure-cli]:

* **Portál Azure**: při vytváření clusteru, můžete nastavit hello velikost uzlů hello používá hello cluster:

    ![Obrázek průvodce vytvořením clusteru s výběrem velikost uzlu](./media/hdinsight-high-availability-linux/headnodesize.png)

* **Rozhraní příkazového řádku Azure**: při použití hello `azure hdinsight cluster create` příkaz hello velikost hello head, pracovního procesu a uzly ZooKeeper lze nastavit pomocí hello `--headNodeSize`, `--workerNodeSize`, a `--zookeeperNodeSize` parametry.

* **Prostředí Azure PowerShell**: při použití hello `New-AzureRmHDInsightCluster` rutiny, můžete nastavit velikost hello hello head, pracovního procesu a uzly ZooKeeper pomocí hello `-HeadNodeVMSize`, `-WorkerNodeSize`, a `-ZookeeperNodeSize` parametry.

## <a name="next-steps"></a>Další kroky

Použijte následující odkazy toolearn více informací o věcí uvedených v tomto dokumentu hello.

* [Ambari REST odkaz](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [Instalace a konfigurace hello rozhraní příkazového řádku Azure](../cli-install-nodejs.md)
* [Nainstalujte a nakonfigurujte Azure PowerShell.](/powershell/azure/overview)
* [Spravovat HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md)
* [Zřizování clusterů HDInsight se systémem Linux](hdinsight-hadoop-provision-linux-clusters.md)

[preview-portal]: https://portal.azure.com/
[azure-powershell]: /powershell/azureps-cmdlets-docs
[azure-cli]: ../cli-install-nodejs.md
