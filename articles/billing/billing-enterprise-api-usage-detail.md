---
title: "Azure fakturace Enterprise rozhraní API – podrobnosti o použití | Microsoft Docs"
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
ms.openlocfilehash: 5b49220e6eb27544dba54255ee88c56ad79c3141
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="reporting-apis-for-enterprise-customers---usage-details"></a>Rozhraní API pro vytváření sestav pro podnikové zákazníky – podrobnosti o použití

Rozhraní API podrobnosti o využití nabízí denní rozpis těchto spotřebované počty a odhadované poplatky podle zápisu. Výsledek také obsahuje informace o instancích, měřidla a oddělení. Rozhraní API můžete položit dotaz na fakturační období nebo zadaný počáteční a koncové datum. 
## <a name="consumption-apis"></a>Rozhraní API spotřeba


##<a name="request"></a>Žádost 
Nejsou zadány společných vlastností hlavičky, které je třeba přidat [zde](billing-enterprise-api.md). Pokud není zadán fakturační období, je vrácena data pro aktuální fakturační období. Vlastní časových rozsahů lze určit spuštění a ukončení datum parametry, které jsou ve formátu RRRR MM-dd. Maximální podporovaný časový rozsah je 36 měsíců.  

|Metoda | Identifikátor URI požadavku|
|-|-|
|GET|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / usagedetails 
|GET|/billingPeriods/ https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} {billingPeriod} / usagedetails|
|GET|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / usagedetailsbycustomdate? startTime = 2017-01-01 & endTime = 2017-01-10|

> [!Note]
> Pokud chcete používat verzi preview rozhraní API, nahraďte v2 v1 v výše uvedenou adresu URL.
>

## <a name="response"></a>Odpověď

> Z důvodu potenciálně velkého objemu dat výsledek je stránkovaného sady. Pokud je k dispozici, určuje vlastnost nextLink odkaz na další stránku dat. Pokud na odkaz je prázdná, označuje to, že je poslední stránky. 
<br/>

    {
        "id": "string",
        "data": [
            {                       
            "accountId": 0,
            "productId": 0,
            "resourceLocationId": 0,
            "consumedServiceId": 0,
            "departmentId": 0,
            "accountOwnerEmail": "string",
            "accountName": "string",
            "serviceAdministratorId": "string",
            "subscriptionId": 0,
            "subscriptionGuid": "string",
            "subscriptionName": "string",
            "date": "2017-04-27T23:01:43.799Z",
            "product": "string",
            "meterId": "string",
            "meterCategory": "string",
            "meterSubCategory": "string",
            "meterRegion": "string",
            "meterName": "string",
            "consumedQuantity": 0,
            "resourceRate": 0,
            "Cost": 0,
            "resourceLocation": "string",
            "consumedService": "string",
            "instanceId": "string",
            "serviceInfo1": "string",
            "serviceInfo2": "string",
            "additionalInfo": "string",
            "tags": "string",
            "storeServiceIdentifier": "string",
            "departmentName": "string",
            "costCenter": "string",
            "unitOfMeasure": "string",
            "resourceGroup": "string"
            }
        ],
        "nextLink": "string"
    }

<br/>
**Definice vlastností odpovědi**

|Název vlastnosti| Typ| Popis
|-|-|-|
|id| Řetězec| Jedinečné Id pro volání rozhraní API. |
|data| Pole JSON| Pole pro každý instance\meter denní podrobnosti o použití.|
|odkaz nextLink| Řetězec| Pokud jsou dostupné další stránky dat odkaz nextLink odkazuje na URL pro návrat na další stránku data. |
|ID účtu| celá čísla| Zastaralé pole. Dispozici z důvodu zpětné kompatibility. |
|productId| celá čísla| Zastaralé pole. Dispozici z důvodu zpětné kompatibility. |
|resourceLocationId| celá čísla| Zastaralé pole. Dispozici z důvodu zpětné kompatibility. |
|consumedServiceID| celá čísla| Zastaralé pole. Dispozici z důvodu zpětné kompatibility. |
|departmentId| celá čísla| Zastaralé pole. Dispozici z důvodu zpětné kompatibility. |
|accountOwnerEmail| Řetězec| E-mailový účet vlastníka účtu. |
|název účtu| Řetězec| Zákazník zadaný název účtu. |
|serviceAdministratorId| Řetězec| E-mailovou adresu z Správce služeb. |
|subscriptionId| celá čísla| Zastaralé pole. Dispozici z důvodu zpětné kompatibility. |
|subscriptionGuid| Řetězec| Globální jedinečný identifikátor pro předplatné. |
|Název_předplatného| Řetězec| Název odběru. |
|Datum| Řetězec| Datum, kdy došlo k využívání. |
|Produktu| Řetězec| Další informace o měřidla. Příklad: A1 systému Windows (VM) - Asie a Tichomoří – východ|
|meterId| Řetězec| Identifikátor pro monitorování, které vygenerované využití. |
|meterCategory| Řetězec| Služba platformy Azure, která byla použita. |
|meterSubCategory| Řetězec| Definuje typ služby Azure, který může mít vliv na rychlost. Příklad: A1 virtuálních počítačů (jiný systém než Windows|
|meterRegion| Řetězec| Určuje polohu datového centra. U některých služeb vycházejí ceny z umístění datového centra. |
|meterName| Řetězec| Název měřidla. |
|consumedQuantity| Double| Množství monitorování, které bylo spotřebováno. |
|resourceRate| Double| Míra použít na fakturovatelný jednotku. |
|Náklady| Double| Poplatků, které vznikly pro měřidla. |
|resourceLocation| Řetězec| Identifikuje datacenter, kde je spuštěna měřidla. |
|consumedService| Řetězec| Služba platformy Azure, která byla použita. |
|identifikátor instanceId| Řetězec| Tento identifikátor je název prostředku nebo plně kvalifikovaný ID prostředku. Další informace najdete v tématu [rozhraní API služby Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/resources) |
|serviceInfo1| Řetězec| Metadata pro interní služby Azure. |
|serviceInfo2| Řetězec| Například typ image pro virtuální počítač a název poskytovatele internetových služeb pro ExpressRoute. |
|additionalInfo| Řetězec| Metadata specifická pro služby. Například bitové kopie typ pro virtuální počítač. |
|tags| Řetězec| Zákazník přidat značky. Další informace najdete v tématu [uspořádání prostředků Azure pomocí značek](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags). |
|storeServiceIdentifier| Řetězec| Tento sloupce se nepoužije. Dispozici z důvodu zpětné kompatibility. |
|DepartmentName| Řetězec| Název oddělení. |
|CostCenter| Řetězec| Nákladové středisko, které je přidružené využití. |
|unitOfMeasure| Řetězec| Určuje jednotku, která služba je účtován v. Příklad: GB, hodin, 10 000 s. |
|Skupina prostředků| Řetězec| Skupinu prostředků, ve které je nasazený měření spuštěný v. Další informace naleznete v tématu [Přehled Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). |
<br/>
## <a name="see-also"></a>Viz také

* [Fakturační období rozhraní API](billing-enterprise-api-billing-periods.md)

* [Vyrovnávat a souhrn rozhraní API](billing-enterprise-api-balance-summary.md)

* [Úložiště Marketplace poplatků rozhraní API](billing-enterprise-api-marketplace-storecharge.md) 

* [Rozhraní API list cena](billing-enterprise-api-pricesheet.md)
