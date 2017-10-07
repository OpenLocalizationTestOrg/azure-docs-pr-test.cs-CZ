---
title: "Akce požadavku a odpovědi aaaUse | Microsoft Docs"
description: "Přehled hello žádostí a odpovědí aktivační události a akce v aplikaci Azure logiky"
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 566924a4-0988-4d86-9ecd-ad22507858c0
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 24c378cc12d5f3f65116d5e59278236186a99662
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-request-and-response-components"></a>Začínáme s komponentami hello žádosti a odpovědi
S hello součásti v aplikaci logiky žádostí a odpovědí může reagovat v reálném čase tooevents.

Můžete například provést následující věci:

* Odpověď žádost HTTP tooan s daty z místní databáze pomocí aplikace logiky.
* Aktivovat aplikaci logiky z externí webhooku události.
* Volání aplikace logiky pomocí akce žádostí a odpovědí z v rámci jiné aplikace logiky.

tooget spuštění pomocí hello žádostí a odpovědí akce v aplikaci logiky, aplikaci najdete v části [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-hello-http-request-trigger"></a>Použití hello aktivační událost požadavku HTTP
Aktivační událost je událost, která lze použít toostart hello workflow, který je definován v aplikaci logiky. [Další informace o aktivační události](connectors-overview.md).

Zde je příklad posloupnosti jak tooset zálohu protokolu HTTP žádosti v hello návrhář aplikace logiky.

1. Přidat aktivační událost hello **požadavku - obdrží žádost HTTP při** v aplikaci logiky. Volitelně můžete zadat schématu JSON (pomocí některého nástroje, například [JSONSchema.net](http://jsonschema.net)) tělo žádosti hello. To umožňuje hello návrháře toogenerate tokeny pro vlastnosti v žádosti o hello protokolu HTTP.
2. Přidáte další akci, takže můžete uložit aplikaci logiky hello.
3. Po uložení hello aplikace logiky, můžete získat hello HTTP požadavku na adresu URL z karty hello požadavku.
4. HTTP POST (můžete použít nástroje, jako je [Postman](https://www.getpostman.com/)) aktivace adresy URL toohello hello aplikace logiky.

> [!NOTE]
> Pokud nedefinujete akce odpovědi, `202 ACCEPTED` odpověď se vrátí okamžitě toohello volajícího. Můžete použít hello odpovědi akce toocustomize odpověď.
> 
> 

![Aktivační událost odpovědi](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-hello-http-response-action"></a>Použití hello akce odpovědi HTTP
Hello odpověď HTTP akce je platná pouze při použití v pracovním postupu, která se spustí při požadavku HTTP. Pokud nedefinujete akce odpovědi, `202 ACCEPTED` odpověď se vrátí okamžitě toohello volajícího.  Akce odpovědi můžete přidat v kterémkoliv kroku v rámci pracovního postupu hello. aplikace logiky Hello pouze udržuje příchozího požadavku hello otevřete jednu minutu pro odpověď.  Za minutu, pokud žádná odpověď byla odeslána z pracovního postupu hello (a existuje akce odpovědi v definici hello) `504 GATEWAY TIMEOUT` je vrácen toohello volajícího.

Tady je způsob tooadd akce odpovědi HTTP:

1. Vyberte hello **nový krok** tlačítko.
2. Zvolte **přidat akci**.
3. Hello akce vyhledávacího pole zadejte **odpovědi** toolist hello akce odpovědi.
   
    ![Vyberte akci odpovědi hello](./media/connectors-native-reqres/using-action-1.png)
4. Přidejte do žádné parametry, které jsou požadovány pro zprávu odpovědi HTTP hello.
   
    ![Akce odpovědi dokončení hello](./media/connectors-native-reqres/using-action-2.png)
5. Klikněte na tlačítko hello levého horního rohu toosave hello panelu nástrojů a vaše logiku aplikace uloží obou a publikování (aktivovat).

## <a name="request-trigger"></a>Žádost o aktivační události
Tady jsou hello podrobnosti hello aktivační událost, která tento konektor podporuje. Je aktivační událost jedné žádosti.

| Aktivační události | Popis |
| --- | --- |
| Žádost |Nastane, když obdrží požadavek HTTP |

## <a name="response-action"></a>Akce odpovědi
Tady jsou hello podrobnosti hello akce, který podporuje tento konektor. Není k dispozici odpověď jedné akci, která lze použít pouze když je přiložena aktivační událost požadavku.

| Akce | Popis |
| --- | --- |
| Odpověď |Vrátí že odpověď toohello korelační požadavku HTTP |

### <a name="trigger-and-action-details"></a>Podrobnosti o aktivační události a akce
Hello následující tabulky popisují hello vstupní pole pro aktivační událost hello a akce a odpovídající podrobnosti výstup hello.

#### <a name="request-trigger"></a>Žádost o aktivační události
Hello následuje vstupní pole pro aktivační událost hello z příchozí žádosti protokolu HTTP.

| Zobrazované jméno | Název vlastnosti | Popis |
| --- | --- | --- |
| Schématu JSON |Schéma |schéma JSON Hello obsahu žádosti HTTP hello |

<br>

**Podrobnosti o výstupu**

Hello následují výstup podrobnosti požadavku hello.

| Název vlastnosti | Datový typ | Popis |
| --- | --- | --- |
| Záhlaví |Objekt |Hlavičky požadavku |
| Tělo |Objekt |Objekt žádosti |

#### <a name="response-action"></a>Akce odpovědi
Hello následují vstupní pole pro hello akce odpovědi HTTP. A * znamená, že je povinné pole.

| Zobrazované jméno | Název vlastnosti | Popis |
| --- | --- | --- |
| Stavový kód * |statusCode |Hello stavový kód protokolu HTTP |
| Záhlaví |Záhlaví |Objekt JSON žádné tooinclude hlavičky odpovědi |
| Tělo |Text |text odpovědi Hello |

## <a name="next-steps"></a>Další kroky
Teď vyzkoušet hello platformy a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md). Můžete prozkoumat hello dalších dostupných konektorů v aplikacích logiky pohledem na našem [rozhraní API seznamu](apis-list.md).

