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
# <a name="high-availability-of-your-data-with-apache-kafka-preview-on-hdinsight"></a><span data-ttu-id="c40a2-104">Vysoká dostupnost dat s využitím Apache Kafka (Preview) ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="c40a2-104">High availability of your data with Apache Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="c40a2-105">Zjistěte, jak tooconfigure oddílu replik pro Kafka témata tootake využívat základní hardware rack konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c40a2-105">Learn how tooconfigure partition replicas for Kafka topics tootake advantage of underlying hardware rack configuration.</span></span> <span data-ttu-id="c40a2-106">Tato konfigurace zajistí hello dostupnosti dat uložených v Kafka Apache v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c40a2-106">This configuration ensures hello availability of data stored in Apache Kafka on HDInsight.</span></span>

## <a name="fault-and-update-domains-with-kafka"></a><span data-ttu-id="c40a2-107">Domény selhání a aktualizační domény s využitím Kafka</span><span class="sxs-lookup"><span data-stu-id="c40a2-107">Fault and update domains with Kafka</span></span>

<span data-ttu-id="c40a2-108">Doména selhání je logické seskupení základního hardwaru v datovém centru Azure.</span><span class="sxs-lookup"><span data-stu-id="c40a2-108">A fault domain is a logical grouping of underlying hardware in an Azure data center.</span></span> <span data-ttu-id="c40a2-109">Všechny domény selhání sdílí společný zdroje napájení a síťový přepínač.</span><span class="sxs-lookup"><span data-stu-id="c40a2-109">Each fault domain shares a common power source and network switch.</span></span> <span data-ttu-id="c40a2-110">Hello virtuální počítače a spravované disky, které implementují hello uzlů v clusteru služby HDInsight jsou rozmístěny v těchto domén selhání.</span><span class="sxs-lookup"><span data-stu-id="c40a2-110">hello virtual machines and managed disks that implement hello nodes within an HDInsight cluster are distributed across these fault domains.</span></span> <span data-ttu-id="c40a2-111">Tato architektura omezuje hello potenciální dopad selhání fyzického hardwaru.</span><span class="sxs-lookup"><span data-stu-id="c40a2-111">This architecture limits hello potential impact of physical hardware failures.</span></span>

<span data-ttu-id="c40a2-112">Každá oblast Azure má určitý počet domén selhání.</span><span class="sxs-lookup"><span data-stu-id="c40a2-112">Each Azure region has a specific number of fault domains.</span></span> <span data-ttu-id="c40a2-113">Seznam domén a hello počet domén selhání obsahují najdete v tématu hello [sady dostupnosti](../virtual-machines/linux/regions-and-availability.md#availability-sets) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="c40a2-113">For a list of domains and hello number of fault domains they contain, see hello [Availability sets](../virtual-machines/linux/regions-and-availability.md#availability-sets) documentation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c40a2-114">Kafka nemá o doménách selhání žádné informace.</span><span class="sxs-lookup"><span data-stu-id="c40a2-114">Kafka is not aware of fault domains.</span></span> <span data-ttu-id="c40a2-115">Když vytvoříte téma v Kafka, může je uložit všechny repliky oddílu v hello stejné domény selhání.</span><span class="sxs-lookup"><span data-stu-id="c40a2-115">When you create a topic in Kafka, it may store all partition replicas in hello same fault domain.</span></span> <span data-ttu-id="c40a2-116">toosolve tyto potíže odstranit, poskytujeme hello [Kafka oddílu obnovte rovnováhu nástroj](https://github.com/hdinsight/hdinsight-kafka-tools).</span><span class="sxs-lookup"><span data-stu-id="c40a2-116">toosolve this problem, we provide hello [Kafka partition rebalance tool](https://github.com/hdinsight/hdinsight-kafka-tools).</span></span>

## <a name="when-toorebalance-partition-replicas"></a><span data-ttu-id="c40a2-117">Když toorebalance oddílu repliky</span><span class="sxs-lookup"><span data-stu-id="c40a2-117">When toorebalance partition replicas</span></span>

<span data-ttu-id="c40a2-118">tooensure hello nejvyšší dostupnost vašich dat Kafka, musíte znovu vyvážit hello oddílu replik pro dané téma na hello časy následující:</span><span class="sxs-lookup"><span data-stu-id="c40a2-118">tooensure hello highest availability of your Kafka data, you should rebalance hello partition replicas for your topic at hello following times:</span></span>

* <span data-ttu-id="c40a2-119">Při vytvoření nového tématu nebo oddílu</span><span class="sxs-lookup"><span data-stu-id="c40a2-119">When a new topic or partition is created</span></span>

* <span data-ttu-id="c40a2-120">Při vertikálním navýšení kapacity clusteru</span><span class="sxs-lookup"><span data-stu-id="c40a2-120">When you scale up a cluster</span></span>

## <a name="replication-factor"></a><span data-ttu-id="c40a2-121">Faktor replikace</span><span class="sxs-lookup"><span data-stu-id="c40a2-121">Replication factor</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c40a2-122">Doporučujeme použít oblast Azure, která obsahuje tři domény selhání, a použít faktor replikace 3.</span><span class="sxs-lookup"><span data-stu-id="c40a2-122">We recommend using an Azure region that contains three fault domains, and using a replication factor of 3.</span></span>

<span data-ttu-id="c40a2-123">Pokud musíte použít oblasti, která obsahuje pouze dva domén selhání, použijte replikace Multi-Factor 4 toospread hello replik rovnoměrně mezi dvěma doménami selhání hello.</span><span class="sxs-lookup"><span data-stu-id="c40a2-123">If you must use a region that contains only two fault domains, use a replication factor of 4 toospread hello replicas evenly across hello two fault domains.</span></span>

<span data-ttu-id="c40a2-124">Příklad vytvoření témata a nastavení hello replikace faktor, naleznete v části hello [začínat Kafka v HDInsight](hdinsight-apache-kafka-get-started.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="c40a2-124">For an example of creating topics and setting hello replication factor, see hello [Start with Kafka on HDInsight](hdinsight-apache-kafka-get-started.md) document.</span></span>

## <a name="how-toorebalance-partition-replicas"></a><span data-ttu-id="c40a2-125">Jak toorebalance oddílu repliky</span><span class="sxs-lookup"><span data-stu-id="c40a2-125">How toorebalance partition replicas</span></span>

<span data-ttu-id="c40a2-126">Použití hello [Kafka oddílu obnovte rovnováhu nástroj](https://github.com/hdinsight/hdinsight-kafka-tools) toorebalance vybrané témata.</span><span class="sxs-lookup"><span data-stu-id="c40a2-126">Use hello [Kafka partition rebalance tool](https://github.com/hdinsight/hdinsight-kafka-tools) toorebalance selected topics.</span></span> <span data-ttu-id="c40a2-127">Tento nástroj musí spustili z SSH relace toohello hlavního uzlu clusteru Kafka.</span><span class="sxs-lookup"><span data-stu-id="c40a2-127">This tool must be ran from an SSH session toohello head node of your Kafka cluster.</span></span>

<span data-ttu-id="c40a2-128">Další informace o připojení pomocí protokolu SSH tooHDInsight najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="c40a2-128">For more information on connecting tooHDInsight using SSH, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c40a2-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c40a2-129">Next steps</span></span>

* [<span data-ttu-id="c40a2-130">Škálovatelnost Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="c40a2-130">Scalability of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-scalability.md)
* [<span data-ttu-id="c40a2-131">Zrcadlení s využitím Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="c40a2-131">Mirroring with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)