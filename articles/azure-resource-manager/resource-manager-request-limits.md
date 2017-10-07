---
title: "omezení žádostí o aaaAzure Resource Manager | Microsoft Docs"
description: "Popisuje, jak toouse omezení pomocí Azure Resource Manageru požadavky při dosáhli limity předplatného."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: e1047233-b8e4-4232-8919-3268d93a3824
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2017
ms.author: tomfitz
ms.openlocfilehash: ea274f3145af36aac753730e1f280d8a8b59c3bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="throttling-resource-manager-requests"></a>Omezení žádostí Resource Manager
Pro každé předplatné a klienta Resource Manager omezení čtení too15 požadavky, 000 za hodinu a zápisu too1 požadavky, 200 za hodinu. Tyto limity platí instanci Azure Resource Manager tooeach; existuje více instancí v každé oblasti Azure a Azure Resource Manager je nasazené tooall Azure oblasti.  Tím, že v praxi, jsou efektivně mnohem vyšší než výše uvedené omezení, jako uživatel požadavky jsou obecně obsluhovány pomocí mnoha různými instancemi.

Pokud vaše aplikace nebo skriptu dosáhne tyto limity, je nutné toothrottle své žádosti. Toto téma ukazuje, jak toodetermine hello zbývající požadavky, je nutné před dosažením limitu hello a jak toorespond, když jste dosáhli limitu hello.

Po dosažení limitu hello obdržíte kód stavu hello HTTP **429 příliš mnoho požadavků**.

Hello počet požadavků, které je vymezená tooeither vašeho předplatného nebo tenanta. Pokud máte více souběžných zasílání požadavků v rámci vašeho předplatného, hello požadavky z těchto aplikací se aplikace přidávají společně toodetermine hello počet zbývajících požadavků.

Předplatné obor požadavky jsou ty, které zahrnují hello předávání svoje id předplatného, jako je například načítání hello skupiny prostředků v rámci vašeho předplatného. Požadavky klienta obor nezahrnují svoje id předplatného, jako je například načítání platná umístění Azure.

## <a name="remaining-requests"></a>Zbývající požadavků
Hello počet zbývajících požadavků, které můžete zjistit tak, že prověří hlavičky odpovědi. Každý požadavek obsahuje hodnoty pro počet hello zbývající ke čtení a zápisu požadavků. Hello následující tabulka popisuje hello hlavičky odpovědi, které můžete zkontrolovat pro tyto hodnoty:

| Hlavička odpovědi | Popis |
| --- | --- |
| x-MS-ratelimit-Remaining-Subscription-reads |Předplatné obor čte zbývající |
| x-MS-ratelimit-Remaining-Subscription-Writes |Předplatné obor zapíše zbývající |
| x-MS-ratelimit-Remaining-tenant-reads |Obor klienta čte zbývající |
| x-MS-ratelimit-Remaining-tenant-Writes |Obor klienta zapíše zbývající |
| x-MS-ratelimit-Remaining-Subscription-Resource-Requests |Předplatné obor zbývající žádosti typu prostředku.<br /><br />Hodnotu této hlavičky je vrácena pouze v případě služby má přepsat výchozí limit hello. Resource Manager přidá tato hodnota místo hello předplatné čtení nebo zápisu. |
| x-MS-ratelimit-Remaining-Subscription-Resource-entities-Read |Předplatné obor zbývající žádosti pro kolekci prostředků typu.<br /><br />Hodnotu této hlavičky je vrácena pouze v případě služby má přepsat výchozí limit hello. Tuto hodnotu poskytuje hello počet zbývajících požadavků kolekce (seznam prostředky). |
| x-MS-ratelimit-Remaining-tenant-Resource-Requests |Klienta obor zbývající žádosti typu prostředku.<br /><br />Tuto hlavičku přidat jenom pro požadavky na úrovni klienta a pouze v případě služby má přepsat výchozí limit hello. Resource Manager přidá tato hodnota místo hello klienta čtení nebo zápisu. |
| x-MS-ratelimit-Remaining-tenant-Resource-entities-Read |Klienta obor zbývající žádosti pro kolekci prostředků typu.<br /><br />Tuto hlavičku přidat jenom pro požadavky na úrovni klienta a pouze v případě služby má přepsat výchozí limit hello. |

## <a name="retrieving-hello-header-values"></a>Načítání hodnoty hlavičky hello
Načítání tyto hodnoty hlavičky v kódu nebo skriptu je nejsou jiné než načítání žádnou hodnotu hlavičky. 

Například v **C#**, načtení hello hodnota hlavičky ze **HttpWebResponse** objekt s názvem **odpovědi** s hello následující kód:

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

V **prostředí PowerShell**, načíst hodnotu hlavičky hello z operace Invoke-WebRequest.

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

Nebo, pokud chcete zbývající hello toosee požadavky pro ladění, můžete zadat hello **– ladění** parametr na vaše **prostředí PowerShell** rutiny.

```powershell
Get-AzureRmResourceGroup -Debug
```

Která vrací mnoho hodnot, včetně hello následující hodnota odezvy:

```powershell
...
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 14999
...
```

V **rozhraní příkazového řádku Azure**, načíst hodnotu hlavičky hello pomocí hello podrobnější možnost.

```azurecli
azure group list -vv --json
```

Která vrací mnoho hodnot, včetně hello následujících objektů:

```azurecli
...
silly: returnObject
{
  "statusCode": 200,
  "header": {
    "cache-control": "no-cache",
    "pragma": "no-cache",
    "content-type": "application/json; charset=utf-8",
    "expires": "-1",
    "x-ms-ratelimit-remaining-subscription-reads": "14998",
    ...
```

## <a name="waiting-before-sending-next-request"></a>Čekání před odesláním další požadavek
Po dosažení limitu požadavků hello Resource Manager vrátí hello **429** stavový kód HTTP a **opakovat po** hodnota v hlavičce hello. Hello **opakovat po** hodnota určuje hello počet sekund, které má aplikace čekat (nebo spánku) před odesláním další požadavek hello. Pokud odešlete žádost předtím, než dojde k uplynutí hodnota pro opakovaný pokus hello, váš požadavek nebude zpracován a se vrátí novou hodnotu opakování.

## <a name="next-steps"></a>Další kroky

* Další informace o omezení a kvóty najdete v tématu [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).
* toolearn o zpracování asynchronní REST žádostí, najdete v části [sledovat asynchronní operace Azure](resource-manager-async-operations.md).
