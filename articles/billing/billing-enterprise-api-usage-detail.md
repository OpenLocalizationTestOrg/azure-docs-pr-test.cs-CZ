---
title: "aaaAzure fakturace Enterprise rozhraní API – podrobnosti o použití | Microsoft Docs"
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
ms.openlocfilehash: def0805008261df5872f015db3d2b26e47d25569
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---usage-details"></a>Rozhraní API pro vytváření sestav pro podnikové zákazníky – podrobnosti o použití

Hello podrobnosti o použití rozhraní API nabízí denní rozpis těchto spotřebované počty a odhadované poplatky podle zápisu. výsledek Hello také obsahuje informace o instancích, měřidla a oddělení. Hello rozhraní API můžete položit dotaz na fakturační období nebo zadaný počáteční a koncové datum. 
## <a name="consumption-apis"></a>Rozhraní API spotřeba


##<a name="request"></a>Žádost 
Nejsou zadány společných vlastností hlavičky, které je třeba přidat toobe [zde](billing-enterprise-api.md). Pokud není zadán fakturační období, pak data pro aktuální fakturaci hello období vrácena. Vlastní časových rozsahů lze určit hello spuštění a ukončení datum parametry, které jsou v hello formát rrrr MM-dd. maximální podporovaný časový rozsah Hello je 36 měsíců.  

|Metoda | Identifikátor URI požadavku|
|-|-|
|GET|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / usagedetails 
|GET|/billingPeriods/ https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} {billingPeriod} / usagedetails|
|GET|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / usagedetailsbycustomdate? startTime = 2017-01-01 & endTime = 2017-01-10|

> [!Note]
> verze preview hello toouse rozhraní API, nahraďte v2 v1 v hello výše adresy URL.
>

## <a name="response"></a>Odpověď

> Z důvodu potenciálně velkého objemu dat hello výsledek toohello je stránkovaného sady. Vlastnost nextLink Hello, pokud existuje, určuje hello odkaz na další stránku hello data. Pokud hello odkaz je prázdná, označuje to, že je poslední stránku hello. 
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
|id| Řetězec| Hello jedinečné Id pro volání rozhraní API hello. |
|data| Pole JSON| Pole pro každý instance\meter denní podrobnosti o použití Hello.|
|odkaz nextLink| Řetězec| Pokud jsou dostupné další stránky s daty hello nextLink body toohello URL tooreturn hello další dat. |
|ID účtu| celá čísla| Zastaralé pole. Dispozici z důvodu zpětné kompatibility. |
|productId| celá čísla| Zastaralé pole. Dispozici z důvodu zpětné kompatibility. |
|resourceLocationId| celá čísla| Zastaralé pole. Dispozici z důvodu zpětné kompatibility. |
|consumedServiceID| celá čísla| Zastaralé pole. Dispozici z důvodu zpětné kompatibility. |
|departmentId| celá čísla| Zastaralé pole. Dispozici z důvodu zpětné kompatibility. |
|accountOwnerEmail| Řetězec| E-mailový účet hello vlastníka účtu. |
|název účtu| Řetězec| Zákazník zadaný název účtu hello. |
|serviceAdministratorId| Řetězec| E-mailovou adresu z Správce služeb. |
|subscriptionId| celá čísla| Zastaralé pole. Dispozici z důvodu zpětné kompatibility. |
|subscriptionGuid| Řetězec| Globální jedinečný identifikátor pro předplatné hello. |
|Název_předplatného| Řetězec| Název odběru hello. |
|Datum| Řetězec| Hello datum, kdy došlo k využívání. |
|Produktu| Řetězec| Další informace o měření hello. Příklad: A1 systému Windows (VM) - Asie a Tichomoří – východ|
|meterId| Řetězec| identifikátor Hello měřidlo hello vygenerované využití. |
|meterCategory| Řetězec| Hello služby platformy Azure, která byla použita. |
|meterSubCategory| Řetězec| Definuje typ hello služby Azure, který může mít vliv na rychlost hello. Příklad: A1 virtuálních počítačů (jiný systém než Windows|
|meterRegion| Řetězec| Určuje umístění hello hello datacenter pro určité služby, které jsou za cenu na základě umístění datového centra. |
|meterName| Řetězec| Název měřidla hello. |
|consumedQuantity| Double| Hello množství hello monitorování, které bylo spotřebováno. |
|resourceRate| Double| míra Hello použít na fakturovatelný jednotku. |
|Náklady| Double| Hello poplatků, které vznikly pro měření hello. |
|resourceLocation| Řetězec| Identifikuje hello datacenter se spuštěným systémem měření hello. |
|consumedService| Řetězec| Hello služby platformy Azure, která byla použita. |
|identifikátor instanceId| Řetězec| Tento identifikátor je hello název prostředku hello nebo hello plně kvalifikovaný ID prostředku. Další informace najdete v tématu [rozhraní API služby Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/resources) |
|serviceInfo1| Řetězec| Metadata pro interní služby Azure. |
|serviceInfo2| Řetězec| Například typ image pro virtuální počítač a název poskytovatele internetových služeb pro ExpressRoute. |
|additionalInfo| Řetězec| Metadata specifická pro služby. Například bitové kopie typ pro virtuální počítač. |
|tags| Řetězec| Zákazník přidat značky. Další informace najdete v tématu [uspořádání prostředků Azure pomocí značek](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags). |
|storeServiceIdentifier| Řetězec| Tento sloupce se nepoužije. Dispozici z důvodu zpětné kompatibility. |
|DepartmentName| Řetězec| Název oddělení hello. |
|CostCenter| Řetězec| Nákladové středisko Hello hello využití je přidružen. |
|unitOfMeasure| Řetězec| Identifikuje hello jednotku, která je účtován hello služby v. Příklad: GB, hodin, 10 000 s. |
|Skupina prostředků| Řetězec| v které hello nasazené měření běží ve skupině prostředků Hello. Další informace naleznete v tématu [Přehled Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). |
<br/>
## <a name="see-also"></a>Viz také

* [Fakturační období rozhraní API](billing-enterprise-api-billing-periods.md)

* [Vyrovnávat a souhrn rozhraní API](billing-enterprise-api-balance-summary.md)

* [Úložiště Marketplace poplatků rozhraní API](billing-enterprise-api-marketplace-storecharge.md) 

* [Rozhraní API list cena](billing-enterprise-api-pricesheet.md)
