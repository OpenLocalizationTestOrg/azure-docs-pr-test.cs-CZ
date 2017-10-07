---
title: "aaaAdministration a vývoj seznamu úloh ve službě BizTalk Services | Microsoft Docs"
description: "Plánování a úlohy podpory pro nasazení služby Azure BizTalk Services."
services: biztalk-services
documentationcenter: 
author: msftman
manager: erikre
editor: 
ms.assetid: 0ab70b5b-1a88-4ba5-b329-ec51b785010e
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.openlocfilehash: 544c6b23fcbc2267598b713dbe1626699099d181
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="administration-and-development-task-list-in-biztalk-services"></a>Správu a vývoj seznamu úloh ve službě BizTalk Services

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

## <a name="getting-started"></a>Začínáme
Při práci se službou Microsoft Azure BizTalk Services, je několik místní a cloudové součásti tooconsider. tooget spustili, vezměte v úvahu následující tok procesu hello:  

| Krok | Kdo odpovídá | Úkol | Související odkazy |
| --- | --- | --- | --- |
| 1. |Správce |Vytvoření hello předplatnému Microsoft Azure pomocí účtu Microsoft nebo účtu organizace |[Portál Azure Classic](http://go.microsoft.com/fwlink/p/?LinkID=213885) |
| 2. |Správce |Vytvořit nebo zřizování služby BizTalk. |[Vytvoření služby BizTalk pomocí portálu Azure classic](http://go.microsoft.com/fwlink/p/?LinkID=302280) |
| 3. |Správce |Registrace vám nebo vaší společnosti nasazení služby BizTalk Services |[Registrace a aktualizace nasazení služby BizTalk v hello portál služby BizTalk](https://msdn.microsoft.com/library/azure/hh689837.aspx) |
| 4. |Správce |Platí v případě, že aplikace hello používá služba BizTalk Adapter Service tooconnect tooan v místním obchodní (LOB) systému nebo používá fronta nebo téma cílový.  Vytvořte hello Azure Service Bus Namespace. Udělte tento obor názvů, název vystavitele sběrnice služby a klíč vystavitele sběrnice služby hodnoty toohello developer. |[Postupy: vytvoření nebo úprava Namespace služby Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) a [hodnoty získat název vystavitele a klíč vystavitele](biztalk-issuer-name-issuer-key.md) |
| 5. |Developer |Nainstalujte hello SDK a vytvořte hello projektu služby BizTalk v sadě Visual Studio. |[Instalace služby Azure BizTalk Services SDK](https://msdn.microsoft.com/library/azure/hh689760.aspx) a [vytvoření bohaté zasílání zpráv koncových bodů v Azure](https://msdn.microsoft.com/library/azure/hh689766.aspx) |
| 6. |Developer |Nasazení projektu tooyour vaší služby BizTalk, které služba BizTalk hostované v Azure. |[Nasazení a aktualizaci hello projektu služby BizTalk](https://msdn.microsoft.com/library/azure/hh689881.aspx) |
| 7. |Správce |Pokud používáte EDI se vztahuje.  Můžete přidat partnery a vytvořte smlouvy na hello portálu Microsoft Azure BizTalk Services. Když vytvoříte smlouvu, můžete přidat most hello nebo transformací vytvořené hello vývojáře toohello smlouvy nastavení. |[Konfigurace EDI a AS2, EDIFACT na portálu služby BizTalk](https://msdn.microsoft.com/library/azure/hh689853.aspx) |
| 8. |Správce |Pomocí hello portál Azure classic, sledování stavu hello vaší služby BizTalk, včetně metrik výkonu. |[BizTalk Services: Karty Řídicí panel, Sledování a Škálování](http://go.microsoft.com/fwlink/p/?LinkID=302281) |
| 9. |Správce |Pomocí hello portálu Microsoft Azure BizTalk Services, spravujte hello artefakty používané služby BizTalk Services a sledování zpráv, jako jsou zpracovány pomocí hello most soubory. |[Pomocí hello portál služby BizTalk](https://msdn.microsoft.com/library/azure/dn874043.aspx) |
| 10. |Správce |Vytvoření plánu zálohování tooback až hello služby BizTalk. |[Kontinuita podnikových procesů a zotavení po havárii ve službě BizTalk Services](https://msdn.microsoft.com/library/azure/dn509557.aspx) |

## <a name="next-steps"></a>Další kroky
[Výukové programy a ukázky](https://msdn.microsoft.com/library/azure/hh689895.aspx)

[Vytvoření projektu hello v sadě Visual Studio](https://msdn.microsoft.com/library/azure/hh689811.aspx)

[Instalace služby Azure BizTalk Services SDK](https://msdn.microsoft.com/library/azure/hh689760.aspx)

## <a name="concepts"></a>Koncepty
[Vytvoření projektu hello v sadě Visual Studio](https://msdn.microsoft.com/library/azure/hh689811.aspx)  
[EDI, AS2 a EDIFACT zasílání zpráv (Business tooBusiness)](https://msdn.microsoft.com/library/azure/hh689898.aspx)  

## <a name="other-resources"></a>Další prostředky
[Přidání zdroje, cílové a most koncové body pro zasílání zpráv](https://msdn.microsoft.com/library/azure/hh689877.aspx)  
[Další informace a vytvoření mapy zpráv a transformace](https://msdn.microsoft.com/library/azure/hh689905.aspx)  
[Pomocí hello adaptér služby BizTalk (BAS)](https://msdn.microsoft.com/library/azure/hh689889.aspx)  
[Služby Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=303664)

