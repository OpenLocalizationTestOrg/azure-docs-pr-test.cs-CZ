---
title: "Změnit Azure DocumentDB .NET SDK informačního kanálu procesoru a prostředky | Microsoft Docs"
description: "Další informace o rozhraní API pro změnu kanálu procesoru a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi sadu DocumentDB .NET změnu kanálu procesoru SDK."
services: cosmos-db
documentationcenter: .net
author: ealsur
manager: kirillg
editor: mimig1
ms.assetid: f2dd9438-8879-4f74-bb6c-e1efc2cd0157
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/14/2017
ms.author: maquaran
ms.openlocfilehash: 40c796bc5af1220c46950a6fac062ffdd243e59f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="documentdb-net-change-feed-processor-sdk-download-and-release-notes"></a><span data-ttu-id="fd74d-103">DocumentDB .NET změnu kanálu procesor SDK: Stažení a poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="fd74d-103">DocumentDB .NET Change Feed Processor SDK: Download and release notes</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fd74d-104">.NET</span><span class="sxs-lookup"><span data-stu-id="fd74d-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="fd74d-105">Informační kanál změnu rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="fd74d-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="fd74d-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd74d-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="fd74d-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="fd74d-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="fd74d-108">Java</span><span class="sxs-lookup"><span data-stu-id="fd74d-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="fd74d-109">Python</span><span class="sxs-lookup"><span data-stu-id="fd74d-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="fd74d-110">REST</span><span class="sxs-lookup"><span data-stu-id="fd74d-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="fd74d-111">Poskytovatel prostředků REST</span><span class="sxs-lookup"><span data-stu-id="fd74d-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="fd74d-112">SQL</span><span class="sxs-lookup"><span data-stu-id="fd74d-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="fd74d-113">**Stažení sady SDK**</span><span class="sxs-lookup"><span data-stu-id="fd74d-113">**SDK download**</span></span></td><td>[<span data-ttu-id="fd74d-114">NuGet</span><span class="sxs-lookup"><span data-stu-id="fd74d-114">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)</td></tr>

<tr><td><span data-ttu-id="fd74d-115">**Dokumentaci k rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="fd74d-115">**API documentation**</span></span></td><td>[<span data-ttu-id="fd74d-116">Změnit referenční dokumentace rozhraní API knihovny kanálu procesoru</span><span class="sxs-lookup"><span data-stu-id="fd74d-116">Change Feed Processor library API reference documentation</span></span>](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)</td></tr>

<tr><td><span data-ttu-id="fd74d-117">**Začínáme**</span><span class="sxs-lookup"><span data-stu-id="fd74d-117">**Get started**</span></span></td><td>[<span data-ttu-id="fd74d-118">Začínáme s DocumentDB změnu kanálu procesoru .NET SDK</span><span class="sxs-lookup"><span data-stu-id="fd74d-118">Get started with the DocumentDB Change Feed Processor .NET SDK</span></span>](change-feed.md)</td></tr>

<tr><td><span data-ttu-id="fd74d-119">**Aktuální podporovaných prostředí**</span><span class="sxs-lookup"><span data-stu-id="fd74d-119">**Current supported framework**</span></span></td><td>[<span data-ttu-id="fd74d-120">Microsoft .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="fd74d-120">Microsoft .NET Framework 4.5</span></span>](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="fd74d-121">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="fd74d-121">Release notes</span></span>

### <a name="a-name110110"></a><span data-ttu-id="fd74d-122"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="fd74d-122"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="fd74d-123">Přidat metodu k získání odhad zbývající práce mají být zpracovány v kanálu změnu.</span><span class="sxs-lookup"><span data-stu-id="fd74d-123">Added a method to obtain an estimate of remaining work to be processed in the Change Feed.</span></span>
* <span data-ttu-id="fd74d-124">Kompatibilní s [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) verze 1.13.2 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="fd74d-124">Compatible with [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) versions 1.13.2 and above.</span></span>

### <a name="a-name100100"></a><span data-ttu-id="fd74d-125"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="fd74d-125"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="fd74d-126">GA SDK</span><span class="sxs-lookup"><span data-stu-id="fd74d-126">GA SDK</span></span>
* <span data-ttu-id="fd74d-127">Kompatibilní s [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) verze 1.14.1 a níže.</span><span class="sxs-lookup"><span data-stu-id="fd74d-127">Compatible with [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) versions 1.14.1 and below.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="fd74d-128">Verze & vyřazení kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="fd74d-128">Release & Retirement dates</span></span>
<span data-ttu-id="fd74d-129">Microsoft bude poskytovat oznámení alespoň **dobu 12 měsíců** předem vyřazení sady SDK k funkce smooth přechodu na novější nebo podporované verzi.</span><span class="sxs-lookup"><span data-stu-id="fd74d-129">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order to smooth the transition to a newer/supported version.</span></span>

<span data-ttu-id="fd74d-130">Nové funkce a funkce a optimalizace, jsou přidány pouze v aktuální sadě SDK, jako takový se doporučuje, aby vždy upgradu na nejnovější verze sady SDK v míře.</span><span class="sxs-lookup"><span data-stu-id="fd74d-130">New features and functionality and optimizations are only added to the current SDK, as such it is recommended that you always upgrade to the latest SDK version as early as possible.</span></span> 

<span data-ttu-id="fd74d-131">Každá žádost o DB Cosmos pomocí vyřazeno sady SDK budou odmítnuty službou.</span><span class="sxs-lookup"><span data-stu-id="fd74d-131">Any request to Cosmos DB using a retired SDK will be rejected by the service.</span></span>

<br/>

| <span data-ttu-id="fd74d-132">Verze</span><span class="sxs-lookup"><span data-stu-id="fd74d-132">Version</span></span> | <span data-ttu-id="fd74d-133">Datum vydání</span><span class="sxs-lookup"><span data-stu-id="fd74d-133">Release Date</span></span> | <span data-ttu-id="fd74d-134">Datum vyřazení</span><span class="sxs-lookup"><span data-stu-id="fd74d-134">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="fd74d-135">1.1.0</span><span class="sxs-lookup"><span data-stu-id="fd74d-135">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="fd74d-136">13 srpen 2017</span><span class="sxs-lookup"><span data-stu-id="fd74d-136">August 13, 2017</span></span> |--- |
| [<span data-ttu-id="fd74d-137">1.0.0</span><span class="sxs-lookup"><span data-stu-id="fd74d-137">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="fd74d-138">07 července 2017</span><span class="sxs-lookup"><span data-stu-id="fd74d-138">July 07, 2017</span></span> |--- |


## <a name="faq"></a><span data-ttu-id="fd74d-139">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="fd74d-139">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="fd74d-140">Viz také</span><span class="sxs-lookup"><span data-stu-id="fd74d-140">See also</span></span>
<span data-ttu-id="fd74d-141">Další informace o Cosmos DB najdete v tématu [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stránku služby.</span><span class="sxs-lookup"><span data-stu-id="fd74d-141">To learn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span> 

