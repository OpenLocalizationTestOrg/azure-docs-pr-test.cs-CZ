---
title: "Příprava počítačů k nastavení zotavení po havárii mezi oblastmi Azure po migraci na Azure pomocí Site Recovery | Microsoft Docs"
description: "Tento článek popisuje postup přípravy počítače, které chcete nastavit zotavení po havárii mezi oblastmi Azure po migraci na Azure pomocí Azure Site Recovery."
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
ms.openlocfilehash: 2aee0fb8d1ba1ff1584bee91b4d1cc34b654d97f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-azure-vms-to-another-region-after-migration-to-azure-by-using-azure-site-recovery"></a><span data-ttu-id="6d9ae-103">Replikace virtuálních počítačů Azure do jiné oblasti po migraci na Azure pomocí Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="6d9ae-103">Replicate Azure VMs to another region after migration to Azure by using Azure Site Recovery</span></span>

>[!NOTE]
> <span data-ttu-id="6d9ae-104">Azure Site Recovery replikaci pro virtuální počítače Azure (VM) je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-104">Azure Site Recovery replication for Azure virtual machines (VMs) is currently in preview.</span></span>

## <a name="overview"></a><span data-ttu-id="6d9ae-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="6d9ae-105">Overview</span></span>

<span data-ttu-id="6d9ae-106">Tento článek vám pomůže připravit virtuální počítače Azure pro replikaci mezi dvěma oblastmi Azure po tyto počítače migraci z místního prostředí do Azure pomocí Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-106">This article helps you prepare Azure virtual machines for replication between two Azure regions after these machines have been migrated from an on-premises environment to Azure by using Azure Site Recovery.</span></span>

## <a name="disaster-recovery-and-compliance"></a><span data-ttu-id="6d9ae-107">Zotavení po havárii a dodržování předpisů</span><span class="sxs-lookup"><span data-stu-id="6d9ae-107">Disaster recovery and compliance</span></span>
<span data-ttu-id="6d9ae-108">V současné době jsou více podniky přesunutí zatížení do Azure.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-108">Today, more and more enterprises are moving their workloads to Azure.</span></span> <span data-ttu-id="6d9ae-109">S podniky přesunutí důležité místní úlohy v produkčním prostředí do Azure, nastavení zotavení po havárii pro tyto úlohy je povinné pro dodržování předpisů a zajistí ochranu proti tak možnost narušení v oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-109">With enterprises moving mission-critical on-premises production workloads to Azure, setting up disaster recovery for these workloads is mandatory for compliance and to safeguard against any disruptions in an Azure region.</span></span>

## <a name="steps-for-preparing-migrated-machines-for-replication"></a><span data-ttu-id="6d9ae-110">Kroky pro přípravu migrované počítače pro replikaci</span><span class="sxs-lookup"><span data-stu-id="6d9ae-110">Steps for preparing migrated machines for replication</span></span>
<span data-ttu-id="6d9ae-111">Příprava migrovat počítačů pro nastavení replikace jiné oblasti Azure:</span><span class="sxs-lookup"><span data-stu-id="6d9ae-111">To prepare migrated machines for setting up replication to another Azure region:</span></span>

1. <span data-ttu-id="6d9ae-112">Dokončení migrace.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-112">Complete migration.</span></span>
2. <span data-ttu-id="6d9ae-113">V případě potřeby nainstalujte agenta Azure.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-113">Install the Azure agent if needed.</span></span>
3. <span data-ttu-id="6d9ae-114">Odeberte službu Mobility.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-114">Remove the Mobility service.</span></span>  
4. <span data-ttu-id="6d9ae-115">Restartujte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-115">Restart the VM.</span></span>

<span data-ttu-id="6d9ae-116">Tyto kroky jsou podrobně popsány v další v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-116">These steps are described in more detail in the following sections.</span></span>

### <a name="step-1-migrate-workloads-running-on-hyper-v-vms-vmware-vms-and-physical-servers-to-run-on-azure-vms"></a><span data-ttu-id="6d9ae-117">Krok 1: Migrace úloh běžících na virtuálních počítačů Hyper-V, virtuálních počítačů VMware a fyzické servery spouštět na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="6d9ae-117">Step 1: Migrate workloads running on Hyper-V VMs, VMware VMs, and physical servers to run on Azure VMs</span></span>

<span data-ttu-id="6d9ae-118">Nastavení replikace a migrovat místní technologie Hyper-V, VMware a fyzické úloh do Azure, postupujte podle kroků v [virtuální počítače Azure IaaS migraci mezi oblastmi Azure s Azure Site Recovery](site-recovery-migrate-to-azure.md) článku.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-118">To set up replication and migrate your on-premises Hyper-V, VMware, and physical workloads to Azure, follow the steps in the [Migrate Azure IaaS virtual machines between Azure regions with Azure Site Recovery](site-recovery-migrate-to-azure.md) article.</span></span> 

<span data-ttu-id="6d9ae-119">Po migraci nemusíte potvrďte, nebo odstraňte převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-119">After migration, you don't need to commit or delete a failover.</span></span> <span data-ttu-id="6d9ae-120">Místo toho vyberte **dokončení migrace** možnost pro každý počítač, který chcete migrovat:</span><span class="sxs-lookup"><span data-stu-id="6d9ae-120">Instead, select the **Complete Migration** option for each machine you want to migrate:</span></span>
1. <span data-ttu-id="6d9ae-121">V části **Replikované položky** klikněte pravým tlačítkem na virtuální počítač a klikněte na **Dokončit migraci**.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-121">In **Replicated Items**, right-click the VM, and click **Complete Migration**.</span></span> <span data-ttu-id="6d9ae-122">Klikněte na tlačítko **OK** k dokončení kroku.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-122">Click **OK** to complete the step.</span></span> <span data-ttu-id="6d9ae-123">Můžete sledovat průběh ve vlastnostech virtuálního počítače pomocí monitorování úlohy dokončení migrace v **úlohy Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-123">You can track progress in the VM properties by monitoring the Complete Migration job in **Site Recovery jobs**.</span></span>
2. <span data-ttu-id="6d9ae-124">**Dokončení migrace** akci dokončí proces migrace, odebere replikaci pro počítač a zastaví fakturace Site Recovery pro tento počítač.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-124">The **Complete Migration** action completes the migration process, removes replication for the machine, and stops Site Recovery billing for the machine.</span></span>

   ![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

### <a name="step-2-install-the-azure-vm-agent-on-the-virtual-machine"></a><span data-ttu-id="6d9ae-126">Krok 2: Instalace agenta virtuálního počítače Azure na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="6d9ae-126">Step 2: Install the Azure VM agent on the virtual machine</span></span>
<span data-ttu-id="6d9ae-127">Azure [agenta virtuálního počítače](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) musí být nainstalován na virtuálním počítači pro obnovení lokality rozšíření pro práci a k ochraně virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-127">The Azure [VM agent](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) must be installed on the virtual machine for the Site Recovery extension to work and to help protect the VM.</span></span>

>[!IMPORTANT]
><span data-ttu-id="6d9ae-128">Počínaje verzí 9.7.0.0, na virtuální počítače s Windows, instalační program služby Mobility nainstaluje nejnovější dostupný agent virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-128">Beginning with version 9.7.0.0, on Windows virtual machines, the Mobility service installer also installs the latest available Azure VM agent.</span></span> <span data-ttu-id="6d9ae-129">Virtuální počítač na migraci, splňuje instalace agenta požadované pro používání všech rozšíření virtuálního počítače, včetně přípony Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-129">On migration, the virtual machine meets the agent installation prerequisite for using any VM extension, including the Site Recovery extension.</span></span> <span data-ttu-id="6d9ae-130">Agent virtuálního počítače Azure je nutné ručně nainstalovat jenom v případě, že je služba Mobility na migrované počítači nainstalovaná verze 9,6 nebo dřívější.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-130">The Azure VM agent needs to be manually installed only if the Mobility service installed on the migrated machine is version 9.6 or earlier.</span></span>

<span data-ttu-id="6d9ae-131">Následující tabulka obsahuje další informace o instalaci agenta virtuálního počítače a ověřuje, zda byl nainstalován:</span><span class="sxs-lookup"><span data-stu-id="6d9ae-131">The following table provides additional information about installing the VM agent and validating that it was installed:</span></span>

| <span data-ttu-id="6d9ae-132">**Operace**</span><span class="sxs-lookup"><span data-stu-id="6d9ae-132">**Operation**</span></span> | <span data-ttu-id="6d9ae-133">**Windows**</span><span class="sxs-lookup"><span data-stu-id="6d9ae-133">**Windows**</span></span> | <span data-ttu-id="6d9ae-134">**Linux**</span><span class="sxs-lookup"><span data-stu-id="6d9ae-134">**Linux**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6d9ae-135">Instalace agenta virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6d9ae-135">Installing the VM agent</span></span> |<span data-ttu-id="6d9ae-136">Stáhněte si a nainstalujte [MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="6d9ae-136">Download and install the [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).</span></span> <span data-ttu-id="6d9ae-137">Budete potřebovat oprávnění správce k dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-137">You need administrator privileges to complete the installation.</span></span> |<span data-ttu-id="6d9ae-138">Nainstalujte si nejnovější verzi [agenta systému Linux](../virtual-machines/linux/agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="6d9ae-138">Install the latest [Linux agent](../virtual-machines/linux/agent-user-guide.md).</span></span> <span data-ttu-id="6d9ae-139">Budete potřebovat oprávnění správce k dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-139">You need administrator privileges to complete the installation.</span></span> <span data-ttu-id="6d9ae-140">Doporučujeme nainstalovat agenta z distribuční úložiště.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-140">We recommend installing the agent from your distribution repository.</span></span> <span data-ttu-id="6d9ae-141">Jsme *nedoporučujeme* instalace agenta virtuálního počítače s Linuxem přímo z Githubu.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-141">We *do not recommend* installing the Linux VM agent directly from GitHub.</span></span>  |
| <span data-ttu-id="6d9ae-142">Ověření instalace agenta virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6d9ae-142">Validating the VM agent installation</span></span> |<span data-ttu-id="6d9ae-143">1. Přejděte do složky C:\WindowsAzure\Packages ve virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-143">1. Browse to the C:\WindowsAzure\Packages folder in the Azure VM.</span></span> <span data-ttu-id="6d9ae-144">Měli byste vidět přítomný soubor WaAppAgent.exe.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-144">You should see the WaAppAgent.exe file.</span></span> <br><span data-ttu-id="6d9ae-145">2. Pravým tlačítkem myši klikněte na soubor, přejděte na **Vlastnosti** a poté vyberte kartu **Podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-145">2. Right-click the file, go to **Properties**, and then select the **Details** tab.</span></span> <span data-ttu-id="6d9ae-146">**Verze produktu** pole by mělo být 2.6.1198.718 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-146">The **Product Version** field should be 2.6.1198.718 or higher.</span></span> |<span data-ttu-id="6d9ae-147">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="6d9ae-147">N/A</span></span> |


### <a name="step-3-remove-the-mobility-service-from-the-migrated-virtual-machine"></a><span data-ttu-id="6d9ae-148">Krok 3: Odebrání služba Mobility migrovaného virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6d9ae-148">Step 3: Remove the Mobility service from the migrated virtual machine</span></span>

<span data-ttu-id="6d9ae-149">Pokud jste migrovali místní počítače VMware nebo fyzických serverů v systému Windows nebo Linux, musíte ručně odebrat nebo odinstalujte službu Mobility z migrovaného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-149">If you have migrated your on-premises VMware machines or physical servers on Windows/Linux, you need to manually remove/uninstall the Mobility service from the migrated virtual machine.</span></span>

>[!IMPORTANT]
><span data-ttu-id="6d9ae-150">Tento krok se nevyžaduje pro migraci virtuálních počítačů Hyper-V do Azure.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-150">This step is not required for Hyper-V VMs migrated to Azure.</span></span>

#### <a name="uninstall-the-mobility-service-on-a-windows-server-vm"></a><span data-ttu-id="6d9ae-151">Odinstalujte službu Mobility na virtuální počítač Windows serveru</span><span class="sxs-lookup"><span data-stu-id="6d9ae-151">Uninstall the Mobility service on a Windows Server VM</span></span>
<span data-ttu-id="6d9ae-152">Odinstalujte službu Mobility na počítači s Windows serverem, použijte jednu z následujících metod.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-152">Use one of the following methods to uninstall the Mobility service on a Windows Server computer.</span></span>

##### <a name="uninstall-by-using-the-windows-ui"></a><span data-ttu-id="6d9ae-153">Odinstalovat aplikaci pomocí uživatelského rozhraní systému Windows</span><span class="sxs-lookup"><span data-stu-id="6d9ae-153">Uninstall by using the Windows UI</span></span>
1. <span data-ttu-id="6d9ae-154">V Ovládacích panelech vyberte **programy**.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-154">In the Control Panel, select **Programs**.</span></span>
2. <span data-ttu-id="6d9ae-155">Vyberte **Microsoft Azure Site Recovery Mobility služby nebo hlavní cílový server**a potom vyberte **odinstalovat**.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-155">Select **Microsoft Azure Site Recovery Mobility Service/Master Target server**, and then select **Uninstall**.</span></span>

##### <a name="uninstall-at-a-command-prompt"></a><span data-ttu-id="6d9ae-156">Odinstalujte na příkazovém řádku</span><span class="sxs-lookup"><span data-stu-id="6d9ae-156">Uninstall at a command prompt</span></span>
1. <span data-ttu-id="6d9ae-157">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-157">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="6d9ae-158">Odinstalujte službu Mobility, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6d9ae-158">To uninstall the Mobility service, run the following command:</span></span>

   ```
   MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
   ```

#### <a name="uninstall-the-mobility-service-on-a-linux-computer"></a><span data-ttu-id="6d9ae-159">Odinstalujte službu Mobility na počítač se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="6d9ae-159">Uninstall the Mobility service on a Linux computer</span></span>
1. <span data-ttu-id="6d9ae-160">Na serveru systému Linux, přihlaste se jako **kořenové** uživatele.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-160">On your Linux server, sign in as a **root** user.</span></span>
2. <span data-ttu-id="6d9ae-161">V terminálu přejděte na /user/local/ASR.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-161">In a terminal, go to /user/local/ASR.</span></span>
3. <span data-ttu-id="6d9ae-162">Odinstalujte službu Mobility, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6d9ae-162">To uninstall the Mobility service, run the following command:</span></span>

   ```
   uninstall.sh -Y
   ```

### <a name="step-4-restart-the-vm"></a><span data-ttu-id="6d9ae-163">Krok 4: Restartování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6d9ae-163">Step 4: Restart the VM</span></span>

<span data-ttu-id="6d9ae-164">Po dokončení odinstalace služby Mobility, restartujte virtuální počítač předtím, než nastavení replikace na jiné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="6d9ae-164">After you uninstall the Mobility service, restart the VM before you set up replication to another Azure region.</span></span>


## <a name="next-steps"></a><span data-ttu-id="6d9ae-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6d9ae-165">Next steps</span></span>
- <span data-ttu-id="6d9ae-166">Začněte chránit vaše úlohy [replikovat virtuální počítače Azure](site-recovery-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="6d9ae-166">Start protecting your workloads by [replicating Azure virtual machines](site-recovery-azure-to-azure.md).</span></span>
- <span data-ttu-id="6d9ae-167">Další informace o [sítě pokyny pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="6d9ae-167">Learn more about [networking guidance for replicating Azure virtual machines](site-recovery-azure-to-azure-networking-guidance.md).</span></span>
