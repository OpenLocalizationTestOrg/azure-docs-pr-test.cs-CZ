---
title: "aaaLearn o omezování ve službě BizTalk Services | Microsoft Docs"
description: "Další informace o omezování prahové hodnoty a výsledná modul runtime chování služby BizTalk Services. Omezení je na základě využití paměti a počet zpráv. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: f6663cf2-cda4-4bac-855e-27d2ad5c4fa4
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 46c8806c3a1f4eeb793f721f849771e0ec155197
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-throttling"></a>BizTalk Services: Omezování

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Služba Azure BizTalk Services implementuje služby omezování na základě podmínek, dvě: paměti využití a hello počet souběžných zpracování zpráv. Toto téma uvádí hello omezení prahové hodnoty a popisuje hello modul Runtime chování, když dojde k omezení podmínku.

## <a name="throttling-thresholds"></a>Omezení šířky pásma prahové hodnoty
Následující tabulka seznamy Hello hello omezení zdroje a prahové hodnoty:

|  | Popis | Nízké prahové hodnoty | Horní prahové hodnoty |
| --- | --- | --- | --- |
| Memory (Paměť) |% z celkových systémových paměti k dispozici nebo PageFileBytes. <p><p>Celkový počet dostupných PageFileBytes je přibližně 2 časy hello RAM hello systému. |60% |70% |
| Zpracování zpráv |Počet současně zpracování zpráv |40 * počet jader |100 * počet jader |

Když je dosaženo horní prahové hodnoty, spustí se služba Azure BizTalk Services toothrottle. Omezení zastaví, když je dosaženo hello nízké prahové hodnoty. Například služby používá 65 % systémové paměti. V takovém případě není omezení služby hello. Vaše služba spustí pomocí 70 % systémové paměti. V takovém případě služba hello omezí generovaný a toothrottle se opakuje, dokud služba hello používá 60 % (nízké prahové hodnoty) systémové paměti.

Služba Azure BizTalk Services sleduje hello omezení stav (normální stav oproti omezenému stavu) a hello omezení doby trvání.

## <a name="runtime-behavior"></a>Modul runtime chování
Když služba Azure BizTalk Services vstupuje do stavu omezení, dojde k hello následující:

* Omezení je na instanci role. Například:<br/>
  RoleInstanceA je omezení. RoleInstanceB není omezení. V takovém případě zpracování zpráv v RoleInstanceB podle očekávání. Zprávy v RoleInstanceA se zahodí a dojde k chybě hello následující chybě:<br/><br/>
  **Server je zaneprázdněn. Zkuste to prosím znovu.**<br/><br/>
* Žádné zdroje vyžádání nezadávejte dotazovat nebo stáhnout zprávy. Například:<br/>
  Kanál vrátí zprávy z externího zdroje FTP. Získá instanci role Hello provádění hello vyžádání do stavu omezení. V takovém případě hello kanál zastaví stahování další zprávy, dokud hello role instance zastaví, omezení šířky pásma.
* Odpověď je odeslat toohello klienta, aby hello klienta můžete znovu odeslat uvítací zprávu.
* Je nutné počkat, až do vyřešení hello omezení. Konkrétně je nutné počkat, dokud nebude dosaženo hello nízké prahové hodnoty.

## <a name="important-notes"></a>Důležité poznámky
* Omezení nelze zakázat.
* Omezení šířky pásma prahové hodnoty nemůže být upraven.
* Omezení je implementováno celého systému.
* Hello Server databází SQL Azure má také předdefinované omezení.

## <a name="additional-azure-biztalk-services-topics"></a>Další témata týkající se služby Azure BizTalk Services
* [Instalace hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Kurzy: Služby Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [Jak začít používat hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Služby Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Viz také
* [BizTalk Services: Developer, Basic, Standard a Premium tabulka edic](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk Services: Zřízení pomocí Azure portál classic](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [BizTalk Services: Tabulka stavů zřízení](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: Karty Řídicí panel, Sledování a Škálování](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: Zálohování a obnovení](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: Název a klíč vystavitele](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>

