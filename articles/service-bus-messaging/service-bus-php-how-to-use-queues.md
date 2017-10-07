---
title: aaaHow toouse Service Bus fronty s PHP | Microsoft Docs
description: "Zjistěte, jak fronty toouse Service Bus v Azure. Ukázky kódu jsou vytvořeny v jazyce PHP."
services: service-bus-messaging
documentationcenter: php
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e29c829b-44c5-4350-8f2e-39e0c380a9f2
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 8cf233176029b679d172eaf713632087beca5e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-php"></a>Jak toouse Service Bus fronty s PHP
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tento průvodce vám ukáže, jak toouse fronty Service Bus. Hello ukázky jsou napsané v jazyce PHP a používají hello [Azure SDK pro jazyk PHP](../php-download-sdk.md). Hello pokryté scénáře zahrnují **vytváření front**, **odesílání a přijímání zpráv**, a **odstraňování front**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>Vytvoření aplikace PHP
Hello jen požadavek pro vytvoření aplikace PHP, který přistupuje k službě Azure Blob hello je hello odkazující na tříd v hello [Azure SDK pro jazyk PHP](../php-download-sdk.md) z vašeho kódu. Můžete všechny toocreate nástroje pro vývoj aplikací nebo Poznámkový blok.

> [!NOTE]
> Instalace PHP musí mít také hello [OpenSSL rozšíření](http://php.net/openssl) nainstalované a povolené.
> 
> 

V této příručce bude používat funkce služby, které lze volat z v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.

## <a name="get-hello-azure-client-libraries"></a>Získání knihovny klienta Azure hello
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-toouse-service-bus"></a>Konfigurace vaší aplikace toouse Service Bus
fronty Service Bus hello toouse rozhraní API, hello následující:

1. Referenční dokumentace hello automatického zavaděče soubor pomocí hello [require_once] [ require_once] příkaz.
2. Referenční všechny třídy, které můžete použít.

Hello následující příklad ukazuje, jak tooinclude hello automatického zavaděče souboru a odkaz hello `ServicesBuilder` třídy.

> [!NOTE]
> Tento příklad (a další příklady v tomto článku) předpokládá, že jste nainstalovali hello PHP klientské knihovny pro Azure prostřednictvím autora. Pokud jste nainstalovali hello knihovny ručně nebo jako balíček HRUŠKAMI, musíte odkázat hello **WindowsAzure.php** automatického zavaděče souboru.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

V následujících příkladech hello, hello `require_once` příkaz vždy se zobrazí, ale pouze hello třídy potřebné pro tooexecute příklad hello odkazují.

## <a name="set-up-a-service-bus-connection"></a>Nastavení připojení služby Service Bus
tooinstantiate Service Bus klient platný připojovací řetězec musí mít nejprve v tomto formátu:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Kde `Endpoint` je obvykle ve formátu hello `[yourNamespace].servicebus.windows.net`.

toocreate libovolného klienta služby Azure, je nutné použít hello `ServicesBuilder` třídy. Můžete:

* Předat hello připojovací řetězec přímo tooit.
* Použití hello **CloudConfigurationManager (CCM)** toocheck více externí zdroje pro hello připojovací řetězec:
  * Ve výchozím nastavení je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí
  * Můžete přidat nové zdroje rozšířením hello `ConnectionStringSource` – třída

Zde uvedené příklady hello je předaná přímo hello připojovací řetězec.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-queue"></a>Vytvoření fronty
Můžete provádět operace správy front služby Service Bus přes hello `ServiceBusRestProxy` třídy. A `ServiceBusRestProxy` je objekt vytvořený prostřednictvím hello `ServicesBuilder::createServiceBusService` metoda factory řetězcem odpovídající připojení, který zapouzdřuje hello tokenu oprávnění toomanage ho.

Následující příklad ukazuje, jak Hello tooinstantiate `ServiceBusRestProxy` a volání `ServiceBusRestProxy->createQueue` toocreate frontu s názvem `myqueue` v rámci `MySBNamespace` oboru názvů služby:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    $queueInfo = new QueueInfo("myqueue");

    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
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
> Můžete použít hello `listQueues` metodu `ServiceBusRestProxy` objekty toocheck Pokud fronta se zadaným názvem již existuje v rámci oboru názvů.
> 
> 

## <a name="send-messages-tooa-queue"></a>Odesílat zprávy fronty tooa
toosend fronty Service Bus zprávu tooa aplikace volá hello `ServiceBusRestProxy->sendQueueMessage` metoda. Hello následující kód ukazuje, jak toosend zpráva toohello `myqueue` fronty vytvořili v rámci `MySBNamespace` oboru názvů služby.

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
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
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

Zprávy odeslané příliš (a přijaté z) fronty Service Bus jsou instance třídy hello [BrokeredMessage] [ BrokeredMessage] třídy. [BrokeredMessage] [ BrokeredMessage] objekty mají sadu standardních metod a vlastností, které jsou používané toohold vlastní vlastnosti specifické pro aplikace a tělo s libovolnými aplikačními daty.

Fronty Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md). Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB. Neexistuje žádné omezení na hello počet zpráv držených ve frontě, ale není na hello celková velikost hello zpráv držených ve frontě. Toto omezení velikost fronty je 5 GB.

## <a name="receive-messages-from-a-queue"></a>Příjem zpráv z fronty

Hello nejlepší způsob, jak tooreceive zprávy z fronty je toouse `ServiceBusRestProxy->receiveQueueMessage` metoda. Můžete obdržet zprávy ve dvou různých režimech: [ *ReceiveAndDelete* ](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) a [ *PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock). **PeekLock** je výchozí hello.

Při použití [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) režimu přijímat je jednorázová operace; to znamená, když Service Bus přijme požadavek čtení zprávy ve frontě, označí uvítací zprávu jako spotřebovávanou a vrátí ji toohello aplikace. [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) režimu je hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat hello události selhání se zpráva nezpracuje. toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho. Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.

Ve výchozím nastavení hello [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) režimu, přijímání zprávy se změní na dvě fáze operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv. Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent dalšími uživateli, příjem a vrátí ji toohello aplikace. Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces příliš předáním hello přijata zpráva`ServiceBusRestProxy->deleteMessage`. Když Service Bus uvidí hello `deleteMessage` volání, která se bude označit uvítací zprávu jako spotřebovávanou a odeberte ji z fronty hello.

Následující příklad ukazuje, jak Hello tooreceive a zpracovat zprávu pomocí [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) hello výchozí (režim).

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set hello receive mode tooPeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Jak toohandle aplikace spadne a nečitelných zpráv

Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy. Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello `unlockMessage` na hello přijal zprávu (místo hello `deleteMessage` metoda). To bude způsobit, že Service Bus toounlock uvítací zprávu ve frontě hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.

Je také vypršení časového limitu přidružené zpráva uzamčená v rámci hello fronty, a pokud aplikace hello selže tooprocess uvítací zprávu před hello zámku vyprší časový limit (například pokud hello aplikace spadne), pak Service Bus odemknutím uvítací zprávu automaticky a nastavit jej jako dostupné toobe přijetí.

V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello `deleteMessage` požadavku a potom uvítací zprávu bude víckrát toohello aplikace odešle znovu. To se často označuje jako *nejméně jednou* zpracování; to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích hello může doručit víckrát. Pokud hello scénář nemůže tolerovat zpracování duplicitní pak přidáte další se doporučuje logiku tooapplications toohandle víckrát doručené zprávy. To se často opírá hello `getMessageId` metoda hello zprávy, která zůstává konstantní mezi pokusy o doručení.

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello front Service Bus, najdete v části [fronty, témata a odběry] [ Queues, topics, and subscriptions] Další informace.

Další informace najdete také navštívit hello [středisku pro vývojáře PHP](https://azure.microsoft.com/develop/php/).

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once


