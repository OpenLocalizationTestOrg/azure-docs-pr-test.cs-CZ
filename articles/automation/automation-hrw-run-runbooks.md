---
title: "Spuštění sad runbook pro Azure Automation Hybrid Runbook Worker. | Microsoft Docs"
description: "Tento článek obsahuje informace o spuštění sady runbook na počítačích ve vaší místní datacenter nebo poskytovatele cloudových služeb s rolí hybridní pracovní proces Runbooku."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 06227cda-f3d1-47fe-b3f8-436d2b9d81ee
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/22/2017
ms.author: magoedte
ms.openlocfilehash: 993bc3ea480a329541ca4ae825189cdb5a2b4a8b
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a><span data-ttu-id="f9e87-103">Spuštěné sady runbook pro Hybrid Runbook Worker.</span><span class="sxs-lookup"><span data-stu-id="f9e87-103">Running runbooks on a Hybrid Runbook Worker</span></span> 
<span data-ttu-id="f9e87-104">Není žádný rozdíl ve struktuře sad runbook, které běží v Azure Automation a ty, které běží na hybridní pracovní proces Runbooku.</span><span class="sxs-lookup"><span data-stu-id="f9e87-104">There is no difference in the structure of runbooks that run in Azure Automation and those that run on a Hybrid Runbook Worker.</span></span> <span data-ttu-id="f9e87-105">Sady Runbook, které můžete použít pro každý s největší pravděpodobností se podstatně liší ale vzhledem k tomu obvykle cílení hybridní pracovní proces Runbooku sady runbook spravovat prostředky v místním počítači sám sebe nebo s prostředky v místním prostředí, kde je nasazen, při sady runbook v Služby Azure Automation obvykle spravovat prostředky v cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="f9e87-105">Runbooks that you use with each most likely differ significantly though since runbooks targeting a Hybrid Runbook Worker typically manage resources on the local computer itself or against resources in the local environment where it is deployed, while runbooks in Azure Automation typically manage resources in the Azure cloud.</span></span>

<span data-ttu-id="f9e87-106">Můžete upravit sady runbook pro hybridní pracovní proces Runbooku ve službě Azure Automation, ale mohou mít problémy, pokud se pokusíte o test runbooku se v editoru.</span><span class="sxs-lookup"><span data-stu-id="f9e87-106">You can edit a runbook for Hybrid Runbook Worker in Azure Automation, but you may have difficulties if you try to test the runbook in the editor.</span></span>  <span data-ttu-id="f9e87-107">Moduly prostředí PowerShell, které přístup k místním prostředkům pravděpodobně nainstalována ve vašem prostředí Azure Automation. v takovém případě, test se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="f9e87-107">The PowerShell modules that access the local resources may not be installed in your Azure Automation environment in which case, the test would fail.</span></span>  <span data-ttu-id="f9e87-108">Pokud nainstalujete požadované moduly, spusťte sadu runbook, ale nebude mít přístup k místním prostředkům pro dokončení testu.</span><span class="sxs-lookup"><span data-stu-id="f9e87-108">If you do install the required modules, then the runbook will run, but it will not be able to access local resources for a complete test.</span></span>

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a><span data-ttu-id="f9e87-109">Spuštění sady runbook pro Hybrid Runbook Worker.</span><span class="sxs-lookup"><span data-stu-id="f9e87-109">Starting a runbook on Hybrid Runbook Worker</span></span>
<span data-ttu-id="f9e87-110">[Spuštění sady Runbook ve službě Azure Automation](automation-starting-a-runbook.md) popisuje různé metody pro spuštění sady runbook.</span><span class="sxs-lookup"><span data-stu-id="f9e87-110">[Starting a Runbook in Azure Automation](automation-starting-a-runbook.md) describes different methods for starting a runbook.</span></span>  <span data-ttu-id="f9e87-111">Přidá hybridní pracovní proces Runbooku **RunOn** možnost, kde můžete zadat název skupiny hybridní Runbook Worker.</span><span class="sxs-lookup"><span data-stu-id="f9e87-111">Hybrid Runbook Worker adds a **RunOn** option where you can specify the name of a Hybrid Runbook Worker Group.</span></span>  <span data-ttu-id="f9e87-112">Pokud je skupina zadaná, sada runbook je načíst a spustit pracovníků v této skupině.</span><span class="sxs-lookup"><span data-stu-id="f9e87-112">If a group is specified, then the runbook is retrieved and run by of the workers in that group.</span></span>  <span data-ttu-id="f9e87-113">Pokud není tato možnost zadána, pak spuštění ve službě Azure Automation jako normální.</span><span class="sxs-lookup"><span data-stu-id="f9e87-113">If this option is not specified, then it is run in Azure Automation as normal.</span></span>

<span data-ttu-id="f9e87-114">Při spuštění sady runbook na portálu Azure, se zobrazí **spustit na** možnost, kde můžete vybrat **Azure** nebo **hybridní pracovní proces**.</span><span class="sxs-lookup"><span data-stu-id="f9e87-114">When you start a runbook in the Azure portal, you are presented with a **Run on** option where you can select **Azure** or **Hybrid Worker**.</span></span>  <span data-ttu-id="f9e87-115">Pokud vyberete **hybridní pracovní proces**, pak můžete vybrat skupiny z rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="f9e87-115">If you select **Hybrid Worker**, then you can select the group from a dropdown.</span></span>

<span data-ttu-id="f9e87-116">Použití **RunOn** parametr.</span><span class="sxs-lookup"><span data-stu-id="f9e87-116">Use the **RunOn** parameter.</span></span>  <span data-ttu-id="f9e87-117">Můžete následující příkaz pro spuštění sady runbook s názvem Test-Runbook na hybridní skupina pracovního procesu Runbook s názvem MyHybridGroup pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f9e87-117">You can use the following command to start a runbook named Test-Runbook on a Hybrid Runbook Worker Group named MyHybridGroup using Windows PowerShell.</span></span>

    Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"

> [!NOTE]
> <span data-ttu-id="f9e87-118">**RunOn** parametr byl přidán do **Start-AzureAutomationRunbook** rutiny ve verzi 0.9.1 Microsoft Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f9e87-118">The **RunOn** parameter was added to the **Start-AzureAutomationRunbook** cmdlet in version 0.9.1 of Microsoft Azure PowerShell.</span></span>  <span data-ttu-id="f9e87-119">Měli byste [stáhnout nejnovější verzi](https://azure.microsoft.com/downloads/) Pokud máte některou z dřívějších nainstalována.</span><span class="sxs-lookup"><span data-stu-id="f9e87-119">You should [download the latest version](https://azure.microsoft.com/downloads/) if you have an earlier one installed.</span></span>  <span data-ttu-id="f9e87-120">Stačí tuto verzi nainstalovat na pracovní stanici, kde spouštíte sadu runbook z prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f9e87-120">You only need to install this version on a workstation where you are starting the runbook from Windows PowerShell.</span></span>  <span data-ttu-id="f9e87-121">Nemusíte jej nainstalovat na počítače pracovní, pokud máte v úmyslu spouštění sad runbook z tohoto počítače.</span><span class="sxs-lookup"><span data-stu-id="f9e87-121">You do not need to install it on the worker computer unless you intend to start runbooks from that computer.</span></span>  <span data-ttu-id="f9e87-122">Nelze spustit aktuálně sady runbook na hybridní pracovní proces Runbooku z jiného runbooku vzhledem k tomu, že bude vyžadovat nejnovější verzi prostředí Azure Powershell má být nainstalován do vašeho účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="f9e87-122">You cannot currently start a runbook on a Hybrid Runbook Worker from another runbook since this would require the latest version of Azure Powershell to be installed in your Automation account.</span></span>  <span data-ttu-id="f9e87-123">Nejnovější verze je automaticky aktualizovat ve službě Azure Automation a automaticky instaluje zaměstnancům brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f9e87-123">The latest version is automatically updated in Azure Automation and automatically pushed down to the workers soon.</span></span>
>
>

## <a name="runbook-permissions"></a><span data-ttu-id="f9e87-124">Oprávnění sady Runbook</span><span class="sxs-lookup"><span data-stu-id="f9e87-124">Runbook permissions</span></span>
<span data-ttu-id="f9e87-125">Runbook spuštěných v hybridní pracovní proces Runbooku nelze použít stejnou metodu, která se obvykle používá pro runbooky ověřované pro prostředky Azure, protože jejich přístupu k prostředkům mimo Azure.</span><span class="sxs-lookup"><span data-stu-id="f9e87-125">Runbooks running on a Hybrid Runbook Worker cannot use the same method that is typically used for runbooks authenticating to Azure resources, since they are accessing resources outside of Azure.</span></span>  <span data-ttu-id="f9e87-126">Runbook můžete buď zadat vlastní ověřování k místním prostředkům, nebo můžete zadat účet RunAs zajistit kontext uživatele pro všechny sady runbook.</span><span class="sxs-lookup"><span data-stu-id="f9e87-126">The runbook can either provide its own authentication to local resources, or you can specify a RunAs account to provide a user context for all runbooks.</span></span>

### <a name="runbook-authentication"></a><span data-ttu-id="f9e87-127">Ověření sady Runbook</span><span class="sxs-lookup"><span data-stu-id="f9e87-127">Runbook authentication</span></span>
<span data-ttu-id="f9e87-128">Ve výchozím nastavení sady runbook poběží v kontextu účtu místního systému na místním počítači, musí zadat své vlastní ověřování na prostředky, které budou přistupovat k.</span><span class="sxs-lookup"><span data-stu-id="f9e87-128">By default, runbooks will run in the context of the local System account on the on-premises computer, so they must provide their own authentication to resources that they will access.</span></span>  

<span data-ttu-id="f9e87-129">Můžete použít [pověření](http://msdn.microsoft.com/library/dn940015.aspx) a [certifikát](http://msdn.microsoft.com/library/dn940013.aspx) prostředky ve vašem runbooku pomocí rutiny, které vám umožní zadat přihlašovací údaje, můžete provést ověření na různých prostředků.</span><span class="sxs-lookup"><span data-stu-id="f9e87-129">You can use [Credential](http://msdn.microsoft.com/library/dn940015.aspx) and [Certificate](http://msdn.microsoft.com/library/dn940013.aspx) assets in your runbook with cmdlets that allow you to specify credentials so you can authenticate to different resources.</span></span>  <span data-ttu-id="f9e87-130">Následující příklad ukazuje část sady runbook, který restartuje počítač.</span><span class="sxs-lookup"><span data-stu-id="f9e87-130">The following example shows a portion of a runbook that restarts a computer.</span></span>  <span data-ttu-id="f9e87-131">Načte přihlašovací údaje z asset přihlašovacích údajů a názvu počítače z variabilní prostředek a potom tyto hodnoty používá pomocí rutiny Restart-Computer.</span><span class="sxs-lookup"><span data-stu-id="f9e87-131">It retrieves credentials from a credential asset and the name of the computer from a variable asset and then uses these values with the Restart-Computer cmdlet.</span></span>

    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
    $Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

    Restart-Computer -ComputerName $Computer -Credential $Cred

<span data-ttu-id="f9e87-132">Můžete také využít [InlineScript](automation-powershell-workflow.md#inlinescript), která umožňuje spuštění bloky kódu na jiném počítači s přihlašovacími údaji určeného [PSCredential společný parametr](http://technet.microsoft.com/library/jj129719.aspx).</span><span class="sxs-lookup"><span data-stu-id="f9e87-132">You can also leverage [InlineScript](automation-powershell-workflow.md#inlinescript), which  allows you to run blocks of code on another computer with credentials specified by the [PSCredential common parameter](http://technet.microsoft.com/library/jj129719.aspx).</span></span>

### <a name="runas-account"></a><span data-ttu-id="f9e87-133">Účet Spustit jako</span><span class="sxs-lookup"><span data-stu-id="f9e87-133">RunAs account</span></span>
<span data-ttu-id="f9e87-134">Namísto s runbooky své vlastní ověřování k místním prostředkům, můžete zadat **RunAs** účet pro skupinu hybridních pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="f9e87-134">Instead of having runbooks provide their own authentication to local resources, you can specify a **RunAs** account for a Hybrid worker group.</span></span>  <span data-ttu-id="f9e87-135">Zadáte [asset přihlašovacích údajů](automation-credentials.md) který má přístup k místním prostředkům a všechny sady runbook spouštět tyto přihlašovací údaje při spuštění v hybridní pracovní proces Runbooku ve skupině.</span><span class="sxs-lookup"><span data-stu-id="f9e87-135">You specify a [credential asset](automation-credentials.md) that has access to local resources, and all runbooks run under these credentials when running on a Hybrid Runbook Worker in the group.</span></span>  

<span data-ttu-id="f9e87-136">Uživatelské jméno pro přihlašovací údaje musí být v jednom z následujících formátů:</span><span class="sxs-lookup"><span data-stu-id="f9e87-136">The user name for the credential must be in one of the following formats:</span></span>

* <span data-ttu-id="f9e87-137">doména\uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="f9e87-137">domain\username</span></span>
* username@domain
* <span data-ttu-id="f9e87-138">uživatelské jméno (pro účty místní na místním počítači)</span><span class="sxs-lookup"><span data-stu-id="f9e87-138">username (for accounts local to the on-premises computer)</span></span>

<span data-ttu-id="f9e87-139">Následující postup použijte k určení účtu RunAs pro skupinu hybridních pracovních procesů:</span><span class="sxs-lookup"><span data-stu-id="f9e87-139">Use the following procedure to specify a RunAs account for a Hybrid worker group:</span></span>

1. <span data-ttu-id="f9e87-140">Vytvoření [asset přihlašovacích údajů](automation-credentials.md) přístup k místním prostředkům.</span><span class="sxs-lookup"><span data-stu-id="f9e87-140">Create a [credential asset](automation-credentials.md) with access to local resources.</span></span>
2. <span data-ttu-id="f9e87-141">Otevřete účet Automation na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f9e87-141">Open the Automation account in the Azure portal.</span></span>
3. <span data-ttu-id="f9e87-142">Vyberte **skupinám Hybrid Worker** dlaždici a pak vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="f9e87-142">Select the **Hybrid Worker Groups** tile, and then select the group.</span></span>
4. <span data-ttu-id="f9e87-143">Vyberte **všechna nastavení** a potom **nastavení skupiny hybridních pracovních**.</span><span class="sxs-lookup"><span data-stu-id="f9e87-143">Select **All settings** and then **Hybrid worker group settings**.</span></span>
5. <span data-ttu-id="f9e87-144">Změna **spustit jako** z **výchozí** k **vlastní**.</span><span class="sxs-lookup"><span data-stu-id="f9e87-144">Change **Run As** from **Default** to **Custom**.</span></span>
6. <span data-ttu-id="f9e87-145">Vyberte přihlašovací údaje a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f9e87-145">Select the credential and click **Save**.</span></span>

### <a name="automation-run-as-account"></a><span data-ttu-id="f9e87-146">Účet Spustit jako automatizace</span><span class="sxs-lookup"><span data-stu-id="f9e87-146">Automation Run As account</span></span>
<span data-ttu-id="f9e87-147">Jako součást procesu automatizované sestavení pro nasazení prostředků v Azure možná budete potřebovat přístup k místním systémy pro podporu ve vašem nasazení pořadí úloh nebo sadu kroků.</span><span class="sxs-lookup"><span data-stu-id="f9e87-147">As part of your automated build process for deploying resources in Azure, you may require access to on-premise systems to support a task or set of steps in your deployment sequence.</span></span>  <span data-ttu-id="f9e87-148">Pro podporu ověřování proti Azure pomocí účtu spustit jako, musíte nainstalovat certifikát účtu spustit jako.</span><span class="sxs-lookup"><span data-stu-id="f9e87-148">To support authentication against Azure using the Run As account, you need to install the Run As account certificate.</span></span>  

<span data-ttu-id="f9e87-149">Následující Powershellový runbook *Export RunAsCertificateToHybridWorker*, exportuje certifikát spustit jako z vašeho účtu Azure Automation a soubory ke stažení a naimportuje ho do úložiště certifikátů místního počítače v hybridním pracovní připojený k stejný účet.</span><span class="sxs-lookup"><span data-stu-id="f9e87-149">The following PowerShell runbook, *Export-RunAsCertificateToHybridWorker*, exports the Run As certificate from your Azure Automation account and downloads and imports it into the local machine certificate store on a Hybrid worker connected to the same account.</span></span>  <span data-ttu-id="f9e87-150">Po dokončení tohoto kroku, tak ověří, že pracovní proces může úspěšně ověřit do Azure pomocí účtu spustit jako.</span><span class="sxs-lookup"><span data-stu-id="f9e87-150">Once that step is completed, it verifies the worker can successfully authenticate to Azure using the Run As account.</span></span>

    <#PSScriptInfo
    .VERSION 1.0
    .GUID 3a796b9a-623d-499d-86c8-c249f10a6986
    .AUTHOR Azure Automation Team
    .COMPANYNAME Microsoft
    .COPYRIGHT 
    .TAGS Azure Automation 
    .LICENSEURI 
    .PROJECTURI 
    .ICONURI 
    .EXTERNALMODULEDEPENDENCIES 
    .REQUIREDSCRIPTS 
    .EXTERNALSCRIPTDEPENDENCIES 
    .RELEASENOTES
    #>

    <#  
    .SYNOPSIS  
    Exports the Run As certificate from an Azure Automation account to a hybrid worker in that account. 
  
    .DESCRIPTION  
    This runbook exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.
    Run this runbook in the hybrid worker where you want the certificate installed.
    This allows the use of the AzureRunAsConnection to authenticate to Azure and manage Azure resources from runbooks running in the hybrid worker.

    .EXAMPLE
    .\Export-RunAsCertificateToHybridWorker

    .NOTES
    AUTHOR: Azure Automation Team 
    LASTEDIT: 2016.10.13
    #>

    [OutputType([string])] 

    # Set the password used for this certificate
    $Password = "YourStrongPasswordForTheCert"

    # Stop on errors
    $ErrorActionPreference = 'stop'

    # Get the management certificate that will be used to make calls into Azure Service Management resources
    $RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"
       
    # location to store temporary certificate in the Automation service host
    $CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"
   
    # Save the certificate
    $Cert = $RunAsCert.Export("pfx",$Password)
    Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose 

    Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
    $SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
    Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

    # Test that authentication to Azure Resource Manager is working
    $RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection" 
    
    Add-AzureRmAccount `
      -ServicePrincipal `
      -TenantId $RunAsConnection.TenantId `
      -ApplicationId $RunAsConnection.ApplicationId `
      -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

    Set-AzureRmContext -SubscriptionId $RunAsConnection.SubscriptionID | Write-Verbose

    # List automation accounts to confirm Azure Resource Manager calls are working
    Get-AzureRmAutomationAccount | Select AutomationAccountName

<span data-ttu-id="f9e87-151">Uložit *Export RunAsCertificateToHybridWorker* runbook do počítače s `.ps1` rozšíření.</span><span class="sxs-lookup"><span data-stu-id="f9e87-151">Save the *Export-RunAsCertificateToHybridWorker* runbook to your computer with a `.ps1` extension.</span></span>  <span data-ttu-id="f9e87-152">Importujte ho do vašeho účtu Automation a upravovat sady runbook, změně hodnoty proměnné `$Password` s vlastní heslo.</span><span class="sxs-lookup"><span data-stu-id="f9e87-152">Import it into your Automation account and edit the runbook, changing the value of the variable `$Password` with your own password.</span></span>  <span data-ttu-id="f9e87-153">Publikování a znovu spusťte sady runbook cílené na skupinu hybridních pracovních procesů, které spustit a ověření runbooků pomocí účtu spustit jako.</span><span class="sxs-lookup"><span data-stu-id="f9e87-153">Publish and then run the runbook targeting the Hybrid Worker group that run and authenticate runbooks using the Run As account.</span></span>  <span data-ttu-id="f9e87-154">Stream úloh sestavy pokus o import certifikátu do úložiště místního počítače a odpovídá s více řádky v závislosti na tom, kolik účty Automation jsou definovány v rámci vašeho předplatného, a pokud je ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="f9e87-154">The job stream reports the attempt to import the certificate into the local machine store, and follows with multiple lines depending on how many Automation accounts are defined in your subscription and if authentication is successful.</span></span>  

## <a name="troubleshooting-runbooks-on-hybrid-runbook-worker"></a><span data-ttu-id="f9e87-155">Řešení potíží s sady runbook pro Hybrid Runbook Worker.</span><span class="sxs-lookup"><span data-stu-id="f9e87-155">Troubleshooting runbooks on Hybrid Runbook Worker</span></span>
<span data-ttu-id="f9e87-156">Protokoly se ukládají místně na každém hybridní pracovní proces na C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.</span><span class="sxs-lookup"><span data-stu-id="f9e87-156">Logs are stored locally on each hybrid worker at C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.</span></span>  <span data-ttu-id="f9e87-157">Hybridní pracovní proces taky zaznamenává chyby a události v protokolu událostí systému Windows v rámci **aplikace a služby Logs\Microsoft-SMA\Operational**.</span><span class="sxs-lookup"><span data-stu-id="f9e87-157">Hybrid worker also records errors and events in the Windows event log under **Application and Services Logs\Microsoft-SMA\Operational**.</span></span>  <span data-ttu-id="f9e87-158">Události souvisejících se sadami runbook spustit v pracovním procesu se zapisují do **aplikace a služby Logs\Microsoft-Automation\Operational**.</span><span class="sxs-lookup"><span data-stu-id="f9e87-158">Events related to runbooks executed on the worker are written to **Application and Services Logs\Microsoft-Automation\Operational**.</span></span>  <span data-ttu-id="f9e87-159">**Microsoft SMA** protokolu zahrnuje mnoho další události související s úloha sady runbook nabídnutých do pracovního procesu a zpracování sady runbook.</span><span class="sxs-lookup"><span data-stu-id="f9e87-159">The **Microsoft-SMA** log includes many more events related to the runbook job pushed to the worker and the processing of the runbook.</span></span>  <span data-ttu-id="f9e87-160">Když **automatizace Microsoft** protokolu událostí s podrobnostmi o pomoc při řešení potíží s spuštění sady runbook nemá mnoho událostí, zjistíte alespoň výsledky úlohy runbooku.</span><span class="sxs-lookup"><span data-stu-id="f9e87-160">While the **Microsoft-Automation** event log does not have many events with details assisting with the troubleshooting of runbook execution, you will at least find the results of the runbook job.</span></span>  

<span data-ttu-id="f9e87-161">[Runbook výstup a zprávy](automation-runbook-output-and-messages.md) odešlou do Azure Automation z hybridní pracovní procesy stejně jako úlohy sady runbook běží v cloudu.</span><span class="sxs-lookup"><span data-stu-id="f9e87-161">[Runbook output and messages](automation-runbook-output-and-messages.md) are sent to Azure Automation from hybrid workers just like runbook jobs run in the cloud.</span></span>  <span data-ttu-id="f9e87-162">Můžete také povolit podrobné a průběh datové proudy stejným způsobem jako pro ostatní sady runbook.</span><span class="sxs-lookup"><span data-stu-id="f9e87-162">You can also enable the Verbose and Progress streams the same way you would for other runbooks.</span></span>  

<span data-ttu-id="f9e87-163">Pokud vaše sady runbook nejsou úspěšném dokončení a Souhrn úlohy zobrazuje stav **pozastaveno**, přečtěte si článek o odstraňování potíží [Hybrid Runbook Worker: ukončí úlohy runbooku se stavem pozastaveno ](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).</span><span class="sxs-lookup"><span data-stu-id="f9e87-163">If your runbooks are not completing successfully and the job summary shows a status of **Suspended**, please review the troubleshooting article [Hybrid Runbook Worker: A runbook job terminates with a status of Suspended](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).</span></span>   

## <a name="next-steps"></a><span data-ttu-id="f9e87-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f9e87-164">Next steps</span></span>
* <span data-ttu-id="f9e87-165">Další informace o různých metod, které můžete použít ke spuštění sady runbook najdete v tématu [spuštění sady Runbook ve službě Azure Automation](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="f9e87-165">To learn more about the different methods that can be used to start a runbook, see [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md).</span></span>  
* <span data-ttu-id="f9e87-166">Chcete-li pochopit různé postupy pro práci se sadami runbook Powershellu a pracovní postup prostředí PowerShell ve službě Azure Automation pomocí textový editor, přečtěte si téma [úpravy sady Runbook ve službě Azure Automation](automation-edit-textual-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="f9e87-166">To understand the different procedures for working with PowerShell and PowerShell Workflow runbooks in Azure Automation using the textual editor, see [Editing a Runbook in Azure Automation](automation-edit-textual-runbook.md)</span></span>