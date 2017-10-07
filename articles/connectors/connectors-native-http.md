---
title: "aaaCommunicate s žádný koncový bod přes protokol HTTP - Azure Logic Apps | Microsoft Docs"
description: "Vytvoření aplikace logiky, která může komunikovat s žádný koncový bod přes protokol HTTP"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: e11c6b4d-65a5-4d2d-8e13-38150db09c0b
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: 9793601839437a2b880bdb81e15881270cacc963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-http-action"></a>Začínáme s hello akce HTTP

S hello akce HTTP můžete rozšířit pracovní postupy pro vaši organizaci a koncový bod tooany komunikaci pomocí protokolu HTTP.

Můžete:

* Vytvořte logiku aplikace pracovní postupy, které aktivovat (aktivační události), kdy web, který spravujete přestane fungovat.
* Komunikují koncový bod tooany přes HTTP tooextend vaše pracovní postupy k dalším službám.

tooget spuštění pomocí hello HTTP akce v aplikaci logiky, aplikaci najdete v části [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-hello-http-trigger"></a>Použít aktivační událost hello HTTP
Aktivační událost je událost, která lze použít toostart hello workflow, který je definován v aplikaci logiky. [Další informace o aktivační události](connectors-overview.md).

Zde je příklad posloupnosti jak aktivovat tooset až hello HTTP v hello návrhář aplikace logiky.

1. Přidejte hello triggeru protokolu HTTP v aplikaci logiky.
2. Vyplňte hello parametry pro koncový bod hello HTTP, které chcete toopoll.
3. Změna intervalu opakování hello na tom, jak často má dotazovat.

   aplikace logiky Hello teď aktivuje s obsahem, který je vrácen během každé kontroly.

   ![Trigger HTTP](./media/connectors-native-http/using-trigger.png)

### <a name="how-hello-http-trigger-works"></a>Jak funguje hello triggeru protokolu HTTP

Aktivace protokolu HTTP Hello odešle tooHTTP koncový bod volání na intervalu opakování. Ve výchozím kódu odpovědi HTTP, která je nižší než 300 způsobí, že toorun logiku aplikace. toospecify zda by měly aktivovat aplikaci logiky hello, můžete upravit aplikaci logiky hello v zobrazení kódu a přidejte podmínku, která vyhodnotí po hello volání protokolu HTTP. Tady je příklad HTTP aktivační událost, která aktivuje se v případě hello vrátil kód stavu je větší než nebo roven hodnotě příliš`400`.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

Úplné podrobnosti o parametrech hello HTTP aktivační události jsou k dispozici na [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).

## <a name="use-hello-http-action"></a>Pomocí akce hello HTTP

Akce je operace, která se provádí v pracovním postupu hello, která je definována v aplikaci logiky. 
[Další informace o akcích](connectors-overview.md).

1. Zvolte **nový krok** > **přidat akci**.
3. Hello akce vyhledávacího pole zadejte **http** toolist hello HTTP akce.
   
    ![Vyberte akci hello HTTP](./media/connectors-native-http/using-action-1.png)

4. Přidejte všechny požadované parametry pro volání hello protokolu HTTP.
   
    ![Dokončení hello akce HTTP](./media/connectors-native-http/using-action-2.png)

5. Na hello návrháře nástrojů, klikněte na tlačítko **Uložit**. Uložení a publikování (aktivovaný) na hello svou aplikaci logiky stejnou dobu.

## <a name="http-trigger"></a>Trigger HTTP
Tady jsou hello podrobnosti hello aktivační událost, která tento konektor podporuje. konektor HTTP Hello má jedna aktivační událost.

| Aktivační události | Popis |
| --- | --- |
| HTTP |Provede volání protokolu HTTP a vrátí obsah odpovědi hello. |

## <a name="http-action"></a>Akce HTTP
Tady jsou hello podrobnosti hello akce, který podporuje tento konektor. konektor HTTP Hello má jednu možné akci.

| Akce | Popis |
| --- | --- |
| HTTP |Provede volání protokolu HTTP a vrátí obsah odpovědi hello. |

## <a name="http-details"></a>Podrobnosti protokolu HTTP
Hello následující tabulky popisují hello požadované a volitelné vstupních polí pro akce hello a hello odpovídající výstup podrobností, které jsou spojené s použitím akce hello.

#### <a name="http-request"></a>Požadavek HTTP
Hello následují vstupní pole pro hello akce, která umožňuje odchozí požadavku HTTP.
A * znamená, že je povinné pole.

| Zobrazované jméno | Název vlastnosti | Popis |
| --- | --- | --- |
| Metoda * |– Metoda |příkaz toouse Hello HTTP |
| IDENTIFIKÁTOR URI * |identifikátor URI |Hello identifikátor URI pro požadavek hello HTTP |
| Záhlaví |Záhlaví |Objekt JSON tooinclude hlavičky protokolu HTTP |
| Tělo |Text |Hello požadavku HTTP |
| Authentication |Ověřování |Podrobnosti v hello [ověřování](#authentication) části |

<br>

#### <a name="output-details"></a>Podrobnosti o výstupu
Hello následují výstup podrobnosti hello odpověď HTTP.

| Název vlastnosti | Datový typ | Popis |
| --- | --- | --- |
| Záhlaví |Objekt |Hlavičky odpovědi |
| Tělo |Objekt |Objekt odpovědi |
| Stavový kód |celá čísla |Stavový kód protokolu HTTP |

## <a name="authentication"></a>Authentication
Funkce Logic Apps Hello umožňuje toouse různé typy ověřování proti koncových bodů protokolu HTTP. Toto ověřování můžete použít s hello **HTTP**,  **[HTTP + Swagger](connectors-native-http-swagger.md)**, a  **[Webhooku protokolu HTTP](connectors-native-webhook.md)**  konektory. Hello následující typy ověřování se dají konfigurovat:

* [Základní ověřování](#basic-authentication)
* [Ověřování certifikátu klienta](#client-certificate-authentication)
* [Ověřování Azure Active Directory (Azure AD) OAuth](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Základní ověřování

Hello následující ověřování objektu je potřeba pro základní ověřování.
A * znamená, že je povinné pole.

| Název vlastnosti | Datový typ | Popis |
| --- | --- | --- |
| Typ * |type |Typ ověřování (musí být `Basic` pro základní ověřování) |
| Uživatelské jméno * |uživatelské jméno |Název tooauthenticate uživatele |
| Heslo * |heslo |Tooauthenticate heslo |

> [!TIP]
> Pokud chcete toouse heslo, které nelze načíst z definice hello, použijte `securestring` parametr a hello `@parameters()`  
>  [funkce definice pracovního postupu](http://aka.ms/logicappdocs).

Například:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>Ověřování klientských certifikátů

Hello následující ověřování objektu je potřeba pro ověřování pomocí certifikátu klienta. A * znamená, že je povinné pole.

| Název vlastnosti | Datový typ | Popis |
| --- | --- | --- |
| Typ * |type |typ ověřování Hello (musí být `ClientCertificate` pro klientské certifikáty SSL) |
| SOUBOR PFX * |Soubor PFX |Hello kódováním Base64 obsah souboru hello Personal Information Exchange (PFX) |
| Heslo * |heslo |Hello heslo tooaccess hello souboru PFX |

> [!TIP]
> toouse parametr, který nebude možné číst v definici hello po uložení hello aplikace logiky, můžete použít `securestring` parametr a hello `@parameters()`  
>  [funkce definice pracovního postupu](http://aka.ms/logicappdocs).

Například:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Ověřování služby Azure AD OAuth
Hello následující ověřování objektu je potřeba pro ověřování Azure AD OAuth. A * znamená, že je povinné pole.

| Název vlastnosti | Datový typ | Popis |
| --- | --- | --- |
| Typ * |type |typ ověřování Hello (musí být `ActiveDirectoryOAuth` pro Azure AD OAuth) |
| Klienta * |Klienta |identifikátor Hello klienta pro klienta hello Azure AD |
| Cílová skupina * |Cílová skupina |prostředek Hello jste požádali toouse autorizace. Příklad: `https://management.core.windows.net/` |
| Klient ID * |clientId |Hello identifikátor klienta pro aplikaci hello Azure AD |
| Tajný klíč * |tajný klíč |tajný klíč Hello hello klienta, který žádá o hello token |

> [!TIP]
> Můžete použít `securestring` parametr a hello `@parameters()` [funkce definice pracovního postupu](http://aka.ms/logicappdocs) toouse parametr, který nebude možné číst v definici hello po uložení.
> 
> 

Například:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Další kroky
Teď vyzkoušet hello platformy a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md). Můžete prozkoumat hello dalších dostupných konektorů v Logic Apps pohledem na našem [rozhraní API seznamu](apis-list.md).

