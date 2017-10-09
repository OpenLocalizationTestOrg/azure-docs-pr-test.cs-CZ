---
title: dostupnost aaaHigh s Apache Kafka - Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak tooensure vysoká dostupnost s Apache Kafka v Azure HDInsight. Zjistěte, jak toorebalance oddílu replik v Kafka, aby se na různé chyby domén v rámci hello oblast Azure, který obsahuje HDInsight."
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
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 337468f36b531f83c2999e87907de89cf3d19dd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-of-your-data-with-apache-kafka-preview-on-hdinsight"></a>Vysoká dostupnost dat s využitím Apache Kafka (Preview) ve službě HDInsight

Zjistěte, jak tooconfigure oddílu replik pro Kafka témata tootake využívat základní hardware rack konfigurace. Tato konfigurace zajistí hello dostupnosti dat uložených v Kafka Apache v HDInsight.

## <a name="fault-and-update-domains-with-kafka"></a>Domény selhání a aktualizační domény s využitím Kafka

Doména selhání je logické seskupení základního hardwaru v datovém centru Azure. Všechny domény selhání sdílí společný zdroje napájení a síťový přepínač. Hello virtuální počítače a spravované disky, které implementují hello uzlů v clusteru služby HDInsight jsou rozmístěny v těchto domén selhání. Tato architektura omezuje hello potenciální dopad selhání fyzického hardwaru.

Každá oblast Azure má určitý počet domén selhání. Seznam domén a hello počet domén selhání obsahují najdete v tématu hello [sady dostupnosti](../virtual-machines/linux/regions-and-availability.md#availability-sets) dokumentaci.

> [!IMPORTANT]
> Kafka nemá o doménách selhání žádné informace. Když vytvoříte téma v Kafka, může je uložit všechny repliky oddílu v hello stejné domény selhání. toosolve tyto potíže odstranit, poskytujeme hello [Kafka oddílu obnovte rovnováhu nástroj](https://github.com/hdinsight/hdinsight-kafka-tools).

## <a name="when-toorebalance-partition-replicas"></a>Když toorebalance oddílu repliky

tooensure hello nejvyšší dostupnost vašich dat Kafka, musíte znovu vyvážit hello oddílu replik pro dané téma na hello časy následující:

* Při vytvoření nového tématu nebo oddílu

* Při vertikálním navýšení kapacity clusteru

## <a name="replication-factor"></a>Faktor replikace

> [!IMPORTANT]
> Doporučujeme použít oblast Azure, která obsahuje tři domény selhání, a použít faktor replikace 3.

Pokud musíte použít oblasti, která obsahuje pouze dva domén selhání, použijte replikace Multi-Factor 4 toospread hello replik rovnoměrně mezi dvěma doménami selhání hello.

Příklad vytvoření témata a nastavení hello replikace faktor, naleznete v části hello [začínat Kafka v HDInsight](hdinsight-apache-kafka-get-started.md) dokumentu.

## <a name="how-toorebalance-partition-replicas"></a>Jak toorebalance oddílu repliky

Použití hello [Kafka oddílu obnovte rovnováhu nástroj](https://github.com/hdinsight/hdinsight-kafka-tools) toorebalance vybrané témata. Tento nástroj musí spustili z SSH relace toohello hlavního uzlu clusteru Kafka.

Další informace o připojení pomocí protokolu SSH tooHDInsight najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentu.

## <a name="next-steps"></a>Další kroky

* [Škálovatelnost Kafka ve službě HDInsight](hdinsight-apache-kafka-scalability.md)
* [Zrcadlení s využitím Kafka ve službě HDInsight](hdinsight-apache-kafka-mirroring.md)