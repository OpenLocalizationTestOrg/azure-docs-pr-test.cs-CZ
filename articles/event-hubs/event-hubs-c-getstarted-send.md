---
title: "Odesílání událostí do centra událostí Azure pomocí jazyka C | Microsoft Docs"
description: "Odesílání událostí do centra událostí Azure pomocí jazyka C"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: c
ms.devlang: csharp
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: a615ee39b6c3731cc7df366e9fabeed5219a71b4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="send-events-to-azure-event-hubs-using-c"></a><span data-ttu-id="54074-103">Odesílání událostí do centra událostí Azure pomocí jazyka C</span><span class="sxs-lookup"><span data-stu-id="54074-103">Send events to Azure Event Hubs using C</span></span>

## <a name="introduction"></a><span data-ttu-id="54074-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="54074-104">Introduction</span></span>
<span data-ttu-id="54074-105">Event Hubs je vysoce škálovatelná služba, kterou lze přijímat miliony událostí za sekundu, povolení aplikaci zpracovávat a analyzovat masivní objemy dat vytvářených připojených zařízení a aplikací.</span><span class="sxs-lookup"><span data-stu-id="54074-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application to process and analyze the massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="54074-106">Až se shromáždí do centra událostí, můžete transformovat a ukládat data pomocí úložného clusteru nebo všechny zprostředkovatele datové analýzy v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="54074-106">Once collected into an event hub, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="54074-107">Další informace najdete v tématu [Přehled služby Event Hubs] [Přehled služby Event Hubs].</span><span class="sxs-lookup"><span data-stu-id="54074-107">For more information, please see the [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="54074-108">V tomto kurzu se dozvíte, jak odesílat události do centra událostí pomocí konzolové aplikace v C. Chcete-li přijímat události, klikněte na tlačítko přijímající příslušný jazyk v levé tabulce obsahu.</span><span class="sxs-lookup"><span data-stu-id="54074-108">In this tutorial, you will learn how to send events to an event hub using a console application in C. To receive events, click the appropriate receiving language in the left-hand table of contents.</span></span>

<span data-ttu-id="54074-109">K dokončení tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="54074-109">To complete this tutorial, you will need the following:</span></span>

* <span data-ttu-id="54074-110">Prostředí pro vývoj C.</span><span class="sxs-lookup"><span data-stu-id="54074-110">A C development environment.</span></span> <span data-ttu-id="54074-111">V tomto kurzu budeme předpokládat zásobníku RSZ ve virtuálním počítači Azure Linux s Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="54074-111">For this tutorial, we will assume the gcc stack on an Azure Linux VM with Ubuntu 14.04.</span></span>
* <span data-ttu-id="54074-112">[Sadu Microsoft Visual Studio](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="54074-112">[Microsoft Visual Studio](https://www.visualstudio.com/).</span></span>
* <span data-ttu-id="54074-113">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="54074-113">An active Azure account.</span></span> <span data-ttu-id="54074-114">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="54074-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="54074-115">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="54074-115">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="send-messages-to-event-hubs"></a><span data-ttu-id="54074-116">Zasílání zpráv do služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="54074-116">Send messages to Event Hubs</span></span>
<span data-ttu-id="54074-117">V této části jsme zápisu aplikace C odesílat události do vašeho centra událostí.</span><span class="sxs-lookup"><span data-stu-id="54074-117">In this section we write a C app to send events to your event hub.</span></span> <span data-ttu-id="54074-118">Kód používá knihovnu AMQP kanálem z [Apache Qpid projektu](http://qpid.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="54074-118">The code uses the Proton AMQP library from the [Apache Qpid project](http://qpid.apache.org/).</span></span> <span data-ttu-id="54074-119">Toto je obdobou pomocí front Service Bus a témat s AMQP z C, jak je znázorněno [zde](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504).</span><span class="sxs-lookup"><span data-stu-id="54074-119">This is analogous to using Service Bus queues and topics with AMQP from C as shown [here](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504).</span></span> <span data-ttu-id="54074-120">Další informace najdete v tématu [Qpid kanálem dokumentaci](http://qpid.apache.org/proton/index.html).</span><span class="sxs-lookup"><span data-stu-id="54074-120">For more information, see [Qpid Proton documentation](http://qpid.apache.org/proton/index.html).</span></span>

1. <span data-ttu-id="54074-121">Z [Qpid AMQP Messenger stránky](https://qpid.apache.org/proton/messenger.html), postupujte podle pokynů k instalaci Qpid kanálem, v závislosti na vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="54074-121">From the [Qpid AMQP Messenger page](https://qpid.apache.org/proton/messenger.html), follow the instructions to install Qpid Proton, depending on your environment.</span></span>
2. <span data-ttu-id="54074-122">Kompilace knihovně kanálem, nainstalujte následující balíčky:</span><span class="sxs-lookup"><span data-stu-id="54074-122">To compile the Proton library, install the following packages:</span></span>
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. <span data-ttu-id="54074-123">Stažení [Qpid kanálem knihovny](http://qpid.apache.org/proton/index.html)a rozbalte ho, například:</span><span class="sxs-lookup"><span data-stu-id="54074-123">Download the [Qpid Proton library](http://qpid.apache.org/proton/index.html), and extract it, e.g.:</span></span>
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. <span data-ttu-id="54074-124">Vytvoření adresáře sestavení, kompilace a instalace:</span><span class="sxs-lookup"><span data-stu-id="54074-124">Create a build directory, compile and install:</span></span>
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. <span data-ttu-id="54074-125">Pracovní adresář, vytvořte nový soubor s názvem **sender.c** následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="54074-125">In your work directory, create a new file called **sender.c** with the following code.</span></span> <span data-ttu-id="54074-126">Nezapomeňte nahradit hodnoty pro název centra událostí a název oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="54074-126">Remember to substitute the value for your event hub name and namespace name.</span></span> <span data-ttu-id="54074-127">Také je třeba nahradit verzi klíč pro kódovaná adresou URL **SendRule** vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="54074-127">You must also substitute a URL-encoded version of the key for the **SendRule** created earlier.</span></span> <span data-ttu-id="54074-128">Můžete kódování URL ho [zde](http://www.w3schools.com/tags/ref_urlencode.asp).</span><span class="sxs-lookup"><span data-stu-id="54074-128">You can URL-encode it [here](http://www.w3schools.com/tags/ref_urlencode.asp).</span></span>
   
    ```c
    #include "proton/message.h"
    #include "proton/messenger.h"
   
    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>
   
    #define check(messenger)                                                     
      {                                                                          
        if(pn_messenger_errno(messenger))                                        
        {                                                                        
          printf("check\n");                                                     
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); 
        }                                                                        
      }  
   
    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  
   
    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }
   
    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";
   
        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();
   
        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);
   
        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));
   
        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);
   
        pn_message_free(message);
    }
   
    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");
   
        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);
   
        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }
   
        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);
   
        return 0;
    }
    ```
6. <span data-ttu-id="54074-129">Kompilace souboru, za předpokladu, že **RSZ**:</span><span class="sxs-lookup"><span data-stu-id="54074-129">Compile the file, assuming **gcc**:</span></span>
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > <span data-ttu-id="54074-130">Tento kód používáme okno s odchozí 1 Vynutit zprávy se co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="54074-130">In this code, we use an outgoing window of 1 to force the messages out as soon as possible.</span></span> <span data-ttu-id="54074-131">Obecně platí aplikace se pokuste batch zprávy a pokuste se zvýšit propustnost.</span><span class="sxs-lookup"><span data-stu-id="54074-131">In general, your application should try to batch messages to increase throughput.</span></span> <span data-ttu-id="54074-132">Najdete v článku [Qpid AMQP Messenger stránky](https://qpid.apache.org/proton/messenger.html) informace o tom, jak v knihovně Qpid kanálem v tomto a dalších prostředích a z platforem, pro které jsou k dispozici vazby (aktuálně Perl, PHP, Python a Ruby).</span><span class="sxs-lookup"><span data-stu-id="54074-132">See the [Qpid AMQP Messenger page](https://qpid.apache.org/proton/messenger.html) for information about how to use the Qpid Proton library in this and other environments, and from platforms for which bindings are provided (currently Perl, PHP, Python, and Ruby).</span></span>


## <a name="next-steps"></a><span data-ttu-id="54074-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="54074-133">Next steps</span></span>
<span data-ttu-id="54074-134">Další informace o službě Event Hubs najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="54074-134">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="54074-135">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="54074-135">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md
)
* [<span data-ttu-id="54074-136">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="54074-136">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="54074-137">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="54074-137">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
