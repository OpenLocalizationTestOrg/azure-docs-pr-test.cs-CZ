---
title: "aaaHow toouse úložiště Queue z Javy | Microsoft Docs"
description: "Zjistěte, jak toouse hello fronty Azure service toocreate a odstranění fronty a vložení, získání a odstranění zprávy. Ukázky napsanou v jazyce Java."
services: storage
documentationcenter: java
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 68cecc8e-38c9-4a24-99e8-cb722bc63cf9
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 297f89c9d21a38d2b4a5f4346f66f59f9d487010
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-java"></a><span data-ttu-id="8b3e2-104">Jak toouse úložiště Queue z Javy</span><span class="sxs-lookup"><span data-stu-id="8b3e2-104">How toouse Queue storage from Java</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a><span data-ttu-id="8b3e2-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="8b3e2-105">Overview</span></span>
<span data-ttu-id="8b3e2-106">Tento průvodce vám ukáže, jak hello tooperform běžné scénáře s využitím služby Azure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-106">This guide will show you how tooperform common scenarios using hello Azure Queue storage service.</span></span> <span data-ttu-id="8b3e2-107">Hello ukázky jsou napsané v jazyce Java a používají hello [sada SDK úložiště Azure pro jazyk Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="8b3e2-107">hello samples are written in Java and use hello [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="8b3e2-108">Hello pokryté scénáře zahrnují **vkládání**, **prohlížení**, **získávání**, a **odstraňování** fronty zpráv, a také  **vytváření** a **odstraňování** fronty.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-108">hello scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating** and **deleting** queues.</span></span> <span data-ttu-id="8b3e2-109">Další informace o frontách najdete v části hello [další kroky](#Next-Steps) části.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-109">For more information on queues, see hello [Next steps](#Next-Steps) section.</span></span>

<span data-ttu-id="8b3e2-110">Poznámka: Sada SDK je k dispozici pro vývojáře, kteří na zařízení se systémem Android používají Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-110">Note: An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="8b3e2-111">Další informace najdete v tématu hello [sada SDK úložiště Azure pro Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="8b3e2-111">For more information, see hello [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="8b3e2-112">Vytvoření aplikace Java</span><span class="sxs-lookup"><span data-stu-id="8b3e2-112">Create a Java application</span></span>
<span data-ttu-id="8b3e2-113">V této příručce bude používat funkce úložiště, které lze spustit v rámci aplikace Java místně nebo v kódu běžící v rámci webovou roli nebo role pracovního procesu v Azure.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-113">In this guide, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="8b3e2-114">toodo tedy budete potřebovat tooinstall hello Java Development Kit (JDK) a vytvořit účet úložiště Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-114">toodo so, you will need tooinstall hello Java Development Kit (JDK) and create an Azure storage account in your Azure subscription.</span></span> <span data-ttu-id="8b3e2-115">Jakmile provedete, budete potřebovat tooverify, který váš vývojový systém splňuje minimální požadavky hello a závislosti, které jsou uvedeny v hello [sada SDK úložiště Azure pro jazyk Java] [ Azure Storage SDK for Java] úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-115">Once you have done so, you will need tooverify that your development system meets hello minimum requirements and dependencies which are listed in hello [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="8b3e2-116">Pokud váš systém splňuje tyto požadavky, můžete postupovat podle pokynů hello ke stažení a instalaci hello knihovny úložiště Azure pro jazyk Java systému z tohoto úložiště.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-116">If your system meets those requirements, you can follow hello instructions for downloading and installing hello Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="8b3e2-117">Po dokončení těchto úloh, bude možné toocreate aplikaci Java, která používá hello příklady v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-117">Once you have completed those tasks, you will be able toocreate a Java application which uses hello examples in this article.</span></span>

## <a name="configure-your-application-tooaccess-queue-storage"></a><span data-ttu-id="8b3e2-118">Konfigurace vaší aplikace tooaccess fronty úložiště</span><span class="sxs-lookup"><span data-stu-id="8b3e2-118">Configure your application tooaccess queue storage</span></span>
<span data-ttu-id="8b3e2-119">Přidejte následující import příkazy toohello horní části souboru Java hello místo toouse úložiště Azure rozhraní API tooaccess fronty hello:</span><span class="sxs-lookup"><span data-stu-id="8b3e2-119">Add hello following import statements toohello top of hello Java file where you want toouse Azure storage APIs tooaccess queues:</span></span>

```java
// Include hello following imports toouse queue APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.queue.*;
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="8b3e2-120">Instalační program připojovací řetězec úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="8b3e2-120">Setup an Azure storage connection string</span></span>
<span data-ttu-id="8b3e2-121">Klienta Azure storage používá úložiště připojovací řetězec toostore koncových bodů a přihlašovací údaje pro přístup ke službám dat správy.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-121">An Azure storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="8b3e2-122">Když spustíte v aplikaci klienta, musíte zadat připojovací řetězec úložiště hello v hello formátu, pomocí hello názvu účtu úložiště a hello primární přístupový klíč pro účet úložiště hello uvedené v hello [portálu Azure](https://portal.azure.com)pro hello *AccountName* a *AccountKey* hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-122">When running in a client application, you must provide hello storage connection string in hello following format, using hello name of your storage account and hello Primary access key for hello storage account listed in hello [Azure Portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="8b3e2-123">Tento příklad ukazuje, jak můžou deklarovat statické pole toohold hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="8b3e2-123">This example shows how you can declare a static field toohold hello connection string:</span></span>

```java
// Define hello connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="8b3e2-124">Ve spuštěné aplikace v rámci role ve službě Microsoft Azure, můžete tento řetězec uložené v konfiguračním souboru služby hello, *souboru ServiceConfiguration.cscfg*a je přístupný pomocí volání toohello  **RoleEnvironment.getConfigurationSettings** metoda.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-124">In an application running within a role in Microsoft Azure, this string can be stored in hello service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call toohello **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="8b3e2-125">Tady je příklad získávání hello připojovacího řetězce z **nastavení** element s názvem *StorageConnectionString* v konfiguračním souboru služby hello:</span><span class="sxs-lookup"><span data-stu-id="8b3e2-125">Here's an example of getting hello connection string from a **Setting** element named *StorageConnectionString* in hello service configuration file:</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="8b3e2-126">Hello následující ukázky předpokládejme použít jednu z těchto dvou metod tooget hello úložiště připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-126">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>

## <a name="how-to-create-a-queue"></a><span data-ttu-id="8b3e2-127">Postupy: vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="8b3e2-127">How to: Create a queue</span></span>
<span data-ttu-id="8b3e2-128">A **CloudQueueClient** objektu umožňuje získat odkaz na objekty pro fronty.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-128">A **CloudQueueClient** object lets you get reference objects for queues.</span></span> <span data-ttu-id="8b3e2-129">Hello následující kód vytvoří **CloudQueueClient** objektu.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-129">hello following code creates a **CloudQueueClient** object.</span></span> <span data-ttu-id="8b3e2-130">(Poznámka: existují další způsoby toocreate **CloudStorageAccount** objekty; Další informace najdete v tématu **CloudStorageAccount** v hello [Azure úložiště klienta SDK Reference].)</span><span class="sxs-lookup"><span data-stu-id="8b3e2-130">(Note: There are additional ways toocreate **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in hello [Azure Storage Client SDK Reference].)</span></span>

<span data-ttu-id="8b3e2-131">Použití hello **CloudQueueClient** tooget chcete toouse fronty toohello odkaz na objekt.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-131">Use hello **CloudQueueClient** object tooget a reference toohello queue you want toouse.</span></span> <span data-ttu-id="8b3e2-132">Pokud neexistuje, můžete vytvořit hello fronty.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-132">You can create hello queue if it doesn't exist.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

   // Create hello queue client.
   CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

   // Retrieve a reference tooa queue.
   CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Create hello queue if it doesn't already exist.
   queue.createIfNotExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-a-message-tooa-queue"></a><span data-ttu-id="8b3e2-133">Postupy: Přidání tooa fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="8b3e2-133">How to: Add a message tooa queue</span></span>
<span data-ttu-id="8b3e2-134">tooinsert zprávu do existující fronty, vytvořte nejdříve novou **CloudQueueMessage**.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-134">tooinsert a message into an existing queue, first create a new **CloudQueueMessage**.</span></span> <span data-ttu-id="8b3e2-135">Pak zavolejte hello **addMessage** metoda.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-135">Next, call hello **addMessage** method.</span></span> <span data-ttu-id="8b3e2-136">A **CloudQueueMessage** lze vytvořit z řetězce (ve formátu UTF-8) nebo pole bajtů.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-136">A **CloudQueueMessage** can be created from either a string (in UTF-8 format) or a byte array.</span></span> <span data-ttu-id="8b3e2-137">Zde je kód, který vytvoří frontu (pokud neexistuje) a vloží uvítací zprávu "Hello, World".</span><span class="sxs-lookup"><span data-stu-id="8b3e2-137">Here is code which creates a queue (if it doesn't exist) and inserts hello message "Hello, World".</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Create hello queue if it doesn't already exist.
    queue.createIfNotExists();

    // Create a message and add it toohello queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.addMessage(message);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-peek-at-hello-next-message"></a><span data-ttu-id="8b3e2-138">Postupy: zobrazení náhledu další zprávy hello</span><span class="sxs-lookup"><span data-stu-id="8b3e2-138">How to: Peek at hello next message</span></span>
<span data-ttu-id="8b3e2-139">Můžete prohlížet zprávy hello v popředí hello fronty bez odebere ji z fronty hello voláním **peekMessage**.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-139">You can peek at hello message in hello front of a queue without removing it from hello queue by calling **peekMessage**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Peek at hello next message.
    CloudQueueMessage peekedMessage = queue.peekMessage();

    // Output hello message value.
    if (peekedMessage != null)
    {
      System.out.println(peekedMessage.getMessageContentAsString());
   }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a><span data-ttu-id="8b3e2-140">Postupy: Změna obsahu zpráv zařazených ve frontě hello</span><span class="sxs-lookup"><span data-stu-id="8b3e2-140">How to: Change hello contents of a queued message</span></span>
<span data-ttu-id="8b3e2-141">Můžete změnit obsah zprávy přímo ve frontě hello hello.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-141">You can change hello contents of a message in-place in hello queue.</span></span> <span data-ttu-id="8b3e2-142">Pokud hello zpráva představuje pracovní úlohu, můžete použít tuto funkci tooupdate hello stav hello pracovní úlohy.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-142">If hello message represents a work task, you could use this feature tooupdate hello status of hello work task.</span></span> <span data-ttu-id="8b3e2-143">Hello následující kód aktualizuje zprávy fronty hello nový obsah, a nastaví hello tooextend časový limit viditelnosti jiné 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-143">hello following code updates hello queue message with new contents, and sets hello visibility timeout tooextend another 60 seconds.</span></span> <span data-ttu-id="8b3e2-144">Uloží hello stav práce spojený s uvítací zprávu a poskytuje další minutu toocontinue pracující na uvítací zprávu klienta hello.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-144">This saves hello state of work associated with hello message, and gives hello client another minute toocontinue working on hello message.</span></span> <span data-ttu-id="8b3e2-145">Můžete použít tento postup tootrack vícekrokového pracovní postupy pro zprávy ve frontě, bez nutnosti toostart přes od začátku hello, pokud se nezdaří krok zpracování z důvodu selhání toohardware nebo softwaru.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-145">You could use this technique tootrack multi-step workflows on queue messages, without having toostart over from hello beginning if a processing step fails due toohardware or software failure.</span></span> <span data-ttu-id="8b3e2-146">Obvykle byste udržovali také počet opakování, a pokud hello zpráva se pokus o více než  *n*  krát, odstranili byste ji.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-146">Typically, you would keep a retry count as well, and if hello message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="8b3e2-147">Je to ochrana proti tomu, aby zpráva při každém pokusu o zpracování nevyvolala chyby aplikace.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-147">This protects against a message that triggers an application error each time it is processed.</span></span>

<span data-ttu-id="8b3e2-148">Následující Hello kódu ukázka hledání prostřednictvím hello frontu zpráv, vyhledá hello první zprávu, která odpovídá "Hello, World" hello obsahu pak upraví uvítací zprávu obsahu a ukončí.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-148">hello following code sample searches through hello queue of messages, locates hello first message that matches "Hello, World" for hello content, then modifies hello message content and exits.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // hello maximum number of messages that can be retrieved is 32.
    final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

    // Loop through hello messages in hello queue.
    for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
    {
        // Check for a specific string.
        if (message.getMessageContentAsString().equals("Hello, World"))
        {
            // Modify hello content of hello first matching message.
            message.setMessageContent("Updated contents.");
            // Set it toobe visible in 30 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update hello message.
            queue.updateMessage(message, 30, updateFields, null, null);
            break;
        }
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

<span data-ttu-id="8b3e2-149">Alternativně hello následující ukázka kódu aktualizuje jenom hello první viditelné zprávu ve frontě hello.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-149">Alternatively, hello following code sample updates just hello first visible message on hello queue.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve hello first visible message in hello queue.
    CloudQueueMessage message = queue.retrieveMessage();

    if (message != null)
    {
        // Modify hello message content.
        message.setMessageContent("Updated contents.");
        // Set it toobe visible in 60 seconds.
        EnumSet<MessageUpdateFields> updateFields =
            EnumSet.of(MessageUpdateFields.CONTENT,
            MessageUpdateFields.VISIBILITY);
        // Update hello message.
        queue.updateMessage(message, 60, updateFields, null, null);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-get-hello-queue-length"></a><span data-ttu-id="8b3e2-150">Postupy: získání délky fronty hello</span><span class="sxs-lookup"><span data-stu-id="8b3e2-150">How to: Get hello queue length</span></span>
<span data-ttu-id="8b3e2-151">Můžete získat odhad hello počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-151">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="8b3e2-152">Hello **downloadAttributes** metoda požádá službu front hello několik aktuální hodnoty, včetně počet jsou počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-152">hello **downloadAttributes** method asks hello Queue service for several current values, including a count of how many messages are in a queue.</span></span> <span data-ttu-id="8b3e2-153">počet Hello je pouze přibližné, protože zprávy můžete přidat nebo odebrat po tooyour žádost odpoví hello služby front.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-153">hello count is only approximate because messages can be added or removed after hello Queue service responds tooyour request.</span></span> <span data-ttu-id="8b3e2-154">Hello **getApproximateMessageCount** metoda vrátí hello poslední hodnotu načtenou hello volání příliš**downloadAttributes**, bez volání služby front hello.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-154">hello **getApproximateMessageCount** method returns hello last value retrieved by hello call too**downloadAttributes**, without calling hello Queue service.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Download hello approximate message count from hello server.
    queue.downloadAttributes();

    // Retrieve hello newly cached approximate message count.
    long cachedMessageCount = queue.getApproximateMessageCount();

    // Display hello queue length.
    System.out.println(String.format("Queue length: %d", cachedMessageCount));
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-dequeue-hello-next-message"></a><span data-ttu-id="8b3e2-155">Postupy: dequeue – další zprávy hello</span><span class="sxs-lookup"><span data-stu-id="8b3e2-155">How to: Dequeue hello next message</span></span>
<span data-ttu-id="8b3e2-156">Váš kód dequeues zprávu z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-156">Your code dequeues a message from a queue in two steps.</span></span> <span data-ttu-id="8b3e2-157">Při volání **retrieveMessage**, získat hello další zprávu ve frontě.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-157">When you call **retrieveMessage**, you get hello next message in a queue.</span></span> <span data-ttu-id="8b3e2-158">Zpráva vrácená metodou **retrieveMessage** stane neviditelnou tooany další kód, který čte zprávy z této fronty.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-158">A message returned from **retrieveMessage** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="8b3e2-159">Ve výchozím nastavení tato zpráva zůstává neviditelná po dobu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-159">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="8b3e2-160">toofinish odebírání uvítací zprávu z fronty hello, musíte také zavolat **deleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-160">toofinish removing hello message from hello queue, you must also call **deleteMessage**.</span></span> <span data-ttu-id="8b3e2-161">Tento dvoukrokový proces odebrání zprávy zaručuje, pokud se váš kód nezdaří tooprocess, které můžete získat zprávu z důvodu selhání toohardware nebo softwaru, jiná instance vašeho kódu hello stejnou zprávu a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-161">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="8b3e2-162">Váš kód zavolá metodu **deleteMessage** hned po zpracování zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-162">Your code calls **deleteMessage** right after hello message has been processed.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve hello first visible message in hello queue.
    CloudQueueMessage retrievedMessage = queue.retrieveMessage();

    if (retrievedMessage != null)
    {
        // Process hello message in less than 30 seconds, and then delete hello message.
        queue.deleteMessage(retrievedMessage);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="additional-options-for-dequeuing-messages"></a><span data-ttu-id="8b3e2-163">Další možnosti pro vyřazení zprávy</span><span class="sxs-lookup"><span data-stu-id="8b3e2-163">Additional options for dequeuing messages</span></span>
<span data-ttu-id="8b3e2-164">Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-164">There are two ways you can customize message retrieval from a queue.</span></span> <span data-ttu-id="8b3e2-165">Nejprve můžete načíst dávku zpráv (až too32).</span><span class="sxs-lookup"><span data-stu-id="8b3e2-165">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="8b3e2-166">Druhý můžete nastavit časový limit neviditelnosti delší nebo kratší, povolení váš kód více nebo méně času toofully zpracování jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-166">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span>

<span data-ttu-id="8b3e2-167">Hello následující příklad kódu používá hello **retrieveMessages** metoda tooget 20 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-167">hello following code example uses hello **retrieveMessages** method tooget 20 messages in one call.</span></span> <span data-ttu-id="8b3e2-168">Pak se každá zpráva zpracuje pomocí **pro** smyčky.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-168">Then it processes each message using a **for** loop.</span></span> <span data-ttu-id="8b3e2-169">Nastaví taky hello neviditelnosti časový limit toofive minut (300 sekund) pro každou zprávu.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-169">It also sets hello invisibility timeout toofive minutes (300 seconds) for each message.</span></span> <span data-ttu-id="8b3e2-170">Všimněte si, že hello pět minut spustí pro všechny zprávy v hello stejný čas, takže když pěti minut od volání hello příliš předané**retrieveMessages**, všechny zprávy, které nebyly odstraněny, opět viditelné.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-170">Note that hello five minutes starts for all messages at hello same time, so when five minutes have passed since hello call too**retrieveMessages**, any messages which have not been deleted will become visible again.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve 20 messages from hello queue with a visibility timeout of 300 seconds.
    for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
        // Do processing for all messages in less than 5 minutes,
        // deleting each message after processing.
        queue.deleteMessage(message);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-hello-queues"></a><span data-ttu-id="8b3e2-171">Postupy: seznam hello fronty</span><span class="sxs-lookup"><span data-stu-id="8b3e2-171">How to: List hello queues</span></span>
<span data-ttu-id="8b3e2-172">tooobtain seznam hello aktuální front, volání hello **CloudQueueClient.listQueues()** metodu, která vrátí kolekci **CloudQueue** objekty.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-172">tooobtain a list of hello current queues, call hello **CloudQueueClient.listQueues()** method, which will return a collection of **CloudQueue** objects.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient =
        storageAccount.createCloudQueueClient();

    // Loop through hello collection of queues.
    for (CloudQueue queue : queueClient.listQueues())
    {
        // Output each queue name.
        System.out.println(queue.getName());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="8b3e2-173">Postupy: odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="8b3e2-173">How to: Delete a queue</span></span>
<span data-ttu-id="8b3e2-174">toodelete frontu a všechny zprávy hello obsažené v něm volání hello **deleteIfExists** metodu hello **CloudQueue** objektu.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-174">toodelete a queue and all hello messages contained in it, call hello **deleteIfExists** method on hello **CloudQueue** object.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Delete hello queue if it exists.
    queue.deleteIfExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a><span data-ttu-id="8b3e2-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8b3e2-175">Next steps</span></span>
<span data-ttu-id="8b3e2-176">Teď, když jste se naučili základy hello fronty úložiště, postupujte podle těchto odkazů toolearn o složitějších úlohách úložiště.</span><span class="sxs-lookup"><span data-stu-id="8b3e2-176">Now that you've learned hello basics of queue storage, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="8b3e2-177">[Úložiště Azure SDK pro jazyk Java][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="8b3e2-177">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="8b3e2-178">[Referenční informace sady SDK úložiště Azure klienta][Azure úložiště klienta SDK Reference]</span><span class="sxs-lookup"><span data-stu-id="8b3e2-178">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="8b3e2-179">[REST API služby Azure Storage][Azure Storage Services REST API]</span><span class="sxs-lookup"><span data-stu-id="8b3e2-179">[Azure Storage Services REST API][Azure Storage Services REST API]</span></span>
* <span data-ttu-id="8b3e2-180">[Blog týmu Azure Storage][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="8b3e2-180">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure úložiště klienta SDK Reference]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage Services REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
