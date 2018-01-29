---
title: "Začínáme s Azure Mobile Engagementem pro nasazení Unity Android"
description: "Naučte se používat Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace Unity nasazované v zařízeních iOS."
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
ms.openlocfilehash: bf0b758159d475b4ed7eadb84227e4824e11ba86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Začínáme s Azure Mobile Engagementem pro nasazení Unity Android
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

V tomto tématu si ukážeme, jak používat Azure Mobile Engagement, jak porozumět používání aplikací a odesílat nabízená oznámení segmentovaným uživatelům aplikace Unity při nasazení v zařízení Android.
Tento kurz používá jako výchozí bod klasický kurz pro Unity, hru Roll a Ball. Před pokračováním v integraci Mobile Engagementu, kterou představujeme v kurzu níže, postupujte podle kroků v tomto [kurzu](mobile-engagement-unity-roll-a-ball.md). 

V tomto kurzu budete potřebovat následující:

* [Unity Editor](http://unity3d.com/get-unity)
* [Mobile Engagement Unity SDK](https://aka.ms/azmeunitysdk)
* Google Android SDK

> [!NOTE]
> K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).
> 
> 

## <a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro Android
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Připojení aplikace k back-endu Mobile Engagementu
### <a name="import-the-unity-package"></a>Import balíčku Unity
1. Stáhněte si [balíček Mobile Engagement Unity](https://aka.ms/azmeunitysdk) a uložte jej do místního počítače. 
2. Přejděte na stránku **Assets (Prostředky) -> Import Package (Importovat balíček) -> Custom Package (Vlastní balíček)** a vyberte balíček, který jste stáhli v předchozím kroku. 
   
    ![][70] 
3. Zkontrolujte, zda jsou vybrány všechny soubory, a klikněte na tlačítko **Import**. 
   
    ![][71] 
4. Po úspěšném importu se importované soubory SDK zobrazí v projektu.  
   
    ![][72] 

### <a name="update-the-engagementconfiguration"></a>Aktualizace EngagementConfiguration
1. Ve složce SDK otevřete soubor skriptu **EngagementConfiguration** a aktualizujte **ANDROID\_CONNECTION\_STRING** připojovacím řetězcem, který jste dříve získali z portálu Azure.  
   
    ![][73]
2. Soubor uložte 
3. Spusťte **File (Soubor) -> Engagement -> Generate Android Manifest (Generovat manifest Android)**. Jedná se o plugin přidaný sadou Mobile Engagement SDK, který po kliknutí automaticky aktualizuje nastavení projektu. 
   
    ![][74]

> [!IMPORTANT]
> Tento příkaz spusťte při každé aktualizaci souboru **EngagementConfiguration**, jinak se změny v aplikaci neprojeví. 
> 
> 

### <a name="configure-the-app-for-basic-tracking"></a>Konfigurace aplikace pro základní sledování
1. Otevřete skript **PlayerController** připojený k objektu Player pro úpravy. 
2. Přidejte následující příkaz using:
   
        using Microsoft.Azure.Engagement.Unity;
3. Do metody `Start()` přidejte následující
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### <a name="deploy-and-run-the-app"></a>Nasazení a spuštění aplikace
Před nasazením aplikace Unity do zařízení zkontrolujte, zda je v počítači nainstalován balíček Android SDK. 

1. Připojte zařízení Android k vašemu počítači. 
2. Klikněte na položky **File (Soubor) -> Build Settings (Nastavení sestavení)** 
   
    ![][40]
3. Vyberte **Android** a potom klikněte na **Switch Platform** (Přepnout platformu).
   
    ![][51]
   
    ![][52]
4. Klikněte na **Player settings** (Nastavení hráče) a zadejte platný identifikátor svazku. 
   
    ![][53]
5. Nakonec klikněte na **Build And Run** (Sestavit a spustit).
   
    ![][54]
6. Můžete být vyzváni k zadání názvu složky, do které se uloží balíček Android. 
7. Pokud všechno půjde dobře, balíček se nasadí v připojeném zařízení a v telefonu by se měla zobrazit vaše hra Unity. 

## <a id="monitor"></a>Připojení aplikace se sledováním v reálném čase
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci
[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

### <a name="update-the-engagementconfiguration"></a>Aktualizace EngagementConfiguration
1. Ve složce SDK otevřete soubor skriptu **EngagementConfiguration** a aktualizujte **ANDROID\_GOOGLE\_NUMBER** **číslem projektu Google**, které jste dříve získali z portálu Google Cloud Developer. Jedná se o řetězcovou hodnotu, takže ji nezapomeňte zavřít do uvozovek. 
   
    ![][75]
2. Uložte soubor. 
3. Spusťte **File (Soubor) -> Engagement -> Generate Android Manifest (Generovat manifest Android)**. Jedná se o plugin přidaný sadou Mobile Engagement SDK, který po kliknutí automaticky aktualizuje nastavení projektu. 
   
    ![][74]

### <a name="configure-the-app-to-receive-notifications"></a>Konfigurace aplikace k přijímání oznámení
1. Otevřete skript **PlayerController** připojený k objektu Player pro úpravy. 
2. Do metody `Start()` přidejte následující
   
        EngagementReachAgent.Initialize();
3. Když je nyní aplikace aktualizovaná, nasaďte ji a spusťte v zařízení podle níže uvedených pokynů. 

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
