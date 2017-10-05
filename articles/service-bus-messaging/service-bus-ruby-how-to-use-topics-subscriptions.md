---
title: "Jak používat témata Service Bus (Ruby) | Microsoft Docs"
description: "Naučte se používat témata a odběry Service Bus v Azure. Ukázky kódu jsou napsané pro poznámky Ruby aplikace."
services: service-bus-messaging
documentationcenter: ruby
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3ef2295e-7c5f-4c54-a13b-a69c8045d4b6
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 4a4c9949843b16ae6be2f516de4fd1e3f7415959
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-ruby"></a><span data-ttu-id="30a06-104">Jak používat témata a odběry Service Bus s Ruby</span><span class="sxs-lookup"><span data-stu-id="30a06-104">How to use Service Bus topics and subscriptions with Ruby</span></span>
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="30a06-105">Tento článek popisuje, jak používat témata Service Bus a odběry z Ruby aplikací.</span><span class="sxs-lookup"><span data-stu-id="30a06-105">This article describes how to use Service Bus topics and subscriptions from Ruby applications.</span></span> <span data-ttu-id="30a06-106">Pokryté scénáře zahrnují **vytváření témat a odběrů, vytváření filtrů odběrů, odesílání zpráv** do tématu, **přijímání zpráv z odběru**, a **odstranění témat a odběrů**.</span><span class="sxs-lookup"><span data-stu-id="30a06-106">The scenarios covered include **creating topics and subscriptions, creating subscription filters, sending messages** to a topic, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span> <span data-ttu-id="30a06-107">Další informace o témat a odběrů, najdete v článku [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="30a06-107">For more information on topics and subscriptions, see the [Next Steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a><span data-ttu-id="30a06-108">Vytvoření tématu</span><span class="sxs-lookup"><span data-stu-id="30a06-108">Create a topic</span></span>
<span data-ttu-id="30a06-109">**Azure::ServiceBusService** objektu umožňuje pracovat s tématy.</span><span class="sxs-lookup"><span data-stu-id="30a06-109">The **Azure::ServiceBusService** object enables you to work with topics.</span></span> <span data-ttu-id="30a06-110">Následující kód vytvoří **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="30a06-110">The following code creates an **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="30a06-111">Chcete-li vytvořit téma, použijte `create_topic()` metoda.</span><span class="sxs-lookup"><span data-stu-id="30a06-111">To create a topic, use the `create_topic()` method.</span></span> <span data-ttu-id="30a06-112">Následující příklad vytvoří téma nebo vytiskne chyby, pokud jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="30a06-112">The following example creates a topic or prints out the errors if there are any.</span></span>

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

<span data-ttu-id="30a06-113">Můžete také předat **Azure::ServiceBus::Topic** objekt s další možnosti, které vám umožní přepsat výchozí nastavení téma například čas zprávy do fronty za provozu nebo maximální velikost.</span><span class="sxs-lookup"><span data-stu-id="30a06-113">You can also pass an **Azure::ServiceBus::Topic** object with additional options, which enable you to override default topic settings such as message time to live or maximum queue size.</span></span> <span data-ttu-id="30a06-114">Následující příklad ukazuje, nastavení maximální velikost fronty 5 GB a čas TTL na 1 minutu:</span><span class="sxs-lookup"><span data-stu-id="30a06-114">The following example shows setting the maximum queue size to 5 GB and time to live to 1 minute:</span></span>

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a><span data-ttu-id="30a06-115">Vytvořte odběr</span><span class="sxs-lookup"><span data-stu-id="30a06-115">Create subscriptions</span></span>
<span data-ttu-id="30a06-116">Odběry témat taky jsou vytvořeny pomocí **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="30a06-116">Topic subscriptions are also created with the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="30a06-117">Odběry mají názvy a můžou mít volitelné filtry, které omezují výběr zpráv doručených do virtuální fronty odběru.</span><span class="sxs-lookup"><span data-stu-id="30a06-117">Subscriptions are named and can have an optional filter that restricts the set of messages delivered to the subscription's virtual queue.</span></span>

<span data-ttu-id="30a06-118">Odběry jsou trvalé a bude dál existovat, dokud buď jejich nebo tématu jsou spojeny s, se odstraní.</span><span class="sxs-lookup"><span data-stu-id="30a06-118">Subscriptions are persistent and will continue to exist until either they, or the topic they are associated with, are deleted.</span></span> <span data-ttu-id="30a06-119">Pokud vaše aplikace obsahuje logiku pro vytvoření odběru, musí nejprve zkontrolujte Pokud předplatné už existuje. pomocí metody getSubscription.</span><span class="sxs-lookup"><span data-stu-id="30a06-119">If your application contains logic to create a subscription, it should first check if the subscription already exists by using the getSubscription method.</span></span>

### <a name="create-a-subscription-with-the-default-matchall-filter"></a><span data-ttu-id="30a06-120">Vytvoření odběru s výchozím filtrem (MatchAll).</span><span class="sxs-lookup"><span data-stu-id="30a06-120">Create a subscription with the default (MatchAll) filter</span></span>
<span data-ttu-id="30a06-121">Filtr **MatchAll** je výchozí filtr, který se použije v případě, že při vytváření nového odběru nezadáte žádný filtr.</span><span class="sxs-lookup"><span data-stu-id="30a06-121">The **MatchAll** filter is the default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="30a06-122">Když **MatchAll** filtr se používá, všechny zprávy publikované do tématu jsou umístěny do virtuální fronty odběru.</span><span class="sxs-lookup"><span data-stu-id="30a06-122">When the **MatchAll** filter is used, all messages published to the topic are placed in the subscription's virtual queue.</span></span> <span data-ttu-id="30a06-123">Následující příklad vytvoří odběr s názvem "všechna zprávy" a používá výchozí **MatchAll** filtru.</span><span class="sxs-lookup"><span data-stu-id="30a06-123">The following example creates a subscription named "all-messages" and uses the default **MatchAll** filter.</span></span>

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="30a06-124">Vytvoření odběru s filtry</span><span class="sxs-lookup"><span data-stu-id="30a06-124">Create subscriptions with filters</span></span>
<span data-ttu-id="30a06-125">Můžete také definovat filtry, které vám umožní určit, které by měl zprávy odeslané do tématu zobrazit do konkrétní předplatné.</span><span class="sxs-lookup"><span data-stu-id="30a06-125">You can also define filters that enable you to specify which messages sent to a topic should show up within a specific subscription.</span></span>

<span data-ttu-id="30a06-126">Nejflexibilnější filtr, který odběry podporují je **Azure::ServiceBus::SqlFilter**, který implementuje je podmnožinou SQL92.</span><span class="sxs-lookup"><span data-stu-id="30a06-126">The most flexible type of filter supported by subscriptions is the **Azure::ServiceBus::SqlFilter**, which implements a subset of SQL92.</span></span> <span data-ttu-id="30a06-127">Filtry SQL pracují s vlastnostmi zpráv publikované do tématu.</span><span class="sxs-lookup"><span data-stu-id="30a06-127">SQL filters operate on the properties of the messages that are published to the topic.</span></span> <span data-ttu-id="30a06-128">Další podrobnosti o výrazy, které lze použít s filtrem SQL, projděte si [SqlFilter](service-bus-messaging-sql-filter.md) syntaxe.</span><span class="sxs-lookup"><span data-stu-id="30a06-128">For more details about the expressions that can be used with a SQL filter, review the [SqlFilter](service-bus-messaging-sql-filter.md) syntax.</span></span>

<span data-ttu-id="30a06-129">Filtry k odběru můžete přidat pomocí `create_rule()` metodu **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="30a06-129">You can add filters to a subscription by using the `create_rule()` method of the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="30a06-130">Tato metoda umožňuje přidat nové filtry k existujícímu předplatnému.</span><span class="sxs-lookup"><span data-stu-id="30a06-130">This method enables you to add new filters to an existing subscription.</span></span>

<span data-ttu-id="30a06-131">Vzhledem k tomu, že výchozí filtr automaticky použije na všechny nové odběry, je nutno nejprve odstranit výchozí filtr, nebo **MatchAll** přepíše ostatní filtry, můžete určit.</span><span class="sxs-lookup"><span data-stu-id="30a06-131">Since the default filter is applied automatically to all new subscriptions, you must first remove the default filter, or the **MatchAll** will override any other filters you may specify.</span></span> <span data-ttu-id="30a06-132">Výchozí pravidla můžete odebrat pomocí `delete_rule()` metodu **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="30a06-132">You can remove the default rule by using the `delete_rule()` method on the **Azure::ServiceBusService** object.</span></span>

<span data-ttu-id="30a06-133">Následující příklad vytvoří odběr s názvem "vysoce zprávy" se **Azure::ServiceBus::SqlFilter** který vybere jen zprávy, které mají vlastní `message_number` vlastnost větší než 3:</span><span class="sxs-lookup"><span data-stu-id="30a06-133">The following example creates a subscription named "high-messages" with an **Azure::ServiceBus::SqlFilter** that only selects messages that have a custom `message_number` property greater than 3:</span></span>

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

<span data-ttu-id="30a06-134">Podobně platí, tento příklad vytvoří odběr s názvem `low-messages` s **Azure::ServiceBus::SqlFilter** který vybere jen zprávy, které mají `message_number` vlastnost menší než nebo rovna na 3:</span><span class="sxs-lookup"><span data-stu-id="30a06-134">Similarly, the following example creates a subscription named `low-messages` with an **Azure::ServiceBus::SqlFilter** that only selects messages that have a `message_number` property less than or equal to 3:</span></span>

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

<span data-ttu-id="30a06-135">Když je nyní odeslána zpráva `test-topic`, je vždy doručena příjemci `all-messages` odběr tématu a selektivně příjemcům přihlásit k odběru `high-messages` a `low-messages` odběry témat (v závislosti na obsahu zprávy).</span><span class="sxs-lookup"><span data-stu-id="30a06-135">When a message is now sent to `test-topic`, it is always be delivered to receivers subscribed to the `all-messages` topic subscription, and selectively delivered to receivers subscribed to the `high-messages` and `low-messages` topic subscriptions (depending upon the message content).</span></span>

## <a name="send-messages-to-a-topic"></a><span data-ttu-id="30a06-136">Odeslání zprávy do tématu</span><span class="sxs-lookup"><span data-stu-id="30a06-136">Send messages to a topic</span></span>
<span data-ttu-id="30a06-137">K odeslání zprávy do tématu Service Bus, musí vaše aplikace používat `send_topic_message()` metodu **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="30a06-137">To send a message to a Service Bus topic, your application must use the `send_topic_message()` method on the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="30a06-138">Zprávy odeslané do témat Service Bus jsou instance **Azure::ServiceBus::BrokeredMessage** objekty.</span><span class="sxs-lookup"><span data-stu-id="30a06-138">Messages sent to Service Bus topics are instances of the **Azure::ServiceBus::BrokeredMessage** objects.</span></span> <span data-ttu-id="30a06-139">**Azure::ServiceBus::BrokeredMessage** objekty mají sadu standardních vlastností (jako například `label` a `time_to_live`), slovník používaný pro udržení vlastních vlastností specifické pro aplikaci a obsah řetězec dat.</span><span class="sxs-lookup"><span data-stu-id="30a06-139">**Azure::ServiceBus::BrokeredMessage** objects have a set of standard properties (such as `label` and `time_to_live`), a dictionary that is used to hold custom application-specific properties, and a body of string data.</span></span> <span data-ttu-id="30a06-140">Aplikace může tělo zprávy nastavit předáním řetězcové hodnoty `send_topic_message()` metoda a všechny nezbytné standardní vlastnosti vyplní výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="30a06-140">An application can set the body of the message by passing a string value to the `send_topic_message()` method and any required standard properties will be populated by default values.</span></span>

<span data-ttu-id="30a06-141">Následující příklad ukazuje, jak odeslat pět zkušebních zpráv do `test-topic`.</span><span class="sxs-lookup"><span data-stu-id="30a06-141">The following example demonstrates how to send five test messages to `test-topic`.</span></span> <span data-ttu-id="30a06-142">Všimněte si, že `message_number` hodnotu vlastní vlastnosti každé zprávy se liší na iteraci smyčky (to určuje, jaké předplatné přijetí):</span><span class="sxs-lookup"><span data-stu-id="30a06-142">Note that the `message_number` custom property value of each message varies on the iteration of the loop (this determines which subscription receives it):</span></span>

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

<span data-ttu-id="30a06-143">Témata Service Bus podporují maximální velikost zprávy 256 KB [na úrovni Standard](service-bus-premium-messaging.md) a 1 MB [na úrovni Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="30a06-143">Service Bus topics support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="30a06-144">Hlavička, která obsahuje standardní a vlastní vlastnosti aplikace, může mít velikost až 64 KB.</span><span class="sxs-lookup"><span data-stu-id="30a06-144">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="30a06-145">Počet zpráv držených v tématu není omezený, ale celková velikost zpráv držených v tématu omezená je.</span><span class="sxs-lookup"><span data-stu-id="30a06-145">There is no limit on the number of messages held in a topic but there is a cap on the total size of the messages held by a topic.</span></span> <span data-ttu-id="30a06-146">Velikost tématu se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="30a06-146">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="30a06-147">Příjem zpráv z odběru</span><span class="sxs-lookup"><span data-stu-id="30a06-147">Receive messages from a subscription</span></span>
<span data-ttu-id="30a06-148">Přijímání zpráv z odběru pomocí `receive_subscription_message()` metodu **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="30a06-148">Messages are received from a subscription using the `receive_subscription_message()` method on the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="30a06-149">Ve výchozím nastavení zprávy jsou read(peak) a uzamčení bez odstranění z předplatného.</span><span class="sxs-lookup"><span data-stu-id="30a06-149">By default, messages are read(peak) and locked without deleting it from the subscription.</span></span> <span data-ttu-id="30a06-150">Můžete číst a odstranění zprávy z odběru nastavením `peek_lock` možnost k **false**.</span><span class="sxs-lookup"><span data-stu-id="30a06-150">You can read and delete the message from the subscription by setting the `peek_lock` option to **false**.</span></span>

<span data-ttu-id="30a06-151">Výchozí chování umožňuje čtení a odstraňování dvoufázová operaci, která také umožňuje podpora aplikací, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="30a06-151">The default behavior makes the reading and deleting a two-stage operation, which also makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="30a06-152">Když Service Bus přijme požadavek, najde zprávu, která je na řadě ke spotřebování, uzamkne ji proti spotřebování jinými spotřebiteli a vrátí ji do aplikace.</span><span class="sxs-lookup"><span data-stu-id="30a06-152">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span> <span data-ttu-id="30a06-153">Když aplikace dokončí zpracování zprávy (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze přijetí volání `delete_subscription_message()` metoda a poskytující zprávu odstranit jako parametr.</span><span class="sxs-lookup"><span data-stu-id="30a06-153">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling `delete_subscription_message()` method and providing the message to be deleted as a parameter.</span></span> <span data-ttu-id="30a06-154">`delete_subscription_message()` Metoda bude označí zprávu jako spotřebovávanou a odebrat ji z odběru.</span><span class="sxs-lookup"><span data-stu-id="30a06-154">The `delete_subscription_message()` method will mark the message as being consumed and remove it from the subscription.</span></span>

<span data-ttu-id="30a06-155">Pokud `:peek_lock` parametr je nastaven na **false**, čtení a odstranění zprávy stane nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="30a06-155">If the `:peek_lock` parameter is set to **false**, reading and deleting the message becomes the simplest model, and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="30a06-156">Pro lepší vysvětlení si představte scénář, ve kterém spotřebitel vyšle požadavek na přijetí, ale než ji může zpracovat, dojde v něm k chybě a ukončí se.</span><span class="sxs-lookup"><span data-stu-id="30a06-156">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="30a06-157">Vzhledem k tomu, že Service Bus se už ale zprávu označila jako spotřebovávanou, pak když se aplikace restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily zprávu, která se spotřebovala před havárii.</span><span class="sxs-lookup"><span data-stu-id="30a06-157">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="30a06-158">Následující příklad ukazuje, jak můžete obdržet zprávy a zpracování pomocí `receive_subscription_message()`.</span><span class="sxs-lookup"><span data-stu-id="30a06-158">The following example demonstrates how messages can be received and processed using `receive_subscription_message()`.</span></span> <span data-ttu-id="30a06-159">V příkladu nejprve přijme a odstraní zprávu z `low-messages` předplatné pomocí `:peek_lock` nastavena na **false**, pak obdrží další zprávu z `high-messages` a poté se odstraní zprávu pomocí `delete_subscription_message()`:</span><span class="sxs-lookup"><span data-stu-id="30a06-159">The example first receives and deletes a message from the `low-messages` subscription by using `:peek_lock` set to **false**, then it receives another message from the `high-messages` and then deletes the message using `delete_subscription_message()`:</span></span>

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="30a06-160">Zpracování pádů aplikace a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="30a06-160">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="30a06-161">Service Bus poskytuje funkce, které vám pomůžou se elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="30a06-161">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="30a06-162">Pokud přijímající aplikace nedokáže zpracovat zprávu z nějakého důvodu, pak může zavolat `unlock_subscription_message()` metodu **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="30a06-162">If a receiver application is unable to process the message for some reason, then it can call the `unlock_subscription_message()` method on the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="30a06-163">To způsobí, že Service Bus zprávu odemkne v odběru a zpřístupní ji pro další přijetí, buďto stejnou spotřebitelskou aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="30a06-163">This causes Service Bus to unlock the message within the subscription and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="30a06-164">Je také vypršení časového limitu zpráva uzamčená v odběru, a pokud se nepodaří aplikace zprávu nezpracuje zámku vyprší časový limit (například pokud aplikace spadne), pak odemknutím Service Bus zprávu automaticky a zpřístupní ji pro přijetí.</span><span class="sxs-lookup"><span data-stu-id="30a06-164">There is also a timeout associated with a message locked within the subscription, and if the application fails to process the message before the lock timeout expires (for example, if the application crashes), then Service Bus will unlock the message automatically and make it available to be received again.</span></span>

<span data-ttu-id="30a06-165">V případě, že aplikace spadne po zpracování zprávy, ale předtím, než `delete_subscription_message()` metoda je volána, pak zprávy je víckrát do aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="30a06-165">In the event that the application crashes after processing the message but before the `delete_subscription_message()` method is called, then the message is redelivered to the application when it restarts.</span></span> <span data-ttu-id="30a06-166">To se často označuje jako *zpracování nejméně jednou*; to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích může doručit víckrát stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="30a06-166">This is often called *At Least Once Processing*; that is, each message will be processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="30a06-167">Pokud daný scénář nemůže tolerovat zpracování víc než jednou, vývojáři aplikace by měli přidat další logiku navíc pro zpracování víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="30a06-167">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="30a06-168">Tato logika se často opírá `message_id` vlastnosti zprávy, která zůstane konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="30a06-168">This logic is often achieved using the `message_id` property of the message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="30a06-169">Odstranění témat a odběrů</span><span class="sxs-lookup"><span data-stu-id="30a06-169">Delete topics and subscriptions</span></span>
<span data-ttu-id="30a06-170">Témata a odběry jsou trvalé a musí být explicitně odstranit buď prostřednictvím [portál Azure] [ Azure portal] nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="30a06-170">Topics and subscriptions are persistent, and must be explicitly deleted either through the [Azure portal][Azure portal] or programmatically.</span></span> <span data-ttu-id="30a06-171">Následující příklad ukazuje, jak odstranit téma s názvem `test-topic`.</span><span class="sxs-lookup"><span data-stu-id="30a06-171">The example below demonstrates how to delete the topic named `test-topic`.</span></span>

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

<span data-ttu-id="30a06-172">Pokud se odstraní téma, odstraní se i všechny odběry registrované k tomuto tématu.</span><span class="sxs-lookup"><span data-stu-id="30a06-172">Deleting a topic also deletes any subscriptions that are registered with the topic.</span></span> <span data-ttu-id="30a06-173">Odběry se taky dají odstranit samostatně.</span><span class="sxs-lookup"><span data-stu-id="30a06-173">Subscriptions can also be deleted independently.</span></span> <span data-ttu-id="30a06-174">Následující kód ukazuje, jak odstranit odběr s názvem `high-messages` z `test-topic` tématu:</span><span class="sxs-lookup"><span data-stu-id="30a06-174">The following code demonstrates how to delete the subscription named `high-messages` from the `test-topic` topic:</span></span>

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a><span data-ttu-id="30a06-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="30a06-175">Next steps</span></span>
<span data-ttu-id="30a06-176">Teď, když jste se naučili základy témat sběrnice Service Bus, postupujte podle následujících odkazech na další informace.</span><span class="sxs-lookup"><span data-stu-id="30a06-176">Now that you've learned the basics of Service Bus topics, follow these links to learn more.</span></span>

* <span data-ttu-id="30a06-177">V tématu [fronty, témata a odběry](service-bus-queues-topics-subscriptions.md).</span><span class="sxs-lookup"><span data-stu-id="30a06-177">See [Queues, topics, and subscriptions](service-bus-queues-topics-subscriptions.md).</span></span>
* <span data-ttu-id="30a06-178">Reference pro API pro [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).</span><span class="sxs-lookup"><span data-stu-id="30a06-178">API reference for [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).</span></span>
* <span data-ttu-id="30a06-179">Přejděte [Azure SDK pro Ruby](https://github.com/Azure/azure-sdk-for-ruby) úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="30a06-179">Visit the [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) repository on GitHub.</span></span>

[Azure portal]: https://portal.azure.com
