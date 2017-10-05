---
title: "Automatizaci úloh správy na virtuálních počítačích SQL (klasické) | Microsoft Docs"
description: "Toto téma popisuje, jak spravovat rozšíření agenta systému SQL Server, který automatizuje konkrétní úlohy správy systému SQL Server. Mezi ně patří automatizovaného zálohování, automatizovaných oprav a Azure Key Vault integrace. Toto téma používá režim nasazení classic."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a9bda2e7-cdba-427c-bc30-77cde4376f3a
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30fa9128cd51a7498449c991b58500ad9acdd3d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-classic"></a><span data-ttu-id="31c93-105">Automatizaci úloh správy ve virtuálních počítačích Azure s rozšíření agenta systému SQL Server (klasické)</span><span class="sxs-lookup"><span data-stu-id="31c93-105">Automate management tasks on Azure Virtual Machines with the SQL Server Agent Extension (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="31c93-106">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="31c93-106">Resource Manager</span></span>](../sql/virtual-machines-windows-sql-server-agent-extension.md)
> * [<span data-ttu-id="31c93-107">Classic</span><span class="sxs-lookup"><span data-stu-id="31c93-107">Classic</span></span>](../classic/sql-server-agent-extension.md)
> 
>
 
<span data-ttu-id="31c93-108">Rozšíření agenta IaaS serveru SQL (SQLIaaSAgent) běží na virtuálních počítačích Azure k automatizaci úloh správy.</span><span class="sxs-lookup"><span data-stu-id="31c93-108">The SQL Server IaaS Agent Extension (SQLIaaSAgent) runs on Azure virtual machines to automate administration tasks.</span></span> <span data-ttu-id="31c93-109">Toto téma obsahuje přehled služby podporuje rozšíření a také pokyny pro instalaci, stavu a odebrání.</span><span class="sxs-lookup"><span data-stu-id="31c93-109">This topic provides an overview of the services supported by the extension as well as instructions for installation, status, and removal.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="31c93-110">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="31c93-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="31c93-111">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="31c93-111">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="31c93-112">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="31c93-112">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="31c93-113">Resource Manager verzi v tomto článku najdete v tématu [rozšíření agenta systému SQL Server pro SQL Server virtuální počítače Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="31c93-113">To view the Resource Manager version of this article, see [SQL Server Agent Extension for SQL Server VMs Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md).</span></span>

## <a name="supported-services"></a><span data-ttu-id="31c93-114">Podporované služby</span><span class="sxs-lookup"><span data-stu-id="31c93-114">Supported services</span></span>
<span data-ttu-id="31c93-115">Rozšíření agenta systému SQL Server IaaS podporuje následující úlohy správy:</span><span class="sxs-lookup"><span data-stu-id="31c93-115">The SQL Server IaaS Agent Extension supports the following administration tasks:</span></span>

| <span data-ttu-id="31c93-116">Funkce správy</span><span class="sxs-lookup"><span data-stu-id="31c93-116">Administration feature</span></span> | <span data-ttu-id="31c93-117">Popis</span><span class="sxs-lookup"><span data-stu-id="31c93-117">Description</span></span> |
| --- | --- |
| <span data-ttu-id="31c93-118">**Automatizované zálohování SQL**</span><span class="sxs-lookup"><span data-stu-id="31c93-118">**SQL Automated Backup**</span></span> |<span data-ttu-id="31c93-119">Automatizuje plánování zálohování pro všechny databáze pro výchozí instanci systému SQL Server ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="31c93-119">Automates the scheduling of backups for all databases for the default instance of SQL Server in the VM.</span></span> <span data-ttu-id="31c93-120">Další informace najdete v tématu [automatizované zálohování pro SQL Server v Azure Virtual Machines (klasické)](../classic/sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="31c93-120">For more information, see [Automated backup for SQL Server in Azure Virtual Machines (Classic)](../classic/sql-automated-backup.md).</span></span> |
| <span data-ttu-id="31c93-121">**Automatizované opravy pro SQL**</span><span class="sxs-lookup"><span data-stu-id="31c93-121">**SQL Automated Patching**</span></span> |<span data-ttu-id="31c93-122">Konfiguruje okno údržby, během které k virtuálnímu počítači může proběhnout aktualizace, takže se můžete vyhnout aktualizace během špiček pro úlohy.</span><span class="sxs-lookup"><span data-stu-id="31c93-122">Configures a maintenance window during which updates to your VM can take place, so  you can avoid updates during peak times for your workload.</span></span> <span data-ttu-id="31c93-123">Další informace najdete v tématu [automatizované opravy pro SQL Server v Azure Virtual Machines (klasické)](../classic/sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="31c93-123">For more information, see [Automated patching for SQL Server in Azure Virtual Machines (Classic)](../classic/sql-automated-patching.md).</span></span> |
| <span data-ttu-id="31c93-124">**Integrace se službou Azure Key Vault**</span><span class="sxs-lookup"><span data-stu-id="31c93-124">**Azure Key Vault Integration**</span></span> |<span data-ttu-id="31c93-125">Umožňuje automaticky nainstalovat a nakonfigurovat Azure Key Vault na virtuální počítač s SQL serverem.</span><span class="sxs-lookup"><span data-stu-id="31c93-125">Enables you to automatically install and configure Azure Key Vault on your SQL Server VM.</span></span> <span data-ttu-id="31c93-126">Další informace najdete v tématu [nakonfigurovat klíč trezoru integrace se službou Azure pro systém SQL Server na virtuálních počítačích Azure (klasický)](../classic/ps-sql-keyvault.md).</span><span class="sxs-lookup"><span data-stu-id="31c93-126">For more information, see [Configure Azure Key Vault Integration for SQL Server on Azure VMs (Classic)](../classic/ps-sql-keyvault.md).</span></span> |

## <a name="prerequisites"></a><span data-ttu-id="31c93-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="31c93-127">Prerequisites</span></span>
<span data-ttu-id="31c93-128">Požadavky pro použití IaaS agenta rozšíření systému SQL Server na virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="31c93-128">Requirements to use the SQL Server IaaS Agent Extension on your VM:</span></span>

### <a name="operating-system"></a><span data-ttu-id="31c93-129">Operační systém:</span><span class="sxs-lookup"><span data-stu-id="31c93-129">Operating System:</span></span>
* <span data-ttu-id="31c93-130">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="31c93-130">Windows Server 2012</span></span>
* <span data-ttu-id="31c93-131">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="31c93-131">Windows Server 2012 R2</span></span>
* <span data-ttu-id="31c93-132">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="31c93-132">Windows Server 2016</span></span>

### <a name="sql-server-versions"></a><span data-ttu-id="31c93-133">Verze systému SQL Server:</span><span class="sxs-lookup"><span data-stu-id="31c93-133">SQL Server versions:</span></span>
* <span data-ttu-id="31c93-134">SQL Server 2012</span><span class="sxs-lookup"><span data-stu-id="31c93-134">SQL Server 2012</span></span>
* <span data-ttu-id="31c93-135">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="31c93-135">SQL Server 2014</span></span>
* <span data-ttu-id="31c93-136">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="31c93-136">SQL Server 2016</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="31c93-137">Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="31c93-137">Azure PowerShell:</span></span>
<span data-ttu-id="31c93-138">[Stáhnout a nakonfigurovat nejnovější příkazy prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="31c93-138">[Download and configure the latest Azure PowerShell commands](/powershell/azure/overview).</span></span>

<span data-ttu-id="31c93-139">Spusťte prostředí Windows PowerShell a připojte ho k předplatnému Azure s **Add-AzureAccount** příkaz.</span><span class="sxs-lookup"><span data-stu-id="31c93-139">Start Windows PowerShell, and connect it to your Azure subscription with the **Add-AzureAccount** command.</span></span>

    Add-AzureAccount

<span data-ttu-id="31c93-140">Pokud máte více předplatných, použijte **Select-AzureSubscription** vyberte předplatné, který obsahuje cílových classic virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="31c93-140">If you have multiple subscriptions, use **Select-AzureSubscription** to select the subscription that contains your target classic VM.</span></span>

    Select-AzureSubscription -SubscriptionName <subscriptionname>

<span data-ttu-id="31c93-141">V tomto okamžiku můžete získat seznam klasické virtuální počítače a jejich názvy přidružená služba s **Get-AzureVM** příkaz.</span><span class="sxs-lookup"><span data-stu-id="31c93-141">At this point, you can get a list of the classic virtual machines and their associated service names with the **Get-AzureVM** command.</span></span>

    Get-AzureVM

## <a name="installation"></a><span data-ttu-id="31c93-142">Instalace</span><span class="sxs-lookup"><span data-stu-id="31c93-142">Installation</span></span>
<span data-ttu-id="31c93-143">Pro klasické virtuální počítače musíte použít PowerShell k instalaci rozšíření agenta systému SQL Server IaaS a konfiguraci jejích přidružených služeb.</span><span class="sxs-lookup"><span data-stu-id="31c93-143">For classic VMs, you must use PowerShell to install the SQL Server IaaS Agent Extension and configure its associated services.</span></span> <span data-ttu-id="31c93-144">Použití **Set-AzureVMSqlServerExtension** rutiny prostředí PowerShell k instalaci rozšíření.</span><span class="sxs-lookup"><span data-stu-id="31c93-144">Use the **Set-AzureVMSqlServerExtension** PowerShell cmdlet to install the extension.</span></span> <span data-ttu-id="31c93-145">Například následující příkaz instalaci rozšíření na virtuálním počítači Windows serveru (klasické) a pojmenuje ji "SQLIaaSExtension".</span><span class="sxs-lookup"><span data-stu-id="31c93-145">For example, the following command installs the extension on a Windows Server VM (classic) and names it "SQLIaaSExtension".</span></span>

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

<span data-ttu-id="31c93-146">Při aktualizaci na nejnovější verzi agenta rozšíření IaaS SQL, je nutné restartovat virtuální počítač po aktualizaci rozšíření.</span><span class="sxs-lookup"><span data-stu-id="31c93-146">If you update to the latest version of the SQL IaaS Agent Extension, you must restart your virtual machine after updating the extension.</span></span>

> [!NOTE]
> <span data-ttu-id="31c93-147">Klasické virtuální počítače nemají možnost instalace a konfigurace rozšíření agenta systému SQL IaaS prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="31c93-147">Classic virtual machines do not have an option to install and configure the SQL IaaS Agent Extension through the portal.</span></span>
> 
> 

## <a name="status"></a><span data-ttu-id="31c93-148">Status</span><span class="sxs-lookup"><span data-stu-id="31c93-148">Status</span></span>
<span data-ttu-id="31c93-149">Chcete-li ověřit, zda je nainstalován rozšíření je zobrazíte stav agenta na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="31c93-149">One way to verify that the extension is installed is to view the agent status in the Azure Portal.</span></span> <span data-ttu-id="31c93-150">Vyberte virtuální počítač uvedený v okně virtuálního počítače a potom klikněte na **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="31c93-150">Select a virtual machine listed in the virtual machine blade, and then click on **Extensions**.</span></span> <span data-ttu-id="31c93-151">Měli byste vidět **SQLIaaSAgent** rozšíření uvedené.</span><span class="sxs-lookup"><span data-stu-id="31c93-151">You should see the **SQLIaaSAgent** extension listed.</span></span>

![Rozšíření IaaS agenta systému SQL Server na portálu Azure](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

<span data-ttu-id="31c93-153">Můžete také **Get-AzureVMSqlServerExtension** rutiny Azure Powershellu.</span><span class="sxs-lookup"><span data-stu-id="31c93-153">You can also use the **Get-AzureVMSqlServerExtension** Azure Powershell cmdlet.</span></span>

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a><span data-ttu-id="31c93-154">Odebrání</span><span class="sxs-lookup"><span data-stu-id="31c93-154">Removal</span></span>
<span data-ttu-id="31c93-155">Na portálu Azure můžete odinstalovat rozšíření kliknutím na tlačítko se třemi tečkami na **rozšíření** okno vaší vlastnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="31c93-155">In the Azure Portal, you can uninstall the extension by clicking the ellipsis on the **Extensions** blade of your virtual machine properties.</span></span> <span data-ttu-id="31c93-156">Pak klikněte na tlačítko **odinstalovat**.</span><span class="sxs-lookup"><span data-stu-id="31c93-156">Then click **Uninstall**.</span></span>

![Odinstalovat rozšíření IaaS agenta systému SQL Server na portálu Azure](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

<span data-ttu-id="31c93-158">Můžete také **odebrat AzureVMSqlServerExtension** rutiny prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="31c93-158">You can also use the **Remove-AzureVMSqlServerExtension** Powershell cmdlet.</span></span>

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="31c93-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31c93-159">Next Steps</span></span>
<span data-ttu-id="31c93-160">Začněte pomocí jedné ze služeb, podporuje rozšíření.</span><span class="sxs-lookup"><span data-stu-id="31c93-160">Begin using one of the services supported by the extension.</span></span> <span data-ttu-id="31c93-161">Další podrobnosti najdete v tématech v odkazuje [podporované služby](#supported-services) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="31c93-161">For more details, see the topics referenced in the [Supported services](#supported-services) section of this article.</span></span>

<span data-ttu-id="31c93-162">Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="31c93-162">For more information about running SQL Server on Azure Virtual Machines, see [SQL Server on Azure Virtual Machines overview](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

