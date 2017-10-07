---
title: "aaaException zpracování a chyby protokolování scénář – Azure Logic Apps | Microsoft Docs"
description: "Popisuje případu použití skutečné o pokročilé zpracování výjimek a protokolování chyb pro Azure Logic Apps"
keywords: 
services: logic-apps
author: hedidin
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 63b0b843-f6b0-4d9a-98d0-17500be17385
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/29/2016
ms.author: LADocs; b-hoedid
ms.openlocfilehash: e893a7b652254dca7b8a82398e8afd571f6ccd25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scenario-exception-handling-and-error-logging-for-logic-apps"></a>Scénář: Zpracování výjimek a protokolování chyb pro logic apps

Tento scénář popisuje, jak můžete rozšířit zpracování výjimek toobetter podporu logiku aplikace. Použili jsme použití skutečném případu tooanswer hello otázku: "Azure Logic Apps podporuje výjimky a zpracování chyb?"

> [!NOTE]
> Hello aktuální schéma Azure Logic Apps poskytuje standardní šablona pro akce odpovědi. Tato šablona obsahuje interní ověření a chybové odpovědi vrácená z aplikace API.

## <a name="scenario-and-use-case-overview"></a>Přehled scénáře a použití případu

Tady je hello scénáře jako hello případ použití pro tento scénář: 

Dobře známé zdravotní péče organizace nám pověření toodevelop Azure řešení, které by vytvořit pacienta portálu pomocí Microsoft Dynamics CRM Online. Potřebovali toosend schůzku záznamy mezi hello Dynamics CRM Online pacienta portál a Salesforce. Nemůžeme se zobrazí výzva, toouse hello [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) standard pro všechny pacienta záznamy.

Hello projekt má dva hlavní požadavky:  

* Záznamy a metoda toolog odeslaný hello Dynamics CRM Online portálu
* Způsob tooview chyby, ke kterým došlo v rámci pracovního postupu hello

> [!TIP]
> Souhrnné video o tomto projektu najdete v tématu [skupiny uživatelů integrace](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "skupiny uživatelů integrace").

## <a name="how-we-solved-hello-problem"></a>Jak jsme vyřešit problém hello

Jsme zvolili [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/ "Azure Cosmos DB") jako úložiště pro záznamy protokolu a chyba hello (Cosmos DB odkazuje toorecords jako dokumenty). Azure Logic Apps obsahuje standardní šablonu pro všechny odpovědi, a proto jsme nebude mít toocreate vlastní schéma. Jsme může vytvořit aplikaci API příliš**vložit** a **dotazu** pro záznamy chyba a protokolu. Pro každý v rámci aplikace API hello jsme také může definovat schéma.  

Další požadavky se záznamy toopurge po určitém datu. Cosmos DB má vlastnost s názvem [čas tooLive](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "čas tooLive") (TTL), což povolené nám tooset **čas tooLive** hodnotu pro každý záznam nebo kolekce. Tato funkce eliminovat hello nutné toomanually odstranění záznamů v databázi Cosmos.

> [!IMPORTANT]
> toocomplete tohoto kurzu budete potřebovat toocreate Cosmos DB databáze a dvě kolekce (protokolování a chyby).

## <a name="create-hello-logic-app"></a>Vytvoření aplikace logiky hello

prvním krokem Hello je toocreate hello logiku aplikace a otevřete hello aplikace v návrháři aplikace logiky. V tomto příkladu používáme aplikace logiky nadřazený podřízený. Předpokládejme jsme už máte vytvořené hello nadřazené a budou toocreate jedna podřízené logiku aplikace.

Vzhledem k tomu, že přidáme záznam hello toolog vycházejících z Dynamics CRM Online, Začněme v horní části hello. Musí používáme **požadavku** aktivovat, protože aplikace logiky nadřazené hello aktivuje tohoto dítěte.

### <a name="logic-app-trigger"></a>Aktivační událostí aplikace logiky

Používáme **požadavku** spustit jak je znázorněno v hello následující ukázka:

```` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

````


## <a name="steps"></a>Kroky

Zdroj hello (požadavek) pacienta záznamu hello jsme musíte se přihlásit z hello Dynamics CRM Online portálu.

1. Nový záznam schůzku nám musí získat ze služby Dynamics CRM Online.

   aktivační událost Hello pocházejících z CRM nám poskytuje hello **CRM PatentId**, **typ záznamu**, **nový nebo aktualizovat záznam** (nový nebo aktualizovat logická hodnota), a  **SalesforceId**. Hello **SalesforceId** může mít hodnotu null, protože se používá pouze pro aktualizaci.
   Nemůžeme získat hello CRM záznam pomocí hello CRM **PatientID** a hello **typ záznamu**.

2. Dále je třeba tooadd naše aplikace DocumentDB API **InsertLogEntry** operace, jak je vidět tady v návrháři aplikace logiky.

   **Vložit položky protokolu**

   ![Vložit položky protokolu](media/logic-apps-scenario-error-and-exception-handling/lognewpatient.png)

   **Vložit položku chyby**

   ![Vložit položky protokolu](media/logic-apps-scenario-error-and-exception-handling/insertlogentry.png)

   **Kontrola pro vytvoření záznamu selhání**

   ![Podmínka](media/logic-apps-scenario-error-and-exception-handling/condition.png)

## <a name="logic-app-source-code"></a>Zdrojovém kódu aplikace logiky

> [!NOTE]
> Hello následující příklady jsou pouze ukázky. Protože v tomto kurzu vychází z implementace teď v produkčním prostředí, hello hodnotu **zdrojový uzel** se nemusí zobrazit vlastnosti, které jsou související tooscheduling na schůzku. > 

### <a name="logging"></a>Protokolování

Následující kód aplikace logiky Hello ukázkové ukazuje, jak toohandle protokolování.

#### <a name="log-entry"></a>Položka protokolu

Tady je hello logiku aplikace zdrojový kód pro vložení položka protokolu.

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a>Žádost protokolu

Zde je zpráva požadavku protokolu hello odeslány toohello aplikace API.

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{/"Pragma/":/"no-cache/",/"x-ms-request-id/":/"e750c9a9-bd48-44c4-bbba-1688b6f8a132/",/"OData-Version/":/"4.0/",/"Cache-Control/":/"no-cache/",/"Date/":/"Fri, 10 Jun 2016 22:31:56 GMT/",/"Set-Cookie/":/"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1/",/"Server/":/"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0/",/"X-AspNet-Version/":/"4.0.30319/",/"X-Powered-By/":/"ASP.NET/",/"Content-Length/":/"1935/",/"Content-Type/":/"application/json; odata.metadata=minimal; odata.streaming=true/",/"Expires/":/"-1/"}"
        }
    }

```


#### <a name="log-response"></a>Odpověď protokolu

Zde je zpráva odpovědi protokolu hello z aplikace API hello.

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "/"0400fc2f-0000-0000-0000-575b3ff00000/"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{/"Pragma/":/"no-cache/",/"x-ms-request-id/":/"e750c9a9-bd48-44c4-bbba-1688b6f8a132/",/"OData-Version/":/"4.0/",/"Cache-Control/":/"no-cache/",/"Date/":/"Fri, 10 Jun 2016 22:31:56 GMT/",/"Set-Cookie/":/"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1/",/"Server/":/"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0/",/"X-AspNet-Version/":/"4.0.30319/",/"X-Powered-By/":/"ASP.NET/",/"Content-Length/":/"1935/",/"Content-Type/":/"application/json; odata.metadata=minimal; odata.streaming=true/",/"Expires/":/"-1/"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

Nyní Podíváme se na zpracování kroky hello chyb.

### <a name="error-handling"></a>Zpracování chyb

Hello následující ukázka kódu aplikace logiky ukazuje, jak můžete implementovat zpracování chyb.

#### <a name="create-error-record"></a>Vytvořit záznam chyby

Tady je zdrojovém kódu aplikace hello logiku pro vytvoření záznam chyby.

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}             
```

#### <a name="insert-error-into-cosmos-db--request"></a>Chyba vložit do DB Cosmos – požadavku

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed toocomplete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{/"Account_Class_vod__c/":/"PRAC/",/"Account_Status_MED__c/":/"I/",/"CRM_HUB_ID__c/":/"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0/",/"Credentials_vod__c/",/"DTC_ID_MED__c/":/"/",/"Fax/":/"/",/"FirstName/":/"A/",/"Gender_vod__c/":/"/",/"IMS_ID__c/":/"/",/"LastName/":/"BAILEY/",/"MasterID_mp__c/":/"/",/"C_ID_MED__c/":/"851588/",/"Middle_vod__c/":/"/",/"NPI_vod__c/":/"/",/"PDRP_MED__c/":false,/"PersonDoNotCall/":false,/"PersonEmail/":/"/",/"PersonHasOptedOutOfEmail/":false,/"PersonHasOptedOutOfFax/":false,/"PersonMobilePhone/":/"/",/"Phone/":/"/",/"Practicing_Specialty__c/":/"FM - FAMILY MEDICINE/",/"Primary_City__c/":/"/",/"Primary_State__c/":/"/",/"Primary_Street_Line2__c/":/"/",/"Primary_Street__c/":/"/",/"Primary_Zip__c/":/"/",/"RecordTypeId/":/"012U0000000JaPWIA0/",/"Request_Date__c/":/"2016-06-10T22:31:55.9647467Z/",/"ONY_ID__c/":/"/",/"Specialty_1_vod__c/":/"/",/"Suffix_vod__c/":/"/",/"Website/":/"/"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-cosmos-db--response"></a>Chyba vložit do databáze Cosmos – odpověď

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "/"0c00eaac-0000-0000-0000-575b3fdc0000/"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed toocomplete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{/"Account_Class_vod__c/":/"PRAC/",/"Account_Status_MED__c/":/"I/",/"CRM_HUB_ID__c/":/"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0/",/"Credentials_vod__c/":/"DO - Degree level is DO/",/"DTC_ID_MED__c/":/"/",/"Fax/":/"/",/"FirstName/":/"A/",/"Gender_vod__c/":/"/",/"IMS_ID__c/":/"/",/"LastName/":/"BAILEY/",/"MterID_mp__c/":/"/",/"Medicis_ID_MED__c/":/"851588/",/"Middle_vod__c/":/"/",/"NPI_vod__c/":/"/",/"PDRP_MED__c/":false,/"PersonDoNotCall/":false,/"PersonEmail/":/"/",/"PersonHasOptedOutOfEmail/":false,/"PersonHasOptedOutOfFax/":false,/"PersonMobilePhone/":/"/",/"Phone/":/"/",/"Practicing_Specialty__c/":/"FM - FAMILY MEDICINE/",/"Primary_City__c/":/"/",/"Primary_State__c/":/"/",/"Primary_Street_Line2__c/":/"/",/"Primary_Street__c/":/"/",/"Primary_Zip__c/":/"/",/"RecordTypeId/":/"012U0000000JaPWIA0/",/"Request_Date__c/":/"2016-06-10T22:31:55.9647467Z/",/"XXXXXXX/":/"/",/"Specialty_1_vod__c/":/"/",/"Suffix_vod__c/":/"/",/"Website/":/"/"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a>Salesforce chybové odpovědi

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed toocomplete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="return-hello-response-back-tooparent-logic-app"></a>Vrátí hello odpověď zpět tooparent logiku aplikace

Po získání odpovědi hello můžete předat hello odpověď zpět toohello nadřazené logiku aplikace.

#### <a name="return-success-response-tooparent-logic-app"></a>Vrátí aplikace logiky tooparent úspěšné odpovědi

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "    Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-tooparent-logic-app"></a>Vrátí aplikace logiky tooparent chybové odpovědi

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="cosmos-db-repository-and-portal"></a>Úložiště cosmos DB a portálu

Naše řešení možnosti s přidané [Cosmos DB](https://azure.microsoft.com/services/documentdb).

### <a name="error-management-portal"></a>Portál pro správu chyby

chyby hello tooview, můžete vytvořit MVC webové aplikace toodisplay hello chyba záznamy z databáze Cosmos. Hello **seznamu**, **podrobnosti**, **upravit**, a **odstranit** operace jsou zahrnuté v aktuální verzi hello.

> [!NOTE]
> Upravit operace: Cosmos DB nahrazuje hello celý dokument. Hello záznamy zobrazené na hello **seznamu** a **podrobností** zobrazení jsou pouze vzorky. Nejsou skutečné pacienta schůzku záznamy.

Zde jsou příklady naše aplikace MVC podrobnosti vytvořili s hello dříve popsané přístup.

#### <a name="error-management-list"></a>Seznam chyb správy
![Seznam chyb](media/logic-apps-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a>Zobrazení podrobností chyb správy
![Podrobnosti o chybě](media/logic-apps-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a>Portál pro správu protokolu

protokoly hello tooview, jsme také vytvořit webové aplikace MVC. Zde jsou příklady naše aplikace MVC podrobnosti vytvořili s hello dříve popsané přístup.

#### <a name="sample-log-detail-view"></a>Zobrazení podrobností protokolu ukázka
![Zobrazení podrobností protokolu](media/logic-apps-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a>Podrobnosti o aplikaci API

#### <a name="logic-apps-exception-management-api"></a>Rozhraní API správy výjimky aplikace logiky

Naše open-source Azure Logic Apps Výjimka rozhraní API pro správu aplikací poskytuje funkci podle postupu popsaného tady – existují dva řadiče:

* **ErrorController** vloží záznam chyby (dokument) v kolekci DocumentDB.
* **LogController** vloží záznam protokolu (dokument) v kolekci DocumentDB.

> [!TIP]
> Použít oba řadiče `async Task<dynamic>` operace a operace tooresolve za běhu, takže jsme můžete vytvořit hello schématu DocumentDB v textu hello hello operace. 
> 

Každému dokumentu v DocumentDB musí mít jedinečné ID. Používáme `PatientId` a přidání časového razítka, která je převést hodnotu časového razítka Unix tooa (double). Jsme zkrátit hello hodnota tooremove hello desetinnou hodnotu.

Můžete zobrazit zdrojový kód hello chyba kontroleru rozhraní API [z Githubu](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).

Říkáme hello rozhraní API z aplikace logiky pomocí hello následující syntaxi:

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

Hello výrazu v hello předcházející kontroly ukázkový kód pro hello *Create_NewPatientRecord* stav **se nezdařilo**.

## <a name="summary"></a>Souhrn

* Můžete snadno implementovat protokolování a zpracování chyb v aplikaci logiky.
* DocumentDB můžete použít jako úložiště hello záznamů protokolu a chyby (dokumentů).
* Můžete použít MVC toocreate portálu toodisplay protokolu a záznamy o chybách.

### <a name="source-code"></a>Zdrojový kód

zdrojový kód Hello hello správu výjimek aplikace logiky aplikace rozhraní API je k dispozici v tomto [úložiště GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "rozhraní API pro správu aplikace logiky aplikace výjimka").

## <a name="next-steps"></a>Další kroky

* [Zobrazit další logiku aplikace příkladů a scénářů](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Další informace o sledování aplikací logiky](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Vytvoření šablony pro automatické nasazení pro logic apps](../logic-apps/logic-apps-create-deploy-template.md)
