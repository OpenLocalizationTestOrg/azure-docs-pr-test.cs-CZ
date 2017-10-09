---
title: "aaaHow toouse úložiště Queue z Node.js | Microsoft Docs"
description: "Zjistěte, jak toouse hello fronty Azure service toocreate a odstranění fronty a vložení, získání a odstranění zprávy. Ukázky napsané v Node.js."
services: storage
documentationcenter: nodejs
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: a8a92db0-4333-43dd-a116-28b3147ea401
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 977e5994c0be1b5d71c60b7479698ccb694ab860
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-nodejs"></a>Jak toouse úložiště Queue z Node.js
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak tooperform běžné scénáře s využitím hello služby Microsoft Azure Queue. Ukázky Hello jsou zapsány pomocí rozhraní API Node.js hello. Hello pokryté scénáře zahrnují **vkládání**, **prohlížení**, **získávání**, a **odstraňování** fronty zpráv, a také  **vytváření a odstraňování front**.

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Vytvoření aplikace Node.js
Vytvoření prázdné aplikace Node.js. Pokyny k vytvoření aplikace Node.js najdete v tématu [vytvoření webové aplikace Node.js ve službě Azure App Service], [sestavení a nasazení tooan aplikace Node.js Azure Cloud Service] pomocí prostředí Windows PowerShell nebo [ Vytváření a nasazování tooAzure webové aplikace Node.js, pomocí Web Matrix].

## <a name="configure-your-application-tooaccess-storage"></a>Nakonfigurujte si aplikace tooAccess úložiště
toouse úložiště Azure, musíte hello sada SDK úložiště Azure pro platformu Node.js, která obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště hello.

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a>Použijte uzel balíček správce (NPM) tooobtain hello balíček
1. Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows) **Terminálové** (Mac), nebo **Bash** (Unix), přejděte toohello složku, kde jste vytvořili ukázkové aplikaci.
2. Typ **npm nainstalujte azure-storage** v příkazovém okně hello. Výstup z příkazu hello je podobné toohello následující ukázka.
 
    ```
    azure-storage@0.5.0 node_modules\azure-storage
    +-- extend@1.2.1
    +-- xmlbuilder@0.4.3
    +-- mime@1.2.11
    +-- node-uuid@1.4.3
    +-- validator@3.22.2
    +-- underscore@1.4.4
    +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
    +-- xml2js@0.2.7 (sax@0.5.2)
    +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
    ```

3. Můžete ručně spustit hello **ls** tooverify příkaz, **uzlu\_moduly** složka byla vytvořena. V této složce budou najde hello **azure-storage** balíček, který obsahuje hello knihovny, které potřebujete získat přístup k úložišti.

### <a name="import-hello-package"></a>Importovat balíček hello
Pomocí programu Poznámkový blok nebo jiném textovém editoru, přidejte následující toohello horní hello **server.js** souboru hello aplikace, kde chcete toouse úložiště:

```
var azure = require('azure-storage');
```

## <a name="setup-an-azure-storage-connection"></a>Nastavit připojení k úložišti Azure
modul Hello azure, bude číst hello proměnné prostředí AZURE\_úložiště\_účet a AZURE\_úložiště\_přístup\_klíč nebo AZURE\_úložiště\_připojení \_Řetězec pro požadované informace tooconnect tooyour účtu úložiště Azure. Pokud nejsou nastavené těchto proměnných prostředí, musíte zadat informace o účtu hello při volání metody **createQueueService**.

Příklad nastavení proměnných prostředí hello v hello [portálu Azure](https://portal.azure.com) web Azure, najdete v části [Node.js webové aplikace pomocí služby Azure Table hello].

## <a name="how-to-create-a-queue"></a>Postupy: Vytvoření fronty
Hello následující kód vytvoří **QueueService** objekt, který vám umožní toowork s fronty.

```
var queueSvc = azure.createQueueService();
```

Použití hello **createQueueIfNotExists** metoda, která vrátí zadanou frontu hello, pokud již existuje nebo vytvoří novou frontu se zadaným názvem hello, pokud ještě neexistuje.

```
queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
  if(!error){
    // Queue created or exists
  }
});
```

Pokud je vytvářena fronta hello, `result.created` hodnotu true. Pokud existuje hello fronty, `result.created` je false.

### <a name="filters"></a>Filtry
Volitelné filtrování operace může být použité toooperations provádí pomocí **QueueService**. Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu podpisem hello:

```
function handle (requestOptions, next)
```

Až to předzpracování na možnosti hello žádost, musí hello metoda toocall tlačítko Další"předání zpětné volání s hello následující podpis:

```
function (returnObject, finalCallback, next)
```

V této zpětného volání a po zpracování hello returnObject (hello odpověď ze serveru toohello požadavek hello), zpětné volání hello musí tooeither vyvolat další, pokud existuje toocontinue zpracování ostatní filtry nebo jednoduše vyvolat finalCallback jinak tooend až hello volání služby.

Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí hello Azure SDK pro Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**. vytvoří následující Hello **QueueService** objekt, který používá hello **ExponentialRetryPolicyFilter**:

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var queueSvc = azure.createQueueService().withFilter(retryOperations);
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Postupy: Vložit zprávu do fronty
tooinsert zprávu do fronty, použijte hello **createMessage** metoda vytvořte novou zprávu a přidat ji toohello fronty.

```
queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
  if(!error){
    // Message inserted
  }
});
```

## <a name="how-to-peek-at-hello-next-message"></a>Postupy: Zobrazení náhledu další zprávy hello
Můžete prohlížet zprávy hello v popředí hello fronty bez odebere ji z fronty hello pomocí volání hello **peekMessages** metoda. Ve výchozím nastavení **peekMessages** prohlédne do jedné zprávy.

```
queueSvc.peekMessages('myqueue', function(error, result, response){
  if(!error){
    // Message text is in messages[0].messageText
  }
});
```

Hello `result` obsahuje uvítací zprávu.

> [!NOTE]
> Pomocí **peekMessages** Pokud nejsou žádné zprávy ve frontě hello nebude vrátí chybu, ale žádné zprávy budou vráceny.
> 
> 

## <a name="how-to-dequeue-hello-next-message"></a>Postupy: Dequeue – hello další zprávy
Zpracování zprávy je dvoustupňový proces:

1. Dequeue – uvítací zprávu.
2. Hello zprávu odstraňte.

toodequeue zprávu, použijte **getMessages**. Díky tomu zprávy hello neviditelná ve frontě hello, takže je může zpracovat žádné další klienty. Po zpracování zprávy aplikace volání **deleteMessage** toodelete ji z fronty hello. Následující ukázka Hello získá zprávu, a pak ji odstraní:

```
queueSvc.getMessages('myqueue', function(error, result, response){
  if(!error){
    // Message text is in messages[0].messageText
    var message = result[0];
    queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
      if(!error){
        //message deleted
      }
    });
  }
});
```

> [!NOTE]
> Ve výchozím nastavení je zprávu skrytá pouze po dobu 30 sekund, po které je viditelné tooother klientů. Můžete zadat jinou hodnotu pomocí `options.visibilityTimeout` s **getMessages**.
> 
> [!NOTE]
> Pomocí **getMessages** Pokud nejsou žádné zprávy ve frontě hello nebude vrátí chybu, ale žádné zprávy budou vráceny.
> 
> 

## <a name="how-to-change-hello-contents-of-a-queued-message"></a>Postupy: Změna hello obsah zprávy ve frontě
Můžete změnit hello obsah zprávu na místě v hello fronty pomocí **updateMessage**. Následující ukázka Hello aktualizací hello text zprávy:

```
queueSvc.getMessages('myqueue', function(error, result, response){
  if(!error){
    // Got hello message
    var message = result[0];
    queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
      if(!error){
        // Message updated successfully
      }
    });
  }
});
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Postupy: Další možnosti pro vyřazení zprávy
Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby:

* `options.numOfMessages`-Načíst dávku zpráv (až too32.)
* `options.visibilityTimeout`-Nastavte časový limit neviditelnosti delší nebo kratší.

Hello následující příklad používá hello **getMessages** metoda tooget 15 zpráv v jednom volání. Pak se každá zpráva zpracuje pomocí pro smyčky. Nastaví taky hello neviditelnosti časový limit toofive minut pro všechny zprávy vrácená touto metodou.

```
queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
  if(!error){
    // Messages retreived
    for(var index in result){
      // text is available in result[index].messageText
      var message = result[index];
      queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, response){
        if(!error){
          // Message deleted
        }
      });
    }
  }
});
```

## <a name="how-to-get-hello-queue-length"></a>Postupy: Získání hello délka fronty
Hello **getQueueMetadata** vrací metadata o hello fronty, včetně hello přibližný počet zpráv čekajících ve frontě hello.

```
queueSvc.getQueueMetadata('myqueue', function(error, result, response){
  if(!error){
    // Queue length is available in result.approximateMessageCount
  }
});
```

## <a name="how-to-list-queues"></a>Postupy: Seznam fronty
Seznam front, použijte tooretrieve **listQueuesSegmented**. tooretrieve seznam filtrovat podle konkrétní předpony, použijte **listQueuesSegmentedWithPrefix**.

```
queueSvc.listQueuesSegmented(null, function(error, result, response){
  if(!error){
    // result.entries contains hello list of queues
  }
});
```

Pokud nelze vrátit všechny fronty, `result.continuationToken` lze použít jako první parametr hello **listQueuesSegmented** nebo hello druhý parametr **listQueuesSegmentedWithPrefix** tooretrieve více výsledků.

## <a name="how-to-delete-a-queue"></a>Postupy: Odstranění fronty
toodelete frontu a všechny zprávy hello obsažené v něm, volejte **deleteQueue** metoda na objekt fronty hello.

```
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

použít všechny zprávy z fronty bez odstranění, tooclear **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Postupy: práce s podpisy sdíleného přístupu
Podpisy sdíleného přístupu (SAS) jsou bezpečný tooprovide granulární přístup tooqueues bez zadání názvu účtu úložiště nebo klíče. SAS jsou často používané tooprovide omezený přístup tooyour fronty, například umožníte toosubmit zprávy mobilní aplikace.

Důvěryhodné aplikace, jako je Cloudová služba vygeneruje SAS pomocí hello **generateSharedAccessSignature** z hello **QueueService**a poskytuje ji tooan nedůvěryhodné nebo částečně důvěryhodné aplikace. Například mobilní aplikace. Hello SAS je generována pomocí zásad, která popisuje hello počáteční a koncové datum během které hello SAS je platný, a také hello držitel SAS úrovně toohello udělí přístup.

Hello následující ukázka vytvoří nové zásady sdíleného přístupu, který vám umožní hello SAS držitel tooadd zprávy toohello fronty a vyprší platnost 100 minut po času hello, je vytvořena.

```
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};

var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
var host = queueSvc.host;
```

Všimněte si, že informace o hostiteli hello musí být zadaná také, jako je povinný při hello SAS držitel pokusí tooaccess hello fronty.

Hello klientskou aplikaci, pak používá hello SAS s **QueueServiceWithSAS** tooperform operace u hello fronty. Následující ukázka Hello připojí toohello fronty a vytvoří zprávu.

```
var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
  if(!error){
    //message added
  }
});
```

Vzhledem k tomu, že hello SAS se vygeneroval s přidat přístup, pokud byly proveden pokus o tooread, aktualizace nebo odstranění zprávy, bude vrácena chyba.

### <a name="access-control-lists"></a>Seznamy řízení přístupu
Můžete taky zásady přístupu hello tooset seznamu řízení přístupu (ACL) pro SAS. To je užitečné, pokud chcete tooallow více klientů tooaccess hello fronty, ale poskytnutí zásad, jiný přístup pro každého klienta.

Seznam ACL je implementovaná pomocí pole zásady přístupu s ID spojené s každou zásadu. Následující ukázka Hello definuje dvě zásady; jeden pro "uživatel1" a jeden pro, uživatel2":

```
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

Následující příklad získá Hello hello aktuální seznam ACL pro **Moje_fronta**, pak přidá hello pomocí nové zásady **setQueueAcl**. Tento přístup umožňuje:

```
var extend = require('extend');
queueSvc.getQueueAcl('myqueue', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

Jednou hello byla nastavena seznamu ACL, potom můžete vytvořit na základě hello ID pro zásadu SAS. Hello následující příklad vytvoří nový SAS pro, uživatel2":

```
queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello fronty úložiště, postupujte podle těchto odkazů toolearn o složitějších úlohách úložiště.

* Navštivte hello [Blog týmu Azure Storage][Azure Storage Team Blog].
* Navštivte hello [sada SDK úložiště Azure pro uzel] [ Azure Storage SDK for Node] úložišti na Githubu.

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node
[using hello REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com
[vytvoření webové aplikace Node.js ve službě Azure App Service]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js webové aplikace pomocí služby Azure Table hello]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md


[sestavení a nasazení tooan aplikace Node.js Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[ Vytváření a nasazování tooAzure webové aplikace Node.js, pomocí Web Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
