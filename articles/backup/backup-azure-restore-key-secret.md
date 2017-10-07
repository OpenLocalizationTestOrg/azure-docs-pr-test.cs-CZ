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
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Obnovení klíče trezoru klíč a tajný klíč pro šifrované virtuální počítače pomocí služby Azure Backup
V tomto článku bude zmíněn pomocí zálohování virtuálních počítačů Azure tooperform obnovení šifrovaných virtuálních počítačích Azure, pokud klíč a tajný klíč neexistují v trezoru klíčů hello. Tyto kroky můžete také použijí, pokud chcete, aby toomaintain samostatnou kopii klíč (klíč šifrovacího klíče) a tajný klíč (klíč šifrování nástrojem BitLocker) pro hello obnovit virtuální počítač.

## <a name="prerequisites"></a>Požadavky
* **Šifrované zálohování virtuálních počítačů** – šifrované virtuální počítače Azure byly zálohovány pomocí služby Azure Backup. Najdete v článku hello [Správa zálohování a obnovení virtuálních počítačů Azure pomocí prostředí PowerShell](backup-azure-vms-automation.md) podrobnosti o tom, jak toobackup šifrované virtuálních počítačích Azure.
* **Azure Key Vault konfigurovat** – Ujistěte se, že trezor klíčů toowhich klíče a tajné klíče toobe nutné obnovit již existuje. Najdete v článku hello [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md) podrobnosti o správě trezoru klíčů.
* **Obnovení disku** – Ujistěte se, že máte aktivuje úlohy obnovení pro obnovení disků pro šifrované virtuální počítač pomocí [kroky PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm). Je to proto, že tato úloha generuje soubor JSON v účtu úložiště, který obsahuje klíče a tajné klíče pro toobe hello šifrované virtuální počítač obnovit.

## <a name="get-key-and-secret-from-azure-backup"></a>Získat klíč a tajný klíč z Azure Backup

> [!NOTE]
> Po obnovení disku pro hello šifrování virtuálních počítačů, ujistěte se, že:
> 1. $details naplněný podrobnosti úlohy obnovení disku, jak je uvedeno v [prostředí PowerShell kroky v části disky hello obnovení](backup-azure-vms-automation.md#restore-an-azure-vm)
> 2. Virtuální počítač by měl být vytvořen z obnovené disků pouze **po klíč a tajný klíč trezoru obnovené tookey**.
>
>

Dotaz hello obnovit vlastnosti disku podrobnosti úlohy hello.

```
PS C:\> $properties = $details.properties
PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
PS C:\> $containerName = $properties["Config Blob Container Name"]
PS C:\> $encryptedBlobName = $properties["Encryption Info Blob Name"]
```

Nastavit kontext hello úložiště Azure a obnovte konfigurační soubor JSON obsahující klíče a tajné údaje pro šifrované virtuální počítač.

```
PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
PS C:\> $destination_path = 'C:\vmencryption_config.json'
PS C:\> Get-AzureStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
PS C:\> $encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a>Obnovení klíče
Jakmile soubor JSON hello se generuje ve výše uvedených hello cílovou cestu, generování souboru objektu blob klíče z hello JSON a kanál zpět v trezoru klíčů hello toorestore klíče rutiny tooput hello klíč (KEK).

```
PS C:\> $keyDestination = 'C:\keyDetails.blob'
PS C:\> [io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a>Obnovit tajný klíč
Použijte soubor JSON hello vygeneruje výše tooget tajný název a hodnotu a kanál zpět v trezoru klíčů hello tooset tajný rutiny tooput hello tajný klíč (BEK). **Tyto rutiny použijte, pokud virtuální počítač je zašifrovaný pomocí BEK a KEK.**

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

Pokud je virtuální počítač **šifrovat jenom pomocí BEK**, vygenerujte soubor tajný blob z hello JSON a kanál zpět v trezoru klíčů hello toorestore tajný rutiny tooput hello tajný klíč (BEK).

```
PS C:\> $secretDestination = 'C:\secret.blob'
PS C:\> [io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
PS C:\> Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
```

> [!NOTE]
> 1. Nelze získat hodnotu $secretname odkazující toohello výstup $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl a použitím text po tajných klíčů / například výstup tajný adresa URL je https://keyvaultname.vault.azure.net/secrets/ B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 a tajný název je B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> 2. Hodnota značky hello DiskEncryptionKeyFileName je stejný jako tajný název.
>
>

## <a name="create-virtual-machine-from-restored-disk"></a>Vytvoření virtuálního počítače z obnovené disku
Pokud jste zálohovali šifrované virtuálního počítače pomocí zálohování virtuálních počítačů Azure, hello rutiny prostředí PowerShell nápovědy zmíněné, obnovit klíče a tajné back toohello trezoru klíčů. Po obnovení je, najdete v článku hello [Správa zálohování a obnovení virtuálních počítačů Azure pomocí prostředí PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate zašifrování virtuální počítače z obnovené disku, klíče a tajný klíč.

## <a name="legacy-approach"></a>Starší verze přístup
přístup Hello zmíněné by fungovat pro všechny body obnovení hello. Hello starší způsob získání klíče a tajné informace z bodu obnovení, by však bylo platné pro body obnovení, které jsou starší než 11 července 2017 zašifrovaný pomocí BEK a KEK virtuálních počítačů. Po dokončení pro šifrované virtuální počítač pomocí úlohy obnovení disku [kroky PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm), ujistěte se, že je $rp naplněný platnou hodnotu.

### <a name="restore-key"></a>Obnovení klíče
Pomocí následující rutiny tooget klíč (KEK) informace z bodu obnovení hello a informačního kanálu tooput klíče rutiny toorestore zpátky v trezoru klíčů hello.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a>Obnovit tajný klíč
Pomocí následující rutiny tooget tajný klíč (BEK) informace z bodu obnovení hello a informačního kanálu tooput tajný rutiny tooset zpátky v trezoru klíčů hello.

```
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> 1. Tím, že odkazuje toohello výstup $rp1 nelze získat hodnotu $secretname. KeyAndSecretDetails.SecretUrl a pomocí textu po tajných klíčů / například výstup tajný adresa URL je https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 a je tajný název B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> 2. Hodnota značky hello DiskEncryptionKeyFileName je stejný jako tajný název.
> 3. Hodnota pro DiskEncryptionKeyEncryptionKeyURL můžete získat z trezoru klíčů po obnovení zpět hello klíče a použití [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) rutiny
>
>

## <a name="next-steps"></a>Další kroky
Po obnovení klíče a tajné back tookey trezoru, najdete v článku hello [Správa zálohování a obnovení virtuálních počítačů Azure pomocí prostředí PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate zašifrování virtuální počítače z obnovené disku, klíče a tajný klíč.
