---
title: "aaaAzure Application Insights pro Windows server a pracovní role | Microsoft Docs"
description: "Ručně přidejte hello Application Insights SDK tooyour ASP.NET aplikace tooanalyze využití, dostupnosti a výkonu."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 106ba99b-b57a-43b8-8866-e02f626c8190
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: 64643ef637195d10f87fc6020a77169bca66c1f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manually-configure-application-insights-for-net-applications"></a>Ruční konfigurace služby Application Insights pro aplikace .NET

Můžete nakonfigurovat [Application Insights](app-insights-overview.md) toomonitor celou řadu aplikací nebo aplikační role, komponenty nebo mikroslužeb. Pro webové aplikace a služby nabízí Visual Studio [konfiguraci v jednom kroku](app-insights-asp-net.md). V případě jiných typů aplikací .NET, například u rolí back-end serveru nebo u desktopových aplikací, můžete nakonfigurovat službu Application Insights ručně.

![Příklady tabulek sledování výkonu](./media/app-insights-windows-services/10-perf.png)

#### <a name="before-you-start"></a>Než začnete

Budete potřebovat:

* Předplatné příliš[Microsoft Azure](http://azure.com). Pokud váš tým nebo společnost má předplatné Azure, vlastník hello vás může přidat tooit, pomocí vaší [účtu Microsoft](http://live.com).
* Visual Studio 2013 nebo novější.

## <a name="add"></a>1. Volba prostředku služby Application Insights

Hello prostředků je, kde je vaše data shromažďovat a zobrazí v hello portálu Azure. Jestli potřebujete toodecide toocreate novou, nebo sdílet některého ze stávajících.

### <a name="part-of-a-larger-app-use-existing-resource"></a>Součást větší aplikace – použití existujícího prostředku

Pokud vaše webová aplikace má několik komponenty, například front-endové webové aplikace a jeden nebo více back endové služby - pak by měl odeslání telemetrie z všechny součásti toohello hello stejného zdroje. Tím je povolit toobe zobrazí na jednu mapu aplikace a bylo možné tootrace žádost od tooanother jedna součást.

Ano, pokud jste již monitorování jiných součástí této aplikace, pak stačí použít hello stejné prostředků.

Otevření prostředku hello v hello [portál Azure](https://portal.azure.com/). 

### <a name="self-contained-app-create-a-new-resource"></a>Samostatná aplikace – vytvoření nového prostředku

Pokud je nové aplikace hello nesouvisejícími tooother aplikace, měl by mít svůj vlastní prostředek.

Přihlaste se toohello [portál Azure](https://portal.azure.com/)a vytvořte nový prostředek Application Insights. Vyberte jako typ aplikace hello ASP.NET.

![Klikněte na tlačítko Nový, Application Insights](./media/app-insights-windows-services/01-new-asp.png)

Hello volba typu aplikace nastaví výchozí obsah oken prostředků hello hello.

## <a name="2-copy-hello-instrumentation-key"></a>2. Zkopírujte hello klíč instrumentace
Hello klíč identifikuje prostředek hello. Nainstalujte ho brzy v hello SDK, v pořadí toodirect data toohello prostředku.

![Klikněte na tlačítko Vlastnosti, vyberte klíč hello a stiskněte ctrl + C](./media/app-insights-windows-services/02-props-asp.png)

## <a name="sdk"></a>3. Instalovat balíček Application Insights hello v aplikaci
Instalace a konfigurace hello Application Insights se liší v závislosti na platformě hello, kterou právě pracujete na balíčku. 

1. Ve Visual Studiu klikněte pravým tlačítkem na projekt a zvolte **Spravovat balíčky NuGet**.
   
    ![Klikněte pravým tlačítkem na projekt hello a vyberte spravovat balíčky Nuget](./media/app-insights-windows-services/03-nuget.png)
2. Instalovat balíček hello Application Insights pro aplikace pro Windows server, "Microsoft.ApplicationInsights.WindowsServer."
   
    ![Vyhledání Application Insights](./media/app-insights-windows-services/04-ai-nuget.png)
   
    *Kterou verzi?*

    Zkontrolujte **zahrnout předběžné verze** Pokud chcete tootry našich nejnovějších funkcí. Hello relevantní dokumenty nebo blogy Všimněte si, zda je nutné předprodejní verze.
    
    *Mohu použít jiné balíčky?*
   
    Ano. Zvolte "Microsoft.ApplicationInsights", pokud chcete pouze toouse hello rozhraní API toosend vlastní telemetrii. balíček aplikace systému Windows Server Hello zahrnuje hello API plus počet dalších balíčků, jako jsou kolekce čítačů výkonu a monitorování závislostí. 

### <a name="tooupgrade-toofuture-package-versions"></a>verze balíčku toofuture tooupgrade
Jsme vydání nové verze hello SDK z tootime čas.

tooupgrade tooa [novou verzi balíčku hello](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/), otevřete znovu Správce balíčků NuGet a filtrujte nainstalované balíčky. Vyberte **Microsoft.ApplicationInsights.WindowsServer** a zvolte **Upgradovat**.

Pokud jste provedli jakékoli úpravy tooApplicationInsights.config, uložte jeho kopii před upgradem a následně slučte změny do nové verze hello.

## <a name="4-send-telemetry"></a>4. Odeslání telemetrie
**Pokud jste nainstalovali pouze hello rozhraní API balíčku:**

* Nastavte klíč instrumentace hello v kódu, například `main()`: 
  
    `TelemetryConfiguration.Active.InstrumentationKey = "`*váš klíč*`";` 
* [Psát vlastní telemetrii pomocí rozhraní API hello](app-insights-api-custom-events-metrics.md#ikey).

**Pokud jste nainstalovali další balíčky Application Insights** , pokud dáváte přednost, můžete klíč instrumentace hello .config souboru tooset hello:

* Upravte soubor ApplicationInsights.config (který byl přidán hello nainstalovat NuGet). Vložte tuto položku těsně před uzavírací značka hello:
  
    `<InstrumentationKey>`*klíč instrumentace hello jste zkopírovali*`</InstrumentationKey>`
* Ujistěte se, že jsou příliš nastavena hello vlastnosti souboru ApplicationInsights.config v Průzkumníku řešení**akce sestavení = obsah, kopie tooOutput Directory = kopie**.

Je užitečné tooset klíč instrumentace hello v kódu, pokud chcete příliš[přepínač hello klíč pro konfigurace různých sestavení](app-insights-separate-resources.md). Pokud jste nastavili hello klíč v kódu, nemáte tooset v hello `.config` souboru.

## <a name="run"></a> Spuštění projektu
Použití hello **F5** toorun aplikaci a vyzkoušejte ji: Otevřete různé stránky toogenerate nějaké telemetrie.

V sadě Visual Studio zobrazí počet hello události, které byly odeslány.

![Počet událostí v sadě Visual Studio](./media/app-insights-windows-services/appinsights-09eventcount.png)

## <a name="monitor"></a> Zobrazení telemetrických dat
Vrátí toohello [portál Azure](https://portal.azure.com/) a procházet tooyour prostředek Application Insights.

Vyhledejte data v tabulkách přehled hello. Na první pohled uvidíte pouze jeden nebo dva body. Například:

![Klikněte na tlačítko prostřednictvím toomore dat](./media/app-insights-windows-services/12-first-perf.png)

Klikněte na tlačítko prostřednictvím jakékoli grafu toosee podrobnější metriky. [Další informace o metrikách.](app-insights-web-monitor-performance.md)

### <a name="no-data"></a>Žádná data?
* Pomocí aplikace hello, otevřete různé stránky tak, aby ji k vygenerování nějaké telemetrie.
* Otevřete hello [vyhledávání](app-insights-diagnostic-search.md) dlaždici toosee jednotlivé události. Někdy trvá událostem trochu déle, tooget skrz kanály metriky hello.
* Počkejte několik sekund a klikněte na tlačítko **Aktualizovat**. Pravidelně grafy sami obnovit, ale můžete je aktualizovat ručně pokud čekáte pro některé data tooshow.
* Viz [Poradce při potížích](app-insights-troubleshoot-faq.md).

## <a name="publish-your-app"></a>Publikování aplikace
Teď nasadit aplikačního serveru tooyour nebo shromažďování dat tooAzure a sledujte hello.

![Pomocí sady Visual Studio toopublish vaší aplikace](./media/app-insights-windows-services/15-publish.png)

Při spuštění v režimu ladění prochází telemetrie skrz kanálu hello, takže byste měli vidět zobrazení dat během sekund. Při nasazení aplikace v rámci konfigurace verze se data hromadí pomaleji.

### <a name="no-data-after-you-publish-tooyour-server"></a>Žádná data po publikování tooyour serveru?
Otevřete v bráně firewall serveru porty pro odchozí provoz. V tématu [tuto stránku](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses) hello seznam požadované adresy 

### <a name="trouble-on-your-build-server"></a>Potíže na vašem serveru sestavení?
Podívejte se na [tuto položku Řešení potíží](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

> [!NOTE]
> Pokud vaše aplikace generuje mnoho telemetrie, hello modul adaptivního vzorkování automaticky sníží hello svazek, který je odeslán toohello portál odesláním pouze reprezentativní části události. Ale události, které jsou související toohello stejném požadavku bude vybrán nebo nevybrány jako skupina, takže mohou procházet mezi souvisejícími událostmi. 
> [Další informace o vzorkování](app-insights-sampling.md).
> 
> 

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Další kroky
* [Přidat další telemetrie](app-insights-asp-net-more.md) tooget hello úplné 360 stupňové zobrazení vaší aplikace.

