---
title: "Přehled rozhraní API předávání aaaAzure | Microsoft Docs"
description: "Přehled dostupných rozhraní API předávání přes Azure"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: sethm
ms.openlocfilehash: 3c4d737d5fee9a8babce094fa6dfddb28910834b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="available-relay-apis"></a>K dispozici předávání přes rozhraní API

## <a name="runtime-apis"></a>Modul runtime rozhraní API

Hello následující tabulka uvádí všechny klienty aktuálně k dispozici modul runtime předávání.

Hello [Další informace o](#additional-information) část obsahuje další informace o stavu hello jednotlivých modulu runtime knihoven.

| Jazyk nebo platformu | Dostupné funkce | Balíček klienta | Úložiště |
| --- | --- | --- | --- |
| Standardní rozhraní .NET | Hybridní připojení | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| Rozhraní .NET framework | WCF Relay | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | Není k dispozici |
| Node | Hybridní připojení | [`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Další informace

#### <a name="net"></a>.NET
Hello .NET ekosystém má více moduly runtime a proto nejsou více knihovny .NET pro centra událostí. knihovny .NET standardní Hello lze spouštět s využitím .NET Core nebo hello rozhraní .NET Framework, zatímco hello rozhraní .NET Framework – knihovna může spustit pouze v prostředí .NET Framework. Další informace o rozhraní .NET Framework naleznete v tématu [framework verze](/dotnet/articles/standard/frameworks#framework-versions).

## <a name="next-steps"></a>Další kroky
Další informace o předávání přes Azure, toolearn naleznete pod těmito odkazy:
* [Co je Azure Relay?](relay-what-is-it.md)
* [Přenos – nejčastější dotazy](relay-faq.md)