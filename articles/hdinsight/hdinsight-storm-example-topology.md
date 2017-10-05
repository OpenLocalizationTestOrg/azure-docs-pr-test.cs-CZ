---
title: "Příklad topologií Apache Storm v HDInsight | Microsoft Docs"
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
ms.openlocfilehash: 71482e24e519319d506d61e0582420e35f1cae70
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="example-storm-topologies-and-components-for-apache-storm-on-hdinsight"></a>Příklad topologií Storm a součásti pro Apache Storm v HDInsight

Následuje seznam příkladů vytvořen a spravován společností Microsoft pro použití s Apache Storm v HDInsight. Tyto příklady zahrnují různá témata z vytváření základní C# a Java topologie spolupracuje se službami Azure, například služby Event Hubs, Cosmos DB, Power BI, databáze SQL, databáze HBase v HDInsight a Azure Storage. Některé příklady ukazují, jak pracovat s technologií mimo Azure, nebo i jiných společností než Microsoft, například SignalR a Socket.IO.

| Popis | Demonstruje | Jazyk nebo Framework |
|:--- |:--- |:--- |
| [Zápis do Azure Data Lake Store z Apache Storm](hdinsight-storm-write-data-lake-store.md) |Zápis do Azure Data Lake Store |Java |
| [Event Hub Spout a Bolt zdroj](https://github.com/apache/storm/tree/master/external/storm-eventhubs) |Zdroj pro Event Hub Spout a Bolt |Java |
| [Vyvíjet topologie založené na jazyce Java pro Apache Storm v HDInsight][5797064f] |Maven |Java |
| [Vývoj topologie C# pro Apache Storm v HDInsight pomocí sady Visual Studio][16fce2d1] |Nástroje HDInsight pro Visual Studio |C#, Java |
| [Vytvoření více datových proudů v topologie C# Storm][ec5a4064] |Víc datových proudů |C# |
| [Zpracování událostí z Azure Event Hubs se Storm v HDInsight (C#)][844d1d81] |Event Hubs |C# a Java |
| [Zpracování událostí z Azure Event Hubs se Storm v HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md) |Event Hubs |Java |
| [Pomocí Power Bi vizualizovat data z topologie Storm][94d15238] |Power BI |C# |
| [Analýza dat snímače pomocí Storm a HBase v HDInsight][ab894747] |Event Hubs, HBase, Socket.IO, řídicího panelu webové |C#, Java, JavaScript, HTML |
| [Zpracování dat snímačů vehicle ze služby Event Hubs pomocí Storm v HDInsight][246ee964] |Události rozbočovače, Cosmos DB úložiště Azure Blob (WASB) |C#, Java |
| [Extrakce, transformace a načítání (ETL) z Azure Event Hubs k HBase, používat Storm v HDInsight][b4b68194] |Služba Event Hubs, HBase |C# |
| [Šablona projektu topologie C# Storm pro práci se službami Azure z Storm v HDInsight][ce0c02a2] |Event Hubs, Cosmos databáze, SQL databáze, HBase, SignalR |C#, Java |
| [Škálovatelnost srovnávacích testů pro čtení z Azure Event Hubs pomocí Storm v HDInsight][d6c540e3] |Propustnost zpráv, Event Hubs, databáze SQL |C#, Java |
| [Korelovat události pomocí nástrojů Storm a HBase v HDInsight](hdinsight-storm-correlation-topology.md) |HBase |C# |
| [Používat jazyk Python se Storm v HDInsight](hdinsight-storm-develop-python-topology.md) |Součásti Python s topologií tok |Python |
| [Kafka pomocí Storm v HDInsight](hdinsight-apache-storm-with-kafka.md) | Apache Storm čtení a zápis do Apache Kafka | Java |

### <a name="next-steps"></a>Další kroky

* [Začínáme s Apache Storm v HDInsight][2b8c3488]
* [Zjistěte, jak nasadit a spravovat topologie Storm se Storm v HDInsight][6eb0d3b8]

[2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Naučte se vytvářet Storm na clusteru HDInsight a používat ovládací panel Storm k nasazení příklad topologií."
[6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Informace o nasazení a Správa topologie pomocí řídicího panelu webové Storm a uživatelské rozhraní Storm nebo nástroje HDInsight pro Visual Studio."
[16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Naučte se vytvářet topologie C# Storm pomocí nástrojů HDInsight pro Visual Studio."
[5797064f]: hdinsight-storm-develop-java-topology.md "Zjistěte, jak k vytváření topologie Storm v jazyce Java, pomocí nástroje Maven, vytvořením topologie základní wordcount."
[94d15238]: hdinsight-storm-power-bi-topology.md "Ukazuje, jak zapsat data do Power BI od topologie C# a pak z dat vytvořit graf a řídicí panel."
[ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Ukazuje základní topologie Storm, který provádí wordcount, implementované v jazyce C#. To také ukazuje, jak vytvořit více datových proudů v rámci topologie C#."
[844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Zjistěte, jak číst a zapisovat data z Azure Event Hubs se Storm v HDInsight."
[ab894747]: hdinsight-storm-sensor-data-analysis.md "Další informace o použití Apache Storm v HDInsight pro zpracování dat snímačů z Azure Event Hubs, vizualizovat pomocí D3.js a (volitelně), uložte ho do HBase."
[246ee964]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md "Naučte se používat topologie Storm ke čtení zpráv z Azure Event Hubs, přečtěte si dokumenty z Azure Cosmos databáze pro odkazování na data a uložení dat do služby Azure Storage."
[d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Několik topologií k předvedení propustnost při čtení ze služby Azure Event Hubs a ukládání do databáze SQL pomocí Apache Storm v HDInsight."
[b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Zjistěte, jak číst data z Azure Event Hubs, což je agregační & transformaci dat a pak jej uložte do HBase v HDInsight."
[ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Tento projekt obsahuje šablony pro funkcích spouts, funkce bolts a topologie pro interakci s různými službami Azure jako Event Hubs, Cosmos databáze a databáze SQL."

