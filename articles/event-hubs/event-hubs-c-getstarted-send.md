---
title: "aaaSend události tooAzure Event Hubs pomocí jazyka C | Microsoft Docs"
description: "Odesílání událostí tooAzure Event Hubs pomocí jazyka C"
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
ms.openlocfilehash: bb53300c070debb4a3658a38df9d3966f08e81ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-events-tooazure-event-hubs-using-c"></a><span data-ttu-id="c808e-103">Odesílání událostí tooAzure Event Hubs pomocí jazyka C</span><span class="sxs-lookup"><span data-stu-id="c808e-103">Send events tooAzure Event Hubs using C</span></span>

## <a name="introduction"></a><span data-ttu-id="c808e-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="c808e-104">Introduction</span></span>
<span data-ttu-id="c808e-105">Event Hubs je vysoce škálovatelná služba, která může přijímat miliony událostí za sekundu, povolení tooprocess aplikace a analyzovat masivní objemy dat vytvářených připojených zařízení a aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c808e-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application tooprocess and analyze hello massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="c808e-106">Až se shromáždí do centra událostí, můžete transformovat a ukládat data pomocí úložného clusteru nebo všechny zprostředkovatele datové analýzy v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="c808e-106">Once collected into an event hub, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="c808e-107">Další informace najdete v tématu hello [Přehled služby Event Hubs] [Přehled služby Event Hubs].</span><span class="sxs-lookup"><span data-stu-id="c808e-107">For more information, please see hello [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="c808e-108">V tomto kurzu se dozvíte, jak toosend události tooan centra událostí pomocí konzolové aplikace v č. tooreceive události, klikněte na příslušný přijímající jazyk hello v levé tabulce hello obsahu.</span><span class="sxs-lookup"><span data-stu-id="c808e-108">In this tutorial, you will learn how toosend events tooan event hub using a console application in C. tooreceive events, click hello appropriate receiving language in hello left-hand table of contents.</span></span>

<span data-ttu-id="c808e-109">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="c808e-109">toocomplete this tutorial, you will need hello following:</span></span>

* <span data-ttu-id="c808e-110">Prostředí pro vývoj C.</span><span class="sxs-lookup"><span data-stu-id="c808e-110">A C development environment.</span></span> <span data-ttu-id="c808e-111">V tomto kurzu budeme předpokládat hello RSZ zásobníku ve virtuálním počítači Azure Linux s Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="c808e-111">For this tutorial, we will assume hello gcc stack on an Azure Linux VM with Ubuntu 14.04.</span></span>
* <span data-ttu-id="c808e-112">[Sadu Microsoft Visual Studio](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="c808e-112">[Microsoft Visual Studio](https://www.visualstudio.com/).</span></span>
* <span data-ttu-id="c808e-113">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="c808e-113">An active Azure account.</span></span> <span data-ttu-id="c808e-114">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="c808e-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c808e-115">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c808e-115">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="send-messages-tooevent-hubs"></a><span data-ttu-id="c808e-116">Odesílání zpráv tooEvent rozbočovače</span><span class="sxs-lookup"><span data-stu-id="c808e-116">Send messages tooEvent Hubs</span></span>
<span data-ttu-id="c808e-117">V této části jsme zápisu C aplikace toosend události tooyour centra událostí.</span><span class="sxs-lookup"><span data-stu-id="c808e-117">In this section we write a C app toosend events tooyour event hub.</span></span> <span data-ttu-id="c808e-118">Kód Hello používá hello knihovně kanálem AMQP z hello [Apache Qpid projektu](http://qpid.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="c808e-118">hello code uses hello Proton AMQP library from hello [Apache Qpid project](http://qpid.apache.org/).</span></span> <span data-ttu-id="c808e-119">Toto je obdobou toousing fronty Service Bus a témat AMQP z C, jak je znázorněno [zde](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504).</span><span class="sxs-lookup"><span data-stu-id="c808e-119">This is analogous toousing Service Bus queues and topics with AMQP from C as shown [here](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504).</span></span> <span data-ttu-id="c808e-120">Další informace najdete v tématu [Qpid kanálem dokumentaci](http://qpid.apache.org/proton/index.html).</span><span class="sxs-lookup"><span data-stu-id="c808e-120">For more information, see [Qpid Proton documentation](http://qpid.apache.org/proton/index.html).</span></span>

1. <span data-ttu-id="c808e-121">Z hello [Qpid AMQP Messenger stránky](https://qpid.apache.org/proton/messenger.html), postupujte podle pokynů tooinstall hello Qpid kanálem, v závislosti na vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="c808e-121">From hello [Qpid AMQP Messenger page](https://qpid.apache.org/proton/messenger.html), follow hello instructions tooinstall Qpid Proton, depending on your environment.</span></span>
2. <span data-ttu-id="c808e-122">toocompile hello knihovně kanálem, nainstalujte hello následující balíčky:</span><span class="sxs-lookup"><span data-stu-id="c808e-122">toocompile hello Proton library, install hello following packages:</span></span>
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. <span data-ttu-id="c808e-123">Stáhnout hello [Qpid kanálem knihovny](http://qpid.apache.org/proton/index.html)a rozbalte ho, například:</span><span class="sxs-lookup"><span data-stu-id="c808e-123">Download hello [Qpid Proton library](http://qpid.apache.org/proton/index.html), and extract it, e.g.:</span></span>
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. <span data-ttu-id="c808e-124">Vytvoření adresáře sestavení, kompilace a instalace:</span><span class="sxs-lookup"><span data-stu-id="c808e-124">Create a build directory, compile and install:</span></span>
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. <span data-ttu-id="c808e-125">Pracovní adresář, vytvořte nový soubor s názvem **sender.c** s hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="c808e-125">In your work directory, create a new file called **sender.c** with hello following code.</span></span> <span data-ttu-id="c808e-126">Mějte na paměti, toosubstitute hello hodnotu pro název centra událostí a název oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="c808e-126">Remember toosubstitute hello value for your event hub name and namespace name.</span></span> <span data-ttu-id="c808e-127">Také nahraďte kódovaná adresou URL verzi hello klíč pro hello **SendRule** vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="c808e-127">You must also substitute a URL-encoded version of hello key for hello **SendRule** created earlier.</span></span> <span data-ttu-id="c808e-128">Můžete kódování URL ho [zde](http://www.w3schools.com/tags/ref_urlencode.asp).</span><span class="sxs-lookup"><span data-stu-id="c808e-128">You can URL-encode it [here](http://www.w3schools.com/tags/ref_urlencode.asp).</span></span>
   
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
        printf("Press Ctrl-C toostop hello sender process\n");
   
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
6. <span data-ttu-id="c808e-129">Kompilace souboru hello za předpokladu, že **RSZ**:</span><span class="sxs-lookup"><span data-stu-id="c808e-129">Compile hello file, assuming **gcc**:</span></span>
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > <span data-ttu-id="c808e-130">V tomto kódu používáme okno s odchozí 1 tooforce hello zpráv se co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="c808e-130">In this code, we use an outgoing window of 1 tooforce hello messages out as soon as possible.</span></span> <span data-ttu-id="c808e-131">Obecně platí vaše aplikace by měla zkuste toobatch zprávy tooincrease propustnost.</span><span class="sxs-lookup"><span data-stu-id="c808e-131">In general, your application should try toobatch messages tooincrease throughput.</span></span> <span data-ttu-id="c808e-132">V tématu hello [Qpid AMQP Messenger stránky](https://qpid.apache.org/proton/messenger.html) informace o tom, jak toouse hello Qpid kanálem knihovny v tomto a dalších prostředích a z platforem, pro které jsou k dispozici vazby (aktuálně Perl, PHP, Python a Ruby).</span><span class="sxs-lookup"><span data-stu-id="c808e-132">See hello [Qpid AMQP Messenger page](https://qpid.apache.org/proton/messenger.html) for information about how toouse hello Qpid Proton library in this and other environments, and from platforms for which bindings are provided (currently Perl, PHP, Python, and Ruby).</span></span>


## <a name="next-steps"></a><span data-ttu-id="c808e-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c808e-133">Next steps</span></span>
<span data-ttu-id="c808e-134">Další informace o službě Event Hubs návštěvou hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="c808e-134">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="c808e-135">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="c808e-135">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md
)
* [<span data-ttu-id="c808e-136">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="c808e-136">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="c808e-137">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="c808e-137">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
