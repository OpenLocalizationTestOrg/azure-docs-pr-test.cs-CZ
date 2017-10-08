---
title: "aaaAutomated opravy pro virtuální počítače serveru SQL (Resource Manager) | Microsoft Docs"
description: "Vysvětluje funkci hello automatizované opravy pro SQL Server virtuální počítače běžící v Azure pomocí Správce prostředků."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 58232e92-318f-456b-8f0a-2201a541e08d
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 8bb8d0fb265e69d7bbf1fa047f5ceef02e7c56fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a><span data-ttu-id="fc6d2-103">Automatizované opravy pro SQL Server v Azure Virtual Machines (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="fc6d2-103">Automated Patching for SQL Server in Azure Virtual Machines (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fc6d2-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="fc6d2-104">Resource Manager</span></span>](virtual-machines-windows-sql-automated-patching.md)
> * [<span data-ttu-id="fc6d2-105">Classic</span><span class="sxs-lookup"><span data-stu-id="fc6d2-105">Classic</span></span>](../classic/sql-automated-patching.md)
> 
> 

<span data-ttu-id="fc6d2-106">Automatizovaných oprav určuje časové období údržby pro virtuální počítač Azure systémem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-106">Automated Patching establishes a maintenance window for an Azure Virtual Machine running SQL Server.</span></span> <span data-ttu-id="fc6d2-107">Automatické aktualizace lze nainstalovat pouze během tohoto časového období údržby.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-107">Automated Updates can only be installed during this maintenance window.</span></span> <span data-ttu-id="fc6d2-108">Pro systém SQL Server tento rescriction zajistí, že aktualizace systému a všechny přidružené restartuje dojít na nejlepší možný čas hello hello databáze.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-108">For SQL Server, this rescriction ensures that system updates and any associated restarts occur at hello best possible time for hello database.</span></span> <span data-ttu-id="fc6d2-109">Automatizovaných oprav závisí na hello [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="fc6d2-109">Automated Patching depends on hello [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="fc6d2-110">tooview hello classic verze tohoto článku, najdete v části [automatizované opravy pro SQL Server v Azure virtuální počítače Classic](../classic/sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="fc6d2-110">tooview hello classic version of this article, see [Automated Patching for SQL Server in Azure Virtual Machines Classic](../classic/sql-automated-patching.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fc6d2-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fc6d2-111">Prerequisites</span></span>
<span data-ttu-id="fc6d2-112">toouse automatizované opravy, vezměte v úvahu hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="fc6d2-112">toouse Automated Patching, consider hello following prerequisites:</span></span>

<span data-ttu-id="fc6d2-113">**Operační systém**:</span><span class="sxs-lookup"><span data-stu-id="fc6d2-113">**Operating System**:</span></span>

* <span data-ttu-id="fc6d2-114">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="fc6d2-114">Windows Server 2012</span></span>
* <span data-ttu-id="fc6d2-115">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="fc6d2-115">Windows Server 2012 R2</span></span>
* <span data-ttu-id="fc6d2-116">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="fc6d2-116">Windows Server 2016</span></span>

<span data-ttu-id="fc6d2-117">**Verze systému SQL Server**:</span><span class="sxs-lookup"><span data-stu-id="fc6d2-117">**SQL Server version**:</span></span>

* <span data-ttu-id="fc6d2-118">SQL Server 2012</span><span class="sxs-lookup"><span data-stu-id="fc6d2-118">SQL Server 2012</span></span>
* <span data-ttu-id="fc6d2-119">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="fc6d2-119">SQL Server 2014</span></span>
* <span data-ttu-id="fc6d2-120">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="fc6d2-120">SQL Server 2016</span></span>

<span data-ttu-id="fc6d2-121">**Prostředí Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="fc6d2-121">**Azure PowerShell**:</span></span>

* <span data-ttu-id="fc6d2-122">[Nainstalujte nejnovější příkazy prostředí Azure PowerShell hello](/powershell/azure/overview) Pokud máte v plánu tooconfigure automatizovaných oprav pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-122">[Install hello latest Azure PowerShell commands](/powershell/azure/overview) if you plan tooconfigure Automated Patching with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="fc6d2-123">Automatizovaných oprav spoléhá na hello rozšíření IaaS agenta systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-123">Automated Patching relies on hello SQL Server IaaS Agent Extension.</span></span> <span data-ttu-id="fc6d2-124">Aktuální SQL bitové kopie virtuálních počítačů Galerie přidejte toto rozšíření ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-124">Current SQL virtual machine gallery images add this extension by default.</span></span> <span data-ttu-id="fc6d2-125">Další informace najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="fc6d2-125">For more information, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>
> 
> 

## <a name="settings"></a><span data-ttu-id="fc6d2-126">Nastavení</span><span class="sxs-lookup"><span data-stu-id="fc6d2-126">Settings</span></span>
<span data-ttu-id="fc6d2-127">Hello následující tabulka popisuje možnosti hello, které mohou být konfigurovány pro automatizované opravy.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-127">hello following table describes hello options that can be configured for Automated Patching.</span></span> <span data-ttu-id="fc6d2-128">kroky skutečné konfigurace Hello lišit v závislosti na tom, zda používáte hello portál Azure nebo příkazů prostředí Windows PowerShell pro Azure.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-128">hello actual configuration steps vary depending on whether you use hello Azure portal or Azure Windows PowerShell commands.</span></span>

| <span data-ttu-id="fc6d2-129">Nastavení</span><span class="sxs-lookup"><span data-stu-id="fc6d2-129">Setting</span></span> | <span data-ttu-id="fc6d2-130">Možné hodnoty</span><span class="sxs-lookup"><span data-stu-id="fc6d2-130">Possible values</span></span> | <span data-ttu-id="fc6d2-131">Popis</span><span class="sxs-lookup"><span data-stu-id="fc6d2-131">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fc6d2-132">**Automatizované opravy**</span><span class="sxs-lookup"><span data-stu-id="fc6d2-132">**Automated Patching**</span></span> |<span data-ttu-id="fc6d2-133">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="fc6d2-133">Enable/Disable (Disabled)</span></span> |<span data-ttu-id="fc6d2-134">Povolí nebo zakáže automatizované opravy pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-134">Enables or disables Automated Patching for an Azure virtual machine.</span></span> |
| <span data-ttu-id="fc6d2-135">**Plán údržby.**</span><span class="sxs-lookup"><span data-stu-id="fc6d2-135">**Maintenance schedule**</span></span> |<span data-ttu-id="fc6d2-136">Každý den, pondělí, úterý, středu, čtvrtek a pátek, sobota, neděle</span><span class="sxs-lookup"><span data-stu-id="fc6d2-136">Everyday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday</span></span> |<span data-ttu-id="fc6d2-137">Hello plán pro stahování a instalace aktualizací s Windows, SQL Server a Microsoft pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-137">hello schedule for downloading and installing Windows, SQL Server, and Microsoft updates for your virtual machine.</span></span> |
| <span data-ttu-id="fc6d2-138">**Hodina spouštění údržby**</span><span class="sxs-lookup"><span data-stu-id="fc6d2-138">**Maintenance start hour**</span></span> |<span data-ttu-id="fc6d2-139">0-24</span><span class="sxs-lookup"><span data-stu-id="fc6d2-139">0-24</span></span> |<span data-ttu-id="fc6d2-140">Hello místní počáteční čas tooupdate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-140">hello local start time tooupdate hello virtual machine.</span></span> |
| <span data-ttu-id="fc6d2-141">**Doba trvání okna údržby**</span><span class="sxs-lookup"><span data-stu-id="fc6d2-141">**Maintenance window duration**</span></span> |<span data-ttu-id="fc6d2-142">30-180</span><span class="sxs-lookup"><span data-stu-id="fc6d2-142">30-180</span></span> |<span data-ttu-id="fc6d2-143">Hello počet minut povolené toocomplete hello stažení a instalace aktualizací.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-143">hello number of minutes permitted toocomplete hello download and installation of updates.</span></span> |
| <span data-ttu-id="fc6d2-144">**Oprava kategorie**</span><span class="sxs-lookup"><span data-stu-id="fc6d2-144">**Patch Category**</span></span> |<span data-ttu-id="fc6d2-145">Důležité</span><span class="sxs-lookup"><span data-stu-id="fc6d2-145">Important</span></span> |<span data-ttu-id="fc6d2-146">Hello kategorie aktualizace toodownload a instalaci.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-146">hello category of updates toodownload and install.</span></span> |

## <a name="configuration-in-hello-portal"></a><span data-ttu-id="fc6d2-147">Konfigurace v hello portálu</span><span class="sxs-lookup"><span data-stu-id="fc6d2-147">Configuration in hello Portal</span></span>
<span data-ttu-id="fc6d2-148">Můžete použít hello Azure portálu tooconfigure automatizovaných oprav při zřizování nebo pro existující virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-148">You can use hello Azure portal tooconfigure Automated Patching during provisioning or for existing VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="fc6d2-149">Nové virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="fc6d2-149">New VMs</span></span>
<span data-ttu-id="fc6d2-150">Použití hello Azure portálu tooconfigure automatizovaných oprav při vytváření nového virtuálního počítače SQL serveru v modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-150">Use hello Azure portal tooconfigure Automated Patching when you create a new SQL Server Virtual Machine in hello Resource Manager deployment model.</span></span>

<span data-ttu-id="fc6d2-151">V hello **nastavení systému SQL Server** vyberte **automatizované opravy**.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-151">In hello **SQL Server settings** blade, select **Automated patching**.</span></span> <span data-ttu-id="fc6d2-152">Hello následující Azure portálu snímek obrazovky ukazuje hello **automatizovaných oprav SQL** okno.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-152">hello following Azure portal screenshot shows hello **SQL Automated Patching** blade.</span></span>

![Automatizovaných oprav SQL na portálu Azure](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

<span data-ttu-id="fc6d2-154">Kontext, tématu hello dokončení na [zřizování virtuálního počítače s SQL serverem v Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="fc6d2-154">For context, see hello complete topic on [provisioning a SQL Server virtual machine in Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="fc6d2-155">Existující virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="fc6d2-155">Existing VMs</span></span>
<span data-ttu-id="fc6d2-156">Pro existující virtuální počítače systému SQL Server vyberte virtuální počítač systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-156">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="fc6d2-157">Potom vyberte hello **konfigurace systému SQL Server** části hello **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-157">Then select hello **SQL Server configuration** section of hello **Settings** blade.</span></span>

![Automatické opravy SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

<span data-ttu-id="fc6d2-159">V hello **konfigurace systému SQL Server** okně klikněte na tlačítko hello **upravit** tlačítka na hello automatizovaných oprav části.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-159">In hello **SQL Server configuration** blade, click hello **Edit** button in hello Automated patching section.</span></span>

![Konfigurace automatizovaných oprav SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

<span data-ttu-id="fc6d2-161">Po dokončení klikněte na tlačítko hello **OK** na konci hello hello tlačítko **konfigurace systému SQL Server** okno toosave změny.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-161">When finished, click hello **OK** button on hello bottom of hello **SQL Server configuration** blade toosave your changes.</span></span>

<span data-ttu-id="fc6d2-162">Chcete-li povolit automatizované opravy pro hello poprvé, nakonfiguruje Azure hello IaaS agenta systému SQL Server hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-162">If you are enabling Automated Patching for hello first time, Azure configures hello SQL Server IaaS Agent in hello background.</span></span> <span data-ttu-id="fc6d2-163">Během této doby nemusí zobrazit hello portálu Azure, je nakonfigurován automatizovaných oprav.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-163">During this time, hello Azure portal might not show that Automated Patching is configured.</span></span> <span data-ttu-id="fc6d2-164">Počkejte několik minut, než toobe hello agenta nainstalovat, nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-164">Wait several minutes for hello agent toobe installed, configured.</span></span> <span data-ttu-id="fc6d2-165">Po této hello Azure portal odráží hello nové nastavení.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-165">After that hello Azure portal reflects hello new settings.</span></span>

> [!NOTE]
> <span data-ttu-id="fc6d2-166">Můžete také nakonfigurovat automatizovaných oprav pomocí šablony.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-166">You can also configure Automated Patching using a template.</span></span> <span data-ttu-id="fc6d2-167">Další informace najdete v tématu [šablony Azure rychlý start pro automatizované opravy](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).</span><span class="sxs-lookup"><span data-stu-id="fc6d2-167">For more information, see [Azure quickstart template for Automated Patching](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).</span></span>
> 
> 

## <a name="configuration-with-powershell"></a><span data-ttu-id="fc6d2-168">Konfigurace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="fc6d2-168">Configuration with PowerShell</span></span>
<span data-ttu-id="fc6d2-169">Po zřízení virtuálního počítače SQL, pomocí prostředí PowerShell tooconfigure automatizovaných oprav.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-169">After provisioning your SQL VM, use PowerShell tooconfigure Automated Patching.</span></span>

<span data-ttu-id="fc6d2-170">V následujícím příkladu hello, prostředí PowerShell je použité tooconfigure automatizovaných oprav na existující virtuální počítač serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-170">In hello following example, PowerShell is used tooconfigure Automated Patching on an existing SQL Server VM.</span></span> <span data-ttu-id="fc6d2-171">Hello **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig** příkaz nakonfiguruje nové okno údržby pro automatické aktualizace.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-171">hello **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig** command configures a new maintenance window for automatic updates.</span></span>

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

<span data-ttu-id="fc6d2-172">Podle toho, v tomto příkladu, hello následující tabulka popisuje hello praktické vliv na hello cílový virtuální počítač Azure:</span><span class="sxs-lookup"><span data-stu-id="fc6d2-172">Based on this example, hello following table describes hello practical effect on hello target Azure VM:</span></span>

| <span data-ttu-id="fc6d2-173">Parametr</span><span class="sxs-lookup"><span data-stu-id="fc6d2-173">Parameter</span></span> | <span data-ttu-id="fc6d2-174">Efekt</span><span class="sxs-lookup"><span data-stu-id="fc6d2-174">Effect</span></span> |
| --- | --- |
| <span data-ttu-id="fc6d2-175">**DayOfWeek**</span><span class="sxs-lookup"><span data-stu-id="fc6d2-175">**DayOfWeek**</span></span> |<span data-ttu-id="fc6d2-176">Každý čtvrtek nainstalovány opravy.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-176">Patches installed every Thursday.</span></span> |
| <span data-ttu-id="fc6d2-177">**MaintenanceWindowStartingHour**</span><span class="sxs-lookup"><span data-stu-id="fc6d2-177">**MaintenanceWindowStartingHour**</span></span> |<span data-ttu-id="fc6d2-178">Začátek aktualizace na 11:00.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-178">Begin updates at 11:00am.</span></span> |
| <span data-ttu-id="fc6d2-179">**MaintenanceWindowsDuration**</span><span class="sxs-lookup"><span data-stu-id="fc6d2-179">**MaintenanceWindowsDuration**</span></span> |<span data-ttu-id="fc6d2-180">Během 120 minut musí být nainstalované opravy.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-180">Patches must be installed within 120 minutes.</span></span> <span data-ttu-id="fc6d2-181">Podle času zahájení hello, musí provést podle 1:00 pm.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-181">Based on hello start time, they must complete by 1:00pm.</span></span> |
| <span data-ttu-id="fc6d2-182">**PatchCategory**</span><span class="sxs-lookup"><span data-stu-id="fc6d2-182">**PatchCategory**</span></span> |<span data-ttu-id="fc6d2-183">Hello jedinou možnou nastavení pro tento parametr je **důležité**.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-183">hello only possible setting for this parameter is **Important**.</span></span> |

<span data-ttu-id="fc6d2-184">Ho může trvat několik minut tooinstall a nakonfigurujte hello IaaS agenta systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-184">It could take several minutes tooinstall and configure hello SQL Server IaaS Agent.</span></span>

<span data-ttu-id="fc6d2-185">toodisable automatizovaných oprav spuštění hello stejný skript bez hello **-povolit** parametr toohello **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig**.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-185">toodisable Automated Patching, run hello same script without hello **-Enable** parameter toohello **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig**.</span></span> <span data-ttu-id="fc6d2-186">Hello absenci hello **-povolit** parametr signály hello příkaz toodisable hello funkce.</span><span class="sxs-lookup"><span data-stu-id="fc6d2-186">hello absence of hello **-Enable** parameter signals hello command toodisable hello feature.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fc6d2-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fc6d2-187">Next steps</span></span>
<span data-ttu-id="fc6d2-188">Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="fc6d2-188">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

<span data-ttu-id="fc6d2-189">Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fc6d2-189">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

