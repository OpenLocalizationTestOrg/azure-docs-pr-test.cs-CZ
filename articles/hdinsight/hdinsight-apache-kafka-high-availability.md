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
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: f8164d1c3483b28e5f2abcc8035da78880daec1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="high-availability-of-your-data-with-apache-kafka-preview-on-hdinsight"></a><span data-ttu-id="ecc69-104">Vysoká dostupnost dat s využitím Apache Kafka (Preview) ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="ecc69-104">High availability of your data with Apache Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="ecc69-105">Zjistěte, jak nakonfigurovat repliky oddílů pro témata Kafka a využít výhod konfigurace použitého hardwarového racku.</span><span class="sxs-lookup"><span data-stu-id="ecc69-105">Learn how to configure partition replicas for Kafka topics to take advantage of underlying hardware rack configuration.</span></span> <span data-ttu-id="ecc69-106">Tato konfigurace zajišťuje dostupnost dat uložených v Apache Kafka ve službě HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ecc69-106">This configuration ensures the availability of data stored in Apache Kafka on HDInsight.</span></span>

## <a name="fault-and-update-domains-with-kafka"></a><span data-ttu-id="ecc69-107">Domény selhání a aktualizační domény s využitím Kafka</span><span class="sxs-lookup"><span data-stu-id="ecc69-107">Fault and update domains with Kafka</span></span>

<span data-ttu-id="ecc69-108">Doména selhání je logické seskupení základního hardwaru v datovém centru Azure.</span><span class="sxs-lookup"><span data-stu-id="ecc69-108">A fault domain is a logical grouping of underlying hardware in an Azure data center.</span></span> <span data-ttu-id="ecc69-109">Všechny domény selhání sdílí společný zdroje napájení a síťový přepínač.</span><span class="sxs-lookup"><span data-stu-id="ecc69-109">Each fault domain shares a common power source and network switch.</span></span> <span data-ttu-id="ecc69-110">Virtuální počítače a spravované disky, které implementují uzly v clusteru služby HDInsight, jsou distribuované napříč těmito doménami selhání.</span><span class="sxs-lookup"><span data-stu-id="ecc69-110">The virtual machines and managed disks that implement the nodes within an HDInsight cluster are distributed across these fault domains.</span></span> <span data-ttu-id="ecc69-111">Tato architektura omezuje potenciální dopad selhání fyzického hardwaru.</span><span class="sxs-lookup"><span data-stu-id="ecc69-111">This architecture limits the potential impact of physical hardware failures.</span></span>

<span data-ttu-id="ecc69-112">Každá oblast Azure má určitý počet domén selhání.</span><span class="sxs-lookup"><span data-stu-id="ecc69-112">Each Azure region has a specific number of fault domains.</span></span> <span data-ttu-id="ecc69-113">Seznam domén a počet domén selhání, které obsahují, najdete v dokumentaci [Skupiny dostupnosti](../virtual-machines/linux/regions-and-availability.md#availability-sets).</span><span class="sxs-lookup"><span data-stu-id="ecc69-113">For a list of domains and the number of fault domains they contain, see the [Availability sets](../virtual-machines/linux/regions-and-availability.md#availability-sets) documentation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ecc69-114">Kafka nemá o doménách selhání žádné informace.</span><span class="sxs-lookup"><span data-stu-id="ecc69-114">Kafka is not aware of fault domains.</span></span> <span data-ttu-id="ecc69-115">Když vytvoříte téma v Kafka, může uložit všechny repliky oddílů ve stejné doméně selhání.</span><span class="sxs-lookup"><span data-stu-id="ecc69-115">When you create a topic in Kafka, it may store all partition replicas in the same fault domain.</span></span> <span data-ttu-id="ecc69-116">K vyřešení tohoto problému poskytujeme [nástroj pro obnovení rovnováhy oddílů Kafka](https://github.com/hdinsight/hdinsight-kafka-tools).</span><span class="sxs-lookup"><span data-stu-id="ecc69-116">To solve this problem, we provide the [Kafka partition rebalance tool](https://github.com/hdinsight/hdinsight-kafka-tools).</span></span>

## <a name="when-to-rebalance-partition-replicas"></a><span data-ttu-id="ecc69-117">Kdy obnovit rovnováhu replik oddílů</span><span class="sxs-lookup"><span data-stu-id="ecc69-117">When to rebalance partition replicas</span></span>

<span data-ttu-id="ecc69-118">K zajištění nejvyšší dostupnost dat Kafka byste měli obnovit rovnováhu replik oddílů pro vaše téma v těchto situacích:</span><span class="sxs-lookup"><span data-stu-id="ecc69-118">To ensure the highest availability of your Kafka data, you should rebalance the partition replicas for your topic at the following times:</span></span>

* <span data-ttu-id="ecc69-119">Při vytvoření nového tématu nebo oddílu</span><span class="sxs-lookup"><span data-stu-id="ecc69-119">When a new topic or partition is created</span></span>

* <span data-ttu-id="ecc69-120">Při vertikálním navýšení kapacity clusteru</span><span class="sxs-lookup"><span data-stu-id="ecc69-120">When you scale up a cluster</span></span>

## <a name="replication-factor"></a><span data-ttu-id="ecc69-121">Faktor replikace</span><span class="sxs-lookup"><span data-stu-id="ecc69-121">Replication factor</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ecc69-122">Doporučujeme použít oblast Azure, která obsahuje tři domény selhání, a použít faktor replikace 3.</span><span class="sxs-lookup"><span data-stu-id="ecc69-122">We recommend using an Azure region that contains three fault domains, and using a replication factor of 3.</span></span>

<span data-ttu-id="ecc69-123">Pokud musíte použít oblast, která obsahuje jenom dvě domény selhání, použijte faktor replikace 4, abyste zajistili rovnoměrné rozložení replik napříč těmito dvěma doménami selhání.</span><span class="sxs-lookup"><span data-stu-id="ecc69-123">If you must use a region that contains only two fault domains, use a replication factor of 4 to spread the replicas evenly across the two fault domains.</span></span>

<span data-ttu-id="ecc69-124">Příklad vytvoření tématu a nastavení faktoru replikace najdete v dokumentu [Začínáme s Kafka ve službě HDInsight](hdinsight-apache-kafka-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ecc69-124">For an example of creating topics and setting the replication factor, see the [Start with Kafka on HDInsight](hdinsight-apache-kafka-get-started.md) document.</span></span>

## <a name="how-to-rebalance-partition-replicas"></a><span data-ttu-id="ecc69-125">Jak obnovit rovnováhu replik oddílů</span><span class="sxs-lookup"><span data-stu-id="ecc69-125">How to rebalance partition replicas</span></span>

<span data-ttu-id="ecc69-126">K obnovení rovnováhy vybraných témat použijte [nástroj pro obnovení rovnováhy oddílů Kafka](https://github.com/hdinsight/hdinsight-kafka-tools).</span><span class="sxs-lookup"><span data-stu-id="ecc69-126">Use the [Kafka partition rebalance tool](https://github.com/hdinsight/hdinsight-kafka-tools) to rebalance selected topics.</span></span> <span data-ttu-id="ecc69-127">Tento nástroj se musí spustit z relace SSH na hlavní uzel clusteru Kafka.</span><span class="sxs-lookup"><span data-stu-id="ecc69-127">This tool must be ran from an SSH session to the head node of your Kafka cluster.</span></span>

<span data-ttu-id="ecc69-128">Další informace o připojení ke službě HDInsight pomocí SSH najdete v dokumentu [Použití SSH s HDInsightem](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ecc69-128">For more information on connecting to HDInsight using SSH, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ecc69-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ecc69-129">Next steps</span></span>

* [<span data-ttu-id="ecc69-130">Škálovatelnost Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="ecc69-130">Scalability of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-scalability.md)
* [<span data-ttu-id="ecc69-131">Zrcadlení s využitím Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="ecc69-131">Mirroring with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)