---
title: "Zálohuje classic nasazené virtuální počítače Azure do trezoru záloh | Microsoft Docs"
description: "Zjistit, zaregistrujte a zálohování virtuálních počítačů s tyto postupy pro zálohování virtuálních počítačů Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "zálohování virtuálních počítačů; zálohování virtuálního počítače; zálohování a zotavení po havárii; zálohování virtuálních počítačů"
ms.assetid: c0ab5469-65fd-4af5-ae9b-f5d183f82ce8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: e1da8bce96078a43c656f84005cefc8bbe81c9e3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="back-up-azure-virtual-machines-classic-portal"></a><span data-ttu-id="f97d5-104">Vytvořte zálohu virtuálních počítačích Azure (klasický portál)</span><span class="sxs-lookup"><span data-stu-id="f97d5-104">Back up Azure virtual machines (classic portal)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f97d5-105">Zálohování virtuálních počítačů do trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="f97d5-105">Back up VMs to Recovery Services vault</span></span>](backup-azure-arm-vms.md)
> * [<span data-ttu-id="f97d5-106">Zálohování virtuálních počítačů do trezoru zálohování</span><span class="sxs-lookup"><span data-stu-id="f97d5-106">Back up VMs to Backup vault</span></span>](backup-azure-vms.md)
>
>

<span data-ttu-id="f97d5-107">Tento článek obsahuje postupy pro zálohování nasazení Classic Azure virtuálního počítače (VM) do úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="f97d5-107">This article provides the procedures for backing up a Classic-deployed Azure virtual machine (VM) to a Backup vault.</span></span> <span data-ttu-id="f97d5-108">Existuje několik úloh, které potřebujete k postará o předtím, než můžete zálohovat virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="f97d5-108">There are a few tasks you need to take care of before you can back up an Azure virtual machine.</span></span> <span data-ttu-id="f97d5-109">Pokud jste tak již neučinili, proveďte [požadavky](backup-azure-vms-prepare.md) při přípravě svého prostředí pro zálohování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f97d5-109">If you haven't already done so, complete the [prerequisites](backup-azure-vms-prepare.md) to prepare your environment for backing up your VMs.</span></span>

<span data-ttu-id="f97d5-110">Další informace najdete v článcích na [plánování infrastruktury zálohování virtuálních počítačů v Azure](backup-azure-vms-introduction.md) a [virtuální počítače Azure](https://azure.microsoft.com/documentation/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="f97d5-110">For additional information, see the articles on [planning your VM backup infrastructure in Azure](backup-azure-vms-introduction.md) and [Azure virtual machines](https://azure.microsoft.com/documentation/services/virtual-machines/).</span></span>

> [!NOTE]
> <span data-ttu-id="f97d5-111">Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f97d5-111">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="f97d5-112">Úložiště záloh lze chránit pouze virtuálních počítačů nasazených Classic.</span><span class="sxs-lookup"><span data-stu-id="f97d5-112">A Backup vault can only protect Classic-deployed VMs.</span></span> <span data-ttu-id="f97d5-113">Nelze chránit virtuálních počítačů nasazených Resource Manager pomocí úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="f97d5-113">You cannot protect Resource Manager-deployed VMs with a Backup vault.</span></span> <span data-ttu-id="f97d5-114">V tématu [zálohování virtuálních počítačů do trezoru služeb zotavení](backup-azure-arm-vms.md) podrobnosti o práci se službou obnovení trezory.</span><span class="sxs-lookup"><span data-stu-id="f97d5-114">See [Back up VMs to Recovery Services vault](backup-azure-arm-vms.md) for details on working with Recovery Services vaults.</span></span>
>
>

<span data-ttu-id="f97d5-115">Zálohování virtuálních počítačů Azure zahrnuje tři základní kroky:</span><span class="sxs-lookup"><span data-stu-id="f97d5-115">Backing up Azure virtual machines involves three key steps:</span></span>

![Tři kroky pro zálohování virtuálního počítače Azure IaaS](./media/backup-azure-vms/3-steps-for-backup.png)

> [!NOTE]
> <span data-ttu-id="f97d5-117">Zálohování virtuálních počítačů je místní proces.</span><span class="sxs-lookup"><span data-stu-id="f97d5-117">Backing up virtual machines is a local process.</span></span> <span data-ttu-id="f97d5-118">Virtuální počítače v jedné oblasti nelze zálohovat do trezoru záloh v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="f97d5-118">You cannot back up virtual machines in one region to a backup vault in another region.</span></span> <span data-ttu-id="f97d5-119">Proto je nutné vytvořit úložiště záloh v každé oblasti Azure, kde existují virtuální počítače, které se bude zálohovat.</span><span class="sxs-lookup"><span data-stu-id="f97d5-119">So, you must create a backup vault in each Azure region, where there are VMs that will be backed up.</span></span>
>
> [!IMPORTANT]
> <span data-ttu-id="f97d5-120">Od března 2017 již nelze k vytvoření trezorů služby Backup použít portál Classic.</span><span class="sxs-lookup"><span data-stu-id="f97d5-120">Starting March 2017, you can no longer use the classic portal to create Backup vaults.</span></span>
> <span data-ttu-id="f97d5-121">Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="f97d5-121">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="f97d5-122">Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="f97d5-122">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="f97d5-123">Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="f97d5-123">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="f97d5-124">Od 15. října 2017 nebude možné pomocí PowerShellu vytvářet trezory služby Backup.</span><span class="sxs-lookup"><span data-stu-id="f97d5-124">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="f97d5-125">**Do 1. listopadu 2017:**</span><span class="sxs-lookup"><span data-stu-id="f97d5-125">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="f97d5-126">Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="f97d5-126">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="f97d5-127">Nebudete mít přístup k datům záloh na portálu Classic.</span><span class="sxs-lookup"><span data-stu-id="f97d5-127">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="f97d5-128">Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f97d5-128">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="step-1---discover-azure-virtual-machines"></a><span data-ttu-id="f97d5-129">Krok 1 – zjistit virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="f97d5-129">Step 1 - Discover Azure virtual machines</span></span>
<span data-ttu-id="f97d5-130">Aby se identifikují všechny nové virtuální počítače (VM) k předplatnému přidat před registrací, spusťte proces vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="f97d5-130">To ensure any new virtual machines (VMs) added to the subscription are identified before registering, run the discovery process.</span></span> <span data-ttu-id="f97d5-131">Proces se dotáže Azure na seznam virtuálních počítačů v rámci předplatného společně s dalšími informacemi, jako například název cloudové služby a oblast.</span><span class="sxs-lookup"><span data-stu-id="f97d5-131">The process queries Azure for the list of virtual machines in the subscription, along with additional information like the cloud service name and the region.</span></span>

1. <span data-ttu-id="f97d5-132">Přihlaste se k [portál Classic](http://manage.windowsazure.com/)</span><span class="sxs-lookup"><span data-stu-id="f97d5-132">Sign in to the [Classic portal](http://manage.windowsazure.com/)</span></span>
2. <span data-ttu-id="f97d5-133">V seznamu služeb Azure, klikněte na **služeb zotavení** otevření seznamu trezorů zálohování a obnovení lokality.</span><span class="sxs-lookup"><span data-stu-id="f97d5-133">In the list of Azure services, click **Recovery Services** to open the list of Backup and Site Recovery vaults.</span></span>
    <span data-ttu-id="f97d5-134">![Otevřít trezor seznamu](./media/backup-azure-vms/choose-vault-list.png)</span><span class="sxs-lookup"><span data-stu-id="f97d5-134">![Open vault list](./media/backup-azure-vms/choose-vault-list.png)</span></span>
3. <span data-ttu-id="f97d5-135">V seznamu trezory Backup vyberte trezor pro zálohování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f97d5-135">In the list of Backup vaults, select the vault to back up a VM.</span></span>

    <span data-ttu-id="f97d5-136">Pokud je to nový trezor otevře na portálu **rychlý Start** stránky.</span><span class="sxs-lookup"><span data-stu-id="f97d5-136">If this is a new vault the portal opens to the **Quick Start** page.</span></span>

    ![Otevřete registrovaná položky nabídky](./media/backup-azure-vms/vault-quick-start.png)

    <span data-ttu-id="f97d5-138">Pokud byl dříve nakonfigurován trezoru, portál ho otevře naposledy použité nabídky.</span><span class="sxs-lookup"><span data-stu-id="f97d5-138">If the vault has previously been configured, the portal opens to the most recently used menu.</span></span>
4. <span data-ttu-id="f97d5-139">V nabídce trezoru (v horní části stránky), klikněte na tlačítko **registrované položky**.</span><span class="sxs-lookup"><span data-stu-id="f97d5-139">From the vault menu (at the top of the page), click **Registered Items**.</span></span>

    ![Otevřete registrovaná položky nabídky](./media/backup-azure-vms/vault-menu.png)
5. <span data-ttu-id="f97d5-141">Z nabídky **Typ** vyberte **Virtuální počítač Azure**.</span><span class="sxs-lookup"><span data-stu-id="f97d5-141">From the **Type** menu, select **Azure Virtual Machine**.</span></span>

    ![Výběr úlohy](./media/backup-azure-vms/discovery-select-workload.png)
6. <span data-ttu-id="f97d5-143">Klikněte na **Vyhledat** ve spodní části stránky.</span><span class="sxs-lookup"><span data-stu-id="f97d5-143">Click **DISCOVER** at the bottom of the page.</span></span>
    <span data-ttu-id="f97d5-144">![Tlačítko Vyhledat](./media/backup-azure-vms/discover-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="f97d5-144">![Discover button](./media/backup-azure-vms/discover-button-only.png)</span></span>

    <span data-ttu-id="f97d5-145">Proces vyhledávání může kvůli srovnávání virtuálních počítačů trvat i několik minut.</span><span class="sxs-lookup"><span data-stu-id="f97d5-145">The discovery process may take a few minutes while the virtual machines are being tabulated.</span></span> <span data-ttu-id="f97d5-146">Ve spodní části obrazovky je oznámení, které indikuje, že proces běží.</span><span class="sxs-lookup"><span data-stu-id="f97d5-146">There is a notification at the bottom of the screen that lets you know that the process is running.</span></span>

    ![Vyhledání virtuálních počítačů](./media/backup-azure-vms/discovering-vms.png)

    <span data-ttu-id="f97d5-148">Po dokončení procesu se oznámení změní.</span><span class="sxs-lookup"><span data-stu-id="f97d5-148">The notification changes when the process is complete.</span></span> <span data-ttu-id="f97d5-149">Pokud proces zjišťování nebyl nalezen virtuální počítače, nejdříve se ujistěte, že existují virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="f97d5-149">If the discovery process did not find the virtual machines, first ensure the VMs exist.</span></span> <span data-ttu-id="f97d5-150">Pokud existují virtuální počítače, ověřte, že jsou virtuální počítače ve stejné oblasti jako trezor záloh.</span><span class="sxs-lookup"><span data-stu-id="f97d5-150">If the VMs exist, ensure the VMs are in the same region as the backup vault.</span></span> <span data-ttu-id="f97d5-151">Pokud virtuálních počítačů existují a jsou ve stejné oblasti, zajistěte, aby že virtuální počítače nejsou zaregistrována do trezoru záloh.</span><span class="sxs-lookup"><span data-stu-id="f97d5-151">If the VMs exist and are in the same region, ensure the VMs are not already registered to a backup vault.</span></span> <span data-ttu-id="f97d5-152">Pokud je virtuální počítač přiřazen do trezoru záloh není k dispozici pro přiřazení dalších záloh.</span><span class="sxs-lookup"><span data-stu-id="f97d5-152">If a VM is assigned to a backup vault it is not available to be assigned to other backup vaults.</span></span>

    ![Vyhledávání dokončeno](./media/backup-azure-vms/discovery-complete.png)

    <span data-ttu-id="f97d5-154">Po zjištění nové položky přejděte ke kroku 2 a zaregistrujte virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="f97d5-154">Once you have discovered the new items, go to Step 2 and register your VMs.</span></span>

## <a name="step-2---register-azure-virtual-machines"></a><span data-ttu-id="f97d5-155">Krok 2 – registrace virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="f97d5-155">Step 2 - Register Azure virtual machines</span></span>
<span data-ttu-id="f97d5-156">Zaregistrujte virtuální počítač Azure přidružit pomocí služby Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="f97d5-156">You register an Azure virtual machine to associate it with the Azure Backup service.</span></span> <span data-ttu-id="f97d5-157">Obvykle se jedná o jednorázové aktivity.</span><span class="sxs-lookup"><span data-stu-id="f97d5-157">This is typically a one-time activity.</span></span>

1. <span data-ttu-id="f97d5-158">Přejděte do trezoru záloh v části **služeb zotavení** v portálu Azure a pak klikněte na tlačítko **registrované položky**.</span><span class="sxs-lookup"><span data-stu-id="f97d5-158">Navigate to the backup vault under **Recovery Services** in the Azure portal, and then click **Registered Items**.</span></span>
2. <span data-ttu-id="f97d5-159">Z rozevírací nabídky vyberte **Virtuální počítač Azure**</span><span class="sxs-lookup"><span data-stu-id="f97d5-159">Select **Azure Virtual Machine** from the drop-down menu.</span></span>

    ![Výběr úlohy](./media/backup-azure-vms/discovery-select-workload.png)
3. <span data-ttu-id="f97d5-161">Klikněte na **Registrovat** ve spodní části stránky.</span><span class="sxs-lookup"><span data-stu-id="f97d5-161">Click **REGISTER** at the bottom of the page.</span></span>
    <span data-ttu-id="f97d5-162">![Tlačítko Registrovat](./media/backup-azure-vms/register-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="f97d5-162">![Register button](./media/backup-azure-vms/register-button-only.png)</span></span>
4. <span data-ttu-id="f97d5-163">V místní nabídce **Registrovat položky** vyberte virtuální počítače, které chcete zaregistrovat.</span><span class="sxs-lookup"><span data-stu-id="f97d5-163">In the **Register Items** shortcut menu, select the virtual machines that you want to register.</span></span> <span data-ttu-id="f97d5-164">Pokud existují dvě nebo více virtuálních počítačů se stejným názvem, použijte k rozlišení mezi nimi cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="f97d5-164">If there are two or more virtual machines with the same name, use the cloud service to distinguish between them.</span></span>

   > [!TIP]
   > <span data-ttu-id="f97d5-165">Najednou může být registrováno více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f97d5-165">Multiple virtual machines can be registered at one time.</span></span>
   >
   >

    <span data-ttu-id="f97d5-166">Pro každý virtuální počítač, který jste vybrali, se vytvoří úloha.</span><span class="sxs-lookup"><span data-stu-id="f97d5-166">A job is created for each virtual machine that you've selected.</span></span>
5. <span data-ttu-id="f97d5-167">Klikněte na **Zobrazit úlohy** v oznámení pro přechod na stránku **Úlohy**.</span><span class="sxs-lookup"><span data-stu-id="f97d5-167">Click **View Job** in the notification to go to the **Jobs** page.</span></span>

    ![Registrace úlohy](./media/backup-azure-vms/register-create-job.png)

    <span data-ttu-id="f97d5-169">Virtuální počítač se také zobrazí v seznamu registrovaných položek společně se stavem operace registrování.</span><span class="sxs-lookup"><span data-stu-id="f97d5-169">The virtual machine also appears in the list of registered items, along with the status of the registration operation.</span></span>

    ![Stav registrace 1](./media/backup-azure-vms/register-status01.png)

    <span data-ttu-id="f97d5-171">Po dokončení operace se stav změní, aby reflektoval stav *registrován*.</span><span class="sxs-lookup"><span data-stu-id="f97d5-171">When the operation completes, the status changes to reflect the *registered* state.</span></span>

    ![Stav registrace 2](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---protect-azure-virtual-machines"></a><span data-ttu-id="f97d5-173">Krok 3 – chránit virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="f97d5-173">Step 3 - Protect Azure virtual machines</span></span>
<span data-ttu-id="f97d5-174">Nyní můžete nastavit zásadu zálohování a uchovávání pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f97d5-174">Now you can set up a backup and retention policy for the virtual machine.</span></span> <span data-ttu-id="f97d5-175">Více virtuálních počítačů může být chráněn pomocí jedné chránit akce.</span><span class="sxs-lookup"><span data-stu-id="f97d5-175">Multiple virtual machines can be protected by using a single protect action.</span></span>

<span data-ttu-id="f97d5-176">Azure trezory Backup vytvořené po květen 2015 dodávají s výchozí zásadou integrovaný do trezoru.</span><span class="sxs-lookup"><span data-stu-id="f97d5-176">Azure Backup vaults created after May 2015 come with a default policy built into the vault.</span></span> <span data-ttu-id="f97d5-177">Tato výchozí zásada se dodává s výchozí uchování 30 dnů a plán zálohování jednou denně.</span><span class="sxs-lookup"><span data-stu-id="f97d5-177">This default policy comes with a default retention of 30 days and a once-daily backup schedule.</span></span>

1. <span data-ttu-id="f97d5-178">Přejděte do trezoru záloh v části **služeb zotavení** v portálu Azure a pak klikněte na tlačítko **registrované položky**.</span><span class="sxs-lookup"><span data-stu-id="f97d5-178">Navigate to the backup vault under **Recovery Services** in the Azure portal, and then click **Registered Items**.</span></span>
2. <span data-ttu-id="f97d5-179">Z rozevírací nabídky vyberte **Virtuální počítač Azure**</span><span class="sxs-lookup"><span data-stu-id="f97d5-179">Select **Azure Virtual Machine** from the drop-down menu.</span></span>

    ![Výběr úlohy v portálu](./media/backup-azure-vms/select-workload.png)
3. <span data-ttu-id="f97d5-181">Klikněte na **Chránit** ve spodní části stránky.</span><span class="sxs-lookup"><span data-stu-id="f97d5-181">Click **PROTECT** at the bottom of the page.</span></span>

    <span data-ttu-id="f97d5-182">**Průvodce ochranou položek** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="f97d5-182">The **Protect Items wizard** appears.</span></span> <span data-ttu-id="f97d5-183">Průvodce je uveden pouze virtuální počítače, které jsou zaregistrované a nechráněné.</span><span class="sxs-lookup"><span data-stu-id="f97d5-183">The wizard only lists virtual machines that are registered and not protected.</span></span> <span data-ttu-id="f97d5-184">Vyberte virtuální počítače, které chcete ochránit.</span><span class="sxs-lookup"><span data-stu-id="f97d5-184">Select the virtual machines that you want to protect.</span></span>

    <span data-ttu-id="f97d5-185">Pokud existují dvě nebo více virtuálních počítačů se stejným názvem, použijte k rozlišení mezi virtuálními počítači cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="f97d5-185">If there are two or more virtual machines with the same name, use the cloud service to distinguish between the virtual machines.</span></span>

   > [!TIP]
   > <span data-ttu-id="f97d5-186">Najednou můžete chránit více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f97d5-186">You can protect multiple virtual machines at one time.</span></span>
   >
   >

    ![Škálování konfigurace ochrany](./media/backup-azure-vms/protect-at-scale.png)

4. <span data-ttu-id="f97d5-188">Vyberte **plán zálohování** zálohovat virtuální počítače, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="f97d5-188">Choose a **backup schedule** to back up the virtual machines that you've selected.</span></span> <span data-ttu-id="f97d5-189">Můžete vybrat z existující sady zásad nebo zadejte nový.</span><span class="sxs-lookup"><span data-stu-id="f97d5-189">You can pick from an existing set of policies or define a new one.</span></span>

    <span data-ttu-id="f97d5-190">Ke každé zásadě zálohování může být přidružených více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f97d5-190">Each backup policy can have multiple virtual machines associated with it.</span></span> <span data-ttu-id="f97d5-191">Však virtuálního počítače lze přidružit pouze s jednu zásadu v libovolném bodě v čase.</span><span class="sxs-lookup"><span data-stu-id="f97d5-191">However, the virtual machine can only be associated with one policy at any given point in time.</span></span>

    ![Ochrana pomocí nové zásady](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > <span data-ttu-id="f97d5-193">Zásada zálohování obsahuje schéma uchovávání pro plánované zálohy.</span><span class="sxs-lookup"><span data-stu-id="f97d5-193">A backup policy includes a retention scheme for the scheduled backups.</span></span> <span data-ttu-id="f97d5-194">Pokud vyberete existující zásady zálohování, nelze změnit možnosti uchovávání v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="f97d5-194">If you select an existing backup policy, you cannot modify the retention options in the next step.</span></span>
   >
   >

5. <span data-ttu-id="f97d5-195">Vyberte **rozsah uchování** přidružení k zálohování.</span><span class="sxs-lookup"><span data-stu-id="f97d5-195">Choose a **retention range** to associate with the backups.</span></span>

    ![Chránit pomocí flexibilní uchování](./media/backup-azure-vms/policy-retention.png)

    <span data-ttu-id="f97d5-197">Zásada uchovávání specifikuje, jak dlouho se záloha má uchovat.</span><span class="sxs-lookup"><span data-stu-id="f97d5-197">Retention policy specifies the length of time for storing a backup.</span></span> <span data-ttu-id="f97d5-198">Můžete zadat rozdílné zásady v závislosti na tom, kdy dochází k zálohování.</span><span class="sxs-lookup"><span data-stu-id="f97d5-198">You can specify different retention policies based on when the backup is taken.</span></span> <span data-ttu-id="f97d5-199">Například bod zálohy pořizovat denně (který slouží jako bod obnovení provozní) může být zachována 90 dní.</span><span class="sxs-lookup"><span data-stu-id="f97d5-199">For example, a backup point taken daily (which serves as an operational recovery point) might be preserved for 90 days.</span></span> <span data-ttu-id="f97d5-200">Porovnání může bod zálohy prováděné na konci každé čtvrtletí (pro účely auditu) potřeba zachovat pro mnoho měsíců nebo let.</span><span class="sxs-lookup"><span data-stu-id="f97d5-200">In comparison, a backup point taken at the end of each quarter (for audit purposes) may need to be preserved for many months or years.</span></span>

    ![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/long-term-retention.png)

    <span data-ttu-id="f97d5-202">Na tomto obrázku příklad:</span><span class="sxs-lookup"><span data-stu-id="f97d5-202">In this example image:</span></span>

   * <span data-ttu-id="f97d5-203">**Denní zásady uchovávání informací**: zálohy pořizovat denně se uchovávají po dobu 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="f97d5-203">**Daily retention policy**: Backups taken daily are stored for 30 days.</span></span>
   * <span data-ttu-id="f97d5-204">**Týdenní zásady uchovávání informací**: zálohy pořízené každý týden v neděli zachovány 104 týdny.</span><span class="sxs-lookup"><span data-stu-id="f97d5-204">**Weekly retention policy**: Backups taken every week on Sunday are preserved for 104 weeks.</span></span>
   * <span data-ttu-id="f97d5-205">**Měsíční zásady uchovávání informací**: zálohy pořízené poslední neděli každého měsíce se zachovají pro 120 měsíců.</span><span class="sxs-lookup"><span data-stu-id="f97d5-205">**Monthly retention policy**: Backups taken on the last Sunday of each month are preserved for 120 months.</span></span>
   * <span data-ttu-id="f97d5-206">**Roční zásady uchovávání informací**: zálohy vytvořené v první neděli daného každých leden se zachovají pro 99 let.</span><span class="sxs-lookup"><span data-stu-id="f97d5-206">**Yearly retention policy**: Backups taken on the first Sunday of every January are preserved for 99 years.</span></span>

     <span data-ttu-id="f97d5-207">Úloha se vytvoří pro nakonfigurujte zásady ochrany a přidružte virtuální počítače k této zásadě pro každý virtuální počítač, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="f97d5-207">A job is created to configure the protection policy and associate the virtual machines to that policy for each virtual machine that you've selected.</span></span>
6. <span data-ttu-id="f97d5-208">Chcete-li zobrazit seznam **Konfigurace ochrany** klikněte na úlohy, v nabídce trezory **úlohy** a vyberte **Konfigurace ochrany** z **operace** filtru.</span><span class="sxs-lookup"><span data-stu-id="f97d5-208">To view the list of **Configure Protection** jobs, from the vaults menu, click **Jobs** and select **Configure Protection** from the **Operation** filter.</span></span>

    ![Úloha konfigurace ochrany](./media/backup-azure-vms/protect-configureprotection.png)

## <a name="initial-backup"></a><span data-ttu-id="f97d5-210">Prvotní zálohování</span><span class="sxs-lookup"><span data-stu-id="f97d5-210">Initial backup</span></span>
<span data-ttu-id="f97d5-211">Jakmile je virtuální počítač je chráněný pomocí zásad, se zobrazí v části **chráněné položky** karta se stavem *chráněno – (čekání na prvotní zálohování)*.</span><span class="sxs-lookup"><span data-stu-id="f97d5-211">Once the virtual machine is protected with a policy, it shows up under the **Protected Items** tab with the status of *Protected - (pending initial backup)*.</span></span> <span data-ttu-id="f97d5-212">Ve výchozím nastavení je první plánovanou zálohou *prvotní záloha*.</span><span class="sxs-lookup"><span data-stu-id="f97d5-212">By default, the first scheduled backup is the *initial backup*.</span></span>

<span data-ttu-id="f97d5-213">K aktivaci prvotní zálohování okamžitě po konfiguraci ochrany:</span><span class="sxs-lookup"><span data-stu-id="f97d5-213">To trigger the initial backup immediately after configuring protection:</span></span>

1. <span data-ttu-id="f97d5-214">V dolní části **chráněné položky** klikněte na tlačítko **zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="f97d5-214">At the bottom of the **Protected Items** page, click **Backup Now**.</span></span>

    <span data-ttu-id="f97d5-215">Služba Azure Backup vytvoří úlohu zálohování pro operaci prvotního zálohování.</span><span class="sxs-lookup"><span data-stu-id="f97d5-215">The Azure Backup service creates a backup job for the initial backup operation.</span></span>
2. <span data-ttu-id="f97d5-216">Chcete-li zobrazit seznam úloh, klikněte na kartu **Úlohy**.</span><span class="sxs-lookup"><span data-stu-id="f97d5-216">Click the **Jobs** tab to view the list of jobs.</span></span>

    ![Probíhá zálohování](./media/backup-azure-vms/protect-inprogress.png)

> [!NOTE]
> <span data-ttu-id="f97d5-218">Během operace zálohování vydá služba Azure Backup příkaz rozšířením zálohování v každém virtuálním počítači k vyprázdnění všech úloh zápisu a pořízení konzistentního snímku.</span><span class="sxs-lookup"><span data-stu-id="f97d5-218">During the backup operation, the Azure Backup service issues a command to the backup extension in each virtual machine to flush all write jobs and take a consistent snapshot.</span></span>
>
>

<span data-ttu-id="f97d5-219">Po prvotní zálohování dokončení, stav virtuálního počítače **chráněné položky** karta je *chráněné*.</span><span class="sxs-lookup"><span data-stu-id="f97d5-219">When the initial backup finishes, the status of the virtual machine in the **Protected Items** tab is *Protected*.</span></span>

![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/protect-backedupvm.png)

## <a name="viewing-backup-status-and-details"></a><span data-ttu-id="f97d5-221">Zobrazení stavu zálohy a podrobnosti</span><span class="sxs-lookup"><span data-stu-id="f97d5-221">Viewing backup status and details</span></span>
<span data-ttu-id="f97d5-222">Jakmile chráněný, také zvyšuje počet virtuálních počítačů v **řídicí panel** stránky Souhrn.</span><span class="sxs-lookup"><span data-stu-id="f97d5-222">Once protected, the virtual machine count also increases in the **Dashboard** page summary.</span></span> <span data-ttu-id="f97d5-223">**Řídicí panel** stránka zobrazuje také počet úloh z za posledních 24 hodin, které byly *úspěšné*, mají *se nezdařilo*a jsou *v průběhu*.</span><span class="sxs-lookup"><span data-stu-id="f97d5-223">The **Dashboard** page also shows the number of jobs from the last 24 hours that were *successful*, have *failed*, and are *in progress*.</span></span> <span data-ttu-id="f97d5-224">Na **úlohy** stránky, použijte **stav**, **operace**, nebo **z** a **k** nabídky pro filtrování úloh.</span><span class="sxs-lookup"><span data-stu-id="f97d5-224">On the **Jobs** page, use the **Status**, **Operation**, or **From** and **To** menus to filter the jobs.</span></span>

![Stav služby backup v stránka řídicího panelu](./media/backup-azure-vms/dashboard-protectedvms.png)

<span data-ttu-id="f97d5-226">Hodnoty v řídicím panelu se aktualizuje každých 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="f97d5-226">Values in the dashboard are refreshed once every 24 hours.</span></span>

## <a name="troubleshooting-errors"></a><span data-ttu-id="f97d5-227">Řešení potíží s chybami</span><span class="sxs-lookup"><span data-stu-id="f97d5-227">Troubleshooting errors</span></span>
<span data-ttu-id="f97d5-228">Pokud narazíte na problémy při zálohování virtuálního počítače, podívejte se na [článku Poradce při potížích virtuálních počítačů](backup-azure-vms-troubleshoot.md) nápovědu.</span><span class="sxs-lookup"><span data-stu-id="f97d5-228">If you run into issues while backing up your virtual machine, look at the [VM     troubleshooting article](backup-azure-vms-troubleshoot.md) for help.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f97d5-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f97d5-229">Next steps</span></span>
* [<span data-ttu-id="f97d5-230">Správa a monitorování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="f97d5-230">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="f97d5-231">Obnovení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="f97d5-231">Restore virtual machines</span></span>](backup-azure-restore-vms.md)
