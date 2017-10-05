---
title: "Nasadit a spravovat zálohy pro správce prostředků virtuálních počítačů nasazených pomocí prostředí PowerShell | Microsoft Docs"
description: "Pomocí prostředí PowerShell můžete nasadit a spravovat zálohy v Azure pro virtuálních počítačů nasazených Resource Manager"
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
ms.openlocfilehash: 861346a50df6641abb9e454644228146e14b4078
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-azurermrecoveryservicesbackup-cmdlets-to-back-up-virtual-machines"></a><span data-ttu-id="a73b4-103">Pomocí rutin AzureRM.RecoveryServices.Backup zálohování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a73b4-103">Use AzureRM.RecoveryServices.Backup cmdlets to back up virtual machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a73b4-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a73b4-104">Resource Manager</span></span>](backup-azure-vms-automation.md)
> * [<span data-ttu-id="a73b4-105">Classic</span><span class="sxs-lookup"><span data-stu-id="a73b4-105">Classic</span></span>](backup-azure-vms-classic-automation.md)
>
>

<span data-ttu-id="a73b4-106">Tento článek ukazuje, jak používat rutiny prostředí Azure PowerShell k zálohování a obnovení z trezoru služeb zotavení Azure virtuální počítač (VM).</span><span class="sxs-lookup"><span data-stu-id="a73b4-106">This article shows you how to use Azure PowerShell cmdlets to back up and recover an Azure virtual machine (VM) from a Recovery Services vault.</span></span> <span data-ttu-id="a73b4-107">Trezor služeb zotavení je prostředek Azure Resource Manager a slouží k ochraně dat a prostředků služby Azure Backup a Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="a73b4-107">A Recovery Services vault is an Azure Resource Manager resource and is used to protect data and assets in both Azure Backup and Azure Site Recovery services.</span></span> <span data-ttu-id="a73b4-108">Trezor služeb zotavení můžete použít k ochraně virtuálních počítačů nasazených Azure Service Manager a virtuální počítače nasazené Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a73b4-108">You can use a Recovery Services vault to protect Azure Service Manager-deployed VMs, and Azure Resource Manager-deployed VMs.</span></span>

> [!NOTE]
> <span data-ttu-id="a73b4-109">Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a73b4-109">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="a73b4-110">Tento článek je pro použití s virtuálními počítači, které jsou vytvořené pomocí modelu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a73b4-110">This article is for use with VMs created using the Resource Manager model.</span></span>
>
>

<span data-ttu-id="a73b4-111">Tento článek vás provede pomocí prostředí PowerShell k ochraně virtuálního počítače a obnovení dat z bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="a73b4-111">This article walks you through using PowerShell to protect a VM, and restore data from a recovery point.</span></span>

## <a name="concepts"></a><span data-ttu-id="a73b4-112">Koncepty</span><span class="sxs-lookup"><span data-stu-id="a73b4-112">Concepts</span></span>
<span data-ttu-id="a73b4-113">Pokud nejste obeznámeni s služby Azure Backup Přehled služby, podívejte se na [co je Azure Backup?](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="a73b4-113">If you are not familiar with the Azure Backup service, for an overview of the service, check out [What is Azure Backup?](backup-introduction-to-azure-backup.md)</span></span> <span data-ttu-id="a73b4-114">Než začnete, ujistěte se, že, zahrnují essentials o součásti potřebné pro práci s Azure Backup a omezení aktuálního řešení zálohování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a73b4-114">Before you start, ensure that you cover the essentials about the prerequisites needed to work with Azure Backup, and the limitations of the current VM backup solution.</span></span>

<span data-ttu-id="a73b4-115">Pomocí prostředí PowerShell efektivně, je potřeba pochopit hierarchii objektů a odkud spustit.</span><span class="sxs-lookup"><span data-stu-id="a73b4-115">To use PowerShell effectively, it is necessary to understand the hierarchy of objects and from where to start.</span></span>

![Hierarchie objektů služby obnovení](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

<span data-ttu-id="a73b4-117">Odkazu na rutiny Powershellu AzureRm.RecoveryServices.Backup najdete v tématu [Azure Backup - rutin služby obnovení](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) v knihovně Azure.</span><span class="sxs-lookup"><span data-stu-id="a73b4-117">To view the AzureRm.RecoveryServices.Backup PowerShell cmdlet reference, see the [Azure Backup - Recovery Services Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) in the Azure library.</span></span>

## <a name="setup-and-registration"></a><span data-ttu-id="a73b4-118">Instalace a registrace</span><span class="sxs-lookup"><span data-stu-id="a73b4-118">Setup and Registration</span></span>
<span data-ttu-id="a73b4-119">Chcete-li začít:</span><span class="sxs-lookup"><span data-stu-id="a73b4-119">To begin:</span></span>

1. <span data-ttu-id="a73b4-120">[Stáhněte si nejnovější verzi prostředí PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (minimální požadovaná verze je: 1.4.0)</span><span class="sxs-lookup"><span data-stu-id="a73b4-120">[Download the latest version of PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (the minimum version required is: 1.4.0)</span></span>
2. <span data-ttu-id="a73b4-121">Rutiny prostředí PowerShell zálohování Azure k dispozici najděte tak, že zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a73b4-121">Find the Azure Backup PowerShell cmdlets available by typing the following command:</span></span>

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


<span data-ttu-id="a73b4-122">Následující úlohy je možné automatizovat pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="a73b4-122">The following tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="a73b4-123">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="a73b4-123">Create a Recovery Services vault</span></span>
* <span data-ttu-id="a73b4-124">Zálohování virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="a73b4-124">Back up Azure VMs</span></span>
* <span data-ttu-id="a73b4-125">Aktivuje úlohu zálohování</span><span class="sxs-lookup"><span data-stu-id="a73b4-125">Trigger a backup job</span></span>
* <span data-ttu-id="a73b4-126">Monitorování úlohy zálohování</span><span class="sxs-lookup"><span data-stu-id="a73b4-126">Monitor a backup job</span></span>
* <span data-ttu-id="a73b4-127">Obnovení virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="a73b4-127">Restore an Azure VM</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="a73b4-128">Vytvoření trezoru služby Recovery Services</span><span class="sxs-lookup"><span data-stu-id="a73b4-128">Create a recovery services vault</span></span>
<span data-ttu-id="a73b4-129">Následující kroky vás provedou vytvoření trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="a73b4-129">The following steps lead you through creating a Recovery Services vault.</span></span> <span data-ttu-id="a73b4-130">Trezor služeb zotavení se liší od úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="a73b4-130">A Recovery Services vault is different than a Backup vault.</span></span>

1. <span data-ttu-id="a73b4-131">Pokud používáte Azure Backup poprvé, musíte použít  **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)**  rutiny k registraci poskytovatele služeb zotavení Azure s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="a73b4-131">If you are using Azure Backup for the first time, you must use the **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)** cmdlet to register the Azure Recovery Service provider with your subscription.</span></span>

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. <span data-ttu-id="a73b4-132">Trezor služeb zotavení je prostředek Resource Manager, proto musíte umístit do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="a73b4-132">The Recovery Services vault is a Resource Manager resource, so you need to place it within a resource group.</span></span> <span data-ttu-id="a73b4-133">Můžete použít existující skupinu prostředků nebo vytvořte skupinu prostředků s  **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)**  rutiny.</span><span class="sxs-lookup"><span data-stu-id="a73b4-133">You can use an existing resource group, or create a resource group with the **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)** cmdlet.</span></span> <span data-ttu-id="a73b4-134">Při vytváření skupiny prostředků, zadejte název a umístění pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a73b4-134">When creating a resource group, specify the name and location for the resource group.</span></span>  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. <span data-ttu-id="a73b4-135">Použití  **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)**  rutiny pro vytvoření trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="a73b4-135">Use the **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)** cmdlet to create the Recovery Services vault.</span></span> <span data-ttu-id="a73b4-136">Ujistěte se, že zadejte stejné umístění pro úložiště, jako byl použit pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a73b4-136">Be sure to specify the same location for the vault as was used for the resource group.</span></span>

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
4. <span data-ttu-id="a73b4-137">Zadejte typ redundance úložiště se použije. můžete použít [místně redundantní úložiště (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) nebo [geograficky redundantní úložiště (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="a73b4-137">Specify the type of storage redundancy to use; you can use [Locally Redundant Storage (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) or [Geo Redundant Storage (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span></span> <span data-ttu-id="a73b4-138">Následující příklad ukazuje, že je možnost - BackupStorageRedundancy pro testvault nastavena na GeoRedundant.</span><span class="sxs-lookup"><span data-stu-id="a73b4-138">The following example shows the -BackupStorageRedundancy option for testvault is set to GeoRedundant.</span></span>

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testvault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > <span data-ttu-id="a73b4-139">Mnoho rutin Azure Backup vyžadují objekt trezoru služeb zotavení jako vstup.</span><span class="sxs-lookup"><span data-stu-id="a73b4-139">Many Azure Backup cmdlets require the Recovery Services vault object as an input.</span></span> <span data-ttu-id="a73b4-140">Z tohoto důvodu je vhodné pro uložení objektu trezoru služeb zotavení zálohování v proměnné.</span><span class="sxs-lookup"><span data-stu-id="a73b4-140">For this reason, it is convenient to store the Backup Recovery Services vault object in a variable.</span></span>
   >
   >

## <a name="view-the-vaults-in-a-subscription"></a><span data-ttu-id="a73b4-141">Zobrazit trezorů v předplatném.</span><span class="sxs-lookup"><span data-stu-id="a73b4-141">View the vaults in a subscription</span></span>
<span data-ttu-id="a73b4-142">Použití  **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)**  Chcete-li zobrazit seznam všech trezorů v aktuálním předplatném.</span><span class="sxs-lookup"><span data-stu-id="a73b4-142">Use **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)** to view the list of all vaults in the current subscription.</span></span> <span data-ttu-id="a73b4-143">Tento příkaz můžete použít, chcete-li zkontrolovat, zda byl vytvořen nový trezor nebo zobrazíte dostupné trezorů v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="a73b4-143">You can use this command to check that a new vault was created, or to see the available vaults in the subscription.</span></span>

<span data-ttu-id="a73b4-144">Spuštění příkazu Get-AzureRmRecoveryServicesVault, chcete-li zobrazit všechny trezorů v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="a73b4-144">Run the command, Get-AzureRmRecoveryServicesVault, to view all vaults in the subscription.</span></span> <span data-ttu-id="a73b4-145">Následující příklad ukazuje informace zobrazené pro každý trezor.</span><span class="sxs-lookup"><span data-stu-id="a73b4-145">The following example shows the information displayed for each vault.</span></span>

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


## <a name="back-up-azure-vms"></a><span data-ttu-id="a73b4-146">Zálohování virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="a73b4-146">Back up Azure VMs</span></span>
<span data-ttu-id="a73b4-147">Ochrana virtuálních počítačů pomocí trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="a73b4-147">Use a Recovery Services vault to protect your virtual machines.</span></span> <span data-ttu-id="a73b4-148">Před použitím ochranu nastavit kontext trezoru (typ dat v úložišti) a ověřte zásady ochrany.</span><span class="sxs-lookup"><span data-stu-id="a73b4-148">Before you apply the protection, set the vault context (the type of data protected in the vault), and verify the protection policy.</span></span> <span data-ttu-id="a73b4-149">Zásady ochrany je plán při spuštění úloh zálohování a jak dlouho mají být uchována každý snímek zálohy.</span><span class="sxs-lookup"><span data-stu-id="a73b4-149">The protection policy is the schedule when the backup jobs run, and how long each backup snapshot is retained.</span></span>

### <a name="set-vault-context"></a><span data-ttu-id="a73b4-150">Kontext sady trezoru</span><span class="sxs-lookup"><span data-stu-id="a73b4-150">Set vault context</span></span>
<span data-ttu-id="a73b4-151">Než povolíte ochranu na virtuálním počítači, použijte  **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  nastavit kontext úložiště.</span><span class="sxs-lookup"><span data-stu-id="a73b4-151">Before enabling protection on a VM, use **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** to set the vault context.</span></span> <span data-ttu-id="a73b4-152">Po nastavení trezoru rámci platí pro všechny následující rutiny.</span><span class="sxs-lookup"><span data-stu-id="a73b4-152">Once the vault context is set, it applies to all subsequent cmdlets.</span></span> <span data-ttu-id="a73b4-153">Následující příklad nastaví kontext trezoru trezoru, *testvault*.</span><span class="sxs-lookup"><span data-stu-id="a73b4-153">The following example sets the vault context for the vault, *testvault*.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a><span data-ttu-id="a73b4-154">Vytvoření zásady ochrany</span><span class="sxs-lookup"><span data-stu-id="a73b4-154">Create a protection policy</span></span>
<span data-ttu-id="a73b4-155">Při vytváření trezoru služeb zotavení dodává s výchozí ochrana a zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="a73b4-155">When you create a Recovery Services vault, it comes with default protection and retention policies.</span></span> <span data-ttu-id="a73b4-156">Výchozí zásady ochrany aktivuje úlohu zálohování každý den v zadanou dobu.</span><span class="sxs-lookup"><span data-stu-id="a73b4-156">The default protection policy triggers a backup job each day at a specified time.</span></span> <span data-ttu-id="a73b4-157">Výchozí zásady uchovávání informací zachová denního bodu obnovení po dobu 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="a73b4-157">The default retention policy retains the daily recovery point for 30 days.</span></span> <span data-ttu-id="a73b4-158">Výchozí zásady můžete rychle zajistit ochranu virtuálního počítače a upravovat zásady později pomocí různých údajů.</span><span class="sxs-lookup"><span data-stu-id="a73b4-158">You can use the default policy to quickly protect your VM and edit the policy later with different details.</span></span>

<span data-ttu-id="a73b4-159">Použití  **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)**  Chcete-li zobrazit zásady ochrany v trezoru.</span><span class="sxs-lookup"><span data-stu-id="a73b4-159">Use **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)** to view the protection policies in the vault.</span></span> <span data-ttu-id="a73b4-160">Tuto rutinu můžete použít, chcete-li získat konkrétní zásadu, nebo pokud chcete zobrazit zásady přidružené typu úlohy.</span><span class="sxs-lookup"><span data-stu-id="a73b4-160">You can use this cmdlet to get a specific policy, or to view the policies associated with a workload type.</span></span> <span data-ttu-id="a73b4-161">Následující příklad získá zásady pro typ pracovního vytížení, AzureVM.</span><span class="sxs-lookup"><span data-stu-id="a73b4-161">The following example gets policies for workload type, AzureVM.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> <span data-ttu-id="a73b4-162">Časové pásmo BackupTime pole v prostředí PowerShell je UTC.</span><span class="sxs-lookup"><span data-stu-id="a73b4-162">The timezone of the BackupTime field in PowerShell is UTC.</span></span> <span data-ttu-id="a73b4-163">Ale když čas zálohování se zobrazí na portálu Azure, čas se upraví na vaše místní časové pásmo.</span><span class="sxs-lookup"><span data-stu-id="a73b4-163">However, when the backup time is shown in the Azure portal, the time is adjusted to your local timezone.</span></span>
>
>

<span data-ttu-id="a73b4-164">Zásady zálohování ochrany je přidružen alespoň jeden zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="a73b4-164">A backup protection policy is associated with at least one retention policy.</span></span> <span data-ttu-id="a73b4-165">Zásady uchovávání informací definuje, jak dlouho bod obnovení je udržováno před odstraněním.</span><span class="sxs-lookup"><span data-stu-id="a73b4-165">Retention policy defines how long a recovery point is kept before it is deleted.</span></span> <span data-ttu-id="a73b4-166">Použití  **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)**  zobrazení výchozí zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="a73b4-166">Use **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)** to view the default retention policy.</span></span>  <span data-ttu-id="a73b4-167">Podobně můžete použít  **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)**  získat výchozí plán zásady.</span><span class="sxs-lookup"><span data-stu-id="a73b4-167">Similarly you can use **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)** to obtain the default schedule policy.</span></span> <span data-ttu-id="a73b4-168"> **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  rutina vytvoří objekt prostředí PowerShell, který obsahuje informace zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="a73b4-168">The **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** cmdlet creates a PowerShell object that holds backup policy information.</span></span> <span data-ttu-id="a73b4-169">Objekty zásad plán a uchovávání dat se používají jako vstupy pro  **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  rutiny.</span><span class="sxs-lookup"><span data-stu-id="a73b4-169">The schedule and retention policy objects are used as inputs to the **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** cmdlet.</span></span> <span data-ttu-id="a73b4-170">Následující příklad ukládá v proměnné plán zásad a zásad uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="a73b4-170">The following example stores the schedule policy and the retention policy in variables.</span></span> <span data-ttu-id="a73b4-171">V příkladu používá k definici parametrů při vytváření zásady ochrany, tyto proměnné *NewPolicy*.</span><span class="sxs-lookup"><span data-stu-id="a73b4-171">The example uses those variables to define the parameters when creating a protection policy, *NewPolicy*.</span></span>

```
PS C:\> $schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```


### <a name="enable-protection"></a><span data-ttu-id="a73b4-172">Povolení ochrany</span><span class="sxs-lookup"><span data-stu-id="a73b4-172">Enable protection</span></span>
<span data-ttu-id="a73b4-173">Po definování zásad zálohování ochrany je stále nutné povolit zásady pro položku.</span><span class="sxs-lookup"><span data-stu-id="a73b4-173">Once you have defined the backup protection policy, you still must enable the policy for an item.</span></span> <span data-ttu-id="a73b4-174">Použití  **[povolit AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)**  k povolení ochrany.</span><span class="sxs-lookup"><span data-stu-id="a73b4-174">Use **[Enable-AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)** to enable protection.</span></span> <span data-ttu-id="a73b4-175">Povolení ochrany vyžaduje dva objekty - položky a zásady.</span><span class="sxs-lookup"><span data-stu-id="a73b4-175">Enabling protection requires two objects - the item and the policy.</span></span> <span data-ttu-id="a73b4-176">Jakmile zásady je už přidružená k trezoru, aktivuje se v době definované v plán zásad zálohování pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="a73b4-176">Once the policy has been associated with the vault, the backup workflow is triggered at the time defined in the policy schedule.</span></span>

<span data-ttu-id="a73b4-177">Následující příklad povolí ochranu pro položku, V2VM, pomocí zásad, NewPolicy.</span><span class="sxs-lookup"><span data-stu-id="a73b4-177">The following example enables protection for the item, V2VM, using the policy, NewPolicy.</span></span> <span data-ttu-id="a73b4-178">K povolení ochrany na nešifrované virtuálních počítačích Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a73b4-178">To enable the protection on non-encrypted Resource Manager VMs</span></span>

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

<span data-ttu-id="a73b4-179">Pokud chcete povolit ochranu na šifrované virtuálních počítačích (zašifrovaný pomocí BEK a KEK), musíte poskytnout oprávnění služby Azure Backup ke čtení klíče a tajné klíče z trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="a73b4-179">To enable the protection on encrypted VMs (encrypted using BEK and KEK), you need to give the Azure Backup service permission to read keys and secrets from key vault.</span></span>

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

<span data-ttu-id="a73b4-180">Chcete-li povolit ochranu na šifrování virtuálních počítačů (šifrují pomocí BEK pouze), je potřeba udělit oprávnění služby Azure Backup pro čtení tajných klíčů z trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="a73b4-180">To enable the protection on encrypted VMs (encrypted using BEK only), you need to give the Azure Backup service permission to read secrets from key vault.</span></span>

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> <span data-ttu-id="a73b4-181">Pokud používáte cloudu Azure Government, použijte pro parametr hodnotu ff281ffe-705c-4f53-9f37-a40e6f2c68f3 **- ServicePrincipalName** v [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) rutiny.</span><span class="sxs-lookup"><span data-stu-id="a73b4-181">If you are using the Azure Government cloud, then use the value ff281ffe-705c-4f53-9f37-a40e6f2c68f3 for the parameter **-ServicePrincipalName** in [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet.</span></span>
>
>

<span data-ttu-id="a73b4-182">Pro klasické virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="a73b4-182">For classic VMs</span></span>

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a><span data-ttu-id="a73b4-183">Upravit zásady ochrany</span><span class="sxs-lookup"><span data-stu-id="a73b4-183">Modify a protection policy</span></span>
<span data-ttu-id="a73b4-184">Chcete-li upravit zásady ochrany, použijte [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) upravit SchedulePolicy nebo RetentionPolicy objekty.</span><span class="sxs-lookup"><span data-stu-id="a73b4-184">To modify the protection policy, use [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) to modify the SchedulePolicy or RetentionPolicy objects.</span></span>

<span data-ttu-id="a73b4-185">Následující příklad změní uchování bodu obnovení do 365 dní.</span><span class="sxs-lookup"><span data-stu-id="a73b4-185">The following example changes the recovery point retention to 365 days.</span></span>

```
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol.DailySchedule.DurationCountInDays = 365
PS C:\> $pol= Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a><span data-ttu-id="a73b4-186">Spustit zálohu</span><span class="sxs-lookup"><span data-stu-id="a73b4-186">Trigger a backup</span></span>
<span data-ttu-id="a73b4-187">Můžete použít  **[zálohování AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**  se spustit úlohu zálohování.</span><span class="sxs-lookup"><span data-stu-id="a73b4-187">You can use **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)** to trigger a backup job.</span></span> <span data-ttu-id="a73b4-188">Pokud je prvotní zálohování, je úplné zálohování.</span><span class="sxs-lookup"><span data-stu-id="a73b4-188">If it is the initial backup, it is a full backup.</span></span> <span data-ttu-id="a73b4-189">Následné zálohy trvat přírůstkové kopie.</span><span class="sxs-lookup"><span data-stu-id="a73b4-189">Subsequent backups take an incremental copy.</span></span> <span data-ttu-id="a73b4-190">Nezapomeňte použít  **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  nastavit kontext trezoru před spuštěním úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="a73b4-190">Be sure to use **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** to set the vault context before triggering the backup job.</span></span> <span data-ttu-id="a73b4-191">V následujícím příkladu se předpokládá, že byl nastaven kontext úložiště.</span><span class="sxs-lookup"><span data-stu-id="a73b4-191">The following example assumes vault context was set.</span></span>

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
PS C:\> $job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup               InProgress            4/23/2016 5:00:30 PM                       cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> <span data-ttu-id="a73b4-192">Časové pásmo čas spuštění a čas ukončení polí v prostředí PowerShell je UTC.</span><span class="sxs-lookup"><span data-stu-id="a73b4-192">The timezone of the StartTime and EndTime fields in PowerShell is UTC.</span></span> <span data-ttu-id="a73b4-193">Pokud čas se zobrazí na portálu Azure, čas upraví na vaše místní časové pásmo.</span><span class="sxs-lookup"><span data-stu-id="a73b4-193">However, when the time is shown in the Azure portal, the time is adjusted to your local timezone.</span></span>
>
>

## <a name="monitoring-a-backup-job"></a><span data-ttu-id="a73b4-194">Monitorování úlohy zálohování</span><span class="sxs-lookup"><span data-stu-id="a73b4-194">Monitoring a backup job</span></span>
<span data-ttu-id="a73b4-195">Dlouhotrvající operace, jako je například úlohy zálohování, můžete monitorovat bez použití portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a73b4-195">You can monitor long-running operations, such as backup jobs, without using the Azure portal.</span></span> <span data-ttu-id="a73b4-196">Stav probíhající úlohy, použijte  **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)**  rutiny.</span><span class="sxs-lookup"><span data-stu-id="a73b4-196">To get the status of an in-progress job, use the **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)** cmdlet.</span></span> <span data-ttu-id="a73b4-197">Tato rutina načte úlohy zálohování pro konkrétní úložiště a tento trezor je uveden v kontextu trezoru.</span><span class="sxs-lookup"><span data-stu-id="a73b4-197">This cmdlet gets the backup jobs for a specific vault, and that vault is specified in the vault context.</span></span> <span data-ttu-id="a73b4-198">Následující příklad získá stav úlohu v průběhu jako pole a ukládá stav v $joblist proměnné.</span><span class="sxs-lookup"><span data-stu-id="a73b4-198">The following example gets the status of an in-progress job as an array, and stores the status in the $joblist variable.</span></span>

```
PS C:\> $joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
PS C:\> $joblist[0]
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

<span data-ttu-id="a73b4-199">Místo dotazování tyto úlohy pro dokončení – což je zbytečné další kód – použít  **[čekání AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  rutiny.</span><span class="sxs-lookup"><span data-stu-id="a73b4-199">Instead of polling these jobs for completion - which is unnecessary additional code - use the **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** cmdlet.</span></span> <span data-ttu-id="a73b4-200">Tato rutina pozastaví spuštění až do dokončení úlohy nebo zadaný časový limit.</span><span class="sxs-lookup"><span data-stu-id="a73b4-200">This cmdlet pauses the execution until either the job completes or the specified timeout value is reached.</span></span>

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a><span data-ttu-id="a73b4-201">Obnovení virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="a73b4-201">Restore an Azure VM</span></span>
<span data-ttu-id="a73b4-202">Je klíčové rozdíl mezi obnovení virtuálního počítače pomocí portálu Azure a obnovení virtuálního počítače pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a73b4-202">There is a key difference between the restoring a VM using the Azure portal and restoring a VM using PowerShell.</span></span> <span data-ttu-id="a73b4-203">V prostředí PowerShell je dokončena operace obnovení, jakmile disky a informace o konfiguraci z bodu obnovení se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="a73b4-203">With PowerShell, the restore operation is complete once the disks and configuration information from the recovery point are created.</span></span>

> [!NOTE]
> <span data-ttu-id="a73b4-204">Operace obnovení nevytvoří virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a73b4-204">The restore operation does not create a virtual machine.</span></span>
>
>

<span data-ttu-id="a73b4-205">K vytvoření virtuálního počítače z disku, najdete v části [vytvoření virtuálního počítače z uložené disků](backup-azure-vms-automation.md#create-a-vm-from-stored-disks).</span><span class="sxs-lookup"><span data-stu-id="a73b4-205">To create a virtual machine from disk, see the section, [Create the VM from stored disks](backup-azure-vms-automation.md#create-a-vm-from-stored-disks).</span></span> <span data-ttu-id="a73b4-206">Toto jsou základní kroky k obnovení virtuálního počítače Azure:</span><span class="sxs-lookup"><span data-stu-id="a73b4-206">The basic steps to restore an Azure VM are:</span></span>

* <span data-ttu-id="a73b4-207">Vyberte virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="a73b4-207">Select the VM</span></span>
* <span data-ttu-id="a73b4-208">Vyberte bod obnovení</span><span class="sxs-lookup"><span data-stu-id="a73b4-208">Choose a recovery point</span></span>
* <span data-ttu-id="a73b4-209">Obnovení disky</span><span class="sxs-lookup"><span data-stu-id="a73b4-209">Restore the disks</span></span>
* <span data-ttu-id="a73b4-210">Vytvoření virtuálního počítače z uložené disků</span><span class="sxs-lookup"><span data-stu-id="a73b4-210">Create the VM from stored disks</span></span>

<span data-ttu-id="a73b4-211">Následující obrázek znázorňuje hierarchie objektů z RecoveryServicesVault dolů BackupRecoveryPoint.</span><span class="sxs-lookup"><span data-stu-id="a73b4-211">The following graphic shows the object hierarchy from the RecoveryServicesVault down to the BackupRecoveryPoint.</span></span>

![Hierarchie objektů služby obnovení zobrazující BackupContainer](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

<span data-ttu-id="a73b4-213">Chcete-li obnovit zálohovaná data, Identifikujte položku zálohovanou a bod obnovení, který obsahuje data v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="a73b4-213">To restore backup data, identify the backed-up item and the recovery point that holds the point-in-time data.</span></span> <span data-ttu-id="a73b4-214">Použití  **[obnovení AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  rutiny k obnovení dat z trezoru do účtu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="a73b4-214">Use the **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** cmdlet to restore data from the vault to the customer's account.</span></span>

### <a name="select-the-vm"></a><span data-ttu-id="a73b4-215">Vyberte virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="a73b4-215">Select the VM</span></span>
<span data-ttu-id="a73b4-216">GET pro objekt prostředí PowerShell, který označuje správné záložní položku, spusťte z kontejneru v trezoru a směrem dolů hierarchie objektů.</span><span class="sxs-lookup"><span data-stu-id="a73b4-216">To get the PowerShell object that identifies the right backup item, start from the container in the vault, and work your way down the object hierarchy.</span></span> <span data-ttu-id="a73b4-217">Pokud chcete vybrat kontejner, který představuje virtuální počítač, použijte  **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)**  rutiny a prostřednictvím kanálu, který chcete  **[ Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**  rutiny.</span><span class="sxs-lookup"><span data-stu-id="a73b4-217">To select the container that represents the VM, use the **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)** cmdlet and pipe that to the **[Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)** cmdlet.</span></span>

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" –Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem –Container $namedContainer  –WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a><span data-ttu-id="a73b4-218">Vyberte bod obnovení</span><span class="sxs-lookup"><span data-stu-id="a73b4-218">Choose a recovery point</span></span>
<span data-ttu-id="a73b4-219">Použití  **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)**  rutiny seznam všech bodů obnovení pro položku zálohování.</span><span class="sxs-lookup"><span data-stu-id="a73b4-219">Use the **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)** cmdlet to list all recovery points for the backup item.</span></span> <span data-ttu-id="a73b4-220">Zvolte bod obnovení pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="a73b4-220">Then choose the recovery point to restore.</span></span> <span data-ttu-id="a73b4-221">Pokud si nejste jisti, který bod obnovení používat, je dobrým zvykem zvolte nejnovější RecoveryPointType = AppConsistent bodu v seznamu.</span><span class="sxs-lookup"><span data-stu-id="a73b4-221">If you are unsure which recovery point to use, it is a good practice to choose the most recent RecoveryPointType = AppConsistent point in the list.</span></span>

<span data-ttu-id="a73b4-222">V následující skript, proměnné **$rp**, je pole bodů obnovení pro vybranou položku zálohování, v posledních sedmi dnech.</span><span class="sxs-lookup"><span data-stu-id="a73b4-222">In the following script, the variable, **$rp**, is an array of recovery points for the selected backup item, from the past seven days.</span></span> <span data-ttu-id="a73b4-223">Toto pole je seřazen v obráceném pořadí času s indexem 0 nejnovější bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="a73b4-223">The array is sorted in reverse order of time with the latest recovery point at index 0.</span></span> <span data-ttu-id="a73b4-224">Použijte standardní prostředí PowerShell pole indexování a vyberte bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="a73b4-224">Use standard PowerShell array indexing to pick the recovery point.</span></span> <span data-ttu-id="a73b4-225">V příkladu $rp [0] vybere nejnovější bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="a73b4-225">In the example, $rp[0] selects the latest recovery point.</span></span>

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



### <a name="restore-the-disks"></a><span data-ttu-id="a73b4-226">Obnovení disky</span><span class="sxs-lookup"><span data-stu-id="a73b4-226">Restore the disks</span></span>
<span data-ttu-id="a73b4-227">Použití  **[obnovení AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  rutiny k obnovení dat a konfigurace zálohování položek bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="a73b4-227">Use the **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** cmdlet to restore a backup item's data and configuration to a recovery point.</span></span> <span data-ttu-id="a73b4-228">Jakmile jste našli bod obnovení, je využít jako hodnota **- RecoveryPoint** parametr.</span><span class="sxs-lookup"><span data-stu-id="a73b4-228">Once you have identified a recovery point, use it as the value for the **-RecoveryPoint** parameter.</span></span> <span data-ttu-id="a73b4-229">V předchozím ukázkovém kódu **$rp [0]** byl bod obnovení použít.</span><span class="sxs-lookup"><span data-stu-id="a73b4-229">In the previous sample code, **$rp[0]** was the recovery point to use.</span></span> <span data-ttu-id="a73b4-230">V následujícím vzorovém kódu **$rp [0]** je bod obnovení pro obnovení disku.</span><span class="sxs-lookup"><span data-stu-id="a73b4-230">In the following sample code, **$rp[0]** is the recovery point to use for restoring the disk.</span></span>

<span data-ttu-id="a73b4-231">Chcete-li obnovit disky a informace o konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="a73b4-231">To restore the disks and configuration information:</span></span>

```
PS C:\> $restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
PS C:\> $restorejob
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

<span data-ttu-id="a73b4-232">Použití  **[čekání AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  rutiny čekání na dokončení úlohy obnovení.</span><span class="sxs-lookup"><span data-stu-id="a73b4-232">Use the **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** cmdlet to wait for the Restore job to complete.</span></span>

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

<span data-ttu-id="a73b4-233">Po dokončení úlohy obnovení, použijte  **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)**  rutiny Získejte podrobnosti o operaci obnovení.</span><span class="sxs-lookup"><span data-stu-id="a73b4-233">Once the Restore job has completed, use the **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)** cmdlet to get the details of the restore operation.</span></span> <span data-ttu-id="a73b4-234">Vlastnost JobDetails obsahuje informace potřebné pro virtuální počítač znovu sestavit.</span><span class="sxs-lookup"><span data-stu-id="a73b4-234">The JobDetails property has the information needed to rebuild the VM.</span></span>

```
PS C:\> $restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

<span data-ttu-id="a73b4-235">Po obnovení disky, přejděte k další části vytvořte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a73b4-235">Once you restore the disks, go to the next section to create the VM.</span></span>

## <a name="create-a-vm-from-restored-disks"></a><span data-ttu-id="a73b4-236">Vytvoření virtuálního počítače z obnovené disků</span><span class="sxs-lookup"><span data-stu-id="a73b4-236">Create a VM from restored disks</span></span>
<span data-ttu-id="a73b4-237">Po obnovení disky, tyto kroky použijte k vytvoření a konfiguraci virtuálního počítače z disku.</span><span class="sxs-lookup"><span data-stu-id="a73b4-237">After you have restored the disks, use these steps to create and configure the virtual machine from disk.</span></span>

> [!NOTE]
> <span data-ttu-id="a73b4-238">Pokud chcete vytvořit šifrovaný virtuální počítače z obnovené disků, musí mít vaše Azure role oprávnění k provedení akce, **Microsoft.KeyVault/vaults/deploy/action**.</span><span class="sxs-lookup"><span data-stu-id="a73b4-238">To create encrypted VMs from restored disks, your Azure role must have permission to perform the action, **Microsoft.KeyVault/vaults/deploy/action**.</span></span> <span data-ttu-id="a73b4-239">Pokud vaše role nemá toto oprávnění, vytvořte vlastní role pomocí této akce.</span><span class="sxs-lookup"><span data-stu-id="a73b4-239">If your role does not have this permission, create a custom role with this action.</span></span> <span data-ttu-id="a73b4-240">Další informace najdete v tématu [vlastní role v Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="a73b4-240">For more information, see [Custom Roles in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span></span>
>
>

1. <span data-ttu-id="a73b4-241">Dotaz na vlastnosti obnovené disku podrobnosti úlohy.</span><span class="sxs-lookup"><span data-stu-id="a73b4-241">Query the restored disk properties for the job details.</span></span>

  ```
  PS C:\> $properties = $details.properties
  PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
  PS C:\> $containerName = $properties["Config Blob Container Name"]
  PS C:\> $blobName = $properties["Config Blob Name"]
  ```

2. <span data-ttu-id="a73b4-242">Nastavit kontext úložiště Azure a obnovte konfiguračního souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="a73b4-242">Set the Azure storage context and restore the JSON configuration file.</span></span>

    ```
    PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
    PS C:\> $destination_path = "C:\vmconfig.json"
    PS C:\> Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path
    PS C:\> $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
    ```

3. <span data-ttu-id="a73b4-243">Použijte konfigurační soubor JSON pro vytvoření konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a73b4-243">Use the JSON configuration file to create the VM configuration.</span></span>

    ```
   PS C:\> $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
    ```

4. <span data-ttu-id="a73b4-244">Připojte disk operačního systému a datové disky.</span><span class="sxs-lookup"><span data-stu-id="a73b4-244">Attach the OS disk and data disks.</span></span> <span data-ttu-id="a73b4-245">V závislosti na konfiguraci virtuálních počítačů klikněte na odkaz důležité zobrazíte příslušné rutiny:</span><span class="sxs-lookup"><span data-stu-id="a73b4-245">Depending on the configuration of your VMs, click on the relevant link to view respective cmdlets:</span></span> 
    - [<span data-ttu-id="a73b4-246">Nespravované, bez šifrování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a73b4-246">Non-managed, non-encrypted VMs</span></span>](#non-managed-non-encrypted-vms)
    - [<span data-ttu-id="a73b4-247">Nespravované, šifrované virtuální počítače (pouze BEK)</span><span class="sxs-lookup"><span data-stu-id="a73b4-247">Non-managed, encrypted VMs (BEK only)</span></span>](#non-managed-encrypted-vms-bek-only)
    - [<span data-ttu-id="a73b4-248">Nespravované, šifrované virtuálních počítačů (BEK a KEK)</span><span class="sxs-lookup"><span data-stu-id="a73b4-248">Non-managed, encrypted VMs (BEK and KEK)</span></span>](#non-managed-encrypted-vms-bek-and-kek)
    - [<span data-ttu-id="a73b4-249">Spravovaných, bez šifrování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a73b4-249">Managed, non-encrypted VMs</span></span>](#managed-non-encrypted-vms)
    - [<span data-ttu-id="a73b4-250">Spravovaných, šifrované virtuálních počítačů (BEK a KEK)</span><span class="sxs-lookup"><span data-stu-id="a73b4-250">Managed, encrypted VMs (BEK and KEK)</span></span>](#managed-encrypted-vms-bek-and-kek)
    
    #### <a name="non-managed-non-encrypted-vms"></a><span data-ttu-id="a73b4-251">Nespravované, bez šifrování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a73b4-251">Non-managed, non-encrypted VMs</span></span>

    <span data-ttu-id="a73b4-252">Použijte následující příklad nespravované, bez šifrování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a73b4-252">Use the following sample for non-managed, non-encrypted VMs.</span></span>

    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
    PS C:\> foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
    {
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="non-managed-encrypted-vms-bek-only"></a><span data-ttu-id="a73b4-253">Nespravované, šifrované virtuální počítače (pouze BEK)</span><span class="sxs-lookup"><span data-stu-id="a73b4-253">Non-managed, encrypted VMs (BEK only)</span></span>

    <span data-ttu-id="a73b4-254">Nespravované, šifrované virtuálních počítačů (šifrují pomocí BEK pouze) musíte k obnovení tajný klíč pro trezor klíčů, než je možné připojit disky.</span><span class="sxs-lookup"><span data-stu-id="a73b4-254">For non-managed, encrypted VMs (encrypted using BEK only), you need to restore the secret to the key vault before you can attach disks.</span></span> <span data-ttu-id="a73b4-255">Další informace najdete v článku [obnovení šifrovaných virtuálního počítače z bodu obnovení Azure Backup](backup-azure-restore-key-secret.md).</span><span class="sxs-lookup"><span data-stu-id="a73b4-255">For more information, please see the article, [Restore an encrypted virtual machine from an Azure Backup recovery point](backup-azure-restore-key-secret.md).</span></span> <span data-ttu-id="a73b4-256">Následující příklad ukazuje, jak připojit operačního systému a datové disky pro virtuální počítače šifrovaná.</span><span class="sxs-lookup"><span data-stu-id="a73b4-256">The following sample shows how to attach OS and data disks for encrypted VMs.</span></span>

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

    #### <a name="non-managed-encrypted-vms-bek-and-kek"></a><span data-ttu-id="a73b4-257">Nespravované, šifrované virtuálních počítačů (BEK a KEK)</span><span class="sxs-lookup"><span data-stu-id="a73b4-257">Non-managed, encrypted VMs (BEK and KEK)</span></span>

    <span data-ttu-id="a73b4-258">Nespravované, šifrované virtuálních počítačů (zašifrovaný pomocí BEK a KEK) budete muset obnovit klíč a tajný klíč do trezoru klíčů, než připojíte disky.</span><span class="sxs-lookup"><span data-stu-id="a73b4-258">For non-managed, encrypted VMs (encrypted using BEK and KEK), you need to restore the key and secret to the key vault before you can attach disks.</span></span> <span data-ttu-id="a73b4-259">Další informace najdete v článku [obnovení šifrovaných virtuálního počítače z bodu obnovení Azure Backup](backup-azure-restore-key-secret.md).</span><span class="sxs-lookup"><span data-stu-id="a73b4-259">For more information, please see the article, [Restore an encrypted virtual machine from an Azure Backup recovery point](backup-azure-restore-key-secret.md).</span></span> <span data-ttu-id="a73b4-260">Následující příklad ukazuje, jak připojit operačního systému a datové disky pro virtuální počítače šifrovaná.</span><span class="sxs-lookup"><span data-stu-id="a73b4-260">The following sample shows how to attach OS and data disks for encrypted VMs.</span></span>

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

    #### <a name="managed-non-encrypted-vms"></a><span data-ttu-id="a73b4-261">Spravovaných, bez šifrování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a73b4-261">Managed, non-encrypted VMs</span></span>

    <span data-ttu-id="a73b4-262">Pro spravovaných bez šifrování virtuálních počítačů budete muset vytvořit spravované disky z úložiště objektů blob a pak připojte disky.</span><span class="sxs-lookup"><span data-stu-id="a73b4-262">For managed non-encrypted VMs, you'll need to create managed disks from blob storage, and then attach the disks.</span></span> <span data-ttu-id="a73b4-263">Podrobné informace najdete v článku [připojit datový disk pro virtuální počítač s Windows pomocí prostředí PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a73b4-263">For in-depth information, see the article, [Attach a data disk to a Windows VM using PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span></span> <span data-ttu-id="a73b4-264">Následující vzorový kód ukazuje, jak připojit datových disků pro virtuální počítače spravované bez šifrování.</span><span class="sxs-lookup"><span data-stu-id="a73b4-264">The following sample code shows how to attach the data disks for managed non-encrypted VMs.</span></span>

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

    #### <a name="managed-encrypted-vms-bek-and-kek"></a><span data-ttu-id="a73b4-265">Spravovaných, šifrované virtuálních počítačů (BEK a KEK)</span><span class="sxs-lookup"><span data-stu-id="a73b4-265">Managed, encrypted VMs (BEK and KEK)</span></span>

    <span data-ttu-id="a73b4-266">Pro spravované šifrované virtuální počítače (zašifrovaný pomocí BEK a KEK) budete muset vytvořit spravované disky z úložiště objektů blob a pak připojte disky.</span><span class="sxs-lookup"><span data-stu-id="a73b4-266">For managed encrypted VMs (encrypted using BEK and KEK), you'll need to create managed disks from blob storage, and then attach the disks.</span></span> <span data-ttu-id="a73b4-267">Podrobné informace najdete v článku [připojit datový disk pro virtuální počítač s Windows pomocí prostředí PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a73b4-267">For in-depth information, see the article, [Attach a data disk to a Windows VM using PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span></span> <span data-ttu-id="a73b4-268">Následující vzorový kód ukazuje, jak připojit datových disků pro virtuální počítače spravované šifrované.</span><span class="sxs-lookup"><span data-stu-id="a73b4-268">The following sample code shows how to attach the data disks for managed encrypted VMs.</span></span>

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

5. <span data-ttu-id="a73b4-269">Nakonfigurujte nastavení sítě.</span><span class="sxs-lookup"><span data-stu-id="a73b4-269">Set the Network settings.</span></span>

    ```
    PS C:\> $nicName="p1234"
    PS C:\> $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    PS C:\> $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    PS C:\> $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    PS C:\> $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. <span data-ttu-id="a73b4-270">Vytvořte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a73b4-270">Create the virtual machine.</span></span>

    ```    
    PS C:\> New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="next-steps"></a><span data-ttu-id="a73b4-271">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a73b4-271">Next steps</span></span>
<span data-ttu-id="a73b4-272">Pokud dáváte přednost zapojit vašich prostředků Azure pomocí prostředí PowerShell, najdete v článku prostředí PowerShell [nasadit a spravovat zálohy pro Windows Server](backup-client-automation.md).</span><span class="sxs-lookup"><span data-stu-id="a73b4-272">If you prefer to use PowerShell to engage with your Azure resources, see the PowerShell article, [Deploy and Manage Backup for Windows Server](backup-client-automation.md).</span></span> <span data-ttu-id="a73b4-273">Pokud budete spravovat zálohy aplikace DPM, najdete v článku [nasadit a spravovat zálohy pro DPM](backup-dpm-automation.md).</span><span class="sxs-lookup"><span data-stu-id="a73b4-273">If you manage DPM backups, see the article, [Deploy and Manage Backup for DPM](backup-dpm-automation.md).</span></span> <span data-ttu-id="a73b4-274">Mají obě z těchto článků verze pro nasazení Resource Manager a nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="a73b4-274">Both of these articles have a version for Resource Manager deployments and Classic deployments.</span></span>  
