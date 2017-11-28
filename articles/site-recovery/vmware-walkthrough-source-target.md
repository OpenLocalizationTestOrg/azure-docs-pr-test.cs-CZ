---
title: "Nastavit zdroje a cíle replikace VMware do Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky nastavit zdroj a cíl nastavení pro replikaci virtuálních počítačů VMware do úložiště Azure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99e422e-daf7-4fa8-af3c-af2340340136
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 94b629a62c3a54eee69ee397b2f27e3f20b753d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-8-set-up-the-source-and-target-for-vmware-replication-to-azure"></a><span data-ttu-id="370af-103">Krok 8: Nastavení zdroje a cíle replikace VMware do Azure</span><span class="sxs-lookup"><span data-stu-id="370af-103">Step 8: Set up the source and target for VMware replication to Azure</span></span>

<span data-ttu-id="370af-104">Tento článek popisuje postup konfigurace nastavení zdrojové a cílové při replikaci na lokální virtuální počítače VMware do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="370af-104">This article describes how to configure source and target settings when replicating on-premises VMware virtual machines to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="370af-105">POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="370af-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-the-source-environment"></a><span data-ttu-id="370af-106">Nastavení zdrojového prostředí</span><span class="sxs-lookup"><span data-stu-id="370af-106">Set up the source environment</span></span>

<span data-ttu-id="370af-107">Nastavení konfigurace serveru, registrace v trezoru a vyhledání virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="370af-107">Set up the configuration server, register it in the vault, and discover VMs.</span></span>

1. <span data-ttu-id="370af-108">Klikněte na tlačítko **lokality obnovení** > **krok 1: Příprava infrastruktury** > **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="370af-108">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Source**.</span></span>
2. <span data-ttu-id="370af-109">Pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server**.</span><span class="sxs-lookup"><span data-stu-id="370af-109">If you don’t have a configuration server, click **+Configuration server**.</span></span>
3. <span data-ttu-id="370af-110">V **přidat Server**, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.</span><span class="sxs-lookup"><span data-stu-id="370af-110">In **Add Server**, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="370af-111">Stáhněte instalační soubor nástroje Unified instalace nástroje Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="370af-111">Download the Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="370af-112">Stáhnout registrační klíč trezoru</span><span class="sxs-lookup"><span data-stu-id="370af-112">Download the vault registration key.</span></span> <span data-ttu-id="370af-113">Tuto funkci potřebujete po spuštění Unified instalace.</span><span class="sxs-lookup"><span data-stu-id="370af-113">You need this when you run Unified Setup.</span></span> <span data-ttu-id="370af-114">Klíč je platný pět dní od jeho vygenerování.</span><span class="sxs-lookup"><span data-stu-id="370af-114">The key is valid for five days after you generate it.</span></span>

   ![Nastavení zdroje](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-the-configuration-server-in-the-vault"></a><span data-ttu-id="370af-116">Zaregistrujte konfigurační server v trezoru</span><span class="sxs-lookup"><span data-stu-id="370af-116">Register the configuration server in the vault</span></span>

<span data-ttu-id="370af-117">Předtím, než můžete spustit, a potom spuštěním Unified instalačního programu nainstalujte konfigurační server, procesový server a hlavní cílový server, postupujte takto.</span><span class="sxs-lookup"><span data-stu-id="370af-117">Do the following before you start, then run Unified Setup to install the configuration server, the process server, and the master target server.</span></span>
    - <span data-ttu-id="370af-118">Vám zajistí rychlý přehled video</span><span class="sxs-lookup"><span data-stu-id="370af-118">Get a quick video overview</span></span>

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - <span data-ttu-id="370af-119">Na serveru, konfigurace virtuálního počítače, zkontrolujte, jestli se systémové hodiny synchronizované se [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span><span class="sxs-lookup"><span data-stu-id="370af-119">On the configuration server VM, make sure that the system clock is synchronized with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span></span> <span data-ttu-id="370af-120">Měla by se shodovat.</span><span class="sxs-lookup"><span data-stu-id="370af-120">It should match.</span></span> <span data-ttu-id="370af-121">Pokud je 15 minut před nebo za instalace může selhat.</span><span class="sxs-lookup"><span data-stu-id="370af-121">If it's 15 minutes in front or behind, setup might fail.</span></span>
    - <span data-ttu-id="370af-122">Spusťte instalační program jako místní správce na serveru konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="370af-122">Run setup as a Local Administrator on the configuration server VM.</span></span>
    - <span data-ttu-id="370af-123">Ujistěte se, že je povolená TLS 1.0 ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="370af-123">Make sure TLS 1.0 is enabled on the VM.</span></span>


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="370af-124">Konfigurační server lze také nainstalovat [z příkazového řádku](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="370af-124">The configuration server can also be installed [from the command line](http://aka.ms/installconfigsrv).</span></span>



## <a name="connect-to-vmware-servers"></a><span data-ttu-id="370af-125">Připojení k serverům VMware</span><span class="sxs-lookup"><span data-stu-id="370af-125">Connect to VMware servers</span></span>

<span data-ttu-id="370af-126">Povolit Azure Site Recovery se zjistit virtuální počítače spuštěné v místním prostředí, kterou potřebujete připojit VMware vCenter Server nebo hostitelů vSphere ESXi službou Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="370af-126">To allow Azure Site Recovery to discover virtual machines running in your on-premises environment, you need to connect your VMware vCenter Server or vSphere ESXi hosts with Site Recovery.</span></span> <span data-ttu-id="370af-127">Než začnete, vezměte na vědomí následující:</span><span class="sxs-lookup"><span data-stu-id="370af-127">Note the following before you start:</span></span>

- <span data-ttu-id="370af-128">Pokud přidáte vCenter server nebo hostitelů vSphere pro obnovení lokality pomocí účtu bez oprávnění správce na serveru, že účet potřebuje těchto oprávnění povoleno:</span><span class="sxs-lookup"><span data-stu-id="370af-128">If you add the vCenter server or vSphere hosts to Site Recovery with an account without administrator privileges on the server, the account needs these privileges enabled:</span></span>
    - <span data-ttu-id="370af-129">Datacenter, úložiště, složka, hostitele, sítě, prostředků, virtuální počítač, vSphere distribuované přepínače.</span><span class="sxs-lookup"><span data-stu-id="370af-129">Datacenter, Datastore, Folder, Host, Network, Resource, Virtual machine, vSphere Distributed Switch.</span></span>
    - <span data-ttu-id="370af-130">VCenter server potřebuje oprávnění úložišť zobrazení.</span><span class="sxs-lookup"><span data-stu-id="370af-130">The vCenter server needs Storage views permissions.</span></span>
- <span data-ttu-id="370af-131">Když přidáte servery VMware server pro obnovení lokality, může trvat 15 minut nebo déle se objevily na portálu.</span><span class="sxs-lookup"><span data-stu-id="370af-131">When you add VMware servers to Site Recovery, it can take 15 minutes or longer for them to appear in the portal.</span></span>

### <a name="add-the-account-for-automatic-discovery"></a><span data-ttu-id="370af-132">Přidejte účet pro automatické zjišťování</span><span class="sxs-lookup"><span data-stu-id="370af-132">Add the account for automatic discovery</span></span>

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="set-up-a-connection"></a><span data-ttu-id="370af-133">Nastavit připojení</span><span class="sxs-lookup"><span data-stu-id="370af-133">Set up a connection</span></span>

<span data-ttu-id="370af-134">Připojení k serverům následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="370af-134">Connect to servers as follows:</span></span>

1. <span data-ttu-id="370af-135">Vyberte **+ vCenter** zahájíte připojení serveru VMware vCenter server nebo hostiteli ESXi VMware vSphere.</span><span class="sxs-lookup"><span data-stu-id="370af-135">Select **+vCenter** to start connecting a VMware vCenter server or a VMware vSphere ESXi host.</span></span>
2. <span data-ttu-id="370af-136">V části **Přidat server vCenter** zadejte popisný název pro hostitele vSphere nebo server vCenter, a poté zadejte IP adresu nebo plně kvalifikovaný název domény serveru.</span><span class="sxs-lookup"><span data-stu-id="370af-136">In **Add vCenter**, specify a friendly name for the vSphere host or vCenter server, and then specify the IP address or FQDN of the server.</span></span>
3. <span data-ttu-id="370af-137">Pokud vaše servery VMware nejsou konfigurované k naslouchání požadavkům na jiném portu, ponechte port 443.</span><span class="sxs-lookup"><span data-stu-id="370af-137">Leave the port as 443 unless your VMware servers are configured to listen for requests on a different port.</span></span> <span data-ttu-id="370af-138">Vyberte účet, který se má připojit k serveru VMware vCenter nebo vSphere ESXi.</span><span class="sxs-lookup"><span data-stu-id="370af-138">Select the account that is to connect to the VMware vCenter or vSphere ESXi server.</span></span> <span data-ttu-id="370af-139">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="370af-139">Click **OK**.</span></span>
4. <span data-ttu-id="370af-140">Site Recovery se připojuje k VMware servery pomocí zadané nastavení a vyhledá virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="370af-140">Site Recovery connects to VMware servers using the specified settings, and discovers VMs.</span></span>

> [!NOTE]
> <span data-ttu-id="370af-141">Pokud přidáváte server nebo hostitele pomocí účtu, který nemá oprávnění správce na serveru vCenter nebo hostitele, ujistěte se, že má účet oprávnění tyto povoleno: Datacenter, úložiště, složky, hostitele, sítě, prostředků, virtuální počítač a vSphere distribuované přepínače.</span><span class="sxs-lookup"><span data-stu-id="370af-141">If you're adding a server or host with an account that doesn't have administrator privileges on the vCenter or host server, make sure that the account has these privileges enabled: Datacenter, Datastore, Folder, Host, Network, Resource, Virtual machine, and vSphere Distributed Switch.</span></span> <span data-ttu-id="370af-142">Kromě toho serveru VMware vCenter server potřebuje oprávnění k zobrazení úložiště povolené.</span><span class="sxs-lookup"><span data-stu-id="370af-142">In addition, the VMware vCenter server needs the Storage Views privilege enabled.</span></span>


## <a name="set-up-the-target-environment"></a><span data-ttu-id="370af-143">Nastavení cílového prostředí</span><span class="sxs-lookup"><span data-stu-id="370af-143">Set up the target environment</span></span>

<span data-ttu-id="370af-144">Než nastavíte cílovém prostředí, ujistěte se, že máte účet úložiště Azure a virtuální sítě nastavit.</span><span class="sxs-lookup"><span data-stu-id="370af-144">Before you set up the target environment, make sure you have an Azure storage account and virtual network set up.</span></span>

1. <span data-ttu-id="370af-145">Klikněte na **Připravit infrastrukturu** > **Cíl** a vyberte předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="370af-145">Click **Prepare infrastructure** > **Target**, and select the Azure subscription you want to use.</span></span>
2. <span data-ttu-id="370af-146">Určit, jestli je váš model nasazení cílového využívající Resource Manager a classic.</span><span class="sxs-lookup"><span data-stu-id="370af-146">Specify whether your target deployment model is Resource Manager-based, or classic.</span></span>
3. <span data-ttu-id="370af-147">Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="370af-147">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

   ![cíl](./media/vmware-walkthrough-source-target/gs-target.png)
4. <span data-ttu-id="370af-149">Pokud jste dosud nevytvořili účet úložiště nebo sítě, klikněte na tlačítko **+ účet úložiště** nebo **+ síť**, abyste mohli vytvořit účet správce prostředků nebo sítě vložené.</span><span class="sxs-lookup"><span data-stu-id="370af-149">If you haven't created a storage account or network, click **+Storage account** or **+Network**, to create a Resource Manager account or network inline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="370af-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="370af-150">Next steps</span></span>

<span data-ttu-id="370af-151">Přejděte na [krok 9: nastavení zásad replikace](vmware-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="370af-151">Go to [Step 9: Set up a replication policy](vmware-walkthrough-replication.md)</span></span>
