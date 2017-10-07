---
title: "úlohy správy aaaAutomate na virtuálních počítačích SQL (klasické) | Microsoft Docs"
description: "Toto téma popisuje, jak toomanage hello přípony agenta systému SQL Server, který automatizuje konkrétní úlohy správy systému SQL Server. Mezi ně patří automatizovaného zálohování, automatizovaných oprav a Azure Key Vault integrace. Toto téma používá režim hello nasazení classic."
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
ms.openlocfilehash: a1dc011e0526845701eaf0c365007938f9ee32ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-hello-sql-server-agent-extension-classic"></a><span data-ttu-id="fc2ed-105">Automatizaci úloh správy ve virtuálních počítačích Azure s hello rozšíření systému SQL Server Agent (klasické)</span><span class="sxs-lookup"><span data-stu-id="fc2ed-105">Automate management tasks on Azure Virtual Machines with hello SQL Server Agent Extension (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fc2ed-106">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="fc2ed-106">Resource Manager</span></span>](../sql/virtual-machines-windows-sql-server-agent-extension.md)
> * [<span data-ttu-id="fc2ed-107">Classic</span><span class="sxs-lookup"><span data-stu-id="fc2ed-107">Classic</span></span>](../classic/sql-server-agent-extension.md)
> 
>
 
<span data-ttu-id="fc2ed-108">Hello IaaS agenta rozšíření (SQLIaaSAgent) systému SQL Server spouští na úlohy správy tooautomate virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-108">hello SQL Server IaaS Agent Extension (SQLIaaSAgent) runs on Azure virtual machines tooautomate administration tasks.</span></span> <span data-ttu-id="fc2ed-109">Toto téma obsahuje přehled služby hello podporované hello rozšíření, jakož i pokyny k instalaci, stavu a odebrání.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-109">This topic provides an overview of hello services supported by hello extension as well as instructions for installation, status, and removal.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="fc2ed-110">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="fc2ed-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="fc2ed-111">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-111">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="fc2ed-112">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-112">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="fc2ed-113">tooview hello Resource Manager verze tohoto článku, najdete v části [rozšíření agenta systému SQL Server pro SQL Server virtuální počítače Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="fc2ed-113">tooview hello Resource Manager version of this article, see [SQL Server Agent Extension for SQL Server VMs Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md).</span></span>

## <a name="supported-services"></a><span data-ttu-id="fc2ed-114">Podporované služby</span><span class="sxs-lookup"><span data-stu-id="fc2ed-114">Supported services</span></span>
<span data-ttu-id="fc2ed-115">Hello rozšíření IaaS agenta systému SQL Server podporuje hello následující úlohy správy:</span><span class="sxs-lookup"><span data-stu-id="fc2ed-115">hello SQL Server IaaS Agent Extension supports hello following administration tasks:</span></span>

| <span data-ttu-id="fc2ed-116">Funkce správy</span><span class="sxs-lookup"><span data-stu-id="fc2ed-116">Administration feature</span></span> | <span data-ttu-id="fc2ed-117">Popis</span><span class="sxs-lookup"><span data-stu-id="fc2ed-117">Description</span></span> |
| --- | --- |
| <span data-ttu-id="fc2ed-118">**Automatizované zálohování SQL**</span><span class="sxs-lookup"><span data-stu-id="fc2ed-118">**SQL Automated Backup**</span></span> |<span data-ttu-id="fc2ed-119">Automatizuje hello plánování zálohování pro všechny databáze hello výchozí instanci systému SQL Server v hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-119">Automates hello scheduling of backups for all databases for hello default instance of SQL Server in hello VM.</span></span> <span data-ttu-id="fc2ed-120">Další informace najdete v tématu [automatizované zálohování pro SQL Server v Azure Virtual Machines (klasické)](../classic/sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="fc2ed-120">For more information, see [Automated backup for SQL Server in Azure Virtual Machines (Classic)](../classic/sql-automated-backup.md).</span></span> |
| <span data-ttu-id="fc2ed-121">**Automatizované opravy pro SQL**</span><span class="sxs-lookup"><span data-stu-id="fc2ed-121">**SQL Automated Patching**</span></span> |<span data-ttu-id="fc2ed-122">Konfiguraci časového období údržby, během které aktualizace umístit tooyour, může trvat virtuálních počítačů, tak, aby se můžete vyhnout aktualizace během špiček pro úlohy.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-122">Configures a maintenance window during which updates tooyour VM can take place, so  you can avoid updates during peak times for your workload.</span></span> <span data-ttu-id="fc2ed-123">Další informace najdete v tématu [automatizované opravy pro SQL Server v Azure Virtual Machines (klasické)](../classic/sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="fc2ed-123">For more information, see [Automated patching for SQL Server in Azure Virtual Machines (Classic)](../classic/sql-automated-patching.md).</span></span> |
| <span data-ttu-id="fc2ed-124">**Integrace se službou Azure Key Vault**</span><span class="sxs-lookup"><span data-stu-id="fc2ed-124">**Azure Key Vault Integration**</span></span> |<span data-ttu-id="fc2ed-125">Umožňuje vám tooautomatically instalace a konfigurace Azure Key Vault na virtuální počítač s SQL serverem.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-125">Enables you tooautomatically install and configure Azure Key Vault on your SQL Server VM.</span></span> <span data-ttu-id="fc2ed-126">Další informace najdete v tématu [nakonfigurovat klíč trezoru integrace se službou Azure pro systém SQL Server na virtuálních počítačích Azure (klasický)](../classic/ps-sql-keyvault.md).</span><span class="sxs-lookup"><span data-stu-id="fc2ed-126">For more information, see [Configure Azure Key Vault Integration for SQL Server on Azure VMs (Classic)](../classic/ps-sql-keyvault.md).</span></span> |

## <a name="prerequisites"></a><span data-ttu-id="fc2ed-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fc2ed-127">Prerequisites</span></span>
<span data-ttu-id="fc2ed-128">Požadavky na toouse hello rozšíření IaaS agenta systému SQL Server na vašem virtuálním počítači:</span><span class="sxs-lookup"><span data-stu-id="fc2ed-128">Requirements toouse hello SQL Server IaaS Agent Extension on your VM:</span></span>

### <a name="operating-system"></a><span data-ttu-id="fc2ed-129">Operační systém:</span><span class="sxs-lookup"><span data-stu-id="fc2ed-129">Operating System:</span></span>
* <span data-ttu-id="fc2ed-130">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="fc2ed-130">Windows Server 2012</span></span>
* <span data-ttu-id="fc2ed-131">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="fc2ed-131">Windows Server 2012 R2</span></span>
* <span data-ttu-id="fc2ed-132">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="fc2ed-132">Windows Server 2016</span></span>

### <a name="sql-server-versions"></a><span data-ttu-id="fc2ed-133">Verze systému SQL Server:</span><span class="sxs-lookup"><span data-stu-id="fc2ed-133">SQL Server versions:</span></span>
* <span data-ttu-id="fc2ed-134">SQL Server 2012</span><span class="sxs-lookup"><span data-stu-id="fc2ed-134">SQL Server 2012</span></span>
* <span data-ttu-id="fc2ed-135">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="fc2ed-135">SQL Server 2014</span></span>
* <span data-ttu-id="fc2ed-136">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="fc2ed-136">SQL Server 2016</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="fc2ed-137">Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="fc2ed-137">Azure PowerShell:</span></span>
<span data-ttu-id="fc2ed-138">[Stáhnout a nakonfigurovat Azure PowerShell příkazy nejnovější hello](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fc2ed-138">[Download and configure hello latest Azure PowerShell commands](/powershell/azure/overview).</span></span>

<span data-ttu-id="fc2ed-139">Spusťte prostředí Windows PowerShell a připojte ho tooyour předplatného Azure s hello **Add-AzureAccount** příkaz.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-139">Start Windows PowerShell, and connect it tooyour Azure subscription with hello **Add-AzureAccount** command.</span></span>

    Add-AzureAccount

<span data-ttu-id="fc2ed-140">Pokud máte více předplatných, použijte **Select-AzureSubscription** tooselect hello předplatné, které obsahuje cílový classic virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-140">If you have multiple subscriptions, use **Select-AzureSubscription** tooselect hello subscription that contains your target classic VM.</span></span>

    Select-AzureSubscription -SubscriptionName <subscriptionname>

<span data-ttu-id="fc2ed-141">V tomto okamžiku můžete získat seznam hello klasické virtuální počítače a jejich názvy přidružená služba s hello **Get-AzureVM** příkaz.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-141">At this point, you can get a list of hello classic virtual machines and their associated service names with hello **Get-AzureVM** command.</span></span>

    Get-AzureVM

## <a name="installation"></a><span data-ttu-id="fc2ed-142">Instalace</span><span class="sxs-lookup"><span data-stu-id="fc2ed-142">Installation</span></span>
<span data-ttu-id="fc2ed-143">Pro klasické virtuální počítače musíte použít PowerShell tooinstall hello rozšíření agenta systému SQL Server IaaS a nakonfigurovat jejích přidružených služeb.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-143">For classic VMs, you must use PowerShell tooinstall hello SQL Server IaaS Agent Extension and configure its associated services.</span></span> <span data-ttu-id="fc2ed-144">Použití hello **Set-AzureVMSqlServerExtension** rozšíření hello tooinstall rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-144">Use hello **Set-AzureVMSqlServerExtension** PowerShell cmdlet tooinstall hello extension.</span></span> <span data-ttu-id="fc2ed-145">Hello následující příkaz například nainstaluje hello rozšíření na virtuálním počítači Windows serveru (klasické) a pojmenuje ji "SQLIaaSExtension".</span><span class="sxs-lookup"><span data-stu-id="fc2ed-145">For example, hello following command installs hello extension on a Windows Server VM (classic) and names it "SQLIaaSExtension".</span></span>

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

<span data-ttu-id="fc2ed-146">Pokud aktualizujete toohello nejnovější verzi hello rozšíření agenta systému SQL IaaS, je nutné restartovat virtuální počítač po aktualizaci hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-146">If you update toohello latest version of hello SQL IaaS Agent Extension, you must restart your virtual machine after updating hello extension.</span></span>

> [!NOTE]
> <span data-ttu-id="fc2ed-147">Klasické virtuální počítače nemají možnost tooinstall a nakonfigurovat hello rozšíření agenta systému SQL IaaS prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-147">Classic virtual machines do not have an option tooinstall and configure hello SQL IaaS Agent Extension through hello portal.</span></span>
> 
> 

## <a name="status"></a><span data-ttu-id="fc2ed-148">Status</span><span class="sxs-lookup"><span data-stu-id="fc2ed-148">Status</span></span>
<span data-ttu-id="fc2ed-149">Jedním ze způsobů tooverify nainstalovanou hello rozšíření je stav agenta hello tooview hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-149">One way tooverify that hello extension is installed is tooview hello agent status in hello Azure Portal.</span></span> <span data-ttu-id="fc2ed-150">Vyberte virtuální počítač uvedený v okně hello virtuálního počítače a potom klikněte na **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-150">Select a virtual machine listed in hello virtual machine blade, and then click on **Extensions**.</span></span> <span data-ttu-id="fc2ed-151">Měli byste vidět hello **SQLIaaSAgent** rozšíření uvedené.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-151">You should see hello **SQLIaaSAgent** extension listed.</span></span>

![Rozšíření IaaS agenta systému SQL Server na portálu Azure](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

<span data-ttu-id="fc2ed-153">Můžete taky hello **Get-AzureVMSqlServerExtension** rutiny Azure Powershellu.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-153">You can also use hello **Get-AzureVMSqlServerExtension** Azure Powershell cmdlet.</span></span>

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a><span data-ttu-id="fc2ed-154">Odebrání</span><span class="sxs-lookup"><span data-stu-id="fc2ed-154">Removal</span></span>
<span data-ttu-id="fc2ed-155">V hello portálu Azure, můžete odinstalovat rozšíření hello kliknutím hello třemi tečkami na hello **rozšíření** okno vaší vlastnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-155">In hello Azure Portal, you can uninstall hello extension by clicking hello ellipsis on hello **Extensions** blade of your virtual machine properties.</span></span> <span data-ttu-id="fc2ed-156">Pak klikněte na tlačítko **odinstalovat**.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-156">Then click **Uninstall**.</span></span>

![Odinstalujte hello rozšíření IaaS agenta systému SQL Server na portálu Azure](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

<span data-ttu-id="fc2ed-158">Můžete taky hello **odebrat AzureVMSqlServerExtension** rutiny prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-158">You can also use hello **Remove-AzureVMSqlServerExtension** Powershell cmdlet.</span></span>

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="fc2ed-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fc2ed-159">Next Steps</span></span>
<span data-ttu-id="fc2ed-160">Začnete používat jednu z hello služby podporované hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-160">Begin using one of hello services supported by hello extension.</span></span> <span data-ttu-id="fc2ed-161">Další podrobnosti najdete v tématu hello témata, kterou se odkazuje v hello [podporované služby](#supported-services) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="fc2ed-161">For more details, see hello topics referenced in hello [Supported services](#supported-services) section of this article.</span></span>

<span data-ttu-id="fc2ed-162">Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fc2ed-162">For more information about running SQL Server on Azure Virtual Machines, see [SQL Server on Azure Virtual Machines overview](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

