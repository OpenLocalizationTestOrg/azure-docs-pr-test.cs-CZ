---
title: "aaaRunbook vstupní parametry | Microsoft Docs"
description: "Vstupní parametry Runbooku zvyšování flexibility hello sad runbook tak, že vám toopass data tooa runbook, jakmile je spuštěno. Tento článek popisuje různé scénáře, kdy se používá vstupních parametrů v sadách runbook."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 4d3dff2c-1f55-498d-9a0e-eee497e5bedb
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: sngun
ms.openlocfilehash: f3abaf92382e7d41019616bafb14af23cf98dd9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-input-parameters"></a>Vstupní parametry runbooku
Vstupní parametry Runbooku zvyšování flexibility hello sad runbook tím, že toopass data tooit při jeho spuštění. Parametry Hello povolit hello runbook akce toobe určený pro konkrétní scénáře a různá prostředí. V tomto článku jsme vás provede různé scénáře použití vstupních parametrů v sadách runbook.

## <a name="configure-input-parameters"></a>Konfigurace vstupní parametry
Vstupní parametry se dá nakonfigurovat v prostředí PowerShell, pracovní postup prostředí PowerShell a grafické runbooky. Sada runbook může mít několik parametrů s různými datovými typy nebo žádné parametry vůbec. Vstupní parametry lze povinné nebo volitelné a můžete přiřadit výchozí hodnotu pro volitelné parametry. Můžete přiřadit hodnoty toohello vstupní parametry pro sady runbook po spuštění prostřednictvím jednoho z dostupných metod hello. Tyto metody zahrnují spuštění runbooku z portálu hello nebo webové službě. Můžete také spustit jako podřízené sady runbook, která je volána vložené v jiné sady runbook.

## <a name="configure-input-parameters-in-powershell-and-powershell-workflow-runbooks"></a>Konfigurace vstupních parametrů v sadách runbook Powershellu a pracovní postup prostředí PowerShell
Prostředí PowerShell a [runbooky pracovních postupů Powershellu](automation-first-runbook-textual.md) ve službě Azure Automation podporují vstupní parametry, které jsou definovány pomocí hello následující atributy.  

| **Vlastnost** | **Popis** |
|:--- |:--- |
| Typ |Povinná hodnota. datový typ Hello očekávaná hodnota parametru hello. Libovolného typu .NET je platný. |
| Name (Název) |Povinná hodnota. Hello název parametru hello. Tomu musí být jedinečný v rámci hello runbook a může obsahovat jenom písmena, číslice nebo podtržítka. Musí začínat písmenem. |
| Povinné |Volitelné. Určuje, zda musí být zadána hodnota parametru hello. Pokud nastavíte příliš**$true**, potom při spuštění runbooku hello, je třeba zadat hodnotu. Pokud nastavíte příliš**$false**, pak hodnota je volitelná. |
| Výchozí hodnota |Volitelné.  Určuje hodnotu, která se použije pro parametr hello, pokud není hodnota předaná při spuštění runbooku hello. Výchozí hodnota se dá nastavit pro libovolný parametr a bude automaticky nastavit hello parametr volitelný bez ohledu na to hello povinné nastavení. |

Prostředí Windows PowerShell podporuje další atributy vstupní parametry než těch, které zde uvedeny, například ověřování, aliasy, a nastaví parametr. Ale automatizace Azure aktuálně podporuje pouze hello vstupní parametry uvedené výše.

Definici parametru v runboocích pracovního postupu Powershellu má hello následující obecné formulář, kde jsou několik parametrů oddělených čárkami.

   ```
     Param
     (
         [Parameter (Mandatory= $true/$false)]
         [Type] Name1 = <Default value>,

         [Parameter (Mandatory= $true/$false)]
         [Type] Name2 = <Default value>
     )
   ```

> [!NOTE]
> Když definujete parametry, pokud neurčíte hello **povinné** atribut, pak ve výchozím nastavení, je parametr hello považovat za volitelné. Navíc pokud nastavíte výchozí hodnotu pro parametr v runboocích pracovního postupu Powershellu, bude považována pomocí prostředí PowerShell jako volitelný parametr, bez ohledu na to hello **povinné** hodnota atributu.
> 
> 

Jako příklad nakonfigurujeme hello vstupní parametry runbooku pracovního postupu Powershellu, který zobrazí podrobnosti o virtuální počítače, jeden virtuální počítač nebo všechny virtuální počítače ve skupině prostředků. Tato sada runbook má dva parametry, jak ukazuje následující snímek obrazovky hello: hello název virtuálního počítače a hello název skupiny prostředků hello.

![Pracovní postup automatizace prostředí PowerShell](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

V této definici parametru hello parametry **$VMName** a **$resourceGroupName** jsou jednoduché parametry typu String. Ale prostředí PowerShell a pracovní postup prostředí PowerShell runbooky podporují všechny typy jednoduché a komplexní typy, jako například **objekt** nebo **PSCredential** pro vstupní parametry.

Pokud vaše sada runbook má vstupní parametr typu objektu, pak použít PowerShell zatřiďovací tabulku s (název, hodnotu) páry toopass hodnotu. Například pokud máte hello následující parametr v sadě runbook:

     [Parameter (Mandatory = $true)]
     [object] $FullName

Pak můžete předat hello následující parametr toohello hodnoty:

    @{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}


## <a name="configure-input-parameters-in-graphical-runbooks"></a>Konfigurace vstupní parametry v grafické runbooky
příliš[konfigurace grafický runbook](automation-first-runbook-graphical.md) s vstupní parametry, vytvoříme grafického runbooku, který zobrazí podrobnosti o virtuálních počítačích, buď jeden virtuální počítač nebo všechny virtuální počítače ve skupině prostředků. Konfigurace sady runbook se skládá z dvě hlavní aktivity, jak je popsáno níže.

[**Ověření Runbooků pomocí účtu spustit v Azure jako** ](automation-sec-configure-azure-runas-account.md) tooauthenticate s Azure.

[**Get-AzureRmVm** ](https://msdn.microsoft.com/library/mt603718.aspx) tooget hello vlastnosti virtuálních počítačů.

Můžete použít hello [ **Write-Output** ](https://technet.microsoft.com/library/hh849921.aspx) aktivity toooutput hello názvy virtuálních počítačů. Hello aktivity **Get-AzureRmVm** přijímá dva parametry hello **název virtuálního počítače** a hello **název skupiny prostředků**. Vzhledem k tomu, že tyto parametry může vyžadovat různé hodnoty, při každém spuštění sady runbook hello, můžete přidat runbook tooyour vstupní parametry. Zde jsou vstupní parametry tooadd hello kroky:

1. Vyberte hello grafický runbook z hello **Runbooky** okna a pak klikněte na tlačítko [ **upravit** ](automation-graphical-authoring-intro.md) ho.
2. V editoru runbooků hello, klikněte na **vstup a výstup** tooopen hello **vstup a výstup** okno.
   
    ![Grafický runbook automatizace](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)
3. Hello **vstup a výstup** zobrazuje seznam vstupních parametrů, které jsou definovány pro sadu runbook hello. V tomto okně můžete přidat nový vstupní parametr nebo upravit konfiguraci hello existující vstupního parametru. Klikněte na tlačítko tooadd nový parametr hello sady runbook, **přidat vstup** tooopen hello **vstupní parametr Runbooku** okno. Zde můžete nakonfigurovat hello následující parametry:
   
   | **Vlastnost** | **Popis** |
   |:--- |:--- |
   | Name (Název) |Povinná hodnota.  Hello název parametru hello. Tomu musí být jedinečný v rámci hello runbook a může obsahovat jenom písmena, číslice nebo podtržítka. Musí začínat písmenem. |
   | Popis |Volitelné. Popis o hello účel vstupní parametr. |
   | Typ |Volitelné. Hello datový typ, který je očekávaná hodnota parametru hello. Parametr podporované typy jsou **řetězec**, **Int32**, **Int64**, **Decimal**, **Boolean**, **data a času**, a **objekt**. V případě, že datový typ není vybrána, bude výchozí příliš**řetězec**. |
   | Povinné |Volitelné. Určuje, zda musí být zadána hodnota parametru hello. Pokud se rozhodnete **Ano**, potom při spuštění runbooku hello, je třeba zadat hodnotu. Pokud se rozhodnete **žádné**, pak hodnota není požadovaná při spuštění runbooku hello a může být nastaven výchozí hodnotu. |
   | Výchozí hodnota |Volitelné. Určuje hodnotu, která se použije pro parametr hello, pokud není hodnota předaná při spuštění runbooku hello. Výchozí hodnotu lze nastavit pro parametr, který není povinné. Zvolte tooset výchozí hodnotu, **vlastní**. Tato hodnota se používá, pokud je při spuštění runbooku hello zadat jinou hodnotu. Zvolte **žádné** Pokud nechcete, aby tooprovide žádné výchozí hodnota. |
   
    ![Přidat nové vstup](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. Vytvořte dva parametry s hello následující vlastnosti, které se použijí hello **Get-AzureRmVm** aktivity:
   
   * **Parametr1:**
     
     * Název – VMName
     * Typ – řetězec
     * Povinné – ne
   * **Parameter2:**
     
     * Název – název skupiny prostředků
     * Typ – řetězec
     * Povinné – ne
     * Výchozí hodnota - vlastní
     * Vlastní výchozí hodnota - \<název skupiny prostředků hello, která obsahuje virtuální počítače hello >
5. Jakmile přidáte hello parametry, klikněte na tlačítko **OK**.  Můžete nyní zobrazit v hello **vstup a výstup okna**. Klikněte na tlačítko **OK** znovu a potom klikněte na **Uložit** a **publikovat** runbooku.

## <a name="assign-values-tooinput-parameters-in-runbooks"></a>Přiřadit hodnoty tooinput parametrů v sadách runbook
Hodnoty můžete předat parametry tooinput v sadách runbook ve hello následující scénáře.

### <a name="start-a-runbook-and-assign-parameters"></a>Spuštění sady runbook a přiřaďte parametry
Sady runbook lze spustit mnoho způsobů: prostřednictvím hello portál Azure, s webhook, jehož, pomocí rutin prostředí PowerShell, hello REST API nebo s hello SDK. Níže probereme různé metody pro spuštění sady runbook a přiřazení parametry.

#### <a name="start-a-published-runbook-by-using-hello-azure-portal-and-assign-parameters"></a>Spusťte publikované sady runbook pomocí portálu Azure hello a přiřaďte parametry
Pokud jste [spuštění sady runbook hello](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), hello **spuštění Runbooku** otevře se okno a můžete nastavit hodnoty pro parametry hello, které jste právě vytvořili.

![Začít používat portál hello](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

V popisku hello pod hello vstupní pole uvidíte hello atributy, které byly nastaveny pro parametr hello. Atributy zahrnují povinné nebo volitelné, typ a výchozí hodnota. V hello nápovědy bublinách další toohello názvu parametru se zobrazí všechny informace hello klíče, které budete potřebovat toomake rozhodnutí o vstupní hodnoty parametrů. Tyto informace zahrnují, zda je parametr povinný nebo volitelné. Zahrnuje také hello typ a výchozí hodnota (pokud existuje) a další užitečné poznámky.

![Bublině nápovědy](media/automation-runbook-input-parameters/automation-05-helpbaloon.png)

> [!NOTE]
> Podpora parametry typu String **prázdný** hodnoty řetězců.  Zadání **[EmptyString]** v hello vstupní parametr pole předá parametru toohello prázdný řetězec. Také nepodporují parametry typu řetězec **Null** předáním hodnoty. Pokud předáte nemáte žádné hodnota toohello řetězcový parametr, pak prostředí PowerShell bude interpretovat jako hodnotu null.
> 
> 

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a>Spustit publikované sady runbook pomocí rutin prostředí PowerShell a přiřaďte parametry
* **Rutiny Azure Resource Manager:** můžete spustit runbook služby automatizace, který byl vytvořen ve skupině prostředků s použitím [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).
  
  **Příklad:**
  
  ```
  $params = @{“VMName”=”WSVMClassic”;”resourceGroupeName”=”WSVMClassicSG”}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” –ResourceGroupName $resourceGroupName -Parameters $params
  ```
* **Rutiny Azure Service Management:** můžete spustit runbook služby automatizace, který byl vytvořen v do výchozí skupiny prostředků pomocí [Start-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).
  
  **Příklad:**
  
  ```
  $params = @{“VMName”=”WSVMClassic”; ”ServiceName”=”WSVMClassicSG”}
  
  Start-AzureAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” -Parameters $params
  ```

> [!NOTE]
> Při spuštění sady runbook pomocí rutin prostředí PowerShell, parametr výchozí **MicrosoftApplicationManagementStartedBy** je vytvořena s hodnotou hello **prostředí PowerShell**. Tento parametr můžete zobrazit v hello **podrobnosti úlohy** okno.  
> 
> 

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a>Spuštění sady runbook pomocí sady SDK a přiřaďte parametry
* **Azure Resource Manager metoda:** sady runbook můžete spustit pomocí hello SDK programovací jazyk. Níže je fragmentu kódu C# pro spuštění sady runbook ve vašem účtu Automation. Můžete zobrazit všechny hello kódu v našem [úložiště GitHub](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).  
  
  ```
   public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
      {
        var response = AutomationClient.Jobs.Create(resourceGroupName, automationAccount, new JobCreateParameters
         {
            Properties = new JobCreateProperties
             {
                Runbook = new RunbookAssociationProperty
                 {
                   Name = runbookName
                 },
                   Parameters = parameters
             }
         });
      return response.Job;
      }
  ```
* **Azure Service Management metoda:** sady runbook můžete spustit pomocí hello SDK programovací jazyk. Níže je fragmentu kódu C# pro spuštění sady runbook ve vašem účtu Automation. Můžete zobrazit všechny hello kódu v našem [úložiště GitHub](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).
  
  ```      
  public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
    {
      var response = AutomationClient.Jobs.Create(automationAccount, new JobCreateParameters
    {
      Properties = new JobCreateProperties
         {
           Runbook = new RunbookAssociationProperty
         {
           Name = runbookName
              },
                Parameters = parameters
              }
       });
      return response.Job;
    }
  ```
  
  toostart této metody vytvoření hello toostore slovník parametrů sady runbook, **VMName** a **resourceGroupName**a jejich hodnoty. Spusťte hello runbook. Níže je hello C# fragment kódu pro volání metody hello, který je definovaný výše.
  
  ```
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters toohello dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call hello StartRunbook method with parameters
  StartRunbook(“Get-AzureVMGraphical”, RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-hello-rest-api-and-assign-parameters"></a>Spuštění sady runbook pomocí hello REST API a přiřaďte parametry
Úlohy runbooku můžete vytvořit a začít s hello REST API služby Azure Automation pomocí hello **PUT** metoda s hello následující identifikátor URI požadavku.

    https://management.core.windows.net/<subscription-id>/cloudServices/<cloud-service-name>/resources/automation/~/automationAccounts/<automation-account-name>/jobs/<job-id>?api-version=2014-12-08`

V identifikátoru URI žádosti hello nahraďte hello následující parametry:

* **id předplatného:** ID vašeho předplatného Azure.  
* **Název cloudové služby:** hello název hello žádost o hello toowhich cloudové služby by měly být odeslány.  
* **název účtu Automation:** hello název účtu automation, který je hostován v rámci hello zadaný cloudové služby.  
* **id úlohy:** hello identifikátor GUID pro úlohy hello. Identifikátory GUID v prostředí PowerShell můžete vytvořit pomocí hello **[GUID]::NewGuid(). ToString()** příkaz.

V pořadí toopass parametry toohello runbook úlohy použijte hello textu požadavku. Jak dlouho trvá hello následující dvě vlastnosti zadaný ve formátu JSON:

* **Název sady Runbook:** vyžaduje. Hello název sady runbook hello toostart úlohy hello.  
* **Parametry sady Runbook:** volitelné. Slovník hello seznam parametrů v (název, hodnotu) formátu, kde název by měl být typu String a hodnota může být libovolná platná hodnota JSON.

Pokud chcete, aby toostart hello **Get-AzureVMTextual** runbooku, který byl vytvořen již dříve s **VMName** a **resourceGroupName** jako parametry, použijte následující formát JSON hello tělo žádosti hello.

   ```
    {
      "properties":{
        "runbook":{
        "name":"Get-AzureVMTextual"},
      "parameters":{
         "VMName":"WSVMClassic",
         "resourceGroupName":”WSCS1”}
        }
    }
   ```

Stavový kód HTTP 201 je vrácena, pokud hello úlohy je úspěšně vytvořen. Další informace o hlavičky odpovědi a hello odpovědi, najdete v části článku toohello o příliš[vytvořit úlohu runbooku pomocí hello REST API.](https://msdn.microsoft.com/library/azure/mt163849.aspx)

### <a name="test-a-runbook-and-assign-parameters"></a>Otestování sady runbook a přiřaďte parametry
Při jste [testovací hello verzi konceptu sady runbook](automation-testing-runbook.md) pomocí hello testovací možnost hello **testování** otevře se okno a můžete nastavit hodnoty pro parametry hello, které jste právě vytvořili.

![Testování a přiřadit parametry](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-tooa-runbook-and-assign-parameters"></a>Propojit plán tooa runbooku a přiřadit parametry
Můžete [propojit plán s](automation-schedules.md) tooyour runbook tak, že hello runbook spouští v určitém čase. Když vytvoříte plán hello a hello runbook použije tyto hodnoty, když se spustí podle plánu hello přiřadíte vstupní parametry. Plán hello nelze uložit, dokud nebudou k dispozici všechny hodnoty povinný parametr.

![Parametry plánu a přiřazení](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a>Vytvoření webhooku pro sadu runbook a přiřazení parametry
Můžete vytvořit [webhooku](automation-webhooks.md) pro runbook a nakonfigurovat vstupní parametry runbooku. Hello webhooku nelze uložit, dokud nebudou k dispozici všechny hodnoty povinný parametr.

![Vytvoření webhooku a přiřazení parametry](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

Při spuštění sady runbook pomocí webhook, jehož, hello předdefinované vstupní parametr  **[Webhookdata](automation-webhooks.md#details-of-a-webhook)**  je odeslaný společně hello vstupní parametry, které jste definovali. Můžete kliknout na tooexpand hello **WebhookData** parametr další podrobnosti.

![Parametr WebhookData](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="next-steps"></a>Další kroky
* Další informace o runbook vstup a výstup, najdete v části [Azure Automation: runbook vnořených sad runbook, vstup a výstup](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).
* Podrobnosti o různé způsoby toostart sady runbook najdete v tématu [spuštění sady runbook](automation-starting-a-runbook.md).
* tooedit textovou sady runbook, najdete v příliš[úpravy textovou runbooky](automation-edit-textual-runbook.md).
* tooedit grafický runbook odkazovat příliš[vytváření grafického obsahu ve službě Azure Automation](automation-graphical-authoring-intro.md).

