---
title: "aaaGet začít s Azure Mobile Engagement pro Windows Phone aplikace Silverlight"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace pro Windows Phone Silverlight."
services: mobile-engagement
documentationcenter: windows
author: piyushjo
manager: erikre
editor: 
ms.assetid: aa34692f-87f7-47c6-a20c-a1972750bc25
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: b39a838ab03217b2dc845cbf59d7bf8b094dac1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a>Začínáme s Azure Mobile Engagementem pro aplikace Silverlight pro Windows Phone
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse využití aplikací a odesílat nabízená oznámení uživatelům toosegmented aplikace pro Windows Phone Silverlight.
Tento kurz představuje scénář hello jednoduchého vysílání přes Mobile Engagement. Vytvoříte prázdnou aplikaci Silverlight pro Windows Phone, která bude shromažďovat základní data a přijímat nabízená oznámení pomocí Microsoft Služby nabízených oznámení (MPNS).

> [!NOTE]
> Služba Azure Mobile Engagement Hello vyřadí března 2018 a je aktuálně pouze k dispozici tooexisting zákazníků. Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

> [!NOTE]
> Projekty pro Windows Phone 8.1 a starší verze nejsou podporovány v sadě Visual Studio 2017.  Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

> [!NOTE]
> Pokud cílíte na Windows Phone 8.1 (bez Silverlight), přečtěte si téma toohello [kurz pro univerzální aplikace Windows](mobile-engagement-windows-store-dotnet-get-started.md).
> 
> 

Tento kurz vyžaduje hello následující:

* Visual Studio 2013
* Balíček NuGet [MicrosoftAzure.MobileEngagement]

> [!NOTE]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).
> 
> 

## <a id="setup-azme"></a>Nastavení Mobile Engagementu pro aplikaci pro Windows Phone
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end
Tento kurzu si představíme "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení. Hello kompletní dokumentaci k integraci najdete v hello [integrace sady Mobile Engagement Windows Phone SDK](mobile-engagement-windows-phone-sdk-overview.md)

Pomocí integrace sady Visual Studio toodemonstrate hello vytvoříme základní aplikaci.

### <a name="create-a-new-windows-phone-silverlight-project"></a>Vytvoření nového projektu Windows Phone Silverlight
Hello následující postup předpokládá použití hello sady Visual Studio 2015 když hello postup je podobný jako ve starších verzích sady Visual Studio. 

1. Spuštění sady Visual Studio a v hello **Domů** obrazovku, vyberte **nový projekt**.
2. V místní nabídce hello vyberte **Windows 8** -> **Windows Phone** -> **prázdná aplikace (Windows Phone Silverlight)**. Vyplňte aplikace hello **název** a **název řešení**a potom klikněte na **OK**.
   
    ![][1]
3. Buď můžete tootarget **Windows Phone 8.0** nebo **Windows Phone 8.1**.

Nyní jste vytvořili novou aplikaci Silverlight pro Windows Phone, do které budeme integrovat hello Azure Mobile Engagement SDK.

### <a name="connect-your-app-toohello-mobile-engagement-backend"></a>Připojit vaše aplikace toohello Mobile Engagement back-end
1. Nainstalujte hello [MicrosoftAzure.MobileEngagement] balíček nuget do projektu.
2. Otevřete `WMAppManifest.xml` (ve složce vlastnosti hello) a zajistěte, aby jsou deklarovány následující hello (přidejte je, pokud nejsou) v hello `<Capabilities />` značky:
   
        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />
   
    ![][2]
3. Nyní vložte hello připojovací řetězec, který jste zkopírovali dříve pro aplikaci Mobile Engagement a vložte ji do hello `Resources\EngagementConfiguration.xml` souboru mezi hello `<connectionString>` a `</connectionString>` značky:
   
    ![][3]
4. V hello `App.xaml.cs` souboru:
   
    a. Přidat hello `using` příkaz:
   
            using Microsoft.Azure.Engagement;
   
    b. Inicializace hello SDK v hello `Application_Launching` metoda:
   
            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }
   
    c. Vložte následující hello hello `Application_Activated`:
   
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

## <a id="monitor"></a>Povolení sledování v reálném čase
V pořadí toostart odesílat data a zajistit, že hello uživatelé jsou aktivní musíte odeslat alespoň jednu obrazovku (aktivitu) toohello Mobile Engagement back-end.

1. Hello MainPage.xaml.cs přidejte hello `using` příkaz:
   
        using Microsoft.Azure.Engagement;
2. Nahraďte základní třídu hello **MainPage**, která je před **PhoneApplicationPage**, s **EngagementPage**.
   
        class MainPage : EngagementPage 
3. V souboru `MainPage.xml`:
   
    a. Přidejte tooyour deklarací oborů názvů:
   
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
   
    b. Nahraďte `phone:PhoneApplicationPage` v názvu značky XML hello s `engagement:EngagementPage`.

## <a id="monitor"></a>Připojení aplikace se sledováním v reálném čase
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci
Mobile Engagement vám umožní toointeract oslovit uživatele a pomocí nabízených oznámení a zasílání zpráv v aplikaci v kontextu hello kampaní. Tento modul je hello portálu Mobile Engagement nazývá REACH.
Hello následujících sekcích nastavíte tooreceive vaše aplikace je.

### <a name="enable-your-app-tooreceive-mpns-push-notifications"></a>Povolit tooreceive nabízených oznámení MPNS vaší aplikace
Přidat nové možnosti tooyour `WMAppManifest.xml` souboru:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

### <a name="initialize-hello-reach-sdk"></a>Inicializace hello REACH SDK
1. V `App.xaml.cs`, volání `EngagementReach.Instance.Init();` v hello **Application_Launching** funkce hned po inicializaci agenta hello:
   
        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }
2. V `App.xaml.cs`, volání `EngagementReach.Instance.OnActivated(e);` v hello **Application_Activated** funkce hned po inicializaci agenta hello:
   
        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

Nyní je vše připraveno. A teď ověříme, zda jste základní integraci provedli správně.

## <a id="send"></a>Poslat tooyour aplikaci oznámení
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Pokud je aplikace hello otevřete jinak jako oznámení s informační zprávou jako následující hello, měli byste vidět oznámení na vašem zařízení, které se zobrazí jako oznámení v aplikaci: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
