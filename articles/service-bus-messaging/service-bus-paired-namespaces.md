---
title: "Obory názvů spárovat Azure Service Bus | Microsoft Docs"
description: "Podrobnosti implementace spárované obor názvů a náklady"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 2440c8d3-ed2e-47e0-93cf-ab7fbb855d2e
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: sethm
ms.openlocfilehash: a200ea7937b9f5296c743928a9408897adfba428
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="paired-namespace-implementation-details-and-cost-implications"></a><span data-ttu-id="24606-103">Spárovat podrobnosti implementace obor názvů a náklady dopad</span><span class="sxs-lookup"><span data-stu-id="24606-103">Paired namespace implementation details and cost implications</span></span>
<span data-ttu-id="24606-104">[PairNamespaceAsync] [ PairNamespaceAsync] metoda, použití [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] instance, provádí viditelné úlohy vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="24606-104">The [PairNamespaceAsync][PairNamespaceAsync] method, using a [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions] instance, performs visible tasks on your behalf.</span></span> <span data-ttu-id="24606-105">Vzhledem k tomu, že jsou náklady aspekty při použití funkce, je vhodné pochopit tyto úlohy tak, aby očekávané chování při Odehrává se.</span><span class="sxs-lookup"><span data-stu-id="24606-105">Because there are cost considerations when using the feature, it is useful to understand those tasks so that you expect the behavior when it happens.</span></span> <span data-ttu-id="24606-106">Rozhraní API zapojí následující automatické chování vaším jménem:</span><span class="sxs-lookup"><span data-stu-id="24606-106">The API engages the following automatic behavior on your behalf:</span></span>

* <span data-ttu-id="24606-107">Vytvoření nevyřízených položek fronty.</span><span class="sxs-lookup"><span data-stu-id="24606-107">Creation of backlog queues.</span></span>
* <span data-ttu-id="24606-108">Vytvoření [MessageSender] [ MessageSender] objekt, který komunikuje se fronty nebo témata.</span><span class="sxs-lookup"><span data-stu-id="24606-108">Creation of a [MessageSender][MessageSender] object that talks to queues or topics.</span></span>
* <span data-ttu-id="24606-109">Při zasílání zpráv entity stane nedostupným, odešle příkaz ping zprávy do entity ve snaze rozpoznat, kdy se dané entity opět k dispozici.</span><span class="sxs-lookup"><span data-stu-id="24606-109">When a messaging entity becomes unavailable, sends ping messages to the entity in an attempt to detect when that entity becomes available again.</span></span>
* <span data-ttu-id="24606-110">Volitelně vytváří sadu "čerpadel zpráva", přesunovat zprávy z fronty nevyřízených položek do primární front.</span><span class="sxs-lookup"><span data-stu-id="24606-110">Optionally creates of a set of “message pumps” that move messages from the backlog queues to the primary queues.</span></span>
* <span data-ttu-id="24606-111">Koordinuje ukončovací nebo chybující primárních a sekundárních [MessagingFactory] [ MessagingFactory] instance.</span><span class="sxs-lookup"><span data-stu-id="24606-111">Coordinates closing/faulting of the primary and secondary [MessagingFactory][MessagingFactory] instances.</span></span>

<span data-ttu-id="24606-112">Na vysoké úrovni, tato funkce funguje takto: když primární entita je v pořádku, dojít k žádným změnám chování.</span><span class="sxs-lookup"><span data-stu-id="24606-112">At a high level, the feature works as follows: when the primary entity is healthy, no behavior changes occur.</span></span> <span data-ttu-id="24606-113">Když [FailoverInterval] [ FailoverInterval] po uplynutí předem doba trvání a ne úspěšné primární entity vidí pošle za jiný přechodná [MessagingException] [ MessagingException] nebo [TimeoutException][TimeoutException], očekávejte toto chování:</span><span class="sxs-lookup"><span data-stu-id="24606-113">When the [FailoverInterval][FailoverInterval] duration elapses, and the primary entity sees no successful sends after a non-transient [MessagingException][MessagingException] or a [TimeoutException][TimeoutException], the following behavior occurs:</span></span>

1. <span data-ttu-id="24606-114">Odešlete na primární entity operací jsou zakázány a systém pomocí příkazu ping primární entity dokud příkazy ping mohou být zajišťovány úspěšně.</span><span class="sxs-lookup"><span data-stu-id="24606-114">Send operations to the primary entity are disabled and the system pings the primary entity until pings can be successfully delivered.</span></span>
2. <span data-ttu-id="24606-115">Je vybrán náhodných nevyřízené položky fronty.</span><span class="sxs-lookup"><span data-stu-id="24606-115">A random backlog queue is selected.</span></span>
3. <span data-ttu-id="24606-116">[BrokeredMessage] [ BrokeredMessage] objekty jsou směrovány do fronty zvolené nevyřízených položek.</span><span class="sxs-lookup"><span data-stu-id="24606-116">[BrokeredMessage][BrokeredMessage] objects are routed to the chosen backlog queue.</span></span>
4. <span data-ttu-id="24606-117">Pokud se nezdaří operaci odeslání do zvolené nevyřízené položky fronty, této fronty pocházejí z otáčení a je vybraná novou frontu.</span><span class="sxs-lookup"><span data-stu-id="24606-117">If a send operation to the chosen backlog queue fails, that queue is pulled from the rotation and a new queue is selected.</span></span> <span data-ttu-id="24606-118">Na všech uživatelů [MessagingFactory] [ MessagingFactory] instance Další informace o selhání.</span><span class="sxs-lookup"><span data-stu-id="24606-118">All senders on the [MessagingFactory][MessagingFactory] instance learn of the failure.</span></span>

<span data-ttu-id="24606-119">Následující obrázky zobrazit v ní sekvenci.</span><span class="sxs-lookup"><span data-stu-id="24606-119">The following figures depict the sequence.</span></span> <span data-ttu-id="24606-120">Nejprve odesílatel odešle zprávy.</span><span class="sxs-lookup"><span data-stu-id="24606-120">First, the sender sends messages.</span></span>

![Spárované obory názvů][0]

<span data-ttu-id="24606-122">Při selhání k odeslání do primární fronty začne odesílatel odesílání zpráv do fronty náhodně vybrané nevyřízených položek.</span><span class="sxs-lookup"><span data-stu-id="24606-122">Upon failure to send to the primary queue, the sender begins sending messages to a randomly chosen backlog queue.</span></span> <span data-ttu-id="24606-123">Současně spustí úlohu příkazu ping.</span><span class="sxs-lookup"><span data-stu-id="24606-123">Simultaneously, it starts a ping task.</span></span>

![Spárované obory názvů][1]

<span data-ttu-id="24606-125">V tomto okamžiku zprávy jsou stále v sekundární fronty a nebyly dodány do primární fronty.</span><span class="sxs-lookup"><span data-stu-id="24606-125">At this point the messages are still in the secondary queue and have not been delivered to the primary queue.</span></span> <span data-ttu-id="24606-126">Jakmile primární fronta je v pořádku znovu, by měl běžet Trativod alespoň jeden proces.</span><span class="sxs-lookup"><span data-stu-id="24606-126">Once the primary queue is healthy again, at least one process should be running the syphon.</span></span> <span data-ttu-id="24606-127">Trativod přináší zprávy ze všech různých front nevyřízených položek správné cílové entity (fronty a témata).</span><span class="sxs-lookup"><span data-stu-id="24606-127">The syphon delivers the messages from all the various backlog queues to the proper destination entities (queues and topics).</span></span>

![Spárované obory názvů][2]

<span data-ttu-id="24606-129">Zbývající část tohoto tématu popisuje konkrétní podrobnosti o fungování tyto údaje.</span><span class="sxs-lookup"><span data-stu-id="24606-129">The remainder of this topic discusses the specific details of how these pieces work.</span></span>

## <a name="creation-of-backlog-queues"></a><span data-ttu-id="24606-130">Vytvoření nevyřízených položek fronty</span><span class="sxs-lookup"><span data-stu-id="24606-130">Creation of backlog queues</span></span>
<span data-ttu-id="24606-131">[SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] byl předán objekt [PairNamespaceAsync] [ PairNamespaceAsync] metoda označuje počet nevyřízených položek fronty, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="24606-131">The [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions] object passed to the [PairNamespaceAsync][PairNamespaceAsync] method indicates the number of backlog queues you want to use.</span></span> <span data-ttu-id="24606-132">Každou frontu nevyřízených položek je pak vytvořen s následujícími vlastnostmi explicitně nastavit (všechny ostatní hodnoty jsou nastaveny na [QueueDescription] [ QueueDescription] výchozí nastavení):</span><span class="sxs-lookup"><span data-stu-id="24606-132">Each backlog queue is then created with the following properties explicitly set (all other values are set to the [QueueDescription][QueueDescription] defaults):</span></span>

| <span data-ttu-id="24606-133">Cesta</span><span class="sxs-lookup"><span data-stu-id="24606-133">Path</span></span> | <span data-ttu-id="24606-134">[primární obor názvů] / x-servicebus přenos / [index] kde [index] je hodnota [0, BacklogQueueCount)</span><span class="sxs-lookup"><span data-stu-id="24606-134">[primary namespace]/x-servicebus-transfer/[index] where [index] is a value in [0, BacklogQueueCount)</span></span> |
| --- | --- |
| <span data-ttu-id="24606-135">MaxSizeInMegabytes</span><span class="sxs-lookup"><span data-stu-id="24606-135">MaxSizeInMegabytes</span></span> |<span data-ttu-id="24606-136">5120</span><span class="sxs-lookup"><span data-stu-id="24606-136">5120</span></span> |
| <span data-ttu-id="24606-137">maxDeliveryCount</span><span class="sxs-lookup"><span data-stu-id="24606-137">MaxDeliveryCount</span></span> |<span data-ttu-id="24606-138">int. MaxValue</span><span class="sxs-lookup"><span data-stu-id="24606-138">int.MaxValue</span></span> |
| <span data-ttu-id="24606-139">DefaultMessageTimeToLive</span><span class="sxs-lookup"><span data-stu-id="24606-139">DefaultMessageTimeToLive</span></span> |<span data-ttu-id="24606-140">TimeSpan.MaxValue, což</span><span class="sxs-lookup"><span data-stu-id="24606-140">TimeSpan.MaxValue</span></span> |
| <span data-ttu-id="24606-141">AutoDeleteOnIdle</span><span class="sxs-lookup"><span data-stu-id="24606-141">AutoDeleteOnIdle</span></span> |<span data-ttu-id="24606-142">TimeSpan.MaxValue, což</span><span class="sxs-lookup"><span data-stu-id="24606-142">TimeSpan.MaxValue</span></span> |
| <span data-ttu-id="24606-143">Trvání uzamčení</span><span class="sxs-lookup"><span data-stu-id="24606-143">LockDuration</span></span> |<span data-ttu-id="24606-144">1 minuta</span><span class="sxs-lookup"><span data-stu-id="24606-144">1 minute</span></span> |
| <span data-ttu-id="24606-145">EnableDeadLetteringOnMessageExpiration</span><span class="sxs-lookup"><span data-stu-id="24606-145">EnableDeadLetteringOnMessageExpiration</span></span> |<span data-ttu-id="24606-146">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="24606-146">true</span></span> |
| <span data-ttu-id="24606-147">EnableBatchedOperations</span><span class="sxs-lookup"><span data-stu-id="24606-147">EnableBatchedOperations</span></span> |<span data-ttu-id="24606-148">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="24606-148">true</span></span> |

<span data-ttu-id="24606-149">Například vytvořit první nevyřízených položek fronty pro obor názvů **contoso** jmenuje `contoso/x-servicebus-transfer/0`.</span><span class="sxs-lookup"><span data-stu-id="24606-149">For example, the first backlog queue created for namespace **contoso** is named `contoso/x-servicebus-transfer/0`.</span></span>

<span data-ttu-id="24606-150">Při vytváření front, kód nejprve zkontroluje, zda existuje takový fronty.</span><span class="sxs-lookup"><span data-stu-id="24606-150">When creating the queues, the code first checks to see if such a queue exists.</span></span> <span data-ttu-id="24606-151">Pokud fronta neexistuje, je vytvářena fronta.</span><span class="sxs-lookup"><span data-stu-id="24606-151">If the queue does not exist, then the queue is created.</span></span> <span data-ttu-id="24606-152">Kód nemá vyčištění "výběr" nevyřízené položky fronty.</span><span class="sxs-lookup"><span data-stu-id="24606-152">The code does not clean up "extra" backlog queues.</span></span> <span data-ttu-id="24606-153">Konkrétně Pokud aplikace s primární obor názvů **contoso** požadavky pět nevyřízené položky fronty, ale nevyřízené položky fronty s cestou `contoso/x-servicebus-transfer/7` existuje, této navíc nevyřízené položky fronty stále existuje, ale nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="24606-153">Specifically, if the application with the primary namespace **contoso** requests five backlog queues but a backlog queue with the path `contoso/x-servicebus-transfer/7` exists, that extra backlog queue is still present but is not used.</span></span> <span data-ttu-id="24606-154">Systém umožňuje explicitně navíc nevyřízené položky fronty existovat, které nebyla použita.</span><span class="sxs-lookup"><span data-stu-id="24606-154">The system explicitly allows extra backlog queues to exist that would not be used.</span></span> <span data-ttu-id="24606-155">Jako vlastník obor názvů je zodpovědná za vyčištění žádné nepoužité nebo nežádoucí nevyřízené položky fronty.</span><span class="sxs-lookup"><span data-stu-id="24606-155">As the namespace owner, you are responsible for cleaning up any unused/unwanted backlog queues.</span></span> <span data-ttu-id="24606-156">Z důvodu pro toto rozhodnutí je, že Service Bus nemůže vědět, jaké účely existovat pro všechny fronty, které jsou v oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="24606-156">The reason for this decision is that Service Bus cannot know what purposes exist for all the queues in your namespace.</span></span> <span data-ttu-id="24606-157">Kromě toho pokud existuje s daným názvem fronty, ale nesplňuje předpokládané [QueueDescription][QueueDescription], pak jsou vaše důvodů vlastní pro změnu výchozího chování.</span><span class="sxs-lookup"><span data-stu-id="24606-157">Furthermore, if a queue exists with the given name but does not meet the assumed [QueueDescription][QueueDescription], then your reasons are your own for changing the default behavior.</span></span> <span data-ttu-id="24606-158">Žádné záruky probíhají změny nevyřízené položky fronty podle vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="24606-158">No guarantees are made for modifications to the backlog queues by your code.</span></span> <span data-ttu-id="24606-159">Ujistěte se, že jste důkladně otestovat změny.</span><span class="sxs-lookup"><span data-stu-id="24606-159">Make sure to test your changes thoroughly.</span></span>

## <a name="custom-messagesender"></a><span data-ttu-id="24606-160">Vlastní MessageSender</span><span class="sxs-lookup"><span data-stu-id="24606-160">Custom MessageSender</span></span>
<span data-ttu-id="24606-161">Při odesílání, všechny zprávy projít interní [MessageSender] [ MessageSender] objekt, který se chovat normálně když všechno funguje a přesměruje na nevyřízené položky fronty při věcí "break."</span><span class="sxs-lookup"><span data-stu-id="24606-161">When sending, all messages go through an internal [MessageSender][MessageSender] object that behaves normally when everything works, and redirects to the backlog queues when things "break."</span></span> <span data-ttu-id="24606-162">Po přijetí selhání bez přechodná, spustí časovač.</span><span class="sxs-lookup"><span data-stu-id="24606-162">Upon receiving a non-transient failure, a timer starts.</span></span> <span data-ttu-id="24606-163">Po [časový interval] [ TimeSpan] období skládající se z [FailoverInterval] [ FailoverInterval] provozuje během které se odesílají žádné úspěšné zprávy, hodnota vlastnosti převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="24606-163">After a [TimeSpan][TimeSpan] period consisting of the [FailoverInterval][FailoverInterval] property value during which no successful messages are sent, the failover is engaged.</span></span> <span data-ttu-id="24606-164">V tomto okamžiku následujících akcí dojít u každé entity:</span><span class="sxs-lookup"><span data-stu-id="24606-164">At this point, the following things happen for each entity:</span></span>

* <span data-ttu-id="24606-165">Spustí úlohu ping každých [PingPrimaryInterval] [ PingPrimaryInterval] ke kontrole, pokud tato entita je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="24606-165">A ping task executes every [PingPrimaryInterval][PingPrimaryInterval] to check if the entity is available.</span></span> <span data-ttu-id="24606-166">Jakmile tato úloha úspěšná, všechny kód klienta, který používá entity okamžitě spustí, odesílání nových zpráv do primární oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="24606-166">Once this task succeeds, all client code that uses the entity immediately starts sending new messages to the primary namespace.</span></span>
* <span data-ttu-id="24606-167">Bude mít za následek budoucí požadavky na odeslání této stejné entity z jiní odesílatelé [BrokeredMessage] [ BrokeredMessage] odesílána upravit tak, aby nacházejí ve frontě nevyřízených položek.</span><span class="sxs-lookup"><span data-stu-id="24606-167">Future requests to send to that same entity from any other senders will result in the [BrokeredMessage][BrokeredMessage] being sent to be modified to sit in the backlog queue.</span></span> <span data-ttu-id="24606-168">Úpravy odebere některé vlastnosti z [BrokeredMessage] [ BrokeredMessage] objektu a ukládá je jinde.</span><span class="sxs-lookup"><span data-stu-id="24606-168">The modification removes some properties from the [BrokeredMessage][BrokeredMessage] object and stores them elsewhere.</span></span> <span data-ttu-id="24606-169">Následující vlastnosti jsou vymazána a přidají pod nový alias, povolení služby Service Bus a sady SDK ke zpracování zpráv jednotně:</span><span class="sxs-lookup"><span data-stu-id="24606-169">The following properties are cleared and added under a new alias, allowing Service Bus and the SDK to process messages uniformly:</span></span>

| <span data-ttu-id="24606-170">Původní název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="24606-170">Old Property Name</span></span> | <span data-ttu-id="24606-171">Nový název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="24606-171">New Property Name</span></span> |
| --- | --- |
| <span data-ttu-id="24606-172">ID relace</span><span class="sxs-lookup"><span data-stu-id="24606-172">SessionId</span></span> |<span data-ttu-id="24606-173">x-ms-ID relace</span><span class="sxs-lookup"><span data-stu-id="24606-173">x-ms-sessionid</span></span> |
| <span data-ttu-id="24606-174">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="24606-174">TimeToLive</span></span> |<span data-ttu-id="24606-175">x-ms-timetolive</span><span class="sxs-lookup"><span data-stu-id="24606-175">x-ms-timetolive</span></span> |
| <span data-ttu-id="24606-176">ScheduledEnqueueTimeUtc</span><span class="sxs-lookup"><span data-stu-id="24606-176">ScheduledEnqueueTimeUtc</span></span> |<span data-ttu-id="24606-177">x-ms-path</span><span class="sxs-lookup"><span data-stu-id="24606-177">x-ms-path</span></span> |

<span data-ttu-id="24606-178">Původní cílovou cestu jsou také uložená v zprávu jako vlastnost s názvem x-ms-path.</span><span class="sxs-lookup"><span data-stu-id="24606-178">The original destination path is also stored within the message as a property named x-ms-path.</span></span> <span data-ttu-id="24606-179">Tento návrh umožňuje zprávy pro mnoho entity k společně existovat v jednom nevyřízené položky fronty.</span><span class="sxs-lookup"><span data-stu-id="24606-179">This design allows messages for many entities to coexist in a single backlog queue.</span></span> <span data-ttu-id="24606-180">Vlastnosti jsou přeložen zpět Trativod.</span><span class="sxs-lookup"><span data-stu-id="24606-180">The properties are translated back by the syphon.</span></span>

<span data-ttu-id="24606-181">Vlastní [MessageSender] [ MessageSender] objekt může dojít k potížím při zprávy přístupu limit 256 KB a využívána převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="24606-181">The custom [MessageSender][MessageSender] object can encounter issues when messages approach the 256-KB limit and failover is engaged.</span></span> <span data-ttu-id="24606-182">Vlastní [MessageSender] [ MessageSender] objekt ukládá zprávy pro všechny fronty a témata společně v nevyřízené položky fronty.</span><span class="sxs-lookup"><span data-stu-id="24606-182">The custom [MessageSender][MessageSender] object stores messages for all queues and topics together in the backlog queues.</span></span> <span data-ttu-id="24606-183">Tento objekt zkombinuje zprávy z mnoha základní barvy společně v rámci nevyřízené položky fronty.</span><span class="sxs-lookup"><span data-stu-id="24606-183">This object mixes messages from many primaries together within the backlog queues.</span></span> <span data-ttu-id="24606-184">Zpracovat Vyrovnávání zatížení mezi mnoho klientů, které mezi sebou neznáte, sadu SDK náhodně vybere jeden nevyřízených položek fronty pro každou [QueueClient] [ QueueClient] nebo [TopicClient] [ TopicClient] vytvoříte v kódu.</span><span class="sxs-lookup"><span data-stu-id="24606-184">To handle load balancing among many clients that do not know each other, the SDK randomly picks one backlog queue for each [QueueClient][QueueClient] or [TopicClient][TopicClient] you create in code.</span></span>

## <a name="pings"></a><span data-ttu-id="24606-185">Příkazy ping</span><span class="sxs-lookup"><span data-stu-id="24606-185">Pings</span></span>
<span data-ttu-id="24606-186">Zprávu ping je prázdná [BrokeredMessage] [ BrokeredMessage] s jeho [ContentType] [ ContentType] vlastnost nastavena na hodnotu aplikace/vnd.ms-servicebus-ping a [TimeToLive] [ TimeToLive] hodnota 1 sekunda.</span><span class="sxs-lookup"><span data-stu-id="24606-186">A ping message is an empty [BrokeredMessage][BrokeredMessage] with its [ContentType][ContentType] property set to application/vnd.ms-servicebus-ping and a [TimeToLive][TimeToLive] value of 1 second.</span></span> <span data-ttu-id="24606-187">Tento příkaz ping má jeden speciální vlastnosti v Service Bus: serveru nikdy přináší příkazu ping, když si vyžádá všechny volající [BrokeredMessage][BrokeredMessage].</span><span class="sxs-lookup"><span data-stu-id="24606-187">This ping has one special characteristic in Service Bus: the server never delivers a ping when any caller requests a [BrokeredMessage][BrokeredMessage].</span></span> <span data-ttu-id="24606-188">Proto není nutné zjistěte, jak přijmout a tyto zprávy ignorovat.</span><span class="sxs-lookup"><span data-stu-id="24606-188">Thus, you never have to learn how to receive and ignore these messages.</span></span> <span data-ttu-id="24606-189">Každá entita (jedinečný fronta nebo téma) za [MessagingFactory] [ MessagingFactory] instance pro každý klient bude příkazu ping zkontrolujte dosažitelnost při jsou považovány za nedostupný.</span><span class="sxs-lookup"><span data-stu-id="24606-189">Each entity (unique queue or topic) per [MessagingFactory][MessagingFactory] instance per client will be pinged when they are considered to be unavailable.</span></span> <span data-ttu-id="24606-190">Ve výchozím nastavení k tomu dojde jednou za minutu.</span><span class="sxs-lookup"><span data-stu-id="24606-190">By default, this happens once per minute.</span></span> <span data-ttu-id="24606-191">Zprávy ping se považují za regulární zpráv Service Bus a může mít za následek poplatky za zprávy a šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="24606-191">Ping messages are considered to be regular Service Bus messages, and can result in charges for bandwidth and messages.</span></span> <span data-ttu-id="24606-192">Jakmile klienti zjišťovat, zda je k dispozici v systému, zastavte zprávy.</span><span class="sxs-lookup"><span data-stu-id="24606-192">As soon as the clients detect that the system is available, the messages stop.</span></span>

## <a name="the-syphon"></a><span data-ttu-id="24606-193">Trativod</span><span class="sxs-lookup"><span data-stu-id="24606-193">The syphon</span></span>
<span data-ttu-id="24606-194">Alespoň jeden spustitelný program v aplikaci by měl být aktivně běžícími Trativod.</span><span class="sxs-lookup"><span data-stu-id="24606-194">At least one executable program in the application should be actively running the syphon.</span></span> <span data-ttu-id="24606-195">Trativod provádí s dlouhým přijímat dotazování, které trvá po 15 minut.</span><span class="sxs-lookup"><span data-stu-id="24606-195">The syphon performs a long poll receive that lasts 15 minutes.</span></span> <span data-ttu-id="24606-196">Když jsou k dispozici všechny entity a máte 10 nevyřízené položky fronty, aplikace, který je hostitelem Trativod volání operace příjmu 40 časy za hodinu, 960 časy denně a časy 28 800 bitů za 30 dní.</span><span class="sxs-lookup"><span data-stu-id="24606-196">When all entities are available and you have 10 backlog queues, the application that hosts the syphon calls the receive operation 40 times per hour, 960 times per day, and 28800 times in 30 days.</span></span> <span data-ttu-id="24606-197">Při Trativod je aktivně přesunu zprávy z nevyřízené položky do primární fronty, každá zpráva vyskytne následující poplatky (standardní poplatky za velikost zprávy a šířky pásma použít ve všech fázích):</span><span class="sxs-lookup"><span data-stu-id="24606-197">When the syphon is actively moving messages from the backlog to the primary queue, each message experiences the following charges (standard charges for message size and bandwidth apply in all stages):</span></span>

1. <span data-ttu-id="24606-198">Nevyřízené položky poslat.</span><span class="sxs-lookup"><span data-stu-id="24606-198">Send to the backlog.</span></span>
2. <span data-ttu-id="24606-199">Nevyřízené položky dostávat.</span><span class="sxs-lookup"><span data-stu-id="24606-199">Receive from the backlog.</span></span>
3. <span data-ttu-id="24606-200">Odeslat na primární.</span><span class="sxs-lookup"><span data-stu-id="24606-200">Send to the primary.</span></span>
4. <span data-ttu-id="24606-201">Přijímat z primární.</span><span class="sxs-lookup"><span data-stu-id="24606-201">Receive from the primary.</span></span>

## <a name="closefault-behavior"></a><span data-ttu-id="24606-202">Zavřít nebo selhání chování</span><span class="sxs-lookup"><span data-stu-id="24606-202">Close/fault behavior</span></span>
<span data-ttu-id="24606-203">V rámci aplikace, který je hostitelem Trativod, jednou primární nebo sekundární [MessagingFactory] [ MessagingFactory] chyb nebo je ukončeno, aniž se svým partnerem zároveň s chybou nebo zavře a Trativod zjistí tento stav Trativod jednání.</span><span class="sxs-lookup"><span data-stu-id="24606-203">Within an application that hosts the syphon, once the primary or secondary [MessagingFactory][MessagingFactory] faults or is closed without its partner also being faulted/closed and the syphon detects this state, the syphon acts.</span></span> <span data-ttu-id="24606-204">Pokud dalších [MessagingFactory] [ MessagingFactory] není uzavřený během 5 sekund, bude Trativod poruch stále otevřete [MessagingFactory][MessagingFactory].</span><span class="sxs-lookup"><span data-stu-id="24606-204">If the other [MessagingFactory][MessagingFactory] is not closed within 5 seconds, the syphon will fault the still open [MessagingFactory][MessagingFactory].</span></span>

## <a name="next-steps"></a><span data-ttu-id="24606-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="24606-205">Next steps</span></span>
<span data-ttu-id="24606-206">V tématu [asynchronní vzory a vysoká dostupnost pro zasílání zpráv] [ Asynchronous messaging patterns and high availability] pro podrobnou diskuzi o asynchronní zasílání zpráv Service Bus.</span><span class="sxs-lookup"><span data-stu-id="24606-206">See [Asynchronous messaging patterns and high availability][Asynchronous messaging patterns and high availability] for a detailed discussion of Service Bus asynchronous messaging.</span></span> 

[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PairNamespaceAsync_Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[MessageSender]: /dotnet/api/microsoft.servicebus.messaging.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[FailoverInterval]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions#Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_FailoverInterval
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[TimeSpan]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
[PingPrimaryInterval]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_PingPrimaryInterval
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[ContentType]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ContentType
[TimeToLive]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md
[0]: ./media/service-bus-paired-namespaces/IC673405.png
[1]: ./media/service-bus-paired-namespaces/IC673406.png
[2]: ./media/service-bus-paired-namespaces/IC673407.png