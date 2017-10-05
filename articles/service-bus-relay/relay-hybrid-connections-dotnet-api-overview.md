---
title: "Přehled Azure předávání přes standardní API technologie .NET | Microsoft Docs"
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
ms.openlocfilehash: f3f4a2e721b1a75a5b92a5c17a9939c7013340d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-relay-hybrid-connections-net-standard-api-overview"></a><span data-ttu-id="d9546-103">Přehled služby Azure předávání hybridní připojení .NET standardní API</span><span class="sxs-lookup"><span data-stu-id="d9546-103">Azure Relay Hybrid Connections .NET Standard API overview</span></span>

<span data-ttu-id="d9546-104">Tento článek je uveden přehled Azure předávání hybridní připojení .NET standardní klíče [rozhraní API klienta](/dotnet/api/microsoft.azure.relay).</span><span class="sxs-lookup"><span data-stu-id="d9546-104">This article summarizes some of the key Azure Relay Hybrid Connections .NET Standard [client APIs](/dotnet/api/microsoft.azure.relay).</span></span>
  
## <a name="relay-connection-string-builder"></a><span data-ttu-id="d9546-105">Předávání Tvůrce připojovacích řetězců</span><span class="sxs-lookup"><span data-stu-id="d9546-105">Relay Connection String Builder</span></span>

<span data-ttu-id="d9546-106">[RelayConnectionStringBuilder] [ RelayConnectionStringBuilder] třída formáty připojovací řetězce, které jsou specifické pro předávání hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="d9546-106">The [RelayConnectionStringBuilder][RelayConnectionStringBuilder] class formats connection strings that are specific to Relay Hybrid Connections.</span></span> <span data-ttu-id="d9546-107">Můžete ho se ověřit formát připojovací řetězec, nebo vytvořit připojovací řetězec od začátku.</span><span class="sxs-lookup"><span data-stu-id="d9546-107">You can use it to verify the format of a connection string, or to build a connection string from scratch.</span></span> <span data-ttu-id="d9546-108">Najdete příklad následující kód:</span><span class="sxs-lookup"><span data-stu-id="d9546-108">See the following code for an example:</span></span>

```csharp
var endpoint = "{Relay namespace}";
var entityPath = "{Name of the Hybrid Connection}";
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

<span data-ttu-id="d9546-109">Připojovací řetězec můžete předat také přímo na `RelayConnectionStringBuilder` metoda.</span><span class="sxs-lookup"><span data-stu-id="d9546-109">You can also pass a connection string directly to the `RelayConnectionStringBuilder` method.</span></span> <span data-ttu-id="d9546-110">Tato operace umožňuje ověřte, zda je připojovací řetězec v platném formátu.</span><span class="sxs-lookup"><span data-stu-id="d9546-110">This operation enables you to verify that the connection string is in a valid format.</span></span> <span data-ttu-id="d9546-111">Pokud některý z parametrů jsou neplatné, vygeneruje konstruktoru `ArgumentException`.</span><span class="sxs-lookup"><span data-stu-id="d9546-111">If any of the parameters are invalid, the constructor generates an `ArgumentException`.</span></span>

```csharp
var myConnectionString = "{RelayConnectionString}";
// Declare the connectionStringBuilder so that it can be used outside of the loop if needed
RelayConnectionStringBuilder connectionStringBuilder;
try
{
    // Create the connectionStringBuilder using the supplied connection string
    connectionStringBuilder = new RelayConnectionStringBuilder(myConnectionString);
}
catch (ArgumentException ae)
{
    // Perform some error handling
}
```

## <a name="hybrid-connection-stream"></a><span data-ttu-id="d9546-112">Hybridní připojení datového proudu</span><span class="sxs-lookup"><span data-stu-id="d9546-112">Hybrid Connection Stream</span></span>
<span data-ttu-id="d9546-113">[HybridConnectionStream] [ HCStream] třída je primární objekt použitý k odesílat a přijímat data z Azure předávání koncový bod, zda pracujete [HybridConnectionClient] [ HCClient], nebo [HybridConnectionListener][HCListener].</span><span class="sxs-lookup"><span data-stu-id="d9546-113">The [HybridConnectionStream][HCStream] class is the primary object used to send and receive data from an Azure Relay endpoint, whether you are working with a [HybridConnectionClient][HCClient], or a [HybridConnectionListener][HCListener].</span></span>

### <a name="getting-a-hybrid-connection-stream"></a><span data-ttu-id="d9546-114">Získávání datový proud s hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="d9546-114">Getting a Hybrid Connection Stream</span></span>

#### <a name="listener"></a><span data-ttu-id="d9546-115">Naslouchací proces</span><span class="sxs-lookup"><span data-stu-id="d9546-115">Listener</span></span>
<span data-ttu-id="d9546-116">Použití [HybridConnectionListener][HCListener], můžete získat `HybridConnectionStream` objektu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d9546-116">Using a [HybridConnectionListener][HCListener], you can obtain a `HybridConnectionStream` object as follows:</span></span>

```csharp
// Use the RelayConnectionStringBuilder to get a valid connection string
var listener = new HybridConnectionListener(csb.ToString());
// Open a connection to the Relay endpoint
await listener.OpenAsync();
// Get a `HybridConnectionStream`
var hybridConnectionStream = await listener.AcceptConnectionAsync();
```

#### <a name="client"></a><span data-ttu-id="d9546-117">Klient</span><span class="sxs-lookup"><span data-stu-id="d9546-117">Client</span></span>
<span data-ttu-id="d9546-118">Použití [HybridConnectionClient][HCClient], můžete získat `HybridConnectionStream` objektu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d9546-118">Using a [HybridConnectionClient][HCClient], you can obtain a `HybridConnectionStream` object as follows:</span></span>

```csharp
// Use the RelayConnectionStringBuilder to get a valid connection string
var client = new HybridConnectionClient(csb.ToString());
// Open a connection to the Relay endpoint and get a `HybridConnectionStream`
var hybridConnectionStream = await client.CreateConnectionAsync();
```

### <a name="receiving-data"></a><span data-ttu-id="d9546-119">Přijetí dat</span><span class="sxs-lookup"><span data-stu-id="d9546-119">Receiving data</span></span>
<span data-ttu-id="d9546-120">[HybridConnectionStream] [ HCStream] třída umožňuje obousměrnou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="d9546-120">The [HybridConnectionStream][HCStream] class enables two-way communication.</span></span> <span data-ttu-id="d9546-121">Ve většině případů nepřetržitě obdržíte z datového proudu.</span><span class="sxs-lookup"><span data-stu-id="d9546-121">In most cases, you continuously receive from the stream.</span></span> <span data-ttu-id="d9546-122">Text při čtení z datového proudu, můžete také použít [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) objekt, který umožňuje jednodušší analýza dat.</span><span class="sxs-lookup"><span data-stu-id="d9546-122">If you are reading text from the stream, you may also want to use a [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) object, which enables easier parsing of the data.</span></span> <span data-ttu-id="d9546-123">Například můžete číst data jako text, nikoli jako `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="d9546-123">For example, you can read data as text, rather than as `byte[]`.</span></span>

<span data-ttu-id="d9546-124">Následující kód čte jednotlivé řádky textu z datového proudu, dokud se požaduje zrušení:</span><span class="sxs-lookup"><span data-stu-id="d9546-124">The following code reads individual lines of text from the stream until a cancellation is requested:</span></span>

```csharp
// Create a CancellationToken, so that we can cancel the while loop
var cancellationToken = new CancellationToken();
// Create a StreamReader from the 'hybridConnectionStream`
var streamReader = new StreamReader(hybridConnectionStream);

while (!cancellationToken.IsCancellationRequested)
{
    // Read a line of input until a newline is encountered
    var line = await streamReader.ReadLineAsync();
    if (string.IsNullOrEmpty(line))
    {
        // If there's no input data, we will signal that 
        // we will no longer send data on this connection
        // and then break out of the processing loop.
        await hybridConnectionStream.ShutdownAsync(cancellationToken);
        break;
    }
}
```

### <a name="sending-data"></a><span data-ttu-id="d9546-125">Odeslání dat</span><span class="sxs-lookup"><span data-stu-id="d9546-125">Sending data</span></span>
<span data-ttu-id="d9546-126">Jakmile máte připojení bylo vytvořeno, můžete odeslat zprávu na předávání přes koncový bod.</span><span class="sxs-lookup"><span data-stu-id="d9546-126">Once you have a connection established, you can send a message to the Relay endpoint.</span></span> <span data-ttu-id="d9546-127">Protože objekt připojení dědí [datového proudu](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), odesílat data podle `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="d9546-127">Because the connection object inherits [Stream](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), send your data as a `byte[]`.</span></span> <span data-ttu-id="d9546-128">Následující příklad ukazuje, jak to udělat:</span><span class="sxs-lookup"><span data-stu-id="d9546-128">The following example shows how to do this:</span></span>

```csharp
var data = Encoding.UTF8.GetBytes("hello");
await clientConnection.WriteAsync(data, 0, data.Length);
```

<span data-ttu-id="d9546-129">Ale pokud chcete odeslat text přímo, aniž by museli zakódujte řetězec pokaždé, když, můžete zabalit `hybridConnectionStream` objektu s [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) objektu.</span><span class="sxs-lookup"><span data-stu-id="d9546-129">However, if you want to send text directly, without needing to encode the string each time, you can wrap the `hybridConnectionStream` object with a [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) object.</span></span>

```csharp
// The StreamWriter object only needs to be created once
var textWriter = new StreamWriter(hybridConnectionStream);
await textWriter.WriteLineAsync("hello");
```

## <a name="next-steps"></a><span data-ttu-id="d9546-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9546-130">Next steps</span></span>
<span data-ttu-id="d9546-131">Další informace o předávání přes Azure, najdete pomocí těchto odkazů:</span><span class="sxs-lookup"><span data-stu-id="d9546-131">To learn more about Azure Relay, visit these links:</span></span>

* [<span data-ttu-id="d9546-132">Odkaz na Microsoft.Azure.Relay</span><span class="sxs-lookup"><span data-stu-id="d9546-132">Microsoft.Azure.Relay reference</span></span>](/dotnet/api/microsoft.azure.relay)
* [<span data-ttu-id="d9546-133">Co je Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="d9546-133">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="d9546-134">K dispozici předávání přes rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d9546-134">Available Relay APIs</span></span>](relay-api-overview.md)

[RelayConnectionStringBuilder]: /dotnet/api/microsoft.azure.relay.relayconnectionstringbuilder
[HCStream]: /dotnet/api/microsoft.azure.relay.hybridconnectionstream
[HCClient]: /dotnet/api/microsoft.azure.relay.hybridconnectionclient
[HCListener]: /dotnet/api/microsoft.azure.relay.hybridconnectionlistener