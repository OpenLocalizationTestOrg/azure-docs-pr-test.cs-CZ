---
title: "Automatizované zálohování v2 pro SQL Server 2016 virtuální počítače Azure | Microsoft Docs"
description: "Vysvětluje funkci automatizované zálohování pro SQL Server 2016 virtuální počítače spuštěné v Azure. Tento článek je specifické pro virtuální počítače pomocí Správce prostředků."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: ebd23868-821c-475b-b867-06d4a2e310c7
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/05/2017
ms.author: jroth
ms.openlocfilehash: e7e14b0243f82c672392d5ab4bb6aca01156465b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automated-backup-v2-for-sql-server-2016-azure-virtual-machines-resource-manager"></a>Automatizované zálohování v2 pro SQL Server 2016 virtuální počítače Azure (Resource Manager)

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016](virtual-machines-windows-sql-automated-backup-v2.md)

Automatizované zálohování v2 automaticky nakonfiguruje [spravovaného zálohování Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) pro všechny stávající a nové databáze na virtuální počítač Azure běžet tyto edice SQL Server 2016 Standard, Enterprise nebo Developer. To umožňuje nakonfigurovat standardní databázi zálohování, které využívají služby odolné Azure blob storage. Automatizované zálohování v2 závisí na [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Požadavky
Pokud chcete používat v2 automatizovaného zálohování, zkontrolujte splnění následujících předpokladů:

**Operační systém**:

- Windows Server 2012 R2
- Windows Server 2016

**Verzi nebo edici systému SQL Server**:

- SQL Server 2016 Standard
- SQL Server 2016 Enterprise
- SQL Server 2016 Developer

> [!IMPORTANT]
> Automatizované zálohování v2 spolupracuje se službou SQL Server 2016. Pokud používáte SQL Server 2014, můžete zálohovat databáze automatizovaného zálohování v1. Další informace najdete v tématu [automatizované zálohování pro SQL Server 2014 virtuální počítače Azure](virtual-machines-windows-sql-automated-backup.md).

**Konfigurace databáze**:

- Cílové databáze musí mít úplném modelu obnovení. Další informace o vlivu úplném modelu obnovení na zálohování najdete v tématu [zálohování v části the úplný Model obnovení](https://technet.microsoft.com/library/ms190217.aspx).
- Systémové databáze není nutné používat úplném modelu obnovení. Pokud budete potřebovat zálohy protokolu, které mají být provedeny pro Model, nebo databázi MSDB, ale musí používat úplném modelu obnovení.
- Cílové databáze musí být na výchozí instanci SQL serveru. IaaS rozšíření systému SQL Server nepodporuje pojmenované instance.

**Model nasazení Azure**:

- Resource Manager

> [!NOTE]
> Automatizované zálohování spoléhá na **rozšíření agenta systému SQL Server IaaS**. Aktuální SQL bitové kopie virtuálních počítačů Galerie přidejte toto rozšíření ve výchozím nastavení. Další informace najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Nastavení
Následující tabulka popisuje možnosti, které mohou být konfigurovány pro automatizované zálohování v2. Skutečné konfiguračních kroků se liší v závislosti na tom, zda používáte portál Azure nebo příkazů prostředí Windows PowerShell pro Azure.

### <a name="basic-settings"></a>Základní nastavení

| Nastavení | Rozsah (výchozí) | Popis |
| --- | --- | --- |
| **Automatizované zálohování** | Povolí nebo zakáže (zakázáno) | Povolí nebo zakáže automatizované zálohování pro virtuální počítač Azure SQL Server 2016 Standard nebo Enterprise. |
| **Doba uchování dat** | 1 až 30 dní (30 dní) | Počet dní uchování záloh. |
| **Účet úložiště** | Účet služby Azure Storage | Účet úložiště Azure pro ukládání souborů automatizovaného zálohování do úložiště objektů blob. Kontejner se vytvoří v tomto umístění pro uložení všechny záložní soubory. Zásady vytváření názvů záložní soubor obsahuje date, time a GUID databáze. |
| **Šifrování** |Povolí nebo zakáže (zakázáno) | Povolí nebo zakáže šifrování. Když je povolené šifrování, certifikátů používaných pro obnovení zálohy jsou umístěné v zadaný účet úložiště ve stejné **automaticbackup** kontejneru pomocí stejné zásady vytváření názvů. Pokud se změní heslo, se toto heslo se vygeneruje nový certifikát, ale pořád starý certifikát pro obnovení předchozí zálohy. |
| **Heslo** |Heslo | Heslo pro šifrovací klíče. Toto je pouze vyžaduje, pokud je povolené šifrování. Chcete-li obnovit šifrované zálohování, musí mít správné heslo a související certifikátu, který byl použit v době, kdy bylo provedeno zálohování. |

### <a name="advanced-settings"></a>Upřesnit nastavení

| Nastavení | Rozsah (výchozí) | Popis |
| --- | --- | --- |
| **Zálohování databáze systému** | Povolí nebo zakáže (zakázáno) | Když je povolené, tato funkce bude také zálohování databází systému: hlavní server, databázi MSDB a modelu. Pro databázi MSDB a Model databáze ověřte, zda je v režimu úplného obnovení zálohy protokolu, které mají být provedeny, chcete-li. Zálohy protokolů se nikdy provádějí pro hlavní server. A jsou provedeny žádné zálohy pro databázi TempDB. |
| **Plán zálohování** | Ruční nebo automatické (Automated) | Ve výchozím nastavení plán zálohování automaticky určí založené na protokolu růst. Ruční plán zálohování umožňuje uživateli zadat časový interval pro zálohy. V takovém případě zálohování bude vždy jen probíhat na zadané četnosti a během zadaného časového okna pro daný den. |
| **Četnost úplné zálohování** | Denně nebo týdně | Četnost úplné zálohy. V obou případech bude zahájena úplné zálohy během okna další naplánovanou dobu. Pokud je vybraná týdně, zálohování může zahrnovat více dní, dokud všechny databáze úspěšně zálohovali. |
| **Čas spuštění úplného zálohování** | 00:00 – 23:00 (01:00) | Počáteční čas daný den, během které úplné zálohování lze provést. |
| **Úplné zálohování časový interval** | 1 – 23 hodin (1 hodina) | Doba trvání časový interval daný den, během které úplné zálohování lze provést. |
| **Četnost záloh protokolu** | 5 – 60 minut (60 minut) | Četnost záloh protokolu. |

## <a name="understanding-full-backup-frequency"></a>Principy četnost úplné zálohování
Je důležité si uvědomit rozdíl mezi denní nebo týdenní úplné zálohování. V této snahy můžeme provede dva ukázkové scénáře.

### <a name="scenario-1-weekly-backups"></a>Scénář 1: Týdenní zálohy
Máte virtuální počítač na serveru SQL, který obsahuje počet velmi velké databáze.

V pondělí povolíte automatizované zálohování v2 s následujícím nastavením:

- Plán zálohování: **ruční**
- Úplné četnost zálohování: **týdně**
- Úplné zálohování počáteční čas: **01:00**
- Úplné zálohování časové okno: **1 hodina**

To znamená, že dalším dostupném časovém intervalu zálohování je úterý v 1: 00 1 hodinu. V té době zahájíte automatizovaného zálohování, zálohování databází jeden najednou. V tomto scénáři jsou dostatečně velký, že úplné zálohy dokončí pro první databáze několik databází. Ale po jedné hodině všechny databáze byly zálohovány.

V takovém případě automatizovaného zálohování bude zahájeno zálohování databází zbývající další den středa v 1: 00 1 hodinu. Pokud nejsou všechny databáze byly zálohovány v tento čas, pokusí se znovu další den ve stejnou dobu. To bude pokračovat, dokud všechny databáze byly úspěšně zálohovány.

Jakmile ho znovu dosáhne úterý, bude automatizovaného zálohování začít zálohovat všechny databáze ještě jednou.

Tento scénář popisuje automatizovaného zálohování bude fungovat pouze v rámci určeného časového období, a každou databázi, budou zálohovány jednou za týden. To také ukazuje, že je možné pro zálohy do zahrnovat více dní v případě, kde není možné dokončit všechny zálohy za jeden den.

### <a name="scenario-2-daily-backups"></a>Scénář 2: Denní zálohy
Máte virtuální počítač na serveru SQL, který obsahuje počet velmi velké databáze.

V pondělí povolíte automatizované zálohování v2 s následujícím nastavením:

- Plán zálohování: ruční
- Úplných záloh četnost: denně
- Úplné zálohování počáteční čas: 22:00
- Úplné zálohování časové okno: 6 hodin

To znamená, že dalším dostupném časovém intervalu zálohování je pondělí na 22: 00 6 hodin. V té době zahájíte automatizovaného zálohování, zálohování databází jeden najednou.

Pak úterý v 10 6 hodin, budou úplné zálohy všech databází spusťte znovu.

> [!IMPORTANT]
> Při plánování denní zálohy, doporučujeme, abyste naplánovali široké časové okno zajistit, že všechny databáze lze zálohovat v rámci této doby. To je obzvláště důležité v případě, kdy máte velké množství dat. Chcete-li zálohovat.

## <a name="configuration-in-the-portal"></a>Konfigurace na portálu

Na portálu Azure můžete použít ke konfiguraci automatizovaného zálohování v2 při zřizování nebo pro existující SQL Server 2016 virtuální počítače.

### <a name="new-vms"></a>Nové virtuální počítače

Použití portálu Azure ke konfiguraci automatizovaného zálohování v2 při vytváření nového SQL serveru 2016 virtuálního počítače v modelu nasazení Resource Manager. 

V **nastavení systému SQL Server** vyberte **automatizované zálohování**. Následující Azure portálu snímek obrazovky ukazuje **automatizované zálohování SQL** okno.

![Konfigurace automatického zálohování SQL na portálu Azure](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> Automatizované zálohování v2 ve výchozím nastavení vypnutá.

Kontext, naleznete v tématu dokončení na [zřizování virtuálního počítače s SQL serverem v Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Existující virtuální počítače

Pro existující virtuální počítače systému SQL Server vyberte virtuální počítač systému SQL Server. Vyberte **konfigurace systému SQL Server** části **nastavení** okno.

![Automatizované zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)

V **konfigurace systému SQL Server** okně klikněte **upravit** tlačítko v části Automatické zálohování.

![Konfigurace automatického zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration-edit.png)

Po dokončení klikněte **OK** tlačítko v dolní části **konfigurace systému SQL Server** okno a uložte změny.

Jestliže povolíte automatizované zálohování poprvé, nakonfiguruje Azure IaaS Agent serveru SQL Server na pozadí. Během této doby nemusí zobrazit na portálu Azure, že automatizované zálohování je nakonfigurované. Počkejte několik minut, než agent, který se má nainstalovat, nakonfigurovat. Potom se projeví na portálu Azure nové nastavení.

## <a name="configuration-with-powershell"></a>Konfigurace pomocí prostředí PowerShell

Prostředí PowerShell můžete použít ke konfiguraci automatizovaného zálohování v2. Než začnete, musíte provést tyto akce:

- [Stáhněte a nainstalujte nejnovější prostředí Azure PowerShell](http://aka.ms/webpi-azps).
- Otevřete prostředí Windows PowerShell a přidružit svůj účet. To provedete podle kroků v [konfigurovat předplatné](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) zřizování tématu.

### <a name="install-the-sql-iaas-extension"></a>Nainstalujte rozšíření IaaS SQL
Pokud zřízení virtuálního počítače s SQL serverem na portálu Azure IaaS rozšíření systému SQL Server musí být již nainstalován. Můžete určit, pokud je nainstalovaná pro virtuální počítač voláním **Get-AzureRmVM** příkaz a prozkoumání **rozšíření** vlastnost.

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions 
```

Pokud je nainstalovaná rozšíření IaaS agenta systému SQL Server, měli byste vidět, že je uveden jako "SqlIaaSAgent" nebo "SQLIaaSExtension". **Stav zřizování** pro rozšíření by měl také zobrazit, "Succeeded". 

Pokud není nainstalovaná nebo se nepovedlo zřídit, můžete ho nainstalovat pomocí následujícího příkazu. Kromě názvu a prostředek skupiny virtuálních počítačů, je nutné také zadat oblast (**$region**) umístěnou ve virtuálního počítače.

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region 
```

### <a id="verifysettings"></a>Ověřte aktuální nastavení
Pokud jste povolili automatizované zálohování při zřizování, můžete použít PowerShell ke kontrole aktuální konfigurace. Spustit **Get-AzureRmVMSqlServerExtension** příkaz a zkontrolujte **AutoBackupSettings** vlastnost:

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Měli byste obdržet výstup podobný následujícímu:

```
Enable                      : True
EnableEncryption            : False
RetentionPeriod             : 30
StorageUrl                  : https://test.blob.core.windows.net/
StorageAccessKey            :  
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : Manual
FullBackupFrequency         : WEEKLY
FullBackupStartTime         : 2
FullBackupWindowHours       : 2
LogBackupFrequency          : 60
```

Pokud vaše výstup ukazuje, že **povolit** je nastaven na **False**, budete muset povolit automatizované zálohování. Dobrá zpráva je, že můžete povolit a nakonfigurovat automatizovaného zálohování stejným způsobem. Najdete v další části pro tyto informace.

> [!NOTE] 
> Pokud zaškrtnete okamžitě po provedení změny nastavení, je možné, zobrazí se zpět původní hodnoty konfigurace. Počkejte několik minut a zkontrolujte nastavení znovu a ujistěte se, že byly použity změny.

### <a name="configure-automated-backup-v2"></a>Konfigurace automatického zálohování v2
Prostředí PowerShell můžete použít k povolení automatizované zálohování i, kdykoli upravit jeho konfiguraci a chování. 

Nejdřív vyberte nebo vytvořte účet úložiště pro záložní soubory. Následující skript vybere účet úložiště, nebo ji vytvoří, pokud neexistuje.

```powershell
$storage_accountname = “yourstorageaccount”
$storage_resourcegroupname = $resourcegroupname

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region } 
```

> [!NOTE]
> Automatizované zálohování nepodporuje ukládání záloh v storage úrovně premium, ale může trvat zálohy z disků virtuálních počítačů, které používají úložiště Premium.

Potom pomocí **New-AzureRmVMSqlServerAutoBackupConfig** příkaz povolení a konfigurace nastavení automatizovaného zálohování v2 pro ukládání záloh v účtu úložiště Azure. V tomto příkladu jsou zálohy nastaveny pro zachování 10 dní. Zálohování databáze systému jsou povolené. Úplné zálohy jsou naplánovány pro každý týden s časovým oknem začínající na 20:00 pro dvě hodiny. Zálohy protokolů jsou naplánovány pro každých 30 minut. V druhém příkazu **Set-AzureRmVMSqlServerExtension**, aktualizuje zadaný virtuální počítač Azure s těmito nastaveními.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname 
```

Ho může trvat několik minut k instalaci a konfiguraci IaaS Agent serveru SQL Server. 

Chcete-li povolit šifrování, změňte předchozí skript, který chcete předat **EnableEncryption** společně s heslo (zabezpečený řetězec) pro parametr **CertificatePassword** parametr. Následující skript umožňuje nastavení automatizovaného zálohování v předchozím příkladu a přidá šifrování.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Potvrďte nastavení se použijí, [ověřit konfiguraci automatizovaného zálohování](#verifysettings).

### <a name="disable-automated-backup"></a>Zakázat automatizované zálohování
Pokud chcete zakázat automatizovaného zálohování, spusťte stejný skriptu bez **-povolit** parametru **New-AzureRmVMSqlServerAutoBackupConfig** příkaz. Neexistence **-povolit** parametr signály příkaz funkci zakážete. Stejně jako u instalace, se může trvat několik minut zakázat automatizovaného zálohování.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a>Ukázkový skript
Následující skript představuje sadu proměnných, které můžete přizpůsobit povolit a konfigurovat automatizované zálohování pro virtuální počítač. V váš případ může být nutné přizpůsobit skript podle svých požadavků. Například nutné provést změny, pokud chcete zakázat zálohování databází systému nebo povolit šifrování.

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = “Azure region name such as EASTUS2”
$storage_accountname = “storageaccountname”
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10
$backupscheduletype = "Manual"
$fullbackupfrequency = "Weekly"
$fullbackupstarthour = "20"
$fullbackupwindow = "2"
$logbackupfrequency = "30"

# ResourceGroupName is the resource group which is hosting the VM where you are deploying the SQL IaaS Extension 

Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account to store the backups

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType $backupscheduletype -FullBackupFrequency $fullbackupfrequency `
    -FullBackupStartHour $fullbackupstarthour -FullBackupWindowInHours $fullbackupwindow `
    -LogBackupFrequencyInMinutes $logbackupfrequency

# Apply the Automated Backup settings to the VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Další kroky
Automatizované zálohování v2 nakonfiguruje spravovaného zálohování na virtuálních počítačích Azure. Proto je důležité [najdete v dokumentaci pro spravovanou zálohu](https://msdn.microsoft.com/library/dn449496.aspx) pochopit chování a důsledky.

Můžete najít další zálohování a obnovení pokyny pro SQL Server na virtuálních počítačích Azure v následujícím tématu: [zálohování a obnovení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).

