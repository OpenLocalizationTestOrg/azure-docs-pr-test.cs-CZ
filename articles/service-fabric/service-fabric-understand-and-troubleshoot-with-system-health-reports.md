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
# <a name="use-system-health-reports-to-troubleshoot"></a><span data-ttu-id="ec702-103">Řešení problémů pomocí sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="ec702-103">Use system health reports to troubleshoot</span></span>
<span data-ttu-id="ec702-104">Azure Service Fabric součásti sestavy mimo pole na všech entit v clusteru.</span><span class="sxs-lookup"><span data-stu-id="ec702-104">Azure Service Fabric components report out of the box on all entities in the cluster.</span></span> <span data-ttu-id="ec702-105">[Úložiště stavu](service-fabric-health-introduction.md#health-store) vytvoří nebo odstraní entit na základě sestav systému.</span><span class="sxs-lookup"><span data-stu-id="ec702-105">The [health store](service-fabric-health-introduction.md#health-store) creates and deletes entities based on the system reports.</span></span> <span data-ttu-id="ec702-106">Také slouží k uspořádání je v hierarchii, která zaznamená interakce entity.</span><span class="sxs-lookup"><span data-stu-id="ec702-106">It also organizes them in a hierarchy that captures entity interactions.</span></span>

> [!NOTE]
> <span data-ttu-id="ec702-107">Abyste pochopili související se stavem koncepty, další informace v [modelu stavu Service Fabric](service-fabric-health-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ec702-107">To understand health-related concepts, read more at [Service Fabric health model](service-fabric-health-introduction.md).</span></span>
> 
> 

<span data-ttu-id="ec702-108">Sestav o stavu systému poskytují přehled o clusteru a příznak problémy prostřednictvím stavu a funkce aplikací.</span><span class="sxs-lookup"><span data-stu-id="ec702-108">System health reports provide visibility into cluster and application functionality and flag issues through health.</span></span> <span data-ttu-id="ec702-109">Pro aplikace a služby ověřte sestav o stavu systému entity jsou implementované a chovají správně z hlediska Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="ec702-109">For applications and services, system health reports verify that entities are implemented and are behaving correctly from the Service Fabric perspective.</span></span> <span data-ttu-id="ec702-110">Sestavy neposkytují žádné sledování stavu obchodní logiky služby nebo zjišťování "zamrzlých" procesů.</span><span class="sxs-lookup"><span data-stu-id="ec702-110">The reports do not provide any health monitoring of the business logic of the service or detection of hung processes.</span></span> <span data-ttu-id="ec702-111">Uživatel služby lze rozšířit údaje o stavu informace specifické pro jejich logiku.</span><span class="sxs-lookup"><span data-stu-id="ec702-111">User services can enrich the health data with information specific to their logic.</span></span>

> [!NOTE]
> <span data-ttu-id="ec702-112">Sestavy stavu watchdogs jsou viditelné pouze *po* součástech systému vytvořte entitu.</span><span class="sxs-lookup"><span data-stu-id="ec702-112">Watchdogs health reports are visible only *after* the system components create an entity.</span></span> <span data-ttu-id="ec702-113">Při odstranění entity úložiště zdravotní automaticky odstraní všechny sestavy stavu s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="ec702-113">When an entity is deleted, the health store automatically deletes all health reports associated with it.</span></span> <span data-ttu-id="ec702-114">Stejné hodnotu true, pokud je vytvořena nová instance entity (například je vytvořena nová instance repliky stavové trvalou služby).</span><span class="sxs-lookup"><span data-stu-id="ec702-114">The same is true when a new instance of the entity is created (for example, a new stateful persisted service replica instance is created).</span></span> <span data-ttu-id="ec702-115">Všechny sestavy přidružené k původní instanci jsou odstraněna a vyčistit z úložiště.</span><span class="sxs-lookup"><span data-stu-id="ec702-115">All reports associated with the old instance are deleted and cleaned up from the store.</span></span>
> 
> 

<span data-ttu-id="ec702-116">Součást systému sestavy, jsou identifikovány zdroj, který začíná "**systému.**"</span><span class="sxs-lookup"><span data-stu-id="ec702-116">The system component reports are identified by the source, which starts with the "**System.**"</span></span> <span data-ttu-id="ec702-117">Předpona.</span><span class="sxs-lookup"><span data-stu-id="ec702-117">prefix.</span></span> <span data-ttu-id="ec702-118">Watchdogs nelze používat stejnou předponu pro jejich zdroje, jako jsou odmítnuta sestavy se neplatné parametry.</span><span class="sxs-lookup"><span data-stu-id="ec702-118">Watchdogs can't use the same prefix for their sources, as reports with invalid parameters are rejected.</span></span>
<span data-ttu-id="ec702-119">Podívejme se na některé sestavy systému pochopit, co je aktivuje a jak chybu opravit možné problémy, které reprezentují.</span><span class="sxs-lookup"><span data-stu-id="ec702-119">Let's look at some system reports to understand what triggers them and how to correct the possible issues they represent.</span></span>

> [!NOTE]
> <span data-ttu-id="ec702-120">Service Fabric i nadále přidat sestavy týkající se podmínek, které vylepšují získat přehled o dění v clusteru a aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec702-120">Service Fabric continues to add reports on conditions of interest that improve visibility into what is happening in the cluster and application.</span></span> <span data-ttu-id="ec702-121">Existující sestavy lze také rozšířit o další podrobnosti k řešení problému rychlejší.</span><span class="sxs-lookup"><span data-stu-id="ec702-121">Existing reports can also be enhanced with more details to help troubleshoot the problem faster.</span></span>
> 
> 

## <a name="cluster-system-health-reports"></a><span data-ttu-id="ec702-122">Cluster sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="ec702-122">Cluster system health reports</span></span>
<span data-ttu-id="ec702-123">Stav entity clusteru se vytvoří automaticky v health store.</span><span class="sxs-lookup"><span data-stu-id="ec702-123">The cluster health entity is created automatically in the health store.</span></span> <span data-ttu-id="ec702-124">Pokud všechno funguje správně, nemá sestavu system.</span><span class="sxs-lookup"><span data-stu-id="ec702-124">If everything works properly, it doesn't have a system report.</span></span>

### <a name="neighborhood-loss"></a><span data-ttu-id="ec702-125">Ztráta Okolní počítače</span><span class="sxs-lookup"><span data-stu-id="ec702-125">Neighborhood loss</span></span>
<span data-ttu-id="ec702-126">**System.Federation** nahlásí chybu, když zjistí ztrátu okolí.</span><span class="sxs-lookup"><span data-stu-id="ec702-126">**System.Federation** reports an error when it detects a neighborhood loss.</span></span> <span data-ttu-id="ec702-127">Sestava je z jednotlivých uzlů a ID uzlu je součástí názvu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="ec702-127">The report is from individual nodes, and the node ID is included in the property name.</span></span> <span data-ttu-id="ec702-128">Pokud je jeden okolí ztratili v celé prstenec Service Fabric, můžete očekávat obvykle dvě události (obou stranách sestavy mezera).</span><span class="sxs-lookup"><span data-stu-id="ec702-128">If one neighborhood is lost in the entire Service Fabric ring, you can typically expect two events (both sides of the gap report).</span></span> <span data-ttu-id="ec702-129">Pokud další sousedství jsou ztraceny, existují další události.</span><span class="sxs-lookup"><span data-stu-id="ec702-129">If more neighborhoods are lost, there are more events.</span></span>

<span data-ttu-id="ec702-130">Sestava Určuje časový limit zapůjčení globální jako TTL.</span><span class="sxs-lookup"><span data-stu-id="ec702-130">The report specifies the global lease timeout as the time to live.</span></span> <span data-ttu-id="ec702-131">Sestava je nutno každých poloviny doby trvání TTL pro podmínku zůstává aktivní.</span><span class="sxs-lookup"><span data-stu-id="ec702-131">The report is resent every half of the TTL duration for as long as the condition remains active.</span></span> <span data-ttu-id="ec702-132">Událost je automaticky odstraněna po jeho vypršení.</span><span class="sxs-lookup"><span data-stu-id="ec702-132">The event is automatically removed when it expires.</span></span> <span data-ttu-id="ec702-133">Odeberte, pokud vypršela platnost zaručuje, že sestava je vyčištěna z health store správně, přestože uzlu vytváření sestav je vypnutý.</span><span class="sxs-lookup"><span data-stu-id="ec702-133">Remove when expired behavior ensures that the report is cleaned up from the health store correctly, even if the reporting node is down.</span></span>

* <span data-ttu-id="ec702-134">**SourceId**: System.Federation</span><span class="sxs-lookup"><span data-stu-id="ec702-134">**SourceId**: System.Federation</span></span>
* <span data-ttu-id="ec702-135">**Vlastnost**: začíná **okolí** a obsahuje informace o uzlu</span><span class="sxs-lookup"><span data-stu-id="ec702-135">**Property**: Starts with **Neighborhood** and includes node information</span></span>
* <span data-ttu-id="ec702-136">**Další kroky**: Zjistěte, proč dojde ke ztrátě okolí (například zkontrolovat komunikaci mezi uzly clusteru).</span><span class="sxs-lookup"><span data-stu-id="ec702-136">**Next steps**: Investigate why the neighborhood is lost (for example, check the communication between cluster nodes).</span></span>

## <a name="node-system-health-reports"></a><span data-ttu-id="ec702-137">Uzel sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="ec702-137">Node system health reports</span></span>
<span data-ttu-id="ec702-138">**System.FM**, který představuje službu Failover Manager je autority, který spravuje informace o uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="ec702-138">**System.FM**, which represents the Failover Manager service, is the authority that manages information about cluster nodes.</span></span> <span data-ttu-id="ec702-139">Každý uzel musí mít jednu sestavu z System.FM zobrazuje stav.</span><span class="sxs-lookup"><span data-stu-id="ec702-139">Each node should have one report from System.FM showing its state.</span></span> <span data-ttu-id="ec702-140">Uzel entity, které se odeberou, když se odebere stav uzlu (viz [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).</span><span class="sxs-lookup"><span data-stu-id="ec702-140">The node entities are removed when the node state is removed (see [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).</span></span>

### <a name="node-updown"></a><span data-ttu-id="ec702-141">Uzel nahoru/dolů</span><span class="sxs-lookup"><span data-stu-id="ec702-141">Node up/down</span></span>
<span data-ttu-id="ec702-142">System.FM nahlásí jako OK, když se uzel připojí prstenec (je spuštěná).</span><span class="sxs-lookup"><span data-stu-id="ec702-142">System.FM reports as OK when the node joins the ring (it's up and running).</span></span> <span data-ttu-id="ec702-143">Nahlásí chybu, pokud uzel vyplouvající řetězci (je vypnutý, buď pro upgrade nebo jednoduše protože se nezdařilo).</span><span class="sxs-lookup"><span data-stu-id="ec702-143">It reports an error when the node departs the ring (it's down, either for upgrading or simply because it has failed).</span></span> <span data-ttu-id="ec702-144">Stav hierarchie sestavena úložiště zdravotní provádět akce se nasazené entity v korelace s System.FM sestavy uzlu.</span><span class="sxs-lookup"><span data-stu-id="ec702-144">The health hierarchy built by the health store takes action on deployed entities in correlation with System.FM node reports.</span></span> <span data-ttu-id="ec702-145">Nadřazený virtuální všechny nasazené entit považuje uzlu.</span><span class="sxs-lookup"><span data-stu-id="ec702-145">It considers the node a virtual parent of all deployed entities.</span></span> <span data-ttu-id="ec702-146">Nasazené entit na tomto uzlu se zveřejňují přes dotazy, pokud uzel je hlášen jako až podle System.FM s stejnou instanci jako instanci spojenou s entity.</span><span class="sxs-lookup"><span data-stu-id="ec702-146">The deployed entities on that node are exposed through queries if the node is reported as up by System.FM, with the same instance as the instance associated with the entities.</span></span> <span data-ttu-id="ec702-147">Když System.FM ohlásí, že uzel je mimo provoz nebo restartovat (nové instance), úložiště zdravotní automaticky vyčistí nasazené entitami, které může existovat jenom v uzlu dolů nebo na předchozí instanci uzlu.</span><span class="sxs-lookup"><span data-stu-id="ec702-147">When System.FM reports that the node is down or restarted (a new instance), the health store automatically cleans up the deployed entities that can exist only on the down node or on the previous instance of the node.</span></span>

* <span data-ttu-id="ec702-148">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="ec702-148">**SourceId**: System.FM</span></span>
* <span data-ttu-id="ec702-149">**Vlastnost**: stavu</span><span class="sxs-lookup"><span data-stu-id="ec702-149">**Property**: State</span></span>
* <span data-ttu-id="ec702-150">**Další kroky**: Pokud je uzel dolů k upgradu by měl mít zpět po byl upgradován.</span><span class="sxs-lookup"><span data-stu-id="ec702-150">**Next steps**: If the node is down for an upgrade, it should come back up once it has been upgraded.</span></span> <span data-ttu-id="ec702-151">V takovém případě by měl stav přepněte zpět na OK.</span><span class="sxs-lookup"><span data-stu-id="ec702-151">In this case, the health state should switch back to OK.</span></span> <span data-ttu-id="ec702-152">Pokud uzel nemá vraťte nebo se nezdaří, problém potřebuje další šetření.</span><span class="sxs-lookup"><span data-stu-id="ec702-152">If the node doesn't come back or it fails, the problem needs more investigation.</span></span>

<span data-ttu-id="ec702-153">Následující příklad ukazuje System.FM události se stavem stavu OK pro uzel:</span><span class="sxs-lookup"><span data-stu-id="ec702-153">The following example shows the System.FM event with a health state of OK for node up:</span></span>

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


### <a name="certificate-expiration"></a><span data-ttu-id="ec702-154">Vypršení platnosti certifikátu</span><span class="sxs-lookup"><span data-stu-id="ec702-154">Certificate expiration</span></span>
<span data-ttu-id="ec702-155">**System.FabricNode** sestavy upozornění, když se certifikáty používané uzlu blíží vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="ec702-155">**System.FabricNode** reports a warning when certificates used by the node are near expiration.</span></span> <span data-ttu-id="ec702-156">Existují tři certifikáty na uzel: **Certificate_cluster**, **Certificate_server**, a **Certificate_default_client**.</span><span class="sxs-lookup"><span data-stu-id="ec702-156">There are three certificates per node: **Certificate_cluster**, **Certificate_server**, and **Certificate_default_client**.</span></span> <span data-ttu-id="ec702-157">Při vypršení je alespoň dva týdny, je sestava stavu v pořádku.</span><span class="sxs-lookup"><span data-stu-id="ec702-157">When the expiration is at least two weeks away, the report health state is OK.</span></span> <span data-ttu-id="ec702-158">Pokud doba vypršení platnosti je během dvou týdnů, typ sestavy je upozornění.</span><span class="sxs-lookup"><span data-stu-id="ec702-158">When the expiration is within two weeks, the report type is a warning.</span></span> <span data-ttu-id="ec702-159">Hodnota TTL z těchto událostí je nekonečno, a budou odstraněny Jestliže uzel opustí clusteru.</span><span class="sxs-lookup"><span data-stu-id="ec702-159">TTL of these events is infinite, and they are removed when a node leaves the cluster.</span></span>

* <span data-ttu-id="ec702-160">**SourceId**: System.FabricNode</span><span class="sxs-lookup"><span data-stu-id="ec702-160">**SourceId**: System.FabricNode</span></span>
* <span data-ttu-id="ec702-161">**Vlastnost**: začíná **certifikát** a obsahuje další informace o typ certifikátu</span><span class="sxs-lookup"><span data-stu-id="ec702-161">**Property**: Starts with **Certificate** and contains more information about the certificate type</span></span>
* <span data-ttu-id="ec702-162">**Další kroky**: aktualizovat certifikáty, pokud se blíží vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="ec702-162">**Next steps**: Update the certificates if they are near expiration.</span></span>

### <a name="load-capacity-violation"></a><span data-ttu-id="ec702-163">Načíst porušení kapacity</span><span class="sxs-lookup"><span data-stu-id="ec702-163">Load capacity violation</span></span>
<span data-ttu-id="ec702-164">Vyrovnávání zatížení Service Fabric hlásí upozornění, když zjistí porušení kapacity uzlu.</span><span class="sxs-lookup"><span data-stu-id="ec702-164">The Service Fabric Load Balancer reports a warning when it detects a node capacity violation.</span></span>

* <span data-ttu-id="ec702-165">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="ec702-165">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="ec702-166">**Vlastnost**: začíná **kapacity**</span><span class="sxs-lookup"><span data-stu-id="ec702-166">**Property**: Starts with **Capacity**</span></span>
* <span data-ttu-id="ec702-167">**Další kroky**: Zkontrolujte zadaný metriky a zobrazit aktuální kapacitu na uzlu.</span><span class="sxs-lookup"><span data-stu-id="ec702-167">**Next steps**: Check provided metrics and view the current capacity on the node.</span></span>

## <a name="application-system-health-reports"></a><span data-ttu-id="ec702-168">Aplikace sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="ec702-168">Application system health reports</span></span>
<span data-ttu-id="ec702-169">**System.CM**, který představuje službu Správce clusteru, je že úřad, který spravuje informace o aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ec702-169">**System.CM**, which represents the Cluster Manager service, is the authority that manages information about an application.</span></span>

### <a name="state"></a><span data-ttu-id="ec702-170">Stav</span><span class="sxs-lookup"><span data-stu-id="ec702-170">State</span></span>
<span data-ttu-id="ec702-171">System.CM nahlásí jako OK když aplikace byly vytvořeny nebo aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="ec702-171">System.CM reports as OK when the application has been created or updated.</span></span> <span data-ttu-id="ec702-172">Informuje úložiště zdravotní po odstranění aplikace tak, aby bylo možné odebrat z úložiště.</span><span class="sxs-lookup"><span data-stu-id="ec702-172">It informs the health store when the application has been deleted, so that it can be removed from store.</span></span>

* <span data-ttu-id="ec702-173">**SourceId**: System.CM</span><span class="sxs-lookup"><span data-stu-id="ec702-173">**SourceId**: System.CM</span></span>
* <span data-ttu-id="ec702-174">**Vlastnost**: stavu</span><span class="sxs-lookup"><span data-stu-id="ec702-174">**Property**: State</span></span>
* <span data-ttu-id="ec702-175">**Další kroky**: Pokud aplikace má byly vytvořeny nebo aktualizovány, měl by obsahovat sestava stavu Správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="ec702-175">**Next steps**: If the application has been created or updated, it should include the Cluster Manager health report.</span></span> <span data-ttu-id="ec702-176">Jinak, zkontrolujte stav aplikace vydáním dotazu (například rutinu prostředí PowerShell **Get ServiceFabricApplication - ApplicationName *applicationName***).</span><span class="sxs-lookup"><span data-stu-id="ec702-176">Otherwise, check the state of the application by issuing a query (for example, the PowerShell cmdlet **Get-ServiceFabricApplication -ApplicationName *applicationName***).</span></span>

<span data-ttu-id="ec702-177">Následující příklad ukazuje události stavu na **fabric: / WordCount** aplikace:</span><span class="sxs-lookup"><span data-stu-id="ec702-177">The following example shows the state event on the **fabric:/WordCount** application:</span></span>

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

## <a name="service-system-health-reports"></a><span data-ttu-id="ec702-178">Služba sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="ec702-178">Service system health reports</span></span>
<span data-ttu-id="ec702-179">**System.FM**, který představuje službu Failover Manager je autority, který spravuje informace o službách.</span><span class="sxs-lookup"><span data-stu-id="ec702-179">**System.FM**, which represents the Failover Manager service, is the authority that manages information about services.</span></span>

### <a name="state"></a><span data-ttu-id="ec702-180">Stav</span><span class="sxs-lookup"><span data-stu-id="ec702-180">State</span></span>
<span data-ttu-id="ec702-181">System.FM sestavy jako OK po vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="ec702-181">System.FM reports as OK when the service has been created.</span></span> <span data-ttu-id="ec702-182">Odstraní entitu z health store, pokud služba je Odstraněná.</span><span class="sxs-lookup"><span data-stu-id="ec702-182">It deletes the entity from the health store when the service has been deleted.</span></span>

* <span data-ttu-id="ec702-183">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="ec702-183">**SourceId**: System.FM</span></span>
* <span data-ttu-id="ec702-184">**Vlastnost**: stavu</span><span class="sxs-lookup"><span data-stu-id="ec702-184">**Property**: State</span></span>

<span data-ttu-id="ec702-185">Následující příklad ukazuje události stavu služby **fabric: / WordCount/WordCountWebService**:</span><span class="sxs-lookup"><span data-stu-id="ec702-185">The following example shows the state event on the service **fabric:/WordCount/WordCountWebService**:</span></span>

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

### <a name="service-correlation-error"></a><span data-ttu-id="ec702-186">Chyba korelace služby</span><span class="sxs-lookup"><span data-stu-id="ec702-186">Service correlation error</span></span>
<span data-ttu-id="ec702-187">**System.PLB** nahlásí chybu, když zjistí, zda aktualizace služby ke korelaci se jiné služby vytvoří řetězec vztahů.</span><span class="sxs-lookup"><span data-stu-id="ec702-187">**System.PLB** reports an error when it detects that updating a service to be correlated with another service creates an affinity chain.</span></span> <span data-ttu-id="ec702-188">Sestava je vymazán poté, co se stane úspěšná aktualizace.</span><span class="sxs-lookup"><span data-stu-id="ec702-188">The report is cleared when successful update happens.</span></span>

* <span data-ttu-id="ec702-189">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="ec702-189">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="ec702-190">**Vlastnost**: ServiceDescription</span><span class="sxs-lookup"><span data-stu-id="ec702-190">**Property**: ServiceDescription</span></span>
* <span data-ttu-id="ec702-191">**Další kroky**: Zkontrolujte popis korelační služby.</span><span class="sxs-lookup"><span data-stu-id="ec702-191">**Next steps**: Check the correlated service descriptions.</span></span>

## <a name="partition-system-health-reports"></a><span data-ttu-id="ec702-192">Oddíl sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="ec702-192">Partition system health reports</span></span>
<span data-ttu-id="ec702-193">**System.FM**, který představuje službu Failover Manager je autority, který spravuje informace o oddílech služby.</span><span class="sxs-lookup"><span data-stu-id="ec702-193">**System.FM**, which represents the Failover Manager service, is the authority that manages information about service partitions.</span></span>

### <a name="state"></a><span data-ttu-id="ec702-194">Stav</span><span class="sxs-lookup"><span data-stu-id="ec702-194">State</span></span>
<span data-ttu-id="ec702-195">System.FM nahlásí jako OK když oddíl existuje a je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="ec702-195">System.FM reports as OK when the partition has been created and is healthy.</span></span> <span data-ttu-id="ec702-196">Odstraní entitu z health store při odstranění oddílu.</span><span class="sxs-lookup"><span data-stu-id="ec702-196">It deletes the entity from the health store when the partition is deleted.</span></span>

<span data-ttu-id="ec702-197">Pokud oddílu je menší než počet minimální repliky, nahlásí chybu.</span><span class="sxs-lookup"><span data-stu-id="ec702-197">If the partition is below the minimum replica count, it reports an error.</span></span> <span data-ttu-id="ec702-198">Pokud oddíl není nižší než počet minimální repliky, ale je nižší než počtu cílových replik, sestavy upozornění.</span><span class="sxs-lookup"><span data-stu-id="ec702-198">If the partition is not below the minimum replica count, but it is below the target replica count, it reports a warning.</span></span> <span data-ttu-id="ec702-199">Pokud oddíl je ve ztrátě kvora, System.FM nahlásí chybu.</span><span class="sxs-lookup"><span data-stu-id="ec702-199">If the partition is in quorum loss, System.FM reports an error.</span></span>

<span data-ttu-id="ec702-200">Další důležité události zahrnovat upozornění změnu konfigurace trvá déle, než se očekávalo, a při sestavení trvá déle, než se očekávalo.</span><span class="sxs-lookup"><span data-stu-id="ec702-200">Other important events include a warning when the reconfiguration takes longer than expected and when the build takes longer than expected.</span></span> <span data-ttu-id="ec702-201">Očekávané časy pro sestavení a změny konfigurace se dají konfigurovat na základě služby scénářů.</span><span class="sxs-lookup"><span data-stu-id="ec702-201">The expected times for the build and reconfiguration are configurable based on service scenarios.</span></span> <span data-ttu-id="ec702-202">Například pokud má služba terabajt stavu, například SQL Database, sestavení trvá déle než pro službu s malou stavu.</span><span class="sxs-lookup"><span data-stu-id="ec702-202">For example, if a service has a terabyte of state, such as SQL Database, the build takes longer than for a service with a small amount of state.</span></span>

* <span data-ttu-id="ec702-203">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="ec702-203">**SourceId**: System.FM</span></span>
* <span data-ttu-id="ec702-204">**Vlastnost**: stavu</span><span class="sxs-lookup"><span data-stu-id="ec702-204">**Property**: State</span></span>
* <span data-ttu-id="ec702-205">**Další kroky**: Pokud stav není v pořádku, je možné, že nebyly některé repliky vytvoření, otevření nebo povýšen na primární nebo sekundární správně.</span><span class="sxs-lookup"><span data-stu-id="ec702-205">**Next steps**: If the health state is not OK, it's possible that some replicas have not been created, opened, or promoted to primary or secondary correctly.</span></span> <span data-ttu-id="ec702-206">V mnoha případech je hlavní příčinou chyby služby k implementaci otevřené nebo změny role.</span><span class="sxs-lookup"><span data-stu-id="ec702-206">In many instances, the root cause is a service bug in the open or change-role implementation.</span></span>

<span data-ttu-id="ec702-207">Následující příklad ukazuje oddíl v pořádku:</span><span class="sxs-lookup"><span data-stu-id="ec702-207">The following example shows a healthy partition:</span></span>

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

<span data-ttu-id="ec702-208">Následující příklad ukazuje stav oddíl, který je pod počtu cílových replik.</span><span class="sxs-lookup"><span data-stu-id="ec702-208">The following example shows the health of a partition that is below target replica count.</span></span> <span data-ttu-id="ec702-209">Dalším krokem je popis oddílu, který ukazuje, jak jsou nakonfigurované získání: **MinReplicaSetSize** je tři a **TargetReplicaSetSize** je 7.</span><span class="sxs-lookup"><span data-stu-id="ec702-209">The next step is to get the partition description, which shows how it is configured: **MinReplicaSetSize** is three and **TargetReplicaSetSize** is seven.</span></span> <span data-ttu-id="ec702-210">Potom získat počet uzlů v clusteru: pět.</span><span class="sxs-lookup"><span data-stu-id="ec702-210">Then get the number of nodes in the cluster: five.</span></span> <span data-ttu-id="ec702-211">Proto v tomto případě dvě repliky nelze umístit, protože cílový počet replik je vyšší než počet uzlů, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ec702-211">So in this case, two replicas can't be placed because the target number of replicas is higher than the number of nodes available.</span></span>

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

### <a name="replica-constraint-violation"></a><span data-ttu-id="ec702-212">Porušení omezení repliky</span><span class="sxs-lookup"><span data-stu-id="ec702-212">Replica constraint violation</span></span>
<span data-ttu-id="ec702-213">**System.PLB** sestavy upozornění, pokud zjistí porušení omezení repliky a nelze umístit všechny repliky oddílu.</span><span class="sxs-lookup"><span data-stu-id="ec702-213">**System.PLB** reports a warning if it detects a replica constraint violation and can't place all partition replicas.</span></span> <span data-ttu-id="ec702-214">Zobrazí podrobnosti sestavy, které omezení a vlastnosti zabránit umístění repliky.</span><span class="sxs-lookup"><span data-stu-id="ec702-214">The report details show which constraints and properties prevent the replica placement.</span></span>

* <span data-ttu-id="ec702-215">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="ec702-215">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="ec702-216">**Vlastnost**: začíná **ReplicaConstraintViolation**</span><span class="sxs-lookup"><span data-stu-id="ec702-216">**Property**: Starts with **ReplicaConstraintViolation**</span></span>

## <a name="replica-system-health-reports"></a><span data-ttu-id="ec702-217">Repliky sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="ec702-217">Replica system health reports</span></span>
<span data-ttu-id="ec702-218">**System.RA**, která představuje součást reconfiguration agent, je autorita pro stav repliky.</span><span class="sxs-lookup"><span data-stu-id="ec702-218">**System.RA**, which represents the reconfiguration agent component, is the authority for the replica state.</span></span>

### <a name="state"></a><span data-ttu-id="ec702-219">Stav</span><span class="sxs-lookup"><span data-stu-id="ec702-219">State</span></span>
<span data-ttu-id="ec702-220">**System.RA** sestavy OK po vytvoření repliky.</span><span class="sxs-lookup"><span data-stu-id="ec702-220">**System.RA** reports OK when the replica has been created.</span></span>

* <span data-ttu-id="ec702-221">**SourceId**: System.RA</span><span class="sxs-lookup"><span data-stu-id="ec702-221">**SourceId**: System.RA</span></span>
* <span data-ttu-id="ec702-222">**Vlastnost**: stavu</span><span class="sxs-lookup"><span data-stu-id="ec702-222">**Property**: State</span></span>

<span data-ttu-id="ec702-223">Následující příklad ukazuje repliku v pořádku:</span><span class="sxs-lookup"><span data-stu-id="ec702-223">The following example shows a healthy replica:</span></span>

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

### <a name="replica-open-status"></a><span data-ttu-id="ec702-224">Otevřete stav repliky</span><span class="sxs-lookup"><span data-stu-id="ec702-224">Replica open status</span></span>
<span data-ttu-id="ec702-225">Popis této sestavy health obsahuje čas spuštění (Coordinated Universal Time) při volání rozhraní API byla volána.</span><span class="sxs-lookup"><span data-stu-id="ec702-225">The description of this health report contains the start time (Coordinated Universal Time) when the API call was invoked.</span></span>

<span data-ttu-id="ec702-226">**System.RA** sestavy upozornění, pokud repliku open trvá déle, než nastaveném časovém intervalu (výchozí: 30 minut).</span><span class="sxs-lookup"><span data-stu-id="ec702-226">**System.RA** reports a warning if the replica open takes longer than the configured period (default: 30 minutes).</span></span> <span data-ttu-id="ec702-227">Pokud rozhraní API má dopad na dostupnost služby, je vydána sestavu mnohem rychleji (Konfigurovat intervalu, výchozí hodnota je 30 sekund).</span><span class="sxs-lookup"><span data-stu-id="ec702-227">If the API impacts service availability, the report is issued much faster (a configurable interval, with a default of 30 seconds).</span></span> <span data-ttu-id="ec702-228">Měření času zahrnuje čas potřebný pro open Replikátor a otevřete službu.</span><span class="sxs-lookup"><span data-stu-id="ec702-228">The time measured includes the time taken for the replicator open and the service open.</span></span> <span data-ttu-id="ec702-229">Vlastnost se změní na OK open dokončení.</span><span class="sxs-lookup"><span data-stu-id="ec702-229">The property changes to OK if the open completes.</span></span>

* <span data-ttu-id="ec702-230">**SourceId**: System.RA</span><span class="sxs-lookup"><span data-stu-id="ec702-230">**SourceId**: System.RA</span></span>
* <span data-ttu-id="ec702-231">**Vlastnost**: **ReplicaOpenStatus**</span><span class="sxs-lookup"><span data-stu-id="ec702-231">**Property**: **ReplicaOpenStatus**</span></span>
* <span data-ttu-id="ec702-232">**Další kroky**: Pokud stav není v pořádku, zjistěte, proč repliku open trvá déle, než se očekávalo.</span><span class="sxs-lookup"><span data-stu-id="ec702-232">**Next steps**: If the health state is not OK, investigate why the replica open takes longer than expected.</span></span>

### <a name="slow-service-api-call"></a><span data-ttu-id="ec702-233">Pomalá volání rozhraní API služby</span><span class="sxs-lookup"><span data-stu-id="ec702-233">Slow service API call</span></span>
<span data-ttu-id="ec702-234">**System.RAP** a **System.Replicator** sestavy upozornění, pokud volání do kódu uživatele služby trvá déle, než je doba, nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="ec702-234">**System.RAP** and **System.Replicator** report a warning if a call to the user service code takes longer than the configured time.</span></span> <span data-ttu-id="ec702-235">Upozornění je vymazán po dokončení volání.</span><span class="sxs-lookup"><span data-stu-id="ec702-235">The warning is cleared when the call completes.</span></span>

* <span data-ttu-id="ec702-236">**SourceId**: System.RAP nebo System.Replicator</span><span class="sxs-lookup"><span data-stu-id="ec702-236">**SourceId**: System.RAP or System.Replicator</span></span>
* <span data-ttu-id="ec702-237">**Vlastnost**: název pomalé rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ec702-237">**Property**: The name of the slow API.</span></span> <span data-ttu-id="ec702-238">Popis poskytuje další podrobnosti o době, kdy byl čeká na rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ec702-238">The description provides more details about the time the API has been pending.</span></span>
* <span data-ttu-id="ec702-239">**Další kroky**: Zjistěte, proč trvá déle, než bylo očekáváno volání.</span><span class="sxs-lookup"><span data-stu-id="ec702-239">**Next steps**: Investigate why the call takes longer than expected.</span></span>

<span data-ttu-id="ec702-240">Následující příklad ukazuje oddíl ve ztrátě kvora a provádí zjistěte, proč kroky šetření.</span><span class="sxs-lookup"><span data-stu-id="ec702-240">The following example shows a partition in quorum loss, and the investigation steps done to figure out why.</span></span> <span data-ttu-id="ec702-241">Jedna z replik má stav varování, proto jeho stav.</span><span class="sxs-lookup"><span data-stu-id="ec702-241">One of the replicas has a warning health state, so you get its health.</span></span> <span data-ttu-id="ec702-242">Zobrazuje, že operace služby trvá déle, než se očekávalo, hlášené System.RAP událost.</span><span class="sxs-lookup"><span data-stu-id="ec702-242">It shows that the service operation takes longer than expected, an event reported by System.RAP.</span></span> <span data-ttu-id="ec702-243">Po přijetí těchto informací je dalším krokem je kódu služby a prozkoumat existuje.</span><span class="sxs-lookup"><span data-stu-id="ec702-243">After this information is received, the next step is to look at the service code and investigate there.</span></span> <span data-ttu-id="ec702-244">Pro tento případ **RunAsync** implementace stavové služby, vyvolá k neošetřené výjimce.</span><span class="sxs-lookup"><span data-stu-id="ec702-244">For this case, the **RunAsync** implementation of the stateful service throws an unhandled exception.</span></span> <span data-ttu-id="ec702-245">Repliky jsou recyklace, takže se nemusí zobrazovat všechny repliky ve stavu upozornění.</span><span class="sxs-lookup"><span data-stu-id="ec702-245">The replicas are recycling, so you may not see any replicas in the warning state.</span></span> <span data-ttu-id="ec702-246">Můžete opakovat získávání stavu a vyhledejte případné rozdíly v ID repliky.</span><span class="sxs-lookup"><span data-stu-id="ec702-246">You can retry getting the health state and look for any differences in the replica ID.</span></span> <span data-ttu-id="ec702-247">V některých případech opakované pokusy vám může poskytnout různá vodítka.</span><span class="sxs-lookup"><span data-stu-id="ec702-247">In certain cases, the retries can give you clues.</span></span>

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

<span data-ttu-id="ec702-248">Při spuštění aplikace vadný v ladicím programu zobrazit windows diagnostických událostí výjimky z RunAsync:</span><span class="sxs-lookup"><span data-stu-id="ec702-248">When you start the faulty application under the debugger, the diagnostic events windows show the exception thrown from RunAsync:</span></span>

![Visual Studio 2015 diagnostických událostí: selhání RunAsync v fabric: / HelloWorldStatefulApplication.][1]

<span data-ttu-id="ec702-250">Visual Studio 2015 diagnostických událostí: selhání RunAsync v **fabric: / HelloWorldStatefulApplication**.</span><span class="sxs-lookup"><span data-stu-id="ec702-250">Visual Studio 2015 diagnostic events: RunAsync failure in **fabric:/HelloWorldStatefulApplication**.</span></span>

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a><span data-ttu-id="ec702-251">Replikační fronta je plná</span><span class="sxs-lookup"><span data-stu-id="ec702-251">Replication queue full</span></span>
<span data-ttu-id="ec702-252">**System.Replicator** nahlásí upozornění, když se fronta replikací je plná.</span><span class="sxs-lookup"><span data-stu-id="ec702-252">**System.Replicator** reports a warning when the replication queue is full.</span></span> <span data-ttu-id="ec702-253">Na primárním fronty replikací obvykle plný protože jeden nebo více sekundárních replikách jsou pomalé potvrdit operace.</span><span class="sxs-lookup"><span data-stu-id="ec702-253">On the primary, replication queue usually becomes full because one or more secondary replicas are slow to acknowledge operations.</span></span> <span data-ttu-id="ec702-254">Na sekundárním to obvykle se stane, když služba pomalé použít operace.</span><span class="sxs-lookup"><span data-stu-id="ec702-254">On the secondary, this usually happens when the service is slow to apply the operations.</span></span> <span data-ttu-id="ec702-255">Upozornění je vymazán poté, co už fronta je plná.</span><span class="sxs-lookup"><span data-stu-id="ec702-255">The warning is cleared when the queue is no longer full.</span></span>

* <span data-ttu-id="ec702-256">**SourceId**: System.Replicator</span><span class="sxs-lookup"><span data-stu-id="ec702-256">**SourceId**: System.Replicator</span></span>
* <span data-ttu-id="ec702-257">**Vlastnost**: **PrimaryReplicationQueueStatus** nebo **SecondaryReplicationQueueStatus**, v závislosti na roli repliky</span><span class="sxs-lookup"><span data-stu-id="ec702-257">**Property**: **PrimaryReplicationQueueStatus** or **SecondaryReplicationQueueStatus**, depending on the replica role</span></span>

### <a name="slow-naming-operations"></a><span data-ttu-id="ec702-258">Pomalé operations pojmenování</span><span class="sxs-lookup"><span data-stu-id="ec702-258">Slow Naming operations</span></span>
<span data-ttu-id="ec702-259">**System.NamingService** při operaci pojmenování trvá déle než přijatelné hlásí stav na jeho primární repliky.</span><span class="sxs-lookup"><span data-stu-id="ec702-259">**System.NamingService** reports health on its primary replica when a Naming operation takes longer than acceptable.</span></span> <span data-ttu-id="ec702-260">Příklady operací pojmenování [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) nebo [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync).</span><span class="sxs-lookup"><span data-stu-id="ec702-260">Examples of Naming operations are [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) or [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync).</span></span> <span data-ttu-id="ec702-261">Další metody naleznete v části FabricClient, například v [služby metody správy](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) nebo [metody správy vlastnost](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).</span><span class="sxs-lookup"><span data-stu-id="ec702-261">More methods can be found under FabricClient, for example under [service management methods](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) or [property management methods](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).</span></span>

> [!NOTE]
> <span data-ttu-id="ec702-262">Službu Naming překládá názvy služby do umístění v clusteru a umožňuje uživatelům spravovat služby názvy a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="ec702-262">The Naming service resolves service names to a location in the cluster and enables users to manage service names and properties.</span></span> <span data-ttu-id="ec702-263">Je, že služba jako trvalý, Service Fabric rozdělena na oddíly.</span><span class="sxs-lookup"><span data-stu-id="ec702-263">It is a Service Fabric partitioned persisted service.</span></span> <span data-ttu-id="ec702-264">Jeden z oddílů představuje Authority Owner, který obsahuje metadata o všechny názvy Service Fabric a služeb.</span><span class="sxs-lookup"><span data-stu-id="ec702-264">One of the partitions represents the Authority Owner, which contains metadata about all Service Fabric names and services.</span></span> <span data-ttu-id="ec702-265">Service Fabric názvy jsou namapované na různé oddíly, označované jako oddíly Name Owner, tak služba je rozšiřitelný.</span><span class="sxs-lookup"><span data-stu-id="ec702-265">The Service Fabric names are mapped to different partitions, called Name Owner partitions, so the service is extensible.</span></span> <span data-ttu-id="ec702-266">Další informace o [Naming service](service-fabric-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="ec702-266">Read more about [Naming service](service-fabric-architecture.md).</span></span>
> 
> 

<span data-ttu-id="ec702-267">Při operaci pojmenování trvá déle, než se očekávalo, operace je příznak se sestavou upozornění na *primární repliky oddílu pojmenování služby, který slouží operaci*.</span><span class="sxs-lookup"><span data-stu-id="ec702-267">When a Naming operation takes longer than expected, the operation is flagged with a Warning report on the *primary replica of the Naming service partition that serves the operation*.</span></span> <span data-ttu-id="ec702-268">Pokud po úspěšném dokončení operace je zrušeno upozornění.</span><span class="sxs-lookup"><span data-stu-id="ec702-268">If the operation completes successfully, the Warning is cleared.</span></span> <span data-ttu-id="ec702-269">Dokončení operace s chybou, sestava stavu zahrnuje podrobnosti o této chybě.</span><span class="sxs-lookup"><span data-stu-id="ec702-269">If the operation completes with an error, the health report includes details about the error.</span></span>

* <span data-ttu-id="ec702-270">**SourceId**: System.NamingService</span><span class="sxs-lookup"><span data-stu-id="ec702-270">**SourceId**: System.NamingService</span></span>
* <span data-ttu-id="ec702-271">**Vlastnost**: začíná předponu **Duration_** a identifikuje pomalé operaci a název Service Fabric, na kterém se používá operaci.</span><span class="sxs-lookup"><span data-stu-id="ec702-271">**Property**: Starts with prefix **Duration_** and identifies the slow operation and the Service Fabric name on which the operation is applied.</span></span> <span data-ttu-id="ec702-272">Například pokud vytvořit službu na název fabric: / MyApp/Moje_služba trvá příliš dlouho, vlastnost je Duration_AOCreateService.fabric:/MyApp/MyService.</span><span class="sxs-lookup"><span data-stu-id="ec702-272">For example, if create service at name fabric:/MyApp/MyService takes too long, the property is Duration_AOCreateService.fabric:/MyApp/MyService.</span></span> <span data-ttu-id="ec702-273">AO odkazuje na roli pojmenování oddílu pro tento název a operaci.</span><span class="sxs-lookup"><span data-stu-id="ec702-273">AO points to the role of the Naming partition for this name and operation.</span></span>
* <span data-ttu-id="ec702-274">**Další kroky**: Kontrola proč pojmenování operace selže.</span><span class="sxs-lookup"><span data-stu-id="ec702-274">**Next steps**: Check why the Naming operation fails.</span></span> <span data-ttu-id="ec702-275">Každé operace může mít různé kořenové příčiny.</span><span class="sxs-lookup"><span data-stu-id="ec702-275">Each operation can have different root causes.</span></span> <span data-ttu-id="ec702-276">Například odstranit, může být služba zablokována na uzlu, protože udržuje na uzlu z důvodu chyby uživatele v kódu služby chybám hostitele aplikací.</span><span class="sxs-lookup"><span data-stu-id="ec702-276">For example, delete service may be stuck on a node because the application host keeps crashing on a node due to a user bug in the service code.</span></span>

<span data-ttu-id="ec702-277">Následující příklad ukazuje operaci vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="ec702-277">The following example shows a create service operation.</span></span> <span data-ttu-id="ec702-278">Operace trvalo déle, než nakonfigurovaná doba trvání.</span><span class="sxs-lookup"><span data-stu-id="ec702-278">The operation took longer than the configured duration.</span></span> <span data-ttu-id="ec702-279">AO opakování a odešle pracovní ne.</span><span class="sxs-lookup"><span data-stu-id="ec702-279">AO retries and sends work to NO.</span></span> <span data-ttu-id="ec702-280">JIŽ byla dokončena poslední operaci s vypršením časového limitu.</span><span class="sxs-lookup"><span data-stu-id="ec702-280">NO completed the last operation with Timeout.</span></span> <span data-ttu-id="ec702-281">V takovém případě je stejné repliky primární AO a žádné role.</span><span class="sxs-lookup"><span data-stu-id="ec702-281">In this case, the same replica is primary for both the AO and NO roles.</span></span>

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

## <a name="deployedapplication-system-health-reports"></a><span data-ttu-id="ec702-282">DeployedApplication sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="ec702-282">DeployedApplication system health reports</span></span>
<span data-ttu-id="ec702-283">**System.Hosting** autoritou na nasazené entity.</span><span class="sxs-lookup"><span data-stu-id="ec702-283">**System.Hosting** is the authority on deployed entities.</span></span>

### <a name="activation"></a><span data-ttu-id="ec702-284">Aktivace</span><span class="sxs-lookup"><span data-stu-id="ec702-284">Activation</span></span>
<span data-ttu-id="ec702-285">System.Hosting nahlásí jako OK když aplikace úspěšně aktivuje na uzlu.</span><span class="sxs-lookup"><span data-stu-id="ec702-285">System.Hosting reports as OK when an application has been successfully activated on the node.</span></span> <span data-ttu-id="ec702-286">V opačném případě nahlásí chybu.</span><span class="sxs-lookup"><span data-stu-id="ec702-286">Otherwise, it reports an error.</span></span>

* <span data-ttu-id="ec702-287">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="ec702-287">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="ec702-288">**Vlastnost**: aktivace, včetně zavedení verze</span><span class="sxs-lookup"><span data-stu-id="ec702-288">**Property**: Activation, including the rollout version</span></span>
* <span data-ttu-id="ec702-289">**Další kroky**: Pokud aplikace není v pořádku, zjistěte, proč aktivace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="ec702-289">**Next steps**: If the application is unhealthy, investigate why the activation failed.</span></span>

<span data-ttu-id="ec702-290">Následující příklad ukazuje úspěšné aktivaci:</span><span class="sxs-lookup"><span data-stu-id="ec702-290">The following example shows successful activation:</span></span>

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

### <a name="download"></a><span data-ttu-id="ec702-291">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="ec702-291">Download</span></span>
<span data-ttu-id="ec702-292">**System.Hosting** nahlásí chybu, pokud stahování balíčku aplikace selže.</span><span class="sxs-lookup"><span data-stu-id="ec702-292">**System.Hosting** reports an error if the application package download fails.</span></span>

* <span data-ttu-id="ec702-293">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="ec702-293">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="ec702-294">**Vlastnost**:  **stáhnout:*RolloutVersion***</span><span class="sxs-lookup"><span data-stu-id="ec702-294">**Property**: **Download:*RolloutVersion***</span></span>
* <span data-ttu-id="ec702-295">**Další kroky**: Zjistěte, proč se stahování v tomto uzlu selhal.</span><span class="sxs-lookup"><span data-stu-id="ec702-295">**Next steps**: Investigate why the download failed on the node.</span></span>

## <a name="deployedservicepackage-system-health-reports"></a><span data-ttu-id="ec702-296">DeployedServicePackage sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="ec702-296">DeployedServicePackage system health reports</span></span>
<span data-ttu-id="ec702-297">**System.Hosting** autoritou na nasazené entity.</span><span class="sxs-lookup"><span data-stu-id="ec702-297">**System.Hosting** is the authority on deployed entities.</span></span>

### <a name="service-package-activation"></a><span data-ttu-id="ec702-298">Služba aktivace balíčku</span><span class="sxs-lookup"><span data-stu-id="ec702-298">Service package activation</span></span>
<span data-ttu-id="ec702-299">System.Hosting jako OK sestavy, pokud je aktivace balíček služby v uzlu úspěšná.</span><span class="sxs-lookup"><span data-stu-id="ec702-299">System.Hosting reports as OK if the service package activation on the node is successful.</span></span> <span data-ttu-id="ec702-300">V opačném případě nahlásí chybu.</span><span class="sxs-lookup"><span data-stu-id="ec702-300">Otherwise, it reports an error.</span></span>

* <span data-ttu-id="ec702-301">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="ec702-301">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="ec702-302">**Vlastnost**: Aktivace</span><span class="sxs-lookup"><span data-stu-id="ec702-302">**Property**: Activation</span></span>
* <span data-ttu-id="ec702-303">**Další kroky**: Zjistěte, proč aktivace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="ec702-303">**Next steps**: Investigate why the activation failed.</span></span>

### <a name="code-package-activation"></a><span data-ttu-id="ec702-304">Aktivace balíčku kódu.</span><span class="sxs-lookup"><span data-stu-id="ec702-304">Code package activation</span></span>
<span data-ttu-id="ec702-305">**System.Hosting** sestavy jako OK pro každý balíček kódu, pokud aktivace nebude úspěšná.</span><span class="sxs-lookup"><span data-stu-id="ec702-305">**System.Hosting** reports as OK for each code package if the activation is successful.</span></span> <span data-ttu-id="ec702-306">Pokud se aktivace nezdaří, sestavy upozornění podle konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ec702-306">If the activation fails, it reports a warning as configured.</span></span> <span data-ttu-id="ec702-307">Pokud **CodePackage** nepodaří aktivovat nebo ukončí s chybou větší než nakonfigurované **CodePackageHealthErrorThreshold**, nahlásí chybu, který je hostitelem.</span><span class="sxs-lookup"><span data-stu-id="ec702-307">If **CodePackage** fails to activate or terminates with an error greater than the configured **CodePackageHealthErrorThreshold**, hosting reports an error.</span></span> <span data-ttu-id="ec702-308">Pokud balíček služby obsahuje více balíčků kódu, zprávu o aktivaci se generuje pro každé z nich.</span><span class="sxs-lookup"><span data-stu-id="ec702-308">If a service package contains multiple code packages, an activation report is generated for each one.</span></span>

* <span data-ttu-id="ec702-309">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="ec702-309">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="ec702-310">**Vlastnost**: používá předponu **CodePackageActivation** a obsahuje název balíček kódu a vstupního bodu jako  **CodePackageActivation:*CodePackageName*:*SetupEntryPoint/EntryPoint*** (například **CodePackageActivation:Code:SetupEntryPoint**)</span><span class="sxs-lookup"><span data-stu-id="ec702-310">**Property**: Uses the prefix **CodePackageActivation** and contains the name of the code package and the entry point as **CodePackageActivation:*CodePackageName*:*SetupEntryPoint/EntryPoint*** (for example, **CodePackageActivation:Code:SetupEntryPoint**)</span></span>

### <a name="service-type-registration"></a><span data-ttu-id="ec702-311">Typ registrace služby</span><span class="sxs-lookup"><span data-stu-id="ec702-311">Service type registration</span></span>
<span data-ttu-id="ec702-312">**System.Hosting** sestavy jako OK, pokud typ služby byl úspěšně zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="ec702-312">**System.Hosting** reports as OK if the service type has been registered successfully.</span></span> <span data-ttu-id="ec702-313">Nahlásí chybu, pokud nebyla provedena registrace v čase (jak je nakonfigurovat pomocí **ServiceTypeRegistrationTimeout**).</span><span class="sxs-lookup"><span data-stu-id="ec702-313">It reports an error if the registration wasn't done in time (as configured by using **ServiceTypeRegistrationTimeout**).</span></span> <span data-ttu-id="ec702-314">Pokud je zavřená modulu runtime, typ služby je odregistrovat z uzlu a hostitelský sestavy upozornění.</span><span class="sxs-lookup"><span data-stu-id="ec702-314">If the runtime is closed, the service type is unregistered from the node and Hosting reports a warning.</span></span>

* <span data-ttu-id="ec702-315">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="ec702-315">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="ec702-316">**Vlastnost**: používá předponu **ServiceTypeRegistration** a obsahuje název typu služby (například **ServiceTypeRegistration:FileStoreServiceType**)</span><span class="sxs-lookup"><span data-stu-id="ec702-316">**Property**: Uses the prefix **ServiceTypeRegistration** and contains the service type name (for example, **ServiceTypeRegistration:FileStoreServiceType**)</span></span>

<span data-ttu-id="ec702-317">Následující příklad ukazuje v pořádku nasazený balíček služby:</span><span class="sxs-lookup"><span data-stu-id="ec702-317">The following example shows a healthy deployed service package:</span></span>

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

### <a name="download"></a><span data-ttu-id="ec702-318">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="ec702-318">Download</span></span>
<span data-ttu-id="ec702-319">**System.Hosting** nahlásí chybu, pokud služba stahování balíčku selže.</span><span class="sxs-lookup"><span data-stu-id="ec702-319">**System.Hosting** reports an error if the service package download fails.</span></span>

* <span data-ttu-id="ec702-320">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="ec702-320">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="ec702-321">**Vlastnost**:  **stáhnout:*RolloutVersion***</span><span class="sxs-lookup"><span data-stu-id="ec702-321">**Property**: **Download:*RolloutVersion***</span></span>
* <span data-ttu-id="ec702-322">**Další kroky**: Zjistěte, proč se stahování v tomto uzlu selhal.</span><span class="sxs-lookup"><span data-stu-id="ec702-322">**Next steps**: Investigate why the download failed on the node.</span></span>

### <a name="upgrade-validation"></a><span data-ttu-id="ec702-323">Ověření upgradu</span><span class="sxs-lookup"><span data-stu-id="ec702-323">Upgrade validation</span></span>
<span data-ttu-id="ec702-324">**System.Hosting** nahlásí chybu, pokud selže ověření během upgradu nebo pokud upgrade selže na uzlu.</span><span class="sxs-lookup"><span data-stu-id="ec702-324">**System.Hosting** reports an error if validation during the upgrade fails or if the upgrade fails on the node.</span></span>

* <span data-ttu-id="ec702-325">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="ec702-325">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="ec702-326">**Vlastnost**: používá předponu **FabricUpgradeValidation** a obsahuje upgradovaná verze</span><span class="sxs-lookup"><span data-stu-id="ec702-326">**Property**: Uses the prefix **FabricUpgradeValidation** and contains the upgrade version</span></span>
* <span data-ttu-id="ec702-327">**Popis**: odkazuje na došlo k chybě</span><span class="sxs-lookup"><span data-stu-id="ec702-327">**Description**: Points to the error encountered</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec702-328">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec702-328">Next steps</span></span>
[<span data-ttu-id="ec702-329">Zobrazit sestavy stavu Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ec702-329">View Service Fabric health reports</span></span>](service-fabric-view-entities-aggregated-health.md)

[<span data-ttu-id="ec702-330">Postup vytvoření sestavy a zkontrolujte stav služby</span><span class="sxs-lookup"><span data-stu-id="ec702-330">How to report and check service health</span></span>](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[<span data-ttu-id="ec702-331">Monitorování a Diagnostika služby místně</span><span class="sxs-lookup"><span data-stu-id="ec702-331">Monitor and diagnose services locally</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="ec702-332">Upgrade aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ec702-332">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

