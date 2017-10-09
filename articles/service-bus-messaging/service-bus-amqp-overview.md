---
title: aaaOverview protokolu AMQP 1.0 v Azure Service Bus | Microsoft Docs
description: "Další informace o použití hello Advanced zpráv služby Řízení front Protocol (AMQP) 1.0 v Azure."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 0e8d19cc-de36-478e-84ae-e089bbc2d515
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/27/2017
ms.author: sethm
ms.openlocfilehash: c35a20c50dd24845d9a4c7a0251e8240848a6f4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="amqp-10-support-in-service-bus"></a>Podpora protokolu AMQP 1.0 v Service Bus
Obě hello Cloudová služba Azure Service Bus a místní [sběrnice služby pro Windows Server (Service Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) podporu hello Advanced zpráv služby Řízení front Protocol (AMQP) 1.0. AMQP umožňuje vám toobuild napříč platformami, hybridní aplikace pomocí otevřený standardní protokol. Můžete vytvořit aplikací s použitím komponenty, které jsou integrovány v různých jazycích a architektury a které běží na různých operačních systémech. Všechny tyto součásti můžete připojit tooService sběrnice a bezproblémově výměna strukturovaných obchodní zpráv, efektivní a na úplné věrnosti.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Úvod: Co je protokolu AMQP 1.0 a proč je důležité?
Produkty zaměřené na konkrétní zprávu middleware tradičně používaly speciální protokoly pro komunikaci mezi klientskými aplikacemi a zprostředkovatelé. To znamená, že po dokončení výběru zasílání zpráv zprostředkovatele od dodavatele, je třeba použít od dodavatele knihovny tooconnect vaší klientské aplikace toothat zprostředkovatele. Portování jiným produktem tooa aplikace vyžaduje změny kódu ve všech aplikacích hello připojené výsledkem stupeň závislost na příslušného dodavatele. 

Kromě toho se připojit zprostředkovatelé zasílání zpráv od různých dodavatelů složité. To obvykle vyžaduje přemostění zprávy toomove na úrovni aplikace z jednoho systému tooanother a tootranslate mezi jejich formáty proprietární zpráv. Toto je běžné požadavek; například při musí zadat nové tooolder jednotné rozhraní různorodých systémů nebo integrovat následující fúzi systémy IT.

Hello softwarového oboru se o přesouvání fast firmu; Někdy nepřeberné tempem byly zavedeny nové programovací jazyky a architektur aplikací. Podobně hello požadavky systémy IT v průběhu času vyvíjejí a vývojáři mají tootake výhody nejnovějších funkcí platformy hello. Ale někdy hello vybraného zasílání zpráv dodavatele nepodporuje tyto platformy. Protože zasílání zpráv protokoly jsou vlastní, není možné pro ostatní tooprovide knihovny pro tyto nové platformy. Proto je nutné použít přístupy, jako je například vytváření mostů, které umožňují nebo brány toocontinue toouse hello zasílání zpráv produktu.

vývoj Hello hello Advanced zpráv služby Řízení front Protocol (AMQP) 1.0 byl motivované tyto problémy. Bylo provedeno v Chase Nováková JP, kdo jako firmami nejvíce finančních služeb, jsou náročné uživatele middleware orientovaný na zprávy. cílem Hello je jednoduchý: toocreate protokol otevřené standardní zasílání zpráv, který provedené ji možné toobuild na základě zpráv aplikace komponent vytvořených pomocí různých jazyků, architektury a operační systémy, všechny pomocí součásti nejlépe svého druhu ze rozsah dodavatelů.

## <a name="amqp-10-technical-features"></a>Technické funkce protokolu AMQP 1.0
AMQP 1.0 je efektivní, spolehlivou a úroveň protokolu zasílání zpráv, které můžete použít toobuild robustní a platformy, zasílání zpráv na úrovni aplikace. protokol Hello má jednoduchého cíle: toodefine hello mechanismů hello zabezpečeným, spolehlivým a efektivní přenosu zpráv mezi dvěma účastníky. samotné Hello zprávy jsou zakódovány pomocí znázornění přenosné datové umožňující heterogenní odesílateli a příjemci tooexchange strukturovaná obchodní zprávy na úplné přesnost. Hello Následuje souhrn hello nejdůležitější funkce:

* **Efektivní**: protokolu AMQP 1.0 je protokol orientovaná na připojení, používá binární kódování pro hello protokolu pokyny a hello firmy zprávy přenosu ho. Její součástí jsou sofistikované řízení toku schémata toomaximize hello využití sítě hello a součásti hello připojení. Ale nutné dodat, hello protokol byl navrženou toostrike rovnováhu mezi efektivitu, flexibilitu a vzájemná funkční spolupráce.
* **Spolehlivé**: hello protokol AMQP 1.0 umožňuje toobe zprávy vyměňují s rozsahem spolehlivost záruky, z ještě efektivněji a zapomněli tooreliable přesně-jednou potvrdí doručení.
* **Flexibilní**: protokolu AMQP 1.0 je flexibilní protokol, který lze použít toosupport topologiemi. stejný protokol Hello lze použít pro komunikaci klienta klienta, klient zprostředkovatele a zprostředkovatele zprostředkovatele.
* **Zprostředkovatel modelu nezávisle**: hello AMQP 1.0 – specifikace zajistěte, aby nebyly žádné požadavky pro zasílání zpráv model hello používá zprostředkovatel. To znamená, že je možné přidat tooeasily tooexisting podporu protokolu AMQP 1.0 zprostředkovatelé zasílání zpráv.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 je Standard (s na velké.)
AMQP 1.0 je standard mezinárodní schváleny ISO a IEC jako ISO/IEC 19464:2014.

AMQP 1.0 byl vyvíjen od 2008, Skupina základních víc než 20 společností technologie dodavatelé a podniky koncového uživatele. Během této doby uživatel podniky přispěla jejich skutečné obchodní požadavky a dodavatelé technologie hello vyvinuly hello protokol toomeet tyto požadavky. V průběhu procesu hello dodavatelé účastnili pracovní konference, ve kterých se spolupracovali toovalidate hello vzájemná funkční spolupráce mezi jejich implementace.

V říjnu 2011 vývoji hello přešla tooa technický výbor v rámci hello organizace pro hello rozvoj z strukturovaných informace standardy (OASIS) a hello OASIS AMQP 1.0 standardní byla vydána v říjen 2012. Hello následující podniky účastnili technický výbor hello při vývoji hello hello standardní:

* **Dodavatelé technologie**: Axway softwaru, Huawei technologie, IIT softwaru, INETCO systémy, Kaazing, Microsoft, Mitre Corporation, Primeton technologie, průběh softwaru, Red Hat, SITA, softwaru AG, Solace systémy, VMware, WSO2, Zenika.
* **Uživatel podniky**: Bank Amerika, platební Suisse, Boerse německých, Goldman Sachs, JPMorgan Chase.

Některé hello běžně citovalo, že otevřete standardy výhody patří:

* Menší riziko dodavatele zámku v
* Vzájemná funkční spolupráce
* Obecné dostupnosti knihovny a nástroje
* Ochrana proti životnosti
* Dostupnost zkušení pracovníci
* Menší a spravovat rizika

## <a name="amqp-10-and-service-bus"></a>AMQP 1.0 a Service Bus
Podpora protokolu AMQP 1.0 v Azure Service Bus znamená, že můžete nyní využít služby Řízení front Service Bus hello a publikování a přihlášení k odběru zprostředkované zasílání zpráv funkcí z rozsahu platforem a efektivní binární protokol. Kromě toho můžete vytvořit aplikace skládá z komponenty sestaven pomocí kombinace jazyky, architektury a operační systémy.

Hello následující obrázek ukazuje příklad nasazení ve které Java klientů se systémem Linux, vytvořené pomocí hello standardní Java zprávy služby (JMS) rozhraní API a rozhraní .NET klientů se systémem Windows, exchange zprávy přes Service Bus pomocí protokolu AMQP 1.0.

![][0]

**Obrázek 1: Příklad nasazení zobrazující napříč platformami zasílání zpráv pomocí sběrnice a protokolu AMQP 1.0**

V tento čas hello následujících klientských knihoven známé toowork službou Service Bus:

| Jazyk | Knihovna |
| --- | --- |
| Java |Klient Apache Qpid Java zprávy služby (JMS)<br/>SwiftMQ Java IIT softwaru klienta |
| C |Apache Qpid kanálem C |
| PHP |Apache Qpid kanálem PHP |
| Python |Apache Qpid kanálem Python |
| C# |AMQP .net Lite |

**Obrázek 2: Tabulka knihoven klienta protokolu AMQP 1.0**

## <a name="summary"></a>Souhrn
* AMQP 1.0 je otevřený, spolehlivé protokol zasílání zpráv, které můžete použít toobuild napříč platformami hybridní aplikace. AMQP 1.0 je OASIS standard.
* Podpora protokolu AMQP 1.0 je teď dostupná v Azure Service Bus, jakož i sběrnice služby pro Windows Server (Service Bus 1.1). Ceny je stejná jako u hello existující protokoly hello.

## <a name="next-steps"></a>Další kroky
Připraveno toolearn další? Hello najdete na následující odkazy:

* [Pomocí protokolu AMQP služby Service Bus pomocí technologie .NET]
* [Pomocí protokolu AMQP sběrnice z Javy]
* [Instalace Apache Qpid kanálem C ve virtuálním počítači Azure Linux]
* [AMQP v Service Bus pro systém Windows Server]

[0]: ./media/service-bus-amqp-overview/service-bus-amqp-1.png
[Pomocí protokolu AMQP služby Service Bus pomocí technologie .NET]: service-bus-amqp-dotnet.md
[Pomocí protokolu AMQP sběrnice z Javy]: service-bus-amqp-java.md
[Instalace Apache Qpid kanálem C ve virtuálním počítači Azure Linux]: service-bus-amqp-apache.md
[AMQP v Service Bus pro systém Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
