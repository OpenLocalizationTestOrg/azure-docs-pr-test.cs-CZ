---
title: "aaaExample topologií Apache Storm v HDInsight | Microsoft Docs"
description: "Seznam příkladů topologie Storm vytvořit a otestovat s Apache Storm v HDInsight, včetně základní topologie C# a Java a práci s Event Hubs."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f9b1bdff-5928-4705-a76d-52fd200917cb
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.openlocfilehash: b20299112f6489b7c99360f0a539fc732703c64a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="example-storm-topologies-and-components-for-apache-storm-on-hdinsight"></a>Příklad topologií Storm a součásti pro Apache Storm v HDInsight

Hello následuje seznam příkladů vytvořen a spravován společností Microsoft pro použití s Apache Storm v HDInsight. Tyto příklady zahrnují různá témata z vytvoření základní C# a tooworking topologie Java pomocí služby Azure, například služby Event Hubs, Cosmos DB, Power BI, databáze SQL, databáze HBase v HDInsight a Azure Storage. Některé příklady také ukazují, jak toowork s technologií mimo Azure, nebo i jiných společností než Microsoft, například SignalR a Socket.IO.

| Popis | Demonstruje | Jazyk nebo Framework |
|:--- |:--- |:--- |
| [Zápis tooAzure Data Lake Store z Apache Storm](hdinsight-storm-write-data-lake-store.md) |Zápis tooAzure Data Lake Store |Java |
| [Event Hub Spout a Bolt zdroj](https://github.com/apache/storm/tree/master/external/storm-eventhubs) |Zdroj pro hello Event Hub Spout a Bolt |Java |
| [Vyvíjet topologie založené na jazyce Java pro Apache Storm v HDInsight][5797064f] |Maven |Java |
| [Vývoj topologie C# pro Apache Storm v HDInsight pomocí sady Visual Studio][16fce2d1] |Nástroje HDInsight pro Visual Studio |C#, Java |
| [Vytvoření více datových proudů v topologie C# Storm][ec5a4064] |Víc datových proudů |C# |
| [Zpracování událostí z Azure Event Hubs se Storm v HDInsight (C#)][844d1d81] |Event Hubs |C# a Java |
| [Zpracování událostí z Azure Event Hubs se Storm v HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md) |Event Hubs |Java |
| [Použít data toovisualize Power Bi od topologie Storm][94d15238] |Power BI |C# |
| [Analýza dat snímače pomocí Storm a HBase v HDInsight][ab894747] |Event Hubs, HBase, Socket.IO, řídicího panelu webové |C#, Java, JavaScript, HTML |
| [Zpracování dat snímačů vehicle ze služby Event Hubs pomocí Storm v HDInsight][246ee964] |Události rozbočovače, Cosmos DB úložiště Azure Blob (WASB) |C#, Java |
| [Extrakce, transformace a načítání (ETL) z Azure Event Hubs tooHBase pomocí Storm v HDInsight][b4b68194] |Služba Event Hubs, HBase |C# |
| [Šablona projektu topologie C# Storm pro práci se službami Azure z Storm v HDInsight][ce0c02a2] |Event Hubs, Cosmos databáze, SQL databáze, HBase, SignalR |C#, Java |
| [Škálovatelnost srovnávacích testů pro čtení z Azure Event Hubs pomocí Storm v HDInsight][d6c540e3] |Propustnost zpráv, Event Hubs, databáze SQL |C#, Java |
| [Korelovat události pomocí nástrojů Storm a HBase v HDInsight](hdinsight-storm-correlation-topology.md) |HBase |C# |
| [Používat jazyk Python se Storm v HDInsight](hdinsight-storm-develop-python-topology.md) |Součásti Python s topologií tok |Python |
| [Kafka pomocí Storm v HDInsight](hdinsight-apache-storm-with-kafka.md) | Apache Storm čtení a zápis tooApache Kafka | Java |

### <a name="next-steps"></a>Další kroky

* [Začínáme s Apache Storm v HDInsight][2b8c3488]
* [Zjistěte, jak toodeploy a správa topologií Storm se Storm v HDInsight][6eb0d3b8]

[2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Zjistěte, jak toocreate Storm v clusteru HDInsight a používání hello řídicí panel Storm toodeploy příklad topologií."
[6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Zjistěte, jak toodeploy a Správa topologie pomocí hello webové řídicí panel Storm a uživatelské rozhraní Storm nebo hello nástroje HDInsight pro Visual Studio."
[16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Zjistěte, jak hello toocreate topologie C# Storm pomocí nástrojů HDInsight pro Visual Studio."
[5797064f]: hdinsight-storm-develop-java-topology.md "Zjistěte, jak toocreate topologie Storm v jazyce Java, pomocí nástroje Maven, vytvořením topologie základní wordcount."
[94d15238]: hdinsight-storm-power-bi-topology.md "Ukazuje, jak toowrite data tooPower BI z topologie C#, pak vytvořit graf a řídicí panel z dat hello."
[ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Ukazuje základní topologie Storm, který provádí wordcount, implementované v jazyce C#. To také ukazuje, jak toocreate víc datových proudů v rámci topologie C#."
[844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Zjistěte, jak tooread a zápis dat z Azure Event Hubs se Storm v HDInsight."
[ab894747]: hdinsight-storm-sensor-data-analysis.md "Zjistěte, jak pomocí D3.js vizualizovat toouse Apache Storm v HDInsight tooprocess data snímačů z Azure Event Hubs a (volitelně), uložte ho tooHBase."
[246ee964]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md "Zjistěte, jak toouse zprávy tooread topologie Storm z Azure Event Hubs, přečtěte si dokumenty z Azure Cosmos databáze pro odkazování na data a uložit data tooAzure úložiště."
[d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Několik topologií toodemonstrate propustnost při čtení z Azure Event Hubs a ukládání tooSQL databáze pomocí Apache Storm v HDInsight."
[b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Zjistěte, jak tooread dat z Azure Event Hubs, agregace & transformace hello dat a potom uložte ho tooHBase v HDInsight."
[ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Tento projekt obsahuje šablony pro toointeract funkcích spouts, funkce bolts a topologie s různými službami Azure jako Event Hubs, Cosmos databáze a databáze SQL."

