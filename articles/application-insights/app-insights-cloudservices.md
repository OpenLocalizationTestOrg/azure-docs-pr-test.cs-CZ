---
title: aaaApplication Insights pro Azure Cloud Services | Microsoft Docs
description: "Efektivní sledování webových rolí a rolí pracovních procesů s využitím Application Insights"
services: application-insights
documentationcenter: 
keywords: WAD2AI, diagnostika Azure
author: CFreemanwa
manager: carmonm
editor: alancameronwills
ms.assetid: 5c7a5b34-329e-42b7-9330-9dcbb9ff1f88
ms.service: application-insights
ms.devlang: na
ms.tgt_pltfrm: ibiza
ms.topic: get-started-article
ms.workload: tbd
ms.date: 05/05/2017
ms.author: bwren
ms.openlocfilehash: 6956ce423eea1e2cf387bd98250bae32d9501ed0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-azure-cloud-services"></a>Application Insights pro Azure Cloud Services
U [aplikací cloudových služeb Microsoft Azure](https://azure.microsoft.com/services/cloud-services/) lze pomocí služby [Application Insights][start] sledovat dostupnost, výkon, chyby a využití díky kombinování dat ze sad SDK služby Application Insights a dat [diagnostiky Azure](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/azure-diagnostics) z vašich cloudových služeb. S hello zpětnou vazbu, které máte o hello výkon a efektivitu aplikace v rámci hello divoký můžete provést informované volby o hello směr hello návrhu v každé životního cyklu.

![Příklad](./media/app-insights-cloudservices/sample.png)

## <a name="before-you-start"></a>Než začnete
Budete potřebovat:

* Předplatné [Microsoft Azure](http://azure.com). Přihlaste se pomocí účtu Microsoft, který můžete mít zřízen pro Windows, XBox Live nebo jiné cloudové služby Microsoftu. 
* Nástroje Microsoft Azure 2.9 nebo novější
* Developer Analytics Tools 7.10 nebo novější

## <a name="quick-start"></a>Rychlý start
Hello toomonitor nejrychlejší a nejjednodušší způsob, cloudové služby s nástrojem Application Insights je toochoose, která možnost při publikování tooAzure vaší služby.

![Příklad](./media/app-insights-cloudservices/azure-cloud-application-insights.png)

Tato možnost instruments aplikace za běhu, která poskytuje všechny telemetrická hello budete potřebovat toomonitor požadavky, výjimky a závislosti ve vaší webové role a také výkonu čítače z vaší rolí pracovního procesu. Všechny diagnostické trasování, vygeneruje aplikace jsou odesílány také tooApplication statistiky.

Pokud to je všechno, co potřebujete, jste hotovi! Dalšími kroky jsou [zobrazení metrik z aplikace](app-insights-metrics-explorer.md), [zadávání dotazů na data pomocí Analytics](app-insights-analytics.md) a případně i nastavení [řídicího panelu](app-insights-dashboards.md). Můžete chtít tooset až [testy dostupnosti](app-insights-monitor-web-app-availability.md) a [přidat kód tooyour webové stránky](app-insights-javascript.md) toomonitor výkonu v prohlížeči hello.

Můžete ovšem využívat i další možnosti:

* Odesílání dat z různých součástí a konfigurace sestavení tooseparate prostředky.
* Přidání vlastní telemetrie ze své aplikace.

Pokud tyto možnosti jsou tooyou zájmu, přečtěte si.

## <a name="sample-application-instrumented-with-application-insights"></a>Ukázková aplikace používaná s Application Insights
Podívejte se na to [ukázkové aplikace](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) Application Insights ve kterém se přidá tooa cloudové služby s dvě role pracovního procesu, které jsou hostované v Azure. 

Co následuje se dozvíte, jak tooadapt cloudové služby projektu v hello stejným způsobem.

## <a name="plan-resources-and-resource-groups"></a>Plánování prostředků a skupin prostředků
Hello telemetrie z vaší aplikace uložené, analyzovat a zobrazit v Azure prostředek typu Application Insights. 

Každý prostředek patří tooa skupinu prostředků. Skupiny prostředků se používají pro správu nákladů pro udělení přístupu členů tooteam a toodeploy aktualizace v jediné koordinované transakce. Například může [zápisu skriptu toodeploy](../azure-resource-manager/resource-group-template-deploy.md) cloudové služby Azure a jeho sledování prostředků všechny v rámci jedné operace Application Insights.

### <a name="resources-for-components"></a>Prostředky pro komponenty
Hello doporučoval schéma je toocreate samostatné prostředků pro jednotlivé součásti aplikace – tj, každou webovou roli a roli pracovního procesu. Můžete analyzovat jednotlivé komponenty samostatně, ale můžete vytvořit [řídicí panel](app-insights-dashboards.md) který spojuje klíče grafy hello z všechny součásti hello tak, aby můžete porovnat a monitorovat je společně. 

Alternativní schéma je toosend hello telemetrie z více než jednu roli toohello stejný prostředek, ale [přidat položku dimenze vlastnost tooeach telemetrie](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer) identifikující jeho zdrojovou roli. V tomto schématu metriky grafů, jako je například výjimky normálně zobrazit agregaci hello počty z hello různé role, ale můžete rozdělit hello grafu pomocí hello identifikátor role v případě potřeby. Hledání můžete také filtrovat podle hello stejné dimenze. Díky této alternativní je trochu snazší tooview vše na hello stejný čas, ale může také vést toosome nejasnosti mezi rolemi hello.

Telemetrie prohlížeče je obvykle součástí hello stejného zdroje jako jeho webové serverové role.

Uveďte hello prostředky Application Insights pro různé součásti hello v jedné skupině prostředků. Díky tomu snadno toomanage je společně. 

### <a name="separating-development-test-and-production"></a>Oddělení vývoje, testování a provozu
Pokud vyvíjíte vlastních událostí pro vaše další funkce, když hello předchozí verze za provozu, budete chtít toosend hello vývoj telemetrie tooa samostatné prostředek Application Insights. V opačném případě bude pevný toofind telemetrie testovací mezi všechny hello provoz z hello živý web.

tooavoid této situaci se vytvořit samostatné prostředky pro každou konfiguraci sestavení nebo 'razítko' (vývoj, testovací, výroby,...) vašeho systému. Uveďte hello prostředky pro každou konfiguraci sestavení do skupiny samostatné prostředků. 

toosend hello telemetrie toohello odpovídající prostředky, které můžete nastavit hello Application Insights SDK tak, aby ho převezme jiný instrumentace klíč v závislosti na konfiguraci sestavení hello. 

## <a name="create-an-application-insights-resource-for-each-role"></a>Vytvoření prostředku Application Insights pro každou roli
Pokud jste se rozhodli toocreate samostatné prostředků pro každou roli - a případně samostatné nastavit pro každé sestavení konfigurace – pak je nejjednodušší toocreate je vše na portál Application Insights hello. (Pokud vytvoříte mnoho prostředků, můžete [automatizovat proces hello](app-insights-powershell.md).

1. V hello [portál Azure][portal], vytvořte nový prostředek Application Insights. Jako typ aplikace vyberte aplikaci ASP.NET. 

    ![Klikněte na tlačítko Nový, Application Insights](./media/app-insights-cloudservices/01-new.png)
2. Všimněte si, že každý prostředek je identifikován instrumentačním klíčem. Pokud chcete, aby toomanually to může být nutné později nastavte nebo zkontrolujte konfiguraci hello hello SDK.

    ![Klikněte na tlačítko Vlastnosti, vyberte klíč hello a stiskněte ctrl + C](./media/app-insights-cloudservices/02-props.png) 

## <a name="set-up-azure-diagnostics-for-each-role"></a>Nastavení diagnostiky Azure pro každou roli
Nastavte tuto možnost toomonitor vaší aplikace pomocí Application Insights. V případě webových rolí tento postup umožňuje monitorování, výstrahy a diagnostiku výkonu a také analýzu využití. Pro jiné role můžete vyhledat a monitorovat Azure diagnostics například restartování, čítače výkonu a tooSystem.Diagnostics.Trace volání. 

1. V Průzkumníku řešení Visual Studio klikněte v části &lt;YourCloudService&gt;, role, otevřete vlastnosti hello jednotlivých rolí.
2. V **konfigurace**, nastavte **odeslání diagnostiky dat tooApplication Insights** a vyberte hello odpovídající prostředek Application Insights, kterou jste vytvořili dříve.

Pokud jste se rozhodli toouse samostatné prostředek Application Insights pro každou konfiguraci sestavení, vyberte nejdřív hello konfigurace.

![V dialogovém okně Vlastnosti hello jednotlivých rolí Azure nakonfigurujte Application Insights](./media/app-insights-cloudservices/configure-azure-diagnostics.png)

Tato akce nemá vliv hello vložení klíče instrumentace Application Insights do hello souborů s názvem `ServiceConfiguration.*.cscfg`. ([Vzorový kód](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/AzureEmailService/ServiceConfiguration.Cloud.cscfg).)

Pokud chcete úroveň hello toovary diagnostické informace odesílané tooApplication statistiky, můžete tak učinit [úpravou hello `.cscfg` soubory přímo](app-insights-azure-diagnostics.md).

## <a name="sdk"></a>Nainstalujte hello SDK v každém projektu
Tato možnost přidá hello možnost tooadd vlastní obchodní telemetrie tooany roli, k analýze blíže se používá a provede aplikace.

V sadě Visual Studio nakonfigurujte hello Application Insights SDK pro každý projekt cloudové aplikace.

1. **Webové role**: klikněte pravým tlačítkem na projekt hello a zvolte **konfigurovat Application Insights** nebo **Přidat > telemetrie Application Insights**.

2. **Role pracovních procesů**: 
 * Klikněte pravým tlačítkem na projekt hello a vyberte **spravovat balíčky Nuget**.
 * Přidejte balíček [Application Insights pro servery Windows](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/).

    ![Vyhledání Application Insights](./media/app-insights-cloudservices/04-ai-nuget.png)

3. Nakonfigurujte hello SDK toosend data toohello prostředek Application Insights.

    Ve funkci spuštění vhodný nastavte klíč instrumentace hello z nastavení konfigurace hello v souboru .cscfg hello:
 
    ```C#
   
     TelemetryConfiguration.Active.InstrumentationKey = RoleEnvironment.GetConfigurationSettingValue("APPINSIGHTS_INSTRUMENTATIONKEY");
    ```
   
    Proveďte tento postup pro každou roli v aplikaci. Příklady hello:
   
   * [Webová role](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Global.asax.cs#L27)
   * [Role pracovního procesu](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L232)
   * [Pro webové stránky](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Views/Shared/_Layout.cshtml#L13) 
4. Sada hello souboru ApplicationInsights.config souboru toobe zkopírovat vždy toohello výstupního adresáře. 
   
    (V souboru .config hello, zobrazí se zprávy s dotazem, můžete tooplace hello klíč instrumentace existuje. Pro cloudové aplikace, je však lepší tooset ze souboru .cscfg hello. Tím se zajistí, že je tato role hello identifikovány správně hello portálu.)

#### <a name="run-and-publish-hello-app"></a>Spuštění a publikování aplikace hello
Spusťte aplikaci a přihlaste se k Azure. Prostředky Application Insights otevřete hello jste vytvořili, a zobrazí se v jednotlivých datových bodů [vyhledávání](app-insights-diagnostic-search.md), a agregovat data v [Explorer metrika](app-insights-metrics-explorer.md). 

Přidejte další telemetrie – najdete v následující části obsahují - hello a potom publikovat vaše tooget za provozu diagnostiky a použití zpětné vazby aplikace. 

#### <a name="no-data"></a>Žádná data?
* Otevřete hello [vyhledávání] [ diagnostic] dlaždici toosee jednotlivé události.
* Pomocí aplikace hello, otevřete různé stránky tak, aby ji k vygenerování nějaké telemetrie.
* Počkejte několik sekund a klikněte na možnost Aktualizovat.
* Další informace najdete v tématu [Poradce při potížích][qna].

## <a name="view-azure-diagnostic-events"></a>Zobrazení událostí diagnostiky Azure
Kde toofind hello [Azure Diagnostics](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/azure-diagnostics) informace ve službě Application Insights:

* Čítače výkonu se zobrazují jako vlastní metriky. 
* Protokoly událostí systému Windows se zobrazují jako trasování a vlastní události.
* Protokoly aplikací, protokoly trasování událostí pro Windows a veškeré protokoly infrastruktury diagnostiky se zobrazují jako trasování.

čítače výkonu toosee a počet událostí, otevřete [Průzkumníku metrik](app-insights-metrics-explorer.md) a přidejte nový graf:

![Diagnostická data Azure](./media/app-insights-cloudservices/23-wad.png)

Použití [vyhledávání](app-insights-diagnostic-search.md) nebo [Analytics dotazu](app-insights-analytics-tour.md) toosearch napříč různými protokoly poslal Azure Diagnostics sledování hello. Předpokládejme například, že máte k neošetřené výjimce, který chybu způsobil Role toocrash a recyklaci. Tyto informace se zobrazí v hello aplikace kanál z protokolu událostí systému Windows. Můžete použít vyhledávání toolook v hello Chyba protokolu událostí systému Windows a získat trasování hello úplné zásobníku pro výjimku hello. Který vám pomůže najít hello hlavní příčinu problému hello.

![Hledání v diagnostice Azure](./media/app-insights-cloudservices/25-wad.png)

## <a name="more-telemetry"></a>Další telemetrická data
Hello oddílech Zobrazit jak tooget další telemetrie z různých aspektů aplikace.

## <a name="track-requests-from-worker-roles"></a>Sledování požadavků z rolí pracovních procesů
Ve webové role hello požadavky modulu automaticky shromažďuje data o požadavcích HTTP. V tématu hello [ukázkové MVCWebRole](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole) příklady, jak můžete přepsat hello výchozí kolekci chování. 

Můžete zaznamenat hello výkonu volání tooworker rolí pomocí funkce sledování je v hello stejným způsobem jako požadavky HTTP. Ve službě Application Insights měří hello požadavek telemetrie typu s názvem pracovní straně serveru, který můžete vypršel časový limit a můžete nezávisle úspěch nebo neúspěch jednotka. Zatímco požadavky HTTP jsou automaticky zachycenou hello SDK, můžete vložit vlastní kód tootrack požadavky tooworker role.

V tématu požadavků instrumentovaného tooreport hello dva ukázkové pracovního procesu worker role: [WorkerRoleA](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleA) a [WorkerRoleB](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleB)

## <a name="exceptions"></a>Výjimky
V tématu [Monitorování výjimek v Application Insights](app-insights-asp-net-exceptions.md) najdete informace o tom, jak shromažďovat neošetřené výjimky z různých typů webových aplikací.

Hello ukázkové webové role má MVC5 a webovém rozhraní API 2 řadičů. Hello neošetřené výjimky z hello dva jsou zachytit pomocí hello následující obslužné rutiny:

* [AiHandleErrorAttribute](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiHandleErrorAttribute.cs) nastavená [zde](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/FilterConfig.cs#L12) pro kontrolery rozhraní MVC5
* [AiWebApiExceptionLogger](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiWebApiExceptionLogger.cs) nastavená [sem](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/WebApiConfig.cs#L25) pro kontrolery rozhraní Web API 2

Pro role pracovního procesu existují dva způsoby tootrack výjimky:

* TrackException(ex)
* Pokud jste přidali balíček NuGet hello Application Insights trasování naslouchací proces, můžete použít **System.Diagnostics.Trace** toolog výjimky. [Příklad kódu.](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L107)

## <a name="performance-counters"></a>Čítače výkonu
ve výchozím nastavení se shromažďují Hello následující čítače:

    * \Process(??APP_WIN32_PROC??)\% Processor Time
    * \Memory\Available Bytes
    * \.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec
    * \Process(??APP_WIN32_PROC??)\Private Bytes
    * \Process(??APP_WIN32_PROC??)\IO Data Bytes/sec
    * \Processor(_Total)\% Processor Time

Pro webové role se shromažďují i tyto čítače:

    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue

Další vlastní čítače výkonu nebo jiné čítače výkonu Windows můžete určit provedením úpravy souboru ApplicationInsights.config [jako v tomto příkladu](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/ApplicationInsights.config#L14).

  ![Čítače výkonu](./media/app-insights-cloudservices/OLfMo2f.png)

## <a name="correlated-telemetry-for-worker-roles"></a>Korelační telemetrická data pro role pracovních procesů
Bohaté diagnostiky prostředí, je při uvidíte, jaké vedených tooa se nezdařilo nebo vysokou latencí požadavku. S webové role hello SDK automaticky nastaví korelace mezi související telemetrii. Pro role pracovního procesu můžete vlastní telemetrii inicializátoru tooset společný atribut kontextu Operation.Id pro všechny tooachieve telemetrie hello to. To vám umožní toosee zda hello latenci nebo selhání problém byl způsobený z důvodu závislosti mezi tooa nebo kódu, na první pohled! 

Zde je uveden postup:

* Nastavte hello korelace Id do CallContext, jak je znázorněno [zde](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L36). V tomto případě používáme hello ID požadavku jako id korelace hello
* Přidáte vlastní implementaci TelemetryInitializer tooset hello Operation.Id toohello correlationId nastavit výše. Příklad je zde: [ItemCorrelationTelemetryInitializer](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/Telemetry/ItemCorrelationTelemetryInitializer.cs#L13)
* Přidáte vlastní telemetrii inicializátoru hello. Můžete to udělat v soubor ApplicationInsights.config hello nebo v kódu znázorněné [sem](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L233)

A to je vše! Práce s portálem Hello je již drátové sítě si toohelp že zobrazí všechny přidružené telemetrii na první pohled:

![Korelační telemetrická data](./media/app-insights-cloudservices/bHxuUhd.png)

## <a name="client-telemetry"></a>Telemetrická data klienta
[Přidat hello webové stránky JavaScript SDK tooyour] [ client] tooget založené na prohlížeči telemetrie například počtu zobrazení stránky, časů načtení stránky, výjimek skriptu a toolet psát vlastní telemetrii ve skriptech stránky.

## <a name="availability-tests"></a>Testy dostupnosti
[Nastavit testy webu] [ availability] toomake, že vaše aplikace zůstává aktivní a reagující.

## <a name="display-everything-together"></a>Zobrazení všeho najednou
tooget celkový přehled systému, můžete zahrnout hello klíč tabulek sledování společně na jednom [řídicí panel](app-insights-dashboards.md). Například může připnout hello požadavku a počet selhání jednotlivých rolí. 

Pokud váš systém využívá jiné služby Azure, například Stream Analytics, jsou zahrnuty i jejich grafy monitorování. 

Pokud máte mobilní aplikace klienta, vložte některé kód toosend vlastní události na operace klíče uživatele a vytvořit [HockeyApp most](app-insights-hockeyapp-bridge-app.md). Vytváření dotazů v nástroji [Analytics](app-insights-analytics.md) toodisplay hello počty událostí a připnete ji toohello řídicího panelu.

## <a name="example"></a>Příklad
[Příklad Hello](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) monitoruje službu, která obsahuje webovou roli a dvě role pracovního procesu.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Výjimka „metoda nebyla nalezena“ při spuštění v Azure Cloud Services
Vytvořili jste sestavení pro .NET 4.6? Verze 4.6 není v rolích Azure Cloud Services podporována automaticky. Před spuštěním aplikace [nainstalujte pro každou roli verzi 4.6](../cloud-services/cloud-services-dotnet-install-dotnet.md).

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Další kroky
* [Konfigurace odesílání Azure Diagnostics tooApplication statistiky](app-insights-azure-diagnostics.md)
* [Automatizace vytváření prostředků Application Insights](app-insights-powershell.md)
* [Automatizace diagnostiky Azure](app-insights-powershell-azure-diagnostics.md)
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample)

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[azure]: app-insights-azure.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[netlogs]: app-insights-asp-net-trace-logs.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md 
