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
# <a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>Určení Twitter trendů témata s Apache Storm v HDInsight

Zjistěte, jak používat Trident k vytváření topologie Storm, který určuje trendů témata (hodnota hash značky) na Twitteru.

Trident má vysokou úroveň abstrakce, která poskytuje nástroje, jako je spojení, agregací, seskupování, funkce a filtry. Kromě toho Trident přidává primitiv pro provádění stavová, přírůstkové zpracování. V příkladu v tomto dokumentu je Trident topologie s vlastní spout a funkce. Také používá několik integrovaných funkcí poskytovaných Trident.

## <a name="requirements"></a>Požadavky

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java a JDK 1.8</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a>

* <a href="http://git-scm.com/" target="_blank">Git</a>

* Účet pro vývojáře služby Twitter.

## <a name="download-the-project"></a>Stažení projektu

Použijte následující kód klonovat projektu místně.

    git clone https://github.com/Blackmist/TwitterTrending

## <a name="understanding-the-topology"></a>Principy topologie

Následující diagram znázorňuje z tok dat prostřednictvím Tato topologie:

![topologie](./media/hdinsight-storm-twitter-trending/trident.png)

> [!NOTE]
> Tento diagram je zjednodušený přehled topologie. Více instancí součásti jsou rozdělené mezi uzly v clusteru.


Trident kód, který implementuje topologii vypadá takto:

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

Tento kód provede následující akce:

1. Vytvoří datový proud z funkcí spout. Spout načte tweetů z Twitteru a filtry je pro konkrétní klíčová slova (láska, Hudba a kávy v tomto příkladu).

2. HashtagExtractor, vlastní funkci, slouží k extrakci hash značky z každé tweet. Hodnota hash značky jsou vydávány do datového proudu.

3. Datový proud je seskupené podle značky hash a předaný agregátoru. Tento agregátor vytvoří počet kolikrát jednotlivé značky hash došlo k chybě. Tato data uložena v paměti. Nakonec nového datového proudu jsou vydávány obsahující značku hodnoty hash a počet.

4. **FirstN** sestavení se použije pro vrátit pouze prvních 10 hodnoty, na základě pole count.

> [!NOTE]
> Další informace o práci s Trident naleznete v tématu [přehled Trident API](http://storm.apache.org/releases/current/Trident-API-Overview.html) dokumentu.

### <a name="the-spout"></a>Spout

Spout, **TwitterSpout**, používá [Twitter4j](http://twitter4j.org/en/) k načtení tweetů ze služby Twitter. Filtr se vytvoří pro slova __rádi__, **Hudba**, a **kávy**. Příchozí tweetů (stav), které odpovídají filtru jsou uloženy v propojených blokování fronty. Nakonec jsou položky vyžádat vypnout fronty a vygenerované do topologie.

### <a name="the-hashtagextractor"></a>HashtagExtractor

Extrahovat hodnotu hash značky [getHashtagEntities](http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--) se používá k načtení všechny značky hash, které jsou součástí tweet. Tyto jsou pak vygenerované do datového proudu.

## <a name="configure-twitter"></a>Konfigurace služby Twitter.

Zaregistrujte novou aplikaci služby Twitter a získat token informace příjemce a přístup ke čtení z Twitteru potřeba pomocí následujících kroků:

1. Přejděte na [Twitter aplikace](https://apps.twitter.com) a klikněte na **vytvořit novou aplikaci** tlačítko. Po vyplnění formuláře, ponechte **adresu URL zpětné volání** pole prázdná.

2. Po vytvoření aplikace, klikněte na tlačítko **klíče a přístupové tokeny** kartě.

3. Kopírování **uživatelský klíč** a **uživatelský tajný klíč** informace.

4. V dolní části stránky, vyberte **vytvořit můj přístupový token** Pokud neexistují žádné tokeny. Při tokeny byly vytvořeny, zkopírovat **tokenu přístupu** a **tajný klíč přístupového tokenu** informace.

5. V **TwitterSpoutTopology** dříve klonovat, otevřete projekt **resources/twitter4j.properties** soubor, přidejte informace o shromážděných v předchozích krocích a pak soubor uložte.

## <a name="build-the-topology"></a>Sestavení topologie

Použijte následující kód pro sestavení projektu:

        cd [directoryname]
        mvn compile

## <a name="test-the-topology"></a>Testování topologie

K testování topologie místně, použijte následující příkaz:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

Po spuštění topologii, měli byste vidět, informace o ladění, který obsahuje hodnotu hash značky a počty emitovaného podle topologie. Výstup by měl vypadat přibližně následující text:

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

Teď, když testování topologie místně, můžete zjistit, jak k nasazení topologie: [nasazení a správa topologií Apache Storm v HDInsight](hdinsight-storm-deploy-monitor-topology.md).

Může také by vás také zajímat v následujících tématech Storm:

* [Vývoj topologie Java pro Storm v HDInsight pomocí nástroje Maven](hdinsight-storm-develop-java-topology.md)
* [Vývoj C# topologií pro Storm v HDInsight pomocí sady Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Další příklady Storm pro HDInsight:

* [Příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)

