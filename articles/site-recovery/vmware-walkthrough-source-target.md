---
title: "aaaSet hello zdroj a cíl pro tooAzure replikace VMware s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky tooset hello nastavení zdroj a cíl pro replikaci virtuálních počítačů VMware tooAzure úložiště s Azure Site Recovery"
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
ms.openlocfilehash: ef33a44bc5da17afb0442be63f576925f5b9a8b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-vmware-replication-tooazure"></a><span data-ttu-id="7ebfc-103">Krok 8: Nastavení hello zdroj a cíl pro tooAzure replikace VMware</span><span class="sxs-lookup"><span data-stu-id="7ebfc-103">Step 8: Set up hello source and target for VMware replication tooAzure</span></span>

<span data-ttu-id="7ebfc-104">Tento článek popisuje, jak tooconfigure zdrojové a cílové nastavení při replikaci místně tooAzure virtuální počítače VMware, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-104">This article describes how tooconfigure source and target settings when replicating on-premises VMware virtual machines tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="7ebfc-105">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="7ebfc-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-hello-source-environment"></a><span data-ttu-id="7ebfc-106">Nastavit hello zdrojové prostředí</span><span class="sxs-lookup"><span data-stu-id="7ebfc-106">Set up hello source environment</span></span>

<span data-ttu-id="7ebfc-107">Nastavení konfigurace serveru hello, zaregistrovat v trezoru hello a vyhledání virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-107">Set up hello configuration server, register it in hello vault, and discover VMs.</span></span>

1. <span data-ttu-id="7ebfc-108">Klikněte na tlačítko **lokality obnovení** > **krok 1: Příprava infrastruktury** > **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-108">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Source**.</span></span>
2. <span data-ttu-id="7ebfc-109">Pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server**.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-109">If you don’t have a configuration server, click **+Configuration server**.</span></span>
3. <span data-ttu-id="7ebfc-110">V **přidat Server**, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-110">In **Add Server**, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="7ebfc-111">Stáhněte si soubor instalace hello Unified instalace nástroje Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-111">Download hello Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="7ebfc-112">Stáhněte si registrační klíč trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-112">Download hello vault registration key.</span></span> <span data-ttu-id="7ebfc-113">Tuto funkci potřebujete po spuštění Unified instalace.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-113">You need this when you run Unified Setup.</span></span> <span data-ttu-id="7ebfc-114">Hello klíč je platný pět dní od jeho vygenerování.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-114">hello key is valid for five days after you generate it.</span></span>

   ![Nastavení zdroje](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-hello-configuration-server-in-hello-vault"></a><span data-ttu-id="7ebfc-116">Zaregistrujte konfigurační server hello v trezoru hello</span><span class="sxs-lookup"><span data-stu-id="7ebfc-116">Register hello configuration server in hello vault</span></span>

<span data-ttu-id="7ebfc-117">Následující hello před start a potom spusťte tooinstall hello konfigurační server, hello procesový server a hlavní cílový server hello sjednocený instalační program.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-117">Do hello following before you start, then run Unified Setup tooinstall hello configuration server, hello process server, and hello master target server.</span></span>
    - <span data-ttu-id="7ebfc-118">Vám zajistí rychlý přehled video</span><span class="sxs-lookup"><span data-stu-id="7ebfc-118">Get a quick video overview</span></span>

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - <span data-ttu-id="7ebfc-119">Ujistěte se, že hello systémové hodiny synchronizované s na konfigurační server hello virtuálních počítačů, [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span><span class="sxs-lookup"><span data-stu-id="7ebfc-119">On hello configuration server VM, make sure that hello system clock is synchronized with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span></span> <span data-ttu-id="7ebfc-120">Měla by se shodovat.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-120">It should match.</span></span> <span data-ttu-id="7ebfc-121">Pokud je 15 minut před nebo za instalace může selhat.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-121">If it's 15 minutes in front or behind, setup might fail.</span></span>
    - <span data-ttu-id="7ebfc-122">Spusťte instalační program jako místní správce na konfigurační server hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-122">Run setup as a Local Administrator on hello configuration server VM.</span></span>
    - <span data-ttu-id="7ebfc-123">Ujistěte se, že je povolená TLS 1.0 na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-123">Make sure TLS 1.0 is enabled on hello VM.</span></span>


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="7ebfc-124">Hello konfigurační server lze také nainstalovat [z příkazového řádku hello](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="7ebfc-124">hello configuration server can also be installed [from hello command line](http://aka.ms/installconfigsrv).</span></span>



## <a name="connect-toovmware-servers"></a><span data-ttu-id="7ebfc-125">Připojte tooVMware servery</span><span class="sxs-lookup"><span data-stu-id="7ebfc-125">Connect tooVMware servers</span></span>

<span data-ttu-id="7ebfc-126">tooallow Azure Site Recovery toodiscover virtuální počítače běžící na vašem místním prostředí, musíte tooconnect serveru VMware vCenter nebo hostitelů vSphere ESXi službou Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-126">tooallow Azure Site Recovery toodiscover virtual machines running in your on-premises environment, you need tooconnect your VMware vCenter Server or vSphere ESXi hosts with Site Recovery.</span></span> <span data-ttu-id="7ebfc-127">Než začnete, vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="7ebfc-127">Note hello following before you start:</span></span>

- <span data-ttu-id="7ebfc-128">Pokud přidáte hello vCenter server nebo hostitelů vSphere-tooSite obnovení pomocí účtu bez oprávnění správce na serveru hello, potřebuje účet hello těchto oprávnění povoleno:</span><span class="sxs-lookup"><span data-stu-id="7ebfc-128">If you add hello vCenter server or vSphere hosts tooSite Recovery with an account without administrator privileges on hello server, hello account needs these privileges enabled:</span></span>
    - <span data-ttu-id="7ebfc-129">Datacenter, úložiště, složka, hostitele, sítě, prostředků, virtuální počítač, vSphere distribuované přepínače.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-129">Datacenter, Datastore, Folder, Host, Network, Resource, Virtual machine, vSphere Distributed Switch.</span></span>
    - <span data-ttu-id="7ebfc-130">Hello vCenter server potřebuje oprávnění úložišť zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-130">hello vCenter server needs Storage views permissions.</span></span>
- <span data-ttu-id="7ebfc-131">Když přidáte tooSite servery VMware obnovení, může trvat 15 minut nebo déle pro ně tooappear hello portálu.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-131">When you add VMware servers tooSite Recovery, it can take 15 minutes or longer for them tooappear in hello portal.</span></span>

### <a name="add-hello-account-for-automatic-discovery"></a><span data-ttu-id="7ebfc-132">Přidat účet hello pro automatické zjišťování</span><span class="sxs-lookup"><span data-stu-id="7ebfc-132">Add hello account for automatic discovery</span></span>

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="set-up-a-connection"></a><span data-ttu-id="7ebfc-133">Nastavit připojení</span><span class="sxs-lookup"><span data-stu-id="7ebfc-133">Set up a connection</span></span>

<span data-ttu-id="7ebfc-134">Připojte tooservers následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="7ebfc-134">Connect tooservers as follows:</span></span>

1. <span data-ttu-id="7ebfc-135">Vyberte **+ vCenter** toostart připojuje VMware vCenter server nebo hostiteli ESXi VMware vSphere.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-135">Select **+vCenter** toostart connecting a VMware vCenter server or a VMware vSphere ESXi host.</span></span>
2. <span data-ttu-id="7ebfc-136">V **přidat vCenter**, zadejte popisný název pro hello vSphere hostitele nebo vCenter server a pak zadejte hello IP adresu nebo plně kvalifikovaný název domény serveru hello.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-136">In **Add vCenter**, specify a friendly name for hello vSphere host or vCenter server, and then specify hello IP address or FQDN of hello server.</span></span>
3. <span data-ttu-id="7ebfc-137">Pokud jsou vaše servery VMware nakonfigurovaná toolisten pro požadavky na jiném portu nechte hello port 443.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-137">Leave hello port as 443 unless your VMware servers are configured toolisten for requests on a different port.</span></span> <span data-ttu-id="7ebfc-138">Vyberte hello účtu, který je tooconnect toohello VMware vCenter nebo vSphere ESXi serveru.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-138">Select hello account that is tooconnect toohello VMware vCenter or vSphere ESXi server.</span></span> <span data-ttu-id="7ebfc-139">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-139">Click **OK**.</span></span>
4. <span data-ttu-id="7ebfc-140">Obnovení lokality se připojuje tooVMware servery pomocí hello zadané nastavení a vyhledá virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-140">Site Recovery connects tooVMware servers using hello specified settings, and discovers VMs.</span></span>

> [!NOTE]
> <span data-ttu-id="7ebfc-141">Pokud přidáváte server nebo hostitele pomocí účtu, který nemá oprávnění správce na serveru vCenter nebo hostitelů hello, ujistěte se, zda má účet hello těchto oprávnění povoleno: Datacenter, úložiště, složku, hostitele, sítě a prostředků, virtuální počítač a vSphere distribuované přepínače.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-141">If you're adding a server or host with an account that doesn't have administrator privileges on hello vCenter or host server, make sure that hello account has these privileges enabled: Datacenter, Datastore, Folder, Host, Network, Resource, Virtual machine, and vSphere Distributed Switch.</span></span> <span data-ttu-id="7ebfc-142">Kromě toho hello VMware vCenter server potřebuje oprávnění povolená zobrazení hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-142">In addition, hello VMware vCenter server needs hello Storage Views privilege enabled.</span></span>


## <a name="set-up-hello-target-environment"></a><span data-ttu-id="7ebfc-143">Nastavení cílového prostředí hello</span><span class="sxs-lookup"><span data-stu-id="7ebfc-143">Set up hello target environment</span></span>

<span data-ttu-id="7ebfc-144">Než nastavíte hello cílové prostředí, ujistěte se, že máte účet úložiště Azure a virtuální sítě nastavit.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-144">Before you set up hello target environment, make sure you have an Azure storage account and virtual network set up.</span></span>

1. <span data-ttu-id="7ebfc-145">Klikněte na tlačítko **připravit infrastrukturu** > **cíl**, a vyberte hello chcete toouse předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-145">Click **Prepare infrastructure** > **Target**, and select hello Azure subscription you want toouse.</span></span>
2. <span data-ttu-id="7ebfc-146">Určit, jestli je váš model nasazení cílového využívající Resource Manager a classic.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-146">Specify whether your target deployment model is Resource Manager-based, or classic.</span></span>
3. <span data-ttu-id="7ebfc-147">Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-147">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

   ![cíl](./media/vmware-walkthrough-source-target/gs-target.png)
4. <span data-ttu-id="7ebfc-149">Pokud jste dosud nevytvořili účet úložiště nebo sítě, klikněte na tlačítko **+ účet úložiště** nebo **+ síť**, toocreate správce prostředků účtu nebo síti vložené.</span><span class="sxs-lookup"><span data-stu-id="7ebfc-149">If you haven't created a storage account or network, click **+Storage account** or **+Network**, toocreate a Resource Manager account or network inline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7ebfc-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ebfc-150">Next steps</span></span>

<span data-ttu-id="7ebfc-151">Přejděte příliš[krok 9: nastavení zásad replikace](vmware-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="7ebfc-151">Go too[Step 9: Set up a replication policy](vmware-walkthrough-replication.md)</span></span>
