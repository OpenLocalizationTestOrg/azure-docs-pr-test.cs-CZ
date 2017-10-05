---
title: "Zpracovávání výjimek v jazyce & scénář protokolování chyb – Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 044de27c75da93c95609110d2b73336c42f746fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scenario-exception-handling-and-error-logging-for-logic-apps"></a><span data-ttu-id="e587b-103">Scénář: Zpracování výjimek a protokolování chyb pro logic apps</span><span class="sxs-lookup"><span data-stu-id="e587b-103">Scenario: Exception handling and error logging for logic apps</span></span>

<span data-ttu-id="e587b-104">Tento scénář popisuje, jak můžete rozšířit aplikace logiky lepší podpory výjimek.</span><span class="sxs-lookup"><span data-stu-id="e587b-104">This scenario describes how you can extend a logic app to better support exception handling.</span></span> <span data-ttu-id="e587b-105">Jste použili jsme případu použití reálnými odpověď na otázku: "Azure Logic Apps podporuje výjimky a zpracování chyb?"</span><span class="sxs-lookup"><span data-stu-id="e587b-105">We've used a real-life use case to answer the question: "Does Azure Logic Apps support exception and error handling?"</span></span>

> [!NOTE]
> <span data-ttu-id="e587b-106">Aktuální schéma Azure Logic Apps poskytuje standardní šablona pro akce odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e587b-106">The current Azure Logic Apps schema provides a standard template for action responses.</span></span> <span data-ttu-id="e587b-107">Tato šablona obsahuje interní ověření a chybové odpovědi vrácená z aplikace API.</span><span class="sxs-lookup"><span data-stu-id="e587b-107">This template includes both internal validation and error responses returned from an API app.</span></span>

## <a name="scenario-and-use-case-overview"></a><span data-ttu-id="e587b-108">Přehled scénáře a použití případu</span><span class="sxs-lookup"><span data-stu-id="e587b-108">Scenario and use case overview</span></span>

<span data-ttu-id="e587b-109">Tady je článek jako případ použití pro tento scénář:</span><span class="sxs-lookup"><span data-stu-id="e587b-109">Here's the story as the use case for this scenario:</span></span> 

<span data-ttu-id="e587b-110">Dobře známé zdravotní péče organizace pověření nám vyvíjet Azure řešení, které by vytvořit pacienta portálu pomocí Microsoft Dynamics CRM Online.</span><span class="sxs-lookup"><span data-stu-id="e587b-110">A well-known healthcare organization engaged us to develop an Azure solution that would create a patient portal by using Microsoft Dynamics CRM Online.</span></span> <span data-ttu-id="e587b-111">Potřebovali odeslat schůzku záznamy mezi Dynamics CRM Online pacienta portál a Salesforce.</span><span class="sxs-lookup"><span data-stu-id="e587b-111">They needed to send appointment records between the Dynamics CRM Online patient portal and Salesforce.</span></span> <span data-ttu-id="e587b-112">Jsme se zobrazí dotaz, použít [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) standard pro všechny pacienta záznamy.</span><span class="sxs-lookup"><span data-stu-id="e587b-112">We were asked to use the [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) standard for all patient records.</span></span>

<span data-ttu-id="e587b-113">Projekt má dva hlavní požadavky:</span><span class="sxs-lookup"><span data-stu-id="e587b-113">The project had two major requirements:</span></span>  

* <span data-ttu-id="e587b-114">Metoda na protokolování záznamů odeslaných z portálu pro Dynamics CRM Online</span><span class="sxs-lookup"><span data-stu-id="e587b-114">A method to log records sent from the Dynamics CRM Online portal</span></span>
* <span data-ttu-id="e587b-115">Způsob, jak zobrazit chyby, ke kterým došlo v rámci pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="e587b-115">A way to view any errors that occurred within the workflow</span></span>

> [!TIP]
> <span data-ttu-id="e587b-116">Souhrnné video o tomto projektu najdete v tématu [skupiny uživatelů integrace](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "Integration User Group").</span><span class="sxs-lookup"><span data-stu-id="e587b-116">For a high-level video about this project, see [Integration User Group](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "Integration User Group").</span></span>

## <a name="how-we-solved-the-problem"></a><span data-ttu-id="e587b-117">Jak jsme problém byl</span><span class="sxs-lookup"><span data-stu-id="e587b-117">How we solved the problem</span></span>

<span data-ttu-id="e587b-118">Jsme zvolili [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/ "Azure Cosmos DB") jako úložiště pro záznamy protokolu a chyby (Cosmos DB odkazuje na záznamy jako dokumenty).</span><span class="sxs-lookup"><span data-stu-id="e587b-118">We chose [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/ "Azure Cosmos DB") As a repository for the log and error records (Cosmos DB refers to records as documents).</span></span> <span data-ttu-id="e587b-119">Azure Logic Apps obsahuje standardní šablonu pro všechny odpovědi, a proto jsme nebude muset vytvořit vlastní schéma.</span><span class="sxs-lookup"><span data-stu-id="e587b-119">Because Azure Logic Apps has a standard template for all responses, we would not have to create a custom schema.</span></span> <span data-ttu-id="e587b-120">Vytvoříme může aplikace API k **vložit** a **dotazu** pro záznamy chyba a protokolu.</span><span class="sxs-lookup"><span data-stu-id="e587b-120">We could create an API app to **Insert** and **Query** for both error and log records.</span></span> <span data-ttu-id="e587b-121">Může také definujeme schéma pro jednotlivé aplikace API.</span><span class="sxs-lookup"><span data-stu-id="e587b-121">We could also define a schema for each within the API app.</span></span>  

<span data-ttu-id="e587b-122">Další požadavky se k vyprázdnění záznamů po určitém datu.</span><span class="sxs-lookup"><span data-stu-id="e587b-122">Another requirement was to purge records after a certain date.</span></span> <span data-ttu-id="e587b-123">Cosmos DB má vlastnost s názvem [TTL](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "TTL") (TTL), což nám nastavit povolené **TTL** hodnotu pro každý záznam nebo kolekce.</span><span class="sxs-lookup"><span data-stu-id="e587b-123">Cosmos DB has a property called [Time to Live](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Time to Live") (TTL), which allowed us to set a **Time to Live** value for each record or collection.</span></span> <span data-ttu-id="e587b-124">Tato funkce eliminovat potřeba ručně odstranit záznamy v databázi Cosmos.</span><span class="sxs-lookup"><span data-stu-id="e587b-124">This capability eliminated the need to manually delete records in Cosmos DB.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e587b-125">K dokončení tohoto kurzu, musíte vytvořit databázi Cosmos DB a dvě kolekce (protokolování a chyby).</span><span class="sxs-lookup"><span data-stu-id="e587b-125">To complete this tutorial, you need to create a Cosmos DB database and two collections (Logging and Errors).</span></span>

## <a name="create-the-logic-app"></a><span data-ttu-id="e587b-126">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="e587b-126">Create the logic app</span></span>

<span data-ttu-id="e587b-127">Prvním krokem je vytvoření aplikace logiky a otevřete aplikaci v návrháři aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="e587b-127">The first step is to create the logic app and open the app in Logic App Designer.</span></span> <span data-ttu-id="e587b-128">V tomto příkladu používáme aplikace logiky nadřazený podřízený.</span><span class="sxs-lookup"><span data-stu-id="e587b-128">In this example, we are using parent-child logic apps.</span></span> <span data-ttu-id="e587b-129">Předpokládejme, že jsme už máte vytvořené nadřazené a se chystáte vytvořit jednu aplikaci logiky podřízené.</span><span class="sxs-lookup"><span data-stu-id="e587b-129">Let's assume that we have already created the parent and are going to create one child logic app.</span></span>

<span data-ttu-id="e587b-130">Vzhledem k tomu, že přidáme protokolu záznam vycházejících z Dynamics CRM Online, Začněme v horní části.</span><span class="sxs-lookup"><span data-stu-id="e587b-130">Because we are going to log the record coming out of Dynamics CRM Online, let's start at the top.</span></span> <span data-ttu-id="e587b-131">Musí používáme **požadavku** aktivovat, protože aplikace logiky nadřazené aktivuje tohoto dítěte.</span><span class="sxs-lookup"><span data-stu-id="e587b-131">We must use a **Request** trigger because the parent logic app triggers this child.</span></span>

### <a name="logic-app-trigger"></a><span data-ttu-id="e587b-132">Aktivační událostí aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="e587b-132">Logic app trigger</span></span>

<span data-ttu-id="e587b-133">Používáme **požadavku** spustit jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="e587b-133">We are using a **Request** trigger as shown in the following example:</span></span>

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


## <a name="steps"></a><span data-ttu-id="e587b-134">Kroky</span><span class="sxs-lookup"><span data-stu-id="e587b-134">Steps</span></span>

<span data-ttu-id="e587b-135">Zdroj (požadavek) pacienta záznamu jsme musíte se přihlásit z portálu Dynamics CRM Online.</span><span class="sxs-lookup"><span data-stu-id="e587b-135">We must log the source (request) of the patient record from the Dynamics CRM Online portal.</span></span>

1. <span data-ttu-id="e587b-136">Nový záznam schůzku nám musí získat ze služby Dynamics CRM Online.</span><span class="sxs-lookup"><span data-stu-id="e587b-136">We must get a new appointment record from Dynamics CRM Online.</span></span>

   <span data-ttu-id="e587b-137">Aktivační události pocházející z CRM poskytuje nám s **CRM PatentId**, **typ záznamu**, **nový nebo aktualizovat záznam** (nový nebo aktualizovat logická hodnota), a **SalesforceId**.</span><span class="sxs-lookup"><span data-stu-id="e587b-137">The trigger coming from CRM provides us with the **CRM PatentId**, **record type**, **New or Updated Record** (new or update Boolean value), and **SalesforceId**.</span></span> <span data-ttu-id="e587b-138">**SalesforceId** může mít hodnotu null, protože se používá pouze pro aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="e587b-138">The **SalesforceId** can be null because it's only used for an update.</span></span>
   <span data-ttu-id="e587b-139">Se nám získat záznam CRM pomocí aplikace CRM **PatientID** a **typ záznamu**.</span><span class="sxs-lookup"><span data-stu-id="e587b-139">We get the CRM record by using the CRM **PatientID** and the **Record Type**.</span></span>

2. <span data-ttu-id="e587b-140">Dále je potřeba přidat naše aplikace DocumentDB API **InsertLogEntry** operace, jak je vidět tady v návrháři aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="e587b-140">Next, we need to add our DocumentDB API app **InsertLogEntry** operation as shown here in Logic App Designer.</span></span>

   <span data-ttu-id="e587b-141">**Vložit položky protokolu**</span><span class="sxs-lookup"><span data-stu-id="e587b-141">**Insert log entry**</span></span>

   ![Vložit položky protokolu](media/logic-apps-scenario-error-and-exception-handling/lognewpatient.png)

   <span data-ttu-id="e587b-143">**Vložit položku chyby**</span><span class="sxs-lookup"><span data-stu-id="e587b-143">**Insert error entry**</span></span>

   ![Vložit položky protokolu](media/logic-apps-scenario-error-and-exception-handling/insertlogentry.png)

   <span data-ttu-id="e587b-145">**Kontrola pro vytvoření záznamu selhání**</span><span class="sxs-lookup"><span data-stu-id="e587b-145">**Check for create record failure**</span></span>

   ![Podmínka](media/logic-apps-scenario-error-and-exception-handling/condition.png)

## <a name="logic-app-source-code"></a><span data-ttu-id="e587b-147">Zdrojovém kódu aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="e587b-147">Logic app source code</span></span>

> [!NOTE]
> <span data-ttu-id="e587b-148">Následující příklady jsou pouze ukázky.</span><span class="sxs-lookup"><span data-stu-id="e587b-148">The following examples are samples only.</span></span> <span data-ttu-id="e587b-149">Protože v tomto kurzu vychází z implementace teď v produkčním prostředí, hodnotu **zdrojový uzel** se nemusí zobrazit vlastnosti, které se vztahují k plánování na schůzku. ></span><span class="sxs-lookup"><span data-stu-id="e587b-149">Because this tutorial is based on an implementation now in production, the value of a **Source Node** might not display properties that are related to scheduling an appointment.></span></span> 

### <a name="logging"></a><span data-ttu-id="e587b-150">Protokolování</span><span class="sxs-lookup"><span data-stu-id="e587b-150">Logging</span></span>

<span data-ttu-id="e587b-151">Následující ukázka kódu aplikace logiky ukazuje způsob zpracování protokolování.</span><span class="sxs-lookup"><span data-stu-id="e587b-151">The following logic app code sample shows how to handle logging.</span></span>

#### <a name="log-entry"></a><span data-ttu-id="e587b-152">Položka protokolu</span><span class="sxs-lookup"><span data-stu-id="e587b-152">Log entry</span></span>

<span data-ttu-id="e587b-153">Tady je zdrojovém kódu aplikace logiky pro vkládání položka protokolu.</span><span class="sxs-lookup"><span data-stu-id="e587b-153">Here is the logic app source code for inserting a log entry.</span></span>

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

#### <a name="log-request"></a><span data-ttu-id="e587b-154">Žádost protokolu</span><span class="sxs-lookup"><span data-stu-id="e587b-154">Log request</span></span>

<span data-ttu-id="e587b-155">Zde je zpráva protokolu žádost odeslat do aplikace API.</span><span class="sxs-lookup"><span data-stu-id="e587b-155">Here is the log request message posted to the API app.</span></span>

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


#### <a name="log-response"></a><span data-ttu-id="e587b-156">Odpověď protokolu</span><span class="sxs-lookup"><span data-stu-id="e587b-156">Log response</span></span>

<span data-ttu-id="e587b-157">Zde je zpráva odpovědi protokolu z aplikace API.</span><span class="sxs-lookup"><span data-stu-id="e587b-157">Here is the log response message from the API app.</span></span>

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

<span data-ttu-id="e587b-158">Nyní Podíváme se na zpracování kroky chyb.</span><span class="sxs-lookup"><span data-stu-id="e587b-158">Now let's look at the error handling steps.</span></span>

### <a name="error-handling"></a><span data-ttu-id="e587b-159">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="e587b-159">Error handling</span></span>

<span data-ttu-id="e587b-160">Následující ukázka kódu aplikace logiky ukazuje, jak můžete implementovat zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="e587b-160">The following logic app code sample shows how you can implement error handling.</span></span>

#### <a name="create-error-record"></a><span data-ttu-id="e587b-161">Vytvořit záznam chyby</span><span class="sxs-lookup"><span data-stu-id="e587b-161">Create error record</span></span>

<span data-ttu-id="e587b-162">Tady je zdrojovém kódu aplikace logiky pro vytvoření záznam chyby.</span><span class="sxs-lookup"><span data-stu-id="e587b-162">Here is the logic app source code for creating an error record.</span></span>

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

#### <a name="insert-error-into-cosmos-db--request"></a><span data-ttu-id="e587b-163">Chyba vložit do DB Cosmos – požadavku</span><span class="sxs-lookup"><span data-stu-id="e587b-163">Insert error into Cosmos DB--request</span></span>

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{/"Account_Class_vod__c/":/"PRAC/",/"Account_Status_MED__c/":/"I/",/"CRM_HUB_ID__c/":/"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0/",/"Credentials_vod__c/",/"DTC_ID_MED__c/":/"/",/"Fax/":/"/",/"FirstName/":/"A/",/"Gender_vod__c/":/"/",/"IMS_ID__c/":/"/",/"LastName/":/"BAILEY/",/"MasterID_mp__c/":/"/",/"C_ID_MED__c/":/"851588/",/"Middle_vod__c/":/"/",/"NPI_vod__c/":/"/",/"PDRP_MED__c/":false,/"PersonDoNotCall/":false,/"PersonEmail/":/"/",/"PersonHasOptedOutOfEmail/":false,/"PersonHasOptedOutOfFax/":false,/"PersonMobilePhone/":/"/",/"Phone/":/"/",/"Practicing_Specialty__c/":/"FM - FAMILY MEDICINE/",/"Primary_City__c/":/"/",/"Primary_State__c/":/"/",/"Primary_Street_Line2__c/":/"/",/"Primary_Street__c/":/"/",/"Primary_Zip__c/":/"/",/"RecordTypeId/":/"012U0000000JaPWIA0/",/"Request_Date__c/":/"2016-06-10T22:31:55.9647467Z/",/"ONY_ID__c/":/"/",/"Specialty_1_vod__c/":/"/",/"Suffix_vod__c/":/"/",/"Website/":/"/"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-cosmos-db--response"></a><span data-ttu-id="e587b-164">Chyba vložit do databáze Cosmos – odpověď</span><span class="sxs-lookup"><span data-stu-id="e587b-164">Insert error into Cosmos DB--response</span></span>

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
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
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

#### <a name="salesforce-error-response"></a><span data-ttu-id="e587b-165">Salesforce chybové odpovědi</span><span class="sxs-lookup"><span data-stu-id="e587b-165">Salesforce error response</span></span>

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
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="return-the-response-back-to-parent-logic-app"></a><span data-ttu-id="e587b-166">Vrátí odpověď zpět do nadřazené aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="e587b-166">Return the response back to parent logic app</span></span>

<span data-ttu-id="e587b-167">Po získání odpovědi můžete předat odpověď zpět do aplikace logiky nadřazené.</span><span class="sxs-lookup"><span data-stu-id="e587b-167">After you get the response, you can pass the response back to the parent logic app.</span></span>

#### <a name="return-success-response-to-parent-logic-app"></a><span data-ttu-id="e587b-168">Vrátí úspěšná odpověď do nadřazené logiku aplikace</span><span class="sxs-lookup"><span data-stu-id="e587b-168">Return success response to parent logic app</span></span>

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

#### <a name="return-error-response-to-parent-logic-app"></a><span data-ttu-id="e587b-169">Vrátí chybové odpovědi do nadřazené logiku aplikace</span><span class="sxs-lookup"><span data-stu-id="e587b-169">Return error response to parent logic app</span></span>

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


## <a name="cosmos-db-repository-and-portal"></a><span data-ttu-id="e587b-170">Úložiště cosmos DB a portálu</span><span class="sxs-lookup"><span data-stu-id="e587b-170">Cosmos DB repository and portal</span></span>

<span data-ttu-id="e587b-171">Naše řešení možnosti s přidané [Cosmos DB](https://azure.microsoft.com/services/documentdb).</span><span class="sxs-lookup"><span data-stu-id="e587b-171">Our solution added capabilities with [Cosmos DB](https://azure.microsoft.com/services/documentdb).</span></span>

### <a name="error-management-portal"></a><span data-ttu-id="e587b-172">Portál pro správu chyby</span><span class="sxs-lookup"><span data-stu-id="e587b-172">Error management portal</span></span>

<span data-ttu-id="e587b-173">Chcete-li zobrazit chyby, můžete vytvořit webové aplikace MVC zobrazí chyba záznamy z databáze Cosmos.</span><span class="sxs-lookup"><span data-stu-id="e587b-173">To view the errors, you can create an MVC web app to display the error records from Cosmos DB.</span></span> <span data-ttu-id="e587b-174">**Seznamu**, **podrobnosti**, **upravit**, a **odstranit** operace jsou zahrnuté v aktuální verzi.</span><span class="sxs-lookup"><span data-stu-id="e587b-174">The **List**, **Details**, **Edit**, and **Delete** operations are included in the current version.</span></span>

> [!NOTE]
> <span data-ttu-id="e587b-175">Upravit operace: Cosmos DB nahradí celý dokument.</span><span class="sxs-lookup"><span data-stu-id="e587b-175">Edit operation: Cosmos DB replaces the entire document.</span></span> <span data-ttu-id="e587b-176">Záznamy zobrazené na **seznamu** a **podrobností** zobrazení jsou pouze vzorky.</span><span class="sxs-lookup"><span data-stu-id="e587b-176">The records shown in the **List** and **Detail** views are samples only.</span></span> <span data-ttu-id="e587b-177">Nejsou skutečné pacienta schůzku záznamy.</span><span class="sxs-lookup"><span data-stu-id="e587b-177">They are not actual patient appointment records.</span></span>

<span data-ttu-id="e587b-178">Zde jsou příklady naše podrobnosti o aplikaci MVC vytvořené pomocí výše popsaných přístup.</span><span class="sxs-lookup"><span data-stu-id="e587b-178">Here are examples of our MVC app details created with the previously described approach.</span></span>

#### <a name="error-management-list"></a><span data-ttu-id="e587b-179">Seznam chyb správy</span><span class="sxs-lookup"><span data-stu-id="e587b-179">Error management list</span></span>
![Seznam chyb](media/logic-apps-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a><span data-ttu-id="e587b-181">Zobrazení podrobností chyb správy</span><span class="sxs-lookup"><span data-stu-id="e587b-181">Error management detail view</span></span>
![Podrobnosti o chybě](media/logic-apps-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a><span data-ttu-id="e587b-183">Portál pro správu protokolu</span><span class="sxs-lookup"><span data-stu-id="e587b-183">Log management portal</span></span>

<span data-ttu-id="e587b-184">K zobrazení protokolů, také jsme vytvořili webové aplikace MVC.</span><span class="sxs-lookup"><span data-stu-id="e587b-184">To view the logs, we also created an MVC web app.</span></span> <span data-ttu-id="e587b-185">Zde jsou příklady naše podrobnosti o aplikaci MVC vytvořené pomocí výše popsaných přístup.</span><span class="sxs-lookup"><span data-stu-id="e587b-185">Here are examples of our MVC app details created with the previously described approach.</span></span>

#### <a name="sample-log-detail-view"></a><span data-ttu-id="e587b-186">Zobrazení podrobností protokolu ukázka</span><span class="sxs-lookup"><span data-stu-id="e587b-186">Sample log detail view</span></span>
![Zobrazení podrobností protokolu](media/logic-apps-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a><span data-ttu-id="e587b-188">Podrobnosti o aplikaci API</span><span class="sxs-lookup"><span data-stu-id="e587b-188">API app details</span></span>

#### <a name="logic-apps-exception-management-api"></a><span data-ttu-id="e587b-189">Rozhraní API správy výjimky aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="e587b-189">Logic Apps exception management API</span></span>

<span data-ttu-id="e587b-190">Naše open-source Azure Logic Apps Výjimka rozhraní API pro správu aplikací poskytuje funkci podle postupu popsaného tady – existují dva řadiče:</span><span class="sxs-lookup"><span data-stu-id="e587b-190">Our open-source Azure Logic Apps exception management API app provides functionality as described here - there are two controllers:</span></span>

* <span data-ttu-id="e587b-191">**ErrorController** vloží záznam chyby (dokument) v kolekci DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="e587b-191">**ErrorController** inserts an error record (document) in a DocumentDB collection.</span></span>
* <span data-ttu-id="e587b-192">**LogController** vloží záznam protokolu (dokument) v kolekci DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="e587b-192">**LogController** Inserts a log record (document) in a DocumentDB collection.</span></span>

> [!TIP]
> <span data-ttu-id="e587b-193">Použít oba řadiče `async Task<dynamic>` operace a operace přeložit za běhu, takže jsme můžete vytvořit schéma DocumentDB v těle operace.</span><span class="sxs-lookup"><span data-stu-id="e587b-193">Both controllers use `async Task<dynamic>` operations, allowing operations to resolve at runtime, so we can create the DocumentDB schema in the body of the operation.</span></span> 
> 

<span data-ttu-id="e587b-194">Každému dokumentu v DocumentDB musí mít jedinečné ID.</span><span class="sxs-lookup"><span data-stu-id="e587b-194">Every document in DocumentDB must have a unique ID.</span></span> <span data-ttu-id="e587b-195">Používáme `PatientId` a přidání časové razítko, které jsou převedeny na hodnotu časového razítka systému Unix (double).</span><span class="sxs-lookup"><span data-stu-id="e587b-195">We are using `PatientId` and adding a timestamp that is converted to a Unix timestamp value (double).</span></span> <span data-ttu-id="e587b-196">Jsme zkrátit hodnota odebrat desetinnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e587b-196">We truncate the value to remove the fractional value.</span></span>

<span data-ttu-id="e587b-197">Můžete zobrazit zdrojový kód chyby kontroleru rozhraní API [z Githubu](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).</span><span class="sxs-lookup"><span data-stu-id="e587b-197">You can view the source code of our error controller API [from GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).</span></span>

<span data-ttu-id="e587b-198">Jsme volání rozhraní API z aplikace logiky pomocí následující syntaxe:</span><span class="sxs-lookup"><span data-stu-id="e587b-198">We call the API from a logic app by using the following syntax:</span></span>

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

<span data-ttu-id="e587b-199">Vyhledá výraz v předchozím příkladu kódu *Create_NewPatientRecord* stav **se nezdařilo**.</span><span class="sxs-lookup"><span data-stu-id="e587b-199">The expression in the preceding code sample checks for the *Create_NewPatientRecord* status of **Failed**.</span></span>

## <a name="summary"></a><span data-ttu-id="e587b-200">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e587b-200">Summary</span></span>

* <span data-ttu-id="e587b-201">Můžete snadno implementovat protokolování a zpracování chyb v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="e587b-201">You can easily implement logging and error handling in a logic app.</span></span>
* <span data-ttu-id="e587b-202">DocumentDB slouží jako úložiště pro záznamy protokolu a chyby (dokumentů).</span><span class="sxs-lookup"><span data-stu-id="e587b-202">You can use DocumentDB as the repository for log and error records (documents).</span></span>
* <span data-ttu-id="e587b-203">MVC slouží k vytvoření portálu zobrazit záznamy protokolu a chyby.</span><span class="sxs-lookup"><span data-stu-id="e587b-203">You can use MVC to create a portal to display log and error records.</span></span>

### <a name="source-code"></a><span data-ttu-id="e587b-204">Zdrojový kód</span><span class="sxs-lookup"><span data-stu-id="e587b-204">Source code</span></span>

<span data-ttu-id="e587b-205">Zdrojový kód pro správu výjimek aplikace logiky aplikace rozhraní API je k dispozici v tomto [úložiště GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "rozhraní API pro správu aplikace logiky aplikace výjimka").</span><span class="sxs-lookup"><span data-stu-id="e587b-205">The source code for the Logic Apps exception management API application is available in this [GitHub repository](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logic App Exception Management API").</span></span>

## <a name="next-steps"></a><span data-ttu-id="e587b-206">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e587b-206">Next steps</span></span>

* [<span data-ttu-id="e587b-207">Zobrazit další logiku aplikace příkladů a scénářů</span><span class="sxs-lookup"><span data-stu-id="e587b-207">View more logic app examples and scenarios</span></span>](../logic-apps/logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="e587b-208">Další informace o sledování aplikací logiky</span><span class="sxs-lookup"><span data-stu-id="e587b-208">Learn about monitoring logic apps</span></span>](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [<span data-ttu-id="e587b-209">Vytvoření šablony pro automatické nasazení pro logic apps</span><span class="sxs-lookup"><span data-stu-id="e587b-209">Create automated deployment templates for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
