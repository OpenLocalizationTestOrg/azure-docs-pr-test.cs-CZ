---
title: "Úvod k systému Apache Kafka ve službě HDInsight – Azure | Dokumentace Microsoftu"
description: "Přečtěte si o systému Apache Kafka ve službě HDInsight: co to je, co to dělá a kde najít příklady a informace pro začátek."
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
ms.openlocfilehash: 1976c52bd7fa56bb07104e205ab3699b2dfa4c50
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="introducing-apache-kafka-on-hdinsight-preview"></a><span data-ttu-id="05327-103">Představení Apache Kafka ve službě HDInsight (Preview)</span><span class="sxs-lookup"><span data-stu-id="05327-103">Introducing Apache Kafka on HDInsight (preview)</span></span>

<span data-ttu-id="05327-104">[Apache Kafka](https://kafka.apache.org) je open source distribuovaná streamovací platforma, kterou lze použít k vytváření aplikací a kanálů pro streamování dat v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="05327-104">[Apache Kafka](https://kafka.apache.org) is an open-source distributed streaming platform that can be used to build real-time streaming data pipelines and applications.</span></span> <span data-ttu-id="05327-105">Kafka také poskytuje funkci pro zprostředkování zpráv podobnou frontě zpráv, ve které můžete publikovat pojmenované datové proudy a přihlásit se k jejich odběru.</span><span class="sxs-lookup"><span data-stu-id="05327-105">Kafka also provides message broker functionality similar to a message queue, where you can publish and subscribe to named data streams.</span></span> <span data-ttu-id="05327-106">Kafka ve službě HDInsight představuje spravovanou, vysoce škálovatelnou a vysoce dostupnou službu v cloudu Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="05327-106">Kafka on HDInsight provides you with a managed, highly scalable, and highly available service in the Microsoft Azure cloud.</span></span>

## <a name="why-use-kafka-on-hdinsight"></a><span data-ttu-id="05327-107">Proč používat Kafka ve službě HDInsight?</span><span class="sxs-lookup"><span data-stu-id="05327-107">Why use Kafka on HDInsight?</span></span>

<span data-ttu-id="05327-108">Kafka poskytuje následující funkce:</span><span class="sxs-lookup"><span data-stu-id="05327-108">Kafka provides the following features:</span></span>

* <span data-ttu-id="05327-109">Vzorec zasílání zpráv na principu publikování a odběru: Kafka poskytuje rozhraní API pro autory k publikování záznamů v tématech Kafka.</span><span class="sxs-lookup"><span data-stu-id="05327-109">Publish-subscribe messaging pattern: Kafka provides a Producer API for publishing records to a Kafka topic.</span></span> <span data-ttu-id="05327-110">Rozhraní API pro příjemce se používá při přihlášení k odběru tématu.</span><span class="sxs-lookup"><span data-stu-id="05327-110">The Consumer API is used when subscribing to a topic.</span></span>

* <span data-ttu-id="05327-111">Zpracování datových proudů: Kafka se často používá společně s Apache Storm nebo Spark ke zpracování datových proudů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="05327-111">Stream processing: Kafka is often used with Apache Storm or Spark for real-time stream processing.</span></span> <span data-ttu-id="05327-112">Kafka 0.10.0.0 (HDInsight verze 3.5) zavedla rozhraní API pro streamování, které umožňuje vytvářet řešení streamování bez potřeby Stormu nebo Sparku.</span><span class="sxs-lookup"><span data-stu-id="05327-112">Kafka 0.10.0.0 (HDInsight version 3.5) introduced a streaming API that allows you to build streaming solutions without requiring Storm or Spark.</span></span>

* <span data-ttu-id="05327-113">Horizontální škálování: Kafka rozděluje datové proudy mezi uzly v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="05327-113">Horizontal scale: Kafka partitions streams across the nodes in the HDInsight cluster.</span></span> <span data-ttu-id="05327-114">Procesy příjemců můžou být přidružené k jednotlivým oddílům pro zajištění vyrovnávání zatížení při využívání záznamů.</span><span class="sxs-lookup"><span data-stu-id="05327-114">Consumer processes can be associated with individual partitions to provide load balancing when consuming records.</span></span>

* <span data-ttu-id="05327-115">Doručení v daném pořadí: V rámci každého oddílu se záznamy ukládají v datovém proudu v pořadí, v jakém byly přijaty.</span><span class="sxs-lookup"><span data-stu-id="05327-115">In-order delivery: Within each partition, records are stored in the stream in the order that they were received.</span></span> <span data-ttu-id="05327-116">Přidružením jednoho procesu příjemce na oddíl můžete zajistit zpracování záznamů v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="05327-116">By associating one consumer process per partition, you can guarantee that records are processed in-order.</span></span>

* <span data-ttu-id="05327-117">Odolnost proti chybám: Oddíly je možné replikovat mezi uzly a zajistit tak odolnost proti chybám.</span><span class="sxs-lookup"><span data-stu-id="05327-117">Fault-tolerant: Partitions can be replicated between nodes to provide fault tolerance.</span></span>

* <span data-ttu-id="05327-118">Integrace se službou Azure Managed Disks: Spravované disky přináší virtuálním počítačům v clusteru HDInsight lepší škálování a vyšší propustnost.</span><span class="sxs-lookup"><span data-stu-id="05327-118">Integration with Azure Managed Disks: Managed disks provides higher scale and throughput for the disks used by the virtual machines in the HDInsight cluster.</span></span>

    <span data-ttu-id="05327-119">Správa disků je v systému Kafka v HDInsight ve výchozím nastavení zapnutá a počet disků v jednotlivých uzlech můžete nastavit při vytváření prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="05327-119">Managed disks are enabled by default for Kafka on HDInsight, and the number of disks used per node can be configured during HDInsight creation.</span></span> <span data-ttu-id="05327-120">Další informace o spravovaných discích najdete v tématu [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="05327-120">For more information on managed disks, see [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md).</span></span>

    <span data-ttu-id="05327-121">Informace o konfiguraci spravovaných disků v systému Kafka v HDInsight najdete v tématu [Zvýšení škálovatelnosti systému Kafka v prostředí HDInsight](hdinsight-apache-kafka-scalability.md).</span><span class="sxs-lookup"><span data-stu-id="05327-121">For information on configuring managed disks with Kafka on HDInsight, see [Increase scalability of Kafka on HDInsight](hdinsight-apache-kafka-scalability.md).</span></span>

## <a name="use-cases"></a><span data-ttu-id="05327-122">Případy použití</span><span class="sxs-lookup"><span data-stu-id="05327-122">Use cases</span></span>

* <span data-ttu-id="05327-123">**Zasílání zpráv:** Protože Kafka podporuje vzorec zasílání zpráv na principu publikování a odběru, často se používá jako zprostředkovatel zpráv.</span><span class="sxs-lookup"><span data-stu-id="05327-123">**Messaging**: Since it supports the publish-subscribe message pattern, Kafka is often used as a message broker.</span></span>

* <span data-ttu-id="05327-124">**Sledování aktivit:** Protože Kafka poskytuje protokolování záznamů v daném pořadí, je možné ji použít ke sledování a opětovnému vytvoření aktivit.</span><span class="sxs-lookup"><span data-stu-id="05327-124">**Activity tracking**: Since Kafka provides in-order logging of records, it can be used to track and re-create activities.</span></span> <span data-ttu-id="05327-125">Například akcí uživatelů na webu nebo v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="05327-125">For example, user actions on a web site or within an application.</span></span>

* <span data-ttu-id="05327-126">**Agregace:** Pomocí zpracování datových proudů můžete agregovat a kombinovat informace z různých datových proudů a centralizovat je v podobě provozních dat.</span><span class="sxs-lookup"><span data-stu-id="05327-126">**Aggregation**: Using stream processing, you can aggregate information from different streams to combine and centralize the information into operational data.</span></span>

* <span data-ttu-id="05327-127">**Transformace:** Pomocí zpracování datových proudů můžete kombinovat data z více vstupních témat a rozšiřovat je do jednoho nebo několika výstupních témat.</span><span class="sxs-lookup"><span data-stu-id="05327-127">**Transformation**: Using stream processing, you can combine and enrich data from multiple input topics into one or more output topics.</span></span>

## <a name="next-steps"></a><span data-ttu-id="05327-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="05327-128">Next steps</span></span>

<span data-ttu-id="05327-129">Následující odkazy popisují používání Apache Kafka ve službě HDInsight:</span><span class="sxs-lookup"><span data-stu-id="05327-129">Use the following links to learn how to use Apache Kafka on HDInsight:</span></span>

* [<span data-ttu-id="05327-130">Začínáme s Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="05327-130">Get started with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)

* [<span data-ttu-id="05327-131">Vytvoření repliky Kafka ve službě HDInsight pomocí MirrorMakeru</span><span class="sxs-lookup"><span data-stu-id="05327-131">Use MirrorMaker to create a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)

* [<span data-ttu-id="05327-132">Použití Apache Stormu se systémem Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="05327-132">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)

* [<span data-ttu-id="05327-133">Použití Apache Sparku se systémem Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="05327-133">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)

* [<span data-ttu-id="05327-134">Připojení k systému Kafka přes virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="05327-134">Connect to Kafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)