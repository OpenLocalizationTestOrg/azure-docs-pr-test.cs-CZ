---
title: "Model hostování služby Fabric aaaAzure | Microsoft Docs"
description: "Popisuje vztah mezi repliky (nebo instance) nasazené služby Statistika prostředků infrastruktury a hostitele služby procesu."
services: service-fabric
documentationcenter: .net
author: harahma
manager: timlt
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/15/2017
ms.author: harahma
ms.openlocfilehash: 30e0ba19cd672a722f76477a311ecef7c213b055
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-hosting-model"></a>Model hostování Service Fabric
Tento článek obsahuje přehled hostování modely poskytované Service Fabric aplikace a popisuje hello rozdíly mezi hello **sdílený proces** a **výhradní proces** modelů. Popisuje, jak vypadá nasazené aplikace v Service Fabric uzlu a vztah mezi repliky (nebo instance) hello služby a hello hostitele služby procesu.

Než budete pokračovat, ujistěte se, že jste obeznámeni s [Service Fabric aplikačního modelu] [ a1] a pochopit různé koncepty a vztah mezi nimi. 

> [!NOTE]
> V tomto článku, pro jednoduchost Pokud není výslovně uvedeno:
>
> - Všechny používá aplikace word *repliky* odkazuje tooboth repliku stavové služby nebo instance statless služby.
> - *CodePackage* ošetřených ekvivalentní je příliš*ServiceHost* proces, který registruje *ServiceType* a hostitele repliky služeb této *ServiceType*.
>

toounderstand hello model hostování, dejte nám provede příklad. Dejte nám Řekněme, máme *ApplicationType* 'MyAppType', který má *ServiceType* 'MyServiceType', který zajišťuje *ServicePackage* 'MyServicePackage', který má *CodePackage* 'MyCodePackage', který registruje *ServiceType* 'MyServiceType, když je spuštěna.

Řekněme, že máme 3 uzly clusteru a vytvoříme *aplikace* **fabric: / App1** typu 'MyAppType'. Uvnitř to *aplikace* **prostředků infrastruktury: / App1** vytvoříme služby **fabric: / App1/ServiceA** typu 'MyServiceType', která má 2 oddíly (vyslovení **P1**  &  **P2**) a 3 repliky na jeden oddíl. Hello následující diagram znázorňuje hello zobrazení této aplikace, jak se bude nakonec nasazené na uzlu.

<center>
![Zobrazení uzlu nasazené aplikace][node-view-one]
</center>

Service Fabric aktivovat 'MyServicePackage, kterou spustit 'MyCodePackage', který je hostitelem repliky z obou oddílů hello tj **P1** & **P2**. Všimněte si, že všechny hello uzly v clusteru hello budou mít stejné zobrazení vzhledem k tomu, že jsme zvolili počet replik na oddíl rovna toonumber uzlů v clusteru hello. Vytvoříme jiné služby **prostředků infrastruktury: / App1/ServiceB** v aplikaci **prostředků infrastruktury: / App1** který má 1 oddílu (vyslovení **P3**) a 3 repliky na jeden oddíl. Následující diagram znázorňuje hello nové zobrazení v uzlu hello:

<center>
![Zobrazení uzlu nasazené aplikace][node-view-two]
</center>

Jak jsme najdete v části Service Fabric umístit nového serveru repliky pro oddíl hello **P3** služby **fabric: / App1/ServiceB** v hello existující aktivace 'MyServicePackage'. Teď umožňuje vytvořit jiné *aplikace* **prostředků infrastruktury: / počítači App2** typu 'MyAppType' a uvnitř **prostředků infrastruktury: / počítači App2** vytvoření služby **fabric: / počítači App2/ServiceA** jehož 2 oddíly (vyslovení **P4** & **P5**) a 3 repliky na jeden oddíl. Následující diagramy ukazuje hello nové zobrazení uzlu:

<center>
![Zobrazení uzlu nasazené aplikace][node-view-three]
</center>

Tentokrát Service Fabric aktivoval novou kopii 'MyServicePackage', který spustí novou kopii 'MyCodePackage' a repliky z obou oddílů služby **fabric: / počítači App2/ServiceA** (tj. **P4**  &  **P5**) jsou umístěny v této nové kopie 'MyCodePackage'.

## <a name="shared-process-model"></a>Sdílené model procesu
Co jsme viděli výše je výchozí hello hostování modelu poskytované Service Fabric a označuje tooas **sdílený proces** modelu. V tomto modelu pro daného *aplikace*, jenom jeden zkopírovat z dané *ServicePackage* se aktivuje na *uzlu* (které spustí všechny hello *CodePackages* obsažené v ní) a všechny hello repliky všechny služby daný *ServiceType* jsou umístěny v hello *CodePackage* , který registruje *ServiceType*. Jinými slovy, všechny hello repliky všech služeb danou *ServiceType* sdílet hello stejný postup.

## <a name="exclusive-process-model"></a>Model výhradní procesu
Hello jiných hostitelských model poskytované Service Fabric je **výhradní proces** modelu. V tomto modelu na danou *uzlu*, k umístění každou repliku, Service Fabric aktivuje novou kopii *ServicePackage* (které spustí všechny hello *CodePackages* obsažené v ní ) a repliky je umístěn v hello *CodePackage* této registrované hello *ServiceType* z hello služby toowhich replika patří. Jinými slovy žije každé repliky ve svém vlastním procesu vyhrazené. 

Tento model je podporováno od verze 5.6 Service Fabric. **Výhradní proces** modelu může být zvolena při hello vytváření služby hello (pomocí [prostředí PowerShell][p1], [REST] [ r1]nebo [FabricClient][c1]) tak, že zadáte **ServicePackageActivationMode** jako 'ExclusiveProcess'.

```powershell
PS C:\>New-ServiceFabricService -ApplicationName "fabric:/App1" -ServiceName "fabric:/App1/ServiceA" -ServiceTypeName "MyServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1 -ServicePackageActivationMode "ExclusiveProcess"
```

```csharp
var serviceDescription = new StatelessServiceDescription
{
    ApplicationName = new Uri("fabric:/App1"),
    ServiceName = new Uri("fabric:/App1/ServiceA"),
    ServiceTypeName = "MyServiceType",
    PartitionSchemeDescription = new SingletonPartitionSchemeDescription(),
    InstanceCount = -1,
    ServicePackageActivationMode = ServicePackageActivationMode.ExclusiveProcess
};

var fabricClient = new FabricClient(clusterEndpoints);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Pokud máte výchozí služba v manifestu aplikace, můžete vybrat **výhradní proces** modelu zadáním **ServicePackageActivationMode** atributu, jak je uvedeno níže:

```xml
<DefaultServices>
  <Service Name="MyService" ServicePackageActivationMode="ExclusiveProcess">
    <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
      <SingletonPartition/>
    </StatelessService>
  </Service>
</DefaultServices>
```
Pokračujte v našem příkladu výše, umožňuje vytvořit jinou službu **prostředků infrastruktury: / App1/ServiceC** v aplikaci **fabric: / App1** jehož 2 oddílech (vyslovení **P6**  &  **N7**) a 3 repliky na oddíl s **ServicePackageActivationMode** nastavit too'ExclusiveProcess'. Následující diagram znázorňuje nové zobrazení v uzlu hello:

<center>
![Zobrazení uzlu nasazené aplikace][node-view-four]
</center>

Jak vidíte, Service Fabric aktivovat dva nové kopie 'MyServicePackage' (jeden pro každou repliku z oddílu **P6** & **N7**) a umístit každou repliku v jeho vyhrazené kopie *CodePackage*. Jiné věc toonote tady je, když **výhradní proces** model se používá, pro daného *aplikace*, více zkopíruje z dané *ServicePackage* může být aktivní v  *Uzel*. Ve výše příkladu vidíme pro aktivní tři kopie 'MyServicePackage' **fabric: / App1**. Každá z těchto aktivní kopie 'MyServicePackage' má **ServicePackageAtivationId** spojené s ním, které identifikuje tuto kopii v rámci *aplikace* **fabric: / App1**.

Při pouze **sdílený proces** model se používá pro *aplikace*, například **fabric: / počítači App2** v existuje pouze jeden aktivní kopie výše například *ServicePackage*  na *uzlu* a **ServicePackageAtivationId** u této aktivace *ServicePackage* je "prázdný řetězec".

> [!NOTE]
>- **Sdílený proces** model hostování odpovídá příliš**ServicePackageAtivationMode** rovna **SharedProcess**. Toto je výchozí hello model hostování a **ServicePackageAtivationMode** není nutné zadávat v okamžiku hello vytváření hello služby.
>
>- **Výhradní proces** model hostování odpovídá příliš**ServicePackageAtivationMode** rovna **ExclusiveProcess** a potřebovat toobe explicitně určena v době vytváření hello hello Služba. 
>
>- Model hostování služby může být známé dotazováním hello [popisu služby] [ p2] a prohlížení hodnotu **ServicePackageAtivationMode**.
>
>

## <a name="working-with-deployed-service-package"></a>Práce s nasazený balíček služby
Aktivní kopie *ServicePackage* na uzlu se označuje jako [nasazený balíček služby][p3]. Jak už jsme zmínili výše, pokud **výhradní proces** model se používá pro vytváření služeb, pro daný *aplikace*, může existovat více nasazené služby balíčky pro hello stejné  *ServicePackage*. Při provádění operací, jako balíček služby konkrétní toodeployed [vytváření sestav stavu balíčku nasazené služby] [ p4] nebo [restartování balíček kódu nasazené služby balíčku] [ p5] atd., **ServicePackageActivationId** toobe musí zadat tooidentify balíček konkrétní nasazené služby.

 **ServicePackageActivationId** nasazené služby balíček lze získat pomocí dotazu na seznam hello [nasazené balíčky služeb] [ p3] na uzlu. Při dotazování [nasazené typů služeb][p6], [nasazení repliky] [ p7] a [nasadit balíčky kódu] [ p8] na uzlu, výsledek dotazu hello také obsahuje hello **ServicePackageActivationId** nadřazená nasazená balíčku služby.

> [!NOTE]
>- V části **sdílený proces** model, hostování na daný *uzlu*, pro daný *aplikace*, jenom jeden zkopírovat z *ServicePackage* se aktivuje . Má **ServicePackageActivationId** rovnat příliš*prázdný řetězec* a není nutné zadávat při operací souvisejících s výkonem nasazený balíček služby. 
>
> - V části **výhradní proces** model, hostování na dané *uzlu*, pro daného *aplikace*, jeden nebo více kopií *ServicePackage* může být aktivní. Každá aktivace, má *neprázdný* **ServicePackageActivationId** a je třeba toobe zadaný během operací souvisejících s výkonem nasazený balíček služby. 
>
> - Pokud **ServicePackageActivationId** ommited too'empty řetězec výchozí hodnota je '. Pokud službu nasazenou balíčku, který byl aktivován pod **sdílený proces** modelu je k dispozici, pak se provede operaci, v opačném případě hello se nezdaří.
>
> - Není doporučeno tooquery jednou a mezipaměti **ServicePackageActivationId** tak, jak se dynamicky vygeneruje a můžete změnit z různých důvodů. Před provedením operace, která potřebuje **ServicePackageActivationId**, musí nejprve dotaz hello seznam [nasazené balíčky služeb] [ p3] na uzel a pak použít  *ServicePackageActivationId** z dotazu výsledek tooperform hello původní operace.
>
>

## <a name="guest-executable-and-container-applications"></a>Spustitelný soubor a kontejneru aplikace hosta
Service Fabric zpracovává [spustitelný soubor hosta] [ a2] a [kontejneru] [ a3] aplikace jako statless služby, které jsou samostatný, tj. neexistuje žádné modulu runtime Service Fabric v *ServiceHost* (procesu nebo kontejneru). Vzhledem k tomu, že tyto služby jsou samostatný, počet replik na *ServiceHost* není použitelná pro tyto služby. Hello nejběžnější konfigurace používají tyto služby je jedním oddílem s [InstanceCount] [ c2] rovnat příliš-1 (tj. jednu kopii hello kódu služby spuštěné v každém uzlu clusteru). 

Hello výchozí **ServicePackageActivationMode** pro tyto služby je **SharedProcess** v takovém případě Service Fabric aktivuje pouze jedné kopie *ServicePackage* na *Uzlu* pro danou *aplikace* což znamená, že pouze jedna kopie kódu služby se spustí *uzlu*. Pokud chcete na více kopií vaší toorun kódu služby *uzlu* při vytváření více služeb (*Service1* příliš*ServiceN*) z *ServiceType* (zadaný v *ServiceManifest*) nebo pokud je vaše služba více oddíly, musíte zadat **ServicePackageActivationMode** jako **ExclusiveProcess**hello během vytváření služby hello.

## <a name="changing-hosting-model-of-an-existing-service"></a>Změna existující službu hostování modelu
Změna hostování modelu existující službu z **sdílený proces** příliš**výhradní proces** a naopak prostřednictvím upgradovat nebo aktualizovat mechanismus (nebo ve výchozích služeb specifikace v aplikaci manifest) se aktuálně nepodporuje. Podpora pro tuto funkci bude pocházet v budoucích verzích.

## <a name="choosing-between-shared-process-and-exclusive-process-model"></a>Volba mezi sdílený proces a model výhradní procesu
Obě tyto hostování modely mají jeho výhody a nevýhody a uživatel musí tooevaluate, která nejlépe vyhovuje jejich požadavkům. **Sdílený proces** model umožňuje lepší využití prostředků operačního systému, protože jsou méně procesy vytvořený, víc replik v hello stejný proces můžete sdílet porty atd. Ale pokud jeden z repliky hello přístupy k chybě, kde je nutné toobring dolů hello hostitele služby, bude to mít vliv všech replik ve stejném procesu.

 **Výhradní proces** model zajišťuje lepší izolaci s každé repliky ve svém vlastním procesu a identifikovala repliky nebude mít vliv na ostatní repliky. Se hodit pro případy, kdy sdílení portů nepodporuje hello komunikační protokol. Zařídí hello možnost tooapply prostředků zásad správného řízení na úrovni repliky. Na druhé straně hello **výhradní proces** budou spotřebovávat více prostředků operačního systému, jak ho vytvoří jeden proces pro každou repliku na uzlu hello.

## <a name="exclusive-process-model-and-application-model-considerations"></a>Model procesu výhradní a aplikací modelu aspekty
Hello doporučená způsob toomodel aplikace v Service Fabric je tookeep jeden *ServiceType* za *ServicePackage* a tento model funguje dobře pro většinu aplikací hello. 

Ale speciální scénáře tooenable tam, kde jeden *ServiceType* za *ServicePackage* nemusí být žádoucí, funkčně, Service Fabric umožňuje toohave více než jeden *ServiceType* za *ServicePackage* (a jeden *CodePackage* můžete zaregistrovat více než jeden *ServiceType*). Zde jsou některé hello scénáře, kde může být užitečné tyto konfigurace:

- Chcete využití prostředků toooptimize operačního systému tak, že při vytváření kopie méně procesy a s vyšší hustotu repliky podle procesu.
- Repliky z různých ServiceTypes potřebovat tooshare některé běžné data, která má vysokou inicializace nebo paměti náklady.
- Máte nabídku bezplatné služby a chcete tooput omezení na využití prostředků umístěním všechny repliky hello služby ve stejném procesu.

**Výhradní proces** model hostování není souvislý s modelem aplikace s více *ServiceTypes* za *ServicePackage*. Důvodem je, že více *ServiceTypes* za *ServicePackage* je navrženou tooachieve sdílení mezi repliky vyšší prostředků a umožňuje vyšší hustotu repliky podle procesu. Toto je rozporu toowhat **výhradní proces** navrženou tooachieve je model.

Podívejte se hello více *ServiceTypes* za *ServicePackage* jiné *CodePackage* registrace každý *ServiceType*. Řekněme, že máme *ServicePackage* 'MultiTypeServicePackge', která má dva *CodePackages*:

- 'MyCodePackageA', který registruje *ServiceType* 'MyServiceTypeA'.
- 'MyCodePackageB', který registruje *ServiceType* 'MyServiceTypeB'.

Teď umožňuje vyslovení, vytvoříme *aplikace* **prostředků infrastruktury: / SpecialApp** a uvnitř **fabric: / SpecialApp** vytvoříme následující dvě služby s **výhradní Proces** modelu:

- Služba **fabric: / SpecialApp/ServiceA** typu 'MyServiceTypeA' se dva oddíly (vyslovení **P1** a **P2**) a 3 repliky na jeden oddíl.
- Služba **fabric: / SpecialApp/ServiceB** typu 'MyServiceTypeB' se dva oddíly (vyslovení **P3** a **P4**) a 3 repliky na jeden oddíl.

V daném uzlu bude mít obě služby hello dvě repliky. Vzhledem k tomu, že jsme použili **výhradní proces** modelu toocreate hello služby, Service Fabric se budou aktivovat novou kopii 'MyServicePackge' pro každou repliku. Každá aktivace, MultiTypeServicePackge"se spustí kopie 'MyCodePackageA' a 'MyCodePackageB'. Však pouze jeden 'MyCodePackageA' nebo 'MyCodePackageB' bude hostitelem hello repliky, pro který byla aktivována 'MultiTypeServicePackge'. Následující diagram znázorňuje zobrazení uzlu hello:

<center>
![Zobrazení uzlu nasazené aplikace][node-view-five]
</center>

 Jak jsme můžete vidět v hello aktivace 'MultiTypeServicePackge' pro repliku oddílu **P1** služby **fabric: / SpecialApp/ServiceA**, 'MyCodePackageA' je hostitelem repliky hello a. MyCodePackageB se právě spuštěný a funkční. Podobně při aktivaci 'MultiTypeServicePackge' pro repliku oddílu **P3** služby **fabric: / SpecialApp/ServiceB**'MyCodePackageB' je hostitelem repliky hello a je 'MyCodePackageA. právě spuštěná a tak dále. Proto více hello počet *CodePackages* (registraci různých *ServiceTypes*) za *ServicePackage*, vyšší bude využití redundantní prostředků. 
 
 Na druhé straně hello, pokud se nám vytvořit služby **fabric: / SpecialApp/ServiceA** a **fabric: / SpecialApp/ServiceB** s **sdílený proces** modelu, Service Fabric se Aktivace pouze jedné kopie 'MultiTypeServicePackge' pro *aplikace* **fabric: / SpecialApp** (jak jsme viděli dříve). 'MyCodePackageA' bude hostitelem všechny repliky pro službu **fabric: / SpecialApp/ServiceA** (nebo všechny služby typu 'MyServiceTypeA' toobe přesnější) a 'MyCodePackageB' bude hostitelem všechny repliky pro službu **fabric: / SpecialApp/ServiceB** (nebo všechny služby typu 'MyServiceTypeB' toobe přesnější). Následující diagram znázorňuje zobrazení uzlu hello na toto nastavení: 

<center>
![Zobrazení uzlu nasazené aplikace][node-view-six]
</center>

V hello výše příklad, může domníváte, že pokud zaregistruje 'MyCodePackageA', 'MyServiceTypeA' a 'MyServiceTypeB' a neexistuje žádný MyCodePackageB, pak budou existovat žádné redundantní *CodePackage* systémem. Je to v pořádku, ale, jak je uvedeno nahoře, tento model aplikace nejsou správně zarovnané s **výhradní proces** model hostování. Pokud je cílem tooput každé repliky ve svém vlastním vyhrazeným procesem pak registrace obě *ServiceTypes* ze stejné *CodePackage* nepotřebujete a vložení každý *ServiceType* v vlastní *ServicePacakge* je více fyzických volbou.

## <a name="next-steps"></a>Další kroky
[Balíček aplikace] [ a4] a mohli ho připravené toodeploy.

[Nasazení a odebírat aplikace] [ a5] popisuje, jak instancemi aplikace toomanage toouse prostředí PowerShell.

<!--Image references-->
[node-view-one]: ./media/service-fabric-hosting-model/node-view-one.png
[node-view-two]: ./media/service-fabric-hosting-model/node-view-two.png
[node-view-three]: ./media/service-fabric-hosting-model/node-view-three.png
[node-view-four]: ./media/service-fabric-hosting-model/node-view-four.png
[node-view-five]: ./media/service-fabric-hosting-model/node-view-five.png
[node-view-six]: ./media/service-fabric-hosting-model/node-view-six.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-deploy-existing-app.md
[a3]: service-fabric-containers-overview.md
[a4]: service-fabric-package-apps.md
[a5]: service-fabric-deploy-remove-applications.md

[r1]: https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-createservice

[c1]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync
[c2]: https://docs.microsoft.com/dotnet/api/system.fabric.description.statelessservicedescription.instancecount

[p1]: https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricservice
[p2]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricservicedescription
[p3]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicePackage
[p4]: https://docs.microsoft.com/powershell/servicefabric/vlatest/send-servicefabricdeployedservicepackagehealthreport
[p5]: https://docs.microsoft.com/powershell/servicefabric/vlatest/restart-servicefabricdeployedcodepackage
[p6]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicetype
[p7]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedreplica
[p8]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedcodepackage