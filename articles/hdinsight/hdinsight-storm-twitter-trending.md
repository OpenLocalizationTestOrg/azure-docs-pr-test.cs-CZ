---
title: "Twitter trendů témata s Apache Storm v HDInsight | Microsoft Docs"
description: "Další informace o použití Trident vytvořit Apache Storm topologie, která určuje témata trendů na Twitteru podle hashtags."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 63b280ea-5c07-4dc8-a35f-dccf5a96ba93
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/14/2017
ms.author: larryfr
ms.openlocfilehash: d588221586f151319436525c5098b0bb2694e5f9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a><span data-ttu-id="b5f2e-103">Určení Twitter trendů témata s Apache Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b5f2e-103">Determine Twitter trending topics with Apache Storm on HDInsight</span></span>

<span data-ttu-id="b5f2e-104">Zjistěte, jak používat Trident k vytváření topologie Storm, který určuje trendů témata (hodnota hash značky) na Twitteru.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-104">Learn how to use Trident to create a Storm topology that determines trending topics (hash tags) on Twitter.</span></span>

<span data-ttu-id="b5f2e-105">Trident má vysokou úroveň abstrakce, která poskytuje nástroje, jako je spojení, agregací, seskupování, funkce a filtry.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-105">Trident is a high-level abstraction that provides tools such as joins, aggregations, grouping, functions, and filters.</span></span> <span data-ttu-id="b5f2e-106">Kromě toho Trident přidává primitiv pro provádění stavová, přírůstkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-106">Additionally, Trident adds primitives for doing stateful, incremental processing.</span></span> <span data-ttu-id="b5f2e-107">V příkladu v tomto dokumentu je Trident topologie s vlastní spout a funkce.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-107">The example used in this document is a Trident topology with a custom spout and function.</span></span> <span data-ttu-id="b5f2e-108">Také používá několik integrovaných funkcí poskytovaných Trident.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-108">It also uses several built-in functions provided by Trident.</span></span>

## <a name="requirements"></a><span data-ttu-id="b5f2e-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b5f2e-109">Requirements</span></span>

* <span data-ttu-id="b5f2e-110"><a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java a JDK 1.8</a></span><span class="sxs-lookup"><span data-stu-id="b5f2e-110"><a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java and the JDK 1.8</a></span></span>

* <span data-ttu-id="b5f2e-111"><a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a></span><span class="sxs-lookup"><span data-stu-id="b5f2e-111"><a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a></span></span>

* <span data-ttu-id="b5f2e-112"><a href="http://git-scm.com/" target="_blank">Git</a></span><span class="sxs-lookup"><span data-stu-id="b5f2e-112"><a href="http://git-scm.com/" target="_blank">Git</a></span></span>

* <span data-ttu-id="b5f2e-113">Účet pro vývojáře služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-113">A Twitter developer account</span></span>

## <a name="download-the-project"></a><span data-ttu-id="b5f2e-114">Stažení projektu</span><span class="sxs-lookup"><span data-stu-id="b5f2e-114">Download the project</span></span>

<span data-ttu-id="b5f2e-115">Použijte následující kód klonovat projektu místně.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-115">Use the following code to clone the project locally.</span></span>

    git clone https://github.com/Blackmist/TwitterTrending

## <a name="understanding-the-topology"></a><span data-ttu-id="b5f2e-116">Principy topologie</span><span class="sxs-lookup"><span data-stu-id="b5f2e-116">Understanding the topology</span></span>

<span data-ttu-id="b5f2e-117">Následující diagram znázorňuje z tok dat prostřednictvím Tato topologie:</span><span class="sxs-lookup"><span data-stu-id="b5f2e-117">The following diagram shows of how data flows through this topology:</span></span>

![topologie](./media/hdinsight-storm-twitter-trending/trident.png)

> [!NOTE]
> <span data-ttu-id="b5f2e-119">Tento diagram je zjednodušený přehled topologie.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-119">This diagram is a simplified view of the topology.</span></span> <span data-ttu-id="b5f2e-120">Více instancí součásti jsou rozdělené mezi uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-120">Multiple instances of the components are distributed across the nodes in the cluster.</span></span>


<span data-ttu-id="b5f2e-121">Trident kód, který implementuje topologii vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="b5f2e-121">The Trident code that implements the topology is as follows:</span></span>

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

<span data-ttu-id="b5f2e-122">Tento kód provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="b5f2e-122">This code performs the following actions:</span></span>

1. <span data-ttu-id="b5f2e-123">Vytvoří datový proud z funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-123">Creates a stream from the spout.</span></span> <span data-ttu-id="b5f2e-124">Spout načte tweetů z Twitteru a filtry je pro konkrétní klíčová slova (láska, Hudba a kávy v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="b5f2e-124">The spout retrieves tweets from Twitter, and filters them for specific keywords (love, music, and coffee in this example).</span></span>

2. <span data-ttu-id="b5f2e-125">HashtagExtractor, vlastní funkci, slouží k extrakci hash značky z každé tweet.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-125">HashtagExtractor, a custom function, is used to extract hash tags from each tweet.</span></span> <span data-ttu-id="b5f2e-126">Hodnota hash značky jsou vydávány do datového proudu.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-126">The hash tags are emitted to the stream.</span></span>

3. <span data-ttu-id="b5f2e-127">Datový proud je seskupené podle značky hash a předaný agregátoru.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-127">The stream is grouped by hash tag, and passed to an aggregator.</span></span> <span data-ttu-id="b5f2e-128">Tento agregátor vytvoří počet kolikrát jednotlivé značky hash došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-128">This aggregator creates a count of how many times each hash tag has occurred.</span></span> <span data-ttu-id="b5f2e-129">Tato data uložena v paměti.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-129">This data is persisted in memory.</span></span> <span data-ttu-id="b5f2e-130">Nakonec nového datového proudu jsou vydávány obsahující značku hodnoty hash a počet.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-130">Finally, a new stream is emitted that contains the hash tag and the count.</span></span>

4. <span data-ttu-id="b5f2e-131">**FirstN** sestavení se použije pro vrátit pouze prvních 10 hodnoty, na základě pole count.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-131">The **FirstN** assembly is applied to return only the top 10 values, based on the count field.</span></span>

> [!NOTE]
> <span data-ttu-id="b5f2e-132">Další informace o práci s Trident naleznete v tématu [přehled Trident API](http://storm.apache.org/releases/current/Trident-API-Overview.html) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-132">For more information on working with Trident, see the [Trident API overview](http://storm.apache.org/releases/current/Trident-API-Overview.html) document.</span></span>

### <a name="the-spout"></a><span data-ttu-id="b5f2e-133">Spout</span><span class="sxs-lookup"><span data-stu-id="b5f2e-133">The spout</span></span>

<span data-ttu-id="b5f2e-134">Spout, **TwitterSpout**, používá [Twitter4j](http://twitter4j.org/en/) k načtení tweetů ze služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-134">The spout, **TwitterSpout**, uses [Twitter4j](http://twitter4j.org/en/) to retrieve tweets from Twitter.</span></span> <span data-ttu-id="b5f2e-135">Filtr se vytvoří pro slova __rádi__, **Hudba**, a **kávy**.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-135">A filter is created for the words __love__, **music**, and **coffee**.</span></span> <span data-ttu-id="b5f2e-136">Příchozí tweetů (stav), které odpovídají filtru jsou uloženy v propojených blokování fronty.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-136">Incoming tweets (status) that match the filter are stored in a linked blocking queue.</span></span> <span data-ttu-id="b5f2e-137">Nakonec jsou položky vyžádat vypnout fronty a vygenerované do topologie.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-137">Finally, items are pulled off the queue and emitted to the topology.</span></span>

### <a name="the-hashtagextractor"></a><span data-ttu-id="b5f2e-138">HashtagExtractor</span><span class="sxs-lookup"><span data-stu-id="b5f2e-138">The HashtagExtractor</span></span>

<span data-ttu-id="b5f2e-139">Extrahovat hodnotu hash značky [getHashtagEntities](http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--) se používá k načtení všechny značky hash, které jsou součástí tweet.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-139">To extract hash tags, [getHashtagEntities](http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--) is used to retrieve all hash tags that are contained in the tweet.</span></span> <span data-ttu-id="b5f2e-140">Tyto jsou pak vygenerované do datového proudu.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-140">These are then emitted to the stream.</span></span>

## <a name="configure-twitter"></a><span data-ttu-id="b5f2e-141">Konfigurace služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-141">Configure Twitter</span></span>

<span data-ttu-id="b5f2e-142">Zaregistrujte novou aplikaci služby Twitter a získat token informace příjemce a přístup ke čtení z Twitteru potřeba pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="b5f2e-142">Use the following steps to register a new Twitter application and obtain the consumer and access token information needed to read from Twitter:</span></span>

1. <span data-ttu-id="b5f2e-143">Přejděte na [Twitter aplikace](https://apps.twitter.com) a klikněte na **vytvořit novou aplikaci** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-143">Go to [Twitter Apps](https://apps.twitter.com) and click the **Create new app** button.</span></span> <span data-ttu-id="b5f2e-144">Po vyplnění formuláře, ponechte **adresu URL zpětné volání** pole prázdná.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-144">When filling in the form, leave the **Callback URL** field empty.</span></span>

2. <span data-ttu-id="b5f2e-145">Po vytvoření aplikace, klikněte na tlačítko **klíče a přístupové tokeny** kartě.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-145">When the app is created, click the **Keys and Access Tokens** tab.</span></span>

3. <span data-ttu-id="b5f2e-146">Kopírování **uživatelský klíč** a **uživatelský tajný klíč** informace.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-146">Copy the **Consumer Key** and **Consumer Secret** information.</span></span>

4. <span data-ttu-id="b5f2e-147">V dolní části stránky, vyberte **vytvořit můj přístupový token** Pokud neexistují žádné tokeny.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-147">At the bottom of the page, select **Create my access token** if no tokens exist.</span></span> <span data-ttu-id="b5f2e-148">Při tokeny byly vytvořeny, zkopírovat **tokenu přístupu** a **tajný klíč přístupového tokenu** informace.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-148">When the tokens have been created, copy the **Access Token** and **Access Token Secret** information.</span></span>

5. <span data-ttu-id="b5f2e-149">V **TwitterSpoutTopology** dříve klonovat, otevřete projekt **resources/twitter4j.properties** soubor, přidejte informace o shromážděných v předchozích krocích a pak soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-149">In the **TwitterSpoutTopology** project you previously cloned, open the **resources/twitter4j.properties** file, add the information you gathered in the previous steps, and then save the file.</span></span>

## <a name="build-the-topology"></a><span data-ttu-id="b5f2e-150">Sestavení topologie</span><span class="sxs-lookup"><span data-stu-id="b5f2e-150">Build the topology</span></span>

<span data-ttu-id="b5f2e-151">Použijte následující kód pro sestavení projektu:</span><span class="sxs-lookup"><span data-stu-id="b5f2e-151">Use the following code to build the project:</span></span>

        cd [directoryname]
        mvn compile

## <a name="test-the-topology"></a><span data-ttu-id="b5f2e-152">Testování topologie</span><span class="sxs-lookup"><span data-stu-id="b5f2e-152">Test the topology</span></span>

<span data-ttu-id="b5f2e-153">K testování topologie místně, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b5f2e-153">Use the following command to test the topology locally:</span></span>

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

<span data-ttu-id="b5f2e-154">Po spuštění topologii, měli byste vidět, informace o ladění, který obsahuje hodnotu hash značky a počty emitovaného podle topologie.</span><span class="sxs-lookup"><span data-stu-id="b5f2e-154">After the topology starts, you should see debug information that contains the hash tags and counts emitted by the topology.</span></span> <span data-ttu-id="b5f2e-155">Výstup by měl vypadat přibližně následující text:</span><span class="sxs-lookup"><span data-stu-id="b5f2e-155">The output should appear similar to the following text:</span></span>

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

## <a name="next-steps"></a><span data-ttu-id="b5f2e-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5f2e-156">Next steps</span></span>

<span data-ttu-id="b5f2e-157">Teď, když testování topologie místně, můžete zjistit, jak k nasazení topologie: [nasazení a správa topologií Apache Storm v HDInsight](hdinsight-storm-deploy-monitor-topology.md).</span><span class="sxs-lookup"><span data-stu-id="b5f2e-157">Now that you have tested the topology locally, discover how to deploy the topology: [Deploy and manage Apache Storm topologies on HDInsight](hdinsight-storm-deploy-monitor-topology.md).</span></span>

<span data-ttu-id="b5f2e-158">Může také by vás také zajímat v následujících tématech Storm:</span><span class="sxs-lookup"><span data-stu-id="b5f2e-158">You may also be interested in the following Storm topics:</span></span>

* [<span data-ttu-id="b5f2e-159">Vývoj topologie Java pro Storm v HDInsight pomocí nástroje Maven</span><span class="sxs-lookup"><span data-stu-id="b5f2e-159">Develop Java topologies for Storm on HDInsight using Maven</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="b5f2e-160">Vývoj C# topologií pro Storm v HDInsight pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b5f2e-160">Develop C# topologies for Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

<span data-ttu-id="b5f2e-161">Další příklady Storm pro HDInsight:</span><span class="sxs-lookup"><span data-stu-id="b5f2e-161">For more Storm examples for HDInsight:</span></span>

* [<span data-ttu-id="b5f2e-162">Příklad topologií pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b5f2e-162">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)

