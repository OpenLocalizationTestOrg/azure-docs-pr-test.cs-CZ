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
# <a name="introducing-apache-kafka-on-hdinsight-preview"></a><span data-ttu-id="da81a-103">Představení Apache Kafka ve službě HDInsight (Preview)</span><span class="sxs-lookup"><span data-stu-id="da81a-103">Introducing Apache Kafka on HDInsight (preview)</span></span>

<span data-ttu-id="da81a-104">[Apache Kafka](https://kafka.apache.org) je open-source distribuované streamování platforma, která lze použít v reálném čase toobuild streamování datových kanálů a aplikace.</span><span class="sxs-lookup"><span data-stu-id="da81a-104">[Apache Kafka](https://kafka.apache.org) is an open-source distributed streaming platform that can be used toobuild real-time streaming data pipelines and applications.</span></span> <span data-ttu-id="da81a-105">Kafka také poskytuje funkce podobné tooa fronta zpráv, kde můžete publikovat a přihlásit se toonamed datové proudy zprostředkovatele zpráv.</span><span class="sxs-lookup"><span data-stu-id="da81a-105">Kafka also provides message broker functionality similar tooa message queue, where you can publish and subscribe toonamed data streams.</span></span> <span data-ttu-id="da81a-106">Kafka v HDInsight vám poskytne spravované, vysoce škálovatelné a vysoce dostupné služby v cloudu Microsoft Azure hello.</span><span class="sxs-lookup"><span data-stu-id="da81a-106">Kafka on HDInsight provides you with a managed, highly scalable, and highly available service in hello Microsoft Azure cloud.</span></span>

## <a name="why-use-kafka-on-hdinsight"></a><span data-ttu-id="da81a-107">Proč používat Kafka ve službě HDInsight?</span><span class="sxs-lookup"><span data-stu-id="da81a-107">Why use Kafka on HDInsight?</span></span>

<span data-ttu-id="da81a-108">Kafka poskytuje hello následující funkce:</span><span class="sxs-lookup"><span data-stu-id="da81a-108">Kafka provides hello following features:</span></span>

* <span data-ttu-id="da81a-109">Publikování a odběru zpráv: Kafka poskytuje producent rozhraní API pro publikování záznamů tooa Kafka tématu.</span><span class="sxs-lookup"><span data-stu-id="da81a-109">Publish-subscribe messaging pattern: Kafka provides a Producer API for publishing records tooa Kafka topic.</span></span> <span data-ttu-id="da81a-110">Hello příjemce rozhraní API se používá při přihlášení k odběru tooa tématu.</span><span class="sxs-lookup"><span data-stu-id="da81a-110">hello Consumer API is used when subscribing tooa topic.</span></span>

* <span data-ttu-id="da81a-111">Zpracování datových proudů: Kafka se často používá společně s Apache Storm nebo Spark ke zpracování datových proudů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="da81a-111">Stream processing: Kafka is often used with Apache Storm or Spark for real-time stream processing.</span></span> <span data-ttu-id="da81a-112">Kafka 0.10.0.0 (HDInsight verze 3.5) zavedla streamování rozhraní API, které vám umožní toobuild streamování řešení bez nutnosti použití Storm a Spark.</span><span class="sxs-lookup"><span data-stu-id="da81a-112">Kafka 0.10.0.0 (HDInsight version 3.5) introduced a streaming API that allows you toobuild streaming solutions without requiring Storm or Spark.</span></span>

* <span data-ttu-id="da81a-113">Vodorovné škálování: Kafka oddíly datové proudy mezi hello uzly v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="da81a-113">Horizontal scale: Kafka partitions streams across hello nodes in hello HDInsight cluster.</span></span> <span data-ttu-id="da81a-114">Příjemce procesy lze přidružit jednotlivé oddíly tooprovide Vyrovnávání zatížení při využívání záznamy.</span><span class="sxs-lookup"><span data-stu-id="da81a-114">Consumer processes can be associated with individual partitions tooprovide load balancing when consuming records.</span></span>

* <span data-ttu-id="da81a-115">Doručení v daném pořadí: v rámci každý oddíl záznamy se ukládají do datového proudu hello v pořadí hello bylo přijato.</span><span class="sxs-lookup"><span data-stu-id="da81a-115">In-order delivery: Within each partition, records are stored in hello stream in hello order that they were received.</span></span> <span data-ttu-id="da81a-116">Přidružením jednoho procesu příjemce na oddíl můžete zajistit zpracování záznamů v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="da81a-116">By associating one consumer process per partition, you can guarantee that records are processed in-order.</span></span>

* <span data-ttu-id="da81a-117">Odolné proti chybám: Oddílů je možné replikovat mezi uzly tooprovide odolnost proti chybám.</span><span class="sxs-lookup"><span data-stu-id="da81a-117">Fault-tolerant: Partitions can be replicated between nodes tooprovide fault tolerance.</span></span>

* <span data-ttu-id="da81a-118">Integrace s Azure spravované disky: spravované disky poskytuje vyšší škálování a propustnost pro disky hello používá hello virtuální počítače v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="da81a-118">Integration with Azure Managed Disks: Managed disks provides higher scale and throughput for hello disks used by hello virtual machines in hello HDInsight cluster.</span></span>

    <span data-ttu-id="da81a-119">Spravované disky jsou ve výchozím nastavení povolená pro Kafka v HDInsight a hello počet disků použitou na uzlu můžete nakonfigurovat při vytváření HDInsight.</span><span class="sxs-lookup"><span data-stu-id="da81a-119">Managed disks are enabled by default for Kafka on HDInsight, and hello number of disks used per node can be configured during HDInsight creation.</span></span> <span data-ttu-id="da81a-120">Další informace o spravovaných discích najdete v tématu [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="da81a-120">For more information on managed disks, see [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md).</span></span>

    <span data-ttu-id="da81a-121">Informace o konfiguraci spravovaných disků v systému Kafka v HDInsight najdete v tématu [Zvýšení škálovatelnosti systému Kafka v prostředí HDInsight](hdinsight-apache-kafka-scalability.md).</span><span class="sxs-lookup"><span data-stu-id="da81a-121">For information on configuring managed disks with Kafka on HDInsight, see [Increase scalability of Kafka on HDInsight](hdinsight-apache-kafka-scalability.md).</span></span>

## <a name="use-cases"></a><span data-ttu-id="da81a-122">Případy použití</span><span class="sxs-lookup"><span data-stu-id="da81a-122">Use cases</span></span>

* <span data-ttu-id="da81a-123">**Zasílání zpráv**: vzhledem k tomu, že podporuje hello publikování a odběru zpráv vzor, Kafka se často používá jako zprostředkovatele zpráv.</span><span class="sxs-lookup"><span data-stu-id="da81a-123">**Messaging**: Since it supports hello publish-subscribe message pattern, Kafka is often used as a message broker.</span></span>

* <span data-ttu-id="da81a-124">**Aktivita sledování**: protože Kafka poskytuje v daném pořadí protokolování záznamů, může být použité tootrack a znovu vytvořit aktivity.</span><span class="sxs-lookup"><span data-stu-id="da81a-124">**Activity tracking**: Since Kafka provides in-order logging of records, it can be used tootrack and re-create activities.</span></span> <span data-ttu-id="da81a-125">Například akcí uživatelů na webu nebo v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="da81a-125">For example, user actions on a web site or within an application.</span></span>

* <span data-ttu-id="da81a-126">**Agregace**: pomocí zpracování datového proudu, můžete shromažďovat informace z různých datových proudů toocombine a centralizovat hello informace do provozních dat.</span><span class="sxs-lookup"><span data-stu-id="da81a-126">**Aggregation**: Using stream processing, you can aggregate information from different streams toocombine and centralize hello information into operational data.</span></span>

* <span data-ttu-id="da81a-127">**Transformace:** Pomocí zpracování datových proudů můžete kombinovat data z více vstupních témat a rozšiřovat je do jednoho nebo několika výstupních témat.</span><span class="sxs-lookup"><span data-stu-id="da81a-127">**Transformation**: Using stream processing, you can combine and enrich data from multiple input topics into one or more output topics.</span></span>

## <a name="next-steps"></a><span data-ttu-id="da81a-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="da81a-128">Next steps</span></span>

<span data-ttu-id="da81a-129">Použití hello následující odkazy toolearn jak toouse Apache Kafka v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="da81a-129">Use hello following links toolearn how toouse Apache Kafka on HDInsight:</span></span>

* [<span data-ttu-id="da81a-130">Začínáme s Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="da81a-130">Get started with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)

* [<span data-ttu-id="da81a-131">Použít MirrorMaker toocreate repliku Kafka v HDInsight</span><span class="sxs-lookup"><span data-stu-id="da81a-131">Use MirrorMaker toocreate a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)

* [<span data-ttu-id="da81a-132">Použití Apache Stormu se systémem Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="da81a-132">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)

* [<span data-ttu-id="da81a-133">Použití Apache Sparku se systémem Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="da81a-133">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)

* [<span data-ttu-id="da81a-134">Připojení tooKafka přes virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="da81a-134">Connect tooKafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)
