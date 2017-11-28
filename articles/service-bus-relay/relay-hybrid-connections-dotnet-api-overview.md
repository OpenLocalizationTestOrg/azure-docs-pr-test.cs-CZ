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
# <a name="azure-relay-hybrid-connections-net-standard-api-overview"></a><span data-ttu-id="f7f9d-103">Přehled služby Azure předávání hybridní připojení .NET standardní API</span><span class="sxs-lookup"><span data-stu-id="f7f9d-103">Azure Relay Hybrid Connections .NET Standard API overview</span></span>

<span data-ttu-id="f7f9d-104">Tento článek shrnuje některé klíče hello Azure předávání hybridní připojení .NET standardní [rozhraní API klienta](/dotnet/api/microsoft.azure.relay).</span><span class="sxs-lookup"><span data-stu-id="f7f9d-104">This article summarizes some of hello key Azure Relay Hybrid Connections .NET Standard [client APIs](/dotnet/api/microsoft.azure.relay).</span></span>
  
## <a name="relay-connection-string-builder"></a><span data-ttu-id="f7f9d-105">Předávání Tvůrce připojovacích řetězců</span><span class="sxs-lookup"><span data-stu-id="f7f9d-105">Relay Connection String Builder</span></span>

<span data-ttu-id="f7f9d-106">Hello [RelayConnectionStringBuilder] [ RelayConnectionStringBuilder] třída formáty připojovací řetězce, které jsou specifické tooRelay hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="f7f9d-106">hello [RelayConnectionStringBuilder][RelayConnectionStringBuilder] class formats connection strings that are specific tooRelay Hybrid Connections.</span></span> <span data-ttu-id="f7f9d-107">Můžete ji pomocí formátu hello tooverify připojovací řetězec, nebo toobuild připojovací řetězec od začátku.</span><span class="sxs-lookup"><span data-stu-id="f7f9d-107">You can use it tooverify hello format of a connection string, or toobuild a connection string from scratch.</span></span> <span data-ttu-id="f7f9d-108">Viz následující příklad kódu hello:</span><span class="sxs-lookup"><span data-stu-id="f7f9d-108">See hello following code for an example:</span></span>

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

<span data-ttu-id="f7f9d-109">Můžete také předat připojení přímo řetězec toohello `RelayConnectionStringBuilder` metoda.</span><span class="sxs-lookup"><span data-stu-id="f7f9d-109">You can also pass a connection string directly toohello `RelayConnectionStringBuilder` method.</span></span> <span data-ttu-id="f7f9d-110">Tato operace povolí tooverify, který je hello připojovací řetězec v platném formátu.</span><span class="sxs-lookup"><span data-stu-id="f7f9d-110">This operation enables you tooverify that hello connection string is in a valid format.</span></span> <span data-ttu-id="f7f9d-111">Pokud některý z parametrů hello jsou neplatné, generuje konstruktor hello `ArgumentException`.</span><span class="sxs-lookup"><span data-stu-id="f7f9d-111">If any of hello parameters are invalid, hello constructor generates an `ArgumentException`.</span></span>

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

## <a name="hybrid-connection-stream"></a><span data-ttu-id="f7f9d-112">Hybridní připojení datového proudu</span><span class="sxs-lookup"><span data-stu-id="f7f9d-112">Hybrid Connection Stream</span></span>
<span data-ttu-id="f7f9d-113">Hello [HybridConnectionStream] [ HCStream] třída je hello primární objekt použitý toosend a přijímat data z Azure předávání koncový bod, zda pracujete [HybridConnectionClient] [ HCClient], nebo [HybridConnectionListener][HCListener].</span><span class="sxs-lookup"><span data-stu-id="f7f9d-113">hello [HybridConnectionStream][HCStream] class is hello primary object used toosend and receive data from an Azure Relay endpoint, whether you are working with a [HybridConnectionClient][HCClient], or a [HybridConnectionListener][HCListener].</span></span>

### <a name="getting-a-hybrid-connection-stream"></a><span data-ttu-id="f7f9d-114">Získávání datový proud s hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="f7f9d-114">Getting a Hybrid Connection Stream</span></span>

#### <a name="listener"></a><span data-ttu-id="f7f9d-115">Naslouchací proces</span><span class="sxs-lookup"><span data-stu-id="f7f9d-115">Listener</span></span>
<span data-ttu-id="f7f9d-116">Použití [HybridConnectionListener][HCListener], můžete získat `HybridConnectionStream` objektu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f7f9d-116">Using a [HybridConnectionListener][HCListener], you can obtain a `HybridConnectionStream` object as follows:</span></span>

```csharp
// Use hello RelayConnectionStringBuilder tooget a valid connection string
var listener = new HybridConnectionListener(csb.ToString());
// Open a connection toohello Relay endpoint
await listener.OpenAsync();
// Get a `HybridConnectionStream`
var hybridConnectionStream = await listener.AcceptConnectionAsync();
```

#### <a name="client"></a><span data-ttu-id="f7f9d-117">Klient</span><span class="sxs-lookup"><span data-stu-id="f7f9d-117">Client</span></span>
<span data-ttu-id="f7f9d-118">Použití [HybridConnectionClient][HCClient], můžete získat `HybridConnectionStream` objektu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f7f9d-118">Using a [HybridConnectionClient][HCClient], you can obtain a `HybridConnectionStream` object as follows:</span></span>

```csharp
// Use hello RelayConnectionStringBuilder tooget a valid connection string
var client = new HybridConnectionClient(csb.ToString());
// Open a connection toohello Relay endpoint and get a `HybridConnectionStream`
var hybridConnectionStream = await client.CreateConnectionAsync();
```

### <a name="receiving-data"></a><span data-ttu-id="f7f9d-119">Přijetí dat</span><span class="sxs-lookup"><span data-stu-id="f7f9d-119">Receiving data</span></span>
<span data-ttu-id="f7f9d-120">Hello [HybridConnectionStream] [ HCStream] třída umožňuje obousměrnou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="f7f9d-120">hello [HybridConnectionStream][HCStream] class enables two-way communication.</span></span> <span data-ttu-id="f7f9d-121">Ve většině případů nepřetržitě obdržíte z datového proudu hello.</span><span class="sxs-lookup"><span data-stu-id="f7f9d-121">In most cases, you continuously receive from hello stream.</span></span> <span data-ttu-id="f7f9d-122">Při čtení textu ze streamu hello, může být také vhodné toouse [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) objekt, který umožňuje jednodušší analýza dat hello.</span><span class="sxs-lookup"><span data-stu-id="f7f9d-122">If you are reading text from hello stream, you may also want toouse a [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) object, which enables easier parsing of hello data.</span></span> <span data-ttu-id="f7f9d-123">Například můžete číst data jako text, nikoli jako `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="f7f9d-123">For example, you can read data as text, rather than as `byte[]`.</span></span>

<span data-ttu-id="f7f9d-124">Hello následující kód čte jednotlivé řádky textu z datového proudu hello až zrušení je požadováno:</span><span class="sxs-lookup"><span data-stu-id="f7f9d-124">hello following code reads individual lines of text from hello stream until a cancellation is requested:</span></span>

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

### <a name="sending-data"></a><span data-ttu-id="f7f9d-125">Odeslání dat</span><span class="sxs-lookup"><span data-stu-id="f7f9d-125">Sending data</span></span>
<span data-ttu-id="f7f9d-126">Jakmile máte připojení bylo vytvořeno, můžete odeslat koncový bod zpráva toohello předávání.</span><span class="sxs-lookup"><span data-stu-id="f7f9d-126">Once you have a connection established, you can send a message toohello Relay endpoint.</span></span> <span data-ttu-id="f7f9d-127">Protože objekt připojení hello dědí [datového proudu](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), odesílat data podle `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="f7f9d-127">Because hello connection object inherits [Stream](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), send your data as a `byte[]`.</span></span> <span data-ttu-id="f7f9d-128">Následující příklad ukazuje, jak Hello toodo toto:</span><span class="sxs-lookup"><span data-stu-id="f7f9d-128">hello following example shows how toodo this:</span></span>

```csharp
var data = Encoding.UTF8.GetBytes("hello");
await clientConnection.WriteAsync(data, 0, data.Length);
```

<span data-ttu-id="f7f9d-129">Pokud chcete toosend text přímo, bez nutnosti tooencode hello řetězec pokaždé, když, můžete však zabalit hello `hybridConnectionStream` objektu s [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) objektu.</span><span class="sxs-lookup"><span data-stu-id="f7f9d-129">However, if you want toosend text directly, without needing tooencode hello string each time, you can wrap hello `hybridConnectionStream` object with a [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) object.</span></span>

```csharp
// hello StreamWriter object only needs toobe created once
var textWriter = new StreamWriter(hybridConnectionStream);
await textWriter.WriteLineAsync("hello");
```

## <a name="next-steps"></a><span data-ttu-id="f7f9d-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f7f9d-130">Next steps</span></span>
<span data-ttu-id="f7f9d-131">Další informace o předávání přes Azure, toolearn naleznete pod těmito odkazy:</span><span class="sxs-lookup"><span data-stu-id="f7f9d-131">toolearn more about Azure Relay, visit these links:</span></span>

* [<span data-ttu-id="f7f9d-132">Odkaz na Microsoft.Azure.Relay</span><span class="sxs-lookup"><span data-stu-id="f7f9d-132">Microsoft.Azure.Relay reference</span></span>](/dotnet/api/microsoft.azure.relay)
* [<span data-ttu-id="f7f9d-133">Co je Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="f7f9d-133">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="f7f9d-134">K dispozici předávání přes rozhraní API</span><span class="sxs-lookup"><span data-stu-id="f7f9d-134">Available Relay APIs</span></span>](relay-api-overview.md)

[RelayConnectionStringBuilder]: /dotnet/api/microsoft.azure.relay.relayconnectionstringbuilder
[HCStream]: /dotnet/api/microsoft.azure.relay.hybridconnectionstream
[HCClient]: /dotnet/api/microsoft.azure.relay.hybridconnectionclient
[HCListener]: /dotnet/api/microsoft.azure.relay.hybridconnectionlistener