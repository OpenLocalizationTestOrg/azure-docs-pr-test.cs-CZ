---
title: "aaaDeploy a spravovat zálohy pro správce prostředků virtuálních počítačů nasazených pomocí prostředí PowerShell | Microsoft Docs"
description: "Pomocí prostředí PowerShell toodeploy a spravovat zálohy v Azure pro virtuálních počítačů nasazených Resource Manager"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 68606e4f-536d-4eac-9f80-8a198ea94d52
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/28/2017
ms.author: markgal;trinadhk
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 486fb3ae1902403fe6bf303df57244b76677ab17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azurermrecoveryservicesbackup-cmdlets-tooback-up-virtual-machines"></a><span data-ttu-id="9618c-103">Použijte rutiny tooback AzureRM.RecoveryServices.Backup až virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="9618c-103">Use AzureRM.RecoveryServices.Backup cmdlets tooback up virtual machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9618c-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9618c-104">Resource Manager</span></span>](backup-azure-vms-automation.md)
> * [<span data-ttu-id="9618c-105">Classic</span><span class="sxs-lookup"><span data-stu-id="9618c-105">Classic</span></span>](backup-azure-vms-classic-automation.md)
>
>

<span data-ttu-id="9618c-106">Tento článek ukazuje, jak trezoru tooback rutin prostředí Azure PowerShell toouse zálohu a obnovte virtuální počítač Azure (VM) ze služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="9618c-106">This article shows you how toouse Azure PowerShell cmdlets tooback up and recover an Azure virtual machine (VM) from a Recovery Services vault.</span></span> <span data-ttu-id="9618c-107">Trezor služeb zotavení je prostředek Azure Resource Manager a je použité tooprotect data a prostředky služby Azure Backup a Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="9618c-107">A Recovery Services vault is an Azure Resource Manager resource and is used tooprotect data and assets in both Azure Backup and Azure Site Recovery services.</span></span> <span data-ttu-id="9618c-108">Můžete použít tooprotect trezoru služeb zotavení Azure Service Manager nasazené virtuální počítače a virtuální počítače nasazené Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9618c-108">You can use a Recovery Services vault tooprotect Azure Service Manager-deployed VMs, and Azure Resource Manager-deployed VMs.</span></span>

> [!NOTE]
> <span data-ttu-id="9618c-109">Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="9618c-109">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="9618c-110">Tento článek je pro použití s virtuálními počítači, které jsou vytvořené pomocí modelu Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-110">This article is for use with VMs created using hello Resource Manager model.</span></span>
>
>

<span data-ttu-id="9618c-111">Tento článek vás provede pomocí prostředí PowerShell tooprotect virtuálních počítačů a obnovení dat z bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="9618c-111">This article walks you through using PowerShell tooprotect a VM, and restore data from a recovery point.</span></span>

## <a name="concepts"></a><span data-ttu-id="9618c-112">Koncepty</span><span class="sxs-lookup"><span data-stu-id="9618c-112">Concepts</span></span>
<span data-ttu-id="9618c-113">Pokud nejste obeznámeni s hello služby Azure Backup Přehled služby hello, podívejte se na [co je Azure Backup?](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="9618c-113">If you are not familiar with hello Azure Backup service, for an overview of hello service, check out [What is Azure Backup?](backup-introduction-to-azure-backup.md)</span></span> <span data-ttu-id="9618c-114">Než začnete, zkontrolujte, zda jste zahrnují hello essentials o hello součásti potřebné toowork s Azure Backup a hello omezení hello aktuální virtuální počítač řešení zálohování.</span><span class="sxs-lookup"><span data-stu-id="9618c-114">Before you start, ensure that you cover hello essentials about hello prerequisites needed toowork with Azure Backup, and hello limitations of hello current VM backup solution.</span></span>

<span data-ttu-id="9618c-115">toouse prostředí PowerShell efektivně, je nutné toounderstand hello hierarchii objektů a odkud toostart.</span><span class="sxs-lookup"><span data-stu-id="9618c-115">toouse PowerShell effectively, it is necessary toounderstand hello hierarchy of objects and from where toostart.</span></span>

![Hierarchie objektů služby obnovení](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

<span data-ttu-id="9618c-117">tooview hello odkazu na rutiny Powershellu AzureRm.RecoveryServices.Backup, najdete v části hello [Azure Backup - rutin služby obnovení](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) v hello knihovna Azure.</span><span class="sxs-lookup"><span data-stu-id="9618c-117">tooview hello AzureRm.RecoveryServices.Backup PowerShell cmdlet reference, see hello [Azure Backup - Recovery Services Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) in hello Azure library.</span></span>

## <a name="setup-and-registration"></a><span data-ttu-id="9618c-118">Instalace a registrace</span><span class="sxs-lookup"><span data-stu-id="9618c-118">Setup and Registration</span></span>
<span data-ttu-id="9618c-119">toobegin:</span><span class="sxs-lookup"><span data-stu-id="9618c-119">toobegin:</span></span>

1. <span data-ttu-id="9618c-120">[Stáhněte si nejnovější verzi prostředí PowerShell hello](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (hello minimální požadovaná verze je: 1.4.0)</span><span class="sxs-lookup"><span data-stu-id="9618c-120">[Download hello latest version of PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (hello minimum version required is: 1.4.0)</span></span>
2. <span data-ttu-id="9618c-121">Najděte hello rutin prostředí PowerShell zálohování Azure, které jsou k dispozici zadáním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9618c-121">Find hello Azure Backup PowerShell cmdlets available by typing hello following command:</span></span>

```
PS C:\> Get-Command *azurermrecoveryservices*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Backup-AzureRmRecoveryServicesBackupItem           1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Disable-AzureRmRecoveryServicesBackupProtection    1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Enable-AzureRmRecoveryServicesBackupProtection     1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupContainer         1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupItem              1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupJob               1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupJobDetails        1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupManagementServer  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupProperties        1.4.0      AzureRM.RecoveryServices
Cmdlet          Get-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRMRecoveryServicesBackupRecoveryPoint     1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupRetentionPolic... 1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupSchedulePolicy... 1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesVault                   1.4.0      AzureRM.RecoveryServices
Cmdlet          Get-AzureRmRecoveryServicesVaultSettingsFile       1.4.0      AzureRM.RecoveryServices
Cmdlet          New-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          New-AzureRmRecoveryServicesVault                   1.4.0      AzureRM.RecoveryServices
Cmdlet          Remove-AzureRmRecoveryServicesProtectionPolicy     1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Remove-AzureRmRecoveryServicesVault                1.4.0      AzureRM.RecoveryServices
Cmdlet          Restore-AzureRMRecoveryServicesBackupItem          1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Set-AzureRmRecoveryServicesBackupProperties        1.4.0      AzureRM.RecoveryServices
Cmdlet          Set-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Set-AzureRmRecoveryServicesVaultContext            1.4.0      AzureRM.RecoveryServices
Cmdlet          Stop-AzureRmRecoveryServicesBackupJob              1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Unregister-AzureRmRecoveryServicesBackupContainer  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Unregister-AzureRmRecoveryServicesBackupManagem... 1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Wait-AzureRmRecoveryServicesBackupJob              1.4.0      AzureRM.RecoveryServices.Backup
```


<span data-ttu-id="9618c-122">pomocí prostředí PowerShell je možné automatizovat Hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="9618c-122">hello following tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="9618c-123">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="9618c-123">Create a Recovery Services vault</span></span>
* <span data-ttu-id="9618c-124">Zálohování virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="9618c-124">Back up Azure VMs</span></span>
* <span data-ttu-id="9618c-125">Aktivuje úlohu zálohování</span><span class="sxs-lookup"><span data-stu-id="9618c-125">Trigger a backup job</span></span>
* <span data-ttu-id="9618c-126">Monitorování úlohy zálohování</span><span class="sxs-lookup"><span data-stu-id="9618c-126">Monitor a backup job</span></span>
* <span data-ttu-id="9618c-127">Obnovení virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="9618c-127">Restore an Azure VM</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="9618c-128">Vytvoření trezoru služby Recovery Services</span><span class="sxs-lookup"><span data-stu-id="9618c-128">Create a recovery services vault</span></span>
<span data-ttu-id="9618c-129">Hello následující kroky vás provedou vytvoření trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="9618c-129">hello following steps lead you through creating a Recovery Services vault.</span></span> <span data-ttu-id="9618c-130">Trezor služeb zotavení se liší od úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="9618c-130">A Recovery Services vault is different than a Backup vault.</span></span>

1. <span data-ttu-id="9618c-131">Pokud používáte Azure Backup pro hello poprvé, musíte použít hello  **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)**  rutiny tooregister hello službu Azure Recovery zprostředkovatele s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="9618c-131">If you are using Azure Backup for hello first time, you must use hello **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)** cmdlet tooregister hello Azure Recovery Service provider with your subscription.</span></span>

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. <span data-ttu-id="9618c-132">Hello trezor služeb zotavení je prostředku Resource Manager, takže je třeba tooplace ji ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="9618c-132">hello Recovery Services vault is a Resource Manager resource, so you need tooplace it within a resource group.</span></span> <span data-ttu-id="9618c-133">Můžete použít existující skupinu prostředků nebo vytvořte skupinu prostředků s hello  **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)**  rutiny.</span><span class="sxs-lookup"><span data-stu-id="9618c-133">You can use an existing resource group, or create a resource group with hello **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)** cmdlet.</span></span> <span data-ttu-id="9618c-134">Při vytváření skupiny prostředků, zadejte hello název a umístění pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-134">When creating a resource group, specify hello name and location for hello resource group.</span></span>  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. <span data-ttu-id="9618c-135">Použití hello  **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)**  hello toocreate rutiny trezor služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="9618c-135">Use hello **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)** cmdlet toocreate hello Recovery Services vault.</span></span> <span data-ttu-id="9618c-136">Ujistěte se, toospecify hello stejné umístění pro hello trezoru, protože byl použit pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-136">Be sure toospecify hello same location for hello vault as was used for hello resource group.</span></span>

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
4. <span data-ttu-id="9618c-137">Zadejte typ hello toouse redundance úložiště; můžete použít [místně redundantní úložiště (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) nebo [geograficky redundantní úložiště (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="9618c-137">Specify hello type of storage redundancy toouse; you can use [Locally Redundant Storage (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) or [Geo Redundant Storage (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span></span> <span data-ttu-id="9618c-138">Hello následující příklad ukazuje, že je možnost hello - BackupStorageRedundancy pro testvault nastavena tooGeoRedundant.</span><span class="sxs-lookup"><span data-stu-id="9618c-138">hello following example shows hello -BackupStorageRedundancy option for testvault is set tooGeoRedundant.</span></span>

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testvault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > <span data-ttu-id="9618c-139">Mnoho rutin Azure Backup vyžadují objekt trezoru služeb zotavení hello jako vstup.</span><span class="sxs-lookup"><span data-stu-id="9618c-139">Many Azure Backup cmdlets require hello Recovery Services vault object as an input.</span></span> <span data-ttu-id="9618c-140">Z tohoto důvodu je vhodné toostore hello služeb zotavení zálohy trezoru objekt v proměnné.</span><span class="sxs-lookup"><span data-stu-id="9618c-140">For this reason, it is convenient toostore hello Backup Recovery Services vault object in a variable.</span></span>
   >
   >

## <a name="view-hello-vaults-in-a-subscription"></a><span data-ttu-id="9618c-141">Zobrazení hello trezorů v předplatném.</span><span class="sxs-lookup"><span data-stu-id="9618c-141">View hello vaults in a subscription</span></span>
<span data-ttu-id="9618c-142">Použití  **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)**  tooview hello seznam všech trezorů v aktuálním předplatném hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-142">Use **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)** tooview hello list of all vaults in hello current subscription.</span></span> <span data-ttu-id="9618c-143">Můžete použít tento příkaz toocheck, zda byl vytvořen nový trezor, nebo toosee hello k dispozici trezorů v předplatném hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-143">You can use this command toocheck that a new vault was created, or toosee hello available vaults in hello subscription.</span></span>

<span data-ttu-id="9618c-144">Spusťte příkaz hello, Get-AzureRmRecoveryServicesVault tooview všechny trezory v předplatném hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-144">Run hello command, Get-AzureRmRecoveryServicesVault, tooview all vaults in hello subscription.</span></span> <span data-ttu-id="9618c-145">Hello následující příklad ukazuje hello informace zobrazené pro každý trezor.</span><span class="sxs-lookup"><span data-stu-id="9618c-145">hello following example shows hello information displayed for each vault.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesVault
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="back-up-azure-vms"></a><span data-ttu-id="9618c-146">Zálohování virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="9618c-146">Back up Azure VMs</span></span>
<span data-ttu-id="9618c-147">Použijte tooprotect trezoru služeb zotavení virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="9618c-147">Use a Recovery Services vault tooprotect your virtual machines.</span></span> <span data-ttu-id="9618c-148">Před použitím ochrany hello nastavit kontext trezoru hello (hello typ dat chráněných v trezoru hello) a ověřte zásady ochrany hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-148">Before you apply hello protection, set hello vault context (hello type of data protected in hello vault), and verify hello protection policy.</span></span> <span data-ttu-id="9618c-149">zásady ochrany Hello je plán hello při spuštění úlohy zálohování hello a jak dlouho mají být uchována každý snímek zálohy.</span><span class="sxs-lookup"><span data-stu-id="9618c-149">hello protection policy is hello schedule when hello backup jobs run, and how long each backup snapshot is retained.</span></span>

### <a name="set-vault-context"></a><span data-ttu-id="9618c-150">Kontext sady trezoru</span><span class="sxs-lookup"><span data-stu-id="9618c-150">Set vault context</span></span>
<span data-ttu-id="9618c-151">Než povolíte ochranu na virtuálním počítači, použijte  **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  tooset hello trezoru kontextu.</span><span class="sxs-lookup"><span data-stu-id="9618c-151">Before enabling protection on a VM, use **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** tooset hello vault context.</span></span> <span data-ttu-id="9618c-152">Jakmile je nastaven kontext trezoru hello, platí následující rutiny tooall.</span><span class="sxs-lookup"><span data-stu-id="9618c-152">Once hello vault context is set, it applies tooall subsequent cmdlets.</span></span> <span data-ttu-id="9618c-153">Hello následující příklad nastaví hello trezoru kontext pro úložiště hello *testvault*.</span><span class="sxs-lookup"><span data-stu-id="9618c-153">hello following example sets hello vault context for hello vault, *testvault*.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a><span data-ttu-id="9618c-154">Vytvoření zásady ochrany</span><span class="sxs-lookup"><span data-stu-id="9618c-154">Create a protection policy</span></span>
<span data-ttu-id="9618c-155">Při vytváření trezoru služeb zotavení dodává s výchozí ochrana a zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="9618c-155">When you create a Recovery Services vault, it comes with default protection and retention policies.</span></span> <span data-ttu-id="9618c-156">zásady ochrany výchozí Hello aktivuje úlohu zálohování každý den v zadanou dobu.</span><span class="sxs-lookup"><span data-stu-id="9618c-156">hello default protection policy triggers a backup job each day at a specified time.</span></span> <span data-ttu-id="9618c-157">zásady uchovávání informací výchozí Hello zachová hello denního bodu obnovení po dobu 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="9618c-157">hello default retention policy retains hello daily recovery point for 30 days.</span></span> <span data-ttu-id="9618c-158">Můžete použít výchozí hello zásad tooquickly chránit váš počítač a upravovat zásady hello později pomocí různých údajů.</span><span class="sxs-lookup"><span data-stu-id="9618c-158">You can use hello default policy tooquickly protect your VM and edit hello policy later with different details.</span></span>

<span data-ttu-id="9618c-159">Použití  **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)**  zásady ochrany hello tooview v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-159">Use **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)** tooview hello protection policies in hello vault.</span></span> <span data-ttu-id="9618c-160">Tato rutina tooget můžete použít konkrétní zásadu nebo zásady hello tooview přidružený k typu úlohy.</span><span class="sxs-lookup"><span data-stu-id="9618c-160">You can use this cmdlet tooget a specific policy, or tooview hello policies associated with a workload type.</span></span> <span data-ttu-id="9618c-161">Následující ukázka Hello získá zásady pro typ pracovního vytížení, AzureVM.</span><span class="sxs-lookup"><span data-stu-id="9618c-161">hello following example gets policies for workload type, AzureVM.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> <span data-ttu-id="9618c-162">časové pásmo Hello hello BackupTime pole v prostředí PowerShell je UTC.</span><span class="sxs-lookup"><span data-stu-id="9618c-162">hello timezone of hello BackupTime field in PowerShell is UTC.</span></span> <span data-ttu-id="9618c-163">Při čas zálohování hello se zobrazí v hello portálu Azure, je čas hello však upravenou tooyour místním časovém pásmu.</span><span class="sxs-lookup"><span data-stu-id="9618c-163">However, when hello backup time is shown in hello Azure portal, hello time is adjusted tooyour local timezone.</span></span>
>
>

<span data-ttu-id="9618c-164">Zásady zálohování ochrany je přidružen alespoň jeden zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="9618c-164">A backup protection policy is associated with at least one retention policy.</span></span> <span data-ttu-id="9618c-165">Zásady uchovávání informací definuje, jak dlouho bod obnovení je udržováno před odstraněním.</span><span class="sxs-lookup"><span data-stu-id="9618c-165">Retention policy defines how long a recovery point is kept before it is deleted.</span></span> <span data-ttu-id="9618c-166">Použití  **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)**  hello tooview, výchozí zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="9618c-166">Use **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)** tooview hello default retention policy.</span></span>  <span data-ttu-id="9618c-167">Podobně můžete použít  **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)**  tooobtain hello výchozí plán zásad.</span><span class="sxs-lookup"><span data-stu-id="9618c-167">Similarly you can use **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)** tooobtain hello default schedule policy.</span></span> <span data-ttu-id="9618c-168">Hello  **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  rutina vytvoří objekt prostředí PowerShell, který obsahuje informace zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="9618c-168">hello **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** cmdlet creates a PowerShell object that holds backup policy information.</span></span> <span data-ttu-id="9618c-169">Hello plán a uchovávání objektů zásad slouží jako vstup toohello  **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  rutiny.</span><span class="sxs-lookup"><span data-stu-id="9618c-169">hello schedule and retention policy objects are used as inputs toohello **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** cmdlet.</span></span> <span data-ttu-id="9618c-170">Hello následující příklad ukládá hello plán zásad a zásad uchovávání informací hello v proměnné.</span><span class="sxs-lookup"><span data-stu-id="9618c-170">hello following example stores hello schedule policy and hello retention policy in variables.</span></span> <span data-ttu-id="9618c-171">Příklad Hello používá tyto proměnné toodefine hello parametry při vytváření zásady ochrany, *NewPolicy*.</span><span class="sxs-lookup"><span data-stu-id="9618c-171">hello example uses those variables toodefine hello parameters when creating a protection policy, *NewPolicy*.</span></span>

```
PS C:\> $schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```


### <a name="enable-protection"></a><span data-ttu-id="9618c-172">Povolení ochrany</span><span class="sxs-lookup"><span data-stu-id="9618c-172">Enable protection</span></span>
<span data-ttu-id="9618c-173">Po definování zásad zálohování ochrany hello je stále nutné povolit hello zásady pro položku.</span><span class="sxs-lookup"><span data-stu-id="9618c-173">Once you have defined hello backup protection policy, you still must enable hello policy for an item.</span></span> <span data-ttu-id="9618c-174">Použití  **[povolit AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)**  tooenable ochrany.</span><span class="sxs-lookup"><span data-stu-id="9618c-174">Use **[Enable-AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)** tooenable protection.</span></span> <span data-ttu-id="9618c-175">Povolení ochrany vyžaduje dva objekty - hello položky a zásady hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-175">Enabling protection requires two objects - hello item and hello policy.</span></span> <span data-ttu-id="9618c-176">Jakmile zásady hello byla přidružena hello trezoru, aktivaci zálohování pracovního postupu hello během hello definované v plán zásad hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-176">Once hello policy has been associated with hello vault, hello backup workflow is triggered at hello time defined in hello policy schedule.</span></span>

<span data-ttu-id="9618c-177">Následující příklad povolí ochranu u hello položky, V2VM, pomocí zásad hello, NewPolicy Hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-177">hello following example enables protection for hello item, V2VM, using hello policy, NewPolicy.</span></span> <span data-ttu-id="9618c-178">tooenable hello ochrany na nešifrované virtuálních počítačích Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9618c-178">tooenable hello protection on non-encrypted Resource Manager VMs</span></span>

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

<span data-ttu-id="9618c-179">tooenable hello ochrany na šifrovaný virtuálních počítačů (zašifrovaný pomocí BEK a KEK), musíte toogive hello Azure Backup service oprávnění tooread klíče a tajné klíče z trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="9618c-179">tooenable hello protection on encrypted VMs (encrypted using BEK and KEK), you need toogive hello Azure Backup service permission tooread keys and secrets from key vault.</span></span>

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

<span data-ttu-id="9618c-180">tooenable hello ochrany na šifrovaný virtuálních počítačů (šifrují pomocí BEK pouze), musíte toogive hello Azure Backup service oprávnění tooread tajné klíče z trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="9618c-180">tooenable hello protection on encrypted VMs (encrypted using BEK only), you need toogive hello Azure Backup service permission tooread secrets from key vault.</span></span>

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> <span data-ttu-id="9618c-181">Pokud používáte hello cloudu Azure Government, použijte pro parametr hello hello hodnotu ff281ffe-705c-4f53-9f37-a40e6f2c68f3 **- ServicePrincipalName** v [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) rutiny .</span><span class="sxs-lookup"><span data-stu-id="9618c-181">If you are using hello Azure Government cloud, then use hello value ff281ffe-705c-4f53-9f37-a40e6f2c68f3 for hello parameter **-ServicePrincipalName** in [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet.</span></span>
>
>

<span data-ttu-id="9618c-182">Pro klasické virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="9618c-182">For classic VMs</span></span>

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a><span data-ttu-id="9618c-183">Upravit zásady ochrany</span><span class="sxs-lookup"><span data-stu-id="9618c-183">Modify a protection policy</span></span>
<span data-ttu-id="9618c-184">zásady ochrany toomodify hello, použijte [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) toomodify hello SchedulePolicy nebo RetentionPolicy objektů.</span><span class="sxs-lookup"><span data-stu-id="9618c-184">toomodify hello protection policy, use [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) toomodify hello SchedulePolicy or RetentionPolicy objects.</span></span>

<span data-ttu-id="9618c-185">Hello následující příklad změní hello obnovení bodu uchování too365 dnů.</span><span class="sxs-lookup"><span data-stu-id="9618c-185">hello following example changes hello recovery point retention too365 days.</span></span>

```
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol.DailySchedule.DurationCountInDays = 365
PS C:\> $pol= Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a><span data-ttu-id="9618c-186">Spustit zálohu</span><span class="sxs-lookup"><span data-stu-id="9618c-186">Trigger a backup</span></span>
<span data-ttu-id="9618c-187">Můžete použít  **[zálohování AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**  tootrigger úlohu zálohování.</span><span class="sxs-lookup"><span data-stu-id="9618c-187">You can use **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)** tootrigger a backup job.</span></span> <span data-ttu-id="9618c-188">Pokud je hello prvotní zálohování, je úplné zálohování.</span><span class="sxs-lookup"><span data-stu-id="9618c-188">If it is hello initial backup, it is a full backup.</span></span> <span data-ttu-id="9618c-189">Následné zálohy trvat přírůstkové kopie.</span><span class="sxs-lookup"><span data-stu-id="9618c-189">Subsequent backups take an incremental copy.</span></span> <span data-ttu-id="9618c-190">Být jisti toouse  **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  tooset hello trezoru kontextu před spuštěním úlohy zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-190">Be sure toouse **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** tooset hello vault context before triggering hello backup job.</span></span> <span data-ttu-id="9618c-191">Hello následující příklad předpokládá, že byl nastaven kontext úložiště.</span><span class="sxs-lookup"><span data-stu-id="9618c-191">hello following example assumes vault context was set.</span></span>

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
PS C:\> $job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup               InProgress            4/23/2016 5:00:30 PM                       cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> <span data-ttu-id="9618c-192">časové pásmo Hello hello čas spuštění a čas ukončení polí v prostředí PowerShell je UTC.</span><span class="sxs-lookup"><span data-stu-id="9618c-192">hello timezone of hello StartTime and EndTime fields in PowerShell is UTC.</span></span> <span data-ttu-id="9618c-193">Při hello čas se zobrazí v hello portálu Azure, je čas hello však upravenou tooyour místním časovém pásmu.</span><span class="sxs-lookup"><span data-stu-id="9618c-193">However, when hello time is shown in hello Azure portal, hello time is adjusted tooyour local timezone.</span></span>
>
>

## <a name="monitoring-a-backup-job"></a><span data-ttu-id="9618c-194">Monitorování úlohy zálohování</span><span class="sxs-lookup"><span data-stu-id="9618c-194">Monitoring a backup job</span></span>
<span data-ttu-id="9618c-195">Dlouhotrvající operace, jako je například úlohy zálohování, můžete monitorovat bez použití hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9618c-195">You can monitor long-running operations, such as backup jobs, without using hello Azure portal.</span></span> <span data-ttu-id="9618c-196">Stav hello tooget probíhající úlohy, použijte hello  **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)**  rutiny.</span><span class="sxs-lookup"><span data-stu-id="9618c-196">tooget hello status of an in-progress job, use hello **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)** cmdlet.</span></span> <span data-ttu-id="9618c-197">Tato rutina načte hello úlohy zálohování pro konkrétní trezoru a že trezor je uveden v kontextu trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-197">This cmdlet gets hello backup jobs for a specific vault, and that vault is specified in hello vault context.</span></span> <span data-ttu-id="9618c-198">Hello následující příklad získá stav hello úlohu v průběhu jako pole a ukládá stav hello hello $joblist proměnné.</span><span class="sxs-lookup"><span data-stu-id="9618c-198">hello following example gets hello status of an in-progress job as an array, and stores hello status in hello $joblist variable.</span></span>

```
PS C:\> $joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
PS C:\> $joblist[0]
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

<span data-ttu-id="9618c-199">Místo dotazování tyto úlohy pro dokončení – což je zbytečné další kód – použít hello  **[čekání AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  rutiny.</span><span class="sxs-lookup"><span data-stu-id="9618c-199">Instead of polling these jobs for completion - which is unnecessary additional code - use hello **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** cmdlet.</span></span> <span data-ttu-id="9618c-200">Tato rutina pozastaví spuštění hello až do dokončení úlohy hello nebo hello zadaný časový limit.</span><span class="sxs-lookup"><span data-stu-id="9618c-200">This cmdlet pauses hello execution until either hello job completes or hello specified timeout value is reached.</span></span>

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a><span data-ttu-id="9618c-201">Obnovení virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="9618c-201">Restore an Azure VM</span></span>
<span data-ttu-id="9618c-202">Je klíčové rozdíl mezi hello obnovení virtuálního počítače pomocí hello portál Azure a obnovení virtuálního počítače pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9618c-202">There is a key difference between hello restoring a VM using hello Azure portal and restoring a VM using PowerShell.</span></span> <span data-ttu-id="9618c-203">V prostředí PowerShell hello obnovení bylo dokončeno po vytvoření hello disky a informace o konfiguraci z bodu obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-203">With PowerShell, hello restore operation is complete once hello disks and configuration information from hello recovery point are created.</span></span>

> [!NOTE]
> <span data-ttu-id="9618c-204">operace obnovení Hello nevytváří virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9618c-204">hello restore operation does not create a virtual machine.</span></span>
>
>

<span data-ttu-id="9618c-205">toocreate virtuálního počítače z disku, najdete v části hello [hello vytvořit virtuální počítač z uložené disků](backup-azure-vms-automation.md#create-a-vm-from-stored-disks).</span><span class="sxs-lookup"><span data-stu-id="9618c-205">toocreate a virtual machine from disk, see hello section, [Create hello VM from stored disks](backup-azure-vms-automation.md#create-a-vm-from-stored-disks).</span></span> <span data-ttu-id="9618c-206">jsou základní kroky toorestore Hello virtuální počítač Azure:</span><span class="sxs-lookup"><span data-stu-id="9618c-206">hello basic steps toorestore an Azure VM are:</span></span>

* <span data-ttu-id="9618c-207">Vyberte hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="9618c-207">Select hello VM</span></span>
* <span data-ttu-id="9618c-208">Vyberte bod obnovení</span><span class="sxs-lookup"><span data-stu-id="9618c-208">Choose a recovery point</span></span>
* <span data-ttu-id="9618c-209">Obnovení hello disky</span><span class="sxs-lookup"><span data-stu-id="9618c-209">Restore hello disks</span></span>
* <span data-ttu-id="9618c-210">Vytvořit hello virtuálních počítačů z uložené disků</span><span class="sxs-lookup"><span data-stu-id="9618c-210">Create hello VM from stored disks</span></span>

<span data-ttu-id="9618c-211">Hello následující obrázek znázorňuje hello hierarchie objektů z hello RecoveryServicesVault dolů toohello BackupRecoveryPoint.</span><span class="sxs-lookup"><span data-stu-id="9618c-211">hello following graphic shows hello object hierarchy from hello RecoveryServicesVault down toohello BackupRecoveryPoint.</span></span>

![Hierarchie objektů služby obnovení zobrazující BackupContainer](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

<span data-ttu-id="9618c-213">zálohovaná data toorestore, určete hello zálohované položky a hello bod obnovení, který obsahuje data v daném okamžiku hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-213">toorestore backup data, identify hello backed-up item and hello recovery point that holds hello point-in-time data.</span></span> <span data-ttu-id="9618c-214">Použití hello  **[obnovení AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  rutiny toorestore data z hello trezoru účet toohello zákazníka.</span><span class="sxs-lookup"><span data-stu-id="9618c-214">Use hello **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** cmdlet toorestore data from hello vault toohello customer's account.</span></span>

### <a name="select-hello-vm"></a><span data-ttu-id="9618c-215">Vyberte hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="9618c-215">Select hello VM</span></span>
<span data-ttu-id="9618c-216">tooget hello prostředí PowerShell objekt, který identifikuje hello právo zálohování položek, spusťte v hello kontejneru v trezoru hello a směrem dolů hierarchie objektů hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-216">tooget hello PowerShell object that identifies hello right backup item, start from hello container in hello vault, and work your way down hello object hierarchy.</span></span> <span data-ttu-id="9618c-217">tooselect hello kontejneru, který představuje hello virtuální počítač, použijte hello  **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)**  rutiny a zřetězit této toohello  **[ Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**  rutiny.</span><span class="sxs-lookup"><span data-stu-id="9618c-217">tooselect hello container that represents hello VM, use hello **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)** cmdlet and pipe that toohello **[Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)** cmdlet.</span></span>

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" –Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem –Container $namedContainer  –WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a><span data-ttu-id="9618c-218">Vyberte bod obnovení</span><span class="sxs-lookup"><span data-stu-id="9618c-218">Choose a recovery point</span></span>
<span data-ttu-id="9618c-219">Použití hello  **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)**  toolist rutiny všechny body obnovení pro položky zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-219">Use hello **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)** cmdlet toolist all recovery points for hello backup item.</span></span> <span data-ttu-id="9618c-220">Zvolte toorestore bodu obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-220">Then choose hello recovery point toorestore.</span></span> <span data-ttu-id="9618c-221">Pokud si nejste jistí, které toouse bodu obnovení, je vhodné toochoose hello nejnovější RecoveryPointType = AppConsistent bodu v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-221">If you are unsure which recovery point toouse, it is a good practice toochoose hello most recent RecoveryPointType = AppConsistent point in hello list.</span></span>

<span data-ttu-id="9618c-222">V následující skript hello, hello proměnnou, **$rp**, je pole bodů obnovení pro hello vybrané zálohování položek z hello posledních sedmi dnech.</span><span class="sxs-lookup"><span data-stu-id="9618c-222">In hello following script, hello variable, **$rp**, is an array of recovery points for hello selected backup item, from hello past seven days.</span></span> <span data-ttu-id="9618c-223">pole Hello je seřazen v obráceném pořadí času s hello nejnovější bod obnovení na pozici 0.</span><span class="sxs-lookup"><span data-stu-id="9618c-223">hello array is sorted in reverse order of time with hello latest recovery point at index 0.</span></span> <span data-ttu-id="9618c-224">Použijte standardní prostředí PowerShell pole indexování bod obnovení toopick hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-224">Use standard PowerShell array indexing toopick hello recovery point.</span></span> <span data-ttu-id="9618c-225">V příkladu hello vybere $rp [0] hello nejnovější bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="9618c-225">In hello example, $rp[0] selects hello latest recovery point.</span></span>

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
PS C:\> $rp[0]
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```



### <a name="restore-hello-disks"></a><span data-ttu-id="9618c-226">Obnovení hello disky</span><span class="sxs-lookup"><span data-stu-id="9618c-226">Restore hello disks</span></span>
<span data-ttu-id="9618c-227">Použití hello  **[obnovení AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  toorestore rutiny položku Zálohování dat a konfigurace tooa bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="9618c-227">Use hello **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** cmdlet toorestore a backup item's data and configuration tooa recovery point.</span></span> <span data-ttu-id="9618c-228">Jakmile jste našli bod obnovení, použijte ji jako hello hodnotu pro hello **- RecoveryPoint** parametr.</span><span class="sxs-lookup"><span data-stu-id="9618c-228">Once you have identified a recovery point, use it as hello value for hello **-RecoveryPoint** parameter.</span></span> <span data-ttu-id="9618c-229">V ukázkovém kódu předchozí hello **$rp [0]** byla toouse bodu obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-229">In hello previous sample code, **$rp[0]** was hello recovery point toouse.</span></span> <span data-ttu-id="9618c-230">V následující ukázkový kód hello **$rp [0]** je hello obnovení bodu toouse pro obnovení disku hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-230">In hello following sample code, **$rp[0]** is hello recovery point toouse for restoring hello disk.</span></span>

<span data-ttu-id="9618c-231">toorestore hello disky a informace o konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="9618c-231">toorestore hello disks and configuration information:</span></span>

```
PS C:\> $restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
PS C:\> $restorejob
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

<span data-ttu-id="9618c-232">Použití hello  **[čekání AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  toowait rutiny pro toocomplete úlohy obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-232">Use hello **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** cmdlet toowait for hello Restore job toocomplete.</span></span>

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

<span data-ttu-id="9618c-233">Po dokončení úlohy obnovení hello použít hello  **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)**  rutiny tooget hello podrobnosti o hello operaci obnovení.</span><span class="sxs-lookup"><span data-stu-id="9618c-233">Once hello Restore job has completed, use hello **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)** cmdlet tooget hello details of hello restore operation.</span></span> <span data-ttu-id="9618c-234">Hello JobDetails vlastnost má hello informace potřebné toorebuild hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9618c-234">hello JobDetails property has hello information needed toorebuild hello VM.</span></span>

```
PS C:\> $restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

<span data-ttu-id="9618c-235">Po obnovení hello disky, přejděte toohello další části toocreate hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9618c-235">Once you restore hello disks, go toohello next section toocreate hello VM.</span></span>

## <a name="create-a-vm-from-restored-disks"></a><span data-ttu-id="9618c-236">Vytvoření virtuálního počítače z obnovené disků</span><span class="sxs-lookup"><span data-stu-id="9618c-236">Create a VM from restored disks</span></span>
<span data-ttu-id="9618c-237">Po obnovení hello disky, použijte tyto kroky toocreate a nakonfigurujte hello virtuálního počítače z disku.</span><span class="sxs-lookup"><span data-stu-id="9618c-237">After you have restored hello disks, use these steps toocreate and configure hello virtual machine from disk.</span></span>

> [!NOTE]
> <span data-ttu-id="9618c-238">toocreate šifrování virtuálních počítačů z obnovené disků, vaše Azure role musí mít oprávnění tooperform hello akce, **Microsoft.KeyVault/vaults/deploy/action**.</span><span class="sxs-lookup"><span data-stu-id="9618c-238">toocreate encrypted VMs from restored disks, your Azure role must have permission tooperform hello action, **Microsoft.KeyVault/vaults/deploy/action**.</span></span> <span data-ttu-id="9618c-239">Pokud vaše role nemá toto oprávnění, vytvořte vlastní role pomocí této akce.</span><span class="sxs-lookup"><span data-stu-id="9618c-239">If your role does not have this permission, create a custom role with this action.</span></span> <span data-ttu-id="9618c-240">Další informace najdete v tématu [vlastní role v Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="9618c-240">For more information, see [Custom Roles in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span></span>
>
>

1. <span data-ttu-id="9618c-241">Dotaz hello obnovit vlastnosti disku podrobnosti úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-241">Query hello restored disk properties for hello job details.</span></span>

  ```
  PS C:\> $properties = $details.properties
  PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
  PS C:\> $containerName = $properties["Config Blob Container Name"]
  PS C:\> $blobName = $properties["Config Blob Name"]
  ```

2. <span data-ttu-id="9618c-242">Nastavit kontext hello úložiště Azure a obnovte hello JSON konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="9618c-242">Set hello Azure storage context and restore hello JSON configuration file.</span></span>

    ```
    PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
    PS C:\> $destination_path = "C:\vmconfig.json"
    PS C:\> Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path
    PS C:\> $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
    ```

3. <span data-ttu-id="9618c-243">Použijte hello JSON konfigurační soubor toocreate hello konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9618c-243">Use hello JSON configuration file toocreate hello VM configuration.</span></span>

    ```
   PS C:\> $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
    ```

4. <span data-ttu-id="9618c-244">Připojte hello disk operačního systému a datové disky.</span><span class="sxs-lookup"><span data-stu-id="9618c-244">Attach hello OS disk and data disks.</span></span> <span data-ttu-id="9618c-245">V závislosti na konfiguraci hello virtuální počítače klikněte na příslušné rutiny tooview hello relevantní odkaz:</span><span class="sxs-lookup"><span data-stu-id="9618c-245">Depending on hello configuration of your VMs, click on hello relevant link tooview respective cmdlets:</span></span> 
    - [<span data-ttu-id="9618c-246">Nespravované, bez šifrování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="9618c-246">Non-managed, non-encrypted VMs</span></span>](#non-managed-non-encrypted-vms)
    - [<span data-ttu-id="9618c-247">Nespravované, šifrované virtuální počítače (pouze BEK)</span><span class="sxs-lookup"><span data-stu-id="9618c-247">Non-managed, encrypted VMs (BEK only)</span></span>](#non-managed-encrypted-vms-bek-only)
    - [<span data-ttu-id="9618c-248">Nespravované, šifrované virtuálních počítačů (BEK a KEK)</span><span class="sxs-lookup"><span data-stu-id="9618c-248">Non-managed, encrypted VMs (BEK and KEK)</span></span>](#non-managed-encrypted-vms-bek-and-kek)
    - [<span data-ttu-id="9618c-249">Spravovaných, bez šifrování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="9618c-249">Managed, non-encrypted VMs</span></span>](#managed-non-encrypted-vms)
    - [<span data-ttu-id="9618c-250">Spravovaných, šifrované virtuálních počítačů (BEK a KEK)</span><span class="sxs-lookup"><span data-stu-id="9618c-250">Managed, encrypted VMs (BEK and KEK)</span></span>](#managed-encrypted-vms-bek-and-kek)
    
    #### <a name="non-managed-non-encrypted-vms"></a><span data-ttu-id="9618c-251">Nespravované, bez šifrování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="9618c-251">Non-managed, non-encrypted VMs</span></span>

    <span data-ttu-id="9618c-252">Použijte hello následující ukázka nespravované, bez šifrování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9618c-252">Use hello following sample for non-managed, non-encrypted VMs.</span></span>

    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
    PS C:\> foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
    {
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="non-managed-encrypted-vms-bek-only"></a><span data-ttu-id="9618c-253">Nespravované, šifrované virtuální počítače (pouze BEK)</span><span class="sxs-lookup"><span data-stu-id="9618c-253">Non-managed, encrypted VMs (BEK only)</span></span>

    <span data-ttu-id="9618c-254">Pro nespravované, šifrované virtuálních počítačů (šifrují pomocí BEK pouze) budete potřebovat toorestore hello tajný toohello trezoru klíčů, než je možné připojit disky.</span><span class="sxs-lookup"><span data-stu-id="9618c-254">For non-managed, encrypted VMs (encrypted using BEK only), you need toorestore hello secret toohello key vault before you can attach disks.</span></span> <span data-ttu-id="9618c-255">Další informace najdete v tématu hello článku [obnovení šifrovaných virtuálního počítače z bodu obnovení Azure Backup](backup-azure-restore-key-secret.md).</span><span class="sxs-lookup"><span data-stu-id="9618c-255">For more information, please see hello article, [Restore an encrypted virtual machine from an Azure Backup recovery point](backup-azure-restore-key-secret.md).</span></span> <span data-ttu-id="9618c-256">Následující ukázka Hello ukazuje, jak šifrované tooattach operačního systému a datové disky pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="9618c-256">hello following sample shows how tooattach OS and data disks for encrypted VMs.</span></span>

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

    #### <a name="non-managed-encrypted-vms-bek-and-kek"></a><span data-ttu-id="9618c-257">Nespravované, šifrované virtuálních počítačů (BEK a KEK)</span><span class="sxs-lookup"><span data-stu-id="9618c-257">Non-managed, encrypted VMs (BEK and KEK)</span></span>

    <span data-ttu-id="9618c-258">Nespravované, šifrované virtuálních počítačů (zašifrovaný pomocí BEK a KEK) musíte toorestore hello klíče a tajné toohello trezoru klíčů předtím, než je možné připojit disky.</span><span class="sxs-lookup"><span data-stu-id="9618c-258">For non-managed, encrypted VMs (encrypted using BEK and KEK), you need toorestore hello key and secret toohello key vault before you can attach disks.</span></span> <span data-ttu-id="9618c-259">Další informace najdete v tématu hello článku [obnovení šifrovaných virtuálního počítače z bodu obnovení Azure Backup](backup-azure-restore-key-secret.md).</span><span class="sxs-lookup"><span data-stu-id="9618c-259">For more information, please see hello article, [Restore an encrypted virtual machine from an Azure Backup recovery point](backup-azure-restore-key-secret.md).</span></span> <span data-ttu-id="9618c-260">Následující ukázka Hello ukazuje, jak šifrované tooattach operačního systému a datové disky pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="9618c-260">hello following sample shows how tooattach OS and data disks for encrypted VMs.</span></span>

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

    #### <a name="managed-non-encrypted-vms"></a><span data-ttu-id="9618c-261">Spravovaných, bez šifrování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="9618c-261">Managed, non-encrypted VMs</span></span>

    <span data-ttu-id="9618c-262">Pro spravovaných bez šifrování virtuálních počítačů budete potřebovat toocreate spravované disky z úložiště objektů blob a pak připojte hello disky.</span><span class="sxs-lookup"><span data-stu-id="9618c-262">For managed non-encrypted VMs, you'll need toocreate managed disks from blob storage, and then attach hello disks.</span></span> <span data-ttu-id="9618c-263">Podrobné informace najdete v článku hello, [připojit tooa disku data virtuálního počítače s Windows pomocí prostředí PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="9618c-263">For in-depth information, see hello article, [Attach a data disk tooa Windows VM using PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span></span> <span data-ttu-id="9618c-264">Hello následující vzorový kód ukazuje, jak tooattach hello datových disků pro virtuální počítače spravované bez šifrování.</span><span class="sxs-lookup"><span data-stu-id="9618c-264">hello following sample code shows how tooattach hello data disks for managed non-encrypted VMs.</span></span>

    ```
    PS C:\> $storageType = "StandardLRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="managed-encrypted-vms-bek-and-kek"></a><span data-ttu-id="9618c-265">Spravovaných, šifrované virtuálních počítačů (BEK a KEK)</span><span class="sxs-lookup"><span data-stu-id="9618c-265">Managed, encrypted VMs (BEK and KEK)</span></span>

    <span data-ttu-id="9618c-266">Pro spravované šifrované virtuální počítače (zašifrovaný pomocí BEK a KEK) budete potřebovat toocreate spravované disky z úložiště objektů blob a pak připojte hello disky.</span><span class="sxs-lookup"><span data-stu-id="9618c-266">For managed encrypted VMs (encrypted using BEK and KEK), you'll need toocreate managed disks from blob storage, and then attach hello disks.</span></span> <span data-ttu-id="9618c-267">Podrobné informace najdete v článku hello, [připojit tooa disku data virtuálního počítače s Windows pomocí prostředí PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="9618c-267">For in-depth information, see hello article, [Attach a data disk tooa Windows VM using PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span></span> <span data-ttu-id="9618c-268">Hello následující vzorový kód ukazuje, jak tooattach hello datových disků pro spravovaných šifrované virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9618c-268">hello following sample code shows how tooattach hello data disks for managed encrypted VMs.</span></span>

     ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> $storageType = "StandardLRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

5. <span data-ttu-id="9618c-269">Nastavení sítě hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-269">Set hello Network settings.</span></span>

    ```
    PS C:\> $nicName="p1234"
    PS C:\> $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    PS C:\> $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    PS C:\> $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    PS C:\> $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. <span data-ttu-id="9618c-270">Vytvořte virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="9618c-270">Create hello virtual machine.</span></span>

    ```    
    PS C:\> New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="next-steps"></a><span data-ttu-id="9618c-271">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9618c-271">Next steps</span></span>
<span data-ttu-id="9618c-272">Pokud dáváte přednost tooengage toouse prostředí PowerShell s vašich prostředků Azure, najdete v článku prostředí PowerShell hello [nasadit a spravovat zálohy pro Windows Server](backup-client-automation.md).</span><span class="sxs-lookup"><span data-stu-id="9618c-272">If you prefer toouse PowerShell tooengage with your Azure resources, see hello PowerShell article, [Deploy and Manage Backup for Windows Server](backup-client-automation.md).</span></span> <span data-ttu-id="9618c-273">Pokud budete spravovat zálohy aplikace DPM, najdete v článku hello, [nasadit a spravovat zálohy pro DPM](backup-dpm-automation.md).</span><span class="sxs-lookup"><span data-stu-id="9618c-273">If you manage DPM backups, see hello article, [Deploy and Manage Backup for DPM](backup-dpm-automation.md).</span></span> <span data-ttu-id="9618c-274">Mají obě z těchto článků verze pro nasazení Resource Manager a nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="9618c-274">Both of these articles have a version for Resource Manager deployments and Classic deployments.</span></span>  
