---
title: "Diagnostika aaaEnable v Azure Cloud Services pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak tooenable diagnostiky pro cloud services pomocí prostředí PowerShell"
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 66e08754-8639-4022-ae18-4237749ba17d
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/06/2016
ms.author: adegeo
ms.openlocfilehash: 7c7444df13edc8d7f5663e20ec7558d36aac45d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Povolte diagnostiku ve službě Azure Cloud Services pomocí prostředí PowerShell
Můžete shromáždit diagnostických dat, jako jsou protokoly aplikací, čítačích výkonu atd. z cloudové služby pomocí hello rozšíření Azure Diagnostics. Tento článek popisuje, jak tooenable hello rozšíření Azure Diagnostics pro cloudové služby pomocí prostředí PowerShell.  V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) pro hello součásti potřebné k tomuto článku.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Povolit rozšíření diagnostiky jako součást nasazení cloudové služby
Tento přístup je použít toocontinuous integrace typ scénáře, kde hello hello diagnostiky, rozšíření můžete povolit jako součást nasazení cloudové služby. Při vytváření nového nasazení cloudové služby můžete povolit rozšíření diagnostiky hello předávání hello *ExtensionConfiguration* parametr toohello [New-AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) rutiny. Hello *ExtensionConfiguration* parametr přijímá pole Konfigurace diagnostiky, které lze vytvořit pomocí hello [New-AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) rutiny.

Hello následující příklad ukazuje, jak můžete povolit diagnostiku pro cloudové služby, WebRole a WorkerRole, každý s jinou diagnostiky konfigurace.

```powershell
$service_name = "MyService"
$service_package = "CloudService.cspkg"
$service_config = "ServiceConfiguration.Cloud.cscfg"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)
```

Pokud určuje hello diagnostiky konfigurační soubor `StorageAccount` element s názvem účtu úložiště, pak hello `New-AzureServiceDiagnosticsExtensionConfig` rutiny budou automaticky používat tento účet úložiště. Pro tento toowork hello účet úložiště musí toobe v hello stejného předplatného jako hello nasazení cloudové služby.

Z Azure SDK 2.6 dalšího hello rozšíření konfigurační soubory generované hello MSBuild publikování cílový výstup bude obsahovat název účtu úložiště hello podle hello diagnostiky konfigurace řetězec zadaný v konfiguračním souboru služby hello (.cscfg). skript Hello níže ukazuje, jak tooparse hello konfigurační soubory s příponou z hello výstup cíl publikování a konfiguraci rozšíření diagnostiky pro každou roli při nasazování hello cloudové služby.

```powershell
$service_name = "MyService"
$service_package = "C:\build\output\CloudService.cspkg"
$service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

#Find hello Extensions path based on service configuration file
$extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

$diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
$diagnosticsConfigurations = @()
foreach ($extPath in $diagnosticsExtensions)
{
    #Find hello RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
    {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
    }
}
New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations
```

Visual Studio Online používá podobný postup pro automatické nasazení cloudových služeb s rozšíření diagnostiky hello. V tématu [publikovat AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) kompletní příklad.

Pokud žádné `StorageAccount` byla zadaná v konfiguraci diagnostiky hello, pak je nutné toopass v hello *StorageAccountName* parametr toohello rutiny. Pokud hello *StorageAccountName* parametr zadaný, bude rutina hello vždycky používají účet úložiště hello, který je zadaný v parametru hello a není ten, který je zadán v souboru konfigurace diagnostiky hello hello.

Pokud účet úložiště je v jiném předplatném. z hello Cloudová služba, pak je nutné tooexplicitly diagnostiky hello předat hello *StorageAccountName* a *StorageAccountKey* parametry toohello rutiny. Hello *StorageAccountKey* parametr není potřeba při hello diagnostiky účet úložiště není v hello stejného předplatného, stejně jako hello rutiny můžete automaticky dotaz a nastavte hodnotu klíče hello při povolování rozšíření diagnostiky hello. Ale pokud hello účet úložiště diagnostiky je v jiném předplatném, pak hello rutiny nemusí být možné tooget hello klíč automaticky a musíte tooexplicitly zadejte klíč hello prostřednictvím hello *StorageAccountKey* parametr.

```powershell
$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
```

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Povolit rozšíření diagnostiky na existující službu Cloud
Můžete použít hello [Set-AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) rutiny tooenable nebo aktualizaci konfigurace diagnostiky na Cloudová služba, která je již spuštěna.

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

```powershell
$service_name = "MyService"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name
```

## <a name="get-current-diagnostics-extension-configuration"></a>Získejte aktuální konfiguraci rozšíření diagnostiky
Použití hello [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) rutiny tooget hello aktuální konfigurace diagnostiky pro cloudové služby.

```powershell
Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

## <a name="remove-diagnostics-extension"></a>Odeberte rozšíření diagnostiky
Služba tooturn vypnout diagnostiky v cloudu, můžete použít hello [odebrat AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) rutiny.

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Pokud jste povolili rozšíření diagnostiky hello pomocí *Set-AzureServiceDiagnosticsExtension* nebo hello *New-AzureServiceDiagnosticsExtensionConfig* bez hello *Role* parametr pak je můžete odebrat pomocí rozšíření hello *odebrat AzureServiceDiagnosticsExtension* bez hello *Role* parametr. Pokud hello *Role* parametr byla použita při povolení rozšíření hello mělo musí být rovněž použit při odebírání rozšíření hello.

rozšíření diagnostiky tooremove hello každou jednotlivou roli:

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```

## <a name="next-steps"></a>Další kroky
* Další informace pomocí diagnostiky Azure a jiné problémy tootroubleshoot techniky, najdete v části [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](cloud-services-dotnet-diagnostics.md).
* Hello [schéma konfigurace diagnostiky](https://msdn.microsoft.com/library/azure/dn782207.aspx) vysvětluje různé konfigurace xml možnosti pro rozšíření diagnostiky hello hello.
* jak tooenable hello rozšíření diagnostiky pro virtuální počítače, najdete v části toolearn [vytvořit Windows virtuální počítač s monitorování a Diagnostika pomocí šablony Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md)
