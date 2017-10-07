---
title: "Přehled sady Mobile Engagement Windows Phone Silverlight SDK aaaAzure | Microsoft Docs"
description: "Přehled hello Windows Phone Silverlight SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0e3d2420-0509-4952-8891-392e3dad9aaf
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2016
ms.author: piyushjo
ms.openlocfilehash: ff2febed2202127e0538373ebbabe674993ce39d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone Silverlight SDK přehled pro Azure Mobile Engagement
Začněte zde tooget hello podrobnosti o tom, toointegrate Azure Mobile Engagement v aplikaci Silverlight pro Windows Phone. Pokud chcete toogive ho zkuste to nejprve zkontrolujte, zda dokončení naše [15 minut kurzu](mobile-engagement-windows-phone-get-started.md).

Klikněte na tlačítko toosee hello [SDK obsahu](mobile-engagement-windows-phone-sdk-content.md)

## <a name="integration-procedures"></a>Integrace procedury
1. Začněte zde: [jak toointegrate Mobile Engagement v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
2. Pro oznámení: [jak toointegrate Reach (oznámení) v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement-reach.md)
3. Značka plán implementace: [jak toouse hello advanced Mobile Engagement označování rozhraní API v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-use-engagement-api.md)

## <a name="release-notes"></a>Poznámky k verzi
###<a name="331-11032016"></a>3.3.1 (11/03/2016)
Součást hello *MicrosoftAzure.MobileEngagement* balíček Nuget **v3.4.1**

* Zlepšení stability.

Starší verze najdete v tématu hello [dokončení poznámky k verzi](mobile-engagement-windows-phone-release-notes.md)

## <a name="upgrade-procedures"></a>Postupy upgradu
Pokud již máte integrovanou starší verze naše sady SDK do své aplikace, musíte tooconsider hello následující body při upgradu hello SDK.

Toofollow může mít několik postupů, pokud provedena několik verzí hello SDK. V tématu hello dokončení [postupy upgradu](mobile-engagement-windows-phone-upgrade-procedure.md). Například pokud migrujete z 0.10.1 too0.11.0 máte toofirst postupujte podle hello "z 0.9.0 too0.10.1" postup pak hello "z 0.10.1 too0.11.0" postup.

### <a name="from-200-too330"></a>Z 2.0.0 too3.3.0
#### <a name="test-logs"></a>Protokolů testování
Protokoly konzoly vyprodukované hello SDK teď může být povolena nebo zakázána nebo filtrovat. toocustomize se aktualizovat hello vlastnost `EngagementAgent.Instance.TestLogEnabled` tooone hello hodnota dostupná z hello `EngagementTestLogLevel` výčtu, například:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Upgrade ze starších verzí
V tématu [Upgrade procedury](mobile-engagement-windows-phone-upgrade-procedure.md)

