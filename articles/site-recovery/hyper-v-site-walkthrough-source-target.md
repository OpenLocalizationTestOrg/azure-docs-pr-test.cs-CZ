---
title: "aaaSet hello zdroj a cíl pro tooAzure replikace technologie Hyper-V (bez System Center VMM) s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky tooset hello nastavení zdroj a cíl pro replikaci virtuálních počítačů Hyper-V tooAzure úložiště s Azure Site Recovery"
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
ms.openlocfilehash: 105b90e6ac053d5b842c54a36c460a26d0f5c2ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-hyper-v-replication-tooazure"></a><span data-ttu-id="5af92-103">Krok 8: Nastavení hello zdroj a cíl pro tooAzure replikace technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="5af92-103">Step 8: Set up hello source and target for Hyper-V replication tooAzure</span></span>

<span data-ttu-id="5af92-104">Tento článek popisuje, jak tooconfigure zdrojové a cílové nastavení při replikaci místně tooAzure technologie Hyper-V virtuální počítače (bez System Center VMM), pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5af92-104">This article describes how tooconfigure source and target settings when replicating on-premises Hyper-V virtual machines (without System Center VMM) tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="5af92-105">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="5af92-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-hello-source-environment"></a><span data-ttu-id="5af92-106">Nastavit hello zdrojové prostředí</span><span class="sxs-lookup"><span data-stu-id="5af92-106">Set up hello source environment</span></span>

<span data-ttu-id="5af92-107">Nastavení lokality hello technologie Hyper-V, nainstalujte hello zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure hello na hostitele Hyper-V a registraci hello lokality v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="5af92-107">Set up hello Hyper-V site, install hello Azure Site Recovery Provider and hello Azure Recovery Services agent on Hyper-V hosts, and register hello site in hello vault.</span></span>

1. <span data-ttu-id="5af92-108">V **Příprava infrastruktury**, klikněte na tlačítko **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="5af92-108">In **Prepare Infrastructure**, click **Source**.</span></span> <span data-ttu-id="5af92-109">Klikněte na tlačítko tooadd nové lokality Hyper-V jako kontejner pro hostitele Hyper-V nebo clusterů, **serveru technologie Hyper-V +**.</span><span class="sxs-lookup"><span data-stu-id="5af92-109">tooadd a new Hyper-V site as a container for your Hyper-V hosts or clusters, click **+Hyper-V Site**.</span></span>

    ![Nastavení zdroje](./media/hyper-v-site-walkthrough-source-target/set-source1.png)
2. <span data-ttu-id="5af92-111">V **web vytvořit Hyper-V**, zadejte název lokality hello.</span><span class="sxs-lookup"><span data-stu-id="5af92-111">In **Create Hyper-V site**, specify a name for hello site.</span></span> <span data-ttu-id="5af92-112">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5af92-112">Then click **OK**.</span></span> <span data-ttu-id="5af92-113">Nyní, vyberte lokalitu hello jste vytvořili a klikněte na tlačítko **+ technologie Hyper-V Server** tooadd toohello serveru lokality.</span><span class="sxs-lookup"><span data-stu-id="5af92-113">Now, select hello site you created, and click **+Hyper-V Server** tooadd a server toohello site.</span></span>

    ![Nastavení zdroje](./media/hyper-v-site-walkthrough-source-target/set-source2.png)

3. <span data-ttu-id="5af92-115">V **přidat Server** > **typ serveru**, zkontrolujte, zda **technologie Hyper-V server** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="5af92-115">In **Add Server** > **Server type**, check that **Hyper-V server** is displayed.</span></span>

    - <span data-ttu-id="5af92-116">Zajistěte, aby tento server hello technologie Hyper-V chcete tooadd odpovídá hello [požadavky](#on-premises-prerequisites), a je možné tooaccess hello zadaných adres URL.</span><span class="sxs-lookup"><span data-stu-id="5af92-116">Make sure that hello Hyper-V server you want tooadd complies with hello [prerequisites](#on-premises-prerequisites), and is able tooaccess hello specified URLs.</span></span>
    - <span data-ttu-id="5af92-117">Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery hello.</span><span class="sxs-lookup"><span data-stu-id="5af92-117">Download hello Azure Site Recovery Provider installation file.</span></span> <span data-ttu-id="5af92-118">Spusťte tento soubor tooinstall hello zprostředkovatele a hello agenta služeb zotavení na každém hostiteli technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="5af92-118">You run this file tooinstall hello Provider and hello Recovery Services agent on each Hyper-V host.</span></span>

    ![Nastavení zdroje](./media/hyper-v-site-walkthrough-source-target/set-source3.png)


## <a name="install-hello-provider-and-agent"></a><span data-ttu-id="5af92-120">Instalace hello zprostředkovatel a agent</span><span class="sxs-lookup"><span data-stu-id="5af92-120">Install hello Provider and agent</span></span>

1. <span data-ttu-id="5af92-121">Spusťte instalační soubor zprostředkovatele hello na každém hostiteli jste přidali lokality toohello technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="5af92-121">Run hello Provider setup file on each host you added toohello Hyper-V site.</span></span> <span data-ttu-id="5af92-122">Pokud instalujete na clusteru s podporou technologie Hyper-V, spusťte instalační program na každém uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="5af92-122">If you're installing on a Hyper-V cluster, run setup on each cluster node.</span></span> <span data-ttu-id="5af92-123">Instalace a registrace každého uzlu clusteru technologie Hyper-V zajistí, že jsou chráněné virtuální počítače, i v případě, že migraci mezi uzly.</span><span class="sxs-lookup"><span data-stu-id="5af92-123">Installing and registering each Hyper-V cluster node ensures that VMs are protected, even if they migrate across nodes.</span></span>
2. <span data-ttu-id="5af92-124">V rámci **Microsoft Update** můžete vyjádřit výslovný souhlas s aktualizacemi, aby se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="5af92-124">In **Microsoft Update**, you can opt in for updates so that Provider updates are installed in accordance with your Microsoft Update policy.</span></span>
3. <span data-ttu-id="5af92-125">V **instalace**, přijmout nebo úpravě hello výchozí umístění instalace zprostředkovatele a klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="5af92-125">In **Installation**, accept or modify hello default Provider installation location and click **Install**.</span></span>
4. <span data-ttu-id="5af92-126">V **nastavení trezoru**, klikněte na tlačítko **Procházet** tooselect hello trezoru klíčů soubor, který jste stáhli.</span><span class="sxs-lookup"><span data-stu-id="5af92-126">In **Vault Settings**, click **Browse** tooselect hello vault key file that you downloaded.</span></span> <span data-ttu-id="5af92-127">Zadejte předplatné Azure Site Recovery hello, hello název trezoru, a patří hello technologie Hyper-V serveru toowhich hello technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="5af92-127">Specify hello Azure Site Recovery subscription, hello vault name, and hello Hyper-V site toowhich hello Hyper-V server belongs.</span></span>

    ![Registrace serveru](./media/hyper-v-site-walkthrough-source-target/provider3.png)

5. <span data-ttu-id="5af92-129">V **nastavení proxy serveru**, zadejte, jak hello hello zprostředkovatel, který běží na hostitelích technologie Hyper-V připojí tooAzure Site Recovery přes internet.</span><span class="sxs-lookup"><span data-stu-id="5af92-129">In **Proxy Settings**, specify how hello Provider running on Hyper-V hosts connects tooAzure Site Recovery over hello internet.</span></span>

    * <span data-ttu-id="5af92-130">Pokud chcete hello zprostředkovatele tooconnect přímo vyberte **připojit přímo bez proxy tooAzure Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="5af92-130">If you want hello Provider tooconnect directly select **Connect directly tooAzure Site Recovery without a proxy**.</span></span>
    * <span data-ttu-id="5af92-131">Pokud váš stávající proxy server vyžaduje ověření, nebo chcete použít pro připojení poskytovatele hello toouse vlastní proxy server, vyberte **připojit tooAzure Site Recovery pomocí proxy serveru**.</span><span class="sxs-lookup"><span data-stu-id="5af92-131">If your existing proxy requires authentication, or you want toouse a custom proxy for hello Provider connection, select **Connect tooAzure Site Recovery using a proxy server**.</span></span>
    * <span data-ttu-id="5af92-132">Pokud používáte proxy server:</span><span class="sxs-lookup"><span data-stu-id="5af92-132">If you use a proxy:</span></span>
        - <span data-ttu-id="5af92-133">Zadejte hello adresu, port a přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="5af92-133">Specify hello address, port, and credentials</span></span>
        - <span data-ttu-id="5af92-134">Ujistěte se, zda text hello adresy URL popisované v hello [požadavky](#prerequisites) jsou povoleny přes proxy server hello.</span><span class="sxs-lookup"><span data-stu-id="5af92-134">Make sure hello URLs described in hello [prerequisites](#prerequisites) are allowed through hello proxy.</span></span>

    ![Internet](./media/hyper-v-site-walkthrough-source-target/provider7.png)

6. <span data-ttu-id="5af92-136">Po dokončení instalace, klikněte na tlačítko **zaregistrovat** tooregister hello server v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="5af92-136">After installation finishes, click **Register** tooregister hello server in hello vault.</span></span>

    ![Umístění instalace](./media/hyper-v-site-walkthrough-source-target/provider2.png)

7. <span data-ttu-id="5af92-138">Po dokončení registrace pomocí Azure Site Recovery je načíst metadata ze serveru hello technologie Hyper-V a hello server se zobrazí v **infrastruktura Site Recovery** > **hostitelů Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="5af92-138">After registration finishes, metadata from hello Hyper-V server is retrieved by Azure Site Recovery, and hello server is displayed in **Site Recovery Infrastructure** > **Hyper-V Hosts**.</span></span>


## <a name="set-up-hello-target-environment"></a><span data-ttu-id="5af92-139">Nastavení cílového prostředí hello</span><span class="sxs-lookup"><span data-stu-id="5af92-139">Set up hello target environment</span></span>

<span data-ttu-id="5af92-140">Zadejte hello účtu úložiště Azure pro replikaci a hello toowhich síť Azure virtuální počítače Azure připojí po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="5af92-140">Specify hello Azure storage account for replication, and hello Azure network toowhich Azure VMs will connect after failover.</span></span>

1. <span data-ttu-id="5af92-141">Klikněte na tlačítko **připravit infrastrukturu** > **cíl**.</span><span class="sxs-lookup"><span data-stu-id="5af92-141">Click **Prepare infrastructure** > **Target**.</span></span>
2. <span data-ttu-id="5af92-142">Vyberte hello předplatné a skupina prostředků hello, ve kterém chcete toocreate hello virtuální počítače Azure po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="5af92-142">Select hello subscription and hello resource group in which you want toocreate hello Azure VMs after failover.</span></span> <span data-ttu-id="5af92-143">Zvolte hello nasazení modelu, že chcete toouse v Azure (klasický nebo prostředek management) pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5af92-143">Choose hello deployment model that you want toouse in Azure (classic or resource management) for hello VMs.</span></span>

3. <span data-ttu-id="5af92-144">Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="5af92-144">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

    - <span data-ttu-id="5af92-145">Pokud nemáte účet úložiště, klikněte na tlačítko **+ úložiště** toocreate vložené založené na správci prostředků účtu.</span><span class="sxs-lookup"><span data-stu-id="5af92-145">If you don't have a storage account, click **+Storage** toocreate a Resource Manager-based account inline.</span></span> 
    - <span data-ttu-id="5af92-146">Pokud nemáte síť Azure, klikněte na tlačítko **+ síť** toocreate vložené sítě pomocí Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="5af92-146">If you don't have a Azure network, click **+Network** toocreate a Resource Manager-based network inline.</span></span>






## <a name="next-steps"></a><span data-ttu-id="5af92-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5af92-147">Next steps</span></span>

<span data-ttu-id="5af92-148">Přejděte příliš[krok 9: nastavení zásad replikace](hyper-v-site-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="5af92-148">Go too[Step 9: Set up a replication policy](hyper-v-site-walkthrough-replication.md)</span></span>
