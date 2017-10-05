---
title: "Automatizované zálohování pro SQL Server 2014 virtuální počítače Azure | Microsoft Docs"
description: "Vysvětluje funkci automatizované zálohování pro SQL Server 2014, virtuální počítače spuštěné v Azure. Tento článek je specifické pro virtuální počítače pomocí Správce prostředků."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: bdc63fd1-db49-4e76-87d5-b5c6a890e53c
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 91aab896dd5f06c950ee0ed8f36cc6a953d91611
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a>Automatizované zálohování pro virtuální počítače SQL serveru 2014 (Resource Manager)

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016](virtual-machines-windows-sql-automated-backup-v2.md)

Automatizované zálohování automaticky nakonfiguruje [spravovaného zálohování Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) pro všechny stávající a nové databáze na virtuální počítač Azure SQL Server 2014 Standard nebo Enterprise. To umožňuje nakonfigurovat standardní databázi zálohování, které využívají služby odolné Azure blob storage. Automatizované zálohování závisí na [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Požadavky
Pomocí automatizovaného zálohování, zvažte následující požadavky:

**Operační systém**:

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

**Verzi nebo edici systému SQL Server**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

> [!IMPORTANT]
> Automatizované zálohování funguje s SQL serverem 2014. Pokud používáte SQL Server 2016, můžete k zálohování databází v2 automatizovaného zálohování. Další informace najdete v tématu [v2 automatizované zálohování pro SQL Server 2016 virtuální počítače Azure](virtual-machines-windows-sql-automated-backup-v2.md).

**Konfigurace databáze**:

- Cílové databáze musí mít úplném modelu obnovení. Další informace o vlivu úplném modelu obnovení na zálohování najdete v tématu [zálohování v části the úplný Model obnovení](https://technet.microsoft.com/library/ms190217.aspx).
- Cílové databáze musí být na výchozí instanci SQL serveru. IaaS rozšíření systému SQL Server nepodporuje pojmenované instance.

**Model nasazení Azure**:

- Resource Manager

**Prostředí Azure PowerShell**:

- [Nainstalujte nejnovější příkazy prostředí Azure PowerShell](/powershell/azure/overview) Pokud chcete konfigurovat automatizovaného zálohování pomocí prostředí PowerShell.

> [!NOTE]
> Automatizované zálohování spoléhá na rozšíření agenta systému SQL Server IaaS. Aktuální SQL bitové kopie virtuálních počítačů Galerie přidejte toto rozšíření ve výchozím nastavení. Další informace najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Nastavení

Následující tabulka popisuje možnosti, které lze konfigurovat pro automatizované zálohování. Skutečné konfiguračních kroků se liší v závislosti na tom, zda používáte portál Azure nebo příkazů prostředí Windows PowerShell pro Azure.

| Nastavení | Rozsah (výchozí) | Popis |
| --- | --- | --- |
| **Automatizované zálohování** | Povolí nebo zakáže (zakázáno) | Povolí nebo zakáže automatizované zálohování pro virtuální počítač Azure SQL Server 2014 Standard nebo Enterprise. |
| **Doba uchování dat** | 1 až 30 dní (30 dní) | Počet dní, které chcete zachovat zálohu. |
| **Účet úložiště** | Účet služby Azure Storage | Účet úložiště Azure pro ukládání souborů automatizovaného zálohování do úložiště objektů blob. Kontejner se vytvoří v tomto umístění pro uložení všechny záložní soubory. Zásady vytváření názvů záložní soubor obsahuje datum, čas a název počítače. |
| **Šifrování** | Povolí nebo zakáže (zakázáno) | Povolí nebo zakáže šifrování. Když je povolené šifrování, certifikátů používaných pro obnovení zálohy jsou umístěné v zadaný účet úložiště ve stejné `automaticbackup` kontejneru pomocí stejné zásady vytváření názvů. Pokud se změní heslo, se toto heslo se vygeneruje nový certifikát, ale pořád starý certifikát pro obnovení předchozí zálohy. |
| **Heslo** | Heslo | Heslo pro šifrovací klíče. Toto je pouze vyžaduje, pokud je povolené šifrování. Chcete-li obnovit šifrované zálohování, musí mít správné heslo a související certifikátu, který byl použit v době, kdy bylo provedeno zálohování. |

## <a name="configuration-in-the-portal"></a>Konfigurace na portálu

Na portálu Azure můžete použít ke konfiguraci automatizovaného zálohování při zřizování nebo pro existující SQL Server 2014 virtuální počítače.

### <a name="new-vms"></a>Nové virtuální počítače

Použití portálu Azure ke konfiguraci automatizovaného zálohování při vytváření nového SQL serveru 2014 virtuálního počítače v modelu nasazení Resource Manager.

V **nastavení systému SQL Server** vyberte **automatizované zálohování**. Následující Azure portálu snímek obrazovky ukazuje **automatizované zálohování SQL** okno.

![Konfigurace automatického zálohování SQL na portálu Azure](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Kontext, naleznete v tématu dokončení na [zřizování virtuálního počítače s SQL serverem v Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Existující virtuální počítače

Pro existující virtuální počítače systému SQL Server vyberte virtuální počítač systému SQL Server. Vyberte **konfigurace systému SQL Server** části **nastavení** okno.

![Automatizované zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

V **konfigurace systému SQL Server** okně klikněte **upravit** tlačítko v části Automatické zálohování.

![Konfigurace automatického zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Po dokončení klikněte **OK** tlačítko v dolní části **konfigurace systému SQL Server** okno a uložte změny.

Jestliže povolíte automatizované zálohování poprvé, nakonfiguruje Azure IaaS Agent serveru SQL Server na pozadí. Během této doby nemusí zobrazit na portálu Azure, že automatizované zálohování je nakonfigurované. Počkejte několik minut, než agent, který se má nainstalovat, nakonfigurovat. Potom se projeví na portálu Azure nové nastavení.

> [!NOTE]
> Můžete také nakonfigurovat automatizovaného zálohování pomocí šablony. Další informace najdete v tématu [šablony Azure rychlý start pro automatizované zálohování](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configuration-with-powershell"></a>Konfigurace pomocí prostředí PowerShell

Prostředí PowerShell můžete použít ke konfiguraci automatizovaného zálohování. Než začnete, musíte provést tyto akce:

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
Enable                      : False
EnableEncryption            : False
RetentionPeriod             : -1
StorageUrl                  : NOTSET
StorageAccessKey            : 
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : 
FullBackupFrequency         : 
FullBackupStartTime         : 
FullBackupWindowHours       : 
LogBackupFrequency          : 
```

Pokud vaše výstup ukazuje, že **povolit** je nastaven na **False**, budete muset povolit automatizované zálohování. Dobrá zpráva je, že můžete povolit a nakonfigurovat automatizovaného zálohování stejným způsobem. Najdete v další části pro tyto informace.

> [!NOTE] 
> Pokud zaškrtnete okamžitě po provedení změny nastavení, je možné, zobrazí se zpět původní hodnoty konfigurace. Počkejte několik minut a zkontrolujte nastavení znovu a ujistěte se, že byly použity změny.

### <a name="configure-automated-backup"></a>Konfigurace automatického zálohování
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

Potom pomocí **New-AzureRmVMSqlServerAutoBackupConfig** příkaz pro povolení a konfigurace nastavení automatizovaného zálohování pro ukládání záloh v účtu úložiště Azure. V tomto příkladu jsou zálohy nastaveny pro zachování 10 dní. V druhém příkazu **Set-AzureRmVMSqlServerExtension**, aktualizuje zadaný virtuální počítač Azure s těmito nastaveními.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Ho může trvat několik minut k instalaci a konfiguraci IaaS Agent serveru SQL Server.

> [!NOTE]
> Existují další nastavení pro **New-AzureRmVMSqlServerAutoBackupConfig** , se vztahují pouze na SQL Server 2016 a automatizované zálohování v2. SQL Server 2014 nepodporuje následující nastavení: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**, **FullBackupStartHour**, **FullBackupWindowInHours**, a **LogBackupFrequencyInMinutes**. Pokud se pokusíte nakonfigurovat tato nastavení na virtuální počítač SQL Server 2014, se nezobrazí žádná chyba, ale nastavení získat nebyly použity. Pokud chcete použít tato nastavení na virtuálním počítači, SQL Server 2016, přečtěte si téma [v2 automatizované zálohování pro SQL Server 2016 virtuální počítače Azure](virtual-machines-windows-sql-automated-backup-v2.md).

Chcete-li povolit šifrování, změňte předchozí skript, který chcete předat **EnableEncryption** společně s heslo (zabezpečený řetězec) pro parametr **CertificatePassword** parametr. Následující skript umožňuje nastavení automatizovaného zálohování v předchozím příkladu a přidá šifrování.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

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
    -ResourceGroupName $storage_resourcegroupname

# Apply the Automated Backup settings to the VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Další kroky

Automatizované zálohování nakonfiguruje spravovaného zálohování na virtuálních počítačích Azure. Proto je důležité [najdete v dokumentaci pro spravovanou zálohu](https://msdn.microsoft.com/library/dn449496.aspx) pochopit chování a důsledky.

Můžete najít další zálohování a obnovení pokyny pro SQL Server na virtuálních počítačích Azure v následujícím tématu: [zálohování a obnovení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).

