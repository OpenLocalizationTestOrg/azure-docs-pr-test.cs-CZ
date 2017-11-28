---
title: Konfigurace aaaCompiling v Azure Automation DSC. | Microsoft Docs
description: "Tento článek popisuje, jak toocompile konfigurace konfigurace požadovaného stavu (DSC) automatizace Azure."
services: automation
documentationcenter: na
author: eslesar
manager: carmonm
ms.assetid: 49f20b31-4fa5-4712-b1c7-8f4409f1aecc
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 02/07/2017
ms.author: magoedte; eslesar
ms.openlocfilehash: b195311318a2d7431c4d6b29f4b9a5f3a0a0a9a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="compiling-configurations-in-azure-automation-dsc"></a><span data-ttu-id="3c805-103">Kompilování konfigurace v Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="3c805-103">Compiling configurations in Azure Automation DSC</span></span>

<span data-ttu-id="3c805-104">Zkompilujete konfigurace konfigurace požadovaného stavu (DSC) s Azure Automation dvěma způsoby: v hello portál Azure a pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3c805-104">You can compile Desired State Configuration (DSC) configurations in two ways with Azure Automation: in hello Azure portal, and with Windows PowerShell.</span></span> <span data-ttu-id="3c805-105">Hello následující tabulka vám pomůže určit, kdy toouse, jakou metodu na základě hello charakteristik jednotlivých:</span><span class="sxs-lookup"><span data-stu-id="3c805-105">hello following table will help you determine when toouse which method based on hello characteristics of each:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="3c805-106">portál Azure</span><span class="sxs-lookup"><span data-stu-id="3c805-106">Azure portal</span></span>

* <span data-ttu-id="3c805-107">Nejjednodušším způsobem s interaktivní uživatelské rozhraní</span><span class="sxs-lookup"><span data-stu-id="3c805-107">Simplest method with interactive user interface</span></span>
* <span data-ttu-id="3c805-108">Formulář tooprovide jednoduché parametr hodnoty</span><span class="sxs-lookup"><span data-stu-id="3c805-108">Form tooprovide simple parameter values</span></span>
* <span data-ttu-id="3c805-109">Snadno sledovat stav úlohy</span><span class="sxs-lookup"><span data-stu-id="3c805-109">Easily track job state</span></span>
* <span data-ttu-id="3c805-110">Přístup k ověření pomocí přihlášení Azure</span><span class="sxs-lookup"><span data-stu-id="3c805-110">Access authenticated with Azure logon</span></span>

### <a name="windows-powershell"></a><span data-ttu-id="3c805-111">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="3c805-111">Windows PowerShell</span></span>

* <span data-ttu-id="3c805-112">Volání z příkazového řádku pomocí rutin prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="3c805-112">Call from command line with Windows PowerShell cmdlets</span></span>
* <span data-ttu-id="3c805-113">Můžou být součástí automatizované řešení s více kroků</span><span class="sxs-lookup"><span data-stu-id="3c805-113">Can be included in automated solution with multiple steps</span></span>
* <span data-ttu-id="3c805-114">Zadejte parametr jednoduché a komplexní hodnoty</span><span class="sxs-lookup"><span data-stu-id="3c805-114">Provide simple and complex parameter values</span></span>
* <span data-ttu-id="3c805-115">Sledování stavu úlohy</span><span class="sxs-lookup"><span data-stu-id="3c805-115">Track job state</span></span>
* <span data-ttu-id="3c805-116">Klient vyžaduje toosupport rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="3c805-116">Client required toosupport PowerShell cmdlets</span></span>
* <span data-ttu-id="3c805-117">Předat ConfigurationData</span><span class="sxs-lookup"><span data-stu-id="3c805-117">Pass ConfigurationData</span></span>
* <span data-ttu-id="3c805-118">Zkompilování konfigurace, které používají pověření</span><span class="sxs-lookup"><span data-stu-id="3c805-118">Compile configurations that use credentials</span></span>

<span data-ttu-id="3c805-119">Jakmile jste se rozhodli metodě kompilace, můžete provést následující toostart kompilování hello příslušné postupy.</span><span class="sxs-lookup"><span data-stu-id="3c805-119">Once you have decided on a compilation method, you can follow hello respective procedures below toostart compiling.</span></span>

## <a name="compiling-a-dsc-configuration-with-hello-azure-portal"></a><span data-ttu-id="3c805-120">Kompilaci konfigurace DSC s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3c805-120">Compiling a DSC Configuration with hello Azure portal</span></span>

1. <span data-ttu-id="3c805-121">Z vašeho účtu Automation, klikněte na tlačítko **konfigurace DSC**.</span><span class="sxs-lookup"><span data-stu-id="3c805-121">From your Automation account, click **DSC Configurations**.</span></span>
2. <span data-ttu-id="3c805-122">Klikněte na možnost konfigurace tooopen její okno.</span><span class="sxs-lookup"><span data-stu-id="3c805-122">Click a configuration tooopen its blade.</span></span>
3. <span data-ttu-id="3c805-123">Klikněte na tlačítko **zkompilovat**.</span><span class="sxs-lookup"><span data-stu-id="3c805-123">Click **Compile**.</span></span>
4. <span data-ttu-id="3c805-124">Pokud konfigurace hello nemá žádné parametry, jestli se mají toocompile nebudete výzvami tooconfirm ho.</span><span class="sxs-lookup"><span data-stu-id="3c805-124">If hello configuration has no parameters, you will be prompted tooconfirm whether you want toocompile it.</span></span> <span data-ttu-id="3c805-125">Pokud konfigurace hello obsahuje parametry, hello **zkompilování konfigurace** , můžete zadat hodnoty parametrů, otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="3c805-125">If hello configuration has parameters, hello **Compile Configuration** blade will open so you can provide parameter values.</span></span> <span data-ttu-id="3c805-126">V tématu hello [ **základních parametrů** ](#basic-parameters) části níže další podrobnosti o parametrech.</span><span class="sxs-lookup"><span data-stu-id="3c805-126">See hello [**Basic Parameters**](#basic-parameters) section below for further details on parameters.</span></span>
5. <span data-ttu-id="3c805-127">Hello **úloha kompilace** se otevře okno, takže můžete sledovat stav úlohy kompilace hello a hello konfigurace uzlu (dokumenty konfigurace MOF) se nezdařila z důvodu toobe umístit do Azure Automation DSC pro vyžádání obsahu Server hello.</span><span class="sxs-lookup"><span data-stu-id="3c805-127">hello **Compilation Job** blade is opened so that you can track hello compilation job's status, and hello node configurations (MOF configuration documents) it caused toobe placed on hello Azure Automation DSC Pull Server.</span></span>

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a><span data-ttu-id="3c805-128">Kompilaci konfigurace DSC pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="3c805-128">Compiling a DSC Configuration with Windows PowerShell</span></span>

<span data-ttu-id="3c805-129">Můžete použít [ `Start-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) toostart kompilace pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3c805-129">You can use [`Start-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) toostart compiling with Windows PowerShell.</span></span> <span data-ttu-id="3c805-130">Hello následující vzorový kód spustí kompilaci konfigurace DSC názvem **SampleConfig**.</span><span class="sxs-lookup"><span data-stu-id="3c805-130">hello following sample code starts compilation of a DSC configuration called **SampleConfig**.</span></span>

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
```

<span data-ttu-id="3c805-131">`Start-AzureRmAutomationDscCompilationJob`Vrátí kompilace úlohy, které můžete použít tootrack její stav objektu.</span><span class="sxs-lookup"><span data-stu-id="3c805-131">`Start-AzureRmAutomationDscCompilationJob` returns a compilation job object that you can use tootrack its status.</span></span> <span data-ttu-id="3c805-132">Pak můžete použít tento objekt úlohy kompilace s [ `Get-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) toodetermine hello stav úlohy kompilace hello, a [ `Get-AzureRmAutomationDscCompilationJobOutput` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) tooview jeho datové proudy (výstup).</span><span class="sxs-lookup"><span data-stu-id="3c805-132">You can then use this compilation job object with [`Get-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) toodetermine hello status of hello compilation job, and [`Get-AzureRmAutomationDscCompilationJobOutput`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) tooview its streams (output).</span></span> <span data-ttu-id="3c805-133">Hello následující vzorový kód spustí kompilace hello **SampleConfig** konfigurace, čeká na dokončení a potom zobrazí jeho datové proudy.</span><span class="sxs-lookup"><span data-stu-id="3c805-133">hello following sample code starts compilation of hello **SampleConfig** configuration, waits until it has completed, and then displays its streams.</span></span>

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"

while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

## <a name="basic-parameters"></a><span data-ttu-id="3c805-134">Základní parametry</span><span class="sxs-lookup"><span data-stu-id="3c805-134">Basic Parameters</span></span>
<span data-ttu-id="3c805-135">Deklarace parametru v konfiguracích DSC, včetně typy parametrů a vlastnosti, funguje hello stejné jako v Azure Automation runbook.</span><span class="sxs-lookup"><span data-stu-id="3c805-135">Parameter declaration in DSC configurations, including parameter types and properties, works hello same as in Azure Automation runbooks.</span></span> <span data-ttu-id="3c805-136">V tématu [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md) toolearn více informací o parametry runbooku.</span><span class="sxs-lookup"><span data-stu-id="3c805-136">See [Starting a runbook in Azure Automation](automation-starting-a-runbook.md) toolearn more about runbook parameters.</span></span>

<span data-ttu-id="3c805-137">Hello následující příklad používá dva parametry s názvem **FeatureName** a **IsPresent**, toodetermine hello hodnoty vlastností v hello **ParametersExample.sample** uzlu Konfigurace vygenerovaných během kompilace.</span><span class="sxs-lookup"><span data-stu-id="3c805-137">hello following example uses two parameters called **FeatureName** and **IsPresent**, toodetermine hello values of properties in hello **ParametersExample.sample** node configuration, generated during compilation.</span></span>

```powershell
Configuration ParametersExample
{
    param(
        [Parameter(Mandatory=$true)]

        [string] $FeatureName,

        [Parameter(Mandatory=$true)]
        [boolean] $IsPresent
    )

    $EnsureString = "Present"
    if($IsPresent -eq $false)
    {
        $EnsureString = "Absent"
    }

    Node "sample"
    {
        WindowsFeature ($FeatureName + "Feature")
        {
            Ensure = $EnsureString
            Name = $FeatureName
        }
    }
}
```

<span data-ttu-id="3c805-138">Můžete zkompilovat konfigurace DSC, které používají základní parametry, na portálu Azure Automation DSC hello nebo s prostředím Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="3c805-138">You can compile DSC Configurations that use basic parameters in hello Azure Automation DSC portal, or with Azure PowerShell:</span></span>

### <a name="portal"></a><span data-ttu-id="3c805-139">Portál</span><span class="sxs-lookup"><span data-stu-id="3c805-139">Portal</span></span>

<span data-ttu-id="3c805-140">Hello portálu, můžete zadat hodnoty parametrů po kliknutí na **zkompilovat**.</span><span class="sxs-lookup"><span data-stu-id="3c805-140">In hello portal, you can enter parameter values after clicking **Compile**.</span></span>

![alternativní text](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a><span data-ttu-id="3c805-142">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3c805-142">PowerShell</span></span>

<span data-ttu-id="3c805-143">Vyžaduje parametry v prostředí PowerShell [zatřiďovací tabulky](http://technet.microsoft.com/library/hh847780.aspx) hello klíč odpovídá názvu parametru hello, kde hello hodnota se rovná hodnotě parametru hello.</span><span class="sxs-lookup"><span data-stu-id="3c805-143">PowerShell requires parameters in a [hashtable](http://technet.microsoft.com/library/hh847780.aspx) where hello key matches hello parameter name, and hello value equals hello parameter value.</span></span>

```powershell
$Parameters = @{
    "FeatureName" = "Web-Server"
    "IsPresent" = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters
```

<span data-ttu-id="3c805-144">Informace o předávání PSCredentials jako parametry najdete v tématu <a href="#credential-assets"> **prostředků přihlašovacích údajů** </a> níže.</span><span class="sxs-lookup"><span data-stu-id="3c805-144">For information about passing PSCredentials as parameters, see <a href="#credential-assets">**Credential Assets**</a> below.</span></span>

## <a name="configurationdata"></a><span data-ttu-id="3c805-145">ConfigurationData</span><span class="sxs-lookup"><span data-stu-id="3c805-145">ConfigurationData</span></span>
<span data-ttu-id="3c805-146">**ConfigurationData** vám umožní tooseparate strukturální konfiguraci z žádnou konkrétní konfiguraci prostředí při používání DSC prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3c805-146">**ConfigurationData** allows you tooseparate structural configuration from any environment specific configuration while using PowerShell DSC.</span></span> <span data-ttu-id="3c805-147">V tématu [oddělení "Co" z "Kde" v prostředí PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) toolearn více informací o **ConfigurationData**.</span><span class="sxs-lookup"><span data-stu-id="3c805-147">See [Separating "What" from "Where" in PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) toolearn more about **ConfigurationData**.</span></span>

> [!NOTE]
> <span data-ttu-id="3c805-148">Můžete použít **ConfigurationData** při kompilaci v Azure Automation DSC pomocí Azure PowerShell, ale není v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3c805-148">You can use **ConfigurationData** when compiling in Azure Automation DSC using Azure PowerShell, but not in hello Azure portal.</span></span>

<span data-ttu-id="3c805-149">Hello následující příklad DSC konfigurace používá **ConfigurationData** prostřednictvím hello **$ConfigurationData** a **$AllNodes** klíčová slova.</span><span class="sxs-lookup"><span data-stu-id="3c805-149">hello following example DSC configuration uses **ConfigurationData** via hello **$ConfigurationData** and **$AllNodes** keywords.</span></span> <span data-ttu-id="3c805-150">Budete také potřebovat hello [ **xWebAdministration** modulu](https://www.powershellgallery.com/packages/xWebAdministration/) v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="3c805-150">You'll also need hello [**xWebAdministration** module](https://www.powershellgallery.com/packages/xWebAdministration/) for this example:</span></span>

```powershell
Configuration ConfigurationDataSample
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    Write-Verbose $ConfigurationData.NonNodeData.SomeMessage

    Node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
    {
        xWebsite Site
        {
            Name = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure   = "Present"
        }
    }
}
```

<span data-ttu-id="3c805-151">Konfigurace DSC hello výše v prostředí PowerShell můžete zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="3c805-151">You can compile hello DSC configuration above with PowerShell.</span></span> <span data-ttu-id="3c805-152">přidá dvě toohello konfigurace uzlu Azure Automation DSC pro vyžádání obsahu Server Hello níže prostředí PowerShell: **ConfigurationDataSample.MyVM1** a **ConfigurationDataSample.MyVM3**:</span><span class="sxs-lookup"><span data-stu-id="3c805-152">hello below PowerShell adds two node configurations toohello Azure Automation DSC Pull Server: **ConfigurationDataSample.MyVM1** and **ConfigurationDataSample.MyVM3**:</span></span>

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = "MyVM1"
            Role = "WebServer"
        },
        @{
            NodeName = "MyVM2"
            Role = "SQLServer"
        },
        @{
            NodeName = "MyVM3"
            Role = "WebServer"
        }
    )

    NonNodeData = @{
        SomeMessage = "I love Azure Automation DSC!"
    }
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ConfigurationDataSample" -ConfigurationData $ConfigData
```

## <a name="assets"></a><span data-ttu-id="3c805-153">Prostředky</span><span class="sxs-lookup"><span data-stu-id="3c805-153">Assets</span></span>

<span data-ttu-id="3c805-154">Asset odkazy jsou hello stejné v konfiguracích DSC Azure Automation a sady runbook.</span><span class="sxs-lookup"><span data-stu-id="3c805-154">Asset references are hello same in Azure Automation DSC configurations and runbooks.</span></span> <span data-ttu-id="3c805-155">V tématu hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="3c805-155">See hello following for more information:</span></span>

* [<span data-ttu-id="3c805-156">Certifikáty</span><span class="sxs-lookup"><span data-stu-id="3c805-156">Certificates</span></span>](automation-certificates.md)
* [<span data-ttu-id="3c805-157">Připojení</span><span class="sxs-lookup"><span data-stu-id="3c805-157">Connections</span></span>](automation-connections.md)
* [<span data-ttu-id="3c805-158">Přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="3c805-158">Credentials</span></span>](automation-credentials.md)
* [<span data-ttu-id="3c805-159">Proměnné</span><span class="sxs-lookup"><span data-stu-id="3c805-159">Variables</span></span>](automation-variables.md)

### <a name="credential-assets"></a><span data-ttu-id="3c805-160">Prostředků přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="3c805-160">Credential Assets</span></span>

<span data-ttu-id="3c805-161">Během konfigurace DSC ve službě Azure Automation, můžete odkazovat pomocí prostředků přihlašovacích údajů **Get-AzureRmAutomationCredential**, prostředků přihlašovacích údajů můžete také být předán prostřednictvím parametrů, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="3c805-161">While DSC configurations in Azure Automation can reference credential assets using **Get-AzureRmAutomationCredential**, credential assets can also be passed in via parameters, if desired.</span></span> <span data-ttu-id="3c805-162">Pokud konfigurace přebírá parametr **PSCredential** typ, pak jako hodnotu tohoto parametru, nikoli objekt PSCredential, potřebujete název řetězce hello toopass prostředek přihlašovacích údajů automatizace Azure.</span><span class="sxs-lookup"><span data-stu-id="3c805-162">If a configuration takes a parameter of **PSCredential** type, then you need toopass hello string name of an Azure Automation credential asset as that parameter’s value, rather than a PSCredential object.</span></span> <span data-ttu-id="3c805-163">Pozadí hello bude načíst a předán toohello konfigurace asset přihlašovacích údajů Azure Automation hello s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="3c805-163">Behind hello scenes, hello Azure Automation credential asset with that name will be retrieved and passed toohello configuration.</span></span>

<span data-ttu-id="3c805-164">Zachování pověření zabezpečení v uzlu Konfigurace (configuration dokumenty MOF) vyžaduje šifrování hello pověření v souboru MOF konfigurace uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="3c805-164">Keeping credentials secure in node configurations (MOF configuration documents) requires encrypting hello credentials in hello node configuration MOF file.</span></span> <span data-ttu-id="3c805-165">Služby Azure Automation trvá tento krok jeden další a šifruje hello celý soubor MOF.</span><span class="sxs-lookup"><span data-stu-id="3c805-165">Azure Automation takes this one step further and encrypts hello entire MOF file.</span></span> <span data-ttu-id="3c805-166">Ale právě se musí zjistit DSC prostředí PowerShell je to v pořádku pro toobe přihlašovací údaje, které jsou výstupem ve formátu prostého textu v průběhu generování MOF konfigurace uzlu, protože prostředí PowerShell DSC neví, že Azure Automation bude šifrování hello celý soubor MOF po jeho generování prostřednictvím úlohu kompilace.</span><span class="sxs-lookup"><span data-stu-id="3c805-166">However, currently you must tell PowerShell DSC it is okay for credentials toobe outputted in plain text during node configuration MOF generation, because PowerShell DSC doesn’t know that Azure Automation will be encrypting hello entire MOF file after its generation via a compilation job.</span></span>

<span data-ttu-id="3c805-167">Se dá zjistit DSC Powershellu, je to v pořádku pro toobe přihlašovací údaje, které jsou výstupem ve formátu prostého textu v konfiguraci uzlu hello generované soubory MOF pomocí [ **ConfigurationData**](#configurationdata).</span><span class="sxs-lookup"><span data-stu-id="3c805-167">You can tell PowerShell DSC that it is okay for credentials toobe outputted in plain text in hello generated node configuration MOFs using [**ConfigurationData**](#configurationdata).</span></span> <span data-ttu-id="3c805-168">By měla předávat `PSDscAllowPlainTextPassword = $true` prostřednictvím **ConfigurationData** pro každý uzel blok název, který se zobrazí v konfiguraci hello DSC a použije přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="3c805-168">You should pass `PSDscAllowPlainTextPassword = $true` via **ConfigurationData** for each node block’s name that appears in hello DSC configuration and uses credentials.</span></span>

<span data-ttu-id="3c805-169">Hello následující příklad ukazuje konfigurace DSC, který používá prostředek přihlašovacích údajů automatizace.</span><span class="sxs-lookup"><span data-stu-id="3c805-169">hello following example shows a DSC configuration that uses an Automation credential asset.</span></span>

```powershell
Configuration CredentialSample
{
    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -AutomationAccountName "AutomationAcct" -Name "SomeCredentialAsset"

    Node $AllNodes.NodeName
    {
        File ExampleFile
        {
            SourcePath = "\\Server\share\path\file.ext"
            DestinationPath = "C:\destinationPath"
            Credential = $Cred
        }
    }
}
```

<span data-ttu-id="3c805-170">Konfigurace DSC hello výše v prostředí PowerShell můžete zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="3c805-170">You can compile hello DSC configuration above with PowerShell.</span></span> <span data-ttu-id="3c805-171">přidá dvě toohello konfigurace uzlu Azure Automation DSC pro vyžádání obsahu Server Hello níže prostředí PowerShell: **CredentialSample.MyVM1** a **CredentialSample.MyVM2**.</span><span class="sxs-lookup"><span data-stu-id="3c805-171">hello below PowerShell adds two node configurations toohello Azure Automation DSC Pull Server:  **CredentialSample.MyVM1** and **CredentialSample.MyVM2**.</span></span>

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = "*"
            PSDscAllowPlainTextPassword = $True
        },
        @{
            NodeName = "MyVM1"
        },
        @{
            NodeName = "MyVM2"
        }
    )
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "CredentialSample" -ConfigurationData $ConfigData
```

## <a name="importing-node-configurations"></a><span data-ttu-id="3c805-172">Import konfigurace uzlu</span><span class="sxs-lookup"><span data-stu-id="3c805-172">Importing node configurations</span></span>

<span data-ttu-id="3c805-173">Můžete také importovat configuratons uzlu (soubory MOF), které zkompilujete mimo Azure.</span><span class="sxs-lookup"><span data-stu-id="3c805-173">You can also import node configuratons (MOFs) that you have compiled outside of Azure.</span></span> <span data-ttu-id="3c805-174">Jednou z výhod to je, že tento uzel confiturations může být podepsané.</span><span class="sxs-lookup"><span data-stu-id="3c805-174">One advantage of this is that node confiturations can be signed.</span></span>
<span data-ttu-id="3c805-175">Konfigurace podepsaný uzlu je ověřit místně v uzlu spravovaného agentem hello DSC, zajistíte, že tato konfigurace hello se uzel použité toohello pochází z autorizovaná zdrojová.</span><span class="sxs-lookup"><span data-stu-id="3c805-175">A signed node configuration is verified locally on a managed node by hello DSC agent, ensuring that hello configuration being applied toohello node comes from an authorized source.</span></span>

> [!NOTE]
> <span data-ttu-id="3c805-176">Můžete importovat podepsané konfigurace do účtu Azure Automation, ale automatizace Azure aktuálně nepodporuje kompilování podepsaný konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3c805-176">You can use import signed configurations into your Azure Automation account, but Azure Automation does not currently support compiling signed configurations.</span></span>

> [!NOTE]
> <span data-ttu-id="3c805-177">Soubor konfigurace uzlu musí být větší než 1 MB tooallow ho toobe importovat do Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="3c805-177">A node configuration file must be no larger than 1 MB tooallow it toobe imported into Azure Automation.</span></span>

<span data-ttu-id="3c805-178">Dozvíte, jak konfigurace uzlu toosign v https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.</span><span class="sxs-lookup"><span data-stu-id="3c805-178">You can learn how toosign node configurations at https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.</span></span>

### <a name="importing-a-node-configuration-in-hello-azure-portal"></a><span data-ttu-id="3c805-179">Import konfigurace uzlu v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3c805-179">Importing a node configuration in hello Azure portal</span></span>

1. <span data-ttu-id="3c805-180">Z vašeho účtu Automation, klikněte na tlačítko **konfigurace uzlu DSC**.</span><span class="sxs-lookup"><span data-stu-id="3c805-180">From your Automation account, click **DSC node configurations**.</span></span>

    ![Konfigurace uzlu DSC](./media/automation-dsc-compile/node-config.png)
2. <span data-ttu-id="3c805-182">V hello **konfigurace uzlu DSC** okně klikněte na tlačítko **přidat NodeConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="3c805-182">In hello **DSC node configurations** blade, click **Add a NodeConfiguration**.</span></span>
3. <span data-ttu-id="3c805-183">V hello **Import** okně klikněte na tlačítko Další toohello ikonu hello složky **uzlu konfigurační soubor** toobrowse textové pole pro uzel konfigurační soubor (MOF) v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="3c805-183">In hello **Import** blade, click hello folder icon next toohello **Node Configuration File** textbox toobrowse for a node configuration file (MOF) on your local computer.</span></span>

    ![Procházením vyhledejte místního souboru](./media/automation-dsc-compile/import-browse.png)
4. <span data-ttu-id="3c805-185">Zadejte název v hello **název konfigurace** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3c805-185">Enter a name in hello **Configuration Name** textbox.</span></span> <span data-ttu-id="3c805-186">Tento název musí odpovídat názvu hello hello konfigurace, ze kterého byl kompilován hello konfigurace uzlu.</span><span class="sxs-lookup"><span data-stu-id="3c805-186">This name must match hello name of hello configuration from which hello node configuration was compiled.</span></span>
5. <span data-ttu-id="3c805-187">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3c805-187">Click **OK**.</span></span>

### <a name="importing-a-node-configuration-with-powershell"></a><span data-ttu-id="3c805-188">Import konfigurace uzlu pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="3c805-188">Importing a node configuration with PowerShell</span></span>

<span data-ttu-id="3c805-189">Můžete použít hello [Import AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) tooimport rutiny konfigurace uzlu do vašeho účtu automation.</span><span class="sxs-lookup"><span data-stu-id="3c805-189">You can use hello [Import-AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) cmdlet tooimport a node configuration into your automation account.</span></span>

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName "MyAutomationAccount" -ResourceGroupName "MyResourceGroup" -ConfigurationName "MyNodeConfiguration" -Path "C:\MyConfigurations\TestVM1.mof"
```



