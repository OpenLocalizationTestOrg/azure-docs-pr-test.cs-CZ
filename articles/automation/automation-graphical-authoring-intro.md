---
title: "aaaGraphical vytváření obsahu v Azure Automation | Microsoft Docs"
description: "Vytváření grafického obsahu můžete toocreate sady runbook pro automatizaci Azure bez práce s kódem. Tento článek obsahuje toographical Úvod vytváření a všechny podrobnosti hello potřeby toostart vytváření grafický runbook."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 4b6f840c-e941-4293-a728-b33407317943
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/14/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 6ddf18b992d5e5f7f4af95f344007a63ac498549
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="graphical-authoring-in-azure-automation"></a>Grafické vytváření obsahu v Azure Automation.
## <a name="introduction"></a>Úvod
Vytváření grafického obsahu vám umožní toocreate sady runbook pro automatizaci Azure bez složité kroky hello hello základní prostředí Windows PowerShell nebo pracovního postupu Powershellu kódu. Přidejte plátno toohello aktivity z knihovny rutin a sady runbook, odkaz je společně a nakonfigurujte tooform pracovního postupu.  Pokud jste již někdy se System Center Orchestrator nebo Service Management Automation (SMA), to by měla vypadat známé tooyou.   

Tento článek obsahuje úvod toographical vytváření obsahu a hello koncepty, které že budete potřebovat tooget spuštěna při vytváření grafický runbook.

## <a name="graphical-runbooks"></a>Grafické runbooky
Všechny runbooky ve službě Azure Automation jsou pracovní postupy prostředí Windows PowerShell.  Grafické pracovní postup prostředí PowerShell a grafický runbook generování kódu prostředí PowerShell, který se spouští hello automatizace zaměstnanci, ale nejsou možné tooview ji nebo ji přímo upravovat.  Grafický runbook může být runbook převedený tooa grafické pracovního postupu Powershellu a naopak, ale nemohou být převedená tooa textovou runbook. Existující textovou sadu runbook nelze importovat do hello grafický editor.  

## <a name="overview-of-graphical-editor"></a>Přehled grafického editoru
Grafický editor hello v hello portálu Azure můžete otevřít vytvořením nebo úpravou grafický runbook.

![Grafické prostoru](media/automation-graphical-authoring-intro/runbook-graphical-editor.png)

Hello následující části popisují hello ovládacích prvků v hello grafický editor.

### <a name="canvas"></a>Plátno
Hello plátno je, kde můžete navrhnout vaše sada runbook.  Přidání aktivit z uzlů hello v hello knihovně řízení toohello runbook a jejich spojení s odkazy toodefine hello logiku hello sady runbook.

Ovládací prvky hello dole hello hello plátno toozoom můžete a odhlášení.

![Grafické prostoru](media/automation-graphical-authoring-intro/runbook-canvas-controls.png)

### <a name="library-control"></a>Knihovna řízení
Hello prvku knihovna slouží k výběru [aktivity](#activities) tooadd tooyour runbook.  Můžete je přidat toohello plátno, kdy se připojit je tooother aktivity.  Obsahuje čtyři oddíly, které jsou popsané v následující tabulce hello.

| Část | Popis |
|:--- |:--- |
| Rutiny |Zahrnuje všechny rutiny hello, které lze použít v sadě runbook.  Rutiny jsou uspořádané podle modulu.  Všechny hello moduly, které jste nainstalovali v účtu automation bude k dispozici. |
| Runbooky |Zahrnuje hello sady runbook ve vašem účtu automation. Tyto sady runbook lze přidat toobe toohello plátno použít jako podřízené sady runbook. Jsou zobrazeny pouze runbooky hello stejné základní typ jako hello runbook upravovaný; pro grafický jsou zobrazeny pouze pomocí prostředí PowerShell runbooky sady runbook, zatímco pro runbooky pracovních postupů grafické prostředí PowerShell jsou zobrazeny pouze PowerShell založené na pracovním postupu sady runbook. |
| Prostředky |Zahrnuje hello [prostředky automation](http://msdn.microsoft.com/library/dn939988.aspx) ve vašem účtu automation, který lze použít v sadě runbook.  Když přidáte runbook tooa služby asset, přidá aktivitu pracovního postupu, který získá hello vybrané asset.  V případě hello proměnné prostředků můžete vybrat, zda tooadd tooget aktivity hello hello proměnnou nebo nastavte proměnnou. |
| Řízení sady Runbook |Zahrnuje řízení aktivity sady runbook, které lze použít v aktuální sadě runbook. A *spojení* trvá více vstupů a čeká na všechny dokončili před trvalého hello pracovního postupu. A *kód* aktivita běží jeden nebo více řádků kódu prostředí PowerShell nebo pracovního postupu Powershellu v závislosti na typu hello grafický runbook.  Tuto aktivitu můžete použít pro vlastní kód nebo pro funkce, které je obtížné tooachieve s ostatními aktivitami. |

### <a name="configuration-control"></a>Řízení konfigurace
Hello řízení konfigurace je, kde zadáte informace pro objekt vybrané na plátno hello. Hello vlastnosti dostupné u tohoto ovládacího prvku bude záviset na typu hello vybraného objektu.  Když vyberete možnost v hello řízení konfigurace, otevře se další okna v pořadí tooprovide Další informace.

### <a name="test-control"></a>Testování ovládacího prvku
Hello testovací řízení se nezobrazí při prvním spuštění hello grafický editor. Když je otevřen můžete interaktivně [testování grafický runbook](#graphical-runbook-procedures).  

## <a name="graphical-runbook-procedures"></a>Postupy grafický runbook
### <a name="exporting-and-importing-a-graphical-runbook"></a>Export a import grafický runbook
Můžete exportovat pouze hello publikovanou verzi grafický runbook.  Pokud hello runbook ještě nebyla publikována, pak hello **Export publikovaná** tlačítko bude zakázán.  Když kliknete na tlačítko hello **Export publikovaná** tlačítko, hello sada runbook je stažené tooyour místního počítače.  Hello název souboru hello odpovídá názvu hello hello sady runbook s *graphrunbook* rozšíření.

![Export publikované](media/automation-graphical-authoring-intro/runbook-export.png)

Soubor runbook grafický nebo grafické prostředí PowerShell pracovního postupu můžete importovat tak, že vyberete hello **importovat** možnost při přidávání sady runbook.   Když vyberete soubor tooimport hello, můžete zachovat stejné hello **název** nebo zadejte nový.  Hello pole Typ Runbooku se zobrazí hello typ runbooku, až se vyhodnocuje hello soubor vybraný a pokud se pokusíte tooselect jiný typ, který není správný, že zpráva zobrazí poznamenat existují potenciální konflikty a během převodu, může dojít k chyby syntaxe.  

![Import sady runbook](media/automation-graphical-authoring-intro/runbook-import-revised20165.png)

### <a name="testing-a-graphical-runbook"></a>Testování grafický runbook
Hello koncept runbooku můžete otestovat ve hello portálu Azure při opuštění hello publikovaná verze sady runbook hello beze změny, nebo můžete otestovat nové sady runbook před publikování. Díky tomu, že jste tooverify který hello sady runbook před nahrazením publikované verze hello pracuje správně. Při testování runbooku hello koncept runbooku se spustí a všechny akce, které provádí se dokončí. Nevytvoří se žádné historie úlohy, ale zobrazí se výstup v hello podokna výstup testu. 

Otevřete hello řízení testů pro sadu runbook tak, že otevřete hello runbook pro úpravy a potom klikněte na hello **testovací podokno** tlačítko.

![Tlačítko testovací podokno](media/automation-graphical-authoring-intro/runbook-edit-test-pane.png)

Hello testovací řízení zobrazí výzvu pro všechny vstupní parametry a hello runbook můžete spustit kliknutím na hello **spustit** tlačítko.

![Test ovládací tlačítka](media/automation-graphical-authoring-intro/runbook-test-start.png)

### <a name="publishing-a-graphical-runbook"></a>Publikování grafický runbook
Každá sada runbook ve službě Azure Automation má koncept a publikovanou verzi. Pouze publikovaná verze hello je k dispozici toobe spustit a lze upravovat pouze hello verzi konceptu. Hello publikovaná verze není ovlivněna verzi konceptu toohello žádné změny. Pokud koncept hello je připraven toobe k dispozici, pak ji publikujete hello publikovaná verze přepíše s hello koncept.

Můžete publikovat grafický runbook otevřením hello runbook pro úpravy a potom kliknete na hello **publikovat** tlačítko.

![Tlačítko Publikovat](media/automation-graphical-authoring-intro/runbook-edit-publish.png)

Když runbook ještě nebyla publikována, má stav **nový**.  Při publikování, má stav **publikováno**.  Pokud chcete upravit hello runbook po publikování a hello koncept a publikovaný verze se liší, hello runbook je ve stavu **v upravit**.

![Stavy, které jsou sady Runbook](media/automation-graphical-authoring-intro/runbook-statuses-revised20165.png) 

Máte také hello možnost toorevert toohello publikovaná verze sady runbook.  Vyvolá rychle všechny změny provedené od hello runbook naposledy publikován a nahradí hello verzi konceptu sady runbook hello hello publikovanou verzi.

![Vrátit toopublished tlačítko](media/automation-graphical-authoring-intro/runbook-edit-revert-published.png)

## <a name="activities"></a>Aktivity
Aktivity jsou hello stavební bloky sady runbook.  Aktivita může být rutiny prostředí PowerShell, podřízené sady runbook nebo aktivit pracovního postupu.  Přidat toohello sada runbook pravým tlačítkem v hello prvku knihovna a výběrem **přidat toocanvas**.  Potom můžete klikněte a přetáhněte tooplace aktivity hello ho kdekoli na hello plátno, že se vám líbí.  umístění hello hello aktivit na plátno hello Hello neovlivňuje hello operace sady runbook hello žádným způsobem.  Můžete rozložení runbookem ale pro vás nejvhodnější toovisualize své činnosti. 

![Přidat toocanvas](media/automation-graphical-authoring-intro/add-to-canvas-revised20165.png)

Vyberte aktivitu hello na plátno tooconfigure hello jeho vlastnosti a parametry v okně Konfigurace hello.  Můžete změnit hello **popisek** z toosomething hello aktivity, který je popisný tooyou.  původní rutiny Hello je stále spuštěn, jednoduše měníte jeho zobrazovaný název, který se použije v hello grafický editor.  Popisek Hello musí být jedinečný v rámci hello runbook. 

### <a name="parameter-sets"></a>Sady parametrů
Definuje sadu parametrů hello povinné a nepovinné parametry, které bude přijímat hodnoty pro konkrétní rutiny.  Všechny rutiny mít minimálně jeden parametr nastavit a některé mají více.  Pokud rutina obsahuje několik sad parametrů, je třeba které z nich použijete před konfigurací parametry vybrat.  Hello parametry, které můžete nakonfigurovat bude záviset na hello sadu parametrů, který zvolíte.  Můžete změnit sadu parametrů hello používaná aktivitou výběrem **nastavený parametr** a vyberte jinou sadu.  V takovém případě budou ztraceny všechny hodnoty parametrů, které jste nakonfigurovali.

V následujícím příkladu hello hello Get-AzureRmVM rutina má tři sady parametrů.  Hodnoty parametru nelze nakonfigurovat, dokud vyberete jednu z hello sady parametrů.  Hello ListVirtualMachineInResourceGroupParamSet sada parametrů je pro návrat všechny virtuální počítače ve skupině prostředků a má jeden volitelný parametr.  Hello GetVirtualMachineInResourceGroupParamSet je pro zadání hello virtuální počítač má tooreturn a má dva povinné a jeden volitelný parametr.

![Sada parametrů](media/automation-graphical-authoring-intro/get-azurermvm-parameter-sets.png)

#### <a name="parameter-values"></a>Hodnoty parametru
Pokud zadáte hodnotu pro parametr, můžete vybrat toodetermine zdroje dat, jak bude zadána hodnota hello.  Hello datových zdrojů, které jsou k dispozici pro konkrétní parametr bude záviset na hello platné hodnoty pro tento parametr.  Například Null nebude k dispozici možnost pro parametr, který nepovoluje hodnoty null.

| Zdroj dat | Popis |
|:--- |:--- |
| Konstantní hodnota |Zadejte hodnotu pro parametr hello.  To je dostupná jenom pro následující typy dat hello: Int32, Int64, řetězec, logická hodnota, datum a čas, přepínač. |
| Výstup aktivity |Výstup z aktivity, která předchází hello aktuální aktivity v pracovním postupu hello.  Objeví se všechny platné aktivity.  Vyberte právě toouse aktivity hello jeho výstup hello hodnotu parametru.  Pokud výstupem hello aktivity je objekt s více vlastností, potom můžete zadat v hello název vlastnosti hello po výběru hello aktivity. |
| Vstup z Runbooku |Vyberte vstupní parametr runbooku jako vstupní toohello parametr aktivity. |
| Variabilní prostředek |Vyberte proměnné Automation jako vstup. |
| Asset přihlašovacích údajů |Vyberte pověření Automation jako vstup. |
| Asset certifikátu |Vyberte certifikát Automation jako vstup. |
| Asset připojení |Vyberte připojení Automation jako vstup. |
| Powershellový výraz |Zadat jednoduchý [Powershellový výraz](#powershell-expressions).  výraz Hello bude vyhodnocena před aktivity a hello výsledek hello používá pro hodnotu parametru hello.  Můžete použít proměnné toorefer toohello výstup aktivity nebo vstupní parametr runbooku. |
| Není nakonfigurováno |Vymaže hodnotu, která byla nastavena. |

#### <a name="optional-additional-parameters"></a>Další volitelné parametry
Všechny rutiny bude mít hello možnost tooprovide další parametry.  Jedná se o běžné parametry Powershellu nebo jiné vlastní parametry.  Se zobrazí v textovém poli, ve kterém můžete zadat parametry pomocí syntaxe Powershellu.  Například toouse hello **podrobné** společný parametr, zadali byste **"-Verbose: $True"**.

### <a name="retry-activity"></a>Opakujte aktivity
**Postup opakování** umožňuje toobe aktivity spustit vícekrát, dokud je splněna určitá podmínka, podobně jako smyčku.  Tuto funkci můžete použít pro aktivity, které musí spustit vícekrát, jsou chyby náchylné k chybám a může potřebovat více než jeden pokus pro úspěch, nebo testovat hello výstupních informacích aktivity hello platná data.    

Když povolíte opakování pro aktivitu, můžete nastavit zpoždění a podmínku.  zpoždění Hello je čas hello (měřeno v několika sekund nebo minut) dané hello sady runbook bude čekat před jeho spuštěním hello aktivity.  Pokud žádné zpoždění není zadaný, pak hello aktivity se spustí znovu ihned po jeho dokončení. 

![Zpoždění při opakování aktivity](media/automation-graphical-authoring-intro/retry-delay.png)

Podmínka opakování Hello je Powershellový výraz, který se vyhodnotí po každé aktivitě hello čas spuštění.  Pokud hello výraz řeší tooTrue, pak hello aktivity spouští znovu.  Pokud hello výraz přeloží tooFalse hello aktivita není spusťte znovu a hello runbook přesune na další aktivitu toohello. 

![Zpoždění při opakování aktivity](media/automation-graphical-authoring-intro/retry-condition.png)

Hello opakování podmínku můžete použít proměnné s názvem $RetryData poskytující přístup tooinformation o opakování aktivity hello.  Tato proměnná nemá hello vlastnosti v hello následující tabulka.

| Vlastnost | Popis |
|:--- |:--- |
| NumberOfAttempts |Počet pokusů, které aktivity hello byl spuštěn. |
| Výstup |Výstup hello posledního spuštění aktivity hello. |
| Agent |Vypršel časový limit aktivity hello od spuštění hello poprvé. |
| StartedAt |Čas v UTC formátu hello aktivity byl prvním spuštění. |

Následují příklady aktivity opakování podmínky.

    # Run hello activity exactly 10 times.
    $RetryData.NumberOfAttempts -ge 10 

    # Run hello activity repeatedly until it produces any output.
    $RetryData.Output.Count -ge 1 

    # Run hello activity repeatedly until 2 minutes has elapsed. 
    $RetryData.TotalDuration.TotalMinutes -ge 2

Po dokončení konfigurace podmínku opakování pro aktivitu, hello aktivity obsahuje dva vizuální upozornění tooremind můžete.  Jeden se zobrazí v aktivitě hello a hello jiných je při kontrole konfigurace hello hello aktivity.

![Indikátory Visual opakování aktivity](media/automation-graphical-authoring-intro/runbook-activity-retry-visual-cue.png)

### <a name="workflow-script-control"></a>Řízení pracovního postupu skriptu
Ovládací prvek kód je speciální aktivitě, který přijímá skript prostředí PowerShell nebo pracovního postupu Powershellu v závislosti na typu hello grafický runbook právě vytvořené v pořadí tooprovide funkce, které nemusí být jinak k dispozici.  Nemůže přijmout parametry, ale může použít proměnné pro aktivitu výstup a runbook vstupní parametry.  Žádný výstup aktivity hello je přidaná datové sběrnice toohello pouze pokud má odchozí propojení ve kterém je případ se přidal výstup toohello hello sady runbook.

Například hello následující kód provede výpočty data pomocí sady runbook vstupní proměnné názvem $NumberOfDays.  Pak odešle počítané datum čas jako výstup toobe používané následné aktivity v sadě runbook hello.

    $DateTimeNow = (Get-Date).ToUniversalTime()
    $DateTimeStart = ($DateTimeNow).AddDays(-$NumberOfDays)}
    $DateTimeStart


## <a name="links-and-workflow"></a>Odkazy a pracovní postup
A **odkaz** v grafický runbook připojení dvě aktivity.  Na plátně hello se zobrazí jako šipka z hello zdrojové aktivity toohello cílové aktivity.  aktivity Hello spusťte hello směrové šipky hello s hello cílová aktivita spouští po dokončení zdrojové aktivity hello.  

### <a name="create-a-link"></a>Vytvoření odkazu
Vytvořte propojení mezi dvěma aktivitami aktivitou výběr zdroje hello a kliknutím na kruh hello v hello dolní části obrazce hello.  Přetáhněte hello šipku toohello cílová aktivita a verzi.

![Vytvoření odkazu](media/automation-graphical-authoring-intro/create-link-revised20165.png)

V okně Konfigurace hello vyberte hello odkaz tooconfigure jeho vlastnosti.  Sem patří hello typu odkaz, který je popsaný v následující tabulce hello.

| Typ propojení | Popis |
|:--- |:--- |
| Kanál |Cílová aktivita Hello je spustit jednou pro každý objekt výstup z hello zdrojové aktivity.  Hello cílová aktivita se nespustí, pokud hello zdrojové aktivity, které jsou výsledkem žádný výstup.  Výstup hello zdrojové aktivity k dispozici jako objekt. |
| Pořadí |Hello cílová aktivita se spustí jenom jednou.  Obdrží pole objektů ze zdrojové aktivity hello.  Výstup hello zdrojové aktivity k dispozici jako pole objektů. |

### <a name="starting-activity"></a>Spuštění aktivity
Grafický runbook se spustí se veškeré aktivity, které nemají příchozí propojení.  Často se bude jenom jedna aktivita, která bude fungovat jako hello spuštění aktivity sady runbook hello.  Pokud více aktivit nemají příchozí propojení, hello runbook se spustí spuštěním paralelně.  Ho bude potom postupujte podle hello odkazy toorun dalšími aktivitami každý dokončení.

### <a name="conditions"></a>Podmínky
Když zadáte podmínku na propojení, hello cílová aktivita se spustí jenom v případě hello podmínka přeložená tootrue.  Obvykle použijete proměnnou $ActivityOutput v podmínky tooretrieve hello výstup hello zdrojové aktivity.  

Pro kanál odkaz zadejte podmínku pro jediný objekt a podmínky hello je vyhodnocován pro každý objekt výstup hello zdrojové aktivity.  Cílová aktivita Hello je pak spusťte pro každý objekt, který splňuje podmínky hello.  Například s aktivitou zdroj Get-AzureRmVm hello se syntaxí by mohly být použity kanálu podmíněného propojení tooretrieve pouze virtuální počítače v hello skupinu prostředků s názvem *Group1*.  

    $ActivityOutput['Get Azure VMs'].Name -match "Group1"

Pro odkaz a pořadí hello podmínka je Vyhodnocená jenom jednou vzhledem k tomu, že do jednoho pole se vrátí, obsahuje všechny objekty výstup hello zdrojové aktivity.  Z toho důvodu pořadí odkaz nelze použít pro filtrování jako propojení kanálu, ale jednoduše určí, zda text hello další aktivita spuštěna. Například trvat hello následující sadu aktivit v našem runbook spustit virtuální počítač.<br> ![Podmíněného propojení s pořadí](media/automation-graphical-authoring-intro/runbook-conditional-links-sequence.png)<br>
Existují tři různé pořadí propojení, které jsou ověření hodnoty byly poskytnuty vstupní parametry pro tootwo runbook představující název virtuálního počítače a název skupiny prostředků v pořadí toodetermine, což je tootake hello příslušné akce – spuštění jednoho virtuálního počítače, spuštění všech virtuálních počítačů v hello Skupina odstraňovaného prostředku, nebo všechny virtuální počítače v předplatném.  Hello pořadí propojení mezi tooAzure připojení a jeden virtuální počítač Get zde je logiku hello podmínku:

    <# 
    Both VMName and ResourceGroupName runbook input parameters have values 
    #>
    (
    (($VMName -ne $null) -and ($VMName.Length -gt 0))
    ) -and (
    (($ResourceGroupName -ne $null) -and ($ResourceGroupName.Length -gt 0))
    )

Při použití podmíněného propojení hello podmínka bude filtrováno hello data dostupná z hello zdroj aktivity tooother aktivit uvedené pobočky.  Pokud aktivita hello zdroj toomultiple odkazy, pak hello data, která je k dispozici tooactivities v každé větve, bude záviset na hello podmínky v odkazu hello připojení toothat větev.

Například hello **Start-AzureRmVm** aktivity v sadě runbook hello níže spustí všechny virtuální počítače.  Má dva podmíněných propojení.  první podmíněného propojení Hello používá hello výraz *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - eq $true* toofilter Pokud aktivitu Start-AzureRmVm hello úspěšně dokončit.  Hello používá druhý výraz hello *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - ne $true* toofilter Pokud hello Start-AzureRmVm aktivity se nepovedlo toostart hello virtuálního počítače.  

![Příklad podmíněného propojení](media/automation-graphical-authoring-intro/runbook-conditional-links.png)

Všechny aktivity, která odpovídá hello první odkaz a používá hello výstup aktivity z Get-AzureVM pouze získají hello virtuálních počítačů, které byly spuštěny v době hello, která byla spuštěna Get-AzureVM.  Všechny aktivity, která odpovídá druhý odkaz hello pouze získají hello hello virtuálních počítačů, které jste zastavili během hello, která byla spuštěna Get-AzureVM.  Všechny aktivity následující odkaz třetí hello se všechny virtuální počítače bez ohledu na jejich stavu spuštěno.

### <a name="junctions"></a>Spojovacích bodech
Spojení je speciální aktivitě počká, byly dokončeny všechny příchozí větve.  To vám umožní toorun více aktivit paralelně a ujistěte se, že všechny dokončili než budete pokračovat.

Když spojení, může mít neomezený počet příchozí odkazy, více než jeden z těchto odkazů může být kanálu.  Hello počet odkazů na příchozí pořadí není omezené.  Bude možné toocreate hello spojení s více příchozí propojení kanálu a uložení hello runbooku, ale dojde při spuštění.

Následující příklad Hello je součástí sady runbook, která se spouští sada virtuálních počítačů při stahování současně opravy toobe použití toothose počítače.  Spojení je použité tooensure že oba tyto procesy jsou dokončeny před hello runbook pokračuje.

![Spojení](media/automation-graphical-authoring-intro/runbook-junction.png)

### <a name="cycles"></a>Cykly
Cyklus je při cílová aktivita odkazuje zpět tooits zdrojové aktivity nebo tooanother aktivity, nakonec odkazy Zpět tooits zdroje.  Cykly nejsou aktuálně povolená ve vytváření grafického obsahu.  Pokud vaše sada runbook má cyklus, uloží správně, ale dojde k chybě při spuštění.

![Cyklus](media/automation-graphical-authoring-intro/runbook-cycle.png)

### <a name="sharing-data-between-activities"></a>Sdílení dat mezi aktivitami
Všechna data, která je výstupní aktivitou s odchozí propojení se zapíše toohello *datové sběrnice* hello sady runbook.  Všechny aktivity v hello runbook můžete použít dat na hodnoty parametrů toopopulate hello datové sběrnice nebo zahrnout kód skriptu.  Aktivity mají přístup k výstup hello všechny předchozí aktivity v pracovním postupu hello.     

Jak hello data se zapisují datové sběrnice toohello závisí na typu hello odkazu na hello aktivity.  Pro **kanálu**, hello dat je výstup jako objekty násobky.  Pro **pořadí** propojit, hello dat je výstup jako pole.  Pokud existuje pouze jedna hodnota, bude výstup jako pole jediným elementem.

Můžete přistupovat k datům v datové sběrnice hello pomocí jedné ze dvou způsobů.  Nejprve používá **výstup aktivity** zdroje dat toopopulate parametr jiné aktivity.  Pokud výstup hello je objekt, můžete zadat vlastnosti jediné.

![Výstup aktivity](media/automation-graphical-authoring-intro/activity-output-datasource-revised20165.png)

Můžete také načíst výstup hello aktivity v **Powershellový výraz** zdroj dat nebo z **Workflow je skript** aktivitu se proměnná ActivityOutput.  Pokud výstup hello je objekt, můžete zadat vlastnosti jediné.  Proměnné ActivityOutput pomocí následující syntaxe hello.

    $ActivityOutput['Activity Label']
    $ActivityOutput['Activity Label'].PropertyName 

### <a name="checkpoints"></a>Kontrolní body
Můžete nastavit [kontrolní body](automation-powershell-workflow.md#checkpoints) v sadě runbook pracovního postupu grafické prostředí PowerShell tak, že vyberete *kontrolního bodu runbook* na žádnou aktivitu.  To způsobí, že kontrolní bod toobe, nastavit po spuštění aktivity hello.

![Kontrolní bod](media/automation-graphical-authoring-intro/set-checkpoint.png)

Kontrolní body jsou povoleny pouze v runboocích pracovního postupu grafické prostředí PowerShell, není k dispozici v grafické runbooky.  Pokud hello runbook používá rutiny Azure, postupujte podle žádnou aktivitu kontrolní bod s Add-AzureRMAccount v případě, že hello runbook pozastavený a restartuje z této kontrolního bodu na jiný pracovní. 

## <a name="authenticating-tooazure-resources"></a>Ověřování tooAzure prostředky
Sady Runbook ve službě Azure Automation, které spravovat prostředky Azure bude vyžadovat tooAzure ověřování.  Hello [účet Spustit jako](automation-offering-get-started.md#creating-an-automation-account) (také odkazované tooas instanční objekt) je hello výchozí metoda tooaccess prostředky Azure Resource Manager ve vašem předplatném pomocí runbooků Automation.  Tato funkce tooa grafický runbook můžete přidat tak, že přidáte hello **AzureRunAsConnection** asset připojení, která používá hello prostředí PowerShell [Get-AutomationConnection](https://technet.microsoft.com/library/dn919922%28v=sc.16%29.aspx) rutiny a [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) rutiny toohello plátno. To je znázorněno v následující ukázka hello.<br>![Spustit jako ověřování aktivity](media/automation-graphical-authoring-intro/authenticate-run-as-account.png)<br>
Hello aktivity získat připojení spustit jako (tj. Get-AutomationConnection), je nakonfigurovaný s konstantní hodnotou zdroj dat s názvem AzureRunAsConnection.<br>![Konfigurace připojení spustit jako](media/automation-graphical-authoring-intro/authenticate-runas-parameterset.png)<br>
Hello další aktivitu, Add-AzureRmAccount, přidá hello ověření spustit jako účet pro použití v runbooku hello.<br>
![Sada parametrů přidat AzureRmAccount](media/automation-graphical-authoring-intro/authenticate-conn-to-azure-parameter-set.png)<br>
Pro parametry hello **APPLICATIONID**, **CERTIFICATETHUMBPRINT**, a **TENANTID** budete potřebovat toospecify hello název hello vlastnosti pro cestu pole hello protože výstupem Hello aktivity je objekt s více vlastnostmi.  V opačném případě při spuštění sady runbook hello dojde při pokusu tooauthenticate.  Je to, co je potřeba na minimální tooauthenticate vaše sada runbook s hello účet Spustit jako.

toomaintain zpětné kompatibility pro odběratele, kteří vytvořili Automation účet pomocí [účtu uživatele Azure AD](automation-create-aduser-account.md) toomanage nasazení Azure classic nebo pro prostředky Azure Resource Manager hello tooauthenticate – metoda je rutina Add-AzureAccount hello s [asset přihlašovacích údajů](automation-credentials.md) představující uživatele služby Active Directory s toohello přístup k účtu Azure.

Tato funkce tooa grafický runbook můžete přidat tak, že přidáte na plátno toohello asset přihlašovacích údajů následuje Add-AzureAccount aktivitu.  Přidat-AzureAccount používá hello pověření aktivity pro vstupní.  To je znázorněno v následující ukázka hello.

![Ověřování aktivity](media/automation-graphical-authoring-intro/authentication-activities.png)

Máte tooauthenticate při spuštění hello hello sady runbook a po každém kontrolního bodu.  To znamená, přidání aktivity přidání Add-AzureAccount po žádnou aktivitu Checkpoint-Workflow. Není nutné pověření Přidání aktivity, protože můžete použít stejné hello 

![Výstup aktivity](media/automation-graphical-authoring-intro/authentication-activity-output.png)

## <a name="runbook-input-and-output"></a>Runbook vstup a výstup
### <a name="runbook-input"></a>Vstup z Runbooku
Sada runbook může vyžadovat vstup buď od uživatele při spuštění runbooku hello prostřednictvím hello portál Azure nebo z jiného runbooku Pokud hello stávající slouží jako podřízený.
Například pokud máte sadu runbook, která vytvoří virtuální počítač, musíte tooprovide informace, jako je název hello hello virtuálního počítače a další vlastnosti pokaždé, když spustíte hello runbook.  

Přijmout vstup pro sadu runbook tak, že definujete jeden nebo více vstupních parametrů.  Zadejte hodnoty pro tyto parametry, které každý čas hello runbooku.  Pokud runbook spustíte s hello portálu Azure, vás vyzve tooprovide hodnoty pro hello hello runbook vstupních parametrů.

Vstupní parametry pro sady runbook můžete přístup kliknutím hello **vstup a výstup** tlačítka na panelu nástrojů runbook hello.  

![Vstup z Runbooku výstup](media/automation-graphical-authoring-intro/runbook-edit-input-output.png) 

Tím se otevře hello **vstup a výstup** řízení, kde můžete upravit existující vstupní parametr nebo vytvořte novou kliknutím **přidat vstup**. 

![Přidat vstup](media/automation-graphical-authoring-intro/runbook-edit-add-input.png)

Každý vstupní parametr je definován vlastností hello v hello následující tabulka.

| Vlastnost | Popis |
|:--- |:--- |
| Name (Název) |Hello jedinečný název parametru hello.  To může obsahovat pouze alfanumerické znaky a nesmí obsahovat mezery. |
| Popis |Volitelný popis pro vstupní parametr hello. |
| Typ |Datový typ pro hodnotu parametru hello.  Hello portál Azure poskytne příslušný ovládací prvek pro hello datový typ pro každý parametr při zobrazení výzvy ke vstupu. |
| Povinné |Určuje, zda musí být zadána hodnota parametru hello.  Hello runbook nejde spustit, pokud nezadáte hodnotu pro každý povinný parametr, který nemá výchozí hodnotu definovanou. |
| Výchozí hodnota |Určuje, jaká hodnota se používá pro parametr hello, pokud není zadáno.  To může být buď hodnotu Null nebo konkrétní hodnotu. |

### <a name="runbook-output"></a>Výstup runbooku
Data vytvořená aktivitou, která nemá odchozí propojení se přidá toohello [výstup hello runbook](http://msdn.microsoft.com/library/azure/dn879148.aspx).  Hello výstup bude uložen s hello úloha sady runbook a je k dispozici tooa nadřízený runbook, když hello runbook slouží jako podřízený.  

## <a name="powershell-expressions"></a>Výrazy prostředí PowerShell
Jednou z výhod hello vytváření grafického obsahu nabízí možnost toobuild hello sady runbook s minimálními znalostmi prostředí PowerShell.  V současné době potřebují tooknow verze prostředí PowerShell, i když pro sestavování určité [hodnoty parametrů](#activities) a pro nastavení [odkaz podmínky](#links-and-workflow).  Tato část obsahuje rychlý úvod tooPowerShell výrazy pro uživatele, kteří nemusí být obeznámeni s ním.  Podrobnosti o prostředí PowerShell jsou k dispozici na [skriptování v prostředí Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx). 

### <a name="powershell-expression-data-source"></a>Zdroj dat výraz prostředí PowerShell
Výraz prostředí PowerShell můžete použít jako zdroj toopopulate hello hodnotou dat [parametr aktivity](#activities) s výsledky hello některé kódu prostředí PowerShell.  To může být jediný řádek kódu, který provádí některé jednoduché funkce nebo více řádků, které provádějí některé komplexní logiku.  Všechny výstupy z příkaz, který není přiřazen tooa proměnné je hodnota parametru toohello výstup. 

Hello následující příkaz například by výstup hello aktuální datum. 

    Get-Date

Hello následující příkazy vytvořit řetězec z hello aktuální datum a přiřaďte ho tooa proměnné.  obsah Hello hello proměnné se pak odešlou toohello výstup 

    $string = "hello current date is " + (Get-Date)
    $string

Hello následující příkazy vyhodnotit hello aktuální datum a vrátí řetězec označující, zda text hello aktuální den víkendu nebo den v týdnu. 

    $date = Get-Date
    if (($date.DayOfWeek = "Saturday") -or ($date.DayOfWeek = "Sunday")) { "Weekend" }
    else { "Weekday" }


### <a name="activity-output"></a>Výstup aktivity
výstup hello toouse z předchozí aktivity v sadě runbook hello, použijte hello $ActivityOutput proměnné se syntaxí hello.

    $ActivityOutput['Activity Label'].PropertyName

Například může mít aktivita, jejíž vlastnosti, která vyžaduje hello název virtuálního počítače v takovém případě můžete použít následující výraz hello.

    $ActivityOutput['Get-AzureVm'].Name

Pokud by vrátit hello vlastnost, která vyžaduje objektu virtuálního počítače hello místo právě vlastnost a potom můžete pomocí celý objekt hello hello následující syntaxe.

    $ActivityOutput['Get-AzureVm']

Můžete taky hello výstup aktivity ve složitější výrazu například následující text hello, který zřetězí název virtuálního počítače toohello text.

    "hello computer name is " + $ActivityOutput['Get-AzureVm'].Name


### <a name="conditions"></a>Podmínky
Použití [operátory porovnání](https://technet.microsoft.com/library/hh847759.aspx) toocompare hodnoty nebo zjistit, jestli hodnota odpovídá zadanému vzoru.  Porovnání vrací hodnotu $true nebo $false.

Například následující podmínku hello Určuje, zda text hello virtuálního počítače z aktivity s názvem *Get-AzureVM* právě *zastavena*. 

    $ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped"

Hello následující podmínka kontroluje, zda hello stejný virtuální počítač je v libovolném stavu jiné než *zastavena*.

    $ActivityOutput["Get-AzureVM"].PowerState –ne "Stopped"

Toho se můžete zapojit více podmínek použití [logický operátor](https://technet.microsoft.com/library/hh847789.aspx) , jako **- a** nebo **- nebo**.  Například hello následující podmínka kontroluje, zda hello stejný virtuální počítač v předchozím příkladu hello je ve stavu *zastavena* nebo *zastavení*.

    ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped") -or ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopping") 


### <a name="hashtables"></a>Zatřiďovacích tabulkách
[Zatřiďovacích tabulkách](http://technet.microsoft.com/library/hh847780.aspx) jsou páry název/hodnota, které jsou užitečné pro vrácení sadu hodnot.  Vlastnosti pro určité aktivity se očekává, že zatřiďovací tabulku namísto jednoduché hodnoty.  Může se také zobrazit jako zatřiďovací tabulky označuje tooas slovník. 

Vytvořit tabulku hash pomocí následující syntaxe hello.  Zatřiďovací tabulky může obsahovat libovolný počet položek, ale každý je definována podle názvu a hodnoty.

    @{ <name> = <value>; [<name> = <value> ] ...}

Například hello následující výraz vytvoří zatřiďovací tabulky toobe, použít ve zdroji dat hello parametru aktivity, která očekává zatřiďovací tabulku s hodnotami pro hledání v Internetu.

    $query = "Azure Automation"
    $count = 10
    $h = @{'q'=$query; 'lr'='lang_ja';  'count'=$Count}
    $h

Hello následující příklad používá výstupem z aktivity volá *získat připojení služby Twitter* toopopulate zatřiďovací tabulku.

    @{'ApiKey'=$ActivityOutput['Get Twitter Connection'].ConsumerAPIKey;
      'ApiSecret'=$ActivityOutput['Get Twitter Connection'].ConsumerAPISecret;
      'AccessToken'=$ActivityOutput['Get Twitter Connection'].AccessToken;
      'AccessTokenSecret'=$ActivityOutput['Get Twitter Connection'].AccessTokenSecret}



## <a name="next-steps"></a>Další kroky
* tooget kroky s runbooky pracovních postupů Powershellu, najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md) 
* tooget kroky s grafickými runbooky najdete v části [Můj první grafický runbook](automation-first-runbook-graphical.md)
* tooknow Další informace o typech runbooků, jejich výhody a omezení, najdete v části [typy runbooků ve službě Azure Automation](automation-runbook-types.md)
* toounderstand způsobu použití tooauthenticate hello účtu Automation spustit jako najdete v [konfigurace Azure účet Spustit jako](automation-sec-configure-azure-runas-account.md)

