---
title: "Azure fakturace Enterprise rozhraní API | Microsoft Docs"
description: "Další informace o rozhraních API vytváření sestav, které umožňují zákazníkům Enterprise Azure a si vyžádá data spotřeby prostřednictvím kódu programu."
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
ms.openlocfilehash: e3a5f9bcd6b54a51c29df649f1ae8ac185b153a1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a>Přehled rozhraní API pro vytváření sestav pro podnikové zákazníky
Rozhraní API pro vytváření sestav umožňují zákazníky, kteří Enterprise Azure prostřednictvím kódu programu vyžádání využívání a fakturační údaje do nástrojů pro analýzu dat upřednostňované. 

## <a name="enabling-data-access-to-the-api"></a>Povolení přístupu k datům v rozhraní API
* **Generovat nebo načíst klíč rozhraní API** - protokolu v podnikovém portálu a postupujte podle kurzu v části Nápověda - Reporting rozhraní API. V první části v tomto článku nápovědy vysvětluje, jak vygenerovat nebo načíst klíč rozhraní API pro zadaný registrace.
* **Předávání klíče v rozhraní API** – klíč rozhraní API musí být předán pro každé volání pro ověřování a autorizaci. Následující vlastnost musí být hlavičkami protokolu HTTP

|Klíč hlavičky požadavku | Hodnota|
|-|-|
|Autorizace| Zadejte hodnotu v tomto formátu: **nosiče {API_KEY}** <br/> Příklad: nosiče eyr... 09|

## <a name="consumption-apis"></a>Rozhraní API spotřeba
Koncový bod Swagger je k dispozici [sem](https://consumption.azure.com/swagger/ui/index) pro rozhraní API popsané, pod kterou by měl povolit snadno introspection rozhraní API a generovat klientské sady SDK, pomocí [AutoRest](https://github.com/Azure/AutoRest) nebo [Swagger CodeGen](http://swagger.io/swagger-codegen/). Data od 1 pravděpodobně 2014 je k dispozici prostřednictvím tohoto rozhraní API. 

* **Souhrn a vyrovnávat** – [vyrovnávat a souhrn rozhraní API](billing-enterprise-api-balance-summary.md) nabízí měsíční souhrnné informace o zůstatky, nové nákupy, poplatky za služby Azure Marketplace, přizpůsobení a Nadlimitní poplatky.

* **Podrobnosti o použití** – [podrobnosti o použití rozhraní API](billing-enterprise-api-usage-detail.md) nabízí denní rozpis těchto spotřebované počty a odhadované poplatky podle zápisu. Výsledek také obsahuje informace o instancích, měřidla a oddělení. Rozhraní API můžete položit dotaz na fakturační období nebo zadaný počáteční a koncové datum. 

* **Úložiště Marketplace poplatků** – [Marketplace úložiště poplatků API](billing-enterprise-api-marketplace-storecharge.md) vrátí rozdělení na základě využití marketplace poplatky za den pro zadaný fakturační období nebo počáteční a koncové datum (jednou poplatky nejsou součástí).

* **Ceník** – [Price Sheet API](billing-enterprise-api-pricesheet.md) umožňuje použít rychlost pro každé monitorování dané registraci a fakturační období. 

## <a name="helper-apis"></a>Pomocná rozhraní API
 **Seznam fakturační období** – [fakturační období API](billing-enterprise-api-billing-periods.md) vrátí seznam hodnot fakturační období, která mají data energie pro zadanou registraci v obráceném chronologickém pořadí. Každé období obsahuje vlastnost odkazuje na trasu rozhraní API pro čtyři sady dat – BalanceSummary, UsageDetails, Marketplace poplatky a ceníku.


## <a name="api-response-codes"></a>Kódy odpovědí rozhraní API  
|Stavový kód odpovědi|Zpráva|Popis|
|-|-|-|
|200| OK|Žádná chyba|
|401| Neautorizováno| Klíč rozhraní API nebyl nalezen platný platnost atd.|
|404| Není k dispozici| Koncový bod sestavy nebyl nalezen|
|400| Chybný požadavek| Neplatné parametry – rozsahy dat, EA čísel atd.|
|500| Chyba serveru| Unexoected při zpracování žádosti| 









