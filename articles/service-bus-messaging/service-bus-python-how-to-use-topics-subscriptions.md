---
title: "Jak používat témata služby Azure Service Bus s Pythonem | Microsoft Docs"
description: "Naučte se používat Azure Service Bus témata a odběry z Pythonu."
services: service-bus-messaging
documentationcenter: python
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4f1d76c-7567-4b33-9193-3788f82934e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 15269f9728e9dc45e6436e53b1859f76d4a7a0c9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-python"></a><span data-ttu-id="87565-103">Jak používat témata a odběry Service Bus s Pythonem</span><span class="sxs-lookup"><span data-stu-id="87565-103">How to use Service Bus topics and subscriptions with Python</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="87565-104">Tento článek popisuje, jak používat témata a odběry služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="87565-104">This article describes how to use Service Bus topics and subscriptions.</span></span> <span data-ttu-id="87565-105">Ukázky jsou napsané v Pythonu a použití [balíček Azure Python SDK][Azure Python package].</span><span class="sxs-lookup"><span data-stu-id="87565-105">The samples are written in Python and use the [Azure Python SDK package][Azure Python package].</span></span> <span data-ttu-id="87565-106">Pokryté scénáře zahrnují **vytváření témat a odběrů**, **vytváření filtrů odběrů**, **odesílání zpráv do tématu**, **přijímání zpráv z odběru**, a **odstranění témat a odběrů**.</span><span class="sxs-lookup"><span data-stu-id="87565-106">The scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages to a topic**, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span> <span data-ttu-id="87565-107">Další informace o tématech a odběrech najdete v tématu [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="87565-107">For more information about topics and subscriptions, see the [Next Steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

> [!NOTE] 
> <span data-ttu-id="87565-108">Pokud potřebujete k instalaci Pythonu nebo [balíček Azure Python][Azure Python package], najdete v článku [Průvodce instalací Python](../python-how-to-install.md).</span><span class="sxs-lookup"><span data-stu-id="87565-108">If you need to install Python or the [Azure Python package][Azure Python package], see the [Python Installation Guide](../python-how-to-install.md).</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="87565-109">Vytvoření tématu</span><span class="sxs-lookup"><span data-stu-id="87565-109">Create a topic</span></span>
<span data-ttu-id="87565-110">**ServiceBusService** objektu umožňuje pracovat s tématy.</span><span class="sxs-lookup"><span data-stu-id="87565-110">The **ServiceBusService** object enables you to work with topics.</span></span> <span data-ttu-id="87565-111">Přidejte následující v horní části všech soubor Python, ve kterém chcete programový přístupu k Service Bus:</span><span class="sxs-lookup"><span data-stu-id="87565-111">Add the following near the top of any Python file in which you wish to programmatically access Service Bus:</span></span>

```python
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

<span data-ttu-id="87565-112">Následující kód vytvoří **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="87565-112">The following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="87565-113">Nahraďte `mynamespace`, `sharedaccesskeyname`, a `sharedaccesskey` s vašeho oboru názvů skutečný název a klíč hodnotu klíče sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="87565-113">Replace `mynamespace`, `sharedaccesskeyname`, and `sharedaccesskey` with your actual namespace, Shared Access Signature (SAS) key name, and key value.</span></span>

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

<span data-ttu-id="87565-114">Můžete získat hodnoty pro název klíče SAS a hodnotu z [portál Azure][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="87565-114">You can obtain the values for the SAS key name and value from the [Azure portal][Azure portal].</span></span>

```python
bus_service.create_topic('mytopic')
```

<span data-ttu-id="87565-115">`create_topic` Metoda také podporuje další možnosti, které vám umožní přepsat výchozí nastavení téma například zpráva hodnota time to live nebo téma maximální velikost.</span><span class="sxs-lookup"><span data-stu-id="87565-115">The `create_topic` method also supports additional options, which enable you to override default topic settings such as message time to live or maximum topic size.</span></span> <span data-ttu-id="87565-116">Následující příklad nastaví téma maximální velikost 5 GB a čas live (TTL) hodnotu 1 minuta:</span><span class="sxs-lookup"><span data-stu-id="87565-116">The following example sets the maximum topic size to 5 GB, and a time to live (TTL) value of 1 minute:</span></span>

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a><span data-ttu-id="87565-117">Vytvořte odběr</span><span class="sxs-lookup"><span data-stu-id="87565-117">Create subscriptions</span></span>
<span data-ttu-id="87565-118">Odběry témat taky jsou vytvořeny pomocí **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="87565-118">Subscriptions to topics are also created with the **ServiceBusService** object.</span></span> <span data-ttu-id="87565-119">Odběry mají názvy a můžou mít volitelné filtry, které omezují výběr zpráv doručených do virtuální fronty odběru.</span><span class="sxs-lookup"><span data-stu-id="87565-119">Subscriptions are named and can have an optional filter that restricts the set of messages delivered to the subscription's virtual queue.</span></span>

> [!NOTE]
> <span data-ttu-id="87565-120">Odběry jsou trvalé a bude dál existovat, dokud buď jejich, nebo v tématu, ke kterému jsou předplatné, se odstraní.</span><span class="sxs-lookup"><span data-stu-id="87565-120">Subscriptions are persistent and will continue to exist until either they, or the topic to which they are subscribed, are deleted.</span></span>
> 
> 

### <a name="create-a-subscription-with-the-default-matchall-filter"></a><span data-ttu-id="87565-121">Vytvoření odběru s výchozím filtrem (MatchAll).</span><span class="sxs-lookup"><span data-stu-id="87565-121">Create a subscription with the default (MatchAll) filter</span></span>
<span data-ttu-id="87565-122">Filtr **MatchAll** je výchozí filtr, který se použije v případě, že při vytváření nového odběru nezadáte žádný filtr.</span><span class="sxs-lookup"><span data-stu-id="87565-122">The **MatchAll** filter is the default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="87565-123">Když **MatchAll** filtr se používá, všechny zprávy publikované do tématu jsou umístěny do virtuální fronty odběru.</span><span class="sxs-lookup"><span data-stu-id="87565-123">When the **MatchAll** filter is used, all messages published to the topic are placed in the subscription's virtual queue.</span></span> <span data-ttu-id="87565-124">Následující příklad vytvoří odběr s názvem `AllMessages` a používá výchozí **MatchAll** filtru.</span><span class="sxs-lookup"><span data-stu-id="87565-124">The following example creates a subscription named `AllMessages` and uses the default **MatchAll** filter.</span></span>

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="87565-125">Vytvoření odběru s filtry</span><span class="sxs-lookup"><span data-stu-id="87565-125">Create subscriptions with filters</span></span>
<span data-ttu-id="87565-126">Můžete také definovat filtry, které vám umožní určit, které by měl zprávy odeslané do tématu objeví v konkrétním odběru tématu.</span><span class="sxs-lookup"><span data-stu-id="87565-126">You can also define filters that enable you to specify which messages sent to a topic should show up within a specific topic subscription.</span></span>

<span data-ttu-id="87565-127">Nejflexibilnější filtr, který odběry podporují je **SqlFilter**, který implementuje je podmnožinou SQL92.</span><span class="sxs-lookup"><span data-stu-id="87565-127">The most flexible type of filter supported by subscriptions is a **SqlFilter**, which implements a subset of SQL92.</span></span> <span data-ttu-id="87565-128">Filtry SQL pracují s vlastnostmi zpráv publikované do tématu.</span><span class="sxs-lookup"><span data-stu-id="87565-128">SQL filters operate on the properties of the messages that are published to the topic.</span></span> <span data-ttu-id="87565-129">Další informace o výrazech, které se dají použít s filtrem SQL, najdete v syntaxi [SqlFilter.SqlExpression][SqlFilter.SqlExpression].</span><span class="sxs-lookup"><span data-stu-id="87565-129">For more information about the expressions that can be used with a SQL filter, see the [SqlFilter.SqlExpression][SqlFilter.SqlExpression] syntax.</span></span>

<span data-ttu-id="87565-130">Filtry k odběru můžete přidat pomocí **vytvořit\_pravidlo** metodu **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="87565-130">You can add filters to a subscription by using the **create\_rule** method of the **ServiceBusService** object.</span></span> <span data-ttu-id="87565-131">Tato metoda umožňuje přidat nové filtry k existujícímu předplatnému.</span><span class="sxs-lookup"><span data-stu-id="87565-131">This method allows you to add new filters to an existing subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="87565-132">Protože výchozí filtr je automaticky použita pro všechny nové odběry, je nutno nejprve odstranit výchozí filtr nebo **MatchAll** přepíše ostatní filtry, můžete určit.</span><span class="sxs-lookup"><span data-stu-id="87565-132">Because the default filter is applied automatically to all new subscriptions, you must first remove the default filter or the **MatchAll** will override any other filters you may specify.</span></span> <span data-ttu-id="87565-133">Výchozí pravidla můžete odebrat pomocí `delete_rule` metodu **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="87565-133">You can remove the default rule by using the `delete_rule` method of the **ServiceBusService** object.</span></span>
> 
> 

<span data-ttu-id="87565-134">Následující příklad vytvoří odběr s názvem `HighMessages` s **SqlFilter** který vybere jen zprávy, které mají vlastní `messagenumber` vlastnost větší než 3:</span><span class="sxs-lookup"><span data-stu-id="87565-134">The following example creates a subscription named `HighMessages` with a **SqlFilter** that only selects messages that have a custom `messagenumber` property greater than 3:</span></span>

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

<span data-ttu-id="87565-135">Podobně platí, tento příklad vytvoří odběr s názvem `LowMessages` s **SqlFilter** který vybere jen zprávy, které mají `messagenumber` vlastnost menší než nebo rovna na 3:</span><span class="sxs-lookup"><span data-stu-id="87565-135">Similarly, the following example creates a subscription named `LowMessages` with a **SqlFilter** that only selects messages that have a `messagenumber` property less than or equal to 3:</span></span>

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

<span data-ttu-id="87565-136">Teď, když je odeslána zpráva `mytopic` vždy se dodá příjemci **AllMessages** odběr tématu a selektivně příjemcům přihlásit k odběru **HighMessages** a **LowMessages** odběry témat (v závislosti na obsahu zprávy).</span><span class="sxs-lookup"><span data-stu-id="87565-136">Now, when a message is sent to `mytopic` it is always delivered to receivers subscribed to the **AllMessages** topic subscription, and selectively delivered to receivers subscribed to the **HighMessages** and **LowMessages** topic subscriptions (depending on the message content).</span></span>

## <a name="send-messages-to-a-topic"></a><span data-ttu-id="87565-137">Odeslání zprávy do tématu</span><span class="sxs-lookup"><span data-stu-id="87565-137">Send messages to a topic</span></span>
<span data-ttu-id="87565-138">K odeslání zprávy do tématu Service Bus, musí vaše aplikace používat `send_topic_message` metodu **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="87565-138">To send a message to a Service Bus topic, your application must use the `send_topic_message` method of the **ServiceBusService** object.</span></span>

<span data-ttu-id="87565-139">Následující příklad ukazuje, jak odeslat pět zkušebních zpráv do `mytopic`.</span><span class="sxs-lookup"><span data-stu-id="87565-139">The following example demonstrates how to send five test messages to `mytopic`.</span></span> <span data-ttu-id="87565-140">Všimněte si, že `messagenumber` hodnota vlastnosti každé zprávy se liší na iteraci smyčky (to určuje, které odběry ji přijmou):</span><span class="sxs-lookup"><span data-stu-id="87565-140">Note that the `messagenumber` property value of each message varies on the iteration of the loop (this determines which subscriptions receive it):</span></span>

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

<span data-ttu-id="87565-141">Témata Service Bus podporují maximální velikost zprávy 256 KB [na úrovni Standard](service-bus-premium-messaging.md) a 1 MB [na úrovni Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="87565-141">Service Bus topics support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="87565-142">Hlavička, která obsahuje standardní a vlastní vlastnosti aplikace, může mít velikost až 64 KB.</span><span class="sxs-lookup"><span data-stu-id="87565-142">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="87565-143">Počet zpráv držených v tématu není omezený, ale celková velikost zpráv držených v tématu omezená je.</span><span class="sxs-lookup"><span data-stu-id="87565-143">There is no limit on the number of messages held in a topic but there is a cap on the total size of the messages held by a topic.</span></span> <span data-ttu-id="87565-144">Velikost tématu se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="87565-144">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span> <span data-ttu-id="87565-145">Další informace o kvótách najdete v tématu [Service Bus kvóty][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="87565-145">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="87565-146">Příjem zpráv z odběru</span><span class="sxs-lookup"><span data-stu-id="87565-146">Receive messages from a subscription</span></span>
<span data-ttu-id="87565-147">Přijímání zpráv z odběru pomocí `receive_subscription_message` metodu **ServiceBusService** objektu:</span><span class="sxs-lookup"><span data-stu-id="87565-147">Messages are received from a subscription using the `receive_subscription_message` method on the **ServiceBusService** object:</span></span>

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

<span data-ttu-id="87565-148">Zprávy jsou odstraněny z předplatného, jako při jejich přečtení parametr `peek_lock` je nastaven na **False**.</span><span class="sxs-lookup"><span data-stu-id="87565-148">Messages are deleted from the subscription as they are read when the parameter `peek_lock` is set to **False**.</span></span> <span data-ttu-id="87565-149">Můžete číst (funkce Náhled) a uzamčení zpráva bez odstranění z fronty nastavením parametru `peek_lock` k **True**.</span><span class="sxs-lookup"><span data-stu-id="87565-149">You can read (peek) and lock the message without deleting it from the queue by setting the parameter `peek_lock` to **True**.</span></span>

<span data-ttu-id="87565-150">Chování čtení a odstranění zprávy v rámci operace příjmu je nejjednodušší model a funguje nejlépe pro scénáře, ve kterých aplikace může tolerovat selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="87565-150">The behavior of reading and deleting the message as part of the receive operation is the simplest model, and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="87565-151">Pro lepší vysvětlení si představte scénář, ve kterém spotřebitel vyšle požadavek na přijetí, ale než ji může zpracovat, dojde v něm k chybě a ukončí se.</span><span class="sxs-lookup"><span data-stu-id="87565-151">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="87565-152">Vzhledem k tomu, že Service Bus se už ale zprávu označila jako spotřebovávanou, pak když se aplikace restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily zprávu, která se spotřebovala před havárii.</span><span class="sxs-lookup"><span data-stu-id="87565-152">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="87565-153">Pokud `peek_lock` parametr je nastaven na **True**, receive stane dvoufázového operaci, která umožňuje podporuje aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="87565-153">If the `peek_lock` parameter is set to **True**, the receive becomes a two stage operation, which makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="87565-154">Když Service Bus přijme požadavek, najde zprávu, která je na řadě ke spotřebování, uzamkne ji proti spotřebování jinými spotřebiteli a vrátí ji do aplikace.</span><span class="sxs-lookup"><span data-stu-id="87565-154">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span> <span data-ttu-id="87565-155">Když aplikace dokončí zpracování zprávy (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze přijetí volání `delete` metodu **zpráva** objektu.</span><span class="sxs-lookup"><span data-stu-id="87565-155">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling `delete` method on the **Message** object.</span></span> <span data-ttu-id="87565-156">`delete` Metoda označí zprávu jako spotřebovávanou a odstraní ji z odběru.</span><span class="sxs-lookup"><span data-stu-id="87565-156">The `delete` method marks the message as being consumed and removes it from the subscription.</span></span>

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="87565-157">Zpracování pádů aplikace a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="87565-157">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="87565-158">Service Bus poskytuje funkce, které vám pomůžou se elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="87565-158">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="87565-159">Pokud přijímající aplikace nedokáže zpracovat zprávu z nějakého důvodu, pak může zavolat `unlock` metodu **zpráva** objektu.</span><span class="sxs-lookup"><span data-stu-id="87565-159">If a receiver application is unable to process the message for some reason, then it can call the `unlock` method on the **Message** object.</span></span> <span data-ttu-id="87565-160">To způsobí, že Service Bus zprávu odemkne v odběru a zpřístupní ji pro další přijetí, stejnou spotřebitelskou aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="87565-160">This will cause Service Bus to unlock the message within the subscription and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="87565-161">Je také vypršení časového limitu zpráva uzamčená v odběru, a pokud se nepodaří aplikace zprávu nezpracuje zámku vyprší časový limit (například pokud aplikace spadne), pak Service Bus zprávu automaticky odemkne a ji zpřístupní k přijetí.</span><span class="sxs-lookup"><span data-stu-id="87565-161">There is also a timeout associated with a message locked within the subscription, and if the application fails to process the message before the lock timeout expires (for example, if the application crashes), then Service Bus unlocks the message automatically and makes it available to be received again.</span></span>

<span data-ttu-id="87565-162">V případě, že aplikace spadne po zpracování zprávy, ale předtím, než `delete` metoda je volána, pak zpráva bude vysláním do aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="87565-162">In the event that the application crashes after processing the message but before the `delete` method is called, then the message will be redelivered to the application when it restarts.</span></span> <span data-ttu-id="87565-163">To se často označuje jako *zpracování nejméně jednou*, který je každá zpráva se zpracuje alespoň jednou, ale v některých situacích může doručit víckrát stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="87565-163">This is often called *At Least Once Processing*, that is, each message will be processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="87565-164">Pokud daný scénář nemůže tolerovat zpracování víc než jednou, vývojáři aplikace by měli přidat další logiku navíc pro zpracování víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="87565-164">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="87565-165">To se často opírá **MessageId** vlastnosti zprávy, která zůstane konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="87565-165">This is often achieved using the **MessageId** property of the message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="87565-166">Odstranění témat a odběrů</span><span class="sxs-lookup"><span data-stu-id="87565-166">Delete topics and subscriptions</span></span>
<span data-ttu-id="87565-167">Témata a odběry jsou trvalé a musí být explicitně odstranit buď prostřednictvím [portál Azure] [ Azure portal] nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="87565-167">Topics and subscriptions are persistent, and must be explicitly deleted either through the [Azure portal][Azure portal] or programmatically.</span></span> <span data-ttu-id="87565-168">Následující příklad ukazuje, jak odstranit téma s názvem `mytopic`:</span><span class="sxs-lookup"><span data-stu-id="87565-168">The following example shows how to delete the topic named `mytopic`:</span></span>

```python
bus_service.delete_topic('mytopic')
```

<span data-ttu-id="87565-169">Pokud se odstraní téma, odstraní se i všechny odběry registrované k tomuto tématu.</span><span class="sxs-lookup"><span data-stu-id="87565-169">Deleting a topic also deletes any subscriptions that are registered with the topic.</span></span> <span data-ttu-id="87565-170">Odběry se taky dají odstranit samostatně.</span><span class="sxs-lookup"><span data-stu-id="87565-170">Subscriptions can also be deleted independently.</span></span> <span data-ttu-id="87565-171">Následující kód ukazuje, jak odstranit odběr s názvem `HighMessages` z `mytopic` tématu:</span><span class="sxs-lookup"><span data-stu-id="87565-171">The following code shows how to delete a subscription named `HighMessages` from the `mytopic` topic:</span></span>

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a><span data-ttu-id="87565-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87565-172">Next steps</span></span>
<span data-ttu-id="87565-173">Teď, když jste se naučili základy témat sběrnice Service Bus, postupujte podle následujících odkazech na další informace.</span><span class="sxs-lookup"><span data-stu-id="87565-173">Now that you've learned the basics of Service Bus topics, follow these links to learn more.</span></span>

* <span data-ttu-id="87565-174">V tématu [fronty, témata a odběry][Queues, topics, and subscriptions].</span><span class="sxs-lookup"><span data-stu-id="87565-174">See [Queues, topics, and subscriptions][Queues, topics, and subscriptions].</span></span>
* <span data-ttu-id="87565-175">Reference pro [SqlFilter.SqlExpression][SqlFilter.SqlExpression].</span><span class="sxs-lookup"><span data-stu-id="87565-175">Reference for [SqlFilter.SqlExpression][SqlFilter.SqlExpression].</span></span>

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
