---
title: "Konfigurace požadovaného stavu Azure přehled | Microsoft Docs"
description: "Přehled pro používání obslužná rutina rozšíření Microsoft Azure pro konfigurace požadovaného stavu prostředí PowerShell. Včetně požadavky, architektura, rutiny..."
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 01/09/2017
ms.author: zachal
ms.openlocfilehash: c05c2d541a5f526f362f9cd72fe6d878374112b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a><span data-ttu-id="84e4c-104">Úvod do rozšíření obslužné rutiny konfigurace požadovaného stavu Azure</span><span class="sxs-lookup"><span data-stu-id="84e4c-104">Introduction to the Azure Desired State Configuration extension handler</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="84e4c-105">Agent virtuálního počítače Azure a související rozšíření jsou součástí infrastrukturní služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="84e4c-105">The Azure VM Agent and associated Extensions are part of the Microsoft Azure Infrastructure Services.</span></span> <span data-ttu-id="84e4c-106">Rozšíření virtuálního počítače jsou softwarové komponenty, které rozšiřují funkce virtuálních počítačů a zjednodušit různé operace správy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="84e4c-106">VM Extensions are software components that extend the VM functionality and simplify various VM management operations.</span></span> <span data-ttu-id="84e4c-107">Například slouží rozšíření VMAccess k resetování hesla správce nebo rozšíření vlastních skriptů můžete použít ke spuštění skriptu ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="84e4c-107">For example, the VMAccess extension can be used to reset an administrator's password, or the Custom Script extension can be used to execute a script on the VM.</span></span>

<span data-ttu-id="84e4c-108">Tento článek představuje rozšíření prostředí PowerShell požadovaného stavu konfigurace (DSC) pro virtuální počítače Azure v rámci sady Azure SDK prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84e4c-108">This article introduces the PowerShell Desired State Configuration (DSC) Extension for Azure VMs as part of the Azure PowerShell SDK.</span></span> <span data-ttu-id="84e4c-109">Nové rutiny můžete používat k odesílání a použití konfigurace DSC prostředí PowerShell ve virtuálním počítači Azure s příponou DSC prostředí PowerShell povolené.</span><span class="sxs-lookup"><span data-stu-id="84e4c-109">You can use new cmdlets to upload and apply a PowerShell DSC configuration on an Azure VM enabled with the PowerShell DSC extension.</span></span> <span data-ttu-id="84e4c-110">DSC prostředí PowerShell rozšíření volání do prostředí PowerShell DSC uplatní obdržená konfigurace DSC do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="84e4c-110">The PowerShell DSC extension calls into PowerShell DSC to enact the received DSC configuration on the VM.</span></span> <span data-ttu-id="84e4c-111">Tato funkce je také k dispozici prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="84e4c-111">This functionality is also available through the Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84e4c-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="84e4c-112">Prerequisites</span></span>
<span data-ttu-id="84e4c-113">**Místní počítač** pro interakci s rozšíření virtuálního počítače Azure, budete muset použít portál Azure nebo Azure PowerShell SDK.</span><span class="sxs-lookup"><span data-stu-id="84e4c-113">**Local machine** To interact with the Azure VM extension, you need to use either the Azure portal or the Azure PowerShell SDK.</span></span> 

<span data-ttu-id="84e4c-114">**Agent hosta** virtuálního počítače Azure, který je nakonfigurovaný pomocí konfigurace DSC musí být operační systém, který podporuje buď Windows Management Framework (WMF) 4.0 nebo 5.0.</span><span class="sxs-lookup"><span data-stu-id="84e4c-114">**Guest Agent** The Azure VM that is configured by the DSC configuration needs to be an OS that supports either Windows Management Framework (WMF) 4.0 or 5.0.</span></span> <span data-ttu-id="84e4c-115">Úplný seznam podporovaných verzí operačního systému najdete na [historie verzí rozšíření DSC](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).</span><span class="sxs-lookup"><span data-stu-id="84e4c-115">The full list of supported OS versions can be found at the [DSC Extension Version History](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).</span></span>

## <a name="terms-and-concepts"></a><span data-ttu-id="84e4c-116">Podmínky a koncepty</span><span class="sxs-lookup"><span data-stu-id="84e4c-116">Terms and concepts</span></span>
<span data-ttu-id="84e4c-117">Tato příručka předpokládá znalost následující koncepty:</span><span class="sxs-lookup"><span data-stu-id="84e4c-117">This guide presumes familiarity with the following concepts:</span></span>

<span data-ttu-id="84e4c-118">Konfigurace – dokumentu konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="84e4c-118">Configuration - A DSC configuration document.</span></span> 

<span data-ttu-id="84e4c-119">Uzel - cíle pro konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="84e4c-119">Node - A target for a DSC configuration.</span></span> <span data-ttu-id="84e4c-120">V tomto dokumentu vždy "uzel" odkazuje na virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="84e4c-120">In this document, "node" always refers to an Azure VM.</span></span>

<span data-ttu-id="84e4c-121">Konfigurační Data - .psd1 soubor obsahující data prostředí pro konfiguraci</span><span class="sxs-lookup"><span data-stu-id="84e4c-121">Configuration Data - A .psd1 file containing environmental data for a configuration</span></span>

## <a name="architectural-overview"></a><span data-ttu-id="84e4c-122">Přehled architektury</span><span class="sxs-lookup"><span data-stu-id="84e4c-122">Architectural overview</span></span>
<span data-ttu-id="84e4c-123">Rozšíření Azure DSC používá rozhraní agenta virtuálního počítače Azure, aby doručily, uplatní a sestav o konfiguracích DSC běžící na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="84e4c-123">The Azure DSC extension uses the Azure VM Agent framework to deliver, enact, and report on DSC configurations running on Azure VMs.</span></span> <span data-ttu-id="84e4c-124">Rozšíření DSC očekává, že soubor ZIP obsahující alespoň dokumentu konfigurace a sadu parametrů zadaných prostřednictvím sady SDK Azure PowerShell nebo prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="84e4c-124">The DSC extension expects a .zip file containing at least a configuration document, and a set of parameters provided either through the Azure PowerShell SDK or through the Azure portal.</span></span>

<span data-ttu-id="84e4c-125">Při prvním volání rozšíření, spustí instalační proces.</span><span class="sxs-lookup"><span data-stu-id="84e4c-125">When the extension is called for the first time, it runs an installation process.</span></span> <span data-ttu-id="84e4c-126">Tento proces nainstaluje verzi Windows Management Framework (WMF), ke pomocí následujícího postupu:</span><span class="sxs-lookup"><span data-stu-id="84e4c-126">This process installs a version of the Windows Management Framework (WMF) using the following logic:</span></span>

1. <span data-ttu-id="84e4c-127">Je-li operačním systémem virtuálního počítače Azure Windows Server 2016, nebyla provedena žádná akce.</span><span class="sxs-lookup"><span data-stu-id="84e4c-127">If the Azure VM OS is Windows Server 2016, no action is taken.</span></span> <span data-ttu-id="84e4c-128">Windows Server 2016 již nejnovější verzi prostředí PowerShell nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="84e4c-128">Windows Server 2016 already has the latest version of PowerShell installed.</span></span>
2. <span data-ttu-id="84e4c-129">Pokud `wmfVersion` je zadána vlastnost, je nainstalovaná této verze WMF, pokud není kompatibilní s operačním systémem Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="84e4c-129">If the `wmfVersion` property is specified, that version of the WMF is installed unless it is incompatible with the VM's OS.</span></span>
3. <span data-ttu-id="84e4c-130">Pokud žádné `wmfVersion` je zadána vlastnost, je nainstalovaná nejnovější verze WMF, která je použít.</span><span class="sxs-lookup"><span data-stu-id="84e4c-130">If no `wmfVersion` property is specified, the latest applicable version of the WMF is installed.</span></span>

<span data-ttu-id="84e4c-131">Instalace WMF vyžaduje restart.</span><span class="sxs-lookup"><span data-stu-id="84e4c-131">Installation of the WMF requires a reboot.</span></span> <span data-ttu-id="84e4c-132">Po restartování rozšíření stáhne zadaný v souboru ZIP `modulesUrl` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="84e4c-132">After reboot, the extension downloads the .zip file specified in the `modulesUrl` property.</span></span> <span data-ttu-id="84e4c-133">Pokud toto umístění je v Azure blob storage, SAS token lze zadat v `sasToken` vlastnost, která má přístup k souboru.</span><span class="sxs-lookup"><span data-stu-id="84e4c-133">If this location is in Azure blob storage, a SAS token can be specified in the `sasToken` property to access the file.</span></span> <span data-ttu-id="84e4c-134">Po stažení a vybaleno ZIP funkce konfigurace definované v `configurationFunction` běží ke generování souboru MOF.</span><span class="sxs-lookup"><span data-stu-id="84e4c-134">After the .zip is downloaded and unpacked, the configuration function defined in `configurationFunction` is run to generate the MOF file.</span></span> <span data-ttu-id="84e4c-135">Rozšíření se pak spustí `Start-DscConfiguration -Force` na vygenerovaný soubor MOF.</span><span class="sxs-lookup"><span data-stu-id="84e4c-135">The extension then runs `Start-DscConfiguration -Force` on the generated MOF file.</span></span> <span data-ttu-id="84e4c-136">Přípona zachytí výstup a zápis zpátky stav kanál Azure.</span><span class="sxs-lookup"><span data-stu-id="84e4c-136">The extension captures output and writes it back out to the Azure Status Channel.</span></span> <span data-ttu-id="84e4c-137">Z tohoto bodu na DSC LCM zpracovává monitorování a oprava jako normální.</span><span class="sxs-lookup"><span data-stu-id="84e4c-137">From this point on, the DSC LCM handles monitoring and correction as normal.</span></span> 

## <a name="powershell-cmdlets"></a><span data-ttu-id="84e4c-138">Rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="84e4c-138">PowerShell cmdlets</span></span>
<span data-ttu-id="84e4c-139">Rutiny prostředí PowerShell použít s Azure Resource Manager nebo modelu nasazení classic do balíčku, publikovat a monitorování nasazení rozšíření DSC.</span><span class="sxs-lookup"><span data-stu-id="84e4c-139">PowerShell cmdlets can be used with Azure Resource Manager or the classic deployment model to package, publish, and monitor DSC extension deployments.</span></span> <span data-ttu-id="84e4c-140">Moduly nasazení classic jsou následující rutiny uvedené, ale lze "Azure" nahradit "AzureRm" pomocí modelu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="84e4c-140">The following cmdlets listed are the classic deployment modules, but "Azure" can be replaced with "AzureRm" to use the Azure Resource Manager model.</span></span> <span data-ttu-id="84e4c-141">Například `Publish-AzureVMDscConfiguration` používá model nasazení classic, kde `Publish-AzureRmVMDscConfiguration` používá Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="84e4c-141">For example,  `Publish-AzureVMDscConfiguration` uses the classic deployment model, where `Publish-AzureRmVMDscConfiguration` uses Azure Resource Manager.</span></span> 

<span data-ttu-id="84e4c-142">`Publish-AzureVMDscConfiguration`trvá v konfiguračním souboru, hledá závislé prostředky DSC a vytvoří soubor ZIP obsahující konfiguraci a potřebné k uplatní konfiguraci prostředků DSC.</span><span class="sxs-lookup"><span data-stu-id="84e4c-142">`Publish-AzureVMDscConfiguration` takes in a configuration file, scans it for dependent DSC resources, and creates a .zip file containing the configuration and DSC resources needed to enact the configuration.</span></span> <span data-ttu-id="84e4c-143">Můžete také vytvořit balíček místně pomocí `-ConfigurationArchivePath` parametr.</span><span class="sxs-lookup"><span data-stu-id="84e4c-143">It can also create the package locally using the `-ConfigurationArchivePath` parameter.</span></span> <span data-ttu-id="84e4c-144">Jinak publikuje soubor .zip do Azure blob storage a zabezpečuje s tokenem SAS.</span><span class="sxs-lookup"><span data-stu-id="84e4c-144">Otherwise, it publishes the .zip file to Azure blob storage and secures it with a SAS token.</span></span>

<span data-ttu-id="84e4c-145">Soubor .zip vytvořených touto rutinou má .ps1 konfigurační skript v kořenové složce archivu.</span><span class="sxs-lookup"><span data-stu-id="84e4c-145">The .zip file created by this cmdlet has the .ps1 configuration script at the root of the archive folder.</span></span> <span data-ttu-id="84e4c-146">Prostředky mít složce modulu, který je umístěn ve složce archivu.</span><span class="sxs-lookup"><span data-stu-id="84e4c-146">Resources have the module folder placed in the archive folder.</span></span> 

<span data-ttu-id="84e4c-147">`Set-AzureVMDscExtension`Vloží nastavení vyžadovaná rozšíření DSC Powershellu do objekt konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="84e4c-147">`Set-AzureVMDscExtension` injects the settings needed by the PowerShell DSC extension into a VM configuration object.</span></span> <span data-ttu-id="84e4c-148">V modelu nasazení classic, změny virtuálního počítače je nutné použít na virtuální počítač Azure s `Update-AzureVM`.</span><span class="sxs-lookup"><span data-stu-id="84e4c-148">In the classic deployment model, the VM changes must be applied to an Azure VM with `Update-AzureVM`.</span></span> 

<span data-ttu-id="84e4c-149">`Get-AzureVMDscExtension`načte stav rozšíření DSC konkrétní virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="84e4c-149">`Get-AzureVMDscExtension` retrieves the DSC extension status of a particular VM.</span></span> 

<span data-ttu-id="84e4c-150">`Get-AzureVMDscExtensionStatus`načte stav konfigurace DSC vydaných obslužná rutina rozšíření DSC.</span><span class="sxs-lookup"><span data-stu-id="84e4c-150">`Get-AzureVMDscExtensionStatus` retrieves the status of the DSC configuration enacted by the DSC extension handler.</span></span> <span data-ttu-id="84e4c-151">Tuto akci lze provést na jeden virtuální počítač nebo skupinu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="84e4c-151">This action can be performed on a single VM, or group of VMs.</span></span>

<span data-ttu-id="84e4c-152">`Remove-AzureVMDscExtension`Obslužná rutina rozšíření odebere z daného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="84e4c-152">`Remove-AzureVMDscExtension` removes the extension handler from a given virtual machine.</span></span> <span data-ttu-id="84e4c-153">Tato rutina nemá **není** odebrat konfiguraci, WMF odinstalovat nebo změnit nastavení použitá na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="84e4c-153">This cmdlet does **not** remove the configuration, uninstall the WMF, or change the applied settings on the virtual machine.</span></span> <span data-ttu-id="84e4c-154">Odebere pouze obslužná rutina rozšíření.</span><span class="sxs-lookup"><span data-stu-id="84e4c-154">It only removes the extension handler.</span></span> 

<span data-ttu-id="84e4c-155">**Hlavní rozdíly ve rutiny ASM a Azure Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="84e4c-155">**Key differences in ASM and Azure Resource Manager cmdlets**</span></span>

* <span data-ttu-id="84e4c-156">Rutiny Azure Resource Manager jsou synchronní.</span><span class="sxs-lookup"><span data-stu-id="84e4c-156">Azure Resource Manager cmdlets are synchronous.</span></span> <span data-ttu-id="84e4c-157">ASM rutiny jsou asynchronní.</span><span class="sxs-lookup"><span data-stu-id="84e4c-157">ASM cmdlets are asynchronous.</span></span>
* <span data-ttu-id="84e4c-158">Název skupiny prostředků, VMName, ArchiveStorageAccountName, verzi a umístění jsou všechny požadované parametry ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="84e4c-158">ResourceGroupName, VMName, ArchiveStorageAccountName, Version, and Location are all required parameters in Azure Resource Manager.</span></span>
* <span data-ttu-id="84e4c-159">ArchiveResourceGroupName je nový volitelný parametr pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="84e4c-159">ArchiveResourceGroupName is a new optional parameter for Azure Resource Manager.</span></span> <span data-ttu-id="84e4c-160">Tento parametr můžete zadat, když váš účet úložiště patří do jiné skupině prostředků než ten, kde se má vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="84e4c-160">You can specify this parameter when your storage account belongs to a different resource group than the one where the virtual machine is created.</span></span>
* <span data-ttu-id="84e4c-161">ConfigurationArchive nazývá ArchiveBlobName ve službě Správce prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="84e4c-161">ConfigurationArchive is called ArchiveBlobName in Azure Resource Manager</span></span>
* <span data-ttu-id="84e4c-162">ContainerName nazývá ArchiveContainerName ve službě Správce prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="84e4c-162">ContainerName is called ArchiveContainerName in Azure Resource Manager</span></span>
* <span data-ttu-id="84e4c-163">StorageEndpointSuffix nazývá ArchiveStorageEndpointSuffix ve službě Správce prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="84e4c-163">StorageEndpointSuffix is called ArchiveStorageEndpointSuffix in Azure Resource Manager</span></span>
* <span data-ttu-id="84e4c-164">Přepínač automatických aktualizací byla přidána do Azure Resource Manager povolit automatické aktualizace rozšíření obslužné rutiny na nejnovější verzi jako a není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="84e4c-164">The AutoUpdate switch has been added to Azure Resource Manager to enable automatic updating of the extension handler to the latest version as and when it is available.</span></span> <span data-ttu-id="84e4c-165">Všimněte si, že tento parametr má by mohly mít restartování virtuálního počítače po vydání nové verze WMF.</span><span class="sxs-lookup"><span data-stu-id="84e4c-165">Note this parameter has the potential to cause reboots on the VM when a new version of the WMF is released.</span></span> 

## <a name="azure-portal-functionality"></a><span data-ttu-id="84e4c-166">Funkce Azure portálu</span><span class="sxs-lookup"><span data-stu-id="84e4c-166">Azure portal functionality</span></span>
<span data-ttu-id="84e4c-167">Přejděte do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="84e4c-167">Browse to a VM.</span></span> <span data-ttu-id="84e4c-168">V části Nastavení -> Obecné klikněte na tlačítko "Rozšíření".</span><span class="sxs-lookup"><span data-stu-id="84e4c-168">Under Settings -> General click "Extensions."</span></span> <span data-ttu-id="84e4c-169">Vytvoří se nové podokno.</span><span class="sxs-lookup"><span data-stu-id="84e4c-169">A new pane is created.</span></span> <span data-ttu-id="84e4c-170">Klikněte na tlačítko "Přidat" a vyberte DSC prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84e4c-170">Click "Add" and select PowerShell DSC.</span></span>

<span data-ttu-id="84e4c-171">Na portálu musí vstup.</span><span class="sxs-lookup"><span data-stu-id="84e4c-171">The portal needs input.</span></span>
<span data-ttu-id="84e4c-172">**Konfigurace moduly nebo skriptu**: Toto pole je povinné.</span><span class="sxs-lookup"><span data-stu-id="84e4c-172">**Configuration Modules or Script**: This field is mandatory.</span></span> <span data-ttu-id="84e4c-173">Vyžaduje souboru s příponou .ps1 obsahující konfigurační skript, nebo soubor ZIP s .ps1 konfigurační skript v kořenu a všechny závislé zdroje ve složkách modul v rámci ZIP.</span><span class="sxs-lookup"><span data-stu-id="84e4c-173">Requires a .ps1 file containing a configuration script, or a .zip file with a .ps1 configuration script at the root, and all dependent resources in module folders within the .zip.</span></span> <span data-ttu-id="84e4c-174">Může být vytvořen pomocí `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` rutiny, které jsou součástí sady SDK Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84e4c-174">It can be created with the `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` cmdlet included in the Azure PowerShell SDK.</span></span> <span data-ttu-id="84e4c-175">Soubor .zip nahraje do úložiště objektů blob uživatele zabezpečeny SAS token.</span><span class="sxs-lookup"><span data-stu-id="84e4c-175">The .zip file is uploaded into your user blob storage secured by a SAS token.</span></span> 

<span data-ttu-id="84e4c-176">**Soubor konfiguračních dat PSD1**: Toto pole je nepovinné.</span><span class="sxs-lookup"><span data-stu-id="84e4c-176">**Configuration Data PSD1 File**: This field is optional.</span></span> <span data-ttu-id="84e4c-177">Pokud vaše konfigurace vyžaduje datový soubor konfigurace v .psd1, použijte toto pole vyberte ho a odešlete ji do úložiště objektů blob uživatele, kde je zabezpečená službou SAS token.</span><span class="sxs-lookup"><span data-stu-id="84e4c-177">If your configuration requires a configuration data file in .psd1, use this field to select it and upload it to your user blob storage, where it is secured by a SAS token.</span></span> 

<span data-ttu-id="84e4c-178">**Modul kvalifikovaný název konfigurace**: soubory .ps1 může mít několik konfiguračních funkcí.</span><span class="sxs-lookup"><span data-stu-id="84e4c-178">**Module-Qualified Name of Configuration**: .ps1 files can have multiple configuration functions.</span></span> <span data-ttu-id="84e4c-179">Zadejte název skriptu konfigurace .ps1, za nímž následuje '\' a název funkce konfigurace.</span><span class="sxs-lookup"><span data-stu-id="84e4c-179">Enter the name of the configuration .ps1 script followed by a  '\' and the name of the configuration function.</span></span> <span data-ttu-id="84e4c-180">Například pokud váš skript .ps1 má název "configuration.ps1" a "IisInstall" je konfigurace, můžete zadáním:`configuration.ps1\IisInstall`</span><span class="sxs-lookup"><span data-stu-id="84e4c-180">For example, if your .ps1 script has the name "configuration.ps1", and the configuration is "IisInstall", you would enter: `configuration.ps1\IisInstall`</span></span>

<span data-ttu-id="84e4c-181">**Konfigurace argumentů**: Pokud konfigurace funkce přijímá argumenty, zadejte je sem ve formátu `argumentName1=value1,argumentName2=value2`.</span><span class="sxs-lookup"><span data-stu-id="84e4c-181">**Configuration Arguments**: If the configuration function takes arguments, enter them in here in the format `argumentName1=value1,argumentName2=value2`.</span></span> <span data-ttu-id="84e4c-182">Všimněte si, že tento formát je do jiného formátu než jak konfigurace argumenty jsou přijímány prostřednictvím rutin prostředí PowerShell nebo šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="84e4c-182">Note this format is a different format than how configuration arguments are accepted through PowerShell cmdlets or Resource Manager templates.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="84e4c-183">Začínáme</span><span class="sxs-lookup"><span data-stu-id="84e4c-183">Getting started</span></span>
<span data-ttu-id="84e4c-184">Rozšíření Azure DSC trvá v dokumentech konfigurace DSC a představuje je na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="84e4c-184">The Azure DSC extension takes in DSC configuration documents and enacts them on Azure VMs.</span></span> <span data-ttu-id="84e4c-185">Následuje jednoduchý příklad konfigurace.</span><span class="sxs-lookup"><span data-stu-id="84e4c-185">A simple example of a configuration follows.</span></span> <span data-ttu-id="84e4c-186">Uložte místně jako "IisInstall.ps1":</span><span class="sxs-lookup"><span data-stu-id="84e4c-186">Save it locally as "IisInstall.ps1":</span></span>

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

<span data-ttu-id="84e4c-187">Následující kroky umístěte skript IisInstall.ps1 na zadaný virtuální počítač, provedení konfigurace a zpětně hlásit stav.</span><span class="sxs-lookup"><span data-stu-id="84e4c-187">The following steps place the IisInstall.ps1 script on the specified VM, execute the configuration, and report back on status.</span></span>
###<a name="classic-model"></a><span data-ttu-id="84e4c-188">Klasického modelu</span><span class="sxs-lookup"><span data-stu-id="84e4c-188">Classic model</span></span>
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "IisInstall.ps1.zip" -StorageContext $storageContext -ConfigurationName "IisInstall" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```
###<a name="azure-resource-manager-model"></a><span data-ttu-id="84e4c-189">Azure modelu Resource Manager</span><span class="sxs-lookup"><span data-stu-id="84e4c-189">Azure Resource Manager model</span></span>

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish the configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVmDscExtension -Version 2.21 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"

```

## <a name="logging"></a><span data-ttu-id="84e4c-190">Protokolování</span><span class="sxs-lookup"><span data-stu-id="84e4c-190">Logging</span></span>
<span data-ttu-id="84e4c-191">Protokoly jsou umístěny ve:</span><span class="sxs-lookup"><span data-stu-id="84e4c-191">Logs are placed in:</span></span>

<span data-ttu-id="84e4c-192">C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[číslo verze]</span><span class="sxs-lookup"><span data-stu-id="84e4c-192">C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\[Version Number]</span></span>

## <a name="next-steps"></a><span data-ttu-id="84e4c-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="84e4c-193">Next steps</span></span>
<span data-ttu-id="84e4c-194">Další informace o DSC Powershellu [přejděte do centra dokumentace k prostředí PowerShell](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="84e4c-194">For more information about PowerShell DSC, [visit the PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

<span data-ttu-id="84e4c-195">Zkontrolujte [šablony Azure Resource Manageru pro rozšíření DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="84e4c-195">Examine the [Azure Resource Manager template for the DSC extension](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="84e4c-196">Najít další funkce, které můžete spravovat pomocí prostředí PowerShell DSC [procházet galerii prostředí PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) pro další prostředky DSC.</span><span class="sxs-lookup"><span data-stu-id="84e4c-196">To find additional functionality you can manage with PowerShell DSC, [browse the PowerShell gallery](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) for more DSC resources.</span></span>

<span data-ttu-id="84e4c-197">Podrobnosti o předávání citlivých parametry do konfigurace najdete v tématu [spravovat pověření bezpečně s obslužná rutina rozšíření DSC](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="84e4c-197">For details on passing sensitive parameters into configurations, see [Manage credentials securely with the DSC extension handler](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

