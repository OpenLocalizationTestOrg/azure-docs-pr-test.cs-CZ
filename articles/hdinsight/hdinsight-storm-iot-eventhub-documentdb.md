---
title: "data snímače vehicle aaaProcess s Apache Storm v HDInsight | Microsoft Docs"
description: "Zjistěte, jak tooprocess vehicle senzor, data ze služby Event Hubs pomocí Apache Storm v HDInsight. Přidání modelu dat z databáze Cosmos Azure a ukládání výstupu toostorage."
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
ms.openlocfilehash: 8f7b1dbb9072e095ea32160bb731bedd071288af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Zpracování dat snímačů vehicle z Azure Event Hubs pomocí Apache Storm v HDInsight

Zjistěte, jak tooprocess vehicle data snímačů z Azure Event Hubs pomocí Apache Storm v HDInsight. Tento příklad čte data snímačů z Azure Event Hubs, vylepšuje hello data pod položkou data uložená v Azure Cosmos DB. Hello data se ukládají do Azure Storage pomocí hello Hadoop File System (HDFS).

![HDInsight a hello diagram architektury Internet věcí (IoT)](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

## <a name="overview"></a>Přehled

Přidání senzorů toovehicles umožňuje toopredict vybavení problémů na základě trendů historická data. Umožňuje také toomake vylepšení toofuture verze na základě analýzy využití vzoru. Musí být schopný tooquickly a efektivně načtení hello dat ze všech vozidel do Hadoop než dojde k zpracování prostředí MapReduce. Kromě toho můžete toodo analýza cest kritické chybě (teplota motoru, brzdy atd.) v reálném čase.

Azure Event Hubs je sestaveno toohandle hello masivnímu objemu dat vygenerovaných senzory. Apache Storm je možné použít tooload a zpracování dat hello před jejich uložením do HDFS.

## <a name="solution"></a>Řešení

Telemetrická data pro modul teploty, o teplotě a rychlosti se zaznamenává podle senzorů. Data se pak odešlou tooEvent centra spolu s hello car Vehicle identifikační číslo (VIN) a časové razítko. Z ní čte hello data topologie Storm systémem Apache Storm v clusteru HDInsight, procesy a ukládá je do HDFS.

Během zpracování, je hello VIN použité tooretrieve modelu informace z databáze Cosmos. Tato data se přidá toohello datový proud, než je uložen.

Hello komponenty používané v hello topologie Storm jsou:

* **EventHubSpout** -čte data z Azure Event Hubs
* **TypeConversionBolt** – převede text hello řetězce formátu JSON ze služby Event Hubs do řazené kolekce členů obsahující následující data snímačů hello:
    * Modul teploty
    * O teplotě
    * Rychlost
    * VIN
    * časové razítko
* **DataReferencBolt** -vyhledává hello vehicle model z databáze Cosmos pomocí hello VIN
* **WasbStoreBolt** -úložiště hello tooHDFS dat (Azure Storage)

Následující obrázek Hello je diagram tohoto řešení:

![Topologie Storm](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

## <a name="implementation"></a>Implementace

Dokončení, automatizovaná řešení pro tento scénář je k dispozici jako součást hello [HDInsight-Storm-příklady](https://github.com/hdinsight/hdinsight-storm-examples) úložišti na Githubu. toouse tento příklad, postupujte podle kroků hello v hello [IoTExample README. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Další kroky

Další příklad topologií Storm, najdete v části [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md).

