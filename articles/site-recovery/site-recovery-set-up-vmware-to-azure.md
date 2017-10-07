---
title: "Nastavení prostředí pro zdroj hello (VMware tooAzure) | Microsoft Docs"
description: "Tento článek popisuje, jak tooset si vaše místní prostředí toostart replikace VMware virtuálních počítačů tooAzure."
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
ms.openlocfilehash: cbeeffc1061b11223e12ff3e237fa7e55b999aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-hello-source-environment-vmware-tooazure"></a><span data-ttu-id="95541-103">Nastavení prostředí pro zdroj hello (VMware tooAzure)</span><span class="sxs-lookup"><span data-stu-id="95541-103">Set up hello source environment (VMware tooAzure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="95541-104">VMware tooAzure</span><span class="sxs-lookup"><span data-stu-id="95541-104">VMware tooAzure</span></span>](./site-recovery-set-up-vmware-to-azure.md)
> * [<span data-ttu-id="95541-105">Fyzické tooAzure</span><span class="sxs-lookup"><span data-stu-id="95541-105">Physical tooAzure</span></span>](./site-recovery-set-up-physical-to-azure.md)

<span data-ttu-id="95541-106">Tento článek popisuje, jak tooset si vaše místní prostředí toostart replikovat virtuální počítače běží na VMware tooAzure.</span><span class="sxs-lookup"><span data-stu-id="95541-106">This article describes how tooset up your on-premises environment toostart replicating virtual machines running on VMware tooAzure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95541-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="95541-107">Prerequisites</span></span>

<span data-ttu-id="95541-108">Hello článek předpokládá, že jste již vytvořili:</span><span class="sxs-lookup"><span data-stu-id="95541-108">hello article assumes that you have already created:</span></span>
- <span data-ttu-id="95541-109">Trezor služeb zotavení v hello [portál Azure](http://portal.azure.com "portál Azure").</span><span class="sxs-lookup"><span data-stu-id="95541-109">A Recovery Services Vault in hello [Azure portal](http://portal.azure.com "Azure portal").</span></span>
- <span data-ttu-id="95541-110">Vyhrazený účet do systému VMware vCenter, který lze použít pro [automatického zjišťování](./site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="95541-110">A dedicated account in your VMware vCenter that can be used for [automatic discovery](./site-recovery-vmware-to-azure.md).</span></span>
- <span data-ttu-id="95541-111">Virtuální počítač, na které tooinstall hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="95541-111">A virtual machine on which tooinstall hello configuration server.</span></span>

## <a name="configuration-server-minimum-requirements"></a><span data-ttu-id="95541-112">Minimální požadavky na konfiguraci serveru</span><span class="sxs-lookup"><span data-stu-id="95541-112">Configuration server minimum requirements</span></span>
<span data-ttu-id="95541-113">Hello konfigurace serverového softwaru musí být nasazené na virtuální počítače VMware s vysokou dostupností.</span><span class="sxs-lookup"><span data-stu-id="95541-113">hello configuration server software should be deployed on a highly available VMware virtual machine.</span></span> <span data-ttu-id="95541-114">Hello následující tabulka uvádí hello minimální požadavky na hardware, software a sítě pro konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="95541-114">hello following table lists hello minimum hardware, software, and network requirements for a configuration server.</span></span>
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> <span data-ttu-id="95541-115">Založený na protokolu HTTPS proxy servery nejsou podporovány hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="95541-115">HTTPS-based proxy servers are not supported by hello configuration server.</span></span>

## <a name="choose-your-protection-goals"></a><span data-ttu-id="95541-116">Volba cílů ochrany</span><span class="sxs-lookup"><span data-stu-id="95541-116">Choose your protection goals</span></span>

1. <span data-ttu-id="95541-117">V hello portálu Azure, přejděte toohello **služeb zotavení** okno trezoru a vyberte svůj trezor.</span><span class="sxs-lookup"><span data-stu-id="95541-117">In hello Azure portal, go toohello **Recovery Services** vault blade and select your vault.</span></span>
2. <span data-ttu-id="95541-118">V nabídce hello prostředků úložiště hello přejděte příliš**Začínáme** > **Site Recovery** > **krok 1: připravte infrastrukturu**  >  **Cíl ochrany**.</span><span class="sxs-lookup"><span data-stu-id="95541-118">On hello resource menu of hello vault, go too**Getting Started** > **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>

    ![Zvolte cíle.](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. <span data-ttu-id="95541-120">V **cíl ochrany**, vyberte **tooAzure**a zvolte **Ano, s hypervisoru VMware vSphere**.</span><span class="sxs-lookup"><span data-stu-id="95541-120">In **Protection goal**, select **tooAzure**, and choose **Yes, with VMware vSphere Hypervisor**.</span></span> <span data-ttu-id="95541-121">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="95541-121">Then click **OK**.</span></span>

    ![Zvolte cíle.](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-hello-source-environment"></a><span data-ttu-id="95541-123">Nastavit hello zdrojové prostředí</span><span class="sxs-lookup"><span data-stu-id="95541-123">Set up hello source environment</span></span>
<span data-ttu-id="95541-124">Nastavení hello zdrojové prostředí zahrnuje dva hlavní činnosti:</span><span class="sxs-lookup"><span data-stu-id="95541-124">Setting up hello source environment involves two main activities:</span></span>

- <span data-ttu-id="95541-125">Nainstalujte a zaregistrujte konfigurační server pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="95541-125">Install and register a configuration server with Site Recovery.</span></span>
- <span data-ttu-id="95541-126">Zjistit virtuální počítače na místní připojením Site Recovery tooyour místní VMware vCenter nebo vSphere EXSi hostitelů.</span><span class="sxs-lookup"><span data-stu-id="95541-126">Discover your on-premises virtual machines by connecting Site Recovery tooyour on-premises VMware vCenter or vSphere EXSi hosts.</span></span>

### <a name="step-1-install-and-register-a-configuration-server"></a><span data-ttu-id="95541-127">Krok 1: Instalace a zaregistrujte konfigurační server</span><span class="sxs-lookup"><span data-stu-id="95541-127">Step 1: Install and register a configuration server</span></span>

1. <span data-ttu-id="95541-128">Klikněte na tlačítko **krok 1: Příprava infrastruktury** > **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="95541-128">Click **Step 1: Prepare Infrastructure** > **Source**.</span></span> <span data-ttu-id="95541-129">V **připravit zdroj**, pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server** tooadd jeden.</span><span class="sxs-lookup"><span data-stu-id="95541-129">In **Prepare source**, if you don’t have a configuration server, click **+Configuration server** tooadd one.</span></span>

    ![Nastavení zdroje](./media/site-recovery-set-up-vmware-to-azure/set-source1.png)
2. <span data-ttu-id="95541-131">Na hello **přidat Server** okno, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.</span><span class="sxs-lookup"><span data-stu-id="95541-131">On hello **Add Server** blade, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="95541-132">Stáhněte si soubor instalace hello Unified instalace nástroje Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="95541-132">Download hello Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="95541-133">Stáhněte si registrační klíč trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="95541-133">Download hello vault registration key.</span></span> <span data-ttu-id="95541-134">Když spustíte instalační program Unified musíte hello registrační klíč.</span><span class="sxs-lookup"><span data-stu-id="95541-134">You need hello registration key when you run Unified Setup.</span></span> <span data-ttu-id="95541-135">Hello klíč je platný pět dní od jeho vygenerování.</span><span class="sxs-lookup"><span data-stu-id="95541-135">hello key is valid for five days after you generate it.</span></span>

    ![Nastavení zdroje](./media/site-recovery-set-up-vmware-to-azure/set-source2.png)
6. <span data-ttu-id="95541-137">Na počítači hello používáte jako hello konfigurační server, spusťte **Unified instalace nástroje Azure Site Recovery** tooinstall hello konfigurační server hello procesový server a hlavní hello cílí na serveru.</span><span class="sxs-lookup"><span data-stu-id="95541-137">On hello machine you’re using as hello configuration server, run **Azure Site Recovery Unified Setup** tooinstall hello configuration server, hello process server, and hello master target server.</span></span>

#### <a name="run-azure-site-recovery-unified-setup"></a><span data-ttu-id="95541-138">Spuštění Azure Site Recovery sjednocený instalační program</span><span class="sxs-lookup"><span data-stu-id="95541-138">Run Azure Site Recovery Unified Setup</span></span>

> [!TIP]
> <span data-ttu-id="95541-139">Registrace serveru konfigurace selže, pokud čas hello na váš počítač systémové hodiny se liší od místního času ve více než pět minut.</span><span class="sxs-lookup"><span data-stu-id="95541-139">Configuration server registration fails if hello time on your computer's system clock differs from local time by more than five minutes.</span></span> <span data-ttu-id="95541-140">Synchronizovat systémových hodin s [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) před zahájením instalace hello.</span><span class="sxs-lookup"><span data-stu-id="95541-140">Synchronize your system clock with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) before starting hello installation.</span></span>

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="95541-141">Hello konfigurační server lze nainstalovat pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="95541-141">hello configuration server can be installed via command line.</span></span> <span data-ttu-id="95541-142">Další informace najdete v tématu [instalace hello konfigurační server pomocí nástroje příkazového řádku](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="95541-142">For more information, see [Installing hello configuration server using Command-line tools](http://aka.ms/installconfigsrv).</span></span>

#### <a name="add-hello-vmware-account-for-automatic-discovery"></a><span data-ttu-id="95541-143">Přidat účet hello VMware pro automatické zjišťování</span><span class="sxs-lookup"><span data-stu-id="95541-143">Add hello VMware account for automatic discovery</span></span>

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="step-2-add-a-vcenter"></a><span data-ttu-id="95541-144">Krok 2: Přidání systému vCenter</span><span class="sxs-lookup"><span data-stu-id="95541-144">Step 2: Add a vCenter</span></span>
<span data-ttu-id="95541-145">tooallow Azure Site Recovery toodiscover virtuální počítače běžící na vašem místním prostředí, musíte tooconnect serveru VMware vCenter nebo hostitelů vSphere ESXi službou Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="95541-145">tooallow Azure Site Recovery toodiscover virtual machines running in your on-premises environment, you need tooconnect your VMware vCenter Server or vSphere ESXi hosts with Site Recovery.</span></span>

<span data-ttu-id="95541-146">Vyberte **+ vCenter** toostart připojuje VMware vCenter server nebo hostiteli ESXi VMware vSphere.</span><span class="sxs-lookup"><span data-stu-id="95541-146">Select **+vCenter** toostart connecting a VMware vCenter server or a VMware vSphere ESXi host.</span></span>

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a><span data-ttu-id="95541-147">Běžné problémy</span><span class="sxs-lookup"><span data-stu-id="95541-147">Common issues</span></span>
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a><span data-ttu-id="95541-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="95541-148">Next steps</span></span>
<span data-ttu-id="95541-149">[Nastavení cílového prostředí](./site-recovery-prepare-target-vmware-to-azure.md) v Azure.</span><span class="sxs-lookup"><span data-stu-id="95541-149">[Set up your target environment](./site-recovery-prepare-target-vmware-to-azure.md) in Azure.</span></span>
