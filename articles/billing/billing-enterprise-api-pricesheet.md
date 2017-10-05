---
title: "Azure fakturace rozhraní API Enterprise - ceník | Microsoft Docs"
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
ms.openlocfilehash: 2e7d6e883abe4cee13bc5f684baf2a1ea9c6c397
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---price-sheet"></a>Rozhraní API pro vytváření sestav pro podnikové zákazníky - ceníku

Rozhraní API Price Sheet poskytuje použít rychlost pro každé monitorování pro danou registrace a fakturace období.

##<a name="request"></a>Žádost
Nejsou zadány společných vlastností hlavičky, které je třeba přidat [zde](billing-enterprise-api.md). Pokud není zadán fakturační období, je vrácena data pro aktuální fakturační období.

|Metoda | Identifikátor URI požadavku|
|-|-|
|GET|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / ceník|
|GET|/billingPeriods/ https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} {billingPeriod} / ceník|

> [!Note]
> Pokud chcete používat verzi preview rozhraní API, nahraďte v2 v1 v výše uvedenou adresu URL.
>

## <a name="response"></a>Odpověď

    
        [
            {
                "id": "enrollments/57354989/billingperiods/201601/products/343/pricesheets",
                "billingPeriodId": "201704",
                "meterId": "dc210ecb-97e8-4522-8134-2385494233c0",
                "meterName": "A1 VM",
                "unitOfMeasure": "100 Hours",
                "includedQuantity": 0,
                "partNumber": "N7H-00015",
                "unitPrice": 0.00,
                "currencyCode": "USD"
            },
            {
                "id": "enrollments/57354989/billingperiods/201601/products/2884/pricesheets",
                "billingPeriodId": "201404",
                "meterId": "dc210ecb-97e8-4522-8134-5385494233c0",
                "meterName": "Locally Redundant Storage Premium Storage - Snapshots - AU East",
                "unitOfMeasure": "100 GB",
                "includedQuantity": 0,
                "partNumber": "N9H-00402",
                "unitPrice": 0.00,
                "currencyCode": "USD"
            },
            ...
        ]
    

> [!Note]
>Pokud používáte rozhraní API verze Preview, meterId pole není k dispozici.
>

**Definice vlastností odpovědi**

|Název vlastnosti| Typ| Popis
|-|-|-|
|id| Řetězec| Jedinečné Id, který představuje položku určitý Ceník (monitorovat pomocí fakturační období)|
|billingPeriodId| Řetězec| Jedinečné Id, který představuje konkrétní období fakturace|
|meterId| Řetězec| Identifikátor pro měřidla. Může být namapované na meterId využití.|
|meterName| Řetězec| Název měřidla|
|unitOfMeasure| Řetězec| Jednotka pro měření služby|
|includedQuantity| Decimal| Množství, které je součástí |
|partNumber| Řetězec| Výrobní číslo spojené s měřidla|
|unitPrice| Decimal| Jednotkové ceny pro měřidla|
|Kód měny| Řetězec| Kód měny pro pole unitPrice|
<br/>
## <a name="see-also"></a>Viz také

* [Fakturační období rozhraní API](billing-enterprise-api-billing-periods.md)

* [Podrobnosti o použití rozhraní API](billing-enterprise-api-usage-detail.md)

* [Vyrovnávat a souhrn rozhraní API](billing-enterprise-api-balance-summary.md)

* [Úložiště Marketplace poplatků rozhraní API](billing-enterprise-api-marketplace-storecharge.md)
