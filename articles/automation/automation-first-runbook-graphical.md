---
title: "první grafický runbook aaaMy ve službě Azure Automation | Microsoft Docs"
description: "Kurz vás provede hello vytvořením, otestováním a publikováním jednoduchého grafického runbooku."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "runbook, šablona sady runbook, automatizace sady runbook, runbook azure"
ms.assetid: dcb88f19-ed2b-4372-9724-6625cd287c8a
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/17/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 964cf8bae75ca597959bfc39b2b07c22bbc9acb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="my-first-graphical-runbook"></a>Můj první grafický runbook

> [!div class="op_single_selector"]
> * [Grafický](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [Pracovní postup PowerShellu](automation-first-runbook-textual.md)
> 
> 

Tento kurz vás provede procesem vytvoření hello [grafický runbook](automation-runbook-types.md#graphical-runbooks) ve službě Azure Automation.  Začneme s jednoduchým runbookem, který testuje a publikuje současně vám vysvětlíme, jak tootrack hello stav úlohy runbooku hello.  Potom jsme upravit hello runbook tooactually spravovat prostředky Azure, v takovém případě spuštění virtuálního počítače Azure.  Potom jsme dokončení kurzu hello tím, že hello runbook robustnější přidáním parametrů a podmíněných odkazů.

## <a name="prerequisites"></a>Požadavky
toocomplete tohoto kurzu budete potřebovat následující hello.

* Předplatné Azure.  Pokud nemáte účet, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo <a href="/pricing/free-account/" target="_blank">[si zaregistrovat bezplatný účet](https://azure.microsoft.com/free/).
* [Účet Azure Automation](automation-sec-configure-azure-runas-account.md) toohold hello sady runbook a ověření tooAzure prostředky.  Tento účet musí mít oprávnění toostart a zastavit hello virtuální počítač.
* Virtuální počítač Azure.  Počítač zastavíme a spustíme, proto to nesmí být produkční počítač.

## <a name="step-1---create-runbook"></a>Krok 1 – vytvoření runbooku
Začneme vytvořením jednoduchého runbooku, který zobrazí hello text *Hello, World*.

1. V hello portálu Azure otevřete účet Automation.  
   stránka účtu Automation Hello nabízí rychlý přehled o hello prostředky v rámci tohoto účtu.  Už byste tam měli mít nějaké assety.  Většina z nich jsou moduly hello, které jsou automaticky obsažené v novém účtu Automation.  Měli byste také mít hello asset přihlašovacích údajů, který je uvedený v hello [požadavky](#prerequisites).
2. Klikněte na tlačítko hello **Runbooky** dlaždice tooopen hello seznamu sad runbook.<br> ![Řízení runbooků](media/automation-first-runbook-graphical/runbooks-resources-tile.png)
3. Vytvořit nový runbook kliknutím na hello **přidat runbook** tlačítko a potom **vytvořit nový runbook**.
4. Pojmenujte hello hello runbook *MyFirstRunbook-Graphical*.
5. V tomto případě vytvoříme toocreate [grafický runbook](automation-graphical-authoring-intro.md) tak vyberte **grafický** pro **typ Runbooku**.<br> ![Nový runbook](media/automation-first-runbook-graphical/create-new-runbook.png)<br>
6. Klikněte na tlačítko **vytvořit** toocreate hello sady runbook a hello otevřete grafický editor.

## <a name="step-2---add-activities-toohello-runbook"></a>Krok 2 – Přidání aktivity toohello runbook
Hello ovládací prvek knihovna na levé straně hello hello editoru vám umožní tooselect aktivity tooadd tooyour runbook.  Vytvoříme tooadd **Write-Output** rutiny toooutput text ze sady runbook hello.

1. V hello prvku knihovna klikněte do textového pole hledání hello a zadejte **Write-Output**.  výsledky hledání Hello se zobrazí níže. <br> ![Microsoft.PowerShell.Utility](media/automation-first-runbook-graphical/search-powershell-cmdlet-writeoutput.png)
2. Posuňte se dolů toohello dolní části seznamu hello.  Můžete buď klikněte pravým tlačítkem na **Write-Output** a vyberte **přidat toocanvas** nebo klikněte na tlačítko hello elipsy další toohello rutiny a pak vyberte **přidat toocanvas**.
3. Klikněte na tlačítko hello **Write-Output** aktivity na plátno hello.  Otevře se okno ovládacího prvku konfigurace hello, což vám umožní tooconfigure hello aktivity.
4. Hello **popisek** výchozí název toohello hello rutiny, ale můžeme ho změnit toosomething více popisný. Změnit příliš*zápisu Hello, World toooutput*.
5. Klikněte na tlačítko **parametry** tooprovide hodnoty pro parametry rutiny hello.  
   Některé rutiny obsahují několik sad parametrů a potřebujete tooselect, které jste jeden toouse. V takovém případě **Write-Output** má jenom jednu sadu parametrů, takže není nutné tooselect jeden. <br> ![Vlastnosti Write-Output](media/automation-first-runbook-graphical/write-output-properties-b.png)
6. Vyberte hello **InputObject** parametr.  Toto je parametr hello, kde určíme hello text toosend toohello výstupního datového proudu.
7. V hello **zdroj dat** rozevíracího seznamu vyberte **Powershellový výraz**.  Hello **zdroj dat** rozevírací seznam obsahuje různé zdroje, že používáte toopopulate hodnotu parametru.  
   Můžete použít výstup z těchto zdrojů, například další aktivitu, asset Automation nebo powershellový výraz.  V takovém případě stačí chceme textu hello toooutput *Hello, World*. Můžeme použít powershellový výraz a zadat řetězec.
8. V hello **výraz** zadejte *"Hello, World"* a pak klikněte na **OK** dvakrát tooreturn toohello plátno.<br> ![Powershellový výraz](media/automation-first-runbook-graphical/expression-hello-world.png)
9. Uložte hello runbook kliknutím **Uložit**.<br> ![Uložení runbooku](media/automation-first-runbook-graphical/runbook-toolbar-save-revised20165.png)

## <a name="step-3---test-hello-runbook"></a>Krok 3: Test hello runbook
Před publikováním runbooku toomake hello je k dispozici v produkčním prostředí, chceme tootest ho toomake jisti, že funguje správně.  Když runbook testujete, spustíte jeho  verzi **Koncept** a interaktivně zobrazíte jeho výsledek.

1. Klikněte na tlačítko **testovací podokno** tooopen hello testovací okno.<br> ![Testovací podokno](media/automation-first-runbook-graphical/runbook-toolbar-test-revised20165.png)
2. Klikněte na tlačítko **spustit** toostart hello testu.  To by měl být hello povolit pouze možnost.
3. A [úlohy runbooku](automation-runbook-execution.md) je vytvořena a její stav se zobrazí v podokně hello.  
   Stav úlohy Hello spustí jako *zařazeno ve frontě* označující, že se čeká na pracovního procesu runbooku v toobecome cloudu hello k dispozici.  Poté přesune příliš*počáteční* když pracovní proces hello úlohy a potom *systémem* při hello runbook skutečně spustí.  
4. Po dokončení úlohy runbooku hello se zobrazí jeho výstup. V našem případě bychom měli vidět text *Hello World*.<br> ![Hello World](media/automation-first-runbook-graphical/runbook-test-results.png)
5. Zavřete hello testovací okno tooreturn toohello plátno.

## <a name="step-4---publish-and-start-hello-runbook"></a>Krok 4 – publikování a spuštění sady runbook hello
Hello runbooku, který jsme vytvořili je stále v režimu konceptu. Potřebujeme toopublish ho před jeho spuštěním v produkčním prostředí.  Když runbook publikujete, můžete přepsat hello stávající publikované verze hello koncept.  V našem případě nemáme publikovanou verzi ještě vzhledem k tomu, že právě vytvořený hello runbook.

1. Klikněte na tlačítko **publikovat** toopublish hello runbook a potom **Ano** po zobrazení výzvy.<br> ![Publikování](media/automation-first-runbook-graphical/runbook-toolbar-publish-revised20166.png)
2. Pokud se posunete levém tooview hello runbook v hello **Runbooky** okně se zobrazí **stav vytváření** z **publikováno**.
3. Přejděte zpět toohello správné tooview hello okno pro **MyFirstRunbook**.  
   Hello možnosti v horní části hello nám umožňují toostart hello runbooku, naplánování toostart někdy v budoucnu hello nebo vytvořit [webhooku](automation-webhooks.md) , může být spuštění prostřednictvím volání protokolu HTTP.
4. Právě chceme toostart hello runbook proto klikněte na **spustit** a potom **Ano** po zobrazení výzvy.<br> ![Spuštění runbooku](media/automation-first-runbook-graphical/runbook-controls-start-revised20165.png)
5. Okno úlohy, je otevřené hello úlohy runbooku, který jsme vytvořili.  Můžeme zavřít toto okno, ale v takovém případě jsme ho můžete nechat otevřený, abychom mohli sledovat průběh úlohy hello.
6. Stav úlohy Hello se zobrazuje v **Souhrn úlohy** a odpovídá hello stavy, které jsme viděli při testování runbooku hello.<br> ![Souhrn úlohy](media/automation-first-runbook-graphical/runbook-job-summary.png)
7. Jednou hello stav runbooku zobrazí *dokončeno*, klikněte na tlačítko **výstup**. Hello **výstup** otevře okno a My uvidíme náš *Hello, World* v podokně hello.<br> ![Souhrn úlohy](media/automation-first-runbook-graphical/runbook-job-output.png)  
8. Okno výstup hello zavřít.
9. Klikněte na tlačítko **všechny protokoly** tooopen hello datové proudy okno úlohy runbooku hello.  Bychom měli vidět jenom *Hello, World* v hello výstupní datový proud, ale můžou se zobrazit jiné datové proudy z úlohy runbooku, například podrobný nebo Chyba Pokud hello runbook zapisuje toothem.<br> ![Souhrn úlohy](media/automation-first-runbook-graphical/runbook-job-AllLogs.png)
10. Zavřete okno všechny protokoly hello a hello úlohy okno tooreturn toohello MyFirstRunbook okno.
11. Klikněte na tlačítko **úlohy** tooopen hello okno úlohy pro tuto sadu runbook.  Rutina Vypíše seznam všech úloh hello vytvořené z této sady runbook. Bychom měli vidět jenom jednu úlohu, protože jsme ji spustili jenom hello úlohy jednou.<br> ![Úlohy](media/automation-first-runbook-graphical/runbook-control-jobs.png)
12. Tuto úlohu můžete kliknout na tooopen hello stejné podokno úlohy, které jsme zobrazili při spuštění sady runbook hello.  To vám umožní toogo zpět v čas a zobrazení hello podrobnosti libovolné úlohy, který byl vytvořen pro konkrétní runbook.

## <a name="step-5---create-variable-assets"></a>Krok 5 – vytvoření proměnných assetů
Runbook jsme otestovali a publikovali, ale zatím nedělá nic užitečného. Chceme toohave ho spravovat prostředky Azure.  Předtím, než nakonfigurujeme hello runbook tooauthenticate, vytvoření proměnné toohold hello ID předplatného a po tooauthenticate hello aktivity v kroku 6 níže nastavíme něj odkazovat.  Včetně kontext předplatného toohello odkaz vám umožní tooeasily práce s několika předplatnými.  Než budete pokračovat, zkopírujte si ID předplatného z hello odběry možnost vypnout hello navigačním podokně.  

1. V okně účty Automation hello, klikněte na tlačítko hello **prostředky** dlaždice a hello **prostředky** otevře okno.
2. V okně prostředků hello, klikněte na tlačítko hello **proměnné** dlaždici.
3. V okně proměnné hello, klikněte na tlačítko **přidat proměnnou**.<br>![Proměnná služby Automation](media/automation-first-runbook-graphical/create-new-subscriptionid-variable.png)
4. V okně nové proměnné hello v hello **název** zadejte **AzureSubscriptionId** a v hello **hodnotu** pole zadejte ID předplatného.  Zachovat *řetězec* pro hello **typ** a výchozí hodnotou hello **šifrování**.  
5. Klikněte na tlačítko **vytvořit** toocreate hello proměnné.  

## <a name="step-6---add-authentication-toomanage-azure-resources"></a>Krok 6 – přidání ověřování toomanage prostředků Azure
Teď, když máme proměnnou toohold naše ID předplatného, jsme nakonfigurovat tooauthenticate naše sady runbook s hello spustit jako přihlašovací údaje, které označují tooin hello [požadavky](#prerequisites).  Provedeme to přidáním hello spustit v Azure jako připojení **Asset** a **Add-AzureRMAccount** rutiny toohello plátno.  

1. Hello otevřete grafický editor kliknutím **upravit** v okně MyFirstRunbook hello.<br> ![Úprava runbooku](media/automation-first-runbook-graphical/runbook-controls-edit-revised20165.png)
2. Společnost Microsoft nepotřebuje hello **zápisu Hello, World toooutput** už nepotřebujeme, takže pravým tlačítkem myši a vyberte **odstranit**.
3. V hello prvku knihovna rozbalte **připojení** a přidejte **AzureRunAsConnection** toohello plátno výběrem **přidat toocanvas**.
4. Na plátně hello vyberte **AzureRunAsConnection** a v podokně ovládacího prvku konfigurace hello zadejte **získat připojení spustit jako** v hello **popisek** textové pole.  Toto je hello připojení
5. Hello prvku knihovna zadejte **Add-AzureRmAccount** textového pole hledání text hello.
6. Přidat **Add-AzureRmAccount** toohello plátno.<br> ![Add-AzureRMAccount](media/automation-first-runbook-graphical/search-powershell-cmdlet-addazurermaccount.png)
7. Pozastavte ukazatel myši nad **získat připojení spustit jako** dokud se nezobrazí kruh na hello dolní části obrazce hello. Klikněte hello kruh a přetáhněte šipku hello příliš**Add-AzureRmAccount**.  je Hello šipka, kterou jste vytvořili *odkaz*.  Sada runbook Hello začíná **získat připojení spustit jako** a spusťte **Add-AzureRmAccount**.<br> ![Vytvoření propojení mezi aktivitami](media/automation-first-runbook-graphical/runbook-link-auth-activities.png)
8. Na plátně hello vyberte **Add-AzureRmAccount** a v hello konfigurace řízení podokně typu **tooAzure přihlášení** v hello **popisek** textové pole.
9. Klikněte na tlačítko **parametry** a zobrazí se okno Konfigurace parametru aktivity hello.
10. **Přidat-AzureRmAccount** obsahuje několik sad parametrů, takže potřebujeme tooselect jeden před zadáním hodnot parametru.  Klikněte na tlačítko **nastavený parametr** a pak vyberte hello **ServicePrincipalCertificatewithSubscriptionId** sadu parametrů.
11. Jakmile vyberete hello sadu parametrů, hello parametry se zobrazí v okně Konfigurace parametru aktivity hello.  Klikněte na **APPLICATIONID**.<br> ![Přidání parametrů účtu Azure RM](media/automation-first-runbook-graphical/add-azurermaccount-params.png)
12. V okně hodnota parametru hello vyberte **výstup aktivity** pro hello **zdroj dat** a vyberte **získat připojení spustit jako** hello seznamu v hello **pole cesta** zadejte **ApplicationId**a potom klikněte na **OK**.  Název hello hello vlastnosti pro cestu pole hello zadáváme vzhledem k tomu, že výstupem aktivity hello je objekt s více vlastnostmi.
13. Klikněte na tlačítko **CERTIFICATETHUMBPRINT**a v okně hodnota parametru hello vyberte **výstup aktivity** pro hello **zdroj dat**.  Vyberte **získat připojení spustit jako** hello seznamu v hello **cesta pole** zadejte **CertificateThumbprint**a potom klikněte na **OK**.
14. Klikněte na tlačítko **SERVICEPRINCIPAL**a v okně hodnota parametru hello vyberte **ConstantValue** pro hello **zdroj dat**, klikněte na možnost hello **True**a potom klikněte na **OK**.
15. Klikněte na tlačítko **TENANTID**a v okně hodnota parametru hello vyberte **výstup aktivity** pro hello **zdroj dat**.  Vyberte **získat připojení spustit jako** hello seznamu v hello **cesta pole** zadejte **TenantId**a potom klikněte na **OK** dvakrát.  
16. Hello prvku knihovna zadejte **Set-AzureRmContext** textového pole hledání text hello.
17. Přidat **Set-AzureRmContext** toohello plátno.
18. Na plátně hello vyberte **Set-AzureRmContext** a v hello konfigurace řízení podokně typu **zadat Id předplatného** v hello **popisek** textové pole.
19. Klikněte na tlačítko **parametry** a zobrazí se okno Konfigurace parametru aktivity hello.
20. **Set-AzureRmContext** obsahuje několik sad parametrů, takže potřebujeme tooselect jeden před zadáním hodnot parametru.  Klikněte na tlačítko **nastavený parametr** a pak vyberte hello **SubscriptionId** sadu parametrů.  
21. Jakmile vyberete hello sadu parametrů, hello parametry se zobrazí v okně Konfigurace parametru aktivity hello.  Klikněte na **ID předplatného**.
22. V okně hodnota parametru hello vyberte **variabilní prostředek** pro hello **zdroj dat** a vyberte **AzureSubscriptionId** z hello seznamu a pak klikněte na tlačítko **OK**  dvakrát.   
23. Pozastavte ukazatel myši nad **tooAzure přihlášení** dokud se nezobrazí kruh na hello dolní části obrazce hello. Klikněte hello kruh a přetáhněte šipku hello příliš**zadat Id předplatného**.

Runbook by měl vypadat jako hello v tomto okamžiku následující: <br>![Konfigurace ověření runbooku](media/automation-first-runbook-graphical/runbook-auth-config.png)

## <a name="step-7---add-activity-toostart-a-virtual-machine"></a>Krok 7 – Přidání aktivity toostart virtuálního počítače
Zde přidáme **Start-AzureRmVM** toostart aktivity virtuálního počítače.  Můžete si vybrat jakýkoli virtuální počítač ve vašem předplatném Azure a pro teď můžete používat pevné kódování, který název do rutiny hello.

1. Hello prvku knihovna zadejte **Start-AzureRm** textového pole hledání text hello.
2. Přidat **Start-AzureRmVM** toohello, plátno a potom klikněte na tlačítko a přetáhněte ji pod **zadat Id předplatného**.
3. Pozastavte ukazatel myši nad **zadat Id předplatného** dokud se nezobrazí kruh na hello dolní části obrazce hello.  Klikněte hello kruh a přetáhněte šipku hello příliš**Start-AzureRmVM**.
4. Vyberte **Start-AzureRmVM**.  Klikněte na tlačítko **parametry** a potom **nastavený parametr** tooview hello nastaví pro **Start-AzureRmVM**.  Vyberte hello **ResourceGroupNameParameterSetName** sadu parametrů. Všimněte si, že vedle **ResourceGroupName** a **Název** se zobrazuje vykřičník.  To znamená, že tyto parametry jsou povinné.  Všimněte si také, že oba očekávají řetězcové hodnoty.
5. Vyberte **Název**.  Vyberte **Powershellový výraz** pro hello **zdroj dat** a zadejte název hello hello virtuálního počítače v uvozovkách, které jsme začínají na tuto sadu runbook.  Klikněte na **OK**.<br>![Hodnota parametru názvu Start-AzureRmVM](media/automation-first-runbook-graphical/runbook-startvm-nameparameter.png)
6. Vyberte **ResourceGroupName**. Použití **Powershellový výraz** pro hello **zdroj dat** a zadejte název hello hello skupiny prostředků v uvozovkách.  Klikněte na **OK**.<br> ![Parametry Start-AzureRmVM](media/automation-first-runbook-graphical/startazurermvm-params.png)
7. Klikněte na testovací podokno, abychom můžete otestovat sadu runbook hello.
8. Klikněte na tlačítko **spustit** toostart hello testu.  Po jeho dokončení zkontrolujte, zda text hello virtuálního počítače byla spuštěna.

Runbook by měl vypadat jako hello v tomto okamžiku následující: <br>![Konfigurace ověření runbooku](media/automation-first-runbook-graphical/runbook-startvm.png)

## <a name="step-8---add-additional-input-parameters-toohello-runbook"></a>Krok 8 – přidání dalších vstupních parametrů toohello runbook
Náš runbook aktuálně spouští hello virtuálního počítače ve skupině prostředků hello, kterou jsme uvedli v hello **Start-AzureRmVM** rutiny.  Náš runbook by být užitečnější, pokud bychom mohli obě zadat při spuštění runbooku hello.  Teď přidáme vstupní parametry toohello runbook tooprovide této funkce.

1. Hello otevřete grafický editor kliknutím **upravit** na hello **MyFirstRunbook** podokně.
2. Klikněte na tlačítko **vstup a výstup** a potom **přidat vstup** podokno vstupní parametr Runbooku tooopen hello.<br> ![Vstup a výstup z runbooku](media/automation-first-runbook-graphical/runbook-toolbar-InputandOutput-revised20165.png)
3. Zadejte *VMName* pro hello **název**.  Zachovat *řetězec* pro hello **typ**, ale změnit **povinné** příliš*Ano*.  Klikněte na **OK**.
4. Vytvořte druhý povinný vstupní parametr s názvem *ResourceGroupName* a pak klikněte na **OK** tooclose hello **vstup a výstup** podokně.<br> ![Vstupní parametry runbooku](media/automation-first-runbook-graphical/start-azurermvm-params-outputs.png)
5. Vyberte hello **Start-AzureRmVM** aktivity a pak klikněte na tlačítko **parametry**.
6. Změna hello **zdroj dat** pro **název** příliš**vstup z Runbooku** a pak vyberte **VMName**.<br>
7. Změna hello **zdroj dat** pro **ResourceGroupName** příliš**vstup z Runbooku** a pak vyberte **ResourceGroupName**.<br> ![Parametry Start-AzureVM](media/automation-first-runbook-graphical/start-azurermvm-params-runbookinput.png)
8. Uložte hello runbook a otevřete testovací podokno hello.  Všimněte si, že teď můžete zadat hodnoty pro hello dvou vstupních proměnných, které používáte v testu hello.
9. Zavřít hello testovací podokno.
10. Klikněte na tlačítko **publikovat** toopublish hello nové verze sady runbook hello.
11. Zastavte hello virtuální počítač, který jste spustili v předchozím kroku hello.
12. Klikněte na tlačítko **spustit** toostart hello runbook.  Typ v hello **VMName** a **ResourceGroupName** budete toostart hello virtuálního počítače.<br> ![Spuštění runbooku](media/automation-first-runbook-graphical/runbook-start-inputparams.png)
13. Po dokončení hello runbook, zkontrolujte, zda text hello virtuálního počítače byla spuštěna.

## <a name="step-9---create-a-conditional-link"></a>Krok 9 – vytvoření podmíněného propojení
Jsme hello runbook nyní změnit tak, že pokud již není spuštěn pouze pokusí toostart hello virtuálního počítače.  To uděláte tak, že přidáte **Get-AzureRmVM** rutiny toohello runbooku, který získá stav úrovně instance hello hello virtuálního počítače. Pak přidejte modul kód pracovního postupu Powershellu s názvem **získání stavu** pomocí prostředí PowerShell fragmentu kódu toodetermine Pokud hello stav virtuálního počítače je spuštěná nebo zastavená.  Podmíněné propojení z hello **získání stavu** modulu spouští pouze **Start-AzureRmVM** Pokud hello je aktuálně spuštěný stav je zastaveno.  Nakonec jsme výstup zprávy tooinform vám, zda hello virtuálního počítače byla úspěšně spuštěna nebo není pomocí hello rutiny prostředí PowerShell Write-Output.

1. Otevřete **MyFirstRunbook** v hello grafický editor.
2. Odebrat odkaz hello mezi **zadat Id předplatného** a **Start-AzureRmVM** na ni kliknete, a potom stisknutím klávesy hello *odstranit* klíč.
3. Hello prvku knihovna zadejte **Get-AzureRm** textového pole hledání text hello.
4. Přidat **Get-AzureRmVM** toohello plátno.
5. Vyberte **Get-AzureRmVM** a potom **nastavený parametr** tooview hello nastaví pro **Get-AzureRmVM**.  Vyberte hello **GetVirtualMachineInResourceGroupNameParamSet** sadu parametrů.  Všimněte si, že vedle **ResourceGroupName** a **Název** se zobrazuje vykřičník.  To znamená, že tyto parametry jsou povinné.  Všimněte si také, že oba očekávají řetězcové hodnoty.
6. V části **Zdroj dat** u možnosti **Název** vyberte **Vstup z runbooku** a potom vyberte **VMName**.  Klikněte na tlačítko **OK**.
7. V části **Zdroj dat** u možnosti **ResourceGroupName** vyberte **Vstup z runbooku** a potom vyberte **ResourceGroupName**.  Klikněte na **OK**.
8. V části **Zdroj dat** u možnosti **Stav** vyberte **Konstantní hodnota** a potom klikněte na **Pravda**.  Klikněte na **OK**.  
9. Vytvořit vazbu mezi **zadat Id předplatného** příliš**Get-AzureRmVM**.
10. V ovládacím prvku knihovna hello, rozbalte položku **řízení sady Runbook** a přidejte **kód** toohello plátno.  
11. Vytvořit vazbu mezi **Get-AzureRmVM** příliš**kód**.  
12. Klikněte na tlačítko **kód** a v podokně hello konfigurace změňte popisek příliš**získání stavu**.
13. Vyberte **kód** parametr a hello **Editor kódu** otevře se okno.  
14. V editoru kódu hello vložte následující fragment kódu hello:
    
     ```
     $StatusesJson = $ActivityOutput['Get-AzureRmVM'].StatusesText
     $Statuses = ConvertFrom-Json $StatusesJson
     $StatusOut =""
     foreach ($Status in $Statuses){
     if($Status.Code -eq "Powerstate/running"){$StatusOut = "running"}
     elseif ($Status.Code -eq "Powerstate/deallocated") {$StatusOut = "stopped"}
     }
     $StatusOut
     ```
15. Vytvořit vazbu mezi **získání stavu** příliš**Start-AzureRmVM**.<br> ![Runbook s modulem kódu](media/automation-first-runbook-graphical/runbook-startvm-get-status.png)  
16. Vyberte odkaz hello a v podokně Konfigurace hello změňte **použít podmínku** příliš**Ano**.   Poznámka: hello propojení se změní tooa přerušovanou čáru indikující, že hello cílové aktivity spouští pouze pokud hello podmínka přeložená tootrue.  
17. Pro hello **výrazu podmínky**, typ *$ActivityOutput ['Get Status'] - eq "Stopped"*.  **Start-AzureRmVM** nyní lze spustit pouze pokud je zastavena hello virtuálního počítače.
18. V hello prvku knihovna rozbalte **rutiny** a potom **Microsoft.PowerShell.Utility**.
19. Přidat **Write-Output** toohello plátno dvakrát.<br> ![Runbook s aktivitou Write-Output](media/automation-first-runbook-graphical/runbook-startazurermvm-complete.png)
20. Na hello první **Write-Output** řídit, klikněte na tlačítko **parametry** a změňte hello **popisek** hodnota příliš*oznámit spuštění virtuálního počítače*.
21. Pro **InputObject**, změňte **zdroj dat** příliš**Powershellový výraz** a zadejte výraz hello *"$VMName successfully started."* .
22. Na hello druhý **Write-Output** řídit, klikněte na tlačítko **parametry** a změňte hello **popisek** hodnota příliš*oznámit neúspěšné spuštění virtuálního počítače*
23. Pro **InputObject**, změňte **zdroj dat** příliš**Powershellový výraz** a zadejte výraz hello *"$VMName Could not start."* .
24. Vytvořit vazbu mezi **Start-AzureRmVM** příliš**oznámit spuštění virtuálního počítače** a **oznámit neúspěšné spuštění virtuálního počítače**.
25. Vyberte odkaz hello příliš**oznámit spuštění virtuálního počítače** a změňte **použít podmínku** příliš**True**.
26. Pro hello **výrazu podmínky**, typ *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - eq $true*.  Write-Output řízení nyní pouze spustí, pokud hello virtuální počítač se úspěšně spustila.
27. Vyberte odkaz hello příliš**oznámit neúspěšné spuštění virtuálního počítače** a změňte **použít podmínku** příliš**True**.
28. Pro hello **výrazu podmínky**, typ *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - ne $true*.  Write-Output řízení teď spustí jenom, pokud hello virtuální počítač není úspěšně spuštěna.
29. Uložte hello runbook a otevřete testovací podokno hello.
30. Spusťte hello runbook s virtuálním počítačem hello zastavena a by se měl spustit.

## <a name="next-steps"></a>Další kroky
* toolearn Další informace o vytváření grafického obsahu, najdete v části [vytváření grafického obsahu ve službě Azure Automation](automation-graphical-authoring-intro.md)
* tooget kroky s runbooky prostředí PowerShell najdete v části [Můj první Powershellový runbook](automation-first-runbook-textual-powershell.md)
* tooget kroky s runbooky pracovních postupů Powershellu, najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md)

