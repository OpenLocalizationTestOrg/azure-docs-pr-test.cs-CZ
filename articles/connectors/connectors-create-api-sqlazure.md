---
title: "konektor Azure SQL Database hello aaaAdd ve vašich Logic Apps | Microsoft Docs"
description: "Přehled konektoru Azure SQL Database s parametry rozhraní REST API"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d8a319d0-e4df-40cf-88f0-29a6158c898c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: a9ca0f446d05dc00f310a908eee8d50e41fcd82b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-azure-sql-database-connector"></a>Začínáme s Azure SQL Database konektor hello
Pomocí konektoru Azure SQL Database hello vytvořte pracovní postupy pro vaši organizaci, které spravují data v tabulkách. 

S databází SQL můžete:

* Vytvořte pracovní postup přidávání nové databáze zákazníci tooa zákazníka nebo aktualizaci pořadí, v databázi objednávky.
* Pomocí akce tooget řádek dat, vložit nový řádek a ani odstranit. Například v aplikaci Dynamics CRM Online (aktivační událost) je vytvořen záznam, pak vložte řádek v Azure SQL Database (akce). 

Toto téma ukazuje, jak toouse hello konektor databáze SQL v aplikaci logiky, a také seznamy hello akce.

toolearn Další informace o Logic Apps, najdete v části [co jsou logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-tooazure-sql-database"></a>Připojit tooAzure databáze SQL
Než se aplikace logiky k jakékoli služby, nejprve vytvořit *připojení* toohello služby. Připojení poskytuje připojení mezi aplikace logiky a jiné služby. Například tooconnect tooSQL databáze, je nejprve vytvořit databázi SQL *připojení*. toocreate připojení, zadejte přihlašovací údaje hello normálně používat tooaccess hello služby, ke kterému se připojujete. Ano v databázi SQL, zadejte vaše toocreate hello připojení k databázi SQL přihlašovací údaje. 

#### <a name="create-hello-connection"></a>Vytvoření připojení hello
> [!INCLUDE [Create hello connection tooSQL Azure](../../includes/connectors-create-api-sqlazure.md)]
> 
> 

## <a name="use-a-trigger"></a>Použít aktivační událost
Tento konektor nemá žádné aktivační události. Použijte jiné aktivační události toostart hello aplikace logiky, jako jsou aktivační událost opakování, aktivační události Webhooku protokolu HTTP, aktivační události, které jsou k dispozici ostatní konektory a další. [Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) poskytuje příklad.

## <a name="use-an-action"></a>Použít akci.
Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky. [Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Vyberte znaménko plus hello. Zobrazí několik možností: **přidat akci**, **přidat podmínku**, nebo jeden z hello **Další** možnosti.
   
    ![](./media/connectors-create-api-sqlazure/add-action.png)
2. Zvolte **přidat akci**.
3. Hello textového pole zadejte "sql" tooget seznam všechny dostupné akce hello.
   
    ![](./media/connectors-create-api-sqlazure/sql-1.png) 
4. V našem příkladu zvolte **systému SQL Server - Get řádek**. Pokud připojení již existuje, pak vyberte hello **název tabulky** z rozevíracího seznamu hello seznamu a zadejte hello **ID řádku** chcete tooreturn.
   
    ![](./media/connectors-create-api-sqlazure/sample-table.png)
   
    Pokud budete vyzváni k hello informace o připojení, zadejte hello podrobnosti toocreate hello připojení. [Vytvoření připojení hello](connectors-create-api-sqlazure.md#create-the-connection) v tomto tématu popisuje tyto vlastnosti. 
   
   > [!NOTE]
   > V tomto příkladu jsme vrátí řádek z tabulky. toosee hello data v tomto řádku přidat akci, která se vytvoří soubor pomocí hello polí z tabulky hello. Například přidejte OneDrive akci, která používá hello FirstName a LastName pole toocreate nový soubor v hello účet cloudového úložiště. 
   > 
   > 
5. **Uložit** změny (levém horním rohu hello panelu nástrojů). Aplikace logiky je uloženo a automaticky povolí.

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/sql/). 

## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md). Prozkoumejte hello dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).

