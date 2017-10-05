---
title: "Řešení potíží s aplikací upgrady | Microsoft Docs"
description: "Tento článek popisuje některé běžné problémy kolem upgrade aplikace Service Fabric a způsob jejich řešení."
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
ms.openlocfilehash: f7f6bc0c29e2b43fbc8e451c5a4a50110b78349e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-application-upgrades"></a><span data-ttu-id="f3929-103">Řešení potíží s upgrady aplikací</span><span class="sxs-lookup"><span data-stu-id="f3929-103">Troubleshoot application upgrades</span></span>
<span data-ttu-id="f3929-104">Tento článek popisuje některé běžné problémy, týkající se upgradu aplikace Azure Service Fabric a způsob jejich řešení.</span><span class="sxs-lookup"><span data-stu-id="f3929-104">This article covers some of the common issues around upgrading an Azure Service Fabric application and how to resolve them.</span></span>

## <a name="troubleshoot-a-failed-application-upgrade"></a><span data-ttu-id="f3929-105">Řešení potíží při upgradu aplikaci, která selhala</span><span class="sxs-lookup"><span data-stu-id="f3929-105">Troubleshoot a failed application upgrade</span></span>
<span data-ttu-id="f3929-106">Pokud upgrade selže, výstup **Get-ServiceFabricApplicationUpgrade** příkaz obsahuje další informace o ladění selhání.</span><span class="sxs-lookup"><span data-stu-id="f3929-106">When an upgrade fails, the output of the **Get-ServiceFabricApplicationUpgrade** command contains additional information for debugging the failure.</span></span>  <span data-ttu-id="f3929-107">V následujícím seznamu se určuje, jak je možné použít další informace:</span><span class="sxs-lookup"><span data-stu-id="f3929-107">The following list specifies how the additional information can be used:</span></span>

1. <span data-ttu-id="f3929-108">Určete typ chyby.</span><span class="sxs-lookup"><span data-stu-id="f3929-108">Identify the failure type.</span></span>
2. <span data-ttu-id="f3929-109">Určete důvod selhání.</span><span class="sxs-lookup"><span data-stu-id="f3929-109">Identify the failure reason.</span></span>
3. <span data-ttu-id="f3929-110">Izolujte jednu nebo více součástí selhání dále prozkoumat.</span><span class="sxs-lookup"><span data-stu-id="f3929-110">Isolate one or more failing components for further investigation.</span></span>

<span data-ttu-id="f3929-111">Tyto informace jsou k dispozici, když Service Fabric zjistí selhání bez ohledu na to, jestli **FailureAction** se používá k vrácení nebo pozastavit upgradu.</span><span class="sxs-lookup"><span data-stu-id="f3929-111">This information is available when Service Fabric detects the failure regardless of whether the **FailureAction** is to roll back or suspend the upgrade.</span></span>

### <a name="identify-the-failure-type"></a><span data-ttu-id="f3929-112">Identifikujte typ chyby</span><span class="sxs-lookup"><span data-stu-id="f3929-112">Identify the failure type</span></span>
<span data-ttu-id="f3929-113">Ve výstupu **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** identifikuje časové razítko (ve formátu UTC), kdy bylo zjištěno selhání upgradu pomocí Service Fabric a  **FailureAction** byla aktivována.</span><span class="sxs-lookup"><span data-stu-id="f3929-113">In the output of **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** identifies the timestamp (in UTC) at which an upgrade failure was detected by Service Fabric and **FailureAction** was triggered.</span></span> <span data-ttu-id="f3929-114">**Důvod chyby** označuje jeden tři základní příčin selhání:</span><span class="sxs-lookup"><span data-stu-id="f3929-114">**FailureReason** identifies one of three potential high-level causes of the failure:</span></span>

1. <span data-ttu-id="f3929-115">UpgradeDomainTimeout – označuje, že konkrétní upgradu domény trvalo příliš dlouho dokončení a **UpgradeDomainTimeout** vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="f3929-115">UpgradeDomainTimeout - Indicates that a particular upgrade domain took too long to complete and **UpgradeDomainTimeout** expired.</span></span>
2. <span data-ttu-id="f3929-116">OverallUpgradeTimeout – označuje, že celkový upgradu trvalo příliš dlouho dokončení a **UpgradeTimeout** vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="f3929-116">OverallUpgradeTimeout - Indicates that the overall upgrade took too long to complete and **UpgradeTimeout** expired.</span></span>
3. <span data-ttu-id="f3929-117">HealthCheck – označuje, že po dokončení upgradu domény služby aktualizace zůstal není v pořádku podle zásady zadaného stavu aplikace a **HealthCheckRetryTimeout** vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="f3929-117">HealthCheck - Indicates that after upgrading an update domain, the application remained unhealthy according to the specified health policies and **HealthCheckRetryTimeout** expired.</span></span>

<span data-ttu-id="f3929-118">Tyto položky pouze zobrazí ve výstupu se upgrade nezdaří a spustí vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="f3929-118">These entries only show up in the output when the upgrade fails and starts rolling back.</span></span> <span data-ttu-id="f3929-119">Další informace se zobrazí v závislosti na typu selhání.</span><span class="sxs-lookup"><span data-stu-id="f3929-119">Further information is displayed depending on the type of the failure.</span></span>

### <a name="investigate-upgrade-timeouts"></a><span data-ttu-id="f3929-120">Prozkoumat upgradu časové limity</span><span class="sxs-lookup"><span data-stu-id="f3929-120">Investigate upgrade timeouts</span></span>
<span data-ttu-id="f3929-121">Upgrade vypršení časového limitu, selhání jsou nejčastěji způsobeny problémy s dostupností služby.</span><span class="sxs-lookup"><span data-stu-id="f3929-121">Upgrade timeout failures are most commonly caused by service availability issues.</span></span> <span data-ttu-id="f3929-122">Výstup níže je typický pro upgrady, kde služby replik nebo instancí nepodaří spustit v nové verzi kódu.</span><span class="sxs-lookup"><span data-stu-id="f3929-122">The output following this paragraph is typical of upgrades where service replicas or instances fail to start in the new code version.</span></span> <span data-ttu-id="f3929-123">**UpgradeDomainProgressAtFailure** pole zaznamená snímek žádné čekající upgradu práce v době selhání.</span><span class="sxs-lookup"><span data-stu-id="f3929-123">The **UpgradeDomainProgressAtFailure** field captures a snapshot of any pending upgrade work at the time of failure.</span></span>

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

<span data-ttu-id="f3929-124">V tomto příkladu upgrade se nezdařil v doméně pro upgrade *MYUD1* a dva oddíly (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* a *4b43f4d8-b26b-424e-9307-7a7a62e79750*) se zablokuje.</span><span class="sxs-lookup"><span data-stu-id="f3929-124">In this example, the upgrade failed at upgrade domain *MYUD1* and two partitions (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* and *4b43f4d8-b26b-424e-9307-7a7a62e79750*) were stuck.</span></span> <span data-ttu-id="f3929-125">Oddíly, které byly zablokované, protože modul runtime nemohl umístit primární repliky (*WaitForPrimaryPlacement*) na cílové uzly *Uzel1* a *Uzel4*.</span><span class="sxs-lookup"><span data-stu-id="f3929-125">The partitions were stuck because the runtime was unable to place primary replicas (*WaitForPrimaryPlacement*) on target nodes *Node1* and *Node4*.</span></span>

<span data-ttu-id="f3929-126">**Get-ServiceFabricNode** příkaz lze použít k ověření, že jsou tyto dva uzly v doméně pro upgrade *MYUD1*.</span><span class="sxs-lookup"><span data-stu-id="f3929-126">The **Get-ServiceFabricNode** command can be used to verify that these two nodes are in upgrade domain *MYUD1*.</span></span> <span data-ttu-id="f3929-127">*UpgradePhase* uvádí *PostUpgradeSafetyCheck*, což znamená, že tyto kontroly zabezpečení dochází po dokončení upgradu všech uzlech v upgradovací doméně.</span><span class="sxs-lookup"><span data-stu-id="f3929-127">The *UpgradePhase* says *PostUpgradeSafetyCheck*, which means that these safety checks are occurring after all nodes in the upgrade domain have finished upgrading.</span></span> <span data-ttu-id="f3929-128">Tyto informace se odkazuje na potenciální problém s novou verzí kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3929-128">All this information points to a potential issue with the new version of the application code.</span></span> <span data-ttu-id="f3929-129">Nejběžnějším problémům, jsou chyby služby v otevřené nebo povýšení na primární kód cesty.</span><span class="sxs-lookup"><span data-stu-id="f3929-129">The most common issues are service errors in the open or promotion to primary code paths.</span></span>

<span data-ttu-id="f3929-130">*UpgradePhase* z *PreUpgradeSafetyCheck* znamená byly nějaké problémy Příprava upgradu domény předtím, než byla provedena.</span><span class="sxs-lookup"><span data-stu-id="f3929-130">An *UpgradePhase* of *PreUpgradeSafetyCheck* means there were issues preparing the upgrade domain before it was performed.</span></span> <span data-ttu-id="f3929-131">Nejběžnějších problémů v tomto případě jsou chyby služby v zavřít nebo snížení úrovně z cesty primární kódu.</span><span class="sxs-lookup"><span data-stu-id="f3929-131">The most common issues in this case are service errors in the close or demotion from primary code paths.</span></span>

<span data-ttu-id="f3929-132">Aktuální **UpgradeState** je *RollingBackCompleted*, takže původní upgrade musí prováděly s vrácení zpět **FailureAction**, které automaticky nezobrazený Proveďte upgrade při selhání.</span><span class="sxs-lookup"><span data-stu-id="f3929-132">The current **UpgradeState** is *RollingBackCompleted*, so the original upgrade must have been performed with a rollback **FailureAction**, which automatically rolled back the upgrade upon failure.</span></span> <span data-ttu-id="f3929-133">Pokud původní upgradu byla provedena ručního **FailureAction**, pak upgradu by místo toho se v pozastaveném stavu, aby živých ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3929-133">If the original upgrade was performed with a manual **FailureAction**, then the upgrade would instead be in a suspended state to allow live debugging of the application.</span></span>

### <a name="investigate-health-check-failures"></a><span data-ttu-id="f3929-134">Prozkoumat selhání kontroly stavu</span><span class="sxs-lookup"><span data-stu-id="f3929-134">Investigate health check failures</span></span>
<span data-ttu-id="f3929-135">Selhání kontroly stavu mohou být spouštěny různé problémy, které může dojít po všechny uzly v doméně upgradu dokončete upgrade a předání všechny kontroly zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="f3929-135">Health check failures can be triggered by various issues that can happen after all nodes in an upgrade domain finish upgrading and passing all safety checks.</span></span> <span data-ttu-id="f3929-136">Výstup níže je typické pro upgrade nezdaří z důvodu kontroly stavu se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="f3929-136">The output following this paragraph is typical of an upgrade failure due to failed health checks.</span></span> <span data-ttu-id="f3929-137">**UnhealthyEvaluations** pole zaznamená snímek kontroly stavu, které selhaly během upgradu podle zadaného [zásady stavu](service-fabric-health-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f3929-137">The **UnhealthyEvaluations** field captures a snapshot of health checks that failed at the time of the upgrade according to the specified [health policy](service-fabric-health-introduction.md).</span></span>

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

<span data-ttu-id="f3929-138">Příčin selhání kontroly stavu nejprve vyžaduje znalosti o stavu modelu Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="f3929-138">Investigating health check failures first requires an understanding of the Service Fabric health model.</span></span> <span data-ttu-id="f3929-139">Ale i bez takové detailní znalost, uvidíte, že jsou dvě služby není v pořádku: *fabric: / DemoApp/Svc3* a *fabric: / DemoApp/Svc2*, společně s sestav stavu chyby ("InjectedFault" v tomto případě).</span><span class="sxs-lookup"><span data-stu-id="f3929-139">But even without such an in-depth understanding, we can see that two services are unhealthy: *fabric:/DemoApp/Svc3* and *fabric:/DemoApp/Svc2*, along with the error health reports ("InjectedFault" in this case).</span></span> <span data-ttu-id="f3929-140">V tomto příkladu dva ze čtyř jsou služby není v pořádku, což je pod cílovou výchozí hodnotou 0 % není v pořádku (*MaxPercentUnhealthyServices*).</span><span class="sxs-lookup"><span data-stu-id="f3929-140">In this example, two out of four services are unhealthy, which is below the default target of 0% unhealthy (*MaxPercentUnhealthyServices*).</span></span>

<span data-ttu-id="f3929-141">Upgrade zadáním pozastavením při selhání **FailureAction** ruční při spouštění upgradu.</span><span class="sxs-lookup"><span data-stu-id="f3929-141">The upgrade was suspended upon failing by specifying a **FailureAction** of manual when starting the upgrade.</span></span> <span data-ttu-id="f3929-142">Tento režim umožňuje nám prozkoumat systému za provozu v chybovém stavu před provedením žádné další akce.</span><span class="sxs-lookup"><span data-stu-id="f3929-142">This mode allows us to investigate the live system in the failed state before taking any further action.</span></span>

### <a name="recover-from-a-suspended-upgrade"></a><span data-ttu-id="f3929-143">Obnovit pozastavenou upgradem</span><span class="sxs-lookup"><span data-stu-id="f3929-143">Recover from a suspended upgrade</span></span>
<span data-ttu-id="f3929-144">S vrácení zpět **FailureAction**, obnovit potřeby vzhledem k tomu, že upgrade automaticky vrátí zpět při selhání.</span><span class="sxs-lookup"><span data-stu-id="f3929-144">With a rollback **FailureAction**, there is no recovery needed since the upgrade automatically rolls back upon failing.</span></span> <span data-ttu-id="f3929-145">S ruční **FailureAction**, máte několik možností obnovení:</span><span class="sxs-lookup"><span data-stu-id="f3929-145">With a manual **FailureAction**, there are several recovery options:</span></span>

1.  <span data-ttu-id="f3929-146">aktivační událost vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="f3929-146">trigger a rollback</span></span>
2. <span data-ttu-id="f3929-147">Pokračovat ve zbývající části upgrade ručně</span><span class="sxs-lookup"><span data-stu-id="f3929-147">Proceed through the remainder of the upgrade manually</span></span>
3. <span data-ttu-id="f3929-148">Obnovit monitorovaných upgradu</span><span class="sxs-lookup"><span data-stu-id="f3929-148">Resume the monitored upgrade</span></span>

<span data-ttu-id="f3929-149">**Start-ServiceFabricApplicationRollback** příkaz lze použít kdykoli spustit vrácení zpět aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3929-149">The **Start-ServiceFabricApplicationRollback** command can be used at any time to start rolling back the application.</span></span> <span data-ttu-id="f3929-150">Jakmile příkaz vrátí úspěšně, žádost o vrácení zpět je zaregistrován v systému a spustí krátce po tomto datu.</span><span class="sxs-lookup"><span data-stu-id="f3929-150">Once the command returns successfully, the rollback request has been registered in the system and starts shortly thereafter.</span></span>

<span data-ttu-id="f3929-151">**Resume-ServiceFabricApplicationUpgrade** příkaz lze použít, chcete-li pokračovat ve zbývající části upgrade ručně, jednu upgradovací doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="f3929-151">The **Resume-ServiceFabricApplicationUpgrade** command can be used to proceed through the remainder of the upgrade manually, one upgrade domain at a time.</span></span> <span data-ttu-id="f3929-152">V tomto režimu budou provedeny kontroly zabezpečení pouze v systému.</span><span class="sxs-lookup"><span data-stu-id="f3929-152">In this mode, only safety checks are performed by the system.</span></span> <span data-ttu-id="f3929-153">Budou provedeny žádné další kontroly stavu.</span><span class="sxs-lookup"><span data-stu-id="f3929-153">No more health checks are performed.</span></span> <span data-ttu-id="f3929-154">Tento příkaz lze použít, když *UpgradeState* ukazuje *RollingForwardPending*, což znamená, že doména upgradu současné dokončil upgrade ale dalšímu nebyla spuštěna (čeká na zpracování).</span><span class="sxs-lookup"><span data-stu-id="f3929-154">This command can only be used when the *UpgradeState* shows *RollingForwardPending*, which means that the current upgrade domain has finished upgrading but the next one has not started (pending).</span></span>

<span data-ttu-id="f3929-155">**Aktualizace ServiceFabricApplicationUpgrade** příkaz lze použít obnovit monitorovaných upgrade s obou zabezpečení a kontroluje stav provádí.</span><span class="sxs-lookup"><span data-stu-id="f3929-155">The **Update-ServiceFabricApplicationUpgrade** command can be used to resume the monitored upgrade with both safety and health checks being performed.</span></span>

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

<span data-ttu-id="f3929-156">Z upgradovací doméně, kde byl poslední pozastavený a použijete stejný upgrade parametry a zásad stavu jako před bude pokračovat v upgradu.</span><span class="sxs-lookup"><span data-stu-id="f3929-156">The upgrade continues from the upgrade domain where it was last suspended and use the same upgrade parameters and health policies as before.</span></span> <span data-ttu-id="f3929-157">V případě potřeby jakýkoli z upgradu parametry a zásady stavu zobrazené ve výstupu předchozí lze změnit v jednom příkazu při upgradu obnoví.</span><span class="sxs-lookup"><span data-stu-id="f3929-157">If needed, any of the upgrade parameters and health policies shown in the preceding output can be changed in the same command when the upgrade resumes.</span></span> <span data-ttu-id="f3929-158">V tomto příkladu bylo obnoveno upgradu v režimu monitorované s parametry a zásad stavu beze změny.</span><span class="sxs-lookup"><span data-stu-id="f3929-158">In this example, the upgrade was resumed in Monitored mode, with the parameters and the health policies unchanged.</span></span>

## <a name="further-troubleshooting"></a><span data-ttu-id="f3929-159">Další informace o řešení</span><span class="sxs-lookup"><span data-stu-id="f3929-159">Further troubleshooting</span></span>
### <a name="service-fabric-is-not-following-the-specified-health-policies"></a><span data-ttu-id="f3929-160">Service Fabric není následující zásady zadaného stavu</span><span class="sxs-lookup"><span data-stu-id="f3929-160">Service Fabric is not following the specified health policies</span></span>
<span data-ttu-id="f3929-161">Možné příčiny 1:</span><span class="sxs-lookup"><span data-stu-id="f3929-161">Possible Cause 1:</span></span>

<span data-ttu-id="f3929-162">Service Fabric překládá všechny procenta do skutečný počet entit (například repliky, oddíly a služby) pro vyhodnocení stavu a vždy se zaokrouhlí na celý entity.</span><span class="sxs-lookup"><span data-stu-id="f3929-162">Service Fabric translates all percentages into actual numbers of entities (for example, replicas, partitions, and services) for health evaluation and always rounds up to whole entities.</span></span> <span data-ttu-id="f3929-163">Například pokud maximální *MaxPercentUnhealthyReplicasPerPartition* je 21 % a existují pět repliky, pak Service Fabric umožňuje až dvě repliky není v pořádku (tedy`Math.Ceiling (5*0.21)`).</span><span class="sxs-lookup"><span data-stu-id="f3929-163">For example, if the maximum *MaxPercentUnhealthyReplicasPerPartition* is 21% and there are five replicas, then Service Fabric allows up to two unhealthy replicas (that is,`Math.Ceiling (5*0.21)`).</span></span> <span data-ttu-id="f3929-164">Zásady stavu proto musí být nastaven odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="f3929-164">Thus, health policies should be set accordingly.</span></span>

<span data-ttu-id="f3929-165">Možná příčina 2:</span><span class="sxs-lookup"><span data-stu-id="f3929-165">Possible Cause 2:</span></span>

<span data-ttu-id="f3929-166">Zásady stavu jsou specifikované jako procenta celkového služby a instance nejsou specifické služby.</span><span class="sxs-lookup"><span data-stu-id="f3929-166">Health policies are specified in terms of percentages of total services and not specific service instances.</span></span> <span data-ttu-id="f3929-167">Například před provedením upgradu, pokud aplikace má čtyři služby instancí A, B, C a D, kde je služba D není v pořádku, ale s malý vliv k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f3929-167">For example, before an upgrade, if an application has four service instances A, B, C, and D, where service D is unhealthy but with little impact to the application.</span></span> <span data-ttu-id="f3929-168">Chceme ignorovat známé služby není v pořádku D během upgradu a nastavte parametr *MaxPercentUnhealthyServices* být 25 %, za předpokladu, že pouze A a B a C musí být v pořádku.</span><span class="sxs-lookup"><span data-stu-id="f3929-168">We want to ignore the known unhealthy service D during upgrade and set the parameter *MaxPercentUnhealthyServices* to be 25%, assuming only A, B, and C need to be healthy.</span></span>

<span data-ttu-id="f3929-169">Však během upgradu D může se nezotavila během C se změní na není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="f3929-169">However, during the upgrade, D may become healthy while C becomes unhealthy.</span></span> <span data-ttu-id="f3929-170">Upgrade by být přesto úspěšné, protože jsou pouze 25 % služby není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="f3929-170">The upgrade would still succeed because only 25% of the services are unhealthy.</span></span> <span data-ttu-id="f3929-171">Však může vést k neočekávané chybám kvůli C se neočekávaně není v pořádku místo D. V takovém případě by měl být D modelovaná jako typu různé služby z A a B a c Vzhledem k tomu, že zásady stavu jsou nastaveny na typ služby, jiný není v pořádku procento prahové hodnoty je použít pro různé služby.</span><span class="sxs-lookup"><span data-stu-id="f3929-171">However, it might result in unanticipated errors due to C being unexpectedly unhealthy instead of D. In this situation, D should be modeled as a different service type from A, B, and C. Since health policies are specified per service type, different unhealthy percentage thresholds can be applied to different services.</span></span> 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a><span data-ttu-id="f3929-172">I nezadali zásady stavu pro upgradu aplikace, ale pro některé vypršení časových limitů, nikdy zadaných stále selhání upgradu</span><span class="sxs-lookup"><span data-stu-id="f3929-172">I did not specify a health policy for application upgrade, but the upgrade still fails for some time-outs that I never specified</span></span>
<span data-ttu-id="f3929-173">Pokud zásady stavu nejsou zadaný pro žádost o upgrade, jsou převzaty z *ApplicationManifest.xml* aktuální verze aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3929-173">When health policies aren't provided to the upgrade request, they are taken from the *ApplicationManifest.xml* of the current application version.</span></span> <span data-ttu-id="f3929-174">Například pokud upgradujete z verze 1.0 na verzi 2.0, zásady stavu aplikace uvedených v verze 1.0 pro aplikace X používají.</span><span class="sxs-lookup"><span data-stu-id="f3929-174">For example, if you're upgrading Application X from version 1.0 to version 2.0, application health policies specified for in version 1.0 are used.</span></span> <span data-ttu-id="f3929-175">Pokud chcete zásadu jiný stav upgradu, zásady musí být zadaný jako součást upgradu volání rozhraní API aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3929-175">If a different health policy should be used for the upgrade, then the policy needs to be specified as part of the application upgrade API call.</span></span> <span data-ttu-id="f3929-176">Zásady, zadaný jako součást volání rozhraní API se uplatní jenom během upgradu.</span><span class="sxs-lookup"><span data-stu-id="f3929-176">The policies specified as part of the API call only apply during the upgrade.</span></span> <span data-ttu-id="f3929-177">Po dokončení upgradu zásady zadaná v *ApplicationManifest.xml* se používají.</span><span class="sxs-lookup"><span data-stu-id="f3929-177">Once the upgrade is complete, the policies specified in the *ApplicationManifest.xml* are used.</span></span>

### <a name="incorrect-time-outs-are-specified"></a><span data-ttu-id="f3929-178">Jsou zadány nesprávné vypršení časových limitů</span><span class="sxs-lookup"><span data-stu-id="f3929-178">Incorrect time-outs are specified</span></span>
<span data-ttu-id="f3929-179">Vám může mít zajímalo o co se stane, když jsou nekonzistentně nastavit vypršení časových limitů.</span><span class="sxs-lookup"><span data-stu-id="f3929-179">You may have wondered about what happens when time-outs are set inconsistently.</span></span> <span data-ttu-id="f3929-180">Například můžete mít *UpgradeTimeout* to je méně než *UpgradeDomainTimeout*.</span><span class="sxs-lookup"><span data-stu-id="f3929-180">For example, you may have an *UpgradeTimeout* that's less than the *UpgradeDomainTimeout*.</span></span> <span data-ttu-id="f3929-181">Odpověď se vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="f3929-181">The answer is that an error is returned.</span></span> <span data-ttu-id="f3929-182">Chyby se vrátí v případě *UpgradeDomainTimeout* je menší než součet *HealthCheckWaitDuration* a *HealthCheckRetryTimeout*, nebo pokud  *UpgradeDomainTimeout* je menší než součet *HealthCheckWaitDuration* a *HealthCheckStableDuration*.</span><span class="sxs-lookup"><span data-stu-id="f3929-182">Errors are returned if the *UpgradeDomainTimeout* is less than the sum of *HealthCheckWaitDuration* and *HealthCheckRetryTimeout*, or if *UpgradeDomainTimeout* is less than the sum of *HealthCheckWaitDuration* and *HealthCheckStableDuration*.</span></span>

### <a name="my-upgrades-are-taking-too-long"></a><span data-ttu-id="f3929-183">Moje upgrady trvá příliš dlouho</span><span class="sxs-lookup"><span data-stu-id="f3929-183">My upgrades are taking too long</span></span>
<span data-ttu-id="f3929-184">Doba pro upgrade k dokončení závisí na kontroly stavu a vypršení časových limitů zadán.</span><span class="sxs-lookup"><span data-stu-id="f3929-184">The time for an upgrade to complete depends on the health checks and time-outs specified.</span></span> <span data-ttu-id="f3929-185">Kontroly stavu a vypršení časových limitů závisí na tom, jak dlouho trvá kopírování, nasazení a stabilizaci aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3929-185">Health checks and time-outs depend on how long it takes to copy, deploy, and stabilize the application.</span></span> <span data-ttu-id="f3929-186">Probíhá příliš agresivní s vypršení časových limitů může znamenat více se selháním upgradu, takže doporučujeme můžete začít s delší časové limity.</span><span class="sxs-lookup"><span data-stu-id="f3929-186">Being too aggressive with time-outs might mean more failed upgrades, so we recommend starting conservatively with longer time-outs.</span></span>

<span data-ttu-id="f3929-187">Tady je rychlý aktualizačnímu programu na způsob, jakým vypršení časových limitů komunikovat s upgradem časy:</span><span class="sxs-lookup"><span data-stu-id="f3929-187">Here's a quick refresher on how the time-outs interact with the upgrade times:</span></span>

<span data-ttu-id="f3929-188">Upgrady pro upgradovací doméně nemůže dokončit rychlejší než *HealthCheckWaitDuration* + *HealthCheckStableDuration*.</span><span class="sxs-lookup"><span data-stu-id="f3929-188">Upgrades for an upgrade domain cannot complete faster than *HealthCheckWaitDuration* + *HealthCheckStableDuration*.</span></span>

<span data-ttu-id="f3929-189">Selhání při upgradu nemůže proběhnout, rychlejší než *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.</span><span class="sxs-lookup"><span data-stu-id="f3929-189">Upgrade failure cannot occur faster than *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.</span></span>

<span data-ttu-id="f3929-190">Doba upgradu upgradu domény je omezena *UpgradeDomainTimeout*.</span><span class="sxs-lookup"><span data-stu-id="f3929-190">The upgrade time for an upgrade domain is limited by *UpgradeDomainTimeout*.</span></span>  <span data-ttu-id="f3929-191">Pokud *HealthCheckRetryTimeout* a *HealthCheckStableDuration* jsou oba nenulové a stavu aplikace udržuje přepínání a zpět, a nakonec vyprší upgrade na *UpgradeDomainTimeout*.</span><span class="sxs-lookup"><span data-stu-id="f3929-191">If *HealthCheckRetryTimeout* and *HealthCheckStableDuration* are both non-zero and the health of the application keeps switching back and forth, then the upgrade eventually times out on *UpgradeDomainTimeout*.</span></span> <span data-ttu-id="f3929-192">*UpgradeDomainTimeout* spustí počítání dolů jednou upgradu pro začne aktuální upgradovací doméně.</span><span class="sxs-lookup"><span data-stu-id="f3929-192">*UpgradeDomainTimeout* starts counting down once the upgrade for the current upgrade domain begins.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3929-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3929-193">Next steps</span></span>
<span data-ttu-id="f3929-194">[Upgrade vaší aplikace pomocí sady Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f3929-194">[Upgrading your Application Using Visual Studio](service-fabric-application-upgrade-tutorial.md) walks you through an application upgrade using Visual Studio.</span></span>

<span data-ttu-id="f3929-195">[Upgrade vaší aplikace pomocí prostředí Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vás provede upgrade aplikace pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f3929-195">[Upgrading your Application Using Powershell](service-fabric-application-upgrade-tutorial-powershell.md) walks you through an application upgrade using PowerShell.</span></span>

<span data-ttu-id="f3929-196">Řídí, jak vaše aplikace upgraduje pomocí [Upgrade parametry](service-fabric-application-upgrade-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="f3929-196">Control how your application upgrades by using [Upgrade Parameters](service-fabric-application-upgrade-parameters.md).</span></span>

<span data-ttu-id="f3929-197">Zkontrolujte upgradů aplikace kompatibilní podle naučit se používat [serializace dat](service-fabric-application-upgrade-data-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="f3929-197">Make your application upgrades compatible by learning how to use [Data Serialization](service-fabric-application-upgrade-data-serialization.md).</span></span>

<span data-ttu-id="f3929-198">Další informace o použití pokročilých funkcí při upgradu vaší aplikace tím, že odkazuje na [Pokročilá témata](service-fabric-application-upgrade-advanced.md).</span><span class="sxs-lookup"><span data-stu-id="f3929-198">Learn how to use advanced functionality while upgrading your application by referring to [Advanced Topics](service-fabric-application-upgrade-advanced.md).</span></span>
