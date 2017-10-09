---
title: "První pohled: Zálohování virtuálních počítačů Azure s trezorem zálohování | Dokumentace Microsoftu"
description: "Pomocí klasického portálu tooback hello do trezoru služby Backup tooa virtuálních počítačích Azure. Tento kurz vysvětluje všechny fáze, včetně vytváření hello úložiště záloh, registrace hello virtuálních počítačů, vytvoření zásady zálohování a spouštění hello úlohu prvotního zálohování."
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
ms.openlocfilehash: 77700e69eab9faccbc7ef923e1eb4e1f97be75d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="first-look-backing-up-azure-virtual-machines"></a><span data-ttu-id="5e3e9-104">První pohled: Zálohování virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="5e3e9-104">First look: Backing up Azure virtual machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5e3e9-105">Ochrana virtuálních počítačů v trezoru služby Recovery Services</span><span class="sxs-lookup"><span data-stu-id="5e3e9-105">Protect VMs with a recovery services vault</span></span>](backup-azure-vms-first-look-arm.md)
> * [<span data-ttu-id="5e3e9-106">Ochrana virtuálních počítačů Azure s trezorem zálohování</span><span class="sxs-lookup"><span data-stu-id="5e3e9-106">Protect Azure VMs with a backup vault</span></span>](backup-azure-vms-first-look.md)
>
>

<span data-ttu-id="5e3e9-107">Tento kurz vás provede kroky hello k zálohování virtuálních počítačů (VM) Azure tooa zálohování trezoru služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-107">This tutorial takes you through hello steps for backing up an Azure virtual machine (VM) tooa backup vault in Azure.</span></span> <span data-ttu-id="5e3e9-108">Tento článek popisuje hello klasického modelu nebo model nasazení portálu Service Manager, pro zálohování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-108">This article describes hello classic model or Service Manager deployment model, for backing up VMs.</span></span> <span data-ttu-id="5e3e9-109">Hello následující postup se vztahuje pouze tooBackup trezory vytvořit na portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-109">hello following steps apply only tooBackup vaults created in hello classic portal.</span></span> <span data-ttu-id="5e3e9-110">Společnost Microsoft doporučuje pro nová nasazení pomocí modelu Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-110">Microsoft recommends using hello Resource Manager model for new deployments.</span></span>

<span data-ttu-id="5e3e9-111">Pokud vás zajímá zálohování trezor služeb zotavení tooa virtuálních počítačů, které patří tooa skupinu prostředků, přečtěte si téma [první pohled: ochrana virtuálních počítačů s trezoru služeb zotavení](backup-azure-vms-first-look-arm.md).</span><span class="sxs-lookup"><span data-stu-id="5e3e9-111">If you are interested in backing up a VM tooa Recovery Services vault that belongs tooa Resource Group, see [First look: Protect VMs with a recovery services vault](backup-azure-vms-first-look-arm.md).</span></span>

<span data-ttu-id="5e3e9-112">toosuccessfully dokončit následující hello kurzu, musí existovat tyto požadavky:</span><span class="sxs-lookup"><span data-stu-id="5e3e9-112">toosuccessfully complete hello following tutorial, these prerequisites must exist:</span></span>

* <span data-ttu-id="5e3e9-113">Vytvořili jste virtuální počítač v rámci svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-113">You have created a VM in your Azure subscription.</span></span>
* <span data-ttu-id="5e3e9-114">Hello virtuálních počítačů má připojení tooAzure veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-114">hello VM has connectivity tooAzure public IP addresses.</span></span> <span data-ttu-id="5e3e9-115">Další informace naleznete v tématu [Připojení k síti](backup-azure-vms-prepare.md#network-connectivity).</span><span class="sxs-lookup"><span data-stu-id="5e3e9-115">For additional information, see [Network connectivity](backup-azure-vms-prepare.md#network-connectivity).</span></span>


> [!NOTE]
> <span data-ttu-id="5e3e9-116">Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5e3e9-116">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="5e3e9-117">Tento kurz je určen pro použití s virtuální počítače vytvořené na portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-117">This tutorial is for use with virtual machines created in hello classic portal.</span></span>
>
>

## <a name="create-a-backup-vault"></a><span data-ttu-id="5e3e9-118">Vytvoření trezoru záloh</span><span class="sxs-lookup"><span data-stu-id="5e3e9-118">Create a backup vault</span></span>
<span data-ttu-id="5e3e9-119">Trezor záloh je entita, která ukládá všechny hello zálohy a body obnovení, které byly vytvořeny v čase.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-119">A backup vault is an entity that stores all hello backups and recovery points that have been created over time.</span></span> <span data-ttu-id="5e3e9-120">Hello trezor záloh obsahuje také zásady zálohování hello, které jsou zálohované použité toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-120">hello backup vault also contains hello backup policies that are applied toohello virtual machines being backed up.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5e3e9-121">Od března 2017 se už můžete trezory Backup portálu classic toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-121">Starting March 2017, you can no longer use hello classic portal toocreate Backup vaults.</span></span>
> <span data-ttu-id="5e3e9-122">Můžete upgradovat vaše trezory služeb tooRecovery trezory Backup.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-122">You can upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="5e3e9-123">Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="5e3e9-123">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="5e3e9-124">Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-124">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="5e3e9-125">Po 15 říjen 2017 nemůžete použít trezory Backup toocreate prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-125">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="5e3e9-126">**Do 1. listopadu 2017:**</span><span class="sxs-lookup"><span data-stu-id="5e3e9-126">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="5e3e9-127">Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-127">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="5e3e9-128">Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-128">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="5e3e9-129">Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-129">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="discover-and-register-azure-virtual-machines"></a><span data-ttu-id="5e3e9-130">Vyhledání a registrace virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="5e3e9-130">Discover and Register Azure virtual machines</span></span>
<span data-ttu-id="5e3e9-131">Před registrací hello virtuálních počítačů k trezoru, spusťte tooidentify proces zjišťování hello nových virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-131">Before registering hello VM with a vault, run hello discovery process tooidentify any new VMs.</span></span> <span data-ttu-id="5e3e9-132">Tento příkaz vrátí seznam virtuálních počítačů v rámci předplatného hello, společně s dalšími informacemi, jako je název hello cloudové služby a oblast hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-132">This returns a list of virtual machines in hello subscription, along with additional information like hello cloud service name and hello region.</span></span>

1. <span data-ttu-id="5e3e9-133">Přihlaste se toohello [portálu Azure Classic](http://manage.windowsazure.com/)</span><span class="sxs-lookup"><span data-stu-id="5e3e9-133">Sign in toohello [Azure Classic portal](http://manage.windowsazure.com/)</span></span>
2. <span data-ttu-id="5e3e9-134">V hello portál Azure classic, klikněte na **služeb zotavení** tooopen hello seznamu trezorů služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-134">In hello Azure classic portal, click **Recovery Services** tooopen hello list of Recovery Services vaults.</span></span>
    <span data-ttu-id="5e3e9-135">![Výběr úlohy](./media/backup-azure-vms-first-look/recovery-services-icon.png)</span><span class="sxs-lookup"><span data-stu-id="5e3e9-135">![Select workload](./media/backup-azure-vms-first-look/recovery-services-icon.png)</span></span>
3. <span data-ttu-id="5e3e9-136">Hello seznamu trezorů vyberte trezor tooback hello si virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-136">From hello list of vaults, select hello vault tooback up a VM.</span></span>

    <span data-ttu-id="5e3e9-137">Když vyberete svůj trezor, otevře se v hello **rychlý Start** stránky</span><span class="sxs-lookup"><span data-stu-id="5e3e9-137">When you select your vault, it opens in hello **Quick Start** page</span></span>
4. <span data-ttu-id="5e3e9-138">Z nabídky hello trezoru, klikněte na tlačítko **registrované položky**.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-138">From hello vault menu, click **Registered Items**.</span></span>

    ![Výběr úlohy](./media/backup-azure-vms-first-look/configure-registered-items.png)
5. <span data-ttu-id="5e3e9-140">Z hello **typ** nabídce vyberte možnost **virtuální počítač Azure**.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-140">From hello **Type** menu, select **Azure Virtual Machine**.</span></span>

    ![Výběr úlohy](./media/backup-azure-vms/discovery-select-workload.png)
6. <span data-ttu-id="5e3e9-142">Klikněte na tlačítko **DISCOVER** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-142">Click **DISCOVER** at hello bottom of hello page.</span></span>
    <span data-ttu-id="5e3e9-143">![Tlačítko Vyhledat](./media/backup-azure-vms/discover-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="5e3e9-143">![Discover button](./media/backup-azure-vms/discover-button-only.png)</span></span>

    <span data-ttu-id="5e3e9-144">proces zjišťování Hello může trvat několik minut hello virtuální počítače jsou se v tabulce.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-144">hello discovery process may take a few minutes while hello virtual machines are being tabulated.</span></span> <span data-ttu-id="5e3e9-145">Je oznámení dole hello úvodní obrazovka, která vás informuje, zda je spuštěn proces hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-145">There is a notification at hello bottom of hello screen that lets you know that hello process is running.</span></span>

    ![Vyhledání virtuálních počítačů](./media/backup-azure-vms/discovering-vms.png)

    <span data-ttu-id="5e3e9-147">Hello oznámení změny po dokončení procesu hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-147">hello notification changes when hello process is complete.</span></span>

    ![Vyhledávání dokončeno](./media/backup-azure-vms-first-look/discovery-complete.png)
7. <span data-ttu-id="5e3e9-149">Klikněte na tlačítko **ZAREGISTROVAT** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-149">Click **REGISTER** at hello bottom of hello page.</span></span>
    <span data-ttu-id="5e3e9-150">![Tlačítko Registrovat](./media/backup-azure-vms-first-look/register-icon.png)</span><span class="sxs-lookup"><span data-stu-id="5e3e9-150">![Register button](./media/backup-azure-vms-first-look/register-icon.png)</span></span>
8. <span data-ttu-id="5e3e9-151">V hello **registrovat položky** místní nabídky, vyberte hello virtuálních počítačů, které chcete tooregister.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-151">In hello **Register Items** shortcut menu, select hello virtual machines that you want tooregister.</span></span>

   > [!TIP]
   > <span data-ttu-id="5e3e9-152">Najednou může být registrováno více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-152">Multiple virtual machines can be registered at one time.</span></span>
   >
   >

    <span data-ttu-id="5e3e9-153">Pro každý virtuální počítač, který jste vybrali, se vytvoří úloha.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-153">A job is created for each virtual machine that you've selected.</span></span>
9. <span data-ttu-id="5e3e9-154">Klikněte na tlačítko **zobrazit úlohy** v hello oznámení toogo toohello **úlohy** stránky.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-154">Click **View Job** in hello notification toogo toohello **Jobs** page.</span></span>

    ![Registrace úlohy](./media/backup-azure-vms/register-create-job.png)

    <span data-ttu-id="5e3e9-156">virtuální počítač Hello se také zobrazí v hello seznamu registrovaných položek společně hello stav operace Registrování hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-156">hello virtual machine also appears in hello list of registered items, along with hello status of hello registration operation.</span></span>

    ![Stav registrace 1](./media/backup-azure-vms/register-status01.png)

    <span data-ttu-id="5e3e9-158">Po dokončení operace hello hello stav se změní tooreflect hello *zaregistrován* stavu.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-158">When hello operation completes, hello status changes tooreflect hello *registered* state.</span></span>

    ![Stav registrace 2](./media/backup-azure-vms/register-status02.png)

## <a name="install-hello-vm-agent-on-hello-virtual-machine"></a><span data-ttu-id="5e3e9-160">Nainstalujte agenta virtuálního počítače hello hello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5e3e9-160">Install hello VM Agent on hello virtual machine</span></span>
<span data-ttu-id="5e3e9-161">Hello agenta virtuálního počítače Azure musí být nainstalován na hello virtuálního počítače, které jsou pro toowork rozšíření hello zálohování Azure.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-161">hello Azure VM Agent must be installed on hello Azure virtual machine for hello Backup extension toowork.</span></span> <span data-ttu-id="5e3e9-162">Pokud virtuální počítač byl vytvořen z Galerie Azure hello, hello agenta virtuálního počítače je již nainstalován na hello virtuálních počítačů; můžete přeskočit příliš[ochranu virtuálních počítačů](backup-azure-vms-first-look.md#create-the-backup-policy).</span><span class="sxs-lookup"><span data-stu-id="5e3e9-162">If your VM was created from hello Azure gallery, hello VM Agent is already present on hello VM; you can skip too[protecting your VMs](backup-azure-vms-first-look.md#create-the-backup-policy).</span></span>

<span data-ttu-id="5e3e9-163">Pokud virtuální počítač přenesen z překážek místní datacentra, hello virtuální počítač pravděpodobně neobsahuje hello nainstalovaný Agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-163">If your VM migrated from an on-premises datacenter, hello VM probably does not have hello VM Agent installed.</span></span> <span data-ttu-id="5e3e9-164">Hello agenta virtuálního počítače je nutné nainstalovat na virtuální počítač hello před pokračováním tooprotect hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-164">You must install hello VM Agent on hello virtual machine before proceeding tooprotect hello VM.</span></span> <span data-ttu-id="5e3e9-165">Podrobné pokyny k instalaci hello agenta virtuálního počítače, najdete v části hello [agenta virtuálního počítače části článku zálohování virtuálních počítačů hello](backup-azure-vms-prepare.md#vm-agent).</span><span class="sxs-lookup"><span data-stu-id="5e3e9-165">For detailed steps on installing hello VM Agent, see hello [VM Agent section of hello Backup VMs article](backup-azure-vms-prepare.md#vm-agent).</span></span>

## <a name="create-hello-backup-policy"></a><span data-ttu-id="5e3e9-166">Vytvořit zásady zálohování hello</span><span class="sxs-lookup"><span data-stu-id="5e3e9-166">Create hello backup policy</span></span>
<span data-ttu-id="5e3e9-167">Předtím, než aktivujete úlohu prvotního zálohování hello, nastavte plán hello při pořizování snímků záloh.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-167">Before you trigger hello initial backup job, set hello schedule when backup snapshots are taken.</span></span> <span data-ttu-id="5e3e9-168">Hello naplánovat, kdy jsou pořízeny snímků zálohy a hello dobu tyto snímky jsou zachována, je hello zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-168">hello schedule when backup snapshots are taken, and hello length of time those snapshots are retained, is hello backup policy.</span></span> <span data-ttu-id="5e3e9-169">informace o zachování Hello je založena na Dědečka. otec SYN schématu rotace záloh.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-169">hello retention information is based on Grandfather-father-son backup rotation scheme.</span></span>

1. <span data-ttu-id="5e3e9-170">Přejděte toohello zálohy trezoru pod **služeb zotavení** v hello portálu Azure Classic a klikněte na tlačítko **registrované položky**.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-170">Navigate toohello backup vault under **Recovery Services** in hello Azure Classic portal, and  click **Registered Items**.</span></span>
2. <span data-ttu-id="5e3e9-171">Vyberte **virtuální počítač Azure** z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-171">Select **Azure Virtual Machine** from hello drop-down menu.</span></span>

    ![Výběr úlohy v portálu](./media/backup-azure-vms/select-workload.png)
3. <span data-ttu-id="5e3e9-173">Klikněte na tlačítko **CHRÁNIT** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-173">Click **PROTECT** at hello bottom of hello page.</span></span>
    <span data-ttu-id="5e3e9-174">![Klikněte na Chránit](./media/backup-azure-vms-first-look/protect-icon.png)</span><span class="sxs-lookup"><span data-stu-id="5e3e9-174">![Click Protect](./media/backup-azure-vms-first-look/protect-icon.png)</span></span>

    <span data-ttu-id="5e3e9-175">Hello **Průvodce ochranou položek** se zobrazí a uvádí *pouze* virtuálních počítačů, které jsou zaregistrované a nechráněné.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-175">hello **Protect Items wizard** appears and lists *only* virtual machines that are registered and not protected.</span></span>

    ![Škálování konfigurace ochrany](./media/backup-azure-vms/protect-at-scale.png)
4. <span data-ttu-id="5e3e9-177">Vyberte hello virtuálních počítačů, které chcete tooprotect.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-177">Select hello virtual machines that you want tooprotect.</span></span>

    <span data-ttu-id="5e3e9-178">Pokud existují dvě nebo více virtuálních počítačů s hello stejný název, použijte hello Cloudová služba toodistinguish mezi virtuálními počítači hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-178">If there are two or more virtual machines with hello same name, use hello Cloud Service toodistinguish between hello virtual machines.</span></span>
5. <span data-ttu-id="5e3e9-179">Na hello **Konfigurace ochrany** nabídce vyberte existující zásadu nebo vytvořte nové zásady tooprotect hello virtuálních počítačů, které jste určili.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-179">On hello **Configure protection** menu select an existing policy or create a new policy tooprotect hello virtual machines that you identified.</span></span>

    <span data-ttu-id="5e3e9-180">Nové trezory Backup mít výchozí zásady přidružený k trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-180">New Backup vaults have a default policy associated with hello vault.</span></span> <span data-ttu-id="5e3e9-181">Tato zásada pořizuje každý večer a denní snímek hello se uchovávají po dobu 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-181">This policy takes a daily snapshot each evening, and hello daily snapshot is retained for 30 days.</span></span> <span data-ttu-id="5e3e9-182">Ke každé zásadě zálohování může být přidružených více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-182">Each backup policy can have multiple virtual machines associated with it.</span></span> <span data-ttu-id="5e3e9-183">Ale hello virtuálního počítače lze přidružit pouze jeden zásadám najednou.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-183">However, hello virtual machine can only be associated with one policy at a time.</span></span>

    ![Ochrana pomocí nové zásady](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > <span data-ttu-id="5e3e9-185">Zásada zálohování obsahuje schéma uchovávání pro hello naplánované zálohování.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-185">A backup policy includes a retention scheme for hello scheduled backups.</span></span> <span data-ttu-id="5e3e9-186">Pokud vyberete existující zásady zálohování, bude v dalším kroku hello možnosti uchovávání nelze toomodify hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-186">If you select an existing backup policy, you will be unable toomodify hello retention options in hello next step.</span></span>
   >
   >
6. <span data-ttu-id="5e3e9-187">Na **rozsah uchování** definovat hello denní, týdenní, měsíční nebo roční rozsah pro konkrétní body zálohy hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-187">On **Retention Range** define hello daily, weekly, monthly, and yearly scope for hello specific backup points.</span></span>

    ![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/long-term-retention.png)

    <span data-ttu-id="5e3e9-189">Zásady uchovávání informací určuje hello dobu pro uložení zálohy.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-189">Retention policy specifies hello length of time for storing a backup.</span></span> <span data-ttu-id="5e3e9-190">Můžete zadat různé zásady uchovávání informací na základě při pořízení zálohy hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-190">You can specify different retention policies based on when hello backup is taken.</span></span>
7. <span data-ttu-id="5e3e9-191">Klikněte na tlačítko **úlohy** seznamu hello tooview **Konfigurace ochrany** úlohy.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-191">Click **Jobs** tooview hello list of **Configure Protection** jobs.</span></span>

    ![Úloha konfigurace ochrany](./media/backup-azure-vms/protect-configureprotection.png)

    <span data-ttu-id="5e3e9-193">Teď, když jste vytvořili hello zásad, přejděte toohello další krok a spusťte prvotní zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-193">Now that you've established hello policy, go toohello next step and run hello initial backup.</span></span>

## <a name="initial-backup"></a><span data-ttu-id="5e3e9-194">Prvotní zálohování</span><span class="sxs-lookup"><span data-stu-id="5e3e9-194">Initial backup</span></span>
<span data-ttu-id="5e3e9-195">Jakmile je virtuální počítač je chráněný pomocí zásad, můžete zobrazit tuto relaci na hello **chráněné položky** kartě. Dokud hello proběhne prvotní zálohování, hello **stav ochrany** zobrazuje jako **chráněno – (čekání na prvotní zálohování)**.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-195">Once a virtual machine has been protected with a policy, you can view that relationship on hello **Protected Items** tab. Until hello initial backup occurs, hello **Protection Status** shows as **Protected - (pending initial backup)**.</span></span> <span data-ttu-id="5e3e9-196">Ve výchozím nastavení, je prvním plánovaným zálohováním hello hello *prvotní zálohování*.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-196">By default, hello first scheduled backup is hello *initial backup*.</span></span>

![Zálohování čeká na zpracování](./media/backup-azure-vms-first-look/protection-pending-border.png)

<span data-ttu-id="5e3e9-198">toostart hello prvotní zálohování nyní:</span><span class="sxs-lookup"><span data-stu-id="5e3e9-198">toostart hello initial backup now:</span></span>

1. <span data-ttu-id="5e3e9-199">Na hello **chráněné položky** klikněte na tlačítko **zálohovat nyní** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-199">On hello **Protected Items** page, click **Backup Now** at hello bottom of hello page.</span></span>
    <span data-ttu-id="5e3e9-200">![Ikona Zálohovat nyní](./media/backup-azure-vms-first-look/backup-now-icon.png)</span><span class="sxs-lookup"><span data-stu-id="5e3e9-200">![Backup Now icon](./media/backup-azure-vms-first-look/backup-now-icon.png)</span></span>

    <span data-ttu-id="5e3e9-201">Hello služba Azure Backup vytvoří úlohu zálohování pro operaci prvotního zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-201">hello Azure Backup service creates a backup job for hello initial backup operation.</span></span>
2. <span data-ttu-id="5e3e9-202">Klikněte na tlačítko hello **úlohy** kartě tooview hello seznamu úloh.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-202">Click hello **Jobs** tab tooview hello list of jobs.</span></span>

    ![Probíhá zálohování](./media/backup-azure-vms-first-look/protect-inprogress.png)

    <span data-ttu-id="5e3e9-204">Po dokončení prvotní zálohování, stav hello hello virtuálního počítače v hello **chráněné položky** karta je *chráněné*.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-204">When initial backup is complete, hello status of hello virtual machine in hello **Protected Items** tab is *Protected*.</span></span>

    ![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/protect-backedupvm.png)

   > [!NOTE]
   > <span data-ttu-id="5e3e9-206">Zálohování virtuálních počítačů je místní proces.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-206">Backing up virtual machines is a local process.</span></span> <span data-ttu-id="5e3e9-207">Z jedné oblasti úložiště tooa záloh, které v jiné oblasti nelze zálohovat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-207">You cannot back up virtual machines from one region tooa backup vault in another region.</span></span> <span data-ttu-id="5e3e9-208">Ano pro každou oblast Azure, který má virtuální počítače, které je třeba zálohovat toobe, je nutné vytvořit alespoň jeden trezor záloh v této oblasti.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-208">So, for every Azure region that has VMs that need toobe backed up, at least one backup vault must be created in that region.</span></span>
   >
   >

## <a name="next-steps"></a><span data-ttu-id="5e3e9-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5e3e9-209">Next steps</span></span>
<span data-ttu-id="5e3e9-210">Když jste teď úspěšně zálohovali virtuální počítač, je několik dalších kroků, které by vás mohly zajímat.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-210">Now that you have successfully backed up a VM, there are several next steps that could be of interest.</span></span> <span data-ttu-id="5e3e9-211">Hello nejlogičtějším krokem je toofamiliarize sami s obnovováním dat tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-211">hello most logical step is toofamiliarize yourself with restoring data tooa VM.</span></span> <span data-ttu-id="5e3e9-212">Nicméně existují úlohy správy, které vám pomohou pochopit, jak tookeep bezpečné dat a minimalizovat náklady.</span><span class="sxs-lookup"><span data-stu-id="5e3e9-212">However, there are management tasks that will help you understand how tookeep your data safe and minimize costs.</span></span>

* [<span data-ttu-id="5e3e9-213">Správa a monitorování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5e3e9-213">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="5e3e9-214">Obnovení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5e3e9-214">Restore virtual machines</span></span>](backup-azure-restore-vms.md)
* [<span data-ttu-id="5e3e9-215">Pokyny při řešení potíží</span><span class="sxs-lookup"><span data-stu-id="5e3e9-215">Troubleshooting guidance</span></span>](backup-azure-vms-troubleshoot.md)

## <a name="questions"></a><span data-ttu-id="5e3e9-216">Máte dotazy?</span><span class="sxs-lookup"><span data-stu-id="5e3e9-216">Questions?</span></span>
<span data-ttu-id="5e3e9-217">Pokud máte otázky, nebo pokud se některé funkce, které byste chtěli toosee zahrnuty, [pošlete nám svůj názor](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="5e3e9-217">If you have questions, or if there is any feature that you would like toosee included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>
