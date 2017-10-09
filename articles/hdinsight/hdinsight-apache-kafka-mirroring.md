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
# <a name="use-mirrormaker-tooreplicate-apache-kafka-topics-with-kafka-on-hdinsight-preview"></a>Použití MirrorMaker tooreplicate Apache Kafka témata s Kafka v HDInsight (preview)

Zjistěte, jak je toouse Apache Kafka zrcadlení funkce tooreplicate témata tooa sekundární clusteru. Zrcadlení lze byla spuštěna jako nepřetržitý proces, nebo použít občas jako metodu migrace dat z jednoho clusteru tooanother.

V tomto příkladu je zrcadlení použité tooreplicate témata mezi dvěma clustery HDInsight. Oba clustery jsou ve virtuální síti Azure v hello stejné oblasti.

> [!WARNING]
> Zrcadlení by se neměla považovat jako znamená tooachieve odolnosti proti chybám. Posunutí tooitems Hello v tématu se liší mezi zdrojovým a cílovým clustery hello, takže klienti nemohou použít hello dva zcela zaměnitelným významem.
>
> Pokud máte obavy o odolnost proti chybám, byste měli nastavit replikace pro hello témata v rámci clusteru. Další informace najdete v tématu [začít pracovat s Kafka v HDInsight](hdinsight-apache-kafka-get-started.md).

## <a name="how-kafka-mirroring-works"></a>Jak funguje Kafka zrcadlení

Zrcadlení funguje s použitím hello MirrorMaker nástroj (součást Apache Kafka) tooconsume od témata na hello zdrojovém clusteru a pak vytvořit místní kopii na hello cílový cluster. MirrorMaker používá (nejméně jeden) *příjemci* který číst z hello zdrojovém clusteru a *producent* , zapíše toohello místní (cíl) clusteru.

Hello následující diagram znázorňuje proces zrcadlení hello:

![Diagram hello zrcadlení procesu](./media/hdinsight-apache-kafka-mirroring/kafka-mirroring.png)

Apache Kafka v HDInsight neposkytuje oproti hello přístup toohello Kafka služby veřejného Internetu. Producenti Kafka nebo příjemci musí být v hello stejné virtuální síti Azure jako hello uzly v clusteru Kafka hello. V tomto příkladu hello Kafka zdroje a cílových clusterech jsou umístěny ve virtuální sítě Azure. Hello následující diagram znázorňuje tok komunikace mezi clustery hello:

![Diagram zdrojové a cílové Kafka clusterů v virtuální sítě Azure](./media/hdinsight-apache-kafka-mirroring/spark-kafka-vnet.png)

Hello zdrojové a cílové clustery mohou být různé hello počtu uzlů a oddíly a odsazení v rámci témata hello se také liší. Zrcadlení udržuje hello hodnotu klíče, který se používá pro vytváření oddílů, takže pořadí záznamů se zachová, i na základě na klíč.

### <a name="mirroring-across-network-boundaries"></a>Zrcadlení napříč síťovými hranicemi

Pokud potřebujete toomirror mezi clustery Kafka v jiných sítích, existují hello následující další důležité informace:

* **Brány**: hello sítě musí být schopný toocommunicate v hello TCPIP úroveň.

* **Překlad názvů**: hello Kafka clusterů v každé sítě musí být schopný tooconnect tooeach jiných pomocí názvy hostitelů. To může vyžadovat, že server systému DNS (Domain Name) v každé sítě, která je nakonfigurovaná tooforward požadavky toohello další sítě.

    Při vytváření virtuální síť Azure, místo použití hello poskytnuté automatické DNS s hello síť, musíte zadat vlastní DNS serveru a hello IP adresu pro hello server. Po hello, který byl vytvořen virtuální sítě musí pak vytvořit virtuální počítač Azure, který používá IP adresu, pak nainstalujte a nakonfigurujte DNS software na něm.

    > [!WARNING]
    > Vytvořit a nakonfigurovat hello vlastního serveru DNS před instalací HDInsight do hello virtuální sítě. Neexistuje žádná další konfigurace požadované pro HDInsight toouse hello server DNS nakonfigurovaný pro hello virtuální sítě.

Další informace o připojení dvou virtuálních sítí Azure najdete v tématu [konfigurace připojení typu VNet-to-VNet](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

## <a name="create-kafka-clusters"></a>Vytvoření Kafka clusterů

Když vytvoříte virtuální síť Azure a Kafka clusterů ručně, je snazší toouse šablonu Azure Resource Manager. Použijte následující kroky toodeploy hello virtuální sítě Azure a dvě Kafka clustery tooyour předplatného Azure.

1. Použijte hello tlačítko toosign v tooAzure a otevřete hello šablony v hello portálu Azure.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-kafka-mirroring/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   
    Hello šablony Azure Resource Manageru se nachází v **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.

    > [!WARNING]
    > dostupnost tooguarantee Kafka v HDInsight, cluster musí obsahovat aspoň tři uzly pracovního procesu. Tato šablona vytvoří cluster Kafka, který obsahuje tři uzly pracovního procesu.

2. Hello použijte následující informace toopopulate hello položky na hello **vlastní nasazení** okno:
    
    ![HDInsight vlastní nasazení](./media/hdinsight-apache-kafka-mirroring/parameters.png)
    
    * **Skupina prostředků**: vytvoření skupiny nebo vyberte nějaký existující. Tato skupina obsahuje hello clusteru HDInsight.

    * **Umístění**: Vyberte tooyou geograficky zavřít umístění.
     
    * **Základní název clusteru**: Tato hodnota se používá jako hello základní název pro hello Kafka clusterů. Například zadáním **hdi** vytvoří clustery s názvem **zdroj hdi** a **cíle hdi**.

    * **Uživatelské jméno přihlášení clusteru**: hello uživatelské jméno správce pro hello zdrojové a cílové Kafka clusterů.

    * **Heslo pro přihlášení clusteru**: heslo uživatele správce hello k hello zdrojové a cílové Kafka clusterů.

    * **Uživatelské jméno SSH**: hello SSH uživatele toocreate pro hello zdrojové a cílové Kafka clusterů.

    * **Heslo SSH**: hello heslo pro uživatele SSH hello hello zdrojové a cílové Kafka clusterů.

3. Čtení hello **podmínky a ujednání**a potom vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**.

4. Nakonec zkontrolujte **Pin toodashboard** a pak vyberte **nákupu**. Trvá přibližně 20 minut toocreate hello clustery.

Po vytvoření hello prostředky jste přesměrovaného tooa okně hello skupinu prostředků, která obsahuje hello clustery a webové řídicí panel.

![Okno skupiny prostředků pro virtuální síť hello a clustery](./media/hdinsight-apache-kafka-mirroring/groupblade.png)

> [!IMPORTANT]
> Všimněte si, že jsou názvy hello clusterů HDInsight hello **zdroj BASENAME** a **cíle BASENAME**, kde BASENAME je hello jméno, které jste zadali toohello šablony. Použít tyto názvy v dalších krocích při připojení toohello clustery.

## <a name="create-topics"></a>Vytvoření témata

1. Připojit toohello **zdroj** clusteru pomocí protokolu SSH:

    ```bash
    ssh sshuser@source-BASENAME-ssh.azurehdinsight.net
    ```

    Nahraďte **sshuser** s uživatelským jménem SSH hello používá při vytváření clusteru hello. Nahraďte **BASENAME** základní názvem hello používá při vytváření clusteru hello.

    Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Použití hello následující příkazy toofind hello Zookeeper hostitele pro zdrojový cluster hello:

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

3. Byla vytvořena hello použijte následující příkaz tooverify, který hello tématu:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $SOURCE_ZKHOSTS
    ```

    Hello odpovědi obsahuje `testtopic`.

4. Použití hello následující tooview hello Zookeeper hostitele informace pro tento (hello **zdroj**) clusteru:

    ```bash
    echo $SOURCE_ZKHOSTS
    ```

    Tento příkaz vrátí informace podobné toohello následující text:

    `zk0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181,zk1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181`

    Tyto informace uložte. Používá se v další části hello.

## <a name="configure-mirroring"></a>Konfigurace zrcadlení

1. Připojit toohello **cílové** clusteru pomocí jiné relace SSH:

    ```bash
    ssh sshuser@dest-BASENAME-ssh.azurehdinsight.net
    ```

    Nahraďte **sshuser** s uživatelským jménem SSH hello používá při vytváření clusteru hello. Nahraďte **BASENAME** základní názvem hello používá při vytváření clusteru hello.

    Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Použití hello následující příkaz toocreate `consumer.properties` soubor, který popisuje, jak toocommunicate s hello **zdroj** clusteru:

    ```bash
    nano consumer.properties
    ```

    Použití hello následující text jako hello obsah hello `consumer.properties` souboru:

    ```yaml
    zookeeper.connect=SOURCE_ZKHOSTS
    group.id=mirrorgroup
    ```

    Nahraďte **SOURCE_ZKHOSTS** s hello Zookeeper hostitelem informace z hello **zdroj** clusteru.

    Tento soubor popisuje hello příjemce informace toouse při čtení ze zdroje hello Kafka clusteru. Další informace o uživatelských nastavení, naleznete v tématu [příjemce konfigurací](https://kafka.apache.org/documentation#consumerconfigs) v kafka.apache.org.

    toosave hello soubor, použijte **kombinaci kláves Ctrl + X**, **Y**a potom **Enter**.

3. Před konfigurací hello producent, který komunikuje s hello cílový cluster, musíte vyhledat hello zprostředkovatele hostitele pro hello **cílové** clusteru. Použijte následující příkazy tooretrieve hello tyto informace:

    ```bash
    sudo apt -y install jq
    DEST_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $DEST_BROKERHOSTS
    ```

    Nahraďte `$PASSWORD` s hello přihlášení heslo účtu (správce) pro hello cluster.

    Nahraďte `$CLUSTERNAME` s názvem hello hello cílový cluster.

    Tyto příkazy vrátit podobné toohello následující informace:

        wn0-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn1-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092

4. Použití hello následující toocreate `producer.properties` soubor, který popisuje, jak toocommunicate s hello **cílové** clusteru:

    ```bash
    nano producer.properties
    ```

    Použití hello následující text jako hello obsah hello `producer.properties` souboru:

    ```yaml
    bootstrap.servers=DEST_BROKERS
    compression.type=none
    ```

    Nahraďte **DEST_BROKERS** s informacemi o zprostředkovatele hello hello v předchozím kroku.

    Další informace o producent konfigurace, najdete v části [producent konfigurací](https://kafka.apache.org/documentation#producerconfigs) v kafka.apache.org.

## <a name="start-mirrormaker"></a>Spustit MirrorMaker

1. Z toohello připojení SSH hello **cílové** clusteru, použijte následující příkaz toostart hello MirrorMaker proces hello:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    Hello parametry použité v tomto příkladu jsou:

    * **--consumer.config**: Určuje hello soubor, který obsahuje příjemce vlastnosti. Tyto vlastnosti jsou použité toocreate příjemce, který čte z hello *zdroj* Kafka clusteru.

    * **--producer.config**: Určuje hello soubor, který obsahuje producent vlastnosti. Tyto vlastnosti jsou použité toocreate producent, který zapíše toohello *cílové* Kafka clusteru.

    * **seznam povolených adres –**: seznam témat, která replikuje MirrorMaker z hello zdroj clusteru toohello cíl.

    * **--num.streams**: hello počet vláken toocreate příjemce.

 Při spuštění vrátí MirrorMaker informace podobné toohello následující text:

    ```json
    {metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-3, security.protocol=PLAINTEXT}{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-0, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-kafka.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-2, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-1, security.protocol=PLAINTEXT}
    ```

2. Z toohello připojení SSH hello **zdroj** clusteru, použijte následující příkaz toostart producent hello a odesílat zprávy toohello tématu:

    ```bash
    SOURCE_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

    Nahraďte `$PASSWORD` s hello heslo pro přihlášení (správce) pro zdrojový cluster hello.

    Nahraďte `$CLUSTERNAME` s názvem hello hello zdrojového clusteru.

     Až přijedete do prázdný řádek s kurzoru, zadejte několik textové zprávy. Tyto jsou odesílány toohello tématu na hello **zdroj** clusteru. Až budete hotoví, použijte **kombinaci kláves Ctrl + C** tooend hello producent procesu.

3. Z toohello připojení SSH hello **cílové** clusteru, použijte **kombinaci kláves Ctrl + C** tooend hello MirrorMaker procesu. Pak použijte hello následující příkazy tooverify této hello `testtopic` tématu byl vytvořen a tato data v tématu hello byl replikované toothis zrcadlení:

    ```bash
    DEST_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $DEST_ZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $DEST_ZKHOSTS --topic testtopic --from-beginning
    ```

    Nahraďte `$PASSWORD` s hello heslo pro přihlášení (správce) pro hello cílový cluster.

    Nahraďte `$CLUSTERNAME` s názvem hello hello cílový cluster.

    Hello seznam témat nyní zahrnuje `testtopic`, který se vytvoří při MirrorMaster zrcadlí hello téma z hello zdroj clusteru toohello cíl. zprávy Hello načtena z tématu hello jsou, stejně jako na zdrojovém clusteru hello zadán hello.

## <a name="delete-hello-cluster"></a>Odstranění clusteru hello

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Vzhledem k tomu, že hello kroky v tomto dokumentu vytvořte oba clustery v hello stejnou skupinu prostředků Azure, můžete odstranit skupinu prostředků hello hello portálu Azure. Odstraněním skupiny prostředků hello odebere všechny prostředky, které jsou vytvořené pomocí následujících tento dokument, hello virtuální sítě Azure a účet úložiště, které jsou používané clustery hello.

## <a name="next-steps"></a>Další kroky

V tomto dokumentu jste zjistili, jak toouse MirrorMaker toocreate repliku Kafka clusteru. Použijte následující odkazy toodiscover hello jiné způsoby toowork s Kafka:

* [Dokumentaci Apache Kafka MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) v cwiki.apache.org.
* [Začínáme s Apache Kafka v HDInsight](hdinsight-apache-kafka-get-started.md)
* [Použití Apache Sparku se systémem Kafka ve službě HDInsight](hdinsight-apache-spark-with-kafka.md)
* [Použití Apache Stormu se systémem Kafka ve službě HDInsight](hdinsight-apache-storm-with-kafka.md)
* [Připojení tooKafka přes virtuální síť Azure](hdinsight-apache-kafka-connect-vpn-gateway.md)
