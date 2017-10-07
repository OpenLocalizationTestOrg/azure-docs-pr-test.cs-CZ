---
title: "aaaAzure fakturace Enterprise rozhraní API – souhrn a vyrovnávat | Microsoft Docs"
description: "Další informace o využití fakturace Azure a RateCard rozhraní API, které jsou používané tooprovide přehled o využívání prostředků Azure a trendy."
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
ms.openlocfilehash: b031de2c347e5abeacd11743cc96024434518918
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---balance-and-summary"></a>Rozhraní API pro vytváření sestav pro podnikové zákazníky – souhrn a vyrovnávat

Hello vyrovnávat a souhrn rozhraní API nabízí měsíční souhrnné informace na zůstatky, nové nákupy, poplatky za služby Azure Marketplace, úpravy a Nadlimitní poplatky.


##<a name="request"></a>Žádost 
Nejsou zadány společných vlastností hlavičky, které je třeba přidat toobe [zde](billing-enterprise-api.md). Pokud není zadán fakturační období, pak data pro aktuální fakturaci hello období vrácena.

|Metoda | Identifikátor URI požadavku|
|-|-|
|GET| https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / balancesummary|
|GET| /billingPeriods/ https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} {billingPeriod} / balancesummary|

> [!Note]
> verze preview hello toouse rozhraní API, nahraďte v2 v1 v hello výše adresy URL.
>

## <a name="response"></a>Odpověď

        {
            "id": "enrollments/100/billingperiods/201507/balancesummaries",
            "billingPeriodId": 201507,
            "currencyCode": "USD",
            "beginningBalance": 0,
            "endingBalance": 1.1,
            "newPurchases": 1,
            "adjustments": 1.1,
            "utilized": 1.1,
            "serviceOverage": 1,
            "chargesBilledSeparately": 1,
            "totalOverage": 1,
            "totalUsage": 1.1,
            "azureMarketplaceServiceCharges": 1,
            "newPurchasesDetails": [
                {
                "name": "",
                "value": 1
                }
            ],
            "adjustmentDetails": [
                {
                "name": "Promo Credit",
                "value": 1.1
                },
                {
                "name": "SIE Credit",
                "value": 1.0
                }
            ]
        }


**Definice vlastností odpovědi**

|Název vlastnosti| Typ| Popis
|-|-|-|
|id|Řetězec|Hello jedinečné Id pro konkrétní fakturační období a registrace|
|billingPeriodId|Řetězec |Hello fakturační období Id|
|Kód měny|Řetězec |Kód měny Hello|
|beginningBalance|Decimal| Hello počáteční zůstatek pro fakturační období hello|
|endingBalance|Decimal| Hello konečný zůstatek hello fakturační období (pro otevřete období, které to se budou denně aktualizovat)|
|newPurchases|Decimal| Celková velikost nového nákupu|
|úpravy|Decimal| Množství celkový počet nastavení|
|využité|Decimal| Celkové využití závazků|
|serviceOverage|Decimal| Nadlimitní hodnota pro služby Azure|
|chargesBilledSeparately|Decimal| Poplatky za fakturuje samostatně|
|totalOverage|Decimal| serviceOverage + chargesBilledSeparately|
|totalUsage|Decimal| Služba Azure závazků + celková Nadlimitní hodnota|
|azureMarketplaceServiceCharges|Decimal| Celkové náklady pro Azure Marketplace|
|newPurchasesDetails|Pole JSON řetězec dvojic název hodnota|Seznam nových nákupů|
|adjustmentDetails|Pole JSON řetězec dvojic název hodnota|Seznam úpravy (platební povýšení, SIE platební atd.) |


<br/>
## <a name="see-also"></a>Viz také

* [Fakturační období rozhraní API](billing-enterprise-api-billing-periods.md)

* [Podrobnosti o použití rozhraní API](billing-enterprise-api-usage-detail.md) 

* [Úložiště Marketplace poplatků rozhraní API](billing-enterprise-api-marketplace-storecharge.md) 

* [Rozhraní API list cena](billing-enterprise-api-pricesheet.md)