---
title: "Interní informace o stavu spolehlivé infrastruktury Azure Service Manager a spolehlivé kolekce | Microsoft Docs"
description: "Podrobné informace o konceptech spolehlivé kolekce a návrhu v Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: d607449a16e886337ab1bd96213fbb4231124353
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-service-fabric-reliable-state-manager-and-reliable-collection-internals"></a><span data-ttu-id="1112c-103">Interní informace o stavu spolehlivé infrastruktury Azure Service Manager a spolehlivé kolekce</span><span class="sxs-lookup"><span data-stu-id="1112c-103">Azure Service Fabric Reliable State Manager and Reliable Collection internals</span></span>
<span data-ttu-id="1112c-104">Tento dokument obsahuje podrobné uvnitř spolehlivé správce stavu a spolehlivé kolekcí najdete v části Jak základní komponenty fungují na pozadí.</span><span class="sxs-lookup"><span data-stu-id="1112c-104">This document delves inside Reliable State Manager and Reliable Collections to see how core components work behind the scenes.</span></span>

> [!NOTE]
> <span data-ttu-id="1112c-105">Tento dokument je práce v průběhu.</span><span class="sxs-lookup"><span data-stu-id="1112c-105">This document is work in-progress.</span></span> <span data-ttu-id="1112c-106">Přidávání komentářů k Řekněte nám, co téma, které chcete zobrazit další informace o tomto článku.</span><span class="sxs-lookup"><span data-stu-id="1112c-106">Add comments to this article to tell us what topic you would like to learn more about.</span></span>
>

##  <a name="local-persistence-model-log-and-checkpoint"></a><span data-ttu-id="1112c-107">Model místní trvalost: kontrolního bodu a protokolu</span><span class="sxs-lookup"><span data-stu-id="1112c-107">Local persistence model: log and checkpoint</span></span>
<span data-ttu-id="1112c-108">Správce spolehlivé stavu a spolehlivé kolekce postupujte podle trvalost model, který se nazývá kontrolního bodu a protokolu.</span><span class="sxs-lookup"><span data-stu-id="1112c-108">The Reliable State Manager and Reliable Collections follow a persistence model that is called Log and Checkpoint.</span></span>
<span data-ttu-id="1112c-109">V tomto modelu je každé změně stavu nejprve zaznamenána na disku a potom se použijí v paměti.</span><span class="sxs-lookup"><span data-stu-id="1112c-109">In this model, each state change is logged on disk first and then applied in memory.</span></span>
<span data-ttu-id="1112c-110">Stav dokončení samotné je nastavené jako trvalé jen občas (také známa jako</span><span class="sxs-lookup"><span data-stu-id="1112c-110">The complete state itself is persisted only occasionally (a.k.a.</span></span> <span data-ttu-id="1112c-111">Kontrolní bod).</span><span class="sxs-lookup"><span data-stu-id="1112c-111">Checkpoint).</span></span>
<span data-ttu-id="1112c-112">Výhodou je, že rozdíly jsou převedena na sekvenční připojovacího zápisy na disk pro zlepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="1112c-112">The benefit is that deltas are turned into sequential append-only writes on disk for improved performance.</span></span>

<span data-ttu-id="1112c-113">Abyste lépe pochopili modelu kontrolního bodu a protokolu, nejprve Podíváme se na scénář nekonečné disku.</span><span class="sxs-lookup"><span data-stu-id="1112c-113">To better understand the Log and Checkpoint model, let’s first look at the infinite disk scenario.</span></span>
<span data-ttu-id="1112c-114">Správce spolehlivé stavu zaznamená každé operace předtím, než se replikují.</span><span class="sxs-lookup"><span data-stu-id="1112c-114">The Reliable State Manager logs every operation before it is replicated.</span></span>
<span data-ttu-id="1112c-115">Protokolování umožňuje spolehlivé kolekce použití operace jenom v paměti.</span><span class="sxs-lookup"><span data-stu-id="1112c-115">Logging allows the Reliable Collections to apply the operation only in memory.</span></span>
<span data-ttu-id="1112c-116">Vzhledem k tomu, že protokoly jsou nastavené jako trvalé, i když replika selže a je třeba restartovat, spolehlivé správce stavu má dostatek informací v protokolu opakování všechny operace, které repliky ztratil.</span><span class="sxs-lookup"><span data-stu-id="1112c-116">Since logs are persisted, even when the replica fails and needs to be restarted, the Reliable State Manager has enough information in its log to replay all the operations the replica has lost.</span></span>
<span data-ttu-id="1112c-117">Protože disk je nekonečno, záznamy protokolu nikdy třeba je odebrat a spolehlivé kolekce je potřeba spravovat jenom stav v paměti.</span><span class="sxs-lookup"><span data-stu-id="1112c-117">As the disk is infinite, log records never need to be removed and the Reliable Collection needs to manage only the in-memory state.</span></span>

<span data-ttu-id="1112c-118">Nyní Podíváme se na scénář konečné disku.</span><span class="sxs-lookup"><span data-stu-id="1112c-118">Now let’s look at the finite disk scenario.</span></span>
<span data-ttu-id="1112c-119">Jako záznamy protokolu hromadit, spolehlivé správce stavu spustí nedostatek místa na disku.</span><span class="sxs-lookup"><span data-stu-id="1112c-119">As log records accumulate, the Reliable State Manager will run out of disk space.</span></span>
<span data-ttu-id="1112c-120">Předtím, než k tomu dojde, je potřeba zkrátit protokol, aby uvolnil prostor pro novější záznamy spolehlivé správce stavu.</span><span class="sxs-lookup"><span data-stu-id="1112c-120">Before that happens, the Reliable State Manager needs to truncate its log to make room for the newer records.</span></span>
<span data-ttu-id="1112c-121">Správce spolehlivé stavu požadavků spolehlivé kolekce kontrolního bodu jejich stavu v paměti na disk.</span><span class="sxs-lookup"><span data-stu-id="1112c-121">Reliable State Manager requests the Reliable Collections to checkpoint their in-memory state to disk.</span></span>
<span data-ttu-id="1112c-122">V tomto okamžiku by spolehlivé kolekce zachována jeho stav v paměti.</span><span class="sxs-lookup"><span data-stu-id="1112c-122">At this point, the Reliable Collections' would persist its in-memory state.</span></span>
<span data-ttu-id="1112c-123">Jakmile spolehlivé kolekce dokončit jejich kontrolní body, spolehlivé správce stavu můžete zkrátit protokol pro uvolnění místa na disku.</span><span class="sxs-lookup"><span data-stu-id="1112c-123">Once the Reliable Collections complete their checkpoints, the Reliable State Manager can truncate the log to free up disk space.</span></span>
<span data-ttu-id="1112c-124">Když repliku je třeba restartovat, spolehlivé kolekce obnovit jejich kontrolní bod stavu a spolehlivé správce stavu obnoví hraje zpět všechny změny stavu, k nimž došlo od posledního kontrolního bodu.</span><span class="sxs-lookup"><span data-stu-id="1112c-124">When the replica needs to be restarted, Reliable Collections recover their checkpointed state, and the Reliable State Manager recovers and plays back all the state changes that occurred since the last checkpoint.</span></span>

<span data-ttu-id="1112c-125">Jiné hodnoty add vytváření kontrolních bodů je, že vylepšuje časy obnovení v běžné scénáře.</span><span class="sxs-lookup"><span data-stu-id="1112c-125">Another value add of checkpointing is that it improves recovery times in common scenarios.</span></span> <span data-ttu-id="1112c-126">Protokol obsahuje všechny operace, které došlo od posledního kontrolního bodu.</span><span class="sxs-lookup"><span data-stu-id="1112c-126">Log contains all operations that have happened since the last checkpoint.</span></span>
<span data-ttu-id="1112c-127">Proto může zahrnovat víc verzí položky jako více hodnot pro daný řádek v spolehlivé slovníku.</span><span class="sxs-lookup"><span data-stu-id="1112c-127">So it may include multiple versions of an item like multiple values for a given row in Reliable Dictionary.</span></span>
<span data-ttu-id="1112c-128">Naproti tomu kontrolní body spolehlivé kolekce nejnovější verze jednotlivých hodnot pro klíč.</span><span class="sxs-lookup"><span data-stu-id="1112c-128">In contrast, a Reliable Collection checkpoints only the latest version of each value for a key.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1112c-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1112c-129">Next steps</span></span>
* [<span data-ttu-id="1112c-130">Transakce a zámky.</span><span class="sxs-lookup"><span data-stu-id="1112c-130">Transactions and Locks</span></span>](service-fabric-reliable-services-reliable-collections-transactions-locks.md)

