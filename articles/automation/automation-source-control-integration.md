---
title: "Zdroj integrace ovládacích prvků ve službě Azure Automation | Microsoft Docs"
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
ms.openlocfilehash: 763bf5805e3a3cb95ad63c7a354dd3d4cd531b2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="source-control-integration-in-azure-automation"></a>Integrace správy zdrojového kódu ve službě Azure Automation
Integrace ovládacích prvků zdrojového umožňuje přidružit sady runbook ve vašem účtu Automation se úložištěm řízení zdrojů Githubu. Správa zdrojového kódu umožňuje snadno spolupracovat s týmem, sledovat změny a vrátit zpět na dřívější verze sadu runbook. Například Správa zdrojového kódu umožňuje synchronizovat různých větví ve správě zdrojového kódu do vaší vývoj, testovací nebo produkční účty Automation, což usnadňuje povýšit kód, který byl testován v vývojové prostředí pro vaše produkční automatizace účet.

Správa zdrojového kódu umožňuje nabízené kódu do správy zdrojového kódu ve službě Azure Automation nebo runbooků od správy zdrojového kódu pro Azure Automation pro vyžádání obsahu. Tento článek popisuje, jak nastavit zdrojového kódu ve vašem prostředí Azure Automation. Spustíme konfigurací Azure Automation pro přístup k vaší úložiště GitHub a provede různé operace, které lze provést pomocí integrace ovládacích prvků zdrojového. 

> [!NOTE]
> Správa zdrojového kódu podporuje stahování a předání [runbooky pracovních postupů Powershellu](automation-runbook-types.md#powershell-workflow-runbooks) a také [Powershellové runbooky](automation-runbook-types.md#powershell-runbooks). [Grafické runbooky](automation-runbook-types.md#graphical-runbooks) ještě nejsou podporovány.<br><br>
> 
> 

Existují dvě jednoduchých kroků, které jsou potřeba ke konfiguraci zdrojového kódu pro svůj účet Automation a jenom jedna, pokud již máte účet GitHub. Jsou:

## <a name="step-1--create-a-github-repository"></a>Krok 1 – Vytvoření úložiště GitHub
Pokud již máte účet GitHub a úložiště, který chcete propojit s Azure Automation a přihlášení k existující účet a začít od kroku 2 níže. Jinak přejděte na [Githubu](https://github.com/), zaregistrujte si nový účet a [vytvořit nové úložiště](https://help.github.com/articles/create-a-repo/).

## <a name="step-2--set-up-source-control-in-azure-automation"></a>Krok 2 – nastavení zdrojového kódu ve službě Azure Automation
1. V okně účtu Automation na portálu Azure klikněte na tlačítko **nastavit řízení zdrojů.** 
   
    ![Nastavit řízení zdrojů](media/automation-source-control-integration/automation_01_SetUpSourceControl.png)
2. **Správy zdrojového kódu** otevře se okno, kde můžete nakonfigurovat údaje o vašem účtu GitHub. Níže je uvedený seznam parametrů ke konfiguraci:  
   
   | **Parametr** | **Popis** |
   |:--- |:--- |
   | Vyberte zdroj |Vyberte zdroj. V současné době pouze **Githubu** je podporována. |
   | Autorizace |Klikněte **Autorizovat** tlačítko k udělení přístupu službě Azure Automation se svým úložištěm GitHub. Pokud jste již přihlášení k účtu GitHub v jiném okně, jsou použita pověření tohoto účtu. Po ověření je úspěšné, v okně se zobrazí vaše uživatelské jméno Githubu pod **autorizace vlastnost**. |
   | Vyberte úložiště |Vyberte ze seznamu dostupných úložišť úložiště GitHub. |
   | Vyberte firemní pobočky |Vyberte ze seznamu dostupných větví větev. Pouze **hlavní** větev se zobrazí, pokud jste nevytvořili žádné větve. |
   | Cesta ke složce sady Runbook |Cesta ke složce runbook Určuje cestu v úložišti GitHub, ze kterého chcete push nebo pull kódu. Musí být zadána ve formátu **/název_složky/subfoldername**. Jenom runbooky ve složce cesty sady runbook se budou synchronizovat s účtu Automation. Sady Runbook v podsložkách cesty ke složce runbook bude **není** synchronizovat. Použití  **/**  k synchronizaci všech sad runbook v rámci úložiště. |
3. Například, pokud máte úložiště s názvem **PowerShellScripts** složku s názvem, který obsahuje **RootFolder**, která obsahuje složku s názvem **podsložky**. K synchronizaci každé úrovni složky, můžete použít následující řetězce:
   
   1. Pro synchronizaci runbooků **úložiště**, je cesta ke složce sady runbook*/*
   2. Pro synchronizaci runbooků **RootFolder**, je cesta ke složce runbook */RootFolder*
   3. Pro synchronizaci runbooků **podsložky**, je cesta ke složce runbook */RootFolder/podsložky*.
4. Po dokončení konfigurace parametry, jsou zobrazeny na **okno nastavit řízení zdrojů.**  
   
    ![Konfigurace okna](media/automation-source-control-integration/automation_02_SourceControlConfigure.png)
5. Po kliknutí na tlačítko OK, integrace ovládacích prvků zdrojového je teď nakonfigurovaný na účtu Automation a je třeba aktualizovat vaše informace Githubu. Nyní můžete kliknutím na tuto část zobrazíte všechna vaše historie úlohy synchronizace zdroj ovládacího prvku.  
   
    ![Hodnoty úložiště](media/automation-source-control-integration/automation_03_RepoValues.png)
6. Po nastavení správy zdrojového kódu, budou vytvořeny následující prostředky služby Automation v účtu Automation:  
   Dva [proměnných assetů](automation-variables.md) se vytvářejí.  
   
   * Proměnná **Microsoft.Azure.Automation.SourceControl.Connection** obsahuje hodnoty připojovacího řetězce, jak je uvedeno níže.  
     
     | **Parametr** | **Hodnota** |
     |:--- |:--- |
     | Name (Název) |Microsoft.Azure.Automation.SourceControl.Connection |
     | Typ |Řetězec |
     | Hodnota |{"Větve":\<*název vaší firemní pobočky*>, "RunbookFolderPath":\<*cesta ke složce Runbook*>, "typ zprostředkovatele":\<*má hodnotu 1 pro GitHub*>, "Úložiště":\<*název ve vašem úložišti*>, "Username":\<*Githubu vaše uživatelské jméno*>} |

    * Proměnná **Microsoft.Azure.Automation.SourceControl.OAuthToken**, obsahuje zabezpečené zašifrovanou hodnotu vaší OAuthToken.  

    |**Parametr**            |**Hodnota** |
    |:---|:---|
    | Name (Název)  | Microsoft.Azure.Automation.SourceControl.OAuthToken |
    | Typ | Unknown(Encrypted) |
    | Hodnota | <*Šifrované OAuthToken*> |  

    ![Proměnné](media/automation-source-control-integration/automation_04_Variables.png)  

    * **Automatizace správy zdrojového kódu** se přidá jako autorizovaný aplikace ke svému účtu GitHub. Chcete-li zobrazit aplikaci: domovskou stránku Githubu, přejděte k vaší **profil** > **nastavení** > **aplikace**. Tato aplikace umožňuje Azure Automation k synchronizaci úložiště GitHub pro účet Automation.  

    ![Git aplikace](media/automation-source-control-integration/automation_05_GitApplication.png)


## <a name="using-source-control-in-automation"></a>Použití správy zdrojového kódu v automatizace
### <a name="check-in-a-runbook-from-azure-automation-to-source-control"></a>Vrácení se změnami sadu runbook ve službě Azure Automation do správy zdrojového kódu
Runbook se změnami umožňuje odešlete změny, které jste provedli v sadě runbook ve službě Azure Automation do úložiště řízení zdrojů. Tady jsou kroky pro vrácení se změnami sadu runbook:

1. Z vašeho účtu Automation [vytvořte nový textový runbook](automation-first-runbook-textual.md), nebo [upravit runbook služby existující, textovou](automation-edit-textual-runbook.md). Tato sada runbook může být v pracovním postupu Powershellu nebo sadu runbook skript prostředí PowerShell.  
2. Po úpravě runbooku, uložte ho a klikněte na tlačítko **vrácení se změnami** z **upravit** okno.  
   
    ![Tlačítko vrácení se změnami](media/automation-source-control-integration/automation_06_CheckinButton.png)

     > [!NOTE] 
     > Vrácení se změnami ve službě Azure Automation přepíše kód, který aktuálně existuje v zdrojového kódu. Instrukce ekvivalent příkazového řádku Gitu k vrácení se změnami je **přidat git + git potvrzení + git push**  

1. Když kliknete na tlačítko **vrácení se změnami**, budete vyzváni k potvrzení, kliknutím na tlačítko Ano pokračovat.  
   
    ![Zpráva vrácení se změnami](media/automation-source-control-integration/automation_07_CheckinMessage.png)
2. Vrácení se změnami spouští runbook řízení zdroj: **synchronizace MicrosoftAzureAutomationAccountToGitHubV1**. Tato sada runbook se připojuje k webu GitHub a nabízených oznámení změny ve službě Azure Automation se svým úložištěm. Chcete-li zobrazit historii úlohy vrácení se změnami, přejděte zpět na **integrace ovládacích prvků zdrojového** a kliknutím otevřete okno synchronizace úložiště. Toto okno obsahuje všechny vaše úlohy řízení zdrojů.  Vyberte úlohu chcete zobrazit a kliknutím zobrazíte podrobnosti.  
   
    ![Vrácení se změnami sadu Runbook](media/automation-source-control-integration/automation_08_CheckinRunbook.png)
   
   > [!NOTE]
   > Zdroj řízení sady runbook jsou speciální automatizace sady runbook, které nemůžete zobrazit nebo upravit. Když se nezobrazí v seznamu sad runbook, zobrazí se úloh synchronizace zobrazovat v seznamu úloh.
   > 
   > 
3. Název změněné runbooku je odeslán jako vstupní parametr runbooku vrácení se změnami. Můžete [zobrazení podrobností o úloze](automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) rozšířením sady runbook v **úložiště synchronizace** okno.  
   
    ![Vstup vrácení se změnami](media/automation-source-control-integration/automation_09_CheckinInput.png)
4. Po dokončení úlohy a prohlédněte si změny, aktualizujte úložiště GitHub.  Měla by existovat potvrzení změn v úložišti zprávou potvrzení: **aktualizované *název sady Runbook* ve službě Azure Automation.**  

### <a name="sync-runbooks-from-source-control-to-azure-automation"></a>Synchronizace sady runbook od správy zdrojového kódu pro Azure Automation.
Tlačítko Synchronizovat v okně synchronizace úložiště umožňuje načítat všechny sady runbook v cestě ke složce runbook ve vašem úložišti ke svému účtu Automation. Stejného úložiště můžete synchronizovat s více než jeden účet Automation. Tady jsou kroky pro synchronizaci sady runbook:

1. Účet Automation, kde můžete nastavit řízení zdrojů, otevřete **synchronizace integrace/úložiště zdroj ovládacího prvku okno** a klikněte na tlačítko **synchronizace** a zobrazí se výzva s potvrzení zpráva, klikněte na tlačítko **Ano** pokračujte.  
   
    ![Tlačítko Synchronizovat](media/automation-source-control-integration/automation_10_SyncButtonwithMessage.png)
2. Sada runbook spustí synchronizace: **synchronizace MicrosoftAzureAutomationAccountFromGitHubV1**. Tato sada runbook se připojuje k webu GitHub a vrátí změny z úložiště pro Azure Automation. Měli byste vidět nové úlohy na **úložiště synchronizace** okno pro tuto akci. Chcete-li zobrazit podrobnosti o úloze synchronizace, klikněte na tlačítko otevřete okno Podrobnosti úlohy.  
   
    ![Synchronizace sady Runbook](media/automation-source-control-integration/automation_11_SyncRunbook.png)

    > [!NOTE] 
    > K synchronizaci od správy zdrojového kódu přepíše verzi konceptu sady runbook, které aktuálně neexistuje v účtu Automation pro **všechny** sady runbook, které jsou právě ve zdrojové ovládacího prvku. Instrukce ekvivalent příkazového řádku Gitu pro synchronizaci je **vyžádání git**


## <a name="troubleshooting-source-control-problems"></a>Řešení potíží zdroj ovládacího prvku
Pokud nejsou žádné chyby s úlohou vrácení se změnami nebo synchronizace, stav úlohy by měla být pozastavena, a v okně úlohy můžete zobrazit další podrobnosti o této chybě.  **Všechny protokoly** část zobrazí všechna prostředí PowerShell datové proudy přidružené k této úlohy. To vám poskytne informace potřebné k vám pomůžou s opravit problémy s vrácení se změnami nebo synchronizace. Také zobrazí se posloupnost akcí, které došlo k chybě při synchronizaci nebo ve vrácení sady runbook.  

![Obrázek AllLogs](media/automation-source-control-integration/automation_13_AllLogs.png)

## <a name="disconnecting-source-control"></a>Odpojuje se správa zdrojového kódu
Chcete-li odpojit z vašeho účtu GitHub, otevřete okno úložiště synchronizace a klikněte na **odpojení**. Po odpojení zdrojového kódu sady runbook, které byly dříve synchronizované se stále na účtu Automation, ale okně úložiště synchronizace nebude povolen.  

  ![Tlačítko Odpojit](media/automation-source-control-integration/automation_12_Disconnect.png)

## <a name="next-steps"></a>Další kroky
Další informace o integrace ovládacích prvků zdrojového najdete v následujících zdrojích informací:  

* [Služby Azure Automation: Integrace ovládacích prvků zdrojového ve službě Azure Automation](https://azure.microsoft.com/blog/azure-automation-source-control-13/)  
* [Hlas pro Oblíbené ve zdrojovém systému ovládací prvek](https://www.surveymonkey.com/r/?sm=2dVjdcrCPFdT0dFFI8nUdQ%3d%3d)  
* [Služby Azure Automation: Runbook zdroj ovládacího prvku pomocí Visual Studio Team Services integraci](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/)  

