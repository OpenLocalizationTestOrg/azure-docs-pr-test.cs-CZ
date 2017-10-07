---
title: "aaaCustom kód pro Azure Logic Apps s Azure Functions | Microsoft Docs"
description: "Vytvoření a spuštění vlastní kód pro Azure Logic Apps s Azure Functions"
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 18b3821ed7e434feb568b9b96e9a5a2189dba3bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-and-run-custom-code-for-logic-apps-through-azure-functions"></a>Přidejte a spuštění vlastní kód pro logic apps prostřednictvím Azure Functions

toorun vlastní fragmenty kódu C# nebo node.js v aplikace logiky, můžete vytvořit vlastní funkce prostřednictvím Azure Functions. 
[Azure Functions](../azure-functions/functions-overview.md) nabízí práci bez serveru v Microsoft Azure a jsou užitečné pro provádění těchto úkolů:

* Pokročilé formátování nebo výpočetní polí v aplikace logiky
* Provádění výpočtů v pracovním postupu.
* Rozšířit funkce aplikace logiky hello s funkcemi, které jsou podporovány v C# nebo node.js

## <a name="create-custom-functions-for-your-logic-apps"></a>Vytvoření vlastní funkce pro logic apps

Doporučujeme vytvořit funkci Azure Functions portálu hello z hello **obecný Webhook - uzlu** nebo **obecný Webhook - C#** šablony. výsledek Hello vytvoří automaticky vyplněna šablonu, která přijímá `application/json` z aplikace logiky. Funkce, které můžete vytvořit z těchto šablon se automaticky zjišťují a zobrazují v hello návrhář aplikace logiky v rámci **Azure Functions v mojí oblasti.**

V portálu Azure, na hello hello **integrací** v podokně funkce, vaše šablona by měla ukazují, že **režimu** nastavit příliš**Webhooku** a **Webhooku typ** je nastaven příliš**obecné JSON**. 

Funkce Webhooku žádost o přijmout a předejte ji do metody hello přes `data` proměnné. Máte přístup k vlastnosti hello vaše datové části pomocí zápisu s tečkou jako `data.function-name`. Například jednoduché funkce JavaScript, která převede hodnotu data a času na řetězec datum vypadá hello následující ukázka:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-logic-apps"></a>Azure Functions volání z aplikace logiky

kontejnery hello toolist v vaše předplatné a vyberte hello funkce, které chcete toocall v návrháři aplikace logiky, klikněte na tlačítko hello **akce** nabídce a vyberte z **Azure Functions v mojí oblasti**.

Po výběru hello funkce, se zobrazí výzva toospecify datová část vstupního objektu. Tento objekt je uvítací zprávu, která hello logiku aplikace odesílá toohello funkce a musí být objekt JSON. Například, pokud chcete, aby toopass v hello **poslední změny** data ze služby Salesforce aktivační událost, datové funkce hello může vypadat jako tento ukázkový:

![Datum poslední změny][1]

## <a name="trigger-logic-apps-from-a-function"></a>Aplikace logiky aktivační události z funkce

Můžete aktivovat aplikaci logiky z uvnitř funkce. V tématu [Logic apps jako koncové body s](logic-apps-http-endpoint.md). Vytvoření aplikace logiky, který má aktivační události ruční a potom z uvnitř vaší funkce generovat adresu URL ruční aktivační událost toohello HTTP POST s odebranou datovou částí hello, který chcete odeslat toohello aplikace logiky.

### <a name="create-a-function-from-logic-app-designer"></a>Vytvoření funkce z návrháře aplikace logiky

Můžete také vytvořit funkci webhooku node.js z Návrháře hello. Nejdřív vyberte **Azure Functions v mojí oblasti** a potom vyberte kontejner pro funkce. Pokud ještě nemáte kontejner, je třeba toocreate jeden z hello [portálu Azure Functions](https://functions.azure.com/signin). Potom vyberte **vytvořit nový**.  

toogenerate šablonu na základě hello dat, které chcete toocompute, zadejte objekt kontextu hello plánování toopass do funkce. Tento objekt musí být objekt JSON. Například pokud předáte obsah souboru hello z akce FTP, datové části kontextu hello vypadá v tomto příkladu:

![Datová část kontextu][2]

> [!NOTE]
> Protože tento objekt nebyl přetypovat jako řetězec, se přidá hello obsah přímo toohello datové části JSON. Však dojde k chybě, pokud objekt hello není token JSON (to znamená, že řetězec nebo JSON objekt nebo pole). toocast hello objekt jako řetězec, přidávat uvozovky, jak je znázorněno v hello první obrázek v tomto článku.
> 

Návrhář Hello potom vytvoří šablonu funkce, můžete vytvořit vložený. Proměnné jsou předem vytvořené podle kontextu hello plánování toopass do funkce hello.

<!--Image references-->
[1]: ./media/logic-apps-azure-functions/callfunction.png
[2]: ./media/logic-apps-azure-functions/createfunction.png
