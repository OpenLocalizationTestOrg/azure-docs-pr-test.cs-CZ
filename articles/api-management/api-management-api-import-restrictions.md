---
title: "import aaaRestrictions a známé problémy v Azure API Management API | Microsoft Docs"
description: "Podrobnosti o známých problémech a omezení při importu do Azure API Management pomocí hello otevřené rozhraní API, WSDL nebo WADL formáty."
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
ms.openlocfilehash: 0bed5ace47de6ccbfbecba25ea6b69c5329de089
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="api-import-restrictions-and-known-issues"></a>Omezení import rozhraní API a známé problémy
## <a name="about-this-list"></a>O tomto seznamu
Při každé úsilí tooensure, který import rozhraní API do Azure API Management jako plynulé a bezproblémový nejblíže budeme příležitostně ukládat omezení nebo identifikovat problémy vyžadující toobe opravena, aby bylo možné úspěšně naimportovat. Tento článek tyto dokumenty organizuje hello import formát hello rozhraní API.

## <a name="open-api"></a>Otevřete rozhraní API/Swagger
Obecně platí, pokud obdržíte chyby import dokumentu otevřené rozhraní API, zkontrolujte, zda jste ověřili jeho – buď pomocí návrháře hello v hello nového portálu Azure (Editor specifikace otevřené rozhraní API návrhu - Front-endu -), nebo se 3. stran nástroj <a href="http://www.swagger.io"> Swagger Editor</a>.

* **Název hostitele** jsme vyžadují atribut název hostitele.
* **Základní cesta** jsme vyžadují atribut základní cesta.
* **Schémata** vyžadujeme schéma pole. 

## <a name="wsdl"></a>WSDL
WSDL soubory jsou použité toogenerate rozhraní API průchozí SOAP nebo slouží jako hello back-end SOAP REST API.

* **WSDL: import** aktuálně nepodporujeme rozhraní API pomocí tohoto atributu. Zákazníci měli sloučit hello importovat elementy do jednoho dokumentu.
* **Zprávy s více částmi** aktuálně nejsou podporované.
* **WCF wsHttpBinding** SOAP služby vytvořené pomocí Windows Communication Foundation by měl používat basicHttpBinding – wsHttpBinding není podporován.
* **MTOM** služby používající MTOM <em>může</em> fungovat. V tuto chvíli není nabídnuta oficiální podporu.
* **Rekurze** typy, které jsou definované rekurzivně (například viz. tooan pole sami) nejsou podporovány.

## <a name="wadl"></a>WADL
Aktuálně neexistují žádné známé problémy WADL importu.


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
