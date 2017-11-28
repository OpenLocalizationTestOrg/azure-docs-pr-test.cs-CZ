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
# <a name="send-events-tooazure-event-hubs-using-java"></a><span data-ttu-id="3c19c-103">Odesílání událostí tooAzure Event Hubs pomocí Java</span><span class="sxs-lookup"><span data-stu-id="3c19c-103">Send events tooAzure Event Hubs using Java</span></span>

## <a name="introduction"></a><span data-ttu-id="3c19c-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="3c19c-104">Introduction</span></span>
<span data-ttu-id="3c19c-105">Event Hubs je vysoce škálovatelná služba, která může přijímat miliony událostí za sekundu, povolení tooprocess aplikace a analyzovat masivní objemy dat vytvářených připojených zařízení a aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3c19c-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application tooprocess and analyze hello massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="3c19c-106">Až se shromáždí do centra událostí, můžete transformovat a ukládat data pomocí úložného clusteru nebo všechny zprostředkovatele datové analýzy v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="3c19c-106">Once collected into an event hub, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="3c19c-107">Další informace najdete v tématu hello [Přehled služby Event Hubs][Event Hubs overview].</span><span class="sxs-lookup"><span data-stu-id="3c19c-107">For more information, see hello [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="3c19c-108">Tento kurz ukazuje, jak toosend události tooan centra událostí pomocí konzolové aplikace v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="3c19c-108">This tutorial shows how toosend events tooan event hub by using a console application in Java.</span></span> <span data-ttu-id="3c19c-109">tooreceive událostí pomocí hello knihovně Java Event Processor Host, najdete v části [v tomto článku](event-hubs-java-get-started-receive-eph.md), nebo klikněte na příslušný přijímající jazyk hello v levé tabulce hello obsahu.</span><span class="sxs-lookup"><span data-stu-id="3c19c-109">tooreceive events using hello Java Event Processor Host library, see [this article](event-hubs-java-get-started-receive-eph.md), or click hello appropriate receiving language in hello left-hand table of contents.</span></span>

<span data-ttu-id="3c19c-110">V toocomplete pořadí v tomto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="3c19c-110">In order toocomplete this tutorial, you will need hello following:</span></span>

* <span data-ttu-id="3c19c-111">Vývojové prostředí Java.</span><span class="sxs-lookup"><span data-stu-id="3c19c-111">A Java development environment.</span></span> <span data-ttu-id="3c19c-112">V tomto kurzu budeme předpokládat [Eclipse](https://www.eclipse.org/).</span><span class="sxs-lookup"><span data-stu-id="3c19c-112">For this tutorial, we assume [Eclipse](https://www.eclipse.org/).</span></span>
* <span data-ttu-id="3c19c-113">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="3c19c-113">An active Azure account.</span></span> <br/><span data-ttu-id="3c19c-114">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný účet.</span><span class="sxs-lookup"><span data-stu-id="3c19c-114">If you don't have an account, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="3c19c-115">Podrobnosti najdete v článku <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Bezplatná zkušební verze Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="3c19c-115">For details, see <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure Free Trial</a>.</span></span>

## <a name="send-messages-tooevent-hubs"></a><span data-ttu-id="3c19c-116">Odesílání zpráv tooEvent rozbočovače</span><span class="sxs-lookup"><span data-stu-id="3c19c-116">Send messages tooEvent Hubs</span></span>
<span data-ttu-id="3c19c-117">Hello Java klientské knihovny pro službu Event Hubs je k dispozici pro použití v projektech Maven z hello [Maven centrálním úložišti](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span><span class="sxs-lookup"><span data-stu-id="3c19c-117">hello Java client library for Event Hubs is available for use in Maven projects from hello [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span></span> <span data-ttu-id="3c19c-118">Tato knihovna pomocí hello následující závislost deklarace v souboru projektu Maven, můžete odkazovat:</span><span class="sxs-lookup"><span data-stu-id="3c19c-118">You can reference this library using hello following dependency declaration inside your Maven project file:</span></span>    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```

<span data-ttu-id="3c19c-119">Pro různé typy prostředí sestavení, můžete explicitně získat souborů JAR hello nejnovější vydání z hello [Maven centrálním úložišti](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span><span class="sxs-lookup"><span data-stu-id="3c19c-119">For different types of build environments, you can explicitly obtain hello latest released JAR files from hello [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span></span>  

<span data-ttu-id="3c19c-120">Jednoduchá událost vydavatele, import hello *com.microsoft.azure.eventhubs* balíček pro třídy klienta hello Event Hubs a hello *com.microsoft.azure.servicebus* balíček pro nástroj třídy, například jako běžné výjimky, které jsou sdíleny s klientem zasílání zpráv Azure Service Bus hello.</span><span class="sxs-lookup"><span data-stu-id="3c19c-120">For a simple event publisher, import hello *com.microsoft.azure.eventhubs* package for hello Event Hubs client classes and hello *com.microsoft.azure.servicebus* package for utility classes such as common exceptions that are shared with hello Azure Service Bus messaging client.</span></span> 

<span data-ttu-id="3c19c-121">Následující ukázkový text hello nejprve vytvořte nový projekt Maven s pro aplikace konzoly nebo prostředí v oblíbených vývojové prostředí Java.</span><span class="sxs-lookup"><span data-stu-id="3c19c-121">For hello following sample, first create a new Maven project for a console/shell application in your favorite Java development environment.</span></span> <span data-ttu-id="3c19c-122">Název třídy hello `Send`.</span><span class="sxs-lookup"><span data-stu-id="3c19c-122">Name hello class `Send`.</span></span>     

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

<span data-ttu-id="3c19c-123">Nahraďte názvy hello obor názvů a události rozbočovače hello hodnoty použité při vytváření centra událostí hello.</span><span class="sxs-lookup"><span data-stu-id="3c19c-123">Replace hello namespace and event hub names with hello values used when you created hello event hub.</span></span>

```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

<span data-ttu-id="3c19c-124">Pak vytvořte singulární událostí pomocí transformace řetězec na jeho kódování bajtů ve formátu UTF-8.</span><span class="sxs-lookup"><span data-stu-id="3c19c-124">Then, create a singular event by transforming a string into its UTF-8 byte encoding.</span></span> <span data-ttu-id="3c19c-125">Pak vytvořte novou instanci služby Event Hubs klienta z hello připojovací řetězec a odeslat zprávu hello.</span><span class="sxs-lookup"><span data-stu-id="3c19c-125">Then, create a new Event Hubs client instance from hello connection string and send hello message.</span></span>   

```java 

    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);

    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 

## <a name="next-steps"></a><span data-ttu-id="3c19c-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c19c-126">Next steps</span></span>
<span data-ttu-id="3c19c-127">Další informace o službě Event Hubs návštěvou hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="3c19c-127">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="3c19c-128">Přijímat události pomocí hello EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="3c19c-128">Receive events using hello EventProcessorHost</span></span>](event-hubs-java-get-started-receive-eph.md)
* <span data-ttu-id="3c19c-129">[Přehled služby Event Hubs][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="3c19c-129">[Event Hubs overview][Event Hubs overview]</span></span>
* [<span data-ttu-id="3c19c-130">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="3c19c-130">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="3c19c-131">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="3c19c-131">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md