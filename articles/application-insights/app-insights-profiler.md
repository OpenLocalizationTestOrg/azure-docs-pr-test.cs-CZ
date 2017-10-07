---
title: "aaaProfiling za provozu webové aplikace v Azure s Application Insights | Microsoft Docs"
description: "Identifikujte hello aktivní trase v kódu webového serveru s nízkými nároky profileru."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: bwren
ms.openlocfilehash: 3c7f21076f19335e0f006327932e13623ec9526b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="profiling-live-azure-web-apps-with-application-insights"></a>Profilace za provozu Azure web apps s Application Insights

*Tato funkce Application Insights je GA pro aplikační služby a ve verzi preview pro výpočet.*

Zjistěte, jak dlouho je strávil v jednotlivých metod v za provozu webové aplikace pomocí hello profilace nástroje [Azure Application Insights](app-insights-overview.md). Ukazuje podrobné profily živé požadavky, které se spouští vaše aplikace a zvýrazňuje hello 'aktivní cesta', která používá hello většinu času. Automaticky vybere příklady, které mají různé doby odezvy. profileru Hello používá toominimize režijní náklady na různé techniky.

Hello profileru aktuálně funguje pro technologii ASP.NET webové aplikace běžící v Azure App Services, v nejméně hello Basic cenová úroveň. 

<a id="installation"></a>
## <a name="enable-hello-profiler"></a>Povolit hello profileru

[Nainstalujte službu Application Insights](app-insights-asp-net.md) ve vašem kódu. Pokud je již nainstalována, ujistěte se, že máte nejnovější verzi hello. (toodo, klikněte pravým tlačítkem na projekt v Průzkumníku řešení a zvolte spravovat balíčky NuGet balíčky. Vyberte aktualizaci a aktualizovat všechny balíčky.) Znovu nasazení vaší aplikace.

*Pomocí ASP.NET Core? [Zkontrolujte, zde](#aspnetcore).*

V [https://portal.azure.com](https://portal.azure.com), otevřete hello prostředek Application Insights pro webové aplikace. Otevřete **výkonu** a klikněte na tlačítko **povolit Application Insights profileru...** .

![Klikněte na možnost na hello povolit banner profileru][enable-profiler-banner]

Alternativně můžete vždy kliknout na **konfigurace** tooview stav, povolit nebo zakázat hello profileru.

![V okně hello výkonu kliknutím na tlačítko Konfigurovat][performance-blade]

V okně konfigurace jsou uvedeny webových aplikací, které jsou nakonfigurovány s Application Insights. Postupujte podle pokynů tooinstall hello profileru agenta v případě potřeby. Pokud žádná webová aplikace je nakonfigurovaný s Application Insights ještě, klikněte na tlačítko *přidat propojené aplikace*.

Použití hello *povolit profileru* nebo *zakázat profileru* tlačítka v hello konfigurovat okno toocontrol hello profileru na všechny propojené webové aplikace.



![Konfigurace okna][linked app services]

toostop nebo restartování hello profileru pro jednotlivé instance služby App Service, naleznete je **v hello prostředek služby App Service**v **webové úlohy**. toodelete ho, naleznete v části **rozšíření**.

![Zakázat profilovací nástroj pro webové úlohy][disable-profiler-webjob]

Doporučujeme, abyste měli hello profileru na toodiscover o všechny vaše aplikace webové co nejdříve problémů s výkonem, všechny povolené.

Pokud používáte WebDeploy toodeploy změny tooyour webové aplikace, ujistěte se, abyste vyloučili hello **App_Data** složku před odstraněním během nasazení. Jinak, hello profileru rozšíření soubory se odstraní při další nasazení hello webové aplikace tooAzure.

### <a name="using-profiler-with-azure-vms-and-compute-resources-preview"></a>Pomocí profileru s virtuálními počítači Azure a výpočetní prostředky (preview)

Pokud jste [povolit Application Insights pro aplikace Azure services v době běhu](app-insights-azure-web-apps.md#run-time-instrumentation-with-application-insights), profileru je automaticky dostupný. (Pokud už jste povolili Application Insights pro hello prostředků, je třeba tooupdate toohello lates verzi prostřednictvím hello **konfigurace** průvodce.)

Je [verze preview hello profileru pro Azure výpočetní prostředky](https://go.microsoft.com/fwlink/?linkid=848155).


## <a name="limits"></a>Omezení

uchovávání dat výchozí Hello je 5 dní. Maximální 10 GB požity za den.

Není nijak zpoplatněn služby profileru hello. Webové aplikace musí být uloženy ve alespoň hello základní úroveň aplikační služby.

## <a name="viewing-profiler-data"></a>Zobrazení dat profileru

Otevřete okno výkon hello a projděte dolů seznam operací toohello.




![Okna Statistika výkonu aplikací příklady sloupce][performance-blade-examples]

Hello sloupců v tabulce hello jsou:

* **Počet** -hello počet tyto požadavky v hello časové rozmezí hello okna.
* **Medián** -hello Typická doba toorespond tooa požadavek trvá vaší aplikace. Polovinu všechny odpovědi byly rychlejší než to.
* **95. percentil** 95 % odpovědí byly rychlejší než to. Pokud na tomto obrázku je příliš neliší od hello medián, může být občasný problém s vaší aplikací. (Nebo může být vysvětlené návrh funkce jako je například ukládání do mezipaměti.)
* **Příklady** – ikona označuje, že hello profileru má zaznamenat trasování zásobníku pro tuto operaci.

Kliknutím na hello příklady ikonu tooopen hello trasování Průzkumníka. Zobrazuje Průzkumníka Hello má několik vzorků, které hello profileru zachycení klasifikovány podle doby odezvy.

Vyberte tooshow ukázkový kód úrovni rozpis čas strávený provádění požadavku hello.

![Application Insights trasování Explorer][trace-explorer]

**Zobrazit aktivní trase** otevře hello největších uzel typu list nebo alespoň něco zavřete. Ve většině případů bude tento uzel přiléhající tooa přetížení.



* **Popisek**: název hello hello funkce nebo událostí. Hello Strom zobrazuje směs kód a události, které došlo k chybě (například události SQL a http). horní událostí Hello představuje celkový hello požadavku doba trvání.
* **Uplynulý čas**: hello časový interval mezi počátečním hello hello operace a hello end.
* **Když**: zobrazí, pokud byl spuštěn hello funkce nebo události v vztah tooother funkce.

## <a name="how-tooread-performance-data"></a>Jak tooread údaje o výkonu

Microsoft služba profileru používá kombinaci vzorkování metoda a instrumentace tooanalyze hello výkon aplikace.
Když probíhá podrobné kolekce, ukázky profileru služby hello ukazatel instrukce každého z procesoru počítače hello v každé milisekundu.
Každá ukázka zaznamená zásobníku dokončení volání hello hello vlákno aktuálně provádění, udělíte podrobnější a užitečné informace o tom, co vlákno se to, že na úrovni i maximální a minimální úrovně abstrakce. Služba profileru shromažďuje také další události, jako je například přepínání události kontextu, TPL události a korelace aktivity tootrack události fondu vláken a příčiny.

Zásobník volání Hello vidět v zobrazení časové osy hello je hello výsledek hello nad vzorkování a instrumentace. Protože každá ukázka zaznamená zásobníku dokončení volání hello hello přístup z více vláken, obsahuje kód z hello rozhraní .NET framework, stejně jako ostatní platformy, na něž odkazuje.

### <a id="jitnewobj"></a>Objekt přidělení (`clr!JIT\_New or clr!JIT\_Newarr1`)
`clr!JIT\_New and clr!JIT\_Newarr1`jsou pomocné funkce v rozhraní .NET framework, která se přidělí paměť z spravovaná halda. `clr!JIT\_New`je voláno, když je přidělen objektu. `clr!JIT\_Newarr1`je voláno, když je přidělen pole objektu. Tyto dvě funkce jsou obvykle velmi rychlé a provést relativně malé množství času. Pokud se zobrazí `clr!JIT\_New` nebo `clr!JIT\_Newarr1` trvat významné množství času na časové ose, je to znamenat, že hello kód může být přidělování mnoho objektů a spotřebovávat značné množství paměti.

### <a id="theprestub"></a>Načítání kódu (`clr!ThePreStub`)
`clr!ThePreStub`je pomocné funkce v rozhraní .NET framework, který připraví hello kód tooexecute hello poprvé. To obvykle zahrnuje, ale bez omezení (pouze v době) JIT – kompilace. Pro každou C# metodu `clr!ThePreStub` by měla být volána alespoň jednou během doby života hello procesu.

Pokud se zobrazí `clr!ThePreStub` trvá značné množství času požadavku, označuje tento požadavek je hello první, kterou provádí tento metoda a hello času pro rozhraní .NET framework runtime tooload, že metoda je důležité. Můžete zvážit warm-up proces, který provede část kódu hello před vaši uživatelé k němu přístup nebo zvažte spuštění NGen vaše sestavení.

### <a id="lockcontention"></a>Zamknout kolizí (`clr!JITutil\_MonContention` nebo `clr!JITutil\_MonEnterWorker`)
`clr!JITutil\_MonContention`nebo `clr!JITutil\_MonEnterWorker` znamenat hello aktuální vlákno čeká zámek toobe vydání. To obvykle objeví při provádění jazyka C# zámku výpis, volání metody Monitor.Enter nebo volání metody s atributem MethodImplOptions.Synchronized. Spory uzamčení obvykle dojde, když vláken A získá zámek a vlákno B pokusí tooacquire hello stejné zamknout před přístup z více vláken A uvolněním.

### <a id="ngencold"></a>Načítání kódu (`[COLD]`)
Pokud obsahuje název metody hello `[COLD]`, jako například `mscorlib.ni![COLD]System.Reflection.CustomAttribute.IsDefined`, znamená to, že runtime rozhraní .NET framework hello provádí kód, který není optimalizována podle <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">optimalizace na základě profilu</a> pro hello poprvé. Pro každou metodu měl by se zobrazit maximálně jednou během doby života hello hello procesu.

Pokud významné množství času pro žádost o načtení kód používá, znamená to, této žádosti je hello první jeden tooexecute hello neoptimalizované část metoda hello. Můžete zvážit záložním až proces, který provede část kódu hello potom vaši uživatelé přístup.

### <a id="httpclientsend"></a>Odeslání požadavku HTTP
Metody, jako `HttpClient.Send` znamenat hello kód čeká toocomplete požadavku HTTP.

### <a id="sqlcommand"></a>Operace databáze
Metoda například SqlCommand.Execute označuje, že kód hello čeká toocomplete operace databáze.

### <a id="await"></a>Čeká se na (`AWAIT\_TIME`)
`AWAIT\_TIME`Označuje, že kód hello čeká na jiné toocomplete úloh. K tomu obvykle dochází, pomocí C#, await, příkaz. Pokud nemá hello kódu C#, await, hello přístup z více vláken unwinds a vrátí ovládací prvek toohello-fondu vláken a neexistuje žádné vlákno, které se zablokuje čekáním na toofinish "await" hello. Však logicky hello přístup z více vláken, nebyla hello await 'blokováno.' čekání toocomplete operaci hello. `AWAIT\_TIME` Označuje hello blokované dobu čekání toocomplete úloh hello.

### <a id="block"></a>Blokované čas
`BLOCKED_TIME`Označuje, že kód hello čeká na jiné toobe prostředek k dispozici, například čekání na synchronizační objekt čekání na vlákno toobe k dispozici nebo čekání toofinish požadavku.

### <a id="cpu"></a>Čas procesoru
Hello procesoru je zaneprázdněný prováděna hello pokyny.

### <a id="disk"></a>Doba disku
aplikace Hello provádí operace disku.

### <a id="network"></a>Čas sítě
aplikace Hello je provádění síťových operací.

### <a id="when"></a>Pokud sloupec
Toto je vizualizaci jak hello včetně ukázky shromážděné pro uzel lišit v průběhu času. Celkový rozsah Hello hello požadavku je rozdělené do 32 kbelíků čas a hello včetně ukázky pro tento uzel počítají do těchto 32 intervalů. Každý blok je pak reprezentován jako panel, jehož výška reprezentuje hodnotu škálovat. Pro uzly označena `CPU_TIME` nebo `BLOCKED_TIME`, nebo tam, kde je zřejmé vztah o využívání prostředků (procesor, disk, vlákno), hello panelu představuje jeden z těchto prostředků využívání hello dobu z této sady. Pro tyto metriky můžete získat větší než 100 % podle spotřebovávat více prostředků. Například pokud se v průměru v intervalu, použijte dva procesory, můžete získat 200 %.


## <a id="troubleshooting"></a>Řešení potíží

### <a name="how-can-i-know-whether-application-insights-profiler-is-running"></a>Jak poznám, jestli je spuštěný profileru Application Insights?

Hello profileru spouští jako nepřetržité webové úlohy ve webové aplikaci. Můžete otevřít prostředek hello webové aplikace v https://portal.azure.com a zkontrolujte stav "ApplicationInsightsProfiler" v okně webové úlohy hello. Pokud není spuštěna, otevřete **protokoly** toofind Další.

### <a name="why-cant-i-find-any-stack-examples-even-though-hello-profiler-is-running"></a>Proč nelze najít žádné příklady zásobníku, i když je spuštěna hello profileru?

Tady jsou můžete zkontrolovat pár věcí.

1. Zkontrolujte, že váš Web plán služby App Service je úroveň Basic a vyšší.
2. Zajistěte, aby vaše webová aplikace má Application Insights SDK 2.2 Beta a vyšší než povolené.
3. Ujistěte se, že vaše webová aplikace má nastavení APPINSIGHTS_INSTRUMENTATIONKEY hello s hello používá stejný klíč instrumentace Application Insights SDK.
4. Zajistěte, aby vaše webová aplikace běží na rozhraní .net Framework 4.6.
5. Pokud je aplikace ASP.NET Core, zkontrolujte také, zda [hello požadované závislosti](#aspnetcore).

Po spuštění profileru hello není krátké warm-up období, kdy hello profileru aktivně shromažďuje několik trasování výkonu. Potom hello profileru shromažďuje trasování výkonu pro dvou minut za každou hodinu.  

### <a name="i-was-using-azure-service-profiler-what-happened-tooit"></a>Použití profileru služby Azure. Jaké happened tooit?  

Když povolíte profileru Application Insights, profileru služby Azure agenta je zakázána.

### <a id="double-counting"></a>Dvojitý počítání v paralelních vláken

V některých případech hello metrika celkový čas v prohlížeči zásobníku hello je větší než skutečná doba trvání hello hello požadavku.

Tomu může dojít, pokud jsou dostupné dvě nebo více vláken, které jsou přidružené k požadavku operační paralelně. Celkový počet vláken čas Hello je pak více než hello uplynulý čas. V mnoha případech může být jedno vlákno čekání na hello jiných toocomplete. Hello prohlížeč pokusů toodetect to a vynechejte hello nezajímavá čekání, ale chybuje na straně hello zobrazující příliš mnoho místo vynechání co může být důležitých informací.  

Pokud uvidíte paralelních vláken ve vašem trasování, je potřeba toodetermine, který vláken čekají, můžete určit hello kritické cesty pro požadavek hello. Ve většině případů hello hello vlákno, které rychle přejde do stavu čekání jednoduše čekání na další vlákna. Soustředit se jenom na jiné hello a ignorovat hello čas v hello čekajících vláken.

### <a id="issue-loading-trace-in-viewer"></a>Žádná data profilování

1. Pokud hello data zkoušíte tooview je starší než několik týdnů, pokuste se omezit váš filtr času a akci opakujte.

2. Zkontrolujte, že proxy nebo brány firewall nejsou blokované toohttps://gateway.azureserviceprofiler.net přístup.

3. Zkontrolujte, že hello Application Insights je klíč instrumentace, který používáte ve vaší aplikaci hello stejné jako hello prostředek Application Insights, které jste povolili profilace s. klíč Hello je obvykle v souboru ApplicationInsights.config, ale můžete také najít v souboru web.config nebo app.config.

### <a name="error-report-in-hello-profiling-viewer"></a>Chyba sestavy v hello profilace prohlížeč

Soubor lístek podpory z portálu hello. Uveďte ID korelace hello z hello chybová zpráva.

## <a name="manual-installation"></a>Ruční instalace

Při konfiguraci hello profileru hello následující aktualizace jsou vytvářeny nastavení toohello webové aplikace. Můžete je provést sami ručně, pokud vaše prostředí vyžadovat, například pokud vaše aplikace běží v Azure App Service prostředí (App Service Environment):

1. V okně Ovládací prvek webové aplikace hello otevřete nastavení.
2. Nastavit "rozhraní .net Framework, verze" toov4.6.
3. Nastavit tooOn "Always On".
4. Přidání nastavení aplikace "__APPINSIGHTS_INSTRUMENTATIONKEY__" a sadu hello hodnota toohello používá stejný klíč instrumentace hello SDK.
5. Otevřete pokročilé nástroje.
6. Klikněte na tlačítko "Jít" tooopen hello Kudu webu.
7. Hello Kudu webu vyberte "Rozšíření lokality".
8. Nainstalujte "__Application Insights__" z galerie.
9. Restartujte hello webové aplikace.

## <a id="aspnetcore"></a>Podpora jádra ASP.NET

Aplikace ASP.NET Core musí tooinstall 2.1.0-beta6 balíček Microsoft.ApplicationInsights.AspNetCore Nuget nebo vyšší toowork s hello profileru. Po 27/6/2017 už podporujeme hello nižší verze.

## <a name="next-steps"></a>Další kroky

* [Práce s Application Insights v sadě Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png
