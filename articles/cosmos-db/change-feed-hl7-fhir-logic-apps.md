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
# <a name="notifying-patients-of-hl7-fhir-health-care-record-changes-using-logic-apps-and-azure-cosmos-db"></a><span data-ttu-id="ee704-104">Upozornění pacientů HL7 FHIR zdravotní péče záznam změn pomocí Logic Apps a Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ee704-104">Notifying patients of HL7 FHIR health care record changes using Logic Apps and Azure Cosmos DB</span></span>

<span data-ttu-id="ee704-105">Azure MVP Howard Edidin byla nedávno kontaktovat zdravotní péče organizaci, která chtěli tooadd nové funkce tootheir pacienta portálu.</span><span class="sxs-lookup"><span data-stu-id="ee704-105">Azure MVP Howard Edidin was recently contacted by a healthcare organization that wanted tooadd new functionality tootheir patient portal.</span></span> <span data-ttu-id="ee704-106">Budou potřeba toopatients toosend oznámení, když byla aktualizována jejich stavu záznamů a jejich potřeby pacientů toobe možné toosubscribe toothese aktualizace.</span><span class="sxs-lookup"><span data-stu-id="ee704-106">They needed toosend notifications toopatients when their health record was updated, and they needed patients toobe able toosubscribe toothese updates.</span></span> 

<span data-ttu-id="ee704-107">Tento článek vás provede řešení informačního kanálu oznámení změn hello vytvořené pro tuto zdravotní péče organizaci, která používá databázi Cosmos Azure Logic Apps a Service Bus.</span><span class="sxs-lookup"><span data-stu-id="ee704-107">This article walks through hello change feed notification solution created for this healthcare organization using Azure Cosmos DB, Logic Apps, and Service Bus.</span></span> 

## <a name="project-requirements"></a><span data-ttu-id="ee704-108">Požadavky na projektu</span><span class="sxs-lookup"><span data-stu-id="ee704-108">Project requirements</span></span>
- <span data-ttu-id="ee704-109">Zprostředkovatelé send že hl7 konsolidované klinické dokumentu architektura (C-CDA) dokumenty ve formátu XML.</span><span class="sxs-lookup"><span data-stu-id="ee704-109">Providers send HL7 Consolidated-Clinical Document Architecture (C-CDA) documents in XML format.</span></span> <span data-ttu-id="ee704-110">C – CDA dokumenty zahrnovat téměř každý typ klinické dokumentu, včetně klinické dokumenty jako rodiny historie a záznamy o očkování, stejně jako správce, pracovní postup a finanční dokumenty.</span><span class="sxs-lookup"><span data-stu-id="ee704-110">C-CDA documents encompass just about every type of clinical document, including clinical documents such as family histories and immunization records, as well as administrative, workflow, and financial documents.</span></span> 
- <span data-ttu-id="ee704-111">C – CDA dokumenty se převedou příliš[HL7 FHIR prostředky](http://hl7.org/fhir/2017Jan/resourcelist.html) ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="ee704-111">C-CDA documents are converted too[HL7 FHIR Resources](http://hl7.org/fhir/2017Jan/resourcelist.html) in JSON format.</span></span>
- <span data-ttu-id="ee704-112">Upravené dokumenty prostředků FHIR odesílá e-mailu ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="ee704-112">Modified FHIR resource documents are sent by email in JSON format.</span></span>

## <a name="solution-workflow"></a><span data-ttu-id="ee704-113">Pracovní postup řešení</span><span class="sxs-lookup"><span data-stu-id="ee704-113">Solution workflow</span></span> 

<span data-ttu-id="ee704-114">Na vysoké úrovni vyžaduje hello projektu hello následující kroky pracovního postupu:</span><span class="sxs-lookup"><span data-stu-id="ee704-114">At a high level, hello project required hello following workflow steps:</span></span> 
1. <span data-ttu-id="ee704-115">Převeďte C-CDA dokumenty tooFHIR prostředky.</span><span class="sxs-lookup"><span data-stu-id="ee704-115">Convert C-CDA documents tooFHIR resources.</span></span>
2. <span data-ttu-id="ee704-116">Proveďte opakované aktivační událost dotazování pro upravené FHIR prostředky.</span><span class="sxs-lookup"><span data-stu-id="ee704-116">Perform recurring trigger polling for modified FHIR resources.</span></span> 
2. <span data-ttu-id="ee704-117">Volání vlastní aplikaci, FhirNotificationApi, tooconnect tooAzure Cosmos DB a dotazů pro nové nebo upravené dokumenty.</span><span class="sxs-lookup"><span data-stu-id="ee704-117">Call a custom app, FhirNotificationApi, tooconnect tooAzure Cosmos DB and query for new or modified documents.</span></span>
3. <span data-ttu-id="ee704-118">Uložte fronty Service Bus tootoohello hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ee704-118">Save hello response tootoohello Service Bus queue.</span></span>
4. <span data-ttu-id="ee704-119">Dotazování pro nové zprávy ve frontě Service Bus hello.</span><span class="sxs-lookup"><span data-stu-id="ee704-119">Poll for new messages in hello Service Bus queue.</span></span>
5. <span data-ttu-id="ee704-120">Odešlete toopatients e-mailové oznámení.</span><span class="sxs-lookup"><span data-stu-id="ee704-120">Send email notifications toopatients.</span></span>

## <a name="solution-architecture"></a><span data-ttu-id="ee704-121">Architektura řešení</span><span class="sxs-lookup"><span data-stu-id="ee704-121">Solution architecture</span></span>
<span data-ttu-id="ee704-122">Toto řešení vyžaduje tři hello toomeet Logic Apps vyšší požadavky a pracovní postup řešení dokončení hello.</span><span class="sxs-lookup"><span data-stu-id="ee704-122">This solution requires three Logic Apps toomeet hello above requirements and complete hello solution workflow.</span></span> <span data-ttu-id="ee704-123">Hello tři aplikace logiky jsou:</span><span class="sxs-lookup"><span data-stu-id="ee704-123">hello three logic apps are:</span></span>
1. <span data-ttu-id="ee704-124">**Aplikace HL7. FHIR mapování**: obdrží hello HL7 C-CDA dokumentu, ho transformuje toohello FHIR prostředků a pak uloží ho tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ee704-124">**HL7-FHIR-Mapping app**: Receives hello HL7 C-CDA document, transforms it toohello FHIR Resource, then saves it tooAzure Cosmos DB.</span></span>
2. <span data-ttu-id="ee704-125">**Aplikace EHR**: dotazuje hello Azure Cosmos DB FHIR úložiště a uloží fronty Service Bus tooa hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ee704-125">**EHR app**: Queries hello Azure Cosmos DB FHIR repository and saves hello response tooa Service Bus queue.</span></span> <span data-ttu-id="ee704-126">Tato aplikace logiky používá [aplikace API](#api-app) tooretrieve nové a změněné dokumenty.</span><span class="sxs-lookup"><span data-stu-id="ee704-126">This logic app uses an [API app](#api-app) tooretrieve new and changed documents.</span></span>
3. <span data-ttu-id="ee704-127">**Proces oznámení aplikace**: odešle e-mail s oznámením s hello FHIR prostředků dokumenty v textu hello.</span><span class="sxs-lookup"><span data-stu-id="ee704-127">**Process notification app**: Sends an email notification with hello FHIR resource documents in hello body.</span></span>

![použité v řešení zdravotní péče HL7 FHIR Hello tři Logic Apps](./media/change-feed-hl7-fhir-logic-apps/health-care-solution-hl7-fhir.png)



### <a name="azure-services-used-in-hello-solution"></a><span data-ttu-id="ee704-129">Použít v hello řešení služby Azure</span><span class="sxs-lookup"><span data-stu-id="ee704-129">Azure services used in hello solution</span></span>

#### <a name="azure-cosmos-db-documentdb-api"></a><span data-ttu-id="ee704-130">Azure Cosmos databáze DocumentDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ee704-130">Azure Cosmos DB DocumentDB API</span></span>
<span data-ttu-id="ee704-131">Azure Cosmos DB je hello úložiště pro hello FHIR prostředky, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="ee704-131">Azure Cosmos DB is hello repository for hello FHIR resources as shown in hello following figure.</span></span>

![účet Azure Cosmos DB Hello použitý v tomto kurzu HL7 FHIR zdravotní péče](./media/change-feed-hl7-fhir-logic-apps/account.png)

#### <a name="logic-apps"></a><span data-ttu-id="ee704-133">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="ee704-133">Logic Apps</span></span>
<span data-ttu-id="ee704-134">Aplikace logiky zpracovávat hello pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="ee704-134">Logic Apps handle hello workflow process.</span></span> <span data-ttu-id="ee704-135">Hello následující snímky obrazovky ukazují hello logiku aplikace vytvořené pro toto řešení.</span><span class="sxs-lookup"><span data-stu-id="ee704-135">hello following screenshots show hello Logic apps created for this solution.</span></span> 


1. <span data-ttu-id="ee704-136">**Aplikace HL7. FHIR mapování**: přijímat hello HL7 C-CDA dokumentu a transformují je tooan FHIR prostředků pomocí hello Enterprise integrační balíček pro Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="ee704-136">**HL7-FHIR-Mapping app**: Receive hello HL7 C-CDA document and transform it tooan FHIR resource using hello Enterprise Integration Pack for Logic Apps.</span></span> <span data-ttu-id="ee704-137">Hello Enterprise integračního balíčku zpracovává hello mapování z hello C-CDA tooFHIR prostředky.</span><span class="sxs-lookup"><span data-stu-id="ee704-137">hello Enterprise Integration Pack handles hello mapping from hello C-CDA tooFHIR resources.</span></span>

    ![Hello aplikace logiky použít tooreceive HL7 FHIR zdravotní péče záznamů](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-json-transform.png)


2. <span data-ttu-id="ee704-139">**Aplikace EHR**: dotazování úložiště Azure Cosmos DB FHIR hello a uložte hello odpovědi tooa Service Bus ve frontě.</span><span class="sxs-lookup"><span data-stu-id="ee704-139">**EHR app**: Query hello Azure Cosmos DB FHIR repository and save hello response tooa Service Bus queue.</span></span> <span data-ttu-id="ee704-140">Hello kód pro aplikaci GetNewOrModifiedFHIRDocuments hello je níže.</span><span class="sxs-lookup"><span data-stu-id="ee704-140">hello code for hello GetNewOrModifiedFHIRDocuments app is below.</span></span>

    ![tooquery Hello používá aplikace logiky Azure Cosmos DB](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-api-app.png)

3. <span data-ttu-id="ee704-142">**Proces oznámení aplikace**: Odeslat oznámení e-mailu s hello FHIR prostředků dokumenty v textu hello.</span><span class="sxs-lookup"><span data-stu-id="ee704-142">**Process notification app**: Send an email notification with hello FHIR resource documents in hello body.</span></span>

    ![Hello aplikace logiky, která odešle pacienta e-mailu pomocí hello HL7 FHIR prostředků v textu hello](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-send-email.png)

#### <a name="service-bus"></a><span data-ttu-id="ee704-144">Service Bus</span><span class="sxs-lookup"><span data-stu-id="ee704-144">Service Bus</span></span>
<span data-ttu-id="ee704-145">Následující obrázek ukazuje hello pacientů fronty Hello.</span><span class="sxs-lookup"><span data-stu-id="ee704-145">hello following figure shows hello patients queue.</span></span> <span data-ttu-id="ee704-146">Hodnota vlastnosti značky Hello se používá pro předmět e-mailu hello.</span><span class="sxs-lookup"><span data-stu-id="ee704-146">hello Tag property value is used for hello email subject.</span></span>

![Hello použili v tomto kurzu HL7 FHIR frontou Service Bus](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-service-bus-queue.png)

<a id="api-app"></a>

#### <a name="api-app"></a><span data-ttu-id="ee704-148">Aplikace API</span><span class="sxs-lookup"><span data-stu-id="ee704-148">API app</span></span>
<span data-ttu-id="ee704-149">Aplikace API připojí tooAzure Cosmos DB a dotazů pro nové nebo upravené dokumenty FHIR podle typů prostředků.</span><span class="sxs-lookup"><span data-stu-id="ee704-149">An API app connects tooAzure Cosmos DB and queries for new or modified FHIR documents By resource type.</span></span> <span data-ttu-id="ee704-150">Tato aplikace má jeden řadič **FhirNotificationApi** s jednu operaci **GetNewOrModifiedFhirDocuments**, najdete v části [zdroje pro aplikaci API](#api-app-source).</span><span class="sxs-lookup"><span data-stu-id="ee704-150">This app has one controller, **FhirNotificationApi** with a one operation **GetNewOrModifiedFhirDocuments**, see [source for API app](#api-app-source).</span></span>

<span data-ttu-id="ee704-151">Používáme hello [ `CreateDocumentChangeFeedQuery` ](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) třídy z hello Azure Cosmos databáze DocumentDB .NET API.</span><span class="sxs-lookup"><span data-stu-id="ee704-151">We are using hello [`CreateDocumentChangeFeedQuery`](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) class from hello Azure Cosmos DB DocumentDB .NET API.</span></span> <span data-ttu-id="ee704-152">Další informace najdete v tématu hello [změnu kanálu článku](change-feed.md).</span><span class="sxs-lookup"><span data-stu-id="ee704-152">For more information, see hello [change feed article](change-feed.md).</span></span> 

##### <a name="getnewormodifiedfhirdocuments-operation"></a><span data-ttu-id="ee704-153">Operace GetNewOrModifiedFhirDocuments</span><span class="sxs-lookup"><span data-stu-id="ee704-153">GetNewOrModifiedFhirDocuments operation</span></span>

<span data-ttu-id="ee704-154">**Vstupy**</span><span class="sxs-lookup"><span data-stu-id="ee704-154">**Inputs**</span></span>
- <span data-ttu-id="ee704-155">Hodnotu DatabaseId</span><span class="sxs-lookup"><span data-stu-id="ee704-155">DatabaseId</span></span>
- <span data-ttu-id="ee704-156">CollectionId</span><span class="sxs-lookup"><span data-stu-id="ee704-156">CollectionId</span></span>
- <span data-ttu-id="ee704-157">Název typu prostředku FHIR HL7</span><span class="sxs-lookup"><span data-stu-id="ee704-157">HL7 FHIR Resource Type name</span></span>
- <span data-ttu-id="ee704-158">Logická hodnota: Začít od začátku</span><span class="sxs-lookup"><span data-stu-id="ee704-158">Boolean: Start from Beginning</span></span>
- <span data-ttu-id="ee704-159">Int: Počet vrácených dokumentů</span><span class="sxs-lookup"><span data-stu-id="ee704-159">Int: Number of documents returned</span></span>

<span data-ttu-id="ee704-160">**Výstupy**</span><span class="sxs-lookup"><span data-stu-id="ee704-160">**Outputs**</span></span>
- <span data-ttu-id="ee704-161">Úspěch: Stavový kód: odpovědi 200,: seznam dokumentů (pole JSON)</span><span class="sxs-lookup"><span data-stu-id="ee704-161">Success: Status Code: 200, Response: List of Documents (JSON Array)</span></span>
- <span data-ttu-id="ee704-162">Chyba: Stavový kód: odpovědi 404,: "nalezeny žádné dokumenty pro '*název prostředku '* typ prostředku"</span><span class="sxs-lookup"><span data-stu-id="ee704-162">Failure: Status Code: 404, Response: "No Documents found for '*resource name'* Resource Type"</span></span>

<a id="api-app-source"></a>

<span data-ttu-id="ee704-163">**Zdroj pro aplikace hello rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="ee704-163">**Source for hello API app**</span></span>

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

### <a name="testing-hello-fhirnotificationapi"></a><span data-ttu-id="ee704-164">Testování hello FhirNotificationApi</span><span class="sxs-lookup"><span data-stu-id="ee704-164">Testing hello FhirNotificationApi</span></span> 

<span data-ttu-id="ee704-165">Hello následující obrázek ukazuje, jak byl swagger hello použité tootootest [FhirNotificationApi](#api-app-source).</span><span class="sxs-lookup"><span data-stu-id="ee704-165">hello following image demonstrates how swagger was used tootootest hello [FhirNotificationApi](#api-app-source).</span></span>

![soubor Swagger Hello používá aplikace API tootest hello](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-testing-app.png)


### <a name="azure-portal-dashboard"></a><span data-ttu-id="ee704-167">Řídicí panel portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ee704-167">Azure portal dashboard</span></span>

<span data-ttu-id="ee704-168">Hello následující obrázek znázorňuje všechny hello Azure services pro toto řešení spuštěné v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ee704-168">hello following image shows all of hello Azure services for this solution running in hello Azure portal.</span></span>

![Hello portálu Azure znázorňující všechny služby hello používané v tomto kurzu HL7 FHIR](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-portal.png)


## <a name="summary"></a><span data-ttu-id="ee704-170">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ee704-170">Summary</span></span>

- <span data-ttu-id="ee704-171">Jste zjistili, jestli má Azure Cosmos DB nativní podpora pro oznámení o nových nebo upravené dokumenty a jak je snadné toouse.</span><span class="sxs-lookup"><span data-stu-id="ee704-171">You have learned that Azure Cosmos DB has native suppport for notifications for new or modifed documents and how easy it is toouse.</span></span> 
- <span data-ttu-id="ee704-172">S využitím Logic Apps, můžete vytvořit pracovní postupy bez psaní jakéhokoli kódu.</span><span class="sxs-lookup"><span data-stu-id="ee704-172">By leveraging Logic Apps, you can create workflows without writing any code.</span></span>
- <span data-ttu-id="ee704-173">Používání Azure fronty služby Service Bus toohandle hello distribuce pro dokumenty HL7 FHIR hello.</span><span class="sxs-lookup"><span data-stu-id="ee704-173">Using Azure Service Bus Queues toohandle hello distribution for hello HL7 FHIR documents.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee704-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ee704-174">Next steps</span></span>
<span data-ttu-id="ee704-175">Další informace o databázi Cosmos Azure najdete v tématu hello [domovské stránky Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="ee704-175">For more information about Azure Cosmos DB, see hello [Azure Cosmos DB home page](https://azure.microsoft.com/services/cosmos-db/).</span></span> <span data-ttu-id="ee704-176">Více informací o Logic Apps, naleznete v části [Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span><span class="sxs-lookup"><span data-stu-id="ee704-176">For more informaiton about Logic Apps, see [Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span></span>


