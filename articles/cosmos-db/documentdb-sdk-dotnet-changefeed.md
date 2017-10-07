---
title: "aaaAzure DocumentDB .NET změnu kanálu procesoru SDK & prostředky | Microsoft Docs"
description: "Zjistěte všechno o hello změnu kanálu procesoru rozhraní API a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi hello DocumentDB .NET změnu kanálu procesoru SDK."
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
ms.openlocfilehash: 7c001cc77f41c01445fb53328e9d99fd3d312c58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="documentdb-net-change-feed-processor-sdk-download-and-release-notes"></a><span data-ttu-id="2ea41-103">DocumentDB .NET změnu kanálu procesor SDK: Stažení a poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="2ea41-103">DocumentDB .NET Change Feed Processor SDK: Download and release notes</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2ea41-104">.NET</span><span class="sxs-lookup"><span data-stu-id="2ea41-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="2ea41-105">Informační kanál změnu rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="2ea41-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="2ea41-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ea41-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="2ea41-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="2ea41-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="2ea41-108">Java</span><span class="sxs-lookup"><span data-stu-id="2ea41-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="2ea41-109">Python</span><span class="sxs-lookup"><span data-stu-id="2ea41-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="2ea41-110">REST</span><span class="sxs-lookup"><span data-stu-id="2ea41-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="2ea41-111">Poskytovatel prostředků REST</span><span class="sxs-lookup"><span data-stu-id="2ea41-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="2ea41-112">SQL</span><span class="sxs-lookup"><span data-stu-id="2ea41-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="2ea41-113">**Stažení sady SDK**</span><span class="sxs-lookup"><span data-stu-id="2ea41-113">**SDK download**</span></span></td><td>[<span data-ttu-id="2ea41-114">NuGet</span><span class="sxs-lookup"><span data-stu-id="2ea41-114">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)</td></tr>

<tr><td><span data-ttu-id="2ea41-115">**Dokumentaci k rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="2ea41-115">**API documentation**</span></span></td><td>[<span data-ttu-id="2ea41-116">Změnit referenční dokumentace rozhraní API knihovny kanálu procesoru</span><span class="sxs-lookup"><span data-stu-id="2ea41-116">Change Feed Processor library API reference documentation</span></span>](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)</td></tr>

<tr><td><span data-ttu-id="2ea41-117">**Začínáme**</span><span class="sxs-lookup"><span data-stu-id="2ea41-117">**Get started**</span></span></td><td>[<span data-ttu-id="2ea41-118">Začínáme s hello DocumentDB změnu kanálu procesoru .NET SDK</span><span class="sxs-lookup"><span data-stu-id="2ea41-118">Get started with hello DocumentDB Change Feed Processor .NET SDK</span></span>](change-feed.md)</td></tr>

<tr><td><span data-ttu-id="2ea41-119">**Aktuální podporovaných prostředí**</span><span class="sxs-lookup"><span data-stu-id="2ea41-119">**Current supported framework**</span></span></td><td>[<span data-ttu-id="2ea41-120">Microsoft .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="2ea41-120">Microsoft .NET Framework 4.5</span></span>](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="2ea41-121">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="2ea41-121">Release notes</span></span>

### <a name="a-name110110"></a><span data-ttu-id="2ea41-122"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="2ea41-122"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="2ea41-123">Přidat metoda tooobtain odhad zbývající práce toobe zpracovány v hello změnu kanálu.</span><span class="sxs-lookup"><span data-stu-id="2ea41-123">Added a method tooobtain an estimate of remaining work toobe processed in hello Change Feed.</span></span>
* <span data-ttu-id="2ea41-124">Kompatibilní s [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) verze 1.13.2 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="2ea41-124">Compatible with [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) versions 1.13.2 and above.</span></span>

### <a name="a-name100100"></a><span data-ttu-id="2ea41-125"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="2ea41-125"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="2ea41-126">GA SDK</span><span class="sxs-lookup"><span data-stu-id="2ea41-126">GA SDK</span></span>
* <span data-ttu-id="2ea41-127">Kompatibilní s [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) verze 1.14.1 a níže.</span><span class="sxs-lookup"><span data-stu-id="2ea41-127">Compatible with [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) versions 1.14.1 and below.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="2ea41-128">Verze & vyřazení kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="2ea41-128">Release & Retirement dates</span></span>
<span data-ttu-id="2ea41-129">Microsoft bude poskytovat oznámení alespoň **dobu 12 měsíců** před vyřazením z provozu v pořadí toosmooth hello přechod tooa novější nebo v nepodporované verzi sady SDK.</span><span class="sxs-lookup"><span data-stu-id="2ea41-129">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order toosmooth hello transition tooa newer/supported version.</span></span>

<span data-ttu-id="2ea41-130">Nové funkce a funkce a optimalizace, jsou přidány pouze aktuální toohello SDK, jako například se doporučuje tento můžete vždy upgradu toohello nejnovější verze sady SDK co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="2ea41-130">New features and functionality and optimizations are only added toohello current SDK, as such it is recommended that you always upgrade toohello latest SDK version as early as possible.</span></span> 

<span data-ttu-id="2ea41-131">Všechny žádosti o tooCosmos DB pomocí vyřazeno sady SDK budou odmítnuty službou hello.</span><span class="sxs-lookup"><span data-stu-id="2ea41-131">Any request tooCosmos DB using a retired SDK will be rejected by hello service.</span></span>

<br/>

| <span data-ttu-id="2ea41-132">Verze</span><span class="sxs-lookup"><span data-stu-id="2ea41-132">Version</span></span> | <span data-ttu-id="2ea41-133">Datum vydání</span><span class="sxs-lookup"><span data-stu-id="2ea41-133">Release Date</span></span> | <span data-ttu-id="2ea41-134">Datum vyřazení</span><span class="sxs-lookup"><span data-stu-id="2ea41-134">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="2ea41-135">1.1.0</span><span class="sxs-lookup"><span data-stu-id="2ea41-135">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="2ea41-136">13 srpen 2017</span><span class="sxs-lookup"><span data-stu-id="2ea41-136">August 13, 2017</span></span> |--- |
| [<span data-ttu-id="2ea41-137">1.0.0</span><span class="sxs-lookup"><span data-stu-id="2ea41-137">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="2ea41-138">07 července 2017</span><span class="sxs-lookup"><span data-stu-id="2ea41-138">July 07, 2017</span></span> |--- |


## <a name="faq"></a><span data-ttu-id="2ea41-139">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="2ea41-139">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="2ea41-140">Viz také</span><span class="sxs-lookup"><span data-stu-id="2ea41-140">See also</span></span>
<span data-ttu-id="2ea41-141">toolearn Další informace o Cosmos databáze, najdete v části [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stránku služby.</span><span class="sxs-lookup"><span data-stu-id="2ea41-141">toolearn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span> 

