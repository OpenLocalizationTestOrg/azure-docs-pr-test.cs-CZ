---
title: "aaaPrepare počítače tooset až zotavení po havárii mezi oblastmi Azure po migraci tooAzure pomocí Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak tooprepare počítačů tooset až zotavení po havárii mezi oblastmi Azure po migraci tooAzure pomocí Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: ponatara
ms.openlocfilehash: b6274e3df210c1d8a7b8289cc85868ee6414e523
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-tooanother-region-after-migration-tooazure-by-using-azure-site-recovery"></a><span data-ttu-id="a2bb3-103">Replikovat virtuální počítače Azure tooanother oblast po migraci tooAzure pomocí Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="a2bb3-103">Replicate Azure VMs tooanother region after migration tooAzure by using Azure Site Recovery</span></span>

>[!NOTE]
> <span data-ttu-id="a2bb3-104">Azure Site Recovery replikaci pro virtuální počítače Azure (VM) je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-104">Azure Site Recovery replication for Azure virtual machines (VMs) is currently in preview.</span></span>

## <a name="overview"></a><span data-ttu-id="a2bb3-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="a2bb3-105">Overview</span></span>

<span data-ttu-id="a2bb3-106">Tento článek vám pomůže připravit virtuální počítače Azure pro replikaci mezi dvěma oblastmi Azure po migraci těchto počítačů z prostředí tooAzure místní pomocí Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-106">This article helps you prepare Azure virtual machines for replication between two Azure regions after these machines have been migrated from an on-premises environment tooAzure by using Azure Site Recovery.</span></span>

## <a name="disaster-recovery-and-compliance"></a><span data-ttu-id="a2bb3-107">Zotavení po havárii a dodržování předpisů</span><span class="sxs-lookup"><span data-stu-id="a2bb3-107">Disaster recovery and compliance</span></span>
<span data-ttu-id="a2bb3-108">V současné době jsou více podniky přesunutí jejich tooAzure úlohy.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-108">Today, more and more enterprises are moving their workloads tooAzure.</span></span> <span data-ttu-id="a2bb3-109">S podniky přesunutí důležité místní produkční tooAzure úlohy, nastavení služby zotavení po havárii pro tyto úlohy je povinné pro dodržování předpisů a toosafeguard proti tak možnost narušení v oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-109">With enterprises moving mission-critical on-premises production workloads tooAzure, setting up disaster recovery for these workloads is mandatory for compliance and toosafeguard against any disruptions in an Azure region.</span></span>

## <a name="steps-for-preparing-migrated-machines-for-replication"></a><span data-ttu-id="a2bb3-110">Kroky pro přípravu migrované počítače pro replikaci</span><span class="sxs-lookup"><span data-stu-id="a2bb3-110">Steps for preparing migrated machines for replication</span></span>
<span data-ttu-id="a2bb3-111">tooprepare migrovali počítače pro nastavení replikace tooanother oblast Azure:</span><span class="sxs-lookup"><span data-stu-id="a2bb3-111">tooprepare migrated machines for setting up replication tooanother Azure region:</span></span>

1. <span data-ttu-id="a2bb3-112">Dokončení migrace.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-112">Complete migration.</span></span>
2. <span data-ttu-id="a2bb3-113">Nainstalujte hello agent Azure v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-113">Install hello Azure agent if needed.</span></span>
3. <span data-ttu-id="a2bb3-114">Odeberte služby Mobility hello.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-114">Remove hello Mobility service.</span></span>  
4. <span data-ttu-id="a2bb3-115">Restartujte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-115">Restart hello VM.</span></span>

<span data-ttu-id="a2bb3-116">Tyto kroky jsou popsané v podrobněji hello následující části.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-116">These steps are described in more detail in hello following sections.</span></span>

### <a name="step-1-migrate-workloads-running-on-hyper-v-vms-vmware-vms-and-physical-servers-toorun-on-azure-vms"></a><span data-ttu-id="a2bb3-117">Krok 1: Migrace úloh běžících na virtuálních počítačů Hyper-V, virtuálních počítačů VMware a fyzické servery toorun na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="a2bb3-117">Step 1: Migrate workloads running on Hyper-V VMs, VMware VMs, and physical servers toorun on Azure VMs</span></span>

<span data-ttu-id="a2bb3-118">tooset až replikace a migrace vaší místní technologie Hyper-V, VMware a fyzické úlohy tooAzure, postupujte podle hello kroky v hello [virtuální počítače Azure IaaS migraci mezi oblastmi Azure s Azure Site Recovery](site-recovery-migrate-to-azure.md) článku.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-118">tooset up replication and migrate your on-premises Hyper-V, VMware, and physical workloads tooAzure, follow hello steps in hello [Migrate Azure IaaS virtual machines between Azure regions with Azure Site Recovery](site-recovery-migrate-to-azure.md) article.</span></span> 

<span data-ttu-id="a2bb3-119">Po migraci není nutné toocommit nebo odstranit převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-119">After migration, you don't need toocommit or delete a failover.</span></span> <span data-ttu-id="a2bb3-120">Místo toho vyberte hello **dokončení migrace** možnost pro každý počítač má toomigrate:</span><span class="sxs-lookup"><span data-stu-id="a2bb3-120">Instead, select hello **Complete Migration** option for each machine you want toomigrate:</span></span>
1. <span data-ttu-id="a2bb3-121">V **replikované položky**, klikněte pravým tlačítkem na hello virtuálních počítačů a klikněte na **dokončení migrace**.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-121">In **Replicated Items**, right-click hello VM, and click **Complete Migration**.</span></span> <span data-ttu-id="a2bb3-122">Klikněte na tlačítko **OK** toocomplete hello krok.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-122">Click **OK** toocomplete hello step.</span></span> <span data-ttu-id="a2bb3-123">Můžete sledovat průběh ve vlastnostech virtuálního počítače hello sledováním hello úlohy dokončení migrace v **úlohy Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-123">You can track progress in hello VM properties by monitoring hello Complete Migration job in **Site Recovery jobs**.</span></span>
2. <span data-ttu-id="a2bb3-124">Hello **dokončení migrace** akci dokončení procesu migrace hello, odebere replikaci pro počítač hello a zastaví Site Recovery fakturace pro počítač hello.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-124">hello **Complete Migration** action completes hello migration process, removes replication for hello machine, and stops Site Recovery billing for hello machine.</span></span>

   ![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

### <a name="step-2-install-hello-azure-vm-agent-on-hello-virtual-machine"></a><span data-ttu-id="a2bb3-126">Krok 2: Nainstalujte agenta virtuálního počítače Azure hello hello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="a2bb3-126">Step 2: Install hello Azure VM agent on hello virtual machine</span></span>
<span data-ttu-id="a2bb3-127">Hello Azure [agenta virtuálního počítače](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) musí být nainstalován na hello virtuálního počítače pro toowork rozšíření hello Site Recovery a toohelp chránit hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-127">hello Azure [VM agent](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) must be installed on hello virtual machine for hello Site Recovery extension toowork and toohelp protect hello VM.</span></span>

>[!IMPORTANT]
><span data-ttu-id="a2bb3-128">Počínaje verzí 9.7.0.0, na virtuální počítače s Windows, instalační program služby Mobility hello nainstaluje hello nejnovější dostupné agenta virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-128">Beginning with version 9.7.0.0, on Windows virtual machines, hello Mobility service installer also installs hello latest available Azure VM agent.</span></span> <span data-ttu-id="a2bb3-129">Na migraci hello virtuální počítač splňuje instalace agenta požadované pro používání všech rozšíření virtuálního počítače, včetně přípony hello Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-129">On migration, hello virtual machine meets the agent installation prerequisite for using any VM extension, including hello Site Recovery extension.</span></span> <span data-ttu-id="a2bb3-130">Hello virtuálního počítače Azure agenta potřebám toobe ručně nainstalovat jenom v případě, že hello služby Mobility na hello migrovaný počítač je verze 9,6 nebo starším.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-130">hello Azure VM agent needs toobe manually installed only if hello Mobility service installed on hello migrated machine is version 9.6 or earlier.</span></span>

<span data-ttu-id="a2bb3-131">Hello následující tabulka obsahuje další informace o instalaci agenta hello virtuálního počítače a ověřuje, zda byl nainstalován:</span><span class="sxs-lookup"><span data-stu-id="a2bb3-131">hello following table provides additional information about installing hello VM agent and validating that it was installed:</span></span>

| <span data-ttu-id="a2bb3-132">**Operace**</span><span class="sxs-lookup"><span data-stu-id="a2bb3-132">**Operation**</span></span> | <span data-ttu-id="a2bb3-133">**Windows**</span><span class="sxs-lookup"><span data-stu-id="a2bb3-133">**Windows**</span></span> | <span data-ttu-id="a2bb3-134">**Linux**</span><span class="sxs-lookup"><span data-stu-id="a2bb3-134">**Linux**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a2bb3-135">Instalace agenta virtuálního počítače hello</span><span class="sxs-lookup"><span data-stu-id="a2bb3-135">Installing hello VM agent</span></span> |<span data-ttu-id="a2bb3-136">Stáhněte a nainstalujte hello [MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="a2bb3-136">Download and install hello [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).</span></span> <span data-ttu-id="a2bb3-137">Potřebujete instalaci hello toocomplete oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-137">You need administrator privileges toocomplete hello installation.</span></span> |<span data-ttu-id="a2bb3-138">Nainstalujte nejnovější hello [agenta systému Linux](../virtual-machines/linux/agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="a2bb3-138">Install hello latest [Linux agent](../virtual-machines/linux/agent-user-guide.md).</span></span> <span data-ttu-id="a2bb3-139">Potřebujete instalaci hello toocomplete oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-139">You need administrator privileges toocomplete hello installation.</span></span> <span data-ttu-id="a2bb3-140">Doporučujeme nainstalovat agenta hello z distribuční úložiště.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-140">We recommend installing hello agent from your distribution repository.</span></span> <span data-ttu-id="a2bb3-141">Jsme *nedoporučujeme* instalaci hello agenta virtuálního počítače s Linuxem přímo z Githubu.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-141">We *do not recommend* installing hello Linux VM agent directly from GitHub.</span></span>  |
| <span data-ttu-id="a2bb3-142">Ověření instalace agenta virtuálního počítače hello</span><span class="sxs-lookup"><span data-stu-id="a2bb3-142">Validating hello VM agent installation</span></span> |<span data-ttu-id="a2bb3-143">1. Vyhledejte složku C:\WindowsAzure\Packages toohello hello virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-143">1. Browse toohello C:\WindowsAzure\Packages folder in hello Azure VM.</span></span> <span data-ttu-id="a2bb3-144">Měli byste vidět hello přítomný soubor WaAppAgent.exe.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-144">You should see hello WaAppAgent.exe file.</span></span> <br><span data-ttu-id="a2bb3-145">2. Klikněte pravým tlačítkem na soubor hello, přejděte příliš**vlastnosti**a potom vyberte hello **podrobnosti** kartě hello **verze produktu** pole by mělo být 2.6.1198.718 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-145">2. Right-click hello file, go too**Properties**, and then select hello **Details** tab. hello **Product Version** field should be 2.6.1198.718 or higher.</span></span> |<span data-ttu-id="a2bb3-146">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="a2bb3-146">N/A</span></span> |


### <a name="step-3-remove-hello-mobility-service-from-hello-migrated-virtual-machine"></a><span data-ttu-id="a2bb3-147">Krok 3: Odeberte hello služby Mobility ze hello migrovaný virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="a2bb3-147">Step 3: Remove hello Mobility service from hello migrated virtual machine</span></span>

<span data-ttu-id="a2bb3-148">Pokud jste migrovali místní počítače VMware nebo fyzických serverů v systému Windows nebo Linux, je třeba toomanually odebrat nebo odinstalace hello služby Mobility ze hello migrovaný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-148">If you have migrated your on-premises VMware machines or physical servers on Windows/Linux, you need toomanually remove/uninstall hello Mobility service from hello migrated virtual machine.</span></span>

>[!IMPORTANT]
><span data-ttu-id="a2bb3-149">Tento krok není potřeba tooAzure migrovaných virtuálních počítačů Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-149">This step is not required for Hyper-V VMs migrated tooAzure.</span></span>

#### <a name="uninstall-hello-mobility-service-on-a-windows-server-vm"></a><span data-ttu-id="a2bb3-150">Odinstalujte službu Mobility hello na virtuální počítač Windows serveru</span><span class="sxs-lookup"><span data-stu-id="a2bb3-150">Uninstall hello Mobility service on a Windows Server VM</span></span>
<span data-ttu-id="a2bb3-151">Použijte jeden z následujících metod toouninstall hello Mobility service na počítači s Windows serverem hello.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-151">Use one of hello following methods toouninstall hello Mobility service on a Windows Server computer.</span></span>

##### <a name="uninstall-by-using-hello-windows-ui"></a><span data-ttu-id="a2bb3-152">Odinstalovat aplikaci pomocí uživatelského rozhraní Windows hello</span><span class="sxs-lookup"><span data-stu-id="a2bb3-152">Uninstall by using hello Windows UI</span></span>
1. <span data-ttu-id="a2bb3-153">V hello ovládacích panelů, vyberte **programy**.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-153">In hello Control Panel, select **Programs**.</span></span>
2. <span data-ttu-id="a2bb3-154">Vyberte **Microsoft Azure Site Recovery Mobility služby nebo hlavní cílový server**a potom vyberte **odinstalovat**.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-154">Select **Microsoft Azure Site Recovery Mobility Service/Master Target server**, and then select **Uninstall**.</span></span>

##### <a name="uninstall-at-a-command-prompt"></a><span data-ttu-id="a2bb3-155">Odinstalujte na příkazovém řádku</span><span class="sxs-lookup"><span data-stu-id="a2bb3-155">Uninstall at a command prompt</span></span>
1. <span data-ttu-id="a2bb3-156">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-156">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="a2bb3-157">toouninstall hello služby Mobility, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="a2bb3-157">toouninstall hello Mobility service, run hello following command:</span></span>

   ```
   MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
   ```

#### <a name="uninstall-hello-mobility-service-on-a-linux-computer"></a><span data-ttu-id="a2bb3-158">Odinstalujte službu Mobility hello na počítač se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="a2bb3-158">Uninstall hello Mobility service on a Linux computer</span></span>
1. <span data-ttu-id="a2bb3-159">Na serveru systému Linux, přihlaste se jako **kořenové** uživatele.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-159">On your Linux server, sign in as a **root** user.</span></span>
2. <span data-ttu-id="a2bb3-160">V terminálu přejděte příliš/uživatel/místní nebo automatické obnovení systému.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-160">In a terminal, go too/user/local/ASR.</span></span>
3. <span data-ttu-id="a2bb3-161">toouninstall hello služby Mobility, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="a2bb3-161">toouninstall hello Mobility service, run hello following command:</span></span>

   ```
   uninstall.sh -Y
   ```

### <a name="step-4-restart-hello-vm"></a><span data-ttu-id="a2bb3-162">Krok 4: Restartování hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a2bb3-162">Step 4: Restart hello VM</span></span>

<span data-ttu-id="a2bb3-163">Po odinstalování služby Mobility hello nastavit hello restartování virtuálního počítače předtím, než replikace tooanother oblast Azure.</span><span class="sxs-lookup"><span data-stu-id="a2bb3-163">After you uninstall hello Mobility service, restart hello VM before you set up replication tooanother Azure region.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a2bb3-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a2bb3-164">Next steps</span></span>
- <span data-ttu-id="a2bb3-165">Začněte chránit vaše úlohy [replikovat virtuální počítače Azure](site-recovery-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="a2bb3-165">Start protecting your workloads by [replicating Azure virtual machines](site-recovery-azure-to-azure.md).</span></span>
- <span data-ttu-id="a2bb3-166">Další informace o [sítě pokyny pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="a2bb3-166">Learn more about [networking guidance for replicating Azure virtual machines](site-recovery-azure-to-azure-networking-guidance.md).</span></span>
