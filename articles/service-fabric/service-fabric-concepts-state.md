---
title: "Definine a spravovat stav v Azure mikroslužeb | Microsoft Docs"
description: "Tom, jak definovat a spravovat stav služby v Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: f5e618a5-3ea3-4404-94af-122278f91652
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 2f7835fab175bdb7ef7ce3894cab9e09a39457f3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="service-state"></a><span data-ttu-id="146b3-103">Stav služby</span><span class="sxs-lookup"><span data-stu-id="146b3-103">Service state</span></span>
<span data-ttu-id="146b3-104">**Služba stavu** odkazuje v paměti nebo na data na disku, který služba vyžaduje, aby funkce.</span><span class="sxs-lookup"><span data-stu-id="146b3-104">**Service state** refers to the in-memory or on disk data that a service requires to function.</span></span> <span data-ttu-id="146b3-105">Obsahuje, například datové struktury a členské proměnné, které Služba čte a zapisuje fungují.</span><span class="sxs-lookup"><span data-stu-id="146b3-105">It includes, for example, the data structures and member variables that the service reads and writes to do work.</span></span> <span data-ttu-id="146b3-106">V závislosti na tom, jak je navržen služby se mohly by také zahrnovat souborům nebo jiným prostředkům, které jsou uložené na disku.</span><span class="sxs-lookup"><span data-stu-id="146b3-106">Depending on how the service is architected, it could also include files or other resources that are stored on disk.</span></span> <span data-ttu-id="146b3-107">Například soubory databáze využije k uložení dat a protokoly transakcí.</span><span class="sxs-lookup"><span data-stu-id="146b3-107">For example, the files a database would use to store data and transaction logs.</span></span>

<span data-ttu-id="146b3-108">Jako příklad služba zvažte kalkulačky.</span><span class="sxs-lookup"><span data-stu-id="146b3-108">As an example service, let's consider a calculator.</span></span> <span data-ttu-id="146b3-109">Základní kalkulačky služby trvá dvou čísel a vrátí jejich součet.</span><span class="sxs-lookup"><span data-stu-id="146b3-109">A basic calculator service takes two numbers and returns their sum.</span></span> <span data-ttu-id="146b3-110">Provádění tohoto výpočtu zahrnuje žádné proměnné členů nebo Další informace.</span><span class="sxs-lookup"><span data-stu-id="146b3-110">Performing this calculation involves no member variables or other information.</span></span>

<span data-ttu-id="146b3-111">Teď se podíváme stejné kalkulačky, ale s další metodu pro ukládání a vrací poslední součet je počítaný.</span><span class="sxs-lookup"><span data-stu-id="146b3-111">Now consider the same calculator, but with an additional method for storing and returning the last sum it has computed.</span></span> <span data-ttu-id="146b3-112">Tato služba je nyní stateful.</span><span class="sxs-lookup"><span data-stu-id="146b3-112">This service is now stateful.</span></span> <span data-ttu-id="146b3-113">Stateful znamená, že obsahuje některé stavu, který zapíše do při vypočítá nové sum a načte z když požádáte, vrátit poslední vypočítaný součet.</span><span class="sxs-lookup"><span data-stu-id="146b3-113">Stateful means it contains some state that it writes to when it computes a new sum and reads from when you ask it to return the last computed sum.</span></span>

<span data-ttu-id="146b3-114">V Azure Service Fabric nazývá první službě bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="146b3-114">In Azure Service Fabric, the first service is called a stateless service.</span></span> <span data-ttu-id="146b3-115">Druhý služba se nazývá stavové služby.</span><span class="sxs-lookup"><span data-stu-id="146b3-115">The second service is called a stateful service.</span></span>

## <a name="storing-service-state"></a><span data-ttu-id="146b3-116">Ukládání stavu služby</span><span class="sxs-lookup"><span data-stu-id="146b3-116">Storing service state</span></span>
<span data-ttu-id="146b3-117">Stav můžete externalized nebo umístěny společně s kódem, který je manipulace s stavu.</span><span class="sxs-lookup"><span data-stu-id="146b3-117">State can be either externalized or co-located with the code that is manipulating the state.</span></span> <span data-ttu-id="146b3-118">Externalization stavu se obvykle provádí pomocí externí databáze nebo jiného úložiště dat, který běží na různých počítačích přes síť nebo mimo proces na stejném počítači.</span><span class="sxs-lookup"><span data-stu-id="146b3-118">Externalization of state is typically done by using an external database or other data store that runs on different machines over the network or out of process on the same machine.</span></span> <span data-ttu-id="146b3-119">V našem příkladu kalkulačky úložiště dat může být SQL database nebo instanci Azure tabulka úložiště.</span><span class="sxs-lookup"><span data-stu-id="146b3-119">In our calculator example, the data store could be a SQL database or instance of Azure Table Store.</span></span> <span data-ttu-id="146b3-120">Každý požadavek na výpočetní součet provede aktualizace na tato data a požadavky na službu k vrácení výsledku hodnota v hodnotě aktuální iterace z obchodu.</span><span class="sxs-lookup"><span data-stu-id="146b3-120">Every request to compute the sum performs an update on this data, and requests to the service to return the value result in the current value being fetched from the store.</span></span> 

<span data-ttu-id="146b3-121">Stav může být také společně umístěný se kód, který zpracovává stavu.</span><span class="sxs-lookup"><span data-stu-id="146b3-121">State can also be co-located with the code that manipulates the state.</span></span> <span data-ttu-id="146b3-122">Stavové služby v Service Fabric se obvykle vytvořená s využitím tohoto modelu.</span><span class="sxs-lookup"><span data-stu-id="146b3-122">Stateful services in Service Fabric are typically built using this model.</span></span> <span data-ttu-id="146b3-123">Service Fabric poskytuje infrastrukturu pro zkontrolujte, zda tento stav je vysoce dostupný, konzistentní a odolné a že služby vytvořené tímto způsobem můžete snadno škálovat.</span><span class="sxs-lookup"><span data-stu-id="146b3-123">Service Fabric provides the infrastructure to ensure that this state is highly available, consistent, and durable, and that the services built this way can easily scale.</span></span>

## <a name="next-steps"></a><span data-ttu-id="146b3-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="146b3-124">Next steps</span></span>
<span data-ttu-id="146b3-125">Další informace o konceptech Service Fabric najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="146b3-125">For more information on Service Fabric concepts, see the following articles:</span></span>

* [<span data-ttu-id="146b3-126">Dostupnost služeb Service Fabric</span><span class="sxs-lookup"><span data-stu-id="146b3-126">Availability of Service Fabric services</span></span>](service-fabric-availability-services.md)
* [<span data-ttu-id="146b3-127">Škálovatelnost služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="146b3-127">Scalability of Service Fabric services</span></span>](service-fabric-concepts-scalability.md)
* [<span data-ttu-id="146b3-128">Vytváření oddílů služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="146b3-128">Partitioning Service Fabric services</span></span>](service-fabric-concepts-partitioning.md)
* [<span data-ttu-id="146b3-129">Spolehlivé služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="146b3-129">Service Fabric Reliable Services</span></span>](service-fabric-reliable-services-introduction.md)
