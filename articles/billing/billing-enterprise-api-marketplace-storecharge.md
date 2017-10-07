---
title: "aaaAzure fakturace Enterprise rozhraní API - poplatky Marketplace | Microsoft Docs"
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
ms.openlocfilehash: cdf2836b52df06a4bf5ed71a476fe33662c5363c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---marketplace-store-charge"></a>Generování sestav rozhraní API pro podnikové zákazníky - poplatků úložiště Marketplace.

Hello Marketplace úložiště poplatků API vrátí hello na základě využití marketplace poplatky rozpis podle dne pro hello zadané fakturační období nebo počáteční a koncové datum (jednou poplatky nejsou součástí).

##<a name="request"></a>Žádost 
Nejsou zadány společných vlastností hlavičky, které je třeba přidat toobe [zde](billing-enterprise-api.md). Pokud není zadán fakturační období, pak data pro aktuální fakturaci hello období vrácena. Vlastní čas rozsahy lze určit hello počáteční a koncovou datum parametry, které jsou v hello formát rrrr MM-dd hello maximální podporované, když je rozsah 36 měsíců.  

|Metoda | Identifikátor URI požadavku|
|-|-|
|GET|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacecharges|
|GET|/billingPeriods/ https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} {billingPeriod} / marketplacecharges|
|GET|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacechargesbycustomdate? startTime = 2017-01-01 & endTime = 2017-01-10|

> [!Note]
> verze preview hello toouse rozhraní API, nahraďte v2 v1 v hello výše adresy URL.
>

## <a name="response"></a>Odpověď
 
    
        [
            {
                "id": "id",
                "subscriptionGuid": "00000000-0000-0000-0000-000000000000",
                "subscriptionName": "subName",
                "meterId": "2core",
                "usageStartDate": "2015-09-17T00:00:00Z",
                "usageEndDate": "2015-09-17T23:59:59Z",
                "offerName": "Virtual LoadMaster™ (VLM) for Azure",
                "resourceGroup": "Res group",
                "instanceId": "id",
                "additionalInfo": "{\"ImageType\":null,\"ServiceType\":\"Medium\"}",
                "tags": "",
                "orderNumber": "order",
                "unitOfMeasure": "",
                "costCenter": "100",
                "accountId": 100,
                "accountName": "Account Name",
                "accountOwnerId": "account@live.com",
                "departmentId": 101,
                "departmentName": "Department 1",
                "publisherName": "Publisher 1",
                "planName": "Plan name",
                "consumedQuantity": 1.15,
                "resourceRate": 0.1,
                "extendedCost": 1.11
            },
            ...
        ]
    

**Definice vlastností odpovědi**

|Název vlastnosti| Typ| Popis
|-|-|-|
|id|Řetězec|Jedinečné Id pro položku poplatků hello marketplace.|
|subscriptionGuid|Identifikátor GUID|Hello Guid předplatného|
|Název_předplatného|Řetězec|Hello název odběru|
|meterId|Řetězec|ID pro hello vygenerované měření|
|usageStartDate|Data a času|Počáteční čas pro záznam využití hello|
|usageEndDate|Data a času|Koncový čas pro záznam využití hello|
|offerName|Řetězec|Název nabídky Hello|
|Skupina prostředků|Řetězec|Hello prostředků skupiny|
|identifikátor instanceId|Řetězec|Instance Id|
|additionalInfo|Řetězec|Další informace o řetězce formátu JSON|
|tags|Řetězec|Řetězce formátu JSON značky|
|orderNumber|Řetězec|číslo objednávky Hello|
|unitOfMeasure|Řetězec|Měrné jednotky pro měření hello|
|CostCenter|Řetězec|center Hello náklady.|
|ID účtu|celá čísla|Id účtu Hello|
|název účtu|Řetězec |Hello název účtu|
|accountOwnerId|Řetězec|Hello Id vlastníka účtu|
|departmentId|celá čísla|oddělení Hello Id|
|DepartmentName|Řetězec|název oddělení Hello|
|publisherName|Řetězec|Název vydavatele Hello|
|planName|Řetězec|Název plánu Hello|
|consumedQuantity|Decimal|Spotřebované množství během tohoto období|
|resourceRate|Decimal|Jednotkové ceny pro měření hello|
|extendedCost|Decimal|Odhadované poplatků na základě objemu spotřebovaného a rozšířené náklady|
<br/>
## <a name="see-also"></a>Viz také

* [Fakturační období rozhraní API](billing-enterprise-api-billing-periods.md)

* [Podrobnosti o použití rozhraní API](billing-enterprise-api-usage-detail.md) 

* [Vyrovnávat a souhrn rozhraní API](billing-enterprise-api-balance-summary.md)

* [Rozhraní API list cena](billing-enterprise-api-pricesheet.md)