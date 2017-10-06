---
title: aaaPass JSON objektu tooan runbook automatizace Azure. | Microsoft Docs
description: Jak toopass parametry tooa runbook jako objekt JSON
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
keywords: "prostředí PowerShell, sady runbook, json, služby azure automation"
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 06/15/2017
ms.author: eslesar
ms.openlocfilehash: 8229a16015d549927ead5496c70e9fb391d35498
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="pass-a-json-object-tooan-azure-automation-runbook"></a>Předat Azure Automation runbook tooan objekt JSON

Může být užitečné toostore dat, které chcete toopass tooa runbook v souboru JSON.
Například může vytvořit soubor JSON, který obsahuje všechny parametry hello chcete toopass tooa runbook.
toodo, můžete mít tooconvert hello JSON tooa řetězec a pak převést objekt prostředí PowerShell tooa řetězce hello před předáním runbook toohello jeho obsah.

V tomto příkladu vytvoříme skript prostředí PowerShell, který volá [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart Powershellového runbooku, předávání hello obsah sady hello JSON toohello runbook.
Hello Powershellový runbook spustí virtuální počítač Azure, získávání parametrů hello hello virtuálních počítačů z formátu JSON, který byl předán v hello.

## <a name="prerequisites"></a>Požadavky
toocomplete tohoto kurzu budete potřebovat hello následující:

* Předplatné Azure. Pokud nemáte účet, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo <a href="/pricing/free-account/" target="_blank">[si zaregistrovat bezplatný účet](https://azure.microsoft.com/free/).
* [Účet Automation](automation-sec-configure-azure-runas-account.md) toohold hello sady runbook a ověření tooAzure prostředky.  Tento účet musí mít oprávnění toostart a zastavit hello virtuální počítač.
* Virtuální počítač Azure. Počítač zastavíme a spustíme, proto to nesmí být produkční virtuální počítač.
* Azure Powershell nainstalovaný v místním počítači. V tématu [nainstalovat a nakonfigurovat Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) informace o tooget prostředí Azure PowerShell.

## <a name="create-hello-json-file"></a>Vytvořte soubor JSON hello

Typ hello následující testování do textového souboru a uložte ho jako `test.json` někde v místním počítači.

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

## <a name="create-hello-runbook"></a>Vytvoření sady runbook hello

Vytvořte nový runbook prostředí PowerShell s názvem "Test-Json" ve službě Azure Automation.
toolearn jak toocreate novou sadu runbook Powershellu, najdete v části [Můj první Powershellový runbook](automation-first-runbook-textual-powershell.md).

tooaccept hello JSON data, vyžaduje hello runbook objekt jako vstupní parametr.

Hello runbook pak můžete použít hello vlastnosti definované v hello JSON.

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect tooAzure account   
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object tooactual JSON
$json = $json | ConvertFrom-Json

# Use hello values from hello JSON object as hello parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
 ```

 Uložte a publikovat tuto sadu runbook v účtu Automation.

## <a name="call-hello-runbook-from-powershell"></a>Volání sady runbook hello z prostředí PowerShell

Nyní můžete volat hello runbook z místního počítače pomocí prostředí Azure PowerShell.
Spusťte následující příkazy prostředí PowerShell hello:

1. Přihlaste se tooAzure:
   ```powershell
   Login-AzureRmAccount
   ```
    Můžete se výzvami tooenter vaše přihlašovací údaje Azure.
1. Získat hello obsah souboru JSON hello a převést ho tooa řetězec:
    ```powershell
    $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
    ```
    `JsonPath`je hello cesta, kam jste uložili soubor JSON hello.
1. Převést řetězec obsah hello `$json` tooa objekt prostředí PowerShell:
   ```powershell
   $JsonParams = @{"json"=$json}
   ```
1. Vytvořit tabulku hash pro parametry hello `Start-AzureRmAutomstionRunbook`:
   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```
   Všimněte si, že nastavíte hodnotu hello `Parameters` toohello prostředí PowerShell objekt, který obsahuje hodnoty hello ze souboru JSON hello. 
1. Spuštění sady runbook hello
   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

Hello runbook používá hello hodnoty z hello JSON souboru toostart virtuálního počítače.

## <a name="next-steps"></a>Další kroky

* toolearn Další informace o úpravy sady runbook Powershellu a pracovní postup prostředí PowerShell s textový editor, najdete v části [úpravy textovou sady runbook ve službě Azure Automation](automation-edit-textual-runbook.md) 
* toolearn Další informace o vytvoření a import sad runbook, najdete v části [vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md)


