---
title: "příkaz aaaSwitch pro různé akce v Azure Logic Apps | Microsoft Docs"
description: "Zvolte tooperform různé akce v logiku aplikace založené na výrazu hodnoty pomocí příkazu switch"
services: logic-apps
keywords: "Switch – příkaz"
author: derek1ee
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: LADocs; deli
ms.openlocfilehash: 09ed7e4a752003aba157e9156bf4dc89ef86f5ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="perform-different-actions-in-logic-apps-with-a-switch-statement"></a>Provádění různých akcí v aplikacích logiky s příkaz switch

Při vytváření pracovního postupu, máte často tootake různé akce na základě hodnoty hello objektu nebo výraz. Můžete například vaše toobehave aplikace logiky různě v závislosti na hello kód stavu požadavku HTTP, nebo možnosti vybrané v e-mailu.

Příkaz tooimplement přepínač můžete použít tyto scénáře. Aplikace logiky můžete vyhodnotit token nebo výraz a zvolte hello případu se hello stejnou hodnotu tooexecute hello zadané akce. Příkaz switch hello by měl odpovídat pouze jeden případ.

> [!TIP]
> Podobně jako všechny programovacích jazyků příkazech switch podporují pouze operátory rovnosti. Pokud potřebujete další relační operátory, jako je třeba "větší než", použijte příkaz podmínky.
> chování při spuštění deterministickou tooensure, případů musí obsahovat jedinečný a statické hodnotu místo dynamické tokeny nebo výraz.

## <a name="prerequisites"></a>Požadavky

- Aktivní předplatné Azure. Pokud nemáte předplatné Azure aktivní [vytvořit bezplatný účet](https://azure.microsoft.com/free/), nebo zkuste [volné aplikace logiky pro](https://tryappservice.azure.com/).
- [Základní znalosti o aplikace logiky](logic-apps-what-are-logic-apps.md)

## <a name="add-a-switch-statement-tooyour-workflow"></a>Přidat pracovní postup tooyour přepínače – příkaz

Jak funguje příkazu switch tooshow, tento příklad vytvoří aplikace logiky, zda soubory monitorování nahrán tooDropbox. Když jsou odeslány hello nové soubory, aplikace logiky hello odešle e-mailu tooan schvalovatel, kterého se tedy rozhodne zda tootransfer tooSharePoint těchto souborů. aplikace Hello používá příkaz přepínač, který provádí různé akce na základě hello hodnoty, které hello schvalovatel vybere.

1. Vytvoření aplikace logiky a vyberte této aktivační události: **Dropboxu - Pokud dojde k vytvoření souboru**.

   ![Použití Dropboxu - Pokud dojde k vytvoření souboru aktivační události](./media/logic-apps-switch-case/dropbox-trigger.jpg)

2. V části hello aktivační událost, přidejte tuto akci: **Outlook.com - odesílání schválení e-mailu**

   > [!TIP]
   > Aplikace logiky také podporují odesílání e-mailu scénáře schválení z účtu Office 365 Outlook.

   - Pokud nemáte existující připojení, se zobrazí výzva toocreate jeden.
   - Vyplňte hello požadované pole. Například v položce **k**, zadejte e-mailové adresy hello hello schvalovatel e-mailem.
   - V části **možností uživatele**, zadejte `Approve, Reject`.

   ![Konfigurace připojení](./media/logic-apps-switch-case/send-approval-email-action.jpg)

3. Přidejte příkaz, přepínač.

   - Vyberte **+ nový krok** > **... Další** > **přidání přepínače případu**. 
   - Teď chceme tooselect hello akce tooperform podle hello `SelectedOptions` výstup z hello *odeslání e-mailu schválení* akce. 
   Toto pole můžete najít v hello **přidávat dynamický obsah** selektor.
   - Použití *případ 1* toohandle hello schvalovatel výběrem `Approve`.
     - Pokud, zkopírujte hello původní soubor tooSharePoint Online s hello [ **SharePoint Online – vytvoření souboru** akce](../connectors/connectors-create-api-sharepointonline.md).
     - Přidejte další akci hello případu toonotify uživatelé, že nový soubor je k dispozici na webu služby SharePoint.
   - Přidejte jiný případu toohandle, pokud uživatel vybere `Reject`.
     - Pokud zamítnutí, odesílat e-mailové oznámení, informuje o další schvalovatele, že soubor hello je odmítnut a není nutná žádná další akce.
   - `SelectedOptions`poskytuje pouze dvě možnosti, takže jsme můžete nechat hello **výchozí** případ prázdný.

   ![Switch – příkaz](./media/logic-apps-switch-case/switch.jpg)

   > [!NOTE]
   > Příkaz switch musí mít alespoň jeden případ v případě výchozí toohello přidání.

4. Po příkazu switch hello, odstranit původní soubor nahrát tooDropbox hello přidáním tuto akci: **Dropbox – odstranit soubor**

5. Uložte svou aplikaci logiky. Testování aplikace s tím, že nahrajete soubor tooDropbox. Měli byste obdržet e-mailu schválení za chvíli. Vyberte možnost a sledovat chování hello.

   > [!TIP]
   > Podívat, jak příliš[sledování funkce logic apps](logic-apps-monitor-your-logic-apps.md).

## <a name="understand-hello-code-behind-switch-statements"></a>Pochopení kódu hello příkazech switch

Teď, když jste úspěšně vytvořili aplikaci logiky, pomocí příkazu switch, podíváme se na definice kódu hello za příkaz switch hello.

```json
"Switch": {
    "type": "Switch",
    "expression": "@body('Send_approval_email')?['SelectedOption']",
    "cases": {
        "Case 1" : {
            "case" : "Approved",
            "actions" : {}
        },
        "Case 2" : {
            "case" : "Rejected",
            "actions" : {}
        }
    },
    "default": {
        "actions": {}
    },
    "runAfter": {
        "Send_approval_email": [
            "Succeeded"
        ]
    }
}
```

* `"Switch"`je název hello tohoto příkazu hello přepínače, které můžete přejmenovat čitelnější. 
* `"type": "Switch"`Označuje, že akce hello je příkaz switch. 
* `"expression"`vyhodnotí pro každý případ deklarovaný později v definici hello je hello schvalovatel vybranou možnost v tomto příkladu. 
* `"cases"`může obsahovat libovolný počet případů. Pro každý případ `"Case *"` je výchozí název hello hello případu, kdy můžete přejmenovat čitelnější. 
`"case"`Určuje hello případu štítku, kterou hello používá přepínač výraz pro porovnání a musí být jedinečný a konstantní hodnotu. Pokud žádný hello případů neodpovídá hello přepínač výrazu akce v rámci `"default"` provedení.

## <a name="get-help"></a>Podpora

tooask otázky, odpovědi na dotazy a zjistit, jaké jiných uživatelů Azure Logic Apps dělají, navštivte hello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp zlepšení Azure Logic Apps a konektory, hlasovat o nebo odeslání nápadů v hello [web pro zasílání názorů uživatele Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak příliš[přidat podmínky](logic-apps-use-logic-app-features.md)
- Další informace o [chyb a výjimek](logic-apps-exception-handling.md)
- Prozkoumat více [možnosti jazyka pracovního postupu](logic-apps-author-definitions.md)