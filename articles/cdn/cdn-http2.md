---
title: Podpora aaaHTTP nebo 2 v Azure CDN | Microsoft Docs
description: "Další informace o podpoře protokolu HTTP/2 a CDN."
services: cdn
documentationcenter: 
author: lichard
manager: erikre
editor: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: rli
ms.openlocfilehash: 2e5e5345e8cf5c40e080ebf18b4f13a239a5aac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="http2-support-in-azure-cdn"></a>Podpora protokolu HTTP nebo 2 v Azure CDN

HTTP/2 je tooHTTP/1.1\ hlavní revize. Nabízí rychlejší webové výkon, sníženou odezvu a vylepšené uživatelské prostředí, při zachování hello známé metody HTTP, stavové kódy a sémantiku. I když HTTP/2 je navrženou toowork přes protokol HTTP a HTTPS, mnoha webových prohlížečích klienta přes protokol TLS podporují pouze HTTP/2.

###<a name="http2-benefits"></a>Výhody HTTP/2

Příklady výhod Hello HTTP/2:

*   **Multiplexní a souběžnost**

    Pomocí protokolu HTTP 1.1, více provádění více požadavků prostředků vyžaduje více připojení TCP a každé připojení má zatížení s ním spojená. HTTP/2 umožňuje více prostředků toobe požadovaný jednoho připojení TCP.

*   **Komprese záhlaví**

    Komprimací hello HTTP hlavičky pro obsloužit prostředky se výrazně snižuje času na hello přenosová.

*   **Datový proud závislosti**

    Datový proud závislosti povolit hello klienta tooindicate toohello server, který prostředků prioritou.


##<a name="http2-browser-support"></a>Podpora prohlížeče HTTP/2

Všechny hlavní prohlížeče hello v jejich aktuální verze implementovat podporu protokolu HTTP nebo 2. Nepodporovaná prohlížečů bude automaticky záložní tooHTTP/1.1.

|Prohlížeč|Minimální verze|
|-------------|------------|
|Microsoft Edge| 12|
|Google Chrome| 43|
|Mozilla Firefox| 38|
|Opera| 32|
|Safari| 9|

##<a name="enabling-http2-support-in-azure-cdn"></a>Povolení podpory protokolu HTTP nebo 2 v Azure CDN

Aktuálně je aktivní pro podporu protokolu HTTP/2 **Azure CDN společnosti Akamai** a **Azure CDN společnosti Verizon** profily. Žádná další akce je vyžadována od zákazníků.

##<a name="next-steps"></a>Další kroky

toosee hello výhody HTTP/2 v praxi, najdete v části [tuto ukázku společnosti Akamai](https://http2.akamai.com/demo).

Další informace o protokolu HTTP nebo 2, toolearn najdete na adrese hello následující prostředky:

*   [Specifikace protokolu HTTP/2 domovské stránky](https://http2.github.io/)
*   [Oficiální HTTP/2 – nejčastější dotazy](https://http2.github.io/faq/)
*   [Informace o Akamai HTTP/2](https://http2.akamai.com/)

toolearn Další informace o dostupných funkcí Azure CDN, najdete v části hello [přehled CDN Azure](https://azure.microsoft.com/documentation/articles/cdn-overview/).
