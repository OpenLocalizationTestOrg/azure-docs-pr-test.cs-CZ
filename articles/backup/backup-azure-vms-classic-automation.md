---
title: "Nasadit a spravovat zálohy pro virtuální počítače Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak nasadit a spravovat Azure Backup pomocí prostředí PowerShell."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 2e24b1d9-4375-4049-a28d-e3bc01152f32
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/2/2017
ms.author: markgal;trinadhk;jimpark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5f0f06adb8177ce2d17aa0b40666470279c04e22
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-azurermbackup-cmdlets-to-back-up-virtual-machines"></a><span data-ttu-id="84cb9-103">Pomocí rutin AzureRM.Backup zálohování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="84cb9-103">Use AzureRM.Backup cmdlets to back up virtual machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="84cb9-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="84cb9-104">Resource Manager</span></span>](backup-azure-vms-automation.md)
> * [<span data-ttu-id="84cb9-105">Classic</span><span class="sxs-lookup"><span data-stu-id="84cb9-105">Classic</span></span>](backup-azure-vms-classic-automation.md)
>
>

<span data-ttu-id="84cb9-106">Tento článek ukazuje, jak pomocí prostředí Azure PowerShell pro zálohování a obnovení virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="84cb9-106">This article shows you how to use Azure PowerShell for backup and recovery of Azure VMs.</span></span> <span data-ttu-id="84cb9-107">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: Resource Manager a Classic.</span><span class="sxs-lookup"><span data-stu-id="84cb9-107">Azure has two different deployment models for creating and working with resources: Resource Manager and Classic.</span></span> <span data-ttu-id="84cb9-108">Tento článek se zabývá pomocí modelu nasazení Classic k zálohování dat do úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="84cb9-108">This article covers using the Classic deployment model to back up data to a Backup vault.</span></span> <span data-ttu-id="84cb9-109">Pokud jste dosud nevytvořili úložiště záloh ve vašem předplatném, najdete v části správce prostředků verzi tohoto článku, [AzureRM.RecoveryServices.Backup použití rutiny pro zálohování virtuálních počítačů](backup-azure-vms-automation.md).</span><span class="sxs-lookup"><span data-stu-id="84cb9-109">If you have not created a Backup vault in your subscription, see the Resource Manager version of this article, [Use AzureRM.RecoveryServices.Backup cmdlets to back up virtual machines](backup-azure-vms-automation.md).</span></span> <span data-ttu-id="84cb9-110">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="84cb9-110">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="84cb9-111">Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="84cb9-111">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="84cb9-112">Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="84cb9-112">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="84cb9-113">Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="84cb9-113">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="84cb9-114">Od 15. října 2017 nebude možné pomocí PowerShellu vytvářet trezory služby Backup.</span><span class="sxs-lookup"><span data-stu-id="84cb9-114">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="84cb9-115">**Do 1. listopadu 2017:**</span><span class="sxs-lookup"><span data-stu-id="84cb9-115">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="84cb9-116">Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="84cb9-116">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="84cb9-117">Nebudete mít přístup k datům záloh na portálu Classic.</span><span class="sxs-lookup"><span data-stu-id="84cb9-117">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="84cb9-118">Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="84cb9-118">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="concepts"></a><span data-ttu-id="84cb9-119">Koncepty</span><span class="sxs-lookup"><span data-stu-id="84cb9-119">Concepts</span></span>
<span data-ttu-id="84cb9-120">Tento článek obsahuje informace specifické pro rutiny prostředí PowerShell použít k zálohování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="84cb9-120">This article provides information specific to the PowerShell cmdlets used to back up virtual machines.</span></span> <span data-ttu-id="84cb9-121">Úvodní informace o ochraně virtuálních počítačích Azure, najdete v tématu [plánování vaší infrastruktury zálohování virtuálních počítačů v Azure](backup-azure-vms-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="84cb9-121">For introductory information about protecting Azure VMs, please see [Plan your VM backup infrastructure in Azure](backup-azure-vms-introduction.md).</span></span>

> [!NOTE]
> <span data-ttu-id="84cb9-122">Než začnete, přečtěte si [požadavky](backup-azure-vms-prepare.md) potřebné pro práci s Azure Backup a [omezení](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) aktuální virtuálního počítače řešení pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="84cb9-122">Before you start, read the [prerequisites](backup-azure-vms-prepare.md) required to work with Azure Backup, and the [limitations](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) of the current VM backup solution.</span></span>
>
>

<span data-ttu-id="84cb9-123">Pomocí prostředí PowerShell efektivně, pozorně pochopit hierarchii objektů a odkud spustit.</span><span class="sxs-lookup"><span data-stu-id="84cb9-123">To use PowerShell effectively, take a moment to understand the hierarchy of objects and from where to start.</span></span>

![Hierarchie objektů](./media/backup-azure-vms-classic-automation/object-hierarchy.png)

<span data-ttu-id="84cb9-125">Dva nejdůležitější toky jsou povolení ochrany pro virtuální počítač a obnovení dat z bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="84cb9-125">The two most important flows are enabling protection for a VM, and restoring data from a recovery point.</span></span> <span data-ttu-id="84cb9-126">Cílem tohoto článku je se adept na práci s rutinami prostředí PowerShell, aby tyto dva scénáře lépe.</span><span class="sxs-lookup"><span data-stu-id="84cb9-126">The focus of this article is to help you become adept at working with the PowerShell cmdlets to enable these two scenarios.</span></span>

## <a name="setup-and-registration"></a><span data-ttu-id="84cb9-127">Instalace a registrace</span><span class="sxs-lookup"><span data-stu-id="84cb9-127">Setup and Registration</span></span>
<span data-ttu-id="84cb9-128">Chcete-li začít:</span><span class="sxs-lookup"><span data-stu-id="84cb9-128">To begin:</span></span>

1. <span data-ttu-id="84cb9-129">[Stáhněte si nejnovější PowerShell](https://github.com/Azure/azure-powershell/releases) (minimální požadovaná verze je: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="84cb9-129">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>
2. <span data-ttu-id="84cb9-130">Rutiny prostředí PowerShell zálohování Azure k dispozici najděte tak, že zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="84cb9-130">Find the Azure Backup PowerShell cmdlets available by typing the following command:</span></span>

```
PS C:\> Get-Command *azurermbackup*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Backup-AzureRmBackupItem                           1.0.1      AzureRM.Backup
Cmdlet          Disable-AzureRmBackupProtection                    1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupContainerReregistration        1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupProtection                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupContainer                         1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupItem                              1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJob                               1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJobDetails                        1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupRecoveryPoint                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVaultCredentials                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupRetentionPolicyObject             1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Register-AzureRmBackupContainer                    1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupProtectionPolicy               1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupVault                          1.0.1      AzureRM.Backup
Cmdlet          Restore-AzureRmBackupItem                          1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Stop-AzureRmBackupJob                              1.0.1      AzureRM.Backup
Cmdlet          Unregister-AzureRmBackupContainer                  1.0.1      AzureRM.Backup
Cmdlet          Wait-AzureRmBackupJob                              1.0.1      AzureRM.Backup
```

<span data-ttu-id="84cb9-131">Následující instalaci a registraci úlohy je možné automatizovat pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="84cb9-131">The following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="84cb9-132">Vytvoření trezoru záloh</span><span class="sxs-lookup"><span data-stu-id="84cb9-132">Create a backup vault</span></span>
* <span data-ttu-id="84cb9-133">Registrace virtuálních počítačů pomocí služby Azure Backup</span><span class="sxs-lookup"><span data-stu-id="84cb9-133">Registering the VMs with the Azure Backup service</span></span>

### <a name="create-a-backup-vault"></a><span data-ttu-id="84cb9-134">Vytvoření trezoru záloh</span><span class="sxs-lookup"><span data-stu-id="84cb9-134">Create a backup vault</span></span>
> [!WARNING]
> <span data-ttu-id="84cb9-135">Pro zákazníky pomocí služby Azure Backup poprvé budete muset registraci poskytovatele Azure Backup pro použití s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="84cb9-135">For customers using Azure Backup for the first time, you need to register the Azure Backup provider to be used with your subscription.</span></span> <span data-ttu-id="84cb9-136">To můžete provést spuštěním následujícího příkazu: Register-AzureRmResourceProvider - ProviderNamespace "Microsoft.Backup"</span><span class="sxs-lookup"><span data-stu-id="84cb9-136">This can be done by running the following command: Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Backup"</span></span>
>
>

<span data-ttu-id="84cb9-137">Můžete vytvořit nové úložiště záloh pomocí **New-AzureRmBackupVault** rutiny.</span><span class="sxs-lookup"><span data-stu-id="84cb9-137">You can create a new backup vault using the **New-AzureRmBackupVault** cmdlet.</span></span> <span data-ttu-id="84cb9-138">Trezor záloh je prostředek ARM, proto musíte umístit do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="84cb9-138">The backup vault is an ARM resource, so you need to place it within a Resource Group.</span></span> <span data-ttu-id="84cb9-139">V prostředí Azure PowerShell konzolu se zvýšenými oprávněními spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="84cb9-139">In an elevated Azure PowerShell console, run the following commands:</span></span>

```
PS C:\> New-AzureRmResourceGroup –Name “test-rg” –Location “West US”
PS C:\> $backupvault = New-AzureRmBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

<span data-ttu-id="84cb9-140">Můžete získat seznam všech záloh v daném předplatném pomocí **Get-AzureRmBackupVault** rutiny.</span><span class="sxs-lookup"><span data-stu-id="84cb9-140">You can get a list of all the backup vaults in a given subscription using the **Get-AzureRmBackupVault** cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="84cb9-141">Je vhodné pro uložení objektu úložiště záloh do proměnné.</span><span class="sxs-lookup"><span data-stu-id="84cb9-141">It is convenient to store the backup vault object into a variable.</span></span> <span data-ttu-id="84cb9-142">Objekt trezoru je potřeba jako vstup pro mnoho rutin Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="84cb9-142">The vault object is needed as an input for many Azure Backup cmdlets.</span></span>
>
>

### <a name="registering-the-vms"></a><span data-ttu-id="84cb9-143">Registrace virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="84cb9-143">Registering the VMs</span></span>
<span data-ttu-id="84cb9-144">Prvním krokem ke konfiguraci zálohování Azure Backup je při registraci počítače nebo virtuální počítač s trezoru služby Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="84cb9-144">The first step towards configuring backup with Azure Backup is to register your machine or VM with an Azure Backup vault.</span></span> <span data-ttu-id="84cb9-145">**Register-AzureRmBackupContainer** rutina zpracovává vstupní informace virtuálního počítače Azure IaaS a zaregistruje ho se zadanou trezoru.</span><span class="sxs-lookup"><span data-stu-id="84cb9-145">The **Register-AzureRmBackupContainer** cmdlet takes the input information of an Azure IaaS virtual machine and registers it with the specified vault.</span></span> <span data-ttu-id="84cb9-146">Operaci registrace přidruží virtuální počítač Azure úložiště záloh a sleduje virtuálního počítače pomocí zálohování životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="84cb9-146">The register operation associates the Azure virtual machine with the backup vault and tracks the VM through the backup lifecycle.</span></span>

<span data-ttu-id="84cb9-147">Registrace virtuálního počítače pomocí služby Azure Backup vytvoří objekt kontejner nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="84cb9-147">Registering your VM with the Azure Backup service creates a top-level container object.</span></span> <span data-ttu-id="84cb9-148">Kontejner obvykle obsahuje více položek, které lze zálohovat, ale v případě virtuálních počítačů bude jen jednu položku Zálohování kontejneru.</span><span class="sxs-lookup"><span data-stu-id="84cb9-148">A container typically contains multiple items that can be backed up, but in the case of VMs there will be only one backup item for the container.</span></span>

```
PS C:\> $registerjob = Register-AzureRmBackupContainer -Vault $backupvault -Name "testvm" -ServiceName "testvm"
```

## <a name="backup-azure-vms"></a><span data-ttu-id="84cb9-149">Zálohování virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="84cb9-149">Backup Azure VMs</span></span>
### <a name="create-a-protection-policy"></a><span data-ttu-id="84cb9-150">Vytvoření zásady ochrany</span><span class="sxs-lookup"><span data-stu-id="84cb9-150">Create a protection policy</span></span>
<span data-ttu-id="84cb9-151">Není to povinné pro vytvoření nové zásady ochrany spustit zálohování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="84cb9-151">It is not mandatory to create a new protection policy to start backup of your VMs.</span></span> <span data-ttu-id="84cb9-152">Úložiště se dodává s 'výchozí zásadu", můžete použít k rychlému povolení ochrany a potom je upravit později pomocí podrobností vpravo.</span><span class="sxs-lookup"><span data-stu-id="84cb9-152">The vault comes with a 'Default Policy' that can be used to quickly enable protection, and then edited later with the right details.</span></span> <span data-ttu-id="84cb9-153">Seznam zásad, které jsou k dispozici v trezoru můžete získat pomocí **Get-AzureRmBackupProtectionPolicy** rutiny:</span><span class="sxs-lookup"><span data-stu-id="84cb9-153">You can get a list of the policies available in the vault by using the **Get-AzureRmBackupProtectionPolicy** cmdlet:</span></span>

```
PS C:\> Get-AzureRmBackupProtectionPolicy -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DefaultPolicy             AzureVM            Daily              26-Aug-15 12:30:00 AM
```

> [!NOTE]
> <span data-ttu-id="84cb9-154">Časové pásmo BackupTime pole v prostředí PowerShell je UTC.</span><span class="sxs-lookup"><span data-stu-id="84cb9-154">The timezone of the BackupTime field in PowerShell is UTC.</span></span> <span data-ttu-id="84cb9-155">Ale pokud čas zálohování se zobrazí na portálu Azure, časové pásmo je zarovnán do místního systému, společně s posunem od standardu UTC.</span><span class="sxs-lookup"><span data-stu-id="84cb9-155">However, when the backup time is shown in the Azure portal, the timezone is aligned to your local system along with the UTC offset.</span></span>
>
>

<span data-ttu-id="84cb9-156">Zásady zálohování je přidružen alespoň jeden zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="84cb9-156">A backup policy is associated with at least one retention policy.</span></span> <span data-ttu-id="84cb9-157">Zásady uchovávání informací definuje, jak dlouho se uchovává bodu obnovení se Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="84cb9-157">The retention policy defines how long a recovery point is kept with Azure Backup.</span></span> <span data-ttu-id="84cb9-158">**New-AzureRmBackupRetentionPolicy** rutina vytvoří prostředí PowerShell objekty, které obsahují informace o zásadách uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="84cb9-158">The **New-AzureRmBackupRetentionPolicy** cmdlet creates PowerShell objects that hold retention policy information.</span></span> <span data-ttu-id="84cb9-159">Tyto objekty zásad uchovávání dat se používají jako vstupy pro *New-AzureRmBackupProtectionPolicy* rutiny, nebo přímo pomocí *povolit AzureRmBackupProtection* rutiny.</span><span class="sxs-lookup"><span data-stu-id="84cb9-159">These retention policy objects are used as inputs to the *New-AzureRmBackupProtectionPolicy* cmdlet, or directly with the *Enable-AzureRmBackupProtection* cmdlet.</span></span>

<span data-ttu-id="84cb9-160">Zásady zálohování definují, kdy a jak často se provádí zálohování položky.</span><span class="sxs-lookup"><span data-stu-id="84cb9-160">A backup policy defines when and how often the backup of an item is done.</span></span> <span data-ttu-id="84cb9-161">**New-AzureRmBackupProtectionPolicy** rutina vytvoří objekt prostředí PowerShell, který obsahuje informace zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="84cb9-161">The **New-AzureRmBackupProtectionPolicy** cmdlet creates a PowerShell object that holds backup policy information.</span></span> <span data-ttu-id="84cb9-162">Zásady zálohování se používá jako vstup *povolit AzureRmBackupProtection* rutiny.</span><span class="sxs-lookup"><span data-stu-id="84cb9-162">The backup policy is used as an input to the *Enable-AzureRmBackupProtection* cmdlet.</span></span>

```
PS C:\> $Daily = New-AzureRmBackupRetentionPolicyObject -DailyRetention -Retention 30
PS C:\> $newpolicy = New-AzureRmBackupProtectionPolicy -Name DailyBackup01 -Type AzureVM -Daily -BackupTime ([datetime]"3:30 PM") -RetentionPolicy $Daily -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DailyBackup01             AzureVM            Daily              01-Sep-15 3:30:00 PM
```

### <a name="enable-protection"></a><span data-ttu-id="84cb9-163">Povolení ochrany</span><span class="sxs-lookup"><span data-stu-id="84cb9-163">Enable protection</span></span>
<span data-ttu-id="84cb9-164">Povolení ochrany zahrnuje dva objekty - položky a zásady, a obě musí patřit ke stejnému trezoru.</span><span class="sxs-lookup"><span data-stu-id="84cb9-164">Enabling protection involves two objects - the Item and the Policy, and both need to belong to the same vault.</span></span> <span data-ttu-id="84cb9-165">Jakmile zásady je už přidružená k položce, bude zálohování pracovního postupu nové podle definovaného plánu.</span><span class="sxs-lookup"><span data-stu-id="84cb9-165">Once the policy has been associated with the item, the backup workflow will kick in at the defined schedule.</span></span>

```
PS C:\> Get-AzureRmBackupContainer -Type AzureVM -Status Registered -Vault $backupvault | Get-AzureRmBackupItem | Enable-AzureRmBackupProtection -Policy $newpolicy
```

### <a name="initial-backup"></a><span data-ttu-id="84cb9-166">Prvotní zálohování</span><span class="sxs-lookup"><span data-stu-id="84cb9-166">Initial backup</span></span>
<span data-ttu-id="84cb9-167">Plán zálohování se postará o provádění a úplné počáteční kopie pro položku přírůstkové kopie pro následné zálohy.</span><span class="sxs-lookup"><span data-stu-id="84cb9-167">The backup schedule will take care of doing the full initial copy for the item and the incremental copy for the subsequent backups.</span></span> <span data-ttu-id="84cb9-168">Ale pokud chcete vynutit prvotní zálohování k dojít v určitém čase nebo i okamžitě použijte **zálohování AzureRmBackupItem** rutiny:</span><span class="sxs-lookup"><span data-stu-id="84cb9-168">However, if you want to force the initial backup to happen at a certain time or even immediately then use the **Backup-AzureRmBackupItem** cmdlet:</span></span>

```
PS C:\> $container = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -Name "testvm"
PS C:\> $backupjob = Get-AzureRmBackupItem -Container $container | Backup-AzureRmBackupItem
PS C:\> $backupjob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

> [!NOTE]
> <span data-ttu-id="84cb9-169">Časové pásmo čas spuštění a čas ukončení pole zobrazená v prostředí PowerShell je čas UTC.</span><span class="sxs-lookup"><span data-stu-id="84cb9-169">The timezone of the StartTime and EndTime fields shown in PowerShell is UTC.</span></span> <span data-ttu-id="84cb9-170">Ale když podobné informace se zobrazí na portálu Azure, časové pásmo je zarovnán do místních systémových hodin.</span><span class="sxs-lookup"><span data-stu-id="84cb9-170">However, when the similar information is shown in the Azure portal, the timezone is aligned to your local system clock.</span></span>
>
>

### <a name="monitoring-a-backup-job"></a><span data-ttu-id="84cb9-171">Monitorování úlohy zálohování</span><span class="sxs-lookup"><span data-stu-id="84cb9-171">Monitoring a backup job</span></span>
<span data-ttu-id="84cb9-172">Většina dlouho běžící operace ve službě Azure Backup se modelována jako úlohu.</span><span class="sxs-lookup"><span data-stu-id="84cb9-172">Most long-running operations in Azure Backup are modelled as a job.</span></span> <span data-ttu-id="84cb9-173">To umožňuje snadno sledovat průběh, aniž by bylo nutné zachovat portálu Azure otevřete za všech okolností.</span><span class="sxs-lookup"><span data-stu-id="84cb9-173">This makes it easy to track progress without having to keep the Azure portal open at all times.</span></span>

<span data-ttu-id="84cb9-174">Chcete-li získat nejnovější stav probíhající úlohy, použijte **Get-AzureRmBackupJob** rutiny.</span><span class="sxs-lookup"><span data-stu-id="84cb9-174">To get the latest status of an in-progress job, use the **Get-AzureRmBackupJob** cmdlet.</span></span>

```
PS C:\> $joblist = Get-AzureRmBackupJob -Vault $backupvault -Status InProgress
PS C:\> $joblist[0]

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

<span data-ttu-id="84cb9-175">Místo dotazování tyto úlohy pro dokončení – což je zbytečné, další kód – je jednodušší použít **čekání AzureRmBackupJob** rutiny.</span><span class="sxs-lookup"><span data-stu-id="84cb9-175">Instead of polling these jobs for completion - which is unnecessary, additional code - it is simpler to use the **Wait-AzureRmBackupJob** cmdlet.</span></span> <span data-ttu-id="84cb9-176">Při použití ve skriptu, bude rutina pozastavit provádění až do dokončení úlohy nebo zadaný časový limit.</span><span class="sxs-lookup"><span data-stu-id="84cb9-176">When used in a script, the cmdlet will pause the execution until either the job completes or the specified timeout value is reached.</span></span>

```
PS C:\> Wait-AzureRmBackupJob -Job $joblist[0] -Timeout 43200
```


## <a name="restore-an-azure-vm"></a><span data-ttu-id="84cb9-177">Obnovení virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="84cb9-177">Restore an Azure VM</span></span>
<span data-ttu-id="84cb9-178">Chcete-li obnovit zálohovaná data, musíte určit položku zálohovanou a bod obnovení, který obsahuje data v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="84cb9-178">In order to restore backup data, you need to identify the backed-up Item and the Recovery Point that holds the point-in-time data.</span></span> <span data-ttu-id="84cb9-179">Tyto informace jsou poskytovány pro rutinu obnovení AzureRmBackupItem zahájíte obnovení dat z trezoru s účtem zákazníka.</span><span class="sxs-lookup"><span data-stu-id="84cb9-179">This information is supplied to the Restore-AzureRmBackupItem cmdlet to initiate a restore of data from the vault to the customer's account.</span></span>

### <a name="select-the-vm"></a><span data-ttu-id="84cb9-180">Vyberte virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="84cb9-180">Select the VM</span></span>
<span data-ttu-id="84cb9-181">GET pro objekt prostředí PowerShell, který označuje správné záložní položku, musíte spustit z kontejneru v trezoru a směrem dolů hierarchie objektů.</span><span class="sxs-lookup"><span data-stu-id="84cb9-181">To get the PowerShell object that identifies the right backup Item, you need to start from the Container in the vault, and work your way down object hierarchy.</span></span> <span data-ttu-id="84cb9-182">Vyberte kontejner, který představuje virtuální počítač, použijte **Get-AzureRmBackupContainer** rutiny a aby zřetězit **Get-AzureRmBackupItem** rutiny.</span><span class="sxs-lookup"><span data-stu-id="84cb9-182">To select the container that represents the VM, use the **Get-AzureRmBackupContainer** cmdlet and pipe that to the **Get-AzureRmBackupItem** cmdlet.</span></span>

```
PS C:\> $backupitem = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -name "testvm" | Get-AzureRmBackupItem
```

### <a name="choose-a-recovery-point"></a><span data-ttu-id="84cb9-183">Vyberte bod obnovení</span><span class="sxs-lookup"><span data-stu-id="84cb9-183">Choose a recovery point</span></span>
<span data-ttu-id="84cb9-184">Nyní můžete vytvořit seznam všech bodů obnovení pro zálohování položek pomocí **Get-AzureRmBackupRecoveryPoint** rutiny a vyberte bod obnovení pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="84cb9-184">You can now list all the recovery points for the backup item using the **Get-AzureRmBackupRecoveryPoint** cmdlet, and choose the recovery point to restore.</span></span> <span data-ttu-id="84cb9-185">Obvykle vyberte nejnovější uživatelé *AppConsistent* bodu v seznamu.</span><span class="sxs-lookup"><span data-stu-id="84cb9-185">Typically users pick the most recent *AppConsistent* point in the list.</span></span>

```
PS C:\> $rp =  Get-AzureRmBackupRecoveryPoint -Item $backupitem
PS C:\> $rp

RecoveryPointId    RecoveryPointType  RecoveryPointTime      ContainerName
---------------    -----------------  -----------------      -------------
15273496567119     AppConsistent      01-Sep-15 12:27:38 PM  iaasvmcontainer;testvm;testv...
```

<span data-ttu-id="84cb9-186">Proměnná ```$rp``` je pole bodů obnovení pro vybranou zálohu položky, seřazeny v opačném pořadí času – nejnovější bod obnovení je na pozici 0.</span><span class="sxs-lookup"><span data-stu-id="84cb9-186">The variable ```$rp``` is an array of recovery points for the selected backup item, sorted in reverse order of time - the latest recovery point is at index 0.</span></span> <span data-ttu-id="84cb9-187">Použijte standardní prostředí PowerShell pole indexování a vyberte bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="84cb9-187">Use standard PowerShell array indexing to pick the recovery point.</span></span> <span data-ttu-id="84cb9-188">Příklad: ```$rp[0]``` vybere nejnovější bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="84cb9-188">For example: ```$rp[0]``` will select the latest recovery point.</span></span>

### <a name="restoring-disks"></a><span data-ttu-id="84cb9-189">Obnovování disků</span><span class="sxs-lookup"><span data-stu-id="84cb9-189">Restoring disks</span></span>
<span data-ttu-id="84cb9-190">Je klíčové rozdíl mezi operace obnovení provádí prostřednictvím portálu Azure a pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84cb9-190">There is a key difference between the restore operations done through the Azure portal and through Azure PowerShell.</span></span> <span data-ttu-id="84cb9-191">Pomocí prostředí PowerShell operace obnovení zastaví obnovování disky a konfigurační informace z bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="84cb9-191">With PowerShell, the restore operation stops at restoring the disks and config information from the recovery point.</span></span> <span data-ttu-id="84cb9-192">Virtuální počítač se nevytvoří.</span><span class="sxs-lookup"><span data-stu-id="84cb9-192">It does not create a virtual machine.</span></span>

> [!WARNING]
> <span data-ttu-id="84cb9-193">Obnovení-AzureRmBackupItem nedojde k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="84cb9-193">The Restore-AzureRmBackupItem does not create a VM.</span></span> <span data-ttu-id="84cb9-194">Obnoví pouze disky pro zadaný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="84cb9-194">It only restores the disks to the specified storage account.</span></span> <span data-ttu-id="84cb9-195">Nejedná se o stejné chování, které si všimnete na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="84cb9-195">This is not the same behavior you will experience in the Azure portal.</span></span>
>
>

```
PS C:\> $restorejob = Restore-AzureRmBackupItem -StorageAccountName "DestAccount" -RecoveryPoint $rp[0]
PS C:\> $restorejob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Restore         InProgress      01-Sep-15 1:14:01 PM   01-Jan-01 12:00:00 AM
```

<span data-ttu-id="84cb9-196">Můžete získat podrobnosti o použití operaci obnovení **Get-AzureRmBackupJobDetails** rutiny po dokončení úlohy obnovení.</span><span class="sxs-lookup"><span data-stu-id="84cb9-196">You can get the details of the restore operation using the **Get-AzureRmBackupJobDetails** cmdlet once the Restore job has completed.</span></span> <span data-ttu-id="84cb9-197">*Detaily chyby* vlastnost bude mít informace potřebné pro virtuální počítač znovu sestavit.</span><span class="sxs-lookup"><span data-stu-id="84cb9-197">The *ErrorDetails* property will have the information needed to rebuild the VM.</span></span>

```
PS C:\> $restorejob = Get-AzureRmBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmBackupJobDetails -Job $restorejob
```

### <a name="build-the-vm"></a><span data-ttu-id="84cb9-198">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="84cb9-198">Build the VM</span></span>
<span data-ttu-id="84cb9-199">Vytváření virtuálních počítačů mimo obnovené disky lze provést pomocí starší rutin Azure Service Management PowerShell nové šablony Azure Resource Manager, nebo i pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="84cb9-199">Building the VM out of the restored disks can be done using the older Azure Service Management PowerShell cmdlets, the new Azure Resource Manager templates, or even using the Azure portal.</span></span> <span data-ttu-id="84cb9-200">V rychlé příkladu ukážeme, jak získat pomocí rutiny Azure Service Management došlo.</span><span class="sxs-lookup"><span data-stu-id="84cb9-200">In a quick example, we will show how to get there using the Azure Service Management cmdlets.</span></span>

```
$properties  = $details.Properties

$storageAccountName = $properties["Target Storage Account Name"]
$containerName = $properties["Config Blob Container Name"]
$blobName = $properties["Config Blob Name"]

$keys = Get-AzureStorageKey -StorageAccountName $storageAccountName
$storageAccountKey = $keys.Primary
$storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey


$destination_path = "C:\Users\admin\Desktop\vmconfig.xml"
Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path -Context $storageContext


$obj = [xml](((Get-Content -Path $destination_path -Encoding UniCode)).TrimEnd([char]0x00))
$pvr = $obj.PersistentVMRole
$os = $pvr.OSVirtualHardDisk
$dds = $pvr.DataVirtualHardDisks
$osDisk = Add-AzureDisk -MediaLocation $os.MediaLink -OS $os.OS -DiskName "panbhaosdisk"
$vm = New-AzureVMConfig -Name $pvr.RoleName -InstanceSize $pvr.RoleSize -DiskName $osDisk.DiskName

if (!($dds -eq $null))
{
    foreach($d in $dds.DataVirtualHardDisk)
    {
        $lun = 0
        if(!($d.Lun -eq $null))
        {
            $lun = $d.Lun
        }
        $name = "panbhadataDisk" + $lun
        Add-AzureDisk -DiskName $name -MediaLocation $d.MediaLink
        $vm | Add-AzureDataDisk -Import -DiskName $name -LUN $lun
    }
}

New-AzureVM -ServiceName "panbhasample" -Location "SouthEast Asia" -VM $vm
```

<span data-ttu-id="84cb9-201">Další informace o tom, jak vytvořit virtuální počítač z obnovené disků přečtěte si o následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="84cb9-201">For more information on how to build a VM from the restored disks, read about the following cmdlets:</span></span>

* [<span data-ttu-id="84cb9-202">Přidat AzureDisk</span><span class="sxs-lookup"><span data-stu-id="84cb9-202">Add-AzureDisk</span></span>](https://msdn.microsoft.com/library/azure/dn495252.aspx)
* [<span data-ttu-id="84cb9-203">Nové AzureVMConfig</span><span class="sxs-lookup"><span data-stu-id="84cb9-203">New-AzureVMConfig</span></span>](https://msdn.microsoft.com/library/azure/dn495159.aspx)
* [<span data-ttu-id="84cb9-204">Nový AzureVM</span><span class="sxs-lookup"><span data-stu-id="84cb9-204">New-AzureVM</span></span>](https://msdn.microsoft.com/library/azure/dn495254.aspx)

## <a name="code-samples"></a><span data-ttu-id="84cb9-205">Ukázky kódů</span><span class="sxs-lookup"><span data-stu-id="84cb9-205">Code samples</span></span>
### <a name="1-get-the-completion-status-of-job-sub-tasks"></a><span data-ttu-id="84cb9-206">1. Získat stav dokončení dílčí úlohy</span><span class="sxs-lookup"><span data-stu-id="84cb9-206">1. Get the completion status of job sub-tasks</span></span>
<span data-ttu-id="84cb9-207">Chcete-li sledovat stav dokončení jednotlivé dílčí úkoly, můžete použít **Get-AzureRmBackupJobDetails** rutiny:</span><span class="sxs-lookup"><span data-stu-id="84cb9-207">To track the completion status of individual sub-tasks, you can use the **Get-AzureRmBackupJobDetails** cmdlet:</span></span>

```
PS C:\> $details = Get-AzureRmBackupJobDetails -JobId $backupjob.InstanceId -Vault $backupvault
PS C:\> $details.SubTasks

Name                                                        Status
----                                                        ------
Take Snapshot                                               Completed
Transfer data to Backup vault                               InProgress
```

### <a name="2-create-a-dailyweekly-report-of-backup-jobs"></a><span data-ttu-id="84cb9-208">2. Vytvoření sestavy denně nebo týdně úloh zálohování</span><span class="sxs-lookup"><span data-stu-id="84cb9-208">2. Create a daily/weekly report of backup jobs</span></span>
<span data-ttu-id="84cb9-209">Správci obvykle chtít vědět, úlohy zálohování, která byla spuštěna v posledních 24 hodin, stav tyto úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="84cb9-209">Administrators typically want to know what backup jobs ran in the last 24 hours, the status of those backup jobs.</span></span> <span data-ttu-id="84cb9-210">Kromě toho množství dat přenesených dává správcům možnost odhadnout měsíční využití dat.</span><span class="sxs-lookup"><span data-stu-id="84cb9-210">Additionally, the amount of data transferred gives administrators a way to estimate their monthly data usage.</span></span> <span data-ttu-id="84cb9-211">Níže uvedeném skriptu získává nezpracovaná data ze služby Azure Backup a zobrazí informace v konzole PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84cb9-211">The script below pulls the raw data from the Azure Backup service and displays the information in the PowerShell console.</span></span>

```
param(  [Parameter(Mandatory=$True,Position=1)]
        [string]$backupvaultname,

        [Parameter(Mandatory=$False,Position=2)]
        [int]$numberofdays = 7)


#Initialize variables
$DAILYBACKUPSTATS = @()
$backupvault = Get-AzureRmBackupVault -Name $backupvaultname
$enddate = ([datetime]::Today).AddDays(1)
$startdate = ([datetime]::Today)

for( $i = 1; $i -le $numberofdays; $i++ )
{
    # We query one day at a time because pulling 7 days of data might be too much
    $dailyjoblist = Get-AzureRmBackupJob -Vault $backupvault -From $startdate -To $enddate -Type AzureVM -Operation Backup
    Write-Progress -Activity "Getting job information for the last $numberofdays days" -Status "Day -$i" -PercentComplete ([int]([decimal]$i*100/$numberofdays))

    foreach( $job in $dailyjoblist )
    {
        #Extract the information for the reports
        $newstatsobj = New-Object System.Object
        $newstatsobj | Add-Member -Type NoteProperty -Name Date -Value $startdate
        $newstatsobj | Add-Member -Type NoteProperty -Name VMName -Value $job.WorkloadName
        $newstatsobj | Add-Member -Type NoteProperty -Name Duration -Value $job.Duration
        $newstatsobj | Add-Member -Type NoteProperty -Name Status -Value $job.Status

        $details = Get-AzureRmBackupJobDetails -Job $job
        $newstatsobj | Add-Member -Type NoteProperty -Name BackupSize -Value $details.Properties["Backup Size"]
        $DAILYBACKUPSTATS += $newstatsobj
    }

    $enddate = $enddate.AddDays(-1)
    $startdate = $startdate.AddDays(-1)
}

$DAILYBACKUPSTATS | Out-GridView
```

<span data-ttu-id="84cb9-212">Pokud chcete přidat do této sestavy výstup možností vytváření grafů, přečtěte si z v příspěvku blogu TechNet [grafů, pomocí prostředí PowerShell](http://blogs.technet.com/b/richard_macdonald/archive/2009/04/28/3231887.aspx)</span><span class="sxs-lookup"><span data-stu-id="84cb9-212">If you want to add charting capabilities to this report output, learn from the TechNet blog post [Charting with PowerShell](http://blogs.technet.com/b/richard_macdonald/archive/2009/04/28/3231887.aspx)</span></span>

## <a name="next-steps"></a><span data-ttu-id="84cb9-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="84cb9-213">Next steps</span></span>
<span data-ttu-id="84cb9-214">Pokud dáváte přednost, abyste upoutali vašich prostředků Azure pomocí prostředí PowerShell, podívejte se na článek prostředí PowerShell pro ochranu v systému Windows Server [nasadit a spravovat zálohy pro Windows Server](backup-client-automation-classic.md).</span><span class="sxs-lookup"><span data-stu-id="84cb9-214">If you prefer using PowerShell to engage with your Azure resources, check out the PowerShell article for protecting Windows Server, [Deploy and Manage Backup for Windows Server](backup-client-automation-classic.md).</span></span> <span data-ttu-id="84cb9-215">Je také článku prostředí PowerShell pro správu zálohování DPM [nasadit a spravovat zálohy pro DPM](backup-dpm-automation-classic.md).</span><span class="sxs-lookup"><span data-stu-id="84cb9-215">There is also a PowerShell article for managing DPM backups, [Deploy and Manage Backup for DPM](backup-dpm-automation-classic.md).</span></span> <span data-ttu-id="84cb9-216">Mají obě z těchto článků verze pro nasazení Resource Manager a také nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="84cb9-216">Both of these articles have a version for Resource Manager deployments as well as Classic deployments.</span></span>
