---
title: "Nastavení prostředí pro zdroj (fyzických serverů do Azure) | Microsoft Docs"
description: "Tento článek popisuje, jak nastavit v místním prostředí k zahájení replikace fyzických serverů s Windows nebo Linuxem do Azure."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 49b9d2e21dbcb612828a25f21ed4382327d6f64c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-the-source-environment-physical-server-to-azure"></a><span data-ttu-id="68618-103">Nastavení prostředí pro zdroj (fyzického serveru do Azure)</span><span class="sxs-lookup"><span data-stu-id="68618-103">Set up the source environment (physical server to Azure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="68618-104">Z VMware do Azure</span><span class="sxs-lookup"><span data-stu-id="68618-104">VMware to Azure</span></span>](./site-recovery-set-up-vmware-to-azure.md)
> * [<span data-ttu-id="68618-105">Fyzické do Azure</span><span class="sxs-lookup"><span data-stu-id="68618-105">Physical to Azure</span></span>](./site-recovery-set-up-physical-to-azure.md)

<span data-ttu-id="68618-106">Tento článek popisuje, jak nastavit v místním prostředí k zahájení replikace fyzických serverů s Windows nebo Linuxem do Azure.</span><span class="sxs-lookup"><span data-stu-id="68618-106">This article describes how to set up your on-premises environment to start replicating physical servers running Windows or Linux into Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="68618-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="68618-107">Prerequisites</span></span>

<span data-ttu-id="68618-108">Článek předpokládá, že už máte:</span><span class="sxs-lookup"><span data-stu-id="68618-108">The article assumes that you already have:</span></span>
1. <span data-ttu-id="68618-109">Trezor služeb zotavení v [portál Azure](http://portal.azure.com "portál Azure").</span><span class="sxs-lookup"><span data-stu-id="68618-109">A Recovery Services vault in the [Azure portal](http://portal.azure.com "Azure portal").</span></span>
3. <span data-ttu-id="68618-110">Fyzický počítač, na které se mají nainstalovat konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="68618-110">A physical computer on which to install the configuration server.</span></span>

### <a name="configuration-server-minimum-requirements"></a><span data-ttu-id="68618-111">Minimální požadavky na konfiguraci serveru</span><span class="sxs-lookup"><span data-stu-id="68618-111">Configuration server minimum requirements</span></span>
<span data-ttu-id="68618-112">Následující tabulka uvádí minimální hardwaru, softwaru a požadavky sítě pro konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="68618-112">The following table lists the minimum hardware, software, and network requirements for a configuration server.</span></span>
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> <span data-ttu-id="68618-113">Servery proxy server HTTPS nejsou podporovány konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="68618-113">HTTPS-based proxy servers are not supported by the configuration server.</span></span>

## <a name="choose-your-protection-goals"></a><span data-ttu-id="68618-114">Volba cílů ochrany</span><span class="sxs-lookup"><span data-stu-id="68618-114">Choose your protection goals</span></span>

1. <span data-ttu-id="68618-115">V portálu Azure přejděte do **služeb zotavení** trezory okno a vyberte svůj trezor.</span><span class="sxs-lookup"><span data-stu-id="68618-115">In the Azure portal, go to the **Recovery Services** vaults blade and select your vault.</span></span>
2. <span data-ttu-id="68618-116">V **prostředků** nabídce trezoru, klikněte na tlačítko **Začínáme** > **Site Recovery** > **krok 1: připravte infrastrukturu** > **cíl ochrany**.</span><span class="sxs-lookup"><span data-stu-id="68618-116">In the **Resource** menu of the vault, click **Getting Started** > **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>

    ![Zvolte cíle.](./media/site-recovery-set-up-physical-to-azure/choose-goals.png)
3. <span data-ttu-id="68618-118">V **cíl ochrany**, vyberte **do Azure** a **není virtualizované/jiné**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="68618-118">In **Protection goal**, select **To Azure** and **Not virtualized/Other**, and then click **OK**.</span></span>

    ![Zvolte cíle.](./media/site-recovery-set-up-physical-to-azure/physical-protection-goal.PNG)

## <a name="set-up-the-source-environment"></a><span data-ttu-id="68618-120">Nastavení zdrojového prostředí</span><span class="sxs-lookup"><span data-stu-id="68618-120">Set up the source environment</span></span>

1. <span data-ttu-id="68618-121">V **připravit zdroj**, pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server** aby vám ho přidal.</span><span class="sxs-lookup"><span data-stu-id="68618-121">In **Prepare source**, if you don’t have a configuration server, click **+Configuration server** to add one.</span></span>

  ![Nastavení zdroje](./media/site-recovery-set-up-physical-to-azure/plus-config-srv.png)
2. <span data-ttu-id="68618-123">V **přidat Server** okno, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.</span><span class="sxs-lookup"><span data-stu-id="68618-123">In the **Add Server** blade, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="68618-124">Stáhněte instalační soubor nástroje Unified instalace nástroje Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="68618-124">Download the Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="68618-125">Stáhnout registrační klíč trezoru</span><span class="sxs-lookup"><span data-stu-id="68618-125">Download the vault registration key.</span></span> <span data-ttu-id="68618-126">Když spustíte instalační program Unified musíte registrační klíč.</span><span class="sxs-lookup"><span data-stu-id="68618-126">You need the registration key when you run Unified Setup.</span></span> <span data-ttu-id="68618-127">Klíč je platný pět dní od jeho vygenerování.</span><span class="sxs-lookup"><span data-stu-id="68618-127">The key is valid for five days after you generate it.</span></span>

    ![Nastavení zdroje](./media/site-recovery-set-up-physical-to-azure/set-source2.png)
6. <span data-ttu-id="68618-129">Na počítači, který používáte jako konfigurační server, spusťte **Unified instalace nástroje Azure Site Recovery** instalace konfigurační server, procesový server a hlavní cílový server.</span><span class="sxs-lookup"><span data-stu-id="68618-129">On the machine you’re using as the configuration server, run **Azure Site Recovery Unified Setup** to install the configuration server, the process server, and the master target server.</span></span>

#### <a name="run-azure-site-recovery-unified-setup"></a><span data-ttu-id="68618-130">Spuštění Azure Site Recovery sjednocený instalační program</span><span class="sxs-lookup"><span data-stu-id="68618-130">Run Azure Site Recovery Unified Setup</span></span>

> [!TIP]
> <span data-ttu-id="68618-131">Konfigurace serveru registrace selže, pokud čas na systémových hodin vašeho počítače je delší než 5 minut z místního času.</span><span class="sxs-lookup"><span data-stu-id="68618-131">Configuration server registration fails if the time on your computer's system clock is more than five minutes off of local time.</span></span> <span data-ttu-id="68618-132">Synchronizovat systémových hodin s [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) před zahájením instalace.</span><span class="sxs-lookup"><span data-stu-id="68618-132">Synchronize your system clock with a [time server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) before starting the installation.</span></span>

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="68618-133">Konfigurační server lze nainstalovat pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="68618-133">The configuration server can be installed via a command line.</span></span> <span data-ttu-id="68618-134">Další informace najdete v tématu [instalace konfigurační server pomocí nástroje příkazového řádku](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="68618-134">For more information, see [Installing configuration server using command-line tools](http://aka.ms/installconfigsrv).</span></span>


## <a name="common-issues"></a><span data-ttu-id="68618-135">Běžné problémy</span><span class="sxs-lookup"><span data-stu-id="68618-135">Common issues</span></span>

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a><span data-ttu-id="68618-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="68618-136">Next steps</span></span>

<span data-ttu-id="68618-137">Dalším krokem zahrnuje [nastavení cílového prostředí](./site-recovery-prepare-target-physical-to-azure.md) v Azure.</span><span class="sxs-lookup"><span data-stu-id="68618-137">Next step involves [setting up your target environment](./site-recovery-prepare-target-physical-to-azure.md) in Azure.</span></span>
