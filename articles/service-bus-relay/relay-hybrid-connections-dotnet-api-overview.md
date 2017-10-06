---
title: "aaaOverview hello standardní rozhraní API Správce Azure předávání přes rozhraní .NET | Microsoft Docs"
description: "Předávání přehled standardní rozhraní API .NET"
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b1da9ac1-811b-4df7-a22c-ccd013405c40
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2017
ms.author: sethm
ms.openlocfilehash: c90e00e809bd44eb0fbbff5eb03dfc8afa486523
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-relay-hybrid-connections-net-standard-api-overview"></a>Přehled služby Azure předávání hybridní připojení .NET standardní API

Tento článek shrnuje některé klíče hello Azure předávání hybridní připojení .NET standardní [rozhraní API klienta](/dotnet/api/microsoft.azure.relay).
  
## <a name="relay-connection-string-builder"></a>Předávání Tvůrce připojovacích řetězců

Hello [RelayConnectionStringBuilder] [ RelayConnectionStringBuilder] třída formáty připojovací řetězce, které jsou specifické tooRelay hybridní připojení. Můžete ji pomocí formátu hello tooverify připojovací řetězec, nebo toobuild připojovací řetězec od začátku. Viz následující příklad kódu hello:

```csharp
var endpoint = "{Relay namespace}";
var entityPath = "{Name of hello Hybrid Connection}";
var sharedAccessKeyName = "{SAS key name}";
var sharedAccessKey = "{SAS key value}";

var connectionStringBuilder = new RelayConnectionStringBuilder()
{
    Endpoint = endpoint,
    EntityPath = entityPath,
    SharedAccessKeyName = sasKeyName,
    SharedAccessKey = sasKeyValue
};
```

Můžete také předat připojení přímo řetězec toohello `RelayConnectionStringBuilder` metoda. Tato operace povolí tooverify, který je hello připojovací řetězec v platném formátu. Pokud některý z parametrů hello jsou neplatné, generuje konstruktor hello `ArgumentException`.

```csharp
var myConnectionString = "{RelayConnectionString}";
// Declare hello connectionStringBuilder so that it can be used outside of hello loop if needed
RelayConnectionStringBuilder connectionStringBuilder;
try
{
    // Create hello connectionStringBuilder using hello supplied connection string
    connectionStringBuilder = new RelayConnectionStringBuilder(myConnectionString);
}
catch (ArgumentException ae)
{
    // Perform some error handling
}
```

## <a name="hybrid-connection-stream"></a>Hybridní připojení datového proudu
Hello [HybridConnectionStream] [ HCStream] třída je hello primární objekt použitý toosend a přijímat data z Azure předávání koncový bod, zda pracujete [HybridConnectionClient] [ HCClient], nebo [HybridConnectionListener][HCListener].

### <a name="getting-a-hybrid-connection-stream"></a>Získávání datový proud s hybridní připojení

#### <a name="listener"></a>Naslouchací proces
Použití [HybridConnectionListener][HCListener], můžete získat `HybridConnectionStream` objektu následujícím způsobem:

```csharp
// Use hello RelayConnectionStringBuilder tooget a valid connection string
var listener = new HybridConnectionListener(csb.ToString());
// Open a connection toohello Relay endpoint
await listener.OpenAsync();
// Get a `HybridConnectionStream`
var hybridConnectionStream = await listener.AcceptConnectionAsync();
```

#### <a name="client"></a>Klient
Použití [HybridConnectionClient][HCClient], můžete získat `HybridConnectionStream` objektu následujícím způsobem:

```csharp
// Use hello RelayConnectionStringBuilder tooget a valid connection string
var client = new HybridConnectionClient(csb.ToString());
// Open a connection toohello Relay endpoint and get a `HybridConnectionStream`
var hybridConnectionStream = await client.CreateConnectionAsync();
```

### <a name="receiving-data"></a>Přijetí dat
Hello [HybridConnectionStream] [ HCStream] třída umožňuje obousměrnou komunikaci. Ve většině případů nepřetržitě obdržíte z datového proudu hello. Při čtení textu ze streamu hello, může být také vhodné toouse [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) objekt, který umožňuje jednodušší analýza dat hello. Například můžete číst data jako text, nikoli jako `byte[]`.

Hello následující kód čte jednotlivé řádky textu z datového proudu hello až zrušení je požadováno:

```csharp
// Create a CancellationToken, so that we can cancel hello while loop
var cancellationToken = new CancellationToken();
// Create a StreamReader from hello 'hybridConnectionStream`
var streamReader = new StreamReader(hybridConnectionStream);

while (!cancellationToken.IsCancellationRequested)
{
    // Read a line of input until a newline is encountered
    var line = await streamReader.ReadLineAsync();
    if (string.IsNullOrEmpty(line))
    {
        // If there's no input data, we will signal that 
        // we will no longer send data on this connection
        // and then break out of hello processing loop.
        await hybridConnectionStream.ShutdownAsync(cancellationToken);
        break;
    }
}
```

### <a name="sending-data"></a>Odeslání dat
Jakmile máte připojení bylo vytvořeno, můžete odeslat koncový bod zpráva toohello předávání. Protože objekt připojení hello dědí [datového proudu](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), odesílat data podle `byte[]`. Následující příklad ukazuje, jak Hello toodo toto:

```csharp
var data = Encoding.UTF8.GetBytes("hello");
await clientConnection.WriteAsync(data, 0, data.Length);
```

Pokud chcete toosend text přímo, bez nutnosti tooencode hello řetězec pokaždé, když, můžete však zabalit hello `hybridConnectionStream` objektu s [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) objektu.

```csharp
// hello StreamWriter object only needs toobe created once
var textWriter = new StreamWriter(hybridConnectionStream);
await textWriter.WriteLineAsync("hello");
```

## <a name="next-steps"></a>Další kroky
Další informace o předávání přes Azure, toolearn naleznete pod těmito odkazy:

* [Odkaz na Microsoft.Azure.Relay](/dotnet/api/microsoft.azure.relay)
* [Co je Azure Relay?](relay-what-is-it.md)
* [K dispozici předávání přes rozhraní API](relay-api-overview.md)

[RelayConnectionStringBuilder]: /dotnet/api/microsoft.azure.relay.relayconnectionstringbuilder
[HCStream]: /dotnet/api/microsoft.azure.relay.hybridconnectionstream
[HCClient]: /dotnet/api/microsoft.azure.relay.hybridconnectionclient
[HCListener]: /dotnet/api/microsoft.azure.relay.hybridconnectionlistener