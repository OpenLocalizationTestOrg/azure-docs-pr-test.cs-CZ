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
# <a name="available-relay-apis"></a><span data-ttu-id="c8ef9-103">K dispozici předávání přes rozhraní API</span><span class="sxs-lookup"><span data-stu-id="c8ef9-103">Available Relay APIs</span></span>

## <a name="runtime-apis"></a><span data-ttu-id="c8ef9-104">Modul runtime rozhraní API</span><span class="sxs-lookup"><span data-stu-id="c8ef9-104">Runtime APIs</span></span>

<span data-ttu-id="c8ef9-105">Hello následující tabulka uvádí všechny klienty aktuálně k dispozici modul runtime předávání.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-105">hello following table lists all currently available Relay runtime clients.</span></span>

<span data-ttu-id="c8ef9-106">Hello [Další informace o](#additional-information) část obsahuje další informace o stavu hello jednotlivých modulu runtime knihoven.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-106">hello [additional information](#additional-information) section contains more information about hello status of each runtime library.</span></span>

| <span data-ttu-id="c8ef9-107">Jazyk nebo platformu</span><span class="sxs-lookup"><span data-stu-id="c8ef9-107">Language/Platform</span></span> | <span data-ttu-id="c8ef9-108">Dostupné funkce</span><span class="sxs-lookup"><span data-stu-id="c8ef9-108">Available feature</span></span> | <span data-ttu-id="c8ef9-109">Balíček klienta</span><span class="sxs-lookup"><span data-stu-id="c8ef9-109">Client package</span></span> | <span data-ttu-id="c8ef9-110">Úložiště</span><span class="sxs-lookup"><span data-stu-id="c8ef9-110">Repository</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c8ef9-111">Standardní rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="c8ef9-111">.NET Standard</span></span> | <span data-ttu-id="c8ef9-112">Hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="c8ef9-112">Hybrid Connections</span></span> | [<span data-ttu-id="c8ef9-113">Microsoft.Azure.Relay</span><span class="sxs-lookup"><span data-stu-id="c8ef9-113">Microsoft.Azure.Relay</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [<span data-ttu-id="c8ef9-114">GitHub</span><span class="sxs-lookup"><span data-stu-id="c8ef9-114">GitHub</span></span>](https://github.com/azure/azure-relay-dotnet) |
| <span data-ttu-id="c8ef9-115">Rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="c8ef9-115">.NET Framework</span></span> | <span data-ttu-id="c8ef9-116">WCF Relay</span><span class="sxs-lookup"><span data-stu-id="c8ef9-116">WCF Relay</span></span> | [<span data-ttu-id="c8ef9-117">WindowsAzure.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="c8ef9-117">WindowsAzure.ServiceBus</span></span>](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | <span data-ttu-id="c8ef9-118">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c8ef9-118">N/A</span></span> |
| <span data-ttu-id="c8ef9-119">Node</span><span class="sxs-lookup"><span data-stu-id="c8ef9-119">Node</span></span> | <span data-ttu-id="c8ef9-120">Hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="c8ef9-120">Hybrid Connections</span></span> | [`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket) | [<span data-ttu-id="c8ef9-121">GitHub</span><span class="sxs-lookup"><span data-stu-id="c8ef9-121">GitHub</span></span>](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a><span data-ttu-id="c8ef9-122">Další informace</span><span class="sxs-lookup"><span data-stu-id="c8ef9-122">Additional information</span></span>

#### <a name="net"></a><span data-ttu-id="c8ef9-123">.NET</span><span class="sxs-lookup"><span data-stu-id="c8ef9-123">.NET</span></span>
<span data-ttu-id="c8ef9-124">Hello .NET ekosystém má více moduly runtime a proto nejsou více knihovny .NET pro centra událostí.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-124">hello .NET ecosystem has multiple runtimes, hence there are multiple .NET libraries for Event Hubs.</span></span> <span data-ttu-id="c8ef9-125">knihovny .NET standardní Hello lze spouštět s využitím .NET Core nebo hello rozhraní .NET Framework, zatímco hello rozhraní .NET Framework – knihovna může spustit pouze v prostředí .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-125">hello .NET Standard library can be run using either .NET Core or hello .NET Framework, while hello .NET Framework library can only be run in a .NET Framework environment.</span></span> <span data-ttu-id="c8ef9-126">Další informace o rozhraní .NET Framework naleznete v tématu [framework verze](/dotnet/articles/standard/frameworks#framework-versions).</span><span class="sxs-lookup"><span data-stu-id="c8ef9-126">For more information on .NET Frameworks, see [framework versions](/dotnet/articles/standard/frameworks#framework-versions).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8ef9-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c8ef9-127">Next steps</span></span>
<span data-ttu-id="c8ef9-128">Další informace o předávání přes Azure, toolearn naleznete pod těmito odkazy:</span><span class="sxs-lookup"><span data-stu-id="c8ef9-128">toolearn more about Azure Relay, visit these links:</span></span>
* [<span data-ttu-id="c8ef9-129">Co je Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="c8ef9-129">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="c8ef9-130">Přenos – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="c8ef9-130">Relay FAQ</span></span>](relay-faq.md)