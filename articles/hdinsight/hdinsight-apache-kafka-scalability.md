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
# <a name="configure-storage-and-scalability-for-apache-kafka-on-hdinsight"></a><span data-ttu-id="0b32e-103">Konfigurace úložiště a škálovatelnosti pro platformu Apache Kafka v prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="0b32e-103">Configure storage and scalability for Apache Kafka on HDInsight</span></span>

<span data-ttu-id="0b32e-104">Zjistěte, jak tooconfigure hello počet spravovaných disků používané Apache Kafka v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0b32e-104">Learn how tooconfigure hello number of managed disks used by Apache Kafka on HDInsight.</span></span>

<span data-ttu-id="0b32e-105">Kafka v HDInsight používá místní disk hello hello virtuálních počítačů v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-105">Kafka on HDInsight uses hello local disk of hello virtual machines in hello HDInsight cluster.</span></span> <span data-ttu-id="0b32e-106">Vzhledem k tomu, že je Kafka vstupu a výstupu velmi vysoké, [Azure spravované disky](../virtual-machines/windows/managed-disks-overview.md) je použité tooprovide vysoké propustnosti a poskytnout další úložiště na jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="0b32e-106">Since Kafka is very I/O heavy, [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md) is used tooprovide high throughput and provide more storage per node.</span></span> <span data-ttu-id="0b32e-107">Pokud tradiční virtuální pevné disky (VHD) se používá pro Kafka, každý uzel je omezená too1 TB.</span><span class="sxs-lookup"><span data-stu-id="0b32e-107">If traditional virtual hard drives (VHD) were used for Kafka, each node is limited too1 TB.</span></span> <span data-ttu-id="0b32e-108">S spravované disky, můžete použít několik disků tooachieve 16 TB pro každý uzel v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-108">With managed disks, you can use multiple disks tooachieve 16 TB for each node in hello cluster.</span></span>

<span data-ttu-id="0b32e-109">Hello následující diagram představuje porovnání mezi Kafka v HDInsight před spravovaných disků a Kafka v HDInsight s spravované disky:</span><span class="sxs-lookup"><span data-stu-id="0b32e-109">hello following diagram provides a comparison between Kafka on HDInsight before managed disks, and Kafka on HDInsight with managed disks:</span></span>

![Diagram zobrazující platformu Kafka ve službě HDInsight při použití jednoho virtuálního pevného disku a při použití několika spravovaných disků v každém virtuálním počítači](./media/hdinsight-apache-kafka-scalability/kafka-with-managed-disks-architecture.png)

## <a name="configure-managed-disks-azure-portal"></a><span data-ttu-id="0b32e-111">Konfigurace spravovaných disků: portál Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0b32e-111">Configure managed disks: Azure portal</span></span>

1. <span data-ttu-id="0b32e-112">Postupujte podle kroků hello v hello [vytvoření clusteru HDInsight](hdinsight-hadoop-create-linux-clusters-portal.md) toounderstand hello běžné kroky toocreate clusteru pomocí portálu hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-112">Follow hello steps in hello [Create an HDInsight cluster](hdinsight-hadoop-create-linux-clusters-portal.md) toounderstand hello common steps toocreate a cluster using hello portal.</span></span> <span data-ttu-id="0b32e-113">Proces vytvoření portálu hello nejsou dokončeny.</span><span class="sxs-lookup"><span data-stu-id="0b32e-113">Do not complete hello portal creation process.</span></span>

2. <span data-ttu-id="0b32e-114">Z hello __velikost clusteru__ okno, použijte hello __disků na pracovního uzlu__ pole tooconfigure hello počet disků.</span><span class="sxs-lookup"><span data-stu-id="0b32e-114">From hello __Cluster size__ blade, use hello __Disks per worker node__ field tooconfigure hello number of disks.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0b32e-115">Hello typ spravovaného disku může být buď __standardní__ (HDD) nebo __Premium__ (SSD).</span><span class="sxs-lookup"><span data-stu-id="0b32e-115">hello type of managed disk can be either __Standard__ (HDD) or __Premium__ (SSD).</span></span> <span data-ttu-id="0b32e-116">Prémiové disky se používají u virtuálních počítačů řady DS a GS.</span><span class="sxs-lookup"><span data-stu-id="0b32e-116">Premium disks are used with DS and GS series VMs.</span></span> <span data-ttu-id="0b32e-117">Všechny ostatní typy virtuálních počítačů používají standardní disky.</span><span class="sxs-lookup"><span data-stu-id="0b32e-117">All other VM types use standard.</span></span>

    ![Obrázek hello clusteru velikost okna s hello disků na zvýrazněnou pracovního uzlu](./media/hdinsight-apache-kafka-scalability/set-managed-disks-portal.png)

## <a name="configure-managed-disks-resource-manager-template"></a><span data-ttu-id="0b32e-119">Konfigurace spravovaných disků: šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="0b32e-119">Configure managed disks: Resource Manager template</span></span>

<span data-ttu-id="0b32e-120">toocontrol hello počet disky používané hello uzlů pracovního procesu v clusteru s podporou Kafka, hello použijte následující části hello šablony:</span><span class="sxs-lookup"><span data-stu-id="0b32e-120">toocontrol hello number of disks used by hello worker nodes in a Kafka cluster, use hello following section of hello template:</span></span>

```json
"dataDisksGroups": [
    {
        "disksPerNode": "[variables('disksPerWorkerNode')]"
    }
    ],
```

<span data-ttu-id="0b32e-121">Můžete najít úplnou šablonu, která demonstruje, jak tooconfigure spravovaných disků na [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).</span><span class="sxs-lookup"><span data-stu-id="0b32e-121">You can find a complete template that demonstrates how tooconfigure managed disks at [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b32e-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0b32e-122">Next steps</span></span>

<span data-ttu-id="0b32e-123">Další informace o práci s Kafka v HDInsight naleznete v tématu hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="0b32e-123">For more information on working with Kafka on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="0b32e-124">Použít MirrorMaker toocreate repliku Kafka v HDInsight</span><span class="sxs-lookup"><span data-stu-id="0b32e-124">Use MirrorMaker toocreate a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
* [<span data-ttu-id="0b32e-125">Použití Apache Stormu se systémem Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="0b32e-125">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)
* [<span data-ttu-id="0b32e-126">Použití Apache Sparku se systémem Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="0b32e-126">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)
* [<span data-ttu-id="0b32e-127">Připojení tooKafka přes virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="0b32e-127">Connect tooKafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)

* [<span data-ttu-id="0b32e-128">Příspěvek na blogu HDInsight o spravovaných discích se systémem Kafka</span><span class="sxs-lookup"><span data-stu-id="0b32e-128">HDInsight blog on managed disks with Kafka</span></span>](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/)