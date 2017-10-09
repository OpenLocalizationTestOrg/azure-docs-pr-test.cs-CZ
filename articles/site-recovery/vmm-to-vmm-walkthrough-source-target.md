---
title: "aaaSet hello zdroj a cíl pro tooa replikace technologie Hyper-V s Azure Site Recovery sekundární lokality. | Microsoft Docs"
description: "Popisuje, jak tooset až hello zdrojové a cílové při replikaci virtuálních počítačů Hyper-V toosecondary VMM lokalitu s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: fa7809f1-7633-425f-b25d-d10d004e8d0b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 451cb4413ca5c09777a7faf512b1c8ea43e695f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-set-up-hello-replication-source-and-target"></a><span data-ttu-id="c1547-103">Krok 6: Nastavení hello replikace zdroje a cíle</span><span class="sxs-lookup"><span data-stu-id="c1547-103">Step 6: Set up hello replication source and target</span></span>


<span data-ttu-id="c1547-104">Po vytvoření služby zotavení trezoru pro Hyper-V replikace tooa sekundární lokalita VMM s [Azure Site Recovery](site-recovery-overview.md), použijte tento článek tooset se hello zdroj a cíl replikace umístění.</span><span class="sxs-lookup"><span data-stu-id="c1547-104">After creating a Recovery Services vault for Hyper-V replication tooa secondary VMM site with [Azure Site Recovery](site-recovery-overview.md), use this article tooset up hello source and target replication locations.</span></span> 

<span data-ttu-id="c1547-105">Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="c1547-105">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="set-up-hello-source-environment"></a><span data-ttu-id="c1547-106">Nastavit hello zdrojové prostředí</span><span class="sxs-lookup"><span data-stu-id="c1547-106">Set up hello source environment</span></span>

<span data-ttu-id="c1547-107">Nainstalujte hello zprostředkovatele Azure Site Recovery na serverech VMM a zjišťovat a zaregistrujte server v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="c1547-107">Install hello Azure Site Recovery Provider on VMM servers, and discover and register servers in hello vault.</span></span>

1. <span data-ttu-id="c1547-108">Klikněte na tlačítko **krok 1: Příprava infrastruktury** > **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="c1547-108">Click **Step 1: Prepare Infrastructure** > **Source**.</span></span>

    ![Nastavení zdroje](./media/vmm-to-vmm-walkthrough-source-target/goals-source.png)
2. <span data-ttu-id="c1547-110">V **připravit zdroj**, klikněte na tlačítko **+ VMM** tooadd serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="c1547-110">In **Prepare source**, click **+ VMM** tooadd a VMM server.</span></span>

    ![Nastavení zdroje](./media/vmm-to-vmm-walkthrough-source-target/set-source1.png)
3. <span data-ttu-id="c1547-112">V **přidat Server**, zkontrolujte, zda **serveru System Center VMM** se zobrazí v **typ serveru** a tímto serverem VMM hello splňuje hello [požadavky](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="c1547-112">In **Add Server**, check that **System Center VMM server** appears in **Server type** and that hello VMM server meets hello [prerequisites](#prerequisites).</span></span>
4. <span data-ttu-id="c1547-113">Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery hello.</span><span class="sxs-lookup"><span data-stu-id="c1547-113">Download hello Azure Site Recovery Provider installation file.</span></span>
5. <span data-ttu-id="c1547-114">Stáhněte si registrační klíč hello.</span><span class="sxs-lookup"><span data-stu-id="c1547-114">Download hello registration key.</span></span> <span data-ttu-id="c1547-115">Budete ho potřebovat, když spustíte instalaci.</span><span class="sxs-lookup"><span data-stu-id="c1547-115">You need this when you run setup.</span></span> <span data-ttu-id="c1547-116">Hello klíč je platný pět dní od jeho vygenerování.</span><span class="sxs-lookup"><span data-stu-id="c1547-116">hello key is valid for five days after you generate it.</span></span>

    ![Nastavení zdroje](./media/vmm-to-vmm-walkthrough-source-target/set-source3.png)
6. <span data-ttu-id="c1547-118">Nainstalujte hello zprostředkovatele Azure Site Recovery na serveru VMM hello.</span><span class="sxs-lookup"><span data-stu-id="c1547-118">Install hello Azure Site Recovery Provider on hello VMM server.</span></span> <span data-ttu-id="c1547-119">Nepotřebujete tooexplicitly nic instalovat na hostitelských serverech technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="c1547-119">You don't need tooexplicitly install anything on Hyper-V host servers.</span></span>


## <a name="install-hello-azure-site-recovery-provider"></a><span data-ttu-id="c1547-120">Nainstalujte zprostředkovatele Azure Site Recovery hello</span><span class="sxs-lookup"><span data-stu-id="c1547-120">Install hello Azure Site Recovery Provider</span></span>

1. <span data-ttu-id="c1547-121">Spusťte instalační soubor zprostředkovatele hello na každý server VMM.</span><span class="sxs-lookup"><span data-stu-id="c1547-121">Run hello Provider setup file on each VMM server.</span></span> <span data-ttu-id="c1547-122">Pokud VMM je nasazen v clusteru, proveďte následující hello poprvé, co nainstalujete hello:</span><span class="sxs-lookup"><span data-stu-id="c1547-122">If VMM is deployed in a cluster, do hello following hello first time you install:</span></span>
    -  <span data-ttu-id="c1547-123">Nainstalujte poskytovatele hello na aktivním uzlu a dokončit hello instalace tooregister hello VMM server v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="c1547-123">Install hello provider on an active node, and finish hello installation tooregister hello VMM server in hello vault.</span></span>
    - <span data-ttu-id="c1547-124">Potom nainstalujte hello zprostředkovatele na hello dalších uzlů.</span><span class="sxs-lookup"><span data-stu-id="c1547-124">Then, install hello Provider on hello other nodes.</span></span> <span data-ttu-id="c1547-125">Uzly clusteru, měly být spuštěny hello stejnou verzi hello zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="c1547-125">Cluster nodes should all run hello same version of hello Provider.</span></span>
2. <span data-ttu-id="c1547-126">Instalační program spustí několik kontrol požadovaných součástí a požadavky služby VMM hello toostop oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c1547-126">Setup runs a few prerequisite checks, and requests permission toostop hello VMM service.</span></span> <span data-ttu-id="c1547-127">Hello služby VMM se restartuje automaticky po dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="c1547-127">hello VMM service will be restarted automatically when setup finishes.</span></span> <span data-ttu-id="c1547-128">Pokud instalujete na clusteru VMM, můžete se výzvami toostop hello Clusterové role.</span><span class="sxs-lookup"><span data-stu-id="c1547-128">If you install on a VMM cluster, you're prompted toostop hello Cluster role.</span></span>
3. <span data-ttu-id="c1547-129">V **Microsoft Update**, že se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update toospecify je volitelný.</span><span class="sxs-lookup"><span data-stu-id="c1547-129">In **Microsoft Update**, you can opt in toospecify that provider updates are installed in accordance with your Microsoft Update policy.</span></span>
4. <span data-ttu-id="c1547-130">V **instalace**, přijmout nebo upravte výchozí umístění instalace hello a klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="c1547-130">In **Installation**, accept or modify hello default installation location, and click **Install**.</span></span>

    ![Umístění instalace](./media/vmm-to-vmm-walkthrough-source-target/provider-location.png)
5. <span data-ttu-id="c1547-132">Po dokončení instalace klikněte na tlačítko **zaregistrovat** tooregister hello server v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="c1547-132">After installation is complete, click **Register** tooregister hello server in hello vault.</span></span>

    ![Umístění instalace](./media/vmm-to-vmm-walkthrough-source-target/provider-register.png)
6. <span data-ttu-id="c1547-134">V **název trezoru**, ověřte název hello hello úložiště, ve které hello bude server zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="c1547-134">In **Vault name**, verify hello name of hello vault in which hello server will be registered.</span></span> <span data-ttu-id="c1547-135">Klikněte na *Další*.</span><span class="sxs-lookup"><span data-stu-id="c1547-135">Click *Next*.</span></span>

    ![Registrace serveru](./media/vmm-to-vmm-walkthrough-source-target/vaultcred.png)
7. <span data-ttu-id="c1547-137">V **připojení k Internetu**, určete, jak hello zprostředkovatel, který běží na serveru VMM hello připojuje tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c1547-137">In **Internet Connection**, specify how hello provider running on hello VMM server connects tooAzure.</span></span>

    ![Nastavení internetu](./media/vmm-to-vmm-walkthrough-source-target/proxydetails.png)

   - <span data-ttu-id="c1547-139">Můžete zadat tohoto zprostředkovatele hello by se měly připojit přímo toohello Internetu, nebo prostřednictvím proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="c1547-139">You can specify that hello provider should connect directly toohello internet, or via a proxy.</span></span>
   - <span data-ttu-id="c1547-140">Zadejte nastavení proxy serveru v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="c1547-140">Specify proxy settings if needed.</span></span>
   - <span data-ttu-id="c1547-141">Pokud používáte proxy server, účet VMM RunAs (DRAProxyAccount) se vytvoří automaticky pomocí hello zadané přihlašovací údaje pro proxy.</span><span class="sxs-lookup"><span data-stu-id="c1547-141">If you use a proxy, a VMM RunAs account (DRAProxyAccount) is created automatically using hello specified proxy credentials.</span></span> <span data-ttu-id="c1547-142">Hello proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit.</span><span class="sxs-lookup"><span data-stu-id="c1547-142">Configure hello proxy server so that this account can authenticate successfully.</span></span> <span data-ttu-id="c1547-143">Hello nastavení účtu spustit jako lze upravit v konzole VMM hello > **nastavení** > **zabezpečení** > **účty spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="c1547-143">hello RunAs account settings can be modified in hello VMM console > **Settings** > **Security** > **Run As Accounts**.</span></span> <span data-ttu-id="c1547-144">Restartujte změny tooupdate služby VMM hello.</span><span class="sxs-lookup"><span data-stu-id="c1547-144">Restart hello VMM service tooupdate changes.</span></span>
8. <span data-ttu-id="c1547-145">V **registrační klíč**, vyberte klíč hello stáhli z Azure Site Recovery a zkopírovali toohello serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="c1547-145">In **Registration Key**, select hello key that you downloaded from Azure Site Recovery and copied toohello VMM server.</span></span>
9. <span data-ttu-id="c1547-146">nastavení šifrování Hello se používá pouze při replikaci virtuálních počítačů Hyper-V v tooAzure cloudy VMM.</span><span class="sxs-lookup"><span data-stu-id="c1547-146">hello encryption setting is only used when you're replicating Hyper-V VMs in VMM clouds tooAzure.</span></span> <span data-ttu-id="c1547-147">Pokud replikujete tooa sekundární lokality nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="c1547-147">If you're replicating tooa secondary site it's not used.</span></span>
10. <span data-ttu-id="c1547-148">V **název serveru**, zadejte popisný název tooidentify hello VMM server v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="c1547-148">In **Server name**, specify a friendly name tooidentify hello VMM server in hello vault.</span></span> <span data-ttu-id="c1547-149">V konfiguraci clusteru zadejte název role clusteru VMM hello.</span><span class="sxs-lookup"><span data-stu-id="c1547-149">In a cluster configuration specify hello VMM cluster role name.</span></span>
11. <span data-ttu-id="c1547-150">V **synchronizovat metadata cloudu**vyberte, jestli chcete, aby toosynchronize metadata pro všechny cloudy na serveru VMM hello s úložištěm hello.</span><span class="sxs-lookup"><span data-stu-id="c1547-150">In **Synchronize cloud metadata**, select whether you want toosynchronize metadata for all clouds on hello VMM server with hello vault.</span></span> <span data-ttu-id="c1547-151">Tuto akci stačí toohappen jednou na každém serveru.</span><span class="sxs-lookup"><span data-stu-id="c1547-151">This action only needs toohappen once on each server.</span></span> <span data-ttu-id="c1547-152">Pokud nechcete, aby toosynchronize všechny cloudy, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech hello cloudu v konzole VMM hello.</span><span class="sxs-lookup"><span data-stu-id="c1547-152">If you don't want toosynchronize all clouds, you can leave this setting unchecked, and synchronize each cloud individually in hello cloud properties in hello VMM console.</span></span>
12. <span data-ttu-id="c1547-153">Klikněte na tlačítko **Další** toocomplete hello procesu.</span><span class="sxs-lookup"><span data-stu-id="c1547-153">Click **Next** toocomplete hello process.</span></span> <span data-ttu-id="c1547-154">Po registraci načte Azure Site Recovery metadata ze serveru VMM hello.</span><span class="sxs-lookup"><span data-stu-id="c1547-154">After registration, metadata from hello VMM server is retrieved by Azure Site Recovery.</span></span> <span data-ttu-id="c1547-155">Hello server se zobrazí na hello **servery VMM** karty v hello **servery** stránky v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="c1547-155">hello server is displayed on hello **VMM Servers** tab on hello **Servers** page in hello vault.</span></span>

    ![Server](./media/vmm-to-vmm-walkthrough-source-target/provider13.png)
13. <span data-ttu-id="c1547-157">Po hello server je k dispozici v konzole Site Recovery hello v **zdroj** > **připravit zdroj** vyberte hello VMM server a vyberte hello cloudu, ve které hello technologie Hyper-V se hostitel nachází.</span><span class="sxs-lookup"><span data-stu-id="c1547-157">After hello server is available in hello Site Recovery console, in **Source** > **Prepare source** select hello VMM server, and select hello cloud in which hello Hyper-V host is located.</span></span> <span data-ttu-id="c1547-158">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1547-158">Then click **OK**.</span></span>

<span data-ttu-id="c1547-159">Hello zprostředkovatele můžete také nainstalovat z příkazového řádku hello:</span><span class="sxs-lookup"><span data-stu-id="c1547-159">You can also install hello provider from hello command line:</span></span>

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-hello-target-environment"></a><span data-ttu-id="c1547-160">Nastavení cílového prostředí hello</span><span class="sxs-lookup"><span data-stu-id="c1547-160">Set up hello target environment</span></span>

<span data-ttu-id="c1547-161">Vyberte hello cílovém serveru VMM a cloud:</span><span class="sxs-lookup"><span data-stu-id="c1547-161">Select hello target VMM server and cloud:</span></span>

1. <span data-ttu-id="c1547-162">Klikněte na tlačítko **připravit infrastrukturu** > **cíl**a vyberte hello cílovém serveru VMM má toouse.</span><span class="sxs-lookup"><span data-stu-id="c1547-162">Click **Prepare infrastructure** > **Target**, and select hello target VMM server you want toouse.</span></span>
2. <span data-ttu-id="c1547-163">Zobrazí se cloudy na serveru hello, jež jsou synchronizovány se Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="c1547-163">Clouds on hello server that are synchronized with Site Recovery will be displayed.</span></span> <span data-ttu-id="c1547-164">Vyberte cílový cloud hello.</span><span class="sxs-lookup"><span data-stu-id="c1547-164">Select hello target cloud.</span></span>

   ![cíl](./media/vmm-to-vmm-walkthrough-source-target/target-vmm.png)



## <a name="next-steps"></a><span data-ttu-id="c1547-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c1547-166">Next steps</span></span>

<span data-ttu-id="c1547-167">Přejděte příliš[krok 7: nakonfigurování mapování sítě](vmm-to-vmm-walkthrough-network-mapping.md).</span><span class="sxs-lookup"><span data-stu-id="c1547-167">Go too[Step 7: Configure network mapping](vmm-to-vmm-walkthrough-network-mapping.md).</span></span>
