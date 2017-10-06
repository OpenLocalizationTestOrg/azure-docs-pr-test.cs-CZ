---
title: "konektor databáze Oracle hello aaaAdd v Azure Logic Apps | Microsoft Docs"
description: "Použití konektoru databáze Oracle hello v aplikaci logiky"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: mandia; ladocs
ms.openlocfilehash: 8a802a6c4782e210ff71848614152cb46ba5d651
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-oracle-database-connector"></a>Začínáme s konektorem databáze Oracle hello

Pomocí konektoru hello databáze Oracle, vytvoření organizační pracovní postupy, které pomocí data v existující databázi. Tento konektor se může připojit tooan instalaci místní databázi Oracle nebo virtuální počítač Azure s databáze Oracle. Tento konektor můžete:

* Vytvořte pracovní postup přidávání nové databáze zákazníci tooa zákazníka nebo aktualizaci pořadí, v databázi objednávky.
* Pomocí akce tooget řádek dat, vložit nový řádek a ani odstranit. Například v aplikaci Dynamics CRM Online (aktivační událost) je vytvořen záznam, pak vložte řádek v databázi Oracle (akce). 

Toto téma ukazuje, jak toouse hello databáze Oracle konektor v aplikaci logiky.

## <a name="prerequisites"></a>Požadavky

* Podporované verze Oracle: 
    * Oracle 9 a novějším
    * Klientský software Oracle 8.1.7 a novější

* Nainstalujte bránu dat místní hello. [Připojení místní tooon dat z aplikace logiky](../logic-apps/logic-apps-gateway-connection.md) seznamy hello kroky. Brána Hello je požadované tooconnect tooan místní databázi Oracle nebo virtuální počítač Azure s Oracle DB nainstalována. 

    > [!NOTE]
    > Brána dat místní Hello funguje jako mostu a poskytuje přenos zabezpečených dat mezi místními daty (data, která není v cloudu hello) a aplikace logiky. Hello stejnou bránou lze použít s více službami a několik datových zdrojů. Ano jenom musíte tooinstall hello brány jednou.

* Nainstalujte na hello počítače, kam jste nainstalovali hello místní data brána hello klienta Oracle. Ujistěte se, tooinstall hello 64-bit poskytovatele dat Oracle pro .NET z databáze Oracle:  

  [64-bit ODAC 12c verze 4 (12.1.0.2.4) pro Windows x64](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)

    > [!TIP]
    > Pokud není nainstalován klient hello Oracle, dojde k chybě při akci toocreate nebo používat hello připojení. Najdete běžné chyby hello v tomto tématu.


## <a name="add-hello-connector"></a>Přidejte konektor hello

> [!IMPORTANT]
> Tento konektor nemá žádné aktivační události. Má pouze akce. Proto při vytváření aplikace logiky, přidejte jiný toostart aktivační událost aplikace logiky, jako **plán - opakování**, nebo **požadavku nebo odpovědi - odpovědi**. 

1. V hello [portál Azure](https://portal.azure.com), vytvoření aplikace logiky prázdné.

2. Při spuštění hello aplikace logiky, vyberte hello **požadavku nebo odpovědi - požadavek** aktivační události: 

    ![](./media/connectors-create-api-oracledatabase/request-trigger.png)

3. Vyberte **Uložit**. Při ukládání, se automaticky vygeneroval adrese URL žádosti. 

4. Vyberte **nový krok**a vyberte **přidat akci**. Zadejte `oracle` toosee hello dostupné akce: 

    ![](./media/connectors-create-api-oracledatabase/oracledb-actions.png)

    > [!TIP]
    > Toto je také hello nejrychlejší způsob, jak toosee hello triggery a akce, které jsou k dispozici pro všechny konektory. Zadejte v části název konektoru hello, například `oracle`. Návrhář Hello uvádí žádné aktivační události a všechny akce. 

5. Vyberte jednu z hello akcí, jako například **Oracle Database - Get řádek**. Vyberte **připojit prostřednictvím místní brána dat**. Zadejte název serveru Oracle hello, metodu ověřování, uživatelské jméno, heslo a vyberte hello brány:

    ![](./media/connectors-create-api-oracledatabase/create-oracle-connection.png)

6. Po připojení, vyberte tabulku ze seznamu hello a zadejte hello řádek ID tooyour tabulky. Je nutné tooknow hello identifikátor toohello tabulky. Pokud si nejste jisti, obraťte se na správce databáze Oracle a získat výstup hello `select * from yourTableName`. Tato poskytuje hello osobní údaje, které budete potřebovat tooproceed.

    V následujícím příkladu hello se vrací data úlohy z databáze lidských zdrojů: 

    ![](./media/connectors-create-api-oracledatabase/table-rowid.png)

7. V tomto kroku další můžete použít všechny hello jiných konektorů toobuild pracovního postupu. Pokud chcete tootest získávání dat z databáze Oracle a potom odeslat e-mail s hello Oracle data pomocí jedné z hello sami odešlete e-mailu konektory, takové Office 365 nebo z Gmailu. Použití dynamické tokeny hello od hello Oracle tabulky toobuild hello `Subject` a `Body` vašeho e-mailu:

    ![](./media/connectors-create-api-oracledatabase/oracle-send-email.png)

8. **Uložit** vaší aplikace logiky a potom vyberte **spustit**. Zavřít návrháře hello a podívejte se na hello spustí historie stavu hello. Pokud se nezdaří, vyberte řádek neúspěšné zprávy hello. Návrhář Hello otevře a ukáže vám, který krok se nezdařil a také ukazuje hello informace o chybě. Pokud se aktivace podaří, měli byste obdržet e-mail s hello informace, které jste přidali.


### <a name="workflow-ideas"></a>Nápady pracovního postupu

* Chcete toomonitor hello #oracle hashtag a put hello tweetů v databázi, mohou být dotazována a používaných v rámci jiné aplikace. V aplikaci logiky, aplikaci přidat hello `Twitter - When a new tweet is posted` aktivovat a potom zadejte hello **#oracle** hashtag. Pak přidejte hello `Oracle Database - Insert row` akce a vyberte tabulku:

    ![](./media/connectors-create-api-oracledatabase/twitter-oracledb.png)

* Zprávy jsou odesílány tooa fronty Service Bus. Chcete tyto zprávy tooget a umístí je do databáze. V aplikaci logiky, aplikaci přidat hello `Service Bus - when a message is received in a queue` aktivační události a vyberte hello fronty. Pak přidejte hello `Oracle Database - Insert row` akce a vyberte tabulku:

    ![](./media/connectors-create-api-oracledatabase/sbqueue-oracledb.png)

## <a name="common-errors"></a>Běžné chyby

#### <a name="error-cannot-reach-hello-gateway"></a>**Chyba**: Nelze kontaktovat hello brány

**Příčina**: hello místní data brána není možné tooconnect toohello cloudu. 

**Zmírnění dopadů**: Zajistěte, aby vaše brána je spuštěná na hello na místním počítači, kde jste ho nainstalovali, a to může připojit toohello Internetu.  Doporučujeme není hello bránu nainstalovat na počítač, který může být vypnutý nebo v režimu spánku. Můžete také restartovat hello místní služba brány dat (PBIEgwService).

#### <a name="error-hello-provider-being-used-is-deprecated-systemdataoracleclient-requires-oracle-client-software-version-817-or-greater-please-visit-httpsgomicrosoftcomfwlinkplinkid272376httpsgomicrosoftcomfwlinkplinkid272376-tooinstall-hello-official-provider"></a>**Chyba**: používaný hello zprostředkovatel je zastaralý: ' System.Data.OracleClient vyžaduje Oracle verze klientského softwaru 8.1.7 nebo vyšší. ". Navštivte [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) tooinstall hello oficiálního zprostředkovatele.

**Příčina**: Sada SDK není nainstalovaná na počítači hello kde hello klienta Oracle hello místní brána dat je spuštěná.  

**Řešení**: Stáhněte a nainstalujte klienta Oracle hello SDK na hello stejného počítače jako brány dat místní hello.

#### <a name="error-table-tablename-does-not-define-any-key-columns"></a>**Chyba**: Tabulka [Tablename] nedefinuje žádné klíčové sloupce

**Příčina**: hello tabulka neobsahuje žádné primární klíč.  

**Řešení**: hello databáze Oracle connector vyžaduje použít tabulku se sloupcem primárního klíče.

#### <a name="currently-not-supported"></a>Aktuálně není podporována

* Zobrazení a uložených procedur 
* Všechny tabulky s složené klíče
* Vnořené typy objektů v tabulkách
 
## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/oracle/). 

## <a name="get-some-help"></a>Získat pomoc

Hello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) je skvělá umístit tooask otázky, odpovědi na otázky a najdete v části činnosti jiných uživatelů Logic Apps. 

Vám může pomoct zlepšit konektory a aplikace logiky hlasování a odesílání vašich nápadů v [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish). 


## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md)a seznamte se s hello dostupných konektorů v logiku aplikace na náš [rozhraní API seznamu](apis-list.md).
