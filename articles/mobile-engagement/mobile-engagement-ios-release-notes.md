---
title: "aaaAzure Mobile Engagement iOS SDK poznámky k verzi | Microsoft Docs"
description: "Nejnovější aktualizace a postupy pro iOS SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a43ff0f6-90d5-4b3c-8d7a-a1db21bc776b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: ae29d200ebb1784357b29edbd1f66b71df0778cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-ios-sdk-release-notes"></a>Poznámky k verzi sady Azure Mobile Engagement iOS SDK

## <a name="410-07172017"></a>4.1.0 (07/17/2017)
* Odznaky s pevnou vymazat v pozadí.
* Opravené upozornění na XCode 9 o volání rozhraní API není z hlavní fronty.
* Vyřešený nevracení paměti v hlasování Reach.
* Podpora pro iOS vyřadit 6.X. Od této verze hello nasazení cílové aplikace musí být minimálně iOS 7.

## <a name="401-12132016"></a>4.0.1 (12/13/2016)
* Vylepšení protokolu doručení v pozadí.

## <a name="400-09122016"></a>4.0.0 (09/12/2016)
* Opravené oznámení není reakcí na zařízeních s iOS 10.
* Přestat používat XCode 7.

## <a name="324-06302016"></a>3.2.4 (06/30/2016)
* Opravené agregace mezi technické protokoly a další protokoly.

## <a name="323-06072016"></a>3.2.3 (06/07/2016)
* Chyby opravené hello kde zpětnou vazbu o doručení není hlášena, když je aplikace hello pozadí.
* Optimalizované hello odesílání technické protokolů.

## <a name="322-04072016"></a>3.2.2 (04/07/2016)
* Opravené chyby na zrušení požadavku HTTP, což vede někdy toocrash.

## <a name="321-12112015"></a>3.2.1 (12/11/2015)
* Opravené hello zpoždění při aktivaci novou instanci aplikace podle oznámení s přímých odkazů

## <a name="320-10082015"></a>3.2.0 (10/08/2015)
* Povolit Bitcode v toomake SDK hello se pracovat s **Xcode 7**.
* Opravené chyby související s aplikací tooin oznámení.
* Oznámení v aplikaci hello provedené v případě nízký stav baterie. a dalších takových scénářů.
* Odebrat protokoly navíc konzoly generované 3. stran knihovny.

## <a name="310-08262015"></a>3.1.0 (08/26/2015)
* Opravte chyby kompatibility iOS 9 s knihovnou třetích stran. Že způsobuje dojde k chybě při odesílání dotazuje výsledky, informace o aplikaci nebo doplňující data.

## <a name="300-06192015"></a>3.0.0 (06/19/2015)
* Mobile Engagement používá bezobslužných nabízených oznámení.
* Podpora pro iOS vyřadit 4.X. Od této verze hello nasazení cílové aplikace musí být minimálně iOS 6.

## <a name="220-05212015"></a>2.2.0 (05/21/2015)
* id zařízení Hello Mobile Engagement pro zařízení < iOS 6 teď vychází identifikátor GUID generovány v době instalace.

## <a name="210-04242015"></a>2.1.0 (04/24/2015)
* Přidání Swift kompatibility.
* Když kliknete na oznámení, provést hello akci, kterou je adresa URL po otevření aplikace hello vpravo.
* Přidané chybí hlavička souboru v balíčku sady SDK.
* Oprava problému při hello Mobile Engagement havárií zpravodaje, která byla zakázána.

## <a name="200-02172015"></a>2.0.0 (02/17/2015)
* Počáteční verzi Azure Mobile Engagement
* Konfigurace appId/sdkKey je nahrazena konfiguraci řetězec připojení.
* Odebrat toosend rozhraní API a přijímat libovolné protokolu XMPP zprávy z libovolné protokolu XMPP entit.
* Odebrat toosend rozhraní API a přijímat zprávy mezi zařízeními.
* Vylepšení zabezpečení.
* Sledování SmartAd odebrat.
