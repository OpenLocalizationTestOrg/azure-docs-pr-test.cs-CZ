---
title: "Přehled služby Azure API předávání | Microsoft Docs"
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
ms.openlocfilehash: 8d93a0344adc3b0b7617f3a7d85124142d7a7555
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="available-relay-apis"></a><span data-ttu-id="bf8e2-103">K dispozici předávání přes rozhraní API</span><span class="sxs-lookup"><span data-stu-id="bf8e2-103">Available Relay APIs</span></span>

## <a name="runtime-apis"></a><span data-ttu-id="bf8e2-104">Modul runtime rozhraní API</span><span class="sxs-lookup"><span data-stu-id="bf8e2-104">Runtime APIs</span></span>

<span data-ttu-id="bf8e2-105">Následující tabulka uvádí všechny klienty aktuálně k dispozici modul runtime předávání.</span><span class="sxs-lookup"><span data-stu-id="bf8e2-105">The following table lists all currently available Relay runtime clients.</span></span>

<span data-ttu-id="bf8e2-106">[Další informace o](#additional-information) část obsahuje informace o stavu jednotlivých modulu runtime knihoven.</span><span class="sxs-lookup"><span data-stu-id="bf8e2-106">The [additional information](#additional-information) section contains more information about the status of each runtime library.</span></span>

| <span data-ttu-id="bf8e2-107">Jazyk nebo platformu</span><span class="sxs-lookup"><span data-stu-id="bf8e2-107">Language/Platform</span></span> | <span data-ttu-id="bf8e2-108">Dostupné funkce</span><span class="sxs-lookup"><span data-stu-id="bf8e2-108">Available feature</span></span> | <span data-ttu-id="bf8e2-109">Balíček klienta</span><span class="sxs-lookup"><span data-stu-id="bf8e2-109">Client package</span></span> | <span data-ttu-id="bf8e2-110">Úložiště</span><span class="sxs-lookup"><span data-stu-id="bf8e2-110">Repository</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="bf8e2-111">Standardní rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="bf8e2-111">.NET Standard</span></span> | <span data-ttu-id="bf8e2-112">Hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="bf8e2-112">Hybrid Connections</span></span> | [<span data-ttu-id="bf8e2-113">Microsoft.Azure.Relay</span><span class="sxs-lookup"><span data-stu-id="bf8e2-113">Microsoft.Azure.Relay</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [<span data-ttu-id="bf8e2-114">GitHub</span><span class="sxs-lookup"><span data-stu-id="bf8e2-114">GitHub</span></span>](https://github.com/azure/azure-relay-dotnet) |
| <span data-ttu-id="bf8e2-115">Rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="bf8e2-115">.NET Framework</span></span> | <span data-ttu-id="bf8e2-116">WCF Relay</span><span class="sxs-lookup"><span data-stu-id="bf8e2-116">WCF Relay</span></span> | [<span data-ttu-id="bf8e2-117">WindowsAzure.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="bf8e2-117">WindowsAzure.ServiceBus</span></span>](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | <span data-ttu-id="bf8e2-118">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="bf8e2-118">N/A</span></span> |
| <span data-ttu-id="bf8e2-119">Node</span><span class="sxs-lookup"><span data-stu-id="bf8e2-119">Node</span></span> | <span data-ttu-id="bf8e2-120">Hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="bf8e2-120">Hybrid Connections</span></span> | [`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket) | [<span data-ttu-id="bf8e2-121">GitHub</span><span class="sxs-lookup"><span data-stu-id="bf8e2-121">GitHub</span></span>](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a><span data-ttu-id="bf8e2-122">Další informace</span><span class="sxs-lookup"><span data-stu-id="bf8e2-122">Additional information</span></span>

#### <a name="net"></a><span data-ttu-id="bf8e2-123">.NET</span><span class="sxs-lookup"><span data-stu-id="bf8e2-123">.NET</span></span>
<span data-ttu-id="bf8e2-124">Ekosystému .NET má více moduly runtime a proto nejsou více knihovny .NET pro centra událostí.</span><span class="sxs-lookup"><span data-stu-id="bf8e2-124">The .NET ecosystem has multiple runtimes, hence there are multiple .NET libraries for Event Hubs.</span></span> <span data-ttu-id="bf8e2-125">Knihovně .NET Standard můžete spustit pomocí .NET Core nebo rozhraní .NET Framework, když rozhraní .NET Framework – knihovna může spustit pouze v prostředí .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="bf8e2-125">The .NET Standard library can be run using either .NET Core or the .NET Framework, while the .NET Framework library can only be run in a .NET Framework environment.</span></span> <span data-ttu-id="bf8e2-126">Další informace o rozhraní .NET Framework naleznete v tématu [framework verze](/dotnet/articles/standard/frameworks#framework-versions).</span><span class="sxs-lookup"><span data-stu-id="bf8e2-126">For more information on .NET Frameworks, see [framework versions](/dotnet/articles/standard/frameworks#framework-versions).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf8e2-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf8e2-127">Next steps</span></span>
<span data-ttu-id="bf8e2-128">Další informace o předávání přes Azure, najdete pomocí těchto odkazů:</span><span class="sxs-lookup"><span data-stu-id="bf8e2-128">To learn more about Azure Relay, visit these links:</span></span>
* [<span data-ttu-id="bf8e2-129">Co je Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="bf8e2-129">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="bf8e2-130">Přenos – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="bf8e2-130">Relay FAQ</span></span>](relay-faq.md)