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
# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a><span data-ttu-id="2dd51-104">Automatizované zálohování pro virtuální počítače SQL serveru 2014 (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="2dd51-104">Automated Backup for SQL Server 2014 Virtual Machines (Resource Manager)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2dd51-105">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="2dd51-105">SQL Server 2014</span></span>](virtual-machines-windows-sql-automated-backup.md)
> * [<span data-ttu-id="2dd51-106">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="2dd51-106">SQL Server 2016</span></span>](virtual-machines-windows-sql-automated-backup-v2.md)

<span data-ttu-id="2dd51-107">Automatizované zálohování automaticky nakonfiguruje [tooMicrosoft spravovaného zálohování Azure](https://msdn.microsoft.com/library/dn449496.aspx) pro všechny stávající a nové databáze na virtuální počítač Azure SQL Server 2014 Standard nebo Enterprise.</span><span class="sxs-lookup"><span data-stu-id="2dd51-107">Automated Backup automatically configures [Managed Backup tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) for all existing and new databases on an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> <span data-ttu-id="2dd51-108">To vám umožní tooconfigure standardní databázi zálohování, které využívají služby odolné Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="2dd51-108">This enables you tooconfigure regular database backups that utilize durable Azure blob storage.</span></span> <span data-ttu-id="2dd51-109">Automatizované zálohování závisí na hello [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="2dd51-109">Automated Backup depends on hello [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="2dd51-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2dd51-110">Prerequisites</span></span>
<span data-ttu-id="2dd51-111">toouse automatizované zálohování, vezměte v úvahu hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="2dd51-111">toouse Automated Backup, consider hello following prerequisites:</span></span>

<span data-ttu-id="2dd51-112">**Operační systém**:</span><span class="sxs-lookup"><span data-stu-id="2dd51-112">**Operating System**:</span></span>

- <span data-ttu-id="2dd51-113">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="2dd51-113">Windows Server 2012</span></span>
- <span data-ttu-id="2dd51-114">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="2dd51-114">Windows Server 2012 R2</span></span>
- <span data-ttu-id="2dd51-115">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="2dd51-115">Windows Server 2016</span></span>

<span data-ttu-id="2dd51-116">**Verzi nebo edici systému SQL Server**:</span><span class="sxs-lookup"><span data-stu-id="2dd51-116">**SQL Server version/edition**:</span></span>

- <span data-ttu-id="2dd51-117">SQL Server 2014 Standard</span><span class="sxs-lookup"><span data-stu-id="2dd51-117">SQL Server 2014 Standard</span></span>
- <span data-ttu-id="2dd51-118">SQL Server 2014 Enterprise</span><span class="sxs-lookup"><span data-stu-id="2dd51-118">SQL Server 2014 Enterprise</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2dd51-119">Automatizované zálohování funguje s SQL serverem 2014.</span><span class="sxs-lookup"><span data-stu-id="2dd51-119">Automated Backup works with SQL Server 2014.</span></span> <span data-ttu-id="2dd51-120">Pokud používáte SQL Server 2016, můžete použít tooback v2 automatizovaného zálohování do své databáze.</span><span class="sxs-lookup"><span data-stu-id="2dd51-120">If you are using SQL Server 2016, you can use Automated Backup v2 tooback up your databases.</span></span> <span data-ttu-id="2dd51-121">Další informace najdete v tématu [v2 automatizované zálohování pro SQL Server 2016 virtuální počítače Azure](virtual-machines-windows-sql-automated-backup-v2.md).</span><span class="sxs-lookup"><span data-stu-id="2dd51-121">For more information, see [Automated Backup v2 for SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).</span></span>

<span data-ttu-id="2dd51-122">**Konfigurace databáze**:</span><span class="sxs-lookup"><span data-stu-id="2dd51-122">**Database configuration**:</span></span>

- <span data-ttu-id="2dd51-123">Cílové databáze musí mít hello úplném modelu obnovení.</span><span class="sxs-lookup"><span data-stu-id="2dd51-123">Target databases must use hello full recovery model.</span></span> <span data-ttu-id="2dd51-124">Další informace o dopadu hello hello úplném modelu obnovení na zálohování najdete v tématu [zálohování pod hello úplný Model obnovení](https://technet.microsoft.com/library/ms190217.aspx).</span><span class="sxs-lookup"><span data-stu-id="2dd51-124">For more information about hello impact of hello full recovery model on backups, see [Backup Under hello Full Recovery Model](https://technet.microsoft.com/library/ms190217.aspx).</span></span>
- <span data-ttu-id="2dd51-125">Cílové databáze musí být na hello výchozí instanci SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="2dd51-125">Target databases must be on hello default SQL Server instance.</span></span> <span data-ttu-id="2dd51-126">Hello IaaS rozšíření systému SQL Server nepodporuje pojmenované instance.</span><span class="sxs-lookup"><span data-stu-id="2dd51-126">hello SQL Server IaaS Extension does not support named instances.</span></span>

<span data-ttu-id="2dd51-127">**Model nasazení Azure**:</span><span class="sxs-lookup"><span data-stu-id="2dd51-127">**Azure deployment model**:</span></span>

- <span data-ttu-id="2dd51-128">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2dd51-128">Resource Manager</span></span>

<span data-ttu-id="2dd51-129">**Prostředí Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="2dd51-129">**Azure PowerShell**:</span></span>

- <span data-ttu-id="2dd51-130">[Nainstalujte nejnovější příkazy prostředí Azure PowerShell hello](/powershell/azure/overview) Pokud máte v plánu tooconfigure automatizované zálohování pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2dd51-130">[Install hello latest Azure PowerShell commands](/powershell/azure/overview) if you plan tooconfigure Automated Backup with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="2dd51-131">Automatizované zálohování spoléhá na hello rozšíření IaaS agenta systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2dd51-131">Automated Backup relies on hello SQL Server IaaS Agent Extension.</span></span> <span data-ttu-id="2dd51-132">Aktuální SQL bitové kopie virtuálních počítačů Galerie přidejte toto rozšíření ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="2dd51-132">Current SQL virtual machine gallery images add this extension by default.</span></span> <span data-ttu-id="2dd51-133">Další informace najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="2dd51-133">For more information, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="2dd51-134">Nastavení</span><span class="sxs-lookup"><span data-stu-id="2dd51-134">Settings</span></span>

<span data-ttu-id="2dd51-135">Hello následující tabulka popisuje možnosti hello, které lze konfigurovat pro automatizované zálohování.</span><span class="sxs-lookup"><span data-stu-id="2dd51-135">hello following table describes hello options that can be configured for Automated Backup.</span></span> <span data-ttu-id="2dd51-136">kroky skutečné konfigurace Hello lišit v závislosti na tom, zda používáte hello portál Azure nebo příkazů prostředí Windows PowerShell pro Azure.</span><span class="sxs-lookup"><span data-stu-id="2dd51-136">hello actual configuration steps vary depending on whether you use hello Azure portal or Azure Windows PowerShell commands.</span></span>

| <span data-ttu-id="2dd51-137">Nastavení</span><span class="sxs-lookup"><span data-stu-id="2dd51-137">Setting</span></span> | <span data-ttu-id="2dd51-138">Rozsah (výchozí)</span><span class="sxs-lookup"><span data-stu-id="2dd51-138">Range (Default)</span></span> | <span data-ttu-id="2dd51-139">Popis</span><span class="sxs-lookup"><span data-stu-id="2dd51-139">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2dd51-140">**Automatizované zálohování**</span><span class="sxs-lookup"><span data-stu-id="2dd51-140">**Automated Backup**</span></span> | <span data-ttu-id="2dd51-141">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="2dd51-141">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="2dd51-142">Povolí nebo zakáže automatizované zálohování pro virtuální počítač Azure SQL Server 2014 Standard nebo Enterprise.</span><span class="sxs-lookup"><span data-stu-id="2dd51-142">Enables or disables Automated Backup for an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> |
| <span data-ttu-id="2dd51-143">**Doba uchování dat**</span><span class="sxs-lookup"><span data-stu-id="2dd51-143">**Retention Period**</span></span> | <span data-ttu-id="2dd51-144">1 až 30 dní (30 dní)</span><span class="sxs-lookup"><span data-stu-id="2dd51-144">1-30 days (30 days)</span></span> | <span data-ttu-id="2dd51-145">Hello počet dní tooretain zálohu.</span><span class="sxs-lookup"><span data-stu-id="2dd51-145">hello number of days tooretain a backup.</span></span> |
| <span data-ttu-id="2dd51-146">**Účet úložiště**</span><span class="sxs-lookup"><span data-stu-id="2dd51-146">**Storage Account**</span></span> | <span data-ttu-id="2dd51-147">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2dd51-147">Azure storage account</span></span> | <span data-ttu-id="2dd51-148">Toouse účet úložiště Azure pro ukládání souborů automatizovaného zálohování do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="2dd51-148">An Azure storage account toouse for storing Automated Backup files in blob storage.</span></span> <span data-ttu-id="2dd51-149">Kontejner se vytvoří v této toostore umístění všechny záložní soubory.</span><span class="sxs-lookup"><span data-stu-id="2dd51-149">A container is created at this location toostore all backup files.</span></span> <span data-ttu-id="2dd51-150">záložní soubor Hello zásady vytváření názvů zahrnuje hello date, time a název počítače.</span><span class="sxs-lookup"><span data-stu-id="2dd51-150">hello backup file naming convention includes hello date, time, and machine name.</span></span> |
| <span data-ttu-id="2dd51-151">**Šifrování**</span><span class="sxs-lookup"><span data-stu-id="2dd51-151">**Encryption**</span></span> | <span data-ttu-id="2dd51-152">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="2dd51-152">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="2dd51-153">Povolí nebo zakáže šifrování.</span><span class="sxs-lookup"><span data-stu-id="2dd51-153">Enables or disables encryption.</span></span> <span data-ttu-id="2dd51-154">Když je povolené šifrování, hello certifikátů používaných toorestore hello zálohování jsou umístěné v hello zadaný účet úložiště v hello stejné `automaticbackup` kontejneru pomocí hello stejné zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="2dd51-154">When encryption is enabled, hello certificates used toorestore hello backup are located in hello specified storage account in hello same `automaticbackup` container using hello same naming convention.</span></span> <span data-ttu-id="2dd51-155">Pokud se změní heslo hello, se toto heslo se vygeneruje nový certifikát, ale starý certifikát hello zůstane toorestore předchozí zálohy.</span><span class="sxs-lookup"><span data-stu-id="2dd51-155">If hello password changes, a new certificate is generated with that password, but hello old certificate remains toorestore prior backups.</span></span> |
| <span data-ttu-id="2dd51-156">**Heslo**</span><span class="sxs-lookup"><span data-stu-id="2dd51-156">**Password**</span></span> | <span data-ttu-id="2dd51-157">Heslo</span><span class="sxs-lookup"><span data-stu-id="2dd51-157">Password text</span></span> | <span data-ttu-id="2dd51-158">Heslo pro šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="2dd51-158">A password for encryption keys.</span></span> <span data-ttu-id="2dd51-159">Toto je pouze vyžaduje, pokud je povolené šifrování.</span><span class="sxs-lookup"><span data-stu-id="2dd51-159">This is only required if encryption is enabled.</span></span> <span data-ttu-id="2dd51-160">V pořadí toorestore šifrované zálohování, musíte mít hello správné heslo a související certifikátu, který byl použit v době hello hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="2dd51-160">In order toorestore an encrypted backup, you must have hello correct password and related certificate that was used at hello time hello backup was taken.</span></span> |

## <a name="configuration-in-hello-portal"></a><span data-ttu-id="2dd51-161">Konfigurace v hello portálu</span><span class="sxs-lookup"><span data-stu-id="2dd51-161">Configuration in hello Portal</span></span>

<span data-ttu-id="2dd51-162">Hello Azure portálu tooconfigure automatizované zálohování můžete použít při zřizování nebo pro existující SQL Server 2014 virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="2dd51-162">You can use hello Azure portal tooconfigure Automated Backup during provisioning or for existing SQL Server 2014 VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="2dd51-163">Nové virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="2dd51-163">New VMs</span></span>

<span data-ttu-id="2dd51-164">Při vytváření nového SQL serveru 2014 virtuálního počítače v modelu nasazení Resource Manager hello používejte hello Azure portálu tooconfigure automatizované zálohování.</span><span class="sxs-lookup"><span data-stu-id="2dd51-164">Use hello Azure portal tooconfigure Automated Backup when you create a new SQL Server 2014 Virtual Machine in hello Resource Manager deployment model.</span></span>

<span data-ttu-id="2dd51-165">V hello **nastavení systému SQL Server** vyberte **automatizované zálohování**.</span><span class="sxs-lookup"><span data-stu-id="2dd51-165">In hello **SQL Server settings** blade, select **Automated backup**.</span></span> <span data-ttu-id="2dd51-166">Hello následující Azure portálu snímek obrazovky ukazuje hello **automatizované zálohování SQL** okno.</span><span class="sxs-lookup"><span data-stu-id="2dd51-166">hello following Azure portal screenshot shows hello **SQL Automated Backup** blade.</span></span>

![Konfigurace automatického zálohování SQL na portálu Azure](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

<span data-ttu-id="2dd51-168">Kontext, tématu hello dokončení na [zřizování virtuálního počítače s SQL serverem v Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="2dd51-168">For context, see hello complete topic on [provisioning a SQL Server virtual machine in Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="2dd51-169">Existující virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="2dd51-169">Existing VMs</span></span>

<span data-ttu-id="2dd51-170">Pro existující virtuální počítače systému SQL Server vyberte virtuální počítač systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2dd51-170">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="2dd51-171">Potom vyberte hello **konfigurace systému SQL Server** části hello **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="2dd51-171">Then select hello **SQL Server configuration** section of hello **Settings** blade.</span></span>

![Automatizované zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

<span data-ttu-id="2dd51-173">V hello **konfigurace systému SQL Server** okně klikněte na tlačítko hello **upravit** tlačítka na hello automatizované zálohování části.</span><span class="sxs-lookup"><span data-stu-id="2dd51-173">In hello **SQL Server configuration** blade, click hello **Edit** button in hello Automated backup section.</span></span>

![Konfigurace automatického zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

<span data-ttu-id="2dd51-175">Po dokončení klikněte na tlačítko hello **OK** na konci hello hello tlačítko **konfigurace systému SQL Server** okno toosave změny.</span><span class="sxs-lookup"><span data-stu-id="2dd51-175">When finished, click hello **OK** button on hello bottom of hello **SQL Server configuration** blade toosave your changes.</span></span>

<span data-ttu-id="2dd51-176">Pokud povolíte automatizované zálohování pro hello poprvé, Azure automaticky nakonfiguruje hello IaaS agenta systému SQL Server hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="2dd51-176">If you are enabling Automated Backup for hello first time, Azure configures hello SQL Server IaaS Agent in hello background.</span></span> <span data-ttu-id="2dd51-177">Během této doby se nemusí zobrazit hello portál Azure, automatizované zálohování je nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="2dd51-177">During this time, hello Azure portal might not show that Automated Backup is configured.</span></span> <span data-ttu-id="2dd51-178">Počkejte několik minut, než toobe hello agenta nainstalovat, nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="2dd51-178">Wait several minutes for hello agent toobe installed, configured.</span></span> <span data-ttu-id="2dd51-179">Po této hello Azure bude odrážet portál hello nové nastavení.</span><span class="sxs-lookup"><span data-stu-id="2dd51-179">After that hello Azure portal will reflect hello new settings.</span></span>

> [!NOTE]
> <span data-ttu-id="2dd51-180">Můžete také nakonfigurovat automatizovaného zálohování pomocí šablony.</span><span class="sxs-lookup"><span data-stu-id="2dd51-180">You can also configure Automated Backup using a template.</span></span> <span data-ttu-id="2dd51-181">Další informace najdete v tématu [šablony Azure rychlý start pro automatizované zálohování](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).</span><span class="sxs-lookup"><span data-stu-id="2dd51-181">For more information, see [Azure quickstart template for Automated Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).</span></span>

## <a name="configuration-with-powershell"></a><span data-ttu-id="2dd51-182">Konfigurace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2dd51-182">Configuration with PowerShell</span></span>

<span data-ttu-id="2dd51-183">Pomocí prostředí PowerShell tooconfigure automatizované zálohování.</span><span class="sxs-lookup"><span data-stu-id="2dd51-183">You can use PowerShell tooconfigure Automated Backup.</span></span> <span data-ttu-id="2dd51-184">Než začnete, musíte provést tyto akce:</span><span class="sxs-lookup"><span data-stu-id="2dd51-184">Before you begin, you must:</span></span>

- <span data-ttu-id="2dd51-185">[Stáhněte a nainstalujte nejnovější prostředí Azure PowerShell text hello](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="2dd51-185">[Download and install hello latest Azure PowerShell](http://aka.ms/webpi-azps).</span></span>
- <span data-ttu-id="2dd51-186">Otevřete prostředí Windows PowerShell a přidružit svůj účet.</span><span class="sxs-lookup"><span data-stu-id="2dd51-186">Open Windows PowerShell and associate it with your account.</span></span> <span data-ttu-id="2dd51-187">Můžete to provést pomocí následujících kroků hello v hello [konfigurovat předplatné](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) části hello zřizování tématu.</span><span class="sxs-lookup"><span data-stu-id="2dd51-187">You can do this by following hello steps in hello [Configure your subscription](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) section of hello provisioning topic.</span></span>

### <a name="install-hello-sql-iaas-extension"></a><span data-ttu-id="2dd51-188">Nainstalujte hello IaaS rozšíření systému SQL</span><span class="sxs-lookup"><span data-stu-id="2dd51-188">Install hello SQL IaaS Extension</span></span>
<span data-ttu-id="2dd51-189">Pokud jste zřídili virtuálního počítače systému SQL Server z hello portálu Azure, by měl hello IaaS rozšíření systému SQL Server již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="2dd51-189">If you provisioned a SQL Server virtual machine from hello Azure portal, hello SQL Server IaaS Extension should already be installed.</span></span> <span data-ttu-id="2dd51-190">Můžete určit, pokud je nainstalovaná pro virtuální počítač voláním **Get-AzureRmVM** příkaz a prozkoumání hello **rozšíření** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2dd51-190">You can determine if it is installed for your VM by calling **Get-AzureRmVM** command and examining hello **Extensions** property.</span></span>

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions
```

<span data-ttu-id="2dd51-191">Pokud je nainstalovaná hello rozšíření IaaS agenta systému SQL Server, měli byste vidět, že je uveden jako "SqlIaaSAgent" nebo "SQLIaaSExtension".</span><span class="sxs-lookup"><span data-stu-id="2dd51-191">If hello SQL Server IaaS Agent extension is installed, you should see it listed as “SqlIaaSAgent” or “SQLIaaSExtension”.</span></span> <span data-ttu-id="2dd51-192">**Stav zřizování** pro hello rozšíření by měl také zobrazit, "Succeeded".</span><span class="sxs-lookup"><span data-stu-id="2dd51-192">**ProvisioningState** for hello extension should also show “Succeeded”.</span></span>

<span data-ttu-id="2dd51-193">Pokud není nainstalována nebo se nezdařilo toobe zřízený, můžete ho nainstalovat s hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="2dd51-193">If it is not installed or failed toobe provisioned, you can install it with hello following command.</span></span> <span data-ttu-id="2dd51-194">Přidání toohello virtuálních počítačů název nebo skupině prostředků, musíte také určit oblasti hello (**$region**) umístěnou ve virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2dd51-194">In addition toohello VM name and resource group, you must also specify hello region (**$region**) that your VM is located in.</span></span>

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region
```

### <span data-ttu-id="2dd51-195"><a id="verifysettings"></a>Ověřte aktuální nastavení</span><span class="sxs-lookup"><span data-stu-id="2dd51-195"><a id="verifysettings"></a> Verify current settings</span></span>

<span data-ttu-id="2dd51-196">Pokud jste povolili automatizované zálohování při zřizování, můžete použít PowerShell toocheck aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2dd51-196">If you enabled automated backup during provisioning, you can use PowerShell toocheck your current configuration.</span></span> <span data-ttu-id="2dd51-197">Spustit hello **Get-AzureRmVMSqlServerExtension** příkaz a zkontrolujte hello **AutoBackupSettings** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="2dd51-197">Run hello **Get-AzureRmVMSqlServerExtension** command and examine hello **AutoBackupSettings** property:</span></span>

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

<span data-ttu-id="2dd51-198">Měli byste obdržet výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="2dd51-198">You should get output similar toohello following:</span></span>

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

<span data-ttu-id="2dd51-199">Pokud vaše výstup ukazuje, že **povolit** je nastaven příliš**False**, pak máte tooenable automatizované zálohování.</span><span class="sxs-lookup"><span data-stu-id="2dd51-199">If your output shows that **Enable** is set too**False**, then you have tooenable automated backup.</span></span> <span data-ttu-id="2dd51-200">Hello Dobrá zpráva je, že jste povolit a konfigurovat automatizovaného zálohování v hello stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="2dd51-200">hello good news is that you enable and configure Automated Backup in hello same way.</span></span> <span data-ttu-id="2dd51-201">Viz další část hello tyto informace.</span><span class="sxs-lookup"><span data-stu-id="2dd51-201">See hello next section for this information.</span></span>

> [!NOTE] 
> <span data-ttu-id="2dd51-202">Pokud zaškrtnete nastavení hello okamžitě po provedení změny, je možné, že budete mít zpět hello původní hodnoty konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2dd51-202">If you check hello settings immediately after making a change, it is possible that you will get back hello old configuration values.</span></span> <span data-ttu-id="2dd51-203">Počkejte několik minut a zkontrolujte nastavení hello znovu toomake jistotu, že byly použity změny.</span><span class="sxs-lookup"><span data-stu-id="2dd51-203">Wait a few minutes and check hello settings again toomake sure that your changes were applied.</span></span>

### <a name="configure-automated-backup"></a><span data-ttu-id="2dd51-204">Konfigurace automatického zálohování</span><span class="sxs-lookup"><span data-stu-id="2dd51-204">Configure Automated Backup</span></span>
<span data-ttu-id="2dd51-205">Můžete PowerShell tooenable automatizované zálohování, jakož i toomodify jeho konfiguraci a chování kdykoli.</span><span class="sxs-lookup"><span data-stu-id="2dd51-205">You can use PowerShell tooenable Automated Backup as well as toomodify its configuration and behavior at any time.</span></span>

<span data-ttu-id="2dd51-206">Nejdřív vyberte nebo vytvořte účet úložiště pro hello záložní soubory.</span><span class="sxs-lookup"><span data-stu-id="2dd51-206">First, select or create a storage account for hello backup files.</span></span> <span data-ttu-id="2dd51-207">Hello následující skript vybere účet úložiště nebo ji vytvoří, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="2dd51-207">hello following script selects a storage account or creates it if it does not exist.</span></span>

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
> <span data-ttu-id="2dd51-208">Automatizované zálohování nepodporuje ukládání záloh v storage úrovně premium, ale může trvat zálohy z disků virtuálních počítačů, které používají úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="2dd51-208">Automated Backup does not support storing backups in premium storage, but it can take backups from VM disks which use Premium Storage.</span></span>

<span data-ttu-id="2dd51-209">Potom pomocí hello **New-AzureRmVMSqlServerAutoBackupConfig** příkaz tooenable a nakonfigurujte hello automatizovaného zálohování nastavení toostore zálohy v hello účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="2dd51-209">Then use hello **New-AzureRmVMSqlServerAutoBackupConfig** command tooenable and configure hello Automated Backup settings toostore backups in hello Azure storage account.</span></span> <span data-ttu-id="2dd51-210">V tomto příkladu jsou hello zálohy nastaveny toobe uchovávat 10 dní.</span><span class="sxs-lookup"><span data-stu-id="2dd51-210">In this example, hello backups are set toobe retained for 10 days.</span></span> <span data-ttu-id="2dd51-211">Hello druhý příkaz, **Set-AzureRmVMSqlServerExtension**, aktualizace hello zadaný virtuální počítač Azure s těmito nastaveními.</span><span class="sxs-lookup"><span data-stu-id="2dd51-211">hello second command, **Set-AzureRmVMSqlServerExtension**, updates hello specified Azure VM with these settings.</span></span>

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

<span data-ttu-id="2dd51-212">Ho může trvat několik minut tooinstall a nakonfigurujte hello IaaS agenta systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2dd51-212">It could take several minutes tooinstall and configure hello SQL Server IaaS Agent.</span></span>

> [!NOTE]
> <span data-ttu-id="2dd51-213">Existují další nastavení pro **New-AzureRmVMSqlServerAutoBackupConfig** které se týkají pouze tooSQL Server 2016 a automatizované zálohování v2.</span><span class="sxs-lookup"><span data-stu-id="2dd51-213">There are other settings for **New-AzureRmVMSqlServerAutoBackupConfig** that apply only tooSQL Server 2016 and Automated Backup v2.</span></span> <span data-ttu-id="2dd51-214">SQL Server 2014 nepodporuje hello následující nastavení: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**,  **FullBackupStartHour**, **FullBackupWindowInHours**, a **LogBackupFrequencyInMinutes**.</span><span class="sxs-lookup"><span data-stu-id="2dd51-214">SQL Server 2014 does not support hello following settings: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**, **FullBackupStartHour**, **FullBackupWindowInHours**, and **LogBackupFrequencyInMinutes**.</span></span> <span data-ttu-id="2dd51-215">Když zkusíte tooconfigure tato nastavení na virtuálním počítači, SQL Server 2014, se nezobrazí žádná chyba, ale nastavení hello získat nebyly použity.</span><span class="sxs-lookup"><span data-stu-id="2dd51-215">If you attempt tooconfigure these settings on a SQL Server 2014 virtual machine, there is no error, but hello settings do not get applied.</span></span> <span data-ttu-id="2dd51-216">Pokud chcete tato nastavení na virtuálním počítači, SQL Server 2016 toouse, najdete v části [v2 automatizované zálohování pro SQL Server 2016 virtuální počítače Azure](virtual-machines-windows-sql-automated-backup-v2.md).</span><span class="sxs-lookup"><span data-stu-id="2dd51-216">If you want toouse these settings on a SQL Server 2016 virtual machine, see [Automated Backup v2 for SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).</span></span>

<span data-ttu-id="2dd51-217">šifrování tooenable upravit hello předchozí skript toopass hello **EnableEncryption** parametr společně s heslo (zabezpečený řetězec) pro hello **CertificatePassword** parametr.</span><span class="sxs-lookup"><span data-stu-id="2dd51-217">tooenable encryption, modify hello previous script toopass hello **EnableEncryption** parameter along with a password (secure string) for hello **CertificatePassword** parameter.</span></span> <span data-ttu-id="2dd51-218">Hello následující skript umožňuje hello nastavení automatizovaného zálohování v předchozím příkladu hello a přidá šifrování.</span><span class="sxs-lookup"><span data-stu-id="2dd51-218">hello following script enables hello Automated Backup settings in hello previous example and adds encryption.</span></span>

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

<span data-ttu-id="2dd51-219">nastavení se použijí, tooconfirm [ověřte konfiguraci automatizovaného zálohování hello](#verifysettings).</span><span class="sxs-lookup"><span data-stu-id="2dd51-219">tooconfirm your settings are applied, [verify hello Automated Backup configuration](#verifysettings).</span></span>

### <a name="disable-automated-backup"></a><span data-ttu-id="2dd51-220">Zakázat automatizované zálohování</span><span class="sxs-lookup"><span data-stu-id="2dd51-220">Disable Automated Backup</span></span>

<span data-ttu-id="2dd51-221">toodisable automatizované zálohování, spusťte hello stejný skript bez hello **-povolit** parametr toohello **New-AzureRmVMSqlServerAutoBackupConfig** příkaz.</span><span class="sxs-lookup"><span data-stu-id="2dd51-221">toodisable Automated Backup, run hello same script without hello **-Enable** parameter toohello **New-AzureRmVMSqlServerAutoBackupConfig** command.</span></span> <span data-ttu-id="2dd51-222">Hello absenci hello **-povolit** parametr signály hello příkaz toodisable hello funkce.</span><span class="sxs-lookup"><span data-stu-id="2dd51-222">hello absence of hello **-Enable** parameter signals hello command toodisable hello feature.</span></span> <span data-ttu-id="2dd51-223">Stejně jako u instalace může trvat několik minut toodisable automatizované zálohování.</span><span class="sxs-lookup"><span data-stu-id="2dd51-223">As with installation, it can take several minutes toodisable Automated Backup.</span></span>

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a><span data-ttu-id="2dd51-224">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="2dd51-224">Example script</span></span>

<span data-ttu-id="2dd51-225">Hello následující skript představuje sadu proměnných, můžete přizpůsobit tooenable a konfiguraci automatizovaného zálohování pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2dd51-225">hello following script provides a set of variables that you can customize tooenable and configure Automated Backup for your VM.</span></span> <span data-ttu-id="2dd51-226">Ve vašem případě bude pravděpodobně nutné toocustomize hello skript podle svých požadavků.</span><span class="sxs-lookup"><span data-stu-id="2dd51-226">In your case, you might need toocustomize hello script based on your requirements.</span></span> <span data-ttu-id="2dd51-227">Například by mít toomake změny, pokud byste chtěli toodisable hello zálohu systémové databáze nebo povolit šifrování.</span><span class="sxs-lookup"><span data-stu-id="2dd51-227">For example, you would have toomake changes if you wanted toodisable hello backup of system databases or enable encryption.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="2dd51-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2dd51-228">Next steps</span></span>

<span data-ttu-id="2dd51-229">Automatizované zálohování nakonfiguruje spravovaného zálohování na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="2dd51-229">Automated Backup configures Managed Backup on Azure VMs.</span></span> <span data-ttu-id="2dd51-230">Proto je důležité příliš[hello v dokumentaci pro spravovanou zálohu](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello chování a důsledky.</span><span class="sxs-lookup"><span data-stu-id="2dd51-230">So it is important too[review hello documentation for Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello behavior and implications.</span></span>

<span data-ttu-id="2dd51-231">Můžete najít další zálohování a obnovení pokyny pro SQL Server na virtuálních počítačích Azure v následujícím tématu hello: [zálohování a obnovení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="2dd51-231">You can find additional backup and restore guidance for SQL Server on Azure VMs in hello following topic: [Backup and Restore for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span></span>

<span data-ttu-id="2dd51-232">Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="2dd51-232">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

<span data-ttu-id="2dd51-233">Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2dd51-233">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

