---
title: "zpracování zpráv aaaBatch jako skupiny nebo kolekce - Azure Logic Apps | Microsoft Docs"
description: "Odesílat a přijímat zprávy pro dávkové zpracování v aplikacích logiky"
keywords: "dávkové zpracování balíku"
author: jonfancey
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/7/2017
ms.author: LADocs; estfan; jonfan
ms.openlocfilehash: 2603db71ee0659d5b6bf5ce3d32f1b0d13c34194
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-receive-and-batch-process-messages-in-logic-apps"></a>Odesílání, příjem a zpracování zpráv v aplikacích logiky služby batch

zprávy tooprocess společně ve skupinách, může odesílat data položky nebo zprávy, tooa *batch*a pak tyto položky zpracovávat jako dávku. Tento přístup je užitečné, když chcete toomake zda datové položky jsou seskupené v určitým způsobem a společném zpracování. 

Můžete vytvořit logiku aplikace, které zobrazí položky jako dávce pomocí hello **Batch** aktivační události. Potom můžete vytvořit logiku aplikace, které odesílání položek tooa batch pomocí hello **Batch** akce.

Toto téma ukazuje, jak můžete vytvořit dávkování řešení provedením těchto úkolů: 

* [Vytvoření aplikace logiky, která přijímá a shromažďuje položky jako dávku](#batch-receiver). Tato aplikace logiky "batch příjemce" Určuje hello batch název a verzi kritéria toomeet předtím, než aplikace logiky příjemce hello uvolní a zpracovává položky. 

* [Vytvoření aplikace logiky, který odesílá položky tooa batch](#batch-sender). Tato aplikace logiky "batch sender" Určuje, kde toosend položek, které musí být existující aplikace logiky příjemce batch. Můžete také zadat jedinečný klíč, jako je číslo zákazníka, příliš "oddílu" nebo rozdělte, hello cíl batch do podmnožin na základě tohoto klíče. Tímto způsobem, všechny položky s tímto klíčem sběru a zpracování společně. 

## <a name="requirements"></a>Požadavky

toofollow v tomto příkladu budete potřebovat tyto položky:

* Předplatné Azure. Pokud předplatné nemáte, můžete [začít s bezplatným účtem Azure](https://azure.microsoft.com/free/). Jinak si můžete [zaregistrovat předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).

* Základní znalosti o [jak toocreate logic apps](../logic-apps/logic-apps-create-a-logic-app.md) 

* E-mailový účet s žádným [e-mailu zprostředkovatele nepodporuje Azure Logic Apps](../connectors/apis-list.md)

<a name="batch-receiver"></a>

## <a name="create-logic-apps-that-receive-messages-as-a-batch"></a>Vytvoření aplikace logiky, které přijímají zprávy jako dávky

Před odesláním zprávy tooa batch, je nutné nejprve vytvořit aplikace logiky "batch příjemce" s hello **Batch** aktivační události. Tímto způsobem tuto aplikaci logiky příjemce můžete vybrat, když vytvoříte aplikaci logiky odesílatele hello. Pro hello příjemce zadejte název dávkového hello, verze kritéria a další nastavení. 

Aplikace logiky odesílatele potřebovat vědět, kde toosend položky, zatímco aplikace logiky příjemce nepotřebujete tooknow cokoli o odesílatelé hello.

1. V hello [portál Azure](https://portal.azure.com), vytvoření aplikace logiky s tímto názvem: "BatchReceiver" 

2. V návrháři aplikace logiky, přidejte hello **Batch** aktivační události, které spustí pracovní postup aplikace logiky. Hello vyhledávacího pole zadejte "batch" jako filtr. Vyberte této aktivační události: **Batch – zprávy dávkových**

   ![Přidat aktivační událost Batch](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. Zadejte název pro dávku hello a zadejte kritéria pro uvolnění hello batch, například:

   * **Název služby batch**: hello název používaný tooidentify hello batch, což je "TestBatch" v tomto příkladu.
   * **Počet zpráv**: hello počet zpráv toohold jako dávku před uvolněním pro zpracování, což je "5" v tomto příkladu.

   ![Zadejte podrobnosti dávky aktivační události](./media/logic-apps-batch-process-send-receive-messages/receive-batch-trigger-details.png)

4. Přidáte akci, která se pošle e-mailu, když se aktivuje hello batch aktivační události. Každé dávky hello čas má pět položek, aplikace logiky hello odešle e-mail.

   1. V části hello batch aktivační událost, zvolte **+ nový krok** > **přidat akci**.

   2. Hello vyhledávacího pole zadejte "e-mailu" jako filtr.
   Založená na zprostředkovateli e-mailu, vyberte konektor e-mailu.
   
      Například pokud máte pracovní nebo školní účet, vyberte konektor Office 365 Outlook hello. 
      Pokud máte účet Gmail, vyberte konektor z Gmailu hello.

   3. Vyberte tuto akci pro vaše konektor:  **{*poskytovatele e-mailu*}-odeslat e-mailu **

      Například:

      ![Vyberte možnost odeslat e-mail panel. pro váš poskytovatel e-mailu](./media/logic-apps-batch-process-send-receive-messages/add-send-email-action.png)

5. Pokud jste nevytvořili dříve připojení pro poskytovatele e-mailu, zadejte svoje přihlašovací údaje e-mailu pro ověřování při zobrazení výzvy. Další informace o [ověřování přihlašovacích údajů e-mailu](../logic-apps/logic-apps-create-a-logic-app.md).

6. Nastavte vlastnosti hello hello akci, kterou jste právě přidali.

   * V hello **k** zadejte hello příjemce e-mailovou adresu. 
   Pro účely testování můžete použít vlastní e-mailovou adresu.

   * V hello **subjektu** pole, když hello **dynamický obsah** se zobrazí seznam, vyberte hello **název oddílu** pole.

     ![Ze seznamu "Dynamický obsah" hello vyberte "oddíl název"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     V další části můžete zadat klíč oddílu jedinečný, vydělí hello cíl batch do logické nastaví toowhere mohou zasílat zprávy. 
     Každá sada má jedinečné číslo, které je generován aplikace logiky hello odesílatele. 
     Tato funkce umožňuje používat jeden batch s více podmnožin a definovat každý podmnožina s hello název, který zadáte.

   * V hello **textu** pole, když hello **dynamický obsah** se zobrazí seznam, vyberte hello **Id zprávy** pole.

     !["Text" Vyberte možnost "Id zprávy"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

     Protože hello vstup pro hello odesílání e-mailu akce je pole, Návrhář hello automaticky přidá **pro každou** smyčky kolem hello **e-mailovou zprávu** akce. 
     Tato smyčka provede hello vnitřní akce na každou položku v dávce hello. 
     Ano s hello batch aktivační událost sadu toofive položek, získáte pět e-mailů, které aktivuje jednotlivé aktivační události hello čas.

7.  Vytvoření aplikace logiky příjemce batch, uložte svou aplikaci logiky.

    ![Uložení aplikace logiky](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

<a name="batch-sender"></a>

## <a name="create-logic-apps-that-send-messages-tooa-batch"></a>Vytvoření aplikace logiky, které odesílají zprávy tooa batch

Teď vytvořte jeden nebo více aplikací logiky, které odeslat dávku toohello položky definované aplikace logiky příjemce hello. Pro odesílatele hello zadejte aplikaci logiky hello příjemce a název dávkového, obsah zprávy a další nastavení. Volitelně můžete zadat jedinečný oddílu klíče toodivide hello dávky do podmnožin toocollect položky s tímto klíčem.

Aplikace logiky odesílatele potřebovat vědět, kde toosend položky, zatímco aplikace logiky příjemce nepotřebujete tooknow cokoli o odesílatelé hello.

1. Vytvořit jinou aplikaci logiky s tímto názvem: "BatchSender"

   1. Hello vyhledávacího pole zadejte "recurrence" jako filtr. 
   Vyberte této aktivační události: **plán - opakování**

      ![Přidat aktivační událost hello "Plán-Recurrence"](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-receiver.png)

   2. Nastavte četnost hello a aplikace logiky interval toorun hello odesílatele každou minutu.

      ![Nastavení frekvence a intervalu opakování aktivační události](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-receiver-details.png)

2. Přidáte nový krok pro odesílání zpráv tooa dávky.

   1. V části aktivační událost hello opakování, zvolte **+ nový krok** > **přidat akci**.

   2. Hello vyhledávacího pole zadejte "batch" jako filtr. 

   3. Vyberte tuto akci: **odesílat zprávy toobatch – zvolte Logic Apps pracovního postupu se aktivační událostí batch**

      ![Vyberte "Odesílat zprávy toobatch"](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   4. Nyní vyberte svou aplikaci logiky "BatchReceiver", který jste dříve vytvořili, které nyní se zobrazí jako akce.

      ![Vyberte aplikaci logiky "batch příjemce"](./media/logic-apps-batch-process-send-receive-messages/send-batch-select-batch-receiver.png)

      > [!NOTE]
      > seznam Hello také ukazuje všechny ostatní aplikace logiky, které mají batch aktivační události.

3. Nastavit vlastnosti batch hello.

   * **Název služby batch**: název dávkového hello definované hello příjemce logiku aplikace, která je v tomto příkladu "TestBatch" a byl ověřen za běhu.

     > [!IMPORTANT]
     > Zajistěte, aby hello batch název, který musí odpovídat názvu hello dávky, která je zadána aplikace logiky příjemce hello neměnit.
     > Změna název dávkového hello způsobí, že odesílatel hello toofail aplikace logiky.

   * **Obsah zprávy**: hello obsah zprávy, které chcete toosend. 
   V tomto příkladu přidejte tento výraz vloží hello aktuální datum a čas do obsahu, odešlete toohello batch uvítací zprávu:

     1. Když hello **dynamický obsah** seznamu se zobrazí, zvolte **výraz**. 
     2. Zadejte výraz hello **utcnow()**a zvolte **OK**. 

        ![V "Zpráva obsah" Vyberte "Výraz". Zadejte "utcnow()".](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details.png)

4. Nyní nastavte oddíl pro dávku hello. V akci "BatchReceiver" hello, zvolte **zobrazit rozšířené možnosti**.

   * **Název oddílu**: klíče toouse pro dělení hello cíl dávky volitelné jedinečný oddílu. V tomto příkladu přidáte výraz, který generuje náhodné číslo v intervalu jeden a pět.
   
     1. Když hello **dynamický obsah** seznamu se zobrazí, zvolte **výraz**.
     2. Zadejte tento výraz: **rand(1,6)**

        ![Nastavení pro dávku cílový oddíl](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-partition-advanced-options.png)

        To **rand –** funkce generuje v rozmezí jednu až pět. 
        Proto jsou dělení tuto dávku do pěti číslem oddílů, které dynamicky nastaví tento výraz.

   * **Id zprávy**: identifikátor volitelné zpráv a je vytvářenému identifikátoru GUID, pokud je prázdný. 
   V tomto příkladu ponechte toto pole prázdné.

5. Uložte svou aplikaci logiky. Aplikace logiky odesílatele nyní vypadá podobně jako příklad toothis:

   ![Uložte svou aplikaci logiky odesílatele](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details-finished.png)

## <a name="test-your-logic-apps"></a>Testování aplikace logiky

tootest vaše dávkování řešení, nechte aplikace logiky spuštěná pro několik minut. Později, brzy začnete získání e-mailů v pěti skupiny, všechny s hello stejný klíč oddílu.

Aplikace logiky BatchSender spouští každou minutu, vygeneruje náhodné číslo v intervalu jeden a 5 a používá toto generované číslo jako klíč oddílu hello pro dávku cíl hello, které jsou odesílány zprávy. Pokaždé, když hello batch má pět položek s hello stejným klíčem oddílu aplikace logiky BatchReceiver aktivuje a odešle e-mailu pro každou zprávu.

> [!IMPORTANT]
> Po dokončení testování, zkontrolujte, zda zakázat hello BatchSender logiku aplikace toostop zasílání zpráv a předcházeli přetížení vaší doručené poště.

## <a name="next-steps"></a>Další kroky

* [Sestavení na definice aplikace logiky pomocí JSON](../logic-apps/logic-apps-author-definitions.md)
* [Sestavení bez serveru aplikace v sadě Visual Studio s funkcemi a Azure Logic Apps](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Zpracování výjimek a protokolování chyb pro logic apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
