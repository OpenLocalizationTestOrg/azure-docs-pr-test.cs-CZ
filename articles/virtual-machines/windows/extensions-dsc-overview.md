---
title: "aaaDesired stavu konfigurace pro přehled Azure | Microsoft Docs"
description: "Přehled pro používání hello Microsoft Azure rozšíření obslužné rutiny pro konfigurace požadovaného stavu prostředí PowerShell. Včetně požadavky, architektura, rutiny..."
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
ms.openlocfilehash: b0337a2f1124f35e5e40c1478bd7530427e59d44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-azure-desired-state-configuration-extension-handler"></a><span data-ttu-id="c0ab9-104">Obslužná rutina rozšíření Úvod toohello konfigurace požadovaného stavu Azure</span><span class="sxs-lookup"><span data-stu-id="c0ab9-104">Introduction toohello Azure Desired State Configuration extension handler</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="c0ab9-105">Hello agenta virtuálního počítače Azure a související rozšíření jsou součástí hello infrastrukturní služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-105">hello Azure VM Agent and associated Extensions are part of hello Microsoft Azure Infrastructure Services.</span></span> <span data-ttu-id="c0ab9-106">Rozšíření virtuálního počítače jsou softwarové komponenty, které rozšiřovat funkce virtuálního počítače hello a zjednodušit různé operace správy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-106">VM Extensions are software components that extend hello VM functionality and simplify various VM management operations.</span></span> <span data-ttu-id="c0ab9-107">Například může být použité tooreset hesla správce hello rozšíření VMAccess, nebo hello vlastní skript rozšíření lze použít tooexecute skript na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-107">For example, hello VMAccess extension can be used tooreset an administrator's password, or hello Custom Script extension can be used tooexecute a script on hello VM.</span></span>

<span data-ttu-id="c0ab9-108">Tento článek představuje hello rozšíření prostředí PowerShell požadovaného stavu konfigurace (DSC) pro virtuální počítače Azure v rámci hello Azure PowerShell SDK.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-108">This article introduces hello PowerShell Desired State Configuration (DSC) Extension for Azure VMs as part of hello Azure PowerShell SDK.</span></span> <span data-ttu-id="c0ab9-109">Můžete použít nové rutiny tooupload a použití konfigurace DSC prostředí PowerShell ve virtuálním počítači Azure s hello rozšíření DSC prostředí PowerShell povolené.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-109">You can use new cmdlets tooupload and apply a PowerShell DSC configuration on an Azure VM enabled with hello PowerShell DSC extension.</span></span> <span data-ttu-id="c0ab9-110">volání rozšíření DSC Powershellu Hello do prostředí PowerShell DSC tooenact hello přijal konfiguraci DSC na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-110">hello PowerShell DSC extension calls into PowerShell DSC tooenact hello received DSC configuration on hello VM.</span></span> <span data-ttu-id="c0ab9-111">Tato funkce je také k dispozici prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-111">This functionality is also available through hello Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c0ab9-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c0ab9-112">Prerequisites</span></span>
<span data-ttu-id="c0ab9-113">**Místní počítač** toointeract s hello rozšíření virtuálního počítače Azure, třeba toouse buď hello portál Azure nebo Azure PowerShell SDK hello.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-113">**Local machine** toointeract with hello Azure VM extension, you need toouse either hello Azure portal or hello Azure PowerShell SDK.</span></span> 

<span data-ttu-id="c0ab9-114">**Agent hosta** hello virtuálního počítače Azure, který je nakonfigurovaný hello DSC konfigurace musí toobe operační systém, který podporuje buď Windows Management Framework (WMF) 4.0 nebo 5.0.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-114">**Guest Agent** hello Azure VM that is configured by hello DSC configuration needs toobe an OS that supports either Windows Management Framework (WMF) 4.0 or 5.0.</span></span> <span data-ttu-id="c0ab9-115">Hello úplný seznam podporovaných verzí operačního systému najdete na hello [historie verzí rozšíření DSC](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).</span><span class="sxs-lookup"><span data-stu-id="c0ab9-115">hello full list of supported OS versions can be found at hello [DSC Extension Version History](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).</span></span>

## <a name="terms-and-concepts"></a><span data-ttu-id="c0ab9-116">Podmínky a koncepty</span><span class="sxs-lookup"><span data-stu-id="c0ab9-116">Terms and concepts</span></span>
<span data-ttu-id="c0ab9-117">Tato příručka předpokládá znalost hello následující koncepty:</span><span class="sxs-lookup"><span data-stu-id="c0ab9-117">This guide presumes familiarity with hello following concepts:</span></span>

<span data-ttu-id="c0ab9-118">Konfigurace – dokumentu konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-118">Configuration - A DSC configuration document.</span></span> 

<span data-ttu-id="c0ab9-119">Uzel - cíle pro konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-119">Node - A target for a DSC configuration.</span></span> <span data-ttu-id="c0ab9-120">V tomto dokumentu odkazuje "uzel" vždy tooan virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-120">In this document, "node" always refers tooan Azure VM.</span></span>

<span data-ttu-id="c0ab9-121">Konfigurační Data - .psd1 soubor obsahující data prostředí pro konfiguraci</span><span class="sxs-lookup"><span data-stu-id="c0ab9-121">Configuration Data - A .psd1 file containing environmental data for a configuration</span></span>

## <a name="architectural-overview"></a><span data-ttu-id="c0ab9-122">Přehled architektury</span><span class="sxs-lookup"><span data-stu-id="c0ab9-122">Architectural overview</span></span>
<span data-ttu-id="c0ab9-123">Hello rozšíření Azure DSC používá toodeliver framework hello agenta virtuálního počítače Azure, uplatní a ohlásí o konfiguracích DSC běžící na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-123">hello Azure DSC extension uses hello Azure VM Agent framework toodeliver, enact, and report on DSC configurations running on Azure VMs.</span></span> <span data-ttu-id="c0ab9-124">Hello rozšíření DSC očekává soubor ZIP obsahující alespoň dokumentu konfigurace a sadu parametrů poskytuje prostřednictvím hello Azure PowerShell SDK nebo prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-124">hello DSC extension expects a .zip file containing at least a configuration document, and a set of parameters provided either through hello Azure PowerShell SDK or through hello Azure portal.</span></span>

<span data-ttu-id="c0ab9-125">Při rozšíření hello volání pro hello poprvé, spustí instalační proces.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-125">When hello extension is called for hello first time, it runs an installation process.</span></span> <span data-ttu-id="c0ab9-126">Tento proces nainstaluje verzi hello Windows Management Framework (WMF) pomocí následujících logiku hello:</span><span class="sxs-lookup"><span data-stu-id="c0ab9-126">This process installs a version of hello Windows Management Framework (WMF) using hello following logic:</span></span>

1. <span data-ttu-id="c0ab9-127">Pokud hello operačním systémem virtuálního počítače Azure se systémem Windows Server 2016, nebyla provedena žádná akce.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-127">If hello Azure VM OS is Windows Server 2016, no action is taken.</span></span> <span data-ttu-id="c0ab9-128">Windows Server 2016 již hello nejnovější verzi prostředí PowerShell nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-128">Windows Server 2016 already has hello latest version of PowerShell installed.</span></span>
2. <span data-ttu-id="c0ab9-129">Pokud hello `wmfVersion` je zadána vlastnost, je nainstalovaná této verze hello WMF, pokud není kompatibilní s operačním systémem hello Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-129">If hello `wmfVersion` property is specified, that version of hello WMF is installed unless it is incompatible with hello VM's OS.</span></span>
3. <span data-ttu-id="c0ab9-130">Pokud žádné `wmfVersion` vlastnost určena, hello nejnovější použít hello WMF je nainstalovaná verze.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-130">If no `wmfVersion` property is specified, hello latest applicable version of hello WMF is installed.</span></span>

<span data-ttu-id="c0ab9-131">Instalace hello WMF vyžaduje restart.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-131">Installation of hello WMF requires a reboot.</span></span> <span data-ttu-id="c0ab9-132">Po restartování hello rozšíření stáhne soubor ZIP hello zadaný v hello `modulesUrl` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-132">After reboot, hello extension downloads hello .zip file specified in hello `modulesUrl` property.</span></span> <span data-ttu-id="c0ab9-133">Pokud toto umístění je v Azure blob storage, SAS token lze zadat v hello `sasToken` vlastnost tooaccess hello souboru.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-133">If this location is in Azure blob storage, a SAS token can be specified in hello `sasToken` property tooaccess hello file.</span></span> <span data-ttu-id="c0ab9-134">Po stažení a vybaleno hello .zip hello konfigurace funkci definovanou v `configurationFunction` spuštění souboru MOF toogenerate hello.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-134">After hello .zip is downloaded and unpacked, hello configuration function defined in `configurationFunction` is run toogenerate hello MOF file.</span></span> <span data-ttu-id="c0ab9-135">pak spustí Hello rozšíření `Start-DscConfiguration -Force` v souboru MOF hello vygenerovat.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-135">hello extension then runs `Start-DscConfiguration -Force` on hello generated MOF file.</span></span> <span data-ttu-id="c0ab9-136">rozšíření Hello zaznamená výstup a zapíše zpět na toohello kanál stavu Azure.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-136">hello extension captures output and writes it back out toohello Azure Status Channel.</span></span> <span data-ttu-id="c0ab9-137">Z tohoto bodu na hello DSC LCM zpracovává monitorování a oprava jako normální.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-137">From this point on, hello DSC LCM handles monitoring and correction as normal.</span></span> 

## <a name="powershell-cmdlets"></a><span data-ttu-id="c0ab9-138">Rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c0ab9-138">PowerShell cmdlets</span></span>
<span data-ttu-id="c0ab9-139">Rutiny prostředí PowerShell je možné pomocí Azure Resource Manageru nebo hello toopackage modelu nasazení classic, publikovat a monitorování nasazení rozšíření DSC.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-139">PowerShell cmdlets can be used with Azure Resource Manager or hello classic deployment model toopackage, publish, and monitor DSC extension deployments.</span></span> <span data-ttu-id="c0ab9-140">Hello jsou následující rutiny uvedené moduly hello nasazení classic, ale "Azure" lze nahradit pomocí modelu "AzureRm" toouse hello Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-140">hello following cmdlets listed are hello classic deployment modules, but "Azure" can be replaced with "AzureRm" toouse hello Azure Resource Manager model.</span></span> <span data-ttu-id="c0ab9-141">Například `Publish-AzureVMDscConfiguration` používá hello modelu nasazení classic, kde `Publish-AzureRmVMDscConfiguration` používá Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-141">For example,  `Publish-AzureVMDscConfiguration` uses hello classic deployment model, where `Publish-AzureRmVMDscConfiguration` uses Azure Resource Manager.</span></span> 

<span data-ttu-id="c0ab9-142">`Publish-AzureVMDscConfiguration`trvá v konfiguračním souboru, hledá závislé prostředky DSC a vytvoří soubor ZIP obsahující hello konfigurace a konfigurace DSC prostředky potřebné tooenact hello.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-142">`Publish-AzureVMDscConfiguration` takes in a configuration file, scans it for dependent DSC resources, and creates a .zip file containing hello configuration and DSC resources needed tooenact hello configuration.</span></span> <span data-ttu-id="c0ab9-143">Můžete také vytvořit balíček hello místně pomocí hello `-ConfigurationArchivePath` parametr.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-143">It can also create hello package locally using hello `-ConfigurationArchivePath` parameter.</span></span> <span data-ttu-id="c0ab9-144">V opačném případě publikuje úložiště objektů blob tooAzure soubor .zip hello a zabezpečuje s tokenem SAS.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-144">Otherwise, it publishes hello .zip file tooAzure blob storage and secures it with a SAS token.</span></span>

<span data-ttu-id="c0ab9-145">soubor ZIP Hello vytvořených touto rutinou má hello .ps1 konfigurační skript v hello kořenové složky archivu hello.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-145">hello .zip file created by this cmdlet has hello .ps1 configuration script at hello root of hello archive folder.</span></span> <span data-ttu-id="c0ab9-146">Prostředky mít složku modulu hello umístěny ve složce archivu hello.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-146">Resources have hello module folder placed in hello archive folder.</span></span> 

<span data-ttu-id="c0ab9-147">`Set-AzureVMDscExtension`Vloží hello nastavení vyžadovaná hello rozšíření DSC prostředí PowerShell do objekt konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-147">`Set-AzureVMDscExtension` injects hello settings needed by hello PowerShell DSC extension into a VM configuration object.</span></span> <span data-ttu-id="c0ab9-148">V modelu nasazení classic hello hello změny virtuálního počítače musí být použité tooan virtuální počítač Azure s `Update-AzureVM`.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-148">In hello classic deployment model, hello VM changes must be applied tooan Azure VM with `Update-AzureVM`.</span></span> 

<span data-ttu-id="c0ab9-149">`Get-AzureVMDscExtension`načte stav rozšíření DSC hello konkrétní virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-149">`Get-AzureVMDscExtension` retrieves hello DSC extension status of a particular VM.</span></span> 

<span data-ttu-id="c0ab9-150">`Get-AzureVMDscExtensionStatus`načte hello stav konfigurace DSC hello vydat hello DSC rozšíření obslužnou rutinou.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-150">`Get-AzureVMDscExtensionStatus` retrieves hello status of hello DSC configuration enacted by hello DSC extension handler.</span></span> <span data-ttu-id="c0ab9-151">Tuto akci lze provést na jeden virtuální počítač nebo skupinu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-151">This action can be performed on a single VM, or group of VMs.</span></span>

<span data-ttu-id="c0ab9-152">`Remove-AzureVMDscExtension`Odebere obslužnou rutinu rozšíření hello z daného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-152">`Remove-AzureVMDscExtension` removes hello extension handler from a given virtual machine.</span></span> <span data-ttu-id="c0ab9-153">Tato rutina nemá **není** odebrat konfiguraci hello, odinstalovat hello WMF nebo změnit nastavení hello použita na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-153">This cmdlet does **not** remove hello configuration, uninstall hello WMF, or change hello applied settings on hello virtual machine.</span></span> <span data-ttu-id="c0ab9-154">Odebere pouze hello rozšíření obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-154">It only removes hello extension handler.</span></span> 

<span data-ttu-id="c0ab9-155">**Hlavní rozdíly ve rutiny ASM a Azure Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="c0ab9-155">**Key differences in ASM and Azure Resource Manager cmdlets**</span></span>

* <span data-ttu-id="c0ab9-156">Rutiny Azure Resource Manager jsou synchronní.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-156">Azure Resource Manager cmdlets are synchronous.</span></span> <span data-ttu-id="c0ab9-157">ASM rutiny jsou asynchronní.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-157">ASM cmdlets are asynchronous.</span></span>
* <span data-ttu-id="c0ab9-158">Název skupiny prostředků, VMName, ArchiveStorageAccountName, verzi a umístění jsou všechny požadované parametry ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-158">ResourceGroupName, VMName, ArchiveStorageAccountName, Version, and Location are all required parameters in Azure Resource Manager.</span></span>
* <span data-ttu-id="c0ab9-159">ArchiveResourceGroupName je nový volitelný parametr pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-159">ArchiveResourceGroupName is a new optional parameter for Azure Resource Manager.</span></span> <span data-ttu-id="c0ab9-160">Tento parametr můžete zadat, když váš účet úložiště patří tooa jiné skupině prostředků než hello jeden, kde se má vytvořit hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-160">You can specify this parameter when your storage account belongs tooa different resource group than hello one where hello virtual machine is created.</span></span>
* <span data-ttu-id="c0ab9-161">ConfigurationArchive nazývá ArchiveBlobName ve službě Správce prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="c0ab9-161">ConfigurationArchive is called ArchiveBlobName in Azure Resource Manager</span></span>
* <span data-ttu-id="c0ab9-162">ContainerName nazývá ArchiveContainerName ve službě Správce prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="c0ab9-162">ContainerName is called ArchiveContainerName in Azure Resource Manager</span></span>
* <span data-ttu-id="c0ab9-163">StorageEndpointSuffix nazývá ArchiveStorageEndpointSuffix ve službě Správce prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="c0ab9-163">StorageEndpointSuffix is called ArchiveStorageEndpointSuffix in Azure Resource Manager</span></span>
* <span data-ttu-id="c0ab9-164">přepínač Hello automatických aktualizací byla přidána tooAzure Resource Manager tooenable automatické aktualizace hello obslužná rutina toohello nejnovější verze rozšíření jako a v případě, že je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-164">hello AutoUpdate switch has been added tooAzure Resource Manager tooenable automatic updating of hello extension handler toohello latest version as and when it is available.</span></span> <span data-ttu-id="c0ab9-165">Všimněte si, že tento parametr má hello potenciální toocause restartování na hello virtuální počítač při hello WMF vydání nové verze.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-165">Note this parameter has hello potential toocause reboots on hello VM when a new version of hello WMF is released.</span></span> 

## <a name="azure-portal-functionality"></a><span data-ttu-id="c0ab9-166">Funkce Azure portálu</span><span class="sxs-lookup"><span data-stu-id="c0ab9-166">Azure portal functionality</span></span>
<span data-ttu-id="c0ab9-167">Procházejte tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-167">Browse tooa VM.</span></span> <span data-ttu-id="c0ab9-168">V části Nastavení -> Obecné klikněte na tlačítko "Rozšíření".</span><span class="sxs-lookup"><span data-stu-id="c0ab9-168">Under Settings -> General click "Extensions."</span></span> <span data-ttu-id="c0ab9-169">Vytvoří se nové podokno.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-169">A new pane is created.</span></span> <span data-ttu-id="c0ab9-170">Klikněte na tlačítko "Přidat" a vyberte DSC prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-170">Click "Add" and select PowerShell DSC.</span></span>

<span data-ttu-id="c0ab9-171">portál Hello vyžaduje vstup.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-171">hello portal needs input.</span></span>
<span data-ttu-id="c0ab9-172">**Konfigurace moduly nebo skriptu**: Toto pole je povinné.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-172">**Configuration Modules or Script**: This field is mandatory.</span></span> <span data-ttu-id="c0ab9-173">Vyžaduje souboru s příponou .ps1 obsahující konfigurační skript, nebo soubor ZIP s konfigurační skript .ps1 hello kořenové a všechny závislé zdroje ve složkách modul v rámci hello .zip.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-173">Requires a .ps1 file containing a configuration script, or a .zip file with a .ps1 configuration script at hello root, and all dependent resources in module folders within hello .zip.</span></span> <span data-ttu-id="c0ab9-174">Vytvořením s hello `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` rutiny, které jsou součástí hello Azure PowerShell SDK.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-174">It can be created with hello `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` cmdlet included in hello Azure PowerShell SDK.</span></span> <span data-ttu-id="c0ab9-175">soubor ZIP Hello nahraje do úložiště objektů blob uživatele zabezpečeny SAS token.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-175">hello .zip file is uploaded into your user blob storage secured by a SAS token.</span></span> 

<span data-ttu-id="c0ab9-176">**Soubor konfiguračních dat PSD1**: Toto pole je nepovinné.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-176">**Configuration Data PSD1 File**: This field is optional.</span></span> <span data-ttu-id="c0ab9-177">Pokud vaše konfigurace vyžaduje datový soubor konfigurace v .psd1, použijte toto pole tooselect ho a nahrajte ho tooyour úložiště objektů blob uživatele, kde je zabezpečená službou SAS token.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-177">If your configuration requires a configuration data file in .psd1, use this field tooselect it and upload it tooyour user blob storage, where it is secured by a SAS token.</span></span> 

<span data-ttu-id="c0ab9-178">**Modul kvalifikovaný název konfigurace**: soubory .ps1 může mít několik konfiguračních funkcí.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-178">**Module-Qualified Name of Configuration**: .ps1 files can have multiple configuration functions.</span></span> <span data-ttu-id="c0ab9-179">Zadejte název hello hello konfigurace .ps1 skriptu následuje '\' a hello název funkce Konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-179">Enter hello name of hello configuration .ps1 script followed by a  '\' and hello name of hello configuration function.</span></span> <span data-ttu-id="c0ab9-180">Například pokud má váš skript .ps1 hello název "configuration.ps1" a konfigurace hello je "IisInstall", můžete zadáním:`configuration.ps1\IisInstall`</span><span class="sxs-lookup"><span data-stu-id="c0ab9-180">For example, if your .ps1 script has hello name "configuration.ps1", and hello configuration is "IisInstall", you would enter: `configuration.ps1\IisInstall`</span></span>

<span data-ttu-id="c0ab9-181">**Konfigurace argumentů**: Pokud hello konfigurace funkce přijímá argumenty, zadejte je sem ve formátu hello `argumentName1=value1,argumentName2=value2`.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-181">**Configuration Arguments**: If hello configuration function takes arguments, enter them in here in hello format `argumentName1=value1,argumentName2=value2`.</span></span> <span data-ttu-id="c0ab9-182">Všimněte si, že tento formát je do jiného formátu než jak konfigurace argumenty jsou přijímány prostřednictvím rutin prostředí PowerShell nebo šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-182">Note this format is a different format than how configuration arguments are accepted through PowerShell cmdlets or Resource Manager templates.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="c0ab9-183">Začínáme</span><span class="sxs-lookup"><span data-stu-id="c0ab9-183">Getting started</span></span>
<span data-ttu-id="c0ab9-184">Hello rozšíření Azure DSC trvá v dokumentech konfigurace DSC a představuje je na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-184">hello Azure DSC extension takes in DSC configuration documents and enacts them on Azure VMs.</span></span> <span data-ttu-id="c0ab9-185">Následuje jednoduchý příklad konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-185">A simple example of a configuration follows.</span></span> <span data-ttu-id="c0ab9-186">Uložte místně jako "IisInstall.ps1":</span><span class="sxs-lookup"><span data-stu-id="c0ab9-186">Save it locally as "IisInstall.ps1":</span></span>

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

<span data-ttu-id="c0ab9-187">Následující kroky místní hello IisInstall.ps1 skriptu na hello Hello zadaný virtuální počítač, provedení konfigurace hello a zpětně hlásit stav.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-187">hello following steps place hello IisInstall.ps1 script on hello specified VM, execute hello configuration, and report back on status.</span></span>
###<a name="classic-model"></a><span data-ttu-id="c0ab9-188">Klasického modelu</span><span class="sxs-lookup"><span data-stu-id="c0ab9-188">Classic model</span></span>
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish hello configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set hello VM toorun hello DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "IisInstall.ps1.zip" -StorageContext $storageContext -ConfigurationName "IisInstall" -Verbose

#Update hello configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```
###<a name="azure-resource-manager-model"></a><span data-ttu-id="c0ab9-189">Azure modelu Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c0ab9-189">Azure Resource Manager model</span></span>

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish hello configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set hello VM toorun hello DSC configuration
Set-AzureRmVmDscExtension -Version 2.21 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"

```

## <a name="logging"></a><span data-ttu-id="c0ab9-190">Protokolování</span><span class="sxs-lookup"><span data-stu-id="c0ab9-190">Logging</span></span>
<span data-ttu-id="c0ab9-191">Protokoly jsou umístěny ve:</span><span class="sxs-lookup"><span data-stu-id="c0ab9-191">Logs are placed in:</span></span>

<span data-ttu-id="c0ab9-192">C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[číslo verze]</span><span class="sxs-lookup"><span data-stu-id="c0ab9-192">C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\[Version Number]</span></span>

## <a name="next-steps"></a><span data-ttu-id="c0ab9-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c0ab9-193">Next steps</span></span>
<span data-ttu-id="c0ab9-194">Další informace o DSC Powershellu [navštivte centru dokumentace prostředí PowerShell hello](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="c0ab9-194">For more information about PowerShell DSC, [visit hello PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

<span data-ttu-id="c0ab9-195">Zkontrolujte hello [šablony Azure Resource Manageru pro rozšíření hello DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c0ab9-195">Examine hello [Azure Resource Manager template for hello DSC extension](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="c0ab9-196">toofind další funkce, které můžete spravovat pomocí prostředí PowerShell DSC [procházet galerii prostředí PowerShell hello](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) pro další prostředky DSC.</span><span class="sxs-lookup"><span data-stu-id="c0ab9-196">toofind additional functionality you can manage with PowerShell DSC, [browse hello PowerShell gallery](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) for more DSC resources.</span></span>

<span data-ttu-id="c0ab9-197">Podrobnosti o předávání citlivých parametry do konfigurace najdete v tématu [spravovat pověření bezpečně s obslužnou rutinou rozšíření hello DSC](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c0ab9-197">For details on passing sensitive parameters into configurations, see [Manage credentials securely with hello DSC extension handler](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

