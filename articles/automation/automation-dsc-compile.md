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
# <a name="compiling-configurations-in-azure-automation-dsc"></a>Kompilování konfigurace v Azure Automation DSC.

Zkompilujete konfigurace konfigurace požadovaného stavu (DSC) s Azure Automation dvěma způsoby: v hello portál Azure a pomocí prostředí Windows PowerShell. Hello následující tabulka vám pomůže určit, kdy toouse, jakou metodu na základě hello charakteristik jednotlivých:

### <a name="azure-portal"></a>portál Azure

* Nejjednodušším způsobem s interaktivní uživatelské rozhraní
* Formulář tooprovide jednoduché parametr hodnoty
* Snadno sledovat stav úlohy
* Přístup k ověření pomocí přihlášení Azure

### <a name="windows-powershell"></a>Windows PowerShell

* Volání z příkazového řádku pomocí rutin prostředí Windows PowerShell
* Můžou být součástí automatizované řešení s více kroků
* Zadejte parametr jednoduché a komplexní hodnoty
* Sledování stavu úlohy
* Klient vyžaduje toosupport rutiny prostředí PowerShell
* Předat ConfigurationData
* Zkompilování konfigurace, které používají pověření

Jakmile jste se rozhodli metodě kompilace, můžete provést následující toostart kompilování hello příslušné postupy.

## <a name="compiling-a-dsc-configuration-with-hello-azure-portal"></a>Kompilaci konfigurace DSC s hello portálu Azure

1. Z vašeho účtu Automation, klikněte na tlačítko **konfigurace DSC**.
2. Klikněte na možnost konfigurace tooopen její okno.
3. Klikněte na tlačítko **zkompilovat**.
4. Pokud konfigurace hello nemá žádné parametry, jestli se mají toocompile nebudete výzvami tooconfirm ho. Pokud konfigurace hello obsahuje parametry, hello **zkompilování konfigurace** , můžete zadat hodnoty parametrů, otevře se okno. V tématu hello [ **základních parametrů** ](#basic-parameters) části níže další podrobnosti o parametrech.
5. Hello **úloha kompilace** se otevře okno, takže můžete sledovat stav úlohy kompilace hello a hello konfigurace uzlu (dokumenty konfigurace MOF) se nezdařila z důvodu toobe umístit do Azure Automation DSC pro vyžádání obsahu Server hello.

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a>Kompilaci konfigurace DSC pomocí prostředí Windows PowerShell

Můžete použít [ `Start-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) toostart kompilace pomocí prostředí Windows PowerShell. Hello následující vzorový kód spustí kompilaci konfigurace DSC názvem **SampleConfig**.

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
```

`Start-AzureRmAutomationDscCompilationJob`Vrátí kompilace úlohy, které můžete použít tootrack její stav objektu. Pak můžete použít tento objekt úlohy kompilace s [ `Get-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) toodetermine hello stav úlohy kompilace hello, a [ `Get-AzureRmAutomationDscCompilationJobOutput` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) tooview jeho datové proudy (výstup). Hello následující vzorový kód spustí kompilace hello **SampleConfig** konfigurace, čeká na dokončení a potom zobrazí jeho datové proudy.

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"

while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

## <a name="basic-parameters"></a>Základní parametry
Deklarace parametru v konfiguracích DSC, včetně typy parametrů a vlastnosti, funguje hello stejné jako v Azure Automation runbook. V tématu [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md) toolearn více informací o parametry runbooku.

Hello následující příklad používá dva parametry s názvem **FeatureName** a **IsPresent**, toodetermine hello hodnoty vlastností v hello **ParametersExample.sample** uzlu Konfigurace vygenerovaných během kompilace.

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

Můžete zkompilovat konfigurace DSC, které používají základní parametry, na portálu Azure Automation DSC hello nebo s prostředím Azure PowerShell:

### <a name="portal"></a>Portál

Hello portálu, můžete zadat hodnoty parametrů po kliknutí na **zkompilovat**.

![alternativní text](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a>PowerShell

Vyžaduje parametry v prostředí PowerShell [zatřiďovací tabulky](http://technet.microsoft.com/library/hh847780.aspx) hello klíč odpovídá názvu parametru hello, kde hello hodnota se rovná hodnotě parametru hello.

```powershell
$Parameters = @{
    "FeatureName" = "Web-Server"
    "IsPresent" = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters
```

Informace o předávání PSCredentials jako parametry najdete v tématu <a href="#credential-assets"> **prostředků přihlašovacích údajů** </a> níže.

## <a name="configurationdata"></a>ConfigurationData
**ConfigurationData** vám umožní tooseparate strukturální konfiguraci z žádnou konkrétní konfiguraci prostředí při používání DSC prostředí PowerShell. V tématu [oddělení "Co" z "Kde" v prostředí PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) toolearn více informací o **ConfigurationData**.

> [!NOTE]
> Můžete použít **ConfigurationData** při kompilaci v Azure Automation DSC pomocí Azure PowerShell, ale není v hello portálu Azure.

Hello následující příklad DSC konfigurace používá **ConfigurationData** prostřednictvím hello **$ConfigurationData** a **$AllNodes** klíčová slova. Budete také potřebovat hello [ **xWebAdministration** modulu](https://www.powershellgallery.com/packages/xWebAdministration/) v tomto příkladu:

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

Konfigurace DSC hello výše v prostředí PowerShell můžete zkompilovat. přidá dvě toohello konfigurace uzlu Azure Automation DSC pro vyžádání obsahu Server Hello níže prostředí PowerShell: **ConfigurationDataSample.MyVM1** a **ConfigurationDataSample.MyVM3**:

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

## <a name="assets"></a>Prostředky

Asset odkazy jsou hello stejné v konfiguracích DSC Azure Automation a sady runbook. V tématu hello následující informace:

* [Certifikáty](automation-certificates.md)
* [Připojení](automation-connections.md)
* [Přihlašovací údaje](automation-credentials.md)
* [Proměnné](automation-variables.md)

### <a name="credential-assets"></a>Prostředků přihlašovacích údajů

Během konfigurace DSC ve službě Azure Automation, můžete odkazovat pomocí prostředků přihlašovacích údajů **Get-AzureRmAutomationCredential**, prostředků přihlašovacích údajů můžete také být předán prostřednictvím parametrů, v případě potřeby. Pokud konfigurace přebírá parametr **PSCredential** typ, pak jako hodnotu tohoto parametru, nikoli objekt PSCredential, potřebujete název řetězce hello toopass prostředek přihlašovacích údajů automatizace Azure. Pozadí hello bude načíst a předán toohello konfigurace asset přihlašovacích údajů Azure Automation hello s tímto názvem.

Zachování pověření zabezpečení v uzlu Konfigurace (configuration dokumenty MOF) vyžaduje šifrování hello pověření v souboru MOF konfigurace uzlu hello. Služby Azure Automation trvá tento krok jeden další a šifruje hello celý soubor MOF. Ale právě se musí zjistit DSC prostředí PowerShell je to v pořádku pro toobe přihlašovací údaje, které jsou výstupem ve formátu prostého textu v průběhu generování MOF konfigurace uzlu, protože prostředí PowerShell DSC neví, že Azure Automation bude šifrování hello celý soubor MOF po jeho generování prostřednictvím úlohu kompilace.

Se dá zjistit DSC Powershellu, je to v pořádku pro toobe přihlašovací údaje, které jsou výstupem ve formátu prostého textu v konfiguraci uzlu hello generované soubory MOF pomocí [ **ConfigurationData**](#configurationdata). By měla předávat `PSDscAllowPlainTextPassword = $true` prostřednictvím **ConfigurationData** pro každý uzel blok název, který se zobrazí v konfiguraci hello DSC a použije přihlašovací údaje.

Hello následující příklad ukazuje konfigurace DSC, který používá prostředek přihlašovacích údajů automatizace.

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

Konfigurace DSC hello výše v prostředí PowerShell můžete zkompilovat. přidá dvě toohello konfigurace uzlu Azure Automation DSC pro vyžádání obsahu Server Hello níže prostředí PowerShell: **CredentialSample.MyVM1** a **CredentialSample.MyVM2**.

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

## <a name="importing-node-configurations"></a>Import konfigurace uzlu

Můžete také importovat configuratons uzlu (soubory MOF), které zkompilujete mimo Azure. Jednou z výhod to je, že tento uzel confiturations může být podepsané.
Konfigurace podepsaný uzlu je ověřit místně v uzlu spravovaného agentem hello DSC, zajistíte, že tato konfigurace hello se uzel použité toohello pochází z autorizovaná zdrojová.

> [!NOTE]
> Můžete importovat podepsané konfigurace do účtu Azure Automation, ale automatizace Azure aktuálně nepodporuje kompilování podepsaný konfigurace.

> [!NOTE]
> Soubor konfigurace uzlu musí být větší než 1 MB tooallow ho toobe importovat do Azure Automation.

Dozvíte, jak konfigurace uzlu toosign v https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.

### <a name="importing-a-node-configuration-in-hello-azure-portal"></a>Import konfigurace uzlu v hello portálu Azure

1. Z vašeho účtu Automation, klikněte na tlačítko **konfigurace uzlu DSC**.

    ![Konfigurace uzlu DSC](./media/automation-dsc-compile/node-config.png)
2. V hello **konfigurace uzlu DSC** okně klikněte na tlačítko **přidat NodeConfiguration**.
3. V hello **Import** okně klikněte na tlačítko Další toohello ikonu hello složky **uzlu konfigurační soubor** toobrowse textové pole pro uzel konfigurační soubor (MOF) v místním počítači.

    ![Procházením vyhledejte místního souboru](./media/automation-dsc-compile/import-browse.png)
4. Zadejte název v hello **název konfigurace** textové pole. Tento název musí odpovídat názvu hello hello konfigurace, ze kterého byl kompilován hello konfigurace uzlu.
5. Klikněte na **OK**.

### <a name="importing-a-node-configuration-with-powershell"></a>Import konfigurace uzlu pomocí prostředí PowerShell

Můžete použít hello [Import AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) tooimport rutiny konfigurace uzlu do vašeho účtu automation.

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName "MyAutomationAccount" -ResourceGroupName "MyResourceGroup" -ConfigurationName "MyNodeConfiguration" -Path "C:\MyConfigurations\TestVM1.mof"
```



