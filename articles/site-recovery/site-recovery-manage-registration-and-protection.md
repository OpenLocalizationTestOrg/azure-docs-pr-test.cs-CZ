---
title: "aaaRemove servery a zakažte | Microsoft Docs"
description: "Tento článek popisuje, jak toounregister servery ze služby Site Recovery trezoru a toodisable ochranu pro virtuální počítače a fyzické servery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: ef1f31d5-285b-4a0f-89b5-0123cd422d80
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/27/2017
ms.author: raynew
ms.openlocfilehash: 95f20433f782c93685ad4bae93c6bc0e2d2f2356
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="remove-servers-and-disable-protection"></a><span data-ttu-id="3a16b-103">Odebrání serverů a zakázání ochrany</span><span class="sxs-lookup"><span data-stu-id="3a16b-103">Remove servers and disable protection</span></span>

<span data-ttu-id="3a16b-104">Hello služba Azure Site Recovery přispívá tooyour provozní kontinuitu a strategie po havárii (BCDR).</span><span class="sxs-lookup"><span data-stu-id="3a16b-104">hello Azure Site Recovery service contributes tooyour business continuity and disaster recovery (BCDR) strategy.</span></span> <span data-ttu-id="3a16b-105">Hello služeb orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů.</span><span class="sxs-lookup"><span data-stu-id="3a16b-105">hello service orchestrates replication, failover and recovery of virtual machines and physical servers.</span></span> <span data-ttu-id="3a16b-106">Počítače může být replikované tooAzure nebo tooa sekundárního místního datového centra.</span><span class="sxs-lookup"><span data-stu-id="3a16b-106">Machines can be replicated tooAzure, or tooa secondary on-premises data center.</span></span> <span data-ttu-id="3a16b-107">Rychlý přehled najdete v článku [Co je Azure Site Recovery?](site-recovery-overview.md)</span><span class="sxs-lookup"><span data-stu-id="3a16b-107">For a quick overview read [What is Azure Site Recovery?](site-recovery-overview.md)</span></span>

<span data-ttu-id="3a16b-108">Tento článek popisuje, jak trezoru toounregister servery ze služeb zotavení v hello portál Azure a jak toodisable ochranu pro počítače chráněné službou Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="3a16b-108">This article describes how toounregister servers from a Recovery Services vault in hello Azure portal, and how toodisable protection for machines protected by Site Recovery.</span></span>

<span data-ttu-id="3a16b-109">POST jakékoli dotazy nebo připomínky v hello dolní části tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="3a16b-109">Post any comments or questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="unregister-a-connected-configuration-server"></a><span data-ttu-id="3a16b-110">Zrušit registraci připojené konfigurační server</span><span class="sxs-lookup"><span data-stu-id="3a16b-110">Unregister a connected configuration server</span></span>

<span data-ttu-id="3a16b-111">Pokud budete replikovat virtuální počítače VMware nebo fyzických serverů tooAzure Windows nebo Linuxem, můžete zrušit registraci připojené konfigurační server z trezoru následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3a16b-111">If you replicate VMware VMs or Windows/Linux physical servers tooAzure, you can unregister a connected configuration server from a vault as follows:</span></span>

1. <span data-ttu-id="3a16b-112">Zakažte ochranu počítače.</span><span class="sxs-lookup"><span data-stu-id="3a16b-112">Disable machine protection.</span></span> <span data-ttu-id="3a16b-113">V **chráněné položky** > **replikované položky**, klikněte pravým tlačítkem na počítač hello > **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-113">In **Protected Items** > **Replicated Items**, right-click hello machine > **Delete**.</span></span>
2. <span data-ttu-id="3a16b-114">Zrušit přidružení žádné zásady.</span><span class="sxs-lookup"><span data-stu-id="3a16b-114">Disassociate any policies.</span></span> <span data-ttu-id="3a16b-115">V **infrastruktura Site Recovery** > **pro VMWare a fyzické počítače** > **zásady replikace**, dvakrát klikněte na hello přidružené zásady.</span><span class="sxs-lookup"><span data-stu-id="3a16b-115">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Replication Policies**, double-click hello associated policy.</span></span> <span data-ttu-id="3a16b-116">Klikněte pravým tlačítkem na hello konfigurační server > **zrušení spojení**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-116">Right-click hello configuration server > **Disassociate**.</span></span>
3. <span data-ttu-id="3a16b-117">Odeberte všechny další místní proces a hlavních cílových serverů.</span><span class="sxs-lookup"><span data-stu-id="3a16b-117">Remove any additional on-premises process or master target servers.</span></span> <span data-ttu-id="3a16b-118">V **infrastruktura Site Recovery** > **pro VMWare a fyzické počítače** > **konfigurační servery**, klikněte pravým tlačítkem na server hello > **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-118">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Configuration Servers**, right-click hello server > **Delete**.</span></span>
4. <span data-ttu-id="3a16b-119">Odstraňte konfigurační server hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-119">Delete hello configuration server.</span></span>
5. <span data-ttu-id="3a16b-120">Ručně odinstalujte službu Mobility hello systémem hello hlavní cílový server (bude jím buď samostatný server nebo systémem hello konfigurační server).</span><span class="sxs-lookup"><span data-stu-id="3a16b-120">Manually uninstall hello Mobility service running on hello master target server (this will be either a separate server, or running on hello configuration server).</span></span>
6. <span data-ttu-id="3a16b-121">Odinstalujte všechny servery další proces.</span><span class="sxs-lookup"><span data-stu-id="3a16b-121">Uninstall any additional process servers.</span></span>
7. <span data-ttu-id="3a16b-122">Odinstalujte hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="3a16b-122">Uninstall hello configuration server.</span></span>
8. <span data-ttu-id="3a16b-123">Na konfiguračním serveru hello odinstalujte hello instanci databáze MySQL, která byla nainstalována pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="3a16b-123">On hello configuration server, uninstall hello instance of MySQL that was installed by Site Recovery.</span></span>
9. <span data-ttu-id="3a16b-124">V registru hello hello konfigurace serveru odstranit klíč hello ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span><span class="sxs-lookup"><span data-stu-id="3a16b-124">In hello registry of hello configuration server delete hello key ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span></span>

## <a name="unregister-a-unconnected-configuration-server"></a><span data-ttu-id="3a16b-125">Zrušit registraci serveru bez připojení konfigurace</span><span class="sxs-lookup"><span data-stu-id="3a16b-125">Unregister a unconnected configuration server</span></span>

<span data-ttu-id="3a16b-126">Pokud budete replikovat virtuální počítače VMware nebo fyzických serverů tooAzure Windows nebo Linuxem, můžete zrušit registraci serveru bez připojení konfiguraci z úložiště následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3a16b-126">If you replicate VMware VMs or Windows/Linux physical servers tooAzure, you can unregister an unconnected configuration server from a vault as follows:</span></span>

1. <span data-ttu-id="3a16b-127">Zakažte ochranu počítače.</span><span class="sxs-lookup"><span data-stu-id="3a16b-127">Disable machine protection.</span></span> <span data-ttu-id="3a16b-128">V **chráněné položky** > **replikované položky**, klikněte pravým tlačítkem na počítač hello > **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-128">In **Protected Items** > **Replicated Items**, right-click hello machine > **Delete**.</span></span> <span data-ttu-id="3a16b-129">Vyberte **přestat spravovat počítač hello**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-129">Select **Stop managing hello machine**.</span></span>
2. <span data-ttu-id="3a16b-130">Odeberte všechny další místní proces a hlavních cílových serverů.</span><span class="sxs-lookup"><span data-stu-id="3a16b-130">Remove any additional on-premises process or master target servers.</span></span> <span data-ttu-id="3a16b-131">V **infrastruktura Site Recovery** > **pro VMWare a fyzické počítače** > **konfigurační servery**, klikněte pravým tlačítkem na server hello > **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-131">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Configuration Servers**, right-click hello server > **Delete**.</span></span>
3. <span data-ttu-id="3a16b-132">Odstraňte konfigurační server hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-132">Delete hello configuration server.</span></span>
4. <span data-ttu-id="3a16b-133">Ručně odinstalujte službu Mobility hello systémem hello hlavní cílový server (bude jím buď samostatný server nebo systémem hello konfigurační server).</span><span class="sxs-lookup"><span data-stu-id="3a16b-133">Manually uninstall hello Mobility service running on hello master target server (this will be either a separate server, or running on hello configuration server).</span></span>
5. <span data-ttu-id="3a16b-134">Odinstalujte všechny servery další proces.</span><span class="sxs-lookup"><span data-stu-id="3a16b-134">Uninstall any additional process servers.</span></span>
6. <span data-ttu-id="3a16b-135">Odinstalujte hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="3a16b-135">Uninstall hello configuration server.</span></span>
7. <span data-ttu-id="3a16b-136">Na konfiguračním serveru hello odinstalujte hello instanci databáze MySQL, která byla nainstalována pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="3a16b-136">On hello configuration server, uninstall hello instance of MySQL that was installed by Site Recovery.</span></span>
8. <span data-ttu-id="3a16b-137">V registru hello hello konfigurace serveru odstranit klíč hello ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span><span class="sxs-lookup"><span data-stu-id="3a16b-137">In hello registry of hello configuration server delete hello key ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span></span>

## <a name="unregister-a-connected-vmm-server"></a><span data-ttu-id="3a16b-138">Zrušení registrace připojený k serveru VMM</span><span class="sxs-lookup"><span data-stu-id="3a16b-138">Unregister a connected VMM server</span></span>

<span data-ttu-id="3a16b-139">Jako osvědčený postup doporučujeme zrušit registraci serveru VMM hello, pokud byl připojený tooAzure.</span><span class="sxs-lookup"><span data-stu-id="3a16b-139">As a best practice, we recommend that you unregister hello VMM server when it's connected tooAzure.</span></span> <span data-ttu-id="3a16b-140">Tím se zajistí, že jsou správně vyčistit nastavení na serverech VMM hello (a na jiných serverech VMM s spárované cloudy).</span><span class="sxs-lookup"><span data-stu-id="3a16b-140">This ensures that settings on hello VMM servers (and on other VMM servers with paired clouds) are cleaned up properly.</span></span> <span data-ttu-id="3a16b-141">Bez připojení serveru byste měli odebrat, pouze pokud je trvalé potíže s připojením.</span><span class="sxs-lookup"><span data-stu-id="3a16b-141">You should only remove an unconnected server if there's a permanent issue with connectivity.</span></span> <span data-ttu-id="3a16b-142">Pokud není připojený hello VMM server, budete potřebovat toomanually spustit skript tooclean nastavení.</span><span class="sxs-lookup"><span data-stu-id="3a16b-142">If hello VMM server isn’t connected, you will need toomanually run a script tooclean up settings.</span></span>

1. <span data-ttu-id="3a16b-143">Zastavení replikace virtuálních počítačů v cloudech VMM server má tooremove hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-143">Stop replicating VMs in clouds on hello VMM server you want tooremove.</span></span>
2. <span data-ttu-id="3a16b-144">Odstraňte veškerá jeho mapování sítě používá cloudy na serveru VMM má toodelete hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-144">Delete any network mappings used by clouds on hello VMM server you want toodelete.</span></span> <span data-ttu-id="3a16b-145">V **infrastruktura Site Recovery** > **pro System Center VMM** > **mapování sítě**, klikněte pravým tlačítkem na mapování sítě hello >  **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-145">In **Site Recovery Infrastructure** > **For System Center VMM** > **Network Mapping**, right-click hello network mapping > **Delete**.</span></span>
3. <span data-ttu-id="3a16b-146">Zrušit přidružení zásad replikace z cloudy na serveru VMM má tooremove hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-146">Disassociate replication policies from clouds on hello VMM server you want tooremove.</span></span>  <span data-ttu-id="3a16b-147">V **infrastruktura Site Recovery** > **pro System Center VMM** >  **zásady replikace**, dvakrát klikněte na hello přidružené zásady.</span><span class="sxs-lookup"><span data-stu-id="3a16b-147">In **Site Recovery Infrastructure** > **For System Center VMM** >  **Replication Policies**, double-click hello associated policy.</span></span> <span data-ttu-id="3a16b-148">Klikněte pravým tlačítkem na cloudu hello > **zrušení spojení**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-148">Right-click hello cloud > **Disassociate**.</span></span>
4. <span data-ttu-id="3a16b-149">Odstraňte hello VMM server nebo aktivním uzlu VMM.</span><span class="sxs-lookup"><span data-stu-id="3a16b-149">Delete hello VMM server or active VMM node.</span></span> <span data-ttu-id="3a16b-150">V **infrastruktura Site Recovery** > **pro System Center VMM** > **servery VMM**, klikněte pravým tlačítkem na server hello >  **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-150">In **Site Recovery Infrastructure** > **For System Center VMM** > **VMM Servers**, right-click hello server > **Delete**.</span></span>
5. <span data-ttu-id="3a16b-151">Odinstalujte hello zprostředkovatele na serveru VMM hello ručně.</span><span class="sxs-lookup"><span data-stu-id="3a16b-151">Uninstall hello Provider manually on hello VMM server.</span></span> <span data-ttu-id="3a16b-152">Pokud máte cluster, odeberte ze všech uzlů.</span><span class="sxs-lookup"><span data-stu-id="3a16b-152">If you have a cluster, remove from all nodes.</span></span>
6. <span data-ttu-id="3a16b-153">Pokud replikujete tooAzure, ručně odeberte agenta služeb zotavení Microsoft hello z hostitelů Hyper-V v cloudech hello odstranit.</span><span class="sxs-lookup"><span data-stu-id="3a16b-153">If you're replicating tooAzure, manually remove hello Microsoft Recovery Services agent from Hyper-V hosts in hello deleted clouds.</span></span>



### <a name="unregister-an-unconnected-vmm-server"></a><span data-ttu-id="3a16b-154">Zrušit registraci serveru VMM bez připojení</span><span class="sxs-lookup"><span data-stu-id="3a16b-154">Unregister an unconnected VMM server</span></span>

1. <span data-ttu-id="3a16b-155">Zastavení replikace virtuálních počítačů v cloudech VMM server má tooremove hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-155">Stop replicating VMs in clouds on hello VMM server you want tooremove.</span></span>
2. <span data-ttu-id="3a16b-156">Odstraňte veškerá jeho mapování sítě používá cloudy na serveru VMM hello, které chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="3a16b-156">Delete any network mappings used by clouds on hello VMM server that you want toodelete.</span></span> <span data-ttu-id="3a16b-157">V **infrastruktura Site Recovery** > **pro System Center VMM** > **mapování sítě**, klikněte pravým tlačítkem na mapování sítě hello >  **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-157">In **Site Recovery Infrastructure** > **For System Center VMM** > **Network Mapping**, right-click hello network mapping > **Delete**.</span></span>
3. <span data-ttu-id="3a16b-158">Poznamenejte si ID hello hello serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="3a16b-158">Note hello ID of hello VMM server.</span></span>
4. <span data-ttu-id="3a16b-159">Zrušit přidružení zásad replikace z cloudy na serveru VMM má tooremove hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-159">Disassociate replication policies from clouds on hello VMM server you want tooremove.</span></span>  <span data-ttu-id="3a16b-160">V **infrastruktura Site Recovery** > **pro System Center VMM** >  **zásady replikace**, dvakrát klikněte na hello přidružené zásady.</span><span class="sxs-lookup"><span data-stu-id="3a16b-160">In **Site Recovery Infrastructure** > **For System Center VMM** >  **Replication Policies**, double-click hello associated policy.</span></span> <span data-ttu-id="3a16b-161">Klikněte pravým tlačítkem na cloudu hello > **zrušení spojení**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-161">Right-click hello cloud > **Disassociate**.</span></span>
5. <span data-ttu-id="3a16b-162">Odstraňte hello VMM server nebo aktivní uzel.</span><span class="sxs-lookup"><span data-stu-id="3a16b-162">Delete hello VMM server or active node.</span></span> <span data-ttu-id="3a16b-163">V **infrastruktura Site Recovery** > **pro System Center VMM** > **servery VMM**, klikněte pravým tlačítkem na server hello >  **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-163">In **Site Recovery Infrastructure** > **For System Center VMM** > **VMM Servers**, right-click hello server > **Delete**.</span></span>
6. <span data-ttu-id="3a16b-164">Stažení a spuštění hello [skript pro vyčištění](http://aka.ms/asr-cleanup-script-vmm) na serveru VMM hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-164">Download and run hello [cleanup script](http://aka.ms/asr-cleanup-script-vmm) on hello VMM server.</span></span> <span data-ttu-id="3a16b-165">Otevřete prostředí PowerShell s hello **spustit jako správce** možnost zásady spouštění hello toochange pro obor hello výchozí (LocalMachine).</span><span class="sxs-lookup"><span data-stu-id="3a16b-165">Open PowerShell with hello **Run as Administrator** option, toochange hello execution policy for hello default (LocalMachine) scope.</span></span> <span data-ttu-id="3a16b-166">Ve skriptu hello zadejte ID hello hello chcete tooremove serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="3a16b-166">In hello script, specify hello ID of hello VMM server you want tooremove.</span></span> <span data-ttu-id="3a16b-167">skript Hello odebere registrace a cloudového párování informace ze serveru hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-167">hello script removes registration and cloud pairing information from hello server.</span></span>
5. <span data-ttu-id="3a16b-168">Spusťte skript vyčištění hello na jiných serverech VMM, které obsahují cloudy, které jsou spárovaný s cloudy na serveru VMM má tooremove hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-168">Run hello cleanup script on any other VMM servers that contain clouds that are paired with clouds on hello VMM server you want tooremove.</span></span>
6. <span data-ttu-id="3a16b-169">Spusťte skript vyčištění hello na žádné jiné pasivní uzly clusteru VMM mající nainstalovaný zprostředkovatel hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-169">Run hello  cleanup script on any other passive VMM cluster nodes that have hello Provider installed.</span></span>
7. <span data-ttu-id="3a16b-170">Odinstalujte hello zprostředkovatele na serveru VMM hello ručně.</span><span class="sxs-lookup"><span data-stu-id="3a16b-170">Uninstall hello Provider manually on hello VMM server.</span></span> <span data-ttu-id="3a16b-171">Pokud máte cluster, odeberte ze všech uzlů.</span><span class="sxs-lookup"><span data-stu-id="3a16b-171">If you have a cluster, remove from all nodes.</span></span>
8. <span data-ttu-id="3a16b-172">Pokud je replikace tooAzure, můžete odebrat agenta služeb zotavení Microsoft hello z hostitelů Hyper-V v cloudech hello odstranit.</span><span class="sxs-lookup"><span data-stu-id="3a16b-172">If you replicating tooAzure, you can remove hello Microsoft Recovery Services agent from Hyper-V hosts in hello deleted clouds.</span></span>

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a><span data-ttu-id="3a16b-173">Zrušit registraci hostitele Hyper-V v lokalitě Hyper-V</span><span class="sxs-lookup"><span data-stu-id="3a16b-173">Unregister a Hyper-V host in a Hyper-V Site</span></span>

<span data-ttu-id="3a16b-174">Hostitelé Hyper-V, které nejsou spravované přes VMM jsou shromážděná do lokality Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="3a16b-174">Hyper-V hosts that aren't managed by VMM are gathered into a Hyper-V site.</span></span> <span data-ttu-id="3a16b-175">Odeberte hostitele v lokalitě Hyper-V následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3a16b-175">Remove a host in a Hyper-V site as follows:</span></span>

1. <span data-ttu-id="3a16b-176">Zakážete replikaci pro virtuální počítače Hyper-V umístěný na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-176">Disable replication for Hyper-V VMs located on hello host.</span></span>
2. <span data-ttu-id="3a16b-177">Zrušit přidružení zásad pro web hello Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="3a16b-177">Disassociate policies for hello Hyper-V site.</span></span> <span data-ttu-id="3a16b-178">V **infrastruktura Site Recovery** > **pro weby technologie Hyper-V** >  **zásady replikace**, dvakrát klikněte na hello přidružené zásady.</span><span class="sxs-lookup"><span data-stu-id="3a16b-178">In **Site Recovery Infrastructure** > **For Hyper-V Sites** >  **Replication Policies**, double-click hello associated policy.</span></span> <span data-ttu-id="3a16b-179">Klikněte pravým tlačítkem na hello lokality > **zrušení spojení**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-179">Right-click hello site > **Disassociate**.</span></span>
3. <span data-ttu-id="3a16b-180">Odstraňte hostitele Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="3a16b-180">Delete Hyper-V hosts.</span></span> <span data-ttu-id="3a16b-181">V **infrastruktura Site Recovery** > **pro System Center VMM** > **hostitelů Hyper-V**, klikněte pravým tlačítkem na server hello >  **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-181">In **Site Recovery Infrastructure** > **For System Center VMM** > **Hyper-V Hosts**, right-click hello server > **Delete**.</span></span>
4. <span data-ttu-id="3a16b-182">Odstranění hello technologie Hyper-V lokality po odebrání všech hostitelů z něj.</span><span class="sxs-lookup"><span data-stu-id="3a16b-182">Delete hello Hyper-V site after all hosts have been removed from it.</span></span> <span data-ttu-id="3a16b-183">V **infrastruktura Site Recovery** > **pro System Center VMM** > **lokalit Hyper-V**, klikněte pravým tlačítkem na hello lokality >  **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-183">In **Site Recovery Infrastructure** > **For System Center VMM** > **Hyper-V Sites**, right-click hello site > **Delete**.</span></span>
5. <span data-ttu-id="3a16b-184">Spusťte následující skript na každém hostiteli technologie Hyper-V, který jste odebrali hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-184">Run hello following script on each Hyper-V host that you removed.</span></span> <span data-ttu-id="3a16b-185">skript Hello vyčistí nastavení na serveru hello a zruší registraci hello trezoru.</span><span class="sxs-lookup"><span data-stu-id="3a16b-185">hello script cleans up settings on hello server, and unregisters it from hello vault.</span></span>


        `` pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run hello script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }

            $error.Clear()    
            "This script will remove hello old Azure Site Recovery Provider related properties. Do you want toocontinue (Y/N) ?"
            $choice =  Read-Host

            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }

            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping hello Azure Site Recovery service..."
                net stop $serviceName
            }

            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'

            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."    
                    Remove-Item -Recurse -Path $registrationPath
                }

                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }

                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }

            # First retrive all hello certificates toobe deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete hello certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {    
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0]
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd``



## <a name="disable-protection-for-a-vmware-vm-or-physical-server"></a><span data-ttu-id="3a16b-186">Zakažte ochranu pro virtuální počítač VMware nebo fyzických serverů</span><span class="sxs-lookup"><span data-stu-id="3a16b-186">Disable protection for a VMware VM or physical server</span></span>

1. <span data-ttu-id="3a16b-187">V **chráněné položky** > **replikované položky**, klikněte pravým tlačítkem na počítač hello > **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-187">In **Protected Items** > **Replicated Items**, right-click hello machine > **Delete**.</span></span>
2. <span data-ttu-id="3a16b-188">V **odebrat počítač**, vyberte jednu z těchto možností:</span><span class="sxs-lookup"><span data-stu-id="3a16b-188">In **Remove Machine**, select one of these options:</span></span>
    - <span data-ttu-id="3a16b-189">**Zakažte ochranu pro počítač hello (doporučeno)**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-189">**Disable protection for hello machine (recommended)**.</span></span> <span data-ttu-id="3a16b-190">Použijte tuto možnost toostop replikace hello počítače.</span><span class="sxs-lookup"><span data-stu-id="3a16b-190">Use this option toostop replicating hello machine.</span></span> <span data-ttu-id="3a16b-191">Nastavení obnovení lokality se automaticky vyčistí.</span><span class="sxs-lookup"><span data-stu-id="3a16b-191">Site Recovery settings will be cleaned up automatically.</span></span> <span data-ttu-id="3a16b-192">Zobrazí se jenom tato možnost v hello následující okolnosti:</span><span class="sxs-lookup"><span data-stu-id="3a16b-192">You will only see this option in hello following circumstances:</span></span>
        - <span data-ttu-id="3a16b-193">**Jste ke změně velikosti svazku virtuálního počítače hello**– při změně velikosti na svazku hello virtuální počítač přejde do kritického stavu.</span><span class="sxs-lookup"><span data-stu-id="3a16b-193">**You've resized hello VM volume**—When you resize a volume hello virtual machine goes into a critical state.</span></span> <span data-ttu-id="3a16b-194">Vyberte tuto možnost toodisables ochranu při zachování bodů obnovení v Azure.</span><span class="sxs-lookup"><span data-stu-id="3a16b-194">Select this option toodisables protection while retaining recovery points in Azure.</span></span> <span data-ttu-id="3a16b-195">Pokud povolíte ochranu počítače hello znovu, budou data hello hello po změně velikosti svazku přenášená tooAzure.</span><span class="sxs-lookup"><span data-stu-id="3a16b-195">When you enable protection for hello machine again, hello data for hello resized volume will be transferred tooAzure.</span></span>
        - <span data-ttu-id="3a16b-196">**Nedávno jste si vyzkoušeli převzetí služeb při selhání**– po spuštění převzetí služeb při selhání tootest prostředí, vyberte tuto možnost toostart znovu ochranu počítačů v místě.</span><span class="sxs-lookup"><span data-stu-id="3a16b-196">**You've recently run a failover**—After you've run a failover tootest your environment, select this option toostart protecting on-premises machines again.</span></span> <span data-ttu-id="3a16b-197">Zakáže každý virtuální počítač, a pak je třeba tooenable ochrany pro ně znovu.</span><span class="sxs-lookup"><span data-stu-id="3a16b-197">It disables each virtual machine, and then you need tooenable protection for them again.</span></span> <span data-ttu-id="3a16b-198">Zakázání hello počítač se toto nastavení nemá vliv hello virtuální počítač repliky v Azure.</span><span class="sxs-lookup"><span data-stu-id="3a16b-198">Disabling hello machine with this setting doesn't affect hello replica virtual machine in Azure.</span></span> <span data-ttu-id="3a16b-199">Nemáte odinstalujte službu Mobility hello z počítače hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-199">Don't uninstall hello Mobility service from hello machine.</span></span>
    - <span data-ttu-id="3a16b-200">**Přestat spravovat počítač hello**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-200">**Stop managing hello machine**.</span></span> <span data-ttu-id="3a16b-201">Pokud vyberete tuto možnost, bude počítač hello pouze odebrat z trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-201">If you select this option, hello machine will only be removed from hello vault.</span></span> <span data-ttu-id="3a16b-202">Místní nastavení ochrany pro počítač hello nebude mít vliv.</span><span class="sxs-lookup"><span data-stu-id="3a16b-202">On-premises protection settings for hello machine won’t be affected.</span></span> <span data-ttu-id="3a16b-203">tooremove nastavení v počítači hello a počítač hello tooremove z hello předplatné Azure, musíte tooclean hello nastavení odinstalací služby Mobility hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-203">tooremove settings on hello machine, and tooremove hello machine from hello Azure subscription, you need tooclean hello settings by uninstalling hello Mobility service.</span></span>

## <a name="disable-protection-for-a-hyper-v-vm-in-a-vmm-cloud"></a><span data-ttu-id="3a16b-204">Zakažte ochranu pro virtuální počítač technologie Hyper-V v cloudu VMM</span><span class="sxs-lookup"><span data-stu-id="3a16b-204">Disable protection for a Hyper-V VM in a VMM cloud</span></span>

1. <span data-ttu-id="3a16b-205">V **chráněné položky** > **replikované položky**, klikněte pravým tlačítkem na počítač hello > **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-205">In **Protected Items** > **Replicated Items**, right-click hello machine > **Delete**.</span></span>
2. <span data-ttu-id="3a16b-206">V **odebrat počítač**, vyberte jednu z těchto možností:</span><span class="sxs-lookup"><span data-stu-id="3a16b-206">In **Remove Machine**, select one of these options:</span></span>

    - <span data-ttu-id="3a16b-207">**Zakažte ochranu pro počítač hello (doporučeno)**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-207">**Disable protection for hello machine (recommended)**.</span></span> <span data-ttu-id="3a16b-208">Použijte tuto možnost toostop replikace hello počítače.</span><span class="sxs-lookup"><span data-stu-id="3a16b-208">Use this option toostop replicating hello machine.</span></span> <span data-ttu-id="3a16b-209">Nastavení obnovení lokality se automaticky vyčistí.</span><span class="sxs-lookup"><span data-stu-id="3a16b-209">Site Recovery settings will be cleaned up automatically.</span></span>
    - <span data-ttu-id="3a16b-210">**Přestat spravovat počítač hello**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-210">**Stop managing hello machine**.</span></span> <span data-ttu-id="3a16b-211">Pokud vyberete tuto možnost, bude počítač hello pouze odebrat z trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-211">If you select this option, hello machine will only be removed from hello vault.</span></span> <span data-ttu-id="3a16b-212">Místní nastavení ochrany pro počítač hello nebude mít vliv.</span><span class="sxs-lookup"><span data-stu-id="3a16b-212">On-premises protection settings for hello machine won’t be affected.</span></span> <span data-ttu-id="3a16b-213">tooremove nastavení v počítači hello a počítač hello tooremove z hello předplatné Azure, je nutné nastavení hello tooclean až ručně pomocí níže uvedených pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-213">tooremove settings on hello machine, and tooremove hello machine from hello Azure subscription, you need tooclean hello settings up manually, using hello instructions below.</span></span> <span data-ttu-id="3a16b-214">Všimněte si, že pokud vyberete toodelete hello virtuální počítač a jeho pevné disky, budete vyloučení ze hello cílové umístění.</span><span class="sxs-lookup"><span data-stu-id="3a16b-214">Note that if you select toodelete hello virtual machine and its hard disks, they'll be removed from hello target location.</span></span>

### <a name="clean-up-protection-settings---replication-tooa-secondary-vmm-site"></a><span data-ttu-id="3a16b-215">Vyčištění nastavení ochrany - replikace tooa sekundární lokalita VMM</span><span class="sxs-lookup"><span data-stu-id="3a16b-215">Clean up protection settings - replication tooa secondary VMM site</span></span>

<span data-ttu-id="3a16b-216">Pokud jste vybrali **přestat spravovat počítač hello** a můžete replikovat tooa sekundární lokalitu, spusťte tento skript na primární server tooclean hello hello nastavení pro primární virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-216">If you selected **Stop managing hello machine** and you replicating tooa secondary site, run this script on hello primary server tooclean up hello settings for hello primary virtual machine.</span></span> <span data-ttu-id="3a16b-217">V konzole VMM hello klikněte na konzole VMM PowerShell hello tlačítko tooopen aplikace hello prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3a16b-217">In hello VMM console click hello PowerShell button tooopen hello VMM PowerShell console.</span></span> <span data-ttu-id="3a16b-218">Nahraďte SQLVM1 hello název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3a16b-218">Replace SQLVM1 with hello name of your virtual machine.</span></span>

         ``$vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. <span data-ttu-id="3a16b-219">Na sekundárním serveru VMM hello spusťte tento skript tooclean nastavení hello pro hello sekundární virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="3a16b-219">On hello secondary VMM server run this script tooclean up hello settings for hello secondary virtual machine:</span></span>

        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force``
3. <span data-ttu-id="3a16b-220">Na sekundárním serveru VMM hello aktualizujte hello virtuální počítače na serveru hostitele technologie Hyper-V hello, takže hello sekundární virtuální počítač získá zjistit znovu v konzole VMM hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-220">On hello secondary VMM server, refresh hello virtual machines on hello Hyper-V host server, so that hello secondary VM gets detected again in hello VMM console.</span></span>
4. <span data-ttu-id="3a16b-221">Hello výše kroky vymazány hello nastavení replikace na serveru VMM hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-221">hello above steps clear up hello replication settings on hello VMM server.</span></span> <span data-ttu-id="3a16b-222">Pokud chcete, aby toostop replikace pro virtuální počítač hello, spusťte následující skript jejda hello hello primární a sekundární virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="3a16b-222">If you want toostop replication for hello virtual machine, run hello following script oh hello primary and secondary VMs.</span></span> <span data-ttu-id="3a16b-223">Nahraďte SQLVM1 hello název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3a16b-223">Replace SQLVM1 with hello name of your virtual machine.</span></span>

        ``Remove-VMReplication –VMName “SQLVM1”``

### <a name="clean-up-protection-settings---replication-tooazure"></a><span data-ttu-id="3a16b-224">Vyčištění nastavení ochrany - tooAzure replikace</span><span class="sxs-lookup"><span data-stu-id="3a16b-224">Clean up protection settings - replication tooAzure</span></span>

1. <span data-ttu-id="3a16b-225">Pokud jste vybrali **přestat spravovat počítač hello** a můžete replikovat tooAzure, spusťte tento skript na serveru VMM hello zdroje z konzoly VMM hello pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3a16b-225">If you selected **Stop managing hello machine** and you replicate tooAzure, run this script on hello source VMM server, using PowerShell from hello VMM console.</span></span>
        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. <span data-ttu-id="3a16b-226">Hello výše kroky vymazat hello nastavení replikace na serveru VMM hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-226">hello above steps clear hello replication settings on hello VMM server.</span></span> <span data-ttu-id="3a16b-227">toostop replikace pro virtuální počítač hello spuštěná na serveru hostitele technologie Hyper-V hello, spusťte tento skript.</span><span class="sxs-lookup"><span data-stu-id="3a16b-227">toostop replication for hello virtual machine running on hello Hyper-V host server, run this script.</span></span> <span data-ttu-id="3a16b-228">Nahraďte SQLVM1 hello název virtuálního počítače a host01.contoso.com s názvem hello hello serveru hostitele technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="3a16b-228">Replace SQLVM1 with hello name of your virtual machine, and host01.contoso.com with hello name of hello Hyper-V host server.</span></span>

        ``$vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)``


## <a name="disable-protection-for-a-hyper-v-vm-in-a-hyper-v-site"></a><span data-ttu-id="3a16b-229">Zakažte ochranu pro virtuální počítač technologie Hyper-V v lokalitě Hyper-V</span><span class="sxs-lookup"><span data-stu-id="3a16b-229">Disable protection for a Hyper-V VM in a Hyper-V Site</span></span>

<span data-ttu-id="3a16b-230">Tento postup použijte, pokud replikujete virtuální počítače Hyper-V tooAzure bez serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="3a16b-230">Use this procedure if you're replicating Hyper-V VMs tooAzure without a VMM server.</span></span>

1. <span data-ttu-id="3a16b-231">V **chráněné položky** > **replikované položky**, klikněte pravým tlačítkem na počítač hello > **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-231">In **Protected Items** > **Replicated Items**, right-click hello machine > **Delete**.</span></span>
2. <span data-ttu-id="3a16b-232">V **odebrat počítač**, můžete vybrat hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="3a16b-232">In **Remove Machine**, you can select hello following options:</span></span>

   - <span data-ttu-id="3a16b-233">**Zakažte ochranu pro počítač hello (doporučeno)**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-233">**Disable protection for hello machine (recommended)**.</span></span> <span data-ttu-id="3a16b-234">Použijte tuto možnost toostop replikace hello počítače.</span><span class="sxs-lookup"><span data-stu-id="3a16b-234">Use this option toostop replicating hello machine.</span></span> <span data-ttu-id="3a16b-235">Nastavení obnovení lokality se automaticky vyčistí.</span><span class="sxs-lookup"><span data-stu-id="3a16b-235">Site Recovery settings will be cleaned up automatically.</span></span>
   - <span data-ttu-id="3a16b-236">**Přestat spravovat počítač hello**.</span><span class="sxs-lookup"><span data-stu-id="3a16b-236">**Stop managing hello machine**.</span></span> <span data-ttu-id="3a16b-237">Pokud vyberete tuto možnost hello počítače odebere jenom z trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-237">If you select this option hello machine will only be removed from hello vault.</span></span> <span data-ttu-id="3a16b-238">Místní nastavení ochrany pro počítač hello nebude mít vliv.</span><span class="sxs-lookup"><span data-stu-id="3a16b-238">On-premises protection settings for hello machine won’t be affected.</span></span> <span data-ttu-id="3a16b-239">tooremove nastavení v počítači hello a tooremove hello virtuální počítač z hello předplatné Azure, je nutné nastavení hello tooclean si ručně.</span><span class="sxs-lookup"><span data-stu-id="3a16b-239">tooremove settings on hello machine, and tooremove hello virtual machine from hello Azure subscription, you need tooclean hello settings up manually.</span></span> <span data-ttu-id="3a16b-240">Pokud vyberete toodelete hello virtuální počítač a jeho pevné disky, které budou odebrány z hello cílové umístění.</span><span class="sxs-lookup"><span data-stu-id="3a16b-240">If you select toodelete hello virtual machine and its hard disks they will be removed from hello target location.</span></span>
3. <span data-ttu-id="3a16b-241">Pokud jste vybrali **přestat spravovat počítač hello**, spusťte tento skript na serveru hostitele hello zdroj technologie Hyper-V, tooremove replikace pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="3a16b-241">If you selected **Stop managing hello machine**, run this script on hello source Hyper-V host server, tooremove replication for hello virtual machine.</span></span> <span data-ttu-id="3a16b-242">Nahraďte SQLVM1 hello název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3a16b-242">Replace SQLVM1 with hello name of your virtual machine.</span></span>

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)
