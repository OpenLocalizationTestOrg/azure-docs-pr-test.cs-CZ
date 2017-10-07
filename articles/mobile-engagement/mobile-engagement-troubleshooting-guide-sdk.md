---
title: "aaaAzure Mobile Engagement Průvodce odstraňováním potíží - SDK"
description: "Řešení potíží s problémů s integrací sady SDK v Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: de265cf1-2f88-43ef-8616-156ada5be7b6
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 1c082b81d898f4bdb47b8efe6cfbacfd83fe9279
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>Průvodci odstraňováním potíží problémů s integrací sady SDK
Hello následují možných problémů se můžete setkat s jak Azure Mobile Engagement integruje do vaší aplikace.

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>SDK problémů zjištěných selhání v jiné oblasti vaší aplikace.
### <a name="issue"></a>Problém
* Chyba kolekce dat uživatelského rozhraní (v analýzy, monitorování, segmentace nebo řídicí panely).
* Push selhání (nabízených oznámení nefungují v aplikaci a mimo aplikaci, nebo obojí).
* Pokročilé funkce selhání (sledování, informace o zeměpisné poloze nebo platformy, které nepodporují konkrétní nabízených oznámení).
* Selhání rozhraní API (rozhraní API selhání často bezobslužném režimu bez chybové zprávy).
* Selhání služby (žádná z Azure Mobile Engagement funguje pro vaši aplikaci).

### <a name="causes"></a>Způsobí, že
* Většiny problémů, které je třeba toobe vyřešil hello Azure Mobile Engagement SDK budou zjištěny chybou v aplikaci (například selhání shromažďování dat uživatelského rozhraní, selhání nabízená, chybu upřesňující funkce, selhání rozhraní API, havárie aplikací nebo zřejmá služby výpadek).  
* Pokud konkrétní funkce Azure Mobile Engagement nikdy fungovala ve vaší aplikaci před, budete potřebovat toocomplete hello integrace. 
* Pokud konkrétní funkce Azure Mobile Engagement byla práce a byla zastavena, může být nutné tooupgrade toohello poslední verze s hello Azure Mobile Engagement SDK. Mějte na paměti, že je jinou verzi hello Azure Mobile Engagement SDK pro každou platformu podporovanou nástrojem Azure Mobile Engagement (Android, iOS, Windows a Windows Phone).

#### <a name="sdk-integration"></a>Integrace sady SDK
* Azure Mobile Engagement integrované není správně v sadě SDK (Analytics).
* Dosažení integrované není správně v sadě SDK (v aplikaci a mimo nabízených oznámení aplikace).
* Certificate vs PRODUKČNÍMU vypršela platnost nebo není správný. DEV (jenom iOS).
* GCM nebo ADM integrované není správně v sadě SDK (Android jenom - Service konkrétní nabízených oznámení).
* Sledování není správně integrované v sadě SDK (sledování instalace úložiště).
* Opožděné umístění nebo umístění GPS integrované není správně v sadě SDK (cílení na podle geografického umístění).

**Viz také:**

* [Dokumentaci k sadě SDK – integrace příručky][Link 5] 
* [Průvodce odstraňováním potíží s - Push][Link 23]

#### <a name="sdk-upgrade"></a>Upgrade sady SDK
* Třeba tooupgrade SDK tooresolve problémy se staršími verzemi hello SDK (často související toonewer verze operačního systému zařízení hello).
* Odinstalujte všechny předchozí verze aplikace ze zařízení a znovu nainstalujte nejnovější verzi aplikace hello, hello znovu zaregistrovat ID zařízení z tooconfirm hello Azure Mobile Engagement uživatelského rozhraní, že vaše zařízení používá hello nejnovější verzi aplikace.

**Viz také:**

* [Dokumentaci k sadě SDK – poznámky k verzi](http://go.microsoft.com/fwlink/?LinkId= 525554) 
* [Dokumentaci k sadě SDK - upgradu příručky](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>SDK jiných
* Chyby v Application Manifest "AndroidManifest.xml" může způsobit, že Azure Mobile Engagement toowork (jen Android).
* Běžné problémy s integraci sady SDK a využití rozhraní API je tooconfuse hello klíč SDK a hello klíč rozhraní API.

**Viz také:**

* [Principy – Glosář][Link 6]

## <a name="advanced-coding-issues"></a>Pokročilé kódování problémy
### <a name="issue"></a>Problém
* Kód konkrétní platformy není přímo souvisí s tooAzure Mobile Engagement v iOS, Android a Windows Phone může způsobit problémy.

### <a name="causes"></a>Způsobí, že
* Několik upřesňujících kódování problémy s Azure Mobile Engagement se nezdařila z důvodu nesprávně napsaný platformou konkrétního kódu není přímo souvisí s tooAzure Mobile Engagement. Budete potřebovat tooconsult dokumentace konkrétní toohello platformy, které vyvíjíte pro kromě tooAzure Mobile Engagement dokumentace (Android, iOS, Web, systém Windows a Windows Phone).
* Konfigurace není správně "kategorie", zabraňuje propojení z umístění tooanother oznámení uvnitř nebo vně aplikace hello (jen Android). 
* Není nastavení "UIKit.framework" příliš "volitelné" ve vašem kódu iOS zobrazí "Symbol nebyla nalezena chyba" nebo dojde k chybě na zařízeních s iOS starší (jenom iOS).
* Platnost certifikátů nebo pomocí není správně hello DEV nebo produkčnímu verze hello cert, nabízené příčiny problémů (jenom iOS).
* Existují některá omezení vyplývajících tooa platforma, která Azure Mobile Engagement nemůže řídit (jako jsou jak funguje hello system center pro mimo aplikaci nabízených oznámení v Android a iOS).
* Azure Mobile Engagement publikuje úplný seznam hello interní balíčky používané Azure Mobile Engagementem pro iOS a Android pro referenci. Mějte na paměti, že některé funkce Azure Mobile Engagement jsou konkrétní toohello platformy (Android, iOS, Web, systém Windows a Windows Phone).

### <a name="see-also"></a>Viz také
* [Průvodce odstraňováním potíží s - Push][Link 23] 
* [Dokumentaci k sadě SDK – poznámky k verzi][Link 5]
* [Dokumentaci k sadě SDK - upgradu příručky][Link 5]

## <a name="application-crashes"></a>Selhání aplikace
### <a name="issue"></a>Problém
* Aplikace na zařízení koncoví uživatelé hello dojde k chybě.

### <a name="causes"></a>Způsobí, že
* Informace o havárii lze zobrazit v hello *uživatelského rozhraní Analytics* nebo hello *Analytics rozhraní API*
* Můžete najít hello zařízení ID testovací zařízení a proveďte hello stejnou akci, která způsobila, že vaše aplikace toocrash pro toohelp koncový uživatel určit příčinu hello vaší havárie.
* Známé problémy s hello Azure Mobile Engagement SDK, které způsobí toocrash aplikací jsou někdy vyřešit upgradem toohello nejnovější verzi hello SDK. Ujistěte se, že toocheck hello poznámky o vaši platformu při zkoumání dojde k chybě.

### <a name="see-also"></a>Viz také
* [Dokumentaci k sadě SDK – poznámky k verzi][Link 5]
* [Dokumentaci k sadě SDK - upgradu příručky][Link 5]

## <a name="app-store-upload-failures"></a>Chyb odesílání App store
### <a name="issue"></a>Problém
* Chyby související s toouploading hello nejnovější verzi vaší aplikace tooApple, hello aplikace pro Windows store nebo Google.

### <a name="causes"></a>Způsobí, že
* Aplikace ukládá někdy blokovat aplikacím s určité funkce povolit (například hello Apple Store brání použití hello IDFV v aplikacích v úložišti hello a úložiště GooglePlay hello brání hello sdílení informací o aplikaci mezi aplikacemi). 
* Zajistěte, aby zkontrolujte poznámky k verzi hello o platformy a aktuální verzi hello SDK, pokud máte potíže při odesílání toohello obchod s aplikacemi.

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

