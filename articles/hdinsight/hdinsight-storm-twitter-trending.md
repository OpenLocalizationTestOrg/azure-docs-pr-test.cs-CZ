---
title: "aaaTwitter trendů témata s Apache Storm v HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse Trident toocreate Apache Storm topologie, která určuje témata trendů na Twitteru na základě hashtags."
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
ms.openlocfilehash: 0281b495d10833c63868b36856c96369b139c553
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a><span data-ttu-id="e1523-103">Určení Twitter trendů témata s Apache Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e1523-103">Determine Twitter trending topics with Apache Storm on HDInsight</span></span>

<span data-ttu-id="e1523-104">Zjistěte, jak toouse Trident toocreate topologie Storm, která určují trendů na Twitteru témata (hodnota hash značek).</span><span class="sxs-lookup"><span data-stu-id="e1523-104">Learn how toouse Trident toocreate a Storm topology that determines trending topics (hash tags) on Twitter.</span></span>

<span data-ttu-id="e1523-105">Trident má vysokou úroveň abstrakce, která poskytuje nástroje, jako je spojení, agregací, seskupování, funkce a filtry.</span><span class="sxs-lookup"><span data-stu-id="e1523-105">Trident is a high-level abstraction that provides tools such as joins, aggregations, grouping, functions, and filters.</span></span> <span data-ttu-id="e1523-106">Kromě toho Trident přidává primitiv pro provádění stavová, přírůstkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="e1523-106">Additionally, Trident adds primitives for doing stateful, incremental processing.</span></span> <span data-ttu-id="e1523-107">Hello příkladu v tomto dokumentu je Trident topologie s vlastní spout a funkce.</span><span class="sxs-lookup"><span data-stu-id="e1523-107">hello example used in this document is a Trident topology with a custom spout and function.</span></span> <span data-ttu-id="e1523-108">Také používá několik integrovaných funkcí poskytovaných Trident.</span><span class="sxs-lookup"><span data-stu-id="e1523-108">It also uses several built-in functions provided by Trident.</span></span>

## <a name="requirements"></a><span data-ttu-id="e1523-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e1523-109">Requirements</span></span>

* <span data-ttu-id="e1523-110"><a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java a hello JDK 1.8</a></span><span class="sxs-lookup"><span data-stu-id="e1523-110"><a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java and hello JDK 1.8</a></span></span>

* <span data-ttu-id="e1523-111"><a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a></span><span class="sxs-lookup"><span data-stu-id="e1523-111"><a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a></span></span>

* <span data-ttu-id="e1523-112"><a href="http://git-scm.com/" target="_blank">Git</a></span><span class="sxs-lookup"><span data-stu-id="e1523-112"><a href="http://git-scm.com/" target="_blank">Git</a></span></span>

* <span data-ttu-id="e1523-113">Účet pro vývojáře služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="e1523-113">A Twitter developer account</span></span>

## <a name="download-hello-project"></a><span data-ttu-id="e1523-114">Stáhněte si projekt hello</span><span class="sxs-lookup"><span data-stu-id="e1523-114">Download hello project</span></span>

<span data-ttu-id="e1523-115">Použijte následující kód tooclone hello projektu místně hello.</span><span class="sxs-lookup"><span data-stu-id="e1523-115">Use hello following code tooclone hello project locally.</span></span>

    git clone https://github.com/Blackmist/TwitterTrending

## <a name="understanding-hello-topology"></a><span data-ttu-id="e1523-116">Principy hello topologie</span><span class="sxs-lookup"><span data-stu-id="e1523-116">Understanding hello topology</span></span>

<span data-ttu-id="e1523-117">Hello následující obrázek ukazuje z tok dat prostřednictvím Tato topologie:</span><span class="sxs-lookup"><span data-stu-id="e1523-117">hello following diagram shows of how data flows through this topology:</span></span>

![topologie](./media/hdinsight-storm-twitter-trending/trident.png)

> [!NOTE]
> <span data-ttu-id="e1523-119">Tento diagram je zjednodušený přehled topologie hello.</span><span class="sxs-lookup"><span data-stu-id="e1523-119">This diagram is a simplified view of hello topology.</span></span> <span data-ttu-id="e1523-120">Více instancí hello součásti jsou rozmístěny v hello uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="e1523-120">Multiple instances of hello components are distributed across hello nodes in hello cluster.</span></span>


<span data-ttu-id="e1523-121">Hello Trident kód, který implementuje hello topologie je následující:</span><span class="sxs-lookup"><span data-stu-id="e1523-121">hello Trident code that implements hello topology is as follows:</span></span>

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

<span data-ttu-id="e1523-122">Tento kód provede hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="e1523-122">This code performs hello following actions:</span></span>

1. <span data-ttu-id="e1523-123">Vytvoří datový proud z funkcí spout hello.</span><span class="sxs-lookup"><span data-stu-id="e1523-123">Creates a stream from hello spout.</span></span> <span data-ttu-id="e1523-124">Hello spout načte tweetů z Twitteru a filtry je pro konkrétní klíčová slova (láska, Hudba a kávy v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="e1523-124">hello spout retrieves tweets from Twitter, and filters them for specific keywords (love, music, and coffee in this example).</span></span>

2. <span data-ttu-id="e1523-125">HashtagExtractor, vlastní funkci, je použít tooextract hash značky z každé tweet.</span><span class="sxs-lookup"><span data-stu-id="e1523-125">HashtagExtractor, a custom function, is used tooextract hash tags from each tweet.</span></span> <span data-ttu-id="e1523-126">Hello hash značky jsou emitovaného toohello datového proudu.</span><span class="sxs-lookup"><span data-stu-id="e1523-126">hello hash tags are emitted toohello stream.</span></span>

3. <span data-ttu-id="e1523-127">datový proud Hello je seskupené podle značky hash a předán tooan agregátoru.</span><span class="sxs-lookup"><span data-stu-id="e1523-127">hello stream is grouped by hash tag, and passed tooan aggregator.</span></span> <span data-ttu-id="e1523-128">Tento agregátor vytvoří počet kolikrát jednotlivé značky hash došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="e1523-128">This aggregator creates a count of how many times each hash tag has occurred.</span></span> <span data-ttu-id="e1523-129">Tato data uložena v paměti.</span><span class="sxs-lookup"><span data-stu-id="e1523-129">This data is persisted in memory.</span></span> <span data-ttu-id="e1523-130">Nakonec nového datového proudu jsou vydávány obsahující hello hash značky a počet hello.</span><span class="sxs-lookup"><span data-stu-id="e1523-130">Finally, a new stream is emitted that contains hello hash tag and hello count.</span></span>

4. <span data-ttu-id="e1523-131">Hello **FirstN** sestavení je použité tooreturn pouze hello prvních 10 hodnoty, podle pole s počtem hello.</span><span class="sxs-lookup"><span data-stu-id="e1523-131">hello **FirstN** assembly is applied tooreturn only hello top 10 values, based on hello count field.</span></span>

> [!NOTE]
> <span data-ttu-id="e1523-132">Další informace o práci s Trident naleznete v tématu hello [přehled Trident API](http://storm.apache.org/releases/current/Trident-API-Overview.html) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="e1523-132">For more information on working with Trident, see hello [Trident API overview](http://storm.apache.org/releases/current/Trident-API-Overview.html) document.</span></span>

### <a name="hello-spout"></a><span data-ttu-id="e1523-133">Hello spout</span><span class="sxs-lookup"><span data-stu-id="e1523-133">hello spout</span></span>

<span data-ttu-id="e1523-134">Hello spout **TwitterSpout**, používá [Twitter4j](http://twitter4j.org/en/) tooretrieve tweetů ze služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="e1523-134">hello spout, **TwitterSpout**, uses [Twitter4j](http://twitter4j.org/en/) tooretrieve tweets from Twitter.</span></span> <span data-ttu-id="e1523-135">Filtr se vytvoří pro hello slova __rádi__, **Hudba**, a **kávy**.</span><span class="sxs-lookup"><span data-stu-id="e1523-135">A filter is created for hello words __love__, **music**, and **coffee**.</span></span> <span data-ttu-id="e1523-136">Příchozí tweetů (stav), které odpovídají filtru hello jsou uloženy v propojených blokování fronty.</span><span class="sxs-lookup"><span data-stu-id="e1523-136">Incoming tweets (status) that match hello filter are stored in a linked blocking queue.</span></span> <span data-ttu-id="e1523-137">Nakonec položky jsou vyžádány vypnout hello fronty a vygenerované toohello topologie.</span><span class="sxs-lookup"><span data-stu-id="e1523-137">Finally, items are pulled off hello queue and emitted toohello topology.</span></span>

### <a name="hello-hashtagextractor"></a><span data-ttu-id="e1523-138">Hello HashtagExtractor</span><span class="sxs-lookup"><span data-stu-id="e1523-138">hello HashtagExtractor</span></span>

<span data-ttu-id="e1523-139">tooextract hash značky, [getHashtagEntities](http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--) je použité tooretrieve všechny značky hash, které jsou obsaženy v hello tweet.</span><span class="sxs-lookup"><span data-stu-id="e1523-139">tooextract hash tags, [getHashtagEntities](http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--) is used tooretrieve all hash tags that are contained in hello tweet.</span></span> <span data-ttu-id="e1523-140">Jedná se tedy emitovaného toohello datového proudu.</span><span class="sxs-lookup"><span data-stu-id="e1523-140">These are then emitted toohello stream.</span></span>

## <a name="configure-twitter"></a><span data-ttu-id="e1523-141">Konfigurace služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="e1523-141">Configure Twitter</span></span>

<span data-ttu-id="e1523-142">Použijte hello následující kroky tooregister novou aplikaci služby Twitter a získat token informace potřebné tooread hello příjemce a přístup z Twitteru:</span><span class="sxs-lookup"><span data-stu-id="e1523-142">Use hello following steps tooregister a new Twitter application and obtain hello consumer and access token information needed tooread from Twitter:</span></span>

1. <span data-ttu-id="e1523-143">Přejděte příliš[Twitter aplikace](https://apps.twitter.com) a klikněte na tlačítko hello **vytvořit novou aplikaci** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e1523-143">Go too[Twitter Apps](https://apps.twitter.com) and click hello **Create new app** button.</span></span> <span data-ttu-id="e1523-144">Po vyplnění formuláře hello, nechte hello **adresu URL zpětné volání** pole prázdná.</span><span class="sxs-lookup"><span data-stu-id="e1523-144">When filling in hello form, leave hello **Callback URL** field empty.</span></span>

2. <span data-ttu-id="e1523-145">Po vytvoření aplikace hello klikněte na tlačítko hello **klíče a přístupové tokeny** kartě.</span><span class="sxs-lookup"><span data-stu-id="e1523-145">When hello app is created, click hello **Keys and Access Tokens** tab.</span></span>

3. <span data-ttu-id="e1523-146">Kopírování hello **uživatelský klíč** a **uživatelský tajný klíč** informace.</span><span class="sxs-lookup"><span data-stu-id="e1523-146">Copy hello **Consumer Key** and **Consumer Secret** information.</span></span>

4. <span data-ttu-id="e1523-147">V dolní části hello hello stránky, vyberte **vytvořit můj přístupový token** Pokud neexistují žádné tokeny.</span><span class="sxs-lookup"><span data-stu-id="e1523-147">At hello bottom of hello page, select **Create my access token** if no tokens exist.</span></span> <span data-ttu-id="e1523-148">Pokud byly vytvořeny hello tokeny, kopie hello **tokenu přístupu** a **tajný klíč přístupového tokenu** informace.</span><span class="sxs-lookup"><span data-stu-id="e1523-148">When hello tokens have been created, copy hello **Access Token** and **Access Token Secret** information.</span></span>

5. <span data-ttu-id="e1523-149">V hello **TwitterSpoutTopology** projektu jste dříve klonovaný, otevřete hello **resources/twitter4j.properties** soubor, přidejte hello informace, které jste shromáždili v předchozích krocích hello a pak uložte soubor hello .</span><span class="sxs-lookup"><span data-stu-id="e1523-149">In hello **TwitterSpoutTopology** project you previously cloned, open hello **resources/twitter4j.properties** file, add hello information you gathered in hello previous steps, and then save hello file.</span></span>

## <a name="build-hello-topology"></a><span data-ttu-id="e1523-150">Sestavení hello topologie</span><span class="sxs-lookup"><span data-stu-id="e1523-150">Build hello topology</span></span>

<span data-ttu-id="e1523-151">Použití hello následující kód toobuild hello projektu:</span><span class="sxs-lookup"><span data-stu-id="e1523-151">Use hello following code toobuild hello project:</span></span>

        cd [directoryname]
        mvn compile

## <a name="test-hello-topology"></a><span data-ttu-id="e1523-152">Test hello topologie</span><span class="sxs-lookup"><span data-stu-id="e1523-152">Test hello topology</span></span>

<span data-ttu-id="e1523-153">Použijte následující příkaz tootest hello topologie místně hello:</span><span class="sxs-lookup"><span data-stu-id="e1523-153">Use hello following command tootest hello topology locally:</span></span>

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

<span data-ttu-id="e1523-154">Po spuštění hello topologie, měli byste vidět, informace o ladění, který obsahuje hodnoty hash hello značky a počty emitovaného podle topologie hello.</span><span class="sxs-lookup"><span data-stu-id="e1523-154">After hello topology starts, you should see debug information that contains hello hash tags and counts emitted by hello topology.</span></span> <span data-ttu-id="e1523-155">výstup Hello by měla vypadat podobně jako toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="e1523-155">hello output should appear similar toohello following text:</span></span>

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

## <a name="next-steps"></a><span data-ttu-id="e1523-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e1523-156">Next steps</span></span>

<span data-ttu-id="e1523-157">Teď, když testování hello topologie místně, zjistit, jak toodeploy hello topologie: [nasazení a správa topologií Apache Storm v HDInsight](hdinsight-storm-deploy-monitor-topology.md).</span><span class="sxs-lookup"><span data-stu-id="e1523-157">Now that you have tested hello topology locally, discover how toodeploy hello topology: [Deploy and manage Apache Storm topologies on HDInsight](hdinsight-storm-deploy-monitor-topology.md).</span></span>

<span data-ttu-id="e1523-158">Může také by vás také zajímat v následujících tématech Storm hello:</span><span class="sxs-lookup"><span data-stu-id="e1523-158">You may also be interested in hello following Storm topics:</span></span>

* [<span data-ttu-id="e1523-159">Vývoj topologie Java pro Storm v HDInsight pomocí nástroje Maven</span><span class="sxs-lookup"><span data-stu-id="e1523-159">Develop Java topologies for Storm on HDInsight using Maven</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="e1523-160">Vývoj C# topologií pro Storm v HDInsight pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1523-160">Develop C# topologies for Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

<span data-ttu-id="e1523-161">Další příklady Storm pro HDInsight:</span><span class="sxs-lookup"><span data-stu-id="e1523-161">For more Storm examples for HDInsight:</span></span>

* [<span data-ttu-id="e1523-162">Příklad topologií pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e1523-162">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)

