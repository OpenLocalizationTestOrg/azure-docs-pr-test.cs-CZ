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
# <a name="send-events-tooazure-event-hubs-using-c"></a>Odesílání událostí tooAzure Event Hubs pomocí jazyka C

## <a name="introduction"></a>Úvod
Event Hubs je vysoce škálovatelná služba, která může přijímat miliony událostí za sekundu, povolení tooprocess aplikace a analyzovat masivní objemy dat vytvářených připojených zařízení a aplikace hello. Až se shromáždí do centra událostí, můžete transformovat a ukládat data pomocí úložného clusteru nebo všechny zprostředkovatele datové analýzy v reálném čase.

Další informace najdete v tématu hello [Přehled služby Event Hubs] [Přehled služby Event Hubs].

V tomto kurzu se dozvíte, jak toosend události tooan centra událostí pomocí konzolové aplikace v č. tooreceive události, klikněte na příslušný přijímající jazyk hello v levé tabulce hello obsahu.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Prostředí pro vývoj C. V tomto kurzu budeme předpokládat hello RSZ zásobníku ve virtuálním počítači Azure Linux s Ubuntu 14.04.
* [Sadu Microsoft Visual Studio](https://www.visualstudio.com/).
* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="send-messages-tooevent-hubs"></a>Odesílání zpráv tooEvent rozbočovače
V této části jsme zápisu C aplikace toosend události tooyour centra událostí. Kód Hello používá hello knihovně kanálem AMQP z hello [Apache Qpid projektu](http://qpid.apache.org/). Toto je obdobou toousing fronty Service Bus a témat AMQP z C, jak je znázorněno [zde](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Další informace najdete v tématu [Qpid kanálem dokumentaci](http://qpid.apache.org/proton/index.html).

1. Z hello [Qpid AMQP Messenger stránky](https://qpid.apache.org/proton/messenger.html), postupujte podle pokynů tooinstall hello Qpid kanálem, v závislosti na vašem prostředí.
2. toocompile hello knihovně kanálem, nainstalujte hello následující balíčky:
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. Stáhnout hello [Qpid kanálem knihovny](http://qpid.apache.org/proton/index.html)a rozbalte ho, například:
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. Vytvoření adresáře sestavení, kompilace a instalace:
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. Pracovní adresář, vytvořte nový soubor s názvem **sender.c** s hello následující kód. Mějte na paměti, toosubstitute hello hodnotu pro název centra událostí a název oboru názvů. Také nahraďte kódovaná adresou URL verzi hello klíč pro hello **SendRule** vytvořili dříve. Můžete kódování URL ho [zde](http://www.w3schools.com/tags/ref_urlencode.asp).
   
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
6. Kompilace souboru hello za předpokladu, že **RSZ**:
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > V tomto kódu používáme okno s odchozí 1 tooforce hello zpráv se co nejdříve. Obecně platí vaše aplikace by měla zkuste toobatch zprávy tooincrease propustnost. V tématu hello [Qpid AMQP Messenger stránky](https://qpid.apache.org/proton/messenger.html) informace o tom, jak toouse hello Qpid kanálem knihovny v tomto a dalších prostředích a z platforem, pro které jsou k dispozici vazby (aktuálně Perl, PHP, Python a Ruby).


## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs návštěvou hello následující odkazy:

* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md
)
* [Vytvoření centra událostí](event-hubs-create.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
