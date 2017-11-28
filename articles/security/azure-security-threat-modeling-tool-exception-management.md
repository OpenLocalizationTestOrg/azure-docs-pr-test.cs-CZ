---
title: "aaaException Management - Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
description: "způsoby zmírnění hrozeb v hello nástroj modelování hrozeb"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 247096c10deeca94ebb9b19df7ba60e442ca1e4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-exception-management--mitigations"></a><span data-ttu-id="fb03b-103">Zabezpečení rámce: Správa výjimek | Způsoby zmírnění rizik</span><span class="sxs-lookup"><span data-stu-id="fb03b-103">Security Frame: Exception Management | Mitigations</span></span> 
| <span data-ttu-id="fb03b-104">Produktům a službám</span><span class="sxs-lookup"><span data-stu-id="fb03b-104">Product/Service</span></span> | <span data-ttu-id="fb03b-105">Článek</span><span class="sxs-lookup"><span data-stu-id="fb03b-105">Article</span></span> |
| --------------- | ------- |
| <span data-ttu-id="fb03b-106">**WCF**</span><span class="sxs-lookup"><span data-stu-id="fb03b-106">**WCF**</span></span> | <ul><li>[<span data-ttu-id="fb03b-107">WCF – nezahrnujte serviceDebug uzlu v konfiguračním souboru</span><span class="sxs-lookup"><span data-stu-id="fb03b-107">WCF- Do not include serviceDebug node in configuration file</span></span>](#servicedebug)</li><li>[<span data-ttu-id="fb03b-108">WCF – nezahrnujte serviceMetadata uzlu v konfiguračním souboru</span><span class="sxs-lookup"><span data-stu-id="fb03b-108">WCF- Do not include serviceMetadata node in configuration file</span></span>](#servicemetadata)</li></ul> |
| <span data-ttu-id="fb03b-109">**Webové rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="fb03b-109">**Web API**</span></span> | <ul><li>[<span data-ttu-id="fb03b-110">Zajistit, že je v rozhraní ASP.NET Web API správné zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="fb03b-110">Ensure that proper exception handling is done in ASP.NET Web API </span></span>](#exception)</li></ul> |
| <span data-ttu-id="fb03b-111">**Webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="fb03b-111">**Web Application**</span></span> | <ul><li>[<span data-ttu-id="fb03b-112">Podrobné informace o zabezpečení v chybových zprávách, neuvádějí</span><span class="sxs-lookup"><span data-stu-id="fb03b-112">Do not expose security details in error messages </span></span>](#messages)</li><li>[<span data-ttu-id="fb03b-113">Implementace výchozí chyba zpracování stránky</span><span class="sxs-lookup"><span data-stu-id="fb03b-113">Implement Default error handling page </span></span>](#default)</li><li>[<span data-ttu-id="fb03b-114">Nastavte metodu nasazení tooRetail ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="fb03b-114">Set Deployment Method tooRetail in IIS</span></span>](#deployment)</li><li>[<span data-ttu-id="fb03b-115">Výjimky má bezpečně selhat.</span><span class="sxs-lookup"><span data-stu-id="fb03b-115">Exceptions should fail safely</span></span>](#fail)</li></ul> |

## <span data-ttu-id="fb03b-116"><a id="servicedebug"></a>WCF – nezahrnujte serviceDebug uzlu v konfiguračním souboru</span><span class="sxs-lookup"><span data-stu-id="fb03b-116"><a id="servicedebug"></a>WCF- Do not include serviceDebug node in configuration file</span></span>

| <span data-ttu-id="fb03b-117">Název</span><span class="sxs-lookup"><span data-stu-id="fb03b-117">Title</span></span>                   | <span data-ttu-id="fb03b-118">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="fb03b-118">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="fb03b-119">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="fb03b-119">**Component**</span></span>               | <span data-ttu-id="fb03b-120">WCF</span><span class="sxs-lookup"><span data-stu-id="fb03b-120">WCF</span></span> | 
| <span data-ttu-id="fb03b-121">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="fb03b-121">**SDL Phase**</span></span>               | <span data-ttu-id="fb03b-122">Sestavení</span><span class="sxs-lookup"><span data-stu-id="fb03b-122">Build</span></span> |  
| <span data-ttu-id="fb03b-123">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="fb03b-123">**Applicable Technologies**</span></span> | <span data-ttu-id="fb03b-124">Obecné, NET Framework 3</span><span class="sxs-lookup"><span data-stu-id="fb03b-124">Generic, NET Framework 3</span></span> |
| <span data-ttu-id="fb03b-125">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="fb03b-125">**Attributes**</span></span>              | <span data-ttu-id="fb03b-126">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="fb03b-126">N/A</span></span>  |
| <span data-ttu-id="fb03b-127">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="fb03b-127">**References**</span></span>              | <span data-ttu-id="fb03b-128">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení království](https://vulncat.fortify.com/en/vulncat/index.html)</span><span class="sxs-lookup"><span data-stu-id="fb03b-128">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Kingdom](https://vulncat.fortify.com/en/vulncat/index.html)</span></span> |
| <span data-ttu-id="fb03b-129">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="fb03b-129">**Steps**</span></span> | <span data-ttu-id="fb03b-130">Komunikace Framework služby Windows (WCF) může být nakonfigurované tooexpose ladicí informace.</span><span class="sxs-lookup"><span data-stu-id="fb03b-130">Windows Communication Framework (WCF) services can be configured tooexpose debugging information.</span></span> <span data-ttu-id="fb03b-131">Ladění informace nesmí použít v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="fb03b-131">Debug information should not be used in production environments.</span></span> <span data-ttu-id="fb03b-132">Hello `<serviceDebug>` značky definuje, zda je povolena hello ladicí informace funkce služby WCF.</span><span class="sxs-lookup"><span data-stu-id="fb03b-132">hello `<serviceDebug>` tag defines whether hello debug information feature is enabled for a WCF service.</span></span> <span data-ttu-id="fb03b-133">Pokud atribut hello třídu includeExceptionDetailInFaults nastavena tootrue, informace o výjimce z aplikace hello, bude vrácen tooclients.</span><span class="sxs-lookup"><span data-stu-id="fb03b-133">If hello attribute includeExceptionDetailInFaults is set tootrue, exception information from hello application will be returned tooclients.</span></span> <span data-ttu-id="fb03b-134">Útočníci mohou využívat hello Další informace, které se získají z ladění výstupu toomount útoky cílené na hello framework, databázi nebo jiným prostředkům, které používá aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fb03b-134">Attackers can leverage hello additional information they gain from debugging output toomount attacks targeted on hello framework, database, or other resources used by hello application.</span></span> |

### <a name="example"></a><span data-ttu-id="fb03b-135">Příklad</span><span class="sxs-lookup"><span data-stu-id="fb03b-135">Example</span></span>
<span data-ttu-id="fb03b-136">Hello následující konfigurační soubor obsahuje hello `<serviceDebug>` značky:</span><span class="sxs-lookup"><span data-stu-id="fb03b-136">hello following configuration file includes hello `<serviceDebug>` tag:</span></span> 
```
<configuration> 
<system.serviceModel> 
<behaviors> 
<serviceBehaviors> 
<behavior name=""MyServiceBehavior""> 
<serviceDebug includeExceptionDetailInFaults=""True"" httpHelpPageEnabled=""True""/> 
... 
```
<span data-ttu-id="fb03b-137">Zakažte ladicí informace ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="fb03b-137">Disable debugging information in hello service.</span></span> <span data-ttu-id="fb03b-138">Toho lze dosáhnout odebráním hello `<serviceDebug>` značky z konfiguračního souboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="fb03b-138">This can be accomplished by removing hello `<serviceDebug>` tag from your application's configuration file.</span></span> 

## <span data-ttu-id="fb03b-139"><a id="servicemetadata"></a>WCF – nezahrnujte serviceMetadata uzlu v konfiguračním souboru</span><span class="sxs-lookup"><span data-stu-id="fb03b-139"><a id="servicemetadata"></a>WCF- Do not include serviceMetadata node in configuration file</span></span>

| <span data-ttu-id="fb03b-140">Název</span><span class="sxs-lookup"><span data-stu-id="fb03b-140">Title</span></span>                   | <span data-ttu-id="fb03b-141">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="fb03b-141">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="fb03b-142">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="fb03b-142">**Component**</span></span>               | <span data-ttu-id="fb03b-143">WCF</span><span class="sxs-lookup"><span data-stu-id="fb03b-143">WCF</span></span> | 
| <span data-ttu-id="fb03b-144">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="fb03b-144">**SDL Phase**</span></span>               | <span data-ttu-id="fb03b-145">Sestavení</span><span class="sxs-lookup"><span data-stu-id="fb03b-145">Build</span></span> |  
| <span data-ttu-id="fb03b-146">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="fb03b-146">**Applicable Technologies**</span></span> | <span data-ttu-id="fb03b-147">Obecné</span><span class="sxs-lookup"><span data-stu-id="fb03b-147">Generic</span></span> |
| <span data-ttu-id="fb03b-148">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="fb03b-148">**Attributes**</span></span>              | <span data-ttu-id="fb03b-149">Obecné, NET Framework 3</span><span class="sxs-lookup"><span data-stu-id="fb03b-149">Generic, NET Framework 3</span></span> |
| <span data-ttu-id="fb03b-150">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="fb03b-150">**References**</span></span>              | <span data-ttu-id="fb03b-151">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení království](https://vulncat.fortify.com/en/vulncat/index.html)</span><span class="sxs-lookup"><span data-stu-id="fb03b-151">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Kingdom](https://vulncat.fortify.com/en/vulncat/index.html)</span></span> |
| <span data-ttu-id="fb03b-152">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="fb03b-152">**Steps**</span></span> | <span data-ttu-id="fb03b-153">Útočníkům cenné přehled o tom, jak může zneužít hello služby můžete poskytnout veřejně odhalení informací o služby.</span><span class="sxs-lookup"><span data-stu-id="fb03b-153">Publicly exposing information about a service can provide attackers with valuable insight into how they might exploit hello service.</span></span> <span data-ttu-id="fb03b-154">Hello `<serviceMetadata>` značky povolí funkci publikování metadat hello.</span><span class="sxs-lookup"><span data-stu-id="fb03b-154">hello `<serviceMetadata>` tag enables hello metadata publishing feature.</span></span> <span data-ttu-id="fb03b-155">Metadata služby můžou obsahovat citlivé informace, které by neměl být veřejně přístupná.</span><span class="sxs-lookup"><span data-stu-id="fb03b-155">Service metadata could contain sensitive information that should not be publicly accessible.</span></span> <span data-ttu-id="fb03b-156">Minimálně povolte jenom důvěryhodní uživatelé tooaccess hello metadata a ujistěte se, že nebude vystavena, víc informací.</span><span class="sxs-lookup"><span data-stu-id="fb03b-156">At a minimum, only allow trusted users tooaccess hello metadata and ensure that unnecessary information is not exposed.</span></span> <span data-ttu-id="fb03b-157">Ještě lepší zcela zakážete hello možnost toopublish metadat.</span><span class="sxs-lookup"><span data-stu-id="fb03b-157">Better yet, entirely disable hello ability toopublish metadata.</span></span> <span data-ttu-id="fb03b-158">Bezpečné konfigurace WCF nebude obsahovat hello `<serviceMetadata>` značky.</span><span class="sxs-lookup"><span data-stu-id="fb03b-158">A safe WCF configuration will not contain hello `<serviceMetadata>` tag.</span></span> |

## <span data-ttu-id="fb03b-159"><a id="exception"></a>Zajistit, že je v rozhraní ASP.NET Web API správné zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="fb03b-159"><a id="exception"></a>Ensure that proper exception handling is done in ASP.NET Web API</span></span>

| <span data-ttu-id="fb03b-160">Název</span><span class="sxs-lookup"><span data-stu-id="fb03b-160">Title</span></span>                   | <span data-ttu-id="fb03b-161">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="fb03b-161">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="fb03b-162">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="fb03b-162">**Component**</span></span>               | <span data-ttu-id="fb03b-163">Web API</span><span class="sxs-lookup"><span data-stu-id="fb03b-163">Web API</span></span> | 
| <span data-ttu-id="fb03b-164">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="fb03b-164">**SDL Phase**</span></span>               | <span data-ttu-id="fb03b-165">Sestavení</span><span class="sxs-lookup"><span data-stu-id="fb03b-165">Build</span></span> |  
| <span data-ttu-id="fb03b-166">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="fb03b-166">**Applicable Technologies**</span></span> | <span data-ttu-id="fb03b-167">MVC 5, 6 MVC</span><span class="sxs-lookup"><span data-stu-id="fb03b-167">MVC 5, MVC 6</span></span> |
| <span data-ttu-id="fb03b-168">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="fb03b-168">**Attributes**</span></span>              | <span data-ttu-id="fb03b-169">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="fb03b-169">N/A</span></span>  |
| <span data-ttu-id="fb03b-170">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="fb03b-170">**References**</span></span>              | <span data-ttu-id="fb03b-171">[Zpracování výjimek v rozhraní ASP.NET Web API](http://www.asp.net/web-api/overview/error-handling/exception-handling), [modelu ověřování v rozhraní ASP.NET Web API](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api)</span><span class="sxs-lookup"><span data-stu-id="fb03b-171">[Exception Handling in ASP.NET Web API](http://www.asp.net/web-api/overview/error-handling/exception-handling), [Model Validation in ASP.NET Web API](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api)</span></span> |
| <span data-ttu-id="fb03b-172">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="fb03b-172">**Steps**</span></span> | <span data-ttu-id="fb03b-173">Ve výchozím nastavení jsou nejvíce nezachycená výjimek v rozhraní ASP.NET Web API přeložit na odpovědi HTTP se stavovým kódem`500, Internal Server Error`</span><span class="sxs-lookup"><span data-stu-id="fb03b-173">By default, most uncaught exceptions in ASP.NET Web API are translated into an HTTP response with status code `500, Internal Server Error`</span></span>|

### <a name="example"></a><span data-ttu-id="fb03b-174">Příklad</span><span class="sxs-lookup"><span data-stu-id="fb03b-174">Example</span></span>
<span data-ttu-id="fb03b-175">toocontrol hello stavový kód vrácený hello rozhraní API, `HttpResponseException` je možné, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="fb03b-175">toocontrol hello status code returned by hello API, `HttpResponseException` can be used as shown below:</span></span> 
```C#
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        throw new HttpResponseException(HttpStatusCode.NotFound);
    }
    return item;
}
```

### <a name="example"></a><span data-ttu-id="fb03b-176">Příklad</span><span class="sxs-lookup"><span data-stu-id="fb03b-176">Example</span></span>
<span data-ttu-id="fb03b-177">Pro další ovládání na odpověď výjimka hello hello `HttpResponseMessage` třídu lze použít, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="fb03b-177">For further control on hello exception response, hello `HttpResponseMessage` class can be used as shown below:</span></span> 
```C#
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        var resp = new HttpResponseMessage(HttpStatusCode.NotFound)
        {
            Content = new StringContent(string.Format("No product with ID = {0}", id)),
            ReasonPhrase = "Product ID Not Found"
        }
        throw new HttpResponseException(resp);
    }
    return item;
}
```
<span data-ttu-id="fb03b-178">toocatch neošetřené výjimky, které nejsou typu hello `HttpResponseException`, lze použít filtry výjimek.</span><span class="sxs-lookup"><span data-stu-id="fb03b-178">toocatch unhandled exceptions that are not of hello type `HttpResponseException`, Exception Filters can be used.</span></span> <span data-ttu-id="fb03b-179">Filtry výjimek implementovat hello `System.Web.Http.Filters.IExceptionFilter` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fb03b-179">Exception filters implement hello `System.Web.Http.Filters.IExceptionFilter` interface.</span></span> <span data-ttu-id="fb03b-180">Nejjednodušší způsob, jak toowrite Hello filtru výjimek je tooderive z hello `System.Web.Http.Filters.ExceptionFilterAttribute` třídy a přepsat metodu OnException hello.</span><span class="sxs-lookup"><span data-stu-id="fb03b-180">hello simplest way toowrite an exception filter is tooderive from hello `System.Web.Http.Filters.ExceptionFilterAttribute` class and override hello OnException method.</span></span> 

### <a name="example"></a><span data-ttu-id="fb03b-181">Příklad</span><span class="sxs-lookup"><span data-stu-id="fb03b-181">Example</span></span>
<span data-ttu-id="fb03b-182">Tady je filtr, který převádí `NotImplementedException` výjimky do stavového kódu protokolu HTTP `501, Not Implemented`:</span><span class="sxs-lookup"><span data-stu-id="fb03b-182">Here is a filter that converts `NotImplementedException` exceptions into HTTP status code `501, Not Implemented`:</span></span> 
```C#
namespace ProductStore.Filters
{
    using System;
    using System.Net;
    using System.Net.Http;
    using System.Web.Http.Filters;

    public class NotImplExceptionFilterAttribute : ExceptionFilterAttribute 
    {
        public override void OnException(HttpActionExecutedContext context)
        {
            if (context.Exception is NotImplementedException)
            {
                context.Response = new HttpResponseMessage(HttpStatusCode.NotImplemented);
            }
        }
    }
}
```

<span data-ttu-id="fb03b-183">Existuje několik způsobů tooregister filtr výjimek webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="fb03b-183">There are several ways tooregister a Web API exception filter:</span></span>
- <span data-ttu-id="fb03b-184">Akce</span><span class="sxs-lookup"><span data-stu-id="fb03b-184">By action</span></span>
- <span data-ttu-id="fb03b-185">Adaptérem</span><span class="sxs-lookup"><span data-stu-id="fb03b-185">By controller</span></span>
- <span data-ttu-id="fb03b-186">Globálně</span><span class="sxs-lookup"><span data-stu-id="fb03b-186">Globally</span></span>

### <a name="example"></a><span data-ttu-id="fb03b-187">Příklad</span><span class="sxs-lookup"><span data-stu-id="fb03b-187">Example</span></span>
<span data-ttu-id="fb03b-188">tooapply hello filtrovat tooa určité akce, přidejte filtr hello jako atribut toohello akce:</span><span class="sxs-lookup"><span data-stu-id="fb03b-188">tooapply hello filter tooa specific action, add hello filter as an attribute toohello action:</span></span> 
```C#
public class ProductsController : ApiController
{
    [NotImplExceptionFilter]
    public Contact GetContact(int id)
    {
        throw new NotImplementedException("This method is not implemented");
    }
}
```
### <a name="example"></a><span data-ttu-id="fb03b-189">Příklad</span><span class="sxs-lookup"><span data-stu-id="fb03b-189">Example</span></span>
<span data-ttu-id="fb03b-190">tooapply hello filtru tooall hello akcí na `controller`, přidejte filtr hello jako atributu toohello `controller` třídy:</span><span class="sxs-lookup"><span data-stu-id="fb03b-190">tooapply hello filter tooall of hello actions on a `controller`, add hello filter as an attribute toohello `controller` class:</span></span> 

```C#
[NotImplExceptionFilter]
public class ProductsController : ApiController
{
    // ...
}
```

### <a name="example"></a><span data-ttu-id="fb03b-191">Příklad</span><span class="sxs-lookup"><span data-stu-id="fb03b-191">Example</span></span>
<span data-ttu-id="fb03b-192">tooapply hello filtrovat globálně tooall webového rozhraní API řadiče, přidá instanci hello filtru toohello `GlobalConfiguration.Configuration.Filters` kolekce.</span><span class="sxs-lookup"><span data-stu-id="fb03b-192">tooapply hello filter globally tooall Web API controllers, add an instance of hello filter toohello `GlobalConfiguration.Configuration.Filters` collection.</span></span> <span data-ttu-id="fb03b-193">Filtry výjimek v této kolekci použít tooany akce kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fb03b-193">Exception filters in this collection apply tooany Web API controller action.</span></span> 
```C#
GlobalConfiguration.Configuration.Filters.Add(
    new ProductStore.NotImplExceptionFilterAttribute());
```

### <a name="example"></a><span data-ttu-id="fb03b-194">Příklad</span><span class="sxs-lookup"><span data-stu-id="fb03b-194">Example</span></span>
<span data-ttu-id="fb03b-195">Pro ověření modelu můžete stav modelu hello předán tooCreateErrorResponse metoda, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="fb03b-195">For model validation, hello model state can be passed tooCreateErrorResponse method as shown below:</span></span> 
```C#
public HttpResponseMessage PostProduct(Product item)
{
    if (!ModelState.IsValid)
    {
        return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
    }
    // Implementation not shown...
}
```

<span data-ttu-id="fb03b-196">Zkontrolujte hello odkazy v části hello odkazy na další podrobnosti o výjimečných zpracování a ověření modelu v rozhraní ASP.Net Web API</span><span class="sxs-lookup"><span data-stu-id="fb03b-196">Check hello links in hello references section for additional details about exceptional handling and model validation in ASP.Net Web API</span></span> 

## <span data-ttu-id="fb03b-197"><a id="messages"></a>Podrobné informace o zabezpečení v chybových zprávách, neuvádějí</span><span class="sxs-lookup"><span data-stu-id="fb03b-197"><a id="messages"></a>Do not expose security details in error messages</span></span>

| <span data-ttu-id="fb03b-198">Název</span><span class="sxs-lookup"><span data-stu-id="fb03b-198">Title</span></span>                   | <span data-ttu-id="fb03b-199">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="fb03b-199">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="fb03b-200">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="fb03b-200">**Component**</span></span>               | <span data-ttu-id="fb03b-201">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="fb03b-201">Web Application</span></span> | 
| <span data-ttu-id="fb03b-202">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="fb03b-202">**SDL Phase**</span></span>               | <span data-ttu-id="fb03b-203">Sestavení</span><span class="sxs-lookup"><span data-stu-id="fb03b-203">Build</span></span> |  
| <span data-ttu-id="fb03b-204">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="fb03b-204">**Applicable Technologies**</span></span> | <span data-ttu-id="fb03b-205">Obecné</span><span class="sxs-lookup"><span data-stu-id="fb03b-205">Generic</span></span> |
| <span data-ttu-id="fb03b-206">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="fb03b-206">**Attributes**</span></span>              | <span data-ttu-id="fb03b-207">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="fb03b-207">N/A</span></span>  |
| <span data-ttu-id="fb03b-208">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="fb03b-208">**References**</span></span>              | <span data-ttu-id="fb03b-209">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="fb03b-209">N/A</span></span>  |
| <span data-ttu-id="fb03b-210">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="fb03b-210">**Steps**</span></span> | <p><span data-ttu-id="fb03b-211">Obecné chybové zprávy jsou poskytovány přímo toohello uživatele bez včetně dat citlivé aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fb03b-211">Generic error messages are provided directly toohello user without including sensitive application data.</span></span> <span data-ttu-id="fb03b-212">Příklady citlivých dat:</span><span class="sxs-lookup"><span data-stu-id="fb03b-212">Examples of sensitive data include:</span></span></p><ul><li><span data-ttu-id="fb03b-213">Názvy serverů</span><span class="sxs-lookup"><span data-stu-id="fb03b-213">Server names</span></span></li><li><span data-ttu-id="fb03b-214">Připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="fb03b-214">Connection strings</span></span></li><li><span data-ttu-id="fb03b-215">Uživatelská jména</span><span class="sxs-lookup"><span data-stu-id="fb03b-215">Usernames</span></span></li><li><span data-ttu-id="fb03b-216">Hesla</span><span class="sxs-lookup"><span data-stu-id="fb03b-216">Passwords</span></span></li><li><span data-ttu-id="fb03b-217">Postupy SQL</span><span class="sxs-lookup"><span data-stu-id="fb03b-217">SQL procedures</span></span></li><li><span data-ttu-id="fb03b-218">Podrobnosti o selhání dynamické SQL</span><span class="sxs-lookup"><span data-stu-id="fb03b-218">Details of dynamic SQL failures</span></span></li><li><span data-ttu-id="fb03b-219">Trasování zásobníku a řádky kódu</span><span class="sxs-lookup"><span data-stu-id="fb03b-219">Stack trace and lines of code</span></span></li><li><span data-ttu-id="fb03b-220">Proměnné uložené v paměti</span><span class="sxs-lookup"><span data-stu-id="fb03b-220">Variables stored in memory</span></span></li><li><span data-ttu-id="fb03b-221">Umístění jednotky a složky</span><span class="sxs-lookup"><span data-stu-id="fb03b-221">Drive and folder locations</span></span></li><li><span data-ttu-id="fb03b-222">Body instalace aplikací</span><span class="sxs-lookup"><span data-stu-id="fb03b-222">Application install points</span></span></li><li><span data-ttu-id="fb03b-223">Nastavení konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="fb03b-223">Host configuration settings</span></span></li><li><span data-ttu-id="fb03b-224">Další podrobnosti o interní aplikace</span><span class="sxs-lookup"><span data-stu-id="fb03b-224">Other internal application details</span></span></li></ul><p><span data-ttu-id="fb03b-225">Zachycení všech chyb v aplikaci a poskytuje obecné chybové zprávy, jakož i povolení vlastní chyby v rámci služby IIS vám pomůže zabránili odhalení informací.</span><span class="sxs-lookup"><span data-stu-id="fb03b-225">Trapping all errors within an application and providing generic error messages, as well as enabling custom errors within IIS will help prevent information disclosure.</span></span> <span data-ttu-id="fb03b-226">Databáze systému SQL Server a .NET zpracování výjimek, mezi jiné chybě zpracování architektury, jsou uživatel se zlými úmysly zejména podrobné a velmi užitečné tooa profilace aplikace.</span><span class="sxs-lookup"><span data-stu-id="fb03b-226">SQL Server database and .NET Exception handling, among other error handling architectures, are especially verbose and extremely useful tooa malicious user profiling your application.</span></span> <span data-ttu-id="fb03b-227">To není přímo hello zobrazení obsahu třídy odvozené od třídy výjimky .NET hello a zajistěte, abyste měli správné zpracování výjimek, aby neočekávané výjimky nechtěně není vyvolána přímo toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="fb03b-227">Do not directly display hello contents of a class derived from hello .NET Exception class, and ensure that you have proper exception handling so that an unexpected exception isn't inadvertently raised directly toohello user.</span></span></p><ul><li><span data-ttu-id="fb03b-228">Poskytují obecné chybové zprávy přímo toohello uživatele, který abstraktní tokeny konkrétní podrobnosti najít přímo v hello výjimky nebo chybové zprávě</span><span class="sxs-lookup"><span data-stu-id="fb03b-228">Provide generic error messages directly toohello user that abstract away specific details found directly in hello exception/error message</span></span></li><li><span data-ttu-id="fb03b-229">Není obsah zobrazení hello výjimky .NET přímo třídu toohello uživatele</span><span class="sxs-lookup"><span data-stu-id="fb03b-229">Do not display hello contents of a .NET exception class directly toohello user</span></span></li><li><span data-ttu-id="fb03b-230">Depeše všechny chybové zprávy a podle potřeby informovat uživatele hello prostřednictvím obecnou chybovou zprávu odeslané toohello aplikace klienta</span><span class="sxs-lookup"><span data-stu-id="fb03b-230">Trap all error messages and if appropriate inform hello user via a generic error message sent toohello application client</span></span></li><li><span data-ttu-id="fb03b-231">Nevystavujte hello obsah třídy výjimek hello přímo toohello uživatele, zejména hello návratová hodnota z `.ToString()`, nebo hello hodnoty vlastností hello zpráv nebo trasování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="fb03b-231">Do not expose hello contents of hello Exception class directly toohello user, especially hello return value from `.ToString()`, or hello values of hello Message or StackTrace properties.</span></span> <span data-ttu-id="fb03b-232">Bezpečně protokolu tyto informace a zobrazit více neškodné uživatele toohello zpráv</span><span class="sxs-lookup"><span data-stu-id="fb03b-232">Securely log this information and display a more innocuous message toohello user</span></span></li></ul>|

## <span data-ttu-id="fb03b-233"><a id="default"></a>Implementace výchozí chyba zpracování stránky</span><span class="sxs-lookup"><span data-stu-id="fb03b-233"><a id="default"></a>Implement Default error handling page</span></span>

| <span data-ttu-id="fb03b-234">Název</span><span class="sxs-lookup"><span data-stu-id="fb03b-234">Title</span></span>                   | <span data-ttu-id="fb03b-235">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="fb03b-235">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="fb03b-236">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="fb03b-236">**Component**</span></span>               | <span data-ttu-id="fb03b-237">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="fb03b-237">Web Application</span></span> | 
| <span data-ttu-id="fb03b-238">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="fb03b-238">**SDL Phase**</span></span>               | <span data-ttu-id="fb03b-239">Sestavení</span><span class="sxs-lookup"><span data-stu-id="fb03b-239">Build</span></span> |  
| <span data-ttu-id="fb03b-240">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="fb03b-240">**Applicable Technologies**</span></span> | <span data-ttu-id="fb03b-241">Obecné</span><span class="sxs-lookup"><span data-stu-id="fb03b-241">Generic</span></span> |
| <span data-ttu-id="fb03b-242">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="fb03b-242">**Attributes**</span></span>              | <span data-ttu-id="fb03b-243">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="fb03b-243">N/A</span></span>  |
| <span data-ttu-id="fb03b-244">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="fb03b-244">**References**</span></span>              | <span data-ttu-id="fb03b-245">[Upravit dialogové okno nastavení stránky ASP.NET chyby](https://technet.microsoft.com/library/dd569096(WS.10).aspx)</span><span class="sxs-lookup"><span data-stu-id="fb03b-245">[Edit ASP.NET Error Pages Settings Dialog Box](https://technet.microsoft.com/library/dd569096(WS.10).aspx)</span></span> |
| <span data-ttu-id="fb03b-246">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="fb03b-246">**Steps**</span></span> | <p><span data-ttu-id="fb03b-247">Pokud aplikace ASP.NET nezdaří a dojde HTTP/1.x 500 Vnitřní chyba serveru nebo konfigurace funkcí (například filtrování požadavků) zabraňuje zobrazení stránky, se budou generovat chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="fb03b-247">When an ASP.NET application fails and causes an HTTP/1.x 500 Internal Server Error, or a feature configuration (such as Request Filtering) prevents a page from being displayed, an error message will be generated.</span></span> <span data-ttu-id="fb03b-248">Můžou správci rozhodnout, zda má aplikace hello zobrazit popisnou zprávu toohello klienta, podrobné chybové zprávy toohello klienta nebo podrobné chybové zprávy jenom toolocalhost.</span><span class="sxs-lookup"><span data-stu-id="fb03b-248">Administrators can choose whether or not hello application should display a friendly message toohello client, detailed error message toohello client, or detailed error message toolocalhost only.</span></span> <span data-ttu-id="fb03b-249">Hello <customErrors> v souboru web.config hello má tři režimy:</span><span class="sxs-lookup"><span data-stu-id="fb03b-249">hello <customErrors> tag in hello web.config has three modes:</span></span></p><ul><li><span data-ttu-id="fb03b-250">**Na:** Určuje, že jsou povoleny vlastní chyby.</span><span class="sxs-lookup"><span data-stu-id="fb03b-250">**On:** Specifies that custom errors are enabled.</span></span> <span data-ttu-id="fb03b-251">Pokud není zadán žádný atribut defaultRedirect, uživatelé uvidí Obecná chyba.</span><span class="sxs-lookup"><span data-stu-id="fb03b-251">If no defaultRedirect attribute is specified, users see a generic error.</span></span> <span data-ttu-id="fb03b-252">vlastní chyby Hello jsou zobrazeny toohello vzdálení klienti a toohello místního hostitele</span><span class="sxs-lookup"><span data-stu-id="fb03b-252">hello custom errors are shown toohello remote clients and toohello local host</span></span></li><li><span data-ttu-id="fb03b-253">**Vypnutí:** Určuje, že vlastní chyby jsou vypnuté.</span><span class="sxs-lookup"><span data-stu-id="fb03b-253">**Off:** Specifies that custom errors are disabled.</span></span> <span data-ttu-id="fb03b-254">Hello podrobné chyby technologie ASP.NET se zobrazují toohello vzdálení klienti a toohello místního hostitele</span><span class="sxs-lookup"><span data-stu-id="fb03b-254">hello detailed ASP.NET errors are shown toohello remote clients and toohello local host</span></span></li><li><span data-ttu-id="fb03b-255">**RemoteOnly:** Určuje, že vlastní chyby jsou zobrazeny pouze toohello vzdálení klienti a že ASP.NET chyby jsou zobrazeny toohello místního hostitele.</span><span class="sxs-lookup"><span data-stu-id="fb03b-255">**RemoteOnly:** Specifies that custom errors are shown only toohello remote clients, and that ASP.NET errors are shown toohello local host.</span></span> <span data-ttu-id="fb03b-256">Toto je výchozí hodnota hello</span><span class="sxs-lookup"><span data-stu-id="fb03b-256">This is hello default value</span></span></li></ul><p><span data-ttu-id="fb03b-257">Otevřete hello `web.config` souboru hello aplikace nebo webu a zkontrolujte, zda text hello značka má `<customErrors mode="RemoteOnly" />` nebo `<customErrors mode="On" />` definované.</span><span class="sxs-lookup"><span data-stu-id="fb03b-257">Open hello `web.config` file for hello application/site and ensure that hello tag has either `<customErrors mode="RemoteOnly" />` or `<customErrors mode="On" />` defined.</span></span></p>|

## <span data-ttu-id="fb03b-258"><a id="deployment"></a>Nastavte metodu nasazení tooRetail ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="fb03b-258"><a id="deployment"></a>Set Deployment Method tooRetail in IIS</span></span>

| <span data-ttu-id="fb03b-259">Název</span><span class="sxs-lookup"><span data-stu-id="fb03b-259">Title</span></span>                   | <span data-ttu-id="fb03b-260">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="fb03b-260">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="fb03b-261">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="fb03b-261">**Component**</span></span>               | <span data-ttu-id="fb03b-262">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="fb03b-262">Web Application</span></span> | 
| <span data-ttu-id="fb03b-263">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="fb03b-263">**SDL Phase**</span></span>               | <span data-ttu-id="fb03b-264">Nasazení</span><span class="sxs-lookup"><span data-stu-id="fb03b-264">Deployment</span></span> |  
| <span data-ttu-id="fb03b-265">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="fb03b-265">**Applicable Technologies**</span></span> | <span data-ttu-id="fb03b-266">Obecné</span><span class="sxs-lookup"><span data-stu-id="fb03b-266">Generic</span></span> |
| <span data-ttu-id="fb03b-267">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="fb03b-267">**Attributes**</span></span>              | <span data-ttu-id="fb03b-268">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="fb03b-268">N/A</span></span>  |
| <span data-ttu-id="fb03b-269">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="fb03b-269">**References**</span></span>              | <span data-ttu-id="fb03b-270">[nasazení – Element (schéma nastavení ASP.NET)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx)</span><span class="sxs-lookup"><span data-stu-id="fb03b-270">[deployment Element (ASP.NET Settings Schema)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx)</span></span> |
| <span data-ttu-id="fb03b-271">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="fb03b-271">**Steps**</span></span> | <p><span data-ttu-id="fb03b-272">Hello `<deployment retail>` přepínač je určená pro produkční servery služby IIS.</span><span class="sxs-lookup"><span data-stu-id="fb03b-272">hello `<deployment retail>` switch is intended for use by production IIS servers.</span></span> <span data-ttu-id="fb03b-273">Tento přepínač je použité toohelp se spouští s hello nejlepší možný výkon aplikace a nejmenší informace o zabezpečení, že úniků zakázáním hello aplikace možnost toogenerate výstup trasování na stránce, zákaz toodisplay možnost hello podrobná chyba Uživatelé tooend zprávy a deaktivace hello ladění přepínače.</span><span class="sxs-lookup"><span data-stu-id="fb03b-273">This switch is used toohelp applications run with hello best possible performance and least possible security information leakages by disabling hello application's ability toogenerate trace output on a page, disabling hello ability toodisplay detailed error messages tooend users, and disabling hello debug switch.</span></span></p><p><span data-ttu-id="fb03b-274">Často, přepínače a možnosti, které jsou zaměřené na vývojáře, například se nezdařilo žádosti o trasování a ladění, jsou povolené během vývoje aktivní.</span><span class="sxs-lookup"><span data-stu-id="fb03b-274">Often times, switches and options that are developer-focused, such as failed request tracing and debugging, are enabled during active development.</span></span> <span data-ttu-id="fb03b-275">Doporučuje se, že hello metodu nasazení na žádném serveru pro produkční nastavit tooretail.</span><span class="sxs-lookup"><span data-stu-id="fb03b-275">It is recommended that hello deployment method on any production server be set tooretail.</span></span> <span data-ttu-id="fb03b-276">Otevřete soubor hello souboru machine.config a ujistěte se, že `<deployment retail="true" />` zůstane nastavit tootrue.</span><span class="sxs-lookup"><span data-stu-id="fb03b-276">open hello machine.config file and ensure that `<deployment retail="true" />` remains set tootrue.</span></span></p>|

## <span data-ttu-id="fb03b-277"><a id="fail"></a>Výjimky má bezpečně selhat.</span><span class="sxs-lookup"><span data-stu-id="fb03b-277"><a id="fail"></a>Exceptions should fail safely</span></span>

| <span data-ttu-id="fb03b-278">Název</span><span class="sxs-lookup"><span data-stu-id="fb03b-278">Title</span></span>                   | <span data-ttu-id="fb03b-279">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="fb03b-279">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="fb03b-280">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="fb03b-280">**Component**</span></span>               | <span data-ttu-id="fb03b-281">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="fb03b-281">Web Application</span></span> | 
| <span data-ttu-id="fb03b-282">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="fb03b-282">**SDL Phase**</span></span>               | <span data-ttu-id="fb03b-283">Sestavení</span><span class="sxs-lookup"><span data-stu-id="fb03b-283">Build</span></span> |  
| <span data-ttu-id="fb03b-284">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="fb03b-284">**Applicable Technologies**</span></span> | <span data-ttu-id="fb03b-285">Obecné</span><span class="sxs-lookup"><span data-stu-id="fb03b-285">Generic</span></span> |
| <span data-ttu-id="fb03b-286">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="fb03b-286">**Attributes**</span></span>              | <span data-ttu-id="fb03b-287">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="fb03b-287">N/A</span></span>  |
| <span data-ttu-id="fb03b-288">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="fb03b-288">**References**</span></span>              | [<span data-ttu-id="fb03b-289">Bezpečně nezdaří</span><span class="sxs-lookup"><span data-stu-id="fb03b-289">Fail securely</span></span>](https://www.owasp.org/index.php/Fail_securely) |
| <span data-ttu-id="fb03b-290">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="fb03b-290">**Steps**</span></span> | <span data-ttu-id="fb03b-291">Aplikace má bezpečně selhat.</span><span class="sxs-lookup"><span data-stu-id="fb03b-291">Application should fail safely.</span></span> <span data-ttu-id="fb03b-292">Libovolné metody, která vrací logickou hodnotu, podle které určité rozhoduje, měli pečlivě vytvořit bloku výjimky.</span><span class="sxs-lookup"><span data-stu-id="fb03b-292">Any method that returns a Boolean value, based on which certain decision is made, should have exception block carefully created.</span></span> <span data-ttu-id="fb03b-293">Existuje mnoho logických chyb z důvodu nárůstu problémy zabezpečení toowhich v při neuváženě zápisu bloku výjimky hello.</span><span class="sxs-lookup"><span data-stu-id="fb03b-293">There are lot of logical errors due toowhich security issues creep in, when hello exception block is written carelessly.</span></span>|

### <a name="example"></a><span data-ttu-id="fb03b-294">Příklad</span><span class="sxs-lookup"><span data-stu-id="fb03b-294">Example</span></span>
```C#
        public static bool ValidateDomain(string pathToValidate, Uri currentUrl)
        {
            try
            {
                if (!string.IsNullOrWhiteSpace(pathToValidate))
                {
                    var domain = RetrieveDomain(currentUrl);
                    var replyPath = new Uri(pathToValidate);
                    var replyDomain = RetrieveDomain(replyPath);

                    if (string.Compare(domain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                    {
                        //// Adding additional check tooenable CMS urls if they are not hosted on same domain.
                        if (!string.IsNullOrWhiteSpace(Utilities.CmsBase))
                        {
                            var cmsDomain = RetrieveDomain(new Uri(Utilities.Base.Trim()));
                            if (string.Compare(cmDomain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                            {
                                return false;
                            }
                            else
                            {
                                return true;
                            }
                        }

                        return false;
                    }
                }

                return true;
            }
            catch (UriFormatException ex)
            {
                LogHelper.LogException("Utilities:ValidateDomain", ex);
                return true;
            }
        }
```
<span data-ttu-id="fb03b-295">Hello výše metoda vždy vrátí hodnotu True, pokud dojde k výjimce.</span><span class="sxs-lookup"><span data-stu-id="fb03b-295">hello above method will always return True, if some exception happens.</span></span> <span data-ttu-id="fb03b-296">Pokud koncový uživatel hello poskytuje poškozená adresa URL, které hello ohledech prohlížeče, ale hello `Uri()` konstruktor není to vyvolá výjimku a postižené hello se provedou toohello platný, ale chybná adresa URL.</span><span class="sxs-lookup"><span data-stu-id="fb03b-296">If hello end user provides a malformed URL, that hello browser respects, but hello `Uri()` constructor doesn't, this will throw an exception, and hello victim will be taken toohello valid but malformed URL.</span></span> 
