---
title: "aaaAzure ladicí program snímku Application Insights pro aplikace .NET | Microsoft Docs"
description: "Ladění snímky jsou shromažďovány automaticky, pokud jsou výjimky vyvolány v produkční aplikace .NET"
services: application-insights
documentationcenter: 
author: qubitron
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: bwren
ms.openlocfilehash: f0173a752b5795d934fbab1bd53eb077433edc90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>Ladění snímků výjimky v aplikacích .NET

Když dojde k výjimce, může automaticky shromažďovat snímku ladění z provozu webové aplikace. Hello snímku ukazuje stav hello zdrojový kód a došlo k proměnné v hello chvíli hello výjimka. Hello ladicí program snímku (preview) v [Azure Application Insights](app-insights-overview.md) monitoruje výjimka telemetrie z vaší webové aplikace. Shromažďuje snímky na vaše horní vyvolání výjimky, tak, aby hello informace, které potřebujete toodiagnose problémy v produkčním prostředí. Zahrnout hello [balíček NuGet kolekce snímku](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) ve vaší aplikaci a volitelně nakonfigurujte parametry kolekce v [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Snímky zobrazí na [výjimky](app-insights-asp-net-exceptions.md) hello portálu Application Insights.

Můžete zobrazit ladění snímky v zásobníku volání hello portálu toosee hello a prohlédnout proměnné v každé rámce zásobníku volání. tooget výkonnější ladění zkušeností se zdrojovým kódem open snímky s Visual Studio Enterprise 2017 podle [stahování hello ladicí program snímku rozšíření pro Visual Studio](https://aka.ms/snapshotdebugger).

Snímek kolekce je k dispozici pro:
* Aplikace rozhraní .NET framework a ASP.NET spuštění rozhraní .NET Framework 4.5 nebo novější.
* Aplikace rozhraní .NET 2.0 core a ASP.NET Core 2.0 v systému Windows.

### <a name="configure-snapshot-collection-for-aspnet-applications"></a>Konfigurace shromažďování snímků pro aplikace ASP.NET

1. [Povolit Application Insights ve vaší webové aplikaci](app-insights-asp-net.md), pokud jste ho ještě neučinili.

2. Zahrnout hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) balíček NuGet v aplikaci. 

3. Zkontrolujte hello výchozí možnosti, které hello balíček přidat příliš[souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- hello default is true, but you can disable Snapshot Debugging by setting it toofalse -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this tootrue. -->
        <!-- DeveloperMode is a property on hello active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need toosee an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>5</ThresholdForSnapshotting>
        <!-- hello maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- hello maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often tooreset problem counters. -->
        <ProblemCounterResetInterval>06:00:00</ProblemCounterResetInterval>
        <!-- hello maximum number of snapshots allowed in one minute. -->
        <SnapshotsPerMinuteLimit>2</SnapshotsPerMinuteLimit>
        <!-- hello maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>50</SnapshotsPerDayLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. Snímky se shromažďují pouze na výjimky, které jsou hlášené tooApplication statistiky. V některých případech (například starší verze platformy .NET hello), může být příliš nutné[konfigurace shromažďování výjimek](app-insights-asp-net-exceptions.md#exceptions) toosee výjimky se snímky hello portálu.


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a>Konfigurace shromažďování snímků pro technologii ASP.NET 2.0 základní aplikace

1. [Povolit Application Insights ve vaší webové aplikaci ASP.NET Core](app-insights-asp-net-core.md), pokud jste ho ještě neučinili.

2. Zahrnout hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) balíček NuGet v aplikaci.

3. Upravit hello `ConfigureServices` metoda ve vaší aplikaci `Startup` třídy tooadd hello snímek kolekce telemetrie procesoru. Hello kódu, které byste měli přidat závisí na hello odkazované verzi hello balíček Microsoft.ApplicationInsights.ASPNETCore NuGet.

   Pro Microsoft.ApplicationInsights.AspNetCore 2.1.0 přidejte:
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       // This method is called by hello runtime. Use it tooadd services toohello container.
       public void ConfigureServices(IServiceCollection services)
       {
           services.AddSingleton<Func<ITelemetryProcessor, ITelemetryProcessor>>(next => new SnapshotCollectorTelemetryProcessor(next));
           // TODO: Add any other services your application needs here.
       }
   }
   ```

   Pro Microsoft.ApplicationInsights.AspNetCore 2.1.1 přidejte:
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
       {
           public ITelemetryProcessor Create(ITelemetryProcessor next) =>
               new SnapshotCollectorTelemetryProcessor(next);
       }

       // This method is called by hello runtime. Use it tooadd services toohello container.
       public void ConfigureServices(IServiceCollection services)
       {
            services.AddSingleton<ITelemetryProcessorFactory>(new SnapshotCollectorTelemetryProcessorFactory());
           // TODO: Add any other services your application needs here.
       }
   }
   ```

### <a name="configure-snapshot-collection-for-other-net-applications"></a>Konfigurace shromažďování snímků pro jiné aplikace rozhraní .NET

1. Pokud vaše aplikace není instrumentována již s nástrojem Application Insights, začít [povolení Application Insights a klíč instrumentace hello nastavení](app-insights-windows-desktop.md).

2. Přidat hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) balíček NuGet v aplikaci.

3. Snímky se shromažďují pouze na výjimky, které jsou hlášené tooApplication statistiky. Může být nutné toomodify tooreport váš kód je. zpracování kódu výjimek Hello závisí na hello struktura vaší aplikace, ale zde je příklad:
    ```C#
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle hello request.
        }
        catch (Exception ex)
        {
            // Report hello exception tooApplication Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow hello exception if desired.
        }
   }
    ```
    
## <a name="grant-permissions"></a>Udělení oprávnění

Vlastníci hello předplatného Azure můžete prohlédnout snímky. Ostatní uživatelé musí udělit oprávnění vlastníka.

toogrant oprávnění, přiřaďte hello `Application Insights Snapshot Debugger` toousers role, který bude kontrolovat snímky. Tato role může být přiřazena tooindividual uživatelé nebo skupiny s vlastníky předplatného pro hello cílový prostředek Application Insights nebo jeho skupiny prostředků nebo předplatného.

1. Otevřete okno hello řízení přístupu (IAM).
1. Klikněte na tlačítko Přidat + hello.
1. Hello role rozevíracím seznamu vyberte Application Insights snímku ladicí program.
1. Vyhledejte a zadejte název pro uživatele tooadd hello.
1. Klikněte na tlačítko Uložit hello, tooadd role toohello uživatele hello.


[!IMPORTANT]
    Snímky mohou obsahovat osobní a dalších citlivých informací v proměnné a parametr hodnoty.

## <a name="debug-snapshots-in-hello-application-insights-portal"></a>Ladění snímky portálu Application Insights hello

Pokud snímek je k dispozici pro danou výjimky nebo ID problému, **otevřít ladění snímek** na hello se zobrazí tlačítko [výjimka](app-insights-asp-net-exceptions.md) hello portálu Application Insights.

![Tlačítko Otevřít ladění snímku na výjimky](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

V zobrazení ladění snímku hello uvidíte zásobník volání a podokně proměnné. Když vyberete hello volání rámce zásobníku v podokně Zásobník volání hello, můžete zobrazit místní proměnné a parametry pro tuto funkci volání v podokně proměnné hello.

![Zobrazení ladění snímku hello portálu](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

Snímky mohou obsahovat citlivé informace a ve výchozím nastavení nejsou viditelná. tooview snímky, musí mít hello `Application Insights Snapshot Debugger` tooyou přiřazenou roli.

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a>Ladění snímky s Visual Studio 2017 Enterprise
1. Klikněte na tlačítko hello **stáhnout snímku** tlačítko toodownload `.diagsession` souboru, který lze otevřít v aplikaci Visual Studio Enterprise 2017. 

2. tooopen hello `.diagsession` souboru, je nutné nejprve [stáhněte a nainstalujte hello ladicí program snímku rozšíření pro Visual Studio](https://aka.ms/snapshotdebugger).

3. Po otevření souboru snímku hello, zobrazí se stránka minimální výpis ladění hello v sadě Visual Studio. Klikněte na tlačítko **ladění spravovaného kódu** toostart ladění hello snímku. Hello snímku otevře toohello řádek kódu, kde byla vyvolána výjimka hello, takže můžete ladit, aktuální stav procesu hello hello.

    ![Zobrazení ladění snímku v sadě Visual Studio](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

stažený snímku Hello obsahuje symbol soubory, které nebyly nalezeny na vašem webovém serveru aplikace. Tyto soubory symbolů jsou požadované tooassociate data snímku se zdrojovým kódem. Pro aplikace služby App Service zkontrolujte že nasazení symbol tooenable při publikování webové aplikace.

## <a name="how-snapshots-work"></a>Jak fungují snímky

Když se aplikace spustí, proces osoba samostatné snímku je vytvořen, který monitoruje vaše aplikace pro požadavky na snímku. Pokud se požaduje snímku, stínové kopie hello spuštěn proces se provádí v přibližně 10 minut too20. proces stínové Hello pak analýzy a snímku se vytvoří během procesu hlavní hello pokračuje toorun a sloužit toousers provoz. Hello snímek je pak nahrané tooApplication Statistika společně s všechny relevantní symbolu (.pdb) soubory, které jsou potřeba tooview hello snímku.

## <a name="current-limitations"></a>Aktuální omezení

### <a name="publish-symbols"></a>Publikování symboly
Hello snímku ladicí program vyžaduje soubory symbolů na hello provozním serveru toodecode proměnné a tooprovide zkušenosti s laděním v sadě Visual Studio. Pokud tato možnost publikuje tooApp služby Hello 15.2 verzi Visual Studio 2017 publikuje symboly pro verzi sestavení ve výchozím nastavení. V předchozích verzích, potřebujete následující hello tooadd profilu publikování řádku tooyour `.pubxml` tak, aby symboly jsou publikovány v režimu vydání:

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

Pro Azure Compute a dalších typů ujistit, že soubory symbolů hello hello stejné složce dll hlavní aplikace hello (obvykle `wwwroot/bin`) nebo je k dispozici v aktuální cestě hello.

### <a name="optimized-builds"></a>Optimalizované sestavení
V některých případech místní proměnné nelze zobrazit, v sestavení pro vydání z důvodu optimalizace, které se použijí během procesu sestavení hello.

## <a name="troubleshooting"></a>Řešení potíží

Tyto tipy pomáhají při řešení problémů s hello snímku ladicí program.

### <a name="verify-hello-instrumentation-key"></a>Ověřte klíč instrumentace hello

Ujistěte se, že používáte klíč instrumentace správné hello v k publikované aplikaci. Application Insights obvykle čte klíč instrumentace hello z soubor ApplicationInsights.config hello. Ověřte, že hodnota hello je text hello stejné jako klíč instrumentace hello hello prostředku Application Insights se zobrazí na portálu hello.

### <a name="check-hello-uploader-logs"></a>Zkontrolujte protokoly osoba hello

Po vytvoření snímku se vytvoří soubor s minimálním (.dmp) na disku. Proces samostatné osoba trvá tento soubor minimální výpis a odesílá, společně s všechny přidružené soubory PDB tooApplication ladicí program Statistika snímku úložiště. Po úspěšném odeslání minimální výpis hello je odstraněn z disku. na disku zůstanou zachovány Hello soubory protokolu pro hello minimální výpis procesu pro načtení. V prostředí služby App Service, můžete najít tyto protokoly v `D:\Home\LogFiles\Uploader_*.log`. Použití hello Kudu Správa lokality pro App Service toofind tyto soubory protokolu.

1. Otevřete aplikaci služby App Service v hello portálu Azure.

2. Vyberte hello **Rozšířené nástroje** okno, nebo vyhledejte **Kudu**.
3. Klikněte na tlačítko **přejděte**.
4. V hello **konzolou pro ladění** rozevíracím seznamu vyberte **CMD**.
5. Klikněte na tlačítko **LogFiles**.

Měli byste vidět alespoň jeden soubor s názvem, který začíná `Uploader_` a `.log` rozšíření. Klikněte na příslušnou ikonu toodownload hello všechny soubory protokolů, nebo je, otevřete v prohlížeči.
Název souboru Hello zahrnuje název počítače hello. Pokud vaše instance služby App Service je hostitelem více než jeden počítač, nejsou samostatné soubory protokolu pro každý počítač. Zjistí-li hello osoba nový soubor s minimálními výpisy, zaznamenává se v souboru protokolu hello. Tady je příklad úspěšné odeslání:

```
MinidumpUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:08.0349846Z
MinidumpUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 329.12 MB
    DateTime=2017-05-25T14:25:16.0145444Z
MinidumpUploader.exe Information: 0 : Upload successful.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2017-05-25T14:25:44.2310982Z
MinidumpUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2017-05-25T14:25:44.5435948Z
MinidumpUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:44.6095821Z
```

V předchozím příkladu hello klíč instrumentace hello je `c12a605e73c44346a984e00000000000`. Tato hodnota musí odpovídat hello klíč instrumentace pro vaši aplikaci.
minimální výpis Hello souvisí s snímku s hello ID `139e411a23934dc0b9ea08a626db16c5`. Toto ID můžete použít novější hello toolocate přidružené telemetrie výjimek ve statistice analýza aplikace.

osoba Hello hledá nové soubory PDB o jednou za 15 minut. Tady je příklad:

```
MinidumpUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\home\site\wwwroot\ for local PDBs.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\local\Temporary ASP.NET Files\root\a6554c94\e3ad6f22\assembly\dl3\81d5008b\00b93cc8_dec5d201 for local PDBs.
    DateTime=2017-05-25T15:11:38.8160276Z
MinidumpUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2017-05-25T15:11:38.8316450Z
MinidumpUploader.exe Information: 0 : Deleted PDB scan marker D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\.pdbscan.
    DateTime=2017-05-25T15:11:38.8316450Z
```

Pro aplikace, které jsou _není_ hostované ve službě App Service, hello osoba protokoly jsou ve hello stejné složce jako minimálním výpisem hello: `%TEMP%\Dumps\<ikey>` (kde `<ikey>` je klíč instrumentace).

### <a name="use-application-insights-search-toofind-exceptions-with-snapshots"></a>Vyhledávání pomocí Application Insights toofind výjimky se snímky

Při vytváření snímku se označí hello vyvolání výjimky s ID snímku. Statistika hlášené tooApplication po telemetrie výjimek hello toto ID snímku je zahrnuta jako vlastní vlastnost. V okně hledání hello Application Insights, můžete najít všechny telemetrická s hello `ai.snapshot.id` vlastní vlastnosti.

1. Vyhledejte prostředek Application Insights tooyour hello portálu Azure.
2. Klikněte na tlačítko **vyhledávání**.
3. Typ `ai.snapshot.id` v hello textového vyhledávacího pole a stiskněte klávesu Enter.

![Vyhledejte telemetrie s ID snímku hello portálu](./media/app-insights-snapshot-debugger/search-snapshot-portal.png)

V případě, že toto hledání nebyly vráceny žádné výsledky, žádné snímky byly hlášené tooApplication Insights pro aplikace v hello vybrané časové rozmezí.

toosearch pro konkrétní snímku ID z hello nástroje odeslání protokolů, zadejte toto ID hello vyhledávacího pole. Pokud nemůžete najít telemetrii pro snímek, který víte, že byl odeslán, postupujte takto:

1. Zkontrolujte, že se díváte na pravém prostředek Application Insights hello kontrolou klíč instrumentace hello.

2. Pomocí hello časové razítko z protokolu nástroje odeslání hello, upravte filtr časové rozmezí hello hello vyhledávání toocover tento časový rozsah.

Pokud stále nevidíte výjimku s tímto ID snímku, nebyl telemetrie výjimek hello hlášené tooApplication statistiky. Tato situace může nastat, pokud vaše aplikace došlo k chybě po trvalo hello snímku, ale před jeho hlášené telemetrie výjimek hello. V takovém případě zkontrolujte protokoly služby App Service v části hello `Diagnose and solve problems` toosee, pokud došlo k neočekávané restartuje nebo neošetřené výjimky.

## <a name="next-steps"></a>Další kroky

* [V kódu nastavit snappoints](https://azure.microsoft.com/blog/snapshot-debugger-for-azure/) tooget snímky bez čekání na výjimku.
* [Diagnostikovat výjimky ve webových aplikacích](app-insights-asp-net-exceptions.md) vysvětluje, jak toomake další výjimky viditelné tooApplication statistiky. 
* [Inteligentní detekce](app-insights-proactive-diagnostics.md) automaticky vyhledá anomálie výkonu.
