---
title: "Kompilování konfigurace v Azure Automation DSC. | Microsoft Docs"
description: "Tento článek popisuje způsob kompilace konfigurace konfigurace požadovaného stavu (DSC) automatizace Azure."
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
ms.openlocfilehash: 1aadd604e676659475f00760af3b0bdfb13a4792
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="compiling-configurations-in-azure-automation-dsc"></a><span data-ttu-id="de460-103">Kompilování konfigurace v Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="de460-103">Compiling configurations in Azure Automation DSC</span></span>

<span data-ttu-id="de460-104">Zkompilujete konfigurace konfigurace požadovaného stavu (DSC) s Azure Automation dvěma způsoby: na portálu Azure a pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="de460-104">You can compile Desired State Configuration (DSC) configurations in two ways with Azure Automation: in the Azure portal, and with Windows PowerShell.</span></span> <span data-ttu-id="de460-105">Následující tabulka vám pomůže určit, kdy použít jakou metodu na základě charakteristik jednotlivých:</span><span class="sxs-lookup"><span data-stu-id="de460-105">The following table will help you determine when to use which method based on the characteristics of each:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="de460-106">portál Azure</span><span class="sxs-lookup"><span data-stu-id="de460-106">Azure portal</span></span>

* <span data-ttu-id="de460-107">Nejjednodušším způsobem s interaktivní uživatelské rozhraní</span><span class="sxs-lookup"><span data-stu-id="de460-107">Simplest method with interactive user interface</span></span>
* <span data-ttu-id="de460-108">Formuláře zadat jednoduchý parametr hodnoty</span><span class="sxs-lookup"><span data-stu-id="de460-108">Form to provide simple parameter values</span></span>
* <span data-ttu-id="de460-109">Snadno sledovat stav úlohy</span><span class="sxs-lookup"><span data-stu-id="de460-109">Easily track job state</span></span>
* <span data-ttu-id="de460-110">Přístup k ověření pomocí přihlášení Azure</span><span class="sxs-lookup"><span data-stu-id="de460-110">Access authenticated with Azure logon</span></span>

### <a name="windows-powershell"></a><span data-ttu-id="de460-111">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="de460-111">Windows PowerShell</span></span>

* <span data-ttu-id="de460-112">Volání z příkazového řádku pomocí rutin prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="de460-112">Call from command line with Windows PowerShell cmdlets</span></span>
* <span data-ttu-id="de460-113">Můžou být součástí automatizované řešení s více kroků</span><span class="sxs-lookup"><span data-stu-id="de460-113">Can be included in automated solution with multiple steps</span></span>
* <span data-ttu-id="de460-114">Zadejte parametr jednoduché a komplexní hodnoty</span><span class="sxs-lookup"><span data-stu-id="de460-114">Provide simple and complex parameter values</span></span>
* <span data-ttu-id="de460-115">Sledování stavu úlohy</span><span class="sxs-lookup"><span data-stu-id="de460-115">Track job state</span></span>
* <span data-ttu-id="de460-116">Klienta, které jsou potřebné k podpoře rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="de460-116">Client required to support PowerShell cmdlets</span></span>
* <span data-ttu-id="de460-117">Předat ConfigurationData</span><span class="sxs-lookup"><span data-stu-id="de460-117">Pass ConfigurationData</span></span>
* <span data-ttu-id="de460-118">Zkompilování konfigurace, které používají pověření</span><span class="sxs-lookup"><span data-stu-id="de460-118">Compile configurations that use credentials</span></span>

<span data-ttu-id="de460-119">Jakmile jste se rozhodli metodě kompilace, můžete provést následující spuštění kompilace příslušné procedury.</span><span class="sxs-lookup"><span data-stu-id="de460-119">Once you have decided on a compilation method, you can follow the respective procedures below to start compiling.</span></span>

## <a name="compiling-a-dsc-configuration-with-the-azure-portal"></a><span data-ttu-id="de460-120">Kompilaci konfigurace DSC pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="de460-120">Compiling a DSC Configuration with the Azure portal</span></span>

1. <span data-ttu-id="de460-121">Z vašeho účtu Automation, klikněte na tlačítko **konfigurace DSC**.</span><span class="sxs-lookup"><span data-stu-id="de460-121">From your Automation account, click **DSC Configurations**.</span></span>
2. <span data-ttu-id="de460-122">Klikněte na konfiguraci, kterou chcete otevřete její okno.</span><span class="sxs-lookup"><span data-stu-id="de460-122">Click a configuration to open its blade.</span></span>
3. <span data-ttu-id="de460-123">Klikněte na tlačítko **zkompilovat**.</span><span class="sxs-lookup"><span data-stu-id="de460-123">Click **Compile**.</span></span>
4. <span data-ttu-id="de460-124">Pokud konfigurace nemá žádné parametry, vyzve k potvrzení, zda chcete jej zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="de460-124">If the configuration has no parameters, you will be prompted to confirm whether you want to compile it.</span></span> <span data-ttu-id="de460-125">Pokud konfigurace obsahuje parametry, **zkompilování konfigurace** , můžete zadat hodnoty parametrů, otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="de460-125">If the configuration has parameters, the **Compile Configuration** blade will open so you can provide parameter values.</span></span> <span data-ttu-id="de460-126">Najdete v článku [ **základních parametrů** ](#basic-parameters) části níže další podrobnosti o parametrech.</span><span class="sxs-lookup"><span data-stu-id="de460-126">See the [**Basic Parameters**](#basic-parameters) section below for further details on parameters.</span></span>
5. <span data-ttu-id="de460-127">**Úloha kompilace** otevře se okno, takže můžete sledovat stav úlohy kompilace a konfigurace uzlu (MOF konfigurace dokumenty) se nezdařila z důvodu má být umístěn do Azure Automation DSC pro vyžádání obsahu serveru.</span><span class="sxs-lookup"><span data-stu-id="de460-127">The **Compilation Job** blade is opened so that you can track the compilation job's status, and the node configurations (MOF configuration documents) it caused to be placed on the Azure Automation DSC Pull Server.</span></span>

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a><span data-ttu-id="de460-128">Kompilaci konfigurace DSC pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="de460-128">Compiling a DSC Configuration with Windows PowerShell</span></span>

<span data-ttu-id="de460-129">Můžete použít [ `Start-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) spuštění kompilace pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="de460-129">You can use [`Start-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) to start compiling with Windows PowerShell.</span></span> <span data-ttu-id="de460-130">Následující vzorový kód spustí kompilaci konfigurace DSC názvem **SampleConfig**.</span><span class="sxs-lookup"><span data-stu-id="de460-130">The following sample code starts compilation of a DSC configuration called **SampleConfig**.</span></span>

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
```

<span data-ttu-id="de460-131">`Start-AzureRmAutomationDscCompilationJob`Vrátí objekt úlohy kompilace, která můžete použít ke sledování jeho stavu.</span><span class="sxs-lookup"><span data-stu-id="de460-131">`Start-AzureRmAutomationDscCompilationJob` returns a compilation job object that you can use to track its status.</span></span> <span data-ttu-id="de460-132">Pak můžete použít tento objekt úlohy kompilace s [ `Get-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) k určení stavu úlohy kompilace a [ `Get-AzureRmAutomationDscCompilationJobOutput` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) zobrazíte její datové proudy (výstup).</span><span class="sxs-lookup"><span data-stu-id="de460-132">You can then use this compilation job object with [`Get-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) to determine the status of the compilation job, and [`Get-AzureRmAutomationDscCompilationJobOutput`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) to view its streams (output).</span></span> <span data-ttu-id="de460-133">Následující vzorový kód spustí kompilaci **SampleConfig** konfigurace, čeká na dokončení a potom zobrazí jeho datové proudy.</span><span class="sxs-lookup"><span data-stu-id="de460-133">The following sample code starts compilation of the **SampleConfig** configuration, waits until it has completed, and then displays its streams.</span></span>

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"

while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

## <a name="basic-parameters"></a><span data-ttu-id="de460-134">Základní parametry</span><span class="sxs-lookup"><span data-stu-id="de460-134">Basic Parameters</span></span>
<span data-ttu-id="de460-135">Deklarace parametru v konfiguracích DSC, včetně typy parametrů a vlastnosti, funguje stejně jako v Azure Automation runbook.</span><span class="sxs-lookup"><span data-stu-id="de460-135">Parameter declaration in DSC configurations, including parameter types and properties, works the same as in Azure Automation runbooks.</span></span> <span data-ttu-id="de460-136">V tématu [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md) Další informace o parametry runbooku.</span><span class="sxs-lookup"><span data-stu-id="de460-136">See [Starting a runbook in Azure Automation](automation-starting-a-runbook.md) to learn more about runbook parameters.</span></span>

<span data-ttu-id="de460-137">Následující příklad používá dva parametry s názvem **FeatureName** a **IsPresent**, jak určit hodnoty vlastností v **ParametersExample.sample** uzlu Konfigurace vygenerovaných během kompilace.</span><span class="sxs-lookup"><span data-stu-id="de460-137">The following example uses two parameters called **FeatureName** and **IsPresent**, to determine the values of properties in the **ParametersExample.sample** node configuration, generated during compilation.</span></span>

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

<span data-ttu-id="de460-138">Můžete zkompilovat konfigurace DSC, které používají základní parametry, na portálu Azure Automation DSC, nebo pomocí prostředí Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="de460-138">You can compile DSC Configurations that use basic parameters in the Azure Automation DSC portal, or with Azure PowerShell:</span></span>

### <a name="portal"></a><span data-ttu-id="de460-139">Portál</span><span class="sxs-lookup"><span data-stu-id="de460-139">Portal</span></span>

<span data-ttu-id="de460-140">Na portálu, můžete zadat hodnoty parametrů po kliknutí na **zkompilovat**.</span><span class="sxs-lookup"><span data-stu-id="de460-140">In the portal, you can enter parameter values after clicking **Compile**.</span></span>

![alternativní text](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a><span data-ttu-id="de460-142">PowerShell</span><span class="sxs-lookup"><span data-stu-id="de460-142">PowerShell</span></span>

<span data-ttu-id="de460-143">Vyžaduje parametry v prostředí PowerShell [zatřiďovací tabulky](http://technet.microsoft.com/library/hh847780.aspx) kde klíč odpovídá názvu parametru, a hodnota se rovná hodnotě parametru.</span><span class="sxs-lookup"><span data-stu-id="de460-143">PowerShell requires parameters in a [hashtable](http://technet.microsoft.com/library/hh847780.aspx) where the key matches the parameter name, and the value equals the parameter value.</span></span>

```powershell
$Parameters = @{
    "FeatureName" = "Web-Server"
    "IsPresent" = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters
```

<span data-ttu-id="de460-144">Informace o předávání PSCredentials jako parametry najdete v tématu <a href="#credential-assets"> **prostředků přihlašovacích údajů** </a> níže.</span><span class="sxs-lookup"><span data-stu-id="de460-144">For information about passing PSCredentials as parameters, see <a href="#credential-assets">**Credential Assets**</a> below.</span></span>

## <a name="configurationdata"></a><span data-ttu-id="de460-145">ConfigurationData</span><span class="sxs-lookup"><span data-stu-id="de460-145">ConfigurationData</span></span>
<span data-ttu-id="de460-146">**ConfigurationData** umožňuje oddělit strukturální konfiguraci z žádnou konkrétní konfiguraci prostředí při používání DSC prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="de460-146">**ConfigurationData** allows you to separate structural configuration from any environment specific configuration while using PowerShell DSC.</span></span> <span data-ttu-id="de460-147">V tématu [oddělení "Co" z "Kde" v prostředí PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) Další informace o **ConfigurationData**.</span><span class="sxs-lookup"><span data-stu-id="de460-147">See [Separating "What" from "Where" in PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) to learn more about **ConfigurationData**.</span></span>

> [!NOTE]
> <span data-ttu-id="de460-148">Můžete použít **ConfigurationData** při kompilaci v Azure Automation DSC pomocí Azure PowerShell, ale není v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="de460-148">You can use **ConfigurationData** when compiling in Azure Automation DSC using Azure PowerShell, but not in the Azure portal.</span></span>

<span data-ttu-id="de460-149">Následující příklad DSC konfigurace používá **ConfigurationData** prostřednictvím **$ConfigurationData** a **$AllNodes** klíčová slova.</span><span class="sxs-lookup"><span data-stu-id="de460-149">The following example DSC configuration uses **ConfigurationData** via the **$ConfigurationData** and **$AllNodes** keywords.</span></span> <span data-ttu-id="de460-150">Musíte také [ **xWebAdministration** modulu](https://www.powershellgallery.com/packages/xWebAdministration/) v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="de460-150">You'll also need the [**xWebAdministration** module](https://www.powershellgallery.com/packages/xWebAdministration/) for this example:</span></span>

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

<span data-ttu-id="de460-151">Konfigurace DSC výše v prostředí PowerShell můžete zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="de460-151">You can compile the DSC configuration above with PowerShell.</span></span> <span data-ttu-id="de460-152">Níže prostředí PowerShell přidá dvě konfigurace uzlu DSC pro vyžádání obsahu server Azure Automation: **ConfigurationDataSample.MyVM1** a **ConfigurationDataSample.MyVM3**:</span><span class="sxs-lookup"><span data-stu-id="de460-152">The below PowerShell adds two node configurations to the Azure Automation DSC Pull Server: **ConfigurationDataSample.MyVM1** and **ConfigurationDataSample.MyVM3**:</span></span>

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

## <a name="assets"></a><span data-ttu-id="de460-153">Prostředky</span><span class="sxs-lookup"><span data-stu-id="de460-153">Assets</span></span>

<span data-ttu-id="de460-154">Asset odkazy jsou stejné ve sady runbook a konfigurace Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="de460-154">Asset references are the same in Azure Automation DSC configurations and runbooks.</span></span> <span data-ttu-id="de460-155">Najdete následující informace:</span><span class="sxs-lookup"><span data-stu-id="de460-155">See the following for more information:</span></span>

* [<span data-ttu-id="de460-156">Certifikáty</span><span class="sxs-lookup"><span data-stu-id="de460-156">Certificates</span></span>](automation-certificates.md)
* [<span data-ttu-id="de460-157">Připojení</span><span class="sxs-lookup"><span data-stu-id="de460-157">Connections</span></span>](automation-connections.md)
* [<span data-ttu-id="de460-158">Přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="de460-158">Credentials</span></span>](automation-credentials.md)
* [<span data-ttu-id="de460-159">Proměnné</span><span class="sxs-lookup"><span data-stu-id="de460-159">Variables</span></span>](automation-variables.md)

### <a name="credential-assets"></a><span data-ttu-id="de460-160">Prostředků přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="de460-160">Credential Assets</span></span>

<span data-ttu-id="de460-161">Během konfigurace DSC ve službě Azure Automation, můžete odkazovat pomocí prostředků přihlašovacích údajů **Get-AzureRmAutomationCredential**, prostředků přihlašovacích údajů můžete také být předán prostřednictvím parametrů, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="de460-161">While DSC configurations in Azure Automation can reference credential assets using **Get-AzureRmAutomationCredential**, credential assets can also be passed in via parameters, if desired.</span></span> <span data-ttu-id="de460-162">Pokud konfigurace přebírá parametr **PSCredential** typ, je třeba předat název řetězce objektu prostředek přihlašovacích údajů Azure Automation jako hodnota tohoto parametru, a nikoli objekt PSCredential.</span><span class="sxs-lookup"><span data-stu-id="de460-162">If a configuration takes a parameter of **PSCredential** type, then you need to pass the string name of an Azure Automation credential asset as that parameter’s value, rather than a PSCredential object.</span></span> <span data-ttu-id="de460-163">Na pozadí se načíst a předaný konfigurace prostředek přihlašovacích údajů Azure Automation s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="de460-163">Behind the scenes, the Azure Automation credential asset with that name will be retrieved and passed to the configuration.</span></span>

<span data-ttu-id="de460-164">Zachování pověření zabezpečení v uzlu Konfigurace (configuration dokumenty MOF) vyžaduje šifrování přihlašovacích údajů v souboru MOF konfigurace uzlu.</span><span class="sxs-lookup"><span data-stu-id="de460-164">Keeping credentials secure in node configurations (MOF configuration documents) requires encrypting the credentials in the node configuration MOF file.</span></span> <span data-ttu-id="de460-165">Služby Azure Automation trvá tento krok jeden další a šifruje celý soubor MOF.</span><span class="sxs-lookup"><span data-stu-id="de460-165">Azure Automation takes this one step further and encrypts the entire MOF file.</span></span> <span data-ttu-id="de460-166">Ale právě se musí zjistit DSC Powershellu, že je to v pořádku pro přihlašovací údaje k výstupu v prostém textu v průběhu generování MOF konfigurace uzlu, protože prostředí PowerShell DSC neví, že Azure Automation bude možné šifrování celý soubor MOF po jeho generaci prostřednictvím úlohu kompilace.</span><span class="sxs-lookup"><span data-stu-id="de460-166">However, currently you must tell PowerShell DSC it is okay for credentials to be outputted in plain text during node configuration MOF generation, because PowerShell DSC doesn’t know that Azure Automation will be encrypting the entire MOF file after its generation via a compilation job.</span></span>

<span data-ttu-id="de460-167">DSC Powershellu, které je to v pořádku pro přihlašovací údaje k výstupu v prostém textu v konfiguraci uzlu generované soubory MOF zjistíte pomocí [ **ConfigurationData**](#configurationdata).</span><span class="sxs-lookup"><span data-stu-id="de460-167">You can tell PowerShell DSC that it is okay for credentials to be outputted in plain text in the generated node configuration MOFs using [**ConfigurationData**](#configurationdata).</span></span> <span data-ttu-id="de460-168">By měla předávat `PSDscAllowPlainTextPassword = $true` prostřednictvím **ConfigurationData** pro každý uzel blok název, který se zobrazí v konfigurace DSC a použije přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="de460-168">You should pass `PSDscAllowPlainTextPassword = $true` via **ConfigurationData** for each node block’s name that appears in the DSC configuration and uses credentials.</span></span>

<span data-ttu-id="de460-169">Následující příklad ukazuje konfigurace DSC, který používá prostředek přihlašovacích údajů automatizace.</span><span class="sxs-lookup"><span data-stu-id="de460-169">The following example shows a DSC configuration that uses an Automation credential asset.</span></span>

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

<span data-ttu-id="de460-170">Konfigurace DSC výše v prostředí PowerShell můžete zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="de460-170">You can compile the DSC configuration above with PowerShell.</span></span> <span data-ttu-id="de460-171">Níže prostředí PowerShell přidá dvě konfigurace uzlu DSC pro vyžádání obsahu server Azure Automation: **CredentialSample.MyVM1** a **CredentialSample.MyVM2**.</span><span class="sxs-lookup"><span data-stu-id="de460-171">The below PowerShell adds two node configurations to the Azure Automation DSC Pull Server:  **CredentialSample.MyVM1** and **CredentialSample.MyVM2**.</span></span>

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

## <a name="importing-node-configurations"></a><span data-ttu-id="de460-172">Import konfigurace uzlu</span><span class="sxs-lookup"><span data-stu-id="de460-172">Importing node configurations</span></span>

<span data-ttu-id="de460-173">Můžete také importovat configuratons uzlu (soubory MOF), které zkompilujete mimo Azure.</span><span class="sxs-lookup"><span data-stu-id="de460-173">You can also import node configuratons (MOFs) that you have compiled outside of Azure.</span></span> <span data-ttu-id="de460-174">Jednou z výhod to je, že tento uzel confiturations může být podepsané.</span><span class="sxs-lookup"><span data-stu-id="de460-174">One advantage of this is that node confiturations can be signed.</span></span>
<span data-ttu-id="de460-175">Konfigurace podepsaný uzlu je ověřováno místně na spravovaný uzel DSC agenta, zajistíte, že konfigurace aplikované na uzlu pochází z autorizovaná zdrojová.</span><span class="sxs-lookup"><span data-stu-id="de460-175">A signed node configuration is verified locally on a managed node by the DSC agent, ensuring that the configuration being applied to the node comes from an authorized source.</span></span>

> [!NOTE]
> <span data-ttu-id="de460-176">Můžete importovat podepsané konfigurace do účtu Azure Automation, ale automatizace Azure aktuálně nepodporuje kompilování podepsaný konfigurace.</span><span class="sxs-lookup"><span data-stu-id="de460-176">You can use import signed configurations into your Azure Automation account, but Azure Automation does not currently support compiling signed configurations.</span></span>

> [!NOTE]
> <span data-ttu-id="de460-177">Soubor konfigurace uzlu musí být větší než 1 MB tak, aby ji bylo možné importovat do Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="de460-177">A node configuration file must be no larger than 1 MB to allow it to be imported into Azure Automation.</span></span>

<span data-ttu-id="de460-178">Můžete zjistěte, jak se přihlásit konfigurace uzlu v https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.</span><span class="sxs-lookup"><span data-stu-id="de460-178">You can learn how to sign node configurations at https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.</span></span>

### <a name="importing-a-node-configuration-in-the-azure-portal"></a><span data-ttu-id="de460-179">Import konfigurace uzlu na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="de460-179">Importing a node configuration in the Azure portal</span></span>

1. <span data-ttu-id="de460-180">Z vašeho účtu Automation, klikněte na tlačítko **konfigurace uzlu DSC**.</span><span class="sxs-lookup"><span data-stu-id="de460-180">From your Automation account, click **DSC node configurations**.</span></span>

    ![Konfigurace uzlu DSC](./media/automation-dsc-compile/node-config.png)
2. <span data-ttu-id="de460-182">V **konfigurace uzlu DSC** okně klikněte na tlačítko **přidat NodeConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="de460-182">In the **DSC node configurations** blade, click **Add a NodeConfiguration**.</span></span>
3. <span data-ttu-id="de460-183">V **Import** okně klikněte na ikonu složky vedle **uzlu konfigurační soubor** textové pole a vyhledejte soubor konfigurace uzlu (MOF) v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="de460-183">In the **Import** blade, click the folder icon next to the **Node Configuration File** textbox to browse for a node configuration file (MOF) on your local computer.</span></span>

    ![Procházením vyhledejte místního souboru](./media/automation-dsc-compile/import-browse.png)
4. <span data-ttu-id="de460-185">Zadejte název do pole **název konfigurace** textové pole.</span><span class="sxs-lookup"><span data-stu-id="de460-185">Enter a name in the **Configuration Name** textbox.</span></span> <span data-ttu-id="de460-186">Tento název musí odpovídat názvu konfigurace, ze kterého byl kompilován konfigurace uzlu.</span><span class="sxs-lookup"><span data-stu-id="de460-186">This name must match the name of the configuration from which the node configuration was compiled.</span></span>
5. <span data-ttu-id="de460-187">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="de460-187">Click **OK**.</span></span>

### <a name="importing-a-node-configuration-with-powershell"></a><span data-ttu-id="de460-188">Import konfigurace uzlu pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="de460-188">Importing a node configuration with PowerShell</span></span>

<span data-ttu-id="de460-189">Můžete použít [Import AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) rutiny Import konfigurace uzlu do vašeho účtu automation.</span><span class="sxs-lookup"><span data-stu-id="de460-189">You can use the [Import-AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) cmdlet to import a node configuration into your automation account.</span></span>

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName "MyAutomationAccount" -ResourceGroupName "MyResourceGroup" -ConfigurationName "MyNodeConfiguration" -Path "C:\MyConfigurations\TestVM1.mof"
```



