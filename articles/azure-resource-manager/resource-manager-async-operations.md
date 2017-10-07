---
title: "asynchronní operace aaaAzure | Microsoft Docs"
description: "Popisuje, jak tootrack asynchronních operací v Azure."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2017
ms.author: tomfitz
ms.openlocfilehash: b81254196013adf87998eff11a50993efa52d40d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="track-asynchronous-azure-operations"></a>Sledování asynchronní operace v Azure
Některé operace Azure REST spustit asynchronně, protože hello operaci nelze dokončit rychle. Toto téma popisuje, jak tootrack hello stav asynchronní operace prostřednictvím hodnoty, vrátí se v odpovědi hello.  

## <a name="status-codes-for-asynchronous-operations"></a>Stavové kódy pro asynchronní operace
Asynchronní operace původně vrátí kód stavu HTTP buď:

* 201 (vytvořeno)
* 202 (platných) 

Při úspěšném dokončení operace hello, vrátí buď:

* 200 (OK)
* 204 (žádný obsah) 

Odkazovat toohello [dokumentace k REST API](/rest/api/) toosee hello odpovědi pro operaci hello jsou prováděny. 

## <a name="monitor-status-of-operation"></a>Sledujte stav operace
Hello asynchronní REST operations návratový hodnoty hlavičky, které používají toodetermine hello stav hello operaci. Potenciálně existují tři hlavičky tooexamine hodnoty:

* `Azure-AsyncOperation`-Adresa URL pro kontrolu hello probíhající stav operace hello. Pokud vaše operace vrací hodnotu této, vždy používejte it (namísto umístění) tootrack hello stav operace hello.
* `Location`-Adresa URL pro určení po dokončení operace. Tato hodnota se používá jenom v případě, že Azure AsyncOperation nevrátí.
* `Retry-After`-hello počet sekund toowait před zaškrtnutím hello stav asynchronní operace hello.

Všechny tyto hodnoty se ale vrátí se nemusí být vždy asynchronní operaci. Například může být nutné hodnota hlavičky Azure AsyncOperation hello tooevaluate pro jednu operaci a hodnota hlavičky hello umístění pro jiná operace. 

Hodnoty hlavičky hello načíst, protože by načíst všechny hodnoty v záhlaví požadavku. Například v jazyce C#, můžete načíst hodnotu hlavičky hello z `HttpWebResponse` objekt s názvem `response` s hello následující kód:

```cs
response.Headers.GetValues("Azure-AsyncOperation").GetValue(0)
```

## <a name="azure-asyncoperation-request-and-response"></a>Azure AsyncOperation žádosti a odpovědi

tooget hello stav asynchronní operace hello, odeslat požadavek GET toohello URL v Azure AsyncOperation hodnotu hlavičky.

Hello text odpovědi hello z této operace obsahuje informace o operaci hello. Hello následující příklad ukazuje hello vrácená z operace hello možné hodnoty:

```json
{
    "id": "{resource path from GET operation}",
    "name": "{operation-id}", 
    "status" : "Succeeded | Failed | Canceled | {resource provider values}", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
    "percentComplete": {double between 0 and 100 },
    "properties": {
        /* Specific resource provider values for successful operations */
    },
    "error" : { 
        "code": "{error code}",  
        "message": "{error description}" 
    }
}
```

Pouze `status` se vrátí pro všechny odpovědi. Pokud stav hello se nezdařilo nebo zrušeno, je vrácen objekt chyba Hello. Všechny ostatní hodnoty jsou volitelné. tedy hello odpovědi, které obdržíte může vypadat jinak, než příklad hello.

## <a name="provisioningstate-values"></a>hodnoty stavu zřizování

Operace, které slouží k vytvoření, aktualizace nebo odstranění (DELETE PUT, PATCH,) prostředku obvykle vrací `provisioningState` hodnotu. Po dokončení operace se vrátí jednu z následujících tří hodnot: 

* Úspěch
* Se nezdařilo
* Zrušeno

Všechny ostatní hodnoty označují, že je stále spuštěna operace hello. Poskytovatel prostředků Hello může vrátit vlastní hodnotu, která určuje jeho stav. Například můžete obdržet **platných** po hello žádosti přijaté a spuštěná.

## <a name="example-requests-and-responses"></a>Příklad požadavky a odpovědi

### <a name="start-virtual-machine-202-with-azure-asyncoperation"></a>Spustit virtuální počítač (202 s Azure AsyncOperation)
Tento příklad ukazuje, jak toodetermine hello stav **spustit** operace pro virtuální počítače. počáteční žádost Hello je hello následující formát:

```HTTP
POST 
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Compute/virtualMachines/{vm-name}/start?api-version=2016-03-30
```

Vrátí stavový kód 202. Mezi hodnoty hlavičky hello zobrazí:

```HTTP
Azure-AsyncOperation : https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

toocheck hello stav asynchronní operace hello odesílání jinou adresu URL toothat požadavku.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

text odpovědi Hello obsahuje hello stav operace hello:

```json
{
  "startTime": "2017-01-06T18:58:24.7596323+00:00",
  "status": "InProgress",
  "name": "9a062a88-e463-4697-bef2-fe039df73a02"
}
```

### <a name="deploy-resources-201-with-azure-asyncoperation"></a>Nasadit prostředky (201 s Azure AsyncOperation)

Tento příklad ukazuje, jak toodetermine hello stav **nasazení** operace pro nasazení tooAzure prostředky. počáteční žádost Hello je hello následující formát:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/microsoft.resources/deployments/{deployment-name}?api-version=2016-09-01
```

Vrátí stavový kód 201. Hello text odpovědi hello zahrnuje:

```json
"provisioningState":"Accepted",
```

Mezi hodnoty hlavičky hello zobrazí:

```HTTP
Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

toocheck hello stav asynchronní operace hello odesílání jinou adresu URL toothat požadavku.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

text odpovědi Hello obsahuje hello stav operace hello:

```json
{"status":"Running"}
```

Po dokončení nasazení hello hello odpovědi obsahuje:

```json
{"status":"Succeeded"}
```

### <a name="create-storage-account-202-with-location-and-retry-after"></a>Vytvořit účet úložiště (202 umístění a zkuste to znovu za)

Tento příklad ukazuje, jak toodetermine hello stav hello **vytvořit** operace pro účty úložiště. počáteční žádost Hello je hello následující formát:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2016-01-01
```

A hello text žádosti obsahuje vlastnosti pro účet úložiště hello:

```json
{ "location": "South Central US", "properties": {}, "sku": { "name": "Standard_LRS" }, "kind": "Storage" }
```

Vrátí stavový kód 202. Mezi hodnoty hlavičky hello najdete v části hello následující dvě hodnoty:

```HTTP
Location: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
Retry-After: 17
```

Po uplynutí počtu sekund zadaný v opakovat po, zkontrolujte hello stav asynchronní operace hello odesláním jinou adresu URL toothat požadavku.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
```

Pokud požadavek hello je stále spuštěná, obdržíte kód stavu 202. Pokud požadavek hello byl dokončen, vaše přijímat stavovým kódem 200 a hello text odpovědi hello obsahuje vlastnosti hello hello účtu úložiště, který byl vytvořen.

## <a name="next-steps"></a>Další kroky

* Dokumentaci o každé operace REST najdete v tématu [dokumentace k REST API](/rest/api/).
* Informace o správě prostředků prostřednictvím hello REST API Resource Manageru najdete v tématu [hello pomocí REST API Resource Manageru](resource-manager-rest-api.md).
* informace o nasazení šablony prostřednictvím hello REST API Resource Manageru najdete v tématu [nasazení prostředků pomocí šablony Resource Manageru a REST API Resource Manageru](resource-group-template-deploy-rest.md).
