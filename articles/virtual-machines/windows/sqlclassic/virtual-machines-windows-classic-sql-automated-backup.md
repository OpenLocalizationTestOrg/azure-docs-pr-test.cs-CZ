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
# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a><span data-ttu-id="6d809-103">Automatizované zálohování pro SQL Server na virtuálních počítačích Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="6d809-103">Automated Backup for SQL Server in Azure Virtual Machines (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6d809-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6d809-104">Resource Manager</span></span>](../sql/virtual-machines-windows-sql-automated-backup.md)
> * [<span data-ttu-id="6d809-105">Classic</span><span class="sxs-lookup"><span data-stu-id="6d809-105">Classic</span></span>](../classic/sql-automated-backup.md)
> 
> 

<span data-ttu-id="6d809-106">Automatizované zálohování automaticky nakonfiguruje [tooMicrosoft spravovaného zálohování Azure](https://msdn.microsoft.com/library/dn449496.aspx) pro všechny stávající a nové databáze na virtuální počítač Azure SQL Server 2014 Standard nebo Enterprise.</span><span class="sxs-lookup"><span data-stu-id="6d809-106">Automated Backup automatically configures [Managed Backup tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) for all existing and new databases on an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> <span data-ttu-id="6d809-107">To vám umožní tooconfigure standardní databázi zálohování, které využívají služby odolné Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="6d809-107">This enables you tooconfigure regular database backups that utilize durable Azure blob storage.</span></span> <span data-ttu-id="6d809-108">Automatizované zálohování závisí na hello [rozšíření agenta systému SQL Server IaaS](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6d809-108">Automated Backup depends on hello [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="6d809-109">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="6d809-109">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="6d809-110">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="6d809-110">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="6d809-111">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="6d809-111">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="6d809-112">tooview hello Resource Manager verze tohoto článku, najdete v části [automatizované zálohování pro SQL Server ve službě Správce prostředků virtuálních počítačů Azure](../sql/virtual-machines-windows-sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="6d809-112">tooview hello Resource Manager version of this article, see [Automated Backup for SQL Server in Azure Virtual Machines Resource Manager](../sql/virtual-machines-windows-sql-automated-backup.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6d809-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6d809-113">Prerequisites</span></span>
<span data-ttu-id="6d809-114">toouse automatizované zálohování, vezměte v úvahu hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="6d809-114">toouse Automated Backup, consider hello following prerequisites:</span></span>

<span data-ttu-id="6d809-115">**Operační systém**:</span><span class="sxs-lookup"><span data-stu-id="6d809-115">**Operating System**:</span></span>

* <span data-ttu-id="6d809-116">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="6d809-116">Windows Server 2012</span></span>
* <span data-ttu-id="6d809-117">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="6d809-117">Windows Server 2012 R2</span></span>
* <span data-ttu-id="6d809-118">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="6d809-118">Windows Server 2016</span></span>

<span data-ttu-id="6d809-119">**Verzi nebo edici systému SQL Server**:</span><span class="sxs-lookup"><span data-stu-id="6d809-119">**SQL Server version/edition**:</span></span>

* <span data-ttu-id="6d809-120">SQL Server 2014 Standard</span><span class="sxs-lookup"><span data-stu-id="6d809-120">SQL Server 2014 Standard</span></span>
* <span data-ttu-id="6d809-121">SQL Server 2014 Enterprise</span><span class="sxs-lookup"><span data-stu-id="6d809-121">SQL Server 2014 Enterprise</span></span>

> [!NOTE]
> <span data-ttu-id="6d809-122">SQL Server 2016 není dosud podporována pro automatizované zálohování.</span><span class="sxs-lookup"><span data-stu-id="6d809-122">SQL Server 2016 is not yet supported for Automated Backup.</span></span>
> 
> 

<span data-ttu-id="6d809-123">**Konfigurace databáze**:</span><span class="sxs-lookup"><span data-stu-id="6d809-123">**Database configuration**:</span></span>

* <span data-ttu-id="6d809-124">Cílové databáze musí mít hello úplném modelu obnovení.</span><span class="sxs-lookup"><span data-stu-id="6d809-124">Target databases must use hello full recovery model.</span></span>

<span data-ttu-id="6d809-125">**Prostředí Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="6d809-125">**Azure PowerShell**:</span></span>

* <span data-ttu-id="6d809-126">[Nainstalujte nejnovější příkazy prostředí Azure PowerShell hello](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6d809-126">[Install hello latest Azure PowerShell commands](/powershell/azure/overview).</span></span>

<span data-ttu-id="6d809-127">**Rozšíření systému SQL Server IaaS**:</span><span class="sxs-lookup"><span data-stu-id="6d809-127">**SQL Server IaaS Extension**:</span></span>

* <span data-ttu-id="6d809-128">[Nainstalujte hello rozšíření systému SQL Server IaaS](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="6d809-128">[Install hello SQL Server IaaS Extension](../classic/sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="6d809-129">Nastavení</span><span class="sxs-lookup"><span data-stu-id="6d809-129">Settings</span></span>
<span data-ttu-id="6d809-130">Hello následující tabulka popisuje možnosti hello, které lze konfigurovat pro automatizované zálohování.</span><span class="sxs-lookup"><span data-stu-id="6d809-130">hello following table describes hello options that can be configured for Automated Backup.</span></span> <span data-ttu-id="6d809-131">Pro klasické virtuální počítače je nutné použít PowerShell tooconfigure těchto nastavení.</span><span class="sxs-lookup"><span data-stu-id="6d809-131">For classic VMs, you must use PowerShell tooconfigure these settings.</span></span>

| <span data-ttu-id="6d809-132">Nastavení</span><span class="sxs-lookup"><span data-stu-id="6d809-132">Setting</span></span> | <span data-ttu-id="6d809-133">Rozsah (výchozí)</span><span class="sxs-lookup"><span data-stu-id="6d809-133">Range (Default)</span></span> | <span data-ttu-id="6d809-134">Popis</span><span class="sxs-lookup"><span data-stu-id="6d809-134">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6d809-135">**Automatizované zálohování**</span><span class="sxs-lookup"><span data-stu-id="6d809-135">**Automated Backup**</span></span> |<span data-ttu-id="6d809-136">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="6d809-136">Enable/Disable (Disabled)</span></span> |<span data-ttu-id="6d809-137">Povolí nebo zakáže automatizované zálohování pro virtuální počítač Azure SQL Server 2014 Standard nebo Enterprise.</span><span class="sxs-lookup"><span data-stu-id="6d809-137">Enables or disables Automated Backup for an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> |
| <span data-ttu-id="6d809-138">**Doba uchování dat**</span><span class="sxs-lookup"><span data-stu-id="6d809-138">**Retention Period**</span></span> |<span data-ttu-id="6d809-139">1 až 30 dní (30 dní)</span><span class="sxs-lookup"><span data-stu-id="6d809-139">1-30 days (30 days)</span></span> |<span data-ttu-id="6d809-140">Hello počet dní tooretain zálohu.</span><span class="sxs-lookup"><span data-stu-id="6d809-140">hello number of days tooretain a backup.</span></span> |
| <span data-ttu-id="6d809-141">**Účet úložiště**</span><span class="sxs-lookup"><span data-stu-id="6d809-141">**Storage Account**</span></span> |<span data-ttu-id="6d809-142">Účet úložiště Azure (hello účtu úložiště vytvořeném pro hello zadané virtuálního počítače)</span><span class="sxs-lookup"><span data-stu-id="6d809-142">Azure storage account (hello storage account created for hello specified VM)</span></span> |<span data-ttu-id="6d809-143">Toouse účet úložiště Azure pro ukládání souborů automatizovaného zálohování do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="6d809-143">An Azure storage account toouse for storing Automated Backup files in blob storage.</span></span> <span data-ttu-id="6d809-144">Kontejner se vytvoří v této toostore umístění všechny záložní soubory.</span><span class="sxs-lookup"><span data-stu-id="6d809-144">A container is created at this location toostore all backup files.</span></span> <span data-ttu-id="6d809-145">záložní soubor Hello zásady vytváření názvů zahrnuje hello date, time a název počítače.</span><span class="sxs-lookup"><span data-stu-id="6d809-145">hello backup file naming convention includes hello date, time, and machine name.</span></span> |
| <span data-ttu-id="6d809-146">**Šifrování**</span><span class="sxs-lookup"><span data-stu-id="6d809-146">**Encryption**</span></span> |<span data-ttu-id="6d809-147">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="6d809-147">Enable/Disable (Disabled)</span></span> |<span data-ttu-id="6d809-148">Povolí nebo zakáže šifrování.</span><span class="sxs-lookup"><span data-stu-id="6d809-148">Enables or disables encryption.</span></span> <span data-ttu-id="6d809-149">Když je povolené šifrování, hello certifikáty používané toorestore hello zálohování jsou umístěné v hello zadaný účet úložiště v hello pomocí stejné automaticbackup kontejneru hello stejné zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="6d809-149">When encryption is enabled, hello certificates used toorestore hello backup are located in hello specified storage account in hello same automaticbackup container using hello same naming convention.</span></span> <span data-ttu-id="6d809-150">Pokud se změní heslo hello, se toto heslo se vygeneruje nový certifikát, ale starý certifikát hello zůstane toorestore předchozí zálohy.</span><span class="sxs-lookup"><span data-stu-id="6d809-150">If hello password changes, a new certificate is generated with that password, but hello old certificate remains toorestore prior backups.</span></span> |
| <span data-ttu-id="6d809-151">**Heslo**</span><span class="sxs-lookup"><span data-stu-id="6d809-151">**Password**</span></span> |<span data-ttu-id="6d809-152">Text heslo, (None)</span><span class="sxs-lookup"><span data-stu-id="6d809-152">Password text (None)</span></span> |<span data-ttu-id="6d809-153">Heslo pro šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="6d809-153">A password for encryption keys.</span></span> <span data-ttu-id="6d809-154">Toto je pouze vyžaduje, pokud je povolené šifrování.</span><span class="sxs-lookup"><span data-stu-id="6d809-154">This is only required if encryption is enabled.</span></span> <span data-ttu-id="6d809-155">V pořadí toorestore šifrované zálohování, musíte mít hello správné heslo a související certifikátu, který byl použit v době hello hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="6d809-155">In order toorestore an encrypted backup, you must have hello correct password and related certificate that was used at hello time hello backup was taken.</span></span> | <span data-ttu-id="6d809-156">**Systém zálohování databáze**</span><span class="sxs-lookup"><span data-stu-id="6d809-156">**Backup system databases**</span></span> | <span data-ttu-id="6d809-157">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="6d809-157">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="6d809-158">Proveďte úplné zálohování Master, Model a databázi MSDB</span><span class="sxs-lookup"><span data-stu-id="6d809-158">Take full backups of Master, Model, and MSDB</span></span> |
| <span data-ttu-id="6d809-159">**Konfigurovat plán zálohování**</span><span class="sxs-lookup"><span data-stu-id="6d809-159">**Configure backup schedule**</span></span> | <span data-ttu-id="6d809-160">Ruční nebo automatické (Automated)</span><span class="sxs-lookup"><span data-stu-id="6d809-160">Manual/Automated (Automated)</span></span> | <span data-ttu-id="6d809-161">Vyberte **automatizovaná** tooautomatically provést úplné a protokolu zálohy založené na protokolu růst.</span><span class="sxs-lookup"><span data-stu-id="6d809-161">Select **Automated** tooautomatically take full and log backups based on log growth.</span></span> <span data-ttu-id="6d809-162">Vyberte **ruční** toospecify hello plán pro úplnou a protokolu zálohování.</span><span class="sxs-lookup"><span data-stu-id="6d809-162">Select **Manual** toospecify hello schedule for full and log backups.</span></span> |

## <a name="configuration-with-powershell"></a><span data-ttu-id="6d809-163">Konfigurace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6d809-163">Configuration with PowerShell</span></span>
<span data-ttu-id="6d809-164">V následující příklad PowerShell text hello automatizované zálohování je nakonfigurován pro existující virtuální počítač SQL Server 2014.</span><span class="sxs-lookup"><span data-stu-id="6d809-164">In hello following PowerShell example, Automated Backup is configured for an existing SQL Server 2014 VM.</span></span> <span data-ttu-id="6d809-165">Hello **New-AzureVMSqlServerAutoBackupConfig** příkaz nakonfiguruje hello automatizovaného zálohování nastavení toostore zálohy v účtu úložiště Azure hello určené proměnnou hello $storageaccount.</span><span class="sxs-lookup"><span data-stu-id="6d809-165">hello **New-AzureVMSqlServerAutoBackupConfig** command configures hello Automated Backup settings toostore backups in hello Azure storage account specified by hello $storageaccount variable.</span></span> <span data-ttu-id="6d809-166">Tyto zálohy bude uchovávat 10 dní.</span><span class="sxs-lookup"><span data-stu-id="6d809-166">These backups will be retained for 10 days.</span></span> <span data-ttu-id="6d809-167">Hello **Set-AzureVMSqlServerExtension** příkaz aktualizace hello zadaný virtuální počítač Azure s těmito nastaveními.</span><span class="sxs-lookup"><span data-stu-id="6d809-167">hello **Set-AzureVMSqlServerExtension** command updates hello specified Azure VM with these settings.</span></span>

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

<span data-ttu-id="6d809-168">Ho může trvat několik minut tooinstall a nakonfigurujte hello IaaS agenta systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6d809-168">It could take several minutes tooinstall and configure hello SQL Server IaaS Agent.</span></span>

<span data-ttu-id="6d809-169">tooenable šifrování, upravte hello předchozí skript toopass hello EnableEncryption parametr společně s heslo (zabezpečený řetězec) pro parametr CertificatePassword hello.</span><span class="sxs-lookup"><span data-stu-id="6d809-169">tooenable encryption, modify hello previous script toopass hello EnableEncryption parameter along with a password (secure string) for hello CertificatePassword parameter.</span></span> <span data-ttu-id="6d809-170">Hello následující skript umožňuje hello nastavení automatizovaného zálohování v předchozím příkladu hello a přidá šifrování.</span><span class="sxs-lookup"><span data-stu-id="6d809-170">hello following script enables hello Automated Backup settings in hello previous example and adds encryption.</span></span>

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

<span data-ttu-id="6d809-171">toodisable automatické zálohování, spusťte hello stejný skript bez hello **-povolit** parametr toohello **New-AzureVMSqlServerAutoBackupConfig**.</span><span class="sxs-lookup"><span data-stu-id="6d809-171">toodisable automatic backup, run hello same script without hello **-Enable** parameter toohello **New-AzureVMSqlServerAutoBackupConfig**.</span></span> <span data-ttu-id="6d809-172">Stejně jako u instalace může trvat několik minut toodisable automatizované zálohování.</span><span class="sxs-lookup"><span data-stu-id="6d809-172">As with installation, it can take several minutes toodisable Automated Backup.</span></span>

> [!NOTE]
> <span data-ttu-id="6d809-173">Zakázání a odinstalace agenta systému SQL Server IaaS hello neodebere hello dříve nakonfigurované nastavení spravovaného zálohování.</span><span class="sxs-lookup"><span data-stu-id="6d809-173">Disabling and uninstalling hello SQL Server IaaS Agent does not remove hello previously configured Managed Backup settings.</span></span> <span data-ttu-id="6d809-174">Měli byste zakázat automatizovaného zálohování před zakázáním nebo odinstalací hello IaaS agenta systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6d809-174">You should disable Automated Backup before disabling or uninstalling hello SQL Server IaaS Agent.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="6d809-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6d809-175">Next steps</span></span>
<span data-ttu-id="6d809-176">Automatizované zálohování nakonfiguruje spravovaného zálohování na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="6d809-176">Automated Backup configures Managed Backup on Azure VMs.</span></span> <span data-ttu-id="6d809-177">Proto je důležité příliš[hello v dokumentaci pro spravovanou zálohu](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello chování a důsledky.</span><span class="sxs-lookup"><span data-stu-id="6d809-177">So it is important too[review hello documentation for Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello behavior and implications.</span></span>

<span data-ttu-id="6d809-178">Můžete najít další zálohování a obnovení pokyny pro SQL Server na virtuálních počítačích Azure v následujícím tématu hello: [zálohování a obnovení pro SQL Server v Azure Virtual Machines](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6d809-178">You can find additional backup and restore guidance for SQL Server on Azure VMs in hello following topic: [Backup and Restore for SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span></span>

<span data-ttu-id="6d809-179">Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="6d809-179">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md).</span></span>

<span data-ttu-id="6d809-180">Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6d809-180">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

