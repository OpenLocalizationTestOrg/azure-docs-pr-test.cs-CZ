---
title: "Vysoká dostupnost a zotavení po havárii systému SAP HANA v Azure (velké instance) | Microsoft Docs"
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
ms.openlocfilehash: f95e944fc3ec3a831d97386443eb644420ae54dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a><span data-ttu-id="901c6-103">SAP HANA (velké instance) vysoké dostupnosti a zotavení po havárii v Azure</span><span class="sxs-lookup"><span data-stu-id="901c6-103">SAP HANA (large instances) high availability and disaster recovery on Azure</span></span> 

<span data-ttu-id="901c6-104">Vysoká dostupnost a zotavení po havárii jsou důležité aspekty vaše důležité SAP HANA spuštěných na serverech Azure (velké instance).</span><span class="sxs-lookup"><span data-stu-id="901c6-104">High availability and disaster recovery are important aspects of running your mission-critical SAP HANA on Azure (large instances) servers.</span></span> <span data-ttu-id="901c6-105">Je důležité pro práci s SAP, vaše systémový Integrátor nebo Microsoft správně architektury a implementovat strategie právo vysokou dostupnosti nebo zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="901c6-105">It's important to work with SAP, your system integrator, or Microsoft to properly architect and implement the right high-availability/disaster-recovery strategy.</span></span> <span data-ttu-id="901c6-106">Také je potřeba vzít v úvahu plánovaného bodu obnovení a cíli času obnovení, které jsou specifické pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="901c6-106">It is also important to consider the recovery point objective and recovery time objective, which are specific to your environment.</span></span>

## <a name="high-availability"></a><span data-ttu-id="901c6-107">Vysoká dostupnost</span><span class="sxs-lookup"><span data-stu-id="901c6-107">High availability</span></span>

<span data-ttu-id="901c6-108">Společnost Microsoft podporuje SAP HANA vysokou dostupnost metody "se na pole a", mezi které patří:</span><span class="sxs-lookup"><span data-stu-id="901c6-108">Microsoft supports SAP HANA high-availability methods "out of the box," which include:</span></span>

- <span data-ttu-id="901c6-109">**Replikace úložiště:** schopnost systému úložiště replikovat všechna data do jiného umístění (v rámci, nebo odděleně od stejného datového centra).</span><span class="sxs-lookup"><span data-stu-id="901c6-109">**Storage replication:** The storage system's ability to replicate all data to another location (within, or separate from, the same datacenter).</span></span> <span data-ttu-id="901c6-110">SAP HANA funguje nezávisle na tuto metodu.</span><span class="sxs-lookup"><span data-stu-id="901c6-110">SAP HANA operates independently of this method.</span></span>
- <span data-ttu-id="901c6-111">**Replikace systému HANA:** replikace všech dat v SAP HANA na samostatném systému SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-111">**HANA system replication:** The replication of all data in SAP HANA to a separate SAP HANA system.</span></span> <span data-ttu-id="901c6-112">Plánovanou dobu obnovení je minimalizován prostřednictvím replikace dat v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="901c6-112">The recovery time objective is minimized through data replication at regular intervals.</span></span> <span data-ttu-id="901c6-113">SAP HANA podporuje asynchronní, synchronní v paměti a synchronní režimy (doporučeno pouze pro SAP HANA systémy, které jsou ve stejném datovém centru nebo méně než 100 KM od sebe).</span><span class="sxs-lookup"><span data-stu-id="901c6-113">SAP HANA supports asynchronous, synchronous in-memory, and synchronous modes (recommended only for SAP HANA systems that are within the same datacenter or less than 100 KM apart).</span></span> <span data-ttu-id="901c6-114">V aktuální návrhu HANA velké instance razítka replikaci systému HANA slouží pouze pro vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="901c6-114">In the current design of HANA large-instance stamps, HANA system replication can be used for high availability only.</span></span>
- <span data-ttu-id="901c6-115">**Automatické převzetí služeb při selhání hostitele:** místní selhání obnovení řešení použít jako alternativu k replikaci systému.</span><span class="sxs-lookup"><span data-stu-id="901c6-115">**Host auto-failover:** A local fault-recovery solution to use as an alternative to system replication.</span></span> <span data-ttu-id="901c6-116">Pokud hlavní uzel stane nedostupným, jsou v režimu Škálováním na více systémů nakonfigurovat jeden nebo více uzlů SAP HANA pohotovostní a SAP HANA automaticky převezme jiný uzel.</span><span class="sxs-lookup"><span data-stu-id="901c6-116">When the master node becomes unavailable, one or more standby SAP HANA nodes are configured in scale-out mode and SAP HANA automatically fails over to another node.</span></span>

<span data-ttu-id="901c6-117">Další informace o vysoké dostupnosti SAP HANA najdete následující informace SAP:</span><span class="sxs-lookup"><span data-stu-id="901c6-117">For more information on SAP HANA high availability, see the following SAP information:</span></span>

- [<span data-ttu-id="901c6-118">SAP HANA vysokou dostupnost dokument White Paper</span><span class="sxs-lookup"><span data-stu-id="901c6-118">SAP HANA High-Availability Whitepaper</span></span>](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [<span data-ttu-id="901c6-119">Příručka pro správu SAP HANA</span><span class="sxs-lookup"><span data-stu-id="901c6-119">SAP HANA Administration Guide</span></span>](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [<span data-ttu-id="901c6-120">SAP Academy Video o replikaci systému SAP HANA</span><span class="sxs-lookup"><span data-stu-id="901c6-120">SAP Academy Video on SAP HANA System Replication</span></span>](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [<span data-ttu-id="901c6-121">SAP podporu Poznámka #1999880 – nejčastější dotazy na replikaci systému SAP HANA</span><span class="sxs-lookup"><span data-stu-id="901c6-121">SAP Support Note #1999880 – FAQ on SAP HANA System Replication</span></span>](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- <span data-ttu-id="901c6-122">[SAP podporu Poznámka #2165547 – SAP HANA zálohování a obnovení v rámci prostředí replikace systému SAP HANA](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)</span><span class="sxs-lookup"><span data-stu-id="901c6-122">[SAP Support Note #2165547 – SAP HANA Backup and Restore within SAP HANA System Replication Environment](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)</span></span>
- <span data-ttu-id="901c6-123">[Pro Exchange hardwaru minimální nebo nula. výpadků SAP podporu Poznámka #1984882 – pomocí replikace systému SAP HANA](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)</span><span class="sxs-lookup"><span data-stu-id="901c6-123">[SAP Support Note #1984882 – Using SAP HANA System Replication for Hardware Exchange with Minimum/Zero Downtime](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)</span></span>

## <a name="disaster-recovery"></a><span data-ttu-id="901c6-124">Zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="901c6-124">Disaster recovery</span></span>

<span data-ttu-id="901c6-125">SAP HANA v Azure (velké instance) je k dispozici v dvě Azure oblasti v geopolitické oblasti.</span><span class="sxs-lookup"><span data-stu-id="901c6-125">SAP HANA on Azure (large instances) is offered in two Azure regions in a geopolitical region.</span></span> <span data-ttu-id="901c6-126">Mezi dvěma velké instance razítka ze dvou různých oblastech je přímého síťového připojení pro replikaci dat během zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="901c6-126">Between the two large-instance stamps of two different regions is a direct network connectivity for replicating data during disaster recovery.</span></span> <span data-ttu-id="901c6-127">Replikace dat je na základě infrastruktury úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-127">The replication of the data is storage-infrastructure based.</span></span> <span data-ttu-id="901c6-128">Ve výchozím nastavení se provádí replikace.</span><span class="sxs-lookup"><span data-stu-id="901c6-128">The replication is not done by default.</span></span> <span data-ttu-id="901c6-129">Dokončí konfigurace zákazníka, které seřazené zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="901c6-129">It is done for the customer configurations that ordered the disaster recovery.</span></span> <span data-ttu-id="901c6-130">V aktuální návrhu HANA systému replikace nelze použít pro zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="901c6-130">In the current design, HANA system replication can't be used for disaster recovery.</span></span>

<span data-ttu-id="901c6-131">Však využít výhod zotavení po havárii, je nutné spustit návrhu připojení k síti na dvou různých oblastech Azure.</span><span class="sxs-lookup"><span data-stu-id="901c6-131">However, to take advantage of the disaster recovery, you need to start to design the network connectivity to the two different Azure regions.</span></span> <span data-ttu-id="901c6-132">To pokud chcete udělat, je třeba připojení okruh Azure ExpressRoute z místního v vaši hlavní oblast Azure a jiný okruh připojení z lokálních pro vaši oblast zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="901c6-132">To do so, you need an Azure ExpressRoute circuit connection from on-premises in your main Azure region and another circuit connection from on-premises to your disaster-recovery region.</span></span> <span data-ttu-id="901c6-133">Tato míra by mělo obsahovat situace, ve kterém k dokončení oblasti Azure, včetně umístění směrovače (MSEE) Microsoft enterprise edge, má problém.</span><span class="sxs-lookup"><span data-stu-id="901c6-133">This measure would cover a situation in which a complete Azure region, including a Microsoft enterprise edge router (MSEE) location, has an issue.</span></span>

<span data-ttu-id="901c6-134">Jako druhá míra se můžete připojit virtuální sítě Azure, která se připojují k SAP HANA v Azure (velké instance) v jedné oblasti do obou těchto okruhy ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="901c6-134">As a second measure, you can connect all Azure virtual networks that connect to SAP HANA on Azure (large instances) in one of the regions to both of those ExpressRoute circuits.</span></span> <span data-ttu-id="901c6-135">Tato míra řeší případ, kdy jenom jedna z umístění MSEE, který se připojí vaše místní umístění pomocí Azure ocitne mimo službu.</span><span class="sxs-lookup"><span data-stu-id="901c6-135">This measure addresses a case where only one of the MSEE locations that connects your on-premises location with Azure goes off duty.</span></span>

<span data-ttu-id="901c6-136">Následující obrázek znázorňuje optimální konfigurace pro zotavení po havárii:</span><span class="sxs-lookup"><span data-stu-id="901c6-136">The following figure shows the optimal configuration for disaster recovery:</span></span>

![Optimální konfigurace pro zotavení po havárii](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)

<span data-ttu-id="901c6-138">Optimální případ zotavení po havárii konfigurace sítě je mít dva okruhy ExpressRoute z místního na dvou různých oblastech Azure.</span><span class="sxs-lookup"><span data-stu-id="901c6-138">The optimal case for a disaster-recovery configuration of the network is to have two ExpressRoute circuits from on-premises to the two different Azure regions.</span></span> <span data-ttu-id="901c6-139">Jeden okruh přejde na oblast #1 spuštěna instance produkční.</span><span class="sxs-lookup"><span data-stu-id="901c6-139">One circuit goes to region #1, running a production instance.</span></span> <span data-ttu-id="901c6-140">Druhý okruh ExpressRoute přejde na oblast #2, spuštěné některé instance HANA nevýrobní prostředí.</span><span class="sxs-lookup"><span data-stu-id="901c6-140">The second ExpressRoute circuit goes to region #2, running some non-production HANA instances.</span></span> <span data-ttu-id="901c6-141">(To je důležité, pokud celou oblast Azure, včetně MSEE a velkých instance razítka, přejde vypnout mřížky.)</span><span class="sxs-lookup"><span data-stu-id="901c6-141">(This is important if an entire Azure region, including the MSEE and large-instance stamp, goes off the grid.)</span></span>

<span data-ttu-id="901c6-142">Jako druhá míra jsou různých virtuálních sítí připojené k různým okruhy ExpressRoute, které jsou připojené k SAP HANA v Azure (velké instance).</span><span class="sxs-lookup"><span data-stu-id="901c6-142">As a second measure, the various virtual networks are connected to the various ExpressRoute circuits that are connected to SAP HANA on Azure (large instances).</span></span> <span data-ttu-id="901c6-143">Místo, kde MSEE selhává, nebo můžete snížit plánovaného bodu obnovení pro zotavení po havárii můžete obejít probereme později.</span><span class="sxs-lookup"><span data-stu-id="901c6-143">You can bypass the location where an MSEE is failing, or you can lower the recovery point objective for disaster recovery, as we discuss later.</span></span>

<span data-ttu-id="901c6-144">Další požadavky pro instalaci zotavení po havárii jsou:</span><span class="sxs-lookup"><span data-stu-id="901c6-144">The next requirements for a disaster-recovery setup are:</span></span>

- <span data-ttu-id="901c6-145">Musí pořadí SAP HANA na Azure (velké instance) SKU stejnou velikost jako provozním SKU a nasaďte je do oblasti zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="901c6-145">You must order SAP HANA on Azure (large instances) SKUs of the same size as your production SKUs and deploy them in the disaster-recovery region.</span></span> <span data-ttu-id="901c6-146">Tyto instance slouží ke spuštění testu, izolovaného prostoru nebo QA HANA instance.</span><span class="sxs-lookup"><span data-stu-id="901c6-146">These instances can be used to run test, sandbox, or QA HANA instances.</span></span>
- <span data-ttu-id="901c6-147">Musíte pro každou z vaší SAP HANA na SKU Azure (velké instance), které chcete obnovit v lokalitě zotavení po havárii v případě potřeby uspořádat profil zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="901c6-147">You must order a disaster-recovery profile for each of your SAP HANA on Azure (large instances) SKUs that you want to recover in the disaster-recovery site, if necessary.</span></span> <span data-ttu-id="901c6-148">Tato akce vede k přidělení úložiště svazky, které jsou cílem replikace úložiště z vaší oblasti produkční do oblasti zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="901c6-148">This action leads to the allocation of storage volumes, which are the target of the storage replication from your production region into the disaster-recovery region.</span></span>

<span data-ttu-id="901c6-149">Pokud jsou splněny požadavky na předchozí je vaší povinností spustit replikaci úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-149">After you meet the preceding requirements, it is your responsibility to start the storage replication.</span></span> <span data-ttu-id="901c6-150">V infrastruktuře úložiště používá pro SAP HANA v Azure (velké instance) je základ replikace úložiště snímků úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-150">In the storage infrastructure used for SAP HANA on Azure (large instances), the basis of storage replication is storage snapshots.</span></span> <span data-ttu-id="901c6-151">Pokud chcete spustit replikaci zotavení po havárii, budete muset provést:</span><span class="sxs-lookup"><span data-stu-id="901c6-151">To start the disaster-recovery replication, you need to perform:</span></span>

- <span data-ttu-id="901c6-152">Snímek spouštěcích logické jednotky, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="901c6-152">A snapshot of your boot LUN, as described earlier.</span></span>
- <span data-ttu-id="901c6-153">Snímek související HANA svazků, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="901c6-153">A snapshot of your HANA-related volumes, as described earlier.</span></span>

<span data-ttu-id="901c6-154">Po provedení těchto snímků počáteční repliky svazků se nasadí na svazcích, které jsou spojeny s profilem zotavení po havárii v oblasti zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="901c6-154">After you execute these snapshots, an initial replica of the volumes is seeded on the volumes that are associated with your disaster-recovery profile in the disaster-recovery region.</span></span>

<span data-ttu-id="901c6-155">Následně se používá nejnovější snímku úložiště každou hodinu pro replikaci rozdílů, které vývoji na svazky úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-155">Subsequently, the most recent storage snapshot is used every hour to replicate the deltas that develop on the storage volumes.</span></span>

<span data-ttu-id="901c6-156">Cíl bodu obnovení, které je dosaženo pomocí této konfigurace se od 60 až 90 minut.</span><span class="sxs-lookup"><span data-stu-id="901c6-156">The recovery point objective that's achieved with this configuration is from 60 to 90 minutes.</span></span> <span data-ttu-id="901c6-157">K dosažení lepšího plánovaného bodu obnovení v případě zotavení po havárii, zkopírujte zálohování transakčního protokolu HANA z SAP HANA v Azure (velké instance) oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="901c6-157">To achieve a better recovery point objective in the disaster-recovery case, copy the HANA transaction log backups from SAP HANA on Azure (large instances) to the other Azure region.</span></span> <span data-ttu-id="901c6-158">K dosažení tohoto cíle bodu obnovení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="901c6-158">To achieve this recovery point objective, do the following:</span></span>

1. <span data-ttu-id="901c6-159">Zálohování transakce HANA protokolu možné se často jako /hana/log/backup.</span><span class="sxs-lookup"><span data-stu-id="901c6-159">Back up the HANA transaction log as frequently as possible to /hana/log/backup.</span></span>
2. <span data-ttu-id="901c6-160">Zkopírujte zálohování transakčního protokolu, po ukončení práce do Azure virtuálního počítače (VM), který je ve virtuální síti, která je připojena k SAP HANA na serveru Azure (velké instance).</span><span class="sxs-lookup"><span data-stu-id="901c6-160">Copy the transaction log backups when they are finished to an Azure virtual machine (VM), which is in a virtual network that's connected to the SAP HANA on Azure (large instances) server.</span></span>
3. <span data-ttu-id="901c6-161">Z tohoto virtuálního počítače zkopírujte zálohování na virtuální počítač, který je ve virtuální síti v oblasti zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="901c6-161">From that VM, copy the backup to a VM that's in a virtual network in the disaster-recovery region.</span></span>
4. <span data-ttu-id="901c6-162">Zachovat transakce zálohy protokolu v této oblasti ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="901c6-162">Keep the transaction log backups in that region in the VM.</span></span>

<span data-ttu-id="901c6-163">V případě havárie po nasazení profilu zotavení po havárii na skutečné serveru, zkopírujte zálohování transakčního protokolu z virtuálního počítače do SAP HANA v Azure (velké instance), který je teď primárním serverem v oblasti zotavení po havárii a ty obnovení zálohování.</span><span class="sxs-lookup"><span data-stu-id="901c6-163">In case of disaster, after the disaster-recovery profile has been deployed on an actual server, copy the transaction log backups from the VM to the SAP HANA on Azure (large instances) that is now the primary server in the disaster-recovery region, and restore those backups.</span></span> <span data-ttu-id="901c6-164">Toto obnovení je možné, protože stav HANA na discích zotavení po havárii je, že HANA snímku.</span><span class="sxs-lookup"><span data-stu-id="901c6-164">This recovery is possible because the state of HANA on the disaster-recovery disks is that of a HANA snapshot.</span></span> <span data-ttu-id="901c6-165">Toto je posunutí bod pro další obnovení zálohy protokolu transakcí.</span><span class="sxs-lookup"><span data-stu-id="901c6-165">This is the offset point for further restorations of transaction log backups.</span></span>

## <a name="backup-and-restore"></a><span data-ttu-id="901c6-166">Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="901c6-166">Backup and restore</span></span>

<span data-ttu-id="901c6-167">Jedním z nejdůležitějších aspektů do provozní databáze je Ujistěte se, že databáze se dají chránit z různých závažné události.</span><span class="sxs-lookup"><span data-stu-id="901c6-167">One of the most important aspects to operating databases is making sure the database can be protected from various catastrophic events.</span></span> <span data-ttu-id="901c6-168">Tyto události může být způsobeno nic z přírodní katastrofy chyby jednoduché uživatele.</span><span class="sxs-lookup"><span data-stu-id="901c6-168">These events can be caused by anything from natural disasters to simple user errors.</span></span>

<span data-ttu-id="901c6-169">Zálohování databáze, možnost obnovit do libovolného bodu v čase (například před odstraněním někdo důležitá data), umožňuje obnovení do stavu, který je co nejblíže k způsob, jakým ho byl před došlo k narušení.</span><span class="sxs-lookup"><span data-stu-id="901c6-169">Backing up a database, with the ability to restore it to any point in time (such as before somebody deleted critical data), allows restoration to a state that is as close as possible to the way it was before the disruption occurred.</span></span>

<span data-ttu-id="901c6-170">Pro dosažení co nejlepších výsledků se musí provádět dva typy zálohování:</span><span class="sxs-lookup"><span data-stu-id="901c6-170">Two types of backups must be performed for best results:</span></span>

- <span data-ttu-id="901c6-171">Zálohování databáze</span><span class="sxs-lookup"><span data-stu-id="901c6-171">Database backups</span></span>
- <span data-ttu-id="901c6-172">Zálohování transakčního protokolu</span><span class="sxs-lookup"><span data-stu-id="901c6-172">Transaction log backups</span></span>

<span data-ttu-id="901c6-173">Kromě zálohování úplné databáze provést na úrovni aplikace můžete být i důkladnější provedením zálohy snímků úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-173">In addition to full database backups performed at an application-level, you can be even more thorough by performing backups with storage snapshots.</span></span> <span data-ttu-id="901c6-174">Provádění záloh protokolu je také důležité k obnovení databáze (a pro prázdný protokoly z již potvrzené transakce).</span><span class="sxs-lookup"><span data-stu-id="901c6-174">Performing log backups is also important for restoring the database (and to empty the logs from already committed transactions).</span></span>

<span data-ttu-id="901c6-175">SAP HANA v Azure (velké instance) nabízí dvě možnosti pro zálohování a obnovení:</span><span class="sxs-lookup"><span data-stu-id="901c6-175">SAP HANA on Azure (large instances) offers two backup and restore options:</span></span>

- <span data-ttu-id="901c6-176">Provést sami (DIY).</span><span class="sxs-lookup"><span data-stu-id="901c6-176">Do it yourself (DIY).</span></span> <span data-ttu-id="901c6-177">Po můžete vypočítat zajistit dostatek místa na disku, proveďte úplné zálohování databáze a protokolu pomocí metody zálohy disku (na tyto disky).</span><span class="sxs-lookup"><span data-stu-id="901c6-177">After you calculate to ensure enough disk space, perform full database and log backups by using disk backup methods (to those disks).</span></span> <span data-ttu-id="901c6-178">V čase zálohování se zkopírují do účtu úložiště Azure (po nastavení Azure souborovém serveru s prakticky neomezené úložiště), nebo použijte trezoru zálohování Azure nebo úložiště Azure uloží málo používaná data.</span><span class="sxs-lookup"><span data-stu-id="901c6-178">Over time, the backups are copied to an Azure storage account (after you set up an Azure-based file server with virtually unlimited storage), or use an Azure Backup vault or Azure cold storage.</span></span> <span data-ttu-id="901c6-179">Další možností je použít nástroj ochrany dat třetích stran, jako je například Commvault, k ukládání záloh potom, co se zkopíruje na účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-179">Another option is to use a third-party data protection tool, such as Commvault, to store the backups after they are copied to a storage account.</span></span> <span data-ttu-id="901c6-180">DIY možnost zálohování může být také nutné pro data, která je potřeba uchovat po delší dobu pro dodržování předpisů a auditování.</span><span class="sxs-lookup"><span data-stu-id="901c6-180">The DIY backup option might also be necessary for data that needs to be stored for longer periods for compliancy and auditing purposes.</span></span>
- <span data-ttu-id="901c6-181">Použít funkci zálohování a obnovení funkce, která poskytuje základní infrastruktura SAP HANA v Azure (velké instance).</span><span class="sxs-lookup"><span data-stu-id="901c6-181">Use the backup and restore functionality that the underlying infrastructure of SAP HANA on Azure (large instances) provides.</span></span> <span data-ttu-id="901c6-182">Tuto možnost splňuje potřeby pro zálohování a ručního zálohování umožňuje téměř zastaralé (s výjimkou případů, kde jsou vyžadovány pro dodržování předpisů pro účely zálohování dat).</span><span class="sxs-lookup"><span data-stu-id="901c6-182">This option fulfills the need for backups, and it makes manual backups nearly obsolete (except where data backups are required for compliance purposes).</span></span> <span data-ttu-id="901c6-183">Zbývající část tohoto oddílu řeší funkci zálohování a obnovení, které nabízí s HANA (velké instance).</span><span class="sxs-lookup"><span data-stu-id="901c6-183">The rest of this section addresses the backup and restore functionality that's offered with HANA (large instances).</span></span>

> [!NOTE]
> <span data-ttu-id="901c6-184">Snímek technologie, která je používána základní infrastruktura HANA (velké instance) má závislost na snímky SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-184">The snapshot technology that is used by the underlying infrastructure of HANA (large instances) has a dependency on SAP HANA snapshots.</span></span> <span data-ttu-id="901c6-185">Snímky SAP HANA ve spojení s SAP HANA víceklientské databáze kontejnery nefungují.</span><span class="sxs-lookup"><span data-stu-id="901c6-185">SAP HANA snapshots do not work in conjunction with SAP HANA Multitenant Database Containers.</span></span> <span data-ttu-id="901c6-186">Tato metoda zálohování v důsledku toho nelze použít k nasazení SAP HANA víceklientské databáze kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="901c6-186">As a result, this method of backup cannot be used to deploy SAP HANA Multitenant Database Containers.</span></span>

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a><span data-ttu-id="901c6-187">Pomocí snímků úložiště SAP HANA v Azure (velké instance)</span><span class="sxs-lookup"><span data-stu-id="901c6-187">Using storage snapshots of SAP HANA on Azure (large instances)</span></span>

<span data-ttu-id="901c6-188">Infrastrukturu úložiště základní SAP HANA v Azure (velké instance) podporuje představu o úložiště snímků svazků.</span><span class="sxs-lookup"><span data-stu-id="901c6-188">The storage infrastructure underlying SAP HANA on Azure (large instances) supports the notion of a storage snapshot of volumes.</span></span> <span data-ttu-id="901c6-189">Zálohování a obnovení pro určitý svazek, podporují se následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="901c6-189">Both backup and restoration of a particular volume are supported, with the following considerations:</span></span>

- <span data-ttu-id="901c6-190">Místo zálohy databáze jsou na základě časté prováděné úložiště snímků svazků.</span><span class="sxs-lookup"><span data-stu-id="901c6-190">Instead of database backups, storage volume snapshots are taken on a frequent basis.</span></span>
- <span data-ttu-id="901c6-191">Úložiště snímků zahájí SAP HANA snímku, než se provede snímku úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-191">The storage snapshot initiates an SAP HANA snapshot before it executes the storage snapshot.</span></span> <span data-ttu-id="901c6-192">Tento snímek SAP HANA je bod instalační program pro případné protokolu obnovení po obnovení snímku úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-192">This SAP HANA snapshot is the setup point for eventual log restorations after recovery of the storage snapshot.</span></span>
- <span data-ttu-id="901c6-193">V místě, kde je snímku úložiště byl úspěšně proveden je odstraněn snímek SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-193">At the point where the storage snapshot is executed successfully, the SAP HANA snapshot is deleted.</span></span>
- <span data-ttu-id="901c6-194">Zálohy protokolů jsou často prováděné a uložená v zálohování svazku protokolu nebo v Azure.</span><span class="sxs-lookup"><span data-stu-id="901c6-194">Log backups are taken frequently and stored in the log backup volume or in Azure.</span></span>
- <span data-ttu-id="901c6-195">Pokud databázi musíte je obnovit do určité míry v čase, požadavku na podporu společnosti Microsoft Azure (produkční výpadek) nebo SAP HANA na Azure Service Management k obnovení určité úložiště snímků (například plánované obnovení systému izolovaného prostoru pro jeho původní stav).</span><span class="sxs-lookup"><span data-stu-id="901c6-195">If the database must be restored to a certain point in time, a request is made to Microsoft Azure Support (production outage) or SAP HANA on Azure Service Management to restore to a certain storage snapshot (for example, a planned restoration of a sandbox system to its original state).</span></span>
- <span data-ttu-id="901c6-196">SAP HANA snímek, který je součástí úložiště snímků je bod posunutí pro použití zálohy protokolu, které byly provedeny a uložené po pořízení snímku úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-196">The SAP HANA snapshot that's included in the storage snapshot is an offset point for applying log backups that have been executed and stored after the storage snapshot was taken.</span></span>
- <span data-ttu-id="901c6-197">Tyto zálohy protokolů jsou přesměrováni na Obnovit databázi zpět do určité míry v čase.</span><span class="sxs-lookup"><span data-stu-id="901c6-197">These log backups are taken to restore the database back to a certain point in time.</span></span>

<span data-ttu-id="901c6-198">Určení zálohování\_název vytvoří snímek následujících svazků:</span><span class="sxs-lookup"><span data-stu-id="901c6-198">Specifying the backup\_name creates a snapshot of the following volumes:</span></span>

- <span data-ttu-id="901c6-199">Hana nebo dat</span><span class="sxs-lookup"><span data-stu-id="901c6-199">hana/data</span></span>
- <span data-ttu-id="901c6-200">Hana a protokolování</span><span class="sxs-lookup"><span data-stu-id="901c6-200">hana/log</span></span>
- <span data-ttu-id="901c6-201">Hana nebo protokolu\_zálohování (připojit jako zálohování do hana nebo protokolu)</span><span class="sxs-lookup"><span data-stu-id="901c6-201">hana/log\_backup (mounted as backup into hana/log)</span></span>
- <span data-ttu-id="901c6-202">/ sdílené Hana</span><span class="sxs-lookup"><span data-stu-id="901c6-202">hana/shared</span></span>

### <a name="storage-snapshot-considerations"></a><span data-ttu-id="901c6-203">Aspekty volby úložiště snímků</span><span class="sxs-lookup"><span data-stu-id="901c6-203">Storage snapshot considerations</span></span>

>[!NOTE]
><span data-ttu-id="901c6-204">Úložiště snímků jsou _není_ poskytuje bezplatně, protože musí být přidělený dalšího volného místa.</span><span class="sxs-lookup"><span data-stu-id="901c6-204">Storage snapshots are _not_ provided free of charge, because additional storage space must be allocated.</span></span>

<span data-ttu-id="901c6-205">Konkrétní mechanismů úložiště snímků pro SAP HANA v Azure (velké instance) patří:</span><span class="sxs-lookup"><span data-stu-id="901c6-205">The specific mechanics of storage snapshots for SAP HANA on Azure (large instances) include:</span></span>

- <span data-ttu-id="901c6-206">Konkrétní úložiště snímků (v bodě v čase, kdy se provede) spotřebovává velmi málo úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-206">A specific storage snapshot (at the point in time when it is taken) consumes very little storage.</span></span>
- <span data-ttu-id="901c6-207">Jako obsahu změny dat a obsah SAP HANA data, která soubory změnit na svazek úložiště je potřeba uložit původní obsah bloku snímku.</span><span class="sxs-lookup"><span data-stu-id="901c6-207">As data content changes and the content in SAP HANA data files change on the storage volume, the snapshot needs to store the original block content.</span></span>
- <span data-ttu-id="901c6-208">Úložiště snímků nárůstu velikosti.</span><span class="sxs-lookup"><span data-stu-id="901c6-208">The storage snapshot increases in size.</span></span> <span data-ttu-id="901c6-209">Čím delší snímku existuje, tím větší bude snímku úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-209">The longer the snapshot exists, the larger the storage snapshot becomes.</span></span>
- <span data-ttu-id="901c6-210">Další změny provedené v průběhu životnosti úložiště snímku, čím vyšší stane spotřeby prostor úložiště snímku svazku databáze SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-210">The more changes made to the SAP HANA database volume over the lifetime of a storage snapshot, the larger the space consumption of the storage snapshot becomes.</span></span>

<span data-ttu-id="901c6-211">SAP HANA v Azure (velké instance) se dodává s velikostí pevný svazek pro SAP HANA svazek protokolu a data.</span><span class="sxs-lookup"><span data-stu-id="901c6-211">SAP HANA on Azure (large instances) comes with fixed volume sizes for the SAP HANA data and log volume.</span></span> <span data-ttu-id="901c6-212">Provádění snímky tyto svazky eats do místo ve svazku, takže je vaší povinností při plánování úložiště snímků (v rámci SAP HANA na proces Azure [velké instance]).</span><span class="sxs-lookup"><span data-stu-id="901c6-212">Performing snapshots of those volumes eats into your volume space, so it is your responsibility to schedule storage snapshots (within the SAP HANA on Azure [large instances] process).</span></span>

<span data-ttu-id="901c6-213">Následující části obsahují informace pro provedení tyto snímky, včetně obecná doporučení:</span><span class="sxs-lookup"><span data-stu-id="901c6-213">The following sections provide information for performing these snapshots, including general recommendations:</span></span>

- <span data-ttu-id="901c6-214">I když hardware tolerovat 255 snímků na svazku, důrazně doporučujeme, abyste zůstat dobře pod tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="901c6-214">Though the hardware can sustain 255 snapshots per volume, we highly recommend that you stay well below this number.</span></span>
- <span data-ttu-id="901c6-215">Před provedením úložiště snímků, monitorování a sledování volného místa.</span><span class="sxs-lookup"><span data-stu-id="901c6-215">Before you perform storage snapshots, monitor and keep track of free space.</span></span>
- <span data-ttu-id="901c6-216">Nižší počet snímků úložiště podle volného místa.</span><span class="sxs-lookup"><span data-stu-id="901c6-216">Lower the number of storage snapshots based on free space.</span></span> <span data-ttu-id="901c6-217">Možná budete muset snížit počet snímků, které můžete zachovat, nebo možná budete muset rozšířit svazky.</span><span class="sxs-lookup"><span data-stu-id="901c6-217">You might need to lower the number of snapshots that you keep, or you might need to extend the volumes.</span></span> <span data-ttu-id="901c6-218">(Další úložiště můžete uspořádat v jednotkách 1 TB.)</span><span class="sxs-lookup"><span data-stu-id="901c6-218">(You can order additional storage in 1-TB units.)</span></span>
- <span data-ttu-id="901c6-219">Během aktivity, například přesun dat do SAP HANA s nástroji pro migraci systému (s R3load nebo po obnovení databáze SAP HANA ze zálohy) důrazně doporučujeme nebude provádět všechny snímky úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-219">During activities such as moving data into SAP HANA with system migration tools (with R3load, or by restoring SAP HANA databases from backups), we highly recommended that you not perform any storage snapshots.</span></span> <span data-ttu-id="901c6-220">(Pokud migrace systému probíhá na nový systém SAP HANA, úložiště snímků nebude muset provést.)</span><span class="sxs-lookup"><span data-stu-id="901c6-220">(If a system migration is being done on a new SAP HANA system, storage snapshots would not need to be performed.)</span></span>
- <span data-ttu-id="901c6-221">Během větší uspořádání SAP HANA tabulek úložiště snímků je nutno Pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="901c6-221">During larger reorganizations of SAP HANA tables, storage snapshots should be avoided if possible.</span></span>
- <span data-ttu-id="901c6-222">Úložiště snímků jsou předpokladem pro zapojení možnosti zotavení po havárii SAP HANA v Azure (velké instance).</span><span class="sxs-lookup"><span data-stu-id="901c6-222">Storage snapshots are a prerequisite to engaging the disaster-recovery capabilities of SAP HANA on Azure (large instances).</span></span>

### <a name="setting-up-storage-snapshots"></a><span data-ttu-id="901c6-223">Nastavení úložiště snímků</span><span class="sxs-lookup"><span data-stu-id="901c6-223">Setting up storage snapshots</span></span>

1. <span data-ttu-id="901c6-224">Zajistěte, aby Perl nainstalovanou v operačním systému Linux na serveru HANA (velké instance).</span><span class="sxs-lookup"><span data-stu-id="901c6-224">Make sure that Perl is installed in the Linux operating system on the HANA (large instances) server.</span></span>
2. <span data-ttu-id="901c6-225">Upravit/etc/ssh/ssh\_konfigurace přidat řádek _Mac hmac-sha1_.</span><span class="sxs-lookup"><span data-stu-id="901c6-225">Modify /etc/ssh/ssh\_config to add the line _MACs hmac-sha1_.</span></span>
3. <span data-ttu-id="901c6-226">Vytvoření účtu uživatele zálohování SAP HANA na hlavní uzel pro každou instanci SAP HANA, spuštěný (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="901c6-226">Create an SAP HANA backup user account on the master node for each SAP HANA instance you are running (if applicable).</span></span>
4. <span data-ttu-id="901c6-227">Klient SAP HANA HDB musí být nainstalován na všech serverech SAP HANA (velké instance).</span><span class="sxs-lookup"><span data-stu-id="901c6-227">The SAP HANA HDB client must be installed on all SAP HANA (large instances) servers.</span></span>
5. <span data-ttu-id="901c6-228">Na prvním serveru SAP HANA (velké instance) každou oblast, musí být vytvořený veřejný klíč pro přístup k podkladové infrastruktury úložiště, který řídí vytváření snímku.</span><span class="sxs-lookup"><span data-stu-id="901c6-228">On the first SAP HANA (large instances) server of each region, a public key must be created to access the underlying storage infrastructure that controls snapshot creation.</span></span>
6. <span data-ttu-id="901c6-229">Zkopírujte skript azure\_hana\_backup.pl z/Scripts umístění **hdbsql** instalace SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-229">Copy the script azure\_hana\_backup.pl from /scripts to the location of **hdbsql** of the SAP HANA installation.</span></span>
7. <span data-ttu-id="901c6-230">Zkopírujte soubor HANABackupDetails.txt z/Scripts do stejného umístění jako skript Perlu.</span><span class="sxs-lookup"><span data-stu-id="901c6-230">Copy the HANABackupDetails.txt file from /scripts to the same location as the Perl script.</span></span>
8. <span data-ttu-id="901c6-231">Upravte soubor HANABackupDetails.txt v případě potřeby u specifikace odpovídající zákazníka.</span><span class="sxs-lookup"><span data-stu-id="901c6-231">Modify the HANABackupDetails.txt file as necessary for the appropriate customer specifications.</span></span>

### <a name="step-1-install-sap-hana-hdbclient"></a><span data-ttu-id="901c6-232">Krok 1: Instalace SAP HANA HDBClient</span><span class="sxs-lookup"><span data-stu-id="901c6-232">Step 1: Install SAP HANA HDBClient</span></span>

<span data-ttu-id="901c6-233">Linux nainstalován na SAP HANA v Azure (velké instance) obsahuje složky a skripty potřebné k provedení SAP HANA úložiště snímků pro účely zálohování a zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="901c6-233">The Linux installed on SAP HANA on Azure (large instances) includes the folders and scripts necessary to execute SAP HANA storage snapshots for backup and disaster-recovery purposes.</span></span> <span data-ttu-id="901c6-234">Je však za to, že instalaci SAP HANA HDBclient, když instalujete SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-234">However, it is your responsibility to install SAP HANA HDBclient while you are installing SAP HANA.</span></span> <span data-ttu-id="901c6-235">(Microsoft nainstaluje HDBclient ani SAP HANA.)</span><span class="sxs-lookup"><span data-stu-id="901c6-235">(Microsoft installs neither the HDBclient nor SAP HANA.)</span></span>

### <a name="step-2-change-etcsshsshconfig"></a><span data-ttu-id="901c6-236">Krok 2: Změnit/etc/ssh/ssh\_konfigurace</span><span class="sxs-lookup"><span data-stu-id="901c6-236">Step 2: Change /etc/ssh/ssh\_config</span></span>

<span data-ttu-id="901c6-237">Změnit/etc/ssh/ssh\_konfigurace přidáním _Mac hmac-sha1_ řádek, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="901c6-237">Change /etc/ssh/ssh\_config by adding _MACs hmac-sha1_ line as shown here:</span></span>
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

### <a name="step-3-create-a-public-key"></a><span data-ttu-id="901c6-238">Krok 3: Vytvoření veřejný klíč</span><span class="sxs-lookup"><span data-stu-id="901c6-238">Step 3: Create a public key</span></span>

<span data-ttu-id="901c6-239">Na první SAP HANA na serveru Azure (velké instance) v každé oblasti Azure vytvořte veřejný klíč, který se má použít pro přístup k infrastruktury úložiště, takže můžete vytvářet snímky.</span><span class="sxs-lookup"><span data-stu-id="901c6-239">On the first SAP HANA on Azure (large instances) server in each Azure region, create a public key to be used to access the storage infrastructure so that you can create snapshots.</span></span> <span data-ttu-id="901c6-240">Veřejný klíč zajistí, že pro přihlášení k úložišti není vyžadováno heslo a že nejsou spravovány heslo pověření.</span><span class="sxs-lookup"><span data-stu-id="901c6-240">The public key ensures that a password is not required to sign in to the storage and that password credentials are not maintained.</span></span> <span data-ttu-id="901c6-241">V systému Linux na serveru SAP HANA (velké instance) spusťte následující příkaz pro vytvoření veřejný klíč:</span><span class="sxs-lookup"><span data-stu-id="901c6-241">In Linux on the SAP HANA (large instances) server, execute the following command to generate the public key:</span></span>
```
  ssh-keygen –t dsa –b 1024
```
<span data-ttu-id="901c6-242">Nové umístění je _/root/.ssh/id\_dsa.pub.</span><span class="sxs-lookup"><span data-stu-id="901c6-242">The new location is _/root/.ssh/id\_dsa.pub.</span></span> <span data-ttu-id="901c6-243">Nezadávejte skutečné přístupové heslo, jinak bude nutné zadat heslo při každém přihlášení.</span><span class="sxs-lookup"><span data-stu-id="901c6-243">Do not enter an actual passphrase, or else you will be required to enter the passphrase each time you sign in.</span></span> <span data-ttu-id="901c6-244">Místo toho stiskněte **Enter** dvakrát možnosti odeberete požadavek zadejte přístupové heslo pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="901c6-244">Instead, press **Enter** twice to remove the enter passphrase requirement for signing in.</span></span>

<span data-ttu-id="901c6-245">Zkontrolujte, že veřejný klíč byl opraven podle očekávání změnou /root/.ssh/ složky a potom provádění **ls** příkaz.</span><span class="sxs-lookup"><span data-stu-id="901c6-245">Check to make sure that the public key was corrected as expected by changing folders to /root/.ssh/ and then executing the **ls** command.</span></span> <span data-ttu-id="901c6-246">Pokud je klíč přítomen, můžete ji zkopírovat tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="901c6-246">If the key is present, you can copy it by running the following command:</span></span>

![Veřejný klíč se zkopíruje spuštěním tohoto příkazu](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

<span data-ttu-id="901c6-248">V tomto okamžiku obraťte SAP HANA na Azure Service Management a zadat klíč.</span><span class="sxs-lookup"><span data-stu-id="901c6-248">At this point, contact SAP HANA on Azure Service Management and provide the key.</span></span> <span data-ttu-id="901c6-249">Zástupce služeb používá veřejný klíč a zaregistrujte ho v podkladové infrastruktury úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-249">The service representative uses the public key to register it in the underlying storage infrastructure.</span></span>

### <a name="step-4-create-an-sap-hana-user-account"></a><span data-ttu-id="901c6-250">Krok 4: Vytvoření účtu uživatele SAP HANA</span><span class="sxs-lookup"><span data-stu-id="901c6-250">Step 4: Create an SAP HANA user account</span></span>

<span data-ttu-id="901c6-251">Vytvořte uživatelský účet SAP HANA studia SAP HANA pro účely zálohování.</span><span class="sxs-lookup"><span data-stu-id="901c6-251">Create an SAP HANA user account within SAP HANA Studio for backup purposes.</span></span> <span data-ttu-id="901c6-252">Tento účet musí mít následující oprávnění: _správce zálohování_ a _katalogu čtení_.</span><span class="sxs-lookup"><span data-stu-id="901c6-252">This account must have the following privileges: _Backup Admin_ and _Catalog Read_.</span></span> <span data-ttu-id="901c6-253">V tomto příkladu uživatelské jméno SCADMIN se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="901c6-253">In this example, the username SCADMIN is created.</span></span>

![Vytvoření uživatele v HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

### <a name="step-5-authorize-the-sap-hana-user-account"></a><span data-ttu-id="901c6-255">Krok 5: Autorizace uživatelský účet SAP HANA</span><span class="sxs-lookup"><span data-stu-id="901c6-255">Step 5: Authorize the SAP HANA user account</span></span>

<span data-ttu-id="901c6-256">Autorizovat SAP HANA uživatelský účet (který se má použít skripty bez nutnosti autorizace pokaždé, když se skript spouští).</span><span class="sxs-lookup"><span data-stu-id="901c6-256">Authorize the SAP HANA user account (to be used by the scripts without requiring authorization every time the script is run).</span></span> <span data-ttu-id="901c6-257">Příkaz SAP HANA `hdbuserstore` umožňuje vytvoření SAP HANA uživatelský klíč, který je uložený na jeden nebo více uzlů SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-257">The SAP HANA command `hdbuserstore` allows the creation of an SAP HANA user key, which is stored on one or more SAP HANA nodes.</span></span> <span data-ttu-id="901c6-258">Uživatelský klíč také umožňuje uživateli přístup k SAP HANA bez nutnosti Správa hesel z v rámci skriptování procesu, který je popsán později.</span><span class="sxs-lookup"><span data-stu-id="901c6-258">The user key also allows the user to access SAP HANA without having to manage passwords from within the scripting process that's discussed later.</span></span>

>[!IMPORTANT]
><span data-ttu-id="901c6-259">Spusťte následující příkaz jako `_root_`.</span><span class="sxs-lookup"><span data-stu-id="901c6-259">Run the following command as `_root_`.</span></span> <span data-ttu-id="901c6-260">Skript, jinak hodnota nemůže pracovat správně.</span><span class="sxs-lookup"><span data-stu-id="901c6-260">Otherwise, the script cannot work properly.</span></span>

<span data-ttu-id="901c6-261">Zadejte `hdbuserstore` příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="901c6-261">Enter the `hdbuserstore` command as follows:</span></span>

![Zadejte příkaz hdbuserstore](./media/hana-overview-high-availability-disaster-recovery/image4-hdbuserstore-command.png)

<span data-ttu-id="901c6-263">V následujícím příkladu, kde je uživatel SCADMIN01 a název hostitele je lhanad01, příkaz je:</span><span class="sxs-lookup"><span data-stu-id="901c6-263">In the following example, where the user is SCADMIN01 and the hostname is lhanad01, the command is:</span></span>
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
<span data-ttu-id="901c6-264">Spravujte všechny skriptování na jednom serveru pro škálování HANA instance.</span><span class="sxs-lookup"><span data-stu-id="901c6-264">Manage all scripting from a single server for scale-out HANA instances.</span></span> <span data-ttu-id="901c6-265">V tomto příkladu musí být klíč SAP HANA SCADMIN01 změněn pro každého hostitele tak, aby odráží hostitele, která souvisí s klíč.</span><span class="sxs-lookup"><span data-stu-id="901c6-265">In this example, the SAP HANA key SCADMIN01 must be altered for each host in a way that reflects the host that is related to the key.</span></span> <span data-ttu-id="901c6-266">To znamená, účet záloh SAP HANA se mění se počet instancí HANA databáze, **lhanad**.</span><span class="sxs-lookup"><span data-stu-id="901c6-266">That is, the SAP HANA backup account is amended with the instance number of the HANA DB, **lhanad**.</span></span> <span data-ttu-id="901c6-267">Klíč musí mít oprávnění správce na hostiteli, který je přiřazen, a zálohování uživatele Škálováním na více systémů, musíte mít přístupová práva na všechny instance SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-267">The key must have administrative privileges on the host it is assigned to, and the backup user for scale-out must have access rights to all SAP HANA instances.</span></span>
```
hdbuserstore set SCADMIN01 lhanad:30015 SCADMIN <password>
hdbuserstore set SCADMIN02 lhanad:30115 SCADMIN <password>
hdbuserstore set SCADMIN03 lhanad:30215 SCADMIN <password>
```

### <a name="step-6-copy-items-from-the-scripts-folder"></a><span data-ttu-id="901c6-268">Krok 6: Kopírovat položky ze složky/Scripts</span><span class="sxs-lookup"><span data-stu-id="901c6-268">Step 6: Copy items from the /scripts folder</span></span>

<span data-ttu-id="901c6-269">Zkopírujte následující položky ze složky/Scripts (obsažené na bitovou kopii gold instalace) do pracovní adresář pro **hdbsql**.</span><span class="sxs-lookup"><span data-stu-id="901c6-269">Copy the following items from the /scripts folder (included on the gold image of the installation) to the working directory for **hdbsql**.</span></span> <span data-ttu-id="901c6-270">Pro aktuální HANA instalace, tento adresář se /hana/shared/D01/exe/linuxx86\_64/hdb.</span><span class="sxs-lookup"><span data-stu-id="901c6-270">For current HANA installations, this directory is /hana/shared/D01/exe/linuxx86\_64/hdb.</span></span>
```
azure\_hana\_backup.pl
testHANAConnection.pl
testStorageSnapshotConnection.pl
removeTestStorageSnapshot.pl
HANABackupCustomerDetails.txt
```
<span data-ttu-id="901c6-271">Pokud používají Škálováním na více systémů nebo OLAP, zkopírujte následující položky:</span><span class="sxs-lookup"><span data-stu-id="901c6-271">Copy the following items if they are running scale-out or OLAP:</span></span>
```
azure\_hana\_backup\_bw.pl
testHANAConnectionBW.pl
testStorageSnapshotConnectionBW.pl
removeTestStorageSnapshotBW.pl
HANABackupCustomerDetailsBW.txt
```
<span data-ttu-id="901c6-272">Soubor HANABackupCustomerDetails.txt je upravitelnými takto pro nasazení škálování.</span><span class="sxs-lookup"><span data-stu-id="901c6-272">The HANABackupCustomerDetails.txt file is modifiable as follows for a scale-up deployment.</span></span> <span data-ttu-id="901c6-273">Je ovládací prvek a konfigurační soubor pro skript, který běží úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="901c6-273">It is the control and configuration file for the script that runs the storage snapshots.</span></span> <span data-ttu-id="901c6-274">Byste měli obdržet _název zálohy úložiště_ a _úložiště IP adresu_ ze SAP HANA na Azure Service Management při nasazení vaší instance.</span><span class="sxs-lookup"><span data-stu-id="901c6-274">You should have received the _Storage Backup Name_ and _Storage IP Address_ from SAP HANA on Azure Service Management when your instances were deployed.</span></span> <span data-ttu-id="901c6-275">Pořadí, nelze upravit, řazení, nebo mezer všech proměnných, nebo skript nejde správně spustit.</span><span class="sxs-lookup"><span data-stu-id="901c6-275">You cannot modify the sequence, ordering, or spacing of any of the variables, or the script does not run properly.</span></span>

<span data-ttu-id="901c6-276">Pro škálování nasazení bude vypadat konfiguračního souboru:</span><span class="sxs-lookup"><span data-stu-id="901c6-276">For a scale-up deployment, the configuration file would look like:</span></span>
```
#Provided by Microsoft Service Management
Storage Backup Name: lhanad01backup
Storage IP Address: 10.250.20.21
#Created by customer using hdbuserstore
HANA Backup Name: SCADMIND01
```
<span data-ttu-id="901c6-277">Pro konfiguraci škálovaného na více systémů bude vypadat HANABackupCustomerDetailsBW.txt souboru:</span><span class="sxs-lookup"><span data-stu-id="901c6-277">For a scale-out configuration, the HANABackupCustomerDetailsBW.txt file would look like:</span></span>
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
><span data-ttu-id="901c6-278">V současné době pouze podrobnosti o 1 uzlu se používají ve skutečné skriptu HANA úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="901c6-278">Currently, only Node 1 details are used in the actual HANA storage snapshot script.</span></span> <span data-ttu-id="901c6-279">Doporučujeme, abyste otestovali přístup do nebo ze všech uzlů HANA tak, že pokud se hlavní uzel zálohování někdy změní, již zajistíte, že jiného uzlu může trvat jeho místo změnou podrobnosti v uzlu 1.</span><span class="sxs-lookup"><span data-stu-id="901c6-279">We recommend that you test access to or from all HANA nodes so that, if the master backup node ever changes, you have already ensured that any other node can take its place by modifying the details in Node 1.</span></span>

<span data-ttu-id="901c6-280">Zkontrolujte správné konfigurace v konfiguračním souboru nebo správné připojení k instancím HANA, spusťte některý z následujících skriptů:</span><span class="sxs-lookup"><span data-stu-id="901c6-280">To check for the correct configurations in the configuration file or proper connectivity to the HANA instances, run either of the following scripts:</span></span>
- <span data-ttu-id="901c6-281">Pro škálování konfigurace (nezávislou na pracovním vytížení SAP):</span><span class="sxs-lookup"><span data-stu-id="901c6-281">For a scale-up configuration (independent on SAP workload):</span></span>

 ```
testHANAConnection.pl
```
- <span data-ttu-id="901c6-282">Pro škálovatelnou konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="901c6-282">For a scale-out configuration:</span></span>

 ```
testHANAConnectionBW.pl
```

<span data-ttu-id="901c6-283">Zajistěte, aby hlavní instance HANA přístup na všechny požadované servery HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-283">Ensure that the master HANA instance has access to all required HANA servers.</span></span> <span data-ttu-id="901c6-284">Neexistují žádné parametry skriptu, ale je třeba provést odpovídající HANABackupCustomerDetails / HANABackupCustomerDetailsBW souborů pro skript, který chcete spustit správně.</span><span class="sxs-lookup"><span data-stu-id="901c6-284">There are no parameters to the script, but you must complete the appropriate HANABackupCustomerDetails/ HANABackupCustomerDetailsBW file for the script to run properly.</span></span> <span data-ttu-id="901c6-285">Vzhledem k tomu, že se vrátí jenom kódy chyb prostředí příkaz, není možné skript tak, aby všechny instance Kontrola chyb.</span><span class="sxs-lookup"><span data-stu-id="901c6-285">Because only the shell command error codes are returned, it is not possible for the script to error-check every instance.</span></span> <span data-ttu-id="901c6-286">I tak skript zadejte některé užitečné komentáře můžete znovu zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="901c6-286">Even so, the script does provide some helpful comments for you to double-check.</span></span>

<span data-ttu-id="901c6-287">Pro spuštění skriptu:</span><span class="sxs-lookup"><span data-stu-id="901c6-287">To run the script:</span></span>
```
 ./testHANAConnection.pl
```
 <span data-ttu-id="901c6-288">Pokud skript úspěšně získá stav instance HANA, zobrazí se zpráva, že HANA připojení bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="901c6-288">If the script successfully obtains the status of the HANA instance, it displays a message that the HANA connection was successful.</span></span>

<span data-ttu-id="901c6-289">Kromě toho je druhý typ skriptu, které můžete použít ke kontrole hlavní HANA instance serveru možnost přihlásit se do úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-289">Additionally, there is a second type of script you can use to check the master HANA instance server's ability to sign in to the storage.</span></span> <span data-ttu-id="901c6-290">Před spuštěním azure\_hana\_zálohování (\_bw) PL skriptu, je třeba spustit další skript.</span><span class="sxs-lookup"><span data-stu-id="901c6-290">Before you execute the azure\_hana\_backup(\_bw).pl script, you must execute the next script.</span></span> <span data-ttu-id="901c6-291">Pokud svazek obsahuje žádné snímky, je možné určit, zda je svazek jednoduše prázdný nebo není ssh získání podrobných informací o snímku se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="901c6-291">If a volume contains no snapshots, it is impossible to determine whether the volume is simply empty or there is an ssh failure to obtain the snapshot details.</span></span> <span data-ttu-id="901c6-292">Z tohoto důvodu se skript spustí dva kroky:</span><span class="sxs-lookup"><span data-stu-id="901c6-292">For this reason, the script executes two steps:</span></span>

- <span data-ttu-id="901c6-293">Ověří, že konzole úložiště je dostupný.</span><span class="sxs-lookup"><span data-stu-id="901c6-293">It verifies that the storage console is accessible.</span></span>
- <span data-ttu-id="901c6-294">Vytvoří snímek testu nebo fiktivní, pro každý svazek HANA instance.</span><span class="sxs-lookup"><span data-stu-id="901c6-294">It creates a test, or dummy, snapshot for each volume by HANA instance.</span></span>

<span data-ttu-id="901c6-295">Z tohoto důvodu je zahrnuta jako argument HANA instance.</span><span class="sxs-lookup"><span data-stu-id="901c6-295">For this reason, the HANA instance is included as an argument.</span></span> <span data-ttu-id="901c6-296">Znovu není možné zajistit kontrolu chyb pro připojení k úložišti, ale skript poskytuje užitečné pomocné parametry, pokud se nezdaří spuštění.</span><span class="sxs-lookup"><span data-stu-id="901c6-296">Again, it is not possible to provide error checking for the storage connection, but the script provides helpful hints if the execution fails.</span></span>

<span data-ttu-id="901c6-297">Spuštění skriptu jako:</span><span class="sxs-lookup"><span data-stu-id="901c6-297">The script is run as:</span></span>
```
 ./testStorageSnapshotConnection.pl <hana instance>
```
<span data-ttu-id="901c6-298">Nebo ji spustit jako:</span><span class="sxs-lookup"><span data-stu-id="901c6-298">Or it is run as:</span></span>
```
./testStorageSnapshotConnectionBW.pl <hana instance>
```
<span data-ttu-id="901c6-299">Skript také zobrazí zprávu, že budete moci správně přihlásit ke klientovi nasazené úložiště, který je založen na logickou jednotku (LUN), které používá instance serveru, které vlastníte.</span><span class="sxs-lookup"><span data-stu-id="901c6-299">The script also displays a message that you are able to sign in appropriately to your deployed storage tenant, which is organized around the logical unit numbers (LUNs) that are used by the server instances you own.</span></span>

<span data-ttu-id="901c6-300">Před spuštěním první zálohy založené na snímku úložiště, spusťte další skripty a ujistěte se, že konfigurace je správná.</span><span class="sxs-lookup"><span data-stu-id="901c6-300">Before you execute the first storage snapshot-based backups, run the next scripts to make sure that the configuration is correct.</span></span>

<span data-ttu-id="901c6-301">Po spuštění těchto skriptů, můžete spuštěním odstranit snímky:</span><span class="sxs-lookup"><span data-stu-id="901c6-301">After running these scripts, you can delete the snapshots by executing:</span></span>
```
./removeTestStorageSnapshot.pl <hana instance>
```
<span data-ttu-id="901c6-302">Nebo</span><span class="sxs-lookup"><span data-stu-id="901c6-302">Or</span></span>
```
./removeTestStorageSnapshot.pl <hana instance>
```

### <a name="step-7-perform-on-demand-snapshots"></a><span data-ttu-id="901c6-303">Krok 7: Provedení snímků na vyžádání</span><span class="sxs-lookup"><span data-stu-id="901c6-303">Step 7: Perform on-demand snapshots</span></span>

<span data-ttu-id="901c6-304">Provádět na vyžádání snímků (a také naplánovat pravidelné snímky pomocí procesu cron) podle postupu popsaného tady.</span><span class="sxs-lookup"><span data-stu-id="901c6-304">Perform on-demand snapshots (as well as schedule regular snapshots by using cron) as described here.</span></span>

<span data-ttu-id="901c6-305">Pro škálování konfigurace spusťte následující skript:</span><span class="sxs-lookup"><span data-stu-id="901c6-305">For scale-up configurations, execute the following script:</span></span>
```
./azure_hana_backup.pl lhanad01 customer 20
```
<span data-ttu-id="901c6-306">Konfigurace Škálováním na více systémů spusťte následující skript:</span><span class="sxs-lookup"><span data-stu-id="901c6-306">For scale-out configurations, execute the following script:</span></span>
```
./azure_hana_backup_bw.pl lhanad01 customer 20
```
<span data-ttu-id="901c6-307">Skript Škálováním na více systémů nepodporuje některé další kontrolu a ujistěte se, že všechny servery HANA je přístupná, a všechny instance HANA vrátí příslušný stav instance před pokračováním vytváření snímků SAP HANA nebo úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-307">The scale-out script does some additional checking to make sure that all HANA servers can be accessed, and all HANA instances return appropriate status of the instance before proceeding with creating SAP HANA or storage snapshots.</span></span>

<span data-ttu-id="901c6-308">Následující argumenty jsou povinné:</span><span class="sxs-lookup"><span data-stu-id="901c6-308">The following arguments are required:</span></span>

- <span data-ttu-id="901c6-309">Instance HANA vyžadují zálohování.</span><span class="sxs-lookup"><span data-stu-id="901c6-309">The HANA instance requiring backup.</span></span>
- <span data-ttu-id="901c6-310">Předpona snímku pro úložiště snímku.</span><span class="sxs-lookup"><span data-stu-id="901c6-310">The snapshot prefix for the storage snapshot.</span></span>
- <span data-ttu-id="901c6-311">Počet snímků, která se mají uchovat pro konkrétní předponu.</span><span class="sxs-lookup"><span data-stu-id="901c6-311">The number of snapshots to be kept for the specific prefix.</span></span>

```
./azure_hana_backup.pl lhanad01 customer 20
```

<span data-ttu-id="901c6-312">Provádění skriptu vytvoří snímek úložiště v těchto tří různých fází:</span><span class="sxs-lookup"><span data-stu-id="901c6-312">The execution of the script creates the storage snapshot in these three distinct phases:</span></span>

- <span data-ttu-id="901c6-313">Spusťte HANA snímku.</span><span class="sxs-lookup"><span data-stu-id="901c6-313">Execute a HANA snapshot.</span></span>
- <span data-ttu-id="901c6-314">Spusťte snímku úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-314">Execute a storage snapshot.</span></span>
- <span data-ttu-id="901c6-315">Odeberte HANA snímku.</span><span class="sxs-lookup"><span data-stu-id="901c6-315">Remove the HANA snapshot.</span></span>

<span data-ttu-id="901c6-316">Spusťte skript voláním z HDB složku spustitelného souboru, který jste zkopírovali do.</span><span class="sxs-lookup"><span data-stu-id="901c6-316">Execute the script by calling it from the HDB executable folder that it was copied to.</span></span> <span data-ttu-id="901c6-317">Zálohuje alespoň následujících svazků, ale také zálohuje jakýkoli svazek, který má explicitní název instance SAP HANA v název svazku.</span><span class="sxs-lookup"><span data-stu-id="901c6-317">It backs up at least the following volumes, but it also backs up any volume that has the explicit SAP HANA instance name in the volume name.</span></span>
```
hana_data_<hana instance>_prod_t020_vol
hana_log_<hana instance>_prod_t020_vol
hana_log_backup_<hana instance>_prod_t020_vol
hana_shared_<hana instance>_prod_t020_vol
```
<span data-ttu-id="901c6-318">Doba uchování je výhradně spravovat, s počet snímků odeslána jako parametr při spuštění skriptu (například 20 uvedený výše).</span><span class="sxs-lookup"><span data-stu-id="901c6-318">The retention period is strictly administered, with the number of snapshots submitted as a parameter when you execute the script (such as 20, shown previously).</span></span> <span data-ttu-id="901c6-319">Množství času, proto je funkce, doba provádění a počet snímků ve volání skriptu.</span><span class="sxs-lookup"><span data-stu-id="901c6-319">So the amount of time is a function of the period of execution and the number of snapshots in the call of the script.</span></span> <span data-ttu-id="901c6-320">Pokud počet snímků, které jsou zachovány překračuje počet, který je pojmenován jako parametr ve volání skriptu, nejstarší úložiště snímku tento popisek (v našem případě předchozí _vlastní_) je odstraněn před provedením nový snímek.</span><span class="sxs-lookup"><span data-stu-id="901c6-320">If the number of snapshots that are kept exceeds the number that are named as a parameter in the call of the script, the oldest storage snapshot of this label (in our previous case, _custom_) is deleted before a new snapshot is executed.</span></span> <span data-ttu-id="901c6-321">To znamená počet poskytnout jako poslední parametr volání je číslo můžete použít k řízení počet snímků.</span><span class="sxs-lookup"><span data-stu-id="901c6-321">This means the number you give as the last parameter of the call is the number you can use to control the number of snapshots.</span></span>

>[!NOTE]
><span data-ttu-id="901c6-322">Jakmile změníte štítek, počítání znovu spustí.</span><span class="sxs-lookup"><span data-stu-id="901c6-322">As soon as you change the label, the counting starts again.</span></span>

<span data-ttu-id="901c6-323">Je nutné zahrnout název instance HANA, které poskytuje SAP HANA na Azure Service Management jako argument jejich vytváření snímků v prostředích s více uzly.</span><span class="sxs-lookup"><span data-stu-id="901c6-323">You need to include the HANA instance name that's provided by SAP HANA on Azure Service Management as an argument if they are creating snapshots in multi-node environments.</span></span> <span data-ttu-id="901c6-324">V prostředích s jedním uzlem název SAP HANA na jednotce Azure (velké instance) je dostačující, ale přesto se doporučuje název instance HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-324">In single-node environments, the name of the SAP HANA on Azure (large instances) unit is sufficient, but the HANA instance name is still recommended.</span></span>

<span data-ttu-id="901c6-325">Kromě toho můžete spouštěcí volumes\LUNs pomocí stejného skriptu zálohovat.</span><span class="sxs-lookup"><span data-stu-id="901c6-325">Additionally, you can back up boot volumes\LUNs by using the same script.</span></span> <span data-ttu-id="901c6-326">Je nutné zálohovat spouštěcí svazek alespoň jednou při prvním spuštění HANA, i když vám doporučujeme týdenní nebo noční plán zálohování pro spouštění v procesu cron.</span><span class="sxs-lookup"><span data-stu-id="901c6-326">You must back up your boot volume at least once when you first run HANA, although we recommend a weekly or nightly backup schedule for booting in cron.</span></span> <span data-ttu-id="901c6-327">Spíše než přidat název instance SAP HANA, vložte _spouštěcí_ jako argument do skriptu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="901c6-327">Rather than add an SAP HANA instance name, insert _boot_ as the argument into the script as follows:</span></span>
```
./azure_hana_backup boot customer 20
```
<span data-ttu-id="901c6-328">Stejné zásady uchovávání informací je zaměřena na spouštěcí svazek.</span><span class="sxs-lookup"><span data-stu-id="901c6-328">The same retention policy is afforded to the boot volume as well.</span></span> <span data-ttu-id="901c6-329">Použijte na vyžádání snímky, jak je popsáno dříve, pro zvláštní případy, například během upgradu SAP vylepšení balíčku (EHP), nebo pokud budete potřebovat pro vytvoření snímku odlišné úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-329">Use on-demand snapshots, as described previously, for special cases only, such as during an SAP enhancement package (EHP) upgrade, or when you need to create a distinct storage snapshot.</span></span>

<span data-ttu-id="901c6-330">Doporučujeme provést plánované úložiště snímků pomocí procesu cron a doporučujeme použít stejný skriptu pro všechny zálohy a zotavení po havárii potřebám (úprava vstupy skript tak, aby odpovídaly různých požadovaný čas zálohování).</span><span class="sxs-lookup"><span data-stu-id="901c6-330">We encourage you to perform scheduled storage snapshots using cron, and we recommend that you use the same script for all backups and disaster-recovery needs (modifying the script inputs to match the various requested backup times).</span></span> <span data-ttu-id="901c6-331">Tyto jsou všechny naplánované jinak v procesu cron v závislosti na jejich čas spuštění: hodinové, 12 hodin, denní nebo týdenní.</span><span class="sxs-lookup"><span data-stu-id="901c6-331">These are all scheduled differently in cron depending on their execution time: hourly, 12-hour, daily, or weekly.</span></span> <span data-ttu-id="901c6-332">Plán cron slouží k vytvoření snímků úložiště, které odpovídají dříve popsaných uchování štítky pro dlouhodobé odlehlého zálohování.</span><span class="sxs-lookup"><span data-stu-id="901c6-332">The cron schedule is designed to create storage snapshots that match the previously discussed retention labeling for long-term off-site backup.</span></span> <span data-ttu-id="901c6-333">Skript obsahuje příkazy zálohovat všechny svazky produkčního, v závislosti na jejich požadovanou četnost (data a soubory protokolu jsou zálohovány každou hodinu, zatímco spouštěcí svazek je zálohovat denně).</span><span class="sxs-lookup"><span data-stu-id="901c6-333">The script includes commands to back up all production volumes, depending on their requested frequency (data and log files are backed up hourly, whereas the boot volume is backed up daily).</span></span>

<span data-ttu-id="901c6-334">Položky v následující skript cron spouštět každou hodinu v desetinu minutu každých 12 hodin na desetinu minutu a každý den v desetinu minutu.</span><span class="sxs-lookup"><span data-stu-id="901c6-334">The entries in the following cron script run every hour at the tenth minute, every 12 hours at the tenth minute, and daily at the tenth minute.</span></span> <span data-ttu-id="901c6-335">Cron, které úlohy se vytvářejí tak, že pouze jeden úložiště snímků SAP HANA probíhá během konkrétní hodina, tak, aby hodinové a denní zálohy se nevyskytují ve stejnou dobu (12:10:00).</span><span class="sxs-lookup"><span data-stu-id="901c6-335">The cron jobs are created in such a way that only one SAP HANA storage snapshot takes place during any particular hour, so that the hourly and daily backups do not occur at the same time (12:10 AM).</span></span> <span data-ttu-id="901c6-336">Chcete-li optimalizovat vytváření snímků a replikace, SAP HANA na Azure Service Management poskytuje doporučené čas pro spuštění zálohování.</span><span class="sxs-lookup"><span data-stu-id="901c6-336">To help optimize your snapshot creation and replication, SAP HANA on Azure Service Management provides the recommended time for you to run your backups.</span></span>

<span data-ttu-id="901c6-337">Cron výchozí plánování v /etc/crontab vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="901c6-337">The default cron scheduling in /etc/crontab is as follows:</span></span>
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 14
```
<span data-ttu-id="901c6-338">V předchozích pokynů cron získat HANA svazky (bez spouštěcí svazek) každou hodinu snímku s popiskem.</span><span class="sxs-lookup"><span data-stu-id="901c6-338">In the previous cron instructions, the HANA volumes (without boot volume) get an hourly snapshot with the label.</span></span> <span data-ttu-id="901c6-339">Tyto snímků 66 zachovány.</span><span class="sxs-lookup"><span data-stu-id="901c6-339">Of these snapshots, 66 are retained.</span></span> <span data-ttu-id="901c6-340">Kromě toho jsou uchovány 14 snímky s popiskem 12 hodin.</span><span class="sxs-lookup"><span data-stu-id="901c6-340">Additionally, 14 snapshots with the 12-hour label are retained.</span></span> <span data-ttu-id="901c6-341">Potenciálně získat HODINOVĚ snímky pro tři dny a snímky 12 hodin pro jiné čtyř dní, které vám celý týden snímků.</span><span class="sxs-lookup"><span data-stu-id="901c6-341">You potentially get hourly snapshots for three days, plus 12-hour snapshots for another four days, which gives you an entire week of snapshots.</span></span>

<span data-ttu-id="901c6-342">Plánování v rámci procesu cron může být složité, protože pouze jeden skript má být spuštěn v jakémkoli čase, pokud skripty rozkládají o několik minut.</span><span class="sxs-lookup"><span data-stu-id="901c6-342">Scheduling within cron can be tricky, because only one script should be executed at any particular time, unless the scripts are staggered by several minutes.</span></span> <span data-ttu-id="901c6-343">Pokud chcete denní zálohy pro dlouhodobé uchovávání, denní snímek je uložen spolu s snímku 12 hodin (s uchování počtem 7 každý) nebo hodinové snímku rozložena proběhla 10 minut později.</span><span class="sxs-lookup"><span data-stu-id="901c6-343">If you want daily backups for long-term retention, either a daily snapshot is kept along with a 12-hour snapshot (with a retention count of seven each), or the hourly snapshot is staggered to take place 10 minutes later.</span></span> <span data-ttu-id="901c6-344">Pouze jeden denní snímek je udržováno na produkční svazku.</span><span class="sxs-lookup"><span data-stu-id="901c6-344">Only one daily snapshot is kept in the production volume.</span></span>
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 7
10 0 * * * ./azure_hana_backup.pl lhanad01 daily 7
```
<span data-ttu-id="901c6-345">Frekvence tady jsou jenom příklady.</span><span class="sxs-lookup"><span data-stu-id="901c6-345">The frequencies listed here are only examples.</span></span> <span data-ttu-id="901c6-346">Odvození vaší optimální počet snímků, použijte následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="901c6-346">To derive your optimum number of snapshots, use the following criteria:</span></span>

- <span data-ttu-id="901c6-347">Požadavky v cíli času obnovení pro obnovení bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="901c6-347">Requirements in recovery time objective for point-in-time recovery.</span></span>
- <span data-ttu-id="901c6-348">Využití místa.</span><span class="sxs-lookup"><span data-stu-id="901c6-348">Space usage.</span></span>
- <span data-ttu-id="901c6-349">Požadavky v rámci obnovování bodu cíl a cíli času obnovení pro zotavení po případné havárii.</span><span class="sxs-lookup"><span data-stu-id="901c6-349">Requirements in recovery point objective and recovery time objective for potential disaster recovery.</span></span>
- <span data-ttu-id="901c6-350">Závěrečné provádění záloh úplné databáze HANA proti disky.</span><span class="sxs-lookup"><span data-stu-id="901c6-350">Eventual execution of HANA full database backups against disks.</span></span> <span data-ttu-id="901c6-351">Vždy, když úplnou zálohu databáze na disky, nebo _backint_ rozhraní, se provádí, se nezdaří spuštění úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="901c6-351">Whenever a full database backup against disks, or _backint_ interface, is performed, the execution of storage snapshots fails.</span></span> <span data-ttu-id="901c6-352">Pokud máte v plánu provést zálohování úplné databáze nad úložiště snímků, ujistěte se, že během této doby je zakázána provádění úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="901c6-352">If you plan to execute full database backups on top of storage snapshots, make sure that the execution of storage snapshots is disabled during this time.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="901c6-353">Používání úložiště snímků pro SAP HANA zálohy je platný jenom v případě, že snímky jsou prováděny ve spojení s zálohy protokolu SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-353">The use of storage snapshots for SAP HANA backups is valid only when the snapshots are performed in conjunction with SAP HANA log backups.</span></span> <span data-ttu-id="901c6-354">Tyto zálohy protokolu musí být schopný tak, aby pokrývalo časových období mezi snímky úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-354">These log backups need to be able to cover the time periods between the storage snapshots.</span></span> <span data-ttu-id="901c6-355">Pokud jste nastavili závazek uživatelům v okamžiku obnovení 30 dní, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="901c6-355">If you've set a commitment to users of a point-in-time recovery of 30 days, you need the following:</span></span>

- <span data-ttu-id="901c6-356">Možnost pro přístup k snímek úložiště, který je 30 dní.</span><span class="sxs-lookup"><span data-stu-id="901c6-356">Ability to access a storage snapshot that is 30 days old.</span></span>
- <span data-ttu-id="901c6-357">Souvislý protokolu zálohování za posledních 30 dní.</span><span class="sxs-lookup"><span data-stu-id="901c6-357">Contiguous log backups over the last 30 days.</span></span>

<span data-ttu-id="901c6-358">V oblasti zálohy protokolů se vytvoření snímku objemu protokol o zálohování.</span><span class="sxs-lookup"><span data-stu-id="901c6-358">In the range of log backups, create a snapshot of the backup log volume as well.</span></span> <span data-ttu-id="901c6-359">Nezapomeňte však provést zálohování protokolu regulární, aby bylo možné:</span><span class="sxs-lookup"><span data-stu-id="901c6-359">However, be sure to perform regular log backups so that you can:</span></span>

- <span data-ttu-id="901c6-360">Mít zálohy souvislý protokolů, které jsou potřebné k obnovení bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="901c6-360">Have the contiguous log backups needed to perform point-in-time recoveries.</span></span>
- <span data-ttu-id="901c6-361">Zabránit spuštění nedostatek místa na svazku protokolu SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-361">Prevent the SAP HANA log volume from running out of space.</span></span>

<span data-ttu-id="901c6-362">Jeden z poslední kroků je naplánovat zálohování protokolů SAP HANA v SAP HANA Studio.</span><span class="sxs-lookup"><span data-stu-id="901c6-362">One of the last steps is to schedule SAP HANA backup logs in SAP HANA Studio.</span></span> <span data-ttu-id="901c6-363">SAP HANA protokol o zálohování cílového místa je speciálně vytvořený hana nebo protokolu\_zálohování svazku s přípojný bod /hana/log/backups.</span><span class="sxs-lookup"><span data-stu-id="901c6-363">The SAP HANA backup log target destination is the specially created hana/log\_backups volume with the mount point of /hana/log/backups.</span></span>

![Plán zálohování SAP HANA přihlásí SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

<span data-ttu-id="901c6-365">Můžete vybrat zálohování, které jsou častější, než každých 15 minut.</span><span class="sxs-lookup"><span data-stu-id="901c6-365">You can choose backups that are more frequent than every 15 minutes.</span></span> <span data-ttu-id="901c6-366">Někteří uživatelé i provést zálohování protokolu každou minutu, i když není doporučeno probíhající _přes_ 15 minut.</span><span class="sxs-lookup"><span data-stu-id="901c6-366">Some users even perform log backups every minute, although we do not recommend going _over_ 15 minutes.</span></span>

<span data-ttu-id="901c6-367">Posledním krokem je vytvoření jedné zálohování položku, která musí existovat v rámci katalogu zálohování provést zálohování na základě souborů (po počáteční instalaci SAP HANA).</span><span class="sxs-lookup"><span data-stu-id="901c6-367">The final step is to perform a file-based backup (after the initial installation of SAP HANA) to create a single backup entry that must exist within the backup catalog.</span></span> <span data-ttu-id="901c6-368">V opačném případě SAP HANA nelze zahájit zadaného protokolu zálohování.</span><span class="sxs-lookup"><span data-stu-id="901c6-368">Otherwise SAP HANA cannot initiate your specified log backups.</span></span>

![Vytvořit zálohu na základě souborů k vytvoření jedné položky zálohování](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)

### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a><span data-ttu-id="901c6-370">Monitorování počtu a velikosti snímků na diskovém svazku</span><span class="sxs-lookup"><span data-stu-id="901c6-370">Monitoring the number and size of snapshots on the disk volume</span></span>

<span data-ttu-id="901c6-371">Na svazku konkrétní úložiště můžete sledovat počet snímků a spotřebu úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="901c6-371">On a particular storage volume, you can monitor the number of snapshots and the storage consumption of snapshots.</span></span> <span data-ttu-id="901c6-372">`ls` Příkaz nezobrazí snímek adresáře nebo soubory.</span><span class="sxs-lookup"><span data-stu-id="901c6-372">The `ls` command doesn't show the snapshot directory or files.</span></span> <span data-ttu-id="901c6-373">Ale příkaz operační systém Linux `du` nemá pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="901c6-373">However, the Linux OS command `du` does, with the following commands:</span></span>

- <span data-ttu-id="901c6-374">`du –sh .snapshot`poskytuje celkem všechny snímky v adresáři snímku.</span><span class="sxs-lookup"><span data-stu-id="901c6-374">`du –sh .snapshot` provides a total of all snapshots within the snapshot directory.</span></span>
- <span data-ttu-id="901c6-375">`du –sh --max-depth=1`Zobrazí všechny snímky, které jsou uloženy ve složce .snapshot a velikosti jednotlivých snímků.</span><span class="sxs-lookup"><span data-stu-id="901c6-375">`du –sh --max-depth=1` lists all snapshots that are saved in the .snapshot folder and the size of each snapshot.</span></span>
- <span data-ttu-id="901c6-376">`du –hc`poskytuje celkovou velikost používané všechny snímky.</span><span class="sxs-lookup"><span data-stu-id="901c6-376">`du –hc` provides the total size used by all snapshots.</span></span>

<span data-ttu-id="901c6-377">Pomocí těchto příkazů a ujistěte se, že snímky, které jsou provedeny a uloženy nejsou využívá všechny úložiště na svazcích.</span><span class="sxs-lookup"><span data-stu-id="901c6-377">Use these commands to make sure that the snapshots that are taken and stored are not consuming all the storage on the volumes.</span></span>

### <a name="reducing-the-number-of-snapshots-on-a-server"></a><span data-ttu-id="901c6-378">Omezení počtu snímků na serveru</span><span class="sxs-lookup"><span data-stu-id="901c6-378">Reducing the number of snapshots on a server</span></span>

<span data-ttu-id="901c6-379">Jak je popsáno výše, můžete snížit počet určité popisky snímků, které jsou uloženy.</span><span class="sxs-lookup"><span data-stu-id="901c6-379">As explained earlier, you can reduce the number of certain labels of snapshots that you store.</span></span> <span data-ttu-id="901c6-380">Poslední dva parametry příkazu zahájíte snímek jsou štítky a počet snímků, které chcete zachovat.</span><span class="sxs-lookup"><span data-stu-id="901c6-380">The last two parameters of the command to initiate a snapshot are a label and the number of snapshots you want to retain.</span></span>
```
./azure_hana_backup.pl lhanad01 customer 20
```
<span data-ttu-id="901c6-381">V předchozím příkladu popisek snímku je _zákazníka_ a počet snímků s tímto štítkem pro zachování _20_.</span><span class="sxs-lookup"><span data-stu-id="901c6-381">In the previous example, the snapshot label is _customer_ and the number of snapshots with this label to be retained is _20_.</span></span> <span data-ttu-id="901c6-382">Jak můžete reagovat na využívání místa na disku, můžete snížit počet uložené snímky.</span><span class="sxs-lookup"><span data-stu-id="901c6-382">As you respond to disk space consumption, you might want to reduce the number of stored snapshots.</span></span> <span data-ttu-id="901c6-383">Snadný způsob, jak snížit počet snímků, je pro spuštění skriptu s parametrem poslední nastavena na 5:</span><span class="sxs-lookup"><span data-stu-id="901c6-383">The easy way to reduce the number of snapshots is to run the script with the last parameter set to 5:</span></span>
```
./azure_hana_backup.pl lhanad01 customer 5
```
<span data-ttu-id="901c6-384">V důsledku spuštění skriptu se toto nastavení, počet snímků, včetně nový snímek úložiště je _5_.</span><span class="sxs-lookup"><span data-stu-id="901c6-384">As a result of running the script with this setting, the number of snapshots, including the new storage snapshot, is _5_.</span></span>

 >[!NOTE]
 > <span data-ttu-id="901c6-385">Tento skript snižuje počet snímků, pouze pokud je více než jednu hodinu stará nejnovější předchozí snímek.</span><span class="sxs-lookup"><span data-stu-id="901c6-385">This script reduces the number of snapshots only if the most recent previous snapshot is more than one hour old.</span></span> <span data-ttu-id="901c6-386">Skript nedojde k odstranění snímků, které jsou menší než hodinu stará.</span><span class="sxs-lookup"><span data-stu-id="901c6-386">The script does not delete snapshots that are less than one hour old.</span></span>

<span data-ttu-id="901c6-387">Tato omezení se vztahují k zotavení po havárii volitelné funkce nabízené.</span><span class="sxs-lookup"><span data-stu-id="901c6-387">These restrictions are related to the optional disaster-recovery functionality offered.</span></span>

<span data-ttu-id="901c6-388">Pokud již nechcete udržovat sadu snímky s tuto předponu, můžete spustit skript s _0_ jako číslo uchování odebrat všechny snímky odpovídající tuto předponu.</span><span class="sxs-lookup"><span data-stu-id="901c6-388">If you no longer want to maintain a set of snapshots with that prefix, you can execute the script with _0_ as the retention number to remove all snapshots matching that prefix.</span></span> <span data-ttu-id="901c6-389">Odebrání všechny snímky však může ovlivnit možnosti zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="901c6-389">However, removing all snapshots can affect the capabilities of disaster recovery.</span></span>

### <a name="recovering-to-the-most-recent-hana-snapshot"></a><span data-ttu-id="901c6-390">Obnovení na poslední snímek HANA</span><span class="sxs-lookup"><span data-stu-id="901c6-390">Recovering to the most recent HANA snapshot</span></span>

<span data-ttu-id="901c6-391">V případě, že dojde rozbalovací produkční scénář, procesem obnovení ze snímku úložiště lze inicializovat jako zákazník incidentu s SAP HANA na Azure Service Management.</span><span class="sxs-lookup"><span data-stu-id="901c6-391">In the event that you experience a production-down scenario, the process of recovering from a storage snapshot can be initiated as a customer incident with SAP HANA on Azure Service Management.</span></span> <span data-ttu-id="901c6-392">Neočekávané scénář může být vysokou naléhavost věci, pokud byla data odstraněna v produkční systému a je jediný způsob, jak načíst data obnovit produkční databázi.</span><span class="sxs-lookup"><span data-stu-id="901c6-392">Such an unexpected scenario might be a high-urgency matter if data was deleted in a production system and the only way to retrieve the data is to restore the production database.</span></span>

<span data-ttu-id="901c6-393">Obnovení bodu v čase na druhé straně může být nízkou naléhavost a plánované předem dní.</span><span class="sxs-lookup"><span data-stu-id="901c6-393">On the other hand, a point-in-time recovery could be low urgency and planned for days in advance.</span></span> <span data-ttu-id="901c6-394">Toto obnovení s SAP HANA můžete naplánovat na Azure Service Management místo vyvolání problém s vysokou prioritou.</span><span class="sxs-lookup"><span data-stu-id="901c6-394">You can plan this recovery with SAP HANA on Azure Service Management instead of raising a high-priority issue.</span></span> <span data-ttu-id="901c6-395">Například je může být plánování Zkuste upgrade softwaru SAP použitím nový balíček vylepšení a pak je nutné vrátit zpět na snímek, který představuje stav před upgradem EHP.</span><span class="sxs-lookup"><span data-stu-id="901c6-395">For example, you might be planning to try an upgrade of the SAP software by applying a new enhancement package, and you then need to revert back to a snapshot that represents the state before the EHP upgrade.</span></span>

<span data-ttu-id="901c6-396">Před vydáním žádost, budete muset provést určitou přípravu.</span><span class="sxs-lookup"><span data-stu-id="901c6-396">Before you issue the request, you need to do some preparation.</span></span> <span data-ttu-id="901c6-397">SAP HANA týmu Azure Service Management můžete žádost zpracovat a poskytují obnovené svazky.</span><span class="sxs-lookup"><span data-stu-id="901c6-397">SAP HANA on Azure Service Management team can then handle the request and provide the restored volumes.</span></span> <span data-ttu-id="901c6-398">Potom můžete se rozhodnout obnovit databázi HANA podle snímky.</span><span class="sxs-lookup"><span data-stu-id="901c6-398">Afterward, it is up to you to restore the HANA database based on the snapshots.</span></span> <span data-ttu-id="901c6-399">Tady je postup přípravy pro žádost:</span><span class="sxs-lookup"><span data-stu-id="901c6-399">Here is how to prepare for the request:</span></span>

>[!NOTE]
><span data-ttu-id="901c6-400">Uživatelské rozhraní se můžou lišit od následující snímky obrazovky, v závislosti na verzi SAP HANA, který používáte.</span><span class="sxs-lookup"><span data-stu-id="901c6-400">Your user interface might vary from the following screenshots, depending on the SAP HANA release you are using.</span></span>

1. <span data-ttu-id="901c6-401">Rozhodněte, které snímek pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="901c6-401">Decide which snapshot to restore.</span></span> <span data-ttu-id="901c6-402">Pokud jste se nedostanete budou obnoveny pouze hana nebo datový svazek.</span><span class="sxs-lookup"><span data-stu-id="901c6-402">Only the hana/data volume would be restored unless you are instructed otherwise.</span></span>

2. <span data-ttu-id="901c6-403">Vypněte instanci HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-403">Shut down the HANA instance.</span></span>

 ![Vypnout instanci HANA](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. <span data-ttu-id="901c6-405">Odpojte objemy dat v každém uzlu HANA databáze.</span><span class="sxs-lookup"><span data-stu-id="901c6-405">Unmount the data volumes on each HANA database node.</span></span> <span data-ttu-id="901c6-406">Obnovení snímku se nezdaří, pokud nejsou datové svazky.</span><span class="sxs-lookup"><span data-stu-id="901c6-406">The restoration of the snapshot fails if the data volumes are not unmounted.</span></span>

 ![Odpojte svazky dat v každém uzlu databáze HANA](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. <span data-ttu-id="901c6-408">Otevřete žádost o podporu Azure dáte pokyn, aby obnovení snímku konkrétní.</span><span class="sxs-lookup"><span data-stu-id="901c6-408">Open an Azure support request to instruct the restoration of a specific snapshot.</span></span>

 - <span data-ttu-id="901c6-409">Během obnovení: SAP HANA na Azure Service Management může vás k účasti na konferenci zajistit, že je ztratili žádná data.</span><span class="sxs-lookup"><span data-stu-id="901c6-409">During the restoration: SAP HANA on Azure Service Management might ask you to attend a conference call to ensure that no data is getting lost.</span></span>

 - <span data-ttu-id="901c6-410">Po obnovení: SAP HANA na Azure Service Management vás upozorní, jakmile byla obnovena snímku úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-410">After the restoration: SAP HANA on Azure Service Management notifies you when the storage snapshot has been restored.</span></span>

5. <span data-ttu-id="901c6-411">Po dokončení procesu obnovení je znovu připojte všechny datové svazky.</span><span class="sxs-lookup"><span data-stu-id="901c6-411">After the restoration process is complete, remount all data volumes.</span></span>

 ![Znovu připojte všechny datové svazky](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. <span data-ttu-id="901c6-413">Možnosti obnovení v rámci SAP HANA Studio, vyberte, pokud nepocházejí automaticky po opětovném připojení k databázi HANA přes SAP HANA Studio.</span><span class="sxs-lookup"><span data-stu-id="901c6-413">Select recovery options within SAP HANA Studio, if they do not automatically come up when you reconnect to HANA DB through SAP HANA Studio.</span></span> <span data-ttu-id="901c6-414">Následující příklad ukazuje obnovení poslední snímek HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-414">The following example shows a restoration to the last HANA snapshot.</span></span> <span data-ttu-id="901c6-415">Jeden snímek HANA vloží snímek úložiště a pokud obnovujete na poslední snímek úložiště, musí být poslední HANA snímku.</span><span class="sxs-lookup"><span data-stu-id="901c6-415">A storage snapshot embeds one HANA snapshot, and if you are restoring to the most recent storage snapshot, it should be the most recent HANA snapshot.</span></span> <span data-ttu-id="901c6-416">(Pokud obnovujete do starší úložiště snímků, budete muset najít HANA snímku na základě doby pořízení snímku úložiště.)</span><span class="sxs-lookup"><span data-stu-id="901c6-416">(If you are restoring to older storage snapshots, you need to locate the HANA snapshot based on the time the storage snapshot was taken.)</span></span>

 ![Vyberte možnosti obnovení studia SAP HANA](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. <span data-ttu-id="901c6-418">Vyberte **obnovit databázi do konkrétních dat zálohování nebo úložiště snímků**.</span><span class="sxs-lookup"><span data-stu-id="901c6-418">Select **Recover the database to a specific data backup or storage snapshot**.</span></span>

 ![Okno "Zadejte typ obnovení"](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. <span data-ttu-id="901c6-420">Vyberte **zadejte zálohování bez katalogu**.</span><span class="sxs-lookup"><span data-stu-id="901c6-420">Select **Specify backup without catalog**.</span></span>

 !["Zadejte umístění zálohy" okna](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. <span data-ttu-id="901c6-422">V **cílový typ** seznamu, vyberte **snímku**.</span><span class="sxs-lookup"><span data-stu-id="901c6-422">In the **Destination Type** list, select **Snapshot**.</span></span>

 ![Okno "Zadejte zálohu k obnovení"](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. <span data-ttu-id="901c6-424">Klikněte na tlačítko **Dokončit** zahájíte proces obnovení.</span><span class="sxs-lookup"><span data-stu-id="901c6-424">Click **Finish** to start the recovery process.</span></span>

 ![Klikněte na tlačítko "Dokončit" zahájíte proces obnovení](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. <span data-ttu-id="901c6-426">Databázi HANA je obnovit a obnovit HANA snímek, který je součástí úložiště snímku.</span><span class="sxs-lookup"><span data-stu-id="901c6-426">The HANA database is restored and recovered to the HANA snapshot that's included in the storage snapshot.</span></span>

 ![Databáze HANA je obnovit a obnovit snímek HANA](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recovering-to-the-most-recent-state"></a><span data-ttu-id="901c6-428">Obnovení na nejnovější stav</span><span class="sxs-lookup"><span data-stu-id="901c6-428">Recovering to the most recent state</span></span>

<span data-ttu-id="901c6-429">Následující proces obnoví HANA snímek, který je součástí úložiště snímku.</span><span class="sxs-lookup"><span data-stu-id="901c6-429">The following process restores the HANA snapshot that is included in the storage snapshot.</span></span> <span data-ttu-id="901c6-430">Obnoví pak zálohování transakčního protokolu nejnovější stavu databázi před obnovením snímku úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-430">It then restores the transaction log backups to the most recent state of the database before restoring the storage snapshot.</span></span>

>[!IMPORTANT]
><span data-ttu-id="901c6-431">Než budete pokračovat, ujistěte se, že máte úplný a souvislý řetězec zálohy protokolu transakcí.</span><span class="sxs-lookup"><span data-stu-id="901c6-431">Before you proceed, make sure that you have a complete and contiguous chain of transaction log backups.</span></span> <span data-ttu-id="901c6-432">Aktuální stav databáze nelze obnovit bez tyto zálohy.</span><span class="sxs-lookup"><span data-stu-id="901c6-432">Without these backups, you cannot restore the current state of the database.</span></span>

1. <span data-ttu-id="901c6-433">Proveďte kroky 1 – 6 v předchozím postupu v "Obnovení poslední snímek HANA."</span><span class="sxs-lookup"><span data-stu-id="901c6-433">Complete steps 1-6 of the preceding procedure in "Recovering to the most recent HANA snapshot."</span></span>

2. <span data-ttu-id="901c6-434">Vyberte **obnovit databázi do stavu nejnovější**.</span><span class="sxs-lookup"><span data-stu-id="901c6-434">Select **Recover the database to its most recent state**.</span></span>

 ![Vyberte možnost "Obnovit databázi do stavu poslední"](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. <span data-ttu-id="901c6-436">Zadejte umístění poslední zálohy protokolu HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-436">Specify the location of the most recent HANA log backups.</span></span> <span data-ttu-id="901c6-437">Umístění musí obsahovat všechny HANA zálohování transakčního protokolu ze snímku HANA nejnovější stav.</span><span class="sxs-lookup"><span data-stu-id="901c6-437">The location needs to contain all HANA transaction log backups from the HANA snapshot to the most recent state.</span></span>

 ![Zadejte umístění poslední zálohy protokolu HANA](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. <span data-ttu-id="901c6-439">Vyberte zálohu jako základ, ze kterého chcete obnovit databázi.</span><span class="sxs-lookup"><span data-stu-id="901c6-439">Select a backup as a base from which to recover the database.</span></span> <span data-ttu-id="901c6-440">V našem příkladu je to HANA snímek, který je zahrnutý ve snímku úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-440">In our example, this is the HANA snapshot that was included in the storage snapshot.</span></span> <span data-ttu-id="901c6-441">(Jen jeden snímek je uveden na následujícím snímku obrazovky).</span><span class="sxs-lookup"><span data-stu-id="901c6-441">(Only one snapshot is listed in the following screenshot.)</span></span>

 ![Vyberte zálohu jako základ, ze kterého chcete obnovit databázi](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. <span data-ttu-id="901c6-443">Vymazat **použití rozdílové zálohy** zaškrtávací políčko, pokud nejsou k dispozici rozdílů mezi časem HANA snímku a nejnovější stav.</span><span class="sxs-lookup"><span data-stu-id="901c6-443">Clear the **Use Delta Backups** check box if deltas do not exist between the time of the HANA snapshot and the most recent state.</span></span>

 ![Zrušte zaškrtnutí políčka "Použití rozdílové zálohy", pokud neexistuje žádné rozdíly](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. <span data-ttu-id="901c6-445">Na obrazovce Přehled klikněte na tlačítko **Dokončit** spustíte proces obnovení.</span><span class="sxs-lookup"><span data-stu-id="901c6-445">On the summary screen, click **Finish** to start the restoration procedure.</span></span>

 ![Klikněte na tlačítko "Dokončit" na obrazovce Přehled](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recovering-to-another-point-in-time"></a><span data-ttu-id="901c6-447">Obnovení do jiného bodu v čase</span><span class="sxs-lookup"><span data-stu-id="901c6-447">Recovering to another point in time</span></span>
<span data-ttu-id="901c6-448">Obnovit do bodu v čase mezi HANA snímku (zahrnutá ve snímku úložiště) a ten, který je novější než obnovení bodu v čase HANA snímku, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="901c6-448">To recover to a point in time between the HANA snapshot (included in the storage snapshot) and one that is later than the HANA snapshot point-in-time recovery, do the following:</span></span>

1. <span data-ttu-id="901c6-449">Ujistěte se, že máte všechny zálohování transakčního protokolu ze snímku HANA na dobu, kterou chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="901c6-449">Make sure that you have all transaction log backups from the HANA snapshot to the time you want to recover.</span></span>
2. <span data-ttu-id="901c6-450">Začněte postup v části "Obnovení poslední stavu."</span><span class="sxs-lookup"><span data-stu-id="901c6-450">Begin the procedure under "Recovering to the most recent state."</span></span>
3. <span data-ttu-id="901c6-451">V kroku 2 postupu, v **zadejte typ obnovení** vyberte **obnovit databázi do následující bodu v čase**, určete bod v čase a potom proveďte kroky 3 až 6.</span><span class="sxs-lookup"><span data-stu-id="901c6-451">In step 2 of the procedure, in the **Specify Recovery Type** window, select **Recover the database to the following point in time**, specify the point in time, and then complete steps 3-6.</span></span>

## <a name="monitoring-the-execution-of-snapshots"></a><span data-ttu-id="901c6-452">Monitorování provádění snímky</span><span class="sxs-lookup"><span data-stu-id="901c6-452">Monitoring the execution of snapshots</span></span>

<span data-ttu-id="901c6-453">Musíte monitorovat úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="901c6-453">You need to monitor the execution of storage snapshots.</span></span> <span data-ttu-id="901c6-454">Skript, který provede snímku úložiště zapíše výstup do souboru a pak ji uloží je do stejného umístění jako tyto skripty.</span><span class="sxs-lookup"><span data-stu-id="901c6-454">The script that executes a storage snapshot writes output to a file and then saves it to the same location as the Perl scripts.</span></span> <span data-ttu-id="901c6-455">Samostatný soubor je napsán pro každý snímek.</span><span class="sxs-lookup"><span data-stu-id="901c6-455">A separate file is written for each snapshot.</span></span> <span data-ttu-id="901c6-456">Výstup každého souboru zřetelně zobrazí v různých fázích, že se snímek skript spustí:</span><span class="sxs-lookup"><span data-stu-id="901c6-456">The output of each file clearly shows the various phases that the snapshot script executes:</span></span>

- <span data-ttu-id="901c6-457">Hledání svazky, které je třeba pro vytvoření snímku</span><span class="sxs-lookup"><span data-stu-id="901c6-457">Finding the volumes that need to create a snapshot</span></span>
- <span data-ttu-id="901c6-458">Hledání snímky převzaty z těchto svazků</span><span class="sxs-lookup"><span data-stu-id="901c6-458">Finding the snapshots taken from these volumes</span></span>
- <span data-ttu-id="901c6-459">Odstranění případné existující snímků tak, aby odpovídaly počet snímků, které zadáte</span><span class="sxs-lookup"><span data-stu-id="901c6-459">Deleting eventual existing snapshots to match the number of snapshots you specified</span></span>
- <span data-ttu-id="901c6-460">Vytvoření snímku HANA</span><span class="sxs-lookup"><span data-stu-id="901c6-460">Creating a HANA snapshot</span></span>
- <span data-ttu-id="901c6-461">Vytvoření snímku úložiště přes svazky</span><span class="sxs-lookup"><span data-stu-id="901c6-461">Creating the storage snapshot over the volumes</span></span>
- <span data-ttu-id="901c6-462">Odstraněním snímku HANA</span><span class="sxs-lookup"><span data-stu-id="901c6-462">Deleting the HANA snapshot</span></span>
- <span data-ttu-id="901c6-463">Přejmenování nejnovější snímku do **.0**</span><span class="sxs-lookup"><span data-stu-id="901c6-463">Renaming the most recent snapshot to **.0**</span></span>

<span data-ttu-id="901c6-464">Nejdůležitější část skriptu jsou tyto:</span><span class="sxs-lookup"><span data-stu-id="901c6-464">The most important part of the script is this:</span></span>
```
**********************Creating HANA snapshot**********************
Creating the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
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
Deleting the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
<span data-ttu-id="901c6-465">Zobrazí se od této ukázky jak skript záznamy vytvoření snímku HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-465">You can see from this sample how the script records the creation of the HANA snapshot.</span></span> <span data-ttu-id="901c6-466">V případě Škálováním na více systémů je tento proces zahájit v hlavní uzel.</span><span class="sxs-lookup"><span data-stu-id="901c6-466">In the scale-out case, this process is initiated on the master node.</span></span> <span data-ttu-id="901c6-467">Hlavní uzel zahájí synchronní vytvoření snímků na všechny uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="901c6-467">The master node initiates the synchronous creation of the snapshots on each of the worker nodes.</span></span> <span data-ttu-id="901c6-468">Potom pořízení snímku úložiště.</span><span class="sxs-lookup"><span data-stu-id="901c6-468">Then the storage snapshot is taken.</span></span> <span data-ttu-id="901c6-469">Po úspěšné provedení snímků úložiště je odstranění snímku HANA.</span><span class="sxs-lookup"><span data-stu-id="901c6-469">After the successful execution of the storage snapshots, the HANA snapshot is deleted.</span></span>
