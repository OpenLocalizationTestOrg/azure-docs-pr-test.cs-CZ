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
# <a name="use-azurermrecoveryservicesbackup-cmdlets-tooback-up-virtual-machines"></a>Použijte rutiny tooback AzureRM.RecoveryServices.Backup až virtuální počítače
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-vms-automation.md)
> * [Classic](backup-azure-vms-classic-automation.md)
>
>

Tento článek ukazuje, jak trezoru tooback rutin prostředí Azure PowerShell toouse zálohu a obnovte virtuální počítač Azure (VM) ze služeb zotavení. Trezor služeb zotavení je prostředek Azure Resource Manager a je použité tooprotect data a prostředky služby Azure Backup a Azure Site Recovery. Můžete použít tooprotect trezoru služeb zotavení Azure Service Manager nasazené virtuální počítače a virtuální počítače nasazené Azure Resource Manager.

> [!NOTE]
> Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek je pro použití s virtuálními počítači, které jsou vytvořené pomocí modelu Resource Manager hello.
>
>

Tento článek vás provede pomocí prostředí PowerShell tooprotect virtuálních počítačů a obnovení dat z bodu obnovení.

## <a name="concepts"></a>Koncepty
Pokud nejste obeznámeni s hello služby Azure Backup Přehled služby hello, podívejte se na [co je Azure Backup?](backup-introduction-to-azure-backup.md) Než začnete, zkontrolujte, zda jste zahrnují hello essentials o hello součásti potřebné toowork s Azure Backup a hello omezení hello aktuální virtuální počítač řešení zálohování.

toouse prostředí PowerShell efektivně, je nutné toounderstand hello hierarchii objektů a odkud toostart.

![Hierarchie objektů služby obnovení](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

tooview hello odkazu na rutiny Powershellu AzureRm.RecoveryServices.Backup, najdete v části hello [Azure Backup - rutin služby obnovení](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) v hello knihovna Azure.

## <a name="setup-and-registration"></a>Instalace a registrace
toobegin:

1. [Stáhněte si nejnovější verzi prostředí PowerShell hello](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (hello minimální požadovaná verze je: 1.4.0)
2. Najděte hello rutin prostředí PowerShell zálohování Azure, které jsou k dispozici zadáním hello následující příkaz:

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


pomocí prostředí PowerShell je možné automatizovat Hello následující úlohy:

* Vytvoření trezoru Služeb zotavení
* Zálohování virtuálních počítačů Azure
* Aktivuje úlohu zálohování
* Monitorování úlohy zálohování
* Obnovení virtuálního počítače Azure

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru služby Recovery Services
Hello následující kroky vás provedou vytvoření trezoru služeb zotavení. Trezor služeb zotavení se liší od úložiště záloh.

1. Pokud používáte Azure Backup pro hello poprvé, musíte použít hello  **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)**  rutiny tooregister hello službu Azure Recovery zprostředkovatele s vaším předplatným.

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. Hello trezor služeb zotavení je prostředku Resource Manager, takže je třeba tooplace ji ve skupině prostředků. Můžete použít existující skupinu prostředků nebo vytvořte skupinu prostředků s hello  **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)**  rutiny. Při vytváření skupiny prostředků, zadejte hello název a umístění pro skupinu prostředků hello.  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. Použití hello  **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)**  hello toocreate rutiny trezor služeb zotavení. Ujistěte se, toospecify hello stejné umístění pro hello trezoru, protože byl použit pro skupinu prostředků hello.

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
4. Zadejte typ hello toouse redundance úložiště; můžete použít [místně redundantní úložiště (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) nebo [geograficky redundantní úložiště (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). Hello následující příklad ukazuje, že je možnost hello - BackupStorageRedundancy pro testvault nastavena tooGeoRedundant.

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testvault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > Mnoho rutin Azure Backup vyžadují objekt trezoru služeb zotavení hello jako vstup. Z tohoto důvodu je vhodné toostore hello služeb zotavení zálohy trezoru objekt v proměnné.
   >
   >

## <a name="view-hello-vaults-in-a-subscription"></a>Zobrazení hello trezorů v předplatném.
Použití  **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)**  tooview hello seznam všech trezorů v aktuálním předplatném hello. Můžete použít tento příkaz toocheck, zda byl vytvořen nový trezor, nebo toosee hello k dispozici trezorů v předplatném hello.

Spusťte příkaz hello, Get-AzureRmRecoveryServicesVault tooview všechny trezory v předplatném hello. Hello následující příklad ukazuje hello informace zobrazené pro každý trezor.

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


## <a name="back-up-azure-vms"></a>Zálohování virtuálních počítačů Azure
Použijte tooprotect trezoru služeb zotavení virtuální počítače. Před použitím ochrany hello nastavit kontext trezoru hello (hello typ dat chráněných v trezoru hello) a ověřte zásady ochrany hello. zásady ochrany Hello je plán hello při spuštění úlohy zálohování hello a jak dlouho mají být uchována každý snímek zálohy.

### <a name="set-vault-context"></a>Kontext sady trezoru
Než povolíte ochranu na virtuálním počítači, použijte  **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  tooset hello trezoru kontextu. Jakmile je nastaven kontext trezoru hello, platí následující rutiny tooall. Hello následující příklad nastaví hello trezoru kontext pro úložiště hello *testvault*.

```
PS C:\> Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a>Vytvoření zásady ochrany
Při vytváření trezoru služeb zotavení dodává s výchozí ochrana a zásady uchovávání informací. zásady ochrany výchozí Hello aktivuje úlohu zálohování každý den v zadanou dobu. zásady uchovávání informací výchozí Hello zachová hello denního bodu obnovení po dobu 30 dnů. Můžete použít výchozí hello zásad tooquickly chránit váš počítač a upravovat zásady hello později pomocí různých údajů.

Použití  **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)**  zásady ochrany hello tooview v trezoru hello. Tato rutina tooget můžete použít konkrétní zásadu nebo zásady hello tooview přidružený k typu úlohy. Následující ukázka Hello získá zásady pro typ pracovního vytížení, AzureVM.

```
PS C:\> Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> časové pásmo Hello hello BackupTime pole v prostředí PowerShell je UTC. Při čas zálohování hello se zobrazí v hello portálu Azure, je čas hello však upravenou tooyour místním časovém pásmu.
>
>

Zásady zálohování ochrany je přidružen alespoň jeden zásady uchovávání informací. Zásady uchovávání informací definuje, jak dlouho bod obnovení je udržováno před odstraněním. Použití  **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)**  hello tooview, výchozí zásady uchovávání informací.  Podobně můžete použít  **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)**  tooobtain hello výchozí plán zásad. Hello  **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  rutina vytvoří objekt prostředí PowerShell, který obsahuje informace zásady zálohování. Hello plán a uchovávání objektů zásad slouží jako vstup toohello  **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  rutiny. Hello následující příklad ukládá hello plán zásad a zásad uchovávání informací hello v proměnné. Příklad Hello používá tyto proměnné toodefine hello parametry při vytváření zásady ochrany, *NewPolicy*.

```
PS C:\> $schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```


### <a name="enable-protection"></a>Povolení ochrany
Po definování zásad zálohování ochrany hello je stále nutné povolit hello zásady pro položku. Použití  **[povolit AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)**  tooenable ochrany. Povolení ochrany vyžaduje dva objekty - hello položky a zásady hello. Jakmile zásady hello byla přidružena hello trezoru, aktivaci zálohování pracovního postupu hello během hello definované v plán zásad hello.

Následující příklad povolí ochranu u hello položky, V2VM, pomocí zásad hello, NewPolicy Hello. tooenable hello ochrany na nešifrované virtuálních počítačích Resource Manager

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

tooenable hello ochrany na šifrovaný virtuálních počítačů (zašifrovaný pomocí BEK a KEK), musíte toogive hello Azure Backup service oprávnění tooread klíče a tajné klíče z trezoru klíčů.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

tooenable hello ochrany na šifrovaný virtuálních počítačů (šifrují pomocí BEK pouze), musíte toogive hello Azure Backup service oprávnění tooread tajné klíče z trezoru klíčů.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> Pokud používáte hello cloudu Azure Government, použijte pro parametr hello hello hodnotu ff281ffe-705c-4f53-9f37-a40e6f2c68f3 **- ServicePrincipalName** v [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) rutiny .
>
>

Pro klasické virtuální počítače

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a>Upravit zásady ochrany
zásady ochrany toomodify hello, použijte [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) toomodify hello SchedulePolicy nebo RetentionPolicy objektů.

Hello následující příklad změní hello obnovení bodu uchování too365 dnů.

```
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol.DailySchedule.DurationCountInDays = 365
PS C:\> $pol= Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a>Spustit zálohu
Můžete použít  **[zálohování AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**  tootrigger úlohu zálohování. Pokud je hello prvotní zálohování, je úplné zálohování. Následné zálohy trvat přírůstkové kopie. Být jisti toouse  **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  tooset hello trezoru kontextu před spuštěním úlohy zálohování hello. Hello následující příklad předpokládá, že byl nastaven kontext úložiště.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
PS C:\> $job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup               InProgress            4/23/2016 5:00:30 PM                       cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> časové pásmo Hello hello čas spuštění a čas ukončení polí v prostředí PowerShell je UTC. Při hello čas se zobrazí v hello portálu Azure, je čas hello však upravenou tooyour místním časovém pásmu.
>
>

## <a name="monitoring-a-backup-job"></a>Monitorování úlohy zálohování
Dlouhotrvající operace, jako je například úlohy zálohování, můžete monitorovat bez použití hello portálu Azure. Stav hello tooget probíhající úlohy, použijte hello  **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)**  rutiny. Tato rutina načte hello úlohy zálohování pro konkrétní trezoru a že trezor je uveden v kontextu trezoru hello. Hello následující příklad získá stav hello úlohu v průběhu jako pole a ukládá stav hello hello $joblist proměnné.

```
PS C:\> $joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
PS C:\> $joblist[0]
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Místo dotazování tyto úlohy pro dokončení – což je zbytečné další kód – použít hello  **[čekání AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  rutiny. Tato rutina pozastaví spuštění hello až do dokončení úlohy hello nebo hello zadaný časový limit.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a>Obnovení virtuálního počítače Azure
Je klíčové rozdíl mezi hello obnovení virtuálního počítače pomocí hello portál Azure a obnovení virtuálního počítače pomocí prostředí PowerShell. V prostředí PowerShell hello obnovení bylo dokončeno po vytvoření hello disky a informace o konfiguraci z bodu obnovení hello.

> [!NOTE]
> operace obnovení Hello nevytváří virtuálního počítače.
>
>

toocreate virtuálního počítače z disku, najdete v části hello [hello vytvořit virtuální počítač z uložené disků](backup-azure-vms-automation.md#create-a-vm-from-stored-disks). jsou základní kroky toorestore Hello virtuální počítač Azure:

* Vyberte hello virtuálních počítačů
* Vyberte bod obnovení
* Obnovení hello disky
* Vytvořit hello virtuálních počítačů z uložené disků

Hello následující obrázek znázorňuje hello hierarchie objektů z hello RecoveryServicesVault dolů toohello BackupRecoveryPoint.

![Hierarchie objektů služby obnovení zobrazující BackupContainer](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

zálohovaná data toorestore, určete hello zálohované položky a hello bod obnovení, který obsahuje data v daném okamžiku hello. Použití hello  **[obnovení AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  rutiny toorestore data z hello trezoru účet toohello zákazníka.

### <a name="select-hello-vm"></a>Vyberte hello virtuálních počítačů
tooget hello prostředí PowerShell objekt, který identifikuje hello právo zálohování položek, spusťte v hello kontejneru v trezoru hello a směrem dolů hierarchie objektů hello. tooselect hello kontejneru, který představuje hello virtuální počítač, použijte hello  **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)**  rutiny a zřetězit této toohello  **[ Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**  rutiny.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" –Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem –Container $namedContainer  –WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Vyberte bod obnovení
Použití hello  **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)**  toolist rutiny všechny body obnovení pro položky zálohování hello. Zvolte toorestore bodu obnovení hello. Pokud si nejste jistí, které toouse bodu obnovení, je vhodné toochoose hello nejnovější RecoveryPointType = AppConsistent bodu v seznamu hello.

V následující skript hello, hello proměnnou, **$rp**, je pole bodů obnovení pro hello vybrané zálohování položek z hello posledních sedmi dnech. pole Hello je seřazen v obráceném pořadí času s hello nejnovější bod obnovení na pozici 0. Použijte standardní prostředí PowerShell pole indexování bod obnovení toopick hello. V příkladu hello vybere $rp [0] hello nejnovější bod obnovení.

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



### <a name="restore-hello-disks"></a>Obnovení hello disky
Použití hello  **[obnovení AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  toorestore rutiny položku Zálohování dat a konfigurace tooa bodu obnovení. Jakmile jste našli bod obnovení, použijte ji jako hello hodnotu pro hello **- RecoveryPoint** parametr. V ukázkovém kódu předchozí hello **$rp [0]** byla toouse bodu obnovení hello. V následující ukázkový kód hello **$rp [0]** je hello obnovení bodu toouse pro obnovení disku hello.

toorestore hello disky a informace o konfiguraci:

```
PS C:\> $restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
PS C:\> $restorejob
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Použití hello  **[čekání AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  toowait rutiny pro toocomplete úlohy obnovení hello.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

Po dokončení úlohy obnovení hello použít hello  **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)**  rutiny tooget hello podrobnosti o hello operaci obnovení. Hello JobDetails vlastnost má hello informace potřebné toorebuild hello virtuálních počítačů.

```
PS C:\> $restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

Po obnovení hello disky, přejděte toohello další části toocreate hello virtuálních počítačů.

## <a name="create-a-vm-from-restored-disks"></a>Vytvoření virtuálního počítače z obnovené disků
Po obnovení hello disky, použijte tyto kroky toocreate a nakonfigurujte hello virtuálního počítače z disku.

> [!NOTE]
> toocreate šifrování virtuálních počítačů z obnovené disků, vaše Azure role musí mít oprávnění tooperform hello akce, **Microsoft.KeyVault/vaults/deploy/action**. Pokud vaše role nemá toto oprávnění, vytvořte vlastní role pomocí této akce. Další informace najdete v tématu [vlastní role v Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).
>
>

1. Dotaz hello obnovit vlastnosti disku podrobnosti úlohy hello.

  ```
  PS C:\> $properties = $details.properties
  PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
  PS C:\> $containerName = $properties["Config Blob Container Name"]
  PS C:\> $blobName = $properties["Config Blob Name"]
  ```

2. Nastavit kontext hello úložiště Azure a obnovte hello JSON konfigurační soubor.

    ```
    PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
    PS C:\> $destination_path = "C:\vmconfig.json"
    PS C:\> Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path
    PS C:\> $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
    ```

3. Použijte hello JSON konfigurační soubor toocreate hello konfigurace virtuálního počítače.

    ```
   PS C:\> $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
    ```

4. Připojte hello disk operačního systému a datové disky. V závislosti na konfiguraci hello virtuální počítače klikněte na příslušné rutiny tooview hello relevantní odkaz: 
    - [Nespravované, bez šifrování virtuálních počítačů](#non-managed-non-encrypted-vms)
    - [Nespravované, šifrované virtuální počítače (pouze BEK)](#non-managed-encrypted-vms-bek-only)
    - [Nespravované, šifrované virtuálních počítačů (BEK a KEK)](#non-managed-encrypted-vms-bek-and-kek)
    - [Spravovaných, bez šifrování virtuálních počítačů](#managed-non-encrypted-vms)
    - [Spravovaných, šifrované virtuálních počítačů (BEK a KEK)](#managed-encrypted-vms-bek-and-kek)
    
    #### <a name="non-managed-non-encrypted-vms"></a>Nespravované, bez šifrování virtuálních počítačů

    Použijte hello následující ukázka nespravované, bez šifrování virtuálních počítačů.

    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
    PS C:\> foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
    {
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="non-managed-encrypted-vms-bek-only"></a>Nespravované, šifrované virtuální počítače (pouze BEK)

    Pro nespravované, šifrované virtuálních počítačů (šifrují pomocí BEK pouze) budete potřebovat toorestore hello tajný toohello trezoru klíčů, než je možné připojit disky. Další informace najdete v tématu hello článku [obnovení šifrovaných virtuálního počítače z bodu obnovení Azure Backup](backup-azure-restore-key-secret.md). Následující ukázka Hello ukazuje, jak šifrované tooattach operačního systému a datové disky pro virtuální počítače.

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

    #### <a name="non-managed-encrypted-vms-bek-and-kek"></a>Nespravované, šifrované virtuálních počítačů (BEK a KEK)

    Nespravované, šifrované virtuálních počítačů (zašifrovaný pomocí BEK a KEK) musíte toorestore hello klíče a tajné toohello trezoru klíčů předtím, než je možné připojit disky. Další informace najdete v tématu hello článku [obnovení šifrovaných virtuálního počítače z bodu obnovení Azure Backup](backup-azure-restore-key-secret.md). Následující ukázka Hello ukazuje, jak šifrované tooattach operačního systému a datové disky pro virtuální počítače.

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

    #### <a name="managed-non-encrypted-vms"></a>Spravovaných, bez šifrování virtuálních počítačů

    Pro spravovaných bez šifrování virtuálních počítačů budete potřebovat toocreate spravované disky z úložiště objektů blob a pak připojte hello disky. Podrobné informace najdete v článku hello, [připojit tooa disku data virtuálního počítače s Windows pomocí prostředí PowerShell](../virtual-machines/windows/attach-disk-ps.md). Hello následující vzorový kód ukazuje, jak tooattach hello datových disků pro virtuální počítače spravované bez šifrování.

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

    #### <a name="managed-encrypted-vms-bek-and-kek"></a>Spravovaných, šifrované virtuálních počítačů (BEK a KEK)

    Pro spravované šifrované virtuální počítače (zašifrovaný pomocí BEK a KEK) budete potřebovat toocreate spravované disky z úložiště objektů blob a pak připojte hello disky. Podrobné informace najdete v článku hello, [připojit tooa disku data virtuálního počítače s Windows pomocí prostředí PowerShell](../virtual-machines/windows/attach-disk-ps.md). Hello následující vzorový kód ukazuje, jak tooattach hello datových disků pro spravovaných šifrované virtuálních počítačů.

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

5. Nastavení sítě hello.

    ```
    PS C:\> $nicName="p1234"
    PS C:\> $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    PS C:\> $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    PS C:\> $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    PS C:\> $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. Vytvořte virtuální počítač hello.

    ```    
    PS C:\> New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="next-steps"></a>Další kroky
Pokud dáváte přednost tooengage toouse prostředí PowerShell s vašich prostředků Azure, najdete v článku prostředí PowerShell hello [nasadit a spravovat zálohy pro Windows Server](backup-client-automation.md). Pokud budete spravovat zálohy aplikace DPM, najdete v článku hello, [nasadit a spravovat zálohy pro DPM](backup-dpm-automation.md). Mají obě z těchto článků verze pro nasazení Resource Manager a nasazení Classic.  
