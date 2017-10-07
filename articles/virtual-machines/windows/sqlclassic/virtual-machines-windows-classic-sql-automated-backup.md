---
title: "aaaAutomated zálohování pro SQL Server virtuální počítače (klasické) | Microsoft Docs"
description: "Vysvětluje funkci hello automatizované zálohování pro SQL Server běžící ve virtuálních počítačích Azure pomocí Resource Manager. "
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 3333e830-8a60-42f5-9f44-8e02e9868d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 5d8f0412578c2d86edc6e54093a5da4891d3847e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Automatizované zálohování pro SQL Server na virtuálních počítačích Azure (klasický)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-automated-backup.md)
> * [Classic](../classic/sql-automated-backup.md)
> 
> 

Automatizované zálohování automaticky nakonfiguruje [tooMicrosoft spravovaného zálohování Azure](https://msdn.microsoft.com/library/dn449496.aspx) pro všechny stávající a nové databáze na virtuální počítač Azure SQL Server 2014 Standard nebo Enterprise. To vám umožní tooconfigure standardní databázi zálohování, které využívají služby odolné Azure blob storage. Automatizované zálohování závisí na hello [rozšíření agenta systému SQL Server IaaS](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. tooview hello Resource Manager verze tohoto článku, najdete v části [automatizované zálohování pro SQL Server ve službě Správce prostředků virtuálních počítačů Azure](../sql/virtual-machines-windows-sql-automated-backup.md).

## <a name="prerequisites"></a>Požadavky
toouse automatizované zálohování, vezměte v úvahu hello následující požadavky:

**Operační systém**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**Verzi nebo edici systému SQL Server**:

* SQL Server 2014 Standard
* SQL Server 2014 Enterprise

> [!NOTE]
> SQL Server 2016 není dosud podporována pro automatizované zálohování.
> 
> 

**Konfigurace databáze**:

* Cílové databáze musí mít hello úplném modelu obnovení.

**Prostředí Azure PowerShell**:

* [Nainstalujte nejnovější příkazy prostředí Azure PowerShell hello](/powershell/azure/overview).

**Rozšíření systému SQL Server IaaS**:

* [Nainstalujte hello rozšíření systému SQL Server IaaS](../classic/sql-server-agent-extension.md).

## <a name="settings"></a>Nastavení
Hello následující tabulka popisuje možnosti hello, které lze konfigurovat pro automatizované zálohování. Pro klasické virtuální počítače je nutné použít PowerShell tooconfigure těchto nastavení.

| Nastavení | Rozsah (výchozí) | Popis |
| --- | --- | --- |
| **Automatizované zálohování** |Povolí nebo zakáže (zakázáno) |Povolí nebo zakáže automatizované zálohování pro virtuální počítač Azure SQL Server 2014 Standard nebo Enterprise. |
| **Doba uchování dat** |1 až 30 dní (30 dní) |Hello počet dní tooretain zálohu. |
| **Účet úložiště** |Účet úložiště Azure (hello účtu úložiště vytvořeném pro hello zadané virtuálního počítače) |Toouse účet úložiště Azure pro ukládání souborů automatizovaného zálohování do úložiště objektů blob. Kontejner se vytvoří v této toostore umístění všechny záložní soubory. záložní soubor Hello zásady vytváření názvů zahrnuje hello date, time a název počítače. |
| **Šifrování** |Povolí nebo zakáže (zakázáno) |Povolí nebo zakáže šifrování. Když je povolené šifrování, hello certifikáty používané toorestore hello zálohování jsou umístěné v hello zadaný účet úložiště v hello pomocí stejné automaticbackup kontejneru hello stejné zásady vytváření názvů. Pokud se změní heslo hello, se toto heslo se vygeneruje nový certifikát, ale starý certifikát hello zůstane toorestore předchozí zálohy. |
| **Heslo** |Text heslo, (None) |Heslo pro šifrovací klíče. Toto je pouze vyžaduje, pokud je povolené šifrování. V pořadí toorestore šifrované zálohování, musíte mít hello správné heslo a související certifikátu, který byl použit v době hello hello zálohy. | **Systém zálohování databáze** | Povolí nebo zakáže (zakázáno) | Proveďte úplné zálohování Master, Model a databázi MSDB |
| **Konfigurovat plán zálohování** | Ruční nebo automatické (Automated) | Vyberte **automatizovaná** tooautomatically provést úplné a protokolu zálohy založené na protokolu růst. Vyberte **ruční** toospecify hello plán pro úplnou a protokolu zálohování. |

## <a name="configuration-with-powershell"></a>Konfigurace pomocí prostředí PowerShell
V následující příklad PowerShell text hello automatizované zálohování je nakonfigurován pro existující virtuální počítač SQL Server 2014. Hello **New-AzureVMSqlServerAutoBackupConfig** příkaz nakonfiguruje hello automatizovaného zálohování nastavení toostore zálohy v účtu úložiště Azure hello určené proměnnou hello $storageaccount. Tyto zálohy bude uchovávat 10 dní. Hello **Set-AzureVMSqlServerExtension** příkaz aktualizace hello zadaný virtuální počítač Azure s těmito nastaveními.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Ho může trvat několik minut tooinstall a nakonfigurujte hello IaaS agenta systému SQL Server.

tooenable šifrování, upravte hello předchozí skript toopass hello EnableEncryption parametr společně s heslo (zabezpečený řetězec) pro parametr CertificatePassword hello. Hello následující skript umožňuje hello nastavení automatizovaného zálohování v předchozím příkladu hello a přidá šifrování.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

toodisable automatické zálohování, spusťte hello stejný skript bez hello **-povolit** parametr toohello **New-AzureVMSqlServerAutoBackupConfig**. Stejně jako u instalace může trvat několik minut toodisable automatizované zálohování.

> [!NOTE]
> Zakázání a odinstalace agenta systému SQL Server IaaS hello neodebere hello dříve nakonfigurované nastavení spravovaného zálohování. Měli byste zakázat automatizovaného zálohování před zakázáním nebo odinstalací hello IaaS agenta systému SQL Server.
> 
> 

## <a name="next-steps"></a>Další kroky
Automatizované zálohování nakonfiguruje spravovaného zálohování na virtuálních počítačích Azure. Proto je důležité příliš[hello v dokumentaci pro spravovanou zálohu](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello chování a důsledky.

Můžete najít další zálohování a obnovení pokyny pro SQL Server na virtuálních počítačích Azure v následujícím tématu hello: [zálohování a obnovení pro SQL Server v Azure Virtual Machines](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).

Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](../classic/sql-server-agent-extension.md).

Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

