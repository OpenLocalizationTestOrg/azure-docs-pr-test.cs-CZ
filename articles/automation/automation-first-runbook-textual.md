---
title: "aaaMy první runbook pracovního postupu Powershellu ve službě Azure Automation | Microsoft Docs"
description: "Kurz vás provede procesem vytvoření Dobrý den, otestováním a publikováním jednoduchého textového runbooku pomocí prostředí PowerShell. pracovní postup."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "pracovní postup v powershellu, příklady pracovního postupu v powershellu, pracovní postup powershell"
ms.assetid: 0002d7f7-e2b5-46e3-b5eb-4596b84fd526
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/26/2017
ms.author: magoedte;bwren
ms.openlocfilehash: b5a34d363ef4865878f3f68119833367b5280f83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="my-first-powershell-workflow-runbook"></a>Můj první runbook pracovního postupu PowerShellu

> [!div class="op_single_selector"]
> * [Grafický](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [Pracovní postup PowerShellu](automation-first-runbook-textual.md)
> 
> 

Tento kurz vás provede procesem vytvoření hello [runbook pracovního postupu Powershellu](automation-runbook-types.md#powershell-workflow-runbooks) ve službě Azure Automation. Začneme s jednoduchým runbookem, který jsme testování a publikování při vysvětlením, jak tootrack hello stav úlohy runbooku hello. Potom jsme upravit hello runbook tooactually spravovat prostředky Azure, v takovém případě spuštění virtuálního počítače Azure. Nakonec uděláme hello runbook robustnější přidáním parametrů.

## <a name="prerequisites"></a>Požadavky
toocomplete tohoto kurzu budete potřebovat hello následující:

* Předplatné Azure. Pokud nemáte účet, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo <a href="/pricing/free-account/" target="_blank">[si zaregistrovat bezplatný účet](https://azure.microsoft.com/free/).
* [Účet Automation](automation-sec-configure-azure-runas-account.md) toohold hello sady runbook a ověření tooAzure prostředky.  Tento účet musí mít oprávnění toostart a zastavit hello virtuální počítač.
* Virtuální počítač Azure. Počítač zastavíme a spustíme, proto to nesmí být produkční virtuální počítač.

## <a name="step-1---create-new-runbook"></a>Krok 1 – vytvoření nového runbooku
Začneme vytvořením jednoduchého runbooku, který zobrazí hello text *Hello, World*.

1. V hello portálu Azure otevřete účet Automation.  
   stránka účtu Automation Hello nabízí rychlý přehled o hello prostředky v rámci tohoto účtu. Už byste tam měli mít nějaké prostředky. Většina z nich jsou moduly hello, které jsou automaticky obsažené v novém účtu Automation. Měli byste také mít hello asset přihlašovacích údajů, který je uvedený v hello [požadavky](#prerequisites).
2. Klikněte na hello **Runbooky** dlaždice tooopen hello seznamu sad runbook.<br><br> ![Řízení runbooků](media/automation-first-runbook-textual/runbooks-control-tiles.png)
3. Vytvořit nový runbook kliknutím na hello **přidat runbook** tlačítko a potom **vytvořit nový runbook**.
4. Pojmenujte hello hello runbook *MyFirstRunbook-Workflow*.
5. V tomto případě vytvoříme toocreate [runbook pracovního postupu Powershellu](automation-runbook-types.md#powershell-workflow-runbooks) tak vyberte **pracovního postupu Powershellu** pro **typ Runbooku**.<br><br> ![Nový runbook](media/automation-first-runbook-textual/new-runbook-properties.png)
6. Klikněte na tlačítko **vytvořit** toocreate hello sady runbook a otevřete hello textový editor.

## <a name="step-2---add-code-toohello-runbook"></a>Krok 2 – přidání kódu toohello runbook
Můžete buď typ kódu přímo do runbooku hello, nebo můžete vybrat rutiny, runbooky a prostředky z hello prvku knihovna a je přidali toohello sada runbook se všemi souvisejícími parametry. V tomto návodu budeme zapisovat přímo do runbooku hello.

1. Náš runbook je teď prázdný pouze hello požadované *pracovního postupu* – klíčové slovo, název hello naše sady runbook a hello složené závorky, které uzavřou celý pracovní postup hello.

   ```
   Workflow MyFirstRunbook-Workflow
   {
   }
   ```
2. Napište text *Write-Output "Hello World".* mezi složené závorky hello.

   ```
   Workflow MyFirstRunbook-Workflow
   {
   Write-Output "Hello World"
   }
   ```
3. Uložte hello runbook kliknutím **Uložit**.<br><br> ![Uložení runbooku](media/automation-first-runbook-textual/automation-runbook-edit-controls-save.png)

## <a name="step-3---test-hello-runbook"></a>Krok 3: Test hello runbook
Před publikováním runbooku toomake hello je k dispozici v produkčním prostředí, chceme tootest ho toomake jisti, že funguje správně. Když runbook testujete, spustíte jeho  verzi **Koncept** a interaktivně zobrazíte jeho výsledek.

1. Klikněte na tlačítko **testovací podokno** tooopen hello testovací podokno.<br><br> ![Testovací podokno](media/automation-first-runbook-textual/automation-runbook-edit-controls-test.png)
2. Klikněte na tlačítko **spustit** toostart hello testu. To by měl být hello povolit pouze možnost.
3. Vytvoří se [úloha runbooku](automation-runbook-execution.md) a její stav se zobrazí.  
   Počáteční stav úlohy Hello bude *zařazeno ve frontě* označující, že se čeká na pracovního procesu runbooku v toocome cloudu hello k dispozici. Pak ji přesune příliš*počáteční* když pracovní proces hello úlohy a potom *systémem* při hello runbook skutečně spustí.  
4. Po dokončení úlohy runbooku hello se zobrazí jeho výstup. V našem případě bychom měli vidět text *Hello World*.<br><br> ![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)
5. Zavřete hello testovací podokno tooreturn toohello plátno.

## <a name="step-4---publish-and-start-hello-runbook"></a>Krok 4 – publikování a spuštění sady runbook hello
Hello runbook, který jsme právě vytvořili, je stále v režimu konceptu. Potřebujeme toopublish ho před jeho spuštěním v produkčním prostředí. Když runbook publikujete, můžete přepsat hello stávající publikované verze hello koncept. V našem případě nemáme publikovanou verzi ještě vzhledem k tomu, že právě vytvořený hello runbook.

1. Klikněte na tlačítko **publikovat** toopublish hello runbook a potom **Ano** po zobrazení výzvy.<br><br> ![Publikování](media/automation-first-runbook-textual/automation-runbook-edit-controls-publish.png)
2. Pokud se posunete levém tooview hello runbook v hello **sady Runbook** podokně se teď zobrazí **stav vytváření** z **publikováno**.
3. Přejděte zpět toohello správné tooview hello podokno **MyFirstRunbook-Workflow**.  
   Hello možnosti v horní části hello nám umožňují toostart hello runbooku, naplánování toostart někdy v budoucnu hello nebo vytvořit [webhooku](automation-webhooks.md) , může být spuštění prostřednictvím volání protokolu HTTP.
4. Právě chceme toostart hello runbook proto klikněte na **spustit** a potom **Ano** po zobrazení výzvy.<br><br> ![Spuštění runbooku](media/automation-first-runbook-textual/automation-runbook-controls-start.png)
5. Podokno úlohy je otevřené hello úlohy runbooku, který jsme právě vytvořili. Toto podokno můžeme zavřít, ale v takovém případě ho necháme otevřené, abychom mohli sledovat průběh úlohy hello.
6. Stav úlohy Hello se zobrazuje v **Souhrn úlohy** a odpovídá hello stavy, které jsme viděli při testování runbooku hello.<br><br> ![Souhrn úlohy](media/automation-first-runbook-textual/job-pane-status-blade-jobsummary.png)
7. Jednou hello stav runbooku zobrazí *dokončeno*, klikněte na tlačítko **výstup**. Otevře se podokno výstup Hello a My uvidíme náš *Hello, World*.<br><br> ![Souhrn úlohy](media/automation-first-runbook-textual/job-pane-status-blade-outputtile.png)  
8. V podokně výstupu zavřít hello.
9. Klikněte na tlačítko **všechny protokoly** podokno datové proudy hello tooopen hello úloze runbooku. Bychom měli vidět jenom *Hello, World* v hello výstupní datový proud, ale můžou se zobrazit jiné datové proudy z úlohy runbooku, například podrobný nebo Chyba Pokud hello runbook zapisuje toothem.<br><br> ![Souhrn úlohy](media/automation-first-runbook-textual/job-pane-status-blade-alllogstile.png)
10. Zavřete podokno datové proudy hello a hello úlohy tooreturn toohello MyFirstRunbook podokno.
11. Klikněte na tlačítko **úlohy** tooopen hello podokno úlohy pro tuto sadu runbook. Seznam všech hello úloh, které tento runbook vytvořil. Bychom měli vidět jenom jednu úlohu, protože jsme ji spustili jenom hello úlohy jednou.<br><br> ![Úlohy](media/automation-first-runbook-textual/runbook-control-job-tile.png)
12. Kliknutím na tuto úlohu tooopen hello stejné podokno úlohy, které jsme zobrazili při spuštění sady runbook hello. To vám umožní toogo zpět v čas a zobrazení hello podrobnosti libovolné úlohy, který byl vytvořen pro konkrétní runbook.

## <a name="step-5---add-authentication-toomanage-azure-resources"></a>Krok 5 – Přidání ověřování toomanage prostředků Azure
Runbook jsme otestovali a publikovali, ale zatím nedělá nic užitečného. Chceme toohave ho spravovat prostředky Azure. Nebude moct toodo, že v případě, pokud máme ověřit pomocí hello přihlašovacích údajů, které jsou podle tooin hello [požadavky](#prerequisites). Provedeme to pomocí hello **Add-AzureRMAccount** rutiny.

1. Hello otevřete textový editor kliknutím **upravit** v podokně MyFirstRunbook-Workflow hello.<br><br> ![Úprava runbooku](media/automation-first-runbook-textual/automation-runbook-controls-edit.png)
2. Společnost Microsoft nepotřebuje hello **Write-Output** řádek už, proto pokračujte a odstraňte ji.
3. Pozice hello kurzor na prázdný řádek mezi složené závorky hello.
4. Zadejte nebo zkopírujte a vložte následující kód, který bude zpracovávat ověřování hello pomocí účtu spustit v Automation jako hello:

   ```
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   ```
5. Klikněte na tlačítko **testovací podokno** tak, aby abychom mohli otestovat sadu runbook hello.
6. Klikněte na tlačítko **spustit** toostart hello testu. Po jeho dokončení byste měli obdržet výstup podobný toohello následující, zobrazení základní informace z vašeho účtu. Tím se potvrzuje, že hello přihlašovací údaje jsou platné.<br><br> ![Ověření](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-toostart-a-virtual-machine"></a>Krok 6 – přidání kódu toostart virtuálního počítače
Teď, když runbook umí ověřit tooour předplatné Azure, můžeme začít spravovat prostředky. Přidáme příkaz toostart virtuálního počítače. Můžete vybrat jakýkoli virtuální počítač ve vašem předplatném Azure a prozatím jsme bude hardcoding, název v hello runbook.

1. Po *Add-AzureRmAccount*, typ *Start-AzureRmVM-Name 'VMName' - ResourceGroupName 'NameofResourceGroup'* poskytnutí hello název a název skupiny prostředků toostart hello virtuálního počítače.  

   ```
   workflow MyFirstRunbook-Workflow
   {
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
   }
   ```
2. Uložte hello runbook a pak klikněte na tlačítko **testovací podokno** tak, aby abychom mohli otestovat.
3. Klikněte na tlačítko **spustit** toostart hello testu. Po jeho dokončení zkontrolujte, zda text hello virtuálního počítače byla spuštěna.

## <a name="step-7---add-an-input-parameter-toohello-runbook"></a>Krok 7 – přidání runbook toohello služby vstupní parametr
Náš runbook aktuálně spouští hello virtuální počítač, který jsme pevně kódovaný v hello runbook, ale by být užitečnější, pokud bychom mohli zadat hello virtuální počítač při spuštění runbooku hello. Teď přidáme vstupní parametry toohello runbook tooprovide této funkce.

1. Přidání parametrů pro *VMName* a *ResourceGroupName* toohello runbook a použijte tyto proměnné s hello **Start-AzureRmVM** rutiny jako v následujícím příkladu hello.

   ```
   workflow MyFirstRunbook-Workflow
   {
    Param(
     [string]$VMName,
     [string]$ResourceGroupName
    )  
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
   }
   ```
2. Uložte hello runbook a otevřete testovací podokno hello. Všimněte si, že teď můžete zadat hodnoty pro hello dvou vstupních proměnných, které se použijí v testu hello.
3. Zavřít hello testovací podokno.
4. Klikněte na tlačítko **publikovat** toopublish hello nové verze sady runbook hello.
5. Zastavte hello virtuální počítač, který jste spustili v předchozím kroku hello.
6. Klikněte na tlačítko **spustit** toostart hello runbook. Typ v hello **VMName** a **ResourceGroupName** budete toostart hello virtuálního počítače.<br><br> ![Spuštění runbooku](media/automation-first-runbook-textual/automation-pass-params.png)<br>  
7. Po dokončení hello runbook, zkontrolujte, zda text hello virtuálního počítače byla spuštěna.  

## <a name="next-steps"></a>Další kroky
* tooget kroky s grafickými runbooky najdete v části [Můj první grafický runbook](automation-first-runbook-graphical.md)
* tooget kroky s runbooky prostředí PowerShell najdete v části [Můj první Powershellový runbook](automation-first-runbook-textual-powershell.md)
* toolearn Další informace o typech runbooků, jejich výhody a omezení, najdete v části [typy runbooků ve službě Azure Automation](automation-runbook-types.md)
* Další informace o funkci podpory powershellových skriptů najdete v článku [Nativní podpora powershellových skriptů ve službě Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)
