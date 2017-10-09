---
title: "aaaAzure fakturace Enterprise rozhraní API | Microsoft Docs"
description: "Další informace o vytváření sestav rozhraní API, která umožňují Enterprise Azure zákazníků toopull spotřeby dat prostřednictvím kódu programu hello."
services: 
documentationcenter: 
author: aedwin
manager: aedwin
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: aedwin
ms.openlocfilehash: 017cecc57ad6bdeb402b5d9d57fc95df9b033a42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a>Přehled rozhraní API pro vytváření sestav pro podnikové zákazníky
Hello rozhraní API pro vytváření sestav povolit Enterprise Azure zákazníků tooprogrammatically vyžádání využívání a fakturační údaje do upřednostňovaných dat nástrojů pro analýzu. 

## <a name="enabling-data-access-toohello-api"></a>Povolení toohello API služby data access
* **Generovat nebo načíst klíč hello rozhraní API** - protokolu v toohello Enterprise portal a postupujte podle hello kurzu v části Nápověda - Reporting rozhraní API. první část Hello podle tohoto článku nápovědy vysvětluje, jak zadat toogenerate nebo načíst klíč hello rozhraní API pro hello registrace.
* **Předávání klíče v hello rozhraní API** -hello API klíč musí toobe předaná pro každé volání pro ověřování a autorizaci. Hello následující vlastnost potřebuje toobe toohello HTTP hlavičky

|Klíč hlavičky požadavku | Hodnota|
|-|-|
|Autorizace| Zadejte hodnotu hello v tomto formátu: **nosiče {API_KEY}** <br/> Příklad: nosiče eyr... 09|

## <a name="consumption-apis"></a>Rozhraní API spotřeba
Koncový bod Swagger je k dispozici [sem](https://consumption.azure.com/swagger/ui/index) pro hello rozhraní API popsané, pod kterou by měl povolit snadno introspection hello rozhraní API a hello možnost toogenerate klientskou sadu SDK pomocí [AutoRest](https://github.com/Azure/AutoRest) nebo [ Swagger CodeGen](http://swagger.io/swagger-codegen/). Data od 1 pravděpodobně 2014 je k dispozici prostřednictvím tohoto rozhraní API. 

* **Souhrn a vyrovnávat** – hello [vyrovnávat a souhrn rozhraní API](billing-enterprise-api-balance-summary.md) nabízí měsíční souhrnné informace o zůstatky, nové nákupy, poplatky za služby Azure Marketplace, přizpůsobení a Nadlimitní poplatky.

* **Podrobnosti o použití** – hello [podrobnosti o použití rozhraní API](billing-enterprise-api-usage-detail.md) nabízí denní rozpis těchto spotřebované počty a odhadované poplatky podle zápisu. výsledek Hello také obsahuje informace o instancích, měřidla a oddělení. Hello rozhraní API můžete položit dotaz na fakturační období nebo zadaný počáteční a koncové datum. 

* **Úložiště Marketplace poplatků** – hello [Marketplace úložiště poplatků API](billing-enterprise-api-marketplace-storecharge.md) vrací hello na základě využití marketplace poplatky rozpis podle dne pro hello zadané fakturační období nebo počáteční a koncové datum (jednou poplatky nejsou součástí) .

* **Ceník** – hello [Price Sheet API](billing-enterprise-api-pricesheet.md) poskytuje hello použít rychlost pro každé monitorování pro hello daného registrace a fakturační období. 

## <a name="helper-apis"></a>Pomocná rozhraní API
 **Seznam fakturační období** – hello [fakturační období API](billing-enterprise-api-billing-periods.md) vrátí seznam hodnot fakturační období, které mají datům o spotřebě pro hello zadané v chronologickém pořadí zpětného zápisu. Každé období obsahuje vlastnost odkazující toohello trasu rozhraní API pro hello čtyři sady dat – BalanceSummary, UsageDetails, Marketplace poplatky a ceníku.


## <a name="api-response-codes"></a>Kódy odpovědí rozhraní API  
|Stavový kód odpovědi|Zpráva|Popis|
|-|-|-|
|200| OK|Žádná chyba|
|401| Neautorizováno| Klíč rozhraní API nebyl nalezen platný platnost atd.|
|404| Není k dispozici| Koncový bod sestavy nebyl nalezen|
|400| Chybný požadavek| Neplatné parametry – rozsahy dat, EA čísel atd.|
|500| Chyba serveru| Unexoected při zpracování žádosti| 









