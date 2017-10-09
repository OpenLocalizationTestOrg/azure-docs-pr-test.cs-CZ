---
title: "témata Service Bus toouse aaaHow s PHP | Microsoft Docs"
description: "Zjistěte, jak toouse témat sběrnice Service Bus s PHP v Azure."
services: service-bus-messaging
documentationcenter: php
author: sethmanheim
manager: timlt
editor: 
ms.assetid: faaa4bbd-f6ef-42ff-aca7-fc4353976449
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/27/2017
ms.author: sethm
ms.openlocfilehash: 0ca8625fa3edc5854c0d6c1c2f6adab6a2d42f91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-php"></a>Jak toouse Service Bus témata a odběry s PHP

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tento článek ukazuje, jak toouse Service Bus témat a odběrů. Hello ukázky jsou napsané v jazyce PHP a používají hello [Azure SDK pro jazyk PHP](../php-download-sdk.md). Hello pokryté scénáře zahrnují **vytváření témat a odběrů**, **vytváření filtrů odběrů**, **odesílání zpráv tooa tématu**, **přijetí zprávy z odběru**, a **odstranění témat a odběrů**.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>Vytvoření aplikace PHP
Hello jen požadavek pro vytvoření aplikace PHP, který přistupuje k službě Azure Blob hello je tooreference třídy v hello [Azure SDK pro jazyk PHP](../php-download-sdk.md) z vašeho kódu. Můžete všechny toocreate nástroje pro vývoj aplikací nebo Poznámkový blok.

> [!NOTE]
> Instalace PHP musí mít také hello [OpenSSL rozšíření](http://php.net/openssl) nainstalované a povolené.
> 
> 

Tento článek popisuje, jak toouse služby funkce, které lze volat v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.

## <a name="get-hello-azure-client-libraries"></a>Získání knihovny klienta Azure hello
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-toouse-service-bus"></a>Konfigurace vaší aplikace toouse Service Bus
toouse hello API pro Service Bus:

1. Referenční dokumentace hello automatického zavaděče soubor pomocí hello [require_once] [ require-once] příkaz.
2. Referenční všechny třídy, které můžete použít.

Hello následující příklad ukazuje, jak tooinclude hello automatického zavaděče souboru a odkaz hello **ServiceBusService** třídy.

> [!NOTE]
> Tento příklad (a další příklady v tomto článku) předpokládá, že jste nainstalovali hello PHP klientské knihovny pro Azure prostřednictvím autora. Pokud jste nainstalovali hello knihovny ručně nebo jako balíček HRUŠKAMI, musíte odkázat hello **WindowsAzure.php** automatického zavaděče souboru.
> 
> 

```php
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

V následující příklady hello, hello `require_once` příkaz vždy se zobrazí, ale pouze hello třídy potřebné pro tooexecute příklad hello odkazují.

## <a name="set-up-a-service-bus-connection"></a>Nastavení připojení služby Service Bus
tooinstantiate Service Bus klienta, musíte nejdřív mají platný připojovací řetězec v tomto formátu:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Kde `Endpoint` je obvykle ve formátu hello `https://[yourNamespace].servicebus.windows.net`.

toocreate libovolného klienta služby Azure, je nutné použít hello `ServicesBuilder` třídy. Můžete:

* Předat hello připojovací řetězec přímo tooit.
* Použití hello **CloudConfigurationManager (CCM)** toocheck více externí zdroje pro hello připojovací řetězec:
  * Ve výchozím nastavení je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí.
  * Můžete přidat nové zdroje rozšířením hello `ConnectionStringSource` třídy.

Zde uvedené příklady hello je předaná přímo hello připojovací řetězec.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Vytvoření tématu
Můžete provádět operace správy témat sběrnice Service Bus přes hello `ServiceBusRestProxy` třídy. A `ServiceBusRestProxy` je objekt vytvořený prostřednictvím hello `ServicesBuilder::createServiceBusService` metoda factory řetězcem odpovídající připojení, který zapouzdřuje hello tokenu oprávnění toomanage ho.

Následující příklad ukazuje, jak Hello tooinstantiate `ServiceBusRestProxy` a volání `ServiceBusRestProxy->createTopic` toocreate téma s názvem `mytopic` v rámci `MySBNamespace` obor názvů:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {        
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [!NOTE]
> Můžete použít hello `listTopics` metodu `ServiceBusRestProxy` objekty toocheck Pokud téma se zadaným názvem již existuje v rámci oboru názvů služby.
> 
> 

## <a name="create-a-subscription"></a>Vytvoření odběru
Odběry témat taky jsou vytvořeny pomocí hello `ServiceBusRestProxy->createSubscription` metoda. Odběry mají názvy a můžou mít volitelné filtry, které omezují skupinu zpráv předávaných virtuální fronty odběru toohello hello.

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a>Vytvoření odběru s filtrem (MatchAll) výchozí hello
Hello **MatchAll** filtr je hello výchozí filtr, který se používá v případě, že při vytvoření nového předplatného je zadán žádný filtr. Když hello **MatchAll** filtr se používá, všechny zprávy publikované toohello tématu ukládány do virtuální fronty odběru hello. Hello následující příklad vytvoří odběr s názvem 'mysubscription' a používá hello výchozí **MatchAll** filtru.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Vytvoření odběru s filtry
Můžete také nastavit filtry, které umožňují toospecify příjem zpráv odeslaných tooa tématu by měl být použit v konkrétním odběru tématu. Hello nejflexibilnější filtr, který odběry podporují je hello [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter), který implementuje je podmnožinou SQL92. Filtry SQL pracují hello vlastnosti hello zpráv, které jsou publikované toohello tématu. Další informace o SqlFilters najdete v tématu [SqlFilter.SqlExpression vlastnost][sqlfilter].

> [!NOTE]
> Každé pravidlo na předplatném zpracuje příchozí zprávy nezávisle, přidání svoje předplatné toohello výsledek zprávy. Kromě toho má každý nový odběr výchozí **pravidlo** objekt s filtr, který přidá všechny zprávy z odběru toohello tématu hello. tooreceive pouze zprávy odpovídající vašemu filtru, musíte odebrat hello výchozí pravidlo. Hello výchozí pravidla můžete odebrat pomocí hello `ServiceBusRestProxy->deleteRule` metoda.
> 
> 

Hello následující příklad vytvoří odběr s názvem `HighMessages` s **SqlFilter** který vybere jen zprávy, které mají vlastní `MessageNumber` vlastnost větší než 3. V tématu [odeslání zprávy tooa tématu](#send-messages-to-a-topic) informace o přidání vlastních vlastností toomessages.

```php
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Všimněte si, že tento kód vyžaduje použití hello další oboru názvů: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Podobně hello následující příklad vytvoří odběr s názvem `LowMessages` s `SqlFilter` který vybere jen zprávy, které mají `MessageNumber` vlastnost menší než nebo rovna too3.

```php
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Teď, když je odeslána zpráva toohello `mytopic` tématu, vždy se dodá tooreceivers odběru toohello `mysubscription` předplatného a selektivně tooreceivers odběru toohello `HighMessages` a `LowMessages` (odběrů v závislosti na obsahu zprávy hello).

## <a name="send-messages-tooa-topic"></a>Odeslání zprávy tooa tématu
toosend tématu Service Bus zprávu tooa aplikace volá hello `ServiceBusRestProxy->sendTopicMessage` metoda. Hello následující kód ukazuje, jak toosend zpráva toohello `mytopic` vytvořili v tématu `MySBNamespace` oboru názvů služby.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");

    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Zprávy odeslané témata tooService Bus jsou instance třídy hello [BrokeredMessage] [ BrokeredMessage] třídy. [BrokeredMessage] [ BrokeredMessage] objekty mají sadu standardních vlastností a metod, jakož i vlastnosti, které se dají použít toohold vlastní vlastnosti specifické pro aplikaci. Hello následující příklad ukazuje, jak testovací toosend 5 zprávy toohello `mytopic` tématu vytvořili. Hello `setProperty` metoda je použité tooadd vlastní vlastnosti (`MessageNumber`) tooeach zprávy. Všimněte si, že hello `MessageNumber` vlastnost hodnota se liší u každé zprávy (můžete použít tuto hodnotu toodetermine, které odběry ji, přijmou, jak je znázorněno v hello [vytvořit odběr](#create-a-subscription) část):

```php
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);

    // Set custom property.
    $message->setProperty("MessageNumber", $i);

    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Témata Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md). Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB. Neexistuje žádné omezení na hello počet zpráv držených v tématu, ale není na hello celková velikost hello zpráv držených v tématu. Toto omezení velikost tématu je 5 GB. Další informace o kvótách najdete v tématu [Service Bus kvóty][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Příjem zpráv z odběru
Hello nejlepší způsob, jak tooreceive zprávy z odběru je toouse `ServiceBusRestProxy->receiveSubscriptionMessage` metoda. Můžete obdržet zprávy ve dvou různých režimech: [ *ReceiveAndDelete* a *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode). **PeekLock** je výchozí hello.

Při použití hello [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) režimu přijímat je jednorázová operace; to znamená, když Service Bus přijme požadavek čtení zprávy v odběru, označí uvítací zprávu jako spotřebovávanou a vrátí ji toohello aplikace. [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) * režimu je hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat hello události selhání se zpráva nezpracuje. toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho. Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.

Ve výchozím nastavení hello [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) režimu, přijímání zprávy se změní na dvě fáze operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv. Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace. Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces příliš předáním hello přijata zpráva`ServiceBusRestProxy->deleteMessage`. Když Service Bus uvidí hello `deleteMessage` volání, která se bude označit uvítací zprávu jako spotřebovávanou a odeberte ji z fronty hello.

Následující příklad ukazuje, jak Hello tooreceive a zpracovat zprávu pomocí [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) hello výchozí (režim). 

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set receive mode tooPeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Postupy: zpracování pádů aplikace a nečitelných zpráv
Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy. Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello `unlockMessage` na hello přijal zprávu (místo hello `deleteMessage` metoda). To bude způsobit, že Service Bus toounlock uvítací zprávu ve frontě hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.

Je také vypršení časového limitu přidružené zpráva uzamčená v rámci hello fronty, a pokud aplikace hello selže tooprocess uvítací zprávu před hello zámku vyprší časový limit (například pokud hello aplikace spadne), pak Service Bus odemknutím uvítací zprávu automaticky a nastavit jej jako dostupné toobe přijetí.

V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello `deleteMessage` požadavku a potom uvítací zprávu bude víckrát toohello aplikace odešle znovu. To se často označuje jako *nejméně jednou* zpracování; to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích hello může doručit víckrát. Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tooapplications toohandle víckrát doručené zprávy. To se často opírá hello `getMessageId` metoda hello zprávy, která zůstává konstantní mezi pokusy o doručení.

## <a name="delete-topics-and-subscriptions"></a>Odstranění témat a odběrů
toodelete a tématu nebo předplatného, použijte hello `ServiceBusRestProxy->deleteTopic` nebo hello `ServiceBusRestProxy->deleteSubscripton` metody, v uvedeném pořadí. Všimněte si, že se odstraní téma také odstraní všechny odběry, které jsou registrovány hello tématu.

Hello následující příklad ukazuje, jak toodelete téma s názvem `mytopic` a jeho registrované odběry.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {        
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Pomocí hello `deleteSubscription` metodu, můžete odstranit odběr nezávisle:

```php
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello front Service Bus, najdete v části [fronty, témata a odběry] [ Queues, topics, and subscriptions] Další informace.

[BrokeredMessage]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter
[require-once]: http://php.net/require_once
[Service Bus quotas]: service-bus-quotas.md
