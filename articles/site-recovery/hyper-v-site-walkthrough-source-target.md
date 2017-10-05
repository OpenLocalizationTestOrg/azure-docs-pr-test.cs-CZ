---
title: "Nastavit zdroje a cíle pro replikaci technologie Hyper-V do Azure (bez System Center VMM) s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky nastavit zdroj a cíl nastavení pro replikaci virtuálních počítačů technologie Hyper-V do úložiště Azure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d2010d85-77fd-4dea-84f3-1c960ed4c63f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: b38eb3a011d46f2239891ea1d1bcac2a4059a866
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-8-set-up-the-source-and-target-for-hyper-v-replication-to-azure"></a><span data-ttu-id="bef2d-103">Krok 8: Nastavte zdroj a cíl pro replikaci technologie Hyper-V do Azure.</span><span class="sxs-lookup"><span data-stu-id="bef2d-103">Step 8: Set up the source and target for Hyper-V replication to Azure</span></span>

<span data-ttu-id="bef2d-104">Tento článek popisuje, jak nakonfigurovat nastavení zdrojové a cílové během replikovat místní virtuální počítače Hyper-V (bez System Center VMM) do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bef2d-104">This article describes how to configure source and target settings when replicating on-premises Hyper-V virtual machines (without System Center VMM) to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="bef2d-105">POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="bef2d-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-the-source-environment"></a><span data-ttu-id="bef2d-106">Nastavení zdrojového prostředí</span><span class="sxs-lookup"><span data-stu-id="bef2d-106">Set up the source environment</span></span>

<span data-ttu-id="bef2d-107">Nastavte web Hyper-V, nainstalujte zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure na hostitele Hyper-V a zaregistrujte lokality v trezoru.</span><span class="sxs-lookup"><span data-stu-id="bef2d-107">Set up the Hyper-V site, install the Azure Site Recovery Provider and the Azure Recovery Services agent on Hyper-V hosts, and register the site in the vault.</span></span>

1. <span data-ttu-id="bef2d-108">V **Příprava infrastruktury**, klikněte na tlačítko **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="bef2d-108">In **Prepare Infrastructure**, click **Source**.</span></span> <span data-ttu-id="bef2d-109">Chcete-li přidat nový web technologie Hyper-V jako kontejner pro hostitele Hyper-V nebo clusterů, klikněte na tlačítko **serveru technologie Hyper-V +**.</span><span class="sxs-lookup"><span data-stu-id="bef2d-109">To add a new Hyper-V site as a container for your Hyper-V hosts or clusters, click **+Hyper-V Site**.</span></span>

    ![Nastavení zdroje](./media/hyper-v-site-walkthrough-source-target/set-source1.png)
2. <span data-ttu-id="bef2d-111">V **web vytvořit Hyper-V**, zadejte název pro tuto lokalitu.</span><span class="sxs-lookup"><span data-stu-id="bef2d-111">In **Create Hyper-V site**, specify a name for the site.</span></span> <span data-ttu-id="bef2d-112">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="bef2d-112">Then click **OK**.</span></span> <span data-ttu-id="bef2d-113">Nyní, vyberte lokalitu, jste vytvořili a klikněte na tlačítko **+ technologie Hyper-V Server** přidání serveru do lokality.</span><span class="sxs-lookup"><span data-stu-id="bef2d-113">Now, select the site you created, and click **+Hyper-V Server** to add a server to the site.</span></span>

    ![Nastavení zdroje](./media/hyper-v-site-walkthrough-source-target/set-source2.png)

3. <span data-ttu-id="bef2d-115">V **přidat Server** > **typ serveru**, zkontrolujte, zda **technologie Hyper-V server** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="bef2d-115">In **Add Server** > **Server type**, check that **Hyper-V server** is displayed.</span></span>

    - <span data-ttu-id="bef2d-116">Ujistěte se, že server Hyper-V, které chcete přidat v souladu se [požadavky](#on-premises-prerequisites)a bude schopen získat přístup k zadané adresy URL.</span><span class="sxs-lookup"><span data-stu-id="bef2d-116">Make sure that the Hyper-V server you want to add complies with the [prerequisites](#on-premises-prerequisites), and is able to access the specified URLs.</span></span>
    - <span data-ttu-id="bef2d-117">Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="bef2d-117">Download the Azure Site Recovery Provider installation file.</span></span> <span data-ttu-id="bef2d-118">Je-li spustit tento soubor k instalaci zprostředkovatele a agenta služeb zotavení na každém hostiteli technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="bef2d-118">You run this file to install the Provider and the Recovery Services agent on each Hyper-V host.</span></span>

    ![Nastavení zdroje](./media/hyper-v-site-walkthrough-source-target/set-source3.png)


## <a name="install-the-provider-and-agent"></a><span data-ttu-id="bef2d-120">Nainstaluje zprostředkovatele a agenta</span><span class="sxs-lookup"><span data-stu-id="bef2d-120">Install the Provider and agent</span></span>

1. <span data-ttu-id="bef2d-121">Spusťte instalační soubor na každém hostiteli, které jste přidali do lokality Hyper-V zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="bef2d-121">Run the Provider setup file on each host you added to the Hyper-V site.</span></span> <span data-ttu-id="bef2d-122">Pokud instalujete na clusteru s podporou technologie Hyper-V, spusťte instalační program na každém uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="bef2d-122">If you're installing on a Hyper-V cluster, run setup on each cluster node.</span></span> <span data-ttu-id="bef2d-123">Instalace a registrace každého uzlu clusteru technologie Hyper-V zajistí, že jsou chráněné virtuální počítače, i v případě, že migraci mezi uzly.</span><span class="sxs-lookup"><span data-stu-id="bef2d-123">Installing and registering each Hyper-V cluster node ensures that VMs are protected, even if they migrate across nodes.</span></span>
2. <span data-ttu-id="bef2d-124">V rámci **Microsoft Update** můžete vyjádřit výslovný souhlas s aktualizacemi, aby se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="bef2d-124">In **Microsoft Update**, you can opt in for updates so that Provider updates are installed in accordance with your Microsoft Update policy.</span></span>
3. <span data-ttu-id="bef2d-125">V okně **Instalace** přijměte nebo změňte výchozí umístění instalace zprostředkovatele a klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="bef2d-125">In **Installation**, accept or modify the default Provider installation location and click **Install**.</span></span>
4. <span data-ttu-id="bef2d-126">V **nastavení trezoru**, klikněte na tlačítko **Procházet** a vyberte soubor klíče trezoru, který jste si stáhli.</span><span class="sxs-lookup"><span data-stu-id="bef2d-126">In **Vault Settings**, click **Browse** to select the vault key file that you downloaded.</span></span> <span data-ttu-id="bef2d-127">Zadejte předplatné Azure Site Recovery, název trezoru a Web Hyper-V, do které patří server technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="bef2d-127">Specify the Azure Site Recovery subscription, the vault name, and the Hyper-V site to which the Hyper-V server belongs.</span></span>

    ![Registrace serveru](./media/hyper-v-site-walkthrough-source-target/provider3.png)

5. <span data-ttu-id="bef2d-129">V **nastavení proxy serveru**, určete, jak se zprostředkovatel, který běží na hostitelích technologie Hyper-V připojuje k Azure Site Recovery přes internet.</span><span class="sxs-lookup"><span data-stu-id="bef2d-129">In **Proxy Settings**, specify how the Provider running on Hyper-V hosts connects to Azure Site Recovery over the internet.</span></span>

    * <span data-ttu-id="bef2d-130">Pokud chcete, aby se zprostředkovatel připojil přímo, vyberte **Připojit k Azure Site Recovery přímo bez proxy serveru**.</span><span class="sxs-lookup"><span data-stu-id="bef2d-130">If you want the Provider to connect directly select **Connect directly to Azure Site Recovery without a proxy**.</span></span>
    * <span data-ttu-id="bef2d-131">Pokud váš stávající proxy server vyžaduje ověření, nebo pokud chcete používat vlastní proxy server pro připojení poskytovatele, vyberte **připojit k Azure Site Recovery pomocí proxy serveru**.</span><span class="sxs-lookup"><span data-stu-id="bef2d-131">If your existing proxy requires authentication, or you want to use a custom proxy for the Provider connection, select **Connect to Azure Site Recovery using a proxy server**.</span></span>
    * <span data-ttu-id="bef2d-132">Pokud používáte proxy server:</span><span class="sxs-lookup"><span data-stu-id="bef2d-132">If you use a proxy:</span></span>
        - <span data-ttu-id="bef2d-133">Zadejte adresu, port a přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="bef2d-133">Specify the address, port, and credentials</span></span>
        - <span data-ttu-id="bef2d-134">Zajistěte, aby k adresám URL popsaným v [požadavky](#prerequisites) jsou povoleny prostřednictvím proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="bef2d-134">Make sure the URLs described in the [prerequisites](#prerequisites) are allowed through the proxy.</span></span>

    ![Internet](./media/hyper-v-site-walkthrough-source-target/provider7.png)

6. <span data-ttu-id="bef2d-136">Po dokončení instalace, klikněte na tlačítko **zaregistrovat** zaregistrujte server v trezoru.</span><span class="sxs-lookup"><span data-stu-id="bef2d-136">After installation finishes, click **Register** to register the server in the vault.</span></span>

    ![Umístění instalace](./media/hyper-v-site-walkthrough-source-target/provider2.png)

7. <span data-ttu-id="bef2d-138">Po dokončení registrace pomocí Azure Site Recovery je načíst metadata ze serveru technologie Hyper-V a server se zobrazí v **infrastruktura Site Recovery** > **hostitelů Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="bef2d-138">After registration finishes, metadata from the Hyper-V server is retrieved by Azure Site Recovery, and the server is displayed in **Site Recovery Infrastructure** > **Hyper-V Hosts**.</span></span>


## <a name="set-up-the-target-environment"></a><span data-ttu-id="bef2d-139">Nastavení cílového prostředí</span><span class="sxs-lookup"><span data-stu-id="bef2d-139">Set up the target environment</span></span>

<span data-ttu-id="bef2d-140">Zadejte účet úložiště Azure pro replikaci a síť Azure, ke které se virtuální počítače Azure připojí po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="bef2d-140">Specify the Azure storage account for replication, and the Azure network to which Azure VMs will connect after failover.</span></span>

1. <span data-ttu-id="bef2d-141">Klikněte na tlačítko **připravit infrastrukturu** > **cíl**.</span><span class="sxs-lookup"><span data-stu-id="bef2d-141">Click **Prepare infrastructure** > **Target**.</span></span>
2. <span data-ttu-id="bef2d-142">Vyberte předplatné a skupině prostředků, ve kterém chcete vytvořit virtuální počítače Azure po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="bef2d-142">Select the subscription and the resource group in which you want to create the Azure VMs after failover.</span></span> <span data-ttu-id="bef2d-143">Vyberte model nasazení, který chcete použít pro virtuální počítače v Azure (klasický nebo prostředek management).</span><span class="sxs-lookup"><span data-stu-id="bef2d-143">Choose the deployment model that you want to use in Azure (classic or resource management) for the VMs.</span></span>

3. <span data-ttu-id="bef2d-144">Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="bef2d-144">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

    - <span data-ttu-id="bef2d-145">Pokud nemáte účet úložiště, klikněte na tlačítko **+ úložiště** vytvořit vložený založené na správci prostředků účtu.</span><span class="sxs-lookup"><span data-stu-id="bef2d-145">If you don't have a storage account, click **+Storage** to create a Resource Manager-based account inline.</span></span> 
    - <span data-ttu-id="bef2d-146">Pokud nemáte síť Azure, klikněte na tlačítko **+ síť** vytvořit vložený sítě pomocí Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="bef2d-146">If you don't have a Azure network, click **+Network** to create a Resource Manager-based network inline.</span></span>






## <a name="next-steps"></a><span data-ttu-id="bef2d-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bef2d-147">Next steps</span></span>

<span data-ttu-id="bef2d-148">Přejděte na [krok 9: nastavení zásad replikace](hyper-v-site-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="bef2d-148">Go to [Step 9: Set up a replication policy](hyper-v-site-walkthrough-replication.md)</span></span>
