---
title: "aaaTasks povoleny v různých stavů nebo stavy, které jsou ve službě BizTalk Services | Microsoft Docs"
description: "Hello povolené v různých MABS stav akce nebo operace: zastavit, spustit, restartovat, pozastavit, obnovit, odstranit, škálovat, aktualizace konfigurace a základní nahoru"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: aea738f3-ec76-4099-a41b-e17fea9e252f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2016
ms.author: mandia
ms.openlocfilehash: 643307ba6fa9b05c82b867912feab249c42b65dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-you-can-and-cant-do-using-hello-biztalk-service-state"></a>Můžete a nemůže provádět pomocí hello stavu služby BizTalk

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

V závislosti na hello aktuální stav hello služby BizTalk jsou operace, které lze nebo nelze provést na hello služby BizTalk.

Například můžete zřídit nové služby BizTalk v hello portál Azure classic. Po úspěšném dokončení, hello služba BizTalk je v `active` stavu. V aktivním stavu hello můžete zastavit, pozastavit a odstranění služby BizTalk hello. Pokud zastavíte službu BizTalk hello a zastavení selže, pak hello služby BizTalk přejde tooa `StopFailed` stavu. V hello `StopFailed` stavu, je možné restartovat službu BizTalk hello. Pokud se pokusíte operaci, která není povolena, jako je obnovení, dojde k následující chybě hello:

`Operation not allowed`

## <a name="view-hello-possible-states"></a>Možné stavy hello zobrazení

Hello následující tabulky operace výpisu hello nebo akce, které lze provést, pokud je hello služby BizTalk v určitém stavu. ✔ znamená, že hello operace je povolena v tomto stavu. Prázdná položka znamená, že hello operaci nelze provést, když v tomto stavu.

| Stav služby | Start | Zastavit | Restartování | Pozastavit | Obnovit | Odstranění | Měřítko | Aktualizace <br/> Konfigurace | Zálohování |
| --- | --- | --- | --- | --- | --- | --- |--- | --- | --- |
| Aktivní |  | ✔ | ✔ | ✔ |  | ✔ |✔ |✔ |✔ |
| Zakázáno |  |  |  |  |  | ✔ | |  |  | 
| pozastaveno |  |  |  |  | ✔ | ✔ | |  | ✔ |
| Zastaveno | ✔ |  | ✔ |  |  | ✔ | |  | ✔ |
| Aktualizace služby se nezdařila. |  |  |  |  |  | ✔ | |  |  | 
| DisableFailed |  |  |  |  |  | ✔ | |  |  | 
| EnableFailed |  |  |  |  |  | ✔ | |  |  | 
| StartFailed <br/> StopFailed <br/> RestartFailed | ✔ | ✔ | ✔ |  |  | ✔ | | ✔ | |
| SuspendedFailed <br/> ResumeFailed|  |  |  | ✔ | ✔ | ✔ | |  |  | 
| CreatedFailed <br/> RestoreFailed |  |  |  |  |  | ✔ | |  |  | 
| ConfigUpdateFailed  |  |  | ✔ |  |  | ✔ | |✔ | |
| ScaleFailed |  |  |  |  |  | ✔ |✔ | |  |  | 



## <a name="see-also"></a>Viz také
* [Vytvoření služby BizTalk pomocí hello portál Azure classic](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [Co můžete dělat v karty řídicí panel, sledování a škálování hello ve službě BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [Získat hello Developer, Basic, Standard a Premium edicích ve službě BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [Jak tooback a obnovení služby BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [Omezení podrobně BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
* [Načíst hello Service Bus a řízení přístupu vystavitele název a vystavitele klíčové hodnoty pro vaši službu BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
* [Jak začít používat hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

