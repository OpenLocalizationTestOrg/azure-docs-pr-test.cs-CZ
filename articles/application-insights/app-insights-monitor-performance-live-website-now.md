---
title: "aaaMonitor za provozu technologie ASP.NET webové aplikace pomocí služby Azure Application Insights | Microsoft Docs"
description: "Monitorování výkonu webu bez opětovného nasazení. Funguje s místně hostovanými webovými aplikacemi v ASP.NET, na virtuálních počítačích nebo v Azure."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: bwren
ms.openlocfilehash: 0d53f0a59974f40767fae681bafc4f358d1283a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="instrument-web-apps-at-runtime-with-application-insights"></a>Instrumentace webových aplikací za běhu pomocí nástrojů Application Insights


Můžete instrumentace živou webovou aplikaci pomocí služby Azure Application Insights, bez nutnosti toomodify nebo znovu nasadit vašeho kódu. Pokud vaše aplikace hostuje místní server služby IIS, nainstalujte Monitorování stavu. Pokud už webové aplikace Azure nebo spusťte virtuální počítač Azure, můžete přepnout na Application Insights monitorování z hello Azure ovládací panely. (Existují i samostatné články o instrumentaci [živých webových aplikací J2EE](app-insights-java-live.md) a [Azure Cloud Services](app-insights-cloudservices.md).) Budete potřebovat předplatné [Microsoft Azure](http://azure.com).

![ukázkové grafy](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

Máte možnost volby tří trasy tooapply Application Insights tooyour .NET webových aplikací:

* **Čas sestavení:** [hello přidat Application Insights SDK] [ greenbrown] tooyour kódu webové aplikace.
* **Čas spuštění:** instrumentaci vaší webové aplikace na serveru hello, jak je popsáno níže, bez znovu sestavit a znovu nasazovat hello kódu.
* **Obě:** sestavení hello SDK do kódu webové aplikace a také použít rozšíření běhu hello. Získat hello nejlepší z obou možností.

Tady je rekapitulace toho, co každý způsob přináší:

|  | Při sestavení | Za běhu |
| --- | --- | --- |
| Požadavky a výjimky |Ano |Ano |
| [Podrobnější výjimky](app-insights-asp-net-exceptions.md) | |Ano |
| [Diagnostika závislostí](app-insights-asp-net-dependencies.md) |Na platformě .NET 4.6+, ale méně podrobná |Ano, úplné podrobnosti: kódy výsledků, text příkazu SQL, příkaz HTTP|
| [Čítače výkonu systému](app-insights-performance-counters.md) |Ano |Ano |
| [Rozhraní API pro vlastní telemetrii][api] |Ano |Ne |
| [Integrace protokolu trasování](app-insights-asp-net-trace-logs.md) |Ano |Ne |
| [Zobrazení stránky a uživatelská data](app-insights-javascript.md) |Ano |Ne |
| Třeba toorebuild kódu |Ano | Ne |


## <a name="monitor-a-live-azure-web-app"></a>Monitorování živé webové aplikace Azure

Pokud vaše aplikace běží jako k službě Azure web, sem způsobu tooswitch na monitorování:

* Vyberte Application Insights v Ovládacích panelech hello aplikace v Azure.

    ![Nastavení Application Insights pro webovou aplikaci Azure](./media/app-insights-monitor-performance-live-website-now/azure-web-setup.png)
* Když se otevře stránka souhrnu hello Application Insights, klepněte na odkaz hello hello dolní tooopen hello úplné prostředek Application Insights.

    ![Klikněte na tlačítko prostřednictvím tooApplication statistiky](./media/app-insights-monitor-performance-live-website-now/azure-web-view-more.png)

[Monitorování cloudových aplikací a aplikací virtuálních počítačů](app-insights-azure.md).

### <a name="enable-client-side-monitoring-in-azure"></a>Povolení monitorování na straně klienta v Azure

Pokud jste v Azure povolili nástroje Application Insights, můžete přidat zobrazení stránky a telemetrii uživatelů.

1. Klikněte na Nastavení > Nastavení aplikace.
2.  V části Nastavení aplikace přidejte novou dvojici klíče a hodnoty: 
   
    Klíč: `APPINSIGHTS_JAVASCRIPT_ENABLED` 
    
    Hodnota: `true`
3. **Uložit** hello nastavení a **restartujte** vaší aplikace.

Hello Application Insights JavaScript SDK je nyní vsunout do každé webové stránky.

## <a name="monitor-a-live-iis-web-app"></a>Monitorování živé webové aplikace IIS

Pokud je vaše aplikace hostovaná na serveru služby IIS, povolte Application Insights pomocí Monitorování stavu.

1. Na webovém serveru služby IIS se přihlaste pomocí přihlašovacích údajů správce.
2. Pokud ještě není nainstalovaný monitorování stavu Application Insights, stáhněte a spusťte hello [instalačního programu Sledování stavu](http://go.microsoft.com/fwlink/?LinkId=506648) (nebo spustit [instalačního programu webové platformy](https://www.microsoft.com/web/downloads/platform.aspx) a vyhledejte v něm přehled stavu aplikace. Monitorování).
3. V monitorování stavu vyberte hello nainstalované webové aplikace nebo weby, které chcete toomonitor. Přihlaste se pomocí přihlašovacích údajů Azure.

    Konfigurace prostředků hello místo, kam chcete výsledky hello toosee hello portálu Application Insights. (Obvykle je nejlepší toocreate nový prostředek. Vyberte existující prostředek, pokud už pro tuto aplikaci máte [webové testy][availability] dostupnosti nebo [monitorování klienta][client] .) 

    ![Vyberte aplikaci a prostředek.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

4. Restartujte službu IIS.

    ![Zvolte restartování v horní části hello hello dialogového okna.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    Webová služba bude na krátkou dobu přerušena.

## <a name="customize-monitoring-options"></a>Přizpůsobení možností monitorování

Povolení Application Insights přidá knihovny DLL a soubor ApplicationInsights.config tooyour webové aplikace. Můžete [upravte soubor .config hello](app-insights-configuration-with-applicationinsights-config.md) toochange některé z možností hello.

## <a name="when-you-re-publish-your-app-re-enable-application-insights"></a>Když opětovně publikujete aplikaci, znovu povolte Application Insights

Před znovu publikovat aplikace, vezměte v úvahu [přidání kódu toohello Application Insights v sadě Visual Studio][greenbrown]. Získáte podrobnější telemetrie a hello možnost toowrite vlastní telemetrii.

Pokud chcete, aby toore-publikovat bez přidání Application Insights toohello kódu, uvědomte si, že proces nasazení hello může odstranit hello knihovny DLL a soubor ApplicationInsights.config z hello publikování webu. Proto:

1. Pokud jste upravili soubor ApplicationInsights.config, pořiďte si jeho zálohu, než budete aplikaci znovu publikovat.
2. Znovu publikujte aplikaci.
3. Znovu povolte monitorování pomocí Application Insights. (Použijte odpovídající metodu hello: hello Azure webové aplikace ovládacích panelů nebo hello monitorování stavu na hostiteli služby IIS.)
4. Obnovit všechny úpravy, které jste provedli v souboru .config hello.


## <a name="troubleshooting-runtime-configuration-of-application-insights"></a>Řešení potíží s konfigurací modulu runtime služby Application Insights

### <a name="cant-connect-no-telemetry"></a>Nelze se připojit? Žádná telemetrie?

* Otevřete [hello nezbytné Odchozí porty](app-insights-ip-addresses.md#outgoing-ports) v toowork monitorování stavu tooallow brány firewall vašeho serveru.

* Otevřete monitorování stavu a vyberte svou aplikaci v levém podokně. Zkontrolujte, zda existují jakékoli zprávy diagnostiky pro tuto aplikaci v části "Konfigurace oznámení" hello:

  ![Otevřete hello výkonu okno toosee žádost, dobu odezvy, závislosti a další data](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)
* Na serveru hello Pokud se zobrazí zpráva o "nedostatečných oprávněních", zkuste následující hello:
  * Ve Správci služby IIS vyberte fond aplikací, otevřete **Upřesnit nastavení**a v části **Model procesu** Poznámka: hello identity.
  * V Ovládacích panelech správy počítače přidejte tuto skupinu identity toohello Performance Monitor Users.
* Pokud máte na serveru nainstalovaný MMA/SCOM (System Center Operations Manager), může u některých verzí dojít ke konfliktu. Odinstalujte SCOM a sledování stavu a znovu nainstalujte nejnovější verze hello.
* Další informace najdete v tématu [Poradce při potížích][qna].

## <a name="system-requirements"></a>Systémové požadavky
Podpora operačního systému pro sledování stavu Application Insights na serveru:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows server 2012 R2
* Windows Server 2016

s nejnovější aktualizací SP a rozhraním .NET Framework 4.5

Na straně klienta hello: systém Windows 7, 8, 8.1 a 10, znovu pomocí rozhraní .NET Framework 4.5

Podpora služby IIS je: IIS 7, 7.5, 8, 8.5 (je vyžadována služba IIS)

## <a name="automation-with-powershell"></a>Automatizace v prostředí PowerShell
K zahájení a spuštění monitorování můžete na serveru služby IIS použít prostředí PowerShell.

Nejdříve naimportujte modul Application Insights hello:

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

Zjistěte, které aplikace se monitorují:

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* `-Name`Hello (volitelné) název webové aplikace.
* Zobrazí hello monitorování stavu Application Insights pro každou webovou aplikaci (nebo s názvem aplikace hello) v tomto serveru IIS.
* Vrátí `ApplicationInsightsApplication` pro každou aplikaci:

  * `SdkState==EnabledAfterDeployment`: Aplikace je monitorována a byla instrumentována v době běhu pomocí nástroje Monitor stavu hello nebo pomocí `Start-ApplicationInsightsMonitoring`.
  * `SdkState==Disabled`: hello aplikace není instrumentována pro službu Application Insights. Buď nebyla nikdy instrumentována, nebo spuštění monitorování zakázal hello nástroje Monitor stavu nebo s `Stop-ApplicationInsightsMonitoring`.
  * `SdkState==EnabledByCodeInstrumentation`: hello aplikace byla instrumentována přidáním hello SDK toohello zdrojového kódu. Její SDK nelze aktualizovat ani zastavit.
  * `SdkVersion`Zobrazuje verze hello používá pro monitorování této aplikace.
  * `LatestAvailableSdkVersion`Zobrazuje hello verzi aktuálně k dispozici v galerii NuGet hello. verze toothis tooupgrade hello aplikace, použijte `Update-ApplicationInsightsMonitoring`.

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* `-Name`Název Hello hello aplikace ve službě IIS
* `-InstrumentationKey`Hello ikey prostředku Application Insights, kam chcete toobe hello výsledky zobrazí hello.
* Tato rutina ovlivní pouze aplikace, které již nejsou instrumentovány – to znamená, SdkState==NotInstrumented.

    Hello rutina nemá vliv na aplikaci, která je již instrumentována. Ji není důležité, zda text hello aplikace byla instrumentována v okamžiku sestavení přidáním hello SDK toohello kód, nebo na dobu běhu předchozí pomocí této rutiny.

    Hello SDK verze použitá tooinstrument hello aplikace je, že hello verzi, která byla nedávno většina toothis server stáhli.

    toodownload hello nejnovější verzi, použijte příkaz Update-ApplicationInsightsVersion.
* V případě úspěchu vrátí `ApplicationInsightsApplication`. Pokud se nezdaří, zaprotokoluje trasování toostderr.

          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* `-Name`Název Hello aplikace ve službě IIS
* `-All` Zastaví monitorování všech aplikací v tomto serveru IIS, pro který platí, že `SdkState==EnabledAfterDeployment`.
* Zastaví monitorování hello dané aplikace a odebere instrumentace. Funguje pouze pro aplikace, které byly instrumentovány v běhu pomocí hello nástroje pro monitorování stavu nebo příkazu Start-ApplicationInsightsApplication. (`SdkState==EnabledAfterDeployment`)
* Vrátí ApplicationInsightsApplication.

`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]

* `-Name`: hello název webové aplikace ve službě IIS.
* `-InstrumentationKey` (Volitelné) Pomocí této toochange hello prostředků toowhich hello telemetrii aplikace je odeslán.
* Tato rutina:
  * Upgrady hello s názvem aplikace toohello verzi hello SDK naposledy stažené toothis počítače. (Funguje pouze v případě `SdkState==EnabledAfterDeployment`)
  * Pokud jste zadali kód instrumentace, změněnou konfigurací toosend telemetrie toohello prostředek s tímto klíčem, je hello s názvem aplikace. (Funguje v případě `SdkState != Disabled`)

`Update-ApplicationInsightsVersion`

* Stáhne hello nejnovější Application Insights SDK toohello serveru.

## <a name="questions"></a>Dotazy týkající se Monitorování stavu

### <a name="what-is-status-monitor"></a>Co je Monitorování stavu?

Desktopová aplikace, kterou instalujete s webovým serverem IIS. Pomáhá provádět instrumentaci a konfiguraci webových aplikací. 

### <a name="when-do-i-use-status-monitor"></a>Kdy použít Monitorování stavu?

* tooinstrument žádné webové aplikace, která běží na serveru se službou IIS - i v případě, že je již spuštěna.
* Další telemetrické tooenable pro webové aplikace, které byly [vytvořené s nástroji hello Application Insights SDK](app-insights-asp-net.md) v době kompilace. 

### <a name="can-i-close-it-after-it-runs"></a>Můžu ji po spuštění zavřít?

Ano. Poté, co se má instrumentována hello webů, které vyberete, můžete ho zavřít.

Sama o sobě telemetrii neshromažďuje. Právě nakonfiguruje hello webové aplikace a nastaví některá oprávnění.

### <a name="what-does-status-monitor-do"></a>K čemu Monitorování stavu slouží?

Když vyberete webovou aplikaci pro monitorování stavu tooinstrument:

* Stáhne a umístí hello Application Insights sestavení a souboru .config složku binárních souborů hello webové aplikace.
* Upraví `web.config` tooadd hello Application Insights na požadavek HTTP sledování modulu.
* Umožňuje CLR profilace toocollect závislostí volání.

### <a name="do-i-need-toorun-status-monitor-whenever-i-update-hello-app"></a>Potřebuji toorun monitorování stavu, vždy, když aktualizuji hello aplikace?

Ne, pokud provádíte opakované nasazení postupně. 

Pokud vyberete možnost, odstraňte stávající soubory"hello v hello publikování procesu, potřebovali byste toore spustit monitorování stavu tooconfigure Application Insights.

### <a name="what-telemetry-is-collected"></a>Jaké telemetrická data se shromažďují?

Pro aplikace instrumentované pouze za běhu pomocí Monitorování stavu:

* Požadavky HTTP
* Volání toodependencies
* Výjimky
* Čítače výkonu

Pro aplikace již instrumentované v době kompilace:

 * Čítače procesů
 * Volání závislostí (.NET 4.5); návratové hodnoty ve voláních závislostí (.NET 4.6)
 * Hodnoty trasování zásobníku výjimek

[Další informace](http://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next"></a>Další kroky

Zobrazení telemetrických dat:

* [Zkoumat metriky](app-insights-metrics-explorer.md) toomonitor výkonu a využití
* [Hledání událostí a protokolů] [ diagnostic] toodiagnose problémy
* [Analýzy](app-insights-analytics.md) pro pokročilejší dotazy
* [Vytváření řídicích panelů](app-insights-dashboards.md)

Přidání další telemetrie:

* [Vytvářejte webové testy] [ availability] toomake, zda web zůstává živý.
* [Přidat telemetrie webového klienta] [ usage] toosee výjimek z kódu webové stránky a toolet vložení trasovacího volání.
* [Přidejte Application Insights SDK tooyour kód] [ greenbrown] , aby mohli vložit trasování a protokolování volání

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-javascript.md
