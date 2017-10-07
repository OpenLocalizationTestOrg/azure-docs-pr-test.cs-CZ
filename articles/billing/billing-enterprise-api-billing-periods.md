---
title: "aaaAzure fakturace Enterprise rozhraní API - fakturační období | Microsoft Docs"
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
ms.openlocfilehash: d4e17f25b22729a7f213306fb019ee0dbeca87ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---billing-periods"></a>Rozhraní API pro vytváření sestav pro podnikové zákazníky - fakturační období

Hello fakturační období API vrátí seznam fakturační období, které mají datům o spotřebě pro hello zadat registrace v obráceném chronologickém pořadí. Každé období obsahuje vlastnost odkazující toohello trasu rozhraní API pro hello čtyři sady dat – BalanceSummary, UsageDetails, Marktplace poplatky a ceník. Pokud hello období neobsahuje žádná data, hello odpovídající vlastnost má hodnotu null. 


##<a name="request"></a>Žádost 
Nejsou zadány společných vlastností hlavičky, které je třeba přidat toobe [zde](billing-enterprise-api.md). 

|Metoda | Identifikátor URI požadavku|
|-|-|
|GET| https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / billingperiods|

> [!Note]
> verze preview hello toouse rozhraní API, nahraďte v2 v1 v hello výše adresy URL.
>

## <a name="response"></a>Odpověď
 
    
    
      [
            {
                "billingPeriodId": "201704",
                "billingStart": "2017-04-01T00:00:00Z",
                "billingEnd": "2017-04-30T11:59:59Z",
                "balanceSummary": "/v1/enrollments/100/billingperiods/201704/balancesummary",
                "usageDetails": "/v1/enrollments/100/billingperiods/201704/usagedetails",
                "marketplaceCharges": "/v1/enrollments/100/billingperiods/201704/marketplacecharges",
                "priceSheet": "/v1/enrollments/100/billingperiods/201704/pricesheet"
            },          
            ....
      ]
    

**Definice vlastností odpovědi**

|Název vlastnosti| Typ| Popis
|-|-|-|
|billingPeriodId| Řetězec| Hello jedinečné Id, který představuje konkrétní období fakturace|
|billingStart| Data a času| ISO 8601 řetězec představující datum začátku období hello|
|billingEnd| Data a času| ISO 8601 řetězec představující datum ukončení období hello|
|balanceSummary| Řetězec| Hello cestu adresy URL, který směruje toohello vyrovnávání souhrnná data pro toto období|
|usageDetails| Řetězec| Hello cestu adresy URL, který směruje toohello podrobnosti o použití dat pro toto období|
|marketplaceCharges| Řetězec| Hello cestu adresy URL, který směruje toohello poplatky Marketplace. data pro toto období|
|Ceník| Řetězec| Hello cestu adresy URL, který směruje toohello ceník data pro toto období|

<br/>
## <a name="see-also"></a>Viz také

* [Vyrovnávat a souhrn rozhraní API](billing-enterprise-api-balance-summary.md)

* [Podrobnosti o použití rozhraní API](billing-enterprise-api-usage-detail.md) 

* [Úložiště Marketplace poplatků rozhraní API](billing-enterprise-api-marketplace-storecharge.md) 

* [Rozhraní API list cena](billing-enterprise-api-pricesheet.md)