---
title: "aaaAdd podmínky a spustit pracovní postupy - Azure Logic Apps | Microsoft Docs"
description: "Řídí, jak spouštět pracovní postupy v Azure Logic Apps přidáním podmíněnou logiku, aktivační události, akce a parametrů."
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e4e24de4-049a-4b3a-a14c-3bf3163287a8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/28/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: 76d5e44590ffa14cf70d7a93b99a241d286d555b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-logic-apps-features"></a>Pomocí funkcí Logic Apps

V [předchozí téma](../logic-apps/logic-apps-create-a-logic-app.md), jste vytvořili svou první aplikaci logiky. toocontrol svou aplikaci logiky pracovního postupu, můžete zadat různé cesty pro vaše toorun logiku aplikace a jak příliš zpracování dat v polích, kolekce a dávek. V pracovním postupu aplikace logiky můžete zahrnout tyto prvky:

* Podmínky a [přepínač příkazy](../logic-apps/logic-apps-switch-case.md) umožní aplikaci logiky spustit různé akce, které jsou založené na tom, zda jsou splněny určité podmínky.

* [Smyčky](../logic-apps/logic-apps-loops-and-scopes.md) umožní aplikaci logiky opakovaně spustit kroky. Například můžete opakovat akce přes pole při použití **for_each –** smyčky. Nebo akce můžete opakovat, dokud je splněna podmínka, pokud použijete **dokud** smyčky.

* [Obory](../logic-apps/logic-apps-loops-and-scopes.md) umožňují můžete seskupit sérii akcí, například tooimplement výjimek.

* [Debatching](../logic-apps/logic-apps-loops-and-scopes.md) umožní aplikaci logiky spuštění samostatné pracovní postupy pro položky v poli při použití hello **SplitOn** příkaz.

Toto téma představuje další koncepty pro vytvoření aplikace logiky:

* Kód zobrazení tooedit existující aplikace logiky
* Možnosti pro spuštění pracovního postupu

## <a name="conditions-run-steps-only-after-meeting-a-condition"></a>Podmínky: Spustit kroky až po splnění podmínku

toohave aplikace logiky spustit kroky jenom v případě, že data splňuje určitá kritéria, můžete přidat podmínku, která porovná data v pracovním postupu hello proti konkrétní pole nebo hodnoty.

Předpokládejme například, že máte aplikaci logiky, který odesílá příliš mnoho e-mailům v příspěvcích na informačního kanálu RSS na web. Můžete přidat, že podmínka, aby aplikace logiky odešle e-mailu jenom v případě, že hello nového příspěvku patří tooa určité kategorie.

1. V hello [portál Azure](https://portal.azure.com), najít a otevřít aplikaci logiky v návrháři aplikace logiky.

2. Umístění pracovního postupu toohello podmínky, které chcete přidáte. 

   Podmínka hello tooadd mezi existující kroky v pracovním postupu logiku aplikace hello, přesuňte ukazatel hello hello šipku místo tooadd hello podmínku. 
   Zvolte hello **znaménko plus** (**+**), pak zvolte **přidat podmínku**. Například:

   ![Přidat podmínku toologic aplikaci](./media/logic-apps-use-logic-app-features/add-condition.png)

   > [!NOTE]
   > Pokud chcete tooadd podmínku na konci hello aktuálního pracovního postupu, přejděte toohello dolní části svou aplikaci logiky a znovu vyberte **+ nový krok**.

3. Nyní Definujte podmínku hello. Zadejte pole zdroje hello chcete tooevaluate, tooperform hello operace a hodnota cíle hello nebo pole. existující tooadd polí tooyour podmínky, vyberte z hello **dynamického obsahu seznamu přidat**.

   Například:

   ![Upravit podmínky v základní režimu](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode.png)

   Tady je hello dokončení podmínku:

   ![Dokončení podmínky](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode-2.png)

   > [!TIP]
   > Vyberte podmínku hello toodefine v kódu **upravit v rozšířeném režimu**. Například:
   > 
   > ![Upravit podmínky v kódu](./media/logic-apps-use-logic-app-features/edit-condition-advanced-mode.png)

4. V části **Pokud Ano** a **ne v případě**, přidejte tooperform kroky hello založené na tom, zda je splněna podmínka hello.

   Například:

   ![Podmínky se Ano a žádné cesty](./media/logic-apps-use-logic-app-features/condition-yes-no-path.png)

   > [!TIP]
   > Přetáhněte stávající akce do hello **Pokud Ano** a **ne v případě** cesty.

5. Když jste hotovi, uložte svou aplikaci logiky.

Pouze v případě, že hello příspěvcích splňují vaše podmínky se teď se e-mailů.

## <a name="repeat-actions-over-a-list-with-foreach"></a>Opakování akce pro seznam pomocí příkazu forEach

smyčka typu forEach Hello určuje přes pole toorepeat akce. Pokud není pole, tok hello se nezdaří. Například pokud máte action1, který jako výstup pole zprávy a chcete toosend každou zprávu, můžete zahrnout tento příkaz forEach ve vlastnostech hello vaše akce:`forEach : "@action('action1').outputs.messages"`

## <a name="edit-hello-code-definition-for-a-logic-app"></a>Upravte definici hello kódu pro aplikace logiky

I když máte hello návrhář aplikace logiky, můžete upravit přímo hello kód, který definuje logiku aplikace.

1. Na panelu příkazů hello, zvolte **Code zobrazení**.

    Úplné editor otevře a zobrazí hello definice, které jste upravili.

    ![Zobrazení kódu](media/logic-apps-use-logic-app-features/codeview.png)

    V hello textový editor, můžete zkopírovat a vložit libovolného počtu akcí v rámci hello stejné aplikace logiky nebo mezi aplikace logiky. 
    Můžete také snadno přidávat nebo odebírat celé oddíly z definice hello a definice můžete také sdílet s ostatními.

2. Zvolte provedené úpravy toosave **Uložit**.

## <a name="parameters"></a>Parametry

Některé funkce Logic Apps jsou k dispozici pouze v zobrazení kódu, například parametry. Parametry umožňují snadno tooreuse hodnoty v celé aplikaci logiky. Například pokud máte e-mailovou adresu, kterou chcete použít v několik akcí, měli byste tuto e-mailovou adresu jako parametr.

Parametry jsou vhodné pro stahování na hodnoty jsou pravděpodobně toochange mnoho. Jsou užitečné zejména v případě, že potřebujete toooverride parametry v různých prostředích. jak zjistit, na základě prostředí, parametry toooverride toolearn [vytváření definic aplikace logiky](../logic-apps/logic-apps-author-definitions.md) a [dokumentace k REST API](https://docs.microsoft.com/rest/api/logic).

Tento příklad ukazuje, jak tooupdate existující aplikace logiky tak, aby parametry lze použít pro termín dotazu, hello.

1. V zobrazení kódu, najde hello `parameters : {}` objektu a přidejte `currentFeedUrl` objektu:

        "currentFeedUrl" : {
            "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
        }

2. Přejděte toohello `When_a_feed-item_is_published` akce, najít hello `queries` části a nahradit hodnotu dotazu hello:`"feedUrl": "#@{parameters('currentFeedUrl')}"` 

    toojoin dva nebo víc řetězců, můžete použít také hello `concat` funkce. 
    Například `"@concat('#',parameters('currentFeedUrl'))"` funguje hello stejné jako hello výše.

3.  Až budete hotoví, zvolte **Uložit**. 

    Teď můžete změnit hello webu kanálu RSS předáním jinou adresu URL prostřednictvím hello `currentFeedURL` objektu.

Další informace o [jak definice aplikace logiky tooauthor](../logic-apps/logic-apps-author-definitions.md).

## <a name="start-logic-app-workflows"></a>Spusťte aplikaci logiky pracovních postupů

Existuje několik možností pro spuštění pracovního postupu hello definované v aplikaci logiky. Pracovního postupu na vyžádání můžete vždy spustit v hello [portál Azure].

### <a name="recurrence-triggers"></a>Aktivuje opakování

Opakování aktivační událost se spustí na časový interval, který zadáte. Pokud je aktivační událost hello podmíněnou logiku, aktivační události hello Určuje, zda hello pracovního postupu musí toorun. Aktivační událost označuje hello pracovní postup spuštěn vrácením `200` stavový kód. Když pracovní postup hello nepotřebuje toorun, vrátí hello aktivační události `202` stavový kód.

### <a name="callback-using-rest-apis"></a>Zpětné volání pomocí rozhraní REST API

toostart pracovní postup služby můžete volat koncový bod logiku aplikace. Vyberte tento druh logiku aplikace na vyžádání, toostart **spustit nyní** na panelu příkazů hello. V tématu [spustit pracovní postupy voláním logiku aplikace koncové body jako triggery](../logic-apps/logic-apps-http-endpoint.md). 

<!-- Shared links -->
[portál Azure]: https://portal.azure.com

## <a name="next-steps"></a>Další kroky

* [Příkazech Switch](../logic-apps/logic-apps-switch-case.md) 
* [Smyčky, obory a rozdělení dávek](../logic-apps/logic-apps-loops-and-scopes.md)
* [Vytváření definic aplikací logiky](../logic-apps/logic-apps-author-definitions.md)