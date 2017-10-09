---
title: "aaaService správce prostředků infrastruktury Cluster - skupin aplikací | Microsoft Docs"
description: "Přehled hello funkce skupiny aplikací v hello správce prostředků clusteru Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 4cae2370-77b3-49ce-bf40-030400c4260d
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: b4f068862d962b53a0b3ea813b89bb13ee395681
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooapplication-groups"></a>Úvod tooApplication skupiny
Správce prostředků clusteru Service Fabric obvykle spravuje prostředky clusteru tak, že se zatížení hello (reprezentované prostřednictvím [metriky](service-fabric-cluster-resource-manager-metrics.md)) rovnoměrně v rámci clusteru hello. Service Fabric spravuje kapacitu hello hello uzlů v clusteru hello a hello clusteru jako celek prostřednictvím [kapacity](service-fabric-cluster-resource-manager-cluster-description.md). Metriky a kapacity pracovní velká pro řadu úloh, ale vzorů, které hodně využívají různé instance aplikace Service Fabric někdy předány další požadavky. Například můžete chtít:

- Záložní některé kapacita hello uzlů v clusteru hello hello služeb v rámci některé instance s názvem aplikace
- Omezit hello celkový počet uzlů, které hello služeb v rámci instance s názvem aplikace běží na (namísto šíří je po celý cluster hello)
- Definovat kapacity na samotnou instanci s názvem aplikace hello toolimit hello počet služeb nebo celková spotřeba prostředků hello služeb je uvnitř

toomeet tyto požadavky hello správce prostředků clusteru Service Fabric podporuje funkci skupiny aplikací.

## <a name="limiting-hello-maximum-number-of-nodes"></a>Omezení hello maximální počet uzlů
Hello nejjednodušší případ použití kapacity aplikace je když instance aplikace potřebuje toobe omezené tooa určité maximální počet uzlů. Tím dojde ke konsolidaci všechny služby v rámci této instance aplikace na se stanoveným počtem počítačů. Konsolidace je užitečné, když se snažíte toopredict nebo cap využití fyzických prostředků hello služby v rámci dané aplikace s názvem instance. 

Hello následující obrázek ukazuje instanci aplikace a bez maximální počet uzlů, které jsou definované:

<center>
![Instance aplikace definování maximální počet uzlů][Image1]
</center>

V levém příkladu hello hello aplikace nemá maximální počet uzlů, které jsou definované a má tři služby. Hello správce prostředků clusteru má na všech replik rozloženy šesti dostupných uzlů tooachieve hello vyvážit v clusteru hello (hello výchozí chování). V pravém příkladu hello vidíme hello stejnou aplikaci omezený počet uzlů toothree.

Hello parametr, který určuje toto chování se nazývá MaximumNodes. Tento parametr můžete nastavit při vytváření aplikace, nebo aktualizovat pro instanci aplikace, která je již spuštěna.

PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
await fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
await fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

```

V rámci hello sada uzlů hello správce prostředků clusteru není zárukou toho, jaké objekty služby získat umístit společně nebo získat uzlů, které používá.

## <a name="application-metrics-load-and-capacity"></a>Aplikace metriky zatížení a kapacity
Skupiny aplikací také umožní toodefine metriky přidružené k dané aplikaci s názvem instance a tuto instanci aplikace kapacity pro tyto metriky. Metriky aplikace povolí tootrack rezervy a omezování spotřeby prostředků hello hello služeb v této instanci aplikace.

Pro jednotlivé aplikace metriky existují dvě hodnoty, které lze nastavit:

- **Celková kapacita aplikace** – toto nastavení představuje celkové kapacity hello hello aplikace pro konkrétní metriky. Hello správce prostředků clusteru zakáže hello vytváření všech nových služeb v rámci této instance aplikace, které by způsobily celkové zatížení tooexceed tuto hodnotu. Řekněme například, instanci aplikace hello měl kapacitou 10 a již obsahuje pět zatížení. Vytvoření Hello služby se zatížením celkový výchozí 10 by povoleny.
- **Maximální kapacita uzlu** – toto nastavení určuje maximální celkové zatížení hello hello aplikace v jednom uzlu. Pokud zatížení prochází přes tuto kapacitu, hello správce prostředků clusteru přesune uzly tooother repliky tak, aby snížení technologie hello zatížení.


Prostředí PowerShell:

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -Metrics @("MetricName:Metric1,MaximumNodeCapacity:100,MaximumApplicationCapacity:1000")
```

C#:

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;
appMetric.MaximumNodeCapacity = 100;
ad.Metrics.Add(appMetric);
await fc.ApplicationManager.CreateApplicationAsync(ad);
```

## <a name="reserving-capacity"></a>Rezervaci kapacity
Jiné běžně používá pro skupin aplikací je tooensure že hello prostředků v rámci clusteru jsou vyhrazené pro instanci dané aplikaci. místo Hello je vyhrazena vždy při vytvoření instance aplikace hello.

Pro aplikace hello se stane okamžitě vyhrazením místa v clusteru hello i v případě:
- instance aplikace Hello je vytvořen, ale nemá žádné služby, v něm ještě
- pokaždé, když se změní Hello počet služeb v rámci instance aplikace hello 
- Hello služby existuje, ale nejsou spotřebovávat hello prostředků 

Rezervování prostředků pro instanci aplikace vyžaduje zadání další dva parametry: *MinimumNodes* a *NodeReservationCapacity*

- **MinimumNodes** -definuje hello minimální počet uzlů, které aplikace hello by neměl být spuštěný instance.  
- **NodeReservationCapacity** – toto nastavení je za Metrika pro aplikaci hello. Hodnota Hello je hello množství tuto metriku vyhrazené pro aplikace hello v každém uzlu, kde to hello služby v této aplikaci spusťte.

Kombinování **MinimumNodes** a **NodeReservationCapacity** zaručuje rezervace minimální zatížení pro hello aplikací v rámci clusteru hello. Pokud je menší zbývající kapacity hello clusteru než hello celkový rezervace je povinná, vytvoření aplikace hello nezdaří. 

Podívejme se na příklad rezervaci kapacity:

<center>
![Instance aplikace definování rezervované kapacity][Image2]
</center>

V levém příkladu hello aplikace nemají žádné aplikace kapacity definované. Hello správce prostředků clusteru vyrovnává všechno podle toonormal pravidla.

V příkladu hello na pravém hello Řekněme, že Application1 byl vytvořen s hello následující nastavení:

- MinimumNodes nastavit tootwo
- Metrika definovaná pomocí aplikace
  - NodeReservationCapacity 20

PowerShell

 ``` posh
 New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MinimumNodes 2 -Metrics @("MetricName:Metric1,NodeReservationCapacity:20")
 ```

C#

 ``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MinimumNodes = 2;

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.NodeReservationCapacity = 20;

ad.Metrics.Add(appMetric);

await fc.ApplicationManager.CreateApplicationAsync(ad);
```

Service Fabric rezervuje kapacitu na dva uzly pro Application1 a neumožňuje služby od Application2 tooconsume tuto kapacitu i v případě, že nejsou že žádné zatížení je spotřebovávanou službami hello uvnitř Application1 během hello. Tato vyhrazená aplikace kapacita považuje za spotřebované a započítává hello zbývající kapacity v tomto uzlu a v rámci clusteru hello.  Hello rezervace je odečtena od hello zbývající kapacita clusteru okamžitě, ale hello vyhrazené spotřeba je odečtena od hello kapacitu konkrétním uzlu jenom v případě, že je alespoň jedna služba objekt je umístěn na něm. Tento novější rezervace umožňuje flexibilitu a lepší využití prostředků vzhledem k tomu, že prostředky jsou vyhrazeny pouze na uzlech v případě potřeby.

## <a name="obtaining-hello-application-load-information"></a>Získávání informací o zatížení aplikace hello
Pro každou aplikaci, která má kapacitu aplikace definovaná pro jeden nebo více metriky můžete získat hello informace o hello agregační zatížení hlášené repliky jeho služby.

Prostředí PowerShell:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1
```

C#

``` csharp
var v = await fc.QueryManager.GetApplicationLoadInformationAsync("fabric:/MyApplication1");
var metrics = v.ApplicationLoadMetricInformation;
foreach (ApplicationLoadMetricInformation metric in metrics)
{
    Console.WriteLine(metric.ApplicationCapacity);  //total capacity for this metric in this application instance
    Console.WriteLine(metric.ReservationCapacity);  //reserved capacity for this metric in this application instance
    Console.WriteLine(metric.ApplicationLoad);  //current load for this metric in this application instance
}
```

Hello ApplicationLoad dotaz vrátí hello základní informace o kapacitě aplikace, která byla zadaná pro aplikaci hello. Tyto informace zahrnují informace o hello uzly minimální a maximální počet uzlů a hello číslo, které je aktuálně zabírá aplikace hello. Zahrnuje taky informace o jednotlivých metrika zatížení aplikací, včetně:

* Název metriky: Název metriky hello.
* Rezervaci kapacity: Clusteru rezervace kapacity v hello clusteru pro tuto aplikaci.
* Zatížení aplikací: Celkový počet zatížení replik podřízené této aplikace.
* Aplikace kapacity: Maximální povolená hodnota zatížení aplikace.

## <a name="removing-application-capacity"></a>Odebrání aplikace kapacity
Jakmile hello kapacity aplikace parametry jsou nastavené pro aplikace, budou se dá odebrat pomocí rozhraní API pro aktualizaci aplikace nebo rutiny Powershellu. Například:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Tento příkaz odebere všechny parametry kapacity správy aplikací z instance aplikace hello. To zahrnuje MinimumNodes, MaximumNodes a metriky aplikace hello, pokud existuje. účinek Hello hello příkazu se okamžitě. Po dokončení tohoto příkazu hello správce prostředků clusteru používá hello výchozí chování pro správu aplikací. Parametry kapacity aplikace mohou být zadané znovu prostřednictvím `Update-ServiceFabricApplication` / `System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.

### <a name="restrictions-on-application-capacity"></a>Omezení kapacity aplikace
Existuje několik omezení kapacity aplikace parametry, které je nutné dodržovat. Pokud nejsou chyby ověření žádné změny proběhnout.

- Všechny parametry celé číslo musí být nezáporné číslo.
- MinimumNodes, nikdy musí být větší než MaximumNodes.
- Pokud jsou definovány kapacity pro metrika zatížení, musí se postupujte tato pravidla:
  - Uzel rezervaci kapacity nesmí být větší než maximální kapacita uzlu. Například nelze omezit hello kapacity pro metriku hello "Procesoru" v hello uzlu tootwo jednotky a akci tooreserve tři jednotky na každém uzlu.
  - Pokud MaximumNodes není zadaný, nesmí být větší než celková kapacita aplikace hello produktu MaximumNodes a maximální kapacita uzlu. Předpokládejme například že hello maximální kapacita uzlu pro metrika zatížení, "Procesor" je nastavena tooeight. Také se stát, že nastavíte hello too10 maximální počet uzlů. V takovém případě celková kapacita aplikace musí být větší než 80 pro tato metrika zatížení.

jak při vytváření aplikací a aktualizací, vynutí se omezení Hello.

## <a name="how-not-toouse-application-capacity"></a>Jak toouse kapacity aplikace
- Nepokoušejte toouse hello skupiny aplikací funkce tooconstrain hello aplikace tooa _konkrétní_ dílčí sadu uzlů. Jinými slovy, můžete zadat, že aplikace hello funguje na nejvíce pět uzlů, ale není konkrétní pět uzlů, které v clusteru hello. Chovaly aplikaci toospecific uzly lze dosáhnout pomocí omezení umístění pro služby.
- Nepokoušejte tooensure kapacity aplikace hello toouse, že dvě služby z hello stejná aplikace se umístí na hello stejným uzlům. Místo toho použít [spřažení](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) nebo [omezení umístění](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).

## <a name="next-steps"></a>Další kroky
- Další informace o konfiguraci služby [Další informace o konfiguraci služby](service-fabric-cluster-resource-manager-configure-services.md)
- toofind si o tom, jak hello správce prostředků clusteru spravuje a vyrovnává zatížení v clusteru hello, podívejte se na článek hello na [Vyrovnávání zatížení](service-fabric-cluster-resource-manager-balancing.md)
- Začít od začátku hello a [získat Úvod toohello správce prostředků clusteru Service Fabric](service-fabric-cluster-resource-manager-introduction.md)
- Další informace o fungování metriky obecně, přečtěte si [metriky zatížení Service Fabric](service-fabric-cluster-resource-manager-metrics.md)
- Hello správce prostředků clusteru má mnoho možností pro popisující hello clusteru. toofind Další informace o jejich, projděte si tento článek na [popisující cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)

[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
