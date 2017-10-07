---
title: "aaaAzure typy Runbooků Automation | Microsoft Docs"
description: "Popisuje různé typy hello sad runbook, které můžete použít v Azure Automation a důležité informace, které byste měli vzít v úvahu při určování, které toouse typu. "
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9265c975-4281-4819-a84f-d86641277f36
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/01/2017
ms.author: bwren
ms.openlocfilehash: c28aa57c77025764b16784372308a4ff2f596914
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-runbook-types"></a>Typy runbooků služby Azure Automation
Služby Azure Automation podporuje čtyři typy sad runbook, které jsou popsány v následující tabulce hello.  Hello níže uvedených částech poskytují další informace o jednotlivých typech včetně posouzení když toouse každý.

| Typ | Popis |
|:--- |:--- |
| [Grafický](#graphical-runbooks) |Na základě prostředí Windows PowerShell a vytvořeno a upravená zcela v grafický editor na portálu Azure. |
| [Pracovní postup grafické prostředí PowerShell](#graphical-runbooks) |Na základě pracovního postupu prostředí Windows PowerShell a vytvořeno a upravená zcela v hello grafický editor na portálu Azure. |
| [PowerShell](#powershell-runbooks) |Text runbook založené na skriptu prostředí Windows PowerShell. |
| [Pracovní postup PowerShellu](#powershell-workflow-runbooks) |Text runbook podle pracovního postupu prostředí Windows PowerShell. |

## <a name="graphical-runbooks"></a>Grafické runbooky
[Grafické](automation-runbook-types.md#graphical-runbooks) a runbooky pracovních postupů grafické prostředí PowerShell jsou vytvořeny a upravovat pomocí grafického editoru hello v hello portálu Azure.  Můžete exportovat, je soubor tooa a pak je importovat do jiného účtu automation, ale nelze vytvořit nebo upravit je jiný nástroj.  Grafické runbooky generování kódu prostředí PowerShell, ale nemůžou přímo zobrazovat nebo upravovat kód hello. Grafické runbooky nemůže být převedená tooone hello [textových formátů](automation-runbook-types.md), ani lze sadu runbook text převedený toographical formátu. Grafické runbooky může být převedená tooGraphical runbooky pracovních postupů Powershellu během importu a naopak.

### <a name="advantages"></a>Výhody
* Visual vložení odkazu nakonfigurovat pro tvorbu modelu  
* Zaměřit se na tok dat prostřednictvím procesu hello  
* Vizuální představují procesů správy  
* Zahrnout další sady runbook jako podřízené sady runbook toocreate vysoké úrovně pracovních postupů  
* Podporuje častější modulární programování  


### <a name="limitations"></a>Omezení
* Nelze upravit runbook mimo portál Azure.
* Může vyžadovat kód aktivity, který obsahuje prostředí PowerShell kód tooperform komplexní logiku.
* Nelze zobrazit, nebo přímo upravit kód hello prostředí PowerShell, který byl vytvořený hello grafický workflow. Všimněte si, že můžete zobrazit hello kód, který vytvoříte v žádné aktivity kódu.

## <a name="powershell-runbooks"></a>Powershellové runbooky
Powershellové runbooky jsou založené na prostředí Windows PowerShell.  Přímo upravovat kód hello hello sady runbook pomocí textového editoru hello v hello portálu Azure.  Můžete použít také všechny offline textovém editoru a [importovat hello runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) do Azure Automation.

### <a name="advantages"></a>Výhody
* Implementujte všechny komplexní logiku kódem Powershellu bez další složitosti hello pracovního postupu prostředí PowerShell. 
* Runbook se spustí rychleji než runbooky pracovních postupů Powershellu, protože nepotřebuje toobe zkompilovat dřív, než spustíte.

### <a name="limitations"></a>Omezení
* Musí být obeznámeni s skriptů prostředí PowerShell.
* Nelze použít [paralelní zpracování](automation-powershell-workflow.md#parallel-processing) tooperform více akcí paralelně.
* Nelze použít [kontrolní body](automation-powershell-workflow.md#checkpoints) tooresume runbook v případě chyby.
* Runbooky pracovních postupů Powershellu a grafické runbooky lze pouze zahrnuty jako podřízené sady runbook pomocí rutiny Start-AzureAutomationRunbook hello, která vytvoří novou úlohu.

### <a name="known-issues"></a>Známé problémy
Toto jsou aktuální známé problémy s Powershellovými runbooky.

* Powershellové runbooky nejde nelze načíst nezašifrované [variabilní prostředek](automation-variables.md) s hodnotou null.
* Nelze načíst Powershellové runbooky [variabilní prostředek](automation-variables.md) s  *~*  v názvu hello.
* Get-Process ve smyčce v prostředí PowerShell runbook může dojít k chybě po přibližně 80 iterací. 
* Powershellový runbook může selhat, pokud najednou pokusí toowrite velmi velký objem dat toohello výstupního datového proudu.   Tento problém můžete vyřešit obvykle podle výstupu právě hello informace, které je třeba při práci s rozsáhlé objekty.  Například místo výstup podobný vytvořeného *Get-Process*, výstup můžete právě hello požadované pole s *Get-Process | Vyberte název_procesu procesoru*.

## <a name="powershell-workflow-runbooks"></a>Runbooky pracovních postupů Powershellu
Runbooky pracovních postupů Powershellu jsou text sad runbook na základě [pracovního postupu prostředí Windows PowerShell](automation-powershell-workflow.md).  Přímo upravovat kód hello hello sady runbook pomocí textového editoru hello v hello portálu Azure.  Můžete použít také všechny offline textovém editoru a [importovat hello runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) do Azure Automation.

### <a name="advantages"></a>Výhody
* Implementujte všechny komplexní logiku s kódem pracovního postupu Powershellu.
* Použití [kontrolní body](automation-powershell-workflow.md#checkpoints) tooresume runbook v případě chyby.
* Použití [paralelní zpracování](automation-powershell-workflow.md#parallel-processing) tooperform více akcí paralelně.
* Může obsahovat jiné grafické runbooky a runbooky pracovních postupů Powershellu jako podřízené sady runbook toocreate vysoké úrovně pracovních postupů.

### <a name="limitations"></a>Omezení
* Autor musí být obeznámeni s pracovním postupem prostředí PowerShell.
* Sady Runbook musí řešit hello další složitosti pracovního postupu prostředí PowerShell, jako [deserializovat objekty](automation-powershell-workflow.md#code-changes).
* Runbook provede toostart delší než Powershellové runbooky vzhledem k tomu, že je nutné toobe zkompilovat dřív, než spustíte.
* Powershellové runbooky může být pouze zahrnuta jako podřízené sady runbook pomocí rutiny Start-AzureAutomationRunbook hello, která vytvoří novou úlohu.

## <a name="considerations"></a>Požadavky
Byste měli vzít do účtu hello následující další rozhodnutí při určování, které typ toouse konkrétní sady runbook.

* Sady runbook nelze převést z typu grafické tootextual nebo naopak.
* Existují omezení pomocí runbooků různých typů jako podřízené sady runbook.  V tématu [podřízené runbooky ve službě Azure Automation](automation-child-runbooks.md) Další informace.

## <a name="next-steps"></a>Další kroky
* toolearn Další informace o vytváření grafického runbooku, najdete v části [vytváření grafického obsahu ve službě Azure Automation](automation-graphical-authoring-intro.md)
* toounderstand hello rozdíly mezi prostředí PowerShell a prostředí PowerShell pracovní postupy pro sady runbook najdete v tématu [Learning pracovního postupu prostředí Windows PowerShell](automation-powershell-workflow.md)
* Další informace o tom, jak toocreate nebo import Runbooku, najdete v [vytvoření nebo import Runbooku](automation-creating-importing-runbook.md)

