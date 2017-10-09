---
title: "aaaHigh dostupnosti a zotavení po havárii systému SAP HANA v Azure (velké instance) | Microsoft Docs"
description: "Vytvoření vysoké dostupnosti a plán pro zotavení po havárii SAP HANA v Azure (velké instance)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0c0967f54cf29bbb275eb7cda9d36608488add9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a><span data-ttu-id="63f5a-103">SAP HANA (velké instance) vysoké dostupnosti a zotavení po havárii v Azure</span><span class="sxs-lookup"><span data-stu-id="63f5a-103">SAP HANA (large instances) high availability and disaster recovery on Azure</span></span> 

<span data-ttu-id="63f5a-104">Vysoká dostupnost a zotavení po havárii jsou důležité aspekty vaše důležité SAP HANA spuštěných na serverech Azure (velké instance).</span><span class="sxs-lookup"><span data-stu-id="63f5a-104">High availability and disaster recovery are important aspects of running your mission-critical SAP HANA on Azure (large instances) servers.</span></span> <span data-ttu-id="63f5a-105">Jeho důležité toowork s SAP, vaše systémový Integrátor nebo architekt tooproperly Microsoft a implementujte hello strategie právo vysokou dostupnosti nebo zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="63f5a-105">It's important toowork with SAP, your system integrator, or Microsoft tooproperly architect and implement hello right high-availability/disaster-recovery strategy.</span></span> <span data-ttu-id="63f5a-106">Je také plánovaného bodu obnovení hello důležité tooconsider a cíli času obnovení, které jsou specifické tooyour prostředí.</span><span class="sxs-lookup"><span data-stu-id="63f5a-106">It is also important tooconsider hello recovery point objective and recovery time objective, which are specific tooyour environment.</span></span>

## <a name="high-availability"></a><span data-ttu-id="63f5a-107">Vysoká dostupnost</span><span class="sxs-lookup"><span data-stu-id="63f5a-107">High availability</span></span>

<span data-ttu-id="63f5a-108">Společnost Microsoft podporuje SAP HANA vysokou dostupnost metody "mimo pole hello", mezi které patří:</span><span class="sxs-lookup"><span data-stu-id="63f5a-108">Microsoft supports SAP HANA high-availability methods "out of hello box," which include:</span></span>

- <span data-ttu-id="63f5a-109">**Replikace úložiště:** hello systému úložiště možnost tooreplicate všechny tooanother umístění dat (v rámci, nebo odděleně od, hello stejného datového centra).</span><span class="sxs-lookup"><span data-stu-id="63f5a-109">**Storage replication:** hello storage system's ability tooreplicate all data tooanother location (within, or separate from, hello same datacenter).</span></span> <span data-ttu-id="63f5a-110">SAP HANA funguje nezávisle na tuto metodu.</span><span class="sxs-lookup"><span data-stu-id="63f5a-110">SAP HANA operates independently of this method.</span></span>
- <span data-ttu-id="63f5a-111">**Replikace systému HANA:** hello replikace všech dat v samostatném systému SAP HANA SAP HANA tooa.</span><span class="sxs-lookup"><span data-stu-id="63f5a-111">**HANA system replication:** hello replication of all data in SAP HANA tooa separate SAP HANA system.</span></span> <span data-ttu-id="63f5a-112">plánovanou dobu obnovení Hello je minimalizován prostřednictvím replikace dat v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="63f5a-112">hello recovery time objective is minimized through data replication at regular intervals.</span></span> <span data-ttu-id="63f5a-113">SAP HANA podporuje asynchronní, synchronní režimy v paměti a synchronní (doporučeno pouze pro SAP HANA hello systémy, které jsou ve stejném datovém centru nebo méně než 100 KM od sebe).</span><span class="sxs-lookup"><span data-stu-id="63f5a-113">SAP HANA supports asynchronous, synchronous in-memory, and synchronous modes (recommended only for SAP HANA systems that are within hello same datacenter or less than 100 KM apart).</span></span> <span data-ttu-id="63f5a-114">V aktuální návrhu hello HANA velké instance razítek, která replikaci systému HANA slouží pouze pro vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="63f5a-114">In hello current design of HANA large-instance stamps, HANA system replication can be used for high availability only.</span></span>
- <span data-ttu-id="63f5a-115">**Automatické převzetí služeb při selhání hostitele:** toouse řešení místní selhání obnovení jako alternativní toosystem replikace.</span><span class="sxs-lookup"><span data-stu-id="63f5a-115">**Host auto-failover:** A local fault-recovery solution toouse as an alternative toosystem replication.</span></span> <span data-ttu-id="63f5a-116">Pokud hlavní uzel hello stane nedostupným, jsou v režimu Škálováním na více systémů nakonfigurovat jeden nebo více uzlů SAP HANA pohotovostní a SAP HANA automaticky převezme tooanother uzlu.</span><span class="sxs-lookup"><span data-stu-id="63f5a-116">When hello master node becomes unavailable, one or more standby SAP HANA nodes are configured in scale-out mode and SAP HANA automatically fails over tooanother node.</span></span>

<span data-ttu-id="63f5a-117">Další informace o vysoké dostupnosti SAP HANA najdete následující informace SAP hello:</span><span class="sxs-lookup"><span data-stu-id="63f5a-117">For more information on SAP HANA high availability, see hello following SAP information:</span></span>

- [<span data-ttu-id="63f5a-118">SAP HANA vysokou dostupnost dokument White Paper</span><span class="sxs-lookup"><span data-stu-id="63f5a-118">SAP HANA High-Availability Whitepaper</span></span>](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [<span data-ttu-id="63f5a-119">Příručka pro správu SAP HANA</span><span class="sxs-lookup"><span data-stu-id="63f5a-119">SAP HANA Administration Guide</span></span>](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [<span data-ttu-id="63f5a-120">SAP Academy Video o replikaci systému SAP HANA</span><span class="sxs-lookup"><span data-stu-id="63f5a-120">SAP Academy Video on SAP HANA System Replication</span></span>](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [<span data-ttu-id="63f5a-121">SAP podporu Poznámka #1999880 – nejčastější dotazy na replikaci systému SAP HANA</span><span class="sxs-lookup"><span data-stu-id="63f5a-121">SAP Support Note #1999880 – FAQ on SAP HANA System Replication</span></span>](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- <span data-ttu-id="63f5a-122">[SAP podporu Poznámka #2165547 – SAP HANA zálohování a obnovení v rámci prostředí replikace systému SAP HANA](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)</span><span class="sxs-lookup"><span data-stu-id="63f5a-122">[SAP Support Note #2165547 – SAP HANA Backup and Restore within SAP HANA System Replication Environment](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)</span></span>
- <span data-ttu-id="63f5a-123">[Pro Exchange hardwaru minimální nebo nula. výpadků SAP podporu Poznámka #1984882 – pomocí replikace systému SAP HANA](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)</span><span class="sxs-lookup"><span data-stu-id="63f5a-123">[SAP Support Note #1984882 – Using SAP HANA System Replication for Hardware Exchange with Minimum/Zero Downtime](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)</span></span>

## <a name="disaster-recovery"></a><span data-ttu-id="63f5a-124">Zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="63f5a-124">Disaster recovery</span></span>

<span data-ttu-id="63f5a-125">SAP HANA v Azure (velké instance) je k dispozici v dvě Azure oblasti v geopolitické oblasti.</span><span class="sxs-lookup"><span data-stu-id="63f5a-125">SAP HANA on Azure (large instances) is offered in two Azure regions in a geopolitical region.</span></span> <span data-ttu-id="63f5a-126">Mezi hello dva velké instance razítka dvou různých oblastí je přímého síťového připojení pro replikaci dat během zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="63f5a-126">Between hello two large-instance stamps of two different regions is a direct network connectivity for replicating data during disaster recovery.</span></span> <span data-ttu-id="63f5a-127">Hello replikaci dat hello je na základě infrastruktury úložiště.</span><span class="sxs-lookup"><span data-stu-id="63f5a-127">hello replication of hello data is storage-infrastructure based.</span></span> <span data-ttu-id="63f5a-128">ve výchozím nastavení není provádí replikace Hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-128">hello replication is not done by default.</span></span> <span data-ttu-id="63f5a-129">Dokončí hello zákazníka konfigurace, které seřazené zotavení po havárii hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-129">It is done for hello customer configurations that ordered hello disaster recovery.</span></span> <span data-ttu-id="63f5a-130">V aktuální návrhu hello nelze použít replikaci HANA systému pro zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="63f5a-130">In hello current design, HANA system replication can't be used for disaster recovery.</span></span>

<span data-ttu-id="63f5a-131">Však využít tootake hello zotavení po havárii, budete potřebovat toostart toodesign hello síťové připojení toohello dvou různých oblastech Azure.</span><span class="sxs-lookup"><span data-stu-id="63f5a-131">However, tootake advantage of hello disaster recovery, you need toostart toodesign hello network connectivity toohello two different Azure regions.</span></span> <span data-ttu-id="63f5a-132">toodo tedy potřebujete připojení okruh Azure ExpressRoute z místního ve vaši hlavní oblast Azure a jiné okruh připojení z místní tooyour zotavení po havárii oblasti.</span><span class="sxs-lookup"><span data-stu-id="63f5a-132">toodo so, you need an Azure ExpressRoute circuit connection from on-premises in your main Azure region and another circuit connection from on-premises tooyour disaster-recovery region.</span></span> <span data-ttu-id="63f5a-133">Tato míra by mělo obsahovat situace, ve kterém k dokončení oblasti Azure, včetně umístění směrovače (MSEE) Microsoft enterprise edge, má problém.</span><span class="sxs-lookup"><span data-stu-id="63f5a-133">This measure would cover a situation in which a complete Azure region, including a Microsoft enterprise edge router (MSEE) location, has an issue.</span></span>

<span data-ttu-id="63f5a-134">Jako druhá míra se můžete připojit virtuální sítě Azure, které se připojují tooSAP HANA v Azure (velké instance) v jedné oblasti tooboth hello těchto okruhů ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="63f5a-134">As a second measure, you can connect all Azure virtual networks that connect tooSAP HANA on Azure (large instances) in one of hello regions tooboth of those ExpressRoute circuits.</span></span> <span data-ttu-id="63f5a-135">Tato míra řeší případ, kdy jenom jeden hello MSEE umístění, které se připojí vaše místní umístění pomocí Azure ocitne mimo službu.</span><span class="sxs-lookup"><span data-stu-id="63f5a-135">This measure addresses a case where only one of hello MSEE locations that connects your on-premises location with Azure goes off duty.</span></span>

<span data-ttu-id="63f5a-136">Hello následující obrázek znázorňuje hello optimální konfigurace pro zotavení po havárii:</span><span class="sxs-lookup"><span data-stu-id="63f5a-136">hello following figure shows hello optimal configuration for disaster recovery:</span></span>

![Optimální konfigurace pro zotavení po havárii](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)

<span data-ttu-id="63f5a-138">Hello optimální případ zotavení po havárii konfigurace sítě hello je toohave dvou okruhů ExpressRoute z místní toohello dvou různých oblastech Azure.</span><span class="sxs-lookup"><span data-stu-id="63f5a-138">hello optimal case for a disaster-recovery configuration of hello network is toohave two ExpressRoute circuits from on-premises toohello two different Azure regions.</span></span> <span data-ttu-id="63f5a-139">Jeden okruh přejde tooregion #1 spuštěna instance produkční.</span><span class="sxs-lookup"><span data-stu-id="63f5a-139">One circuit goes tooregion #1, running a production instance.</span></span> <span data-ttu-id="63f5a-140">druhý okruh ExpressRoute Hello přejde tooregion #2, spuštěné některé instance HANA nevýrobní prostředí.</span><span class="sxs-lookup"><span data-stu-id="63f5a-140">hello second ExpressRoute circuit goes tooregion #2, running some non-production HANA instances.</span></span> <span data-ttu-id="63f5a-141">(To je důležité, pokud celou oblast Azure, včetně hello MSEE a velkých instance razítka, přejde vypnout hello mřížky.)</span><span class="sxs-lookup"><span data-stu-id="63f5a-141">(This is important if an entire Azure region, including hello MSEE and large-instance stamp, goes off hello grid.)</span></span>

<span data-ttu-id="63f5a-142">Jako druhá míra hello různých virtuálních sítí jsou připojené toohello různé okruhy ExpressRoute, které jsou připojené tooSAP HANA v Azure (velké instance).</span><span class="sxs-lookup"><span data-stu-id="63f5a-142">As a second measure, hello various virtual networks are connected toohello various ExpressRoute circuits that are connected tooSAP HANA on Azure (large instances).</span></span> <span data-ttu-id="63f5a-143">Hello místo, kde MSEE selhává, nebo můžete snížit hello plánovaného bodu obnovení pro zotavení po havárii můžete obejít probereme později.</span><span class="sxs-lookup"><span data-stu-id="63f5a-143">You can bypass hello location where an MSEE is failing, or you can lower hello recovery point objective for disaster recovery, as we discuss later.</span></span>

<span data-ttu-id="63f5a-144">Hello další požadavky pro instalaci zotavení po havárii jsou:</span><span class="sxs-lookup"><span data-stu-id="63f5a-144">hello next requirements for a disaster-recovery setup are:</span></span>

- <span data-ttu-id="63f5a-145">SAP HANA musí pořadí v Azure (velké instance) SKU systému hello stejnou velikost jako provozním SKU a nasaďte je do oblasti hello zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="63f5a-145">You must order SAP HANA on Azure (large instances) SKUs of hello same size as your production SKUs and deploy them in hello disaster-recovery region.</span></span> <span data-ttu-id="63f5a-146">Tato instance může být použité toorun test, izolovaného prostoru nebo QA HANA instance.</span><span class="sxs-lookup"><span data-stu-id="63f5a-146">These instances can be used toorun test, sandbox, or QA HANA instances.</span></span>
- <span data-ttu-id="63f5a-147">Musíte uspořádat profil zotavení po havárii pro každou z vaší SAP HANA na SKU Azure (velké instance), které chcete toorecover v lokalitě hello zotavení po havárii, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="63f5a-147">You must order a disaster-recovery profile for each of your SAP HANA on Azure (large instances) SKUs that you want toorecover in hello disaster-recovery site, if necessary.</span></span> <span data-ttu-id="63f5a-148">Tato akce vede toohello přidělení úložiště svazky, které jsou cílem hello hello replikace úložiště z vaší oblasti produkční do oblasti hello zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="63f5a-148">This action leads toohello allocation of storage volumes, which are hello target of hello storage replication from your production region into hello disaster-recovery region.</span></span>

<span data-ttu-id="63f5a-149">Po hello předcházející požadavky splňujete, je vaší zodpovědností toostart hello úložiště replikace.</span><span class="sxs-lookup"><span data-stu-id="63f5a-149">After you meet hello preceding requirements, it is your responsibility toostart hello storage replication.</span></span> <span data-ttu-id="63f5a-150">V infrastruktuře úložiště hello používá pro SAP HANA v Azure (velké instance) je základ hello replikace úložiště snímků úložiště.</span><span class="sxs-lookup"><span data-stu-id="63f5a-150">In hello storage infrastructure used for SAP HANA on Azure (large instances), hello basis of storage replication is storage snapshots.</span></span> <span data-ttu-id="63f5a-151">toostart hello zotavení po havárii replikace, je třeba tooperform:</span><span class="sxs-lookup"><span data-stu-id="63f5a-151">toostart hello disaster-recovery replication, you need tooperform:</span></span>

- <span data-ttu-id="63f5a-152">Snímek spouštěcích logické jednotky, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="63f5a-152">A snapshot of your boot LUN, as described earlier.</span></span>
- <span data-ttu-id="63f5a-153">Snímek související HANA svazků, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="63f5a-153">A snapshot of your HANA-related volumes, as described earlier.</span></span>

<span data-ttu-id="63f5a-154">Po provedení tyto snímky počáteční replika hello svazky se nasadí na hello svazky, které jsou spojeny s profilem zotavení po havárii v oblasti hello zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="63f5a-154">After you execute these snapshots, an initial replica of hello volumes is seeded on hello volumes that are associated with your disaster-recovery profile in hello disaster-recovery region.</span></span>

<span data-ttu-id="63f5a-155">Následně se nejnovější úložiště snímků hello používá každou hodinu tooreplicate hello rozdílů, které vývoji na hello svazky úložiště.</span><span class="sxs-lookup"><span data-stu-id="63f5a-155">Subsequently, hello most recent storage snapshot is used every hour tooreplicate hello deltas that develop on hello storage volumes.</span></span>

<span data-ttu-id="63f5a-156">Hello plánovaného bodu obnovení, které je dosaženo pomocí této konfigurace je 60 minut too90.</span><span class="sxs-lookup"><span data-stu-id="63f5a-156">hello recovery point objective that's achieved with this configuration is from 60 too90 minutes.</span></span> <span data-ttu-id="63f5a-157">tooachieve lepší obnovení bodu v případě zotavení po havárii hello, kopie hello HANA zálohování transakčního protokolu ze SAP HANA na Azure (velké instance) toohello cíl jiné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="63f5a-157">tooachieve a better recovery point objective in hello disaster-recovery case, copy hello HANA transaction log backups from SAP HANA on Azure (large instances) toohello other Azure region.</span></span> <span data-ttu-id="63f5a-158">tooachieve tento cíl bodu obnovení hello následující:</span><span class="sxs-lookup"><span data-stu-id="63f5a-158">tooachieve this recovery point objective, do hello following:</span></span>

1. <span data-ttu-id="63f5a-159">Zálohování hello HANA transakce se přihlaste jako často možném příliš/hana / / zálohy protokolu.</span><span class="sxs-lookup"><span data-stu-id="63f5a-159">Back up hello HANA transaction log as frequently as possible too/hana/log/backup.</span></span>
2. <span data-ttu-id="63f5a-160">Zkopírujte hello zálohování transakčního protokolu, po dokončení tooan Azure virtuální počítač (VM), který je ve virtuální síti, která je připojena toohello SAP HANA na serveru Azure (velké instance).</span><span class="sxs-lookup"><span data-stu-id="63f5a-160">Copy hello transaction log backups when they are finished tooan Azure virtual machine (VM), which is in a virtual network that's connected toohello SAP HANA on Azure (large instances) server.</span></span>
3. <span data-ttu-id="63f5a-161">Z tohoto virtuálního počítače zkopírujte hello zálohování tooa virtuální počítač, který je ve virtuální síti v oblasti hello zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="63f5a-161">From that VM, copy hello backup tooa VM that's in a virtual network in hello disaster-recovery region.</span></span>
4. <span data-ttu-id="63f5a-162">V této oblasti v hello virtuálního počítače zachovat hello transakce zálohy protokolu.</span><span class="sxs-lookup"><span data-stu-id="63f5a-162">Keep hello transaction log backups in that region in hello VM.</span></span>

<span data-ttu-id="63f5a-163">V případě havárie, po hello zotavení po havárii profil nasazený na skutečné serveru, zkopírujte zálohování transakčního protokolu hello z virtuálních počítačů toohello hello SAP HANA v Azure (velké instance), který je teď primárním serverem hello v oblasti hello zotavení po havárii, a Tyto zálohy obnovte.</span><span class="sxs-lookup"><span data-stu-id="63f5a-163">In case of disaster, after hello disaster-recovery profile has been deployed on an actual server, copy hello transaction log backups from hello VM toohello SAP HANA on Azure (large instances) that is now hello primary server in hello disaster-recovery region, and restore those backups.</span></span> <span data-ttu-id="63f5a-164">Toto obnovení je možné, protože stav hello HANA na discích hello zotavení po havárii je, že HANA snímku.</span><span class="sxs-lookup"><span data-stu-id="63f5a-164">This recovery is possible because hello state of HANA on hello disaster-recovery disks is that of a HANA snapshot.</span></span> <span data-ttu-id="63f5a-165">Toto je hello posunutí bod pro další obnovení zálohy protokolu transakcí.</span><span class="sxs-lookup"><span data-stu-id="63f5a-165">This is hello offset point for further restorations of transaction log backups.</span></span>

## <a name="backup-and-restore"></a><span data-ttu-id="63f5a-166">Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="63f5a-166">Backup and restore</span></span>

<span data-ttu-id="63f5a-167">Jeden z hello nejdůležitější aspekty toooperating databáze se ujistěte se, že hello databáze se dají chránit z různých závažné události.</span><span class="sxs-lookup"><span data-stu-id="63f5a-167">One of hello most important aspects toooperating databases is making sure hello database can be protected from various catastrophic events.</span></span> <span data-ttu-id="63f5a-168">Tyto události může být způsobeno nic z přírodní katastrofy toosimple uživatele chyb.</span><span class="sxs-lookup"><span data-stu-id="63f5a-168">These events can be caused by anything from natural disasters toosimple user errors.</span></span>

<span data-ttu-id="63f5a-169">Zálohování databáze, s hello možnost toorestore ho tooany bodu v čase (například před odstraněním někdo důležitá data), umožňuje obnovení tooa stavu, který je jako zavřete jako možným způsobem toohello bylo dříve, než dojde k přerušení hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-169">Backing up a database, with hello ability toorestore it tooany point in time (such as before somebody deleted critical data), allows restoration tooa state that is as close as possible toohello way it was before hello disruption occurred.</span></span>

<span data-ttu-id="63f5a-170">Pro dosažení co nejlepších výsledků se musí provádět dva typy zálohování:</span><span class="sxs-lookup"><span data-stu-id="63f5a-170">Two types of backups must be performed for best results:</span></span>

- <span data-ttu-id="63f5a-171">Zálohování databáze</span><span class="sxs-lookup"><span data-stu-id="63f5a-171">Database backups</span></span>
- <span data-ttu-id="63f5a-172">Zálohování transakčního protokolu</span><span class="sxs-lookup"><span data-stu-id="63f5a-172">Transaction log backups</span></span>

<span data-ttu-id="63f5a-173">Kromě zálohování databáze toofull provést na úrovni aplikace, můžete být i důkladnější provedením zálohy snímků úložiště.</span><span class="sxs-lookup"><span data-stu-id="63f5a-173">In addition toofull database backups performed at an application-level, you can be even more thorough by performing backups with storage snapshots.</span></span> <span data-ttu-id="63f5a-174">Provádění záloh protokolu je také důležité pro obnovení databáze hello (a tooempty hello protokoly z již potvrzené transakce).</span><span class="sxs-lookup"><span data-stu-id="63f5a-174">Performing log backups is also important for restoring hello database (and tooempty hello logs from already committed transactions).</span></span>

<span data-ttu-id="63f5a-175">SAP HANA v Azure (velké instance) nabízí dvě možnosti pro zálohování a obnovení:</span><span class="sxs-lookup"><span data-stu-id="63f5a-175">SAP HANA on Azure (large instances) offers two backup and restore options:</span></span>

- <span data-ttu-id="63f5a-176">Provést sami (DIY).</span><span class="sxs-lookup"><span data-stu-id="63f5a-176">Do it yourself (DIY).</span></span> <span data-ttu-id="63f5a-177">Po výpočtu tooensure dostatek místa na disku, proveďte úplné zálohování databáze a protokolu pomocí metody zálohování na disk (toothose disky).</span><span class="sxs-lookup"><span data-stu-id="63f5a-177">After you calculate tooensure enough disk space, perform full database and log backups by using disk backup methods (toothose disks).</span></span> <span data-ttu-id="63f5a-178">V průběhu času hello zálohy jsou zkopírované tooan účtu úložiště Azure (po nastavení Azure souborovém serveru s prakticky neomezené úložiště) nebo použít trezoru zálohování Azure nebo úložiště Azure uloží málo používaná data.</span><span class="sxs-lookup"><span data-stu-id="63f5a-178">Over time, hello backups are copied tooan Azure storage account (after you set up an Azure-based file server with virtually unlimited storage), or use an Azure Backup vault or Azure cold storage.</span></span> <span data-ttu-id="63f5a-179">Další možností je toouse nástroj ochrany dat třetích stran, jako je Commvault toostore hello zálohy, jakmile jsou zkopírovány tooa účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="63f5a-179">Another option is toouse a third-party data protection tool, such as Commvault, toostore hello backups after they are copied tooa storage account.</span></span> <span data-ttu-id="63f5a-180">Hello DIY možnost zálohování může být také nutné pro data, která potřebují toobe uložené delší dobu pro dodržování předpisů a auditování.</span><span class="sxs-lookup"><span data-stu-id="63f5a-180">hello DIY backup option might also be necessary for data that needs toobe stored for longer periods for compliancy and auditing purposes.</span></span>
- <span data-ttu-id="63f5a-181">Použijte hello zálohování a obnovení funkce, které hello podpůrné infrastruktuře SAP HANA v Azure (velké instance) poskytuje.</span><span class="sxs-lookup"><span data-stu-id="63f5a-181">Use hello backup and restore functionality that hello underlying infrastructure of SAP HANA on Azure (large instances) provides.</span></span> <span data-ttu-id="63f5a-182">Tato možnost plnit hello potřebu zálohování, a umožňuje téměř zastaralé (s výjimkou případů, kde jsou vyžadovány pro dodržování předpisů pro účely zálohování dat) ručního zálohování.</span><span class="sxs-lookup"><span data-stu-id="63f5a-182">This option fulfills hello need for backups, and it makes manual backups nearly obsolete (except where data backups are required for compliance purposes).</span></span> <span data-ttu-id="63f5a-183">Hello zbytek této části adresy hello zálohování a obnovení funkce, které nabízí s HANA (velké instance).</span><span class="sxs-lookup"><span data-stu-id="63f5a-183">hello rest of this section addresses hello backup and restore functionality that's offered with HANA (large instances).</span></span>

> [!NOTE]
> <span data-ttu-id="63f5a-184">Hello snímku technologie, která je používána hello základní infrastruktury HANA (velké instance) má závislost na snímky SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="63f5a-184">hello snapshot technology that is used by hello underlying infrastructure of HANA (large instances) has a dependency on SAP HANA snapshots.</span></span> <span data-ttu-id="63f5a-185">Snímky SAP HANA ve spojení s SAP HANA víceklientské databáze kontejnery nefungují.</span><span class="sxs-lookup"><span data-stu-id="63f5a-185">SAP HANA snapshots do not work in conjunction with SAP HANA Multitenant Database Containers.</span></span> <span data-ttu-id="63f5a-186">Tato metoda zálohování v důsledku toho nelze použít toodeploy SAP HANA víceklientské databáze kontejnery.</span><span class="sxs-lookup"><span data-stu-id="63f5a-186">As a result, this method of backup cannot be used toodeploy SAP HANA Multitenant Database Containers.</span></span>

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a><span data-ttu-id="63f5a-187">Pomocí snímků úložiště SAP HANA v Azure (velké instance)</span><span class="sxs-lookup"><span data-stu-id="63f5a-187">Using storage snapshots of SAP HANA on Azure (large instances)</span></span>

<span data-ttu-id="63f5a-188">infrastruktura úložiště Hello základní SAP HANA v Azure (velké instance) podporuje hello představu o úložiště snímků svazků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-188">hello storage infrastructure underlying SAP HANA on Azure (large instances) supports hello notion of a storage snapshot of volumes.</span></span> <span data-ttu-id="63f5a-189">Zálohování a obnovení pro určitý svazek, podporují s hello následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="63f5a-189">Both backup and restoration of a particular volume are supported, with hello following considerations:</span></span>

- <span data-ttu-id="63f5a-190">Místo zálohy databáze jsou na základě časté prováděné úložiště snímků svazků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-190">Instead of database backups, storage volume snapshots are taken on a frequent basis.</span></span>
- <span data-ttu-id="63f5a-191">Hello úložiště snímků zahájí SAP HANA snímku, než se provede hello úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-191">hello storage snapshot initiates an SAP HANA snapshot before it executes hello storage snapshot.</span></span> <span data-ttu-id="63f5a-192">Tento snímek SAP HANA je bod hello instalační program pro případné protokolu obnovení po obnovení snímku úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-192">This SAP HANA snapshot is hello setup point for eventual log restorations after recovery of hello storage snapshot.</span></span>
- <span data-ttu-id="63f5a-193">V okamžiku hello kde hello úložiště snímků je proveden úspěšně je odstraněn hello SAP HANA snímek.</span><span class="sxs-lookup"><span data-stu-id="63f5a-193">At hello point where hello storage snapshot is executed successfully, hello SAP HANA snapshot is deleted.</span></span>
- <span data-ttu-id="63f5a-194">Zálohy protokolů jsou často prováděné a uložená v hello protokolu zálohování svazku nebo v Azure.</span><span class="sxs-lookup"><span data-stu-id="63f5a-194">Log backups are taken frequently and stored in hello log backup volume or in Azure.</span></span>
- <span data-ttu-id="63f5a-195">Pokud musíte obnovit databázi hello tooa určité bodu v čase, požadavku tooMicrosoft podporu Azure (produkční výpadek) nebo SAP HANA na Azure Service Management toorestore tooa určité úložiště snímků (například o plánované obnovení systému izolovaného prostoru tooits původního stavu).</span><span class="sxs-lookup"><span data-stu-id="63f5a-195">If hello database must be restored tooa certain point in time, a request is made tooMicrosoft Azure Support (production outage) or SAP HANA on Azure Service Management toorestore tooa certain storage snapshot (for example, a planned restoration of a sandbox system tooits original state).</span></span>
- <span data-ttu-id="63f5a-196">Hello SAP HANA snímek, který je součástí hello úložiště snímků je bod posunutí pro použití zálohy protokolu, které byly provedeny a uložené po pořízení snímku úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-196">hello SAP HANA snapshot that's included in hello storage snapshot is an offset point for applying log backups that have been executed and stored after hello storage snapshot was taken.</span></span>
- <span data-ttu-id="63f5a-197">Tyto zálohy protokolu se provádějí toorestore hello databáze back tooa určité bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="63f5a-197">These log backups are taken toorestore hello database back tooa certain point in time.</span></span>

<span data-ttu-id="63f5a-198">Zadání hello zálohování\_název vytvoří snímek hello následujících svazků:</span><span class="sxs-lookup"><span data-stu-id="63f5a-198">Specifying hello backup\_name creates a snapshot of hello following volumes:</span></span>

- <span data-ttu-id="63f5a-199">Hana nebo dat</span><span class="sxs-lookup"><span data-stu-id="63f5a-199">hana/data</span></span>
- <span data-ttu-id="63f5a-200">Hana a protokolování</span><span class="sxs-lookup"><span data-stu-id="63f5a-200">hana/log</span></span>
- <span data-ttu-id="63f5a-201">Hana nebo protokolu\_zálohování (připojit jako zálohování do hana nebo protokolu)</span><span class="sxs-lookup"><span data-stu-id="63f5a-201">hana/log\_backup (mounted as backup into hana/log)</span></span>
- <span data-ttu-id="63f5a-202">/ sdílené Hana</span><span class="sxs-lookup"><span data-stu-id="63f5a-202">hana/shared</span></span>

### <a name="storage-snapshot-considerations"></a><span data-ttu-id="63f5a-203">Aspekty volby úložiště snímků</span><span class="sxs-lookup"><span data-stu-id="63f5a-203">Storage snapshot considerations</span></span>

>[!NOTE]
><span data-ttu-id="63f5a-204">Úložiště snímků jsou _není_ poskytuje bezplatně, protože musí být přidělený dalšího volného místa.</span><span class="sxs-lookup"><span data-stu-id="63f5a-204">Storage snapshots are _not_ provided free of charge, because additional storage space must be allocated.</span></span>

<span data-ttu-id="63f5a-205">konkrétní mechanismy Hello úložiště snímků pro SAP HANA v Azure (velké instance) patří:</span><span class="sxs-lookup"><span data-stu-id="63f5a-205">hello specific mechanics of storage snapshots for SAP HANA on Azure (large instances) include:</span></span>

- <span data-ttu-id="63f5a-206">Konkrétní úložiště snímků (na hello bodu v čase, kdy se provede) spotřebovává velmi málo úložiště.</span><span class="sxs-lookup"><span data-stu-id="63f5a-206">A specific storage snapshot (at hello point in time when it is taken) consumes very little storage.</span></span>
- <span data-ttu-id="63f5a-207">Jako obsahu změny dat a hello obsah SAP HANA data, která soubory změnit na hello svazek úložiště musí hello snímku toostore hello původní blokování obsahu.</span><span class="sxs-lookup"><span data-stu-id="63f5a-207">As data content changes and hello content in SAP HANA data files change on hello storage volume, hello snapshot needs toostore hello original block content.</span></span>
- <span data-ttu-id="63f5a-208">Hello úložiště snímků nárůstu velikosti.</span><span class="sxs-lookup"><span data-stu-id="63f5a-208">hello storage snapshot increases in size.</span></span> <span data-ttu-id="63f5a-209">existuje Hello delší hello snímku, bude hello větší hello úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-209">hello longer hello snapshot exists, hello larger hello storage snapshot becomes.</span></span>
- <span data-ttu-id="63f5a-210">Dobrý den další změny toohello svazku databáze SAP HANA průběhu životnosti hello úložiště snímku, bude hello větší hello místo spotřeba úložiště snímku hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-210">hello more changes made toohello SAP HANA database volume over hello lifetime of a storage snapshot, hello larger hello space consumption of hello storage snapshot becomes.</span></span>

<span data-ttu-id="63f5a-211">SAP HANA v Azure (velké instance) se dodává s velikostí pevný svazek pro svazek protokolu a data SAP HANA hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-211">SAP HANA on Azure (large instances) comes with fixed volume sizes for hello SAP HANA data and log volume.</span></span> <span data-ttu-id="63f5a-212">Provádění snímky tyto svazky eats do místo ve svazku, takže je vaše odpovědnosti tooschedule úložiště snímků (v rámci hello SAP HANA na proces Azure [velké instance]).</span><span class="sxs-lookup"><span data-stu-id="63f5a-212">Performing snapshots of those volumes eats into your volume space, so it is your responsibility tooschedule storage snapshots (within hello SAP HANA on Azure [large instances] process).</span></span>

<span data-ttu-id="63f5a-213">Hello následující části obsahují informace o provádění tyto snímky, včetně obecná doporučení:</span><span class="sxs-lookup"><span data-stu-id="63f5a-213">hello following sections provide information for performing these snapshots, including general recommendations:</span></span>

- <span data-ttu-id="63f5a-214">I když hello hardware tolerovat 255 snímků na svazku, důrazně doporučujeme, abyste zůstat dobře pod tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="63f5a-214">Though hello hardware can sustain 255 snapshots per volume, we highly recommend that you stay well below this number.</span></span>
- <span data-ttu-id="63f5a-215">Před provedením úložiště snímků, monitorování a sledování volného místa.</span><span class="sxs-lookup"><span data-stu-id="63f5a-215">Before you perform storage snapshots, monitor and keep track of free space.</span></span>
- <span data-ttu-id="63f5a-216">Nižší číslo hello úložiště snímků podle volného místa.</span><span class="sxs-lookup"><span data-stu-id="63f5a-216">Lower hello number of storage snapshots based on free space.</span></span> <span data-ttu-id="63f5a-217">Může být nutné toolower hello počet snímků, které můžete zachovat, nebo může být nutné tooextend hello svazky.</span><span class="sxs-lookup"><span data-stu-id="63f5a-217">You might need toolower hello number of snapshots that you keep, or you might need tooextend hello volumes.</span></span> <span data-ttu-id="63f5a-218">(Další úložiště můžete uspořádat v jednotkách 1 TB.)</span><span class="sxs-lookup"><span data-stu-id="63f5a-218">(You can order additional storage in 1-TB units.)</span></span>
- <span data-ttu-id="63f5a-219">Během aktivity, například přesun dat do SAP HANA s nástroji pro migraci systému (s R3load nebo po obnovení databáze SAP HANA ze zálohy) důrazně doporučujeme nebude provádět všechny snímky úložiště.</span><span class="sxs-lookup"><span data-stu-id="63f5a-219">During activities such as moving data into SAP HANA with system migration tools (with R3load, or by restoring SAP HANA databases from backups), we highly recommended that you not perform any storage snapshots.</span></span> <span data-ttu-id="63f5a-220">(Pokud migrace systému probíhá na nový systém SAP HANA, úložiště snímků nebude potřeba provést toobe.)</span><span class="sxs-lookup"><span data-stu-id="63f5a-220">(If a system migration is being done on a new SAP HANA system, storage snapshots would not need toobe performed.)</span></span>
- <span data-ttu-id="63f5a-221">Během větší uspořádání SAP HANA tabulek úložiště snímků je nutno Pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="63f5a-221">During larger reorganizations of SAP HANA tables, storage snapshots should be avoided if possible.</span></span>
- <span data-ttu-id="63f5a-222">Úložiště snímků jsou možnosti pro hello zotavení po havárii požadovaných tooengaging SAP HANA v Azure (velké instance).</span><span class="sxs-lookup"><span data-stu-id="63f5a-222">Storage snapshots are a prerequisite tooengaging hello disaster-recovery capabilities of SAP HANA on Azure (large instances).</span></span>

### <a name="setting-up-storage-snapshots"></a><span data-ttu-id="63f5a-223">Nastavení úložiště snímků</span><span class="sxs-lookup"><span data-stu-id="63f5a-223">Setting up storage snapshots</span></span>

1. <span data-ttu-id="63f5a-224">Zajistěte, aby Perl nainstalovanou v operačním systému Linux hello na serveru (velké instance) HANA hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-224">Make sure that Perl is installed in hello Linux operating system on hello HANA (large instances) server.</span></span>
2. <span data-ttu-id="63f5a-225">Upravit/etc/ssh/ssh\_konfigurace tooadd hello řádku _Mac hmac-sha1_.</span><span class="sxs-lookup"><span data-stu-id="63f5a-225">Modify /etc/ssh/ssh\_config tooadd hello line _MACs hmac-sha1_.</span></span>
3. <span data-ttu-id="63f5a-226">Vytvoření účtu SAP HANA zálohování uživatele na hello hlavní uzel pro každou instanci SAP HANA, spuštěný (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="63f5a-226">Create an SAP HANA backup user account on hello master node for each SAP HANA instance you are running (if applicable).</span></span>
4. <span data-ttu-id="63f5a-227">Klient SAP HANA HDB Hello musí být nainstalován na všech serverech SAP HANA (velké instance).</span><span class="sxs-lookup"><span data-stu-id="63f5a-227">hello SAP HANA HDB client must be installed on all SAP HANA (large instances) servers.</span></span>
5. <span data-ttu-id="63f5a-228">Na serveru SAP HANA (velké instance) první hello každé oblasti musí být veřejný klíč vytvořený tooaccess hello základní infrastrukturu úložiště, která řídí vytváření snímku.</span><span class="sxs-lookup"><span data-stu-id="63f5a-228">On hello first SAP HANA (large instances) server of each region, a public key must be created tooaccess hello underlying storage infrastructure that controls snapshot creation.</span></span>
6. <span data-ttu-id="63f5a-229">Zkopírujte skript hello azure\_hana\_backup.pl z umístění toohello/Scripts **hdbsql** Dobrý den instalace SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="63f5a-229">Copy hello script azure\_hana\_backup.pl from /scripts toohello location of **hdbsql** of hello SAP HANA installation.</span></span>
7. <span data-ttu-id="63f5a-230">Kopírování hello HANABackupDetails.txt souboru z toohello/Scripts stejné umístění jako hello Perl skriptu.</span><span class="sxs-lookup"><span data-stu-id="63f5a-230">Copy hello HANABackupDetails.txt file from /scripts toohello same location as hello Perl script.</span></span>
8. <span data-ttu-id="63f5a-231">Upravte soubor HANABackupDetails.txt hello v případě potřeby u specifikace hello odpovídající zákazníka.</span><span class="sxs-lookup"><span data-stu-id="63f5a-231">Modify hello HANABackupDetails.txt file as necessary for hello appropriate customer specifications.</span></span>

### <a name="step-1-install-sap-hana-hdbclient"></a><span data-ttu-id="63f5a-232">Krok 1: Instalace SAP HANA HDBClient</span><span class="sxs-lookup"><span data-stu-id="63f5a-232">Step 1: Install SAP HANA HDBClient</span></span>

<span data-ttu-id="63f5a-233">Hello Linux nainstalován na SAP HANA v Azure (velké instance) obsahuje složky hello a skripty potřebné tooexecute SAP HANA úložiště snímků pro účely zálohování a zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="63f5a-233">hello Linux installed on SAP HANA on Azure (large instances) includes hello folders and scripts necessary tooexecute SAP HANA storage snapshots for backup and disaster-recovery purposes.</span></span> <span data-ttu-id="63f5a-234">Je však vaše odpovědnosti tooinstall SAP HANA HDBclient při instalaci SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="63f5a-234">However, it is your responsibility tooinstall SAP HANA HDBclient while you are installing SAP HANA.</span></span> <span data-ttu-id="63f5a-235">(Microsoft nainstaluje hello HDBclient ani SAP HANA.)</span><span class="sxs-lookup"><span data-stu-id="63f5a-235">(Microsoft installs neither hello HDBclient nor SAP HANA.)</span></span>

### <a name="step-2-change-etcsshsshconfig"></a><span data-ttu-id="63f5a-236">Krok 2: Změnit/etc/ssh/ssh\_konfigurace</span><span class="sxs-lookup"><span data-stu-id="63f5a-236">Step 2: Change /etc/ssh/ssh\_config</span></span>

<span data-ttu-id="63f5a-237">Změnit/etc/ssh/ssh\_konfigurace přidáním _Mac hmac-sha1_ řádek, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="63f5a-237">Change /etc/ssh/ssh\_config by adding _MACs hmac-sha1_ line as shown here:</span></span>
```
#   RhostsRSAAuthentication no
#   RSAAuthentication yes
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   GSSAPIKeyExchange no
#   GSSAPITrustDNS no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/identity
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   Port 22
Protocol 2
#   Cipher 3des
#   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc
#   MACs hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160
MACs hmac-sha1
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
```

### <a name="step-3-create-a-public-key"></a><span data-ttu-id="63f5a-238">Krok 3: Vytvoření veřejný klíč</span><span class="sxs-lookup"><span data-stu-id="63f5a-238">Step 3: Create a public key</span></span>

<span data-ttu-id="63f5a-239">Na hello první SAP HANA na serveru Azure (velké instance) v každé oblasti Azure vytvořit infrastrukturu veřejných klíčů toobe používá tooaccess hello úložiště, takže můžete pořídit snímky.</span><span class="sxs-lookup"><span data-stu-id="63f5a-239">On hello first SAP HANA on Azure (large instances) server in each Azure region, create a public key toobe used tooaccess hello storage infrastructure so that you can create snapshots.</span></span> <span data-ttu-id="63f5a-240">veřejný klíč Hello zajistí, že heslo není požadovaná toosign v úložišti toohello a že nejsou spravovány hesla pověření.</span><span class="sxs-lookup"><span data-stu-id="63f5a-240">hello public key ensures that a password is not required toosign in toohello storage and that password credentials are not maintained.</span></span> <span data-ttu-id="63f5a-241">V systému Linux na hello SAP HANA (velké instance) serveru spusťte následující příkaz toogenerate hello veřejný klíč hello:</span><span class="sxs-lookup"><span data-stu-id="63f5a-241">In Linux on hello SAP HANA (large instances) server, execute hello following command toogenerate hello public key:</span></span>
```
  ssh-keygen –t dsa –b 1024
```
<span data-ttu-id="63f5a-242">nové umístění Hello je _/root/.ssh/id\_dsa.pub.</span><span class="sxs-lookup"><span data-stu-id="63f5a-242">hello new location is _/root/.ssh/id\_dsa.pub.</span></span> <span data-ttu-id="63f5a-243">Nezadávejte skutečné přístupové heslo, jinak bude požadované tooenter hello heslo při každém přihlášení.</span><span class="sxs-lookup"><span data-stu-id="63f5a-243">Do not enter an actual passphrase, or else you will be required tooenter hello passphrase each time you sign in.</span></span> <span data-ttu-id="63f5a-244">Místo toho stiskněte **Enter** dvakrát tooremove hello zadejte požadavek přístupové heslo pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="63f5a-244">Instead, press **Enter** twice tooremove hello enter passphrase requirement for signing in.</span></span>

<span data-ttu-id="63f5a-245">Zkontrolujte, zda tento veřejný klíč hello byl opraven podle očekávání změnou too/root/.ssh/ složky a potom provádění hello toomake **ls** příkaz.</span><span class="sxs-lookup"><span data-stu-id="63f5a-245">Check toomake sure that hello public key was corrected as expected by changing folders too/root/.ssh/ and then executing hello **ls** command.</span></span> <span data-ttu-id="63f5a-246">Pokud se nachází hello klíč, můžete ji zkopírovat spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="63f5a-246">If hello key is present, you can copy it by running hello following command:</span></span>

![Veřejný klíč se zkopíruje spuštěním tohoto příkazu](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

<span data-ttu-id="63f5a-248">V tomto okamžiku obraťte se na SAP HANA na Azure Service Management a zadejte klíč hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-248">At this point, contact SAP HANA on Azure Service Management and provide hello key.</span></span> <span data-ttu-id="63f5a-249">Hello zástupce služeb používá hello veřejného klíče tooregister v hello základní infrastruktury úložiště.</span><span class="sxs-lookup"><span data-stu-id="63f5a-249">hello service representative uses hello public key tooregister it in hello underlying storage infrastructure.</span></span>

### <a name="step-4-create-an-sap-hana-user-account"></a><span data-ttu-id="63f5a-250">Krok 4: Vytvoření účtu uživatele SAP HANA</span><span class="sxs-lookup"><span data-stu-id="63f5a-250">Step 4: Create an SAP HANA user account</span></span>

<span data-ttu-id="63f5a-251">Vytvořte uživatelský účet SAP HANA studia SAP HANA pro účely zálohování.</span><span class="sxs-lookup"><span data-stu-id="63f5a-251">Create an SAP HANA user account within SAP HANA Studio for backup purposes.</span></span> <span data-ttu-id="63f5a-252">Tento účet musí mít následující oprávnění hello: _správce zálohování_ a _katalogu čtení_.</span><span class="sxs-lookup"><span data-stu-id="63f5a-252">This account must have hello following privileges: _Backup Admin_ and _Catalog Read_.</span></span> <span data-ttu-id="63f5a-253">V tomto příkladu hello uživatelské jméno SCADMIN se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="63f5a-253">In this example, hello username SCADMIN is created.</span></span>

![Vytvoření uživatele v HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

### <a name="step-5-authorize-hello-sap-hana-user-account"></a><span data-ttu-id="63f5a-255">Krok 5: Autorizace hello SAP HANA uživatelský účet</span><span class="sxs-lookup"><span data-stu-id="63f5a-255">Step 5: Authorize hello SAP HANA user account</span></span>

<span data-ttu-id="63f5a-256">Autorizovat hello SAP HANA uživatelský účet (toobe používá skripty hello bez vyžadování ověřování při každém spuštění skriptu hello).</span><span class="sxs-lookup"><span data-stu-id="63f5a-256">Authorize hello SAP HANA user account (toobe used by hello scripts without requiring authorization every time hello script is run).</span></span> <span data-ttu-id="63f5a-257">Hello SAP HANA příkaz `hdbuserstore` umožňuje vytvoření hello SAP HANA uživatelský klíč, který je uložený na jeden nebo více uzlů SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="63f5a-257">hello SAP HANA command `hdbuserstore` allows hello creation of an SAP HANA user key, which is stored on one or more SAP HANA nodes.</span></span> <span data-ttu-id="63f5a-258">uživatelský klíč Hello také umožňuje hello uživatele tooaccess SAP HANA bez nutnosti toomanage hesla z v rámci hello skriptování proces, který je popsán později.</span><span class="sxs-lookup"><span data-stu-id="63f5a-258">hello user key also allows hello user tooaccess SAP HANA without having toomanage passwords from within hello scripting process that's discussed later.</span></span>

>[!IMPORTANT]
><span data-ttu-id="63f5a-259">Spuštění hello následující příkaz jako `_root_`.</span><span class="sxs-lookup"><span data-stu-id="63f5a-259">Run hello following command as `_root_`.</span></span> <span data-ttu-id="63f5a-260">Hello skriptu, jinak hodnota nemůže pracovat správně.</span><span class="sxs-lookup"><span data-stu-id="63f5a-260">Otherwise, hello script cannot work properly.</span></span>

<span data-ttu-id="63f5a-261">Zadejte hello `hdbuserstore` příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="63f5a-261">Enter hello `hdbuserstore` command as follows:</span></span>

![Zadejte příkaz hdbuserstore hello](./media/hana-overview-high-availability-disaster-recovery/image4-hdbuserstore-command.png)

<span data-ttu-id="63f5a-263">V následujícím příkladu, kde je uživatel hello SCADMIN01 a název hostitele hello je lhanad01, hello hello příkaz je:</span><span class="sxs-lookup"><span data-stu-id="63f5a-263">In hello following example, where hello user is SCADMIN01 and hello hostname is lhanad01, hello command is:</span></span>
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
<span data-ttu-id="63f5a-264">Spravujte všechny skriptování na jednom serveru pro škálování HANA instance.</span><span class="sxs-lookup"><span data-stu-id="63f5a-264">Manage all scripting from a single server for scale-out HANA instances.</span></span> <span data-ttu-id="63f5a-265">V tomto příkladu musí být pro každého hostitele tak, aby odráží hello hostitele, který je klíč související toohello změněn hello SAP HANA klíč SCADMIN01.</span><span class="sxs-lookup"><span data-stu-id="63f5a-265">In this example, hello SAP HANA key SCADMIN01 must be altered for each host in a way that reflects hello host that is related toohello key.</span></span> <span data-ttu-id="63f5a-266">To znamená, se mění hello účet záloh SAP HANA s hello instance počet hello HANA DB **lhanad**.</span><span class="sxs-lookup"><span data-stu-id="63f5a-266">That is, hello SAP HANA backup account is amended with hello instance number of hello HANA DB, **lhanad**.</span></span> <span data-ttu-id="63f5a-267">Hello klíč musí mít oprávnění správce na hostiteli hello, které je přiřazen, a hello zálohování uživatele Škálováním na více systémů musí mít přístup práva tooall SAP HANA instance.</span><span class="sxs-lookup"><span data-stu-id="63f5a-267">hello key must have administrative privileges on hello host it is assigned to, and hello backup user for scale-out must have access rights tooall SAP HANA instances.</span></span>
```
hdbuserstore set SCADMIN01 lhanad:30015 SCADMIN <password>
hdbuserstore set SCADMIN02 lhanad:30115 SCADMIN <password>
hdbuserstore set SCADMIN03 lhanad:30215 SCADMIN <password>
```

### <a name="step-6-copy-items-from-hello-scripts-folder"></a><span data-ttu-id="63f5a-268">Krok 6: Kopírovat položky ze složky/Scripts hello</span><span class="sxs-lookup"><span data-stu-id="63f5a-268">Step 6: Copy items from hello /scripts folder</span></span>

<span data-ttu-id="63f5a-269">Kopírování hello následující položky z hello/skriptů složky (obsažené na hello gold bitovou kopii instalace hello) toohello pracovní adresář pro **hdbsql**.</span><span class="sxs-lookup"><span data-stu-id="63f5a-269">Copy hello following items from hello /scripts folder (included on hello gold image of hello installation) toohello working directory for **hdbsql**.</span></span> <span data-ttu-id="63f5a-270">Pro aktuální HANA instalace, tento adresář se /hana/shared/D01/exe/linuxx86\_64/hdb.</span><span class="sxs-lookup"><span data-stu-id="63f5a-270">For current HANA installations, this directory is /hana/shared/D01/exe/linuxx86\_64/hdb.</span></span>
```
azure\_hana\_backup.pl
testHANAConnection.pl
testStorageSnapshotConnection.pl
removeTestStorageSnapshot.pl
HANABackupCustomerDetails.txt
```
<span data-ttu-id="63f5a-271">Zkopírujte následující položky, pokud používají Škálováním na více systémů hello nebo OLAP:</span><span class="sxs-lookup"><span data-stu-id="63f5a-271">Copy hello following items if they are running scale-out or OLAP:</span></span>
```
azure\_hana\_backup\_bw.pl
testHANAConnectionBW.pl
testStorageSnapshotConnectionBW.pl
removeTestStorageSnapshotBW.pl
HANABackupCustomerDetailsBW.txt
```
<span data-ttu-id="63f5a-272">soubor HANABackupCustomerDetails.txt Hello je upravitelnými takto pro nasazení škálování.</span><span class="sxs-lookup"><span data-stu-id="63f5a-272">hello HANABackupCustomerDetails.txt file is modifiable as follows for a scale-up deployment.</span></span> <span data-ttu-id="63f5a-273">Je hello řízení a konfigurační soubor pro hello skript, který běží hello úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-273">It is hello control and configuration file for hello script that runs hello storage snapshots.</span></span> <span data-ttu-id="63f5a-274">Byste měli obdržet hello _název zálohy úložiště_ a _úložiště IP adresu_ ze SAP HANA na Azure Service Management při nasazení vaší instance.</span><span class="sxs-lookup"><span data-stu-id="63f5a-274">You should have received hello _Storage Backup Name_ and _Storage IP Address_ from SAP HANA on Azure Service Management when your instances were deployed.</span></span> <span data-ttu-id="63f5a-275">Nelze upravit hello pořadí řazení, nebo mezery všech proměnných hello nebo skriptu hello správně spustit.</span><span class="sxs-lookup"><span data-stu-id="63f5a-275">You cannot modify hello sequence, ordering, or spacing of any of hello variables, or hello script does not run properly.</span></span>

<span data-ttu-id="63f5a-276">Pro škálování nasazení bude vypadat hello konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="63f5a-276">For a scale-up deployment, hello configuration file would look like:</span></span>
```
#Provided by Microsoft Service Management
Storage Backup Name: lhanad01backup
Storage IP Address: 10.250.20.21
#Created by customer using hdbuserstore
HANA Backup Name: SCADMIND01
```
<span data-ttu-id="63f5a-277">Pro konfiguraci škálovaného na více systémů bude vypadat hello HANABackupCustomerDetailsBW.txt souboru:</span><span class="sxs-lookup"><span data-stu-id="63f5a-277">For a scale-out configuration, hello HANABackupCustomerDetailsBW.txt file would look like:</span></span>
```
#Provided by Microsoft Service Management
Storage Backup Name: lhanad01backup
Storage IP Address: 10.250.20.21
#Node IP addresses, instance numbers, and HANA backup name
#provided by customer.  HANA backup name created using
#hdbuserstore utility.
Node 1 IP Address: 10.254.15.21
Node 1 HANA instance number: 01
Node 1 HANA Backup Name: SCADMIN01
Node 2 IP Address: 10.254.15.22
Node 2 HANA instance number: 02
Node 2 HANA Backup Name: SCADMIN02
Node 3 IP Address: 10.254.15.23
Node 3 HANA instance number: 03
Node 3 HANA Backup Name: SCADMIN03
Node 4 IP Address: 10.254.15.24
Node 4 HANA instance number: 04
Node 4 HANA Backup Name: SCADMIN04
Node 5 IP Address: 10.254.15.25
Node 5 HANA instance number: 05
Node 5 HANA Backup Name: SCADMIN05
Node 6 IP Address: 10.254.15.26
Node 6 HANA instance number: 06
Node 6 HANA Backup Name: SCADMIN06
Node 7 IP Address: 10.254.15.27
Node 7 HANA instance number: 07
Node 7 HANA Backup Name: SCADMIN07
Node 8 IP Address: 10.254.15.28
Node 8 HANA instance number: 08
Node 8 HANA Backup Name: SCADMIN08
```
>[!NOTE]
><span data-ttu-id="63f5a-278">V současné době pouze podrobnosti o 1 uzlu se používají v hello skutečné HANA úložiště snímků skriptu.</span><span class="sxs-lookup"><span data-stu-id="63f5a-278">Currently, only Node 1 details are used in hello actual HANA storage snapshot script.</span></span> <span data-ttu-id="63f5a-279">Doporučujeme, abyste otestovali tooor přístup ze všech uzlů HANA tak, že pokud se hlavní uzel zálohování hello někdy změní, již zajistíte, že jiného uzlu může trvat jeho místo změnou hello podrobnosti v uzlu 1.</span><span class="sxs-lookup"><span data-stu-id="63f5a-279">We recommend that you test access tooor from all HANA nodes so that, if hello master backup node ever changes, you have already ensured that any other node can take its place by modifying hello details in Node 1.</span></span>

<span data-ttu-id="63f5a-280">toocheck pro hello správné konfigurace v konfiguračním souboru hello nebo instancí HANA toohello správné připojení, spusťte některý z následujících skriptů hello:</span><span class="sxs-lookup"><span data-stu-id="63f5a-280">toocheck for hello correct configurations in hello configuration file or proper connectivity toohello HANA instances, run either of hello following scripts:</span></span>
- <span data-ttu-id="63f5a-281">Pro škálování konfigurace (nezávislou na pracovním vytížení SAP):</span><span class="sxs-lookup"><span data-stu-id="63f5a-281">For a scale-up configuration (independent on SAP workload):</span></span>

 ```
testHANAConnection.pl
```
- <span data-ttu-id="63f5a-282">Pro škálovatelnou konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="63f5a-282">For a scale-out configuration:</span></span>

 ```
testHANAConnectionBW.pl
```

<span data-ttu-id="63f5a-283">Zajistěte, aby byl tuto instanci hello hlavní HANA servery HANA tooall vyžaduje přístup.</span><span class="sxs-lookup"><span data-stu-id="63f5a-283">Ensure that hello master HANA instance has access tooall required HANA servers.</span></span> <span data-ttu-id="63f5a-284">Neexistují žádné parametry toohello skript, ale musíte dokončit hello odpovídající HANABackupCustomerDetails / HANABackupCustomerDetailsBW souboru skriptu toorun hello správně.</span><span class="sxs-lookup"><span data-stu-id="63f5a-284">There are no parameters toohello script, but you must complete hello appropriate HANABackupCustomerDetails/ HANABackupCustomerDetailsBW file for hello script toorun properly.</span></span> <span data-ttu-id="63f5a-285">Vzhledem k tomu, že se vrátí jenom hello prostředí příkaz kódy chyb, není možné pro hello skriptu tooerror zkontrolujte všechny instance.</span><span class="sxs-lookup"><span data-stu-id="63f5a-285">Because only hello shell command error codes are returned, it is not possible for hello script tooerror-check every instance.</span></span> <span data-ttu-id="63f5a-286">I tak hello skriptu zadejte některé užitečné komentáře toodouble kontrola.</span><span class="sxs-lookup"><span data-stu-id="63f5a-286">Even so, hello script does provide some helpful comments for you toodouble-check.</span></span>

<span data-ttu-id="63f5a-287">toorun hello skriptu:</span><span class="sxs-lookup"><span data-stu-id="63f5a-287">toorun hello script:</span></span>
```
 ./testHANAConnection.pl
```
 <span data-ttu-id="63f5a-288">Pokud skript hello úspěšně získá stav hello hello HANA instance, zobrazí zprávu, zda text hello HANA připojení bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="63f5a-288">If hello script successfully obtains hello status of hello HANA instance, it displays a message that hello HANA connection was successful.</span></span>

<span data-ttu-id="63f5a-289">Kromě toho je druhý typ skriptu můžete použít možnost toosign toocheck hello hlavní HANA instanci serveru v toohello úložiště.</span><span class="sxs-lookup"><span data-stu-id="63f5a-289">Additionally, there is a second type of script you can use toocheck hello master HANA instance server's ability toosign in toohello storage.</span></span> <span data-ttu-id="63f5a-290">Před spuštěním hello azure\_hana\_zálohování (\_bw) PL skriptu, je třeba spustit skript další hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-290">Before you execute hello azure\_hana\_backup(\_bw).pl script, you must execute hello next script.</span></span> <span data-ttu-id="63f5a-291">Pokud svazek obsahuje žádné snímky, je možné toodetermine zda hello svazek je jednoduše prázdný nebo není ssh selhání tooobtain hello snímku podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="63f5a-291">If a volume contains no snapshots, it is impossible toodetermine whether hello volume is simply empty or there is an ssh failure tooobtain hello snapshot details.</span></span> <span data-ttu-id="63f5a-292">Z tohoto důvodu hello skript spustí dva kroky:</span><span class="sxs-lookup"><span data-stu-id="63f5a-292">For this reason, hello script executes two steps:</span></span>

- <span data-ttu-id="63f5a-293">Ověří, jestli tato konzola hello úložiště dostupné.</span><span class="sxs-lookup"><span data-stu-id="63f5a-293">It verifies that hello storage console is accessible.</span></span>
- <span data-ttu-id="63f5a-294">Vytvoří snímek testu nebo fiktivní, pro každý svazek HANA instance.</span><span class="sxs-lookup"><span data-stu-id="63f5a-294">It creates a test, or dummy, snapshot for each volume by HANA instance.</span></span>

<span data-ttu-id="63f5a-295">Z tohoto důvodu je zahrnuta jako argument hello HANA instance.</span><span class="sxs-lookup"><span data-stu-id="63f5a-295">For this reason, hello HANA instance is included as an argument.</span></span> <span data-ttu-id="63f5a-296">Znovu není možné tooprovide Chyba při ověřování pro připojení hello úložiště, ale hello skriptu poskytuje užitečné pomocné parametry, pokud se nezdaří spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-296">Again, it is not possible tooprovide error checking for hello storage connection, but hello script provides helpful hints if hello execution fails.</span></span>

<span data-ttu-id="63f5a-297">spuštění skriptu Hello jako:</span><span class="sxs-lookup"><span data-stu-id="63f5a-297">hello script is run as:</span></span>
```
 ./testStorageSnapshotConnection.pl <hana instance>
```
<span data-ttu-id="63f5a-298">Nebo ji spustit jako:</span><span class="sxs-lookup"><span data-stu-id="63f5a-298">Or it is run as:</span></span>
```
./testStorageSnapshotConnectionBW.pl <hana instance>
```
<span data-ttu-id="63f5a-299">skript Hello také zobrazí zprávu, že budete mít toosign v odpovídajícím způsobem tooyour nasadit úložiště klienta, který se věnuje hello logické jednotky (LUN) používané hello instance serveru, které vlastníte.</span><span class="sxs-lookup"><span data-stu-id="63f5a-299">hello script also displays a message that you are able toosign in appropriately tooyour deployed storage tenant, which is organized around hello logical unit numbers (LUNs) that are used by hello server instances you own.</span></span>

<span data-ttu-id="63f5a-300">Před spuštěním hello první úložiště založené na snímek zálohy, spusťte hello další skripty toomake se, že tato konfigurace hello je správný.</span><span class="sxs-lookup"><span data-stu-id="63f5a-300">Before you execute hello first storage snapshot-based backups, run hello next scripts toomake sure that hello configuration is correct.</span></span>

<span data-ttu-id="63f5a-301">Po spuštění těchto skriptů, můžete odstranit snímky hello spuštěním:</span><span class="sxs-lookup"><span data-stu-id="63f5a-301">After running these scripts, you can delete hello snapshots by executing:</span></span>
```
./removeTestStorageSnapshot.pl <hana instance>
```
<span data-ttu-id="63f5a-302">Nebo</span><span class="sxs-lookup"><span data-stu-id="63f5a-302">Or</span></span>
```
./removeTestStorageSnapshot.pl <hana instance>
```

### <a name="step-7-perform-on-demand-snapshots"></a><span data-ttu-id="63f5a-303">Krok 7: Provedení snímků na vyžádání</span><span class="sxs-lookup"><span data-stu-id="63f5a-303">Step 7: Perform on-demand snapshots</span></span>

<span data-ttu-id="63f5a-304">Provádět na vyžádání snímků (a také naplánovat pravidelné snímky pomocí procesu cron) podle postupu popsaného tady.</span><span class="sxs-lookup"><span data-stu-id="63f5a-304">Perform on-demand snapshots (as well as schedule regular snapshots by using cron) as described here.</span></span>

<span data-ttu-id="63f5a-305">Pro škálování konfigurace spusťte následující skript hello:</span><span class="sxs-lookup"><span data-stu-id="63f5a-305">For scale-up configurations, execute hello following script:</span></span>
```
./azure_hana_backup.pl lhanad01 customer 20
```
<span data-ttu-id="63f5a-306">Konfigurace Škálováním na více systémů spusťte následující skript hello:</span><span class="sxs-lookup"><span data-stu-id="63f5a-306">For scale-out configurations, execute hello following script:</span></span>
```
./azure_hana_backup_bw.pl lhanad01 customer 20
```
<span data-ttu-id="63f5a-307">skript Škálováním na více systémů Hello provádí některé další kontroly toomake se, že všechny servery HANA je přístupná, a všechny instance HANA vrátí příslušný stav instance hello před pokračováním vytváření snímků SAP HANA nebo úložiště.</span><span class="sxs-lookup"><span data-stu-id="63f5a-307">hello scale-out script does some additional checking toomake sure that all HANA servers can be accessed, and all HANA instances return appropriate status of hello instance before proceeding with creating SAP HANA or storage snapshots.</span></span>

<span data-ttu-id="63f5a-308">vyžadují se následující argumenty Hello:</span><span class="sxs-lookup"><span data-stu-id="63f5a-308">hello following arguments are required:</span></span>

- <span data-ttu-id="63f5a-309">Hello HANA instance vyžadují zálohování.</span><span class="sxs-lookup"><span data-stu-id="63f5a-309">hello HANA instance requiring backup.</span></span>
- <span data-ttu-id="63f5a-310">Předpona Hello snímku pro hello úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-310">hello snapshot prefix for hello storage snapshot.</span></span>
- <span data-ttu-id="63f5a-311">Hello počet snímků toobe zachovány pro konkrétní předponu hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-311">hello number of snapshots toobe kept for hello specific prefix.</span></span>

```
./azure_hana_backup.pl lhanad01 customer 20
```

<span data-ttu-id="63f5a-312">Hello spuštění skriptu hello vytvoří snímek hello úložiště v těchto tří různých fází:</span><span class="sxs-lookup"><span data-stu-id="63f5a-312">hello execution of hello script creates hello storage snapshot in these three distinct phases:</span></span>

- <span data-ttu-id="63f5a-313">Spusťte HANA snímku.</span><span class="sxs-lookup"><span data-stu-id="63f5a-313">Execute a HANA snapshot.</span></span>
- <span data-ttu-id="63f5a-314">Spusťte snímku úložiště.</span><span class="sxs-lookup"><span data-stu-id="63f5a-314">Execute a storage snapshot.</span></span>
- <span data-ttu-id="63f5a-315">Odeberte hello HANA snímku.</span><span class="sxs-lookup"><span data-stu-id="63f5a-315">Remove hello HANA snapshot.</span></span>

<span data-ttu-id="63f5a-316">Spusťte skript hello voláním z hello HDB složku spustitelného souboru zkopírované do.</span><span class="sxs-lookup"><span data-stu-id="63f5a-316">Execute hello script by calling it from hello HDB executable folder that it was copied to.</span></span> <span data-ttu-id="63f5a-317">Se zálohuje alespoň hello tyto svazky, ale také zálohuje jakýkoli svazek, který má název instance SAP HANA explicitní hello v název svazku hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-317">It backs up at least hello following volumes, but it also backs up any volume that has hello explicit SAP HANA instance name in hello volume name.</span></span>
```
hana_data_<hana instance>_prod_t020_vol
hana_log_<hana instance>_prod_t020_vol
hana_log_backup_<hana instance>_prod_t020_vol
hana_shared_<hana instance>_prod_t020_vol
```
<span data-ttu-id="63f5a-318">Doba uchování Hello je výhradně spravovat, s hello počet snímků odeslána jako parametr při spuštění skriptu hello (například 20 uvedený výše).</span><span class="sxs-lookup"><span data-stu-id="63f5a-318">hello retention period is strictly administered, with hello number of snapshots submitted as a parameter when you execute hello script (such as 20, shown previously).</span></span> <span data-ttu-id="63f5a-319">Hello množství času, proto je funkce hello dobu spuštění a hello počet snímků ve volání hello hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="63f5a-319">So hello amount of time is a function of hello period of execution and hello number of snapshots in hello call of hello script.</span></span> <span data-ttu-id="63f5a-320">Překročí-li hello počet snímků, které jsou zachovány hello číslo, které jsou pojmenované jako parametr v hello volání hello skriptu, hello nejstarší úložiště snímku tento popisek (v našem případě předchozí _vlastní_) je odstraněn, než se nový snímek spustit.</span><span class="sxs-lookup"><span data-stu-id="63f5a-320">If hello number of snapshots that are kept exceeds hello number that are named as a parameter in hello call of hello script, hello oldest storage snapshot of this label (in our previous case, _custom_) is deleted before a new snapshot is executed.</span></span> <span data-ttu-id="63f5a-321">To znamená hello číslo, kterou přiřadíte jako hello poslední parametr hello volání je hello číslo můžete použít toocontrol hello počet snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-321">This means hello number you give as hello last parameter of hello call is hello number you can use toocontrol hello number of snapshots.</span></span>

>[!NOTE]
><span data-ttu-id="63f5a-322">Jakmile změníte hello štítek, hello počítání znovu spustí.</span><span class="sxs-lookup"><span data-stu-id="63f5a-322">As soon as you change hello label, hello counting starts again.</span></span>

<span data-ttu-id="63f5a-323">Je nutné tooinclude hello HANA instance název, který poskytuje SAP HANA na Azure Service Management jako argument jejich vytváření snímků v prostředích s více uzly.</span><span class="sxs-lookup"><span data-stu-id="63f5a-323">You need tooinclude hello HANA instance name that's provided by SAP HANA on Azure Service Management as an argument if they are creating snapshots in multi-node environments.</span></span> <span data-ttu-id="63f5a-324">V prostředích s jedním uzlem hello název hello SAP HANA na jednotce Azure (velké instance) je dostačující, ale přesto se doporučuje název instance HANA hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-324">In single-node environments, hello name of hello SAP HANA on Azure (large instances) unit is sufficient, but hello HANA instance name is still recommended.</span></span>

<span data-ttu-id="63f5a-325">Kromě toho můžete můžete zálohovat spouštěcí volumes\LUNs pomocí hello stejný skriptu.</span><span class="sxs-lookup"><span data-stu-id="63f5a-325">Additionally, you can back up boot volumes\LUNs by using hello same script.</span></span> <span data-ttu-id="63f5a-326">Je nutné zálohovat spouštěcí svazek alespoň jednou při prvním spuštění HANA, i když vám doporučujeme týdenní nebo noční plán zálohování pro spouštění v procesu cron.</span><span class="sxs-lookup"><span data-stu-id="63f5a-326">You must back up your boot volume at least once when you first run HANA, although we recommend a weekly or nightly backup schedule for booting in cron.</span></span> <span data-ttu-id="63f5a-327">Spíše než přidat název instance SAP HANA, vložte _spouštěcí_ jako hello argument do skriptu hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="63f5a-327">Rather than add an SAP HANA instance name, insert _boot_ as hello argument into hello script as follows:</span></span>
```
./azure_hana_backup boot customer 20
```
<span data-ttu-id="63f5a-328">Hello stejné zásady uchovávání informací je poskytované také toohello spouštěcí svazek.</span><span class="sxs-lookup"><span data-stu-id="63f5a-328">hello same retention policy is afforded toohello boot volume as well.</span></span> <span data-ttu-id="63f5a-329">Použijte na vyžádání snímky, jak je popsáno dříve, pro zvláštní případy, například během upgradu SAP vylepšení balíčku (EHP), nebo když potřebujete toocreate snímku odlišné úložiště.</span><span class="sxs-lookup"><span data-stu-id="63f5a-329">Use on-demand snapshots, as described previously, for special cases only, such as during an SAP enhancement package (EHP) upgrade, or when you need toocreate a distinct storage snapshot.</span></span>

<span data-ttu-id="63f5a-330">Doporučujeme vám tooperform naplánované úložiště snímků pomocí procesu cron a doporučujeme použít hello stejný skriptu pro všechny zálohy a zotavení po havárii potřebám (změny hello skriptu vstupy toomatch hello různé požadovaný čas zálohování).</span><span class="sxs-lookup"><span data-stu-id="63f5a-330">We encourage you tooperform scheduled storage snapshots using cron, and we recommend that you use hello same script for all backups and disaster-recovery needs (modifying hello script inputs toomatch hello various requested backup times).</span></span> <span data-ttu-id="63f5a-331">Tyto jsou všechny naplánované jinak v procesu cron v závislosti na jejich čas spuštění: hodinové, 12 hodin, denní nebo týdenní.</span><span class="sxs-lookup"><span data-stu-id="63f5a-331">These are all scheduled differently in cron depending on their execution time: hourly, 12-hour, daily, or weekly.</span></span> <span data-ttu-id="63f5a-332">plán cron Hello je navrženou toocreate úložiště snímků, které odpovídají hello dřív popsané uchování označování pro dlouhodobé odlehlého zálohování.</span><span class="sxs-lookup"><span data-stu-id="63f5a-332">hello cron schedule is designed toocreate storage snapshots that match hello previously discussed retention labeling for long-term off-site backup.</span></span> <span data-ttu-id="63f5a-333">Hello skript obsahuje příkazy tooback zálohování všech svazků produkčního, v závislosti na jejich požadovanou četnost (data a soubory protokolu jsou zálohovány každou hodinu, zatímco hello spouštěcí svazek je zálohovat denně).</span><span class="sxs-lookup"><span data-stu-id="63f5a-333">hello script includes commands tooback up all production volumes, depending on their requested frequency (data and log files are backed up hourly, whereas hello boot volume is backed up daily).</span></span>

<span data-ttu-id="63f5a-334">Hello položek v hello následující skript cron spouštět každou hodinu v hello desetinu minutu, každých 12 hodin v hello desetinu minutu a každý den v hello desetinu minutu.</span><span class="sxs-lookup"><span data-stu-id="63f5a-334">hello entries in hello following cron script run every hour at hello tenth minute, every 12 hours at hello tenth minute, and daily at hello tenth minute.</span></span> <span data-ttu-id="63f5a-335">Hello cron, které úlohy se vytvářejí tak, že pouze jeden úložiště snímků SAP HANA probíhá během konkrétní hodina, tak, aby hello hodinové a denní zálohy se nevyskytují v hello, že stejný čas (12:10:00).</span><span class="sxs-lookup"><span data-stu-id="63f5a-335">hello cron jobs are created in such a way that only one SAP HANA storage snapshot takes place during any particular hour, so that hello hourly and daily backups do not occur at hello same time (12:10 AM).</span></span> <span data-ttu-id="63f5a-336">toohelp optimalizovat vytváření snímků a replikace, SAP HANA na Azure Service Management poskytuje hello doporučená dobu toorun můžete své zálohy.</span><span class="sxs-lookup"><span data-stu-id="63f5a-336">toohelp optimize your snapshot creation and replication, SAP HANA on Azure Service Management provides hello recommended time for you toorun your backups.</span></span>

<span data-ttu-id="63f5a-337">Hello výchozí cron plánování v /etc/crontab vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="63f5a-337">hello default cron scheduling in /etc/crontab is as follows:</span></span>
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 14
```
<span data-ttu-id="63f5a-338">V předchozích pokynů cron hello získat hello HANA svazky (bez spouštěcí svazek) každou hodinu snímku s popiskem hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-338">In hello previous cron instructions, hello HANA volumes (without boot volume) get an hourly snapshot with hello label.</span></span> <span data-ttu-id="63f5a-339">Tyto snímků 66 zachovány.</span><span class="sxs-lookup"><span data-stu-id="63f5a-339">Of these snapshots, 66 are retained.</span></span> <span data-ttu-id="63f5a-340">Kromě toho jsou uchovány 14 snímky s popiskem hello 12 hodin.</span><span class="sxs-lookup"><span data-stu-id="63f5a-340">Additionally, 14 snapshots with hello 12-hour label are retained.</span></span> <span data-ttu-id="63f5a-341">Potenciálně získat HODINOVĚ snímky pro tři dny a snímky 12 hodin pro jiné čtyř dní, které vám celý týden snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-341">You potentially get hourly snapshots for three days, plus 12-hour snapshots for another four days, which gives you an entire week of snapshots.</span></span>

<span data-ttu-id="63f5a-342">Plánování v rámci procesu cron může být složité, protože pouze jeden skript má být spuštěn v jakémkoli čase, pokud hello skripty rozkládají o několik minut.</span><span class="sxs-lookup"><span data-stu-id="63f5a-342">Scheduling within cron can be tricky, because only one script should be executed at any particular time, unless hello scripts are staggered by several minutes.</span></span> <span data-ttu-id="63f5a-343">Pokud chcete denní zálohy pro dlouhodobé uchovávání, denní snímek je uložen spolu s snímku 12 hodin (s uchování počtem 7 každý) nebo hodinové snímku hello je postupný tootake místní 10 minut později.</span><span class="sxs-lookup"><span data-stu-id="63f5a-343">If you want daily backups for long-term retention, either a daily snapshot is kept along with a 12-hour snapshot (with a retention count of seven each), or hello hourly snapshot is staggered tootake place 10 minutes later.</span></span> <span data-ttu-id="63f5a-344">Pouze jeden denní snímek je uložen v svazek provozního hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-344">Only one daily snapshot is kept in hello production volume.</span></span>
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 7
10 0 * * * ./azure_hana_backup.pl lhanad01 daily 7
```
<span data-ttu-id="63f5a-345">frekvence Hello tady jsou jenom příklady.</span><span class="sxs-lookup"><span data-stu-id="63f5a-345">hello frequencies listed here are only examples.</span></span> <span data-ttu-id="63f5a-346">tooderive optimální počet snímků, hello používá následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="63f5a-346">tooderive your optimum number of snapshots, use hello following criteria:</span></span>

- <span data-ttu-id="63f5a-347">Požadavky v cíli času obnovení pro obnovení bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="63f5a-347">Requirements in recovery time objective for point-in-time recovery.</span></span>
- <span data-ttu-id="63f5a-348">Využití místa.</span><span class="sxs-lookup"><span data-stu-id="63f5a-348">Space usage.</span></span>
- <span data-ttu-id="63f5a-349">Požadavky v rámci obnovování bodu cíl a cíli času obnovení pro zotavení po případné havárii.</span><span class="sxs-lookup"><span data-stu-id="63f5a-349">Requirements in recovery point objective and recovery time objective for potential disaster recovery.</span></span>
- <span data-ttu-id="63f5a-350">Závěrečné provádění záloh úplné databáze HANA proti disky.</span><span class="sxs-lookup"><span data-stu-id="63f5a-350">Eventual execution of HANA full database backups against disks.</span></span> <span data-ttu-id="63f5a-351">Vždy, když úplnou zálohu databáze na disky, nebo _backint_ rozhraní, se provádí, se nezdaří spuštění hello úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-351">Whenever a full database backup against disks, or _backint_ interface, is performed, hello execution of storage snapshots fails.</span></span> <span data-ttu-id="63f5a-352">Pokud máte v plánu zálohování úplné databáze tooexecute nad úložiště snímků, ujistěte se, že během této doby je zakázána hello provádění úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-352">If you plan tooexecute full database backups on top of storage snapshots, make sure that hello execution of storage snapshots is disabled during this time.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="63f5a-353">použití Hello úložiště snímků pro SAP HANA zálohy je platný jenom v případě, že hello snímky jsou prováděny ve spojení s zálohy protokolu SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="63f5a-353">hello use of storage snapshots for SAP HANA backups is valid only when hello snapshots are performed in conjunction with SAP HANA log backups.</span></span> <span data-ttu-id="63f5a-354">Tyto zálohy nutné toobe možné toocover hello protokolu časových období mezi hello úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-354">These log backups need toobe able toocover hello time periods between hello storage snapshots.</span></span> <span data-ttu-id="63f5a-355">Pokud jste nastavili toousers závazků v okamžiku obnovení 30 dní, je třeba hello následující:</span><span class="sxs-lookup"><span data-stu-id="63f5a-355">If you've set a commitment toousers of a point-in-time recovery of 30 days, you need hello following:</span></span>

- <span data-ttu-id="63f5a-356">Možnost tooaccess snímek úložiště, který je 30 dní.</span><span class="sxs-lookup"><span data-stu-id="63f5a-356">Ability tooaccess a storage snapshot that is 30 days old.</span></span>
- <span data-ttu-id="63f5a-357">Souvislý protokolu zálohování přes hello posledních 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="63f5a-357">Contiguous log backups over hello last 30 days.</span></span>

<span data-ttu-id="63f5a-358">V rozsahu hello záloh protokolu vytvořte snímek také hello protokol o zálohování svazku.</span><span class="sxs-lookup"><span data-stu-id="63f5a-358">In hello range of log backups, create a snapshot of hello backup log volume as well.</span></span> <span data-ttu-id="63f5a-359">Ale být zda tooperform protokolu pravidelných záloh, aby bylo možné:</span><span class="sxs-lookup"><span data-stu-id="63f5a-359">However, be sure tooperform regular log backups so that you can:</span></span>

- <span data-ttu-id="63f5a-360">Hello souvislý protokolu zálohy, musel tooperform v okamžiku obnovení.</span><span class="sxs-lookup"><span data-stu-id="63f5a-360">Have hello contiguous log backups needed tooperform point-in-time recoveries.</span></span>
- <span data-ttu-id="63f5a-361">Zabránit s nedostatkem místa na svazku protokolu hello SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="63f5a-361">Prevent hello SAP HANA log volume from running out of space.</span></span>

<span data-ttu-id="63f5a-362">Jeden z kroků poslední hello je tooschedule SAP HANA zálohování protokolů v SAP HANA Studio.</span><span class="sxs-lookup"><span data-stu-id="63f5a-362">One of hello last steps is tooschedule SAP HANA backup logs in SAP HANA Studio.</span></span> <span data-ttu-id="63f5a-363">Hello SAP HANA protokol o zálohování cílového místa je vytvořena speciálně hello hana nebo protokolu\_zálohování svazku s /hana/log/backups hello přípojný bod.</span><span class="sxs-lookup"><span data-stu-id="63f5a-363">hello SAP HANA backup log target destination is hello specially created hana/log\_backups volume with hello mount point of /hana/log/backups.</span></span>

![Plán zálohování SAP HANA přihlásí SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

<span data-ttu-id="63f5a-365">Můžete vybrat zálohování, které jsou častější, než každých 15 minut.</span><span class="sxs-lookup"><span data-stu-id="63f5a-365">You can choose backups that are more frequent than every 15 minutes.</span></span> <span data-ttu-id="63f5a-366">Někteří uživatelé i provést zálohování protokolu každou minutu, i když není doporučeno probíhající _přes_ 15 minut.</span><span class="sxs-lookup"><span data-stu-id="63f5a-366">Some users even perform log backups every minute, although we do not recommend going _over_ 15 minutes.</span></span>

<span data-ttu-id="63f5a-367">posledním krokem Hello je tooperform na základě souborů zálohování (po hello počáteční instalace SAP HANA) toocreate jeden zálohování položku, která musí existovat v rámci zálohování katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-367">hello final step is tooperform a file-based backup (after hello initial installation of SAP HANA) toocreate a single backup entry that must exist within hello backup catalog.</span></span> <span data-ttu-id="63f5a-368">V opačném případě SAP HANA nelze zahájit zadaného protokolu zálohování.</span><span class="sxs-lookup"><span data-stu-id="63f5a-368">Otherwise SAP HANA cannot initiate your specified log backups.</span></span>

![Proveďte zálohování toocreate na základě souborů jednu položku Zálohování](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)

### <a name="monitoring-hello-number-and-size-of-snapshots-on-hello-disk-volume"></a><span data-ttu-id="63f5a-370">Monitorování hello počet a velikost snímků na diskovém svazku hello</span><span class="sxs-lookup"><span data-stu-id="63f5a-370">Monitoring hello number and size of snapshots on hello disk volume</span></span>

<span data-ttu-id="63f5a-371">Na svazku konkrétní úložiště můžete sledovat počet hello snímky a spotřebu úložiště hello snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-371">On a particular storage volume, you can monitor hello number of snapshots and hello storage consumption of snapshots.</span></span> <span data-ttu-id="63f5a-372">Hello `ls` příkaz nezobrazí hello snímek adresáře nebo soubory.</span><span class="sxs-lookup"><span data-stu-id="63f5a-372">hello `ls` command doesn't show hello snapshot directory or files.</span></span> <span data-ttu-id="63f5a-373">Ale hello operační systém Linux příkaz `du` nemá s hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="63f5a-373">However, hello Linux OS command `du` does, with hello following commands:</span></span>

- <span data-ttu-id="63f5a-374">`du –sh .snapshot`poskytuje celkem všechny snímky v adresáři hello snímku.</span><span class="sxs-lookup"><span data-stu-id="63f5a-374">`du –sh .snapshot` provides a total of all snapshots within hello snapshot directory.</span></span>
- <span data-ttu-id="63f5a-375">`du –sh --max-depth=1`Zobrazí všechny snímky, které jsou uloženy ve složce .snapshot hello a hello velikost jednotlivých snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-375">`du –sh --max-depth=1` lists all snapshots that are saved in hello .snapshot folder and hello size of each snapshot.</span></span>
- <span data-ttu-id="63f5a-376">`du –hc`poskytuje hello celkovou velikost používané všechny snímky.</span><span class="sxs-lookup"><span data-stu-id="63f5a-376">`du –hc` provides hello total size used by all snapshots.</span></span>

<span data-ttu-id="63f5a-377">Použijte tyto příkazy toomake jistotu, že nejsou hello snímky, které jsou provedeny a uloženy využívá všechny hello úložiště na svazcích hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-377">Use these commands toomake sure that hello snapshots that are taken and stored are not consuming all hello storage on hello volumes.</span></span>

### <a name="reducing-hello-number-of-snapshots-on-a-server"></a><span data-ttu-id="63f5a-378">Snižuje hello počet snímků na serveru</span><span class="sxs-lookup"><span data-stu-id="63f5a-378">Reducing hello number of snapshots on a server</span></span>

<span data-ttu-id="63f5a-379">Jak je popsáno výše, můžete snížit počet hello určité popisky snímků, které jsou uloženy.</span><span class="sxs-lookup"><span data-stu-id="63f5a-379">As explained earlier, you can reduce hello number of certain labels of snapshots that you store.</span></span> <span data-ttu-id="63f5a-380">Hello poslední dva parametry tooinitiate příkaz hello snímku se popisek a hello počet snímků, které chcete tooretain.</span><span class="sxs-lookup"><span data-stu-id="63f5a-380">hello last two parameters of hello command tooinitiate a snapshot are a label and hello number of snapshots you want tooretain.</span></span>
```
./azure_hana_backup.pl lhanad01 customer 20
```
<span data-ttu-id="63f5a-381">V předchozím příkladu hello popisek snímku hello je _zákazníka_ a hello počet snímků, se tento štítek toobe uchovávají _20_.</span><span class="sxs-lookup"><span data-stu-id="63f5a-381">In hello previous example, hello snapshot label is _customer_ and hello number of snapshots with this label toobe retained is _20_.</span></span> <span data-ttu-id="63f5a-382">Jak můžete reagovat spotřeba toodisk místa, můžete chtít tooreduce hello počet uložené snímky.</span><span class="sxs-lookup"><span data-stu-id="63f5a-382">As you respond toodisk space consumption, you might want tooreduce hello number of stored snapshots.</span></span> <span data-ttu-id="63f5a-383">snadno Hello tooreduce hello počet snímků je toorun hello skriptu s hello poslední parametr sady too5:</span><span class="sxs-lookup"><span data-stu-id="63f5a-383">hello easy way tooreduce hello number of snapshots is toorun hello script with hello last parameter set too5:</span></span>
```
./azure_hana_backup.pl lhanad01 customer 5
```
<span data-ttu-id="63f5a-384">V důsledku spouštění skriptu hello s tímto nastavením, hello počet snímků, včetně nového úložiště hello snímku, je _5_.</span><span class="sxs-lookup"><span data-stu-id="63f5a-384">As a result of running hello script with this setting, hello number of snapshots, including hello new storage snapshot, is _5_.</span></span>

 >[!NOTE]
 > <span data-ttu-id="63f5a-385">Tento skript snižuje hello počet snímků, pouze pokud je více než jednu hodinu stará hello nejnovější předchozí snímek.</span><span class="sxs-lookup"><span data-stu-id="63f5a-385">This script reduces hello number of snapshots only if hello most recent previous snapshot is more than one hour old.</span></span> <span data-ttu-id="63f5a-386">skript Hello nedojde k odstranění snímků, které jsou menší než hodinu stará.</span><span class="sxs-lookup"><span data-stu-id="63f5a-386">hello script does not delete snapshots that are less than one hour old.</span></span>

<span data-ttu-id="63f5a-387">Tato omezení jsou související toohello volitelné zotavení po havárii funkce nabízené.</span><span class="sxs-lookup"><span data-stu-id="63f5a-387">These restrictions are related toohello optional disaster-recovery functionality offered.</span></span>

<span data-ttu-id="63f5a-388">Pokud již nechcete toomaintain sadu snímky s tuto předponu, můžete spustit skript hello se _0_ jako hello uchování číslo tooremove všechny snímky odpovídající tuto předponu.</span><span class="sxs-lookup"><span data-stu-id="63f5a-388">If you no longer want toomaintain a set of snapshots with that prefix, you can execute hello script with _0_ as hello retention number tooremove all snapshots matching that prefix.</span></span> <span data-ttu-id="63f5a-389">Odebrání všechny snímky však může ovlivnit hello možnosti zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="63f5a-389">However, removing all snapshots can affect hello capabilities of disaster recovery.</span></span>

### <a name="recovering-toohello-most-recent-hana-snapshot"></a><span data-ttu-id="63f5a-390">Obnovení toohello nejnovější HANA snímku</span><span class="sxs-lookup"><span data-stu-id="63f5a-390">Recovering toohello most recent HANA snapshot</span></span>

<span data-ttu-id="63f5a-391">V případě hello zaznamenáte rozbalovací produkční scénář, hello procesem obnovení ze snímku úložiště lze inicializovat jako zákazník incidentu s SAP HANA na Azure Service Management.</span><span class="sxs-lookup"><span data-stu-id="63f5a-391">In hello event that you experience a production-down scenario, hello process of recovering from a storage snapshot can be initiated as a customer incident with SAP HANA on Azure Service Management.</span></span> <span data-ttu-id="63f5a-392">Neočekávané scénáři může být vysokou naléhavost věci, pokud byla data odstraněna produkční systému a hello pouze způsobem tooretrieve hello dat je toorestore hello produkční databázi.</span><span class="sxs-lookup"><span data-stu-id="63f5a-392">Such an unexpected scenario might be a high-urgency matter if data was deleted in a production system and hello only way tooretrieve hello data is toorestore hello production database.</span></span>

<span data-ttu-id="63f5a-393">Na hello druhé straně, obnovení v okamžiku může být nízkou naléhavost a plánované předem dní.</span><span class="sxs-lookup"><span data-stu-id="63f5a-393">On hello other hand, a point-in-time recovery could be low urgency and planned for days in advance.</span></span> <span data-ttu-id="63f5a-394">Toto obnovení s SAP HANA můžete naplánovat na Azure Service Management místo vyvolání problém s vysokou prioritou.</span><span class="sxs-lookup"><span data-stu-id="63f5a-394">You can plan this recovery with SAP HANA on Azure Service Management instead of raising a high-priority issue.</span></span> <span data-ttu-id="63f5a-395">Například může plánování tootry upgrade hello SAP softwaru s použitím nového balíčku vylepšení a potom potřebovat toorevert zpět tooa snímek, který představuje stav hello před provedením upgradu hello EHP.</span><span class="sxs-lookup"><span data-stu-id="63f5a-395">For example, you might be planning tootry an upgrade of hello SAP software by applying a new enhancement package, and you then need toorevert back tooa snapshot that represents hello state before hello EHP upgrade.</span></span>

<span data-ttu-id="63f5a-396">Před vydáním hello žádost, musíte toodo určitou přípravu.</span><span class="sxs-lookup"><span data-stu-id="63f5a-396">Before you issue hello request, you need toodo some preparation.</span></span> <span data-ttu-id="63f5a-397">SAP HANA týmu Azure Service Management může potom hello požadavek zpracovat a zadejte hello obnovit svazky.</span><span class="sxs-lookup"><span data-stu-id="63f5a-397">SAP HANA on Azure Service Management team can then handle hello request and provide hello restored volumes.</span></span> <span data-ttu-id="63f5a-398">Potom je tooyou toorestore hello HANA databáze založené na hello snímky.</span><span class="sxs-lookup"><span data-stu-id="63f5a-398">Afterward, it is up tooyou toorestore hello HANA database based on hello snapshots.</span></span> <span data-ttu-id="63f5a-399">Tady je způsob tooprepare pro hello požadavků:</span><span class="sxs-lookup"><span data-stu-id="63f5a-399">Here is how tooprepare for hello request:</span></span>

>[!NOTE]
><span data-ttu-id="63f5a-400">Uživatelské rozhraní se můžou lišit od hello následující snímky obrazovky, v závislosti na hello verze SAP HANA, který používáte.</span><span class="sxs-lookup"><span data-stu-id="63f5a-400">Your user interface might vary from hello following screenshots, depending on hello SAP HANA release you are using.</span></span>

1. <span data-ttu-id="63f5a-401">Rozhodněte, které toorestore snímku.</span><span class="sxs-lookup"><span data-stu-id="63f5a-401">Decide which snapshot toorestore.</span></span> <span data-ttu-id="63f5a-402">Pokud jste se nedostanete budou obnoveny pouze hello hana nebo datový svazek.</span><span class="sxs-lookup"><span data-stu-id="63f5a-402">Only hello hana/data volume would be restored unless you are instructed otherwise.</span></span>

2. <span data-ttu-id="63f5a-403">Vypněte instanci HANA hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-403">Shut down hello HANA instance.</span></span>

 ![Vypnout instanci HANA hello](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. <span data-ttu-id="63f5a-405">Odpojte hello datové svazky na každém uzlu HANA databáze.</span><span class="sxs-lookup"><span data-stu-id="63f5a-405">Unmount hello data volumes on each HANA database node.</span></span> <span data-ttu-id="63f5a-406">Hello obnovení snímku hello selže, pokud nejsou hello datové svazky.</span><span class="sxs-lookup"><span data-stu-id="63f5a-406">hello restoration of hello snapshot fails if hello data volumes are not unmounted.</span></span>

 ![Odpojení hello datové svazky na každém uzlu databáze HANA](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. <span data-ttu-id="63f5a-408">Otevřete podporu Azure požadavek tooinstruct hello obnovení konkrétní snímku.</span><span class="sxs-lookup"><span data-stu-id="63f5a-408">Open an Azure support request tooinstruct hello restoration of a specific snapshot.</span></span>

 - <span data-ttu-id="63f5a-409">Při obnovení hello: SAP HANA na Azure Service Management může vás tooattend tooensure konferenční volání, která je ztratili žádná data.</span><span class="sxs-lookup"><span data-stu-id="63f5a-409">During hello restoration: SAP HANA on Azure Service Management might ask you tooattend a conference call tooensure that no data is getting lost.</span></span>

 - <span data-ttu-id="63f5a-410">Po obnovení hello: SAP HANA na Azure Service Management vás upozorní, jakmile byla obnovena hello úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-410">After hello restoration: SAP HANA on Azure Service Management notifies you when hello storage snapshot has been restored.</span></span>

5. <span data-ttu-id="63f5a-411">Po dokončení procesu obnovení hello znovu připojte všechny datové svazky.</span><span class="sxs-lookup"><span data-stu-id="63f5a-411">After hello restoration process is complete, remount all data volumes.</span></span>

 ![Znovu připojte všechny datové svazky](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. <span data-ttu-id="63f5a-413">Možnosti obnovení v rámci SAP HANA Studio, vyberte, pokud nepocházejí automaticky po opětovném tooHANA DB prostřednictvím SAP HANA Studio.</span><span class="sxs-lookup"><span data-stu-id="63f5a-413">Select recovery options within SAP HANA Studio, if they do not automatically come up when you reconnect tooHANA DB through SAP HANA Studio.</span></span> <span data-ttu-id="63f5a-414">Hello následující příklad ukazuje poslední HANA snímek toohello obnovení.</span><span class="sxs-lookup"><span data-stu-id="63f5a-414">hello following example shows a restoration toohello last HANA snapshot.</span></span> <span data-ttu-id="63f5a-415">Úložiště snímků Vloží jednu HANA snímku a když obnovujete toohello nejnovější úložiště snímků, měla by být hello nejnovější HANA snímku.</span><span class="sxs-lookup"><span data-stu-id="63f5a-415">A storage snapshot embeds one HANA snapshot, and if you are restoring toohello most recent storage snapshot, it should be hello most recent HANA snapshot.</span></span> <span data-ttu-id="63f5a-416">(Pokud obnovujete tooolder úložiště snímků, je třeba toolocate pořízení snímku HANA hello podle hello čas hello úložiště snímků.)</span><span class="sxs-lookup"><span data-stu-id="63f5a-416">(If you are restoring tooolder storage snapshots, you need toolocate hello HANA snapshot based on hello time hello storage snapshot was taken.)</span></span>

 ![Vyberte možnosti obnovení studia SAP HANA](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. <span data-ttu-id="63f5a-418">Vyberte **obnovit hello tooa konkrétní data zálohování nebo úložiště snímek databáze**.</span><span class="sxs-lookup"><span data-stu-id="63f5a-418">Select **Recover hello database tooa specific data backup or storage snapshot**.</span></span>

 ![okno "Zadejte typ obnovení" Hello](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. <span data-ttu-id="63f5a-420">Vyberte **zadejte zálohování bez katalogu**.</span><span class="sxs-lookup"><span data-stu-id="63f5a-420">Select **Specify backup without catalog**.</span></span>

 ![okno "Zadejte umístění zálohy" Hello](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. <span data-ttu-id="63f5a-422">V hello **cílový typ** seznamu, vyberte **snímku**.</span><span class="sxs-lookup"><span data-stu-id="63f5a-422">In hello **Destination Type** list, select **Snapshot**.</span></span>

 ![okno "Zadejte hello zálohování tooRecover" Hello](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. <span data-ttu-id="63f5a-424">Klikněte na tlačítko **Dokončit** proces obnovení toostart hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-424">Click **Finish** toostart hello recovery process.</span></span>

 ![Klikněte na tlačítko "Dokončit" proces obnovení toostart hello](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. <span data-ttu-id="63f5a-426">databáze HANA Hello obnovení a obnovit toohello HANA snímek, který je součástí hello úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-426">hello HANA database is restored and recovered toohello HANA snapshot that's included in hello storage snapshot.</span></span>

 ![HANA databáze je obnovena a obnovené toohello HANA snímku](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recovering-toohello-most-recent-state"></a><span data-ttu-id="63f5a-428">Obnovování stavu nejnovější toohello</span><span class="sxs-lookup"><span data-stu-id="63f5a-428">Recovering toohello most recent state</span></span>

<span data-ttu-id="63f5a-429">Hello následující proces obnoví hello HANA snímek, který je součástí hello úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-429">hello following process restores hello HANA snapshot that is included in hello storage snapshot.</span></span> <span data-ttu-id="63f5a-430">Poté obnoví hello protokolu zálohování toohello nejnovější stav transakce hello databáze před obnovením hello úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-430">It then restores hello transaction log backups toohello most recent state of hello database before restoring hello storage snapshot.</span></span>

>[!IMPORTANT]
><span data-ttu-id="63f5a-431">Než budete pokračovat, ujistěte se, že máte úplný a souvislý řetězec zálohy protokolu transakcí.</span><span class="sxs-lookup"><span data-stu-id="63f5a-431">Before you proceed, make sure that you have a complete and contiguous chain of transaction log backups.</span></span> <span data-ttu-id="63f5a-432">Bez tyto zálohy nelze obnovit, aktuální stav databáze hello hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-432">Without these backups, you cannot restore hello current state of hello database.</span></span>

1. <span data-ttu-id="63f5a-433">Proveďte kroky 1 – 6 hello předcházející postup v "Obnovení toohello nejnovější HANA snímek."</span><span class="sxs-lookup"><span data-stu-id="63f5a-433">Complete steps 1-6 of hello preceding procedure in "Recovering toohello most recent HANA snapshot."</span></span>

2. <span data-ttu-id="63f5a-434">Vyberte **obnovit nejnovější stav tooits hello databáze**.</span><span class="sxs-lookup"><span data-stu-id="63f5a-434">Select **Recover hello database tooits most recent state**.</span></span>

 ![Vyberte možnost "Obnovit nejnovější stav tooits hello databáze"](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. <span data-ttu-id="63f5a-436">Zadejte umístění hello hello nejnovější zálohy protokolu HANA.</span><span class="sxs-lookup"><span data-stu-id="63f5a-436">Specify hello location of hello most recent HANA log backups.</span></span> <span data-ttu-id="63f5a-437">umístění Hello musí toocontain z nejnovější stav hello HANA snímku toohello všechny zálohy protokolu transakcí HANA.</span><span class="sxs-lookup"><span data-stu-id="63f5a-437">hello location needs toocontain all HANA transaction log backups from hello HANA snapshot toohello most recent state.</span></span>

 ![Zadejte umístění hello hello nejnovější zálohy protokolu HANA](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. <span data-ttu-id="63f5a-439">Vyberte zálohu jako základní z které toorecover hello databáze.</span><span class="sxs-lookup"><span data-stu-id="63f5a-439">Select a backup as a base from which toorecover hello database.</span></span> <span data-ttu-id="63f5a-440">V našem příkladu je to hello HANA snímku, která byla součástí hello úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-440">In our example, this is hello HANA snapshot that was included in hello storage snapshot.</span></span> <span data-ttu-id="63f5a-441">(Jenom jeden snímek je uvedena v následující snímek obrazovky hello).</span><span class="sxs-lookup"><span data-stu-id="63f5a-441">(Only one snapshot is listed in hello following screenshot.)</span></span>

 ![Vyberte zálohu jako základní z které toorecover hello databáze](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. <span data-ttu-id="63f5a-443">Vymazat hello **použití rozdílové zálohy** zaškrtávací políčko, pokud nejsou k dispozici rozdílů mezi časem hello hello HANA snímku a nejnovější stav hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-443">Clear hello **Use Delta Backups** check box if deltas do not exist between hello time of hello HANA snapshot and hello most recent state.</span></span>

 ![Vymazat hello "Použití rozdílové zálohy" zaškrtávací políčko Pokud neexistují žádné rozdíly](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. <span data-ttu-id="63f5a-445">Na obrazovce Přehled hello, klikněte na tlačítko **Dokončit** postup obnovení toostart hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-445">On hello summary screen, click **Finish** toostart hello restoration procedure.</span></span>

 ![Klikněte na tlačítko "Dokončit" na obrazovce Přehled hello](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recovering-tooanother-point-in-time"></a><span data-ttu-id="63f5a-447">Obnovení tooanother bodu v čase</span><span class="sxs-lookup"><span data-stu-id="63f5a-447">Recovering tooanother point in time</span></span>
<span data-ttu-id="63f5a-448">toorecover tooa bodu v čase mezi hello HANA snímku (součástí snímku úložiště hello) a ten, který je novější než hello HANA snímku v okamžiku obnovení hello následující:</span><span class="sxs-lookup"><span data-stu-id="63f5a-448">toorecover tooa point in time between hello HANA snapshot (included in hello storage snapshot) and one that is later than hello HANA snapshot point-in-time recovery, do hello following:</span></span>

1. <span data-ttu-id="63f5a-449">Ujistěte se, že máte všechny zálohy protokolu transakcí z hello HANA snímku toohello dobu, kterou chcete toorecover.</span><span class="sxs-lookup"><span data-stu-id="63f5a-449">Make sure that you have all transaction log backups from hello HANA snapshot toohello time you want toorecover.</span></span>
2. <span data-ttu-id="63f5a-450">Začněte hello postupem v části "Stavu nejnovější obnovování toohello."</span><span class="sxs-lookup"><span data-stu-id="63f5a-450">Begin hello procedure under "Recovering toohello most recent state."</span></span>
3. <span data-ttu-id="63f5a-451">V kroku 2 postupu hello v hello **zadejte typ obnovení** vyberte **následující toohello databáze hello obnovení bodu v čase**, zadejte hello bodu v čase a potom proveďte kroky 3 až 6.</span><span class="sxs-lookup"><span data-stu-id="63f5a-451">In step 2 of hello procedure, in hello **Specify Recovery Type** window, select **Recover hello database toohello following point in time**, specify hello point in time, and then complete steps 3-6.</span></span>

## <a name="monitoring-hello-execution-of-snapshots"></a><span data-ttu-id="63f5a-452">Monitorování provádění hello snímků</span><span class="sxs-lookup"><span data-stu-id="63f5a-452">Monitoring hello execution of snapshots</span></span>

<span data-ttu-id="63f5a-453">Je třeba spuštění hello toomonitor úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="63f5a-453">You need toomonitor hello execution of storage snapshots.</span></span> <span data-ttu-id="63f5a-454">zapíše výstupní soubor tooa Hello skript, který provede snímku úložiště a uloží ho toohello stejné umístění jako hello tyto skripty.</span><span class="sxs-lookup"><span data-stu-id="63f5a-454">hello script that executes a storage snapshot writes output tooa file and then saves it toohello same location as hello Perl scripts.</span></span> <span data-ttu-id="63f5a-455">Samostatný soubor je napsán pro každý snímek.</span><span class="sxs-lookup"><span data-stu-id="63f5a-455">A separate file is written for each snapshot.</span></span> <span data-ttu-id="63f5a-456">výstup Hello každého souboru jasně ukazuje hello různých fází, které se spustí skript snímku hello:</span><span class="sxs-lookup"><span data-stu-id="63f5a-456">hello output of each file clearly shows hello various phases that hello snapshot script executes:</span></span>

- <span data-ttu-id="63f5a-457">Hledání hello svazky, které je třeba toocreate snímku</span><span class="sxs-lookup"><span data-stu-id="63f5a-457">Finding hello volumes that need toocreate a snapshot</span></span>
- <span data-ttu-id="63f5a-458">Hledání hello snímky převzaty z těchto svazků</span><span class="sxs-lookup"><span data-stu-id="63f5a-458">Finding hello snapshots taken from these volumes</span></span>
- <span data-ttu-id="63f5a-459">Odstranění případné existující snímky toomatch hello počet snímků, které jste zadali</span><span class="sxs-lookup"><span data-stu-id="63f5a-459">Deleting eventual existing snapshots toomatch hello number of snapshots you specified</span></span>
- <span data-ttu-id="63f5a-460">Vytvoření snímku HANA</span><span class="sxs-lookup"><span data-stu-id="63f5a-460">Creating a HANA snapshot</span></span>
- <span data-ttu-id="63f5a-461">Vytvoření snímku úložiště hello přes hello svazky</span><span class="sxs-lookup"><span data-stu-id="63f5a-461">Creating hello storage snapshot over hello volumes</span></span>
- <span data-ttu-id="63f5a-462">Odstranění snímku HANA hello</span><span class="sxs-lookup"><span data-stu-id="63f5a-462">Deleting hello HANA snapshot</span></span>
- <span data-ttu-id="63f5a-463">Přejmenování hello poslední snímek příliš**.0**</span><span class="sxs-lookup"><span data-stu-id="63f5a-463">Renaming hello most recent snapshot too**.0**</span></span>

<span data-ttu-id="63f5a-464">Hello nejdůležitější část skriptu hello jsou tyto:</span><span class="sxs-lookup"><span data-stu-id="63f5a-464">hello most important part of hello script is this:</span></span>
```
**********************Creating HANA snapshot**********************
Creating hello HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
HANA snapshot created successfully.
**********************Creating Storage snapshot**********************
Taking snapshot hourly.recent for hana_data_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_backup_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_shared_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for sapmnt_lhanad01_t020_vol ...
Snapshot created successfully.
**********************Deleting HANA snapshot**********************
Deleting hello HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
<span data-ttu-id="63f5a-465">Zobrazí se od této ukázky jak hello skriptu záznamy hello vytvoření snímku HANA hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-465">You can see from this sample how hello script records hello creation of hello HANA snapshot.</span></span> <span data-ttu-id="63f5a-466">V případě hello Škálováním na více systémů je tento proces zahájit v hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="63f5a-466">In hello scale-out case, this process is initiated on hello master node.</span></span> <span data-ttu-id="63f5a-467">hlavní uzel Hello zahájí hello synchronní vytváření snímků hello na všechny uzly pracovního procesu hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-467">hello master node initiates hello synchronous creation of hello snapshots on each of hello worker nodes.</span></span> <span data-ttu-id="63f5a-468">Potom pořízení snímku úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="63f5a-468">Then hello storage snapshot is taken.</span></span> <span data-ttu-id="63f5a-469">Po úspěšném spuštění hello hello úložiště snímků je odstraněn hello HANA snímek.</span><span class="sxs-lookup"><span data-stu-id="63f5a-469">After hello successful execution of hello storage snapshots, hello HANA snapshot is deleted.</span></span>
