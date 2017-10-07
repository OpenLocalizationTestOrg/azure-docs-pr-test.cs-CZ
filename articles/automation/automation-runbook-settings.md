---
title: "nastavení aaaRunbook | Microsoft Docs"
description: "Popisuje hello nastavení konfigurace pro sady runbook ve službě Azure Automation a jak toochange je pomocí obou hello portálu pro správu Azure a prostředí Windows PowerShell."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: a726f20c-a952-48b8-88ee-36d76aa3ac61
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/11/2016
ms.author: bwren
ms.openlocfilehash: 6f0ef09c148355a351464424c22c33df9300f0dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-settings"></a>Nastavení runbooku
Každá sada runbook ve službě Azure Automation má několik nastavení, které jej pomáhají identifikovat toobe a toochange její chování při protokolování. Každá z těchto nastavení je popsána níže následují procedury na postupy toomodify je.

## <a name="settings"></a>Nastavení
### <a name="name-and-description"></a>Název a popis
Hello název sady runbook nelze změnit po jeho vytvoření. Hello popis je volitelný a může být až too512 znaků.

### <a name="tags"></a>Značky
Značky povolit, že jste tooassign různá slova a frází toohelp sadu runbook identifikovat. Například při odeslání runbook toohello [Galerie prostředí PowerShell](https://www.powershellgallery.com/), můžete zadat konkrétní značky tooidentify kategorie, které hello runbook by měl být uvedený v. Můžete určit více značek pro sadu runbook oddělte je čárkami.

### <a name="logging"></a>Protokolování
Ve výchozím nastavení nezapisují podrobné a průběh záznamy historie toojob. Můžete změnit hello nastavení pro konkrétní runbook toolog tyto záznamy. Další informace o tyto záznamy, najdete v části [výstup a zprávy Runbooku](automation-runbook-output-and-messages.md).

## <a name="changing-runbook-settings"></a>Změna nastavení sady runbook

### <a name="changing-runbook-settings-with-hello-azure-portal"></a>Změna nastavení sady runbook pomocí portálu Azure hello
Můžete změnit nastavení sady runbook v portálu Azure hello hello **nastavení** okno hello sady runbook.

1. V hello portálu Azure, vyberte **automatizace** a pak klikněte hello název účtu automation.
2. Vyberte hello **Runbooky** kartě.
3. Klikněte na název hello sady runbook a jste okno nastavení směrovanou toohello hello sady runbook. Odsud můžete zadat nebo změnit značky, hello popis sady runbook, konfigurovat protokolování a nastavení pro trasování a přístup k toohelp Podpora nástrojů pro řešení problémů.     

### <a name="changing-runbook-settings-with-windows-powershell"></a>Změna nastavení sady runbook pomocí prostředí Windows PowerShell
Můžete použít hello [Set-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) rutiny toochange hello nastavení sady runbook. Pokud chcete toospecify více značek, můžete buď zadat pole nebo jeden řetězec s parametrem značky toohello hodnoty oddělené čárkami. Můžete získat aktuální značky hello s hello [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).

Hello následující vzorové příkazy ukazují, jak tooset hello vlastností pro sadu runbook. Tato ukázka přidá tři značky toohello existující značky a určuje, zda mají být protokolovány podrobných záznamů.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a>Další kroky
* jak zjistit, toocreate a načtení výstupní a chybové zprávy ze sady runbook, toolearn [výstup a zprávy Runbooku](automation-runbook-output-and-messages.md) 
* jak zjistit, tooadd sady runbook, která již vyvinula hello komunity nebo jiné zdroje nebo toocreate vlastní runbook toounderstand [vytvoření nebo import Runbooku](automation-creating-importing-runbook.md) 

