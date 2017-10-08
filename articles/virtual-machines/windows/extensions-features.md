---
title: "aaaVirtual počítače rozšíření a funkce pro Windows v Azure | Microsoft Docs"
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
ms.openlocfilehash: 61ccfd696b38e9be1026d836d5796c2346fd650f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-machine-extensions-and-features-for-windows"></a><span data-ttu-id="aae85-103">Rozšíření virtuálního počítače a funkce pro Windows</span><span class="sxs-lookup"><span data-stu-id="aae85-103">Virtual machine extensions and features for Windows</span></span>

<span data-ttu-id="aae85-104">Rozšíření virtuálního počítače Azure se malých aplikacích, které poskytují konfiguraci a automatizaci úloh po nasazení na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="aae85-104">Azure virtual machine extensions are small applications that provide post-deployment configuration and automation tasks on Azure virtual machines.</span></span> <span data-ttu-id="aae85-105">Například pokud virtuální počítač vyžaduje instalace softwaru, ochrana proti virům nebo Docker konfigurace, rozšíření virtuálního počítače může být použité toocomplete tyto úlohy.</span><span class="sxs-lookup"><span data-stu-id="aae85-105">For example, if a virtual machine requires software installation, anti-virus protection, or Docker configuration, a VM extension can be used toocomplete these tasks.</span></span> <span data-ttu-id="aae85-106">Rozšíření virtuálního počítače Azure můžete spustit pomocí hello rozhraní příkazového řádku Azure, prostředí PowerShell, šablon Azure Resource Manageru a hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="aae85-106">Azure VM extensions can be run by using hello Azure CLI, PowerShell, Azure Resource Manager templates, and hello Azure portal.</span></span> <span data-ttu-id="aae85-107">Rozšíření můžete dodávat s nové nasazení virtuálního počítače nebo spouštění všechny existující systém.</span><span class="sxs-lookup"><span data-stu-id="aae85-107">Extensions can be bundled with a new virtual machine deployment or run against any existing system.</span></span>

<span data-ttu-id="aae85-108">Tento dokument obsahuje přehled rozšíření virtuálního počítače, požadavky na tom, jak spravovat toodetect a odeberte rozšíření virtuálního počítače pomocí rozšíření virtuálního počítače a pokyny.</span><span class="sxs-lookup"><span data-stu-id="aae85-108">This document provides an overview of virtual machine extensions, prerequisites for using virtual machine extensions, and guidance on how toodetect, manage, and remove virtual machine extensions.</span></span> <span data-ttu-id="aae85-109">Tento dokument obsahuje zobecněný informace, protože mnoho rozšíření virtuálního počítače nejsou k dispozici, každý s konfigurací potenciálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="aae85-109">This document provides generalized information because many VM extensions are available, each with a potentially unique configuration.</span></span> <span data-ttu-id="aae85-110">Podrobnosti o konkrétní rozšíření najdete v každé jednotlivé rozšíření konkrétní toohello dokumentu.</span><span class="sxs-lookup"><span data-stu-id="aae85-110">Extension-specific details can be found in each document specific toohello individual extension.</span></span>

## <a name="use-cases-and-samples"></a><span data-ttu-id="aae85-111">Případy použití a ukázky</span><span class="sxs-lookup"><span data-stu-id="aae85-111">Use cases and samples</span></span>

<span data-ttu-id="aae85-112">Nejsou k dispozici mnoho různých rozšíření virtuálního počítače Azure, každý s konkrétní případ použití.</span><span class="sxs-lookup"><span data-stu-id="aae85-112">There are many different Azure VM extensions available, each with a specific use case.</span></span> <span data-ttu-id="aae85-113">Jsou některé případy použití příklad:</span><span class="sxs-lookup"><span data-stu-id="aae85-113">Some example use cases are:</span></span>

- <span data-ttu-id="aae85-114">Použijte PowerShell požadovaná stav konfigurace tooa virtuálního počítače pomocí rozšíření hello DSC pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="aae85-114">Apply PowerShell Desired State configurations tooa virtual machine by using hello DSC extension for Windows.</span></span> <span data-ttu-id="aae85-115">Další informace najdete v tématu [stavu Azure požadovaná konfigurace rozšíření](extensions-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="aae85-115">For more information, see [Azure Desired State configuration extension](extensions-dsc-overview.md).</span></span>
- <span data-ttu-id="aae85-116">Konfigurace monitorování virtuálních počítačů pomocí rozšíření virtuálního počítače agenta monitorování Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="aae85-116">Configure virtual machine monitoring by using hello Microsoft Monitoring Agent VM extension.</span></span> <span data-ttu-id="aae85-117">Další informace najdete v tématu [tooLog virtuální počítače Azure připojit Analytics](../../log-analytics/log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="aae85-117">For more information, see [Connect Azure virtual machines tooLog Analytics](../../log-analytics/log-analytics-azure-vm-extension.md).</span></span>
- <span data-ttu-id="aae85-118">Konfigurace sledování infrastruktury Azure s hello Datadog rozšíření.</span><span class="sxs-lookup"><span data-stu-id="aae85-118">Configure monitoring of your Azure infrastructure with hello Datadog extension.</span></span> <span data-ttu-id="aae85-119">Další informace najdete v tématu hello [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span><span class="sxs-lookup"><span data-stu-id="aae85-119">For more information, see hello [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span></span>
- <span data-ttu-id="aae85-120">Konfigurace virtuálního počítače Azure pomocí Chef.</span><span class="sxs-lookup"><span data-stu-id="aae85-120">Configure an Azure virtual machine by using Chef.</span></span> <span data-ttu-id="aae85-121">Další informace najdete v tématu [automatizace Azure nasazení virtuálního počítače s Chef](chef-automation.md).</span><span class="sxs-lookup"><span data-stu-id="aae85-121">For more information, see [Automating Azure virtual machine deployment with Chef](chef-automation.md).</span></span>

<span data-ttu-id="aae85-122">Kromě toho tooprocess konkrétní rozšíření, rozšíření vlastních skriptů je k dispozici pro virtuální počítače Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="aae85-122">In addition tooprocess-specific extensions, a Custom Script extension is available for both Windows and Linux virtual machines.</span></span> <span data-ttu-id="aae85-123">rozšíření vlastních skriptů pro Windows Hello umožňuje žádné toobe skript prostředí PowerShell spustit na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="aae85-123">hello Custom Script extension for Windows allows any PowerShell script toobe run on a virtual machine.</span></span> <span data-ttu-id="aae85-124">To je užitečné při návrhu Azure nasazení, které vyžadují konfiguraci nad rámec jaké nativní Azure nástrojů může poskytnout.</span><span class="sxs-lookup"><span data-stu-id="aae85-124">This is useful when you're designing Azure deployments that require configuration beyond what native Azure tooling can provide.</span></span> <span data-ttu-id="aae85-125">Další informace najdete v tématu [rozšíření skriptů vlastní virtuální počítač Windows](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="aae85-125">For more information, see [Windows VM Custom Script extension](extensions-customscript.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="aae85-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="aae85-126">Prerequisites</span></span>

<span data-ttu-id="aae85-127">Každé rozšíření virtuálního počítače může mít vlastní sadu požadavků.</span><span class="sxs-lookup"><span data-stu-id="aae85-127">Each virtual machine extension may have its own set of prerequisites.</span></span> <span data-ttu-id="aae85-128">Například hello rozšíření virtuálního počítače Docker má předpokladem podporované distribuce systému Linux.</span><span class="sxs-lookup"><span data-stu-id="aae85-128">For instance, hello Docker VM extension has a prerequisite of a supported Linux distribution.</span></span> <span data-ttu-id="aae85-129">Požadavky jednotlivých rozšíření jsou podrobně popsané v dokumentaci pro konkrétní rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="aae85-129">Requirements of individual extensions are detailed in hello extension-specific documentation.</span></span>

### <a name="azure-vm-agent"></a><span data-ttu-id="aae85-130">Agent virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="aae85-130">Azure VM agent</span></span>
<span data-ttu-id="aae85-131">agent virtuálního počítače Azure Hello spravuje interakci mezi virtuální počítač Azure a prostředků infrastruktury Azure řadiče hello.</span><span class="sxs-lookup"><span data-stu-id="aae85-131">hello Azure VM agent manages interaction between an Azure virtual machine and hello Azure fabric controller.</span></span> <span data-ttu-id="aae85-132">agent virtuálního počítače Hello je zodpovědná za funkční aspekty nasazení a správa virtuálních počítačích Azure, včetně spuštění rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aae85-132">hello VM agent is responsible for many functional aspects of deploying and managing Azure virtual machines, including running VM extensions.</span></span> <span data-ttu-id="aae85-133">agent virtuálního počítače Azure Hello je předinstalován v Azure Marketplace bitové kopie a může být nainstalována na podporovaných operačních systémech.</span><span class="sxs-lookup"><span data-stu-id="aae85-133">hello Azure VM agent is preinstalled on Azure Marketplace images and can be installed on supported operating systems.</span></span>

<span data-ttu-id="aae85-134">Informace o podporovaných operačních systémů a pokyny k instalaci najdete v tématu [agent virtuálního počítače Azure](agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="aae85-134">For information on supported operating systems and installation instructions, see [Azure virtual machine agent](agent-user-guide.md).</span></span>

## <a name="discover-vm-extensions"></a><span data-ttu-id="aae85-135">Zjistit rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="aae85-135">Discover VM extensions</span></span>
<span data-ttu-id="aae85-136">Mnoho různých rozšíření virtuálního počítače jsou k dispozici pro použití s virtuálními počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="aae85-136">Many different VM extensions are available for use with Azure virtual machines.</span></span> <span data-ttu-id="aae85-137">toosee úplný seznam, spusťte následující příkaz Powershellu pro Azure Resource Manager modulem hello hello.</span><span class="sxs-lookup"><span data-stu-id="aae85-137">toosee a complete list, run hello following command with hello Azure Resource Manager PowerShell module.</span></span> <span data-ttu-id="aae85-138">Pokud používáte tento příkaz, zkontrolujte že toospecify hello požadovaného umístění.</span><span class="sxs-lookup"><span data-stu-id="aae85-138">Make sure toospecify hello desired location when you're running this command.</span></span>

```powershell
Get-AzureRmVmImagePublisher -Location WestUS | `
Get-AzureRmVMExtensionImageType | `
Get-AzureRmVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a><span data-ttu-id="aae85-139">Spuštění rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="aae85-139">Run VM extensions</span></span>

<span data-ttu-id="aae85-140">Rozšíření virtuálního počítače Azure můžete spustit na existující virtuální počítače, což je užitečné, když potřebujete toomake změny konfigurace nebo obnovit připojení již nasazené virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aae85-140">Azure virtual machine extensions can be run on existing virtual machines, which is useful when you need toomake configuration changes or recover connectivity on an already deployed VM.</span></span> <span data-ttu-id="aae85-141">Rozšíření virtuálního počítače můžete také dodávat s nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="aae85-141">VM extensions can also be bundled with Azure Resource Manager template deployments.</span></span> <span data-ttu-id="aae85-142">Pomocí rozšíření pomocí šablony Resource Manageru můžete povolit toobe virtuální počítače Azure, nasazení a nakonfigurování hello nevyžaduje zásah po nasazení.</span><span class="sxs-lookup"><span data-stu-id="aae85-142">By using extensions with Resource Manager templates, you can enable Azure virtual machines toobe deployed and configured without hello need for post-deployment intervention.</span></span>

<span data-ttu-id="aae85-143">Hello následující metody se dá použít toorun rozšíření proti existujícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aae85-143">hello following methods can be used toorun an extension against an existing virtual machine.</span></span>

### <a name="powershell"></a><span data-ttu-id="aae85-144">PowerShell</span><span class="sxs-lookup"><span data-stu-id="aae85-144">PowerShell</span></span>

<span data-ttu-id="aae85-145">Existuje několik příkazů prostředí PowerShell pro spouštění jednotlivých rozšíření.</span><span class="sxs-lookup"><span data-stu-id="aae85-145">Several PowerShell commands exist for running individual extensions.</span></span> <span data-ttu-id="aae85-146">toosee seznamu, spusťte následující příkazy prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="aae85-146">toosee a list, run hello following PowerShell commands.</span></span>

```powershell
get-command Set-AzureRM*Extension* -Module AzureRM.Compute
```

<span data-ttu-id="aae85-147">To poskytuje podobné toohello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="aae85-147">This provides output similar toohello following:</span></span>

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

<span data-ttu-id="aae85-148">Hello následující příklad používá hello vlastní skript rozšíření toodownload skript z úložiště Githubu na hello cílový virtuální počítač a spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="aae85-148">hello following example uses hello Custom Script extension toodownload a script from a GitHub repository onto hello target virtual machine and then run hello script.</span></span> <span data-ttu-id="aae85-149">Další informace o hello rozšíření vlastních skriptů najdete v tématu [přehled rozšíření vlastních skriptů](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="aae85-149">For more information on hello Custom Script extension, see [Custom Script extension overview](extensions-customscript.md).</span></span>

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

<span data-ttu-id="aae85-150">V tomto příkladu je hello rozšíření pro přístup virtuálních počítačů používaných tooreset hello hesla pro správu systému Windows virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aae85-150">In this example, hello VM Access extension is used tooreset hello administrative password of a Windows virtual machine.</span></span> <span data-ttu-id="aae85-151">Další informace o hello rozšíření pro přístup virtuálních počítačů najdete v tématu [služby Vzdálená plocha resetovat ve virtuálním počítači Windows](reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="aae85-151">For more information on hello VM Access extension, see [Reset Remote Desktop service in a Windows VM](reset-rdp.md).</span></span>

```powershell
$cred=Get-Credential

Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

<span data-ttu-id="aae85-152">Hello `Set-AzureRmVMExtension` příkaz lze použít toostart žádné rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aae85-152">hello `Set-AzureRmVMExtension` command can be used toostart any VM extension.</span></span> <span data-ttu-id="aae85-153">Další informace najdete v tématu hello [odkaz na sadu AzureRmVMExtension](https://msdn.microsoft.com/en-us/library/mt603745.aspx).</span><span class="sxs-lookup"><span data-stu-id="aae85-153">For more information, see hello [Set-AzureRmVMExtension reference](https://msdn.microsoft.com/en-us/library/mt603745.aspx).</span></span>


### <a name="azure-portal"></a><span data-ttu-id="aae85-154">portál Azure</span><span class="sxs-lookup"><span data-stu-id="aae85-154">Azure portal</span></span>

<span data-ttu-id="aae85-155">Rozšíření virtuálního počítače může být použité tooan existujícího virtuálního počítače prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="aae85-155">A VM extension can be applied tooan existing virtual machine through hello Azure portal.</span></span> <span data-ttu-id="aae85-156">toodo tedy vyberte hello virtuální počítač má toouse, zvolte **rozšíření**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="aae85-156">toodo so, select hello virtual machine you want toouse, choose **Extensions**, and click **Add**.</span></span> <span data-ttu-id="aae85-157">To poskytuje seznam dostupných rozšíření.</span><span class="sxs-lookup"><span data-stu-id="aae85-157">This provides a list of available extensions.</span></span> <span data-ttu-id="aae85-158">Vyberte hello jeden chcete a postupujte podle kroků hello v Průvodci hello.</span><span class="sxs-lookup"><span data-stu-id="aae85-158">Select hello one you want and follow hello steps in hello wizard.</span></span>

<span data-ttu-id="aae85-159">Hello následující obrázek znázorňuje instalaci hello hello rozšíření Microsoft Antimalware z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="aae85-159">hello following image shows hello installation of hello Microsoft Antimalware extension from hello Azure portal.</span></span>

![Nainstalujte rozšíření proti malwaru](./media/extensions-features/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="aae85-161">Šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="aae85-161">Azure Resource Manager templates</span></span>

<span data-ttu-id="aae85-162">Rozšíření virtuálního počítače může být přidané tooan šablony Azure Resource Manageru a provést s hello nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="aae85-162">VM extensions can be added tooan Azure Resource Manager template and executed with hello deployment of hello template.</span></span> <span data-ttu-id="aae85-163">Nasazení rozšíření pomocí šablony je užitečné pro vytváření kompletně nakonfigurovaný Azure nasazení.</span><span class="sxs-lookup"><span data-stu-id="aae85-163">Deploying extensions with a template is useful for creating fully configured Azure deployments.</span></span> <span data-ttu-id="aae85-164">Například hello, pořízení následujícím kódu JSON ze šablony Resource Manageru, která nasadí sadu Vyrovnávání zatížení sítě virtuálních počítačů a Azure SQL database a poté nainstaluje aplikace .NET Core na každém virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="aae85-164">For example, hello following JSON is taken from a Resource Manager template that deploys a set of load-balanced virtual machines and an Azure SQL database, and then installs a .NET Core application on each VM.</span></span> <span data-ttu-id="aae85-165">Instalace softwaru hello postará Hello rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aae85-165">hello VM extension takes care of hello software installation.</span></span>

<span data-ttu-id="aae85-166">Další informace najdete v tématu hello [úplné šablony Resource Manageru](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="aae85-166">For more information, see hello [full Resource Manager template](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

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

<span data-ttu-id="aae85-167">Další informace najdete v tématu [šablon pro tvorbu Azure Resource Manageru pomocí rozšíření virtuálního počítače Windows](template-description.md#extensions).</span><span class="sxs-lookup"><span data-stu-id="aae85-167">For more information, see [Authoring Azure Resource Manager templates with Windows VM extensions](template-description.md#extensions).</span></span>

## <a name="secure-vm-extension-data"></a><span data-ttu-id="aae85-168">Zabezpečení dat rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="aae85-168">Secure VM extension data</span></span>

<span data-ttu-id="aae85-169">Když používáte rozšíření virtuálního počítače, může být nutné tooinclude citlivé informace, jako je například přihlašovací údaje, názvy účtů úložiště a přístupových klíčů k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aae85-169">When you're running a VM extension, it may be necessary tooinclude sensitive information such as credentials, storage account names, and storage account access keys.</span></span> <span data-ttu-id="aae85-170">Mnoho rozšíření virtuálního počítače zahrnují chráněné konfigurace, která data šifruje a dešifruje ji pouze uvnitř hello cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aae85-170">Many VM extensions include a protected configuration that encrypts data and only decrypts it inside hello target virtual machine.</span></span> <span data-ttu-id="aae85-171">Každé rozšíření obsahuje schéma konkrétní chráněné konfigurace, které budou popsané v dokumentaci konkrétní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="aae85-171">Each extension has a specific protected configuration schema that will be detailed in extension-specific documentation.</span></span>

<span data-ttu-id="aae85-172">Hello následující příklad ukazuje instanci hello rozšíření vlastních skriptů pro Windows.</span><span class="sxs-lookup"><span data-stu-id="aae85-172">hello following example shows an instance of hello Custom Script extension for Windows.</span></span> <span data-ttu-id="aae85-173">Všimněte si, že tento příkaz tooexecute hello zahrnuje sadu přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="aae85-173">Notice that hello command tooexecute includes a set of credentials.</span></span> <span data-ttu-id="aae85-174">V tomto příkladu hello příkaz tooexecute se šifrovat nebude.</span><span class="sxs-lookup"><span data-stu-id="aae85-174">In this example, hello command tooexecute will not be encrypted.</span></span>


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

<span data-ttu-id="aae85-175">Zabezpečené spouštění řetězec hello přesunutím hello **příkaz tooexecute** vlastnost toohello **chráněné** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="aae85-175">Secure hello execution string by moving hello **command tooexecute** property toohello **protected** configuration.</span></span>

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

## <a name="troubleshoot-vm-extensions"></a><span data-ttu-id="aae85-176">Řešení potíží s rozšířeními virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="aae85-176">Troubleshoot VM extensions</span></span>

<span data-ttu-id="aae85-177">Každé rozšíření virtuálního počítače může mít specifické pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="aae85-177">Each VM extension may have specific troubleshooting steps.</span></span> <span data-ttu-id="aae85-178">Například pokud používáte rozšíření vlastních skriptů hello, podrobnosti provádění skriptu naleznete místně na hello virtuálního počítače, na kterém byl spuštěn hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="aae85-178">For instance, when you're using hello Custom Script extension, script execution details can be found locally on hello virtual machine on which hello extension was run.</span></span> <span data-ttu-id="aae85-179">Kroky řešení potíží konkrétní rozšíření jsou podrobně popsané v dokumentaci k konkrétní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="aae85-179">Any extension-specific troubleshooting steps are detailed in extension-specific documentation.</span></span>

<span data-ttu-id="aae85-180">Hello následující kroky řešení potíží použít tooall rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aae85-180">hello following troubleshooting steps apply tooall virtual machine extensions.</span></span>

### <a name="view-extension-status"></a><span data-ttu-id="aae85-181">Zobrazit stav rozšíření</span><span class="sxs-lookup"><span data-stu-id="aae85-181">View extension status</span></span>

<span data-ttu-id="aae85-182">Po spuštění rozšíření virtuálního počítače pro virtuální počítač, použijte následující příkaz prostředí PowerShell, tooreturn stav rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="aae85-182">After a virtual machine extension has been run against a virtual machine, use hello following PowerShell command tooreturn extension status.</span></span> <span data-ttu-id="aae85-183">Názvy parametrů příkladu nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="aae85-183">Replace example parameter names with your own values.</span></span> <span data-ttu-id="aae85-184">Hello `Name` přebírá parametr hello název rozšíření toohello v době provedení.</span><span class="sxs-lookup"><span data-stu-id="aae85-184">hello `Name` parameter takes hello name given toohello extension at execution time.</span></span>

```PowerShell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="aae85-185">výstup Hello vypadá hello následující:</span><span class="sxs-lookup"><span data-stu-id="aae85-185">hello output looks like hello following:</span></span>

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

<span data-ttu-id="aae85-186">Stav spuštění rozšíření naleznete také v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="aae85-186">Extension execution status can also be found in hello Azure portal.</span></span> <span data-ttu-id="aae85-187">Stav hello tooview rozšíření, vyberte hello virtuálního počítače, zvolte **rozšíření**, a vyberte hello požadované rozšíření.</span><span class="sxs-lookup"><span data-stu-id="aae85-187">tooview hello status of an extension, select hello virtual machine, choose **Extensions**, and select hello desired extension.</span></span>

### <a name="rerun-vm-extensions"></a><span data-ttu-id="aae85-188">Znovu spustit, rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="aae85-188">Rerun VM extensions</span></span>

<span data-ttu-id="aae85-189">Můžou nastat případy, ve kterých rozšíření virtuálního počítače potřebuje toobe spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="aae85-189">There may be cases in which a virtual machine extension needs toobe rerun.</span></span> <span data-ttu-id="aae85-190">To provedete tak, že rozšíření hello s metodu provádění zvoleného zajistit opětovné spuštění rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="aae85-190">You can do this by removing hello extension and then rerunning hello extension with an execution method of your choice.</span></span> <span data-ttu-id="aae85-191">tooremove rozšíření, spusťte následující příkaz s modul Azure PowerShell hello hello.</span><span class="sxs-lookup"><span data-stu-id="aae85-191">tooremove an extension, run hello following command with hello Azure PowerShell module.</span></span> <span data-ttu-id="aae85-192">Názvy parametrů příkladu nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="aae85-192">Replace example parameter names with your own values.</span></span>

```powershell
Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="aae85-193">Rozšíření může být odebrán také pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="aae85-193">An extension can also be removed using hello Azure portal.</span></span> <span data-ttu-id="aae85-194">toodo tak:</span><span class="sxs-lookup"><span data-stu-id="aae85-194">toodo so:</span></span>

1. <span data-ttu-id="aae85-195">Vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="aae85-195">Select a virtual machine.</span></span>
2. <span data-ttu-id="aae85-196">Vyberte **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="aae85-196">Select **Extensions**.</span></span>
3. <span data-ttu-id="aae85-197">Zvolte rozšíření hello potřeby.</span><span class="sxs-lookup"><span data-stu-id="aae85-197">Choose hello desired extension.</span></span>
4. <span data-ttu-id="aae85-198">Vyberte **odinstalovat**.</span><span class="sxs-lookup"><span data-stu-id="aae85-198">Select **Uninstall**.</span></span>

## <a name="common-vm-extensions-reference"></a><span data-ttu-id="aae85-199">Běžné odkaz rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="aae85-199">Common VM extensions reference</span></span>
| <span data-ttu-id="aae85-200">Název rozšíření</span><span class="sxs-lookup"><span data-stu-id="aae85-200">Extension name</span></span> | <span data-ttu-id="aae85-201">Popis</span><span class="sxs-lookup"><span data-stu-id="aae85-201">Description</span></span> | <span data-ttu-id="aae85-202">Další informace</span><span class="sxs-lookup"><span data-stu-id="aae85-202">More information</span></span> |
| --- | --- | --- |
| <span data-ttu-id="aae85-203">Rozšíření vlastních skriptů pro Windows</span><span class="sxs-lookup"><span data-stu-id="aae85-203">Custom Script Extension for Windows</span></span> |<span data-ttu-id="aae85-204">Spouštění skriptů na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="aae85-204">Run scripts against an Azure virtual machine</span></span> |[<span data-ttu-id="aae85-205">Rozšíření vlastních skriptů pro Windows</span><span class="sxs-lookup"><span data-stu-id="aae85-205">Custom Script Extension for Windows</span></span>](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| <span data-ttu-id="aae85-206">Rozšíření DSC pro Windows</span><span class="sxs-lookup"><span data-stu-id="aae85-206">DSC Extension for Windows</span></span> |<span data-ttu-id="aae85-207">Rozšíření prostředí PowerShell DSC (Desired State Configuration)</span><span class="sxs-lookup"><span data-stu-id="aae85-207">PowerShell DSC (Desired State Configuration) Extension</span></span> |[<span data-ttu-id="aae85-208">Rozšíření DSC pro Windows</span><span class="sxs-lookup"><span data-stu-id="aae85-208">DSC Extension for Windows</span></span>](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| <span data-ttu-id="aae85-209">Rozšíření Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="aae85-209">Azure Diagnostics Extension</span></span> |<span data-ttu-id="aae85-210">Správa Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="aae85-210">Manage Azure Diagnostics</span></span> |[<span data-ttu-id="aae85-211">Rozšíření diagnostiky Azure</span><span class="sxs-lookup"><span data-stu-id="aae85-211">Azure Diagnostics Extension</span></span>](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| <span data-ttu-id="aae85-212">Rozšíření pro přístup virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="aae85-212">Azure VM Access Extension</span></span> |<span data-ttu-id="aae85-213">Spravovat uživatele a přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="aae85-213">Manage users and credentials</span></span> |[<span data-ttu-id="aae85-214">Rozšíření pro přístup virtuálních počítačů pro Linux</span><span class="sxs-lookup"><span data-stu-id="aae85-214">VM Access Extension for Linux</span></span>](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
