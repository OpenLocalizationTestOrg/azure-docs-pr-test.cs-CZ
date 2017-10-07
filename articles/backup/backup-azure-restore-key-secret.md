---
title: "aaaRestore Key Vault klíč a tajný klíč pro šifrované virtuálních počítačů pomocí služby Azure Backup | Microsoft Docs"
description: "Zjistěte, jak toorestore klíč trezoru klíč a tajný klíč v Azure Backup pomocí prostředí PowerShell"
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
ms.openlocfilehash: 659b2f647493ffcc9494caee111c8bd584ef9c7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a><span data-ttu-id="e0611-103">Obnovení klíče trezoru klíč a tajný klíč pro šifrované virtuální počítače pomocí služby Azure Backup</span><span class="sxs-lookup"><span data-stu-id="e0611-103">Restore Key Vault key and secret for encrypted VMs using Azure Backup</span></span>
<span data-ttu-id="e0611-104">V tomto článku bude zmíněn pomocí zálohování virtuálních počítačů Azure tooperform obnovení šifrovaných virtuálních počítačích Azure, pokud klíč a tajný klíč neexistují v trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="e0611-104">This article talks about using Azure VM Backup tooperform restore of encrypted Azure VMs, if your key and secret do not exist in hello key vault.</span></span> <span data-ttu-id="e0611-105">Tyto kroky můžete také použijí, pokud chcete, aby toomaintain samostatnou kopii klíč (klíč šifrovacího klíče) a tajný klíč (klíč šifrování nástrojem BitLocker) pro hello obnovit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e0611-105">These steps can also be used if you want toomaintain a separate copy of key (Key Encryption Key) and secret (BitLocker Encryption Key) for hello restored VM.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e0611-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e0611-106">Prerequisites</span></span>
* <span data-ttu-id="e0611-107">**Šifrované zálohování virtuálních počítačů** – šifrované virtuální počítače Azure byly zálohovány pomocí služby Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="e0611-107">**Backup encrypted VMs** - Encrypted Azure VMs have been backed up using Azure Backup.</span></span> <span data-ttu-id="e0611-108">Najdete v článku hello [Správa zálohování a obnovení virtuálních počítačů Azure pomocí prostředí PowerShell](backup-azure-vms-automation.md) podrobnosti o tom, jak toobackup šifrované virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="e0611-108">Refer hello article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md) for details about how toobackup encrypted Azure VMs.</span></span>
* <span data-ttu-id="e0611-109">**Azure Key Vault konfigurovat** – Ujistěte se, že trezor klíčů toowhich klíče a tajné klíče toobe nutné obnovit již existuje.</span><span class="sxs-lookup"><span data-stu-id="e0611-109">**Configure Azure Key Vault** – Ensure that key vault toowhich keys and secrets need toobe restored is already present.</span></span> <span data-ttu-id="e0611-110">Najdete v článku hello [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md) podrobnosti o správě trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e0611-110">Refer hello article [Get Started with Azure Key Vault](../key-vault/key-vault-get-started.md) for details about key vault management.</span></span>
* <span data-ttu-id="e0611-111">**Obnovení disku** – Ujistěte se, že máte aktivuje úlohy obnovení pro obnovení disků pro šifrované virtuální počítač pomocí [kroky PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm).</span><span class="sxs-lookup"><span data-stu-id="e0611-111">**Restore disk** - Ensure that you have triggered restore job for restoring disks for encrypted VM using [PowerShell steps](backup-azure-vms-automation.md#restore-an-azure-vm).</span></span> <span data-ttu-id="e0611-112">Je to proto, že tato úloha generuje soubor JSON v účtu úložiště, který obsahuje klíče a tajné klíče pro toobe hello šifrované virtuální počítač obnovit.</span><span class="sxs-lookup"><span data-stu-id="e0611-112">This is because this job generates a JSON file in your storage account containing keys and secrets for hello encrypted VM toobe restored.</span></span>

## <a name="get-key-and-secret-from-azure-backup"></a><span data-ttu-id="e0611-113">Získat klíč a tajný klíč z Azure Backup</span><span class="sxs-lookup"><span data-stu-id="e0611-113">Get key and secret from Azure Backup</span></span>

> [!NOTE]
> <span data-ttu-id="e0611-114">Po obnovení disku pro hello šifrování virtuálních počítačů, ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="e0611-114">Once disk has been restored for hello encrypted VM, ensure that:</span></span>
> 1. <span data-ttu-id="e0611-115">$details naplněný podrobnosti úlohy obnovení disku, jak je uvedeno v [prostředí PowerShell kroky v části disky hello obnovení](backup-azure-vms-automation.md#restore-an-azure-vm)</span><span class="sxs-lookup"><span data-stu-id="e0611-115">$details is populated with restore disk job details, as mentioned in [PowerShell steps in Restore hello Disks section](backup-azure-vms-automation.md#restore-an-azure-vm)</span></span>
> 2. <span data-ttu-id="e0611-116">Virtuální počítač by měl být vytvořen z obnovené disků pouze **po klíč a tajný klíč trezoru obnovené tookey**.</span><span class="sxs-lookup"><span data-stu-id="e0611-116">VM should be created from restored disks only **after key and secret is restored tookey vault**.</span></span>
>
>

<span data-ttu-id="e0611-117">Dotaz hello obnovit vlastnosti disku podrobnosti úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="e0611-117">Query hello restored disk properties for hello job details.</span></span>

```
PS C:\> $properties = $details.properties
PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
PS C:\> $containerName = $properties["Config Blob Container Name"]
PS C:\> $encryptedBlobName = $properties["Encryption Info Blob Name"]
```

<span data-ttu-id="e0611-118">Nastavit kontext hello úložiště Azure a obnovte konfigurační soubor JSON obsahující klíče a tajné údaje pro šifrované virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e0611-118">Set hello Azure storage context and restore JSON configuration file containing key and secret details for encrypted VM.</span></span>

```
PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
PS C:\> $destination_path = 'C:\vmencryption_config.json'
PS C:\> Get-AzureStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
PS C:\> $encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a><span data-ttu-id="e0611-119">Obnovení klíče</span><span class="sxs-lookup"><span data-stu-id="e0611-119">Restore key</span></span>
<span data-ttu-id="e0611-120">Jakmile soubor JSON hello se generuje ve výše uvedených hello cílovou cestu, generování souboru objektu blob klíče z hello JSON a kanál zpět v trezoru klíčů hello toorestore klíče rutiny tooput hello klíč (KEK).</span><span class="sxs-lookup"><span data-stu-id="e0611-120">Once hello JSON file is generated in hello destination path mentioned above, generate key blob file from hello JSON and feed it toorestore key cmdlet tooput hello key (KEK) back in hello key vault.</span></span>

```
PS C:\> $keyDestination = 'C:\keyDetails.blob'
PS C:\> [io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a><span data-ttu-id="e0611-121">Obnovit tajný klíč</span><span class="sxs-lookup"><span data-stu-id="e0611-121">Restore secret</span></span>
<span data-ttu-id="e0611-122">Použijte soubor JSON hello vygeneruje výše tooget tajný název a hodnotu a kanál zpět v trezoru klíčů hello tooset tajný rutiny tooput hello tajný klíč (BEK).</span><span class="sxs-lookup"><span data-stu-id="e0611-122">Use hello JSON file generated above tooget secret name and value and feed it tooset secret cmdlet tooput hello secret (BEK) back in hello key vault.</span></span> <span data-ttu-id="e0611-123">**Tyto rutiny použijte, pokud virtuální počítač je zašifrovaný pomocí BEK a KEK.**</span><span class="sxs-lookup"><span data-stu-id="e0611-123">**Use these cmdlets if your VM is encrypted using BEK and KEK.**</span></span>

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

<span data-ttu-id="e0611-124">Pokud je virtuální počítač **šifrovat jenom pomocí BEK**, vygenerujte soubor tajný blob z hello JSON a kanál zpět v trezoru klíčů hello toorestore tajný rutiny tooput hello tajný klíč (BEK).</span><span class="sxs-lookup"><span data-stu-id="e0611-124">If your VM is **encrypted using BEK only**, generate secret blob file from hello JSON and feed it toorestore secret cmdlet tooput hello secret (BEK) back in hello key vault.</span></span>

```
PS C:\> $secretDestination = 'C:\secret.blob'
PS C:\> [io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
PS C:\> Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
```

> [!NOTE]
> 1. <span data-ttu-id="e0611-125">Nelze získat hodnotu $secretname odkazující toohello výstup $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl a použitím text po tajných klíčů / například výstup tajný adresa URL je https://keyvaultname.vault.azure.net/secrets/ B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 a tajný název je B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span><span class="sxs-lookup"><span data-stu-id="e0611-125">Value for $secretname can be obtained by referring toohello output of $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl and using text after secrets/ e.g. output secret URL is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 and secret name is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span></span>
> 2. <span data-ttu-id="e0611-126">Hodnota značky hello DiskEncryptionKeyFileName je stejný jako tajný název.</span><span class="sxs-lookup"><span data-stu-id="e0611-126">Value of hello tag DiskEncryptionKeyFileName is same as secret name.</span></span>
>
>

## <a name="create-virtual-machine-from-restored-disk"></a><span data-ttu-id="e0611-127">Vytvoření virtuálního počítače z obnovené disku</span><span class="sxs-lookup"><span data-stu-id="e0611-127">Create virtual machine from restored disk</span></span>
<span data-ttu-id="e0611-128">Pokud jste zálohovali šifrované virtuálního počítače pomocí zálohování virtuálních počítačů Azure, hello rutiny prostředí PowerShell nápovědy zmíněné, obnovit klíče a tajné back toohello trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e0611-128">If you have backed up encrypted VM using Azure VM Backup, hello PowerShell cmdlets mentioned above help you restore key and secret back toohello key vault.</span></span> <span data-ttu-id="e0611-129">Po obnovení je, najdete v článku hello [Správa zálohování a obnovení virtuálních počítačů Azure pomocí prostředí PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate zašifrování virtuální počítače z obnovené disku, klíče a tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="e0611-129">After restoring them, refer hello article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate encrypted VMs from restored disk, key, and secret.</span></span>

## <a name="legacy-approach"></a><span data-ttu-id="e0611-130">Starší verze přístup</span><span class="sxs-lookup"><span data-stu-id="e0611-130">Legacy approach</span></span>
<span data-ttu-id="e0611-131">přístup Hello zmíněné by fungovat pro všechny body obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="e0611-131">hello approach mentioned above would work for all hello recovery points.</span></span> <span data-ttu-id="e0611-132">Hello starší způsob získání klíče a tajné informace z bodu obnovení, by však bylo platné pro body obnovení, které jsou starší než 11 července 2017 zašifrovaný pomocí BEK a KEK virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e0611-132">However, hello older approach of getting key and secret information from recovery point, would be valid for recovery points older than July 11, 2017 for VMs encrypted using BEK and KEK.</span></span> <span data-ttu-id="e0611-133">Po dokončení pro šifrované virtuální počítač pomocí úlohy obnovení disku [kroky PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm), ujistěte se, že je $rp naplněný platnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e0611-133">Once restore disk job is complete for encrypted VM using [PowerShell steps](backup-azure-vms-automation.md#restore-an-azure-vm), ensure that $rp is populated with a valid value.</span></span>

### <a name="restore-key"></a><span data-ttu-id="e0611-134">Obnovení klíče</span><span class="sxs-lookup"><span data-stu-id="e0611-134">Restore key</span></span>
<span data-ttu-id="e0611-135">Pomocí následující rutiny tooget klíč (KEK) informace z bodu obnovení hello a informačního kanálu tooput klíče rutiny toorestore zpátky v trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="e0611-135">Use hello following cmdlets tooget key (KEK) information from recovery point and feed it toorestore key cmdlet tooput it back in hello key vault.</span></span>

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a><span data-ttu-id="e0611-136">Obnovit tajný klíč</span><span class="sxs-lookup"><span data-stu-id="e0611-136">Restore secret</span></span>
<span data-ttu-id="e0611-137">Pomocí následující rutiny tooget tajný klíč (BEK) informace z bodu obnovení hello a informačního kanálu tooput tajný rutiny tooset zpátky v trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="e0611-137">Use hello following cmdlets tooget secret (BEK) information from recovery point and feed it tooset secret cmdlet tooput it back in hello key vault.</span></span>

```
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> 1. <span data-ttu-id="e0611-138">Tím, že odkazuje toohello výstup $rp1 nelze získat hodnotu $secretname. KeyAndSecretDetails.SecretUrl a pomocí textu po tajných klíčů / například výstup tajný adresa URL je https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 a je tajný název B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span><span class="sxs-lookup"><span data-stu-id="e0611-138">Value for $secretname can be obtained by referring toohello output of $rp1.KeyAndSecretDetails.SecretUrl and using text after secrets/ e.g. output secret URL is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 and secret name is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span></span>
> 2. <span data-ttu-id="e0611-139">Hodnota značky hello DiskEncryptionKeyFileName je stejný jako tajný název.</span><span class="sxs-lookup"><span data-stu-id="e0611-139">Value of hello tag DiskEncryptionKeyFileName is same as secret name.</span></span>
> 3. <span data-ttu-id="e0611-140">Hodnota pro DiskEncryptionKeyEncryptionKeyURL můžete získat z trezoru klíčů po obnovení zpět hello klíče a použití [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) rutiny</span><span class="sxs-lookup"><span data-stu-id="e0611-140">Value for DiskEncryptionKeyEncryptionKeyURL can be obtained from key vault after restoring hello keys back and using [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) cmdlet</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="e0611-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0611-141">Next steps</span></span>
<span data-ttu-id="e0611-142">Po obnovení klíče a tajné back tookey trezoru, najdete v článku hello [Správa zálohování a obnovení virtuálních počítačů Azure pomocí prostředí PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate zašifrování virtuální počítače z obnovené disku, klíče a tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="e0611-142">After restoring key and secret back tookey vault, refer hello article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate encrypted VMs from restored disk, key and secret.</span></span>
