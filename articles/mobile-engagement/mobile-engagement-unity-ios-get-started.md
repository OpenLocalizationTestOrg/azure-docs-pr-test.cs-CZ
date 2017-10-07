---
title: "aaaGet Začínáme s Azure Mobile Engagementem pro nasazení Unity iOS"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace Unity nasazované tooiOS zařízení."
services: mobile-engagement
documentationcenter: unity
author: piyushjo
manager: erikre
editor: 
ms.assetid: 7ddfbac3-8d13-4ebe-b061-c865f357297f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-unity-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: f4247b0a9240cbe2bf1a6618388919d3554c07fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a>Začínáme s Azure Mobile Engagementem pro nasazení aplikací Unity v systému iOS
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse používání aplikace a jak toosend nabízená oznámení toosegmented uživatelům aplikace Unity při nasazení tooan zařízení iOS.
Tento kurz používá hello classic vrátit Unity míč kurzu jako výchozí bod hello. Postupujte podle kroků hello v tomto [kurzu](mobile-engagement-unity-roll-a-ball.md) před pokračováním hello integraci Mobile Engagementu Představujeme v kurzu hello níže. 

Tento kurz vyžaduje hello následující:

* [Unity Editor](http://unity3d.com/get-unity)
* [Mobile Engagement Unity SDK](https://aka.ms/azmeunitysdk)
* XCode Editor

> [!NOTE]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).
> 
> 

## <a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro iOS
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end
### <a name="import-hello-unity-package"></a>Import balíčku Unity hello
1. Stáhnout hello [balíček Mobile Engagement Unity](https://aka.ms/azmeunitysdk) a uložte ho tooyour místního počítače. 
2. Přejděte příliš**prostředků -> Import balíčku -> vlastní balíček** a vyberte hello balíčku, které jste si stáhli v hello výše krok. 
   
    ![][70] 
3. Zkontrolujte, zda jsou vybrány všechny soubory, a klikněte na tlačítko **Import**. 
   
    ![][71] 
4. Po úspěšném importu, zobrazí se soubory SDK hello importovat ve vašem projektu.  
   
    ![][72] 

### <a name="update-hello-engagementconfiguration"></a>Hello aktualizace EngagementConfiguration
1. Otevřete hello **EngagementConfiguration** soubor skriptu hello SDK složku a aktualizace hello **IOS\_připojení\_řetězec** s jste dříve získali hello připojovací řetězec z hello portálu Azure.  
   
    ![][73]
2. Uložte soubor hello. 

### <a name="configure-hello-app-for-basic-tracking"></a>Konfigurace aplikace hello pro základní sledování
1. Otevřete hello **PlayerController** přidružen skript toohello objektu Player pro úpravy. 
2. Přidat hello následující příkaz using:
   
        using Microsoft.Azure.Engagement.Unity;
3. Přidejte následující toohello hello `Start()` – metoda
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### <a name="deploy-and-run-hello-app"></a>Nasazení a spuštění aplikace hello
1. Připojte počítač tooyour zařízení s iOS. 
2. Klikněte na položky **File (Soubor) -> Build Settings (Nastavení sestavení)** 
   
    ![][40]
3. Vyberte **iOS** a potom klikněte na **Switch Platform** (Přepnout platformu)
   
    ![][41]
   
    ![][42]
4. Klikněte na **Player settings** (Nastavení hráče) a zadejte platný identifikátor svazku. 
   
    ![][53]
5. Nakonec klikněte na **Build And Run** (Sestavit a spustit).
   
    ![][54]
6. Může být výzva tooprovide balíček iOS hello toostore název složky. 
   
    ![][43]
7. Pokud všechno půjde dobře, projekt hello se zkompiluje a jste měli ho otevřít v aplikaci XCode. 
8. Ujistěte se, že hello **identifikátor svazku** správné v projektu hello.  
   
    ![][75]
9. Teď spusťte hello aplikaci v XCode, tak, aby balíček hello je nasazené tooyour připojených zařízení a v telefonu by měla zobrazit vaše hra Unity! 

## <a id="monitor"></a>Připojení aplikace se sledováním v reálném čase
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci
Mobile Engagement umožňuje toointeract s uživateli a REACH s nabízená oznámení a zasílání zpráv v kontextu hello kampaní v aplikaci. Tento modul je hello portálu Mobile Engagement nazývá REACH.
Nemáte toodo žádné další konfigurace v aplikaci tooreceive oznámení a je již nastavena pro ni.

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png
