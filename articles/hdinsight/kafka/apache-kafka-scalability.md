---
title: "Zvýšení rozsahu platformy Apache Kafka – Azure HDInsight | Dokumentace Microsoftu"
description: "Zjistěte, jak nakonfigurovat spravované disky pro cluster Apache Kafka v prostředí Azure HDInsight, které zvýší škálovatelnost."
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
ms.date: 11/28/2017
ms.author: larryfr
ms.openlocfilehash: e0fef242810148938e62dab9f77ee77b543fbbc0
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="configure-storage-and-scalability-for-apache-kafka-on-hdinsight"></a>Konfigurace úložiště a škálovatelnosti pro platformu Apache Kafka v prostředí HDInsight

Naučte se konfigurovat spravované disky, které využívá Apache Kafka v prostředí HDInsight.

Platforma Kafka ve službě HDInsight používá místní disky virtuálních počítačů v clusteru HDInsight. Protože je platforma Kafka náročná na V/V prostředky, k zajištění vysoké propustnosti a vyšší kapacity úložiště na každý uzel se využívá služba [Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md). Kdyby platforma Kafka využívala tradiční virtuální pevné disky (VHD), každý uzel by byl omezený na 1 TB. Díky službě Managed Disks můžete používat více disků, se kterými každý uzel v clusteru nabídne kapacitu až 16 TB.

Následující diagram porovnává platformu Kafka ve službě HDInsight před použitím spravovaných disků a s nimi:

![Diagram zobrazující platformu Kafka ve službě HDInsight při použití jednoho virtuálního pevného disku a při použití několika spravovaných disků v každém virtuálním počítači](./media/apache-kafka-scalability/kafka-with-managed-disks-architecture.png)

## <a name="configure-managed-disks-azure-portal"></a>Konfigurace spravovaných disků: portál Azure Portal

1. Postupujte podle kroků v tématu [Vytvoření clusteru HDInsight](../hdinsight-hadoop-create-linux-clusters-portal.md), kde se dozvíte, jaký je obecný postup pro vytvoření clusteru pomocí portálu. Proces vytvoření clusteru pomocí portálu nedokončujte.

2. V části __Velikost clusteru__ pomocí pole __Počet disků v pracovním uzlu__ nakonfigurujte počet disků.

    > [!NOTE]
    > Typ spravovaného disku může být buď __Standardní__ (HDD), nebo __Prémiový__ (SSD). Prémiové disky se používají u virtuálních počítačů řady DS a GS. Všechny ostatní typy virtuálních počítačů používají standardní disky.

    ![Obrázek části Velikost clusteru se zvýrazněným počtem disků v pracovním uzlu](./media/apache-kafka-scalability/set-managed-disks-portal.png)

## <a name="configure-managed-disks-resource-manager-template"></a>Konfigurace spravovaných disků: šablony Resource Manageru

Pokud chcete nastavit počet disků, které využívají pracovní uzly v clusteru Kafka, použijte následující část šablony:

```json
"dataDisksGroups": [
    {
        "disksPerNode": "[variables('disksPerWorkerNode')]"
    }
    ],
```

Úplnou šablonu s ukázkou konfigurace spravovaných disků najdete na adrese [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).

## <a name="next-steps"></a>Další kroky

Další informace o práci se systémem Kafka v prostředí HDInsight najdete v následujících dokumentech:

* [Vytvoření repliky Kafka ve službě HDInsight pomocí MirrorMakeru](apache-kafka-mirroring.md)
* [Použití Apache Stormu se systémem Kafka ve službě HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Použití Apache Sparku se systémem Kafka ve službě HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Připojení k systému Kafka přes virtuální síť Azure](apache-kafka-connect-vpn-gateway.md)

* [Příspěvek na blogu HDInsight o spravovaných discích se systémem Kafka](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/)