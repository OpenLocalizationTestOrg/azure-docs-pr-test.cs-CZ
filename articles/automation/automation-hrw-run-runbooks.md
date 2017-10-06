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
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Spuštěné sady runbook pro Hybrid Runbook Worker. 
Není žádný rozdíl ve struktuře hello sad runbook, které běží v Azure Automation a ty, které běží na hybridní pracovní proces Runbooku. Sady Runbook, které můžete použít pro každý s největší pravděpodobností se podstatně liší ale vzhledem k tomu obvykle cílení hybridní pracovní proces Runbooku sady runbook spravovat prostředky na místním počítači hello, sám sebe nebo s prostředky v hello místní prostředí, kde je nasazen, při sady runbook ve službě Azure Automation obvykle spravovat prostředky v cloudu Azure hello.

Můžete upravit sady runbook pro hybridní pracovní proces Runbooku ve službě Azure Automation, ale mohou mít problémy, pokud se pokusíte tootest hello runbook v editoru hello.  Hello moduly Powershellu, které přístup k místním prostředkům hello se nenainstaluje ve vašem prostředí Azure Automation. v takovém případě, hello test skončí s chybou.  Pokud nainstalujete hello požadované moduly, pak spustí hello runbook, ale nebude možné tooaccess místních prostředků pro dokončení testovacího.

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a>Spuštění sady runbook pro Hybrid Runbook Worker.
[Spuštění sady Runbook ve službě Azure Automation](automation-starting-a-runbook.md) popisuje různé metody pro spuštění sady runbook.  Přidá hybridní pracovní proces Runbooku **RunOn** možnost, kde můžete určit název hello hybridní Runbook pracovní skupiny.  Pokud je skupina zadaná, je hello runbooku načíst a spustit hello pracovníků v této skupině.  Pokud není tato možnost zadána, pak spuštění ve službě Azure Automation jako normální.

Při spuštění runbooku v hello portálu Azure, se zobrazí **spustit na** možnost, kde můžete vybrat **Azure** nebo **hybridní pracovní proces**.  Pokud vyberete **hybridní pracovní proces**, pak můžete vybrat skupinu hello z rozevírací seznam.

Použití hello **RunOn** parametr.  Můžete použít následující příkaz toostart sady runbook s názvem Test-Runbook na hybridní skupina pracovního procesu Runbook s názvem MyHybridGroup pomocí prostředí Windows PowerShell hello.

    Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"

> [!NOTE]
> Hello **RunOn** byl přidán parametr toohello **Start-AzureAutomationRunbook** rutiny ve verzi 0.9.1 Microsoft Azure PowerShell.  Měli byste [stažení nejnovější verze hello](https://azure.microsoft.com/downloads/) Pokud máte některou z dřívějších nainstalována.  Potřebujete jenom tooinstall touto verzí na pracovní stanici, kde spouštíte hello runbook z prostředí Windows PowerShell.  Není nutné tooinstall ho na počítači pracovní hello Pokud hodláte toostart sady runbook z tohoto počítače.  Nelze spustit aktuálně sady runbook na hybridní pracovní proces Runbooku z jiného runbooku vzhledem k tomu, že bude vyžadovat hello nejnovější verzi prostředí Azure Powershell toobe nainstalovaný ve vašem účtu Automation.  nejnovější verzi Hello je automaticky aktualizovat ve službě Azure Automation a automaticky instaluje dolů toohello pracovníci brzy k dispozici.
>
>

## <a name="runbook-permissions"></a>Oprávnění sady Runbook
Runbook spuštěných v hybridní pracovní proces Runbooku nelze použít hello stejné metody, která se obvykle používá pro ověřování tooAzure prostředky, protože jejich přístupu k prostředkům mimo Azure sady runbook.  Hello runbook můžete buď zadat vlastní ověřování toolocal prostředky, nebo můžete zadat tooprovide účet RunAs kontext uživatele pro všechny sady runbook.

### <a name="runbook-authentication"></a>Ověření sady Runbook
Ve výchozím nastavení bude sady runbook spustit v kontextu hello hello místní systémový účet na místním počítači hello, takže musí zadat své vlastní tooresources ověřování, který bude přistupovat k.  

Můžete použít [pověření](http://msdn.microsoft.com/library/dn940015.aspx) a [certifikát](http://msdn.microsoft.com/library/dn940013.aspx) prostředky ve vašem runbooku pomocí rutin, které umožní toospecify přihlašovací údaje, můžete ověřovat toodifferent prostředky.  Hello následující příklad ukazuje část sady runbook, který restartuje počítač.  Načte přihlašovací údaje z název přihlašovacího údaje asset a hello hello počítače z variabilní prostředek a potom tyto hodnoty používá pomocí rutiny Restart-Computer hello.

    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
    $Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

    Restart-Computer -ComputerName $Computer -Credential $Cred

Můžete také využít [InlineScript](automation-powershell-workflow.md#inlinescript), což vám umožní toorun bloky kódu na jiném počítači pomocí přihlašovacích údajů určeného hello [PSCredential společný parametr](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="runas-account"></a>Účet Spustit jako
Namísto nutnosti poskytnout své vlastní ověřování toolocal zdroje sady runbook, můžete zadat **RunAs** účet pro skupinu hybridních pracovních procesů.  Zadáte [asset přihlašovacích údajů](automation-credentials.md) který má přístup k prostředkům toolocal a všechny sady runbook spouštět tyto přihlašovací údaje při spuštění v hybridní pracovní proces Runbooku ve skupině hello.  

Hello uživatelské jméno pro přihlašovací údaje hello musí být v jednom z následujících formátů hello:

* doména\uživatelské jméno
* username@domain
* uživatelské jméno (pro účty místní toohello na místní počítač)

Použijte následující postup toospecify účet RunAs pro skupinu hybridních pracovních procesů hello:

1. Vytvoření [asset přihlašovacích údajů](automation-credentials.md) s prostředky toolocal přístup.
2. V hello portálu Azure otevřete účet Automation hello.
3. Vyberte hello **skupinám Hybrid Worker** dlaždici a pak vyberte skupinu hello.
4. Vyberte **všechna nastavení** a potom **nastavení skupiny hybridních pracovních**.
5. Změna **spustit jako** z **výchozí** příliš**vlastní**.
6. Vyberte hello přihlašovacích údajů a klikněte na **Uložit**.

### <a name="automation-run-as-account"></a>Účet Spustit jako automatizace
Jako součást procesu automatizované sestavení pro nasazení prostředků v Azure možná budete potřebovat přístup k místní tooon systémy toosupport úkolu nebo sady kroků v pořadí nasazení.  toosupport ověřování proti Azure pomocí účtu spustit jako hello, musíte tooinstall hello účet Spustit jako certifikátu.  

Hello následující Powershellový runbook *Export RunAsCertificateToHybridWorker*, exportuje hello spustit jako certifikát z vašeho účtu Azure Automation a soubory ke stažení a naimportuje ho do úložiště certifikátů místního počítače hello na Hybridní pracovní proces připojení toohello stejný účet.  Po dokončení tohoto kroku, tak ověří, že pracovní hello může úspěšně ověřit tooAzure pomocí účtu spustit jako hello.

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

Uložit hello *Export RunAsCertificateToHybridWorker* runbook tooyour počítač s `.ps1` rozšíření.  Importujte ho do vašeho účtu Automation a upravit runbook hello, změna hello hello proměnné `$Password` s vlastní heslo.  Publikování a znovu spusťte sady runbook hello cílení hello skupinu hybridních pracovních procesů, které spustit a ověření runbooků pomocí účtu spustit jako hello.  Hello úlohy datový proud sestavy hello pokus o tooimport hello certifikát do úložiště místního počítače hello a způsobem s více řádky v závislosti na tom, kolik účty Automation jsou definovány v rámci vašeho předplatného, a pokud je ověření úspěšné.  

## <a name="troubleshooting-runbooks-on-hybrid-runbook-worker"></a>Řešení potíží s sady runbook pro Hybrid Runbook Worker.
Protokoly se ukládají místně na každém hybridní pracovní proces na C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.  Hybridní pracovní proces taky zaznamenává chyby a události v protokolu událostí systému Windows hello pod **aplikace a služby Logs\Microsoft-SMA\Operational**.  Události související s toorunbooks provést u hello pracovní zapisují příliš**aplikace a služby Logs\Microsoft-Automation\Operational**.  Hello **Microsoft SMA** protokolu zahrnuje mnoho další události související toohello zpracování sad runbook úlohy stisknutí toohello worker a hello hello sady runbook.  Při hello **automatizace Microsoft** protokolu událostí s podrobnostmi s hello řešení potíží s spuštění sady runbook, které nemá mnoho událostí, zjistíte alespoň hello výsledky úlohy runbooku hello.  

[Runbook výstup a zprávy](automation-runbook-output-and-messages.md) odešlou tooAzure Automation z hybridní pracovní procesy jenom jako úlohy sady runbook spustit v cloudu hello.  Můžete také povolit hello podrobné a datových proudů průběh hello stejný způsobem, jakým byste pro jiné sady runbook.  

Pokud vaše sady runbook nejsou nelze úspěšně dokončit a souhrn hello úlohy zobrazuje stav **pozastaveno**, najdete v tématu Poradce při potížích s článku hello [Hybrid Runbook Worker: ukončí úlohy runbooku se stavem Pozastaveno](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).   

## <a name="next-steps"></a>Další kroky
* toolearn Další informace o hello různé metody, které se dají použít toostart sady runbook, najdete v části [spuštění sady Runbook ve službě Azure Automation](automation-starting-a-runbook.md).  
* toounderstand hello různé postupy pro práci se sadami runbook Powershellu a pracovní postup prostředí PowerShell ve službě Azure Automation pomocí hello textový editor, najdete v části [úpravy sady Runbook ve službě Azure Automation](automation-edit-textual-runbook.md)
