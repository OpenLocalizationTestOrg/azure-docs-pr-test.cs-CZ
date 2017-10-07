---
title: "aaaStarting sady runbook ve službě Azure Automation | Microsoft Docs"
description: "Shrnuje hello různé metody, které lze použít toostart sady runbook ve službě Azure Automation a poskytuje podrobnosti o použití obě hello portál Azure a prostředí Windows PowerShell."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 6ee756b4-9200-4eb2-9bda-ec156853803b
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte;bwren
ms.openlocfilehash: e44bce5b56b8e803f9247fbb4f3d4db7ab35c913
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="starting-a-runbook-in-azure-automation"></a>Spuštění sady runbook ve službě Azure Automation
Hello následující tabulka vám pomůže určit hello metoda toostart sady runbook ve službě Azure Automation, který je nejvhodnější tooyour určitého scénáře. Tento článek obsahuje informace o spuštění sady runbook s hello portál Azure a prostředí Windows PowerShell. Podrobnosti na hello jiných metod jsou uvedeny v jiných dokumentace, která je přístupné z hello odkazy níže.

| **– METODA** | **VLASTNOSTI** |
| --- | --- |
| [Azure Portal](#starting-a-runbook-with-the-azure-portal) |<li>Nejjednodušším způsobem s interaktivní uživatelské rozhraní.<br> <li>Formulář tooprovide jednoduché parametr hodnoty.<br> <li>Snadno sledovat stav úlohy.<br> <li>Přístup k ověření pomocí přihlášení Azure. |
| [Prostředí Windows PowerShell](https://msdn.microsoft.com/library/dn690259.aspx) |<li>Volání z příkazového řádku pomocí rutin prostředí Windows PowerShell.<br> <li>Můžou být součástí automatizované řešení s více kroků.<br> <li>Ověření žádosti o certifikát nebo OAuth uživatele hlavní / service hlavní.<br> <li>Zadejte hodnoty parametrů jednoduché a komplexní.<br> <li>Sledovat stav úlohy.<br> <li>Klient vyžaduje toosupport rutiny prostředí PowerShell. |
| [Rozhraní API služby Azure Automation](https://msdn.microsoft.com/library/azure/mt662285.aspx) |<li>Nejflexibilnější, ale také většina komplexní.<br> <li>Volat z libovolný vlastní kód, který umí vytvářet požadavky HTTP.<br> <li>Žádost o ověření pomocí certifikátu nebo Oauth uživatele hlavní / service hlavní.<br> <li>Zadejte hodnoty parametrů jednoduché a komplexní.<br> <li>Sledovat stav úlohy. |
| [Webhooky](automation-webhooks.md) |<li>Spusťte runbook z jednoho požadavku HTTP.<br> <li>K ověření pomocí tokenu zabezpečení v adrese URL.<br> <li>Hodnoty parametrů zadané při vytvoření webhooku nejde přepsat klienta. Sada Runbook může definovat jeden parametr, který je naplněn hello podrobnosti požadavku HTTP.<br> <li>Žádná možnost tootrack stav úlohy prostřednictvím URL webhooku se nenačetla. |
| [Odpověď tooAzure výstrahy](../log-analytics/log-analytics-alerts.md) |<li>Spuštění runbooku v odpovědi tooAzure výstrahy.<br> <li>Nakonfigurujte webhooku pro sadu runbook a propojit tooalert.<br> <li>K ověření pomocí tokenu zabezpečení v adrese URL. |
| [Plán](automation-schedules.md) |<li>Runbook se automaticky spustí podle plánu hodinové, denní, týdenní nebo měsíční.<br> <li>Upravit plán prostřednictvím portálu Azure, rutiny prostředí PowerShell nebo rozhraní API služby Azure.<br> <li>Zadejte parametr hodnoty toobe použít s plánem. |
| [Z jiného Runbooku](automation-child-runbooks.md) |<li>Sada runbook použijte jako aktivita v jiné sady runbook.<br> <li>Tato možnost je užitečná pro funkce, které používá více sad runbook.<br> <li>Zadejte parametr hodnoty toochild runbook a výstup použít v nadřazené sady runbook. |

Hello následující obrázek ukazuje podrobné celým procesem v životním cyklu hello sady runbook. Zahrnuje to různými způsoby, kterými spuštění sady runbook ve službě Azure Automation komponent potřebných pro hybridní pracovní proces Runbooku tooexecute Azure Automation runbook a interakce mezi různými součástmi. toolearn o spouštění runbooků služeb automatizace ve vašem datovém centru, najdete v příliš[procesy hybrid runbook Worker](automation-hybrid-runbook-worker.md)

![Architektura sady Runbook](media/automation-starting-runbook/runbooks-architecture.png)

## <a name="starting-a-runbook-with-hello-azure-portal"></a>Spuštění sady runbook s hello portálu Azure
1. V hello portálu Azure, vyberte **automatizace** a pak klikněte hello název účtu automation.
2. V nabídce centra hello vyberte **Runbooky**.
3. Na hello **Runbooky** okně Vybrat sadu runbook a pak klikněte na **spustit**.
4. Pokud hello runbook obsahuje parametry, budou hodnoty výzvami tooprovide se textové pole pro každý parametr. V tématu [parametry Runbooku](#Runbook-parameters) níže další podrobnosti o parametry.
5. Na hello **úlohy** okno, můžete zobrazit stav úlohy runbooku hello hello.

## <a name="starting-a-runbook-with-windows-powershell"></a>Spuštění sady runbook pomocí prostředí Windows PowerShell
Můžete použít hello [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart sady runbook pomocí prostředí Windows PowerShell. Hello následující vzorový kód spustí runbook s názvem Test-Runbook.

```
Start-AzureRmAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

Vrátí AzureRmAutomationRunbook počáteční úlohu a objektů, které můžete použít tootrack její stav po spuštění runbooku hello. Pak můžete použít tento objekt úlohy v [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) toodetermine hello stav úlohy hello a [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) tooget její výstup. Hello následující vzorový kód spustí runbook s názvem Test-Runbook, počká byla dokončena a potom zobrazí jeho výstup.

```
$runbookName = "Test-Runbook"
$ResourceGroup = "ResourceGroup01"
$AutomationAcct = "MyAutomationAccount"

$job = Start-AzureRmAutomationRunbook –AutomationAccountName $AutomationAcct -Name $runbookName -ResourceGroupName $ResourceGroup

$doLoop = $true
While ($doLoop) {
   $job = Get-AzureRmAutomationJob –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup
   $status = $job.Status
   $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzureRmAutomationJobOutput –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup –Stream Output
```

Pokud hello runbook vyžaduje parametry, pak je nutné zadat jako [zatřiďovací tabulky](http://technet.microsoft.com/library/hh847780.aspx) kde klíč zatřiďovací tabulky hello hello odpovídá hello parametr název a hodnotu hello je hodnota parametru hello. Hello následující příklad ukazuje, jak toostart runbooku se dvěma řetězcovými parametry s názvem FirstName a LastName, celočíselným parametrem s názvem RepeatCount a logickým parametrem s názvem Show. Další informace o parametrech najdete v tématu [parametry Runbooku](#Runbook-parameters) níže.

```
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="runbook-parameters"></a>Parametry Runbooku
Při spuštění runbooku z hello portálu Azure nebo prostředí Windows PowerShell hello instrukce se posílá prostřednictvím hello webové služby Azure Automation. Tato služba nepodporuje parametry s komplexními datovými typy. Pokud potřebujete tooprovide hodnotu komplexního parametru, pak je musíte ji volat z jiného runbooku jak je popsáno v [podřízené runbooky ve službě Azure Automation](automation-child-runbooks.md).

Hello webové služby Azure Automation nabízí zvláštní funkce pro parametry pomocí určitých datových typů, jak je popsáno v následující části hello.

### <a name="named-values"></a>Pojmenovaných hodnot
Pokud parametr hello je datového typu [object], pak můžete použít následující toosend formátu JSON je seznam s názvem hodnoty hello: *{název1: 'Value1', NÁZEV2: 'Hodnota2', Name3: 'Hodnota3'}*. Tyto hodnoty musí být jednoduché typy. Hello runbook obdrží parametr hello jako [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) s vlastnostmi, které odpovídají tooeach s názvem hodnotu.

Vezměte v úvahu následující testovací runbook, který přijme parametr s názvem uživatele hello.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][object]$user
   )
    $userObject = $user | ConvertFrom-JSON
    if ($userObject.Show) {
        foreach ($i in 1..$userObject.RepeatCount) {
            $userObject.FirstName
            $userObject.LastName
        }
    }
}
```

Následující text Hello může použitý pro parametr hello uživatele.

```
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

Výsledkem je následující výstup hello.

```
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a>Pole
Pokud je parametr hello pole, jako třeba [array] nebo [string []], můžete použít hello následující toosend formátu JSON je seznam hodnot: *[hodnota1, hodnota2, hodnota3]*. Tyto hodnoty musí být jednoduché typy.

Vezměte v úvahu následující testovací runbook, který přijme parametr s názvem hello *uživatele*.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][array]$user
   )
    if ($user[3]) {
        foreach ($i in 1..$user[2]) {
            $ user[0]
            $ user[1]
        }
    }
}
```

Následující text Hello může použitý pro parametr hello uživatele.

```
["Joe","Smith",2,true]
```

Výsledkem je následující výstup hello.

```
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a>Přihlašovací údaje
Pokud parametr hello je datový typ **PSCredential**, můžete zadat název hello Azure Automation [asset přihlašovacích údajů](automation-credentials.md). Hello runbook načte hello přihlašovací údaje s vámi určeným názvem hello.

Vezměte v úvahu hello následující testovací runbook, který přijme parametr s názvem přihlašovacích údajů.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

Hello následující text by mohly být použity hello parametr za předpokladu, že byla volána asset přihlašovacích údajů *moje pověření*.

```
My Credential
```

Za předpokladu, že hello uživatelské jméno v přihlašovacích údajů hello *jsmith*, výsledkem je následující výstup hello.

```
jsmith
```

## <a name="next-steps"></a>Další kroky
* architektura sady runbook Hello v aktuální článek poskytuje souhrnné informace o správě prostředků sady runbook v Azure a místně s hello hybridní pracovní proces Runbooku.  toolearn o spouštění runbooků služeb automatizace ve vašem datovém centru, najdete v příliš[procesy Hybrid Runbook Worker](automation-hybrid-runbook-worker.md).
* toolearn Další informace o vytváření toobe modulární runbooky používat ostatní runbooky pro konkrétní nebo běžné funkce hello odkazovat příliš[podřízené Runbooky](automation-child-runbooks.md).

