---
title: "aaaBack do trezoru záloh tooa classic nasazené virtuální počítače Azure | Microsoft Docs"
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
ms.openlocfilehash: 048e32d9b2bd5bdd7a125225a71a6d805bb4fbd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-virtual-machines-classic-portal"></a><span data-ttu-id="fb467-104">Vytvořte zálohu virtuálních počítačích Azure (klasický portál)</span><span class="sxs-lookup"><span data-stu-id="fb467-104">Back up Azure virtual machines (classic portal)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fb467-105">Zálohování trezor služeb tooRecovery virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="fb467-105">Back up VMs tooRecovery Services vault</span></span>](backup-azure-arm-vms.md)
> * [<span data-ttu-id="fb467-106">Zálohování virtuálních počítačů tooBackup trezoru</span><span class="sxs-lookup"><span data-stu-id="fb467-106">Back up VMs tooBackup vault</span></span>](backup-azure-vms.md)
>
>

<span data-ttu-id="fb467-107">Tento článek obsahuje hello postupy pro zálohování úložiště záloh tooa Classic nasadit virtuální počítač Azure (VM).</span><span class="sxs-lookup"><span data-stu-id="fb467-107">This article provides hello procedures for backing up a Classic-deployed Azure virtual machine (VM) tooa Backup vault.</span></span> <span data-ttu-id="fb467-108">Existuje několik úloh, které potřebujete tootake znak c/o předtím, než můžete zálohovat virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="fb467-108">There are a few tasks you need tootake care of before you can back up an Azure virtual machine.</span></span> <span data-ttu-id="fb467-109">Pokud jste již neučinili Ano, dokončení hello [požadavky](backup-azure-vms-prepare.md) tooprepare prostředí pro zálohování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fb467-109">If you haven't already done so, complete hello [prerequisites](backup-azure-vms-prepare.md) tooprepare your environment for backing up your VMs.</span></span>

<span data-ttu-id="fb467-110">Další informace najdete v tématu články hello na [plánování infrastruktury zálohování virtuálních počítačů v Azure](backup-azure-vms-introduction.md) a [virtuální počítače Azure](https://azure.microsoft.com/documentation/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="fb467-110">For additional information, see hello articles on [planning your VM backup infrastructure in Azure](backup-azure-vms-introduction.md) and [Azure virtual machines](https://azure.microsoft.com/documentation/services/virtual-machines/).</span></span>

> [!NOTE]
> <span data-ttu-id="fb467-111">Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="fb467-111">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="fb467-112">Úložiště záloh lze chránit pouze virtuálních počítačů nasazených Classic.</span><span class="sxs-lookup"><span data-stu-id="fb467-112">A Backup vault can only protect Classic-deployed VMs.</span></span> <span data-ttu-id="fb467-113">Nelze chránit virtuálních počítačů nasazených Resource Manager pomocí úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="fb467-113">You cannot protect Resource Manager-deployed VMs with a Backup vault.</span></span> <span data-ttu-id="fb467-114">V tématu [zálohování virtuálních počítačů trezor služeb tooRecovery](backup-azure-arm-vms.md) podrobnosti o práci se službou obnovení trezory.</span><span class="sxs-lookup"><span data-stu-id="fb467-114">See [Back up VMs tooRecovery Services vault](backup-azure-arm-vms.md) for details on working with Recovery Services vaults.</span></span>
>
>

<span data-ttu-id="fb467-115">Zálohování virtuálních počítačů Azure zahrnuje tři základní kroky:</span><span class="sxs-lookup"><span data-stu-id="fb467-115">Backing up Azure virtual machines involves three key steps:</span></span>

![Tři kroky tooback zálohu virtuálního počítače Azure IaaS](./media/backup-azure-vms/3-steps-for-backup.png)

> [!NOTE]
> <span data-ttu-id="fb467-117">Zálohování virtuálních počítačů je místní proces.</span><span class="sxs-lookup"><span data-stu-id="fb467-117">Backing up virtual machines is a local process.</span></span> <span data-ttu-id="fb467-118">Nelze zálohovat virtuální počítače v jedné oblasti úložiště tooa záloh, které v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="fb467-118">You cannot back up virtual machines in one region tooa backup vault in another region.</span></span> <span data-ttu-id="fb467-119">Proto je nutné vytvořit úložiště záloh v každé oblasti Azure, kde existují virtuální počítače, které se bude zálohovat.</span><span class="sxs-lookup"><span data-stu-id="fb467-119">So, you must create a backup vault in each Azure region, where there are VMs that will be backed up.</span></span>
>
> [!IMPORTANT]
> <span data-ttu-id="fb467-120">Od března 2017 se už můžete trezory Backup portálu classic toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-120">Starting March 2017, you can no longer use hello classic portal toocreate Backup vaults.</span></span>
> <span data-ttu-id="fb467-121">Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup.</span><span class="sxs-lookup"><span data-stu-id="fb467-121">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="fb467-122">Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="fb467-122">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="fb467-123">Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.</span><span class="sxs-lookup"><span data-stu-id="fb467-123">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="fb467-124">Po 15 říjen 2017 nemůžete použít trezory Backup toocreate prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fb467-124">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="fb467-125">**Do 1. listopadu 2017:**</span><span class="sxs-lookup"><span data-stu-id="fb467-125">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="fb467-126">Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.</span><span class="sxs-lookup"><span data-stu-id="fb467-126">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="fb467-127">Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-127">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="fb467-128">Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="fb467-128">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="step-1---discover-azure-virtual-machines"></a><span data-ttu-id="fb467-129">Krok 1 – zjistit virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="fb467-129">Step 1 - Discover Azure virtual machines</span></span>
<span data-ttu-id="fb467-130">tooensure žádné nové předplatné přidané toohello virtuální počítače (VM) jsou identifikovány před registrací, spusťte proces zjišťování hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-130">tooensure any new virtual machines (VMs) added toohello subscription are identified before registering, run hello discovery process.</span></span> <span data-ttu-id="fb467-131">Hello proces se dotáže Azure hello seznam virtuálních počítačů v rámci předplatného hello, společně s dalšími informacemi, jako třeba název hello cloudové služby a oblast hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-131">hello process queries Azure for hello list of virtual machines in hello subscription, along with additional information like hello cloud service name and hello region.</span></span>

1. <span data-ttu-id="fb467-132">Přihlaste se toohello [portál Classic](http://manage.windowsazure.com/)</span><span class="sxs-lookup"><span data-stu-id="fb467-132">Sign in toohello [Classic portal](http://manage.windowsazure.com/)</span></span>
2. <span data-ttu-id="fb467-133">V seznamu hello služeb Azure, klikněte na **služeb zotavení** tooopen hello seznamu trezorů zálohování a obnovení lokality.</span><span class="sxs-lookup"><span data-stu-id="fb467-133">In hello list of Azure services, click **Recovery Services** tooopen hello list of Backup and Site Recovery vaults.</span></span>
    <span data-ttu-id="fb467-134">![Otevřít trezor seznamu](./media/backup-azure-vms/choose-vault-list.png)</span><span class="sxs-lookup"><span data-stu-id="fb467-134">![Open vault list](./media/backup-azure-vms/choose-vault-list.png)</span></span>
3. <span data-ttu-id="fb467-135">V seznamu hello trezory Backup vyberte hello trezoru tooback zálohu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fb467-135">In hello list of Backup vaults, select hello vault tooback up a VM.</span></span>

    <span data-ttu-id="fb467-136">Pokud je to nový trezor hello portál otevře toohello **rychlý Start** stránky.</span><span class="sxs-lookup"><span data-stu-id="fb467-136">If this is a new vault hello portal opens toohello **Quick Start** page.</span></span>

    ![Otevřete registrovaná položky nabídky](./media/backup-azure-vms/vault-quick-start.png)

    <span data-ttu-id="fb467-138">Pokud byl dříve nakonfigurován hello trezoru, hello portál ho otevře nabídky toohello naposledy použili.</span><span class="sxs-lookup"><span data-stu-id="fb467-138">If hello vault has previously been configured, hello portal opens toohello most recently used menu.</span></span>
4. <span data-ttu-id="fb467-139">V nabídce trezoru hello (u hello horní části stránky hello), klikněte na tlačítko **registrované položky**.</span><span class="sxs-lookup"><span data-stu-id="fb467-139">From hello vault menu (at hello top of hello page), click **Registered Items**.</span></span>

    ![Otevřete registrovaná položky nabídky](./media/backup-azure-vms/vault-menu.png)
5. <span data-ttu-id="fb467-141">Z hello **typ** nabídce vyberte možnost **virtuální počítač Azure**.</span><span class="sxs-lookup"><span data-stu-id="fb467-141">From hello **Type** menu, select **Azure Virtual Machine**.</span></span>

    ![Výběr úlohy](./media/backup-azure-vms/discovery-select-workload.png)
6. <span data-ttu-id="fb467-143">Klikněte na tlačítko **DISCOVER** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-143">Click **DISCOVER** at hello bottom of hello page.</span></span>
    <span data-ttu-id="fb467-144">![Tlačítko Vyhledat](./media/backup-azure-vms/discover-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="fb467-144">![Discover button](./media/backup-azure-vms/discover-button-only.png)</span></span>

    <span data-ttu-id="fb467-145">proces zjišťování Hello může trvat několik minut hello virtuální počítače jsou se v tabulce.</span><span class="sxs-lookup"><span data-stu-id="fb467-145">hello discovery process may take a few minutes while hello virtual machines are being tabulated.</span></span> <span data-ttu-id="fb467-146">Je oznámení dole hello úvodní obrazovka, která vás informuje, zda je spuštěn proces hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-146">There is a notification at hello bottom of hello screen that lets you know that hello process is running.</span></span>

    ![Vyhledání virtuálních počítačů](./media/backup-azure-vms/discovering-vms.png)

    <span data-ttu-id="fb467-148">Hello oznámení změny po dokončení procesu hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-148">hello notification changes when hello process is complete.</span></span> <span data-ttu-id="fb467-149">Pokud proces zjišťování hello nenalezl hello virtuální počítače, nejdříve se ujistěte, že existují hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fb467-149">If hello discovery process did not find hello virtual machines, first ensure hello VMs exist.</span></span> <span data-ttu-id="fb467-150">Pokud existují hello virtuálních počítačů, ujistěte se, hello virtuální počítače jsou v hello stejné oblasti jako trezor záloh hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-150">If hello VMs exist, ensure hello VMs are in hello same region as hello backup vault.</span></span> <span data-ttu-id="fb467-151">Pokud hello virtuálních počítačů existují a jsou ve stejné oblasti text hello, ujistěte se, že hello virtuální počítače ještě nejsou registrované tooa úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="fb467-151">If hello VMs exist and are in hello same region, ensure hello VMs are not already registered tooa backup vault.</span></span> <span data-ttu-id="fb467-152">Pokud je virtuální počítač přiřazený tooa úložiště záloh není k dispozici toobe přiřazené tooother záloh.</span><span class="sxs-lookup"><span data-stu-id="fb467-152">If a VM is assigned tooa backup vault it is not available toobe assigned tooother backup vaults.</span></span>

    ![Vyhledávání dokončeno](./media/backup-azure-vms/discovery-complete.png)

    <span data-ttu-id="fb467-154">Po zjištění nové položky hello přejděte tooStep 2 a zaregistrujte virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="fb467-154">Once you have discovered hello new items, go tooStep 2 and register your VMs.</span></span>

## <a name="step-2---register-azure-virtual-machines"></a><span data-ttu-id="fb467-155">Krok 2 – registrace virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="fb467-155">Step 2 - Register Azure virtual machines</span></span>
<span data-ttu-id="fb467-156">Zaregistrovat tooassociate virtuální počítač Azure se službou hello služby zálohování Azure.</span><span class="sxs-lookup"><span data-stu-id="fb467-156">You register an Azure virtual machine tooassociate it with hello Azure Backup service.</span></span> <span data-ttu-id="fb467-157">Obvykle se jedná o jednorázové aktivity.</span><span class="sxs-lookup"><span data-stu-id="fb467-157">This is typically a one-time activity.</span></span>

1. <span data-ttu-id="fb467-158">Přejděte toohello zálohy trezoru pod **služeb zotavení** v hello portálu Azure a potom klikněte na **registrované položky**.</span><span class="sxs-lookup"><span data-stu-id="fb467-158">Navigate toohello backup vault under **Recovery Services** in hello Azure portal, and then click **Registered Items**.</span></span>
2. <span data-ttu-id="fb467-159">Vyberte **virtuální počítač Azure** z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-159">Select **Azure Virtual Machine** from hello drop-down menu.</span></span>

    ![Výběr úlohy](./media/backup-azure-vms/discovery-select-workload.png)
3. <span data-ttu-id="fb467-161">Klikněte na tlačítko **ZAREGISTROVAT** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-161">Click **REGISTER** at hello bottom of hello page.</span></span>
    <span data-ttu-id="fb467-162">![Tlačítko Registrovat](./media/backup-azure-vms/register-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="fb467-162">![Register button](./media/backup-azure-vms/register-button-only.png)</span></span>
4. <span data-ttu-id="fb467-163">V hello **registrovat položky** místní nabídky, vyberte hello virtuálních počítačů, které chcete tooregister.</span><span class="sxs-lookup"><span data-stu-id="fb467-163">In hello **Register Items** shortcut menu, select hello virtual machines that you want tooregister.</span></span> <span data-ttu-id="fb467-164">Pokud existují dvě nebo více virtuálních počítačů s hello stejný název, použijte hello cloudové služby toodistinguish mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="fb467-164">If there are two or more virtual machines with hello same name, use hello cloud service toodistinguish between them.</span></span>

   > [!TIP]
   > <span data-ttu-id="fb467-165">Najednou může být registrováno více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fb467-165">Multiple virtual machines can be registered at one time.</span></span>
   >
   >

    <span data-ttu-id="fb467-166">Pro každý virtuální počítač, který jste vybrali, se vytvoří úloha.</span><span class="sxs-lookup"><span data-stu-id="fb467-166">A job is created for each virtual machine that you've selected.</span></span>
5. <span data-ttu-id="fb467-167">Klikněte na tlačítko **zobrazit úlohy** v hello oznámení toogo toohello **úlohy** stránky.</span><span class="sxs-lookup"><span data-stu-id="fb467-167">Click **View Job** in hello notification toogo toohello **Jobs** page.</span></span>

    ![Registrace úlohy](./media/backup-azure-vms/register-create-job.png)

    <span data-ttu-id="fb467-169">virtuální počítač Hello se také zobrazí v hello seznamu registrovaných položek společně hello stav operace Registrování hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-169">hello virtual machine also appears in hello list of registered items, along with hello status of hello registration operation.</span></span>

    ![Stav registrace 1](./media/backup-azure-vms/register-status01.png)

    <span data-ttu-id="fb467-171">Po dokončení operace hello hello stav se změní tooreflect hello *zaregistrován* stavu.</span><span class="sxs-lookup"><span data-stu-id="fb467-171">When hello operation completes, hello status changes tooreflect hello *registered* state.</span></span>

    ![Stav registrace 2](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---protect-azure-virtual-machines"></a><span data-ttu-id="fb467-173">Krok 3 – chránit virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="fb467-173">Step 3 - Protect Azure virtual machines</span></span>
<span data-ttu-id="fb467-174">Nyní můžete nastavit zásadu zálohování a uchovávání hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fb467-174">Now you can set up a backup and retention policy for hello virtual machine.</span></span> <span data-ttu-id="fb467-175">Více virtuálních počítačů může být chráněn pomocí jedné chránit akce.</span><span class="sxs-lookup"><span data-stu-id="fb467-175">Multiple virtual machines can be protected by using a single protect action.</span></span>

<span data-ttu-id="fb467-176">Azure trezory Backup vytvořené po květen 2015 dodává s výchozí zásady integrovaný do trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-176">Azure Backup vaults created after May 2015 come with a default policy built into hello vault.</span></span> <span data-ttu-id="fb467-177">Tato výchozí zásada se dodává s výchozí uchování 30 dnů a plán zálohování jednou denně.</span><span class="sxs-lookup"><span data-stu-id="fb467-177">This default policy comes with a default retention of 30 days and a once-daily backup schedule.</span></span>

1. <span data-ttu-id="fb467-178">Přejděte toohello zálohy trezoru pod **služeb zotavení** v hello portálu Azure a potom klikněte na **registrované položky**.</span><span class="sxs-lookup"><span data-stu-id="fb467-178">Navigate toohello backup vault under **Recovery Services** in hello Azure portal, and then click **Registered Items**.</span></span>
2. <span data-ttu-id="fb467-179">Vyberte **virtuální počítač Azure** z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-179">Select **Azure Virtual Machine** from hello drop-down menu.</span></span>

    ![Výběr úlohy v portálu](./media/backup-azure-vms/select-workload.png)
3. <span data-ttu-id="fb467-181">Klikněte na tlačítko **CHRÁNIT** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-181">Click **PROTECT** at hello bottom of hello page.</span></span>

    <span data-ttu-id="fb467-182">Hello **Průvodce ochranou položek** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="fb467-182">hello **Protect Items wizard** appears.</span></span> <span data-ttu-id="fb467-183">Průvodce Hello je uveden pouze virtuální počítače, které jsou zaregistrované a nechráněné.</span><span class="sxs-lookup"><span data-stu-id="fb467-183">hello wizard only lists virtual machines that are registered and not protected.</span></span> <span data-ttu-id="fb467-184">Vyberte hello virtuálních počítačů, které chcete tooprotect.</span><span class="sxs-lookup"><span data-stu-id="fb467-184">Select hello virtual machines that you want tooprotect.</span></span>

    <span data-ttu-id="fb467-185">Pokud existují dvě nebo více virtuálních počítačů s hello stejný název, použijte hello cloudové služby toodistinguish mezi virtuálními počítači hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-185">If there are two or more virtual machines with hello same name, use hello cloud service toodistinguish between hello virtual machines.</span></span>

   > [!TIP]
   > <span data-ttu-id="fb467-186">Najednou můžete chránit více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fb467-186">You can protect multiple virtual machines at one time.</span></span>
   >
   >

    ![Škálování konfigurace ochrany](./media/backup-azure-vms/protect-at-scale.png)

4. <span data-ttu-id="fb467-188">Vyberte **plán zálohování** tooback až hello virtuálních počítačů, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="fb467-188">Choose a **backup schedule** tooback up hello virtual machines that you've selected.</span></span> <span data-ttu-id="fb467-189">Můžete vybrat z existující sady zásad nebo zadejte nový.</span><span class="sxs-lookup"><span data-stu-id="fb467-189">You can pick from an existing set of policies or define a new one.</span></span>

    <span data-ttu-id="fb467-190">Ke každé zásadě zálohování může být přidružených více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fb467-190">Each backup policy can have multiple virtual machines associated with it.</span></span> <span data-ttu-id="fb467-191">Však hello virtuálního počítače lze přidružit pouze s jednu zásadu v libovolném bodě v čase.</span><span class="sxs-lookup"><span data-stu-id="fb467-191">However, hello virtual machine can only be associated with one policy at any given point in time.</span></span>

    ![Ochrana pomocí nové zásady](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > <span data-ttu-id="fb467-193">Zásada zálohování obsahuje schéma uchovávání pro hello naplánované zálohování.</span><span class="sxs-lookup"><span data-stu-id="fb467-193">A backup policy includes a retention scheme for hello scheduled backups.</span></span> <span data-ttu-id="fb467-194">Pokud vyberete existující zásady zálohování, nelze změnit možnosti uchovávání hello v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-194">If you select an existing backup policy, you cannot modify hello retention options in hello next step.</span></span>
   >
   >

5. <span data-ttu-id="fb467-195">Vyberte **rozsah uchování** tooassociate s hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="fb467-195">Choose a **retention range** tooassociate with hello backups.</span></span>

    ![Chránit pomocí flexibilní uchování](./media/backup-azure-vms/policy-retention.png)

    <span data-ttu-id="fb467-197">Zásady uchovávání informací určuje hello dobu pro uložení zálohy.</span><span class="sxs-lookup"><span data-stu-id="fb467-197">Retention policy specifies hello length of time for storing a backup.</span></span> <span data-ttu-id="fb467-198">Můžete zadat různé zásady uchovávání informací na základě při pořízení zálohy hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-198">You can specify different retention policies based on when hello backup is taken.</span></span> <span data-ttu-id="fb467-199">Například bod zálohy pořizovat denně (který slouží jako bod obnovení provozní) může být zachována 90 dní.</span><span class="sxs-lookup"><span data-stu-id="fb467-199">For example, a backup point taken daily (which serves as an operational recovery point) might be preserved for 90 days.</span></span> <span data-ttu-id="fb467-200">Porovnání bod zálohy prováděné na konci hello každé čtvrtletí (pro účely auditu) může být nutné toobe zachovají pro mnoho měsíců nebo let.</span><span class="sxs-lookup"><span data-stu-id="fb467-200">In comparison, a backup point taken at hello end of each quarter (for audit purposes) may need toobe preserved for many months or years.</span></span>

    ![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/long-term-retention.png)

    <span data-ttu-id="fb467-202">Na tomto obrázku příklad:</span><span class="sxs-lookup"><span data-stu-id="fb467-202">In this example image:</span></span>

   * <span data-ttu-id="fb467-203">**Denní zásady uchovávání informací**: zálohy pořizovat denně se uchovávají po dobu 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="fb467-203">**Daily retention policy**: Backups taken daily are stored for 30 days.</span></span>
   * <span data-ttu-id="fb467-204">**Týdenní zásady uchovávání informací**: zálohy pořízené každý týden v neděli zachovány 104 týdny.</span><span class="sxs-lookup"><span data-stu-id="fb467-204">**Weekly retention policy**: Backups taken every week on Sunday are preserved for 104 weeks.</span></span>
   * <span data-ttu-id="fb467-205">**Měsíční zásady uchovávání informací**: zálohy pořízené hello poslední neděle každý měsíc zachovány po dobu 120 měsíců.</span><span class="sxs-lookup"><span data-stu-id="fb467-205">**Monthly retention policy**: Backups taken on hello last Sunday of each month are preserved for 120 months.</span></span>
   * <span data-ttu-id="fb467-206">**Roční zásady uchovávání informací**: zálohy pořízené hello První neděle každých leden se zachovají pro 99 let.</span><span class="sxs-lookup"><span data-stu-id="fb467-206">**Yearly retention policy**: Backups taken on hello first Sunday of every January are preserved for 99 years.</span></span>

     <span data-ttu-id="fb467-207">Úloha se vytvoří zásady ochrany hello tooconfigure a přidružit zásady toothat hello virtuálních počítačů pro každý virtuální počítač, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="fb467-207">A job is created tooconfigure hello protection policy and associate hello virtual machines toothat policy for each virtual machine that you've selected.</span></span>
6. <span data-ttu-id="fb467-208">seznam hello tooview **Konfigurace ochrany** úlohy v nabídce hello trezory, klikněte na tlačítko **úlohy** a vyberte **Konfigurace ochrany** z hello **operace**  filtru.</span><span class="sxs-lookup"><span data-stu-id="fb467-208">tooview hello list of **Configure Protection** jobs, from hello vaults menu, click **Jobs** and select **Configure Protection** from hello **Operation** filter.</span></span>

    ![Úloha konfigurace ochrany](./media/backup-azure-vms/protect-configureprotection.png)

## <a name="initial-backup"></a><span data-ttu-id="fb467-210">Prvotní zálohování</span><span class="sxs-lookup"><span data-stu-id="fb467-210">Initial backup</span></span>
<span data-ttu-id="fb467-211">Jakmile hello virtuální počítač je chráněný pomocí zásad, se zobrazí v části hello **chráněné položky** karta s hello stav *chráněno – (čekání na prvotní zálohování)*.</span><span class="sxs-lookup"><span data-stu-id="fb467-211">Once hello virtual machine is protected with a policy, it shows up under hello **Protected Items** tab with hello status of *Protected - (pending initial backup)*.</span></span> <span data-ttu-id="fb467-212">Ve výchozím nastavení, je prvním plánovaným zálohováním hello hello *prvotní zálohování*.</span><span class="sxs-lookup"><span data-stu-id="fb467-212">By default, hello first scheduled backup is hello *initial backup*.</span></span>

<span data-ttu-id="fb467-213">hello prvotní zálohování tootrigger okamžitě po konfiguraci ochrany:</span><span class="sxs-lookup"><span data-stu-id="fb467-213">tootrigger hello initial backup immediately after configuring protection:</span></span>

1. <span data-ttu-id="fb467-214">Na konci hello hello **chráněné položky** klikněte na tlačítko **zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="fb467-214">At hello bottom of hello **Protected Items** page, click **Backup Now**.</span></span>

    <span data-ttu-id="fb467-215">Hello služba Azure Backup vytvoří úlohu zálohování pro operaci prvotního zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="fb467-215">hello Azure Backup service creates a backup job for hello initial backup operation.</span></span>
2. <span data-ttu-id="fb467-216">Klikněte na tlačítko hello **úlohy** kartě tooview hello seznamu úloh.</span><span class="sxs-lookup"><span data-stu-id="fb467-216">Click hello **Jobs** tab tooview hello list of jobs.</span></span>

    ![Probíhá zálohování](./media/backup-azure-vms/protect-inprogress.png)

> [!NOTE]
> <span data-ttu-id="fb467-218">Během operace zálohování hello vydává hello služba Azure Backup příkaz toohello rozšířením zálohování v každé virtuální počítač tooflush všechny zápisu úlohy a pořízení konzistentního snímku.</span><span class="sxs-lookup"><span data-stu-id="fb467-218">During hello backup operation, hello Azure Backup service issues a command toohello backup extension in each virtual machine tooflush all write jobs and take a consistent snapshot.</span></span>
>
>

<span data-ttu-id="fb467-219">Po prvotní zálohování hello dokončení, stav hello hello virtuálního počítače v hello **chráněné položky** karta je *chráněné*.</span><span class="sxs-lookup"><span data-stu-id="fb467-219">When hello initial backup finishes, hello status of hello virtual machine in hello **Protected Items** tab is *Protected*.</span></span>

![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/protect-backedupvm.png)

## <a name="viewing-backup-status-and-details"></a><span data-ttu-id="fb467-221">Zobrazení stavu zálohy a podrobnosti</span><span class="sxs-lookup"><span data-stu-id="fb467-221">Viewing backup status and details</span></span>
<span data-ttu-id="fb467-222">Jakmile chráněný, počet virtuálních počítačů hello také nárůstu hello **řídicí panel** stránky Souhrn.</span><span class="sxs-lookup"><span data-stu-id="fb467-222">Once protected, hello virtual machine count also increases in hello **Dashboard** page summary.</span></span> <span data-ttu-id="fb467-223">Hello **řídicí panel** stránka zobrazuje také počet hello úlohy z hello posledních 24 hodin, které byly *úspěšné*, mají *se nezdařilo*a jsou *v průběhu*.</span><span class="sxs-lookup"><span data-stu-id="fb467-223">hello **Dashboard** page also shows hello number of jobs from hello last 24 hours that were *successful*, have *failed*, and are *in progress*.</span></span> <span data-ttu-id="fb467-224">Na hello **úlohy** stránky, použijte hello **stav**, **operace**, nebo **z** a **k** toofilter nabídky Hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="fb467-224">On hello **Jobs** page, use hello **Status**, **Operation**, or **From** and **To** menus toofilter hello jobs.</span></span>

![Stav služby backup v stránka řídicího panelu](./media/backup-azure-vms/dashboard-protectedvms.png)

<span data-ttu-id="fb467-226">Hodnoty v řídicím panelu hello se aktualizuje každých 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="fb467-226">Values in hello dashboard are refreshed once every 24 hours.</span></span>

## <a name="troubleshooting-errors"></a><span data-ttu-id="fb467-227">Řešení potíží s chybami</span><span class="sxs-lookup"><span data-stu-id="fb467-227">Troubleshooting errors</span></span>
<span data-ttu-id="fb467-228">Pokud narazíte na problémy při zálohování virtuálního počítače, podívejte se na hello [článku Poradce při potížích virtuálních počítačů](backup-azure-vms-troubleshoot.md) nápovědu.</span><span class="sxs-lookup"><span data-stu-id="fb467-228">If you run into issues while backing up your virtual machine, look at hello [VM     troubleshooting article](backup-azure-vms-troubleshoot.md) for help.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb467-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fb467-229">Next steps</span></span>
* [<span data-ttu-id="fb467-230">Správa a monitorování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fb467-230">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="fb467-231">Obnovení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fb467-231">Restore virtual machines</span></span>](backup-azure-restore-vms.md)
