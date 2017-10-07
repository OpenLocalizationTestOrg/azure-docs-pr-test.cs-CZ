---
title: "aaaTesting sady runbook ve službě Azure Automation | Microsoft Docs"
description: "Před publikováním runbooku ve službě Azure Automation, můžete ji otestovat tooensure, který funguje podle očekávání.  Tento článek popisuje, jak tootest sady runbook a zobrazíte jeho výsledek."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 7f7db785-52c0-4613-aa12-b02fd32a5182
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/12/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 8c531f702699d586f8215d4c171cb0ecf94732b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="testing-a-runbook-in-azure-automation"></a>Testování runbooku ve službě Azure Automation
Při testování runbooku hello [koncept](automation-creating-importing-runbook.md#publishing-a-runbook) se spustí a všechny akce, které provádí se dokončí. Historie úlohy je vytvořen, avšak hello [výstup](automation-runbook-output-and-messages.md#output-stream) a [upozornění a chyb](automation-runbook-output-and-messages.md#message-streams) datových proudů se zobrazují v hello výstup testovacího podokna. Zprávy toohello [podrobný datový proud](automation-runbook-output-and-messages.md#message-streams) se zobrazují v podokně výstup hello pouze v případě hello [proměnnou $VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) tooContinue nastavena.

I když je právě spuštěna verze konceptu hello, hello runbook dál normálně spouští pracovní postup hello a dělá všechny akce u prostředků v prostředí hello. Z tohoto důvodu jste měli runbooky testovat jenom na prostředky mimo produkční.

Hello postup tootest [typ runbooku](automation-runbook-types.md) hello stejné, a není žádný rozdíl ve testování mezi hello textový editor a hello grafický editor v hello portálu Azure.  

## <a name="tootest-a-runbook-in-hello-azure-portal"></a>tootest sady runbook v hello portálu Azure
Můžete spolupracovat s kterýmkoli [typ runbooku](automation-runbook-types.md) v hello portálu Azure.

1. Otevřete hello koncept runbooku hello buď hello [textový editor](automation-edit-textual-runbook.md) nebo [grafický editor](automation-graphical-authoring-intro.md).
2. Klikněte na tlačítko hello **Test** tlačítko tooopen hello testovací okno.
3. Pokud hello runbook obsahuje parametry, budou uvedené v levém podokně hello, kde můžete zadat hodnoty toobe používá pro hello test.
4. Pokud chcete testovací hello toorun na [hybridní pracovní proces Runbooku](automation-hybrid-runbook-worker.md), pak změňte **spustit nastavení** příliš**hybridní pracovní proces** a vyberte hello název hello cílovou skupinu.  Jinak použijte výchozí hello **Azure** toorun hello testování v cloudu hello.
5. Klikněte na tlačítko hello **spustit** tlačítko toostart hello testu.
6. Pokud je hello runbook [pracovního postupu Powershellu](automation-runbook-types.md#powershell-workflow-runbooks) nebo [grafický](automation-runbook-types.md#graphical-runbooks), pak můžete zastavit nebo pozastavit ji, pokud se testuje u tlačítka hello pod hello podokno výstup. Při hello runbook pozastavíte, dokončí aktuální aktivitu hello teprve pak se pozastaví. Jakmile hello runbook pozastavena, můžete případně ji zastavte nebo ho restartovat.
7. Zkontrolujte výstup hello hello runbook v podokně výstup hello.

## <a name="next-steps"></a>Další kroky
* toolearn jak toocreate nebo import runbooku, najdete v části [vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md)
* toolearn Další informace o vytváření grafického obsahu, najdete v části [vytváření grafického obsahu ve službě Azure Automation](automation-graphical-authoring-intro.md)
* tooget kroky s runbooky pracovních postupů Powershellu, najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md)
* toolearn Další informace o konfiguraci runboks tooreturn stavových zpráv a chyby, včetně doporučené postupy, najdete v části [Runbook výstup a zprávy v Azure Automation.](automation-runbook-output-and-messages.md)

