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
# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a><span data-ttu-id="c086e-104">Automatizované zálohování pro virtuální počítače SQL serveru 2014 (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="c086e-104">Automated Backup for SQL Server 2014 Virtual Machines (Resource Manager)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c086e-105">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="c086e-105">SQL Server 2014</span></span>](virtual-machines-windows-sql-automated-backup.md)
> * [<span data-ttu-id="c086e-106">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="c086e-106">SQL Server 2016</span></span>](virtual-machines-windows-sql-automated-backup-v2.md)

<span data-ttu-id="c086e-107">Automatizované zálohování automaticky nakonfiguruje [spravovaného zálohování Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) pro všechny stávající a nové databáze na virtuální počítač Azure SQL Server 2014 Standard nebo Enterprise.</span><span class="sxs-lookup"><span data-stu-id="c086e-107">Automated Backup automatically configures [Managed Backup to Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) for all existing and new databases on an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> <span data-ttu-id="c086e-108">To umožňuje nakonfigurovat standardní databázi zálohování, které využívají služby odolné Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="c086e-108">This enables you to configure regular database backups that utilize durable Azure blob storage.</span></span> <span data-ttu-id="c086e-109">Automatizované zálohování závisí na [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="c086e-109">Automated Backup depends on the [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="c086e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c086e-110">Prerequisites</span></span>
<span data-ttu-id="c086e-111">Pomocí automatizovaného zálohování, zvažte následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="c086e-111">To use Automated Backup, consider the following prerequisites:</span></span>

<span data-ttu-id="c086e-112">**Operační systém**:</span><span class="sxs-lookup"><span data-stu-id="c086e-112">**Operating System**:</span></span>

- <span data-ttu-id="c086e-113">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="c086e-113">Windows Server 2012</span></span>
- <span data-ttu-id="c086e-114">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="c086e-114">Windows Server 2012 R2</span></span>
- <span data-ttu-id="c086e-115">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="c086e-115">Windows Server 2016</span></span>

<span data-ttu-id="c086e-116">**Verzi nebo edici systému SQL Server**:</span><span class="sxs-lookup"><span data-stu-id="c086e-116">**SQL Server version/edition**:</span></span>

- <span data-ttu-id="c086e-117">SQL Server 2014 Standard</span><span class="sxs-lookup"><span data-stu-id="c086e-117">SQL Server 2014 Standard</span></span>
- <span data-ttu-id="c086e-118">SQL Server 2014 Enterprise</span><span class="sxs-lookup"><span data-stu-id="c086e-118">SQL Server 2014 Enterprise</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c086e-119">Automatizované zálohování funguje s SQL serverem 2014.</span><span class="sxs-lookup"><span data-stu-id="c086e-119">Automated Backup works with SQL Server 2014.</span></span> <span data-ttu-id="c086e-120">Pokud používáte SQL Server 2016, můžete k zálohování databází v2 automatizovaného zálohování.</span><span class="sxs-lookup"><span data-stu-id="c086e-120">If you are using SQL Server 2016, you can use Automated Backup v2 to back up your databases.</span></span> <span data-ttu-id="c086e-121">Další informace najdete v tématu [v2 automatizované zálohování pro SQL Server 2016 virtuální počítače Azure](virtual-machines-windows-sql-automated-backup-v2.md).</span><span class="sxs-lookup"><span data-stu-id="c086e-121">For more information, see [Automated Backup v2 for SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).</span></span>

<span data-ttu-id="c086e-122">**Konfigurace databáze**:</span><span class="sxs-lookup"><span data-stu-id="c086e-122">**Database configuration**:</span></span>

- <span data-ttu-id="c086e-123">Cílové databáze musí mít úplném modelu obnovení.</span><span class="sxs-lookup"><span data-stu-id="c086e-123">Target databases must use the full recovery model.</span></span> <span data-ttu-id="c086e-124">Další informace o vlivu úplném modelu obnovení na zálohování najdete v tématu [zálohování v části the úplný Model obnovení](https://technet.microsoft.com/library/ms190217.aspx).</span><span class="sxs-lookup"><span data-stu-id="c086e-124">For more information about the impact of the full recovery model on backups, see [Backup Under the Full Recovery Model](https://technet.microsoft.com/library/ms190217.aspx).</span></span>
- <span data-ttu-id="c086e-125">Cílové databáze musí být na výchozí instanci SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="c086e-125">Target databases must be on the default SQL Server instance.</span></span> <span data-ttu-id="c086e-126">IaaS rozšíření systému SQL Server nepodporuje pojmenované instance.</span><span class="sxs-lookup"><span data-stu-id="c086e-126">The SQL Server IaaS Extension does not support named instances.</span></span>

<span data-ttu-id="c086e-127">**Model nasazení Azure**:</span><span class="sxs-lookup"><span data-stu-id="c086e-127">**Azure deployment model**:</span></span>

- <span data-ttu-id="c086e-128">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c086e-128">Resource Manager</span></span>

<span data-ttu-id="c086e-129">**Prostředí Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="c086e-129">**Azure PowerShell**:</span></span>

- <span data-ttu-id="c086e-130">[Nainstalujte nejnovější příkazy prostředí Azure PowerShell](/powershell/azure/overview) Pokud chcete konfigurovat automatizovaného zálohování pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c086e-130">[Install the latest Azure PowerShell commands](/powershell/azure/overview) if you plan to configure Automated Backup with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="c086e-131">Automatizované zálohování spoléhá na rozšíření agenta systému SQL Server IaaS.</span><span class="sxs-lookup"><span data-stu-id="c086e-131">Automated Backup relies on the SQL Server IaaS Agent Extension.</span></span> <span data-ttu-id="c086e-132">Aktuální SQL bitové kopie virtuálních počítačů Galerie přidejte toto rozšíření ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="c086e-132">Current SQL virtual machine gallery images add this extension by default.</span></span> <span data-ttu-id="c086e-133">Další informace najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="c086e-133">For more information, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="c086e-134">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c086e-134">Settings</span></span>

<span data-ttu-id="c086e-135">Následující tabulka popisuje možnosti, které lze konfigurovat pro automatizované zálohování.</span><span class="sxs-lookup"><span data-stu-id="c086e-135">The following table describes the options that can be configured for Automated Backup.</span></span> <span data-ttu-id="c086e-136">Skutečné konfiguračních kroků se liší v závislosti na tom, zda používáte portál Azure nebo příkazů prostředí Windows PowerShell pro Azure.</span><span class="sxs-lookup"><span data-stu-id="c086e-136">The actual configuration steps vary depending on whether you use the Azure portal or Azure Windows PowerShell commands.</span></span>

| <span data-ttu-id="c086e-137">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c086e-137">Setting</span></span> | <span data-ttu-id="c086e-138">Rozsah (výchozí)</span><span class="sxs-lookup"><span data-stu-id="c086e-138">Range (Default)</span></span> | <span data-ttu-id="c086e-139">Popis</span><span class="sxs-lookup"><span data-stu-id="c086e-139">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c086e-140">**Automatizované zálohování**</span><span class="sxs-lookup"><span data-stu-id="c086e-140">**Automated Backup**</span></span> | <span data-ttu-id="c086e-141">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="c086e-141">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="c086e-142">Povolí nebo zakáže automatizované zálohování pro virtuální počítač Azure SQL Server 2014 Standard nebo Enterprise.</span><span class="sxs-lookup"><span data-stu-id="c086e-142">Enables or disables Automated Backup for an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> |
| <span data-ttu-id="c086e-143">**Doba uchování dat**</span><span class="sxs-lookup"><span data-stu-id="c086e-143">**Retention Period**</span></span> | <span data-ttu-id="c086e-144">1 až 30 dní (30 dní)</span><span class="sxs-lookup"><span data-stu-id="c086e-144">1-30 days (30 days)</span></span> | <span data-ttu-id="c086e-145">Počet dní, které chcete zachovat zálohu.</span><span class="sxs-lookup"><span data-stu-id="c086e-145">The number of days to retain a backup.</span></span> |
| <span data-ttu-id="c086e-146">**Účet úložiště**</span><span class="sxs-lookup"><span data-stu-id="c086e-146">**Storage Account**</span></span> | <span data-ttu-id="c086e-147">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="c086e-147">Azure storage account</span></span> | <span data-ttu-id="c086e-148">Účet úložiště Azure pro ukládání souborů automatizovaného zálohování do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c086e-148">An Azure storage account to use for storing Automated Backup files in blob storage.</span></span> <span data-ttu-id="c086e-149">Kontejner se vytvoří v tomto umístění pro uložení všechny záložní soubory.</span><span class="sxs-lookup"><span data-stu-id="c086e-149">A container is created at this location to store all backup files.</span></span> <span data-ttu-id="c086e-150">Zásady vytváření názvů záložní soubor obsahuje datum, čas a název počítače.</span><span class="sxs-lookup"><span data-stu-id="c086e-150">The backup file naming convention includes the date, time, and machine name.</span></span> |
| <span data-ttu-id="c086e-151">**Šifrování**</span><span class="sxs-lookup"><span data-stu-id="c086e-151">**Encryption**</span></span> | <span data-ttu-id="c086e-152">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="c086e-152">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="c086e-153">Povolí nebo zakáže šifrování.</span><span class="sxs-lookup"><span data-stu-id="c086e-153">Enables or disables encryption.</span></span> <span data-ttu-id="c086e-154">Když je povolené šifrování, certifikátů používaných pro obnovení zálohy jsou umístěné v zadaný účet úložiště ve stejné `automaticbackup` kontejneru pomocí stejné zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="c086e-154">When encryption is enabled, the certificates used to restore the backup are located in the specified storage account in the same `automaticbackup` container using the same naming convention.</span></span> <span data-ttu-id="c086e-155">Pokud se změní heslo, se toto heslo se vygeneruje nový certifikát, ale pořád starý certifikát pro obnovení předchozí zálohy.</span><span class="sxs-lookup"><span data-stu-id="c086e-155">If the password changes, a new certificate is generated with that password, but the old certificate remains to restore prior backups.</span></span> |
| <span data-ttu-id="c086e-156">**Heslo**</span><span class="sxs-lookup"><span data-stu-id="c086e-156">**Password**</span></span> | <span data-ttu-id="c086e-157">Heslo</span><span class="sxs-lookup"><span data-stu-id="c086e-157">Password text</span></span> | <span data-ttu-id="c086e-158">Heslo pro šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="c086e-158">A password for encryption keys.</span></span> <span data-ttu-id="c086e-159">Toto je pouze vyžaduje, pokud je povolené šifrování.</span><span class="sxs-lookup"><span data-stu-id="c086e-159">This is only required if encryption is enabled.</span></span> <span data-ttu-id="c086e-160">Chcete-li obnovit šifrované zálohování, musí mít správné heslo a související certifikátu, který byl použit v době, kdy bylo provedeno zálohování.</span><span class="sxs-lookup"><span data-stu-id="c086e-160">In order to restore an encrypted backup, you must have the correct password and related certificate that was used at the time the backup was taken.</span></span> |

## <a name="configuration-in-the-portal"></a><span data-ttu-id="c086e-161">Konfigurace na portálu</span><span class="sxs-lookup"><span data-stu-id="c086e-161">Configuration in the Portal</span></span>

<span data-ttu-id="c086e-162">Na portálu Azure můžete použít ke konfiguraci automatizovaného zálohování při zřizování nebo pro existující SQL Server 2014 virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="c086e-162">You can use the Azure portal to configure Automated Backup during provisioning or for existing SQL Server 2014 VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="c086e-163">Nové virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c086e-163">New VMs</span></span>

<span data-ttu-id="c086e-164">Použití portálu Azure ke konfiguraci automatizovaného zálohování při vytváření nového SQL serveru 2014 virtuálního počítače v modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c086e-164">Use the Azure portal to configure Automated Backup when you create a new SQL Server 2014 Virtual Machine in the Resource Manager deployment model.</span></span>

<span data-ttu-id="c086e-165">V **nastavení systému SQL Server** vyberte **automatizované zálohování**.</span><span class="sxs-lookup"><span data-stu-id="c086e-165">In the **SQL Server settings** blade, select **Automated backup**.</span></span> <span data-ttu-id="c086e-166">Následující Azure portálu snímek obrazovky ukazuje **automatizované zálohování SQL** okno.</span><span class="sxs-lookup"><span data-stu-id="c086e-166">The following Azure portal screenshot shows the **SQL Automated Backup** blade.</span></span>

![Konfigurace automatického zálohování SQL na portálu Azure](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

<span data-ttu-id="c086e-168">Kontext, naleznete v tématu dokončení na [zřizování virtuálního počítače s SQL serverem v Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="c086e-168">For context, see the complete topic on [provisioning a SQL Server virtual machine in Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="c086e-169">Existující virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c086e-169">Existing VMs</span></span>

<span data-ttu-id="c086e-170">Pro existující virtuální počítače systému SQL Server vyberte virtuální počítač systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c086e-170">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="c086e-171">Vyberte **konfigurace systému SQL Server** části **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="c086e-171">Then select the **SQL Server configuration** section of the **Settings** blade.</span></span>

![Automatizované zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

<span data-ttu-id="c086e-173">V **konfigurace systému SQL Server** okně klikněte **upravit** tlačítko v části Automatické zálohování.</span><span class="sxs-lookup"><span data-stu-id="c086e-173">In the **SQL Server configuration** blade, click the **Edit** button in the Automated backup section.</span></span>

![Konfigurace automatického zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

<span data-ttu-id="c086e-175">Po dokončení klikněte **OK** tlačítko v dolní části **konfigurace systému SQL Server** okno a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="c086e-175">When finished, click the **OK** button on the bottom of the **SQL Server configuration** blade to save your changes.</span></span>

<span data-ttu-id="c086e-176">Jestliže povolíte automatizované zálohování poprvé, nakonfiguruje Azure IaaS Agent serveru SQL Server na pozadí.</span><span class="sxs-lookup"><span data-stu-id="c086e-176">If you are enabling Automated Backup for the first time, Azure configures the SQL Server IaaS Agent in the background.</span></span> <span data-ttu-id="c086e-177">Během této doby nemusí zobrazit na portálu Azure, že automatizované zálohování je nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="c086e-177">During this time, the Azure portal might not show that Automated Backup is configured.</span></span> <span data-ttu-id="c086e-178">Počkejte několik minut, než agent, který se má nainstalovat, nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="c086e-178">Wait several minutes for the agent to be installed, configured.</span></span> <span data-ttu-id="c086e-179">Potom se projeví na portálu Azure nové nastavení.</span><span class="sxs-lookup"><span data-stu-id="c086e-179">After that the Azure portal will reflect the new settings.</span></span>

> [!NOTE]
> <span data-ttu-id="c086e-180">Můžete také nakonfigurovat automatizovaného zálohování pomocí šablony.</span><span class="sxs-lookup"><span data-stu-id="c086e-180">You can also configure Automated Backup using a template.</span></span> <span data-ttu-id="c086e-181">Další informace najdete v tématu [šablony Azure rychlý start pro automatizované zálohování](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).</span><span class="sxs-lookup"><span data-stu-id="c086e-181">For more information, see [Azure quickstart template for Automated Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).</span></span>

## <a name="configuration-with-powershell"></a><span data-ttu-id="c086e-182">Konfigurace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c086e-182">Configuration with PowerShell</span></span>

<span data-ttu-id="c086e-183">Prostředí PowerShell můžete použít ke konfiguraci automatizovaného zálohování.</span><span class="sxs-lookup"><span data-stu-id="c086e-183">You can use PowerShell to configure Automated Backup.</span></span> <span data-ttu-id="c086e-184">Než začnete, musíte provést tyto akce:</span><span class="sxs-lookup"><span data-stu-id="c086e-184">Before you begin, you must:</span></span>

- <span data-ttu-id="c086e-185">[Stáhněte a nainstalujte nejnovější prostředí Azure PowerShell](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="c086e-185">[Download and install the latest Azure PowerShell](http://aka.ms/webpi-azps).</span></span>
- <span data-ttu-id="c086e-186">Otevřete prostředí Windows PowerShell a přidružit svůj účet.</span><span class="sxs-lookup"><span data-stu-id="c086e-186">Open Windows PowerShell and associate it with your account.</span></span> <span data-ttu-id="c086e-187">To provedete podle kroků v [konfigurovat předplatné](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) zřizování tématu.</span><span class="sxs-lookup"><span data-stu-id="c086e-187">You can do this by following the steps in the [Configure your subscription](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) section of the provisioning topic.</span></span>

### <a name="install-the-sql-iaas-extension"></a><span data-ttu-id="c086e-188">Nainstalujte rozšíření IaaS SQL</span><span class="sxs-lookup"><span data-stu-id="c086e-188">Install the SQL IaaS Extension</span></span>
<span data-ttu-id="c086e-189">Pokud zřízení virtuálního počítače s SQL serverem na portálu Azure IaaS rozšíření systému SQL Server musí být již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="c086e-189">If you provisioned a SQL Server virtual machine from the Azure portal, the SQL Server IaaS Extension should already be installed.</span></span> <span data-ttu-id="c086e-190">Můžete určit, pokud je nainstalovaná pro virtuální počítač voláním **Get-AzureRmVM** příkaz a prozkoumání **rozšíření** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c086e-190">You can determine if it is installed for your VM by calling **Get-AzureRmVM** command and examining the **Extensions** property.</span></span>

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions
```

<span data-ttu-id="c086e-191">Pokud je nainstalovaná rozšíření IaaS agenta systému SQL Server, měli byste vidět, že je uveden jako "SqlIaaSAgent" nebo "SQLIaaSExtension".</span><span class="sxs-lookup"><span data-stu-id="c086e-191">If the SQL Server IaaS Agent extension is installed, you should see it listed as “SqlIaaSAgent” or “SQLIaaSExtension”.</span></span> <span data-ttu-id="c086e-192">**Stav zřizování** pro rozšíření by měl také zobrazit, "Succeeded".</span><span class="sxs-lookup"><span data-stu-id="c086e-192">**ProvisioningState** for the extension should also show “Succeeded”.</span></span>

<span data-ttu-id="c086e-193">Pokud není nainstalovaná nebo se nepovedlo zřídit, můžete ho nainstalovat pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="c086e-193">If it is not installed or failed to be provisioned, you can install it with the following command.</span></span> <span data-ttu-id="c086e-194">Kromě názvu a prostředek skupiny virtuálních počítačů, je nutné také zadat oblast (**$region**) umístěnou ve virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c086e-194">In addition to the VM name and resource group, you must also specify the region (**$region**) that your VM is located in.</span></span>

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region
```

### <span data-ttu-id="c086e-195"><a id="verifysettings"></a>Ověřte aktuální nastavení</span><span class="sxs-lookup"><span data-stu-id="c086e-195"><a id="verifysettings"></a> Verify current settings</span></span>

<span data-ttu-id="c086e-196">Pokud jste povolili automatizované zálohování při zřizování, můžete použít PowerShell ke kontrole aktuální konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c086e-196">If you enabled automated backup during provisioning, you can use PowerShell to check your current configuration.</span></span> <span data-ttu-id="c086e-197">Spustit **Get-AzureRmVMSqlServerExtension** příkaz a zkontrolujte **AutoBackupSettings** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="c086e-197">Run the **Get-AzureRmVMSqlServerExtension** command and examine the **AutoBackupSettings** property:</span></span>

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

<span data-ttu-id="c086e-198">Měli byste obdržet výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="c086e-198">You should get output similar to the following:</span></span>

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

<span data-ttu-id="c086e-199">Pokud vaše výstup ukazuje, že **povolit** je nastaven na **False**, budete muset povolit automatizované zálohování.</span><span class="sxs-lookup"><span data-stu-id="c086e-199">If your output shows that **Enable** is set to **False**, then you have to enable automated backup.</span></span> <span data-ttu-id="c086e-200">Dobrá zpráva je, že můžete povolit a nakonfigurovat automatizovaného zálohování stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="c086e-200">The good news is that you enable and configure Automated Backup in the same way.</span></span> <span data-ttu-id="c086e-201">Najdete v další části pro tyto informace.</span><span class="sxs-lookup"><span data-stu-id="c086e-201">See the next section for this information.</span></span>

> [!NOTE] 
> <span data-ttu-id="c086e-202">Pokud zaškrtnete okamžitě po provedení změny nastavení, je možné, zobrazí se zpět původní hodnoty konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c086e-202">If you check the settings immediately after making a change, it is possible that you will get back the old configuration values.</span></span> <span data-ttu-id="c086e-203">Počkejte několik minut a zkontrolujte nastavení znovu a ujistěte se, že byly použity změny.</span><span class="sxs-lookup"><span data-stu-id="c086e-203">Wait a few minutes and check the settings again to make sure that your changes were applied.</span></span>

### <a name="configure-automated-backup"></a><span data-ttu-id="c086e-204">Konfigurace automatického zálohování</span><span class="sxs-lookup"><span data-stu-id="c086e-204">Configure Automated Backup</span></span>
<span data-ttu-id="c086e-205">Prostředí PowerShell můžete použít k povolení automatizované zálohování i, kdykoli upravit jeho konfiguraci a chování.</span><span class="sxs-lookup"><span data-stu-id="c086e-205">You can use PowerShell to enable Automated Backup as well as to modify its configuration and behavior at any time.</span></span>

<span data-ttu-id="c086e-206">Nejdřív vyberte nebo vytvořte účet úložiště pro záložní soubory.</span><span class="sxs-lookup"><span data-stu-id="c086e-206">First, select or create a storage account for the backup files.</span></span> <span data-ttu-id="c086e-207">Následující skript vybere účet úložiště, nebo ji vytvoří, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c086e-207">The following script selects a storage account or creates it if it does not exist.</span></span>

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
> <span data-ttu-id="c086e-208">Automatizované zálohování nepodporuje ukládání záloh v storage úrovně premium, ale může trvat zálohy z disků virtuálních počítačů, které používají úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="c086e-208">Automated Backup does not support storing backups in premium storage, but it can take backups from VM disks which use Premium Storage.</span></span>

<span data-ttu-id="c086e-209">Potom pomocí **New-AzureRmVMSqlServerAutoBackupConfig** příkaz pro povolení a konfigurace nastavení automatizovaného zálohování pro ukládání záloh v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="c086e-209">Then use the **New-AzureRmVMSqlServerAutoBackupConfig** command to enable and configure the Automated Backup settings to store backups in the Azure storage account.</span></span> <span data-ttu-id="c086e-210">V tomto příkladu jsou zálohy nastaveny pro zachování 10 dní.</span><span class="sxs-lookup"><span data-stu-id="c086e-210">In this example, the backups are set to be retained for 10 days.</span></span> <span data-ttu-id="c086e-211">V druhém příkazu **Set-AzureRmVMSqlServerExtension**, aktualizuje zadaný virtuální počítač Azure s těmito nastaveními.</span><span class="sxs-lookup"><span data-stu-id="c086e-211">The second command, **Set-AzureRmVMSqlServerExtension**, updates the specified Azure VM with these settings.</span></span>

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

<span data-ttu-id="c086e-212">Ho může trvat několik minut k instalaci a konfiguraci IaaS Agent serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c086e-212">It could take several minutes to install and configure the SQL Server IaaS Agent.</span></span>

> [!NOTE]
> <span data-ttu-id="c086e-213">Existují další nastavení pro **New-AzureRmVMSqlServerAutoBackupConfig** , se vztahují pouze na SQL Server 2016 a automatizované zálohování v2.</span><span class="sxs-lookup"><span data-stu-id="c086e-213">There are other settings for **New-AzureRmVMSqlServerAutoBackupConfig** that apply only to SQL Server 2016 and Automated Backup v2.</span></span> <span data-ttu-id="c086e-214">SQL Server 2014 nepodporuje následující nastavení: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**, **FullBackupStartHour**, **FullBackupWindowInHours**, a **LogBackupFrequencyInMinutes**.</span><span class="sxs-lookup"><span data-stu-id="c086e-214">SQL Server 2014 does not support the following settings: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**, **FullBackupStartHour**, **FullBackupWindowInHours**, and **LogBackupFrequencyInMinutes**.</span></span> <span data-ttu-id="c086e-215">Pokud se pokusíte nakonfigurovat tato nastavení na virtuální počítač SQL Server 2014, se nezobrazí žádná chyba, ale nastavení získat nebyly použity.</span><span class="sxs-lookup"><span data-stu-id="c086e-215">If you attempt to configure these settings on a SQL Server 2014 virtual machine, there is no error, but the settings do not get applied.</span></span> <span data-ttu-id="c086e-216">Pokud chcete použít tato nastavení na virtuálním počítači, SQL Server 2016, přečtěte si téma [v2 automatizované zálohování pro SQL Server 2016 virtuální počítače Azure](virtual-machines-windows-sql-automated-backup-v2.md).</span><span class="sxs-lookup"><span data-stu-id="c086e-216">If you want to use these settings on a SQL Server 2016 virtual machine, see [Automated Backup v2 for SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).</span></span>

<span data-ttu-id="c086e-217">Chcete-li povolit šifrování, změňte předchozí skript, který chcete předat **EnableEncryption** společně s heslo (zabezpečený řetězec) pro parametr **CertificatePassword** parametr.</span><span class="sxs-lookup"><span data-stu-id="c086e-217">To enable encryption, modify the previous script to pass the **EnableEncryption** parameter along with a password (secure string) for the **CertificatePassword** parameter.</span></span> <span data-ttu-id="c086e-218">Následující skript umožňuje nastavení automatizovaného zálohování v předchozím příkladu a přidá šifrování.</span><span class="sxs-lookup"><span data-stu-id="c086e-218">The following script enables the Automated Backup settings in the previous example and adds encryption.</span></span>

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

<span data-ttu-id="c086e-219">Potvrďte nastavení se použijí, [ověřit konfiguraci automatizovaného zálohování](#verifysettings).</span><span class="sxs-lookup"><span data-stu-id="c086e-219">To confirm your settings are applied, [verify the Automated Backup configuration](#verifysettings).</span></span>

### <a name="disable-automated-backup"></a><span data-ttu-id="c086e-220">Zakázat automatizované zálohování</span><span class="sxs-lookup"><span data-stu-id="c086e-220">Disable Automated Backup</span></span>

<span data-ttu-id="c086e-221">Pokud chcete zakázat automatizovaného zálohování, spusťte stejný skriptu bez **-povolit** parametru **New-AzureRmVMSqlServerAutoBackupConfig** příkaz.</span><span class="sxs-lookup"><span data-stu-id="c086e-221">To disable Automated Backup, run the same script without the **-Enable** parameter to the **New-AzureRmVMSqlServerAutoBackupConfig** command.</span></span> <span data-ttu-id="c086e-222">Neexistence **-povolit** parametr signály příkaz funkci zakážete.</span><span class="sxs-lookup"><span data-stu-id="c086e-222">The absence of the **-Enable** parameter signals the command to disable the feature.</span></span> <span data-ttu-id="c086e-223">Stejně jako u instalace, se může trvat několik minut zakázat automatizovaného zálohování.</span><span class="sxs-lookup"><span data-stu-id="c086e-223">As with installation, it can take several minutes to disable Automated Backup.</span></span>

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a><span data-ttu-id="c086e-224">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="c086e-224">Example script</span></span>

<span data-ttu-id="c086e-225">Následující skript představuje sadu proměnných, které můžete přizpůsobit povolit a konfigurovat automatizované zálohování pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c086e-225">The following script provides a set of variables that you can customize to enable and configure Automated Backup for your VM.</span></span> <span data-ttu-id="c086e-226">V váš případ může být nutné přizpůsobit skript podle svých požadavků.</span><span class="sxs-lookup"><span data-stu-id="c086e-226">In your case, you might need to customize the script based on your requirements.</span></span> <span data-ttu-id="c086e-227">Například nutné provést změny, pokud chcete zakázat zálohování databází systému nebo povolit šifrování.</span><span class="sxs-lookup"><span data-stu-id="c086e-227">For example, you would have to make changes if you wanted to disable the backup of system databases or enable encryption.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="c086e-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c086e-228">Next steps</span></span>

<span data-ttu-id="c086e-229">Automatizované zálohování nakonfiguruje spravovaného zálohování na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="c086e-229">Automated Backup configures Managed Backup on Azure VMs.</span></span> <span data-ttu-id="c086e-230">Proto je důležité [najdete v dokumentaci pro spravovanou zálohu](https://msdn.microsoft.com/library/dn449496.aspx) pochopit chování a důsledky.</span><span class="sxs-lookup"><span data-stu-id="c086e-230">So it is important to [review the documentation for Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) to understand the behavior and implications.</span></span>

<span data-ttu-id="c086e-231">Můžete najít další zálohování a obnovení pokyny pro SQL Server na virtuálních počítačích Azure v následujícím tématu: [zálohování a obnovení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="c086e-231">You can find additional backup and restore guidance for SQL Server on Azure VMs in the following topic: [Backup and Restore for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span></span>

<span data-ttu-id="c086e-232">Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="c086e-232">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

<span data-ttu-id="c086e-233">Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c086e-233">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

