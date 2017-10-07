---
title: "aaaWindows Universal integraci sady SDK zapojení aplikace"
description: "Jak tooIntegrate Azure Mobile Engagement s univerzálních aplikací pro Windows"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 71236b68-5ebd-44aa-8c82-c7ca8098ea05
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 18543798099c233dbe55cc387ba0216e369c8596
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-engagement-sdk-integration"></a>Integraci sady Engagement SDK univerzálních aplikací pro Windows
> [!div class="op_single_selector"]
> * [Univerzální platforma Windows](mobile-engagement-windows-store-integrate-engagement.md) 
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [iOS](mobile-engagement-ios-integrate-engagement.md) 
> * [Android](mobile-engagement-android-integrate-engagement.md) 
> 
> 

Tento postup popisuje analýzy a monitorování funkce v aplikaci univerzální pro Windows hello nejjednodušší způsob, jak tooactivate Engagement.

Hello následující kroky jsou že dostatek tooactivate hello protokolů je nutná sestava toocompute všechny statistické údaje týkající se uživatelů, relací, aktivity, dojde k chybě a Technicals. Hello protokolů je nutná sestava toocompute další statistiky jako události a chyby úlohy je třeba provést ručně pomocí hello Engagement rozhraní API (v tématu [jak toouse hello advanced označování rozhraní API v aplikaci Windows Universal Mobile Engagement](mobile-engagement-windows-store-use-engagement-api.md) od Tyto statistické údaje jsou závislé aplikace.

## <a name="supported-versions"></a>Podporované verze
Mobile Engagement SDK pro univerzální aplikace pro Windows Hello lze pouze integrovat do prostředí Windows Runtime a aplikací pro univerzální platformu Windows:

* Windows 8
* Windows 8.1
* Windows Phone 8.1
* Windows 10 (desktop a mobile rodiny)

> [!NOTE]
> Pokud jsou cílem Windows Phone Silverlight pak odkazovat toohello [postup integrace Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md).
> 
> 

## <a name="install-hello-mobile-engagement-universal-apps-sdk"></a>Nainstalujte hello Universal SDK aplikace Mobile Engagement
### <a name="all-platforms"></a>Všechny platformy
Mobile Engagement SDK pro univerzální aplikace pro Windows Hello je k dispozici jako balíček Nuget s názvem *MicrosoftAzure.MobileEngagement*. Můžete ji nainstalovat ze hello Správce balíčků Nuget Visual Studio.

### <a name="windows-8x-and-windows-phone-81"></a>Windows 8.x a Windows Phone 8.1
NuGet automaticky nasadí hello prostředků sady SDK v hello `Resources` složku v kořenovém adresáři projektu aplikace hello.

### <a name="windows-10-universal-windows-platform-applications"></a>Aplikace Windows 10 univerzální platformu Windows
NuGet automaticky Nenasazuje hello SDK prostředky v aplikaci UWP ještě. Je nutné ji ručně až prostředky nasazení je znovu zavedena v NuGet toodo:

1. Otevřete váš Průzkumníka souborů.
2. Přejděte toohello následující umístění (**x.x.x** je hello verzi Engagement instalujete): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\*  *x.x.x**\\content\win81*
3. Přetažení hello **prostředky** složku z hello souboru explorer toohello kořenového adresáře projektu v sadě Visual Studio.
4. V sadě Visual Studio vyberte projekt a aktivovat hello **zobrazit všechny soubory** ikonu nad hello **Průzkumníku řešení**.
5. Některé soubory nejsou zahrnuty v projektu hello. tooimport je najednou klikněte pravým tlačítkem na hello **prostředky** složky, **vyloučit z projektu** pak jiné klikněte pravým tlačítkem na hello **prostředky** složky, **zahrnout v projektu** toore-zahrnovat celou složku hello. Všechny soubory z hello **prostředky** složky jsou teď součástí projektu.

## <a name="add-hello-capabilities"></a>Přidání možností hello
Některé možnosti hello Windows SDK v pořadí toowork musí Hello Engagement SDK správně.

Otevřete váš `Package.appxmanifest` soubor a zkontrolujte, zda jsou deklarovány této hello následující možnosti:

* `Internet (Client)`

## <a name="initialize-hello-engagement-sdk"></a>Inicializace hello Engagement SDK
### <a name="engagement-configuration"></a>Konfigurace zapojení
Konfigurace Engagement Hello je centralizovaná v hello `Resources\EngagementConfiguration.xml` souboru projektu.

Upravte tento soubor toospecify:

* Připojovací řetězec aplikací mezi značky `<connectionString>` a `<\connectionString>`.

Pokud chcete, aby ho za běhu místo toho můžete volat hello následující toospecify metoda před inicializací agenta hello Engagement:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set hello Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

Hello připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure Classic hello.

### <a name="engagement-initialization"></a>Inicializace zapojení
Když vytvoříte nový projekt, `App.xaml.cs` se vygeneruje soubor. Tato třída dědí z `Application` a obsahuje mnoho důležité metody. Bude také použít tooinitialize hello Engagement SDK.

Upravit hello `App.xaml.cs`:

* Přidat tooyour `using` příkazy:
  
      using Microsoft.Azure.Engagement;
* Definujte metoda tooshare hello Engagement inicializace jednou pro všechna volání:
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
  
        // or
  
        EngagementAgent.Instance.Init(e, engagementConfiguration);
      }
* Volání `InitEngagement` v hello `OnLaunched` metoda:
  
      protected override void OnLaunched(LaunchActivatedEventArgs e)
      {
        InitEngagement(e);
      }
* Při spuštění vaší aplikace pomocí vlastní schéma jiná aplikace nebo hello příkazového řádku pak hello `OnActivated` metoda je volána. Musíte taky tooinitiate hello Engagement SDK, když je aktivován vaší aplikace. toodo tedy přepsat `OnActivated` metoda:
  
      protected override void OnActivated(IActivatedEventArgs args)
      {
        InitEngagement(args);
      }

> [!IMPORTANT]
> Jsme důrazně bránit vám tooadd hello Engagement inicializace na jiném místě vaší aplikace.
> 
> 

## <a name="basic-reporting"></a>Vytváření základních sestav
### <a name="recommended-method-overload-your-page-classes"></a>Doporučená metoda: přetížení vaší `Page` třídy
V pořadí tooactivate hello sestavu všechny protokoly hello vyžadují zapojení toocompute uživatelů, relací, aktivity, dojde k chybě a technické statistiky, můžete jednoduše provést všechny vaše `Page` dílčí třídy dědí hello `EngagementPage` třídy.

Tady je příklad toho, jak toodo to pro stránku vaší aplikace. Můžete provést hello samé pro všechny stránky vaší aplikace.

#### <a name="c-source-file"></a>C# zdrojový soubor
Upravit stránku `.xaml.cs` souboru:

* Přidat tooyour `using` příkazy:
  
      using Microsoft.Azure.Engagement;
* Nahraďte `Page` s `EngagementPage`:

**Bez Engagement:**

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**S Engagement:**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!IMPORTANT]
> Pokud stránka přepíše hello `OnNavigatedTo` metoda, že toocall být `base.OnNavigatedTo(e)`. V opačném hello aktivita nebude hlášena (hello `EngagementPage` volání `StartActivity` uvnitř jeho `OnNavigatedTo` metoda).
> 
> 

#### <a name="xaml-file"></a>Souboru XAML
Upravit stránku `.xaml` souboru:

* Přidejte tooyour deklarací oborů názvů:
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* Nahraďte `Page` s `engagement:EngagementPage`:

**Bez Engagement:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**S Engagement:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-hello-default-behaviour"></a>Přepsat výchozí chování hello
Ve výchozím nastavení je název třídy hello stránku hello uvedená jako název aktivity hello a žádné navíc. Pokud třída hello používá příponu "Stránka" hello, Engagement také odebere.

Pokud chcete toooverride hello výchozí chování pro název hello, jednoduše přidejte tento kód tooyour:

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Pokud chcete, tooreport některé další – informace s vaší aktivitou, můžete přidat tento kód tooyour:

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Tyto metody jsou volat v rámci hello `OnNavigatedTo` metoda stránky.

### <a name="alternate-method-call-startactivity-manually"></a>Alternativní metoda: volání `StartActivity()` ručně
Pokud nemůžete nebo nechcete, aby toooverload vaše `Page` třídy, můžete místo toho spustit vaše aktivity voláním `EngagementAgent` metody přímo.

Doporučujeme, abyste toocall `StartActivity` uvnitř vaší `OnNavigatedTo` metoda stránky.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> Zajistěte, aby že správně ukončete svou relaci.
> 
> Hello sady Windows Universal SDK automaticky volá hello `EndActivity` metoda při zavření aplikace hello. Z toho důvodu je **vysoce** doporučená toocall hello `StartActivity` metoda vždy, když aktivita hello hello uživatele změnit a příliš**nikdy** volání hello `EndActivity` metoda, tato metoda odesílá tooEngagement Server, že má aktuální uživatel aplikace hello ponechejte, budou ovlivňuje všechny protokoly aplikací.
> 
> 

## <a name="advanced-reporting"></a>Rozšířená tvorba sestav
Volitelně můžete tooreport aplikace konkrétní události, chyb a úlohy, toodo tak, použijte hello další metody nalezeny v hello `EngagementAgent` třídy. Hello Engagement API umožňuje toouse všechny rozšířené možnosti Engagement.

Další informace najdete v tématu [jak toouse hello advanced označování rozhraní API v aplikaci Windows Universal Mobile Engagement](mobile-engagement-windows-store-use-engagement-api.md).

## <a name="advanced-configuration"></a>Pokročilá konfigurace
### <a name="disable-automatic-crash-reporting"></a>Zakázat automatické hlášení chyb
Můžete vypnout automatické hlášení hello funkci Engagement generování sestav. Pak když dojde k neošetřené výjimce, Engagement nebude provádět žádné kroky.

> [!WARNING]
> Pokud máte v plánu toodisable tuto funkci, mějte na paměti, když dojde k nezpracované chybě ve vaší aplikaci, nepošle Engagement hello havárií **a** nebude zavřete hello relace a úlohy.
> 
> 

Automatické hlášení toodisable vytváření sestav, stačí upravit konfiguraci v závislosti na způsob hello deklarujete objekt:

#### <a name="from-engagementconfigurationxml-file"></a>Z `EngagementConfiguration.xml` souboru
Nastavit sestavy zhroutí příliš`false` mezi `<reportCrash>` a `</reportCrash>` značky.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Z `EngagementConfiguration` objektu v době běhu
Nastavit toofalse havárií sestavy pomocí EngagementConfiguration objektu.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Režim shluku
Ve výchozím nastavení sestavy služby zapojení hello protokolů v reálném čase. Pokud vaše aplikace hlásí protokoly velmi často, je lepší toobuffer hello protokoly a tooreport je všechny najednou na pravidelné základní časové (tomu se říká hello "shluků režim").

toodo tedy volat metodu hello:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Hello argument je hodnota **milisekundách**. Kdykoli Pokud chcete protokolování v reálném čase hello tooreactivate, stačí zavoláte metoda hello bez zadání parametru nebo s hodnotou hello 0.

Hello shluků režimu mírně zvýšit hello baterie životnosti ale má vliv na hello monitorování Engagement: všechny doba trvání relace a úlohy se prahová hodnota shluků zaokrouhlené toohello (tedy relací a úlohy kratší, než je prahová hodnota shluků hello nemusí být zobrazeny). Je doporučeno toouse práh shluků než 30000 (30s). Máte toobe vědět, které uložené protokoly jsou omezené too300 položky. Pokud odesílání je příliš dlouhý. může dojít ke ztrátě některých protokoly.

> [!WARNING]
> Prahová hodnota shluků Hello nelze konfigurovat tooa období menší než hodnotami 1. Pokud se pokusíte toodo tak, hello SDK bude zobrazit trasování s chybou hello a bude automaticky toohello výchozí hodnotu, tedy 0s obnovit. Tím se aktivuje hello SDK tooreport hello protokolů v reálném čase.
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview

