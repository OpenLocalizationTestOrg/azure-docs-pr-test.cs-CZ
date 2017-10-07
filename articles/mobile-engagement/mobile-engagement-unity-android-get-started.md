---
title: "aaaGet Začínáme s Azure Mobile Engagementem pro nasazení Unity Android"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace Unity nasazované tooiOS zařízení."
services: mobile-engagement
documentationcenter: unity
author: piyushjo
manager: erikre
editor: 
ms.assetid: d5f0ef79-be00-4cec-97a5-a0b2fdaa380e
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-unity-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: c4d34691daeb7544b11c2d6895b2474af0f902b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Začínáme s Azure Mobile Engagementem pro nasazení Unity Android
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse používání aplikace a jak toosend nabízená oznámení toosegmented uživatelům aplikace Unity při nasazení tooan zařízení se systémem Android.
Tento kurz používá hello classic vrátit Unity míč kurzu jako výchozí bod hello. Postupujte podle kroků hello v tomto [kurzu](mobile-engagement-unity-roll-a-ball.md) před pokračováním hello integraci Mobile Engagementu Představujeme v kurzu hello níže. 

Tento kurz vyžaduje hello následující:

* [Unity Editor](http://unity3d.com/get-unity)
* [Mobile Engagement Unity SDK](https://aka.ms/azmeunitysdk)
* Google Android SDK

> [!NOTE]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).
> 
> 

## <a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro Android
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
1. Otevřete hello **EngagementConfiguration** soubor skriptu hello SDK složku a aktualizace hello **ANDROID\_připojení\_řetězec** s jste získali hello připojovací řetězec dříve z hello portálu Azure.  
   
    ![][73]
2. Uložte soubor hello 
3. Spusťte **File (Soubor) -> Engagement -> Generate Android Manifest (Generovat manifest Android)**. Toto je hello plugin přidaný sadou Mobile Engagement SDK hello a kliknutí na tlačítko se automaticky aktualizuje nastavení projektu. 
   
    ![][74]

> [!IMPORTANT]
> Ujistěte se, že tooexecute to při každé aktualizaci hello **EngagementConfiguration** souboru vaše změny se neprojeví v aplikaci hello. 
> 
> 

### <a name="configure-hello-app-for-basic-tracking"></a>Konfigurace aplikace hello pro základní sledování
1. Otevřete hello **PlayerController** přidružen skript toohello objektu Player pro úpravy. 
2. Přidat hello následující příkaz using:
   
        using Microsoft.Azure.Engagement.Unity;
3. Přidejte následující toohello hello `Start()` – metoda
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### <a name="deploy-and-run-hello-app"></a>Nasazení a spuštění aplikace hello
Ujistěte se, že máte nainstalovaný na počítači před pokusem o toodeploy toto zařízení tooyour aplikace Unity Android SDK. 

1. Připojte počítači tooyour zařízení se systémem Android. 
2. Klikněte na položky **File (Soubor) -> Build Settings (Nastavení sestavení)** 
   
    ![][40]
3. Vyberte **Android** a potom klikněte na **Switch Platform** (Přepnout platformu).
   
    ![][51]
   
    ![][52]
4. Klikněte na **Player settings** (Nastavení hráče) a zadejte platný identifikátor svazku. 
   
    ![][53]
5. Nakonec klikněte na **Build And Run** (Sestavit a spustit).
   
    ![][54]
6. Může být výzva tooprovide složky název toostore hello Android balíček. 
7. Pokud všechno půjde dobře, balíček hello bude nasazený tooyour připojené zařízení a které by měla zobrazit vaše hra Unity na váš telefon! 

## <a id="monitor"></a>Připojení aplikace se sledováním v reálném čase
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci
[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

### <a name="update-hello-engagementconfiguration"></a>Hello aktualizace EngagementConfiguration
1. Otevřete hello **EngagementConfiguration** soubor skriptu hello SDK složku a aktualizace hello **ANDROID\_GOOGLE\_číslo** s hello **projektu Google Číslo** jste dříve získali z portálu Google Cloud Developer hello. Toto je řetězec hodnotu, ujistěte se, že tooenclose ji do uvozovek. 
   
    ![][75]
2. Uložte soubor hello. 
3. Spusťte **File (Soubor) -> Engagement -> Generate Android Manifest (Generovat manifest Android)**. Toto je hello plugin přidaný sadou Mobile Engagement SDK hello a kliknutí na tlačítko se automaticky aktualizuje nastavení projektu. 
   
    ![][74]

### <a name="configure-hello-app-tooreceive-notifications"></a>Konfigurace oznámení tooreceive aplikace hello
1. Otevřete hello **PlayerController** přidružen skript toohello objektu Player pro úpravy. 
2. Přidejte následující toohello hello `Start()` – metoda
   
        EngagementReachAgent.Initialize();
3. Teď, když hello aplikace se aktualizuje, nasazení a spuštění aplikace hello na zařízení podle níže uvedených pokynů hello. 

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
