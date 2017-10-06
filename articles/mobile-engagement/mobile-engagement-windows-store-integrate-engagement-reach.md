---
title: "aaaWindows univerzální aplikace dosáhnout integrace sady SDK"
description: "Jak tooIntegrate Azure Mobile Engagement Reach s univerzálních aplikací pro Windows"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a31ca1d6-856f-4aec-898a-07969ae5f7ec
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: af311c65940014083333853875a00173b8d6783e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-reach-sdk-integration"></a>Univerzální aplikace Windows dosáhnout integraci sady SDK
Je třeba postupovat podle hello integrace postup popsaný v hello [integraci sady Windows Universal Engagement SDK](mobile-engagement-windows-store-integrate-engagement.md) před těchto pokynů.

## <a name="embed-hello-engagement-reach-sdk-into-your-windows-universal-project"></a>Vložení hello Engagement Reach SDK do projektu univerzální pro Windows
Nemáte nic tooadd. `EngagementReach`odkazy a prostředky jsou už ve vašem projektu.

> [!TIP]
> Můžete přizpůsobit bitové kopie, které jsou umístěné v hello `Resources` složky projektu, zejména hello brand ikona (této výchozí toohello Engagement). Pro univerzální aplikace také můžete přesunout hello `Resources` složky na vaše sdílený projekt tooshare jeho obsah mezi aplikací, ale bude mít tookeep hello `Resources\EngagementConfiguration.xml` souborů na výchozího umístění, jako je platforma závislé.
> 
> 

## <a name="enable-hello-windows-notification-service"></a>Povolit hello služby oznámení Windows
### <a name="windows-8x-and-windows-phone-81-only"></a>Windows 8.x a Windows Phone 8.1 pouze
V pořadí toouse hello **služby oznámení Windows** (označované jako WNS) ve vaší `Package.appxmanifest` souborů na `Application UI` klikněte na `All Image Assets` pole levé robota hello. V hello napravo od hello `Notifications`, změňte `toast capable` z `(not set)` příliš`(Yes)`.

### <a name="all-platforms"></a>Všechny platformy
Je nutné toosynchronize vaší aplikace tooyour Microsoft účet a toohello platforma pro zapojení. Pro tuto potřebovat toocreate účet nebo přihlásit [Centrum vývojářů pro windows](https://dev.windows.com). Po, vytvořte novou aplikaci a najít hello SID a tajný klíč. Na hello engagement front-endu, přejděte na nastavení vaší aplikace v `native push` a vložte svoje přihlašovací údaje. Potom klikněte pravým tlačítkem na projekt, vyberte `store` a `Associate App with hello Store...`. Stačí tooselect hello aplikace musíte mít před toosynchronize ji vytvořit.

## <a name="initialize-hello-engagement-reach-sdk"></a>Inicializace hello Engagement Reach SDK
Upravit hello `App.xaml.cs`:

* Vložit `EngagementReach.Instance.Init` právě po `EngagementAgent.Instance.Init` ve vaší `InitEngagement` metoda:
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
        EngagementReach.Instance.Init(e);
      }
  
  Hello `EngagementReach.Instance.Init` běží ve vyhrazené vlákno. Nemáte toodo ho sami.

> [!NOTE]
> Pokud používáte nabízená oznámení jinde v aplikaci, pak máte příliš[sdílet kanál nabízené](#push-channel-sharing) s Engagement Reach.
> 
> 

## <a name="integration"></a>Integrace
Engagement nabízí dva způsoby, jak tooadd hello Reach v aplikaci plakáty a vkládaná zobrazení pro oznámení a dotazuje se ve vaší aplikaci: hello překrytí, integrace a hello webové zobrazení ruční integrace. Nesmí kombinovat obě přístup na hello stejné stránce.

Volba Hello mezi dvěma integrace hello může shrnout takto:

* Můžete rozhodnout integrace překrytí hello, pokud vaše stránky již dědí z hello agenta `EngagementPage`, bude stačit nahrazení `EngagementPage` podle `EngagementPageOverlay` a `xmlns:engagement="using:Microsoft.Azure.Engagement"` podle `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` na stránkách.
* Můžete se rozhodnout hello webové zobrazení ruční integrace Pokud chcete tooprecisely místní hello dosáhnout uživatelského rozhraní v rámci vaší stránky, nebo pokud nechcete, aby tooadd jiné stránky úrovně tooyour dědičnosti. 

### <a name="overlay-integration"></a>Integrace překrytí
Hello Engagement překrytí dynamicky přidá prvky uživatelského rozhraní hello použitou kampaně Reach toodisplay v stránku. Pokud není hello překrytí vyhovovaly vaší rozložení byste měli zvážit hello webové zobrazení ruční integrace místo.

V souboru změny XAML `EngagementPage` příliš reference`EngagementPageOverlay`

* Přidejte tooyour deklarací oborů názvů:
  
      xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"
* Nahraďte `engagement:EngagementPage` s `engagement:EngagementPageOverlay`:

**S EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">

            <!-- Your layout -->
        </engagement:EngagementPage>

**S EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">

            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

V souboru .cs značky stránku v `EngagementPageOverlay` místo `EngagementPage` a import `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

* Nahraďte `EngagementPage` s `EngagementPageOverlay`:

**S EngagementPage:**

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**S EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


Přidá technologie Hello Engagement překrytí `Grid` elementu na stránku tvořený rozložení a hello dva `WebView` prvky jeden pro hello banner a hello další jeden pro zobrazení vkládaná hello.

Můžete přizpůsobit hello překrytí elementů přímo v hello `EngagementPageOverlay.cs` souboru.

### <a name="web-views-manual-integration"></a>Webové zobrazení ruční integrace
Reach bude hledání svoje stránky pro hello dva `WebView` elementy odpovědná za zobrazování hello banner a hello vkládaná zobrazení. Dobrý den, co máte toodo je tooadd pouze tyto dvě `WebView` elementy někde na stránkách, tady je příklad:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


V tento příklad hello `WebView` elementy jsou roztažené toofit jejich kontejneru, který automaticky je znovu velikostí v případě změna velikosti obrazovky otočení nebo okna.

> [!WARNING]
> Je důležité tookeep hello stejné názvy `engagement_notification_content` a `engagement_announcement_content` pro hello `WebView` elementy. Reach je identifikace je podle názvu. 
> 
> 

## <a name="handle-datapush-optional"></a>Popisovač datapush (volitelné)
Pokud chcete, aby vaše aplikace toobe možné tooreceive Reach datová oznámení, máte dvě události tooimplement hello EngagementReach třídy:

V souboru App.xaml.cs v konstruktoru App() hello přidejte:

            EngagementReach.Instance.DataPushStringReceived += (body) =>
            {
              Debug.WriteLine("String data push message received: " + body);
              return true;
            };

            EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
            {
              Debug.WriteLine("Base64 data push message received: " + encodedBody);
              // Do something useful with decodedBody like updating an image view
              return true;
            };

Uvidíte, že hello zpětné volání z každé metoda vrátí logickou hodnotu. Zapojení odešle zpětné vazby tooits back-end po odeslání hello datová oznámení. Pokud zpětné volání hello vrací hodnotu false, hello `exit` zpětné vazby bude odesílat. Jinak bude `action`. Pokud je pro události hello nastavené žádné zpětné volání, hello `drop` tooEngagement bude vrácen zpětnou vazbu.

> [!WARNING]
> Zapojení není možné tooreceive násobky názory pro datová oznámení. Pokud máte v plánu tooset několik obslužné rutiny na události, mějte na paměti, že zpětnou vazbu hello bude odpovídat toohello naposledy odeslána. V takovém případě doporučujeme tooalways vrátí hello stejnou hodnotu tooavoid s matoucí zpětnou vazbu o hello front-endu.
> 
> 

## <a name="customize-ui-optional"></a>Přizpůsobení uživatelského rozhraní (volitelné)
### <a name="first-step"></a>Prvním krokem
Můžeme vám umožňují toocustomize hello reach uživatelského rozhraní.

toodo tedy máte toocreate, podtřídou třídy hello `EngagementReachHandler` třídy.

**Ukázkový kód:**

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

Potom nastavte hello obsah hello `EngagementReach.Instance.Handler` pole s vaší vlastní objekt v vaše `App.xaml.cs` třídu v rámci hello `App()` metoda.

**Ukázkový kód:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [!NOTE]
> Ve výchozím nastavení používá Engagement jejich vlastní implementaci `EngagementReachHandler`.
> Nemáte toocreate vlastní, a pokud tak učiníte, nemáte toooverride každou metodu. Hello výchozí chování je základní objekt Engagement tooselect hello.
> 
> 

### <a name="web-view"></a>Webové zobrazení
Ve výchozím nastavení použije Reach hello vložených prostředků hello DLL toodisplay hello oznámení a stránky.

tooprovide úplné možnosti přizpůsobení používáme pouze zobrazení webové stránky. Pokud chcete toocustomize rozložení, přepsat přímo soubory zdrojů hello `EngagementAnnouncement.html` a `EngagementNotification.html`. Zapojení musí všechny kód v `<body></body>` toorun správně. Ale můžete přidat značky vnější `engagement_webview_area`.

Můžete však rozhodnout toouse vaše vlastní prostředky.

Můžete přepsat `EngagementReachHandler` metody ve vaší podtřídami tootell Engagement toouse rozložení, ale trvat pozor tooembedded hello engagement mechanismus:

**Ukázkový kód:**

            // In your subclass of EngagementReachHandler

            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


Ve výchozím nastavení je AnnouncementHTML `ms-appx-web:///Resources/EngagementAnnouncement.html`. Reprezentuje hello souboru html, který návrh hello obsah nabízená zpráva (Text oznámení, webové anoucement a dotazování oznámení). Je AnnouncementName `engagement_announcement_content`. Je název hello hello webové zobrazení návrhu v stránku xaml.

Je NotfificationHTML `ms-appx-web:///Resources/EngagementNotification.html`. Reprezentuje hello souboru html, který návrh hello oznámení o nabízené zprávy. Je NotfificationName `engagement_notification_content`. Je název hello hello webové zobrazení návrhu v stránku xaml.

### <a name="customization"></a>Přizpůsobení
Můžete přizpůsobit oznámení a oznámení, že má webové zobrazení, je-li zachovat Engagement objektu. Ujistěte se, že objekt webové zobrazení je popsáno třikrát – hello poprvé ve vašem jazyce xaml, druhý čas v souboru .cs v metodě "setwebview()" hello a třetí čas v souboru html hello.

* Ve vašem xaml popisují hello aktuální grafické rozložení webového zobrazení součásti.
* V souboru .cs můžete definovat "setwebview()", ve kterém můžete nastavit hello dimenze hello dva webové zobrazení (oznámení, oznámení). Je velmi účinné při aplikace hello je změna velikosti.
* V souboru html Engagement hello jsme popisují hello webové zobrazení obsahu, návrhu a hello pozic prvky mezi sebou.

### <a name="launch-message"></a>Spusťte zprávu
Když uživatel klikne na systémové oznámení (oznámení), Engagement spustí aplikace hello, obsah hello zatížení hello nabízené zprávy a zobrazit stránku hello hello odpovídající kampaně.

Dochází ke zpoždění mezi hello spuštění aplikace a hello zobrazení hello hello stránky (v závislosti na rychlosti sítě hello).

tooindicate toohello uživatele, který se načítá něco, by měl poskytovat vizuální informace, jako je indikátor průběhu nebo indikátor průběhu. Zapojení nemůže zpracovat samostatně, ale poskytuje několik obslužné rutiny pro vás.

Přidejte tooimplement hello zpětného volání v souboru App.xaml.cs v "Veřejné App() {}":

            /* hello application has launched and hello content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };

            /* hello application has finished loading hello content and hello page
             * is about toobe displayed.
             * You should hide hello indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };

            /* hello content has been loaded, but an error has occurred.
             * You can provide an information toohello user.
             * You should hide hello indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

Zpětné volání hello můžete nastavit vaše metoda "Veřejné App() {}" vaší `App.xaml.cs` soubor, pokud možno před hello `EngagementReach.Instance.Init()` volání.

> [!TIP]
> Každý obslužná rutina je volána službou hello vlákna uživatelského rozhraní. Při použití a MessageBox nebo něco uživatelského rozhraní související nemáte tooworry.
> 
> 

## <a id="push-channel-sharing"></a>Push sdílení kanálu
Pokud používáte nabízená oznámení pro jiný účel ve vaší aplikaci máte toouse hello nabízené kanál sdílení funkce hello Engagement SDK. Toto je nabízené tooavoid vynechán.

* Můžete zadat vlastní nabízené kanál toohello Engagement Reach inicializace. Hello SDK použije místo požaduje novou.

Aktualizace hello Engagement Reach inicializace pomocí nabízených kanál v hello `InitEngagement` metoda z hello `App.xaml.cs` souboru:

    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

* Případně pokud chcete tooconsume hello push kanál po inicializaci Reach hello pak můžete nastavit zpětné volání na Engagement Reach tooget hello nabízené kanálu po jejím vytvoření hello SDK.

Nastavit vaše zpětné volání v jakémkoli místě **po** hello Reach inicializace:

    /* Set action on hello SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* hello forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Tip pro vlastní schéma
Poskytujeme použití vlastní schéma. Můžete odeslat jiný typ identifikátoru URI z toobe front-endu zapojení použít v aplikaci zapojení. Výchozí schéma jako `http, ftp, ...` jsou spravovat v systému Windows, bude okno řádku by šlo žádná výchozí aplikace nainstalované v zařízení. Můžete také vytvořit své vlastní schéma pro vaši aplikaci.

Hello jednoduchý způsob tooset vlastní schéma v aplikaci je tooopen vaší `Package.appxmanifest` přejděte v `Declarations` panelu. Vyberte `Protocol` v hello posuňte se k dispozici deklarace pole a přidejte ji. Upravit hello `Name` pole s nový protokol požadovaný název.

Nyní toouse tento protokol upravit vaše `App.xaml.cs` s hello `OnActivated` metoda a nezapomeňte také zde tooinitialize engagement:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);

              //TODO design action toodo when app is launch

              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;

                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion

