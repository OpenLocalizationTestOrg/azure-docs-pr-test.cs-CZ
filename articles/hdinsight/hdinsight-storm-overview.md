---
title: aaaWhat je Apache Storm - Azure HDInsight | Microsoft Docs
description: "Apache Storm můžete tooprocess datové proudy dat v reálném čase. Azure HDInsight vám umožní tooeasily vytvořit clustery Storm na hello cloudu Azure. Pomocí sady Visual Studio můžete vytvořit řešení Storm pomocí jazyka C# a pak nasadit tooyour, které clusterů HDInsight Storm."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "případy použití apache storm,cluster storm,co je apache storm"
ms.assetid: 72d54080-1e48-4a5e-aa50-cce4ffc85077
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: 6c6b2925ef3e5666dfecc3fb3c835bb362902c51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-apache-storm-on-azure-hdinsight"></a>Co je Apache Storm ve službě Azure HDInsight?

[Apache Storm](http://storm.apache.org/) je distribuovaný výpočetní systém typu open source a je odolný vůči poruchám. Můžete vytvořit datové proudy tooprocess Storm dat v reálném čase s Hadoop. Řešení Storm můžete také zajišťují garantované zpracování dat, s hello možnost tooreplay data, která nebyla úspěšně zpracována hello poprvé.

Storm v HDInsight poskytuje hello následující klíčové výhody:

* Funguje jako spravovaná služby se smlouvou SLA s 99,9% dostupností.

* Podporuje snadné přizpůsobení prostřednictvím spouštění skriptů v clusteru Storm během nebo po jeho vytvoření. Další informace obsahuje článek [Přizpůsobení clusterů HDInsight pomocí skriptových akcí](hdinsight-hadoop-customize-cluster-linux.md).

* Využívá různé jazyky. Můžete napsat komponenty Storm v jazyce hello podle vaší volby, například Java, C# a Python.

    * Visual Studio se integruje s HDInsight pro vývoj hello, Správa a monitorování topologií C#. Další informace najdete v tématu [topologie vývoj C# Storm pomocí nástrojů HDInsight pro Visual Studio hello](hdinsight-storm-develop-csharp-visual-studio-topology.md).

    * Podporuje hello Trident Java rozhraní. Můžete vytvářet topologie Storm, které podporují právě jedno zpracování zpráv, transakční trvalé uchovávání datového skladu a sadu běžných operací pro analýzy datového proudu.

*  Kapacita clusterů Storm se snadno vertikálně snižuje i zvyšuje. Můžete přidávat nebo odebírat uzly pracovního procesu s topologií Storm toorunning žádný dopad.

* Integruje se službou hello následující služby Azure:

    * Azure Event Hubs

    * Azure Virtual Network

    * Azure SQL Database

    * Azure Storage

    * Azure Cosmos DB

* Bezpečně kombinuje hello možností více clusterů HDInsight pomocí virtuální sítě. Můžete vytvářet analytické kanály, které používají clustery Storm, Kafka, Spark, HBase nebo Hadoop.

Seznam společností, které používají pro svá řešení pro analýzu v reálném čase Apache Storm, najdete v tématu [Společnosti využívající Apache Storm](https://storm.apache.org/documentation/Powered-By.html).

tooget začít používat Storm, najdete v části [Začínáme se Storm v HDInsight][gettingstarted].

## <a name="how-does-storm-work"></a>Jak funguje Storm

Storm spustí topologie místo hello úloh MapReduce, které je možné, že znáte. Topologie Storm se skládají z několika součástí, které jsou uspořádány do orientovaného acyklického grafu (DAG). Data proudí mezi součástmi hello v grafu hello. Každá komponenta spotřebovává jeden či více datových streamů a případně může i jeden či více streamů vysílat. Hello následující diagram znázorňuje tok dat mezi součástmi v topologii počtu slov základní:

![Příklad uspořádání součástí v topologii Storm](./media/hdinsight-storm-overview/example-apache-storm-topology-diagram.png)

* Součásti spout přenášejí data do topologie. Se generuje jeden nebo více datových proudů do topologie hello.

* Součásti bolt zpracovávají datové proudy vyslané ze spoutů nebo z jiných boltů. Funkce Bolts může volitelně emitování datových proudů do topologie hello. Funkce Bolts jsou také zodpovědné za zápis dat tooexternal služeb nebo úložiště, například HDFS, Kafka nebo HBase.

## <a name="ease-of-creation"></a>Snadné vytvoření

V HDInsight můžete zřídit nový cluster Storm během několika minut. Další informace týkající se vytvoření clusteru Storm naleznete v článku [Začínáme se Stormem v HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).

## <a name="ease-of-use"></a>Snadné používání

* __Secure Shell (SSH) připojení__: hello hlavních uzlech clusteru Storm můžete přístup přes hello Internet pomocí protokolu SSH. Příkazy můžete spouštět přímo v clusteru prostřednictvím SSH.

  Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* __Webové připojení__: všechny HDInsight clustery poskytují webovému uživatelskému rozhraní Ambari hello. Můžete snadno sledovat, konfigurovat a spravovat služby v clusteru pomocí webovému uživatelskému rozhraní Ambari hello. Clustery Storm také zajišťují hello uživatelské rozhraní Storm. Můžete monitorovat a správě spuštěných topologií Storm z prohlížeče pomocí hello uživatelské rozhraní Storm.

  Další informace najdete v tématu hello [spravovat HDInsight pomocí hello webové uživatelské rozhraní Ambari](hdinsight-hadoop-manage-ambari.md) a [monitorování a spravovat pomocí hello uživatelské rozhraní Storm](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-storm-ui) dokumenty.

* __Azure PowerShell a rozhraní příkazového řádku Azure__: prostředí PowerShell a rozhraní příkazového řádku obě poskytují nástroje příkazového řádku, které můžete použít z vašeho klienta systému toowork s HDInsight a jinými službami Azure.

* __Integrace aplikace Visual Studio__: nástrojů Azure Data Lake pro Visual Studio zahrnují šablony projektů pro vytvoření topologie C# Storm pomocí hello SCP.Net framework. Nástroje data Lake také poskytují nástroje toodeploy, sledovat a spravovat řešení se Storm v HDInsight.

  Další informace najdete v tématu [topologie vývoj C# Storm pomocí nástrojů HDInsight pro Visual Studio hello](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="integration-with-other-azure-services"></a>Integrace s dalšími službami Azure

* __Azure Data Lake Store__: Příklad použití Data Lake Store s clusterem Storm najdete v části [Používání Azure Data Lake Store pomocí Apache Storm v HDInsight](hdinsight-storm-write-data-lake-store.md).

* __Služba Event Hubs__: Příklad použití služby Event Hubs s clusteru Storm naleznete v části hello následující dokumenty:

    * [Vývoj topologie založené na jazyce Java pro Storm v HDInsight](hdinsight-storm-develop-java-topology.md)

    * [Zpracování událostí z Azure Event Hubs se Stormem v HDInsight (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md)

* __Databáze SQL__, __Cosmos DB__, __Event Hubs__, a __HBase__: Příklady šablony jsou součástí hello nástrojů Data Lake pro Visual Studio. Další informace najdete v článku [Vývoj topologií v jazyce C# pro Storm v HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="reliability"></a>Spolehlivost

Apache Storm zaručuje, že příchozí zprávy je vždy plně zpracovány, i když je analýza dat hello rozloženy stovky uzly.

uzel Nimbus Hello poskytuje toohello podobné funkce jako Hadoop JobTracker a přiřazuje úlohy tooother uzlů v clusteru prostřednictvím Zookeeper. Uzly zookeeper poskytují koordinaci pro cluster a usnadňují komunikace mezi Nimbus a hello nadřízeného proces na hello uzlů pracovního procesu. Pokud jednoho uzlu zpracování přestane fungovat, hello uzel Nimbus je informován a přiřadí hello úlohu a přidružená data tooanother uzlu.

Hello výchozí konfiguraci pro clustery Apache Storm je toohave pouze jeden uzel Nimbus. Storm v HDInsight poskytuje dva uzly Nimbus. V případě selhání primárního uzlu hello hello Storm cluster přepínačů toohello sekundárního uzlu při hello primární uzel se obnoví. Hello následující diagram znázorňuje konfiguraci toku hello úloh pro Storm v HDInsight:

![Graf nimbusu, zookeeper a supervisor](./media/hdinsight-storm-overview/nimbus.png)

## <a name="scale"></a>Měřítko

Kapacitu clusterů HDInsight je možné dynamicky měnit přidáváním nebo odebíráním pracovních uzlů. Tato operace může běžet i během zpracovávání dat.

> [!IMPORTANT]
> tootake využívat nové uzly přidávají prostřednictvím škálování, je nutné toorebalance Storm topologie spuštěné před zvýšením velikosti clusteru hello.

## <a name="support"></a>Podpora

Storm v HDInsight obsahuje nepřetržitou plnou podporu na úrovni rozlehlé sítě. Storm v HDInsight má také SLA 99,9 %. To znamená, že Zaručujeme, že má Storm cluster externí konektivitu minimálně 99,9 % času hello.

Další informace najdete v článku [Podpora Azure](https://azure.microsoft.com/support/options/).

## <a name="apache-storm-use-cases"></a>Případy použití Apache Storm

Hello tady jsou některé běžné scénáře, pro které může používat Storm v HDInsight:

* Internet věcí (IoT)
* Odhalování podvodů
* Sociální analýzy
* Extrakce, transformace a načítání (ETL)
* Monitorování sítě
* Hledání
* Mobile engagement

Informace o scénářích reálného světa najdete v tématu hello [jak společnosti využívají Storm](https://storm.apache.org/documentation/Powered-By.html) dokumentu.

## <a name="development"></a>Vývoj

Nástroje Data Lake Tools pro Visual Studio umožňují vývojářům .NET navrhovat a implementovat topologie v jazyce C#. Můžete také vytvářet hybridní topologie, které využívají součásti Java a C#.

Další informace najdete v článku [Vývoj topologií C# pro Storm v HDInsight pomocí sady Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Také můžete vyvíjet řešení Java pomocí hello IDE podle svého výběru. Další informace najdete v článku [Vývoj topologií v jazyce Java pro Storm v HDInsight](hdinsight-storm-develop-java-topology.md).

Součásti použité toodevelop Storm může být také Python. Další informace najdete v článku [Vývoj topologií Storm pomocí Pythonu v HDInsight](hdinsight-storm-develop-python-topology.md).

## <a name="common-development-patterns"></a>Běžné vývojové vzory

### <a name="guaranteed-message-processing"></a>Zaručené zpracování zprávy

Apache Storm můžete poskytovat různé úrovně zaručeného zpracování zprávy. Například základní aplikace Storm může zaručit alespoň jedno zpracování a Trident může zaručit přesně jedno zpracování.

Další informace naleznete v tématu [Záruky na zpracování dat](https://storm.apache.org/about/guarantees-data-processing.html) na webu apache.org.

### <a name="ibasicbolt"></a>IBasicBolt

Hello vzor čtení vstupního záznamu, generování nula nebo více řazené kolekce členů, a potom okamžitá hello vstupní řazené kolekce členů okamžitě na konci hello hello spusťte metoda je běžné. Storm poskytuje hello [IBasicBolt](https://storm.apache.org/releases/1.0.3/javadocs/org/apache/storm/topology/IBasicBolt.html) rozhraní tooautomate tohoto vzoru.

### <a name="joins"></a>Spojení

Způsob spojení datových proudů se v jednotlivých aplikacích liší. Můžete například spojovat jednotlivé řazené kolekce členů z různých datových proudů do jednoho nového datového proudu nebo můžete spojovat pouze dávky řazených kolekcí členů pro konkrétní okno. V obou případech můžete spojení provést pomocí příkazu [fieldsGrouping](http://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-). Pole seskupování je způsob definice způsobu směrovaný toobolts řazené kolekce členů.

V následujícím příkladu Java hello se fieldsGrouping použité tooroute řazené kolekce členů, které pocházejí z komponenty "1", "2" a "3" toohello MyJoiner bolt:

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batches"></a>Dávky

Apache Storm poskytuje interní mechanismus časování, který je znám jako odškrtávání řazených kolekcí členů. Můžete nastavit, jak často se odškrtávání řazených kolekcí členů ve vaší topologii vysílá.

Příklad použití odškrtávaní řazených kolekcí členů v součásti v C# najdete v souboru [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java).

### <a name="caches"></a>Caches

Ukládání do mezipaměti se často používá jako mechanismus pro urychlení zpracování, protože udržuje často používané prostředky v paměti. Protože se topologie distribuuje mezi více uzlů a v rámci každého uzlu se distribuuje více procesů, měli byste zvážit použití příkazu [fieldsGrouping](http://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-). Použití `fieldsGrouping` tooensure, že záznamy obsahující pole hello, které se používají pro vyhledávání v mezipaměti jsou vždy směrované toohello stejný proces. Díky této funkci seskupování předejdete duplikaci položek mezipaměti napříč procesy.

### <a name="stream-top-n"></a>Datový proud „horních N“

Pokud vaše topologie závisí na výpočtu hodnoty hlavních, vypočítá hello nejvyšší hodnotu N paralelně. Pak sloučit výstup z těchto výpočtů do globální hodnoty hello. Tuto operaci lze provést pomocí [fieldsGrouping](http://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) tooroute podle pole pro paralelní zpracování. Potom můžete směrovat tooa bolt, které globálně určují hodnotu hlavních hello.

Příklad výpočtu hodnoty hlavních, naleznete v části hello [RollingTopWords](https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/org/apache/storm/starter/RollingTopWords.java) příklad.

## <a name="logging"></a>Protokolování

Storm používá Apache Log4j toolog informace. Ve výchozím nastavení je zaznamenána velké množství dat a může být obtížné toosort prostřednictvím hello informace. Jako součást vaší chování při protokolování toocontrol topologie Storm můžete zahrnout soubor konfigurace protokolování.

Pro ukázkové topologie, které ukazuje, jak zjistit, tooconfigure protokolování [založené na jazyce Java WordCount](hdinsight-storm-develop-java-topology.md) příklad Storm v HDInsight.

## <a name="next-steps"></a>Další kroky

Další informace o řešení pro analýzu v reálném čase se Stormem v HDInsight:

* [Začínáme s Apache Storm v HDInsight][gettingstarted]
* [Příklad topologií pro Apache Storm v HDInsight](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md
