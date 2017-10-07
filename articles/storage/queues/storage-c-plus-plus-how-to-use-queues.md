---
title: "aaaHow toouse fronty úložiště (C++) | Microsoft Docs"
description: "Zjistěte, jak toouse hello fronty služby úložiště v Azure. Ukázky jsou napsané v jazyce C++."
services: storage
documentationcenter: .net
author: cbrooksmsft
manager: jahogg
editor: tysonn
ms.assetid: c8a36365-29f6-404d-8fd1-858a7f33b50a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 05/11/2017
ms.author: cbrooksmsft
ms.openlocfilehash: b0cddf017878e9fab87f47d24b2906e40c9f4ad5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-c"></a><span data-ttu-id="2ef89-104">Jak toouse Queue Storage z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="2ef89-104">How toouse Queue Storage from C++</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="2ef89-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="2ef89-105">Overview</span></span>
<span data-ttu-id="2ef89-106">Tento průvodce vám ukáže, jak hello tooperform běžné scénáře s využitím služby Azure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="2ef89-106">This guide will show you how tooperform common scenarios using hello Azure Queue storage service.</span></span> <span data-ttu-id="2ef89-107">Hello ukázky jsou napsané v jazyce C++ a používají hello [Klientská knihovna pro úložiště Azure pro jazyk C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="2ef89-107">hello samples are written in C++ and use hello [Azure Storage Client Library for C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span></span> <span data-ttu-id="2ef89-108">Hello pokryté scénáře zahrnují **vkládání**, **prohlížení**, **získávání**, a **odstraňování** fronty zpráv, a také  **vytváření a odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="2ef89-108">hello scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span>

> [!NOTE]
> <span data-ttu-id="2ef89-109">Tato příručka cílí hello Klientská knihovna pro úložiště Azure pro jazyk C++ verze 1.0.0 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="2ef89-109">This guide targets hello Azure Storage Client Library for C++ version 1.0.0 and above.</span></span> <span data-ttu-id="2ef89-110">Hello doporučená verze je klientská knihovna pro úložiště 2.2.0, která je k dispozici prostřednictvím [NuGet](http://www.nuget.org/packages/wastorage) nebo [Githubu](http://github.com/Azure/azure-storage-cpp/).</span><span class="sxs-lookup"><span data-stu-id="2ef89-110">hello recommended version is Storage Client Library 2.2.0, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](http://github.com/Azure/azure-storage-cpp/).</span></span>
> 
> 

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a><span data-ttu-id="2ef89-111">Vytvoření aplikace C++</span><span class="sxs-lookup"><span data-stu-id="2ef89-111">Create a C++ application</span></span>
<span data-ttu-id="2ef89-112">V této příručce bude používat funkce úložiště, které lze spustit v rámci aplikace C++.</span><span class="sxs-lookup"><span data-stu-id="2ef89-112">In this guide, you will use storage features which can be run within a C++ application.</span></span>

<span data-ttu-id="2ef89-113">toodo tedy budete potřebovat tooinstall hello Klientská knihovna pro úložiště Azure pro jazyk C++ a vytvoření účtu úložiště Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="2ef89-113">toodo so, you will need tooinstall hello Azure Storage Client Library for C++ and create an Azure storage account in your Azure subscription.</span></span>

<span data-ttu-id="2ef89-114">tooinstall hello Klientská knihovna pro úložiště Azure pro jazyk C++, můžete použít následující metody hello:</span><span class="sxs-lookup"><span data-stu-id="2ef89-114">tooinstall hello Azure Storage Client Library for C++, you can use hello following methods:</span></span>

* <span data-ttu-id="2ef89-115">**Linux:** postupujte podle pokynů hello uvedenému v hello [Klientská knihovna pro úložiště Azure pro C++ – soubor README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) stránky.</span><span class="sxs-lookup"><span data-stu-id="2ef89-115">**Linux:** Follow hello instructions given in hello [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>
* <span data-ttu-id="2ef89-116">**Windows:** v sadě Visual Studio, klikněte na tlačítko **nástroje > Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="2ef89-116">**Windows:** In Visual Studio, click **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="2ef89-117">Typ hello následující příkaz do hello [Konzola správce balíčků NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="2ef89-117">Type hello following command into hello [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>

```  
Install-Package wastorage
```

## <a name="configure-your-application-tooaccess-queue-storage"></a><span data-ttu-id="2ef89-118">Konfigurace vaší aplikace tooaccess Queue Storage</span><span class="sxs-lookup"><span data-stu-id="2ef89-118">Configure your application tooaccess Queue Storage</span></span>
<span data-ttu-id="2ef89-119">Přidáte následující hello obsahovat toohello příkazy na začátek souboru C++ hello místo toouse hello úložiště Azure rozhraní API tooaccess fronty:</span><span class="sxs-lookup"><span data-stu-id="2ef89-119">Add hello following include statements toohello top of hello C++ file where you want toouse hello Azure storage APIs tooaccess queues:</span></span>  

```cpp
#include <was/storage_account.h>
#include <was/queue.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="2ef89-120">Nastavit připojovací řetězec úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="2ef89-120">Set up an Azure storage connection string</span></span>
<span data-ttu-id="2ef89-121">Klienta Azure storage používá úložiště připojovací řetězec toostore koncových bodů a přihlašovací údaje pro přístup ke službám dat správy.</span><span class="sxs-lookup"><span data-stu-id="2ef89-121">An Azure storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="2ef89-122">Když spustíte v aplikaci klienta, je nutné zadat připojovací řetězec úložiště hello v hello formátu, pomocí hello název účtu a hello úložiště přístupový klíč k úložišti pro účet úložiště hello uvedené v hello [portálu Azure](https://portal.azure.com)pro hello *AccountName* a *AccountKey* hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2ef89-122">When running in a client application, you must provide hello storage connection string in hello following format, using hello name of your storage account and hello storage access key for hello storage account listed in hello [Azure Portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="2ef89-123">Informace o účtech úložiště a přístupové klávesy, najdete v části [o účtech úložiště Azure](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2ef89-123">For information on storage accounts and access keys, see [About Azure Storage Accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).</span></span> <span data-ttu-id="2ef89-124">Tento příklad ukazuje, jak můžou deklarovat statické pole toohold hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="2ef89-124">This example shows how you can declare a static field toohold hello connection string:</span></span>  

```cpp
// Define hello connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

<span data-ttu-id="2ef89-125">tootest aplikace v místním počítači s Windows, můžete použít hello Microsoft Azure [emulátor úložiště](../common/storage-use-emulator.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) který se instaluje s hello [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="2ef89-125">tootest your application in your local Windows computer, you can use hello Microsoft Azure [storage emulator](../common/storage-use-emulator.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) that is installed with hello [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="2ef89-126">emulátor úložiště Hello je nástroj, který simuluje hello objektů Blob, Queue a Table služby v Azure k dispozici na místním vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="2ef89-126">hello storage emulator is a utility that simulates hello Blob, Queue, and Table services available in Azure on your local development machine.</span></span> <span data-ttu-id="2ef89-127">Hello následující příklad ukazuje, jak lze deklarovat statické pole toohold hello připojovací řetězec tooyour emulátor místního úložiště:</span><span class="sxs-lookup"><span data-stu-id="2ef89-127">hello following example shows how you can declare a static field toohold hello connection string tooyour local storage emulator:</span></span>  

```cpp
// Define hello connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

<span data-ttu-id="2ef89-128">emulátor úložiště Azure toostart hello, vyberte hello **spustit** tlačítko nebo klikněte na tlačítko hello **Windows** klíč.</span><span class="sxs-lookup"><span data-stu-id="2ef89-128">toostart hello Azure storage emulator, select hello **Start** button or press hello **Windows** key.</span></span> <span data-ttu-id="2ef89-129">Začněte psát **emulátoru úložiště Azure**a vyberte **emulátor úložiště Microsoft Azure** hello seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="2ef89-129">Begin typing **Azure Storage Emulator**, and select **Microsoft Azure Storage Emulator** from hello list of applications.</span></span>

<span data-ttu-id="2ef89-130">Hello následující ukázky předpokládejme použít jednu z těchto dvou metod tooget hello úložiště připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="2ef89-130">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>

## <a name="retrieve-your-connection-string"></a><span data-ttu-id="2ef89-131">Načtení připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="2ef89-131">Retrieve your connection string</span></span>
<span data-ttu-id="2ef89-132">Můžete použít hello **cloud_storage_account** třídy toorepresent informací o vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ef89-132">You can use hello **cloud_storage_account** class toorepresent your Storage Account information.</span></span> <span data-ttu-id="2ef89-133">tooretrieve účet úložiště informací z připojovacího řetězce úložiště hello, můžete použít hello **analyzovat** metoda.</span><span class="sxs-lookup"><span data-stu-id="2ef89-133">tooretrieve your storage account information from hello storage connection string, you can use hello **parse** method.</span></span>

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="how-to-create-a-queue"></a><span data-ttu-id="2ef89-134">Postupy: vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="2ef89-134">How to: Create a queue</span></span>
<span data-ttu-id="2ef89-135">A **cloud_queue_client** objektu umožňuje získat odkaz na objekty pro fronty.</span><span class="sxs-lookup"><span data-stu-id="2ef89-135">A **cloud_queue_client** object lets you get reference objects for queues.</span></span> <span data-ttu-id="2ef89-136">Hello následující kód vytvoří **cloud_queue_client** objektu.</span><span class="sxs-lookup"><span data-stu-id="2ef89-136">hello following code creates a **cloud_queue_client** object.</span></span>

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create a queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();
```

<span data-ttu-id="2ef89-137">Použití hello **cloud_queue_client** tooget chcete toouse fronty toohello odkaz na objekt.</span><span class="sxs-lookup"><span data-stu-id="2ef89-137">Use hello **cloud_queue_client** object tooget a reference toohello queue you want toouse.</span></span> <span data-ttu-id="2ef89-138">Pokud neexistuje, můžete vytvořit hello fronty.</span><span class="sxs-lookup"><span data-stu-id="2ef89-138">You can create hello queue if it doesn't exist.</span></span>

```cpp
// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create hello queue if it doesn't already exist.
 queue.create_if_not_exists();  
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="2ef89-139">Postupy: vložit zprávu do fronty</span><span class="sxs-lookup"><span data-stu-id="2ef89-139">How to: Insert a message into a queue</span></span>
<span data-ttu-id="2ef89-140">tooinsert zprávu do existující fronty, vytvořte nejdříve novou **cloud_queue_message**.</span><span class="sxs-lookup"><span data-stu-id="2ef89-140">tooinsert a message into an existing queue, first create a new **cloud_queue_message**.</span></span> <span data-ttu-id="2ef89-141">Pak zavolejte hello **add_message** metoda.</span><span class="sxs-lookup"><span data-stu-id="2ef89-141">Next, call hello **add_message** method.</span></span> <span data-ttu-id="2ef89-142">A **cloud_queue_message** můžete vytvořit buď z řetězce nebo **bajtů** pole.</span><span class="sxs-lookup"><span data-stu-id="2ef89-142">A **cloud_queue_message** can be created from either a string or a **byte** array.</span></span> <span data-ttu-id="2ef89-143">Zde je kód, který vytvoří frontu (pokud neexistuje) a vloží uvítací zprávu "Hello, World":</span><span class="sxs-lookup"><span data-stu-id="2ef89-143">Here is code which creates a queue (if it doesn't exist) and inserts hello message 'Hello, World':</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create hello queue if it doesn't already exist.
queue.create_if_not_exists();

// Create a message and add it toohello queue.
azure::storage::cloud_queue_message message1(U("Hello, World"));
queue.add_message(message1);  
```

## <a name="how-to-peek-at-hello-next-message"></a><span data-ttu-id="2ef89-144">Postupy: zobrazení náhledu další zprávy hello</span><span class="sxs-lookup"><span data-stu-id="2ef89-144">How to: Peek at hello next message</span></span>
<span data-ttu-id="2ef89-145">Můžete prohlížet zprávy hello v popředí hello fronty bez odebere ji z fronty hello pomocí volání hello **peek_message** metoda.</span><span class="sxs-lookup"><span data-stu-id="2ef89-145">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **peek_message** method.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Peek at hello next message.
azure::storage::cloud_queue_message peeked_message = queue.peek_message();

// Output hello message content.
std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a><span data-ttu-id="2ef89-146">Postupy: Změna obsahu zpráv zařazených ve frontě hello</span><span class="sxs-lookup"><span data-stu-id="2ef89-146">How to: Change hello contents of a queued message</span></span>
<span data-ttu-id="2ef89-147">Můžete změnit obsah zprávy přímo ve frontě hello hello.</span><span class="sxs-lookup"><span data-stu-id="2ef89-147">You can change hello contents of a message in-place in hello queue.</span></span> <span data-ttu-id="2ef89-148">Pokud hello zpráva představuje pracovní úlohu, můžete použít tuto funkci tooupdate hello stav hello pracovní úlohy.</span><span class="sxs-lookup"><span data-stu-id="2ef89-148">If hello message represents a work task, you could use this feature tooupdate hello status of hello work task.</span></span> <span data-ttu-id="2ef89-149">Hello následující kód aktualizuje zprávy fronty hello nový obsah, a nastaví hello tooextend časový limit viditelnosti jiné 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="2ef89-149">hello following code updates hello queue message with new contents, and sets hello visibility timeout tooextend another 60 seconds.</span></span> <span data-ttu-id="2ef89-150">Uloží hello stav práce spojený s uvítací zprávu a poskytuje další minutu toocontinue pracující na uvítací zprávu klienta hello.</span><span class="sxs-lookup"><span data-stu-id="2ef89-150">This saves hello state of work associated with hello message, and gives hello client another minute toocontinue working on hello message.</span></span> <span data-ttu-id="2ef89-151">Můžete použít tento postup tootrack vícekrokového pracovní postupy pro zprávy ve frontě, bez nutnosti toostart přes od začátku hello, pokud se nezdaří krok zpracování z důvodu selhání toohardware nebo softwaru.</span><span class="sxs-lookup"><span data-stu-id="2ef89-151">You could use this technique tootrack multi-step workflows on queue messages, without having toostart over from hello beginning if a processing step fails due toohardware or software failure.</span></span> <span data-ttu-id="2ef89-152">Obvykle byste udržovali také počet opakování, a pokud uvítací zprávu se pokus o více než n krát, odstranili byste ji.</span><span class="sxs-lookup"><span data-stu-id="2ef89-152">Typically, you would keep a retry count as well, and if hello message is retried more than n times, you would delete it.</span></span> <span data-ttu-id="2ef89-153">Je to ochrana proti tomu, aby zpráva při každém pokusu o zpracování nevyvolala chyby aplikace.</span><span class="sxs-lookup"><span data-stu-id="2ef89-153">This protects against a message that triggers an application error each time it is processed.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get hello message from hello queue and update hello message contents.
// hello visibility timeout "0" means make it visible immediately.
// hello visibility timeout "60" means hello client can get another minute toocontinue
// working on hello message.
azure::storage::cloud_queue_message changed_message = queue.get_message();

changed_message.set_content(U("Changed message"));
queue.update_message(changed_message, std::chrono::seconds(60), true);

// Output hello message content.
std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  
```

## <a name="how-to-de-queue-hello-next-message"></a><span data-ttu-id="2ef89-154">Postupy: zrušte hello další zprávu ve frontě</span><span class="sxs-lookup"><span data-stu-id="2ef89-154">How to: De-queue hello next message</span></span>
<span data-ttu-id="2ef89-155">Váš kód vyřazuje zprávy z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="2ef89-155">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="2ef89-156">Při volání **get_message**, získat hello další zprávu ve frontě.</span><span class="sxs-lookup"><span data-stu-id="2ef89-156">When you call **get_message**, you get hello next message in a queue.</span></span> <span data-ttu-id="2ef89-157">Zpráva vrácená metodou **get_message** stane neviditelnou tooany další kód, který čte zprávy z této fronty.</span><span class="sxs-lookup"><span data-stu-id="2ef89-157">A message returned from **get_message** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="2ef89-158">toofinish odebírání uvítací zprávu z fronty hello, musíte také zavolat **delete_message**.</span><span class="sxs-lookup"><span data-stu-id="2ef89-158">toofinish removing hello message from hello queue, you must also call **delete_message**.</span></span> <span data-ttu-id="2ef89-159">Tento dvoukrokový proces odebrání zprávy zaručuje, pokud se váš kód nezdaří tooprocess, které můžete získat zprávu z důvodu selhání toohardware nebo softwaru, jiná instance vašeho kódu hello stejnou zprávu a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="2ef89-159">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="2ef89-160">Váš kód zavolá metodu **delete_message** hned po zpracování zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="2ef89-160">Your code calls **delete_message** right after hello message has been processed.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get hello next message.
azure::storage::cloud_queue_message dequeued_message = queue.get_message();
std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

// Delete hello message.
queue.delete_message(dequeued_message);
```

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a><span data-ttu-id="2ef89-161">Postupy: využívání dalších možností pro vyřazování zpráv z fronty</span><span class="sxs-lookup"><span data-stu-id="2ef89-161">How to: Leverage additional options for de-queuing messages</span></span>
<span data-ttu-id="2ef89-162">Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="2ef89-162">There are two ways you can customize message retrieval from a queue.</span></span> <span data-ttu-id="2ef89-163">Nejprve můžete načíst dávku zpráv (až too32).</span><span class="sxs-lookup"><span data-stu-id="2ef89-163">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="2ef89-164">Druhý můžete nastavit časový limit neviditelnosti delší nebo kratší, povolení váš kód více nebo méně času toofully zpracování jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="2ef89-164">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="2ef89-165">Hello následující příklad kódu používá hello **get_messages** metoda tooget 20 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="2ef89-165">hello following code example uses hello **get_messages** method tooget 20 messages in one call.</span></span> <span data-ttu-id="2ef89-166">Pak se každá zpráva zpracuje pomocí **pro** smyčky.</span><span class="sxs-lookup"><span data-stu-id="2ef89-166">Then it processes each message using a **for** loop.</span></span> <span data-ttu-id="2ef89-167">Nastaví taky hello neviditelnosti časový limit toofive minut pro každou zprávu.</span><span class="sxs-lookup"><span data-stu-id="2ef89-167">It also sets hello invisibility timeout toofive minutes for each message.</span></span> <span data-ttu-id="2ef89-168">Všimněte si, že hello 5 minut spustí pro všechny zprávy s hello stejný čas, takže po 5 minut od volání hello příliš předané**get_messages**, všechny zprávy, které nebyly odstraněny, opět viditelné.</span><span class="sxs-lookup"><span data-stu-id="2ef89-168">Note that hello 5 minutes starts for all messages at hello same time, so after 5 minutes have passed since hello call too**get_messages**, any messages which have not been deleted will become visible again.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
// 5 minutes (300 seconds).
azure::storage::queue_request_options options;
azure::storage::operation_context context;

// Retrieve 20 messages from hello queue with a visibility timeout of 300 seconds.
std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

for (auto it = messages.cbegin(); it != messages.cend(); ++it)
{
    // Display hello contents of hello message.
    std::wcout << U("Get: ") << it->content_as_string() << std::endl;
}
```

## <a name="how-to-get-hello-queue-length"></a><span data-ttu-id="2ef89-169">Postupy: získání délky fronty hello</span><span class="sxs-lookup"><span data-stu-id="2ef89-169">How to: Get hello queue length</span></span>
<span data-ttu-id="2ef89-170">Můžete získat odhad hello počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="2ef89-170">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="2ef89-171">Hello **download_attributes** metoda požádá hello fronty služby tooretrieve hello atributů fronty, včetně počtu zpráv hello.</span><span class="sxs-lookup"><span data-stu-id="2ef89-171">hello **download_attributes** method asks hello Queue service tooretrieve hello queue attributes, including hello message count.</span></span> <span data-ttu-id="2ef89-172">Hello **approximate_message_count** metoda získá hello přibližný počet zpráv ve frontě hello.</span><span class="sxs-lookup"><span data-stu-id="2ef89-172">hello **approximate_message_count** method gets hello approximate number of messages in hello queue.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Fetch hello queue attributes.
queue.download_attributes();

// Retrieve hello cached approximate message count.
int cachedMessageCount = queue.approximate_message_count();

// Display number of messages.
std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="2ef89-173">Postupy: odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="2ef89-173">How to: Delete a queue</span></span>
<span data-ttu-id="2ef89-174">fronty a všechny zprávy hello obsažené v něm volání hello toodelete **delete_queue_if_exists** metoda na objekt fronty hello.</span><span class="sxs-lookup"><span data-stu-id="2ef89-174">toodelete a queue and all hello messages contained in it, call hello **delete_queue_if_exists** method on hello queue object.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// If hello queue exists and delete it.
queue.delete_queue_if_exists();  
```

## <a name="next-steps"></a><span data-ttu-id="2ef89-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2ef89-175">Next steps</span></span>
<span data-ttu-id="2ef89-176">Teď, když jste se naučili základy hello fronty úložiště, postupujte podle těchto odkazů toolearn Další informace o Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="2ef89-176">Now that you've learned hello basics of Queue storage, follow these links toolearn more about Azure Storage.</span></span>

* [<span data-ttu-id="2ef89-177">Jak toouse úložiště objektů Blob z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="2ef89-177">How toouse Blob Storage from C++</span></span>](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="2ef89-178">Jak toouse úložiště Table z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="2ef89-178">How toouse Table Storage from C++</span></span>](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [<span data-ttu-id="2ef89-179">Seznam prostředků úložiště Azure v jazyce C++</span><span class="sxs-lookup"><span data-stu-id="2ef89-179">List Azure Storage Resources in C++</span></span>](../common/storage-c-plus-plus-enumeration.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [<span data-ttu-id="2ef89-180">Klientská knihovna pro úložiště pro C++ – referenční informace</span><span class="sxs-lookup"><span data-stu-id="2ef89-180">Storage Client Library for C++ Reference</span></span>](http://azure.github.io/azure-storage-cpp)
* [<span data-ttu-id="2ef89-181">Dokumentace k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2ef89-181">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)