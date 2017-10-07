---
title: "aaaReceive události z Azure Event Hubs pomocí Java | Microsoft Docs"
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
ms.openlocfilehash: 05414a22e6616296752c678bb0af887d6f070c12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="receive-events-from-azure-event-hubs-using-java"></a>Přijímat události z Azure Event Hubs pomocí Java


## <a name="introduction"></a>Úvod
Event Hubs je vysoce škálovatelná služba, která může přijímat miliony událostí za sekundu, povolení tooprocess aplikace a analyzovat masivní objemy dat vytvářených připojených zařízení a aplikace hello. Když Event Hubs shromáždí data, můžete je zpracovat a uložit pomocí libovolného úložného clusteru nebo poskytovatele datové analýzy v reálném čase.

Další informace najdete v tématu hello [Přehled služby Event Hubs][Event Hubs overview].

Tento kurz ukazuje, jak tooreceive události do centra událostí pomocí konzolové aplikace napsané v jazyce Java.

## <a name="prerequisites"></a>Požadavky

Pořadí toocomplete tento kurz, je třeba hello následující požadavky:

* Vývojové prostředí Java. V tomto kurzu budeme předpokládat [Eclipse](https://www.eclipse.org/).
* Aktivní účet Azure. <br/>Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný účet. Podrobnosti najdete v článku <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Bezplatná zkušební verze Azure</a>.

## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Přijímání zpráv pomocí třídy EventProcessorHost v Javě

**EventProcessorHost** je třída Java, která zjednodušuje přijímání události ze služby Event Hubs tím, že spravuje trvalé kontrolní body a paralelní příjemce událostí ze služby Event Hubs. Pomocí třídy EventProcessorHost, můžete události rozdělit mezi několik příjemců, i když jsou hostované v různých uzlech. Tento příklad ukazuje, jak toouse EventProcessorHost pro jednoho příjemce.

### <a name="create-a-storage-account"></a>vytvořit účet úložiště
toouse EventProcessorHost, musíte mít [účet úložiště Azure][Azure Storage account]:

1. Přihlaste se toohello [portál Azure][Azure portal]a klikněte na tlačítko **+ nový** na levé straně hello úvodní obrazovka.
2. Klikněte na **Storage** a poté klikněte na **Účet úložiště**. V hello **vytvořit účet úložiště** okno, zadejte název pro účet úložiště hello. Dokončení hello rest hello polí, vyberte požadovanou oblast a pak klikněte na tlačítko **vytvořit**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. Klikněte na tlačítko hello nově vytvořený účet úložiště a potom klikněte na **spravovat přístupové klíče**:
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    Zkopírujte hello primární přístupový klíč tooa dočasného umístění, toouse později v tomto kurzu.

### <a name="create-a-java-project-using-hello-eventprocessor-host"></a>Vytvoření projektu Java pomocí hello EventProcessor hostitele
Hello Java klientské knihovny pro službu Event Hubs je k dispozici pro použití v projektech Maven z hello [centrálním úložišti Maven][Maven Package]a můžete odkazovat pomocí hello následující závislost deklarace uvnitř vaší Soubor projektu maven:    

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

Pro různé typy prostředí sestavení, můžete explicitně získat souborů JAR hello nejnovější vydání z hello [centrálním úložišti Maven] [ Maven Package] nebo z [hello verze distribuční bod na GitHub](https://github.com/Azure/azure-event-hubs/releases).  

1. Následující ukázkový text hello nejprve vytvořte nový projekt Maven s pro aplikace konzoly nebo prostředí v oblíbených vývojové prostředí Java. je volána třída Hello `ErrorNotificationHandler`.     
   
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
2. Použití hello následující kód toocreate novou třídu s názvem `EventProcessor`.
   
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
3. Vytvořte jeden další třídu s názvem `EventProcessorSample`pomocí hello následující kód.
   
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
   
            System.out.println("Press enter toostop");
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
4. Nahraďte hello následující pole s hodnotami hello při vytvoření hello události rozbočovače a účet úložiště.
   
    ```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
   
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
   
    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [!NOTE]
> Tento kurz používá jednu instanci třídy EventProcessorHost. tooincrease propustnost, doporučujeme spustit víc instancí služby EventProcessorHost, pokud možno na samostatných počítačích.  To poskytuje také redundanci. V takových případech hello různé instance navzájem automaticky souřadnic v pořadí tooload vyrovnávání hello přijatých událostí. Pokud chcete, aby více příjemců tooeach proces *všechny* hello události, je nutné použít hello **ConsumerGroup** koncept. Když přijímáte události z různých počítačů, ve kterých jsou nasazené může být užitečné toospecify názvy instancí EventProcessorHost podle počítače hello (nebo rolí).
> 
> 

## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs návštěvou hello následující odkazy:

* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
* [Vytvoření centra událostí](event-hubs-create.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Azure Storage account]: ../storage/common/storage-create-storage-account.md
[Azure portal]: https://portal.azure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png
