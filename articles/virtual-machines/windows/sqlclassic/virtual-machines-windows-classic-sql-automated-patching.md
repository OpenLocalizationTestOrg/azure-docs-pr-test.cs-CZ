---
title: "aaaAutomated opravy pro virtuální počítače serveru SQL (klasické) | Microsoft Docs"
description: "Vysvětluje funkci hello automatizované opravy pro SQL Server virtuální počítače běžící v Azure pomocí režimu hello nasazení classic."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 737b2f65-08b9-4f54-b867-e987730265a8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 2ef06b95705fc457605d6eb2fbc0afd490321843
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a><span data-ttu-id="e8821-103">Automatizované opravy pro SQL Server na virtuálních počítačích Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="e8821-103">Automated Patching for SQL Server in Azure Virtual Machines (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e8821-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e8821-104">Resource Manager</span></span>](../sql/virtual-machines-windows-sql-automated-patching.md)
> * [<span data-ttu-id="e8821-105">Classic</span><span class="sxs-lookup"><span data-stu-id="e8821-105">Classic</span></span>](../classic/sql-automated-patching.md)
> 
> 

<span data-ttu-id="e8821-106">Automatizovaných oprav určuje časové období údržby pro virtuální počítač Azure systémem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e8821-106">Automated Patching establishes a maintenance window for an Azure Virtual Machine running SQL Server.</span></span> <span data-ttu-id="e8821-107">Automatické aktualizace lze nainstalovat pouze během tohoto časového období údržby.</span><span class="sxs-lookup"><span data-stu-id="e8821-107">Automated Updates can only be installed during this maintenance window.</span></span> <span data-ttu-id="e8821-108">Pro systém SQL Server tím se zajistí, že aktualizace systému a všechny přidružené restartuje dojít na nejlepší možný čas hello hello databáze.</span><span class="sxs-lookup"><span data-stu-id="e8821-108">For SQL Server, this ensures that system updates and any associated restarts occur at hello best possible time for hello database.</span></span> <span data-ttu-id="e8821-109">Automatizovaných oprav závisí na hello [rozšíření agenta systému SQL Server IaaS](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="e8821-109">Automated Patching depends on hello [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="e8821-110">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="e8821-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="e8821-111">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="e8821-111">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="e8821-112">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="e8821-112">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="e8821-113">tooview hello Resource Manager verze tohoto článku, najdete v části [automatizované opravy pro SQL Server ve službě Správce prostředků virtuálních počítačů Azure](../sql/virtual-machines-windows-sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="e8821-113">tooview hello Resource Manager version of this article, see [Automated Patching for SQL Server in Azure Virtual Machines Resource Manager](../sql/virtual-machines-windows-sql-automated-patching.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8821-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e8821-114">Prerequisites</span></span>
<span data-ttu-id="e8821-115">toouse automatizované opravy, vezměte v úvahu hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="e8821-115">toouse Automated Patching, consider hello following prerequisites:</span></span>

<span data-ttu-id="e8821-116">**Operační systém**:</span><span class="sxs-lookup"><span data-stu-id="e8821-116">**Operating System**:</span></span>

* <span data-ttu-id="e8821-117">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="e8821-117">Windows Server 2012</span></span>
* <span data-ttu-id="e8821-118">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="e8821-118">Windows Server 2012 R2</span></span>
* <span data-ttu-id="e8821-119">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="e8821-119">Windows Server 2016</span></span>

<span data-ttu-id="e8821-120">**Verze systému SQL Server**:</span><span class="sxs-lookup"><span data-stu-id="e8821-120">**SQL Server version**:</span></span>

* <span data-ttu-id="e8821-121">SQL Server 2012</span><span class="sxs-lookup"><span data-stu-id="e8821-121">SQL Server 2012</span></span>
* <span data-ttu-id="e8821-122">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="e8821-122">SQL Server 2014</span></span>
* <span data-ttu-id="e8821-123">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="e8821-123">SQL Server 2016</span></span>

<span data-ttu-id="e8821-124">**Prostředí Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="e8821-124">**Azure PowerShell**:</span></span>

* <span data-ttu-id="e8821-125">[Nainstalujte nejnovější příkazy prostředí Azure PowerShell hello](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e8821-125">[Install hello latest Azure PowerShell commands](/powershell/azure/overview).</span></span>

<span data-ttu-id="e8821-126">**Rozšíření systému SQL Server IaaS**:</span><span class="sxs-lookup"><span data-stu-id="e8821-126">**SQL Server IaaS Extension**:</span></span>

* <span data-ttu-id="e8821-127">[Nainstalujte hello rozšíření systému SQL Server IaaS](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="e8821-127">[Install hello SQL Server IaaS Extension](../classic/sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="e8821-128">Nastavení</span><span class="sxs-lookup"><span data-stu-id="e8821-128">Settings</span></span>
<span data-ttu-id="e8821-129">Hello následující tabulka popisuje možnosti hello, které mohou být konfigurovány pro automatizované opravy.</span><span class="sxs-lookup"><span data-stu-id="e8821-129">hello following table describes hello options that can be configured for Automated Patching.</span></span> <span data-ttu-id="e8821-130">Pro klasické virtuální počítače je nutné použít PowerShell tooconfigure těchto nastavení.</span><span class="sxs-lookup"><span data-stu-id="e8821-130">For classic VMs, you must use PowerShell tooconfigure these settings.</span></span>

| <span data-ttu-id="e8821-131">Nastavení</span><span class="sxs-lookup"><span data-stu-id="e8821-131">Setting</span></span> | <span data-ttu-id="e8821-132">Možné hodnoty</span><span class="sxs-lookup"><span data-stu-id="e8821-132">Possible values</span></span> | <span data-ttu-id="e8821-133">Popis</span><span class="sxs-lookup"><span data-stu-id="e8821-133">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e8821-134">**Automatizované opravy**</span><span class="sxs-lookup"><span data-stu-id="e8821-134">**Automated Patching**</span></span> |<span data-ttu-id="e8821-135">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="e8821-135">Enable/Disable (Disabled)</span></span> |<span data-ttu-id="e8821-136">Povolí nebo zakáže automatizované opravy pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="e8821-136">Enables or disables Automated Patching for an Azure virtual machine.</span></span> |
| <span data-ttu-id="e8821-137">**Plán údržby.**</span><span class="sxs-lookup"><span data-stu-id="e8821-137">**Maintenance schedule**</span></span> |<span data-ttu-id="e8821-138">Každý den, pondělí, úterý, středu, čtvrtek a pátek, sobota, neděle</span><span class="sxs-lookup"><span data-stu-id="e8821-138">Everyday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday</span></span> |<span data-ttu-id="e8821-139">Hello plán pro stahování a instalace aktualizací s Windows, SQL Server a Microsoft pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e8821-139">hello schedule for downloading and installing Windows, SQL Server, and Microsoft updates for your virtual machine.</span></span> |
| <span data-ttu-id="e8821-140">**Hodina spouštění údržby**</span><span class="sxs-lookup"><span data-stu-id="e8821-140">**Maintenance start hour**</span></span> |<span data-ttu-id="e8821-141">0-24</span><span class="sxs-lookup"><span data-stu-id="e8821-141">0-24</span></span> |<span data-ttu-id="e8821-142">Hello místní počáteční čas tooupdate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e8821-142">hello local start time tooupdate hello virtual machine.</span></span> |
| <span data-ttu-id="e8821-143">**Doba trvání okna údržby**</span><span class="sxs-lookup"><span data-stu-id="e8821-143">**Maintenance window duration**</span></span> |<span data-ttu-id="e8821-144">30-180</span><span class="sxs-lookup"><span data-stu-id="e8821-144">30-180</span></span> |<span data-ttu-id="e8821-145">Hello počet minut povolené toocomplete hello stažení a instalace aktualizací.</span><span class="sxs-lookup"><span data-stu-id="e8821-145">hello number of minutes permitted toocomplete hello download and installation of updates.</span></span> |
| <span data-ttu-id="e8821-146">**Oprava kategorie**</span><span class="sxs-lookup"><span data-stu-id="e8821-146">**Patch Category**</span></span> |<span data-ttu-id="e8821-147">Důležité</span><span class="sxs-lookup"><span data-stu-id="e8821-147">Important</span></span> |<span data-ttu-id="e8821-148">Hello kategorie aktualizace toodownload a instalaci.</span><span class="sxs-lookup"><span data-stu-id="e8821-148">hello category of updates toodownload and install.</span></span> |

## <a name="configuration-with-powershell"></a><span data-ttu-id="e8821-149">Konfigurace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e8821-149">Configuration with PowerShell</span></span>
<span data-ttu-id="e8821-150">V následujícím příkladu hello, prostředí PowerShell je použité tooconfigure automatizovaných oprav na existující virtuální počítač serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="e8821-150">In hello following example, PowerShell is used tooconfigure Automated Patching on an existing SQL Server VM.</span></span> <span data-ttu-id="e8821-151">Hello **New-AzureVMSqlServerAutoPatchingConfig** příkaz nakonfiguruje nové okno údržby pro automatické aktualizace.</span><span class="sxs-lookup"><span data-stu-id="e8821-151">hello **New-AzureVMSqlServerAutoPatchingConfig** command configures a new maintenance window for automatic updates.</span></span>

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

<span data-ttu-id="e8821-152">Podle toho, v tomto příkladu, hello následující tabulka popisuje hello praktické vliv na hello cílový virtuální počítač Azure:</span><span class="sxs-lookup"><span data-stu-id="e8821-152">Based on this example, hello following table describes hello practical effect on hello target Azure VM:</span></span>

| <span data-ttu-id="e8821-153">Parametr</span><span class="sxs-lookup"><span data-stu-id="e8821-153">Parameter</span></span> | <span data-ttu-id="e8821-154">Efekt</span><span class="sxs-lookup"><span data-stu-id="e8821-154">Effect</span></span> |
| --- | --- |
| <span data-ttu-id="e8821-155">**DayOfWeek**</span><span class="sxs-lookup"><span data-stu-id="e8821-155">**DayOfWeek**</span></span> |<span data-ttu-id="e8821-156">Každý čtvrtek nainstalovány opravy.</span><span class="sxs-lookup"><span data-stu-id="e8821-156">Patches installed every Thursday.</span></span> |
| <span data-ttu-id="e8821-157">**MaintenanceWindowStartingHour**</span><span class="sxs-lookup"><span data-stu-id="e8821-157">**MaintenanceWindowStartingHour**</span></span> |<span data-ttu-id="e8821-158">Začátek aktualizace na 11:00.</span><span class="sxs-lookup"><span data-stu-id="e8821-158">Begin updates at 11:00am.</span></span> |
| <span data-ttu-id="e8821-159">**MaintenanceWindowsDuration**</span><span class="sxs-lookup"><span data-stu-id="e8821-159">**MaintenanceWindowsDuration**</span></span> |<span data-ttu-id="e8821-160">Během 120 minut musí být nainstalované opravy.</span><span class="sxs-lookup"><span data-stu-id="e8821-160">Patches must be installed within 120 minutes.</span></span> <span data-ttu-id="e8821-161">Podle času zahájení hello, musí provést podle 1:00 pm.</span><span class="sxs-lookup"><span data-stu-id="e8821-161">Based on hello start time, they must complete by 1:00pm.</span></span> |
| <span data-ttu-id="e8821-162">**PatchCategory**</span><span class="sxs-lookup"><span data-stu-id="e8821-162">**PatchCategory**</span></span> |<span data-ttu-id="e8821-163">Hello pouze možné nastavení pro tento parametr je "Důležité".</span><span class="sxs-lookup"><span data-stu-id="e8821-163">hello only possible setting for this parameter is “Important”.</span></span> |

<span data-ttu-id="e8821-164">Ho může trvat několik minut tooinstall a nakonfigurujte hello IaaS agenta systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e8821-164">It could take several minutes tooinstall and configure hello SQL Server IaaS Agent.</span></span>

<span data-ttu-id="e8821-165">toodisable automatizovaných oprav spuštění hello stejný skript bez hello - povolit parametr toohello AzureVMSqlServerAutoPatchingConfig nový.</span><span class="sxs-lookup"><span data-stu-id="e8821-165">toodisable Automated Patching, run hello same script without hello -Enable parameter toohello New-AzureVMSqlServerAutoPatchingConfig.</span></span> <span data-ttu-id="e8821-166">Jak instalaci, může trvat několik minut toodisable automatizovaných oprav.</span><span class="sxs-lookup"><span data-stu-id="e8821-166">As with installation, it can take several minutes toodisable Automated Patching.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e8821-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e8821-167">Next steps</span></span>
<span data-ttu-id="e8821-168">Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="e8821-168">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md).</span></span>

<span data-ttu-id="e8821-169">Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e8821-169">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

