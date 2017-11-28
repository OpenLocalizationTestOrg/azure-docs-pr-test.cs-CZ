---
title: "témata Service Bus toouse aaaHow (Ruby) | Microsoft Docs"
description: "Zjistěte, jak toouse Service Bus témat a odběrů v Azure. Ukázky kódu jsou napsané pro poznámky Ruby aplikace."
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
ms.openlocfilehash: 236d6495825e68e336c23e1b500d0764ee512e49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-ruby"></a><span data-ttu-id="e6cad-104">Jak toouse Service Bus témata a odběry s Ruby</span><span class="sxs-lookup"><span data-stu-id="e6cad-104">How toouse Service Bus topics and subscriptions with Ruby</span></span>
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="e6cad-105">Tento článek popisuje, jak toouse Service Bus témata a odběry z Ruby aplikací.</span><span class="sxs-lookup"><span data-stu-id="e6cad-105">This article describes how toouse Service Bus topics and subscriptions from Ruby applications.</span></span> <span data-ttu-id="e6cad-106">Hello pokryté scénáře zahrnují **vytváření témat a odběrů, vytváření filtrů odběrů, odesílání zpráv** tooa tématu **přijímání zpráv z odběru**, a  **Odstranění témat a odběrů**.</span><span class="sxs-lookup"><span data-stu-id="e6cad-106">hello scenarios covered include **creating topics and subscriptions, creating subscription filters, sending messages** tooa topic, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span> <span data-ttu-id="e6cad-107">Další informace o témat a odběrů, najdete v části hello [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="e6cad-107">For more information on topics and subscriptions, see hello [Next Steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a><span data-ttu-id="e6cad-108">Vytvoření tématu</span><span class="sxs-lookup"><span data-stu-id="e6cad-108">Create a topic</span></span>
<span data-ttu-id="e6cad-109">Hello **Azure::ServiceBusService** objekt vám umožní toowork s tématy.</span><span class="sxs-lookup"><span data-stu-id="e6cad-109">hello **Azure::ServiceBusService** object enables you toowork with topics.</span></span> <span data-ttu-id="e6cad-110">Hello následující kód vytvoří **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="e6cad-110">hello following code creates an **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="e6cad-111">toocreate téma, použijte hello `create_topic()` metoda.</span><span class="sxs-lookup"><span data-stu-id="e6cad-111">toocreate a topic, use hello `create_topic()` method.</span></span> <span data-ttu-id="e6cad-112">Hello následující ukázka vytvoří téma nebo vytiskne hello chyby, pokud jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e6cad-112">hello following example creates a topic or prints out hello errors if there are any.</span></span>

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

<span data-ttu-id="e6cad-113">Můžete také předat **Azure::ServiceBus::Topic** objekt s další možnosti, které umožňují toooverride výchozí téma nastavení jako je například velikost fronty toolive nebo maximální doba zprávy.</span><span class="sxs-lookup"><span data-stu-id="e6cad-113">You can also pass an **Azure::ServiceBus::Topic** object with additional options, which enable you toooverride default topic settings such as message time toolive or maximum queue size.</span></span> <span data-ttu-id="e6cad-114">Hello následující příklad ukazuje nastavení fronty hello maximální velikosti too5 GB a času toolive too1 minutu:</span><span class="sxs-lookup"><span data-stu-id="e6cad-114">hello following example shows setting hello maximum queue size too5 GB and time toolive too1 minute:</span></span>

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a><span data-ttu-id="e6cad-115">Vytvořte odběr</span><span class="sxs-lookup"><span data-stu-id="e6cad-115">Create subscriptions</span></span>
<span data-ttu-id="e6cad-116">Odběry témat taky jsou vytvořeny pomocí hello **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="e6cad-116">Topic subscriptions are also created with hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="e6cad-117">Odběry mají názvy a můžou mít volitelné filtry, které omezuje hello sadu zpráv doručených virtuální fronty odběru toohello.</span><span class="sxs-lookup"><span data-stu-id="e6cad-117">Subscriptions are named and can have an optional filter that restricts hello set of messages delivered toohello subscription's virtual queue.</span></span>

<span data-ttu-id="e6cad-118">Odběry jsou trvalé a bude pokračovat tooexist, dokud nebudou buď jejich nebo hello téma jejich jsou přidruženy, budou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="e6cad-118">Subscriptions are persistent and will continue tooexist until either they, or hello topic they are associated with, are deleted.</span></span> <span data-ttu-id="e6cad-119">Pokud vaše aplikace obsahuje logiku toocreate předplatné, musí nejprve zkontrolujte, zda hello předplatné už existuje pomocí metody getSubscription hello.</span><span class="sxs-lookup"><span data-stu-id="e6cad-119">If your application contains logic toocreate a subscription, it should first check if hello subscription already exists by using hello getSubscription method.</span></span>

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a><span data-ttu-id="e6cad-120">Vytvoření odběru s filtrem (MatchAll) výchozí hello</span><span class="sxs-lookup"><span data-stu-id="e6cad-120">Create a subscription with hello default (MatchAll) filter</span></span>
<span data-ttu-id="e6cad-121">Hello **MatchAll** filtr je hello výchozí filtr, který se používá v případě, že při vytvoření nového předplatného je zadán žádný filtr.</span><span class="sxs-lookup"><span data-stu-id="e6cad-121">hello **MatchAll** filter is hello default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="e6cad-122">Když hello **MatchAll** filtr se používá, všechny zprávy publikované toohello tématu ukládány do virtuální fronty odběru hello.</span><span class="sxs-lookup"><span data-stu-id="e6cad-122">When hello **MatchAll** filter is used, all messages published toohello topic are placed in hello subscription's virtual queue.</span></span> <span data-ttu-id="e6cad-123">Hello následující příklad vytvoří odběr s názvem "všechna zprávy" a používá hello výchozí **MatchAll** filtru.</span><span class="sxs-lookup"><span data-stu-id="e6cad-123">hello following example creates a subscription named "all-messages" and uses hello default **MatchAll** filter.</span></span>

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="e6cad-124">Vytvoření odběru s filtry</span><span class="sxs-lookup"><span data-stu-id="e6cad-124">Create subscriptions with filters</span></span>
<span data-ttu-id="e6cad-125">Můžete také definovat filtry, které umožňují toospecify příjem zpráv odeslaných tooa tématu by měl zobrazit do konkrétní předplatné.</span><span class="sxs-lookup"><span data-stu-id="e6cad-125">You can also define filters that enable you toospecify which messages sent tooa topic should show up within a specific subscription.</span></span>

<span data-ttu-id="e6cad-126">Hello nejflexibilnější filtr, který odběry podporují je hello **Azure::ServiceBus::SqlFilter**, který implementuje je podmnožinou SQL92.</span><span class="sxs-lookup"><span data-stu-id="e6cad-126">hello most flexible type of filter supported by subscriptions is hello **Azure::ServiceBus::SqlFilter**, which implements a subset of SQL92.</span></span> <span data-ttu-id="e6cad-127">Filtry SQL pracují hello vlastnosti hello zpráv, které jsou publikované toohello tématu.</span><span class="sxs-lookup"><span data-stu-id="e6cad-127">SQL filters operate on hello properties of hello messages that are published toohello topic.</span></span> <span data-ttu-id="e6cad-128">Další podrobnosti o hello výrazy, které lze použít s filtrem SQL, projděte si hello [SqlFilter](service-bus-messaging-sql-filter.md) syntaxe.</span><span class="sxs-lookup"><span data-stu-id="e6cad-128">For more details about hello expressions that can be used with a SQL filter, review hello [SqlFilter](service-bus-messaging-sql-filter.md) syntax.</span></span>

<span data-ttu-id="e6cad-129">Filtry tooa odběru můžete přidat pomocí hello `create_rule()` metoda hello **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="e6cad-129">You can add filters tooa subscription by using hello `create_rule()` method of hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="e6cad-130">Tato metoda umožňuje tooadd nové filtry tooan existující předplatné.</span><span class="sxs-lookup"><span data-stu-id="e6cad-130">This method enables you tooadd new filters tooan existing subscription.</span></span>

<span data-ttu-id="e6cad-131">Vzhledem k tomu, že se automaticky použije výchozí filtr hello tooall nových předplatných, musíte nejprve odebrat hello výchozí filtr, nebo hello **MatchAll** přepíše ostatní filtry, můžete určit.</span><span class="sxs-lookup"><span data-stu-id="e6cad-131">Since hello default filter is applied automatically tooall new subscriptions, you must first remove hello default filter, or hello **MatchAll** will override any other filters you may specify.</span></span> <span data-ttu-id="e6cad-132">Hello výchozí pravidla můžete odebrat pomocí hello `delete_rule()` metodu hello **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="e6cad-132">You can remove hello default rule by using hello `delete_rule()` method on hello **Azure::ServiceBusService** object.</span></span>

<span data-ttu-id="e6cad-133">Hello následující příklad vytvoří odběr s názvem "vysoce zprávy" **Azure::ServiceBus::SqlFilter** který vybere jen zprávy, které mají vlastní `message_number` vlastnost větší než 3:</span><span class="sxs-lookup"><span data-stu-id="e6cad-133">hello following example creates a subscription named "high-messages" with an **Azure::ServiceBus::SqlFilter** that only selects messages that have a custom `message_number` property greater than 3:</span></span>

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

<span data-ttu-id="e6cad-134">Podobně hello následující příklad vytvoří odběr s názvem `low-messages` s **Azure::ServiceBus::SqlFilter** který vybere jen zprávy, které mají `message_number` vlastnost menší než nebo rovna too3:</span><span class="sxs-lookup"><span data-stu-id="e6cad-134">Similarly, hello following example creates a subscription named `low-messages` with an **Azure::ServiceBus::SqlFilter** that only selects messages that have a `message_number` property less than or equal too3:</span></span>

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

<span data-ttu-id="e6cad-135">Když je nyní odeslána zpráva příliš`test-topic`, je vždycky být toohello doručené tooreceivers odběru `all-messages` odběr tématu a odběru selektivně tooreceivers toohello `high-messages` a `low-messages` (odběry tématu v závislosti na obsahu zprávy hello).</span><span class="sxs-lookup"><span data-stu-id="e6cad-135">When a message is now sent too`test-topic`, it is always be delivered tooreceivers subscribed toohello `all-messages` topic subscription, and selectively delivered tooreceivers subscribed toohello `high-messages` and `low-messages` topic subscriptions (depending upon hello message content).</span></span>

## <a name="send-messages-tooa-topic"></a><span data-ttu-id="e6cad-136">Odeslání zprávy tooa tématu</span><span class="sxs-lookup"><span data-stu-id="e6cad-136">Send messages tooa topic</span></span>
<span data-ttu-id="e6cad-137">toosend tématu zpráva tooa Service Bus, vaše aplikace musí používat hello `send_topic_message()` metodu hello **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="e6cad-137">toosend a message tooa Service Bus topic, your application must use hello `send_topic_message()` method on hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="e6cad-138">Zprávy odeslané témata tooService Bus jsou instance třídy hello **Azure::ServiceBus::BrokeredMessage** objekty.</span><span class="sxs-lookup"><span data-stu-id="e6cad-138">Messages sent tooService Bus topics are instances of hello **Azure::ServiceBus::BrokeredMessage** objects.</span></span> <span data-ttu-id="e6cad-139">**Azure::ServiceBus::BrokeredMessage** objekty mají sadu standardních vlastností (jako například `label` a `time_to_live`), slovník, který je použité toohold vlastní vlastnosti specifické pro aplikace a tělo dat řetězce.</span><span class="sxs-lookup"><span data-stu-id="e6cad-139">**Azure::ServiceBus::BrokeredMessage** objects have a set of standard properties (such as `label` and `time_to_live`), a dictionary that is used toohold custom application-specific properties, and a body of string data.</span></span> <span data-ttu-id="e6cad-140">Aplikace může nastavit hello těla zprávy hello předáním toohello hodnotu řetězec `send_topic_message()` metoda a všechny nezbytné standardní vlastnosti vyplní výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e6cad-140">An application can set hello body of hello message by passing a string value toohello `send_topic_message()` method and any required standard properties will be populated by default values.</span></span>

<span data-ttu-id="e6cad-141">Hello následující příklad ukazuje, jak testovací toosend pět zprávy příliš`test-topic`.</span><span class="sxs-lookup"><span data-stu-id="e6cad-141">hello following example demonstrates how toosend five test messages too`test-topic`.</span></span> <span data-ttu-id="e6cad-142">Všimněte si, že hello `message_number` hodnotu vlastní vlastnosti každé zprávy se liší na hello iteraci smyčky hello (to určuje, jaké předplatné přijetí):</span><span class="sxs-lookup"><span data-stu-id="e6cad-142">Note that hello `message_number` custom property value of each message varies on hello iteration of hello loop (this determines which subscription receives it):</span></span>

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

<span data-ttu-id="e6cad-143">Témata Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="e6cad-143">Service Bus topics support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="e6cad-144">Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB.</span><span class="sxs-lookup"><span data-stu-id="e6cad-144">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="e6cad-145">Neexistuje žádné omezení na hello počet zpráv držených v tématu, ale není na hello celková velikost hello zpráv držených v tématu.</span><span class="sxs-lookup"><span data-stu-id="e6cad-145">There is no limit on hello number of messages held in a topic but there is a cap on hello total size of hello messages held by a topic.</span></span> <span data-ttu-id="e6cad-146">Velikost tématu se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="e6cad-146">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="e6cad-147">Příjem zpráv z odběru</span><span class="sxs-lookup"><span data-stu-id="e6cad-147">Receive messages from a subscription</span></span>
<span data-ttu-id="e6cad-148">Přijímání zpráv z odběru pomocí hello `receive_subscription_message()` metodu hello **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="e6cad-148">Messages are received from a subscription using hello `receive_subscription_message()` method on hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="e6cad-149">Ve výchozím nastavení zprávy jsou read(peak) a uzamčení bez odstranění z předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="e6cad-149">By default, messages are read(peak) and locked without deleting it from hello subscription.</span></span> <span data-ttu-id="e6cad-150">Můžete číst a odstraňte uvítací zprávu z předplatného hello nastavení hello `peek_lock` možnost příliš**false**.</span><span class="sxs-lookup"><span data-stu-id="e6cad-150">You can read and delete hello message from hello subscription by setting hello `peek_lock` option too**false**.</span></span>

<span data-ttu-id="e6cad-151">výchozí chování Hello díky hello čtení a odstraňování dvoufázová operace, takže také je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="e6cad-151">hello default behavior makes hello reading and deleting a two-stage operation, which also makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="e6cad-152">Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6cad-152">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="e6cad-153">Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces voláním `delete_subscription_message()` metoda a poskytování toobe hello zprávu odstranit jako parametr.</span><span class="sxs-lookup"><span data-stu-id="e6cad-153">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling `delete_subscription_message()` method and providing hello message toobe deleted as a parameter.</span></span> <span data-ttu-id="e6cad-154">Hello `delete_subscription_message()` metoda bude označit uvítací zprávu jako spotřebovávanou a jeho odebrání ze hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="e6cad-154">hello `delete_subscription_message()` method will mark hello message as being consumed and remove it from hello subscription.</span></span>

<span data-ttu-id="e6cad-155">Pokud hello `:peek_lock` parametr je nastaven příliš**false**, čtení a odstranění uvítací zprávu stane hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat není zpracování zprávy ve hello události došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="e6cad-155">If hello `:peek_lock` parameter is set too**false**, reading and deleting hello message becomes hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="e6cad-156">toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho.</span><span class="sxs-lookup"><span data-stu-id="e6cad-156">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="e6cad-157">Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.</span><span class="sxs-lookup"><span data-stu-id="e6cad-157">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="e6cad-158">Hello následující příklad ukazuje, jak můžete obdržet zprávy a zpracování pomocí `receive_subscription_message()`.</span><span class="sxs-lookup"><span data-stu-id="e6cad-158">hello following example demonstrates how messages can be received and processed using `receive_subscription_message()`.</span></span> <span data-ttu-id="e6cad-159">Příklad Hello nejprve přijme a odstraní zprávu z hello `low-messages` předplatné pomocí `:peek_lock` nastavit příliš**false**, pak obdrží další zprávu ze hello `high-messages` a poté odstraní hello zprávu pomocí `delete_subscription_message()`:</span><span class="sxs-lookup"><span data-stu-id="e6cad-159">hello example first receives and deletes a message from hello `low-messages` subscription by using `:peek_lock` set too**false**, then it receives another message from hello `high-messages` and then deletes hello message using `delete_subscription_message()`:</span></span>

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="e6cad-160">Jak toohandle aplikace spadne a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="e6cad-160">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="e6cad-161">Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="e6cad-161">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="e6cad-162">Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello `unlock_subscription_message()` metodu hello **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="e6cad-162">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlock_subscription_message()` method on hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="e6cad-163">Tato příčiny hello toounlock sběrnice zpráv v rámci předplatného hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="e6cad-163">This causes Service Bus toounlock hello message within hello subscription and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="e6cad-164">Je také přidružené zpráva uzamčená v rámci předplatného hello vypršení časového limitu, a pokud aplikace hello selže tooprocess uvítací zprávu před hello zámku vyprší časový limit (například pokud hello aplikace spadne), pak Service Bus odemknutím uvítací zprávu automaticky a nastavit jej jako dostupné toobe přijetí.</span><span class="sxs-lookup"><span data-stu-id="e6cad-164">There is also a timeout associated with a message locked within hello subscription, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="e6cad-165">V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello `delete_subscription_message()` metoda je volána, pak uvítací zprávu je víckrát toohello aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="e6cad-165">In hello event that hello application crashes after processing hello message but before hello `delete_subscription_message()` method is called, then hello message is redelivered toohello application when it restarts.</span></span> <span data-ttu-id="e6cad-166">To se často označuje jako *zpracování nejméně jednou*; to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích hello může doručit víckrát.</span><span class="sxs-lookup"><span data-stu-id="e6cad-166">This is often called *At Least Once Processing*; that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="e6cad-167">Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="e6cad-167">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="e6cad-168">Tato logika se často opírá hello `message_id` vlastnost hello zprávy, která zůstane konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="e6cad-168">This logic is often achieved using hello `message_id` property of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="e6cad-169">Odstranění témat a odběrů</span><span class="sxs-lookup"><span data-stu-id="e6cad-169">Delete topics and subscriptions</span></span>
<span data-ttu-id="e6cad-170">Témata a odběry jsou trvalé a musí být explicitně odstranit buď prostřednictvím hello [portál Azure] [ Azure portal] nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="e6cad-170">Topics and subscriptions are persistent, and must be explicitly deleted either through hello [Azure portal][Azure portal] or programmatically.</span></span> <span data-ttu-id="e6cad-171">Hello Příklad dole ukazuje, jak s názvem toodelete hello tématu `test-topic`.</span><span class="sxs-lookup"><span data-stu-id="e6cad-171">hello example below demonstrates how toodelete hello topic named `test-topic`.</span></span>

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

<span data-ttu-id="e6cad-172">Odstraní téma také odstraní všechny odběry, které jsou registrovány hello tématu.</span><span class="sxs-lookup"><span data-stu-id="e6cad-172">Deleting a topic also deletes any subscriptions that are registered with hello topic.</span></span> <span data-ttu-id="e6cad-173">Odběry se taky dají odstranit samostatně.</span><span class="sxs-lookup"><span data-stu-id="e6cad-173">Subscriptions can also be deleted independently.</span></span> <span data-ttu-id="e6cad-174">Hello následující kód ukazuje, jak s názvem odběru hello toodelete `high-messages` z hello `test-topic` tématu:</span><span class="sxs-lookup"><span data-stu-id="e6cad-174">hello following code demonstrates how toodelete hello subscription named `high-messages` from hello `test-topic` topic:</span></span>

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a><span data-ttu-id="e6cad-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e6cad-175">Next steps</span></span>
<span data-ttu-id="e6cad-176">Teď, když jste se naučili základy hello témat Service Bus, postupujte podle těchto odkazů toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="e6cad-176">Now that you've learned hello basics of Service Bus topics, follow these links toolearn more.</span></span>

* <span data-ttu-id="e6cad-177">V tématu [fronty, témata a odběry](service-bus-queues-topics-subscriptions.md).</span><span class="sxs-lookup"><span data-stu-id="e6cad-177">See [Queues, topics, and subscriptions](service-bus-queues-topics-subscriptions.md).</span></span>
* <span data-ttu-id="e6cad-178">Reference pro API pro [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).</span><span class="sxs-lookup"><span data-stu-id="e6cad-178">API reference for [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).</span></span>
* <span data-ttu-id="e6cad-179">Navštivte hello [Azure SDK pro Ruby](https://github.com/Azure/azure-sdk-for-ruby) úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="e6cad-179">Visit hello [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) repository on GitHub.</span></span>

[Azure portal]: https://portal.azure.com
