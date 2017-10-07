---
title: "aaaAzure fakturace Enterprise rozhraní API - ceník | Microsoft Docs"
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
ms.openlocfilehash: 4cfe694c63fba266d117054b046d9caacb3b7197
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---price-sheet"></a>Rozhraní API pro vytváření sestav pro podnikové zákazníky - ceníku

Hello Price Sheet API poskytuje hello použít rychlost pro každé monitorování pro hello daného registrace a fakturace období.

##<a name="request"></a>Žádost
Nejsou zadány společných vlastností hlavičky, které je třeba přidat toobe [zde](billing-enterprise-api.md). Pokud není zadán fakturační období, pak data pro aktuální fakturaci hello období vrácena.

|Metoda | Identifikátor URI požadavku|
|-|-|
|GET|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / ceník|
|GET|/billingPeriods/ https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} {billingPeriod} / ceník|

> [!Note]
> verze preview hello toouse rozhraní API, nahraďte v2 v1 v hello výše adresy URL.
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
>Pokud používáte hello Preview rozhraní API, meterId pole není k dispozici.
>

**Definice vlastností odpovědi**

|Název vlastnosti| Typ| Popis
|-|-|-|
|id| Řetězec| Hello jedinečné Id, který představuje položku určitý Ceník (monitorovat pomocí fakturační období)|
|billingPeriodId| Řetězec| Hello jedinečné Id, který představuje konkrétní období fakturace|
|meterId| Řetězec| identifikátor Hello hello měření. Může být namapované toohello meterId využití.|
|meterName| Řetězec| Název měřidla Hello|
|unitOfMeasure| Řetězec| Hello jednotka pro měření hello služby|
|includedQuantity| Decimal| Množství, které je součástí |
|partNumber| Řetězec| Hello výrobní číslo spojené s hello měření|
|unitPrice| Decimal| Hello jednotkové ceny pro měření hello|
|Kód měny| Řetězec| Kód měny Hello pro hello unitPrice|
<br/>
## <a name="see-also"></a>Viz také

* [Fakturační období rozhraní API](billing-enterprise-api-billing-periods.md)

* [Podrobnosti o použití rozhraní API](billing-enterprise-api-usage-detail.md)

* [Vyrovnávat a souhrn rozhraní API](billing-enterprise-api-balance-summary.md)

* [Úložiště Marketplace poplatků rozhraní API](billing-enterprise-api-marketplace-storecharge.md)
