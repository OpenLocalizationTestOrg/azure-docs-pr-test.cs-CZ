---
title: "úlohy správy aaaAutomate na virtuálních počítačích SQL (Resource Manager) | Microsoft Docs"
description: "Toto téma popisuje, jak toomanage hello přípony agenta systému SQL Server, který automatizuje konkrétní úlohy správy systému SQL Server. Mezi ně patří automatizovaného zálohování, automatizovaných oprav a Azure Key Vault integrace. Toto téma používá hello režimu nasazení Resource Manager."
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
ms.openlocfilehash: ae917612c4af59f12c0b083440673bdc555e9d56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-hello-sql-server-agent-extension-resource-manager"></a><span data-ttu-id="371fa-105">Automatizaci úloh správy ve virtuálních počítačích Azure s hello rozšíření systému SQL Server Agent (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="371fa-105">Automate management tasks on Azure Virtual Machines with hello SQL Server Agent Extension (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="371fa-106">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="371fa-106">Resource Manager</span></span>](virtual-machines-windows-sql-server-agent-extension.md)
> * [<span data-ttu-id="371fa-107">Classic</span><span class="sxs-lookup"><span data-stu-id="371fa-107">Classic</span></span>](../classic/sql-server-agent-extension.md)
> 
> 

<span data-ttu-id="371fa-108">Hello IaaS agenta rozšíření (SQLIaaSExtension) systému SQL Server spouští na úlohy správy tooautomate virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="371fa-108">hello SQL Server IaaS Agent Extension (SQLIaaSExtension) runs on Azure virtual machines tooautomate administration tasks.</span></span> <span data-ttu-id="371fa-109">Toto téma obsahuje přehled služby hello podporované hello rozšíření, jakož i pokyny k instalaci, stavu a odebrání.</span><span class="sxs-lookup"><span data-stu-id="371fa-109">This topic provides an overview of hello services supported by hello extension as well as instructions for installation, status, and removal.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="371fa-110">tooview hello classic verze tohoto článku, najdete v části [rozšíření agenta systému SQL Server pro SQL Server virtuální počítače Classic](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="371fa-110">tooview hello classic version of this article, see [SQL Server Agent Extension for SQL Server VMs Classic](../classic/sql-server-agent-extension.md).</span></span>

## <a name="supported-services"></a><span data-ttu-id="371fa-111">Podporované služby</span><span class="sxs-lookup"><span data-stu-id="371fa-111">Supported services</span></span>
<span data-ttu-id="371fa-112">Hello rozšíření IaaS agenta systému SQL Server podporuje hello následující úlohy správy:</span><span class="sxs-lookup"><span data-stu-id="371fa-112">hello SQL Server IaaS Agent Extension supports hello following administration tasks:</span></span>

| <span data-ttu-id="371fa-113">Funkce správy</span><span class="sxs-lookup"><span data-stu-id="371fa-113">Administration feature</span></span> | <span data-ttu-id="371fa-114">Popis</span><span class="sxs-lookup"><span data-stu-id="371fa-114">Description</span></span> |
| --- | --- |
| <span data-ttu-id="371fa-115">**Automatizované zálohování SQL**</span><span class="sxs-lookup"><span data-stu-id="371fa-115">**SQL Automated Backup**</span></span> |<span data-ttu-id="371fa-116">Automatizuje hello plánování zálohování pro všechny databáze hello výchozí instanci systému SQL Server v hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="371fa-116">Automates hello scheduling of backups for all databases for hello default instance of SQL Server in hello VM.</span></span> <span data-ttu-id="371fa-117">Další informace najdete v tématu [automatizované zálohování pro SQL Server v Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="371fa-117">For more information, see [Automated backup for SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md).</span></span> |
| <span data-ttu-id="371fa-118">**Automatizované opravy pro SQL**</span><span class="sxs-lookup"><span data-stu-id="371fa-118">**SQL Automated Patching**</span></span> |<span data-ttu-id="371fa-119">Konfiguraci časového období údržby, během které aktualizace umístit tooyour, může trvat virtuálních počítačů, tak, aby se můžete vyhnout aktualizace během špiček pro úlohy.</span><span class="sxs-lookup"><span data-stu-id="371fa-119">Configures a maintenance window during which updates tooyour VM can take place, so  you can avoid updates during peak times for your workload.</span></span> <span data-ttu-id="371fa-120">Další informace najdete v tématu [automatizované opravy pro SQL Server v Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="371fa-120">For more information, see [Automated patching for SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md).</span></span> |
| <span data-ttu-id="371fa-121">**Integrace se službou Azure Key Vault**</span><span class="sxs-lookup"><span data-stu-id="371fa-121">**Azure Key Vault Integration**</span></span> |<span data-ttu-id="371fa-122">Umožňuje vám tooautomatically instalace a konfigurace Azure Key Vault na virtuální počítač s SQL serverem.</span><span class="sxs-lookup"><span data-stu-id="371fa-122">Enables you tooautomatically install and configure Azure Key Vault on your SQL Server VM.</span></span> <span data-ttu-id="371fa-123">Další informace najdete v tématu [nakonfigurovat klíč trezoru integrace se službou Azure pro systém SQL Server na virtuálních počítačích Azure (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md).</span><span class="sxs-lookup"><span data-stu-id="371fa-123">For more information, see [Configure Azure Key Vault Integration for SQL Server on Azure VMs (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md).</span></span> |

<span data-ttu-id="371fa-124">Po dokončení instalace a spuštění, hello rozšíření agenta systému SQL Server IaaS zpřístupní tyto funkce pro správu na panelu systému SQL Server hello hello virtuálního počítače v hello portálu Azure a pomocí prostředí Azure PowerShell pro bitové kopie systému SQL Server marketplace a prostřednictvím Chcete-li prostředí Azure PowerShell pro ruční instalací rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="371fa-124">Once installed and running, hello SQL Server IaaS Agent Extension makes these administration features available on hello SQL Server panel of hello virtual machine in hello Azure Portal and through Azure PowerShell for SQL Server marketplace images, and through Azure PowerShell for manual installations of hello extension.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="371fa-125">Požadavky</span><span class="sxs-lookup"><span data-stu-id="371fa-125">Prerequisites</span></span>
<span data-ttu-id="371fa-126">Požadavky na toouse hello rozšíření IaaS agenta systému SQL Server na vašem virtuálním počítači:</span><span class="sxs-lookup"><span data-stu-id="371fa-126">Requirements toouse hello SQL Server IaaS Agent Extension on your VM:</span></span>

<span data-ttu-id="371fa-127">**Operační systém**:</span><span class="sxs-lookup"><span data-stu-id="371fa-127">**Operating System**:</span></span>

* <span data-ttu-id="371fa-128">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="371fa-128">Windows Server 2012</span></span>
* <span data-ttu-id="371fa-129">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="371fa-129">Windows Server 2012 R2</span></span>
* <span data-ttu-id="371fa-130">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="371fa-130">Windows Server 2016</span></span>

<span data-ttu-id="371fa-131">**Verze systému SQL Server**:</span><span class="sxs-lookup"><span data-stu-id="371fa-131">**SQL Server versions**:</span></span>

* <span data-ttu-id="371fa-132">SQL Server 2012</span><span class="sxs-lookup"><span data-stu-id="371fa-132">SQL Server 2012</span></span>
* <span data-ttu-id="371fa-133">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="371fa-133">SQL Server 2014</span></span>
* <span data-ttu-id="371fa-134">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="371fa-134">SQL Server 2016</span></span>

<span data-ttu-id="371fa-135">**Prostředí Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="371fa-135">**Azure PowerShell**:</span></span>

* [<span data-ttu-id="371fa-136">Stáhnout a nakonfigurovat Azure PowerShell příkazy nejnovější hello</span><span class="sxs-lookup"><span data-stu-id="371fa-136">Download and configure hello latest Azure PowerShell commands</span></span>](/powershell/azure/overview)

## <a name="installation"></a><span data-ttu-id="371fa-137">Instalace</span><span class="sxs-lookup"><span data-stu-id="371fa-137">Installation</span></span>
<span data-ttu-id="371fa-138">Hello rozšíření IaaS agenta systému SQL Server se automaticky nainstaluje při zřizování hello bitové kopie Galerie virtuálních počítačů systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="371fa-138">hello SQL Server IaaS Agent Extension is automatically installed when you provision one of hello SQL Server virtual machine gallery images.</span></span> <span data-ttu-id="371fa-139">Pokud potřebujete tooreinstall hello rozšíření na jednu z těchto virtuálních počítačích SQL Server ručně, použijte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="371fa-139">If you need tooreinstall hello extension manually on one of these SQL Server VMs, use hello following PowerShell command:</span></span>

```powershell
Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"
```

<span data-ttu-id="371fa-140">Je také možné tooinstall hello rozšíření IaaS agenta systému SQL Server na virtuálním počítači jen operačního systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="371fa-140">It is also possible tooinstall hello SQL Server IaaS Agent Extension on an OS-only Windows Server virtual machine.</span></span> <span data-ttu-id="371fa-141">Je podporováno pouze pokud jste nainstalovali také ručně SQL Server na tomto počítači.</span><span class="sxs-lookup"><span data-stu-id="371fa-141">This is only supported if you have also manually installed SQL Server on that machine.</span></span> <span data-ttu-id="371fa-142">Pak instalace rozšíření hello ručně pomocí hello stejné **Set-AzureVMSqlServerExtension** rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="371fa-142">Then install hello extension manually by using hello same **Set-AzureVMSqlServerExtension** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="371fa-143">Pokud nainstalujete ručně hello rozšíření IaaS agenta systému SQL Server na jen operačního systému Windows Server virtuální počítač, se nedají spravovat nastavení konfigurace systému SQL Server hello prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="371fa-143">If you manually install hello SQL Server IaaS Agent Extension on an OS-only Windows Server VM, you can not manage hello SQL Server configuration settings through hello Azure portal.</span></span> <span data-ttu-id="371fa-144">V tomto scénáři je nutné provést všechny změny v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="371fa-144">In this scenario, you must make all changes with PowerShell.</span></span>

## <a name="status"></a><span data-ttu-id="371fa-145">Status</span><span class="sxs-lookup"><span data-stu-id="371fa-145">Status</span></span>
<span data-ttu-id="371fa-146">Jedním ze způsobů tooverify nainstalovanou hello rozšíření je stav agenta hello tooview hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="371fa-146">One way tooverify that hello extension is installed is tooview hello agent status in hello Azure Portal.</span></span> <span data-ttu-id="371fa-147">Vyberte **všechna nastavení** v hello okno virtuálního počítače a potom klikněte na **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="371fa-147">Select **All settings** in hello virtual machine blade, and then click on **Extensions**.</span></span> <span data-ttu-id="371fa-148">Měli byste vidět hello **SQLIaaSExtension** rozšíření uvedené.</span><span class="sxs-lookup"><span data-stu-id="371fa-148">You should see hello **SQLIaaSExtension** extension listed.</span></span>

![Rozšíření IaaS agenta systému SQL Server na portálu Azure](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

<span data-ttu-id="371fa-150">Můžete taky hello **Get-AzureVMSqlServerExtension** rutiny Azure Powershellu.</span><span class="sxs-lookup"><span data-stu-id="371fa-150">You can also use hello **Get-AzureVMSqlServerExtension** Azure Powershell cmdlet.</span></span>

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

<span data-ttu-id="371fa-151">předchozí příkaz Hello potvrdí hello agenta je nainstalován a poskytuje obecné informace stavu.</span><span class="sxs-lookup"><span data-stu-id="371fa-151">hello previous command confirms hello agent is installed and provides general status information.</span></span> <span data-ttu-id="371fa-152">Také můžete získat informace o automatizované zálohování a oprav konkrétní stavu s hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="371fa-152">You can also get specific status information about Automated Backup and Patching with hello following commands.</span></span>

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a><span data-ttu-id="371fa-153">Odebrání</span><span class="sxs-lookup"><span data-stu-id="371fa-153">Removal</span></span>
<span data-ttu-id="371fa-154">V hello portálu Azure, můžete odinstalovat rozšíření hello kliknutím hello třemi tečkami na hello **rozšíření** okno vaší vlastnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="371fa-154">In hello Azure Portal, you can uninstall hello extension by clicking hello ellipsis on hello **Extensions** blade of your virtual machine properties.</span></span> <span data-ttu-id="371fa-155">Pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="371fa-155">Then click **Delete**.</span></span>

![Odinstalujte hello rozšíření IaaS agenta systému SQL Server na portálu Azure](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

<span data-ttu-id="371fa-157">Můžete taky hello **odebrat AzureRmVMSqlServerExtension** rutiny prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="371fa-157">You can also use hello **Remove-AzureRmVMSqlServerExtension** Powershell cmdlet.</span></span>

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a><span data-ttu-id="371fa-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="371fa-158">Next Steps</span></span>
<span data-ttu-id="371fa-159">Začnete používat jednu z hello služby podporované hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="371fa-159">Begin using one of hello services supported by hello extension.</span></span> <span data-ttu-id="371fa-160">Další podrobnosti najdete v tématu hello témata, kterou se odkazuje v hello [podporované služby](#supported-services) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="371fa-160">For more details, see hello topics referenced in hello [Supported services](#supported-services) section of this article.</span></span>

<span data-ttu-id="371fa-161">Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="371fa-161">For more information about running SQL Server on Azure Virtual Machines, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

