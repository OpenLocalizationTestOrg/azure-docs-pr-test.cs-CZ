---
title: "Rozšíření virtuálního počítače a funkce pro Windows v Azure | Microsoft Docs"
description: "Zjistěte, jaká rozšíření jsou k dispozici pro virtuální počítače Azure, seskupené podle co se poskytují nebo zvýšit."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 999d63ee-890e-432e-9391-25b3fc6cde28
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/06/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1ce0eebd2585c9457d7f922898d7f2fa3e7ffad7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="virtual-machine-extensions-and-features-for-windows"></a><span data-ttu-id="7fc5e-103">Rozšíření virtuálního počítače a funkce pro Windows</span><span class="sxs-lookup"><span data-stu-id="7fc5e-103">Virtual machine extensions and features for Windows</span></span>

<span data-ttu-id="7fc5e-104">Rozšíření virtuálního počítače Azure se malých aplikacích, které poskytují konfiguraci a automatizaci úloh po nasazení na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-104">Azure virtual machine extensions are small applications that provide post-deployment configuration and automation tasks on Azure virtual machines.</span></span> <span data-ttu-id="7fc5e-105">Například pokud virtuální počítač vyžaduje instalace softwaru, ochrana proti virům nebo Docker konfigurace, rozšíření virtuálního počítače můžete použít k dokončení těchto úloh.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-105">For example, if a virtual machine requires software installation, anti-virus protection, or Docker configuration, a VM extension can be used to complete these tasks.</span></span> <span data-ttu-id="7fc5e-106">Rozšíření virtuálního počítače Azure můžete spustit pomocí rozhraní příkazového řádku Azure, prostředí PowerShell, šablony Azure Resource Manager a portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-106">Azure VM extensions can be run by using the Azure CLI, PowerShell, Azure Resource Manager templates, and the Azure portal.</span></span> <span data-ttu-id="7fc5e-107">Rozšíření můžete dodávat s nové nasazení virtuálního počítače nebo spouštění všechny existující systém.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-107">Extensions can be bundled with a new virtual machine deployment or run against any existing system.</span></span>

<span data-ttu-id="7fc5e-108">Tento dokument obsahuje přehled rozšíření virtuálního počítače, požadavků na používání rozšíření virtuálního počítače a pokyny o tom, jak zjistit, spravovat a odebrání rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-108">This document provides an overview of virtual machine extensions, prerequisites for using virtual machine extensions, and guidance on how to detect, manage, and remove virtual machine extensions.</span></span> <span data-ttu-id="7fc5e-109">Tento dokument obsahuje zobecněný informace, protože mnoho rozšíření virtuálního počítače nejsou k dispozici, každý s konfigurací potenciálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-109">This document provides generalized information because many VM extensions are available, each with a potentially unique configuration.</span></span> <span data-ttu-id="7fc5e-110">Podrobnosti o konkrétní rozšíření najdete v každý dokument specifické pro jednotlivé rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-110">Extension-specific details can be found in each document specific to the individual extension.</span></span>

## <a name="use-cases-and-samples"></a><span data-ttu-id="7fc5e-111">Případy použití a ukázky</span><span class="sxs-lookup"><span data-stu-id="7fc5e-111">Use cases and samples</span></span>

<span data-ttu-id="7fc5e-112">Nejsou k dispozici mnoho různých rozšíření virtuálního počítače Azure, každý s konkrétní případ použití.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-112">There are many different Azure VM extensions available, each with a specific use case.</span></span> <span data-ttu-id="7fc5e-113">Jsou některé případy použití příklad:</span><span class="sxs-lookup"><span data-stu-id="7fc5e-113">Some example use cases are:</span></span>

- <span data-ttu-id="7fc5e-114">Použijte PowerShell požadovaná stav konfigurace virtuálního počítače pomocí rozšíření DSC pro Windows.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-114">Apply PowerShell Desired State configurations to a virtual machine by using the DSC extension for Windows.</span></span> <span data-ttu-id="7fc5e-115">Další informace najdete v tématu [stavu Azure požadovaná konfigurace rozšíření](extensions-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7fc5e-115">For more information, see [Azure Desired State configuration extension](extensions-dsc-overview.md).</span></span>
- <span data-ttu-id="7fc5e-116">Konfigurace monitorování virtuálních počítačů pomocí rozšíření Microsoft Monitoring Agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-116">Configure virtual machine monitoring by using the Microsoft Monitoring Agent VM extension.</span></span> <span data-ttu-id="7fc5e-117">Další informace najdete v tématu [virtuálních počítačích Azure připojit k analýze protokolů](../../log-analytics/log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="7fc5e-117">For more information, see [Connect Azure virtual machines to Log Analytics](../../log-analytics/log-analytics-azure-vm-extension.md).</span></span>
- <span data-ttu-id="7fc5e-118">Konfigurace sledování infrastruktury Azure s příponou Datadog.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-118">Configure monitoring of your Azure infrastructure with the Datadog extension.</span></span> <span data-ttu-id="7fc5e-119">Další informace najdete v tématu [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span><span class="sxs-lookup"><span data-stu-id="7fc5e-119">For more information, see the [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span></span>
- <span data-ttu-id="7fc5e-120">Konfigurace virtuálního počítače Azure pomocí Chef.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-120">Configure an Azure virtual machine by using Chef.</span></span> <span data-ttu-id="7fc5e-121">Další informace najdete v tématu [automatizace Azure nasazení virtuálního počítače s Chef](chef-automation.md).</span><span class="sxs-lookup"><span data-stu-id="7fc5e-121">For more information, see [Automating Azure virtual machine deployment with Chef](chef-automation.md).</span></span>

<span data-ttu-id="7fc5e-122">Kromě rozšíření specifické pro proces rozšíření vlastních skriptů je k dispozici pro virtuální počítače Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-122">In addition to process-specific extensions, a Custom Script extension is available for both Windows and Linux virtual machines.</span></span> <span data-ttu-id="7fc5e-123">Rozšíření vlastních skriptů pro systém Windows umožňuje všech skriptů prostředí PowerShell ke spuštění na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-123">The Custom Script extension for Windows allows any PowerShell script to be run on a virtual machine.</span></span> <span data-ttu-id="7fc5e-124">To je užitečné při návrhu Azure nasazení, které vyžadují konfiguraci nad rámec jaké nativní Azure nástrojů může poskytnout.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-124">This is useful when you're designing Azure deployments that require configuration beyond what native Azure tooling can provide.</span></span> <span data-ttu-id="7fc5e-125">Další informace najdete v tématu [rozšíření skriptů vlastní virtuální počítač Windows](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="7fc5e-125">For more information, see [Windows VM Custom Script extension](extensions-customscript.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="7fc5e-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7fc5e-126">Prerequisites</span></span>

<span data-ttu-id="7fc5e-127">Každé rozšíření virtuálního počítače může mít vlastní sadu požadavků.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-127">Each virtual machine extension may have its own set of prerequisites.</span></span> <span data-ttu-id="7fc5e-128">Například rozšíření virtuálního počítače Docker má předpokladem podporované distribuce systému Linux.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-128">For instance, the Docker VM extension has a prerequisite of a supported Linux distribution.</span></span> <span data-ttu-id="7fc5e-129">Požadavky jednotlivých rozšíření jsou podrobně popsané v dokumentaci k konkrétní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-129">Requirements of individual extensions are detailed in the extension-specific documentation.</span></span>

### <a name="azure-vm-agent"></a><span data-ttu-id="7fc5e-130">Agent virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="7fc5e-130">Azure VM agent</span></span>
<span data-ttu-id="7fc5e-131">Agent virtuálního počítače Azure spravuje interakci mezi virtuální počítač Azure a kontroleru prostředků infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-131">The Azure VM agent manages interaction between an Azure virtual machine and the Azure fabric controller.</span></span> <span data-ttu-id="7fc5e-132">Agent virtuálního počítače je zodpovědná za funkční aspekty nasazení a správa virtuálních počítačích Azure, včetně spuštění rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-132">The VM agent is responsible for many functional aspects of deploying and managing Azure virtual machines, including running VM extensions.</span></span> <span data-ttu-id="7fc5e-133">Agent virtuálního počítače Azure je předinstalován v Azure Marketplace bitové kopie a může být nainstalována na podporovaných operačních systémech.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-133">The Azure VM agent is preinstalled on Azure Marketplace images and can be installed on supported operating systems.</span></span>

<span data-ttu-id="7fc5e-134">Informace o podporovaných operačních systémů a pokyny k instalaci najdete v tématu [agent virtuálního počítače Azure](agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="7fc5e-134">For information on supported operating systems and installation instructions, see [Azure virtual machine agent](agent-user-guide.md).</span></span>

## <a name="discover-vm-extensions"></a><span data-ttu-id="7fc5e-135">Zjistit rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="7fc5e-135">Discover VM extensions</span></span>
<span data-ttu-id="7fc5e-136">Mnoho různých rozšíření virtuálního počítače jsou k dispozici pro použití s virtuálními počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-136">Many different VM extensions are available for use with Azure virtual machines.</span></span> <span data-ttu-id="7fc5e-137">Pokud chcete zobrazit úplný seznam, spusťte následující příkaz s modul Powershellu pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-137">To see a complete list, run the following command with the Azure Resource Manager PowerShell module.</span></span> <span data-ttu-id="7fc5e-138">Ujistěte se, že pokud používáte tento příkaz zadejte požadované umístění.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-138">Make sure to specify the desired location when you're running this command.</span></span>

```powershell
Get-AzureRmVmImagePublisher -Location WestUS | `
Get-AzureRmVMExtensionImageType | `
Get-AzureRmVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a><span data-ttu-id="7fc5e-139">Spuštění rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="7fc5e-139">Run VM extensions</span></span>

<span data-ttu-id="7fc5e-140">Rozšíření virtuálního počítače Azure lze spustit na existující virtuální počítače, což je užitečné, když potřebujete udělat změny konfigurace nebo obnovit připojení již nasazené virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-140">Azure virtual machine extensions can be run on existing virtual machines, which is useful when you need to make configuration changes or recover connectivity on an already deployed VM.</span></span> <span data-ttu-id="7fc5e-141">Rozšíření virtuálního počítače můžete také dodávat s nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-141">VM extensions can also be bundled with Azure Resource Manager template deployments.</span></span> <span data-ttu-id="7fc5e-142">Pomocí rozšíření pomocí šablony Resource Manageru můžete povolit virtuální počítače Azure k nasazení a nakonfigurování bez nutnosti zásahu po nasazení.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-142">By using extensions with Resource Manager templates, you can enable Azure virtual machines to be deployed and configured without the need for post-deployment intervention.</span></span>

<span data-ttu-id="7fc5e-143">Tyto metody slouží ke spuštění rozšíření stávajícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-143">The following methods can be used to run an extension against an existing virtual machine.</span></span>

### <a name="powershell"></a><span data-ttu-id="7fc5e-144">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7fc5e-144">PowerShell</span></span>

<span data-ttu-id="7fc5e-145">Existuje několik příkazů prostředí PowerShell pro spouštění jednotlivých rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-145">Several PowerShell commands exist for running individual extensions.</span></span> <span data-ttu-id="7fc5e-146">Pokud chcete zobrazit seznam, spusťte následující příkazy prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-146">To see a list, run the following PowerShell commands.</span></span>

```powershell
get-command Set-AzureRM*Extension* -Module AzureRM.Compute
```

<span data-ttu-id="7fc5e-147">To poskytuje výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="7fc5e-147">This provides output similar to the following:</span></span>

```powershell
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Set-AzureRmVMAccessExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMADDomainExtension                     2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMAEMExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBackupExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBginfoExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMChefExtension                         2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMCustomScriptExtension                 2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiagnosticsExtension                  2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiskEncryptionExtension               2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDscExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMExtension                             2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMSqlServerExtension                    2.2.0      AzureRM.Compute
```

<span data-ttu-id="7fc5e-148">Následující příklad používá rozšíření vlastních skriptů na stažení skriptu z úložiště Githubu do cílového virtuálního počítače a pak spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-148">The following example uses the Custom Script extension to download a script from a GitHub repository onto the target virtual machine and then run the script.</span></span> <span data-ttu-id="7fc5e-149">Další informace o rozšíření vlastních skriptů najdete v tématu [přehled rozšíření vlastních skriptů](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="7fc5e-149">For more information on the Custom Script extension, see [Custom Script extension overview](extensions-customscript.md).</span></span>

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

<span data-ttu-id="7fc5e-150">V tomto příkladu se používá rozšíření pro virtuální počítač přístup k resetování hesla pro správu systému Windows virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-150">In this example, the VM Access extension is used to reset the administrative password of a Windows virtual machine.</span></span> <span data-ttu-id="7fc5e-151">Další informace o rozšíření pro přístup virtuálních počítačů najdete v tématu [služby Vzdálená plocha resetovat ve virtuálním počítači Windows](reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="7fc5e-151">For more information on the VM Access extension, see [Reset Remote Desktop service in a Windows VM](reset-rdp.md).</span></span>

```powershell
$cred=Get-Credential

Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

<span data-ttu-id="7fc5e-152">`Set-AzureRmVMExtension` Příkaz můžete použít ke spuštění všech rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-152">The `Set-AzureRmVMExtension` command can be used to start any VM extension.</span></span> <span data-ttu-id="7fc5e-153">Další informace najdete v tématu [odkaz na sadu AzureRmVMExtension](https://msdn.microsoft.com/en-us/library/mt603745.aspx).</span><span class="sxs-lookup"><span data-stu-id="7fc5e-153">For more information, see the [Set-AzureRmVMExtension reference](https://msdn.microsoft.com/en-us/library/mt603745.aspx).</span></span>


### <a name="azure-portal"></a><span data-ttu-id="7fc5e-154">portál Azure</span><span class="sxs-lookup"><span data-stu-id="7fc5e-154">Azure portal</span></span>

<span data-ttu-id="7fc5e-155">Rozšíření virtuálního počítače je použít pro existující virtuální počítač prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-155">A VM extension can be applied to an existing virtual machine through the Azure portal.</span></span> <span data-ttu-id="7fc5e-156">To pokud chcete udělat, vyberte virtuální počítač, který chcete použít, zvolte **rozšíření**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-156">To do so, select the virtual machine you want to use, choose **Extensions**, and click **Add**.</span></span> <span data-ttu-id="7fc5e-157">To poskytuje seznam dostupných rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-157">This provides a list of available extensions.</span></span> <span data-ttu-id="7fc5e-158">Vyberte možnost, chcete a postupujte podle kroků v průvodci.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-158">Select the one you want and follow the steps in the wizard.</span></span>

<span data-ttu-id="7fc5e-159">Následující obrázek znázorňuje instalaci rozšíření Microsoft Antimalware z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-159">The following image shows the installation of the Microsoft Antimalware extension from the Azure portal.</span></span>

![Nainstalujte rozšíření proti malwaru](./media/extensions-features/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="7fc5e-161">Šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="7fc5e-161">Azure Resource Manager templates</span></span>

<span data-ttu-id="7fc5e-162">Rozšíření virtuálních počítačů můžete přidat do šablony Azure Resource Manager a provést při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-162">VM extensions can be added to an Azure Resource Manager template and executed with the deployment of the template.</span></span> <span data-ttu-id="7fc5e-163">Nasazení rozšíření pomocí šablony je užitečné pro vytváření kompletně nakonfigurovaný Azure nasazení.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-163">Deploying extensions with a template is useful for creating fully configured Azure deployments.</span></span> <span data-ttu-id="7fc5e-164">Například následující kód JSON je převzat ze šablony Resource Manageru, která nasadí sada virtuálních počítačů s vyrovnáváním zatížení a Azure SQL database a poté nainstaluje aplikace .NET Core na každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-164">For example, the following JSON is taken from a Resource Manager template that deploys a set of load-balanced virtual machines and an Azure SQL database, and then installs a .NET Core application on each VM.</span></span> <span data-ttu-id="7fc5e-165">Rozšíření virtuálního počítače má na starosti instalace softwaru.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-165">The VM extension takes care of the software installation.</span></span>

<span data-ttu-id="7fc5e-166">Další informace najdete v tématu [úplné šablony Resource Manageru](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="7fc5e-166">For more information, see the [full Resource Manager template](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

<span data-ttu-id="7fc5e-167">Další informace najdete v tématu [šablon pro tvorbu Azure Resource Manageru pomocí rozšíření virtuálního počítače Windows](template-description.md#extensions).</span><span class="sxs-lookup"><span data-stu-id="7fc5e-167">For more information, see [Authoring Azure Resource Manager templates with Windows VM extensions](template-description.md#extensions).</span></span>

## <a name="secure-vm-extension-data"></a><span data-ttu-id="7fc5e-168">Zabezpečení dat rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="7fc5e-168">Secure VM extension data</span></span>

<span data-ttu-id="7fc5e-169">Když používáte rozšíření virtuálního počítače, může být nutné zahrnovat citlivé informace, jako je například přihlašovací údaje, názvy účtů úložiště a přístupových klíčů k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-169">When you're running a VM extension, it may be necessary to include sensitive information such as credentials, storage account names, and storage account access keys.</span></span> <span data-ttu-id="7fc5e-170">Mnoho rozšíření virtuálního počítače zahrnují chráněné konfigurace, která data šifruje a dešifruje ji pouze uvnitř cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-170">Many VM extensions include a protected configuration that encrypts data and only decrypts it inside the target virtual machine.</span></span> <span data-ttu-id="7fc5e-171">Každé rozšíření obsahuje schéma konkrétní chráněné konfigurace, které budou popsané v dokumentaci konkrétní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-171">Each extension has a specific protected configuration schema that will be detailed in extension-specific documentation.</span></span>

<span data-ttu-id="7fc5e-172">Následující příklad ukazuje instanci rozšíření vlastních skriptů pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-172">The following example shows an instance of the Custom Script extension for Windows.</span></span> <span data-ttu-id="7fc5e-173">Všimněte si, že příkaz k provedení obsahuje sadu přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-173">Notice that the command to execute includes a set of credentials.</span></span> <span data-ttu-id="7fc5e-174">V tomto příkladu spuštění příkazu se šifrovat nebude.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-174">In this example, the command to execute will not be encrypted.</span></span>


```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ],
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

<span data-ttu-id="7fc5e-175">Zabezpečené spouštění řetězec přesunutím **příkaz k provedení** vlastnost, která má **chráněné** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-175">Secure the execution string by moving the **command to execute** property to the **protected** configuration.</span></span>

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

## <a name="troubleshoot-vm-extensions"></a><span data-ttu-id="7fc5e-176">Řešení potíží s rozšířeními virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="7fc5e-176">Troubleshoot VM extensions</span></span>

<span data-ttu-id="7fc5e-177">Každé rozšíření virtuálního počítače může mít specifické pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-177">Each VM extension may have specific troubleshooting steps.</span></span> <span data-ttu-id="7fc5e-178">Například pokud používáte rozšíření vlastních skriptů, podrobnosti provádění skriptu naleznete místně na virtuálním počítači, na kterém byl rozšíření spustit.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-178">For instance, when you're using the Custom Script extension, script execution details can be found locally on the virtual machine on which the extension was run.</span></span> <span data-ttu-id="7fc5e-179">Kroky řešení potíží konkrétní rozšíření jsou podrobně popsané v dokumentaci k konkrétní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-179">Any extension-specific troubleshooting steps are detailed in extension-specific documentation.</span></span>

<span data-ttu-id="7fc5e-180">Následující kroky řešení potíží použít na všechny přípony virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-180">The following troubleshooting steps apply to all virtual machine extensions.</span></span>

### <a name="view-extension-status"></a><span data-ttu-id="7fc5e-181">Zobrazit stav rozšíření</span><span class="sxs-lookup"><span data-stu-id="7fc5e-181">View extension status</span></span>

<span data-ttu-id="7fc5e-182">Po spuštění rozšíření virtuálního počítače pro virtuální počítač, použijte následující příkaz prostředí PowerShell vrátit stav rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-182">After a virtual machine extension has been run against a virtual machine, use the following PowerShell command to return extension status.</span></span> <span data-ttu-id="7fc5e-183">Názvy parametrů příkladu nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-183">Replace example parameter names with your own values.</span></span> <span data-ttu-id="7fc5e-184">`Name` Přebírá parametr daný název rozšíření v době provedení.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-184">The `Name` parameter takes the name given to the extension at execution time.</span></span>

```PowerShell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="7fc5e-185">Výstup vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="7fc5e-185">The output looks like the following:</span></span>

```json
ResourceGroupName       : myResourceGroup
VMName                  : myVM
Name                    : myExtensionName
Location                : westus
Etag                    : null
Publisher               : Microsoft.Azure.Extensions
ExtensionType           : DockerExtension
TypeHandlerVersion      : 1.0
Id                      : /subscriptions/mySubscriptionIS/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM/extensions/myExtensionName
PublicSettings          :
ProtectedSettings       :
ProvisioningState       : Succeeded
Statuses                :
SubStatuses             :
AutoUpgradeMinorVersion : False
ForceUpdateTag          :
```

<span data-ttu-id="7fc5e-186">Stav spuštění rozšíření možné také najít na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-186">Extension execution status can also be found in the Azure portal.</span></span> <span data-ttu-id="7fc5e-187">Chcete-li zobrazit stav rozšíření, vyberte virtuální počítač, zvolte **rozšíření**a vyberte požadované rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-187">To view the status of an extension, select the virtual machine, choose **Extensions**, and select the desired extension.</span></span>

### <a name="rerun-vm-extensions"></a><span data-ttu-id="7fc5e-188">Znovu spustit, rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="7fc5e-188">Rerun VM extensions</span></span>

<span data-ttu-id="7fc5e-189">Můžou nastat případy, ve kterých musí být znovu rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-189">There may be cases in which a virtual machine extension needs to be rerun.</span></span> <span data-ttu-id="7fc5e-190">To provedete pomocí odebírání rozšíření a potom znovu spustit rozšíření s metodu provádění podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-190">You can do this by removing the extension and then rerunning the extension with an execution method of your choice.</span></span> <span data-ttu-id="7fc5e-191">Chcete-li odebrat rozšíření, spusťte následující příkaz s modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-191">To remove an extension, run the following command with the Azure PowerShell module.</span></span> <span data-ttu-id="7fc5e-192">Názvy parametrů příkladu nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-192">Replace example parameter names with your own values.</span></span>

```powershell
Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="7fc5e-193">Rozšíření může být odebrán také pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-193">An extension can also be removed using the Azure portal.</span></span> <span data-ttu-id="7fc5e-194">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="7fc5e-194">To do so:</span></span>

1. <span data-ttu-id="7fc5e-195">Vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-195">Select a virtual machine.</span></span>
2. <span data-ttu-id="7fc5e-196">Vyberte **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-196">Select **Extensions**.</span></span>
3. <span data-ttu-id="7fc5e-197">Vyberte požadované rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-197">Choose the desired extension.</span></span>
4. <span data-ttu-id="7fc5e-198">Vyberte **odinstalovat**.</span><span class="sxs-lookup"><span data-stu-id="7fc5e-198">Select **Uninstall**.</span></span>

## <a name="common-vm-extensions-reference"></a><span data-ttu-id="7fc5e-199">Běžné odkaz rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="7fc5e-199">Common VM extensions reference</span></span>
| <span data-ttu-id="7fc5e-200">Název rozšíření</span><span class="sxs-lookup"><span data-stu-id="7fc5e-200">Extension name</span></span> | <span data-ttu-id="7fc5e-201">Popis</span><span class="sxs-lookup"><span data-stu-id="7fc5e-201">Description</span></span> | <span data-ttu-id="7fc5e-202">Další informace</span><span class="sxs-lookup"><span data-stu-id="7fc5e-202">More information</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7fc5e-203">Rozšíření vlastních skriptů pro Windows</span><span class="sxs-lookup"><span data-stu-id="7fc5e-203">Custom Script Extension for Windows</span></span> |<span data-ttu-id="7fc5e-204">Spouštění skriptů na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="7fc5e-204">Run scripts against an Azure virtual machine</span></span> |[<span data-ttu-id="7fc5e-205">Rozšíření vlastních skriptů pro Windows</span><span class="sxs-lookup"><span data-stu-id="7fc5e-205">Custom Script Extension for Windows</span></span>](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| <span data-ttu-id="7fc5e-206">Rozšíření DSC pro Windows</span><span class="sxs-lookup"><span data-stu-id="7fc5e-206">DSC Extension for Windows</span></span> |<span data-ttu-id="7fc5e-207">Rozšíření prostředí PowerShell DSC (Desired State Configuration)</span><span class="sxs-lookup"><span data-stu-id="7fc5e-207">PowerShell DSC (Desired State Configuration) Extension</span></span> |[<span data-ttu-id="7fc5e-208">Rozšíření DSC pro Windows</span><span class="sxs-lookup"><span data-stu-id="7fc5e-208">DSC Extension for Windows</span></span>](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| <span data-ttu-id="7fc5e-209">Rozšíření Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="7fc5e-209">Azure Diagnostics Extension</span></span> |<span data-ttu-id="7fc5e-210">Správa Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="7fc5e-210">Manage Azure Diagnostics</span></span> |[<span data-ttu-id="7fc5e-211">Rozšíření diagnostiky Azure</span><span class="sxs-lookup"><span data-stu-id="7fc5e-211">Azure Diagnostics Extension</span></span>](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| <span data-ttu-id="7fc5e-212">Rozšíření pro přístup virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="7fc5e-212">Azure VM Access Extension</span></span> |<span data-ttu-id="7fc5e-213">Spravovat uživatele a přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="7fc5e-213">Manage users and credentials</span></span> |[<span data-ttu-id="7fc5e-214">Rozšíření pro přístup virtuálních počítačů pro Linux</span><span class="sxs-lookup"><span data-stu-id="7fc5e-214">VM Access Extension for Linux</span></span>](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
