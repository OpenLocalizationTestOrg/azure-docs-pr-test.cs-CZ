---
title: aaaMigrating z Orchestrator tooAzure automatizace | Microsoft Docs
description: "Popisuje, jak toomigrate sady runbook a integrační sady z produktu System Center Orchestrator tooAzure automatizace."
services: automation
documentationcenter: 
author: bwren
manager: stevenka
editor: tysonn
ms.assetid: 1a7da58c-7a98-49b5-9d9d-001a9f6e631a
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2016
ms.author: bwren
ms.openlocfilehash: 797b50067ef2aa68470760e99d494b89ab7baacf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-from-orchestrator-tooazure-automation-beta"></a>Migrace z produktu Orchestrator tooAzure automatizace (Beta)
Sady Runbook v [System Center Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) jsou založené na aktivit z integračních balíčků, které jsou napsané konkrétně pro Orchestrator, zatímco runbooky ve službě Azure Automation jsou založené na prostředí Windows PowerShell.  [Grafické runbooky](automation-runbook-types.md#graphical-runbooks) ve službě Azure Automation mají podobné runbooky tooOrchestrator vzhled s jejich aktivity představující rutiny prostředí PowerShell, podřízené runbooky a prostředky.

Hello [nástrojů System Center Orchestrator migrace](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) zahrnuje nástroje tooassist v převod z Orchestrator tooAzure automatizace sady runbook.  Kromě toho tooconverting hello sady runbook sami, je nutné převést hello integrační balíčky s aktivitami hello že hello sady runbook použijte moduly toointegration pomocí rutin prostředí Windows PowerShell.  

Následuje hello základní proces převodu tooAzure sady runbook Orchestrator automatizace.  Každý z těchto kroků je podrobně popsány v následujících částech hello.

1. Stáhnout hello [nástrojů System Center Orchestrator migrace](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) obsahující hello nástrojů a modulů, které jsou popsané v tomto článku.
2. Import [standardní aktivity modulu](#standard-activities-module) do služby Azure Automation.  To zahrnuje převedený verzích standardní Orchestrator aktivity, které může být používán převedené sady runbook.
3. Import [System Center Orchestrator integrační moduly](#system-center-orchestrator-integration-modules) do Azure Automation pro tyto integrační balíčky používané vaší sady runbook, které přístup k produktu System Center.
4. Převést vlastní a třetích stran integrační balíčky pomocí hello [Integration Pack převaděč](#integration-pack-converter) a importovat do Azure Automation.
5. Převést sady runbook nástroje Orchestrator pomocí hello [Runbook převaděč](#runbook-converter) a nainstalujte ve službě Azure Automation.
6. Vzhledem k tomu, že hello Runbook převaděč nepřevádět tyto prostředky, vytvořte ručně požadované prostředky Orchestrator ve službě Azure Automation.
7. Konfigurace [hybridní pracovní proces Runbooku](#hybrid-runbook-worker) ve vašich runboocích toorun převést center místní data, která bude přístup k místním prostředkům.

## <a name="service-management-automation"></a>Service Management Automation
[Službě Service Management Automation](http://technet.microsoft.com/library/dn469260.aspx) (SMA) ukládá a spouští sady runbook ve vašem místním datovém centru jako Orchestrator a používá hello stejné moduly integrace jako Azure Automation. Hello [Runbook převaděč](#runbook-converter) ale převede toographical runbooky služby Orchestrator runbook, které nejsou podporovány ve službě SMA.  Můžete nainstalovat hello [standardní aktivity modulu](#standard-activities-module) a [System Center Orchestrator integrační moduly](#system-center-orchestrator-integration-modules) do služby SMA, ale vy musíte ručně [přepisování runbooky](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker
Sady Runbook v nástroji Orchestrator jsou uložené na serveru databáze a spusťte v serverech sady runbook, i v místním datovém centru.  Runbooky ve službě Azure Automation jsou uložené v hello cloudu Azure a můžete spustit v místní data pomocí center [hybridní pracovní proces Runbooku](automation-hybrid-runbook-worker.md).  Toto je, jak bude obvykle spuštění sad runbook převést z nástroje Orchestrator, protože jsou navrženou toorun na místní servery.

## <a name="integration-pack-converter"></a>Převaděč Integration Pack
Hello Integration Pack převaděč převede integrační balíčky, které byly vytvořené pomocí hello [Orchestrator Integration Toolkit (OIT)](http://technet.microsoft.com/library/hh855853.aspx) toointegration moduly založené na prostředí Windows PowerShell, který lze importovat do Azure Automation, nebo Službě Service Management Automation.  

Když spustíte hello Integration Pack převaděč, máte k ruce průvodce, který vám umožní tooselect soubor integračního balíčku (.oip).  Průvodce Hello potom zobrazí hello aktivity součástí daného integračního balíčku a umožňuje tooselect, které se budou migrovat.  Po dokončení Průvodce hello vytvoří modul integrace, která zahrnuje odpovídajících rutin pro každý hello aktivit v hello původní integračního balíčku.

### <a name="parameters"></a>Parametry
Převedený tooparameters hello odpovídajících rutin v modulu integrace hello jsou jakékoli vlastnosti aktivity v hello integračního balíčku.  Rutiny prostředí Windows PowerShell obsahují sadu [společné parametry](http://technet.microsoft.com/library/hh847884.aspx) který lze použít s všechny rutiny.  Například hello - Verbose parametr způsobí, že rutiny toooutput podrobné informace o své činnosti.  Parametr s hello stejný název jako společný parametr může mít žádné rutiny.  Pokud aktivita nemá vlastnost s hello stejný název jako společný parametr, zobrazí se Průvodce hello výzvu tooprovide můžete jiný název pro parametr hello.

### <a name="monitor-activities"></a>Monitorování aktivit
Monitorování sady runbook v produktu Orchestrator začněte s [sledovat činnost](http://technet.microsoft.com/library/hh403827.aspx) a spouštět nepřetržitě toobe čekání vyvolané určitá událost.  Služby Azure Automation nepodporuje sady runbook monitorování, takže všechny aktivity monitorování v hello integračního balíčku nebudou převedeny.  Místo toho zástupný symbol rutiny se vytvoří v modulu integrace hello monitorování aktivity hello.  Tato rutina má používat žádné funkce, ale umožňuje převedené sady runbook, která jej používá toobe nainstalována.  Tato sada runbook nebude možné toorun ve službě Azure Automation, ale dá se nainstalovat tak, aby ho upravit.

### <a name="integration-packs-that-cannot-be-converted"></a>Integrační balíčky, které nelze převést
Integrační balíčky, které nebyly vytvořené s OIT nelze převést s hello Integration Pack převaděče. Existují také některé integrační balíčky od společnosti Microsoft, který momentálně nelze převést pomocí tohoto nástroje.  Převedený verzích tyto integrační balíčky byly [poskytnout ke stažení](#system-center-orchestrator-integration-modules) tak, aby mohou být nainstalovány v Azure Automation nebo Service Management Automation.

## <a name="standard-activities-module"></a>Standardní aktivity modulu
Orchestrator obsahuje sadu [standardní aktivity](http://technet.microsoft.com/library/hh403832.aspx) které nejsou součástí integračního balíčku, ale jsou používány kolik sad runbook.  modul standardní aktivity Hello je modul integrace, která zahrnuje ekvivalentní rutiny pro každou z těchto aktivit.  Tento modul integrace ve službě Azure Automation nainstalujte před importem převedený sady runbook, který použít standardní aktivitu.

Kromě toho toosupporting převést sady runbook, hello rutiny v modulu hello standardní aktivity mohou být využívána někdo obeznámeni s Orchestrator toobuild nové sady runbook ve službě Azure Automation.  Při hello funkce všech standardních aktivit, hello lze provést pomocí rutin, může fungovat různě.  Hello rutiny v hello převést standardní aktivity, které budou fungovat modulu hello stejné jako jejich odpovídajících aktivit a používání hello stejnými parametry.  To může pomoct hello existující Orchestrator runbook autora v jejich přechod tooAzure runbooků služeb automatizace.

## <a name="system-center-orchestrator-integration-modules"></a>Moduly integrace produktu System Center Orchestrator
Společnost Microsoft poskytuje [integrační balíčky](http://technet.microsoft.com/library/hh295851.aspx) pro tvorbu součástí produktu System Center tooautomate sady runbook a ostatními produkty.  Některé z těchto integračních balíčků jsou aktuálně založené na OIT ale nemůže být aktuálně převedený toointegration moduly kvůli známé problémy.  [Moduly System Center Orchestrator Integration](https://www.microsoft.com/download/details.aspx?id=49555) zahrnuje převedený verzích tyto integrační balíčky, které lze importovat do Azure Automation a Service Management Automation.  

Ve verzi RTM hello tohoto nástroje aktualizované verze hello integrační balíčky podle OIT, který lze převést pomocí hello Integration Pack převaděč bude publikována.  Pokyny se také poskytnout tooassist v převodu sady runbook pomocí aktivit z integračních balíčků hello není podle OIT.

## <a name="runbook-converter"></a>Převaděč sady Runbook
Hello Runbook převaděč převede sady runbook nástroje Orchestrator do [grafické runbooky](automation-runbook-types.md#graphical-runbooks) který lze importovat do Azure Automation.  

Převaděč Runbook je implementovaný jako modul prostředí PowerShell pomocí rutiny volat **ConvertFrom SCORunbook** který provede převod hello.  Při instalaci nástroje pro hello vytvoří relaci prostředí PowerShell tooa zástupce, který načítá hello rutiny.   

Následující je základní proces tooconvert hello sady runbook Orchestrator a naimportujte ho do Azure Automation.  Hello následující části obsahují další podrobnosti o používání nástroje hello a práci se sadami runbook převedený.

1. Jedna nebo více sad runbook exportujte z nástroje Orchestrator.
2. Získáte moduly integrace pro všechny aktivity v sadě runbook hello.
3. Převeďte runbooky služby Orchestrator hello v exportovaném souboru hello.
4. Zkontrolujte informace v protokolech toovalidate hello převod a toodetermine všechny požadované ruční úlohy.
5. Importování sad runbook převedený do Azure Automation.
6. Vytvořte všechny požadované prostředky ve službě Azure Automation.
7. Upravte runbook hello v Azure Automation toomodify všech požadovaných aktivit.

### <a name="using-runbook-converter"></a>Pomocí převaděče sady Runbook
Hello syntaxe **ConvertFrom SCORunbook** vypadá takto:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string>

* RunbookPath - export toohello cestu souboru obsahujícího sady runbook tooconvert hello.
* Modul – čárkami oddělený seznam obsahující aktivity v sadách runbook hello moduly integrace.
* OutputFolder - cesta toohello složky toocreate převést grafické runbooky.

Následující ukázkový příkaz převede hello sady runbook v souboru exportu, názvem Hello **MyRunbooks.ois_export**.  Tyto sady runbook pomocí hello služby Active Directory a integrační balíčky Data Protection Manager.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks"


### <a name="log-files"></a>Soubory protokolu
Hello převaděč Runbook vytvoří následující soubory protokolů ve hello hello stejné umístění jako hello převést sady runbook.  Pokud hello soubory již existují, budou přepsány informace z poslední převod hello.

| File | Obsah |
|:--- |:--- |
| Převaděč Runbook - Progress.log |Podrobné kroky hello převodu, včetně informací pro každou aktivitu úspěšně převést a upozornění pro každou aktivitu nehodí pro převod. |
| Převaděč Runbook - Summary.log |Souhrn hello poslední převod včetně všechna upozornění a následnou akci úlohy, které je třeba tooperform, jako je například vytváření proměnné vyžaduje se pro runbook hello převést. |

### <a name="exporting-runbooks-from-orchestrator"></a>Export sad runbook z nástroje Orchestrator
Hello Runbook převaděč pracuje s souboru exportu z nástroje Orchestrator, který obsahuje jeden nebo více sad runbook.  V souboru exportu hello vytvoří odpovídající runbook automatizace Azure. pro každou sadu runbook Orchestrator.  

tooexport sady runbook z nástroje Orchestrator, klikněte pravým tlačítkem na název hello hello sady runbook v programu Runbook Designer a vyberte **exportovat**.  tooexport všechny runbooky ve složce, klikněte pravým tlačítkem na název hello hello složky a vyberte **exportovat**.

### <a name="runbook-activities"></a>Aktivity sady Runbook
Hello Runbook převaděč převede každá aktivita v hello Orchestrator runbook tooa odpovídající aktivita ve službě Azure Automation.  Pro ty aktivity, které nelze převést vytvoří se v runbooku hello textem upozornění aktivitu zástupný symbol.  Po importu do Azure Automation runbook hello převést, je potřeba nahradit některé z těchto aktivit platný aktivit, které provádějí hello požadované funkce.

Žádné aktivity Orchestrator v hello [standardní aktivity modulu](#standard-activities-module) bude převeden.  Existují některé standardní Orchestrator aktivity, které nejsou v tomto modulu ale a nejsou převést.  Například **odeslání události platformy** vzhledem k tomu, že událost hello je konkrétní tooOrchestrator neobsahuje ekvivalent Azure Automation.

[Monitorování aktivit](https://technet.microsoft.com/library/hh403827.aspx) se nepřevádějí vzhledem k tomu, že není žádná ekvivalentní toothem ve službě Azure Automation.  Hello výjimky jsou monitorování aktivit v [převést integrační balíčky](#integration-pack-converter) , bude převedený toohello zástupný symbol aktivity.

Všechny aktivity z [převést integračního balíčku](#integration-pack-converter) bude převeden, pokud modul integrace toohello cesta hello poskytnete hello **moduly** parametr.  Integrační balíčky System Center, můžete použít hello [System Center Orchestrator integrační moduly](#system-center-orchestrator-integration-modules).

### <a name="orchestrator-resources"></a>Prostředky nástroje Orchestrator
Hello převaděč Runbook pouze převede sady runbook, není další prostředky nástroje Orchestrator například čítače, proměnné nebo připojení.  Čítače nejsou podporovány ve službě Azure Automation.  Připojení a proměnné jsou podporované, ale je třeba vytvořit ručně.  soubory protokolů Hello bude vás informovat, pokud hello runbook vyžaduje takových prostředků a zadejte odpovídající prostředky, je nutné, toocreate ve službě Azure Automation pro hello převést runbook toooperate správně.

Sada runbook může například použít proměnné toopopulate určitou hodnotu v aktivitě.  Hello převedený runbook bude převést hello aktivity a zadejte proměnný asset s hello stejný název jako proměnné hello Orchestrator ve službě Azure Automation.  To bude zaznamenán v hello **Runbook převaděč - Summary.log** souboru, který je vytvořen po převodu hello.  Budete potřebovat toomanually vytvořit tento variabilní prostředek ve službě Azure Automation před použitím hello runbook.

### <a name="input-parameters"></a>Vstupní parametry
Sady Runbook v nástroji Orchestrator přijmout vstupní parametry s hello **inicializovat Data** aktivity.  Pokud runbook hello převáděné zahrnuje tato aktivita pak [vstupní parametr](automation-graphical-authoring-intro.md#runbook-input-and-output) v hello Azure Automation runbook se vytvoří pro každý parametr v aktivitě hello.  A [řízení pracovního postupu skriptu](automation-graphical-authoring-intro.md#activities) vytvoření aktivity v hello převést runbooku, který načte a vrátí jednotlivé parametry.  Všechny aktivity v hello runbook, které používají vstupní parametr naleznete toohello výstup z této aktivity.

jestli se používá tato strategie Hello skutečnost toobest zrcadlení hello funkce v hello Orchestrator runbook.  Aktivity v nové grafické runbooky by měla odkazovat přímo tooinput parametry s využitím zdroj vstupních dat sady Runbook.

### <a name="invoke-runbook-activity"></a>Aktivitu vyvolat sadu Runbook
Sady Runbook v nástroji Orchestrator spouštět další sady runbook s hello **vyvolání sady Runbook** aktivity. Pokud runbook hello převáděné zahrnuje tato aktivita a hello **čekání na dokončení** je možnost nastavena, tak aktivity sady runbook je pro něj vytvořit v hello převést runbook.  Pokud hello **čekání na dokončení** není nastavena možnost a potom vytvoří aktivita pracovního postupu skriptu, který používá **Start-AzureAutomationRunbook** toostart hello runbook.  Po importu do Azure Automation runbook hello převést, musíte upravit tuto aktivitu hello informací uvedených v hello aktivity.

## <a name="related-articles"></a>Související články
* [System Center 2012 – Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
* [Službě Service Management Automation](https://technet.microsoft.com/library/dn469260.aspx)
* [Hybridní pracovní proces Runbooku](automation-hybrid-runbook-worker.md)
* [Standardní aktivity Orchestrator](http://technet.microsoft.com/library/hh403832.aspx)
* [Stažení System Center Orchestrator Migration Toolkit](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
