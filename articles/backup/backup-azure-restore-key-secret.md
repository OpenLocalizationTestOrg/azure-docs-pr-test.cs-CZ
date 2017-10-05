---
title: "Obnovení klíče trezoru klíč a tajný klíč pro šifrované virtuální počítače pomocí služby Azure Backup | Microsoft Docs"
description: "Zjistěte, jak obnovit klíč trezoru klíč a tajný klíč v Azure Backup pomocí prostředí PowerShell"
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 45214083-d5fc-4eb3-a367-0239dc59e0f6
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f2db3449187d655248b13198b268841052570626
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a><span data-ttu-id="3e4e8-103">Obnovení klíče trezoru klíč a tajný klíč pro šifrované virtuální počítače pomocí služby Azure Backup</span><span class="sxs-lookup"><span data-stu-id="3e4e8-103">Restore Key Vault key and secret for encrypted VMs using Azure Backup</span></span>
<span data-ttu-id="3e4e8-104">V tomto článku bude zmíněn pomocí zálohování virtuálních počítačů Azure k provedení obnovení šifrovaných virtuálních počítačích Azure, pokud klíč a tajný klíč neexistují v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-104">This article talks about using Azure VM Backup to perform restore of encrypted Azure VMs, if your key and secret do not exist in the key vault.</span></span> <span data-ttu-id="3e4e8-105">Tyto kroky můžete také použijí, pokud chcete zachovat samostatnou kopii klíč (klíč šifrovacího klíče) a tajný klíč (klíč šifrování nástrojem BitLocker) pro obnovený virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-105">These steps can also be used if you want to maintain a separate copy of key (Key Encryption Key) and secret (BitLocker Encryption Key) for the restored VM.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3e4e8-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3e4e8-106">Prerequisites</span></span>
* <span data-ttu-id="3e4e8-107">**Šifrované zálohování virtuálních počítačů** – šifrované virtuální počítače Azure byly zálohovány pomocí služby Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-107">**Backup encrypted VMs** - Encrypted Azure VMs have been backed up using Azure Backup.</span></span> <span data-ttu-id="3e4e8-108">Najdete v článku [Správa zálohování a obnovení virtuálních počítačů Azure pomocí prostředí PowerShell](backup-azure-vms-automation.md) podrobnosti o tom, jak zálohovat šifrované virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-108">Refer the article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md) for details about how to backup encrypted Azure VMs.</span></span>
* <span data-ttu-id="3e4e8-109">**Azure Key Vault konfigurovat** – Ujistěte se, že trezor klíčů, ke kterému klíče a tajné klíče je nutné obnovit již existuje.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-109">**Configure Azure Key Vault** – Ensure that key vault to which keys and secrets need to be restored is already present.</span></span> <span data-ttu-id="3e4e8-110">Najdete v článku [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md) podrobnosti o správě trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-110">Refer the article [Get Started with Azure Key Vault](../key-vault/key-vault-get-started.md) for details about key vault management.</span></span>
* <span data-ttu-id="3e4e8-111">**Obnovení disku** – Ujistěte se, že máte aktivuje úlohy obnovení pro obnovení disků pro šifrované virtuální počítač pomocí [kroky PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm).</span><span class="sxs-lookup"><span data-stu-id="3e4e8-111">**Restore disk** - Ensure that you have triggered restore job for restoring disks for encrypted VM using [PowerShell steps](backup-azure-vms-automation.md#restore-an-azure-vm).</span></span> <span data-ttu-id="3e4e8-112">Je to proto, že tato úloha generuje soubor JSON v účtu úložiště, který obsahuje klíče a tajné klíče pro šifrovaná virtuální počítač pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-112">This is because this job generates a JSON file in your storage account containing keys and secrets for the encrypted VM to be restored.</span></span>

## <a name="get-key-and-secret-from-azure-backup"></a><span data-ttu-id="3e4e8-113">Získat klíč a tajný klíč z Azure Backup</span><span class="sxs-lookup"><span data-stu-id="3e4e8-113">Get key and secret from Azure Backup</span></span>

> [!NOTE]
> <span data-ttu-id="3e4e8-114">Jakmile disku byla obnovena pro šifrované virtuálních počítačů, zajistěte, aby:</span><span class="sxs-lookup"><span data-stu-id="3e4e8-114">Once disk has been restored for the encrypted VM, ensure that:</span></span>
> 1. <span data-ttu-id="3e4e8-115">$details naplněný podrobnosti úlohy obnovení disku, jak je uvedeno v [prostředí PowerShell kroky obnovení části disky](backup-azure-vms-automation.md#restore-an-azure-vm)</span><span class="sxs-lookup"><span data-stu-id="3e4e8-115">$details is populated with restore disk job details, as mentioned in [PowerShell steps in Restore the Disks section](backup-azure-vms-automation.md#restore-an-azure-vm)</span></span>
> 2. <span data-ttu-id="3e4e8-116">Virtuální počítač by měl být vytvořen z obnovené disků pouze **po obnovení klíče a tajné klíče trezoru**.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-116">VM should be created from restored disks only **after key and secret is restored to key vault**.</span></span>
>
>

<span data-ttu-id="3e4e8-117">Dotaz na vlastnosti obnovené disku podrobnosti úlohy.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-117">Query the restored disk properties for the job details.</span></span>

```
PS C:\> $properties = $details.properties
PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
PS C:\> $containerName = $properties["Config Blob Container Name"]
PS C:\> $encryptedBlobName = $properties["Encryption Info Blob Name"]
```

<span data-ttu-id="3e4e8-118">Nastavit kontext úložiště Azure a obnovte konfigurační soubor JSON obsahující klíče a tajné údaje pro šifrované virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-118">Set the Azure storage context and restore JSON configuration file containing key and secret details for encrypted VM.</span></span>

```
PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
PS C:\> $destination_path = 'C:\vmencryption_config.json'
PS C:\> Get-AzureStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
PS C:\> $encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a><span data-ttu-id="3e4e8-119">Obnovení klíče</span><span class="sxs-lookup"><span data-stu-id="3e4e8-119">Restore key</span></span>
<span data-ttu-id="3e4e8-120">Po vygenerování souboru JSON v cílové cestě zmíněné generování klíče objektu blob souboru z formátu JSON a kanálu ho k obnovení klíče rutiny uvést klíč (KEK) zpět do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-120">Once the JSON file is generated in the destination path mentioned above, generate key blob file from the JSON and feed it to restore key cmdlet to put the key (KEK) back in the key vault.</span></span>

```
PS C:\> $keyDestination = 'C:\keyDetails.blob'
PS C:\> [io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a><span data-ttu-id="3e4e8-121">Obnovit tajný klíč</span><span class="sxs-lookup"><span data-stu-id="3e4e8-121">Restore secret</span></span>
<span data-ttu-id="3e4e8-122">K nastavení tajný rutiny uvést tajný klíč (BEK) zpět do trezoru klíčů použijte soubor JSON generované výše získat tajný název a hodnotu a jeho kanálu.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-122">Use the JSON file generated above to get secret name and value and feed it to set secret cmdlet to put the secret (BEK) back in the key vault.</span></span> <span data-ttu-id="3e4e8-123">**Tyto rutiny použijte, pokud virtuální počítač je zašifrovaný pomocí BEK a KEK.**</span><span class="sxs-lookup"><span data-stu-id="3e4e8-123">**Use these cmdlets if your VM is encrypted using BEK and KEK.**</span></span>

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

<span data-ttu-id="3e4e8-124">Pokud je virtuální počítač **šifrovat jenom pomocí BEK**, vygenerujte soubor tajný blob z JSON a kanál ho obnovit tajný rutiny uvést tajný klíč (BEK) zpět do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-124">If your VM is **encrypted using BEK only**, generate secret blob file from the JSON and feed it to restore secret cmdlet to put the secret (BEK) back in the key vault.</span></span>

```
PS C:\> $secretDestination = 'C:\secret.blob'
PS C:\> [io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
PS C:\> Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
```

> [!NOTE]
> 1. <span data-ttu-id="3e4e8-125">Nelze získat hodnotu $secretname odkazy na výstup $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl a použitím text po tajných klíčů / například výstup tajný adresa URL https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163, tajný název B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span><span class="sxs-lookup"><span data-stu-id="3e4e8-125">Value for $secretname can be obtained by referring to the output of $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl and using text after secrets/ e.g. output secret URL is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 and secret name is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span></span>
> 2. <span data-ttu-id="3e4e8-126">Hodnota značky DiskEncryptionKeyFileName je stejný jako tajný název.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-126">Value of the tag DiskEncryptionKeyFileName is same as secret name.</span></span>
>
>

## <a name="create-virtual-machine-from-restored-disk"></a><span data-ttu-id="3e4e8-127">Vytvoření virtuálního počítače z obnovené disku</span><span class="sxs-lookup"><span data-stu-id="3e4e8-127">Create virtual machine from restored disk</span></span>
<span data-ttu-id="3e4e8-128">Pokud jste zálohovali šifrované virtuálního počítače pomocí zálohování virtuálních počítačů Azure, rutin prostředí PowerShell nápovědy zmíněné, obnovit klíče a tajné zpět do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-128">If you have backed up encrypted VM using Azure VM Backup, the PowerShell cmdlets mentioned above help you restore key and secret back to the key vault.</span></span> <span data-ttu-id="3e4e8-129">Po obnovení je, najdete v článku [Správa zálohování a obnovení virtuálních počítačů Azure pomocí prostředí PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) vytvořit šifrovaný virtuální počítače z obnovené disku, klíče a tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-129">After restoring them, refer the article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) to create encrypted VMs from restored disk, key, and secret.</span></span>

## <a name="legacy-approach"></a><span data-ttu-id="3e4e8-130">Starší verze přístup</span><span class="sxs-lookup"><span data-stu-id="3e4e8-130">Legacy approach</span></span>
<span data-ttu-id="3e4e8-131">Přístup zmíněné by fungovat pro všechny body obnovení.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-131">The approach mentioned above would work for all the recovery points.</span></span> <span data-ttu-id="3e4e8-132">Starší způsob získání klíče a tajné informace z bodu obnovení, by však být platný pro body obnovení, které jsou starší než 11 července 2017 zašifrovaný pomocí BEK a KEK virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-132">However, the older approach of getting key and secret information from recovery point, would be valid for recovery points older than July 11, 2017 for VMs encrypted using BEK and KEK.</span></span> <span data-ttu-id="3e4e8-133">Po dokončení pro šifrované virtuální počítač pomocí úlohy obnovení disku [kroky PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm), ujistěte se, že je $rp naplněný platnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-133">Once restore disk job is complete for encrypted VM using [PowerShell steps](backup-azure-vms-automation.md#restore-an-azure-vm), ensure that $rp is populated with a valid value.</span></span>

### <a name="restore-key"></a><span data-ttu-id="3e4e8-134">Obnovení klíče</span><span class="sxs-lookup"><span data-stu-id="3e4e8-134">Restore key</span></span>
<span data-ttu-id="3e4e8-135">Následující rutiny můžete použijte k získání informací o klíči (KEK) z bodu obnovení a kanál ho k obnovení klíče rutiny vrátit zpět v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-135">Use the following cmdlets to get key (KEK) information from recovery point and feed it to restore key cmdlet to put it back in the key vault.</span></span>

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a><span data-ttu-id="3e4e8-136">Obnovit tajný klíč</span><span class="sxs-lookup"><span data-stu-id="3e4e8-136">Restore secret</span></span>
<span data-ttu-id="3e4e8-137">Následující rutiny můžete použijte k získání tajné informace (BEK) z bodu obnovení a kanál ho nastavit tajný rutiny vrátit zpět v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-137">Use the following cmdlets to get secret (BEK) information from recovery point and feed it to set secret cmdlet to put it back in the key vault.</span></span>

```
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> 1. <span data-ttu-id="3e4e8-138">Hodnota pro $secretname je možné získat odkaz na výstup $rp1. KeyAndSecretDetails.SecretUrl a pomocí textu po tajných klíčů / například výstup tajný adresa URL https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163, tajný název B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span><span class="sxs-lookup"><span data-stu-id="3e4e8-138">Value for $secretname can be obtained by referring to the output of $rp1.KeyAndSecretDetails.SecretUrl and using text after secrets/ e.g. output secret URL is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 and secret name is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span></span>
> 2. <span data-ttu-id="3e4e8-139">Hodnota značky DiskEncryptionKeyFileName je stejný jako tajný název.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-139">Value of the tag DiskEncryptionKeyFileName is same as secret name.</span></span>
> 3. <span data-ttu-id="3e4e8-140">Hodnota pro DiskEncryptionKeyEncryptionKeyURL můžete získat z trezoru klíčů po obnovení zpět klíče a použití [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) rutiny</span><span class="sxs-lookup"><span data-stu-id="3e4e8-140">Value for DiskEncryptionKeyEncryptionKeyURL can be obtained from key vault after restoring the keys back and using [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) cmdlet</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="3e4e8-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3e4e8-141">Next steps</span></span>
<span data-ttu-id="3e4e8-142">Po obnovení klíče a tajné zpět do trezoru klíčů, najdete v článku [Správa zálohování a obnovení virtuálních počítačů Azure pomocí prostředí PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) vytvořit šifrovaný virtuální počítače z obnovené disku, klíče a tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="3e4e8-142">After restoring key and secret back to key vault, refer the article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) to create encrypted VMs from restored disk, key and secret.</span></span>
