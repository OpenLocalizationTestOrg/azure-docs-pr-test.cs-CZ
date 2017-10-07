---
title: "aaaManage Azure mikroslužbu zatížení pomocí metriky | Microsoft Docs"
description: "Informace o tom, jak tooconfigure a používání metriky v Service Fabric toomanage služby spotřeby prostředků."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 0d622ea6-a7c7-4bef-886b-06e6b85a97fb
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 592dc749ce30683a1e439a702b7d0dc0a638276f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-resource-consumption-and-load-in-service-fabric-with-metrics"></a>Správa spotřeby prostředků a zatížení v Service Fabric pomocí metrik
*Metriky* hello prostředků, které vaše služby péče o a které jsou k dispozici hello uzly v clusteru hello. Metrika je nic, které chcete toomanage v pořadí tooimprove nebo monitorování výkonu hello vašich služeb. Pokud je přetížena služby, například může sledovat tooknow spotřeba paměti. Použijte jiný je toofigure se, zda může služba hello přesunout jinde kde paměti je že menší omezené v pořadí tooget lepší výkon.

Příklady metriky jsou věci jako využití paměti, disku a procesoru. Tyto metriky jsou fyzické metriky, prostředky, které odpovídají toophysical prostředky v hello uzlu, který potřebují toobe spravované. Metriky může být také (a často jsou) logické metriky. Logické metriky věci jako "MyWorkQueueDepth" nebo "MessagesToProcess" nebo "TotalRecords". Logické metriky jsou definované aplikací a nepřímo odpovídají spotřeby toosome fyzických prostředků. Logické metriky jsou společné, protože může být pevný toomeasure a sestava spotřeby fyzické prostředky na základě služby. složitost Hello měření a vytváření sestav vlastní fyzické metriky je také proč Service Fabric nabízí několik výchozích metrik.

## <a name="default-metrics"></a>Výchozích metrik
Řekněme, že chcete tooget spuštění zápisu a nasazení služby. V tomto okamžiku nevíte fyzické nebo logické prostředky spotřebuje. Není to! Hello správce prostředků clusteru Service Fabric používá několik výchozích metrik, pokud nejsou zadány žádné jiné metriky. Jsou:

  - PrimaryCount - počet primární repliky na uzlu hello 
  - ReplicaCount - počet celkový stavová repliky na uzlu hello
  - Počet - počet všech objektů služby (bezstavové a stavové) na uzlu hello

| Metrika | Bezstavové Instance zatížení | Stavová zatížení sekundární | Stavová zatížení primární |
| --- | --- | --- | --- |
| PrimaryCount |0 |0 |1 |
| ReplicaCount |0 |1 |1 |
| Počet |1 |1 |1 |

Pro základní úlohy výchozích metrik hello zadejte dostatečnou distribuční práce v clusteru hello. V následujícím příkladu hello Podíváme se, co se stane, když jsme vytvořit dvě služby a spoléhají na hello výchozích metrik pro vyrovnávání. repliku cíl nastavit velikost tří první službě Hello je stavové služby s tři oddíly. druhý služba Hello je bezstavové služby s jedním oddílem a počet instancí tří.

Tady je můžete získat:

<center>
![Cluster rozložení s výchozím nastavení metriky][Image1]
</center>

Toonote některé věci:
  - Primární repliky pro hello stavové služby, které jsou rozmístěny v několika uzly
  - Repliky pro hello stejného oddílu jsou na různých uzlech
  - Celkový počet Hello základní barvy a sekundárních databází se distribuuje v clusteru hello
  - Celkový počet objektů služby Hello jsou rovnoměrně přiděleny na každém uzlu

Dobré!

Hello výchozích metrik fungovat skvěle jako spuštění. Ale hello výchozích metrik bude pouze provádění můžete dosavadní práce. Příklad: co je hello pravděpodobnost, že dělení hello scheme je vybráno výsledky perfektně i využití tím všechny oddíly? Co je hello pravděpodobnost, že hello zatížení pro danou službu je konstantní v průběhu času, nebo i právě stejné hello napříč více oddílů nyní?

Můžete spustit s právě hello výchozích metrik. Obvykle to však znamená, že využití vašeho clusteru je menší a více nerovnoměrné, než byste chtěli. Je to proto hello výchozích metrik nejsou adaptivní a předpokládá, že všechno, co je ekvivalentní. "1" metrika PrimaryCount toohello přispět například primární, který je zaneprázdněn a ten, který není obou. V nejhorším případě hello pomocí pouze hello výchozích metrik může také způsobit overscheduled uzly vést k problémům s výkonem. Pokud vás zajímá získávání hello většina mimo cluster a vyhnout problémům s výkonem, je třeba toouse vlastní metriky a vytváření sestav dynamického zatížení.

## <a name="custom-metrics"></a>Vlastní metriky
Metriky jsou nakonfigurované na základě jednotlivých s názvem service instancí při vytváření služby hello.

Všechny metrika se některé vlastnosti, které popisují ho: název, váhu a výchozí zatížení.

* Název metriky: název hello metrika hello. Název metriky Hello je jedinečný identifikátor pro metriku hello v rámci clusteru hello z hlediska hello Resource Manager.
* Váha: Váha metriky definuje, jak důležité tato metrika je relativní toohello další metriky pro tuto službu.
* Výchozí zatížení: hello výchozí zatížení je reprezentována odlišně v závislosti na tom, zda je služba hello bezstavové nebo stateful.
  * Pro bezstavové služby jednotlivé metriky má jednu vlastnost s názvem DefaultLoad
  * Pro stavové služby můžete definovat:
    * PrimaryDefaultLoad: výchozí velikost hello tuto metriku, které tato služba využívá, pokud je primární
    * SecondaryDefaultLoad: tuto metriku, které tato služba využívá případě, že je sekundární velikost výchozí hello

> [!NOTE]
> Pokud definujete vlastní metriky a chcete too_also_ použít hello výchozí nastavení, je třeba too_explicitly_ přidat hello výchozích metrik zpět a zadejte váhu a hodnoty pro ně. Je to proto, že je nutné definovat hello vztah mezi hello výchozích metrik a vaše vlastní metriky. Například možná se zajímáte o ConnectionCount nebo WorkQueueDepth víc než primární distribuce. Pomocí výchozí hello váhu hello PrimaryCount metrika se horní, takže chcete tooreduce ho tooMedium při přidání vaší jiné metriky tooensure jejich přednost.
>

### <a name="defining-metrics-for-your-service---an-example"></a>Definice metrik služby – příklad
Může se stát, že chcete hello následující konfigurace:

  - Sestavy služby metriky s názvem "ConnectionCount"
  - Chcete taky toouse hello výchozích metrik 
  - Jste provádí některé měření a vědět, že normálně primární repliky této služby zabírají 20 jednotek "ConnectionCount"
  - Sekundární databáze použijte 5 jednotky "ConnectionCount"
  - Víte, že "ConnectionCount" hello nejdůležitější metriky z hlediska Správa výkonu hello této konkrétní služby
  - Stále chcete vyrovnáváním primární repliky. Vyrovnávání primární repliky obecně je vhodné bez ohledu na to, co. To pomáhá zabránit ztrátě hello některé domény uzlu nebo selhání většiny primární repliky společně s jeho, které mají vliv. 
  - Jinak jsou hello výchozích metrik

Tady je kód hello, že byste měli psát toocreate služby s touto konfigurací metriky:

Kód:

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
StatefulServiceLoadMetricDescription connectionMetric = new StatefulServiceLoadMetricDescription();
connectionMetric.Name = "ConnectionCount";
connectionMetric.PrimaryDefaultLoad = 20;
connectionMetric.SecondaryDefaultLoad = 5;
connectionMetric.Weight = ServiceLoadMetricWeight.High;

StatefulServiceLoadMetricDescription primaryCountMetric = new StatefulServiceLoadMetricDescription();
primaryCountMetric.Name = "PrimaryCount";
primaryCountMetric.PrimaryDefaultLoad = 1;
primaryCountMetric.SecondaryDefaultLoad = 0;
primaryCountMetric.Weight = ServiceLoadMetricWeight.Medium;

StatefulServiceLoadMetricDescription replicaCountMetric = new StatefulServiceLoadMetricDescription();
replicaCountMetric.Name = "ReplicaCount";
replicaCountMetric.PrimaryDefaultLoad = 1;
replicaCountMetric.SecondaryDefaultLoad = 1;
replicaCountMetric.Weight = ServiceLoadMetricWeight.Low;

StatefulServiceLoadMetricDescription totalCountMetric = new StatefulServiceLoadMetricDescription();
totalCountMetric.Name = "Count";
totalCountMetric.PrimaryDefaultLoad = 1;
totalCountMetric.SecondaryDefaultLoad = 1;
totalCountMetric.Weight = ServiceLoadMetricWeight.Low;

serviceDescription.Metrics.Add(connectionMetric);
serviceDescription.Metrics.Add(primaryCountMetric);
serviceDescription.Metrics.Add(replicaCountMetric);
serviceDescription.Metrics.Add(totalCountMetric);

await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Prostředí PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ConnectionCount,High,20,5”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

> [!NOTE]
> Hello výše příklady a hello zbytek tohoto dokumentu popisuje správu metriky na základě za-s názvem service. Je také možné toodefine metriky pro vaše služby v hello service _typ_ úroveň. To se provádí zadáním v manifestech vaší služby. Definování typu metriky na úrovni se nedoporučuje z několika důvodů. Hello prvním důvodem je, že metriky názvy jsou často konkrétní prostředí. Pokud je na místě závazné kontrakt, nemůže se ujistěte, že tento metrika hello "Jader" v jednom prostředí není "MiliCores" nebo "Jader" k ostatním. Pokud vaše metriky, které jsou definovány v manifestu je třeba manifesty nového toocreate za prostředí. To obvykle vede tooa, jak narůstá počet jinou manifestech s pouze drobné rozdíly, které může způsobit problémy toomanagement.  
>
> Metriky zatížení jsou běžně přiřadit na základě jednotlivých s názvem service instancí. Předpokládejme například, že vytvoříte jednu instanci hello služby pro CustomerA, kdo plány toouse ho pouze lehkým. Také se stát, že vytvoříte jiné pro CustomerB, který má větší zatížení. V takovém případě budete by pravděpodobně chtít načte výchozí hello tootweak pro tyto služby. Pokud máte metriky a zatížením definované prostřednictvím manifesty a vy chcete toosupport tento scénář, vyžaduje pro každého zákazníka jiné aplikace a typů služeb. Hello hodnot definovaných v okamžiku vytvoření služby přepíšou nastavení definované v manifestu hello, takže můžete použít tento tooset hello konkrétní výchozí hodnoty. Ale učinit způsobí, že hodnoty hello deklarované v hello manifesty odpovídají toonot těchto hello služba je ve skutečnosti spuštěna s. To může způsobit tooconfusion. 
>

Jako připomenutí: Pokud chcete toouse hello výchozích metrik, není nutné tootouch hello metriky kolekce vůbec nebo provádět žádné zvláštní při vytváření služby. Hello výchozích metrik získat použít automaticky, když jsou definovány žádné jiné. 

Nyní se Vraťme se prostřednictvím každé z těchto nastavení podrobněji a mluvit o hello chování, které má vliv.

## <a name="load"></a>Načtení
Hello celou bodu definice metrik je toorepresent některé zatížení. *Zatížení* je kolik danou metriku obsazením některé instance služby nebo repliky v daném uzlu. Zatížení se dá nakonfigurovat téměř libovolném okamžiku. Například:

  - Zatížení se dá definovat při vytváření služby. To se označuje jako _výchozí zatížení_.
  - Hello metriky informace, včetně výchozí načte pro službu lze aktualizovat po vytvoření služby hello. To se označuje jako _aktualizace služby_. 
  - Hello zatížení pro daný oddíl může být resetování toohello výchozí hodnoty pro tuto službu. To se označuje jako _obnovení zatížení oddílu_.
  - Zatížení mohou být zaznamenány na za objekt individuálních dynamicky za běhu. To se označuje jako _reporting zatížení_. 
  
Všechny tyto strategie lze použít v rámci stejné služby průběhu své životnosti hello. 

## <a name="default-load"></a>Výchozí zatížení
*Výchozí zatížení* je kolik hello metrika využívá každý objekt služby (bezstavové instance nebo stavová repliky) této služby. Hello clusteru Resource Manager používá toto číslo pro zatížení hello objektu služby hello až do obdržení jiné informace, jako je například dynamického zatížení sestavy. Pro jednodušší služby je výchozí zatížení hello definici statické. Hello výchozí zatížení nikdy se aktualizuje a slouží k hello životnost hello služby. Výchozí načte funguje dobře pro jednoduché plánování scénáře, kde určité množství prostředků jsou vyhrazené toodifferent úlohy a neměňte kapacity.

> [!NOTE]
> Další informace o řízení kapacity a definování kapacity pro hello uzly v clusteru, najdete v tématu [v tomto článku](service-fabric-cluster-resource-manager-cluster-description.md#capacity).
> 

Hello clusteru Resource Manager umožňuje toospecify stavové služby různé výchozí zatížení pro své základní barvy a sekundární repliky. Bezstavové služby můžete zadat jenom jednu hodnotu, která se použije tooall instance. Pro stavové služby se obvykle liší hello výchozí zatížení pro primární i sekundární repliky, protože repliky proveďte různé druhy práce v každé role. Například základní barvy obvykle sloužit čtení a zápisu a zpracovat většinu hello výpočetní zatížení, když sekundární databáze nepodporují. Obvykle je vyšší než hello výchozí zatížení pro sekundární repliky hello výchozí zatížení pro primární repliku. reálná čísla Hello by měl závisí na svých vlastních hodnot.

## <a name="dynamic-load"></a>Dynamické načtení
Řekněme, že jste byl spuštěn služby nějakou dobu. S některá monitorování jste si všimli který:

1. Některé oddíly nebo instance dané služby spotřebují více prostředků než jiné
2. Některé služby mají zatížení, který se liší v čase.

Je velké množství věcí, které by mohly způsobit tyto typy kolísání zátěže. Například různé služby nebo oddíly jsou přidruženy různých zákazníků, jejichž jiné požadavky. Zatížení může také změnit, protože hello množství práce hello služby se liší průběhu hello hello den. Bez ohledu na to z toho důvodu hello obvykle neexistuje, můžete použít pro výchozí číslo jeden. To platí hlavně pokud chcete tooget hello většina využití mimo hello clusteru. Libovolná hodnota, kterou vyberete pro výchozí zatížení je nesprávný některé hello času. Nesprávný výchozí načte vést hello správce prostředků clusteru nad nebo pod přidělování prostředků. V důsledku toho máte uzlů, které jsou nad nebo pod využité Přestože hello správce prostředků clusteru předpokládá, že jsou rovnoměrně hello clusteru. Výchozí zatížení jsou stále vhodné, protože poskytují některé informace pro počáteční umístění, ale nejsou kompletní scénáře pro skutečné úlohy. zachycení tooaccurately změna požadavky na prostředky, hello clusteru Resource Manager umožňuje každý objekt tooupdate služby vlastní zatížení za běhu. Tomu se říká reporting dynamického zatížení.

Dynamické načtení sestav povolit replik nebo instancí tooadjust jejich přidělení/hlášené zatížení metrik průběhu své životnosti. Replika služby nebo instanci, která se uloží málo používaná data a není to veškerou práci, by obvykle sestavy, aby používalo nízkou množství danou metriku. Zaneprázdněný replik nebo instancí by hlásit, že používají více.

Vytváření sestav zatížení za replik nebo instancí umožňuje hello tooreorganize správce prostředků clusteru objekty hello jednotlivé služby v clusteru hello. Reorganizace hello služby pomáhá zajistit, že získají hello prostředky, které vyžadují. Zaneprázdněný služby efektivně získat příliš "uvolnit" prostředky z dalších replik nebo instancí, které jsou aktuálně studené nebo méně pracuje.

V rámci spolehlivé služby hello kód tooreport zatížení dynamicky vypadá takto:

Kód:

```csharp
this.Partition.ReportLoad(new List<LoadMetric> { new LoadMetric("CurrentConnectionCount", 1234), new LoadMetric("metric1", 42) });
```

Služba může hlásit na žádném z hello metriky definované pro něj v okamžiku vytvoření. Pokud zatížení sestavy služby pro metriku, který není nakonfigurovaný toouse, Service Fabric ignoruje této sestavy. Pokud existují další metriky hlášené v hello stejnou dobu, po kterou jsou platné, jsou podmínky přijaty tyto sestavy. Kódu služby můžete měřit a sestavy, všechny hello metriky věděl, že může postupy, a operátory můžete zadat toouse hello metriky konfigurace bez nutnosti kódu služby toochange hello. 

### <a name="updating-a-services-metric-configuration"></a>Aktualizuje se konfigurace metriky služby
Hello seznam metriky související se službou hello a hello tyto metriky můžete aktualizovat vlastnosti dynamicky při provozu služby hello. To umožňuje experimentování a flexibilitu. Některé příklady Pokud to je užitečné, jsou:

  - zakázání metrika s buggy zprávy pro konkrétní službu
  - Změna konfigurace hello váhu metriky na základě požadovaného chování
  - povolení nové metrika až po hello kód již nasazen a ověřit pomocí jiných mechanismů
  - Změna hello výchozí zatížení pro službu na základě pozorovaných chování a spotřeba

Hello hlavní rozhraní API pro změnu metriky konfigurace jsou `FabricClient.ServiceManagementClient.UpdateServiceAsync` v jazyce C# a `Update-ServiceFabricService` v prostředí PowerShell. Ať informace, které zadáte pomocí těchto rozhraní API nahrazuje hello stávající informace o metrikách služby hello okamžitě. 

## <a name="mixing-default-load-values-and-dynamic-load-reports"></a>Kombinování výchozí hodnoty zatížení a dynamické načtení sestav
Výchozí zatížení a dynamické zatížení lze použít pro hello stejnou službu. Když služba využívá výchozí zatížení a dynamické načtení sestavy, výchozí zatížení slouží jako odhad dokud dynamické sestavy, na které se objeví. Výchozí zatížení je vhodný, protože nabízí hello správce prostředků clusteru něco toowork s. Hello výchozí zatížení umožňuje hello tooplace správce prostředků clusteru při jejich vytváření objektů služby hello v dobré umístění. Pokud je zadaný žádný výchozí informace o zatížení, je efektivně náhodný umístění služeb. Při načítání sestavy příchodu později hello počáteční náhodných umístění je často nesprávný a hello správce prostředků clusteru má toomove služby.

Umožňuje trvat naše předchozí příklad a zjistěte, co se stane, když přidáme některé vlastní metriky a vytváření sestav dynamického zatížení. V tomto příkladu používáme jako metriku příklad "MemoryInMb".

> [!NOTE]
> Paměť je jedním z hello systému metriky, které můžete Service Fabric [prostředek řídí](service-fabric-resource-governance.md), a reporting sami je obvykle obtížná. Nemáte Očekáváme, že ve skutečnosti můžete tooreport na spotřebu paměti; Paměť se používá jako toolearning podpory o možnostech hello hello správce prostředků clusteru.
>

Pojďme se předpokládá jsme původně vytvořili hello stavové služby s hello následující příkaz:

Prostředí PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("MemoryInMb,High,21,11”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

Připomínáme je tato syntaxe (MetricName, MetricWeight, PrimaryDefaultLoad, SecondaryDefaultLoad").

Podívejme se, co jeden rozložení možné clusteru může vypadat podobně jako:

<center>
![Cluster rovnováha s výchozí i vlastní metriky][Image2]
</center>

Některé věci, které jsou vhodné poznamenat:

* Každý sekundární repliky v rámci oddílu může mít vlastní zatížení
* Celkové vypadat vyrovnáváním hello metriky. O paměť, hello poměr mezi hello maximální a minimální zatížení je 1,75 (hello uzlu s hello většinu zatížení je N3, hello alespoň N2 a 28/16 = 1,75).

Existují některé věci, potřebujeme ještě tooexplain:

* Co je určit, zda byla poměr 1,75 přiměřené nebo ne? Jak hello správce prostředků clusteru, že pokud to je výhodné dostatečně, nebo pokud existuje více toodo pracovní?
* Pokud vyrovnávání dojít?
* Co to znamená, že byl "Základní" váha paměti?

## <a name="metric-weights"></a>Metriky váhu
Sledování hello stejné metriky mezi různé služby je důležité. Zda je globální zobrazení, co umožňuje hello energie tootrack správce prostředků clusteru v clusteru hello, vyvážit spotřeba mezi uzly a ujistěte se, uzly si projít kapacity. Služby však mohou mít různá zobrazení jako toohello význam hello stejnou metriku. V clusteru s mnoha metriky a velké množství služeb, navíc perfektně vyrovnáváním řešení neexistuje pro všechny metriky. Jak hello správce prostředků clusteru pracovat s těmito situacemi?

Metriky váhu povolit hello toodecide správce prostředků clusteru, jak toobalance hello clusteru při volaná úplně bez chyby. Metriky váhu také povolit hello správce prostředků clusteru toobalance určitým službám jinak. Metriky lze vytvořit čtyři úrovně různých váhy: nula, nízké, střední a vysokou. Metrika s váhou nula přispívá nic při určování, zda jsou vyváženy věcí, nebo ne. Jejím zatížení však stále přispívat toocapacity správy. Metriky s nulovou tloušťkou jsou stále užitečné a často používají jako součást chování služby a sledování výkonu. [Tento článek](service-fabric-diagnostics-event-generation-infra.md) poskytuje další informace o použití hello metriky pro monitorování a Diagnostika vašich služeb. 

Hello skutečné dopad jiné metriky váhu v clusteru hello je, že tento hello správce prostředků clusteru generuje různá řešení. Metriky váhu řekněte hello správce prostředků clusteru, že určité metriky jsou důležitější než jiné. Pokud není žádná hello ideální řešení správce prostředků clusteru dáváte přednost řešení, které vyvážit hello vyšší vyvážené metriky lepší. Pokud služba rootu předpokládá, že konkrétní metrika je důležitý, že jejich použití tohoto Metrika může najít imbalanced. To umožňuje rovnoměrné rozdělení některé metriku, je důležité tooit tooget jiné služby.

Podívejme se na příklad některých sestav zatížení a jak budou různí metrika provede výsledky v různých přidělení v clusteru hello. V tomto příkladu vidíte, že přepnutí hello relativní váhu hello metriky způsobí, že hello toocreate správce prostředků clusteru změnit uspořádání služeb.

<center>
![Příklad váha metriky a její vliv na řešení vyrovnávání][Image3]
</center>

V tomto příkladu jsou čtyři různé služby, všechny sestavy různé hodnoty pro dva různé metriky, MetricA a MetricB. V jednom případě všechny služby hello definovat MetricA je hello nejdůležitější (váhy = vysoce) a MetricB jako důležitý (váhy = nízká). V důsledku toho vidíte, že tento hello správce prostředků clusteru umístí hello služby tak, aby lépe než MetricB vyrovnáváním MetricA. "Lépe vyrovnáváním" znamená, že MetricA má nižší má nižší směrodatnou odchylku než MetricB. V druhém případě hello jsme obrátíte metriky váhu hello. V důsledku toho hello správce prostředků clusteru prohození služeb A a B toocome až s přidělení, kde je MetricB lépe než MetricA vyrovnáváním.

> [!NOTE]
> Metriky váhu určit, jak by měla vyvážit hello správce prostředků clusteru, ale když vyrovnávání došlo k neočekávané chybě. Další informace o vyrovnávání, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-balancing.md)
>

### <a name="global-metric-weights"></a>Globální metriky váhu
Řekněme, že ServiceA definuje MetricA jako vážené vysokou a ServiceB nastaví hello váha pro MetricA tooLow nebo nulový. Co je hello skutečné váhy, který končí získávání použít?

Existuje více váhu sledovaných pro každý metriku. první váhy Hello je hello definovaná pro metriku hello při vytvoření služby hello. Hello jiné váhy je globální váhy, které se počítá automaticky. Hello správce prostředků clusteru používá obě tyto váhy při vyhodnocování řešení. Vezme v úvahu oba váhu je důležité. To umožňuje hello správce prostředků clusteru toobalance každé služby podle tooits vlastní priority a ujistěte se také hello clusteru jako celek je přidělen správně.

Co by mohlo dojít, pokud nebyla hello správce prostředků clusteru vědět o globální a místní vyrovnávání? Dobře je snadné tooconstruct řešení globálně vyrovnáváním, ale důsledkem toho rovnováze nízký prostředků pro jednotlivé služby. V následujícím příkladu hello umožňuje vyhledat služba nakonfigurovaná pomocí právě hello výchozích metrik a najdete, co se stane, když je považován za pouze globální vyrovnávání:

<center>
![Hello dopad globální řešení jenom][Image4]
</center>

V příkladu nejvyšší hello podle pouze globální vyrovnávání je skutečně vyvážit hello clusteru jako celek. Všechny uzly mají hello stejný počet základní barvy a hello stejné číslo celkový počet replik. Ale pokud se podíváte na hello skutečný dopad toto přidělení není proto vhodné: hello ztrátě libovolného uzlu ovlivňuje určité zatížení nepřiměřeně, protože trvá se všechny jeho základní barvy. Například pokud hello první uzel selže hello tři základní barvy pro hello tři různé oddíly hello kruh služby by všechny ztratí. Naopak hello trojúhelníček a šestiúhelník služby mají jejich oddíly ztratit repliku. To způsobí, že bez přerušení, kromě nutnosti toorecover hello dolů repliky.

V příkladu dolní hello hello správce prostředků clusteru byla distribuována hello repliky založené na obou hello globální a za službou Vyrovnávání. Při výpočtu skóre hello hello řešení poskytuje nejvíc hello váhy toohello globální řešení a tooindividual služeb část (Konfigurovat). Globální vyrovnávání pro metriku je vypočítáváno na hello průměr hello metriky váhu z jednotlivých služeb. Každá služba jsou rovnoměrně podle tooits vlastní definované metriky váhu. Tím se zajistí, že jsou vyváženy hello služby v rámci sami podle vlastních potřeb tootheir. Výsledkem je pokud hello stejné první uzel selže selhání hello se distribuuje do všech oddílů všech služeb. tooeach dopad Hello je hello stejné.

## <a name="next-steps"></a>Další kroky
- Další informace o konfiguraci služby [Další informace o konfiguraci služby](service-fabric-cluster-resource-manager-configure-services.md)(service-fabric-cluster-resource-manager-configure-services.md)
- Definování defragmentace metriky je jedním ze způsobů tooconsolidate zatížení na uzlech místo rozšíří ho. toolearn jak tooconfigure defragmentace, získáte příliš[v tomto článku](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
- toofind si o tom, jak hello správce prostředků clusteru spravuje a vyrovnává zatížení v clusteru hello, podívejte se na článek hello na [Vyrovnávání zatížení](service-fabric-cluster-resource-manager-balancing.md)
- Začít od začátku hello a [získat Úvod toohello správce prostředků clusteru Service Fabric](service-fabric-cluster-resource-manager-introduction.md)
- Náklady na přesunutí je jeden ze způsobů signalizace toohello správce prostředků clusteru, že jsou určité služby toomove dražší než jiné. toolearn Další informace o náklady na přesunutí, najdete v příliš[v tomto článku](service-fabric-cluster-resource-manager-movement-cost.md)

[Image1]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-cluster-layout-with-default-metrics.png
[Image2]:./media/service-fabric-cluster-resource-manager-metrics/Service-Fabric-Resource-Manager-Dynamic-Load-Reports.png
[Image3]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-metric-weights-impact.png
[Image4]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-global-vs-local-balancing.png
