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
# <a name="use-powershell-tooenable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>Pomocí prostředí PowerShell tooenable Azure Diagnostics v virtuálního počítače se systémem Windows
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Azure Diagnostics je hello funkce v rámci Azure, která umožňuje hello shromažďování diagnostických dat na nasazené aplikace. Můžete použít hello diagnostiky rozšíření toocollect diagnostických dat jako protokoly aplikací a čítače výkonu z Azure virtuálního počítače (VM) se systémem Windows. Tento článek popisuje, jak toouse prostředí Windows PowerShell tooenable hello rozšíření diagnostiky pro virtuální počítač. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) pro hello součásti potřebné k tomuto článku.

## <a name="enable-hello-diagnostics-extension-if-you-use-hello-resource-manager-deployment-model"></a>Povolit rozšíření diagnostiky hello, pokud používáte model nasazení Resource Manager hello
Když vytvoříte virtuální počítač s Windows pomocí modelu nasazení Azure Resource Manager hello přidáním šablony Resource Manageru toohello konfigurace rozšíření hello můžete povolit rozšíření diagnostiky hello. V tématu [vytvoření virtuálního počítače s Windows pomocí monitorování a Diagnostika pomocí šablony Azure Resource Manager hello](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

tooenable hello rozšíření diagnostiky na existující virtuální počítač, který byl vytvořen pomocí modelu nasazení Resource Manager hello, můžete použít hello [Set-AzureRMVMDiagnosticsExtension](/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension) rutiny prostředí PowerShell, jak je uvedeno níže.

    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* je hello cesta toohello soubor, který obsahuje konfiguraci diagnostiky hello v XML, jak je popsáno v hello [ukázka](#sample-diagnostics-configuration) níže.  

Pokud určuje hello diagnostiky konfigurační soubor **StorageAccount** element s názvem účtu úložiště, pak hello *Set-AzureRMVMDiagnosticsExtension* skript automaticky nastaví hello rozšíření toosend diagnostických dat toothat účet úložiště diagnostiky. Pro tento toowork hello účet úložiště musí toobe v hello stejného předplatného jako hello virtuálních počítačů.

Pokud žádné **StorageAccount** byla zadaná v konfiguraci diagnostiky hello, pak je nutné toopass v hello *StorageAccountName* parametr toohello rutiny. Pokud hello *StorageAccountName* parametr zadaný, bude rutina hello vždycky používají účet úložiště hello, který je zadaný v parametru hello a není ten, který je zadán v souboru konfigurace diagnostiky hello hello.

Pokud účet úložiště je v jiném předplatném. z hello virtuální počítač, pak je nutné tooexplicitly diagnostiky hello předat hello *StorageAccountName* a *StorageAccountKey* rutiny toohello parametry. Hello *StorageAccountKey* parametr není potřeba při hello diagnostiky účet úložiště není v hello stejného předplatného, stejně jako hello rutiny můžete automaticky dotaz a nastavte hodnotu klíče hello při povolování rozšíření diagnostiky hello. Ale pokud hello účet úložiště diagnostiky je v jiném předplatném, pak hello rutiny nemusí být možné tooget hello klíč automaticky a musíte tooexplicitly zadejte klíč hello prostřednictvím hello *StorageAccountKey* parametr.  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Jakmile bude rozšíření diagnostiky hello je povoleno na virtuálním počítači, můžete získat aktuální nastavení hello pomocí hello [Get-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/get-azurermvmdiagnosticsextension) rutiny.

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

Hello rutina vrátí *PublicSettings*, který obsahuje konfiguraci diagnostiky hello. Existují dva typy konfigurace podporována, WadCfg a xmlCfg. WadCfg je konfigurace JSON a xmlCfg je konfigurační soubor XML ve formátu kódováním Base64. tooread hello XML, musíte toodecode ho.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

Hello [odebrat AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/remove-azurermvmdiagnosticsextension) rutiny lze použít tooremove rozšíření diagnostiky hello z hello virtuálních počítačů.  

## <a name="enable-hello-diagnostics-extension-if-you-use-hello-classic-deployment-model"></a>Povolit rozšíření diagnostiky hello, pokud používáte model nasazení classic hello
Můžete použít hello [Set-AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) tooenable rutiny rozšíření diagnostiky pro virtuální počítač, který vytvoříte pomocí modelu nasazení classic hello. Hello následující příklad ukazuje, jak toocreate nového virtuálního počítače pomocí modelu nasazení classic hello s rozšíření diagnostiky hello povolena.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

tooenable hello rozšíření diagnostiky na existující virtuální počítač, který byl vytvořen pomocí modelu nasazení classic hello, první použití hello [Get-AzureVM](/powershell/module/azure/get-azurevm) konfigurace virtuálního počítače hello tooget rutiny. Aktualizujte hello virtuálních počítačů konfigurace tooinclude hello rozšíření diagnostiky pomocí hello [Set-AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) rutiny. Nakonec použít toohello hello aktualizovat konfiguraci virtuálních počítačů s využitím [aktualizace-AzureVM](/powershell/module/azure/update-azurevm).

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Ukázková konfigurace diagnostiky
Dobrý den, které lze použít následující kód XML pro veřejné konfigurace diagnostiky hello s hello výše skripty. Tato ukázka konfigurace bude přenos různé výkonu čítače toohello účet úložiště diagnostiky, společně s chyby ze hello aplikací, zabezpečení a systémové kanály v protokolech událostí systému Windows hello a všechny chyby z hello diagnostiky protokoly infrastruktury.

Konfigurace Hello potřebuje toobe aktualizované tooinclude hello následující:

* Hello *resourceID* atribut hello **metriky** element musí toobe aktualizováno hello ID prostředku pro hello virtuálních počítačů.
  
  * Hello prostředků ID lze sestavit pomocí hello následující vzor: "/ subscriptions / {*ID odběru pro odběr hello s hello virtuálních počítačů*} /resourceGroups/ {*hello resourcegroup název hello virtuálních počítačů*} / providers/Microsoft.Compute/virtualMachines/ {*hello název virtuálního počítače*} ".
  * Například pokud hello ID odběru pro odběr hello hello virtuálního počítače se spuštěným systémem je **11111111-1111-1111-1111-111111111111**, je název skupiny prostředků hello pro skupinu prostředků hello **MyResourceGroup**, a hello název virtuálního počítače **MyWindowsVM**, pak hello hodnotu *resourceID* by:
    
      ```
      <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
      ```
  * Další informace o tom, jak metriky jsou generované na základě hello čítače výkonu a konfigurace metriky najdete v tématu [tabulky Azure Diagnostics metriky v úložišti](extensions-diagnostics-template.md#wadmetrics-tables-in-storage).
* Hello **StorageAccount** element musí toobe aktualizováno hello název účtu úložiště diagnostiky hello.
  
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

## <a name="next-steps"></a>Další kroky
* Další pokyny k používání funkce Azure Diagnostics hello a jiné problémy tootroubleshoot techniky najdete v tématu [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](../../cloud-services/cloud-services-dotnet-diagnostics.md).
* [Schéma konfigurace diagnostiky](https://msdn.microsoft.com/library/azure/mt634524.aspx) vysvětluje různé konfigurace XML možnosti pro rozšíření diagnostiky hello hello.

