---
title: "ukázkovou aplikaci Mobile Engagement aaaAzure | Microsoft Docs"
description: "Popisuje, kde toodownload, jak toouse a hello výhody používání Azure Mobile Engagement ukázku aplikace"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: f624d5aa-254b-4ad0-96a3-f00e6c3a2c97
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2016
ms.author: piyushjo
ms.openlocfilehash: 9ff0df0d21e1bad6aff573db49304a55593df1c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-demo-app"></a>Azure Mobile Engagement ukázkovou aplikaci
Publikování ukázkové aplikace Azure Mobile Engagement pro **iOS**, **Android**, a **Windows** toohelp platformy je užitečné zdroje toofind a další informace o mobilních Zapojení.

aplikace Hello vám pomůže:

* Snadno najít užitečné odkazy tooMobile Engagement prostředkům, jako jsou videa, dokumentace, hello fórum podpory, a kde toogo tooraise funkce požadavky.
* Zaznamenat ukázka oznámení, které podporuje nápady tooget Mobile Engagementu pro mobilní aplikace.
* Jak používat odkaz na implementaci toostudy tooimplement Mobile Engagementu do vlastní aplikace. Informace najdete na:
  
  * Shromažďování dat analytics.
  * Implementace pokročilé scénáře oznámení typů jako *přes celou obrazovku vkládaná* nebo *automaticky otevírané okno*.
  * Implementovat průzkumy a hlasování.
  * Implementujte scénáře tichou nabízené dat a posílejte nabízená oznámení.   

## <a name="app-installation"></a>Instalace aplikace
Tato aplikace je k dispozici v hello následující obchody s aplikacemi:

* **Univerzální pro Windows ukázkovou aplikaci**:
  
  * Stáhněte si aplikaci hello v hello [aplikace pro Windows store](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).
  * aplikace Hello byl vyvinut jako Windows 10 univerzální aplikace. Hello zdrojový kód je k dispozici na [Githubu](https://github.com/Azure/azure-mobile-engagement-app-windows).
* **iOS ukázku aplikace**:
  
  * Stáhněte si aplikaci hello v hello [Apple storu](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090).
  * Hello aplikace byla vyvinutá v iOS Swift. Hello zdrojový kód je k dispozici na [Githubu](https://github.com/Azure/azure-mobile-engagement-app-ios).
* **Android ukázkovou aplikaci**:
  
  * Stáhněte si aplikaci hello v hello [obchodu Google Play](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement).
  * Hello zdrojový kód je k dispozici na [Githubu](https://github.com/Azure/azure-mobile-engagement-app-android).

![Univerzální pro Windows ukázkovou aplikaci][1]

![Ukázka aplikace iOS][2]
![Android ukázkovou aplikaci][3]

## <a name="usage"></a>Využití
Tuto aplikaci můžete použít v hello následující způsoby:

**Stáhněte hello aplikace na zařízení z odkazů úložiště aplikace hello (uvedených výše):**

> [!IMPORTANT]
> Nepotřebujete účet Azure nebo potřebovat tooconnect hello aplikace tooa back-end. aplikace Hello funguje nezávisle.
> 
> 

* Když máte hello aplikaci na vašem zařízení, potom můžete přejít prostřednictvím hello odkazů v hello nabídky na levé straně toofind hello užitečné zdroje informací o Mobile Engagement.
* Přidali jsme hello [informačního kanálu RSS služby](https://aka.ms/azmerssfeed) do této aplikace, aby se vždy aktualizovat o hello nejnovější aktualizace produktu.
* Můžete také projít hello ukázkové scénáře tooexperience hello typ oznámení oznámení, které jsou podporovány Mobile Engagementu pro každou platformu. Tato oznámení mohou být zkušeného místně – to znamená, můžete kliknout na tlačítka hello na hello obrazovky tooshow hello prostředí oznámení, která je stejná toosending hello oznámení z platformy Mobile Engagement hello.

![Nabídky aplikace pro Windows][4]

![Nabídky aplikace pro iOS][5]
![nabídky aplikace pro Android][6]

**Stáhněte hello zdrojového kódu z hello Githubu odkazů (uvedených výše):**

* Po stažení hello zdrojový kód, otevře se v prostředí pro příslušné vývoj hello – XCode pro iOS, Android Studio pro Android a Visual Studio pro Windows.
* Potom postupujte podle našich [základní kroky pro integraci sady SDK](mobile-engagement-windows-store-dotnet-get-started.md) tak, že budete mít tooconnect tooits tuto aplikaci vlastní instance back-end Mobile Engagement.
  * Je nutné tooconfigure připojovací řetězec v aplikaci hello.
  * Budete také potřebovat tooconfigure hello nabízená oznámení platformy pro vaši aplikaci.
* Můžete si všimnout, že je s Mobile Engagementem instrumentovány v samotné aplikaci hello. Proto při otevření aplikace hello po jejím propojením toohello back-end, budete moct toosee hello uživatelské relace, aktivity, události a tak dále, na hello **monitorování** kartě.
* Také budete moct toosend oznámení toothis aplikace z vlastního instance Mobile Engagement, místo použití místního oznámení.
  
  * Sem můžete přidat zařízení jako testovací zařízení pomocí hello **Get hello ID zařízení** položky nabídky v aplikaci hello. To vám dává ID zařízení, která pak zaregistrovat jako testovací zařízení s vaší instancí back-end platformy.
    
    ![ID zařízení v systému Windows][7]
    
    ![ID zařízení v systému iOS][8]
    ![ID zařízení v systému Android][9]

## <a name="key-features-of-hello-demo-app"></a>Klíčové funkce hello ukázkovou aplikaci
* Jak už bylo zmíněno dříve, s touto aplikací mít ve vaší ruky všechny hello klíče prostředky pro Mobile Engagement. Hello odkazy můžete projít v levé nabídce hello.
* Oznámení se na aplikace pro každou platformu můžete zaznamenat. Tato oznámení mohou být zajišťovány jako **pouze oznámení**, kde kliknutím na oznámení hello jednoduše otevře nativní obrazovky aplikace hello (pomocí **přímé propojení**)--nebo jako **Web oznámení**, kde můžete dodat další obsah HTML z hello end Mobile Engagementu zpět toobe zobrazí po kliknutí na hello oznámení.
  
    ![Oznámení se na aplikace][29]
* V systému iOS máte tooclose hello toosee hello se na aplikace nebo systému nabízená oznámení v aplikaci. Můžete se podívat na implementaci hello zde pro přidání **tlačítka akce**, jako je ta, která jsou přidané toothis oznámení se na aplikace pro hello *zpětné vazby* a *sdílenou složku* (tak, aby Hello uživatel může provádět akce přímo z vlastního hello oznámení).
  
    ![Oznámení se na aplikace v systému iOS][11] ![Zobrazení oznámení se na aplikace v systému iOS][14]
* V systému Android se hello možnosti, které jsou podporovány přidáváte víceřádkový text (**dlouhý Text**) nebo bitovou kopii oznámení (**velký obrázek**) toohello oznámení, společně s hello **akce tlačítka** (jak podporuje iOS).
  
    ![Oznámení se na aplikace v systému Android][12] ![Zobrazení oznámení se na aplikace v systému Android][15]
* Ve Windows 10 uvidíte, jak hello oznámení vypadá na hello PC. Toto oznámení taky se zobrazí v hello Windows 10 **centru oznámení**. Neexistuje žádná podpora pro přidání **tlačítka akce** v hello Windows SDK okamžiku hello.
  
    ![Oznámení se na aplikace v systému Windows][10] ![Zobrazení se na aplikace v systému Windows][13]
* Oznámení "v aplikaci" výchozí pro každou platformu můžete zaznamenat. Toto je prostředí dvoustupňové kde **oznámení** okna je zobrazen jako první. Když kliknete na tlačítko ji, otevře se nahoru na celé obrazovce **oznámení**, jak je uvedeno v následující snímek obrazovky hello. Hello obsah tohoto oznámení pochází z vaší instance back-end Mobile Engagement. Hello SDK má hello šablony pro oznámení a oznámení. Můžete snadno přizpůsobit, jak je znázorněno v této ukázkové aplikaci uveďte hello naše logo a barevné zvýrazňování.  
  
    ![Oznámení v aplikaci v systému Windows][16]
  
    ![Oznámení v aplikaci v iOS][17]  ![Oznámení v aplikaci pro Android][18]
  
    **iOS**, **Android**
* Můžete taky hello **kategorie** funkce Mobile Engagement toocustomize toto výchozí chování. V hello ukázkovou aplikaci jsme jste ukázán dvě běžné způsoby toochange hello činnost hello oznámení. Všimněte si, že v hello Windows SDK ještě není podporovaný této funkce kategorie hello.
  
    **Přes celou obrazovku vkládaná:**
  
    ![Oznámení v aplikaci - vkládaná kategorie][30]
  
    ![Vkládaná kategorie v systému iOS][21]     ![Vkládaná kategorie v systému Android][22]
  
    **Automaticky otevírané okno oznámení:**
  
    ![Oznámení v aplikaci - automaticky otevírané okno kategorie][31]
  
    ![Automaticky otevírané okno oznámení v iOS][19]    ![Automaticky otevírané okno oznámení v systému Android][20]

**iOS**, **Android**

* Mobile Engagement také podporuje speciálním typem oznámení v aplikaci s názvem **hlasování**. To vám umožní toosend rychlé průzkumy tooyour segmentované aplikace uživatelů. Můžete přidat otázky a možnosti pro každou otázku jako hello následující snímek obrazovky. To pak nezobrazí jako uživatel aplikace toohello oznámení v aplikaci.   
  
    ![Dotazování oznámení][32]
  
    ![Průzkum v systému Windows][26]
  
    ![Nástroj na navrhování průzkumů v systému iOS][27]   ![Průzkum v systému Android][28]

**iOS**, **Android**

* Mobile Engagement také podporuje odesílání tichou **Data Push** oznámení. Pomocí těchto oznámení může odesílat data ze služby (např. hello data JSON v hello následující příklad), které lze zpracovat ve vaší aplikaci a určitou akci. Příkladem je, jak jsme se mění hello ceny položky selektivně pomocí dat nabízená oznámení.
  
    ![Data nabízená oznámení][33]
  
    ![Data nabízených oznámení v systému Windows][23]
  
    ![Data nabízených oznámení v iOS][24]  ![Data nabízených oznámení v systému Android][25]

**iOS**, **Android**

> [!NOTE]
> Podrobné pokyny pro některý z těchto oznámení můžete zobrazit kliknutím **klepněte sem a pokyny, jak toosend tato oznámení z platformy Mobile Engagement** na jakékoli obrazovce ukázka oznámení.
> 
> 

[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png
