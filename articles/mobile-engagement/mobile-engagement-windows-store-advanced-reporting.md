---
title: "aaaWindows Universal pokročilé vytváření sestav s MobileApps zapojení"
description: "Jak tooIntegrate Azure Mobile Engagement s univerzálních aplikací pro Windows"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: ea5030bf-73ac-49b7-bc3e-c25fc10e945a
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 20968f238ef7ae9dc0b8bb6dac3fb8bdb9bc3a10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-reporting-with-hello-windows-universal-apps-engagement-sdk"></a>Pokročilé vytváření sestav s hello Engagement SDK univerzální aplikace Windows
> [!div class="op_single_selector"]
> * [Univerzální platforma Windows](mobile-engagement-windows-store-advanced-reporting.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-reporting.md)
> 
> 

Toto téma popisuje další scénáře vytváření sestav v aplikaci univerzální pro Windows. Mezi tyto scénáře patří možnosti, které můžete tooapply toohello aplikace vytvořená v hello [Začínáme](mobile-engagement-windows-store-dotnet-get-started.md) kurzu.

## <a name="prerequisites"></a>Požadavky
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Před zahájením tohoto kurzu, musíte nejdřív dokončit hello [Začínáme](mobile-engagement-windows-store-dotnet-get-started.md) kurz, který je úmyslně přímé a jednoduché. Tento kurz se zaměřuje na další možnosti, které můžete vybrat z.

## <a name="specifying-engagement-configuration-at-runtime"></a>Určení konfigurace engagement za běhu
Konfigurace Engagement Hello je centralizovaná v hello `Resources\EngagementConfiguration.xml` souboru projektu, který je tam, kde byl zadán v hello [Začínáme](mobile-engagement-windows-store-dotnet-get-started.md) tématu.

Ale můžete je také zadat v době běhu: můžete volat hello následující metody před inicializaci agenta hello Engagement:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set hello Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Doporučená metoda: přetížení vaší `Page` třídy
vytváření sestav hello tooactivate všechny protokoly hello vyžadují zapojení toocompute uživatelů, relací, aktivity, dojde k chybě a technické statistiky, ujistěte se, všechna vaše `Page` dílčí třídy dědí hello `EngagementPage` třídy.

Tady je příklad pro stránku vaší aplikace. Můžete provést hello samé pro všechny stránky vaší aplikace.

### <a name="c-source-file"></a>C# zdrojový soubor
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
> Pokud stránka přepíše hello `OnNavigatedTo` metoda, že toocall být `base.OnNavigatedTo(e)`. Jinak, není hlášena hello aktivity (hello `EngagementPage` volání `StartActivity` uvnitř jeho `OnNavigatedTo` metoda).
> 
> 

### <a name="xaml-file"></a>Souboru XAML
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

### <a name="override-hello-default-behaviour"></a>Přepsat výchozí chování hello
Ve výchozím nastavení je název třídy hello stránku hello uvedená jako název aktivity hello a žádné navíc. Pokud třída hello používá příponu "Stránka" hello, Engagement jeho odstranění.

toooverride hello výchozí chování pro název hello přidejte tento kód:

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

tooreport doplňující informace s vaší aktivitou, přidejte tento kód:

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Tyto metody jsou volat v rámci hello `OnNavigatedTo` metoda stránky.

### <a name="alternate-method-call-startactivity-manually"></a>Alternativní metoda: volání `StartActivity()` ručně
Pokud nemůžete nebo nechcete, aby toooverload vaše `Page` třídy, můžete místo toho spustit vaše aktivity voláním `EngagementAgent` metody přímo.

Doporučujeme, abyste volání `StartActivity` uvnitř vaší `OnNavigatedTo` metoda stránky.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> Zajistěte, aby že správně ukončete svou relaci.
> 
> Hello sady Windows Universal SDK automaticky volá hello `EndActivity` metoda při zavření aplikace hello. Z toho důvodu je **vysoce** doporučená toocall hello `StartActivity` metoda vždy, když aktivita hello hello uživatele změnit a příliš**nikdy** volání hello `EndActivity` metoda. Tato metoda hello Engagement server oznámení, že tento aktuální uživatel hello opustil hello aplikaci, které ovlivní všechny protokoly aplikací.
> 
> 

## <a name="advanced-reporting"></a>Rozšířená tvorba sestav
Volitelně můžete tooreport události specifické pro aplikaci, chyb a úlohy, toodo tak, použijte hello další metody nalezeny v hello `EngagementAgent` třídy. Hello Engagement API umožňuje použití pokročilých funkcí pro všechny zapojení.

Další informace najdete v tématu [jak toouse hello advanced označování rozhraní API v aplikaci Windows Universal Mobile Engagement](mobile-engagement-windows-store-use-engagement-api.md).

