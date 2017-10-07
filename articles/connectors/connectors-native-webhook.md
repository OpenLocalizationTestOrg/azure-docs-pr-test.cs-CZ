---
title: konektor aaaWebhook pro Azure Logic Apps | Microsoft Docs
description: "Jak akce webhooku toouse a aktivační události tooperform akce, jako třeba pole filtru z aplikace logiky"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 71775384-6c3a-482c-a484-6624cbe4fcc7
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: b2dee12750f3f20f10e7b257da05a79f28f90f43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-webhook-connector"></a>Začínáme s konektorem webhooku hello

Akce webhooku hello a aktivační události můžete spustit, pozastavit a obnovit toky tooperform tyto úlohy:

* Aktivovat z [centra událostí Azure](https://github.com/logicappsio/EventHubAPI) při doručení položky
* Čekat na schválení před pokračováním pracovního postupu

Další informace o [jak toocreate vlastní rozhraní API, které podporují webhook, jehož](../logic-apps/logic-apps-create-api-app.md).

## <a name="use-hello-webhook-trigger"></a>Použít aktivační události webhooku hello

A [ *aktivační událost* ](connectors-overview.md) je událost, která spustí pracovní postup aplikace logiky. Aktivační události webhooku je založený na událostech a nemá spoléhají na cyklické dotazování na nové položky. Jako hello [aktivační událost požadavku](connectors-native-reqres.md), aktivuje se v aplikaci logiky hello hello rychlé, že událost nastane. zaregistruje aktivační události webhooku Hello *adresu URL zpětné volání* tooa služby a používá tuto adresu URL toofire hello logiku aplikace jako potřeby.

Tady je příklad, který ukazuje, jak aktivovat tooset zálohu protokolu HTTP v hello návrhář aplikace logiky. Hello kroky předpokládají už mají nasazenou nebo přístupu k rozhraní API, která následuje hello [webhooku přihlášení k odběru a odhlášení vzor v aplikacích logiky](../logic-apps/logic-apps-create-api-app.md#webhook-triggers). Hello přihlášení k odběru volání přišla vždy, když je aplikace logiky uložit s novou webhooku nebo přechod z zakázané tooenabled. Hello odhlásit volání provádí při odebrat a uložit nebo přechod z povoleno toodisabled aktivační události webhooku aplikace logiky.

**aktivační události webhooku tooadd hello**

1. Přidat hello **Webhooku protokolu HTTP** aktivační událost jako první krok hello v aplikaci logiky.
2. Vyplňte hello parametry webhooku hello přihlášení k odběru a zrušit volání.

   Tento krok následuje hello stejný vzor jako hello [akce HTTP](connectors-native-http.md) formátu.

     ![Aktivace protokolu HTTP](./media/connectors-native-webhook/using-trigger.png)

3. Přidejte alespoň jednu akci.
4. Klikněte na tlačítko **Uložit** toopublish hello logiku aplikace. Tento krok volání hello přihlášení k odběru koncový bod s hello zpětného volání URL potřeby tootrigger tuto aplikaci logiky.
5. Vždy, když hello díky služby `HTTP POST` toohello URL zpětné volání, hello aktivuje aplikaci logiky a zahrnuje všechna data předané do hello požadavku.

## <a name="use-hello-webhook-action"></a>Pomocí akce webhooku hello

[ *Akce* ](connectors-overview.md) se operace provádí v pracovním postupu hello definované v aplikaci logiky. Akce webhooku registruje *adresu URL zpětné volání* s služby a počká na adresu URL hello je volána před provedením obnovení. Hello ["Odeslat E-mail schválení"](connectors-create-api-office365-outlook.md) je příkladem konektor, který následuje tohoto vzoru. Tento vzor můžete rozšířit do jakékoli služby prostřednictvím akce webhooku hello. 

Tady je příklad, který ukazuje, jak tooset se akce webhooku v hello návrhář aplikace logiky. Tento postup předpokládá už mají nasazenou nebo přístupu k rozhraní API, která následuje hello [webhooku přihlášení k odběru a odhlášení způsobem používaným v aplikacích logiky](../logic-apps/logic-apps-create-api-app.md#webhook-actions). Hello přihlášení k odběru volání se provádí, když aplikace logiky, provede akce webhooku hello. Hello odhlásit volání provádí při spuštění je zrušená při čekání na odpověď, nebo před hello logiku aplikace časového limitu.

**tooadd akce webhooku**

1. Zvolte **nový krok** > **přidat akci**.

2. Hello vyhledávacího pole zadejte "webhooku" toofind hello **Webhooku protokolu HTTP** akce.

    ![Vyberte akci dotazu](./media/connectors-native-webhook/using-action-1.png)

3. Vyplňte hello parametry webhooku hello přihlášení k odběru a zrušit volání

   Tento krok následuje hello stejný vzor jako hello [akce HTTP](connectors-native-http.md) formátu.

     ![Dokončení dotazu akce](./media/connectors-native-webhook/using-action-2.png)
   
   V době běhu hello logiku aplikace volání hello přihlášení k odběru koncový bod po dosažení tohoto kroku.

4. Klikněte na tlačítko **Uložit** toopublish hello logiku aplikace.

## <a name="technical-details"></a>Technické podrobnosti

Zde jsou další informace o hello triggery a akce se tento webhook podporuje.

## <a name="webhook-triggers"></a>Aktivační události Webhooku

| Akce | Popis |
| --- | --- |
| Webhooku protokolu HTTP |Přihlášení k odběru služby tooa adresu URL zpětné volání, která můžete volat hello URL toofire logiku aplikace podle potřeby. |

### <a name="trigger-details"></a>Podrobnosti o aktivační události

#### <a name="http-webhook"></a>Webhooku protokolu HTTP

Přihlášení k odběru služby tooa adresu URL zpětné volání, která můžete volat hello URL toofire logiku aplikace podle potřeby.
* Znamená povinné pole.

| Zobrazovaný název | Název vlastnosti | Popis |
| --- | --- | --- |
| Přihlášení k odběru metoda * |– Metoda |Metoda HTTP toouse pro požadavek přihlásit k odběru |
| Přihlášení k odběru URI * |identifikátor URI |Identifikátor URI HTTP toouse pro požadavek přihlásit k odběru |
| Odhlášení metoda * |– Metoda |Toouse metoda HTTP pro žádosti o odhlášení odběru |
| Odhlášení URI * |identifikátor URI |Identifikátor URI HTTP toouse pro žádosti o odhlášení odběru |
| Přihlášení k odběru textu |Text |Požadavku HTTP pro přihlásit k odběru |
| Přihlášení k odběru hlavičky |Záhlaví |Hlavičky požadavku HTTP pro přihlásit k odběru |
| Přihlášení k odběru ověřování |Ověřování |Toouse ověřování protokolu HTTP pro přihlásit k odběru. [Najdete v části konektor HTTP](connectors-native-http.md#authentication) podrobnosti |
| Odhlášení textu |Text |Požadavku HTTP pro zrušení odběru |
| Odhlášení hlavičky |Záhlaví |Hlavičky požadavku HTTP pro zrušení odběru |
| Zrušit ověřování |Ověřování |Toouse ověřování protokolu HTTP pro zrušení odběru. [Najdete v části konektor HTTP](connectors-native-http.md#authentication) podrobnosti |

**Podrobnosti o výstupu**

Žádost o Webhooku

| Název vlastnosti | Datový typ | Popis |
| --- | --- | --- |
| Záhlaví |Objekt |Hlavičky žádosti Webhooku |
| Tělo |Objekt |Objekt žádosti Webhooku |
| Stavový kód |celá čísla |Stavový kód žádosti o Webhooku |

## <a name="webhook-actions"></a>Akce Webhooku

| Akce | Popis |
| --- | --- |
| Webhooku protokolu HTTP |Přihlášení k odběru služba tooa adresu URL zpětné volání, která můžete volat hello URL tooresume krok pracovního postupu, podle potřeby. |

### <a name="action-details"></a>Podrobnosti akce

#### <a name="http-webhook"></a>Webhooku protokolu HTTP

Přihlášení k odběru služba tooa adresu URL zpětné volání, která můžete volat hello URL tooresume krok pracovního postupu, podle potřeby.
* Znamená povinné pole.

| Zobrazovaný název | Název vlastnosti | Popis |
| --- | --- | --- |
| Přihlášení k odběru metoda * |– Metoda |Metoda HTTP toouse pro požadavek přihlásit k odběru |
| Přihlášení k odběru URI * |identifikátor URI |Identifikátor URI HTTP toouse pro požadavek přihlásit k odběru |
| Odhlášení metoda * |– Metoda |Toouse metoda HTTP pro žádosti o odhlášení odběru |
| Odhlášení URI * |identifikátor URI |Identifikátor URI HTTP toouse pro žádosti o odhlášení odběru |
| Přihlášení k odběru textu |Text |Požadavku HTTP pro přihlásit k odběru |
| Přihlášení k odběru hlavičky |Záhlaví |Hlavičky požadavku HTTP pro přihlásit k odběru |
| Přihlášení k odběru ověřování |Ověřování |Toouse ověřování protokolu HTTP pro přihlásit k odběru. [Najdete v části konektor HTTP](connectors-native-http.md#authentication) podrobnosti |
| Odhlášení textu |Text |Požadavku HTTP pro zrušení odběru |
| Odhlášení hlavičky |Záhlaví |Hlavičky požadavku HTTP pro zrušení odběru |
| Zrušit ověřování |Ověřování |Toouse ověřování protokolu HTTP pro zrušení odběru. [Najdete v části konektor HTTP](connectors-native-http.md#authentication) podrobnosti |

**Podrobnosti o výstupu**

Žádost o Webhooku

| Název vlastnosti | Datový typ | Popis |
| --- | --- | --- |
| Záhlaví |Objekt |Hlavičky žádosti Webhooku |
| Tělo |Objekt |Objekt žádosti Webhooku |
| Stavový kód |celá čísla |Stavový kód žádosti o Webhooku |

## <a name="next-steps"></a>Další kroky

* [Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md)
* [Vyhledání jiných konektorů](apis-list.md)