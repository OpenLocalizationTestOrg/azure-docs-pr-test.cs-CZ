---
title: "aaaAutomated v2 zálohování pro SQL Server 2016 Azure Virtual Machines | Microsoft Docs"
description: "Vysvětluje funkci hello automatizované zálohování pro SQL Server 2016 virtuální počítače spuštěné v Azure. Tento článek je konkrétní tooVMs pomocí hello Resource Manager."
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
ms.openlocfilehash: a322792fb22c76bfa74fafb711b8b1927a6e2b3a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automated-backup-v2-for-sql-server-2016-azure-virtual-machines-resource-manager"></a>Automatizované zálohování v2 pro SQL Server 2016 virtuální počítače Azure (Resource Manager)

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016](virtual-machines-windows-sql-automated-backup-v2.md)

Automatizované zálohování v2 automaticky nakonfiguruje [tooMicrosoft spravovaného zálohování Azure](https://msdn.microsoft.com/library/dn449496.aspx) pro všechny stávající a nové databáze na virtuální počítač Azure běžet tyto edice SQL Server 2016 Standard, Enterprise nebo Developer. To vám umožní tooconfigure standardní databázi zálohování, které využívají služby odolné Azure blob storage. Automatizované zálohování v2 závisí na hello [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Požadavky
toouse v2 automatizovaného zálohování, projděte si hello následující požadavky:

**Operační systém**:

- Windows Server 2012 R2
- Windows Server 2016

**Verzi nebo edici systému SQL Server**:

- SQL Server 2016 Standard
- SQL Server 2016 Enterprise
- SQL Server 2016 Developer

> [!IMPORTANT]
> Automatizované zálohování v2 spolupracuje se službou SQL Server 2016. Pokud používáte SQL Server 2014, můžete použít tooback v1 automatizovaného zálohování do své databáze. Další informace najdete v tématu [automatizované zálohování pro SQL Server 2014 virtuální počítače Azure](virtual-machines-windows-sql-automated-backup.md).

**Konfigurace databáze**:

- Cílové databáze musí mít hello úplném modelu obnovení. Další informace o dopadu hello hello úplném modelu obnovení na zálohování najdete v tématu [zálohování pod hello úplný Model obnovení](https://technet.microsoft.com/library/ms190217.aspx).
- Systémové databáze nemají toouse úplném modelu obnovení. Pokud budete potřebovat toobe zálohy protokolu pro Model, nebo databázi MSDB, ale musí používat úplném modelu obnovení.
- Cílové databáze musí být na hello výchozí instanci SQL serveru. Hello IaaS rozšíření systému SQL Server nepodporuje pojmenované instance.

**Model nasazení Azure**:

- Resource Manager

> [!NOTE]
> Automatizované zálohování spoléhá na hello **rozšíření agenta systému SQL Server IaaS**. Aktuální SQL bitové kopie virtuálních počítačů Galerie přidejte toto rozšíření ve výchozím nastavení. Další informace najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Nastavení
Hello následující tabulka popisuje možnosti hello, které mohou být konfigurovány pro automatizované zálohování v2. kroky skutečné konfigurace Hello lišit v závislosti na tom, zda používáte hello portál Azure nebo příkazů prostředí Windows PowerShell pro Azure.

### <a name="basic-settings"></a>Základní nastavení

| Nastavení | Rozsah (výchozí) | Popis |
| --- | --- | --- |
| **Automatizované zálohování** | Povolí nebo zakáže (zakázáno) | Povolí nebo zakáže automatizované zálohování pro virtuální počítač Azure SQL Server 2016 Standard nebo Enterprise. |
| **Doba uchování dat** | 1 až 30 dní (30 dní) | Hello počet dní tooretain záloh. |
| **Účet úložiště** | Účet služby Azure Storage | Toouse účet úložiště Azure pro ukládání souborů automatizovaného zálohování do úložiště objektů blob. Kontejner se vytvoří v této toostore umístění všechny záložní soubory. záložní soubor Hello zásady vytváření názvů zahrnuje hello date, time a GUID databáze. |
| **Šifrování** |Povolí nebo zakáže (zakázáno) | Povolí nebo zakáže šifrování. Když je povolené šifrování, hello certifikátů používaných toorestore hello zálohování jsou umístěné v hello zadaný účet úložiště v hello stejné **automaticbackup** kontejneru pomocí hello stejné zásady vytváření názvů. Pokud se změní heslo hello, se toto heslo se vygeneruje nový certifikát, ale starý certifikát hello zůstane toorestore předchozí zálohy. |
| **Heslo** |Heslo | Heslo pro šifrovací klíče. Toto je pouze vyžaduje, pokud je povolené šifrování. V pořadí toorestore šifrované zálohování, musíte mít hello správné heslo a související certifikátu, který byl použit v době hello hello zálohy. |

### <a name="advanced-settings"></a>Upřesnit nastavení

| Nastavení | Rozsah (výchozí) | Popis |
| --- | --- | --- |
| **Zálohování databáze systému** | Povolí nebo zakáže (zakázáno) | Když je povolené, tato funkce bude také zálohování databází systému hello: hlavní server, databázi MSDB a modelu. Hello databázi MSDB a databáze modelu ověřte, že jsou v režimu úplného obnovení, pokud chcete, aby toobe zálohy protokolu prováděné. Zálohy protokolů se nikdy provádějí pro hlavní server. A jsou provedeny žádné zálohy pro databázi TempDB. |
| **Plán zálohování** | Ruční nebo automatické (Automated) | Ve výchozím nastavení plán zálohování hello automaticky určí založené na protokolu růst hello. Ruční plán zálohování umožňuje hello uživatele toospecify hello časové okno zálohování. V takovém případě bude vždy jen trvat zálohy na hello zadané frekvence a během hello zadaný časový interval daný den. |
| **Četnost úplné zálohování** | Denně nebo týdně | Četnost úplné zálohy. V obou případech úplné zálohování bude zahájena při dalším plánovaném čase intervalu hello. Pokud je vybraná týdně, zálohování může zahrnovat více dní, dokud všechny databáze úspěšně zálohovali. |
| **Čas spuštění úplného zálohování** | 00:00 – 23:00 (01:00) | Počáteční čas daný den, během které úplné zálohování lze provést. |
| **Úplné zálohování časový interval** | 1 – 23 hodin (1 hodina) | Doba trvání hello časové okno daný den, během které úplné zálohování lze provést. |
| **Četnost záloh protokolu** | 5 – 60 minut (60 minut) | Četnost záloh protokolu. |

## <a name="understanding-full-backup-frequency"></a>Principy četnost úplné zálohování
Je důležité toounderstand hello rozdíl mezi denní nebo týdenní úplné zálohování. V této snahy můžeme provede dva ukázkové scénáře.

### <a name="scenario-1-weekly-backups"></a>Scénář 1: Týdenní zálohy
Máte virtuální počítač na serveru SQL, který obsahuje počet velmi velké databáze.

V pondělí povolíte automatizované zálohování v2 hello následující nastavení:

- Plán zálohování: **ruční**
- Úplné četnost zálohování: **týdně**
- Úplné zálohování počáteční čas: **01:00**
- Úplné zálohování časové okno: **1 hodina**

To znamená, že tento hello další dostupné intervalu zálohování bude úterý v 1: 00 1 hodinu. V té době zahájíte automatizovaného zálohování, zálohování databází jeden najednou. V tomto scénáři jsou dostatečně velké na to, že úplné zálohy dokončí hello první několik databází vaší databáze. Ale po jedné hodině všechny hello databáze byly zálohovány.

V takovém případě automatizovaného zálohování bude zahájeno zálohování hello zbývající databáze hello další den, středu v 1: 00 1 hodinu. Pokud nejsou všechny databáze byly zálohovány v tento čas, se pokusí znovu hello další den v hello stejný čas. To bude pokračovat, dokud všechny databáze byly úspěšně zálohovány.

Jakmile ho znovu dosáhne úterý, bude automatizovaného zálohování začít zálohovat všechny databáze ještě jednou.

Tento scénář popisuje automatizovaného zálohování bude fungovat pouze v rámci hello zadané časové období, a každou databázi, budou zálohovány jednou za týden. To také ukazuje, že je možné pro zálohování toospan více dní v hello případ Pokud to není možné toocomplete všechny zálohy za jeden den.

### <a name="scenario-2-daily-backups"></a>Scénář 2: Denní zálohy
Máte virtuální počítač na serveru SQL, který obsahuje počet velmi velké databáze.

V pondělí povolíte automatizované zálohování v2 hello následující nastavení:

- Plán zálohování: ruční
- Úplných záloh četnost: denně
- Úplné zálohování počáteční čas: 22:00
- Úplné zálohování časové okno: 6 hodin

To znamená, že tento hello další dostupné intervalu zálohování bude pondělí v 22: 00 6 hodin. V té době zahájíte automatizovaného zálohování, zálohování databází jeden najednou.

Pak úterý v 10 6 hodin, budou úplné zálohy všech databází spusťte znovu.

> [!IMPORTANT]
> Při plánování denní zálohy, doporučujeme, abyste naplánovali tooensure okno celou dobu, všechny databáze může být zálohována během této doby. To je obzvláště důležité v případě hello, kdy máte velké množství dat tooback nahoru.

## <a name="configuration-in-hello-portal"></a>Konfigurace v hello portálu

V2 automatizovaného zálohování Azure portálu tooconfigure hello můžete použít při zřizování nebo pro existující SQL Server 2016 virtuální počítače.

### <a name="new-vms"></a>Nové virtuální počítače

Použijte v2 automatizovaného zálohování Azure portálu tooconfigure hello při vytváření nového SQL serveru 2016 virtuálního počítače v modelu nasazení Resource Manager hello. 

V hello **nastavení systému SQL Server** vyberte **automatizované zálohování**. Hello následující Azure portálu snímek obrazovky ukazuje hello **automatizované zálohování SQL** okno.

![Konfigurace automatického zálohování SQL na portálu Azure](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> Automatizované zálohování v2 ve výchozím nastavení vypnutá.

Kontext, tématu hello dokončení na [zřizování virtuálního počítače s SQL serverem v Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Existující virtuální počítače

Pro existující virtuální počítače systému SQL Server vyberte virtuální počítač systému SQL Server. Potom vyberte hello **konfigurace systému SQL Server** části hello **nastavení** okno.

![Automatizované zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)

V hello **konfigurace systému SQL Server** okně klikněte na tlačítko hello **upravit** tlačítka na hello automatizované zálohování části.

![Konfigurace automatického zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration-edit.png)

Po dokončení klikněte na tlačítko hello **OK** na konci hello hello tlačítko **konfigurace systému SQL Server** okno toosave změny.

Pokud povolíte automatizované zálohování pro hello poprvé, Azure automaticky nakonfiguruje hello IaaS agenta systému SQL Server hello pozadí. Během této doby se nemusí zobrazit hello portál Azure, automatizované zálohování je nakonfigurované. Počkejte několik minut, než toobe hello agenta nainstalovat, nakonfigurovat. Po této hello Azure bude odrážet portál hello nové nastavení.

## <a name="configuration-with-powershell"></a>Konfigurace pomocí prostředí PowerShell

Pomocí prostředí PowerShell tooconfigure v2 automatizovaného zálohování. Než začnete, musíte provést tyto akce:

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

Pokud vaše výstup ukazuje, že **povolit** je nastaven příliš**False**, pak máte tooenable automatizované zálohování. Hello Dobrá zpráva je, že jste povolit a konfigurovat automatizovaného zálohování v hello stejným způsobem. Viz další část hello tyto informace.

> [!NOTE] 
> Pokud zaškrtnete nastavení hello okamžitě po provedení změny, je možné, že budete mít zpět hello původní hodnoty konfigurace. Počkejte několik minut a zkontrolujte nastavení hello znovu toomake jistotu, že byly použity změny.

### <a name="configure-automated-backup-v2"></a>Konfigurace automatického zálohování v2
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

Potom pomocí hello **New-AzureRmVMSqlServerAutoBackupConfig** příkaz tooenable a nakonfigurujte hello v2 automatizovaného zálohování nastavení toostore zálohy v účtu úložiště Azure hello. V tomto příkladu jsou hello zálohy nastaveny toobe uchovávat 10 dní. Zálohování databáze systému jsou povolené. Úplné zálohy jsou naplánovány pro každý týden s časovým oknem začínající na 20:00 pro dvě hodiny. Zálohy protokolů jsou naplánovány pro každých 30 minut. Hello druhý příkaz, **Set-AzureRmVMSqlServerExtension**, aktualizace hello zadaný virtuální počítač Azure s těmito nastaveními.

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

Ho může trvat několik minut tooinstall a nakonfigurujte hello IaaS agenta systému SQL Server. 

šifrování tooenable upravit hello předchozí skript toopass hello **EnableEncryption** parametr společně s heslo (zabezpečený řetězec) pro hello **CertificatePassword** parametr. Hello následující skript umožňuje hello nastavení automatizovaného zálohování v předchozím příkladu hello a přidá šifrování.

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
$backupscheduletype = "Manual"
$fullbackupfrequency = "Weekly"
$fullbackupstarthour = "20"
$fullbackupwindow = "2"
$logbackupfrequency = "30"

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
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType $backupscheduletype -FullBackupFrequency $fullbackupfrequency `
    -FullBackupStartHour $fullbackupstarthour -FullBackupWindowInHours $fullbackupwindow `
    -LogBackupFrequencyInMinutes $logbackupfrequency

# Apply hello Automated Backup settings toohello VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Další kroky
Automatizované zálohování v2 nakonfiguruje spravovaného zálohování na virtuálních počítačích Azure. Proto je důležité příliš[hello v dokumentaci pro spravovanou zálohu](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello chování a důsledky.

Můžete najít další zálohování a obnovení pokyny pro SQL Server na virtuálních počítačích Azure v následujícím tématu hello: [zálohování a obnovení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).

