---
title: "Nastavit zdroje a cíle pro fyzický server replikaci do Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky k zadání nastavení zdroje a cíle replikace fyzických serverů do úložiště Azure se službou Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 2e3d03bc-4e53-4590-8a01-f702d3cc9500
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: e89bbf5a2c1d71852e49da43d3106a05ebfc28a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-7-set-up-the-source-and-target-for-physical-server-replication-to-azure"></a><span data-ttu-id="87ed7-103">Krok 7: Nastavte zdroj a cíl pro fyzický server replikaci do Azure.</span><span class="sxs-lookup"><span data-stu-id="87ed7-103">Step 7: Set up the source and target for physical server replication to Azure</span></span>

<span data-ttu-id="87ed7-104">Tento článek popisuje, jak nakonfigurovat nastavení zdrojové a cílové během replikace místní fyzických serverů do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="87ed7-104">This article describes how to configure source and target settings when replicating on-premises physical servers to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="87ed7-105">POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="87ed7-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-the-source-environment"></a><span data-ttu-id="87ed7-106">Nastavení zdrojového prostředí</span><span class="sxs-lookup"><span data-stu-id="87ed7-106">Set up the source environment</span></span>

<span data-ttu-id="87ed7-107">Nastavení konfigurace serveru, registrace v trezoru a zjistit počítače.</span><span class="sxs-lookup"><span data-stu-id="87ed7-107">Set up the configuration server, register it in the vault, and discover machines.</span></span>

1. <span data-ttu-id="87ed7-108">Klikněte na tlačítko **lokality obnovení** > **krok 1: Příprava infrastruktury** > **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="87ed7-108">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Source**.</span></span>
2. <span data-ttu-id="87ed7-109">Pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server**.</span><span class="sxs-lookup"><span data-stu-id="87ed7-109">If you don’t have a configuration server, click **+Configuration server**.</span></span>
3. <span data-ttu-id="87ed7-110">V **přidat Server**, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.</span><span class="sxs-lookup"><span data-stu-id="87ed7-110">In **Add Server**, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="87ed7-111">Stáhněte instalační soubor nástroje Unified instalace nástroje Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="87ed7-111">Download the Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="87ed7-112">Stáhnout registrační klíč trezoru</span><span class="sxs-lookup"><span data-stu-id="87ed7-112">Download the vault registration key.</span></span> <span data-ttu-id="87ed7-113">Tuto funkci potřebujete po spuštění Unified instalace.</span><span class="sxs-lookup"><span data-stu-id="87ed7-113">You need this when you run Unified Setup.</span></span> <span data-ttu-id="87ed7-114">Klíč je platný pět dní od jeho vygenerování.</span><span class="sxs-lookup"><span data-stu-id="87ed7-114">The key is valid for five days after you generate it.</span></span>

   ![Nastavení zdroje](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-the-configuration-server-in-the-vault"></a><span data-ttu-id="87ed7-116">Zaregistrujte konfigurační server v trezoru</span><span class="sxs-lookup"><span data-stu-id="87ed7-116">Register the configuration server in the vault</span></span>

<span data-ttu-id="87ed7-117">Předtím, než můžete spustit, a potom spuštěním Unified instalačního programu nainstalujte konfigurační server, procesový server a hlavní cílový server, postupujte takto.</span><span class="sxs-lookup"><span data-stu-id="87ed7-117">Do the following before you start, then run Unified Setup to install the configuration server, the process server, and the master target server.</span></span> <span data-ttu-id="87ed7-118">Video popisuje nastavení součásti pro virtuální počítač VMware do Azure replikace.</span><span class="sxs-lookup"><span data-stu-id="87ed7-118">The video describes setting up the components for VMware VM to Azure replication.</span></span> <span data-ttu-id="87ed7-119">Stejný postup je však platný pro fyzický server na Azure replikaci.</span><span class="sxs-lookup"><span data-stu-id="87ed7-119">However, the same process is valid for physical server to Azure replication.</span></span>

- <span data-ttu-id="87ed7-120">Na serveru, konfigurace virtuálního počítače, zkontrolujte, jestli se systémové hodiny synchronizované se [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span><span class="sxs-lookup"><span data-stu-id="87ed7-120">On the configuration server VM, make sure that the system clock is synchronized with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span></span> <span data-ttu-id="87ed7-121">Měla by se shodovat.</span><span class="sxs-lookup"><span data-stu-id="87ed7-121">It should match.</span></span> <span data-ttu-id="87ed7-122">Pokud je 15 minut před nebo za instalace může selhat.</span><span class="sxs-lookup"><span data-stu-id="87ed7-122">If it's 15 minutes in front or behind, setup might fail.</span></span>
- <span data-ttu-id="87ed7-123">Spusťte instalační program jako místní správce na serveru, konfigurace.</span><span class="sxs-lookup"><span data-stu-id="87ed7-123">Run setup as a Local Administrator on the configuration server machine.</span></span>
- <span data-ttu-id="87ed7-124">Ujistěte se, že je povolená TLS 1.0 ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="87ed7-124">Make sure TLS 1.0 is enabled on the VM.</span></span>


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="87ed7-125">Konfigurační server lze také nainstalovat [z příkazového řádku](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="87ed7-125">The configuration server can also be installed [from the command line](http://aka.ms/installconfigsrv).</span></span>




## <a name="set-up-the-target-environment"></a><span data-ttu-id="87ed7-126">Nastavení cílového prostředí</span><span class="sxs-lookup"><span data-stu-id="87ed7-126">Set up the target environment</span></span>

<span data-ttu-id="87ed7-127">Než nastavíte cílovém prostředí, ujistěte se, že máte účet úložiště Azure a virtuální sítě nastavit.</span><span class="sxs-lookup"><span data-stu-id="87ed7-127">Before you set up the target environment, make sure you have an Azure storage account and virtual network set up.</span></span>

1. <span data-ttu-id="87ed7-128">Klikněte na **Připravit infrastrukturu** > **Cíl** a vyberte předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="87ed7-128">Click **Prepare infrastructure** > **Target**, and select the Azure subscription you want to use.</span></span>
2. <span data-ttu-id="87ed7-129">Určit, jestli je váš model nasazení cílového využívající Resource Manager a classic.</span><span class="sxs-lookup"><span data-stu-id="87ed7-129">Specify whether your target deployment model is Resource Manager-based, or classic.</span></span>
3. <span data-ttu-id="87ed7-130">Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="87ed7-130">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

   ![cíl](./media/physical-walkthrough-source-target/gs-target.png)

4. <span data-ttu-id="87ed7-132">Pokud jste dosud nevytvořili účet úložiště nebo sítě, klikněte na tlačítko **+ účet úložiště** nebo **+ síť**, abyste mohli vytvořit účet správce prostředků nebo sítě vložené.</span><span class="sxs-lookup"><span data-stu-id="87ed7-132">If you haven't created a storage account or network, click **+Storage account** or **+Network**, to create a Resource Manager account or network inline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="87ed7-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87ed7-133">Next steps</span></span>

<span data-ttu-id="87ed7-134">Přejděte na [krok 8: nastavení zásad replikace](physical-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="87ed7-134">Go to [Step 8: Set up a replication policy](physical-walkthrough-replication.md)</span></span>
