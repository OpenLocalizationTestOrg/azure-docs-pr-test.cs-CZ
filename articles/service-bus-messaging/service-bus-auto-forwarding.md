---
title: "předávání aaaAuto entity zasílání zpráv Azure Service Bus | Microsoft Docs"
description: "Jak toochain Service Bus fronty nebo fronty odběru tooanother nebo téma."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f7060778-3421-402c-97c7-735dbf6a61e8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: af18273e4e2f81c5363eb830c7decf313afd8307
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="chaining-service-bus-entities-with-auto-forwarding"></a><span data-ttu-id="60e88-103">Řetězení entit služby Service Bus s automatické přesměrování</span><span class="sxs-lookup"><span data-stu-id="60e88-103">Chaining Service Bus entities with auto-forwarding</span></span>

<span data-ttu-id="60e88-104">Hello Service Bus *automatické přesměrování* funkce vám umožní toochain fronty nebo fronty odběru tooanother nebo téma, které je součástí hello stejný obor názvů.</span><span class="sxs-lookup"><span data-stu-id="60e88-104">hello Service Bus *auto-forwarding* feature enables you toochain a queue or subscription tooanother queue or topic that is part of hello same namespace.</span></span> <span data-ttu-id="60e88-105">Pokud je povoleno automatické přesměrování, Service Bus automaticky odebere zprávy, které jsou umístěny v první fronty hello nebo předplatné (zdroj) a umístí je do hello druhý fronta nebo téma (cíl).</span><span class="sxs-lookup"><span data-stu-id="60e88-105">When auto-forwarding is enabled, Service Bus automatically removes messages that are placed in hello first queue or subscription (source) and puts them in hello second queue or topic (destination).</span></span> <span data-ttu-id="60e88-106">Všimněte si, že je stále možné toosend zpráva toohello cílové entity přímo.</span><span class="sxs-lookup"><span data-stu-id="60e88-106">Note that it is still possible toosend a message toohello destination entity directly.</span></span> <span data-ttu-id="60e88-107">Navíc není možné toochain dílčí fronta, například fronty nedoručených zpráv, tooanother fronta nebo téma.</span><span class="sxs-lookup"><span data-stu-id="60e88-107">Also, it is not possible toochain a subqueue, such as a deadletter queue, tooanother queue or topic.</span></span>

## <a name="using-auto-forwarding"></a><span data-ttu-id="60e88-108">Pomocí automatické přesměrování</span><span class="sxs-lookup"><span data-stu-id="60e88-108">Using auto-forwarding</span></span>
<span data-ttu-id="60e88-109">Můžete povolit automatické přesměrování nastavení hello [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] nebo [SubscriptionDescription.ForwardTo] [ SubscriptionDescription.ForwardTo] Vlastnosti hello [QueueDescription] [ QueueDescription] nebo [SubscriptionDescription] [ SubscriptionDescription] objekty pro zdroj hello jako hello Následující příklad.</span><span class="sxs-lookup"><span data-stu-id="60e88-109">You can enable auto-forwarding by setting hello [QueueDescription.ForwardTo][QueueDescription.ForwardTo] or [SubscriptionDescription.ForwardTo][SubscriptionDescription.ForwardTo] properties on hello [QueueDescription][QueueDescription] or [SubscriptionDescription][SubscriptionDescription] objects for hello source, as in hello following example.</span></span>

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

<span data-ttu-id="60e88-110">Hello cílové entity, musí existovat ve hello dobu, kdy se vytvoří hello zdrojové entity.</span><span class="sxs-lookup"><span data-stu-id="60e88-110">hello destination entity must exist at hello time hello source entity is created.</span></span> <span data-ttu-id="60e88-111">Pokud hello Cílová entita neexistuje, vrátí Service Bus výjimku, pokud kladené toocreate hello zdrojové entity.</span><span class="sxs-lookup"><span data-stu-id="60e88-111">If hello destination entity does not exist, Service Bus returns an exception when asked toocreate hello source entity.</span></span>

<span data-ttu-id="60e88-112">Můžete použít automatické přesměrování tooscale na jednotlivé tématu.</span><span class="sxs-lookup"><span data-stu-id="60e88-112">You can use auto-forwarding tooscale out an individual topic.</span></span> <span data-ttu-id="60e88-113">Service Bus omezení hello [počet předplatných na dané téma](service-bus-quotas.md) too2, 000.</span><span class="sxs-lookup"><span data-stu-id="60e88-113">Service Bus limits hello [number of subscriptions on a given topic](service-bus-quotas.md) too2,000.</span></span> <span data-ttu-id="60e88-114">Další odběry zvládne vytvořením témata druhé úrovně.</span><span class="sxs-lookup"><span data-stu-id="60e88-114">You can accommodate additional subscriptions by creating second-level topics.</span></span> <span data-ttu-id="60e88-115">Všimněte si, že i v případě, že nejsou svázaná s hello může zvýšit omezení hello počtu předplatných, přidání druhou úroveň témat Service Bus hello celkovou propustnost téma.</span><span class="sxs-lookup"><span data-stu-id="60e88-115">Note that even if you are not bound by hello Service Bus limitation on hello number of subscriptions, adding a second level of topics can improve hello overall throughput of your topic.</span></span>

![Scénáře automatické přesměrování][0]

<span data-ttu-id="60e88-117">Můžete také použít odesílatelé zpráv toodecouple automatické přesměrování z příjemců.</span><span class="sxs-lookup"><span data-stu-id="60e88-117">You can also use auto-forwarding toodecouple message senders from receivers.</span></span> <span data-ttu-id="60e88-118">Představte si třeba ERP systém, který se skládá ze tří modulů: pořadí zpracování, Správa inventáře a řízení vztahů se zákazníky.</span><span class="sxs-lookup"><span data-stu-id="60e88-118">For example, consider an ERP system that consists of three modules: order processing, inventory management, and customer relations management.</span></span> <span data-ttu-id="60e88-119">Každý z těchto modulů generuje zprávy, které jsou zařazených do fronty do příslušné téma.</span><span class="sxs-lookup"><span data-stu-id="60e88-119">Each of these modules generates messages that are enqueued into a corresponding topic.</span></span> <span data-ttu-id="60e88-120">Alice a Bob jsou obchodní zástupce, kteří se chtějí všechny zprávy, které se týkají tootheir zákazníků.</span><span class="sxs-lookup"><span data-stu-id="60e88-120">Alice and Bob are sales representatives that are interested in all messages that relate tootheir customers.</span></span> <span data-ttu-id="60e88-121">tooreceive ty zprávy, Alice a Bob vytvořit osobní fronty a odběru na každém hello ERP témat, která automaticky předávat všechny tootheir fronty zpráv.</span><span class="sxs-lookup"><span data-stu-id="60e88-121">tooreceive those messages, Alice and Bob each create a personal queue and a subscription on each of hello ERP topics that automatically forward all messages tootheir queue.</span></span>

![Scénáře automatické přesměrování][1]

<span data-ttu-id="60e88-123">Pokud Alice přejde na dovolenou, svůj osobní fronty, nikoli hello ERP tématu, zaplní.</span><span class="sxs-lookup"><span data-stu-id="60e88-123">If Alice goes on vacation, her personal queue, rather than hello ERP topic, fills up.</span></span> <span data-ttu-id="60e88-124">V tomto scénáři protože obchodním zástupcem neobdržel všechny zprávy žádné témat ERP hello někdy dosáhnout kvóty.</span><span class="sxs-lookup"><span data-stu-id="60e88-124">In this scenario, because a sales representative has not received any messages, none of hello ERP topics ever reach quota.</span></span>

## <a name="auto-forwarding-considerations"></a><span data-ttu-id="60e88-125">Důležité informace o automatické přesměrování</span><span class="sxs-lookup"><span data-stu-id="60e88-125">Auto-forwarding considerations</span></span>

<span data-ttu-id="60e88-126">Pokud hello cílové entity shromažďuje příliš mnoho zpráv a překračuje kvótu hello nebo hello cílové entity je zakázaná, přidá hello zdrojové entitě hello zprávy tooits [frontu nedoručených zpráv](service-bus-dead-letter-queues.md) dokud není místa v cílovém umístění hello (nebo entita hello je znovu povolit).</span><span class="sxs-lookup"><span data-stu-id="60e88-126">If hello destination entity accumulates too many messages and exceeds hello quota, or hello destination entity is disabled, hello source entity adds hello messages tooits [dead-letter queue](service-bus-dead-letter-queues.md) until there is space in hello destination (or hello entity is re-enabled).</span></span> <span data-ttu-id="60e88-127">Tyto zprávy bude pokračovat toolive do fronty nedoručených zpráv hello, takže se musí explicitně zobrazí a jejich zpracování z fronty nedoručených zpráv hello.</span><span class="sxs-lookup"><span data-stu-id="60e88-127">Those messages will continue toolive in hello dead-letter queue, so you must explicitly receive and process them from hello dead-letter queue.</span></span>

<span data-ttu-id="60e88-128">Pokud řetězení společně jednotlivých témata tooobtain složené téma se mnoho odběrů, doporučuje se, že máte středně velkým počtem odběry tématu první úrovně hello a mnoho odběrů na témata hello druhé úrovně.</span><span class="sxs-lookup"><span data-stu-id="60e88-128">When chaining together individual topics tooobtain a composite topic with many subscriptions, it is recommended that you have a moderate number of subscriptions on hello first-level topic and many subscriptions on hello second-level topics.</span></span> <span data-ttu-id="60e88-129">Například první úrovně téma s 20 odběry, každý z nich zřetězené tooa téma druhé úrovně se 200 odběry, umožňuje vyšší propustnost než první úrovně téma se 200 předplatná každý zřetězené tooa téma druhé úrovně se 20 odběrů .</span><span class="sxs-lookup"><span data-stu-id="60e88-129">For example, a first-level topic with 20 subscriptions, each of them chained tooa second-level topic with 200 subscriptions, allows for higher throughput than a first-level topic with 200 subscriptions, each chained tooa second-level topic with 20 subscriptions.</span></span>

<span data-ttu-id="60e88-130">Service Bus bills jednu operaci pro každý přesměrovaná zpráva.</span><span class="sxs-lookup"><span data-stu-id="60e88-130">Service Bus bills one operation for each forwarded message.</span></span> <span data-ttu-id="60e88-131">Například odesílání zpráv tooa téma s 20 odběry, každý z nich nakonfigurován tooauto předání zprávy tooanother fronta nebo téma, se fakturuje jako 21 operace Pokud všechny odběry první úrovně obdrží kopii zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="60e88-131">For example, sending a message tooa topic with 20 subscriptions, each of them configured tooauto-forward messages tooanother queue or topic, is billed as 21 operations if all first-level subscriptions receive a copy of hello message.</span></span>

<span data-ttu-id="60e88-132">toocreate předplatné, které je zřetězené tooanother fronta nebo téma, musí mít hello Tvůrce odběru hello **spravovat** oprávnění na hello zdrojové i cílové entity hello.</span><span class="sxs-lookup"><span data-stu-id="60e88-132">toocreate a subscription that is chained tooanother queue or topic, hello creator of hello subscription must have **Manage** permissions on both hello source and hello destination entity.</span></span> <span data-ttu-id="60e88-133">Odeslání zprávy toohello zdroj tématu vyžaduje pouze **odeslat** oprávnění k tématu zdroj hello.</span><span class="sxs-lookup"><span data-stu-id="60e88-133">Sending messages toohello source topic only requires **Send** permissions on hello source topic.</span></span>

## <a name="next-steps"></a><span data-ttu-id="60e88-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60e88-134">Next steps</span></span>

<span data-ttu-id="60e88-135">Podrobné informace o automatické přesměrování najdete v části hello následující odkazy na témata:</span><span class="sxs-lookup"><span data-stu-id="60e88-135">For detailed information about auto-forwarding, see hello following reference topics:</span></span>

* <span data-ttu-id="60e88-136">[ForwardTo][QueueDescription.ForwardTo]</span><span class="sxs-lookup"><span data-stu-id="60e88-136">[ForwardTo][QueueDescription.ForwardTo]</span></span>
* <span data-ttu-id="60e88-137">[QueueDescription][QueueDescription]</span><span class="sxs-lookup"><span data-stu-id="60e88-137">[QueueDescription][QueueDescription]</span></span>
* <span data-ttu-id="60e88-138">[SubscriptionDescription][SubscriptionDescription]</span><span class="sxs-lookup"><span data-stu-id="60e88-138">[SubscriptionDescription][SubscriptionDescription]</span></span>

<span data-ttu-id="60e88-139">toolearn Další informace o vylepšení výkonu služby Service Bus, najdete v části</span><span class="sxs-lookup"><span data-stu-id="60e88-139">toolearn more about Service Bus performance improvements, see</span></span> 

* [<span data-ttu-id="60e88-140">Osvědčené postupy pro zlepšení výkonu pomocí zasílání zpráv Service Bus</span><span class="sxs-lookup"><span data-stu-id="60e88-140">Best Practices for performance improvements using Service Bus Messaging</span></span>](service-bus-performance-improvements.md)
* <span data-ttu-id="60e88-141">[Segmentované entity zasílání zpráv][Partitioned messaging entities].</span><span class="sxs-lookup"><span data-stu-id="60e88-141">[Partitioned messaging entities][Partitioned messaging entities].</span></span>

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md
