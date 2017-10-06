---
title: "aaaGet začít s Windows Universal aplikace Azure Mobile Engagement"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro univerzální aplikace Windows."
services: mobile-engagement
documentationcenter: windows
author: piyushjo
manager: erikre
editor: 
ms.assetid: 48103867-7f64-4646-b019-42bd797d38e2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 8224a6d3789cfe4784bbc9472005f9eddb94a8b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-universal-apps"></a>Začínáme s Azure Mobile Engagementem pro univerzální aplikace pro Windows
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse využití aplikací a odesílat nabízená oznámení uživatelům toosegmented univerzální aplikace pro Windows.
Tento kurz představuje scénář hello jednoduchého vysílání přes Mobile Engagement. Vytvoříte prázdnou univerzální aplikaci pro Windows, která bude shromažďovat základní data o využití aplikace a přijímat nabízená oznámení pomocí služby oznamování systému Windows (WNS).

> [!NOTE]
> Služba Azure Mobile Engagement Hello vyřadí března 2018 a je aktuálně pouze k dispozici tooexisting zákazníků. Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

## <a name="prerequisites"></a>Požadavky
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="set-up-mobile-engagement-for-your-windows-universal-app"></a>Nastavení Mobile Engagementu pro univerzální aplikaci pro Windows
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end
V tomto kurzu uvede "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení. Hello kompletní dokumentaci k integraci najdete v hello [integrace sady Mobile Engagement Windows Universal SDK](mobile-engagement-windows-store-sdk-overview.md).

Vytvoříte základní aplikaci s integrace hello toodemonstrate sady Visual Studio.

### <a name="create-a-windows-universal-app-project"></a>Vytvoření projektu univerzální aplikace pro Windows
Hello následující postup předpokládá použití hello sady Visual Studio 2015 když hello postup je podobný jako ve starších verzích sady Visual Studio.

1. Spuštění sady Visual Studio a v hello **Domů** obrazovku, vyberte **nový projekt**.
2. V místní nabídce hello vyberte **Windows** -> **Universal** -> **prázdná aplikace (univerzální pro Windows)**. Vyplňte aplikace hello **název** a **název řešení**a potom klikněte na **OK**.

    ![][1]

Nyní jste vytvořili projekt univerzální aplikace pro Windows, do které vedle integrovat hello Azure Mobile Engagement SDK.

### <a name="connect-your-app-toomobile-engagement-backend"></a>Připojit vaše tooMobile Engagement back-end aplikace
1. Nainstalujte hello [MicrosoftAzure.MobileEngagement] balíček Nuget do projektu. Pokud cílíte na platformy Windows i Windows Phone, musíte toodo to pro oba projekty. Pro Windows 8.x a Windows Phone 8.1, hello stejné Nuget balíček místech hello správné specifické pro platformu binárních souborů do každého projektu.
2. Otevřete **Package.appxmanifest** a zajistěte, aby je přidána této hello následující funkce:

        Internet (Client)

    ![][2]
3. Nyní hello připojovací řetězec, který jste zkopírovali dříve pro aplikaci Mobile Engagementu zkopírujte a vložte ji do hello `Resources\EngagementConfiguration.xml` souboru mezi hello `<connectionString>` a `</connectionString>` značky:

    ![][3]

    > [!TIP]
    > Pokud vaše aplikace cílí na platformy Windows i Windows Phone, měli byste stále vytvořit dvě aplikace Mobile Engagementu – jednu pro každou podporovanou platformu. Má dvě aplikace zajišťuje, že jste správně segmentovat cílovou skupinu hello můžete vytvořit a můžete posílat vhodně cílená oznámení pro každou platformu.

    > [!IMPORTANT]
    > NuGet automaticky nekopíruje hello prostředků sady SDK v aplikaci Windows 10 UWP. Jej budete mít toodo ručně následující hello kroky, které ukazují nahoru (readme.txt) při instalaci balíčku Nuget hello.  

1. V hello `App.xaml.cs` souboru:

    a. Přidat hello `using` příkaz:

            using Microsoft.Azure.Engagement;

    b. Přidejte metodu, která inicializuje hello Engagement:

           private void InitEngagement(IActivatedEventArgs e)
           {
             EngagementAgent.Instance.Init(e);

             //... rest of hello code
           }

    c. Inicializace hello SDK v hello **OnLaunched** metoda:

            protected override void OnLaunched(LaunchActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of hello code
            }

    c. Vložte následující hello hello **OnActivated** metoda a přidejte metodu hello, pokud již není přítomna:

            protected override void OnActivated(IActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of hello code
            }

## <a id="monitor"></a>Povolení sledování v reálném čase
toostart odesílat data a zajistit, že hello uživatelé jsou aktivní, musíte odeslat alespoň jednu obrazovku (aktivitu) toohello Mobile Engagement back-end.

1. V hello **MainPage.xaml.cs**, přidejte následující hello `using` příkaz:

    použití příkazu Microsoft.Azure.Engagement.Overlay;
2. Změňte základní třídu hello **MainPage** z **stránky** příliš**EngagementPageOverlay**:

        class MainPage : EngagementPageOverlay
3. V hello `MainPage.xaml` souboru:

    a. Přidejte tooyour deklarací oborů názvů:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

    b. Nahraďte hello **stránky** v názvu značky XML hello s **engagement: EngagementPageOverlay**

> [!IMPORTANT]
> Pokud stránka přepíše hello `OnNavigatedTo` metoda, že toocall být `base.OnNavigatedTo(e)`. Jinak, není hlášena hello aktivity `EngagementPage` volání `StartActivity` uvnitř jeho `OnNavigatedTo` metoda). To je obzvláště důležité v projektu Windows Phone, kde má výchozí šablona hello `OnNavigatedTo` metoda.
>
> Pro **10 univerzálních aplikací pro Windows**, použijte metodu hello doporučení v hello "doporučená metoda: přetížení třídy stránky" části [pokročilé vytváření sestav s hello Windows Universal SDK aplikace Engagement](mobile-engagement-windows-store-advanced-reporting.md) , místo hello jeden uvedených výše.

## <a id="monitor"></a>Připojení aplikace se sledováním v reálném čase
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci
Mobile Engagement vám umožní toointeract oslovit uživatele a komunikovat s nabízená oznámení a zasílání zpráv v kontextu hello kampaní v aplikaci. Tento modul je hello portálu Mobile Engagement nazývá REACH.
Hello následujících sekcích nastavíte tooreceive vaše aplikace je.

### <a name="enable-your-app-tooreceive-wns-push-notifications"></a>Povolit tooreceive nabízených oznámení WNS vaší aplikace
1. V hello `Package.appxmanifest` souboru ve hello **aplikace** v části **oznámení**, nastavte **podporující oznamovací zprávy:** příliš**Ano**

    ![][5]

### <a name="initialize-hello-reach-sdk"></a>Inicializace hello REACH SDK
V `App.xaml.cs`, volání **EngagementReach.Instance.Init(e);** v hello **InitEngagement** funkce hned po inicializaci agenta hello:

        private void InitEngagement(IActivatedEventArgs e)
        {
           EngagementAgent.Instance.Init(e);
           EngagementReach.Instance.Init(e);
        }

Jste připravené toosend oznámení. Dál ověříme, jestli jste základní integraci provedli správně.

### <a name="grant-access-toomobile-engagement-toosend-notifications"></a>Udělení přístupu tooMobile Engagement toosend oznámení
1. Otevřete [Centrum vývojářů pro Windows Store] ve webovém prohlížeči, přihlaste se a v případě potřeby si vytvořte účet.
2. Klikněte na tlačítko **řídicí panel** v hello pravém horním rohu a pak klikněte na **vytvořit novou aplikaci** z nabídky na levém panelu hello.

    ![][9]
3. Vytvoření aplikace rezervací názvu.

    ![][10]
4. Po vytvoření aplikace hello přejděte příliš**služby -> nabízená oznámení** hello levé nabídce.

    ![][11]
5. V hello nabízená oznámení oddíl, klikněte na tlačítko hello **Web služeb Live Services** odkaz.

    ![][12]
6. Přejdete toohello nabízené přihlašovací údaje části. Zkontrolujte, zda jsou v hello **nastavení aplikace** a poté zkopírujte vaše **SID balíčku** a **tajný klíč klienta**

    ![][13]
7. Přejděte toohello **nastavení** z portálu Mobile Engagement a klikněte na tlačítko hello **nativní oznámení** části na levé straně hello. Potom klikněte na hello **upravit** tooenter tlačítko vaše **identifikátor zabezpečení (SID) balíčku** a **tajný klíč** znázorněné:

    ![][6]
8. Nakonec nezapomeňte přiřadit aplikaci Visual Studio k této vytvořené aplikaci v obchodě s aplikacemi hello. V sadě Visual Studio klikněte na **Propojit aplikaci se Storem**.

    ![][7]

## <a id="send"></a>Poslat tooyour aplikaci oznámení
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Pokud hello aplikace běží, zobrazí se oznámení v aplikaci. Pokud aplikace hello je zavřená, v opačném případě zobrazí oznámení s informační zprávou.
Pokud se zobrazí oznámení v aplikaci, ale ne oznámení s informační zprávou a hello aplikace běží v režimu ladění v sadě Visual Studio, opakujte **události životního cyklu -> pozastavit** v panelu nástrojů tooensure hello aplikaci hello pozastaveno. Pokud jste klikli hello tlačítko Domů při ladění aplikace hello v sadě Visual Studio, poté ji není vždy dojít k jejímu pozastavení a při hello oznámení se zobrazí v aplikaci, nezobrazí jako oznámení s informační zprávou.  

![][8]

<!-- URLs. -->
[Mobile Engagement Windows Universal SDK documentation]: ../mobile-engagement-windows-store-integrate-engagement/
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9864592
[Centrum vývojářů pro Windows Store]: https://dev.windows.com
[Windows Universal Apps - Overlay integration]: ../mobile-engagement-windows-store-integrate-engagement-reach/#overlay-integration

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-store-dotnet-get-started/universal-app-creation.png
[2]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-capabilities.png
[3]: ./media/mobile-engagement-windows-store-dotnet-get-started/add-connection-info.png
[5]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-toast.png
[6]: ./media/mobile-engagement-windows-store-dotnet-get-started/enter-credentials.png
[7]: ./media/mobile-engagement-windows-store-dotnet-get-started/associate-app-store.png
[8]: ./media/mobile-engagement-windows-store-dotnet-get-started/vs-suspend.png
[9]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_create_app.png
[10]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_app_name.png
[11]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push.png
[12]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_1.png
[13]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_creds.png
