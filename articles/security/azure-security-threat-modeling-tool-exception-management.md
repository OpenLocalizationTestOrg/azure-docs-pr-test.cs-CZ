---
title: "Výjimka Management - nástroj Microsoft Threat modelování – Azure | Microsoft Docs"
description: "způsoby zmírnění hrozeb, které jsou zveřejněné v nástroji pro modelování hrozeb"
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
ms.openlocfilehash: bbf357b902474a1812eb7a5a2c914d0c8b91934b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="security-frame-exception-management--mitigations"></a><span data-ttu-id="95b3c-103">Zabezpečení rámce: Správa výjimek | Způsoby zmírnění rizik</span><span class="sxs-lookup"><span data-stu-id="95b3c-103">Security Frame: Exception Management | Mitigations</span></span> 
| <span data-ttu-id="95b3c-104">Produktům a službám</span><span class="sxs-lookup"><span data-stu-id="95b3c-104">Product/Service</span></span> | <span data-ttu-id="95b3c-105">Článek</span><span class="sxs-lookup"><span data-stu-id="95b3c-105">Article</span></span> |
| --------------- | ------- |
| <span data-ttu-id="95b3c-106">**WCF**</span><span class="sxs-lookup"><span data-stu-id="95b3c-106">**WCF**</span></span> | <ul><li>[<span data-ttu-id="95b3c-107">WCF – nezahrnujte serviceDebug uzlu v konfiguračním souboru</span><span class="sxs-lookup"><span data-stu-id="95b3c-107">WCF- Do not include serviceDebug node in configuration file</span></span>](#servicedebug)</li><li>[<span data-ttu-id="95b3c-108">WCF – nezahrnujte serviceMetadata uzlu v konfiguračním souboru</span><span class="sxs-lookup"><span data-stu-id="95b3c-108">WCF- Do not include serviceMetadata node in configuration file</span></span>](#servicemetadata)</li></ul> |
| <span data-ttu-id="95b3c-109">**Webové rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="95b3c-109">**Web API**</span></span> | <ul><li>[<span data-ttu-id="95b3c-110">Zajistit, že je v rozhraní ASP.NET Web API správné zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="95b3c-110">Ensure that proper exception handling is done in ASP.NET Web API </span></span>](#exception)</li></ul> |
| <span data-ttu-id="95b3c-111">**Webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="95b3c-111">**Web Application**</span></span> | <ul><li>[<span data-ttu-id="95b3c-112">Podrobné informace o zabezpečení v chybových zprávách, neuvádějí</span><span class="sxs-lookup"><span data-stu-id="95b3c-112">Do not expose security details in error messages </span></span>](#messages)</li><li>[<span data-ttu-id="95b3c-113">Implementace výchozí chyba zpracování stránky</span><span class="sxs-lookup"><span data-stu-id="95b3c-113">Implement Default error handling page </span></span>](#default)</li><li>[<span data-ttu-id="95b3c-114">Nastavte metodu nasazení prodejní ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="95b3c-114">Set Deployment Method to Retail in IIS</span></span>](#deployment)</li><li>[<span data-ttu-id="95b3c-115">Výjimky má bezpečně selhat.</span><span class="sxs-lookup"><span data-stu-id="95b3c-115">Exceptions should fail safely</span></span>](#fail)</li></ul> |

## <span data-ttu-id="95b3c-116"><a id="servicedebug"></a>WCF – nezahrnujte serviceDebug uzlu v konfiguračním souboru</span><span class="sxs-lookup"><span data-stu-id="95b3c-116"><a id="servicedebug"></a>WCF- Do not include serviceDebug node in configuration file</span></span>

| <span data-ttu-id="95b3c-117">Název</span><span class="sxs-lookup"><span data-stu-id="95b3c-117">Title</span></span>                   | <span data-ttu-id="95b3c-118">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="95b3c-118">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="95b3c-119">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="95b3c-119">**Component**</span></span>               | <span data-ttu-id="95b3c-120">WCF</span><span class="sxs-lookup"><span data-stu-id="95b3c-120">WCF</span></span> | 
| <span data-ttu-id="95b3c-121">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="95b3c-121">**SDL Phase**</span></span>               | <span data-ttu-id="95b3c-122">Sestavení</span><span class="sxs-lookup"><span data-stu-id="95b3c-122">Build</span></span> |  
| <span data-ttu-id="95b3c-123">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="95b3c-123">**Applicable Technologies**</span></span> | <span data-ttu-id="95b3c-124">Obecné, NET Framework 3</span><span class="sxs-lookup"><span data-stu-id="95b3c-124">Generic, NET Framework 3</span></span> |
| <span data-ttu-id="95b3c-125">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="95b3c-125">**Attributes**</span></span>              | <span data-ttu-id="95b3c-126">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="95b3c-126">N/A</span></span>  |
| <span data-ttu-id="95b3c-127">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="95b3c-127">**References**</span></span>              | <span data-ttu-id="95b3c-128">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení království](https://vulncat.fortify.com/en/vulncat/index.html)</span><span class="sxs-lookup"><span data-stu-id="95b3c-128">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Kingdom](https://vulncat.fortify.com/en/vulncat/index.html)</span></span> |
| <span data-ttu-id="95b3c-129">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="95b3c-129">**Steps**</span></span> | <span data-ttu-id="95b3c-130">Ke zveřejnění informace o ladění lze nakonfigurovat komunikaci Framework služby Windows (WCF).</span><span class="sxs-lookup"><span data-stu-id="95b3c-130">Windows Communication Framework (WCF) services can be configured to expose debugging information.</span></span> <span data-ttu-id="95b3c-131">Ladění informace nesmí použít v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="95b3c-131">Debug information should not be used in production environments.</span></span> <span data-ttu-id="95b3c-132">`<serviceDebug>` Značky definuje, zda je povolena funkce informace o ladění služby WCF.</span><span class="sxs-lookup"><span data-stu-id="95b3c-132">The `<serviceDebug>` tag defines whether the debug information feature is enabled for a WCF service.</span></span> <span data-ttu-id="95b3c-133">Pokud třídu includeExceptionDetailInFaults atributu je nastavena na hodnotu true, informace o výjimce z aplikace, bude vrácen klientům.</span><span class="sxs-lookup"><span data-stu-id="95b3c-133">If the attribute includeExceptionDetailInFaults is set to true, exception information from the application will be returned to clients.</span></span> <span data-ttu-id="95b3c-134">Útočníci můžou využít další informace, které získají z ladění výstupu připojit útoků, které cílí na rozhraní, databázi nebo jiným prostředkům, které používá aplikace.</span><span class="sxs-lookup"><span data-stu-id="95b3c-134">Attackers can leverage the additional information they gain from debugging output to mount attacks targeted on the framework, database, or other resources used by the application.</span></span> |

### <a name="example"></a><span data-ttu-id="95b3c-135">Příklad</span><span class="sxs-lookup"><span data-stu-id="95b3c-135">Example</span></span>
<span data-ttu-id="95b3c-136">Zahrnuje následující konfigurační soubor `<serviceDebug>` značky:</span><span class="sxs-lookup"><span data-stu-id="95b3c-136">The following configuration file includes the `<serviceDebug>` tag:</span></span> 
```
<configuration> 
<system.serviceModel> 
<behaviors> 
<serviceBehaviors> 
<behavior name=""MyServiceBehavior""> 
<serviceDebug includeExceptionDetailInFaults=""True"" httpHelpPageEnabled=""True""/> 
... 
```
<span data-ttu-id="95b3c-137">Zakažte ladicí informace ve službě.</span><span class="sxs-lookup"><span data-stu-id="95b3c-137">Disable debugging information in the service.</span></span> <span data-ttu-id="95b3c-138">Toho lze dosáhnout odebráním `<serviceDebug>` značky z konfiguračního souboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="95b3c-138">This can be accomplished by removing the `<serviceDebug>` tag from your application's configuration file.</span></span> 

## <span data-ttu-id="95b3c-139"><a id="servicemetadata"></a>WCF – nezahrnujte serviceMetadata uzlu v konfiguračním souboru</span><span class="sxs-lookup"><span data-stu-id="95b3c-139"><a id="servicemetadata"></a>WCF- Do not include serviceMetadata node in configuration file</span></span>

| <span data-ttu-id="95b3c-140">Název</span><span class="sxs-lookup"><span data-stu-id="95b3c-140">Title</span></span>                   | <span data-ttu-id="95b3c-141">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="95b3c-141">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="95b3c-142">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="95b3c-142">**Component**</span></span>               | <span data-ttu-id="95b3c-143">WCF</span><span class="sxs-lookup"><span data-stu-id="95b3c-143">WCF</span></span> | 
| <span data-ttu-id="95b3c-144">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="95b3c-144">**SDL Phase**</span></span>               | <span data-ttu-id="95b3c-145">Sestavení</span><span class="sxs-lookup"><span data-stu-id="95b3c-145">Build</span></span> |  
| <span data-ttu-id="95b3c-146">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="95b3c-146">**Applicable Technologies**</span></span> | <span data-ttu-id="95b3c-147">Obecné</span><span class="sxs-lookup"><span data-stu-id="95b3c-147">Generic</span></span> |
| <span data-ttu-id="95b3c-148">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="95b3c-148">**Attributes**</span></span>              | <span data-ttu-id="95b3c-149">Obecné, NET Framework 3</span><span class="sxs-lookup"><span data-stu-id="95b3c-149">Generic, NET Framework 3</span></span> |
| <span data-ttu-id="95b3c-150">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="95b3c-150">**References**</span></span>              | <span data-ttu-id="95b3c-151">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení království](https://vulncat.fortify.com/en/vulncat/index.html)</span><span class="sxs-lookup"><span data-stu-id="95b3c-151">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Kingdom](https://vulncat.fortify.com/en/vulncat/index.html)</span></span> |
| <span data-ttu-id="95b3c-152">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="95b3c-152">**Steps**</span></span> | <span data-ttu-id="95b3c-153">Útočníkům cenné přehled o tom, jak mohou využívat služby můžete poskytnout veřejně odhalení informací o služby.</span><span class="sxs-lookup"><span data-stu-id="95b3c-153">Publicly exposing information about a service can provide attackers with valuable insight into how they might exploit the service.</span></span> <span data-ttu-id="95b3c-154">`<serviceMetadata>` Značky povolí funkci publikování metadat.</span><span class="sxs-lookup"><span data-stu-id="95b3c-154">The `<serviceMetadata>` tag enables the metadata publishing feature.</span></span> <span data-ttu-id="95b3c-155">Metadata služby můžou obsahovat citlivé informace, které by neměl být veřejně přístupná.</span><span class="sxs-lookup"><span data-stu-id="95b3c-155">Service metadata could contain sensitive information that should not be publicly accessible.</span></span> <span data-ttu-id="95b3c-156">Minimálně povolíte jenom důvěryhodným uživatelům přístup k metadatům a ujistěte se, že nebude vystavena, víc informací.</span><span class="sxs-lookup"><span data-stu-id="95b3c-156">At a minimum, only allow trusted users to access the metadata and ensure that unnecessary information is not exposed.</span></span> <span data-ttu-id="95b3c-157">Ještě lepší zcela zakážete možnost publikování metadat.</span><span class="sxs-lookup"><span data-stu-id="95b3c-157">Better yet, entirely disable the ability to publish metadata.</span></span> <span data-ttu-id="95b3c-158">Nebude obsahovat bezpečné konfigurace WCF `<serviceMetadata>` značky.</span><span class="sxs-lookup"><span data-stu-id="95b3c-158">A safe WCF configuration will not contain the `<serviceMetadata>` tag.</span></span> |

## <span data-ttu-id="95b3c-159"><a id="exception"></a>Zajistit, že je v rozhraní ASP.NET Web API správné zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="95b3c-159"><a id="exception"></a>Ensure that proper exception handling is done in ASP.NET Web API</span></span>

| <span data-ttu-id="95b3c-160">Název</span><span class="sxs-lookup"><span data-stu-id="95b3c-160">Title</span></span>                   | <span data-ttu-id="95b3c-161">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="95b3c-161">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="95b3c-162">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="95b3c-162">**Component**</span></span>               | <span data-ttu-id="95b3c-163">Web API</span><span class="sxs-lookup"><span data-stu-id="95b3c-163">Web API</span></span> | 
| <span data-ttu-id="95b3c-164">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="95b3c-164">**SDL Phase**</span></span>               | <span data-ttu-id="95b3c-165">Sestavení</span><span class="sxs-lookup"><span data-stu-id="95b3c-165">Build</span></span> |  
| <span data-ttu-id="95b3c-166">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="95b3c-166">**Applicable Technologies**</span></span> | <span data-ttu-id="95b3c-167">MVC 5, 6 MVC</span><span class="sxs-lookup"><span data-stu-id="95b3c-167">MVC 5, MVC 6</span></span> |
| <span data-ttu-id="95b3c-168">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="95b3c-168">**Attributes**</span></span>              | <span data-ttu-id="95b3c-169">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="95b3c-169">N/A</span></span>  |
| <span data-ttu-id="95b3c-170">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="95b3c-170">**References**</span></span>              | <span data-ttu-id="95b3c-171">[Zpracování výjimek v rozhraní ASP.NET Web API](http://www.asp.net/web-api/overview/error-handling/exception-handling), [modelu ověřování v rozhraní ASP.NET Web API](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api)</span><span class="sxs-lookup"><span data-stu-id="95b3c-171">[Exception Handling in ASP.NET Web API](http://www.asp.net/web-api/overview/error-handling/exception-handling), [Model Validation in ASP.NET Web API](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api)</span></span> |
| <span data-ttu-id="95b3c-172">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="95b3c-172">**Steps**</span></span> | <span data-ttu-id="95b3c-173">Ve výchozím nastavení jsou nejvíce nezachycená výjimek v rozhraní ASP.NET Web API přeložit na odpovědi HTTP se stavovým kódem`500, Internal Server Error`</span><span class="sxs-lookup"><span data-stu-id="95b3c-173">By default, most uncaught exceptions in ASP.NET Web API are translated into an HTTP response with status code `500, Internal Server Error`</span></span>|

### <a name="example"></a><span data-ttu-id="95b3c-174">Příklad</span><span class="sxs-lookup"><span data-stu-id="95b3c-174">Example</span></span>
<span data-ttu-id="95b3c-175">K řízení vrátil rozhraní API, kód stavu `HttpResponseException` je možné, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="95b3c-175">To control the status code returned by the API, `HttpResponseException` can be used as shown below:</span></span> 
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

### <a name="example"></a><span data-ttu-id="95b3c-176">Příklad</span><span class="sxs-lookup"><span data-stu-id="95b3c-176">Example</span></span>
<span data-ttu-id="95b3c-177">Pro další ovládací prvek v odpovědi výjimka `HttpResponseMessage` třídu lze použít, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="95b3c-177">For further control on the exception response, the `HttpResponseMessage` class can be used as shown below:</span></span> 
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
<span data-ttu-id="95b3c-178">K zachycení neošetřených výjimek, které nejsou typu `HttpResponseException`, lze použít filtry výjimek.</span><span class="sxs-lookup"><span data-stu-id="95b3c-178">To catch unhandled exceptions that are not of the type `HttpResponseException`, Exception Filters can be used.</span></span> <span data-ttu-id="95b3c-179">Filtry výjimek implementovat `System.Web.Http.Filters.IExceptionFilter` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="95b3c-179">Exception filters implement the `System.Web.Http.Filters.IExceptionFilter` interface.</span></span> <span data-ttu-id="95b3c-180">Nejjednodušší způsob, jak zápis filtru výjimek je odvozena od `System.Web.Http.Filters.ExceptionFilterAttribute` třídy a přepsat metodu OnException.</span><span class="sxs-lookup"><span data-stu-id="95b3c-180">The simplest way to write an exception filter is to derive from the `System.Web.Http.Filters.ExceptionFilterAttribute` class and override the OnException method.</span></span> 

### <a name="example"></a><span data-ttu-id="95b3c-181">Příklad</span><span class="sxs-lookup"><span data-stu-id="95b3c-181">Example</span></span>
<span data-ttu-id="95b3c-182">Tady je filtr, který převádí `NotImplementedException` výjimky do stavového kódu protokolu HTTP `501, Not Implemented`:</span><span class="sxs-lookup"><span data-stu-id="95b3c-182">Here is a filter that converts `NotImplementedException` exceptions into HTTP status code `501, Not Implemented`:</span></span> 
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

<span data-ttu-id="95b3c-183">Existuje několik způsobů, jak zaregistrovat filtr výjimek webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="95b3c-183">There are several ways to register a Web API exception filter:</span></span>
- <span data-ttu-id="95b3c-184">Akce</span><span class="sxs-lookup"><span data-stu-id="95b3c-184">By action</span></span>
- <span data-ttu-id="95b3c-185">Adaptérem</span><span class="sxs-lookup"><span data-stu-id="95b3c-185">By controller</span></span>
- <span data-ttu-id="95b3c-186">Globálně</span><span class="sxs-lookup"><span data-stu-id="95b3c-186">Globally</span></span>

### <a name="example"></a><span data-ttu-id="95b3c-187">Příklad</span><span class="sxs-lookup"><span data-stu-id="95b3c-187">Example</span></span>
<span data-ttu-id="95b3c-188">Pokud chcete použít filtr na určité akce, přidejte filtr jako atribut na akci:</span><span class="sxs-lookup"><span data-stu-id="95b3c-188">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span> 
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
### <a name="example"></a><span data-ttu-id="95b3c-189">Příklad</span><span class="sxs-lookup"><span data-stu-id="95b3c-189">Example</span></span>
<span data-ttu-id="95b3c-190">Filtr pro všechny akce, které použijete na `controller`, přidejte filtr jako atributu `controller` – třída:</span><span class="sxs-lookup"><span data-stu-id="95b3c-190">To apply the filter to all of the actions on a `controller`, add the filter as an attribute to the `controller` class:</span></span> 

```C#
[NotImplExceptionFilter]
public class ProductsController : ApiController
{
    // ...
}
```

### <a name="example"></a><span data-ttu-id="95b3c-191">Příklad</span><span class="sxs-lookup"><span data-stu-id="95b3c-191">Example</span></span>
<span data-ttu-id="95b3c-192">Pokud chcete použít filtr globálně na všechny řadiče webového rozhraní API, přidá instanci filtr, který má `GlobalConfiguration.Configuration.Filters` kolekce.</span><span class="sxs-lookup"><span data-stu-id="95b3c-192">To apply the filter globally to all Web API controllers, add an instance of the filter to the `GlobalConfiguration.Configuration.Filters` collection.</span></span> <span data-ttu-id="95b3c-193">Filtry výjimek v této kolekci platí pro všechny akce kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="95b3c-193">Exception filters in this collection apply to any Web API controller action.</span></span> 
```C#
GlobalConfiguration.Configuration.Filters.Add(
    new ProductStore.NotImplExceptionFilterAttribute());
```

### <a name="example"></a><span data-ttu-id="95b3c-194">Příklad</span><span class="sxs-lookup"><span data-stu-id="95b3c-194">Example</span></span>
<span data-ttu-id="95b3c-195">Pro ověření modelu stav modelu, který lze předat CreateErrorResponse metoda, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="95b3c-195">For model validation, the model state can be passed to CreateErrorResponse method as shown below:</span></span> 
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

<span data-ttu-id="95b3c-196">Zkontrolujte na odkazy v části odkazy na další podrobnosti o výjimečných zpracování a ověření modelu v rozhraní ASP.Net Web API</span><span class="sxs-lookup"><span data-stu-id="95b3c-196">Check the links in the references section for additional details about exceptional handling and model validation in ASP.Net Web API</span></span> 

## <span data-ttu-id="95b3c-197"><a id="messages"></a>Podrobné informace o zabezpečení v chybových zprávách, neuvádějí</span><span class="sxs-lookup"><span data-stu-id="95b3c-197"><a id="messages"></a>Do not expose security details in error messages</span></span>

| <span data-ttu-id="95b3c-198">Název</span><span class="sxs-lookup"><span data-stu-id="95b3c-198">Title</span></span>                   | <span data-ttu-id="95b3c-199">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="95b3c-199">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="95b3c-200">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="95b3c-200">**Component**</span></span>               | <span data-ttu-id="95b3c-201">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="95b3c-201">Web Application</span></span> | 
| <span data-ttu-id="95b3c-202">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="95b3c-202">**SDL Phase**</span></span>               | <span data-ttu-id="95b3c-203">Sestavení</span><span class="sxs-lookup"><span data-stu-id="95b3c-203">Build</span></span> |  
| <span data-ttu-id="95b3c-204">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="95b3c-204">**Applicable Technologies**</span></span> | <span data-ttu-id="95b3c-205">Obecné</span><span class="sxs-lookup"><span data-stu-id="95b3c-205">Generic</span></span> |
| <span data-ttu-id="95b3c-206">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="95b3c-206">**Attributes**</span></span>              | <span data-ttu-id="95b3c-207">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="95b3c-207">N/A</span></span>  |
| <span data-ttu-id="95b3c-208">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="95b3c-208">**References**</span></span>              | <span data-ttu-id="95b3c-209">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="95b3c-209">N/A</span></span>  |
| <span data-ttu-id="95b3c-210">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="95b3c-210">**Steps**</span></span> | <p><span data-ttu-id="95b3c-211">Obecné chybové zprávy jsou poskytovány přímo na uživatele bez včetně dat citlivé aplikaci.</span><span class="sxs-lookup"><span data-stu-id="95b3c-211">Generic error messages are provided directly to the user without including sensitive application data.</span></span> <span data-ttu-id="95b3c-212">Příklady citlivých dat:</span><span class="sxs-lookup"><span data-stu-id="95b3c-212">Examples of sensitive data include:</span></span></p><ul><li><span data-ttu-id="95b3c-213">Názvy serverů</span><span class="sxs-lookup"><span data-stu-id="95b3c-213">Server names</span></span></li><li><span data-ttu-id="95b3c-214">Připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="95b3c-214">Connection strings</span></span></li><li><span data-ttu-id="95b3c-215">Uživatelská jména</span><span class="sxs-lookup"><span data-stu-id="95b3c-215">Usernames</span></span></li><li><span data-ttu-id="95b3c-216">Hesla</span><span class="sxs-lookup"><span data-stu-id="95b3c-216">Passwords</span></span></li><li><span data-ttu-id="95b3c-217">Postupy SQL</span><span class="sxs-lookup"><span data-stu-id="95b3c-217">SQL procedures</span></span></li><li><span data-ttu-id="95b3c-218">Podrobnosti o selhání dynamické SQL</span><span class="sxs-lookup"><span data-stu-id="95b3c-218">Details of dynamic SQL failures</span></span></li><li><span data-ttu-id="95b3c-219">Trasování zásobníku a řádky kódu</span><span class="sxs-lookup"><span data-stu-id="95b3c-219">Stack trace and lines of code</span></span></li><li><span data-ttu-id="95b3c-220">Proměnné uložené v paměti</span><span class="sxs-lookup"><span data-stu-id="95b3c-220">Variables stored in memory</span></span></li><li><span data-ttu-id="95b3c-221">Umístění jednotky a složky</span><span class="sxs-lookup"><span data-stu-id="95b3c-221">Drive and folder locations</span></span></li><li><span data-ttu-id="95b3c-222">Body instalace aplikací</span><span class="sxs-lookup"><span data-stu-id="95b3c-222">Application install points</span></span></li><li><span data-ttu-id="95b3c-223">Nastavení konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="95b3c-223">Host configuration settings</span></span></li><li><span data-ttu-id="95b3c-224">Další podrobnosti o interní aplikace</span><span class="sxs-lookup"><span data-stu-id="95b3c-224">Other internal application details</span></span></li></ul><p><span data-ttu-id="95b3c-225">Zachycení všech chyb v aplikaci a poskytuje obecné chybové zprávy, jakož i povolení vlastní chyby v rámci služby IIS vám pomůže zabránili odhalení informací.</span><span class="sxs-lookup"><span data-stu-id="95b3c-225">Trapping all errors within an application and providing generic error messages, as well as enabling custom errors within IIS will help prevent information disclosure.</span></span> <span data-ttu-id="95b3c-226">Databáze systému SQL Server a .NET zpracování výjimek, mezi jiné chybě zpracování architektury, jsou obzvláště podrobné a uživatel se zlými úmysly profilace aplikace velmi užitečné.</span><span class="sxs-lookup"><span data-stu-id="95b3c-226">SQL Server database and .NET Exception handling, among other error handling architectures, are especially verbose and extremely useful to a malicious user profiling your application.</span></span> <span data-ttu-id="95b3c-227">Ne přímo zobrazit obsah třídy odvozené od třídy výjimky .NET a zajistěte, abyste měli správné zpracování výjimek, aby neočekávanou výjimku není vyvolána nechtěně přímo na uživatele.</span><span class="sxs-lookup"><span data-stu-id="95b3c-227">Do not directly display the contents of a class derived from the .NET Exception class, and ensure that you have proper exception handling so that an unexpected exception isn't inadvertently raised directly to the user.</span></span></p><ul><li><span data-ttu-id="95b3c-228">Zadejte obecné chybové zprávy přímo na uživatele, který abstraktní tokeny konkrétní podrobnosti najít přímo v výjimky nebo chybové zprávě</span><span class="sxs-lookup"><span data-stu-id="95b3c-228">Provide generic error messages directly to the user that abstract away specific details found directly in the exception/error message</span></span></li><li><span data-ttu-id="95b3c-229">Nezobrazovat obsah třídy výjimky .NET přímo na uživatele</span><span class="sxs-lookup"><span data-stu-id="95b3c-229">Do not display the contents of a .NET exception class directly to the user</span></span></li><li><span data-ttu-id="95b3c-230">Depeše všechny chybové zprávy a podle potřeby informovat uživatele na základě obecnou chybovou zprávu odeslat klientovi aplikace</span><span class="sxs-lookup"><span data-stu-id="95b3c-230">Trap all error messages and if appropriate inform the user via a generic error message sent to the application client</span></span></li><li><span data-ttu-id="95b3c-231">Nevystavujte obsah třídy výjimek, přímo na uživatele, zejména vrácená hodnota z `.ToString()`, nebo hodnoty vlastností zpráv nebo trasování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="95b3c-231">Do not expose the contents of the Exception class directly to the user, especially the return value from `.ToString()`, or the values of the Message or StackTrace properties.</span></span> <span data-ttu-id="95b3c-232">Bezpečně protokolu tyto informace a zobrazit více neškodné zprávu pro uživatele</span><span class="sxs-lookup"><span data-stu-id="95b3c-232">Securely log this information and display a more innocuous message to the user</span></span></li></ul>|

## <span data-ttu-id="95b3c-233"><a id="default"></a>Implementace výchozí chyba zpracování stránky</span><span class="sxs-lookup"><span data-stu-id="95b3c-233"><a id="default"></a>Implement Default error handling page</span></span>

| <span data-ttu-id="95b3c-234">Název</span><span class="sxs-lookup"><span data-stu-id="95b3c-234">Title</span></span>                   | <span data-ttu-id="95b3c-235">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="95b3c-235">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="95b3c-236">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="95b3c-236">**Component**</span></span>               | <span data-ttu-id="95b3c-237">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="95b3c-237">Web Application</span></span> | 
| <span data-ttu-id="95b3c-238">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="95b3c-238">**SDL Phase**</span></span>               | <span data-ttu-id="95b3c-239">Sestavení</span><span class="sxs-lookup"><span data-stu-id="95b3c-239">Build</span></span> |  
| <span data-ttu-id="95b3c-240">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="95b3c-240">**Applicable Technologies**</span></span> | <span data-ttu-id="95b3c-241">Obecné</span><span class="sxs-lookup"><span data-stu-id="95b3c-241">Generic</span></span> |
| <span data-ttu-id="95b3c-242">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="95b3c-242">**Attributes**</span></span>              | <span data-ttu-id="95b3c-243">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="95b3c-243">N/A</span></span>  |
| <span data-ttu-id="95b3c-244">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="95b3c-244">**References**</span></span>              | <span data-ttu-id="95b3c-245">[Upravit dialogové okno nastavení stránky ASP.NET chyby](https://technet.microsoft.com/library/dd569096(WS.10).aspx)</span><span class="sxs-lookup"><span data-stu-id="95b3c-245">[Edit ASP.NET Error Pages Settings Dialog Box](https://technet.microsoft.com/library/dd569096(WS.10).aspx)</span></span> |
| <span data-ttu-id="95b3c-246">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="95b3c-246">**Steps**</span></span> | <p><span data-ttu-id="95b3c-247">Pokud aplikace ASP.NET nezdaří a dojde HTTP/1.x 500 Vnitřní chyba serveru nebo konfigurace funkcí (například filtrování požadavků) zabraňuje zobrazení stránky, se budou generovat chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="95b3c-247">When an ASP.NET application fails and causes an HTTP/1.x 500 Internal Server Error, or a feature configuration (such as Request Filtering) prevents a page from being displayed, an error message will be generated.</span></span> <span data-ttu-id="95b3c-248">Můžou správci rozhodnout, zda aplikace by měla pro klienta, podrobných chybových zpráv klientovi nebo podrobné chybové zprávy na localhost jenom zobrazit přátelskou zprávou.</span><span class="sxs-lookup"><span data-stu-id="95b3c-248">Administrators can choose whether or not the application should display a friendly message to the client, detailed error message to the client, or detailed error message to localhost only.</span></span> <span data-ttu-id="95b3c-249"><customErrors> v souboru web.config má tři režimy:</span><span class="sxs-lookup"><span data-stu-id="95b3c-249">The <customErrors> tag in the web.config has three modes:</span></span></p><ul><li><span data-ttu-id="95b3c-250">**Na:** Určuje, že jsou povoleny vlastní chyby.</span><span class="sxs-lookup"><span data-stu-id="95b3c-250">**On:** Specifies that custom errors are enabled.</span></span> <span data-ttu-id="95b3c-251">Pokud není zadán žádný atribut defaultRedirect, uživatelé uvidí Obecná chyba.</span><span class="sxs-lookup"><span data-stu-id="95b3c-251">If no defaultRedirect attribute is specified, users see a generic error.</span></span> <span data-ttu-id="95b3c-252">Vlastní chyby jsou zobrazeny ke vzdáleným klientům a na místního hostitele</span><span class="sxs-lookup"><span data-stu-id="95b3c-252">The custom errors are shown to the remote clients and to the local host</span></span></li><li><span data-ttu-id="95b3c-253">**Vypnutí:** Určuje, že vlastní chyby jsou vypnuté.</span><span class="sxs-lookup"><span data-stu-id="95b3c-253">**Off:** Specifies that custom errors are disabled.</span></span> <span data-ttu-id="95b3c-254">Podrobné chyby technologie ASP.NET se zobrazí ke vzdáleným klientům a místního hostitele</span><span class="sxs-lookup"><span data-stu-id="95b3c-254">The detailed ASP.NET errors are shown to the remote clients and to the local host</span></span></li><li><span data-ttu-id="95b3c-255">**RemoteOnly:** Určuje, že vlastní chyby jsou zobrazeny pouze ke vzdáleným klientům, a že ASP.NET chyby jsou zobrazeny na místního hostitele.</span><span class="sxs-lookup"><span data-stu-id="95b3c-255">**RemoteOnly:** Specifies that custom errors are shown only to the remote clients, and that ASP.NET errors are shown to the local host.</span></span> <span data-ttu-id="95b3c-256">Toto je výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="95b3c-256">This is the default value</span></span></li></ul><p><span data-ttu-id="95b3c-257">Otevřete `web.config` souboru aplikace nebo webu a zkontrolujte, zda má značky buď `<customErrors mode="RemoteOnly" />` nebo `<customErrors mode="On" />` definované.</span><span class="sxs-lookup"><span data-stu-id="95b3c-257">Open the `web.config` file for the application/site and ensure that the tag has either `<customErrors mode="RemoteOnly" />` or `<customErrors mode="On" />` defined.</span></span></p>|

## <span data-ttu-id="95b3c-258"><a id="deployment"></a>Nastavte metodu nasazení prodejní ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="95b3c-258"><a id="deployment"></a>Set Deployment Method to Retail in IIS</span></span>

| <span data-ttu-id="95b3c-259">Název</span><span class="sxs-lookup"><span data-stu-id="95b3c-259">Title</span></span>                   | <span data-ttu-id="95b3c-260">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="95b3c-260">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="95b3c-261">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="95b3c-261">**Component**</span></span>               | <span data-ttu-id="95b3c-262">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="95b3c-262">Web Application</span></span> | 
| <span data-ttu-id="95b3c-263">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="95b3c-263">**SDL Phase**</span></span>               | <span data-ttu-id="95b3c-264">Nasazení</span><span class="sxs-lookup"><span data-stu-id="95b3c-264">Deployment</span></span> |  
| <span data-ttu-id="95b3c-265">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="95b3c-265">**Applicable Technologies**</span></span> | <span data-ttu-id="95b3c-266">Obecné</span><span class="sxs-lookup"><span data-stu-id="95b3c-266">Generic</span></span> |
| <span data-ttu-id="95b3c-267">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="95b3c-267">**Attributes**</span></span>              | <span data-ttu-id="95b3c-268">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="95b3c-268">N/A</span></span>  |
| <span data-ttu-id="95b3c-269">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="95b3c-269">**References**</span></span>              | <span data-ttu-id="95b3c-270">[nasazení – Element (schéma nastavení ASP.NET)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx)</span><span class="sxs-lookup"><span data-stu-id="95b3c-270">[deployment Element (ASP.NET Settings Schema)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx)</span></span> |
| <span data-ttu-id="95b3c-271">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="95b3c-271">**Steps**</span></span> | <p><span data-ttu-id="95b3c-272">`<deployment retail>` Přepínač je určená pro produkční servery služby IIS.</span><span class="sxs-lookup"><span data-stu-id="95b3c-272">The `<deployment retail>` switch is intended for use by production IIS servers.</span></span> <span data-ttu-id="95b3c-273">Tento přepínač se používá k pomoci se spouští s nejmenšími možnými úniků informace o zabezpečení a nejlepší možný výkon zakázáním schopnost aplikace generovat výstup trasování na stránce, zakázání schopnost zobrazovat podrobné chybové zprávy pro koncové uživatele a zakázání přepínač ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="95b3c-273">This switch is used to help applications run with the best possible performance and least possible security information leakages by disabling the application's ability to generate trace output on a page, disabling the ability to display detailed error messages to end users, and disabling the debug switch.</span></span></p><p><span data-ttu-id="95b3c-274">Často, přepínače a možnosti, které jsou zaměřené na vývojáře, například se nezdařilo žádosti o trasování a ladění, jsou povolené během vývoje aktivní.</span><span class="sxs-lookup"><span data-stu-id="95b3c-274">Often times, switches and options that are developer-focused, such as failed request tracing and debugging, are enabled during active development.</span></span> <span data-ttu-id="95b3c-275">Doporučuje se, že metoda nasazení na žádném serveru pro produkční být nastavená na prodejní.</span><span class="sxs-lookup"><span data-stu-id="95b3c-275">It is recommended that the deployment method on any production server be set to retail.</span></span> <span data-ttu-id="95b3c-276">otevření souboru machine.config a ujistěte se, že `<deployment retail="true" />` zůstane nastavený na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="95b3c-276">open the machine.config file and ensure that `<deployment retail="true" />` remains set to true.</span></span></p>|

## <span data-ttu-id="95b3c-277"><a id="fail"></a>Výjimky má bezpečně selhat.</span><span class="sxs-lookup"><span data-stu-id="95b3c-277"><a id="fail"></a>Exceptions should fail safely</span></span>

| <span data-ttu-id="95b3c-278">Název</span><span class="sxs-lookup"><span data-stu-id="95b3c-278">Title</span></span>                   | <span data-ttu-id="95b3c-279">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="95b3c-279">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="95b3c-280">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="95b3c-280">**Component**</span></span>               | <span data-ttu-id="95b3c-281">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="95b3c-281">Web Application</span></span> | 
| <span data-ttu-id="95b3c-282">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="95b3c-282">**SDL Phase**</span></span>               | <span data-ttu-id="95b3c-283">Sestavení</span><span class="sxs-lookup"><span data-stu-id="95b3c-283">Build</span></span> |  
| <span data-ttu-id="95b3c-284">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="95b3c-284">**Applicable Technologies**</span></span> | <span data-ttu-id="95b3c-285">Obecné</span><span class="sxs-lookup"><span data-stu-id="95b3c-285">Generic</span></span> |
| <span data-ttu-id="95b3c-286">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="95b3c-286">**Attributes**</span></span>              | <span data-ttu-id="95b3c-287">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="95b3c-287">N/A</span></span>  |
| <span data-ttu-id="95b3c-288">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="95b3c-288">**References**</span></span>              | [<span data-ttu-id="95b3c-289">Bezpečně nezdaří</span><span class="sxs-lookup"><span data-stu-id="95b3c-289">Fail securely</span></span>](https://www.owasp.org/index.php/Fail_securely) |
| <span data-ttu-id="95b3c-290">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="95b3c-290">**Steps**</span></span> | <span data-ttu-id="95b3c-291">Aplikace má bezpečně selhat.</span><span class="sxs-lookup"><span data-stu-id="95b3c-291">Application should fail safely.</span></span> <span data-ttu-id="95b3c-292">Libovolné metody, která vrací logickou hodnotu, podle které určité rozhoduje, měli pečlivě vytvořit bloku výjimky.</span><span class="sxs-lookup"><span data-stu-id="95b3c-292">Any method that returns a Boolean value, based on which certain decision is made, should have exception block carefully created.</span></span> <span data-ttu-id="95b3c-293">Existuje mnoho logické chyby kvůli které problémy se zabezpečením nárůstu v při neuváženě zápisu bloku výjimky.</span><span class="sxs-lookup"><span data-stu-id="95b3c-293">There are lot of logical errors due to which security issues creep in, when the exception block is written carelessly.</span></span>|

### <a name="example"></a><span data-ttu-id="95b3c-294">Příklad</span><span class="sxs-lookup"><span data-stu-id="95b3c-294">Example</span></span>
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
                        //// Adding additional check to enable CMS urls if they are not hosted on same domain.
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
<span data-ttu-id="95b3c-295">Výše metoda vždy vrátí hodnotu True, pokud dojde k výjimce.</span><span class="sxs-lookup"><span data-stu-id="95b3c-295">The above method will always return True, if some exception happens.</span></span> <span data-ttu-id="95b3c-296">Pokud koncový uživatel poskytuje poškozená adresa URL, která respektuje prohlížeče, ale `Uri()` konstruktor není to vyvolá výjimku a napadené budete přesměrováni na adresu URL platná, ale poškozený.</span><span class="sxs-lookup"><span data-stu-id="95b3c-296">If the end user provides a malformed URL, that the browser respects, but the `Uri()` constructor doesn't, this will throw an exception, and the victim will be taken to the valid but malformed URL.</span></span> 