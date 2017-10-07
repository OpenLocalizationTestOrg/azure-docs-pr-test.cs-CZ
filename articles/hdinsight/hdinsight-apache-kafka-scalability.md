---
title: "aaaApache Kafka zvětšit měřítko - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak tooconfigure spravuje disky pro cluster Apache Kafka na škálovatelnost tooincrease Azure HDInsight."
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
ms.date: 06/14/2017
ms.author: larryfr
ms.openlocfilehash: b51114b33359f2c385f057a7a7a5b134cad27e51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-storage-and-scalability-for-apache-kafka-on-hdinsight"></a>Konfigurace úložiště a škálovatelnosti pro platformu Apache Kafka v prostředí HDInsight

Zjistěte, jak tooconfigure hello počet spravovaných disků používané Apache Kafka v HDInsight.

Kafka v HDInsight používá místní disk hello hello virtuálních počítačů v clusteru HDInsight hello. Vzhledem k tomu, že je Kafka vstupu a výstupu velmi vysoké, [Azure spravované disky](../virtual-machines/windows/managed-disks-overview.md) je použité tooprovide vysoké propustnosti a poskytnout další úložiště na jeden uzel. Pokud tradiční virtuální pevné disky (VHD) se používá pro Kafka, každý uzel je omezená too1 TB. S spravované disky, můžete použít několik disků tooachieve 16 TB pro každý uzel v clusteru hello.

Hello následující diagram představuje porovnání mezi Kafka v HDInsight před spravovaných disků a Kafka v HDInsight s spravované disky:

![Diagram zobrazující platformu Kafka ve službě HDInsight při použití jednoho virtuálního pevného disku a při použití několika spravovaných disků v každém virtuálním počítači](./media/hdinsight-apache-kafka-scalability/kafka-with-managed-disks-architecture.png)

## <a name="configure-managed-disks-azure-portal"></a>Konfigurace spravovaných disků: portál Azure Portal

1. Postupujte podle kroků hello v hello [vytvoření clusteru HDInsight](hdinsight-hadoop-create-linux-clusters-portal.md) toounderstand hello běžné kroky toocreate clusteru pomocí portálu hello. Proces vytvoření portálu hello nejsou dokončeny.

2. Z hello __velikost clusteru__ okno, použijte hello __disků na pracovního uzlu__ pole tooconfigure hello počet disků.

    > [!NOTE]
    > Hello typ spravovaného disku může být buď __standardní__ (HDD) nebo __Premium__ (SSD). Prémiové disky se používají u virtuálních počítačů řady DS a GS. Všechny ostatní typy virtuálních počítačů používají standardní disky.

    ![Obrázek hello clusteru velikost okna s hello disků na zvýrazněnou pracovního uzlu](./media/hdinsight-apache-kafka-scalability/set-managed-disks-portal.png)

## <a name="configure-managed-disks-resource-manager-template"></a>Konfigurace spravovaných disků: šablony Resource Manageru

toocontrol hello počet disky používané hello uzlů pracovního procesu v clusteru s podporou Kafka, hello použijte následující části hello šablony:

```json
"dataDisksGroups": [
    {
        "disksPerNode": "[variables('disksPerWorkerNode')]"
    }
    ],
```

Můžete najít úplnou šablonu, která demonstruje, jak tooconfigure spravovaných disků na [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).

## <a name="next-steps"></a>Další kroky

Další informace o práci s Kafka v HDInsight naleznete v tématu hello následující dokumenty:

* [Použít MirrorMaker toocreate repliku Kafka v HDInsight](hdinsight-apache-kafka-mirroring.md)
* [Použití Apache Stormu se systémem Kafka ve službě HDInsight](hdinsight-apache-storm-with-kafka.md)
* [Použití Apache Sparku se systémem Kafka ve službě HDInsight](hdinsight-apache-spark-with-kafka.md)
* [Připojení tooKafka přes virtuální síť Azure](hdinsight-apache-kafka-connect-vpn-gateway.md)

* [Příspěvek na blogu HDInsight o spravovaných discích se systémem Kafka](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/)