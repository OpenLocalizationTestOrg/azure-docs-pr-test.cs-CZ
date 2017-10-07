---
title: "vzorkování aaaTelemetry ve službě Azure Application Insights | Microsoft Docs"
description: Jak tookeep hello svazku telemetrie pod kontrolou.
services: application-insights
documentationcenter: windows
author: vgorbenko
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bwren
ms.openlocfilehash: e19c350d0a5f16736c100322a9922fcfbf5d7b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sampling-in-application-insights"></a>Vzorkování ve službě Application Insights


Vzorkování je funkce v [Azure Application Insights](app-insights-overview.md). Je, že hello doporučené způsob tooreduce telemetrie provozu a úložiště, při zachování statisticky správné analýzy dat aplikací. Filtr Hello vybere položky, které se vztahují, tak, aby můžete procházet mezi položkami při provádění diagnostických šetření.
Když metriky počty uvádíme tooyou hello portálu, jsou renormalized tootake účet hello vzorkování, toominimize žádný vliv na hello statistiky.

Vzorkování snižuje náklady na provoz a data a umožňuje vyhnout se omezení.

## <a name="in-brief"></a>Stručný postup:
* Vzorkování uchovává 1 v  *n*  zaznamenává a zahodí hello rest. Například je může zachovat události 1 v 5, vzorkovací frekvenci 20 %. 
* Vzorkování se stane automaticky, když vaše aplikace odešle velké množství telemetrických dat, v aplikacích pro ASP.NET web server.
* Můžete také nastavit vzorkování ručně, buď v hello portál hello ceny stránky; nebo v hello sady SDK technologie ASP.NET v souboru .config hello, snižte tooalso hello síťový provoz.
* Pokud protokolu vlastní události a chcete toomake se, že sadu událostí, které je buď uchovávají nebo zrušených společně, ujistěte se, že budou mít stejnou hodnotu OperationId hello.
* vzorkování Hello dělitel  *n*  údajně všechny záznamy v hello vlastnost `itemCount`, která v hledání se zobrazí pod hello popisný název "počtu žádostí o" nebo "počet událostí". Pokud vzorkování není v provozu se `itemCount==1`.
* Pokud píšete analytické dotazy, měli byste [vzít v úvahu vzorkování](app-insights-analytics-tour.md#counting-sampled-data). Konkrétně místo jednoduše počítání záznamy, měli byste použít `summarize sum(itemCount)`.

## <a name="types-of-sampling"></a>Typy vzorkování
Existují tři metody vzorkování alternativní:

* **Adaptivního vzorkování** automaticky přizpůsobí hello objem telemetrická data odesílaná z hello SDK v aplikaci ASP.NET. Výchozí hodnota je ze sady SDK v 2.0.0-beta3. Aktuálně dostupné pro ASP.NET serverové pouze telemetrie. 
* **Míry vzorkování** snižuje objem hello telemetrická data odesílaná ze serveru ASP.NET a z prohlížečů uživatelů. Můžete nastavit rychlost hello. Hello klient a server bude synchronizovat jejich vzorkování tak, že v hledání, mohou procházet mezi zobrazení související stránky a požadavky.
* **Přijímání vzorkování** funguje v hello portálu Azure. Zahodí některé hello telemetrická data přenášená z vaší aplikace, který nastavíte. Není omezit přenos telemetrie, ale umožňuje udržovat v rámci měsíční kvóta. Hello velkých výhod přijímání vzorkování je, že můžete ho nastavit bez opětovného nasazení aplikace a funguje jednotně pro všechny servery a klienty. 

Pokud Adaptivní nebo pevné míry vzorkování v operaci, přijímání vzorkování je zakázána.

## <a name="ingestion-sampling"></a>Přijímání vzorkování
Tato forma vzorkování funguje v okamžiku hello kde hello telemetrie z vaší webový server, prohlížečů a zařízení dosáhne koncového bodu služby Application Insights hello. I když nesnižuje přenosy hello telemetrie z vaší aplikace, je zkrátit dobu, kterou hello zpracovat a zachovává (a účtovat poplatek za) pomocí Application Insights.

Tento typ vzorkování použijte, pokud vaše aplikace často prochází přes jeho měsíční kvóta a nemáte možnost hello některou z hello typy vzorkování na základě sady SDK. 

Nastavit hello vzorkovací frekvenci v hello kvóty a cena okno:

![V okně Přehled aplikace hello klikněte na tlačítko Nastavení, kvóty a ukázky, pak vyberte vzorkovací frekvenci a kliknutím na tlačítko Aktualizovat.](./media/app-insights-sampling/04.png)

Podobně jako ostatní typy vzorkování zachová hello algoritmus telemetrii související položky. Například pokud jste probíhá kontrola hello telemetrie ve vyhledávání, bude možné žádost hello toofind související tooa určité výjimky. Metrika počítá jako je například rychlost požadavků a rychlost výjimka správně zachovány.

Datové body, které jsou zrušených vzorkování nejsou k dispozici ve všech funkcí, Application Insights, jako [průběžné exportovat](app-insights-export-telemetry.md).

Přijímání vzorkování nepracuje při vzorkování Adaptivní nebo pevnou sazbou na základě sady SDK je v provozu. Pokud hello vzorkovací frekvenci na hello SDK je menší než 100 %, pak hello přijímání vzorkovací frekvenci, který nastavíte ignorována.

> [!WARNING]
> Hello hodnoty zobrazené na dlaždici hello označuje hello hodnotu, která nastavíte pro přijímání vzorkování. Hello skutečné vzorkovací frekvenci nepředstavuje Pokud vzorkování SDK je v provozu.
> 
> 

## <a name="adaptive-sampling-at-your-web-server"></a>Adaptivního vzorkování na webovém serveru
Adaptivního vzorkování je k dispozici pro hello Application Insights SDK pro aplikace ASP.NET v 2.0.0-beta3 nebo novější a je ve výchozím nastavení povolené. 

Adaptivního vzorkování ovlivňuje hello objem telemetrická data odesílaná ze serveru vaší webové aplikace toohello služby Application Insights. Hello svazku se upraví automaticky tookeep v rámci zadaný maximální rychlost přenosu.

Ho nepracuje na nízkou svazky telemetrie, takže aplikace při ladění nebo nebude mít vliv na web s nízkou využití.

tooachieve hello cílový svazek, některé hello telemetrii vygenerovanou se zahodí. Ale stejně jako jiné typy vzorkování hello algoritmus zachová související telemetrii položky. Například pokud jste probíhá kontrola hello telemetrie ve vyhledávání, bude možné žádost hello toofind související tooa určité výjimky. 

Metrika spočítá, jako jsou frekvence výjimky a rychlost požadavků se upravenou toocompensate pro hello vzorkování míru, tak, aby se v Průzkumníku metrika zobrazit přibližně správné hodnoty.

**Aktualizace vašeho projektu NuGet** nejnovější balíčky toohello *předběžné verze* verzi Application Insights: hello projekt v Průzkumníku řešení klikněte pravým tlačítkem, zvolte spravovat balíčky NuGet, zkontrolujte **zahrnout předběžné verze** a vyhledejte Microsoft.ApplicationInsights.Web. 

V [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), můžete upravit několik parametrů v hello `AdaptiveSamplingTelemetryProcessor` uzlu. Hello obrázky, zobrazí se výchozí hodnoty hello:

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`
  
    Hello cíl rychlost, kterou hello adaptivní algoritmus cílem je pro **na každém hostiteli serveru**. Pokud vaše webová aplikace běží na mnoho hostitelů, snížit tak jako tooremain v rámci vaší cílový počet přenosů dat na portál Application Insights hello tuto hodnotu.
* `<EvaluationInterval>00:00:15</EvaluationInterval>` 
  
    Hello interval, ve které hello se znovu zhodnotí aktuální rychlost telemetrie. Hodnocení je provést, protože klouzavého průměru. Tooshorten může použít tento interval, pokud je vaše telemetrie zodpovědná toosudden shluky.
* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`
  
    V případě změny hodnot procento vzorkování, jak brzy po jsou jsme povoleno toolower vzorkování procento znovu toocapture méně data.
* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`
  
    V případě změny hodnot procento vzorkování, jak brzy po jsou jsme povoleno tooincrease vzorkování procento znovu toocapture další data.
* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`
  
    Protože vzorkování procento liší, jaká je minimální hodnota hello tooset jsme máte oprávnění.
* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`
  
    Protože vzorkování procento liší, jaká je maximální hodnota hello tooset jsme máte oprávnění.
* `<MovingAverageRatio>0.25</MovingAverageRatio>` 
  
    V hello výpočet hello klouzavý průměr přiřazena hello váhy toohello poslední hodnota. Použijte stejné tooor hodnotu menší než 1. Menší hodnoty měnit hello algoritmus méně reaktivní toosudden.
* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`
  
    Hello hodnotu přiřazenou při aplikace hello byl spuštěn. Není to snížit při ladění. 

* `<ExcludedTypes>Trace;Exception</ExcludedTypes>`
  
    Středníkem oddělené seznam typů, které nechcete toobe vzorků. Rozpoznat typy jsou: závislost, události, výjimky, stránkové zobrazení, požadavku, trasování. Všechny instance hello zadané typy přenášejí; jsou odebírána data Hello typy, které nejsou zadané.

* `<IncludedTypes>Request;Dependency</IncludedTypes>`
  
    Středníky oddělený seznam typů, které chcete toobe vzorků. Rozpoznat typy jsou: závislost, události, výjimky, stránkové zobrazení, požadavku, trasování. Hello zadané typy jsou odebírána data; všechny instance z hello je vždy přenesen jiné typy.


**tooswitch vypnout** adaptivního vzorkování, odeberte hello AdaptiveSamplingTelemetryProcessor uzlu z applicationinsights-config.

### <a name="alternative-configure-adaptive-sampling-in-code"></a>Alternativní: Konfigurace adaptivního vzorkování v kódu
Místo úpravě vzorkování v souboru .config hello, můžete použít kód. To vám umožní toospecify funkce zpětného volání, která je volána vždy, když se znovu zhodnotí hello vzorkovací frekvenci. Můžete to použít, například toofind na jaké vzorkovací frekvenci používá.

Odebrat hello `AdaptiveSamplingTelemetryProcessor` ze souboru .config hello.

*C#*

```C#

    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.Channel.Implementation;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var adaptiveSamplingSettings = new SamplingPercentageEstimatorSettings();

    // Optional: here you can adjust hello settings from their defaults.

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;

    builder.UseAdaptiveSampling(
         adaptiveSamplingSettings,

        // Callback on rate re-evaluation:
        (double afterSamplingTelemetryItemRatePerSecond,
         double currentSamplingPercentage,
         double newSamplingPercentage,
         bool isSamplingPercentageChanged,
         SamplingPercentageEstimatorSettings s
        ) =>
        {
          if (isSamplingPercentageChanged)
          {
             // Report hello sampling rate.
             telemetryClient.TrackMetric("samplingPercentage", newSamplingPercentage);
          }
      });

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Další informace o telemetrie procesory](app-insights-api-filtering-sampling.md#filtering).)

<a name="other-web-pages"></a>

## <a name="sampling-for-web-pages-with-javascript"></a>Vzorkování pro webové stránky v jazyce JavaScript
Můžete nakonfigurovat webové stránky pro – míra vzorkování z jakéhokoli serveru. 

Pokud jste [konfigurace hello webové stránky pro službu Application Insights](app-insights-javascript.md), hello fragment, kterou můžete získat z portálu služby Application Insights hello upravit. (V aplikacích ASP.NET hello fragment obvykle bude v _Layout.cshtml.)  Vložit řádek jako `samplingPercentage: 10,` před klíč instrumentace hello:

    <script>
    var appInsights= ... 
    }({ 


    // Value must be 100/N where N is an integer.
    // Valid examples: 50, 25, 20, 10, 5, 1, 0.1, ...
    samplingPercentage: 10, 

    instrumentationKey:...
    }); 

    window.appInsights=appInsights; 
    appInsights.trackPageView(); 
    </script> 

Procento hello vzorkování zvolte procentuální hodnotu, která je zavřít too100/N, kde N je celé číslo.  Aktuálně vzorkování nepodporuje ostatní hodnoty.

Pokud povolíte také – míra vzorkování na hello server, hello klienty a server bude synchronizovat tak, aby toto, v hledání mezi můžete procházet zobrazení související stránky a požadavky.

## <a name="fixed-rate-sampling-for-aspnet-web-sites"></a>Míry vzorkování pro weby technologie ASP.NET
Opravené míry vzorkování omezuje hello přenosy odesílané z webového serveru a webových prohlížečů. Na rozdíl od adaptivního vzorkování, snižuje telemetrická data s pevnou sazbou, které jste se rozhodli. Také synchronizuje hello klient a server vzorkování tak, aby se zachovají související položky – například tak, aby se podíváte na zobrazení stránky ve vyhledávání, budete moci najít související požadavku.

algoritmus vzorkování Hello zachová související položky. Pro každý požadavek HTTP událostí, ho a její související události jsou buď zrušených nebo přenosu. 

V Průzkumníku metrik sazby například požadavku a výjimky počty násobí toocompensate faktor pro hello vzorkovací frekvenci, aby byly přibližně správné.

1. **Aktualizovat balíčky NuGet projektu na** toohello nejnovější *předběžné verze* verzi Application Insights. Klikněte pravým tlačítkem na projekt hello v Průzkumníku řešení, zvolte spravovat balíčky NuGet, zkontrolujte **zahrnout předběžné verze** a vyhledejte Microsoft.ApplicationInsights.Web. 
2. **Zakázat adaptivního vzorkování**: V [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), odeberte, nebo komentář hello `AdaptiveSamplingTelemetryProcessor` uzlu.
   
    ```xml
   
    <TelemetryProcessors>
   
    <!-- Disabled adaptive sampling:
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    -->

    ```

1. **Povolí modul – míra vzorkování hello.** Přidejte tento fragment příliš[souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):
   
    ```XML
   
    <TelemetryProcessors>
     <Add  Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
   
      <!-- Set a percentage close too100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
      <SamplingPercentage>10</SamplingPercentage>
      </Add>
    </TelemetryProcessors>
   
    ```

> [!NOTE]
> Procento hello vzorkování zvolte procentuální hodnotu, která je zavřít too100/N, kde N je celé číslo.  Aktuálně vzorkování nepodporuje ostatní hodnoty.
> 
> 

### <a name="alternative-enable-fixed-rate-sampling-in-your-server-code"></a>Alternativní: Povolte-míry vzorkování v serverovém kódu
Místo v souboru .config hello nastavení parametru vzorkování hello, můžete použít kód. 

*C#*

```C#

    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.GetTelemetryProcessorChainBuilder();
    builder.UseSampling(10.0); // percentage

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Další informace o telemetrie procesory](app-insights-api-filtering-sampling.md#filtering).)

## <a name="when-toouse-sampling"></a>Když toouse vzorkování?
Pokud používáte hello 2.0.0-beta3 verze sady SDK technologie ASP.NET je automaticky povolen adaptivního vzorkování nebo novější. Bez ohledu na to, jaké používáte verze sady SDK můžete přijímat vzorkování (v našem server).

Nepotřebujete vzorkování pro většinu aplikací malé a střední velikosti. shromažďování dat pro všechny aktivity uživatele Hello nejužitečnější diagnostické informace a nejpřesnější statistiky jsou získat. 

hlavní výhody Hello vzorkování jsou:

* Aplikace Statistika vyřazuje ("omezení") dat body služby když vaše aplikace odesílá velmi vysoká míra telemetrie v krátkém časový interval. 
* tookeep v rámci hello [kvóty](app-insights-pricing.md) datových bodů pro cenovou úroveň. 
* tooreduce síťový provoz z kolekce hello telemetrie. 

### <a name="which-type-of-sampling-should-i-use"></a>Jaký typ vzorkování mám použít?
**Použijte přijímání vzorkování, pokud:**

* Často projít vaše měsíční kvóta telemetrie.
* Používáte verzi hello SDK, která nepodporuje vzorkování – například, hello sady Java SDK nebo ASP.NET verze starší než 2.
* Vám mnoho telemetrie z webových prohlížečů uživatelů.

**Použijte – míra vzorkování, pokud:**

* Používáte-li hello Application Insights SDK pro ASP.NET web services verze 2.0.0 nebo novější, a
* Chcete, aby synchronizovaná vzorkování mezi klientem a serverem, takže když jste příčin události v [vyhledávání](app-insights-diagnostic-search.md), mohou procházet mezi souvisejícími událostmi hello klienta a serveru, například zobrazení stránky a požadavky http.
* Jste si jisti, procenta hello odpovídající vzorkování pro vaši aplikaci. Měl by být dostatečně vysoký tooget přesných metrik, ale níže hello rychlost, jakou překročí kvótu cenovou a hello omezení. 

**Použijte adaptivního vzorkování:**

Jinak doporučujeme, abyste adaptivního vzorkování. Tato možnost je povolena ve výchozím nastavení v serveru technologie ASP.NET hello SDK verze 2.0.0-beta3 nebo novější. Nesnižuje provoz až na určité minimální míru, takže ho nebude mít vliv na použití nízké lokality.

## <a name="how-do-i-know-whether-sampling-is-in-operation"></a>Jak zjistím, zda je vzorkování v operaci?
toodiscover hello skutečné vzorkování míru bez ohledu na to, kde bylo použito, použijte [Analytics dotazu](app-insights-analytics.md) jako je tato:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

V každé uchovávají záznam, `itemCount` označuje hello původní záznamy, které představuje, rovna too1 + hello počet předchozích zrušených záznamů. 

## <a name="how-does-sampling-work"></a>Jak funguje vzorkování?
-Rate a adaptivního vzorkování jsou funkce hello SDK v technologii ASP.NET verze z 2.0.0 a vyšší. Přijímání vzorkování je funkce hello služby Application Insights a může být v rámci operace, pokud hello SDK neprovádí vzorkování. 

algoritmus vzorkování Hello, rozhodne se které toodrop položky telemetrie a které těch, které jsou tookeep (jestli je v hello SDK nebo ve službě Application Insights hello). rozhodnutí vzorkování Hello je založeno na několik pravidel, která zaměřte toopreserve všechny body vzájemně souvisejících dat beze změn, udržování diagnostiky prostředí ve službě Application Insights, který je možné použít a spolehlivé i s menší datové sady. Například pokud vaše aplikace pro chybné žádosti odesílá další telemetrické položky (například výjimky a trasování, přihlášení z této žádosti), vzorkování nebude rozdělit tento požadavek a další telemetrií. Je buď udržuje nebo je zahodí všechny společně. Výsledkem je když se podíváte na podrobnosti požadavku hello ve službě Application Insights, vždy uvidíte hello žádost spolu s jeho položky přidružené telemetrie. 

Pro aplikace, které definují "user" (který je nejčastější webové aplikace), hello vzorkování rozhodnutí vycházet z hello hash hello id uživatele, což znamená, že všechny telemetrická pro konkrétní uživatele, je buď zachovaná, nebo vyřadit. Pro hello typy aplikací, které nejsou definovat uživatele (například webové služby) hello vzorkování rozhodnutí podle id operace hello hello požadavku. Pro hello telemetrie položky, které se mají ani id uživatele ani operaci nastavit (pro položky telemetrie příklad nahlásila asynchronní vláken s žádný kontext http) je nakonec vzorkování jednoduše zaznamená procento položek telemetrie každého typu. 

Během zobrazení telemetrie back tooyou, hello hello Application Insights service upraví hello metriky ve stejné procento vzorkování, která byla použita v době hello kolekce, toocompensate pro hello chybějící datové body. Proto při prohlížení hello telemetrii ve službě Application Insights, uživatelé hello se zobrazuje statisticky správné aproximace, které jsou velmi podobné toohello reálná čísla.

Hello přesnost hello aproximace do značné míry závisí na procento hello nakonfigurované vzorkování. Navíc hello přesnost zvyšuje pro aplikace, které zpracovávají velké množství obecně podobné požadavky od velký počet uživatelů. Na dobrý den druhé straně, pro aplikace, které nefungují s výrazném zatížení, vzorkování není potřeba, protože tyto aplikace mohou zasílat obvykle jejich telemetrických dat při zachování v rámci kvóty hello, aniž by došlo ke ztrátě dat z omezení. 

Všimněte si, že Application Insights pro tyto typy není ukázkové metriky a relací typy telemetrických dat, protože snížení hello přesnost může být vysoce nežádoucí. 

### <a name="adaptive-sampling"></a>Adaptivního vzorkování
Adaptivního vzorkování přidá součásti tohoto monitorování hello aktuální rychlost přenosu z hello SDK a upraví hello vzorkování procento tootry toostay v rámci maximální rychlost přenosu cíl hello. Hello úpravu jsou přepočítána v pravidelných intervalech a vychází z o pohyblivý průměr hello odchozí rychlost přenosu.

## <a name="sampling-and-hello-javascript-sdk"></a>Vzorkování a hello JavaScript SDK
Hello straně klienta (JavaScript) SDK účastní – míra vzorkování ve spojení s hello na straně serveru SDK. Hello instrumentovány stránky pouze odesílání telemetrických dat na straně klienta z hello stejným uživatelům, pro které hello serverové provedené své rozhodnutí příliš "ukázkové ve." Tato logika je navrženou toomaintain integritu uživatelské relace mezi tisk klienta a serveru stranách. V důsledku toho z libovolné položky konkrétní telemetrii ve službě Application Insights můžete najít všechny ostatní položky telemetrie pro tohoto uživatele nebo relace. 

*Moje klientské a serverové telemetrie nezobrazovat koordinované ukázky, jak se uvádí výše.*

* Povolte – míra vzorkování serveru a klienta.
* Ujistěte se, že verze sady SDK hello je 2.0 nebo novější.
* Zkontrolujte, že nastavíte hello stejné vzorkování procento v hello klient i server.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
*Proč není vzorkování jednoduchý "shromažďování X procento každého typu telemetrie"?*

* Při velmi vysokou přesnost v metriky aproximace by poskytnout tento přístup vzorkování, by rozdělit možnost toocorrelate diagnostických dat na uživatele, relace a požadavku, což je důležité pro diagnostiku. Proto vzorkování funguje lépe s "shromažďovat všechny telemetrie položky pro X procento uživatelů aplikace" nebo "shromažďování všech telemetrie X Procento požadavků aplikace" logiku. Pro položky telemetrie hello nejsou přidružené hello požadavků (jako je například asynchronní zpracování na pozadí) patří hello zpět je příliš "shromažďování X procento všechny položky pro každý typ telemetrie." 

*Časem změnit procento vzorkování hello?*

* Ano, adaptivního vzorkování postupně změní hello vzorkování procento, podle hello aktuálně zjištěnými svazku hello telemetrie.

*Pokud se používá – míra vzorkování, jak lze zjistit, které vzorkování procento bude fungovat hello nejvhodnější pro mojí aplikaci?*

* Jedním ze způsobů je toostart s adaptivního vzorkování, podívejte se, co ohodnotit vyrovná na (viz výše otázku hello), a potom přepínač toofixed – míra vzorkování pomocí tohoto kurzu. 
  
    Jinak máte tooguess. Analýza vašeho aktuálního využití telemetrie v AI, sledovat žádné omezení, ke kterým dochází a odhad hello objem shromážděných hello telemetrie. Tyto tři vstupy, společně s vybranou cenovou úroveň, navrhnout, kolik můžete chtít tooreduce hello objem shromážděných hello telemetrie. Však může zvýšit počet hello vaši uživatelé nebo jiné posunutí ve svazku hello telemetrie neplatným odhad.

*Co se stane, když je možné nakonfigurovat vzorkování procento příliš nízko?*

* Procento příliš nízkou vzorkování (over-aggressive vzorkování) snižuje hello přesnost aproximace hello, když se pokusí toocompensate hello vizualizace dat hello pro snížení objemu dat hello Application Insights. Navíc diagnostiky prostředí může být negativně ovlivněn, jako některé z hello zřídka selhání nebo se vzorkovat lze pomalé požadavky.

*Co se stane, pokud je možné nakonfigurovat vzorkování procento příliš vysoká.*

* Konfigurace příliš vysoké vzorkování procento (ne agresivní dostatečně) výsledky v na nedostatečná snížení objemu hello hello shromažďovat telemetrická data. Přesto můžete setkat telemetrie, které toothrottling a hello náklady na používání Application Insights může být vyšší než můžete naplánovat kvůli toooverage poplatky související s ztráty dat.

*Na platformách, které lze použít vzorkování?*

* Přijímání vzorkování může dojít automaticky pro všechny telemetrie výše určité svazku, pokud hello SDK neprovádí vzorkování. To bude fungovat, například pokud vaše aplikace používá serveru Java, nebo pokud používáte starší verzi hello sady SDK technologie ASP.NET.
* Pokud používáte sady SDK technologie ASP.NET verze 2.0.0 a vyšší (hostované v Azure nebo na vlastní server), můžete získat adaptivního vzorkování ve výchozím nastavení, ale můžete přepnout toofixed míry, jak je popsáno výše. S pevnou sazbou vzorkování hello prohlížeče SDK automaticky synchronizuje toosample související události. 

*Existují určité výjimečných události chci, aby toosee. Načtení je po hello vzorkování modulu?*

* Inicializujte samostatnou instanci TelemetryClient s novou TelemetryConfiguration (ne hello výchozí aktivní). Pomocí této toosend výjimečných událostí.

## <a name="next-steps"></a>Další kroky
* [Filtrování](app-insights-api-filtering-sampling.md) můžete zadat další přísnou kontrolu co pošle váš SDK.

