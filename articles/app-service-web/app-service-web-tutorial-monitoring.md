---
title: "aaaMonitor webové aplikace | Microsoft Docs"
description: "Zjistěte, jak tooset až monitorování na vaší webové aplikace"
services: App-Service
keywords: 
author: btardif
ms.author: byvinyal
ms.date: 04/04/2017
ms.topic: article
ms.service: app-service-web
ms.openlocfilehash: c2f5e9842c732a804f1caee5d67e53dad24e190a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-app-service"></a>Monitorování služby App Service
Tento kurz vás provede monitorování aplikace a použití hello integrovanou platformu nástroje toosolve problémy, když k nim dojde.

Každá část tohoto dokumentu prochází přes konkrétní funkci. Pomocí funkce hello společně umožňují:
- Identifikace problém ve vaší aplikaci.
- Určení, kdy hello problém je způsobený platformou kód nebo hello.
- Zúží hello zdroj hello problému v kódu.
- Ladění a opravě problému hello.

## <a name="before-you-begin"></a>Než začnete
- Potřebujete toomonitor webové aplikace a hello postupujte podle uvedených kroků.
    - Můžete vytvořit aplikace hello kroků popsaných v hello [vytvoření aplikace ASP.NET v Azure SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) kurzu.

- Pokud chcete tootry **vzdálené ladění** vaší aplikace, musíte Visual Studio.
    - Pokud ještě nemáte nainstalované Visual Studio 2017, můžete stáhnout a použít hello volné [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).
    - Ujistěte se, že povolíte **Azure development** při instalaci sady Visual Studio hello.

## <a name="metrics"></a>Krok 1 – zobrazit metriky
**Metriky** jsou užitečné toounderstand:
- Stav aplikace
- Výkon aplikace
- Spotřeba prostředků

Při zkoumání problému s aplikací, je kontrola metriky toostart vhodné místo. Portál Azure je rychlý způsob toovisually, zkontrolujte hello metrik vaší aplikace pomocí **Azure monitorování**.

Metriky zadejte Historický přehled napříč několika klíčových agregace pro vaši aplikaci. Pro žádné aplikace hostované ve službě app service měli byste sledovat hello webové aplikace a hello plán služby App Service.

> [!NOTE]
> Plán služby App Service představuje kolekci hello toohost fyzické prostředky, které používá vaše aplikace. Všechny aplikace přiřazené tooan služby App Service plán sdílené složky hello prostředky definovaná tímto povolení toosave nákladů při hostování více aplikací.
>
> Plány služby App Service definují:
> * Oblast: Severní Evropa, východní USA, jihovýchodní Asie, atd.
> * Velikost instance: Malé, střední, velké, atd.
> * Počet škálování: jednu, dvě nebo tři instance, atd.
> * Skladová položka: Free, sdílené, Basic, Standard, Premium, atd.

tooreview metriky pro vaši webovou aplikaci, přejděte toohello **přehled** okno aplikace hello chcete toomonitor. Tady můžete zobrazit graf metrik vaší aplikace, jako **dlaždice monitorování**. Klikněte na dlaždici tooedit hello a nakonfigurovat co tooview metriky a hello toodisplay rozsah času.

Ve výchozím okně prostředků hello poskytuje zobrazení pro žádosti o aplikace hello a chyby pro hello poslední hodinu.
![Monitorování aplikace](media/app-service-web-tutorial-monitoring/app-service-monitor.png)

Jak vidíte v příkladu hello, budeme mít aplikaci, která generuje mnoho **chyby protokolu HTTP serveru**. Hello velkému počtu chyb je hello první znamenat, že potřebujeme tooinvestigate této aplikace.

> [!TIP]
> Další informace o monitorování Azure s hello následující odkazy:
> - [Začínáme s Azure monitorování](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [Azure metriky](..\monitoring-and-diagnostics\monitoring-overview-metrics.md)
> - [Podporované metriky s monitorováním Azure](..\monitoring-and-diagnostics\monitoring-supported-metrics.md)
> - [Azure řídicí panely](..\azure-portal\azure-portal-dashboards.md)

## <a name="alerts"></a>Krok 2 – konfigurace výstrah
**Výstrahy** může být nakonfigurované tootrigger na konkrétní podmínky pro vaši aplikaci.

V [krok 1 – zobrazit metriky](#metrics), jsme viděli, že aplikace hello měl velký počet chyb.

Umožňuje nakonfigurovat výstrahu tooautomatically dostat upozornění, pokud dojde k chybám. V takovém případě má toosend hello výstrah a e-mailu pokaždé, když hello počet chyb HTTP 50 X prochází přes určitou mez.

toocreate výstraha, přejděte příliš**monitorování** > **výstrahy** a klikněte na tlačítko **[+] přidat upozornění**.

![Výstrahy](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts.png)

Zadejte hodnoty pro konfiguraci výstrah hello:
- **Prostředek:** hello toomonitor lokality s hello výstrahy.
- **Název:** název výstrahy, v tomto případě: *vysoké HTTP 50 X*.
- **Popis:** prostý text vysvětlení, co je tato výstraha prohlížení.
- **Výstraha:** výstrahy můžete si prohlédnout metriky nebo událostí, v tomto příkladu Těšíme se na metriky.
- **Metrika:** jaké metriky toomonitor, v tomto případě: *chyby protokolu HTTP serveru*.
- **Podmínka:** při tooalert, v tomto případě vyberte hello *větší než* možnost.
- **Prahová hodnota:** co toolook hodnotu, je v tomto případě: *400*.
- **Období:** výstrahy pracovat s průměrnou hodnotu hello metriky. Menší období yield citlivější výstrahy. v takovém případě Těšíme se na *5 minut*.
- **E-mailu vlastníci a přispěvatelé:** v tomto případě: *povoleno*.

Je teď vytvořená hello výstraha e-mail je odeslán pokaždé, když hello aplikace přejde nad prahovou hodnotou hello nakonfigurované. Aktivní výstrahy mohou být také zjišťovány hello portálu Azure.

![Spouštěná výstrahy](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts-triggered.png)


> [!TIP]
> Další informace o výstrahách Azure s hello následující odkazy:
> - [Co jsou výstrahy v Microsoft Azure](..\monitoring-and-diagnostics\monitoring-overview-alerts.md)
> - [Provést akci pro metriky](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [Vytvoření metriky výstrahy](..\monitoring-and-diagnostics\insights-alerts-portal.md)

## <a name="companion"></a>Krok 3 – doprovodné služby App Service
**Služby App Service doprovodné** nabízí pohodlný způsob toomonitor aplikace s nativním prostředím v mobilních zařízení (iOS nebo Android).

Použijte doprovodné služby App Service na:
- Zkontrolujte metriky aplikace
- Zkontrolujte a provádění akcí na výstrahy aplikace a doporučení
- Provádět základní řešení problémů (Procházet, spuštění, zastavení, restartování aplikace hello)
- Získáte nabízená oznámení pro kritické události.

![Doprovodná služby App Service](media/app-service-web-tutorial-monitoring/app-service-companion.png)

[![Aplikace služby doprovodné obchod](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![doprovodné aplikace služby Google Play](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)

Doprovodná služby App Service si můžete nainstalovat z hello [obchod](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) nebo [Google Play](https://play.google.com/store/apps/details?id=azureApps.AzureApps)

## <a name="diagnose"></a>Krok 4 – Diagnostika a řešení problémů
**Diagnostika a řešení problémů** pomáhá oddělíte problémy aplikací formuláři problém s platformou. Ho můžete také navrhnout možné způsoby zmírnění tooget back toohealthy vaší webové aplikace.

![Diagnostika a řešení problémů](media/app-service-web-tutorial-monitoring/app-service-monitor-diagnosis.png)

Pokračováním hello příklad formuláře předchozí kroky, vidíme, že aplikace hello má byla dostupnosti problémy s. Naproti tomu hello platformy dostupnosti nepřesunul ze 100 %.

Když aplikace hello má problém a hello platformy zůstane až, je jasný náznak, který jsme se problému s aplikací pracovat.

## <a name="logging"></a>Krok 5 – protokolování
Teď, když budeme mít co nejlépe určen hello selhání tooan aplikace problém, můžete podíváme na tooget protokoly aplikací a server hello Další informace.

Protokolování vám umožní toocollect obou **rozhraní Application Diagnostics** a **Diagnostika webového serveru** protokoly pro vaši webovou aplikaci.

### <a name="application-diagnostics"></a>Rozhraní Application Diagnostics
Rozhraní Application diagnostics umožňuje vám trasování toocapture vyprodukované hello aplikace za běhu.

Přidání trasování tooyour aplikace výrazně zlepšuje možnost toodebug a problémy Kotvicí bod.

V technologii ASP.NET, může přihlásit pomocí trasování aplikací [System.Diagnostics.Trace – třída](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) toogenerate události, které jsou zachyceny hello protokolu infrastruktury. Můžete také zadat hello závažnost hello trasování pro snazší filtrování.

```csharp
public ActionResult Delete(Guid? id)
{
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id);
    if (id == null)
    {
        System.Diagnostics.Trace.TraceError("/Todos/Delete/ failed, ID is null");
        return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
    }
    Todo todo = db.Todoes.Find(id);
    if (todo == null)
    {
        System.Diagnostics.Trace.TraceWarning("/Todos/Delete/ failed, ID: " + id + " could not be found");
        return HttpNotFound();
    }
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id + "completed successfully");
    return View(todo);
}
```
protokolování aplikací tooenable přejděte příliš**monitorování** > **diagnostické protokoly** a povolit protokolování aplikace pomocí přepínačů hello.

![Monitorování aplikace](media/app-service-web-tutorial-monitoring/app-service-monitor-applogs.png)

Protokoly aplikací může být systém souborů uložených tooyour webovou aplikaci nebo instalováni tooblob úložiště. U produkčních scénářích je doporučené toouse úložiště objektů blob.

> [!IMPORTANT]
> Povolení protokolování má vliv na vaše aplikace výkonu a využití prostředků. Pro produkčních scénářích se doporučují protokoly chyb. Podrobnější protokolování povolte pouze v případě příčin problémů.

 ### <a name="web-server-diagnostics"></a>Diagnostika webového serveru
Protokoly webového serveru jsou generovány, i v případě, že vaše aplikace není instrumentována. Služby App Service může shromažďovat tři různé typy protokolů serveru:

- **Protokolování webového serveru**
    - Informace o transakcích HTTP pomocí hello [rozšířený formát protokolu W3C souboru](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
    - Užitečné při určování metriky celkového lokality například hello počet požadavků zpracovaných nebo je počet požadavků z konkrétní IP adresu.
- **Podrobné informace o chybě protokolování**
    - Podrobné informace o chybě pro stavové kódy HTTP, které indikují chybu (kód stavu 400 nebo vyšší).
    - [Další informace o protokolování podrobné informace o chybě](https://www.iis.net/learn/troubleshoot/diagnosing-http-errors/how-to-use-http-detailed-errors-in-iis)
- **Trasování neúspěšných žádostí.**
    - Podrobné informace o chybných žádostech, včetně trasování pro součásti služby IIS hello používá tooprocess hello požadavku a hello doba v jednotlivých součástí.
    - Při pokusu o tooisolate co ho způsobuje. konkrétní chyba protokolu HTTP lze využít protokoly chybných požadavků.
    - [Další informace o trasování neúspěšných žádostí.](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)

protokolování serveru tooenable:
- přejděte příliš**monitorování** > **diagnostické protokoly**.
- Povolte hello různé typy Diagnostika webového serveru pomocí přepínačů hello.

![Monitorování aplikace](media/app-service-web-tutorial-monitoring/app-service-monitor-serverlogs.png)

> [!IMPORTANT]
> Povolení protokolování má vliv na vaše aplikace výkonu a využití prostředků. Pro produkčních scénářích se doporučuje protokoly chyb, pouze povolit podrobnější protokolování při zkoumání problémů.

### <a name="accessing-logs"></a>Přístup k protokoly
Protokolů uložených v úložišti objektů blob ke kterým se přistupuje pomocí Průzkumníka úložiště Azure. Protokolů uložených v systému souborů hello webové aplikace jsou přístupné prostřednictvím FTP v rámci hello následující cesty:

- **Protokoly aplikací** - `%HOME%/LogFiles/Application/`.
    - Tato složka obsahuje jeden nebo více souborů textu obsahujícího informace o vyprodukované protokolování aplikací.
- **Trasování požadavku se nezdařilo** - `%HOME%/LogFiles/W3SVC#########/`.
    - Tato složka obsahuje soubor XSL a jeden nebo více souborů XML.
- **Podrobné protokoly chyb** - `%HOME%/LogFiles/DetailedErrors/`.
    - Tato složka obsahuje jeden nebo více souborů HTM rozsáhlé informace na chyby protokolu HTTP vygenerovaný vaší aplikací.
- **Webový Server protokoly** - `%HOME%/LogFiles/http/RawLogs`.
    - Tato složka obsahuje jeden nebo více souborů text formátován pomocí formát souboru protokolu rozšířené hello W3C.

## <a name="streaming"></a>Krok 6: protokolu streamování
Protokoly streamování jsou vhodné při ladění aplikace, protože ho šetří čas porovnání příliš[přístup k hello protokoly](#Accessing-Logs) pomocí služby FTP.

Služby App Service můžete stream **protokoly aplikací** a **protokolů webového serveru** jako jsou generovány.

> [!TIP]
> Než to zkusíte toostream protokoly, ujistěte se, jak je popsáno v hello jste povolili shromažďování protokolů [protokolování](#logging) části.

protokoly toostream přejděte příliš**monitorování**> **datový proud protokolu**. Vyberte **protokoly aplikací** nebo **webové protokoly serveru** podle informací hledáte. Tady můžete pozastavit, restartovat a vymazat hello vyrovnávací paměti.

![Protokoly streamování](media/app-service-web-tutorial-monitoring/app-service-monitor-logstream.png)

> [!TIP]
> Protokoly jsou generovány pouze, pokud je provoz na hello aplikace, můžete taky zvýšit hello podrobností protokoly tooget další události nebo informace.

## <a name="remote"></a>Krok 7 – vzdálené ladění
Jakmile máte PIN kód odkazoval hello zdroje hello problémů aplikace, použijte **vzdálené ladění** toowalk prostřednictvím hello kódu.

Vzdálené ladění umožňuje připojit ladicí program tooyour webová aplikace spuštěna v cloudu hello. Můžete nastavit zarážky, pracovat přímo s paměti, krok prostřednictvím kódu a dokonce změnit hello kódové cestě stejně, jako je tomu u aplikace spuštěná místně.

aplikaci tooyour ladicího programu hello tooattach spuštěnou v cloudu hello:

- Pomocí Visual Studio 2017, otevřete hello řešení pro aplikaci hello chcete toodebug
- Nastavte některé body brzdy stejně, jako byste pro místní vývoj.
- Otevřete **Průzkumník cloudu** (pev.cenu + /, ctrl + x).
- Přihlaste se pomocí přihlašovacích údajů azure podle potřeby.
- Aplikace hello najít chcete toodebug
- Vyberte **připojit ladicí program** formuláře hello **akce** podokně.

![Vzdálené ladění](media/app-service-web-tutorial-monitoring/app-service-monitor-vsdebug.png)

Visual Studio nakonfiguruje aplikaci pro vzdálené ladění a spustí okno prohlížeče, který odkazuje tooyour aplikace. Procházet body rozdělení tootrigger vaší aplikace a krok prostřednictvím hello kódu.

> [!WARNING]
> Spuštění v režimu ladění v produkčním prostředí se nedoporučuje. Pokud vaše produkční aplikace není škálovat instance serveru toomultiple, ladění zabránit hello webový server z odpovídá tooother žádostí. Při řešení problémů produkční, nejlepší prostředek je příliš[konfigurace protokolování](#logging) a [Application Insights](#insights).



## <a name="explorer"></a>Krok 8 – Průzkumníka procesů
Pokud vaše aplikace je škálovat toomore než jednu instanci, **Průzkumníka procesů** můžete identifikovat problémy konkrétní instanci.

Použití **zpracovat Explorer** na:

- Výčet všech procesů hello mezi různými instancemi plánu služby App Service.
- Procházet a zobrazte hello obslužné rutiny a moduly, které jsou přidružené k jednotlivých procesů.
- Počet zobrazení procesoru, pracovní sady a přístup z více vláken v hello zpracovat úrovně toohelp identifikovat nezvladatelné procesy
- Najděte otevřených popisovačů souborů a i kill instance konkrétní procesu.

Průzkumníka procesů naleznete v části **monitorování** > **Průzkumníka procesů**.

![Průzkumník procesů](media/app-service-web-tutorial-monitoring/app-service-monitor-processexplorer.png)


## <a name="insights"></a>Krok 9 – Application Insights
**Application Insights** poskytuje profilace aplikací a rozšířené možnosti monitorování pro vaši aplikaci.

Pomocí Application Insights toodetect a diagnostikovat výjimky a problémy s výkonem ve vaší webové aplikaci.

Můžete povolit Application Insights pro webové aplikace v rámci **monitorování** > **Application Insights**

> [!NOTE]
> Application Insights může výzvu tooinstall hello Application Insights lokality rozšíření toostart shromažďování dat. Instaluje se rozšíření lokality hello způsobí restartování aplikace.

![Application Insights](media/app-service-web-tutorial-monitoring/app-service-monitor-appinsights.png)

Application Insights obsahuje bohaté funkce nastavit, toolearn víc, použijte odkazy hello součástí hello [další kroky](#next) části.

## <a name="next"></a> Další kroky

 - [Co je Application Insights](..\application-insights\app-insights-overview.md)
 - [Monitorování výkonu Azure webové aplikace pomocí Application Insights](..\application-insights\app-insights-azure-web-apps.md)
 - [Sledování dostupnosti a odezvy libovolných webů s Application Insights](..\application-insights\app-insights-monitor-web-app-availability.md)
