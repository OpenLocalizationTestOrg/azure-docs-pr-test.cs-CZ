---
title: "aaaSource integrace ovládacích prvků ve službě Azure Automation | Microsoft Docs"
description: "Tento článek popisuje integrace ovládacích prvků zdrojového s Githubu ve službě Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 224d7375-9887-44dd-b137-06ffe396a4b4
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/21/2016
ms.author: magoedte;sngun
ms.openlocfilehash: 6ceee1de8065fafe85a13bbd7f585e74dbc96b47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="source-control-integration-in-azure-automation"></a>Integrace správy zdrojového kódu ve službě Azure Automation
Integrace ovládacích prvků zdrojového umožňuje tooassociate sady runbook v automatizace účet tooa Githubu úložiště řízení zdrojů. Správa zdrojového kódu vám umožní tooeasily se svým týmem spolupracovat, sledování změn a vrácení verze tooearlier sadu runbook. Správa zdrojového kódu umožňuje například můžete toosync různých větví ve správě zdrojového kódu tooyour vývoj, testovací nebo produkční účty Automation, takže je easy toopromote kód, který byl testován v provozním prostředí tooyour vývoj automatizace účet.

Správa zdrojového kódu vám umožní toopush kód z Azure Automation toosource řízení nebo runbooky z zdroj ovládacího prvku tooAzure automatizace pro vyžádání obsahu. Tento článek popisuje, jak řídit tooset se zdroj v prostředí Azure Automation. Jsme se začněte tím, že konfigurace úložiště GitHub tooaccess Azure Automation a provede různé operace, které lze provést pomocí integrace ovládacích prvků zdrojového. 

> [!NOTE]
> Správa zdrojového kódu podporuje stahování a předání [runbooky pracovních postupů Powershellu](automation-runbook-types.md#powershell-workflow-runbooks) a také [Powershellové runbooky](automation-runbook-types.md#powershell-runbooks). [Grafické runbooky](automation-runbook-types.md#graphical-runbooks) ještě nejsou podporovány.<br><br>
> 
> 

Pokud již máte účet GitHub existují dvě jednoduchých kroků požadované tooconfigure zdrojového kódu pro svůj účet Automation a pouze jeden. Jsou:

## <a name="step-1--create-a-github-repository"></a>Krok 1 – Vytvoření úložiště GitHub
Pokud již máte účet GitHub a úložiště, který chcete toolink tooAzure automatizace a pak přihlášení tooyour existující účet a začít od kroku 2 níže. Jinak přejděte příliš[Githubu](https://github.com/), zaregistrujte si nový účet a [vytvořit nové úložiště](https://help.github.com/articles/create-a-repo/).

## <a name="step-2--set-up-source-control-in-azure-automation"></a>Krok 2 – nastavení zdrojového kódu ve službě Azure Automation
1. V okně účtu Automation hello v hello portálu Azure, klikněte na tlačítko **nastavit řízení zdrojů.** 
   
    ![Nastavit řízení zdrojů](media/automation-source-control-integration/automation_01_SetUpSourceControl.png)
2. Hello **správy zdrojového kódu** otevře se okno, kde můžete nakonfigurovat údaje o vašem účtu GitHub. Níže je seznam hello tooconfigure parametry:  
   
   | **Parametr** | **Popis** |
   |:--- |:--- |
   | Vyberte zdroj |Vyberte zdroj hello. V současné době pouze **Githubu** je podporována. |
   | Autorizace |Klikněte na tlačítko hello **Autorizovat** tlačítko toogrant Azure Automation přístup tooyour úložiště GitHub. Pokud jste již přihlášeni tooyour účtu GitHub v jiné časové období a pak hello jsou použita pověření tohoto účtu. Po úspěšné autorizace hello okno se zobrazí vaše uživatelské jméno Githubu pod **autorizace vlastnost**. |
   | Vyberte úložiště |Vyberte úložiště GitHub hello seznam dostupné úložiště. |
   | Vyberte firemní pobočky |Vyberte větev z hello seznam dostupných větve. Pouze hello **hlavní** větev se zobrazí, pokud jste nevytvořili žádné větve. |
   | Cesta ke složce sady Runbook |Cesta ke složce runbook Hello Určuje cestu hello v úložišti GitHub hello, ze kterého mají být toopush nebo přijetí změn kódu. Musí být zadána ve formátu hello **/název_složky/subfoldername**. Jenom runbooky hello sady runbook ve složce v cestě bude synchronizovaná tooyour účet Automation. Sady Runbook v podsložkách hello hello cesty složky sady runbook bude **není** synchronizovat. Použití  **/**  toosync všechny sady runbook v rámci úložiště hello hello. |
3. Například, pokud máte úložiště s názvem **PowerShellScripts** složku s názvem, který obsahuje **RootFolder**, která obsahuje složku s názvem **podsložky**. Můžete použít následující řetězce toosync hello každé úrovni složky:
   
   1. toosync runbooků **úložiště**, je cesta ke složce sady runbook*/*
   2. toosync runbooků **RootFolder**, je cesta ke složce runbook */RootFolder*
   3. toosync runbooků **podsložky**, je cesta ke složce runbook */RootFolder/podsložky*.
4. Po dokončení konfigurace hello parametry, jsou zobrazeny na hello **okno nastavit řízení zdrojů.**  
   
    ![Konfigurace okna](media/automation-source-control-integration/automation_02_SourceControlConfigure.png)
5. Po kliknutí na tlačítko OK, integrace ovládacích prvků zdrojového je teď nakonfigurovaný na účtu Automation a je třeba aktualizovat vaše informace Githubu. Nyní můžete kliknout na na tuto část tooview všechny zdrojového kódu synchronizace historie úlohy.  
   
    ![Hodnoty úložiště](media/automation-source-control-integration/automation_03_RepoValues.png)
6. Po nastavení správy zdrojového kódu hello následující prostředky Automation se vytvoří ve vašem účtu Automation:  
   Dva [proměnných assetů](automation-variables.md) se vytvářejí.  
   
   * Proměnná Hello **Microsoft.Azure.Automation.SourceControl.Connection** obsahuje hodnoty hello hello připojovacího řetězce, jak je uvedeno níže.  
     
     | **Parametr** | **Hodnota** |
     |:--- |:--- |
     | Name (Název) |Microsoft.Azure.Automation.SourceControl.Connection |
     | Typ |Řetězec |
     | Hodnota |{"Větve":\<*název vaší firemní pobočky*>, "RunbookFolderPath":\<*cesta ke složce Runbook*>, "typ zprostředkovatele":\<*má hodnotu 1 pro GitHub*>, "Úložiště":\<*název ve vašem úložišti*>, "Username":\<*Githubu vaše uživatelské jméno*>} |

    * Proměnná Hello **Microsoft.Azure.Automation.SourceControl.OAuthToken**, obsahuje hello zabezpečené zašifrovanou hodnotu vaší OAuthToken.  

    |**Parametr**            |**Hodnota** |
    |:---|:---|
    | Name (Název)  | Microsoft.Azure.Automation.SourceControl.OAuthToken |
    | Typ | Unknown(Encrypted) |
    | Hodnota | <*Šifrované OAuthToken*> |  

    ![Proměnné](media/automation-source-control-integration/automation_04_Variables.png)  

    * **Automatizace správy zdrojového kódu** se přidá jako oprávnění aplikace tooyour účtu GitHub. aplikace hello tooview: na domovské stránce Githubu přejděte tooyour **profil** > **nastavení** > **aplikace**. Tato aplikace umožňuje Azure Automation toosync vaše tooan úložiště GitHub účet Automation.  

    ![Git aplikace](media/automation-source-control-integration/automation_05_GitApplication.png)


## <a name="using-source-control-in-automation"></a>Použití správy zdrojového kódu v automatizace
### <a name="check-in-a-runbook-from-azure-automation-toosource-control"></a>Vrácení se změnami sadu runbook z ovládacího prvku toosource Azure Automation.
Runbook se změnami můžete toopush hello změny provedené tooa runbook ve službě Azure Automation do úložiště řízení zdrojů. Tady jsou kroky hello toocheck v sady runbook:

1. Z vašeho účtu Automation [vytvořte nový textový runbook](automation-first-runbook-textual.md), nebo [upravit runbook služby existující, textovou](automation-edit-textual-runbook.md). Tato sada runbook může být v pracovním postupu Powershellu nebo sadu runbook skript prostředí PowerShell.  
2. Po úpravě runbooku, uložte ho a klikněte na tlačítko **vrácení se změnami** z hello **upravit** okno.  
   
    ![Tlačítko vrácení se změnami](media/automation-source-control-integration/automation_06_CheckinButton.png)

     > [!NOTE] 
     > Vrácení se změnami ve službě Azure Automation přepíše hello kód, který aktuálně existuje v zdrojového kódu. je Hello instrukce ekvivalent příkazového řádku Gitu toocheck v **přidat git + git potvrzení + git push**  

1. Když kliknete na tlačítko **vrácení se změnami**, budete vyzváni k potvrzení, klikněte na tlačítko Ano toocontinue.  
   
    ![Zpráva vrácení se změnami](media/automation-source-control-integration/automation_07_CheckinMessage.png)
2. Vrácení se změnami spustí hello zdroj řízení sady runbook: **synchronizace MicrosoftAzureAutomationAccountToGitHubV1**. Tato sada runbook připojí tooGitHub a nabízených oznámení změny z úložiště tooyour Azure Automation. tooview hello vrácení se změnami historie úlohy, přejděte zpět toohello **integrace ovládacích prvků zdrojového** a klikněte na okno úložiště synchronizace tooopen hello. Toto okno obsahuje všechny vaše úlohy řízení zdrojů.  Vyberte úlohu hello tooview a klikněte na podrobnosti tooview hello.  
   
    ![Vrácení se změnami sadu Runbook](media/automation-source-control-integration/automation_08_CheckinRunbook.png)
   
   > [!NOTE]
   > Zdroj řízení sady runbook jsou speciální automatizace sady runbook, které nemůžete zobrazit nebo upravit. Když se nezobrazí v seznamu sad runbook, zobrazí se úloh synchronizace zobrazovat v seznamu úloh.
   > 
   > 
3. Název Hello hello upravit runbook se odešle jako toohello vrácení se změnami runbook služby vstupní parametr. Můžete [zobrazení podrobností o úloze hello](automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) rozšířením sady runbook v **úložiště synchronizace** okno.  
   
    ![Vstup vrácení se změnami](media/automation-source-control-integration/automation_09_CheckinInput.png)
4. Po dokončení úlohy hello tooview hello změny, aktualizujte úložiště GitHub.  Měla by existovat potvrzení změn v úložišti zprávou potvrzení: **aktualizované *název sady Runbook* ve službě Azure Automation.**  

### <a name="sync-runbooks-from-source-control-tooazure-automation"></a>Synchronizace sady runbook z zdroj ovládacího prvku tooAzure automatizace
tlačítko Synchronizovat Hello v okně hello synchronizace úložiště vám umožní toopull všechny hello sady runbook v hello runbook cesta ke složce vašeho úložiště tooyour účet Automation. Hello stejného úložiště může být synchronizovaná toomore než jeden účet Automation. Tady jsou kroky toosync hello sady runbook:

1. Hello účet Automation, kde můžete nastavit řízení zdrojů, otevřete hello **synchronizace integrace/úložiště zdroj ovládacího prvku okno** a klikněte na tlačítko **synchronizace** a zobrazí se výzva s potvrzení zpráva, klikněte na tlačítko **Ano** toocontinue.  
   
    ![Tlačítko Synchronizovat](media/automation-source-control-integration/automation_10_SyncButtonwithMessage.png)
2. Synchronizace spouští hello runbook: **synchronizace MicrosoftAzureAutomationAccountFromGitHubV1**. Tato sada runbook připojí tooGitHub a vrátí hello změny z vašeho úložiště tooAzure automatizace. Měli byste vidět nové úlohy na hello **úložiště synchronizace** okno pro tuto akci. tooview podrobnosti o hello úloha synchronizace, klikněte na okno Podrobnosti úlohy tooopen hello.  
   
    ![Synchronizace sady Runbook](media/automation-source-control-integration/automation_11_SyncRunbook.png)

    > [!NOTE] 
    > K synchronizaci od správy zdrojového kódu přepíše hello koncept hello sady runbook, který aktuálně neexistuje v účtu Automation pro **všechny** sady runbook, které jsou právě ve zdrojové ovládacího prvku. Hello Git je ekvivalentní příkazového řádku instrukce toosync **vyžádání git**


## <a name="troubleshooting-source-control-problems"></a>Řešení potíží zdroj ovládacího prvku
Pokud nejsou žádné chyby s úlohou vrácení se změnami nebo synchronizace, stav úlohy hello pozastaví a v okně úlohy hello můžete zobrazit další podrobnosti o chybě hello.  Hello **všechny protokoly** část vám ukáže všechny hello prostředí PowerShell datové proudy přidružené k této úlohy. To zajistí, že jste s podrobnostmi hello toohelp opravte všechny problémy s vrácení se změnami nebo synchronizace. Také zobrazí se text hello posloupnost akcí, které byly při synchronizaci nebo ve vrácení sady runbook.  

![Obrázek AllLogs](media/automation-source-control-integration/automation_13_AllLogs.png)

## <a name="disconnecting-source-control"></a>Odpojuje se správa zdrojového kódu
Otevřete okno úložiště synchronizace hello toodisconnect z vašeho účtu GitHub a klikněte na tlačítko **odpojení**. Po odpojení zdrojového kódu sady runbook, které byly dříve synchronizované se stále na účtu Automation, ale okno hello úložiště synchronizace nebude povolen.  

  ![Tlačítko Odpojit](media/automation-source-control-integration/automation_12_Disconnect.png)

## <a name="next-steps"></a>Další kroky
Další informace o integrace ovládacích prvků zdrojového najdete v tématu hello následující prostředky:  

* [Služby Azure Automation: Integrace ovládacích prvků zdrojového ve službě Azure Automation](https://azure.microsoft.com/blog/azure-automation-source-control-13/)  
* [Hlas pro Oblíbené ve zdrojovém systému ovládací prvek](https://www.surveymonkey.com/r/?sm=2dVjdcrCPFdT0dFFI8nUdQ%3d%3d)  
* [Služby Azure Automation: Runbook zdroj ovládacího prvku pomocí Visual Studio Team Services integraci](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/)  

