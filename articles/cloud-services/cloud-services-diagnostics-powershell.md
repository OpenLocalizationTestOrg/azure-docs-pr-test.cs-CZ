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
# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a><span data-ttu-id="0818f-103">Povolte diagnostiku ve službě Azure Cloud Services pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="0818f-103">Enable diagnostics in Azure Cloud Services using PowerShell</span></span>
<span data-ttu-id="0818f-104">Můžete shromáždit diagnostických dat, jako jsou protokoly aplikací, čítačích výkonu atd. z cloudové služby pomocí hello rozšíření Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="0818f-104">You can collect diagnostic data like application logs, performance counters etc. from a Cloud Service using hello Azure Diagnostics extension.</span></span> <span data-ttu-id="0818f-105">Tento článek popisuje, jak tooenable hello rozšíření Azure Diagnostics pro cloudové služby pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0818f-105">This article describes how tooenable hello Azure Diagnostics extension for a Cloud Service using PowerShell.</span></span>  <span data-ttu-id="0818f-106">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) pro hello součásti potřebné k tomuto článku.</span><span class="sxs-lookup"><span data-stu-id="0818f-106">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for hello prerequisites needed for this article.</span></span>

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a><span data-ttu-id="0818f-107">Povolit rozšíření diagnostiky jako součást nasazení cloudové služby</span><span class="sxs-lookup"><span data-stu-id="0818f-107">Enable diagnostics extension as part of deploying a Cloud Service</span></span>
<span data-ttu-id="0818f-108">Tento přístup je použít toocontinuous integrace typ scénáře, kde hello hello diagnostiky, rozšíření můžete povolit jako součást nasazení cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="0818f-108">This approach is applicable toocontinuous integration type of scenarios, where hello diagnostics extension can be enabled as part of deploying hello cloud service.</span></span> <span data-ttu-id="0818f-109">Při vytváření nového nasazení cloudové služby můžete povolit rozšíření diagnostiky hello předávání hello *ExtensionConfiguration* parametr toohello [New-AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) rutiny.</span><span class="sxs-lookup"><span data-stu-id="0818f-109">When creating a new Cloud Service deployment you can enable hello diagnostics extension by passing in hello *ExtensionConfiguration* parameter toohello [New-AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) cmdlet.</span></span> <span data-ttu-id="0818f-110">Hello *ExtensionConfiguration* parametr přijímá pole Konfigurace diagnostiky, které lze vytvořit pomocí hello [New-AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) rutiny.</span><span class="sxs-lookup"><span data-stu-id="0818f-110">hello *ExtensionConfiguration* parameter takes an array of diagnostics configurations that can be created using hello [New-AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) cmdlet.</span></span>

<span data-ttu-id="0818f-111">Hello následující příklad ukazuje, jak můžete povolit diagnostiku pro cloudové služby, WebRole a WorkerRole, každý s jinou diagnostiky konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0818f-111">hello following example shows how you can enable diagnostics for a cloud service with a WebRole and WorkerRole, each having a different diagnostics configuration.</span></span>

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

<span data-ttu-id="0818f-112">Pokud určuje hello diagnostiky konfigurační soubor `StorageAccount` element s názvem účtu úložiště, pak hello `New-AzureServiceDiagnosticsExtensionConfig` rutiny budou automaticky používat tento účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="0818f-112">If hello diagnostics configuration file specifies a `StorageAccount` element with a storage account name, then hello `New-AzureServiceDiagnosticsExtensionConfig` cmdlet will automatically use that storage account.</span></span> <span data-ttu-id="0818f-113">Pro tento toowork hello účet úložiště musí toobe v hello stejného předplatného jako hello nasazení cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="0818f-113">For this toowork, hello storage account needs toobe in hello same subscription as hello Cloud Service being deployed.</span></span>

<span data-ttu-id="0818f-114">Z Azure SDK 2.6 dalšího hello rozšíření konfigurační soubory generované hello MSBuild publikování cílový výstup bude obsahovat název účtu úložiště hello podle hello diagnostiky konfigurace řetězec zadaný v konfiguračním souboru služby hello (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="0818f-114">From Azure SDK 2.6 onward hello extension configuration files generated by hello MSBuild publish target output will include hello storage account name based on hello diagnostics configuration string specified in hello service configuration file (.cscfg).</span></span> <span data-ttu-id="0818f-115">skript Hello níže ukazuje, jak tooparse hello konfigurační soubory s příponou z hello výstup cíl publikování a konfiguraci rozšíření diagnostiky pro každou roli při nasazování hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="0818f-115">hello script below shows you how tooparse hello Extension configuration files from hello publish target output and configure diagnostics extension for each role when deploying hello cloud service.</span></span>

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

<span data-ttu-id="0818f-116">Visual Studio Online používá podobný postup pro automatické nasazení cloudových služeb s rozšíření diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="0818f-116">Visual Studio Online uses a similar approach for automated deployments of Cloud Services with hello diagnostics extension.</span></span> <span data-ttu-id="0818f-117">V tématu [publikovat AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) kompletní příklad.</span><span class="sxs-lookup"><span data-stu-id="0818f-117">See [Publish-AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) for a complete example.</span></span>

<span data-ttu-id="0818f-118">Pokud žádné `StorageAccount` byla zadaná v konfiguraci diagnostiky hello, pak je nutné toopass v hello *StorageAccountName* parametr toohello rutiny.</span><span class="sxs-lookup"><span data-stu-id="0818f-118">If no `StorageAccount` was specified in hello diagnostics configuration, then you need toopass in hello *StorageAccountName* parameter toohello cmdlet.</span></span> <span data-ttu-id="0818f-119">Pokud hello *StorageAccountName* parametr zadaný, bude rutina hello vždycky používají účet úložiště hello, který je zadaný v parametru hello a není ten, který je zadán v souboru konfigurace diagnostiky hello hello.</span><span class="sxs-lookup"><span data-stu-id="0818f-119">If hello *StorageAccountName* parameter is specified, then hello cmdlet will always use hello storage account that is specified in hello parameter and not hello one that is specified in hello diagnostics configuration file.</span></span>

<span data-ttu-id="0818f-120">Pokud účet úložiště je v jiném předplatném. z hello Cloudová služba, pak je nutné tooexplicitly diagnostiky hello předat hello *StorageAccountName* a *StorageAccountKey* parametry toohello rutiny.</span><span class="sxs-lookup"><span data-stu-id="0818f-120">If hello diagnostics storage account is in a different subscription from hello Cloud Service, then you need tooexplicitly pass in hello *StorageAccountName* and *StorageAccountKey* parameters toohello cmdlet.</span></span> <span data-ttu-id="0818f-121">Hello *StorageAccountKey* parametr není potřeba při hello diagnostiky účet úložiště není v hello stejného předplatného, stejně jako hello rutiny můžete automaticky dotaz a nastavte hodnotu klíče hello při povolování rozšíření diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="0818f-121">hello *StorageAccountKey* parameter is not needed when hello diagnostics storage account is in hello same subscription, as hello cmdlet can automatically query and set hello key value when enabling hello diagnostics extension.</span></span> <span data-ttu-id="0818f-122">Ale pokud hello účet úložiště diagnostiky je v jiném předplatném, pak hello rutiny nemusí být možné tooget hello klíč automaticky a musíte tooexplicitly zadejte klíč hello prostřednictvím hello *StorageAccountKey* parametr.</span><span class="sxs-lookup"><span data-stu-id="0818f-122">However, if hello diagnostics storage account is in a different subscription, then hello cmdlet might not be able tooget hello key automatically and you need tooexplicitly specify hello key through hello *StorageAccountKey* parameter.</span></span>

```powershell
$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
```

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a><span data-ttu-id="0818f-123">Povolit rozšíření diagnostiky na existující službu Cloud</span><span class="sxs-lookup"><span data-stu-id="0818f-123">Enable diagnostics extension on an existing Cloud Service</span></span>
<span data-ttu-id="0818f-124">Můžete použít hello [Set-AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) rutiny tooenable nebo aktualizaci konfigurace diagnostiky na Cloudová služba, která je již spuštěna.</span><span class="sxs-lookup"><span data-stu-id="0818f-124">You can use hello [Set-AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet tooenable or update diagnostics configuration on a Cloud Service that is already running.</span></span>

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

```powershell
$service_name = "MyService"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name
```

## <a name="get-current-diagnostics-extension-configuration"></a><span data-ttu-id="0818f-125">Získejte aktuální konfiguraci rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="0818f-125">Get current diagnostics extension configuration</span></span>
<span data-ttu-id="0818f-126">Použití hello [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) rutiny tooget hello aktuální konfigurace diagnostiky pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="0818f-126">Use hello [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet tooget hello current diagnostics configuration for a cloud service.</span></span>

```powershell
Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

## <a name="remove-diagnostics-extension"></a><span data-ttu-id="0818f-127">Odeberte rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="0818f-127">Remove diagnostics extension</span></span>
<span data-ttu-id="0818f-128">Služba tooturn vypnout diagnostiky v cloudu, můžete použít hello [odebrat AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) rutiny.</span><span class="sxs-lookup"><span data-stu-id="0818f-128">tooturn off diagnostics on a cloud service you can use hello [Remove-AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet.</span></span>

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

<span data-ttu-id="0818f-129">Pokud jste povolili rozšíření diagnostiky hello pomocí *Set-AzureServiceDiagnosticsExtension* nebo hello *New-AzureServiceDiagnosticsExtensionConfig* bez hello *Role* parametr pak je můžete odebrat pomocí rozšíření hello *odebrat AzureServiceDiagnosticsExtension* bez hello *Role* parametr.</span><span class="sxs-lookup"><span data-stu-id="0818f-129">If you enabled hello diagnostics extension using either *Set-AzureServiceDiagnosticsExtension* or hello *New-AzureServiceDiagnosticsExtensionConfig* without hello *Role* parameter then you can remove hello extension using *Remove-AzureServiceDiagnosticsExtension* without hello *Role* parameter.</span></span> <span data-ttu-id="0818f-130">Pokud hello *Role* parametr byla použita při povolení rozšíření hello mělo musí být rovněž použit při odebírání rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="0818f-130">If hello *Role* parameter was used when enabling hello extension then it must also be used when removing hello extension.</span></span>

<span data-ttu-id="0818f-131">rozšíření diagnostiky tooremove hello každou jednotlivou roli:</span><span class="sxs-lookup"><span data-stu-id="0818f-131">tooremove hello diagnostics extension from each individual role:</span></span>

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```

## <a name="next-steps"></a><span data-ttu-id="0818f-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0818f-132">Next Steps</span></span>
* <span data-ttu-id="0818f-133">Další informace pomocí diagnostiky Azure a jiné problémy tootroubleshoot techniky, najdete v části [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="0818f-133">For additional guidance on using Azure diagnostics and other techniques tootroubleshoot problems, see [Enabling Diagnostics in Azure Cloud Services and Virtual Machines](cloud-services-dotnet-diagnostics.md).</span></span>
* <span data-ttu-id="0818f-134">Hello [schéma konfigurace diagnostiky](https://msdn.microsoft.com/library/azure/dn782207.aspx) vysvětluje různé konfigurace xml možnosti pro rozšíření diagnostiky hello hello.</span><span class="sxs-lookup"><span data-stu-id="0818f-134">hello [Diagnostics Configuration Schema](https://msdn.microsoft.com/library/azure/dn782207.aspx) explains hello various xml configurations options for hello diagnostics extension.</span></span>
* <span data-ttu-id="0818f-135">jak tooenable hello rozšíření diagnostiky pro virtuální počítače, najdete v části toolearn [vytvořit Windows virtuální počítač s monitorování a Diagnostika pomocí šablony Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md)</span><span class="sxs-lookup"><span data-stu-id="0818f-135">toolearn how tooenable hello diagnostics extension for Virtual Machines, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md)</span></span>
