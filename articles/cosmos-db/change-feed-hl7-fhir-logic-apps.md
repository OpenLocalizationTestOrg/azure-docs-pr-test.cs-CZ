---
title: "aaaChange kanálu pro prostředky HL7 FHIR - Azure Cosmos DB | Microsoft Docs"
description: "Zjistěte, jak změnit tooset až oznámení pro záznamy HL7 FHIR pacientů zdravotní péče pomocí Azure Logic Apps, Azure Cosmos DB a služby Service Bus."
keywords: hl7 fhir
services: cosmos-db
author: hedidin
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 0d25c11f-9197-419a-aa19-4614c6ab2d06
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: b-hoedid
ms.openlocfilehash: d2809bf5c6d8c193c49438d20684c56caea646bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="notifying-patients-of-hl7-fhir-health-care-record-changes-using-logic-apps-and-azure-cosmos-db"></a>Upozornění pacientů HL7 FHIR zdravotní péče záznam změn pomocí Logic Apps a Azure Cosmos DB

Azure MVP Howard Edidin byla nedávno kontaktovat zdravotní péče organizaci, která chtěli tooadd nové funkce tootheir pacienta portálu. Budou potřeba toopatients toosend oznámení, když byla aktualizována jejich stavu záznamů a jejich potřeby pacientů toobe možné toosubscribe toothese aktualizace. 

Tento článek vás provede řešení informačního kanálu oznámení změn hello vytvořené pro tuto zdravotní péče organizaci, která používá databázi Cosmos Azure Logic Apps a Service Bus. 

## <a name="project-requirements"></a>Požadavky na projektu
- Zprostředkovatelé send že hl7 konsolidované klinické dokumentu architektura (C-CDA) dokumenty ve formátu XML. C – CDA dokumenty zahrnovat téměř každý typ klinické dokumentu, včetně klinické dokumenty jako rodiny historie a záznamy o očkování, stejně jako správce, pracovní postup a finanční dokumenty. 
- C – CDA dokumenty se převedou příliš[HL7 FHIR prostředky](http://hl7.org/fhir/2017Jan/resourcelist.html) ve formátu JSON.
- Upravené dokumenty prostředků FHIR odesílá e-mailu ve formátu JSON.

## <a name="solution-workflow"></a>Pracovní postup řešení 

Na vysoké úrovni vyžaduje hello projektu hello následující kroky pracovního postupu: 
1. Převeďte C-CDA dokumenty tooFHIR prostředky.
2. Proveďte opakované aktivační událost dotazování pro upravené FHIR prostředky. 
2. Volání vlastní aplikaci, FhirNotificationApi, tooconnect tooAzure Cosmos DB a dotazů pro nové nebo upravené dokumenty.
3. Uložte fronty Service Bus tootoohello hello odpovědi.
4. Dotazování pro nové zprávy ve frontě Service Bus hello.
5. Odešlete toopatients e-mailové oznámení.

## <a name="solution-architecture"></a>Architektura řešení
Toto řešení vyžaduje tři hello toomeet Logic Apps vyšší požadavky a pracovní postup řešení dokončení hello. Hello tři aplikace logiky jsou:
1. **Aplikace HL7. FHIR mapování**: obdrží hello HL7 C-CDA dokumentu, ho transformuje toohello FHIR prostředků a pak uloží ho tooAzure Cosmos DB.
2. **Aplikace EHR**: dotazuje hello Azure Cosmos DB FHIR úložiště a uloží fronty Service Bus tooa hello odpovědi. Tato aplikace logiky používá [aplikace API](#api-app) tooretrieve nové a změněné dokumenty.
3. **Proces oznámení aplikace**: odešle e-mail s oznámením s hello FHIR prostředků dokumenty v textu hello.

![použité v řešení zdravotní péče HL7 FHIR Hello tři Logic Apps](./media/change-feed-hl7-fhir-logic-apps/health-care-solution-hl7-fhir.png)



### <a name="azure-services-used-in-hello-solution"></a>Použít v hello řešení služby Azure

#### <a name="azure-cosmos-db-documentdb-api"></a>Azure Cosmos databáze DocumentDB rozhraní API
Azure Cosmos DB je hello úložiště pro hello FHIR prostředky, jak ukazuje následující obrázek hello.

![účet Azure Cosmos DB Hello použitý v tomto kurzu HL7 FHIR zdravotní péče](./media/change-feed-hl7-fhir-logic-apps/account.png)

#### <a name="logic-apps"></a>Logic Apps
Aplikace logiky zpracovávat hello pracovní postup. Hello následující snímky obrazovky ukazují hello logiku aplikace vytvořené pro toto řešení. 


1. **Aplikace HL7. FHIR mapování**: přijímat hello HL7 C-CDA dokumentu a transformují je tooan FHIR prostředků pomocí hello Enterprise integrační balíček pro Logic Apps. Hello Enterprise integračního balíčku zpracovává hello mapování z hello C-CDA tooFHIR prostředky.

    ![Hello aplikace logiky použít tooreceive HL7 FHIR zdravotní péče záznamů](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-json-transform.png)


2. **Aplikace EHR**: dotazování úložiště Azure Cosmos DB FHIR hello a uložte hello odpovědi tooa Service Bus ve frontě. Hello kód pro aplikaci GetNewOrModifiedFHIRDocuments hello je níže.

    ![tooquery Hello používá aplikace logiky Azure Cosmos DB](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-api-app.png)

3. **Proces oznámení aplikace**: Odeslat oznámení e-mailu s hello FHIR prostředků dokumenty v textu hello.

    ![Hello aplikace logiky, která odešle pacienta e-mailu pomocí hello HL7 FHIR prostředků v textu hello](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-send-email.png)

#### <a name="service-bus"></a>Service Bus
Následující obrázek ukazuje hello pacientů fronty Hello. Hodnota vlastnosti značky Hello se používá pro předmět e-mailu hello.

![Hello použili v tomto kurzu HL7 FHIR frontou Service Bus](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-service-bus-queue.png)

<a id="api-app"></a>

#### <a name="api-app"></a>Aplikace API
Aplikace API připojí tooAzure Cosmos DB a dotazů pro nové nebo upravené dokumenty FHIR podle typů prostředků. Tato aplikace má jeden řadič **FhirNotificationApi** s jednu operaci **GetNewOrModifiedFhirDocuments**, najdete v části [zdroje pro aplikaci API](#api-app-source).

Používáme hello [ `CreateDocumentChangeFeedQuery` ](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) třídy z hello Azure Cosmos databáze DocumentDB .NET API. Další informace najdete v tématu hello [změnu kanálu článku](change-feed.md). 

##### <a name="getnewormodifiedfhirdocuments-operation"></a>Operace GetNewOrModifiedFhirDocuments

**Vstupy**
- Hodnotu DatabaseId
- CollectionId
- Název typu prostředku FHIR HL7
- Logická hodnota: Začít od začátku
- Int: Počet vrácených dokumentů

**Výstupy**
- Úspěch: Stavový kód: odpovědi 200,: seznam dokumentů (pole JSON)
- Chyba: Stavový kód: odpovědi 404,: "nalezeny žádné dokumenty pro '*název prostředku '* typ prostředku"

<a id="api-app-source"></a>

**Zdroj pro aplikace hello rozhraní API**

```C#

    using System.Collections.Generic;
    using System.Linq;
    using System.Net;
    using System.Net.Http;
    using System.Threading.Tasks;
    using System.Web.Http;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Swashbuckle.Swagger.Annotations;
    using TRex.Metadata;
    
    namespace FhirNotificationApi.Controllers
    {
        /// <summary>
        ///     FHIR Resource Type Controller
        /// </summary>
        /// <seealso cref="System.Web.Http.ApiController" />
        public class FhirResourceTypeController : ApiController
        {
            /// <summary>
            ///     Gets hello new or modified FHIR documents from Last Run Date 
            ///     or create date of hello collection
            /// </summary>
            /// <param name="databaseId"></param>
            /// <param name="collectionId"></param>
            /// <param name="resourceType"></param>
            /// <param name="startfromBeginning"></param>
            /// <param name="maximumItemCount">-1 returns all (default)</param>
            /// <returns></returns>
            [Metadata("Get New or Modified FHIR Documents",
                "Query for new or modifed FHIR Documents By Resource Type " +
                "from Last Run Date or Begining of Collection creation"
            )]
            [SwaggerResponse(HttpStatusCode.OK, type: typeof(Task<dynamic>))]
            [SwaggerResponse(HttpStatusCode.NotFound, "No New or Modifed Documents found")]
            [SwaggerOperation("GetNewOrModifiedFHIRDocuments")]
            public async Task<dynamic> GetNewOrModifiedFhirDocuments(
                [Metadata("Database Id", "Database Id")] string databaseId,
                [Metadata("Collection Id", "Collection Id")] string collectionId,
                [Metadata("Resource Type", "FHIR resource type name")] string resourceType,
                [Metadata("Start from Beginning ", "Change Feed Option")] bool startfromBeginning,
                [Metadata("Maximum Item Count", "Number of documents returned. '-1 returns all' (default)")] int maximumItemCount = -1
            )
            {
                var collectionLink = UriFactory.CreateDocumentCollectionUri(databaseId, collectionId);
    
                var context = new DocumentDbContext();  
    
                var docs = new List<dynamic>();
    
                var partitionKeyRanges = new List<PartitionKeyRange>();
                FeedResponse<PartitionKeyRange> pkRangesResponse;
    
                do
                {
                    pkRangesResponse = await context.Client.ReadPartitionKeyRangeFeedAsync(collectionLink);
                    partitionKeyRanges.AddRange(pkRangesResponse);
                } while (pkRangesResponse.ResponseContinuation != null);
    
                foreach (var pkRange in partitionKeyRanges)
                {
                    var changeFeedOptions = new ChangeFeedOptions
                    {
                        StartFromBeginning = startfromBeginning,
                        RequestContinuation = null,
                        MaxItemCount = maximumItemCount,
                        PartitionKeyRangeId = pkRange.Id
                    };
    
                    using (var query = context.Client.CreateDocumentChangeFeedQuery(collectionLink, changeFeedOptions))
                    {
                        do
                        {
                            if (query != null)
                            {
                                var results = await query.ExecuteNextAsync<dynamic>().ConfigureAwait(false);
                                if (results.Count > 0)
                                    docs.AddRange(results.Where(doc => doc.resourceType == resourceType));
                            }
                            else
                            {
                                throw new HttpResponseException(new HttpResponseMessage(HttpStatusCode.NotFound));
                            }
                        } while (query.HasMoreResults);
                    }
                }
                if (docs.Count > 0)
                    return docs;
                var msg = new StringContent("No documents found for " + resourceType + " Resource");
                var response = new HttpResponseMessage
                {
                    StatusCode = HttpStatusCode.NotFound,
                    Content = msg
                };
                return response;
            }
        }
    }
    
```

### <a name="testing-hello-fhirnotificationapi"></a>Testování hello FhirNotificationApi 

Hello následující obrázek ukazuje, jak byl swagger hello použité tootootest [FhirNotificationApi](#api-app-source).

![soubor Swagger Hello používá aplikace API tootest hello](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-testing-app.png)


### <a name="azure-portal-dashboard"></a>Řídicí panel portálu Azure

Hello následující obrázek znázorňuje všechny hello Azure services pro toto řešení spuštěné v hello portálu Azure.

![Hello portálu Azure znázorňující všechny služby hello používané v tomto kurzu HL7 FHIR](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-portal.png)


## <a name="summary"></a>Souhrn

- Jste zjistili, jestli má Azure Cosmos DB nativní podpora pro oznámení o nových nebo upravené dokumenty a jak je snadné toouse. 
- S využitím Logic Apps, můžete vytvořit pracovní postupy bez psaní jakéhokoli kódu.
- Používání Azure fronty služby Service Bus toohandle hello distribuce pro dokumenty HL7 FHIR hello.

## <a name="next-steps"></a>Další kroky
Další informace o databázi Cosmos Azure najdete v tématu hello [domovské stránky Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Více informací o Logic Apps, naleznete v části [Logic Apps](https://azure.microsoft.com/services/logic-apps/).


