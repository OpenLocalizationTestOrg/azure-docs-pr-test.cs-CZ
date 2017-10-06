---
title: "aaaTroubleshoot s sestav o stavu systému | Microsoft Docs"
description: "Popisuje sestav stavu hello odesílají součásti Azure Service Fabric a jejich využití pro řešení potíží clusteru nebo problémy s aplikací."
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
ms.openlocfilehash: c77a6cdd0440ce5d354cd8760f40151f674a3529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-system-health-reports-tootroubleshoot"></a>Použít tootroubleshoot sestavy stavu systému
Azure Service Fabric součásti sestavy předinstalované hello na všechny entity v clusteru hello. Hello [úložiště stavu](service-fabric-health-introduction.md#health-store) vytvoří nebo odstraní entity založené na zprávách systému hello. Také slouží k uspořádání je v hierarchii, která zaznamená interakce entity.

> [!NOTE]
> Koncepty toounderstand související se stavem, další informace v [modelu stavu Service Fabric](service-fabric-health-introduction.md).
> 
> 

Sestav o stavu systému poskytují přehled o clusteru a příznak problémy prostřednictvím stavu a funkce aplikací. Pro aplikace a služby ověřte sestav o stavu systému entity jsou implementované a chovají správně z hello perspektivy Service Fabric. sestavy Hello neposkytují žádné sledování stavu hello obchodní logiky hello služby nebo zjišťování "zamrzlých" procesů. Data stavu hello se informace o konkrétní tootheir logikou lze rozšířit uživatele služby.

> [!NOTE]
> Sestavy stavu watchdogs jsou viditelné pouze *po* součásti systému hello vytvořte entitu. Při odstranění entity hello stavu úložiště automaticky odstraní všechny sestavy stavu s ním spojená. Hello stejné je hodnota true, když je vytvořena nová instance entity hello (například je vytvořena nová instance repliky stavové trvalou služby). Všechny sestavy přidružené k původní instance hello jsou odstraněna a vyčistit z úložiště hello.
> 
> 

Hello sestav součásti systému jsou identifikovány hello zdroj, který začíná textem hello "**systému.**" Předpona. Watchdogs nelze použít hello stejnou předponu pro jejich zdroje, jako jsou odmítnuta sestavy se neplatné parametry.
Pojďme podívejte se na některé systému sestavy toounderstand, co je aktivuje a jak toocorrect hello možných problémů se představují.

> [!NOTE]
> Service Fabric pokračuje tooadd sestavy týkající se podmínek, které vylepšují získat přehled o dění v hello cluster a aplikace. Existující sestavy lze také rozšířit s dalšími podrobnostmi toohelp řešení hello problému rychlejší.
> 
> 

## <a name="cluster-system-health-reports"></a>Cluster sestav o stavu systému
Hello clusteru stavu entity se vytvoří automaticky v hello health store. Pokud všechno funguje správně, nemá sestavu system.

### <a name="neighborhood-loss"></a>Ztráta Okolní počítače
**System.Federation** nahlásí chybu, když zjistí ztrátu okolí. Sestava Hello je z jednotlivých uzlů a ID uzlu hello je součástí názvu vlastnosti hello. Pokud je jeden okolí ztratili v Service Fabric prstenec celý text hello, můžete očekávat obvykle dvě události (obou stranách sestavy mezera hello). Pokud další sousedství jsou ztraceny, existují další události.

Sestava Hello Určuje časový limit zapůjčení globální hello jako čas hello toolive. Sestava Hello je nutno každých poloviny doba TTL hello tak dlouho, dokud podmínky hello zůstává aktivní. po jeho vypršení je automaticky odstraněna Hello událost. Odeberte, pokud vypršela platnost zaručuje, že sestava hello je vyčištěna z hello health store správně, přestože sestavy uzlu hello je vypnutý.

* **SourceId**: System.Federation
* **Vlastnost**: začíná **okolí** a obsahuje informace o uzlu
* **Další kroky**: Zjistěte, proč dojde ke ztrátě (např. Zkontrolujte hello komunikaci mezi uzly clusteru) a okolí hello.

## <a name="node-system-health-reports"></a>Uzel sestav o stavu systému
**System.FM**, který představuje službu Failover Manager hello, hello autoritou, která spravuje informace o uzly clusteru. Každý uzel musí mít jednu sestavu z System.FM zobrazuje stav. Hello uzlu entity se odeberou, když se odebere stav uzlu hello (viz [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).

### <a name="node-updown"></a>Uzel nahoru/dolů
System.FM nahlásí jako OK, když se uzel hello připojí hello prstenec (je spuštěná). Nahlásí chybu, pokud uzel hello vyplouvající hello prstenec (je vypnutý, buď pro upgrade nebo jednoduše protože se nezdařilo). Hello stavu hierarchie sestavena úložiště stavu hello provádět akce se nasazené entity v korelace s System.FM sestavy uzlu. Uzel hello považuje virtuální nadřazené všechny nasazené entit. Hello nasazení entit na tomto uzlu se zveřejňují přes dotazy, pokud uzel hello hlášení jako si podle System.FM, s hello stejné instance jako hello instanci spojenou s hello entity. Když System.FM ohlásí tento uzel hello je mimo provoz nebo restartovat (nové instance), úložiště stavu hello automaticky vyčistí hello nasazení entit, které může existovat pouze na hello dolů uzlu nebo na předchozí instanci hello hello uzlu.

* **SourceId**: System.FM
* **Vlastnost**: stavu
* **Další kroky**: Pokud uzel hello je vypnutý pro upgrade, by měl mít zpět po byl upgradován. V takovém případě by měl hello stav přepnout zpět tooOK. Pokud uzel hello nepřejde do stavu zpět nebo se nezdaří, problém hello potřebuje další šetření.

Hello následující příklad ukazuje hello System.FM události se stavem stavu OK pro uzel:

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
**System.FabricNode** sestavy upozornění, když se certifikáty používané hello uzlu blíží vypršení platnosti. Existují tři certifikáty na uzel: **Certificate_cluster**, **Certificate_server**, a **Certificate_default_client**. Alespoň dva týdny po vypršení platnosti hello je hello sestavy stavu v pořádku. Po vypršení platnosti hello během dvou týdnů hello typ sestavy je upozornění. Hodnota TTL z těchto událostí je nekonečno, a budou odstraněny Jestliže uzel opustí hello clusteru.

* **SourceId**: System.FabricNode
* **Vlastnost**: začíná **certifikát** a obsahuje další informace o typu certifikátu hello
* **Další kroky**: aktualizace certifikátů hello, pokud se blíží vypršení platnosti.

### <a name="load-capacity-violation"></a>Načíst porušení kapacity
Hello nástroj pro vyrovnávání zatížení Service Fabric hlásí upozornění, když zjistí porušení kapacity uzlu.

* **SourceId**: System.PLB
* **Vlastnost**: začíná **kapacity**
* **Další kroky**: Kontrola poskytuje metriky a zobrazení hello aktuální kapacity na uzlu hello.

## <a name="application-system-health-reports"></a>Aplikace sestav o stavu systému
**System.CM**, který představuje službu hello Správce clusteru, je hello autority, který spravuje informace o aplikaci.

### <a name="state"></a>Stav
System.CM nahlásí jako OK když aplikace hello byly vytvořeny nebo aktualizovány. Informuje hello stavu úložiště po odstranění aplikace hello tak, aby bylo možné odebrat z úložiště.

* **SourceId**: System.CM
* **Vlastnost**: stavu
* **Další kroky**: Pokud aplikace hello má byly vytvořeny nebo aktualizovány, měl by obsahovat sestava stavu hello Správce clusteru. Zkontrolujte, zda hello se stav aplikace hello vydáním dotazu (například hello rutiny prostředí PowerShell **Get ServiceFabricApplication - ApplicationName *applicationName***).

Hello následující příklad ukazuje událost stavu hello na hello **fabric: / WordCount** aplikace:

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
**System.FM**, který představuje službu Failover Manager hello, hello autoritou, která spravuje informace o službách.

### <a name="state"></a>Stav
System.FM sestavy jako OK po vytvoření služby hello. Odstraní hello entity z hello health store, pokud služba hello byla odstraněna.

* **SourceId**: System.FM
* **Vlastnost**: stavu

Hello následující příklad ukazuje hello události stavu služby hello **fabric: / WordCount/WordCountWebService**:

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
**System.PLB** nahlásí chybu, když zjistí, že aktualizace služby toobe korelační s jinou službu vytvoří řetězec vztahů. Sestava Hello je vymazán poté, co se stane úspěšná aktualizace.

* **SourceId**: System.PLB
* **Vlastnost**: ServiceDescription
* **Další kroky**: Kontrola hello korelační popis služby.

## <a name="partition-system-health-reports"></a>Oddíl sestav o stavu systému
**System.FM**, který představuje službu Failover Manager hello, hello autoritou, která spravuje informace o oddílech služby.

### <a name="state"></a>Stav
System.FM nahlásí jako OK když hello oddílu byl vytvořen a je v pořádku. Odstraní hello entity z hello health store při odstranění hello oddílu.

Pokud hello oddílu je menší než počet hello minimální repliky, nahlásí chybu. Pokud hello oddíl není nižší než počet hello minimální repliky, ale je nižší než počet replik cíl hello, sestavy upozornění. Pokud hello oddíl je ve ztrátě kvora, System.FM nahlásí chybu.

Další důležité události zahrnovat varování hello Rekonfigurace trvá déle, než se očekávalo, a při sestavení hello trvá déle, než se očekávalo. dobu Hello očekává hello sestavení a změny konfigurace se dají konfigurovat na základě scénářů služby. Například pokud má služba terabajt stavu, například SQL Database, hello sestavení trvá déle než pro službu s malou stavu.

* **SourceId**: System.FM
* **Vlastnost**: stavu
* **Další kroky**: Pokud hello stav není v pořádku, je možné, že některé repliky nebyly vytvořené, otevřenou nebo propagovaných tooprimary nebo sekundární správně. V mnoha případech je hello příčiny chyb služby v hello otevřít nebo změnit roli implementace.

Hello následující příklad zobrazuje oddíl v pořádku:

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

Hello následující příklad ukazuje stav hello oddíl, který je pod počtu cílových replik. dalším krokem Hello je tooget hello oddílu popis, který ukazuje, jak jsou nakonfigurované: **MinReplicaSetSize** je tři a **TargetReplicaSetSize** je 7. Potom získat hello počet uzlů v clusteru hello: pět. Proto v tomto případě dvě repliky nelze umístit, protože hello cílový počet replik je vyšší než hello počet uzlů, které jsou k dispozici.

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
**System.PLB** sestavy upozornění, pokud zjistí porušení omezení repliky a nelze umístit všechny repliky oddílu. Zobrazí podrobnosti sestavy Hello které omezení a vlastnosti zabránit umístění repliky hello.

* **SourceId**: System.PLB
* **Vlastnost**: začíná **ReplicaConstraintViolation**

## <a name="replica-system-health-reports"></a>Repliky sestav o stavu systému
**System.RA**, která představuje hello součást reconfiguration agent, je hello autority pro stav repliky hello.

### <a name="state"></a>Stav
**System.RA** sestavy OK po vytvoření repliky hello.

* **SourceId**: System.RA
* **Vlastnost**: stavu

Hello následující příklad ukazuje repliku v pořádku:

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
Hello popis této sestavy stavu obsahuje čas zahájení hello (Coordinated Universal Time), kdy byl vyvolán volání hello rozhraní API.

**System.RA** sestavy upozornění, pokud hello repliku open trvá déle, než období hello nakonfigurovaný (výchozí: 30 minut). Pokud hello API má dopad na dostupnost služby, je vydána hello sestavy mnohem rychleji (Konfigurovat intervalu, výchozí hodnota je 30 sekund). měření času Hello zahrnuje hello doba hello Replikátor otevřené a služba hello otevřete. dokončení Hello vlastnost změny tooOK Pokud hello otevřít.

* **SourceId**: System.RA
* **Vlastnost**: **ReplicaOpenStatus**
* **Další kroky**: Pokud hello stav není v pořádku, zjistěte, proč hello repliku open trvá déle, než se očekávalo.

### <a name="slow-service-api-call"></a>Pomalá volání rozhraní API služby
**System.RAP** a **System.Replicator** sestavy upozornění, pokud kód volání toohello uživatele služby trvá déle, než čas hello nakonfigurované. upozornění Hello je vymazán po dokončení volání hello.

* **SourceId**: System.RAP nebo System.Replicator
* **Vlastnost**: název hello hello pomalé rozhraní API. Popis Hello poskytuje další podrobnosti o hello čas hello rozhraní API byla čekající na vyřízení.
* **Další kroky**: Zjistěte, proč trvá déle, než bylo očekáváno volání hello.

Hello následující příklad zobrazuje oddíl ve ztrátě kvora a toofigure kroky provést šetření hello se důvod, proč. Jedna z repliky hello má stav varování, proto jeho stav. Zobrazuje, že operace hello služby trvá déle, než se očekávalo, událost hlášené System.RAP. Po přijetí těchto informací hello dalším krokem je toolook v kódu služby hello a prozkoumat existuje. Pro tento případ hello **RunAsync** implementace hello stavové služby, vyvolá k neošetřené výjimce. Hello repliky jsou recyklace, takže se nemusí zobrazovat všechny repliky ve stavu upozornění hello. Můžete opakovat získávání hello stavu a vyhledejte případné rozdíly v ID hello repliky. V některých případech hello opakování vám může poskytnout různá vodítka.

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

Při spuštění hello vadný aplikací v rámci v ladicím programu hello zobrazit hello diagnostických událostí windows hello došlo k výjimce z RunAsync:

![Visual Studio 2015 diagnostických událostí: selhání RunAsync v fabric: / HelloWorldStatefulApplication.][1]

Visual Studio 2015 diagnostických událostí: selhání RunAsync v **fabric: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Replikační fronta je plná
**System.Replicator** nahlásí upozornění, když hello fronta replikací je plná. Na hello primární fronty replikací obvykle plný protože jeden nebo více sekundárních replikách jsou pomalé tooacknowledge operace. Na hello sekundární to obvykle se stane, když služba hello pomalé tooapply hello operace. upozornění Hello je zrušeno při hello už zaplnění fronty.

* **SourceId**: System.Replicator
* **Vlastnost**: **PrimaryReplicationQueueStatus** nebo **SecondaryReplicationQueueStatus**, v závislosti na role repliky hello

### <a name="slow-naming-operations"></a>Pomalé operations pojmenování
**System.NamingService** při operaci pojmenování trvá déle než přijatelné hlásí stav na jeho primární repliky. Příklady operací pojmenování [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) nebo [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync). Další metody naleznete v části FabricClient, například v [služby metody správy](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) nebo [metody správy vlastnost](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).

> [!NOTE]
> Hello Naming service řeší služba názvy tooa umístění v clusteru hello a umožňuje uživatelům toomanage služby názvy a vlastnosti. Je, že služba jako trvalý, Service Fabric rozdělena na oddíly. Jeden z oddílů hello představuje hello Authority Owner, který obsahuje metadata o všechny názvy Service Fabric a služeb. Hello Service Fabric názvy nejsou namapované toodifferent oddíly, označované jako oddíly Name Owner, tak službu hello lze rozšířit. Další informace o [Naming service](service-fabric-architecture.md).
> 
> 

Při operaci pojmenování trvá déle, než se očekávalo, operace hello je označené zprávu upozornění o hello *primární repliky hello pojmenování oddílu služby, který slouží hello operaci*. Po úspěšném dokončení operace hello hello upozornění není zaškrtnuto. V případě hello operace skončí s chybou, hello stavu sestava obsahuje podrobnosti o chybě hello.

* **SourceId**: System.NamingService
* **Vlastnost**: začíná předponu **Duration_** a identifikuje hello pomalé operace a název hello Service Fabric, na které hello operaci použít. Například pokud vytvořit službu na název fabric: / MyApp/Moje_služba trvá příliš dlouho, vlastnost hello je Duration_AOCreateService.fabric:/MyApp/MyService. AO body toohello role hello pojmenování oddílu pro tento název a operaci.
* **Další kroky**: Kontrola proč hello pojmenování operace selže. Každé operace může mít různé kořenové příčiny. Například odstranit, může být služba zablokována na uzlu, protože hostitel aplikace hello udržuje chybám na uzlu z důvodu chyb tooa uživatele v kódu služby hello.

Hello následující příklad ukazuje operaci vytvoření služby. operace Hello trvalo déle než hello nakonfigurovaná doba trvání. AO opakování a odešle pracovní tooNO. ŽÁDNÉ dokončené hello poslední operaci s vypršením časového limitu. V takovém případě hello stejné replika je primární hello AO a žádné role.

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
                        Description           : hello AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
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
                        Description           : hello NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>DeployedApplication sestav o stavu systému
**System.Hosting** hello autoritou na nasazené entity.

### <a name="activation"></a>Aktivace
System.Hosting sestavy jako OK aplikace byla úspěšně aktivaci v uzlu hello. V opačném případě nahlásí chybu.

* **SourceId**: System.Hosting
* **Vlastnost**: aktivace, včetně hello zavedení verze
* **Další kroky**: Pokud aplikace hello není v pořádku, zjistěte, proč hello aktivace se nezdařila.

Hello následující příklad ukazuje úspěšné aktivaci:

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
                                     Description           : hello application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>Ke stažení
**System.Hosting** nahlásí chybu, pokud stahování balíčku aplikace hello selže.

* **SourceId**: System.Hosting
* **Vlastnost**:  **stáhnout:*RolloutVersion***
* **Další kroky**: Zjistěte, proč se nepodařilo stáhnout hello na uzlu hello.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage sestav o stavu systému
**System.Hosting** hello autoritou na nasazené entity.

### <a name="service-package-activation"></a>Služba aktivace balíčku
System.Hosting sestavy jako OK, pokud je aktivace balíčku hello služby v uzlu hello úspěšná. V opačném případě nahlásí chybu.

* **SourceId**: System.Hosting
* **Vlastnost**: Aktivace
* **Další kroky**: Zjistěte, proč hello aktivace se nezdařila.

### <a name="code-package-activation"></a>Aktivace balíčku kódu.
**System.Hosting** sestavy jako OK pro každý balíček kódu, pokud je hello aktivace úspěšná. Pokud hello aktivace nepodaří, sestavy upozornění podle konfigurace. Pokud **CodePackage** selže tooactivate nebo ukončí s chybou větší než nakonfigurovaný hello **CodePackageHealthErrorThreshold**, nahlásí chybu, který je hostitelem. Pokud balíček služby obsahuje více balíčků kódu, zprávu o aktivaci se generuje pro každé z nich.

* **SourceId**: System.Hosting
* **Vlastnost**: Předpona hello používá **CodePackageActivation** a obsahuje název hello balíček kódu hello a hello vstupního bodu jako  **CodePackageActivation:* CodePackageName*:*SetupEntryPoint/EntryPoint*** (například **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Typ registrace služby
**System.Hosting** sestavy jako OK, pokud typ služby hello byl úspěšně zaregistrován. Nahlásí chybu, pokud nebyla provedena registrace hello v čase (jak je nakonfigurovat pomocí **ServiceTypeRegistrationTimeout**). Pokud je zavřená hello runtime, typ služby hello je odregistrovat z uzlu hello a hostitelský oznámí upozornění.

* **SourceId**: System.Hosting
* **Vlastnost**: Předpona hello používá **ServiceTypeRegistration** a obsahuje název typu služby hello (například **ServiceTypeRegistration:FileStoreServiceType**)

Hello následující příklad ukazuje v pořádku nasazený balíček služby:

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
                             Description           : hello ServicePackage was activated successfully.
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
                             Description           : hello CodePackage was activated successfully.
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
                             Description           : hello ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>Ke stažení
**System.Hosting** nahlásí chybu, pokud stahování balíčku služby hello selže.

* **SourceId**: System.Hosting
* **Vlastnost**:  **stáhnout:*RolloutVersion***
* **Další kroky**: Zjistěte, proč se nepodařilo stáhnout hello na uzlu hello.

### <a name="upgrade-validation"></a>Ověření upgradu
**System.Hosting** nahlásí chybu, pokud selže ověření během upgradu hello nebo pokud hello upgrade selže na uzlu hello.

* **SourceId**: System.Hosting
* **Vlastnost**: Předpona hello používá **FabricUpgradeValidation** a obsahuje hello upgradu verze
* **Popis**: body toohello došlo k chybě

## <a name="next-steps"></a>Další kroky
[Zobrazit sestavy stavu Service Fabric](service-fabric-view-entities-aggregated-health.md)

[Jak tooreport a zkontrolujte stav služby](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Monitorování a Diagnostika služby místně](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade aplikace Service Fabric](service-fabric-application-upgrade.md)

