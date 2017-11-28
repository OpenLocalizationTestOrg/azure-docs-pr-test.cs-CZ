---
title: "Přehled rozhraní API centra událostí aaaAzure | Microsoft Docs"
description: "Přehled rozhraní API centra událostí Azure k dispozici"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3f221a0c-182d-4e39-9f3d-3a3c16c5c6ed
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 46dfcc544ff92642cfd7a967f9ec38a0d8e2bd5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="available-event-hubs-apis"></a><span data-ttu-id="3f527-103">Rozhraní API k dispozici události rozbočovače</span><span class="sxs-lookup"><span data-stu-id="3f527-103">Available Event Hubs APIs</span></span>

## <a name="runtime-apis"></a><span data-ttu-id="3f527-104">Modul runtime rozhraní API</span><span class="sxs-lookup"><span data-stu-id="3f527-104">Runtime APIs</span></span>

<span data-ttu-id="3f527-105">Hello následuje popis všech klientů aktuálně k dispozici modul runtime Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="3f527-105">hello following is a description of all currently available Azure Event Hubs runtime clients.</span></span> <span data-ttu-id="3f527-106">I když některé z těchto knihoven, také mají omezenou správu funkci, existují také [specifické knihovny](#management-apis) vyhrazené toomanagement operace.</span><span class="sxs-lookup"><span data-stu-id="3f527-106">While some of these libraries also include limited management functionality, there are also [specific libraries](#management-apis) dedicated toomanagement operations.</span></span> <span data-ttu-id="3f527-107">fokus základní Hello tyto knihovny je toosend a přijímat zprávy z centra událostí.</span><span class="sxs-lookup"><span data-stu-id="3f527-107">hello core focus of these libraries is toosend and receive messages from an event hub.</span></span>

<span data-ttu-id="3f527-108">V tématu [Další informace o](#additional-information) další podrobnosti o hello aktuální stav jednotlivých modulu runtime knihoven.</span><span class="sxs-lookup"><span data-stu-id="3f527-108">See [additional information](#additional-information) for more details on hello current status of each runtime library.</span></span>

| <span data-ttu-id="3f527-109">Jazyk nebo platformu</span><span class="sxs-lookup"><span data-stu-id="3f527-109">Language/Platform</span></span> | <span data-ttu-id="3f527-110">Balíček klienta</span><span class="sxs-lookup"><span data-stu-id="3f527-110">Client package</span></span> | <span data-ttu-id="3f527-111">Balíček EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="3f527-111">EventProcessorHost package</span></span> | <span data-ttu-id="3f527-112">Úložiště</span><span class="sxs-lookup"><span data-stu-id="3f527-112">Repository</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3f527-113">Standardní rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="3f527-113">.NET Standard</span></span> | [<span data-ttu-id="3f527-114">NuGet</span><span class="sxs-lookup"><span data-stu-id="3f527-114">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) | [<span data-ttu-id="3f527-115">NuGet</span><span class="sxs-lookup"><span data-stu-id="3f527-115">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) | [<span data-ttu-id="3f527-116">GitHub</span><span class="sxs-lookup"><span data-stu-id="3f527-116">GitHub</span></span>](https://github.com/azure/azure-event-hubs-dotnet) |
| <span data-ttu-id="3f527-117">Rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="3f527-117">.NET Framework</span></span> | [<span data-ttu-id="3f527-118">NuGet</span><span class="sxs-lookup"><span data-stu-id="3f527-118">NuGet</span></span>](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | [<span data-ttu-id="3f527-119">NuGet</span><span class="sxs-lookup"><span data-stu-id="3f527-119">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) | <span data-ttu-id="3f527-120">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3f527-120">N/A</span></span> |
| <span data-ttu-id="3f527-121">Java</span><span class="sxs-lookup"><span data-stu-id="3f527-121">Java</span></span> | [<span data-ttu-id="3f527-122">Maven</span><span class="sxs-lookup"><span data-stu-id="3f527-122">Maven</span></span>](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) | [<span data-ttu-id="3f527-123">Maven</span><span class="sxs-lookup"><span data-stu-id="3f527-123">Maven</span></span>](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22) | [<span data-ttu-id="3f527-124">GitHub</span><span class="sxs-lookup"><span data-stu-id="3f527-124">GitHub</span></span>](https://github.com/Azure/azure-event-hubs-java) |
| <span data-ttu-id="3f527-125">Node</span><span class="sxs-lookup"><span data-stu-id="3f527-125">Node</span></span> | [<span data-ttu-id="3f527-126">NPM</span><span class="sxs-lookup"><span data-stu-id="3f527-126">NPM</span></span>](https://www.npmjs.com/package/azure-event-hubs) | <span data-ttu-id="3f527-127">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3f527-127">N/A</span></span> | [<span data-ttu-id="3f527-128">GitHub</span><span class="sxs-lookup"><span data-stu-id="3f527-128">GitHub</span></span>](https://github.com/Azure/azure-event-hubs-node) |
| <span data-ttu-id="3f527-129">C</span><span class="sxs-lookup"><span data-stu-id="3f527-129">C</span></span> | <span data-ttu-id="3f527-130">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3f527-130">N/A</span></span> | <span data-ttu-id="3f527-131">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3f527-131">N/A</span></span> | [<span data-ttu-id="3f527-132">GitHub</span><span class="sxs-lookup"><span data-stu-id="3f527-132">GitHub</span></span>](https://github.com/Azure/azure-event-hubs-c) |

### <a name="additional-information"></a><span data-ttu-id="3f527-133">Další informace</span><span class="sxs-lookup"><span data-stu-id="3f527-133">Additional information</span></span>

#### <a name="net"></a><span data-ttu-id="3f527-134">.NET</span><span class="sxs-lookup"><span data-stu-id="3f527-134">.NET</span></span>
<span data-ttu-id="3f527-135">Hello .NET ekosystém má více moduly runtime a proto nejsou více knihovny .NET pro centra událostí.</span><span class="sxs-lookup"><span data-stu-id="3f527-135">hello .NET ecosystem has multiple runtimes, hence there are multiple .NET libraries for Event Hubs.</span></span> <span data-ttu-id="3f527-136">knihovny .NET standardní Hello lze spouštět s využitím .NET Core nebo hello rozhraní .NET Framework, zatímco hello rozhraní .NET Framework – knihovna může spustit pouze v prostředí .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3f527-136">hello .NET Standard library can be run using either .NET Core or hello .NET Framework, while hello .NET Framework library can only be run in a .NET Framework environment.</span></span> <span data-ttu-id="3f527-137">Další informace o rozhraní .NET Framework naleznete v tématu [framework verze](https://docs.microsoft.com/dotnet/articles/standard/frameworks#framework-versions).</span><span class="sxs-lookup"><span data-stu-id="3f527-137">For more information on .NET Frameworks, see [framework versions](https://docs.microsoft.com/dotnet/articles/standard/frameworks#framework-versions).</span></span>

#### <a name="node"></a><span data-ttu-id="3f527-138">Node</span><span class="sxs-lookup"><span data-stu-id="3f527-138">Node</span></span>

<span data-ttu-id="3f527-139">Knihovna Node.js Hello je aktuálně ve verzi preview a je udržován jako straně projekt tak, že zaměstnanci společnosti Microsoft a externí přispěvatele.</span><span class="sxs-lookup"><span data-stu-id="3f527-139">hello Node.js library is currently in preview and is maintained as a side project by Microsoft employees and external contributors.</span></span> <span data-ttu-id="3f527-140">Všechny příspěvky včetně zdrojového kódu se úvodní a bude znovu.</span><span class="sxs-lookup"><span data-stu-id="3f527-140">All contributions including source code are welcome and will be reviewed.</span></span>

## <a name="management-apis"></a><span data-ttu-id="3f527-141">Rozhraní API pro správu</span><span class="sxs-lookup"><span data-stu-id="3f527-141">Management APIs</span></span>

<span data-ttu-id="3f527-142">Hello následuje seznam všech aktuálně k dispozici správa specifické knihovny.</span><span class="sxs-lookup"><span data-stu-id="3f527-142">hello following is a listing of all currently available management specific libraries.</span></span> <span data-ttu-id="3f527-143">Žádná z těchto knihoven obsahovat operace runtime a jsou k jedinému účelu hello Správa entit služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="3f527-143">None of these libraries contain runtime operations, and are for hello sole purpose of managing Event Hubs entities.</span></span>

| <span data-ttu-id="3f527-144">Jazyk nebo platformu</span><span class="sxs-lookup"><span data-stu-id="3f527-144">Language/Platform</span></span> | <span data-ttu-id="3f527-145">Balíček pro správu</span><span class="sxs-lookup"><span data-stu-id="3f527-145">Management package</span></span> | <span data-ttu-id="3f527-146">Úložiště</span><span class="sxs-lookup"><span data-stu-id="3f527-146">Repository</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3f527-147">Standardní rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="3f527-147">.NET Standard</span></span> | [<span data-ttu-id="3f527-148">NuGet</span><span class="sxs-lookup"><span data-stu-id="3f527-148">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.EventHub) | [<span data-ttu-id="3f527-149">GitHub</span><span class="sxs-lookup"><span data-stu-id="3f527-149">GitHub</span></span>](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/ResourceManagement/EventHub) |

## <a name="next-steps"></a><span data-ttu-id="3f527-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3f527-150">Next steps</span></span>
<span data-ttu-id="3f527-151">Další informace o službě Event Hubs návštěvou hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="3f527-151">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="3f527-152">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="3f527-152">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="3f527-153">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="3f527-153">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="3f527-154">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="3f527-154">Event Hubs FAQ</span></span>](event-hubs-faq.md)