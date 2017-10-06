---
title: aaaAndroid integraci sady SDK pro Azure Mobile Engagement
description: Popisuje, jak toointegrate Azure Mobile Engagement SDK do aplikace pro Android
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a91ed04f-f3ce-4692-a6dd-b56a28d7dee8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo;ricksal
ms.openlocfilehash: 0c63bfaf673abbda7ea498390f8282c43e2fb8df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="android-sdk-integration-for-azure-mobile-engagement"></a>Integraci sady Android SDK pro Azure Mobile Engagement
> [!div class="op_single_selector"]
> * [Univerzální platforma Windows](mobile-engagement-windows-store-sdk-overview.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-sdk-overview.md)
> * [iOS](mobile-engagement-ios-sdk-overview.md)
> * [Android](mobile-engagement-android-sdk-overview.md)
> 
> 

Tento dokument popisuje hello integrace a konfigurace k dispozici všechny možnosti pro Azure Mobile Engagement Android SDK.

## <a name="prerequisites"></a>Požadavky
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="advanced-features"></a>Pokročilé funkce
### <a name="reporting-features"></a>Funkce vytváření sestav
Můžete přidat tyto funkce:

1. [Rozšířené možnosti vytváření sestav](mobile-engagement-android-advanced-reporting.md)
2. [Možnosti zasílání sestav umístění](mobile-engagement-android-location-reporting.md)
3. [Rozšířené možnosti konfigurace](mobile-engagement-android-advanced-configuration.md)

### <a name="notifications"></a>Oznámení:
[Jak toointegrate Reach (oznámení) v aplikacích pro Android](mobile-engagement-android-integrate-engagement-reach.md)

1. Google Cloud Messaging (GCM): [jak tooIntegrate GCM s Mobile Engagement](mobile-engagement-android-gcm-integrate.md)
2. Zasílání zpráv zařízení Amazon (ADM): [jak tooIntegrate ADM s Mobile Engagement](mobile-engagement-android-adm-integrate.md)

### <a name="tag-plan-implementation"></a>Implementace plánu značky:
[Jak toouse hello advanced Mobile Engagement označování rozhraní API v aplikacích pro Android](mobile-engagement-android-use-engagement-api.md)

## <a name="release-notes"></a>Poznámky k verzi

### <a name="431-07172017"></a>4.3.1 (07/17/2017)
* Opravte havárii, která se může občas nastat při volání metody `EngagementAgentUtils.isInDedicatedEngagementProcess`, které používá také hello `EngagementApplication` třídy.

### <a name="430-06272017"></a>4.3.0 (06/27/2017)
* 8 podpora pro Android (předchozí verze hello SDK nebude fungovat v systému Android 8).
* Žádné další závislosti na podporu knihovny.
* Odebrat `EngagementFragmentActivity` třídy.
* Kvůli příliš[omezení provádění pozadí](https://developer.android.com/preview/features/background.html) na Android 8 může zpoždění protokoly v pozadí, dokud hello uživatel pracuje s hello zařízení, to bude mít dopad na kampaň nabízených **doručené** a **Systémové oznámení zobrazené** statistiky je zpožděno. Pokud byl hello zařízení v režimu spánku (hello oznámení bude i nadále zobrazovat, bude prstence a zavibrovat v reálném čase bez problémů).
* Kvůli příliš[omezení umístění pozadí](https://developer.android.com/preview/features/background-location-limits.html), hello reálném čase umístění pozadí se nebude často aktualizovat na Android 8.

Všechny verze najdete v tématu hello [dokončení poznámky k verzi](mobile-engagement-android-release-notes.md).

## <a name="upgrade-procedures"></a>Postupy upgradu
Pokud již máte integrovanou starší verze naše sady SDK do své aplikace, projděte si [postupy upgradu](mobile-engagement-android-upgrade-procedure.md).

