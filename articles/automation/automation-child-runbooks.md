---
title: "aaaChild runbooky ve službě Azure Automation | Microsoft Docs"
description: "Popisuje hello různé metody pro spuštění sady runbook ve službě Azure Automation z jiného runbooku a sdílení informací mezi nimi."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 919887b9-43e2-4c16-883c-f81807fe37db
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2017
ms.author: magoedte;bwren
ms.openlocfilehash: d3d06818d344b565d53cc4f4705b41dcfcf9a376
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="child-runbooks-in-azure-automation"></a>Podřízené runbooky ve službě Azure Automation
Je osvědčeným postupem v Azure Automation toowrite opakovaně použitelné modulární runbooky se samostatnou funkcí, které můžete používat ostatní runbooky. Nadřazená sada runbook často bude volat podřízené runbooky jeden nebo více tooperform požadované funkce. Existují dva způsoby toocall podřízeného runbooku a jsou mezi nimi značné rozdíly, kterým byste měli porozumět, aby mohla určit, která bude vhodné pro různé scénáře.

## <a name="invoking-a-child-runbook-using-inline-execution"></a>Vyvolání podřízeného runbooku pomocí vložené spouštění
tooinvoke přiřazený runbook z jiného runbooku použijete název hello hello sady runbook a zadejte hodnoty jeho parametrů stejně, jako byste používali aktivitu nebo rutinu.  Všechny sady runbook v hello stejný účet služby Automation jsou k dispozici tooall ostatní toobe použití tímto způsobem. Hello nadřazený runbook bude čekat hello podřízené sady runbook toocomplete před přesunutím toohello další řádek a jakýkoliv výstup se vrátí přímo toohello nadřazené.

Když vyvoláte přiřazený runbook běží ve stejné úloze jako nadřízený runbook hello hello. Budou existovat žádná informace v historii úlohy hello hello podřízeného runbooku, který byl spuštěn. Jakékoli výjimky a jakýkoli z podřízeného runbooku hello výstupní datový proud, budou přidruženy k nadřazené hello. To má za následek méně úlohy a umožňuje jejich jednodušší tootrack a tootroubleshoot od jakékoli výjimky vyvolané hello podřízené sady runbook a všechny jejich výstup datového proudu jsou přidružené hello nadřazené úloze.

Při publikování runbooku musí být všechny podřízené runbooky, které volá již publikován. Je to proto, že Azure Automation vytvoří přidružení se všemi podřízenými runbooky při runbooku. Pokud nejsou, hello nadřízený runbook se objeví toopublish správně, ale vygeneruje výjimku, když je spuštěno. V takovém případě můžete znovu publikovat nadřízený runbook hello v pořadí tooproperly odkaz hello podřízené runbooky. Pokud některý z podřízených runbooků hello jsou změnit, protože přidružení hello se již byly vytvořeny nepotřebujete toorepublish hello nadřízený runbook.

Hello parametry podřízeného runbooku s přiřazeným voláním můžou být jakéhokoli datového typu včetně složitých objektů a neexistuje žádný [serializace JSON](automation-starting-a-runbook.md#runbook-parameters) není při spuštění runbooku hello pomocí hello portálu pro správu Azure nebo s hello Rutina Start-AzureRmAutomationRunbook.

### <a name="runbook-types"></a>Typy runbooků
Typy, které můžete volat vzájemně:

* A [Powershellový runbook](automation-runbook-types.md#powershell-runbooks) a [grafické runbooky](automation-runbook-types.md#graphical-runbooks) můžete volat každý další vložené (obě jsou na základě prostředí PowerShell).
* A [runbook pracovního postupu Powershellu](automation-runbook-types.md#powershell-workflow-runbooks) a runbooky pracovních postupů grafické prostředí PowerShell můžete volat každý další vložené (obě jsou na základě pracovního postupu Powershellu)
* Hello typy prostředí PowerShell a hello, které typy pracovního postupu Powershellu nelze volat navzájem vložené a musí používat Start AzureRmAutomationRunbook.

Při publikování pořadí věci:

* Hello publikovat pořadí záleží jenom sady runbook pro sady runbook PowerShell Workflow a pracovní postup grafické prostředí PowerShell.

Při volání pomocí vložené spouštění podřízeného runbooku grafický nebo pracovního postupu Powershellu použijete právě hello název sady runbook hello.  Při volání podřízeného runbooku prostředí PowerShell, musí před jeho název s *.\\*  toospecify, který hello skript se nachází v místním adresáři hello. 

### <a name="example"></a>Příklad
Hello následující ukázka volá sadu runbook podřízené test, který přijímá tři parametry, komplexní objekt, celé číslo a logickou hodnotu. výstup Hello hello podřízeného runbooku je přiřazený tooa proměnné.  V takovém případě hello podřízeného runbooku je runbook pracovního postupu Powershellu

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Následuje hello příkladě pomocí prostředí PowerShell runbook jako podřízené hello.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook.ps1 –VM $vm –RepeatCount 2 –Restart $true


## <a name="starting-a-child-runbook-using-cmdlet"></a>Spouštění podřízeného runbooku pomocí rutiny
Můžete použít hello [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) rutiny toostart sady runbook, jak je popsáno v [toostart sady runbook pomocí prostředí Windows PowerShell](automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Existují dva režimy použití pro tuto rutinu.  V jednom režimu hello rutina vrátí id úlohy hello při vytvoření hello podřízené úlohy pro podřízený runbook hello.  V hello jiných režim, ve kterém povolíte zadáním hello **-počkejte** parametr rutiny hello bude odložena hello podřízené úlohy dokončení a vrátí výstup hello z podřízeného runbooku hello.

Hello úloha z podřízeného runbooku spuštěná pomocí rutiny poběží v samostatné úloze z hello nadřízený runbook. To má za následek víc úloh než při vyvolání přiřazený runbook hello a dává je obtížnější tootrack. nadřazené Hello můžete spustit víc podřízených runbooků asynchronně bez čekání na každý toocomplete. Pro stejný druh paralelního provedení by volání přiřazených runbooků hello, hello muselo toouse hello [paralelní klíčové slovo](automation-powershell-workflow.md#parallel-processing).

Parametry pro podřízený runbook spuštěný pomocí rutiny jsou uvedeny jako zatřiďovací tabulky, jak je popsáno v [parametry Runbooku](automation-starting-a-runbook.md#runbook-parameters). Můžete použít jenom jednoduché datové typy. Pokud má hello runbook parametr s komplexního datového typu, pak ji musí být volaný jako přiřazený.

### <a name="example"></a>Příklad
Hello následující příklad spouští podřízený runbook s parametry a potom počká pro něj toocomplete pomocí hello Start AzureRmAutomationRunbook-počkejte parametr. Po dokončení její výstup se shromažďují ze hello podřízené sady runbook.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResourceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Porovnání metod pro volání podřízeného runbooku
Hello následující tabulka shrnuje hello rozdíly mezi hello dvě metody pro volání sady runbook z jiné sady runbook.

|  | Vložené | Rutina |
|:--- |:--- |:--- |
| Úloha |Podřízené runbooky spuštěné ve stejné úloze jako nadřazený hello hello. |Pro podřízený runbook hello se vytvoří samostatná úloha. |
| Provádění |Nadřízený runbook čeká hello podřízené sady runbook toocomplete než budete pokračovat. |Nadřízený runbook pokračuje hned po spuštění podřízeného runbooku *nebo* nadřízený runbook čeká hello podřízené úlohy toofinish. |
| Výstup |Nadřízený runbook může získat výstup přímo z podřízeného runbooku. |Nadřízený runbook musí načíst výstup z úlohy podřízeného runbooku *nebo* nadřízený runbook může získat výstup přímo z podřízeného runbooku. |
| Parametry |Hodnoty pro parametry podřízeného runbooku hello se zadávají samostatně a můžete použít jakéhokoli datového typu. |Hodnoty pro hello podřízeného runbooku parametry musí zkombinovat do jedné zatřiďovací tabulky a může obsahovat jenom jednoduché, pole a objektu, že datové typy, které využívají serializaci JSON. |
| Účet Automation |Nadřízený runbook lze použít pouze podřízené sady runbook v hello stejný účet automation. |Nadřízený runbook můžete použít podřízeného runbooku z jakékoli účtu automation na hello stejného předplatného Azure a i jiné předplatné Pokud máte tooit připojení. |
| Publikování |Podřízené sady runbook musí publikovat před publikováním nadřazené sady runbook. |Podřízené sady runbook musí být publikovaný kdykoli před nadřízeného runbooku. |

## <a name="next-steps"></a>Další kroky
* [Spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md)
* [Výstup a zprávy ve službě Azure Automation Runbooku](automation-runbook-output-and-messages.md)

