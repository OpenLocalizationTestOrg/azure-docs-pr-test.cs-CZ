---
title: "aaaAzure zálohování - použijte PowerShell tooback až úloh DPM | Microsoft Docs"
description: "Zjistěte, jak toodeploy a spravovat Azure Backup pro Data Protection Manager (DPM) pomocí prostředí PowerShell"
services: backup
documentationcenter: 
author: NKolli1
manager: shreeshd
editor: 
ms.assetid: e9bd223c-2398-4eb1-9bf3-50e08970fea7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/23/2017
ms.author: adigan;anuragm;trinadhk;markgal
ms.openlocfilehash: 27d2b4b3127b68c9da564697eb61dc3ccbc34b3d
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

Tento článek ukazuje, jak toouse prostředí PowerShell toosetup Azure Backup na serveru aplikace DPM a toomanage zálohování a obnovení.

## <a name="setting-up-hello-powershell-environment"></a>Nastavení prostředí PowerShell hello
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Než budete moci použít PowerShell toomanage záloh z tooAzure Data Protection Manager, je třeba toohave hello správné prostředí v prostředí PowerShell. Při spuštění hello relace prostředí PowerShell text hello Ujistěte se, spusťte následující příkaz tooimport hello správné moduly hello a umožňují rutiny DPM hello toocorrectly odkaz:

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

* Vytvoření trezoru Služeb zotavení
* Instalace agenta Azure Backup hello
* Registrace hello služby zálohování Azure
* Nastavení sítě
* Nastavení šifrování

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru služby Recovery Services
Hello následující kroky vás provedou vytvoření trezoru služeb zotavení. Trezor služeb zotavení se liší od úložiště záloh.

1. Pokud používáte Azure Backup pro hello poprvé, musíte použít hello **Register-AzureRMResourceProvider** rutiny tooregister hello službu Azure Recovery zprostředkovatele s vaším předplatným.

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. Hello trezor služeb zotavení je ARM prostředek, takže je třeba tooplace ji ve skupině prostředků. Můžete použít existující skupinu prostředků nebo vytvořte novou. Když vytváříte novou skupinu prostředků, zadejte hello název a umístění pro skupinu prostředků hello.  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. Použití hello **New-AzureRmRecoveryServicesVault** rutiny toocreate nový trezor. Ujistěte se, toospecify hello stejné umístění pro hello trezoru, protože byl použit pro skupinu prostředků hello.

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
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

Spusťte příkaz hello, Get-AzureRmRecoveryServicesVault, a jsou uvedeny všechny trezorů v předplatném hello.

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


## <a name="installing-hello-azure-backup-agent-on-a-dpm-server"></a>Instalace agenta Azure Backup hello na serveru aplikace DPM
Před instalací agenta Azure Backup hello, musíte instalační program toohave hello stažené a na serveru Windows hello. Můžete získat nejnovější verzi instalačního programu hello hello hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) nebo z trezoru služeb zotavení hello stránka řídicího panelu. Uložení instalačního programu hello tooan snadno dostupné místo jako * C:\Downloads\*.

tooinstall hello agenta, spusťte následující příkaz v konzolu prostředí PowerShell se zvýšenými oprávněními hello **na serveru DPM hello**:

```
PS C:\> MARSAgentInstaller.exe /q
```

Tím se nainstaluje hello agent se všemi možnostmi výchozí hello. Hello instalace trvá několik minut v pozadí hello. Pokud nezadáte hello */nu* možnost hello **Windows Update** otevře se okno na konci hello hello toocheck instalace pro všechny aktualizace.

Hello agent se zobrazí v seznamu nainstalovaných programů hello. toosee hello seznamu nainstalovaných programů, přejděte příliš**ovládací panely** > **programy** > **programy a funkce**.

![Instalaci agenta](./media/backup-dpm-automation/installed-agent-listing.png)

### <a name="installation-options"></a>Možnosti instalace
toosee všechny možnosti hello k dispozici prostřednictvím příkazového řádku hello, hello použijte následující příkaz:

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

## <a name="registering-dpm-tooa-recovery-services-vault"></a>Registrace aplikace DPM tooa trezoru služeb zotavení
Po vytvoření trezoru služeb zotavení hello stáhnout nejnovější verzi agenta hello a přihlašovací údaje trezoru hello a uložte ho do vhodného umístění jako C:\Downloads.

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
PS C:\> $credsfilename
C:\downloads\testvault\_Sun Apr 10 2016.VaultCredentials
```

Na serveru aplikace DPM hello spusťte hello [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) rutiny tooregister hello počítač s hello trezoru.

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :West US
Machine registration succeeded.
```

### <a name="initial-configuration-settings"></a>Počáteční konfigurace nastavení
Jakmile hello serveru aplikace DPM není zaregistrována hello trezor služeb zotavení, začne s výchozím nastavením odběru. Tato nastavení odběru zahrnují sítě, šifrování a hello pracovní oblasti. nastavení odběru toochange potřebujete toofirst získat popisovač pro stávající nastavení (výchozí) hello pomocí hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) rutiny:

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

Všechny změny jsou učiněna toothis místní prostředí PowerShell objekt ```$setting``` a pak je úplná objekt hello potvrdit tooDPM a toosave Azure Backup je pomocí hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny. Je třeba toouse hello ```–Commit``` tooensure příznak, který hello změny jsou nastavené jako trvalé. Hello nastavení nebude použita a pokud potvrzené používá Azure Backup.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="networking"></a>Sítě
Pokud připojení hello hello DPM počítač toohello služby Azure Backup na hello internet přes proxy server, měli nastavení proxy serveru hello zadat pro úspěšné zálohy. To se provádí pomocí hello ```-ProxyServer```a ```-ProxyPort```, ```-ProxyUsername``` a hello ```ProxyPassword``` parametry s hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny. V tomto příkladu není žádný proxy server, jsme jsou explicitně vymazání údaje související s proxy serverem.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

Využití šířky pásma se dá taky nastavit pomocí možnosti ```-WorkHourBandwidth``` a ```-NonWorkHourBandwidth``` pro danou sadu dny v týdnu hello. V tomto příkladu jsme nejsou nastavení žádné omezení.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

## <a name="configuring-hello-staging-area"></a>Konfigurace hello pracovní oblasti
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
Každý Agent aplikace DPM zná hello seznam zdrojů dat na hello serveru, který je nainstalovaný. tooadd toohello zdroj dat skupiny ochrany, hello agenta aplikace DPM musí toofirst odeslání seznamu hello zdrojů dat zpět toohello DPM server. Jeden nebo více zdrojů dat jsou pak vybrali a přidat toohello skupiny ochrany. kroky PowerShell Hello potřeba tooachieve to jsou:

1. Získat seznam všech serverů spravovaných aplikací DPM prostřednictvím hello agenta aplikace DPM.
2. Vyberte konkrétní server.
3. Získat seznam všech zdrojů dat na serveru hello.
4. Vyberte jeden nebo více zdrojů dat a přidat je toohello skupiny ochrany

Hello seznam serverů, na které hello agenta aplikace DPM je nainstalovaná a je spravován systémem hello serveru aplikace DPM je získán pomocí hello [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) rutiny. V tomto příkladu jsme bude filtrovat a konfigurovat jenom PS s názvem *productionserver01* pro zálohování.

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

Nyní načíst hello seznam zdrojů dat v ```$server``` pomocí hello [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) rutiny. V tomto příkladu jsme pro svazek hello filtrování * D:\* chceme tooconfigure pro zálohování. Tento zdroj dat se pak přidá toohello skupiny ochrany pomocí hello [přidat DPMChildDatasource](https://technet.microsoft.com/library/hh881732) rutiny. Mějte na paměti, toouse hello *upravitelnými* objektu skupiny ochrany ```$MPG``` dodatky toomake hello.

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

Opakujte tento krok tolikrát, kolikrát podle potřeby, dokud nepřidáte všechny hello zvolené skupiny zdrojů dat toohello ochrany. Můžete začít s jenom jeden zdroj dat a dokončení hello pracovní postup pro vytváření hello skupiny ochrany a později přidat další zdroje dat toohello skupiny ochrany.

### <a name="selecting-hello-data-protection-method"></a>Vyberte způsob ochrany dat hello
Jakmile hello zdroje dat byly přidány toohello skupiny ochrany, hello dalším krokem je způsob ochrany hello toospecify pomocí hello [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) rutiny. V tomto příkladu hello skupiny ochrany je nastaven pro místního disku a zálohování. Musíte taky toospecify hello zdroj dat, které chcete pomocí hello toocloud tooprotect [přidat DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) rutiny příznakem - Online.

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-hello-retention-range"></a>Nastavení rozsah uchování hello
Nastavit hello uchování pro body zálohy hello pomocí hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) rutiny. Při uchování hello liché tooset může zdát, než byla definována hello plán zálohování, pomocí hello ```Set-DPMPolicyObjective``` rutiny automaticky nastaví výchozí plán zálohování, který je pak možné upravit. Je možné tooset plán zálohování hello vždy nejprve a hello zásady uchovávání informací po.

V příkladu hello níže hello rutiny nastaví parametry uchování hello zálohování na disk. To bude uchování záloh pro 10 dnů a synchronizaci dat každých 6 hodin mezi hello provozním serveru a server aplikace DPM hello. Hello ```SynchronizationFrequencyMinutes``` nedefinuje četnost zálohování je bod vytvořen, ale jak často dat je zkopírovaný toohello serveru aplikace DPM.  Toto nastavení zabrání zálohy příliš velká.

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

Pro zálohování budete tooAzure (aplikace DPM se týká toothem jako zálohování Online) rozsahy uchovávání hello lze nakonfigurovat pro [dlouhodobé uchovávání používá schéma Dědečka. otec SYN (GFS)](backup-azure-backup-cloud-as-tape.md). To znamená můžete definovat zásady uchovávání informací kombinované zahrnující denní, týdenní, měsíční a roční zásady uchovávání informací. V tomto příkladu vytvoření představující schéma hello komplexní uchovávání, který má být pole a pak nakonfigurujte rozsah uchování hello pomocí hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) rutiny.

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

V hello výše například ```$onlineSch``` je pole se čtyřmi prvky, které obsahuje hello existující plán ochranu online pro hello skupiny ochrany ve schématu GFS hello:

1. ```$onlineSch[0]```obsahuje denní plán hello
2. ```$onlineSch[1]```obsahuje Týdenní plán hello
3. ```$onlineSch[2]```obsahuje plánování měsíčně hello
4. ```$onlineSch[3]```obsahuje roční plán hello

Takže pokud budete potřebovat toomodify hello týdenní plán, je třeba toorefer toohello ```$onlineSch[1]```.

### <a name="initial-backup"></a>Prvotní zálohování
Při zálohování zdroje dat pro hello poprvé, aplikace DPM musí vytvoří počáteční repliky, vytvoří úplnou kopii toobe hello zdroje dat chráněné na svazku repliky DPM. Tato aktivita bude naplánována buď pro určitý čas nebo lze spustit ručně, pomocí hello [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) rutiny s parametrem hello ```-NOW```.

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-hello-size-of-dpm-replica--recovery-point-volume"></a>Změna velikosti hello repliky aplikace DPM a svazek bodu obnovení
Můžete také změnit hello velikost svazku repliky DPM a stínové kopie svazku pomocí [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) rutiny jako hello následující ukázka: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation - Datasource $DS - ProtectionGroup $MPG – ruční - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)

### <a name="committing-hello-changes-toohello-protection-group"></a>Potvrzení hello změny toohello skupiny ochrany
Nakonec hello změny nutné toobe potvrzené předtím, než aplikace DPM může provést zálohování hello za hello novou konfiguraci skupiny ochrany. Toho lze dosáhnout pomocí hello [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) rutiny.

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-hello-backup-points"></a>Body zálohy hello zobrazení
Můžete použít hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) rutiny tooget seznam všech bodů obnovení pro zdroj dat. V tomto příkladu provedeme následující:

* načtení všech hello PGs na serveru DPM hello a uloženy do pole```$PG```
* získat odpovídající toohello hello zdrojů dat```$PG[0]```
* získání všech hello bodů obnovení pro zdroj dat.

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a>Obnovit data chráněná v Azure
Obnovení dat je kombinací ```RecoverableItem``` objektu a ```RecoveryOption``` objektu. V předchozí části hello My seznam hello body zálohy pro zdroj dat.

V příkladu hello dole ukážeme, jak toorestore Hyper-V virtuální počítač z Azure Backup kombinování zálohování body s hello cíle pro obnovení. Tento příklad obsahuje:

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
* Další informace o DPM najdete v části tooAzure zálohování [Úvod tooDPM zálohování](backup-azure-dpm-introduction.md)
