---
title: "Nastavit zdroje a cíle pro replikaci technologie Hyper-V do sekundární lokality s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak nastavit zdroje a cíle při replikaci virtuálních počítačů Hyper-V do sekundární lokalita VMM s Azure Site Recovery."
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
ms.openlocfilehash: 07135e9b5e619971a59cc22ec68a0a4e8bcaabe1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-6-set-up-the-replication-source-and-target"></a><span data-ttu-id="d9275-103">Krok 6: Nastavení replikace zdroje a cíle</span><span class="sxs-lookup"><span data-stu-id="d9275-103">Step 6: Set up the replication source and target</span></span>


<span data-ttu-id="d9275-104">Po vytvoření služby zotavení trezoru pro replikaci technologie Hyper-V do sekundární lokalita VMM s [Azure Site Recovery](site-recovery-overview.md), použijte tento článek k nastavení zdrojové a cílové umístění pro replikaci.</span><span class="sxs-lookup"><span data-stu-id="d9275-104">After creating a Recovery Services vault for Hyper-V replication to a secondary VMM site with [Azure Site Recovery](site-recovery-overview.md), use this article to set up the source and target replication locations.</span></span> 

<span data-ttu-id="d9275-105">Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="d9275-105">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="set-up-the-source-environment"></a><span data-ttu-id="d9275-106">Nastavení zdrojového prostředí</span><span class="sxs-lookup"><span data-stu-id="d9275-106">Set up the source environment</span></span>

<span data-ttu-id="d9275-107">Nainstalujte zprostředkovatele Azure Site Recovery na serverech VMM a zjišťovat a zaregistrujte server v trezoru.</span><span class="sxs-lookup"><span data-stu-id="d9275-107">Install the Azure Site Recovery Provider on VMM servers, and discover and register servers in the vault.</span></span>

1. <span data-ttu-id="d9275-108">Klikněte na tlačítko **krok 1: Příprava infrastruktury** > **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="d9275-108">Click **Step 1: Prepare Infrastructure** > **Source**.</span></span>

    ![Nastavení zdroje](./media/vmm-to-vmm-walkthrough-source-target/goals-source.png)
2. <span data-ttu-id="d9275-110">V okně **Připravit zdroj** klikněte na **+ VMM** a přidejte server VMM.</span><span class="sxs-lookup"><span data-stu-id="d9275-110">In **Prepare source**, click **+ VMM** to add a VMM server.</span></span>

    ![Nastavení zdroje](./media/vmm-to-vmm-walkthrough-source-target/set-source1.png)
3. <span data-ttu-id="d9275-112">V **přidat Server**, zkontrolujte, zda **serveru System Center VMM** se zobrazí v **typ serveru** a že VMM server splňuje požadavky [požadavky](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="d9275-112">In **Add Server**, check that **System Center VMM server** appears in **Server type** and that the VMM server meets the [prerequisites](#prerequisites).</span></span>
4. <span data-ttu-id="d9275-113">Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="d9275-113">Download the Azure Site Recovery Provider installation file.</span></span>
5. <span data-ttu-id="d9275-114">Stáhněte si registrační klíč.</span><span class="sxs-lookup"><span data-stu-id="d9275-114">Download the registration key.</span></span> <span data-ttu-id="d9275-115">Budete ho potřebovat, když spustíte instalaci.</span><span class="sxs-lookup"><span data-stu-id="d9275-115">You need this when you run setup.</span></span> <span data-ttu-id="d9275-116">Klíč je platný pět dní od jeho vygenerování.</span><span class="sxs-lookup"><span data-stu-id="d9275-116">The key is valid for five days after you generate it.</span></span>

    ![Nastavení zdroje](./media/vmm-to-vmm-walkthrough-source-target/set-source3.png)
6. <span data-ttu-id="d9275-118">Nainstalujte zprostředkovatele Azure Site Recovery na server VMM.</span><span class="sxs-lookup"><span data-stu-id="d9275-118">Install the Azure Site Recovery Provider on the VMM server.</span></span> <span data-ttu-id="d9275-119">Nemusíte explicitně nic instalovat na hostitelských serverech technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="d9275-119">You don't need to explicitly install anything on Hyper-V host servers.</span></span>


## <a name="install-the-azure-site-recovery-provider"></a><span data-ttu-id="d9275-120">Nainstalujte zprostředkovatele Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="d9275-120">Install the Azure Site Recovery Provider</span></span>

1. <span data-ttu-id="d9275-121">Spusťte instalační soubor na každý server VMM zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="d9275-121">Run the Provider setup file on each VMM server.</span></span> <span data-ttu-id="d9275-122">Pokud VMM je nasazen v clusteru, takto prvním instalaci:</span><span class="sxs-lookup"><span data-stu-id="d9275-122">If VMM is deployed in a cluster, do the following the first time you install:</span></span>
    -  <span data-ttu-id="d9275-123">Nainstalujte poskytovatele na aktivní uzel a dokončením instalace zaregistrujte VMM server v trezoru.</span><span class="sxs-lookup"><span data-stu-id="d9275-123">Install the provider on an active node, and finish the installation to register the VMM server in the vault.</span></span>
    - <span data-ttu-id="d9275-124">Potom nainstalujte zprostředkovatele na ostatních uzlech.</span><span class="sxs-lookup"><span data-stu-id="d9275-124">Then, install the Provider on the other nodes.</span></span> <span data-ttu-id="d9275-125">Všechny uzly clusteru by měl spuštěna stejná verze zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="d9275-125">Cluster nodes should all run the same version of the Provider.</span></span>
2. <span data-ttu-id="d9275-126">Instalační program spustí několik kontrol požadovaných součástí a požádá o oprávnění k zastavení služby VMM.</span><span class="sxs-lookup"><span data-stu-id="d9275-126">Setup runs a few prerequisite checks, and requests permission to stop the VMM service.</span></span> <span data-ttu-id="d9275-127">Služba VMM se restartuje automaticky po dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="d9275-127">The VMM service will be restarted automatically when setup finishes.</span></span> <span data-ttu-id="d9275-128">Pokud instalujete na clusteru VMM, se zobrazí výzva k zastavení Cluster role.</span><span class="sxs-lookup"><span data-stu-id="d9275-128">If you install on a VMM cluster, you're prompted to stop the Cluster role.</span></span>
3. <span data-ttu-id="d9275-129">V **Microsoft Update**, můžete vyjádřit výslovný souhlas k určení, že se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="d9275-129">In **Microsoft Update**, you can opt in to specify that provider updates are installed in accordance with your Microsoft Update policy.</span></span>
4. <span data-ttu-id="d9275-130">V **instalace**, přijmout nebo upravte výchozí umístění instalace a klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="d9275-130">In **Installation**, accept or modify the default installation location, and click **Install**.</span></span>

    ![Umístění instalace](./media/vmm-to-vmm-walkthrough-source-target/provider-location.png)
5. <span data-ttu-id="d9275-132">Po dokončení instalace klikněte na tlačítko **zaregistrovat** zaregistrujte server v trezoru.</span><span class="sxs-lookup"><span data-stu-id="d9275-132">After installation is complete, click **Register** to register the server in the vault.</span></span>

    ![Umístění instalace](./media/vmm-to-vmm-walkthrough-source-target/provider-register.png)
6. <span data-ttu-id="d9275-134">V poli **Název trezoru** ověřte název trezoru, ve kterém bude server zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="d9275-134">In **Vault name**, verify the name of the vault in which the server will be registered.</span></span> <span data-ttu-id="d9275-135">Klikněte na *Další*.</span><span class="sxs-lookup"><span data-stu-id="d9275-135">Click *Next*.</span></span>

    ![Registrace serveru](./media/vmm-to-vmm-walkthrough-source-target/vaultcred.png)
7. <span data-ttu-id="d9275-137">V **připojení k Internetu**, určete, jak se zprostředkovatel, který běží na serveru VMM připojuje k Azure.</span><span class="sxs-lookup"><span data-stu-id="d9275-137">In **Internet Connection**, specify how the provider running on the VMM server connects to Azure.</span></span>

    ![Nastavení internetu](./media/vmm-to-vmm-walkthrough-source-target/proxydetails.png)

   - <span data-ttu-id="d9275-139">Můžete určit, zda zprostředkovatel by měl připojit přímo k Internetu, nebo prostřednictvím proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="d9275-139">You can specify that the provider should connect directly to the internet, or via a proxy.</span></span>
   - <span data-ttu-id="d9275-140">Zadejte nastavení proxy serveru v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="d9275-140">Specify proxy settings if needed.</span></span>
   - <span data-ttu-id="d9275-141">Pokud používáte proxy server, vytvoří se účet VMM RunAs (DRAProxyAccount) automaticky pomocí přihlašovacích údajů, zadaný proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="d9275-141">If you use a proxy, a VMM RunAs account (DRAProxyAccount) is created automatically using the specified proxy credentials.</span></span> <span data-ttu-id="d9275-142">Proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit.</span><span class="sxs-lookup"><span data-stu-id="d9275-142">Configure the proxy server so that this account can authenticate successfully.</span></span> <span data-ttu-id="d9275-143">V konzole VMM lze upravovat nastavení účtu RunAs > **nastavení** > **zabezpečení** > **účty spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="d9275-143">The RunAs account settings can be modified in the VMM console > **Settings** > **Security** > **Run As Accounts**.</span></span> <span data-ttu-id="d9275-144">Restartujte službu VMM k aktualizaci změn.</span><span class="sxs-lookup"><span data-stu-id="d9275-144">Restart the VMM service to update changes.</span></span>
8. <span data-ttu-id="d9275-145">V části **Registrační klíč** vyberte klíč, který jste si stáhli z Azure Site Recovery a zkopírovali na server VMM.</span><span class="sxs-lookup"><span data-stu-id="d9275-145">In **Registration Key**, select the key that you downloaded from Azure Site Recovery and copied to the VMM server.</span></span>
9. <span data-ttu-id="d9275-146">Nastavení šifrování se používá pouze při replikaci virtuálních počítačů Hyper-V v cloudech VMM do Azure.</span><span class="sxs-lookup"><span data-stu-id="d9275-146">The encryption setting is only used when you're replicating Hyper-V VMs in VMM clouds to Azure.</span></span> <span data-ttu-id="d9275-147">Pokud provádíte replikaci do sekundární lokality, tak se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="d9275-147">If you're replicating to a secondary site it's not used.</span></span>
10. <span data-ttu-id="d9275-148">Do pole **Název serveru** zadejte popisný název, který bude identifikovat server VMM v trezoru.</span><span class="sxs-lookup"><span data-stu-id="d9275-148">In **Server name**, specify a friendly name to identify the VMM server in the vault.</span></span> <span data-ttu-id="d9275-149">V konfiguraci clusteru zadejte název role clusteru VMM.</span><span class="sxs-lookup"><span data-stu-id="d9275-149">In a cluster configuration specify the VMM cluster role name.</span></span>
11. <span data-ttu-id="d9275-150">V **synchronizovat metadata cloudu**vyberte, jestli chcete synchronizovat metadata pro všechny cloudy na serveru VMM v trezoru.</span><span class="sxs-lookup"><span data-stu-id="d9275-150">In **Synchronize cloud metadata**, select whether you want to synchronize metadata for all clouds on the VMM server with the vault.</span></span> <span data-ttu-id="d9275-151">Tuto akci stačí na každém serveru provést pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="d9275-151">This action only needs to happen once on each server.</span></span> <span data-ttu-id="d9275-152">Pokud nechcete, aby k synchronizaci veškerých cloudů, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech cloudu v konzole nástroje VMM.</span><span class="sxs-lookup"><span data-stu-id="d9275-152">If you don't want to synchronize all clouds, you can leave this setting unchecked, and synchronize each cloud individually in the cloud properties in the VMM console.</span></span>
12. <span data-ttu-id="d9275-153">Dokončete proces kliknutím na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d9275-153">Click **Next** to complete the process.</span></span> <span data-ttu-id="d9275-154">Po registraci načte Azure Site Recovery metadata ze serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="d9275-154">After registration, metadata from the VMM server is retrieved by Azure Site Recovery.</span></span> <span data-ttu-id="d9275-155">Server se zobrazí na kartě **Servery VMM** stránky **Servery** v trezoru.</span><span class="sxs-lookup"><span data-stu-id="d9275-155">The server is displayed on the **VMM Servers** tab on the **Servers** page in the vault.</span></span>

    ![Server](./media/vmm-to-vmm-walkthrough-source-target/provider13.png)
13. <span data-ttu-id="d9275-157">Jakmile je k dispozici v konzole Site Recovery na server v **zdroj** > **připravit zdroj** vyberte VMM server a vyberte cloud, ve kterém se nachází hostiteli Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="d9275-157">After the server is available in the Site Recovery console, in **Source** > **Prepare source** select the VMM server, and select the cloud in which the Hyper-V host is located.</span></span> <span data-ttu-id="d9275-158">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="d9275-158">Then click **OK**.</span></span>

<span data-ttu-id="d9275-159">Zprostředkovatel můžete také nainstalovat z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="d9275-159">You can also install the provider from the command line:</span></span>

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-the-target-environment"></a><span data-ttu-id="d9275-160">Nastavení cílového prostředí</span><span class="sxs-lookup"><span data-stu-id="d9275-160">Set up the target environment</span></span>

<span data-ttu-id="d9275-161">Vyberte cílový server VMM a cloud:</span><span class="sxs-lookup"><span data-stu-id="d9275-161">Select the target VMM server and cloud:</span></span>

1. <span data-ttu-id="d9275-162">Klikněte na tlačítko **připravit infrastrukturu** > **cíl**a vyberte cílovém serveru VMM, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="d9275-162">Click **Prepare infrastructure** > **Target**, and select the target VMM server you want to use.</span></span>
2. <span data-ttu-id="d9275-163">Zobrazí se cloudy na serveru, které jsou synchronizovány s Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="d9275-163">Clouds on the server that are synchronized with Site Recovery will be displayed.</span></span> <span data-ttu-id="d9275-164">Vyberte cílový cloud.</span><span class="sxs-lookup"><span data-stu-id="d9275-164">Select the target cloud.</span></span>

   ![cíl](./media/vmm-to-vmm-walkthrough-source-target/target-vmm.png)



## <a name="next-steps"></a><span data-ttu-id="d9275-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9275-166">Next steps</span></span>

<span data-ttu-id="d9275-167">Přejděte na [krok 7: nakonfigurování mapování sítě](vmm-to-vmm-walkthrough-network-mapping.md).</span><span class="sxs-lookup"><span data-stu-id="d9275-167">Go to [Step 7: Configure network mapping](vmm-to-vmm-walkthrough-network-mapping.md).</span></span>
