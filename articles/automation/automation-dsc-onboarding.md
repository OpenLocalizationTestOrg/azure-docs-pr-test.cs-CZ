---
title: "Registrace počítačů pro správu Azure Automation DSC. | Microsoft Docs"
description: "Postup nastavení počítačů pro správu pomocí Azure Automation DSC."
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
ms.openlocfilehash: cc9b1ea19b4e17374d47e12f970cb333a8051559
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="onboarding-machines-for-management-by-azure-automation-dsc"></a><span data-ttu-id="4c0f3-103">Registrace počítačů pro správu Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-103">Onboarding machines for management by Azure Automation DSC</span></span>

## <a name="why-manage-machines-with-azure-automation-dsc"></a><span data-ttu-id="4c0f3-104">Proč spravovat počítače s Azure Automation DSC?</span><span class="sxs-lookup"><span data-stu-id="4c0f3-104">Why manage machines with Azure Automation DSC?</span></span>

<span data-ttu-id="4c0f3-105">Jako [konfigurace požadovaného stavu prostředí PowerShell](https://technet.microsoft.com/library/dn249912.aspx), konfigurace požadovaného stavu aplikace Azure Automation je jednoduché, ale výkonné, služba správy konfigurace pro uzly DSC (fyzické a virtuální počítače) ve všech cloudu nebo místně v datovém centru.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-105">Like [PowerShell Desired State Configuration](https://technet.microsoft.com/library/dn249912.aspx), Azure Automation Desired State Configuration is a simple, yet powerful, configuration management service for DSC nodes (physical and virtual machines) in any cloud or on-premises datacenter.</span></span> <span data-ttu-id="4c0f3-106">Umožňuje škálovatelnost napříč tisíce počítačů snadno a rychle z centrální a zabezpečené umístění.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-106">It enables scalability across thousands of machines quickly and easily from a central, secure location.</span></span> <span data-ttu-id="4c0f3-107">Můžete snadno připojit počítače, přiřaďte jim deklarativní konfigurace a zobrazovat sestavy zobrazující každý počítač je dodržování předpisů pro požadovaný stav, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-107">You can easily onboard machines, assign them declarative configurations, and view reports showing each machine's compliance to the desired state you specified.</span></span> <span data-ttu-id="4c0f3-108">Vrstva správy Azure Automation DSC je DSC, co je Azure Automation vrstvy správy skriptů prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-108">The Azure Automation DSC management layer is to DSC what the Azure Automation management layer is to PowerShell scripting.</span></span> <span data-ttu-id="4c0f3-109">Jinými slovy v stejným způsobem, který Azure Automation umožňuje spravovat skripty prostředí PowerShell, taky pomáhá při správě konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-109">In other words, in the same way that Azure Automation helps you manage PowerShell scripts, it also helps you manage DSC configurations.</span></span> <span data-ttu-id="4c0f3-110">Další informace o výhodách používání Azure Automation DSC, najdete v části [přehled Azure Automation DSC](automation-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4c0f3-110">To learn more about the benefits of using Azure Automation DSC, see [Azure Automation DSC overview](automation-dsc-overview.md).</span></span>

<span data-ttu-id="4c0f3-111">Azure Automation DSC umožňuje spravovat celou řadu počítače:</span><span class="sxs-lookup"><span data-stu-id="4c0f3-111">Azure Automation DSC can be used to manage a variety of machines:</span></span>

* <span data-ttu-id="4c0f3-112">Virtuální počítače Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="4c0f3-112">Azure virtual machines (classic)</span></span>
* <span data-ttu-id="4c0f3-113">Virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="4c0f3-113">Azure virtual machines</span></span>
* <span data-ttu-id="4c0f3-114">Virtuální počítače Amazon Web Services (AWS)</span><span class="sxs-lookup"><span data-stu-id="4c0f3-114">Amazon Web Services (AWS) virtual machines</span></span>
* <span data-ttu-id="4c0f3-115">Windows fyzický nebo virtuální počítače na místních počítačích nebo v cloudu, než Azure nebo AWS</span><span class="sxs-lookup"><span data-stu-id="4c0f3-115">Physical/virtual Windows machines on-premises, or in a cloud other than Azure/AWS</span></span>
* <span data-ttu-id="4c0f3-116">Linux fyzický nebo virtuální počítače na místních počítačích v Azure nebo v cloudu, než Azure</span><span class="sxs-lookup"><span data-stu-id="4c0f3-116">Physical/virtual Linux machines on-premises, in Azure, or in a cloud other than Azure</span></span>

<span data-ttu-id="4c0f3-117">Kromě toho pokud si nejste připravení spravovat konfiguraci počítače z cloudu, Azure Automation DSC lze také jako koncový bod pouze sestavy.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-117">In addition, if you are not ready to manage machine configuration from the cloud, Azure Automation DSC can also be used as a report-only endpoint.</span></span> <span data-ttu-id="4c0f3-118">Můžete nastavit (push) požadované konfigurace pomocí DSC na místě a zobrazte bohaté reporting podrobnosti o dodržování předpisů uzlu s požadovaný stav ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-118">This allows you to set (push) desired configuration through DSC on-premises and view rich reporting details on node compliance with the desired state in Azure Automation.</span></span>

<span data-ttu-id="4c0f3-119">Následující oddíly popisují, jak můžete připojit každý typ počítače do Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-119">The following sections outline how you can onboard each type of machine to Azure Automation DSC.</span></span>

## <a name="azure-virtual-machines-classic"></a><span data-ttu-id="4c0f3-120">Virtuální počítače Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="4c0f3-120">Azure virtual machines (classic)</span></span>

<span data-ttu-id="4c0f3-121">S Azure Automation DSC můžete snadno připojit virtuální počítače Azure (klasický) za účelem správy konfigurace buď pomocí portálu Azure, nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-121">With Azure Automation DSC, you can easily onboard Azure virtual machines (classic) for configuration management using either the Azure portal, or PowerShell.</span></span> <span data-ttu-id="4c0f3-122">Pod pokličkou a bez nutnosti vzdáleného do virtuálního počítače správce zaregistruje rozšíření konfigurace požadovaného stavu aplikace Azure virtuálních počítačů virtuálního počítače se Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-122">Under the hood, and without an administrator having to remote into the VM, the Azure VM Desired State Configuration extension registers the VM with Azure Automation DSC.</span></span> <span data-ttu-id="4c0f3-123">Vzhledem k tomu, že rozšíření konfigurace požadovaného stavu aplikace Azure virtuální počítač se spustí asynchronně, jsou kroky k jeho průběh sledovat a řešit potíže se součástí [ **řešení potíží s Azure virtuálního počítače registrace** ](#troubleshooting-azure-virtual-machine-onboarding) části.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-123">Since the Azure VM Desired State Configuration extension runs asynchronously, steps to track its progress or troubleshoot it are provided in the [**Troubleshooting Azure virtual machine onboarding**](#troubleshooting-azure-virtual-machine-onboarding) section below.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="4c0f3-124">portál Azure</span><span class="sxs-lookup"><span data-stu-id="4c0f3-124">Azure portal</span></span>

<span data-ttu-id="4c0f3-125">V [portál Azure](http://portal.azure.com/), klikněte na tlačítko **Procházet** -> **virtuálních počítačů (klasické)**.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-125">In the [Azure portal](http://portal.azure.com/), click **Browse** -> **Virtual machines (classic)**.</span></span> <span data-ttu-id="4c0f3-126">Vyberte virtuální počítač Windows, které chcete zařadit do provozu.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-126">Select the Windows VM you want to onboard.</span></span> <span data-ttu-id="4c0f3-127">V okně virtuálního počítače řídicí panel, klikněte na tlačítko **všechna nastavení** -> **rozšíření** -> **přidat** -> **Azure Automation DSC** -> **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-127">On the virtual machine's dashboard blade, click **All settings** -> **Extensions** -> **Add** -> **Azure Automation DSC** -> **Create**.</span></span> <span data-ttu-id="4c0f3-128">Zadejte [správce místní konfigurace DSC prostředí PowerShell hodnoty](https://msdn.microsoft.com/powershell/dsc/metaconfig4) požadované pro váš případ použití, účet Automation registrační klíč a adresa URL registrace a volitelně konfigurace uzlu přiřadit k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-128">Enter the [PowerShell DSC Local Configuration Manager values](https://msdn.microsoft.com/powershell/dsc/metaconfig4) required for your use case, your Automation account's registration key and registration URL, and optionally a node configuration to assign to the VM.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_1.png)

<span data-ttu-id="4c0f3-129">K vyhledání adresu URL pro registraci a klíče pro účet služby Automation zařadit do počítače, najdete v článku [ **zabezpečení registrace** ](#secure-registration) části níže.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-129">To find the registration URL and key for the Automation account to onboard the machine to, see the [**Secure registration**](#secure-registration) section below.</span></span>

### <a name="powershell"></a><span data-ttu-id="4c0f3-130">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4c0f3-130">PowerShell</span></span>

```powershell
# log in to both Azure Service Management and Azure Resource Manager
Add-AzureAccount
Add-AzureRmAccount

# fill in correct values for your VM/Automation account here
$VMName = ""
$ServiceName = ""
$AutomationAccountName = ""
$AutomationAccountResourceGroup = ""

# fill in the name of a Node Configuration in Azure Automation DSC, for this VM to conform to
$NodeConfigName = ""

# get Azure Automation DSC registration info
$Account = Get-AzureRmAutomationAccount -ResourceGroupName $AutomationAccountResourceGroup -Name $AutomationAccountName
$RegistrationInfo = $Account | Get-AzureRmAutomationRegistrationInfo

# use the DSC extension to onboard the VM for management with Azure Automation DSC
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

## <a name="azure-virtual-machines"></a><span data-ttu-id="4c0f3-131">Virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="4c0f3-131">Azure virtual machines</span></span>

<span data-ttu-id="4c0f3-132">Azure Automation DSC umožňuje snadno připojit virtuální počítače Azure za účelem správy konfigurace, pomocí portálu Azure, šablon Azure Resource Manageru nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-132">Azure Automation DSC lets you easily onboard Azure virtual machines for configuration management, using either the Azure portal, Azure Resource Manager templates, or PowerShell.</span></span> <span data-ttu-id="4c0f3-133">Pod pokličkou a bez nutnosti vzdáleného do virtuálního počítače správce zaregistruje rozšíření konfigurace požadovaného stavu aplikace Azure virtuálních počítačů virtuálního počítače se Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-133">Under the hood, and without an administrator having to remote into the VM, the Azure VM Desired State Configuration extension registers the VM with Azure Automation DSC.</span></span> <span data-ttu-id="4c0f3-134">Vzhledem k tomu, že rozšíření konfigurace požadovaného stavu aplikace Azure virtuální počítač se spustí asynchronně, jsou kroky k jeho průběh sledovat a řešit potíže se součástí [ **řešení potíží s Azure virtuálního počítače registrace** ](#troubleshooting-azure-virtual-machine-onboarding) části.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-134">Since the Azure VM Desired State Configuration extension runs asynchronously, steps to track its progress or troubleshoot it are provided in the [**Troubleshooting Azure virtual machine onboarding**](#troubleshooting-azure-virtual-machine-onboarding) section below.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="4c0f3-135">portál Azure</span><span class="sxs-lookup"><span data-stu-id="4c0f3-135">Azure portal</span></span>

<span data-ttu-id="4c0f3-136">V [portál Azure](https://portal.azure.com/), přejděte na účet Azure Automation, ve které chcete zařadit virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-136">In the [Azure portal](https://portal.azure.com/), navigate to the Azure Automation account where you want to onboard virtual machines.</span></span> <span data-ttu-id="4c0f3-137">Na řídicím panelu účet Automation, klikněte na tlačítko **uzly DSC** -> **přidat virtuální počítač Azure**.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-137">On the Automation account dashboard, click **DSC Nodes** -> **Add Azure VM**.</span></span>

<span data-ttu-id="4c0f3-138">V části **vybrat virtuální počítače chcete zařadit**, vyberte jednu nebo více Azure virtuálních počítačů do zaváděním.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-138">Under **Select virtual machines to onboard**, select one or more Azure virtual machines to onboard.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_2.png)

<span data-ttu-id="4c0f3-139">V části **konfigurace registrační data**, zadejte [správce místní konfigurace DSC prostředí PowerShell hodnoty](https://msdn.microsoft.com/powershell/dsc/metaconfig4) vyžadované pro váš případ použití a volitelně konfigurace uzlu přiřadit k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-139">Under **Configure registration data**, enter the [PowerShell DSC Local Configuration Manager values](https://msdn.microsoft.com/powershell/dsc/metaconfig4) required for your use case, and optionally a node configuration to assign to the VM.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_3.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="4c0f3-140">Šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="4c0f3-140">Azure Resource Manager templates</span></span>

<span data-ttu-id="4c0f3-141">Virtuální počítače Azure můžete nasadit a zařazený nemá na Azure Automation DSC pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-141">Azure virtual machines can be deployed and onboarded to Azure Automation DSC via Azure Resource Manager templates.</span></span> <span data-ttu-id="4c0f3-142">V tématu [konfigurace virtuálního počítače prostřednictvím rozšíření DSC a Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) pro šablonu příklad této onboards existující virtuální počítač do Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-142">See [Configure a VM via DSC extension and Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) for an example template that onboards an existing VM to Azure Automation DSC.</span></span> <span data-ttu-id="4c0f3-143">Najít registrační klíč a adresa URL registrace prováděné jako vstup v této šabloně najdete v článku [ **zabezpečení registrace** ](#secure-registration) části níže.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-143">To find the registration key and registration URL taken as input in this template, see the [**Secure registration**](#secure-registration) section below.</span></span>

### <a name="powershell"></a><span data-ttu-id="4c0f3-144">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4c0f3-144">PowerShell</span></span>

<span data-ttu-id="4c0f3-145">[Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) rutiny je možné připojit virtuální počítače na portálu Azure pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-145">The [Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) cmdlet can be used to onboard virtual machines in the Azure portal via PowerShell.</span></span>

## <a name="amazon-web-services-aws-virtual-machines"></a><span data-ttu-id="4c0f3-146">Virtuální počítače Amazon Web Services (AWS)</span><span class="sxs-lookup"><span data-stu-id="4c0f3-146">Amazon Web Services (AWS) virtual machines</span></span>

<span data-ttu-id="4c0f3-147">Můžete snadno připojit Amazon Web Services virtuálních počítačů pro správu konfigurace pomocí sady nástrojů pro AWS DSC Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-147">You can easily onboard Amazon Web Services virtual machines for configuration management by Azure Automation DSC using the AWS DSC Toolkit.</span></span> <span data-ttu-id="4c0f3-148">Další informace o sadě nástrojů [zde](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).</span><span class="sxs-lookup"><span data-stu-id="4c0f3-148">You can learn more about the toolkit [here](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).</span></span>

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws"></a><span data-ttu-id="4c0f3-149">Windows fyzický nebo virtuální počítače na místních počítačích nebo v cloudu, než Azure nebo AWS</span><span class="sxs-lookup"><span data-stu-id="4c0f3-149">Physical/virtual Windows machines on-premises, or in a cloud other than Azure/AWS</span></span>

<span data-ttu-id="4c0f3-150">Místní počítače s Windows a počítače s Windows v cloudech mimo Azure (například Amazon Web Services) možné zařadit také do Azure Automation DSC, tak dlouho, dokud mají přístup k Internetu pomocí několika jednoduchých kroků:</span><span class="sxs-lookup"><span data-stu-id="4c0f3-150">On-premises Windows machines and Windows machines in non-Azure clouds (such as Amazon Web Services) can also be onboarded to Azure Automation DSC, as long as they have outbound access to the internet, via a few simple steps:</span></span>

1. <span data-ttu-id="4c0f3-151">Zajistěte, aby nejnovější verzi [WMF 5](http://aka.ms/wmf5latest) nainstalován v počítačích, které chcete, aby se budou registrovat do Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-151">Make sure the latest version of [WMF 5](http://aka.ms/wmf5latest) is installed on the machines you want to onboard to Azure Automation DSC.</span></span>
2. <span data-ttu-id="4c0f3-152">Postupujte podle pokynů v části [ **generování DSC metaconfigurations** ](#generating-dsc-metaconfigurations) dole vygenerujte složku obsahující potřebné metaconfigurations DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-152">Follow the directions in section [**Generating DSC metaconfigurations**](#generating-dsc-metaconfigurations) below to generate a folder containing the needed DSC metaconfigurations.</span></span>
3. <span data-ttu-id="4c0f3-153">Vzdáleně použijte metakonfiguraci PowerShell DSC na počítače, které chcete zařadit do provozu.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-153">Remotely apply the PowerShell DSC metaconfiguration to the machines you want to onboard.</span></span> <span data-ttu-id="4c0f3-154">**Počítač je tento příkaz spuštěn z musí mít nejnovější verzi [WMF 5](http://aka.ms/wmf5latest) nainstalován**:</span><span class="sxs-lookup"><span data-stu-id="4c0f3-154">**The machine this command is run from must have the latest version of [WMF 5](http://aka.ms/wmf5latest) installed**:</span></span>

    ```powershell
    Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
    ```

4. <span data-ttu-id="4c0f3-155">Pokud nemůžete použít metaconfigurations DSC Powershellu vzdáleně, zkopírujte složku metaconfigurations z kroku 2 na každý počítač zařadit do provozu.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-155">If you cannot apply the PowerShell DSC metaconfigurations remotely, copy the metaconfigurations folder from step 2 onto each machine to onboard.</span></span> <span data-ttu-id="4c0f3-156">Potom zavolejte **Set-DscLocalConfigurationManager** místně na každém počítači zařadit do provozu.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-156">Then call **Set-DscLocalConfigurationManager** locally on each machine to onboard.</span></span>
5. <span data-ttu-id="4c0f3-157">Pomocí portálu Azure nebo rutin zkontrolujte, že počítače zařadit do provozu nyní zobrazují jako uzly DSC zaregistrovaný v účtu Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-157">Using the Azure portal or cmdlets, check that the machines to onboard now show up as DSC nodes registered in your Azure Automation account.</span></span>

## <a name="physicalvirtual-linux-machines-on-premises-in-azure-or-in-a-cloud-other-than-azure"></a><span data-ttu-id="4c0f3-158">Linux fyzický nebo virtuální počítače na místních počítačích v Azure nebo v cloudu, než Azure</span><span class="sxs-lookup"><span data-stu-id="4c0f3-158">Physical/virtual Linux machines on-premises, in Azure, or in a cloud other than Azure</span></span>

<span data-ttu-id="4c0f3-159">Místní počítače se systémem Linux, počítače s Linuxem v Azure a počítače se systémem Linux v cloudech mimo Azure možné zařadit také do Azure Automation DSC, tak dlouho, dokud mají přístup k Internetu pomocí několika jednoduchých kroků:</span><span class="sxs-lookup"><span data-stu-id="4c0f3-159">On-premises Linux machines, Linux machines in Azure, and Linux machines in non-Azure clouds can also be onboarded to Azure Automation DSC, as long as they have outbound access to the internet, via a few simple steps:</span></span>

1. <span data-ttu-id="4c0f3-160">Zajistěte, aby nejnovější verzi [prostředí PowerShell konfigurace požadovaného stavu pro Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) nainstalován v počítačích, které chcete, aby se budou registrovat do Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-160">Make sure the latest version of [PowerShell Desired State Configuration for Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) is installed on the machines you want to onboard to Azure Automation DSC.</span></span>
2. <span data-ttu-id="4c0f3-161">Pokud [správce místní konfigurace DSC prostředí PowerShell výchozí](https://msdn.microsoft.com/powershell/dsc/metaconfig4) odpovídající váš případ použití, a chcete připojit počítače tak, že se **obě** načítat z a nahlásit Azure Automation DSC:</span><span class="sxs-lookup"><span data-stu-id="4c0f3-161">If the [PowerShell DSC Local Configuration Manager defaults](https://msdn.microsoft.com/powershell/dsc/metaconfig4) match your use case, and you want to onboard machines such that they **both** pull from and report to Azure Automation DSC:</span></span>

   + <span data-ttu-id="4c0f3-162">Na každém počítači Linux mohl připojit k Azure Automation DSC pomocí Register.py zařadit pomocí výchozího nastavení správce místní konfigurace DSC prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="4c0f3-162">On each Linux machine to onboard to Azure Automation DSC, use Register.py to onboard using the PowerShell DSC Local Configuration Manager defaults:</span></span>

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   + <span data-ttu-id="4c0f3-163">Registrační klíč a adresa URL registrace u vašeho účtu Automation, najdete v tématu [ **zabezpečení registrace** ](#secure-registration) části níže.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-163">To find the registration key and registration URL for your Automation account, see the [**Secure registration**](#secure-registration) section below.</span></span>

     <span data-ttu-id="4c0f3-164">Pokud výchozí nastavení správce místní konfigurace DSC Powershellu **provést** **není** shodu váš případ použití nebo je chcete zařadit do počítače, tak, aby pouze ohlásí Azure Automation DSC, ale proveďte není konfigurace vyžádané nebo moduly Powershellu z něj, postupujte podle kroků 3 až 6.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-164">If the PowerShell DSC Local Configuration Manager defaults **do** **not** match your use case, or you want to onboard machines such that they only report to Azure Automation DSC, but do not pull configuration or PowerShell modules from it,  follow steps 3 - 6.</span></span> <span data-ttu-id="4c0f3-165">V opačném přímo přejděte na krok 6.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-165">Otherwise, proceed directly to step 6.</span></span>

3. <span data-ttu-id="4c0f3-166">Postupujte podle pokynů [ **metaconfigurations generování DSC** ](#generating-dsc-metaconfigurations) části dole vygenerujte složku obsahující potřebné metaconfigurations DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-166">Follow the directions in the [**Generating DSC metaconfigurations**](#generating-dsc-metaconfigurations) section below to generate a folder containing the needed DSC metaconfigurations.</span></span>
4. <span data-ttu-id="4c0f3-167">Vzdáleně použijte metakonfiguraci PowerShell DSC na počítače, které chcete zařadit do provozu:</span><span class="sxs-lookup"><span data-stu-id="4c0f3-167">Remotely apply the PowerShell DSC metaconfiguration to the machines you want to onboard:</span></span>

    ```powershell
    $SecurePass = ConvertTo-SecureString -String "<root password>" -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential "root", $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard

    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

<span data-ttu-id="4c0f3-168">Počítač je tento příkaz spuštěn z musí mít nejnovější verzi [WMF 5](http://aka.ms/wmf5latest) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-168">The machine this command is run from must have the latest version of [WMF 5](http://aka.ms/wmf5latest) installed.</span></span>

1. <span data-ttu-id="4c0f3-169">Pokud nemůžete použít metaconfigurations DSC Powershellu vzdáleně, pro každý počítač Linux zařadit, zkopírujte metakonfiguraci odpovídající k tomuto počítači ze složky v kroku 5 do počítače Linux.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-169">If you cannot apply the PowerShell DSC metaconfigurations remotely, for each Linux machine to onboard, copy the metaconfiguration corresponding to that machine from the folder in step 5 onto the Linux machine.</span></span> <span data-ttu-id="4c0f3-170">Potom zavolejte `SetDscLocalConfigurationManager.py` místně na každém počítači Linux chcete, aby se budou registrovat do Azure Automation DSC:</span><span class="sxs-lookup"><span data-stu-id="4c0f3-170">Then call `SetDscLocalConfigurationManager.py` locally on each Linux machine you want to onboard to Azure Automation DSC:</span></span>

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

2. <span data-ttu-id="4c0f3-171">Pomocí portálu Azure nebo rutin zkontrolujte, že počítače zařadit do provozu nyní zobrazují jako uzly DSC zaregistrovaný v účtu Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-171">Using the Azure portal or cmdlets, check that the machines to onboard now show up as DSC nodes registered in your Azure Automation account.</span></span>

## <a name="generating-dsc-metaconfigurations"></a><span data-ttu-id="4c0f3-172">Generování DSC metaconfigurations</span><span class="sxs-lookup"><span data-stu-id="4c0f3-172">Generating DSC metaconfigurations</span></span>

<span data-ttu-id="4c0f3-173">Obecně zařadit do provozu žádné počítače do Azure Automation DSC, [metakonfiguraci DSC](https://msdn.microsoft.com/en-us/powershell/dsc/metaconfig) může být generována, při použití informuje DSC agenta na počítači, aby načítat z nebo nahlásit Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-173">To generically onboard any machine to Azure Automation DSC, a [DSC metaconfiguration](https://msdn.microsoft.com/en-us/powershell/dsc/metaconfig) can be generated that, when applied, tells the DSC agent on the machine to pull from and/or report to Azure Automation DSC.</span></span> <span data-ttu-id="4c0f3-174">Metaconfigurations DSC pro Azure Automation DSC může být generována pomocí konfigurace DSC prostředí PowerShell nebo rutin prostředí PowerShell Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-174">DSC metaconfigurations for Azure Automation DSC can be generated using either a PowerShell DSC configuration, or the Azure Automation PowerShell cmdlets.</span></span>

> [!NOTE]
> <span data-ttu-id="4c0f3-175">DSC metaconfigurations obsahovat všechna tajemství potřeba zařadit do počítače s Automation účet pro správu.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-175">DSC metaconfigurations contain the secrets needed to onboard a machine to an Automation account for management.</span></span> <span data-ttu-id="4c0f3-176">Ujistěte se, zda jste správně chránit všechny metaconfigurations DSC, které vytvoříte, nebo je odstranit po použití.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-176">Make sure to properly protect any DSC metaconfigurations you create, or delete them after use.</span></span>

### <a name="using-a-dsc-configuration"></a><span data-ttu-id="4c0f3-177">Používání konfigurace DSC</span><span class="sxs-lookup"><span data-stu-id="4c0f3-177">Using a DSC Configuration</span></span>

1. <span data-ttu-id="4c0f3-178">Otevřete prostředí PowerShell ISE jako správce v počítači ve vašem místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-178">Open the PowerShell ISE as an administrator in a machine in your local environment.</span></span> <span data-ttu-id="4c0f3-179">Počítač musí mít nejnovější verzi [WMF 5](http://aka.ms/wmf5latest) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-179">The machine must have the latest version of [WMF 5](http://aka.ms/wmf5latest) installed.</span></span>
2. <span data-ttu-id="4c0f3-180">Zkopírujte následující skript místně.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-180">Copy the following script locally.</span></span> <span data-ttu-id="4c0f3-181">Tento skript obsahuje konfiguraci DSC prostředí PowerShell pro vytváření metaconfigurations a příkaz k vytvoření metakonfiguraci ji.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-181">This script contains a PowerShell DSC configuration for creating metaconfigurations, and a command to kick off the metaconfiguration creation.</span></span>

    ```powershell
    # The DSC configuration that will generate metaconfigurations
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

    # Create the metaconfigurations
    # TODO: edit the below as needed for your use case
    $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM to onboard>', '<some other VM to onboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set to $True to have machines only report to AA DSC but not pull from it
    }

    # Use PowerShell splatting to pass parameters to the DSC configuration being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    DscMetaConfigs @Params
    ```

3. <span data-ttu-id="4c0f3-182">Zadejte registrační klíč a adresu URL pro svůj účet Automation, jakož i názvy počítačů zařadit do provozu.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-182">Fill in the registration key and URL for your Automation account, as well as the names of the machines to onboard.</span></span> <span data-ttu-id="4c0f3-183">Všechny ostatní parametry jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-183">All other parameters are optional.</span></span> <span data-ttu-id="4c0f3-184">Registrační klíč a adresa URL registrace u vašeho účtu Automation, najdete v tématu [ **zabezpečení registrace** ](#secure-registration) části níže.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-184">To find the registration key and registration URL for your Automation account, see the [**Secure registration**](#secure-registration) section below.</span></span>
4. <span data-ttu-id="4c0f3-185">Pokud chcete počítače tak, aby odesílaly informace o stavu DSC Azure Automation DSC, ale není konfigurace vyžádané nebo moduly Powershellu, nastavte **ReportOnly** parametr na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-185">If you want the machines to report DSC status information to Azure Automation DSC, but not pull configuration or PowerShell modules, set the **ReportOnly** parameter to true.</span></span>
5. <span data-ttu-id="4c0f3-186">Spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-186">Run the script.</span></span> <span data-ttu-id="4c0f3-187">Teď byste měli mít složku s názvem **DscMetaConfigs** v pracovním adresáři, který obsahuje metaconfigurations PowerShell DSC pro počítače se budou registrovat (jako správce):</span><span class="sxs-lookup"><span data-stu-id="4c0f3-187">You should now have a folder called **DscMetaConfigs** in your working directory, containing the PowerShell DSC metaconfigurations for the machines to onboard (as an administrator):</span></span>

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-the-azure-automation-cmdlets"></a><span data-ttu-id="4c0f3-188">Pomocí rutiny Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-188">Using the Azure Automation cmdlets</span></span>

<span data-ttu-id="4c0f3-189">Pokud výchozí nastavení správce místní konfigurace DSC prostředí PowerShell odpovídá váš případ použití a chcete zařadit počítače tak, aby se jak načítat z a nahlásit Azure Automation DSC, rutiny Azure Automation poskytují zjednodušenou metodu generování metaconfigurations DSC potřebné:</span><span class="sxs-lookup"><span data-stu-id="4c0f3-189">If the PowerShell DSC Local Configuration Manager defaults match your use case, and you want to onboard machines such that they both pull from and report to Azure Automation DSC, the Azure Automation cmdlets provide a simplified method of generating the DSC metaconfigurations needed:</span></span>

1. <span data-ttu-id="4c0f3-190">Otevřete konzolu PowerShell nebo ISE prostředí PowerShell jako správce v počítači ve vašem místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-190">Open the PowerShell console or PowerShell ISE as an administrator in a machine in your local environment.</span></span>
2. <span data-ttu-id="4c0f3-191">Připojte se k Azure Resource Manager pomocí **Add-AzureRmAccount**</span><span class="sxs-lookup"><span data-stu-id="4c0f3-191">Connect to Azure Resource Manager using **Add-AzureRmAccount**</span></span>
3. <span data-ttu-id="4c0f3-192">Stáhněte metaconfigurations PowerShell DSC pro počítače, které chcete zařadit do provozu z účtu Automation, ke které chcete zařadit uzly:</span><span class="sxs-lookup"><span data-stu-id="4c0f3-192">Download the PowerShell DSC metaconfigurations for the machines you want to onboard from the Automation account to which you want to onboard nodes:</span></span>

    ```powershell
    # Define the parameters for Get-AzureRmAutomationDscOnboardingMetaconfig using PowerShell Splatting
    $Params = @{

        ResourceGroupName = 'ContosoResources'; # The name of the ARM Resource Group that contains your Azure Automation Account
        AutomationAccountName = 'ContosoAutomation'; # The name of the Azure Automation Account where you want a node on-boarded to
        ComputerName = @('web01', 'web02', 'sql01'); # The names of the computers that the meta configuration will be generated for
        OutputFolder = "$env:UserProfile\Desktop\";
    }
    # Use PowerShell splatting to pass parameters to the Azure Automation cmdlet being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    Get-AzureRmAutomationDscOnboardingMetaconfig @Params
    ```
    
4. <span data-ttu-id="4c0f3-193">Teď byste měli mít složku s názvem ***DscMetaConfigs***, obsahující metaconfigurations PowerShell DSC pro počítače se budou registrovat (jako správce):</span><span class="sxs-lookup"><span data-stu-id="4c0f3-193">You should now have a folder called ***DscMetaConfigs***, containing the PowerShell DSC metaconfigurations for the machines to onboard (as an administrator):</span></span>
    
    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a><span data-ttu-id="4c0f3-194">Zabezpečené registrace</span><span class="sxs-lookup"><span data-stu-id="4c0f3-194">Secure registration</span></span>

<span data-ttu-id="4c0f3-195">Počítače můžou bezpečně připojit k účtu Azure Automation přes protokol registrace WMF 5 DSC, který umožňuje uzlu DSC k ověření serveru prostředí PowerShell DSC V2 Pull nebo vytváření sestav (včetně Azure Automation DSC).</span><span class="sxs-lookup"><span data-stu-id="4c0f3-195">Machines can securely onboard to an Azure Automation account through the WMF 5 DSC registration protocol, which allows a DSC node to authenticate to a PowerShell DSC V2 Pull or Reporting server (including Azure Automation DSC).</span></span> <span data-ttu-id="4c0f3-196">Uzel zaregistruje na server v **URL pro registraci**, ověřování pomocí **registrační klíč**.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-196">The node registers to the server at a **Registration URL**, authenticating using a **Registration key**.</span></span> <span data-ttu-id="4c0f3-197">Během registrace vyjednat jedinečný certifikát pro tento uzel používané při ověřování k serveru po registraci uzlu DSC a server pro vyžádání obsahu nebo sestavy DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-197">During registration, the DSC node and DSC Pull/Reporting server negotiate a unique certificate for this node to use for authentication to the server post-registration.</span></span> <span data-ttu-id="4c0f3-198">Tento proces zabraňuje zařazený nemá uzly z jednoho jiné, jako je například pokud uzel dojde k ohrožení bezpečnosti zosobnění a chovají závadně.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-198">This process prevents onboarded nodes from impersonating one another, such as if a node is compromised and behaving maliciously.</span></span> <span data-ttu-id="4c0f3-199">Po registraci registrační klíč se nepoužije pro ověřování znovu a se odstraní z uzlu.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-199">After registration, the Registration key is not used for authentication again, and is deleted from the node.</span></span>

<span data-ttu-id="4c0f3-200">Můžete získat požadované informace o protokolu registrace DSC z **Správa klíčů** okno na portálu Azure preview.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-200">You can get the information required for the DSC registration protocol from the **Manage Keys** blade in the Azure preview portal.</span></span> <span data-ttu-id="4c0f3-201">Otevřete toto okno kliknutím na ikonu klíče na **Essentials** panel pro účet Automation.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-201">Open this blade by clicking the key icon on the **Essentials** panel for the Automation account.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

* <span data-ttu-id="4c0f3-202">Adresa URL registrace je pole adresy URL v okně správy klíčů.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-202">Registration URL is the URL field in the Manage Keys blade.</span></span>
* <span data-ttu-id="4c0f3-203">Registrační klíč je primární přístupový klíč nebo sekundární přístupový klíč v okně správy klíčů.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-203">Registration key is the Primary Access Key or Secondary Access Key in the Manage Keys blade.</span></span> <span data-ttu-id="4c0f3-204">Můžete použít buď klíč.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-204">Either key can be used.</span></span>

<span data-ttu-id="4c0f3-205">Pro zvýšení zabezpečení, primární a sekundární přístupové klíče účtu Automation může být kdykoli znovu vygenerovat (na **Správa klíčů** okno) aby budoucí uzlu registrace pomocí předchozí klíče.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-205">For added security, the primary and secondary access keys of an Automation account can be regenerated at any time (on the **Manage Keys** blade) to prevent future node registrations using previous keys.</span></span>

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a><span data-ttu-id="4c0f3-206">Řešení potíží s registrace virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="4c0f3-206">Troubleshooting Azure virtual machine onboarding</span></span>

<span data-ttu-id="4c0f3-207">Azure Automation DSC umožňuje snadno připojit virtuální počítače Azure Windows pro správu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-207">Azure Automation DSC lets you easily onboard Azure Windows VMs for configuration management.</span></span> <span data-ttu-id="4c0f3-208">Pod pokličkou rozšíření konfigurace požadovaného stavu aplikace Azure virtuálních počítačů slouží k registraci virtuálního počítače s Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-208">Under the hood, the Azure VM Desired State Configuration extension is used to register the VM with Azure Automation DSC.</span></span> <span data-ttu-id="4c0f3-209">Vzhledem k tomu, že rozšíření konfigurace požadovaného stavu aplikace Azure virtuální počítač se spustí asynchronně, může být důležité jejím průběhu sledování a řešení potíží s jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-209">Since the Azure VM Desired State Configuration extension runs asynchronously, tracking its progress and troubleshooting its execution may be important.</span></span>

> [!NOTE]
> <span data-ttu-id="4c0f3-210">Jakékoli metody objektu registrace virtuálního počítače Windows Azure do Azure Automation DSC, který používá rozšíření konfigurace požadovaného stavu aplikace Azure virtuálních počítačů může trvat až jednu hodinu pro uzel k zobrazení až as registrované ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-210">Any method of onboarding an Azure Windows VM to Azure Automation DSC that uses the Azure VM Desired State Configuration extension could take up to an hour for the node to show up as registered in Azure Automation.</span></span> <span data-ttu-id="4c0f3-211">Toto je instalace Windows Management Framework 5.0 na virtuálním počítači rozšíření virtuálních počítačů DSC Azure, který je vyžadován zařadit do virtuálního počítače Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-211">This is due to the installation of Windows Management Framework 5.0 on the VM by the Azure VM DSC extension, which is required to onboard the VM to Azure Automation DSC.</span></span>

<span data-ttu-id="4c0f3-212">Pro vyřešení problémů nebo zobrazit stav rozšíření konfigurace požadovaného stavu aplikace Azure virtuálních počítačů, na portálu Azure přejděte do virtuálního počítače se zařazený nemá a pak klikněte na -> **všechna nastavení** -> **rozšíření** -> **DSC**.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-212">To troubleshoot or view the status of the Azure VM Desired State Configuration extension, in the Azure portal navigate to the VM being onboarded, then click -> **All settings** -> **Extensions** -> **DSC**.</span></span> <span data-ttu-id="4c0f3-213">Další podrobnosti, můžete kliknout na **zobrazit podrobné informace o stavu**.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-213">For more details, you can click **View detailed status**.</span></span>

[![](./media/automation-dsc-onboarding/DSC_Onboarding_5.png)](https://technet.microsoft.com/library/dn249912.aspx)

## <a name="certificate-expiration-and-reregistration"></a><span data-ttu-id="4c0f3-214">Vypršení platnosti certifikátu a registrace</span><span class="sxs-lookup"><span data-stu-id="4c0f3-214">Certificate expiration and reregistration</span></span>

<span data-ttu-id="4c0f3-215">Po registraci počítače s jako uzel DSC v Azure Automation DSC, jsou z mnoha důvodů, proč se budete muset znovu registrovat tento uzel v budoucnosti:</span><span class="sxs-lookup"><span data-stu-id="4c0f3-215">After registering a machine as a DSC node in Azure Automation DSC, there are a number of reasons why you may need to reregister that node in the future:</span></span>

* <span data-ttu-id="4c0f3-216">Po registraci, automatické každý uzel jedinečný certifikát pro ověřování, jejíž platnost vyprší po jednom roce.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-216">After registering, each node automatically negotiates a unique certificate for authentication that expires after one year.</span></span> <span data-ttu-id="4c0f3-217">V současné době protokol registrace DSC Powershellu nelze obnovit automaticky certifikáty při jejich blížícím se koncem platnosti, takže budete muset znovu registrovat uzly po času v roce.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-217">Currently, the PowerShell DSC registration protocol cannot automatically renew certificates when they are nearing expiration, so you need to reregister the nodes after a year's time.</span></span> <span data-ttu-id="4c0f3-218">Před opětovná registrace, ujistěte se, že každý uzel běží Windows Management Framework 5.0 RTM.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-218">Before reregistering, ensure that each node is running Windows Management Framework 5.0 RTM.</span></span> <span data-ttu-id="4c0f3-219">Pokud vyprší platnost certifikátu ověřování uzlu, a není znovu zaregistruje uzel, uzel nebude moci komunikovat s Azure Automation a budou označeny 'Unresponsive.'</span><span class="sxs-lookup"><span data-stu-id="4c0f3-219">If a node's authentication certificate expires, and the node is not reregistered, the node will be unable to communicate with Azure Automation and will be marked 'Unresponsive.'</span></span> <span data-ttu-id="4c0f3-220">Opětovná provést 90 dní nebo méně z čas vypršení platnosti certifikátu, nebo kdykoli po času vypršení platnosti certifikátu, bude výsledkem nový certifikát se generovat a používat.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-220">Reregistration performed 90 days or less from the certificate expiration time, or at any point after the certificate expiration time, will result in a new certificate being generated and used.</span></span>
* <span data-ttu-id="4c0f3-221">Chcete-li změnit některé [správce místní konfigurace DSC prostředí PowerShell hodnoty](https://msdn.microsoft.com/powershell/dsc/metaconfig4) které byly nastavené při počáteční registraci uzlu, jako je například ConfigurationMode.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-221">To change any [PowerShell DSC Local Configuration Manager values](https://msdn.microsoft.com/powershell/dsc/metaconfig4) that were set during initial registration of the node, such as ConfigurationMode.</span></span> <span data-ttu-id="4c0f3-222">V současné době můžete tyto hodnoty agenta DSC změnit jenom prostřednictvím registrace.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-222">Currently, these DSC agent values can only be changed through reregistration.</span></span> <span data-ttu-id="4c0f3-223">Jedinou výjimkou je přiřazena k uzlu Konfigurace uzlu – přímo lze změnit v Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-223">The one exception is the Node Configuration assigned to the node -- this can be changed in Azure Automation DSC directly.</span></span>

<span data-ttu-id="4c0f3-224">Registrace lze provést stejným způsobem registrovat uzlu na začátku pomocí libovolné metody registrace popsané v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-224">Reregistration can be performed in the same way you registered the node initially, using any of the onboarding methods described in this document.</span></span> <span data-ttu-id="4c0f3-225">Není nutné před opětovná registrace se zrušit registraci uzlu z Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4c0f3-225">You do not need to unregister a node from Azure Automation DSC before reregistering it.</span></span>

## <a name="related-articles"></a><span data-ttu-id="4c0f3-226">Související články</span><span class="sxs-lookup"><span data-stu-id="4c0f3-226">Related Articles</span></span>

* [<span data-ttu-id="4c0f3-227">Přehled služby Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="4c0f3-227">Azure Automation DSC overview</span></span>](automation-dsc-overview.md)
* [<span data-ttu-id="4c0f3-228">Rutiny Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="4c0f3-228">Azure Automation DSC cmdlets</span></span>](/powershell/module/azurerm.automation/#automation)
* [<span data-ttu-id="4c0f3-229">Ceny služby Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="4c0f3-229">Azure Automation DSC pricing</span></span>](https://azure.microsoft.com/pricing/details/automation/)
