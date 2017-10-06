---
title: aaaOverview z Azure Event Hubs zaznamenat | Microsoft Docs
description: "Zaznamenat telemetrická data s zaznamenat centra událostí"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e53cdeea-8a6a-474e-9f96-59d43c0e8562
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: sethm;darosa
ms.openlocfilehash: 0238cae712a0ed7bdf3e87ee93a069a553cb65df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-event-hubs-capture"></a>Zachycení Azure Event Hubs

Zaznamenat Azure Event Hubs vám umožní hello doručit tooautomatically streamování dat ve službě Event Hubs tooan [úložiště objektů Azure Blob](https://azure.microsoft.com/services/storage/blobs/) nebo [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) flexibilitu přidání účtu zvoleného s hello zadávání intervalu jednorázově nebo velikost. Nastavení zachycení je rychlé, neexistují žádné náklady na správu toorun ho a škáluje automaticky službou Event Hubs [jednotky propustnosti](event-hubs-features.md#capacity). Zaznamenat centra událostí je hello nejjednodušší způsob, jak tooload streamování dat do Azure a umožní vám toofocus pro zpracování dat, ne na sběr dat.

Zaznamenat centra událostí vám umožní tooprocess v reálném čase a na základě batch kanály na hello stejného datového proudu. To znamená, že můžete vytvářet řešení, která růst s časem vašim potřebám. Ať už vytváříte batch systémy dnes s přehled směrem pozdější zpracování v reálném čase, nebo chcete tooadd efektivní neaktivní trase tooan existujícího v reálném čase řešení, zaznamenat centra událostí je práce s streamování dat jednodušší.

## <a name="how-event-hubs-capture-works"></a>Jak funguje zaznamenat centra událostí

Event Hubs je doba uchování trvanlivý vyrovnávací paměti pro příchozí telemetrická data, podobné tooa distribuované protokolu. Hello klíče tooscaling v Event Hubs je hello [modelu oddělených příjemců](event-hubs-features.md#partitions). Každý oddíl je segment nezávislé dat a obsazením nezávisle. V průběhu času, které tato data ages vypnout založené na dobu uchování konfigurovat hello. V důsledku toho se dané události rozbočovače nikdy získá "příliš úplná."

Zaznamenat centra událostí umožňuje vám toospecify vlastního účtu Azure Blob storage a kontejner nebo účet Azure Data Lake Store, což jsou použité toostore hello zaznamenat data. Tyto účty může být v hello stejné oblasti jako vaše Centrum událostí nebo v jiné oblasti, přidání toohello flexibilitu hello funkci zachycení centra událostí.

Zaznamenaná data je napsána v [Apache Avro] [ Apache Avro] formátu: compact, rychlá a binární formát, který poskytuje bohaté datové struktury vloženého schématu. Tento formát se často používá v ekosystému Hadoop hello, Stream Analytics a Azure Data Factory. Další informace o práci s Avro najdete dále v tomto článku.

### <a name="capture-windowing"></a>Zaznamenat okna

Zaznamenat centra událostí umožňuje tooset až zaznamenávání toocontrol okno. Toto okno je minimální velikost a konfigurace času zásadám"první wins," což znamená, že, operace zachycení hello první příčiny aktivační události došlo. Pokud máte 15 minut, 100 MB okna Sběr a odeslat 1 MB za sekundu, hello velikost okna aktivace před hello časový interval. Každý oddíl zaznamená nezávisle a zapíše objekt blob bloku dokončené v době hello zachycení, s názvem hello dobu, na které hello došlo k zachycení intervalu. zásady vytváření názvů Hello úložiště je následující:

```
[namespace]/[event hub]/[partition]/[YYYY]/[MM]/[DD]/[HH]/[mm]/[ss]
```

### <a name="scaling-toothroughput-units"></a>Jednotky škálování toothroughput

Provoz centra událostí řídí [jednotky propustnosti](event-hubs-features.md#capacity). Jedna jednotka propustnosti umožňuje 1 MB za sekundu nebo 1000 událostí za sekundu příjem příchozích dat a dvakrát toto množství odchozí. Standardní služby Event Hubs se dá nakonfigurovat s 1-20 jednotky propustnosti a další můžete zakoupit kvótu zvýšit [žádost o podporu][support request]. Použití mimo vaší zakoupené jednotky propustnosti je omezen. Zaznamenat centra událostí zkopíruje data přímo z hello interního úložiště služby Event Hubs, obcházení kvóty odchozí jednotky propustnosti a uložit vaše odchozí pro další zpracování čtečky, jako je například Stream Analytics nebo Spark.

Po nakonfigurování zaznamenat centra událostí spustí automaticky při odeslání první událost a běžet dál. toomake snazší pro vaše zpracování příjmu dat tooknow funkčního hello proces Event Hubs zapíše prázdné soubory po žádná data. Tento proces zajišťuje předvídatelný cadence a značky, který může zadat vaše batch procesory.

## <a name="setting-up-event-hubs-capture"></a>Nastavení zachycení centra událostí

Zaznamenat lze nakonfigurovat v hello událostí okamžiku vytvoření centra pomocí hello [portál Azure](https://portal.azure.com), nebo pomocí šablony Azure Resource Manager. Další informace najdete v tématu hello následující články:

- [Povolit pomocí portálu Azure hello Capture centra událostí](event-hubs-capture-enable-through-portal.md)
- [Vytvoření oboru názvů Event Hubs s centra událostí a povolte zachycení pomocí šablony Azure Resource Manager](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)

## <a name="exploring-hello-captured-files-and-working-with-avro"></a>Prohlížení soubory hello zachytit a práci s Avro

Zaznamenat centra událostí vytvoří soubory ve formátu Avro, jako je zadaný v hello nakonfigurovaný časový interval. Tyto soubory můžete zobrazit v libovolného nástroje, jako [Azure Storage Explorer][Azure Storage Explorer]. Můžete si stáhnout hello soubory místně toowork na ně.

soubory Hello vyprodukované zaznamenat centra událostí mít hello schématu Avro následující:

![][3]

Soubory Avro tooexplore snadný způsob je pomocí hello [nástroje Avro] [ Avro Tools] jar z Apache. Po stažení této jar, uvidíte hello schéma konkrétní soubor Avro spuštěním hello následující příkaz:

```
java -jar avro-tools-1.8.2.jar getschema <name of capture file>
```

Tento příkaz vrátí

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

Můžete také používat nástroje Avro tooconvert hello tooJSON formát a provádět další zpracování.

tooperform více pokročilé zpracování, stáhněte a nainstalujte Avro pro vaši volbu platformy. V době psaní tohoto textu hello nejsou k dispozici pro C, C++, C implementace\#, Java, NodeJS, Perl, PHP, Python nebo Ruby.

Dokončení Průvodce Začínáme pro má Apache Avro [Java] [ Java] a [Python][Python]. Můžete si také přečíst hello [Začínáme se službou Event Hubs zaznamenat](event-hubs-capture-python.md) článku.

## <a name="how-event-hubs-capture-is-charged"></a>Jak je účtován zaznamenat centra událostí

Zaznamenat centra událostí je – měření podle objemu podobně toothroughput jednotky: jako poplatek po hodinách. zdarma Hello je přímo úměrná toohello počet pro obor názvů hello nakoupených jednotek propustnosti. Jednotky propustnosti jsou vyšší a zmenšit, zachycení událostí centra měřidla zvýšit nebo snížit tooprovide odpovídající výkonu. Hello měřidla nastat současně. Podrobnosti o cenách najdete v části [cenách služby Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/). 

## <a name="next-steps"></a>Další kroky

Zaznamenat centra událostí jsou hello nejjednodušší způsob, jak tooget data do Azure. Pomocí Azure Data Lake, Azure Data Factory a Azure HDInsight, můžete provést dávkové zpracování a dalších analytics pomocí známých nástrojů a platformy dle vlastního výběru, v jakémkoli měřítku potřebujete.

Další informace o službě Event Hubs návštěvou hello následující odkazy:

* [Začínáme s odesílání a příjem událostí](event-hubs-dotnet-framework-getstarted-send.md)
* Úplná [ukázková aplikace, která používá službu Event Hubs][sample application that uses Event Hubs]
* [Přehled služby Event Hubs][Event Hubs overview]

[Apache Avro]: http://avro.apache.org/
[support request]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[Azure Storage Explorer]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-capture-overview/event-hubs-capture3.png
[Avro Tools]: http://www-us.apache.org/dist/avro/avro-1.8.2/java/avro-tools-1.8.2.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
