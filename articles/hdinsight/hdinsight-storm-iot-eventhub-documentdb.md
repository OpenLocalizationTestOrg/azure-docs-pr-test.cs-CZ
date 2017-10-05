---
title: "Zpracování dat snímačů vehicle s Apache Storm v HDInsight | Microsoft Docs"
description: "Zjistěte, jak ke zpracování dat snímačů vehicle ze služby Event Hubs pomocí Apache Storm v HDInsight. Přidání modelu dat z databáze Cosmos Azure a ukládání výstupu do úložiště."
services: hdinsight,documentdb,notification-hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 78980635-8bef-4c33-96c3-fae50e932e31
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/03/2017
ms.author: larryfr
ms.openlocfilehash: 8e8ebc724e1c70e8fcd56312adef5da2342373ea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Zpracování dat snímačů vehicle z Azure Event Hubs pomocí Apache Storm v HDInsight

Zjistěte, jak zpracovat vehicle data snímačů z Azure Event Hubs pomocí Apache Storm v HDInsight. Tento příklad čte data snímačů z Azure Event Hubs, vylepšuje data pod položkou data uložená v Azure Cosmos DB. Data jsou uložena do Azure Storage pomocí Hadoop File System (HDFS).

![HDInsight a diagram architektury Internet věcí (IoT)](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

## <a name="overview"></a>Přehled

Přidání snímače do vozidel umožňuje předvídat problémy zařízení na základě trendů historická data. Také umožňuje vylepšení na budoucí verze na základě analýzy využití vzoru. Musí umět rychle a efektivně načíst data ze všech vozidel do Hadoop než dojde k zpracování prostředí MapReduce. Kromě toho můžete provést analýzu cest kritické chybě (teplota motoru, brzdy atd.) v reálném čase.

Azure Event Hubs je vytvořená tak, aby zpracovávat obrovské objemu dat vygenerovaných senzory. Apache Storm slouží k načtení a zpracování dat před jejich uložením do HDFS.

## <a name="solution"></a>Řešení

Telemetrická data pro modul teploty, o teplotě a rychlosti se zaznamenává podle senzorů. Data se pak posílají do centra událostí spolu s car Vehicle identifikační číslo (VIN) a časové razítko. Z tohoto místa topologie Storm Apache Storm v clusteru HDInsight se systémem čte data, procesy a ukládá je do HDFS.

Během zpracování VIN umožňuje načíst informace o modelu z databáze Cosmos. Tato data se přidá do datového proudu, než je uložen.

Komponenty používané v topologii Storm jsou:

* **EventHubSpout** -čte data z Azure Event Hubs
* **TypeConversionBolt** – převede text JSON ze služby Event Hubs řazené kolekce členů obsahující následující data snímače:
    * Modul teploty
    * O teplotě
    * Rychlost
    * VIN
    * časové razítko
* **DataReferencBolt** -vyhledává vehicle model z databáze Cosmos pomocí VIN
* **WasbStoreBolt** – ukládá data do HDFS (Azure Storage)

Na následujícím obrázku je diagram tohoto řešení:

![Topologie Storm](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

## <a name="implementation"></a>Implementace

Dokončení, automatizovaná řešení pro tento scénář je k dispozici jako součást [HDInsight-Storm-příklady](https://github.com/hdinsight/hdinsight-storm-examples) úložišti na Githubu. Tento příklad, postupujte podle kroků v [IoTExample README. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Další kroky

Další příklad topologií Storm, najdete v části [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md).

