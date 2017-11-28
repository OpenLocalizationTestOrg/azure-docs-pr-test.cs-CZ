---
title: "aaaHow toouse úložiště Queue z Ruby | Microsoft Docs"
description: "Zjistěte, jak toouse hello fronty Azure service toocreate a odstranění fronty a vložení, získání a odstranění zprávy. Ukázky napsané v Ruby."
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
ms.openlocfilehash: c8eacac058442419cb9e8fe62cb69ad7ef1e2fc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-ruby"></a><span data-ttu-id="60961-104">Jak toouse úložiště Queue z Ruby</span><span class="sxs-lookup"><span data-stu-id="60961-104">How toouse Queue storage from Ruby</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="60961-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="60961-105">Overview</span></span>
<span data-ttu-id="60961-106">Tento průvodce vám ukáže, jak tooperform běžné scénáře s využitím hello služby Microsoft Azure Queue Storage.</span><span class="sxs-lookup"><span data-stu-id="60961-106">This guide shows you how tooperform common scenarios using hello Microsoft Azure Queue Storage service.</span></span> <span data-ttu-id="60961-107">Ukázky Hello jsou zapsány pomocí hello Ruby rozhraní API Azure.</span><span class="sxs-lookup"><span data-stu-id="60961-107">hello samples are written using hello Ruby Azure API.</span></span>
<span data-ttu-id="60961-108">Hello pokryté scénáře zahrnují **vkládání**, **prohlížení**, **získávání**, a **odstraňování** fronty zpráv, a také  **vytváření a odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="60961-108">hello scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="60961-109">Vytvoření Ruby aplikace</span><span class="sxs-lookup"><span data-stu-id="60961-109">Create a Ruby Application</span></span>
<span data-ttu-id="60961-110">Vytvořte aplikaci pro poznámky Ruby.</span><span class="sxs-lookup"><span data-stu-id="60961-110">Create a Ruby application.</span></span> <span data-ttu-id="60961-111">Pokyny najdete v tématu [Ruby, na které webové aplikace ve virtuálním počítači Azure](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="60961-111">For instructions, see [Ruby on Rails Web application on an Azure VM](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span></span>

## <a name="configure-your-application-tooaccess-storage"></a><span data-ttu-id="60961-112">Nakonfigurujte si aplikace tooAccess úložiště</span><span class="sxs-lookup"><span data-stu-id="60961-112">Configure Your Application tooAccess Storage</span></span>
<span data-ttu-id="60961-113">toouse úložiště Azure, budete potřebovat toodownload a použití hello Ruby balíček azure, který obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="60961-113">toouse Azure storage, you need toodownload and use hello Ruby azure package, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-rubygems-tooobtain-hello-package"></a><span data-ttu-id="60961-114">Použijte RubyGems tooobtain hello balíček</span><span class="sxs-lookup"><span data-stu-id="60961-114">Use RubyGems tooobtain hello package</span></span>
1. <span data-ttu-id="60961-115">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="60961-115">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="60961-116">Zadejte "gem instalace azure" hello příkazového okna tooinstall hello gem a závislosti.</span><span class="sxs-lookup"><span data-stu-id="60961-116">Type "gem install azure" in hello command window tooinstall hello gem and dependencies.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="60961-117">Importovat balíček hello</span><span class="sxs-lookup"><span data-stu-id="60961-117">Import hello package</span></span>
<span data-ttu-id="60961-118">Použít svém oblíbeném textovém editoru, přidejte následující toohello horní části hello Ruby souboru, kde chcete úložiště toouse hello:</span><span class="sxs-lookup"><span data-stu-id="60961-118">Use your favorite text editor, add hello following toohello top of hello Ruby file where you intend toouse storage:</span></span>

```ruby
require "azure"
```

## <a name="setup-an-azure-storage-connection"></a><span data-ttu-id="60961-119">Nastavit připojení k úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="60961-119">Setup an Azure Storage Connection</span></span>
<span data-ttu-id="60961-120">modul Hello azure, bude číst proměnné prostředí hello **AZURE\_úložiště\_účet** a **AZURE\_úložiště\_ACCESS_KEY** pro účet úložiště Azure tooyour tooconnect jsou požadovány informace.</span><span class="sxs-lookup"><span data-stu-id="60961-120">hello azure module will read hello environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS_KEY** for information required tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="60961-121">Pokud nejsou nastavené těchto proměnných prostředí, je nutné zadat informace o účtu hello před použitím **Azure::QueueService** s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="60961-121">If these environment variables are not set, you must specify hello account information before using **Azure::QueueService** with hello following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your Azure storage access key>"
```

<span data-ttu-id="60961-122">tooobtain tyto hodnoty ze Správce prostředků úložiště nebo klasický účet v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="60961-122">tooobtain these values from a classic or Resource Manager storage account in hello Azure portal:</span></span>

1. <span data-ttu-id="60961-123">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="60961-123">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="60961-124">Přejděte toohello úložiště účet, že který má toouse.</span><span class="sxs-lookup"><span data-stu-id="60961-124">Navigate toohello storage account you want toouse.</span></span>
3. <span data-ttu-id="60961-125">V okně Nastavení hello na hello správné, klikněte na tlačítko **přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="60961-125">In hello Settings blade on hello right, click **Access Keys**.</span></span>
4. <span data-ttu-id="60961-126">V okně klíče přístup hello který se zobrazí uvidíte hello přístupový klíč 1 a 2 přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="60961-126">In hello Access keys blade that appears, you'll see hello access key 1 and access key 2.</span></span> <span data-ttu-id="60961-127">Můžete použít kteroukoli z nich.</span><span class="sxs-lookup"><span data-stu-id="60961-127">You can use either of these.</span></span> 
5. <span data-ttu-id="60961-128">Klikněte na tlačítko hello kopírování ikonu toocopy hello klíče toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="60961-128">Click hello copy icon toocopy hello key toohello clipboard.</span></span> 

## <a name="how-to-create-a-queue"></a><span data-ttu-id="60961-129">Postupy: Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="60961-129">How To: Create a Queue</span></span>
<span data-ttu-id="60961-130">Hello následující kód vytvoří **Azure::QueueService** objekt, který vám umožní toowork s fronty.</span><span class="sxs-lookup"><span data-stu-id="60961-130">hello following code creates a **Azure::QueueService** object, which enables you toowork with queues.</span></span>

```ruby
azure_queue_service = Azure::QueueService.new
```

<span data-ttu-id="60961-131">Použití hello **create_queue()** toocreate metoda fronta se hello zadaný název.</span><span class="sxs-lookup"><span data-stu-id="60961-131">Use hello **create_queue()** method toocreate a queue with hello specified name.</span></span>

```ruby
begin
  azure_queue_service.create_queue("test-queue")
rescue
  puts $!
end
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="60961-132">Postupy: Vložit zprávu do fronty</span><span class="sxs-lookup"><span data-stu-id="60961-132">How To: Insert a Message into a Queue</span></span>
<span data-ttu-id="60961-133">tooinsert zprávu do fronty, použijte hello **create_message()** metoda toocreate novou zprávu a přidat ji toohello fronty.</span><span class="sxs-lookup"><span data-stu-id="60961-133">tooinsert a message into a queue, use hello **create_message()** method toocreate a new message and add it toohello queue.</span></span>

```ruby
azure_queue_service.create_message("test-queue", "test message")
```

## <a name="how-to-peek-at-hello-next-message"></a><span data-ttu-id="60961-134">Postupy: Zobrazení náhledu další zprávy hello</span><span class="sxs-lookup"><span data-stu-id="60961-134">How To: Peek at hello Next Message</span></span>
<span data-ttu-id="60961-135">Můžete prohlížet zprávy hello v popředí hello fronty bez odebere ji z fronty hello pomocí volání hello **funkce Náhled\_messages()** metoda.</span><span class="sxs-lookup"><span data-stu-id="60961-135">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **peek\_messages()** method.</span></span> <span data-ttu-id="60961-136">Ve výchozím nastavení **funkce Náhled\_messages()** prohlédne do jedné zprávy.</span><span class="sxs-lookup"><span data-stu-id="60961-136">By default, **peek\_messages()** peeks at a single message.</span></span> <span data-ttu-id="60961-137">Můžete také určit, kolik zpráv, které chcete toopeek.</span><span class="sxs-lookup"><span data-stu-id="60961-137">You can also specify how many messages you want toopeek.</span></span>

```ruby
result = azure_queue_service.peek_messages("test-queue",
  {:number_of_messages => 10})
```

## <a name="how-to-dequeue-hello-next-message"></a><span data-ttu-id="60961-138">Postupy: Dequeue – hello další zprávy</span><span class="sxs-lookup"><span data-stu-id="60961-138">How To: Dequeue hello Next Message</span></span>
<span data-ttu-id="60961-139">Můžete odebrat zprávu z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="60961-139">You can remove a message from a queue in two steps.</span></span>

1. <span data-ttu-id="60961-140">Při volání **seznamu\_messages()**, získat hello další zprávu ve frontě ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="60961-140">When you call **list\_messages()**, you get hello next message in a queue by default.</span></span> <span data-ttu-id="60961-141">Můžete také určit, kolik zpráv, které chcete tooget.</span><span class="sxs-lookup"><span data-stu-id="60961-141">You can also specify how many messages you want tooget.</span></span> <span data-ttu-id="60961-142">Hello zpráv vrácených z **seznamu\_messages()** stane neviditelnou tooany další kód, který čte zprávy z této fronty.</span><span class="sxs-lookup"><span data-stu-id="60961-142">hello messages returned from **list\_messages()** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="60961-143">Můžete předat hello viditelnost vypršení časového limitu v sekundách jako parametr.</span><span class="sxs-lookup"><span data-stu-id="60961-143">You pass in hello visibility timeout in seconds as a parameter.</span></span>
2. <span data-ttu-id="60961-144">toofinish odebírání uvítací zprávu z fronty hello, musíte také zavolat **delete_message()**.</span><span class="sxs-lookup"><span data-stu-id="60961-144">toofinish removing hello message from hello queue, you must also call **delete_message()**.</span></span>

<span data-ttu-id="60961-145">Tento dvoukrokový proces odebrání zprávy zaručuje, když vaše tooprocess kódu nezdaří, které můžete získat zprávu z důvodu selhání toohardware nebo softwaru, jiná instance vašeho kódu hello stejné zprávy a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="60961-145">This two-step process of removing a message assures that when your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="60961-146">Váš kód zavolá metodu **odstranit\_message()** hned po zpracování zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="60961-146">Your code calls **delete\_message()** right after hello message has been processed.</span></span>

```ruby
messages = azure_queue_service.list_messages("test-queue", 30)
azure_queue_service.delete_message("test-queue", 
  messages[0].id, messages[0].pop_receipt)
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a><span data-ttu-id="60961-147">Postupy: Změna hello obsah zprávy ve frontě</span><span class="sxs-lookup"><span data-stu-id="60961-147">How To: Change hello Contents of a Queued Message</span></span>
<span data-ttu-id="60961-148">Můžete změnit obsah zprávy přímo ve frontě hello hello.</span><span class="sxs-lookup"><span data-stu-id="60961-148">You can change hello contents of a message in-place in hello queue.</span></span> <span data-ttu-id="60961-149">Následující kód Hello používá hello **update_message()** metoda tooupdate zprávu.</span><span class="sxs-lookup"><span data-stu-id="60961-149">hello code below uses hello **update_message()** method tooupdate a message.</span></span> <span data-ttu-id="60961-150">Hello metoda vrátí řazené kolekce členů, která obsahuje hello pop přijetí zprávy fronty hello a hodnotu Datum čas UTC, která představuje při uvítací zprávu bude zobrazovat na hello fronty.</span><span class="sxs-lookup"><span data-stu-id="60961-150">hello method will return a tuple which contains hello pop receipt of hello queue message and a UTC date time value that represents when hello message will be visible on hello queue.</span></span>

```ruby
message = azure_queue_service.list_messages("test-queue", 30)
pop_receipt, time_next_visible = azure_queue_service.update_message(
  "test-queue", message.id, message.pop_receipt, "updated test message", 
  30)
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a><span data-ttu-id="60961-151">Postupy: Další možnosti pro vyřazení zprávy</span><span class="sxs-lookup"><span data-stu-id="60961-151">How To: Additional Options for Dequeuing Messages</span></span>
<span data-ttu-id="60961-152">Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="60961-152">There are two ways you can customize message retrieval from a queue.</span></span>

1. <span data-ttu-id="60961-153">Můžete načíst dávku zpráv.</span><span class="sxs-lookup"><span data-stu-id="60961-153">You can get a batch of message.</span></span>
2. <span data-ttu-id="60961-154">Můžete nastavit časový limit neviditelnosti delší nebo kratší, povolení váš kód více nebo méně času toofully zpracování jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="60961-154">You can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span>

<span data-ttu-id="60961-155">Hello následující příklad kódu používá hello **seznamu\_messages()** metoda tooget 15 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="60961-155">hello following code example uses hello **list\_messages()** method tooget 15 messages in one call.</span></span> <span data-ttu-id="60961-156">Pak se vytiskne a odstraní se každá zpráva.</span><span class="sxs-lookup"><span data-stu-id="60961-156">Then it prints and deletes each message.</span></span> <span data-ttu-id="60961-157">Nastaví taky hello neviditelnosti časový limit toofive minut pro každou zprávu.</span><span class="sxs-lookup"><span data-stu-id="60961-157">It also sets hello invisibility timeout toofive minutes for each message.</span></span>

```ruby
azure_queue_service.list_messages("test-queue", 300
  {:number_of_messages => 15}).each do |m|
  puts m.message_text
  azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
end
```

## <a name="how-to-get-hello-queue-length"></a><span data-ttu-id="60961-158">Postupy: Získání hello délka fronty</span><span class="sxs-lookup"><span data-stu-id="60961-158">How To: Get hello Queue Length</span></span>
<span data-ttu-id="60961-159">Můžete získat odhad hello počet zpráv ve frontě hello.</span><span class="sxs-lookup"><span data-stu-id="60961-159">You can get an estimation of hello number of messages in hello queue.</span></span> <span data-ttu-id="60961-160">Hello **získat\_fronty\_metadata()** metoda zeptá na počet hello fronty služby tooreturn hello přibližnou zpráv a metadata o hello fronty.</span><span class="sxs-lookup"><span data-stu-id="60961-160">hello **get\_queue\_metadata()** method asks hello queue service tooreturn hello approximate message count and metadata about hello queue.</span></span>

```ruby
message_count, metadata = azure_queue_service.get_queue_metadata(
  "test-queue")
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="60961-161">Postupy: Odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="60961-161">How To: Delete a Queue</span></span>
<span data-ttu-id="60961-162">toodelete frontu a všechny zprávy hello obsažené v něm volání hello **odstranit\_queue()** metoda na objekt fronty hello.</span><span class="sxs-lookup"><span data-stu-id="60961-162">toodelete a queue and all hello messages contained in it, call hello **delete\_queue()** method on hello queue object.</span></span>

```ruby
azure_queue_service.delete_queue("test-queue")
```

## <a name="next-steps"></a><span data-ttu-id="60961-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60961-163">Next Steps</span></span>
<span data-ttu-id="60961-164">Teď, když jste se naučili základy hello fronty úložiště, postupujte podle těchto odkazů toolearn o složitějších úlohách úložiště.</span><span class="sxs-lookup"><span data-stu-id="60961-164">Now that you've learned hello basics of queue storage, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="60961-165">Navštivte hello [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)</span><span class="sxs-lookup"><span data-stu-id="60961-165">Visit hello [Azure Storage Team Blog](http://blogs.msdn.com/b/windowsazurestorage/)</span></span>
* <span data-ttu-id="60961-166">Navštivte hello [Azure SDK pro Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) úložišti na Githubu</span><span class="sxs-lookup"><span data-stu-id="60961-166">Visit hello [Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>

<span data-ttu-id="60961-167">Porovnání mezi hello službu front Azure popsané v tomto článku a fronty služby Service Bus Azure popsané v hello [jak toouse fronty služby Service Bus](/develop/ruby/how-to-guides/service-bus-queues/) článku najdete v tématu [fronty Azure a fronty služby Service Bus - porovnání a Rozdíl od aktualizovaného](../../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span><span class="sxs-lookup"><span data-stu-id="60961-167">For a comparison between hello Azure Queue Service discussed in this article and Azure Service Bus Queues discussed in hello [How toouse Service Bus Queues](/develop/ruby/how-to-guides/service-bus-queues/) article, see [Azure Queues and Service Bus Queues - Compared and Contrasted](../../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span></span>
