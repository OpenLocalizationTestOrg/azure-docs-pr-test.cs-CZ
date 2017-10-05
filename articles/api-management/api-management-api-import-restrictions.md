---
title: "Omezení a známé problémy v Azure API Management API import | Microsoft Docs"
description: "Podrobnosti o známých problémech a omezení při importu do Azure API Management pomocí otevřené rozhraní API, WSDL nebo WADL formáty."
services: api-management
documentationcenter: 
author: mattfarm
manager: vlvinogr
editor: 
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: apipm
ms.openlocfilehash: ac799d66b5038c207413086b0fa71239ff2a332f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="api-import-restrictions-and-known-issues"></a><span data-ttu-id="e95af-103">Omezení import rozhraní API a známé problémy</span><span class="sxs-lookup"><span data-stu-id="e95af-103">API import restrictions and known issues</span></span>
## <a name="about-this-list"></a><span data-ttu-id="e95af-104">O tomto seznamu</span><span class="sxs-lookup"><span data-stu-id="e95af-104">About this list</span></span>
<span data-ttu-id="e95af-105">Při každé úsilí zajistit, že import rozhraní API do Azure API Management je jako plynulé a bez problémů, co jsme příležitostně ukládat omezení nebo identifikovat problémy, které budou muset být opravena, aby bylo možné úspěšně naimportovat.</span><span class="sxs-lookup"><span data-stu-id="e95af-105">While every effort is made to ensure that importing your API into Azure API Management is as seamless and problem-free as possible, we do occasionally impose restrictions or identify issues that will need to be rectified before you can successfully import.</span></span> <span data-ttu-id="e95af-106">Tento článek tyto dokumenty organizuje formát import rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e95af-106">This article documents these, organised by the import format of the API.</span></span>

## <span data-ttu-id="e95af-107"><a name="open-api"></a>Otevřete rozhraní API/Swagger</span><span class="sxs-lookup"><span data-stu-id="e95af-107"><a name="open-api"> </a>Open API/Swagger</span></span>
<span data-ttu-id="e95af-108">Obecně platí, pokud obdržíte chyby import dokumentu otevřené rozhraní API, zkontrolujte, zda jste ověřili jeho – buď pomocí návrháře nového portálu Azure (návrhu - Front-endu - otevřené rozhraní API specifikace Editor), nebo se 3. stran nástroj <a href="http://www.swagger.io">Swagger Editor</a>.</span><span class="sxs-lookup"><span data-stu-id="e95af-108">In general, if you are receiving errors importing your Open API document, please ensure you have validated it - either using the designer in the new Azure Portal (Design - Front End - Open API Specification Editor), or with a 3rd party tool such as <a href="http://www.swagger.io">Swagger Editor</a>.</span></span>

* <span data-ttu-id="e95af-109">**Název hostitele** jsme vyžadují atribut název hostitele.</span><span class="sxs-lookup"><span data-stu-id="e95af-109">**Host Name** we require a host name attribute.</span></span>
* <span data-ttu-id="e95af-110">**Základní cesta** jsme vyžadují atribut základní cesta.</span><span class="sxs-lookup"><span data-stu-id="e95af-110">**Base Path** we require a base path attribute.</span></span>
* <span data-ttu-id="e95af-111">**Schémata** vyžadujeme schéma pole.</span><span class="sxs-lookup"><span data-stu-id="e95af-111">**Schemes** we require a scheme array.</span></span> 

## <span data-ttu-id="e95af-112"><a name="wsdl"></a>WSDL</span><span class="sxs-lookup"><span data-stu-id="e95af-112"><a name="wsdl"> </a>WSDL</span></span>
<span data-ttu-id="e95af-113">WSDL soubory se používají ke generování SOAP průchozí rozhraní API nebo sloužit jako back-end SOAP REST API.</span><span class="sxs-lookup"><span data-stu-id="e95af-113">WSDL files are used to generate SOAP Pass-through APIs, or serve as the backend of a SOAP-to-REST API.</span></span>

* <span data-ttu-id="e95af-114">**WSDL: import** aktuálně nepodporujeme rozhraní API pomocí tohoto atributu.</span><span class="sxs-lookup"><span data-stu-id="e95af-114">**WSDL:Import** we do not currently support APIs using this attribute.</span></span> <span data-ttu-id="e95af-115">Zákazníci měli sloučit importované elementy do jednoho dokumentu.</span><span class="sxs-lookup"><span data-stu-id="e95af-115">Customers should merge the imported elements into one document.</span></span>
* <span data-ttu-id="e95af-116">**Zprávy s více částmi** aktuálně nejsou podporované.</span><span class="sxs-lookup"><span data-stu-id="e95af-116">**Messages with multiple parts** are currently not supported.</span></span>
* <span data-ttu-id="e95af-117">**WCF wsHttpBinding** SOAP služby vytvořené pomocí Windows Communication Foundation by měl používat basicHttpBinding – wsHttpBinding není podporován.</span><span class="sxs-lookup"><span data-stu-id="e95af-117">**WCF wsHttpBinding** SOAP services created with Windows Communication Foundation should use basicHttpBinding - wsHttpBinding is not supported.</span></span>
* <span data-ttu-id="e95af-118">**MTOM** služby používající MTOM <em>může</em> fungovat.</span><span class="sxs-lookup"><span data-stu-id="e95af-118">**MTOM** Services using MTOM <em>may</em> work.</span></span> <span data-ttu-id="e95af-119">V tuto chvíli není nabídnuta oficiální podporu.</span><span class="sxs-lookup"><span data-stu-id="e95af-119">Official support is not offered at this time.</span></span>
* <span data-ttu-id="e95af-120">**Rekurze** typy, které jsou definované rekurzivně (například odkazovat na pole sami) nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="e95af-120">**Recursion** types that are defined recursively (e.g. refer to an array of themselves) are not supported.</span></span>

## <span data-ttu-id="e95af-121"><a name="wadl"></a>WADL</span><span class="sxs-lookup"><span data-stu-id="e95af-121"><a name="wadl"> </a>WADL</span></span>
<span data-ttu-id="e95af-122">Aktuálně neexistují žádné známé problémy WADL importu.</span><span class="sxs-lookup"><span data-stu-id="e95af-122">There are no known WADL import issues currently.</span></span>


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
