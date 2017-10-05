---
title: "Předpoklady pro VMware do Azure replikaci s využitím Azure Site Recovery | Microsoft Docs"
description: "Shrnuje požadavky na replikaci úlohy běžící na virtuálních počítačích VMware do Azure pomocí služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 318156ba-793b-48d0-98d4-cc5436bf28a3
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 7a82e58d9ff9208130c43fcd11d03dcc3238696a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-2-review-the-prerequisites-for-vmware-to-azure-replication"></a><span data-ttu-id="a42a9-103">Krok 2: Přečtěte si požadavky pro VMware do Azure replikace</span><span class="sxs-lookup"><span data-stu-id="a42a9-103">Step 2: Review the prerequisites for VMware to Azure replication</span></span>

<span data-ttu-id="a42a9-104">Přečtěte si požadavky shrnuté v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="a42a9-104">Read the prerequisites summarized in the following table.</span></span>

<span data-ttu-id="a42a9-105">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="a42a9-105">**Prerequisite**</span></span> | <span data-ttu-id="a42a9-106">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="a42a9-106">**Details**</span></span>
--- | ---
<span data-ttu-id="a42a9-107">**Azure**</span><span class="sxs-lookup"><span data-stu-id="a42a9-107">**Azure**</span></span> | <span data-ttu-id="a42a9-108">Další informace o [požadavky pro Azure](site-recovery-prereq.md#azure-requirements)</span><span class="sxs-lookup"><span data-stu-id="a42a9-108">Learn about [Azure requirements](site-recovery-prereq.md#azure-requirements)</span></span>
<span data-ttu-id="a42a9-109">**Lokální konfigurační server**</span><span class="sxs-lookup"><span data-stu-id="a42a9-109">**On-premises configuration server**</span></span> | <span data-ttu-id="a42a9-110">Je třeba virtuální počítače VMware s Windows serverem 2012 R2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a42a9-110">You need a VMware VM running Windows Server 2012 R2 or later.</span></span> <span data-ttu-id="a42a9-111">Nastavit tento server během nasazování Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="a42a9-111">You set up this server during Site Recovery deployment.</span></span><br/><br/> <span data-ttu-id="a42a9-112">Ve výchozím nastavení jsou procesový server a hlavní cílový server také nainstalované na tomto virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="a42a9-112">By default the process server and master target server are also installed on this VM.</span></span> <span data-ttu-id="a42a9-113">Pokud jste vertikální navýšení kapacity, možná budete muset samostatný procesový server a má stejné požadavky jako konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="a42a9-113">When you scale up, you might need a separate process server, and it has the same requirements as the configuration server.</span></span><br/><br/> <span data-ttu-id="a42a9-114">Další informace o těchto součástí [sem](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements)</span><span class="sxs-lookup"><span data-stu-id="a42a9-114">Learn more about these components [here](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements)</span></span>
<span data-ttu-id="a42a9-115">**Na místní servery VMware**</span><span class="sxs-lookup"><span data-stu-id="a42a9-115">**On-premises VMware servers**</span></span> | <span data-ttu-id="a42a9-116">Jeden nebo více VMware vSphere serverů, 6.5, 6.0, 5.5, 5.1 s nejnovější aktualizace.</span><span class="sxs-lookup"><span data-stu-id="a42a9-116">One or more VMware vSphere servers, running 6.5, 6.0, 5.5, 5.1 with latest updates.</span></span> <span data-ttu-id="a42a9-117">Servery musí nacházet ve stejné síti jako konfigurační server (nebo samostatný procesový server).</span><span class="sxs-lookup"><span data-stu-id="a42a9-117">Servers should be located in the same network as the configuration server (or separate process server).</span></span><br/><br/> <span data-ttu-id="a42a9-118">Doporučujeme, abyste server vCenter pro správu hostitele, kde běží verze 6.5, 6.0 nebo 5.5 s nejnovějšími aktualizacemi.</span><span class="sxs-lookup"><span data-stu-id="a42a9-118">We recommend a vCenter server to manage hosts, running 6.5, 6.0 or 5.5 with the latest updates.</span></span>
<span data-ttu-id="a42a9-119">**Místní virtuální počítače**</span><span class="sxs-lookup"><span data-stu-id="a42a9-119">**On-premises VMs**</span></span> | <span data-ttu-id="a42a9-120">Na virtuálních počítačích, které chcete replikovat, musí běžet [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) a počítače musí splňovat [požadavky na Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="a42a9-120">VMs you want to replicate should be running a [supported operating system](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), and conform with [Azure prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span> <span data-ttu-id="a42a9-121">Virtuální počítač by měl mít spuštění nástroje VMware.</span><span class="sxs-lookup"><span data-stu-id="a42a9-121">VM should have VMware tools running.</span></span>
<span data-ttu-id="a42a9-122">**Adresy URL**</span><span class="sxs-lookup"><span data-stu-id="a42a9-122">**URLs**</span></span> | <span data-ttu-id="a42a9-123">Konfigurační server potřebuje přístup k tyto adresy URL:</span><span class="sxs-lookup"><span data-stu-id="a42a9-123">The configuration server needs access to these URLs:</span></span><br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> <span data-ttu-id="a42a9-124">Pokud máte zavedená pravidla brány firewall založená na IP adrese, zkontrolujte, že tato pravidla umožňují komunikaci s Azure.</span><span class="sxs-lookup"><span data-stu-id="a42a9-124">If you have IP address-based firewall rules, ensure they allow communication to Azure.</span></span><br/></br> <span data-ttu-id="a42a9-125">Povolte [Rozsahy IP adres datového centra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).</span><span class="sxs-lookup"><span data-stu-id="a42a9-125">Allow the [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and the HTTPS (443) port.</span></span><br/></br> <span data-ttu-id="a42a9-126">Povolte rozsahy IP adres pro oblast Azure svého předplatného a pro oblast Západní USA (používá se pro řízení přístupu a správu identit).</span><span class="sxs-lookup"><span data-stu-id="a42a9-126">Allow IP address ranges for the Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span><br/><br/> <span data-ttu-id="a42a9-127">Povolit tuto adresu URL pro stažení MySQL: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.</span><span class="sxs-lookup"><span data-stu-id="a42a9-127">Allow this URL for the MySQL download: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.</span></span>
<span data-ttu-id="a42a9-128">**Služba mobility**</span><span class="sxs-lookup"><span data-stu-id="a42a9-128">**Mobility service**</span></span> | <span data-ttu-id="a42a9-129">Nainstalovat na každý replikovaných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a42a9-129">Installed on every replicated VM.</span></span>




## <a name="limitations"></a><span data-ttu-id="a42a9-130">Omezení</span><span class="sxs-lookup"><span data-stu-id="a42a9-130">Limitations</span></span>

<span data-ttu-id="a42a9-131">Ujistěte se, že rozumíte omezení shrnuto v tabulce před nasazením.</span><span class="sxs-lookup"><span data-stu-id="a42a9-131">Make sure you understand the limitations summarized in the table before you deploy.</span></span>

<span data-ttu-id="a42a9-132">**Omezení**</span><span class="sxs-lookup"><span data-stu-id="a42a9-132">**Limitation**</span></span> | <span data-ttu-id="a42a9-133">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="a42a9-133">**Details**</span></span>
--- | ---
<span data-ttu-id="a42a9-134">**Azure**</span><span class="sxs-lookup"><span data-stu-id="a42a9-134">**Azure**</span></span> | <span data-ttu-id="a42a9-135">Účty úložiště a sítě musí být ve stejné oblasti jako trezor</span><span class="sxs-lookup"><span data-stu-id="a42a9-135">Storage and network accounts must be in the same region as the vault</span></span><br/><br/> <span data-ttu-id="a42a9-136">Pokud používáte účet úložiště premium, potřebujete účet standardního úložiště pro ukládání protokolů replikace</span><span class="sxs-lookup"><span data-stu-id="a42a9-136">If you use a premium storage account, you also need a standard store account to store replication logs</span></span><br/><br/> <span data-ttu-id="a42a9-137">Nelze se replikují na prémiové účty ve střední a – Jih, Indie.</span><span class="sxs-lookup"><span data-stu-id="a42a9-137">You can't replicate to premium accounts in Central and South India.</span></span>
<span data-ttu-id="a42a9-138">**Lokální konfigurační server**</span><span class="sxs-lookup"><span data-stu-id="a42a9-138">**On-premises configuration server**</span></span> | <span data-ttu-id="a42a9-139">Typ adaptéru virtuálního počítače VMware musí být VMXNET3.</span><span class="sxs-lookup"><span data-stu-id="a42a9-139">VMware VM adapter type should be VMXNET3.</span></span> <span data-ttu-id="a42a9-140">Pokud tomu tak není, [tuto aktualizaci nainstalovat](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)</span><span class="sxs-lookup"><span data-stu-id="a42a9-140">If it isn't, [install this update](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)</span></span><br/><br/> <span data-ttu-id="a42a9-141">vSphere PowerCLI 6.0 by měly být nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="a42a9-141">vSphere PowerCLI 6.0 should be installed.</span></span><br/><br> <span data-ttu-id="a42a9-142">Tento počítač by neměl být řadič domény.</span><span class="sxs-lookup"><span data-stu-id="a42a9-142">The machine shouldn't be a domain controller.</span></span> <span data-ttu-id="a42a9-143">Tento počítač by měl mít statickou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="a42a9-143">The machine should have a static IP address.</span></span><br/><br/> <span data-ttu-id="a42a9-144">Název hostitele musí být maximálně 15 znaků nebo méně, a operační systém by měl být v angličtině.</span><span class="sxs-lookup"><span data-stu-id="a42a9-144">The host name should be 15 characters or less, and operating system should be in English.</span></span>
<span data-ttu-id="a42a9-145">**VMware**</span><span class="sxs-lookup"><span data-stu-id="a42a9-145">**VMware**</span></span> | <span data-ttu-id="a42a9-146">Site Recovery nepodporuje nové vCenter a vSphere 6.5 a 6.0 funkce, jako křížové řešení vMotion vCenter, virtuální svazky a úložiště DRS.</span><span class="sxs-lookup"><span data-stu-id="a42a9-146">Site Recovery doesn't support new vCenter and vSphere 6.5 and 6.0 features such as cross vCenter vMotion, virtual volumes, and storage DRS.</span></span>
<span data-ttu-id="a42a9-147">**Virtuální počítače**</span><span class="sxs-lookup"><span data-stu-id="a42a9-147">**VMs**</span></span> | <span data-ttu-id="a42a9-148">Ověřte [omezení virtuálního počítače Azure](site-recovery-prereq.md#azure-requirements)</span><span class="sxs-lookup"><span data-stu-id="a42a9-148">Verify [Azure VM limitations](site-recovery-prereq.md#azure-requirements)</span></span><br/><br/> <span data-ttu-id="a42a9-149">Nelze replikovat virtuální počítače s šifrované disky nebo virtuální počítače pomocí rozhraní UEFI nebo EFI.</span><span class="sxs-lookup"><span data-stu-id="a42a9-149">You can't replicate VMs with encrypted disks, or VMs with UEFI/EFI boot.</span></span><br/><br> <span data-ttu-id="a42a9-150">Sdílený disk, clustery nejsou podporované.</span><span class="sxs-lookup"><span data-stu-id="a42a9-150">Shared disk clusters aren't supported.</span></span> <span data-ttu-id="a42a9-151">Pokud virtuální počítač má zdroj seskupování síťových adaptérů, jsou převedeny na jednu síťovou kartu po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="a42a9-151">If the source VM has NIC teaming, it's converted to a single NIC after failover.</span></span><br/><br/> <span data-ttu-id="a42a9-152">Pokud virtuální počítače iSCSI disk, Site Recovery převede jej na soubor virtuálního pevného disku po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="a42a9-152">If VMs have an iSCSI disk, Site Recovery converts it to a VHD file after failover.</span></span> <span data-ttu-id="a42a9-153">Pokud cíle iSCSI, dosažitelný z virtuálního počítače Azure, se k němu připojuje a zobrazí se i virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="a42a9-153">If the iSCSI target can be reached by the Azure VM, it connects to it, and sees both it and the VHD.</span></span> <span data-ttu-id="a42a9-154">Pokud k tomu dojde, odpojte cíle iSCSI.</span><span class="sxs-lookup"><span data-stu-id="a42a9-154">If this happens, disconnect the iSCSI target.</span></span><br/><br/> <span data-ttu-id="a42a9-155">Pokud chcete povolit konzistence více virtuálních počítačů, která umožňuje virtuální počítače běží stejné zatížení dohromady obnovit konzistentní datový bod, otevřete port 20004 ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="a42a9-155">If you want to enable multi-VM consistency, which enables VMs running the same workload to be recovered together to a consistent data point, open port 20004 on the VM.</span></span><br/><br/> <span data-ttu-id="a42a9-156">Windows musí být nainstalován na jednotce C.</span><span class="sxs-lookup"><span data-stu-id="a42a9-156">Windows must be installed on the C drive.</span></span> <span data-ttu-id="a42a9-157">Disk operačního systému musí být základní a nikoli dynamické.</span><span class="sxs-lookup"><span data-stu-id="a42a9-157">The OS disk should be basic, and not dynamic.</span></span> <span data-ttu-id="a42a9-158">Datový disk může být dynamické.</span><span class="sxs-lookup"><span data-stu-id="a42a9-158">The data disk can be dynamic.</span></span><br/><br/> <span data-ttu-id="a42a9-159">Soubory Linux/etc/hosts na virtuálních počítačích by měly obsahovat položky, které mapování názvu místního hostitele na IP adresy přidružené všechny síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="a42a9-159">Linux /etc/hosts files on VMs should contain entries that map the local host name to IP addresses associated with all network adapters.</span></span> <span data-ttu-id="a42a9-160">Název hostitele, přípojné body, název zařízení, systémové cesty a názvy souborů (/ etc; USR) by měly být pouze v angličtině.</span><span class="sxs-lookup"><span data-stu-id="a42a9-160">The host name, mount points, device name, system paths, and file names (/etc; /usr) should be in English only.</span></span><br/><br/> <span data-ttu-id="a42a9-161">Konkrétní typy [Linux úložiště](site-recovery-support-matrix-to-azure.md#support-for-storage) jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="a42a9-161">Specific types of [Linux storage](site-recovery-support-matrix-to-azure.md#support-for-storage) are supported.</span></span><br/><br/><span data-ttu-id="a42a9-162">Vytvořit nebo nastavit **disk.enableUUID=true** v nastavení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a42a9-162">Create or set **disk.enableUUID=true** in the VM settings.</span></span> <span data-ttu-id="a42a9-163">To poskytuje konzistentní UUID VMDK, tak, aby správně připojí a zajišťuje, že jenom rozdílové změny jsou přenášeny zpět na místní během navrácení služeb po obnovení bez úplná replikace.</span><span class="sxs-lookup"><span data-stu-id="a42a9-163">This provides a consistent UUID to the VMDK, so that it mounts correctly, and ensures that only delta changes are transferred back to on-premises during failback, without full replication.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a42a9-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a42a9-164">Next steps</span></span>

- <span data-ttu-id="a42a9-165">Pokud jste to úplné nasazení, přejděte na [krok 3: plánování kapacity](vmware-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="a42a9-165">If you're doing a full deployment, go to [Step 3: Plan capacity](vmware-walkthrough-capacity.md)</span></span>
- <span data-ttu-id="a42a9-166">Pokud jste to jednoduchá testovací nasazení, přejděte na [krok 4: plánování sítě](vmware-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="a42a9-166">If you're doing a simple test deployment, go to [Step 4: Plan networking](vmware-walkthrough-network.md).</span></span>