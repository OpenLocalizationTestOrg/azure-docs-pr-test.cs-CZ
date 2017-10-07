---
title: aaaHow tooview Azure Service Fabric entit agregovat stavu | Microsoft Docs
description: "Popisuje, jak tooquery, zobrazit a vyhodnotit agregovaný stav entity Azure Service Fabric, prostřednictvím dotazů na stav a obecné dotazy."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: fa34c52d-3a74-4b90-b045-ad67afa43fe5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: oanapl
ms.openlocfilehash: add810551cac26d2b4ff81b57d94ddd780c2cc2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-service-fabric-health-reports"></a>Zobrazit sestavy stavu Service Fabric
Představuje Azure Service Fabric [stavu modelu](service-fabric-health-introduction.md) s entity stavu, ve které součásti systému a watchdogs můžou sestavy místní podmínky, které jsou monitorování. Hello [úložiště stavu](service-fabric-health-introduction.md#health-store) slučuje všechny toodetermine data stavu zda entity jsou v pořádku.

Hello clusteru se automaticky zadá sestavy o stavu odeslaných hello komponent systému. Další informace v [stavu systému pomocí sestavy tootroubleshoot](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Service Fabric nabízí několik způsobů tooget hello agregovat stavu hello entit:

* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) nebo jiné vizualizace nástroje
* Dotazy na stav (pomocí prostředí PowerShell, rozhraní API nebo REST)
* Obecné dotazuje to návratový seznam entit, které mají stav jako jedna z vlastností hello (pomocí prostředí PowerShell, rozhraní API nebo REST)

toodemonstrate tyto možnosti, budeme používat místní cluster s pěti uzly a hello [fabric: / aplikace WordCount](http://aka.ms/servicefabric-wordcountapp). Hello **fabric: / WordCount** aplikace obsahuje dvě výchozí služby, stavové služby typu `WordCountServiceType`a bezstavové služby typu `WordCountWebServiceType`. Po změně hello `ApplicationManifest.xml` toorequire sedm cíl replik pro stavové služby hello a jeden oddíl. Protože jsou pouze pět uzlů v clusteru hello, součásti systému hello sestavy upozornění oddílu služby hello protože je pod počtu cílových hello.

```xml
<Service Name="WordCountService">
<<<<<<< HEAD
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="3">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
=======
  <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
    <UniformInt64Partition PartitionCount="[WordCountService_PartitionCount]" LowKey="1" HighKey="26" />
  </StatefulService>
>>>>>>> 5e84dbdd8e45a5d6b36f435a550b7433b873bf11
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Stav v Service Fabric Exploreru
Service Fabric Explorer nabízí vizuální zobrazení hello clusteru. V následující hello obrázek můžete uvidíte, že:

* Hello aplikace **fabric: / WordCount** je červený (v chyba), protože obsahuje událost chyby hlášené **MyWatchdog** pro vlastnost hello **dostupnosti**.
* Jeden z jejích služeb **fabric: / WordCount/WordCountService** žlutý (v upozornění). Služba Hello je nakonfigurována s sedm repliky a hello cluster obsahuje pět uzlů, takže dvě repicas nemůže být umístěn. Je sice není vidět zde, oddílu služby hello žlutý kvůli sestavy systému z `System.FM` oznámením, že `Partition is below target replica or instance count`. aktivační události žlutý oddílu Hello hello žlutý služby.
* Hello clusteru je red kvůli hello red aplikace.

vyhodnocení Hello používá výchozí zásady z manifestu clusteru hello a manifest aplikace. Jsou striktní zásady a není tolerovat žádné chyby.

Zobrazení hello clusteru Service Fabric Explorer:

![Zobrazení hello clusteru Service Fabric Exploreru.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [!NOTE]
> Další informace o [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
>
>

## <a name="health-queries"></a>Dotazy na stav
Service Fabric zpřístupní dotazů na stav pro každý hello podporované [typy entit](service-fabric-health-introduction.md#health-entities-and-hierarchy). Je přístupný prostřednictvím rozhraní API, pomocí metody na hello [FabricClient.HealthManager](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet), rutiny prostředí PowerShell a REST. Tyto dotazy vrátí informace o hello entity stavu dokončení: hello Agregovat stav, události stavu entity, podřízené stavů (v případě potřeby), není v pořádku hodnocení (Pokud hello entity není v pořádku) a podřízené objekty stavu statistiky (při použít).

> [!NOTE]
> Stav entity je vrácena, pokud je plně naplněna v hello health store. Hello entity musí být aktivní (nebyl odstraněn) a mít sestavy systému. Jeho nadřazené entity v řetězu hello hierarchie musí mít také sestav systému. Pokud nejsou splněné některé z těchto podmínek, je vrácena hello stavu dotazy [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) s [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) `FabricHealthEntityNotFound` který ukazuje, proč se nevrátí hello entity.
>
>

dotazy na stav Hello musí projít identifikátor entity hello, což závisí na typu entity hello. dotazy Hello přijmout parametry zásad volitelné stavu. Pokud nejsou zadány žádné zásady stavu, hello [zásady stavu](service-fabric-health-introduction.md#health-policies) z manifestu clusteru nebo aplikace hello slouží k vyhodnocení. Pokud hello manifesty nesmí obsahovat definice pro zásady stavu, zásady stavu výchozí hello se používají pro vyhodnocení. zásady stavu výchozí Hello není tolerovat případných selhání. dotazy Hello také přijmout filtry pro vrácení pouze částečný podřízené objekty nebo události – hello ty, které jsou respektujících hello zadaných filtrů. Jiný filtr povoluje, s výjimkou statistiky hello podřízené objekty.

> [!NOTE]
> Hello výstupní filtry se použijí na straně serveru hello, tak se snižuje velikost hello zprávy odpovědi. Doporučujeme použít filtry výstup hello toolimit hello data vrácená, ne však použít filtry na straně klienta hello.
>
>

Stav entity obsahuje:

* Hello agregován stav entity hello. Vypočtená podle hello stavu úložiště na základě sestav stavu entity, podřízené stavů (v případě potřeby) a zásad stavu. Další informace o [vyhodnocení stavu entity](service-fabric-health-introduction.md#health-evaluation).  
* události stavu Hello u hello entity.
* Hello kolekce stavů všechny podřízené objekty pro hello entity, které může mít podřízené objekty. Hello stavů obsahovat identifikátory entity a hello agregovaný stav. dokončení health tooget dítěte, volání stavu hello dotazu pro typ entity podřízené hello a předat hello podřízené identifikátor.
* Tento bod toohello sestavy, který není v pořádku hodnocení Hello spuštěna hello stavu entity hello, pokud hello entity není v pořádku. hodnocení Hello jsou rekurzivní, obsahující hello podřízené objekty stavu hodnocení, které spouštějí aktuálním stavu. Například sledovací zařízení oznámil chybu pro repliku. Stav aplikace Hello ukazuje, není v pořádku zkušební verzi z důvodu tooan služby není v pořádku; Hello služby není v pořádku kvůli tooa oddílu v chybě; Hello oddílu není v pořádku kvůli tooa repliky v chybě; replika Hello je z důvodu sestava stavu chyb toohello sledovací zařízení není v pořádku.
* Statistika Hello stavu pro všechny podřízené objekty typu hello entity, které mají podřízené objekty. Například stav clusteru ukazuje hello celkový počet aplikací, služeb, oddíly, repliky a nasadit entity v clusteru hello. Stav služby zobrazí hello celkový počet oddílů a repliky v části hello zadaná služba.

## <a name="get-cluster-health"></a>Získání stavu clusteru
Vrátí hello stavu entity hello clusteru a obsahuje hello stavů aplikací a uzly (podřízené objekty daného clusteru hello). Vstup:

* [Nepovinné] hello zásady stavu clusteru používá tooevaluate hello uzly a události hello clusteru.
* [Nepovinné] hello mapy zásady stavu aplikace, pomocí zásad stavu hello používá manifestu zásady aplikací toooverride hello.
* [Nepovinné] Filtry pro události, uzly a aplikace, které určí položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby). Všechny události, uzly a aplikace jsou stav entity agregovat hello použité tooevaluate, bez ohledu na to hello filtru.
* [Nepovinné] Filtrujte tooexclude stavu statistiky.
* [Nepovinné] Filtrovat tooinclude fabric: / statistiky stavu systému v hello statistiky stavu. Platí jenom při nejsou vyloučení hello stavu statistiky. Ve výchozím nastavení hello stavu statistiky obsahuje pouze statistiku pro uživatele aplikace a hello aplikaci systému.

### <a name="api"></a>Rozhraní API
tooget clusteru stavu, vytvořte `FabricClient` a volání hello [GetClusterHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthasync) metoda na jeho **HealthManager**.

Hello následující volání získá hello clusteru stavu:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

Hello následující kód získá stav clusteru hello pomocí zásad stavu vlastní clusteru a filtry pro uzly a aplikace. Se určuje, že hello stavu statistiky obsahovat hello fabric: / System statistiky. Vytvoří [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthquerydescription), který obsahuje vstupní informace hello.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var healthStatisticsFilter = new ClusterHealthStatisticsFilter()
{
    ExcludeHealthStatistics = false,
    IncludeSystemApplicationHealthStatistics = true
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
    HealthStatisticsFilter = healthStatisticsFilter
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Hello rutiny tooget hello clusteru stav je [Get-ServiceFabricClusterHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealth). Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.

Hello stav clusteru hello je pět uzlů, hello systémová aplikace fabric: / WordCount nakonfigurována, jak je popsáno.

následující rutina Hello získá stav clusteru pomocí výchozích zásad stavu. Hello agregovaný stav je upozornění, protože hello fabric: / WordCount aplikace je v upozornění. Všimněte si, jak hello není v pořádku hodnocení poskytují podrobnosti o hello podmínky, které aktivuje hello agregovat stavu.

```xml
PS D:\ServiceFabric> Get-ServiceFabricClusterHealth


AggregatedHealthState   : Warning
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.
                          
                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.
                          
                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                          
                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.
                          
                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                          
                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                          
                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                          
                          
NodeHealthStates        : 
                          NodeName              : _Node_4
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_3
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_1
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_0
                          AggregatedHealthState : Ok
                          
ApplicationHealthStates : 
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok
                          
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning
                          
HealthEvents            : None
HealthStatistics        : 
                          Node                  : 5 Ok, 0 Warning, 0 Error
                          Replica               : 6 Ok, 0 Warning, 0 Error
                          Partition             : 1 Ok, 1 Warning, 0 Error
                          Service               : 1 Ok, 1 Warning, 0 Error
                          DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                          DeployedApplication   : 5 Ok, 0 Warning, 0 Error
                          Application           : 0 Ok, 1 Warning, 0 Error
```

Hello následující rutiny prostředí PowerShell získá hello stavu hello clusteru pomocí zásad vlastní aplikaci. Filtruje výsledky tooget jenom aplikace a uzly ve chyby nebo upozornění. V důsledku toho jsou vráceny žádné uzly, jako jsou všechny v pořádku. Pouze hello fabric: / aplikace WordCount respektuje filtr aplikace hello. Protože vlastní zásady hello určuje tooconsider upozornění jako chyby pro hello fabric: / WordCount aplikace, aplikace hello je vyhodnocena jako chyba, a stejně tak hello clusteru.

```powershell
PS D:\ServiceFabric> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error" -ExcludeHealthStatistics


AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.
                          
                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.
                          
                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                          
                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                          
                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                          
                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                          
                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.
                          
                          
NodeHealthStates        : None
ApplicationHealthStates : 
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error
                          
HealthEvents            : None
```

### <a name="rest"></a>REST
Můžete získat stav clusteru s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-by-using-a-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.

## <a name="get-node-health"></a>Získat stav uzlu
Vrátí hello stavu uzlu entity a obsahuje události stavu hello ohlášeny hello uzlu. Vstup:

* Název uzlu hello [požadované], identifikující hello uzlu.
* Nastavení zásad stavu clusteru [Nepovinné] hello používá tooevaluate stavu.
* [Nepovinné] Filtry pro události, které určují, položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby). Všechny události jsou stav entity agregovat hello použité tooevaluate, bez ohledu na to hello filtru.

### <a name="api"></a>Rozhraní API
stav uzlu tooget prostřednictvím hello rozhraní API, vytvoření `FabricClient` a volání hello [GetNodeHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getnodehealthasync) metoda na jeho HealthManager.

Hello následující kód získá stav uzlu hello název zadaný uzel hello:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

Hello následující kód získá hello uzlu stavu pro hello zadaný název uzlu a předává v filtr událostí a vlastní zásady prostřednictvím [NodeHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.nodehealthquerydescription):

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Hello rutiny tooget hello uzlu stav je [Get-ServiceFabricNodeHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnodehealth). Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.
následující rutiny Hello získá stav uzlu hello pomocí výchozí zásady stavu:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 7/13/2017 4:39:23 PM
                        ReceivedAt            : 7/13/2017 4:40:47 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 4:40:47 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Hello následující rutiny získá hello stav všech uzlů v clusteru hello:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_4                     Ok
_Node_3                     Ok
_Node_2                     Ok
_Node_1                     Ok
_Node_0                     Ok
```

### <a name="rest"></a>REST
Můžete získat stav uzlu s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node-by-using-a-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.

## <a name="get-application-health"></a>Získání stavu aplikací
Vrátí stav hello entity aplikací. Obsahuje hello stavů hello nasazené aplikace a služby dětí. Vstup:

* [Požadované] hello název aplikace (URI) identifikující aplikace hello.
* [Nepovinné] hello zásady stavu aplikace použít manifestu zásady aplikací toooverride hello.
* [Nepovinné] Filtry pro událostí, služeb a nasazené aplikace, které určují, položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby). Všechny události, služby a nasazené aplikace jsou stav entity agregovat hello použité tooevaluate, bez ohledu na to hello filtru.
* [Nepovinné] Filtrujte tooexclude hello stavu statistiky. Pokud není zadaný, zahrnují hello stavu statistiky hello ok, upozornění a počet chyb pro všechny aplikace, děti: nasazené aplikace služby, oddíly, repliky a nasazené balíčky služeb.

### <a name="api"></a>Rozhraní API
Stav aplikace tooget, vytvořit `FabricClient` a volání hello [GetApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getapplicationhealthasync) metodu na jeho HealthManager.

Hello následující kód získá stav aplikace hello hello zadaný název aplikace (URI):

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

Hello následující kód získá stav aplikace hello hello zadaný název aplikace (URI), s filtry a vlastní zásady zadaný prostřednictvím [ApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.applicationhealthquerydescription).

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Stav aplikací Hello rutiny tooget hello je [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps). Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.

Hello následující rutina vrátí stav hello hello **fabric: / WordCount** aplikace:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                                  
                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                                  
ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok
                                  
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning
                                  
DeployedApplicationHealthStates : 
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok
                                  
HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/13/2017 5:57:05 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM
                                  
HealthStatistics                : 
                                  Replica               : 6 Ok, 0 Warning, 0 Error
                                  Partition             : 1 Ok, 1 Warning, 0 Error
                                  Service               : 1 Ok, 1 Warning, 0 Error
                                  DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                                  DeployedApplication   : 5 Ok, 0 Warning, 0 Error
```

Hello následující předává rutiny prostředí PowerShell v vlastní zásady. Také filtry, děti a události.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                                  
                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.
                                  
ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error
                                  
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>REST
Můžete získat stav aplikací s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application-by-using-an-application-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.

## <a name="get-service-health"></a>Získání stavu služby
Vrátí stav hello entity služby. Obsahuje hello oddílu stavů. Vstup:

* [Požadované] hello název služby (URI) identifikující hello služby.
* [Nepovinné] hello zásady stavu aplikace použít toooverride hello aplikace manifestu zásad.
* [Nepovinné] Filtry pro události a oddíly, které určují, položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby). Všechny události a oddíly, které jsou používané tooevaluate hello entity agregovat stavu, bez ohledu na to hello filtru.
* [Nepovinné] Filtrujte tooexclude stavu statistiky. Pokud není zadaný, hello stavu statistiky zobrazit hello ok, upozornění a chyb počet pro všechny oddíly a repliky služby hello.

### <a name="api"></a>Rozhraní API
Stav služby tooget prostřednictvím hello rozhraní API, vytvoření `FabricClient` a volání hello [GetServiceHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getservicehealthasync) metoda na jeho HealthManager.

Hello následující příklad načte hello stav služby pomocí zadaného názvu služby (URI):

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

Hello následující kód získá hello stavu služby pro hello zadaný název služby (URI), filtrů a vlastní zásady prostřednictvím [ServiceHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.servicehealthquerydescription):

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Stav služby Hello rutiny tooget hello je [Get-ServiceFabricServiceHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicehealth). Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.

následující rutiny Hello získá stav služby hello pomocí výchozích zásad stavu:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                        
                        Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                        
                            Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
PartitionHealthStates : 
                        PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                        AggregatedHealthState : Warning
                        
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 15
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                        
HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
                        Partition             : 0 Ok, 1 Warning, 0 Error
```

### <a name="rest"></a>REST
Můžete získat stav služby s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-by-using-a-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.

## <a name="get-partition-health"></a>Získání stavu oddílu
Vrátí stav hello oddílu entity. Obsahuje hello repliky stavů. Vstup:

* Oddíl [požadované] hello ID (GUID), který identifikuje hello oddílu.
* [Nepovinné] hello zásady stavu aplikace použít toooverride hello aplikace manifestu zásad.
* [Nepovinné] Filtry pro události a repliky, které určují, položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby). Všechny události a repliky jsou stav entity agregovat hello použité tooevaluate, bez ohledu na to hello filtru.
* [Nepovinné] Filtrujte tooexclude stavu statistiky. Pokud není zadaný, hello stavu statistiky zobrazit, kolik repliky byly ve ok, upozornění a chybové stavy.

### <a name="api"></a>Rozhraní API
Vytvoření stavu oddílu tooget prostřednictvím hello rozhraní API, `FabricClient` a volání hello [GetPartitionHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getpartitionhealthasync) metoda na jeho HealthManager. toospecify volitelné parametry, vytvořte [PartitionHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.partitionhealthquerydescription).

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShell
Hello rutiny tooget hello oddílu stav je [Get-ServiceFabricPartitionHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricpartitionhealth). Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.

Hello následující rutiny získá hello stavu pro všechny oddíly hello **fabric: / WordCount/WordCountService** služby a filtry se stavy repliky:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 72
                        SentAt                : 7/13/2017 5:57:29 PM
                        ReceivedAt            : 7/13/2017 5:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/P RD _Node_2 Up 131444422260002646
                          N/S RD _Node_4 Up 131444422293113678
                          N/S RD _Node_3 Up 131444422293113679
                          N/S RD _Node_1 Up 131444422293118720
                          N/S RD _Node_0 Up 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5.)
                        
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 7/13/2017 5:57:48 PM, LastError = 1/1/0001 12:00:00 AM
                        
                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131444445174851664
                        SentAt                : 7/13/2017 6:35:17 PM
                        ReceivedAt            : 7/13/2017 6:35:18 PM
                        TTL                   : 00:01:05
                        Description           : hello Load Balancer was unable toofind a placement for one or more of hello Service's Replicas:
                        Secondary replica could not be placed due toohello following constraints and properties:  
                        TargetReplicaSetSize: 7
                        Placement Constraint: N/A
                        Parent Service: N/A
                        
                        Constraint Elimination Sequence:
                        Existing Secondary Replicas eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        Existing Primary Replica eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.
                        
                        Nodes Eliminated By Constraints:
                        
                        Existing Secondary Replicas -- Nodes with Partition's Existing Secondary Replicas/Instances:
                        --
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None
                        
                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None
                        
                        
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/13/2017 5:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
                        
HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a>REST
Můžete získat stav oddílu s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition-by-using-a-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.

## <a name="get-replica-health"></a>Získání stavu repliky
Vrátí stav hello repliku stavové služby nebo instance bezstavové služby. Vstup:

* [Požadované] hello ID (GUID) a repliky ID oddílu identifikující hello repliky.
* Parametry zásady stavu aplikace [Nepovinné] hello používá manifestu zásady aplikací toooverride hello.
* [Nepovinné] Filtry pro události, které určují, položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby). Všechny události jsou stav entity agregovat hello použité tooevaluate, bez ohledu na to hello filtru.

### <a name="api"></a>Rozhraní API
Vytvoření stavu repliky hello tooget prostřednictvím hello rozhraní API, `FabricClient` a volání hello [GetReplicaHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getreplicahealthasync) metoda na jeho HealthManager. toospecify advanced parametrů, použijte [ReplicaHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.replicahealthquerydescription).

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>PowerShell
Hello rutiny tooget hello repliky stav je [Get-ServiceFabricReplicaHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricreplicahealth). Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.

Hello následující rutiny získá stav hello hello primární repliky pro všechny oddíly hello služby:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Můžete získat stav repliky se [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica-by-using-a-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.

## <a name="get-deployed-application-health"></a>Získat stav nasazení aplikace
Vrátí stav hello aplikace nasazené na uzlu entity. Obsahuje stav balíčku hello nasazené služby. Vstup:

* Název aplikace [požadované] hello (URI) a název uzlu (string), které identifikují hello nasazené aplikace.
* [Nepovinné] hello zásady stavu aplikace použít manifestu zásady aplikací toooverride hello.
* [Nepovinné] Filtry pro události a nasazené služby balíčky, které určují, položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby). Všechny události a nasazené balíčky služeb se stav entity agregovat hello použité tooevaluate, bez ohledu na to hello filtru.
* [Nepovinné] Filtrujte tooexclude stavu statistiky. Pokud není zadaný, zobrazit hello stavu statistiky hello počet nasazené balíčky služeb v ok, upozornění a chybové stavy.

### <a name="api"></a>Rozhraní API
Vytvoření tooget hello stavu aplikace nasazené na uzlu prostřednictvím hello rozhraní API, `FabricClient` a volání hello [GetDeployedApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync) metoda na jeho HealthManager. Volitelné parametry toospecify, použijte [DeployedApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedapplicationhealthquerydescription).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShell
Hello rutiny tooget hello nasazené aplikace stav je [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps). Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny. Spustit toofind, na kterém je aplikace nasazena, [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) a podívejte se na hello nasazené aplikace podřízené objekty.

Hello následující rutiny získá stav hello hello **fabric: / WordCount** aplikace nasazené na **to uzel _Node_2**.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_0


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_0
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok
                                     
                                     ServiceManifestName   : WordCountWebServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok
                                     
HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131444422261848308
                                     SentAt                : 7/13/2017 5:57:06 PM
                                     ReceivedAt            : 7/13/2017 5:57:17 PM
                                     TTL                   : Infinite
                                     Description           : hello application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/13/2017 5:57:17 PM, LastWarning = 1/1/0001 12:00:00 AM
                                     
HealthStatistics                   : 
                                     DeployedServicePackage : 2 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a>REST
Můžete získat stav nasazení aplikace s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application-by-using-a-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.

## <a name="get-deployed-service-package-health"></a>Získat stav balíčku nasazené služby
Vrátí hello stavu entity balíček nasazené služby. Vstup:

* Název aplikace [požadované] hello (URI), název uzlu (string) a manifestu název služby (string), které identifikují hello nasadit balíček služby.
* [Nepovinné] hello zásady stavu aplikace použít toooverride hello aplikace manifestu zásad.
* [Nepovinné] Filtry pro události, které určují, položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby). Všechny události jsou stav entity agregovat hello použité tooevaluate, bez ohledu na to hello filtru.

### <a name="api"></a>Rozhraní API
Vytvoření tooget hello stavu balíčku nasazené služby prostřednictvím hello rozhraní API, `FabricClient` a volání hello [GetDeployedServicePackageHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync) metoda na jeho HealthManager. Volitelné parametry toospecify, použijte [DeployedServicePackageHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedservicepackagehealthquerydescription).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShell
Hello rutiny tooget hello nasazené služby balíček stav je [Get-ServiceFabricDeployedServicePackageHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicepackagehealth). Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny. Spustit toosee, kde je aplikace nasazena, [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) a prohlédněte si hello nasazené aplikace. toosee, které balíčky služeb jsou v aplikaci, vyhledejte v hello nasazené služby balíček dětech do hello [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps) výstup.

Hello následující rutiny získá stav hello hello **WordCountServicePkg** balíček služby hello **fabric: / WordCount** aplikace nasazené na **to uzel _Node_2**. Hello entita má **System.Hosting** sestavy pro úspěšné aktivaci balíček služby a vstupní bod a úspěšnou registraci typ služby.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_2
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131444422267693359
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131444422267903345
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131444422272458374
                             SentAt                : 7/13/2017 5:57:07 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Můžete získat stav balíčku nasazené služby s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package-by-using-a-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.

## <a name="health-chunk-queries"></a>Dotazy na stav bloku
Hello stavu bloku dotazy mohou vracet podřízené víceúrovňovou clusteru (rekurzivně), za vstupní filtry. Podporuje rozšířené filtry, které umožňují značnou flexibilitu při volbě hello, děti toobe vrátila. hello jedinečný identifikátor nebo jiné skupiny nebo stavy, můžete zadat filtry Hello podřízené objekty. Ve výchozím nastavení žádné podřízené objekty jsou zahrnuty jako názvem na rozdíl od toohealth příkazy, které vždy zahrnovat první úrovně podřízené objekty.

Hello [dotazů na stav](service-fabric-view-entities-aggregated-health.md#health-queries) návratový pouze první úroveň podřízených prvků hello zadané entity na požadované filtry. podřízené objekty hello tooget hello podřízených prvků, musí volat další stavu rozhraní API pro každé entity, které vás zajímají. Podobně tooget hello stav konkrétní entit, je třeba zavolat jednoho stavu rozhraní API pro každou požadovanou entitu. Hello bloku dotazu rozšířené filtrování vám umožní toorequest více položek, které vás zajímají v jednom dotazu, minimalizovat velikost zprávy hello a hello počet zpráv.

Hodnota Hello hello bloku dotazu je, že můžete získat stav pro další clusteru entity (potenciálně všechny clusteru entity začínající na požadovaný kořenový) v jednom volání. Komplexní stavu dotazu lze vyjádřit jako:

* Návratový pouze aplikace, které jsou v chybě a pro tyto aplikace zahrnout všechny služby upozornění nebo chyby. Vrácených služeb zahrnují všechny oddíly.
* Vrátí pouze hello stav čtyři aplikací, určeného jejich názvy.
* Vrátí pouze hello stav aplikací typu požadované aplikace.
* Vrátí všechny nasazené entity na uzlu. Vrátí všechny aplikace, všechny nasazené aplikace v hello zadaný uzel a všechny hello nasazené balíčky služeb v tomto uzlu.
* Vrátí všechny repliky v chybě. Vrátí všechny aplikace, služby, oddíly a pouze repliky v chybě.
* Vrátí všechny aplikace. Zadaná služba zahrnují všechny oddíly.

V současné době dotazu bloku hello stavu je vystaven pouze pro entitu hello clusteru. Vrátí bloku stavu clusteru, který obsahuje:

* Hello clusteru agregován stavu.
* Hello stavu stavu bloku seznam uzlů, které respektují vstupní filtry.
* Hello stavu stavu bloku seznam aplikací, které respektují vstupní filtry. Každého bloku stavu stavu aplikace obsahuje seznam bloků dat u všech služeb, které respektují vstupní filtry a seznam bloků dat s všechny nasazené aplikace, které respektují hello filtry. Stejný pro děti hello služeb a nasazené aplikace. Tímto způsobem všechny entity v clusteru hello se může potenciálně vracet Pokud vyžaduje, je hierarchický.

### <a name="cluster-health-chunk-query"></a>Dotaz bloku stavu clusteru
Vrátí hello stavu entity hello clusteru a obsahuje hello hierarchické stavu stavu bloky požadované podřízených prvků. Vstup:

* [Nepovinné] hello zásady stavu clusteru používá tooevaluate hello uzly a události hello clusteru.
* [Nepovinné] hello mapy zásady stavu aplikace, pomocí zásad stavu hello používá manifestu zásady aplikací toooverride hello.
* [Nepovinné] Filtry pro uzly a aplikace, které určí položky, které jsou v zájmu a má být vrácen ve výsledku hello. Hello filtry jsou konkrétní tooan entity nebo skupiny entit nebo použít tooall entity na této úrovni. Hello seznam filtrů může obsahovat jeden obecné filtr a filtry pro konkrétní identifikátory toofine intervalem entity vrácené dotazem hello. Pokud je prázdný, děti hello nebudou zobrazeny ve výchozím nastavení.
  Další informace o hello filtrů při [NodeHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.nodehealthstatefilter) a [ApplicationHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthstatefilter). rekurzivní může filtry aplikace Hello zadejte rozšířené filtry pro děti.

výsledek bloku Hello zahrnuje hello podřízené objekty, které respektují hello filtry.

V současné době hello bloku dotaz nevrátí, není v pořádku hodnocení nebo události entity. Tyto doplňující informace lze získat pomocí hello existující cluster stavu dotaz.

### <a name="api"></a>Rozhraní API
stav clusteru tooget bloku dat, vytvořte `FabricClient` a volání hello [GetClusterHealthChunkAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync) metoda na jeho **HealthManager**. Abyste mohli předávat [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthchunkquerydescription) toodescribe stavu zásad a rozšířené filtry.

Hello následující kód získá bloku stavu clusteru se rozšířené filtry.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except hello ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Hello rutiny tooget hello clusteru stav je [Get-ServiceFabricClusterChunkHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealthchunk). Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.

Hello následující kód získá uzly pouze v případě, že se chyba s výjimkou konkrétním uzlu, který má být vždy vrácen.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in hello cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters


HealthState                  : Warning
NodeHealthStateChunks        : 
                               TotalCount            : 1
                               
                               NodeName              : _Node_1
                               HealthState           : Ok
                               
ApplicationHealthStateChunks : None
```

následující rutiny Hello získá clusteru bloku s filtry aplikace.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 1
                               
                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks : 
                                TotalCount            : 1
                               
                                ServiceName           : fabric:/WordCount/WordCountService
                                HealthState           : Error
                                PartitionHealthStateChunks : 
                                    TotalCount            : 1
                               
                                    PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                                    HealthState           : Error
                                    ReplicaHealthStateChunks : 
                                        TotalCount            : 5
                               
                                        ReplicaOrInstanceId   : 131444422293118720
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293118721
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293113678
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293113679
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422260002646
                                        HealthState           : Error
```

Hello následující rutina vrací všechny nasazené entity na uzlu.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 2
                               
                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1
                               
                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1
                               
                                    ServiceManifestName   : FAS
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
                               
                               
                               
                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1
                               
                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1
                               
                                    ServiceManifestName   : WordCountServicePkg
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
```

### <a name="rest"></a>REST
Můžete získat blok stavu clusteru s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-using-health-chunks) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/health-of-cluster) zahrnující zásady stavu a rozšířené filtry, které jsou popsané v textu hello.

## <a name="general-queries"></a>Obecné dotazy
Obecné dotazy vrátí seznam entity prostředků infrastruktury služby zadaného typu. Se zveřejňují přes hello rozhraní API (prostřednictvím metody hello na **FabricClient.QueryManager**), rutiny prostředí PowerShell a REST. Tyto dotazy agregovat poddotazy z několika součástí. Jeden z nich je hello [úložiště stavu](service-fabric-health-introduction.md#health-store), který naplní hello Agregovat stav pro každý výsledek dotazu.  

> [!NOTE]
> Obecné dotazy vrátit stav hello agregovat hello entity a neobsahuje data bohaté stavu. Pokud entity není v pořádku, můžete sledovat pomocí tooget dotazy stavu všechny jeho stavu informace, včetně událostí, podřízené stavů a není v pořádku hodnocení.
>
>

Pokud obecné dotazy vrátí Neznámý stav pro entitu, je možné že toto úložiště stavu hello nemá dokončení data o hello entity. Je také možné, že úložiště stavu toohello poddotazu nebyla úspěšná (například došlo k chybě komunikace, nebo byla omezena hello stavu úložiště). Pomocí dotazu stavu pro entitu hello následnou akci. Pokud hello poddotazu došlo k přechodné chyby, například problémy se sítí, může být úspěšné následné dotaz. Ho může také získáte další informace z health store hello o proč nebude vystavena hello entity.

Hello dotazů, které obsahují **HealthState** pro entity jsou:

* Seznam uzlů: vrátí hello seznamu uzlů v clusteru hello (stránkovaného).
  * Rozhraní API: [FabricClient.QueryClient.GetNodeListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getnodelistasync)
  * Prostředí PowerShell: Get-ServiceFabricNode
* Seznam aplikací: vrátí hello seznam aplikací v clusteru hello (stránkovaného).
  * Rozhraní API: [FabricClient.QueryClient.GetApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync)
  * Prostředí PowerShell: Get-ServiceFabricApplication
* Seznam služeb: vrátí hello seznam služeb v aplikaci (stránkovaného).
  * Rozhraní API: [FabricClient.QueryClient.GetServiceListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync)
  * Prostředí PowerShell: Get-ServiceFabricService
* Seznam oddílů: vrátí hello seznam oddílů ve službě (stránkovaného).
  * Rozhraní API: [FabricClient.QueryClient.GetPartitionListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getpartitionlistasync)
  * Prostředí PowerShell: Get-ServiceFabricPartition
* Seznam replik: vrátí hello seznam replik v oddílu (stránkovaného).
  * Rozhraní API: [FabricClient.QueryClient.GetReplicaListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getreplicalistasync)
  * Prostředí PowerShell: Get-ServiceFabricReplica
* Nasazení seznam aplikací: vrátí hello seznam nasazené aplikace na uzlu.
  * Rozhraní API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync)
  * Prostředí PowerShell: Get-ServiceFabricDeployedApplication
* Nasazení služby seznam balíčků: hello vrátí seznam balíčků služeb v nasazení aplikace.
  * Rozhraní API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync)
  * Prostředí PowerShell: Get-ServiceFabricDeployedApplication

> [!NOTE]
> Některé dotazy hello vrátí stránkových výsledků. Hello vrátit těchto dotazů je seznam odvozené od [PagedList<T>](https://docs.microsoft.com/dotnet/api/system.fabric.query.pagedlist-1). Pokud výsledky hello nebudou vyhovovat zprávu, je vrácen pouze na stránce a ContinuationToken, který sleduje kde výčtu zastavena. Pokračujte toocall hello stejný dotaz a předejte token pokračování hello z hello předchozí tooget další výsledky dotazu.
>
>

### <a name="examples"></a>Příklady
Hello následující kód získá hello není v pořádku aplikace v clusteru hello:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

Hello následující rutiny získá hello aplikace podrobnosti hello fabric: / WordCount aplikace. Všimněte si, že stav je v upozornění.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

Hello následující rutiny získá hello služby s stav chyby:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Error"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Error
```

## <a name="cluster-and-application-upgrades"></a>Upgrady cluster a aplikace
Během monitorovaných upgrade hello cluster a aplikace Service Fabric kontroluje tooensure stavu, že všechno zůstane v pořádku. Pokud není v pořádku, jak se vyhodnocují se pomocí zásady nakonfigurované stavu entity se hello upgrade platí zásady specifické pro upgrade toodetermine hello další akce. Hello upgradu může být pozastavený tooallow zásah uživatele (například opravě chybové stavy nebo změna zásad) nebo ji může automaticky vrácení předchozí verze dobrý toohello.

Během *clusteru* upgradu, můžete získat stav upgradu hello clusteru. Stav upgradu Hello zahrnuje není v pořádku hodnocení, které toowhat bod je v clusteru hello v chybném stavu. Pokud se hello upgrade je vrácena zpět z důvodu toohealth problémy, stav upgradu hello pamatuje hello poslední není v pořádku důvodů. Tyto informace mohou pomoci správci prozkoumat, kde došlo k chybě po upgradu hello vrácena nebo byla zastavena.

Podobně při *aplikace* upgrade všech není v pořádku hodnocení jsou obsaženy v stav upgradu aplikace hello.

Hello následující ukazuje stav upgradu aplikace hello pro upravené fabric: / WordCount aplikace. Sledovací zařízení ohlásilo chybu na jednom z jejích replik. Hello upgrade je vrácení zpět, protože nejsou dodržovány kontroly stavu hello.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2017 5:23:26 PM
FailureTimestampUtc           : 4/21/2017 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

Další informace o hello [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-tootroubleshoot"></a>Použít tootroubleshoot hodnocení stavu
Vždy, když nastane problém s hello clusteru nebo aplikaci, podívejte se na toopinpoint stavu clusteru nebo aplikace hello co je nesprávný. není v pořádku hodnocení Hello obsahují podrobné informace o jaké spouštěná hello stav aktuální není v pořádku. Pokud potřebujete, můžete k podrobnostem na není v pořádku podřízených entit tooidentify hello hlavní příčinu.

Představte si třeba aplikace není v pořádku, protože není zprávu o chybách na jednom z jejích replik. Hello následující rutiny prostředí Powershell ukazuje, není v pořádku hodnocení hello:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount -EventsFilter None -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                                  
                                        Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.
                                  
                                        Unhealthy replica: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', ReplicaOrInstanceId='131444422260002646', AggregatedHealthState='Error'.
                                  
                                            Error event: SourceId='MyWatchdog', Property='Memory'.
                                  
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

Můžete se podívat na hello repliky tooget Další informace:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricReplicaHealth -ReplicaOrInstanceId 131444422260002646 -PartitionId af2e3e44-a8f8-45ac-9f31-4093eb897600


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Error
UnhealthyEvaluations  : 
                        Error event: SourceId='MyWatchdog', Property='Memory'.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                        
                        SourceId              : MyWatchdog
                        Property              : Memory
                        HealthState           : Error
                        SequenceNumber        : 131444451657749403
                        SentAt                : 7/13/2017 6:46:05 PM
                        ReceivedAt            : 7/13/2017 6:46:05 PM
                        TTL                   : Infinite
                        Description           : 
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 7/13/2017 6:46:05 PM, LastOk = 1/1/0001 12:00:00 AM
```

> [!NOTE]
> Hello není v pořádku hodnocení zobrazit hello prvním důvodem je hello entity vyhodnotit toocurrent stavu. Může být více událostí, které spouštějí tento stav, ale neprojeví v hello hodnocení. tooget Další informace, rozbalení do hello stavu entity toofigure na všechny sestavy není v pořádku hello v clusteru hello.
>
>

## <a name="next-steps"></a>Další kroky
[Použít tootroubleshoot sestavy stavu systému](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Přidat vlastní sestavy stavu Service Fabric](service-fabric-report-health.md)

[Jak tooreport a zkontrolujte stav služby](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Monitorování a Diagnostika služby místně](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade aplikace Service Fabric](service-fabric-application-upgrade.md)
