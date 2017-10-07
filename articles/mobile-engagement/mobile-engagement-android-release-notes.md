---
title: aaaAzure integraci sady Android SDK Mobile Engagement
description: "Nejnovější aktualizace a postupy pro Android SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 585341f9-3f39-459a-af42-864e400a0128
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 16b098198674c49567d720d0c01d984cb763ed8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes"></a>Poznámky k verzi

## <a name="431-07172017"></a>4.3.1 (07/17/2017)
* Opravte havárii, která se může občas nastat při volání metody `EngagementAgentUtils.isInDedicatedEngagementProcess`, které používá také hello `EngagementApplication` třídy.

## <a name="430-06272017"></a>4.3.0 (06/27/2017)
* 8 podpora pro Android (předchozí verze hello SDK nebude fungovat v systému Android 8).
* Žádné další závislosti na podporu knihovny.
* Odebrat `EngagementFragmentActivity` třídy.
* Kvůli příliš[omezení provádění pozadí](https://developer.android.com/preview/features/background.html) na Android 8 může zpoždění protokoly v pozadí, dokud hello uživatel pracuje s hello zařízení, to bude mít dopad na kampaň nabízených **doručené** a **Systémové oznámení zobrazené** statistiky je zpožděno. Pokud byl hello zařízení v režimu spánku (hello oznámení bude i nadále zobrazovat, bude prstence a zavibrovat v reálném čase bez problémů).
* Kvůli příliš[omezení umístění pozadí](https://developer.android.com/preview/features/background-location-limits.html), hello reálném čase umístění pozadí se nebude často aktualizovat na Android 8.

## <a name="424-03302017"></a>4.2.4 (03/30/2017)
* Opravte barvy textu na Android 7 toobe hello stejné jako starší verze Android oznámení v aplikaci.

## <a name="423-08102016"></a>4.2.3 (08/10/2016)
* Žádné další zámek Wi-Fi.
* Opravte zablokování při volání metody getDeviceId před init (počínaje 4.2.0 chyb).

## <a name="422-05172016"></a>4.2.2 (05/17/2016)
* Zlepšení stability.

## <a name="421-05102016"></a>4.2.1 (05/10/2016)
* Zabezpečení: zakážete webové zobrazení místního souboru přístup.
* Zabezpečení: odeberte `EngagementPreferenceActivity` třídu, která rozšiřuje zastaralé a nezabezpečená `PreferenceActivity` třídy.
* Zabezpečení: reach aktivity jsou nyní zdokumentované toouse `exported="false"`, tento příznak lze také v předchozích verzích sady SDK.

## <a name="420-03112016"></a>4.2.0 (03/11/2016)
* Hello SDK je nyní licencováno v rámci MIT.
* Povolí použití identifikátoru vlastní zařízení během inicializace SDK.

## <a name="415-02012016"></a>4.1.5 (02/01/2016)
* Zlepšení stability.

## <a name="414-01262016"></a>4.1.4 (01/26/2016)
* Zlepšení stability.

## <a name="413-1292015"></a>4.1.3 (12/9/2015)
* Zlepšení stability.

## <a name="412-11252015"></a>4.1.2 (11/25/2015)
* Zlepšení stability.

## <a name="411-11042015"></a>4.1.1 (11/04/2015)
* Zlepšení stability.

## <a name="410-08252015"></a>4.1.0 (08/25/2015)
* Zpracujte nový model oprávnění pro Android M.
* Můžete teď nakonfigurovat funkce umístění za běhu místo použití `AndroidManifest.xml`.
* Oprava chyby oprávnění: Pokud používáte `ACCESS_FINE_LOCATION`, pak `ACCESS_COARSE_LOCATION` už není potřeba.
* Zlepšení stability.

## <a name="400-07062015"></a>4.0.0 (07/06/2015)
* Vnitřní změní toomake analýzy a nabízených spolehlivější.
* Nativního nabízení (GCM/ADM) se teď také používá pro oznámení v aplikaci, musíte nakonfigurovat přihlašovací údaje nativního nabízení hello pro jakýkoli typ nabízené kampaně.
* Opravte velký obrázek oznámení: byly zobrazené pouze 10s po se instaluje.
* Oprava chyby ve webovém zobrazení: Kliknutím na odkaz se také provádí hello výchozí akce adresy URL.
* Opravte výjimečných havárií související toolocal správu úložiště.
* Opravte dynamické konfigurace řetězec správy.
* Aktualizujte smlouvy EULA.

## <a name="300-02172015"></a>3.0.0 (02/17/2015)
* Počáteční verzi Azure Mobile Engagement
* Konfigurace appId je nahrazena konfiguraci řetězec připojení.
* Odebrat toosend rozhraní API a přijímat libovolné protokolu XMPP zprávy z libovolné protokolu XMPP entit.
* Odebrat toosend rozhraní API a přijímat zprávy mezi zařízeními.
* Vylepšení zabezpečení.
* Sledování Google Play a SmartAd odebrat.

