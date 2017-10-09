---
title: "aaaHow tooadd operations tooan rozhraní API v Azure API Management | Microsoft Docs"
description: "Zjistěte, jak tooadd operations tooan rozhraní API v Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 1158a023-1913-4e9c-93de-9164b672f9b3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: d57fa59a2b0ceb392cde23150a0cbb326e52d27d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-operations-tooan-api-in-azure-api-management"></a><span data-ttu-id="871fa-103">Jak tooadd operations tooan rozhraní API v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="871fa-103">How tooadd operations tooan API in Azure API Management</span></span>
<span data-ttu-id="871fa-104">Před použitím rozhraní API ve službě API Management, je nutné přidat operace.</span><span class="sxs-lookup"><span data-stu-id="871fa-104">Before an API in API Management can be used, operations must be added.</span></span> <span data-ttu-id="871fa-105">Tato příručka ukazuje jak tooadd a nakonfigurovat různé typy tooan operace rozhraní API ve službě API Management.</span><span class="sxs-lookup"><span data-stu-id="871fa-105">This guide shows how tooadd and configure different types of operations tooan API in API Management.</span></span>

## <span data-ttu-id="871fa-106"><a name="add-operation"></a>Přidání operace</span><span class="sxs-lookup"><span data-stu-id="871fa-106"><a name="add-operation"> </a>Add an operation</span></span>
<span data-ttu-id="871fa-107">Operace se přidají a nakonfigurovat tooan rozhraní API portálu vydavatele hello.</span><span class="sxs-lookup"><span data-stu-id="871fa-107">Operations are added and configured tooan API in hello publisher portal.</span></span> <span data-ttu-id="871fa-108">Klikněte na portálu vydavatele hello tooaccess **portál vydavatele** v hello portál Azure pro služby API Management.</span><span class="sxs-lookup"><span data-stu-id="871fa-108">tooaccess hello publisher portal, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span>

![Portál vydavatele][api-management-management-console]

> <span data-ttu-id="871fa-110">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.</span><span class="sxs-lookup"><span data-stu-id="871fa-110">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="871fa-111">Vyberte hello požadovaného rozhraní API v portálu vydavatele hello a pak vyberte hello **Operations** kartě.</span><span class="sxs-lookup"><span data-stu-id="871fa-111">Select hello desired API in hello publisher portal and then select hello **Operations** tab.</span></span> 

![Operace][api-management-operations]

<span data-ttu-id="871fa-113">Klikněte na tlačítko **přidat operace** tooadd novou operaci.</span><span class="sxs-lookup"><span data-stu-id="871fa-113">Click **Add Operation** tooadd a new operation.</span></span> <span data-ttu-id="871fa-114">Hello **operaci nového** se zobrazí a hello **podpis** ve výchozím nastavení bude vybraná karta.</span><span class="sxs-lookup"><span data-stu-id="871fa-114">hello **New operation** will be displayed and hello **Signature** tab will be selected by default.</span></span>

![Operace přidání][api-management-add-operation]

<span data-ttu-id="871fa-116">Zadejte hello **příkaz HTTP** tak, že zvolíte hello rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="871fa-116">Specify hello **HTTP verb** by choosing from hello drop-down list.</span></span>

![Metoda HTTP][api-management-http-method]

<a name="url-template"></a>

<span data-ttu-id="871fa-118">Definování šablon URL hello zadáním fragment adresy URL, který se skládá z jedné nebo více segmenty cesty adresy URL a nula nebo více parametrů řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="871fa-118">Define hello URL template by typing in a URL fragment consisting of one or more URL path segments and zero or more query string parameters.</span></span> <span data-ttu-id="871fa-119">Hello URL šablony, připojením toohello základní adresu URL hello rozhraní API, identifikuje jednu operaci HTTP.</span><span class="sxs-lookup"><span data-stu-id="871fa-119">hello URL template, appended toohello base URL of hello API, identifies a single HTTP operation.</span></span> <span data-ttu-id="871fa-120">Může obsahovat jeden nebo více s názvem proměnné částí, které jsou určeny složené závorky.</span><span class="sxs-lookup"><span data-stu-id="871fa-120">It may contain one or more named variable parts that are identified by curly braces.</span></span> <span data-ttu-id="871fa-121">Tyto proměnné části se nazývají parametry šablony a dynamicky přiřazené hodnoty extrahovat z adresy URL hello požadavek při zpracování žádosti hello pomocí platformy API Management hello.</span><span class="sxs-lookup"><span data-stu-id="871fa-121">These variable parts are called template parameters and are dynamically assigned values extracted from hello request's URL when hello request is being processed by hello API Management platform.</span></span>

> <span data-ttu-id="871fa-122">Šablona Hello adresa URL může obsahovat zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="871fa-122">hello URL template can include wildcard patterns.</span></span> <span data-ttu-id="871fa-123">Například zadání `/*` bude předávat všechny žádosti týkající se této HTTP Metoda toohello zpět ukončení služby.</span><span class="sxs-lookup"><span data-stu-id="871fa-123">For example, specifying `/*` will forward all requests for that HTTP method toohello back end service.</span></span>

![Adresa URL šablony][api-management-url-template]

<a name="rewrite-url-template"></a>

<span data-ttu-id="871fa-125">V případě potřeby zadejte hello **přepisu adresy URL šablony**.</span><span class="sxs-lookup"><span data-stu-id="871fa-125">If desired, specify hello **Rewrite URL template**.</span></span> <span data-ttu-id="871fa-126">To vám umožní toouse hello standardní šablona adresy URL pro zpracování příchozích požadavků na hello front-endu, při volání hello back-end prostřednictvím adresy URL převedený podle toohello přepisování šablony.</span><span class="sxs-lookup"><span data-stu-id="871fa-126">This allows you toouse hello standard URL template for processing incoming requests on hello front-end, while calling hello back-end via a converted URL according toohello rewrite template.</span></span> <span data-ttu-id="871fa-127">Parametry šablony ze šablony hello adresa URL by měla použít v šabloně přepisování hello.</span><span class="sxs-lookup"><span data-stu-id="871fa-127">Template parameters from hello URL template should be used in hello rewrite template.</span></span> <span data-ttu-id="871fa-128">Hello následující příklad ukazuje, jak obsahu typu kódovaný jako segment cesty v hello webové služby z předchozího příkladu hello lze zadat jako parametr dotazu v hello publikován rozhraní API prostřednictvím hello platformy API Management pomocí šablon hello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="871fa-128">hello following example shows how content type encoded as path segment in hello web service from hello previous example can be provided as a query parameter in hello API published via hello API Management platform using hello URL templates.</span></span>

![Přepisování adres URL šablony][api-management-url-template-rewrite]

<span data-ttu-id="871fa-130">Volající toohello operace budou používat formát hello `/customers?customerid=ALFKI` a to bude příliš namapovat`/Customers('ALFKI')` při volání hello back endové služby.</span><span class="sxs-lookup"><span data-stu-id="871fa-130">Callers toohello operation will use hello format `/customers?customerid=ALFKI` and this will be mapped too`/Customers('ALFKI')` when hello back-end service is invoked.</span></span>

<span data-ttu-id="871fa-131">**Zobrazení** název a **popis** zadejte popis hello operace a jsou použité tooprovide dokumentace toohello vývojáře, kteří používají toto rozhraní API v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="871fa-131">**Display** name and **Description** provide a description of hello operation and are used tooprovide documentation toohello developers using this API in hello developer portal.</span></span>

![Popis][api-management-description]

<span data-ttu-id="871fa-133">Popis operace Hello lze zadat jako prostý text nebo HTML v hello **popis** textové pole.</span><span class="sxs-lookup"><span data-stu-id="871fa-133">hello operation description can be specified as plain text or HTML in hello **Description** text box.</span></span>

## <span data-ttu-id="871fa-134"><a name="operation-caching"></a>Operace ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="871fa-134"><a name="operation-caching"> </a>Operation caching</span></span>
<span data-ttu-id="871fa-135">Ukládání odpovědí do mezipaměti snižuje latence zaznamenatelného spotřebiteli hello rozhraní API, snižuje využití šířky pásma a snížení zatížení hello hello HTTP webové služby implementace hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="871fa-135">Response caching reduces latency perceived by hello API consumers, lowers bandwidth consumption and decreases hello load on hello HTTP web service implementing hello API.</span></span> 

<span data-ttu-id="871fa-136">tooeasily a rychle povolit ukládání do mezipaměti pro hello operaci, vyberte hello **ukládání do mezipaměti** kartě a zkontrolujte hello **povolit** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="871fa-136">tooeasily and quickly enable caching for hello operation, select hello **Caching** tab and check hello **Enable** checkbox.</span></span>

![Ukládání do mezipaměti][api-management-caching-tab]

<span data-ttu-id="871fa-138">**Doba trvání** Určuje časové období, během které hello odpověď operace zůstává v mezipaměti hello hello.</span><span class="sxs-lookup"><span data-stu-id="871fa-138">**Duration** specifies hello time period during which hello operation response remains in hello cache.</span></span> <span data-ttu-id="871fa-139">Hello výchozí hodnota je 3 600 sekund nebo 1 hodina.</span><span class="sxs-lookup"><span data-stu-id="871fa-139">hello default value is 3600 seconds or 1 hour.</span></span>

<span data-ttu-id="871fa-140">Klíče mezipaměti jsou použité toodifferentiate mezi odpovědí tak, aby odpovědi hello odpovídající klíč tooeach různé mezipaměti získáte vlastní samostatné hodnotu uloženou v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="871fa-140">Cache keys are used toodifferentiate between responses so that hello response corresponding tooeach different cache key will get its own separate cached value.</span></span> <span data-ttu-id="871fa-141">Volitelně můžete zadat parametrů řetězce dotazu konkrétní nebo toobe hlavičky protokolu HTTP používá v oblasti výpočetních hodnoty klíče mezipaměti v hello **podle parametrů řetězce dotazu** a **podle hlaviček** textová pole v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="871fa-141">Optionally, enter specific query string parameters and/or HTTP headers toobe used in computing cache key values in hello **Vary by query string parameters** and **Vary by headers** text boxes respectively.</span></span> <span data-ttu-id="871fa-142">Pokud žádný není zadaný, úplná požadavku na adresu URL a hello následující hodnoty hlavičky protokolu HTTP se používají v generování klíče mezipaměti: **přijmout** a **Accept-Charset**.</span><span class="sxs-lookup"><span data-stu-id="871fa-142">When none are specified, full request URL and hello following HTTP header values are used in cache key generation: **Accept** and **Accept-Charset**.</span></span>

> <span data-ttu-id="871fa-143">Další informace o ukládání do mezipaměti a zásady ukládání do mezipaměti najdete v tématu [jak Azure API Management výsledkem operace toocache][How toocache operation results in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="871fa-143">For more information on caching and caching policies, see [How toocache operation results in Azure API Management][How toocache operation results in Azure API Management].</span></span>
> 
> 

## <span data-ttu-id="871fa-144"><a name="request-parameters"></a>Parametry žádosti</span><span class="sxs-lookup"><span data-stu-id="871fa-144"><a name="request-parameters"> </a>Request parameters</span></span>
<span data-ttu-id="871fa-145">Parametry operace se spravují na kartě Parametry hello. Parametry zadané v hello **adresa URL šablony** na hello **podpis** se přidávají automaticky a lze změnit pouze úpravou šablony hello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="871fa-145">Operation parameters are managed on hello Parameters tab. Parameters specified in hello **URL Template** on hello **Signature** tab are added automatically and can be changed only by editing hello URL template.</span></span> <span data-ttu-id="871fa-146">Další parametry lze zadat ručně.</span><span class="sxs-lookup"><span data-stu-id="871fa-146">Additional parameters can be entered manually.</span></span>

<span data-ttu-id="871fa-147">Klikněte na tlačítko tooadd nový parametr dotazu, **přidat parametr dotazu** a zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="871fa-147">tooadd a new query parameter, click **Add Query Parameter** and enter hello following information:</span></span>

* <span data-ttu-id="871fa-148">**Název** -název parametru.</span><span class="sxs-lookup"><span data-stu-id="871fa-148">**Name** - parameter name.</span></span>
* <span data-ttu-id="871fa-149">**Popis** – stručný popis parametru hello (volitelné).</span><span class="sxs-lookup"><span data-stu-id="871fa-149">**Description** - a brief description of hello parameter (optional).</span></span>
* <span data-ttu-id="871fa-150">**Typ** – typ parametru, vybraný v hello rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="871fa-150">**Type** - parameter type, selected in hello drop down.</span></span>
* <span data-ttu-id="871fa-151">**Hodnoty** -hodnoty, které lze přiřadit parametr toothis.</span><span class="sxs-lookup"><span data-stu-id="871fa-151">**Values** - values that can be assigned toothis parameter.</span></span> <span data-ttu-id="871fa-152">Jedna z hodnot hello může být označen jako výchozí (volitelné).</span><span class="sxs-lookup"><span data-stu-id="871fa-152">One of hello values can be marked as default (optional).</span></span>
* <span data-ttu-id="871fa-153">**Požadované** -nastavit povinný parametr hello zaškrtnutím políčka hello.</span><span class="sxs-lookup"><span data-stu-id="871fa-153">**Required** - make hello parameter mandatory by checking hello checkbox.</span></span> 

![Parametry žádosti][api-management-request-parameters]

## <span data-ttu-id="871fa-155"><a name="request-body"></a>Text žádosti</span><span class="sxs-lookup"><span data-stu-id="871fa-155"><a name="request-body"> </a>Request body</span></span>
<span data-ttu-id="871fa-156">Pokud operace hello umožňuje (např. PUT, POST) a vyžaduje text může uveďte příklad ji ve všech hello podporované formáty reprezentace (například json, XML).</span><span class="sxs-lookup"><span data-stu-id="871fa-156">If hello operation allows (e.g. PUT, POST) and requires a body you may provide an example of it in all of hello supported representation formats (e.g. json, XML).</span></span> 

> <span data-ttu-id="871fa-157">Hello textu požadavku se používá pro dokumentaci pouze pro účely a není ověřen.</span><span class="sxs-lookup"><span data-stu-id="871fa-157">hello request body is used for documentation purposes only and is not validated.</span></span>
> 
> 

<span data-ttu-id="871fa-158">tooenter obsah žádosti, přepínač toohello **textu** kartě.</span><span class="sxs-lookup"><span data-stu-id="871fa-158">tooenter a request body, switch toohello **Body** tab.</span></span>

<span data-ttu-id="871fa-159">Klikněte na tlačítko **přidat reprezentace**, začněte psát název požadovaného typu obsahu (například application/json), vyberte ho v hello rozevíracího seznamu a vložení hello potřeby příklad text žádosti ve vybraném formátu hello do textového pole pro hello.</span><span class="sxs-lookup"><span data-stu-id="871fa-159">Click **Add Representation**, start typing desired content type name (e.g. application/json), select it in hello drop-down, and paste hello desired request body example in hello selected format into hello text box.</span></span> 

![Text žádosti][api-management-request-body]

<span data-ttu-id="871fa-161">V další toorepresentations, můžete také zadat volitelné textový popis v hello **popis** textové pole.</span><span class="sxs-lookup"><span data-stu-id="871fa-161">In additional toorepresentations, you can also specify an optional text description in hello **Description** text box.</span></span>

## <span data-ttu-id="871fa-162"><a name="responses"></a>Odpovědí</span><span class="sxs-lookup"><span data-stu-id="871fa-162"><a name="responses"> </a>Responses</span></span>
<span data-ttu-id="871fa-163">Je dobrým zvykem tooprovide, příkladem odpovědi na všechny stavové kódy, které může vytvořit hello operaci.</span><span class="sxs-lookup"><span data-stu-id="871fa-163">It is a good practice tooprovide examples of responses for all status codes that hello operation may produce.</span></span> <span data-ttu-id="871fa-164">Každý kód stavu může mít více než jeden příklad text odpovědi, jeden pro každou hello podporované typy obsahu.</span><span class="sxs-lookup"><span data-stu-id="871fa-164">Each status code may have more than one response body example, one for each of hello supported content types.</span></span> 

<span data-ttu-id="871fa-165">Klikněte na tlačítko tooadd odpověď, **přidat** a začněte psát hello potřeby stavový kód.</span><span class="sxs-lookup"><span data-stu-id="871fa-165">tooadd a response, click **Add** and start typing hello desired status code.</span></span> <span data-ttu-id="871fa-166">V tomto příkladu hello stavu kód je **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="871fa-166">In this example hello status code is **200 OK**.</span></span> <span data-ttu-id="871fa-167">Jakmile hello kódu se zobrazí v hello rozevíracího seznamu, vyberte ho a kód odpovědi hello je vytvořený a přidané tooyour operace.</span><span class="sxs-lookup"><span data-stu-id="871fa-167">Once hello code is displayed in hello drop-down, select it, and hello response code is created and added tooyour operation.</span></span>

![Kód odpovědi][api-management-response-code]

<span data-ttu-id="871fa-169">Klikněte na tlačítko **přidat reprezentace**, začněte psát název hello požadovaný typ obsahu (například application/json) a pak vyberte ho v hello rozevírací nabídku.</span><span class="sxs-lookup"><span data-stu-id="871fa-169">Click **Add Representation**, start typing hello desired content type name (e.g. application/json) and then select it in hello drop down.</span></span>

![Typ obsahu textu][api-management-response-body-content-type]

<span data-ttu-id="871fa-171">Vložte příklad textu odpovědi hello ve vybraném formátu hello do textového pole pro hello.</span><span class="sxs-lookup"><span data-stu-id="871fa-171">Paste hello response body example in hello selected format into hello text box.</span></span> 

![Text odpovědi][api-management-response-body]

<span data-ttu-id="871fa-173">V případě potřeby přidejte volitelný popis do hello **popis** textové pole.</span><span class="sxs-lookup"><span data-stu-id="871fa-173">If desired, add an optional description into hello **Description** text box.</span></span>

<span data-ttu-id="871fa-174">Po nakonfigurování hello operace klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="871fa-174">Once hello operation is configured, click **Save**.</span></span>

## <span data-ttu-id="871fa-175"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="871fa-175"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="871fa-176">Po hello operace jsou přidány tooan rozhraní API, hello dalším krokem je tooassociate hello rozhraní API s produktem a publikujete ji tak, aby vývojáři můžou volat jeho operace.</span><span class="sxs-lookup"><span data-stu-id="871fa-176">Once hello operations are added tooan API, hello next step is tooassociate hello API with a product and publish it so that developers can call its operations.</span></span>

* <span data-ttu-id="871fa-177">[Jak toocreate a publikování produktu][How toocreate and publish a product]</span><span class="sxs-lookup"><span data-stu-id="871fa-177">[How toocreate and publish a product][How toocreate and publish a product]</span></span>

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocache operation results in Azure API Management]: api-management-howto-cache.md
