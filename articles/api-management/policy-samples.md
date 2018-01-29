---
title: "Ukázky zásad Azure API Management | Microsoft Docs"
description: "Další informace o zásady, které jsou k dispozici pro použití v Azure API Management."
services: api-management
documentationcenter: 
author: Juliako
manager: cflower
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: sample
ms.date: 10/31/2017
ms.author: apimpm
ms.custom: mvc
ms.openlocfilehash: e951eb47290a2d1b6f892f1ccf923f6ce0c1899c
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="api-management-policy-samples"></a>Ukázky rozhraní API správy zásad

[Zásady](api-management-howto-policies.md) jsou vynikající funkcí systému, který umožňuje vydavatele můžete změnit chování rozhraní API prostřednictvím konfigurace. Zásady představují kolekci příkazů, které se postupně provádí na základě požadavku nebo odezvy z rozhraní API. Následující tabulka obsahuje odkazy na ukázky a poskytuje stručný popis každého vzorku.

|||
|---|---|
|**Příchozí zásady**||
|[Přidat hlavičku předané umožňuje rozhraní API pro vytvoření správné adresy URL back-end](./policies/set-header-to-enable-backend-to-construct-urls.md?toc=api-management/toc.json) |Ukazuje, jak přidat hlavičku předané v příchozí žádosti o umožňuje rozhraní API pro vytvoření správné adresy URL back-end.|
|[Přidat hlavičku obsahující id korelace](./policies/add-correlation-id.md?toc=api-management/toc.json) |Ukazuje, jak přidat hlavičku obsahující ID korelace na příchozí požadavek.|
|[Přidávání funkcí do back-end službu a odpověď do mezipaměti](./policies/cache-response.md?toc=api-management/toc.json) |Ukazuje, jak pro přidání možností do back-end službu. Například přijměte název místě, ne na zeměpisnou šířku a délku předpovědi počasí rozhraní API.|
|[Autorizace přístupu na základě deklarací JWT](./policies/authorize-request-based-on-jwt-claims.md?toc=api-management/toc.json) |Ukazuje, jak k autorizaci přístupu pro konkrétní metody HTTP do rozhraní API založené na deklaracích identity JWT.|
|[Autorizace přístupu pomocí token Google OAuth](./policies/use-google-as-oauth-token-provider.md?toc=api-management/toc.json) |Ukazuje, jak autorizace přístupu k koncové body pomocí Google jako zprostředkovatel tokenu OAuth.|
|[Vygenerování sdíleného přístupového podpisu a předat dál požadavek do úložiště Azure](./policies/generate-shared-access-signature.md?toc=api-management/toc.json) |Ukazuje, jak vygenerovat [sdíleného přístupového podpisu](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) pomocí výrazů a předání požadavku do úložiště Azure zásadám přepisování uri. |
|[Získání tokenu přístupu OAuth2 z AAD a předá je na back-end](./policies/use-oauth2-for-authorization.md?toc=api-management/toc.json) |Poskytuje příklad použití OAuth2 pro ověřování mezi bránou rozhraní a back-end. Ukazuje, jak získat přístupový token z AAD a předá je na back-end.|
|[Získat X-proti útokům CSRF tokenu z bráně SAP pomocí odeslání žádosti o zásadu](./policies/get-x-csrf-token-from-sap-gateway.md?toc=api-management/toc.json) |Ukazuje, jak implementovat X proti útokům CSRF vzor používá mnoho rozhraní API. V tomto příkladu je konkrétní k bráně SAP. |
|[Směrování požadavku na základě velikosti jeho obsahu](./policies/route-requests-based-on-size.md?toc=api-management/toc.json) |Ukazuje, jak pro směrování požadavků na základě velikosti jejich subjektů.|
|[Poslat informace o kontextu požadavku službě back-end](./policies/send-request-context-info-to-backend-service.md?toc=api-management/toc.json) |Ukazuje, jak odeslat určité kontextové informace službě back-end pro protokolování nebo zpracování.|
|[Nastavení doby trvání mezipaměti odpovědi](./policies/set-cache-duration.md?toc=api-management/toc.json) |Ukazuje, jak nastavit dobu trvání mezipaměti odpovědi v maxAge hodnotu hlavičky Cache-Control poslal back-end.|
|**Odchozí zásady**||
|[Filtrování obsahu odpovědi](./policies/filter-response-content.md?toc=api-management/toc.json) | Ukazuje, jak filtrovat elementy data z datové části odpovědi na základě produktu přidružený k požadavku.|
|**Zásady při chybě**||
|[Ukládat chyby do Stackify](./policies/log-errors-to-stackify.md?toc=api-management/toc.json) |Ukazuje, jak přidat zásadu protokolování chyba k odesílání chyb do Stackify pro protokolování.|
