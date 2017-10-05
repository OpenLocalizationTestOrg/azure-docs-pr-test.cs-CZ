---
title: "Automatizované zálohování pro virtuální počítače serveru SQL (klasické) | Microsoft Docs"
description: "Vysvětluje funkci automatizované zálohování pro SQL Server běžící ve virtuálních počítačích Azure pomocí Resource Manager. "
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
ms.openlocfilehash: f7664291c2f45c422d52f682d08dbb67ab32b099
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a><span data-ttu-id="d63e7-103">Automatizované zálohování pro SQL Server na virtuálních počítačích Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="d63e7-103">Automated Backup for SQL Server in Azure Virtual Machines (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d63e7-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d63e7-104">Resource Manager</span></span>](../sql/virtual-machines-windows-sql-automated-backup.md)
> * [<span data-ttu-id="d63e7-105">Classic</span><span class="sxs-lookup"><span data-stu-id="d63e7-105">Classic</span></span>](../classic/sql-automated-backup.md)
> 
> 

<span data-ttu-id="d63e7-106">Automatizované zálohování automaticky nakonfiguruje [spravovaného zálohování Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) pro všechny stávající a nové databáze na virtuální počítač Azure SQL Server 2014 Standard nebo Enterprise.</span><span class="sxs-lookup"><span data-stu-id="d63e7-106">Automated Backup automatically configures [Managed Backup to Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) for all existing and new databases on an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> <span data-ttu-id="d63e7-107">To umožňuje nakonfigurovat standardní databázi zálohování, které využívají služby odolné Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="d63e7-107">This enables you to configure regular database backups that utilize durable Azure blob storage.</span></span> <span data-ttu-id="d63e7-108">Automatizované zálohování závisí na [rozšíření agenta systému SQL Server IaaS](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d63e7-108">Automated Backup depends on the [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="d63e7-109">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d63e7-109">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="d63e7-110">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="d63e7-110">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="d63e7-111">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d63e7-111">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="d63e7-112">Resource Manager verzi v tomto článku najdete v tématu [automatizované zálohování pro SQL Server ve službě Správce prostředků virtuálních počítačů Azure](../sql/virtual-machines-windows-sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="d63e7-112">To view the Resource Manager version of this article, see [Automated Backup for SQL Server in Azure Virtual Machines Resource Manager](../sql/virtual-machines-windows-sql-automated-backup.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d63e7-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d63e7-113">Prerequisites</span></span>
<span data-ttu-id="d63e7-114">Pomocí automatizovaného zálohování, zvažte následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="d63e7-114">To use Automated Backup, consider the following prerequisites:</span></span>

<span data-ttu-id="d63e7-115">**Operační systém**:</span><span class="sxs-lookup"><span data-stu-id="d63e7-115">**Operating System**:</span></span>

* <span data-ttu-id="d63e7-116">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="d63e7-116">Windows Server 2012</span></span>
* <span data-ttu-id="d63e7-117">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="d63e7-117">Windows Server 2012 R2</span></span>
* <span data-ttu-id="d63e7-118">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="d63e7-118">Windows Server 2016</span></span>

<span data-ttu-id="d63e7-119">**Verzi nebo edici systému SQL Server**:</span><span class="sxs-lookup"><span data-stu-id="d63e7-119">**SQL Server version/edition**:</span></span>

* <span data-ttu-id="d63e7-120">SQL Server 2014 Standard</span><span class="sxs-lookup"><span data-stu-id="d63e7-120">SQL Server 2014 Standard</span></span>
* <span data-ttu-id="d63e7-121">SQL Server 2014 Enterprise</span><span class="sxs-lookup"><span data-stu-id="d63e7-121">SQL Server 2014 Enterprise</span></span>

> [!NOTE]
> <span data-ttu-id="d63e7-122">SQL Server 2016 není dosud podporována pro automatizované zálohování.</span><span class="sxs-lookup"><span data-stu-id="d63e7-122">SQL Server 2016 is not yet supported for Automated Backup.</span></span>
> 
> 

<span data-ttu-id="d63e7-123">**Konfigurace databáze**:</span><span class="sxs-lookup"><span data-stu-id="d63e7-123">**Database configuration**:</span></span>

* <span data-ttu-id="d63e7-124">Cílové databáze musí mít úplném modelu obnovení.</span><span class="sxs-lookup"><span data-stu-id="d63e7-124">Target databases must use the full recovery model.</span></span>

<span data-ttu-id="d63e7-125">**Prostředí Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="d63e7-125">**Azure PowerShell**:</span></span>

* <span data-ttu-id="d63e7-126">[Nainstalujte nejnovější příkazy prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d63e7-126">[Install the latest Azure PowerShell commands](/powershell/azure/overview).</span></span>

<span data-ttu-id="d63e7-127">**Rozšíření systému SQL Server IaaS**:</span><span class="sxs-lookup"><span data-stu-id="d63e7-127">**SQL Server IaaS Extension**:</span></span>

* <span data-ttu-id="d63e7-128">[Nainstalujte rozšíření SQL Server IaaS](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="d63e7-128">[Install the SQL Server IaaS Extension](../classic/sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="d63e7-129">Nastavení</span><span class="sxs-lookup"><span data-stu-id="d63e7-129">Settings</span></span>
<span data-ttu-id="d63e7-130">Následující tabulka popisuje možnosti, které lze konfigurovat pro automatizované zálohování.</span><span class="sxs-lookup"><span data-stu-id="d63e7-130">The following table describes the options that can be configured for Automated Backup.</span></span> <span data-ttu-id="d63e7-131">Pro klasické virtuální počítače musíte použít PowerShell k nakonfigurování těchto nastavení.</span><span class="sxs-lookup"><span data-stu-id="d63e7-131">For classic VMs, you must use PowerShell to configure these settings.</span></span>

| <span data-ttu-id="d63e7-132">Nastavení</span><span class="sxs-lookup"><span data-stu-id="d63e7-132">Setting</span></span> | <span data-ttu-id="d63e7-133">Rozsah (výchozí)</span><span class="sxs-lookup"><span data-stu-id="d63e7-133">Range (Default)</span></span> | <span data-ttu-id="d63e7-134">Popis</span><span class="sxs-lookup"><span data-stu-id="d63e7-134">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d63e7-135">**Automatizované zálohování**</span><span class="sxs-lookup"><span data-stu-id="d63e7-135">**Automated Backup**</span></span> |<span data-ttu-id="d63e7-136">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="d63e7-136">Enable/Disable (Disabled)</span></span> |<span data-ttu-id="d63e7-137">Povolí nebo zakáže automatizované zálohování pro virtuální počítač Azure SQL Server 2014 Standard nebo Enterprise.</span><span class="sxs-lookup"><span data-stu-id="d63e7-137">Enables or disables Automated Backup for an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> |
| <span data-ttu-id="d63e7-138">**Doba uchování dat**</span><span class="sxs-lookup"><span data-stu-id="d63e7-138">**Retention Period**</span></span> |<span data-ttu-id="d63e7-139">1 až 30 dní (30 dní)</span><span class="sxs-lookup"><span data-stu-id="d63e7-139">1-30 days (30 days)</span></span> |<span data-ttu-id="d63e7-140">Počet dní, které chcete zachovat zálohu.</span><span class="sxs-lookup"><span data-stu-id="d63e7-140">The number of days to retain a backup.</span></span> |
| <span data-ttu-id="d63e7-141">**Účet úložiště**</span><span class="sxs-lookup"><span data-stu-id="d63e7-141">**Storage Account**</span></span> |<span data-ttu-id="d63e7-142">Účet úložiště Azure (pro zadaný virtuální počítač vytvořit účet úložiště)</span><span class="sxs-lookup"><span data-stu-id="d63e7-142">Azure storage account (the storage account created for the specified VM)</span></span> |<span data-ttu-id="d63e7-143">Účet úložiště Azure pro ukládání souborů automatizovaného zálohování do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="d63e7-143">An Azure storage account to use for storing Automated Backup files in blob storage.</span></span> <span data-ttu-id="d63e7-144">Kontejner se vytvoří v tomto umístění pro uložení všechny záložní soubory.</span><span class="sxs-lookup"><span data-stu-id="d63e7-144">A container is created at this location to store all backup files.</span></span> <span data-ttu-id="d63e7-145">Zásady vytváření názvů záložní soubor obsahuje datum, čas a název počítače.</span><span class="sxs-lookup"><span data-stu-id="d63e7-145">The backup file naming convention includes the date, time, and machine name.</span></span> |
| <span data-ttu-id="d63e7-146">**Šifrování**</span><span class="sxs-lookup"><span data-stu-id="d63e7-146">**Encryption**</span></span> |<span data-ttu-id="d63e7-147">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="d63e7-147">Enable/Disable (Disabled)</span></span> |<span data-ttu-id="d63e7-148">Povolí nebo zakáže šifrování.</span><span class="sxs-lookup"><span data-stu-id="d63e7-148">Enables or disables encryption.</span></span> <span data-ttu-id="d63e7-149">Když je povolené šifrování, certifikátů používaných pro obnovení zálohy jsou umístěné v zadaný účet úložiště ve stejném kontejneru automaticbackup pomocí stejné zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="d63e7-149">When encryption is enabled, the certificates used to restore the backup are located in the specified storage account in the same automaticbackup container using the same naming convention.</span></span> <span data-ttu-id="d63e7-150">Pokud se změní heslo, se toto heslo se vygeneruje nový certifikát, ale pořád starý certifikát pro obnovení předchozí zálohy.</span><span class="sxs-lookup"><span data-stu-id="d63e7-150">If the password changes, a new certificate is generated with that password, but the old certificate remains to restore prior backups.</span></span> |
| <span data-ttu-id="d63e7-151">**Heslo**</span><span class="sxs-lookup"><span data-stu-id="d63e7-151">**Password**</span></span> |<span data-ttu-id="d63e7-152">Text heslo, (None)</span><span class="sxs-lookup"><span data-stu-id="d63e7-152">Password text (None)</span></span> |<span data-ttu-id="d63e7-153">Heslo pro šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="d63e7-153">A password for encryption keys.</span></span> <span data-ttu-id="d63e7-154">Toto je pouze vyžaduje, pokud je povolené šifrování.</span><span class="sxs-lookup"><span data-stu-id="d63e7-154">This is only required if encryption is enabled.</span></span> <span data-ttu-id="d63e7-155">Chcete-li obnovit šifrované zálohování, musí mít správné heslo a související certifikátu, který byl použit v době, kdy bylo provedeno zálohování.</span><span class="sxs-lookup"><span data-stu-id="d63e7-155">In order to restore an encrypted backup, you must have the correct password and related certificate that was used at the time the backup was taken.</span></span> | <span data-ttu-id="d63e7-156">**Systém zálohování databáze**</span><span class="sxs-lookup"><span data-stu-id="d63e7-156">**Backup system databases**</span></span> | <span data-ttu-id="d63e7-157">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="d63e7-157">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="d63e7-158">Proveďte úplné zálohování Master, Model a databázi MSDB</span><span class="sxs-lookup"><span data-stu-id="d63e7-158">Take full backups of Master, Model, and MSDB</span></span> |
| <span data-ttu-id="d63e7-159">**Konfigurovat plán zálohování**</span><span class="sxs-lookup"><span data-stu-id="d63e7-159">**Configure backup schedule**</span></span> | <span data-ttu-id="d63e7-160">Ruční nebo automatické (Automated)</span><span class="sxs-lookup"><span data-stu-id="d63e7-160">Manual/Automated (Automated)</span></span> | <span data-ttu-id="d63e7-161">Vyberte **automatizovaná** automatické provést úplné a zálohy založené na protokolu růst protokolování.</span><span class="sxs-lookup"><span data-stu-id="d63e7-161">Select **Automated** to automatically take full and log backups based on log growth.</span></span> <span data-ttu-id="d63e7-162">Vyberte **ruční** zadat plán pro úplnou a protokolu zálohování.</span><span class="sxs-lookup"><span data-stu-id="d63e7-162">Select **Manual** to specify the schedule for full and log backups.</span></span> |

## <a name="configuration-with-powershell"></a><span data-ttu-id="d63e7-163">Konfigurace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d63e7-163">Configuration with PowerShell</span></span>
<span data-ttu-id="d63e7-164">V následujícím příkladu prostředí PowerShell automatizované zálohování je nakonfigurován pro existující virtuální počítač SQL Server 2014.</span><span class="sxs-lookup"><span data-stu-id="d63e7-164">In the following PowerShell example, Automated Backup is configured for an existing SQL Server 2014 VM.</span></span> <span data-ttu-id="d63e7-165">**New-AzureVMSqlServerAutoBackupConfig** příkaz nakonfiguruje nastavení automatizovaného zálohování pro ukládání záloh v určené proměnnou $storageaccount účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d63e7-165">The **New-AzureVMSqlServerAutoBackupConfig** command configures the Automated Backup settings to store backups in the Azure storage account specified by the $storageaccount variable.</span></span> <span data-ttu-id="d63e7-166">Tyto zálohy bude uchovávat 10 dní.</span><span class="sxs-lookup"><span data-stu-id="d63e7-166">These backups will be retained for 10 days.</span></span> <span data-ttu-id="d63e7-167">**Set-AzureVMSqlServerExtension** příkaz aktualizuje zadaný virtuální počítač Azure s těmito nastaveními.</span><span class="sxs-lookup"><span data-stu-id="d63e7-167">The **Set-AzureVMSqlServerExtension** command updates the specified Azure VM with these settings.</span></span>

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

<span data-ttu-id="d63e7-168">Ho může trvat několik minut k instalaci a konfiguraci IaaS Agent serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d63e7-168">It could take several minutes to install and configure the SQL Server IaaS Agent.</span></span>

<span data-ttu-id="d63e7-169">Pokud chcete povolit šifrování, upravte předchozí skript k předání parametru EnableEncryption společně s pro parametr CertificatePassword heslo (zabezpečený řetězec).</span><span class="sxs-lookup"><span data-stu-id="d63e7-169">To enable encryption, modify the previous script to pass the EnableEncryption parameter along with a password (secure string) for the CertificatePassword parameter.</span></span> <span data-ttu-id="d63e7-170">Následující skript umožňuje nastavení automatizovaného zálohování v předchozím příkladu a přidá šifrování.</span><span class="sxs-lookup"><span data-stu-id="d63e7-170">The following script enables the Automated Backup settings in the previous example and adds encryption.</span></span>

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

<span data-ttu-id="d63e7-171">Chcete-li zakázat automatické zálohování, spusťte stejný skript bez **-povolit** parametru **New-AzureVMSqlServerAutoBackupConfig**.</span><span class="sxs-lookup"><span data-stu-id="d63e7-171">To disable automatic backup, run the same script without the **-Enable** parameter to the **New-AzureVMSqlServerAutoBackupConfig**.</span></span> <span data-ttu-id="d63e7-172">Stejně jako u instalace, se může trvat několik minut zakázat automatizovaného zálohování.</span><span class="sxs-lookup"><span data-stu-id="d63e7-172">As with installation, it can take several minutes to disable Automated Backup.</span></span>

> [!NOTE]
> <span data-ttu-id="d63e7-173">Zakázání a odinstalace agenta systému SQL Server IaaS neodebere dřív nakonfigurovaná nastavení spravovaného zálohování.</span><span class="sxs-lookup"><span data-stu-id="d63e7-173">Disabling and uninstalling the SQL Server IaaS Agent does not remove the previously configured Managed Backup settings.</span></span> <span data-ttu-id="d63e7-174">Měli byste zakázat automatizovaného zálohování před zakázáním nebo odinstalace agenta systému SQL Server IaaS.</span><span class="sxs-lookup"><span data-stu-id="d63e7-174">You should disable Automated Backup before disabling or uninstalling the SQL Server IaaS Agent.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="d63e7-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d63e7-175">Next steps</span></span>
<span data-ttu-id="d63e7-176">Automatizované zálohování nakonfiguruje spravovaného zálohování na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="d63e7-176">Automated Backup configures Managed Backup on Azure VMs.</span></span> <span data-ttu-id="d63e7-177">Proto je důležité [najdete v dokumentaci pro spravovanou zálohu](https://msdn.microsoft.com/library/dn449496.aspx) pochopit chování a důsledky.</span><span class="sxs-lookup"><span data-stu-id="d63e7-177">So it is important to [review the documentation for Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) to understand the behavior and implications.</span></span>

<span data-ttu-id="d63e7-178">Můžete najít další zálohování a obnovení pokyny pro SQL Server na virtuálních počítačích Azure v následujícím tématu: [zálohování a obnovení pro SQL Server v Azure Virtual Machines](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d63e7-178">You can find additional backup and restore guidance for SQL Server on Azure VMs in the following topic: [Backup and Restore for SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span></span>

<span data-ttu-id="d63e7-179">Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="d63e7-179">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md).</span></span>

<span data-ttu-id="d63e7-180">Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d63e7-180">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

