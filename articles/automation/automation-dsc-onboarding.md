---
title: "Registrace počítačů pro správu Azure Automation DSC. | Microsoft Docs"
description: "Postup nastavení počítačů pro správu pomocí Azure Automation DSC."
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
ms.openlocfilehash: cc9b1ea19b4e17374d47e12f970cb333a8051559
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="onboarding-machines-for-management-by-azure-automation-dsc"></a>Registrace počítačů pro správu Azure Automation DSC.

## <a name="why-manage-machines-with-azure-automation-dsc"></a>Proč spravovat počítače s Azure Automation DSC?

Jako [konfigurace požadovaného stavu prostředí PowerShell](https://technet.microsoft.com/library/dn249912.aspx), konfigurace požadovaného stavu aplikace Azure Automation je jednoduché, ale výkonné, služba správy konfigurace pro uzly DSC (fyzické a virtuální počítače) ve všech cloudu nebo místně v datovém centru. Umožňuje škálovatelnost napříč tisíce počítačů snadno a rychle z centrální a zabezpečené umístění. Můžete snadno připojit počítače, přiřaďte jim deklarativní konfigurace a zobrazovat sestavy zobrazující každý počítač je dodržování předpisů pro požadovaný stav, který jste zadali. Vrstva správy Azure Automation DSC je DSC, co je Azure Automation vrstvy správy skriptů prostředí PowerShell. Jinými slovy v stejným způsobem, který Azure Automation umožňuje spravovat skripty prostředí PowerShell, taky pomáhá při správě konfigurace DSC. Další informace o výhodách používání Azure Automation DSC, najdete v části [přehled Azure Automation DSC](automation-dsc-overview.md).

Azure Automation DSC umožňuje spravovat celou řadu počítače:

* Virtuální počítače Azure (klasický)
* Virtuální počítače Azure
* Virtuální počítače Amazon Web Services (AWS)
* Windows fyzický nebo virtuální počítače na místních počítačích nebo v cloudu, než Azure nebo AWS
* Linux fyzický nebo virtuální počítače na místních počítačích v Azure nebo v cloudu, než Azure

Kromě toho pokud si nejste připravení spravovat konfiguraci počítače z cloudu, Azure Automation DSC lze také jako koncový bod pouze sestavy. Můžete nastavit (push) požadované konfigurace pomocí DSC na místě a zobrazte bohaté reporting podrobnosti o dodržování předpisů uzlu s požadovaný stav ve službě Azure Automation.

Následující oddíly popisují, jak můžete připojit každý typ počítače do Azure Automation DSC.

## <a name="azure-virtual-machines-classic"></a>Virtuální počítače Azure (klasický)

S Azure Automation DSC můžete snadno připojit virtuální počítače Azure (klasický) za účelem správy konfigurace buď pomocí portálu Azure, nebo prostředí PowerShell. Pod pokličkou a bez nutnosti vzdáleného do virtuálního počítače správce zaregistruje rozšíření konfigurace požadovaného stavu aplikace Azure virtuálních počítačů virtuálního počítače se Azure Automation DSC. Vzhledem k tomu, že rozšíření konfigurace požadovaného stavu aplikace Azure virtuální počítač se spustí asynchronně, jsou kroky k jeho průběh sledovat a řešit potíže se součástí [ **řešení potíží s Azure virtuálního počítače registrace** ](#troubleshooting-azure-virtual-machine-onboarding) části.

### <a name="azure-portal"></a>portál Azure

V [portál Azure](http://portal.azure.com/), klikněte na tlačítko **Procházet** -> **virtuálních počítačů (klasické)**. Vyberte virtuální počítač Windows, které chcete zařadit do provozu. V okně virtuálního počítače řídicí panel, klikněte na tlačítko **všechna nastavení** -> **rozšíření** -> **přidat** -> **Azure Automation DSC** -> **vytvořit**. Zadejte [správce místní konfigurace DSC prostředí PowerShell hodnoty](https://msdn.microsoft.com/powershell/dsc/metaconfig4) požadované pro váš případ použití, účet Automation registrační klíč a adresa URL registrace a volitelně konfigurace uzlu přiřadit k virtuálnímu počítači.

![](./media/automation-dsc-onboarding/DSC_Onboarding_1.png)

K vyhledání adresu URL pro registraci a klíče pro účet služby Automation zařadit do počítače, najdete v článku [ **zabezpečení registrace** ](#secure-registration) části níže.

### <a name="powershell"></a>PowerShell

```powershell
# log in to both Azure Service Management and Azure Resource Manager
Add-AzureAccount
Add-AzureRmAccount

# fill in correct values for your VM/Automation account here
$VMName = ""
$ServiceName = ""
$AutomationAccountName = ""
$AutomationAccountResourceGroup = ""

# fill in the name of a Node Configuration in Azure Automation DSC, for this VM to conform to
$NodeConfigName = ""

# get Azure Automation DSC registration info
$Account = Get-AzureRmAutomationAccount -ResourceGroupName $AutomationAccountResourceGroup -Name $AutomationAccountName
$RegistrationInfo = $Account | Get-AzureRmAutomationRegistrationInfo

# use the DSC extension to onboard the VM for management with Azure Automation DSC
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

Azure Automation DSC umožňuje snadno připojit virtuální počítače Azure za účelem správy konfigurace, pomocí portálu Azure, šablon Azure Resource Manageru nebo prostředí PowerShell. Pod pokličkou a bez nutnosti vzdáleného do virtuálního počítače správce zaregistruje rozšíření konfigurace požadovaného stavu aplikace Azure virtuálních počítačů virtuálního počítače se Azure Automation DSC. Vzhledem k tomu, že rozšíření konfigurace požadovaného stavu aplikace Azure virtuální počítač se spustí asynchronně, jsou kroky k jeho průběh sledovat a řešit potíže se součástí [ **řešení potíží s Azure virtuálního počítače registrace** ](#troubleshooting-azure-virtual-machine-onboarding) části.

### <a name="azure-portal"></a>portál Azure

V [portál Azure](https://portal.azure.com/), přejděte na účet Azure Automation, ve které chcete zařadit virtuálních počítačů. Na řídicím panelu účet Automation, klikněte na tlačítko **uzly DSC** -> **přidat virtuální počítač Azure**.

V části **vybrat virtuální počítače chcete zařadit**, vyberte jednu nebo více Azure virtuálních počítačů do zaváděním.

![](./media/automation-dsc-onboarding/DSC_Onboarding_2.png)

V části **konfigurace registrační data**, zadejte [správce místní konfigurace DSC prostředí PowerShell hodnoty](https://msdn.microsoft.com/powershell/dsc/metaconfig4) vyžadované pro váš případ použití a volitelně konfigurace uzlu přiřadit k virtuálnímu počítači.

![](./media/automation-dsc-onboarding/DSC_Onboarding_3.png)

### <a name="azure-resource-manager-templates"></a>Šablony Azure Resource Manageru

Virtuální počítače Azure můžete nasadit a zařazený nemá na Azure Automation DSC pomocí šablony Azure Resource Manager. V tématu [konfigurace virtuálního počítače prostřednictvím rozšíření DSC a Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) pro šablonu příklad této onboards existující virtuální počítač do Azure Automation DSC. Najít registrační klíč a adresa URL registrace prováděné jako vstup v této šabloně najdete v článku [ **zabezpečení registrace** ](#secure-registration) části níže.

### <a name="powershell"></a>PowerShell

[Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) rutiny je možné připojit virtuální počítače na portálu Azure pomocí prostředí PowerShell.

## <a name="amazon-web-services-aws-virtual-machines"></a>Virtuální počítače Amazon Web Services (AWS)

Můžete snadno připojit Amazon Web Services virtuálních počítačů pro správu konfigurace pomocí sady nástrojů pro AWS DSC Azure Automation DSC. Další informace o sadě nástrojů [zde](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws"></a>Windows fyzický nebo virtuální počítače na místních počítačích nebo v cloudu, než Azure nebo AWS

Místní počítače s Windows a počítače s Windows v cloudech mimo Azure (například Amazon Web Services) možné zařadit také do Azure Automation DSC, tak dlouho, dokud mají přístup k Internetu pomocí několika jednoduchých kroků:

1. Zajistěte, aby nejnovější verzi [WMF 5](http://aka.ms/wmf5latest) nainstalován v počítačích, které chcete, aby se budou registrovat do Azure Automation DSC.
2. Postupujte podle pokynů v části [ **generování DSC metaconfigurations** ](#generating-dsc-metaconfigurations) dole vygenerujte složku obsahující potřebné metaconfigurations DSC.
3. Vzdáleně použijte metakonfiguraci PowerShell DSC na počítače, které chcete zařadit do provozu. **Počítač je tento příkaz spuštěn z musí mít nejnovější verzi [WMF 5](http://aka.ms/wmf5latest) nainstalován**:

    ```powershell
    Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
    ```

4. Pokud nemůžete použít metaconfigurations DSC Powershellu vzdáleně, zkopírujte složku metaconfigurations z kroku 2 na každý počítač zařadit do provozu. Potom zavolejte **Set-DscLocalConfigurationManager** místně na každém počítači zařadit do provozu.
5. Pomocí portálu Azure nebo rutin zkontrolujte, že počítače zařadit do provozu nyní zobrazují jako uzly DSC zaregistrovaný v účtu Azure Automation.

## <a name="physicalvirtual-linux-machines-on-premises-in-azure-or-in-a-cloud-other-than-azure"></a>Linux fyzický nebo virtuální počítače na místních počítačích v Azure nebo v cloudu, než Azure

Místní počítače se systémem Linux, počítače s Linuxem v Azure a počítače se systémem Linux v cloudech mimo Azure možné zařadit také do Azure Automation DSC, tak dlouho, dokud mají přístup k Internetu pomocí několika jednoduchých kroků:

1. Zajistěte, aby nejnovější verzi [prostředí PowerShell konfigurace požadovaného stavu pro Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) nainstalován v počítačích, které chcete, aby se budou registrovat do Azure Automation DSC.
2. Pokud [správce místní konfigurace DSC prostředí PowerShell výchozí](https://msdn.microsoft.com/powershell/dsc/metaconfig4) odpovídající váš případ použití, a chcete připojit počítače tak, že se **obě** načítat z a nahlásit Azure Automation DSC:

   + Na každém počítači Linux mohl připojit k Azure Automation DSC pomocí Register.py zařadit pomocí výchozího nastavení správce místní konfigurace DSC prostředí PowerShell:

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   + Registrační klíč a adresa URL registrace u vašeho účtu Automation, najdete v tématu [ **zabezpečení registrace** ](#secure-registration) části níže.

     Pokud výchozí nastavení správce místní konfigurace DSC Powershellu **provést** **není** shodu váš případ použití nebo je chcete zařadit do počítače, tak, aby pouze ohlásí Azure Automation DSC, ale proveďte není konfigurace vyžádané nebo moduly Powershellu z něj, postupujte podle kroků 3 až 6. V opačném přímo přejděte na krok 6.

3. Postupujte podle pokynů [ **metaconfigurations generování DSC** ](#generating-dsc-metaconfigurations) části dole vygenerujte složku obsahující potřebné metaconfigurations DSC.
4. Vzdáleně použijte metakonfiguraci PowerShell DSC na počítače, které chcete zařadit do provozu:

    ```powershell
    $SecurePass = ConvertTo-SecureString -String "<root password>" -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential "root", $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard

    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

Počítač je tento příkaz spuštěn z musí mít nejnovější verzi [WMF 5](http://aka.ms/wmf5latest) nainstalována.

1. Pokud nemůžete použít metaconfigurations DSC Powershellu vzdáleně, pro každý počítač Linux zařadit, zkopírujte metakonfiguraci odpovídající k tomuto počítači ze složky v kroku 5 do počítače Linux. Potom zavolejte `SetDscLocalConfigurationManager.py` místně na každém počítači Linux chcete, aby se budou registrovat do Azure Automation DSC:

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

2. Pomocí portálu Azure nebo rutin zkontrolujte, že počítače zařadit do provozu nyní zobrazují jako uzly DSC zaregistrovaný v účtu Azure Automation.

## <a name="generating-dsc-metaconfigurations"></a>Generování DSC metaconfigurations

Obecně zařadit do provozu žádné počítače do Azure Automation DSC, [metakonfiguraci DSC](https://msdn.microsoft.com/en-us/powershell/dsc/metaconfig) může být generována, při použití informuje DSC agenta na počítači, aby načítat z nebo nahlásit Azure Automation DSC. Metaconfigurations DSC pro Azure Automation DSC může být generována pomocí konfigurace DSC prostředí PowerShell nebo rutin prostředí PowerShell Azure Automation.

> [!NOTE]
> DSC metaconfigurations obsahovat všechna tajemství potřeba zařadit do počítače s Automation účet pro správu. Ujistěte se, zda jste správně chránit všechny metaconfigurations DSC, které vytvoříte, nebo je odstranit po použití.

### <a name="using-a-dsc-configuration"></a>Používání konfigurace DSC

1. Otevřete prostředí PowerShell ISE jako správce v počítači ve vašem místním prostředí. Počítač musí mít nejnovější verzi [WMF 5](http://aka.ms/wmf5latest) nainstalována.
2. Zkopírujte následující skript místně. Tento skript obsahuje konfiguraci DSC prostředí PowerShell pro vytváření metaconfigurations a příkaz k vytvoření metakonfiguraci ji.

    ```powershell
    # The DSC configuration that will generate metaconfigurations
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

    # Create the metaconfigurations
    # TODO: edit the below as needed for your use case
    $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM to onboard>', '<some other VM to onboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set to $True to have machines only report to AA DSC but not pull from it
    }

    # Use PowerShell splatting to pass parameters to the DSC configuration being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    DscMetaConfigs @Params
    ```

3. Zadejte registrační klíč a adresu URL pro svůj účet Automation, jakož i názvy počítačů zařadit do provozu. Všechny ostatní parametry jsou volitelné. Registrační klíč a adresa URL registrace u vašeho účtu Automation, najdete v tématu [ **zabezpečení registrace** ](#secure-registration) části níže.
4. Pokud chcete počítače tak, aby odesílaly informace o stavu DSC Azure Automation DSC, ale není konfigurace vyžádané nebo moduly Powershellu, nastavte **ReportOnly** parametr na hodnotu true.
5. Spusťte skript. Teď byste měli mít složku s názvem **DscMetaConfigs** v pracovním adresáři, který obsahuje metaconfigurations PowerShell DSC pro počítače se budou registrovat (jako správce):

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-the-azure-automation-cmdlets"></a>Pomocí rutiny Azure Automation.

Pokud výchozí nastavení správce místní konfigurace DSC prostředí PowerShell odpovídá váš případ použití a chcete zařadit počítače tak, aby se jak načítat z a nahlásit Azure Automation DSC, rutiny Azure Automation poskytují zjednodušenou metodu generování metaconfigurations DSC potřebné:

1. Otevřete konzolu PowerShell nebo ISE prostředí PowerShell jako správce v počítači ve vašem místním prostředí.
2. Připojte se k Azure Resource Manager pomocí **Add-AzureRmAccount**
3. Stáhněte metaconfigurations PowerShell DSC pro počítače, které chcete zařadit do provozu z účtu Automation, ke které chcete zařadit uzly:

    ```powershell
    # Define the parameters for Get-AzureRmAutomationDscOnboardingMetaconfig using PowerShell Splatting
    $Params = @{

        ResourceGroupName = 'ContosoResources'; # The name of the ARM Resource Group that contains your Azure Automation Account
        AutomationAccountName = 'ContosoAutomation'; # The name of the Azure Automation Account where you want a node on-boarded to
        ComputerName = @('web01', 'web02', 'sql01'); # The names of the computers that the meta configuration will be generated for
        OutputFolder = "$env:UserProfile\Desktop\";
    }
    # Use PowerShell splatting to pass parameters to the Azure Automation cmdlet being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    Get-AzureRmAutomationDscOnboardingMetaconfig @Params
    ```
    
4. Teď byste měli mít složku s názvem ***DscMetaConfigs***, obsahující metaconfigurations PowerShell DSC pro počítače se budou registrovat (jako správce):
    
    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a>Zabezpečené registrace

Počítače můžou bezpečně připojit k účtu Azure Automation přes protokol registrace WMF 5 DSC, který umožňuje uzlu DSC k ověření serveru prostředí PowerShell DSC V2 Pull nebo vytváření sestav (včetně Azure Automation DSC). Uzel zaregistruje na server v **URL pro registraci**, ověřování pomocí **registrační klíč**. Během registrace vyjednat jedinečný certifikát pro tento uzel používané při ověřování k serveru po registraci uzlu DSC a server pro vyžádání obsahu nebo sestavy DSC. Tento proces zabraňuje zařazený nemá uzly z jednoho jiné, jako je například pokud uzel dojde k ohrožení bezpečnosti zosobnění a chovají závadně. Po registraci registrační klíč se nepoužije pro ověřování znovu a se odstraní z uzlu.

Můžete získat požadované informace o protokolu registrace DSC z **Správa klíčů** okno na portálu Azure preview. Otevřete toto okno kliknutím na ikonu klíče na **Essentials** panel pro účet Automation.

![](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

* Adresa URL registrace je pole adresy URL v okně správy klíčů.
* Registrační klíč je primární přístupový klíč nebo sekundární přístupový klíč v okně správy klíčů. Můžete použít buď klíč.

Pro zvýšení zabezpečení, primární a sekundární přístupové klíče účtu Automation může být kdykoli znovu vygenerovat (na **Správa klíčů** okno) aby budoucí uzlu registrace pomocí předchozí klíče.

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>Řešení potíží s registrace virtuální počítač Azure

Azure Automation DSC umožňuje snadno připojit virtuální počítače Azure Windows pro správu konfigurace. Pod pokličkou rozšíření konfigurace požadovaného stavu aplikace Azure virtuálních počítačů slouží k registraci virtuálního počítače s Azure Automation DSC. Vzhledem k tomu, že rozšíření konfigurace požadovaného stavu aplikace Azure virtuální počítač se spustí asynchronně, může být důležité jejím průběhu sledování a řešení potíží s jeho spuštění.

> [!NOTE]
> Jakékoli metody objektu registrace virtuálního počítače Windows Azure do Azure Automation DSC, který používá rozšíření konfigurace požadovaného stavu aplikace Azure virtuálních počítačů může trvat až jednu hodinu pro uzel k zobrazení až as registrované ve službě Azure Automation. Toto je instalace Windows Management Framework 5.0 na virtuálním počítači rozšíření virtuálních počítačů DSC Azure, který je vyžadován zařadit do virtuálního počítače Azure Automation DSC.

Pro vyřešení problémů nebo zobrazit stav rozšíření konfigurace požadovaného stavu aplikace Azure virtuálních počítačů, na portálu Azure přejděte do virtuálního počítače se zařazený nemá a pak klikněte na -> **všechna nastavení** -> **rozšíření** -> **DSC**. Další podrobnosti, můžete kliknout na **zobrazit podrobné informace o stavu**.

[![](./media/automation-dsc-onboarding/DSC_Onboarding_5.png)](https://technet.microsoft.com/library/dn249912.aspx)

## <a name="certificate-expiration-and-reregistration"></a>Vypršení platnosti certifikátu a registrace

Po registraci počítače s jako uzel DSC v Azure Automation DSC, jsou z mnoha důvodů, proč se budete muset znovu registrovat tento uzel v budoucnosti:

* Po registraci, automatické každý uzel jedinečný certifikát pro ověřování, jejíž platnost vyprší po jednom roce. V současné době protokol registrace DSC Powershellu nelze obnovit automaticky certifikáty při jejich blížícím se koncem platnosti, takže budete muset znovu registrovat uzly po času v roce. Před opětovná registrace, ujistěte se, že každý uzel běží Windows Management Framework 5.0 RTM. Pokud vyprší platnost certifikátu ověřování uzlu, a není znovu zaregistruje uzel, uzel nebude moci komunikovat s Azure Automation a budou označeny 'Unresponsive.' Opětovná provést 90 dní nebo méně z čas vypršení platnosti certifikátu, nebo kdykoli po času vypršení platnosti certifikátu, bude výsledkem nový certifikát se generovat a používat.
* Chcete-li změnit některé [správce místní konfigurace DSC prostředí PowerShell hodnoty](https://msdn.microsoft.com/powershell/dsc/metaconfig4) které byly nastavené při počáteční registraci uzlu, jako je například ConfigurationMode. V současné době můžete tyto hodnoty agenta DSC změnit jenom prostřednictvím registrace. Jedinou výjimkou je přiřazena k uzlu Konfigurace uzlu – přímo lze změnit v Azure Automation DSC.

Registrace lze provést stejným způsobem registrovat uzlu na začátku pomocí libovolné metody registrace popsané v tomto dokumentu. Není nutné před opětovná registrace se zrušit registraci uzlu z Azure Automation DSC.

## <a name="related-articles"></a>Související články

* [Přehled služby Azure Automation DSC](automation-dsc-overview.md)
* [Rutiny Azure Automation DSC](/powershell/module/azurerm.automation/#automation)
* [Ceny služby Azure Automation DSC](https://azure.microsoft.com/pricing/details/automation/)
