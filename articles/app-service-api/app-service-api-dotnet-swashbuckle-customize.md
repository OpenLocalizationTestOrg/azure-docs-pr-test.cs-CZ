---
title: "Přizpůsobení definice rozhraní API generovaných ve Swashbuckle"
description: "Zjistěte, jak přizpůsobit definice rozhraní API Swaggeru, generovaných ve Swashbuckle pro aplikace API v Azure App Service."
services: app-service\api
documentationcenter: .net
author: bradygaster
manager: erikre
editor: jimbe
ms.assetid: 6b8cbc38-d282-4a0f-b0c5-762631bae6f3
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 08/29/2016
ms.author: rachelap
ms.openlocfilehash: c83905a97fb2ee988fe06fc1f9a7379c1741fd02
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="customize-swashbuckle-generated-api-definitions"></a>Přizpůsobení definice rozhraní API generovaných ve Swashbuckle
## <a name="overview"></a>Přehled
Tento článek vysvětluje, jak přizpůsobit Swashbuckle ke zpracování běžných scénářů, kde můžete změnit tak výchozí chování:

* Swashbuckle generuje operace duplicitní identifikátory pro přetížení metody kontroleru
* Swashbuckle předpokládá, že je platné pouze odpověď z metody HTTP 200 (OK) 

## <a name="customize-operation-identifier-generation"></a>Přizpůsobení identifikátor generování operací
Swashbuckle generuje Swagger operaci identifikátory zřetězením názvu řadiče a název metody. Tento vzor vytvoří problém, když máte více přetížení metody: Swashbuckle generuje ID duplicitní operace, která je neplatná JSON pro Swagger.

Například následující kód řadiče způsobí, že Swashbuckle ke generování ID operace tři Contact_Get.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

Problém můžete vyřešit ručně tím, že jedinečné názvy, například pro tento příklad následující metody:

* Získat
* GetById
* GetPage

Alternativou je Swashbuckle, chcete-li automaticky generovat jedinečný operaci ID rozšíření.

Následující kroky ukazují, jak přizpůsobit Swashbuckle pomocí *SwaggerConfig.cs* soubor, který je zahrnutý v projektu šablonou projektu sady Visual Studio rozhraní API aplikace Preview.  Můžete také upravit Swashbuckle v projektu webového rozhraní API, který jste nakonfigurovali pro nasazení jako aplikace rozhraní API.

1. Vytvoření vlastní `IOperationFilter` implementace 
   
    `IOperationFilter` Rozhraní poskytuje bod rozšiřitelnosti pro Swashbuckle uživatele, kteří chtějí přizpůsobit různé aspekty proces metadat Swagger. Následující kód ukazuje jednu z metod změny chování generování id operace. Kód připojí názvy parametrů k názvu id operace.  
   
        using Swashbuckle.Swagger;
        using System.Web.Http.Description;
   
        namespace ContactsList
        {
            public class MultipleOperationsWithSameVerbFilter : IOperationFilter
            {
                public void Apply(
                    Operation operation,
                    SchemaRegistry schemaRegistry,
                    ApiDescription apiDescription)
                {
                    if (operation.parameters != null)
                    {
                        operation.operationId += "By";
                        foreach (var parm in operation.parameters)
                        {
                            operation.operationId += string.Format("{0}",parm.name);
                        }
                    }
                }
            }
        }
2. V *App_Start\SwaggerConfig.cs* souboru, volejte `OperationFilter` metoda způsobit Swashbuckle použití nového `IOperationFilter` implementace.
   
        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)
   
    *SwaggerConfig.cs* soubor, který je vyřadit balíček Swashbuckle NuGet obsahuje mnoho komentované příklady body rozšiřitelnosti. Další komentáře nejsou zobrazeny zde. 
   
    Po provedení této změny vašeho `IOperationFilter` implementaci se používá a způsobí, že operace jedinečné ID má být vygenerován.
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>

## <a name="allow-response-codes-other-than-200"></a>Povolit kódy odpovědí než 200
Ve výchozím nastavení, Swashbuckle předpokládá, že odpověď HTTP 200 (OK) se *pouze* oprávněné odpověď z metody webového rozhraní API. V některých případech můžete chtít návratové kódy jiné odpovědi aniž by to způsobilo klientovi vyvolat výjimku.  Například následující kód webového rozhraní API ukazuje scénář, ve které je vhodnější klienta tak, aby přijímal 200 nebo 404 jako platný odpovědi.

    [ResponseType(typeof(Contact))]
    public HttpResponseMessage Get(int id)
    {
        var contacts = GetContacts();

        var requestedContact = contacts.FirstOrDefault(x => x.Id == id);

        if (requestedContact == null)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }
        else
        {
            return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
        }
    }

V tomto scénáři určuje Swagger, který generuje Swashbuckle ve výchozím nastavení pouze jeden legitimní stavový kód HTTP, HTTP 200.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Vzhledem k tomu, že Visual Studio použije definici rozhraní API Swaggeru pro generování kódu pro klienta, vytvoří kód klienta, který vyvolá výjimku pro všechny odpovědi než HTTP 200. Následující kód je z klienta C# generovaná pro tato metoda ukázkové webové rozhraní API.

    if (statusCode != HttpStatusCode.OK)
    {
        HttpOperationException<object> ex = new HttpOperationException<object>();
        ex.Request = httpRequest;
        ex.Response = httpResponse;
        ex.Body = null;
        if (shouldTrace)
        {
            ServiceClientTracing.Error(invocationId, ex);
        }
        throw ex;
    } 

Swashbuckle poskytuje dva způsoby přizpůsobení seznamu očekávané kódy odpovědi HTTP, které se generuje, s využitím komentáře XML nebo `SwaggerResponse` atribut. Atribut je jednodušší, ale je jenom k dispozici v Swashbuckle 5.1.5 nebo novější. Šablony aplikace API preview nový projekt v sadě Visual Studio 2013 zahrnuje Swashbuckle verze 5.0.0, takže pokud použít šablonu a nechcete aktualizovat Swashbuckle, jedinou možností je použít komentáře XML. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>Přizpůsobení kódy očekávané odpovědi pomocí komentáře XML
Tuto metodu použijte k určení kódy odpovědí, pokud vaše verze Swashbuckle, je dřívější než 5.1.5.

1. Nejprve přidejte dokumentační komentáře XML prostřednictvím metody, která si přejete zadejte kódy odpovědi HTTP pro. Odběr vzorku webového rozhraní API akce uvedené výše a použití dokumentace XML k němu by způsobilo kód jako v následujícím příkladu. 
   
        /// <summary>
        /// Returns the specified contact.
        /// </summary>
        /// <param name="id">The ID of the contact.</param>
        /// <returns>A contact record with an HTTP 200, or null with an HTTP 404.</returns>
        /// <response code="200">OK</response>
        /// <response code="404">Not Found</response>
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
   
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
   
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }
2. Přidat podle pokynů *SwaggerConfig.cs* souboru směrovat Swashbuckle nutné používat kód XML souborů dokumentace.
   
   * Otevřete *SwaggerConfig.cs* a vytvoření metody na *SwaggerConfig* třídy pro zadání cesty k souboru XML dokumentace. 
     
           private static string GetXmlCommentsPath()
           {
               return string.Format(@"{0}\XmlComments.xml", 
                   System.AppDomain.CurrentDomain.BaseDirectory);
           }
   * Přejděte dolů v *SwaggerConfig.cs* souborů, dokud neuvidíte komentované řádek kódu připomínající následující kopie obrazovky. 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
   * Zrušením komentáře u řádku umožňující komentáře XML zpracování při generování Swagger. 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
3. Chcete-li vygenerovat souborů dokumentace XML, přejděte do vlastností projektu a povolení souborů dokumentace XML, jak je vidět na tomto snímku obrazovky. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

Po provedení těchto kroků se projeví JSON pro Swagger generované Swashbuckle kódy odpovědí protokolu HTTP, které jste zadali v komentáře XML. Následující snímek obrazovky ukazuje této nové datové části JSON. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

Pokud používáte Visual Studio znovu vygenerovat kód klienta pro REST API, kód C# přijme stavové kódy HTTP OK i nebyl nalezen bez vyvolání výjimky umožňující náročné kódu při rozhodování o tom, jak zpracovat vrátit hodnotu null. Obraťte se na záznamu. 

        if (statusCode != HttpStatusCode.OK && statusCode != HttpStatusCode.NotFound)
        {
            HttpOperationException<object> ex = new HttpOperationException<object>();
            ex.Request = httpRequest;
            ex.Response = httpResponse;
            ex.Body = null;
            if (shouldTrace)
            {
                ServiceClientTracing.Error(invocationId, ex);
            }
                throw ex;
        }

Kód pro této ukázce lze nalézt v [toto úložiště GitHub](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse). Společně s webového rozhraní API je označené dokumentační komentáře XML projektu projekt konzolové aplikace, který obsahuje generovaného klienta pro toto rozhraní API. 

### <a name="customize-expected-response-codes-using-the-swaggerresponse-attribute"></a>Přizpůsobení pomocí atributu SwaggerResponse kódy očekávané odpovědi
[SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) atribut je k dispozici ve Swashbuckle 5.1.5 a novějším. V případě, že máte ve vašem projektu starší verze, v této části se spustí s vysvětlením, jak aktualizovat balíček Swashbuckle NuGet tak, aby tento atribut lze použít.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt webového rozhraní API a klikněte na **spravovat balíčky NuGet**. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)
2. Klikněte *aktualizace* vedle položky *Swashbuckle* balíček NuGet. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)
3. Přidat *SwaggerResponse* atributy metody akce webového rozhraní API, pro které chcete určit platný kódy odpovědi HTTP. 
   
        [SwaggerResponse(HttpStatusCode.OK)]
        [SwaggerResponse(HttpStatusCode.NotFound)]
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
   
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }
4. Přidat `using` příkaz pro obor názvů atributu:
   
        using Swashbuckle.Swagger.Annotations;
5. Vyhledejte */swagger/docs/v1* adresu URL projektu a různé kódy odpovědí protokolu HTTP se nebude zobrazovat v JSON pro Swagger. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

Kód pro této ukázce lze nalézt v [toto úložiště GitHub](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes). Společně s projekt webového rozhraní API označených pomocí *SwaggerResponse* atribut je projekt konzolové aplikace, který obsahuje generovaného klienta pro toto rozhraní API. 

## <a name="next-steps"></a>Další kroky
Tento článek ukazuje, jak přizpůsobit způsob Swashbuckle generuje ID operace a kódy platné odezvy. Další informace najdete v tématu [Swashbuckle na Githubu](https://github.com/domaindrivendev/Swashbuckle).

