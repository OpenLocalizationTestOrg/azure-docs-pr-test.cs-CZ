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
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-java"></a>Jak toouse Service Bus témata a odběry s Javou

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tato příručka popisuje, jak toouse Service Bus témat a odběrů. Hello ukázky jsou napsané v jazyce Java a používají hello [Azure SDK pro jazyk Java][Azure SDK for Java]. Hello pokryté scénáře zahrnují **vytváření témat a odběrů**, **vytváření filtrů odběrů**, **odesílání zpráv tooa tématu**, **přijetí zprávy z odběru**, a **odstranění témat a odběrů**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Co jsou témata a předplatné služby Service Bus?
Témata a předplatné služby Service Bus podporují komunikační model zasílání zpráv *publikování/přihlášení*. Součásti distribuované aplikace při používání témat a předplatných nekomunikují navzájem přímo. Místo toho si zprávy vyměňují prostřednictvím tématu, které slouží jako zprostředkovatel.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Na rozdíl od front služby Service Bus, ve kterých každou zprávu zpracuje jeden spotřebitel, témata a předplatné nabízejí komunikaci v podobě „1:N“ a používají vzor publikování/přihlášení. Je možné zaregistrovat několik předplatných tooa tématu. Při odeslání zprávy tooa tématu, je pak k proces toohandle předplatného dostupná tooeach nezávisle.

Téma tooa předplatného se podobá virtuální frontě, která obdrží kopii zprávy hello, které byly odeslány toohello tématu. Volitelně můžete zaregistrovat pravidla filtru pro téma na základě za předplatné, což vám umožní toofilter nebo omezit přijme které zprávy tooa téma podle předplatných tématu.

Témata a odběry Service Bus umožňují tooscale tooprocess velký počet zpráv mezi velký počet uživatelů a aplikací.

## <a name="create-a-service-namespace"></a>Vytvoření oboru názvů služby
toobegin používání témat a odběrů Service Bus v Azure, musíte nejdřív vytvořit obor názvů, který poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.

toocreate obor názvů:

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-toouse-service-bus"></a>Konfigurace vaší aplikace toouse Service Bus
Ujistěte se, že máte nainstalovanou hello [Azure SDK pro jazyk Java] [ Azure SDK for Java] před vytvořením této ukázce. Pokud používáte Eclipse, můžete nainstalovat hello [nástrojů Azure pro Eclipse] [ Azure Toolkit for Eclipse] zahrnující hello Azure SDK pro jazyk Java. Poté můžete přidat hello **knihovny Microsoft Azure Libraries for Java** tooyour projektu:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Přidejte následující hello `import` toohello příkazy na začátek souboru Java hello:

```java
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Přidejte hello knihovny Azure Libraries for Java tooyour cesta sestavení a její zahrnutí do vašeho projektu nasazení sestavení.

## <a name="create-a-topic"></a>Vytvoření tématu
Operace správy témat sběrnice Service Bus je možné provádět prostřednictvím **ServiceBusContract** třídy. A **ServiceBusContract** objektu je vytvořený pomocí odpovídající konfiguraci, který zapouzdřuje token SAS s oprávněními toomanage a hello **ServiceBusContract** třída je jediný bod hello komunikace s Azure.

Hello **ServiceBusService** třída poskytuje metody toocreate, výčet a odstranění témat. Následující příklad ukazuje, jak Hello **ServiceBusService** objekt může být použité toocreate téma s názvem `TestTopic`, k oboru názvů s názvem `HowToSample`:

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

Způsoby na **TopicInfo** , povolte vlastnosti tématu hello nastavení (například: tooset hello výchozí time to live (TTL) hodnotu toobe použít toomessages odeslané toohello tématu). Hello následující příklad ukazuje, jak toocreate téma s názvem `TestTopic` s maximální velikostí 5 GB:

```java
long maxSizeInMegabytes = 5120;  
TopicInfo topicInfo = new TopicInfo("TestTopic");  
topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateTopicResult result = service.createTopic(topicInfo);
```

Všimněte si, které můžete použít hello **listTopics** metodu **ServiceBusContract** objekty toocheck Pokud téma se zadaným názvem již existuje v rámci oboru názvů služby.

## <a name="create-subscriptions"></a>Vytvořte odběr
Odběry tootopics jsou také vytvořené pomocí hello **ServiceBusService** třídy. Odběry mají názvy a můžou mít volitelné filtry, které omezují skupinu zpráv předávaných virtuální fronty odběru toohello hello.

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a>Vytvoření odběru s filtrem (MatchAll) výchozí hello
Hello **MatchAll** filtr je hello výchozí filtr, který se používá v případě, že při vytvoření nového předplatného je zadán žádný filtr. Když hello **MatchAll** filtr se používá, všechny zprávy publikované toohello tématu ukládány do virtuální fronty odběru. Hello následující příklad vytvoří odběr s názvem "AllMessages" a používá hello výchozí **MatchAll** filtru.

```java
SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
CreateSubscriptionResult result =
    service.createSubscription("TestTopic", subInfo);
```

### <a name="create-subscriptions-with-filters"></a>Vytvoření odběru s filtry
Můžete také vytvořit filtry, které umožňují tooscope příjem zpráv odeslaných tooa tématu by měla objeví v konkrétním odběru tématu.

je technologie Hello nejflexibilnější filtr, který odběry podporují [SqlFilter][SqlFilter], který implementuje je podmnožinou SQL92. Filtry SQL pracují hello vlastnosti hello zpráv, které jsou publikované toohello tématu. Další podrobnosti o hello výrazy, které lze použít s filtrem SQL, projděte si hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntaxe.

Hello následující příklad vytvoří odběr s názvem `HighMessages` s [SqlFilter] [ SqlFilter] objekt, který vybere jen zprávy, které mají vlastní **MessageNumber** Vlastnost větší než 3:

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

Podobně hello následující příklad vytvoří odběr s názvem `LowMessages` s [SqlFilter] [ SqlFilter] objekt, který vybere jen zprávy, které mají **MessageNumber** Vlastnost menší než nebo rovna too3:

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

Když je nyní odeslána zpráva příliš`TestTopic`, bude vždy doručena tooreceivers odběru toohello `AllMessages` předplatného a selektivně tooreceivers odběru toohello `HighMessages` a `LowMessages` odběry ( v závislosti na obsahu zprávy).

## <a name="send-messages-tooa-topic"></a>Odeslání zprávy tooa tématu
Získá toosend tématu Service Bus zprávu tooa, aplikace **ServiceBusContract** objektu. Hello následující kód ukazuje, jak toosend zprávu o hello `TestTopic` tématu předtím vytvořili v rámci hello `HowToSample` obor názvů:

```java
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

Zprávy odeslané témata tooService Bus jsou instance [BrokeredMessage] [ BrokeredMessage] třídy. [BrokeredMessage][BrokeredMessage]* objekty mají sadu standardních metod (například **setLabel** a **TimeToLive**), slovník, který je použité toohold vlastní vlastnosti specifické pro aplikace a tělo s libovolnými aplikačními daty. Aplikace můžete hello tělo zprávy nastavit tak předá jakýkoli serializovatelný objekt do konstruktoru hello [BrokeredMessage][BrokeredMessage]a odpovídající hello **DataContractSerializer**  bude použité tooserialize hello objektu. Alternativně **java.io.InputStream** lze zadat.

Hello následující příklad ukazuje, jak testovací toosend pět zprávy toothe `TestTopic` **MessageSender** jsme získali ve fragmentu kódu hello výše.
Poznámka: jak hello **MessageNumber** hodnotu vlastnosti každé zprávy se liší na hello iteraci smyčky hello (to určí, které odběry ji přijmou):

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

Témata Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md). Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB. Neexistuje žádné omezení na hello počet zpráv držených v tématu, ale je omezena na hello celková velikost hello zpráv držených v tématu. Velikost tématu se definuje při vytvoření, maximální limit je 5 GB.

## <a name="how-tooreceive-messages-from-a-subscription"></a>Jak tooreceive zprávy z odběru
použít tooreceive zprávy z odběru, **ServiceBusContract** objektu. Přijaté zprávy můžou pracovat ve dvou různých režimech: **ReceiveAndDelete** a **PeekLock**.

Při použití hello **ReceiveAndDelete** režimu přijímat je jednorázová operace – to znamená, když Service Bus přijme požadavek čtení zprávy, označí uvítací zprávu jako spotřebovávanou a vrátí ji toothe aplikace. **ReceiveAndDelete** režimu je hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat hello události selhání se zpráva nezpracuje. toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho. Vzhledem k tomu, že Service Bus se už ale zprávu označila jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.

V **PeekLock** režimu, zobrazí se změní na dvě fáze operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv. Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace. Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces voláním **odstranit** na uvítací přijal zprávu. Když Service Bus uvidí hello **odstranit** volání, která se bude označit uvítací zprávu jako spotřebovávanou a jeho odebrání z tématu hello.

Hello následující příklad ukazuje, jak můžete obdržet zprávy a zpracování pomocí **PeekLock** režimu (ne hello výchozí režim). Hello příklad provede smyčku a zpracovává zprávy v hello "HighMessages" předplatného a pak ukončí, pokud nejsou žádné další zprávy (případně může být nastavena toowait pro nové zprávy).

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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Jak toohandle aplikace spadne a nečitelných zpráv
Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy. Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello **unlockMessage** na hello přijal zprávu (místo hello **deleteMessage** metoda). To bude způsobit, že Service Bus toounlock uvítací zprávu v rámci hello tématu a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.

Je také vypršení časového limitu přidružené zpráva uzamčená v tomto tématu, a pokud aplikace hello selže tooprocess uvítací zprávu před zámku vyprší časový limit (například pokud hello aplikace spadne), pak Service Bus se automatické odemknutí uvítací zprávu a nastavte jej k dispozici toobe přijetí.

V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello **deleteMessage** požadavku a potom uvítací zprávu bude víckrát toohello aplikace odešle znovu. To se často označuje jako **zpracování nejméně jednou**, který je každá zpráva se zpracuje alespoň jednou, ale v určitých situacích hello může doručit víckrát. Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy. To se často opírá hello **getMessageId** metoda hello zprávy, která zůstane konstantní mezi pokusy o doručení.

## <a name="delete-topics-and-subscriptions"></a>Odstranění témat a odběrů
Hello primární způsob toodelete témata a odběry je toouse **ServiceBusContract** objektu. Odstraní téma také odstraní všechny odběry, které jsou registrovány hello tématu. Odběry se taky dají odstranit samostatně.

```java
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello front Service Bus, najdete v části [Service Bus fronty, témata a odběry] [ Service Bus queues, topics, and subscriptions] Další informace.

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse.md
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter 
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#Microsoft_ServiceBus_Messaging_SqlFilter_SqlExpression
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage

[0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
[2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
[3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
