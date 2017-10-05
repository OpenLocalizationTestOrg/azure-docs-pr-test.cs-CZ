---
title: "Používání úložiště Queue z Ruby | Microsoft Docs"
description: "Naučte se používat službu front Azure k vytváření a odstraňování front a vložit, získání a odstranění zprávy. Ukázky napsané v Ruby."
services: storage
documentationcenter: ruby
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 59c2d81b-db9c-46ee-ade2-2f0caae6b1e6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: b1a7dd36af6c45bf085342cdf9c1c926a5040792
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-queue-storage-from-ruby"></a><span data-ttu-id="fa6ac-104">Používání úložiště Queue z Ruby</span><span class="sxs-lookup"><span data-stu-id="fa6ac-104">How to use Queue storage from Ruby</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="fa6ac-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="fa6ac-105">Overview</span></span>
<span data-ttu-id="fa6ac-106">Tento průvodce vám ukáže, jak provádět běžné scénáře pomocí služby Microsoft Azure Queue Storage.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-106">This guide shows you how to perform common scenarios using the Microsoft Azure Queue Storage service.</span></span> <span data-ttu-id="fa6ac-107">Ukázky jsou zapsány pomocí rozhraní API služby Azure Ruby.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-107">The samples are written using the Ruby Azure API.</span></span>
<span data-ttu-id="fa6ac-108">Pokryté scénáře zahrnují **vkládání**, **prohlížení**, **získávání**, a **odstraňování** fronty zpráv, a také **vytváření a odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-108">The scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="fa6ac-109">Vytvoření Ruby aplikace</span><span class="sxs-lookup"><span data-stu-id="fa6ac-109">Create a Ruby Application</span></span>
<span data-ttu-id="fa6ac-110">Vytvořte aplikaci pro poznámky Ruby.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-110">Create a Ruby application.</span></span> <span data-ttu-id="fa6ac-111">Pokyny najdete v tématu [Ruby, na které webové aplikace ve virtuálním počítači Azure](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="fa6ac-111">For instructions, see [Ruby on Rails Web application on an Azure VM](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span></span>

## <a name="configure-your-application-to-access-storage"></a><span data-ttu-id="fa6ac-112">Konfigurace aplikace pro přístup k úložišti</span><span class="sxs-lookup"><span data-stu-id="fa6ac-112">Configure Your Application to Access Storage</span></span>
<span data-ttu-id="fa6ac-113">Pokud chcete používat úložiště Azure, musíte stáhnout a použít Ruby azure balíčku, který obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-113">To use Azure storage, you need to download and use the Ruby azure package, which includes a set of convenience libraries that communicate with the storage REST services.</span></span>

### <a name="use-rubygems-to-obtain-the-package"></a><span data-ttu-id="fa6ac-114">Použití RubyGems získat balíček</span><span class="sxs-lookup"><span data-stu-id="fa6ac-114">Use RubyGems to obtain the package</span></span>
1. <span data-ttu-id="fa6ac-115">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="fa6ac-115">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="fa6ac-116">"Gem instalace azure" zadejte v příkazovém okně instalace gem a závislostí.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-116">Type "gem install azure" in the command window to install the gem and dependencies.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="fa6ac-117">Import balíčku</span><span class="sxs-lookup"><span data-stu-id="fa6ac-117">Import the package</span></span>
<span data-ttu-id="fa6ac-118">Použít svém oblíbeném textovém editoru, přidejte na začátek souboru Ruby, kde máte v úmyslu používat úložiště následující:</span><span class="sxs-lookup"><span data-stu-id="fa6ac-118">Use your favorite text editor, add the following to the top of the Ruby file where you intend to use storage:</span></span>

```ruby
require "azure"
```

## <a name="setup-an-azure-storage-connection"></a><span data-ttu-id="fa6ac-119">Nastavit připojení k úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="fa6ac-119">Setup an Azure Storage Connection</span></span>
<span data-ttu-id="fa6ac-120">Modul azure, bude číst proměnné prostředí **AZURE\_úložiště\_účet** a **AZURE\_úložiště\_ACCESS_KEY** informace požadované pro připojení k účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-120">The azure module will read the environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS_KEY** for information required to connect to your Azure storage account.</span></span> <span data-ttu-id="fa6ac-121">Pokud nejsou nastavené těchto proměnných prostředí, je nutné zadat informace o účtu před použitím **Azure::QueueService** následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="fa6ac-121">If these environment variables are not set, you must specify the account information before using **Azure::QueueService** with the following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your Azure storage access key>"
```

<span data-ttu-id="fa6ac-122">K získání těchto hodnot z klasický nebo účet správce prostředků úložiště na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="fa6ac-122">To obtain these values from a classic or Resource Manager storage account in the Azure portal:</span></span>

1. <span data-ttu-id="fa6ac-123">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fa6ac-123">Log in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fa6ac-124">Přejděte na účet úložiště, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-124">Navigate to the storage account you want to use.</span></span>
3. <span data-ttu-id="fa6ac-125">V okně nastavení na pravé straně klikněte na tlačítko **přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-125">In the Settings blade on the right, click **Access Keys**.</span></span>
4. <span data-ttu-id="fa6ac-126">V okně klíče přístup, který se zobrazí uvidíte přístupový klíč 1 a 2 přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-126">In the Access keys blade that appears, you'll see the access key 1 and access key 2.</span></span> <span data-ttu-id="fa6ac-127">Můžete použít kteroukoli z nich.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-127">You can use either of these.</span></span> 
5. <span data-ttu-id="fa6ac-128">Kliknutím na ikonu kopírování do schránky zkopírujte klíč.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-128">Click the copy icon to copy the key to the clipboard.</span></span> 

## <a name="how-to-create-a-queue"></a><span data-ttu-id="fa6ac-129">Postupy: Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="fa6ac-129">How To: Create a Queue</span></span>
<span data-ttu-id="fa6ac-130">Následující kód vytvoří **Azure::QueueService** objekt, který umožňuje pracovat s fronty.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-130">The following code creates a **Azure::QueueService** object, which enables you to work with queues.</span></span>

```ruby
azure_queue_service = Azure::QueueService.new
```

<span data-ttu-id="fa6ac-131">Použití **create_queue()** metodu pro vytvoření fronty se zadaným názvem.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-131">Use the **create_queue()** method to create a queue with the specified name.</span></span>

```ruby
begin
  azure_queue_service.create_queue("test-queue")
rescue
  puts $!
end
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="fa6ac-132">Postupy: Vložit zprávu do fronty</span><span class="sxs-lookup"><span data-stu-id="fa6ac-132">How To: Insert a Message into a Queue</span></span>
<span data-ttu-id="fa6ac-133">Chcete-li vložit zprávu do fronty, použijte **create_message()** metodu pro vytvoření nové zprávy a přidejte ji do fronty.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-133">To insert a message into a queue, use the **create_message()** method to create a new message and add it to the queue.</span></span>

```ruby
azure_queue_service.create_message("test-queue", "test message")
```

## <a name="how-to-peek-at-the-next-message"></a><span data-ttu-id="fa6ac-134">Postupy: Zobrazení náhledu další zprávy</span><span class="sxs-lookup"><span data-stu-id="fa6ac-134">How To: Peek at the Next Message</span></span>
<span data-ttu-id="fa6ac-135">Můžete prohlížet zprávy ve frontě bez odebere ji z fronty voláním **funkce Náhled\_messages()** metoda.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-135">You can peek at the message in the front of a queue without removing it from the queue by calling the **peek\_messages()** method.</span></span> <span data-ttu-id="fa6ac-136">Ve výchozím nastavení **funkce Náhled\_messages()** prohlédne do jedné zprávy.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-136">By default, **peek\_messages()** peeks at a single message.</span></span> <span data-ttu-id="fa6ac-137">Můžete také zadat počet zpráv, které chcete prohlížet.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-137">You can also specify how many messages you want to peek.</span></span>

```ruby
result = azure_queue_service.peek_messages("test-queue",
  {:number_of_messages => 10})
```

## <a name="how-to-dequeue-the-next-message"></a><span data-ttu-id="fa6ac-138">Postupy: Dequeue – další zprávy</span><span class="sxs-lookup"><span data-stu-id="fa6ac-138">How To: Dequeue the Next Message</span></span>
<span data-ttu-id="fa6ac-139">Můžete odebrat zprávu z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-139">You can remove a message from a queue in two steps.</span></span>

1. <span data-ttu-id="fa6ac-140">Při volání **seznamu\_messages()**, získáte další zprávu ve frontě ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-140">When you call **list\_messages()**, you get the next message in a queue by default.</span></span> <span data-ttu-id="fa6ac-141">Můžete také zadat počet zpráv, které chcete získat.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-141">You can also specify how many messages you want to get.</span></span> <span data-ttu-id="fa6ac-142">Zpráv vrácených z **seznamu\_messages()** stane neviditelnou pro jakýkoli jiný kód čtení zpráv z této fronty.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-142">The messages returned from **list\_messages()** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="fa6ac-143">Můžete předat časový limit viditelnosti v sekundách jako parametr.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-143">You pass in the visibility timeout in seconds as a parameter.</span></span>
2. <span data-ttu-id="fa6ac-144">K dokončení odebrání zprávy z fronty, musíte také zavolat **delete_message()**.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-144">To finish removing the message from the queue, you must also call **delete_message()**.</span></span>

<span data-ttu-id="fa6ac-145">Tento dvoukrokový proces odebrání zprávy zaručuje, že když kódu se nepodaří zpracovat zprávu z důvodu selhání hardwaru nebo softwaru, jiná instance vašeho kódu můžete stejnou zprávu získat a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-145">This two-step process of removing a message assures that when your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="fa6ac-146">Váš kód zavolá metodu **odstranit\_message()** pravým po zpracování zprávy.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-146">Your code calls **delete\_message()** right after the message has been processed.</span></span>

```ruby
messages = azure_queue_service.list_messages("test-queue", 30)
azure_queue_service.delete_message("test-queue", 
  messages[0].id, messages[0].pop_receipt)
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a><span data-ttu-id="fa6ac-147">Postupy: Změna obsahu zpráv zařazených ve frontě</span><span class="sxs-lookup"><span data-stu-id="fa6ac-147">How To: Change the Contents of a Queued Message</span></span>
<span data-ttu-id="fa6ac-148">Podle potřeby můžete změnit obsah zprávy přímo ve frontě.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-148">You can change the contents of a message in-place in the queue.</span></span> <span data-ttu-id="fa6ac-149">Kód pod používá **update_message()** metoda aktualizace zprávu.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-149">The code below uses the **update_message()** method to update a message.</span></span> <span data-ttu-id="fa6ac-150">Metoda vrátí řazené kolekce členů, která obsahuje pop příjmu zprávy ve frontě a hodnotu Datum čas UTC, která představuje při zpráva se bude zobrazovat na fronty.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-150">The method will return a tuple which contains the pop receipt of the queue message and a UTC date time value that represents when the message will be visible on the queue.</span></span>

```ruby
message = azure_queue_service.list_messages("test-queue", 30)
pop_receipt, time_next_visible = azure_queue_service.update_message(
  "test-queue", message.id, message.pop_receipt, "updated test message", 
  30)
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a><span data-ttu-id="fa6ac-151">Postupy: Další možnosti pro vyřazení zprávy</span><span class="sxs-lookup"><span data-stu-id="fa6ac-151">How To: Additional Options for Dequeuing Messages</span></span>
<span data-ttu-id="fa6ac-152">Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-152">There are two ways you can customize message retrieval from a queue.</span></span>

1. <span data-ttu-id="fa6ac-153">Můžete načíst dávku zpráv.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-153">You can get a batch of message.</span></span>
2. <span data-ttu-id="fa6ac-154">Můžete nastavit časový limit neviditelnosti delší nebo kratší, aby měl váš kód více nebo méně času na úplné zpracování jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-154">You can set a longer or shorter invisibility timeout, allowing your code more or less time to fully process each message.</span></span>

<span data-ttu-id="fa6ac-155">Následující příklad kódu používá **seznamu\_messages()** metoda získat 15 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-155">The following code example uses the **list\_messages()** method to get 15 messages in one call.</span></span> <span data-ttu-id="fa6ac-156">Pak se vytiskne a odstraní se každá zpráva.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-156">Then it prints and deletes each message.</span></span> <span data-ttu-id="fa6ac-157">Také se pro každou zprávu nastaví časový limit neviditelnosti 5 minut.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-157">It also sets the invisibility timeout to five minutes for each message.</span></span>

```ruby
azure_queue_service.list_messages("test-queue", 300
  {:number_of_messages => 15}).each do |m|
  puts m.message_text
  azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
end
```

## <a name="how-to-get-the-queue-length"></a><span data-ttu-id="fa6ac-158">Postupy: Získání délky fronty</span><span class="sxs-lookup"><span data-stu-id="fa6ac-158">How To: Get the Queue Length</span></span>
<span data-ttu-id="fa6ac-159">Můžete získat odhad počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-159">You can get an estimation of the number of messages in the queue.</span></span> <span data-ttu-id="fa6ac-160">**Získat\_fronty\_metadata()** metoda požádá službu front vrátí počet zpráv přibližnou a metadata o frontě.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-160">The **get\_queue\_metadata()** method asks the queue service to return the approximate message count and metadata about the queue.</span></span>

```ruby
message_count, metadata = azure_queue_service.get_queue_metadata(
  "test-queue")
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="fa6ac-161">Postupy: Odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="fa6ac-161">How To: Delete a Queue</span></span>
<span data-ttu-id="fa6ac-162">Chcete-li odstranit frontu se všemi zprávami, které v ní, zavolejte **odstranit\_queue()** metoda pro objekt fronty.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-162">To delete a queue and all the messages contained in it, call the **delete\_queue()** method on the queue object.</span></span>

```ruby
azure_queue_service.delete_queue("test-queue")
```

## <a name="next-steps"></a><span data-ttu-id="fa6ac-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fa6ac-163">Next Steps</span></span>
<span data-ttu-id="fa6ac-164">Teď, když jste se naučili základy používání služby queue storage, postupujte podle následujících odkazech na další informace o složitějších úlohách úložiště.</span><span class="sxs-lookup"><span data-stu-id="fa6ac-164">Now that you've learned the basics of queue storage, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="fa6ac-165">Přejděte [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)</span><span class="sxs-lookup"><span data-stu-id="fa6ac-165">Visit the [Azure Storage Team Blog](http://blogs.msdn.com/b/windowsazurestorage/)</span></span>
* <span data-ttu-id="fa6ac-166">Přejděte [Azure SDK pro Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) úložišti na Githubu</span><span class="sxs-lookup"><span data-stu-id="fa6ac-166">Visit the [Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>

<span data-ttu-id="fa6ac-167">Porovnání mezi službou Azure fronty popsané v tomto článku a fronty služby Service Bus Azure popsané v [jak používat fronty služby Service Bus](/develop/ruby/how-to-guides/service-bus-queues/) článku najdete v tématu [fronty Azure a fronty služby Service Bus - porovnání a Rozdíl od aktualizovaného](../../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span><span class="sxs-lookup"><span data-stu-id="fa6ac-167">For a comparison between the Azure Queue Service discussed in this article and Azure Service Bus Queues discussed in the [How to use Service Bus Queues](/develop/ruby/how-to-guides/service-bus-queues/) article, see [Azure Queues and Service Bus Queues - Compared and Contrasted](../../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span></span>