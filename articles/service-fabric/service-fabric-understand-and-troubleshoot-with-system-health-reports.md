---
title: "Poradce při potížích s sestav o stavu systému | Microsoft Docs"
description: "Popisuje sestav stavu odesílají součásti Azure Service Fabric a jejich využití pro řešení potíží clusteru nebo problémy s aplikací."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 52574ea7-eb37-47e0-a20a-101539177625
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: oanapl
ms.openlocfilehash: 54e20146b2f1e0ca6153b66319be70c6f7c2fb59
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-system-health-reports-to-troubleshoot"></a>Řešení problémů pomocí sestav o stavu systému
Azure Service Fabric součásti sestavy mimo pole na všech entit v clusteru. [Úložiště stavu](service-fabric-health-introduction.md#health-store) vytvoří nebo odstraní entit na základě sestav systému. Také slouží k uspořádání je v hierarchii, která zaznamená interakce entity.

> [!NOTE]
> Abyste pochopili související se stavem koncepty, další informace v [modelu stavu Service Fabric](service-fabric-health-introduction.md).
> 
> 

Sestav o stavu systému poskytují přehled o clusteru a příznak problémy prostřednictvím stavu a funkce aplikací. Pro aplikace a služby ověřte sestav o stavu systému entity jsou implementované a chovají správně z hlediska Service Fabric. Sestavy neposkytují žádné sledování stavu obchodní logiky služby nebo zjišťování "zamrzlých" procesů. Uživatel služby lze rozšířit údaje o stavu informace specifické pro jejich logiku.

> [!NOTE]
> Sestavy stavu watchdogs jsou viditelné pouze *po* součástech systému vytvořte entitu. Při odstranění entity úložiště zdravotní automaticky odstraní všechny sestavy stavu s ním spojená. Stejné hodnotu true, pokud je vytvořena nová instance entity (například je vytvořena nová instance repliky stavové trvalou služby). Všechny sestavy přidružené k původní instanci jsou odstraněna a vyčistit z úložiště.
> 
> 

Součást systému sestavy, jsou identifikovány zdroj, který začíná "**systému.**" Předpona. Watchdogs nelze používat stejnou předponu pro jejich zdroje, jako jsou odmítnuta sestavy se neplatné parametry.
Podívejme se na některé sestavy systému pochopit, co je aktivuje a jak chybu opravit možné problémy, které reprezentují.

> [!NOTE]
> Service Fabric i nadále přidat sestavy týkající se podmínek, které vylepšují získat přehled o dění v clusteru a aplikace. Existující sestavy lze také rozšířit o další podrobnosti k řešení problému rychlejší.
> 
> 

## <a name="cluster-system-health-reports"></a>Cluster sestav o stavu systému
Stav entity clusteru se vytvoří automaticky v health store. Pokud všechno funguje správně, nemá sestavu system.

### <a name="neighborhood-loss"></a>Ztráta Okolní počítače
**System.Federation** nahlásí chybu, když zjistí ztrátu okolí. Sestava je z jednotlivých uzlů a ID uzlu je součástí názvu vlastnosti. Pokud je jeden okolí ztratili v celé prstenec Service Fabric, můžete očekávat obvykle dvě události (obou stranách sestavy mezera). Pokud další sousedství jsou ztraceny, existují další události.

Sestava Určuje časový limit zapůjčení globální jako TTL. Sestava je nutno každých poloviny doby trvání TTL pro podmínku zůstává aktivní. Událost je automaticky odstraněna po jeho vypršení. Odeberte, pokud vypršela platnost zaručuje, že sestava je vyčištěna z health store správně, přestože uzlu vytváření sestav je vypnutý.

* **SourceId**: System.Federation
* **Vlastnost**: začíná **okolí** a obsahuje informace o uzlu
* **Další kroky**: Zjistěte, proč dojde ke ztrátě okolí (například zkontrolovat komunikaci mezi uzly clusteru).

## <a name="node-system-health-reports"></a>Uzel sestav o stavu systému
**System.FM**, který představuje službu Failover Manager je autority, který spravuje informace o uzly clusteru. Každý uzel musí mít jednu sestavu z System.FM zobrazuje stav. Uzel entity, které se odeberou, když se odebere stav uzlu (viz [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).

### <a name="node-updown"></a>Uzel nahoru/dolů
System.FM nahlásí jako OK, když se uzel připojí prstenec (je spuštěná). Nahlásí chybu, pokud uzel vyplouvající řetězci (je vypnutý, buď pro upgrade nebo jednoduše protože se nezdařilo). Stav hierarchie sestavena úložiště zdravotní provádět akce se nasazené entity v korelace s System.FM sestavy uzlu. Nadřazený virtuální všechny nasazené entit považuje uzlu. Nasazené entit na tomto uzlu se zveřejňují přes dotazy, pokud uzel je hlášen jako až podle System.FM s stejnou instanci jako instanci spojenou s entity. Když System.FM ohlásí, že uzel je mimo provoz nebo restartovat (nové instance), úložiště zdravotní automaticky vyčistí nasazené entitami, které může existovat jenom v uzlu dolů nebo na předchozí instanci uzlu.

* **SourceId**: System.FM
* **Vlastnost**: stavu
* **Další kroky**: Pokud je uzel dolů k upgradu by měl mít zpět po byl upgradován. V takovém případě by měl stav přepněte zpět na OK. Pokud uzel nemá vraťte nebo se nezdaří, problém potřebuje další šetření.

Následující příklad ukazuje System.FM události se stavem stavu OK pro uzel:

```powershell
PS C:\> Get-ServiceFabricNodeHealth  _Node_0

NodeName              : _Node_0
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 8
                        SentAt                : 7/14/2017 4:54:51 PM
                        ReceivedAt            : 7/14/2017 4:55:14 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```


### <a name="certificate-expiration"></a>Vypršení platnosti certifikátu
**System.FabricNode** sestavy upozornění, když se certifikáty používané uzlu blíží vypršení platnosti. Existují tři certifikáty na uzel: **Certificate_cluster**, **Certificate_server**, a **Certificate_default_client**. Při vypršení je alespoň dva týdny, je sestava stavu v pořádku. Pokud doba vypršení platnosti je během dvou týdnů, typ sestavy je upozornění. Hodnota TTL z těchto událostí je nekonečno, a budou odstraněny Jestliže uzel opustí clusteru.

* **SourceId**: System.FabricNode
* **Vlastnost**: začíná **certifikát** a obsahuje další informace o typ certifikátu
* **Další kroky**: aktualizovat certifikáty, pokud se blíží vypršení platnosti.

### <a name="load-capacity-violation"></a>Načíst porušení kapacity
Vyrovnávání zatížení Service Fabric hlásí upozornění, když zjistí porušení kapacity uzlu.

* **SourceId**: System.PLB
* **Vlastnost**: začíná **kapacity**
* **Další kroky**: Zkontrolujte zadaný metriky a zobrazit aktuální kapacitu na uzlu.

## <a name="application-system-health-reports"></a>Aplikace sestav o stavu systému
**System.CM**, který představuje službu Správce clusteru, je že úřad, který spravuje informace o aplikaci.

### <a name="state"></a>Stav
System.CM nahlásí jako OK když aplikace byly vytvořeny nebo aktualizovány. Informuje úložiště zdravotní po odstranění aplikace tak, aby bylo možné odebrat z úložiště.

* **SourceId**: System.CM
* **Vlastnost**: stavu
* **Další kroky**: Pokud aplikace má byly vytvořeny nebo aktualizovány, měl by obsahovat sestava stavu Správce clusteru. Jinak, zkontrolujte stav aplikace vydáním dotazu (například rutinu prostředí PowerShell **Get ServiceFabricApplication - ApplicationName *applicationName***).

Následující příklad ukazuje události stavu na **fabric: / WordCount** aplikace:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/14/2017 4:55:10 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="service-system-health-reports"></a>Služba sestav o stavu systému
**System.FM**, který představuje službu Failover Manager je autority, který spravuje informace o službách.

### <a name="state"></a>Stav
System.FM sestavy jako OK po vytvoření služby. Odstraní entitu z health store, pokud služba je Odstraněná.

* **SourceId**: System.FM
* **Vlastnost**: stavu

Následující příklad ukazuje události stavu služby **fabric: / WordCount/WordCountWebService**:

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountWebService -ExcludeHealthStatistics


ServiceName           : fabric:/WordCount/WordCountWebService
AggregatedHealthState : Ok
PartitionHealthStates : 
                        PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
                        AggregatedHealthState : Ok
                        
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 14
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="service-correlation-error"></a>Chyba korelace služby
**System.PLB** nahlásí chybu, když zjistí, zda aktualizace služby ke korelaci se jiné služby vytvoří řetězec vztahů. Sestava je vymazán poté, co se stane úspěšná aktualizace.

* **SourceId**: System.PLB
* **Vlastnost**: ServiceDescription
* **Další kroky**: Zkontrolujte popis korelační služby.

## <a name="partition-system-health-reports"></a>Oddíl sestav o stavu systému
**System.FM**, který představuje službu Failover Manager je autority, který spravuje informace o oddílech služby.

### <a name="state"></a>Stav
System.FM nahlásí jako OK když oddíl existuje a je v pořádku. Odstraní entitu z health store při odstranění oddílu.

Pokud oddílu je menší než počet minimální repliky, nahlásí chybu. Pokud oddíl není nižší než počet minimální repliky, ale je nižší než počtu cílových replik, sestavy upozornění. Pokud oddíl je ve ztrátě kvora, System.FM nahlásí chybu.

Další důležité události zahrnovat upozornění změnu konfigurace trvá déle, než se očekávalo, a při sestavení trvá déle, než se očekávalo. Očekávané časy pro sestavení a změny konfigurace se dají konfigurovat na základě služby scénářů. Například pokud má služba terabajt stavu, například SQL Database, sestavení trvá déle než pro službu s malou stavu.

* **SourceId**: System.FM
* **Vlastnost**: stavu
* **Další kroky**: Pokud stav není v pořádku, je možné, že nebyly některé repliky vytvoření, otevření nebo povýšen na primární nebo sekundární správně. V mnoha případech je hlavní příčinou chyby služby k implementaci otevřené nebo změny role.

Následující příklad ukazuje oddíl v pořádku:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountWebService | Get-ServiceFabricPartitionHealth -ExcludeHealthStatistics -ReplicasFilter None

PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 70
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Následující příklad ukazuje stav oddíl, který je pod počtu cílových replik. Dalším krokem je popis oddílu, který ukazuje, jak jsou nakonfigurované získání: **MinReplicaSetSize** je tři a **TargetReplicaSetSize** je 7. Potom získat počet uzlů v clusteru: pět. Proto v tomto případě dvě repliky nelze umístit, protože cílový počet replik je vyšší než počet uzlů, které jsou k dispozici.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None -ExcludeHealthStatistics


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 123
                        SentAt                : 7/14/2017 4:55:39 PM
                        ReceivedAt            : 7/14/2017 4:55:44 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/S RD _Node_2 Up 131444422260002646
                          N/S RD _Node_4 Up 131444422293113678
                          N/S RD _Node_3 Up 131444422293113679
                          N/S RD _Node_1 Up 131444422293118720
                          N/P RD _Node_0 Up 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5.)
                        
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:55:44 PM, LastOk = 1/1/0001 12:00:00 AM
                        
                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131445250939703027
                        SentAt                : 7/14/2017 4:58:13 PM
                        ReceivedAt            : 7/14/2017 4:58:14 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        Secondary replica could not be placed due to the following constraints and properties:  
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
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        
                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None
                        
                        
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:56:14 PM, LastOk = 1/1/0001 12:00:00 AM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | select MinReplicaSetSize,TargetReplicaSetSize

MinReplicaSetSize TargetReplicaSetSize
----------------- --------------------
                2                    7                        

PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a>Porušení omezení repliky
**System.PLB** sestavy upozornění, pokud zjistí porušení omezení repliky a nelze umístit všechny repliky oddílu. Zobrazí podrobnosti sestavy, které omezení a vlastnosti zabránit umístění repliky.

* **SourceId**: System.PLB
* **Vlastnost**: začíná **ReplicaConstraintViolation**

## <a name="replica-system-health-reports"></a>Repliky sestav o stavu systému
**System.RA**, která představuje součást reconfiguration agent, je autorita pro stav repliky.

### <a name="state"></a>Stav
**System.RA** sestavy OK po vytvoření repliky.

* **SourceId**: System.RA
* **Vlastnost**: stavu

Následující příklad ukazuje repliku v pořádku:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422293118721
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131445248920273536
                        SentAt                : 7/14/2017 4:54:52 PM
                        ReceivedAt            : 7/14/2017 4:55:13 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_0
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:13 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="replica-open-status"></a>Otevřete stav repliky
Popis této sestavy health obsahuje čas spuštění (Coordinated Universal Time) při volání rozhraní API byla volána.

**System.RA** sestavy upozornění, pokud repliku open trvá déle, než nastaveném časovém intervalu (výchozí: 30 minut). Pokud rozhraní API má dopad na dostupnost služby, je vydána sestavu mnohem rychleji (Konfigurovat intervalu, výchozí hodnota je 30 sekund). Měření času zahrnuje čas potřebný pro open Replikátor a otevřete službu. Vlastnost se změní na OK open dokončení.

* **SourceId**: System.RA
* **Vlastnost**: **ReplicaOpenStatus**
* **Další kroky**: Pokud stav není v pořádku, zjistěte, proč repliku open trvá déle, než se očekávalo.

### <a name="slow-service-api-call"></a>Pomalá volání rozhraní API služby
**System.RAP** a **System.Replicator** sestavy upozornění, pokud volání do kódu uživatele služby trvá déle, než je doba, nakonfigurované. Upozornění je vymazán po dokončení volání.

* **SourceId**: System.RAP nebo System.Replicator
* **Vlastnost**: název pomalé rozhraní API. Popis poskytuje další podrobnosti o době, kdy byl čeká na rozhraní API.
* **Další kroky**: Zjistěte, proč trvá déle, než bylo očekáváno volání.

Následující příklad ukazuje oddíl ve ztrátě kvora a provádí zjistěte, proč kroky šetření. Jedna z replik má stav varování, proto jeho stav. Zobrazuje, že operace služby trvá déle, než se očekávalo, hlášené System.RAP událost. Po přijetí těchto informací je dalším krokem je kódu služby a prozkoumat existuje. Pro tento případ **RunAsync** implementace stavové služby, vyvolá k neošetřené výjimce. Repliky jsou recyklace, takže se nemusí zobrazovat všechny repliky ve stavu upozornění. Můžete opakovat získávání stavu a vyhledejte případné rozdíly v ID repliky. V některých případech opakované pokusy vám může poskytnout různá vodítka.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth -ExcludeHealthStatistics

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 3
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

Při spuštění aplikace vadný v ladicím programu zobrazit windows diagnostických událostí výjimky z RunAsync:

![Visual Studio 2015 diagnostických událostí: selhání RunAsync v fabric: / HelloWorldStatefulApplication.][1]

Visual Studio 2015 diagnostických událostí: selhání RunAsync v **fabric: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Replikační fronta je plná
**System.Replicator** nahlásí upozornění, když se fronta replikací je plná. Na primárním fronty replikací obvykle plný protože jeden nebo více sekundárních replikách jsou pomalé potvrdit operace. Na sekundárním to obvykle se stane, když služba pomalé použít operace. Upozornění je vymazán poté, co už fronta je plná.

* **SourceId**: System.Replicator
* **Vlastnost**: **PrimaryReplicationQueueStatus** nebo **SecondaryReplicationQueueStatus**, v závislosti na roli repliky

### <a name="slow-naming-operations"></a>Pomalé operations pojmenování
**System.NamingService** při operaci pojmenování trvá déle než přijatelné hlásí stav na jeho primární repliky. Příklady operací pojmenování [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) nebo [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync). Další metody naleznete v části FabricClient, například v [služby metody správy](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) nebo [metody správy vlastnost](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).

> [!NOTE]
> Službu Naming překládá názvy služby do umístění v clusteru a umožňuje uživatelům spravovat služby názvy a vlastnosti. Je, že služba jako trvalý, Service Fabric rozdělena na oddíly. Jeden z oddílů představuje Authority Owner, který obsahuje metadata o všechny názvy Service Fabric a služeb. Service Fabric názvy jsou namapované na různé oddíly, označované jako oddíly Name Owner, tak služba je rozšiřitelný. Další informace o [Naming service](service-fabric-architecture.md).
> 
> 

Při operaci pojmenování trvá déle, než se očekávalo, operace je příznak se sestavou upozornění na *primární repliky oddílu pojmenování služby, který slouží operaci*. Pokud po úspěšném dokončení operace je zrušeno upozornění. Dokončení operace s chybou, sestava stavu zahrnuje podrobnosti o této chybě.

* **SourceId**: System.NamingService
* **Vlastnost**: začíná předponu **Duration_** a identifikuje pomalé operaci a název Service Fabric, na kterém se používá operaci. Například pokud vytvořit službu na název fabric: / MyApp/Moje_služba trvá příliš dlouho, vlastnost je Duration_AOCreateService.fabric:/MyApp/MyService. AO odkazuje na roli pojmenování oddílu pro tento název a operaci.
* **Další kroky**: Kontrola proč pojmenování operace selže. Každé operace může mít různé kořenové příčiny. Například odstranit, může být služba zablokována na uzlu, protože udržuje na uzlu z důvodu chyby uživatele v kódu služby chybám hostitele aplikací.

Následující příklad ukazuje operaci vytvoření služby. Operace trvalo déle, než nakonfigurovaná doba trvání. AO opakování a odešle pracovní ne. JIŽ byla dokončena poslední operaci s vypršením časového limitu. V takovém případě je stejné repliky primární AO a žádné role.

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>DeployedApplication sestav o stavu systému
**System.Hosting** autoritou na nasazené entity.

### <a name="activation"></a>Aktivace
System.Hosting nahlásí jako OK když aplikace úspěšně aktivuje na uzlu. V opačném případě nahlásí chybu.

* **SourceId**: System.Hosting
* **Vlastnost**: aktivace, včetně zavedení verze
* **Další kroky**: Pokud aplikace není v pořádku, zjistěte, proč aktivace se nezdařila.

Následující příklad ukazuje úspěšné aktivaci:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ExcludeHealthStatistics

ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_1
                                     AggregatedHealthState : Ok
                                     
HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131445249083836329
                                     SentAt                : 7/14/2017 4:55:08 PM
                                     ReceivedAt            : 7/14/2017 4:55:14 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>Ke stažení
**System.Hosting** nahlásí chybu, pokud stahování balíčku aplikace selže.

* **SourceId**: System.Hosting
* **Vlastnost**:  **stáhnout:*RolloutVersion***
* **Další kroky**: Zjistěte, proč se stahování v tomto uzlu selhal.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage sestav o stavu systému
**System.Hosting** autoritou na nasazené entity.

### <a name="service-package-activation"></a>Služba aktivace balíčku
System.Hosting jako OK sestavy, pokud je aktivace balíček služby v uzlu úspěšná. V opačném případě nahlásí chybu.

* **SourceId**: System.Hosting
* **Vlastnost**: Aktivace
* **Další kroky**: Zjistěte, proč aktivace se nezdařila.

### <a name="code-package-activation"></a>Aktivace balíčku kódu.
**System.Hosting** sestavy jako OK pro každý balíček kódu, pokud aktivace nebude úspěšná. Pokud se aktivace nezdaří, sestavy upozornění podle konfigurace. Pokud **CodePackage** nepodaří aktivovat nebo ukončí s chybou větší než nakonfigurované **CodePackageHealthErrorThreshold**, nahlásí chybu, který je hostitelem. Pokud balíček služby obsahuje více balíčků kódu, zprávu o aktivaci se generuje pro každé z nich.

* **SourceId**: System.Hosting
* **Vlastnost**: používá předponu **CodePackageActivation** a obsahuje název balíček kódu a vstupního bodu jako  **CodePackageActivation:*CodePackageName*:*SetupEntryPoint/EntryPoint*** (například **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Typ registrace služby
**System.Hosting** sestavy jako OK, pokud typ služby byl úspěšně zaregistrován. Nahlásí chybu, pokud nebyla provedena registrace v čase (jak je nakonfigurovat pomocí **ServiceTypeRegistrationTimeout**). Pokud je zavřená modulu runtime, typ služby je odregistrovat z uzlu a hostitelský sestavy upozornění.

* **SourceId**: System.Hosting
* **Vlastnost**: používá předponu **ServiceTypeRegistration** a obsahuje název typu služby (například **ServiceTypeRegistration:FileStoreServiceType**)

Následující příklad ukazuje v pořádku nasazený balíček služby:

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_1
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131445249084026346
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131445249084306362
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131445249088096842
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>Ke stažení
**System.Hosting** nahlásí chybu, pokud služba stahování balíčku selže.

* **SourceId**: System.Hosting
* **Vlastnost**:  **stáhnout:*RolloutVersion***
* **Další kroky**: Zjistěte, proč se stahování v tomto uzlu selhal.

### <a name="upgrade-validation"></a>Ověření upgradu
**System.Hosting** nahlásí chybu, pokud selže ověření během upgradu nebo pokud upgrade selže na uzlu.

* **SourceId**: System.Hosting
* **Vlastnost**: používá předponu **FabricUpgradeValidation** a obsahuje upgradovaná verze
* **Popis**: odkazuje na došlo k chybě

## <a name="next-steps"></a>Další kroky
[Zobrazit sestavy stavu Service Fabric](service-fabric-view-entities-aggregated-health.md)

[Postup vytvoření sestavy a zkontrolujte stav služby](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Monitorování a Diagnostika služby místně](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade aplikace Service Fabric](service-fabric-application-upgrade.md)

