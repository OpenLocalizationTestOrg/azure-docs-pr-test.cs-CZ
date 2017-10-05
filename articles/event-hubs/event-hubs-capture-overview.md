---
title: "Přehled služby Azure Event Hubs zaznamenat | Microsoft Docs"
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
ms.openlocfilehash: 9ae6aa57200b99f382c6e60565db9cfc69f1d3c6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-event-hubs-capture"></a><span data-ttu-id="9bf95-103">Zachycení Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="9bf95-103">Azure Event Hubs Capture</span></span>

<span data-ttu-id="9bf95-104">Zaznamenat Azure Event Hubs umožňuje automaticky doručovat dat v centra událostí pro [úložiště objektů Azure Blob](https://azure.microsoft.com/services/storage/blobs/) nebo [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) účet podle svého výběru s flexibilnější zadání intervalu jednorázově nebo velikost.</span><span class="sxs-lookup"><span data-stu-id="9bf95-104">Azure Event Hubs Capture enables you to automatically deliver the streaming data in Event Hubs to an [Azure Blob storage](https://azure.microsoft.com/services/storage/blobs/) or [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) account of your choice, with the added flexibility of specifying a time or size interval.</span></span> <span data-ttu-id="9bf95-105">Nastavení zachycení je rychlé, neexistují žádné náklady na správu spouštět a automaticky přizpůsobí službou Event Hubs [jednotky propustnosti](event-hubs-features.md#capacity).</span><span class="sxs-lookup"><span data-stu-id="9bf95-105">Setting up Capture is fast, there are no administrative costs to run it, and it scales automatically with Event Hubs [throughput units](event-hubs-features.md#capacity).</span></span> <span data-ttu-id="9bf95-106">Zaznamenat centra událostí je nejjednodušší způsob, jak načtení dat do Azure a umožní vám zaměřit se na zpracování dat, ne na sběr dat.</span><span class="sxs-lookup"><span data-stu-id="9bf95-106">Event Hubs Capture is the easiest way to load streaming data into Azure, and enables you to focus on data processing rather than on data capture.</span></span>

<span data-ttu-id="9bf95-107">Zaznamenat centra událostí můžete ke zpracování v reálném čase a na základě batch kanály pro stejný datový proud.</span><span class="sxs-lookup"><span data-stu-id="9bf95-107">Event Hubs Capture enables you to process real-time and batch-based pipelines on the same stream.</span></span> <span data-ttu-id="9bf95-108">To znamená, že můžete vytvářet řešení, která růst s časem vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="9bf95-108">This means you can build solutions that grow with your needs over time.</span></span> <span data-ttu-id="9bf95-109">Ať už vytváříte batch systémy dnes s přehled směrem pozdější zpracování v reálném čase, nebo chcete přidat do existujícího řešení v reálném čase efektivní neaktivní trase, zaznamenat centra událostí je práce s streamování dat jednodušší.</span><span class="sxs-lookup"><span data-stu-id="9bf95-109">Whether you're building batch-based systems today with an eye towards future real-time processing, or you want to add an efficient cold path to an existing real-time solution, Event Hubs Capture makes working with streaming data easier.</span></span>

## <a name="how-event-hubs-capture-works"></a><span data-ttu-id="9bf95-110">Jak funguje zaznamenat centra událostí</span><span class="sxs-lookup"><span data-stu-id="9bf95-110">How Event Hubs Capture works</span></span>

<span data-ttu-id="9bf95-111">Event Hubs je doba uchování trvanlivý vyrovnávací paměti pro příchozí telemetrická data, podobně jako distribuované protokolu.</span><span class="sxs-lookup"><span data-stu-id="9bf95-111">Event Hubs is a time-retention durable buffer for telemetry ingress, similar to a distributed log.</span></span> <span data-ttu-id="9bf95-112">Klíčem k příjmu ve službě Event Hubs je [modelu oddělených příjemců](event-hubs-features.md#partitions).</span><span class="sxs-lookup"><span data-stu-id="9bf95-112">The key to scaling in Event Hubs is the [partitioned consumer model](event-hubs-features.md#partitions).</span></span> <span data-ttu-id="9bf95-113">Každý oddíl je segment nezávislé dat a obsazením nezávisle.</span><span class="sxs-lookup"><span data-stu-id="9bf95-113">Each partition is an independent segment of data and is consumed independently.</span></span> <span data-ttu-id="9bf95-114">V čase tato data ages vypnutý, založené na dobu uchování konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="9bf95-114">Over time this data ages off, based on the configurable retention period.</span></span> <span data-ttu-id="9bf95-115">V důsledku toho se dané události rozbočovače nikdy získá "příliš úplná."</span><span class="sxs-lookup"><span data-stu-id="9bf95-115">As a result, a given event hub never gets "too full."</span></span>

<span data-ttu-id="9bf95-116">Zaznamenat centra událostí můžete určit vlastní účtu Azure Blob storage a kontejner nebo účtu Azure Data Lake Store, které se používají k ukládání zaznamenaná data.</span><span class="sxs-lookup"><span data-stu-id="9bf95-116">Event Hubs Capture enables you to specify your own Azure Blob storage account and container, or Azure Data Lake Store account, which are used to store the captured data.</span></span> <span data-ttu-id="9bf95-117">Tyto účty může být ve stejné oblasti jako vaše Centrum událostí nebo v jiné oblasti, přidání do flexibilitu funkci zachycení centra událostí.</span><span class="sxs-lookup"><span data-stu-id="9bf95-117">These accounts can be in the same region as your event hub or in another region, adding to the flexibility of the Event Hubs Capture feature.</span></span>

<span data-ttu-id="9bf95-118">Zaznamenaná data je napsána v [Apache Avro] [ Apache Avro] formátu: compact, rychlá a binární formát, který poskytuje bohaté datové struktury vloženého schématu.</span><span class="sxs-lookup"><span data-stu-id="9bf95-118">Captured data is written in [Apache Avro][Apache Avro] format: a compact, fast, binary format that provides rich data structures with inline schema.</span></span> <span data-ttu-id="9bf95-119">Tento formát se často používá v ekosystému Hadoop, Stream Analytics a Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="9bf95-119">This format is widely used in the Hadoop ecosystem, Stream Analytics, and Azure Data Factory.</span></span> <span data-ttu-id="9bf95-120">Další informace o práci s Avro najdete dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="9bf95-120">More information about working with Avro is available later in this article.</span></span>

### <a name="capture-windowing"></a><span data-ttu-id="9bf95-121">Zaznamenat okna</span><span class="sxs-lookup"><span data-stu-id="9bf95-121">Capture windowing</span></span>

<span data-ttu-id="9bf95-122">Zaznamenat centra událostí můžete nastavit okno k řízení zaznamenávání.</span><span class="sxs-lookup"><span data-stu-id="9bf95-122">Event Hubs Capture enables you to set up a window to control capturing.</span></span> <span data-ttu-id="9bf95-123">Toto okno je minimální velikost a konfigurace času zásadám"první wins," znamená, že první aktivační události došlo způsobí, že operace zachycení.</span><span class="sxs-lookup"><span data-stu-id="9bf95-123">This window is a minimum size and time configuration with a "first wins policy," meaning that the first trigger encountered causes a capture operation.</span></span> <span data-ttu-id="9bf95-124">Pokud máte 15 minut, 100 MB okna Sběr a odeslat 1 MB za sekundu, aktivačních událostí velikost okna před časový interval.</span><span class="sxs-lookup"><span data-stu-id="9bf95-124">If you have a fifteen-minute, 100 MB capture window and send 1 MB per second, the size window triggers before the time window.</span></span> <span data-ttu-id="9bf95-125">Každý oddíl zaznamená nezávisle a zapíše objekt blob bloku dokončené v době zachycení, s názvem dobu, kdy došlo k zachycení intervalu.</span><span class="sxs-lookup"><span data-stu-id="9bf95-125">Each partition captures independently and writes a completed block blob at the time of capture, named for the time at which the capture interval was encountered.</span></span> <span data-ttu-id="9bf95-126">Zásady vytváření názvů úložiště je následující:</span><span class="sxs-lookup"><span data-stu-id="9bf95-126">The storage naming convention is as follows:</span></span>

```
[namespace]/[event hub]/[partition]/[YYYY]/[MM]/[DD]/[HH]/[mm]/[ss]
```

### <a name="scaling-to-throughput-units"></a><span data-ttu-id="9bf95-127">Škálování jednotky propustnosti</span><span class="sxs-lookup"><span data-stu-id="9bf95-127">Scaling to throughput units</span></span>

<span data-ttu-id="9bf95-128">Provoz centra událostí řídí [jednotky propustnosti](event-hubs-features.md#capacity).</span><span class="sxs-lookup"><span data-stu-id="9bf95-128">Event Hubs traffic is controlled by [throughput units](event-hubs-features.md#capacity).</span></span> <span data-ttu-id="9bf95-129">Jedna jednotka propustnosti umožňuje 1 MB za sekundu nebo 1000 událostí za sekundu příjem příchozích dat a dvakrát toto množství odchozí.</span><span class="sxs-lookup"><span data-stu-id="9bf95-129">A single throughput unit allows 1 MB per second or 1000 events per second of ingress and twice that amount of egress.</span></span> <span data-ttu-id="9bf95-130">Standardní služby Event Hubs se dá nakonfigurovat s 1-20 jednotky propustnosti a další můžete zakoupit kvótu zvýšit [žádost o podporu][support request].</span><span class="sxs-lookup"><span data-stu-id="9bf95-130">Standard Event Hubs can be configured with 1-20 throughput units, and you can purchase more with a quota increase [support request][support request].</span></span> <span data-ttu-id="9bf95-131">Použití mimo vaší zakoupené jednotky propustnosti je omezen.</span><span class="sxs-lookup"><span data-stu-id="9bf95-131">Usage beyond your purchased throughput units is throttled.</span></span> <span data-ttu-id="9bf95-132">Zaznamenat centra událostí zkopíruje data přímo z interní úložiště služby Event Hubs, obcházení kvóty odchozí jednotky propustnosti a uložit vaše odchozí pro další zpracování čtečky, jako je například Stream Analytics nebo Spark.</span><span class="sxs-lookup"><span data-stu-id="9bf95-132">Event Hubs Capture copies data directly from the internal Event Hubs storage, bypassing throughput unit egress quotas and saving your egress for other processing readers, such as Stream Analytics or Spark.</span></span>

<span data-ttu-id="9bf95-133">Po nakonfigurování zaznamenat centra událostí spustí automaticky při odeslání první událost a běžet dál.</span><span class="sxs-lookup"><span data-stu-id="9bf95-133">Once configured, Event Hubs Capture runs automatically when you send your first event, and continues running.</span></span> <span data-ttu-id="9bf95-134">Aby bylo snazší pro příjem dat zpracování vědět, že proces funguje, zapíše Event Hubs prázdné soubory, pokud nejsou žádná data.</span><span class="sxs-lookup"><span data-stu-id="9bf95-134">To make it easier for your downstream processing to know that the process is working, Event Hubs writes empty files when there is no data.</span></span> <span data-ttu-id="9bf95-135">Tento proces zajišťuje předvídatelný cadence a značky, který může zadat vaše batch procesory.</span><span class="sxs-lookup"><span data-stu-id="9bf95-135">This process provides a predictable cadence and marker that can feed your batch processors.</span></span>

## <a name="setting-up-event-hubs-capture"></a><span data-ttu-id="9bf95-136">Nastavení zachycení centra událostí</span><span class="sxs-lookup"><span data-stu-id="9bf95-136">Setting up Event Hubs Capture</span></span>

<span data-ttu-id="9bf95-137">Zachycení můžete nakonfigurovat pomocí události rozbočovače pro čas vytvoření [portál Azure](https://portal.azure.com), nebo pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9bf95-137">You can configure Capture at the event hub creation time using the [Azure portal](https://portal.azure.com), or using Azure Resource Manager templates.</span></span> <span data-ttu-id="9bf95-138">Další informace najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="9bf95-138">For more information, see the following articles:</span></span>

- [<span data-ttu-id="9bf95-139">Povolit pomocí portálu Azure Capture centra událostí</span><span class="sxs-lookup"><span data-stu-id="9bf95-139">Enable Event Hubs Capture using the Azure portal</span></span>](event-hubs-capture-enable-through-portal.md)
- [<span data-ttu-id="9bf95-140">Vytvoření oboru názvů Event Hubs s centra událostí a povolte zachycení pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9bf95-140">Create an Event Hubs namespace with an event hub and enable Capture using an Azure Resource Manager template</span></span>](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)

## <a name="exploring-the-captured-files-and-working-with-avro"></a><span data-ttu-id="9bf95-141">Zkoumání zaznamenané soubory a práci s Avro</span><span class="sxs-lookup"><span data-stu-id="9bf95-141">Exploring the captured files and working with Avro</span></span>

<span data-ttu-id="9bf95-142">Zaznamenat centra událostí vytvoří soubory ve formátu Avro, jako je zadaný na nakonfigurované časové okno.</span><span class="sxs-lookup"><span data-stu-id="9bf95-142">Event Hubs Capture creates files in Avro format, as specified on the configured time window.</span></span> <span data-ttu-id="9bf95-143">Tyto soubory můžete zobrazit v libovolného nástroje, jako [Azure Storage Explorer][Azure Storage Explorer].</span><span class="sxs-lookup"><span data-stu-id="9bf95-143">You can view these files in any tool such as [Azure Storage Explorer][Azure Storage Explorer].</span></span> <span data-ttu-id="9bf95-144">Můžete stáhnout soubory místně a pracovat s nimi.</span><span class="sxs-lookup"><span data-stu-id="9bf95-144">You can download the files locally to work on them.</span></span>

<span data-ttu-id="9bf95-145">Soubory vytvořené pomocí Capture centra událostí mají následující schématu Avro:</span><span class="sxs-lookup"><span data-stu-id="9bf95-145">The files produced by Event Hubs Capture have the following Avro schema:</span></span>

![][3]

<span data-ttu-id="9bf95-146">Je snadný způsob, jak prozkoumat Avro soubory pomocí [nástroje Avro] [ Avro Tools] jar z Apache.</span><span class="sxs-lookup"><span data-stu-id="9bf95-146">An easy way to explore Avro files is by using the [Avro Tools][Avro Tools] jar from Apache.</span></span> <span data-ttu-id="9bf95-147">Po stažení této jar, uvidíte schéma konkrétní soubor Avro tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9bf95-147">After downloading this jar, you can see the schema of a specific Avro file by running the following command:</span></span>

```
java -jar avro-tools-1.8.2.jar getschema <name of capture file>
```

<span data-ttu-id="9bf95-148">Tento příkaz vrátí</span><span class="sxs-lookup"><span data-stu-id="9bf95-148">This command returns</span></span>

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

<span data-ttu-id="9bf95-149">Můžete taky nástroje Avro převeďte soubor do formátu JSON a provádět další zpracování.</span><span class="sxs-lookup"><span data-stu-id="9bf95-149">You can also use Avro Tools to convert the file to JSON format and perform other processing.</span></span>

<span data-ttu-id="9bf95-150">Pokud chcete provést pokročilejší zpracování, stáhněte a nainstalujte Avro pro vaši volbu platformy.</span><span class="sxs-lookup"><span data-stu-id="9bf95-150">To perform more advanced processing, download and install Avro for your choice of platform.</span></span> <span data-ttu-id="9bf95-151">V době psaní tohoto textu, nejsou k dispozici pro C, C++, C implementace\#, Java, NodeJS, Perl, PHP, Python nebo Ruby.</span><span class="sxs-lookup"><span data-stu-id="9bf95-151">At the time of this writing, there are implementations available for C, C++, C\#, Java, NodeJS, Perl, PHP, Python, and Ruby.</span></span>

<span data-ttu-id="9bf95-152">Dokončení Průvodce Začínáme pro má Apache Avro [Java] [ Java] a [Python][Python].</span><span class="sxs-lookup"><span data-stu-id="9bf95-152">Apache Avro has complete Getting Started guides for [Java][Java] and [Python][Python].</span></span> <span data-ttu-id="9bf95-153">Také můžete přečíst [Začínáme se službou Event Hubs zaznamenat](event-hubs-capture-python.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9bf95-153">You can also read the [Getting started with Event Hubs Capture](event-hubs-capture-python.md) article.</span></span>

## <a name="how-event-hubs-capture-is-charged"></a><span data-ttu-id="9bf95-154">Jak je účtován zaznamenat centra událostí</span><span class="sxs-lookup"><span data-stu-id="9bf95-154">How Event Hubs Capture is charged</span></span>

<span data-ttu-id="9bf95-155">Zaznamenat centra událostí je podobně měřeného na jednotky propustnosti: jako poplatek po hodinách.</span><span class="sxs-lookup"><span data-stu-id="9bf95-155">Event Hubs Capture is metered similarly to throughput units: as an hourly charge.</span></span> <span data-ttu-id="9bf95-156">Zřizování je přímo úměrná počtu jednotek propustnosti zakoupili pro obor názvů.</span><span class="sxs-lookup"><span data-stu-id="9bf95-156">The charge is directly proportional to the number of throughput units purchased for the namespace.</span></span> <span data-ttu-id="9bf95-157">Jednotky propustnosti jsou vyšší a zmenšit, zachycení událostí centra měřidla zvýšit nebo snížit zajistit odpovídající výkon.</span><span class="sxs-lookup"><span data-stu-id="9bf95-157">As throughput units are increased and decreased, Event Hubs Capture meters increase and decrease to provide matching performance.</span></span> <span data-ttu-id="9bf95-158">Měřidla nastat současně.</span><span class="sxs-lookup"><span data-stu-id="9bf95-158">The meters occur in tandem.</span></span> <span data-ttu-id="9bf95-159">Podrobnosti o cenách najdete v části [cenách služby Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="9bf95-159">For pricing details, see [Event Hubs pricing](https://azure.microsoft.com/pricing/details/event-hubs/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="9bf95-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9bf95-160">Next steps</span></span>

<span data-ttu-id="9bf95-161">Zaznamenat centra událostí je nejjednodušší způsob, jak načíst data do Azure.</span><span class="sxs-lookup"><span data-stu-id="9bf95-161">Event Hubs Capture is the easiest way to get data into Azure.</span></span> <span data-ttu-id="9bf95-162">Pomocí Azure Data Lake, Azure Data Factory a Azure HDInsight, můžete provést dávkové zpracování a dalších analytics pomocí známých nástrojů a platformy dle vlastního výběru, v jakémkoli měřítku potřebujete.</span><span class="sxs-lookup"><span data-stu-id="9bf95-162">Using Azure Data Lake, Azure Data Factory, and Azure HDInsight, you can perform batch processing and other analytics using familiar tools and platforms of your choosing, at any scale you need.</span></span>

<span data-ttu-id="9bf95-163">Další informace o službě Event Hubs najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="9bf95-163">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="9bf95-164">Začínáme s odesílání a příjem událostí</span><span class="sxs-lookup"><span data-stu-id="9bf95-164">Get started sending and receiving events</span></span>](event-hubs-dotnet-framework-getstarted-send.md)
* <span data-ttu-id="9bf95-165">Úplná [ukázková aplikace, která používá službu Event Hubs][sample application that uses Event Hubs]</span><span class="sxs-lookup"><span data-stu-id="9bf95-165">A complete [sample application that uses Event Hubs][sample application that uses Event Hubs]</span></span>
* <span data-ttu-id="9bf95-166">[Přehled služby Event Hubs][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="9bf95-166">[Event Hubs overview][Event Hubs overview]</span></span>

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
