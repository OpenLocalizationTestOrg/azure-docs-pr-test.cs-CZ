---
title: "Koncové body REST aaaCall pomocí protokolu HTTP + Swagger konektor pro Azure Logic Apps | Microsoft Docs"
description: "Připojit tooREST koncových bodů z aplikace logiky prostřednictvím Swagger s hello HTTP + Swagger konektoru"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: eccfd87c-c5fe-4cf7-b564-9752775fd667
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: baaa57689ff41fcd052f9d86086e36619ddec46e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-http--swagger-action"></a>Začínáme s hello HTTP + Swagger akce

Můžete vytvořit koncový bod prvotřídní konektor tooany REST prostřednictvím [dokumentu Swagger](https://swagger.io) při použití hello HTTP + Swagger akce v pracovním postupu logiku aplikace. Můžete také rozšířit logiku aplikace toocall žádný koncový bod REST prvotřídní prostředí návrhář aplikace logiky.

jak zjistit, aplikace logiky toocreate s konektory, toolearn [vytvoření nové aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>Použít protokol HTTP + Swagger jako aktivační událost nebo akci.

Vítejte HTTP + Swagger aktivační události a akce pracovní hello stejné jako hello [akce HTTP](connectors-native-http.md) , ale poskytuje lepší prostředí v návrháři aplikace logiky díky zpřístupnění strukturu hello rozhraní API a výstupy z hello [Swagger metadata](https://swagger.io) . Můžete také použít hello HTTP + Swagger konektor jako trigger. Pokud chcete, aby tooimplement cyklického dotazování aktivační událost, postupujte podle hello dotazování vzor, který je popsán v [vytvořit vlastní toocall rozhraní API pro ostatní rozhraní API, služeb a systémů z aplikace logiky](../logic-apps/logic-apps-create-api-app.md#polling-triggers).

Další informace o [logiku aplikace triggery a akce](connectors-overview.md).

Tady je příklad, jak toouse hello HTTP + Swagger operace jako akce v pracovním postupu v aplikaci logiky.

1. Vyberte hello **nový krok** tlačítko.
2. Vyberte **přidat akci**.
3. Hello akce vyhledávacího pole zadejte **swagger** toolist hello HTTP + Swagger akce.
   
    ![Vyberte HTTP + Swagger akce](./media/connectors-native-http-swagger/using-action-1.png)
4. Zadejte adresu URL hello dokumentem Swagger:
   
   * toowork z hello návrhář aplikace na základě logiky, hello adresa URL musí být koncový bod HTTPS a povolení CORS.
   * Pokud dokumentu Swagger hello nesplňuje tento požadavek, můžete použít [Azure Storage s povolením CORS](#hosting-swagger-from-storage) toostore hello dokumentu.
5. Klikněte na tlačítko **Další** tooread a vykreslování z hello dokumentu Swagger.
6. Přidejte do žádné parametry, které jsou požadovány pro volání hello protokolu HTTP.
   
    ![Dokončení akce HTTP](./media/connectors-native-http-swagger/using-action-2.png)
7. toosave a publikovat aplikace logiky, klikněte na tlačítko **Uložit** na panelu nástrojů návrháře.

### <a name="host-swagger-from-azure-storage"></a>Swagger hostitele ze služby Azure Storage
Můžete chtít tooreference dokumentu Swagger, který není hostovaný nebo který nesplňuje požadavky zabezpečení a cross-origin hello hello Designer. tooresolve-li tento problém, můžete ukládat hello dokumentu Swagger ve službě Azure Storage a povolení CORS tooreference hello dokumentu.  

Tady jsou kroky toocreate hello, konfigurace a ukládat dokumenty Swagger ve službě Azure Storage:

1. [Vytvoření účtu úložiště Azure s Azure Blob storage](../storage/common/storage-create-storage-account.md). tooperform tento krok, nastavte oprávnění příliš**veřejný přístup**.

2. Povolení CORS u objektu blob hello. 

   tooautomatically nakonfigurujte toto nastavení, můžete použít [tento skript prostředí PowerShell](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).

3. Nahrajte objekt blob toohello hello Swagger souboru. 

   Tento krok můžete provést z hello [portál Azure](https://portal.azure.com) nebo z nástroje, jako je [Azure Storage Explorer](http://storageexplorer.com/).

4. Referenční dokument HTTPS odkaz toohello v Azure Blob storage. 

   odkaz Hello používá tento formát:

   `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`

## <a name="technical-details"></a>Technické podrobnosti
Následují hello podrobnosti o hello triggery a akce, které tento HTTP + Swagger konektor podporuje.

## <a name="http--swagger-triggers"></a>HTTP + aktivační události Swagger
Aktivační událost je událost, která lze použít toostart hello workflow, který je definován v aplikaci logiky. [Další informace o aktivační události.](connectors-overview.md) Hello HTTP + Swagger konektor má jedna aktivační událost.

| Aktivační události | Popis |
| --- | --- |
| HTTP + Swagger |Ujistěte se, volání protokolu HTTP a vrátí obsah odpovědi hello |

## <a name="http--swagger-actions"></a>HTTP + Swagger akce
Akce je operace, která se provádí v pracovním postupu hello, která je definována v aplikaci logiky. [Další informace o akcích.](connectors-overview.md) Hello HTTP + Swagger konektor má jednu možné akci.

| Akce | Popis |
| --- | --- |
| HTTP + Swagger |Ujistěte se, volání protokolu HTTP a vrátí obsah odpovědi hello |

### <a name="action-details"></a>Podrobnosti akce
Hello HTTP + Swagger konektor se dodává s možné jednu akci. Dále najdete informace o jednotlivých hello akcí, jejich povinné a nepovinné vstupní pole a hello odpovídající výstup podrobnosti, které jsou spojeny s jejich využití.

#### <a name="http--swagger"></a>HTTP + Swagger
Pomoc při metadat Swagger Zkontrolujte odchozí požadavku HTTP.
Znak hvězdičky (*) znamená povinné pole.

| Zobrazované jméno | Název vlastnosti | Popis |
| --- | --- | --- |
| Metoda * |– Metoda |Toouse operaci HTTP. |
| IDENTIFIKÁTOR URI * |identifikátor URI |Identifikátor URI pro požadavek hello protokolu HTTP. |
| Záhlaví |Záhlaví |Objekt JSON tooinclude hlavičky protokolu HTTP. |
| Tělo |Text |Hello požadavku HTTP. |
| Authentication |Ověřování |Ověřování toouse pro požadavek. Další informace najdete v tématu hello [HTTP konektor](connectors-native-http.md#authentication). |

**Podrobnosti o výstupu**

Odpověď HTTP

| Název vlastnosti | Datový typ | Popis |
| --- | --- | --- |
| Záhlaví |Objekt |Hlavičky odpovědi |
| Tělo |Objekt |Objekt odpovědi |
| Stavový kód |celá čísla |Stavový kód protokolu HTTP |

### <a name="http-responses"></a>Odpovědi HTTP
Při provádění akce toovarious volání, může být určité odpovědi. Následuje tabulka, která popisuje odpovídající odpovědi a popisy.

| Name (Název) | Popis |
| --- | --- |
| 200 |OK |
| 202 |Přijmout |
| 400 |Chybný požadavek |
| 401 |Neautorizováno |
| 403 |Je zakázané |
| 404 |Nebyl nalezen |
| 500 |Vnitřní chybu serveru. Došlo k neznámé chybě. |

- - -
## <a name="next-steps"></a>Další kroky

* [Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md)
* [Vyhledání jiných konektorů](apis-list.md)