---
title: "Správce spolehlivé stavu Service Fabric a spolehlivé kolekce internals aaaAzure | Microsoft Docs"
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
ms.openlocfilehash: 651bfb52785a2475e4840cd471e87220d1936391
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-reliable-state-manager-and-reliable-collection-internals"></a><span data-ttu-id="e6a60-103">Interní informace o stavu spolehlivé infrastruktury Azure Service Manager a spolehlivé kolekce</span><span class="sxs-lookup"><span data-stu-id="e6a60-103">Azure Service Fabric Reliable State Manager and Reliable Collection internals</span></span>
<span data-ttu-id="e6a60-104">Tento dokument obsahuje podrobné uvnitř spolehlivé správce stavu a spolehlivé kolekce toosee fungování komponenty jádra pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="e6a60-104">This document delves inside Reliable State Manager and Reliable Collections toosee how core components work behind hello scenes.</span></span>

> [!NOTE]
> <span data-ttu-id="e6a60-105">Tento dokument je práce v průběhu.</span><span class="sxs-lookup"><span data-stu-id="e6a60-105">This document is work in-progress.</span></span> <span data-ttu-id="e6a60-106">Přidejte komentář toothis článek tootell nám jaké tématu chcete více informací o toolearn.</span><span class="sxs-lookup"><span data-stu-id="e6a60-106">Add comments toothis article tootell us what topic you would like toolearn more about.</span></span>
>

##  <a name="local-persistence-model-log-and-checkpoint"></a><span data-ttu-id="e6a60-107">Model místní trvalost: kontrolního bodu a protokolu</span><span class="sxs-lookup"><span data-stu-id="e6a60-107">Local persistence model: log and checkpoint</span></span>
<span data-ttu-id="e6a60-108">Hello spolehlivé správce stavu a spolehlivé kolekce podle trvalost model, který se nazývá kontrolního bodu a protokolu.</span><span class="sxs-lookup"><span data-stu-id="e6a60-108">hello Reliable State Manager and Reliable Collections follow a persistence model that is called Log and Checkpoint.</span></span>
<span data-ttu-id="e6a60-109">V tomto modelu je každé změně stavu nejprve zaznamenána na disku a potom se použijí v paměti.</span><span class="sxs-lookup"><span data-stu-id="e6a60-109">In this model, each state change is logged on disk first and then applied in memory.</span></span>
<span data-ttu-id="e6a60-110">sám dokončení stát Hello je nastavené jako trvalé jen občas (také známa jako</span><span class="sxs-lookup"><span data-stu-id="e6a60-110">hello complete state itself is persisted only occasionally (a.k.a.</span></span> <span data-ttu-id="e6a60-111">Kontrolní bod).</span><span class="sxs-lookup"><span data-stu-id="e6a60-111">Checkpoint).</span></span>
<span data-ttu-id="e6a60-112">Hello výhodou je, že rozdíly jsou převedena na sekvenční připojovacího zápisy na disk pro zlepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="e6a60-112">hello benefit is that deltas are turned into sequential append-only writes on disk for improved performance.</span></span>

<span data-ttu-id="e6a60-113">toobetter pochopit hello protokolu a modelu kontrolního bodu, nejdříve Podíváme se na scénář nekonečné disku hello.</span><span class="sxs-lookup"><span data-stu-id="e6a60-113">toobetter understand hello Log and Checkpoint model, let’s first look at hello infinite disk scenario.</span></span>
<span data-ttu-id="e6a60-114">Hello spolehlivé správce stavu zaznamená každé operace předtím, než se replikují.</span><span class="sxs-lookup"><span data-stu-id="e6a60-114">hello Reliable State Manager logs every operation before it is replicated.</span></span>
<span data-ttu-id="e6a60-115">Protokolování umožňuje hello spolehlivé kolekce tooapply hello operace jenom v paměti.</span><span class="sxs-lookup"><span data-stu-id="e6a60-115">Logging allows hello Reliable Collections tooapply hello operation only in memory.</span></span>
<span data-ttu-id="e6a60-116">Vzhledem k tomu, že protokoly jsou nastavené jako trvalé, i v případě, že repliky hello selže a je třeba toobe restartování hello spolehlivé správce stavu má dostatek informací v jeho tooreplay protokolu všechny hello operace, které hello repliky ztratil.</span><span class="sxs-lookup"><span data-stu-id="e6a60-116">Since logs are persisted, even when hello replica fails and needs toobe restarted, hello Reliable State Manager has enough information in its log tooreplay all hello operations hello replica has lost.</span></span>
<span data-ttu-id="e6a60-117">Jako hello disk je nekonečno, záznamy protokolu nikdy nepotřebují toobe odebrat a hello spolehlivé kolekce musí toomanage pouze hello v paměti stavu.</span><span class="sxs-lookup"><span data-stu-id="e6a60-117">As hello disk is infinite, log records never need toobe removed and hello Reliable Collection needs toomanage only hello in-memory state.</span></span>

<span data-ttu-id="e6a60-118">Nyní Podíváme se na scénář konečné disku hello.</span><span class="sxs-lookup"><span data-stu-id="e6a60-118">Now let’s look at hello finite disk scenario.</span></span>
<span data-ttu-id="e6a60-119">Jako záznamy protokolu hromadit, hello spolehlivé správce stavu spustí nedostatek místa na disku.</span><span class="sxs-lookup"><span data-stu-id="e6a60-119">As log records accumulate, hello Reliable State Manager will run out of disk space.</span></span>
<span data-ttu-id="e6a60-120">Předtím, než k tomu dojde, hello spolehlivé správce stavu musí tootruncate jeho toomake místo protokolu pro hello novější záznamy.</span><span class="sxs-lookup"><span data-stu-id="e6a60-120">Before that happens, hello Reliable State Manager needs tootruncate its log toomake room for hello newer records.</span></span>
<span data-ttu-id="e6a60-121">Požadavky správce stavu spolehlivé hello spolehlivé kolekce toocheckpoint toodisk jejich stav v paměti.</span><span class="sxs-lookup"><span data-stu-id="e6a60-121">Reliable State Manager requests hello Reliable Collections toocheckpoint their in-memory state toodisk.</span></span>
<span data-ttu-id="e6a60-122">V tomto okamžiku by hello spolehlivé kolekce zachována jeho stav v paměti.</span><span class="sxs-lookup"><span data-stu-id="e6a60-122">At this point, hello Reliable Collections' would persist its in-memory state.</span></span>
<span data-ttu-id="e6a60-123">Jakmile spolehlivé kolekce hello dokončit jejich kontrolní body, hello spolehlivé správce stavu můžete zkrátit hello protokolu toofree místa na disku.</span><span class="sxs-lookup"><span data-stu-id="e6a60-123">Once hello Reliable Collections complete their checkpoints, hello Reliable State Manager can truncate hello log toofree up disk space.</span></span>
<span data-ttu-id="e6a60-124">Když toobe restartovat, je potřeba hello repliky, spolehlivé kolekce obnovit jejich kontrolní bod stavu a hello spolehlivé správce stavu obnoví hraje zpět všechny změny stavu hello, kterým došlo od posledního kontrolního bodu hello.</span><span class="sxs-lookup"><span data-stu-id="e6a60-124">When hello replica needs toobe restarted, Reliable Collections recover their checkpointed state, and hello Reliable State Manager recovers and plays back all hello state changes that occurred since hello last checkpoint.</span></span>

<span data-ttu-id="e6a60-125">Jiné hodnoty add vytváření kontrolních bodů je, že vylepšuje časy obnovení v běžné scénáře.</span><span class="sxs-lookup"><span data-stu-id="e6a60-125">Another value add of checkpointing is that it improves recovery times in common scenarios.</span></span> <span data-ttu-id="e6a60-126">Protokol obsahuje všechny operace, které došlo od posledního kontrolního bodu hello.</span><span class="sxs-lookup"><span data-stu-id="e6a60-126">Log contains all operations that have happened since hello last checkpoint.</span></span>
<span data-ttu-id="e6a60-127">Proto může zahrnovat víc verzí položky jako více hodnot pro daný řádek v spolehlivé slovníku.</span><span class="sxs-lookup"><span data-stu-id="e6a60-127">So it may include multiple versions of an item like multiple values for a given row in Reliable Dictionary.</span></span>
<span data-ttu-id="e6a60-128">Naproti tomu kontrolní body spolehlivé kolekce hello pouze nejnovější verze jednotlivých hodnot pro klíč.</span><span class="sxs-lookup"><span data-stu-id="e6a60-128">In contrast, a Reliable Collection checkpoints only hello latest version of each value for a key.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6a60-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e6a60-129">Next steps</span></span>
* [<span data-ttu-id="e6a60-130">Transakce a zámky.</span><span class="sxs-lookup"><span data-stu-id="e6a60-130">Transactions and Locks</span></span>](service-fabric-reliable-services-reliable-collections-transactions-locks.md)

