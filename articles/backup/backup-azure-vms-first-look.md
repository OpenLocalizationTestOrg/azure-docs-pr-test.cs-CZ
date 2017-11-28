---
title: "První pohled: Zálohování virtuálních počítačů Azure s trezorem zálohování | Dokumentace Microsoftu"
description: "Použijte portál Classic k zálohování virtuálních počítačů Azure do trezoru služby Backup. Tento kurz vysvětluje všechny fáze, včetně vytvoření trezoru služby Backup, registrace virtuálních počítačů, vytvoření zásady zálohování a spuštění úlohy prvotního zálohování."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 722820dc-b65f-425c-a9e5-c1946e896a87
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/2/2017
ms.author: markgal;
ms.openlocfilehash: fc31d7654e455ec5b4e4bb9af4cf1a166f1661ee
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="first-look-backing-up-azure-virtual-machines"></a><span data-ttu-id="5bec4-104">První pohled: Zálohování virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="5bec4-104">First look: Backing up Azure virtual machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5bec4-105">Ochrana virtuálních počítačů v trezoru služby Recovery Services</span><span class="sxs-lookup"><span data-stu-id="5bec4-105">Protect VMs with a recovery services vault</span></span>](backup-azure-vms-first-look-arm.md)
> * [<span data-ttu-id="5bec4-106">Ochrana virtuálních počítačů Azure s trezorem zálohování</span><span class="sxs-lookup"><span data-stu-id="5bec4-106">Protect Azure VMs with a backup vault</span></span>](backup-azure-vms-first-look.md)
>
>

<span data-ttu-id="5bec4-107">Tento kurz vás provede kroky pro zálohování virtuálních počítačů (VM) Azure do trezoru zálohování v Azure.</span><span class="sxs-lookup"><span data-stu-id="5bec4-107">This tutorial takes you through the steps for backing up an Azure virtual machine (VM) to a backup vault in Azure.</span></span> <span data-ttu-id="5bec4-108">Tento článek popisuje model Classic nebo model nasazení Resource Manager pro zálohování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5bec4-108">This article describes the classic model or Service Manager deployment model, for backing up VMs.</span></span> <span data-ttu-id="5bec4-109">Následující postup se vztahuje pouze na trezory služby Backup vytvořený na portálu Classic.</span><span class="sxs-lookup"><span data-stu-id="5bec4-109">The following steps apply only to Backup vaults created in the classic portal.</span></span> <span data-ttu-id="5bec4-110">Microsoft doporučuje pro nová nasazení používat model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5bec4-110">Microsoft recommends using the Resource Manager model for new deployments.</span></span>

<span data-ttu-id="5bec4-111">Pokud máte zájem o zálohování virtuálních počítačů do trezoru Recovery Services, který patří do skupiny prostředků, přečtěte si téma [První pohled: Ochrana virtuálních počítačů pomocí trezoru Recovery Services](backup-azure-vms-first-look-arm.md).</span><span class="sxs-lookup"><span data-stu-id="5bec4-111">If you are interested in backing up a VM to a Recovery Services vault that belongs to a Resource Group, see [First look: Protect VMs with a recovery services vault](backup-azure-vms-first-look-arm.md).</span></span>

<span data-ttu-id="5bec4-112">Pro úspěšné dokončení následujícího kurzu musí být splněny tyto požadavky:</span><span class="sxs-lookup"><span data-stu-id="5bec4-112">To successfully complete the following tutorial, these prerequisites must exist:</span></span>

* <span data-ttu-id="5bec4-113">Vytvořili jste virtuální počítač v rámci svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="5bec4-113">You have created a VM in your Azure subscription.</span></span>
* <span data-ttu-id="5bec4-114">Virtuální počítač je připojen k veřejným IP adresám Azure.</span><span class="sxs-lookup"><span data-stu-id="5bec4-114">The VM has connectivity to Azure public IP addresses.</span></span> <span data-ttu-id="5bec4-115">Další informace naleznete v tématu [Připojení k síti](backup-azure-vms-prepare.md#network-connectivity).</span><span class="sxs-lookup"><span data-stu-id="5bec4-115">For additional information, see [Network connectivity](backup-azure-vms-prepare.md#network-connectivity).</span></span>


> [!NOTE]
> <span data-ttu-id="5bec4-116">Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5bec4-116">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="5bec4-117">Tento kurz je určen pro použití s virtuálními počítači vytvořené na portálu Classic.</span><span class="sxs-lookup"><span data-stu-id="5bec4-117">This tutorial is for use with virtual machines created in the classic portal.</span></span>
>
>

## <a name="create-a-backup-vault"></a><span data-ttu-id="5bec4-118">Vytvoření trezoru záloh</span><span class="sxs-lookup"><span data-stu-id="5bec4-118">Create a backup vault</span></span>
<span data-ttu-id="5bec4-119">Trezor záloh je entita, která ukládá všechny vytvořené zálohy a body obnovení.</span><span class="sxs-lookup"><span data-stu-id="5bec4-119">A backup vault is an entity that stores all the backups and recovery points that have been created over time.</span></span> <span data-ttu-id="5bec4-120">Trezor záloh obsahuje také zásady zálohování, které se aplikují na zálohované virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="5bec4-120">The backup vault also contains the backup policies that are applied to the virtual machines being backed up.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5bec4-121">Od března 2017 již nelze k vytvoření trezorů služby Backup použít portál Classic.</span><span class="sxs-lookup"><span data-stu-id="5bec4-121">Starting March 2017, you can no longer use the classic portal to create Backup vaults.</span></span>
> <span data-ttu-id="5bec4-122">Trezory služby Backup můžete upgradovat na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="5bec4-122">You can upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="5bec4-123">Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="5bec4-123">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="5bec4-124">Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="5bec4-124">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="5bec4-125">Od 15. října 2017 nebude možné pomocí PowerShellu vytvářet trezory služby Backup.</span><span class="sxs-lookup"><span data-stu-id="5bec4-125">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="5bec4-126">**Do 1. listopadu 2017:**</span><span class="sxs-lookup"><span data-stu-id="5bec4-126">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="5bec4-127">Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="5bec4-127">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="5bec4-128">Nebudete mít přístup k datům záloh na portálu Classic.</span><span class="sxs-lookup"><span data-stu-id="5bec4-128">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="5bec4-129">Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5bec4-129">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="discover-and-register-azure-virtual-machines"></a><span data-ttu-id="5bec4-130">Vyhledání a registrace virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="5bec4-130">Discover and Register Azure virtual machines</span></span>
<span data-ttu-id="5bec4-131">Před zaregistrováním virtuálního počítače k trezoru spusťte proces vyhledávání pro identifikaci nových virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5bec4-131">Before registering the VM with a vault, run the discovery process to identify any new VMs.</span></span> <span data-ttu-id="5bec4-132">Ten vrátí seznam virtuálních počítačů v rámci předplatného společně s dalšími informacemi, jako například název cloudové služby a oblast.</span><span class="sxs-lookup"><span data-stu-id="5bec4-132">This returns a list of virtual machines in the subscription, along with additional information like the cloud service name and the region.</span></span>

1. <span data-ttu-id="5bec4-133">Přihlaste se k [portálu Azure Classic](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="5bec4-133">Sign in to the [Azure Classic portal](http://manage.windowsazure.com/)</span></span>
2. <span data-ttu-id="5bec4-134">Na portálu Azure Classic klikněte na **Recovery Services** a otevře se seznam trezorů Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="5bec4-134">In the Azure classic portal, click **Recovery Services** to open the list of Recovery Services vaults.</span></span>
    <span data-ttu-id="5bec4-135">![Výběr úlohy](./media/backup-azure-vms-first-look/recovery-services-icon.png)</span><span class="sxs-lookup"><span data-stu-id="5bec4-135">![Select workload](./media/backup-azure-vms-first-look/recovery-services-icon.png)</span></span>
3. <span data-ttu-id="5bec4-136">Ze seznamu trezorů vyberte trezor pro zálohování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5bec4-136">From the list of vaults, select the vault to back up a VM.</span></span>

    <span data-ttu-id="5bec4-137">Po výběru se trezor otevře na stránce **Rychlý start**.</span><span class="sxs-lookup"><span data-stu-id="5bec4-137">When you select your vault, it opens in the **Quick Start** page</span></span>
4. <span data-ttu-id="5bec4-138">V nabídce trezoru klikněte na **Registrované položky**.</span><span class="sxs-lookup"><span data-stu-id="5bec4-138">From the vault menu, click **Registered Items**.</span></span>

    ![Výběr úlohy](./media/backup-azure-vms-first-look/configure-registered-items.png)
5. <span data-ttu-id="5bec4-140">Z nabídky **Typ** vyberte **Virtuální počítač Azure**.</span><span class="sxs-lookup"><span data-stu-id="5bec4-140">From the **Type** menu, select **Azure Virtual Machine**.</span></span>

    ![Výběr úlohy](./media/backup-azure-vms/discovery-select-workload.png)
6. <span data-ttu-id="5bec4-142">Klikněte na **Vyhledat** ve spodní části stránky.</span><span class="sxs-lookup"><span data-stu-id="5bec4-142">Click **DISCOVER** at the bottom of the page.</span></span>
    <span data-ttu-id="5bec4-143">![Tlačítko Vyhledat](./media/backup-azure-vms/discover-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="5bec4-143">![Discover button](./media/backup-azure-vms/discover-button-only.png)</span></span>

    <span data-ttu-id="5bec4-144">Proces vyhledávání může kvůli srovnávání virtuálních počítačů trvat i několik minut.</span><span class="sxs-lookup"><span data-stu-id="5bec4-144">The discovery process may take a few minutes while the virtual machines are being tabulated.</span></span> <span data-ttu-id="5bec4-145">Ve spodní části obrazovky je oznámení, které indikuje, že proces běží.</span><span class="sxs-lookup"><span data-stu-id="5bec4-145">There is a notification at the bottom of the screen that lets you know that the process is running.</span></span>

    ![Vyhledání virtuálních počítačů](./media/backup-azure-vms/discovering-vms.png)

    <span data-ttu-id="5bec4-147">Po dokončení procesu se oznámení změní.</span><span class="sxs-lookup"><span data-stu-id="5bec4-147">The notification changes when the process is complete.</span></span>

    ![Vyhledávání dokončeno](./media/backup-azure-vms-first-look/discovery-complete.png)
7. <span data-ttu-id="5bec4-149">Klikněte na **Registrovat** ve spodní části stránky.</span><span class="sxs-lookup"><span data-stu-id="5bec4-149">Click **REGISTER** at the bottom of the page.</span></span>
    <span data-ttu-id="5bec4-150">![Tlačítko Registrovat](./media/backup-azure-vms-first-look/register-icon.png)</span><span class="sxs-lookup"><span data-stu-id="5bec4-150">![Register button](./media/backup-azure-vms-first-look/register-icon.png)</span></span>
8. <span data-ttu-id="5bec4-151">V místní nabídce **Registrovat položky** vyberte virtuální počítače, které chcete zaregistrovat.</span><span class="sxs-lookup"><span data-stu-id="5bec4-151">In the **Register Items** shortcut menu, select the virtual machines that you want to register.</span></span>

   > [!TIP]
   > <span data-ttu-id="5bec4-152">Najednou může být registrováno více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5bec4-152">Multiple virtual machines can be registered at one time.</span></span>
   >
   >

    <span data-ttu-id="5bec4-153">Pro každý virtuální počítač, který jste vybrali, se vytvoří úloha.</span><span class="sxs-lookup"><span data-stu-id="5bec4-153">A job is created for each virtual machine that you've selected.</span></span>
9. <span data-ttu-id="5bec4-154">Klikněte na **Zobrazit úlohy** v oznámení pro přechod na stránku **Úlohy**.</span><span class="sxs-lookup"><span data-stu-id="5bec4-154">Click **View Job** in the notification to go to the **Jobs** page.</span></span>

    ![Registrace úlohy](./media/backup-azure-vms/register-create-job.png)

    <span data-ttu-id="5bec4-156">Virtuální počítač se také zobrazí v seznamu registrovaných položek společně se stavem operace registrování.</span><span class="sxs-lookup"><span data-stu-id="5bec4-156">The virtual machine also appears in the list of registered items, along with the status of the registration operation.</span></span>

    ![Stav registrace 1](./media/backup-azure-vms/register-status01.png)

    <span data-ttu-id="5bec4-158">Po dokončení operace se stav změní, aby reflektoval stav *registrován*.</span><span class="sxs-lookup"><span data-stu-id="5bec4-158">When the operation completes, the status changes to reflect the *registered* state.</span></span>

    ![Stav registrace 2](./media/backup-azure-vms/register-status02.png)

## <a name="install-the-vm-agent-on-the-virtual-machine"></a><span data-ttu-id="5bec4-160">Instalace agenta virtuálního počítače na virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="5bec4-160">Install the VM Agent on the virtual machine</span></span>
<span data-ttu-id="5bec4-161">Pro fungování rozšíření Backup musí být na virtuálním počítači Azure nainstalovaný agent virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="5bec4-161">The Azure VM Agent must be installed on the Azure virtual machine for the Backup extension to work.</span></span> <span data-ttu-id="5bec4-162">Pokud byl váš virtuální počítač vytvořen z galerie Azure, je na něm agent virtuálního počítače již nainstalován. Můžete přeskočit k [ochraně virtuálních počítačů](backup-azure-vms-first-look.md#create-the-backup-policy).</span><span class="sxs-lookup"><span data-stu-id="5bec4-162">If your VM was created from the Azure gallery, the VM Agent is already present on the VM; you can skip to [protecting your VMs](backup-azure-vms-first-look.md#create-the-backup-policy).</span></span>

<span data-ttu-id="5bec4-163">Pokud byl virtuální počítač přenesen z místního datového centra, pravděpodobně na něm není agent virtuálního počítače nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="5bec4-163">If your VM migrated from an on-premises datacenter, the VM probably does not have the VM Agent installed.</span></span> <span data-ttu-id="5bec4-164">Před pokračováním k ochraně virtuálního počítače je potřeba na něj nainstalovat agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5bec4-164">You must install the VM Agent on the virtual machine before proceeding to protect the VM.</span></span> <span data-ttu-id="5bec4-165">Podrobné pokyny k instalaci agenta virtuálního počítače naleznete v [oddílu Agent virtuálního počítače v článku Zálohování virtuálních počítačů](backup-azure-vms-prepare.md#vm-agent).</span><span class="sxs-lookup"><span data-stu-id="5bec4-165">For detailed steps on installing the VM Agent, see the [VM Agent section of the Backup VMs article](backup-azure-vms-prepare.md#vm-agent).</span></span>

## <a name="create-the-backup-policy"></a><span data-ttu-id="5bec4-166">Vytvoření zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="5bec4-166">Create the backup policy</span></span>
<span data-ttu-id="5bec4-167">Předtím, než aktivujete úlohu prvotního zálohování, nastavte plán pořizování snímků zálohy.</span><span class="sxs-lookup"><span data-stu-id="5bec4-167">Before you trigger the initial backup job, set the schedule when backup snapshots are taken.</span></span> <span data-ttu-id="5bec4-168">Plán pořizování snímků záloh a doba jejich uchování se nazývá zásada zálohování.</span><span class="sxs-lookup"><span data-stu-id="5bec4-168">The schedule when backup snapshots are taken, and the length of time those snapshots are retained, is the backup policy.</span></span> <span data-ttu-id="5bec4-169">Informace o zachování jsou založené na trojgeneračním schématu rotace záloh.</span><span class="sxs-lookup"><span data-stu-id="5bec4-169">The retention information is based on Grandfather-father-son backup rotation scheme.</span></span>

1. <span data-ttu-id="5bec4-170">Na portálu Azure Classic přejděte v **Recovery Services** do trezoru záloh a klikněte na **Registrované položky**.</span><span class="sxs-lookup"><span data-stu-id="5bec4-170">Navigate to the backup vault under **Recovery Services** in the Azure Classic portal, and  click **Registered Items**.</span></span>
2. <span data-ttu-id="5bec4-171">Z rozevírací nabídky vyberte **Virtuální počítač Azure**</span><span class="sxs-lookup"><span data-stu-id="5bec4-171">Select **Azure Virtual Machine** from the drop-down menu.</span></span>

    ![Výběr úlohy v portálu](./media/backup-azure-vms/select-workload.png)
3. <span data-ttu-id="5bec4-173">Klikněte na **Chránit** ve spodní části stránky.</span><span class="sxs-lookup"><span data-stu-id="5bec4-173">Click **PROTECT** at the bottom of the page.</span></span>
    <span data-ttu-id="5bec4-174">![Klikněte na Chránit](./media/backup-azure-vms-first-look/protect-icon.png)</span><span class="sxs-lookup"><span data-stu-id="5bec4-174">![Click Protect](./media/backup-azure-vms-first-look/protect-icon.png)</span></span>

    <span data-ttu-id="5bec4-175">Objeví se **Průvodce ochranou položek** a zobrazí *pouze* virtuální počítače, které jsou zaregistrované a nechráněné.</span><span class="sxs-lookup"><span data-stu-id="5bec4-175">The **Protect Items wizard** appears and lists *only* virtual machines that are registered and not protected.</span></span>

    ![Škálování konfigurace ochrany](./media/backup-azure-vms/protect-at-scale.png)
4. <span data-ttu-id="5bec4-177">Vyberte virtuální počítače, které chcete ochránit.</span><span class="sxs-lookup"><span data-stu-id="5bec4-177">Select the virtual machines that you want to protect.</span></span>

    <span data-ttu-id="5bec4-178">Pokud existují dva nebo více virtuálních počítačů se stejným názvem, použijte k jejich rozlišení Cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="5bec4-178">If there are two or more virtual machines with the same name, use the Cloud Service to distinguish between the virtual machines.</span></span>
5. <span data-ttu-id="5bec4-179">V nabídce **Konfigurace ochrany** vyberte existující zásadu nebo vytvořte novou zásadu k ochraně určených virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5bec4-179">On the **Configure protection** menu select an existing policy or create a new policy to protect the virtual machines that you identified.</span></span>

    <span data-ttu-id="5bec4-180">Nové trezory záloh mají přidruženou výchozí zásadu.</span><span class="sxs-lookup"><span data-stu-id="5bec4-180">New Backup vaults have a default policy associated with the vault.</span></span> <span data-ttu-id="5bec4-181">Tato zásada pořizuje každý večer denní snímek, který je uchován po dobu 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="5bec4-181">This policy takes a daily snapshot each evening, and the daily snapshot is retained for 30 days.</span></span> <span data-ttu-id="5bec4-182">Ke každé zásadě zálohování může být přidružených více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5bec4-182">Each backup policy can have multiple virtual machines associated with it.</span></span> <span data-ttu-id="5bec4-183">K virtuálnímu počítači však může být najednou přidružená pouze jedna zásada.</span><span class="sxs-lookup"><span data-stu-id="5bec4-183">However, the virtual machine can only be associated with one policy at a time.</span></span>

    ![Ochrana pomocí nové zásady](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > <span data-ttu-id="5bec4-185">Zásada zálohování obsahuje schéma uchovávání pro plánované zálohy.</span><span class="sxs-lookup"><span data-stu-id="5bec4-185">A backup policy includes a retention scheme for the scheduled backups.</span></span> <span data-ttu-id="5bec4-186">Pokud zvolíte existující zásadu zálohování, nebudete moci v dalším kroku změnit možnosti uchovávání.</span><span class="sxs-lookup"><span data-stu-id="5bec4-186">If you select an existing backup policy, you will be unable to modify the retention options in the next step.</span></span>
   >
   >
6. <span data-ttu-id="5bec4-187">V **Rozsahu uchovávání** definujte denní, týdenní, měsíční nebo roční rozsah pro konkrétní body zálohy.</span><span class="sxs-lookup"><span data-stu-id="5bec4-187">On **Retention Range** define the daily, weekly, monthly, and yearly scope for the specific backup points.</span></span>

    ![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/long-term-retention.png)

    <span data-ttu-id="5bec4-189">Zásada uchovávání specifikuje, jak dlouho se záloha má uchovat.</span><span class="sxs-lookup"><span data-stu-id="5bec4-189">Retention policy specifies the length of time for storing a backup.</span></span> <span data-ttu-id="5bec4-190">Můžete zadat rozdílné zásady v závislosti na tom, kdy dochází k zálohování.</span><span class="sxs-lookup"><span data-stu-id="5bec4-190">You can specify different retention policies based on when the backup is taken.</span></span>
7. <span data-ttu-id="5bec4-191">Chcete-li zobrazit seznam úloh **Konfigurace ochrany**, klikněte na **Úlohy**.</span><span class="sxs-lookup"><span data-stu-id="5bec4-191">Click **Jobs** to view the list of **Configure Protection** jobs.</span></span>

    ![Úloha konfigurace ochrany](./media/backup-azure-vms/protect-configureprotection.png)

    <span data-ttu-id="5bec4-193">Nyní, když jste vytvořili zásadu, přejděte k dalšímu kroku a spusťte prvotní zálohování.</span><span class="sxs-lookup"><span data-stu-id="5bec4-193">Now that you've established the policy, go to the next step and run the initial backup.</span></span>

## <a name="initial-backup"></a><span data-ttu-id="5bec4-194">Prvotní zálohování</span><span class="sxs-lookup"><span data-stu-id="5bec4-194">Initial backup</span></span>
<span data-ttu-id="5bec4-195">Jakmile je virtuální počítač chráněný zásadou, můžete si tento vztah prohlédnout na kartě **Chráněné položky**. Před provedením prvotního zálohování bude **Stav ochrany** ukazovat **Chráněno – (čekání na prvotní zálohování)**.</span><span class="sxs-lookup"><span data-stu-id="5bec4-195">Once a virtual machine has been protected with a policy, you can view that relationship on the **Protected Items** tab. Until the initial backup occurs, the **Protection Status** shows as **Protected - (pending initial backup)**.</span></span> <span data-ttu-id="5bec4-196">Ve výchozím nastavení je první plánovanou zálohou *prvotní záloha*.</span><span class="sxs-lookup"><span data-stu-id="5bec4-196">By default, the first scheduled backup is the *initial backup*.</span></span>

![Zálohování čeká na zpracování](./media/backup-azure-vms-first-look/protection-pending-border.png)

<span data-ttu-id="5bec4-198">Chcete-li nyní spustit prvotní zálohování:</span><span class="sxs-lookup"><span data-stu-id="5bec4-198">To start the initial backup now:</span></span>

1. <span data-ttu-id="5bec4-199">Na stránce **Chráněné položky** klikněte na **Zálohovat nyní** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="5bec4-199">On the **Protected Items** page, click **Backup Now** at the bottom of the page.</span></span>
    <span data-ttu-id="5bec4-200">![Ikona Zálohovat nyní](./media/backup-azure-vms-first-look/backup-now-icon.png)</span><span class="sxs-lookup"><span data-stu-id="5bec4-200">![Backup Now icon](./media/backup-azure-vms-first-look/backup-now-icon.png)</span></span>

    <span data-ttu-id="5bec4-201">Služba Azure Backup vytvoří úlohu zálohování pro operaci prvotního zálohování.</span><span class="sxs-lookup"><span data-stu-id="5bec4-201">The Azure Backup service creates a backup job for the initial backup operation.</span></span>
2. <span data-ttu-id="5bec4-202">Chcete-li zobrazit seznam úloh, klikněte na kartu **Úlohy**.</span><span class="sxs-lookup"><span data-stu-id="5bec4-202">Click the **Jobs** tab to view the list of jobs.</span></span>

    ![Probíhá zálohování](./media/backup-azure-vms-first-look/protect-inprogress.png)

    <span data-ttu-id="5bec4-204">Po dokončení prvotního zálohování stav virtuálního počítače na kartě **Chráněné položky** je *Chráněný*.</span><span class="sxs-lookup"><span data-stu-id="5bec4-204">When initial backup is complete, the status of the virtual machine in the **Protected Items** tab is *Protected*.</span></span>

    ![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/protect-backedupvm.png)

   > [!NOTE]
   > <span data-ttu-id="5bec4-206">Zálohování virtuálních počítačů je místní proces.</span><span class="sxs-lookup"><span data-stu-id="5bec4-206">Backing up virtual machines is a local process.</span></span> <span data-ttu-id="5bec4-207">Virtuální počítače z jedné oblasti nelze zálohovat do trezoru záloh v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="5bec4-207">You cannot back up virtual machines from one region to a backup vault in another region.</span></span> <span data-ttu-id="5bec4-208">Pro každou oblast Azure s virtuálními počítači, které je třeba zálohovat, je tedy nutné vytvořit v této oblasti alespoň jeden trezor záloh.</span><span class="sxs-lookup"><span data-stu-id="5bec4-208">So, for every Azure region that has VMs that need to be backed up, at least one backup vault must be created in that region.</span></span>
   >
   >

## <a name="next-steps"></a><span data-ttu-id="5bec4-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5bec4-209">Next steps</span></span>
<span data-ttu-id="5bec4-210">Když jste teď úspěšně zálohovali virtuální počítač, je několik dalších kroků, které by vás mohly zajímat.</span><span class="sxs-lookup"><span data-stu-id="5bec4-210">Now that you have successfully backed up a VM, there are several next steps that could be of interest.</span></span> <span data-ttu-id="5bec4-211">Nejlogičtějším krokem je seznámení se s obnovováním dat na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="5bec4-211">The most logical step is to familiarize yourself with restoring data to a VM.</span></span> <span data-ttu-id="5bec4-212">Nicméně existují úlohy správy, které vám pomohou pochopit, jak zabezpečit dat a minimalizovat náklady.</span><span class="sxs-lookup"><span data-stu-id="5bec4-212">However, there are management tasks that will help you understand how to keep your data safe and minimize costs.</span></span>

* [<span data-ttu-id="5bec4-213">Správa a monitorování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5bec4-213">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="5bec4-214">Obnovení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5bec4-214">Restore virtual machines</span></span>](backup-azure-restore-vms.md)
* [<span data-ttu-id="5bec4-215">Pokyny při řešení potíží</span><span class="sxs-lookup"><span data-stu-id="5bec4-215">Troubleshooting guidance</span></span>](backup-azure-vms-troubleshoot.md)

## <a name="questions"></a><span data-ttu-id="5bec4-216">Máte dotazy?</span><span class="sxs-lookup"><span data-stu-id="5bec4-216">Questions?</span></span>
<span data-ttu-id="5bec4-217">Máte-li nějaké dotazy nebo pokud víte o funkci, kterou byste uvítali, [odešlete nám svůj názor](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="5bec4-217">If you have questions, or if there is any feature that you would like to see included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>
