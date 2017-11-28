---
title: "aaaUse diagnostiky tooenable prostředí Azure PowerShell na virtuální počítač s Windows | Microsoft Docs"
services: virtual-machines-windows
documentationcenter: 
description: "Zjistěte, jak toouse prostředí PowerShell tooenable Azure Diagnostics ve virtuálním počítači s Windows"
author: sbtron
manager: timlt
editor: 
ms.assetid: 2e6d88f2-1980-4a24-827e-a81616a0d247
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: saurabh
ms.openlocfilehash: e945f0de154b5ba600f845f0d577b48e2254573b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooenable-azure-diagnostics-in-a-virtual-machine-running-windows"></a><span data-ttu-id="7b02d-103">Pomocí prostředí PowerShell tooenable Azure Diagnostics v virtuálního počítače se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="7b02d-103">Use PowerShell tooenable Azure Diagnostics in a virtual machine running Windows</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="7b02d-104">Azure Diagnostics je hello funkce v rámci Azure, která umožňuje hello shromažďování diagnostických dat na nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="7b02d-104">Azure Diagnostics is hello capability within Azure that enables hello collection of diagnostic data on a deployed application.</span></span> <span data-ttu-id="7b02d-105">Můžete použít hello diagnostiky rozšíření toocollect diagnostických dat jako protokoly aplikací a čítače výkonu z Azure virtuálního počítače (VM) se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="7b02d-105">You can use hello diagnostics extension toocollect diagnostic data like application logs or performance counters from an Azure virtual machine (VM) that is running Windows.</span></span> <span data-ttu-id="7b02d-106">Tento článek popisuje, jak toouse prostředí Windows PowerShell tooenable hello rozšíření diagnostiky pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="7b02d-106">This article describes how toouse Windows PowerShell tooenable hello diagnostics extension for a VM.</span></span> <span data-ttu-id="7b02d-107">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) pro hello součásti potřebné k tomuto článku.</span><span class="sxs-lookup"><span data-stu-id="7b02d-107">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for hello prerequisites needed for this article.</span></span>

## <a name="enable-hello-diagnostics-extension-if-you-use-hello-resource-manager-deployment-model"></a><span data-ttu-id="7b02d-108">Povolit rozšíření diagnostiky hello, pokud používáte model nasazení Resource Manager hello</span><span class="sxs-lookup"><span data-stu-id="7b02d-108">Enable hello diagnostics extension if you use hello Resource Manager deployment model</span></span>
<span data-ttu-id="7b02d-109">Když vytvoříte virtuální počítač s Windows pomocí modelu nasazení Azure Resource Manager hello přidáním šablony Resource Manageru toohello konfigurace rozšíření hello můžete povolit rozšíření diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="7b02d-109">You can enable hello diagnostics extension while you create a Windows VM through hello Azure Resource Manager deployment model by adding hello extension configuration toohello Resource Manager template.</span></span> <span data-ttu-id="7b02d-110">V tématu [vytvoření virtuálního počítače s Windows pomocí monitorování a Diagnostika pomocí šablony Azure Resource Manager hello](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7b02d-110">See [Create a Windows virtual machine with monitoring and diagnostics by using hello Azure Resource Manager template](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="7b02d-111">tooenable hello rozšíření diagnostiky na existující virtuální počítač, který byl vytvořen pomocí modelu nasazení Resource Manager hello, můžete použít hello [Set-AzureRMVMDiagnosticsExtension](/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension) rutiny prostředí PowerShell, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="7b02d-111">tooenable hello diagnostics extension on an existing VM that was created through hello Resource Manager deployment model, you can use hello [Set-AzureRMVMDiagnosticsExtension](/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension) PowerShell cmdlet as shown below.</span></span>

    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


<span data-ttu-id="7b02d-112">*$diagnosticsconfig_path* je hello cesta toohello soubor, který obsahuje konfiguraci diagnostiky hello v XML, jak je popsáno v hello [ukázka](#sample-diagnostics-configuration) níže.</span><span class="sxs-lookup"><span data-stu-id="7b02d-112">*$diagnosticsconfig_path* is hello path toohello file that contains hello diagnostics configuration in XML, as described in hello [sample](#sample-diagnostics-configuration) below.</span></span>  

<span data-ttu-id="7b02d-113">Pokud určuje hello diagnostiky konfigurační soubor **StorageAccount** element s názvem účtu úložiště, pak hello *Set-AzureRMVMDiagnosticsExtension* skript automaticky nastaví hello rozšíření toosend diagnostických dat toothat účet úložiště diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="7b02d-113">If hello diagnostics configuration file specifies a **StorageAccount** element with a storage account name, then hello *Set-AzureRMVMDiagnosticsExtension* script will automatically set hello diagnostics extension toosend diagnostic data toothat storage account.</span></span> <span data-ttu-id="7b02d-114">Pro tento toowork hello účet úložiště musí toobe v hello stejného předplatného jako hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7b02d-114">For this toowork, hello storage account needs toobe in hello same subscription as hello VM.</span></span>

<span data-ttu-id="7b02d-115">Pokud žádné **StorageAccount** byla zadaná v konfiguraci diagnostiky hello, pak je nutné toopass v hello *StorageAccountName* parametr toohello rutiny.</span><span class="sxs-lookup"><span data-stu-id="7b02d-115">If no **StorageAccount** was specified in hello diagnostics configuration, then you need toopass in hello *StorageAccountName* parameter toohello cmdlet.</span></span> <span data-ttu-id="7b02d-116">Pokud hello *StorageAccountName* parametr zadaný, bude rutina hello vždycky používají účet úložiště hello, který je zadaný v parametru hello a není ten, který je zadán v souboru konfigurace diagnostiky hello hello.</span><span class="sxs-lookup"><span data-stu-id="7b02d-116">If hello *StorageAccountName* parameter is specified, then hello cmdlet will always use hello storage account that is specified in hello parameter and not hello one that is specified in hello diagnostics configuration file.</span></span>

<span data-ttu-id="7b02d-117">Pokud účet úložiště je v jiném předplatném. z hello virtuální počítač, pak je nutné tooexplicitly diagnostiky hello předat hello *StorageAccountName* a *StorageAccountKey* rutiny toohello parametry.</span><span class="sxs-lookup"><span data-stu-id="7b02d-117">If hello diagnostics storage account is in a different subscription from hello VM, then you need tooexplicitly pass in hello *StorageAccountName* and *StorageAccountKey* parameters toohello cmdlet.</span></span> <span data-ttu-id="7b02d-118">Hello *StorageAccountKey* parametr není potřeba při hello diagnostiky účet úložiště není v hello stejného předplatného, stejně jako hello rutiny můžete automaticky dotaz a nastavte hodnotu klíče hello při povolování rozšíření diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="7b02d-118">hello *StorageAccountKey* parameter is not needed when hello diagnostics storage account is in hello same subscription, as hello cmdlet can automatically query and set hello key value when enabling hello diagnostics extension.</span></span> <span data-ttu-id="7b02d-119">Ale pokud hello účet úložiště diagnostiky je v jiném předplatném, pak hello rutiny nemusí být možné tooget hello klíč automaticky a musíte tooexplicitly zadejte klíč hello prostřednictvím hello *StorageAccountKey* parametr.</span><span class="sxs-lookup"><span data-stu-id="7b02d-119">However, if hello diagnostics storage account is in a different subscription, then hello cmdlet might not be able tooget hello key automatically and you need tooexplicitly specify hello key through hello *StorageAccountKey* parameter.</span></span>  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

<span data-ttu-id="7b02d-120">Jakmile bude rozšíření diagnostiky hello je povoleno na virtuálním počítači, můžete získat aktuální nastavení hello pomocí hello [Get-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/get-azurermvmdiagnosticsextension) rutiny.</span><span class="sxs-lookup"><span data-stu-id="7b02d-120">Once hello diagnostics extension is enabled on a VM, you can get hello current settings by using hello [Get-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/get-azurermvmdiagnosticsextension) cmdlet.</span></span>

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

<span data-ttu-id="7b02d-121">Hello rutina vrátí *PublicSettings*, který obsahuje konfiguraci diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="7b02d-121">hello cmdlet returns *PublicSettings*, which contains hello diagnostics configuration.</span></span> <span data-ttu-id="7b02d-122">Existují dva typy konfigurace podporována, WadCfg a xmlCfg.</span><span class="sxs-lookup"><span data-stu-id="7b02d-122">There are two kinds of configuration supported, WadCfg and xmlCfg.</span></span> <span data-ttu-id="7b02d-123">WadCfg je konfigurace JSON a xmlCfg je konfigurační soubor XML ve formátu kódováním Base64.</span><span class="sxs-lookup"><span data-stu-id="7b02d-123">WadCfg is JSON configuration, and xmlCfg is XML configuration in a Base64-encoded format.</span></span> <span data-ttu-id="7b02d-124">tooread hello XML, musíte toodecode ho.</span><span class="sxs-lookup"><span data-stu-id="7b02d-124">tooread hello XML, you need toodecode it.</span></span>

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

<span data-ttu-id="7b02d-125">Hello [odebrat AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/remove-azurermvmdiagnosticsextension) rutiny lze použít tooremove rozšíření diagnostiky hello z hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7b02d-125">hello [Remove-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/remove-azurermvmdiagnosticsextension) cmdlet can be used tooremove hello diagnostics extension from hello VM.</span></span>  

## <a name="enable-hello-diagnostics-extension-if-you-use-hello-classic-deployment-model"></a><span data-ttu-id="7b02d-126">Povolit rozšíření diagnostiky hello, pokud používáte model nasazení classic hello</span><span class="sxs-lookup"><span data-stu-id="7b02d-126">Enable hello diagnostics extension if you use hello classic deployment model</span></span>
<span data-ttu-id="7b02d-127">Můžete použít hello [Set-AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) tooenable rutiny rozšíření diagnostiky pro virtuální počítač, který vytvoříte pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="7b02d-127">You can use hello [Set-AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) cmdlet tooenable a diagnostics extension on a VM that you create through hello classic deployment model.</span></span> <span data-ttu-id="7b02d-128">Hello následující příklad ukazuje, jak toocreate nového virtuálního počítače pomocí modelu nasazení classic hello s rozšíření diagnostiky hello povolena.</span><span class="sxs-lookup"><span data-stu-id="7b02d-128">hello following example shows how toocreate a new VM through hello classic deployment model with hello diagnostics extension enabled.</span></span>

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

<span data-ttu-id="7b02d-129">tooenable hello rozšíření diagnostiky na existující virtuální počítač, který byl vytvořen pomocí modelu nasazení classic hello, první použití hello [Get-AzureVM](/powershell/module/azure/get-azurevm) konfigurace virtuálního počítače hello tooget rutiny.</span><span class="sxs-lookup"><span data-stu-id="7b02d-129">tooenable hello diagnostics extension on an existing VM that was created through hello classic deployment model, first use hello [Get-AzureVM](/powershell/module/azure/get-azurevm) cmdlet tooget hello VM configuration.</span></span> <span data-ttu-id="7b02d-130">Aktualizujte hello virtuálních počítačů konfigurace tooinclude hello rozšíření diagnostiky pomocí hello [Set-AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) rutiny.</span><span class="sxs-lookup"><span data-stu-id="7b02d-130">Then update hello VM configuration tooinclude hello diagnostics extension by using hello [Set-AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) cmdlet.</span></span> <span data-ttu-id="7b02d-131">Nakonec použít toohello hello aktualizovat konfiguraci virtuálních počítačů s využitím [aktualizace-AzureVM](/powershell/module/azure/update-azurevm).</span><span class="sxs-lookup"><span data-stu-id="7b02d-131">Finally, apply hello updated configuration toohello VM by using [Update-AzureVM](/powershell/module/azure/update-azurevm).</span></span>

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a><span data-ttu-id="7b02d-132">Ukázková konfigurace diagnostiky</span><span class="sxs-lookup"><span data-stu-id="7b02d-132">Sample diagnostics configuration</span></span>
<span data-ttu-id="7b02d-133">Dobrý den, které lze použít následující kód XML pro veřejné konfigurace diagnostiky hello s hello výše skripty.</span><span class="sxs-lookup"><span data-stu-id="7b02d-133">hello following XML can be used for hello diagnostics public configuration with hello above scripts.</span></span> <span data-ttu-id="7b02d-134">Tato ukázka konfigurace bude přenos různé výkonu čítače toohello účet úložiště diagnostiky, společně s chyby ze hello aplikací, zabezpečení a systémové kanály v protokolech událostí systému Windows hello a všechny chyby z hello diagnostiky protokoly infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="7b02d-134">This sample configuration will transfer various performance counters toohello diagnostics storage account, along with errors from hello application, security, and system channels in hello Windows event logs and any errors from hello diagnostics infrastructure logs.</span></span>

<span data-ttu-id="7b02d-135">Konfigurace Hello potřebuje toobe aktualizované tooinclude hello následující:</span><span class="sxs-lookup"><span data-stu-id="7b02d-135">hello configuration needs toobe updated tooinclude hello following:</span></span>

* <span data-ttu-id="7b02d-136">Hello *resourceID* atribut hello **metriky** element musí toobe aktualizováno hello ID prostředku pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7b02d-136">hello *resourceID* attribute of hello **Metrics** element needs toobe updated with hello resource ID for hello VM.</span></span>
  
  * <span data-ttu-id="7b02d-137">Hello prostředků ID lze sestavit pomocí hello následující vzor: "/ subscriptions / {*ID odběru pro odběr hello s hello virtuálních počítačů*} /resourceGroups/ {*hello resourcegroup název hello virtuálních počítačů*} / providers/Microsoft.Compute/virtualMachines/ {*hello název virtuálního počítače*} ".</span><span class="sxs-lookup"><span data-stu-id="7b02d-137">hello resource ID can be constructed by using hello following pattern: "/subscriptions/{*subscription ID for hello subscription with hello VM*}/resourceGroups/{*hello resourcegroup name for hello VM*}/providers/Microsoft.Compute/virtualMachines/{*hello VM Name*}".</span></span>
  * <span data-ttu-id="7b02d-138">Například pokud hello ID odběru pro odběr hello hello virtuálního počítače se spuštěným systémem je **11111111-1111-1111-1111-111111111111**, je název skupiny prostředků hello pro skupinu prostředků hello **MyResourceGroup**, a hello název virtuálního počítače **MyWindowsVM**, pak hello hodnotu *resourceID* by:</span><span class="sxs-lookup"><span data-stu-id="7b02d-138">For example, if hello subscription ID for hello subscription where hello VM is running is **11111111-1111-1111-1111-111111111111**, hello resource group name for hello resource group is **MyResourceGroup**, and hello VM Name is **MyWindowsVM**, then hello value for *resourceID* would be:</span></span>
    
      ```
      <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
      ```
  * <span data-ttu-id="7b02d-139">Další informace o tom, jak metriky jsou generované na základě hello čítače výkonu a konfigurace metriky najdete v tématu [tabulky Azure Diagnostics metriky v úložišti](extensions-diagnostics-template.md#wadmetrics-tables-in-storage).</span><span class="sxs-lookup"><span data-stu-id="7b02d-139">For more information on how metrics are generated based on hello performance counters and metrics configuration, see [Azure Diagnostics metrics table in storage](extensions-diagnostics-template.md#wadmetrics-tables-in-storage).</span></span>
* <span data-ttu-id="7b02d-140">Hello **StorageAccount** element musí toobe aktualizováno hello název účtu úložiště diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="7b02d-140">hello **StorageAccount** element needs toobe updated with hello name of hello diagnostics storage account.</span></span>
  
    ```
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for hello VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a><span data-ttu-id="7b02d-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7b02d-141">Next steps</span></span>
* <span data-ttu-id="7b02d-142">Další pokyny k používání funkce Azure Diagnostics hello a jiné problémy tootroubleshoot techniky najdete v tématu [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](../../cloud-services/cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="7b02d-142">For additional guidance on using hello Azure Diagnostics capability and other techniques tootroubleshoot problems, see [Enabling Diagnostics in Azure Cloud Services and Virtual Machines](../../cloud-services/cloud-services-dotnet-diagnostics.md).</span></span>
* <span data-ttu-id="7b02d-143">[Schéma konfigurace diagnostiky](https://msdn.microsoft.com/library/azure/mt634524.aspx) vysvětluje různé konfigurace XML možnosti pro rozšíření diagnostiky hello hello.</span><span class="sxs-lookup"><span data-stu-id="7b02d-143">[Diagnostics configurations schema](https://msdn.microsoft.com/library/azure/mt634524.aspx) explains hello various XML configurations options for hello diagnostics extension.</span></span>

