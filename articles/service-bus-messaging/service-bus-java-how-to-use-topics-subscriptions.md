---
title: "Jak používat témata služby Azure Service Bus s Javou | Microsoft Docs"
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
ms.openlocfilehash: b561d6fdcf4fb2839908ac8f53832fb0830dd576
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-java"></a><span data-ttu-id="93b15-103">Jak používat témata a odběry Service Bus s Javou</span><span class="sxs-lookup"><span data-stu-id="93b15-103">How to use Service Bus topics and subscriptions with Java</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="93b15-104">Tato příručka popisuje, jak používat témata a odběry Service Bus.</span><span class="sxs-lookup"><span data-stu-id="93b15-104">This guide describes how to use Service Bus topics and subscriptions.</span></span> <span data-ttu-id="93b15-105">Ukázky jsou napsané v jazyce Java a použít [Azure SDK pro jazyk Java][Azure SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="93b15-105">The samples are written in Java and use the [Azure SDK for Java][Azure SDK for Java].</span></span> <span data-ttu-id="93b15-106">Pokryté scénáře zahrnují **vytváření témat a odběrů**, **vytváření filtrů odběrů**, **odesílání zpráv do tématu**, **přijímání zpráv z odběru**, a **odstranění témat a odběrů**.</span><span class="sxs-lookup"><span data-stu-id="93b15-106">The scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages to a topic**, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span>

## <a name="what-are-service-bus-topics-and-subscriptions"></a><span data-ttu-id="93b15-107">Co jsou témata a předplatné služby Service Bus?</span><span class="sxs-lookup"><span data-stu-id="93b15-107">What are Service Bus topics and subscriptions?</span></span>
<span data-ttu-id="93b15-108">Témata a předplatné služby Service Bus podporují komunikační model zasílání zpráv *publikování/přihlášení*.</span><span class="sxs-lookup"><span data-stu-id="93b15-108">Service Bus topics and subscriptions support a *publish/subscribe* messaging communication model.</span></span> <span data-ttu-id="93b15-109">Součásti distribuované aplikace při používání témat a předplatných nekomunikují navzájem přímo. Místo toho si zprávy vyměňují prostřednictvím tématu, které slouží jako zprostředkovatel.</span><span class="sxs-lookup"><span data-stu-id="93b15-109">When using topics and subscriptions, components of a distributed application do not communicate directly with each other; instead they exchange messages via a topic, which acts as an intermediary.</span></span>

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

<span data-ttu-id="93b15-111">Na rozdíl od front služby Service Bus, ve kterých každou zprávu zpracuje jeden spotřebitel, témata a předplatné nabízejí komunikaci v podobě „1:N“ a používají vzor publikování/přihlášení.</span><span class="sxs-lookup"><span data-stu-id="93b15-111">In contrast with Service Bus queues, in which each message is processed by a single consumer, topics and subscriptions provide a "one-to-many" form of communication, using a publish/subscribe pattern.</span></span> <span data-ttu-id="93b15-112">K jednomu tématu můžete zaregistrovat několik předplatných.</span><span class="sxs-lookup"><span data-stu-id="93b15-112">It is possible to register multiple subscriptions to a topic.</span></span> <span data-ttu-id="93b15-113">Při odeslání zprávy do tématu bude zpráva zpřístupněná všem předplatným, aby ji nezávisle zpracovala.</span><span class="sxs-lookup"><span data-stu-id="93b15-113">When a message is sent to a topic, it is then made available to each subscription to handle/process independently.</span></span>

<span data-ttu-id="93b15-114">Předplatné tématu se podobá virtuální frontě, která obdrží kopii zpráv, které byly odeslány do tématu.</span><span class="sxs-lookup"><span data-stu-id="93b15-114">A subscription to a topic resembles a virtual queue that receives copies of the messages that were sent to the topic.</span></span> <span data-ttu-id="93b15-115">Volitelně můžete zaregistrovat pravidla filtru pro téma na základě za předplatné, které umožňuje filtru nebo omezit příjem zpráv pro téma podle předplatných tématu přijme.</span><span class="sxs-lookup"><span data-stu-id="93b15-115">You can optionally register filter rules for a topic on a per-subscription basis, which allows you to filter/restrict which messages to a topic are received by which topic subscriptions.</span></span>

<span data-ttu-id="93b15-116">Témata a odběry Service Bus umožní škálování zpracovat velký počet zpráv mezi velký počet uživatelů a aplikací.</span><span class="sxs-lookup"><span data-stu-id="93b15-116">Service Bus topics and subscriptions enable you to scale to process a very large number of messages across a very large number of users and applications.</span></span>

## <a name="create-a-service-namespace"></a><span data-ttu-id="93b15-117">Vytvoření oboru názvů služby</span><span class="sxs-lookup"><span data-stu-id="93b15-117">Create a service namespace</span></span>
<span data-ttu-id="93b15-118">Pokud chcete začít používat témata a odběry Service Bus v Azure, musíte nejdřív vytvořit obor názvů, který poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="93b15-118">To begin using Service Bus topics and subscriptions in Azure, you must first create a namespace, which provides a scoping container for addressing Service Bus resources within your application.</span></span>

<span data-ttu-id="93b15-119">Vytvoření oboru názvů:</span><span class="sxs-lookup"><span data-stu-id="93b15-119">To create a namespace:</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a><span data-ttu-id="93b15-120">Konfigurace aplikace pro použití služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="93b15-120">Configure your application to use Service Bus</span></span>
<span data-ttu-id="93b15-121">Ujistěte se, že máte nainstalovanou [Azure SDK pro jazyk Java] [ Azure SDK for Java] před vytvořením této ukázce.</span><span class="sxs-lookup"><span data-stu-id="93b15-121">Make sure you have installed the [Azure SDK for Java][Azure SDK for Java] before building this sample.</span></span> <span data-ttu-id="93b15-122">Pokud používáte Eclipse, můžete nainstalovat [nástrojů Azure pro Eclipse] [ Azure Toolkit for Eclipse] , který obsahuje sadu Azure SDK pro jazyk Java.</span><span class="sxs-lookup"><span data-stu-id="93b15-122">If you are using Eclipse, you can install the [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse] that includes the Azure SDK for Java.</span></span> <span data-ttu-id="93b15-123">Poté můžete přidat **knihovny Microsoft Azure Libraries for Java** do projektu:</span><span class="sxs-lookup"><span data-stu-id="93b15-123">You can then add the **Microsoft Azure Libraries for Java** to your project:</span></span>

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

<span data-ttu-id="93b15-124">Přidejte následující `import` příkazů do horní části souboru Java:</span><span class="sxs-lookup"><span data-stu-id="93b15-124">Add the following `import` statements to the top of the Java file:</span></span>

```java
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

<span data-ttu-id="93b15-125">Přidání knihovny Azure Libraries for Java do cestu k sestavení a její zahrnutí do vašeho projektu nasazení sestavení.</span><span class="sxs-lookup"><span data-stu-id="93b15-125">Add the Azure Libraries for Java to your build path and include it in your project deployment assembly.</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="93b15-126">Vytvoření tématu</span><span class="sxs-lookup"><span data-stu-id="93b15-126">Create a topic</span></span>
<span data-ttu-id="93b15-127">Operace správy témat sběrnice Service Bus je možné provádět prostřednictvím **ServiceBusContract** třídy.</span><span class="sxs-lookup"><span data-stu-id="93b15-127">Management operations for Service Bus topics can be performed via the **ServiceBusContract** class.</span></span> <span data-ttu-id="93b15-128">A **ServiceBusContract** objektu je vytvořený pomocí odpovídající konfiguraci, který zapouzdřuje tokenu SAS s oprávněními ke správě, a **ServiceBusContract** třída je jediný bod komunikace s Azure.</span><span class="sxs-lookup"><span data-stu-id="93b15-128">A **ServiceBusContract** object is constructed with an appropriate configuration that encapsulates the SAS token with permissions to manage it, and the **ServiceBusContract** class is the sole point of communication with Azure.</span></span>

<span data-ttu-id="93b15-129">**ServiceBusService** třída poskytuje metody pro vytvoření, výčet a odstranění témat.</span><span class="sxs-lookup"><span data-stu-id="93b15-129">The **ServiceBusService** class provides methods to create, enumerate, and delete topics.</span></span> <span data-ttu-id="93b15-130">Následující příklad ukazuje způsob **ServiceBusService** objekt lze použít k vytvoření téma s názvem `TestTopic`, k oboru názvů s názvem `HowToSample`:</span><span class="sxs-lookup"><span data-stu-id="93b15-130">The following example shows how a **ServiceBusService** object can be used to create a topic named `TestTopic`, with a namespace called `HowToSample`:</span></span>

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

<span data-ttu-id="93b15-131">Způsoby na **TopicInfo** které umožňují vlastnosti tématu, které chcete nastavit (například: Chcete-li nastavit výchozí time to live (TTL) hodnotu se použije na zprávy odeslané do tématu).</span><span class="sxs-lookup"><span data-stu-id="93b15-131">There are methods on **TopicInfo** that enable properties of the topic to be set (for example: to set the default time-to-live (TTL) value to be applied to messages sent to the topic).</span></span> <span data-ttu-id="93b15-132">Následující příklad ukazuje, jak vytvořit téma s názvem `TestTopic` s maximální velikostí 5 GB:</span><span class="sxs-lookup"><span data-stu-id="93b15-132">The following example shows how to create a topic named `TestTopic` with a maximum size of 5 GB:</span></span>

```java
long maxSizeInMegabytes = 5120;  
TopicInfo topicInfo = new TopicInfo("TestTopic");  
topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateTopicResult result = service.createTopic(topicInfo);
```

<span data-ttu-id="93b15-133">Všimněte si, že můžete použít **listTopics** metodu **ServiceBusContract** objekty, které chcete zkontrolovat, pokud téma se zadaným názvem již existuje v rámci oboru názvů služby.</span><span class="sxs-lookup"><span data-stu-id="93b15-133">Note that you can use the **listTopics** method on **ServiceBusContract** objects to check if a topic with a specified name already exists within a service namespace.</span></span>

## <a name="create-subscriptions"></a><span data-ttu-id="93b15-134">Vytvořte odběr</span><span class="sxs-lookup"><span data-stu-id="93b15-134">Create subscriptions</span></span>
<span data-ttu-id="93b15-135">Odběry témat taky jsou vytvořeny pomocí **ServiceBusService** třídy.</span><span class="sxs-lookup"><span data-stu-id="93b15-135">Subscriptions to topics are also created with the **ServiceBusService** class.</span></span> <span data-ttu-id="93b15-136">Odběry mají názvy a můžou mít volitelné filtry, které omezují výběr zpráv odesílaných do virtuální fronty odběru.</span><span class="sxs-lookup"><span data-stu-id="93b15-136">Subscriptions are named and can have an optional filter that restricts the set of messages passed to the subscription's virtual queue.</span></span>

### <a name="create-a-subscription-with-the-default-matchall-filter"></a><span data-ttu-id="93b15-137">Vytvoření odběru s výchozím filtrem (MatchAll).</span><span class="sxs-lookup"><span data-stu-id="93b15-137">Create a subscription with the default (MatchAll) filter</span></span>
<span data-ttu-id="93b15-138">Filtr **MatchAll** je výchozí filtr, který se použije v případě, že při vytváření nového odběru nezadáte žádný filtr.</span><span class="sxs-lookup"><span data-stu-id="93b15-138">The **MatchAll** filter is the default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="93b15-139">Když **MatchAll** filtr se používá, všechny zprávy publikované do tématu jsou umístěny do virtuální fronty odběru.</span><span class="sxs-lookup"><span data-stu-id="93b15-139">When the **MatchAll** filter is used, all messages published to the topic are placed in the subscription's virtual queue.</span></span> <span data-ttu-id="93b15-140">Následující příklad vytvoří odběr s názvem „AllMessages“ a použije výchozí filtr **MatchAll**.</span><span class="sxs-lookup"><span data-stu-id="93b15-140">The following example creates a subscription named "AllMessages" and uses the default **MatchAll** filter.</span></span>

```java
SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
CreateSubscriptionResult result =
    service.createSubscription("TestTopic", subInfo);
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="93b15-141">Vytvoření odběru s filtry</span><span class="sxs-lookup"><span data-stu-id="93b15-141">Create subscriptions with filters</span></span>
<span data-ttu-id="93b15-142">Můžete taky vytvořit filtry, které umožňují obor, který by měl zprávy odeslané do tématu objeví v konkrétním odběru tématu.</span><span class="sxs-lookup"><span data-stu-id="93b15-142">You can also create filters that enable you to scope which messages sent to a topic should show up within a specific topic subscription.</span></span>

<span data-ttu-id="93b15-143">Nejflexibilnější filtr, který odběry podporují je [SqlFilter][SqlFilter], který implementuje je podmnožinou SQL92.</span><span class="sxs-lookup"><span data-stu-id="93b15-143">The most flexible type of filter supported by subscriptions is the [SqlFilter][SqlFilter], which implements a subset of SQL92.</span></span> <span data-ttu-id="93b15-144">Filtry SQL pracují s vlastnostmi zpráv publikované do tématu.</span><span class="sxs-lookup"><span data-stu-id="93b15-144">SQL filters operate on the properties of the messages that are published to the topic.</span></span> <span data-ttu-id="93b15-145">Další podrobnosti o výrazy, které lze použít s filtrem SQL, projděte si [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntaxe.</span><span class="sxs-lookup"><span data-stu-id="93b15-145">For more details about the expressions that can be used with a SQL filter, review the [SqlFilter.SqlExpression][SqlFilter.SqlExpression] syntax.</span></span>

<span data-ttu-id="93b15-146">Následující příklad vytvoří odběr s názvem `HighMessages` s [SqlFilter] [ SqlFilter] objekt, který vybere jen zprávy, které mají vlastní **MessageNumber** Vlastnost větší než 3:</span><span class="sxs-lookup"><span data-stu-id="93b15-146">The following example creates a subscription named `HighMessages` with a [SqlFilter][SqlFilter] object that only selects messages that have a custom **MessageNumber** property greater than 3:</span></span>

```java
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

<span data-ttu-id="93b15-147">Podobně platí, tento příklad vytvoří odběr s názvem `LowMessages` s [SqlFilter] [ SqlFilter] objekt, který vybere jen zprávy, které mají **MessageNumber** Vlastnost menší než nebo rovné 3:</span><span class="sxs-lookup"><span data-stu-id="93b15-147">Similarly, the following example creates a subscription named `LowMessages` with a [SqlFilter][SqlFilter] object that only selects messages that have a **MessageNumber** property less than or equal to 3:</span></span>

```java
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

<span data-ttu-id="93b15-148">Když je nyní odeslána zpráva `TestTopic`, budou doručeny vždy příjemci `AllMessages` předplatného a selektivně příjemcům přihlásit k odběru `HighMessages` a `LowMessages` odběry (v závislosti na obsah zprávy).</span><span class="sxs-lookup"><span data-stu-id="93b15-148">When a message is now sent to `TestTopic`, it will always be delivered to receivers subscribed to the `AllMessages` subscription, and selectively delivered to receivers subscribed to the `HighMessages` and `LowMessages` subscriptions (depending upon the message content).</span></span>

## <a name="send-messages-to-a-topic"></a><span data-ttu-id="93b15-149">Odeslání zprávy do tématu</span><span class="sxs-lookup"><span data-stu-id="93b15-149">Send messages to a topic</span></span>
<span data-ttu-id="93b15-150">K odeslání zprávy do tématu Service Bus, vaše aplikace získává **ServiceBusContract** objektu.</span><span class="sxs-lookup"><span data-stu-id="93b15-150">To send a message to a Service Bus topic, your application obtains a **ServiceBusContract** object.</span></span> <span data-ttu-id="93b15-151">Následující kód ukazuje, jak odeslat zprávu `TestTopic` předtím vytvořili v tématu `HowToSample` obor názvů:</span><span class="sxs-lookup"><span data-stu-id="93b15-151">The following code demonstrates how to send a message for the `TestTopic` topic created previously within the `HowToSample` namespace:</span></span>

```java
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

<span data-ttu-id="93b15-152">Zprávy odeslané do témat Service Bus jsou instance [BrokeredMessage] [ BrokeredMessage] třídy.</span><span class="sxs-lookup"><span data-stu-id="93b15-152">Messages sent to Service Bus Topics are instances of the [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="93b15-153">[BrokeredMessage][BrokeredMessage]* objekty mají sadu standardních metod (například **setLabel** a **TimeToLive**), slovník, který se používá k ukládání vlastní vlastnosti specifické pro aplikace a tělo s libovolnými aplikačními daty.</span><span class="sxs-lookup"><span data-stu-id="93b15-153">[BrokeredMessage][BrokeredMessage]* objects have a set of standard methods (such as **setLabel** and **TimeToLive**), a dictionary that is used to hold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="93b15-154">Aplikace může tělo zprávy nastavit podle předá jakýkoli serializovatelný objekt do konstruktoru objektu [BrokeredMessage][BrokeredMessage]a odpovídající **DataContractSerializer** pak bude sloužit k serializaci objektu.</span><span class="sxs-lookup"><span data-stu-id="93b15-154">An application can set the body of the message by passing any serializable object into the constructor of the [BrokeredMessage][BrokeredMessage], and the appropriate **DataContractSerializer** will then be used to serialize the object.</span></span> <span data-ttu-id="93b15-155">Alternativně **java.io.InputStream** lze zadat.</span><span class="sxs-lookup"><span data-stu-id="93b15-155">Alternatively, a **java.io.InputStream** can be provided.</span></span>

<span data-ttu-id="93b15-156">Následující příklad ukazuje, jak odeslat pět zkušebních zpráv, které mají `TestTopic` **MessageSender** jsme získali ve výše uvedeném fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="93b15-156">The following example demonstrates how to send five test messages to the `TestTopic` **MessageSender** we obtained in the code snippet above.</span></span>
<span data-ttu-id="93b15-157">Poznámka: Jak **MessageNumber** hodnotu vlastnosti každé zprávy se liší na iteraci smyčky (to určí, které odběry ji přijmou):</span><span class="sxs-lookup"><span data-stu-id="93b15-157">Note how the **MessageNumber** property value of each message varies on the iteration of the loop (this will determine which subscriptions receive it):</span></span>

```java
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

<span data-ttu-id="93b15-158">Témata Service Bus podporují maximální velikost zprávy 256 KB [na úrovni Standard](service-bus-premium-messaging.md) a 1 MB [na úrovni Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="93b15-158">Service Bus topics support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="93b15-159">Hlavička, která obsahuje standardní a vlastní vlastnosti aplikace, může mít velikost až 64 KB.</span><span class="sxs-lookup"><span data-stu-id="93b15-159">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="93b15-160">Neexistuje žádné omezení počtu zpráv držených v tématu, ale je omezena na celková velikost zpráv držených v tématu.</span><span class="sxs-lookup"><span data-stu-id="93b15-160">There is no limit on the number of messages held in a topic but there is a limit on the total size of the messages held by a topic.</span></span> <span data-ttu-id="93b15-161">Velikost tématu se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="93b15-161">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="how-to-receive-messages-from-a-subscription"></a><span data-ttu-id="93b15-162">Jak přijmout zprávy z odběru</span><span class="sxs-lookup"><span data-stu-id="93b15-162">How to receive messages from a subscription</span></span>
<span data-ttu-id="93b15-163">Chcete-li přijímat zprávy z odběru, použijte **ServiceBusContract** objektu.</span><span class="sxs-lookup"><span data-stu-id="93b15-163">To receive messages from a subscription, use a **ServiceBusContract** object.</span></span> <span data-ttu-id="93b15-164">Přijaté zprávy můžou pracovat ve dvou různých režimech: **ReceiveAndDelete** a **PeekLock**.</span><span class="sxs-lookup"><span data-stu-id="93b15-164">Received messages can work in two different modes: **ReceiveAndDelete** and **PeekLock**.</span></span>

<span data-ttu-id="93b15-165">Při použití **ReceiveAndDelete** režimu přijímat je jednorázová operace – to znamená, když Service Bus přijme požadavek čtení zprávy, označí zprávu jako spotřebovávanou a vrátí ji do aplikace.</span><span class="sxs-lookup"><span data-stu-id="93b15-165">When using the **ReceiveAndDelete** mode, receive is a single-shot operation - that is, when Service Bus receives a read request for a message, it marks the message as being consumed and returns it to the application.</span></span> <span data-ttu-id="93b15-166">Režim **ReceiveAndDelete** je nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat možnost, že v případě selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="93b15-166">**ReceiveAndDelete** mode is the simplest model and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="93b15-167">Pro lepší vysvětlení si představte scénář, ve kterém spotřebitel vyšle požadavek na přijetí, ale než ji může zpracovat, dojde v něm k chybě a ukončí se.</span><span class="sxs-lookup"><span data-stu-id="93b15-167">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="93b15-168">Vzhledem k tomu, že Service Bus se už ale zprávu označila jako spotřebovávanou, pak když se aplikace restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily zprávu, která se spotřebovala před havárii.</span><span class="sxs-lookup"><span data-stu-id="93b15-168">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="93b15-169">V **PeekLock** režimu, zobrazí se změní na dvě fáze operaci, která umožňuje podporuje aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="93b15-169">In **PeekLock** mode, receive becomes a two stage operation, which makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="93b15-170">Když Service Bus přijme požadavek, najde zprávu, která je na řadě ke spotřebování, uzamkne ji proti spotřebování jinými spotřebiteli a vrátí ji do aplikace.</span><span class="sxs-lookup"><span data-stu-id="93b15-170">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span> <span data-ttu-id="93b15-171">Když aplikace dokončí zpracování zprávy (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze přijetí volání **odstranit** na přijatou zprávu.</span><span class="sxs-lookup"><span data-stu-id="93b15-171">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling **Delete** on the received message.</span></span> <span data-ttu-id="93b15-172">Když Service Bus uvidí **odstranit** volání, která se bude označí zprávu jako spotřebovávanou a jeho odebrání z tématu.</span><span class="sxs-lookup"><span data-stu-id="93b15-172">When Service Bus sees the **Delete** call, it will mark the message as being consumed and remove it from the topic.</span></span>

<span data-ttu-id="93b15-173">Následující příklad ukazuje, jak můžete obdržet zprávy a zpracování pomocí **PeekLock** režimu (není výchozí režim).</span><span class="sxs-lookup"><span data-stu-id="93b15-173">The example below demonstrates how messages can be received and processed using **PeekLock** mode (not the default mode).</span></span> <span data-ttu-id="93b15-174">Následující příklad provede smyčku a zpracuje zprávy v odběru "HighMessages" a pak ukončí, pokud nejsou žádné další zprávy (případně může být nastavena pro čekání nové zprávy).</span><span class="sxs-lookup"><span data-stu-id="93b15-174">The example below performs a loop and processes messages in the "HighMessages" subscription and then exits when there are no more messages (alternatively, it could be set to wait for new messages).</span></span>

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
            // Display the topic message.
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
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="93b15-175">Zpracování pádů aplikace a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="93b15-175">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="93b15-176">Service Bus poskytuje funkce, které vám pomůžou se elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="93b15-176">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="93b15-177">Pokud přijímající aplikace nedokáže zpracovat zprávu z nějakého důvodu, pak může zavolat **unlockMessage** metoda na přijatou zprávu (místo **deleteMessage** metoda).</span><span class="sxs-lookup"><span data-stu-id="93b15-177">If a receiver application is unable to process the message for some reason, then it can call the **unlockMessage** method on the received message (instead of the **deleteMessage** method).</span></span> <span data-ttu-id="93b15-178">To způsobí, že Service Bus zprávu odemkne v tomto tématu a zpřístupní ji pro další přijetí, stejnou spotřebitelskou aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="93b15-178">This will cause Service Bus to unlock the message within the topic and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="93b15-179">Je také vypršení časového limitu přidružené zpráva uzamčená v tomto tématu, a pokud se nepodaří aplikace zprávu nezpracuje zámku vyprší časový limit (například pokud aplikace spadne), pak se Service Bus zprávu automaticky odemkne a nastavit jej jako k dispozici pro další přijetí.</span><span class="sxs-lookup"><span data-stu-id="93b15-179">There is also a timeout associated with a message locked within the topic, and if the application fails to process the message before the lock timeout expires (for example, if the application crashes), then Service Bus will unlock the message automatically and make it available to be received again.</span></span>

<span data-ttu-id="93b15-180">V případě, že aplikace spadne po zpracování zprávy, ale předtím, než **deleteMessage** požadavku a potom zpráva bude vysláním do aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="93b15-180">In the event that the application crashes after processing the message but before the **deleteMessage** request is issued, then the message will be redelivered to the application when it restarts.</span></span> <span data-ttu-id="93b15-181">To se často označuje jako **zpracování nejméně jednou**, který je každá zpráva se zpracuje alespoň jednou, ale v některých situacích může doručit víckrát stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="93b15-181">This is often called **At Least Once Processing**, that is, each message will be processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="93b15-182">Pokud daný scénář nemůže tolerovat zpracování víc než jednou, vývojáři aplikace by měli přidat další logiku navíc pro zpracování víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="93b15-182">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="93b15-183">To se často opírá **getMessageId** metoda zprávy, která zůstane konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="93b15-183">This is often achieved using the **getMessageId** method of the message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="93b15-184">Odstranění témat a odběrů</span><span class="sxs-lookup"><span data-stu-id="93b15-184">Delete topics and subscriptions</span></span>
<span data-ttu-id="93b15-185">Primární způsob, jak odstranit témat a odběrů je použití **ServiceBusContract** objektu.</span><span class="sxs-lookup"><span data-stu-id="93b15-185">The primary way to delete topics and subscriptions is to use a **ServiceBusContract** object.</span></span> <span data-ttu-id="93b15-186">Odstraní téma také odstraní všechny odběry registrované k tomuto tématu.</span><span class="sxs-lookup"><span data-stu-id="93b15-186">Deleting a topic will also delete any subscriptions that are registered with the topic.</span></span> <span data-ttu-id="93b15-187">Odběry se taky dají odstranit samostatně.</span><span class="sxs-lookup"><span data-stu-id="93b15-187">Subscriptions can also be deleted independently.</span></span>

```java
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a><span data-ttu-id="93b15-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="93b15-188">Next Steps</span></span>
<span data-ttu-id="93b15-189">Teď, když jste se naučili základy front Service Bus, najdete v části [Service Bus fronty, témata a odběry] [ Service Bus queues, topics, and subscriptions] Další informace.</span><span class="sxs-lookup"><span data-stu-id="93b15-189">Now that you've learned the basics of Service Bus queues, see [Service Bus queues, topics, and subscriptions][Service Bus queues, topics, and subscriptions] for more information.</span></span>

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse.md
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter 
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#Microsoft_ServiceBus_Messaging_SqlFilter_SqlExpression
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage

[0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
[2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
[3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
