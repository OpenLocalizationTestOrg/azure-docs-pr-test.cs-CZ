---
title: "Automatizaci úloh správy na virtuálních počítačích SQL (Resource Manager) | Microsoft Docs"
description: "Toto téma popisuje, jak spravovat rozšíření agenta systému SQL Server, který automatizuje konkrétní úlohy správy systému SQL Server. Mezi ně patří automatizovaného zálohování, automatizovaných oprav a Azure Key Vault integrace. Toto téma používá režim nasazení Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7152d184bb6d1d4b81aeb47e2c7c9160ada36023
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-resource-manager"></a><span data-ttu-id="b8a54-105">Automatizaci úloh správy ve virtuálních počítačích Azure s rozšíření agenta systému SQL Server (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="b8a54-105">Automate management tasks on Azure Virtual Machines with the SQL Server Agent Extension (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b8a54-106">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b8a54-106">Resource Manager</span></span>](virtual-machines-windows-sql-server-agent-extension.md)
> * [<span data-ttu-id="b8a54-107">Classic</span><span class="sxs-lookup"><span data-stu-id="b8a54-107">Classic</span></span>](../classic/sql-server-agent-extension.md)
> 
> 

<span data-ttu-id="b8a54-108">Rozšíření agenta IaaS serveru SQL (SQLIaaSExtension) běží na virtuálních počítačích Azure k automatizaci úloh správy.</span><span class="sxs-lookup"><span data-stu-id="b8a54-108">The SQL Server IaaS Agent Extension (SQLIaaSExtension) runs on Azure virtual machines to automate administration tasks.</span></span> <span data-ttu-id="b8a54-109">Toto téma obsahuje přehled služby podporuje rozšíření a také pokyny pro instalaci, stavu a odebrání.</span><span class="sxs-lookup"><span data-stu-id="b8a54-109">This topic provides an overview of the services supported by the extension as well as instructions for installation, status, and removal.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="b8a54-110">Klasické verze tohoto článku najdete v tématu [rozšíření agenta systému SQL Server pro SQL Server virtuální počítače Classic](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="b8a54-110">To view the classic version of this article, see [SQL Server Agent Extension for SQL Server VMs Classic](../classic/sql-server-agent-extension.md).</span></span>

## <a name="supported-services"></a><span data-ttu-id="b8a54-111">Podporované služby</span><span class="sxs-lookup"><span data-stu-id="b8a54-111">Supported services</span></span>
<span data-ttu-id="b8a54-112">Rozšíření agenta systému SQL Server IaaS podporuje následující úlohy správy:</span><span class="sxs-lookup"><span data-stu-id="b8a54-112">The SQL Server IaaS Agent Extension supports the following administration tasks:</span></span>

| <span data-ttu-id="b8a54-113">Funkce správy</span><span class="sxs-lookup"><span data-stu-id="b8a54-113">Administration feature</span></span> | <span data-ttu-id="b8a54-114">Popis</span><span class="sxs-lookup"><span data-stu-id="b8a54-114">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b8a54-115">**Automatizované zálohování SQL**</span><span class="sxs-lookup"><span data-stu-id="b8a54-115">**SQL Automated Backup**</span></span> |<span data-ttu-id="b8a54-116">Automatizuje plánování zálohování pro všechny databáze pro výchozí instanci systému SQL Server ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="b8a54-116">Automates the scheduling of backups for all databases for the default instance of SQL Server in the VM.</span></span> <span data-ttu-id="b8a54-117">Další informace najdete v tématu [automatizované zálohování pro SQL Server v Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="b8a54-117">For more information, see [Automated backup for SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md).</span></span> |
| <span data-ttu-id="b8a54-118">**Automatizované opravy pro SQL**</span><span class="sxs-lookup"><span data-stu-id="b8a54-118">**SQL Automated Patching**</span></span> |<span data-ttu-id="b8a54-119">Konfiguruje okno údržby, během které k virtuálnímu počítači může proběhnout aktualizace, takže se můžete vyhnout aktualizace během špiček pro úlohy.</span><span class="sxs-lookup"><span data-stu-id="b8a54-119">Configures a maintenance window during which updates to your VM can take place, so  you can avoid updates during peak times for your workload.</span></span> <span data-ttu-id="b8a54-120">Další informace najdete v tématu [automatizované opravy pro SQL Server v Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="b8a54-120">For more information, see [Automated patching for SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md).</span></span> |
| <span data-ttu-id="b8a54-121">**Integrace se službou Azure Key Vault**</span><span class="sxs-lookup"><span data-stu-id="b8a54-121">**Azure Key Vault Integration**</span></span> |<span data-ttu-id="b8a54-122">Umožňuje automaticky nainstalovat a nakonfigurovat Azure Key Vault na virtuální počítač s SQL serverem.</span><span class="sxs-lookup"><span data-stu-id="b8a54-122">Enables you to automatically install and configure Azure Key Vault on your SQL Server VM.</span></span> <span data-ttu-id="b8a54-123">Další informace najdete v tématu [nakonfigurovat klíč trezoru integrace se službou Azure pro systém SQL Server na virtuálních počítačích Azure (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md).</span><span class="sxs-lookup"><span data-stu-id="b8a54-123">For more information, see [Configure Azure Key Vault Integration for SQL Server on Azure VMs (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md).</span></span> |

<span data-ttu-id="b8a54-124">Jakmile nainstalovaná a spuštěná, rozšíření agenta systému SQL Server IaaS zpřístupňuje tyto funkce pro správu na panelu systému SQL Server virtuálního počítače na portálu Azure a pomocí prostředí Azure PowerShell pro bitové kopie systému SQL Server marketplace a pomocí prostředí Azure PowerShell pro ruční instalace rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b8a54-124">Once installed and running, the SQL Server IaaS Agent Extension makes these administration features available on the SQL Server panel of the virtual machine in the Azure Portal and through Azure PowerShell for SQL Server marketplace images, and through Azure PowerShell for manual installations of the extension.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="b8a54-125">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b8a54-125">Prerequisites</span></span>
<span data-ttu-id="b8a54-126">Požadavky pro použití IaaS agenta rozšíření systému SQL Server na virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="b8a54-126">Requirements to use the SQL Server IaaS Agent Extension on your VM:</span></span>

<span data-ttu-id="b8a54-127">**Operační systém**:</span><span class="sxs-lookup"><span data-stu-id="b8a54-127">**Operating System**:</span></span>

* <span data-ttu-id="b8a54-128">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="b8a54-128">Windows Server 2012</span></span>
* <span data-ttu-id="b8a54-129">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="b8a54-129">Windows Server 2012 R2</span></span>
* <span data-ttu-id="b8a54-130">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="b8a54-130">Windows Server 2016</span></span>

<span data-ttu-id="b8a54-131">**Verze systému SQL Server**:</span><span class="sxs-lookup"><span data-stu-id="b8a54-131">**SQL Server versions**:</span></span>

* <span data-ttu-id="b8a54-132">SQL Server 2012</span><span class="sxs-lookup"><span data-stu-id="b8a54-132">SQL Server 2012</span></span>
* <span data-ttu-id="b8a54-133">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="b8a54-133">SQL Server 2014</span></span>
* <span data-ttu-id="b8a54-134">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="b8a54-134">SQL Server 2016</span></span>

<span data-ttu-id="b8a54-135">**Prostředí Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="b8a54-135">**Azure PowerShell**:</span></span>

* [<span data-ttu-id="b8a54-136">Stáhnout a nakonfigurovat nejnovější příkazy prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b8a54-136">Download and configure the latest Azure PowerShell commands</span></span>](/powershell/azure/overview)

## <a name="installation"></a><span data-ttu-id="b8a54-137">Instalace</span><span class="sxs-lookup"><span data-stu-id="b8a54-137">Installation</span></span>
<span data-ttu-id="b8a54-138">Rozšíření agenta systému SQL Server IaaS se automaticky nainstaluje při zřizování jednoho z Galerie obrázků virtuálního počítače systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b8a54-138">The SQL Server IaaS Agent Extension is automatically installed when you provision one of the SQL Server virtual machine gallery images.</span></span> <span data-ttu-id="b8a54-139">Pokud potřebujete znovu ručně nainstalujte rozšíření na jednu z těchto virtuálních počítačů serveru SQL, použijte následující příkaz Powershellu:</span><span class="sxs-lookup"><span data-stu-id="b8a54-139">If you need to reinstall the extension manually on one of these SQL Server VMs, use the following PowerShell command:</span></span>

```powershell
Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"
```

<span data-ttu-id="b8a54-140">Také je možné nainstalovat IaaS agenta rozšíření systému SQL Server na virtuálním počítači jen operačního systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="b8a54-140">It is also possible to install the SQL Server IaaS Agent Extension on an OS-only Windows Server virtual machine.</span></span> <span data-ttu-id="b8a54-141">Je podporováno pouze pokud jste nainstalovali také ručně SQL Server na tomto počítači.</span><span class="sxs-lookup"><span data-stu-id="b8a54-141">This is only supported if you have also manually installed SQL Server on that machine.</span></span> <span data-ttu-id="b8a54-142">Pak nainstalujte ručně pomocí stejné rozšíření **Set-AzureVMSqlServerExtension** rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b8a54-142">Then install the extension manually by using the same **Set-AzureVMSqlServerExtension** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="b8a54-143">Pokud ručně nainstalovat rozšíření agenta systému SQL Server IaaS na jen operačního systému Windows Server virtuální počítač, se nedají spravovat nastavení konfigurace systému SQL Server prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b8a54-143">If you manually install the SQL Server IaaS Agent Extension on an OS-only Windows Server VM, you can not manage the SQL Server configuration settings through the Azure portal.</span></span> <span data-ttu-id="b8a54-144">V tomto scénáři je nutné provést všechny změny v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b8a54-144">In this scenario, you must make all changes with PowerShell.</span></span>

## <a name="status"></a><span data-ttu-id="b8a54-145">Status</span><span class="sxs-lookup"><span data-stu-id="b8a54-145">Status</span></span>
<span data-ttu-id="b8a54-146">Chcete-li ověřit, zda je nainstalován rozšíření je zobrazíte stav agenta na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b8a54-146">One way to verify that the extension is installed is to view the agent status in the Azure Portal.</span></span> <span data-ttu-id="b8a54-147">Vyberte **všechna nastavení** v okně virtuálního počítače a pak klikněte na **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="b8a54-147">Select **All settings** in the virtual machine blade, and then click on **Extensions**.</span></span> <span data-ttu-id="b8a54-148">Měli byste vidět **SQLIaaSExtension** rozšíření uvedené.</span><span class="sxs-lookup"><span data-stu-id="b8a54-148">You should see the **SQLIaaSExtension** extension listed.</span></span>

![Rozšíření IaaS agenta systému SQL Server na portálu Azure](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

<span data-ttu-id="b8a54-150">Můžete také **Get-AzureVMSqlServerExtension** rutiny Azure Powershellu.</span><span class="sxs-lookup"><span data-stu-id="b8a54-150">You can also use the **Get-AzureVMSqlServerExtension** Azure Powershell cmdlet.</span></span>

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

<span data-ttu-id="b8a54-151">Předchozí příkaz potvrdí agenta je nainstalován a poskytuje obecné informace stavu.</span><span class="sxs-lookup"><span data-stu-id="b8a54-151">The previous command confirms the agent is installed and provides general status information.</span></span> <span data-ttu-id="b8a54-152">Můžete také získat stav konkrétní informace o automatizované zálohování a oprav pomocí následujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="b8a54-152">You can also get specific status information about Automated Backup and Patching with the following commands.</span></span>

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a><span data-ttu-id="b8a54-153">Odebrání</span><span class="sxs-lookup"><span data-stu-id="b8a54-153">Removal</span></span>
<span data-ttu-id="b8a54-154">Na portálu Azure můžete odinstalovat rozšíření kliknutím na tlačítko se třemi tečkami na **rozšíření** okno vaší vlastnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b8a54-154">In the Azure Portal, you can uninstall the extension by clicking the ellipsis on the **Extensions** blade of your virtual machine properties.</span></span> <span data-ttu-id="b8a54-155">Pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="b8a54-155">Then click **Delete**.</span></span>

![Odinstalovat rozšíření IaaS agenta systému SQL Server na portálu Azure](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

<span data-ttu-id="b8a54-157">Můžete také **odebrat AzureRmVMSqlServerExtension** rutiny prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="b8a54-157">You can also use the **Remove-AzureRmVMSqlServerExtension** Powershell cmdlet.</span></span>

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a><span data-ttu-id="b8a54-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8a54-158">Next Steps</span></span>
<span data-ttu-id="b8a54-159">Začněte pomocí jedné ze služeb, podporuje rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b8a54-159">Begin using one of the services supported by the extension.</span></span> <span data-ttu-id="b8a54-160">Další podrobnosti najdete v tématech v odkazuje [podporované služby](#supported-services) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="b8a54-160">For more details, see the topics referenced in the [Supported services](#supported-services) section of this article.</span></span>

<span data-ttu-id="b8a54-161">Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b8a54-161">For more information about running SQL Server on Azure Virtual Machines, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

