---
title: "Testování runbooku ve službě Azure Automation | Microsoft Docs"
description: "Před publikováním runbooku ve službě Azure Automation, můžete otestovat k zajištění, že to funguje podle očekávání.  Tento článek popisuje postup testování sady runbook a zobrazíte jeho výsledek."
services: automation
documentationcenter: 
author: georgewallace
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
ms.openlocfilehash: 49e8dfa341940386f15932ec4346c8811effbf0b
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="testing-a-runbook-in-azure-automation"></a>Testování runbooku ve službě Azure Automation
Při testování sady runbook [koncept](automation-creating-importing-runbook.md#publishing-a-runbook) se spustí a všechny akce, které provádí se dokončí. Nevytvoří se žádné historie úlohy, ale [výstup](automation-runbook-output-and-messages.md#output-stream) a [upozornění a chyb](automation-runbook-output-and-messages.md#message-streams) datové proudy, které se zobrazují v testovací podokno výstup. Zprávy pro [podrobný datový proud](automation-runbook-output-and-messages.md#message-streams) se zobrazují v podokně výstupu pouze tehdy, pokud [proměnnou $VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) je nastaven na pokračovat.

I když je právě spuštěna verze konceptu, runbook dál normálně spouští pracovní postup a dělá všechny akce u prostředků v prostředí. Z tohoto důvodu jste měli runbooky testovat jenom na prostředky mimo produkční.

Postup testování jednotlivých [typ runbooku](automation-runbook-types.md) je stejné a že je žádné rozdíly při testování mezi textový editor a grafický editor na webu Azure portal.  

## <a name="to-test-a-runbook-in-the-azure-portal"></a>Otestování sady runbook na portálu Azure
Můžete spolupracovat s kterýmkoli [typ runbooku](automation-runbook-types.md) na portálu Azure.

1. Otevřete jeho koncept runbooku buď [textový editor](automation-edit-textual-runbook.md) nebo [grafický editor](automation-graphical-authoring-intro.md).
2. Klikněte **Test** tlačítko otevřete okno Test.
3. Pokud sada runbook obsahuje parametry, budou uvedené v levém podokně, kde můžete zadat hodnoty má být použit pro test.
4. Pokud chcete spustit test [hybridní pracovní proces Runbooku](automation-hybrid-runbook-worker.md), pak změňte **spustit nastavení** k **hybridní pracovní proces** a vyberte název cílové skupiny.  Jinak použijte výchozí **Azure** ke spuštění testu v cloudu.
5. Klikněte **spustit** tlačítko spuštění testu.
6. Pokud je sada runbook [pracovního postupu Powershellu](automation-runbook-types.md#powershell-workflow-runbooks) nebo [grafický](automation-runbook-types.md#graphical-runbooks), pak můžete zastavit nebo pozastavit ji, pokud se testuje tlačítky pod podoknem výstupu. Když runbook pozastavíte, dokončí aktuální aktivitu teprve pak se pozastaví. Jakmile je pozastavená sada runbook, můžete případně ji zastavte nebo ho restartovat.
7. Zkontrolujte výstup z runbooku v podokně výstup.

## <a name="next-steps"></a>Další kroky
* Další postupy k vytvoření nebo import sady runbook najdete v tématu [vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md)
* Další informace o vytváření grafického obsahu najdete v článku [Vytváření grafického obsahu v Azure Automation](automation-graphical-authoring-intro.md).
* První kroky s runbooky pracovních postupů PowerShellu najdete v článku [Můj první runbook pracovního postupu PowerShellu](automation-first-runbook-textual.md).
* Další informace o konfiguraci sady runbook vrátí stavové zprávy a chyby, včetně doporučené postupy, najdete v části [Runbook výstup a zprávy v Azure Automation.](automation-runbook-output-and-messages.md)

