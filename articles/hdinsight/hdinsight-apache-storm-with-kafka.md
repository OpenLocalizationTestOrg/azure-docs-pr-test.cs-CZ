---
title: aaaUse Kafka Apache se Storm v HDInsight - Azure | Microsoft Docs
description: "Apache Kafka se instaluje s Apache Storm v HDInsight. Zjistěte, jak toowrite tooKafka a pak pro čtení z něj pomocí hello KafkaBolt a KafkaSpout součásti, které jsou součástí Storm. Také zjistěte, jak toouse hello tok framework toodefine a odesílání topologie Storm."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e4941329-1580-4cd8-b82e-a2258802c1a7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/21/2017
ms.author: larryfr
ms.openlocfilehash: 95701f51dfdf6f1a859dcde96d7053df4f21701f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-kafka-preview-with-storm-on-hdinsight"></a>Storm v HDInsight pomocí Apache Kafka (preview)

Zjistěte, jak toouse Apache Storm tooread z a zapisovat tooApache Kafka. Tento příklad také ukazuje, jak toosave data z toohello topologie Storm HDFS kompatibilního souboru systému používaného HDInsight.

> [!NOTE]
> Hello kroky v tomto dokumentu vytvořte skupinu prostředků Azure, která obsahuje oba Storm v HDInsight a Kafka na clusteru HDInsight. Tyto clustery jsou obě nachází v rámci virtuální síť Azure, což umožňuje hello toodirectly clusteru Storm komunikovat s hello Kafka clusteru.
> 
> Až skončíte s hello kroky v tomto dokumentu, mějte na paměti toodelete hello clustery tooavoid nadbytečné poplatky.

## <a name="get-hello-code"></a>Získat kód hello

Kód Hello hello příkladu v tomto dokumentu je k dispozici na [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).

toocompile tento projekt, musíte hello následující konfigurace pro vývojové prostředí:

* [Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) nebo vyšší. HDInsight 3.5 nebo vyšší vyžadují Java 8.

* [Maven 3.x](https://maven.apache.org/download.cgi)

* Klientem SSH (třeba hello `ssh` a `scp` příkazy) – informace najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* Textového editoru nebo IDE.

Hello následující proměnné prostředí může být nastaven při instalaci Java a hello JDK na pracovní stanici. Ale byste měli zkontrolovat, že existují a že obsahují hello správné hodnoty pro váš systém.

* `JAVA_HOME`-by měla odkazovat toohello adresář, kam hello JDK je nainstalovat.
* `PATH`-musí obsahovat hello následující cesty:
  
    * `JAVA_HOME`(nebo ekvivalentní cesta hello).
    * `JAVA_HOME\bin`(nebo ekvivalentní cesta hello).
    * Hello adresář, kde je nainstalován Maven.

## <a name="create-hello-clusters"></a>Vytvoření clusterů se hello

Apache Kafka v HDInsight neposkytuje přístup toohello Kafka zprostředkovatelé oproti hello veřejného Internetu. Všechno, co hello rozhovory tooKafka musí být ve stejné virtuální síti Azure jako uzly hello v hello Kafka clusteru. V tomto příkladu hello Kafka a clustery Storm jsou umístěny ve virtuální sítě Azure. Hello následující diagram znázorňuje tok komunikace mezi clustery hello:

![Diagram clustery Storm a Kafka v virtuální sítě Azure](./media/hdinsight-apache-storm-with-kafka/storm-kafka-vnet.png)

> [!NOTE]
> Další služby v clusteru hello, jako je SSH a Ambari je přístupná přes hello Internetu. Další informace o veřejné porty hello k dispozici s HDInsight naleznete v tématu [porty a identifikátory URI používají v prostředí HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Když vytvoříte virtuální síť Azure, Kafka, a clusterů Storm se ručně, je snazší toouse šablonu Azure Resource Manager. Použití hello následující kroky toodeploy virtuální síť Azure, Kafka, a clusterů Storm tooyour předplatného Azure.

1. Použijte hello tlačítko toosign v tooAzure a otevřete hello šablony v hello portálu Azure.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-storm-cluster-in-vnet-v2.json" target="_blank"><img src="./media/hdinsight-apache-storm-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   
    Hello šablony Azure Resource Manageru se nachází v **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**. Vytvoří hello následující prostředky:
    
    * Skupina prostředků Azure
    * Azure Virtual Network
    * Účet služby Azure Storage
    * Kafka v HDInsight verze 3.6 (tři uzly pracovního procesu)
    * Storm v HDInsight verze 3.6 (tři uzly pracovního procesu)

  > [!WARNING]
  > dostupnost tooguarantee Kafka v HDInsight, cluster musí obsahovat aspoň tři uzly pracovního procesu. Tato šablona vytvoří cluster Kafka, který obsahuje tři uzly pracovního procesu.

2. Hello použijte následující pokyny toopopulate hello položky na hello **vlastní nasazení** okno:
   
    ![HDInsight vlastní nasazení](./media/hdinsight-apache-storm-with-kafka/parameters.png)

    * **Skupina prostředků**: vytvoření skupiny nebo vyberte nějaký existující. Tato skupina obsahuje hello clusteru HDInsight.
   
    * **Umístění**: Vyberte tooyou geograficky zavřít umístění.

    * **Základní název clusteru**: Tato hodnota se používá jako hello základní název pro clustery Storm a Kafka hello. Například zadáním **hdi** vytvoří cluster Storm s názvem **storm hdi** a Kafka clusteru s názvem **kafka hdi**.
   
    * **Uživatelské jméno přihlášení clusteru**: uživatelské jméno správce hello u clusterů Storm a Kafka hello.
   
    * **Heslo pro přihlášení clusteru**: heslo uživatele správce hello u clusterů Storm a Kafka hello.
    
    * **Uživatelské jméno SSH**: hello toocreate uživatele SSH pro clustery Storm a Kafka hello.
    
    * **Heslo SSH**: hello heslo pro uživatele SSH hello u clusterů Storm a Kafka hello.

3. Čtení hello **podmínky a ujednání**a potom vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**.

4. Nakonec zkontrolujte **Pin toodashboard** a pak vyberte **nákupu**. Trvá přibližně 20 minut toocreate hello clustery.

Po vytvoření hello prostředky, zobrazí se okno hello pro skupinu prostředků hello.

![Okno skupiny prostředků pro virtuální síť hello a clustery](./media/hdinsight-apache-storm-with-kafka/groupblade.png)

> [!IMPORTANT]
> Všimněte si, že jsou názvy hello clusterů HDInsight hello **storm BASENAME** a **kafka BASENAME**, kde BASENAME je hello jméno, které jste zadali toohello šablony. Použít tyto názvy v dalších krocích při připojení toohello clustery.

## <a name="understanding-hello-code"></a>Pochopení kódu hello

Tento projekt obsahuje dvě topologie:

* **KafkaWriter**: definované hello **writer.yaml** souboru, zapisuje Tato topologie náhodných věty tooKafka pomocí hello KafkaBolt s Apache Storm.

    Tato topologie používá vlastní **SentenceSpout** součást toogenerate náhodných věty.

* **KafkaReader**: definované hello **reader.yaml** souborů, tato topologie čte data z Kafka pomocí hello KafkaSpout s Apache Storm a protokoly hello data toostdout.

    Tato topologie používá hello Storm HdfsBolt toowrite data toodefault úložiště pro hello Storm cluster.
### <a name="flux"></a>Tok

topologie Hello jsou definovány pomocí [tok](https://storm.apache.org/releases/1.1.0/flux.html). Tok byla zavedena v Storm 0.10.x a umožňuje vám tooseparate hello topologie konfigurace z kódu hello. Pro topologie, které používají hello tok framework hello topologie je definována v souboru YAML. Hello YAML soubor může být součástí hello topologie. Také může být samostatný soubor používá při odesílání topologie hello. Tok také podporuje nahrazení proměnné za běhu, který se používá v tomto příkladu.

Hello následující parametry jsou nastavené v době běhu pro tyto topologie:

* `${kafka.topic}`: název hello hello Kafka téma, které hello topologie pro čtení a zápis k.

* `${kafka.broker.hosts}`: hello hostitelem tohoto hello Kafka zprostředkovává spustili. Hello zprostředkovatele informace využívají hello KafkaBolt při zápisu tooKafka.

* `${kafka.zookeeper.hosts}`: hello hostitele, na kterých běží Zookeeper na v hello Kafka clusteru.

Další informace o topologiích tok najdete v tématu [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).

## <a name="download-and-compile-hello-project"></a>Stáhněte si a kompilaci hello projektu

1. Na vašem vývojovém prostředí, stáhněte si projekt hello z [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), otevřete příkazového řádku a změňte umístění toohello adresáře, který jste si stáhli hello projektu.

2. Z hello **hdinsight storm java kafka** adresář, použijte hello následující příkaz toocompile hello projektu a vytvořit balíček pro nasazení:

  ```bash
  mvn clean package
  ```

    proces balíčku Hello vytvoří soubor s názvem `KafkaTopology-1.0-SNAPSHOT.jar` v hello `target` adresáře.

3. Použijte následující příkazy toocopy hello balíček tooyour Storm v clusteru HDInsight hello. Nahraďte **uživatelské jméno** hello uživatelským jménem SSH pro hello cluster. Nahraďte **BASENAME** s názvem základní hello jste použili při vytváření clusteru hello.

  ```bash
  scp ./target/KafkaTopology-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
  ```

    Po zobrazení výzvy zadejte heslo hello, který jste použili při vytváření clusterů hello.

## <a name="configure-hello-topology"></a>Konfigurace hello topologie

1. Použijte jednu z následujících metod toodiscover hello hello hostitelů Kafka zprostředkovatele:

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $clusterName = Read-Host -Prompt "Enter hello Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($brokerHosts -join ":9092,") + ":9092"
    ```

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
    ```

    > [!IMPORTANT]
    > Hello Bash příklad předpokládá, že `$CLUSTERNAME` obsahuje název hello hello clusteru HDInsight. Dále předpokládá, že [jq](https://stedolan.github.io/jq/) je nainstalovaná. Po zobrazení výzvy zadejte hello heslo pro účet přihlášení hello clusteru.

    Vrácená hodnota Hello je podobné toohello následující text:

        wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092

    > [!IMPORTANT]
    > Ačkoli může existovat více než dva hostitele zprostředkovatele pro váš cluster, není nutné tooprovide úplný seznam všech tooclients hostitele. Jeden nebo dva kusy je dost.

2. Použijte jednu z hello následující metody toodiscover hello Kafka Zookeeper hostitelů:

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $clusterName = Read-Host -Prompt "Enter hello Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $zookeeperHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($zookeeperHosts -join ":2181,") + ":2181"
    ```

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2
    ```

    > [!IMPORTANT]
    > Hello Bash příklad předpokládá, že `$CLUSTERNAME` obsahuje název hello hello clusteru HDInsight. Dále předpokládá, že [jq](https://stedolan.github.io/jq/) je nainstalovaná. Po zobrazení výzvy zadejte hello heslo pro účet přihlášení hello clusteru.

    Vrácená hodnota Hello je podobné toohello následující text:

        zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181

    > [!IMPORTANT]
    > Existuje více než dva uzly Zookeeper, není nutné tooprovide úplný seznam všech tooclients hostitele. Jeden nebo dva kusy je dost.

    Tato hodnota, uložte, protože se později používá.

3. Upravit hello `dev.properties` soubor v kořenovém hello hello projektu. Přidejte hello zprostředkovatele a odpovídající řádky toohello, Zookeeper hostitele informace v tomto souboru. Hello následujícím příkladu je nakonfigurované použití hello ukázkové hodnoty z předchozích kroků hello:

        kafka.zookeeper.hosts: zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181
        kafka.broker.hosts: wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092
        kafka.topic: stormtopic

4. Uložit hello `dev.properties` souboru a pak použít hello následující příkaz tooupload ho toohello Storm cluster:

     ```bash
    scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
    ```

    Nahraďte **uživatelské jméno** hello uživatelským jménem SSH pro hello cluster. Nahraďte **BASENAME** s názvem základní hello jste použili při vytváření clusteru hello.

## <a name="start-hello-writer"></a>Spustit zapisovače hello

1. Použijte hello následující tooconnect toohello Storm clusteru pomocí protokolu SSH. Nahraďte **uživatelské jméno** s uživatelským jménem SSH hello používá při vytváření clusteru hello. Nahraďte **BASENAME** základní názvem hello používá při vytváření clusteru hello.

  ```bash
  ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net
  ```

    Po zobrazení výzvy zadejte heslo hello, který jste použili při vytváření clusterů hello.
   
    Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Z hello připojení SSH použijte následující příkaz toocreate hello Kafka tématu používané hello topologie hello:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic stormtopic --zookeeper $KAFKAZKHOSTS
    ```

    Nahraďte `$KAFKAZKHOSTS` s hello Zookeeper hostitele informace, které jste získali v předchozí části hello.

2. Z hello SSH připojení toohello Storm clusteru použijte následující příkaz toostart hello zapisovače topologie hello:

    ```bash
    storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
    ```

    Hello parametry tohoto příkazu jsou:

    * `org.apache.storm.flux.Flux`: Použije tok tooconfigure a spustí se tato topologie.

    * `--remote`: Odešlete tooNimbus topologie hello. Hello topologie se distribuuje do hello uzlů pracovního procesu v clusteru hello.

    * `-R /writer.yaml`: Použití hello `writer.yaml` souboru tooconfigure hello topologie. `-R`Označuje, že tento prostředek je součástí soubor jar hello. Je v kořenovém adresáři hello hello jar, takže `/writer.yaml` je cesta tooit hello.

    * `--filter`: Naplnění položek v hello `writer.yaml` topologie pomocí hodnot v hello `dev.properties` souboru. Například hello hodnotu hello `kafka.topic` záznam v souboru hello je použité tooreplace hello `${kafka.topic}` položku v definici topologie hello.

5. Po zahájení hello topologie, použijte následující příkaz tooverify, zapisuje data toohello Kafka tématu hello:

  ```bash
  /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --from-beginning --topic stormtopic
  ```

    Nahraďte `$KAFKAZKHOSTS` s hello Zookeeper hostitele informace, které jste získali v předchozí části hello.

    Tento příkaz používá skript dodaný s Kafka toomonitor hello tématu. Po chvíli by se měl spustit vrací náhodné věty, který byl toohello tématu. Hello výstup je podobné toohello následující ukázka:

        i am at two with nature             
        an apple a day keeps hello doctor away
        snow white and hello seven dwarfs     
        hello cow jumped over hello moon        
        an apple a day keeps hello doctor away
        an apple a day keeps hello doctor away
        hello cow jumped over hello moon        
        an apple a day keeps hello doctor away
        an apple a day keeps hello doctor away
        four score and seven years ago      
        snow white and hello seven dwarfs     
        snow white and hello seven dwarfs     
        i am at two with nature             
        an apple a day keeps hello doctor away

    Pomocí kombinace kláves Ctrl + c toostop hello skriptu.

## <a name="start-hello-reader"></a>Spustit čtečky hello

1. Z hello SSH relace toohello Storm clusteru použijte následující příkaz toostart hello čtečky topologie hello:

  ```bash
  storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties
  ```

2. Jakmile se spustí hello topologie, otevřete hello uživatelské rozhraní Storm. Tato webového uživatelského rozhraní se nachází v https://storm-BASENAME.azurehdinsight.net/stormui. Nahraďte __BASENAME__ základní názvem hello použita při vytvoření clusteru hello. 

    Pokud budete vyzváni, použijte přihlašovací jméno správce hello (výchozím `admin`) a heslo používané při vytvoření clusteru hello. Zobrazí webové stránky podobné toohello následující bitové kopie:

    ![Storm uživatelského rozhraní](./media/hdinsight-apache-storm-with-kafka/stormui.png)

3. Hello uživatelské rozhraní Storm, vyberte hello __kafka čtečky__ odkaz v hello __souhrn topologie__ části toodisplay informace o hello __kafka čtečky__ topologie.

    ![Topologie části Souhrn hello Storm webového uživatelského rozhraní](./media/hdinsight-apache-storm-with-kafka/topology-summary.png)

4. toodisplay informace o instancích hello hello protokolovacího nástroje bolt součásti, vyberte hello __protokolovač bolt__ odkaz v hello __funkce Bolts (všechny čas)__ části.

    ![Protokolovač bolt odkaz v části funkce bolts hello](./media/hdinsight-apache-storm-with-kafka/bolts.png)

5. V hello __vykonavatelů__ vyberte odkaz v hello __Port__ sloupec toodisplay protokolování informací o tuto instanci součásti hello.

    ![Vykonavatelů odkaz](./media/hdinsight-apache-storm-with-kafka/executors.png)

    Hello protokolu obsahuje protokolu hello data načtená z hello Kafka tématu. Hello informace v protokolu hello jsou podobné toohello následující text:

        2016-11-04 17:47:14.907 c.m.e.LoggerBolt [INFO] Received data: four score and seven years ago
        2016-11-04 17:47:14.907 STDIO [INFO] hello cow jumped over hello moon
        2016-11-04 17:47:14.908 c.m.e.LoggerBolt [INFO] Received data: hello cow jumped over hello moon
        2016-11-04 17:47:14.911 STDIO [INFO] snow white and hello seven dwarfs
        2016-11-04 17:47:14.911 c.m.e.LoggerBolt [INFO] Received data: snow white and hello seven dwarfs
        2016-11-04 17:47:14.932 STDIO [INFO] snow white and hello seven dwarfs
        2016-11-04 17:47:14.932 c.m.e.LoggerBolt [INFO] Received data: snow white and hello seven dwarfs
        2016-11-04 17:47:14.969 STDIO [INFO] an apple a day keeps hello doctor away
        2016-11-04 17:47:14.970 c.m.e.LoggerBolt [INFO] Received data: an apple a day keeps hello doctor away

## <a name="stop-hello-topologies"></a>Zastavení topologie hello

Z clusteru Storm SSH relace toohello použijte následující topologie Storm hello toostop příkazy hello:

  ```bash
  storm kill kafka-writer
  storm kill kafka-reader
  ```

## <a name="delete-hello-cluster"></a>Odstranění clusteru hello

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Vzhledem k tomu, že hello kroky v tomto dokumentu vytvořte oba clustery v hello stejnou skupinu prostředků Azure, můžete odstranit skupinu prostředků hello hello portálu Azure. Odstraněním skupiny prostředků hello odebere všechny prostředky, které jsou vytvořené pomocí následujících tohoto dokumentu.

## <a name="next-steps"></a>Další kroky

Další příklad topologie, které lze použít s Storm v HDInsight, naleznete v části [topologie Storm příklad a součásti](hdinsight-storm-example-topology.md).

Informace o nasazení a monitorování topologií v HDInsight se systémem Linux najdete v tématu [nasazení a správa topologií Apache Storm v HDInsight se systémem Linux](hdinsight-storm-deploy-monitor-topology-linux.md)