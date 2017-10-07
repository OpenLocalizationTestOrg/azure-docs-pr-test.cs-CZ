---
title: aaaHow toouse Service Bus fronty v Node.js | Microsoft Docs
description: "Zjistěte, jak fronty toouse Service Bus v Azure z aplikace Node.js."
services: service-bus-messaging
documentationcenter: nodejs
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a87a00f9-9aba-4c49-a0df-f900a8b67b3f
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: c55354b2061c41aba1093cc3f12ce2a1bc37a3cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-nodejs"></a>Jak toouse Service Bus fronty s Node.js

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tento článek popisuje, jak toouse Service Bus fronty s Node.js. Hello ukázky jsou napsané v jazyce JavaScript a používají hello modul Node.js Azure. Hello pokryté scénáře zahrnují **vytváření front**, **odesílání a přijímání zpráv**, a **odstraňování front**. Další informace o frontách najdete v části hello [další kroky](#next-steps) části.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-nodejs-application"></a>Vytvoření aplikace Node.js
Vytvoření prázdné aplikace Node.js. Pokyny najdete v části toocreate aplikace Node.js, [vytvořit a nasadit tooan aplikace Node.js webu Azure][Create and deploy a Node.js application tooan Azure Website], nebo [Node.js cloudové služby] [ Node.js Cloud Service] pomocí prostředí Windows PowerShell.

## <a name="configure-your-application-toouse-service-bus"></a>Konfigurace vaší aplikace toouse Service Bus
toouse Azure Service Bus, stáhnout a použít balíček Node.js Azure hello. Tento balíček obsahuje sadu knihoven, které komunikují s hello služby REST pro Service Bus.

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a>Použijte uzel balíček správce (NPM) tooobtain hello balíček
1. Použití hello **prostředí Windows PowerShell pro Node.js** příkazového okna toonavigate toohello **c:\\uzlu\\sbqueues\\WebRole1** složku, ve které jste vytvořili vaší ukázkovou aplikaci.
2. Typ **npm nainstalujte azure** v hello příkazové okno, které by měl způsobit výstup podobný toohello následující:

    ```
    azure@0.7.5 node_modules\azure
        ├── dateformat@1.0.2-1.2.3
        ├── xmlbuilder@0.4.2
        ├── node-uuid@1.2.0
        ├── mime@1.2.9
        ├── underscore@1.4.4
        ├── validator@1.1.1
        ├── tunnel@0.0.2
        ├── wns@0.5.3
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```
3. Můžete ručně spustit hello **ls** tooverify příkaz, **node_modules** složka byla vytvořena. V této složce najít hello **azure** balíček, který obsahuje hello knihovny, je nutné tooaccess fronty Service Bus.

### <a name="import-hello-module"></a>Import modulu hello
Pomocí programu Poznámkový blok nebo jiném textovém editoru, přidejte následující toohello horní části hello hello **server.js** souboru aplikace hello:

```javascript
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Nastavit připojení k Azure Service Bus
Hello Azure modul čte proměnnou prostředí hello `AZURE_SERVICEBUS_CONNECTION_STRING` tooobtain informace požadované tooconnect tooService sběrnice. Pokud není nastavena tato proměnná prostředí, musíte zadat informace o účtu hello při volání metody `createServiceBusService`.

Příklad nastavení proměnných prostředí hello v konfiguračním souboru pro cloudové služby Azure, naleznete v části [Node.js Cloudová služba se úložiště][Node.js Cloud Service with Storage].

Příklad nastavení proměnných prostředí hello v hello [portál Azure] [ Azure portal] web Azure, najdete v části [webovou aplikaci Node.js s úložištěm] [ Node.js Web Application with Storage].

## <a name="create-a-queue"></a>Vytvoření fronty
Hello **ServiceBusService** objekt vám umožní toowork pomocí front Service Bus. Hello následující kód vytvoří **ServiceBusService** objektu. Přidat v hello horní části hello **server.js** souboru po hello příkaz tooimport hello modulu Azure:

```javascript
var serviceBusService = azure.createServiceBusService();
```

Při volání `createQueueIfNotExists` na hello **ServiceBusService** objektu hello určena fronty je vrácen (pokud existuje), nebo se vytvoří novou frontu se zadaným názvem hello. Hello následující kód používá `createQueueIfNotExists` toocreate nebo připojení toohello frontu s názvem `myqueue`:

```javascript
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

Hello `createServiceBusService` metoda také podporuje další možnosti, které umožňují toooverride výchozí nastavení fronty například velikost fronty toolive nebo maximální doba zprávy. Hello následující příklad ilustruje hello fronty maximální velikost too5 GB a hodnotu času toolive (TTL), 1 minuta:

```javascript
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### <a name="filters"></a>Filtry
Volitelné filtrování operace může být použité toooperations provádí pomocí **ServiceBusService**. Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu podpisem hello:

```javascript
function handle (requestOptions, next)
```

Až to jeho předběžné zpracování na možnosti hello žádost, musí volat metoda hello `next`, předání zpětné volání s hello následující podpis:

```javascript
function (returnObject, finalCallback, next)
```

V této zpětného volání a po zpracování hello `returnObject` (hello odpovědi ze serveru toohello požadavek hello), musíte buď vyvolání zpětného volání hello `next` pokud existuje toocontinue zpracování ostatní filtry, nebo jednoduše vyvolání `finalCallback`, které končí volání služby Hello.

Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí hello Azure SDK pro Node.js, `ExponentialRetryPolicyFilter` a `LinearRetryPolicyFilter`. Hello následující kód vytvoří `ServiceBusService` objekt, který používá hello `ExponentialRetryPolicyFilter`:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-tooa-queue"></a>Odesílat zprávy fronty tooa
toosend fronty Service Bus zprávu tooa aplikace volá hello `sendQueueMessage` metodu hello **ServiceBusService** objektu. Zprávy odeslané příliš (a přijaté z) jsou fronty služby Service Bus **BrokeredMessage** objekty a mají sadu standardních vlastností (jako například **popisek** a **TimeToLive**), slovník, který je použité toohold vlastní vlastnosti specifické pro aplikace a tělo s libovolnými aplikačními daty. Aplikace můžete nastavit hello těla zprávy hello předáním řetězec jako uvítací zprávu. Všechny požadované vlastnosti standardní se naplní výchozí hodnoty.

Hello následující příklad ukazuje, jak toosend zkušební zprávu toohello frontu s názvem `myqueue` pomocí `sendQueueMessage`:

```javascript
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

Fronty Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md). Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB. Neexistuje žádné omezení na hello počet zpráv držených ve frontě, ale není na hello celková velikost hello zpráv držených ve frontě. Velikost fronty se definuje při vytvoření, maximální limit je 5 GB. Další informace o kvótách najdete v tématu [Service Bus kvóty][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Příjem zpráv z fronty
Přijímání zpráv z fronty pomocí hello `receiveQueueMessage` metodu hello **ServiceBusService** objektu. Ve výchozím nastavení jsou odstraněny zprávy z fronty hello jsou pro čtení; ale můžete číst (funkce Náhled) a uzamčení uvítací zprávu bez odstranění z fronty hello podle nastavení volitelný parametr hello `isPeekLock` příliš**true**.

Hello výchozí chování čtení a odstranění uvítací zprávu jako součást hello přijímat operaci je hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat hello události selhání se zpráva nezpracuje. toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho. Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.

Pokud hello `isPeekLock` parametr je nastaven příliš**true**, přijímat hello stane dvoufázového operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv. Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace. Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces voláním `deleteMessage` metoda a poskytování toobe hello zprávu odstranit jako parametr. Hello `deleteMessage` metoda označí uvítací zprávu jako spotřebovávanou a odstraní ji z fronty hello.

Hello následující příklad ukazuje, jak se proces a tooreceive zprávy, pomocí `receiveQueueMessage`. Hello příklad nejprve obdrží a odstraní zprávu a pak obdrží zprávu pomocí `isPeekLock` nastavit příliš**true**, pak odstraní hello zprávu pomocí `deleteMessage`:

```javascript
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Jak toohandle aplikace spadne a nečitelných zpráv
Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy. Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello `unlockMessage` metodu hello **ServiceBusService** objektu. To bude způsobit toounlock Service Bus zprávu ve frontě hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.

Je také vypršení časového limitu přidružené zpráva uzamčená v rámci hello fronty, a pokud selže hello aplikace, které tooprocess hello zprávy před hello zámku vyprší časový limit (např. Pokud hello aplikace spadne), pak Service Bus se automatické odemknutí uvítací zprávu a nastavte jej k dispozici toobe přijetí.

V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello `deleteMessage` metoda je volána, pak uvítací zprávu bude víckrát toohello aplikace odešle znovu. To se často označuje jako *zpracování nejméně jednou*, který je každá zpráva se zpracuje alespoň jednou, ale v určitých situacích hello může doručit víckrát. Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy. To se často opírá hello **MessageId** vlastnost hello zprávy, která zůstane konstantní mezi pokusy o doručení.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o frontách, najdete v části hello následující prostředky.

* [Fronty, témata a odběry][Queues, topics, and subscriptions]
* [Azure SDK pro uzel] [ Azure SDK for Node] úložišti na Githubu
* [Středisko pro vývojáře Node.js](https://azure.microsoft.com/develop/nodejs/)

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com

[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Create and deploy a Node.js application tooan Azure Website]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-how-to-use-nodejs.md
[Service Bus quotas]: service-bus-quotas.md
