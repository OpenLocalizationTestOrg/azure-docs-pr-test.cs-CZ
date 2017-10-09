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
# <a name="troubleshoot-application-upgrades"></a><span data-ttu-id="32cc0-103">Řešení potíží s upgrady aplikací</span><span class="sxs-lookup"><span data-stu-id="32cc0-103">Troubleshoot application upgrades</span></span>
<span data-ttu-id="32cc0-104">Tento článek popisuje některé běžné problémy hello kolem upgrade aplikace Azure Service Fabric a jak tooresolve je.</span><span class="sxs-lookup"><span data-stu-id="32cc0-104">This article covers some of hello common issues around upgrading an Azure Service Fabric application and how tooresolve them.</span></span>

## <a name="troubleshoot-a-failed-application-upgrade"></a><span data-ttu-id="32cc0-105">Řešení potíží při upgradu aplikaci, která selhala</span><span class="sxs-lookup"><span data-stu-id="32cc0-105">Troubleshoot a failed application upgrade</span></span>
<span data-ttu-id="32cc0-106">Pokud upgrade selže, hello výstup hello **Get-ServiceFabricApplicationUpgrade** příkaz obsahuje další informace o ladění hello selhání.</span><span class="sxs-lookup"><span data-stu-id="32cc0-106">When an upgrade fails, hello output of hello **Get-ServiceFabricApplicationUpgrade** command contains additional information for debugging hello failure.</span></span>  <span data-ttu-id="32cc0-107">Hello následující seznam určuje použití hello Další informace:</span><span class="sxs-lookup"><span data-stu-id="32cc0-107">hello following list specifies how hello additional information can be used:</span></span>

1. <span data-ttu-id="32cc0-108">Určete typ chyby hello.</span><span class="sxs-lookup"><span data-stu-id="32cc0-108">Identify hello failure type.</span></span>
2. <span data-ttu-id="32cc0-109">Určete důvod selhání hello.</span><span class="sxs-lookup"><span data-stu-id="32cc0-109">Identify hello failure reason.</span></span>
3. <span data-ttu-id="32cc0-110">Izolujte jednu nebo více součástí selhání dále prozkoumat.</span><span class="sxs-lookup"><span data-stu-id="32cc0-110">Isolate one or more failing components for further investigation.</span></span>

<span data-ttu-id="32cc0-111">Tyto informace jsou k dispozici, když Service Fabric zjistí selhání hello bez ohledu na to, zda hello **FailureAction** je tooroll zpět nebo pozastavit hello upgradu.</span><span class="sxs-lookup"><span data-stu-id="32cc0-111">This information is available when Service Fabric detects hello failure regardless of whether hello **FailureAction** is tooroll back or suspend hello upgrade.</span></span>

### <a name="identify-hello-failure-type"></a><span data-ttu-id="32cc0-112">Identifikace typu selhání hello</span><span class="sxs-lookup"><span data-stu-id="32cc0-112">Identify hello failure type</span></span>
<span data-ttu-id="32cc0-113">V výstup hello **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** identifikuje hello časové razítko (ve formátu UTC), kdy bylo zjištěno selhání upgradu pomocí Service Fabric a  **FailureAction** byla aktivována.</span><span class="sxs-lookup"><span data-stu-id="32cc0-113">In hello output of **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** identifies hello timestamp (in UTC) at which an upgrade failure was detected by Service Fabric and **FailureAction** was triggered.</span></span> <span data-ttu-id="32cc0-114">**Důvod chyby** označuje jeden tři základní příčin selhání hello:</span><span class="sxs-lookup"><span data-stu-id="32cc0-114">**FailureReason** identifies one of three potential high-level causes of hello failure:</span></span>

1. <span data-ttu-id="32cc0-115">UpgradeDomainTimeout – označuje, že konkrétní upgradu domény trvalo příliš dlouho toocomplete a **UpgradeDomainTimeout** vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="32cc0-115">UpgradeDomainTimeout - Indicates that a particular upgrade domain took too long toocomplete and **UpgradeDomainTimeout** expired.</span></span>
2. <span data-ttu-id="32cc0-116">OverallUpgradeTimeout – označuje, že hello celkové upgradu trvalo příliš dlouho toocomplete a **UpgradeTimeout** vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="32cc0-116">OverallUpgradeTimeout - Indicates that hello overall upgrade took too long toocomplete and **UpgradeTimeout** expired.</span></span>
3. <span data-ttu-id="32cc0-117">HealthCheck – označuje, že po dokončení upgradu domény služby aktualizace zůstal hello aplikace není v pořádku podle toohello zadané zásady stavu a **HealthCheckRetryTimeout** vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="32cc0-117">HealthCheck - Indicates that after upgrading an update domain, hello application remained unhealthy according toohello specified health policies and **HealthCheckRetryTimeout** expired.</span></span>

<span data-ttu-id="32cc0-118">Tyto položky pouze zobrazí ve výstupu hello hello upgradu se nezdaří a spustí vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="32cc0-118">These entries only show up in hello output when hello upgrade fails and starts rolling back.</span></span> <span data-ttu-id="32cc0-119">Další informace se zobrazí v závislosti na typu hello hello selhání.</span><span class="sxs-lookup"><span data-stu-id="32cc0-119">Further information is displayed depending on hello type of hello failure.</span></span>

### <a name="investigate-upgrade-timeouts"></a><span data-ttu-id="32cc0-120">Prozkoumat upgradu časové limity</span><span class="sxs-lookup"><span data-stu-id="32cc0-120">Investigate upgrade timeouts</span></span>
<span data-ttu-id="32cc0-121">Upgrade vypršení časového limitu, selhání jsou nejčastěji způsobeny problémy s dostupností služby.</span><span class="sxs-lookup"><span data-stu-id="32cc0-121">Upgrade timeout failures are most commonly caused by service availability issues.</span></span> <span data-ttu-id="32cc0-122">výstup Hello níže je typický pro upgrady, které služba replik nebo instancí nezdaří toostart v nové verzi kódu hello.</span><span class="sxs-lookup"><span data-stu-id="32cc0-122">hello output following this paragraph is typical of upgrades where service replicas or instances fail toostart in hello new code version.</span></span> <span data-ttu-id="32cc0-123">Hello **UpgradeDomainProgressAtFailure** pole zaznamená snímek žádné čekající upgradu práce v době selhání hello.</span><span class="sxs-lookup"><span data-stu-id="32cc0-123">hello **UpgradeDomainProgressAtFailure** field captures a snapshot of any pending upgrade work at hello time of failure.</span></span>

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

<span data-ttu-id="32cc0-124">V tomto příkladu hello upgrade se nezdařil v doméně pro upgrade *MYUD1* a dva oddíly (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* a *4b43f4d8-b26b-424e-9307-7a7a62e79750*) se zablokuje.</span><span class="sxs-lookup"><span data-stu-id="32cc0-124">In this example, hello upgrade failed at upgrade domain *MYUD1* and two partitions (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* and *4b43f4d8-b26b-424e-9307-7a7a62e79750*) were stuck.</span></span> <span data-ttu-id="32cc0-125">oddíly Hello byly zablokované kvůli primární repliky nelze tooplace hello runtime (*WaitForPrimaryPlacement*) na cílové uzly *Uzel1* a *Uzel4*.</span><span class="sxs-lookup"><span data-stu-id="32cc0-125">hello partitions were stuck because hello runtime was unable tooplace primary replicas (*WaitForPrimaryPlacement*) on target nodes *Node1* and *Node4*.</span></span>

<span data-ttu-id="32cc0-126">Hello **Get-ServiceFabricNode** příkaz lze použít tooverify, které jsou tyto dva uzly v doméně pro upgrade *MYUD1*.</span><span class="sxs-lookup"><span data-stu-id="32cc0-126">hello **Get-ServiceFabricNode** command can be used tooverify that these two nodes are in upgrade domain *MYUD1*.</span></span> <span data-ttu-id="32cc0-127">Hello *UpgradePhase* uvádí *PostUpgradeSafetyCheck*, což znamená, že tyto kontroly zabezpečení dochází po dokončení upgradu všech uzlů v doméně pro upgrade hello.</span><span class="sxs-lookup"><span data-stu-id="32cc0-127">hello *UpgradePhase* says *PostUpgradeSafetyCheck*, which means that these safety checks are occurring after all nodes in hello upgrade domain have finished upgrading.</span></span> <span data-ttu-id="32cc0-128">Tyto informace body tooa potenciální problém s novou verzi kódu aplikace hello hello.</span><span class="sxs-lookup"><span data-stu-id="32cc0-128">All this information points tooa potential issue with hello new version of hello application code.</span></span> <span data-ttu-id="32cc0-129">Většina běžných potíží s Hello jsou chyby služby v hello otevřené nebo cesty kódu tooprimary povýšení.</span><span class="sxs-lookup"><span data-stu-id="32cc0-129">hello most common issues are service errors in hello open or promotion tooprimary code paths.</span></span>

<span data-ttu-id="32cc0-130">*UpgradePhase* z *PreUpgradeSafetyCheck* znamená byly nějaké problémy Příprava domény upgradu hello předtím, než byla provedena.</span><span class="sxs-lookup"><span data-stu-id="32cc0-130">An *UpgradePhase* of *PreUpgradeSafetyCheck* means there were issues preparing hello upgrade domain before it was performed.</span></span> <span data-ttu-id="32cc0-131">Hello nejběžnějších problémů v tomto případě jsou chyby služby v hello zavřít nebo snížení úrovně z cesty primární kódu.</span><span class="sxs-lookup"><span data-stu-id="32cc0-131">hello most common issues in this case are service errors in hello close or demotion from primary code paths.</span></span>

<span data-ttu-id="32cc0-132">Hello aktuální **UpgradeState** je *RollingBackCompleted*, takže původní upgrade hello musí prováděly s vrácení zpět **FailureAction**, které automaticky vrátit zpět upgrade hello při selhání.</span><span class="sxs-lookup"><span data-stu-id="32cc0-132">hello current **UpgradeState** is *RollingBackCompleted*, so hello original upgrade must have been performed with a rollback **FailureAction**, which automatically rolled back hello upgrade upon failure.</span></span> <span data-ttu-id="32cc0-133">Pokud se ruční byl proveden upgrade původní hello **FailureAction**, pak hello upgradu místo toho je v pozastaveném stavu tooallow, který je za provozu, ladění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="32cc0-133">If hello original upgrade was performed with a manual **FailureAction**, then hello upgrade would instead be in a suspended state tooallow live debugging of hello application.</span></span>

### <a name="investigate-health-check-failures"></a><span data-ttu-id="32cc0-134">Prozkoumat selhání kontroly stavu</span><span class="sxs-lookup"><span data-stu-id="32cc0-134">Investigate health check failures</span></span>
<span data-ttu-id="32cc0-135">Selhání kontroly stavu mohou být spouštěny různé problémy, které může dojít po všechny uzly v doméně upgradu dokončete upgrade a předání všechny kontroly zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="32cc0-135">Health check failures can be triggered by various issues that can happen after all nodes in an upgrade domain finish upgrading and passing all safety checks.</span></span> <span data-ttu-id="32cc0-136">výstup Hello níže je typický pro upgrade nezdaří z důvodu toofailed kontroly stavu.</span><span class="sxs-lookup"><span data-stu-id="32cc0-136">hello output following this paragraph is typical of an upgrade failure due toofailed health checks.</span></span> <span data-ttu-id="32cc0-137">Hello **UnhealthyEvaluations** pole zaznamená snímek kontroly stavu, které k chybě v době hello hello upgradu podle toohello zadaný [zásady stavu](service-fabric-health-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="32cc0-137">hello **UnhealthyEvaluations** field captures a snapshot of health checks that failed at hello time of hello upgrade according toohello specified [health policy](service-fabric-health-introduction.md).</span></span>

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

<span data-ttu-id="32cc0-138">Příčin selhání kontroly stavu nejprve vyžaduje znalost hello Service Fabric stavu modelu.</span><span class="sxs-lookup"><span data-stu-id="32cc0-138">Investigating health check failures first requires an understanding of hello Service Fabric health model.</span></span> <span data-ttu-id="32cc0-139">Ale i bez takové detailní znalost, uvidíte, že jsou dvě služby není v pořádku: *fabric: / DemoApp/Svc3* a *fabric: / DemoApp/Svc2*, společně s hello sestav stavu chyby ("InjectedFault "v tomto případě).</span><span class="sxs-lookup"><span data-stu-id="32cc0-139">But even without such an in-depth understanding, we can see that two services are unhealthy: *fabric:/DemoApp/Svc3* and *fabric:/DemoApp/Svc2*, along with hello error health reports ("InjectedFault" in this case).</span></span> <span data-ttu-id="32cc0-140">V tomto příkladu dva ze čtyř jsou služby není v pořádku, což je nižší než cílová výchozí hello 0 % není v pořádku (*MaxPercentUnhealthyServices*).</span><span class="sxs-lookup"><span data-stu-id="32cc0-140">In this example, two out of four services are unhealthy, which is below hello default target of 0% unhealthy (*MaxPercentUnhealthyServices*).</span></span>

<span data-ttu-id="32cc0-141">Hello upgradu byla pozastavena při selhání tak, že zadáte **FailureAction** z ruční při spouštění hello upgradu.</span><span class="sxs-lookup"><span data-stu-id="32cc0-141">hello upgrade was suspended upon failing by specifying a **FailureAction** of manual when starting hello upgrade.</span></span> <span data-ttu-id="32cc0-142">Tento režim umožňuje tooinvestigate hello systému za provozu ve stavu hello se nezdařil před provedením jakékoli další akce.</span><span class="sxs-lookup"><span data-stu-id="32cc0-142">This mode allows us tooinvestigate hello live system in hello failed state before taking any further action.</span></span>

### <a name="recover-from-a-suspended-upgrade"></a><span data-ttu-id="32cc0-143">Obnovit pozastavenou upgradem</span><span class="sxs-lookup"><span data-stu-id="32cc0-143">Recover from a suspended upgrade</span></span>
<span data-ttu-id="32cc0-144">S vrácení zpět **FailureAction**, obnovit potřeby vzhledem k tomu, že hello upgrade automaticky vrátí zpět při selhání.</span><span class="sxs-lookup"><span data-stu-id="32cc0-144">With a rollback **FailureAction**, there is no recovery needed since hello upgrade automatically rolls back upon failing.</span></span> <span data-ttu-id="32cc0-145">S ruční **FailureAction**, máte několik možností obnovení:</span><span class="sxs-lookup"><span data-stu-id="32cc0-145">With a manual **FailureAction**, there are several recovery options:</span></span>

1.  <span data-ttu-id="32cc0-146">aktivační událost vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="32cc0-146">trigger a rollback</span></span>
2. <span data-ttu-id="32cc0-147">Pokračujte hello zbytek hello upgrade ručně</span><span class="sxs-lookup"><span data-stu-id="32cc0-147">Proceed through hello remainder of hello upgrade manually</span></span>
3. <span data-ttu-id="32cc0-148">Obnovit hello monitorovat upgradu</span><span class="sxs-lookup"><span data-stu-id="32cc0-148">Resume hello monitored upgrade</span></span>

<span data-ttu-id="32cc0-149">Hello **Start-ServiceFabricApplicationRollback** příkaz lze použít na všechny toostart čas vrácení zpět aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="32cc0-149">hello **Start-ServiceFabricApplicationRollback** command can be used at any time toostart rolling back hello application.</span></span> <span data-ttu-id="32cc0-150">Jakmile hello příkaz vrátí úspěšně, požadavek na vrácení hello je zaregistrován v systému hello a spustí krátce po tomto datu.</span><span class="sxs-lookup"><span data-stu-id="32cc0-150">Once hello command returns successfully, hello rollback request has been registered in hello system and starts shortly thereafter.</span></span>

<span data-ttu-id="32cc0-151">Hello **Resume-ServiceFabricApplicationUpgrade** příkaz lze použít tooproceed prostřednictvím hello zbytek hello upgrade ručně, jednu upgradovací doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="32cc0-151">hello **Resume-ServiceFabricApplicationUpgrade** command can be used tooproceed through hello remainder of hello upgrade manually, one upgrade domain at a time.</span></span> <span data-ttu-id="32cc0-152">V tomto režimu jsou prováděny pouze bezpečnostní kontroly systému hello.</span><span class="sxs-lookup"><span data-stu-id="32cc0-152">In this mode, only safety checks are performed by hello system.</span></span> <span data-ttu-id="32cc0-153">Budou provedeny žádné další kontroly stavu.</span><span class="sxs-lookup"><span data-stu-id="32cc0-153">No more health checks are performed.</span></span> <span data-ttu-id="32cc0-154">Tento příkaz lze použít pouze v případě hello *UpgradeState* ukazuje *RollingForwardPending*, což znamená, že hello doména upgradu současné dokončil upgrade ale hello další jeden nebyla spuštěna (čeká na zpracování).</span><span class="sxs-lookup"><span data-stu-id="32cc0-154">This command can only be used when hello *UpgradeState* shows *RollingForwardPending*, which means that hello current upgrade domain has finished upgrading but hello next one has not started (pending).</span></span>

<span data-ttu-id="32cc0-155">Hello **aktualizace ServiceFabricApplicationUpgrade** příkaz lze použít tooresume hello monitorovat upgrade s kontroly zabezpečení a stavu se provést.</span><span class="sxs-lookup"><span data-stu-id="32cc0-155">hello **Update-ServiceFabricApplicationUpgrade** command can be used tooresume hello monitored upgrade with both safety and health checks being performed.</span></span>

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

<span data-ttu-id="32cc0-156">Hello upgrade pokračuje z hello upgradovací doméně, kde byl poslední pozastavený a hello použijte stejný upgrade parametry a zásad stavu jako před.</span><span class="sxs-lookup"><span data-stu-id="32cc0-156">hello upgrade continues from hello upgrade domain where it was last suspended and use hello same upgrade parameters and health policies as before.</span></span> <span data-ttu-id="32cc0-157">V případě potřeby žádné parametry upgradu hello a zásad stavu, které jsou uvedené v předcházející výstup hello lze změnit v hello stejný příkaz při upgradu hello obnoví.</span><span class="sxs-lookup"><span data-stu-id="32cc0-157">If needed, any of hello upgrade parameters and health policies shown in hello preceding output can be changed in hello same command when hello upgrade resumes.</span></span> <span data-ttu-id="32cc0-158">V tomto příkladu hello upgrade obnovena v režimu monitorované s parametry hello a zásad stavu hello beze změny.</span><span class="sxs-lookup"><span data-stu-id="32cc0-158">In this example, hello upgrade was resumed in Monitored mode, with hello parameters and hello health policies unchanged.</span></span>

## <a name="further-troubleshooting"></a><span data-ttu-id="32cc0-159">Další informace o řešení</span><span class="sxs-lookup"><span data-stu-id="32cc0-159">Further troubleshooting</span></span>
### <a name="service-fabric-is-not-following-hello-specified-health-policies"></a><span data-ttu-id="32cc0-160">Service Fabric není následující hello zadané zásady stavu</span><span class="sxs-lookup"><span data-stu-id="32cc0-160">Service Fabric is not following hello specified health policies</span></span>
<span data-ttu-id="32cc0-161">Možné příčiny 1:</span><span class="sxs-lookup"><span data-stu-id="32cc0-161">Possible Cause 1:</span></span>

<span data-ttu-id="32cc0-162">Service Fabric překládá všechny procenta do skutečný počet entit (například repliky, oddíly a služby) pro vyhodnocení stavu a vždy zaokrouhlí toowhole entity.</span><span class="sxs-lookup"><span data-stu-id="32cc0-162">Service Fabric translates all percentages into actual numbers of entities (for example, replicas, partitions, and services) for health evaluation and always rounds up toowhole entities.</span></span> <span data-ttu-id="32cc0-163">Například, pokud hello maximální *MaxPercentUnhealthyReplicasPerPartition* je 21 % a existují pět repliky, pak Service Fabric umožňuje tootwo repliky není v pořádku (tedy`Math.Ceiling (5*0.21)`).</span><span class="sxs-lookup"><span data-stu-id="32cc0-163">For example, if hello maximum *MaxPercentUnhealthyReplicasPerPartition* is 21% and there are five replicas, then Service Fabric allows up tootwo unhealthy replicas (that is,`Math.Ceiling (5*0.21)`).</span></span> <span data-ttu-id="32cc0-164">Zásady stavu proto musí být nastaven odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="32cc0-164">Thus, health policies should be set accordingly.</span></span>

<span data-ttu-id="32cc0-165">Možná příčina 2:</span><span class="sxs-lookup"><span data-stu-id="32cc0-165">Possible Cause 2:</span></span>

<span data-ttu-id="32cc0-166">Zásady stavu jsou specifikované jako procenta celkového služby a instance nejsou specifické služby.</span><span class="sxs-lookup"><span data-stu-id="32cc0-166">Health policies are specified in terms of percentages of total services and not specific service instances.</span></span> <span data-ttu-id="32cc0-167">Například před provedením upgradu, pokud aplikace má čtyři služby instancí A, B, C a D, kde je služba D není v pořádku, ale s málo aplikace toohello dopad.</span><span class="sxs-lookup"><span data-stu-id="32cc0-167">For example, before an upgrade, if an application has four service instances A, B, C, and D, where service D is unhealthy but with little impact toohello application.</span></span> <span data-ttu-id="32cc0-168">Chceme tooignore hello známé služby není v pořádku D během upgradu a nastavte hello parametr *MaxPercentUnhealthyServices* toobe 25 %, za předpokladu, že pouze A, B a C potřebovat toobe v pořádku.</span><span class="sxs-lookup"><span data-stu-id="32cc0-168">We want tooignore hello known unhealthy service D during upgrade and set hello parameter *MaxPercentUnhealthyServices* toobe 25%, assuming only A, B, and C need toobe healthy.</span></span>

<span data-ttu-id="32cc0-169">Však během upgradu hello D může se nezotavila během C se změní na není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="32cc0-169">However, during hello upgrade, D may become healthy while C becomes unhealthy.</span></span> <span data-ttu-id="32cc0-170">Hello upgrade by být přesto úspěšné, protože jsou pouze 25 % hello služby není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="32cc0-170">hello upgrade would still succeed because only 25% of hello services are unhealthy.</span></span> <span data-ttu-id="32cc0-171">Však může vést k neočekávané chybám kvůli tooC se neočekávaně není v pořádku místo D. V takovém případě by měl být D modelovaná jako typu různé služby z A a B a c Vzhledem k tomu, že zásady stavu jsou nastaveny na typ služby, může být jiné není v pořádku procento prahové hodnoty použité toodifferent služby.</span><span class="sxs-lookup"><span data-stu-id="32cc0-171">However, it might result in unanticipated errors due tooC being unexpectedly unhealthy instead of D. In this situation, D should be modeled as a different service type from A, B, and C. Since health policies are specified per service type, different unhealthy percentage thresholds can be applied toodifferent services.</span></span> 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-hello-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a><span data-ttu-id="32cc0-172">I nezadali zásady stavu pro upgradu aplikace, ale hello upgrade stále nedaří pro některé vypršení časových limitů, nikdy zadaných</span><span class="sxs-lookup"><span data-stu-id="32cc0-172">I did not specify a health policy for application upgrade, but hello upgrade still fails for some time-outs that I never specified</span></span>
<span data-ttu-id="32cc0-173">Pokud zásady stavu nejsou zadány toohello žádost o upgrade, jsou převzaty z hello *ApplicationManifest.xml* hello aktuální verze aplikace.</span><span class="sxs-lookup"><span data-stu-id="32cc0-173">When health policies aren't provided toohello upgrade request, they are taken from hello *ApplicationManifest.xml* of hello current application version.</span></span> <span data-ttu-id="32cc0-174">Například pokud aplikace X provádíte upgrade z verze 1.0 tooversion 2.0, se používají zásady stavu aplikace podle verze 1.0.</span><span class="sxs-lookup"><span data-stu-id="32cc0-174">For example, if you're upgrading Application X from version 1.0 tooversion 2.0, application health policies specified for in version 1.0 are used.</span></span> <span data-ttu-id="32cc0-175">Pokud se má používat jiný stav zásad pro hello upgrade, zásady hello musí toobe zadaný jako součást upgradu volání rozhraní API hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="32cc0-175">If a different health policy should be used for hello upgrade, then hello policy needs toobe specified as part of hello application upgrade API call.</span></span> <span data-ttu-id="32cc0-176">Hello zásady zadaný jako součást volání hello rozhraní API se projeví pouze během upgradu hello.</span><span class="sxs-lookup"><span data-stu-id="32cc0-176">hello policies specified as part of hello API call only apply during hello upgrade.</span></span> <span data-ttu-id="32cc0-177">Po dokončení upgradu hello hello zásady zadaná v hello *ApplicationManifest.xml* se používají.</span><span class="sxs-lookup"><span data-stu-id="32cc0-177">Once hello upgrade is complete, hello policies specified in hello *ApplicationManifest.xml* are used.</span></span>

### <a name="incorrect-time-outs-are-specified"></a><span data-ttu-id="32cc0-178">Jsou zadány nesprávné vypršení časových limitů</span><span class="sxs-lookup"><span data-stu-id="32cc0-178">Incorrect time-outs are specified</span></span>
<span data-ttu-id="32cc0-179">Vám může mít zajímalo o co se stane, když jsou nekonzistentně nastavit vypršení časových limitů.</span><span class="sxs-lookup"><span data-stu-id="32cc0-179">You may have wondered about what happens when time-outs are set inconsistently.</span></span> <span data-ttu-id="32cc0-180">Například můžete mít *UpgradeTimeout* , je menší než hello *UpgradeDomainTimeout*.</span><span class="sxs-lookup"><span data-stu-id="32cc0-180">For example, you may have an *UpgradeTimeout* that's less than hello *UpgradeDomainTimeout*.</span></span> <span data-ttu-id="32cc0-181">Hello odpověď se vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="32cc0-181">hello answer is that an error is returned.</span></span> <span data-ttu-id="32cc0-182">Chyby se vrátí v případě hello *UpgradeDomainTimeout* je menší než součet hello *HealthCheckWaitDuration* a *HealthCheckRetryTimeout*, nebo pokud  *UpgradeDomainTimeout* je menší než součet hello *HealthCheckWaitDuration* a *HealthCheckStableDuration*.</span><span class="sxs-lookup"><span data-stu-id="32cc0-182">Errors are returned if hello *UpgradeDomainTimeout* is less than hello sum of *HealthCheckWaitDuration* and *HealthCheckRetryTimeout*, or if *UpgradeDomainTimeout* is less than hello sum of *HealthCheckWaitDuration* and *HealthCheckStableDuration*.</span></span>

### <a name="my-upgrades-are-taking-too-long"></a><span data-ttu-id="32cc0-183">Moje upgrady trvá příliš dlouho</span><span class="sxs-lookup"><span data-stu-id="32cc0-183">My upgrades are taking too long</span></span>
<span data-ttu-id="32cc0-184">Hello dobu upgradu toocomplete závisí na kontroly stavu hello a vypršení časových limitů zadán.</span><span class="sxs-lookup"><span data-stu-id="32cc0-184">hello time for an upgrade toocomplete depends on hello health checks and time-outs specified.</span></span> <span data-ttu-id="32cc0-185">Kontroly stavu a vypršení časových limitů závisí na tom, jak dlouho trvalo toocopy, nasazení a stabilizaci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="32cc0-185">Health checks and time-outs depend on how long it takes toocopy, deploy, and stabilize hello application.</span></span> <span data-ttu-id="32cc0-186">Probíhá příliš agresivní s vypršení časových limitů může znamenat více se selháním upgradu, takže doporučujeme můžete začít s delší časové limity.</span><span class="sxs-lookup"><span data-stu-id="32cc0-186">Being too aggressive with time-outs might mean more failed upgrades, so we recommend starting conservatively with longer time-outs.</span></span>

<span data-ttu-id="32cc0-187">Tady je rychlý aktualizačnímu programu na interakci hello vypršení časových limitů s upgradem časy hello:</span><span class="sxs-lookup"><span data-stu-id="32cc0-187">Here's a quick refresher on how hello time-outs interact with hello upgrade times:</span></span>

<span data-ttu-id="32cc0-188">Upgrady pro upgradovací doméně nemůže dokončit rychlejší než *HealthCheckWaitDuration* + *HealthCheckStableDuration*.</span><span class="sxs-lookup"><span data-stu-id="32cc0-188">Upgrades for an upgrade domain cannot complete faster than *HealthCheckWaitDuration* + *HealthCheckStableDuration*.</span></span>

<span data-ttu-id="32cc0-189">Selhání při upgradu nemůže proběhnout, rychlejší než *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.</span><span class="sxs-lookup"><span data-stu-id="32cc0-189">Upgrade failure cannot occur faster than *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.</span></span>

<span data-ttu-id="32cc0-190">Hello doba upgradu upgradu domény je omezena *UpgradeDomainTimeout*.</span><span class="sxs-lookup"><span data-stu-id="32cc0-190">hello upgrade time for an upgrade domain is limited by *UpgradeDomainTimeout*.</span></span>  <span data-ttu-id="32cc0-191">Pokud *HealthCheckRetryTimeout* a *HealthCheckStableDuration* jsou oba nenulové a hello stavu aplikace hello udržuje přepínání a zpět, pak na nakonecvypršíhelloupgradu *UpgradeDomainTimeout*.</span><span class="sxs-lookup"><span data-stu-id="32cc0-191">If *HealthCheckRetryTimeout* and *HealthCheckStableDuration* are both non-zero and hello health of hello application keeps switching back and forth, then hello upgrade eventually times out on *UpgradeDomainTimeout*.</span></span> <span data-ttu-id="32cc0-192">*UpgradeDomainTimeout* spustí jednou odpočítávání hello upgradu pro začne doména upgradu současné hello.</span><span class="sxs-lookup"><span data-stu-id="32cc0-192">*UpgradeDomainTimeout* starts counting down once hello upgrade for hello current upgrade domain begins.</span></span>

## <a name="next-steps"></a><span data-ttu-id="32cc0-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="32cc0-193">Next steps</span></span>
<span data-ttu-id="32cc0-194">[Upgrade vaší aplikace pomocí sady Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="32cc0-194">[Upgrading your Application Using Visual Studio](service-fabric-application-upgrade-tutorial.md) walks you through an application upgrade using Visual Studio.</span></span>

<span data-ttu-id="32cc0-195">[Upgrade vaší aplikace pomocí prostředí Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vás provede upgrade aplikace pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="32cc0-195">[Upgrading your Application Using Powershell](service-fabric-application-upgrade-tutorial-powershell.md) walks you through an application upgrade using PowerShell.</span></span>

<span data-ttu-id="32cc0-196">Řídí, jak vaše aplikace upgraduje pomocí [Upgrade parametry](service-fabric-application-upgrade-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="32cc0-196">Control how your application upgrades by using [Upgrade Parameters](service-fabric-application-upgrade-parameters.md).</span></span>

<span data-ttu-id="32cc0-197">Aby aplikace upgradů kompatibilní metodou učení jak toouse [serializace dat](service-fabric-application-upgrade-data-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="32cc0-197">Make your application upgrades compatible by learning how toouse [Data Serialization](service-fabric-application-upgrade-data-serialization.md).</span></span>

<span data-ttu-id="32cc0-198">Zjistěte, jak toouse pokročilé funkce při upgradu vaší aplikace tím, že odkazuje příliš[Pokročilá témata](service-fabric-application-upgrade-advanced.md).</span><span class="sxs-lookup"><span data-stu-id="32cc0-198">Learn how toouse advanced functionality while upgrading your application by referring too[Advanced Topics](service-fabric-application-upgrade-advanced.md).</span></span>
