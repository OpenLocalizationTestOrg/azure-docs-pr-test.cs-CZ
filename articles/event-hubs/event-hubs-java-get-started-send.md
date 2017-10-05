---
title: "Odesílání událostí do centra událostí Azure pomocí Java | Microsoft Docs"
description: "Začínáme odesílá do centra událostí se používá Java"
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
ms.openlocfilehash: b31771001989e20b88bc8d7bca1afceb58ec197c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="send-events-to-azure-event-hubs-using-java"></a><span data-ttu-id="90b96-103">Odesílání událostí do centra událostí Azure používá Java</span><span class="sxs-lookup"><span data-stu-id="90b96-103">Send events to Azure Event Hubs using Java</span></span>

## <a name="introduction"></a><span data-ttu-id="90b96-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="90b96-104">Introduction</span></span>
<span data-ttu-id="90b96-105">Event Hubs je vysoce škálovatelná služba, kterou lze přijímat miliony událostí za sekundu, povolení aplikaci zpracovávat a analyzovat masivní objemy dat vytvářených připojených zařízení a aplikací.</span><span class="sxs-lookup"><span data-stu-id="90b96-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application to process and analyze the massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="90b96-106">Až se shromáždí do centra událostí, můžete transformovat a ukládat data pomocí úložného clusteru nebo všechny zprostředkovatele datové analýzy v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="90b96-106">Once collected into an event hub, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="90b96-107">Další informace najdete v tématu [Přehled služby Event Hubs][Event Hubs overview].</span><span class="sxs-lookup"><span data-stu-id="90b96-107">For more information, see the [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="90b96-108">Tento kurz ukazuje, jak odesílat události do centra událostí pomocí konzolové aplikace v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="90b96-108">This tutorial shows how to send events to an event hub by using a console application in Java.</span></span> <span data-ttu-id="90b96-109">Chcete-li přijímat události pomocí knihovny Java Event Processor Host, přečtěte si téma [v tomto článku](event-hubs-java-get-started-receive-eph.md), nebo klikněte na příslušný jazyk přijímající v levé tabulce obsahu.</span><span class="sxs-lookup"><span data-stu-id="90b96-109">To receive events using the Java Event Processor Host library, see [this article](event-hubs-java-get-started-receive-eph.md), or click the appropriate receiving language in the left-hand table of contents.</span></span>

<span data-ttu-id="90b96-110">K dokončení tohoto kurzu budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="90b96-110">In order to complete this tutorial, you will need the following:</span></span>

* <span data-ttu-id="90b96-111">Vývojové prostředí Java.</span><span class="sxs-lookup"><span data-stu-id="90b96-111">A Java development environment.</span></span> <span data-ttu-id="90b96-112">V tomto kurzu budeme předpokládat [Eclipse](https://www.eclipse.org/).</span><span class="sxs-lookup"><span data-stu-id="90b96-112">For this tutorial, we assume [Eclipse](https://www.eclipse.org/).</span></span>
* <span data-ttu-id="90b96-113">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="90b96-113">An active Azure account.</span></span> <br/><span data-ttu-id="90b96-114">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný účet.</span><span class="sxs-lookup"><span data-stu-id="90b96-114">If you don't have an account, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="90b96-115">Podrobnosti najdete v článku <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Bezplatná zkušební verze Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="90b96-115">For details, see <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure Free Trial</a>.</span></span>

## <a name="send-messages-to-event-hubs"></a><span data-ttu-id="90b96-116">Zasílání zpráv do služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="90b96-116">Send messages to Event Hubs</span></span>
<span data-ttu-id="90b96-117">Klientská knihovna Java pro službu Event Hubs je k dispozici pro použití v projektech Maven z [Maven centrálním úložišti](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span><span class="sxs-lookup"><span data-stu-id="90b96-117">The Java client library for Event Hubs is available for use in Maven projects from the [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span></span> <span data-ttu-id="90b96-118">Tato knihovna pomocí následující prohlášení závislostí v souboru projektu Maven, můžete odkazovat:</span><span class="sxs-lookup"><span data-stu-id="90b96-118">You can reference this library using the following dependency declaration inside your Maven project file:</span></span>    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```

<span data-ttu-id="90b96-119">Pro různé typy prostředí sestavení, můžete explicitně získat nejnovější vydaná JAR soubory z [Maven centrálním úložišti](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span><span class="sxs-lookup"><span data-stu-id="90b96-119">For different types of build environments, you can explicitly obtain the latest released JAR files from the [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span></span>  

<span data-ttu-id="90b96-120">Jednoduchá událost vydavatel, importovat *com.microsoft.azure.eventhubs* balíček pro třídy klienta služby Event Hubs a *com.microsoft.azure.servicebus* balíček pro nástroj třídy, jako běžné výjimky, které jsou sdíleny s klientem zasílání zpráv Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="90b96-120">For a simple event publisher, import the *com.microsoft.azure.eventhubs* package for the Event Hubs client classes and the *com.microsoft.azure.servicebus* package for utility classes such as common exceptions that are shared with the Azure Service Bus messaging client.</span></span> 

<span data-ttu-id="90b96-121">Pro následující příklad nejprve vytvořte nový projekt Maven pro aplikaci konzoly nebo prostředí v oblíbeném vývojovém prostředí Java.</span><span class="sxs-lookup"><span data-stu-id="90b96-121">For the following sample, first create a new Maven project for a console/shell application in your favorite Java development environment.</span></span> <span data-ttu-id="90b96-122">Název třídy `Send`.</span><span class="sxs-lookup"><span data-stu-id="90b96-122">Name the class `Send`.</span></span>     

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

<span data-ttu-id="90b96-123">Obor názvů a událostí názvy rozbočovačů nahraďte hodnoty používané při vytváření centra událostí.</span><span class="sxs-lookup"><span data-stu-id="90b96-123">Replace the namespace and event hub names with the values used when you created the event hub.</span></span>

```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

<span data-ttu-id="90b96-124">Pak vytvořte singulární událostí pomocí transformace řetězec na jeho kódování bajtů ve formátu UTF-8.</span><span class="sxs-lookup"><span data-stu-id="90b96-124">Then, create a singular event by transforming a string into its UTF-8 byte encoding.</span></span> <span data-ttu-id="90b96-125">Poté vytvořte novou instanci služby Event Hubs klienta z připojovacího řetězce a odeslat zprávu.</span><span class="sxs-lookup"><span data-stu-id="90b96-125">Then, create a new Event Hubs client instance from the connection string and send the message.</span></span>   

```java 

    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);

    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 

## <a name="next-steps"></a><span data-ttu-id="90b96-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="90b96-126">Next steps</span></span>
<span data-ttu-id="90b96-127">Další informace o službě Event Hubs najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="90b96-127">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="90b96-128">Přijímat události pomocí knihovny EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="90b96-128">Receive events using the EventProcessorHost</span></span>](event-hubs-java-get-started-receive-eph.md)
* <span data-ttu-id="90b96-129">[Přehled služby Event Hubs][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="90b96-129">[Event Hubs overview][Event Hubs overview]</span></span>
* [<span data-ttu-id="90b96-130">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="90b96-130">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="90b96-131">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="90b96-131">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md