---
title: "Nastavit zdroje a cíle pro replikaci technologie Hyper-V do Azure (s nástrojem System Center VMM) s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky nastavit zdroj a cíl nastavení pro replikaci virtuálních počítačů technologie Hyper-V v cloudech VMM do úložiště Azure s Azure Site Recovery"
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
ms.openlocfilehash: c72f839d0a1288dccb7deb3e44fc2b20d64818f0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-8-set-up-the-source-and-target-for-hyper-v-with-vmm-replication-to-azure"></a><span data-ttu-id="9c8c9-103">Krok 8: Nastavte zdroj a cíl pro replikaci technologie Hyper-V (s nástrojem VMM) do Azure.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-103">Step 8: Set up the source and target for Hyper-V (with VMM) replication to Azure</span></span>

<span data-ttu-id="9c8c9-104">Po [vytvoření trezoru](vmm-to-azure-walkthrough-create-vault.md) a určení, co chcete replikovat, použijte tento článek ke konfiguraci nastavení zdrojové a cílové při replikaci místní virtuální počítače Hyper-V v cloudech System Center Virtual Machine Manager (VMM) do Azure pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-104">After [creating a vault](vmm-to-azure-walkthrough-create-vault.md) and specifying what you want to replicate, use this article to configure source and target settings when replicating on-premises Hyper-V virtual machines in System Center Virtual Machine Manager (VMM) clouds to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="9c8c9-105">POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="9c8c9-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-the-source-environment"></a><span data-ttu-id="9c8c9-106">Nastavení zdrojového prostředí</span><span class="sxs-lookup"><span data-stu-id="9c8c9-106">Set up the source environment</span></span>

<span data-ttu-id="9c8c9-107">S1.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-107">S1.</span></span> <span data-ttu-id="9c8c9-108">Klikněte na **Připravit infrastrukturu** > **Zdroj**.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-108">Click **Prepare Infrastructure** > **Source**.</span></span>

    ![Set up source](./media/vmm-to-azure-walkthrough-source-target/set-source1.png)

2. <span data-ttu-id="9c8c9-109">V okně **Připravit zdroj** klikněte na **+ VMM** a přidejte server VMM.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-109">In **Prepare source**, click **+ VMM** to add a VMM server.</span></span>

    ![Nastavení zdroje](./media/vmm-to-azure-walkthrough-source-target/set-source2.png)

3. <span data-ttu-id="9c8c9-111">V okně **Přidat server** zkontrolujte, že se v části **Typ serveru** zobrazuje **server System Center VMM** a že server VMM splňuje [obecné požadavky a požadavky na adresu URL](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="9c8c9-111">In **Add Server**, check that **System Center VMM server** appears in **Server type** and that the VMM server meets the [prerequisites and URL requirements](#prerequisites).</span></span>
4. <span data-ttu-id="9c8c9-112">Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-112">Download the Azure Site Recovery Provider installation file.</span></span>
5. <span data-ttu-id="9c8c9-113">Stáhněte si registrační klíč.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-113">Download the registration key.</span></span> <span data-ttu-id="9c8c9-114">Budete ho potřebovat, když spustíte instalaci.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-114">You need this when you run setup.</span></span> <span data-ttu-id="9c8c9-115">Klíč je platný pět dní od jeho vygenerování.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-115">The key is valid for five days after you generate it.</span></span>

    ![Nastavení zdroje](./media/vmm-to-azure-walkthrough-source-target/set-source3.png)

## <a name="install-the-provider-on-the-vmm-server"></a><span data-ttu-id="9c8c9-117">Instalace zprostředkovatele na server VMM</span><span class="sxs-lookup"><span data-stu-id="9c8c9-117">Install the Provider on the VMM server</span></span>

1. <span data-ttu-id="9c8c9-118">Na serveru VMM spusťte instalační soubor zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-118">Run the Provider setup file on the VMM server.</span></span>
2. <span data-ttu-id="9c8c9-119">V rámci **Microsoft Update** můžete vyjádřit výslovný souhlas s aktualizacemi, aby se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-119">In **Microsoft Update**, you can opt in for updates so that Provider updates are installed in accordance with your Microsoft Update policy.</span></span>
3. <span data-ttu-id="9c8c9-120">V okně **Instalace** přijměte nebo změňte výchozí umístění instalace zprostředkovatele a klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-120">In **Installation**, accept or modify the default Provider installation location and click **Install**.</span></span>

    ![Umístění instalace](./media/vmm-to-azure-walkthrough-source-target/provider2.png)
4. <span data-ttu-id="9c8c9-122">Po dokončení instalace klikněte na **Zaregistrovat**, aby se server VMM zaregistroval v trezoru.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-122">When installation finishes, click **Register** to register the VMM server in the vault.</span></span>
5. <span data-ttu-id="9c8c9-123">Na stránce **Nastavení trezoru** klikněte na **Procházet** a vyberte soubor klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-123">In the **Vault Settings** page, click **Browse** to select the vault key file.</span></span> <span data-ttu-id="9c8c9-124">Určuje předplatné Azure Site Recovery a název trezoru.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-124">Specify the Azure Site Recovery subscription and the vault name.</span></span>

    ![Registrace serveru](./media/vmm-to-azure-walkthrough-source-target/provider10.png)
6. <span data-ttu-id="9c8c9-126">V části **Připojení k Internetu** určete, jak se zprostředkovatel, který běží na serveru VMM, připojí k Site Recovery přes internet.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-126">In **Internet Connection**, specify how the Provider running on the VMM server will connect to Site Recovery over the internet.</span></span>

   * <span data-ttu-id="9c8c9-127">Pokud chcete, aby se zprostředkovatel připojil přímo, vyberte **Připojit k Azure Site Recovery přímo bez proxy serveru**.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-127">If you want the Provider to connect directly, select **Connect directly to Azure Site Recovery without a proxy**.</span></span>
   * <span data-ttu-id="9c8c9-128">Pokud váš existující proxy server vyžaduje ověření nebo chcete použít vlastní proxy server, vyberte **Připojit k Azure Site Recovery pomocí proxy serveru**.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-128">If your existing proxy requires authentication, or you want to use a custom proxy, select **Connect to Azure Site Recovery using a proxy server**.</span></span>
   * <span data-ttu-id="9c8c9-129">Pokud budete používat vlastní proxy server, musíte zadat adresu, port a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-129">If you use a custom proxy, specify the address, port, and credentials.</span></span>
   * <span data-ttu-id="9c8c9-130">Pokud používáte proxy server, měli byste už mít povolené adresy URL popisované v [požadavcích](#on-premises-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="9c8c9-130">If you're using a proxy, you should have already allowed the URLs described in [prerequisites](#on-premises-prerequisites).</span></span>
   * <span data-ttu-id="9c8c9-131">Pokud používáte vlastní proxy server, vytvoří se automaticky účet VMM RunAs (DRAProxyAccount) pomocí zadaných přihlašovacích údajů proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-131">If you use a custom proxy, a VMM RunAs account (DRAProxyAccount) will be created automatically using the specified proxy credentials.</span></span> <span data-ttu-id="9c8c9-132">Proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-132">Configure the proxy server so that this account can authenticate successfully.</span></span> <span data-ttu-id="9c8c9-133">Nastavení účtu VMM RunAs můžete upravovat v konzole VMM.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-133">The VMM RunAs account settings can be modified in the VMM console.</span></span> <span data-ttu-id="9c8c9-134">V části **Nastavení** rozbalte položku **Zabezpečení** > **Účty Spustit jako** a potom změňte heslo pro DRAProxyAccount.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-134">In **Settings**, expand **Security** > **Run As Accounts**, and then modify the password for DRAProxyAccount.</span></span> <span data-ttu-id="9c8c9-135">Budete muset službu VMM restartovat, aby se toto nastavení projevilo.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-135">You’ll need to restart the VMM service so that this setting takes effect.</span></span>

     ![Internet](./media/vmm-to-azure-walkthrough-source-target/provider13.png)
7. <span data-ttu-id="9c8c9-137">Přijměte nebo změňte umístění certifikátu SSL, který je automaticky generován pro šifrování dat.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-137">Accept or modify the location of an SSL certificate that’s automatically generated for data encryption.</span></span> <span data-ttu-id="9c8c9-138">Tento certifikát se používá, pokud povolíte šifrování dat pro cloud chráněný platformou Azure na portálu Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-138">This certificate is used if you enable data encryption for a cloud protected by Azure in the Azure Site Recovery portal.</span></span> <span data-ttu-id="9c8c9-139">Uchovávejte tento certifikát v bezpečí.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-139">Keep this certificate safe.</span></span> <span data-ttu-id="9c8c9-140">Při spuštění předání služeb při selhání do Azure ho budete potřebovat k dešifrování, pokud je povolené šifrování dat.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-140">When you run a failover to Azure you’ll need it to decrypt, if data encryption is enabled.</span></span>
8. <span data-ttu-id="9c8c9-141">Do pole **Název serveru** zadejte popisný název, který bude identifikovat server VMM v trezoru.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-141">In **Server name**, specify a friendly name to identify the VMM server in the vault.</span></span> <span data-ttu-id="9c8c9-142">V konfiguraci clusteru zadejte název role clusteru VMM.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-142">In a cluster configuration, specify the VMM cluster role name.</span></span>
9. <span data-ttu-id="9c8c9-143">Pokud chcete synchronizovat metadata pro všechny cloudy na serveru VMM v trezoru, zaškrtněte políčko **Synchronizovat metadata cloudu**.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-143">Enable **Sync cloud metadata**, if you want to synchronize metadata for all clouds on the VMM server with the vault.</span></span> <span data-ttu-id="9c8c9-144">Tuto akci stačí na každém serveru provést pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-144">This action only needs to happen once on each server.</span></span> <span data-ttu-id="9c8c9-145">Pokud nechcete provádět synchronizaci se všemi cloudy, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech cloudu v konzole VMM.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-145">If you don't want to synchronize all clouds, you can leave this setting unchecked and synchronize each cloud individually in the cloud properties in the VMM console.</span></span> <span data-ttu-id="9c8c9-146">Kliknutím na **Zaregistrovat** proces dokončete.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-146">Click **Register** to complete the process.</span></span>

    ![Registrace serveru](./media/vmm-to-azure-walkthrough-source-target/provider16.png)
10. <span data-ttu-id="9c8c9-148">Spustí se registrace.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-148">Registration starts.</span></span> <span data-ttu-id="9c8c9-149">Po dokončení registrace se server zobrazí v části **Infrastruktura Site Recovery** > **Servery VMM**.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-149">After registration finishes, the server is displayed in **Site Recovery Infrastructure** > **VMM Servers**.</span></span>


## <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a><span data-ttu-id="9c8c9-150">Instalace agenta Služeb zotavení Azure na hostitele Hyper-V</span><span class="sxs-lookup"><span data-stu-id="9c8c9-150">Install the Azure Recovery Services agent on Hyper-V hosts</span></span>

1. <span data-ttu-id="9c8c9-151">Po nastavení zprostředkovatele si musíte stáhnout instalační soubor pro agenta Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-151">After you've set up the Provider, you need to download the installation file for the Azure Recovery Services agent.</span></span> <span data-ttu-id="9c8c9-152">Spusťte instalační program na každém serveru Hyper-V v cloudu VMM.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-152">Run setup on each Hyper-V server in the VMM cloud.</span></span>

    ![Lokality Hyper-V](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent1.png)
2. <span data-ttu-id="9c8c9-154">Na stránce **Kontrola předpokladů** klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-154">In **Prerequisites Check**, click **Next**.</span></span> <span data-ttu-id="9c8c9-155">Automaticky se nainstalují všechny chybějící požadované součásti.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-155">Any missing prerequisites will be automatically installed.</span></span>

    ![Požadavky agenta Služeb zotavení](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent2.png)
3. <span data-ttu-id="9c8c9-157">V části **Nastavení instalace** přijměte nebo změňte umístění instalace a mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-157">In **Installation Settings**, accept or modify the installation location, and the cache location.</span></span> <span data-ttu-id="9c8c9-158">Mezipaměť můžete nakonfigurovat na jednotce, na které je minimálně 5 GB dostupného úložiště. Pro mezipaměť ale doporučujeme disk minimálně s 600 GB volného místa.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-158">You can configure the cache on a drive that has at least 5 GB of storage available but we recommend a cache drive with 600 GB or more of free space.</span></span> <span data-ttu-id="9c8c9-159">Pak klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-159">Then click **Install**.</span></span>
4. <span data-ttu-id="9c8c9-160">Po dokončení instalace klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-160">After installation is complete, click **Close** to finish.</span></span>

    ![Registrace agenta MARS](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent3.png)

### <a name="command-line-installation"></a><span data-ttu-id="9c8c9-162">Instalace pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="9c8c9-162">Command line installation</span></span>
<span data-ttu-id="9c8c9-163">Agenta Služeb zotavení Microsoft Azure můžete nainstalovat z příkazového řádku pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="9c8c9-163">You can install the Microsoft Azure Recovery Services Agent from command line using the following command:</span></span>

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a><span data-ttu-id="9c8c9-164">Nastavení internetového přístupu proxy serveru k Site Recovery z hostitelů Hyper-V</span><span class="sxs-lookup"><span data-stu-id="9c8c9-164">Set up internet proxy access to Site Recovery from Hyper-V hosts</span></span>

<span data-ttu-id="9c8c9-165">Agent Služeb zotavení, který běží na hostitelích Hyper-V, potřebuje internetový přístup k Azure pro replikaci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-165">The Recovery Services agent running on Hyper-V hosts needs internet access to Azure for VM replication.</span></span> <span data-ttu-id="9c8c9-166">Pokud získáváte přístup k internetu prostřednictvím serveru proxy, nastavte ho následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9c8c9-166">If you're accessing the internet through a proxy, set it up as follows:</span></span>

1. <span data-ttu-id="9c8c9-167">Otevřete modul snap-in Microsoft Azure Backup konzoly MMC na hostiteli Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-167">Open the Microsoft Azure Backup MMC snap-in on the Hyper-V host.</span></span> <span data-ttu-id="9c8c9-168">Ve výchozím nastavení je na ploše nebo v umístění C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-168">By default, a shortcut for Microsoft Azure Backup is available on the desktop or in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span></span>
2. <span data-ttu-id="9c8c9-169">V modulu snap-in klikněte na **Změnit vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-169">In the snap-in, click **Change Properties**.</span></span>
3. <span data-ttu-id="9c8c9-170">Na kartě **Konfigurace proxy** zadejte informace o proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-170">On the **Proxy Configuration** tab, specify proxy server information.</span></span>

    ![Registrace agenta MARS](./media/vmm-to-azure-walkthrough-source-target/mars-proxy.png)
4. <span data-ttu-id="9c8c9-172">Zkontrolujte, že má agent přístup k adresám URL popsaným v části s [požadavky](#on-premises-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="9c8c9-172">Check that the agent can reach the URLs described in the [prerequisites](#on-premises-prerequisites).</span></span>

## <a name="set-up-the-target-environment"></a><span data-ttu-id="9c8c9-173">Nastavení cílového prostředí</span><span class="sxs-lookup"><span data-stu-id="9c8c9-173">Set up the target environment</span></span>
<span data-ttu-id="9c8c9-174">Zadejte účet úložiště Azure, který se má používat pro replikaci, a síť Azure, ke které se virtuální počítače Azure připojí po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-174">Specify the Azure storage account to be used for replication, and the Azure network to which Azure VMs will connect after failover.</span></span>

1. <span data-ttu-id="9c8c9-175">Klikněte na **Připravit infrastrukturu** > **Cíl** a vyberte předplatné a skupinu prostředků, ve kterých chcete vytvořit virtuální počítače, pro které bylo provedeno převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-175">Click **Prepare infrastructure** > **Target**, select the subscription and the resource group where you want to create the failed over virtual machines.</span></span> <span data-ttu-id="9c8c9-176">Vyberte model nasazení (Classic nebo Resource Manager), který chcete v Azure použít pro virtuální počítače, pro které bylo provedeno převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-176">Choose the deployment model that you want to use in Azure (classic or resource management) for the failed over virtual machines.</span></span>

    ![Úložiště](./media/vmm-to-azure-walkthrough-source-target/enablerep3.png)

2. <span data-ttu-id="9c8c9-178">Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-178">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

    ![Úložiště](./media/vmm-to-azure-walkthrough-source-target/compatible-storage.png)

3. <span data-ttu-id="9c8c9-180">Pokud jste ještě nevytvořili účet úložiště a chcete ho vytvořit pomocí Resource Manageru, klikněte na **+ Účet úložiště** a můžete to provést přímo tady.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-180">If you haven't created a storage account, and you want to create one using Resource Manager, click **+Storage account** to do that inline.</span></span>  <span data-ttu-id="9c8c9-181">V okně **Vytvořit účet úložiště** zadejte název účtu, typ, předplatné a umístění.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-181">On the **Create storage account** blade specify an account name, type, subscription, and location.</span></span> <span data-ttu-id="9c8c9-182">Účet by měl být ve stejném umístění jako trezor Služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-182">The account should be in the same location as the Recovery Services vault.</span></span>

   ![Úložiště](./media/vmm-to-azure-walkthrough-source-target/gs-createstorage.png)


   * <span data-ttu-id="9c8c9-184">Pokud chcete vytvořit účet úložiště pomocí klasického modelu, můžete to udělat na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-184">If you want to create a storage account using the classic model, do that in the Azure portal.</span></span> [<span data-ttu-id="9c8c9-185">Další informace</span><span class="sxs-lookup"><span data-stu-id="9c8c9-185">Learn more</span></span>](../storage/common/storage-create-storage-account.md)
   * <span data-ttu-id="9c8c9-186">Pokud pro replikovaná data používáte účet úložiště Storage úrovně Premium, nastavte si ještě účet úložiště úrovně Standard pro ukládání protokolů replikace, do kterých se zaznamenávají průběžné změny místních dat.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-186">If you’re using a premium storage account for replicated data, set up an additional standard storage account, to store replication logs that capture ongoing changes to on-premises data.</span></span>
5. <span data-ttu-id="9c8c9-187">Pokud jste ještě nevytvořili síť Azure a chcete ji vytvořit pomocí modelu Resource Manageru, klikněte na **+Síť** a můžete to provést přímo tady.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-187">If you haven't created an Azure network, and you want to create one using Resource Manager, click **+Network** to do that inline.</span></span> <span data-ttu-id="9c8c9-188">V okně **Vytvořit virtuální síť** zadejte název sítě, rozsah adres, podrobnosti o podsíti, předplatné a umístění.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-188">On the **Create virtual network** blade specify a network name, address range, subnet details, subscription, and location.</span></span> <span data-ttu-id="9c8c9-189">Síť by měla být ve stejném umístění jako trezor Služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-189">The network should be in the same location as the Recovery Services vault.</span></span>

   ![Síť](./media/vmm-to-azure-walkthrough-source-target/gs-createnetwork.png)

   <span data-ttu-id="9c8c9-191">Pokud chcete vytvořit síť pomocí klasického modelu, udělejte to na portálu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9c8c9-191">If you want to create a network using the classic model, do that in the Azure portal.</span></span> <span data-ttu-id="9c8c9-192">[Další informace](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="9c8c9-192">[Learn more](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).</span></span>





## <a name="next-steps"></a><span data-ttu-id="9c8c9-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9c8c9-193">Next steps</span></span>

<span data-ttu-id="9c8c9-194">Přejděte na [krok 9: nakonfigurování mapování sítě](vmm-to-azure-walkthrough-network-mapping.md)</span><span class="sxs-lookup"><span data-stu-id="9c8c9-194">Go to [Step 9: Configure network mapping](vmm-to-azure-walkthrough-network-mapping.md)</span></span>
