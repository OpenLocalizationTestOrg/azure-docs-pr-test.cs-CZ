---
title: "aaaSend události tooAzure Event Hubs pomocí Java | Microsoft Docs"
description: "Začínáme odesílání tooEvent centra používá Java"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: ec537b8849a0cb49855e76c0c0ef4093108fe83c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-events-tooazure-event-hubs-using-java"></a>Odesílání událostí tooAzure Event Hubs pomocí Java

## <a name="introduction"></a>Úvod
Event Hubs je vysoce škálovatelná služba, která může přijímat miliony událostí za sekundu, povolení tooprocess aplikace a analyzovat masivní objemy dat vytvářených připojených zařízení a aplikace hello. Až se shromáždí do centra událostí, můžete transformovat a ukládat data pomocí úložného clusteru nebo všechny zprostředkovatele datové analýzy v reálném čase.

Další informace najdete v tématu hello [Přehled služby Event Hubs][Event Hubs overview].

Tento kurz ukazuje, jak toosend události tooan centra událostí pomocí konzolové aplikace v jazyce Java. tooreceive událostí pomocí hello knihovně Java Event Processor Host, najdete v části [v tomto článku](event-hubs-java-get-started-receive-eph.md), nebo klikněte na příslušný přijímající jazyk hello v levé tabulce hello obsahu.

V toocomplete pořadí v tomto kurzu budete potřebovat hello následující:

* Vývojové prostředí Java. V tomto kurzu budeme předpokládat [Eclipse](https://www.eclipse.org/).
* Aktivní účet Azure. <br/>Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný účet. Podrobnosti najdete v článku <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Bezplatná zkušební verze Azure</a>.

## <a name="send-messages-tooevent-hubs"></a>Odesílání zpráv tooEvent rozbočovače
Hello Java klientské knihovny pro službu Event Hubs je k dispozici pro použití v projektech Maven z hello [Maven centrálním úložišti](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Tato knihovna pomocí hello následující závislost deklarace v souboru projektu Maven, můžete odkazovat:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```

Pro různé typy prostředí sestavení, můžete explicitně získat souborů JAR hello nejnovější vydání z hello [Maven centrálním úložišti](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Jednoduchá událost vydavatele, import hello *com.microsoft.azure.eventhubs* balíček pro třídy klienta hello Event Hubs a hello *com.microsoft.azure.servicebus* balíček pro nástroj třídy, například jako běžné výjimky, které jsou sdíleny s klientem zasílání zpráv Azure Service Bus hello. 

Následující ukázkový text hello nejprve vytvořte nový projekt Maven s pro aplikace konzoly nebo prostředí v oblíbených vývojové prostředí Java. Název třídy hello `Send`.     

```java
import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Nahraďte názvy hello obor názvů a události rozbočovače hello hodnoty použité při vytváření centra událostí hello.

```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Pak vytvořte singulární událostí pomocí transformace řetězec na jeho kódování bajtů ve formátu UTF-8. Pak vytvořte novou instanci služby Event Hubs klienta z hello připojovací řetězec a odeslat zprávu hello.   

```java 

    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);

    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 

## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs návštěvou hello následující odkazy:

* [Přijímat události pomocí hello EventProcessorHost](event-hubs-java-get-started-receive-eph.md)
* [Přehled služby Event Hubs][Event Hubs overview]
* [Vytvoření centra událostí](event-hubs-create.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md