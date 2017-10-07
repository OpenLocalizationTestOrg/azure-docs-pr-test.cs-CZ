---
title: "provádění aaaRunbook ve službě Azure Automation | Microsoft Docs"
description: "Popisuje hello podrobnosti o způsobu zpracování sady runbook ve službě Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: d10c8ce2-2c0b-4ea7-ba3c-d20e09b2c9ca
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/17/2017
ms.author: bwren
ms.openlocfilehash: bdb535675443353d44640bc7773de3f9dac5e42c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-execution-in-azure-automation"></a>Spuštění sady Runbook ve službě Azure Automation
Při spuštění sady runbook ve službě Azure Automation se vytvoří úloha. Úloha je instance jednoho spuštění sady runbook. Azure Automation pracovního procesu je přiřazen toorun Každá úloha. Když zaměstnanci jsou sdíleny více účtů Azure, úlohy z různých účtů Automation jsou izolované od sebe navzájem. Nemáte kontrolu nad které žádost hello služby pracovního procesu pro úlohu.  Jedné sady runbook může mít několik úloh spuštěných současně. Při zobrazení seznamu hello sad runbook hello portálu Azure, zobrazí se seznam hello stav všech úloh, které byly zahájeny pro každou sadu runbook. Hello seznam úloh pro každou sadu runbook můžete zobrazit v pořadí tootrack hello stav každého z nich. Popis stavy hello různé úlohy najdete v tématu [stavy úlohy](#job-statuses).

Hello následující diagram znázorňuje hello životní cyklus úlohy sady runbook pro [grafické runbooky](automation-runbook-types.md#graphical-runbooks) a [runbooky pracovních postupů Powershellu](automation-runbook-types.md#powershell-workflow-runbooks).

![Stavy úlohy – pracovní postup prostředí PowerShell](./media/automation-runbook-execution/job-statuses.png)

Hello následující diagram znázorňuje hello životní cyklus úlohy sady runbook pro [Powershellové runbooky](automation-runbook-types.md#powershell-runbooks).

![Stavy úlohy - skript prostředí PowerShell](./media/automation-runbook-execution/job-statuses-script.png)

Vaše úlohy mají přístup tooyour Azure prostředky tím, že připojení tooyour předplatného Azure. Mají pouze přístup tooresources ve vašem datovém centru Pokud tyto prostředky jsou přístupné z veřejného cloudu hello.

## <a name="job-statuses"></a>Stavy úlohy
Hello následující tabulka popisuje hello různé stavy, které jsou u úlohy nastat.

| Status | Popis |
|:--- |:--- |
| byla dokončena |Hello úloha byla úspěšně dokončena. |
| Se nezdařilo |Pro [pracovní postup prostředí PowerShell a grafický runbook](automation-runbook-types.md), sada runbook hello selhala toocompile.  Pro [skript prostředí PowerShell runbooky](automation-runbook-types.md), sada runbook hello selhala toostart nebo hello úlohy došlo k výjimce. |
| Chyba, čekání na prostředky |Hello úloha se nezdařila, protože bylo dosaženo hello [úloha dostatečný podíl](#fairshare) omezit třikrát a spustit z hello stejné kontrolního bodu nebo z hello spuštění sady runbook hello pokaždé, když. |
| Ve frontě |Hello úloha nečeká na prostředky na k dispozici automatizaci pracovního procesu toocome tak, aby jeho spuštění. |
| Spouštění |Hello úloha byla přiřazena tooa pracovního procesu, a systém hello v hello proces jeho spuštění. |
| Obnovení |systém Hello je v hello proces obnovení hello úlohy po bylo pozastaveno. |
| Běžící (Spuštěno) |Úloha Hello je spuštěná. |
| Spuštění, čekání na prostředky |Hello úlohy byl odpojen protože bylo dosaženo hello [úloha dostatečný podíl](#fairshare) limit. Obnoví krátce od svého posledního kontrolního bodu. |
| Zastaveno |Hello úloha byla zastavena uživatelem hello před dokončením. |
| Zastavování |systém Hello je v hello proces zastavit úlohu hello. |
| pozastaveno |Hello úlohu pozastavil uživatel hello, hello systém nebo příkaz v runbooku hello. Úlohu, která je pozastaven, můžete spustit znovu a poté obnovte od svého posledního kontrolního bodu nebo od začátku hello hello sady runbook, pokud nemá kontrolní body. Hello runbook bude pouze pozastaveno systémem hello, když dojde k výjimce. Ve výchozím nastavení, je příliš hodnotu ErrorActionPreference**pokračovat**, což znamená tuto úlohu hello neustále běží na chybu. Pokud je tato předvolba proměnné nastavená příliš**Zastavit**, pak pozastaví úlohu hello na chybu.  Platí příliš[pracovní postup prostředí PowerShell a grafický runbook](automation-runbook-types.md) pouze. |
| Přechodem do režimu spánku |Hello systém se pokouší toosuspend hello úlohu na žádost hello hello uživatele. Hello runbook dosažení následujícího kontrolního bodu předtím, než se může pozastavit. Pokud již uplynul svého posledního kontrolního bodu, pak dokončí předtím, než se může pozastavit.  Platí příliš[pracovní postup prostředí PowerShell a grafický runbook](automation-runbook-types.md) pouze. |

## <a name="viewing-job-status-from-hello-azure-portal"></a>Zobrazení stavu úlohy z hello portálu Azure
Můžete zobrazit souhrnnou stav všechny úlohy sady runbook nebo podrobnostem úlohu konkrétní sady runbook v hello portál Azure nebo konfigurace integrace stav úlohy sady runbook tooforward pracovního prostoru analýzy protokolů Microsoft Operations Management Suite (OMS) a datové proudy úlohy.  Další informace o integraci do OMS analýzy protokolů najdete v tématu [předávání zpráv o stavu úlohy a datové proudy úlohy z automatizace tooLog Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).  

### <a name="automation-runbook-jobs-summary"></a>Souhrn úlohy sady runbook automatizace
Na hello napravo od účtu Automation vybrané, se zobrazí shrnutí všech hello úlohy sady runbook pro vybraný účet automatizace v rámci **Statistika projektu** dlaždici.<br><br> ![Dlaždice úlohy statistiky](./media/automation-runbook-execution/automation-account-job-status-summary.png).<br> Tuto dlaždici zobrazí počet a grafické znázornění hello stav úlohy pro všechny úlohy provést.  

Kliknutím na dlaždici hello uvede hello **úlohy** okno, které obsahuje souhrnný seznam všech úloh provést, se stavem, provádění úlohy a spuštění a dokončení.<br><br> ![Okno úlohy účet Automation.](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)<br><br>  Můžete filtrovat hello seznam úloh tak, že vyberete **filtrujte úlohy** a filtrovat na konkrétní sadu runbook, stav úlohy, nebo z rozevíracího seznamu hello hello toosearch rozsah datum a čas v rámci.<br><br> ![Stav úlohy filtru](./media/automation-runbook-execution/automation-account-jobs-filter.png)

Alternativně můžete zobrazit podrobnosti souhrnu úlohy pro konkrétní sadu runbook tak, že vyberete dané sady runbook z hello **Runbooky** okno v účtu Automation a pak vyberte hello **úlohy** dlaždici.  To představuje hello **úlohy** okně a z ní můžete kliknutím na tooview záznamů úlohy hello podrobnosti a výstup.<br><br> ![Okno úlohy účet Automation.](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)<br> 

### <a name="job-summary"></a>Souhrn úlohy
Můžete zobrazit seznam všech hello úloh, které byly vytvořeny pro konkrétní runbook a jejich aktuální stav. Tento seznam můžete filtrovat podle stavu úlohy a hello rozsah dat pro hello poslední změna toohello úlohy. tooview jeho podrobné informace a výstup, klikněte na název hello úlohy. Hello podrobné zobrazení úlohy hello zahrnuje hello hodnoty pro parametry runbooku hello, které byly poskytnuty toothat úlohy.

Můžete použít následující kroky tooview hello úlohy runbooku hello.

1. V hello portálu Azure, vyberte **automatizace** a pak vyberte název hello účet Automation.
2. Z centra hello vyberte **sady Runbook** a pak na hello **sady Runbook** okno hello seznamu vyberte sadu runbook.
3. V okně hello hello vybrané sady runbook, klikněte na tlačítko hello **úlohy** dlaždici.
4. Klikněte na jednu z úloh hello v seznamu hello a na okno Podrobnosti úlohy sady runbook hello můžete zobrazit podrobnosti a výstup.

## <a name="retrieving-job-status-using-windows-powershell"></a>Načtení stavu úlohy pomocí prostředí Windows PowerShell
Můžete použít hello [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) tooretrieve hello úlohy vytvořené pro runbook a hello podrobnosti konkrétní úlohy. Pokud runbook spustíte pomocí prostředí Windows PowerShell [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx), pak vrátí hello výsledná úloha. Použití [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx)tooget výstup výstupu úlohy.

Hello následující vzorové příkazy načíst hello poslední úlohu ukázkového runbooku a zobrazí její stav, hello hodnot pro parametry runbooku hello a hello výstup z úlohy hello.

    $job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
    –RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
    $job.Status
    $job.JobParameters
    Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output

## <a name="fair-share"></a>Úloha dostatečný podíl
V pořadí zdrojů tooshare mezi všechny sady runbook v cloudu hello bude Azure Automation dočasně uvolnit všechny úlohy, až po spuštění tři hodiny.  Během této doby úlohy pro [sady runbook pomocí prostředí PowerShell](automation-runbook-types.md#powershell-runbooks) se zastaví a nebude ji restartovat.  Dobrý den zobrazí stav úlohy **Zastaveno**.  Tento typ runbooku je vždy restartován od začátku hello nepodporují kontrolní body.  

[Založené na prostředí PowerShell pracovního postupu sady runbook](automation-runbook-types.md#powershell-workflow-runbooks) bude pokračovat znovu od jejich poslední [kontrolního bodu](https://docs.microsoft.com/system-center/sma/overview-powershell-workflows#bk_Checkpoints).  Po spuštění tři hodiny, bude úloha sady runbook hello pozastaví hello service a její stav se zobrazuje **spuštěna, čekání na prostředky**.  Jakmile izolovaném prostoru k dispozici, hello runbook automaticky restartuje služba Automation hello a obnoví z posledního kontrolního bodu hello.  To je normální chování pracovního postupu Powershellu pozastavit nebo restartovat.  Překročí-li hello runbook znovu tři hodiny modulu runtime, hello postup se opakuje, až toothree časy.  Po třetím restartování hello, pokud hello runbook ještě nebyla dokončena v tři hodiny, pak hello úloha sady runbook se nezdařilo a ukazuje stav úlohy hello **se nezdařilo, čekání na prostředky**.  V takovém případě se zobrazí následující výjimka hello selhání hello.

*Úloha Hello nemůže pokračovat, systémem, protože bylo opakovaně vyloučeno z hello stejné kontrolního bodu. Zkontrolujte prosím, že vaše sada Runbook neprovádí bez zachování stavu náročná operace.*

Toto je tooprotect hello service ze sady runbook běží hodně dlouho bez dokončení, protože nejsou možné toomake ho následujícího kontrolního bodu toohello bez odpojení znovu.

Pokud hello runbook nemá žádné kontrolní body nebo hello úlohy nedosáhla hello první kontrolní bod před odpojení, pak znovu od začátku hello.  

Při vytváření sady runbook, ujistěte se, že toorun čas hello žádné aktivity mezi dvěma kontrolních bodů není delší než 3 hodiny. Může být nutné tooadd kontrolní body tooyour runbook tooensure, nemá dosažení maximálního počtu tři hodiny nebo rozdělit dlouho spuštěná operace. Vaše sada runbook může například provádět nové indexování velké databáze SQL. Pokud tato jedné operace neskončí v rámci hello správného uvolněna limit sdílené složky a potom hello úlohy a spuštěno znovu od začátku hello. V takovém případě by měl rozdělit hello nové indexování operace do více kroků, například novou indexaci jedna tabulka současně a vložit kontrolní bod po každé operaci, aby hello úlohy mohly pokračovat po poslední operaci toocomplete hello.

## <a name="next-steps"></a>Další kroky
* toolearn Další informace o hello různé metody, které se dají použít toostart sady runbook ve službě Azure Automation najdete v části [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md)

