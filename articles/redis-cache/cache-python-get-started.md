---
title: "Použití Azure Redis Cache s Pythonem | Dokumentace Microsoftu"
description: "Začínáme s Azure Redis Cache pomocí Pythonu"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: v-lincan
ms.assetid: f186202c-fdad-4398-af8c-aee91ec96ba3
ms.service: cache
ms.devlang: python
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/10/2017
ms.author: sdanie
ms.openlocfilehash: cdbee52574d0ffbe82ef3dc98f2848f4d00ba2ff
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-azure-redis-cache-with-python"></a>Použití Azure Redis Cache s Pythonem
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Toto téma ukazuje, jak začít s Azure Redis Cache pomocí Pythonu.

## <a name="prerequisites"></a>Požadavky
Nainstalujte [redis-py](https://github.com/andymccurdy/redis-py).

## <a name="create-a-redis-cache-on-azure"></a>Vytvoření mezipaměti Redis v Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Načtení názvu hostitele a přístupových klíčů
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="enable-the-non-ssl-endpoint"></a>Povolení koncového bodu bez SSL
Někteří klienti Redis nepodporují SSL a ve výchozím nastavení je [port bez SSL pro nové instance služby Azure Redis Cache zakázán](cache-configure.md#access-ports). V době psaní tohoto textu klient [redis-py](https://github.com/andymccurdy/redis-py) nepodporuje SSL. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Přidání dat do mezipaměti a jejich načtení
    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Nahraďte `<name>` názvem vaší mezipaměti a `key` vaším přístupovým klíčem.

<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
