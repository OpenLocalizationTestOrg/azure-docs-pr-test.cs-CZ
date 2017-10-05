---
title: "Azure fakturace rozhraní API Enterprise - poplatky Marketplace | Microsoft Docs"
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
ms.openlocfilehash: 5539623f7ae35e14b6dafe6fdf9efe4bcaba4fd3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---marketplace-store-charge"></a>Generování sestav rozhraní API pro podnikové zákazníky - poplatků úložiště Marketplace.

Rozhraní API Marketplace úložiště poplatků vrátí rozpis poplatky na základě využití marketplace podle den v zadaném období fakturace nebo počáteční a koncové datum (jednou poplatky nejsou součástí).

##<a name="request"></a>Žádost 
Nejsou zadány společných vlastností hlavičky, které je třeba přidat [zde](billing-enterprise-api.md). Pokud není zadán fakturační období, je vrácena data pro aktuální fakturační období. Vlastní časových rozsahů lze určit spuštění a ukončení datum parametry, které jsou ve formátu RRRR MM-dd, maximální podporovaná časový rozsah je 36 měsíců.  

|Metoda | Identifikátor URI požadavku|
|-|-|
|GET|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacecharges|
|GET|/billingPeriods/ https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} {billingPeriod} / marketplacecharges|
|GET|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacechargesbycustomdate? startTime = 2017-01-01 & endTime = 2017-01-10|

> [!Note]
> Pokud chcete používat verzi preview rozhraní API, nahraďte v2 v1 v výše uvedenou adresu URL.
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
|id|Řetězec|Jedinečné Id pro položku poplatků marketplace.|
|subscriptionGuid|Identifikátor GUID|Guid předplatného|
|Název_předplatného|Řetězec|Název odběru|
|meterId|Řetězec|ID pro emitovaného měření|
|usageStartDate|Data a času|Počáteční čas pro záznam využití|
|usageEndDate|Data a času|Koncový čas pro záznam využití|
|offerName|Řetězec|Název nabídky|
|Skupina prostředků|Řetězec|Skupiny prostředků|
|identifikátor instanceId|Řetězec|Instance Id|
|additionalInfo|Řetězec|Další informace o řetězce formátu JSON|
|tags|Řetězec|Řetězce formátu JSON značky|
|orderNumber|Řetězec|Pořadové číslo|
|unitOfMeasure|Řetězec|Měrné jednotky pro měřidla|
|CostCenter|Řetězec|Nákladové středisko|
|ID účtu|celá čísla|Id účtu|
|název účtu|Řetězec |Název účtu|
|accountOwnerId|Řetězec|Id vlastníka účtu|
|departmentId|celá čísla|Oddělení Id|
|DepartmentName|Řetězec|Název oddělení|
|publisherName|Řetězec|Název vydavatele|
|planName|Řetězec|Název plánu|
|consumedQuantity|Decimal|Spotřebované množství během tohoto období|
|resourceRate|Decimal|Jednotkové ceny pro měřidla|
|extendedCost|Decimal|Odhadované poplatků na základě objemu spotřebovaného a rozšířené náklady|
<br/>
## <a name="see-also"></a>Viz také

* [Fakturační období rozhraní API](billing-enterprise-api-billing-periods.md)

* [Podrobnosti o použití rozhraní API](billing-enterprise-api-usage-detail.md) 

* [Vyrovnávat a souhrn rozhraní API](billing-enterprise-api-balance-summary.md)

* [Rozhraní API list cena](billing-enterprise-api-pricesheet.md)