---
title: "aaaUse prostředí PowerShell tooback až Windows serveru tooAzure | Microsoft Docs"
description: "Zjistěte, jak toodeploy a spravovat Azure Backup pomocí prostředí PowerShell"
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 65218095-2996-44d9-917b-8c84fc9ac415
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: saurse;markgal;jimpark;nkolli;trinadhk
ms.openlocfilehash: f13224f53abd6fbd132fee4347b0b99e8f5e2678
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-windows-serverwindows-client-using-powershell"></a>Nasazení a správě zálohování tooAzure pro klienta systému Windows Server a Windows pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [ARM](backup-client-automation.md)
> * [Classic](backup-client-automation-classic.md)
>
>

Tento článek ukazuje, jak toouse prostředí PowerShell pro nastavení služby Azure Backup na Windows serveru nebo klienta Windows a Správa zálohování a obnovení.

## <a name="install-azure-powershell"></a>Instalace prostředí Azure PowerShell
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Tento článek se zaměřuje na hello Azure Resource Manager (ARM) a hello rutiny prostředí PowerShell zálohování Online s MS, povolit toouse trezoru služeb zotavení ve skupině prostředků.

Října 2015 byla vydána Azure PowerShell 1.0. Tato verze byla úspěšná hello 0.9.8 verze a uvést do režimu o významné změny, zejména v vzoru pro pojmenovávání hello rutin hello. postupujte podle 1.0 rutiny hello formát pojmenování {sloveso}-AzureRm {podstatné jméno}; vzhledem k tomu, názvy hello 0.9.8 neobsahují **Rm** (třeba New-AzureRmResourceGroup místo New-AzureResourceGroup). Pokud používáte prostředí Azure PowerShell 0.9.8, musíte nejdřív povolit režimu Resource Manager hello spuštěním hello **Switch-AzureMode AzureResourceManager** příkaz. Tento příkaz není nutné v 1.0 nebo novější.

Pokud chcete toouse skripty napsané pro prostředí hello 0.9.8, v hello 1.0 nebo novější prostředí, měli pečlivě aktualizace a testování skriptů hello v předprodukčním prostředí, než je použijete v produkčním tooavoid neočekávané dopad.

[Stáhněte si nejnovější verzi prostředí PowerShell hello](https://github.com/Azure/azure-powershell/releases) (minimální požadovaná verze je: 1.0.0)

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru služby Recovery Services
Hello následující kroky vás provedou vytvoření trezoru služeb zotavení. Trezor služeb zotavení se liší od úložiště záloh.

1. Pokud používáte Azure Backup pro hello poprvé, musíte použít hello **Register-AzureRMResourceProvider** rutiny tooregister hello službu Azure Recovery zprostředkovatele s vaším předplatným.

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. Hello trezor služeb zotavení je ARM prostředek, takže je třeba tooplace ji ve skupině prostředků. Můžete použít existující skupinu prostředků nebo vytvořte novou. Když vytváříte novou skupinu prostředků, zadejte hello název a umístění pro skupinu prostředků hello.  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "WestUS"
    ```
3. Použití hello **New-AzureRmRecoveryServicesVault** rutiny toocreate hello nový trezor. Ujistěte se, toospecify hello stejné umístění pro hello trezoru, protože byl použit pro skupinu prostředků hello.

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "WestUS"
    ```
4. Zadejte typ hello toouse redundance úložiště; můžete použít [místně redundantní úložiště (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) nebo [geograficky redundantní úložiště (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). Hello následující příklad ukazuje, že je možnost hello - BackupStorageRedundancy pro testVault nastavena tooGeoRedundant.

   > [!TIP]
   > Mnoho rutin Azure Backup vyžadují objekt trezoru služeb zotavení hello jako vstup. Z tohoto důvodu je vhodné toostore hello služeb zotavení zálohy trezoru objekt v proměnné.
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-hello-vaults-in-a-subscription"></a>Zobrazení hello trezorů v předplatném.
Použití **Get-AzureRmRecoveryServicesVault** tooview hello seznam všech trezorů v aktuálním předplatném hello. Můžete použít tento příkaz toocheck, zda byl vytvořen nový trezor nebo toosee jaké trezory jsou k dispozici v rámci předplatného hello.

Spusťte příkaz hello **Get-AzureRmRecoveryServicesVault**, a jsou uvedeny všechny trezorů v předplatném hello.

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


## <a name="installing-hello-azure-backup-agent"></a>Instalace agenta Azure Backup hello
Před instalací agenta Azure Backup hello, musíte instalační program toohave hello stažené a na serveru Windows hello. Můžete získat nejnovější verzi instalačního programu hello hello hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) nebo z trezoru služeb zotavení hello stránka řídicího panelu. Uložení instalačního programu hello tooan snadno dostupné místo jako * C:\Downloads\*.

Můžete taky použijte programu hello tooget prostředí PowerShell:
 
 ```
 $MarsAURL = 'Http://Aka.Ms/Azurebackup_Agent'
 $WC = New-Object System.Net.WebClient
 $WC.DownloadFile($MarsAURL,'C:\downloads\MARSAgentInstaller.EXE')
 C:\Downloads\MARSAgentInstaller.EXE /q
 ```

tooinstall hello agenta, spusťte následující příkaz v konzolu prostředí PowerShell se zvýšenými oprávněními hello:

```
PS C:\> MARSAgentInstaller.exe /q
```

Tím se nainstaluje hello agent se všemi možnostmi výchozí hello. Hello instalace trvá několik minut v pozadí hello. Pokud nezadáte hello */nu* možnost pak hello **Windows Update** otevře se okno na konci hello hello toocheck instalace pro všechny aktualizace. Po instalaci agenta hello se zobrazí v seznamu nainstalovaných programů hello.

toosee hello seznamu nainstalovaných programů, přejděte příliš**ovládací panely** > **programy** > **programy a funkce**.

![Instalaci agenta](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a>Možnosti instalace
toosee, které všechny možnosti, které jsou k dispozici prostřednictvím hello hello příkazového řádku, použijte následující příkaz hello:

```
PS C:\> MARSAgentInstaller.exe /?
```

Hello dostupné možnosti patří:

| Možnost | Podrobnosti | Výchozí |
| --- | --- | --- |
| /q |Tichá instalace |- |
| / p: "umístění" |Cesta toohello instalační složku pro agenta Azure Backup hello. |C:\Program Files\Microsoft Azure Recovery Services agenta |
| / s: "umístění" |Složka mezipaměti toohello cestu pro agenta Azure Backup hello. |C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch |
| /m |Výslovný souhlas tooMicrosoft aktualizace |- |
| /nu |Nekontrolovat aktualizace po dokončení instalace |- |
| /d |Odinstaluje Agenta Microsoft Azure Recovery Services. |- |
| /pH |Adresa proxy hostitele |- |
| /Po |Číslo portu proxy hostitele |- |
| /Pu |Uživatelské jméno proxy hostitele |- |
| /pW |Heslo pro proxy server |- |

## <a name="registering-windows-server-or-windows-client-machine-tooa-recovery-services-vault"></a>Registrace systému Windows Server a Windows klientský počítač tooa trezoru služeb zotavení
Po vytvoření trezoru služeb zotavení hello stáhnout nejnovější verzi agenta hello a přihlašovací údaje trezoru hello a uložte ho do vhodného umístění jako C:\Downloads.

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
```

Na serveru Windows hello nebo klientský počítač systému Windows, spusťte hello [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) rutiny tooregister hello počítač s hello trezoru.
Tato a další rutiny používané pro zálohování, jsou z modulu MSONLINE hello které hello Mars AgentInstaller přidat jako součást procesu instalace hello. 

Instalační program agenta Hello neaktualizuje hello $Env: PSModulePath proměnné. To znamená, že automaticky načíst modul selže. tooresolve to můžete provést následující hello:

```
PS C:\>  $Env:psmodulepath += ';C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules
```

Alternativně můžete ručně načíst modul hello ve vašem skriptu následujícím způsobem:

```
PS C:\>  Import-Module  'C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup'

```

Po načtení hello Online zálohování rutiny zaregistrujete přihlašovací údaje trezoru hello:


```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :WestUS
Machine registration succeeded.
```

> [!IMPORTANT]
> Nepoužívejte soubor s relativní cesty toospecify hello přihlašovacími údaji. Musíte zadat absolutní cestu jako vstupní toohello rutiny.
>
>

## <a name="networking-settings"></a>Nastavení sítě
Při připojení hello hello Windows počítač toohello je Internetu prostřednictvím proxy serveru, nastavení proxy serveru hello se dá zadat i toohello agenta. V tomto příkladu neexistuje žádný proxy server, takže jsme jsou explicitně vymazání údaje související s proxy serverem.

Využití šířky pásma také lze kontrolovat pomocí možnosti hello ```work hour bandwidth``` a ```non-work hour bandwidth``` pro danou sadu dny v týdnu hello.

Nastavení serveru proxy a šířky pásma podrobnosti hello se provádí pomocí hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) rutiny:

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a>Nastavení šifrování
zálohování dat odesílaných tooAzure Hello zálohování je šifrovaný tooprotect hello důvěrnost dat hello. šifrovací přístupové heslo Hello jsou hello "password" toodecrypt hello data v době obnovení hello.

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
PS C:\> $PassPhrase = ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force 
PS C:\> $PassCode   = 'AzureR0ckx'
PS C:\> Set-OBMachineSetting -EncryptionPassPhrase $PassPhrase
Server properties updated successfully
```

> [!IMPORTANT]
> Zachování informací o přístupové heslo hello bezpečném, po nastavení. Nejsou-li být schopný toorestore dat z Azure bez tohoto hesla.
>
>

## <a name="back-up-files-and-folders"></a>Zálohování souborů a složek
Zásady se řídí všech záloh z Windows serverů a klientů tooAzure zálohování. Hello zásad se skládá ze tří částí:

1. A **plán zálohování** určující, kdy zálohování potřebovat toobe provést a synchronizovat se službou hello.
2. A **plán uchovávání informací** určující, jak dlouho body obnovení hello tooretain v Azure.
3. A **zadání zahrnutí nebo vyloučení souboru** který určuje, co by měl být zálohovány.

V tomto dokumentu protože jsme se automatizaci zálohování, budete předpokládáme, že nic nebyl nakonfigurován. Začneme vytvořením nové zásady zálohování pomocí hello [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) rutiny.

```
PS C:\> $newpolicy = New-OBPolicy
```

V tento čas hello je prázdné zásady a jiné rutiny jsou potřebné toodefine co položky budou zahrnout nebo vyloučit, při zálohování spustí a kde hello zálohy budou uloženy.

### <a name="configuring-hello-backup-schedule"></a>Konfigurace plánu zálohování hello
nejprve Hello hello 3 části zásady je plán zálohování hello, který je vytvořený pomocí hello [New-OBSchedule](https://technet.microsoft.com/library/hh770401) rutiny. plán zálohování Hello definuje při zálohování potřebovat toobe provedena. Při vytváření plánu je třeba toospecify 2 vstupní parametry:

* **Počet dnů v týdnu hello** zálohování hello měly být spuštěny. Můžete spustit úlohy zálohování hello pouze jeden den nebo každý den v týdnu hello nebo libovolnou kombinaci mezi nimi.
* **Hello denní dobu** spuštění zálohování hello. Můžete definovat až too3 různou dobu hello, když se spustí zálohování hello.

Například může nakonfigurovat zásady zálohování, který se spustí na 16: 00 každou sobotu a neděli.

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

plán zálohování Hello potřebuje toobe přidružené k zásadě a toho lze dosáhnout pomocí hello [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) rutiny.

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a>Konfigurace zásad uchovávání
zásady uchovávání informací Hello definuje, jak dlouho obnovení, které se uchovají body vytvořené z úlohy zálohování. Při vytváření nových zásad uchovávání informací pomocí hello [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) rutiny, můžete zadat třeba hello počet dní, které hello body obnovení záloh toobe uchovávají s Azure Backup. Následující příklad Hello nastaví zásady uchovávání informací 7 dní.

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

Hello zásady uchovávání informací musí být přidruženy k hlavní zásady hello pomocí rutiny hello [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):

```
PS C:\> Set-OBRetentionPolicy -Policy $newpolicy -RetentionPolicy $retentionpolicy

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          :
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```
### <a name="including-and-excluding-files-toobe-backed-up"></a>Zahrnutí a vyloučení souborů toobe zálohovat
```OBFileSpec``` Objekt definuje hello soubory toobe zahrnout a vyloučit zálohu. Toto je sada pravidel, že obor se hello chráněné soubory a složky na počítači. Můžete mít mnoho souboru pravidla zahrnutí nebo vyloučení podle potřeby a přidružovat je k zásadě. Při vytváření nového objektu OBFileSpec, můžete:

* Zadejte soubory a složky toobe hello zahrnuté
* Zadejte soubory a složky toobe hello vyloučené
* Zadejte rekurzivní zálohování dat v složku (nebo) zda pouze hello nejvyšší úrovně soubory v zadané složce hello by měl být zálohuje nahoru.

Hello pozdější se dosáhne použitím příznaku - nerekurzivní hello v příkazu New-OBFileSpec hello.

V příkladu hello níže jsme budete zálohování svazku C: a D: a vyloučit hello binární soubory operačního systému ve složce Windows hello a všechny dočasné složky. toodo, vytvoříme dvě specifikace souborů pomocí hello [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) rutiny – jeden pro zahrnutí a jeden pro vyloučení. Po vytvoření specifikace hello souborů jsou přidružené zásady hello použitím hello [přidat OBFileSpec](https://technet.microsoft.com/library/hh770424) rutiny.

```
PS C:\> $inclusions = New-OBFileSpec -FileSpec @("C:\", "D:\")

PS C:\> $exclusions = New-OBFileSpec -FileSpec @("C:\windows", "C:\temp") -Exclude

PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $inclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid


PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $exclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\windows
                  IsExclude:True
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\temp
                  IsExclude:True
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

### <a name="applying-hello-policy"></a>Použití zásad hello
Teď objektu zásad hello dokončení a má přidružené plánu zálohování, zásady uchovávání informací a seznam souborů zahrnutí nebo vyloučení. Tato zásada může být nyní potvrdit pro toouse Azure Backup. Před použitím hello nově vytvořený zásad zajistěte, aby žádné existující zásady zálohování, které jsou přidružené k serveru hello pomocí hello [odebrat OBPolicy](https://technet.microsoft.com/library/hh770415) rutiny. Odebírání hello zásady zobrazí výzvu k potvrzení. potvrzení hello tooskip použít hello ```-Confirm:$false``` příznak pomocí rutiny hello.

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want tooremove this backup policy? This will delete all hello backed up data. [Y] Yes [A] Yes tooAll [N] No [L] No tooAll [S] Suspend [?] Help (default is "Y"):
```

Objekt zásad spouštění hello se provádí pomocí hello [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) rutiny. Také to požádá o potvrzení. potvrzení hello tooskip použít hello ```-Confirm:$false``` příznak pomocí rutiny hello.

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want toosave this backup policy ? [Y] Yes [A] Yes tooAll [N] No [L] No tooAll [S] Suspend [?] Help (default is "Y"):
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s)
DsList : {DataSource
         DatasourceId:4508156004108672185
         Name:C:\
         FileSpec:FileSpec
         FileSpec:C:\
         IsExclude:False
         IsRecursive:True,

         FileSpec
         FileSpec:C:\windows
         IsExclude:True
         IsRecursive:True,

         FileSpec
         FileSpec:C:\temp
         IsExclude:True
         IsRecursive:True,

         DataSource
         DatasourceId:4508156005178868542
         Name:D:\
         FileSpec:FileSpec
         FileSpec:D:\
         IsExclude:False
         IsRecursive:True
    }
PolicyName : c2eb6568-8a06-49f4-a20e-3019ae411bac
RetentionPolicy : Retention Days : 7
              WeeklyLTRSchedule :
              Weekly schedule is not set

              MonthlyLTRSchedule :
              Monthly schedule is not set

              YearlyLTRSchedule :
              Yearly schedule is not set
State : Existing PolicyState : Valid
```

Můžete zobrazit podrobnosti o hello hello existující zásady zálohování pomocí hello [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) rutiny. Vám může procházení další pomocí hello [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) rutiny pro plán zálohování hello a hello [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) rutina pro zásady uchovávání informací hello

```
PS C:> Get-OBPolicy | Get-OBSchedule
SchedulePolicyName : 71944081-9950-4f7e-841d-32f0a0a1359a
ScheduleRunDays : {Saturday, Sunday}
ScheduleRunTimes : {16:00:00}
State : Existing

PS C:> Get-OBPolicy | Get-OBRetentionPolicy
RetentionDays : 7
RetentionPolicyName : ca3574ec-8331-46fd-a605-c01743a5265e
State : Existing

PS C:> Get-OBPolicy | Get-OBFileSpec
FileName : *
FilePath : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
FileSpec : D:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\
FileSpec : C:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\windows
FileSpec : C:\windows
IsExclude : True
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\temp
FileSpec : C:\temp
IsExclude : True
IsRecursive : True
```

### <a name="performing-an-ad-hoc-backup"></a>Provádění zálohu ad-hoc
Jakmile byla nastavena zásada zálohování hello zálohy dojde za hello plán. Spuštění služby ad-hoc zálohy je také možné pomocí hello [Start-OBBackup](https://technet.microsoft.com/library/hh770426) rutiny:

```
PS C:> Get-OBPolicy | Start-OBBackup
Initializing
Taking snapshot of volumes...
Preparing storage...
Generating backup metadata information and preparing hello metadata VHD...
Data transfer is in progress. It might take longer since it is hello first backup and all data needs toobe transferred...
Data transfer completed and all backed up data is in hello cloud. Verifying data integrity...
Data transfer completed
In progress...
Job completed.
hello backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a>Obnovení dat z Azure Backup
Tato část vás provede kroky hello pro automatizaci obnovení dat z Azure Backup. To zahrnuje tak hello následující kroky:

1. Vyberte zdrojový svazek hello
2. Vyberte zálohování toorestore bodu
3. Zvolte toorestore položky
4. Proces obnovení hello aktivační události

### <a name="picking-hello-source-volume"></a>Výběr hello zdrojový svazek
V pořadí toorestore položku z Azure Backup je nutné nejprve tooidentify hello zdroj položky hello. Vzhledem k tomu, že jsme se provádění příkazů hello v kontextu hello systému Windows Server nebo klienta Windows, hello počítač je už definovaný. dalším krokem Hello při identifikaci zdroje hello je tooidentify hello svazku, který ji obsahuje. Seznam svazků nebo zdrojů zálohovaných z tohoto počítače se dá načíst spuštěním hello [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) rutiny. Tento příkaz vrátí pole všech zdrojů hello zálohovat z tohoto serveru nebo klienta.

```
PS C:> $source = Get-OBRecoverableSource
PS C:> $source
FriendlyName : C:\
RecoverySourceName : C:\
ServerName : myserver.microsoft.com

FriendlyName : D:\
RecoverySourceName : D:\
ServerName : myserver.microsoft.com
```

### <a name="choosing-a-backup-point-from-which-toorestore"></a>Výběr bodu zálohy z které toorestore
Načíst seznam body zálohy spuštěním hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) rutiny s příslušnými parametry. V našem příkladu jsme vybrali hello nejnovější bod zálohy pro zdrojový svazek hello *D:* a použít ho toorecover konkrétní soubor.

```
PS C:> $rps = Get-OBRecoverableItem -Source $source[1]
IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :

IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 17-Jun-15 6:31:31 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :
```
objekt Hello ```$rps``` je pole body zálohy. první prvek Hello je poslední bod hello a n-tou element hello je nejstarší bod hello. toochoose hello posledního bodu, použijeme ```$rps[0]```.

### <a name="choosing-an-item-toorestore"></a>Výběr toorestore položky
tooidentify hello přesný soubor nebo složku toorestore, rekurzivně použít hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) rutiny. Hierarchie složky hello tento způsob, jak lze procházet výhradně pomocí hello ```Get-OBRecoverableItem```.

V tomto příkladu, pokud chceme souboru hello toorestore *finances.xls* jsme můžete odkazovat pomocí tohoto objektu hello ```$filesFolders[1]```.

```
PS C:> $filesFolders = Get-OBRecoverableItem $rps[0]
PS C:> $filesFolders
IsDir : True
ItemNameFriendly : D:\MyData\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\
LocalMountPoint : D:\
MountPointName : D:\
Name : MyData
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime : 15-Jun-15 8:49:29 AM

PS C:> $filesFolders = Get-OBRecoverableItem $filesFolders[0]
PS C:> $filesFolders
IsDir : False
ItemNameFriendly : D:\MyData\screenshot.oxps
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\screenshot.oxps
LocalMountPoint : D:\
MountPointName : D:\
Name : screenshot.oxps
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 228313
ItemLastModifiedTime : 21-Jun-14 6:45:09 AM

IsDir : False
ItemNameFriendly : D:\MyData\finances.xls
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\finances.xls
LocalMountPoint : D:\
MountPointName : D:\
Name : finances.xls
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 96256
ItemLastModifiedTime : 21-Jun-14 6:43:02 AM
```

Můžete také vyhledat položky toorestore pomocí hello ```Get-OBRecoverableItem``` rutiny. V našem příkladu toosearch pro *finances.xls* nám může získat popisovač souboru hello spuštěním tohoto příkazu:

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-hello-restore-process"></a>Spuštění procesu obnovení hello
proces obnovení hello tootrigger, potřebujeme nejprve možnosti obnovení toospecify hello. Tento krok můžete provést pomocí hello [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) rutiny. Pro tento příklad předpokládejme, že chceme, aby soubory hello toorestore příliš*C:\temp*. Také Předpokládejme, že má být tooskip soubory, které již existují na cílovou složku hello *C:\temp*. toocreate takové možnost obnovení pomocí hello následující příkaz:

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

Teď spustit proces obnovení hello pomocí hello [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) příkaz na hello vybrané ```$item``` z hello výstup hello ```Get-OBRecoverableItem``` rutiny:

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
hello recovery operation completed successfully.
```


## <a name="uninstalling-hello-azure-backup-agent"></a>Odinstalace agenta Azure Backup hello
Odinstalaci agenta Azure Backup hello lze provést pomocí hello následující příkaz:

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

Odinstalace binárních souborů hello agenta z počítače hello má některé tooconsider důsledky:

* Odebere z počítače hello soubor filtru hello a sledování změn je zastavena.
* Všechny informace o zásadách se odebere z počítače hello, ale informace o zásadách hello pokračuje toobe uložené ve službě hello.
* Se odeberou všechny plány zálohování a jsou provedeny žádné další zálohy.

Ale hello data uložená v Azure zůstane a zůstane podle nastavení zásad uchovávání hello vy. Starší body jsou automaticky stará.

## <a name="remote-management"></a>Vzdálená správa
Veškerá Správa hello kolem hello agenta Azure Backup, zásady a zdroje dat lze provést vzdáleně pomocí prostředí PowerShell. Hello počítač, který bude spravovat vzdáleně musí toobe připravený správně.

Ve výchozím nastavení je Vzdálená správa systému Windows hello nakonfigurovaná pro ruční spuštění. musí být nastaven typ spouštění Hello příliš*automatické* a musí být spuštěna služba hello. tooverify, který hello Služba WinRM je spuštěná, by měly být hello hodnotu hello Vlastnost Status *systémem*.

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

Prostředí PowerShell by měl být nakonfigurovaný pro vzdálenou komunikaci.

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up tooreceive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

počítač Hello teď můžete spravovat vzdáleně – od hello instalace agenta hello. Například následující skript hello zkopíruje hello agenta toohello vzdáleného počítače a ho nainstaluje.

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a>Další kroky
Další informace o Azure Backup pro Windows Server nebo klienta najdete v tématu

* [Úvod tooAzure zálohování](backup-introduction-to-azure-backup.md)
* [Zálohování serverů Windows](backup-configure-vault.md)
