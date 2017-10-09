---
title: "aaaAutomate Azure Log Analytics zpracovává s Flow Microsoft"
description: "Zjistěte, jak lze pomocí Microsoft Flow tooquickly automatizovat opakované procesy pomocí konektoru Azure Log Analytics hello."
services: log-analytics
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: log-analytics
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: bwren
ms.openlocfilehash: 52026df968682842cc9f8d55f6f9875c5f9c10b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-log-analytics-processes-with-hello-connector-for-microsoft-flow"></a>Automatizovat procesy analýzy protokolů hello konektor pro Flow Microsoft
[Microsoft Flow](https://ms.flow.microsoft.com) vám umožní toocreate automatizované pracovní postupy pomocí stovky akce pro celou řadu služeb. Výstup z jednu akci je možné použít jako vstupní tooanother, což vám toocreate integrace mezi různými službami.  konektor Hello analýzy protokolů Azure pro Microsoft Flow povolit toobuild pracovní postupy, které zahrnují data načtená při prohledávání protokolu v analýzy protokolů.

Můžete například použít Microsoft Flow toouse analýzy protokolů data e-mailem oznámení z Office 365, vytvoření chyby ve Visual Studio Team Services nebo příspěvek ve Slack.  Při jednoduchého plánu, nebo některá z akcí v připojené služby, jako je při doručení e-mailu nebo tweet, můžete aktivovat pracovní postup.  


> [!NOTE]
> konektor Hello analýzy protokolů Azure pro Microsoft Flow vyžaduje prostoru toobe upgradovat toohello nové analýzy protokolů dotazu jazyka. Můžete získat další informace o nový jazyk hello a získat hello postup tooupgrade pracovního prostoru v [Upgrade vyhledávání protokolu toonew pracovní prostor analýzy protokolů Azure](log-analytics-log-search-upgrade.md).  

Hello kurzu v tomto článku se dozvíte, jak toocreate toku, který automaticky odesílá hello výsledky vyhledávání protokolu analýzy protokolů e-mailem, pouze příklad, jak můžete použít analýzy protokolů v Flow společnosti Microsoft. 


## <a name="step-1-create-a-flow"></a>Krok 1: Vytvoření toku
1. Přihlaste se příliš[Microsoft Flow](http://flow.microsoft.com)a vyberte **Moje toků**.
2. Klikněte na tlačítko **+ vytvořit z prázdné**.

## <a name="step-2-create-a-trigger-for-your-flow"></a>Krok 2: Vytvoření aktivační událost pro vaše tok
1. Klikněte na tlačítko **vyhledávání stovky konektory a aktivační události**.
2. Typ **plán** hello vyhledávacího pole.
3. Vyberte **plán**a potom vyberte **plán - opakování**.
4. V hello **frekvence** zaškrtněte **den** a v hello **Interval** zadejte **1**.<br><br>![Dialogové okno Microsoft Flow aktivační události](media/log-analytics-flow-tutorial/flow01.png)


## <a name="step-3-add-a-log-analytics-action"></a>Krok 3: Přidání akce analýzy protokolů
1. Klikněte na tlačítko **+ nový krok**a potom klikněte na **přidat akci**.
2. Vyhledejte **protokolu analýzy**.
3. Klikněte na tlačítko **Azure Log Analytics – spuštění dotazu a vizualizace výsledků**.<br><br>![Spusťte okno dotazu analýzy protokolů](media/log-analytics-flow-tutorial/flow02.png)

## <a name="step-4-configure-hello-log-analytics-action"></a>Krok 4: Konfigurace analýzy protokolů akce hello

1. Zadejte podrobnosti hello vašeho pracovního prostoru, včetně hello ID předplatné, skupinu prostředků a název pracovního prostoru.
2. Přidejte následující analýzy protokolů dotazu toohello hello **dotazu** okno.  Toto je ukázkový dotaz a můžete nahradit všechny jiné, vrací data.
```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computerindow. 
```

2. Vyberte **tabulky HTML** pro hello **typ grafu**.<br><br>![Akce analýzy protokolů](media/log-analytics-flow-tutorial/flow03.png)

## <a name="step-5-configure-hello-flow-toosend-email"></a>Krok 5: Konfigurace hello toku toosend e-mailu

1. Klikněte na tlačítko **nový krok**a potom klikněte na **+ přidat akci**.
2. Vyhledejte **Office 365 Outlook**.
3. Klikněte na tlačítko **Office 365 Outlook – e-mailu**.<br><br>![Okno Výběr Outlook Office 365](media/log-analytics-flow-tutorial/flow04.png)

4. Zadejte hello e-mailová adresa příjemce v hello **k** okno a předmět e-mailu hello v **subjektu**.
5. Klikněte kamkoli do hello **textu** pole.  A **dynamický obsah** otevře se okno s hodnotami z předchozí akce.  
6. Vyberte **textu**.  Toto je hello výsledky dotazu hello v hello analýzy protokolů akce.
6. Klikněte na tlačítko **zobrazit rozšířené možnosti**.
7. V hello **je HTML** vyberte **Ano**.<br><br>![Okno Konfigurace e-mailu Office 365](media/log-analytics-flow-tutorial/flow05.png)

## <a name="step-6-save-and-test-your-flow"></a>Krok 6: Uložit a testování vaší toku
1. V hello **toku název** pole, přidejte název vaší toku a pak klikněte na tlačítko **vytvořit toku**.<br><br>![Uložit toku](media/log-analytics-flow-tutorial/flow06.png)
2. tok Hello je nyní vytvořen a spustí za den, což je hello plán, který jste zadali. 
3. tok hello tooimmediately testu, klikněte na tlačítko **spustit nyní** a potom **spustit toku**.<br><br>![Spustit toku](media/log-analytics-flow-tutorial/flow07.png)
3. Po dokončení hello toku zkontrolujte e-mailu hello hello příjemce, který jste zadali.  Byste měli obdržet e-mail se podobné toohello následující text:<br><br>![Ukázkového e-mailu](media/log-analytics-flow-tutorial/flow08.png)


## <a name="next-steps"></a>Další kroky

- Další informace o [přihlásit analýzy protokolů hledání](log-analytics-log-search-new.md).
- Další informace o [Microsoft Flow](https://ms.flow.microsoft.com).



