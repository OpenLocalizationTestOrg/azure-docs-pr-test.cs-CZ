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
ms.openlocfilehash: 7e9778da4efa69f2e9d8fd480b9b6f5ace85951e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-nodejs"></a><span data-ttu-id="ad963-104">Jak toouse úložiště Queue z Node.js</span><span class="sxs-lookup"><span data-stu-id="ad963-104">How toouse Queue storage from Node.js</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a><span data-ttu-id="ad963-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="ad963-105">Overview</span></span>
<span data-ttu-id="ad963-106">Tento průvodce vám ukáže, jak tooperform běžné scénáře s využitím hello služby Microsoft Azure Queue.</span><span class="sxs-lookup"><span data-stu-id="ad963-106">This guide shows you how tooperform common scenarios using hello Microsoft Azure Queue service.</span></span> <span data-ttu-id="ad963-107">Ukázky Hello jsou zapsány pomocí rozhraní API Node.js hello.</span><span class="sxs-lookup"><span data-stu-id="ad963-107">hello samples are written using hello Node.js API.</span></span> <span data-ttu-id="ad963-108">Hello pokryté scénáře zahrnují **vkládání**, **prohlížení**, **získávání**, a **odstraňování** fronty zpráv, a také  **vytváření a odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="ad963-108">hello scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="ad963-109">Vytvoření aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="ad963-109">Create a Node.js Application</span></span>
<span data-ttu-id="ad963-110">Vytvoření prázdné aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="ad963-110">Create a blank Node.js application.</span></span> <span data-ttu-id="ad963-111">Pokyny k vytvoření aplikace Node.js najdete v tématu [vytvoření webové aplikace Node.js ve službě Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md), [sestavení a nasazení tooan aplikace Node.js Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) pomocí prostředí Windows PowerShell nebo [ Vytváření a nasazování tooAzure webové aplikace Node.js, pomocí Web Matrix](https://www.microsoft.com/web/webmatrix/).</span><span class="sxs-lookup"><span data-stu-id="ad963-111">For instructions creating a Node.js application, see [Create a Node.js web app in Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md), [Build and deploy a Node.js application tooan Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) using Windows PowerShell, or [Build and deploy a Node.js web app tooAzure using Web Matrix](https://www.microsoft.com/web/webmatrix/).</span></span>

## <a name="configure-your-application-tooaccess-storage"></a><span data-ttu-id="ad963-112">Nakonfigurujte si aplikace tooAccess úložiště</span><span class="sxs-lookup"><span data-stu-id="ad963-112">Configure Your Application tooAccess Storage</span></span>
<span data-ttu-id="ad963-113">toouse úložiště Azure, musíte hello sada SDK úložiště Azure pro platformu Node.js, která obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="ad963-113">toouse Azure storage, you need hello Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a><span data-ttu-id="ad963-114">Použijte uzel balíček správce (NPM) tooobtain hello balíček</span><span class="sxs-lookup"><span data-stu-id="ad963-114">Use Node Package Manager (NPM) tooobtain hello package</span></span>
1. <span data-ttu-id="ad963-115">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows) **Terminálové** (Mac), nebo **Bash** (Unix), přejděte toohello složku, kde jste vytvořili ukázkové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ad963-115">Use a command-line interface such as **PowerShell** (Windows,) **Terminal** (Mac,) or **Bash** (Unix), navigate toohello folder where you created your sample application.</span></span>
2. <span data-ttu-id="ad963-116">Typ **npm nainstalujte azure-storage** v příkazovém okně hello.</span><span class="sxs-lookup"><span data-stu-id="ad963-116">Type **npm install azure-storage** in hello command window.</span></span> <span data-ttu-id="ad963-117">Výstup z příkazu hello je podobné toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="ad963-117">Output from hello command is similar toohello following example.</span></span>
 
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

3. <span data-ttu-id="ad963-118">Můžete ručně spustit hello **ls** tooverify příkaz, **uzlu\_moduly** složka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="ad963-118">You can manually run hello **ls** command tooverify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="ad963-119">V této složce budou najde hello **azure-storage** balíček, který obsahuje hello knihovny, které potřebujete získat přístup k úložišti.</span><span class="sxs-lookup"><span data-stu-id="ad963-119">Inside that folder you will find hello **azure-storage** package, which contains hello libraries you need to access storage.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="ad963-120">Importovat balíček hello</span><span class="sxs-lookup"><span data-stu-id="ad963-120">Import hello package</span></span>
<span data-ttu-id="ad963-121">Pomocí programu Poznámkový blok nebo jiném textovém editoru, přidejte následující toohello horní hello **server.js** souboru hello aplikace, kde chcete toouse úložiště:</span><span class="sxs-lookup"><span data-stu-id="ad963-121">Using Notepad or another text editor, add hello following toohello top the **server.js** file of hello application where you intend toouse storage:</span></span>

```
var azure = require('azure-storage');
```

## <a name="setup-an-azure-storage-connection"></a><span data-ttu-id="ad963-122">Nastavit připojení k úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="ad963-122">Setup an Azure Storage Connection</span></span>
<span data-ttu-id="ad963-123">modul Hello azure, bude číst hello proměnné prostředí AZURE\_úložiště\_účet a AZURE\_úložiště\_přístup\_klíč nebo AZURE\_úložiště\_připojení \_Řetězec pro požadované informace tooconnect tooyour účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ad963-123">hello azure module will read hello environment variables AZURE\_STORAGE\_ACCOUNT and AZURE\_STORAGE\_ACCESS\_KEY, or AZURE\_STORAGE\_CONNECTION\_STRING for information required tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="ad963-124">Pokud nejsou nastavené těchto proměnných prostředí, musíte zadat informace o účtu hello při volání metody **createQueueService**.</span><span class="sxs-lookup"><span data-stu-id="ad963-124">If these environment variables are not set, you must specify hello account information when calling **createQueueService**.</span></span>

<span data-ttu-id="ad963-125">Příklad nastavení proměnných prostředí hello v hello [portálu Azure](https://portal.azure.com) web Azure, najdete v části [Node.js webové aplikace pomocí služby Azure Table hello].</span><span class="sxs-lookup"><span data-stu-id="ad963-125">For an example of setting hello environment variables in hello [Azure Portal](https://portal.azure.com) for an Azure Website, see [Node.js web app using hello Azure Table Service].</span></span>

## <a name="how-to-create-a-queue"></a><span data-ttu-id="ad963-126">Postupy: Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="ad963-126">How To: Create a Queue</span></span>
<span data-ttu-id="ad963-127">Hello následující kód vytvoří **QueueService** objekt, který vám umožní toowork s fronty.</span><span class="sxs-lookup"><span data-stu-id="ad963-127">hello following code creates a **QueueService** object, which enables you toowork with queues.</span></span>

```
var queueSvc = azure.createQueueService();
```

<span data-ttu-id="ad963-128">Použití hello **createQueueIfNotExists** metoda, která vrátí zadanou frontu hello, pokud již existuje nebo vytvoří novou frontu se zadaným názvem hello, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="ad963-128">Use hello **createQueueIfNotExists** method, which returns hello specified queue if it already exists or creates a new queue with hello specified name if it does not already exist.</span></span>

```
queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
  if(!error){
    // Queue created or exists
  }
});
```

<span data-ttu-id="ad963-129">Pokud je vytvářena fronta hello, `result.created` hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="ad963-129">If hello queue is created, `result.created` is true.</span></span> <span data-ttu-id="ad963-130">Pokud existuje hello fronty, `result.created` je false.</span><span class="sxs-lookup"><span data-stu-id="ad963-130">If hello queue exists, `result.created` is false.</span></span>

### <a name="filters"></a><span data-ttu-id="ad963-131">Filtry</span><span class="sxs-lookup"><span data-stu-id="ad963-131">Filters</span></span>
<span data-ttu-id="ad963-132">Volitelné filtrování operace může být použité toooperations provádí pomocí **QueueService**.</span><span class="sxs-lookup"><span data-stu-id="ad963-132">Optional filtering operations can be applied toooperations performed using **QueueService**.</span></span> <span data-ttu-id="ad963-133">Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu podpisem hello:</span><span class="sxs-lookup"><span data-stu-id="ad963-133">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with hello signature:</span></span>

```
function handle (requestOptions, next)
```

<span data-ttu-id="ad963-134">Až to předzpracování na možnosti hello žádost, musí hello metoda toocall tlačítko Další"předání zpětné volání s hello následující podpis:</span><span class="sxs-lookup"><span data-stu-id="ad963-134">After doing its preprocessing on hello request options, hello method needs toocall "next" passing a callback with hello following signature:</span></span>

```
function (returnObject, finalCallback, next)
```

<span data-ttu-id="ad963-135">V této zpětného volání a po zpracování hello returnObject (hello odpověď ze serveru toohello požadavek hello), zpětné volání hello musí tooeither vyvolat další, pokud existuje toocontinue zpracování ostatní filtry nebo jednoduše vyvolat finalCallback jinak tooend až hello volání služby.</span><span class="sxs-lookup"><span data-stu-id="ad963-135">In this callback, and after processing hello returnObject (hello response from hello request toohello server), hello callback needs tooeither invoke next if it exists toocontinue processing other filters or simply invoke finalCallback otherwise tooend up hello service invocation.</span></span>

<span data-ttu-id="ad963-136">Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí hello Azure SDK pro Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="ad963-136">Two filters that implement retry logic are included with hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="ad963-137">vytvoří následující Hello **QueueService** objekt, který používá hello **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="ad963-137">hello following creates a **QueueService** object that uses hello **ExponentialRetryPolicyFilter**:</span></span>

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var queueSvc = azure.createQueueService().withFilter(retryOperations);
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="ad963-138">Postupy: Vložit zprávu do fronty</span><span class="sxs-lookup"><span data-stu-id="ad963-138">How To: Insert a Message into a Queue</span></span>
<span data-ttu-id="ad963-139">tooinsert zprávu do fronty, použijte hello **createMessage** metoda vytvořte novou zprávu a přidat ji toohello fronty.</span><span class="sxs-lookup"><span data-stu-id="ad963-139">tooinsert a message into a queue, use hello **createMessage** method to create a new message and add it toohello queue.</span></span>

```
queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
  if(!error){
    // Message inserted
  }
});
```

## <a name="how-to-peek-at-hello-next-message"></a><span data-ttu-id="ad963-140">Postupy: Zobrazení náhledu další zprávy hello</span><span class="sxs-lookup"><span data-stu-id="ad963-140">How To: Peek at hello Next Message</span></span>
<span data-ttu-id="ad963-141">Můžete prohlížet zprávy hello v popředí hello fronty bez odebere ji z fronty hello pomocí volání hello **peekMessages** metoda.</span><span class="sxs-lookup"><span data-stu-id="ad963-141">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **peekMessages** method.</span></span> <span data-ttu-id="ad963-142">Ve výchozím nastavení **peekMessages** prohlédne do jedné zprávy.</span><span class="sxs-lookup"><span data-stu-id="ad963-142">By default, **peekMessages** peeks at a single message.</span></span>

```
queueSvc.peekMessages('myqueue', function(error, result, response){
  if(!error){
    // Message text is in messages[0].messageText
  }
});
```

<span data-ttu-id="ad963-143">Hello `result` obsahuje uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="ad963-143">hello `result` contains hello message.</span></span>

> [!NOTE]
> <span data-ttu-id="ad963-144">Pomocí **peekMessages** Pokud nejsou žádné zprávy ve frontě hello nebude vrátí chybu, ale žádné zprávy budou vráceny.</span><span class="sxs-lookup"><span data-stu-id="ad963-144">Using **peekMessages** when there are no messages in hello queue will not return an error, however no messages will be returned.</span></span>
> 
> 

## <a name="how-to-dequeue-hello-next-message"></a><span data-ttu-id="ad963-145">Postupy: Dequeue – hello další zprávy</span><span class="sxs-lookup"><span data-stu-id="ad963-145">How To: Dequeue hello Next Message</span></span>
<span data-ttu-id="ad963-146">Zpracování zprávy je dvoustupňový proces:</span><span class="sxs-lookup"><span data-stu-id="ad963-146">Processing a message is a two-stage process:</span></span>

1. <span data-ttu-id="ad963-147">Dequeue – uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="ad963-147">Dequeue hello message.</span></span>
2. <span data-ttu-id="ad963-148">Hello zprávu odstraňte.</span><span class="sxs-lookup"><span data-stu-id="ad963-148">Delete hello message.</span></span>

<span data-ttu-id="ad963-149">toodequeue zprávu, použijte **getMessages**.</span><span class="sxs-lookup"><span data-stu-id="ad963-149">toodequeue a message, use **getMessages**.</span></span> <span data-ttu-id="ad963-150">Díky tomu zprávy hello neviditelná ve frontě hello, takže je může zpracovat žádné další klienty.</span><span class="sxs-lookup"><span data-stu-id="ad963-150">This makes hello messages invisible in hello queue, so no other clients can process them.</span></span> <span data-ttu-id="ad963-151">Po zpracování zprávy aplikace volání **deleteMessage** toodelete ji z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="ad963-151">Once your application has processed a message, call **deleteMessage** toodelete it from hello queue.</span></span> <span data-ttu-id="ad963-152">Následující ukázka Hello získá zprávu, a pak ji odstraní:</span><span class="sxs-lookup"><span data-stu-id="ad963-152">hello following example gets a message, then deletes it:</span></span>

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
> <span data-ttu-id="ad963-153">Ve výchozím nastavení je zprávu skrytá pouze po dobu 30 sekund, po které je viditelné tooother klientů.</span><span class="sxs-lookup"><span data-stu-id="ad963-153">By default, a message is only hidden for 30 seconds, after which it is visible tooother clients.</span></span> <span data-ttu-id="ad963-154">Můžete zadat jinou hodnotu pomocí `options.visibilityTimeout` s **getMessages**.</span><span class="sxs-lookup"><span data-stu-id="ad963-154">You can specify a different value by using `options.visibilityTimeout` with **getMessages**.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="ad963-155">Pomocí **getMessages** Pokud nejsou žádné zprávy ve frontě hello nebude vrátí chybu, ale žádné zprávy budou vráceny.</span><span class="sxs-lookup"><span data-stu-id="ad963-155">Using **getMessages** when there are no messages in hello queue will not return an error, however no messages will be returned.</span></span>
> 
> 

## <a name="how-to-change-hello-contents-of-a-queued-message"></a><span data-ttu-id="ad963-156">Postupy: Změna hello obsah zprávy ve frontě</span><span class="sxs-lookup"><span data-stu-id="ad963-156">How To: Change hello Contents of a Queued Message</span></span>
<span data-ttu-id="ad963-157">Můžete změnit hello obsah zprávu na místě v hello fronty pomocí **updateMessage**.</span><span class="sxs-lookup"><span data-stu-id="ad963-157">You can change hello contents of a message in-place in hello queue using **updateMessage**.</span></span> <span data-ttu-id="ad963-158">Následující ukázka Hello aktualizací hello text zprávy:</span><span class="sxs-lookup"><span data-stu-id="ad963-158">hello following example updates hello text of a message:</span></span>

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

## <a name="how-to-additional-options-for-dequeuing-messages"></a><span data-ttu-id="ad963-159">Postupy: Další možnosti pro vyřazení zprávy</span><span class="sxs-lookup"><span data-stu-id="ad963-159">How To: Additional Options for Dequeuing Messages</span></span>
<span data-ttu-id="ad963-160">Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="ad963-160">There are two ways you can customize message retrieval from a queue:</span></span>

* <span data-ttu-id="ad963-161">`options.numOfMessages`-Načíst dávku zpráv (až too32.)</span><span class="sxs-lookup"><span data-stu-id="ad963-161">`options.numOfMessages` - Retrieve a batch of messages (up too32.)</span></span>
* <span data-ttu-id="ad963-162">`options.visibilityTimeout`-Nastavte časový limit neviditelnosti delší nebo kratší.</span><span class="sxs-lookup"><span data-stu-id="ad963-162">`options.visibilityTimeout` - Set a longer or shorter invisibility timeout.</span></span>

<span data-ttu-id="ad963-163">Hello následující příklad používá hello **getMessages** metoda tooget 15 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="ad963-163">hello following example uses hello **getMessages** method tooget 15 messages in one call.</span></span> <span data-ttu-id="ad963-164">Pak se každá zpráva zpracuje pomocí pro smyčky.</span><span class="sxs-lookup"><span data-stu-id="ad963-164">Then it processes each message using a for loop.</span></span> <span data-ttu-id="ad963-165">Nastaví taky hello neviditelnosti časový limit toofive minut pro všechny zprávy vrácená touto metodou.</span><span class="sxs-lookup"><span data-stu-id="ad963-165">It also sets hello invisibility timeout toofive minutes for all messages returned by this method.</span></span>

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

## <a name="how-to-get-hello-queue-length"></a><span data-ttu-id="ad963-166">Postupy: Získání hello délka fronty</span><span class="sxs-lookup"><span data-stu-id="ad963-166">How To: Get hello Queue Length</span></span>
<span data-ttu-id="ad963-167">Hello **getQueueMetadata** vrací metadata o hello fronty, včetně hello přibližný počet zpráv čekajících ve frontě hello.</span><span class="sxs-lookup"><span data-stu-id="ad963-167">hello **getQueueMetadata** returns metadata about hello queue, including hello approximate number of messages waiting in hello queue.</span></span>

```
queueSvc.getQueueMetadata('myqueue', function(error, result, response){
  if(!error){
    // Queue length is available in result.approximateMessageCount
  }
});
```

## <a name="how-to-list-queues"></a><span data-ttu-id="ad963-168">Postupy: Seznam fronty</span><span class="sxs-lookup"><span data-stu-id="ad963-168">How To: List Queues</span></span>
<span data-ttu-id="ad963-169">Seznam front, použijte tooretrieve **listQueuesSegmented**.</span><span class="sxs-lookup"><span data-stu-id="ad963-169">tooretrieve a list of queues, use **listQueuesSegmented**.</span></span> <span data-ttu-id="ad963-170">tooretrieve seznam filtrovat podle konkrétní předpony, použijte **listQueuesSegmentedWithPrefix**.</span><span class="sxs-lookup"><span data-stu-id="ad963-170">tooretrieve a list filtered by a specific prefix, use **listQueuesSegmentedWithPrefix**.</span></span>

```
queueSvc.listQueuesSegmented(null, function(error, result, response){
  if(!error){
    // result.entries contains hello list of queues
  }
});
```

<span data-ttu-id="ad963-171">Pokud nelze vrátit všechny fronty, `result.continuationToken` lze použít jako první parametr hello **listQueuesSegmented** nebo hello druhý parametr **listQueuesSegmentedWithPrefix** tooretrieve více výsledků.</span><span class="sxs-lookup"><span data-stu-id="ad963-171">If all queues cannot be returned, `result.continuationToken` can be used as hello first parameter of **listQueuesSegmented** or hello second parameter of **listQueuesSegmentedWithPrefix** tooretrieve more results.</span></span>

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="ad963-172">Postupy: Odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="ad963-172">How To: Delete a Queue</span></span>
<span data-ttu-id="ad963-173">toodelete frontu a všechny zprávy hello obsažené v něm, volejte **deleteQueue** metoda na objekt fronty hello.</span><span class="sxs-lookup"><span data-stu-id="ad963-173">toodelete a queue and all hello messages contained in it, call the **deleteQueue** method on hello queue object.</span></span>

```
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

<span data-ttu-id="ad963-174">použít všechny zprávy z fronty bez odstranění, tooclear **clearMessages**.</span><span class="sxs-lookup"><span data-stu-id="ad963-174">tooclear all messages from a queue without deleting it, use **clearMessages**.</span></span>

## <a name="how-to-work-with-shared-access-signatures"></a><span data-ttu-id="ad963-175">Postupy: práce s podpisy sdíleného přístupu</span><span class="sxs-lookup"><span data-stu-id="ad963-175">How to: Work with Shared Access Signatures</span></span>
<span data-ttu-id="ad963-176">Podpisy sdíleného přístupu (SAS) jsou bezpečný tooprovide granulární přístup tooqueues bez zadání názvu účtu úložiště nebo klíče.</span><span class="sxs-lookup"><span data-stu-id="ad963-176">Shared Access Signatures (SAS) are a secure way tooprovide granular access tooqueues without providing your storage account name or keys.</span></span> <span data-ttu-id="ad963-177">SAS jsou často používané tooprovide omezený přístup tooyour fronty, například umožníte toosubmit zprávy mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="ad963-177">SAS are often used tooprovide limited access tooyour queues, such as allowing a mobile app toosubmit messages.</span></span>

<span data-ttu-id="ad963-178">Důvěryhodné aplikace, jako je Cloudová služba vygeneruje SAS pomocí hello **generateSharedAccessSignature** z hello **QueueService**a poskytuje ji tooan nedůvěryhodné nebo částečně důvěryhodné aplikace.</span><span class="sxs-lookup"><span data-stu-id="ad963-178">A trusted application such as a cloud-based service generates a SAS using hello **generateSharedAccessSignature** of hello **QueueService**, and provides it tooan untrusted or semi-trusted application.</span></span> <span data-ttu-id="ad963-179">Například mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="ad963-179">For example, a mobile app.</span></span> <span data-ttu-id="ad963-180">Hello SAS je generována pomocí zásad, která popisuje hello počáteční a koncové datum během které hello SAS je platný, a také hello držitel SAS úrovně toohello udělí přístup.</span><span class="sxs-lookup"><span data-stu-id="ad963-180">hello SAS is generated using a policy, which describes hello start and end dates during which hello SAS is valid, as well as hello access level granted toohello SAS holder.</span></span>

<span data-ttu-id="ad963-181">Hello následující ukázka vytvoří nové zásady sdíleného přístupu, který vám umožní hello SAS držitel tooadd zprávy toohello fronty a vyprší platnost 100 minut po času hello, je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="ad963-181">hello following example generates a new shared access policy that will allow hello SAS holder tooadd messages toohello queue, and expires 100 minutes after hello time it is created.</span></span>

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

<span data-ttu-id="ad963-182">Všimněte si, že informace o hostiteli hello musí být zadaná také, jako je povinný při hello SAS držitel pokusí tooaccess hello fronty.</span><span class="sxs-lookup"><span data-stu-id="ad963-182">Note that hello host information must be provided also, as it is required when hello SAS holder attempts tooaccess hello queue.</span></span>

<span data-ttu-id="ad963-183">Hello klientskou aplikaci, pak používá hello SAS s **QueueServiceWithSAS** tooperform operace u hello fronty.</span><span class="sxs-lookup"><span data-stu-id="ad963-183">hello client application then uses hello SAS with **QueueServiceWithSAS** tooperform operations against hello queue.</span></span> <span data-ttu-id="ad963-184">Následující ukázka Hello připojí toohello fronty a vytvoří zprávu.</span><span class="sxs-lookup"><span data-stu-id="ad963-184">hello following example connects toohello queue and creates a message.</span></span>

```
var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
  if(!error){
    //message added
  }
});
```

<span data-ttu-id="ad963-185">Vzhledem k tomu, že hello SAS se vygeneroval s přidat přístup, pokud byly proveden pokus o tooread, aktualizace nebo odstranění zprávy, bude vrácena chyba.</span><span class="sxs-lookup"><span data-stu-id="ad963-185">Since hello SAS was generated with add access, if an attempt were made tooread, update or delete messages, an error would be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="ad963-186">Seznamy řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="ad963-186">Access control lists</span></span>
<span data-ttu-id="ad963-187">Můžete taky zásady přístupu hello tooset seznamu řízení přístupu (ACL) pro SAS.</span><span class="sxs-lookup"><span data-stu-id="ad963-187">You can also use an Access Control List (ACL) tooset hello access policy for a SAS.</span></span> <span data-ttu-id="ad963-188">To je užitečné, pokud chcete tooallow více klientů tooaccess hello fronty, ale poskytnutí zásad, jiný přístup pro každého klienta.</span><span class="sxs-lookup"><span data-stu-id="ad963-188">This is useful if you wish tooallow multiple clients tooaccess hello queue, but provide different access policies for each client.</span></span>

<span data-ttu-id="ad963-189">Seznam ACL je implementovaná pomocí pole zásady přístupu s ID spojené s každou zásadu.</span><span class="sxs-lookup"><span data-stu-id="ad963-189">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="ad963-190">Následující ukázka Hello definuje dvě zásady; jeden pro "uživatel1" a jeden pro, uživatel2":</span><span class="sxs-lookup"><span data-stu-id="ad963-190">hello  following example defines two policies; one for 'user1' and one for 'user2':</span></span>

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

<span data-ttu-id="ad963-191">Následující příklad získá Hello hello aktuální seznam ACL pro **Moje_fronta**, pak přidá hello pomocí nové zásady **setQueueAcl**.</span><span class="sxs-lookup"><span data-stu-id="ad963-191">hello following example gets hello current ACL for **myqueue**, then adds hello new policies using **setQueueAcl**.</span></span> <span data-ttu-id="ad963-192">Tento přístup umožňuje:</span><span class="sxs-lookup"><span data-stu-id="ad963-192">This approach allows:</span></span>

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

<span data-ttu-id="ad963-193">Jednou hello byla nastavena seznamu ACL, potom můžete vytvořit na základě hello ID pro zásadu SAS.</span><span class="sxs-lookup"><span data-stu-id="ad963-193">Once hello ACL has been set, you can then create a SAS based on hello ID for a policy.</span></span> <span data-ttu-id="ad963-194">Hello následující příklad vytvoří nový SAS pro, uživatel2":</span><span class="sxs-lookup"><span data-stu-id="ad963-194">hello following example creates a new SAS for 'user2':</span></span>

```
queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="ad963-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ad963-195">Next Steps</span></span>
<span data-ttu-id="ad963-196">Teď, když jste se naučili základy hello fronty úložiště, postupujte podle těchto odkazů toolearn o složitějších úlohách úložiště.</span><span class="sxs-lookup"><span data-stu-id="ad963-196">Now that you've learned hello basics of queue storage, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="ad963-197">Navštivte hello [Azure Storage Team Blog] [Blog týmu Azure Storage].</span><span class="sxs-lookup"><span data-stu-id="ad963-197">Visit hello [Azure Storage Team Blog][Azure Storage Team Blog].</span></span>
* <span data-ttu-id="ad963-198">Navštivte hello [sada SDK úložiště Azure pro uzel] [ Azure Storage SDK for Node] úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="ad963-198">Visit hello [Azure Storage SDK for Node][Azure Storage SDK for Node] repository on GitHub.</span></span>

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node
[using hello REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com<span data-ttu-id="ad963-199">
[Vytvoření webové aplikace Node.js ve službě Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md) </span><span class="sxs-lookup"><span data-stu-id="ad963-199">
[Create a Node.js web app in Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md) </span></span>  
<span data-ttu-id="ad963-200">[Webové aplikace Node.js pomocí hello služby Azure Table](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)
</span><span class="sxs-lookup"><span data-stu-id="ad963-200">[Node.js web app using hello Azure Table Service](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)
</span></span>  


<span data-ttu-id="ad963-201">[Sestavení a nasazení tooan aplikace Node.js Cloudová služba Azure](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) </span><span class="sxs-lookup"><span data-stu-id="ad963-201">[Build and deploy a Node.js application tooan Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) </span></span>  
<span data-ttu-id="ad963-202">[Blog týmu azure Storage]: http://blogs.msdn.com/b/windowsazurestorage/ [sestavení a nasazení tooAzure webové aplikace Node.js, pomocí Web Matrix]: https://www.microsoft.com/web/webmatrix/</span><span class="sxs-lookup"><span data-stu-id="ad963-202">[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/ [Build and deploy a Node.js web app tooAzure using Web Matrix]: https://www.microsoft.com/web/webmatrix/</span></span>   
