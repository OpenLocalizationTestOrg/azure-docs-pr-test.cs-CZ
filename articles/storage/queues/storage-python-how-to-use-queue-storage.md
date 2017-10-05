---
title: "Používání úložiště Queue z Pythonu | Microsoft Docs"
description: "Naučte se používat službu Azure Queue z Pythonu vytvářet a odstraňovat fronty a vložit, získání a odstranění zprávy."
services: storage
documentationcenter: python
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: cc0d2da2-379a-4b58-a234-8852b4e3d99d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 963c11acb7939993568a774cd281145a8059b5a6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-queue-storage-from-python"></a><span data-ttu-id="e3857-103">Používání úložiště Queue z Pythonu</span><span class="sxs-lookup"><span data-stu-id="e3857-103">How to use Queue storage from Python</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="e3857-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="e3857-104">Overview</span></span>
<span data-ttu-id="e3857-105">Tento průvodce vám ukáže, jak provádět běžné scénáře s využitím služby Azure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="e3857-105">This guide shows you how to perform common scenarios using the Azure Queue storage service.</span></span> <span data-ttu-id="e3857-106">Ukázky jsou napsané v Pythonu a použití [Microsoft Azure SDK úložiště pro jazyk Python].</span><span class="sxs-lookup"><span data-stu-id="e3857-106">The samples are written in Python and use the [Microsoft Azure Storage SDK for Python].</span></span> <span data-ttu-id="e3857-107">Pokryté scénáře zahrnují **vkládání**, **prohlížení**, **získávání**, a **odstraňování** fronty zpráv, a také **vytváření a odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="e3857-107">The scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span> <span data-ttu-id="e3857-108">Další informace o frontách najdete v oddílu [Další kroky].</span><span class="sxs-lookup"><span data-stu-id="e3857-108">For more information on queues, refer to the [Next Steps] section.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a><span data-ttu-id="e3857-109">Postupy: Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="e3857-109">How To: Create a Queue</span></span>
<span data-ttu-id="e3857-110">**QueueService** objekt vám umožňuje spolupracovat s fronty.</span><span class="sxs-lookup"><span data-stu-id="e3857-110">The **QueueService** object lets you work with queues.</span></span> <span data-ttu-id="e3857-111">Následující kód vytvoří **QueueService** objektu.</span><span class="sxs-lookup"><span data-stu-id="e3857-111">The following code creates a **QueueService** object.</span></span> <span data-ttu-id="e3857-112">Přidejte následující v horní části všech soubor Python, ve kterém chcete k programovému přístupu ke službě Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="e3857-112">Add the following near the top of any Python file in which you wish to programmatically access Azure Storage:</span></span>

```python
from azure.storage.queue import QueueService
```

<span data-ttu-id="e3857-113">Následující kód vytvoří **QueueService** pomocí klíč účet a název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e3857-113">The following code creates a **QueueService** object using the storage account name and account key.</span></span> <span data-ttu-id="e3857-114">Nahraďte název účtu a klíč 'stránku Můj účet' a 'mykey.</span><span class="sxs-lookup"><span data-stu-id="e3857-114">Replace 'myaccount' and 'mykey' with your account name and key.</span></span>

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="e3857-115">Postupy: Vložit zprávu do fronty</span><span class="sxs-lookup"><span data-stu-id="e3857-115">How To: Insert a Message into a Queue</span></span>
<span data-ttu-id="e3857-116">Chcete-li vložit zprávu do fronty, použijte **put\_zpráva** metodu pro vytvoření nové zprávy a přidejte ji do fronty.</span><span class="sxs-lookup"><span data-stu-id="e3857-116">To insert a message into a queue, use the **put\_message** method to create a new message and add it to the queue.</span></span>

```python
queue_service.put_message('taskqueue', u'Hello World')
```

## <a name="how-to-peek-at-the-next-message"></a><span data-ttu-id="e3857-117">Postupy: Zobrazení náhledu další zprávy</span><span class="sxs-lookup"><span data-stu-id="e3857-117">How To: Peek at the Next Message</span></span>
<span data-ttu-id="e3857-118">Můžete prohlížet zprávy ve frontě bez odebere ji z fronty voláním **funkce Náhled\_zprávy** metoda.</span><span class="sxs-lookup"><span data-stu-id="e3857-118">You can peek at the message in the front of a queue without removing it from the queue by calling the **peek\_messages** method.</span></span> <span data-ttu-id="e3857-119">Ve výchozím nastavení **funkce Náhled\_zprávy** prohlédne do jedné zprávy.</span><span class="sxs-lookup"><span data-stu-id="e3857-119">By default, **peek\_messages** peeks at a single message.</span></span>

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="how-to-dequeue-messages"></a><span data-ttu-id="e3857-120">Postupy: Dequeue – zprávy</span><span class="sxs-lookup"><span data-stu-id="e3857-120">How To: Dequeue Messages</span></span>
<span data-ttu-id="e3857-121">Váš kód odebere zprávu z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="e3857-121">Your code removes a message from a queue in two steps.</span></span> <span data-ttu-id="e3857-122">Při volání **získat\_zprávy**, získáte další zprávu ve frontě ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e3857-122">When you call **get\_messages**, you get the next message in a queue by default.</span></span> <span data-ttu-id="e3857-123">Zpráva vrácená metodou **získat\_zprávy** stane neviditelnou pro jakýkoli jiný kód čtení zpráv z této fronty.</span><span class="sxs-lookup"><span data-stu-id="e3857-123">A message returned from **get\_messages** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="e3857-124">Ve výchozím nastavení tato zpráva zůstává neviditelná po dobu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="e3857-124">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="e3857-125">K dokončení odebrání zprávy z fronty, musíte také zavolat **odstranit\_zpráva**.</span><span class="sxs-lookup"><span data-stu-id="e3857-125">To finish removing the message from the queue, you must also call **delete\_message**.</span></span> <span data-ttu-id="e3857-126">Tento dvoukrokový proces odebrání zprávy zaručuje, že když kódu se nepodaří zpracovat zprávu z důvodu selhání hardwaru nebo softwaru, jiná instance vašeho kódu můžete stejnou zprávu získat a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="e3857-126">This two-step process of removing a message assures that when your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="e3857-127">Váš kód zavolá metodu **odstranit\_zpráva** pravým po zpracování zprávy.</span><span class="sxs-lookup"><span data-stu-id="e3857-127">Your code calls **delete\_message** right after the message has been processed.</span></span>

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

<span data-ttu-id="e3857-128">Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="e3857-128">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="e3857-129">Za prvé si můžete načíst dávku zpráv (až 32).</span><span class="sxs-lookup"><span data-stu-id="e3857-129">First, you can get a batch of messages (up to 32).</span></span> <span data-ttu-id="e3857-130">Za druhé si můžete nastavit delší nebo kratší časový limit neviditelnosti, aby měl váš kód více nebo méně času na úplné zpracování jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="e3857-130">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time to fully process each message.</span></span> <span data-ttu-id="e3857-131">Následující příklad kódu používá **získat\_zprávy** metoda získat 16 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="e3857-131">The following code example uses the **get\_messages** method to get 16 messages in one call.</span></span> <span data-ttu-id="e3857-132">Pak se každá zpráva zpracuje pomocí pro smyčky.</span><span class="sxs-lookup"><span data-stu-id="e3857-132">Then it processes each message using a for loop.</span></span> <span data-ttu-id="e3857-133">Také se pro každou zprávu nastaví časový limit neviditelnosti 5 minut.</span><span class="sxs-lookup"><span data-stu-id="e3857-133">It also sets the invisibility timeout to five minutes for each message.</span></span>

```python
messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)        
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a><span data-ttu-id="e3857-134">Postupy: Změna obsahu zpráv zařazených ve frontě</span><span class="sxs-lookup"><span data-stu-id="e3857-134">How To: Change the Contents of a Queued Message</span></span>
<span data-ttu-id="e3857-135">Podle potřeby můžete změnit obsah zprávy přímo ve frontě.</span><span class="sxs-lookup"><span data-stu-id="e3857-135">You can change the contents of a message in-place in the queue.</span></span> <span data-ttu-id="e3857-136">Pokud zpráva představuje pracovní úlohu, mohli byste tuto funkci použít k aktualizaci stavu pracovních úloh.</span><span class="sxs-lookup"><span data-stu-id="e3857-136">If the message represents a work task, you could use this feature to update the status of the work task.</span></span> <span data-ttu-id="e3857-137">Kód pod používá **aktualizace\_zpráva** metoda aktualizace zprávu.</span><span class="sxs-lookup"><span data-stu-id="e3857-137">The code below uses the **update\_message** method to update a message.</span></span> <span data-ttu-id="e3857-138">Časový limit viditelnosti nastavena na hodnotu 0, což znamená, tato zpráva se zobrazí okamžitě a je obsah aktualizován.</span><span class="sxs-lookup"><span data-stu-id="e3857-138">The visibility timeout is set to 0, meaning the message appears immediately and the content is updated.</span></span>

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="how-to-get-the-queue-length"></a><span data-ttu-id="e3857-139">Postupy: Získání délky fronty</span><span class="sxs-lookup"><span data-stu-id="e3857-139">How To: Get the Queue Length</span></span>
<span data-ttu-id="e3857-140">Podle potřeby můžete získat odhadovaný počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="e3857-140">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="e3857-141">**Získat\_fronty\_metadata** metoda požádá službu front vrátit metadata o fronty a **approximate_message_count**.</span><span class="sxs-lookup"><span data-stu-id="e3857-141">The **get\_queue\_metadata** method asks the queue service to return metadata about the queue, and the **approximate_message_count**.</span></span> <span data-ttu-id="e3857-142">Výsledkem je přibližný, protože zprávy můžete přidat nebo odebrat po služby front odpoví na žádost.</span><span class="sxs-lookup"><span data-stu-id="e3857-142">The result is only approximate because messages can be added or removed after the queue service responds to your request.</span></span>

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="e3857-143">Postupy: Odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="e3857-143">How To: Delete a Queue</span></span>
<span data-ttu-id="e3857-144">Chcete-li odstranit frontu se všemi zprávami, které v ní, zavolejte **odstranit\_fronty** metoda.</span><span class="sxs-lookup"><span data-stu-id="e3857-144">To delete a queue and all the messages contained in it, call the **delete\_queue** method.</span></span>

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a><span data-ttu-id="e3857-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e3857-145">Next Steps</span></span>
<span data-ttu-id="e3857-146">Teď, když jste se naučili základy používání služby Queue storage, postupujte podle následujících odkazech na další informace.</span><span class="sxs-lookup"><span data-stu-id="e3857-146">Now that you've learned the basics of Queue storage, follow these links to learn more.</span></span>

* [<span data-ttu-id="e3857-147">Středisko pro vývojáře programující v Pythonu</span><span class="sxs-lookup"><span data-stu-id="e3857-147">Python Developer Center</span></span>](/develop/python/)
* [<span data-ttu-id="e3857-148">REST API služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e3857-148">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="e3857-149">[Blog týmu Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="e3857-149">[Azure Storage Team Blog]</span></span>
* <span data-ttu-id="e3857-150">[Microsoft Azure SDK úložiště pro jazyk Python]</span><span class="sxs-lookup"><span data-stu-id="e3857-150">[Microsoft Azure Storage SDK for Python]</span></span>

[Blog týmu Azure Storage]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure SDK úložiště pro jazyk Python]: https://github.com/Azure/azure-storage-python