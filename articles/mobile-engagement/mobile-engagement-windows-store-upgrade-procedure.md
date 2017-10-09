---
title: "aaaWindows univerzální aplikace SDK Upgrade procedury"
description: "Postupy upgradu systému Windows Universal SDK aplikací pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 4c898175-2cd6-43db-b350-bb408332f24d
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 95aba5d55cd65d4190aad35737f872414b5ed443
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-sdk-upgrade-procedures"></a>Postupy upgradu systému Windows Universal SDK aplikace
Pokud již mít integrovanou starší verze zapojení do své aplikace, musíte tooconsider hello následující body při upgradu hello SDK.

Toofollow může mít několik postupů, pokud provedena několik verzí hello SDK. Například pokud migrujete z 0.10.1 too0.11.0 máte toofirst postupujte podle hello "z 0.9.0 too0.10.1" postup pak hello "z 0.10.1 too0.11.0" postup.

## <a name="from-330-too340"></a>Z 3.3.0 too3.4.0
### <a name="test-logs"></a>Protokolů testování
Protokoly konzoly vyprodukované hello SDK teď může být povolena nebo zakázána nebo filtrovat. toocustomize se aktualizovat hello vlastnost `EngagementAgent.Instance.TestLogEnabled` tooone hello hodnota dostupná z hello `EngagementTestLogLevel` výčtu, například:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Zdroje
bylo vylepšeno Hello Reach překrytí. Je součástí prostředky pro balíček NuGet sady SDK hello.

Při upgradu toohello novou verzi hello SDK můžete zvolit, že jestli se mají tookeep existující soubory z hello překrytí složku vašich prostředků, nebo není:

* Pokud předchozí překrytí hello pracuje pro vás nebo jsou integrací hello `WebView` elementy ručně pak můžete rozhodnout tookeep, vaše stávající soubory, ji budou i nadále fungovat. 
* Pokud chcete, aby tooupdate toohello nové překrytí, jenom nahradit hello celou `overlay` složky z vašich prostředků s novým hello z balíčku SDK hello (aplikace UWP: Po provedení upgradu hello můžete získat novou složku překrytí hello % USERPROFILE %\\. nuget\ packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [!WARNING]
> Pomocí nové překrytí hello přepíše všechny úpravy probíhají hello předchozí verze.
> 
> 

## <a name="from-320-too330"></a>Z 3.2.0 too3.3.0
### <a name="resources"></a>Zdroje
Tento krok se týká jenom vlastní prostředky. Pokud jste upravili hello prostředky poskytované hello SDK (html, obrázky, překrytí), pak máte toobackup je před upgradem a použít znovu vlastní na upgradovat prostředky.

## <a name="from-310-too320"></a>Z 3.1.0 too3.2.0
### <a name="resources"></a>Zdroje
Tento krok se týká jenom vlastní prostředky. Pokud jste upravili hello prostředky poskytované hello SDK (html, obrázky, překrytí), pak máte toobackup je před upgradem a použít znovu vlastní na upgradovat prostředky.

### <a name="webview-integration"></a>Integrace webové zobrazení
V této verzi byly zavedeny velikostem některé vylepšení toomatch jiné zařízení. Ujistěte se, že vaše integrace webové zobrazení hello odpovídají hello následující:

Ve vaší () stránky XAML:

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

A v souboru přidružené .cs:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated toowithin a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();

                /* Set your webview elements toohello correct size. */
                SetWebView(width, height);
              }

              #region tooimplement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update hello webview when hello app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update hello webview when hello app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure toodetach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }

              /* "width" and "height" are hello current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif

              /// <summary>
              /// Set your webview elements toohello correct size.
              /// </summary>
              /// <param name="width">hello width of your current display.</param>
              /// <param name="height">hello height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }

              /// <summary>
              /// Handler that takes hello Windows.Current.SizeChanged and indicates that webviews have toobe resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;

                /* Set your webview elements toohello correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes hello ApplicationView.VisibleBoundsChanged and indicates that webviews have toobe resized
              /// </summary>
              /// <param name="sender">hello related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;

                /* Set your webview elements toohello correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

## <a name="from-200-too300"></a>Z 2.0.0 too3.0.0
### <a name="resources"></a>Zdroje
Tento krok se týká jenom vlastní prostředky. Pokud jste upravili hello prostředky poskytované hello SDK (html, obrázky, překrytí), pak máte toobackup je před upgradem a použít znovu vlastní na upgradovat prostředky.

## <a name="from-111-too200"></a>Z 1.1.1 too2.0.0
Hello následující text popisuje, jak toomigrate integraci sady SDK z hello Capptain služby nabízených Capptain SAS do aplikace používá technologii Azure Mobile Engagement. 

> [!IMPORTANT]
> Capptain a Mobile Engagement nejsou hello stejné služby a postup hello níže uvedené jenom označuje, jak toomigrate hello klientskou aplikaci. Migrace hello SDK v aplikaci hello není migrace dat ze sady Mobile Engagement pro toohello hello Capptain servery
> 
> 

Pokud provádíte migraci ze starší verze, prosím nejdřív najdete hello Capptain webu toomigrate too1.1.1 pak použít následující postup hello

### <a name="nuget-package"></a>Balíček Nuget
Nahraďte **Capptain.WindowsPhone** podle **MicrosoftAzure.MobileEngagement** balíček Nuget.

### <a name="applying-mobile-engagement"></a>Použití Mobile Engagement
Hello SDK používá termín hello `Engagement`. Je třeba tooupdate vašeho projektu toomatch tuto změnu.

Je nutné toouninstall váš aktuální Capptain balíček nuget. Zvažte, se odeberou všechny změny ve složce Capptain prostředky. Pokud chcete, aby tookeep tyto soubory pak proveďte jejich kopii.

Potom nainstalujte balíček nuget Microsoft Azure Engagement nové hello na projektu. Najdete ho přímo na [nuget webu]. nebo sem index. Tato akce nahradí všechny soubory prostředky používané Engagement a přidá nové tooyour Engagement DLL hello projektu odkazy.

Tooclean máte odstraněním Capptain DLL odkazy odkazy projektu. Pokud to neuděláte, budou v konfliktu hello verzi Capptain a dojde k chybám.

Pokud jste upravili Capptain prostředky, zkopírujte původní soubory obsahu a vložit do nové soubory Engagement hello. Upozorňujeme, že soubory xaml a cs obsahují toobe aktualizovat.

Po dokončení těchto kroků stačí tooreplace staré odkazy Capptain podle hello nové Engagement odkazy.

1. Všechny obory názvů Capptain mít toobe aktualizovat.
   
    Před migrací:
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    Po dokončení migrace:
   
        using Microsoft.Azure.Engagement;
2. Všechny třídy Capptain, které obsahují "Capptain" by měly obsahovat "Engagement".
   
    Před migrací:
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    Po dokončení migrace:
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. Pro soubory xaml Capptain obor názvů a atributy také změnit.
   
    Před migrací:
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    Po dokončení migrace:
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. Změny stránky překrytí
   
   > [!IMPORTANT]
   > Překrytí také změní. Jeho nový obor názvů je `Microsoft.Azure.Engagement.Overlay`. Má toobe používají v souborech xaml a cs. Kromě toho `CapptainGrid` jmenuje toobe `EngagementGrid`, `capptain_notification_content` a `capptain_announcement_content` jsou pojmenované `engagement_notification_content` a `engagement_announcement_content`.
   > 
   > 
   
    Pro překrytí:
   
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
   
    Stane se:
   
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>
5. Pro hello jiné prostředky, jako je Capptain obrázky a soubory HTML, Všimněte si, že také byly přejmenovat toouse "Engagement".

### <a name="project-declaration"></a>Deklarace projektu
Na Package.appxmanifest `File Type Associations` byla aktualizována z:

* capptain\_dosáhnout\_obsahu tooengagement\_dosáhnout\_obsahu
* capptain\_protokolu\_souboru tooengagement\_protokolu\_souboru

### <a name="application-id--sdk-key"></a>ID aplikace nebo klíč SDK
Zapojení používá připojovací řetězec. Nemáte toospecify ID aplikací a klíčem SDK s Mobile Engagement, stačí toospecify připojovací řetězec. Můžete ho nastavit tak v souboru EngagementConfiguration.

Hello Engagement konfigurace může být nastavena v vaší `Resources\EngagementConfiguration.xml` souboru projektu.

Upravte tento soubor toospecify:

* Připojovací řetězec aplikací mezi značky `<connectionString>` a `<\connectionString>`.

Pokud chcete, aby ho za běhu místo toho můžete volat hello následující toospecify metoda před inicializací agenta hello Engagement:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

Hello připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure Classic hello.

### <a name="items-name-change"></a>Změna názvu položky
Všechny položky s názvem *capptain* byly pojmenovány *engagement*. Podobně jako pro *Capptain* příliš*Engagement*.

Příklady běžně používané Capptain položek:

* CapptainConfiguration nyní s názvem EngagementConfiguration
* Nyní s názvem EngagementAgent CapptainAgent
* Nyní s názvem EngagementReach CapptainReach
* Nyní s názvem EngagementHttpConfig CapptainHttpConfig
* Nyní s názvem GetEngagementPageName GetCapptainPageName

Všimněte si, že přejmenovat také ovlivňuje přepsat metody.

