---
title: "aaaHow toouse úložiště Queue z Pythonu | Microsoft Docs"
description: "Zjistěte, jak toouse hello služby Azure Queue z Pythonu toocreate odstranit fronty a vložit, získání a odstranění zprávy."
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
ms.openlocfilehash: ce8d999d9fafaef0dab48442560d004c034c0804
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-python"></a><span data-ttu-id="789d0-103">Jak toouse úložiště Queue z Pythonu</span><span class="sxs-lookup"><span data-stu-id="789d0-103">How toouse Queue storage from Python</span></span>
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="789d0-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="789d0-104">Overview</span></span>
<span data-ttu-id="789d0-105">Tento průvodce vám ukáže, jak hello tooperform běžné scénáře s využitím služby Azure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="789d0-105">This guide shows you how tooperform common scenarios using hello Azure Queue storage service.</span></span> <span data-ttu-id="789d0-106">Hello ukázky jsou napsané v Pythonu a používají hello [Microsoft Azure SDK úložiště pro jazyk Python].</span><span class="sxs-lookup"><span data-stu-id="789d0-106">hello samples are written in Python and use hello [Microsoft Azure Storage SDK for Python].</span></span> <span data-ttu-id="789d0-107">Hello pokryté scénáře zahrnují **vkládání**, **prohlížení**, **získávání**, a **odstraňování** fronty zpráv, a také  **vytváření a odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="789d0-107">hello scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span> <span data-ttu-id="789d0-108">Další informace o frontách najdete v části toohello [Další kroky].</span><span class="sxs-lookup"><span data-stu-id="789d0-108">For more information on queues, refer toohello [Next Steps] section.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a><span data-ttu-id="789d0-109">Postupy: Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="789d0-109">How To: Create a Queue</span></span>
<span data-ttu-id="789d0-110">Hello **QueueService** objekt vám umožňuje spolupracovat s fronty.</span><span class="sxs-lookup"><span data-stu-id="789d0-110">hello **QueueService** object lets you work with queues.</span></span> <span data-ttu-id="789d0-111">Hello následující kód vytvoří **QueueService** objektu.</span><span class="sxs-lookup"><span data-stu-id="789d0-111">hello following code creates a **QueueService** object.</span></span> <span data-ttu-id="789d0-112">Přidejte následující hello v horní hello jakéhokoliv Python souboru, ve kterém chcete tooprogrammatically přístupu Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="789d0-112">Add hello following near hello top of any Python file in which you wish tooprogrammatically access Azure Storage:</span></span>

```python
from azure.storage.queue import QueueService
```

<span data-ttu-id="789d0-113">Hello následující kód vytvoří **QueueService** objekt, který používá hello klíč účtu úložiště účet a název.</span><span class="sxs-lookup"><span data-stu-id="789d0-113">hello following code creates a **QueueService** object using hello storage account name and account key.</span></span> <span data-ttu-id="789d0-114">Nahraďte název účtu a klíč 'stránku Můj účet' a 'mykey.</span><span class="sxs-lookup"><span data-stu-id="789d0-114">Replace 'myaccount' and 'mykey' with your account name and key.</span></span>

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="789d0-115">Postupy: Vložit zprávu do fronty</span><span class="sxs-lookup"><span data-stu-id="789d0-115">How To: Insert a Message into a Queue</span></span>
<span data-ttu-id="789d0-116">tooinsert zprávu do fronty, použijte hello **put\_zpráva** metoda vytvořte novou zprávu a přidat ji toohello fronty.</span><span class="sxs-lookup"><span data-stu-id="789d0-116">tooinsert a message into a queue, use hello **put\_message** method to create a new message and add it toohello queue.</span></span>

```python
queue_service.put_message('taskqueue', u'Hello World')
```

## <a name="how-to-peek-at-hello-next-message"></a><span data-ttu-id="789d0-117">Postupy: Zobrazení náhledu další zprávy hello</span><span class="sxs-lookup"><span data-stu-id="789d0-117">How To: Peek at hello Next Message</span></span>
<span data-ttu-id="789d0-118">Můžete prohlížet zprávy hello v popředí hello fronty bez odebere ji z fronty hello pomocí volání hello **funkce Náhled\_zprávy** metoda.</span><span class="sxs-lookup"><span data-stu-id="789d0-118">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **peek\_messages** method.</span></span> <span data-ttu-id="789d0-119">Ve výchozím nastavení **funkce Náhled\_zprávy** prohlédne do jedné zprávy.</span><span class="sxs-lookup"><span data-stu-id="789d0-119">By default, **peek\_messages** peeks at a single message.</span></span>

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="how-to-dequeue-messages"></a><span data-ttu-id="789d0-120">Postupy: Dequeue – zprávy</span><span class="sxs-lookup"><span data-stu-id="789d0-120">How To: Dequeue Messages</span></span>
<span data-ttu-id="789d0-121">Váš kód odebere zprávu z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="789d0-121">Your code removes a message from a queue in two steps.</span></span> <span data-ttu-id="789d0-122">Při volání **získat\_zprávy**, získat hello další zprávu ve frontě ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="789d0-122">When you call **get\_messages**, you get hello next message in a queue by default.</span></span> <span data-ttu-id="789d0-123">Zpráva vrácená metodou **získat\_zprávy** stane neviditelnou tooany další kód, který čte zprávy z této fronty.</span><span class="sxs-lookup"><span data-stu-id="789d0-123">A message returned from **get\_messages** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="789d0-124">Ve výchozím nastavení tato zpráva zůstává neviditelná po dobu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="789d0-124">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="789d0-125">toofinish odebírání uvítací zprávu z fronty hello, musíte také zavolat **odstranit\_zpráva**.</span><span class="sxs-lookup"><span data-stu-id="789d0-125">toofinish removing hello message from hello queue, you must also call **delete\_message**.</span></span> <span data-ttu-id="789d0-126">Tento dvoukrokový proces odebrání zprávy zaručuje, že pokud se váš kód nezdaří tooprocess zprávu z důvodu selhání hardwaru nebo softwaru, jiná instance vašeho kódu můžete stejnou zprávu získat a akci opakujte.</span><span class="sxs-lookup"><span data-stu-id="789d0-126">This two-step process of removing a message assures that when your code fails tooprocess a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="789d0-127">Váš kód zavolá metodu **odstranit\_zpráva** hned po zpracování zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="789d0-127">Your code calls **delete\_message** right after hello message has been processed.</span></span>

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

<span data-ttu-id="789d0-128">Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="789d0-128">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="789d0-129">Nejprve můžete načíst dávku zpráv (až too32).</span><span class="sxs-lookup"><span data-stu-id="789d0-129">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="789d0-130">Druhý můžete nastavit časový limit neviditelnosti delší nebo kratší, povolení váš kód více nebo méně času toofully zpracování jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="789d0-130">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="789d0-131">Hello následující příklad kódu používá **získat\_zprávy** metoda tooget 16 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="789d0-131">hello following code example uses the **get\_messages** method tooget 16 messages in one call.</span></span> <span data-ttu-id="789d0-132">Pak se každá zpráva zpracuje pomocí pro smyčky.</span><span class="sxs-lookup"><span data-stu-id="789d0-132">Then it processes each message using a for loop.</span></span> <span data-ttu-id="789d0-133">Také pět minut pro každou zprávu nastaví časový limit neviditelnosti hello.</span><span class="sxs-lookup"><span data-stu-id="789d0-133">It also sets hello invisibility timeout to five minutes for each message.</span></span>

```python
messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)        
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a><span data-ttu-id="789d0-134">Postupy: Změna hello obsah zprávy ve frontě</span><span class="sxs-lookup"><span data-stu-id="789d0-134">How To: Change hello Contents of a Queued Message</span></span>
<span data-ttu-id="789d0-135">Můžete změnit obsah zprávy přímo ve frontě hello hello.</span><span class="sxs-lookup"><span data-stu-id="789d0-135">You can change hello contents of a message in-place in hello queue.</span></span> <span data-ttu-id="789d0-136">Pokud zpráva představuje pracovní úlohu, můžete použít tuto funkci tooupdate stav hello pracovní úlohy.</span><span class="sxs-lookup"><span data-stu-id="789d0-136">If the message represents a work task, you could use this feature tooupdate the status of hello work task.</span></span> <span data-ttu-id="789d0-137">Následující kód Hello používá hello **aktualizace\_zpráva** metoda tooupdate zprávu.</span><span class="sxs-lookup"><span data-stu-id="789d0-137">hello code below uses hello **update\_message** method tooupdate a message.</span></span> <span data-ttu-id="789d0-138">časový limit viditelnosti Hello nastaven too0, což znamená, tato zpráva se zobrazí okamžitě a hello obsah aktualizován.</span><span class="sxs-lookup"><span data-stu-id="789d0-138">hello visibility timeout is set too0, meaning the message appears immediately and hello content is updated.</span></span>

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="how-to-get-hello-queue-length"></a><span data-ttu-id="789d0-139">Postupy: Získání hello délka fronty</span><span class="sxs-lookup"><span data-stu-id="789d0-139">How To: Get hello Queue Length</span></span>
<span data-ttu-id="789d0-140">Můžete získat odhad hello počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="789d0-140">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="789d0-141">**Získat\_fronty\_metadata** metoda požádá hello fronty služby tooreturn metadata o hello fronty a hello **approximate_message_count**.</span><span class="sxs-lookup"><span data-stu-id="789d0-141">The **get\_queue\_metadata** method asks hello queue service tooreturn metadata about hello queue, and hello **approximate_message_count**.</span></span> <span data-ttu-id="789d0-142">výsledek Hello je pouze přibližné, protože zprávy můžete přidat nebo odebrat po tooyour žádost odpoví služby front.</span><span class="sxs-lookup"><span data-stu-id="789d0-142">hello result is only approximate because messages can be added or removed after the queue service responds tooyour request.</span></span>

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="789d0-143">Postupy: Odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="789d0-143">How To: Delete a Queue</span></span>
<span data-ttu-id="789d0-144">toodelete frontu a všechny zprávy hello obsažené v něm, volejte **odstranit\_fronty** metoda.</span><span class="sxs-lookup"><span data-stu-id="789d0-144">toodelete a queue and all hello messages contained in it, call the **delete\_queue** method.</span></span>

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a><span data-ttu-id="789d0-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="789d0-145">Next Steps</span></span>
<span data-ttu-id="789d0-146">Teď, když jste se naučili základy hello fronty úložiště, postupujte podle těchto odkazů toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="789d0-146">Now that you've learned hello basics of Queue storage, follow these links toolearn more.</span></span>

* [<span data-ttu-id="789d0-147">Středisko pro vývojáře programující v Pythonu</span><span class="sxs-lookup"><span data-stu-id="789d0-147">Python Developer Center</span></span>](/develop/python/)
* [<span data-ttu-id="789d0-148">REST API služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="789d0-148">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="789d0-149">[Blog týmu Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="789d0-149">[Azure Storage Team Blog]</span></span>
* <span data-ttu-id="789d0-150">[Microsoft Azure SDK úložiště pro jazyk Python]</span><span class="sxs-lookup"><span data-stu-id="789d0-150">[Microsoft Azure Storage SDK for Python]</span></span>

[Blog týmu Azure Storage]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure SDK úložiště pro jazyk Python]: https://github.com/Azure/azure-storage-python