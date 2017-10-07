---
title: "aaaSet až analýzy webové aplikace pro technologii ASP.NET pomocí služby Azure Application Insights | Microsoft Docs"
description: "Nakonfigurujte výkon, dostupnost a analýzy využití pro váš web ASP.NET hostovaný místně nebo v Azure."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: 61a3cdce68da48bfb9450b1d296acc1535f50a38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>Nastavení Application Insights pro web ASP.NET

Tento postup nakonfiguruje vaše ASP.NET webové aplikace toosend telemetrie toohello [Azure Application Insights](app-insights-overview.md) služby. Funguje pro aplikace ASP.NET, které jsou hostované v serveru služby IIS nebo v cloudu hello. Získejte grafy a účinný dotazovací jazyk, který vám pomůže pochopit hello výkonu vaší aplikace a jak uživatelé používají, plus Automatické upozornění na selhání nebo problémy s výkonem. Celá řada vývojářů najít tyto funkce skvělé jak jsou, ale můžete také rozšířit a přizpůsobit telemetrie hello, pokud je potřeba.

Nastavení je otázkou několika kliknutí v sadě Visual Studio. Máte hello možnost tooavoid poplatky omezením hello svazku telemetrie. To vám umožní tooexperiment a ladění nebo toomonitor lokality s není mnoha uživateli. Pokud se rozhodnete chcete toogo dopředu a sledovat vaše pracoviště, je snadné tooraise omezení hello později.

## <a name="before-you-start"></a>Než začnete
Budete potřebovat:

* Visual Studio 2013 s aktualizací Update 3 nebo vyšší. Později je lepší.
* Předplatné příliš[Microsoft Azure](http://azure.com). Pokud váš tým nebo společnost má předplatné Azure, vlastník hello vás může přidat tooit, pomocí vaší [účtu Microsoft](http://live.com).

Pokud vás zajímá, existují toolook alternativní témata na:

* [Instrumentace webové aplikace za běhu](app-insights-monitor-performance-live-website-now.md)
* [Azure Cloud Services](app-insights-cloudservices.md)

## <a name="ide"></a>Krok 1: Přidání hello Application Insights SDK

Klikněte pravým tlačítkem myši na projekt webové aplikace v Průzkumníku řešení a vyberte postupně **Přidat** > **Telemetrie Application Insights...** nebo **Konfigurovat Application Insights**.

![Snímek obrazovky Průzkumníka řešení se zvýrazněnou možností Přidat telemetrii Application Insights](./media/app-insights-asp-net/appinsights-03-addExisting.png)

(V sadě Visual Studio 2015 se také možnost tooadd Application Insights v dialogovém okně Nový projekt hello.)

Pokračujte – stránka konfigurace toohello Application Insights:

![Snímek obrazovky stránky registrace vaší aplikace v Application Insights](./media/app-insights-asp-net/visual-studio-register-dialog.png)

**a.** Vyberte hello účet a předplatné, že používáte tooaccess Azure.

**b.** Vyberte prostředek hello v Azure, kam chcete toosee hello data z vaší aplikace. Obvyklý postup:

* Použijte [jeden prostředek pro různé komponenty](app-insights-monitor-multi-role-apps.md) jedné aplikace. 
* Vytvořte samostatné prostředky pro nesouvisející aplikace.
 
Pokud chcete tooset hello prostředků skupiny nebo hello umístění, kde jsou uložené vaše data, klikněte na tlačítko **nakonfigurovat nastavení**. Skupiny prostředků jsou použité toocontrol toodata přístup. Například pokud máte několik aplikací, které tvoří část hello, stejný systém, můžete ukládat svoje data Application Insights do hello stejnou skupinu prostředků.

**c.** Nastavte cap při hello volné dat svazku limitu tooavoid poplatky. Application Insights je uvolněte tooa určité svazku telemetrie. Po vytvoření hello prostředků, můžete změnit výběr portálu hello otevřením **funkce + ceny** > **Správa svazku dat** > **denní svazek cap**.

**d.** Klikněte na tlačítko **zaregistrovat** toogo dopředu a konfigurace Application Insights pro webové aplikace. Telemetrie zašle toohello [portál Azure](https://portal.azure.com), během ladění a po publikování aplikace.

**e.** Pokud nechcete, aby toosend telemetrie toohello portál při ladění, stačí přidat aplikaci tooyour hello Application Insights SDK ale není nakonfigurovat prostředek hello portálu. Při ladění, bude se mít toosee telemetrie v sadě Visual Studio. Později, můžete se vrátit toothis stránku konfigurace, nebo může počkejte po nasazení aplikace a [přepínač na telemetrie v době běhu](app-insights-monitor-performance-live-website-now.md).


## <a name="run"></a>Krok 2: Spuštění aplikace
Spusťte aplikaci pomocí F5. Otevřete různé stránky toogenerate nějaké telemetrie.

V sadě Visual Studio zobrazí počet hello události, které byly zaprotokolovány.

![Snímek obrazovky sady Visual Studio. Zobrazí tlačítko Application Insights Hello během ladění.](./media/app-insights-asp-net/54.png)

## <a name="step-3-see-your-telemetry"></a>Krok 3: Zobrazení vašich telemetrických dat
Můžete zobrazit telemetrii v sadě Visual Studio nebo na webovém portálu Application Insights hello. Hledání telemetrie v sadě Visual Studio toohelp ladění aplikace. Sledování výkonu a využití v hello webový portál, pokud je váš systém za provozu. 

### <a name="see-your-telemetry-in-visual-studio"></a>Zobrazení telemetrických dat v sadě Visual Studio

V sadě Visual Studio otevřete okno Application Insights hello. Buď klikněte na tlačítko hello **Application Insights** tlačítko, nebo klikněte pravým tlačítkem na projekt v Průzkumníku řešení, vyberte **Application Insights**a pak klikněte na tlačítko **vyhledávání Live Telemetrie**.

V okně hledání Visual Studio Application Insights hello najdete v části hello **Data z relaci ladění** zobrazení pro telemetrii vygenerovanou hello vaší aplikace na straně serveru. Experimentujte s filtry hello a klikněte na všechny události toosee podrobněji.

![Snímek obrazovky hello Data z relaci ladění zobrazit v okně hello Application Insights.](./media/app-insights-asp-net/55.png)

> [!NOTE]
> Pokud se nezobrazí žádná data, zajistěte, aby hello časový rozsah je správný a klikněte na ikonu hledání hello.

[Další informace týkající se nástrojů Application Insights v sadě Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a>Zobrazení telemetrických dat na webovém portálu

Můžete také zjistit telemetrii na webovém portálu Application Insights hello (Pokud jste vybrali pouze hello tooinstall SDK). Hello portál obsahuje více grafů, analytických nástrojů a zobrazení mezi komponenty než Visual Studio. portál Hello také nabízí výstrahy.

Otevřete prostředek Application Insights. Buď přihlásit toohello [portál Azure](https://portal.azure.com/) najít ho existuje, nebo klikněte pravým tlačítkem na projekt hello v sadě Visual Studio a nechat ji provést uživatele.

![Snímek obrazovky aplikace Visual Studio, znázorňující, jak tooopen hello portál Application Insights](./media/app-insights-asp-net/appinsights-04-openPortal.png)

> [!NOTE]
> Pokud dojde k chybě přístup: máte více než jednu sadu přihlašovacích údajů Microsoft a jsou jste přihlášeni s použitím hello nesprávnou sadu? Hello portálu Odhlásit se a znovu přihlásit.

Hello portál ho otevře v zobrazení hello telemetrie z vaší aplikace.

![Snímek obrazovky stránky s přehledem Application Insights](./media/app-insights-asp-net/66.png)

Hello portálu klikněte na všechny dlaždice nebo graf toosee podrobněji.

[Další informace o používání Application Insights na portálu Azure hello](app-insights-dashboards.md).

## <a name="step-4-publish-your-app"></a>Krok 4: Publikování aplikace
Publikování aplikací serveru IIS tooyour nebo tooAzure. Kukátko [živý datový proud metriky](app-insights-metrics-explorer.md#live-metrics-stream) toomake se všechno běží bez problémů.

Telemetrie vytvoří hello portálu Application Insights, kde můžete monitorovat metriky, hledání telemetrie a nastavit [řídicí panely](app-insights-dashboards.md). Můžete také použít hello výkonné [analýzy protokolů dotazu jazyka](https://docs.loganalytics.io/) tooanalyze využití a výkonu nebo toofind konkrétní události.

Můžete také pokračovat tooanalyze telemetrie v [Visual Studio](app-insights-visual-studio.md), pomocí nástrojů, jako je vyhledávání diagnostiky a [trendy](app-insights-visual-studio-trends.md).

> [!NOTE]
> Pokud vaše aplikace odesílá dost telemetrických dat tooapproach hello [omezení](app-insights-pricing.md#limits-summary), automatické [vzorkování](app-insights-sampling.md) přepne. Vzorkování snižuje množství hello telemetrická data odesílaná z vaší aplikace, při zachování korelační data k diagnostickým účelům.
>
>

## <a name="land"></a> Nyní je vše připraveno.

Blahopřejeme! Nainstalovaný balíček Application Insights hello ve vaší aplikaci a nakonfigurovat službu Application Insights toohello telemetrie toosend v Azure.

![Diagram přesunu telemetrie](./media/app-insights-asp-net/01-scheme.png)

je identifikována Hello prostředků Azure, která přijímá telemetrii aplikace *klíč instrumentace*. Tento klíč najdete v souboru souboru ApplicationInsights.config hello.


## <a name="upgrade-toofuture-sdk-versions"></a>Upgrade verze sady SDK toofuture
tooupgrade tooa [novou verzi sady hello SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), otevřete hello **Správce balíčků NuGet** znovu a filtr na nainstalované balíčky. Vyberte **Microsoft.ApplicationInsights.Web** a zvolte **Upgradovat**.

Pokud jste provedli jakékoli úpravy tooApplicationInsights.config, uložte jeho kopii před upgradem. Pak sloučit změny do nové verze hello.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Další kroky

### <a name="more-telemetry"></a>Další telemetrická data

* **[Údaje o prohlížečích a o načítání stránek](app-insights-javascript.md)** – Vložte do svých webových stránek fragment kódu.
* **[Dosažení podrobnějšího monitorování závislostí a výjimek](app-insights-monitor-performance-live-website-now.md)** – Nainstalujte si na server Monitorování stavu.
* **[Kód vlastní události](app-insights-api-custom-events-metrics.md)**  toocount, čas nebo měření akce uživatele.
* **[Získání dat protokolu](app-insights-asp-net-trace-logs.md)** – Zjišťujte korelaci dat protokolu s telemetrickými daty.

### <a name="analysis"></a>Analýza

* **[Práce s Application Insights v sadě Visual Studio](app-insights-visual-studio.md)**<br/>Obsahuje informace o ladění pomocí telemetrie, diagnostické vyhledávání a procházení prostřednictvím toocode.
* **[Práce s Application Insights portál hello](app-insights-dashboards.md)**<br/> Zahrnuje informace o řídicích panelech, výkonných nástrojích pro diagnostiku a analýzy, výstrahách, aktivních mapách závislostí vaší aplikace a exportu telemetrie.
* **[Analýza](app-insights-analytics-tour.md)**  -hello účinný dotazovací jazyk.

### <a name="alerts"></a>Výstrahy

* [Testy dostupnosti](app-insights-monitor-web-app-availability.md): vytváření testů toomake, že je váš web viditelná na webu hello.
* [Inteligentní diagnostiky](app-insights-proactive-diagnostics.md): tyto testy se spouštějí automaticky, takže není nutné toodo cokoli tooset je nahoru. Upozorní vás, pokud má aplikace nezvykle velký podíl neúspěšných požadavků.
* [Metriky výstrahy](app-insights-alerts.md): nastavte tyto toowarn vám, zda metriky protne prahovou hodnotu. Upozornění můžete nastavit u vlastních metrik, které v aplikaci naprogramujete.

### <a name="automation"></a>Automation

* [Automatizace vytvoření prostředku Application Insights](app-insights-powershell.md)
