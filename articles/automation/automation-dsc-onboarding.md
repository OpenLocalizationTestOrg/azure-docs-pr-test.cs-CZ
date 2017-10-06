---
title: "aaaOnboarding počítačů pro správu Azure Automation DSC. | Microsoft Docs"
description: "Jak toosetup počítačů pro správu pomocí Azure Automation DSC."
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
ms.assetid: da13e1f5-2a1c-443b-8e3b-9f0d6f9e4810
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 12/13/2016
ms.author: eslesar
ms.openlocfilehash: ef15801fec2ffea4ba62dcba2fbe9af09268e424
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="onboarding-machines-for-management-by-azure-automation-dsc"></a><span data-ttu-id="27c3c-103">Registrace počítačů pro správu Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="27c3c-103">Onboarding machines for management by Azure Automation DSC</span></span>

## <a name="why-manage-machines-with-azure-automation-dsc"></a><span data-ttu-id="27c3c-104">Proč spravovat počítače s Azure Automation DSC?</span><span class="sxs-lookup"><span data-stu-id="27c3c-104">Why manage machines with Azure Automation DSC?</span></span>

<span data-ttu-id="27c3c-105">Jako [konfigurace požadovaného stavu prostředí PowerShell](https://technet.microsoft.com/library/dn249912.aspx), konfigurace požadovaného stavu aplikace Azure Automation je jednoduché, ale výkonné, služba správy konfigurace pro uzly DSC (fyzické a virtuální počítače) ve všech cloudu nebo místně v datovém centru.</span><span class="sxs-lookup"><span data-stu-id="27c3c-105">Like [PowerShell Desired State Configuration](https://technet.microsoft.com/library/dn249912.aspx), Azure Automation Desired State Configuration is a simple, yet powerful, configuration management service for DSC nodes (physical and virtual machines) in any cloud or on-premises datacenter.</span></span> <span data-ttu-id="27c3c-106">Umožňuje škálovatelnost napříč tisíce počítačů snadno a rychle z centrální a zabezpečené umístění.</span><span class="sxs-lookup"><span data-stu-id="27c3c-106">It enables scalability across thousands of machines quickly and easily from a central, secure location.</span></span> <span data-ttu-id="27c3c-107">Můžete snadno připojit počítače, přiřaďte jim deklarativní konfigurace a zobrazovat sestavy zobrazující každý počítač je toohello požadovaného stavu dodržování předpisů, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="27c3c-107">You can easily onboard machines, assign them declarative configurations, and view reports showing each machine's compliance toohello desired state you specified.</span></span> <span data-ttu-id="27c3c-108">vrstva správy Hello Azure Automation DSC je tooDSC co vrstva správy Azure Automation hello je tooPowerShell skriptování.</span><span class="sxs-lookup"><span data-stu-id="27c3c-108">hello Azure Automation DSC management layer is tooDSC what hello Azure Automation management layer is tooPowerShell scripting.</span></span> <span data-ttu-id="27c3c-109">Jinými slovy v hello stejným způsobem, který Azure Automation umožňuje spravovat skripty prostředí PowerShell, také pomáhá při správě konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="27c3c-109">In other words, in hello same way that Azure Automation helps you manage PowerShell scripts, it also helps you manage DSC configurations.</span></span> <span data-ttu-id="27c3c-110">toolearn Další informace o hello výhody použití Azure Automation DSC, najdete v části [přehled Azure Automation DSC](automation-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="27c3c-110">toolearn more about hello benefits of using Azure Automation DSC, see [Azure Automation DSC overview](automation-dsc-overview.md).</span></span>

<span data-ttu-id="27c3c-111">Azure Automation DSC může být použité toomanage různých počítačích:</span><span class="sxs-lookup"><span data-stu-id="27c3c-111">Azure Automation DSC can be used toomanage a variety of machines:</span></span>

* <span data-ttu-id="27c3c-112">Virtuální počítače Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="27c3c-112">Azure virtual machines (classic)</span></span>
* <span data-ttu-id="27c3c-113">Virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="27c3c-113">Azure virtual machines</span></span>
* <span data-ttu-id="27c3c-114">Virtuální počítače Amazon Web Services (AWS)</span><span class="sxs-lookup"><span data-stu-id="27c3c-114">Amazon Web Services (AWS) virtual machines</span></span>
* <span data-ttu-id="27c3c-115">Windows fyzický nebo virtuální počítače na místních počítačích nebo v cloudu, než Azure nebo AWS</span><span class="sxs-lookup"><span data-stu-id="27c3c-115">Physical/virtual Windows machines on-premises, or in a cloud other than Azure/AWS</span></span>
* <span data-ttu-id="27c3c-116">Linux fyzický nebo virtuální počítače na místních počítačích v Azure nebo v cloudu, než Azure</span><span class="sxs-lookup"><span data-stu-id="27c3c-116">Physical/virtual Linux machines on-premises, in Azure, or in a cloud other than Azure</span></span>

<span data-ttu-id="27c3c-117">Kromě toho pokud si nejste připravené toomanage konfiguraci počítače z cloudu hello, Azure Automation DSC lze také jako koncový bod pouze sestavy.</span><span class="sxs-lookup"><span data-stu-id="27c3c-117">In addition, if you are not ready toomanage machine configuration from hello cloud, Azure Automation DSC can also be used as a report-only endpoint.</span></span> <span data-ttu-id="27c3c-118">To vám umožní tooset (push) požadované konfigurace pomocí DSC na místě a bohaté generování sestav zobrazit podrobnosti o dodržování předpisů uzlu s hello požadovaného stavu ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="27c3c-118">This allows you tooset (push) desired configuration through DSC on-premises and view rich reporting details on node compliance with hello desired state in Azure Automation.</span></span>

<span data-ttu-id="27c3c-119">Hello následující části popisují, jak můžete připojit každý typ počítač tooAzure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="27c3c-119">hello following sections outline how you can onboard each type of machine tooAzure Automation DSC.</span></span>

## <a name="azure-virtual-machines-classic"></a><span data-ttu-id="27c3c-120">Virtuální počítače Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="27c3c-120">Azure virtual machines (classic)</span></span>

<span data-ttu-id="27c3c-121">S Azure Automation DSC můžete snadno připojit virtuální počítače Azure (klasický) pro správu konfigurace pomocí hello portál Azure nebo PowerShell.</span><span class="sxs-lookup"><span data-stu-id="27c3c-121">With Azure Automation DSC, you can easily onboard Azure virtual machines (classic) for configuration management using either hello Azure portal, or PowerShell.</span></span> <span data-ttu-id="27c3c-122">Pod pokličkou hello a bez nutnosti tooremote do hello virtuálních počítačů správce hello konfigurace požadovaného stavu aplikace Azure virtuálního počítače rozšíření registruje hello virtuálních počítačů Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="27c3c-122">Under hello hood, and without an administrator having tooremote into hello VM, hello Azure VM Desired State Configuration extension registers hello VM with Azure Automation DSC.</span></span> <span data-ttu-id="27c3c-123">Vzhledem k tomu, že hello rozšíření konfigurace požadovaného stavu aplikace Azure virtuální počítač spustí asynchronně, kroky tootrack jejím průběhu nebo vyřešit potíže s jsou uvedeny v hello [ **řešení potíží s Azure virtuálního počítače registrace** ](#troubleshooting-azure-virtual-machine-onboarding)části níže.</span><span class="sxs-lookup"><span data-stu-id="27c3c-123">Since hello Azure VM Desired State Configuration extension runs asynchronously, steps tootrack its progress or troubleshoot it are provided in hello [**Troubleshooting Azure virtual machine onboarding**](#troubleshooting-azure-virtual-machine-onboarding) section below.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="27c3c-124">portál Azure</span><span class="sxs-lookup"><span data-stu-id="27c3c-124">Azure portal</span></span>

<span data-ttu-id="27c3c-125">V hello [portál Azure](http://portal.azure.com/), klikněte na tlačítko **Procházet** -> **virtuálních počítačů (klasické)**.</span><span class="sxs-lookup"><span data-stu-id="27c3c-125">In hello [Azure portal](http://portal.azure.com/), click **Browse** -> **Virtual machines (classic)**.</span></span> <span data-ttu-id="27c3c-126">Vyberte hello tooonboard chcete virtuální počítač s Windows.</span><span class="sxs-lookup"><span data-stu-id="27c3c-126">Select hello Windows VM you want tooonboard.</span></span> <span data-ttu-id="27c3c-127">V okně řídicí panel hello virtuální počítač, klikněte na tlačítko **všechna nastavení** -> **rozšíření** -> **přidat**  ->   **Služby Azure Automation DSC** -> **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="27c3c-127">On hello virtual machine's dashboard blade, click **All settings** -> **Extensions** -> **Add** -> **Azure Automation DSC** -> **Create**.</span></span> <span data-ttu-id="27c3c-128">Zadejte hello [správce místní konfigurace DSC prostředí PowerShell hodnoty](https://msdn.microsoft.com/powershell/dsc/metaconfig4) požadované pro váš případ použití, účet Automation registrační klíč a adresa URL registrace a volitelně toohello tooassign konfigurace uzlu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="27c3c-128">Enter hello [PowerShell DSC Local Configuration Manager values](https://msdn.microsoft.com/powershell/dsc/metaconfig4) required for your use case, your Automation account's registration key and registration URL, and optionally a node configuration tooassign toohello VM.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_1.png)

<span data-ttu-id="27c3c-129">Adresa URL registrace hello toofind a klíč pro počítač hello hello automatizace účet tooonboard, najdete v části hello [ **zabezpečení registrace** ](#secure-registration) části níže.</span><span class="sxs-lookup"><span data-stu-id="27c3c-129">toofind hello registration URL and key for hello Automation account tooonboard hello machine to, see hello [**Secure registration**](#secure-registration) section below.</span></span>

### <a name="powershell"></a><span data-ttu-id="27c3c-130">PowerShell</span><span class="sxs-lookup"><span data-stu-id="27c3c-130">PowerShell</span></span>

```powershell
# log in tooboth Azure Service Management and Azure Resource Manager
Add-AzureAccount
Add-AzureRmAccount

# fill in correct values for your VM/Automation account here
$VMName = ""
$ServiceName = ""
$AutomationAccountName = ""
$AutomationAccountResourceGroup = ""

# fill in hello name of a Node Configuration in Azure Automation DSC, for this VM tooconform to
$NodeConfigName = ""

# get Azure Automation DSC registration info
$Account = Get-AzureRmAutomationAccount -ResourceGroupName $AutomationAccountResourceGroup -Name $AutomationAccountName
$RegistrationInfo = $Account | Get-AzureRmAutomationRegistrationInfo

# use hello DSC extension tooonboard hello VM for management with Azure Automation DSC
$VM = Get-AzureVM -Name $VMName -ServiceName $ServiceName

$PublicConfiguration = ConvertTo-Json -Depth 8 @{
    SasToken = ""
    ModulesUrl = "https://eus2oaasibizamarketprod1.blob.core.windows.net/automationdscpreview/RegistrationMetaConfigV2.zip"
    ConfigurationFunction = "RegistrationMetaConfigV2.ps1\RegistrationMetaConfigV2"

# update these PowerShell DSC Local Configuration Manager defaults if they do not match your use case.
# See https://technet.microsoft.com/library/dn249922.aspx?f=255&MSPPError=-2147217396 for more details
    Properties = @{
    RegistrationKey = @{
        UserName = 'notused'
        Password = 'PrivateSettingsRef:RegistrationKey'
    }
    RegistrationUrl = $RegistrationInfo.Endpoint
    NodeConfigurationName = $NodeConfigName
    ConfigurationMode = "ApplyAndMonitor"
    ConfigurationModeFrequencyMins = 15
    RefreshFrequencyMins = 30
    RebootNodeIfNeeded = $False
    ActionAfterReboot = "ContinueConfiguration"
    AllowModuleOverwrite = $False
    }
}

$PrivateConfiguration = ConvertTo-Json -Depth 8 @{
    Items = @{
        RegistrationKey = $RegistrationInfo.PrimaryKey
    }
}

$VM = Set-AzureVMExtension `
    -VM $vm `
    -Publisher Microsoft.Powershell `
    -ExtensionName DSC `
    -Version 2.19 `
    -PublicConfiguration $PublicConfiguration `
    -PrivateConfiguration $PrivateConfiguration `
    -ForceUpdate

$VM | Update-AzureVM
```

## <a name="azure-virtual-machines"></a><span data-ttu-id="27c3c-131">Virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="27c3c-131">Azure virtual machines</span></span>

<span data-ttu-id="27c3c-132">Azure Automation DSC umožňuje snadno připojit virtuální počítače Azure za účelem správy konfigurace, pomocí hello portálu Azure, šablon Azure Resource Manageru nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="27c3c-132">Azure Automation DSC lets you easily onboard Azure virtual machines for configuration management, using either hello Azure portal, Azure Resource Manager templates, or PowerShell.</span></span> <span data-ttu-id="27c3c-133">Pod pokličkou hello a bez nutnosti tooremote do hello virtuálních počítačů správce hello konfigurace požadovaného stavu aplikace Azure virtuálního počítače rozšíření registruje hello virtuálních počítačů Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="27c3c-133">Under hello hood, and without an administrator having tooremote into hello VM, hello Azure VM Desired State Configuration extension registers hello VM with Azure Automation DSC.</span></span> <span data-ttu-id="27c3c-134">Vzhledem k tomu, že hello rozšíření konfigurace požadovaného stavu aplikace Azure virtuální počítač spustí asynchronně, kroky tootrack jejím průběhu nebo vyřešit potíže s jsou uvedeny v hello [ **řešení potíží s Azure virtuálního počítače registrace** ](#troubleshooting-azure-virtual-machine-onboarding)části níže.</span><span class="sxs-lookup"><span data-stu-id="27c3c-134">Since hello Azure VM Desired State Configuration extension runs asynchronously, steps tootrack its progress or troubleshoot it are provided in hello [**Troubleshooting Azure virtual machine onboarding**](#troubleshooting-azure-virtual-machine-onboarding) section below.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="27c3c-135">portál Azure</span><span class="sxs-lookup"><span data-stu-id="27c3c-135">Azure portal</span></span>

<span data-ttu-id="27c3c-136">V hello [portál Azure](https://portal.azure.com/), přejděte účet Azure Automation toohello místo tooonboard virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="27c3c-136">In hello [Azure portal](https://portal.azure.com/), navigate toohello Azure Automation account where you want tooonboard virtual machines.</span></span> <span data-ttu-id="27c3c-137">Na řídicím panelu účet Automation hello, klikněte na tlačítko **uzly DSC** -> **přidat virtuální počítač Azure**.</span><span class="sxs-lookup"><span data-stu-id="27c3c-137">On hello Automation account dashboard, click **DSC Nodes** -> **Add Azure VM**.</span></span>

<span data-ttu-id="27c3c-138">V části **vyberte virtuální počítače tooonboard**, vyberte jednu nebo více Azure virtuální počítače tooonboard.</span><span class="sxs-lookup"><span data-stu-id="27c3c-138">Under **Select virtual machines tooonboard**, select one or more Azure virtual machines tooonboard.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_2.png)

<span data-ttu-id="27c3c-139">V části **konfigurace registrační data**, zadejte hello [správce místní konfigurace DSC prostředí PowerShell hodnoty](https://msdn.microsoft.com/powershell/dsc/metaconfig4) vyžadované pro váš případ použití a volitelně toohello tooassign konfigurace uzlu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="27c3c-139">Under **Configure registration data**, enter hello [PowerShell DSC Local Configuration Manager values](https://msdn.microsoft.com/powershell/dsc/metaconfig4) required for your use case, and optionally a node configuration tooassign toohello VM.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_3.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="27c3c-140">Šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="27c3c-140">Azure Resource Manager templates</span></span>

<span data-ttu-id="27c3c-141">Virtuální počítače Azure můžete nasadit a zařazený nemá tooAzure Automation DSC pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="27c3c-141">Azure virtual machines can be deployed and onboarded tooAzure Automation DSC via Azure Resource Manager templates.</span></span> <span data-ttu-id="27c3c-142">V tématu [konfigurace virtuálního počítače prostřednictvím rozšíření DSC a Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) pro šablonu příklad této onboards existující virtuální počítač tooAzure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="27c3c-142">See [Configure a VM via DSC extension and Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) for an example template that onboards an existing VM tooAzure Automation DSC.</span></span> <span data-ttu-id="27c3c-143">toofind hello registrační klíč a registraci adresy URL prováděné jako vstup v této šabloně najdete v části hello [ **zabezpečení registrace** ](#secure-registration) části níže.</span><span class="sxs-lookup"><span data-stu-id="27c3c-143">toofind hello registration key and registration URL taken as input in this template, see hello [**Secure registration**](#secure-registration) section below.</span></span>

### <a name="powershell"></a><span data-ttu-id="27c3c-144">PowerShell</span><span class="sxs-lookup"><span data-stu-id="27c3c-144">PowerShell</span></span>

<span data-ttu-id="27c3c-145">Hello [Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) rutiny lze použít tooonboard virtuálních počítačů v hello portálu Azure pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="27c3c-145">hello [Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) cmdlet can be used tooonboard virtual machines in hello Azure portal via PowerShell.</span></span>

## <a name="amazon-web-services-aws-virtual-machines"></a><span data-ttu-id="27c3c-146">Virtuální počítače Amazon Web Services (AWS)</span><span class="sxs-lookup"><span data-stu-id="27c3c-146">Amazon Web Services (AWS) virtual machines</span></span>

<span data-ttu-id="27c3c-147">Můžete snadno připojit Amazon Web Services virtuálních počítačů pro správu konfigurace pomocí hello AWS DSC Toolkit Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="27c3c-147">You can easily onboard Amazon Web Services virtual machines for configuration management by Azure Automation DSC using hello AWS DSC Toolkit.</span></span> <span data-ttu-id="27c3c-148">Další informace o hello toolkit [zde](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).</span><span class="sxs-lookup"><span data-stu-id="27c3c-148">You can learn more about hello toolkit [here](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).</span></span>

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws"></a><span data-ttu-id="27c3c-149">Windows fyzický nebo virtuální počítače na místních počítačích nebo v cloudu, než Azure nebo AWS</span><span class="sxs-lookup"><span data-stu-id="27c3c-149">Physical/virtual Windows machines on-premises, or in a cloud other than Azure/AWS</span></span>

<span data-ttu-id="27c3c-150">Místní počítače s Windows a počítače s Windows v cloudech mimo Azure (například Amazon Web Services) může být také zařazený nemá tooAzure Automation DSC, tak dlouho, dokud mají toohello odchozí přístup k Internetu prostřednictvím několika jednoduchých kroků:</span><span class="sxs-lookup"><span data-stu-id="27c3c-150">On-premises Windows machines and Windows machines in non-Azure clouds (such as Amazon Web Services) can also be onboarded tooAzure Automation DSC, as long as they have outbound access toohello internet, via a few simple steps:</span></span>

1. <span data-ttu-id="27c3c-151">Ujistěte se, zda text hello nejnovější verzi [WMF 5](http://aka.ms/wmf5latest) je nainstalován na počítačích hello chcete tooonboard tooAzure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="27c3c-151">Make sure hello latest version of [WMF 5](http://aka.ms/wmf5latest) is installed on hello machines you want tooonboard tooAzure Automation DSC.</span></span>
2. <span data-ttu-id="27c3c-152">Postupujte podle pokynů hello v části [ **generování DSC metaconfigurations** ](#generating-dsc-metaconfigurations) pod toogenerate složku obsahující hello potřeby DSC metaconfigurations.</span><span class="sxs-lookup"><span data-stu-id="27c3c-152">Follow hello directions in section [**Generating DSC metaconfigurations**](#generating-dsc-metaconfigurations) below toogenerate a folder containing hello needed DSC metaconfigurations.</span></span>
3. <span data-ttu-id="27c3c-153">Vzdáleně použijte hello DSC Powershellu metakonfiguraci toohello počítače chcete tooonboard.</span><span class="sxs-lookup"><span data-stu-id="27c3c-153">Remotely apply hello PowerShell DSC metaconfiguration toohello machines you want tooonboard.</span></span> <span data-ttu-id="27c3c-154">**Tento příkaz spustí z počítače Hello musí mít hello nejnovější verze [WMF 5](http://aka.ms/wmf5latest) nainstalován**:</span><span class="sxs-lookup"><span data-stu-id="27c3c-154">**hello machine this command is run from must have hello latest version of [WMF 5](http://aka.ms/wmf5latest) installed**:</span></span>

    ```powershell
    Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
    ```

4. <span data-ttu-id="27c3c-155">Pokud nemůžete použít metaconfigurations hello DSC Powershellu vzdáleně, zkopírujte složku metaconfigurations hello z kroku 2 na každý počítač tooonboard.</span><span class="sxs-lookup"><span data-stu-id="27c3c-155">If you cannot apply hello PowerShell DSC metaconfigurations remotely, copy hello metaconfigurations folder from step 2 onto each machine tooonboard.</span></span> <span data-ttu-id="27c3c-156">Potom zavolejte **Set-DscLocalConfigurationManager** místně na každý počítač tooonboard.</span><span class="sxs-lookup"><span data-stu-id="27c3c-156">Then call **Set-DscLocalConfigurationManager** locally on each machine tooonboard.</span></span>
5. <span data-ttu-id="27c3c-157">Použití hello portál Azure nebo rutin zkontrolujte, že hello počítače tooonboard nyní se zobrazí jako uzly DSC registrované ve vašem účtu Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="27c3c-157">Using hello Azure portal or cmdlets, check that hello machines tooonboard now show up as DSC nodes registered in your Azure Automation account.</span></span>

## <a name="physicalvirtual-linux-machines-on-premises-in-azure-or-in-a-cloud-other-than-azure"></a><span data-ttu-id="27c3c-158">Linux fyzický nebo virtuální počítače na místních počítačích v Azure nebo v cloudu, než Azure</span><span class="sxs-lookup"><span data-stu-id="27c3c-158">Physical/virtual Linux machines on-premises, in Azure, or in a cloud other than Azure</span></span>

<span data-ttu-id="27c3c-159">Linux místní počítače, počítače s Linuxem v Azure, a počítače se systémem Linux v cloudech mimo Azure může být také zařazený nemá tooAzure Automation DSC, tak dlouho, dokud mají toohello odchozí přístup k Internetu prostřednictvím několika jednoduchých kroků:</span><span class="sxs-lookup"><span data-stu-id="27c3c-159">On-premises Linux machines, Linux machines in Azure, and Linux machines in non-Azure clouds can also be onboarded tooAzure Automation DSC, as long as they have outbound access toohello internet, via a few simple steps:</span></span>

1. <span data-ttu-id="27c3c-160">Ujistěte se, zda text hello nejnovější verzi [prostředí PowerShell konfigurace požadovaného stavu pro Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) je nainstalován na počítačích hello chcete tooonboard tooAzure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="27c3c-160">Make sure hello latest version of [PowerShell Desired State Configuration for Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) is installed on hello machines you want tooonboard tooAzure Automation DSC.</span></span>
2. <span data-ttu-id="27c3c-161">Pokud hello [správce místní konfigurace DSC prostředí PowerShell výchozí](https://msdn.microsoft.com/powershell/dsc/metaconfig4) odpovídající váš případ použití, a chcete, aby tooonboard počítače tak, že se **obě** načítat z a informovat tooAzure Automation DSC:</span><span class="sxs-lookup"><span data-stu-id="27c3c-161">If hello [PowerShell DSC Local Configuration Manager defaults](https://msdn.microsoft.com/powershell/dsc/metaconfig4) match your use case, and you want tooonboard machines such that they **both** pull from and report tooAzure Automation DSC:</span></span>

   + <span data-ttu-id="27c3c-162">Na každý Linux počítač tooonboard tooAzure Automation DSC použijte Register.py tooonboard pomocí hello výchozí správce místní konfigurace DSC prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="27c3c-162">On each Linux machine tooonboard tooAzure Automation DSC, use Register.py tooonboard using hello PowerShell DSC Local Configuration Manager defaults:</span></span>

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   + <span data-ttu-id="27c3c-163">toofind hello registrační klíč a registraci adresy URL u vašeho účtu Automation, najdete v části hello [ **zabezpečení registrace** ](#secure-registration) části níže.</span><span class="sxs-lookup"><span data-stu-id="27c3c-163">toofind hello registration key and registration URL for your Automation account, see hello [**Secure registration**](#secure-registration) section below.</span></span>

     <span data-ttu-id="27c3c-164">Pokud hello správce místní konfigurace DSC prostředí PowerShell výchozí **provést** **není** odpovídat váš případ použití, nebo chcete tooonboard počítače tak, aby se pouze sestavy tooAzure Automation DSC, ale není pro vyžádání obsahu konfigurace nebo moduly Powershellu z něj, postupujte podle kroků 3 až 6.</span><span class="sxs-lookup"><span data-stu-id="27c3c-164">If hello PowerShell DSC Local Configuration Manager defaults **do** **not** match your use case, or you want tooonboard machines such that they only report tooAzure Automation DSC, but do not pull configuration or PowerShell modules from it,  follow steps 3 - 6.</span></span> <span data-ttu-id="27c3c-165">Jinak přejděte přímo toostep 6.</span><span class="sxs-lookup"><span data-stu-id="27c3c-165">Otherwise, proceed directly toostep 6.</span></span>

3. <span data-ttu-id="27c3c-166">Postupujte podle pokynů hello v hello [ **generování DSC metaconfigurations** ](#generating-dsc-metaconfigurations) části níže toogenerate složku obsahující potřebné hello DSC metaconfigurations.</span><span class="sxs-lookup"><span data-stu-id="27c3c-166">Follow hello directions in hello [**Generating DSC metaconfigurations**](#generating-dsc-metaconfigurations) section below toogenerate a folder containing hello needed DSC metaconfigurations.</span></span>
4. <span data-ttu-id="27c3c-167">Vzdáleně použít hello DSC Powershellu metakonfiguraci toohello počítače chcete tooonboard:</span><span class="sxs-lookup"><span data-stu-id="27c3c-167">Remotely apply hello PowerShell DSC metaconfiguration toohello machines you want tooonboard:</span></span>

    ```powershell
    $SecurePass = ConvertTo-SecureString -String "<root password>" -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential "root", $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine tooonboard

    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

<span data-ttu-id="27c3c-168">Tento příkaz spustí z počítače Hello musí mít hello nejnovější verze [WMF 5](http://aka.ms/wmf5latest) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="27c3c-168">hello machine this command is run from must have hello latest version of [WMF 5](http://aka.ms/wmf5latest) installed.</span></span>

1. <span data-ttu-id="27c3c-169">Pokud nemůžete použít metaconfigurations hello DSC Powershellu vzdáleně, pro každý počítač tooonboard Linux, zkopírujte ze složky hello v kroku 5 do počítače Linux hello hello metakonfiguraci odpovídající toothat počítač.</span><span class="sxs-lookup"><span data-stu-id="27c3c-169">If you cannot apply hello PowerShell DSC metaconfigurations remotely, for each Linux machine tooonboard, copy hello metaconfiguration corresponding toothat machine from hello folder in step 5 onto hello Linux machine.</span></span> <span data-ttu-id="27c3c-170">Potom zavolejte `SetDscLocalConfigurationManager.py` místně na každém počítači Linux chcete tooonboard tooAzure Automation DSC:</span><span class="sxs-lookup"><span data-stu-id="27c3c-170">Then call `SetDscLocalConfigurationManager.py` locally on each Linux machine you want tooonboard tooAzure Automation DSC:</span></span>

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path toometaconfiguration file>`

2. <span data-ttu-id="27c3c-171">Použití hello portál Azure nebo rutin zkontrolujte, že hello počítače tooonboard nyní se zobrazí jako uzly DSC registrované ve vašem účtu Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="27c3c-171">Using hello Azure portal or cmdlets, check that hello machines tooonboard now show up as DSC nodes registered in your Azure Automation account.</span></span>

## <a name="generating-dsc-metaconfigurations"></a><span data-ttu-id="27c3c-172">Generování DSC metaconfigurations</span><span class="sxs-lookup"><span data-stu-id="27c3c-172">Generating DSC metaconfigurations</span></span>

<span data-ttu-id="27c3c-173">toogenerically zaváděním některý počítač tooAzure Automation DSC, [metakonfiguraci DSC](https://msdn.microsoft.com/en-us/powershell/dsc/metaconfig) může být, vygeneruje, když použijí, říká agent hello DSC na počítače toopull hello z nebo sestavy tooAzure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="27c3c-173">toogenerically onboard any machine tooAzure Automation DSC, a [DSC metaconfiguration](https://msdn.microsoft.com/en-us/powershell/dsc/metaconfig) can be generated that, when applied, tells hello DSC agent on hello machine toopull from and/or report tooAzure Automation DSC.</span></span> <span data-ttu-id="27c3c-174">Metaconfigurations DSC pro Azure Automation DSC můžete vygenerovat pomocí buď konfigurace DSC prostředí PowerShell nebo rutin Powershellu pro Azure Automation hello.</span><span class="sxs-lookup"><span data-stu-id="27c3c-174">DSC metaconfigurations for Azure Automation DSC can be generated using either a PowerShell DSC configuration, or hello Azure Automation PowerShell cmdlets.</span></span>

> [!NOTE]
> <span data-ttu-id="27c3c-175">DSC metaconfigurations obsahovat hello tajné klíče potřeby tooonboard počítač tooan účet automatizace pro správu.</span><span class="sxs-lookup"><span data-stu-id="27c3c-175">DSC metaconfigurations contain hello secrets needed tooonboard a machine tooan Automation account for management.</span></span> <span data-ttu-id="27c3c-176">Ujistěte se, že tooproperly chránit všechny metaconfigurations DSC, které vytvoříte, nebo je odstranit po použití.</span><span class="sxs-lookup"><span data-stu-id="27c3c-176">Make sure tooproperly protect any DSC metaconfigurations you create, or delete them after use.</span></span>

### <a name="using-a-dsc-configuration"></a><span data-ttu-id="27c3c-177">Používání konfigurace DSC</span><span class="sxs-lookup"><span data-stu-id="27c3c-177">Using a DSC Configuration</span></span>

1. <span data-ttu-id="27c3c-178">V počítači ve vašem místním prostředí otevřete hello ISE prostředí PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="27c3c-178">Open hello PowerShell ISE as an administrator in a machine in your local environment.</span></span> <span data-ttu-id="27c3c-179">Hello počítač musí mít nejnovější verzi hello [WMF 5](http://aka.ms/wmf5latest) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="27c3c-179">hello machine must have hello latest version of [WMF 5](http://aka.ms/wmf5latest) installed.</span></span>
2. <span data-ttu-id="27c3c-180">Zkopírujte následující skript místně hello.</span><span class="sxs-lookup"><span data-stu-id="27c3c-180">Copy hello following script locally.</span></span> <span data-ttu-id="27c3c-181">Tento skript obsahuje konfiguraci DSC prostředí PowerShell pro vytváření metaconfigurations a tookick příkaz vypnout vytváření metakonfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="27c3c-181">This script contains a PowerShell DSC configuration for creating metaconfigurations, and a command tookick off hello metaconfiguration creation.</span></span>

    ```powershell
    # hello DSC configuration that will generate metaconfigurations
    [DscLocalConfigurationManager()]
    Configuration DscMetaConfigs
    {

        param
        (
            [Parameter(Mandatory=$True)]
            [String]$RegistrationUrl,

            [Parameter(Mandatory=$True)]
            [String]$RegistrationKey,

            [Parameter(Mandatory=$True)]
            [String[]]$ComputerName,

            [Int]$RefreshFrequencyMins = 30,

            [Int]$ConfigurationModeFrequencyMins = 15,

            [String]$ConfigurationMode = "ApplyAndMonitor",

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = "ContinueConfiguration",

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq "")
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
        $RefreshMode = "PUSH"
        }
        else
        {
        $RefreshMode = "PULL"
        }

        Node $ComputerName
        {

            Settings
            {
                RefreshFrequencyMins = $RefreshFrequencyMins
                RefreshMode = $RefreshMode
                ConfigurationMode = $ConfigurationMode
                AllowModuleOverwrite = $AllowModuleOverwrite
                RebootNodeIfNeeded = $RebootNodeIfNeeded
                ActionAfterReboot = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationDSC
                {
                    ServerUrl = $RegistrationUrl
                    RegistrationKey = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationDSC
                {
                ServerUrl = $RegistrationUrl
                RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationDSC
            {
                ServerUrl = $RegistrationUrl
                RegistrationKey = $RegistrationKey
            }
        }
    }

    # Create hello metaconfigurations
    # TODO: edit hello below as needed for your use case
    $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM tooonboard>', '<some other VM tooonboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set too$True toohave machines only report tooAA DSC but not pull from it
    }

    # Use PowerShell splatting toopass parameters toohello DSC configuration being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    DscMetaConfigs @Params
    ```

3. <span data-ttu-id="27c3c-182">Vyplňte hello registrační klíč a adresu URL pro váš účet Automation, jakož i hello názvy počítačů tooonboard hello.</span><span class="sxs-lookup"><span data-stu-id="27c3c-182">Fill in hello registration key and URL for your Automation account, as well as hello names of hello machines tooonboard.</span></span> <span data-ttu-id="27c3c-183">Všechny ostatní parametry jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="27c3c-183">All other parameters are optional.</span></span> <span data-ttu-id="27c3c-184">toofind hello registrační klíč a registraci adresy URL u vašeho účtu Automation, najdete v části hello [ **zabezpečení registrace** ](#secure-registration) části níže.</span><span class="sxs-lookup"><span data-stu-id="27c3c-184">toofind hello registration key and registration URL for your Automation account, see hello [**Secure registration**](#secure-registration) section below.</span></span>
4. <span data-ttu-id="27c3c-185">Pokud chcete hello počítače tooreport DSC stavové informace tooAzure Automation DSC, ale není pro vyžádání obsahu konfigurace nebo moduly Powershellu, nastavte hello **ReportOnly** tootrue parametr.</span><span class="sxs-lookup"><span data-stu-id="27c3c-185">If you want hello machines tooreport DSC status information tooAzure Automation DSC, but not pull configuration or PowerShell modules, set hello **ReportOnly** parameter tootrue.</span></span>
5. <span data-ttu-id="27c3c-186">Spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="27c3c-186">Run hello script.</span></span> <span data-ttu-id="27c3c-187">Teď byste měli mít složku s názvem **DscMetaConfigs** v pracovním adresáři, který obsahuje metaconfigurations hello PowerShell DSC pro počítače tooonboard hello (jako správce):</span><span class="sxs-lookup"><span data-stu-id="27c3c-187">You should now have a folder called **DscMetaConfigs** in your working directory, containing hello PowerShell DSC metaconfigurations for hello machines tooonboard (as an administrator):</span></span>

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-hello-azure-automation-cmdlets"></a><span data-ttu-id="27c3c-188">Pomocí rutin hello Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="27c3c-188">Using hello Azure Automation cmdlets</span></span>

<span data-ttu-id="27c3c-189">Pokud výchozí nastavení správce místní konfigurace DSC prostředí PowerShell hello odpovídá váš případ použití a chcete tooonboard počítače tak, aby se načítat z i vykazování tooAzure Automation DSC, rutiny Azure Automation hello poskytují zjednodušenou metodu generování hello DSC metaconfigurations potřebné:</span><span class="sxs-lookup"><span data-stu-id="27c3c-189">If hello PowerShell DSC Local Configuration Manager defaults match your use case, and you want tooonboard machines such that they both pull from and report tooAzure Automation DSC, hello Azure Automation cmdlets provide a simplified method of generating hello DSC metaconfigurations needed:</span></span>

1. <span data-ttu-id="27c3c-190">Otevřete konzolu PowerShell hello nebo ISE prostředí PowerShell jako správce v počítači ve vašem místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="27c3c-190">Open hello PowerShell console or PowerShell ISE as an administrator in a machine in your local environment.</span></span>
2. <span data-ttu-id="27c3c-191">Připojit pomocí Správce prostředků tooAzure **Add-AzureRmAccount**</span><span class="sxs-lookup"><span data-stu-id="27c3c-191">Connect tooAzure Resource Manager using **Add-AzureRmAccount**</span></span>
3. <span data-ttu-id="27c3c-192">Stáhněte metaconfigurations hello PowerShell DSC pro počítače hello chcete tooonboard z hello automatizace účet toowhich chcete tooonboard uzly:</span><span class="sxs-lookup"><span data-stu-id="27c3c-192">Download hello PowerShell DSC metaconfigurations for hello machines you want tooonboard from hello Automation account toowhich you want tooonboard nodes:</span></span>

    ```powershell
    # Define hello parameters for Get-AzureRmAutomationDscOnboardingMetaconfig using PowerShell Splatting
    $Params = @{

        ResourceGroupName = 'ContosoResources'; # hello name of hello ARM Resource Group that contains your Azure Automation Account
        AutomationAccountName = 'ContosoAutomation'; # hello name of hello Azure Automation Account where you want a node on-boarded to
        ComputerName = @('web01', 'web02', 'sql01'); # hello names of hello computers that hello meta configuration will be generated for
        OutputFolder = "$env:UserProfile\Desktop\";
    }
    # Use PowerShell splatting toopass parameters toohello Azure Automation cmdlet being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    Get-AzureRmAutomationDscOnboardingMetaconfig @Params
    ```
    
4. <span data-ttu-id="27c3c-193">Teď byste měli mít složku s názvem ***DscMetaConfigs***, obsahující hello metaconfigurations PowerShell DSC pro počítače tooonboard hello (jako správce):</span><span class="sxs-lookup"><span data-stu-id="27c3c-193">You should now have a folder called ***DscMetaConfigs***, containing hello PowerShell DSC metaconfigurations for hello machines tooonboard (as an administrator):</span></span>
    
    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a><span data-ttu-id="27c3c-194">Zabezpečené registrace</span><span class="sxs-lookup"><span data-stu-id="27c3c-194">Secure registration</span></span>

<span data-ttu-id="27c3c-195">Počítače můžete bezpečně zařadit tooan účet Azure Automation prostřednictvím hello WMF 5 DSC registrace protokol, který umožňuje DSC uzlu tooauthenticate tooa prostředí PowerShell DSC V2 Pull nebo vytváření sestav serveru (včetně Azure Automation DSC).</span><span class="sxs-lookup"><span data-stu-id="27c3c-195">Machines can securely onboard tooan Azure Automation account through hello WMF 5 DSC registration protocol, which allows a DSC node tooauthenticate tooa PowerShell DSC V2 Pull or Reporting server (including Azure Automation DSC).</span></span> <span data-ttu-id="27c3c-196">uzel Hello zaregistruje server toohello na **URL pro registraci**, ověřování pomocí **registrační klíč**.</span><span class="sxs-lookup"><span data-stu-id="27c3c-196">hello node registers toohello server at a **Registration URL**, authenticating using a **Registration key**.</span></span> <span data-ttu-id="27c3c-197">Během registrace vyjednat jedinečný certifikát pro tento uzel toouse pro ověřování toohello serveru po registraci uzlu hello DSC a server pro vyžádání obsahu nebo sestavy DSC.</span><span class="sxs-lookup"><span data-stu-id="27c3c-197">During registration, hello DSC node and DSC Pull/Reporting server negotiate a unique certificate for this node toouse for authentication toohello server post-registration.</span></span> <span data-ttu-id="27c3c-198">Tento proces zabraňuje zařazený nemá uzly z jednoho jiné, jako je například pokud uzel dojde k ohrožení bezpečnosti zosobnění a chovají závadně.</span><span class="sxs-lookup"><span data-stu-id="27c3c-198">This process prevents onboarded nodes from impersonating one another, such as if a node is compromised and behaving maliciously.</span></span> <span data-ttu-id="27c3c-199">Po registraci hello registrační klíč se nepoužije pro ověřování znovu a se odstraní z uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="27c3c-199">After registration, hello Registration key is not used for authentication again, and is deleted from hello node.</span></span>

<span data-ttu-id="27c3c-200">Můžete získat hello informace požadované pro protokol registrace hello DSC z hello **Správa klíčů** okno na portálu Azure preview hello.</span><span class="sxs-lookup"><span data-stu-id="27c3c-200">You can get hello information required for hello DSC registration protocol from hello **Manage Keys** blade in hello Azure preview portal.</span></span> <span data-ttu-id="27c3c-201">Otevřete toto okno kliknutím na ikonu klíče hello na hello **Essentials** panel pro hello účet Automation.</span><span class="sxs-lookup"><span data-stu-id="27c3c-201">Open this blade by clicking hello key icon on hello **Essentials** panel for hello Automation account.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

* <span data-ttu-id="27c3c-202">Adresa URL registrace je hello pole adresy URL v okně Správa klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="27c3c-202">Registration URL is hello URL field in hello Manage Keys blade.</span></span>
* <span data-ttu-id="27c3c-203">Registrační klíč je hello primární přístupový klíč nebo sekundární přístupový klíč v okně Správa klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="27c3c-203">Registration key is hello Primary Access Key or Secondary Access Key in hello Manage Keys blade.</span></span> <span data-ttu-id="27c3c-204">Můžete použít buď klíč.</span><span class="sxs-lookup"><span data-stu-id="27c3c-204">Either key can be used.</span></span>

<span data-ttu-id="27c3c-205">Pro zvýšení zabezpečení hello primární a sekundární přístupové klíče účtu Automation může být kdykoli znovu vygenerovat (na hello **Správa klíčů** okno) tooprevent budoucí uzlu registrace pomocí předchozí klíče.</span><span class="sxs-lookup"><span data-stu-id="27c3c-205">For added security, hello primary and secondary access keys of an Automation account can be regenerated at any time (on hello **Manage Keys** blade) tooprevent future node registrations using previous keys.</span></span>

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a><span data-ttu-id="27c3c-206">Řešení potíží s registrace virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="27c3c-206">Troubleshooting Azure virtual machine onboarding</span></span>

<span data-ttu-id="27c3c-207">Azure Automation DSC umožňuje snadno připojit virtuální počítače Azure Windows pro správu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="27c3c-207">Azure Automation DSC lets you easily onboard Azure Windows VMs for configuration management.</span></span> <span data-ttu-id="27c3c-208">Pod pokličkou hello hello rozšíření konfigurace požadovaného stavu aplikace Azure virtuálního počítače je použité tooregister hello virtuálního počítače s Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="27c3c-208">Under hello hood, hello Azure VM Desired State Configuration extension is used tooregister hello VM with Azure Automation DSC.</span></span> <span data-ttu-id="27c3c-209">Vzhledem k tomu, že hello rozšíření konfigurace požadovaného stavu aplikace Azure virtuální počítač se spustí asynchronně, může být důležité jejím průběhu sledování a řešení potíží s jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="27c3c-209">Since hello Azure VM Desired State Configuration extension runs asynchronously, tracking its progress and troubleshooting its execution may be important.</span></span>

> [!NOTE]
> <span data-ttu-id="27c3c-210">Jakékoli metody objektu registrace, které tooAzure virtuálního počítače Windows Azure Automation DSC, využívající rozšíření hello konfigurace požadovaného stavu aplikace Azure virtuálních počítačů může trvat až hodinu tooan hello uzlu tooshow až as registrované ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="27c3c-210">Any method of onboarding an Azure Windows VM tooAzure Automation DSC that uses hello Azure VM Desired State Configuration extension could take up tooan hour for hello node tooshow up as registered in Azure Automation.</span></span> <span data-ttu-id="27c3c-211">Toto je z důvodu toohello instalaci Windows Management Framework 5.0 na hello virtuálního počítače pomocí rozšíření hello virtuálních počítačů DSC Azure, které je požadované tooonboard hello virtuálních počítačů tooAzure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="27c3c-211">This is due toohello installation of Windows Management Framework 5.0 on hello VM by hello Azure VM DSC extension, which is required tooonboard hello VM tooAzure Automation DSC.</span></span>

<span data-ttu-id="27c3c-212">tootroubleshoot nebo zobrazení stavu hello hello rozšíření konfigurace požadovaného stavu aplikace Azure virtuálních počítačů v hello portálu Azure přejděte toohello se zařazený nemá virtuální počítač, pak klikněte na -> **všechna nastavení** -> **rozšíření**   ->  **DSC**.</span><span class="sxs-lookup"><span data-stu-id="27c3c-212">tootroubleshoot or view hello status of hello Azure VM Desired State Configuration extension, in hello Azure portal navigate toohello VM being onboarded, then click -> **All settings** -> **Extensions** -> **DSC**.</span></span> <span data-ttu-id="27c3c-213">Další podrobnosti, můžete kliknout na **zobrazit podrobné informace o stavu**.</span><span class="sxs-lookup"><span data-stu-id="27c3c-213">For more details, you can click **View detailed status**.</span></span>

[![](./media/automation-dsc-onboarding/DSC_Onboarding_5.png)](https://technet.microsoft.com/library/dn249912.aspx)

## <a name="certificate-expiration-and-reregistration"></a><span data-ttu-id="27c3c-214">Vypršení platnosti certifikátu a registrace</span><span class="sxs-lookup"><span data-stu-id="27c3c-214">Certificate expiration and reregistration</span></span>

<span data-ttu-id="27c3c-215">Po registraci počítače s jako uzel DSC v Azure Automation DSC, jsou z mnoha důvodů může být nutná tooreregister tento uzel v hello budoucí:</span><span class="sxs-lookup"><span data-stu-id="27c3c-215">After registering a machine as a DSC node in Azure Automation DSC, there are a number of reasons why you may need tooreregister that node in hello future:</span></span>

* <span data-ttu-id="27c3c-216">Po registraci, automatické každý uzel jedinečný certifikát pro ověřování, jejíž platnost vyprší po jednom roce.</span><span class="sxs-lookup"><span data-stu-id="27c3c-216">After registering, each node automatically negotiates a unique certificate for authentication that expires after one year.</span></span> <span data-ttu-id="27c3c-217">V současné době hello DSC Powershellu registrace protokol nelze obnovit automaticky certifikáty při jejich blížícím se koncem platnosti, takže je třeba tooreregister hello uzly po času v roce.</span><span class="sxs-lookup"><span data-stu-id="27c3c-217">Currently, hello PowerShell DSC registration protocol cannot automatically renew certificates when they are nearing expiration, so you need tooreregister hello nodes after a year's time.</span></span> <span data-ttu-id="27c3c-218">Před opětovná registrace, ujistěte se, že každý uzel běží Windows Management Framework 5.0 RTM.</span><span class="sxs-lookup"><span data-stu-id="27c3c-218">Before reregistering, ensure that each node is running Windows Management Framework 5.0 RTM.</span></span> <span data-ttu-id="27c3c-219">Pokud vyprší platnost certifikátu ověřování uzlu, a není znovu zaregistruje hello uzel, uzel hello bude nelze toocommunicate s Azure Automation a budou označeny 'Unresponsive.'</span><span class="sxs-lookup"><span data-stu-id="27c3c-219">If a node's authentication certificate expires, and hello node is not reregistered, hello node will be unable toocommunicate with Azure Automation and will be marked 'Unresponsive.'</span></span> <span data-ttu-id="27c3c-220">Opětovná provést 90 dní nebo méně z hello čas vypršení platnosti certifikátu, nebo kdykoli po hello čas vypršení platnosti certifikátu, bude výsledkem nový certifikát se generovat a používat.</span><span class="sxs-lookup"><span data-stu-id="27c3c-220">Reregistration performed 90 days or less from hello certificate expiration time, or at any point after hello certificate expiration time, will result in a new certificate being generated and used.</span></span>
* <span data-ttu-id="27c3c-221">toochange žádné [správce místní konfigurace DSC prostředí PowerShell hodnoty](https://msdn.microsoft.com/powershell/dsc/metaconfig4) které byly nastavené při počáteční registraci hello uzlu, jako je například ConfigurationMode.</span><span class="sxs-lookup"><span data-stu-id="27c3c-221">toochange any [PowerShell DSC Local Configuration Manager values](https://msdn.microsoft.com/powershell/dsc/metaconfig4) that were set during initial registration of hello node, such as ConfigurationMode.</span></span> <span data-ttu-id="27c3c-222">V současné době můžete tyto hodnoty agenta DSC změnit jenom prostřednictvím registrace.</span><span class="sxs-lookup"><span data-stu-id="27c3c-222">Currently, these DSC agent values can only be changed through reregistration.</span></span> <span data-ttu-id="27c3c-223">Jedinou výjimkou Hello je hello konfigurace uzlu přiřazen uzlu toohello – přímo lze změnit v Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="27c3c-223">hello one exception is hello Node Configuration assigned toohello node -- this can be changed in Azure Automation DSC directly.</span></span>

<span data-ttu-id="27c3c-224">Registrace lze provést v hello stejným způsobem jako registrovat hello uzlu na začátku pomocí libovolné metody registrace hello popsané v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="27c3c-224">Reregistration can be performed in hello same way you registered hello node initially, using any of hello onboarding methods described in this document.</span></span> <span data-ttu-id="27c3c-225">Před opětovná registrace ji nepotřebujete toounregister uzlu z Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="27c3c-225">You do not need toounregister a node from Azure Automation DSC before reregistering it.</span></span>

## <a name="related-articles"></a><span data-ttu-id="27c3c-226">Související články</span><span class="sxs-lookup"><span data-stu-id="27c3c-226">Related Articles</span></span>

* [<span data-ttu-id="27c3c-227">Přehled služby Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="27c3c-227">Azure Automation DSC overview</span></span>](automation-dsc-overview.md)
* [<span data-ttu-id="27c3c-228">Rutiny Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="27c3c-228">Azure Automation DSC cmdlets</span></span>](/powershell/module/azurerm.automation/#automation)
* [<span data-ttu-id="27c3c-229">Ceny služby Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="27c3c-229">Azure Automation DSC pricing</span></span>](https://azure.microsoft.com/pricing/details/automation/)
