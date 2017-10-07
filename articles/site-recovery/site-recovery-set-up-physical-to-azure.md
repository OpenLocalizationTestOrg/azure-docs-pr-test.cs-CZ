---
title: "Nastavení prostředí pro zdroj hello (fyzické servery tooAzure) | Microsoft Docs"
description: "Tento článek popisuje, jak tooset si vaše místní prostředí toostart replikace fyzických serverů s Windows nebo Linuxem do Azure."
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
ms.openlocfilehash: d4702265bf36910015685d2bba99d6e577531bd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-hello-source-environment-physical-server-tooazure"></a><span data-ttu-id="93209-103">Nastavení prostředí pro zdroj hello (tooAzure fyzického serveru)</span><span class="sxs-lookup"><span data-stu-id="93209-103">Set up hello source environment (physical server tooAzure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="93209-104">VMware tooAzure</span><span class="sxs-lookup"><span data-stu-id="93209-104">VMware tooAzure</span></span>](./site-recovery-set-up-vmware-to-azure.md)
> * [<span data-ttu-id="93209-105">Fyzické tooAzure</span><span class="sxs-lookup"><span data-stu-id="93209-105">Physical tooAzure</span></span>](./site-recovery-set-up-physical-to-azure.md)

<span data-ttu-id="93209-106">Tento článek popisuje, jak tooset si vaše místní prostředí toostart replikace fyzických serverů s Windows nebo Linuxem do Azure.</span><span class="sxs-lookup"><span data-stu-id="93209-106">This article describes how tooset up your on-premises environment toostart replicating physical servers running Windows or Linux into Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="93209-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="93209-107">Prerequisites</span></span>

<span data-ttu-id="93209-108">Hello článek předpokládá, že už máte:</span><span class="sxs-lookup"><span data-stu-id="93209-108">hello article assumes that you already have:</span></span>
1. <span data-ttu-id="93209-109">Trezor služeb zotavení v hello [portál Azure](http://portal.azure.com "portál Azure").</span><span class="sxs-lookup"><span data-stu-id="93209-109">A Recovery Services vault in hello [Azure portal](http://portal.azure.com "Azure portal").</span></span>
3. <span data-ttu-id="93209-110">Fyzický počítač, na které tooinstall hello konfiguračním serveru.</span><span class="sxs-lookup"><span data-stu-id="93209-110">A physical computer on which tooinstall hello configuration server.</span></span>

### <a name="configuration-server-minimum-requirements"></a><span data-ttu-id="93209-111">Minimální požadavky na konfiguraci serveru</span><span class="sxs-lookup"><span data-stu-id="93209-111">Configuration server minimum requirements</span></span>
<span data-ttu-id="93209-112">Hello následující tabulka uvádí hello minimální požadavky na hardware, software a sítě pro konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="93209-112">hello following table lists hello minimum hardware, software, and network requirements for a configuration server.</span></span>
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> <span data-ttu-id="93209-113">Založený na protokolu HTTPS proxy servery nejsou podporovány hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="93209-113">HTTPS-based proxy servers are not supported by hello configuration server.</span></span>

## <a name="choose-your-protection-goals"></a><span data-ttu-id="93209-114">Volba cílů ochrany</span><span class="sxs-lookup"><span data-stu-id="93209-114">Choose your protection goals</span></span>

1. <span data-ttu-id="93209-115">V hello portálu Azure, přejděte toohello **služeb zotavení** trezory okno a vyberte svůj trezor.</span><span class="sxs-lookup"><span data-stu-id="93209-115">In hello Azure portal, go toohello **Recovery Services** vaults blade and select your vault.</span></span>
2. <span data-ttu-id="93209-116">V hello **prostředků** nabídky hello trezoru, klikněte na tlačítko **Začínáme** > **Site Recovery** > **krok 1: Příprava Infrastruktura** > **cíl ochrany**.</span><span class="sxs-lookup"><span data-stu-id="93209-116">In hello **Resource** menu of hello vault, click **Getting Started** > **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>

    ![Zvolte cíle.](./media/site-recovery-set-up-physical-to-azure/choose-goals.png)
3. <span data-ttu-id="93209-118">V **cíl ochrany**, vyberte **tooAzure** a **není virtualizované/jiné**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="93209-118">In **Protection goal**, select **tooAzure** and **Not virtualized/Other**, and then click **OK**.</span></span>

    ![Zvolte cíle.](./media/site-recovery-set-up-physical-to-azure/physical-protection-goal.PNG)

## <a name="set-up-hello-source-environment"></a><span data-ttu-id="93209-120">Nastavit hello zdrojové prostředí</span><span class="sxs-lookup"><span data-stu-id="93209-120">Set up hello source environment</span></span>

1. <span data-ttu-id="93209-121">V **připravit zdroj**, pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server** tooadd jeden.</span><span class="sxs-lookup"><span data-stu-id="93209-121">In **Prepare source**, if you don’t have a configuration server, click **+Configuration server** tooadd one.</span></span>

  ![Nastavení zdroje](./media/site-recovery-set-up-physical-to-azure/plus-config-srv.png)
2. <span data-ttu-id="93209-123">V hello **přidat Server** okno, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.</span><span class="sxs-lookup"><span data-stu-id="93209-123">In hello **Add Server** blade, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="93209-124">Stáhněte si soubor instalace hello Unified instalace nástroje Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="93209-124">Download hello Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="93209-125">Stáhněte si registrační klíč trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="93209-125">Download hello vault registration key.</span></span> <span data-ttu-id="93209-126">Když spustíte instalační program Unified musíte hello registrační klíč.</span><span class="sxs-lookup"><span data-stu-id="93209-126">You need hello registration key when you run Unified Setup.</span></span> <span data-ttu-id="93209-127">Hello klíč je platný pět dní od jeho vygenerování.</span><span class="sxs-lookup"><span data-stu-id="93209-127">hello key is valid for five days after you generate it.</span></span>

    ![Nastavení zdroje](./media/site-recovery-set-up-physical-to-azure/set-source2.png)
6. <span data-ttu-id="93209-129">Na počítači hello používáte jako hello konfigurační server, spusťte **Unified instalace nástroje Azure Site Recovery** tooinstall hello konfigurační server hello procesový server a hlavní hello cílí na serveru.</span><span class="sxs-lookup"><span data-stu-id="93209-129">On hello machine you’re using as hello configuration server, run **Azure Site Recovery Unified Setup** tooinstall hello configuration server, hello process server, and hello master target server.</span></span>

#### <a name="run-azure-site-recovery-unified-setup"></a><span data-ttu-id="93209-130">Spuštění Azure Site Recovery sjednocený instalační program</span><span class="sxs-lookup"><span data-stu-id="93209-130">Run Azure Site Recovery Unified Setup</span></span>

> [!TIP]
> <span data-ttu-id="93209-131">Konfigurace serveru registrace selže, pokud čas hello na systémových hodin vašeho počítače je delší než 5 minut z místního času.</span><span class="sxs-lookup"><span data-stu-id="93209-131">Configuration server registration fails if hello time on your computer's system clock is more than five minutes off of local time.</span></span> <span data-ttu-id="93209-132">Synchronizovat systémových hodin s [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) před zahájením instalace hello.</span><span class="sxs-lookup"><span data-stu-id="93209-132">Synchronize your system clock with a [time server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) before starting hello installation.</span></span>

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="93209-133">Hello konfigurační server lze nainstalovat pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="93209-133">hello configuration server can be installed via a command line.</span></span> <span data-ttu-id="93209-134">Další informace najdete v tématu [instalace konfigurační server pomocí nástroje příkazového řádku](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="93209-134">For more information, see [Installing configuration server using command-line tools](http://aka.ms/installconfigsrv).</span></span>


## <a name="common-issues"></a><span data-ttu-id="93209-135">Běžné problémy</span><span class="sxs-lookup"><span data-stu-id="93209-135">Common issues</span></span>

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a><span data-ttu-id="93209-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="93209-136">Next steps</span></span>

<span data-ttu-id="93209-137">Dalším krokem zahrnuje [nastavení cílového prostředí](./site-recovery-prepare-target-physical-to-azure.md) v Azure.</span><span class="sxs-lookup"><span data-stu-id="93209-137">Next step involves [setting up your target environment](./site-recovery-prepare-target-physical-to-azure.md) in Azure.</span></span>
