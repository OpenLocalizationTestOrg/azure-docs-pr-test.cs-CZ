---
title: "Azure fakturace rozhraní API Enterprise – souhrn a vyrovnávat | Microsoft Docs"
description: "Další informace o využití fakturace Azure a RateCard rozhraní API, které poskytují přehled o využívání prostředků Azure a trendy."
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
ms.openlocfilehash: f6b149f0e656d2263705048aa5b644f5bb4a5712
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---balance-and-summary"></a>Rozhraní API pro vytváření sestav pro podnikové zákazníky – souhrn a vyrovnávat

Vyrovnávat a souhrn rozhraní API nabízí měsíční souhrnné informace na zůstatky, nové nákupy, poplatky za služby Azure Marketplace, úpravy a Nadlimitní poplatky.


##<a name="request"></a>Žádost 
Nejsou zadány společných vlastností hlavičky, které je třeba přidat [zde](billing-enterprise-api.md). Pokud není zadán fakturační období, je vrácena data pro aktuální fakturační období.

|Metoda | Identifikátor URI požadavku|
|-|-|
|GET| https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / balancesummary|
|GET| /billingPeriods/ https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} {billingPeriod} / balancesummary|

> [!Note]
> Pokud chcete používat verzi preview rozhraní API, nahraďte v2 v1 v výše uvedenou adresu URL.
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
|id|Řetězec|Jedinečné Id pro konkrétní fakturační období a registrace|
|billingPeriodId|Řetězec |Fakturační období Id|
|Kód měny|Řetězec |Kód měny|
|beginningBalance|Decimal| Počáteční zůstatek za fakturační období|
|endingBalance|Decimal| Konečný zůstatek za fakturační období (pro otevřete období, které to se budou denně aktualizovat)|
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