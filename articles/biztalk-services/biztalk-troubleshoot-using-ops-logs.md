---
title: "BizTalk Services aaaTroubleshoot pomocí protokoly operací | Microsoft Docs"
description: "Řešení potíží s služby BizTalk Services pomocí protokoly operací. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 1081a9c6-58cc-4a76-8ac8-11e5e7a6ea27
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 102779ed6e29784f190c28e4102a7d9670614914
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk Services: Řešení problémů pomocí protokolů operací

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

## <a name="what-are-hello-operation-logs"></a>Jaké jsou protokoly operací hello
Protokoly operací je funkce služby správy, k dispozici v hello portál Azure classic, který vám umožní tooview historické protokoly operací týkajících se služeb Azure, včetně služby BizTalk Services. To vám umožní tooview historická data související s operací toomanagement na své předplatné služby BizTalk až 180 dní.

> [!NOTE]
> Tato funkce zaznamená pouze protokoly pro operace správy v BizTalk Services, například při spuštění služby hello, založenou na aktivní, a tak dále. Tyto operace jsou sledovány bez ohledu na to, jestli se provedlo z hello portál Azure classic nebo pomocí hello [rozhraní API REST služby BizTalk](http://msdn.microsoft.com/library/azure/dn232347.aspx). Úplný seznam operací, které jsou sledovány pomocí služby správy najdete v tématu [Operations sledovaných pomocí služeb Azure Management](#bizops).<br/><br/>
> Není to zachytit hello protokoly pro aktivity související tooBizTalk běh služby (například zprávy zpracovává mostů atd.). tooview tyto protokoly, použijte hello zobrazení sledování z portálu služby BizTalk Services hello. Další informace najdete v tématu [sledování zpráv](http://msdn.microsoft.com/library/azure/hh949805.aspx).
> 
> 

## <a name="view-biztalk-services-operation-logs"></a>Zobrazení, protokoly operací služby BizTalk Services
1. V hello portál Azure classic, vyberte **Management Services**a potom vyberte hello **protokoly operací** kartě.
2. Můžete filtrovat na základě různých parametrů jako předplatné, rozsah kalendářních dat, typu služby (např. služby BizTalk), název služby nebo stav operace hello (úspěšné, neúspěšné) protokoly hello.
3. Vyberte hello zaškrtnutí tooview hello filtrovaný seznam. Hello následující obrázek znázorňuje aktivity související tootestbiztalkservice: ![zobrazit protokoly operací][ViewLogs] 
4. Vyberte řádek hello tooview Další informace o určité operace a klikněte na tlačítko **podrobnosti** hello hlavním panelu v dolní části hello.

## <a name="bizops"></a>Operace sledovat pomocí služby Azure pro
Hello následující tabulka uvádí hello operace, které jsou sledovány pomocí služeb Azure Management hello:

| Název operace | Úkol |
| --- | --- |
| CreateBizTalkService |Operace toocreate novou službu BizTalk |
| DeleteBizTalkService |Operace toodelete služby BizTalk |
| RestartBizTalkService |Operace toorestart služby BizTalk |
| StartBizTalkService |Operace toostart služby BizTalk |
| StopBizTalkService |Operace toostop služby BizTalk |
| DisableBizTalkService |Operace toodisable služby BizTalk |
| EnableBizTalkService |Operace tooenable služby BizTalk |
| BackupBizTalkService |Operace tooback až služby BizTalk |
| RestoreBizTalkService |Operace toorestore služby BizTalk ze zadané zálohy |
| SuspendBizTalkService |Operace toosuspend služby BizTalk |
| ResumeBizTalkService |Operace tooresume služby BizTalk |
| ScaleBizTalkService |Operace tooscale může služba BizTalk nahoru nebo dolů |
| ConfigUpdateBizTalkService |Operace tooupdate hello konfigurace služby BizTalk |
| ServiceUpdateBizTalkService |Operace tooupgrade nebo přechod na starší verzi jinou verzi tooa služby BizTalk |
| PurgeBackupBizTalkService |Operace zálohování toopurge hello služby BizTalk mimo dobu uchování hello |

## <a name="see-also"></a>Viz také
* [Zálohování služby BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [Služba BizTalk obnovit ze zálohy](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk Services: Developer, Basic, Standard a Premium tabulka edic](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk Services: Zřízení pomocí Azure portál classic](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Tabulka stavů zřízení](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Karty Řídicí panel, Sledování a Škálování](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Omezování](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Název a klíč vystavitele](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Jak začít používat hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png

