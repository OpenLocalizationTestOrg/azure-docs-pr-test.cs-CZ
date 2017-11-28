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
# <a name="use-system-health-reports-tootroubleshoot"></a><span data-ttu-id="6a6db-103">Použít tootroubleshoot sestavy stavu systému</span><span class="sxs-lookup"><span data-stu-id="6a6db-103">Use system health reports tootroubleshoot</span></span>
<span data-ttu-id="6a6db-104">Azure Service Fabric součásti sestavy předinstalované hello na všechny entity v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-104">Azure Service Fabric components report out of hello box on all entities in hello cluster.</span></span> <span data-ttu-id="6a6db-105">Hello [úložiště stavu](service-fabric-health-introduction.md#health-store) vytvoří nebo odstraní entity založené na zprávách systému hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-105">hello [health store](service-fabric-health-introduction.md#health-store) creates and deletes entities based on hello system reports.</span></span> <span data-ttu-id="6a6db-106">Také slouží k uspořádání je v hierarchii, která zaznamená interakce entity.</span><span class="sxs-lookup"><span data-stu-id="6a6db-106">It also organizes them in a hierarchy that captures entity interactions.</span></span>

> [!NOTE]
> <span data-ttu-id="6a6db-107">Koncepty toounderstand související se stavem, další informace v [modelu stavu Service Fabric](service-fabric-health-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6a6db-107">toounderstand health-related concepts, read more at [Service Fabric health model](service-fabric-health-introduction.md).</span></span>
> 
> 

<span data-ttu-id="6a6db-108">Sestav o stavu systému poskytují přehled o clusteru a příznak problémy prostřednictvím stavu a funkce aplikací.</span><span class="sxs-lookup"><span data-stu-id="6a6db-108">System health reports provide visibility into cluster and application functionality and flag issues through health.</span></span> <span data-ttu-id="6a6db-109">Pro aplikace a služby ověřte sestav o stavu systému entity jsou implementované a chovají správně z hello perspektivy Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6a6db-109">For applications and services, system health reports verify that entities are implemented and are behaving correctly from hello Service Fabric perspective.</span></span> <span data-ttu-id="6a6db-110">sestavy Hello neposkytují žádné sledování stavu hello obchodní logiky hello služby nebo zjišťování "zamrzlých" procesů.</span><span class="sxs-lookup"><span data-stu-id="6a6db-110">hello reports do not provide any health monitoring of hello business logic of hello service or detection of hung processes.</span></span> <span data-ttu-id="6a6db-111">Data stavu hello se informace o konkrétní tootheir logikou lze rozšířit uživatele služby.</span><span class="sxs-lookup"><span data-stu-id="6a6db-111">User services can enrich hello health data with information specific tootheir logic.</span></span>

> [!NOTE]
> <span data-ttu-id="6a6db-112">Sestavy stavu watchdogs jsou viditelné pouze *po* součásti systému hello vytvořte entitu.</span><span class="sxs-lookup"><span data-stu-id="6a6db-112">Watchdogs health reports are visible only *after* hello system components create an entity.</span></span> <span data-ttu-id="6a6db-113">Při odstranění entity hello stavu úložiště automaticky odstraní všechny sestavy stavu s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="6a6db-113">When an entity is deleted, hello health store automatically deletes all health reports associated with it.</span></span> <span data-ttu-id="6a6db-114">Hello stejné je hodnota true, když je vytvořena nová instance entity hello (například je vytvořena nová instance repliky stavové trvalou služby).</span><span class="sxs-lookup"><span data-stu-id="6a6db-114">hello same is true when a new instance of hello entity is created (for example, a new stateful persisted service replica instance is created).</span></span> <span data-ttu-id="6a6db-115">Všechny sestavy přidružené k původní instance hello jsou odstraněna a vyčistit z úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-115">All reports associated with hello old instance are deleted and cleaned up from hello store.</span></span>
> 
> 

<span data-ttu-id="6a6db-116">Hello sestav součásti systému jsou identifikovány hello zdroj, který začíná textem hello "**systému.**"</span><span class="sxs-lookup"><span data-stu-id="6a6db-116">hello system component reports are identified by hello source, which starts with hello "**System.**"</span></span> <span data-ttu-id="6a6db-117">Předpona.</span><span class="sxs-lookup"><span data-stu-id="6a6db-117">prefix.</span></span> <span data-ttu-id="6a6db-118">Watchdogs nelze použít hello stejnou předponu pro jejich zdroje, jako jsou odmítnuta sestavy se neplatné parametry.</span><span class="sxs-lookup"><span data-stu-id="6a6db-118">Watchdogs can't use hello same prefix for their sources, as reports with invalid parameters are rejected.</span></span>
<span data-ttu-id="6a6db-119">Pojďme podívejte se na některé systému sestavy toounderstand, co je aktivuje a jak toocorrect hello možných problémů se představují.</span><span class="sxs-lookup"><span data-stu-id="6a6db-119">Let's look at some system reports toounderstand what triggers them and how toocorrect hello possible issues they represent.</span></span>

> [!NOTE]
> <span data-ttu-id="6a6db-120">Service Fabric pokračuje tooadd sestavy týkající se podmínek, které vylepšují získat přehled o dění v hello cluster a aplikace.</span><span class="sxs-lookup"><span data-stu-id="6a6db-120">Service Fabric continues tooadd reports on conditions of interest that improve visibility into what is happening in hello cluster and application.</span></span> <span data-ttu-id="6a6db-121">Existující sestavy lze také rozšířit s dalšími podrobnostmi toohelp řešení hello problému rychlejší.</span><span class="sxs-lookup"><span data-stu-id="6a6db-121">Existing reports can also be enhanced with more details toohelp troubleshoot hello problem faster.</span></span>
> 
> 

## <a name="cluster-system-health-reports"></a><span data-ttu-id="6a6db-122">Cluster sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="6a6db-122">Cluster system health reports</span></span>
<span data-ttu-id="6a6db-123">Hello clusteru stavu entity se vytvoří automaticky v hello health store.</span><span class="sxs-lookup"><span data-stu-id="6a6db-123">hello cluster health entity is created automatically in hello health store.</span></span> <span data-ttu-id="6a6db-124">Pokud všechno funguje správně, nemá sestavu system.</span><span class="sxs-lookup"><span data-stu-id="6a6db-124">If everything works properly, it doesn't have a system report.</span></span>

### <a name="neighborhood-loss"></a><span data-ttu-id="6a6db-125">Ztráta Okolní počítače</span><span class="sxs-lookup"><span data-stu-id="6a6db-125">Neighborhood loss</span></span>
<span data-ttu-id="6a6db-126">**System.Federation** nahlásí chybu, když zjistí ztrátu okolí.</span><span class="sxs-lookup"><span data-stu-id="6a6db-126">**System.Federation** reports an error when it detects a neighborhood loss.</span></span> <span data-ttu-id="6a6db-127">Sestava Hello je z jednotlivých uzlů a ID uzlu hello je součástí názvu vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-127">hello report is from individual nodes, and hello node ID is included in hello property name.</span></span> <span data-ttu-id="6a6db-128">Pokud je jeden okolí ztratili v Service Fabric prstenec celý text hello, můžete očekávat obvykle dvě události (obou stranách sestavy mezera hello).</span><span class="sxs-lookup"><span data-stu-id="6a6db-128">If one neighborhood is lost in hello entire Service Fabric ring, you can typically expect two events (both sides of hello gap report).</span></span> <span data-ttu-id="6a6db-129">Pokud další sousedství jsou ztraceny, existují další události.</span><span class="sxs-lookup"><span data-stu-id="6a6db-129">If more neighborhoods are lost, there are more events.</span></span>

<span data-ttu-id="6a6db-130">Sestava Hello Určuje časový limit zapůjčení globální hello jako čas hello toolive.</span><span class="sxs-lookup"><span data-stu-id="6a6db-130">hello report specifies hello global lease timeout as hello time toolive.</span></span> <span data-ttu-id="6a6db-131">Sestava Hello je nutno každých poloviny doba TTL hello tak dlouho, dokud podmínky hello zůstává aktivní.</span><span class="sxs-lookup"><span data-stu-id="6a6db-131">hello report is resent every half of hello TTL duration for as long as hello condition remains active.</span></span> <span data-ttu-id="6a6db-132">po jeho vypršení je automaticky odstraněna Hello událost.</span><span class="sxs-lookup"><span data-stu-id="6a6db-132">hello event is automatically removed when it expires.</span></span> <span data-ttu-id="6a6db-133">Odeberte, pokud vypršela platnost zaručuje, že sestava hello je vyčištěna z hello health store správně, přestože sestavy uzlu hello je vypnutý.</span><span class="sxs-lookup"><span data-stu-id="6a6db-133">Remove when expired behavior ensures that hello report is cleaned up from hello health store correctly, even if hello reporting node is down.</span></span>

* <span data-ttu-id="6a6db-134">**SourceId**: System.Federation</span><span class="sxs-lookup"><span data-stu-id="6a6db-134">**SourceId**: System.Federation</span></span>
* <span data-ttu-id="6a6db-135">**Vlastnost**: začíná **okolí** a obsahuje informace o uzlu</span><span class="sxs-lookup"><span data-stu-id="6a6db-135">**Property**: Starts with **Neighborhood** and includes node information</span></span>
* <span data-ttu-id="6a6db-136">**Další kroky**: Zjistěte, proč dojde ke ztrátě (např. Zkontrolujte hello komunikaci mezi uzly clusteru) a okolí hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-136">**Next steps**: Investigate why hello neighborhood is lost (for example, check hello communication between cluster nodes).</span></span>

## <a name="node-system-health-reports"></a><span data-ttu-id="6a6db-137">Uzel sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="6a6db-137">Node system health reports</span></span>
<span data-ttu-id="6a6db-138">**System.FM**, který představuje službu Failover Manager hello, hello autoritou, která spravuje informace o uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a6db-138">**System.FM**, which represents hello Failover Manager service, is hello authority that manages information about cluster nodes.</span></span> <span data-ttu-id="6a6db-139">Každý uzel musí mít jednu sestavu z System.FM zobrazuje stav.</span><span class="sxs-lookup"><span data-stu-id="6a6db-139">Each node should have one report from System.FM showing its state.</span></span> <span data-ttu-id="6a6db-140">Hello uzlu entity se odeberou, když se odebere stav uzlu hello (viz [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).</span><span class="sxs-lookup"><span data-stu-id="6a6db-140">hello node entities are removed when hello node state is removed (see [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).</span></span>

### <a name="node-updown"></a><span data-ttu-id="6a6db-141">Uzel nahoru/dolů</span><span class="sxs-lookup"><span data-stu-id="6a6db-141">Node up/down</span></span>
<span data-ttu-id="6a6db-142">System.FM nahlásí jako OK, když se uzel hello připojí hello prstenec (je spuštěná).</span><span class="sxs-lookup"><span data-stu-id="6a6db-142">System.FM reports as OK when hello node joins hello ring (it's up and running).</span></span> <span data-ttu-id="6a6db-143">Nahlásí chybu, pokud uzel hello vyplouvající hello prstenec (je vypnutý, buď pro upgrade nebo jednoduše protože se nezdařilo).</span><span class="sxs-lookup"><span data-stu-id="6a6db-143">It reports an error when hello node departs hello ring (it's down, either for upgrading or simply because it has failed).</span></span> <span data-ttu-id="6a6db-144">Hello stavu hierarchie sestavena úložiště stavu hello provádět akce se nasazené entity v korelace s System.FM sestavy uzlu.</span><span class="sxs-lookup"><span data-stu-id="6a6db-144">hello health hierarchy built by hello health store takes action on deployed entities in correlation with System.FM node reports.</span></span> <span data-ttu-id="6a6db-145">Uzel hello považuje virtuální nadřazené všechny nasazené entit.</span><span class="sxs-lookup"><span data-stu-id="6a6db-145">It considers hello node a virtual parent of all deployed entities.</span></span> <span data-ttu-id="6a6db-146">Hello nasazení entit na tomto uzlu se zveřejňují přes dotazy, pokud uzel hello hlášení jako si podle System.FM, s hello stejné instance jako hello instanci spojenou s hello entity.</span><span class="sxs-lookup"><span data-stu-id="6a6db-146">hello deployed entities on that node are exposed through queries if hello node is reported as up by System.FM, with hello same instance as hello instance associated with hello entities.</span></span> <span data-ttu-id="6a6db-147">Když System.FM ohlásí tento uzel hello je mimo provoz nebo restartovat (nové instance), úložiště stavu hello automaticky vyčistí hello nasazení entit, které může existovat pouze na hello dolů uzlu nebo na předchozí instanci hello hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="6a6db-147">When System.FM reports that hello node is down or restarted (a new instance), hello health store automatically cleans up hello deployed entities that can exist only on hello down node or on hello previous instance of hello node.</span></span>

* <span data-ttu-id="6a6db-148">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="6a6db-148">**SourceId**: System.FM</span></span>
* <span data-ttu-id="6a6db-149">**Vlastnost**: stavu</span><span class="sxs-lookup"><span data-stu-id="6a6db-149">**Property**: State</span></span>
* <span data-ttu-id="6a6db-150">**Další kroky**: Pokud uzel hello je vypnutý pro upgrade, by měl mít zpět po byl upgradován.</span><span class="sxs-lookup"><span data-stu-id="6a6db-150">**Next steps**: If hello node is down for an upgrade, it should come back up once it has been upgraded.</span></span> <span data-ttu-id="6a6db-151">V takovém případě by měl hello stav přepnout zpět tooOK.</span><span class="sxs-lookup"><span data-stu-id="6a6db-151">In this case, hello health state should switch back tooOK.</span></span> <span data-ttu-id="6a6db-152">Pokud uzel hello nepřejde do stavu zpět nebo se nezdaří, problém hello potřebuje další šetření.</span><span class="sxs-lookup"><span data-stu-id="6a6db-152">If hello node doesn't come back or it fails, hello problem needs more investigation.</span></span>

<span data-ttu-id="6a6db-153">Hello následující příklad ukazuje hello System.FM události se stavem stavu OK pro uzel:</span><span class="sxs-lookup"><span data-stu-id="6a6db-153">hello following example shows hello System.FM event with a health state of OK for node up:</span></span>

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


### <a name="certificate-expiration"></a><span data-ttu-id="6a6db-154">Vypršení platnosti certifikátu</span><span class="sxs-lookup"><span data-stu-id="6a6db-154">Certificate expiration</span></span>
<span data-ttu-id="6a6db-155">**System.FabricNode** sestavy upozornění, když se certifikáty používané hello uzlu blíží vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="6a6db-155">**System.FabricNode** reports a warning when certificates used by hello node are near expiration.</span></span> <span data-ttu-id="6a6db-156">Existují tři certifikáty na uzel: **Certificate_cluster**, **Certificate_server**, a **Certificate_default_client**.</span><span class="sxs-lookup"><span data-stu-id="6a6db-156">There are three certificates per node: **Certificate_cluster**, **Certificate_server**, and **Certificate_default_client**.</span></span> <span data-ttu-id="6a6db-157">Alespoň dva týdny po vypršení platnosti hello je hello sestavy stavu v pořádku.</span><span class="sxs-lookup"><span data-stu-id="6a6db-157">When hello expiration is at least two weeks away, hello report health state is OK.</span></span> <span data-ttu-id="6a6db-158">Po vypršení platnosti hello během dvou týdnů hello typ sestavy je upozornění.</span><span class="sxs-lookup"><span data-stu-id="6a6db-158">When hello expiration is within two weeks, hello report type is a warning.</span></span> <span data-ttu-id="6a6db-159">Hodnota TTL z těchto událostí je nekonečno, a budou odstraněny Jestliže uzel opustí hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a6db-159">TTL of these events is infinite, and they are removed when a node leaves hello cluster.</span></span>

* <span data-ttu-id="6a6db-160">**SourceId**: System.FabricNode</span><span class="sxs-lookup"><span data-stu-id="6a6db-160">**SourceId**: System.FabricNode</span></span>
* <span data-ttu-id="6a6db-161">**Vlastnost**: začíná **certifikát** a obsahuje další informace o typu certifikátu hello</span><span class="sxs-lookup"><span data-stu-id="6a6db-161">**Property**: Starts with **Certificate** and contains more information about hello certificate type</span></span>
* <span data-ttu-id="6a6db-162">**Další kroky**: aktualizace certifikátů hello, pokud se blíží vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="6a6db-162">**Next steps**: Update hello certificates if they are near expiration.</span></span>

### <a name="load-capacity-violation"></a><span data-ttu-id="6a6db-163">Načíst porušení kapacity</span><span class="sxs-lookup"><span data-stu-id="6a6db-163">Load capacity violation</span></span>
<span data-ttu-id="6a6db-164">Hello nástroj pro vyrovnávání zatížení Service Fabric hlásí upozornění, když zjistí porušení kapacity uzlu.</span><span class="sxs-lookup"><span data-stu-id="6a6db-164">hello Service Fabric Load Balancer reports a warning when it detects a node capacity violation.</span></span>

* <span data-ttu-id="6a6db-165">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="6a6db-165">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="6a6db-166">**Vlastnost**: začíná **kapacity**</span><span class="sxs-lookup"><span data-stu-id="6a6db-166">**Property**: Starts with **Capacity**</span></span>
* <span data-ttu-id="6a6db-167">**Další kroky**: Kontrola poskytuje metriky a zobrazení hello aktuální kapacity na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-167">**Next steps**: Check provided metrics and view hello current capacity on hello node.</span></span>

## <a name="application-system-health-reports"></a><span data-ttu-id="6a6db-168">Aplikace sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="6a6db-168">Application system health reports</span></span>
<span data-ttu-id="6a6db-169">**System.CM**, který představuje službu hello Správce clusteru, je hello autority, který spravuje informace o aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6a6db-169">**System.CM**, which represents hello Cluster Manager service, is hello authority that manages information about an application.</span></span>

### <a name="state"></a><span data-ttu-id="6a6db-170">Stav</span><span class="sxs-lookup"><span data-stu-id="6a6db-170">State</span></span>
<span data-ttu-id="6a6db-171">System.CM nahlásí jako OK když aplikace hello byly vytvořeny nebo aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="6a6db-171">System.CM reports as OK when hello application has been created or updated.</span></span> <span data-ttu-id="6a6db-172">Informuje hello stavu úložiště po odstranění aplikace hello tak, aby bylo možné odebrat z úložiště.</span><span class="sxs-lookup"><span data-stu-id="6a6db-172">It informs hello health store when hello application has been deleted, so that it can be removed from store.</span></span>

* <span data-ttu-id="6a6db-173">**SourceId**: System.CM</span><span class="sxs-lookup"><span data-stu-id="6a6db-173">**SourceId**: System.CM</span></span>
* <span data-ttu-id="6a6db-174">**Vlastnost**: stavu</span><span class="sxs-lookup"><span data-stu-id="6a6db-174">**Property**: State</span></span>
* <span data-ttu-id="6a6db-175">**Další kroky**: Pokud aplikace hello má byly vytvořeny nebo aktualizovány, měl by obsahovat sestava stavu hello Správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a6db-175">**Next steps**: If hello application has been created or updated, it should include hello Cluster Manager health report.</span></span> <span data-ttu-id="6a6db-176">Zkontrolujte, zda hello se stav aplikace hello vydáním dotazu (například hello rutiny prostředí PowerShell **Get ServiceFabricApplication - ApplicationName *applicationName***).</span><span class="sxs-lookup"><span data-stu-id="6a6db-176">Otherwise, check hello state of hello application by issuing a query (for example, hello PowerShell cmdlet **Get-ServiceFabricApplication -ApplicationName *applicationName***).</span></span>

<span data-ttu-id="6a6db-177">Hello následující příklad ukazuje událost stavu hello na hello **fabric: / WordCount** aplikace:</span><span class="sxs-lookup"><span data-stu-id="6a6db-177">hello following example shows hello state event on hello **fabric:/WordCount** application:</span></span>

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

## <a name="service-system-health-reports"></a><span data-ttu-id="6a6db-178">Služba sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="6a6db-178">Service system health reports</span></span>
<span data-ttu-id="6a6db-179">**System.FM**, který představuje službu Failover Manager hello, hello autoritou, která spravuje informace o službách.</span><span class="sxs-lookup"><span data-stu-id="6a6db-179">**System.FM**, which represents hello Failover Manager service, is hello authority that manages information about services.</span></span>

### <a name="state"></a><span data-ttu-id="6a6db-180">Stav</span><span class="sxs-lookup"><span data-stu-id="6a6db-180">State</span></span>
<span data-ttu-id="6a6db-181">System.FM sestavy jako OK po vytvoření služby hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-181">System.FM reports as OK when hello service has been created.</span></span> <span data-ttu-id="6a6db-182">Odstraní hello entity z hello health store, pokud služba hello byla odstraněna.</span><span class="sxs-lookup"><span data-stu-id="6a6db-182">It deletes hello entity from hello health store when hello service has been deleted.</span></span>

* <span data-ttu-id="6a6db-183">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="6a6db-183">**SourceId**: System.FM</span></span>
* <span data-ttu-id="6a6db-184">**Vlastnost**: stavu</span><span class="sxs-lookup"><span data-stu-id="6a6db-184">**Property**: State</span></span>

<span data-ttu-id="6a6db-185">Hello následující příklad ukazuje hello události stavu služby hello **fabric: / WordCount/WordCountWebService**:</span><span class="sxs-lookup"><span data-stu-id="6a6db-185">hello following example shows hello state event on hello service **fabric:/WordCount/WordCountWebService**:</span></span>

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

### <a name="service-correlation-error"></a><span data-ttu-id="6a6db-186">Chyba korelace služby</span><span class="sxs-lookup"><span data-stu-id="6a6db-186">Service correlation error</span></span>
<span data-ttu-id="6a6db-187">**System.PLB** nahlásí chybu, když zjistí, že aktualizace služby toobe korelační s jinou službu vytvoří řetězec vztahů.</span><span class="sxs-lookup"><span data-stu-id="6a6db-187">**System.PLB** reports an error when it detects that updating a service toobe correlated with another service creates an affinity chain.</span></span> <span data-ttu-id="6a6db-188">Sestava Hello je vymazán poté, co se stane úspěšná aktualizace.</span><span class="sxs-lookup"><span data-stu-id="6a6db-188">hello report is cleared when successful update happens.</span></span>

* <span data-ttu-id="6a6db-189">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="6a6db-189">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="6a6db-190">**Vlastnost**: ServiceDescription</span><span class="sxs-lookup"><span data-stu-id="6a6db-190">**Property**: ServiceDescription</span></span>
* <span data-ttu-id="6a6db-191">**Další kroky**: Kontrola hello korelační popis služby.</span><span class="sxs-lookup"><span data-stu-id="6a6db-191">**Next steps**: Check hello correlated service descriptions.</span></span>

## <a name="partition-system-health-reports"></a><span data-ttu-id="6a6db-192">Oddíl sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="6a6db-192">Partition system health reports</span></span>
<span data-ttu-id="6a6db-193">**System.FM**, který představuje službu Failover Manager hello, hello autoritou, která spravuje informace o oddílech služby.</span><span class="sxs-lookup"><span data-stu-id="6a6db-193">**System.FM**, which represents hello Failover Manager service, is hello authority that manages information about service partitions.</span></span>

### <a name="state"></a><span data-ttu-id="6a6db-194">Stav</span><span class="sxs-lookup"><span data-stu-id="6a6db-194">State</span></span>
<span data-ttu-id="6a6db-195">System.FM nahlásí jako OK když hello oddílu byl vytvořen a je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="6a6db-195">System.FM reports as OK when hello partition has been created and is healthy.</span></span> <span data-ttu-id="6a6db-196">Odstraní hello entity z hello health store při odstranění hello oddílu.</span><span class="sxs-lookup"><span data-stu-id="6a6db-196">It deletes hello entity from hello health store when hello partition is deleted.</span></span>

<span data-ttu-id="6a6db-197">Pokud hello oddílu je menší než počet hello minimální repliky, nahlásí chybu.</span><span class="sxs-lookup"><span data-stu-id="6a6db-197">If hello partition is below hello minimum replica count, it reports an error.</span></span> <span data-ttu-id="6a6db-198">Pokud hello oddíl není nižší než počet hello minimální repliky, ale je nižší než počet replik cíl hello, sestavy upozornění.</span><span class="sxs-lookup"><span data-stu-id="6a6db-198">If hello partition is not below hello minimum replica count, but it is below hello target replica count, it reports a warning.</span></span> <span data-ttu-id="6a6db-199">Pokud hello oddíl je ve ztrátě kvora, System.FM nahlásí chybu.</span><span class="sxs-lookup"><span data-stu-id="6a6db-199">If hello partition is in quorum loss, System.FM reports an error.</span></span>

<span data-ttu-id="6a6db-200">Další důležité události zahrnovat varování hello Rekonfigurace trvá déle, než se očekávalo, a při sestavení hello trvá déle, než se očekávalo.</span><span class="sxs-lookup"><span data-stu-id="6a6db-200">Other important events include a warning when hello reconfiguration takes longer than expected and when hello build takes longer than expected.</span></span> <span data-ttu-id="6a6db-201">dobu Hello očekává hello sestavení a změny konfigurace se dají konfigurovat na základě scénářů služby.</span><span class="sxs-lookup"><span data-stu-id="6a6db-201">hello expected times for hello build and reconfiguration are configurable based on service scenarios.</span></span> <span data-ttu-id="6a6db-202">Například pokud má služba terabajt stavu, například SQL Database, hello sestavení trvá déle než pro službu s malou stavu.</span><span class="sxs-lookup"><span data-stu-id="6a6db-202">For example, if a service has a terabyte of state, such as SQL Database, hello build takes longer than for a service with a small amount of state.</span></span>

* <span data-ttu-id="6a6db-203">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="6a6db-203">**SourceId**: System.FM</span></span>
* <span data-ttu-id="6a6db-204">**Vlastnost**: stavu</span><span class="sxs-lookup"><span data-stu-id="6a6db-204">**Property**: State</span></span>
* <span data-ttu-id="6a6db-205">**Další kroky**: Pokud hello stav není v pořádku, je možné, že některé repliky nebyly vytvořené, otevřenou nebo propagovaných tooprimary nebo sekundární správně.</span><span class="sxs-lookup"><span data-stu-id="6a6db-205">**Next steps**: If hello health state is not OK, it's possible that some replicas have not been created, opened, or promoted tooprimary or secondary correctly.</span></span> <span data-ttu-id="6a6db-206">V mnoha případech je hello příčiny chyb služby v hello otevřít nebo změnit roli implementace.</span><span class="sxs-lookup"><span data-stu-id="6a6db-206">In many instances, hello root cause is a service bug in hello open or change-role implementation.</span></span>

<span data-ttu-id="6a6db-207">Hello následující příklad zobrazuje oddíl v pořádku:</span><span class="sxs-lookup"><span data-stu-id="6a6db-207">hello following example shows a healthy partition:</span></span>

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

<span data-ttu-id="6a6db-208">Hello následující příklad ukazuje stav hello oddíl, který je pod počtu cílových replik.</span><span class="sxs-lookup"><span data-stu-id="6a6db-208">hello following example shows hello health of a partition that is below target replica count.</span></span> <span data-ttu-id="6a6db-209">dalším krokem Hello je tooget hello oddílu popis, který ukazuje, jak jsou nakonfigurované: **MinReplicaSetSize** je tři a **TargetReplicaSetSize** je 7.</span><span class="sxs-lookup"><span data-stu-id="6a6db-209">hello next step is tooget hello partition description, which shows how it is configured: **MinReplicaSetSize** is three and **TargetReplicaSetSize** is seven.</span></span> <span data-ttu-id="6a6db-210">Potom získat hello počet uzlů v clusteru hello: pět.</span><span class="sxs-lookup"><span data-stu-id="6a6db-210">Then get hello number of nodes in hello cluster: five.</span></span> <span data-ttu-id="6a6db-211">Proto v tomto případě dvě repliky nelze umístit, protože hello cílový počet replik je vyšší než hello počet uzlů, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="6a6db-211">So in this case, two replicas can't be placed because hello target number of replicas is higher than hello number of nodes available.</span></span>

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

### <a name="replica-constraint-violation"></a><span data-ttu-id="6a6db-212">Porušení omezení repliky</span><span class="sxs-lookup"><span data-stu-id="6a6db-212">Replica constraint violation</span></span>
<span data-ttu-id="6a6db-213">**System.PLB** sestavy upozornění, pokud zjistí porušení omezení repliky a nelze umístit všechny repliky oddílu.</span><span class="sxs-lookup"><span data-stu-id="6a6db-213">**System.PLB** reports a warning if it detects a replica constraint violation and can't place all partition replicas.</span></span> <span data-ttu-id="6a6db-214">Zobrazí podrobnosti sestavy Hello které omezení a vlastnosti zabránit umístění repliky hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-214">hello report details show which constraints and properties prevent hello replica placement.</span></span>

* <span data-ttu-id="6a6db-215">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="6a6db-215">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="6a6db-216">**Vlastnost**: začíná **ReplicaConstraintViolation**</span><span class="sxs-lookup"><span data-stu-id="6a6db-216">**Property**: Starts with **ReplicaConstraintViolation**</span></span>

## <a name="replica-system-health-reports"></a><span data-ttu-id="6a6db-217">Repliky sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="6a6db-217">Replica system health reports</span></span>
<span data-ttu-id="6a6db-218">**System.RA**, která představuje hello součást reconfiguration agent, je hello autority pro stav repliky hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-218">**System.RA**, which represents hello reconfiguration agent component, is hello authority for hello replica state.</span></span>

### <a name="state"></a><span data-ttu-id="6a6db-219">Stav</span><span class="sxs-lookup"><span data-stu-id="6a6db-219">State</span></span>
<span data-ttu-id="6a6db-220">**System.RA** sestavy OK po vytvoření repliky hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-220">**System.RA** reports OK when hello replica has been created.</span></span>

* <span data-ttu-id="6a6db-221">**SourceId**: System.RA</span><span class="sxs-lookup"><span data-stu-id="6a6db-221">**SourceId**: System.RA</span></span>
* <span data-ttu-id="6a6db-222">**Vlastnost**: stavu</span><span class="sxs-lookup"><span data-stu-id="6a6db-222">**Property**: State</span></span>

<span data-ttu-id="6a6db-223">Hello následující příklad ukazuje repliku v pořádku:</span><span class="sxs-lookup"><span data-stu-id="6a6db-223">hello following example shows a healthy replica:</span></span>

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

### <a name="replica-open-status"></a><span data-ttu-id="6a6db-224">Otevřete stav repliky</span><span class="sxs-lookup"><span data-stu-id="6a6db-224">Replica open status</span></span>
<span data-ttu-id="6a6db-225">Hello popis této sestavy stavu obsahuje čas zahájení hello (Coordinated Universal Time), kdy byl vyvolán volání hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6a6db-225">hello description of this health report contains hello start time (Coordinated Universal Time) when hello API call was invoked.</span></span>

<span data-ttu-id="6a6db-226">**System.RA** sestavy upozornění, pokud hello repliku open trvá déle, než období hello nakonfigurovaný (výchozí: 30 minut).</span><span class="sxs-lookup"><span data-stu-id="6a6db-226">**System.RA** reports a warning if hello replica open takes longer than hello configured period (default: 30 minutes).</span></span> <span data-ttu-id="6a6db-227">Pokud hello API má dopad na dostupnost služby, je vydána hello sestavy mnohem rychleji (Konfigurovat intervalu, výchozí hodnota je 30 sekund).</span><span class="sxs-lookup"><span data-stu-id="6a6db-227">If hello API impacts service availability, hello report is issued much faster (a configurable interval, with a default of 30 seconds).</span></span> <span data-ttu-id="6a6db-228">měření času Hello zahrnuje hello doba hello Replikátor otevřené a služba hello otevřete.</span><span class="sxs-lookup"><span data-stu-id="6a6db-228">hello time measured includes hello time taken for hello replicator open and hello service open.</span></span> <span data-ttu-id="6a6db-229">dokončení Hello vlastnost změny tooOK Pokud hello otevřít.</span><span class="sxs-lookup"><span data-stu-id="6a6db-229">hello property changes tooOK if hello open completes.</span></span>

* <span data-ttu-id="6a6db-230">**SourceId**: System.RA</span><span class="sxs-lookup"><span data-stu-id="6a6db-230">**SourceId**: System.RA</span></span>
* <span data-ttu-id="6a6db-231">**Vlastnost**: **ReplicaOpenStatus**</span><span class="sxs-lookup"><span data-stu-id="6a6db-231">**Property**: **ReplicaOpenStatus**</span></span>
* <span data-ttu-id="6a6db-232">**Další kroky**: Pokud hello stav není v pořádku, zjistěte, proč hello repliku open trvá déle, než se očekávalo.</span><span class="sxs-lookup"><span data-stu-id="6a6db-232">**Next steps**: If hello health state is not OK, investigate why hello replica open takes longer than expected.</span></span>

### <a name="slow-service-api-call"></a><span data-ttu-id="6a6db-233">Pomalá volání rozhraní API služby</span><span class="sxs-lookup"><span data-stu-id="6a6db-233">Slow service API call</span></span>
<span data-ttu-id="6a6db-234">**System.RAP** a **System.Replicator** sestavy upozornění, pokud kód volání toohello uživatele služby trvá déle, než čas hello nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="6a6db-234">**System.RAP** and **System.Replicator** report a warning if a call toohello user service code takes longer than hello configured time.</span></span> <span data-ttu-id="6a6db-235">upozornění Hello je vymazán po dokončení volání hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-235">hello warning is cleared when hello call completes.</span></span>

* <span data-ttu-id="6a6db-236">**SourceId**: System.RAP nebo System.Replicator</span><span class="sxs-lookup"><span data-stu-id="6a6db-236">**SourceId**: System.RAP or System.Replicator</span></span>
* <span data-ttu-id="6a6db-237">**Vlastnost**: název hello hello pomalé rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6a6db-237">**Property**: hello name of hello slow API.</span></span> <span data-ttu-id="6a6db-238">Popis Hello poskytuje další podrobnosti o hello čas hello rozhraní API byla čekající na vyřízení.</span><span class="sxs-lookup"><span data-stu-id="6a6db-238">hello description provides more details about hello time hello API has been pending.</span></span>
* <span data-ttu-id="6a6db-239">**Další kroky**: Zjistěte, proč trvá déle, než bylo očekáváno volání hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-239">**Next steps**: Investigate why hello call takes longer than expected.</span></span>

<span data-ttu-id="6a6db-240">Hello následující příklad zobrazuje oddíl ve ztrátě kvora a toofigure kroky provést šetření hello se důvod, proč.</span><span class="sxs-lookup"><span data-stu-id="6a6db-240">hello following example shows a partition in quorum loss, and hello investigation steps done toofigure out why.</span></span> <span data-ttu-id="6a6db-241">Jedna z repliky hello má stav varování, proto jeho stav.</span><span class="sxs-lookup"><span data-stu-id="6a6db-241">One of hello replicas has a warning health state, so you get its health.</span></span> <span data-ttu-id="6a6db-242">Zobrazuje, že operace hello služby trvá déle, než se očekávalo, událost hlášené System.RAP.</span><span class="sxs-lookup"><span data-stu-id="6a6db-242">It shows that hello service operation takes longer than expected, an event reported by System.RAP.</span></span> <span data-ttu-id="6a6db-243">Po přijetí těchto informací hello dalším krokem je toolook v kódu služby hello a prozkoumat existuje.</span><span class="sxs-lookup"><span data-stu-id="6a6db-243">After this information is received, hello next step is toolook at hello service code and investigate there.</span></span> <span data-ttu-id="6a6db-244">Pro tento případ hello **RunAsync** implementace hello stavové služby, vyvolá k neošetřené výjimce.</span><span class="sxs-lookup"><span data-stu-id="6a6db-244">For this case, hello **RunAsync** implementation of hello stateful service throws an unhandled exception.</span></span> <span data-ttu-id="6a6db-245">Hello repliky jsou recyklace, takže se nemusí zobrazovat všechny repliky ve stavu upozornění hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-245">hello replicas are recycling, so you may not see any replicas in hello warning state.</span></span> <span data-ttu-id="6a6db-246">Můžete opakovat získávání hello stavu a vyhledejte případné rozdíly v ID hello repliky.</span><span class="sxs-lookup"><span data-stu-id="6a6db-246">You can retry getting hello health state and look for any differences in hello replica ID.</span></span> <span data-ttu-id="6a6db-247">V některých případech hello opakování vám může poskytnout různá vodítka.</span><span class="sxs-lookup"><span data-stu-id="6a6db-247">In certain cases, hello retries can give you clues.</span></span>

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

<span data-ttu-id="6a6db-248">Při spuštění hello vadný aplikací v rámci v ladicím programu hello zobrazit hello diagnostických událostí windows hello došlo k výjimce z RunAsync:</span><span class="sxs-lookup"><span data-stu-id="6a6db-248">When you start hello faulty application under hello debugger, hello diagnostic events windows show hello exception thrown from RunAsync:</span></span>

![Visual Studio 2015 diagnostických událostí: selhání RunAsync v fabric: / HelloWorldStatefulApplication.][1]

<span data-ttu-id="6a6db-250">Visual Studio 2015 diagnostických událostí: selhání RunAsync v **fabric: / HelloWorldStatefulApplication**.</span><span class="sxs-lookup"><span data-stu-id="6a6db-250">Visual Studio 2015 diagnostic events: RunAsync failure in **fabric:/HelloWorldStatefulApplication**.</span></span>

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a><span data-ttu-id="6a6db-251">Replikační fronta je plná</span><span class="sxs-lookup"><span data-stu-id="6a6db-251">Replication queue full</span></span>
<span data-ttu-id="6a6db-252">**System.Replicator** nahlásí upozornění, když hello fronta replikací je plná.</span><span class="sxs-lookup"><span data-stu-id="6a6db-252">**System.Replicator** reports a warning when hello replication queue is full.</span></span> <span data-ttu-id="6a6db-253">Na hello primární fronty replikací obvykle plný protože jeden nebo více sekundárních replikách jsou pomalé tooacknowledge operace.</span><span class="sxs-lookup"><span data-stu-id="6a6db-253">On hello primary, replication queue usually becomes full because one or more secondary replicas are slow tooacknowledge operations.</span></span> <span data-ttu-id="6a6db-254">Na hello sekundární to obvykle se stane, když služba hello pomalé tooapply hello operace.</span><span class="sxs-lookup"><span data-stu-id="6a6db-254">On hello secondary, this usually happens when hello service is slow tooapply hello operations.</span></span> <span data-ttu-id="6a6db-255">upozornění Hello je zrušeno při hello už zaplnění fronty.</span><span class="sxs-lookup"><span data-stu-id="6a6db-255">hello warning is cleared when hello queue is no longer full.</span></span>

* <span data-ttu-id="6a6db-256">**SourceId**: System.Replicator</span><span class="sxs-lookup"><span data-stu-id="6a6db-256">**SourceId**: System.Replicator</span></span>
* <span data-ttu-id="6a6db-257">**Vlastnost**: **PrimaryReplicationQueueStatus** nebo **SecondaryReplicationQueueStatus**, v závislosti na role repliky hello</span><span class="sxs-lookup"><span data-stu-id="6a6db-257">**Property**: **PrimaryReplicationQueueStatus** or **SecondaryReplicationQueueStatus**, depending on hello replica role</span></span>

### <a name="slow-naming-operations"></a><span data-ttu-id="6a6db-258">Pomalé operations pojmenování</span><span class="sxs-lookup"><span data-stu-id="6a6db-258">Slow Naming operations</span></span>
<span data-ttu-id="6a6db-259">**System.NamingService** při operaci pojmenování trvá déle než přijatelné hlásí stav na jeho primární repliky.</span><span class="sxs-lookup"><span data-stu-id="6a6db-259">**System.NamingService** reports health on its primary replica when a Naming operation takes longer than acceptable.</span></span> <span data-ttu-id="6a6db-260">Příklady operací pojmenování [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) nebo [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync).</span><span class="sxs-lookup"><span data-stu-id="6a6db-260">Examples of Naming operations are [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) or [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync).</span></span> <span data-ttu-id="6a6db-261">Další metody naleznete v části FabricClient, například v [služby metody správy](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) nebo [metody správy vlastnost](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).</span><span class="sxs-lookup"><span data-stu-id="6a6db-261">More methods can be found under FabricClient, for example under [service management methods](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) or [property management methods](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).</span></span>

> [!NOTE]
> <span data-ttu-id="6a6db-262">Hello Naming service řeší služba názvy tooa umístění v clusteru hello a umožňuje uživatelům toomanage služby názvy a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6a6db-262">hello Naming service resolves service names tooa location in hello cluster and enables users toomanage service names and properties.</span></span> <span data-ttu-id="6a6db-263">Je, že služba jako trvalý, Service Fabric rozdělena na oddíly.</span><span class="sxs-lookup"><span data-stu-id="6a6db-263">It is a Service Fabric partitioned persisted service.</span></span> <span data-ttu-id="6a6db-264">Jeden z oddílů hello představuje hello Authority Owner, který obsahuje metadata o všechny názvy Service Fabric a služeb.</span><span class="sxs-lookup"><span data-stu-id="6a6db-264">One of hello partitions represents hello Authority Owner, which contains metadata about all Service Fabric names and services.</span></span> <span data-ttu-id="6a6db-265">Hello Service Fabric názvy nejsou namapované toodifferent oddíly, označované jako oddíly Name Owner, tak službu hello lze rozšířit.</span><span class="sxs-lookup"><span data-stu-id="6a6db-265">hello Service Fabric names are mapped toodifferent partitions, called Name Owner partitions, so hello service is extensible.</span></span> <span data-ttu-id="6a6db-266">Další informace o [Naming service](service-fabric-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="6a6db-266">Read more about [Naming service](service-fabric-architecture.md).</span></span>
> 
> 

<span data-ttu-id="6a6db-267">Při operaci pojmenování trvá déle, než se očekávalo, operace hello je označené zprávu upozornění o hello *primární repliky hello pojmenování oddílu služby, který slouží hello operaci*.</span><span class="sxs-lookup"><span data-stu-id="6a6db-267">When a Naming operation takes longer than expected, hello operation is flagged with a Warning report on hello *primary replica of hello Naming service partition that serves hello operation*.</span></span> <span data-ttu-id="6a6db-268">Po úspěšném dokončení operace hello hello upozornění není zaškrtnuto.</span><span class="sxs-lookup"><span data-stu-id="6a6db-268">If hello operation completes successfully, hello Warning is cleared.</span></span> <span data-ttu-id="6a6db-269">V případě hello operace skončí s chybou, hello stavu sestava obsahuje podrobnosti o chybě hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-269">If hello operation completes with an error, hello health report includes details about hello error.</span></span>

* <span data-ttu-id="6a6db-270">**SourceId**: System.NamingService</span><span class="sxs-lookup"><span data-stu-id="6a6db-270">**SourceId**: System.NamingService</span></span>
* <span data-ttu-id="6a6db-271">**Vlastnost**: začíná předponu **Duration_** a identifikuje hello pomalé operace a název hello Service Fabric, na které hello operaci použít.</span><span class="sxs-lookup"><span data-stu-id="6a6db-271">**Property**: Starts with prefix **Duration_** and identifies hello slow operation and hello Service Fabric name on which hello operation is applied.</span></span> <span data-ttu-id="6a6db-272">Například pokud vytvořit službu na název fabric: / MyApp/Moje_služba trvá příliš dlouho, vlastnost hello je Duration_AOCreateService.fabric:/MyApp/MyService.</span><span class="sxs-lookup"><span data-stu-id="6a6db-272">For example, if create service at name fabric:/MyApp/MyService takes too long, hello property is Duration_AOCreateService.fabric:/MyApp/MyService.</span></span> <span data-ttu-id="6a6db-273">AO body toohello role hello pojmenování oddílu pro tento název a operaci.</span><span class="sxs-lookup"><span data-stu-id="6a6db-273">AO points toohello role of hello Naming partition for this name and operation.</span></span>
* <span data-ttu-id="6a6db-274">**Další kroky**: Kontrola proč hello pojmenování operace selže.</span><span class="sxs-lookup"><span data-stu-id="6a6db-274">**Next steps**: Check why hello Naming operation fails.</span></span> <span data-ttu-id="6a6db-275">Každé operace může mít různé kořenové příčiny.</span><span class="sxs-lookup"><span data-stu-id="6a6db-275">Each operation can have different root causes.</span></span> <span data-ttu-id="6a6db-276">Například odstranit, může být služba zablokována na uzlu, protože hostitel aplikace hello udržuje chybám na uzlu z důvodu chyb tooa uživatele v kódu služby hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-276">For example, delete service may be stuck on a node because hello application host keeps crashing on a node due tooa user bug in hello service code.</span></span>

<span data-ttu-id="6a6db-277">Hello následující příklad ukazuje operaci vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="6a6db-277">hello following example shows a create service operation.</span></span> <span data-ttu-id="6a6db-278">operace Hello trvalo déle než hello nakonfigurovaná doba trvání.</span><span class="sxs-lookup"><span data-stu-id="6a6db-278">hello operation took longer than hello configured duration.</span></span> <span data-ttu-id="6a6db-279">AO opakování a odešle pracovní tooNO.</span><span class="sxs-lookup"><span data-stu-id="6a6db-279">AO retries and sends work tooNO.</span></span> <span data-ttu-id="6a6db-280">ŽÁDNÉ dokončené hello poslední operaci s vypršením časového limitu.</span><span class="sxs-lookup"><span data-stu-id="6a6db-280">NO completed hello last operation with Timeout.</span></span> <span data-ttu-id="6a6db-281">V takovém případě hello stejné replika je primární hello AO a žádné role.</span><span class="sxs-lookup"><span data-stu-id="6a6db-281">In this case, hello same replica is primary for both hello AO and NO roles.</span></span>

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

## <a name="deployedapplication-system-health-reports"></a><span data-ttu-id="6a6db-282">DeployedApplication sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="6a6db-282">DeployedApplication system health reports</span></span>
<span data-ttu-id="6a6db-283">**System.Hosting** hello autoritou na nasazené entity.</span><span class="sxs-lookup"><span data-stu-id="6a6db-283">**System.Hosting** is hello authority on deployed entities.</span></span>

### <a name="activation"></a><span data-ttu-id="6a6db-284">Aktivace</span><span class="sxs-lookup"><span data-stu-id="6a6db-284">Activation</span></span>
<span data-ttu-id="6a6db-285">System.Hosting sestavy jako OK aplikace byla úspěšně aktivaci v uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-285">System.Hosting reports as OK when an application has been successfully activated on hello node.</span></span> <span data-ttu-id="6a6db-286">V opačném případě nahlásí chybu.</span><span class="sxs-lookup"><span data-stu-id="6a6db-286">Otherwise, it reports an error.</span></span>

* <span data-ttu-id="6a6db-287">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="6a6db-287">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="6a6db-288">**Vlastnost**: aktivace, včetně hello zavedení verze</span><span class="sxs-lookup"><span data-stu-id="6a6db-288">**Property**: Activation, including hello rollout version</span></span>
* <span data-ttu-id="6a6db-289">**Další kroky**: Pokud aplikace hello není v pořádku, zjistěte, proč hello aktivace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="6a6db-289">**Next steps**: If hello application is unhealthy, investigate why hello activation failed.</span></span>

<span data-ttu-id="6a6db-290">Hello následující příklad ukazuje úspěšné aktivaci:</span><span class="sxs-lookup"><span data-stu-id="6a6db-290">hello following example shows successful activation:</span></span>

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

### <a name="download"></a><span data-ttu-id="6a6db-291">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="6a6db-291">Download</span></span>
<span data-ttu-id="6a6db-292">**System.Hosting** nahlásí chybu, pokud stahování balíčku aplikace hello selže.</span><span class="sxs-lookup"><span data-stu-id="6a6db-292">**System.Hosting** reports an error if hello application package download fails.</span></span>

* <span data-ttu-id="6a6db-293">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="6a6db-293">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="6a6db-294">**Vlastnost**:  **stáhnout:*RolloutVersion***</span><span class="sxs-lookup"><span data-stu-id="6a6db-294">**Property**: **Download:*RolloutVersion***</span></span>
* <span data-ttu-id="6a6db-295">**Další kroky**: Zjistěte, proč se nepodařilo stáhnout hello na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-295">**Next steps**: Investigate why hello download failed on hello node.</span></span>

## <a name="deployedservicepackage-system-health-reports"></a><span data-ttu-id="6a6db-296">DeployedServicePackage sestav o stavu systému</span><span class="sxs-lookup"><span data-stu-id="6a6db-296">DeployedServicePackage system health reports</span></span>
<span data-ttu-id="6a6db-297">**System.Hosting** hello autoritou na nasazené entity.</span><span class="sxs-lookup"><span data-stu-id="6a6db-297">**System.Hosting** is hello authority on deployed entities.</span></span>

### <a name="service-package-activation"></a><span data-ttu-id="6a6db-298">Služba aktivace balíčku</span><span class="sxs-lookup"><span data-stu-id="6a6db-298">Service package activation</span></span>
<span data-ttu-id="6a6db-299">System.Hosting sestavy jako OK, pokud je aktivace balíčku hello služby v uzlu hello úspěšná.</span><span class="sxs-lookup"><span data-stu-id="6a6db-299">System.Hosting reports as OK if hello service package activation on hello node is successful.</span></span> <span data-ttu-id="6a6db-300">V opačném případě nahlásí chybu.</span><span class="sxs-lookup"><span data-stu-id="6a6db-300">Otherwise, it reports an error.</span></span>

* <span data-ttu-id="6a6db-301">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="6a6db-301">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="6a6db-302">**Vlastnost**: Aktivace</span><span class="sxs-lookup"><span data-stu-id="6a6db-302">**Property**: Activation</span></span>
* <span data-ttu-id="6a6db-303">**Další kroky**: Zjistěte, proč hello aktivace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="6a6db-303">**Next steps**: Investigate why hello activation failed.</span></span>

### <a name="code-package-activation"></a><span data-ttu-id="6a6db-304">Aktivace balíčku kódu.</span><span class="sxs-lookup"><span data-stu-id="6a6db-304">Code package activation</span></span>
<span data-ttu-id="6a6db-305">**System.Hosting** sestavy jako OK pro každý balíček kódu, pokud je hello aktivace úspěšná.</span><span class="sxs-lookup"><span data-stu-id="6a6db-305">**System.Hosting** reports as OK for each code package if hello activation is successful.</span></span> <span data-ttu-id="6a6db-306">Pokud hello aktivace nepodaří, sestavy upozornění podle konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6a6db-306">If hello activation fails, it reports a warning as configured.</span></span> <span data-ttu-id="6a6db-307">Pokud **CodePackage** selže tooactivate nebo ukončí s chybou větší než nakonfigurovaný hello **CodePackageHealthErrorThreshold**, nahlásí chybu, který je hostitelem.</span><span class="sxs-lookup"><span data-stu-id="6a6db-307">If **CodePackage** fails tooactivate or terminates with an error greater than hello configured **CodePackageHealthErrorThreshold**, hosting reports an error.</span></span> <span data-ttu-id="6a6db-308">Pokud balíček služby obsahuje více balíčků kódu, zprávu o aktivaci se generuje pro každé z nich.</span><span class="sxs-lookup"><span data-stu-id="6a6db-308">If a service package contains multiple code packages, an activation report is generated for each one.</span></span>

* <span data-ttu-id="6a6db-309">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="6a6db-309">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="6a6db-310">**Vlastnost**: Předpona hello používá **CodePackageActivation** a obsahuje název hello balíček kódu hello a hello vstupního bodu jako  **CodePackageActivation:* CodePackageName*:*SetupEntryPoint/EntryPoint*** (například **CodePackageActivation:Code:SetupEntryPoint**)</span><span class="sxs-lookup"><span data-stu-id="6a6db-310">**Property**: Uses hello prefix **CodePackageActivation** and contains hello name of hello code package and hello entry point as **CodePackageActivation:*CodePackageName*:*SetupEntryPoint/EntryPoint*** (for example, **CodePackageActivation:Code:SetupEntryPoint**)</span></span>

### <a name="service-type-registration"></a><span data-ttu-id="6a6db-311">Typ registrace služby</span><span class="sxs-lookup"><span data-stu-id="6a6db-311">Service type registration</span></span>
<span data-ttu-id="6a6db-312">**System.Hosting** sestavy jako OK, pokud typ služby hello byl úspěšně zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="6a6db-312">**System.Hosting** reports as OK if hello service type has been registered successfully.</span></span> <span data-ttu-id="6a6db-313">Nahlásí chybu, pokud nebyla provedena registrace hello v čase (jak je nakonfigurovat pomocí **ServiceTypeRegistrationTimeout**).</span><span class="sxs-lookup"><span data-stu-id="6a6db-313">It reports an error if hello registration wasn't done in time (as configured by using **ServiceTypeRegistrationTimeout**).</span></span> <span data-ttu-id="6a6db-314">Pokud je zavřená hello runtime, typ služby hello je odregistrovat z uzlu hello a hostitelský oznámí upozornění.</span><span class="sxs-lookup"><span data-stu-id="6a6db-314">If hello runtime is closed, hello service type is unregistered from hello node and Hosting reports a warning.</span></span>

* <span data-ttu-id="6a6db-315">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="6a6db-315">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="6a6db-316">**Vlastnost**: Předpona hello používá **ServiceTypeRegistration** a obsahuje název typu služby hello (například **ServiceTypeRegistration:FileStoreServiceType**)</span><span class="sxs-lookup"><span data-stu-id="6a6db-316">**Property**: Uses hello prefix **ServiceTypeRegistration** and contains hello service type name (for example, **ServiceTypeRegistration:FileStoreServiceType**)</span></span>

<span data-ttu-id="6a6db-317">Hello následující příklad ukazuje v pořádku nasazený balíček služby:</span><span class="sxs-lookup"><span data-stu-id="6a6db-317">hello following example shows a healthy deployed service package:</span></span>

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

### <a name="download"></a><span data-ttu-id="6a6db-318">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="6a6db-318">Download</span></span>
<span data-ttu-id="6a6db-319">**System.Hosting** nahlásí chybu, pokud stahování balíčku služby hello selže.</span><span class="sxs-lookup"><span data-stu-id="6a6db-319">**System.Hosting** reports an error if hello service package download fails.</span></span>

* <span data-ttu-id="6a6db-320">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="6a6db-320">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="6a6db-321">**Vlastnost**:  **stáhnout:*RolloutVersion***</span><span class="sxs-lookup"><span data-stu-id="6a6db-321">**Property**: **Download:*RolloutVersion***</span></span>
* <span data-ttu-id="6a6db-322">**Další kroky**: Zjistěte, proč se nepodařilo stáhnout hello na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-322">**Next steps**: Investigate why hello download failed on hello node.</span></span>

### <a name="upgrade-validation"></a><span data-ttu-id="6a6db-323">Ověření upgradu</span><span class="sxs-lookup"><span data-stu-id="6a6db-323">Upgrade validation</span></span>
<span data-ttu-id="6a6db-324">**System.Hosting** nahlásí chybu, pokud selže ověření během upgradu hello nebo pokud hello upgrade selže na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="6a6db-324">**System.Hosting** reports an error if validation during hello upgrade fails or if hello upgrade fails on hello node.</span></span>

* <span data-ttu-id="6a6db-325">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="6a6db-325">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="6a6db-326">**Vlastnost**: Předpona hello používá **FabricUpgradeValidation** a obsahuje hello upgradu verze</span><span class="sxs-lookup"><span data-stu-id="6a6db-326">**Property**: Uses hello prefix **FabricUpgradeValidation** and contains hello upgrade version</span></span>
* <span data-ttu-id="6a6db-327">**Popis**: body toohello došlo k chybě</span><span class="sxs-lookup"><span data-stu-id="6a6db-327">**Description**: Points toohello error encountered</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a6db-328">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a6db-328">Next steps</span></span>
[<span data-ttu-id="6a6db-329">Zobrazit sestavy stavu Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6a6db-329">View Service Fabric health reports</span></span>](service-fabric-view-entities-aggregated-health.md)

[<span data-ttu-id="6a6db-330">Jak tooreport a zkontrolujte stav služby</span><span class="sxs-lookup"><span data-stu-id="6a6db-330">How tooreport and check service health</span></span>](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[<span data-ttu-id="6a6db-331">Monitorování a Diagnostika služby místně</span><span class="sxs-lookup"><span data-stu-id="6a6db-331">Monitor and diagnose services locally</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="6a6db-332">Upgrade aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6a6db-332">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

