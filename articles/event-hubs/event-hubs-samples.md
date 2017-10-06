---
title: "ukázky služby Event Hubs aaaAzure | Microsoft Docs"
description: "Ukázek Azure Event Hubs"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: f01f52e6c13f9e885999a6726143440bbc70446d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-samples"></a>Ukázky centra událostí 

Hello sady Azure Event Hubs vzorků ukazuje klíčové funkce v [Azure Event Hubs](/azure/event-hubs/). Tento článek rozděluje a popisuje hello vzorků, které jsou k dispozici, tooeach odkazy.

V době psaní tohoto textu hello Event Hubs ukázky umístěny v několika různých místech:

- [Ukázky kódu vývojáře MSDN](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples)

Další informace o různých verzích hello rozhraní .NET Framework najdete v tématu [architektury a cíle](/dotnet/articles/standard/frameworks).

Další ukázky bude přidán v čase, tak zkontrolujte, vraťte se sem často aktualizací.

## <a name="net-standard"></a>Standardní rozhraní .NET

předvedení technologie Hello následující ukázky jak toosend a příjmu událostí pomocí hello [klienta služby Event Hubs](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) pro hello [.NET standardní knihovna](/dotnet/articles/standard/library).

### <a name="send-events"></a>Odesílání událostí 

Hello [Začínáme odesílání](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) příklad ukazuje, jak toowrite .NET Core Konzolová aplikace, který odesílá centra událostí tooan události.

### <a name="receive-events"></a>Příjem událostí 

Hello [začít pracovat s hello Event Processor Host přijetí](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) ukázka je aplikace konzoly .NET Core, která přijímá zprávy z centra událostí pomocí hello Event Processor Host.

## <a name="net-framework"></a>Rozhraní .NET framework   

Tyto ukázky demonstrují různým funkcím služby Azure Event Hubs, cílení hello [rozhraní .NET Framework – knihovna](/dotnet/framework/index).
 
### <a name="notify-users-of-events-received"></a>Upozorněte uživatele událostí přijatých

Hello [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) ukázka upozorní uživatele z dat přijatých ze senzorů nebo jinými systémy.

### <a name="get-started-with-event-hubs"></a>Začínáme se službou Event Hubs 

Hello [Event Hubs Začínáme](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) příklad znázorňuje hello základní možnosti služby Event Hubs, například jak odesílat události centra událostí tooan toocreate centra událostí a využívat událostí pomocí hello [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).

### <a name="scale-out-event-processing"></a>Horizontální navýšení kapacity zpracování událostí 

Hello [horizontální navýšení kapacity zpracování událostí](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) příklad znázorňuje způsob toouse hello [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) toodistribute hello zatížení spotřeby Event Hubs datového proudu. Ukazuje, jak tooimplement hello **EventProcessor** a **EventProcessorFactory** datového proudu událostí objekty toomanage hello. 

###  <a name="pull-data-from-sql-into-an-event-hub"></a>Získání dat z SQL do centra událostí

Hello [dat stahování SQL](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) příklad ukazuje, jak toopull data z SQL tabulky a poslat ho tooan centra událostí, toouse jako vstup v příjem dat analytických aplikací.

### <a name="pull-web-data-into-an-event-hub"></a>Získání dat webové do centra událostí 

Hello [umožňuje importovat data z webové hello](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) příklad ukazuje, jak toopull data z veřejné kanály (například hello informace o datových přenosech ministerstvo dopravy na informační kanál) a poslat ho tooan centra událostí.

## <a name="next-steps"></a>Další kroky

Další informace o verzích rozhraní .NET Framework návštěvou hello následující odkazy:

- [Rozhraní a cíle](/dotnet/articles/standard/frameworks)
- [Rozhraní .NET framework 4.6 a 4.5](/dotnet/framework/index)

Další informace o službě Event Hubs v hello následující články:

- [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
- [Vytvoření centra událostí](event-hubs-create.md)
- [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)