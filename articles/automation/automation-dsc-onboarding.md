---
title: "aaaOnboarding počítačů pro správu Azure Automation DSC. | Microsoft Docs"
description: "Jak toosetup počítačů pro správu pomocí Azure Automation DSC."
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
ms.assetid: da13e1f5-2a1c-443b-8e3b-9f0d6f9e4810
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 12/13/2016
ms.author: eslesar
ms.openlocfilehash: ef15801fec2ffea4ba62dcba2fbe9af09268e424
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="onboarding-machines-for-management-by-azure-automation-dsc"></a>Registrace počítačů pro správu Azure Automation DSC.

## <a name="why-manage-machines-with-azure-automation-dsc"></a>Proč spravovat počítače s Azure Automation DSC?

Jako [konfigurace požadovaného stavu prostředí PowerShell](https://technet.microsoft.com/library/dn249912.aspx), konfigurace požadovaného stavu aplikace Azure Automation je jednoduché, ale výkonné, služba správy konfigurace pro uzly DSC (fyzické a virtuální počítače) ve všech cloudu nebo místně v datovém centru. Umožňuje škálovatelnost napříč tisíce počítačů snadno a rychle z centrální a zabezpečené umístění. Můžete snadno připojit počítače, přiřaďte jim deklarativní konfigurace a zobrazovat sestavy zobrazující každý počítač je toohello požadovaného stavu dodržování předpisů, které jste zadali. vrstva správy Hello Azure Automation DSC je tooDSC co vrstva správy Azure Automation hello je tooPowerShell skriptování. Jinými slovy v hello stejným způsobem, který Azure Automation umožňuje spravovat skripty prostředí PowerShell, také pomáhá při správě konfigurace DSC. toolearn Další informace o hello výhody použití Azure Automation DSC, najdete v části [přehled Azure Automation DSC](automation-dsc-overview.md).

Azure Automation DSC může být použité toomanage různých počítačích:

* Virtuální počítače Azure (klasický)
* Virtuální počítače Azure
* Virtuální počítače Amazon Web Services (AWS)
* Windows fyzický nebo virtuální počítače na místních počítačích nebo v cloudu, než Azure nebo AWS
* Linux fyzický nebo virtuální počítače na místních počítačích v Azure nebo v cloudu, než Azure

Kromě toho pokud si nejste připravené toomanage konfiguraci počítače z cloudu hello, Azure Automation DSC lze také jako koncový bod pouze sestavy. To vám umožní tooset (push) požadované konfigurace pomocí DSC na místě a bohaté generování sestav zobrazit podrobnosti o dodržování předpisů uzlu s hello požadovaného stavu ve službě Azure Automation.

Hello následující části popisují, jak můžete připojit každý typ počítač tooAzure Automation DSC.

## <a name="azure-virtual-machines-classic"></a>Virtuální počítače Azure (klasický)

S Azure Automation DSC můžete snadno připojit virtuální počítače Azure (klasický) pro správu konfigurace pomocí hello portál Azure nebo PowerShell. Pod pokličkou hello a bez nutnosti tooremote do hello virtuálních počítačů správce hello konfigurace požadovaného stavu aplikace Azure virtuálního počítače rozšíření registruje hello virtuálních počítačů Azure Automation DSC. Vzhledem k tomu, že hello rozšíření konfigurace požadovaného stavu aplikace Azure virtuální počítač spustí asynchronně, kroky tootrack jejím průběhu nebo vyřešit potíže s jsou uvedeny v hello [ **řešení potíží s Azure virtuálního počítače registrace** ](#troubleshooting-azure-virtual-machine-onboarding)části níže.

### <a name="azure-portal"></a>portál Azure

V hello [portál Azure](http://portal.azure.com/), klikněte na tlačítko **Procházet** -> **virtuálních počítačů (klasické)**. Vyberte hello tooonboard chcete virtuální počítač s Windows. V okně řídicí panel hello virtuální počítač, klikněte na tlačítko **všechna nastavení** -> **rozšíření** -> **přidat**  ->   **Služby Azure Automation DSC** -> **vytvořit**. Zadejte hello [správce místní konfigurace DSC prostředí PowerShell hodnoty](https://msdn.microsoft.com/powershell/dsc/metaconfig4) požadované pro váš případ použití, účet Automation registrační klíč a adresa URL registrace a volitelně toohello tooassign konfigurace uzlu virtuálního počítače.

![](./media/automation-dsc-onboarding/DSC_Onboarding_1.png)

Adresa URL registrace hello toofind a klíč pro počítač hello hello automatizace účet tooonboard, najdete v části hello [ **zabezpečení registrace** ](#secure-registration) části níže.

### <a name="powershell"></a>PowerShell

```powershell
# log in tooboth Azure Service Management and Azure Resource Manager
Add-AzureAccount
Add-AzureRmAccount

# fill in correct values for your VM/Automation account here
$VMName = ""
$ServiceName = ""
$AutomationAccountName = ""
$AutomationAccountResourceGroup = ""

# fill in hello name of a Node Configuration in Azure Automation DSC, for this VM tooconform to
$NodeConfigName = ""

# get Azure Automation DSC registration info
$Account = Get-AzureRmAutomationAccount -ResourceGroupName $AutomationAccountResourceGroup -Name $AutomationAccountName
$RegistrationInfo = $Account | Get-AzureRmAutomationRegistrationInfo

# use hello DSC extension tooonboard hello VM for management with Azure Automation DSC
$VM = Get-AzureVM -Name $VMName -ServiceName $ServiceName

$PublicConfiguration = ConvertTo-Json -Depth 8 @{
    SasToken = ""
    ModulesUrl = "https://eus2oaasibizamarketprod1.blob.core.windows.net/automationdscpreview/RegistrationMetaConfigV2.zip"
    ConfigurationFunction = "RegistrationMetaConfigV2.ps1\RegistrationMetaConfigV2"

# update these PowerShell DSC Local Configuration Manager defaults if they do not match your use case.
# See https://technet.microsoft.com/library/dn249922.aspx?f=255&MSPPError=-2147217396 for more details
    Properties = @{
    RegistrationKey = @{
        UserName = 'notused'
        Password = 'PrivateSettingsRef:RegistrationKey'
    }
    RegistrationUrl = $RegistrationInfo.Endpoint
    NodeConfigurationName = $NodeConfigName
    ConfigurationMode = "ApplyAndMonitor"
    ConfigurationModeFrequencyMins = 15
    RefreshFrequencyMins = 30
    RebootNodeIfNeeded = $False
    ActionAfterReboot = "ContinueConfiguration"
    AllowModuleOverwrite = $False
    }
}

$PrivateConfiguration = ConvertTo-Json -Depth 8 @{
    Items = @{
        RegistrationKey = $RegistrationInfo.PrimaryKey
    }
}

$VM = Set-AzureVMExtension `
    -VM $vm `
    -Publisher Microsoft.Powershell `
    -ExtensionName DSC `
    -Version 2.19 `
    -PublicConfiguration $PublicConfiguration `
    -PrivateConfiguration $PrivateConfiguration `
    -ForceUpdate

$VM | Update-AzureVM
```

## <a name="azure-virtual-machines"></a>Virtuální počítače Azure

Azure Automation DSC umožňuje snadno připojit virtuální počítače Azure za účelem správy konfigurace, pomocí hello portálu Azure, šablon Azure Resource Manageru nebo prostředí PowerShell. Pod pokličkou hello a bez nutnosti tooremote do hello virtuálních počítačů správce hello konfigurace požadovaného stavu aplikace Azure virtuálního počítače rozšíření registruje hello virtuálních počítačů Azure Automation DSC. Vzhledem k tomu, že hello rozšíření konfigurace požadovaného stavu aplikace Azure virtuální počítač spustí asynchronně, kroky tootrack jejím průběhu nebo vyřešit potíže s jsou uvedeny v hello [ **řešení potíží s Azure virtuálního počítače registrace** ](#troubleshooting-azure-virtual-machine-onboarding)části níže.

### <a name="azure-portal"></a>portál Azure

V hello [portál Azure](https://portal.azure.com/), přejděte účet Azure Automation toohello místo tooonboard virtuálních počítačů. Na řídicím panelu účet Automation hello, klikněte na tlačítko **uzly DSC** -> **přidat virtuální počítač Azure**.

V části **vyberte virtuální počítače tooonboard**, vyberte jednu nebo více Azure virtuální počítače tooonboard.

![](./media/automation-dsc-onboarding/DSC_Onboarding_2.png)

V části **konfigurace registrační data**, zadejte hello [správce místní konfigurace DSC prostředí PowerShell hodnoty](https://msdn.microsoft.com/powershell/dsc/metaconfig4) vyžadované pro váš případ použití a volitelně toohello tooassign konfigurace uzlu virtuálního počítače.

![](./media/automation-dsc-onboarding/DSC_Onboarding_3.png)

### <a name="azure-resource-manager-templates"></a>Šablony Azure Resource Manageru

Virtuální počítače Azure můžete nasadit a zařazený nemá tooAzure Automation DSC pomocí šablony Azure Resource Manager. V tématu [konfigurace virtuálního počítače prostřednictvím rozšíření DSC a Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) pro šablonu příklad této onboards existující virtuální počítač tooAzure Automation DSC. toofind hello registrační klíč a registraci adresy URL prováděné jako vstup v této šabloně najdete v části hello [ **zabezpečení registrace** ](#secure-registration) části níže.

### <a name="powershell"></a>PowerShell

Hello [Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) rutiny lze použít tooonboard virtuálních počítačů v hello portálu Azure pomocí prostředí PowerShell.

## <a name="amazon-web-services-aws-virtual-machines"></a>Virtuální počítače Amazon Web Services (AWS)

Můžete snadno připojit Amazon Web Services virtuálních počítačů pro správu konfigurace pomocí hello AWS DSC Toolkit Azure Automation DSC. Další informace o hello toolkit [zde](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws"></a>Windows fyzický nebo virtuální počítače na místních počítačích nebo v cloudu, než Azure nebo AWS

Místní počítače s Windows a počítače s Windows v cloudech mimo Azure (například Amazon Web Services) může být také zařazený nemá tooAzure Automation DSC, tak dlouho, dokud mají toohello odchozí přístup k Internetu prostřednictvím několika jednoduchých kroků:

1. Ujistěte se, zda text hello nejnovější verzi [WMF 5](http://aka.ms/wmf5latest) je nainstalován na počítačích hello chcete tooonboard tooAzure Automation DSC.
2. Postupujte podle pokynů hello v části [ **generování DSC metaconfigurations** ](#generating-dsc-metaconfigurations) pod toogenerate složku obsahující hello potřeby DSC metaconfigurations.
3. Vzdáleně použijte hello DSC Powershellu metakonfiguraci toohello počítače chcete tooonboard. **Tento příkaz spustí z počítače Hello musí mít hello nejnovější verze [WMF 5](http://aka.ms/wmf5latest) nainstalován**:

    ```powershell
    Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
    ```

4. Pokud nemůžete použít metaconfigurations hello DSC Powershellu vzdáleně, zkopírujte složku metaconfigurations hello z kroku 2 na každý počítač tooonboard. Potom zavolejte **Set-DscLocalConfigurationManager** místně na každý počítač tooonboard.
5. Použití hello portál Azure nebo rutin zkontrolujte, že hello počítače tooonboard nyní se zobrazí jako uzly DSC registrované ve vašem účtu Azure Automation.

## <a name="physicalvirtual-linux-machines-on-premises-in-azure-or-in-a-cloud-other-than-azure"></a>Linux fyzický nebo virtuální počítače na místních počítačích v Azure nebo v cloudu, než Azure

Linux místní počítače, počítače s Linuxem v Azure, a počítače se systémem Linux v cloudech mimo Azure může být také zařazený nemá tooAzure Automation DSC, tak dlouho, dokud mají toohello odchozí přístup k Internetu prostřednictvím několika jednoduchých kroků:

1. Ujistěte se, zda text hello nejnovější verzi [prostředí PowerShell konfigurace požadovaného stavu pro Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) je nainstalován na počítačích hello chcete tooonboard tooAzure Automation DSC.
2. Pokud hello [správce místní konfigurace DSC prostředí PowerShell výchozí](https://msdn.microsoft.com/powershell/dsc/metaconfig4) odpovídající váš případ použití, a chcete, aby tooonboard počítače tak, že se **obě** načítat z a informovat tooAzure Automation DSC:

   + Na každý Linux počítač tooonboard tooAzure Automation DSC použijte Register.py tooonboard pomocí hello výchozí správce místní konfigurace DSC prostředí PowerShell:

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   + toofind hello registrační klíč a registraci adresy URL u vašeho účtu Automation, najdete v části hello [ **zabezpečení registrace** ](#secure-registration) části níže.

     Pokud hello správce místní konfigurace DSC prostředí PowerShell výchozí **provést** **není** odpovídat váš případ použití, nebo chcete tooonboard počítače tak, aby se pouze sestavy tooAzure Automation DSC, ale není pro vyžádání obsahu konfigurace nebo moduly Powershellu z něj, postupujte podle kroků 3 až 6. Jinak přejděte přímo toostep 6.

3. Postupujte podle pokynů hello v hello [ **generování DSC metaconfigurations** ](#generating-dsc-metaconfigurations) části níže toogenerate složku obsahující potřebné hello DSC metaconfigurations.
4. Vzdáleně použít hello DSC Powershellu metakonfiguraci toohello počítače chcete tooonboard:

    ```powershell
    $SecurePass = ConvertTo-SecureString -String "<root password>" -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential "root", $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine tooonboard

    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

Tento příkaz spustí z počítače Hello musí mít hello nejnovější verze [WMF 5](http://aka.ms/wmf5latest) nainstalována.

1. Pokud nemůžete použít metaconfigurations hello DSC Powershellu vzdáleně, pro každý počítač tooonboard Linux, zkopírujte ze složky hello v kroku 5 do počítače Linux hello hello metakonfiguraci odpovídající toothat počítač. Potom zavolejte `SetDscLocalConfigurationManager.py` místně na každém počítači Linux chcete tooonboard tooAzure Automation DSC:

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path toometaconfiguration file>`

2. Použití hello portál Azure nebo rutin zkontrolujte, že hello počítače tooonboard nyní se zobrazí jako uzly DSC registrované ve vašem účtu Azure Automation.

## <a name="generating-dsc-metaconfigurations"></a>Generování DSC metaconfigurations

toogenerically zaváděním některý počítač tooAzure Automation DSC, [metakonfiguraci DSC](https://msdn.microsoft.com/en-us/powershell/dsc/metaconfig) může být, vygeneruje, když použijí, říká agent hello DSC na počítače toopull hello z nebo sestavy tooAzure Automation DSC. Metaconfigurations DSC pro Azure Automation DSC můžete vygenerovat pomocí buď konfigurace DSC prostředí PowerShell nebo rutin Powershellu pro Azure Automation hello.

> [!NOTE]
> DSC metaconfigurations obsahovat hello tajné klíče potřeby tooonboard počítač tooan účet automatizace pro správu. Ujistěte se, že tooproperly chránit všechny metaconfigurations DSC, které vytvoříte, nebo je odstranit po použití.

### <a name="using-a-dsc-configuration"></a>Používání konfigurace DSC

1. V počítači ve vašem místním prostředí otevřete hello ISE prostředí PowerShell jako správce. Hello počítač musí mít nejnovější verzi hello [WMF 5](http://aka.ms/wmf5latest) nainstalována.
2. Zkopírujte následující skript místně hello. Tento skript obsahuje konfiguraci DSC prostředí PowerShell pro vytváření metaconfigurations a tookick příkaz vypnout vytváření metakonfiguraci hello.

    ```powershell
    # hello DSC configuration that will generate metaconfigurations
    [DscLocalConfigurationManager()]
    Configuration DscMetaConfigs
    {

        param
        (
            [Parameter(Mandatory=$True)]
            [String]$RegistrationUrl,

            [Parameter(Mandatory=$True)]
            [String]$RegistrationKey,

            [Parameter(Mandatory=$True)]
            [String[]]$ComputerName,

            [Int]$RefreshFrequencyMins = 30,

            [Int]$ConfigurationModeFrequencyMins = 15,

            [String]$ConfigurationMode = "ApplyAndMonitor",

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = "ContinueConfiguration",

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq "")
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
        $RefreshMode = "PUSH"
        }
        else
        {
        $RefreshMode = "PULL"
        }

        Node $ComputerName
        {

            Settings
            {
                RefreshFrequencyMins = $RefreshFrequencyMins
                RefreshMode = $RefreshMode
                ConfigurationMode = $ConfigurationMode
                AllowModuleOverwrite = $AllowModuleOverwrite
                RebootNodeIfNeeded = $RebootNodeIfNeeded
                ActionAfterReboot = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationDSC
                {
                    ServerUrl = $RegistrationUrl
                    RegistrationKey = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationDSC
                {
                ServerUrl = $RegistrationUrl
                RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationDSC
            {
                ServerUrl = $RegistrationUrl
                RegistrationKey = $RegistrationKey
            }
        }
    }

    # Create hello metaconfigurations
    # TODO: edit hello below as needed for your use case
    $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM tooonboard>', '<some other VM tooonboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set too$True toohave machines only report tooAA DSC but not pull from it
    }

    # Use PowerShell splatting toopass parameters toohello DSC configuration being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    DscMetaConfigs @Params
    ```

3. Vyplňte hello registrační klíč a adresu URL pro váš účet Automation, jakož i hello názvy počítačů tooonboard hello. Všechny ostatní parametry jsou volitelné. toofind hello registrační klíč a registraci adresy URL u vašeho účtu Automation, najdete v části hello [ **zabezpečení registrace** ](#secure-registration) části níže.
4. Pokud chcete hello počítače tooreport DSC stavové informace tooAzure Automation DSC, ale není pro vyžádání obsahu konfigurace nebo moduly Powershellu, nastavte hello **ReportOnly** tootrue parametr.
5. Spusťte skript hello. Teď byste měli mít složku s názvem **DscMetaConfigs** v pracovním adresáři, který obsahuje metaconfigurations hello PowerShell DSC pro počítače tooonboard hello (jako správce):

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-hello-azure-automation-cmdlets"></a>Pomocí rutin hello Azure Automation.

Pokud výchozí nastavení správce místní konfigurace DSC prostředí PowerShell hello odpovídá váš případ použití a chcete tooonboard počítače tak, aby se načítat z i vykazování tooAzure Automation DSC, rutiny Azure Automation hello poskytují zjednodušenou metodu generování hello DSC metaconfigurations potřebné:

1. Otevřete konzolu PowerShell hello nebo ISE prostředí PowerShell jako správce v počítači ve vašem místním prostředí.
2. Připojit pomocí Správce prostředků tooAzure **Add-AzureRmAccount**
3. Stáhněte metaconfigurations hello PowerShell DSC pro počítače hello chcete tooonboard z hello automatizace účet toowhich chcete tooonboard uzly:

    ```powershell
    # Define hello parameters for Get-AzureRmAutomationDscOnboardingMetaconfig using PowerShell Splatting
    $Params = @{

        ResourceGroupName = 'ContosoResources'; # hello name of hello ARM Resource Group that contains your Azure Automation Account
        AutomationAccountName = 'ContosoAutomation'; # hello name of hello Azure Automation Account where you want a node on-boarded to
        ComputerName = @('web01', 'web02', 'sql01'); # hello names of hello computers that hello meta configuration will be generated for
        OutputFolder = "$env:UserProfile\Desktop\";
    }
    # Use PowerShell splatting toopass parameters toohello Azure Automation cmdlet being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    Get-AzureRmAutomationDscOnboardingMetaconfig @Params
    ```
    
4. Teď byste měli mít složku s názvem ***DscMetaConfigs***, obsahující hello metaconfigurations PowerShell DSC pro počítače tooonboard hello (jako správce):
    
    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a>Zabezpečené registrace

Počítače můžete bezpečně zařadit tooan účet Azure Automation prostřednictvím hello WMF 5 DSC registrace protokol, který umožňuje DSC uzlu tooauthenticate tooa prostředí PowerShell DSC V2 Pull nebo vytváření sestav serveru (včetně Azure Automation DSC). uzel Hello zaregistruje server toohello na **URL pro registraci**, ověřování pomocí **registrační klíč**. Během registrace vyjednat jedinečný certifikát pro tento uzel toouse pro ověřování toohello serveru po registraci uzlu hello DSC a server pro vyžádání obsahu nebo sestavy DSC. Tento proces zabraňuje zařazený nemá uzly z jednoho jiné, jako je například pokud uzel dojde k ohrožení bezpečnosti zosobnění a chovají závadně. Po registraci hello registrační klíč se nepoužije pro ověřování znovu a se odstraní z uzlu hello.

Můžete získat hello informace požadované pro protokol registrace hello DSC z hello **Správa klíčů** okno na portálu Azure preview hello. Otevřete toto okno kliknutím na ikonu klíče hello na hello **Essentials** panel pro hello účet Automation.

![](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

* Adresa URL registrace je hello pole adresy URL v okně Správa klíčů hello.
* Registrační klíč je hello primární přístupový klíč nebo sekundární přístupový klíč v okně Správa klíčů hello. Můžete použít buď klíč.

Pro zvýšení zabezpečení hello primární a sekundární přístupové klíče účtu Automation může být kdykoli znovu vygenerovat (na hello **Správa klíčů** okno) tooprevent budoucí uzlu registrace pomocí předchozí klíče.

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>Řešení potíží s registrace virtuální počítač Azure

Azure Automation DSC umožňuje snadno připojit virtuální počítače Azure Windows pro správu konfigurace. Pod pokličkou hello hello rozšíření konfigurace požadovaného stavu aplikace Azure virtuálního počítače je použité tooregister hello virtuálního počítače s Azure Automation DSC. Vzhledem k tomu, že hello rozšíření konfigurace požadovaného stavu aplikace Azure virtuální počítač se spustí asynchronně, může být důležité jejím průběhu sledování a řešení potíží s jeho spuštění.

> [!NOTE]
> Jakékoli metody objektu registrace, které tooAzure virtuálního počítače Windows Azure Automation DSC, využívající rozšíření hello konfigurace požadovaného stavu aplikace Azure virtuálních počítačů může trvat až hodinu tooan hello uzlu tooshow až as registrované ve službě Azure Automation. Toto je z důvodu toohello instalaci Windows Management Framework 5.0 na hello virtuálního počítače pomocí rozšíření hello virtuálních počítačů DSC Azure, které je požadované tooonboard hello virtuálních počítačů tooAzure Automation DSC.

tootroubleshoot nebo zobrazení stavu hello hello rozšíření konfigurace požadovaného stavu aplikace Azure virtuálních počítačů v hello portálu Azure přejděte toohello se zařazený nemá virtuální počítač, pak klikněte na -> **všechna nastavení** -> **rozšíření**   ->  **DSC**. Další podrobnosti, můžete kliknout na **zobrazit podrobné informace o stavu**.

[![](./media/automation-dsc-onboarding/DSC_Onboarding_5.png)](https://technet.microsoft.com/library/dn249912.aspx)

## <a name="certificate-expiration-and-reregistration"></a>Vypršení platnosti certifikátu a registrace

Po registraci počítače s jako uzel DSC v Azure Automation DSC, jsou z mnoha důvodů může být nutná tooreregister tento uzel v hello budoucí:

* Po registraci, automatické každý uzel jedinečný certifikát pro ověřování, jejíž platnost vyprší po jednom roce. V současné době hello DSC Powershellu registrace protokol nelze obnovit automaticky certifikáty při jejich blížícím se koncem platnosti, takže je třeba tooreregister hello uzly po času v roce. Před opětovná registrace, ujistěte se, že každý uzel běží Windows Management Framework 5.0 RTM. Pokud vyprší platnost certifikátu ověřování uzlu, a není znovu zaregistruje hello uzel, uzel hello bude nelze toocommunicate s Azure Automation a budou označeny 'Unresponsive.' Opětovná provést 90 dní nebo méně z hello čas vypršení platnosti certifikátu, nebo kdykoli po hello čas vypršení platnosti certifikátu, bude výsledkem nový certifikát se generovat a používat.
* toochange žádné [správce místní konfigurace DSC prostředí PowerShell hodnoty](https://msdn.microsoft.com/powershell/dsc/metaconfig4) které byly nastavené při počáteční registraci hello uzlu, jako je například ConfigurationMode. V současné době můžete tyto hodnoty agenta DSC změnit jenom prostřednictvím registrace. Jedinou výjimkou Hello je hello konfigurace uzlu přiřazen uzlu toohello – přímo lze změnit v Azure Automation DSC.

Registrace lze provést v hello stejným způsobem jako registrovat hello uzlu na začátku pomocí libovolné metody registrace hello popsané v tomto dokumentu. Před opětovná registrace ji nepotřebujete toounregister uzlu z Azure Automation DSC.

## <a name="related-articles"></a>Související články

* [Přehled služby Azure Automation DSC](automation-dsc-overview.md)
* [Rutiny Azure Automation DSC](/powershell/module/azurerm.automation/#automation)
* [Ceny služby Azure Automation DSC](https://azure.microsoft.com/pricing/details/automation/)
