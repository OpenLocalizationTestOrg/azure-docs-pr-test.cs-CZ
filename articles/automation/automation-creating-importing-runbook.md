---
title: "aaaCreating nebo import runbooku ve službě Azure Automation"
description: "Tento článek popisuje, jak toocreate novou sadu runbook v Azure Automation nebo importovat jeden ze souboru."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 24414362-b690-4474-8ca7-df18e30fc31d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte;bwren
ms.openlocfilehash: d45f44cf15fbcacdd0de2977668502c2e1671063
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Vytvoření nebo import runbooku ve službě Azure Automation
Můžete přidat tooAzure runbook automatizace, a to buď [vytvořením nového](#creating-a-new-runbook) nebo importováním existujícího runbooku ze souboru nebo z hello [Galerie Runbooků](automation-runbook-gallery.md). Tento článek obsahuje informace o vytvoření a import sad runbook ze souboru.  Můžete získat všechny podrobnosti hello o přístup k komunity runbooky a moduly v [Galerie Runbooků a modulů pro Azure Automation](automation-runbook-gallery.md).

## <a name="creating-a-new-runbook"></a>Vytvoření nové sady runbook
Můžete vytvořit novou sadu runbook ve službě Azure Automation pomocí jedné z hello Azure portály nebo prostředí Windows PowerShell. Po vytvoření sady runbook hello, můžete upravit pomocí informací v [pracovního postupu Powershellu Learning](automation-powershell-workflow.md) a [vytváření grafického obsahu ve službě Azure Automation](automation-graphical-authoring-intro.md).

### <a name="toocreate-a-new-azure-automation-runbook-with-hello-azure-classic-portal"></a>toocreate nové sady Azure Automation runbook pomocí portálu Azure Classic hello
Je možné pracovat pouze s [runbooky pracovních postupů Powershellu](automation-runbook-types.md#powershell-workflow-runbooks) v hello portálu Azure.

1. Na portálu Azure Classic hello klikněte na tlačítko, **nový**, **App Services**, **automatizace**, **Runbook**, **rychle vytvořit**.
2. Zadejte hello požadované informace a pak klikněte na tlačítko **vytvořit**. Název sady runbook Hello musí začínat písmenem a může obsahovat písmena, číslice, podtržítka a pomlčky.
3. Pokud chcete tooedit hello runbook nyní, klepněte na tlačítko **upravit Runbook**. Jinak, klikněte na tlačítko **OK**.
4. Nový runbook se objeví na hello **Runbooky** kartě.

### <a name="toocreate-a-new-azure-automation-runbook-with-hello-azure-portal"></a>toocreate nový runbook automatizace Azure s hello portálu Azure
1. V hello portálu Azure otevřete účet Automation.
2. Hello rozbočovače, vyberte **Runbooky** tooopen hello seznam runbooků.
3. Klikněte na hello **přidat runbook** tlačítko a potom **vytvořit nový runbook**.
4. Zadejte **název** hello sady runbook a vyberte jeho [typu](automation-runbook-types.md). Název sady runbook Hello musí začínat písmenem a může obsahovat písmena, číslice, podtržítka a pomlčky.
5. Klikněte na tlačítko **vytvořit** toocreate hello sady runbook a otevřete hello editor.

### <a name="toocreate-a-new-azure-automation-runbook-with-windows-powershell"></a>toocreate nové sady Azure Automation runbook pomocí prostředí Windows PowerShell
Můžete použít hello [New-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) toocreate rutiny prázdnou [runbook pracovního postupu Powershellu](automation-runbook-types.md#powershell-workflow-runbooks). Můžete buď zadat hello **název** toocreate parametr prázdnou sadu runbook, můžete později upravit, nebo můžete zadat hello **cesta** parametr tooimport soubor sady runbook. Hello **typ** parametr by měl být také součástí toospecify jeden z typů hello čtyři sady runbook.

Hello následující ukázkové příkazy Zobrazit jak toocreate novou prázdnou sadu runbook.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Import runbooku ze souboru do Azure Automation.
Můžete vytvořit novou sadu runbook ve službě Azure Automation importováním skript prostředí PowerShell nebo pracovního postupu Powershellu (soubory s příponou .ps1) nebo exportovaný grafický runbook (.graphrunbook).  Je nutné zadat hello [typ runbooku](automation-runbook-types.md) , bude vytvořena z hello import s ohledem na účet hello následující aspekty.

* Soubor .graphrunbook může importovat jenom do nového [grafický runbook](automation-runbook-types.md#graphical-runbooks), a grafické runbooky, které mohou být vytvořeny pouze ze souboru .graphrunbook.
* Souboru s příponou .ps1 s pracovním postupem prostředí PowerShell lze importovat pouze do [runbook pracovního postupu Powershellu](automation-runbook-types.md#powershell-workflow-runbooks).  Pokud hello soubor obsahuje více pracovních postupů prostředí PowerShell, hello import selže. Musíte uložit každý vlastní soubor pracovního postupu tooits a každý importovat samostatně.
* Souboru s příponou .ps1, který neobsahuje pracovního postupu lze importovat do buď [Powershellový runbook](automation-runbook-types.md#powershell-runbooks) nebo [runbook pracovního postupu Powershellu](automation-runbook-types.md#powershell-workflow-runbooks).  Pokud je importovat do sady runbook PowerShell Workflow a pak ji bude převedený tooa pracovního postupu a komentáře budou zahrnuty do sady runbook hello zadání hello změny, které byly provedeny.

### <a name="tooimport-a-runbook-from-a-file-with-hello-azure-classic-portal"></a>tooimport runbooku ze souboru pomocí portálu Azure Classic hello
Můžete použít následující postup tooimport soubor skriptu do Azure Automation hello.  Všimněte si, že můžete importovat pouze souboru s příponou .ps1 do sady runbook PowerShell Workflow pomocí tento portál.  Hello portál Azure je nutné použít pro jiné typy.

1. V portálu pro správu Azure hello, vyberte **automatizace** a potom vyberte účet Automation.
2. Klikněte na **Importovat**.
3. Klikněte na tlačítko **vyhledat soubor** a vyhledejte tooimport soubor skriptu hello.
4. Pokud chcete tooedit hello runbook nyní, klepněte na tlačítko **upravit Runbook**. V opačném případě klikněte na tlačítko OK.
5. Hello nový runbook se objeví na hello **Runbooky** karta hello účet Automation.
6. Je nutné [publikovat hello runbook](#publishing-a-runbook) předtím, než můžete ji spustit.

### <a name="tooimport-a-runbook-from-a-file-with-hello-azure-portal"></a>tooimport runbooku ze souboru s hello portálu Azure
Můžete použít následující postup tooimport soubor skriptu do Azure Automation hello.  

> [!NOTE]
> Všimněte si, že můžete importovat pouze souboru s příponou .ps1 do pracovního postupu Powershellu runbooku pomocí portálu hello.
> 
> 

1. V hello portálu Azure otevřete účet Automation.
2. Hello rozbočovače, vyberte **Runbooky** tooopen hello seznam runbooků.
3. Klikněte na hello **přidat runbook** tlačítko a potom **Import**.
4. Klikněte na tlačítko **soubor sady Runbook** tooselect hello souboru tooimport
5. Pokud hello **název** pole je přístupné, pak máte možnost toochange hello ho.  Název sady runbook Hello musí začínat písmenem a může obsahovat písmena, číslice, podtržítka a pomlčky.
6. Hello [typ runbooku](automation-runbook-types.md) bude automaticky vybrána, ale můžete změnit typ hello vezmete hello platí omezení v úvahu. 
7. Hello nový runbook se objeví v seznamu hello sad runbook pro hello účet Automation.
8. Je nutné [publikovat hello runbook](#publishing-a-runbook) předtím, než můžete ji spustit.

> [!NOTE]
> Po importu grafický runbook nebo grafický runbook pracovního postupu prostředí PowerShell, máte hello možnost tooconvert toohello jiný typ Pokud chtěli. Nelze převést tootextual.
> 
> 

### <a name="tooimport-a-runbook-from-a-script-file-with-windows-powershell"></a>tooimport runbooku ze souboru skriptu v prostředí Windows PowerShell
Můžete použít hello [Import AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) rutiny tooimport soubor skriptu jako koncept runbook pracovního postupu Powershellu. Pokud již existuje sada runbook hello, hello import se nezdaří, pokud nechcete použít hello *-Force* parametr. 

Hello následující vzorové příkazy ukazují, jak tooimport skript soubor do sady runbook.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Publikování runbooku
Při vytváření nebo importovat nové sady runbook, musíte publikovat předtím, než můžete ji spustit.  Každá sada runbook ve službě Automation má koncept a publikovanou verzi. Pouze publikovaná verze hello je k dispozici toobe spustit a lze upravovat pouze hello verzi konceptu. Hello publikovaná verze není ovlivněna verzi konceptu toohello žádné změny. Když koncept hello by měla být k dispozici, pak ji publikujete hello publikovaná verze přepíše s hello verzi konceptu.

## <a name="toopublish-a-runbook-using-hello-azure-classic-portal"></a>toopublish a runbook pomocí portálu Azure Classic hello
1. Otevřete hello runbook na portálu Azure Classic hello.
2. Hello horní části úvodní obrazovka, klikněte na tlačítko **Autor**.
3. Hello dolní části úvodní obrazovka, klikněte na **publikovat** a potom **Ano** toohello ověřovací zprávu.

## <a name="toopublish-a-runbook-using-hello-azure-portal"></a>toopublish a runbook pomocí portálu Azure hello
1. Otevřete hello runbook v hello portálu Azure.
2. Klikněte na tlačítko hello **upravit** tlačítko.
3. Klikněte na tlačítko hello **publikovat** tlačítko a potom **Ano** toohello ověřovací zprávu.

## <a name="toopublish-a-runbook-using-windows-powershell"></a>toopublish sady runbook pomocí prostředí Windows PowerShell
Můžete použít hello [publikovat AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) rutiny toopublish sady runbook pomocí prostředí Windows PowerShell. Hello následující ukázkové příkazy Zobrazit jak toopublish ukázkové sady runbook.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Další kroky
* toolearn o tom, jak můžete využívat výhod hello sady Runbook a modul Galerie prostředí PowerShell, najdete v části [Galerie Runbooků a modulů pro Azure Automation.](automation-runbook-gallery.md)
* toolearn Další informace o úpravy sady runbook Powershellu a pracovní postup prostředí PowerShell s textový editor, najdete v části [úpravy textovou sady runbook ve službě Azure Automation](automation-edit-textual-runbook.md)
* toolearn Další informace o vytváření grafického runbooku, najdete v části [vytváření grafického obsahu ve službě Azure Automation](automation-graphical-authoring-intro.md)

