---
title: "aaaAutomated zálohování pro SQL Server 2014 Azure Virtual Machines | Microsoft Docs"
description: "Vysvětluje funkci hello automatizované zálohování pro SQL Server 2014, virtuální počítače spuštěné v Azure. Tento článek je konkrétní tooVMs pomocí hello Resource Manager."
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
ms.openlocfilehash: c6803d8ef9f80e44a2f87918d87e099f1b562483
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a>Automatizované zálohování pro virtuální počítače SQL serveru 2014 (Resource Manager)

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016](virtual-machines-windows-sql-automated-backup-v2.md)

Automatizované zálohování automaticky nakonfiguruje [tooMicrosoft spravovaného zálohování Azure](https://msdn.microsoft.com/library/dn449496.aspx) pro všechny stávající a nové databáze na virtuální počítač Azure SQL Server 2014 Standard nebo Enterprise. To vám umožní tooconfigure standardní databázi zálohování, které využívají služby odolné Azure blob storage. Automatizované zálohování závisí na hello [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Požadavky
toouse automatizované zálohování, vezměte v úvahu hello následující požadavky:

**Operační systém**:

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

**Verzi nebo edici systému SQL Server**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

> [!IMPORTANT]
> Automatizované zálohování funguje s SQL serverem 2014. Pokud používáte SQL Server 2016, můžete použít tooback v2 automatizovaného zálohování do své databáze. Další informace najdete v tématu [v2 automatizované zálohování pro SQL Server 2016 virtuální počítače Azure](virtual-machines-windows-sql-automated-backup-v2.md).

**Konfigurace databáze**:

- Cílové databáze musí mít hello úplném modelu obnovení. Další informace o dopadu hello hello úplném modelu obnovení na zálohování najdete v tématu [zálohování pod hello úplný Model obnovení](https://technet.microsoft.com/library/ms190217.aspx).
- Cílové databáze musí být na hello výchozí instanci SQL serveru. Hello IaaS rozšíření systému SQL Server nepodporuje pojmenované instance.

**Model nasazení Azure**:

- Resource Manager

**Prostředí Azure PowerShell**:

- [Nainstalujte nejnovější příkazy prostředí Azure PowerShell hello](/powershell/azure/overview) Pokud máte v plánu tooconfigure automatizované zálohování pomocí prostředí PowerShell.

> [!NOTE]
> Automatizované zálohování spoléhá na hello rozšíření IaaS agenta systému SQL Server. Aktuální SQL bitové kopie virtuálních počítačů Galerie přidejte toto rozšíření ve výchozím nastavení. Další informace najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Nastavení

Hello následující tabulka popisuje možnosti hello, které lze konfigurovat pro automatizované zálohování. kroky skutečné konfigurace Hello lišit v závislosti na tom, zda používáte hello portál Azure nebo příkazů prostředí Windows PowerShell pro Azure.

| Nastavení | Rozsah (výchozí) | Popis |
| --- | --- | --- |
| **Automatizované zálohování** | Povolí nebo zakáže (zakázáno) | Povolí nebo zakáže automatizované zálohování pro virtuální počítač Azure SQL Server 2014 Standard nebo Enterprise. |
| **Doba uchování dat** | 1 až 30 dní (30 dní) | Hello počet dní tooretain zálohu. |
| **Účet úložiště** | Účet služby Azure Storage | Toouse účet úložiště Azure pro ukládání souborů automatizovaného zálohování do úložiště objektů blob. Kontejner se vytvoří v této toostore umístění všechny záložní soubory. záložní soubor Hello zásady vytváření názvů zahrnuje hello date, time a název počítače. |
| **Šifrování** | Povolí nebo zakáže (zakázáno) | Povolí nebo zakáže šifrování. Když je povolené šifrování, hello certifikátů používaných toorestore hello zálohování jsou umístěné v hello zadaný účet úložiště v hello stejné `automaticbackup` kontejneru pomocí hello stejné zásady vytváření názvů. Pokud se změní heslo hello, se toto heslo se vygeneruje nový certifikát, ale starý certifikát hello zůstane toorestore předchozí zálohy. |
| **Heslo** | Heslo | Heslo pro šifrovací klíče. Toto je pouze vyžaduje, pokud je povolené šifrování. V pořadí toorestore šifrované zálohování, musíte mít hello správné heslo a související certifikátu, který byl použit v době hello hello zálohy. |

## <a name="configuration-in-hello-portal"></a>Konfigurace v hello portálu

Hello Azure portálu tooconfigure automatizované zálohování můžete použít při zřizování nebo pro existující SQL Server 2014 virtuální počítače.

### <a name="new-vms"></a>Nové virtuální počítače

Při vytváření nového SQL serveru 2014 virtuálního počítače v modelu nasazení Resource Manager hello používejte hello Azure portálu tooconfigure automatizované zálohování.

V hello **nastavení systému SQL Server** vyberte **automatizované zálohování**. Hello následující Azure portálu snímek obrazovky ukazuje hello **automatizované zálohování SQL** okno.

![Konfigurace automatického zálohování SQL na portálu Azure](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Kontext, tématu hello dokončení na [zřizování virtuálního počítače s SQL serverem v Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Existující virtuální počítače

Pro existující virtuální počítače systému SQL Server vyberte virtuální počítač systému SQL Server. Potom vyberte hello **konfigurace systému SQL Server** části hello **nastavení** okno.

![Automatizované zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

V hello **konfigurace systému SQL Server** okně klikněte na tlačítko hello **upravit** tlačítka na hello automatizované zálohování části.

![Konfigurace automatického zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Po dokončení klikněte na tlačítko hello **OK** na konci hello hello tlačítko **konfigurace systému SQL Server** okno toosave změny.

Pokud povolíte automatizované zálohování pro hello poprvé, Azure automaticky nakonfiguruje hello IaaS agenta systému SQL Server hello pozadí. Během této doby se nemusí zobrazit hello portál Azure, automatizované zálohování je nakonfigurované. Počkejte několik minut, než toobe hello agenta nainstalovat, nakonfigurovat. Po této hello Azure bude odrážet portál hello nové nastavení.

> [!NOTE]
> Můžete také nakonfigurovat automatizovaného zálohování pomocí šablony. Další informace najdete v tématu [šablony Azure rychlý start pro automatizované zálohování](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configuration-with-powershell"></a>Konfigurace pomocí prostředí PowerShell

Pomocí prostředí PowerShell tooconfigure automatizované zálohování. Než začnete, musíte provést tyto akce:

- [Stáhněte a nainstalujte nejnovější prostředí Azure PowerShell text hello](http://aka.ms/webpi-azps).
- Otevřete prostředí Windows PowerShell a přidružit svůj účet. Můžete to provést pomocí následujících kroků hello v hello [konfigurovat předplatné](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) části hello zřizování tématu.

### <a name="install-hello-sql-iaas-extension"></a>Nainstalujte hello IaaS rozšíření systému SQL
Pokud jste zřídili virtuálního počítače systému SQL Server z hello portálu Azure, by měl hello IaaS rozšíření systému SQL Server již nainstalován. Můžete určit, pokud je nainstalovaná pro virtuální počítač voláním **Get-AzureRmVM** příkaz a prozkoumání hello **rozšíření** vlastnost.

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions
```

Pokud je nainstalovaná hello rozšíření IaaS agenta systému SQL Server, měli byste vidět, že je uveden jako "SqlIaaSAgent" nebo "SQLIaaSExtension". **Stav zřizování** pro hello rozšíření by měl také zobrazit, "Succeeded".

Pokud není nainstalována nebo se nezdařilo toobe zřízený, můžete ho nainstalovat s hello následující příkaz. Přidání toohello virtuálních počítačů název nebo skupině prostředků, musíte také určit oblasti hello (**$region**) umístěnou ve virtuálního počítače.

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region
```

### <a id="verifysettings"></a>Ověřte aktuální nastavení

Pokud jste povolili automatizované zálohování při zřizování, můžete použít PowerShell toocheck aktuální konfiguraci. Spustit hello **Get-AzureRmVMSqlServerExtension** příkaz a zkontrolujte hello **AutoBackupSettings** vlastnost:

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Měli byste obdržet výstup podobný toohello následující:

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

Pokud vaše výstup ukazuje, že **povolit** je nastaven příliš**False**, pak máte tooenable automatizované zálohování. Hello Dobrá zpráva je, že jste povolit a konfigurovat automatizovaného zálohování v hello stejným způsobem. Viz další část hello tyto informace.

> [!NOTE] 
> Pokud zaškrtnete nastavení hello okamžitě po provedení změny, je možné, že budete mít zpět hello původní hodnoty konfigurace. Počkejte několik minut a zkontrolujte nastavení hello znovu toomake jistotu, že byly použity změny.

### <a name="configure-automated-backup"></a>Konfigurace automatického zálohování
Můžete PowerShell tooenable automatizované zálohování, jakož i toomodify jeho konfiguraci a chování kdykoli.

Nejdřív vyberte nebo vytvořte účet úložiště pro hello záložní soubory. Hello následující skript vybere účet úložiště nebo ji vytvoří, pokud neexistuje.

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

Potom pomocí hello **New-AzureRmVMSqlServerAutoBackupConfig** příkaz tooenable a nakonfigurujte hello automatizovaného zálohování nastavení toostore zálohy v hello účtu úložiště Azure. V tomto příkladu jsou hello zálohy nastaveny toobe uchovávat 10 dní. Hello druhý příkaz, **Set-AzureRmVMSqlServerExtension**, aktualizace hello zadaný virtuální počítač Azure s těmito nastaveními.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Ho může trvat několik minut tooinstall a nakonfigurujte hello IaaS agenta systému SQL Server.

> [!NOTE]
> Existují další nastavení pro **New-AzureRmVMSqlServerAutoBackupConfig** které se týkají pouze tooSQL Server 2016 a automatizované zálohování v2. SQL Server 2014 nepodporuje hello následující nastavení: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**,  **FullBackupStartHour**, **FullBackupWindowInHours**, a **LogBackupFrequencyInMinutes**. Když zkusíte tooconfigure tato nastavení na virtuálním počítači, SQL Server 2014, se nezobrazí žádná chyba, ale nastavení hello získat nebyly použity. Pokud chcete tato nastavení na virtuálním počítači, SQL Server 2016 toouse, najdete v části [v2 automatizované zálohování pro SQL Server 2016 virtuální počítače Azure](virtual-machines-windows-sql-automated-backup-v2.md).

šifrování tooenable upravit hello předchozí skript toopass hello **EnableEncryption** parametr společně s heslo (zabezpečený řetězec) pro hello **CertificatePassword** parametr. Hello následující skript umožňuje hello nastavení automatizovaného zálohování v předchozím příkladu hello a přidá šifrování.

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

nastavení se použijí, tooconfirm [ověřte konfiguraci automatizovaného zálohování hello](#verifysettings).

### <a name="disable-automated-backup"></a>Zakázat automatizované zálohování

toodisable automatizované zálohování, spusťte hello stejný skript bez hello **-povolit** parametr toohello **New-AzureRmVMSqlServerAutoBackupConfig** příkaz. Hello absenci hello **-povolit** parametr signály hello příkaz toodisable hello funkce. Stejně jako u instalace může trvat několik minut toodisable automatizované zálohování.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a>Ukázkový skript

Hello následující skript představuje sadu proměnných, můžete přizpůsobit tooenable a konfiguraci automatizovaného zálohování pro virtuální počítač. Ve vašem případě bude pravděpodobně nutné toocustomize hello skript podle svých požadavků. Například by mít toomake změny, pokud byste chtěli toodisable hello zálohu systémové databáze nebo povolit šifrování.

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = “Azure region name such as EASTUS2”
$storage_accountname = “storageaccountname”
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10

# ResourceGroupName is hello resource group which is hosting hello VM where you are deploying hello SQL IaaS Extension

Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account toostore hello backups

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

# Apply hello Automated Backup settings toohello VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Další kroky

Automatizované zálohování nakonfiguruje spravovaného zálohování na virtuálních počítačích Azure. Proto je důležité příliš[hello v dokumentaci pro spravovanou zálohu](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello chování a důsledky.

Můžete najít další zálohování a obnovení pokyny pro SQL Server na virtuálních počítačích Azure v následujícím tématu hello: [zálohování a obnovení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).

