---
title: "Automatizované opravy pro virtuální počítače serveru SQL (klasické) | Microsoft Docs"
description: "Vysvětluje funkci automatizované opravy pro SQL Server virtuální počítače běžící v Azure pomocí režimu nasazení classic."
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
ms.openlocfilehash: 1959871141f196ba80ffd7b37e62e5ea5b42dba3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a><span data-ttu-id="17e39-103">Automatizované opravy pro SQL Server na virtuálních počítačích Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="17e39-103">Automated Patching for SQL Server in Azure Virtual Machines (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="17e39-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="17e39-104">Resource Manager</span></span>](../sql/virtual-machines-windows-sql-automated-patching.md)
> * [<span data-ttu-id="17e39-105">Classic</span><span class="sxs-lookup"><span data-stu-id="17e39-105">Classic</span></span>](../classic/sql-automated-patching.md)
> 
> 

<span data-ttu-id="17e39-106">Automatizovaných oprav určuje časové období údržby pro virtuální počítač Azure systémem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="17e39-106">Automated Patching establishes a maintenance window for an Azure Virtual Machine running SQL Server.</span></span> <span data-ttu-id="17e39-107">Automatické aktualizace lze nainstalovat pouze během tohoto časového období údržby.</span><span class="sxs-lookup"><span data-stu-id="17e39-107">Automated Updates can only be installed during this maintenance window.</span></span> <span data-ttu-id="17e39-108">Pro systém SQL Server tím se zajistí, že aktualizace systému a všechny přidružené restartuje dojít na nejlepší možný čas pro databázi.</span><span class="sxs-lookup"><span data-stu-id="17e39-108">For SQL Server, this ensures that system updates and any associated restarts occur at the best possible time for the database.</span></span> <span data-ttu-id="17e39-109">Automatizovaných oprav závisí na [rozšíření agenta systému SQL Server IaaS](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="17e39-109">Automated Patching depends on the [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="17e39-110">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="17e39-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="17e39-111">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="17e39-111">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="17e39-112">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="17e39-112">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="17e39-113">Resource Manager verzi v tomto článku najdete v tématu [automatizované opravy pro SQL Server ve službě Správce prostředků virtuálních počítačů Azure](../sql/virtual-machines-windows-sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="17e39-113">To view the Resource Manager version of this article, see [Automated Patching for SQL Server in Azure Virtual Machines Resource Manager](../sql/virtual-machines-windows-sql-automated-patching.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="17e39-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="17e39-114">Prerequisites</span></span>
<span data-ttu-id="17e39-115">Pomocí automatizované opravy, zvažte následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="17e39-115">To use Automated Patching, consider the following prerequisites:</span></span>

<span data-ttu-id="17e39-116">**Operační systém**:</span><span class="sxs-lookup"><span data-stu-id="17e39-116">**Operating System**:</span></span>

* <span data-ttu-id="17e39-117">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="17e39-117">Windows Server 2012</span></span>
* <span data-ttu-id="17e39-118">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="17e39-118">Windows Server 2012 R2</span></span>
* <span data-ttu-id="17e39-119">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="17e39-119">Windows Server 2016</span></span>

<span data-ttu-id="17e39-120">**Verze systému SQL Server**:</span><span class="sxs-lookup"><span data-stu-id="17e39-120">**SQL Server version**:</span></span>

* <span data-ttu-id="17e39-121">SQL Server 2012</span><span class="sxs-lookup"><span data-stu-id="17e39-121">SQL Server 2012</span></span>
* <span data-ttu-id="17e39-122">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="17e39-122">SQL Server 2014</span></span>
* <span data-ttu-id="17e39-123">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="17e39-123">SQL Server 2016</span></span>

<span data-ttu-id="17e39-124">**Prostředí Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="17e39-124">**Azure PowerShell**:</span></span>

* <span data-ttu-id="17e39-125">[Nainstalujte nejnovější příkazy prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="17e39-125">[Install the latest Azure PowerShell commands](/powershell/azure/overview).</span></span>

<span data-ttu-id="17e39-126">**Rozšíření systému SQL Server IaaS**:</span><span class="sxs-lookup"><span data-stu-id="17e39-126">**SQL Server IaaS Extension**:</span></span>

* <span data-ttu-id="17e39-127">[Nainstalujte rozšíření SQL Server IaaS](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="17e39-127">[Install the SQL Server IaaS Extension](../classic/sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="17e39-128">Nastavení</span><span class="sxs-lookup"><span data-stu-id="17e39-128">Settings</span></span>
<span data-ttu-id="17e39-129">Následující tabulka popisuje možnosti, které mohou být konfigurovány pro automatizované opravy.</span><span class="sxs-lookup"><span data-stu-id="17e39-129">The following table describes the options that can be configured for Automated Patching.</span></span> <span data-ttu-id="17e39-130">Pro klasické virtuální počítače musíte použít PowerShell k nakonfigurování těchto nastavení.</span><span class="sxs-lookup"><span data-stu-id="17e39-130">For classic VMs, you must use PowerShell to configure these settings.</span></span>

| <span data-ttu-id="17e39-131">Nastavení</span><span class="sxs-lookup"><span data-stu-id="17e39-131">Setting</span></span> | <span data-ttu-id="17e39-132">Možné hodnoty</span><span class="sxs-lookup"><span data-stu-id="17e39-132">Possible values</span></span> | <span data-ttu-id="17e39-133">Popis</span><span class="sxs-lookup"><span data-stu-id="17e39-133">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="17e39-134">**Automatizované opravy**</span><span class="sxs-lookup"><span data-stu-id="17e39-134">**Automated Patching**</span></span> |<span data-ttu-id="17e39-135">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="17e39-135">Enable/Disable (Disabled)</span></span> |<span data-ttu-id="17e39-136">Povolí nebo zakáže automatizované opravy pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="17e39-136">Enables or disables Automated Patching for an Azure virtual machine.</span></span> |
| <span data-ttu-id="17e39-137">**Plán údržby.**</span><span class="sxs-lookup"><span data-stu-id="17e39-137">**Maintenance schedule**</span></span> |<span data-ttu-id="17e39-138">Každý den, pondělí, úterý, středu, čtvrtek a pátek, sobota, neděle</span><span class="sxs-lookup"><span data-stu-id="17e39-138">Everyday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday</span></span> |<span data-ttu-id="17e39-139">Plán pro stahování a instalace aktualizací s Windows, SQL Server a Microsoft pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="17e39-139">The schedule for downloading and installing Windows, SQL Server, and Microsoft updates for your virtual machine.</span></span> |
| <span data-ttu-id="17e39-140">**Hodina spouštění údržby**</span><span class="sxs-lookup"><span data-stu-id="17e39-140">**Maintenance start hour**</span></span> |<span data-ttu-id="17e39-141">0-24</span><span class="sxs-lookup"><span data-stu-id="17e39-141">0-24</span></span> |<span data-ttu-id="17e39-142">Místní spuštění aktualizovat virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="17e39-142">The local start time to update the virtual machine.</span></span> |
| <span data-ttu-id="17e39-143">**Doba trvání okna údržby**</span><span class="sxs-lookup"><span data-stu-id="17e39-143">**Maintenance window duration**</span></span> |<span data-ttu-id="17e39-144">30-180</span><span class="sxs-lookup"><span data-stu-id="17e39-144">30-180</span></span> |<span data-ttu-id="17e39-145">Počet minut oprávnění k dokončení stahování a instalaci aktualizací.</span><span class="sxs-lookup"><span data-stu-id="17e39-145">The number of minutes permitted to complete the download and installation of updates.</span></span> |
| <span data-ttu-id="17e39-146">**Oprava kategorie**</span><span class="sxs-lookup"><span data-stu-id="17e39-146">**Patch Category**</span></span> |<span data-ttu-id="17e39-147">Důležité</span><span class="sxs-lookup"><span data-stu-id="17e39-147">Important</span></span> |<span data-ttu-id="17e39-148">Kategorie aktualizací, které se stáhnout a nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="17e39-148">The category of updates to download and install.</span></span> |

## <a name="configuration-with-powershell"></a><span data-ttu-id="17e39-149">Konfigurace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="17e39-149">Configuration with PowerShell</span></span>
<span data-ttu-id="17e39-150">V následujícím příkladu prostředí PowerShell slouží ke konfiguraci automatizovaných oprav na existující virtuální počítač serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="17e39-150">In the following example, PowerShell is used to configure Automated Patching on an existing SQL Server VM.</span></span> <span data-ttu-id="17e39-151">**New-AzureVMSqlServerAutoPatchingConfig** příkaz nakonfiguruje nové okno údržby pro automatické aktualizace.</span><span class="sxs-lookup"><span data-stu-id="17e39-151">The **New-AzureVMSqlServerAutoPatchingConfig** command configures a new maintenance window for automatic updates.</span></span>

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

<span data-ttu-id="17e39-152">Podle toho, v tomto příkladu, následující tabulka popisuje praktická vliv na cílovém virtuálním počítači Azure:</span><span class="sxs-lookup"><span data-stu-id="17e39-152">Based on this example, the following table describes the practical effect on the target Azure VM:</span></span>

| <span data-ttu-id="17e39-153">Parametr</span><span class="sxs-lookup"><span data-stu-id="17e39-153">Parameter</span></span> | <span data-ttu-id="17e39-154">Efekt</span><span class="sxs-lookup"><span data-stu-id="17e39-154">Effect</span></span> |
| --- | --- |
| <span data-ttu-id="17e39-155">**DayOfWeek**</span><span class="sxs-lookup"><span data-stu-id="17e39-155">**DayOfWeek**</span></span> |<span data-ttu-id="17e39-156">Každý čtvrtek nainstalovány opravy.</span><span class="sxs-lookup"><span data-stu-id="17e39-156">Patches installed every Thursday.</span></span> |
| <span data-ttu-id="17e39-157">**MaintenanceWindowStartingHour**</span><span class="sxs-lookup"><span data-stu-id="17e39-157">**MaintenanceWindowStartingHour**</span></span> |<span data-ttu-id="17e39-158">Začátek aktualizace na 11:00.</span><span class="sxs-lookup"><span data-stu-id="17e39-158">Begin updates at 11:00am.</span></span> |
| <span data-ttu-id="17e39-159">**MaintenanceWindowsDuration**</span><span class="sxs-lookup"><span data-stu-id="17e39-159">**MaintenanceWindowsDuration**</span></span> |<span data-ttu-id="17e39-160">Během 120 minut musí být nainstalované opravy.</span><span class="sxs-lookup"><span data-stu-id="17e39-160">Patches must be installed within 120 minutes.</span></span> <span data-ttu-id="17e39-161">Podle času zahájení, musí provést podle 1:00 pm.</span><span class="sxs-lookup"><span data-stu-id="17e39-161">Based on the start time, they must complete by 1:00pm.</span></span> |
| <span data-ttu-id="17e39-162">**PatchCategory**</span><span class="sxs-lookup"><span data-stu-id="17e39-162">**PatchCategory**</span></span> |<span data-ttu-id="17e39-163">Jedinou možnou nastavení pro tento parametr je "Důležité".</span><span class="sxs-lookup"><span data-stu-id="17e39-163">The only possible setting for this parameter is “Important”.</span></span> |

<span data-ttu-id="17e39-164">Ho může trvat několik minut k instalaci a konfiguraci IaaS Agent serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="17e39-164">It could take several minutes to install and configure the SQL Server IaaS Agent.</span></span>

<span data-ttu-id="17e39-165">Chcete-li zakázat automatizovaných oprav, spusťte stejný skript bez parametru-povolit New-AzureVMSqlServerAutoPatchingConfig.</span><span class="sxs-lookup"><span data-stu-id="17e39-165">To disable Automated Patching, run the same script without the -Enable parameter to the New-AzureVMSqlServerAutoPatchingConfig.</span></span> <span data-ttu-id="17e39-166">Stejně jako u instalace, se může trvat několik minut zakázat automatizovaných oprav.</span><span class="sxs-lookup"><span data-stu-id="17e39-166">As with installation, it can take several minutes to disable Automated Patching.</span></span>

## <a name="next-steps"></a><span data-ttu-id="17e39-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="17e39-167">Next steps</span></span>
<span data-ttu-id="17e39-168">Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="17e39-168">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md).</span></span>

<span data-ttu-id="17e39-169">Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="17e39-169">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

