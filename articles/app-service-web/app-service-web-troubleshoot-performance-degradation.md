---
title: "aaaSlow webové aplikace výkonu ve službě App Service | Microsoft Docs"
description: "Tento článek vám pomůže vyřešit problémy s výkonem pomalé webových aplikací ve službě Azure App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: top-support-issue
keywords: "webové aplikace výkonu, pomalé aplikace, aplikace pomalé"
ms.assetid: b8783c10-3a4a-4dd6-af8c-856baafbdde5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: cephalin
ms.openlocfilehash: 3e56b99b48db0e7baae1fac797a7fcb9eff74c9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Řešení potíží s výkonem pomalé webové aplikace v Azure App Service
Tento článek vám pomůže vyřešit problémy s výkonem pomalé webové aplikace v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello MSDN Azure a hello Stack Overflow fóra](https://azure.microsoft.com/support/forums/). Alternativně můžete také soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a klikněte na **získat podporu**.

## <a name="symptom"></a>Příznaky
Při procházení hello webové aplikace hello stránky zatížení pomalu a někdy časový limit.

## <a name="cause"></a>Příčina
Tento problém je často způsoben problémy na úrovni aplikací, například:

* požadavky sítě trvá příliš dlouho
* kód nebo databáze dotazy aplikace probíhá neefektivní
* aplikace pomocí vysoké paměti nebo procesoru
* selhání kvůli výjimce tooan aplikace

## <a name="troubleshooting-steps"></a>Řešení potíží
Řešení potíží s jde rozdělit na tři samostatné úkoly v sekvenčním pořadí:

1. [Sledovat a monitorovat chování aplikace](#observe)
2. [Shromažďování dat](#collect)
3. [Zmírnění hello problém](#mitigate)

[App Service Web Apps](/services/app-service/web/) nabízí různé možnosti v každém kroku.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Sledovat a monitorovat chování aplikace
#### <a name="track-service-health"></a>Sledování stavu služby
Microsoft Azure publicizes pokaždé, když je služba došlo k přerušení nebo výkonu snížení. Stav hello hello služby můžete sledovat na hello [portál Azure](https://portal.azure.com/). Další informace najdete v tématu [sledovat stav služeb](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Monitorování webové aplikace
Tato možnost umožňuje toofind out, pokud vaše aplikace je žádné problémy s. V okně vaší webové aplikace, klikněte na tlačítko hello **požadavky a chyby** dlaždici. Hello **metrika** okno se dozvíte, můžete přidat všechny hello metriky.

Některé hello metriky, můžete toomonitor pro vaši webovou aplikaci

* Průměrná paměti pracovní sady
* Průměrná doba odezvy
* Čas procesoru
* Paměť pracovní sady
* Požadavky

![sledování výkonu webové aplikace](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Další informace naleznete v tématu:

* [Monitorování webové aplikace v Azure App Service](web-sites-monitor.md)
* [Zobrazování oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Sledování stavu webových koncový bod
Pokud používáte webovou aplikaci v hello **standardní** cenovou úroveň, webových aplikací umožňuje monitorovat dva koncové body ze tří zeměpisné umístění.

Monitorování koncového bodu nakonfiguruje webové testy z geograficky distribuovaná umístění, které doba odezvy a provozu adres URL webových testů. Hello test provádí operaci HTTP GET s hello webové adresy URL toodetermine hello doba odezvy a doby provozu z každé umístění. Každé nakonfigurovaná umístění spouští test každých pět minut.

Doba provozu je monitorován pomocí kódů odpovědi HTTP a doba odezvy se měří v milisekundách. Test monitorování selže, pokud hello kódu odpovědi HTTP je větší než nebo rovna too400 nebo pokud odpověď hello trvá déle než 30 sekund. Koncový bod je považován za dostupný, pokud jeho monitorovací testy úspěšné ze všech hello zadané umístění.

tooset ho, najdete v části [monitorování aplikací v Azure App Service](web-sites-monitor.md).

Další informace naleznete v [zachování weby Azure až plus monitorování koncového bodu - s Stefan Schackow](https://channel9.msdn.com/Shows/Azure-Friday/Keeping-Azure-Web-Sites-up-plus-Endpoint-Monitoring-with-Stefan-Schackow) video o monitorování koncového bodu.

#### <a name="application-performance-monitoring-using-extensions"></a>Monitorování výkonu aplikací pomocí rozšíření
Můžete taky sledovat výkon aplikací pomocí *lokality rozšíření*.

Každý webové aplikace App Service poskytuje rozšiřitelný správy koncový bod, který vám umožní toouse výkonnou sadu nástrojů, které jsou nasazeny jako rozšíření lokality. Rozšíření zahrnují: 

- Jako zdrojový kód editory [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx). 
- Nástroje pro správu pro připojené prostředkům, například databáze MySQL připojen tooa webové aplikace.

[Azure Application Insights](/services/application-insights/) a [New Relic](/marketplace/partners/newrelic/newrelic/) jsou dvě rozšíření webů, které jsou k dispozici pro monitorování výkonu hello. toouse New Relic, nainstalujte agenta za běhu. toouse Azure Application Insights, opětovném vytvoření kódu pomocí sady SDK a můžete také nainstalovat rozšíření, které poskytuje přístup k datům tooadditional. Hello SDK umožňuje psát kód toomonitor hello využití a výkonu vaší aplikace podrobněji.

toouse Application Insights, najdete v části [sledování výkonu ve webových aplikacích](../application-insights/app-insights-web-monitor-performance.md).

toouse New Relic, najdete v části [nové Správa výkonu aplikací New Relic v Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />

### <a name="2-collect-data"></a>2. Shromažďování dat
prostředí webové aplikace Hello poskytuje diagnostické funkce pro protokolování informací z hello webový server a hello webové aplikace. informace o Hello je rozdělené na webový server diagnostics a application diagnostics.

#### <a name="enable-web-server-diagnostics"></a>Povolte diagnostiku webového serveru
Můžete povolit nebo zakázat hello následující typy protokolů:

* **Podrobné protokolování chyb** -podrobné informace o chybě pro stavové kódy HTTP, které indikují chybu (kód stavu 400 nebo vyšší). To může obsahovat informace, které vám mohou pomoci určit, proč hello server vrátil kód chyby hello.
* **Se nezdařilo, trasování požadavku** -podrobné informace o chybných žádostech, včetně trasování hello IIS komponenty používané tooprocess hello požadavku a hello doba v jednotlivých součástí. To může být užitečné, pokud se pokoušíte výkon tooimprove webové aplikace nebo izolovat, co ho způsobuje. konkrétní chyba protokolu HTTP.
* **Webový Server protokolování** -informace o použití formátu souborů protokolu rozšířené hello W3C transakce HTTP. To je užitečné, když chcete určit celkový metriky webové aplikace, například hello počet požadavků zpracovaných nebo je počet požadavků z konkrétní IP adresu.

#### <a name="enable-application-diagnostics"></a>Povolit rozhraní application diagnostics
Existuje několik možností toocollect údaje o výkonu aplikací z webové aplikace, profil aplikace za provozu ze sady Visual Studio nebo změnit toolog kódu vaší aplikace, další informace a trasování. Můžete zvolit možnosti hello založené na tom, kolik přístup máte aplikaci toohello a zjištěnými hello nástroje pro sledování.

##### <a name="use-application-insights-profiler"></a>Pomocí nástroje Application Insights Profiler
Můžete povolit toostart Application Insights profileru hello zaznamenávání podrobné výkonu trasování. Můžete přistupovat trasování zachytit až toofive, když potřebujete tooinvestigate problémy před dny došlo v posledních hello. Tuto možnost můžete také mají přístup toohello webové aplikace na prostředek Application Insights na portálu Azure.

Application Insights profileru obsahuje statistiky doba odezvy pro každé volání webové a trasování, která určuje, které řádek kódu způsobila hello pomalé odezvy. Někdy hello aplikaci služby App Service je pomalý, protože není určitý kód napsaný v původce způsobem. Mezi příklady patří sekvenční kód, který se může spouštět ve kolizí zámků paralelní a nežádoucí databáze. Odebrání těchto kritická místa v hello kódu zvyšuje výkon aplikace hello, ale jsou pevné toodetect bez nastavování vypracovala trasování a protokoly. trasování Hello získané nástrojem Application Insights profileru pomáhá identifikace hello řádků kódu, který zpomaluje hello aplikace a vyřešit tento problém pro aplikace služby App Service.

 Další informace najdete v tématu [profilování za provozu Azure web apps s Application Insights](../application-insights/app-insights-profiler.md).

##### <a name="use-remote-profiling"></a>Použití vzdálené profilování
Ve službě Azure App Service Web Apps, aplikace API a webové úlohy může být vzdáleně profilovaným. Tuto možnost zvolte, pokud máte přístup toohello webové aplikace prostředků a víte, jak tooreproduce hello problém nebo pokud znáte přesnou hello se stane čas interval hello týkající se výkonu.

Je užitečné, pokud hello využití CPU procesem hello je vysoká a váš proces běží něco pomalejší, než se očekávalo, nebo hello latence požadavky HTTP jsou vyšší než normální, můžete vzdáleně profilu váš proces a získat hello procesoru vzorkování volání zásobníky tooanalyze vzdálené profilování Hello proces aktivity a aktivní cesty kódu.

Další informace najdete v tématu [vzdálené profilování podpory v Azure App Service](https://azure.microsoft.com/blog/remote-profiling-support-in-azure-app-service).

##### <a name="set-up-diagnostic-traces-manually"></a>Ručně nastavit diagnostické trasování
Pokud máte přístup toohello webové aplikace zdrojového kódu, rozhraní Application diagnostics můžete informace o toocapture vytvořil webovou aplikací. Aplikace ASP.NET můžete použít hello `System.Diagnostics.Trace` rozhraní application diagnostics třída toolog informace toohello protokolu. Ale musíte toochange hello kód a znovu nasaďte aplikaci. Tato metoda se doporučuje, když aplikace běží v testovacím prostředí.

Podrobné pokyny najdete v aplikaci pro protokolování tooconfigure [povolit protokolování diagnostiky pro webové aplikace v Azure App Service](web-sites-enable-diagnostic-log.md).

#### <a name="use-hello-azure-app-service-support-portal"></a>Pomocí portálu pro podporu služby Azure App Service hello
Web Apps poskytuje hello možnost tootroubleshoot problémy související tooyour webové aplikace prohlížením HTTP protokoly, protokoly událostí, výpisy procesů a další. Dostanete tyto informace pomocí náš portál podpory v **http://&lt;název aplikace >.scm.azurewebsites.net/Support**

portál podporu Hello Azure App Service nabízí tři samostatné karty toosupport hello tři kroky běžné scénáře řešení potíží:

1. Sledovat aktuální chování
2. Analýza shromažďování diagnostické informace a spuštěním hello předdefinované analyzátory
3. Zmírnění

Pokud problém hello se děje nyní, klikněte na **analyzovat** > **diagnostiky** > **diagnostikovat teď** toocreate diagnostické relaci, který shromažďuje protokoly HTTP, protokoly Prohlížeče událostí, výpisy paměti, protokoly chyb PHP a sestava procesu PHP.

Jakmile hello data jsou shromažďována, hello podporu portál spouští analýzu dat hello a vám poskytne sestavu ve formátu HTML.

V případě, že chcete toodownload hello data, ve výchozím nastavení, by je uložená ve složce D:\home\data\DaaS hello.

Další informace o portálu podporu hello Azure App Service naleznete v tématu [tooSupport nové aktualizace rozšíření lokality pro weby sady Azure](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-hello-kudu-debug-console"></a>Použití hello konzoly ladění modulu Kudu
Webové aplikace se dodává s konzolou pro ladění, který můžete použít pro ladění, prohlížení, nahrávání souborů a také koncové body JSON pro získání informací o vašem prostředí. Tato konzola se nazývá hello *Kudu konzoly* nebo hello *řídicí panel SCM* pro vaši webovou aplikaci.

Tento řídicí panel můžete přejít pomocí odkazu přejdete toohello **https://&lt;název aplikace >.scm.azurewebsites.net/**.

Jsou některé hello věcí, které Kudu poskytuje:

* nastavení prostředí pro vaši aplikaci
* datový proud protokolu
* diagnostické výpis
* ladění konzoly, ve kterém můžete spouštět rutiny prostředí Powershell a základních příkazů DOS.

Další užitečné funkce Kudu je, že v případě, že aplikace je vyvolání first chance výjimek, můžete použít Kudu a výpisů hello SysInternals nástroj Procdump toocreate paměti. Tyto výpisy paměti jsou snímky hello procesu a často může pomoci při odstraňování složitějších problémů s vaší webové aplikace.

Další informace o funkcích, které jsou k dispozici v Kudu, najdete v části [nástroje Azure weby Team Services byste měli vědět o](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-hello-issue"></a>3. Zmírnění hello problém
#### <a name="scale-hello-web-app"></a>Škálování hello webové aplikace
Ve službě Azure App Service pro vyšší výkon a propustnost, můžete upravit hello škálování, ve kterém je spuštěná vaše aplikace. Škálování webovou aplikaci zahrnuje dvě souvisejících akcích: Změna vašeho plánu služby App Service tooa vyšší cenové úrovně a konfigurace určitá nastavení po Přepnuli jste toohello vyšší cenová úroveň.

Další informace o škálování najdete v tématu [škálování webové aplikace v Azure App Service](web-sites-scale.md).

Kromě toho můžete zvolit toorun aplikaci na více než jednu instanci. Horizontální navýšení kapacity pouze vám poskytne další schopnosti zpracování, ale také vám dává některé množství odolnost proti chybám. Pokud proces hello přestane fungovat na jednu instanci, hello ostatní instance pokračovat tooserve požadavky.

Můžete nastavit hello toobe ruční nebo automatické škálování.

#### <a name="use-autoheal"></a>Pomocí funkce AutoHeal
Funkce AutoHeal recykluje pracovní proces hello pro vaši aplikaci na základě nastavení, které zvolíte (jako jsou změny konfigurace, požadavky, omezení na základě paměti nebo hello čas potřeby tooexecute požadavek). Většinu času hello recyklaci hello proces je hello nejrychlejší způsob, jak toorecover po chybě. I když můžete vždy restartovat hello webové aplikace z přímo v rámci hello portálu Azure, funkce AutoHeal provede se automaticky. Toodo stačí je přidat některé aktivační události v hello kořenovém souboru web.config pro vaši webovou aplikaci. By tato nastavení fungují v hello stejný způsobem, i když se vaše aplikace není aplikace .net.

Další informace najdete v tématu [automatické opravy weby Azure](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-hello-web-app"></a>Restartujte hello webové aplikace
Restartování je často hello nejjednodušší způsob, jak toorecover z jednorázových problémů. Na hello [portál Azure](https://portal.azure.com/), v okně vaší webové aplikace, máte hello možnosti toostop nebo restartujte aplikaci.

 ![Restartujte problémy s výkonem toosolve webových aplikací](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Můžete také spravovat webové aplikace pomocí Azure Powershell. Další informace najdete v tématu [Použití Azure PowerShellu s Azure Resource Managerem](../powershell-azure-resource-manager.md).
