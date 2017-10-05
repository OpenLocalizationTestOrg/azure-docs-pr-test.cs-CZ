---
title: "Nastavení prostředí pro zdroj (VMware do Azure) | Microsoft Docs"
description: "Tento článek popisuje, jak nastavit prostředí místní spuštění replikace virtuálních počítačů VMware do Azure."
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
ms.openlocfilehash: a2fabc56463c8cbf0b8a76b7a84369ed8e535486
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="set-up-the-source-environment-vmware-to-azure"></a><span data-ttu-id="106c1-103">Nastavení prostředí pro zdroj (VMware do Azure)</span><span class="sxs-lookup"><span data-stu-id="106c1-103">Set up the source environment (VMware to Azure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="106c1-104">Z VMware do Azure</span><span class="sxs-lookup"><span data-stu-id="106c1-104">VMware to Azure</span></span>](./site-recovery-set-up-vmware-to-azure.md)
> * [<span data-ttu-id="106c1-105">Fyzické do Azure</span><span class="sxs-lookup"><span data-stu-id="106c1-105">Physical to Azure</span></span>](./site-recovery-set-up-physical-to-azure.md)

<span data-ttu-id="106c1-106">Tento článek popisuje, jak nastavit v místním prostředí k zahájení replikace virtuálních počítačů spuštěných na VMware do Azure.</span><span class="sxs-lookup"><span data-stu-id="106c1-106">This article describes how to set up your on-premises environment to start replicating virtual machines running on VMware to Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="106c1-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="106c1-107">Prerequisites</span></span>

<span data-ttu-id="106c1-108">Článek předpokládá, že jste již vytvořili:</span><span class="sxs-lookup"><span data-stu-id="106c1-108">The article assumes that you have already created:</span></span>
- <span data-ttu-id="106c1-109">Trezor služeb zotavení v [portál Azure](http://portal.azure.com "portál Azure").</span><span class="sxs-lookup"><span data-stu-id="106c1-109">A Recovery Services Vault in the [Azure portal](http://portal.azure.com "Azure portal").</span></span>
- <span data-ttu-id="106c1-110">Vyhrazený účet do systému VMware vCenter, který lze použít pro [automatického zjišťování](./site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="106c1-110">A dedicated account in your VMware vCenter that can be used for [automatic discovery](./site-recovery-vmware-to-azure.md).</span></span>
- <span data-ttu-id="106c1-111">Virtuální počítač, na které se mají nainstalovat konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="106c1-111">A virtual machine on which to install the configuration server.</span></span>

## <a name="configuration-server-minimum-requirements"></a><span data-ttu-id="106c1-112">Minimální požadavky na konfiguraci serveru</span><span class="sxs-lookup"><span data-stu-id="106c1-112">Configuration server minimum requirements</span></span>
<span data-ttu-id="106c1-113">Konfigurace serverového softwaru musí být nasazené na virtuální počítače VMware s vysokou dostupností.</span><span class="sxs-lookup"><span data-stu-id="106c1-113">The configuration server software should be deployed on a highly available VMware virtual machine.</span></span> <span data-ttu-id="106c1-114">Následující tabulka uvádí minimální hardwaru, softwaru a požadavky sítě pro konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="106c1-114">The following table lists the minimum hardware, software, and network requirements for a configuration server.</span></span>
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> <span data-ttu-id="106c1-115">Servery proxy server HTTPS nejsou podporovány konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="106c1-115">HTTPS-based proxy servers are not supported by the configuration server.</span></span>

## <a name="choose-your-protection-goals"></a><span data-ttu-id="106c1-116">Volba cílů ochrany</span><span class="sxs-lookup"><span data-stu-id="106c1-116">Choose your protection goals</span></span>

1. <span data-ttu-id="106c1-117">V portálu Azure přejděte do **služeb zotavení** okno trezoru a vyberte svůj trezor.</span><span class="sxs-lookup"><span data-stu-id="106c1-117">In the Azure portal, go to the **Recovery Services** vault blade and select your vault.</span></span>
2. <span data-ttu-id="106c1-118">V nabídce prostředků úložiště, přejděte na **Začínáme** > **Site Recovery** > **krok 1: připravte infrastrukturu** > **cíl ochrany**.</span><span class="sxs-lookup"><span data-stu-id="106c1-118">On the resource menu of the vault, go to **Getting Started** > **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>

    ![Zvolte cíle.](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. <span data-ttu-id="106c1-120">V **cíl ochrany**, vyberte **do Azure**a zvolte **Ano, s hypervisoru VMware vSphere**.</span><span class="sxs-lookup"><span data-stu-id="106c1-120">In **Protection goal**, select **To Azure**, and choose **Yes, with VMware vSphere Hypervisor**.</span></span> <span data-ttu-id="106c1-121">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="106c1-121">Then click **OK**.</span></span>

    ![Zvolte cíle.](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-source-environment"></a><span data-ttu-id="106c1-123">Nastavení zdrojového prostředí</span><span class="sxs-lookup"><span data-stu-id="106c1-123">Set up the source environment</span></span>
<span data-ttu-id="106c1-124">Nastavení zdrojového prostředí zahrnuje dva hlavní činnosti:</span><span class="sxs-lookup"><span data-stu-id="106c1-124">Setting up the source environment involves two main activities:</span></span>

- <span data-ttu-id="106c1-125">Nainstalujte a zaregistrujte konfigurační server pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="106c1-125">Install and register a configuration server with Site Recovery.</span></span>
- <span data-ttu-id="106c1-126">Zjistit virtuální počítače na místní připojením Site Recovery na vaše místní VMware vCenter nebo vSphere EXSi hostitele.</span><span class="sxs-lookup"><span data-stu-id="106c1-126">Discover your on-premises virtual machines by connecting Site Recovery to your on-premises VMware vCenter or vSphere EXSi hosts.</span></span>

### <a name="step-1-install-and-register-a-configuration-server"></a><span data-ttu-id="106c1-127">Krok 1: Instalace a zaregistrujte konfigurační server</span><span class="sxs-lookup"><span data-stu-id="106c1-127">Step 1: Install and register a configuration server</span></span>

1. <span data-ttu-id="106c1-128">Klikněte na tlačítko **krok 1: Příprava infrastruktury** > **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="106c1-128">Click **Step 1: Prepare Infrastructure** > **Source**.</span></span> <span data-ttu-id="106c1-129">V **připravit zdroj**, pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server** aby vám ho přidal.</span><span class="sxs-lookup"><span data-stu-id="106c1-129">In **Prepare source**, if you don’t have a configuration server, click **+Configuration server** to add one.</span></span>

    ![Nastavení zdroje](./media/site-recovery-set-up-vmware-to-azure/set-source1.png)
2. <span data-ttu-id="106c1-131">Na **přidat Server** okno, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.</span><span class="sxs-lookup"><span data-stu-id="106c1-131">On the **Add Server** blade, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="106c1-132">Stáhněte instalační soubor nástroje Unified instalace nástroje Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="106c1-132">Download the Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="106c1-133">Stáhnout registrační klíč trezoru</span><span class="sxs-lookup"><span data-stu-id="106c1-133">Download the vault registration key.</span></span> <span data-ttu-id="106c1-134">Když spustíte instalační program Unified musíte registrační klíč.</span><span class="sxs-lookup"><span data-stu-id="106c1-134">You need the registration key when you run Unified Setup.</span></span> <span data-ttu-id="106c1-135">Klíč je platný pět dní od jeho vygenerování.</span><span class="sxs-lookup"><span data-stu-id="106c1-135">The key is valid for five days after you generate it.</span></span>

    ![Nastavení zdroje](./media/site-recovery-set-up-vmware-to-azure/set-source2.png)
6. <span data-ttu-id="106c1-137">Na počítači, který používáte jako konfigurační server, spusťte **Unified instalace nástroje Azure Site Recovery** instalace konfigurační server, procesový server a hlavní cílový server.</span><span class="sxs-lookup"><span data-stu-id="106c1-137">On the machine you’re using as the configuration server, run **Azure Site Recovery Unified Setup** to install the configuration server, the process server, and the master target server.</span></span>

#### <a name="run-azure-site-recovery-unified-setup"></a><span data-ttu-id="106c1-138">Spuštění Azure Site Recovery sjednocený instalační program</span><span class="sxs-lookup"><span data-stu-id="106c1-138">Run Azure Site Recovery Unified Setup</span></span>

> [!TIP]
> <span data-ttu-id="106c1-139">Konfigurace serveru registrace selže, pokud čas počítače systémové hodiny se liší od místního času ve více než pět minut.</span><span class="sxs-lookup"><span data-stu-id="106c1-139">Configuration server registration fails if the time on your computer's system clock differs from local time by more than five minutes.</span></span> <span data-ttu-id="106c1-140">Synchronizovat systémových hodin s [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) před zahájením instalace.</span><span class="sxs-lookup"><span data-stu-id="106c1-140">Synchronize your system clock with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) before starting the installation.</span></span>

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="106c1-141">Konfigurační server lze nainstalovat pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="106c1-141">The configuration server can be installed via command line.</span></span> <span data-ttu-id="106c1-142">Další informace najdete v tématu [instalace serveru konfigurace pomocí nástroje příkazového řádku](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="106c1-142">For more information, see [Installing the configuration server using Command-line tools](http://aka.ms/installconfigsrv).</span></span>

#### <a name="add-the-vmware-account-for-automatic-discovery"></a><span data-ttu-id="106c1-143">Přidejte účet VMware pro automatické zjišťování</span><span class="sxs-lookup"><span data-stu-id="106c1-143">Add the VMware account for automatic discovery</span></span>

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="step-2-add-a-vcenter"></a><span data-ttu-id="106c1-144">Krok 2: Přidání systému vCenter</span><span class="sxs-lookup"><span data-stu-id="106c1-144">Step 2: Add a vCenter</span></span>
<span data-ttu-id="106c1-145">Povolit Azure Site Recovery se zjistit virtuální počítače spuštěné v místním prostředí, kterou potřebujete připojit VMware vCenter Server nebo hostitelů vSphere ESXi službou Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="106c1-145">To allow Azure Site Recovery to discover virtual machines running in your on-premises environment, you need to connect your VMware vCenter Server or vSphere ESXi hosts with Site Recovery.</span></span>

<span data-ttu-id="106c1-146">Vyberte **+ vCenter** zahájíte připojení serveru VMware vCenter server nebo hostiteli ESXi VMware vSphere.</span><span class="sxs-lookup"><span data-stu-id="106c1-146">Select **+vCenter** to start connecting a VMware vCenter server or a VMware vSphere ESXi host.</span></span>

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a><span data-ttu-id="106c1-147">Běžné problémy</span><span class="sxs-lookup"><span data-stu-id="106c1-147">Common issues</span></span>
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a><span data-ttu-id="106c1-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="106c1-148">Next steps</span></span>
<span data-ttu-id="106c1-149">[Nastavení cílového prostředí](./site-recovery-prepare-target-vmware-to-azure.md) v Azure.</span><span class="sxs-lookup"><span data-stu-id="106c1-149">[Set up your target environment](./site-recovery-prepare-target-vmware-to-azure.md) in Azure.</span></span>
