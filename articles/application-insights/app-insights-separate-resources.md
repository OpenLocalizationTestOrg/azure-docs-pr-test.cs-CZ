---
title: "aaaSeparating telemetrie z vývoj, testování a verze ve službě Azure Application Insights | Microsoft Docs"
description: "Přímé telemetrie toodifferent prostředky pro vývoj, testování a provozním razítka."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 578e30f0-31ed-4f39-baa8-01b4c2f310c9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: a294c8c70f46d7c29b460461c3494c83e13a0cbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="separating-telemetry-from-development-test-and-production"></a>Oddělení telemetrie z vývoj, testování a provozním

Při vývoji hello další verze webové aplikace, které nechcete toomix až hello [Application Insights](app-insights-overview.md) telemetrie z hello nové verze a hello již vydané verze. tooavoid nedorozuměním odesílání telemetrie hello z různých vývoj zpracuje tooseparate prostředky Application Insights, s klíči samostatné instrumentace (ikeys). toomake je snazší klíč instrumentace hello toochange jako verze přesune z tooanother v jediném kroku, může být užitečné tooset hello ikey v kódu místo v konfiguračním souboru hello. 

(Pokud je váš systém cloudové služby Azure je [jinou metodu nastavení samostatné ikeys](app-insights-cloudservices.md).)

## <a name="about-resources-and-instrumentation-keys"></a>O prostředcích a klíčů instrumentace

Když jste nastavili Application Insights monitorování pro webovou aplikaci, můžete vytvořit Application Insights *prostředků* v Microsoft Azure. Otevřete tento prostředek v hello portálu Azure v pořadí toosee a analyzovat hello telemetrii získanou z vašich aplikací. je identifikována Hello prostředků *klíč instrumentace* (ikey). Při instalaci hello Application Insights balíček toomonitor vaší aplikace, můžete nakonfigurovat ji hello klíč instrumentace, tak, že ví, kde toosend hello telemetrie.

Obvykle zvolíte toouse samostatné prostředky nebo jeden sdílený prostředek v různých scénářích:

* Jiné, nezávislé aplikace – použít samostatné prostředků a ikey pro každou aplikaci.
* Více součástí nebo role jeden obchodní aplikace – použijte [jeden sdílený prostředek](app-insights-monitor-multi-role-apps.md) pro všechny součásti aplikace hello. Telemetrická data lze filtrovat a segmentované podle vlastnosti cloud_RoleName hello.
* Vývoj, testování a verze – použijte samostatné prostředků a ikey pro verze systému hello v 'razítka, nebo fáze produkce.
* A | Testování B – použít jeden zdroj. Vytvořte TelemetryInitializer tooadd telemetrie toohello vlastnost, která identifikuje hello variant.


## <a name="dynamic-ikey"></a>Klíč dynamické instrumentace

toomake, které je snazší toochange hello ikey jako kód hello přesune mezi fáze produkce, nastavit kód místo v konfiguračním souboru hello.

V metodě inicializace, jako jsou například souboru global.aspx.cs v službě ASP.NET nastavit hello:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

V tomto příkladu jsou hello ikeys pro různé prostředky hello umístěny v různých verzích hello soubor webové konfigurace. Prohazuje hello soubor webové konfigurace – což lze provést v rámci skriptu verze hello - bude Prohodit hello cílový prostředek.

### <a name="web-pages"></a>Webové stránky
Hello iKey používá taky na webových stránkách vaší aplikace, v hello [skript, který jste získali v okně rychlý start hello](app-insights-javascript.md). Místo kódování je oznámena do skriptu hello, vygenerujte ho z stav serveru hello. Například v aplikaci ASP.NET:

*JavaScript ve Razor*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a>Vytvořit další prostředky Application Insights
telemetrie tooseparate pro součásti jiné aplikace, nebo pro jiný razítky (dev/testovací/produkční) hello stejné součásti a potom je budete mít toocreate nový prostředek Application Insights.

V hello [portal.azure.com](https://portal.azure.com), přidejte prostředek Application Insights:

![Klikněte na tlačítko Nový, Application Insights](./media/app-insights-separate-resources/01-new.png)

* **Typ aplikace** ovlivňuje, co se zobrazí v okně Přehled hello a hello vlastnosti, které jsou k dispozici v [metriky explorer](app-insights-metrics-explorer.md). Pokud nevidíte vašeho typu aplikace, vyberte jeden z typů webových hello pro webové stránky.
* **Skupina prostředků** je užitečný pro správu vlastnosti, například [řízení přístupu](app-insights-resources-roles-access-control.md). Pro vývoj, testování a provozním můžete použít samostatné skupiny prostředků.
* **Předplatné** je váš účet platebních v Azure.
* **Umístění** je, kde společnost Microsoft uchovávat data. Aktuálně jej nelze změnit. 
* **Přidat toodashboard** vloží dlaždici rychlý přístup pro prostředek na Azure domovské stránky. 

Vytvoření prostředku hello trvá několik sekund. Pokud se provádí, zobrazí se výstraha.

(Můžete napsat [skript prostředí PowerShell](app-insights-powershell-script-create-resource.md) toocreate prostředek automaticky.)

### <a name="getting-hello-instrumentation-key"></a>Získávání klíč instrumentace hello
klíč instrumentace Hello identifikuje hello prostředek, který jste vytvořili. 

![Klikněte na tlačítko Essentials, klikněte na tlačítko hello klíč instrumentace, CTRL + C](./media/app-insights-separate-resources/02-props.png)

Je třeba klíčů instrumentace hello z všechny prostředky toowhich hello vaše aplikace bude posílat data.

## <a name="filter-on-build-number"></a>Filtrovat podle čísla sestavení
Při publikování nové verze aplikace, budete muset toobe možné tooseparate hello telemetrie z jiné sestavení.

Vlastnost verze aplikace hello lze nastavit tak, aby můžete filtrovat [vyhledávání](app-insights-diagnostic-search.md) a [metriky explorer](app-insights-metrics-explorer.md) výsledky.

![Filtrování u vlastnosti](./media/app-insights-separate-resources/050-filter.png)

Existuje několik různých metod nastavení vlastnosti verze aplikace hello.

* Nastavte přímo:

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* Zabalení daného řádku v [telemetrie inicializátoru](app-insights-api-custom-events-metrics.md#defaults) tooensure, zda jsou všechny instance TelemetryClient konzistentně nastavena.
* [ASP.NET] Nastavit verzi hello v `BuildInfo.config`. v modulu web Hello vyzvedne, až bude hello verze z uzlu BuildLabel hello. Zahrnout tento soubor do projektu a zapamatovat si tooset hello kopie vždy vlastnost v Průzkumníku řešení.

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* [ASP.NET] Automaticky generovat BuildInfo.config v nástroji MSBuild. toodo, přidat pár řádků tooyour `.csproj` souboru:

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    Tím se vygeneruje soubor s názvem *Názevvašehoprojektu*. BuildInfo.config. hello proces publikování se přejmenuje tooBuildInfo.config.

    Popisek sestavení Hello obsahuje zástupný znak (AutoGen_...), když vytvoříte pomocí sady Visual Studio. Ale když vytvořené pomocí nástroje MSBuild, je naplněn hello správné číslo verze.

    čísla verzí nástroje MSBuild toogenerate tooallow, nastavit hello verzi jako `1.0.*` v AssemblyReference.cs

## <a name="version-and-release-tracking"></a>Sledování verzí a vydání
verze aplikace hello tootrack, zajistěte, aby `buildinfo.config` je generován váš proces Microsoft Build Engine. Do souboru .csproj přidejte:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Pokud má hello informace o sestavení, webový modul Application Insights hello automaticky přidá **verze aplikace** jako položka tooevery vlastnost telemetrie. Což vám umožní toofilter podle verze při provádění [diagnostických hledání](app-insights-diagnostic-search.md), nebo když jste [zkoumat metriky](app-insights-metrics-explorer.md).

Všimněte si však, že hello číslo verze sestavení je generováno pouze pomocí hello Microsoft Build Engine, ne podle vývojáře hello sestavení v sadě Visual Studio.

### <a name="release-annotations"></a>Poznámky k verzi
Pokud používáte Visual Studio Team Services, můžete [získat značku poznámky](app-insights-annotations.md) přidat tooyour grafy při každém vydání nové verze. Hello následující obrázek ukazuje, jak se zobrazí tato značka.

![Snímek obrazovky grafu s ukázkovou poznámkou k verzi](./media/app-insights-asp-net/release-annotation.png)
## <a name="next-steps"></a>Další kroky

* [Sdílené prostředky pro víc rolí](app-insights-monitor-multi-role-apps.md)
* [Vytvoření toodistinguish inicializátoru Telemetrie A | Variant B](app-insights-api-filtering-sampling.md#add-properties)
