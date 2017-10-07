---
title: chyby aaaDiagnose - Azure Logic Apps | Microsoft Docs
description: "Běžné způsoby toounderstand, kde dochází k selhání aplikace logiky"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 46d318625820034c95e6df3a71ab84c58f076dd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-logic-app-failures"></a>Diagnostikování selhání aplikace logiky
Pokud máte selhání nebo problémy s logic apps, existuje několik přístupů vám může pomoct lépe pochopit, kde hello selhání pocházejících z.  

## <a name="azure-portal-tools"></a>Nástroje portálu Azure
Hello portál Azure poskytuje mnoho toodiagnose nástroje pro každou aplikaci logiky při každém kroku.

### <a name="trigger-history"></a>Aktivační událost historie

Každá aplikace logiky má nejméně jedna aktivační událost. Pokud si všimnete, že aplikace se ohlásí, nejprve vyhledává v historii hello aktivační událost pro další informace. Historie aktivační událost hello v hlavním okně app'ss hello logiky můžete přistupovat.

![Vyhledání hello aktivační událost historie][1]

Historie Hello aktivační události obsahuje seznam všech pokusů o aktivační události, které aplikace logiky udělat. Můžete kliknout na každý pokus o toodrill aktivační události do hello podrobnosti, konkrétně, všechny vstupů nebo výstupů, které hello pokus o aktivační události vygenerovat. Pokud zjistíte, se nezdařila aktivace, vyberte pokus hello aktivační události a zvolte hello **výstupy** odkaz tooreview všechny vygenerované chybové zprávy, například pro přihlašovací údaje serveru FTP, které nejsou platné.

Hello různé stavy, které mohou být zobrazeny jsou:

* **Vynecháno**. koncový bod Hello byl dotazů toocheck pro data a obdržel odpověď, nebyla dostupná žádná data.
* **Úspěšné**. aktivační událost Hello obdržel odpověď, nebyla dostupná data. Tento stav může být výsledkem ruční aktivační událost, aktivační událost opakování nebo cyklického dotazování aktivační událost. Tento stav je obvykle přiložena hello **Fired** stav, ale možná, pokud máte v zobrazení kódu, která nebyla splněná podmínka nebo SplitOn příkaz.
* **Se nezdařilo**. Byla generována chyba.

#### <a name="start-a-trigger-manually"></a>Ruční spuštění aktivační události

Pokud chcete pro aktivační procedury k dispozici okamžitě bez čekání na další opakování hello hello logiku aplikace toocheck, klikněte na tlačítko **vyberte aktivační událost** v hlavním okně tooforce hello kontrolu. Například kliknutím na tento odkaz se aktivační událostí Dropbox způsobí, že hello pracovního postupu tooimmediately dotazování Dropbox pro nové soubory.

### <a name="run-history"></a>Historie spouštění

Všechny aktivní aktivační události výsledkem spustit. Informace o běhu můžete přistupovat z hlavní okno hello, který obsahuje mnoho podrobnosti, které vám může pomoct pochopit, co se stalo během pracovního postupu hello.

![Vyhledání hello historie spouštění][2]

Spustit zobrazí jedno z hello následující stavy:

* **Úspěšné**. Všechny akce bylo úspěšné. Pokud došlo k selhání, že selhání zajišťoval akci, která došlo k chybě později v pracovním postupu hello. To znamená selhání hello zajišťoval akci, která byla nastavena toorun po selhání akce.
* **Se nezdařilo**. Nejméně jedna akce měl selhání, který nebyl zpracován akcí později v pracovním postupu hello.
* **Zrušena**. pracovní postup Hello byl spuštěn, ale přijal žádost o zrušení.
* **Spuštění**. pracovní postup Hello je právě spuštěna. Tento stav může dojít k omezenému pracovních postupů, nebo z důvodu hello aktuální ceny plánu. Podrobnosti najdete v tématu [omezení akce na stránce s cenami hello](https://azure.microsoft.com/pricing/details/app-service/plans/). Konfigurace diagnostiky (hello grafů, které se zobrazují v části historie spouštění hello) také může poskytnout informace o všech omezení událostí, které dojít.

Když se díváte historie spouštění, můžete zobrazit další podrobnosti Další podrobnosti.  

#### <a name="trigger-outputs"></a>Výstupy aktivační události

Aktivační událost výstupy zobrazit hello data, která pochází hello aktivační události. Tyto výstupů vám pomohou určit, zda všechny vlastnosti vrátila podle očekávání.

> [!NOTE]
> Pokud se zobrazí veškerý obsah, který neznáte, zjistěte, jak Azure Logic Apps [zpracovává jiné typy obsahu](../logic-apps/logic-apps-content-type.md).
> 

![Příklady výstup aktivační události][3]

#### <a name="action-inputs-and-outputs"></a>Akce vstupy a výstupy

Můžete rozbalit hello vstupy a výstupy, které přijaly akce. Tato data jsou užitečné pro pochopení hello velikost a tvar hello výstupy a také pro hledání chybové zprávy, které byly vytvořeny.

![Akce vstupy a výstupy][4]

## <a name="debug-workflow-runtime"></a>Ladění modulu runtime pracovního postupu

Společně s hello vstupy, výstupy a aktivuje spuštění monitorování, můžete přidat tooa pracovního postupu některé kroky, které pomáhají s laděním. 
[RequestBin](http://requestb.in) je výkonný nástroj, který můžete přidat jako krok v pracovním postupu. Pomocí RequestBin můžete nastavit protokolu HTTP žádosti inspector toodetermine hello přesnou velikost, tvar a formát požadavku HTTP. Můžete vytvořit RequestBin a vložte adresu URL hello v aplikaci logiky akce HTTP POST s obsah textu, které chcete tootest, například, výraz nebo jiné krok výstup. Po spuštění aplikace logiky hello můžete obnovit vaše RequestBin toosee jak hello požadavek byl vytvořen při vygenerování ze stroje hello Logic Apps.

<!-- image references -->
[1]: ./media/logic-apps-diagnosing-failures/triggerhistory.png
[2]: ./media/logic-apps-diagnosing-failures/runhistory.png
[3]: ./media/logic-apps-diagnosing-failures/triggeroutputslink.png
[4]: ./media/logic-apps-diagnosing-failures/actionoutputs.png
