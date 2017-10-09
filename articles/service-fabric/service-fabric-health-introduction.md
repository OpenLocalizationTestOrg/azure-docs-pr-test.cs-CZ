---
title: "aaaHealth monitorování v nástroji Service Fabric | Microsoft Docs"
description: "Úvod toohello Azure Service Fabric sledování stavu modelu, který poskytuje sledování hello clusteru a příslušné aplikace a služby."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 1d979210-b1eb-4022-be24-799fd9d8e003
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: oanapl
ms.openlocfilehash: 904f36374ca6ca7e4caa1d43c92584e7e4c50087
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooservice-fabric-health-monitoring"></a>Monitorování stavu infrastruktury tooService Úvod
Azure Service Fabric zavádí stavu model, který poskytuje bohatý, flexibilní a rozšiřitelný stavu vyhodnocení a vytváření sestav. Hello model umožňuje téměř v reálném čase monitorování stavu hello hello clusteru a hello služby spuštěné v ní. Můžete snadno získat informace o stavu a opravte potenciální problémy předtím, než v kaskádě a způsobit masivní výpadků. V typické modelu hello služby odesílat sestavy založené na jejich místní zobrazení, a zda jsou informace agregován tooprovide zobrazení celkového úrovni clusteru.

Komponenty Service Fabric pomocí této bohaté stavu modelu tooreport jejich aktuálního stavu. Můžete použít hello stejný mechanismus tooreport stavu z vašich aplikací. Pokud jste investovat do stavu vysoce kvalitní reporting shromažďuje vaše vlastní podmínky, můžete zjistit a opravit problémy mnohem snadněji pro běžící aplikaci.

Hello následující Microsoft Virtual Academy video také popisuje hello Service Fabric stavu modelu a jeho použití:<center><a target="_blank" href="https://mva.microsoft.com/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tevZw56yC_1906218965">
<img src="./media/service-fabric-health-introduction/HealthIntroVid.png" WIDTH="360" HEIGHT="244">
</a></center>

> [!NOTE]
> Spuštění hello stavu subsystému tooaddress potřebu monitorovaných upgrady. Service Fabric nabízí monitorovaných aplikací a cluster upgrady, které zajištění úplné dostupnosti, žádné výpadky a minimální toono zásahu uživatele. kontroluje stav na základě těchto cílů hello upgrade tooachieve nakonfigurovali zásady upgradu. Upgrade můžete pokračovat jenom v případě, že stav respektuje požadované prahové hodnoty. Hello upgrade, jinak hodnota je buď automaticky vrácena zpět nebo pozastavena toogive správci prvního toofix hello problémy. toolearn Další informace o upgradu aplikace, najdete v části [v tomto článku](service-fabric-application-upgrade.md).
> 
> 

## <a name="health-store"></a>Úložiště stavu
úložiště stavu Hello zajišťuje zdravotních informací o entitách hello clusteru pro snadné načítání a vyhodnocení. Jsou implementované jako Service Fabric trvalé stavové služby tooensure vysokou dostupnost a škálovatelnost. Hello stavu úložiště je součástí hello **fabric: / System** aplikace a je k dispozici, pokud je hello cluster v provozu a spuštěná.

## <a name="health-entities-and-hierarchy"></a>Stav entity a hierarchie
entity stavu Hello jsou uspořádány do logických hierarchie, která zaznamená interakce a závislosti mezi různými entitami. úložiště stavu Hello automaticky vytvoří entity stavu a hierarchie, které jsou založené na zprávách přijaté z komponenty Service Fabric.

entity stavu Hello zrcadlení hello Service Fabric entity. (Například **stavu aplikace entity** odpovídá instance aplikace nasazené v clusteru hello, při **stavu uzlu entity** odpovídá Service Fabric uzlu clusteru.) zaznamená hierarchie stavu hello interakce Hello hello systémových entit ale je hello základ pro vyhodnocení pokročilé stavu. Informace o klíčové koncepty Service Fabric najdete [Service Fabric technický přehled](service-fabric-technical-overview.md). Další informace o aplikaci, najdete v části [model aplikace Service Fabric](service-fabric-application-model.md).

Hello stavu entity a hierarchii povolit hello cluster a aplikace toobe efektivně ohlásit, ladit a sledovat. model stavu Hello poskytuje přesný, *granulární* reprezentace hello stav hello mnoho přesunutí součásti v clusteru hello.

![Stav entity.][1]
stav entity Hello uspořádány v hierarchii podle vztahů nadřazenosti a podřízenosti.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

entity Hello stavu jsou:

* **Cluster**. Představuje stav hello cluster Service Fabric. Sestavy stavu clusteru popisují podmínky, které ovlivňují hello celý cluster. Tyto podmínky ovlivňují více entit v clusteru hello nebo samotného clusteru hello. Na základě hello podmínky, nelze hello ohlašování upřesněte hello problém dolů tooone nebo více podřízených prvků není v pořádku. Mezi příklady patří hello mozek hello clusteru rozdělení kvůli toonetwork rozdělení do oddílů nebo komunikační problémy.
* **Uzel**. Představuje hello stavu Service Fabric uzlu. Sestavy stavu uzlu popisují podmínky, které ovlivňují funkčnost uzlu hello. Obvykle mají vliv na všechny entity hello nasazené v rámci něj spuštěna. Mezi příklady patří uzlu nedostatek místa na disku (nebo jiných vlastností celého systému, jako je například paměť, připojení) a po uzlu. Název uzlu hello (string) je identifikována Hello uzlu entity.
* **Aplikace**. Představuje hello stavu instance aplikace spuštěná v clusteru hello. Stavu aplikací sestavy popisují podmínky, které ovlivňují hello celkového stavu aplikace hello. Nemůže být zúžit dolů tooindividual, děti (služby nebo aplikace nasazené). Mezi příklady patří hello začátku do konce interakce mezi různými službami v aplikaci hello. Hello aplikace identifikován podle názvu aplikace hello (URI).
* **Služba**. Představuje hello stav služby spuštěné v clusteru hello. Stav služeb sestav popisují podmínky, které ovlivňují hello celkový stav služby hello. Hello ohlašování nelze zúžit hello problém tooan není v pořádku, oddíl nebo repliky. Mezi příklady patří konfigurace služby (například port nebo externí sdílené složky), která způsobuje problémy pro všechny oddíly. entity služby Hello je identifikována hello název služby (URI).
* **Oddíl**. Představuje stav hello oddílu služby. Sestavy stavu oddílu popisují podmínky, které ovlivňují hello celé sadě replik. Mezi příklady patří při hello počet replik nedosahuje počtu cílových a při oddíl je ve ztrátě kvora. Hello oddílu identifikován hello oddílem ID (GUID).
* **Repliky**. Představuje stav hello repliku stavové služby nebo instance bezstavové služby. replika Hello je hello nejmenší jednotku, která watchdogs a součástí systému můžete sestavy pro aplikaci. Pro stavové služby příklady primární repliky, která nelze replikovat operations toosecondaries a pomalé replikace. Navíc bezstavové instance může hlásit, pokud je dostatek prostředků nebo má problémy s připojením. Entita repliky Hello je identifikována hello ID (GUID) a hello replik nebo instancí ID oddílu (dlouho).
* **DeployedApplication**. Představuje hello stav *aplikace běží na uzlu s*. Sestavy o stavu nasazení aplikace popisují podmínky konkrétní toohello aplikace v hello uzlu, který nemůže být co nejlépe určen tooservice balíčky nasazené na hello stejného uzlu. Mezi příklady patří chyby, pokud v tomto uzlu nelze stáhnout balíček aplikace a problémy nastavení aplikace objekty zabezpečení na uzlu hello. Hello nasazené aplikace je identifikován název aplikace (URI) a název uzlu (string).
* **DeployedServicePackage**. Představuje stav hello balíček služby běží na uzlu v clusteru hello. Další balíčky služeb popisuje balíček služby konkrétní tooa podmínky, které neovlivňují hello na hello stejný uzel pro hello stejná aplikace. Mezi příklady patří balíček kódu v balíčku služby hello, který nelze spustit a konfigurační balíček, který nelze číst. Hello nasazený balíček služby je určen název aplikace (URI), název uzlu (string), manifestu název služby (string) a ID aktivace balíčku služby (string).

členitost Hello hello stavu modelu umožňuje snadno toodetect a odstranit problémy. Například, pokud služba nereaguje, je to vhodné tooreport, který hello instanci aplikace není v pořádku. Vytváření sestav v, úroveň není ideální, ale protože hello problém nemusí být ovlivňuje všechny hello služby v rámci této aplikace. Hello sestavy musí být služba není v pořádku použité toohello nebo tooa konkrétní podřízeného oddílu, pokud informace odkazuje toothat oddílu. Hello data automaticky plochy prostřednictvím hierarchie hello a není v pořádku oddílu je dostupná na úrovni služby a aplikace. Tato agregace pomáhá toopinpoint a vyřešte hello hlavní příčinu problému hello rychleji.

hierarchie stavu Hello se skládá z vztahů nadřazenosti a podřízenosti. Cluster se skládá z uzlů a aplikace. Aplikací mít služeb a nasazené aplikace. Nasazených aplikací mít nasazené balíčky služeb. Služby mají oddíly a každý oddíl má jednu nebo více replik. Je speciální vztah mezi uzly a nasazené entity. Není v pořádku uzlu vykazované součást systému autority, hello Failover Manager service se ovlivňuje hello nasazené aplikace, balíčky služeb a repliky na něm nasazené.

hierarchie stavu Hello představuje hello nejnovější stav systému hello podle hello nejnovější sestav stavu, což je téměř v reálném čase.
Vnitřní a vnější watchdogs sestavy hello stejných entit na základě logiky specifické pro aplikaci nebo vlastní monitorovaných podmínky. Uživatel sestavy souběžnou existenci s hello systému sestavy.

Naplánujte tooinvest v tom, jak navrhnout tooreport a reakce toohealth během hello velké cloudové služby. Tento počáteční investiční díky hello služby jednodušší toodebug, monitorování a pracovat.

## <a name="health-states"></a>Stavy
Service Fabric používá tři stavy toodescribe stavu, zda entita je nebo není v pořádku: OK, upozornění a chyb. Všechny sestavy odeslané toohello stavu úložiště musíte zadat jednu z těchto stavů. Výsledek vyhodnocení stavu Hello je jedním z těchto stavů.

Hello možné [stavů](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate) jsou:

* **OK**. Hello entity je v pořádku. Neexistují žádné známé problémy, které jsou ohlášeny ho nebo jejích podřízených objektech (v případě potřeby).
* **Upozornění**. Hello entita má některé problémy, ale mohou i nadále fungovat správně. Například je, ale nezpůsobí ještě všechny funkční problémy. V některých případech může opravit podmínky upozornění hello bez zásahu externí sám sebe. V těchto případech sestav stavu zvýšení povědomí a poskytují přehled o co se děje. V jiných případech může snížit podmínky upozornění hello do k závažnému problému bez zásahu uživatele.
* **Chyba**. Hello entity není v pořádku. Opatření toofix hello stavu hello entity, protože nemůže správně fungovat.
* **Neznámý**. v hello health store Hello entita neexistuje. Tento výsledek je možné získat z hello distribuované dotazy, které sloučení výsledky z několika součástí. Například hello get uzlu dotaz přejde příliš**FailoverManager**, **ClusterManager**, a **HealthManager**; získání aplikace přejde dotaz příliš **ClusterManager** a **HealthManager**. Tyto dotazy sloučení výsledky z několika komponent systému. Pokud součást jiného systému vrací entity, která se nenachází v health store, hello výsledného má neznámý stav. Entity není v úložišti, protože stav sestavy nebyly dosud zpracovány nebo hello entity byla vyčištěna po jeho odstranění.

## <a name="health-policies"></a>Zásady stavu
úložiště stavu Hello platí toodetermine zásady stavu entity je v pořádku na základě jeho sestavy a její podřízené položky.

> [!NOTE]
> Zásady stavu mohou být zadané v manifestu clusteru hello (pro vyhodnocení stavu clusteru a uzel) nebo v manifestu aplikace hello (pro vyhodnocení aplikace a všech jeho podřízených položek). Žádosti o vyhodnocení stavu můžete předat také v vlastní stavu vyhodnocení zásady, které se používají pouze pro vyhodnocení.
> 
> 

Ve výchozím nastavení Service Fabric používá striktní pravidla (vše, co musí být v pořádku) pro hierarchický vztah hello nadřazený podřízený. Pokud ani jednu z podřízených hello má jednu událost není v pořádku, považuje za hello nadřazené není v pořádku.

### <a name="cluster-health-policy"></a>Zásady stavu clusteru
Hello [clusteru zásady stavu](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy) je stav clusteru hello použité tooevaluate stavu a uzel stavů. Hello zásad může být definováno v manifestu clusteru hello. Pokud není přítomen, použije se výchozí zásady hello (nula. povolená počet selhání).
zásady stavu clusteru Hello obsahuje:

* [ConsiderWarningAsError](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.considerwarningaserror). Určuje, zda tootreat upozornění stavu sestavy jako chyby během vyhodnocení stavu. Výchozí: false.
* [MaxPercentUnhealthyApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications). Určuje maximální povolenou procento hello aplikací, které může být není v pořádku, než hello clusteru považován za chybu.
* [MaxPercentUnhealthyNodes](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes). Určuje maximální. povolená procento hello uzlů, které může být není v pořádku, než hello clusteru považován za chybu. Ve velkých clusterech, některé uzly jsou vždy dolů nebo se pro opravy, takže toto procento by měly být nakonfigurované tootolerate který.
* [ApplicationTypeHealthPolicyMap](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap). Hello aplikace typ stavu zásad mapy můžete použít během typy speciální aplikací toodescribe vyhodnocení stavu clusteru. Ve výchozím nastavení jsou všechny aplikace umístit do fondu a vyhodnotit s MaxPercentUnhealthyApplications. Pokud některé typy aplikací by měly zpracovávat odděleně, může být přijata mimo hello globální fond. Místo toho se vyhodnocují proti hello procenta přidružené k jejich název typu aplikace v mapě hello. Například v clusteru s podporou existují tisíce různých typů aplikací a několik instancí aplikace ovládacího prvku typu speciální aplikace. řízení aplikace Hello by nikdy chyby. Můžete určit globální MaxPercentUnhealthyApplications too20 % tootolerate některé chyby, ale pro hello nastavte typ aplikace "ControlApplicationType" hello MaxPercentUnhealthyApplications too0. Tímto způsobem, pokud některé hello mnoho aplikací není v pořádku, ale pod hello globální procento není v pořádku, hello clusteru bude vyhodnocena tooWarning. Stav varování nemá negativní vliv na upgrade clusteru nebo jiných monitorování aktivovány chybového stavu. Ale aplikace i jeden ovládací prvek v chybě by clusteru není v pořádku, která aktivuje vrácení nebo pozastaví hello upgrade clusteru, v závislosti na konfiguraci upgradu hello.
  Pro typy aplikace hello definované v mapě hello všechny instance aplikace se vyjímají z hello globální fond aplikací. Vyhodnocují na základě celkového počtu aplikací typu aplikace hello hello, pomocí hello konkrétní MaxPercentUnhealthyApplications z hello mapy. Všechny ostatní aplikace hello hello zůstanou hello globálního fondu a vyhodnocují se s MaxPercentUnhealthyApplications.

Následující ukázka Hello je výpisem z manifestu clusteru. toodefine položky v mapě typ hello aplikace, název parametru hello předponu s "ApplicationTypeMaxPercentUnhealthyApplications-", za nímž následuje název typu aplikace hello.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Zásady stavu aplikace
Hello [zásady stavu aplikace](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy) popisuje, jak se provádí vyhodnocení hello události a podřízené stavy agregace pro aplikace a jejich podřízené položky. Může být definováno v manifestu aplikace hello **ApplicationManifest.xml**, v balíčku aplikace hello. Pokud nejsou zadány žádné zásady, Service Fabric předpokládá, že se hello entity není v pořádku, pokud má sestavy stavu, nebo podřízenou položku v hello upozornění nebo chyby stavu.
konfigurovat zásady Hello jsou:

* [ConsiderWarningAsError](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.considerwarningaserror.aspx). Určuje, zda tootreat upozornění stavu sestavy jako chyby během vyhodnocení stavu. Výchozí: false.
* [MaxPercentUnhealthyDeployedApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications). Určuje maximální hello tolerovat procento nasazených aplikací, které se dají není v pořádku, než aplikace hello je považována za chybu. Se počítá jako podíl hello počet není v pořádku nasazených aplikací přes hello počet uzlů, které jsou v clusteru hello aktuálně nasazená aplikace hello v. Výpočet Hello zaokrouhlí tootolerate jednou chybou na malém počtu uzlů. Výchozí procento: nula.
* [DefaultServiceTypeHealthPolicy](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy). Určuje hello výchozí služby typu zásady stavu, která nahrazuje hello výchozí zásady stavu pro všechny typy služby v aplikaci hello.
* [ServiceTypeHealthPolicyMap](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap). Obsahuje mapování zásad stavu service podle typu služby. Tyto zásady nahraďte zásady stavu hello výchozí služby typu pro každý typ zadaná služba. Pokud aplikace má typ služby bezstavové brány a service type stavový stroj, můžete je třeba nakonfigurovat zásady stavu hello pro jejich hodnocení jinak. Když zadáte zásady, za typ služby, můžete získat členitější řízení toho hello stavu služby hello.

### <a name="service-type-health-policy"></a>Zásady stavu typ služby
Hello [služby typu zásady stavu](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy) Určuje, jak tooevaluate a agregace hello služby a hello podřízené objekty služby. Hello zásada obsahuje:

* [MaxPercentUnhealthyPartitionsPerService](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice). Určuje procento hello maximální tolerovat není v pořádku oddílů, než služba není v pořádku. Výchozí procento: nula.
* [MaxPercentUnhealthyReplicasPerPartition](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition). Určuje procento hello maximální tolerovat repliky není v pořádku, než oddíl není v pořádku. Výchozí procento: nula.
* [MaxPercentUnhealthyServices](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices). Určuje procento hello maximální tolerovat služby není v pořádku, než aplikace hello je považována za není v pořádku. Výchozí procento: nula.

Následující ukázka Hello je výňatek ze manifest aplikace:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Vyhodnocení stavu
Uživatele a automatizované služby můžete kdykoli vyhodnocení stavu pro všechny entity. tooevaluate stavu entity, hello health store agreguje všechny stavy hlásí hello entity a vyhodnocuje všechny její podřízené položky (v případě potřeby). algoritmus agregace stavu Hello používá zásady stavu, které určují, jak tooevaluate stavu sestavy a jak tooaggregate podřízené stav (v případě potřeby).

### <a name="health-report-aggregation"></a>Agregace sestav stavu
Jedna entita může mít několik sestav stavu poslal jiný reporters (součásti systému nebo watchdogs) na jiné vlastnosti. používá agregace Hello hello zásady přidružené stavu, zejména hello ConsiderWarningAsError členem aplikace nebo clusteru zásady stavu. Určuje ConsiderWarningAsError jak tooevaluate upozornění.

Hello agregovaný stav je aktivován hello *nejhorší* stavu hlásí hello entity. Pokud je sestava stavu alespoň jednu chybu, hello agregovaný stav je k chybě.

![Agregace sestavy stavu s zprávu o chybách.][2]

Stav entity, který má jeden nebo více sestav stavu chyby je vyhodnocena jako chyba. Hello totéž platí pro zprávu o stavu s ukončenou platností, bez ohledu na jeho stav.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Pokud nejsou žádné zprávy o chybách a jeden nebo více upozornění, hello agregovaný stav je upozornění nebo chyby, v závislosti na hello ConsiderWarningAsError zásad příznak.

![Agregace sestavy stavu se sestava upozornění a ConsiderWarningAsError false.][3]

Agregace sestavy stavu se sestava upozornění a ConsiderWarningAsError nastavit toofalse (výchozí).

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Podřízená položka stavu
Hello agregovaný stav entity odráží hello podřízené stavů (v případě potřeby). Hello algoritmus pro souhrn stavů podřízené používá hello stavu zásady použitelné na základě typu entity hello.

![Podřízená položka stavu entity.][4]

Podřízená položka na základě zásad stavu.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Po vyhodnotit všechny podřízené objekty hello úložiště stavu hello agreguje jejich stavů podle hello nakonfigurované maximální procento není v pořádku, děti. Toto procento je převzat ze hello zásad na základě typu entity a podřízené hello.

* Pokud všechny podřízené objekty mají OK stavy, hello podřízené Agregovat stav je OK.
* Pokud podřízené objekty mají OK a upozornění stavy hello podřízené Agregovat stav je upozornění.
* Pokud existují podřízené objekty s chybové stavy, které neodpovídají hello maximální povolené procento není v pořádku, děti, hello agregovaný stav je k chybě.
* Pokud hello podřízené objekty s chybové stavy ohledem hello maximální povolené procento není v pořádku, děti hello agregovaný stav je upozornění.

## <a name="health-reporting"></a>Vytváření sestav stavu
Součásti systému, aplikace systému prostředků infrastruktury a interní/externí watchdogs může hlásit proti Service Fabric entity. Ujistěte se, Hello reporters *místní* stanovení stavu hello hello monitorovat entit, na základě podmínek hello jejich monitorování. Nemusí toolook v jakékoli globální stav nebo agregovaná data. Hello požadovaného chování je jednoduchý reporters toohave a ne s komplexními organismy, které je třeba toolook na mnoho věcí tooinfer toosend jaké informace.

toosend stavu toohello stavu úložišti, zpravodaje, která potřebuje tooidentify hello vliv entity a vytvoření sestavy stavu. Sestava hello toosend, použijte hello [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) rozhraní API, sestava stavu rozhraní API zveřejněné na hello `Partition` nebo `CodePackageActivationContext` objekty, rutiny prostředí PowerShell nebo REST.

### <a name="health-reports"></a>Sestavy o stavu
Hello [sestav stavu](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthreport) pro každou hello entit v clusteru hello obsahovat hello následující informace:

* **SourceId**. Řetězec, který jednoznačně identifikuje hello ohlašování hello stavu události.
* **Identifikátor entity**. Identifikuje hello entity, kde je použito hello sestavy. Liší se podle hello [typ entity](service-fabric-health-introduction.md#health-entities-and-hierarchy):
  
  * Cluster. Žádné.
  * Uzel. Název uzlu (string).
  * Aplikace. Název aplikace (URI). Představuje hello název instance aplikace hello nasazen v clusteru hello.
  * Služba. Název služby (URI). Představuje název hello instance služby hello nasazen v clusteru hello.
  * Oddíl. Oddíl ID (GUID). Představuje jedinečný identifikátor oddílu hello.
  * Replika. ID repliky Hello stavové služby nebo ID instance bezstavové služby hello (INT64).
  * DeployedApplication. Název aplikace (URI) a název uzlu (string).
  * DeployedServicePackage. Název aplikace (URI), název uzlu (string) a service manifest název (string).
* **Vlastnost**. A *řetězec* (ne na pevnou výčet), který umožňuje hello ohlašování toocategorize hello stavu událost pro určité vlastnosti entity hello. Například ohlašování A mohou zasílat zprávy o stavu hello hello Node01 "Úložiště" vlastnosti a ohlašování B mohou zasílat zprávy o stavu hello vlastnosti hello Node01 "Připojení". V health store hello tyto sestavy jsou považovány za samostatné stavu události pro hello Node01 entity.
* **Popis**. Řetězec, který umožňuje ohlašování tooprovide podrobné informace o událost stavu hello. **SourceId**, **vlastnost**, a **HealthState** by měl plně popisují hello sestavy. Popis Hello přidá čitelná pro člověka informace o hello sestavy. Hello text usnadňuje správcům a uživatelům sestava stavu toounderstand hello.
* **Stav HealthState**. [Výčtu](service-fabric-health-introduction.md#health-states) hello stav hello sestavy, který popisuje. Hello přijaté hodnoty jsou OK, upozornění a chyby.
* **TimeToLive**. Časový interval, který určuje, jak dlouho sestava stavu hello je platný. Kombinaci s **RemoveWhenExpired**, umožňuje úložiště stavu hello vědět, jak tooevaluate platnost události. Ve výchozím nastavení hello hodnota je nekonečno a sestava hello je trvale platné.
* **RemoveWhenExpired**. Logická hodnota. Pokud nastavení tootrue, hello vypršela platnost stavu je automaticky odebrána z hello health store a hello sestavy nebude mít vliv na vyhodnocení stavu entity. Použít při hello sestava je platný pouze a ohlašování hello během zadaného období není nutné tooexplicitly vyčistit ho. Použije toodelete sestavy se také z hello health store (například sledovací zařízení se změní a zastaví zasílání sestav se předchozí zdrojem a vlastnosti). Sestava s krátkou TimeToLive společně s RemoveWhenExpired tooclear může poslat jakékoli předchozí stav z hello health store. Pokud toofalse je nastavena hodnota hello, hello vypršela platnost sestav považovat za chybu na vyhodnocení stavu hello. hodnota false Hello signály toohello úložiště stavu, že tento zdroj hello měli pravidelně zprávu o tuto vlastnost. Pokud tomu tak není, pak musí existovat problém se sledovacího zařízení hello. sledovací Hello zařízení stavu zachycenou zvažování hello událostí za chybu.
* **SequenceNumber**. Kladné celé číslo, které potřebuje toobe stále rostoucí, představuje hello pořadí hello sestavy. Je používána hello stavu úložiště toodetect zastaralé sestavy, které byly přijaty opožděně z důvodu zpoždění sítě nebo jiných problémů. Sestavy se odmítne, pokud hello pořadové číslo je menší než nebo rovna počtu toohello naposledy použity pro hello stejné entity, zdroje a vlastnost. Pokud není zadaný, vygeneruje se automaticky hello pořadové číslo. Je nutné tooput v hello pořadové číslo jenom v případě, že zprávy o přechodů mezi stavy. V takovém případě musí hello zdroj tooremember sestavy se odesílají a udržovat hello informace pro obnovení na převzetí služeb při selhání.

Tyto čtyři části informací – SourceId, identifikátor entity, vlastnosti a elementu HealthState – jsou vyžadovány pro každý sestava stavu. Hello SourceId řetězec není povolen toostart s předponou hello "**systému.**", který je vyhrazený pro sestavy systému. Pro hello stejné entity, je pouze jednu sestavu pro hello stejný zdroj a vlastnost. Hello více sestav pro stejný zdroj a vlastnost přepsat sebou na hello stavu na straně klienta (pokud jsou se dávkově) nebo na straně úložiště stavu hello. nahrazení Hello je založena na pořadových čísel; novější sestavy (s vyšším pořadová čísla) nahraďte starší sestavy.

### <a name="health-events"></a>Stav události
Interně hello health store udržuje [události stavu](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthevent), které obsahují všechny hello informace ze sestav hello a další metadata. Hello metadata zahrnuje hello době hello sestav byl zadán toohello stavu klienta a hello čas, kdy byl změněn na straně serveru hello. události stavu Hello se vrátí pomocí [dotazů na stav](service-fabric-view-entities-aggregated-health.md#health-queries).

Hello přidané metadat obsahuje:

* **SourceUtcTimestamp**. Sestava hello času Hello byl zadán toohello stavu klienta (Coordinated Universal Time).
* **LastModifiedUtcTimestamp**. Hello čas hello sestavy bylo naposledy změněno na straně serveru hello (Coordinated Universal Time).
* **IsExpired**. A příznak tooindicate, zda sestava hello vypršela při hello dotazu byl provedený hello health store. Událost můžete platnost pouze v případě RemoveWhenExpired je false. Hello událost, jinak hodnota není vrácených dotazem a je odebrán z úložiště hello.
* **LastOkTransitionAt**, **LastWarningTransitionAt**, **LastErrorTransitionAt**. Hello čas poslední přechody OK, upozornění nebo chyby. Tato pole poskytnout hello historii hello přechodů mezi stavy stav pro události hello.

pole přechod stavu Hello lze chytřejší výstrahy nebo informace o stavu "historických". Umožňují scénáře, jako:

* Upozornit, když vlastnost byla v upozornění nebo chyby pro více než X minut. Kontrola hello podmínku pro určitou dobu zabraňuje výstrahy na dočasné stavy. Například můžete výstrahu, pokud má byla hello stav upozornění pro více než pět minut přeložit na (stav HealthState == upozornění a nyní - LastWarningTransitionTime > 5 minut).
* Výstraha pouze na podmínky, které se změnily v hello posledních X minut. Pokud sestavu již byla v chyby před hello zadaný čas, může být ignorována, protože byl již signalizovala dříve.
* Pokud vlastnost je při přepínání mezi upozornění a chyb, zjistěte, jak dlouho byla není v pořádku (to znamená, není OK). Například můžete výstrahu, pokud vlastnost hello nebyla v pořádku po dobu více než pět minut přeložit na (stav HealthState! = Ok a teď - LastOkTransitionTime > 5 minut).

## <a name="example-report-and-evaluate-application-health"></a>Příklad: Vytvoření sestavy a vyhodnocování stavu aplikace
Hello následující příklad odešle sestavy stavu pomocí prostředí PowerShell na aplikace hello **fabric: / WordCount** ze zdroje hello **MyWatchdog**. Sestava stavu Hello obsahuje informace o stavu vlastnost hello "dostupnosti" v chybového stavu, s neomezenou TimeToLive. Pak odešle dotaz hello stavu aplikací, která vrátí agregovat chyby stavu stavu a hello hlášené události stavu hello seznamu události stavu.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Použití stavu modelu
model stavu Hello umožňuje cloudové služby a hello základní tooscale platformy Service Fabric, protože stanovení stavu a monitorování jsou rozděleny do různých monitorování hello v rámci clusteru hello.
Jiné systémy musí mít jedinou, centralizovanou služby na úrovni clusteru hello, která analyzuje všechny hello *potenciálně* užitečné informace, které služby. Tento přístup omezuje jejich škálovatelnost. Je také nedovoluje toocollect toohelp identifikovat problémy a s potenciálními problémy jako zavřít toohello příčiny nejdříve konkrétní informace.

model stavu Hello slouží výraznou pro monitorování a diagnostiku pro vyhodnocení stavu cluster a aplikace a monitorovaných upgrady. Jiné služby stavu, který opraví data tooperform automatické používat, sestavení historie stavu clusteru a vydání výstrahy na určité podmínky.

## <a name="next-steps"></a>Další kroky
[Zobrazit sestavy stavu Service Fabric](service-fabric-view-entities-aggregated-health.md)

[Pomocí sestav o stavu systému pro řešení potíží](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Jak tooreport a zkontrolujte stav služby](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Přidat vlastní sestavy stavu Service Fabric](service-fabric-report-health.md)

[Monitorování a Diagnostika služby místně](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade aplikace Service Fabric](service-fabric-application-upgrade.md)

