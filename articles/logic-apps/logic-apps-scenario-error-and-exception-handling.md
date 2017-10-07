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
# <a name="scenario-exception-handling-and-error-logging-for-logic-apps"></a><span data-ttu-id="62bfd-103">Scénář: Zpracování výjimek a protokolování chyb pro logic apps</span><span class="sxs-lookup"><span data-stu-id="62bfd-103">Scenario: Exception handling and error logging for logic apps</span></span>

<span data-ttu-id="62bfd-104">Tento scénář popisuje, jak můžete rozšířit zpracování výjimek toobetter podporu logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="62bfd-104">This scenario describes how you can extend a logic app toobetter support exception handling.</span></span> <span data-ttu-id="62bfd-105">Použili jsme použití skutečném případu tooanswer hello otázku: "Azure Logic Apps podporuje výjimky a zpracování chyb?"</span><span class="sxs-lookup"><span data-stu-id="62bfd-105">We've used a real-life use case tooanswer hello question: "Does Azure Logic Apps support exception and error handling?"</span></span>

> [!NOTE]
> <span data-ttu-id="62bfd-106">Hello aktuální schéma Azure Logic Apps poskytuje standardní šablona pro akce odpovědi.</span><span class="sxs-lookup"><span data-stu-id="62bfd-106">hello current Azure Logic Apps schema provides a standard template for action responses.</span></span> <span data-ttu-id="62bfd-107">Tato šablona obsahuje interní ověření a chybové odpovědi vrácená z aplikace API.</span><span class="sxs-lookup"><span data-stu-id="62bfd-107">This template includes both internal validation and error responses returned from an API app.</span></span>

## <a name="scenario-and-use-case-overview"></a><span data-ttu-id="62bfd-108">Přehled scénáře a použití případu</span><span class="sxs-lookup"><span data-stu-id="62bfd-108">Scenario and use case overview</span></span>

<span data-ttu-id="62bfd-109">Tady je hello scénáře jako hello případ použití pro tento scénář:</span><span class="sxs-lookup"><span data-stu-id="62bfd-109">Here's hello story as hello use case for this scenario:</span></span> 

<span data-ttu-id="62bfd-110">Dobře známé zdravotní péče organizace nám pověření toodevelop Azure řešení, které by vytvořit pacienta portálu pomocí Microsoft Dynamics CRM Online.</span><span class="sxs-lookup"><span data-stu-id="62bfd-110">A well-known healthcare organization engaged us toodevelop an Azure solution that would create a patient portal by using Microsoft Dynamics CRM Online.</span></span> <span data-ttu-id="62bfd-111">Potřebovali toosend schůzku záznamy mezi hello Dynamics CRM Online pacienta portál a Salesforce.</span><span class="sxs-lookup"><span data-stu-id="62bfd-111">They needed toosend appointment records between hello Dynamics CRM Online patient portal and Salesforce.</span></span> <span data-ttu-id="62bfd-112">Nemůžeme se zobrazí výzva, toouse hello [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) standard pro všechny pacienta záznamy.</span><span class="sxs-lookup"><span data-stu-id="62bfd-112">We were asked toouse hello [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) standard for all patient records.</span></span>

<span data-ttu-id="62bfd-113">Hello projekt má dva hlavní požadavky:</span><span class="sxs-lookup"><span data-stu-id="62bfd-113">hello project had two major requirements:</span></span>  

* <span data-ttu-id="62bfd-114">Záznamy a metoda toolog odeslaný hello Dynamics CRM Online portálu</span><span class="sxs-lookup"><span data-stu-id="62bfd-114">A method toolog records sent from hello Dynamics CRM Online portal</span></span>
* <span data-ttu-id="62bfd-115">Způsob tooview chyby, ke kterým došlo v rámci pracovního postupu hello</span><span class="sxs-lookup"><span data-stu-id="62bfd-115">A way tooview any errors that occurred within hello workflow</span></span>

> [!TIP]
> <span data-ttu-id="62bfd-116">Souhrnné video o tomto projektu najdete v tématu [skupiny uživatelů integrace](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "skupiny uživatelů integrace").</span><span class="sxs-lookup"><span data-stu-id="62bfd-116">For a high-level video about this project, see [Integration User Group](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "Integration User Group").</span></span>

## <a name="how-we-solved-hello-problem"></a><span data-ttu-id="62bfd-117">Jak jsme vyřešit problém hello</span><span class="sxs-lookup"><span data-stu-id="62bfd-117">How we solved hello problem</span></span>

<span data-ttu-id="62bfd-118">Jsme zvolili [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/ "Azure Cosmos DB") jako úložiště pro záznamy protokolu a chyba hello (Cosmos DB odkazuje toorecords jako dokumenty).</span><span class="sxs-lookup"><span data-stu-id="62bfd-118">We chose [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/ "Azure Cosmos DB") As a repository for hello log and error records (Cosmos DB refers toorecords as documents).</span></span> <span data-ttu-id="62bfd-119">Azure Logic Apps obsahuje standardní šablonu pro všechny odpovědi, a proto jsme nebude mít toocreate vlastní schéma.</span><span class="sxs-lookup"><span data-stu-id="62bfd-119">Because Azure Logic Apps has a standard template for all responses, we would not have toocreate a custom schema.</span></span> <span data-ttu-id="62bfd-120">Jsme může vytvořit aplikaci API příliš**vložit** a **dotazu** pro záznamy chyba a protokolu.</span><span class="sxs-lookup"><span data-stu-id="62bfd-120">We could create an API app too**Insert** and **Query** for both error and log records.</span></span> <span data-ttu-id="62bfd-121">Pro každý v rámci aplikace API hello jsme také může definovat schéma.</span><span class="sxs-lookup"><span data-stu-id="62bfd-121">We could also define a schema for each within hello API app.</span></span>  

<span data-ttu-id="62bfd-122">Další požadavky se záznamy toopurge po určitém datu.</span><span class="sxs-lookup"><span data-stu-id="62bfd-122">Another requirement was toopurge records after a certain date.</span></span> <span data-ttu-id="62bfd-123">Cosmos DB má vlastnost s názvem [čas tooLive](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "čas tooLive") (TTL), což povolené nám tooset **čas tooLive** hodnotu pro každý záznam nebo kolekce.</span><span class="sxs-lookup"><span data-stu-id="62bfd-123">Cosmos DB has a property called [Time tooLive](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Time tooLive") (TTL), which allowed us tooset a **Time tooLive** value for each record or collection.</span></span> <span data-ttu-id="62bfd-124">Tato funkce eliminovat hello nutné toomanually odstranění záznamů v databázi Cosmos.</span><span class="sxs-lookup"><span data-stu-id="62bfd-124">This capability eliminated hello need toomanually delete records in Cosmos DB.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="62bfd-125">toocomplete tohoto kurzu budete potřebovat toocreate Cosmos DB databáze a dvě kolekce (protokolování a chyby).</span><span class="sxs-lookup"><span data-stu-id="62bfd-125">toocomplete this tutorial, you need toocreate a Cosmos DB database and two collections (Logging and Errors).</span></span>

## <a name="create-hello-logic-app"></a><span data-ttu-id="62bfd-126">Vytvoření aplikace logiky hello</span><span class="sxs-lookup"><span data-stu-id="62bfd-126">Create hello logic app</span></span>

<span data-ttu-id="62bfd-127">prvním krokem Hello je toocreate hello logiku aplikace a otevřete hello aplikace v návrháři aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="62bfd-127">hello first step is toocreate hello logic app and open hello app in Logic App Designer.</span></span> <span data-ttu-id="62bfd-128">V tomto příkladu používáme aplikace logiky nadřazený podřízený.</span><span class="sxs-lookup"><span data-stu-id="62bfd-128">In this example, we are using parent-child logic apps.</span></span> <span data-ttu-id="62bfd-129">Předpokládejme jsme už máte vytvořené hello nadřazené a budou toocreate jedna podřízené logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="62bfd-129">Let's assume that we have already created hello parent and are going toocreate one child logic app.</span></span>

<span data-ttu-id="62bfd-130">Vzhledem k tomu, že přidáme záznam hello toolog vycházejících z Dynamics CRM Online, Začněme v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="62bfd-130">Because we are going toolog hello record coming out of Dynamics CRM Online, let's start at hello top.</span></span> <span data-ttu-id="62bfd-131">Musí používáme **požadavku** aktivovat, protože aplikace logiky nadřazené hello aktivuje tohoto dítěte.</span><span class="sxs-lookup"><span data-stu-id="62bfd-131">We must use a **Request** trigger because hello parent logic app triggers this child.</span></span>

### <a name="logic-app-trigger"></a><span data-ttu-id="62bfd-132">Aktivační událostí aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="62bfd-132">Logic app trigger</span></span>

<span data-ttu-id="62bfd-133">Používáme **požadavku** spustit jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="62bfd-133">We are using a **Request** trigger as shown in hello following example:</span></span>

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


## <a name="steps"></a><span data-ttu-id="62bfd-134">Kroky</span><span class="sxs-lookup"><span data-stu-id="62bfd-134">Steps</span></span>

<span data-ttu-id="62bfd-135">Zdroj hello (požadavek) pacienta záznamu hello jsme musíte se přihlásit z hello Dynamics CRM Online portálu.</span><span class="sxs-lookup"><span data-stu-id="62bfd-135">We must log hello source (request) of hello patient record from hello Dynamics CRM Online portal.</span></span>

1. <span data-ttu-id="62bfd-136">Nový záznam schůzku nám musí získat ze služby Dynamics CRM Online.</span><span class="sxs-lookup"><span data-stu-id="62bfd-136">We must get a new appointment record from Dynamics CRM Online.</span></span>

   <span data-ttu-id="62bfd-137">aktivační událost Hello pocházejících z CRM nám poskytuje hello **CRM PatentId**, **typ záznamu**, **nový nebo aktualizovat záznam** (nový nebo aktualizovat logická hodnota), a  **SalesforceId**.</span><span class="sxs-lookup"><span data-stu-id="62bfd-137">hello trigger coming from CRM provides us with hello **CRM PatentId**, **record type**, **New or Updated Record** (new or update Boolean value), and **SalesforceId**.</span></span> <span data-ttu-id="62bfd-138">Hello **SalesforceId** může mít hodnotu null, protože se používá pouze pro aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="62bfd-138">hello **SalesforceId** can be null because it's only used for an update.</span></span>
   <span data-ttu-id="62bfd-139">Nemůžeme získat hello CRM záznam pomocí hello CRM **PatientID** a hello **typ záznamu**.</span><span class="sxs-lookup"><span data-stu-id="62bfd-139">We get hello CRM record by using hello CRM **PatientID** and hello **Record Type**.</span></span>

2. <span data-ttu-id="62bfd-140">Dále je třeba tooadd naše aplikace DocumentDB API **InsertLogEntry** operace, jak je vidět tady v návrháři aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="62bfd-140">Next, we need tooadd our DocumentDB API app **InsertLogEntry** operation as shown here in Logic App Designer.</span></span>

   <span data-ttu-id="62bfd-141">**Vložit položky protokolu**</span><span class="sxs-lookup"><span data-stu-id="62bfd-141">**Insert log entry**</span></span>

   ![Vložit položky protokolu](media/logic-apps-scenario-error-and-exception-handling/lognewpatient.png)

   <span data-ttu-id="62bfd-143">**Vložit položku chyby**</span><span class="sxs-lookup"><span data-stu-id="62bfd-143">**Insert error entry**</span></span>

   ![Vložit položky protokolu](media/logic-apps-scenario-error-and-exception-handling/insertlogentry.png)

   <span data-ttu-id="62bfd-145">**Kontrola pro vytvoření záznamu selhání**</span><span class="sxs-lookup"><span data-stu-id="62bfd-145">**Check for create record failure**</span></span>

   ![Podmínka](media/logic-apps-scenario-error-and-exception-handling/condition.png)

## <a name="logic-app-source-code"></a><span data-ttu-id="62bfd-147">Zdrojovém kódu aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="62bfd-147">Logic app source code</span></span>

> [!NOTE]
> <span data-ttu-id="62bfd-148">Hello následující příklady jsou pouze ukázky.</span><span class="sxs-lookup"><span data-stu-id="62bfd-148">hello following examples are samples only.</span></span> <span data-ttu-id="62bfd-149">Protože v tomto kurzu vychází z implementace teď v produkčním prostředí, hello hodnotu **zdrojový uzel** se nemusí zobrazit vlastnosti, které jsou související tooscheduling na schůzku. ></span><span class="sxs-lookup"><span data-stu-id="62bfd-149">Because this tutorial is based on an implementation now in production, hello value of a **Source Node** might not display properties that are related tooscheduling an appointment.></span></span> 

### <a name="logging"></a><span data-ttu-id="62bfd-150">Protokolování</span><span class="sxs-lookup"><span data-stu-id="62bfd-150">Logging</span></span>

<span data-ttu-id="62bfd-151">Následující kód aplikace logiky Hello ukázkové ukazuje, jak toohandle protokolování.</span><span class="sxs-lookup"><span data-stu-id="62bfd-151">hello following logic app code sample shows how toohandle logging.</span></span>

#### <a name="log-entry"></a><span data-ttu-id="62bfd-152">Položka protokolu</span><span class="sxs-lookup"><span data-stu-id="62bfd-152">Log entry</span></span>

<span data-ttu-id="62bfd-153">Tady je hello logiku aplikace zdrojový kód pro vložení položka protokolu.</span><span class="sxs-lookup"><span data-stu-id="62bfd-153">Here is hello logic app source code for inserting a log entry.</span></span>

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

#### <a name="log-request"></a><span data-ttu-id="62bfd-154">Žádost protokolu</span><span class="sxs-lookup"><span data-stu-id="62bfd-154">Log request</span></span>

<span data-ttu-id="62bfd-155">Zde je zpráva požadavku protokolu hello odeslány toohello aplikace API.</span><span class="sxs-lookup"><span data-stu-id="62bfd-155">Here is hello log request message posted toohello API app.</span></span>

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


#### <a name="log-response"></a><span data-ttu-id="62bfd-156">Odpověď protokolu</span><span class="sxs-lookup"><span data-stu-id="62bfd-156">Log response</span></span>

<span data-ttu-id="62bfd-157">Zde je zpráva odpovědi protokolu hello z aplikace API hello.</span><span class="sxs-lookup"><span data-stu-id="62bfd-157">Here is hello log response message from hello API app.</span></span>

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

<span data-ttu-id="62bfd-158">Nyní Podíváme se na zpracování kroky hello chyb.</span><span class="sxs-lookup"><span data-stu-id="62bfd-158">Now let's look at hello error handling steps.</span></span>

### <a name="error-handling"></a><span data-ttu-id="62bfd-159">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="62bfd-159">Error handling</span></span>

<span data-ttu-id="62bfd-160">Hello následující ukázka kódu aplikace logiky ukazuje, jak můžete implementovat zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="62bfd-160">hello following logic app code sample shows how you can implement error handling.</span></span>

#### <a name="create-error-record"></a><span data-ttu-id="62bfd-161">Vytvořit záznam chyby</span><span class="sxs-lookup"><span data-stu-id="62bfd-161">Create error record</span></span>

<span data-ttu-id="62bfd-162">Tady je zdrojovém kódu aplikace hello logiku pro vytvoření záznam chyby.</span><span class="sxs-lookup"><span data-stu-id="62bfd-162">Here is hello logic app source code for creating an error record.</span></span>

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

#### <a name="insert-error-into-cosmos-db--request"></a><span data-ttu-id="62bfd-163">Chyba vložit do DB Cosmos – požadavku</span><span class="sxs-lookup"><span data-stu-id="62bfd-163">Insert error into Cosmos DB--request</span></span>

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

#### <a name="insert-error-into-cosmos-db--response"></a><span data-ttu-id="62bfd-164">Chyba vložit do databáze Cosmos – odpověď</span><span class="sxs-lookup"><span data-stu-id="62bfd-164">Insert error into Cosmos DB--response</span></span>

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

#### <a name="salesforce-error-response"></a><span data-ttu-id="62bfd-165">Salesforce chybové odpovědi</span><span class="sxs-lookup"><span data-stu-id="62bfd-165">Salesforce error response</span></span>

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

### <a name="return-hello-response-back-tooparent-logic-app"></a><span data-ttu-id="62bfd-166">Vrátí hello odpověď zpět tooparent logiku aplikace</span><span class="sxs-lookup"><span data-stu-id="62bfd-166">Return hello response back tooparent logic app</span></span>

<span data-ttu-id="62bfd-167">Po získání odpovědi hello můžete předat hello odpověď zpět toohello nadřazené logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="62bfd-167">After you get hello response, you can pass hello response back toohello parent logic app.</span></span>

#### <a name="return-success-response-tooparent-logic-app"></a><span data-ttu-id="62bfd-168">Vrátí aplikace logiky tooparent úspěšné odpovědi</span><span class="sxs-lookup"><span data-stu-id="62bfd-168">Return success response tooparent logic app</span></span>

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

#### <a name="return-error-response-tooparent-logic-app"></a><span data-ttu-id="62bfd-169">Vrátí aplikace logiky tooparent chybové odpovědi</span><span class="sxs-lookup"><span data-stu-id="62bfd-169">Return error response tooparent logic app</span></span>

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


## <a name="cosmos-db-repository-and-portal"></a><span data-ttu-id="62bfd-170">Úložiště cosmos DB a portálu</span><span class="sxs-lookup"><span data-stu-id="62bfd-170">Cosmos DB repository and portal</span></span>

<span data-ttu-id="62bfd-171">Naše řešení možnosti s přidané [Cosmos DB](https://azure.microsoft.com/services/documentdb).</span><span class="sxs-lookup"><span data-stu-id="62bfd-171">Our solution added capabilities with [Cosmos DB](https://azure.microsoft.com/services/documentdb).</span></span>

### <a name="error-management-portal"></a><span data-ttu-id="62bfd-172">Portál pro správu chyby</span><span class="sxs-lookup"><span data-stu-id="62bfd-172">Error management portal</span></span>

<span data-ttu-id="62bfd-173">chyby hello tooview, můžete vytvořit MVC webové aplikace toodisplay hello chyba záznamy z databáze Cosmos.</span><span class="sxs-lookup"><span data-stu-id="62bfd-173">tooview hello errors, you can create an MVC web app toodisplay hello error records from Cosmos DB.</span></span> <span data-ttu-id="62bfd-174">Hello **seznamu**, **podrobnosti**, **upravit**, a **odstranit** operace jsou zahrnuté v aktuální verzi hello.</span><span class="sxs-lookup"><span data-stu-id="62bfd-174">hello **List**, **Details**, **Edit**, and **Delete** operations are included in hello current version.</span></span>

> [!NOTE]
> <span data-ttu-id="62bfd-175">Upravit operace: Cosmos DB nahrazuje hello celý dokument.</span><span class="sxs-lookup"><span data-stu-id="62bfd-175">Edit operation: Cosmos DB replaces hello entire document.</span></span> <span data-ttu-id="62bfd-176">Hello záznamy zobrazené na hello **seznamu** a **podrobností** zobrazení jsou pouze vzorky.</span><span class="sxs-lookup"><span data-stu-id="62bfd-176">hello records shown in hello **List** and **Detail** views are samples only.</span></span> <span data-ttu-id="62bfd-177">Nejsou skutečné pacienta schůzku záznamy.</span><span class="sxs-lookup"><span data-stu-id="62bfd-177">They are not actual patient appointment records.</span></span>

<span data-ttu-id="62bfd-178">Zde jsou příklady naše aplikace MVC podrobnosti vytvořili s hello dříve popsané přístup.</span><span class="sxs-lookup"><span data-stu-id="62bfd-178">Here are examples of our MVC app details created with hello previously described approach.</span></span>

#### <a name="error-management-list"></a><span data-ttu-id="62bfd-179">Seznam chyb správy</span><span class="sxs-lookup"><span data-stu-id="62bfd-179">Error management list</span></span>
![Seznam chyb](media/logic-apps-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a><span data-ttu-id="62bfd-181">Zobrazení podrobností chyb správy</span><span class="sxs-lookup"><span data-stu-id="62bfd-181">Error management detail view</span></span>
![Podrobnosti o chybě](media/logic-apps-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a><span data-ttu-id="62bfd-183">Portál pro správu protokolu</span><span class="sxs-lookup"><span data-stu-id="62bfd-183">Log management portal</span></span>

<span data-ttu-id="62bfd-184">protokoly hello tooview, jsme také vytvořit webové aplikace MVC.</span><span class="sxs-lookup"><span data-stu-id="62bfd-184">tooview hello logs, we also created an MVC web app.</span></span> <span data-ttu-id="62bfd-185">Zde jsou příklady naše aplikace MVC podrobnosti vytvořili s hello dříve popsané přístup.</span><span class="sxs-lookup"><span data-stu-id="62bfd-185">Here are examples of our MVC app details created with hello previously described approach.</span></span>

#### <a name="sample-log-detail-view"></a><span data-ttu-id="62bfd-186">Zobrazení podrobností protokolu ukázka</span><span class="sxs-lookup"><span data-stu-id="62bfd-186">Sample log detail view</span></span>
![Zobrazení podrobností protokolu](media/logic-apps-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a><span data-ttu-id="62bfd-188">Podrobnosti o aplikaci API</span><span class="sxs-lookup"><span data-stu-id="62bfd-188">API app details</span></span>

#### <a name="logic-apps-exception-management-api"></a><span data-ttu-id="62bfd-189">Rozhraní API správy výjimky aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="62bfd-189">Logic Apps exception management API</span></span>

<span data-ttu-id="62bfd-190">Naše open-source Azure Logic Apps Výjimka rozhraní API pro správu aplikací poskytuje funkci podle postupu popsaného tady – existují dva řadiče:</span><span class="sxs-lookup"><span data-stu-id="62bfd-190">Our open-source Azure Logic Apps exception management API app provides functionality as described here - there are two controllers:</span></span>

* <span data-ttu-id="62bfd-191">**ErrorController** vloží záznam chyby (dokument) v kolekci DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="62bfd-191">**ErrorController** inserts an error record (document) in a DocumentDB collection.</span></span>
* <span data-ttu-id="62bfd-192">**LogController** vloží záznam protokolu (dokument) v kolekci DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="62bfd-192">**LogController** Inserts a log record (document) in a DocumentDB collection.</span></span>

> [!TIP]
> <span data-ttu-id="62bfd-193">Použít oba řadiče `async Task<dynamic>` operace a operace tooresolve za běhu, takže jsme můžete vytvořit hello schématu DocumentDB v textu hello hello operace.</span><span class="sxs-lookup"><span data-stu-id="62bfd-193">Both controllers use `async Task<dynamic>` operations, allowing operations tooresolve at runtime, so we can create hello DocumentDB schema in hello body of hello operation.</span></span> 
> 

<span data-ttu-id="62bfd-194">Každému dokumentu v DocumentDB musí mít jedinečné ID.</span><span class="sxs-lookup"><span data-stu-id="62bfd-194">Every document in DocumentDB must have a unique ID.</span></span> <span data-ttu-id="62bfd-195">Používáme `PatientId` a přidání časového razítka, která je převést hodnotu časového razítka Unix tooa (double).</span><span class="sxs-lookup"><span data-stu-id="62bfd-195">We are using `PatientId` and adding a timestamp that is converted tooa Unix timestamp value (double).</span></span> <span data-ttu-id="62bfd-196">Jsme zkrátit hello hodnota tooremove hello desetinnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="62bfd-196">We truncate hello value tooremove hello fractional value.</span></span>

<span data-ttu-id="62bfd-197">Můžete zobrazit zdrojový kód hello chyba kontroleru rozhraní API [z Githubu](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).</span><span class="sxs-lookup"><span data-stu-id="62bfd-197">You can view hello source code of our error controller API [from GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).</span></span>

<span data-ttu-id="62bfd-198">Říkáme hello rozhraní API z aplikace logiky pomocí hello následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="62bfd-198">We call hello API from a logic app by using hello following syntax:</span></span>

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

<span data-ttu-id="62bfd-199">Hello výrazu v hello předcházející kontroly ukázkový kód pro hello *Create_NewPatientRecord* stav **se nezdařilo**.</span><span class="sxs-lookup"><span data-stu-id="62bfd-199">hello expression in hello preceding code sample checks for hello *Create_NewPatientRecord* status of **Failed**.</span></span>

## <a name="summary"></a><span data-ttu-id="62bfd-200">Souhrn</span><span class="sxs-lookup"><span data-stu-id="62bfd-200">Summary</span></span>

* <span data-ttu-id="62bfd-201">Můžete snadno implementovat protokolování a zpracování chyb v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="62bfd-201">You can easily implement logging and error handling in a logic app.</span></span>
* <span data-ttu-id="62bfd-202">DocumentDB můžete použít jako úložiště hello záznamů protokolu a chyby (dokumentů).</span><span class="sxs-lookup"><span data-stu-id="62bfd-202">You can use DocumentDB as hello repository for log and error records (documents).</span></span>
* <span data-ttu-id="62bfd-203">Můžete použít MVC toocreate portálu toodisplay protokolu a záznamy o chybách.</span><span class="sxs-lookup"><span data-stu-id="62bfd-203">You can use MVC toocreate a portal toodisplay log and error records.</span></span>

### <a name="source-code"></a><span data-ttu-id="62bfd-204">Zdrojový kód</span><span class="sxs-lookup"><span data-stu-id="62bfd-204">Source code</span></span>

<span data-ttu-id="62bfd-205">zdrojový kód Hello hello správu výjimek aplikace logiky aplikace rozhraní API je k dispozici v tomto [úložiště GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "rozhraní API pro správu aplikace logiky aplikace výjimka").</span><span class="sxs-lookup"><span data-stu-id="62bfd-205">hello source code for hello Logic Apps exception management API application is available in this [GitHub repository](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logic App Exception Management API").</span></span>

## <a name="next-steps"></a><span data-ttu-id="62bfd-206">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62bfd-206">Next steps</span></span>

* [<span data-ttu-id="62bfd-207">Zobrazit další logiku aplikace příkladů a scénářů</span><span class="sxs-lookup"><span data-stu-id="62bfd-207">View more logic app examples and scenarios</span></span>](../logic-apps/logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="62bfd-208">Další informace o sledování aplikací logiky</span><span class="sxs-lookup"><span data-stu-id="62bfd-208">Learn about monitoring logic apps</span></span>](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [<span data-ttu-id="62bfd-209">Vytvoření šablony pro automatické nasazení pro logic apps</span><span class="sxs-lookup"><span data-stu-id="62bfd-209">Create automated deployment templates for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
