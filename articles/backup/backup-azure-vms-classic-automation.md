---
title: "aaaDeploy a spravovat zálohy pro virtuální počítače Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toodeploy a spravovat Azure Backup pomocí prostředí PowerShell."
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
ms.openlocfilehash: f3ecd3f94c5e3e8fc8018e8786302edd4847744b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azurermbackup-cmdlets-tooback-up-virtual-machines"></a>Použijte rutiny tooback AzureRM.Backup až virtuální počítače
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-vms-automation.md)
> * [Classic](backup-azure-vms-classic-automation.md)
>
>

Tento článek ukazuje, jak toouse prostředí Azure PowerShell pro zálohování a obnovení virtuálních počítačů Azure. Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: Resource Manager a Classic. Tento článek popisuje použití hello klasického nasazení modelu tooback do trezoru služby Backup tooa data. Pokud jste dosud nevytvořili úložiště záloh ve vašem předplatném, naleznete v části hello Resource Manager verze tohoto článku [AzureRM.RecoveryServices.Backup použití rutin tooback až virtuálních počítačů](backup-azure-vms-automation.md). Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

> [!IMPORTANT]
> Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup. Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md). Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.<br/> Po 15 říjen 2017 nemůžete použít trezory Backup toocreate prostředí PowerShell. **Do 1. listopadu 2017:**
>- Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.
>- Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello. Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.
>

## <a name="concepts"></a>Koncepty
Tento článek obsahuje informace o konkrétní toohello rutiny prostředí PowerShell tooback až virtuálních počítačů. Úvodní informace o ochraně virtuálních počítačích Azure, najdete v tématu [plánování vaší infrastruktury zálohování virtuálních počítačů v Azure](backup-azure-vms-introduction.md).

> [!NOTE]
> Než začnete, přečtěte si hello [požadavky](backup-azure-vms-prepare.md) požadované toowork s Azure Backup a hello [omezení](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) hello aktuální virtuální počítač zálohování řešení.
>
>

toouse prostředí PowerShell prakticky trvat hierarchie chvíli toounderstand hello objektů a odkud toostart.

![Hierarchie objektů](./media/backup-azure-vms-classic-automation/object-hierarchy.png)

Hello dva nejdůležitější toky jsou povolení ochrany pro virtuální počítač a obnovení dat z bodu obnovení. Hello cílem tohoto článku je toohelp stát adept na práci s tooenable rutiny prostředí PowerShell hello těchto dvou scénářů.

## <a name="setup-and-registration"></a>Instalace a registrace
toobegin:

1. [Stáhněte si nejnovější PowerShell](https://github.com/Azure/azure-powershell/releases) (minimální požadovaná verze je: 1.0.0)
2. Najděte hello rutin prostředí PowerShell zálohování Azure, které jsou k dispozici zadáním hello následující příkaz:

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

Hello následující nastavení a registrace úlohy je možné automatizovat pomocí prostředí PowerShell:

* Vytvoření trezoru záloh
* Registrace virtuálních počítačů hello hello služby zálohování Azure

### <a name="create-a-backup-vault"></a>Vytvoření trezoru záloh
> [!WARNING]
> Pro zákazníky používající Azure Backup pro hello poprvé budete potřebovat tooregister hello Azure Backup zprostředkovatele toobe používat s vaším předplatným. To lze provést tak, že spustíte následující příkaz hello: Register-AzureRmResourceProvider - ProviderNamespace "Microsoft.Backup"
>
>

Můžete vytvořit nové úložiště záloh pomocí hello **New-AzureRmBackupVault** rutiny. Trezor záloh Hello je prostředek ARM, proto musíte tooplace ji ve skupině prostředků. V prostředí Azure PowerShell konzolu se zvýšenými oprávněními spusťte následující příkazy hello:

```
PS C:\> New-AzureRmResourceGroup –Name “test-rg” –Location “West US”
PS C:\> $backupvault = New-AzureRmBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

Můžete získat seznam všech hello záloh v daném předplatném pomocí hello **Get-AzureRmBackupVault** rutiny.

> [!NOTE]
> Je vhodné toostore hello úložiště záloh objektu do proměnné. objekt trezoru Hello je potřeba jako vstup pro mnoho rutin Azure Backup.
>
>

### <a name="registering-hello-vms"></a>Registrace virtuálních počítačů hello
Hello první krok k konfigurace zálohování Azure Backup je tooregister počítač nebo virtuální počítač s trezoru služby Azure Backup. Hello **Register-AzureRmBackupContainer** rutiny trvá hello vstupní informace virtuálního počítače Azure IaaS a zaregistruje ho se hello zadaný trezor. operaci registrace Hello přidruží úložiště záloh hello hello virtuální počítač Azure a sleduje hello virtuálních počítačů prostřednictvím hello zálohování životního cyklu.

Registrace virtuálního počítače s hello služby Azure Backup vytvoří objekt kontejner nejvyšší úrovně. Kontejner obvykle obsahuje více položek, které lze zálohovat, ale v případě hello virtuálních počítačů bude jen jednu položku zálohování pro kontejner hello.

```
PS C:\> $registerjob = Register-AzureRmBackupContainer -Vault $backupvault -Name "testvm" -ServiceName "testvm"
```

## <a name="backup-azure-vms"></a>Zálohování virtuálních počítačů Azure
### <a name="create-a-protection-policy"></a>Vytvoření zásady ochrany
Není povinné toocreate novou zálohu zásad toostart ochrany virtuálních počítačů. Hello trezoru se dodává s 'výchozí zásadu, která lze použít tooquickly povolení ochrany a potom je upravit později pomocí podrobností vpravo hello. Seznam zásad hello k dispozici v trezoru hello můžete získat pomocí hello **Get-AzureRmBackupProtectionPolicy** rutiny:

```
PS C:\> Get-AzureRmBackupProtectionPolicy -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DefaultPolicy             AzureVM            Daily              26-Aug-15 12:30:00 AM
```

> [!NOTE]
> časové pásmo Hello hello BackupTime pole v prostředí PowerShell je UTC. Pokud čas zálohování hello je zobrazeno v hello portálu Azure, je časové pásmo hello však zarovnaný tooyour místní systém společně s posun hello UTC.
>
>

Zásady zálohování je přidružen alespoň jeden zásady uchovávání informací. zásady uchovávání informací Hello definuje, jak dlouho se uchovává bodu obnovení se Azure Backup. Hello **New-AzureRmBackupRetentionPolicy** rutina vytvoří prostředí PowerShell objekty, které obsahují informace o zásadách uchovávání informací. Tyto objekty zásad uchovávání dat se používají jako vstup toohello *New-AzureRmBackupProtectionPolicy* rutiny, nebo přímo pomocí hello *povolit AzureRmBackupProtection* rutiny.

Zásady zálohování definují, kdy a jak často hello položky ukončení zálohování. Hello **New-AzureRmBackupProtectionPolicy** rutina vytvoří objekt prostředí PowerShell, který obsahuje informace zásady zálohování. Hello zásady zálohování se používá jako vstupní toohello *povolit AzureRmBackupProtection* rutiny.

```
PS C:\> $Daily = New-AzureRmBackupRetentionPolicyObject -DailyRetention -Retention 30
PS C:\> $newpolicy = New-AzureRmBackupProtectionPolicy -Name DailyBackup01 -Type AzureVM -Daily -BackupTime ([datetime]"3:30 PM") -RetentionPolicy $Daily -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DailyBackup01             AzureVM            Daily              01-Sep-15 3:30:00 PM
```

### <a name="enable-protection"></a>Povolení ochrany
Povolení ochrany zahrnuje dva objekty – hello položky a hello zásady a i toohello toobelong nutné stejné trezoru. Jakmile zásady hello už přidružené k položce hello, bude zálohování pracovního postupu hello nové hello definovaného plánu.

```
PS C:\> Get-AzureRmBackupContainer -Type AzureVM -Status Registered -Vault $backupvault | Get-AzureRmBackupItem | Enable-AzureRmBackupProtection -Policy $newpolicy
```

### <a name="initial-backup"></a>Prvotní zálohování
plán zálohování Hello se postará o provádění a hello úplné počáteční kopie pro položku hello hello přírůstkové kopie pro následné zálohy hello. Pokud však chcete tooforce hello počáteční zálohování toohappen v určitém čase nebo i okamžitě pak použít hello **zálohování AzureRmBackupItem** rutiny:

```
PS C:\> $container = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -Name "testvm"
PS C:\> $backupjob = Get-AzureRmBackupItem -Container $container | Backup-AzureRmBackupItem
PS C:\> $backupjob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

> [!NOTE]
> Hello časové pásmo hello čas spuštění a čas ukončení polí zobrazených v prostředí PowerShell je čas UTC. Když hello podobné informace se zobrazí v hello portálu Azure, je časové pásmo hello však zarovnaný tooyour místní systémové hodiny.
>
>

### <a name="monitoring-a-backup-job"></a>Monitorování úlohy zálohování
Většina dlouho běžící operace ve službě Azure Backup se modelována jako úlohu. Díky tomu je snadno tootrack průběh bez nutnosti tookeep hello portálu Azure otevřete za všech okolností.

tooget hello nejnovější stav probíhající úlohy, použijte hello **Get-AzureRmBackupJob** rutiny.

```
PS C:\> $joblist = Get-AzureRmBackupJob -Vault $backupvault -Status InProgress
PS C:\> $joblist[0]

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

Místo dotazování tyto úlohy pro dokončení – což je zbytečné, další kód – je jednodušší hello toouse **čekání AzureRmBackupJob** rutiny. Při použití ve skriptu, bude rutina hello pozastavit provádění hello až do dokončení úlohy hello nebo hello zadaný časový limit.

```
PS C:\> Wait-AzureRmBackupJob -Job $joblist[0] -Timeout 43200
```


## <a name="restore-an-azure-vm"></a>Obnovení virtuálního počítače Azure
V pořadí toorestore zálohovaná data je nutné tooidentify hello zálohované položky a hello bod obnovení, který obsahuje data v daném okamžiku hello. Tyto informace jsou zadaná toohello obnovení AzureRmBackupItem rutiny tooinitiate obnovení dat z hello trezoru toohello účtu zákazníka.

### <a name="select-hello-vm"></a>Vyberte hello virtuálních počítačů
tooget hello prostředí PowerShell objekt, který identifikuje Dobrý den správné záložní položku, je třeba toostart z hello kontejneru v trezoru hello a směrem dolů hierarchie objektů. tooselect hello kontejneru, který představuje hello virtuální počítač, použijte hello **Get-AzureRmBackupContainer** rutiny a zřetězit této toohello **Get-AzureRmBackupItem** rutiny.

```
PS C:\> $backupitem = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -name "testvm" | Get-AzureRmBackupItem
```

### <a name="choose-a-recovery-point"></a>Vyberte bod obnovení
Nyní můžete vytvořit seznam všech bodů obnovení hello pro hello zálohování položek pomocí hello **Get-AzureRmBackupRecoveryPoint** rutiny a zvolte toorestore bodu obnovení hello. Obvykle uživatele vyberte hello nejnovější *AppConsistent* bodu v seznamu hello.

```
PS C:\> $rp =  Get-AzureRmBackupRecoveryPoint -Item $backupitem
PS C:\> $rp

RecoveryPointId    RecoveryPointType  RecoveryPointTime      ContainerName
---------------    -----------------  -----------------      -------------
15273496567119     AppConsistent      01-Sep-15 12:27:38 PM  iaasvmcontainer;testvm;testv...
```

Proměnná Hello ```$rp``` je pole bodů obnovení pro hello vybrané zálohování položky seřazeny v opačném pořadí času – hello nejnovější bod obnovení je na pozici 0. Použijte standardní prostředí PowerShell pole indexování bod obnovení toopick hello. Příklad: ```$rp[0]``` vybere hello nejnovější bod obnovení.

### <a name="restoring-disks"></a>Obnovování disků
Je klíčové rozdíl mezi operace obnovení hello provést prostřednictvím hello portál Azure a pomocí prostředí Azure PowerShell. Pomocí prostředí PowerShell operace obnovení hello zastaví obnovování hello disky a konfigurační informace z bodu obnovení hello. Virtuální počítač se nevytvoří.

> [!WARNING]
> Hello obnovení AzureRmBackupItem nedojde k vytvoření virtuálního počítače. Obnoví pouze disky hello toohello zadaný účet úložiště. Toto není hello stejné chování, které vám bude na základě zkušeností uživatelů hello portálu Azure.
>
>

```
PS C:\> $restorejob = Restore-AzureRmBackupItem -StorageAccountName "DestAccount" -RecoveryPoint $rp[0]
PS C:\> $restorejob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Restore         InProgress      01-Sep-15 1:14:01 PM   01-Jan-01 12:00:00 AM
```

Můžete získat podrobnosti o hello hello operace obnovení pomocí hello **Get-AzureRmBackupJobDetails** rutiny po dokončení úlohy obnovení hello. Hello *detaily chyby* vlastnost bude mít hello informace potřebné toorebuild hello virtuálních počítačů.

```
PS C:\> $restorejob = Get-AzureRmBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmBackupJobDetails -Job $restorejob
```

### <a name="build-hello-vm"></a>Sestavení hello virtuálních počítačů
Hello vytváření virtuálních počítačů mimo hello obnovit disky lze provést pomocí rutiny Azure Service Management PowerShell starší hello, hello nové šablony Azure Resource Manager, nebo i pomocí hello portálu Azure. Ukážeme zběžný příklad, jak tooget zde pomocí rutin Azure Service Management hello.

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

Další informace o toobuild virtuálního počítače z disků hello obnovit, najdete v tématu o hello následující rutiny:

* [Přidat AzureDisk](https://msdn.microsoft.com/library/azure/dn495252.aspx)
* [Nové AzureVMConfig](https://msdn.microsoft.com/library/azure/dn495159.aspx)
* [Nový AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)

## <a name="code-samples"></a>Ukázky kódů
### <a name="1-get-hello-completion-status-of-job-sub-tasks"></a>1. Získat stav dokončení hello dílčí úlohy
tootrack hello stav dokončení jednotlivé dílčí úkoly, můžete použít hello **Get-AzureRmBackupJobDetails** rutiny:

```
PS C:\> $details = Get-AzureRmBackupJobDetails -JobId $backupjob.InstanceId -Vault $backupvault
PS C:\> $details.SubTasks

Name                                                        Status
----                                                        ------
Take Snapshot                                               Completed
Transfer data tooBackup vault                               InProgress
```

### <a name="2-create-a-dailyweekly-report-of-backup-jobs"></a>2. Vytvoření sestavy denně nebo týdně úloh zálohování
Správci mají obvykle tooknow spuštěný jaké úlohy zálohování v hello posledních 24 hodin, stav hello tyto úlohy zálohování. Kromě toho hello množství dat přenesených umožňuje správcům tooestimate způsob jejich měsíčního využití dat. níže uvedený skript Hello získává hello nezpracovaná data ze služby Azure Backup hello a zobrazuje hello informace v konzole PowerShell hello.

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
    $dailyjoblist = Get-AzureRmBackupJob -Vault $backupvault -From $startdate -too$enddate -Type AzureVM -Operation Backup
    Write-Progress -Activity "Getting job information for hello last $numberofdays days" -Status "Day -$i" -PercentComplete ([int]([decimal]$i*100/$numberofdays))

    foreach( $job in $dailyjoblist )
    {
        #Extract hello information for hello reports
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

Pokud chcete tooadd grafů výstup sestavy toothis možnosti, přečtěte si z příspěvek blogu TechNet hello [grafů, pomocí prostředí PowerShell](http://blogs.technet.com/b/richard_macdonald/archive/2009/04/28/3231887.aspx)

## <a name="next-steps"></a>Další kroky
Pokud dáváte přednost, pomocí prostředí PowerShell tooengage vašich prostředků Azure, podívejte se na článek hello prostředí PowerShell pro ochranu v systému Windows Server [nasadit a spravovat zálohy pro Windows Server](backup-client-automation-classic.md). Je také článku prostředí PowerShell pro správu zálohování DPM [nasadit a spravovat zálohy pro DPM](backup-dpm-automation-classic.md). Mají obě z těchto článků verze pro nasazení Resource Manager a také nasazení Classic.
