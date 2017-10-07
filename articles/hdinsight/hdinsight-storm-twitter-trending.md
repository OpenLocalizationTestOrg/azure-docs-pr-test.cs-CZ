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
# <a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>Určení Twitter trendů témata s Apache Storm v HDInsight

Zjistěte, jak toouse Trident toocreate topologie Storm, která určují trendů na Twitteru témata (hodnota hash značek).

Trident má vysokou úroveň abstrakce, která poskytuje nástroje, jako je spojení, agregací, seskupování, funkce a filtry. Kromě toho Trident přidává primitiv pro provádění stavová, přírůstkové zpracování. Hello příkladu v tomto dokumentu je Trident topologie s vlastní spout a funkce. Také používá několik integrovaných funkcí poskytovaných Trident.

## <a name="requirements"></a>Požadavky

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java a hello JDK 1.8</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a>

* <a href="http://git-scm.com/" target="_blank">Git</a>

* Účet pro vývojáře služby Twitter.

## <a name="download-hello-project"></a>Stáhněte si projekt hello

Použijte následující kód tooclone hello projektu místně hello.

    git clone https://github.com/Blackmist/TwitterTrending

## <a name="understanding-hello-topology"></a>Principy hello topologie

Hello následující obrázek ukazuje z tok dat prostřednictvím Tato topologie:

![topologie](./media/hdinsight-storm-twitter-trending/trident.png)

> [!NOTE]
> Tento diagram je zjednodušený přehled topologie hello. Více instancí hello součásti jsou rozmístěny v hello uzly v clusteru hello.


Hello Trident kód, který implementuje hello topologie je následující:

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

Tento kód provede hello následující akce:

1. Vytvoří datový proud z funkcí spout hello. Hello spout načte tweetů z Twitteru a filtry je pro konkrétní klíčová slova (láska, Hudba a kávy v tomto příkladu).

2. HashtagExtractor, vlastní funkci, je použít tooextract hash značky z každé tweet. Hello hash značky jsou emitovaného toohello datového proudu.

3. datový proud Hello je seskupené podle značky hash a předán tooan agregátoru. Tento agregátor vytvoří počet kolikrát jednotlivé značky hash došlo k chybě. Tato data uložena v paměti. Nakonec nového datového proudu jsou vydávány obsahující hello hash značky a počet hello.

4. Hello **FirstN** sestavení je použité tooreturn pouze hello prvních 10 hodnoty, podle pole s počtem hello.

> [!NOTE]
> Další informace o práci s Trident naleznete v tématu hello [přehled Trident API](http://storm.apache.org/releases/current/Trident-API-Overview.html) dokumentu.

### <a name="hello-spout"></a>Hello spout

Hello spout **TwitterSpout**, používá [Twitter4j](http://twitter4j.org/en/) tooretrieve tweetů ze služby Twitter. Filtr se vytvoří pro hello slova __rádi__, **Hudba**, a **kávy**. Příchozí tweetů (stav), které odpovídají filtru hello jsou uloženy v propojených blokování fronty. Nakonec položky jsou vyžádány vypnout hello fronty a vygenerované toohello topologie.

### <a name="hello-hashtagextractor"></a>Hello HashtagExtractor

tooextract hash značky, [getHashtagEntities](http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--) je použité tooretrieve všechny značky hash, které jsou obsaženy v hello tweet. Jedná se tedy emitovaného toohello datového proudu.

## <a name="configure-twitter"></a>Konfigurace služby Twitter.

Použijte hello následující kroky tooregister novou aplikaci služby Twitter a získat token informace potřebné tooread hello příjemce a přístup z Twitteru:

1. Přejděte příliš[Twitter aplikace](https://apps.twitter.com) a klikněte na tlačítko hello **vytvořit novou aplikaci** tlačítko. Po vyplnění formuláře hello, nechte hello **adresu URL zpětné volání** pole prázdná.

2. Po vytvoření aplikace hello klikněte na tlačítko hello **klíče a přístupové tokeny** kartě.

3. Kopírování hello **uživatelský klíč** a **uživatelský tajný klíč** informace.

4. V dolní části hello hello stránky, vyberte **vytvořit můj přístupový token** Pokud neexistují žádné tokeny. Pokud byly vytvořeny hello tokeny, kopie hello **tokenu přístupu** a **tajný klíč přístupového tokenu** informace.

5. V hello **TwitterSpoutTopology** projektu jste dříve klonovaný, otevřete hello **resources/twitter4j.properties** soubor, přidejte hello informace, které jste shromáždili v předchozích krocích hello a pak uložte soubor hello .

## <a name="build-hello-topology"></a>Sestavení hello topologie

Použití hello následující kód toobuild hello projektu:

        cd [directoryname]
        mvn compile

## <a name="test-hello-topology"></a>Test hello topologie

Použijte následující příkaz tootest hello topologie místně hello:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

Po spuštění hello topologie, měli byste vidět, informace o ladění, který obsahuje hodnoty hash hello značky a počty emitovaného podle topologie hello. výstup Hello by měla vypadat podobně jako toohello následující text:

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

## <a name="next-steps"></a>Další kroky

Teď, když testování hello topologie místně, zjistit, jak toodeploy hello topologie: [nasazení a správa topologií Apache Storm v HDInsight](hdinsight-storm-deploy-monitor-topology.md).

Může také by vás také zajímat v následujících tématech Storm hello:

* [Vývoj topologie Java pro Storm v HDInsight pomocí nástroje Maven](hdinsight-storm-develop-java-topology.md)
* [Vývoj C# topologií pro Storm v HDInsight pomocí sady Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Další příklady Storm pro HDInsight:

* [Příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)

