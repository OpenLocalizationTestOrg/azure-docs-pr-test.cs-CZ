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
# <a name="azure-event-hubs-capture"></a><span data-ttu-id="baf02-103">Zachycení Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="baf02-103">Azure Event Hubs Capture</span></span>

<span data-ttu-id="baf02-104">Zaznamenat Azure Event Hubs vám umožní hello doručit tooautomatically streamování dat ve službě Event Hubs tooan [úložiště objektů Azure Blob](https://azure.microsoft.com/services/storage/blobs/) nebo [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) flexibilitu přidání účtu zvoleného s hello zadávání intervalu jednorázově nebo velikost.</span><span class="sxs-lookup"><span data-stu-id="baf02-104">Azure Event Hubs Capture enables you tooautomatically deliver hello streaming data in Event Hubs tooan [Azure Blob storage](https://azure.microsoft.com/services/storage/blobs/) or [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) account of your choice, with hello added flexibility of specifying a time or size interval.</span></span> <span data-ttu-id="baf02-105">Nastavení zachycení je rychlé, neexistují žádné náklady na správu toorun ho a škáluje automaticky službou Event Hubs [jednotky propustnosti](event-hubs-features.md#capacity).</span><span class="sxs-lookup"><span data-stu-id="baf02-105">Setting up Capture is fast, there are no administrative costs toorun it, and it scales automatically with Event Hubs [throughput units](event-hubs-features.md#capacity).</span></span> <span data-ttu-id="baf02-106">Zaznamenat centra událostí je hello nejjednodušší způsob, jak tooload streamování dat do Azure a umožní vám toofocus pro zpracování dat, ne na sběr dat.</span><span class="sxs-lookup"><span data-stu-id="baf02-106">Event Hubs Capture is hello easiest way tooload streaming data into Azure, and enables you toofocus on data processing rather than on data capture.</span></span>

<span data-ttu-id="baf02-107">Zaznamenat centra událostí vám umožní tooprocess v reálném čase a na základě batch kanály na hello stejného datového proudu.</span><span class="sxs-lookup"><span data-stu-id="baf02-107">Event Hubs Capture enables you tooprocess real-time and batch-based pipelines on hello same stream.</span></span> <span data-ttu-id="baf02-108">To znamená, že můžete vytvářet řešení, která růst s časem vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="baf02-108">This means you can build solutions that grow with your needs over time.</span></span> <span data-ttu-id="baf02-109">Ať už vytváříte batch systémy dnes s přehled směrem pozdější zpracování v reálném čase, nebo chcete tooadd efektivní neaktivní trase tooan existujícího v reálném čase řešení, zaznamenat centra událostí je práce s streamování dat jednodušší.</span><span class="sxs-lookup"><span data-stu-id="baf02-109">Whether you're building batch-based systems today with an eye towards future real-time processing, or you want tooadd an efficient cold path tooan existing real-time solution, Event Hubs Capture makes working with streaming data easier.</span></span>

## <a name="how-event-hubs-capture-works"></a><span data-ttu-id="baf02-110">Jak funguje zaznamenat centra událostí</span><span class="sxs-lookup"><span data-stu-id="baf02-110">How Event Hubs Capture works</span></span>

<span data-ttu-id="baf02-111">Event Hubs je doba uchování trvanlivý vyrovnávací paměti pro příchozí telemetrická data, podobné tooa distribuované protokolu.</span><span class="sxs-lookup"><span data-stu-id="baf02-111">Event Hubs is a time-retention durable buffer for telemetry ingress, similar tooa distributed log.</span></span> <span data-ttu-id="baf02-112">Hello klíče tooscaling v Event Hubs je hello [modelu oddělených příjemců](event-hubs-features.md#partitions).</span><span class="sxs-lookup"><span data-stu-id="baf02-112">hello key tooscaling in Event Hubs is hello [partitioned consumer model](event-hubs-features.md#partitions).</span></span> <span data-ttu-id="baf02-113">Každý oddíl je segment nezávislé dat a obsazením nezávisle.</span><span class="sxs-lookup"><span data-stu-id="baf02-113">Each partition is an independent segment of data and is consumed independently.</span></span> <span data-ttu-id="baf02-114">V průběhu času, které tato data ages vypnout založené na dobu uchování konfigurovat hello.</span><span class="sxs-lookup"><span data-stu-id="baf02-114">Over time this data ages off, based on hello configurable retention period.</span></span> <span data-ttu-id="baf02-115">V důsledku toho se dané události rozbočovače nikdy získá "příliš úplná."</span><span class="sxs-lookup"><span data-stu-id="baf02-115">As a result, a given event hub never gets "too full."</span></span>

<span data-ttu-id="baf02-116">Zaznamenat centra událostí umožňuje vám toospecify vlastního účtu Azure Blob storage a kontejner nebo účet Azure Data Lake Store, což jsou použité toostore hello zaznamenat data.</span><span class="sxs-lookup"><span data-stu-id="baf02-116">Event Hubs Capture enables you toospecify your own Azure Blob storage account and container, or Azure Data Lake Store account, which are used toostore hello captured data.</span></span> <span data-ttu-id="baf02-117">Tyto účty může být v hello stejné oblasti jako vaše Centrum událostí nebo v jiné oblasti, přidání toohello flexibilitu hello funkci zachycení centra událostí.</span><span class="sxs-lookup"><span data-stu-id="baf02-117">These accounts can be in hello same region as your event hub or in another region, adding toohello flexibility of hello Event Hubs Capture feature.</span></span>

<span data-ttu-id="baf02-118">Zaznamenaná data je napsána v [Apache Avro] [ Apache Avro] formátu: compact, rychlá a binární formát, který poskytuje bohaté datové struktury vloženého schématu.</span><span class="sxs-lookup"><span data-stu-id="baf02-118">Captured data is written in [Apache Avro][Apache Avro] format: a compact, fast, binary format that provides rich data structures with inline schema.</span></span> <span data-ttu-id="baf02-119">Tento formát se často používá v ekosystému Hadoop hello, Stream Analytics a Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="baf02-119">This format is widely used in hello Hadoop ecosystem, Stream Analytics, and Azure Data Factory.</span></span> <span data-ttu-id="baf02-120">Další informace o práci s Avro najdete dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="baf02-120">More information about working with Avro is available later in this article.</span></span>

### <a name="capture-windowing"></a><span data-ttu-id="baf02-121">Zaznamenat okna</span><span class="sxs-lookup"><span data-stu-id="baf02-121">Capture windowing</span></span>

<span data-ttu-id="baf02-122">Zaznamenat centra událostí umožňuje tooset až zaznamenávání toocontrol okno.</span><span class="sxs-lookup"><span data-stu-id="baf02-122">Event Hubs Capture enables you tooset up a window toocontrol capturing.</span></span> <span data-ttu-id="baf02-123">Toto okno je minimální velikost a konfigurace času zásadám"první wins," což znamená, že, operace zachycení hello první příčiny aktivační události došlo.</span><span class="sxs-lookup"><span data-stu-id="baf02-123">This window is a minimum size and time configuration with a "first wins policy," meaning that hello first trigger encountered causes a capture operation.</span></span> <span data-ttu-id="baf02-124">Pokud máte 15 minut, 100 MB okna Sběr a odeslat 1 MB za sekundu, hello velikost okna aktivace před hello časový interval.</span><span class="sxs-lookup"><span data-stu-id="baf02-124">If you have a fifteen-minute, 100 MB capture window and send 1 MB per second, hello size window triggers before hello time window.</span></span> <span data-ttu-id="baf02-125">Každý oddíl zaznamená nezávisle a zapíše objekt blob bloku dokončené v době hello zachycení, s názvem hello dobu, na které hello došlo k zachycení intervalu.</span><span class="sxs-lookup"><span data-stu-id="baf02-125">Each partition captures independently and writes a completed block blob at hello time of capture, named for hello time at which hello capture interval was encountered.</span></span> <span data-ttu-id="baf02-126">zásady vytváření názvů Hello úložiště je následující:</span><span class="sxs-lookup"><span data-stu-id="baf02-126">hello storage naming convention is as follows:</span></span>

```
[namespace]/[event hub]/[partition]/[YYYY]/[MM]/[DD]/[HH]/[mm]/[ss]
```

### <a name="scaling-toothroughput-units"></a><span data-ttu-id="baf02-127">Jednotky škálování toothroughput</span><span class="sxs-lookup"><span data-stu-id="baf02-127">Scaling toothroughput units</span></span>

<span data-ttu-id="baf02-128">Provoz centra událostí řídí [jednotky propustnosti](event-hubs-features.md#capacity).</span><span class="sxs-lookup"><span data-stu-id="baf02-128">Event Hubs traffic is controlled by [throughput units](event-hubs-features.md#capacity).</span></span> <span data-ttu-id="baf02-129">Jedna jednotka propustnosti umožňuje 1 MB za sekundu nebo 1000 událostí za sekundu příjem příchozích dat a dvakrát toto množství odchozí.</span><span class="sxs-lookup"><span data-stu-id="baf02-129">A single throughput unit allows 1 MB per second or 1000 events per second of ingress and twice that amount of egress.</span></span> <span data-ttu-id="baf02-130">Standardní služby Event Hubs se dá nakonfigurovat s 1-20 jednotky propustnosti a další můžete zakoupit kvótu zvýšit [žádost o podporu][support request].</span><span class="sxs-lookup"><span data-stu-id="baf02-130">Standard Event Hubs can be configured with 1-20 throughput units, and you can purchase more with a quota increase [support request][support request].</span></span> <span data-ttu-id="baf02-131">Použití mimo vaší zakoupené jednotky propustnosti je omezen.</span><span class="sxs-lookup"><span data-stu-id="baf02-131">Usage beyond your purchased throughput units is throttled.</span></span> <span data-ttu-id="baf02-132">Zaznamenat centra událostí zkopíruje data přímo z hello interního úložiště služby Event Hubs, obcházení kvóty odchozí jednotky propustnosti a uložit vaše odchozí pro další zpracování čtečky, jako je například Stream Analytics nebo Spark.</span><span class="sxs-lookup"><span data-stu-id="baf02-132">Event Hubs Capture copies data directly from hello internal Event Hubs storage, bypassing throughput unit egress quotas and saving your egress for other processing readers, such as Stream Analytics or Spark.</span></span>

<span data-ttu-id="baf02-133">Po nakonfigurování zaznamenat centra událostí spustí automaticky při odeslání první událost a běžet dál.</span><span class="sxs-lookup"><span data-stu-id="baf02-133">Once configured, Event Hubs Capture runs automatically when you send your first event, and continues running.</span></span> <span data-ttu-id="baf02-134">toomake snazší pro vaše zpracování příjmu dat tooknow funkčního hello proces Event Hubs zapíše prázdné soubory po žádná data.</span><span class="sxs-lookup"><span data-stu-id="baf02-134">toomake it easier for your downstream processing tooknow that hello process is working, Event Hubs writes empty files when there is no data.</span></span> <span data-ttu-id="baf02-135">Tento proces zajišťuje předvídatelný cadence a značky, který může zadat vaše batch procesory.</span><span class="sxs-lookup"><span data-stu-id="baf02-135">This process provides a predictable cadence and marker that can feed your batch processors.</span></span>

## <a name="setting-up-event-hubs-capture"></a><span data-ttu-id="baf02-136">Nastavení zachycení centra událostí</span><span class="sxs-lookup"><span data-stu-id="baf02-136">Setting up Event Hubs Capture</span></span>

<span data-ttu-id="baf02-137">Zaznamenat lze nakonfigurovat v hello událostí okamžiku vytvoření centra pomocí hello [portál Azure](https://portal.azure.com), nebo pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="baf02-137">You can configure Capture at hello event hub creation time using hello [Azure portal](https://portal.azure.com), or using Azure Resource Manager templates.</span></span> <span data-ttu-id="baf02-138">Další informace najdete v tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="baf02-138">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="baf02-139">Povolit pomocí portálu Azure hello Capture centra událostí</span><span class="sxs-lookup"><span data-stu-id="baf02-139">Enable Event Hubs Capture using hello Azure portal</span></span>](event-hubs-capture-enable-through-portal.md)
- [<span data-ttu-id="baf02-140">Vytvoření oboru názvů Event Hubs s centra událostí a povolte zachycení pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="baf02-140">Create an Event Hubs namespace with an event hub and enable Capture using an Azure Resource Manager template</span></span>](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)

## <a name="exploring-hello-captured-files-and-working-with-avro"></a><span data-ttu-id="baf02-141">Prohlížení soubory hello zachytit a práci s Avro</span><span class="sxs-lookup"><span data-stu-id="baf02-141">Exploring hello captured files and working with Avro</span></span>

<span data-ttu-id="baf02-142">Zaznamenat centra událostí vytvoří soubory ve formátu Avro, jako je zadaný v hello nakonfigurovaný časový interval.</span><span class="sxs-lookup"><span data-stu-id="baf02-142">Event Hubs Capture creates files in Avro format, as specified on hello configured time window.</span></span> <span data-ttu-id="baf02-143">Tyto soubory můžete zobrazit v libovolného nástroje, jako [Azure Storage Explorer][Azure Storage Explorer].</span><span class="sxs-lookup"><span data-stu-id="baf02-143">You can view these files in any tool such as [Azure Storage Explorer][Azure Storage Explorer].</span></span> <span data-ttu-id="baf02-144">Můžete si stáhnout hello soubory místně toowork na ně.</span><span class="sxs-lookup"><span data-stu-id="baf02-144">You can download hello files locally toowork on them.</span></span>

<span data-ttu-id="baf02-145">soubory Hello vyprodukované zaznamenat centra událostí mít hello schématu Avro následující:</span><span class="sxs-lookup"><span data-stu-id="baf02-145">hello files produced by Event Hubs Capture have hello following Avro schema:</span></span>

![][3]

<span data-ttu-id="baf02-146">Soubory Avro tooexplore snadný způsob je pomocí hello [nástroje Avro] [ Avro Tools] jar z Apache.</span><span class="sxs-lookup"><span data-stu-id="baf02-146">An easy way tooexplore Avro files is by using hello [Avro Tools][Avro Tools] jar from Apache.</span></span> <span data-ttu-id="baf02-147">Po stažení této jar, uvidíte hello schéma konkrétní soubor Avro spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="baf02-147">After downloading this jar, you can see hello schema of a specific Avro file by running hello following command:</span></span>

```
java -jar avro-tools-1.8.2.jar getschema <name of capture file>
```

<span data-ttu-id="baf02-148">Tento příkaz vrátí</span><span class="sxs-lookup"><span data-stu-id="baf02-148">This command returns</span></span>

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

<span data-ttu-id="baf02-149">Můžete také používat nástroje Avro tooconvert hello tooJSON formát a provádět další zpracování.</span><span class="sxs-lookup"><span data-stu-id="baf02-149">You can also use Avro Tools tooconvert hello file tooJSON format and perform other processing.</span></span>

<span data-ttu-id="baf02-150">tooperform více pokročilé zpracování, stáhněte a nainstalujte Avro pro vaši volbu platformy.</span><span class="sxs-lookup"><span data-stu-id="baf02-150">tooperform more advanced processing, download and install Avro for your choice of platform.</span></span> <span data-ttu-id="baf02-151">V době psaní tohoto textu hello nejsou k dispozici pro C, C++, C implementace\#, Java, NodeJS, Perl, PHP, Python nebo Ruby.</span><span class="sxs-lookup"><span data-stu-id="baf02-151">At hello time of this writing, there are implementations available for C, C++, C\#, Java, NodeJS, Perl, PHP, Python, and Ruby.</span></span>

<span data-ttu-id="baf02-152">Dokončení Průvodce Začínáme pro má Apache Avro [Java] [ Java] a [Python][Python].</span><span class="sxs-lookup"><span data-stu-id="baf02-152">Apache Avro has complete Getting Started guides for [Java][Java] and [Python][Python].</span></span> <span data-ttu-id="baf02-153">Můžete si také přečíst hello [Začínáme se službou Event Hubs zaznamenat](event-hubs-capture-python.md) článku.</span><span class="sxs-lookup"><span data-stu-id="baf02-153">You can also read hello [Getting started with Event Hubs Capture](event-hubs-capture-python.md) article.</span></span>

## <a name="how-event-hubs-capture-is-charged"></a><span data-ttu-id="baf02-154">Jak je účtován zaznamenat centra událostí</span><span class="sxs-lookup"><span data-stu-id="baf02-154">How Event Hubs Capture is charged</span></span>

<span data-ttu-id="baf02-155">Zaznamenat centra událostí je – měření podle objemu podobně toothroughput jednotky: jako poplatek po hodinách.</span><span class="sxs-lookup"><span data-stu-id="baf02-155">Event Hubs Capture is metered similarly toothroughput units: as an hourly charge.</span></span> <span data-ttu-id="baf02-156">zdarma Hello je přímo úměrná toohello počet pro obor názvů hello nakoupených jednotek propustnosti.</span><span class="sxs-lookup"><span data-stu-id="baf02-156">hello charge is directly proportional toohello number of throughput units purchased for hello namespace.</span></span> <span data-ttu-id="baf02-157">Jednotky propustnosti jsou vyšší a zmenšit, zachycení událostí centra měřidla zvýšit nebo snížit tooprovide odpovídající výkonu.</span><span class="sxs-lookup"><span data-stu-id="baf02-157">As throughput units are increased and decreased, Event Hubs Capture meters increase and decrease tooprovide matching performance.</span></span> <span data-ttu-id="baf02-158">Hello měřidla nastat současně.</span><span class="sxs-lookup"><span data-stu-id="baf02-158">hello meters occur in tandem.</span></span> <span data-ttu-id="baf02-159">Podrobnosti o cenách najdete v části [cenách služby Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="baf02-159">For pricing details, see [Event Hubs pricing](https://azure.microsoft.com/pricing/details/event-hubs/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="baf02-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="baf02-160">Next steps</span></span>

<span data-ttu-id="baf02-161">Zaznamenat centra událostí jsou hello nejjednodušší způsob, jak tooget data do Azure.</span><span class="sxs-lookup"><span data-stu-id="baf02-161">Event Hubs Capture is hello easiest way tooget data into Azure.</span></span> <span data-ttu-id="baf02-162">Pomocí Azure Data Lake, Azure Data Factory a Azure HDInsight, můžete provést dávkové zpracování a dalších analytics pomocí známých nástrojů a platformy dle vlastního výběru, v jakémkoli měřítku potřebujete.</span><span class="sxs-lookup"><span data-stu-id="baf02-162">Using Azure Data Lake, Azure Data Factory, and Azure HDInsight, you can perform batch processing and other analytics using familiar tools and platforms of your choosing, at any scale you need.</span></span>

<span data-ttu-id="baf02-163">Další informace o službě Event Hubs návštěvou hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="baf02-163">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="baf02-164">Začínáme s odesílání a příjem událostí</span><span class="sxs-lookup"><span data-stu-id="baf02-164">Get started sending and receiving events</span></span>](event-hubs-dotnet-framework-getstarted-send.md)
* <span data-ttu-id="baf02-165">Úplná [ukázková aplikace, která používá službu Event Hubs][sample application that uses Event Hubs]</span><span class="sxs-lookup"><span data-stu-id="baf02-165">A complete [sample application that uses Event Hubs][sample application that uses Event Hubs]</span></span>
* <span data-ttu-id="baf02-166">[Přehled služby Event Hubs][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="baf02-166">[Event Hubs overview][Event Hubs overview]</span></span>

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
