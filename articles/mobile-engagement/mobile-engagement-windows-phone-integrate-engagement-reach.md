---
title: "aaaWindows integrace sady SDK dosáhnout Phone Silverlight"
description: Jak tooIntegrate Azure Mobile Engagement Reach s aplikacemi pro Windows Phone Silverlight
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: d3516a6b-db9f-4cdb-a475-4148edf81af1
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 09c8767216e11963c5c600755ab8d4d11cd92034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-reach-sdk-integration"></a>Windows Phone Silverlight Reach integraci sady SDK
Je třeba postupovat podle hello integrace postup popsaný v hello [integraci sady Windows Phone Silverlight Engagement SDK](mobile-engagement-windows-phone-integrate-engagement.md) před těchto pokynů.

## <a name="embed-hello-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>Vložení hello Engagement Reach SDK do projektu Windows Phone Silverlight
Nemáte nic tooadd. `EngagementReach`odkazy a prostředky jsou už ve vašem projektu.

> [!TIP]
> Můžete přizpůsobit bitové kopie, které jsou umístěné v hello `Resources` složky projektu, zejména hello brand ikona (této výchozí toohello Engagement).
> 
> 

## <a name="add-hello-capabilities"></a>Přidání možností hello
Hello Engagement Reach SDK potřebuje některé další funkce.

Otevřete váš `WMAppManifest.xml` soubor a zkontrolujte, zda jsou deklarovány této hello následující možnosti:

* `ID_CAP_PUSH_NOTIFICATION`
* `ID_CAP_WEBBROWSERCOMPONENT`

Hello nejprve jeden používá hello MPNS služby tooallow hello zobrazení informační zpráva. Hello druhá je použité tooembed úlohu prohlížeče do hello SDK.

Upravit hello `WMAppManifest.xml` souboru a přidejte uvnitř hello `<Capabilities />` značky:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

## <a name="enable-hello-microsoft-push-notification-service"></a>Povolit hello Microsoft službu nabízených oznámení
V pořadí toouse hello **Microsoft službu nabízených oznámení** (označované jako MPNS) vaší `WMAppManifest.xml` soubor musí mít `<App />` značku s `Publisher` atribut nastavit toohello název projektu.

## <a name="initialize-hello-engagement-reach-sdk"></a>Inicializace hello Engagement Reach SDK
### <a name="engagement-configuration"></a>Konfigurace zapojení
Konfigurace Engagement Hello je centralizovaná v hello `Resources\EngagementConfiguration.xml` souboru projektu.

Úpravy konfigurace reach toospecify souboru:

* *Volitelné*, zda je aktivována hello nativního nabízení (MPNS) nebo není mezi `<enableNativePush>` a `</enableNativePush>` značky, (`true` ve výchozím nastavení).
* *Volitelné*, označuje název hello hello nabízené kanál mezi `<channelName>` a `</channelName>` značky, poskytují hello stejné, že vaše aplikace může aktuálně použít nebo hodnotu nevyplňujte.

Pokud chcete, aby ho za běhu místo toho můžete volat hello následující toospecify metoda před inicializací agenta hello Engagement:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether hello native push (MPNS) is activated or not. */

    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide hello same channel name that your application may currently use. */

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [!TIP]
> Můžete zadat název hello hello MPNS nabízené kanál svojí aplikace. Ve výchozím nastavení vytvoří Engagement název založený na hello appId. Máte bez nutnosti toospecify hello názvu sami, s výjimkou Pokud máte v plánu toouse hello nabízené kanál mimo zapojení.
> 
> 

### <a name="engagement-initialization"></a>Inicializace zapojení
Upravit hello `App.xaml.cs`:

* Přidat tooyour `using` příkazy:
  
      using Microsoft.Azure.Engagement;
* Vložit `EngagementReach.Instance.Init` právě po `EngagementAgent.Instance.Init` v `Application_Launching` :
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
         EngagementAgent.Instance.Init();
         EngagementReach.Instance.Init();
      }
* Vložit `EngagementReach.Instance.OnActivated` v hello `Application_Activated` metoda:
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
         EngagementReach.Instance.OnActivated(e);
      }

> [!IMPORTANT]
> Hello `EngagementReach.Instance.Init` běží ve vyhrazené vlákno. Nemáte toodo ho sami.
> 
> 

## <a name="app-store-submission-considerations"></a>Aspekty odeslání úložiště aplikací
Microsoft ukládá některá pravidla při použití hello nabízených oznámení:

Z hello Microsoft [zásady aplikací] části 2.9, dokumentace:

1) Musíte požádat hello uživatele tooaccept tooreceive nabízená oznámení. Potom si v nastaveních, přidejte způsob toodisable hello nabízená oznámení.

objekt EngagementReach Hello nabízí dvě metody toomanage hello opt v nebo odhlášení, `EnableNativePush()` a `DisableNativePush()`. Může například vytvořit možnost v nastavení hello s toodisable přepínač nebo povolit MPNS.

Můžete také rozhodnout toodeactivate MPNS prostřednictvím konfigurace Engagement hello\<windows phone-sdk-reach konfigurace\>.

> 2.9.1) aplikace hello musí nejprve popisují toobe oznámení hello zadaný a **získat express oprávnění uživatele hello (výslovný souhlas)**, a **musí nabízejí mechanismus, pomocí které hello uživatele můžete vyjádření výslovného nesouhlasu s přijetím nabízená oznámení**. Všechna oznámení zadaný pomocí hello Microsoft službu nabízených oznámení musí být v souladu s hello poskytnutý popis toohello uživatele a musí dodržovat všechny příslušné [zásady aplikací] [ Content Policies]a [další požadavky pro konkrétní aplikaci typy].
> 
> 

2) Neměli byste používat příliš mnoho nabízená oznámení. Zapojení bude zpracovávat oznámení za vás.

> 2.9.2) hello aplikace a jeho použití programu hello Microsoft službu nabízených oznámení musí není nadměrně pomocí dostatečnou kapacitu sítě nebo šířka pásma hello Microsoft službu nabízených oznámení nebo jinak neoprávněně nebyli Windows Phone nebo jiných Microsoft zařízení nebo službu. s nadměrné nabízená oznámení, jak dle svého uvážení určí společnost Microsoft a nesmí poškodit nebo narušit žádné sítě, Microsoft nebo servery nebo serverech třetích stran nebo sítě připojené toohello Microsoft službu nabízených oznámení.
> 
> 

3) Nespoléhejte na MPNS toosend criticals informace. Zapojení používá MPNS, takže toto pravidlo platí také pro hello kampaně vytvářet v hello Engagement front-endu.

> 2.9.3) hello Microsoft službu nabízených oznámení nemusí být použité toosend oznámení, která jsou mise kritické nebo jinak mohou ovlivnit záležitosti životnosti nebo smrti, včetně bez omezení kritické oznámení související tooa lékařské zařízení nebo podmínku. MICROSOFT výslovně zříká ANY záruky, hello použití z hello MICROSOFT NABÍZENÁ oznámení služby nebo doručení z MICROSOFT NABÍZENÁ oznámení služby oznámení bude být bez přerušení, chyba volné nebo jinak zaručit tooOCCUR základ v reálném čase A ON.
> 
> 

**Nemůžeme zaručit aplikace předá proces ověření hello Pokud nerespektují tato doporučení.**

## <a name="handle-data-push-optional"></a>Zpracování datová oznámení (volitelné)
Pokud chcete, aby vaše aplikace toobe možné tooreceive Reach datová oznámení, máte dvě události tooimplement hello EngagementReach třídy:

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

Potom nastavte hello obsah hello `EngagementReach.Instance.Handler` pole s vaší vlastní objekt v vaše `App.xaml.cs` třídu v rámci hello `Application_Launching` metoda.

**Ukázkový kód:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [!NOTE]
> Ve výchozím nastavení používá Engagement jejich vlastní implementaci `EngagementReachHandler`. Nemáte toocreate vlastní, a pokud tak učiníte, nemáte toooverride každou metodu. Hello výchozí chování je základní objekt Engagement tooselect hello.
> 
> 

### <a name="layouts"></a>Rozložení
Ve výchozím nastavení použije Reach hello vložených prostředků hello DLL toodisplay hello oznámení a stránky.

Ale můžete rozhodnout toouse, vlastní prostředky tooreflect vaší značkou v těchto součástí.

Můžete přepsat `EngagementReachHandler` metody ve vaší podtřídami tootell Engagement toouse vaše rozložení:

**Ukázkový kód:**

    // In your subclass of EngagementReachHandler

    public override string GetTextViewAnnouncementUri()
    {
       // return hello path of your own xaml
    }

    public override string GetWebViewAnnouncementUri()
    {
       // return hello path of your own xaml
    }

    public override string GetPollUri()
    {
       // return hello path of your own xaml
    }

    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [!TIP]
> Hello `CreateNotification` metoda může vrátit hodnotu null. Hello oznámení se nezobrazí a kampaně reach hello se zahodí.
> 
> 

toosimplify implementaci rozložení poskytujeme také vlastní xaml, který může sloužit jako základ pro váš kód. Se nacházejí v archivu Engagement SDK hello (nebo src/reach /).

> [!WARNING]
> Hello zdroje, které poskytujeme jsou hello přesně stejnou ty, které používáme. Takže pokud chcete toomodify je přímo, nemáte zapomněli toochange hello obor názvů a hello název.
> 
> 

### <a name="notification-position"></a>Pozice oznámení
Ve výchozím nastavení v hello spodní části hello aplikace se zobrazí oznámení v aplikaci. Toto chování můžete změnit přepsání hello `GetNotificationPosition` metoda hello `EngagementReachHandler` objektu.

    // In your subclass of EngagementReachHandler

    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of hello EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

V současné době můžete zvolit hello `BOTTOM` (výchozí) a `TOP` pozic.

### <a name="launch-message"></a>Spusťte zprávu
Když uživatel klikne na systémové oznámení (oznámení), spustí Engagement hello aplikace, obsah hello zatížení hello nabízené zprávy a zobrazit stránku hello hello odpovídající kampaně.

Dochází ke zpoždění mezi hello spuštění aplikace a hello zobrazení hello hello stránky (v závislosti na rychlosti sítě hello).

tooindicate toohello uživatele, který se načítá něco, by měl poskytovat vizuální informace, jako je indikátor průběhu nebo indikátor průběhu. Zapojení nemůže zpracovat samostatně, ale poskytuje několik obslužné rutiny pro vás.

tooimplement hello zpětného volání, proveďte:

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

Zpětné volání hello můžete nastavit v vaše `Application_Launching` metodu vaše `App.xaml.cs` soubor, pokud možno před hello `EngagementReach.Instance.Init()` volání.

> [!TIP]
> Každý obslužná rutina je volána službou hello vlákna uživatelského rozhraní. Při použití a MessageBox nebo něco uživatelského rozhraní související nemáte tooworry.
> 
> 

[zásady aplikací]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[další požadavky pro konkrétní aplikaci typy]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx

