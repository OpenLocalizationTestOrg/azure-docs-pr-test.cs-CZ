---
title: "aaaManage cen a datový svazek pro Azure Application Insights | Microsoft Docs"
description: "Správa telemetrie svazků a sledování nákladů ve službě Application Insights."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: bwren
ms.openlocfilehash: 4621c989cd467735aefc48ec9547fcbe1b1ae41b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Správa svazku ceny a data ve službě Application Insights


Ceny pro [Azure Application Insights] [ start] je založena na datový svazek na aplikaci. Nízkou využití během vývoje nebo pro malé aplikaci je pravděpodobně toobe volné, protože není na 1 GB měsíční příspěvek telemetrická data.

Každý prostředek Application Insights je účtován jako samostatná služba a přispívá toohello fakturovaná částka u vašeho předplatného tooAzure.

Existují dva cenovou plány. výchozí plán Hello se nazývá Basic. Pro plán hello Enterprise, který má denní poplatků, ale umožňuje některé další funkce, jako se můžete rozhodnout [průběžné export](app-insights-export-telemetry.md).

Pokud máte dotazy týkající se fungování ceny pro službu Application Insights, klidně volné toopost dotaz v našem [fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights). 

## <a name="hello-price-plans"></a>plány cena Hello

V tématu hello [Application Insights stránce s cenami] [ pricing] pro aktuální ceny ve vaší měny.

### <a name="basic-plan"></a>Základní plán

Základní plán Hello je hello výchozí, když je vytvořen nový prostředek Application Insights a postačí pro většinu zákazníků.

* V hello základní plán, budou se vám účtovat podle objemu dat: počet bajtů přijatých Application Insights telemetrie. Datový svazek se měří jako hello velikost hello nekomprimovaným JSON data balíčku přijatých Application Insights z vaší aplikace.
Pro [tabulková data importovat Analytics](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-import), hello datový svazek se měří jako hello nekomprimovanou velikost souborů odeslaných tooApplication statistiky.  
* Vaše první 1 GB pro každou aplikaci je zdarma, takže pokud jste právě experimentování nebo vývoj, jste pravděpodobně toohave toopay.
* [Živý datový proud metriky](app-insights-live-stream.md) dat není počítá o cenách pro účely.
* [Průběžné Export](app-insights-export-telemetry.md) je k dispozici pro poplatek za navíc za GB v základní plán hello.

### <a name="enterprise-plan"></a>Plán Enterprise

* V plánu podnikového hello vaše aplikace můžete použít všechny funkce hello Application Insights. [Průběžné Export](app-insights-export-telemetry.md) a 

[Protokolu konektor Analytics](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) jsou k dispozici žádné další bezplatně v plánu podnikového hello.
* Platíte za uzlu, který odesílá telemetrická data pro všechny aplikace v plánu podnikového hello. 
 * A *uzlu* je počítač fyzický nebo virtuální server nebo instanci role platforma jako služba, který je hostitelem vaší aplikace.
 * Vývoj pro počítače, klientský prohlížeč a mobilní zařízení nejsou počítají jako uzly.
 * Pokud vaše aplikace obsahuje několik komponent, které odesílají telemetrická data, jako jsou webové služby a pracovní back-end, se počítají samostatně.
 * [Živý datový proud metriky](app-insights-live-stream.md) dat není součet ceny purposes.* napříč předplatné, jsou vaše poplatky na uzel a ne aplikace. Pokud máte pět uzlů odesílat telemetrii pro 12 aplikace a pak hello účtují je pět uzlů.
* I když poplatky jsou v uvozovkách za měsíc, se vám účtovat pouze pro všechny hodinu, ve kterém uzlu odesílá telemetrická data z aplikace. Hello hodinové poplatků je hello v uvozovkách měsíčních poplatků / 744 (hello počtem hodin za měsíc 31 dnů).
* Pro každý uzel zjistil (se hodinách) je zadané přidělení objemu dat 200 MB za den. Přidělení nevyužité dat není přenesené z jeden den toohello Další.
 * Pokud si zvolíte hello cenovou možnost Enterprise, každé předplatné získá denní příspěvek dat na základě hello počtu uzlů odesílání telemetrických dat ze zdroje Application Insights toohello v tomto předplatném. Takže pokud máte 5 uzly odeslání dat celý den, budete mít ve fondu příspěvek ve výši 1 GB použít tooall hello Application Insights prostředky v tomto předplatném. Nezáleží na určitých uzlech odesílání, že více dat než jiné uzly protože hello obsažena data jsou sdílena mezi všechny uzly. Pokud určitý den zobrazí prostředky Application Insights hello víc dat, než je součástí hello denní data přidělení pro toto předplatné, hello za GB Nadlimitní data poplatky. 
 * denní data příspěvek Hello se počítá jako hello počtem hodin za den hello (pomocí UTC), každý uzel odesílá telemetrii dělený 24 časy 200 MB. Takže pokud máte 4 uzly odesílat telemetrii během 15 hello 24 hodin za den hello hello zahrnuty data pro daný den by ((4 x 15) / 24) x 200 MB = 500 MB. Za hello poplatek 2.30 USD za GB za nadměrné využívání dat bude hello zdarma pro rovný 1,15 USD Pokud uzly hello odeslat 1 GB dat daný den.
 * Všimněte si, že denní příspěvek plánu podnikového hello není sdílený s aplikací, pro které jste zvolili možnost Základní hello a nepoužívané povoleného užívání nepřenáší z každodenní. 
* Tady jsou některé příklady určení počet jedinečných uzlů:
| Scénář                               | Celkový počet uzlů na každý den |
|:---------------------------------------|:----------------:|
| aplikace: 1 používá 3 instancí služby Azure App Service a 1 virtuálního serveru | 4 |
| 3 aplikací, které běží na 2 virtuální počítače a hello Application Insights prostředky pro tyto aplikace jsou v hello stejného předplatného a v hello Enterprise plánování | 2 | 
| 4 aplikací, jejíž statistiky aplikace prostředky jsou v hello stejného předplatného. Každá aplikace spouští 2 instance 16 špičku a 4 instancí dobu ve špičce 8. | 13.33 | 
| Cloudové služby s 1 Role pracovního procesu a 1 webové Role, všechny spuštěné instance 2 | 4 | 
| Cluster Service Fabric 5 uzlu službou 50 micro-services, každý micro-spuštěné instance 3 | 5|

* počítání chování Hello přesné uzlu závisí, na kterém je pomocí Application Insights SDK aplikace. 
  * Ve verzích sady SDK, 2.2 a a vyšší, obě hello Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) nebo [sady SDK webové](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) bude každý hostitel aplikace jako uzel sestavy, například hello název počítače pro fyzického serveru a hostitelé virtuálních počítačů nebo hello Název instance v případě hello cloudových služeb.  Hello jen výjimka je aplikace jen pomocí [.NET Core](https://dotnet.github.io/) a hello Application Insights Core SDK, ve kterém případu jenom jeden uzel se ohlásí pro všechny hostitele, protože hello název hostitele není k dispozici. 
  * U starších verzí systému hello SDK hello [sady SDK webové](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) budou chovat jako hello novější verze sady SDK, ale hello [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) oznámí jenom jeden uzel bez ohledu na to hello počet hostitelů skutečné aplikací. 
  * Pokud vaše aplikace používá hello SDK tooset roleInstance tooa vlastní hodnotu, ve výchozím nastavení stejné hodnoty budou použity toodetermine hello počet uzlů. 
  * Pokud používáte novou verzi sady SDK k aplikaci, která se spouští z klientských počítačů nebo mobilních zařízení, je možné, že hello počet uzlů může vrátit číslo, které je moc velké (z hello velký počet klientských počítačů nebo mobilních zařízení). 

### <a name="multi-step-web-tests"></a>Vícekrokové webové testy

Je dalších poplatků pro [vícekrokové webové testy](app-insights-monitor-web-app-availability.md#multi-step-web-tests). Vztahuje se tooweb testy, které provádějí posloupnost akcí. 

Je bezplatná samostatné pro příkaz ping testy jedné stránky. Telemetrie z testů pomocí příkazu ping a testy vícekrokový je účtován spolu s další telemetrie z vaší aplikace.
 
## <a name="operations-management-suite-subscription-entitlement"></a>Nárocích předplatné Operations Management Suite

Jako [nedávno vydal](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/), zákazníci, kteří zakoupili E1 Microsoft Operations Management Suite a E2 jsou možné tooget Application Insights Enterprise jako další komponentu bez dalších poplatků. Jednotky služby Operations Management Suite E1 a E2 konkrétně obsahuje do uzlu too1 nárocích hello Enterprise plánu Application Insights. Jak jsme uvedli výše, obsahuje každý uzel Application Insights až too200 MB dat požity za den (oddělené od analýzy protokolů přijímání dat), s dobou uchování dat 90 dnů bez dalších poplatků. 

> [!NOTE]
> tooensure, že dostanete tento nárok, musíte mít vašich prostředků Application Insights v podniku hello ceny plánu. Toto oprávnění platí pouze jako uzly, takže prostředky Application Insights v základní plán hello nebude potřeba si uvědomit, nějaké výhody. Všimněte si, že tento nárok, nebudou viditelné na hello odhadované náklady zobrazený na funkce hello + cenovou okno. 
>
 
## <a name="review-pricing-plans-and-estimate-costs"></a>Zkontrolujte cenovou plány a odhadnout náklady

Statistika Applicaition umožňuje snadno toounderstand hello cenových plánů, které jsou k dispozici a jaké hello náklady jsou pravděpodobně být založen na poslední vzorce používání. Spuštění pomocí otevírání hello **funkce + cenová** okno v hello prostředek Application Insights hello portálu Azure:

![Zvolte ceny.](./media/app-insights-pricing/01-pricing.png)

**a.** Zkontrolujte datový svazek na měsíc hello. To zahrnuje všechna data hello přijaty a uchovávají (po všech [vzorkování](app-insights-sampling.md) ze serveru a klientské aplikace a z testy dostupnosti.

**b.** Účtována samostatně se provádí [vícekrokové webové testy](app-insights-monitor-web-app-availability.md#multi-step-web-tests). (To neobsahuje testy jednoduché dostupnosti, které jsou součástí hello dat svazku zdarma.)

**c.** Povolte plánu podnikového hello.

**d.** Klikněte na tlačítko prostřednictvím toodata datový svazek tooview možnosti správy pro hello poslední měsíc, nastavit denní zakončení nebo přijímání vzorkování.

Application Insights poplatky budou přidány tooyour Azure faktury. Zobrazí podrobnosti o vašem Azure účtovat pošle na hello fakturace části hello portál Azure nebo v hello [Azure Billing Portal](https://account.windowsazure.com/Subscriptions). 

![V nabídce hello straně zvolte fakturace.](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Rychlost přenosu dat
Existují tři způsoby, ve které hello je omezená svazku odesílat data:

* **Vzorkování:** tento mechanismus lze snížit množství hello telemetrická data odesílaná ze serveru a klienta aplikací, s minimální narušení metrik. Toto je primárním nástrojem hello tootune hello množství dat. Další informace o [vzorkování funkce](app-insights-sampling.md). 
* **Denního limitu:** při vytváření prostředek Application Insights z hello portálu Azure, to je nastavit too500 GB a den. Výchozí text Hello, při vytváření prostředek Application Insights v sadě Visual Studio, je malý (pouze 32,3 MB/den), která je určena pouze toofaciliate testování. V takovém případě je určeno, že tento uživatel hello vyvolá hello denního limitu před nasazením aplikace hello do produkčního prostředí. Hello maximální hodnota je 500 GB a den, pokud požadujete vyšší maximum pro intenzivní provoz aplikace. Použít pozor při nastavení hello denního limitu, jako by měl být vašich představ **nikdy toohit hello denního limitu**, protože se pak dojít ke ztrátě dat pro hello zbytek hello den a být nelze toomonitor vaší aplikace. toochange ji, použijte hello denní svazku cap okno odkazované z okna Správa svazku Data hello (viz níže). Všimněte si, že některé typy předplatného mají kreditu, který nelze použít pro službu Application Insights. Pokud má předplatné hello výdaje omezit, hello denně cap okno bude mít pokyny jak tooremove jej a povolte hello denní cap toobe vyvolá nad rámec 32,3 MB a den.  
* **Omezení šířky pásma:** tento hello omezení dat míra too32 tisíc událostí za sekundu, průměr více než 1 minuta. 


*Co se stane, když aplikace my překračuje omezení rychlost hello?*

* Hello objem dat, která vaše aplikace odesílá se hodnotí každou minutu. Pokud překročí hello za sekundu průměr za minutu hello, hello server odmítá některých požadavků. Hello SDK vyrovnávacích pamětí hello dat a potom se pokusí tooresend, rozšíří nárůst za několik minut. Pokud vaše aplikace konzistentně odesílá data na výše hello omezení míry, některá data se zahodí. (hello ASP.NET, Java či JavaScript SDK zkuste tooresend tímto způsobem, dalších sadách SDK může jednoduše rozevírací omezeny data.) Pokud dojde k omezení šířky pásma, uvidíte oznámení upozornění, že byla odepřena.

*Jak poznám, kolik dat Moje aplikace odesílá?*

* Otevřete hello **Správa svazku dat** okno toosee hello denní data svazku grafu. 
* Nebo v Průzkumníku metrik, přidejte nový graf a vyberte **datového bodu svazku** jako jeho metriku. Přepněte na seskupení a seskupit podle **datový typ**.

## <a name="tooreduce-your-data-rate"></a>tooreduce přenosová rychlost
Zde jsou některé věci, které můžete provést tooreduce datový svazek:

* Použití [vzorkování](app-insights-sampling.md). Tato technologie snižuje rychlost přenosu dat bez zkosení vaše metriky a to bez přerušení hello možnost toonavigate mezi související položky v hledání. V aplikacích serveru funguje automaticky.
* [Omezit hello počet volání Ajax, které mohou být oznámeny](app-insights-javascript.md#detailed-configuration) v každé zobrazení stránky nebo přepínač vypnout Ajax reporting.
* Vypnout kolekce modulů nepotřebujete podle [úpravy souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Například můžete rozhodnout, jestli jsou inessential čítače výkonu nebo data závislostí.
* Rozdělení klíčů instrumentace tooseparate vaší telemetrie. 
* Předem agregační metriky. Pokud jste umístili volání tooTrackMetric ve vaší aplikaci, můžete snížit provozu pomocí hello přetížení, které přijímá vaší výpočtu průměru hello a směrodatnou odchylku dávky měření. Nebo můžete použít [předem totožný balíček](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="managing-hello-maximum-daily-data-volume"></a>Správa svazku dat maximální denní hello

Můžete použít hello denní svazku cap toolimit hello shromažďovaných dat, ale splnění hello cap způsobí ztrátu všechny telemetery odeslaných z vaší aplikace hello zbývající hello den. Je **není vhodné** toohave vaší aplikace toohit hello cap denní od můžete jsou nelze tootrack hello stavu a výkonu vaší aplikace po je přístupů. 

Místo toho použijte [vzorkování](app-insights-sampling.md) tootune hello data toohello úrovni svazku chcete vytvořit a použít denního limitu hello pouze jako "poslední možnost" v případě, že vaše aplikace spustí odesílání mnohem vyšší objemy telemetery neočekávaně. 

zakončení, denní toochange hello v hello část konfigurace vaší aplikace Insihgts prostředku, klikněte na tlačítko **Správa svazku dat** pak **denního limitu**.

![Úprava hello denní telemetrie svazku zakončení](./media/app-insights-pricing/daily-cap.png) 

## <a name="sampling"></a>Vzorkování
[Vzorkování](app-insights-sampling.md) počty stále zachování správné událostí a je metoda zredukování hello rychlost telemetrie je odesílání tooyour aplikace, zatímco zároveň zachovat hello možnost toofind související události během diagnostických hledání. 

Vzorkování je účinný způsob tooreduce poplatky a zůstat v měsíční kvóta. Hello vzorkování algoritmus uchovává související položky telemetrie, takže například při použití hledání můžete najít hello žádosti související tooa určité výjimky. algoritmus Hello také zachová správné počty, abyste viděli hello správné hodnoty v Průzkumníku Metrika pro požadavků, výjimka frekvenci a dalších položek.

Existuje několik formy vzorkování.

* [Adaptivního vzorkování](app-insights-sampling.md) je výchozí hodnotou hello hello sady SDK technologie ASP.NET, které automaticky upraví toohello objem telemetrii, kterou vaše aplikace odesílá. Funguje automaticky v hello SDK ve vaší webové aplikaci, tak, aby se snižuje hello telemetrie provoz v síti hello. 
* *Přijímání vzorkování* představuje alternativu, která pracuje na hello místa, kde telemetrie z vaší aplikace do služby Application Insights hello. Neovlivní hello objem telemetrická data odesílaná z vaší aplikace, ale snižuje hello svazku uchovávají službou hello. Můžete ji použít kvóty hello tooreduce používané telemetrie z prohlížečů a dalších sadách SDK.

přijímání tooset vzorkování, nastavení ovládacího prvku hello v okně Cenová hello:

![Z hello kvóty a ceny okno klikněte na dlaždici ukázky hello a vyberte zlomek vzorkování.](./media/app-insights-pricing/04.png)

> [!WARNING]
> okno dat vzorkování Hello je řízeno jen hello hodnotu přijímání vzorkování. Ho neodráží hello vzorkovací frekvenci, které se uplatňuje hello Application Insights SDK ve vaší aplikaci. Pokud příchozí telemetrii hello má již byla odebírána data v hello SDK, není použita přijímání vzorkování.
> 

toodiscover hello skutečné vzorkování míru bez ohledu na to, kde bylo použito, použijte [Analytics dotazu](app-insights-analytics.md) jako je tato:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

V každé uchovávají záznam, `itemCount` označuje hello původní záznamy, které představuje, rovna too1 + hello počet předchozích zrušených záznamů. 


## <a name="automation"></a>Automation

Můžete napsat plán skriptu tooset hello cena pomocí Správa prostředků Azure. [Zjistěte, jak](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Souhrn omezení
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="next-steps"></a>Další kroky

* [Vzorkování](app-insights-sampling.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/

