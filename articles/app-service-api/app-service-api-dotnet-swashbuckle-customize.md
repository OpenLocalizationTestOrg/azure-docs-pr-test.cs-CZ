---
title: "definice rozhraní API generovaných ve Swashbuckle aaaCustomize"
description: "Zjistěte, jak definice rozhraní API Swaggeru toocustomize, generovaných ve Swashbuckle pro aplikace API v Azure App Service."
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
ms.openlocfilehash: e31c665f8993533c5ec9a935e42cce34f86a5ade
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-swashbuckle-generated-api-definitions"></a>Přizpůsobení definice rozhraní API generovaných ve Swashbuckle
## <a name="overview"></a>Přehled
Tento článek vysvětluje, jak toocustomize Swashbuckle toohandle běžné scénáře může místo tooalter hello výchozí chování:

* Swashbuckle generuje operace duplicitní identifikátory pro přetížení metody kontroleru
* Swashbuckle předpokládá této hello jen HTTP 200 (OK) je platná odpověď z metody 

## <a name="customize-operation-identifier-generation"></a>Přizpůsobení identifikátor generování operací
Swashbuckle generuje Swagger operaci identifikátory zřetězením názvu řadiče a název metody. Tento vzor vytvoří problém, když máte více přetížení metody: Swashbuckle generuje ID duplicitní operace, která je neplatná JSON pro Swagger.

Například následující kód řadiče hello způsobí, že Swashbuckle toogenerate tři ID operace Contact_Get.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

Hello problém můžete vyřešit ručně tím, že metody hello jedinečné názvy, například následující hello v tomto příkladu:

* Získat
* GetById
* GetPage

Hello alternativou je toomake Swashbuckle tooextend ji automaticky generovat jedinečný operaci ID.

Hello následující kroky ukazují, jak toocustomize Swashbuckle pomocí hello *SwaggerConfig.cs* soubor, který je součástí projektu hello šablonou projektu sady Visual Studio rozhraní API aplikace Preview hello.  Můžete také upravit Swashbuckle v projektu webového rozhraní API, který jste nakonfigurovali pro nasazení jako aplikace rozhraní API.

1. Vytvoření vlastní `IOperationFilter` implementace 
   
    Hello `IOperationFilter` rozhraní poskytuje bod rozšiřitelnosti pro Swashbuckle uživatelé, kteří chtějí toocustomize různé aspekty procesu metadata Swagger hello. Hello následující kód ukazuje, jednu z metod změny chování hello generování id operace. Kód Hello připojí název id operace toohello názvy parametrů.  
   
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
2. V *App_Start\SwaggerConfig.cs* soubor, volání hello `OperationFilter` metoda toocause Swashbuckle toouse hello nové `IOperationFilter` implementace.
   
        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)
   
    Hello *SwaggerConfig.cs* soubor, který je v vynechána hello balíčku Swashbuckle NuGet obsahuje mnoho komentované příklady body rozšiřitelnosti. Další komentáře Hello nejsou zobrazeny zde. 
   
    Po provedení této změny vašeho `IOperationFilter` implementaci se používá a způsobí, že operace jedinečné ID toobe vygenerovat.
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>

## <a name="allow-response-codes-other-than-200"></a>Povolit kódy odpovědí než 200
Ve výchozím nastavení, Swashbuckle předpokládá, že odpověď HTTP 200 (OK) se hello *pouze* oprávněné odpověď z metody webového rozhraní API. V některých případech může být vhodné tooreturn další kódy odpovědí aniž by došlo k výjimce tooraise hello klienta.  Například hello následující webového rozhraní API kód ukazuje scénář, ve které je vhodnější tooaccept hello klienta 200 nebo 404 jako platný odpovědi.

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

V tomto scénáři určuje hello Swagger, který generuje Swashbuckle ve výchozím nastavení pouze jeden legitimní stavový kód HTTP, HTTP 200.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Vzhledem k tomu, že Visual Studio použije hello kód toogenerate definice rozhraní API Swaggeru pro hello klienta, vytvoří kód klienta, který vyvolá výjimku pro všechny odpovědi než HTTP 200. Následující kód Hello je z klienta C# generovaná pro tato metoda ukázkové webové rozhraní API.

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

Swashbuckle poskytuje dva způsoby přizpůsobení hello seznam očekávané kódy odpovědi HTTP, které se generuje, s využitím komentáře XML nebo hello `SwaggerResponse` atribut. atribut Hello je jednodušší, ale je jenom k dispozici v Swashbuckle 5.1.5 nebo novější. Hello aplikace API preview nový projekt v sadě Visual Studio 2013 zahrnuje šablona Swashbuckle verze 5.0.0, takže pokud používá hello šablony a nechcete, aby tooupdate Swashbuckle, jedinou možností je toouse XML – komentáře. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>Přizpůsobení kódy očekávané odpovědi pomocí komentáře XML
Kódy odpovědí toospecify tuto metodu použijte, pokud vaše verze Swashbuckle, je dřívější než 5.1.5.

1. Nejprve přidejte dokumentační komentáře XML přes hello metody, které chcete toospecify kódy odpovědi HTTP pro. Trvá hello ukázkové webové rozhraní API akci, kterou tooit dokumentace XML uvedené výše a použití hello by způsobilo jako hello následující ukázka kódu. 
   
        /// <summary>
        /// Returns hello specified contact.
        /// </summary>
        /// <param name="id">hello ID of hello contact.</param>
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
2. Přidání pokyny v hello *SwaggerConfig.cs* souboru toodirect Swashbuckle toomake použití souborů dokumentace XML hello.
   
   * Otevřete *SwaggerConfig.cs* a vytvoření metody na hello *SwaggerConfig* třída toospecify hello cesta toohello dokumentace souboru XML. 
     
           private static string GetXmlCommentsPath()
           {
               return string.Format(@"{0}\XmlComments.xml", 
                   System.AppDomain.CurrentDomain.BaseDirectory);
           }
   * Přejděte dolů v hello *SwaggerConfig.cs* souborů, dokud neuvidíte hello komentované řádek kód podobný úvodní obrazovka snímek níže. 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
   * Zrušením komentáře u hello řádku tooenable hello komentáře XML zpracování při generování Swagger. 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
3. V pořadí toogenerate hello XML dokumentace souboru přejděte do vlastností projektu hello a povolení souborů dokumentace XML hello jak ukazuje následující snímek obrazovky hello. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

Po provedení těchto kroků bude odrážet hello JSON pro Swagger generované Swashbuckle hello kódy odpovědi HTTP, které jste zadali v komentáře XML hello. Následující snímek obrazovky Hello ukazuje této nové datové části JSON. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

Při použití kódu aplikace Visual Studio tooregenerate hello klienta pro REST API, hello C# – kód přijímá oba hello HTTP OK a nebyl nalezen stavové kódy bez vyvolání k výjimce, což svoje náročné kód toomake rozhodnutí o na jak toohandle hello vrátit Null Obraťte se na záznam. 

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

Hello kód této ukázce lze nalézt v [toto úložiště GitHub](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse). Společně s hello webového rozhraní API je označené dokumentační komentáře XML projektu projekt konzolové aplikace, který obsahuje generovaného klienta pro toto rozhraní API. 

### <a name="customize-expected-response-codes-using-hello-swaggerresponse-attribute"></a>Přizpůsobení kódy očekávané odpovědi pomocí atributu SwaggerResponse hello
Hello [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) atribut je k dispozici ve Swashbuckle 5.1.5 a novějším. V případě, že máte ve vašem projektu starší verze, v této části se spustí s vysvětlením, jak tooupdate hello Swashbuckle NuGet balíček tak, aby tento atribut lze použít.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt webového rozhraní API a klikněte na **spravovat balíčky NuGet**. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)
2. Klikněte na tlačítko hello *aktualizace* tlačítko Další toohello *Swashbuckle* balíček NuGet. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)
3. Přidat hello *SwaggerResponse* atributy metody akce webového rozhraní API toohello, pro které chcete toospecify platný kódy odpovědi HTTP. 
   
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
4. Přidat `using` příkaz pro obor názvů hello atribut:
   
        using Swashbuckle.Swagger.Annotations;
5. Procházet toohello */swagger/docs/v1* adresu URL projektu a hello různé kódy odpovědí protokolu HTTP se nebude zobrazovat v hello JSON pro Swagger. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

Hello kód této ukázce lze nalézt v [toto úložiště GitHub](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes). Společně s hello projekt webového rozhraní API označených pomocí hello *SwaggerResponse* atribut je projekt konzolové aplikace, který obsahuje generovaného klienta pro toto rozhraní API. 

## <a name="next-steps"></a>Další kroky
Tento článek ukazuje, jak toocustomize hello způsob Swashbuckle generuje ID operace a kódy platné odezvy. Další informace najdete v tématu [Swashbuckle na Githubu](https://github.com/domaindrivendev/Swashbuckle).

