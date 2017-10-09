---
title: "aaaProcess události ze služby Event Hubs se Storm - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak tooprocess data z Azure Event Hubs s topologií C# Storm vytvořena v sadě Visual Studio pomocí hello nástroje HDInsight pro Visual Studio."
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 67f9d08c-eea0-401b-952b-db765655dad0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: 30cd910d80eba066f283197bcbbaf11145bc5524
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-c"></a>Zpracování událostí z Azure Event Hubs se Storm v HDInsight (C#)

Zjistěte, jak toowork s Azure Event Hubs z Apache Storm v HDInsight. Tento dokument používá C# Storm topologie tooread a zápisu dat z centra Evbent

> [!NOTE]
> Java verzi tohoto projektu, naleznete v části [zpracovat události z Azure Event Hubs se Storm v HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).

## <a name="scpnet"></a>SCP.NET

Hello kroky v tomto dokumentu používají SCP.NET balíčku NuGet, který umožňuje snadno toocreate C# topologií a součásti pro použití s nástrojem Storm v HDInsight.

> [!IMPORTANT]
> Při hello kroky v tomto dokumentu spoléhají na vývojového prostředí systému Windows pomocí sady Visual Studio, zkompilovaný projekt hello může být odeslaná tooa Storm v clusteru HDInsight, který používá Linux. Clustery se systémem Linux vytvořené po 28 říjnu 2016 se podporují jenom SCP.NET topologie.

HDInsight 3.4 a větší použití Mono toorun C# topologií. spolupracuje se službou HDInsight 3.6 Hello příkladu v tomto dokumentu. Pokud máte v plánu na vytváření řešení .NET pro HDInsight, zkontrolujte hello [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentu potenciální nekompatibility.

### <a name="cluster-versioning"></a>Správa verzí clusteru

Hello balíček Microsoft.SCP.Net.SDK NuGet, které používáte pro svůj projekt musí odpovídat hello hlavní verzi Storm v HDInsight nainstalována. HDInsight verze 3.5 a 3.6 používat Storm 1.x, musíte použít verzi 1.0.x.x SCP.NET s těchto clusterů.

> [!IMPORTANT]
> Příklad Hello v tomto dokumentu očekává HDInsight 3.5 nebo 3,6 clusteru.
>
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

Topologie C# musí být také .NET 4.5.

## <a name="how-toowork-with-event-hubs"></a>Jak toowork službou Event Hubs

Společnost Microsoft poskytuje sadu komponent v jazyce Java, které se dají použít toocommunicate službou Event Hubs od topologie Storm. Můžete najít hello Java archivu (JAR) soubor, který obsahuje HDInsight 3.6 kompatibilní verze těchto součástí v [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).

> [!IMPORTANT]
> Při hello součásti jsou napsané v jazyce Java, je můžete snadno použít z topologie C#.

v tomto příkladu se používají Hello následující součásti:

* __EventHubSpout__: čte data ze služby Event Hubs.
* __EventHubBolt__: zapíše tooEvent datového centra.
* __EventHubSpoutConfig__: používá tooconfigure EventHubSpout.
* __EventHubBoltConfig__: používá tooconfigure EventHubBolt.

### <a name="example-spout-usage"></a>Příklad použití funkcí spout

SCP.NET poskytuje metody pro přidání EventHubSpout tooyour topologie. Tyto metody umožňují snadnější tooadd spout než použití hello obecné metody pro přidání součásti Java. Hello následující příklad ukazuje, jak toocreate spout pomocí hello __SetEventHubSpout__ a **EventHubSpoutConfig** metody poskytované SCP.NET:

```csharp
 topologyBuilder.SetEventHubSpout(
    "EventHubSpout",
    new EventHubSpoutConfig(
        ConfigurationManager.AppSettings["EventHubSharedAccessKeyName"],
        ConfigurationManager.AppSettings["EventHubSharedAccessKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        ConfigurationManager.AppSettings["EventHubEntityPath"],
        eventHubPartitions),
    eventHubPartitions);
```

Hello předchozí příklad vytvoří novou funkcí spout součást s názvem __EventHubSpout__a nakonfiguruje ho toocommunicate pomocí centra událostí. Hello paralelismus nápovědu pro součást hello se nastavuje toohello počet oddílů v Centru událostí hello. Toto nastavení umožňuje Storm toocreate instance hello součásti pro každý oddíl.

### <a name="example-bolt-usage"></a>Příklad použití bolt

Použití hello **JavaComponmentConstructor** toocreate metoda instanci hello bolt. Hello následující příklad ukazuje, jak toocreate a nakonfigurujte novou instanci třídy hello **EventHubBolt**:

```csharp
// Java construcvtor for hello Event Hub Bolt
JavaComponentConstructor constructor = JavaComponentConstructor.CreateFromClojureExpr(
    String.Format(@"(org.apache.storm.eventhubs.bolt.EventHubBolt. (org.apache.storm.eventhubs.bolt.EventHubBoltConfig. " +
        @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
        ConfigurationManager.AppSettings["EventHubPolicyName"],
        ConfigurationManager.AppSettings["EventHubPolicyKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        "servicebus.windows.net",
        ConfigurationManager.AppSettings["EventHubName"],
        "true"));

// Set hello bolt toosubscribe toodata from hello spout
topologyBuilder.SetJavaBolt(
    "eventhubbolt",
    constructor,
    partitionCount)
        .shuffleGrouping("Spout");
```

> [!NOTE]
> Tento příklad používá Clojure výrazu předaného jako řetězec, místo použití **JavaComponentConstructor** toocreate **EventHubBoltConfig**, jako příklad spout hello. Buď metoda funguje. Použijte hello metodu, která funguje nejlepší tooyou.

## <a name="download-hello-completed-project"></a>Stáhněte si projekt hello byla dokončena

Můžete stáhnout úplnou verzi vytvořených v tomto kurzu z projektů hello [Githubu](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub). Však stále potřebujete tooprovide konfigurační nastavení pomocí hello postupu v tomto kurzu.

### <a name="prerequisites"></a>Požadavky

* [Apache Storm v HDInsight clusteru verze 3.5 nebo 3.6](hdinsight-apache-storm-tutorial-get-started.md).

    > [!WARNING]
    > Příklad Hello v tomto dokumentu vyžaduje Storm v HDInsight verze 3.5 nebo 3.6. To nefunguje se staršími verzemi HDInsight, z důvodu změny názvu třídy toobreaking. Verzi tohoto příkladu, který funguje s starší clustery, naleznete v části [Githubu](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).

* [Centra událostí Azure](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).

* Hello [Azure .NET SDK](http://azure.microsoft.com/downloads/).

* Hello [nástroje HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

* Java JDK 1.8 nebo později na vašem vývojovém prostředí. Stahování JDK jsou k dispozici na [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

  * Hello **JAVA_HOME** prostředí proměnné musí bodu toohello adresář, který obsahuje Java.
  * Hello **%JAVA_HOME%/bin** adresář se musí nacházet v cestě hello.

## <a name="download-hello-event-hubs-components"></a>Stažení komponent hello Event Hubs

Stažení hello Event Hubs spout a funkce bolt z komponenty [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).

Vytvořte adresář s názvem `eventhubspout`a uložte soubor hello do adresáře hello.

## <a name="configure-event-hubs"></a>Konfigurace služby Event Hubs

Event Hubs je hello zdroj dat pro tento příklad. Použijte hello informace v části "Vytvoření centra událostí" hello z [Začínáme se službou Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).

1. Po vytvoření centra událostí hello zobrazit hello **EventHub** okno v hello Azure portálu a vyberte možnost **zásady sdíleného přístupu**. Vyberte **+ přidat** tooadd hello následující zásady:

   | Name (Název) | Oprávnění |
   | --- | --- |
   | Modul pro zápis |Odeslat |
   | Čtecí modul |Naslouchání |

    ![Okno zásady přístupu – snímek obrazovky sdílené složky](./media/hdinsight-storm-develop-csharp-event-hub-topology/sas.png)

2. Vyberte hello **čtečky** a **zapisovače** zásady. Zkopírujte a uložte hodnotu primárního klíče hello pro obě zásady, jako jsou tyto hodnoty použít později.

## <a name="configure-hello-eventhubwriter"></a>Konfigurace hello EventHubWriter

1. Pokud jste ještě nenainstalovali hello nejnovější verzi hello nástroje HDInsight pro Visual Studio, najdete v části [Začínáme pomocí nástrojů HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Stáhněte si řešení hello z [eventhub. storm hybridní](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).

3. V hello **EventHubWriter** projekt, otevřete hello **App.config** souboru. Pomocí informací hello z centra událostí hello nakonfigurované starší toofill v hello hodnotu hello následující klíče:

   | Klíč | Hodnota |
   | --- | --- |
   | EventHubPolicyName |Zapisovač (Pokud jste použili jiný název pro zásady hello s *odeslat* oprávnění, použijte místo toho.) |
   | EventHubPolicyKey |Hello klíč pro zásady zapisovače hello. |
   | EventHubNamespace |Hello obor názvů, který obsahuje Centrum událostí. |
   | EventHubName |Název centra událostí. |
   | EventHubPartitionCount |Hello počet oddílů v Centru událostí. |

4. Uložte a zavřete hello **App.config** souboru.

## <a name="configure-hello-eventhubreader"></a>Konfigurace hello EventHubReader

1. Otevřete hello **EventHubReader** projektu.

2. Otevřete hello **App.config** souboru hello **EventHubReader**. Pomocí informací hello z centra událostí hello nakonfigurované starší toofill v hello hodnotu hello následující klíče:

   | Klíč | Hodnota |
   | --- | --- |
   | EventHubPolicyName |Čtečka (Pokud jste použili jiný název pro zásady hello s *naslouchání* oprávnění, použijte místo toho.) |
   | EventHubPolicyKey |Hello klíč pro zásady čtečky hello. |
   | EventHubNamespace |Hello obor názvů, který obsahuje Centrum událostí. |
   | EventHubName |Název centra událostí. |
   | EventHubPartitionCount |Hello počet oddílů v Centru událostí. |

3. Uložte a zavřete hello **App.config** souboru.

## <a name="deploy-hello-topologies"></a>Hello topologie nasazení

1. Z **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **EventHubReader** projektu a vyberte **odeslání tooStorm v HDInsight**.

    ![Snímek obrazovky řešení Explorer, se tooStorm odeslání v HDInsight zvýrazněná](./media/hdinsight-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. Na hello **odeslání topologie** dialogové okno, vyberte vaše **Storm Cluster**. Rozbalte položku **další konfigurace**, vyberte **cesty k souborům Java**, vyberte **...** a vyberte hello adresář, který obsahuje soubor JAR hello, který jste předtím stáhli. Nakonec klikněte na **odeslání**.

    ![Dialogové okno snímek obrazovky odeslání topologie](./media/hdinsight-storm-develop-csharp-event-hub-topology/submit.png)

3. Při odeslání hello topologie hello **prohlížeč topologie Storm** se zobrazí. tooview informací o topologii hello, vyberte hello **EventHubReader** topologie v levém podokně hello.

    ![Snímek obrazovky prohlížeč topologie Storm](./media/hdinsight-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. Z **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **EventHubWriter** projektu a vyberte **odeslání tooStorm v HDInsight**.

5. Na hello **odeslání topologie** dialogové okno, vyberte vaše **Storm Cluster**. Rozbalte položku **další konfigurace**, vyberte **cesty k souborům Java**, vyberte **...** , a vyberte hello adresář, který obsahuje soubor JAR hello jste předtím stáhli. Nakonec klikněte na **odeslání**.

6. Pokud byla odeslána hello topologie, aktualizujte seznam topologie hello v hello **prohlížeč topologie Storm** tooverify obou topologií spuštěných v clusteru hello.

7. V **prohlížeč topologie Storm**, vyberte hello **EventHubReader** topologie.

8. součást hello tooopen shrnutí hello bolt, dvakrát klikněte na hello **LogBolt** součásti v diagramu hello.

9. V hello **vykonavatelů** vyberte jeden z odkazů hello v hello **Port** sloupce. Zobrazí se informace protokolována komponentou hello. Hello protokolovat informace jsou podobné toohello následující text:

        2017-03-02 14:51:29.255 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,255 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1830978598,"deviceId":"8566ccbc-034d-45db-883d-d8a31f34068e"}
        2017-03-02 14:51:29.283 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,283 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1756413275,"deviceId":"647a5eff-823d-482f-a8b4-b95b35ae570b"}
        2017-03-02 14:51:29.313 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,312 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1108478910,"deviceId":"206a68fa-8264-4d61-9100-bfdb68ee8f0a"}

## <a name="stop-hello-topologies"></a>Zastavení topologie hello

topologie hello toostop, vyberte každý topologii v hello **prohlížeč topologie Storm**, pak klikněte na tlačítko **Kill**.

![Snímek obrazovky systému Storm topologie prohlížeč, s tlačítkem Kill](./media/hdinsight-storm-develop-csharp-event-hub-topology/killtopology.png)

## <a name="delete-your-cluster"></a>Odstranění clusteru

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Další kroky

V tomto dokumentu jste zjistili, jak toouse hello Java Event Hubs spout a funkce bolt z C# topologie toowork s daty v Azure Event Hubs. toolearn Další informace o vytváření topologie C#, najdete v části hello následující:

* [Vývoj topologie C# pro Apache Storm v HDInsight pomocí sady Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [Průvodce programováním spojovací bod služby](hdinsight-storm-scp-programming-guide.md)
* [Příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)
