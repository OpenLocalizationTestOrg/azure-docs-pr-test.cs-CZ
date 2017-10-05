---
title: "Používání úložiště Queue z Javy | Microsoft Docs"
description: "Naučte se používat službu front Azure k vytváření a odstraňování front a vložit, získání a odstranění zprávy. Ukázky napsanou v jazyce Java."
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
ms.openlocfilehash: a56b345c5efb4ce9c8ee2da91b798d09d44e42be
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-queue-storage-from-java"></a><span data-ttu-id="32a8f-104">Používání úložiště Queue z Javy</span><span class="sxs-lookup"><span data-stu-id="32a8f-104">How to use Queue storage from Java</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a><span data-ttu-id="32a8f-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="32a8f-105">Overview</span></span>
<span data-ttu-id="32a8f-106">Tento průvodce vám ukáže, jak provádět běžné scénáře s využitím služby Azure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="32a8f-106">This guide will show you how to perform common scenarios using the Azure Queue storage service.</span></span> <span data-ttu-id="32a8f-107">Ukázky jsou napsané v jazyce Java a použít [sada SDK úložiště Azure pro jazyk Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="32a8f-107">The samples are written in Java and use the [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="32a8f-108">Pokryté scénáře zahrnují **vkládání**, **prohlížení**, **získávání**, a **odstraňování** fronty zpráv, a také **vytváření** a **odstraňování** fronty.</span><span class="sxs-lookup"><span data-stu-id="32a8f-108">The scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating** and **deleting** queues.</span></span> <span data-ttu-id="32a8f-109">Další informace o frontách najdete v článku [další kroky](#Next-Steps) části.</span><span class="sxs-lookup"><span data-stu-id="32a8f-109">For more information on queues, see the [Next steps](#Next-Steps) section.</span></span>

<span data-ttu-id="32a8f-110">Poznámka: Sada SDK je k dispozici pro vývojáře, kteří na zařízení se systémem Android používají Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="32a8f-110">Note: An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="32a8f-111">Další informace najdete v tématu [sada SDK úložiště Azure pro Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="32a8f-111">For more information, see the [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="32a8f-112">Vytvoření aplikace Java</span><span class="sxs-lookup"><span data-stu-id="32a8f-112">Create a Java application</span></span>
<span data-ttu-id="32a8f-113">V této příručce bude používat funkce úložiště, které lze spustit v rámci aplikace Java místně nebo v kódu běžící v rámci webovou roli nebo role pracovního procesu v Azure.</span><span class="sxs-lookup"><span data-stu-id="32a8f-113">In this guide, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="32a8f-114">V takovém případě budete muset nainstalovat Java Development Kit (JDK) a vytvořit účet úložiště Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="32a8f-114">To do so, you will need to install the Java Development Kit (JDK) and create an Azure storage account in your Azure subscription.</span></span> <span data-ttu-id="32a8f-115">Jakmile provedete, budete muset ověřit, jestli váš vývojový systém splňuje minimální požadavky a závislosti, které jsou uvedeny v [sada SDK úložiště Azure pro jazyk Java] [ Azure Storage SDK for Java] úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="32a8f-115">Once you have done so, you will need to verify that your development system meets the minimum requirements and dependencies which are listed in the [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="32a8f-116">Pokud váš systém splňuje tyto požadavky, můžete podle pokynů ke stažení a instalaci knihovny úložiště Azure pro jazyk Java systému z tohoto úložiště.</span><span class="sxs-lookup"><span data-stu-id="32a8f-116">If your system meets those requirements, you can follow the instructions for downloading and installing the Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="32a8f-117">Po dokončení těchto úloh, bude moci vytvořit aplikaci Java, která používá v příkladech v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="32a8f-117">Once you have completed those tasks, you will be able to create a Java application which uses the examples in this article.</span></span>

## <a name="configure-your-application-to-access-queue-storage"></a><span data-ttu-id="32a8f-118">Konfigurace aplikace pro přístup k úložišti fronty</span><span class="sxs-lookup"><span data-stu-id="32a8f-118">Configure your application to access queue storage</span></span>
<span data-ttu-id="32a8f-119">Přidejte následující příkazy pro import do horní části souboru Java, ve které chcete používat úložiště Azure rozhraní API pro přístup k fronty:</span><span class="sxs-lookup"><span data-stu-id="32a8f-119">Add the following import statements to the top of the Java file where you want to use Azure storage APIs to access queues:</span></span>

```java
// Include the following imports to use queue APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.queue.*;
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="32a8f-120">Instalační program připojovací řetězec úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="32a8f-120">Setup an Azure storage connection string</span></span>
<span data-ttu-id="32a8f-121">Klienta Azure storage používá připojovací řetězec úložiště k ukládání koncových bodů a pověření pro přístup ke službám dat správy.</span><span class="sxs-lookup"><span data-stu-id="32a8f-121">An Azure storage client uses a storage connection string to store endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="32a8f-122">Když spustíte v aplikaci klienta, je nutné zadat připojovací řetězec úložiště v následujícím formátu, pomocí názvu účtu úložiště a primární přístupový klíč pro účet úložiště uvedené v [portálu Azure](https://portal.azure.com) pro *AccountName* a *AccountKey* hodnoty.</span><span class="sxs-lookup"><span data-stu-id="32a8f-122">When running in a client application, you must provide the storage connection string in the following format, using the name of your storage account and the Primary access key for the storage account listed in the [Azure Portal](https://portal.azure.com) for the *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="32a8f-123">Tento příklad ukazuje, jak můžou deklarovat statické pole pro uložení připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="32a8f-123">This example shows how you can declare a static field to hold the connection string:</span></span>

```java
// Define the connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="32a8f-124">Ve spuštěné aplikace v rámci role ve službě Microsoft Azure, můžete tento řetězec uložené v konfiguračním souboru služby *souboru ServiceConfiguration.cscfg*a je přístupný pomocí volání **RoleEnvironment.getConfigurationSettings** metoda.</span><span class="sxs-lookup"><span data-stu-id="32a8f-124">In an application running within a role in Microsoft Azure, this string can be stored in the service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call to the **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="32a8f-125">Tady je příklad získávání připojovacího řetězce z **nastavení** element s názvem *StorageConnectionString* v konfiguračním souboru služby:</span><span class="sxs-lookup"><span data-stu-id="32a8f-125">Here's an example of getting the connection string from a **Setting** element named *StorageConnectionString* in the service configuration file:</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="32a8f-126">Následující ukázky předpokládejme, že používáte jednu z těchto dvou metod k získání připojovacího řetězce úložiště.</span><span class="sxs-lookup"><span data-stu-id="32a8f-126">The following samples assume that you have used one of these two methods to get the storage connection string.</span></span>

## <a name="how-to-create-a-queue"></a><span data-ttu-id="32a8f-127">Postupy: vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="32a8f-127">How to: Create a queue</span></span>
<span data-ttu-id="32a8f-128">A **CloudQueueClient** objektu umožňuje získat odkaz na objekty pro fronty.</span><span class="sxs-lookup"><span data-stu-id="32a8f-128">A **CloudQueueClient** object lets you get reference objects for queues.</span></span> <span data-ttu-id="32a8f-129">Následující kód vytvoří **CloudQueueClient** objektu.</span><span class="sxs-lookup"><span data-stu-id="32a8f-129">The following code creates a **CloudQueueClient** object.</span></span> <span data-ttu-id="32a8f-130">(Poznámka: Chcete-li vytvořit další způsoby **CloudStorageAccount** objekty; Další informace najdete v tématu **CloudStorageAccount** v [odkaz SDK klienta úložiště Azure].)</span><span class="sxs-lookup"><span data-stu-id="32a8f-130">(Note: There are additional ways to create **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in the [Azure Storage Client SDK Reference].)</span></span>

<span data-ttu-id="32a8f-131">Použít **CloudQueueClient** objekt, který chcete získat odkaz na frontu, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="32a8f-131">Use the **CloudQueueClient** object to get a reference to the queue you want to use.</span></span> <span data-ttu-id="32a8f-132">Můžete vytvořit frontu, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="32a8f-132">You can create the queue if it doesn't exist.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

   // Create the queue client.
   CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

   // Retrieve a reference to a queue.
   CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Create the queue if it doesn't already exist.
   queue.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-a-message-to-a-queue"></a><span data-ttu-id="32a8f-133">Postupy: Přidání zprávy do fronty</span><span class="sxs-lookup"><span data-stu-id="32a8f-133">How to: Add a message to a queue</span></span>
<span data-ttu-id="32a8f-134">Pokud chcete vložit zprávu do existující fronty, vytvořte nejdříve novou třídu **CloudQueueMessage**.</span><span class="sxs-lookup"><span data-stu-id="32a8f-134">To insert a message into an existing queue, first create a new **CloudQueueMessage**.</span></span> <span data-ttu-id="32a8f-135">Pak zavolejte **addMessage** metoda.</span><span class="sxs-lookup"><span data-stu-id="32a8f-135">Next, call the **addMessage** method.</span></span> <span data-ttu-id="32a8f-136">A **CloudQueueMessage** lze vytvořit z řetězce (ve formátu UTF-8) nebo pole bajtů.</span><span class="sxs-lookup"><span data-stu-id="32a8f-136">A **CloudQueueMessage** can be created from either a string (in UTF-8 format) or a byte array.</span></span> <span data-ttu-id="32a8f-137">Tady je kód, který vytvoří frontu (pokud neexistuje) a vloží zprávu "Hello, World".</span><span class="sxs-lookup"><span data-stu-id="32a8f-137">Here is code which creates a queue (if it doesn't exist) and inserts the message "Hello, World".</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.createIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.addMessage(message);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-peek-at-the-next-message"></a><span data-ttu-id="32a8f-138">Postupy: zobrazení náhledu další zprávy</span><span class="sxs-lookup"><span data-stu-id="32a8f-138">How to: Peek at the next message</span></span>
<span data-ttu-id="32a8f-139">Můžete prohlížet zprávy ve frontě bez odebere ji z fronty voláním **peekMessage**.</span><span class="sxs-lookup"><span data-stu-id="32a8f-139">You can peek at the message in the front of a queue without removing it from the queue by calling **peekMessage**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Peek at the next message.
    CloudQueueMessage peekedMessage = queue.peekMessage();

    // Output the message value.
    if (peekedMessage != null)
    {
      System.out.println(peekedMessage.getMessageContentAsString());
   }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a><span data-ttu-id="32a8f-140">Postupy: Změna obsahu zpráv zařazených ve frontě</span><span class="sxs-lookup"><span data-stu-id="32a8f-140">How to: Change the contents of a queued message</span></span>
<span data-ttu-id="32a8f-141">Podle potřeby můžete změnit obsah zprávy přímo ve frontě.</span><span class="sxs-lookup"><span data-stu-id="32a8f-141">You can change the contents of a message in-place in the queue.</span></span> <span data-ttu-id="32a8f-142">Pokud zpráva představuje pracovní úlohu, mohli byste tuto funkci použít k aktualizaci stavu pracovních úloh.</span><span class="sxs-lookup"><span data-stu-id="32a8f-142">If the message represents a work task, you could use this feature to update the status of the work task.</span></span> <span data-ttu-id="32a8f-143">Následující kód aktualizuje zprávy ve frontě o nový obsah a prodlouží časový limit viditelnosti na 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="32a8f-143">The following code updates the queue message with new contents, and sets the visibility timeout to extend another 60 seconds.</span></span> <span data-ttu-id="32a8f-144">Uloží se tím stav práce spojený se zprávou a klient získá další minutu, aby mohl pokračovat ve zpracování zprávy.</span><span class="sxs-lookup"><span data-stu-id="32a8f-144">This saves the state of work associated with the message, and gives the client another minute to continue working on the message.</span></span> <span data-ttu-id="32a8f-145">Tímto způsobem může sledovat vícekrokového pracovní postupy pro zprávy ve frontě, aniž by bylo nutné v případě, že krok zpracování z důvodu selhání hardwaru nebo softwaru selže, začít znovu od začátku.</span><span class="sxs-lookup"><span data-stu-id="32a8f-145">You could use this technique to track multi-step workflows on queue messages, without having to start over from the beginning if a processing step fails due to hardware or software failure.</span></span> <span data-ttu-id="32a8f-146">Obvykle byste udržovali také hodnotu počtu opakování, a pokud by se pokus o zpracování zprávy opakoval více než *n*krát, odstranili byste ji.</span><span class="sxs-lookup"><span data-stu-id="32a8f-146">Typically, you would keep a retry count as well, and if the message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="32a8f-147">Je to ochrana proti tomu, aby zpráva při každém pokusu o zpracování nevyvolala chyby aplikace.</span><span class="sxs-lookup"><span data-stu-id="32a8f-147">This protects against a message that triggers an application error each time it is processed.</span></span>

<span data-ttu-id="32a8f-148">Následující ukázka hledání kódu prostřednictvím fronty zpráv, vyhledá první zprávu, která odpovídá "Hello, World" pro obsah, pak upraví obsahu zprávy a ukončí.</span><span class="sxs-lookup"><span data-stu-id="32a8f-148">The following code sample searches through the queue of messages, locates the first message that matches "Hello, World" for the content, then modifies the message content and exits.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // The maximum number of messages that can be retrieved is 32.
    final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

    // Loop through the messages in the queue.
    for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
    {
        // Check for a specific string.
        if (message.getMessageContentAsString().equals("Hello, World"))
        {
            // Modify the content of the first matching message.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 30 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 30, updateFields, null, null);
            break;
        }
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

<span data-ttu-id="32a8f-149">Následující ukázka kódu Alternativně aktualizuje pouze první viditelné zprávu ve frontě.</span><span class="sxs-lookup"><span data-stu-id="32a8f-149">Alternatively, the following code sample updates just the first visible message on the queue.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage message = queue.retrieveMessage();

    if (message != null)
    {
        // Modify the message content.
        message.setMessageContent("Updated contents.");
        // Set it to be visible in 60 seconds.
        EnumSet<MessageUpdateFields> updateFields =
            EnumSet.of(MessageUpdateFields.CONTENT,
            MessageUpdateFields.VISIBILITY);
        // Update the message.
        queue.updateMessage(message, 60, updateFields, null, null);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-get-the-queue-length"></a><span data-ttu-id="32a8f-150">Postupy: získání délky fronty</span><span class="sxs-lookup"><span data-stu-id="32a8f-150">How to: Get the queue length</span></span>
<span data-ttu-id="32a8f-151">Podle potřeby můžete získat odhadovaný počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="32a8f-151">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="32a8f-152">**DownloadAttributes** metoda požádá službu front několik aktuální hodnoty, včetně počet jsou počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="32a8f-152">The **downloadAttributes** method asks the Queue service for several current values, including a count of how many messages are in a queue.</span></span> <span data-ttu-id="32a8f-153">Počet je pouze přibližné, protože zprávy můžete přidat nebo odebrat po služby front odpoví na žádost.</span><span class="sxs-lookup"><span data-stu-id="32a8f-153">The count is only approximate because messages can be added or removed after the Queue service responds to your request.</span></span> <span data-ttu-id="32a8f-154">**GetApproximateMessageCount** metoda vrátí poslední hodnotu načtenou volání **downloadAttributes**, bez volání služby front.</span><span class="sxs-lookup"><span data-stu-id="32a8f-154">The **getApproximateMessageCount** method returns the last value retrieved by the call to **downloadAttributes**, without calling the Queue service.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Download the approximate message count from the server.
    queue.downloadAttributes();

    // Retrieve the newly cached approximate message count.
    long cachedMessageCount = queue.getApproximateMessageCount();

    // Display the queue length.
    System.out.println(String.format("Queue length: %d", cachedMessageCount));
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-dequeue-the-next-message"></a><span data-ttu-id="32a8f-155">Postupy: vyřazení další zprávy z fronty</span><span class="sxs-lookup"><span data-stu-id="32a8f-155">How to: Dequeue the next message</span></span>
<span data-ttu-id="32a8f-156">Váš kód dequeues zprávu z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="32a8f-156">Your code dequeues a message from a queue in two steps.</span></span> <span data-ttu-id="32a8f-157">Při volání **retrieveMessage**, získáte další zprávu ve frontě.</span><span class="sxs-lookup"><span data-stu-id="32a8f-157">When you call **retrieveMessage**, you get the next message in a queue.</span></span> <span data-ttu-id="32a8f-158">Zpráva vrácená metodou **retrieveMessage** stane neviditelnou pro jakýkoli jiný kód čtení zpráv z této fronty.</span><span class="sxs-lookup"><span data-stu-id="32a8f-158">A message returned from **retrieveMessage** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="32a8f-159">Ve výchozím nastavení tato zpráva zůstává neviditelná po dobu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="32a8f-159">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="32a8f-160">K dokončení odebrání zprávy z fronty, musíte také zavolat **deleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="32a8f-160">To finish removing the message from the queue, you must also call **deleteMessage**.</span></span> <span data-ttu-id="32a8f-161">Tento dvoukrokový proces odebrání zprávy zaručuje, aby v případě, že se vašemu kódu nepodaří zprávu zpracovat z důvodu selhání hardwaru nebo softwaru, mohla stejnou zprávu získat jiná instance vašeho kódu a bylo možné to zkusit znovu.</span><span class="sxs-lookup"><span data-stu-id="32a8f-161">This two-step process of removing a message assures that if your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="32a8f-162">Váš kód zavolá metodu **deleteMessage** pravým po zpracování zprávy.</span><span class="sxs-lookup"><span data-stu-id="32a8f-162">Your code calls **deleteMessage** right after the message has been processed.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage retrievedMessage = queue.retrieveMessage();

    if (retrievedMessage != null)
    {
        // Process the message in less than 30 seconds, and then delete the message.
        queue.deleteMessage(retrievedMessage);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="additional-options-for-dequeuing-messages"></a><span data-ttu-id="32a8f-163">Další možnosti pro vyřazení zprávy</span><span class="sxs-lookup"><span data-stu-id="32a8f-163">Additional options for dequeuing messages</span></span>
<span data-ttu-id="32a8f-164">Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="32a8f-164">There are two ways you can customize message retrieval from a queue.</span></span> <span data-ttu-id="32a8f-165">Za prvé si můžete načíst dávku zpráv (až 32).</span><span class="sxs-lookup"><span data-stu-id="32a8f-165">First, you can get a batch of messages (up to 32).</span></span> <span data-ttu-id="32a8f-166">Za druhé si můžete nastavit delší nebo kratší časový limit neviditelnosti, aby měl váš kód více nebo méně času na úplné zpracování jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="32a8f-166">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time to fully process each message.</span></span>

<span data-ttu-id="32a8f-167">Následující příklad kódu používá **retrieveMessages** metoda získá 20 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="32a8f-167">The following code example uses the **retrieveMessages** method to get 20 messages in one call.</span></span> <span data-ttu-id="32a8f-168">Pak se každá zpráva zpracuje pomocí **pro** smyčky.</span><span class="sxs-lookup"><span data-stu-id="32a8f-168">Then it processes each message using a **for** loop.</span></span> <span data-ttu-id="32a8f-169">Také nastaví časový limit neviditelnosti 5 minut (300 sekund) pro každou zprávu.</span><span class="sxs-lookup"><span data-stu-id="32a8f-169">It also sets the invisibility timeout to five minutes (300 seconds) for each message.</span></span> <span data-ttu-id="32a8f-170">Všimněte si, že za pět minut spustí pro všechny zprávy ve stejnou dobu, takže když pět minut předané od volání **retrieveMessages**, všechny zprávy, které nebyly odstraněny, opět viditelné.</span><span class="sxs-lookup"><span data-stu-id="32a8f-170">Note that the five minutes starts for all messages at the same time, so when five minutes have passed since the call to **retrieveMessages**, any messages which have not been deleted will become visible again.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
        // Do processing for all messages in less than 5 minutes,
        // deleting each message after processing.
        queue.deleteMessage(message);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-the-queues"></a><span data-ttu-id="32a8f-171">Postupy: seznam fronty</span><span class="sxs-lookup"><span data-stu-id="32a8f-171">How to: List the queues</span></span>
<span data-ttu-id="32a8f-172">Chcete-li získat seznam aktuální front, zavolejte **CloudQueueClient.listQueues()** metodu, která vrátí kolekci **CloudQueue** objekty.</span><span class="sxs-lookup"><span data-stu-id="32a8f-172">To obtain a list of the current queues, call the **CloudQueueClient.listQueues()** method, which will return a collection of **CloudQueue** objects.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient =
        storageAccount.createCloudQueueClient();

    // Loop through the collection of queues.
    for (CloudQueue queue : queueClient.listQueues())
    {
        // Output each queue name.
        System.out.println(queue.getName());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="32a8f-173">Postupy: odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="32a8f-173">How to: Delete a queue</span></span>
<span data-ttu-id="32a8f-174">Chcete-li odstranit frontu se všemi zprávami, které v ní, zavolejte **deleteIfExists** metodu **CloudQueue** objektu.</span><span class="sxs-lookup"><span data-stu-id="32a8f-174">To delete a queue and all the messages contained in it, call the **deleteIfExists** method on the **CloudQueue** object.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Delete the queue if it exists.
    queue.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a><span data-ttu-id="32a8f-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="32a8f-175">Next steps</span></span>
<span data-ttu-id="32a8f-176">Teď, když jste se naučili základy používání služby queue storage, postupujte podle následujících odkazech na další informace o složitějších úlohách úložiště.</span><span class="sxs-lookup"><span data-stu-id="32a8f-176">Now that you've learned the basics of queue storage, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="32a8f-177">[Úložiště Azure SDK pro jazyk Java][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="32a8f-177">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="32a8f-178">[Referenční informace sady SDK úložiště Azure klienta][odkaz SDK klienta úložiště Azure]</span><span class="sxs-lookup"><span data-stu-id="32a8f-178">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="32a8f-179">[REST API služby Azure Storage][Azure Storage Services REST API]</span><span class="sxs-lookup"><span data-stu-id="32a8f-179">[Azure Storage Services REST API][Azure Storage Services REST API]</span></span>
* <span data-ttu-id="32a8f-180">[Blog týmu Azure Storage][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="32a8f-180">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[odkaz SDK klienta úložiště Azure]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage Services REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
