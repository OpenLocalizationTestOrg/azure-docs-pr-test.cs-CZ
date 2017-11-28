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
# <a name="customize-swashbuckle-generated-api-definitions"></a><span data-ttu-id="c69d3-103">Přizpůsobení definice rozhraní API generovaných ve Swashbuckle</span><span class="sxs-lookup"><span data-stu-id="c69d3-103">Customize Swashbuckle-generated API definitions</span></span>
## <a name="overview"></a><span data-ttu-id="c69d3-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="c69d3-104">Overview</span></span>
<span data-ttu-id="c69d3-105">Tento článek vysvětluje, jak toocustomize Swashbuckle toohandle běžné scénáře může místo tooalter hello výchozí chování:</span><span class="sxs-lookup"><span data-stu-id="c69d3-105">This article explains how toocustomize Swashbuckle toohandle common scenarios where you may want tooalter hello default behavior:</span></span>

* <span data-ttu-id="c69d3-106">Swashbuckle generuje operace duplicitní identifikátory pro přetížení metody kontroleru</span><span class="sxs-lookup"><span data-stu-id="c69d3-106">Swashbuckle generates duplicate operation identifiers for overloads of controller methods</span></span>
* <span data-ttu-id="c69d3-107">Swashbuckle předpokládá této hello jen HTTP 200 (OK) je platná odpověď z metody</span><span class="sxs-lookup"><span data-stu-id="c69d3-107">Swashbuckle assumes that hello only valid response from a method is HTTP 200 (OK)</span></span> 

## <a name="customize-operation-identifier-generation"></a><span data-ttu-id="c69d3-108">Přizpůsobení identifikátor generování operací</span><span class="sxs-lookup"><span data-stu-id="c69d3-108">Customize operation identifier generation</span></span>
<span data-ttu-id="c69d3-109">Swashbuckle generuje Swagger operaci identifikátory zřetězením názvu řadiče a název metody.</span><span class="sxs-lookup"><span data-stu-id="c69d3-109">Swashbuckle generates Swagger operation identifiers by concatenating controller name and method name.</span></span> <span data-ttu-id="c69d3-110">Tento vzor vytvoří problém, když máte více přetížení metody: Swashbuckle generuje ID duplicitní operace, která je neplatná JSON pro Swagger.</span><span class="sxs-lookup"><span data-stu-id="c69d3-110">This pattern creates a problem when you have multiple overloads of a method: Swashbuckle generates duplicate operation ids, which is invalid Swagger JSON.</span></span>

<span data-ttu-id="c69d3-111">Například následující kód řadiče hello způsobí, že Swashbuckle toogenerate tři ID operace Contact_Get.</span><span class="sxs-lookup"><span data-stu-id="c69d3-111">For example, hello following controller code causes Swashbuckle toogenerate three Contact_Get operation ids.</span></span>

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

<span data-ttu-id="c69d3-112">Hello problém můžete vyřešit ručně tím, že metody hello jedinečné názvy, například následující hello v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="c69d3-112">You can solve hello problem manually by giving hello methods unique names, such as hello following for this example:</span></span>

* <span data-ttu-id="c69d3-113">Získat</span><span class="sxs-lookup"><span data-stu-id="c69d3-113">Get</span></span>
* <span data-ttu-id="c69d3-114">GetById</span><span class="sxs-lookup"><span data-stu-id="c69d3-114">GetById</span></span>
* <span data-ttu-id="c69d3-115">GetPage</span><span class="sxs-lookup"><span data-stu-id="c69d3-115">GetPage</span></span>

<span data-ttu-id="c69d3-116">Hello alternativou je toomake Swashbuckle tooextend ji automaticky generovat jedinečný operaci ID.</span><span class="sxs-lookup"><span data-stu-id="c69d3-116">hello alternative is tooextend Swashbuckle toomake it automatically generate unique operation ids.</span></span>

<span data-ttu-id="c69d3-117">Hello následující kroky ukazují, jak toocustomize Swashbuckle pomocí hello *SwaggerConfig.cs* soubor, který je součástí projektu hello šablonou projektu sady Visual Studio rozhraní API aplikace Preview hello.</span><span class="sxs-lookup"><span data-stu-id="c69d3-117">hello following steps show how toocustomize Swashbuckle by using hello *SwaggerConfig.cs* file that is included in hello project by hello Visual Studio API Apps Preview project template.</span></span>  <span data-ttu-id="c69d3-118">Můžete také upravit Swashbuckle v projektu webového rozhraní API, který jste nakonfigurovali pro nasazení jako aplikace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c69d3-118">You can also customize Swashbuckle in a Web API project that you configure for deployment as an API app.</span></span>

1. <span data-ttu-id="c69d3-119">Vytvoření vlastní `IOperationFilter` implementace</span><span class="sxs-lookup"><span data-stu-id="c69d3-119">Create a custom `IOperationFilter` implementation</span></span> 
   
    <span data-ttu-id="c69d3-120">Hello `IOperationFilter` rozhraní poskytuje bod rozšiřitelnosti pro Swashbuckle uživatelé, kteří chtějí toocustomize různé aspekty procesu metadata Swagger hello.</span><span class="sxs-lookup"><span data-stu-id="c69d3-120">hello `IOperationFilter` interface provides an extensibility point for Swashbuckle users who want toocustomize various aspects of hello Swagger metadata process.</span></span> <span data-ttu-id="c69d3-121">Hello následující kód ukazuje, jednu z metod změny chování hello generování id operace.</span><span class="sxs-lookup"><span data-stu-id="c69d3-121">hello following code demonstrates one method of changing hello operation-id-generation behavior.</span></span> <span data-ttu-id="c69d3-122">Kód Hello připojí název id operace toohello názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="c69d3-122">hello code appends parameter names toohello operation id name.</span></span>  
   
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
2. <span data-ttu-id="c69d3-123">V *App_Start\SwaggerConfig.cs* soubor, volání hello `OperationFilter` metoda toocause Swashbuckle toouse hello nové `IOperationFilter` implementace.</span><span class="sxs-lookup"><span data-stu-id="c69d3-123">In *App_Start\SwaggerConfig.cs* file, call hello `OperationFilter` method toocause Swashbuckle toouse hello new `IOperationFilter` implementation.</span></span>
   
        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)
   
    <span data-ttu-id="c69d3-124">Hello *SwaggerConfig.cs* soubor, který je v vynechána hello balíčku Swashbuckle NuGet obsahuje mnoho komentované příklady body rozšiřitelnosti.</span><span class="sxs-lookup"><span data-stu-id="c69d3-124">hello *SwaggerConfig.cs* file that is dropped in by hello Swashbuckle NuGet package contains many commented-out examples of extensibility points.</span></span> <span data-ttu-id="c69d3-125">Další komentáře Hello nejsou zobrazeny zde.</span><span class="sxs-lookup"><span data-stu-id="c69d3-125">hello additional comments are not shown here.</span></span> 
   
    <span data-ttu-id="c69d3-126">Po provedení této změny vašeho `IOperationFilter` implementaci se používá a způsobí, že operace jedinečné ID toobe vygenerovat.</span><span class="sxs-lookup"><span data-stu-id="c69d3-126">After you make this change, your `IOperationFilter` implementation is used and causes unique operation ids toobe generated.</span></span>
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>

## <a name="allow-response-codes-other-than-200"></a><span data-ttu-id="c69d3-127">Povolit kódy odpovědí než 200</span><span class="sxs-lookup"><span data-stu-id="c69d3-127">Allow response codes other than 200</span></span>
<span data-ttu-id="c69d3-128">Ve výchozím nastavení, Swashbuckle předpokládá, že odpověď HTTP 200 (OK) se hello *pouze* oprávněné odpověď z metody webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c69d3-128">By default, Swashbuckle assumes that an HTTP 200 (OK) response is hello *only* legitimate response from a Web API method.</span></span> <span data-ttu-id="c69d3-129">V některých případech může být vhodné tooreturn další kódy odpovědí aniž by došlo k výjimce tooraise hello klienta.</span><span class="sxs-lookup"><span data-stu-id="c69d3-129">In some cases, you may want tooreturn other response codes without causing hello client tooraise an exception.</span></span>  <span data-ttu-id="c69d3-130">Například hello následující webového rozhraní API kód ukazuje scénář, ve které je vhodnější tooaccept hello klienta 200 nebo 404 jako platný odpovědi.</span><span class="sxs-lookup"><span data-stu-id="c69d3-130">For example, hello following Web API code demonstrates a scenario in which you would want hello client tooaccept either a 200 or a 404 as valid responses.</span></span>

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

<span data-ttu-id="c69d3-131">V tomto scénáři určuje hello Swagger, který generuje Swashbuckle ve výchozím nastavení pouze jeden legitimní stavový kód HTTP, HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="c69d3-131">In this scenario, hello Swagger that Swashbuckle generates by default specifies only one legitimate HTTP status code, HTTP 200.</span></span>

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

<span data-ttu-id="c69d3-132">Vzhledem k tomu, že Visual Studio použije hello kód toogenerate definice rozhraní API Swaggeru pro hello klienta, vytvoří kód klienta, který vyvolá výjimku pro všechny odpovědi než HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="c69d3-132">Since Visual Studio uses hello Swagger API definition toogenerate code for hello client, it creates client code that raises an exception for any response other than an HTTP 200.</span></span> <span data-ttu-id="c69d3-133">Následující kód Hello je z klienta C# generovaná pro tato metoda ukázkové webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c69d3-133">hello code below is from a C# client generated for this sample Web API method.</span></span>

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

<span data-ttu-id="c69d3-134">Swashbuckle poskytuje dva způsoby přizpůsobení hello seznam očekávané kódy odpovědi HTTP, které se generuje, s využitím komentáře XML nebo hello `SwaggerResponse` atribut.</span><span class="sxs-lookup"><span data-stu-id="c69d3-134">Swashbuckle provides two ways of customizing hello list of expected HTTP response codes that it generates, using XML comments or hello `SwaggerResponse` attribute.</span></span> <span data-ttu-id="c69d3-135">atribut Hello je jednodušší, ale je jenom k dispozici v Swashbuckle 5.1.5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c69d3-135">hello attribute is easier, but it is only available in Swashbuckle 5.1.5 or later.</span></span> <span data-ttu-id="c69d3-136">Hello aplikace API preview nový projekt v sadě Visual Studio 2013 zahrnuje šablona Swashbuckle verze 5.0.0, takže pokud používá hello šablony a nechcete, aby tooupdate Swashbuckle, jedinou možností je toouse XML – komentáře.</span><span class="sxs-lookup"><span data-stu-id="c69d3-136">hello API Apps preview new-project template in Visual Studio 2013 includes Swashbuckle version 5.0.0, so if you used hello template and don't want tooupdate Swashbuckle, your only option is toouse XML comments.</span></span> 

### <a name="customize-expected-response-codes-using-xml-comments"></a><span data-ttu-id="c69d3-137">Přizpůsobení kódy očekávané odpovědi pomocí komentáře XML</span><span class="sxs-lookup"><span data-stu-id="c69d3-137">Customize expected response codes using XML comments</span></span>
<span data-ttu-id="c69d3-138">Kódy odpovědí toospecify tuto metodu použijte, pokud vaše verze Swashbuckle, je dřívější než 5.1.5.</span><span class="sxs-lookup"><span data-stu-id="c69d3-138">Use this method toospecify response codes if your Swashbuckle version is earlier than 5.1.5.</span></span>

1. <span data-ttu-id="c69d3-139">Nejprve přidejte dokumentační komentáře XML přes hello metody, které chcete toospecify kódy odpovědi HTTP pro.</span><span class="sxs-lookup"><span data-stu-id="c69d3-139">First, add XML documentation comments over hello methods you wish toospecify HTTP response codes for.</span></span> <span data-ttu-id="c69d3-140">Trvá hello ukázkové webové rozhraní API akci, kterou tooit dokumentace XML uvedené výše a použití hello by způsobilo jako hello následující ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="c69d3-140">Taking hello sample Web API action shown above and applying hello XML documentation tooit would result in code like hello following example.</span></span> 
   
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
2. <span data-ttu-id="c69d3-141">Přidání pokyny v hello *SwaggerConfig.cs* souboru toodirect Swashbuckle toomake použití souborů dokumentace XML hello.</span><span class="sxs-lookup"><span data-stu-id="c69d3-141">Add instructions in hello *SwaggerConfig.cs* file toodirect Swashbuckle toomake use of hello XML documentation file.</span></span>
   
   * <span data-ttu-id="c69d3-142">Otevřete *SwaggerConfig.cs* a vytvoření metody na hello *SwaggerConfig* třída toospecify hello cesta toohello dokumentace souboru XML.</span><span class="sxs-lookup"><span data-stu-id="c69d3-142">Open *SwaggerConfig.cs* and create a method on hello *SwaggerConfig* class toospecify hello path toohello documentation XML file.</span></span> 
     
           private static string GetXmlCommentsPath()
           {
               return string.Format(@"{0}\XmlComments.xml", 
                   System.AppDomain.CurrentDomain.BaseDirectory);
           }
   * <span data-ttu-id="c69d3-143">Přejděte dolů v hello *SwaggerConfig.cs* souborů, dokud neuvidíte hello komentované řádek kód podobný úvodní obrazovka snímek níže.</span><span class="sxs-lookup"><span data-stu-id="c69d3-143">Scroll down in hello *SwaggerConfig.cs* file until you see hello commented-out line of code resembling hello screen shot below.</span></span> 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
   * <span data-ttu-id="c69d3-144">Zrušením komentáře u hello řádku tooenable hello komentáře XML zpracování při generování Swagger.</span><span class="sxs-lookup"><span data-stu-id="c69d3-144">Uncomment hello line tooenable hello XML comments processing during Swagger generation.</span></span> 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
3. <span data-ttu-id="c69d3-145">V pořadí toogenerate hello XML dokumentace souboru přejděte do vlastností projektu hello a povolení souborů dokumentace XML hello jak ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="c69d3-145">In order toogenerate hello XML documentation file, go into hello project's properties and enable hello XML documentation file as shown in hello screenshot below.</span></span> 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

<span data-ttu-id="c69d3-146">Po provedení těchto kroků bude odrážet hello JSON pro Swagger generované Swashbuckle hello kódy odpovědi HTTP, které jste zadali v komentáře XML hello.</span><span class="sxs-lookup"><span data-stu-id="c69d3-146">Once you perform these steps, hello Swagger JSON generated by Swashbuckle will reflect hello HTTP response codes that you specified in hello XML comments.</span></span> <span data-ttu-id="c69d3-147">Následující snímek obrazovky Hello ukazuje této nové datové části JSON.</span><span class="sxs-lookup"><span data-stu-id="c69d3-147">hello screenshot below demonstrates this new JSON payload.</span></span> 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

<span data-ttu-id="c69d3-148">Při použití kódu aplikace Visual Studio tooregenerate hello klienta pro REST API, hello C# – kód přijímá oba hello HTTP OK a nebyl nalezen stavové kódy bez vyvolání k výjimce, což svoje náročné kód toomake rozhodnutí o na jak toohandle hello vrátit Null Obraťte se na záznam.</span><span class="sxs-lookup"><span data-stu-id="c69d3-148">When you use Visual Studio tooregenerate hello client code for your REST API, hello C# code accepts both hello HTTP OK and Not Found status codes without raising an exception, allowing your consuming code toomake decisions on how toohandle hello return of a null Contact record.</span></span> 

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

<span data-ttu-id="c69d3-149">Hello kód této ukázce lze nalézt v [toto úložiště GitHub](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse).</span><span class="sxs-lookup"><span data-stu-id="c69d3-149">hello code for this demonstration can be found in [this GitHub repository](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse).</span></span> <span data-ttu-id="c69d3-150">Společně s hello webového rozhraní API je označené dokumentační komentáře XML projektu projekt konzolové aplikace, který obsahuje generovaného klienta pro toto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c69d3-150">Along with hello Web API project marked up with XML documentation comments is a Console Application project that contains a generated client for this API.</span></span> 

### <a name="customize-expected-response-codes-using-hello-swaggerresponse-attribute"></a><span data-ttu-id="c69d3-151">Přizpůsobení kódy očekávané odpovědi pomocí atributu SwaggerResponse hello</span><span class="sxs-lookup"><span data-stu-id="c69d3-151">Customize expected response codes using hello SwaggerResponse attribute</span></span>
<span data-ttu-id="c69d3-152">Hello [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) atribut je k dispozici ve Swashbuckle 5.1.5 a novějším.</span><span class="sxs-lookup"><span data-stu-id="c69d3-152">hello [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) attribute is available in Swashbuckle 5.1.5 and later.</span></span> <span data-ttu-id="c69d3-153">V případě, že máte ve vašem projektu starší verze, v této části se spustí s vysvětlením, jak tooupdate hello Swashbuckle NuGet balíček tak, aby tento atribut lze použít.</span><span class="sxs-lookup"><span data-stu-id="c69d3-153">In case you have an earlier version in your project, this section starts by explaining how tooupdate hello Swashbuckle NuGet package so that you can use this attribute.</span></span>

1. <span data-ttu-id="c69d3-154">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt webového rozhraní API a klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c69d3-154">In **Solution Explorer**, right-click your Web API project and click **Manage NuGet Packages**.</span></span> 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)
2. <span data-ttu-id="c69d3-155">Klikněte na tlačítko hello *aktualizace* tlačítko Další toohello *Swashbuckle* balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="c69d3-155">Click hello *Update* button next toohello *Swashbuckle* NuGet package.</span></span> 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)
3. <span data-ttu-id="c69d3-156">Přidat hello *SwaggerResponse* atributy metody akce webového rozhraní API toohello, pro které chcete toospecify platný kódy odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="c69d3-156">Add hello *SwaggerResponse* attributes toohello Web API action methods for which you want toospecify valid HTTP response codes.</span></span> 
   
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
4. <span data-ttu-id="c69d3-157">Přidat `using` příkaz pro obor názvů hello atribut:</span><span class="sxs-lookup"><span data-stu-id="c69d3-157">Add a `using` statement for hello attribute's namespace:</span></span>
   
        using Swashbuckle.Swagger.Annotations;
5. <span data-ttu-id="c69d3-158">Procházet toohello */swagger/docs/v1* adresu URL projektu a hello různé kódy odpovědí protokolu HTTP se nebude zobrazovat v hello JSON pro Swagger.</span><span class="sxs-lookup"><span data-stu-id="c69d3-158">Browse toohello */swagger/docs/v1* URL of your project and hello various HTTP response codes will be visible in hello Swagger JSON.</span></span> 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

<span data-ttu-id="c69d3-159">Hello kód této ukázce lze nalézt v [toto úložiště GitHub](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes).</span><span class="sxs-lookup"><span data-stu-id="c69d3-159">hello code for this demonstration can be found in [this GitHub repository](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes).</span></span> <span data-ttu-id="c69d3-160">Společně s hello projekt webového rozhraní API označených pomocí hello *SwaggerResponse* atribut je projekt konzolové aplikace, který obsahuje generovaného klienta pro toto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c69d3-160">Along with hello Web API project decorated with hello *SwaggerResponse* attribute is a Console Application project that contains a generated client for this API.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="c69d3-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c69d3-161">Next steps</span></span>
<span data-ttu-id="c69d3-162">Tento článek ukazuje, jak toocustomize hello způsob Swashbuckle generuje ID operace a kódy platné odezvy.</span><span class="sxs-lookup"><span data-stu-id="c69d3-162">This article has shown how toocustomize hello way Swashbuckle generates operation ids and valid response codes.</span></span> <span data-ttu-id="c69d3-163">Další informace najdete v tématu [Swashbuckle na Githubu](https://github.com/domaindrivendev/Swashbuckle).</span><span class="sxs-lookup"><span data-stu-id="c69d3-163">For more information, see [Swashbuckle on GitHub](https://github.com/domaindrivendev/Swashbuckle).</span></span>

