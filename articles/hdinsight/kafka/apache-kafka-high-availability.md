---
title: "Vysoká dostupnost s využitím Apache Kafka – Azure HDInsight | Dokumentace Microsoftu"
description: "Zjistěte, jak zajistit vysokou dostupnost s využitím Apache Kafka ve službě Azure HDInsight. Naučte se obnovit rovnováhu replik oddílů v Kafka, aby byly v různých doménách selhání v rámci oblasti Azure, která obsahuje HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/07/2017
ms.author: larryfr
ms.openlocfilehash: f7a4245435093358cac567cf08c8ce3979371c04
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="high-availability-of-your-data-with-apache-kafka-on-hdinsight"></a>Vysoká dostupnost dat s využitím Apache Kafka ve službě HDInsight

Zjistěte, jak nakonfigurovat repliky oddílů pro témata Kafka a využít výhod konfigurace použitého hardwarového racku. Tato konfigurace zajišťuje dostupnost dat uložených v Apache Kafka ve službě HDInsight.

## <a name="fault-and-update-domains-with-kafka"></a>Domény selhání a aktualizační domény s využitím Kafka

Doména selhání je logické seskupení základního hardwaru v datovém centru Azure. Všechny domény selhání sdílí společný zdroje napájení a síťový přepínač. Virtuální počítače a spravované disky, které implementují uzly v clusteru služby HDInsight, jsou distribuované napříč těmito doménami selhání. Tato architektura omezuje potenciální dopad selhání fyzického hardwaru.

Každá oblast Azure má určitý počet domén selhání. Seznam domén a počet domén selhání, které obsahují, najdete v dokumentaci [Skupiny dostupnosti](../../virtual-machines/windows/regions-and-availability.md#availability-sets).

> [!IMPORTANT]
> Kafka nemá o doménách selhání žádné informace. Když vytvoříte téma v Kafka, může uložit všechny repliky oddílů ve stejné doméně selhání. K vyřešení tohoto problému poskytujeme [nástroj pro obnovení rovnováhy oddílů Kafka](https://github.com/hdinsight/hdinsight-kafka-tools).

## <a name="when-to-rebalance-partition-replicas"></a>Kdy obnovit rovnováhu replik oddílů

K zajištění nejvyšší dostupnost dat Kafka byste měli obnovit rovnováhu replik oddílů pro vaše téma v těchto situacích:

* Při vytvoření nového tématu nebo oddílu

* Při vertikálním navýšení kapacity clusteru

## <a name="replication-factor"></a>Faktor replikace

> [!IMPORTANT]
> Doporučujeme použít oblast Azure, která obsahuje tři domény selhání, a použít faktor replikace 3.

Pokud musíte použít oblast, která obsahuje jenom dvě domény selhání, použijte faktor replikace 4, abyste zajistili rovnoměrné rozložení replik napříč těmito dvěma doménami selhání.

Příklad vytvoření tématu a nastavení faktoru replikace najdete v dokumentu [Začínáme s Kafka ve službě HDInsight](apache-kafka-get-started.md).

## <a name="how-to-rebalance-partition-replicas"></a>Jak obnovit rovnováhu replik oddílů

K obnovení rovnováhy vybraných témat použijte [nástroj pro obnovení rovnováhy oddílů Kafka](https://github.com/hdinsight/hdinsight-kafka-tools). Tento nástroj se musí spustit z relace SSH na hlavní uzel clusteru Kafka.

Další informace o připojení ke službě HDInsight pomocí SSH najdete v dokumentu [Použití SSH s HDInsightem](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="next-steps"></a>Další kroky

* [Škálovatelnost Kafka ve službě HDInsight](apache-kafka-scalability.md)
* [Zrcadlení s využitím Kafka ve službě HDInsight](apache-kafka-mirroring.md)