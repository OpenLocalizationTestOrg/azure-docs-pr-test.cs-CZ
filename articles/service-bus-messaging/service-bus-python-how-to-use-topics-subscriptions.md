---
title: "témata týkající se aaaHow toouse Azure Service Bus s Pythonem | Microsoft Docs"
description: "Zjistěte, jak toouse Azure Service Bus témata a odběry z Pythonu."
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
ms.openlocfilehash: 1171cbe8061bb3d73e2ce92ecc0cf45afae37054
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-python"></a><span data-ttu-id="d631c-103">Jak toouse Service Bus témata a odběry s Pythonem</span><span class="sxs-lookup"><span data-stu-id="d631c-103">How toouse Service Bus topics and subscriptions with Python</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="d631c-104">Tento článek popisuje, jak toouse Service Bus témat a odběrů.</span><span class="sxs-lookup"><span data-stu-id="d631c-104">This article describes how toouse Service Bus topics and subscriptions.</span></span> <span data-ttu-id="d631c-105">Hello ukázky jsou napsané v Pythonu a používají hello [balíček Azure Python SDK][Azure Python package].</span><span class="sxs-lookup"><span data-stu-id="d631c-105">hello samples are written in Python and use hello [Azure Python SDK package][Azure Python package].</span></span> <span data-ttu-id="d631c-106">Hello pokryté scénáře zahrnují **vytváření témat a odběrů**, **vytváření filtrů odběrů**, **odesílání zpráv tooa tématu**, **přijetí zprávy z odběru**, a **odstranění témat a odběrů**.</span><span class="sxs-lookup"><span data-stu-id="d631c-106">hello scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages tooa topic**, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span> <span data-ttu-id="d631c-107">Další informace o tématech a odběrech najdete v tématu hello [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="d631c-107">For more information about topics and subscriptions, see hello [Next Steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

> [!NOTE] 
> <span data-ttu-id="d631c-108">Pokud potřebujete tooinstall Python nebo hello [balíček Azure Python][Azure Python package], najdete v části hello [Průvodce instalací Python](../python-how-to-install.md).</span><span class="sxs-lookup"><span data-stu-id="d631c-108">If you need tooinstall Python or hello [Azure Python package][Azure Python package], see hello [Python Installation Guide](../python-how-to-install.md).</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="d631c-109">Vytvoření tématu</span><span class="sxs-lookup"><span data-stu-id="d631c-109">Create a topic</span></span>
<span data-ttu-id="d631c-110">Hello **ServiceBusService** objekt vám umožní toowork s tématy.</span><span class="sxs-lookup"><span data-stu-id="d631c-110">hello **ServiceBusService** object enables you toowork with topics.</span></span> <span data-ttu-id="d631c-111">Přidejte následující hello v horní hello jakéhokoliv Python souboru, ve kterém chcete tooprogrammatically přístup sběrnice:</span><span class="sxs-lookup"><span data-stu-id="d631c-111">Add hello following near hello top of any Python file in which you wish tooprogrammatically access Service Bus:</span></span>

```python
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

<span data-ttu-id="d631c-112">Hello následující kód vytvoří **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="d631c-112">hello following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="d631c-113">Nahraďte `mynamespace`, `sharedaccesskeyname`, a `sharedaccesskey` s vašeho oboru názvů skutečný název a klíč hodnotu klíče sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="d631c-113">Replace `mynamespace`, `sharedaccesskeyname`, and `sharedaccesskey` with your actual namespace, Shared Access Signature (SAS) key name, and key value.</span></span>

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

<span data-ttu-id="d631c-114">Hello hodnoty pro hello název klíče SAS a hodnotu můžete získat z hello [portál Azure][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="d631c-114">You can obtain hello values for hello SAS key name and value from hello [Azure portal][Azure portal].</span></span>

```python
bus_service.create_topic('mytopic')
```

<span data-ttu-id="d631c-115">Hello `create_topic` metoda také podporuje další možnosti, které umožňují toooverride výchozí téma nastavení jako je například velikost tématu toolive nebo maximální doba zprávy.</span><span class="sxs-lookup"><span data-stu-id="d631c-115">hello `create_topic` method also supports additional options, which enable you toooverride default topic settings such as message time toolive or maximum topic size.</span></span> <span data-ttu-id="d631c-116">Hello následující příklad ilustruje hello tématu maximální velikost too5 GB a hodnotu času toolive (TTL), 1 minuta:</span><span class="sxs-lookup"><span data-stu-id="d631c-116">hello following example sets hello maximum topic size too5 GB, and a time toolive (TTL) value of 1 minute:</span></span>

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a><span data-ttu-id="d631c-117">Vytvořte odběr</span><span class="sxs-lookup"><span data-stu-id="d631c-117">Create subscriptions</span></span>
<span data-ttu-id="d631c-118">Odběry tootopics jsou také vytvořené pomocí hello **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="d631c-118">Subscriptions tootopics are also created with hello **ServiceBusService** object.</span></span> <span data-ttu-id="d631c-119">Odběry mají názvy a můžou mít volitelné filtry, které omezuje hello sadu zpráv doručených virtuální fronty odběru toohello.</span><span class="sxs-lookup"><span data-stu-id="d631c-119">Subscriptions are named and can have an optional filter that restricts hello set of messages delivered toohello subscription's virtual queue.</span></span>

> [!NOTE]
> <span data-ttu-id="d631c-120">Odběry jsou trvalé a bude pokračovat tooexist, dokud nebudou buď jejich nebo hello tématu toowhich jejich přihlášeni, jsou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="d631c-120">Subscriptions are persistent and will continue tooexist until either they, or hello topic toowhich they are subscribed, are deleted.</span></span>
> 
> 

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a><span data-ttu-id="d631c-121">Vytvoření odběru s filtrem (MatchAll) výchozí hello</span><span class="sxs-lookup"><span data-stu-id="d631c-121">Create a subscription with hello default (MatchAll) filter</span></span>
<span data-ttu-id="d631c-122">Hello **MatchAll** filtr je hello výchozí filtr, který se používá v případě, že při vytvoření nového předplatného je zadán žádný filtr.</span><span class="sxs-lookup"><span data-stu-id="d631c-122">hello **MatchAll** filter is hello default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="d631c-123">Když hello **MatchAll** filtr se používá, všechny zprávy publikované toohello tématu ukládány do virtuální fronty odběru hello.</span><span class="sxs-lookup"><span data-stu-id="d631c-123">When hello **MatchAll** filter is used, all messages published toohello topic are placed in hello subscription's virtual queue.</span></span> <span data-ttu-id="d631c-124">Hello následující příklad vytvoří odběr s názvem `AllMessages` a používá hello výchozí **MatchAll** filtru.</span><span class="sxs-lookup"><span data-stu-id="d631c-124">hello following example creates a subscription named `AllMessages` and uses hello default **MatchAll** filter.</span></span>

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="d631c-125">Vytvoření odběru s filtry</span><span class="sxs-lookup"><span data-stu-id="d631c-125">Create subscriptions with filters</span></span>
<span data-ttu-id="d631c-126">Můžete také definovat filtry, které umožňují toospecify příjem zpráv odeslaných tooa tématu by měla objeví v konkrétním odběru tématu.</span><span class="sxs-lookup"><span data-stu-id="d631c-126">You can also define filters that enable you toospecify which messages sent tooa topic should show up within a specific topic subscription.</span></span>

<span data-ttu-id="d631c-127">je technologie Hello nejflexibilnější filtr, který odběry podporují **SqlFilter**, který implementuje je podmnožinou SQL92.</span><span class="sxs-lookup"><span data-stu-id="d631c-127">hello most flexible type of filter supported by subscriptions is a **SqlFilter**, which implements a subset of SQL92.</span></span> <span data-ttu-id="d631c-128">Filtry SQL pracují hello vlastnosti hello zpráv, které jsou publikované toohello tématu.</span><span class="sxs-lookup"><span data-stu-id="d631c-128">SQL filters operate on hello properties of hello messages that are published toohello topic.</span></span> <span data-ttu-id="d631c-129">Další informace o hello výrazy, které lze použít s filtrem SQL najdete v tématu hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntaxe.</span><span class="sxs-lookup"><span data-stu-id="d631c-129">For more information about hello expressions that can be used with a SQL filter, see hello [SqlFilter.SqlExpression][SqlFilter.SqlExpression] syntax.</span></span>

<span data-ttu-id="d631c-130">Filtry tooa odběru můžete přidat pomocí hello **vytvořit\_pravidlo** metoda hello **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="d631c-130">You can add filters tooa subscription by using hello **create\_rule** method of hello **ServiceBusService** object.</span></span> <span data-ttu-id="d631c-131">Tato metoda vám umožní tooadd nové filtry tooan existující předplatné.</span><span class="sxs-lookup"><span data-stu-id="d631c-131">This method allows you tooadd new filters tooan existing subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="d631c-132">Protože se automaticky použije výchozí filtr hello tooall nových předplatných, je nutné nejprve odebrat hello výchozí filtr nebo hello **MatchAll** přepíše ostatní filtry, můžete určit.</span><span class="sxs-lookup"><span data-stu-id="d631c-132">Because hello default filter is applied automatically tooall new subscriptions, you must first remove hello default filter or hello **MatchAll** will override any other filters you may specify.</span></span> <span data-ttu-id="d631c-133">Hello výchozí pravidla můžete odebrat pomocí hello `delete_rule` metoda hello **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="d631c-133">You can remove hello default rule by using hello `delete_rule` method of hello **ServiceBusService** object.</span></span>
> 
> 

<span data-ttu-id="d631c-134">Hello následující příklad vytvoří odběr s názvem `HighMessages` s **SqlFilter** který vybere jen zprávy, které mají vlastní `messagenumber` vlastnost větší než 3:</span><span class="sxs-lookup"><span data-stu-id="d631c-134">hello following example creates a subscription named `HighMessages` with a **SqlFilter** that only selects messages that have a custom `messagenumber` property greater than 3:</span></span>

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

<span data-ttu-id="d631c-135">Podobně hello následující příklad vytvoří odběr s názvem `LowMessages` s **SqlFilter** který vybere jen zprávy, které mají `messagenumber` vlastnost menší než nebo rovna too3:</span><span class="sxs-lookup"><span data-stu-id="d631c-135">Similarly, hello following example creates a subscription named `LowMessages` with a **SqlFilter** that only selects messages that have a `messagenumber` property less than or equal too3:</span></span>

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

<span data-ttu-id="d631c-136">Teď, když je odeslána zpráva příliš`mytopic` vždy se dodá tooreceivers odběru toohello **AllMessages** odběr tématu a odběru selektivně tooreceivers toohello **HighMessages**  a **LowMessages** odběry témat (v závislosti na obsahu zprávy hello).</span><span class="sxs-lookup"><span data-stu-id="d631c-136">Now, when a message is sent too`mytopic` it is always delivered tooreceivers subscribed toohello **AllMessages** topic subscription, and selectively delivered tooreceivers subscribed toohello **HighMessages** and **LowMessages** topic subscriptions (depending on hello message content).</span></span>

## <a name="send-messages-tooa-topic"></a><span data-ttu-id="d631c-137">Odeslání zprávy tooa tématu</span><span class="sxs-lookup"><span data-stu-id="d631c-137">Send messages tooa topic</span></span>
<span data-ttu-id="d631c-138">toosend tématu zpráva tooa Service Bus, vaše aplikace musí používat hello `send_topic_message` metoda hello **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="d631c-138">toosend a message tooa Service Bus topic, your application must use hello `send_topic_message` method of hello **ServiceBusService** object.</span></span>

<span data-ttu-id="d631c-139">Hello následující příklad ukazuje, jak testovací toosend pět zprávy příliš`mytopic`.</span><span class="sxs-lookup"><span data-stu-id="d631c-139">hello following example demonstrates how toosend five test messages too`mytopic`.</span></span> <span data-ttu-id="d631c-140">Všimněte si, že hello `messagenumber` hodnotu vlastnosti každé zprávy se liší na hello iteraci smyčky hello (to určuje, které odběry ji přijmou):</span><span class="sxs-lookup"><span data-stu-id="d631c-140">Note that hello `messagenumber` property value of each message varies on hello iteration of hello loop (this determines which subscriptions receive it):</span></span>

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

<span data-ttu-id="d631c-141">Témata Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="d631c-141">Service Bus topics support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="d631c-142">Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB.</span><span class="sxs-lookup"><span data-stu-id="d631c-142">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="d631c-143">Neexistuje žádné omezení na hello počet zpráv držených v tématu, ale není na hello celková velikost hello zpráv držených v tématu.</span><span class="sxs-lookup"><span data-stu-id="d631c-143">There is no limit on hello number of messages held in a topic but there is a cap on hello total size of hello messages held by a topic.</span></span> <span data-ttu-id="d631c-144">Velikost tématu se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="d631c-144">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span> <span data-ttu-id="d631c-145">Další informace o kvótách najdete v tématu [Service Bus kvóty][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="d631c-145">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="d631c-146">Příjem zpráv z odběru</span><span class="sxs-lookup"><span data-stu-id="d631c-146">Receive messages from a subscription</span></span>
<span data-ttu-id="d631c-147">Přijímání zpráv z odběru pomocí hello `receive_subscription_message` metodu hello **ServiceBusService** objektu:</span><span class="sxs-lookup"><span data-stu-id="d631c-147">Messages are received from a subscription using hello `receive_subscription_message` method on hello **ServiceBusService** object:</span></span>

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

<span data-ttu-id="d631c-148">Zprávy jsou odstraněny z předplatného hello jako jejich přečtení při hello parametr `peek_lock` je nastaven příliš**False**.</span><span class="sxs-lookup"><span data-stu-id="d631c-148">Messages are deleted from hello subscription as they are read when hello parameter `peek_lock` is set too**False**.</span></span> <span data-ttu-id="d631c-149">Můžete číst (funkce Náhled) a uzamčení uvítací zprávu bez odstranění z fronty hello parametrem hello nastavení `peek_lock` příliš**True**.</span><span class="sxs-lookup"><span data-stu-id="d631c-149">You can read (peek) and lock hello message without deleting it from hello queue by setting hello parameter `peek_lock` too**True**.</span></span>

<span data-ttu-id="d631c-150">Hello chování čtení a odstranění uvítací zprávu jako součást hello přijímat operaci je hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat hello události selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="d631c-150">hello behavior of reading and deleting hello message as part of hello receive operation is hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="d631c-151">toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho.</span><span class="sxs-lookup"><span data-stu-id="d631c-151">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="d631c-152">Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.</span><span class="sxs-lookup"><span data-stu-id="d631c-152">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="d631c-153">Pokud hello `peek_lock` parametr je nastaven příliš**True**, přijímat hello stane dvoufázového operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="d631c-153">If hello `peek_lock` parameter is set too**True**, hello receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="d631c-154">Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d631c-154">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="d631c-155">Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces voláním `delete` metodu hello **zpráva** objektu.</span><span class="sxs-lookup"><span data-stu-id="d631c-155">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling `delete` method on hello **Message** object.</span></span> <span data-ttu-id="d631c-156">Hello `delete` metoda označí uvítací zprávu jako spotřebovávanou a odstraní ji z odběru hello.</span><span class="sxs-lookup"><span data-stu-id="d631c-156">hello `delete` method marks hello message as being consumed and removes it from hello subscription.</span></span>

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="d631c-157">Jak toohandle aplikace spadne a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="d631c-157">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="d631c-158">Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="d631c-158">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="d631c-159">Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello `unlock` metodu hello **zpráva** objektu.</span><span class="sxs-lookup"><span data-stu-id="d631c-159">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlock` method on hello **Message** object.</span></span> <span data-ttu-id="d631c-160">To bude způsobit, že Service Bus toounlock uvítací zprávu v rámci předplatného hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="d631c-160">This will cause Service Bus toounlock hello message within hello subscription and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="d631c-161">Je také přidružené zpráva uzamčená v rámci předplatného hello vypršení časového limitu, a pokud aplikace hello selže tooprocess uvítací zprávu před hello zámku vyprší časový limit (například pokud hello aplikace spadne), pak uvítací zprávu odemkne Service Bus automaticky a je k dispozici toobe přijetí.</span><span class="sxs-lookup"><span data-stu-id="d631c-161">There is also a timeout associated with a message locked within hello subscription, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus unlocks hello message automatically and makes it available toobe received again.</span></span>

<span data-ttu-id="d631c-162">V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello `delete` metoda je volána, pak uvítací zprávu bude víckrát toohello aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="d631c-162">In hello event that hello application crashes after processing hello message but before hello `delete` method is called, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="d631c-163">To se často označuje jako *zpracování nejméně jednou*, který je každá zpráva se zpracuje alespoň jednou, ale v určitých situacích hello může doručit víckrát.</span><span class="sxs-lookup"><span data-stu-id="d631c-163">This is often called *At Least Once Processing*, that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="d631c-164">Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="d631c-164">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="d631c-165">To se často opírá hello **MessageId** vlastnost hello zprávy, která zůstane konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="d631c-165">This is often achieved using hello **MessageId** property of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="d631c-166">Odstranění témat a odběrů</span><span class="sxs-lookup"><span data-stu-id="d631c-166">Delete topics and subscriptions</span></span>
<span data-ttu-id="d631c-167">Témata a odběry jsou trvalé a musí být explicitně odstranit buď prostřednictvím hello [portál Azure] [ Azure portal] nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="d631c-167">Topics and subscriptions are persistent, and must be explicitly deleted either through hello [Azure portal][Azure portal] or programmatically.</span></span> <span data-ttu-id="d631c-168">Hello následující příklad ukazuje, jak s názvem toodelete hello tématu `mytopic`:</span><span class="sxs-lookup"><span data-stu-id="d631c-168">hello following example shows how toodelete hello topic named `mytopic`:</span></span>

```python
bus_service.delete_topic('mytopic')
```

<span data-ttu-id="d631c-169">Odstraní téma také odstraní všechny odběry, které jsou registrovány hello tématu.</span><span class="sxs-lookup"><span data-stu-id="d631c-169">Deleting a topic also deletes any subscriptions that are registered with hello topic.</span></span> <span data-ttu-id="d631c-170">Odběry se taky dají odstranit samostatně.</span><span class="sxs-lookup"><span data-stu-id="d631c-170">Subscriptions can also be deleted independently.</span></span> <span data-ttu-id="d631c-171">Hello následující kód ukazuje, jak toodelete předplatné s názvem `HighMessages` z hello `mytopic` tématu:</span><span class="sxs-lookup"><span data-stu-id="d631c-171">hello following code shows how toodelete a subscription named `HighMessages` from hello `mytopic` topic:</span></span>

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a><span data-ttu-id="d631c-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d631c-172">Next steps</span></span>
<span data-ttu-id="d631c-173">Teď, když jste se naučili základy hello témat Service Bus, postupujte podle těchto odkazů toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="d631c-173">Now that you've learned hello basics of Service Bus topics, follow these links toolearn more.</span></span>

* <span data-ttu-id="d631c-174">V tématu [fronty, témata a odběry][Queues, topics, and subscriptions].</span><span class="sxs-lookup"><span data-stu-id="d631c-174">See [Queues, topics, and subscriptions][Queues, topics, and subscriptions].</span></span>
* <span data-ttu-id="d631c-175">Reference pro [SqlFilter.SqlExpression][SqlFilter.SqlExpression].</span><span class="sxs-lookup"><span data-stu-id="d631c-175">Reference for [SqlFilter.SqlExpression][SqlFilter.SqlExpression].</span></span>

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
