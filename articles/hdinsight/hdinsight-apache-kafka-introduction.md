---
title: "aaaAn Úvod tooApache Kafka v HDInsight - Azure | Microsoft Docs"
description: "Další informace o Kafka Apache v HDInsight: co je, jak funguje a kde toofind příklady a jak rychle začít informace."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: f284b6e3-5f3b-4a50-b455-917e77588069
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/15/2017
ms.author: larryfr
ms.openlocfilehash: 1bc198d4cf93a4682030d4fa5f71030f49ad64be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introducing-apache-kafka-on-hdinsight-preview"></a>Představení Apache Kafka ve službě HDInsight (Preview)

[Apache Kafka](https://kafka.apache.org) je open-source distribuované streamování platforma, která lze použít v reálném čase toobuild streamování datových kanálů a aplikace. Kafka také poskytuje funkce podobné tooa fronta zpráv, kde můžete publikovat a přihlásit se toonamed datové proudy zprostředkovatele zpráv. Kafka v HDInsight vám poskytne spravované, vysoce škálovatelné a vysoce dostupné služby v cloudu Microsoft Azure hello.

## <a name="why-use-kafka-on-hdinsight"></a>Proč používat Kafka ve službě HDInsight?

Kafka poskytuje hello následující funkce:

* Publikování a odběru zpráv: Kafka poskytuje producent rozhraní API pro publikování záznamů tooa Kafka tématu. Hello příjemce rozhraní API se používá při přihlášení k odběru tooa tématu.

* Zpracování datových proudů: Kafka se často používá společně s Apache Storm nebo Spark ke zpracování datových proudů v reálném čase. Kafka 0.10.0.0 (HDInsight verze 3.5) zavedla streamování rozhraní API, které vám umožní toobuild streamování řešení bez nutnosti použití Storm a Spark.

* Vodorovné škálování: Kafka oddíly datové proudy mezi hello uzly v clusteru HDInsight hello. Příjemce procesy lze přidružit jednotlivé oddíly tooprovide Vyrovnávání zatížení při využívání záznamy.

* Doručení v daném pořadí: v rámci každý oddíl záznamy se ukládají do datového proudu hello v pořadí hello bylo přijato. Přidružením jednoho procesu příjemce na oddíl můžete zajistit zpracování záznamů v daném pořadí.

* Odolné proti chybám: Oddílů je možné replikovat mezi uzly tooprovide odolnost proti chybám.

* Integrace s Azure spravované disky: spravované disky poskytuje vyšší škálování a propustnost pro disky hello používá hello virtuální počítače v clusteru HDInsight hello.

    Spravované disky jsou ve výchozím nastavení povolená pro Kafka v HDInsight a hello počet disků použitou na uzlu můžete nakonfigurovat při vytváření HDInsight. Další informace o spravovaných discích najdete v tématu [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md).

    Informace o konfiguraci spravovaných disků v systému Kafka v HDInsight najdete v tématu [Zvýšení škálovatelnosti systému Kafka v prostředí HDInsight](hdinsight-apache-kafka-scalability.md).

## <a name="use-cases"></a>Případy použití

* **Zasílání zpráv**: vzhledem k tomu, že podporuje hello publikování a odběru zpráv vzor, Kafka se často používá jako zprostředkovatele zpráv.

* **Aktivita sledování**: protože Kafka poskytuje v daném pořadí protokolování záznamů, může být použité tootrack a znovu vytvořit aktivity. Například akcí uživatelů na webu nebo v aplikaci.

* **Agregace**: pomocí zpracování datového proudu, můžete shromažďovat informace z různých datových proudů toocombine a centralizovat hello informace do provozních dat.

* **Transformace:** Pomocí zpracování datových proudů můžete kombinovat data z více vstupních témat a rozšiřovat je do jednoho nebo několika výstupních témat.

## <a name="next-steps"></a>Další kroky

Použití hello následující odkazy toolearn jak toouse Apache Kafka v HDInsight:

* [Začínáme s Kafka ve službě HDInsight](hdinsight-apache-kafka-get-started.md)

* [Použít MirrorMaker toocreate repliku Kafka v HDInsight](hdinsight-apache-kafka-mirroring.md)

* [Použití Apache Stormu se systémem Kafka ve službě HDInsight](hdinsight-apache-storm-with-kafka.md)

* [Použití Apache Sparku se systémem Kafka ve službě HDInsight](hdinsight-apache-spark-with-kafka.md)

* [Připojení tooKafka přes virtuální síť Azure](hdinsight-apache-kafka-connect-vpn-gateway.md)
