---
title: "Přijímat události z Azure Event Hubs pomocí Java | Microsoft Docs"
description: "Začínáme přijetí ze služby Event Hubs používá Java"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 38e3be53-251c-488f-a856-9a500f41b6ca
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 3c1b455e6298367dc50f0943b58f6cf1e7f1c5fd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="receive-events-from-azure-event-hubs-using-java"></a><span data-ttu-id="d345c-103">Přijímat události z Azure Event Hubs pomocí Java</span><span class="sxs-lookup"><span data-stu-id="d345c-103">Receive events from Azure Event Hubs using Java</span></span>


## <a name="introduction"></a><span data-ttu-id="d345c-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="d345c-104">Introduction</span></span>
<span data-ttu-id="d345c-105">Event Hubs je vysoce škálovatelná služba, kterou lze přijímat miliony událostí za sekundu, povolení aplikaci zpracovávat a analyzovat masivní objemy dat vytvářených připojených zařízení a aplikací.</span><span class="sxs-lookup"><span data-stu-id="d345c-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application to process and analyze the massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="d345c-106">Když Event Hubs shromáždí data, můžete je zpracovat a uložit pomocí libovolného úložného clusteru nebo poskytovatele datové analýzy v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="d345c-106">Once collected into Event Hubs, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="d345c-107">Další informace najdete v tématu [Přehled služby Event Hubs][Event Hubs overview].</span><span class="sxs-lookup"><span data-stu-id="d345c-107">For more information, see the [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="d345c-108">Tento kurz ukazuje, jak přijímat události do centra událostí pomocí konzolové aplikace napsané v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="d345c-108">This tutorial shows how to receive events into an event hub using a console application written in Java.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d345c-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d345c-109">Prerequisites</span></span>

<span data-ttu-id="d345c-110">K dokončení tohoto kurzu potřebujete následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="d345c-110">In order to complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="d345c-111">Vývojové prostředí Java.</span><span class="sxs-lookup"><span data-stu-id="d345c-111">A Java development environment.</span></span> <span data-ttu-id="d345c-112">V tomto kurzu budeme předpokládat [Eclipse](https://www.eclipse.org/).</span><span class="sxs-lookup"><span data-stu-id="d345c-112">For this tutorial, we assume [Eclipse](https://www.eclipse.org/).</span></span>
* <span data-ttu-id="d345c-113">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d345c-113">An active Azure account.</span></span> <br/><span data-ttu-id="d345c-114">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný účet.</span><span class="sxs-lookup"><span data-stu-id="d345c-114">If you don't have an account, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="d345c-115">Podrobnosti najdete v článku <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Bezplatná zkušební verze Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="d345c-115">For details, see <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure Free Trial</a>.</span></span>

## <a name="receive-messages-with-eventprocessorhost-in-java"></a><span data-ttu-id="d345c-116">Přijímání zpráv pomocí třídy EventProcessorHost v Javě</span><span class="sxs-lookup"><span data-stu-id="d345c-116">Receive messages with EventProcessorHost in Java</span></span>

<span data-ttu-id="d345c-117">**EventProcessorHost** je třída Java, která zjednodušuje přijímání události ze služby Event Hubs tím, že spravuje trvalé kontrolní body a paralelní příjemce událostí ze služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="d345c-117">**EventProcessorHost** is a Java class that simplifies receiving events from Event Hubs by managing persistent checkpoints and parallel receives from those Event Hubs.</span></span> <span data-ttu-id="d345c-118">Pomocí třídy EventProcessorHost, můžete události rozdělit mezi několik příjemců, i když jsou hostované v různých uzlech.</span><span class="sxs-lookup"><span data-stu-id="d345c-118">Using EventProcessorHost, you can split events across multiple receivers, even when hosted in different nodes.</span></span> <span data-ttu-id="d345c-119">Tento příklad ukazuje způsob použití třídy EventProcessorHost pro jednoho příjemce.</span><span class="sxs-lookup"><span data-stu-id="d345c-119">This example shows how to use EventProcessorHost for a single receiver.</span></span>

### <a name="create-a-storage-account"></a><span data-ttu-id="d345c-120">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="d345c-120">Create a storage account</span></span>
<span data-ttu-id="d345c-121">Chcete-li použít EventProcessorHost, musíte mít [účet úložiště Azure][Azure Storage account]:</span><span class="sxs-lookup"><span data-stu-id="d345c-121">To use EventProcessorHost, you must have an [Azure Storage account][Azure Storage account]:</span></span>

1. <span data-ttu-id="d345c-122">Přihlaste se na [portál Azure][Azure portal]a klikněte na tlačítko **+ nový** na levé straně obrazovky.</span><span class="sxs-lookup"><span data-stu-id="d345c-122">Log on to the [Azure portal][Azure portal], and click **+ New** on the left-hand side of the screen.</span></span>
2. <span data-ttu-id="d345c-123">Klikněte na **Storage** a poté klikněte na **Účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="d345c-123">Click **Storage**, then click **Storage account**.</span></span> <span data-ttu-id="d345c-124">V okně **Vytvořit účet úložiště** zadejte název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d345c-124">In the **Create storage account** blade, type a name for the storage account.</span></span> <span data-ttu-id="d345c-125">Dokončete polí, vyberte požadovanou oblast a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d345c-125">Complete the rest of the fields, select your desired region, and then click **Create**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. <span data-ttu-id="d345c-126">Klikněte na nově vytvořený účet úložiště a potom klikněte na **Spravovat přístupové klíče**:</span><span class="sxs-lookup"><span data-stu-id="d345c-126">Click the newly created storage account, and then click **Manage Access Keys**:</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    <span data-ttu-id="d345c-127">Zkopírujte primární přístupový klíč do dočasného umístění, k pozdějšímu použití v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d345c-127">Copy the primary access key to a temporary location, to use later in this tutorial.</span></span>

### <a name="create-a-java-project-using-the-eventprocessor-host"></a><span data-ttu-id="d345c-128">Vytvoření projektu jazyka Java pomocí hostitele EventProcessor</span><span class="sxs-lookup"><span data-stu-id="d345c-128">Create a Java project using the EventProcessor Host</span></span>
<span data-ttu-id="d345c-129">Klientská knihovna Java pro službu Event Hubs je k dispozici pro použití v projektech Maven z [centrálním úložišti Maven][Maven Package]a můžete odkazovat pomocí následující prohlášení závislostí v souboru projektu Maven:</span><span class="sxs-lookup"><span data-stu-id="d345c-129">The Java client library for Event Hubs is available for use in Maven projects from the [Maven Central Repository][Maven Package], and can be referenced using the following dependency declaration inside your Maven project file:</span></span>    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>azure-eventhubs-eph</artifactId>
  <version>0.14.0</version>
</dependency>
```

<span data-ttu-id="d345c-130">Pro různé typy prostředí sestavení, můžete explicitně získat nejnovější vydaná JAR soubory z [centrálním úložišti Maven] [ Maven Package] nebo z [distribuční bod verze na Githubu](https://github.com/Azure/azure-event-hubs/releases).</span><span class="sxs-lookup"><span data-stu-id="d345c-130">For different types of build environments, you can explicitly obtain the latest released JAR files from the [Maven Central Repository][Maven Package] or from [the release distribution point on GitHub](https://github.com/Azure/azure-event-hubs/releases).</span></span>  

1. <span data-ttu-id="d345c-131">Pro následující příklad nejprve vytvořte nový projekt Maven pro aplikaci konzoly nebo prostředí v oblíbeném vývojovém prostředí Java.</span><span class="sxs-lookup"><span data-stu-id="d345c-131">For the following sample, first create a new Maven project for a console/shell application in your favorite Java development environment.</span></span> <span data-ttu-id="d345c-132">Třídy se nazývá `ErrorNotificationHandler`.</span><span class="sxs-lookup"><span data-stu-id="d345c-132">The class is called `ErrorNotificationHandler`.</span></span>     
   
    ```java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   
    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```
2. <span data-ttu-id="d345c-133">Použijte následující kód k vytvoření nové třídy s názvem `EventProcessor`.</span><span class="sxs-lookup"><span data-stu-id="d345c-133">Use the following code to create a new class called `EventProcessor`.</span></span>
   
    ```java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;
   
    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;
   
        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }
   
        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
   
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }
   
        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
   
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```
3. <span data-ttu-id="d345c-134">Vytvořte jeden další třídu s názvem `EventProcessorSample`, pomocí následujícího kódu.</span><span class="sxs-lookup"><span data-stu-id="d345c-134">Create one more class called `EventProcessorSample`, using the following code.</span></span>
   
    ```java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;
   
    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";
   
            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
   
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
   
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
   
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }
   
            System.out.println("Press enter to stop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
   
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
   
            System.out.println("End of sample");
        }
    }
    ```
4. <span data-ttu-id="d345c-135">Následující pole nahraďte hodnoty používané při vytváření účtu události rozbočovače a úložiště.</span><span class="sxs-lookup"><span data-stu-id="d345c-135">Replace the following fields with the values used when you created the event hub and storage account.</span></span>
   
    ```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
   
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
   
    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [!NOTE]
> <span data-ttu-id="d345c-136">Tento kurz používá jednu instanci třídy EventProcessorHost.</span><span class="sxs-lookup"><span data-stu-id="d345c-136">This tutorial uses a single instance of EventProcessorHost.</span></span> <span data-ttu-id="d345c-137">Pokud chcete zvýšit propustnost, doporučujeme spustit víc instancí služby EventProcessorHost, pokud možno na samostatných počítačích.</span><span class="sxs-lookup"><span data-stu-id="d345c-137">To increase throughput, it is recommended that you run multiple instances of EventProcessorHost, preferably on separate machines.</span></span>  <span data-ttu-id="d345c-138">To poskytuje také redundanci.</span><span class="sxs-lookup"><span data-stu-id="d345c-138">This provides redundancy as well.</span></span> <span data-ttu-id="d345c-139">V těchto případech se spolu různé instance navzájem automaticky koordinují, aby dokázaly vyrovnávat zatížení přijatých událostí.</span><span class="sxs-lookup"><span data-stu-id="d345c-139">In those cases, the various instances automatically coordinate with each other in order to load balance the received events.</span></span> <span data-ttu-id="d345c-140">Pokud chcete, aby každý z několika příjemců zpracovával *všechny* události, musíte použít koncept **ConsumerGroup**.</span><span class="sxs-lookup"><span data-stu-id="d345c-140">If you want multiple receivers to each process *all* the events, you must use the **ConsumerGroup** concept.</span></span> <span data-ttu-id="d345c-141">Když přijímáte události z různých počítačů, může být užitečné nazvat instance třídy EventProcessorHost podle počítačů (nebo rolí), ve kterých jsou nasazené.</span><span class="sxs-lookup"><span data-stu-id="d345c-141">When receiving events from different machines, it might be useful to specify names for EventProcessorHost instances based on the machines (or roles) in which they are deployed.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="d345c-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d345c-142">Next steps</span></span>
<span data-ttu-id="d345c-143">Další informace o službě Event Hubs najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="d345c-143">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="d345c-144">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="d345c-144">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="d345c-145">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="d345c-145">Create an Event Hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="d345c-146">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="d345c-146">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Azure Storage account]: ../storage/common/storage-create-storage-account.md
[Azure portal]: https://portal.azure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png
