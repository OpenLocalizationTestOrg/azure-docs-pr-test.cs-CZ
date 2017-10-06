---
title: "aaaHow toouse Azure Service Bus témata a odběry s Node.js | Microsoft Docs"
description: "Zjistěte, jak toouse Service Bus témat a odběrů v Azure z aplikace Node.js."
services: service-bus-messaging
documentationcenter: nodejs
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b9f5db85-7b6c-4cc7-bd2c-bd3087c99875
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: e8f6e7ad6ed16d844c408337ac9e50f990e3fafd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-nodejs"></a>Jak tooUse Service Bus témata a odběry s Node.js

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tato příručka popisuje, jak toouse Service Bus témata a odběry z aplikací Node.js. Hello pokryté scénáře zahrnují **vytváření témat a odběrů**, **vytváření filtrů odběrů**, **odesílání zpráv** tooa tématu **přijetí zprávy z odběru**, a **odstranění témat a odběrů**. Další informace o tématech a odběrech najdete v tématu hello [další kroky](#next-steps) části.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Vytvoření aplikace Node.js
Vytvoření prázdné aplikace Node.js. Pokyny pro vytvoření aplikace Node.js, naleznete v části [vytvořit a nasadit tooan aplikace Node.js webu Azure], [Node.js Cloudová služba] [ Node.js Cloud Service] pomocí systému Windows Prostředí PowerShell nebo na webovém serveru pomocí služby WebMatrix.

## <a name="configure-your-application-toouse-service-bus"></a>Konfigurace vaší aplikace toouse Service Bus
toouse Service Bus, stáhněte si balíček Node.js Azure hello. Tento balíček obsahuje sadu knihoven, které komunikují s hello služby REST pro Service Bus.

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a>Použijte uzel balíček správce (NPM) tooobtain hello balíček
1. Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows) **Terminálové** (Mac), nebo **Bash** (Unix), přejděte toohello složku, kde jste vytvořili ukázkové aplikaci.
2. Typ **npm nainstalujte azure** v hello příkazové okno, které by měl způsobit hello následující výstup:

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
3. Můžete ručně spustit hello **ls** tooverify příkaz, **uzlu\_moduly** složka byla vytvořena. V této složce najít **azure** balíček, který obsahuje hello knihovny, je nutné tooaccess témat sběrnice Service Bus.

### <a name="import-hello-module"></a>Import modulu hello
Pomocí programu Poznámkový blok nebo jiném textovém editoru, přidejte následující toohello horní části hello hello **server.js** souboru aplikace hello:

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Nastavení připojení služby Service Bus
Hello Azure modul čte proměnné prostředí hello `AZURE_SERVICEBUS_NAMESPACE` a `AZURE_SERVICEBUS_ACCESS_KEY` informace požadované tooconnect tooService sběrnice. Pokud nejsou nastavené těchto proměnných prostředí, musíte zadat informace o účtu hello při volání metody `createServiceBusService`.

Příklad nastavení hello proměnných prostředí pro cloudové služby Azure, naleznete v části [Node.js Cloudová služba se úložiště][Node.js Cloud Service with Storage].

Příklad nastavení hello proměnných prostředí pro web Azure, naleznete v části [webovou aplikaci Node.js s úložištěm][Node.js Web Application with Storage].

## <a name="create-a-topic"></a>Vytvoření tématu
Hello **ServiceBusService** objekt vám umožní toowork s tématy. Následující kód vytvoří **ServiceBusService** objektu. Přidejte do horní části hello **server.js** souboru po hello příkaz tooimport hello azure modul:

```javascript
var serviceBusService = azure.createServiceBusService();
```

Při volání `createTopicIfNotExists` na hello **ServiceBusService** objektu hello určena, bude vrácen tématu (pokud existuje), nebo se vytvoří nové téma se zadaným názvem hello. Hello následující kód používá `createTopicIfNotExists` toocreate nebo připojení toohello téma s názvem `MyTopic`:

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

Hello `createServiceBusService` metoda také podporuje další možnosti, které umožňují toooverride výchozí téma nastavení jako hodnota time to live zpráva nebo téma maximální velikost. Hello následující příklad ilustruje hello tématu maximální velikost too5GB s časem toolive 1 minuta:

```javascript
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filtry
Volitelné filtrování operace může být použité toooperations provádí pomocí **ServiceBusService**. Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu podpisem hello:

```javascript
function handle (requestOptions, next)
```

Po provedení předzpracování na možnosti hello požadavku, hello volání metod `next`, předání zpětné volání s hello následující podpis:

```javascript
function (returnObject, finalCallback, next)
```

V této zpětného volání a po zpracování hello `returnObject` (hello odpovědi ze serveru toohello požadavek hello), zpětné volání hello musí tooeither vyvolat další, pokud existuje toocontinue zpracování ostatní filtry nebo vyvolat `finalCallback` , jinak hodnota tooend hello volání služby.

Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí hello Azure SDK pro Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**. vytvoří následující Hello **ServiceBusService** objekt, který používá hello **ExponentialRetryPolicyFilter**:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a>Vytvořte odběr
Odběry témat taky jsou vytvořeny pomocí hello **ServiceBusService** objektu. Odběry mají názvy a můžou mít volitelné filtry, které omezuje hello sadu zpráv doručených virtuální fronty odběru toohello.

> [!NOTE]
> Odběry jsou trvalé a bude pokračovat tooexist, dokud nebudou buď jejich nebo hello téma jejich jsou přidruženy, budou odstraněny. Pokud vaše aplikace obsahuje logiku toocreate předplatné, musí nejprve zkontrolujte, zda hello předplatné už existuje s použitím `getSubscription` metoda.
>
>

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a>Vytvoření odběru s filtrem (MatchAll) výchozí hello
Hello **MatchAll** filtr je hello výchozí filtr, který se používá v případě, že při vytvoření nového předplatného je zadán žádný filtr. Když hello **MatchAll** filtr se používá, všechny zprávy publikované toohello tématu ukládány do virtuální fronty odběru. Hello následující příklad vytvoří odběr s názvem "AllMessages" a používá hello výchozí **MatchAll** filtru.

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Vytvoření odběru s filtry
Můžete taky vytvořit filtry, které vám umožňují tooscope příjem zpráv odeslaných tooa tématu by měla objeví v konkrétním odběru tématu.

je technologie Hello nejflexibilnější filtr, který odběry podporují **SqlFilter**, který implementuje je podmnožinou SQL92. Filtry SQL pracují hello vlastnosti hello zpráv, které jsou publikované toohello tématu. Další podrobnosti o hello výrazy, které lze použít s filtrem SQL, projděte si hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntaxe.

Filtry lze přidat předplatné tooa pomocí hello `createRule` metoda hello **ServiceBusService** objektu. Tato metoda umožňuje přidat nové předplatné tooan existující filtry.

> [!NOTE]
> Protože se automaticky použije výchozí filtr hello tooall nových předplatných, je nutné nejprve odebrat hello výchozí filtr nebo **MatchAll** přepíše ostatní filtry, můžete určit. Hello výchozí pravidla můžete odebrat pomocí hello `deleteRule` metodu **ServiceBusService** objektu.
>
>

Hello následující příklad vytvoří odběr s názvem `HighMessages` s **SqlFilter** který vybere jen zprávy, které mají vlastní `messagenumber` vlastnost větší než 3:

```javascript
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'HighMessages',
            'HighMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Podobně hello následující příklad vytvoří odběr s názvem `LowMessages` s **SqlFilter** který vybere jen zprávy, které mají `messagenumber` vlastnost menší než nebo rovna too3:

```javascript
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'LowMessages',
            'LowMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Když je nyní odeslána zpráva příliš`MyTopic`, bude vždy doručit do příjemci toohello `AllMessages` odběr tématu a odběru selektivně tooreceivers toohello `HighMessages` a `LowMessages` odběry témat (v závislosti na obsahu zprávy hello).

## <a name="how-toosend-messages-tooa-topic"></a>Jak toosend zprávy tooa tématu
toosend tématu Service Bus zprávu tooa, musí vaše aplikace používat `sendTopicMessage` metoda hello **ServiceBusService** objektu.
Témata sběrnice tooService jsou odesílány zprávy **BrokeredMessage** objekty.
**BrokeredMessage** objekty mají sadu standardních vlastností (jako například `Label` a `TimeToLive`), slovník, který je použité toohold vlastní vlastnosti specifické pro aplikace a tělo dat řetězce. Aplikace může nastavit hello textu hello zprávy pomocí předání řetězcovou hodnotu hello `sendTopicMessage` a potřebné standardní vlastnosti vyplní výchozí hodnoty.

Hello následující příklad ukazuje, jak toosend pět zkušebních zpráv do `MyTopic`. Všimněte si, že hello `messagenumber` hodnotu vlastnosti každé zprávy se liší na hello iteraci smyčky hello (to určí, které odběry ji přijmou):

```javascript
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Témata Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md). Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB. Neexistuje žádné omezení na hello počet zpráv držených v tématu, ale není na hello celková velikost hello zpráv držených v tématu. Velikost tématu se definuje při vytvoření, maximální limit je 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Příjem zpráv z odběru
Přijímání zpráv z odběru pomocí `receiveSubscriptionMessage` metodu hello **ServiceBusService** objektu. Ve výchozím nastavení jsou odstraněny zprávy z předplatného hello jsou pro čtení; ale můžete číst (funkce Náhled) a uzamčení uvítací zprávu bez odstranění z předplatného hello podle nastavení volitelný parametr hello `isPeekLock` příliš**true**.

výchozí chování Hello čtení a odstraňování uvítací zprávu jako součást operace příjmu je nejjednodušší model hello a funguje nejlépe pro scénáře, ve kterých aplikace může tolerovat hello události selhání se zpráva nezpracuje. toounderstand, vezměte v úvahu scénář, ve kterém hello problémy příjemce přijímání požadavků a pak dojde k chybě před zpracováním ho. Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.

Pokud hello `isPeekLock` parametr je nastaven příliš**true**, přijímat hello stane dvoufázového operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv. Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace.
Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze přijetí hello volání **deleteMessage** metoda a poskytování toobe zpráv Odstranit jako parametr. Hello **deleteMessage** metoda bude označit uvítací zprávu jako spotřebovávanou a jeho odebrání ze hello předplatného.

Hello následující příklad ukazuje, jak můžete obdržet zprávy a zpracování pomocí `receiveSubscriptionMessage`. Hello příklad nejprve obdrží a odstraní zprávu z hello 'LowMessages' předplatného a pak obdrží zprávu z hello 'HighMessages' předplatnému pomocí `isPeekLock` nastavit tootrue. Pak odstraní hello zprávu pomocí `deleteMessage`:

```javascript
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Jak toohandle aplikace spadne a nečitelných zpráv
Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy. Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello `unlockMessage` metodu **ServiceBusService** objektu. To bude způsobit toounlock sběrnice zpráv v rámci předplatného hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.

Je také vypršení časového limitu zpráva uzamčená v odběru, a pokud aplikace hello selže tooprocess uvítací zprávu před hello zámku vyprší časový limit (například pokud hello aplikace spadne), pak uvítací zprávu odemkne Service Bus automaticky a je k dispozici toobe přijetí.

V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello `deleteMessage` metoda je volána, pak uvítací zprávu bude víckrát toohello aplikace odešle znovu. To se často označuje jako *zpracování nejméně jednou*, který je každá zpráva se zpracuje alespoň jednou, ale v určitých situacích hello může doručit víckrát. Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy. To se často opírá **MessageId** vlastnost hello zprávy, která zůstane konstantní mezi pokusy o doručení.

## <a name="delete-topics-and-subscriptions"></a>Odstranění témat a odběrů
Témata a odběry jsou trvalé a musí být explicitně odstranit buď prostřednictvím hello [portál Azure] [ Azure portal] nebo prostřednictvím kódu programu.
Hello následující příklad ukazuje, jak s názvem toodelete hello tématu `MyTopic`:

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

Odstraní téma také odstraní všechny odběry, které jsou registrovány hello tématu. Odběry se taky dají odstranit samostatně. Následující příklad ukazuje, jak toodelete předplatné s názvem `HighMessages` z hello `MyTopic` tématu:

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello témat Service Bus, postupujte podle těchto odkazů toolearn Další.

* V tématu [fronty, témata a odběry][Queues, topics, and subscriptions].
* Reference k rozhraní API pro [SqlFilter][SqlFilter]
* Navštivte hello [Azure SDK pro uzel] [ Azure SDK for Node] úložišti na Githubu.

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[vytvořit a nasadit tooan aplikace Node.js webu Azure]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
