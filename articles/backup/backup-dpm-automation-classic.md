---
title: "Zálohování Azure: Použijte PowerShell tooback až úloh DPM | Microsoft Docs"
description: "Zjistěte, jak toodeploy a spravovat Azure Backup pro Data Protection Manager (DPM) pomocí prostředí PowerShell"
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
ms.assetid: bcbcef79-9d33-4e84-a558-9866614f2cae
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: nkolli;trinadhk;anuragm;markgal
ms.openlocfilehash: 48ebe6b520857836e89749ffb6fe83d1f14c5597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-data-protection-manager-dpm-servers-using-powershell"></a>Nasazení a správě zálohování tooAzure pro Data Protection Manager (DPM) servery pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [ARM](backup-dpm-automation.md)
> * [Classic](backup-dpm-automation-classic.md)
>
>

Tento článek vysvětluje, jak toouse prostředí PowerShell tooback až a obnovení dat aplikace DPM z úložiště záloh. Společnost Microsoft doporučuje používat trezory služeb zotavení pro všechna nová nasazení. Pokud jste nového uživatele Azure Backup, pomocí článku hello [nasadit a spravovat tooAzure data Data Protection Manager pomocí prostředí PowerShell](backup-dpm-automation.md), takže data ukládáte do trezoru služeb zotavení.

> [!IMPORTANT]
> Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup. Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md). Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb. Po 15 říjen 2017 nemůžete použít trezory Backup toocreate prostředí PowerShell. **Do 1. listopadu 2017:**
>- Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.
>- Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello. Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.
>

## <a name="setting-up-hello-powershell-environment"></a>Nastavení prostředí PowerShell hello
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Než budete moct použít PowerShell toomanage záloh z tooAzure Data Protection Manager, budete potřebovat toohave hello správné prostředí v prostředí PowerShell. Při spuštění hello relace prostředí PowerShell text hello Ujistěte se, spusťte následující příkaz tooimport hello správné moduly hello a umožňují rutiny DPM hello toocorrectly odkaz:

```
PS C:> & "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"

Welcome toohello DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a>Instalace a registrace
toobegin:

1. [Stáhněte si nejnovější PowerShell](https://github.com/Azure/azure-powershell/releases) (minimální požadovaná verze je: 1.0.0)
2. Povolit rutiny Azure Backup hello přepnutím příliš*AzureResourceManager* režimu pomocí hello **Switch-AzureMode** příkaz:

```
PS C:\> Switch-AzureMode AzureResourceManager
```

Hello následující nastavení a registrace úlohy je možné automatizovat pomocí prostředí PowerShell:

* Vytvoření trezoru záloh
* Instalace agenta Azure Backup hello
* Registrace hello služby zálohování Azure
* Nastavení sítě
* Nastavení šifrování

### <a name="create-a-backup-vault"></a>Vytvoření trezoru záloh
> [!WARNING]
> Pro zákazníky používající Azure Backup pro hello poprvé budete potřebovat tooregister hello Azure Backup zprostředkovatele toobe používat s vaším předplatným. To lze provést tak, že spustíte následující příkaz hello: Register-AzureProvider - ProviderNamespace "Microsoft.Backup"
>
>

Můžete vytvořit nové úložiště záloh pomocí hello **New-AzureRMBackupVault** příkaz. Trezor záloh Hello je prostředek ARM, proto musíte tooplace ji ve skupině prostředků. V prostředí Azure PowerShell konzolu se zvýšenými oprávněními spusťte následující příkazy hello:

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

Můžete získat seznam všech hello záloh v daném předplatném pomocí hello **Get-AzureRMBackupVault** příkaz.

### <a name="installing-hello-azure-backup-agent-on-a-dpm-server"></a>Instalace agenta Azure Backup hello na serveru aplikace DPM
Před instalací agenta Azure Backup hello, musíte instalační program toohave hello stažené a na serveru Windows hello. Můžete získat nejnovější verzi instalačního programu hello hello hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) nebo ze stránky řídicí panel hello úložiště záloh. Uložení instalačního programu hello tooan snadno dostupné místo jako * C:\Downloads\*.

tooinstall hello agenta, spusťte následující příkaz v konzolu prostředí PowerShell se zvýšenými oprávněními hello **na serveru DPM hello**:

```
PS C:\> MARSAgentInstaller.exe /q
```

Tím se nainstaluje hello agent se všemi možnostmi výchozí hello. Hello instalace trvá několik minut v pozadí hello. Pokud nezadáte hello */nu* možnost hello **Windows Update** otevře se okno na konci hello hello toocheck instalace pro všechny aktualizace.

Hello agent se zobrazí v seznamu nainstalovaných programů hello. toosee hello seznamu nainstalovaných programů, přejděte příliš**ovládací panely** > **programy** > **programy a funkce**.

![Instalaci agenta](./media/backup-dpm-automation/installed-agent-listing.png)

#### <a name="installation-options"></a>Možnosti instalace
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

### <a name="registering-with-hello-azure-backup-service"></a>Registrace hello služby zálohování Azure
Než můžete zaregistrovat s hello služby zálohování Azure, je třeba tooensure této hello [požadavky](backup-azure-dpm-introduction.md) jsou splněny. Postupujte takto:

* Máte platné předplatné Azure
* Mít úložiště záloh

toodownload hello přihlašovací údaje trezoru, spusťte hello **Get-AzureBackupVaultCredentials** příkaz do konzoly Azure PowerShell a úložiště do vhodného umístění, například * C:\Downloads\*.

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

Registrace počítače hello hello trezoru se provádí pomocí hello [Start-DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) rutiny:

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-DPMCloudRegistration -DPMServerName "TestingServer" -VaultCredentialsFilePath $cred
```

To bude zaregistrovat hello serveru aplikace DPM s názvem "Testovacím" úložišti Microsoft Azure pomocí hello zadané přihlašovací údaje trezoru.

> [!IMPORTANT]
> Nepoužívejte soubor s relativní cesty toospecify hello přihlašovacími údaji. Musíte zadat absolutní cestu jako vstupní toohello rutiny.
>
>

### <a name="initial-configuration-settings"></a>Počáteční konfigurace nastavení
Jakmile hello je DPM Server registrován s úložištěm záloh Azure hello, se spustí s výchozím nastavením odběru. Tato nastavení odběru zahrnují sítě, šifrování a hello pracovní oblasti. toobegin změna hello nastavení odběru je třeba toofirst získat popisovač pro stávající nastavení (výchozí) hello pomocí hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) rutiny:

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

Všechny změny jsou učiněna toothis místní prostředí PowerShell objekt ```$setting``` a pak je úplná objekt hello potvrdit tooDPM a toosave Azure Backup je pomocí hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny. Je třeba toouse hello ```–Commit``` tooensure příznak, který hello změny jsou nastavené jako trvalé. Hello nastavení nebude použita a pokud potvrzené používá Azure Backup.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

### <a name="networking"></a>Sítě
Pokud hello připojení hello DPM počítač toohello služby Azure Backup na hello Internetu prostřednictvím proxy serveru, by měl pro zálohování toosucceed zadat hello nastavení proxy serveru. To se provádí pomocí hello ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` a hello ```ProxyPassword``` parametry s hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny. V tomto příkladu není žádný proxy server, jsme jsou explicitně vymazání údaje související s proxy serverem.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

Využití šířky pásma se dá taky nastavit pomocí možnosti ```-WorkHourBandwidth``` a ```-NonWorkHourBandwidth``` pro danou sadu dny v týdnu hello. V tomto příkladu jsme nejsou nastavení žádné omezení.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

### <a name="configuring-hello-staging-area"></a>Konfigurace hello pracovní oblasti
agent Azure Backup Hello spuštěné na serveru DPM hello potřebuje dočasné úložiště pro data obnovená z cloudu hello (místní pracovní oblasti). Konfigurovat pracovní oblasti hello pomocí hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny a hello ```-StagingAreaPath``` parametr.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

V předchozím příkladu hello, hello pracovní oblasti se nastaví příliš*C:\StagingArea* v objektu prostředí PowerShell hello ```$setting```. Ujistěte se, že hello zadaná složka již existuje, jinak hello konečné zápisu nastavení odběru hello nezdaří.

### <a name="encryption-settings"></a>Nastavení šifrování
zálohování dat odesílaných tooAzure Hello zálohování je šifrovaný tooprotect hello důvěrnost dat hello. šifrovací přístupové heslo Hello jsou hello "password" toodecrypt hello data v době obnovení hello. Je důležité tookeep bezpečné této informace a zabezpečení po nastavení.

V příkladu hello níže, převede první příkaz hello hello řetězec ```passphrase123456789``` tooa zabezpečený řetězec a přiřadí hello zabezpečený řetězec toohello proměnné s názvem ```$Passphrase```. druhý příkaz Hello nastaví hello zabezpečený řetězec ```$Passphrase``` jako hello heslo pro šifrování záloh.

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> Zachování informací o přístupové heslo hello bezpečném, po nastavení. Nebudete moct toorestore dat z Azure bez tohoto hesla.
>
>

V tomto okamžiku by měl provedení všech hello požadované změny toohello ```$setting``` objektu. Mějte na paměti, toocommit hello změny.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-tooazure-backup"></a>Ochrana dat tooAzure zálohování
V této části přidáte tooDPM produkčním serveru a poté proveďte ochranu úložiště DPM toolocal hello data a pak tooAzure zálohování. V příkladech hello si předvedeme jak tooback soubory a složky. Hello logiky můžete snadno být rozšířené toobackup libovolný zdroj dat aplikace DPM podporováno. Všechny zálohy aplikace DPM se řídí pomocí ochranu skupiny (PG) s čtyři části:

1. **Členy skupiny** je seznam všech hello chránitelné objekty (také označované jako *zdrojů dat* v DPM), které chcete tooprotect v hello stejné skupiny ochrany. Můžete například tooprotect produkční virtuálních počítačů na jednu skupinu ochrany a databáze systému SQL Server v jiné skupině ochrany mají různé požadavky na zálohování. Než můžete zálohovat žádný zdroj dat na provozním serveru je třeba jestli hello toomake agenta DPM na hello server je nainstalovaná a je spravovaných aplikací DPM. Postupujte podle kroků hello pro [instalaci agenta aplikace DPM hello](https://technet.microsoft.com/library/bb870935.aspx) a jejím propojením toohello vhodné serveru aplikace DPM.
2. **Způsob ochrany dat** určuje hello cíl zálohování umístění - páska, disk a cloud. V našem příkladu jsme bude chránit dat toohello místní disk a toohello v cloudu.
3. A **plán zálohování** určující při zálohování potřebovat toobe prováděné a jak často hello se mají synchronizovat data mezi hello serveru DPM a hello provozním serveru.
4. A **plán uchovávání informací** určující, jak dlouho body obnovení hello tooretain v Azure.

### <a name="creating-a-protection-group"></a>Vytvoření skupiny ochrany
Začněte vytvořením nové skupiny ochrany pomocí hello [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) rutiny.

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

Hello výše rutina vytvoří skupiny ochrany s názvem *ProtectGroup01*. Existující skupiny ochrany můžete také upravit novější tooadd zálohování toohello cloudu Azure. Ale toomake změny toohello skupiny ochrany – nové nebo existující - potřebujeme tooget popisovač na *upravitelnými* objekt, který používá hello [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) rutiny.

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-toohello-protection-group"></a>Přidání skupiny toohello členy skupiny ochrany
Každý Agent aplikace DPM zná hello seznam zdrojů dat na hello serveru, který je nainstalovaný. tooadd toohello zdroj dat skupiny ochrany, hello agenta aplikace DPM musí toofirst odeslání seznamu hello zdrojů dat zpět toohello DPM server. Jeden nebo více zdrojů dat jsou pak vybrali a přidat toohello skupiny ochrany. Hello prostředí PowerShell kroky potřebné tooget dosáhnout tyto:

1. Získat seznam všech serverů spravovaných aplikací DPM prostřednictvím hello agenta aplikace DPM.
2. Vyberte konkrétní server.
3. Získat seznam všech zdrojů dat na serveru hello.
4. Vyberte jeden nebo více zdrojů dat a přidat je toohello skupiny ochrany

Hello seznam serverů, na které hello agenta aplikace DPM je nainstalovaná a je spravován systémem hello serveru aplikace DPM je získán pomocí hello [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) rutiny. V tomto příkladu jsme bude filtrovat a konfigurovat jenom PS s názvem *productionserver01* pro zálohování.

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

Nyní načíst hello seznam zdrojů dat v ```$server``` pomocí hello [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) rutiny. V tomto příkladu jsme pro svazek hello filtrování * D:\* který chceme tooconfigure pro zálohování. Tento zdroj dat se pak přidá toohello skupiny ochrany pomocí hello [přidat DPMChildDatasource](https://technet.microsoft.com/library/hh881732) rutiny. Mějte na paměti, toouse hello *modifable* objektu skupiny ochrany ```$MPG``` dodatky toomake hello.

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

Opakujte tento krok tolikrát, kolikrát podle potřeby, dokud nepřidáte všechny hello zvolené skupiny zdrojů dat toohello ochrany. Můžete začít s jenom jeden zdroj dat a dokončení hello pracovní postup pro vytváření hello skupiny ochrany a později přidat další zdroje dat toohello skupiny ochrany.

### <a name="selecting-hello-data-protection-method"></a>Vyberte způsob ochrany dat hello
Jakmile hello zdroje dat byly přidány toohello skupiny ochrany, hello dalším krokem je způsob ochrany hello toospecify pomocí hello [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) rutiny. V tomto příkladu hello skupiny ochrany bude instalační program pro místního disku a zálohování. Musíte taky toospecify hello zdroj dat, které chcete pomocí hello toocloud tooprotect [přidat DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) rutiny příznakem - Online.

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-hello-retention-range"></a>Nastavení rozsah uchování hello
Nastavit hello uchování pro body zálohy hello pomocí hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) rutiny. Při uchování hello liché tooset může zdát, než byla definována hello plán zálohování, pomocí hello ```Set-DPMPolicyObjective``` rutiny automaticky nastaví výchozí plán zálohování, který je pak možné upravit. Je možné tooset plán zálohování hello vždy nejprve a hello zásady uchovávání informací po.

V příkladu hello níže hello rutiny nastaví parametry uchování hello zálohování na disk. To bude uchování záloh pro 10 dnů a synchronizaci dat každých 6 hodin mezi hello provozním serveru a server aplikace DPM hello. Hello ```SynchronizationFrequencyMinutes``` nedefinuje četnost zálohování je bod vytvořen, ale jak často dat je server DPM zkopírovaný toohello; záloh zabrání příliš velká.

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

Pro zálohování budete tooAzure (aplikace DPM se týká toothese jako zálohování Online) rozsahy uchovávání hello lze nakonfigurovat pro [dlouhodobé uchovávání používá schéma Dědečka. otec SYN (GFS)](backup-azure-backup-cloud-as-tape.md). To znamená můžete definovat zásady uchovávání informací kombinované zahrnující denní, týdenní, měsíční a roční zásady uchovávání informací. V tomto příkladu vytvoření představující schéma hello komplexní uchovávání, který má být pole a pak nakonfigurujte rozsah uchování hello pomocí hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) rutiny.

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-hello-backup-schedule"></a>Plán zálohování sady hello
Aplikace DPM nastaví výchozí plán zálohování automaticky, pokud zadáte hello cíl ochrany pomocí hello ```Set-DPMPolicyObjective``` rutiny. toochange hello výchozích plánů, použijte hello [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) následovanou hello [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) rutiny.

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

Ve výše uvedeném příkladu hello ```$onlineSch``` je pole se čtyřmi prvky, které obsahuje hello existující plán ochranu online pro hello skupiny ochrany ve schématu GFS hello:

1. ```$onlineSch[0]```bude obsahovat denní plán hello
2. ```$onlineSch[1]```bude obsahovat týdenní plán hello
3. ```$onlineSch[2]```bude obsahovat plánování měsíčně hello
4. ```$onlineSch[3]```bude obsahovat roční plán hello

Takže pokud budete potřebovat toomodify hello týdenní plán, je třeba toorefer toohello ```$onlineSch[1]```.

### <a name="initial-backup"></a>Prvotní zálohování
Při zálohování zdroje dat pro hello poprvé, musí aplikace DPM toocreate počáteční repliky, která vytvoří kopii toobe hello zdroje dat chráněné na svazku repliky DPM. Tato aktivita bude naplánována buď pro určitý čas nebo lze spustit ručně, pomocí hello [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) rutiny s parametrem hello ```-NOW```.

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-hello-size-of-dpm-replica--recovery-point-volume"></a>Změna velikosti hello repliky aplikace DPM a svazek bodu obnovení
Můžete také změnit hello velikost svazku repliky DPM, jakož i stínové kopie svazku pomocí [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) rutiny jako hello následujícím příkladu: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation - Datasource $DS - ProtectionGroup $MPG – ruční - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)

### <a name="committing-hello-changes-toohello-protection-group"></a>Potvrzení hello změny toohello skupiny ochrany
Nakonec hello změny nutné toobe potvrzené předtím, než aplikace DPM může provést zálohování hello za hello novou konfiguraci skupiny ochrany. To se provádí pomocí hello [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) rutiny.

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-hello-backup-points"></a>Body zálohy hello zobrazení
Můžete použít hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) rutiny tooget seznam všech bodů obnovení pro zdroj dat. V tomto příkladu provedeme následující:

* načtení všech hello PGs na serveru DPM hello, které se budou ukládat do pole```$PG```
* získat odpovídající toohello hello zdrojů dat```$PG[0]```
* získání všech hello bodů obnovení pro zdroj dat.

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a>Obnovit data chráněná v Azure
Obnovení dat je kombinací ```RecoverableItem``` objektu a ```RecoveryOption``` objektu. V předchozí části hello My seznam hello body zálohy pro zdroj dat.

V příkladu hello dole ukážeme, jak toorestore Hyper-V virtuální počítač z Azure Backup kombinování zálohování body s hello cíle pro obnovení. To zahrnuje následující:

* Vytváření možnost obnovení pomocí hello [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) rutiny.
* Načítání pole hello body zálohy pomocí hello ```Get-DPMRecoveryPoint``` rutiny.
* Výběr zálohování toorestore bodu z.

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

pro jakýkoli typ zdroje dat lze snadno rozšířit Hello příkazy.

## <a name="next-steps"></a>Další kroky
* Další informace o zálohování Azure pro aplikaci DPM najdete v části [Úvod tooDPM zálohování](backup-azure-dpm-introduction.md)
