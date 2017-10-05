---
title: "Používání úložiště Queue z Node.js | Microsoft Docs"
description: "Naučte se používat službu front Azure k vytváření a odstraňování front a vložit, získání a odstranění zprávy. Ukázky napsané v Node.js."
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
ms.openlocfilehash: e30297bd0cc65105c92d6428035d2e6c156448af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-queue-storage-from-nodejs"></a><span data-ttu-id="7e3f9-104">Používání úložiště Queue z Node.js</span><span class="sxs-lookup"><span data-stu-id="7e3f9-104">How to use Queue storage from Node.js</span></span>
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a><span data-ttu-id="7e3f9-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="7e3f9-105">Overview</span></span>
<span data-ttu-id="7e3f9-106">Tento průvodce vám ukáže, jak provádět běžné scénáře pomocí služby Microsoft Azure Queue.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-106">This guide shows you how to perform common scenarios using the Microsoft Azure Queue service.</span></span> <span data-ttu-id="7e3f9-107">Ukázky jsou zapsány pomocí rozhraní API Node.js.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-107">The samples are written using the Node.js API.</span></span> <span data-ttu-id="7e3f9-108">Pokryté scénáře zahrnují **vkládání**, **prohlížení**, **získávání**, a **odstraňování** fronty zpráv, a také **vytváření a odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-108">The scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="7e3f9-109">Vytvoření aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="7e3f9-109">Create a Node.js Application</span></span>
<span data-ttu-id="7e3f9-110">Vytvoření prázdné aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-110">Create a blank Node.js application.</span></span> <span data-ttu-id="7e3f9-111">Pokyny k vytvoření aplikace Node.js najdete v tématu [vytvoření webové aplikace Node.js ve službě Azure App Service], [sestavení a nasazení aplikace Node.js ve službě Azure Cloud Service] pomocí prostředí Windows PowerShell nebo [sestavení a nasazení webové aplikace Node.js do Azure pomocí Web Matrix].</span><span class="sxs-lookup"><span data-stu-id="7e3f9-111">For instructions creating a Node.js application, see [Create a Node.js web app in Azure App Service], [Build and deploy a Node.js application to an Azure Cloud Service] using Windows PowerShell, or [Build and deploy a Node.js web app to Azure using Web Matrix].</span></span>

## <a name="configure-your-application-to-access-storage"></a><span data-ttu-id="7e3f9-112">Konfigurace aplikace pro přístup k úložišti</span><span class="sxs-lookup"><span data-stu-id="7e3f9-112">Configure Your Application to Access Storage</span></span>
<span data-ttu-id="7e3f9-113">Pokud chcete používat úložiště Azure, musíte sady SDK úložiště Azure pro platformu Node.js, která obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-113">To use Azure storage, you need the Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with the storage REST services.</span></span>

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a><span data-ttu-id="7e3f9-114">Získat balíček pomocí uzlu balíček správce (NPM)</span><span class="sxs-lookup"><span data-stu-id="7e3f9-114">Use Node Package Manager (NPM) to obtain the package</span></span>
1. <span data-ttu-id="7e3f9-115">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows) **Terminálové** (Mac), nebo **Bash** (Unix), přejděte do složky, které jste vytvořili ukázkové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-115">Use a command-line interface such as **PowerShell** (Windows,) **Terminal** (Mac,) or **Bash** (Unix), navigate to the folder where you created your sample application.</span></span>
2. <span data-ttu-id="7e3f9-116">Typ **npm nainstalujte azure-storage** v příkazovém okně.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-116">Type **npm install azure-storage** in the command window.</span></span> <span data-ttu-id="7e3f9-117">Výstup z tohoto příkazu se podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-117">Output from the command is similar to the following example.</span></span>
 
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

3. <span data-ttu-id="7e3f9-118">Můžete ručně spustit **ls** příkazu ověřte, že **uzlu\_moduly** složka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-118">You can manually run the **ls** command to verify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="7e3f9-119">Uvnitř této složky najdete **azure-storage** balíček, který obsahuje knihovny, je třeba získat přístup k úložišti.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-119">Inside that folder you will find the **azure-storage** package, which contains the libraries you need to access storage.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="7e3f9-120">Import balíčku</span><span class="sxs-lookup"><span data-stu-id="7e3f9-120">Import the package</span></span>
<span data-ttu-id="7e3f9-121">Pomocí programu Poznámkový blok nebo jiném textovém editoru, přidejte následující horní **server.js** souboru aplikace, které máte v úmyslu používat úložiště:</span><span class="sxs-lookup"><span data-stu-id="7e3f9-121">Using Notepad or another text editor, add the following to the top the **server.js** file of the application where you intend to use storage:</span></span>

```
var azure = require('azure-storage');
```

## <a name="setup-an-azure-storage-connection"></a><span data-ttu-id="7e3f9-122">Nastavit připojení k úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="7e3f9-122">Setup an Azure Storage Connection</span></span>
<span data-ttu-id="7e3f9-123">Modul azure, bude číst proměnné prostředí AZURE\_úložiště\_účet a AZURE\_úložiště\_přístup\_klíč nebo AZURE\_úložiště\_připojení\_řetězec informace požadované pro připojení k účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-123">The azure module will read the environment variables AZURE\_STORAGE\_ACCOUNT and AZURE\_STORAGE\_ACCESS\_KEY, or AZURE\_STORAGE\_CONNECTION\_STRING for information required to connect to your Azure storage account.</span></span> <span data-ttu-id="7e3f9-124">Pokud nejsou nastavené těchto proměnných prostředí, musíte zadat informace o účtu při volání metody **createQueueService**.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-124">If these environment variables are not set, you must specify the account information when calling **createQueueService**.</span></span>

<span data-ttu-id="7e3f9-125">Příklad nastavení proměnných prostředí [portálu Azure](https://portal.azure.com) web Azure, najdete v části [webové aplikace Node.js pomocí služby Azure Table].</span><span class="sxs-lookup"><span data-stu-id="7e3f9-125">For an example of setting the environment variables in the [Azure Portal](https://portal.azure.com) for an Azure Website, see [Node.js web app using the Azure Table Service].</span></span>

## <a name="how-to-create-a-queue"></a><span data-ttu-id="7e3f9-126">Postupy: Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="7e3f9-126">How To: Create a Queue</span></span>
<span data-ttu-id="7e3f9-127">Následující kód vytvoří **QueueService** objekt, který umožňuje pracovat s fronty.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-127">The following code creates a **QueueService** object, which enables you to work with queues.</span></span>

```
var queueSvc = azure.createQueueService();
```

<span data-ttu-id="7e3f9-128">Použití **createQueueIfNotExists** metoda, která vrátí zadanou frontu, pokud již existuje nebo vytvoří novou frontu se zadaným názvem, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-128">Use the **createQueueIfNotExists** method, which returns the specified queue if it already exists or creates a new queue with the specified name if it does not already exist.</span></span>

```
queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
  if(!error){
    // Queue created or exists
  }
});
```

<span data-ttu-id="7e3f9-129">Pokud je vytvářena fronta, `result.created` hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-129">If the queue is created, `result.created` is true.</span></span> <span data-ttu-id="7e3f9-130">Pokud existuje fronty, `result.created` je false.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-130">If the queue exists, `result.created` is false.</span></span>

### <a name="filters"></a><span data-ttu-id="7e3f9-131">Filtry</span><span class="sxs-lookup"><span data-stu-id="7e3f9-131">Filters</span></span>
<span data-ttu-id="7e3f9-132">Volitelné filtrování operace lze použít pro operace provedené pomocí **QueueService**.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-132">Optional filtering operations can be applied to operations performed using **QueueService**.</span></span> <span data-ttu-id="7e3f9-133">Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu s podpisem:</span><span class="sxs-lookup"><span data-stu-id="7e3f9-133">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with the signature:</span></span>

```
function handle (requestOptions, next)
```

<span data-ttu-id="7e3f9-134">Až to předzpracování na žádost o možnostech, potřebuje volat "Další" předání zpětné volání podpisem následující metody:</span><span class="sxs-lookup"><span data-stu-id="7e3f9-134">After doing its preprocessing on the request options, the method needs to call "next" passing a callback with the following signature:</span></span>

```
function (returnObject, finalCallback, next)
```

<span data-ttu-id="7e3f9-135">V této zpětného volání a po zpracování returnObject (odpovědi požadavek na server) zpětné volání je potřeba buď vyvolat další, pokud existuje pokračovat ve zpracovávání ostatní filtry nebo jednoduše vyvolat finalCallback jinak skončili volání služby.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-135">In this callback, and after processing the returnObject (the response from the request to the server), the callback needs to either invoke next if it exists to continue processing other filters or simply invoke finalCallback otherwise to end up the service invocation.</span></span>

<span data-ttu-id="7e3f9-136">Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí sady Azure SDK pro Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-136">Two filters that implement retry logic are included with the Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="7e3f9-137">Vytvoří následující **QueueService** objekt, který používá **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="7e3f9-137">The following creates a **QueueService** object that uses the **ExponentialRetryPolicyFilter**:</span></span>

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var queueSvc = azure.createQueueService().withFilter(retryOperations);
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="7e3f9-138">Postupy: Vložit zprávu do fronty</span><span class="sxs-lookup"><span data-stu-id="7e3f9-138">How To: Insert a Message into a Queue</span></span>
<span data-ttu-id="7e3f9-139">Chcete-li vložit zprávu do fronty, použijte **createMessage** metodu pro vytvoření nové zprávy a přidejte ji do fronty.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-139">To insert a message into a queue, use the **createMessage** method to create a new message and add it to the queue.</span></span>

```
queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
  if(!error){
    // Message inserted
  }
});
```

## <a name="how-to-peek-at-the-next-message"></a><span data-ttu-id="7e3f9-140">Postupy: Zobrazení náhledu další zprávy</span><span class="sxs-lookup"><span data-stu-id="7e3f9-140">How To: Peek at the Next Message</span></span>
<span data-ttu-id="7e3f9-141">Můžete prohlížet zprávy ve frontě bez odebere ji z fronty voláním **peekMessages** metoda.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-141">You can peek at the message in the front of a queue without removing it from the queue by calling the **peekMessages** method.</span></span> <span data-ttu-id="7e3f9-142">Ve výchozím nastavení **peekMessages** prohlédne do jedné zprávy.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-142">By default, **peekMessages** peeks at a single message.</span></span>

```
queueSvc.peekMessages('myqueue', function(error, result, response){
  if(!error){
    // Message text is in messages[0].messageText
  }
});
```

<span data-ttu-id="7e3f9-143">`result` Obsahuje zprávu.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-143">The `result` contains the message.</span></span>

> [!NOTE]
> <span data-ttu-id="7e3f9-144">Pomocí **peekMessages** Pokud nejsou žádné zprávy ve frontě nebude vrátí chybu, ale žádné zprávy budou vráceny.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-144">Using **peekMessages** when there are no messages in the queue will not return an error, however no messages will be returned.</span></span>
> 
> 

## <a name="how-to-dequeue-the-next-message"></a><span data-ttu-id="7e3f9-145">Postupy: Dequeue – další zprávy</span><span class="sxs-lookup"><span data-stu-id="7e3f9-145">How To: Dequeue the Next Message</span></span>
<span data-ttu-id="7e3f9-146">Zpracování zprávy je dvoustupňový proces:</span><span class="sxs-lookup"><span data-stu-id="7e3f9-146">Processing a message is a two-stage process:</span></span>

1. <span data-ttu-id="7e3f9-147">Dequeue – zpráva.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-147">Dequeue the message.</span></span>
2. <span data-ttu-id="7e3f9-148">Tuto zprávu odstraníte.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-148">Delete the message.</span></span>

<span data-ttu-id="7e3f9-149">Chcete-li dequeue – zprávu, použijte **getMessages**.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-149">To dequeue a message, use **getMessages**.</span></span> <span data-ttu-id="7e3f9-150">Díky tomu zprávy neviditelná ve frontě, takže je může zpracovat žádné další klienty.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-150">This makes the messages invisible in the queue, so no other clients can process them.</span></span> <span data-ttu-id="7e3f9-151">Po zpracování zprávy aplikace volání **deleteMessage** odstranění z fronty.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-151">Once your application has processed a message, call **deleteMessage** to delete it from the queue.</span></span> <span data-ttu-id="7e3f9-152">Následující příklad získá zprávu, a pak ji odstraní:</span><span class="sxs-lookup"><span data-stu-id="7e3f9-152">The following example gets a message, then deletes it:</span></span>

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
> <span data-ttu-id="7e3f9-153">Ve výchozím nastavení je zprávu pouze skryto 30 sekund, po kterých je viditelná pro ostatní klienty.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-153">By default, a message is only hidden for 30 seconds, after which it is visible to other clients.</span></span> <span data-ttu-id="7e3f9-154">Můžete zadat jinou hodnotu pomocí `options.visibilityTimeout` s **getMessages**.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-154">You can specify a different value by using `options.visibilityTimeout` with **getMessages**.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="7e3f9-155">Pomocí **getMessages** Pokud nejsou žádné zprávy ve frontě nebude vrátí chybu, ale žádné zprávy budou vráceny.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-155">Using **getMessages** when there are no messages in the queue will not return an error, however no messages will be returned.</span></span>
> 
> 

## <a name="how-to-change-the-contents-of-a-queued-message"></a><span data-ttu-id="7e3f9-156">Postupy: Změna obsahu zpráv zařazených ve frontě</span><span class="sxs-lookup"><span data-stu-id="7e3f9-156">How To: Change the Contents of a Queued Message</span></span>
<span data-ttu-id="7e3f9-157">Můžete změnit obsah zprávy přímo do fronty pomocí **updateMessage**.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-157">You can change the contents of a message in-place in the queue using **updateMessage**.</span></span> <span data-ttu-id="7e3f9-158">Následující příklad aktualizací text zprávy:</span><span class="sxs-lookup"><span data-stu-id="7e3f9-158">The following example updates the text of a message:</span></span>

```
queueSvc.getMessages('myqueue', function(error, result, response){
  if(!error){
    // Got the message
    var message = result[0];
    queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
      if(!error){
        // Message updated successfully
      }
    });
  }
});
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a><span data-ttu-id="7e3f9-159">Postupy: Další možnosti pro vyřazení zprávy</span><span class="sxs-lookup"><span data-stu-id="7e3f9-159">How To: Additional Options for Dequeuing Messages</span></span>
<span data-ttu-id="7e3f9-160">Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="7e3f9-160">There are two ways you can customize message retrieval from a queue:</span></span>

* <span data-ttu-id="7e3f9-161">`options.numOfMessages`-Načíst dávku zpráv (až 32.)</span><span class="sxs-lookup"><span data-stu-id="7e3f9-161">`options.numOfMessages` - Retrieve a batch of messages (up to 32.)</span></span>
* <span data-ttu-id="7e3f9-162">`options.visibilityTimeout`-Nastavte časový limit neviditelnosti delší nebo kratší.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-162">`options.visibilityTimeout` - Set a longer or shorter invisibility timeout.</span></span>

<span data-ttu-id="7e3f9-163">Následující příklad používá **getMessages** metoda získat 15 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-163">The following example uses the **getMessages** method to get 15 messages in one call.</span></span> <span data-ttu-id="7e3f9-164">Pak se každá zpráva zpracuje pomocí pro smyčky.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-164">Then it processes each message using a for loop.</span></span> <span data-ttu-id="7e3f9-165">Také nastaví časový limit neviditelnosti 5 minut pro všechny zprávy vrácená touto metodou.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-165">It also sets the invisibility timeout to five minutes for all messages returned by this method.</span></span>

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

## <a name="how-to-get-the-queue-length"></a><span data-ttu-id="7e3f9-166">Postupy: Získání délky fronty</span><span class="sxs-lookup"><span data-stu-id="7e3f9-166">How To: Get the Queue Length</span></span>
<span data-ttu-id="7e3f9-167">**GetQueueMetadata** vrací metadata o frontě, včetně přibližný počet zpráv čekajících ve frontě.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-167">The **getQueueMetadata** returns metadata about the queue, including the approximate number of messages waiting in the queue.</span></span>

```
queueSvc.getQueueMetadata('myqueue', function(error, result, response){
  if(!error){
    // Queue length is available in result.approximateMessageCount
  }
});
```

## <a name="how-to-list-queues"></a><span data-ttu-id="7e3f9-168">Postupy: Seznam fronty</span><span class="sxs-lookup"><span data-stu-id="7e3f9-168">How To: List Queues</span></span>
<span data-ttu-id="7e3f9-169">Chcete-li načíst seznam front, použijte **listQueuesSegmented**.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-169">To retrieve a list of queues, use **listQueuesSegmented**.</span></span> <span data-ttu-id="7e3f9-170">Chcete-li načíst seznam filtrovat podle konkrétní předpony, použijte **listQueuesSegmentedWithPrefix**.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-170">To retrieve a list filtered by a specific prefix, use **listQueuesSegmentedWithPrefix**.</span></span>

```
queueSvc.listQueuesSegmented(null, function(error, result, response){
  if(!error){
    // result.entries contains the list of queues
  }
});
```

<span data-ttu-id="7e3f9-171">Pokud nelze vrátit všechny fronty, `result.continuationToken` lze použít jako první parametr **listQueuesSegmented** nebo druhý parametr **listQueuesSegmentedWithPrefix** k načtení více výsledků.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-171">If all queues cannot be returned, `result.continuationToken` can be used as the first parameter of **listQueuesSegmented** or the second parameter of **listQueuesSegmentedWithPrefix** to retrieve more results.</span></span>

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="7e3f9-172">Postupy: Odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="7e3f9-172">How To: Delete a Queue</span></span>
<span data-ttu-id="7e3f9-173">Chcete-li odstranit frontu se všemi zprávami, které v ní, zavolejte **deleteQueue** metoda pro objekt fronty.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-173">To delete a queue and all the messages contained in it, call the **deleteQueue** method on the queue object.</span></span>

```
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

<span data-ttu-id="7e3f9-174">Pokud chcete vymazat všechny zprávy z fronty bez odstranění, použijte **clearMessages**.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-174">To clear all messages from a queue without deleting it, use **clearMessages**.</span></span>

## <a name="how-to-work-with-shared-access-signatures"></a><span data-ttu-id="7e3f9-175">Postupy: práce s podpisy sdíleného přístupu</span><span class="sxs-lookup"><span data-stu-id="7e3f9-175">How to: Work with Shared Access Signatures</span></span>
<span data-ttu-id="7e3f9-176">Podpisy sdíleného přístupu (SAS) jsou zabezpečené způsob, jak poskytnout podrobné přístup k frontám bez zadání názvu účtu úložiště nebo klíče.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-176">Shared Access Signatures (SAS) are a secure way to provide granular access to queues without providing your storage account name or keys.</span></span> <span data-ttu-id="7e3f9-177">SAS se často používá k poskytování omezený přístup k vaší front, například povolení mobilní aplikace k odeslání zprávy.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-177">SAS are often used to provide limited access to your queues, such as allowing a mobile app to submit messages.</span></span>

<span data-ttu-id="7e3f9-178">Důvěryhodné aplikace, jako je Cloudová služba vygeneruje SAS pomocí **generateSharedAccessSignature** z **QueueService**a poskytuje k nedůvěryhodné nebo částečně důvěryhodné aplikace.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-178">A trusted application such as a cloud-based service generates a SAS using the **generateSharedAccessSignature** of the **QueueService**, and provides it to an untrusted or semi-trusted application.</span></span> <span data-ttu-id="7e3f9-179">Například mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-179">For example, a mobile app.</span></span> <span data-ttu-id="7e3f9-180">SAS je generována pomocí zásad, která popisuje počátečním a koncovým datem, během které SAS je platný, a také úroveň přístupu k majiteli SAS.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-180">The SAS is generated using a policy, which describes the start and end dates during which the SAS is valid, as well as the access level granted to the SAS holder.</span></span>

<span data-ttu-id="7e3f9-181">Následující příklad vytvoří nové zásady sdíleného přístupu, který vám umožní držitele SAS přidat zprávy do fronty a vyprší platnost 100 minut od okamžiku, kdy je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-181">The following example generates a new shared access policy that will allow the SAS holder to add messages to the queue, and expires 100 minutes after the time it is created.</span></span>

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

<span data-ttu-id="7e3f9-182">Všimněte si, že informace o hostiteli je třeba zadat také, jako je povinný, když se pokusí přistupovat ke frontě držitele SAS.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-182">Note that the host information must be provided also, as it is required when the SAS holder attempts to access the queue.</span></span>

<span data-ttu-id="7e3f9-183">Klientská aplikace pak používá SAS s **QueueServiceWithSAS** k provedení operace u fronty.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-183">The client application then uses the SAS with **QueueServiceWithSAS** to perform operations against the queue.</span></span> <span data-ttu-id="7e3f9-184">Následující příklad se připojí do fronty a vytvoří zprávu.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-184">The following example connects to the queue and creates a message.</span></span>

```
var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
  if(!error){
    //message added
  }
});
```

<span data-ttu-id="7e3f9-185">Vzhledem k tomu, že SAS se vygeneroval s přidat přístup, pokud byly proveden pokus o čtení, aktualizaci nebo odstranění zprávy, bude vrácena chyba.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-185">Since the SAS was generated with add access, if an attempt were made to read, update or delete messages, an error would be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="7e3f9-186">Seznamy řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="7e3f9-186">Access control lists</span></span>
<span data-ttu-id="7e3f9-187">Seznam řízení přístupu (ACL) můžete také nastavit zásady přístupu pro SAS.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-187">You can also use an Access Control List (ACL) to set the access policy for a SAS.</span></span> <span data-ttu-id="7e3f9-188">To je užitečné, pokud chcete povolit více klientům přistupovat ke frontě, ale poskytnutí zásad, jiný přístup pro každého klienta.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-188">This is useful if you wish to allow multiple clients to access the queue, but provide different access policies for each client.</span></span>

<span data-ttu-id="7e3f9-189">Seznam ACL je implementovaná pomocí pole zásady přístupu s ID spojené s každou zásadu.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-189">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="7e3f9-190">V následujícím příkladu definuje dvě zásady; jeden pro "uživatel1" a jeden pro, uživatel2":</span><span class="sxs-lookup"><span data-stu-id="7e3f9-190">The  following example defines two policies; one for 'user1' and one for 'user2':</span></span>

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

<span data-ttu-id="7e3f9-191">Následující příklad načte aktuální seznam ACL pro **Moje_fronta**, pak přidá nové zásady pomocí **setQueueAcl**.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-191">The following example gets the current ACL for **myqueue**, then adds the new policies using **setQueueAcl**.</span></span> <span data-ttu-id="7e3f9-192">Tento přístup umožňuje:</span><span class="sxs-lookup"><span data-stu-id="7e3f9-192">This approach allows:</span></span>

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

<span data-ttu-id="7e3f9-193">Jakmile je nastavená seznamu ACL, potom můžete vytvořit SAS na základě ID pro zásadu.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-193">Once the ACL has been set, you can then create a SAS based on the ID for a policy.</span></span> <span data-ttu-id="7e3f9-194">Následující příklad vytvoří nový SAS pro, uživatel2":</span><span class="sxs-lookup"><span data-stu-id="7e3f9-194">The following example creates a new SAS for 'user2':</span></span>

```
queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="7e3f9-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7e3f9-195">Next Steps</span></span>
<span data-ttu-id="7e3f9-196">Teď, když jste se naučili základy používání služby queue storage, postupujte podle následujících odkazech na další informace o složitějších úlohách úložiště.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-196">Now that you've learned the basics of queue storage, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="7e3f9-197">Přejděte [Blog týmu Azure Storage][Azure Storage Team Blog].</span><span class="sxs-lookup"><span data-stu-id="7e3f9-197">Visit the [Azure Storage Team Blog][Azure Storage Team Blog].</span></span>
* <span data-ttu-id="7e3f9-198">Přejděte [sada SDK úložiště Azure pro uzel] [ Azure Storage SDK for Node] úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="7e3f9-198">Visit the [Azure Storage SDK for Node][Azure Storage SDK for Node] repository on GitHub.</span></span>

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node
[using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com
<span data-ttu-id="7e3f9-199">[vytvoření webové aplikace Node.js ve službě Azure App Service]: ../app-service-web/app-service-web-get-started-nodejs.md</span><span class="sxs-lookup"><span data-stu-id="7e3f9-199">[Create a Node.js web app in Azure App Service]: ../app-service-web/app-service-web-get-started-nodejs.md</span></span>
<span data-ttu-id="7e3f9-200">[webové aplikace Node.js pomocí služby Azure Table]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md</span><span class="sxs-lookup"><span data-stu-id="7e3f9-200">[Node.js web app using the Azure Table Service]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md</span></span>


<span data-ttu-id="7e3f9-201">[sestavení a nasazení aplikace Node.js ve službě Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md</span><span class="sxs-lookup"><span data-stu-id="7e3f9-201">[Build and deploy a Node.js application to an Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md</span></span>
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
<span data-ttu-id="7e3f9-202">[sestavení a nasazení webové aplikace Node.js do Azure pomocí Web Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md</span><span class="sxs-lookup"><span data-stu-id="7e3f9-202">[Build and deploy a Node.js web app to Azure using Web Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md</span></span>
