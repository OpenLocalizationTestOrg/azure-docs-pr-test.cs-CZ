---
title: aaaWindows integraci sady Engagement SDK Phone Silverlight
description: Jak tooIntegrate Azure Mobile Engagement s aplikacemi pro Windows Phone Silverlight
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 447fea8d-f4e3-4ad4-8ec0-8e3cf1ad3ab0
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: f65683a62e5256cea469a3a73d99ade4331cb6bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-engagement-sdk-integration"></a>Integraci sady Windows Phone Silverlight Engagement SDK
> [!div class="op_single_selector"]
> * [Univerzální pro Windows](mobile-engagement-windows-store-integrate-engagement.md) 
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [iOS](mobile-engagement-ios-integrate-engagement.md) 
> * [Android](mobile-engagement-android-integrate-engagement.md) 
> 
> 

Tento postup popisuje hello nejjednodušší způsob, jak tooactivate Azure Mobile Engagement analýzy a monitorování funkce v aplikaci Silverlight pro Windows Phone.

Hello následující kroky jsou že dostatek tooactivate hello protokolů je nutná sestava toocompute všechny statistické údaje týkající se uživatelů, relací, aktivity, dojde k chybě a Technicals. Hello protokolů je nutná sestava toocompute další statistiky jako události a chyby úlohy je třeba provést ručně pomocí hello Engagement rozhraní API (v tématu [jak toouse hello advanced Mobile Engagement označování rozhraní API v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-use-engagement-api.md) dole) vzhledem k tomu, že tyto statistické údaje jsou závislé aplikace.

## <a name="supported-versions"></a>Podporované verze
Mobile Engagement SDK pro Silverlight pro Windows Hello lze pouze integrovat do aplikace cílený na:

* Windows Phone 8.0
* Windows Phone 8.1 Silverlight

> [!NOTE]
> Pokud jste se cílení na Windows Phone 8.1 (bez Silverlight), pak odkazovat toohello [univerzální pro Windows postup integrace](mobile-engagement-windows-store-integrate-engagement.md).
> 
> 

## <a name="install-hello-mobile-engagement-silverlight-sdk"></a>Nainstalujte hello Mobile Engagement Silverlight SDK
Mobile Engagement SDK pro Silverlight pro Windows Hello je k dispozici jako balíček Nuget s názvem *MicrosoftAzure.MobileEngagement*. Můžete ji nainstalovat ze hello Správce balíčků Nuget Visual Studio. 

## <a name="add-hello-capabilities"></a>Přidání možností hello
Některé možnosti hello Windows Phone Silverlight SDK v pořadí toowork musí Hello Engagement SDK správně.

Otevřete váš `WMAppManifest.xml` souboru a ujistěte se, že hello následující funkce jsou deklarované v hello `Capabilities` panelu:

* `ID_CAP_NETWORKING`
* `ID_CAP_IDENTITY_DEVICE`

## <a name="initialize-hello-engagement-sdk"></a>Inicializace hello Engagement SDK
### <a name="engagement-configuration"></a>Konfigurace zapojení
Konfigurace Engagement Hello je centralizovaná v hello `Resources\EngagementConfiguration.xml` souboru projektu.

Upravte tento soubor toospecify:

* Připojovací řetězec aplikací mezi značky `<connectionString>` a `<\connectionString>`.

Pokud chcete, aby ho za běhu místo toho můžete volat hello následující toospecify metoda před inicializací agenta hello Engagement:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

Hello připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure Classic hello.

### <a name="engagement-initialization"></a>Inicializace zapojení
Když vytvoříte nový projekt, `App.xaml.cs` se vygeneruje soubor. Tato třída dědí z `Application` a obsahuje mnoho důležité metody. Bude také použít tooinitialize hello Engagement SDK.

Upravit hello `App.xaml.cs`:

* Přidat tooyour `using` příkazy:
  
      using Microsoft.Azure.Engagement;
* Vložit `EngagementAgent.Instance.Init` v hello `Application_Launching` metoda:
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
        EngagementAgent.Instance.Init();
      }
* Vložit `EngagementAgent.Instance.OnActivated` v hello `Application_Activated` metoda:
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
      }

> [!WARNING]
> Jsme důrazně bránit vám tooadd hello Engagement inicializace na jiném místě vaší aplikace. Ale, uvědomte si, že hello `EngagementAgent.Instance.Init` metoda se spouští na vyhrazené vlákno a ne na hello vlákna uživatelského rozhraní.
> 
> 

## <a name="basic-reporting"></a>Vytváření základních sestav
### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Doporučená metoda: přetížení vaší `PhoneApplicationPage` třídy
V pořadí tooactivate hello sestavu všechny protokoly hello vyžadují zapojení toocompute uživatelů, relací, aktivity, dojde k chybě a technické statistiky, můžete jednoduše provést všechny vaše `PhoneApplicationPage` dílčí třídy dědí hello `EngagementPage` třídy.

Tady je příklad toho, jak toodo to pro stránku vaší aplikace. Můžete provést hello samé pro všechny stránky vaší aplikace.

#### <a name="c-source-file"></a>C# zdrojový soubor
Upravit stránku `.xaml.cs` souboru:

* Přidat tooyour `using` příkazy:
  
      using Microsoft.Azure.Engagement;
* Nahraďte `PhoneApplicationPage` s `EngagementPage` :

**Bez Engagement:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**S Engagement:**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!WARNING]
> Pokud stránka dědí z hello `OnNavigatedTo` metoda, být opatrní toolet hello `base.OnNavigatedTo(e)` volání. V opačném hello aktivita nebude hlášena. Ve skutečnosti hello `EngagementPage` volá `StartActivity` uvnitř hello `OnNavigatedTo` metoda.
> 
> 

#### <a name="xaml-file"></a>Souboru XAML
Upravit stránku `.xaml` souboru:

* Přidejte tooyour deklarací oborů názvů:
  
      xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
* Nahraďte `phone:PhoneApplicationPage` s `engagement:EngagementPage` :

**Bez Engagement:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**S Engagement:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">

            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-hello-default-behavior"></a>Přepsat výchozí chování hello
Ve výchozím nastavení je název třídy hello stránku hello uvedená jako název aktivity hello a žádné navíc. Pokud třída hello používá příponu "Stránka" hello, Engagement také odebere.

Pokud chcete toooverride hello výchozí chování pro název hello, jednoduše přidejte tento kód tooyour:

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Pokud chcete, tooreport některé doplňující informace s vaší aktivitou, můžete přidat tento kód tooyour:

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

Tyto metody jsou volat v rámci hello `OnNavigatedTo` metoda stránky.

### <a name="alternate-method-call-startactivity-manually"></a>Alternativní metoda: volání `StartActivity()` ručně
Pokud nemůžete nebo nechcete, aby toooverload vaše `PhoneApplicationPage` třídy, můžete místo toho spustit vaše aktivity voláním `EngagementAgent` metody přímo.

Doporučujeme, abyste volání `StartActivity` uvnitř vaší `OnNavigatedTo` metoda vaší PhoneApplicationPage.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [!IMPORTANT]
> Zajistěte, aby že správně ukončete svou relaci.
> 
> Hello SDK automaticky volá hello `EndActivity` metoda při zavření aplikace hello. Z toho důvodu je **vysoce** doporučená toocall hello `StartActivity` metoda vždy, když aktivita hello hello uživatele změnit a příliš**nikdy** volání hello `EndActivity` metoda. Tato metoda odesílá server Engagement zpráv toohello, že aktuální uživatel hello opustil hello aplikace, a to ovlivní všechny protokoly aplikací.
> 
> 

## <a name="advanced-reporting"></a>Rozšířená tvorba sestav
Volitelně můžete tooreport aplikace konkrétní události, chyb a úlohy, toodo tak, použijte hello další metody nalezeny v hello `EngagementAgent` třídy. Hello Engagement API umožňuje toouse všechny rozšířené možnosti Engagement.

Další informace najdete v tématu [jak toouse hello advanced Mobile Engagement označování rozhraní API v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-use-engagement-api.md).

## <a name="advanced-configuration"></a>Pokročilá konfigurace
### <a name="disable-automatic-crash-reporting"></a>Zakázat automatické hlášení chyb
Můžete vypnout automatické hlášení hello funkci Engagement generování sestav. Pak když dojde k neošetřené výjimce, Engagement nebude provádět žádné kroky.

> [!WARNING]
> Pokud máte v plánu toodisable tuto funkci, mějte na paměti, když dojde k nezpracované chybě ve vaší aplikaci, nepošle Engagement hello havárií **a** zavřít hello relace a úlohy.
> 
> 

Automatické hlášení toodisable vytváření sestav, stačí upravit konfiguraci v závislosti na způsob hello deklarujete objekt:

#### <a name="from-engagementconfigurationxml-file"></a>Z `EngagementConfiguration.xml` souboru
Nastavit sestavy zhroutí příliš`false` mezi `<reportCrash>` a `</reportCrash>` značky.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Z `EngagementConfiguration` objektu v době běhu
Nastavit toofalse havárií sestavy pomocí EngagementConfiguration objektu.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Režim shluku
Ve výchozím nastavení sestavy služby zapojení hello protokolů v reálném čase. Pokud vaše aplikace hlásí protokoly velmi často, je lepší toobuffer hello protokoly a tooreport je všechny najednou na pravidelné základní časové (tomu se říká hello "shluků režim").

toodo tedy volat metodu hello:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Hello argument je hodnota **milisekundách**. Kdykoli Pokud chcete protokolování v reálném čase hello tooreactivate, stačí zavoláte metoda hello bez zadání parametru nebo s hodnotou hello 0.

Hello shluků režimu mírně zvýšit hello baterie životnosti ale má vliv na hello monitorování Engagement: všechny doba trvání relace a úlohy se prahová hodnota shluků zaokrouhlené toohello (tedy relací a úlohy kratší, než je prahová hodnota shluků hello nemusí být zobrazeny). Je doporučeno toouse práh shluků než 30000 (30s). Máte toobe vědět, které uložené protokoly jsou omezené too300 položky. Pokud odesílání je příliš dlouhý. může dojít ke ztrátě některých protokoly.

> [!WARNING]
> Hello shluků mezní hodnotu nelze konfigurovat tooa období menší než jedna sekunda. Pokud se pokusíte toodo tak, hello SDK bude zobrazit trasování s chybou hello a bude automaticky resetovat toohello výchozí hodnota, který je nula sekund. Tím se aktivuje hello SDK tooreport hello protokolů v reálném čase.
> 
> 

