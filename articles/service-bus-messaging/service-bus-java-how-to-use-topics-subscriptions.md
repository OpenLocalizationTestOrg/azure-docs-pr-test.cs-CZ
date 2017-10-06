---
title: "témata týkající se aaaHow toouse Azure Service Bus s Javou | Microsoft Docs"
description: "Použijte témata a odběry Service Bus v Azure."
services: service-bus-messaging
documentationcenter: java
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 63d6c8bd-8a22-4292-befc-545ffb52e8eb
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 06/28/2017
ms.author: sethm
ms.openlocfilehash: 1aad16fdb5d68a5782b85c8dfda9d695babd57ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-java"></a><span data-ttu-id="2c439-103">Jak toouse Service Bus témata a odběry s Javou</span><span class="sxs-lookup"><span data-stu-id="2c439-103">How toouse Service Bus topics and subscriptions with Java</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="2c439-104">Tato příručka popisuje, jak toouse Service Bus témat a odběrů.</span><span class="sxs-lookup"><span data-stu-id="2c439-104">This guide describes how toouse Service Bus topics and subscriptions.</span></span> <span data-ttu-id="2c439-105">Hello ukázky jsou napsané v jazyce Java a používají hello [Azure SDK pro jazyk Java][Azure SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="2c439-105">hello samples are written in Java and use hello [Azure SDK for Java][Azure SDK for Java].</span></span> <span data-ttu-id="2c439-106">Hello pokryté scénáře zahrnují **vytváření témat a odběrů**, **vytváření filtrů odběrů**, **odesílání zpráv tooa tématu**, **přijetí zprávy z odběru**, a **odstranění témat a odběrů**.</span><span class="sxs-lookup"><span data-stu-id="2c439-106">hello scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages tooa topic**, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span>

## <a name="what-are-service-bus-topics-and-subscriptions"></a><span data-ttu-id="2c439-107">Co jsou témata a předplatné služby Service Bus?</span><span class="sxs-lookup"><span data-stu-id="2c439-107">What are Service Bus topics and subscriptions?</span></span>
<span data-ttu-id="2c439-108">Témata a předplatné služby Service Bus podporují komunikační model zasílání zpráv *publikování/přihlášení*.</span><span class="sxs-lookup"><span data-stu-id="2c439-108">Service Bus topics and subscriptions support a *publish/subscribe* messaging communication model.</span></span> <span data-ttu-id="2c439-109">Součásti distribuované aplikace při používání témat a předplatných nekomunikují navzájem přímo. Místo toho si zprávy vyměňují prostřednictvím tématu, které slouží jako zprostředkovatel.</span><span class="sxs-lookup"><span data-stu-id="2c439-109">When using topics and subscriptions, components of a distributed application do not communicate directly with each other; instead they exchange messages via a topic, which acts as an intermediary.</span></span>

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

<span data-ttu-id="2c439-111">Na rozdíl od front služby Service Bus, ve kterých každou zprávu zpracuje jeden spotřebitel, témata a předplatné nabízejí komunikaci v podobě „1:N“ a používají vzor publikování/přihlášení.</span><span class="sxs-lookup"><span data-stu-id="2c439-111">In contrast with Service Bus queues, in which each message is processed by a single consumer, topics and subscriptions provide a "one-to-many" form of communication, using a publish/subscribe pattern.</span></span> <span data-ttu-id="2c439-112">Je možné zaregistrovat několik předplatných tooa tématu.</span><span class="sxs-lookup"><span data-stu-id="2c439-112">It is possible to register multiple subscriptions tooa topic.</span></span> <span data-ttu-id="2c439-113">Při odeslání zprávy tooa tématu, je pak k proces toohandle předplatného dostupná tooeach nezávisle.</span><span class="sxs-lookup"><span data-stu-id="2c439-113">When a message is sent tooa topic, it is then made available tooeach subscription toohandle/process independently.</span></span>

<span data-ttu-id="2c439-114">Téma tooa předplatného se podobá virtuální frontě, která obdrží kopii zprávy hello, které byly odeslány toohello tématu.</span><span class="sxs-lookup"><span data-stu-id="2c439-114">A subscription tooa topic resembles a virtual queue that receives copies of hello messages that were sent toohello topic.</span></span> <span data-ttu-id="2c439-115">Volitelně můžete zaregistrovat pravidla filtru pro téma na základě za předplatné, což vám umožní toofilter nebo omezit přijme které zprávy tooa téma podle předplatných tématu.</span><span class="sxs-lookup"><span data-stu-id="2c439-115">You can optionally register filter rules for a topic on a per-subscription basis, which allows you toofilter/restrict which messages tooa topic are received by which topic subscriptions.</span></span>

<span data-ttu-id="2c439-116">Témata a odběry Service Bus umožňují tooscale tooprocess velký počet zpráv mezi velký počet uživatelů a aplikací.</span><span class="sxs-lookup"><span data-stu-id="2c439-116">Service Bus topics and subscriptions enable you tooscale tooprocess a very large number of messages across a very large number of users and applications.</span></span>

## <a name="create-a-service-namespace"></a><span data-ttu-id="2c439-117">Vytvoření oboru názvů služby</span><span class="sxs-lookup"><span data-stu-id="2c439-117">Create a service namespace</span></span>
<span data-ttu-id="2c439-118">toobegin používání témat a odběrů Service Bus v Azure, musíte nejdřív vytvořit obor názvů, který poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c439-118">toobegin using Service Bus topics and subscriptions in Azure, you must first create a namespace, which provides a scoping container for addressing Service Bus resources within your application.</span></span>

<span data-ttu-id="2c439-119">toocreate obor názvů:</span><span class="sxs-lookup"><span data-stu-id="2c439-119">toocreate a namespace:</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="2c439-120">Konfigurace vaší aplikace toouse Service Bus</span><span class="sxs-lookup"><span data-stu-id="2c439-120">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="2c439-121">Ujistěte se, že máte nainstalovanou hello [Azure SDK pro jazyk Java] [ Azure SDK for Java] před vytvořením této ukázce.</span><span class="sxs-lookup"><span data-stu-id="2c439-121">Make sure you have installed hello [Azure SDK for Java][Azure SDK for Java] before building this sample.</span></span> <span data-ttu-id="2c439-122">Pokud používáte Eclipse, můžete nainstalovat hello [nástrojů Azure pro Eclipse] [ Azure Toolkit for Eclipse] zahrnující hello Azure SDK pro jazyk Java.</span><span class="sxs-lookup"><span data-stu-id="2c439-122">If you are using Eclipse, you can install hello [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse] that includes hello Azure SDK for Java.</span></span> <span data-ttu-id="2c439-123">Poté můžete přidat hello **knihovny Microsoft Azure Libraries for Java** tooyour projektu:</span><span class="sxs-lookup"><span data-stu-id="2c439-123">You can then add hello **Microsoft Azure Libraries for Java** tooyour project:</span></span>

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

<span data-ttu-id="2c439-124">Přidejte následující hello `import` toohello příkazy na začátek souboru Java hello:</span><span class="sxs-lookup"><span data-stu-id="2c439-124">Add hello following `import` statements toohello top of hello Java file:</span></span>

```java
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

<span data-ttu-id="2c439-125">Přidejte hello knihovny Azure Libraries for Java tooyour cesta sestavení a její zahrnutí do vašeho projektu nasazení sestavení.</span><span class="sxs-lookup"><span data-stu-id="2c439-125">Add hello Azure Libraries for Java tooyour build path and include it in your project deployment assembly.</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="2c439-126">Vytvoření tématu</span><span class="sxs-lookup"><span data-stu-id="2c439-126">Create a topic</span></span>
<span data-ttu-id="2c439-127">Operace správy témat sběrnice Service Bus je možné provádět prostřednictvím **ServiceBusContract** třídy.</span><span class="sxs-lookup"><span data-stu-id="2c439-127">Management operations for Service Bus topics can be performed via the **ServiceBusContract** class.</span></span> <span data-ttu-id="2c439-128">A **ServiceBusContract** objektu je vytvořený pomocí odpovídající konfiguraci, který zapouzdřuje token SAS s oprávněními toomanage a hello **ServiceBusContract** třída je jediný bod hello komunikace s Azure.</span><span class="sxs-lookup"><span data-stu-id="2c439-128">A **ServiceBusContract** object is constructed with an appropriate configuration that encapsulates the SAS token with permissions toomanage it, and hello **ServiceBusContract** class is hello sole point of communication with Azure.</span></span>

<span data-ttu-id="2c439-129">Hello **ServiceBusService** třída poskytuje metody toocreate, výčet a odstranění témat.</span><span class="sxs-lookup"><span data-stu-id="2c439-129">hello **ServiceBusService** class provides methods toocreate, enumerate, and delete topics.</span></span> <span data-ttu-id="2c439-130">Následující příklad ukazuje, jak Hello **ServiceBusService** objekt může být použité toocreate téma s názvem `TestTopic`, k oboru názvů s názvem `HowToSample`:</span><span class="sxs-lookup"><span data-stu-id="2c439-130">hello following example shows how a **ServiceBusService** object can be used toocreate a topic named `TestTopic`, with a namespace called `HowToSample`:</span></span>

```java
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
      "HowToSample",
      "RootManageSharedAccessKey",
      "SAS_key_value",
      ".servicebus.windows.net"
      );

ServiceBusContract service = ServiceBusService.create(config);
TopicInfo topicInfo = new TopicInfo("TestTopic");
try  
{
    CreateTopicResult result = service.createTopic(topicInfo);
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

<span data-ttu-id="2c439-131">Způsoby na **TopicInfo** , povolte vlastnosti tématu hello nastavení (například: tooset hello výchozí time to live (TTL) hodnotu toobe použít toomessages odeslané toohello tématu).</span><span class="sxs-lookup"><span data-stu-id="2c439-131">There are methods on **TopicInfo** that enable properties of hello topic to be set (for example: tooset hello default time-to-live (TTL) value toobe applied toomessages sent toohello topic).</span></span> <span data-ttu-id="2c439-132">Hello následující příklad ukazuje, jak toocreate téma s názvem `TestTopic` s maximální velikostí 5 GB:</span><span class="sxs-lookup"><span data-stu-id="2c439-132">hello following example shows how toocreate a topic named `TestTopic` with a maximum size of 5 GB:</span></span>

```java
long maxSizeInMegabytes = 5120;  
TopicInfo topicInfo = new TopicInfo("TestTopic");  
topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateTopicResult result = service.createTopic(topicInfo);
```

<span data-ttu-id="2c439-133">Všimněte si, které můžete použít hello **listTopics** metodu **ServiceBusContract** objekty toocheck Pokud téma se zadaným názvem již existuje v rámci oboru názvů služby.</span><span class="sxs-lookup"><span data-stu-id="2c439-133">Note that you can use hello **listTopics** method on **ServiceBusContract** objects toocheck if a topic with a specified name already exists within a service namespace.</span></span>

## <a name="create-subscriptions"></a><span data-ttu-id="2c439-134">Vytvořte odběr</span><span class="sxs-lookup"><span data-stu-id="2c439-134">Create subscriptions</span></span>
<span data-ttu-id="2c439-135">Odběry tootopics jsou také vytvořené pomocí hello **ServiceBusService** třídy.</span><span class="sxs-lookup"><span data-stu-id="2c439-135">Subscriptions tootopics are also created with hello **ServiceBusService** class.</span></span> <span data-ttu-id="2c439-136">Odběry mají názvy a můžou mít volitelné filtry, které omezují skupinu zpráv předávaných virtuální fronty odběru toohello hello.</span><span class="sxs-lookup"><span data-stu-id="2c439-136">Subscriptions are named and can have an optional filter that restricts hello set of messages passed toohello subscription's virtual queue.</span></span>

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a><span data-ttu-id="2c439-137">Vytvoření odběru s filtrem (MatchAll) výchozí hello</span><span class="sxs-lookup"><span data-stu-id="2c439-137">Create a subscription with hello default (MatchAll) filter</span></span>
<span data-ttu-id="2c439-138">Hello **MatchAll** filtr je hello výchozí filtr, který se používá v případě, že při vytvoření nového předplatného je zadán žádný filtr.</span><span class="sxs-lookup"><span data-stu-id="2c439-138">hello **MatchAll** filter is hello default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="2c439-139">Když hello **MatchAll** filtr se používá, všechny zprávy publikované toohello tématu ukládány do virtuální fronty odběru.</span><span class="sxs-lookup"><span data-stu-id="2c439-139">When hello **MatchAll** filter is used, all messages published toohello topic are placed in the subscription's virtual queue.</span></span> <span data-ttu-id="2c439-140">Hello následující příklad vytvoří odběr s názvem "AllMessages" a používá hello výchozí **MatchAll** filtru.</span><span class="sxs-lookup"><span data-stu-id="2c439-140">hello following example creates a subscription named "AllMessages" and uses hello default **MatchAll** filter.</span></span>

```java
SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
CreateSubscriptionResult result =
    service.createSubscription("TestTopic", subInfo);
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="2c439-141">Vytvoření odběru s filtry</span><span class="sxs-lookup"><span data-stu-id="2c439-141">Create subscriptions with filters</span></span>
<span data-ttu-id="2c439-142">Můžete také vytvořit filtry, které umožňují tooscope příjem zpráv odeslaných tooa tématu by měla objeví v konkrétním odběru tématu.</span><span class="sxs-lookup"><span data-stu-id="2c439-142">You can also create filters that enable you tooscope which messages sent tooa topic should show up within a specific topic subscription.</span></span>

<span data-ttu-id="2c439-143">je technologie Hello nejflexibilnější filtr, který odběry podporují [SqlFilter][SqlFilter], který implementuje je podmnožinou SQL92.</span><span class="sxs-lookup"><span data-stu-id="2c439-143">hello most flexible type of filter supported by subscriptions is the [SqlFilter][SqlFilter], which implements a subset of SQL92.</span></span> <span data-ttu-id="2c439-144">Filtry SQL pracují hello vlastnosti hello zpráv, které jsou publikované toohello tématu.</span><span class="sxs-lookup"><span data-stu-id="2c439-144">SQL filters operate on hello properties of hello messages that are published toohello topic.</span></span> <span data-ttu-id="2c439-145">Další podrobnosti o hello výrazy, které lze použít s filtrem SQL, projděte si hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntaxe.</span><span class="sxs-lookup"><span data-stu-id="2c439-145">For more details about hello expressions that can be used with a SQL filter, review hello [SqlFilter.SqlExpression][SqlFilter.SqlExpression] syntax.</span></span>

<span data-ttu-id="2c439-146">Hello následující příklad vytvoří odběr s názvem `HighMessages` s [SqlFilter] [ SqlFilter] objekt, který vybere jen zprávy, které mají vlastní **MessageNumber** Vlastnost větší než 3:</span><span class="sxs-lookup"><span data-stu-id="2c439-146">hello following example creates a subscription named `HighMessages` with a [SqlFilter][SqlFilter] object that only selects messages that have a custom **MessageNumber** property greater than 3:</span></span>

```java
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete hello default rule, otherwise hello new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

<span data-ttu-id="2c439-147">Podobně hello následující příklad vytvoří odběr s názvem `LowMessages` s [SqlFilter] [ SqlFilter] objekt, který vybere jen zprávy, které mají **MessageNumber** Vlastnost menší než nebo rovna too3:</span><span class="sxs-lookup"><span data-stu-id="2c439-147">Similarly, hello following example creates a subscription named `LowMessages` with a [SqlFilter][SqlFilter] object that only selects messages that have a **MessageNumber** property less than or equal too3:</span></span>

```java
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete hello default rule, otherwise hello new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

<span data-ttu-id="2c439-148">Když je nyní odeslána zpráva příliš`TestTopic`, bude vždy doručena tooreceivers odběru toohello `AllMessages` předplatného a selektivně tooreceivers odběru toohello `HighMessages` a `LowMessages` odběry ( v závislosti na obsahu zprávy).</span><span class="sxs-lookup"><span data-stu-id="2c439-148">When a message is now sent too`TestTopic`, it will always be delivered tooreceivers subscribed toohello `AllMessages` subscription, and selectively delivered tooreceivers subscribed toohello `HighMessages` and `LowMessages` subscriptions (depending upon the message content).</span></span>

## <a name="send-messages-tooa-topic"></a><span data-ttu-id="2c439-149">Odeslání zprávy tooa tématu</span><span class="sxs-lookup"><span data-stu-id="2c439-149">Send messages tooa topic</span></span>
<span data-ttu-id="2c439-150">Získá toosend tématu Service Bus zprávu tooa, aplikace **ServiceBusContract** objektu.</span><span class="sxs-lookup"><span data-stu-id="2c439-150">toosend a message tooa Service Bus topic, your application obtains a **ServiceBusContract** object.</span></span> <span data-ttu-id="2c439-151">Hello následující kód ukazuje, jak toosend zprávu o hello `TestTopic` tématu předtím vytvořili v rámci hello `HowToSample` obor názvů:</span><span class="sxs-lookup"><span data-stu-id="2c439-151">hello following code demonstrates how toosend a message for hello `TestTopic` topic created previously within hello `HowToSample` namespace:</span></span>

```java
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

<span data-ttu-id="2c439-152">Zprávy odeslané témata tooService Bus jsou instance [BrokeredMessage] [ BrokeredMessage] třídy.</span><span class="sxs-lookup"><span data-stu-id="2c439-152">Messages sent tooService Bus Topics are instances of the [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="2c439-153">[BrokeredMessage][BrokeredMessage]* objekty mají sadu standardních metod (například **setLabel** a **TimeToLive**), slovník, který je použité toohold vlastní vlastnosti specifické pro aplikace a tělo s libovolnými aplikačními daty.</span><span class="sxs-lookup"><span data-stu-id="2c439-153">[BrokeredMessage][BrokeredMessage]* objects have a set of standard methods (such as **setLabel** and **TimeToLive**), a dictionary that is used toohold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="2c439-154">Aplikace můžete hello tělo zprávy nastavit tak předá jakýkoli serializovatelný objekt do konstruktoru hello [BrokeredMessage][BrokeredMessage]a odpovídající hello **DataContractSerializer**  bude použité tooserialize hello objektu.</span><span class="sxs-lookup"><span data-stu-id="2c439-154">An application can set hello body of the message by passing any serializable object into hello constructor of the [BrokeredMessage][BrokeredMessage], and hello appropriate **DataContractSerializer** will then be used tooserialize hello object.</span></span> <span data-ttu-id="2c439-155">Alternativně **java.io.InputStream** lze zadat.</span><span class="sxs-lookup"><span data-stu-id="2c439-155">Alternatively, a **java.io.InputStream** can be provided.</span></span>

<span data-ttu-id="2c439-156">Hello následující příklad ukazuje, jak testovací toosend pět zprávy toothe `TestTopic` **MessageSender** jsme získali ve fragmentu kódu hello výše.</span><span class="sxs-lookup"><span data-stu-id="2c439-156">hello following example demonstrates how toosend five test messages toothe `TestTopic` **MessageSender** we obtained in hello code snippet above.</span></span>
<span data-ttu-id="2c439-157">Poznámka: jak hello **MessageNumber** hodnotu vlastnosti každé zprávy se liší na hello iteraci smyčky hello (to určí, které odběry ji přijmou):</span><span class="sxs-lookup"><span data-stu-id="2c439-157">Note how hello **MessageNumber** property value of each message varies on hello iteration of hello loop (this will determine which subscriptions receive it):</span></span>

```java
for (int i=0; i<5; i++)  {
// Create message, passing a string message for hello body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message toohello topic
service.sendTopicMessage("TestTopic", message);
}
```

<span data-ttu-id="2c439-158">Témata Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="2c439-158">Service Bus topics support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="2c439-159">Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB.</span><span class="sxs-lookup"><span data-stu-id="2c439-159">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="2c439-160">Neexistuje žádné omezení na hello počet zpráv držených v tématu, ale je omezena na hello celková velikost hello zpráv držených v tématu.</span><span class="sxs-lookup"><span data-stu-id="2c439-160">There is no limit on hello number of messages held in a topic but there is a limit on hello total size of hello messages held by a topic.</span></span> <span data-ttu-id="2c439-161">Velikost tématu se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="2c439-161">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="how-tooreceive-messages-from-a-subscription"></a><span data-ttu-id="2c439-162">Jak tooreceive zprávy z odběru</span><span class="sxs-lookup"><span data-stu-id="2c439-162">How tooreceive messages from a subscription</span></span>
<span data-ttu-id="2c439-163">použít tooreceive zprávy z odběru, **ServiceBusContract** objektu.</span><span class="sxs-lookup"><span data-stu-id="2c439-163">tooreceive messages from a subscription, use a **ServiceBusContract** object.</span></span> <span data-ttu-id="2c439-164">Přijaté zprávy můžou pracovat ve dvou různých režimech: **ReceiveAndDelete** a **PeekLock**.</span><span class="sxs-lookup"><span data-stu-id="2c439-164">Received messages can work in two different modes: **ReceiveAndDelete** and **PeekLock**.</span></span>

<span data-ttu-id="2c439-165">Při použití hello **ReceiveAndDelete** režimu přijímat je jednorázová operace – to znamená, když Service Bus přijme požadavek čtení zprávy, označí uvítací zprávu jako spotřebovávanou a vrátí ji toothe aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c439-165">When using hello **ReceiveAndDelete** mode, receive is a single-shot operation - that is, when Service Bus receives a read request for a message, it marks hello message as being consumed and returns it toothe application.</span></span> <span data-ttu-id="2c439-166">**ReceiveAndDelete** režimu je hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat hello události selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="2c439-166">**ReceiveAndDelete** mode is hello simplest model and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="2c439-167">toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho.</span><span class="sxs-lookup"><span data-stu-id="2c439-167">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="2c439-168">Vzhledem k tomu, že Service Bus se už ale zprávu označila jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.</span><span class="sxs-lookup"><span data-stu-id="2c439-168">Because Service Bus will have marked the message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="2c439-169">V **PeekLock** režimu, zobrazí se změní na dvě fáze operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="2c439-169">In **PeekLock** mode, receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="2c439-170">Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c439-170">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="2c439-171">Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces voláním **odstranit** na uvítací přijal zprávu.</span><span class="sxs-lookup"><span data-stu-id="2c439-171">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling **Delete** on hello received message.</span></span> <span data-ttu-id="2c439-172">Když Service Bus uvidí hello **odstranit** volání, která se bude označit uvítací zprávu jako spotřebovávanou a jeho odebrání z tématu hello.</span><span class="sxs-lookup"><span data-stu-id="2c439-172">When Service Bus sees hello **Delete** call, it will mark hello message as being consumed and remove it from hello topic.</span></span>

<span data-ttu-id="2c439-173">Hello následující příklad ukazuje, jak můžete obdržet zprávy a zpracování pomocí **PeekLock** režimu (ne hello výchozí režim).</span><span class="sxs-lookup"><span data-stu-id="2c439-173">hello example below demonstrates how messages can be received and processed using **PeekLock** mode (not hello default mode).</span></span> <span data-ttu-id="2c439-174">Hello příklad provede smyčku a zpracovává zprávy v hello "HighMessages" předplatného a pak ukončí, pokud nejsou žádné další zprávy (případně může být nastavena toowait pro nové zprávy).</span><span class="sxs-lookup"><span data-stu-id="2c439-174">hello example below performs a loop and processes messages in hello "HighMessages" subscription and then exits when there are no more messages (alternatively, it could be set toowait for new messages).</span></span>

```java
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display hello topic message.
            System.out.print("From topic: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added toohandle no more messages.
            // Could instead wait for more messages toobe added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="2c439-175">Jak toohandle aplikace spadne a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="2c439-175">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="2c439-176">Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="2c439-176">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="2c439-177">Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello **unlockMessage** na hello přijal zprávu (místo hello **deleteMessage** metoda).</span><span class="sxs-lookup"><span data-stu-id="2c439-177">If a receiver application is unable tooprocess hello message for some reason, then it can call hello **unlockMessage** method on hello received message (instead of hello **deleteMessage** method).</span></span> <span data-ttu-id="2c439-178">To bude způsobit, že Service Bus toounlock uvítací zprávu v rámci hello tématu a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="2c439-178">This will cause Service Bus toounlock hello message within hello topic and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="2c439-179">Je také vypršení časového limitu přidružené zpráva uzamčená v tomto tématu, a pokud aplikace hello selže tooprocess uvítací zprávu před zámku vyprší časový limit (například pokud hello aplikace spadne), pak Service Bus se automatické odemknutí uvítací zprávu a nastavte jej k dispozici toobe přijetí.</span><span class="sxs-lookup"><span data-stu-id="2c439-179">There is also a timeout associated with a message locked within the topic, and if hello application fails tooprocess hello message before the lock timeout expires (for example, if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="2c439-180">V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello **deleteMessage** požadavku a potom uvítací zprávu bude víckrát toohello aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="2c439-180">In hello event that hello application crashes after processing hello message but before hello **deleteMessage** request is issued, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="2c439-181">To se často označuje jako **zpracování nejméně jednou**, který je každá zpráva se zpracuje alespoň jednou, ale v určitých situacích hello může doručit víckrát.</span><span class="sxs-lookup"><span data-stu-id="2c439-181">This is often called **At Least Once Processing**, that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="2c439-182">Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="2c439-182">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="2c439-183">To se často opírá hello **getMessageId** metoda hello zprávy, která zůstane konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="2c439-183">This is often achieved using hello **getMessageId** method of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="2c439-184">Odstranění témat a odběrů</span><span class="sxs-lookup"><span data-stu-id="2c439-184">Delete topics and subscriptions</span></span>
<span data-ttu-id="2c439-185">Hello primární způsob toodelete témata a odběry je toouse **ServiceBusContract** objektu.</span><span class="sxs-lookup"><span data-stu-id="2c439-185">hello primary way toodelete topics and subscriptions is toouse a **ServiceBusContract** object.</span></span> <span data-ttu-id="2c439-186">Odstraní téma také odstraní všechny odběry, které jsou registrovány hello tématu.</span><span class="sxs-lookup"><span data-stu-id="2c439-186">Deleting a topic will also delete any subscriptions that are registered with hello topic.</span></span> <span data-ttu-id="2c439-187">Odběry se taky dají odstranit samostatně.</span><span class="sxs-lookup"><span data-stu-id="2c439-187">Subscriptions can also be deleted independently.</span></span>

```java
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a><span data-ttu-id="2c439-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c439-188">Next Steps</span></span>
<span data-ttu-id="2c439-189">Teď, když jste se naučili základy hello front Service Bus, najdete v části [Service Bus fronty, témata a odběry] [ Service Bus queues, topics, and subscriptions] Další informace.</span><span class="sxs-lookup"><span data-stu-id="2c439-189">Now that you've learned hello basics of Service Bus queues, see [Service Bus queues, topics, and subscriptions][Service Bus queues, topics, and subscriptions] for more information.</span></span>

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse.md
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter 
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#Microsoft_ServiceBus_Messaging_SqlFilter_SqlExpression
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage

[0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
[2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
[3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
