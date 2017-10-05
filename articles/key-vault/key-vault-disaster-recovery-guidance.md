---
title: "Co dělat v případě Azure služby přerušení, která má vliv na Azure Key Vault | Microsoft Docs"
description: "Zjistěte, co dělat v případě přerušení služby Azure, který má vliv na Azure Key Vault."
services: key-vault
documentationcenter: 
author: adamglick
manager: mbaldwin
editor: 
ms.assetid: 19a9af63-3032-447b-9d1a-b0125f384edb
ms.service: key-vault
ms.workload: key-vault
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: sumedhb;aglick
ms.openlocfilehash: 6419d54c54e7d19103419262b79e7a5268b2268c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-key-vault-availability-and-redundancy"></a><span data-ttu-id="590ca-103">Azure Key Vault dostupnost a redundance</span><span class="sxs-lookup"><span data-stu-id="590ca-103">Azure Key Vault availability and redundancy</span></span>
<span data-ttu-id="590ca-104">Azure Key Vault funkce více vrstev redundance a ujistěte se, že klíče a tajné klíče zůstanou k dispozici pro vaše aplikace i v případě jednotlivé komponenty služby selžou.</span><span class="sxs-lookup"><span data-stu-id="590ca-104">Azure Key Vault features multiple layers of redundancy to make sure that your keys and secrets remain available to your application even if individual components of the service fail.</span></span>

<span data-ttu-id="590ca-105">Obsah trezoru klíčů se replikují do oblasti a do sekundární oblasti aspoň 150 miles ji okamžitě, ale v rámci stejné geography.</span><span class="sxs-lookup"><span data-stu-id="590ca-105">The contents of your key vault are replicated within the region and to a secondary region at least 150 miles away but within the same geography.</span></span> <span data-ttu-id="590ca-106">Tím se zachová vysokou odolnost klíčů a tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="590ca-106">This maintains high durability of your keys and secrets.</span></span> <span data-ttu-id="590ca-107">Najdete v článku [Azure spárovat oblasti](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) dokumentu podrobnosti o konkrétní oblasti páry.</span><span class="sxs-lookup"><span data-stu-id="590ca-107">See the [Azure paired regions](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) document for details on specific region pairs.</span></span>

<span data-ttu-id="590ca-108">Pokud jednotlivých součástí v rámci trezoru klíčů služby selže, krok alternativní součásti v rámci oblasti v poskytovat vaši žádost a ujistěte se, že neexistuje žádná snížení funkčnosti.</span><span class="sxs-lookup"><span data-stu-id="590ca-108">If individual components within the key vault service fail, alternate components within the region step in to serve your request to make sure that there is no degradation of functionality.</span></span> <span data-ttu-id="590ca-109">Není nutné provádět žádnou akci k aktivaci to.</span><span class="sxs-lookup"><span data-stu-id="590ca-109">You do not need to take any action to trigger this.</span></span> <span data-ttu-id="590ca-110">To se stane automaticky a budou průhledné vám.</span><span class="sxs-lookup"><span data-stu-id="590ca-110">It happens automatically and will be transparent to you.</span></span>

<span data-ttu-id="590ca-111">V výjimečná událost, že není k dispozici celý oblasti Azure, se automaticky směrují požadavky, které budete z Azure Key Vault v daném regionu (*převzetí služeb při selhání*) do sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="590ca-111">In the rare event that an entire Azure region is unavailable, the requests that you make of Azure Key Vault in that region are automatically routed (*failed over*) to a secondary region.</span></span> <span data-ttu-id="590ca-112">Když primární oblasti opět k dispozici, požadavky na směrují zpět (*zpět se nezdařilo*) primární oblasti.</span><span class="sxs-lookup"><span data-stu-id="590ca-112">When the primary region is available again, requests are routed back (*failed back*) to the primary region.</span></span> <span data-ttu-id="590ca-113">Znovu není nutné provádět žádnou akci, protože to probíhá automaticky.</span><span class="sxs-lookup"><span data-stu-id="590ca-113">Again, you do not need to take any action because this happens automatically.</span></span>

<span data-ttu-id="590ca-114">Existuje několik aspektů zajímat:</span><span class="sxs-lookup"><span data-stu-id="590ca-114">There are a few caveats to be aware of:</span></span>

* <span data-ttu-id="590ca-115">V případě selhání oblast ho může trvat několik minut, než služba převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="590ca-115">In the event of a region failover, it may take a few minutes for the service to fail over.</span></span> <span data-ttu-id="590ca-116">Odesílat požadavky provedené během této doby může selhat, dokud se nedokončí převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="590ca-116">Requests that are made during this time may fail until the failover completes.</span></span>
* <span data-ttu-id="590ca-117">Po dokončení převzetí služeb trezoru klíčů je v režimu jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="590ca-117">After a failover is complete, your key vault is in read-only mode.</span></span> <span data-ttu-id="590ca-118">Požadavky, které jsou podporovány v tomto režimu jsou:</span><span class="sxs-lookup"><span data-stu-id="590ca-118">Requests that are supported in this mode are:</span></span>
  * <span data-ttu-id="590ca-119">Seznam trezorů klíčů</span><span class="sxs-lookup"><span data-stu-id="590ca-119">List key vaults</span></span>
  * <span data-ttu-id="590ca-120">Získat vlastnosti trezorů klíčů</span><span class="sxs-lookup"><span data-stu-id="590ca-120">Get properties of key vaults</span></span>
  * <span data-ttu-id="590ca-121">Výpis tajných klíčů</span><span class="sxs-lookup"><span data-stu-id="590ca-121">List secrets</span></span>
  * <span data-ttu-id="590ca-122">Získat tajné klíče</span><span class="sxs-lookup"><span data-stu-id="590ca-122">Get secrets</span></span>
  * <span data-ttu-id="590ca-123">Seznam klíčů</span><span class="sxs-lookup"><span data-stu-id="590ca-123">List keys</span></span>
  * <span data-ttu-id="590ca-124">Získání klíčů (Vlastnosti)</span><span class="sxs-lookup"><span data-stu-id="590ca-124">Get (properties of) keys</span></span>
  * <span data-ttu-id="590ca-125">Šifrování</span><span class="sxs-lookup"><span data-stu-id="590ca-125">Encrypt</span></span>
  * <span data-ttu-id="590ca-126">Dešifrování</span><span class="sxs-lookup"><span data-stu-id="590ca-126">Decrypt</span></span>
  * <span data-ttu-id="590ca-127">Zabalení</span><span class="sxs-lookup"><span data-stu-id="590ca-127">Wrap</span></span>
  * <span data-ttu-id="590ca-128">Rozbalení</span><span class="sxs-lookup"><span data-stu-id="590ca-128">Unwrap</span></span>
  * <span data-ttu-id="590ca-129">Ověřit</span><span class="sxs-lookup"><span data-stu-id="590ca-129">Verify</span></span>
  * <span data-ttu-id="590ca-130">Přihlášení</span><span class="sxs-lookup"><span data-stu-id="590ca-130">Sign</span></span>
  * <span data-ttu-id="590ca-131">Zálohování</span><span class="sxs-lookup"><span data-stu-id="590ca-131">Backup</span></span>
* <span data-ttu-id="590ca-132">Po převzetí služeb se nezdařilo zpět, všechny typy požadavku (včetně čtení *a* požadavků na zápis) jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="590ca-132">After a failover is failed back, all request types (including read *and* write requests) are available.</span></span>

