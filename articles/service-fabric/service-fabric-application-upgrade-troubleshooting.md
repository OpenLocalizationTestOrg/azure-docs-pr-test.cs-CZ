---
title: "upgrady aplikací aaaTroubleshooting | Microsoft Docs"
description: "Tento článek popisuje některé běžné problémy týkající se upgradu aplikace Service Fabric a jak tooresolve je."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 19ad152e-ec50-4327-9f19-065c875c003c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 0f56fa61db9b4e32824623f162dc1bfe7fda0f49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-application-upgrades"></a>Řešení potíží s upgrady aplikací
Tento článek popisuje některé běžné problémy hello kolem upgrade aplikace Azure Service Fabric a jak tooresolve je.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Řešení potíží při upgradu aplikaci, která selhala
Pokud upgrade selže, hello výstup hello **Get-ServiceFabricApplicationUpgrade** příkaz obsahuje další informace o ladění hello selhání.  Hello následující seznam určuje použití hello Další informace:

1. Určete typ chyby hello.
2. Určete důvod selhání hello.
3. Izolujte jednu nebo více součástí selhání dále prozkoumat.

Tyto informace jsou k dispozici, když Service Fabric zjistí selhání hello bez ohledu na to, zda hello **FailureAction** je tooroll zpět nebo pozastavit hello upgradu.

### <a name="identify-hello-failure-type"></a>Identifikace typu selhání hello
V výstup hello **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** identifikuje hello časové razítko (ve formátu UTC), kdy bylo zjištěno selhání upgradu pomocí Service Fabric a  **FailureAction** byla aktivována. **Důvod chyby** označuje jeden tři základní příčin selhání hello:

1. UpgradeDomainTimeout – označuje, že konkrétní upgradu domény trvalo příliš dlouho toocomplete a **UpgradeDomainTimeout** vypršela platnost.
2. OverallUpgradeTimeout – označuje, že hello celkové upgradu trvalo příliš dlouho toocomplete a **UpgradeTimeout** vypršela platnost.
3. HealthCheck – označuje, že po dokončení upgradu domény služby aktualizace zůstal hello aplikace není v pořádku podle toohello zadané zásady stavu a **HealthCheckRetryTimeout** vypršela platnost.

Tyto položky pouze zobrazí ve výstupu hello hello upgradu se nezdaří a spustí vrácení zpět. Další informace se zobrazí v závislosti na typu hello hello selhání.

### <a name="investigate-upgrade-timeouts"></a>Prozkoumat upgradu časové limity
Upgrade vypršení časového limitu, selhání jsou nejčastěji způsobeny problémy s dostupností služby. výstup Hello níže je typický pro upgrady, které služba replik nebo instancí nezdaří toostart v nové verzi kódu hello. Hello **UpgradeDomainProgressAtFailure** pole zaznamená snímek žádné čekající upgradu práce v době selhání hello.

```
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
```

V tomto příkladu hello upgrade se nezdařil v doméně pro upgrade *MYUD1* a dva oddíly (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* a *4b43f4d8-b26b-424e-9307-7a7a62e79750*) se zablokuje. oddíly Hello byly zablokované kvůli primární repliky nelze tooplace hello runtime (*WaitForPrimaryPlacement*) na cílové uzly *Uzel1* a *Uzel4*.

Hello **Get-ServiceFabricNode** příkaz lze použít tooverify, které jsou tyto dva uzly v doméně pro upgrade *MYUD1*. Hello *UpgradePhase* uvádí *PostUpgradeSafetyCheck*, což znamená, že tyto kontroly zabezpečení dochází po dokončení upgradu všech uzlů v doméně pro upgrade hello. Tyto informace body tooa potenciální problém s novou verzi kódu aplikace hello hello. Většina běžných potíží s Hello jsou chyby služby v hello otevřené nebo cesty kódu tooprimary povýšení.

*UpgradePhase* z *PreUpgradeSafetyCheck* znamená byly nějaké problémy Příprava domény upgradu hello předtím, než byla provedena. Hello nejběžnějších problémů v tomto případě jsou chyby služby v hello zavřít nebo snížení úrovně z cesty primární kódu.

Hello aktuální **UpgradeState** je *RollingBackCompleted*, takže původní upgrade hello musí prováděly s vrácení zpět **FailureAction**, které automaticky vrátit zpět upgrade hello při selhání. Pokud se ruční byl proveden upgrade původní hello **FailureAction**, pak hello upgradu místo toho je v pozastaveném stavu tooallow, který je za provozu, ladění aplikace hello.

### <a name="investigate-health-check-failures"></a>Prozkoumat selhání kontroly stavu
Selhání kontroly stavu mohou být spouštěny různé problémy, které může dojít po všechny uzly v doméně upgradu dokončete upgrade a předání všechny kontroly zabezpečení. výstup Hello níže je typický pro upgrade nezdaří z důvodu toofailed kontroly stavu. Hello **UnhealthyEvaluations** pole zaznamená snímek kontroly stavu, které k chybě v době hello hello upgradu podle toohello zadaný [zásady stavu](service-fabric-health-introduction.md).

```
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
```

Příčin selhání kontroly stavu nejprve vyžaduje znalost hello Service Fabric stavu modelu. Ale i bez takové detailní znalost, uvidíte, že jsou dvě služby není v pořádku: *fabric: / DemoApp/Svc3* a *fabric: / DemoApp/Svc2*, společně s hello sestav stavu chyby ("InjectedFault "v tomto případě). V tomto příkladu dva ze čtyř jsou služby není v pořádku, což je nižší než cílová výchozí hello 0 % není v pořádku (*MaxPercentUnhealthyServices*).

Hello upgradu byla pozastavena při selhání tak, že zadáte **FailureAction** z ruční při spouštění hello upgradu. Tento režim umožňuje tooinvestigate hello systému za provozu ve stavu hello se nezdařil před provedením jakékoli další akce.

### <a name="recover-from-a-suspended-upgrade"></a>Obnovit pozastavenou upgradem
S vrácení zpět **FailureAction**, obnovit potřeby vzhledem k tomu, že hello upgrade automaticky vrátí zpět při selhání. S ruční **FailureAction**, máte několik možností obnovení:

1.  aktivační událost vrácení zpět.
2. Pokračujte hello zbytek hello upgrade ručně
3. Obnovit hello monitorovat upgradu

Hello **Start-ServiceFabricApplicationRollback** příkaz lze použít na všechny toostart čas vrácení zpět aplikace hello. Jakmile hello příkaz vrátí úspěšně, požadavek na vrácení hello je zaregistrován v systému hello a spustí krátce po tomto datu.

Hello **Resume-ServiceFabricApplicationUpgrade** příkaz lze použít tooproceed prostřednictvím hello zbytek hello upgrade ručně, jednu upgradovací doménu najednou. V tomto režimu jsou prováděny pouze bezpečnostní kontroly systému hello. Budou provedeny žádné další kontroly stavu. Tento příkaz lze použít pouze v případě hello *UpgradeState* ukazuje *RollingForwardPending*, což znamená, že hello doména upgradu současné dokončil upgrade ale hello další jeden nebyla spuštěna (čeká na zpracování).

Hello **aktualizace ServiceFabricApplicationUpgrade** příkaz lze použít tooresume hello monitorovat upgrade s kontroly zabezpečení a stavu se provést.

```
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
```

Hello upgrade pokračuje z hello upgradovací doméně, kde byl poslední pozastavený a hello použijte stejný upgrade parametry a zásad stavu jako před. V případě potřeby žádné parametry upgradu hello a zásad stavu, které jsou uvedené v předcházející výstup hello lze změnit v hello stejný příkaz při upgradu hello obnoví. V tomto příkladu hello upgrade obnovena v režimu monitorované s parametry hello a zásad stavu hello beze změny.

## <a name="further-troubleshooting"></a>Další informace o řešení
### <a name="service-fabric-is-not-following-hello-specified-health-policies"></a>Service Fabric není následující hello zadané zásady stavu
Možné příčiny 1:

Service Fabric překládá všechny procenta do skutečný počet entit (například repliky, oddíly a služby) pro vyhodnocení stavu a vždy zaokrouhlí toowhole entity. Například, pokud hello maximální *MaxPercentUnhealthyReplicasPerPartition* je 21 % a existují pět repliky, pak Service Fabric umožňuje tootwo repliky není v pořádku (tedy`Math.Ceiling (5*0.21)`). Zásady stavu proto musí být nastaven odpovídajícím způsobem.

Možná příčina 2:

Zásady stavu jsou specifikované jako procenta celkového služby a instance nejsou specifické služby. Například před provedením upgradu, pokud aplikace má čtyři služby instancí A, B, C a D, kde je služba D není v pořádku, ale s málo aplikace toohello dopad. Chceme tooignore hello známé služby není v pořádku D během upgradu a nastavte hello parametr *MaxPercentUnhealthyServices* toobe 25 %, za předpokladu, že pouze A, B a C potřebovat toobe v pořádku.

Však během upgradu hello D může se nezotavila během C se změní na není v pořádku. Hello upgrade by být přesto úspěšné, protože jsou pouze 25 % hello služby není v pořádku. Však může vést k neočekávané chybám kvůli tooC se neočekávaně není v pořádku místo D. V takovém případě by měl být D modelovaná jako typu různé služby z A a B a c Vzhledem k tomu, že zásady stavu jsou nastaveny na typ služby, může být jiné není v pořádku procento prahové hodnoty použité toodifferent služby. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-hello-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>I nezadali zásady stavu pro upgradu aplikace, ale hello upgrade stále nedaří pro některé vypršení časových limitů, nikdy zadaných
Pokud zásady stavu nejsou zadány toohello žádost o upgrade, jsou převzaty z hello *ApplicationManifest.xml* hello aktuální verze aplikace. Například pokud aplikace X provádíte upgrade z verze 1.0 tooversion 2.0, se používají zásady stavu aplikace podle verze 1.0. Pokud se má používat jiný stav zásad pro hello upgrade, zásady hello musí toobe zadaný jako součást upgradu volání rozhraní API hello aplikace. Hello zásady zadaný jako součást volání hello rozhraní API se projeví pouze během upgradu hello. Po dokončení upgradu hello hello zásady zadaná v hello *ApplicationManifest.xml* se používají.

### <a name="incorrect-time-outs-are-specified"></a>Jsou zadány nesprávné vypršení časových limitů
Vám může mít zajímalo o co se stane, když jsou nekonzistentně nastavit vypršení časových limitů. Například můžete mít *UpgradeTimeout* , je menší než hello *UpgradeDomainTimeout*. Hello odpověď se vrátí chybu. Chyby se vrátí v případě hello *UpgradeDomainTimeout* je menší než součet hello *HealthCheckWaitDuration* a *HealthCheckRetryTimeout*, nebo pokud  *UpgradeDomainTimeout* je menší než součet hello *HealthCheckWaitDuration* a *HealthCheckStableDuration*.

### <a name="my-upgrades-are-taking-too-long"></a>Moje upgrady trvá příliš dlouho
Hello dobu upgradu toocomplete závisí na kontroly stavu hello a vypršení časových limitů zadán. Kontroly stavu a vypršení časových limitů závisí na tom, jak dlouho trvalo toocopy, nasazení a stabilizaci aplikace hello. Probíhá příliš agresivní s vypršení časových limitů může znamenat více se selháním upgradu, takže doporučujeme můžete začít s delší časové limity.

Tady je rychlý aktualizačnímu programu na interakci hello vypršení časových limitů s upgradem časy hello:

Upgrady pro upgradovací doméně nemůže dokončit rychlejší než *HealthCheckWaitDuration* + *HealthCheckStableDuration*.

Selhání při upgradu nemůže proběhnout, rychlejší než *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.

Hello doba upgradu upgradu domény je omezena *UpgradeDomainTimeout*.  Pokud *HealthCheckRetryTimeout* a *HealthCheckStableDuration* jsou oba nenulové a hello stavu aplikace hello udržuje přepínání a zpět, pak na nakonecvypršíhelloupgradu *UpgradeDomainTimeout*. *UpgradeDomainTimeout* spustí jednou odpočítávání hello upgradu pro začne doména upgradu současné hello.

## <a name="next-steps"></a>Další kroky
[Upgrade vaší aplikace pomocí sady Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí sady Visual Studio.

[Upgrade vaší aplikace pomocí prostředí Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vás provede upgrade aplikace pomocí prostředí PowerShell.

Řídí, jak vaše aplikace upgraduje pomocí [Upgrade parametry](service-fabric-application-upgrade-parameters.md).

Aby aplikace upgradů kompatibilní metodou učení jak toouse [serializace dat](service-fabric-application-upgrade-data-serialization.md).

Zjistěte, jak toouse pokročilé funkce při upgradu vaší aplikace tím, že odkazuje příliš[Pokročilá témata](service-fabric-application-upgrade-advanced.md).
