---
title: aaaAzure Mobile Engagement Windows Universal SDK integrace | Microsoft Docs
description: Windows Universal integraci sady SDK pro Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 9ded187d-5c07-4377-a41c-ce205dd38b50
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2016
ms.author: piyushjo
ms.openlocfilehash: 2f88e58adb349a2a4eb43b0f182f99b3e8b8cfd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Integrace Windows Universal SDK pro Azure Mobile Engagement
Tento dokument popisuje hello integrace a konfigurace k dispozici všechny možnosti pro hello Azure Mobile Engagement Windows Universal SDK.

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu, musíte nejdřív dokončit naše [15 minut kurzu](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="advanced-features"></a>Pokročilé funkce
### <a name="reporting-features"></a>Funkce vytváření sestav
Můžete přidat tyto funkce:

1. [Rozšířené možnosti vytváření sestav](mobile-engagement-windows-store-advanced-reporting.md)
2. [Upřesnit možnosti konfigurace](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Oznámení
[Jak toointegrate Reach (oznámení) v aplikaci univerzální pro Windows](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Implementace plánu značky:
[Jak toouse hello advanced označování rozhraní API v aplikaci Windows Universal Mobile Engagement](mobile-engagement-windows-store-use-engagement-api.md)

## <a name="release-notes"></a>Poznámky k verzi
### <a name="341-11032016"></a>3.4.1 (11/03/2016)

* Zlepšení stability.

Starší verze najdete v tématu hello [dokončení poznámky k verzi](mobile-engagement-windows-store-release-notes.md)

## <a name="upgrade-procedures"></a>Postupy upgradu
Pokud již mít integrovanou starší verze zapojení do své aplikace, musíte tooconsider hello následující body při upgradu hello SDK.

Pokud je vynechán několik verzí hello SDK, mohou obsahovat toofollow několika postupů. V tématu hello dokončení [postupy upgradu](mobile-engagement-windows-store-upgrade-procedure.md). Například pokud migrujete z 0.10.1 too0.11.0 máte toofirst postupujte podle hello "z 0.9.0 too0.10.1" postup pak hello "z 0.10.1 too0.11.0" postup.

### <a name="from-330-too340"></a>Z 3.3.0 too3.4.0
#### <a name="test-logs"></a>Protokolů testování
Protokoly konzoly vyprodukované hello SDK teď může být povolena nebo zakázána nebo filtrovat. toocustomize, vlastnost hello aktualizace `EngagementAgent.Instance.TestLogEnabled` tooone hello hodnot, které jsou k dispozici z hello `EngagementTestLogLevel` výčtu, například:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

#### <a name="resources"></a>Zdroje
bylo vylepšeno Hello Reach překrytí. Je součástí prostředky pro balíček NuGet sady SDK hello.

Při upgradu toohello novou verzi hello SDK, můžete zvolit, že jestli se mají tookeep existující soubory z hello překrytí složku vašich prostředků, nebo není:

* Pokud předchozí překrytí hello pracuje pro vás, nebo jsou integrace hello `WebView` elementy ručně, pak můžete rozhodnout, tookeep vaše ukončení soubory, budou i nadále fungovat.
* tooupdate toohello nové překrytí, nahraďte hello celou `overlay` složky z vašich prostředků s novým hello z balíčku SDK hello (aplikace UWP: Po provedení upgradu hello můžete získat novou složku překrytí hello % USERPROFILE %\\.nuget\packages\ MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [!WARNING]
> Pomocí nové překrytí hello přepíše všechny úpravy probíhají hello předchozí verze.
> 
> 

### <a name="upgrade-from-older-versions"></a>Upgrade ze starších verzí
V tématu [Upgrade procedury](mobile-engagement-windows-store-upgrade-procedure.md)

