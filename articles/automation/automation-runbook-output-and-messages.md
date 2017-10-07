---
title: "aaaRunbook výstupu a zpráv ve službě Azure Automation | Microsoft Docs"
description: "Popisuje, jak toocreate a načtení výstupní zařízení a chybové zprávy ze sady runbook ve službě Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 13a414f5-0e2c-4be2-9558-a3e3ec84b6b2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/11/2016
ms.author: magoedte;bwren
ms.openlocfilehash: c1505fa889473766bfa47e43aaed2449d60ad851
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-output-and-messages-in-azure-automation"></a>Výstup a zprávy ve službě Azure Automation Runbooku
Většina runbooků služeb automatizace Azure budou mít určitou formu výstupu, jako uživatel s chybová zpráva toohello nebo komplexní objekt určené toobe spotřebovávají jiného pracovního postupu. Prostředí Windows PowerShell poskytuje [víc datových proudů](http://blogs.technet.com/heyscriptingguy/archive/2014/03/30/understanding-streams-redirection-and-write-host-in-powershell.aspx) toosend výstup ze skriptu nebo pracovního postupu. Služby Azure Automation funguje s každou z těchto datových proudů jinak a postupujte podle nejlepší postupy jejich toouse každý při vytváření sady runbook.

Hello následující tabulka obsahuje stručný popis jednotlivých datových proudů hello a jejich chování v hello portálu pro správu Azure při spuštění publikovaného runbooku a při [testování runbooku](automation-testing-runbook.md). Další podrobnosti o jednotlivých datových proudech jsou uvedeny v následujících částech.

| Datový proud | Popis | Publikováno | Test |
|:--- |:--- |:--- |:--- |
| Výstup |Objekty určené toobe zpracovávat jiné runbooky. |Zapíše toohello historie úlohy. |Zobrazí v hello podokna výstup testu. |
| Upozornění |Upozornění určené pro uživatele hello. |Zapíše toohello historie úlohy. |Zobrazí v hello podokna výstup testu. |
| Chyba |Chybová zpráva určená pro uživatele hello. Na rozdíl od výjimky hello runbook pokračovat po chybovou zprávu ve výchozím nastavení. |Zapíše toohello historie úlohy. |Zobrazí v hello podokna výstup testu. |
| Verbose |Zprávy, které poskytují informace o obecných nebo ladění. |Zapíše toojob historie pouze pokud je zapnutá podrobné protokolování pro sadu runbook hello. |V podokně výstup testu hello zobrazí jenom v případě $VerbosePreference nastavenou tooContinue v hello runbook. |
| Průběh |Záznamy automaticky generované před a po každé aktivitě v runbooku hello. Hello runbook neměli toocreate vlastní záznamy průběhu, protože ty jsou určené pro interaktivního uživatele. |Zapíše toojob historie pouze pokud probíhá protokolování je zapnuté pro sadu runbook hello. |Nezobrazuje se v hello podokna výstup testu. |
| Ladění |Zprávy určené pro interaktivního uživatele. Nesmí se používat v runboocích. |Nezapíše toojob historie. |Nezapíše tooTest podokno výstup. |

## <a name="output-stream"></a>Výstupní datový proud
Hello výstupní datový proud je určený pro výstup objektů vytvořených skript nebo pracovního postupu při správném spuštění. Ve službě Azure Automation, tento datový proud používá primárně u objektů určených toobe spotřebovávají [nadřazené sady runbook, které volání aktuální sady runbook hello](automation-child-runbooks.md). Pokud jste [voláte přiřazený runbook](automation-child-runbooks.md#invoking-a-child-runbook-using-inline-execution) z nadřízeného runbooku, vrátí data z hello výstupní datový proud toohello nadřazené. Hello výstupní datový proud toocommunicate obecné informace back toohello uživatelů byste měli používat jenom, pokud víte, že hello runbook nebude nikdy volat žádný jiný runbook. Jako osvědčený postup, ale obvykle používejte hello [podrobný datový proud](#Verbose) toocommunicate obecné informace toohello uživatele.

Můžete napsat data toohello výstupní datový proud pomocí [Write-Output](http://technet.microsoft.com/library/hh849921.aspx) nebo umístěním hello objektu na vlastním řádku v sadě runbook hello.

    #hello following lines both write an object toohello output stream.
    Write-Output –InputObject $object
    $object

### <a name="output-from-a-function"></a>Výstup z funkce
Když píšete toohello výstupního datového proudu ve funkci, která je zahrnutá ve vašem runbooku, výstup hello se předá zpět toohello runbook. Pokud hello runbook přidá tento výstup tooa proměnné, nezapíše se toohello výstupního datového proudu. Zápis tooany jiných datových proudů v hello funkci bude zapisovat toohello odpovídajícího datového proudu pro hello runbook.

Vezměte v úvahu následující vzorový runbook hello.

    Workflow Test-Runbook
    {
        Write-Verbose "Verbose outside of function" -Verbose
        Write-Output "Output outside of function"
        $functionOutput = Test-Function
        $functionOutput

    Function Test-Function
     {
        Write-Verbose "Verbose inside of function" -Verbose
        Write-Output "Output inside of function"
      }
    }


Hello výstupního datového proudu pro úlohu hello runbook by byl:

    Output inside of function
    Output outside of function

podrobný datový proud Hello hello úlohy sady runbook by byl:

    Verbose outside of function
    Verbose inside of function

Jakmile publikujete hello runbook a než ho začnete, je potřeba také zapnout podrobné protokolování v hello nastavení sady runbook v pořadí tooget hello výstup podrobný datový proud.

### <a name="declaring-output-data-type"></a>Deklarující výstupní datový typ
Pracovní postup můžete zadat hello datový typ svého výstupu pomocí hello [atributu OutputType](http://technet.microsoft.com/library/hh847785.aspx). Tento atribut nemá žádný vliv za běhu, ale poskytuje Autor sady runbook toohello indikace v době návrhu hello očekávaný výstup runbooku hello. Hello nástrojů pro runbooky stále tooevolve, hello význam deklarování výstupních datových typů v době návrhu zvýší se význam. V důsledku toho je představuje dobrou praxi tooinclude tuto deklaraci v jakékoli sady runbook, který vytvoříte.

Tady je seznam příklad výstupu typy:

* System.String
* System.Int32
* System.Collections.Hashtable
* Microsoft.Azure.Commands.Compute.Models.PSVirtualMachine

Hello následující vzorový runbook výstup objektu řetězce a zahrnuje deklaraci jeho typu výstupu. Pokud runbook jako výstup pole určitého typu, pak měli byste specifikovat typ hello jako pole s názvem na rozdíl od tooan hello typu.

    Workflow Test-Runbook
    {
       [OutputType([string])]

       $output = "This is some string output."
       Write-Output $output
    }

toodeclare výstup zadejte Grapical nebo grafické prostředí PowerShell pracovního postupu sady runbook, můžete vybrat hello **vstup a výstup** nabídky možnost a zadejte název hello hello výstup typu.  Doporučujeme použít hello úplné rozhraní .NET třídy název toomake ho jednoduše rozpoznatelným názvem, pokud se odkazuje z nadřízeného runbooku.  Tím se zpřístupní všechny vlastnosti hello datové sběrnice toohello této třídy v hello runbook a poskytuje značnou flexibilitu při jejich používání pro podmíněnou logiku, protokolování a odkazování na jako hodnoty pro další aktivity v sadě runbook hello.<br> ![Možnost Runbook vstup a výstup](media/automation-runbook-output-and-messages/runbook-menu-input-and-output-option.png)

V následujícím příkladu hello máme dvě grafické runbooky toodemonstrate tuto funkci.  Pokud jsme použít model návrhu hello modulární runbook, máme jedné sady runbook, která slouží jako hello *šablony sad Runbook ověřování* správu ověřování s použitím Azure hello účet Spustit jako.  Druhý runbook, která by za normálních okolností provedete hello základní logiku tooautomate v daném scénáři v takovém případě bude tooexecute hello *šablony sad Runbook ověřování* a zobrazit výsledky tooyour hello **Test** podokno výstup.  Za normálních okolností se nám tuto sadu runbook dělat něco proti výstup hello využívání prostředků z podřízeného runbooku hello.    

Tady je základní logiku hello hello **AuthenticateTo Azure** sady runbook.<br> ![Ověření šablony sad Runbook příklad](media/automation-runbook-output-and-messages/runbook-authentication-template.png).  

Obsahuje hello výstupní typ *Microsoft.Azure.Commands.Profile.Models.PSAzureContext*, který vrátí hello vlastnosti profilu ověřování.<br> ![Příklad výstupu Runbook typu](media/automation-runbook-output-and-messages/runbook-input-and-output-add-blade.png) 

Tato sada runbook je velmi rovnou dál, je jeden toocall položka konfigurace se sem.  Poslední aktivita Hello provádí hello **Write-Output** rutiny a zápisy hello profil data tooa $_ proměnné pomocí prostředí PowerShell výrazu pro hello **Inputobject** parametr, který je vyžadován pro který rutiny.  

Hello druhé sady runbook v tomto příkladu, s názvem *Test ChildOutputType*, jednoduše máme dvě aktivity.<br> ![Příklad podřízených výstupní typ Runbooku](media/automation-runbook-output-and-messages/runbook-display-authentication-results-example.png) 

první aktivitu Hello volá hello **AuthenticateTo Azure** runbook a hello druhá aktivita běží hello **Write-Verbose** rutiny s hello **zdroj dat** z  **Výstup aktivity** a hodnotu hello **cesta pole** je **Context.Subscription.SubscriptionName**, což je zadání hello kontextu výstup hello  **AuthenticateTo Azure** sady runbook.<br> ![Rutiny Write-Verbose parametr zdroje dat](media/automation-runbook-output-and-messages/runbook-write-verbose-parameters-config.png)    

výsledný výstup Hello je hello název odběru hello.<br> ![Výsledky testu ChildOutputType sady Runbook](media/automation-runbook-output-and-messages/runbook-test-childoutputtype-results.png)

Jeden Poznámka o chování hello hello výstupní typ ovládacího prvku.  Pokud zadáte hodnotu v poli Výstupní typ hello na hello vstup a výstup okna vlastností, máte tooclick mimo dosah hello po zadání, aby vaše vstupní toobe rozpoznáno hello řízení.  

## <a name="message-streams"></a>Datové proudy zprávy
Na rozdíl od hello výstupního datového proudu jsou datové proudy zprávy určený toocommunicate informace toohello uživatele. Existují různé datové proudy zpráv pro různé typy informací a každý jinak zpracovávaných Azure Automation.

### <a name="warning-and-error-streams"></a>Datové proudy upozornění a chyb
datové proudy upozornění a chyby Hello jsou určený toolog problémy, které nastat v sadě runbook. Když runbook se spustí a jsou součástí hello podokna výstup testu v hello portálu pro správu Azure při testování sady runbook jsou psány toohello historie úlohy. Ve výchozím nastavení bude hello runbook pokračovat v provádění po upozornění a chyby. Můžete určit, že hello runbook pozastaví při varování nebo chyby nastavením [preferenční proměnné](#PreferenceVariables) v runbooku hello před vytvořením uvítací zprávu. Například toocause toosuspend runbook na chybu výjimku, stejně jako nastavit **$ErrorActionPreference** tooStop.

Vytvořte upozornění nebo chybové zprávy pomocí hello [Write-Warning](https://technet.microsoft.com/library/hh849931.aspx) nebo [Write-Error](http://technet.microsoft.com/library/hh849962.aspx) rutiny. Toothese datových proudů můžou zapisovat taky aktivity.

    #hello following lines create a warning message and then an error message that will suspend hello runbook.

    $ErrorActionPreference = "Stop"
    Write-Warning –Message "This is a warning message."
    Write-Error –Message "This is an error message that will stop hello runbook because of hello preference variable."

### <a name="verbose-stream"></a>Podrobný datový proud
Obecné informace o činnosti runbooku hello je Hello podrobný datový proud zpráv. Od hello [datový proud ladění](#Debug) není k dispozici v sadě runbook, podrobné zprávy se mají použít pro informace o ladění. Ve výchozím nastavení podrobné zprávy z publikovaných runbooků neuloží do historie úlohy hello. toostore podrobné zprávy, nakonfigurujte publikované sady runbook tooLog podrobné záznamy na kartě Konfigurace hello hello sady runbook v hello portálu pro správu Azure. Ve většině případů byste měli mít hello výchozí nastavení není protokolování podrobných záznamů pro runbook z důvodů výkonu. Zapnout tato možnost pouze tootroubleshoot nebo ladění runbooku.

Když [testování runbooku](automation-testing-runbook.md), podrobné zprávy nezobrazují, i když hello runbook je nakonfigurované toolog podrobných záznamů. podrobné zprávy toodisplay při [testování runbooku](automation-testing-runbook.md), je nutné nastavit proměnnou tooContinue hello $VerbosePreference. Pomocí tohoto nastavení proměnné podrobné zprávy zobrazí v hello podokna výstup testu z hello portálu Azure.

Vytvoření podrobné zprávy pomocí hello [Write-Verbose](http://technet.microsoft.com/library/hh849951.aspx) rutiny.

    #hello following line creates a verbose message.

    Write-Verbose –Message "This is a verbose message."

### <a name="debug-stream"></a>Datový proud ladění
datový proud ladění Hello je určena pro použití s interaktivním uživatelem a nesmí se používat v runboocích.

## <a name="progress-records"></a>Záznamů o průběhu
Pokud nakonfigurujete průběh toolog runbook zaznamenává (na kartě Konfigurace hello hello sady runbook v hello portál Azure) a potom záznam se zapíšou historie úlohy toohello před a po spuštění každé aktivity. Ve většině případů byste měli mít hello výchozí nastavení není protokolování záznamů o průběhu pro sadu runbook v pořadí toomaximize výkonu. Zapnout tato možnost pouze tootroubleshoot nebo ladění runbooku. Při testování runbooku se zprávy o průběhu nezobrazují, i když hello runbook je nakonfigurované toolog záznamů o průběhu.

Hello [Write-Progress](http://technet.microsoft.com/library/hh849902.aspx) rutiny není platný v sadě runbook, protože je určená pro použití s interaktivním uživatelem.

## <a name="preference-variables"></a>Proměnné předvoleb
Prostředí Windows PowerShell používá [proměnné předvoleb](http://technet.microsoft.com/library/hh847796.aspx) toodetermine jak toorespond toodata odeslané toodifferent výstupní datové proudy. Můžete nastavit tyto proměnné v runbooku toocontrol, jak bude reagovat, toodata zasílaná do různých datových proudů.

Hello následující tabulka uvádí hello proměnných předvoleb, které mohou být používány sady runbook s platnými a výchozími hodnotami. Všimněte si, že tato tabulka obsahuje jenom hello hodnoty, které jsou platné v runbooku. Další hodnoty jsou platné pro proměnné předvoleb hello při použití v prostředí Windows PowerShell mimo Azure Automation.

| Proměnná | Výchozí hodnota | Platné hodnoty |
|:--- |:--- |:--- |
| WarningPreference |Pokračovat |Zastavit<br>Pokračovat<br>SilentlyContinue |
| ErrorActionPreference |Pokračovat |Zastavit<br>Pokračovat<br>SilentlyContinue |
| VerbosePreference |SilentlyContinue |Zastavit<br>Pokračovat<br>SilentlyContinue |

Hello následující tabulka uvádí chování hello hello hodnoty proměnných předvoleb, které jsou platné v sadách runbook.

| Hodnota | Chování |
|:--- |:--- |
| Pokračovat |Protokoly uvítací zprávu a pokračuje v provádění sady runbook hello. |
| SilentlyContinue |Pokračuje v provádění hello runbooku bez protokolování zprávy hello. Tato akce nemá vliv hello ignorování uvítací zprávu. |
| Zastavit |Protokoly uvítací zprávu a pozastaví hello runbook. |

## <a name="retrieving-runbook-output-and-messages"></a>Načítání výstup a zprávy runbooku
### <a name="azure-portal"></a>portál Azure
Hello podrobnosti úlohy runbooku můžete zobrazit v hello portál Azure z karty hello úlohy sady runbook. Hello souhrn hello úlohy se zobrazí hello vstupní parametry a hello [výstupního datového proudu](#Output) v toogeneral informace o hello úloze a případných výjimkách, pokud k nim došlo. Hello historie bude obsahovat zprávy z hello [výstupního datového proudu](#Output) a [upozornění a chyby datové proudy](#WarningError) v přidání toohello [podrobný datový proud](#Verbose) a [průběh Zaznamenává](#Progress) Pokud je hello runbook nakonfigurované toolog podrobné a záznamů o průběhu.

### <a name="windows-powershell"></a>Windows PowerShell
V prostředí Windows PowerShell můžete načítat výstup a zprávy z runbooku pomocí hello [Get-AzureAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) rutiny. Tato rutina vyžaduje ID úlohy hello hello a má parametr nazvaný datového proudu, kde můžete určit, které tooreturn datového proudu. Můžete zadat všechny tooreturn všechny datové proudy úlohy hello.

Hello následující příklad spustí ukázkové sady runbook a pak čeká na její toocomplete. Po dokončení je jeho výstupní datový proud shromáždí z úlohy hello.

    $job = Start-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook"

    $doLoop = $true
    While ($doLoop) {
       $job = Get-AzureRmAutomationJob -ResourceGroupName "ResourceGroup01" `
       –AutomationAccountName "MyAutomationAccount" -Id $job.JobId
       $status = $job.Status
       $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped")
    }

    Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" -Id $job.JobId –Stream Output

### <a name="graphical-authoring"></a>Vytváření grafického obsahu
Další protokolování pro grafické runbooky, je k dispozici v podobě hello trasování na úrovni aktivity.  Existují dvě úrovně trasování: Basic a podrobné.  V základní trasování, uvidíte hello spuštění a čas ukončení každé aktivity v runbooku hello plus informace související s tooany opakování aktivity, jako je počet pokusů a čas zahájení hello aktivity.  V podrobného trasování získat plus základní trasování vstupní a výstupní data pro každou aktivitu.  Všimněte si, že aktuálně hello trasování záznamy jsou zapsány pomocí hello podrobný datový proud, proto je nutné povolit podrobné protokolování, pokud povolíte trasování.  Pro grafické runbooky s povolené trasování není nutné toolog záznamů o průběhu, protože slouží základní trasování hello hello účelu a je informativnější.

![Grafické vytváření úlohy datové proudy zobrazení](media/automation-runbook-output-and-messages/job-streams-view-blade.png)

Uvidíte z hello výše – snímek obrazovky, když povolíte podrobné protokolování a trasování pro grafické runbooky, mnohem víc informace jsou k dispozici v produkčním prostředí hello, které zobrazit datové proudy úlohy.  Tyto doplňující informace může být nezbytné pro řešení potíží s provozním problémům s sady runbook, a proto byste měli povolit pouze ho k tomuto účelu a ne jako obecně.    
zaznamenává Hello trasování může být obzvláště množství.  S grafický runbook trasování je můžete získat dva záznamy toofour na aktivitu v závislosti na tom, jestli jste nakonfigurovali základním nebo podrobném trasování.  Pokud potřebujete hello průběh tootrack informace o této sady runbook pro řešení potíží, můžete chtít tookeep trasování vypnuto.

**tooenable úrovni aktivity trasování, proveďte následující kroky hello.**

1. V hello portálu Azure otevřete účet Automation.
2. Klikněte na hello **Runbooky** dlaždice tooopen hello seznamu sad runbook.
3. V okně hello sady Runbook klikněte na tlačítko tooselect grafický runbook v seznamu sad runbook.
4. V okně Nastavení hello hello vybrané sady runbook, klikněte na tlačítko **protokolování a trasování**.
5. Na hello protokolování a trasování okno, v části protokolovat podrobné záznamy, klikněte na tlačítko **na** tooenable podrobné protokolování a trasování udner úrovni aktivity, změnit úroveň trasování hello příliš**základní** nebo **podrobné**  podle hello úroveň trasování požadavku.<br>
   
   ![Grafické vytváření protokolování a trasování okno](media/automation-runbook-output-and-messages/logging-and-tracing-settings-blade.png)

### <a name="microsoft-operations-management-suite-oms-log-analytics"></a>Microsoft Operations Management Suite (OMS) Log Analytics
Automatizace můžete odeslat runbook úlohy stavu a úlohu datové proudy tooyour Microsoft Operations Management Suite (OMS) pracovní prostor analýzy protokolů.  Pomocí analýzy protokolů je možné,

* Pohled na vaše úlohy automatizace 
* Aktivační událost e-mailem nebo výstrahy podle runbook stav úlohy (např. chybných nebo pozastavených) 
* Zápis pokročilými dotazy napříč vaše datové proudy úlohy 
* Vazbu mezi úlohy v účtech Automation 
* Vizualizace historii úlohy v čase    

Další informace o tom, jak tooconfigure integrace s toocollect analýzy protokolů korelaci a provádění akcí na data úlohy v tématu [předávání zpráv o stavu úlohy a datové proudy úlohy z automatizace tooLog Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).

## <a name="next-steps"></a>Další kroky
* Další informace o spuštění sady runbook, jak toomonitor úlohy a další technické podrobnosti najdete v tématu toolearn [sledovat úlohy runbooku](automation-runbook-execution.md)
* jak zjistit, toodesign a používání podřízené runbooky, toounderstand [podřízené runbooky ve službě Azure Automation](automation-child-runbooks.md)

