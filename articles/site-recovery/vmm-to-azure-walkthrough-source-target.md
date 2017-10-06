---
title: "aaaSet hello zdroj a cíl pro tooAzure replikace technologie Hyper-V (s nástrojem System Center VMM) s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky tooset hello nastavení zdroj a cíl pro replikaci virtuálních počítačů technologie Hyper-V v úložišti tooAzure cloudy VMM s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5edb6d87-25a5-40fe-b6f1-ddf7b55a6b31
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/25/2017
ms.author: raynew
ms.openlocfilehash: 3f8c5386cb64527c775aef636980bac098ee9905
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-hyper-v-with-vmm-replication-tooazure"></a><span data-ttu-id="e9bf8-103">Krok 8: Nastavení hello zdroj a cíl pro tooAzure replikace technologie Hyper-V (s nástrojem VMM)</span><span class="sxs-lookup"><span data-stu-id="e9bf8-103">Step 8: Set up hello source and target for Hyper-V (with VMM) replication tooAzure</span></span>

<span data-ttu-id="e9bf8-104">Po [vytvoření trezoru](vmm-to-azure-walkthrough-create-vault.md) a určení, co jste tooreplicate, použijte tento článek tooconfigure zdroj a cíl nastavení při replikaci virtuálních počítačů Hyper-V místní v System Center Virtual Machine Manager (VMM) tooAzure cloudy, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-104">After [creating a vault](vmm-to-azure-walkthrough-create-vault.md) and specifying what you want tooreplicate, use this article tooconfigure source and target settings when replicating on-premises Hyper-V virtual machines in System Center Virtual Machine Manager (VMM) clouds tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="e9bf8-105">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="e9bf8-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-hello-source-environment"></a><span data-ttu-id="e9bf8-106">Nastavit hello zdrojové prostředí</span><span class="sxs-lookup"><span data-stu-id="e9bf8-106">Set up hello source environment</span></span>

<span data-ttu-id="e9bf8-107">S1.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-107">S1.</span></span> <span data-ttu-id="e9bf8-108">Klikněte na **Připravit infrastrukturu** > **Zdroj**.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-108">Click **Prepare Infrastructure** > **Source**.</span></span>

    ![Set up source](./media/vmm-to-azure-walkthrough-source-target/set-source1.png)

2. <span data-ttu-id="e9bf8-109">V **připravit zdroj**, klikněte na tlačítko **+ VMM** tooadd serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-109">In **Prepare source**, click **+ VMM** tooadd a VMM server.</span></span>

    ![Nastavení zdroje](./media/vmm-to-azure-walkthrough-source-target/set-source2.png)

3. <span data-ttu-id="e9bf8-111">V **přidat Server**, zkontrolujte, zda **serveru System Center VMM** se zobrazí v **typ serveru** a tímto serverem VMM hello splňuje hello [požadavky a adresy URL požadavky na](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="e9bf8-111">In **Add Server**, check that **System Center VMM server** appears in **Server type** and that hello VMM server meets hello [prerequisites and URL requirements](#prerequisites).</span></span>
4. <span data-ttu-id="e9bf8-112">Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery hello.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-112">Download hello Azure Site Recovery Provider installation file.</span></span>
5. <span data-ttu-id="e9bf8-113">Stáhněte si registrační klíč hello.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-113">Download hello registration key.</span></span> <span data-ttu-id="e9bf8-114">Budete ho potřebovat, když spustíte instalaci.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-114">You need this when you run setup.</span></span> <span data-ttu-id="e9bf8-115">Hello klíč je platný pět dní od jeho vygenerování.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-115">hello key is valid for five days after you generate it.</span></span>

    ![Nastavení zdroje](./media/vmm-to-azure-walkthrough-source-target/set-source3.png)

## <a name="install-hello-provider-on-hello-vmm-server"></a><span data-ttu-id="e9bf8-117">Nainstalujte hello zprostředkovatele na serveru VMM hello</span><span class="sxs-lookup"><span data-stu-id="e9bf8-117">Install hello Provider on hello VMM server</span></span>

1. <span data-ttu-id="e9bf8-118">Spusťte instalační soubor zprostředkovatele hello na serveru VMM hello.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-118">Run hello Provider setup file on hello VMM server.</span></span>
2. <span data-ttu-id="e9bf8-119">V rámci **Microsoft Update** můžete vyjádřit výslovný souhlas s aktualizacemi, aby se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-119">In **Microsoft Update**, you can opt in for updates so that Provider updates are installed in accordance with your Microsoft Update policy.</span></span>
3. <span data-ttu-id="e9bf8-120">V **instalace**, přijmout nebo úpravě hello výchozí umístění instalace zprostředkovatele a klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-120">In **Installation**, accept or modify hello default Provider installation location and click **Install**.</span></span>

    ![Umístění instalace](./media/vmm-to-azure-walkthrough-source-target/provider2.png)
4. <span data-ttu-id="e9bf8-122">Po dokončení instalace klikněte na tlačítko **zaregistrovat** tooregister hello serveru VMM v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-122">When installation finishes, click **Register** tooregister hello VMM server in hello vault.</span></span>
5. <span data-ttu-id="e9bf8-123">V hello **nastavení trezoru** klikněte na tlačítko **Procházet** tooselect hello soubor klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-123">In hello **Vault Settings** page, click **Browse** tooselect hello vault key file.</span></span> <span data-ttu-id="e9bf8-124">Zadejte předplatné Azure Site Recovery hello a název trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-124">Specify hello Azure Site Recovery subscription and hello vault name.</span></span>

    ![Registrace serveru](./media/vmm-to-azure-walkthrough-source-target/provider10.png)
6. <span data-ttu-id="e9bf8-126">V **připojení k Internetu**, zadejte, jak hello hello zprostředkovatel, který běží na serveru VMM hello připojí tooSite obnovení přes internet.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-126">In **Internet Connection**, specify how hello Provider running on hello VMM server will connect tooSite Recovery over hello internet.</span></span>

   * <span data-ttu-id="e9bf8-127">Pokud chcete hello zprostředkovatele tooconnect přímo, vyberte **připojit přímo bez proxy tooAzure Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-127">If you want hello Provider tooconnect directly, select **Connect directly tooAzure Site Recovery without a proxy**.</span></span>
   * <span data-ttu-id="e9bf8-128">Pokud váš stávající proxy server vyžaduje ověření, nebo chcete toouse vlastní proxy server, vyberte **připojit tooAzure Site Recovery pomocí proxy serveru**.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-128">If your existing proxy requires authentication, or you want toouse a custom proxy, select **Connect tooAzure Site Recovery using a proxy server**.</span></span>
   * <span data-ttu-id="e9bf8-129">Pokud používáte vlastní proxy server, zadejte hello adresu, port a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-129">If you use a custom proxy, specify hello address, port, and credentials.</span></span>
   * <span data-ttu-id="e9bf8-130">Pokud používáte proxy server, které by už mít povolené hello adresy URL popisované v [požadavky](#on-premises-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="e9bf8-130">If you're using a proxy, you should have already allowed hello URLs described in [prerequisites](#on-premises-prerequisites).</span></span>
   * <span data-ttu-id="e9bf8-131">Pokud používáte vlastní proxy server, účet VMM RunAs (DRAProxyAccount) se vytvoří automaticky pomocí hello zadané přihlašovací údaje pro proxy.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-131">If you use a custom proxy, a VMM RunAs account (DRAProxyAccount) will be created automatically using hello specified proxy credentials.</span></span> <span data-ttu-id="e9bf8-132">Hello proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-132">Configure hello proxy server so that this account can authenticate successfully.</span></span> <span data-ttu-id="e9bf8-133">nastavení účtu VMM RunAs Hello lze upravit v konzole VMM hello.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-133">hello VMM RunAs account settings can be modified in hello VMM console.</span></span> <span data-ttu-id="e9bf8-134">V **nastavení**, rozbalte položku **zabezpečení** > **účty spustit jako**a potom upravte hello heslo pro DRAProxyAccount.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-134">In **Settings**, expand **Security** > **Run As Accounts**, and then modify hello password for DRAProxyAccount.</span></span> <span data-ttu-id="e9bf8-135">Služba VMM hello toorestart budete potřebovat, aby toto nastavení projevilo.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-135">You’ll need toorestart hello VMM service so that this setting takes effect.</span></span>

     ![Internet](./media/vmm-to-azure-walkthrough-source-target/provider13.png)
7. <span data-ttu-id="e9bf8-137">Přijměte nebo změňte umístění certifikátu SSL, který je automaticky generován pro šifrování dat hello.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-137">Accept or modify hello location of an SSL certificate that’s automatically generated for data encryption.</span></span> <span data-ttu-id="e9bf8-138">Tento certifikát se používá, pokud povolíte šifrování dat pro cloud chráněný platformou Azure na portálu Azure Site Recovery hello.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-138">This certificate is used if you enable data encryption for a cloud protected by Azure in hello Azure Site Recovery portal.</span></span> <span data-ttu-id="e9bf8-139">Uchovávejte tento certifikát v bezpečí.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-139">Keep this certificate safe.</span></span> <span data-ttu-id="e9bf8-140">Když spustíte převzetí služeb při selhání tooAzure budete je potřebovat toodecrypt, pokud je povolené šifrování dat.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-140">When you run a failover tooAzure you’ll need it toodecrypt, if data encryption is enabled.</span></span>
8. <span data-ttu-id="e9bf8-141">V **název serveru**, zadejte popisný název tooidentify hello VMM server v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-141">In **Server name**, specify a friendly name tooidentify hello VMM server in hello vault.</span></span> <span data-ttu-id="e9bf8-142">V konfiguraci clusteru zadejte název role clusteru VMM hello.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-142">In a cluster configuration, specify hello VMM cluster role name.</span></span>
9. <span data-ttu-id="e9bf8-143">Povolit **synchronizovat metadata cloudu**, pokud chcete toosynchronize metadata pro všechny cloudy na serveru VMM hello s úložištěm hello.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-143">Enable **Sync cloud metadata**, if you want toosynchronize metadata for all clouds on hello VMM server with hello vault.</span></span> <span data-ttu-id="e9bf8-144">Tuto akci stačí toohappen jednou na každém serveru.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-144">This action only needs toohappen once on each server.</span></span> <span data-ttu-id="e9bf8-145">Pokud nechcete, aby toosynchronize všechny cloudy, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech hello cloudu v konzole VMM hello.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-145">If you don't want toosynchronize all clouds, you can leave this setting unchecked and synchronize each cloud individually in hello cloud properties in hello VMM console.</span></span> <span data-ttu-id="e9bf8-146">Klikněte na tlačítko **zaregistrovat** toocomplete hello procesu.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-146">Click **Register** toocomplete hello process.</span></span>

    ![Registrace serveru](./media/vmm-to-azure-walkthrough-source-target/provider16.png)
10. <span data-ttu-id="e9bf8-148">Spustí se registrace.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-148">Registration starts.</span></span> <span data-ttu-id="e9bf8-149">Po dokončení registrace hello serveru zobrazuje v **infrastruktura Site Recovery** > **servery VMM**.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-149">After registration finishes, hello server is displayed in **Site Recovery Infrastructure** > **VMM Servers**.</span></span>


## <a name="install-hello-azure-recovery-services-agent-on-hyper-v-hosts"></a><span data-ttu-id="e9bf8-150">Nainstalujte agenta služeb zotavení Azure hello na hostitele Hyper-V</span><span class="sxs-lookup"><span data-stu-id="e9bf8-150">Install hello Azure Recovery Services agent on Hyper-V hosts</span></span>

1. <span data-ttu-id="e9bf8-151">Poté, co jste nastavili hello poskytovatele, musíte pro agenta služeb zotavení Azure hello toodownload hello instalační soubor.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-151">After you've set up hello Provider, you need toodownload hello installation file for hello Azure Recovery Services agent.</span></span> <span data-ttu-id="e9bf8-152">Spusťte instalační program na každém serveru technologie Hyper-V v cloudu VMM hello.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-152">Run setup on each Hyper-V server in hello VMM cloud.</span></span>

    ![Lokality Hyper-V](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent1.png)
2. <span data-ttu-id="e9bf8-154">Na stránce **Kontrola předpokladů** klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-154">In **Prerequisites Check**, click **Next**.</span></span> <span data-ttu-id="e9bf8-155">Automaticky se nainstalují všechny chybějící požadované součásti.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-155">Any missing prerequisites will be automatically installed.</span></span>

    ![Požadavky agenta Služeb zotavení](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent2.png)
3. <span data-ttu-id="e9bf8-157">V **nastavení instalace**přijměte nebo změňte umístění instalace hello a umístění mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-157">In **Installation Settings**, accept or modify hello installation location, and hello cache location.</span></span> <span data-ttu-id="e9bf8-158">Hello mezipaměti můžete nakonfigurovat na jednotce, která má minimálně 5 GB dostupného úložiště, ale doporučujeme mezipaměti jednotku s 600 GB nebo více volného místa.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-158">You can configure hello cache on a drive that has at least 5 GB of storage available but we recommend a cache drive with 600 GB or more of free space.</span></span> <span data-ttu-id="e9bf8-159">Pak klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-159">Then click **Install**.</span></span>
4. <span data-ttu-id="e9bf8-160">Po dokončení instalace klikněte na tlačítko **Zavřít** toofinish.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-160">After installation is complete, click **Close** toofinish.</span></span>

    ![Registrace agenta MARS](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent3.png)

### <a name="command-line-installation"></a><span data-ttu-id="e9bf8-162">Instalace pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e9bf8-162">Command line installation</span></span>
<span data-ttu-id="e9bf8-163">Hello agenta služeb zotavení Microsoft Azure si můžete nainstalovat z příkazového řádku pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e9bf8-163">You can install hello Microsoft Azure Recovery Services Agent from command line using hello following command:</span></span>

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-toosite-recovery-from-hyper-v-hosts"></a><span data-ttu-id="e9bf8-164">Nastavení Internetu proxy přístup tooSite obnovení z hostitelů Hyper-V</span><span class="sxs-lookup"><span data-stu-id="e9bf8-164">Set up internet proxy access tooSite Recovery from Hyper-V hosts</span></span>

<span data-ttu-id="e9bf8-165">agent služeb zotavení Hello spuštěné v hostitelích technologie Hyper-V potřebuje tooAzure přístup k Internetu pro replikaci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-165">hello Recovery Services agent running on Hyper-V hosts needs internet access tooAzure for VM replication.</span></span> <span data-ttu-id="e9bf8-166">Pokud získáváte přístup k hello Internetu prostřednictvím proxy serveru, nastavte ho takto:</span><span class="sxs-lookup"><span data-stu-id="e9bf8-166">If you're accessing hello internet through a proxy, set it up as follows:</span></span>

1. <span data-ttu-id="e9bf8-167">Otevřete hello Microsoft Azure Backup konzoly MMC modul snap-in na hostiteli Hyper-V hello.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-167">Open hello Microsoft Azure Backup MMC snap-in on hello Hyper-V host.</span></span> <span data-ttu-id="e9bf8-168">Ve výchozím nastavení je k dispozici na ploše hello nebo v C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-168">By default, a shortcut for Microsoft Azure Backup is available on hello desktop or in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span></span>
2. <span data-ttu-id="e9bf8-169">V modulu snap-in hello, klikněte na **změnit vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-169">In hello snap-in, click **Change Properties**.</span></span>
3. <span data-ttu-id="e9bf8-170">Na hello **konfiguraci proxy serveru** zadejte informace o proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-170">On hello **Proxy Configuration** tab, specify proxy server information.</span></span>

    ![Registrace agenta MARS](./media/vmm-to-azure-walkthrough-source-target/mars-proxy.png)
4. <span data-ttu-id="e9bf8-172">Zkontrolujte, že agent hello přístup hello adresy URL popisované v hello [požadavky](#on-premises-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="e9bf8-172">Check that hello agent can reach hello URLs described in hello [prerequisites](#on-premises-prerequisites).</span></span>

## <a name="set-up-hello-target-environment"></a><span data-ttu-id="e9bf8-173">Nastavení cílového prostředí hello</span><span class="sxs-lookup"><span data-stu-id="e9bf8-173">Set up hello target environment</span></span>
<span data-ttu-id="e9bf8-174">Zadejte toobe účet úložiště Azure hello používá pro replikaci a hello toowhich síť Azure virtuální počítače Azure připojí po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-174">Specify hello Azure storage account toobe used for replication, and hello Azure network toowhich Azure VMs will connect after failover.</span></span>

1. <span data-ttu-id="e9bf8-175">Klikněte na tlačítko **připravit infrastrukturu** > **cíl**, vyberte předplatné hello a hello skupinu prostředků, kam chcete hello toocreate při selhání virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-175">Click **Prepare infrastructure** > **Target**, select hello subscription and hello resource group where you want toocreate hello failed over virtual machines.</span></span> <span data-ttu-id="e9bf8-176">Vyberte model nasazení hello, který chcete toouse v Azure (klasický nebo prostředek management) pro hello při selhání virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-176">Choose hello deployment model that you want toouse in Azure (classic or resource management) for hello failed over virtual machines.</span></span>

    ![Úložiště](./media/vmm-to-azure-walkthrough-source-target/enablerep3.png)

2. <span data-ttu-id="e9bf8-178">Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-178">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

    ![Úložiště](./media/vmm-to-azure-walkthrough-source-target/compatible-storage.png)

3. <span data-ttu-id="e9bf8-180">Pokud jste ještě nevytvořili účet úložiště, a chcete toocreate jeden pomocí Resource Manager, klikněte na tlačítko **+ účet úložiště** toodo přímo tady.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-180">If you haven't created a storage account, and you want toocreate one using Resource Manager, click **+Storage account** toodo that inline.</span></span>  <span data-ttu-id="e9bf8-181">Na hello **vytvořit účet úložiště** okně zadejte název účtu, typ, předplatné a umístění.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-181">On hello **Create storage account** blade specify an account name, type, subscription, and location.</span></span> <span data-ttu-id="e9bf8-182">Hello účet by měl být v hello stejné umístění jako hello trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-182">hello account should be in hello same location as hello Recovery Services vault.</span></span>

   ![Úložiště](./media/vmm-to-azure-walkthrough-source-target/gs-createstorage.png)


   * <span data-ttu-id="e9bf8-184">Pokud chcete účet úložiště pomocí klasického modelu hello toocreate, udělat na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-184">If you want toocreate a storage account using hello classic model, do that in hello Azure portal.</span></span> [<span data-ttu-id="e9bf8-185">Další informace</span><span class="sxs-lookup"><span data-stu-id="e9bf8-185">Learn more</span></span>](../storage/common/storage-create-storage-account.md)
   * <span data-ttu-id="e9bf8-186">Pokud pro replikovaná data používáte účet úložiště premium, nastavte na účet další standardní úložiště toostore protokoly replikace, které zaznamenání dat tooon místní probíhající změny.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-186">If you’re using a premium storage account for replicated data, set up an additional standard storage account, toostore replication logs that capture ongoing changes tooon-premises data.</span></span>
5. <span data-ttu-id="e9bf8-187">Pokud jste ještě nevytvořili síť Azure a chcete toocreate jeden pomocí Resource Manager, klikněte na tlačítko **+ síť** toodo přímo tady.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-187">If you haven't created an Azure network, and you want toocreate one using Resource Manager, click **+Network** toodo that inline.</span></span> <span data-ttu-id="e9bf8-188">Na hello **vytvořit virtuální síť** okně zadejte název sítě, rozsah adres, podrobnosti o podsíti, předplatné a umístění.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-188">On hello **Create virtual network** blade specify a network name, address range, subnet details, subscription, and location.</span></span> <span data-ttu-id="e9bf8-189">Hello síť musí být ve hello stejné umístění jako hello trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-189">hello network should be in hello same location as hello Recovery Services vault.</span></span>

   ![Síť](./media/vmm-to-azure-walkthrough-source-target/gs-createnetwork.png)

   <span data-ttu-id="e9bf8-191">Pokud chcete toocreate síť pomocí klasického modelu hello, udělat na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e9bf8-191">If you want toocreate a network using hello classic model, do that in hello Azure portal.</span></span> <span data-ttu-id="e9bf8-192">[Další informace](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="e9bf8-192">[Learn more](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).</span></span>





## <a name="next-steps"></a><span data-ttu-id="e9bf8-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e9bf8-193">Next steps</span></span>

<span data-ttu-id="e9bf8-194">Přejděte příliš[krok 9: nakonfigurování mapování sítě](vmm-to-azure-walkthrough-network-mapping.md)</span><span class="sxs-lookup"><span data-stu-id="e9bf8-194">Go too[Step 9: Configure network mapping](vmm-to-azure-walkthrough-network-mapping.md)</span></span>
