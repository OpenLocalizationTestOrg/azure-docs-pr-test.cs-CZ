---
title: "Přidání operací do rozhraní API v Azure API Management | Microsoft Docs"
description: "Postup přidání operací do rozhraní API v Azure API Management."
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
ms.openlocfilehash: 105fc51c2d1152a40a5757985da47330e0b7b8cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a><span data-ttu-id="dacdd-103">Přidání operací do rozhraní API v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="dacdd-103">How to add operations to an API in Azure API Management</span></span>
<span data-ttu-id="dacdd-104">Před použitím rozhraní API ve službě API Management, je nutné přidat operace.</span><span class="sxs-lookup"><span data-stu-id="dacdd-104">Before an API in API Management can be used, operations must be added.</span></span> <span data-ttu-id="dacdd-105">Tato příručka ukazuje, jak přidávat a konfigurovat různé typy operací do rozhraní API ve službě API Management.</span><span class="sxs-lookup"><span data-stu-id="dacdd-105">This guide shows how to add and configure different types of operations to an API in API Management.</span></span>

## <span data-ttu-id="dacdd-106"><a name="add-operation"></a>Přidání operace</span><span class="sxs-lookup"><span data-stu-id="dacdd-106"><a name="add-operation"> </a>Add an operation</span></span>
<span data-ttu-id="dacdd-107">Operace jsou přidány a nakonfigurovat tak, aby rozhraní API na portálu vydavatele.</span><span class="sxs-lookup"><span data-stu-id="dacdd-107">Operations are added and configured to an API in the publisher portal.</span></span> <span data-ttu-id="dacdd-108">Chcete-li získat přístup k portálu vydavatele, klikněte na tlačítko **portál vydavatele** služby API Management na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="dacdd-108">To access the publisher portal, click **Publisher portal** in the Azure Portal for your API Management service.</span></span>

![Portál vydavatele][api-management-management-console]

> <span data-ttu-id="dacdd-110">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="dacdd-110">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="dacdd-111">Vyberte požadované rozhraní API na portálu vydavatele a pak vyberte **Operations** kartě.</span><span class="sxs-lookup"><span data-stu-id="dacdd-111">Select the desired API in the publisher portal and then select the **Operations** tab.</span></span> 

![Operace][api-management-operations]

<span data-ttu-id="dacdd-113">Klikněte na tlačítko **přidat operace** přidat novou operaci.</span><span class="sxs-lookup"><span data-stu-id="dacdd-113">Click **Add Operation** to add a new operation.</span></span> <span data-ttu-id="dacdd-114">**Operaci nového** se zobrazí a **podpis** ve výchozím nastavení bude vybraná karta.</span><span class="sxs-lookup"><span data-stu-id="dacdd-114">The **New operation** will be displayed and the **Signature** tab will be selected by default.</span></span>

![Operace přidání][api-management-add-operation]

<span data-ttu-id="dacdd-116">Zadejte **příkaz HTTP** výběrem z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="dacdd-116">Specify the **HTTP verb** by choosing from the drop-down list.</span></span>

![Metoda HTTP][api-management-http-method]

<a name="url-template"></a>

<span data-ttu-id="dacdd-118">Adresa URL šablony definujte zadáním fragment adresy URL, který se skládá z jedné nebo více segmenty cesty adresy URL a nula nebo více parametrů řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="dacdd-118">Define the URL template by typing in a URL fragment consisting of one or more URL path segments and zero or more query string parameters.</span></span> <span data-ttu-id="dacdd-119">Adresa URL šablony, připojí k základní adresu URL rozhraní API, identifikuje jednu operaci HTTP.</span><span class="sxs-lookup"><span data-stu-id="dacdd-119">The URL template, appended to the base URL of the API, identifies a single HTTP operation.</span></span> <span data-ttu-id="dacdd-120">Může obsahovat jeden nebo více s názvem proměnné částí, které jsou určeny složené závorky.</span><span class="sxs-lookup"><span data-stu-id="dacdd-120">It may contain one or more named variable parts that are identified by curly braces.</span></span> <span data-ttu-id="dacdd-121">Tyto proměnné části se nazývají parametry šablony a dynamicky přiřazené hodnoty extrahovat z adresy URL žádosti při zpracování žádosti podle platformy API Management.</span><span class="sxs-lookup"><span data-stu-id="dacdd-121">These variable parts are called template parameters and are dynamically assigned values extracted from the request's URL when the request is being processed by the API Management platform.</span></span>

> <span data-ttu-id="dacdd-122">Šablona adresa URL může obsahovat zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="dacdd-122">The URL template can include wildcard patterns.</span></span> <span data-ttu-id="dacdd-123">Například zadání `/*` bude předávat všechny požadavky pro danou metodu HTTP na stránce do pozadí ukončení služby.</span><span class="sxs-lookup"><span data-stu-id="dacdd-123">For example, specifying `/*` will forward all requests for that HTTP method to the back end service.</span></span>

![Adresa URL šablony][api-management-url-template]

<a name="rewrite-url-template"></a>

<span data-ttu-id="dacdd-125">V případě potřeby zadejte **přepisu adresy URL šablony**.</span><span class="sxs-lookup"><span data-stu-id="dacdd-125">If desired, specify the **Rewrite URL template**.</span></span> <span data-ttu-id="dacdd-126">To umožňuje používat standardní šablona adresy URL pro zpracování příchozích požadavků na front-endu, při volání back-end prostřednictvím adresy URL převedený podle šablony přepisování.</span><span class="sxs-lookup"><span data-stu-id="dacdd-126">This allows you to use the standard URL template for processing incoming requests on the front-end, while calling the back-end via a converted URL according to the rewrite template.</span></span> <span data-ttu-id="dacdd-127">Parametry šablony ze šablony, adresy URL by měla být použité v šabloně přepisování.</span><span class="sxs-lookup"><span data-stu-id="dacdd-127">Template parameters from the URL template should be used in the rewrite template.</span></span> <span data-ttu-id="dacdd-128">Následující příklad ukazuje, jak obsahu typu kódovaný jako segment cesty v webovou službu z předchozího příkladu lze zadat jako parametr dotazu v rozhraní API publikovat prostřednictvím platformy pro správu rozhraní API pomocí šablon adresy URL.</span><span class="sxs-lookup"><span data-stu-id="dacdd-128">The following example shows how content type encoded as path segment in the web service from the previous example can be provided as a query parameter in the API published via the API Management platform using the URL templates.</span></span>

![Přepisování adres URL šablony][api-management-url-template-rewrite]

<span data-ttu-id="dacdd-130">Operace bude volající formát `/customers?customerid=ALFKI` a to budou mapována na `/Customers('ALFKI')` při volání služby back-end.</span><span class="sxs-lookup"><span data-stu-id="dacdd-130">Callers to the operation will use the format `/customers?customerid=ALFKI` and this will be mapped to `/Customers('ALFKI')` when the back-end service is invoked.</span></span>

<span data-ttu-id="dacdd-131">**Zobrazení** název a **popis** zadejte popis, operace a používají se pro dokumentaci pro vývojáře, kteří používají toto rozhraní API v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="dacdd-131">**Display** name and **Description** provide a description of the operation and are used to provide documentation to the developers using this API in the developer portal.</span></span>

![Popis][api-management-description]

<span data-ttu-id="dacdd-133">Popis operace lze zadat jako prostý text nebo HTML v **popis** textové pole.</span><span class="sxs-lookup"><span data-stu-id="dacdd-133">The operation description can be specified as plain text or HTML in the **Description** text box.</span></span>

## <span data-ttu-id="dacdd-134"><a name="operation-caching"></a>Operace ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="dacdd-134"><a name="operation-caching"> </a>Operation caching</span></span>
<span data-ttu-id="dacdd-135">Ukládání odpovědí do mezipaměti snižuje latence zaznamenatelného k příjemce rozhraní API, snižuje využití šířky pásma a snížení zatížení HTTP webové služby implementaci rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="dacdd-135">Response caching reduces latency perceived by the API consumers, lowers bandwidth consumption and decreases the load on the HTTP web service implementing the API.</span></span> 

<span data-ttu-id="dacdd-136">Pokud chcete rychle a snadno povolit ukládání do mezipaměti pro operaci, vyberte **ukládání do mezipaměti** kartě a zkontrolujte **povolit** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="dacdd-136">To easily and quickly enable caching for the operation, select the **Caching** tab and check the **Enable** checkbox.</span></span>

![Ukládání do mezipaměti][api-management-caching-tab]

<span data-ttu-id="dacdd-138">**Doba trvání** Určuje časové období, během které odpověď operace zůstává v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dacdd-138">**Duration** specifies the time period during which the operation response remains in the cache.</span></span> <span data-ttu-id="dacdd-139">Výchozí hodnota je 3 600 sekund nebo 1 hodina.</span><span class="sxs-lookup"><span data-stu-id="dacdd-139">The default value is 3600 seconds or 1 hour.</span></span>

<span data-ttu-id="dacdd-140">Klíče mezipaměti se používají k rozlišení mezi odpovědí tak, aby odpovědi odpovídající každý klíč různé mezipaměti získáte vlastní samostatné hodnotu uloženou v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dacdd-140">Cache keys are used to differentiate between responses so that the response corresponding to each different cache key will get its own separate cached value.</span></span> <span data-ttu-id="dacdd-141">Volitelně můžete zadat parametrů řetězce dotazu konkrétní nebo hlavičky protokolu HTTP, který se má použít v oblasti výpočetních hodnoty klíče mezipaměti v **podle parametrů řetězce dotazu** a **podle hlaviček** textová pole v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="dacdd-141">Optionally, enter specific query string parameters and/or HTTP headers to be used in computing cache key values in the **Vary by query string parameters** and **Vary by headers** text boxes respectively.</span></span> <span data-ttu-id="dacdd-142">Pokud žádný není zadaný, úplná požadavku na adresu URL a následující hodnoty hlavičky protokolu HTTP se používají v generování klíče mezipaměti: **přijmout** a **Accept-Charset**.</span><span class="sxs-lookup"><span data-stu-id="dacdd-142">When none are specified, full request URL and the following HTTP header values are used in cache key generation: **Accept** and **Accept-Charset**.</span></span>

> <span data-ttu-id="dacdd-143">Další informace o ukládání do mezipaměti a zásady ukládání do mezipaměti najdete v tématu [postupy pro ukládání do mezipaměti operace výsledků v Azure API Management][How to cache operation results in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="dacdd-143">For more information on caching and caching policies, see [How to cache operation results in Azure API Management][How to cache operation results in Azure API Management].</span></span>
> 
> 

## <span data-ttu-id="dacdd-144"><a name="request-parameters"></a>Parametry žádosti</span><span class="sxs-lookup"><span data-stu-id="dacdd-144"><a name="request-parameters"> </a>Request parameters</span></span>
<span data-ttu-id="dacdd-145">Parametry operace se spravují na kartě Parametry.</span><span class="sxs-lookup"><span data-stu-id="dacdd-145">Operation parameters are managed on the Parameters tab.</span></span> <span data-ttu-id="dacdd-146">Parametry zadané v **adresa URL šablony** na **podpis** se přidávají automaticky a lze změnit pouze úpravou šablony adresy URL.</span><span class="sxs-lookup"><span data-stu-id="dacdd-146">Parameters specified in the **URL Template** on the **Signature** tab are added automatically and can be changed only by editing the URL template.</span></span> <span data-ttu-id="dacdd-147">Další parametry lze zadat ručně.</span><span class="sxs-lookup"><span data-stu-id="dacdd-147">Additional parameters can be entered manually.</span></span>

<span data-ttu-id="dacdd-148">Chcete-li přidat nový parametr dotazu, klikněte na tlačítko **přidat parametr dotazu** a zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="dacdd-148">To add a new query parameter, click **Add Query Parameter** and enter the following information:</span></span>

* <span data-ttu-id="dacdd-149">**Název** -název parametru.</span><span class="sxs-lookup"><span data-stu-id="dacdd-149">**Name** - parameter name.</span></span>
* <span data-ttu-id="dacdd-150">**Popis** – stručný popis parametru (volitelné).</span><span class="sxs-lookup"><span data-stu-id="dacdd-150">**Description** - a brief description of the parameter (optional).</span></span>
* <span data-ttu-id="dacdd-151">**Typ** – typ parametru, v rozevírací nabídce vybrali dolů.</span><span class="sxs-lookup"><span data-stu-id="dacdd-151">**Type** - parameter type, selected in the drop down.</span></span>
* <span data-ttu-id="dacdd-152">**Hodnoty** -hodnoty, které lze přiřadit k tento parametr.</span><span class="sxs-lookup"><span data-stu-id="dacdd-152">**Values** - values that can be assigned to this parameter.</span></span> <span data-ttu-id="dacdd-153">Jedna z hodnot může být označen jako výchozí (volitelné).</span><span class="sxs-lookup"><span data-stu-id="dacdd-153">One of the values can be marked as default (optional).</span></span>
* <span data-ttu-id="dacdd-154">**Požadované** -nastavit parametr povinný zaškrtnutím políčka.</span><span class="sxs-lookup"><span data-stu-id="dacdd-154">**Required** - make the parameter mandatory by checking the checkbox.</span></span> 

![Parametry žádosti][api-management-request-parameters]

## <span data-ttu-id="dacdd-156"><a name="request-body"></a>Text žádosti</span><span class="sxs-lookup"><span data-stu-id="dacdd-156"><a name="request-body"> </a>Request body</span></span>
<span data-ttu-id="dacdd-157">Pokud umožňuje operaci (např. PUT, POST) a vyžaduje text může uveďte příklad ji ve všech podporovaných reprezentace formátů (například json, XML).</span><span class="sxs-lookup"><span data-stu-id="dacdd-157">If the operation allows (e.g. PUT, POST) and requires a body you may provide an example of it in all of the supported representation formats (e.g. json, XML).</span></span> 

> <span data-ttu-id="dacdd-158">Textu požadavku se používá pro dokumentaci pouze pro účely a není ověřen.</span><span class="sxs-lookup"><span data-stu-id="dacdd-158">The request body is used for documentation purposes only and is not validated.</span></span>
> 
> 

<span data-ttu-id="dacdd-159">Chcete-li zadat obsah žádosti, přepněte na **textu** kartě.</span><span class="sxs-lookup"><span data-stu-id="dacdd-159">To enter a request body, switch to the **Body** tab.</span></span>

<span data-ttu-id="dacdd-160">Klikněte na tlačítko **přidat reprezentace**, začněte psát název požadovaného typu obsahu (například application/json), vyberte v rozevíracím seznamu a vložení v příkladu textu požadované žádosti ve vybraném formátu do textového pole.</span><span class="sxs-lookup"><span data-stu-id="dacdd-160">Click **Add Representation**, start typing desired content type name (e.g. application/json), select it in the drop-down, and paste the desired request body example in the selected format into the text box.</span></span> 

![Text žádosti][api-management-request-body]

<span data-ttu-id="dacdd-162">V další reprezentace, můžete taky určit volitelný popis v **popis** textové pole.</span><span class="sxs-lookup"><span data-stu-id="dacdd-162">In additional to representations, you can also specify an optional text description in the **Description** text box.</span></span>

## <span data-ttu-id="dacdd-163"><a name="responses"></a>Odpovědí</span><span class="sxs-lookup"><span data-stu-id="dacdd-163"><a name="responses"> </a>Responses</span></span>
<span data-ttu-id="dacdd-164">Je dobrým zvykem příklady odpovědi pro všechny stavové kódy, které může vytvořit operaci.</span><span class="sxs-lookup"><span data-stu-id="dacdd-164">It is a good practice to provide examples of responses for all status codes that the operation may produce.</span></span> <span data-ttu-id="dacdd-165">Každý kód stavu může mít více než jeden příklad text odpovědi, jeden pro každou podporované typy obsahu.</span><span class="sxs-lookup"><span data-stu-id="dacdd-165">Each status code may have more than one response body example, one for each of the supported content types.</span></span> 

<span data-ttu-id="dacdd-166">Chcete-li přidat odpovědi, klikněte na tlačítko **přidat** a začněte psát kód požadovaného stavu.</span><span class="sxs-lookup"><span data-stu-id="dacdd-166">To add a response, click **Add** and start typing the desired status code.</span></span> <span data-ttu-id="dacdd-167">V tomto příkladu je stavový kód **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="dacdd-167">In this example the status code is **200 OK**.</span></span> <span data-ttu-id="dacdd-168">Jakmile se v rozevíracím seznamu se zobrazí kód, vyberte ho a kód odpovědi je vytvořen a přidán do vaší operaci.</span><span class="sxs-lookup"><span data-stu-id="dacdd-168">Once the code is displayed in the drop-down, select it, and the response code is created and added to your operation.</span></span>

![Kód odpovědi][api-management-response-code]

<span data-ttu-id="dacdd-170">Klikněte na tlačítko **přidat reprezentace**, začněte psát název požadovaný typ obsahu (například application/json) a vyberte ho v seznamu dolů.</span><span class="sxs-lookup"><span data-stu-id="dacdd-170">Click **Add Representation**, start typing the desired content type name (e.g. application/json) and then select it in the drop down.</span></span>

![Typ obsahu textu][api-management-response-body-content-type]

<span data-ttu-id="dacdd-172">Příklad textu odpovědi ve vybraném formátu vložte do textového pole.</span><span class="sxs-lookup"><span data-stu-id="dacdd-172">Paste the response body example in the selected format into the text box.</span></span> 

![Text odpovědi][api-management-response-body]

<span data-ttu-id="dacdd-174">V případě potřeby přidejte volitelný popis do **popis** textové pole.</span><span class="sxs-lookup"><span data-stu-id="dacdd-174">If desired, add an optional description into the **Description** text box.</span></span>

<span data-ttu-id="dacdd-175">Po operaci konfigurace, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="dacdd-175">Once the operation is configured, click **Save**.</span></span>

## <span data-ttu-id="dacdd-176"><a name="next-steps"> </a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="dacdd-176"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="dacdd-177">Jakmile se operace, které jsou přidány do rozhraní API, dalším krokem je spojit rozhraní API s produktem a publikujete ji tak, aby vývojáři můžou volat jeho operace.</span><span class="sxs-lookup"><span data-stu-id="dacdd-177">Once the operations are added to an API, the next step is to associate the API with a product and publish it so that developers can call its operations.</span></span>

* <span data-ttu-id="dacdd-178">[Postup vytvoření a publikování produktu][How to create and publish a product]</span><span class="sxs-lookup"><span data-stu-id="dacdd-178">[How to create and publish a product][How to create and publish a product]</span></span>

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

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to cache operation results in Azure API Management]: api-management-howto-cache.md
