---
title: "aaaDefinine a spravovat stav v Azure mikroslužeb | Microsoft Docs"
description: "Jak toodefine a spravovat stav služby v Service Fabric"
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
ms.openlocfilehash: 4a24696da71753d0f343a86df3556b5b7c964834
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-state"></a><span data-ttu-id="8752a-103">Stav služby</span><span class="sxs-lookup"><span data-stu-id="8752a-103">Service state</span></span>
<span data-ttu-id="8752a-104">**Služba stavu** odkazuje toohello v paměti nebo na diskových dat, že služba vyžaduje toofunction.</span><span class="sxs-lookup"><span data-stu-id="8752a-104">**Service state** refers toohello in-memory or on disk data that a service requires toofunction.</span></span> <span data-ttu-id="8752a-105">Obsahuje, například hello datové struktury a členské proměnné služby hello čte a zapisuje toodo práci.</span><span class="sxs-lookup"><span data-stu-id="8752a-105">It includes, for example, hello data structures and member variables that hello service reads and writes toodo work.</span></span> <span data-ttu-id="8752a-106">V závislosti na tom, jak je navržen hello služby se mohly by také zahrnovat souborům nebo jiným prostředkům, které jsou uložené na disku.</span><span class="sxs-lookup"><span data-stu-id="8752a-106">Depending on how hello service is architected, it could also include files or other resources that are stored on disk.</span></span> <span data-ttu-id="8752a-107">Například hello soubory databáze využije toostore dat a protokoly transakcí.</span><span class="sxs-lookup"><span data-stu-id="8752a-107">For example, hello files a database would use toostore data and transaction logs.</span></span>

<span data-ttu-id="8752a-108">Jako příklad služba zvažte kalkulačky.</span><span class="sxs-lookup"><span data-stu-id="8752a-108">As an example service, let's consider a calculator.</span></span> <span data-ttu-id="8752a-109">Základní kalkulačky služby trvá dvou čísel a vrátí jejich součet.</span><span class="sxs-lookup"><span data-stu-id="8752a-109">A basic calculator service takes two numbers and returns their sum.</span></span> <span data-ttu-id="8752a-110">Provádění tohoto výpočtu zahrnuje žádné proměnné členů nebo Další informace.</span><span class="sxs-lookup"><span data-stu-id="8752a-110">Performing this calculation involves no member variables or other information.</span></span>

<span data-ttu-id="8752a-111">Nyní zvažte, zda text hello stejné kalkulačky, ale s další metodu pro ukládání a vrací poslední součet hello má počítaný.</span><span class="sxs-lookup"><span data-stu-id="8752a-111">Now consider hello same calculator, but with an additional method for storing and returning hello last sum it has computed.</span></span> <span data-ttu-id="8752a-112">Tato služba je nyní stateful.</span><span class="sxs-lookup"><span data-stu-id="8752a-112">This service is now stateful.</span></span> <span data-ttu-id="8752a-113">Stateful znamená, že obsahuje některé stavu, který zapíše toowhen se vypočítá nové sum a čte z při požádejte tooreturn hello poslední vypočítaný součet.</span><span class="sxs-lookup"><span data-stu-id="8752a-113">Stateful means it contains some state that it writes toowhen it computes a new sum and reads from when you ask it tooreturn hello last computed sum.</span></span>

<span data-ttu-id="8752a-114">V Azure Service Fabric nazývá první službě hello bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="8752a-114">In Azure Service Fabric, hello first service is called a stateless service.</span></span> <span data-ttu-id="8752a-115">druhý služba Hello se nazývá stavové služby.</span><span class="sxs-lookup"><span data-stu-id="8752a-115">hello second service is called a stateful service.</span></span>

## <a name="storing-service-state"></a><span data-ttu-id="8752a-116">Ukládání stavu služby</span><span class="sxs-lookup"><span data-stu-id="8752a-116">Storing service state</span></span>
<span data-ttu-id="8752a-117">Stav můžete externalized nebo umístěn společně s hello kód, který je manipulace s hello stavu.</span><span class="sxs-lookup"><span data-stu-id="8752a-117">State can be either externalized or co-located with hello code that is manipulating hello state.</span></span> <span data-ttu-id="8752a-118">Externalization stavu se obvykle provádí pomocí externí databáze nebo jiné datové úložiště, že hello běží na různé počítače přes síť hello nebo mimo proces na stejném počítači.</span><span class="sxs-lookup"><span data-stu-id="8752a-118">Externalization of state is typically done by using an external database or other data store that runs on different machines over hello network or out of process on hello same machine.</span></span> <span data-ttu-id="8752a-119">V našem příkladu kalkulačky úložiště dat hello může být SQL database nebo instanci Azure tabulka úložiště.</span><span class="sxs-lookup"><span data-stu-id="8752a-119">In our calculator example, hello data store could be a SQL database or instance of Azure Table Store.</span></span> <span data-ttu-id="8752a-120">Každý požadavek toocompute hello součet provede aktualizace na tato data a požadavky hodnota hello toohello službou tooreturn mají za následek hello aktuální hodnota je načtena z úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="8752a-120">Every request toocompute hello sum performs an update on this data, and requests toohello service tooreturn hello value result in hello current value being fetched from hello store.</span></span> 

<span data-ttu-id="8752a-121">Stav může být také umístěné společně se službou hello kód, který zpracovává hello stavu.</span><span class="sxs-lookup"><span data-stu-id="8752a-121">State can also be co-located with hello code that manipulates hello state.</span></span> <span data-ttu-id="8752a-122">Stavové služby v Service Fabric se obvykle vytvořená s využitím tohoto modelu.</span><span class="sxs-lookup"><span data-stu-id="8752a-122">Stateful services in Service Fabric are typically built using this model.</span></span> <span data-ttu-id="8752a-123">Service Fabric nabízí hello infrastruktury tooensure tento stav je vysoce dostupný, konzistentní a odolné, a že služby hello integrované tímto způsobem můžete snadno škálovat.</span><span class="sxs-lookup"><span data-stu-id="8752a-123">Service Fabric provides hello infrastructure tooensure that this state is highly available, consistent, and durable, and that hello services built this way can easily scale.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8752a-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8752a-124">Next steps</span></span>
<span data-ttu-id="8752a-125">Další informace o konceptech Service Fabric najdete v tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="8752a-125">For more information on Service Fabric concepts, see hello following articles:</span></span>

* [<span data-ttu-id="8752a-126">Dostupnost služeb Service Fabric</span><span class="sxs-lookup"><span data-stu-id="8752a-126">Availability of Service Fabric services</span></span>](service-fabric-availability-services.md)
* [<span data-ttu-id="8752a-127">Škálovatelnost služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="8752a-127">Scalability of Service Fabric services</span></span>](service-fabric-concepts-scalability.md)
* [<span data-ttu-id="8752a-128">Vytváření oddílů služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="8752a-128">Partitioning Service Fabric services</span></span>](service-fabric-concepts-partitioning.md)
* [<span data-ttu-id="8752a-129">Spolehlivé služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="8752a-129">Service Fabric Reliable Services</span></span>](service-fabric-reliable-services-introduction.md)
