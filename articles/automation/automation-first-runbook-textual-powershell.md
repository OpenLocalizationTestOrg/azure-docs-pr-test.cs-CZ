---
title: "aaaMy první Powershellový runbook ve službě Azure Automation | Microsoft Docs"
description: "Kurz vás provede hello vytvořením, otestováním a publikováním jednoduchého Powershellového runbooku."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "azure powershell, kurz k powershellovému scriptu, automatizace powershellu"
ms.assetid: a43b395a-e740-41a3-ae62-40eac9d0ec00
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/26/2017
ms.author: magoedte;sngun
ms.openlocfilehash: abff94abe666cd8423c35b970b4162ba9247bcf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="my-first-powershell-runbook"></a>Můj první powershellový runbook

> [!div class="op_single_selector"]
> * [Grafický](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [Pracovní postup PowerShellu](automation-first-runbook-textual.md)
> 
> 

Tento kurz vás provede procesem vytvoření hello [Powershellový runbook](automation-runbook-types.md#powershell-runbooks) ve službě Azure Automation. Začneme s jednoduchým runbookem, který jsme otestujeme a publikujeme a současně vám vysvětlíme, jak tootrack hello stav úlohy runbooku hello. Potom jsme upravit hello runbook tooactually spravovat prostředky Azure, v takovém případě spuštění virtuálního počítače Azure. A konečně provedeme hello runbook robustnější přidáním parametrů.

## <a name="prerequisites"></a>Požadavky
toocomplete tohoto kurzu budete potřebovat hello následující:

* Předplatné Azure. Pokud nemáte účet, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo <a href="/pricing/free-account/" target="_blank">[si zaregistrovat bezplatný účet](https://azure.microsoft.com/free/).
* [Účet Automation](automation-sec-configure-azure-runas-account.md) toohold hello sady runbook a ověření tooAzure prostředky.  Tento účet musí mít oprávnění toostart a zastavit hello virtuální počítač.
* Virtuální počítač Azure. Počítač zastavíme a spustíme, proto to nesmí být produkční virtuální počítač.

## <a name="step-1---create-new-runbook"></a>Krok 1 – vytvoření nového runbooku
Začneme vytvořením jednoduchého runbooku, který zobrazí hello text *Hello, World*.

1. V hello portálu Azure otevřete účet Automation.  
   stránka účtu Automation Hello nabízí rychlý přehled o hello prostředky v rámci tohoto účtu. Už byste tam měli mít nějaké prostředky. Většina z nich jsou moduly hello, které jsou automaticky obsažené v novém účtu Automation. Měli byste také mít hello asset přihlašovacích údajů, který je uvedený v hello [požadavky](#prerequisites).
2. Klikněte na tlačítko hello **Runbooky** dlaždice tooopen hello seznamu sad runbook.<br><br> ![RunbooksControl](media/automation-first-runbook-textual-powershell/runbooks-control-tiles.png)  
3. Vytvořit novou sadu runbook kliknutím na tlačítko hello **přidat runbook** tlačítko a potom **vytvořit nový runbook**.
4. Pojmenujte hello hello runbook *MyFirstRunbook-PowerShell*.
5. V tomto případě vytvoříme toocreate [Powershellový runbook](automation-runbook-types.md#powershell-runbooks) tak vyberte **prostředí Powershell** pro **typ Runbooku**.<br><br> ![Typ runbooku](media/automation-first-runbook-textual-powershell/automation-runbook-type.png)  
6. Klikněte na tlačítko **vytvořit** toocreate hello sady runbook a otevřete hello textový editor.

## <a name="step-2---add-code-toohello-runbook"></a>Krok 2 – přidání kódu toohello runbook
Můžete buď typ kódu přímo do runbooku hello, nebo můžete vybrat rutiny, runbooky a prostředky z hello prvku knihovna a je přidali toohello sada runbook se všemi souvisejícími parametry. V tomto návodu budeme zadejte přímo v hello runbook.

1. Náš runbook je aktuálně prázdný, zadejte text *Write-Output "Hello World"*.<br><br> ![Hello World](media/automation-first-runbook-textual-powershell/automation-helloworld.png)  
2. Uložte hello runbook kliknutím **Uložit**.<br><br> ![Tlačítko Uložit](media/automation-first-runbook-textual-powershell/automation-runbook-edit-controls-save.png)  

## <a name="step-3---test-hello-runbook"></a>Krok 3: Test hello runbook
Před publikováním runbooku toomake hello je k dispozici v produkčním prostředí, chceme tootest ho toomake jisti, že funguje správně. Když runbook testujete, spustíte jeho  verzi **Koncept** a interaktivně zobrazíte jeho výsledek.

1. Klikněte na tlačítko **testovací podokno** tooopen hello testovací podokno.<br><br> ![Testovací podokno](media/automation-first-runbook-textual-powershell/automation-runbook-edit-controls-test.png)  
2. Klikněte na tlačítko **spustit** toostart hello testu. To by měl být hello povolit pouze možnost.
3. Vytvoří se [úloha runbooku](automation-runbook-execution.md) a její stav se zobrazí.  
   Stav úlohy Hello spustí jako *zařazeno ve frontě* označující, že se čeká na pracovního procesu runbooku v toocome cloudu hello k dispozici. Pak ji přesune příliš*počáteční* když pracovní proces hello úlohy a potom *systémem* při hello runbook skutečně spustí.  
4. Po dokončení úlohy runbooku hello se zobrazí jeho výstup. V našem případě bychom měli vidět text *Hello World*.<br><br> ![Výstup testovacího podokna](media/automation-first-runbook-textual-powershell/automation-testpane-output.png)  
5. Zavřete hello testovací podokno tooreturn toohello plátno.

## <a name="step-4---publish-and-start-hello-runbook"></a>Krok 4 – publikování a spuštění sady runbook hello
Hello runbooku, který jsme vytvořili je stále v režimu konceptu. Potřebujeme toopublish ho před jeho spuštěním v produkčním prostředí.  Když runbook publikujete, můžete přepsat hello stávající publikované verze hello koncept.  V našem případě nemáme publikovanou verzi ještě vzhledem k tomu, že právě vytvořený hello runbook.

1. Klikněte na tlačítko **publikovat** toopublish hello runbook a potom **Ano** po zobrazení výzvy.<br><br> ![Tlačítko Publikovat](media/automation-first-runbook-textual-powershell/automation-runbook-edit-controls-publish.png)  
2. Pokud se posunete levém tooview hello runbook v hello **sady Runbook** podokně se teď zobrazí **stav vytváření** z **publikováno**.
3. Přejděte zpět toohello správné tooview hello podokno **MyFirstRunbook-PowerShell**.  
   Hello možnosti v horní části hello nám umožňují toostart hello runbook, zobrazení hello runbooku, naplánování toostart někdy v budoucnu hello nebo vytvořit [webhooku](automation-webhooks.md) , může být spuštění prostřednictvím volání protokolu HTTP.
4. Jsme má toostart hello runbook, proto klikněte na **spustit** a pak klikněte na **Ok** po otevření okna spuštění Runbooku hello.<br><br> ![Tlačítko Spustit](media/automation-first-runbook-textual-powershell/automation-runbook-controls-start.png)<br>    
5. Podokno úlohy je otevřené hello úlohy runbooku, který jsme vytvořili. Toto podokno můžeme zavřít, ale v takovém případě jsme ho můžete nechat otevřený, abychom mohli sledovat průběh úlohy hello.
6. Stav úlohy Hello se zobrazuje v **Souhrn úlohy** a odpovídá hello stavy, které jsme viděli při testování runbooku hello.<br><br> ![Souhrn úlohy](media/automation-first-runbook-textual-powershell/job-pane-status-blade-jobsummary.png)<br>  
7. Jednou hello stav runbooku zobrazí *dokončeno*, klikněte na tlačítko **výstup**. Otevře se podokno výstup Hello a My uvidíme náš *Hello, World*.<br><br> ![Výstup úlohy](media/automation-first-runbook-textual-powershell/job-pane-status-blade-outputtile.png)<br> 
8. V podokně výstupu zavřít hello.
9. Klikněte na tlačítko **všechny protokoly** podokno datové proudy hello tooopen hello úloze runbooku. Bychom měli vidět jenom *Hello, World* v hello výstupní datový proud, ale můžou se zobrazit jiné datové proudy z úlohy runbooku, například podrobný nebo Chyba Pokud hello runbook zapisuje toothem.<br><br> ![Všechny protokoly](media/automation-first-runbook-textual-powershell/job-pane-status-blade-alllogstile.png)<br>   
10. Zavřete podokno datové proudy hello a hello úlohy tooreturn toohello MyFirstRunbook-PowerShell podokno.
11. Klikněte na tlačítko **úlohy** tooopen hello podokno úlohy pro tuto sadu runbook. Seznam všech hello úloh, které tento runbook vytvořil. Bychom měli vidět jenom jednu úlohu, protože jsme ji spustili jenom hello úlohy jednou.<br><br> ![Seznam úloh](media/automation-first-runbook-textual-powershell/runbook-control-job-tile.png)  
12. Tuto úlohu můžete kliknout na tooopen hello stejné podokno úlohy, které jsme zobrazili při spuštění sady runbook hello. To vám umožní toogo zpět v čas a zobrazení hello podrobnosti libovolné úlohy, který byl vytvořen pro konkrétní runbook.

## <a name="step-5---add-authentication-toomanage-azure-resources"></a>Krok 5 – Přidání ověřování toomanage prostředků Azure
Runbook jsme otestovali a publikovali, ale zatím nedělá nic užitečného. Chceme toohave ho spravovat prostředky Azure. Nebude moct toodo, že v případě, pokud máme ověřit pomocí hello přihlašovacích údajů, které jsou podle tooin hello [požadavky](#prerequisites). Provedeme to pomocí hello **Add-AzureRmAccount** rutiny.

1. Hello otevřete textový editor kliknutím **upravit** v podokně MyFirstRunbook-PowerShell hello.<br><br> ![Úprava runbooku](media/automation-first-runbook-textual-powershell/automation-runbook-controls-edit.png)<br>   
2. Společnost Microsoft nepotřebuje hello **Write-Output** řádek už, proto pokračujte a odstraňte ji.
3. Zadejte nebo zkopírujte a vložte následující kód, který zpracovává hello ověřování pomocí účtu spustit v Automation jako hello:
   
   ```
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   ```
   <br>
4. Klikněte na tlačítko **testovací podokno** tak, aby abychom mohli otestovat sadu runbook hello.
5. Klikněte na tlačítko **spustit** toostart hello testu. Po jeho dokončení byste měli obdržet výstup podobný toohello následující, zobrazení základní informace z vašeho účtu. Tím se potvrzuje, že hello přihlašovací údaje jsou platné.<br><br> ![Ověření](media/automation-first-runbook-textual-powershell/runbook-auth-output.png)

## <a name="step-6---add-code-toostart-a-virtual-machine"></a>Krok 6 – přidání kódu toostart virtuálního počítače
Teď, když runbook umí ověřit tooour předplatné Azure, můžeme začít spravovat prostředky. Přidáme příkaz toostart virtuálního počítače. Můžete vybrat jakýkoli virtuální počítač ve vašem předplatném Azure a teď provedeme závislé tento název v hello runbook.

1. Po *Add-AzureRmAccount*, typ *Start-AzureRmVM-Name 'VMName' - ResourceGroupName 'NameofResourceGroup'* poskytnutí hello název a název skupiny prostředků toostart hello virtuálního počítače.  
   
   ```
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
   ```
   <br>
2. Uložte hello runbook a pak klikněte na tlačítko **testovací podokno** tak, aby abychom mohli otestovat.
3. Klikněte na tlačítko **spustit** toostart hello testu. Po jeho dokončení zkontrolujte, zda text hello virtuálního počítače byla spuštěna.

## <a name="step-7---add-an-input-parameter-toohello-runbook"></a>Krok 7 – přidání runbook toohello služby vstupní parametr
Náš runbook aktuálně spouští hello virtuální počítač, který jsme pevně kódovaný v hello runbook, ale by být užitečnější, pokud bychom při spuštění runbooku hello zadat hello virtuálního počítače. Teď přidáme vstupní parametry toohello runbook tooprovide této funkce.

1. Přidání parametrů pro *VMName* a *ResourceGroupName* toohello runbook a použijte tyto proměnné s hello **Start-AzureRmVM** rutiny jako v následujícím příkladu hello.  
   
   ```
   Param(
    [string]$VMName,
    [string]$ResourceGroupName
   )
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
   ```
   <br>
2. Uložte hello runbook a otevřete testovací podokno hello. Můžete teď poskytujete hodnoty pro hello dvou vstupních proměnných, které se používají v testu hello.
3. Zavřít hello testovací podokno.
4. Klikněte na tlačítko **publikovat** toopublish hello nové verze sady runbook hello.
5. Zastavte hello virtuální počítač, který jste spustili v předchozím kroku hello.
6. Klikněte na tlačítko **spustit** toostart hello runbook. Typ v hello **VMName** a **ResourceGroupName** budete toostart hello virtuálního počítače.<br><br> ![Předání parametru](media/automation-first-runbook-textual-powershell/automation-pass-params.png)<br>  
7. Po dokončení hello runbook, zkontrolujte, zda text hello virtuálního počítače byla spuštěna.

## <a name="differences-from-powershell-workflow"></a>Odlišnosti od runbooků pracovních postupů PowerShellu
Powershellové runbooky mají hello stejný životní cyklus, možnosti a správu jako runbooky pracovních postupů Powershellu, ale existují určité rozdíly a omezení:

1. Powershellové runbooky běží rychlé porovnání tooPowerShell runbooky pracovních postupů, protože neobsahují kompilační krok.
2. Runbooky pracovních postupů Powershellu podporují kontrolní body. pomocí kontrolních bodů, runbooky pracovních postupů Powershellu může pokračovat z libovolného bodu v runbooku hello, zatímco Powershellové runbooky můžou pokračovat jenom od začátku hello.
3. Runbooky pracovních postupů PowerShellu podporují paralelní a sériové provádění, zatímco powershellové runbooky můžou příkazy provádět jenom sériově.
4. V runboocích pracovních postupů PowerShellu může mít aktivita, příkaz nebo blok skriptu svoje vlastní prostředí runspace, zatímco v powershellovém runbooku běží všechno ve skriptu v jednom prostředí runspace. Existují také určité [syntaktické rozdíly](https://technet.microsoft.com/magazine/dn151046.aspx) mezi nativním powershellovým runbookem a runbookem pracovního postupu PowerShellu.

## <a name="next-steps"></a>Další kroky
* tooget kroky s grafickými runbooky najdete v části [Můj první grafický runbook](automation-first-runbook-graphical.md)
* tooget kroky s runbooky pracovních postupů Powershellu, najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md)
* tooknow Další informace o typech runbooků, jejich výhody a omezení, najdete v části [typy runbooků ve službě Azure Automation](automation-runbook-types.md)
* Další informace o funkci podpory powershellových skriptů najdete v článku [Nativní podpora powershellových skriptů ve službě Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)

