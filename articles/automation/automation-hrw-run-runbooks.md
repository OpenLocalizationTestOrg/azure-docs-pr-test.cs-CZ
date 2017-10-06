---
title: sady runbook aaaRun pro Azure Automation Hybrid Runbook Worker. | Microsoft Docs
description: "Tento článek obsahuje informace o spuštění sady runbook na počítačích ve vaší místní datacenter nebo poskytovatele cloudových služeb s rolí hello hybridní pracovní proces Runbooku."
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
ms.openlocfilehash: 51961e02603e5690edd11e577594ad2ddea489a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a><span data-ttu-id="14480-103">Spuštěné sady runbook pro Hybrid Runbook Worker.</span><span class="sxs-lookup"><span data-stu-id="14480-103">Running runbooks on a Hybrid Runbook Worker</span></span> 
<span data-ttu-id="14480-104">Není žádný rozdíl ve struktuře hello sad runbook, které běží v Azure Automation a ty, které běží na hybridní pracovní proces Runbooku.</span><span class="sxs-lookup"><span data-stu-id="14480-104">There is no difference in hello structure of runbooks that run in Azure Automation and those that run on a Hybrid Runbook Worker.</span></span> <span data-ttu-id="14480-105">Sady Runbook, které můžete použít pro každý s největší pravděpodobností se podstatně liší ale vzhledem k tomu obvykle cílení hybridní pracovní proces Runbooku sady runbook spravovat prostředky na místním počítači hello, sám sebe nebo s prostředky v hello místní prostředí, kde je nasazen, při sady runbook ve službě Azure Automation obvykle spravovat prostředky v cloudu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="14480-105">Runbooks that you use with each most likely differ significantly though since runbooks targeting a Hybrid Runbook Worker typically manage resources on hello local computer itself or against resources in hello local environment where it is deployed, while runbooks in Azure Automation typically manage resources in hello Azure cloud.</span></span>

<span data-ttu-id="14480-106">Můžete upravit sady runbook pro hybridní pracovní proces Runbooku ve službě Azure Automation, ale mohou mít problémy, pokud se pokusíte tootest hello runbook v editoru hello.</span><span class="sxs-lookup"><span data-stu-id="14480-106">You can edit a runbook for Hybrid Runbook Worker in Azure Automation, but you may have difficulties if you try tootest hello runbook in hello editor.</span></span>  <span data-ttu-id="14480-107">Hello moduly Powershellu, které přístup k místním prostředkům hello se nenainstaluje ve vašem prostředí Azure Automation. v takovém případě, hello test skončí s chybou.</span><span class="sxs-lookup"><span data-stu-id="14480-107">hello PowerShell modules that access hello local resources may not be installed in your Azure Automation environment in which case, hello test would fail.</span></span>  <span data-ttu-id="14480-108">Pokud nainstalujete hello požadované moduly, pak spustí hello runbook, ale nebude možné tooaccess místních prostředků pro dokončení testovacího.</span><span class="sxs-lookup"><span data-stu-id="14480-108">If you do install hello required modules, then hello runbook will run, but it will not be able tooaccess local resources for a complete test.</span></span>

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a><span data-ttu-id="14480-109">Spuštění sady runbook pro Hybrid Runbook Worker.</span><span class="sxs-lookup"><span data-stu-id="14480-109">Starting a runbook on Hybrid Runbook Worker</span></span>
<span data-ttu-id="14480-110">[Spuštění sady Runbook ve službě Azure Automation](automation-starting-a-runbook.md) popisuje různé metody pro spuštění sady runbook.</span><span class="sxs-lookup"><span data-stu-id="14480-110">[Starting a Runbook in Azure Automation](automation-starting-a-runbook.md) describes different methods for starting a runbook.</span></span>  <span data-ttu-id="14480-111">Přidá hybridní pracovní proces Runbooku **RunOn** možnost, kde můžete určit název hello hybridní Runbook pracovní skupiny.</span><span class="sxs-lookup"><span data-stu-id="14480-111">Hybrid Runbook Worker adds a **RunOn** option where you can specify hello name of a Hybrid Runbook Worker Group.</span></span>  <span data-ttu-id="14480-112">Pokud je skupina zadaná, je hello runbooku načíst a spustit hello pracovníků v této skupině.</span><span class="sxs-lookup"><span data-stu-id="14480-112">If a group is specified, then hello runbook is retrieved and run by of hello workers in that group.</span></span>  <span data-ttu-id="14480-113">Pokud není tato možnost zadána, pak spuštění ve službě Azure Automation jako normální.</span><span class="sxs-lookup"><span data-stu-id="14480-113">If this option is not specified, then it is run in Azure Automation as normal.</span></span>

<span data-ttu-id="14480-114">Při spuštění runbooku v hello portálu Azure, se zobrazí **spustit na** možnost, kde můžete vybrat **Azure** nebo **hybridní pracovní proces**.</span><span class="sxs-lookup"><span data-stu-id="14480-114">When you start a runbook in hello Azure portal, you are presented with a **Run on** option where you can select **Azure** or **Hybrid Worker**.</span></span>  <span data-ttu-id="14480-115">Pokud vyberete **hybridní pracovní proces**, pak můžete vybrat skupinu hello z rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="14480-115">If you select **Hybrid Worker**, then you can select hello group from a dropdown.</span></span>

<span data-ttu-id="14480-116">Použití hello **RunOn** parametr.</span><span class="sxs-lookup"><span data-stu-id="14480-116">Use hello **RunOn** parameter.</span></span>  <span data-ttu-id="14480-117">Můžete použít následující příkaz toostart sady runbook s názvem Test-Runbook na hybridní skupina pracovního procesu Runbook s názvem MyHybridGroup pomocí prostředí Windows PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="14480-117">You can use hello following command toostart a runbook named Test-Runbook on a Hybrid Runbook Worker Group named MyHybridGroup using Windows PowerShell.</span></span>

    Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"

> [!NOTE]
> <span data-ttu-id="14480-118">Hello **RunOn** byl přidán parametr toohello **Start-AzureAutomationRunbook** rutiny ve verzi 0.9.1 Microsoft Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="14480-118">hello **RunOn** parameter was added toohello **Start-AzureAutomationRunbook** cmdlet in version 0.9.1 of Microsoft Azure PowerShell.</span></span>  <span data-ttu-id="14480-119">Měli byste [stažení nejnovější verze hello](https://azure.microsoft.com/downloads/) Pokud máte některou z dřívějších nainstalována.</span><span class="sxs-lookup"><span data-stu-id="14480-119">You should [download hello latest version](https://azure.microsoft.com/downloads/) if you have an earlier one installed.</span></span>  <span data-ttu-id="14480-120">Potřebujete jenom tooinstall touto verzí na pracovní stanici, kde spouštíte hello runbook z prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="14480-120">You only need tooinstall this version on a workstation where you are starting hello runbook from Windows PowerShell.</span></span>  <span data-ttu-id="14480-121">Není nutné tooinstall ho na počítači pracovní hello Pokud hodláte toostart sady runbook z tohoto počítače.</span><span class="sxs-lookup"><span data-stu-id="14480-121">You do not need tooinstall it on hello worker computer unless you intend toostart runbooks from that computer.</span></span>  <span data-ttu-id="14480-122">Nelze spustit aktuálně sady runbook na hybridní pracovní proces Runbooku z jiného runbooku vzhledem k tomu, že bude vyžadovat hello nejnovější verzi prostředí Azure Powershell toobe nainstalovaný ve vašem účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="14480-122">You cannot currently start a runbook on a Hybrid Runbook Worker from another runbook since this would require hello latest version of Azure Powershell toobe installed in your Automation account.</span></span>  <span data-ttu-id="14480-123">nejnovější verzi Hello je automaticky aktualizovat ve službě Azure Automation a automaticky instaluje dolů toohello pracovníci brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="14480-123">hello latest version is automatically updated in Azure Automation and automatically pushed down toohello workers soon.</span></span>
>
>

## <a name="runbook-permissions"></a><span data-ttu-id="14480-124">Oprávnění sady Runbook</span><span class="sxs-lookup"><span data-stu-id="14480-124">Runbook permissions</span></span>
<span data-ttu-id="14480-125">Runbook spuštěných v hybridní pracovní proces Runbooku nelze použít hello stejné metody, která se obvykle používá pro ověřování tooAzure prostředky, protože jejich přístupu k prostředkům mimo Azure sady runbook.</span><span class="sxs-lookup"><span data-stu-id="14480-125">Runbooks running on a Hybrid Runbook Worker cannot use hello same method that is typically used for runbooks authenticating tooAzure resources, since they are accessing resources outside of Azure.</span></span>  <span data-ttu-id="14480-126">Hello runbook můžete buď zadat vlastní ověřování toolocal prostředky, nebo můžete zadat tooprovide účet RunAs kontext uživatele pro všechny sady runbook.</span><span class="sxs-lookup"><span data-stu-id="14480-126">hello runbook can either provide its own authentication toolocal resources, or you can specify a RunAs account tooprovide a user context for all runbooks.</span></span>

### <a name="runbook-authentication"></a><span data-ttu-id="14480-127">Ověření sady Runbook</span><span class="sxs-lookup"><span data-stu-id="14480-127">Runbook authentication</span></span>
<span data-ttu-id="14480-128">Ve výchozím nastavení bude sady runbook spustit v kontextu hello hello místní systémový účet na místním počítači hello, takže musí zadat své vlastní tooresources ověřování, který bude přistupovat k.</span><span class="sxs-lookup"><span data-stu-id="14480-128">By default, runbooks will run in hello context of hello local System account on hello on-premises computer, so they must provide their own authentication tooresources that they will access.</span></span>  

<span data-ttu-id="14480-129">Můžete použít [pověření](http://msdn.microsoft.com/library/dn940015.aspx) a [certifikát](http://msdn.microsoft.com/library/dn940013.aspx) prostředky ve vašem runbooku pomocí rutin, které umožní toospecify přihlašovací údaje, můžete ověřovat toodifferent prostředky.</span><span class="sxs-lookup"><span data-stu-id="14480-129">You can use [Credential](http://msdn.microsoft.com/library/dn940015.aspx) and [Certificate](http://msdn.microsoft.com/library/dn940013.aspx) assets in your runbook with cmdlets that allow you toospecify credentials so you can authenticate toodifferent resources.</span></span>  <span data-ttu-id="14480-130">Hello následující příklad ukazuje část sady runbook, který restartuje počítač.</span><span class="sxs-lookup"><span data-stu-id="14480-130">hello following example shows a portion of a runbook that restarts a computer.</span></span>  <span data-ttu-id="14480-131">Načte přihlašovací údaje z název přihlašovacího údaje asset a hello hello počítače z variabilní prostředek a potom tyto hodnoty používá pomocí rutiny Restart-Computer hello.</span><span class="sxs-lookup"><span data-stu-id="14480-131">It retrieves credentials from a credential asset and hello name of hello computer from a variable asset and then uses these values with hello Restart-Computer cmdlet.</span></span>

    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
    $Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

    Restart-Computer -ComputerName $Computer -Credential $Cred

<span data-ttu-id="14480-132">Můžete také využít [InlineScript](automation-powershell-workflow.md#inlinescript), což vám umožní toorun bloky kódu na jiném počítači pomocí přihlašovacích údajů určeného hello [PSCredential společný parametr](http://technet.microsoft.com/library/jj129719.aspx).</span><span class="sxs-lookup"><span data-stu-id="14480-132">You can also leverage [InlineScript](automation-powershell-workflow.md#inlinescript), which  allows you toorun blocks of code on another computer with credentials specified by hello [PSCredential common parameter](http://technet.microsoft.com/library/jj129719.aspx).</span></span>

### <a name="runas-account"></a><span data-ttu-id="14480-133">Účet Spustit jako</span><span class="sxs-lookup"><span data-stu-id="14480-133">RunAs account</span></span>
<span data-ttu-id="14480-134">Namísto nutnosti poskytnout své vlastní ověřování toolocal zdroje sady runbook, můžete zadat **RunAs** účet pro skupinu hybridních pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="14480-134">Instead of having runbooks provide their own authentication toolocal resources, you can specify a **RunAs** account for a Hybrid worker group.</span></span>  <span data-ttu-id="14480-135">Zadáte [asset přihlašovacích údajů](automation-credentials.md) který má přístup k prostředkům toolocal a všechny sady runbook spouštět tyto přihlašovací údaje při spuštění v hybridní pracovní proces Runbooku ve skupině hello.</span><span class="sxs-lookup"><span data-stu-id="14480-135">You specify a [credential asset](automation-credentials.md) that has access toolocal resources, and all runbooks run under these credentials when running on a Hybrid Runbook Worker in hello group.</span></span>  

<span data-ttu-id="14480-136">Hello uživatelské jméno pro přihlašovací údaje hello musí být v jednom z následujících formátů hello:</span><span class="sxs-lookup"><span data-stu-id="14480-136">hello user name for hello credential must be in one of hello following formats:</span></span>

* <span data-ttu-id="14480-137">doména\uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="14480-137">domain\username</span></span>
* username@domain
* <span data-ttu-id="14480-138">uživatelské jméno (pro účty místní toohello na místní počítač)</span><span class="sxs-lookup"><span data-stu-id="14480-138">username (for accounts local toohello on-premises computer)</span></span>

<span data-ttu-id="14480-139">Použijte následující postup toospecify účet RunAs pro skupinu hybridních pracovních procesů hello:</span><span class="sxs-lookup"><span data-stu-id="14480-139">Use hello following procedure toospecify a RunAs account for a Hybrid worker group:</span></span>

1. <span data-ttu-id="14480-140">Vytvoření [asset přihlašovacích údajů](automation-credentials.md) s prostředky toolocal přístup.</span><span class="sxs-lookup"><span data-stu-id="14480-140">Create a [credential asset](automation-credentials.md) with access toolocal resources.</span></span>
2. <span data-ttu-id="14480-141">V hello portálu Azure otevřete účet Automation hello.</span><span class="sxs-lookup"><span data-stu-id="14480-141">Open hello Automation account in hello Azure portal.</span></span>
3. <span data-ttu-id="14480-142">Vyberte hello **skupinám Hybrid Worker** dlaždici a pak vyberte skupinu hello.</span><span class="sxs-lookup"><span data-stu-id="14480-142">Select hello **Hybrid Worker Groups** tile, and then select hello group.</span></span>
4. <span data-ttu-id="14480-143">Vyberte **všechna nastavení** a potom **nastavení skupiny hybridních pracovních**.</span><span class="sxs-lookup"><span data-stu-id="14480-143">Select **All settings** and then **Hybrid worker group settings**.</span></span>
5. <span data-ttu-id="14480-144">Změna **spustit jako** z **výchozí** příliš**vlastní**.</span><span class="sxs-lookup"><span data-stu-id="14480-144">Change **Run As** from **Default** too**Custom**.</span></span>
6. <span data-ttu-id="14480-145">Vyberte hello přihlašovacích údajů a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="14480-145">Select hello credential and click **Save**.</span></span>

### <a name="automation-run-as-account"></a><span data-ttu-id="14480-146">Účet Spustit jako automatizace</span><span class="sxs-lookup"><span data-stu-id="14480-146">Automation Run As account</span></span>
<span data-ttu-id="14480-147">Jako součást procesu automatizované sestavení pro nasazení prostředků v Azure možná budete potřebovat přístup k místní tooon systémy toosupport úkolu nebo sady kroků v pořadí nasazení.</span><span class="sxs-lookup"><span data-stu-id="14480-147">As part of your automated build process for deploying resources in Azure, you may require access tooon-premise systems toosupport a task or set of steps in your deployment sequence.</span></span>  <span data-ttu-id="14480-148">toosupport ověřování proti Azure pomocí účtu spustit jako hello, musíte tooinstall hello účet Spustit jako certifikátu.</span><span class="sxs-lookup"><span data-stu-id="14480-148">toosupport authentication against Azure using hello Run As account, you need tooinstall hello Run As account certificate.</span></span>  

<span data-ttu-id="14480-149">Hello následující Powershellový runbook *Export RunAsCertificateToHybridWorker*, exportuje hello spustit jako certifikát z vašeho účtu Azure Automation a soubory ke stažení a naimportuje ho do úložiště certifikátů místního počítače hello na Hybridní pracovní proces připojení toohello stejný účet.</span><span class="sxs-lookup"><span data-stu-id="14480-149">hello following PowerShell runbook, *Export-RunAsCertificateToHybridWorker*, exports hello Run As certificate from your Azure Automation account and downloads and imports it into hello local machine certificate store on a Hybrid worker connected toohello same account.</span></span>  <span data-ttu-id="14480-150">Po dokončení tohoto kroku, tak ověří, že pracovní hello může úspěšně ověřit tooAzure pomocí účtu spustit jako hello.</span><span class="sxs-lookup"><span data-stu-id="14480-150">Once that step is completed, it verifies hello worker can successfully authenticate tooAzure using hello Run As account.</span></span>

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
    Exports hello Run As certificate from an Azure Automation account tooa hybrid worker in that account. 
  
    .DESCRIPTION  
    This runbook exports hello Run As certificate from an Azure Automation account tooa hybrid worker in that account.
    Run this runbook in hello hybrid worker where you want hello certificate installed.
    This allows hello use of hello AzureRunAsConnection tooauthenticate tooAzure and manage Azure resources from runbooks running in hello hybrid worker.

    .EXAMPLE
    .\Export-RunAsCertificateToHybridWorker

    .NOTES
    AUTHOR: Azure Automation Team 
    LASTEDIT: 2016.10.13
    #>

    [OutputType([string])] 

    # Set hello password used for this certificate
    $Password = "YourStrongPasswordForTheCert"

    # Stop on errors
    $ErrorActionPreference = 'stop'

    # Get hello management certificate that will be used toomake calls into Azure Service Management resources
    $RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"
       
    # location toostore temporary certificate in hello Automation service host
    $CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"
   
    # Save hello certificate
    $Cert = $RunAsCert.Export("pfx",$Password)
    Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose 

    Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
    $SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
    Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

    # Test that authentication tooAzure Resource Manager is working
    $RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection" 
    
    Add-AzureRmAccount `
      -ServicePrincipal `
      -TenantId $RunAsConnection.TenantId `
      -ApplicationId $RunAsConnection.ApplicationId `
      -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

    Set-AzureRmContext -SubscriptionId $RunAsConnection.SubscriptionID | Write-Verbose

    # List automation accounts tooconfirm Azure Resource Manager calls are working
    Get-AzureRmAutomationAccount | Select AutomationAccountName

<span data-ttu-id="14480-151">Uložit hello *Export RunAsCertificateToHybridWorker* runbook tooyour počítač s `.ps1` rozšíření.</span><span class="sxs-lookup"><span data-stu-id="14480-151">Save hello *Export-RunAsCertificateToHybridWorker* runbook tooyour computer with a `.ps1` extension.</span></span>  <span data-ttu-id="14480-152">Importujte ho do vašeho účtu Automation a upravit runbook hello, změna hello hello proměnné `$Password` s vlastní heslo.</span><span class="sxs-lookup"><span data-stu-id="14480-152">Import it into your Automation account and edit hello runbook, changing hello value of hello variable `$Password` with your own password.</span></span>  <span data-ttu-id="14480-153">Publikování a znovu spusťte sady runbook hello cílení hello skupinu hybridních pracovních procesů, které spustit a ověření runbooků pomocí účtu spustit jako hello.</span><span class="sxs-lookup"><span data-stu-id="14480-153">Publish and then run hello runbook targeting hello Hybrid Worker group that run and authenticate runbooks using hello Run As account.</span></span>  <span data-ttu-id="14480-154">Hello úlohy datový proud sestavy hello pokus o tooimport hello certifikát do úložiště místního počítače hello a způsobem s více řádky v závislosti na tom, kolik účty Automation jsou definovány v rámci vašeho předplatného, a pokud je ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="14480-154">hello job stream reports hello attempt tooimport hello certificate into hello local machine store, and follows with multiple lines depending on how many Automation accounts are defined in your subscription and if authentication is successful.</span></span>  

## <a name="troubleshooting-runbooks-on-hybrid-runbook-worker"></a><span data-ttu-id="14480-155">Řešení potíží s sady runbook pro Hybrid Runbook Worker.</span><span class="sxs-lookup"><span data-stu-id="14480-155">Troubleshooting runbooks on Hybrid Runbook Worker</span></span>
<span data-ttu-id="14480-156">Protokoly se ukládají místně na každém hybridní pracovní proces na C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.</span><span class="sxs-lookup"><span data-stu-id="14480-156">Logs are stored locally on each hybrid worker at C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.</span></span>  <span data-ttu-id="14480-157">Hybridní pracovní proces taky zaznamenává chyby a události v protokolu událostí systému Windows hello pod **aplikace a služby Logs\Microsoft-SMA\Operational**.</span><span class="sxs-lookup"><span data-stu-id="14480-157">Hybrid worker also records errors and events in hello Windows event log under **Application and Services Logs\Microsoft-SMA\Operational**.</span></span>  <span data-ttu-id="14480-158">Události související s toorunbooks provést u hello pracovní zapisují příliš**aplikace a služby Logs\Microsoft-Automation\Operational**.</span><span class="sxs-lookup"><span data-stu-id="14480-158">Events related toorunbooks executed on hello worker are written too**Application and Services Logs\Microsoft-Automation\Operational**.</span></span>  <span data-ttu-id="14480-159">Hello **Microsoft SMA** protokolu zahrnuje mnoho další události související toohello zpracování sad runbook úlohy stisknutí toohello worker a hello hello sady runbook.</span><span class="sxs-lookup"><span data-stu-id="14480-159">hello **Microsoft-SMA** log includes many more events related toohello runbook job pushed toohello worker and hello processing of hello runbook.</span></span>  <span data-ttu-id="14480-160">Při hello **automatizace Microsoft** protokolu událostí s podrobnostmi s hello řešení potíží s spuštění sady runbook, které nemá mnoho událostí, zjistíte alespoň hello výsledky úlohy runbooku hello.</span><span class="sxs-lookup"><span data-stu-id="14480-160">While hello **Microsoft-Automation** event log does not have many events with details assisting with hello troubleshooting of runbook execution, you will at least find hello results of hello runbook job.</span></span>  

<span data-ttu-id="14480-161">[Runbook výstup a zprávy](automation-runbook-output-and-messages.md) odešlou tooAzure Automation z hybridní pracovní procesy jenom jako úlohy sady runbook spustit v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="14480-161">[Runbook output and messages](automation-runbook-output-and-messages.md) are sent tooAzure Automation from hybrid workers just like runbook jobs run in hello cloud.</span></span>  <span data-ttu-id="14480-162">Můžete také povolit hello podrobné a datových proudů průběh hello stejný způsobem, jakým byste pro jiné sady runbook.</span><span class="sxs-lookup"><span data-stu-id="14480-162">You can also enable hello Verbose and Progress streams hello same way you would for other runbooks.</span></span>  

<span data-ttu-id="14480-163">Pokud vaše sady runbook nejsou nelze úspěšně dokončit a souhrn hello úlohy zobrazuje stav **pozastaveno**, najdete v tématu Poradce při potížích s článku hello [Hybrid Runbook Worker: ukončí úlohy runbooku se stavem Pozastaveno](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).</span><span class="sxs-lookup"><span data-stu-id="14480-163">If your runbooks are not completing successfully and hello job summary shows a status of **Suspended**, please review hello troubleshooting article [Hybrid Runbook Worker: A runbook job terminates with a status of Suspended](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).</span></span>   

## <a name="next-steps"></a><span data-ttu-id="14480-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="14480-164">Next steps</span></span>
* <span data-ttu-id="14480-165">toolearn Další informace o hello různé metody, které se dají použít toostart sady runbook, najdete v části [spuštění sady Runbook ve službě Azure Automation](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="14480-165">toolearn more about hello different methods that can be used toostart a runbook, see [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md).</span></span>  
* <span data-ttu-id="14480-166">toounderstand hello různé postupy pro práci se sadami runbook Powershellu a pracovní postup prostředí PowerShell ve službě Azure Automation pomocí hello textový editor, najdete v části [úpravy sady Runbook ve službě Azure Automation](automation-edit-textual-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="14480-166">toounderstand hello different procedures for working with PowerShell and PowerShell Workflow runbooks in Azure Automation using hello textual editor, see [Editing a Runbook in Azure Automation](automation-edit-textual-runbook.md)</span></span>
