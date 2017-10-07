---
title: "aaaEditing textovou runbooky ve službě Azure Automation"
description: "Tento článek obsahuje různé postupy pro práci se sadami runbook Powershellu a pracovní postup prostředí PowerShell ve službě Azure Automation pomocí hello textový editor."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: 6f5b48fb-6f30-4e99-9e14-9061b5554b08
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 3fd87d457838f300ca6c94bc345e82c679a0e011
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="editing-textual-runbooks-in-azure-automation"></a>Úprava textové sady runbook ve službě Azure Automation
Hello textový editor ve službě Azure Automation lze použít tooedit [Powershellové runbooky](automation-runbook-types.md#powershell-runbooks) a [runbooky pracovních postupů Powershellu](automation-runbook-types.md#powershell-workflow-runbooks). Tato akce nemá hello typické funkce další kód editory například intellisense a barevné kódování s další speciální funkce tooassist můžete při přístupu ke společné toorunbooks prostředky.  Tento článek poskytuje podrobné pokyny pro provádění různé funkce pomocí tohoto editoru.

textový editor Hello zahrnuje funkce tooinsert kódu pro aktivity, prostředky a podřízené runbooky do runbooku. Nemusíte psát kód hello sami, můžete vybrat ze seznamu dostupných zdrojů a mají hello vložit správný kód do runbooku hello.

Každá sada runbook ve službě Azure Automation má dvě verze: koncept a publikovaný. Upravte hello verzi konceptu sady runbook hello a pak ho publikujete, aby se Dal spustit. Hello publikovaná verze nelze upravit. V tématu [publikování runbooku](automation-creating-importing-runbook.md#publishing-a-runbook) Další informace.

toowork s [grafické Runbooky](automation-runbook-types.md#graphical-runbooks), najdete v části [vytváření grafického obsahu ve službě Azure Automation](automation-graphical-authoring-intro.md).

## <a name="tooedit-a-runbook-with-hello-azure-portal"></a>tooedit sady runbook pomocí portálu Azure hello
Použijte následující postup tooopen sady runbook pro úpravy v textový editor hello hello.

1. V hello portálu Azure vyberte svůj účet automation.
2. Klikněte na tlačítko hello **Runbooky** dlaždice tooopen hello seznamu sad runbook.
3. Klikněte na název hello hello sady runbook tooedit a pak klikněte na hello **upravit** tlačítko.
4. Proveďte potřebné úpravy hello.
5. Klikněte na tlačítko **Uložit** při dokončení provedené úpravy se.
6. Klikněte na tlačítko **publikovat** Pokud chcete, aby hello nejnovější verzi konceptu hello toobe sady runbook publikovat.

### <a name="tooinsert-a-cmdlet-into-a-runbook"></a>tooinsert rutiny do runbooku
1. V hello plátno hello textový editor umístěte kurzor hello místo, kde chcete tooplace hello rutiny.
2. Rozbalte hello **rutiny** uzlu v hello prvku knihovna.
3. Rozbalte hello modul, který obsahuje rutinu hello chcete toouse.
4. Klikněte pravým tlačítkem na hello rutiny tooinsert a vyberte **přidat toocanvas**.  Pokud rutina hello má více než jeden parametr nastavení, bude přidán hello výchozí sadu.  Můžete také rozšířit hello rutiny tooselect jiným parametrem nastavit.
5. Hello kód pro rutinu hello vložena s jeho celý seznam parametrů.
6. Zadejte odpovídající hodnotu namísto hello datového typu v závorkách <> pro požadované parametry.  Odeberte všechny parametry, které nepotřebujete.

### <a name="tooinsert-code-for-a-child-runbook-into-a-runbook"></a>tooinsert kód pro podřízený runbook do runbooku
1. V hello plátno hello textový editor, umístěte kurzor hello místo tooplace hello kód pro hello [podřízeného runbooku](automation-child-runbooks.md).
2. Rozbalte hello **Runbooky** uzel v hello prvku knihovna.
3. Klikněte pravým tlačítkem na tooinsert hello sady runbook a vyberte **přidat toocanvas**.
4. Hello kód pro podřízený runbook hello vložena s zástupné symboly pro všechny parametry runbooku.
5. Nahraďte zástupné symboly hello s příslušnými hodnotami pro jednotlivé parametry.

### <a name="tooinsert-an-asset-into-a-runbook"></a>tooinsert prostředek do runbooku
1. V hello plátno hello textový editor umístěte kurzor hello, kde chcete tooplace hello kód pro hello podřízené sady runbook.
2. Rozbalte hello **prostředky** uzlu v hello prvku knihovna.
3. Rozbalte uzel hello pro typ prostředku, který má být hello.
4. Klikněte pravým tlačítkem na hello asset tooinsert a vyberte **přidat toocanvas**.  Pro [proměnných assetů](automation-variables.md), vyberte buď **přidat "Získat proměnnou" toocanvas** nebo **přidat "Nastavit proměnnou" toocanvas** v závislosti na tom, zda mají být tooget nebo nastavit proměnnou hello.
5. Hello kód pro prostředek hello je vložen do sady runbook hello.

## <a name="tooedit-a-runbook-with-hello-azure-portal"></a>tooedit sady runbook pomocí portálu Azure hello
Použijte následující postup tooopen sady runbook pro úpravy v textový editor hello hello.

1. V hello portálu Azure, vyberte **automatizace** a pak klikněte hello název účtu automation.
2. Vyberte hello **Runbooky** kartě.
3. Klikněte na název hello hello sady runbook mají tooedit a potom vyberte hello **Autor** kartě.
4. Klikněte na tlačítko hello **upravit** tlačítko dole hello úvodní obrazovka.
5. Proveďte potřebné úpravy hello.
6. Klikněte na tlačítko **Uložit** při dokončení provedené úpravy se.
7. Klikněte na tlačítko **publikovat** Pokud chcete, aby hello nejnovější verzi konceptu hello toobe sady runbook publikovat.

### <a name="tooinsert-an-activity-into-a-runbook"></a>tooinsert aktivity do sady Runbook
1. V hello plátno hello textový editor umístěte kurzor hello místo, kde chcete tooplace hello aktivity.
2. Hello dolní části úvodní obrazovka, klikněte na **vložit** a potom **aktivity**.
3. V hello **modulu integrace** sloupce, vyberte hello modul, který obsahuje aktivitu hello.
4. V hello **aktivity** podokně, vyberte aktivitu.
5. V hello **popis** sloupce, popis hello Poznámka hello aktivity. Volitelně můžete kliknout na podrobné zobrazení nápovědy. toolaunch aktivity hello v prohlížeči hello.
6. Klikněte na šipku vpravo hello.  Pokud má aktivita hello parametry, budou uvedené pro vaši informaci.
7. Klikněte na tlačítko Zkontrolovat hello.  Kód toorun hello aktivity se vloží do runbooku hello.
8. Pokud hello aktivita vyžaduje parametry, zadejte odpovídající hodnotu namísto datového typu hello v <> složené závorky.

### <a name="tooinsert-code-for-a-child-runbook-into-a-runbook"></a>tooinsert kód pro podřízený runbook do runbooku
1. V hello plátno hello textový editor, umístěte kurzor hello místo tooplace hello [podřízeného runbooku](automation-child-runbooks.md).
2. Hello dolní části úvodní obrazovka, klikněte na **vložit** a potom **Runbook**.
3. Vyberte hello runbook tooinsert hello prostředním sloupci a klikněte na šipku vpravo hello.
4. Pokud hello runbook obsahuje parametry, budou uvedené pro vaši informaci.
5. Klikněte na tlačítko Zkontrolovat hello.  Kód toorun hello vybrané sady runbook bude vložen do aktuální sady runbook hello.
6. Pokud hello runbook vyžaduje parametry, zadejte odpovídající hodnotu namísto datového typu hello v <> složené závorky.

### <a name="tooinsert-an-asset-into-a-runbook"></a>tooinsert prostředek do runbooku
1. V hello plátno hello textový editor umístěte kurzor hello místo, kam chcete tooplace hello aktivity tooretrieve hello asset.
2. Hello dolní části úvodní obrazovka, klikněte na **vložit** a potom **nastavení**.
3. V hello **akce nastavení** sloupce, vyberte hello akce, který chcete.
4. Vyberte z dostupných prostředků hello v prostředním sloupci hello.
5. Klikněte na tlačítko Zkontrolovat hello.  Kód tooget nebo sadu hello asset se vloží do runbooku hello.

## <a name="tooedit-an-azure-automation-runbook-using-windows-powershell"></a>tooedit runbook služby Azure Automation pomocí prostředí Windows PowerShell
tooedit sady runbook pomocí prostředí Windows PowerShell, je použít hello editoru podle své volby a uložit jej tooa souboru .ps1. Můžete použít hello [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) rutiny tooretrieve hello obsah hello runbook a potom [Set-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) rutiny tooreplace hello existující Koncept sady runbook s hello upravit jeden.

### <a name="tooretrieve-hello-contents-of-a-runbook-using-windows-powershell"></a>tooRetrieve hello obsah sady Runbook pomocí prostředí Windows PowerShell
Hello následující vzorové příkazy ukazují, jak tooretrieve hello skriptu pro sadu runbook a uložit ho tooa souboru skriptu. V tomto příkladu se načte koncept hello. Je také možné tooretrieve hello publikovaná verze sady runbook hello i když tato verze nedá změnit.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="toochange-hello-contents-of-a-runbook-using-windows-powershell"></a>tooChange hello obsah sady Runbook pomocí prostředí Windows PowerShell
Hello následující vzorové příkazy ukazují, jak tooreplace hello stávajícího obsahu sady runbook s hello obsahem souboru skriptu. Všimněte si, že to je hello stejný postup jako v ukázkové [tooimport runbooku ze souboru skriptu v prostředí Windows PowerShell](automation-creating-importing-runbook.md).

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>Související články
* [Vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md)
* [Učení pracovního postupu Powershellu](automation-powershell-workflow.md)
* [Grafické vytváření obsahu v Azure Automation.](automation-graphical-authoring-intro.md)
* [Certifikáty](automation-certificates.md)
* [Připojení](automation-connections.md)
* [Přihlašovací údaje](automation-credentials.md)
* [Plány](automation-schedules.md)
* [Proměnné](automation-variables.md)
