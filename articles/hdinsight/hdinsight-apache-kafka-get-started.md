---
title: aaaStart s Apache Kafka - Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak toocreate Kafka Apache clusteru v Azure HDInsight. Zjistěte, jak toocreate témata, odběratele a příjemce."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 43585abf-bec1-4322-adde-6db21de98d7f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: b93299d88dc2cf9a9764662509308ff75fd74474
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="start-with-apache-kafka-preview-on-hdinsight"></a>Začínáme s Apache Kafka (Preview) v prostředí HDInsight

Zjistěte, jak toocreate a používat [Apache Kafka](https://kafka.apache.org) clusteru v Azure HDInsight. Kafka je opensourcová distribuovaná streamovací platforma, která je dostupná pro HDInsight. Často se používá jako zprostředkovatel zprávu, protože poskytuje podobné funkce tooa publikování a odběru fronta zpráv.

> [!NOTE]
> Aktuálně jsou pro HDInsight dostupné dvě verze Kafka: 0.9.0 (HDInsight 3.4) a 0.10.0 (HDInsight 3.5 a 3.6). Hello kroky v tomto dokumentu předpokládají, že používáte Kafka na HDInsight 3.6.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="create-a-kafka-cluster"></a>Vytvoření clusteru Kafka

Použijte následující postup toocreate Kafka na clusteru HDInsight hello:

1. Z hello [portál Azure](https://portal.azure.com), vyberte **+ nový**, **Intelligence + analýzy**a potom vyberte **HDInsight**.
   
    ![Vytvoření clusteru HDInsight](./media/hdinsight-apache-kafka-get-started/create-hdinsight.png)

2. Z **Základy**, zadejte hello následující informace:

    * **Název clusteru**: název hello hello clusteru HDInsight.
    * **Předplatné**: Vyberte předplatné toouse hello.
    * **Uživatelské jméno přihlášení clusteru** a **clusteru přihlašovacího hesla**: hello přihlášení při přístupu k hello clusteru přes protokol HTTPS. Tyto přihlašovací údaje služby tooaccess například hello webové uživatelské rozhraní Ambari nebo REST API používáte.
    * **Secure Shell (SSH) username**: hello přihlášení, které se používá při získávání hello clusteru prostřednictvím SSH. Ve výchozím nastavení je hello hello heslo stejné jako heslo pro přihlášení hello clusteru.
    * **Skupina prostředků**: hello prostředků skupiny toocreate hello clusteru v.
    * **Umístění**: hello oblast Azure toocreate hello clusteru v.
   
 ![Výběr předplatného](./media/hdinsight-apache-kafka-get-started/hdinsight-basic-configuration.png)

3. Vyberte **clusteru typ**, a pak sadu hello následující hodnoty z **konfigurace clusteru**:
   
    * **Typ clusteru:** Kafka

    * **Verze:** Kafka 0.10.0 (HDI 3.6)

    * **Úroveň clusteru:** Standard
     
 Nakonec použijte hello **vyberte** tlačítko toosave nastavení.
     
 ![Výběr typu clusteru](./media/hdinsight-apache-kafka-get-started/set-hdinsight-cluster-type.png)

4. Po výběru typu hello clusteru, použijte hello __vyberte__ tooset hello clusteru typ tlačítka. Pak pomocí hello __Další__ tlačítko toofinish základní konfigurace.

5. V části **Úložiště** vyberte nebo vytvořte účet úložiště. Hello kroky v tomto dokumentu ponechte hello ostatní pole na výchozí hodnoty hello. Použití hello __Další__ tlačítko toosave úložiště konfigurace.

    ![Nastavit hello nastavení účtu úložiště pro HDInsight](./media/hdinsight-apache-kafka-get-started/set-hdinsight-storage-account.png)

6. Z __aplikace (volitelné)__, vyberte __Další__ toocontinue. Pro tento příklad se nepožadují žádné aplikace.

7. Z __velikost clusteru__, vyberte __Další__ toocontinue.

    > [!WARNING]
    > dostupnost tooguarantee Kafka v HDInsight, cluster musí obsahovat aspoň tři uzly pracovního procesu.

    ![Sada hello Kafka velikost clusteru](./media/hdinsight-apache-kafka-get-started/kafka-cluster-size.png)

    > [!NOTE]
    > Hello **disků na pracovního uzlu** vstupní ovládací prvky hello škálovatelnost Kafka v HDInsight. Další informace najdete v tématu věnovaném [konfiguraci úložiště a škálovatelnosti Kafka v HDInsightu](hdinsight-apache-kafka-scalability.md).

8. Z __upřesňující nastavení__, vyberte __Další__ toocontinue.

9. Z hello **Souhrn**, zkontrolujte konfiguraci hello hello clusteru. Použití hello __upravit__ odkazy toochange všechna nastavení, která jsou nesprávné. Nakonec použijte the__Create__ tlačítko toocreate hello clusteru.
   
    ![Souhrn konfigurace clusteru](./media/hdinsight-apache-kafka-get-started/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > To může trvat až too20 minut toocreate hello clusteru.

## <a name="connect-toohello-cluster"></a>Připojte toohello cluster

> [!IMPORTANT]
> Při provádění hello následující kroky, je potřeba použít klienta SSH. Další informace najdete v tématu hello [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentu.

Z vašeho klienta pomocí protokolu SSH tooconnect toohello clusteru:

```ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net```

Nahraďte **SSHUSER** s uživatelským jménem SSH hello, které jste zadali při vytváření clusteru. Nahraďte **CLUSTERNAME** s názvem hello hello clusteru.

Po zobrazení výzvy zadejte heslo hello, která jste použili pro hello účtu SSH.

Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="getkafkainfo"></a>Získat informace o hostiteli hello Zookeeper a zprostředkovatele

Při práci s Kafka, musíte znát dvě hodnoty hostitele; Hello *Zookeeper* hostitelů a hello *zprostředkovatele* hostitele. Tito hostitelé se používají s hello Kafka rozhraní API a řadu hello nástroje, které jsou dodávány s Kafka.

Použijte následující proměnné prostředí toocreate kroky, které obsahují informace o hostiteli hello hello. Tyto proměnné prostředí se používají v hello kroky v tomto dokumentu.

1. Z clusteru toohello připojení SSH, použijte následující hello příkaz tooinstall hello `jq` nástroj. Tento nástroj je použité tooparse dokumentů JSON a je užitečný při načítání informací o hostiteli hello zprostředkovatele:
   
    ```bash
    sudo apt -y install jq
    ```

2. proměnné prostředí hello tooset s informacemi o načítají Ambari hello použijte následující příkazy:

    ```bash
    CLUSTERNAME='your cluster name'
    PASSWORD='your cluster password'
    export KAFKAZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    > [!IMPORTANT]
    > Nastavit `CLUSTERNAME=` toohello název hello Kafka clusteru. Nastavit `PASSWORD=` toohello heslo pro přihlášení (správce) můžete použít při vytváření clusteru hello.

    Hello následující text je příkladem hello obsah `$KAFKAZKHOSTS`:
   
    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`
   
    Hello následující text je příkladem hello obsah `$KAFKABROKERS`:
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

    > [!NOTE]
    > Hello `cut` příkaz je použité tootrim hello seznam záznamy hostitele tootwo hostitele. Při vytváření příjemce Kafka nebo proto, že nepotřebujete tooprovide hello úplný seznam hostitelů.
   
    > [!WARNING]
    > Nespoléhejte na hello vrácené informace z této relace tooalways být přesná. Pokud jste škálování hello clusteru, nové zprostředkovatelé přidání nebo odebrání. Pokud dojde k chybě a je nahrazený uzlu, může změnit název hostitele hello hello uzlu.
    >
    > Měli načítat informace hello Zookeeper a zprostředkovatele hostitelů krátce před použít tooensure máte platné informace.

## <a name="create-a-topic"></a>Vytvoření tématu

Kafka ukládá datové proudy do kategorií označovaných jako *témata*. SSH připojení tooa clusteru headnode můžete skript, který se Kafka toocreate téma:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```

Tento příkaz se připojí pomocí hello hostitele informace uložené v tooZookeeper `$KAFKAZKHOSTS`a poté vytvořit téma Kafka s názvem **testování**. Můžete ověřit, že tohoto tématu hello byla vytvořena pomocí hello následující skript toolist témata:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
```

Hello výstup tohoto příkazu obsahuje seznam Kafka témat, která obsahuje hello **testování** tématu.

## <a name="produce-and-consume-records"></a>Produkce a konzumace záznamů

Kafka ukládá *záznamy* v tématech. Záznamy jsou vytvářeny *producenty* a spotřebovávány *konzumenty*. Producenti načítají záznamy ze *zprostředkovatelů* Kafka. Každý pracovní uzel v clusteru HDInsight je zprostředkovatelem Kafka.

Použijte následující postup toostore záznamy do téma test hello jste vytvořili dříve a pak je pomocí příjemce číst hello:

1. Z relace SSH hello použijte skript, který se Kafka toowrite záznamy toohello tématu:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    Výzva toohello nebudou zobrazeny po tento příkaz. Místo toho zadejte několik textové zprávy a pak použijte **kombinaci kláves Ctrl + C** toostop odesílání toohello tématu. Každý řádek se odešle jako samostatný záznam.

2. Použijte skript, který se záznamy tooread Kafka hello tématu:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```
   
    Tento příkaz načte hello záznamů z tématu hello a zobrazí je. Pomocí `--from-beginning` informuje hello příjemce toostart od začátku hello hello datového proudu, proto jsou načteny všechny záznamy.

3. Použití __kombinaci kláves Ctrl + C__ toostop hello příjemce.

## <a name="producer-and-consumer-api"></a>Rozhraní API pro producenta a konzumenta

Můžete také programově vytvářet a využívat záznamů pomocí hello [rozhraní API Kafka](http://kafka.apache.org/documentation#api). toobuild producent Java a příjemce, použijte hello postupem z vývojového prostředí.

> [!IMPORTANT]
> Musíte mít hello následující součásti nainstalované ve vašem vývojovém prostředí:
>
> * [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) nebo ekvivalentní, například OpenJDK.
>
> * [Apache Maven](http://maven.apache.org/)
>
> * Klient SSH a hello `scp` příkaz. Další informace najdete v tématu hello [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentu.

1. Stáhnout hello příklady z [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started). Například hello producent/příjemce použití hello projektu v hello `Producer-Consumer` adresáře. Tento příklad obsahuje hello následující třídy:
   
    * **Spustit** -spustí hello příjemce nebo výrobce.

    * **Producent** -tématu toohello úložiště 1 000 000 záznamů.

    * **Příjemce** -čte záznamů z tématu hello.

2. umístění adresáře toohello hello změnit toocreate jar balíčku, `Producer-Consumer` hello adresáře a použijte následující příkaz:

    ```
    mvn clean package
    ```

    Tento příkaz vytvoří adresář s názvem `target`, který bude obsahovat soubor s názvem `kafka-producer-consumer-1.0-SNAPSHOT.jar`.

3. Použití hello následující příkazy toocopy hello `kafka-producer-consumer-1.0-SNAPSHOT.jar` clusteru HDInsight tooyour souboru:
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    Nahraďte **SSHUSER** hello uživatele SSH pro váš cluster a nahraďte **CLUSTERNAME** s hello názvem vašeho clusteru. Po zobrazení výzvy zadejte hello heslo pro uživatele SSH hello.

4. Jednou hello `scp` příkaz dokončení kopírování hello souboru, připojit toohello clusteru pomocí protokolu SSH. Použijte následující příkaz toowrite záznamy toohello test tématu hello:

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

5. Po dokončení procesu hello, použijte následující příkaz tooread hello tématu hello:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    Zobrazí se záznamy Hello číst, spolu s počtem záznamů. Může se zobrazit několik více než 1 000 000 protokolována jako jste poslali několik záznamů toohello tématu pomocí skriptu v jednom z předchozích kroků.

6. Použití __kombinaci kláves Ctrl + C__ tooexit hello příjemce.

### <a name="multiple-consumers"></a>Víc současných konzumentů

Konzumenti Kafka při čtení záznamů používají skupiny konzumentů. Pomocí více příjemců hello stejné skupiny má za následek zatížení vyvážit čtení z tématu. Každý příjemce ve skupině hello obdrží část hello záznamy. toosee, který tento proces v akci, hello použijte následující kroky:

1. Otevřete nový SSH relace toohello cluster tak, že máte dva z nich. V každé relaci použít hello následující toostart hello příjemce se stejným ID skupiny příjemců:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
    ```

    Tento příkaz spustí příjemce pomocí ID skupiny hello `mygroup`.

    > [!NOTE]
    > Používání příkazů hello v hello [získat informace o hostiteli hello Zookeeper a zprostředkovatele](#getkafkainfo) části tooset `$KAFKABROKERS` pro tuto relaci SSH.

2. Podívejte se na jako každou záznamy hello počty relace, který obdrží z tématu hello. Hello celkem obě relace by měl být hello stejné, jako jste dříve získali od jednoho příjemce.

Spotřeba klienty během hello stejnou skupinu je zpracováván prostřednictvím hello oddílů pro téma hello. Pro hello `test` tématu vytvořený, má osm oddíl. Pokud otevřete osm relace SSH a spouštět příjemce v všechny relace, každý příjemce čte záznamů z jednoho oddílu pro téma hello.

> [!IMPORTANT]
> Ve skupině příjemců nemůže být víc instancí konzumentů než má téma oddílů. V tomto příkladu jeden příjemce skupina může obsahovat až tooeight příjemci vzhledem k tomu, který je hello počet oddílů v tématu hello. Nebo můžete mít více skupin konzumentů, každou s maximálně osmi konzumenty.

Záznamy, které jsou uložené v Kafka se ukládají do hello pořadí, ve kterém jsou přijaty v rámci oddílu. tooachieve v seřazené doručení pro záznamy *v rámci oddílu*, vytvořte skupinu uživatelů, kde hello počet instancí příjemce odpovídá hello počet oddílů. tooachieve v seřazené doručení pro záznamy *v rámci tématu hello*, vytvořte skupinu uživatelů s instancí jenom jednoho příjemce.

## <a name="streaming-api"></a>API pro streamování

Hello streamování rozhraní API se přidal ve verzi 0.10.0; tooKafka starší verze spoléhají na Apache Spark nebo Storm pro zpracování datového proudu.

1. Pokud jste tak již neučinili, stáhněte si hello příklady z [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) tooyour vývojové prostředí. Pro hello streamování příklad, použití hello projektu v hello `streaming` adresáře.
   
    Tento projekt obsahuje pouze jednu třídu, `Stream`, který čte záznamů z hello `test` téma, které jste vytvořili dříve. Počty slov hello číst a vysílá každého slova a počet tooa tématu, s názvem `wordcounts`. Hello `wordcounts` tématu se vytvoří v pozdější fázi v této části.

2. Z příkazového řádku hello ve vašem vývojovém prostředí, změnit umístění adresáře toohello hello `Streaming` adresář a potom hello použijte následující příkaz toocreate jar balíčku:

    ```bash
    mvn clean package
    ```

    Tento příkaz vytvoří adresář s názvem `target`, který bude obsahovat soubor s názvem `kafka-streaming-1.0-SNAPSHOT.jar`.

3. Použití hello následující příkazy toocopy hello `kafka-streaming-1.0-SNAPSHOT.jar` clusteru HDInsight tooyour souboru:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    Nahraďte **SSHUSER** hello uživatele SSH pro váš cluster a nahraďte **CLUSTERNAME** s hello názvem vašeho clusteru. Po zobrazení výzvy zadejte hello heslo pro uživatele SSH hello.

4. Jednou hello `scp` příkaz dokončení kopírování hello souboru, připojit toohello clusteru pomocí protokolu SSH a pak použijte následující příkaz toocreate hello hello `wordcounts` tématu:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    ```

5. V dalším kroku spuštění hello vysílání datového proudu proces použitím hello následující příkaz:
   
    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS 2>/dev/null &
    ```
   
    Tento příkaz spustí hello streamování proces hello pozadí.

6. Použití hello následující příkaz toosend zprávy toohello `test` tématu. Tyto zprávy jsou zpracovávány hello streamování příklad:
   
    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS &>/dev/null &
    ```

7. Hello použijte následující příkaz tooview hello výstupu, který je zapsán toohello `wordcounts` téma podle hello streamování proces:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --from-beginning --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
    ```
   
    > [!NOTE]
    > tooview hello data, musíte upozornit hello příjemce tooprint hello klíč a hello deserializátor toouse pro hello klíč a hodnotu. Název klíče Hello je hello word a hodnota klíče hello obsahuje počet hello.
   
    Hello výstup je podobné toohello následující text:
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
        a       13805
        snow    13637
   
    > [!NOTE]
    > počet Hello zvýší pokaždé, když je zjištěna.

7. Použít hello __kombinaci kláves Ctrl + C__ tooexit hello příjemce, a pak použijte hello `fg` příkaz toobring hello streamování pozadí úloh back toohello popředí. Použití __kombinaci kláves Ctrl + C__ tooexit jej také.

## <a name="delete-hello-cluster"></a>Odstranění clusteru hello

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Řešení potíží

Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Další kroky

V tomto dokumentu jste se naučili základy hello pracovat s Apache Kafka v HDInsight. Použijte následující informace o práci s Kafka toolearn hello:

* [Zajištění vysoké dostupnosti dat s využitím Kafka ve službě HDInsight](hdinsight-apache-kafka-high-availability.md)
* [Zvýšení škálovatelnosti konfigurací spravovaných disků s využitím Kafka v HDInsightu](hdinsight-apache-kafka-scalability.md)
* [Dokumentace Apache Kafka](http://kafka.apache.org/documentation.html) na webu kafka.apache.org.
* [Použít MirrorMaker toocreate repliku Kafka v HDInsight](hdinsight-apache-kafka-mirroring.md)
* [Použití Apache Stormu se systémem Kafka ve službě HDInsight](hdinsight-apache-storm-with-kafka.md)
* [Použití Apache Sparku se systémem Kafka ve službě HDInsight](hdinsight-apache-spark-with-kafka.md)
* [Připojení tooKafka přes virtuální síť Azure](hdinsight-apache-kafka-connect-vpn-gateway.md)
