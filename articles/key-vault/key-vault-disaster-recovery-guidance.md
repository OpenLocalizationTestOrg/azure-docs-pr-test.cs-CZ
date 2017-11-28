---
title: "aaaWhat toodo v události hello narušení služby Azure, který má vliv na Azure Key Vault | Microsoft Docs"
description: "Zjistěte, jaké toodo v události hello narušení služby Azure, který má vliv na Azure Key Vault."
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
ms.openlocfilehash: 88eec82ada401a28323b3eea126168185ba4cdb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-availability-and-redundancy"></a><span data-ttu-id="0962f-103">Azure Key Vault dostupnost a redundance</span><span class="sxs-lookup"><span data-stu-id="0962f-103">Azure Key Vault availability and redundancy</span></span>
<span data-ttu-id="0962f-104">Azure Key Vault funkce více vrstev redundance toomake klíče a tajné klíče zůstat k dispozici tooyour aplikace i v případě, že jednotlivé součásti aplikace hello služby nezdaří.</span><span class="sxs-lookup"><span data-stu-id="0962f-104">Azure Key Vault features multiple layers of redundancy toomake sure that your keys and secrets remain available tooyour application even if individual components of hello service fail.</span></span>

<span data-ttu-id="0962f-105">Hello obsah trezoru klíčů jsou replikované v rámci oblasti hello a sekundární oblasti tooa aspoň 150 miles ji okamžitě, ale v rámci hello stejné geography.</span><span class="sxs-lookup"><span data-stu-id="0962f-105">hello contents of your key vault are replicated within hello region and tooa secondary region at least 150 miles away but within hello same geography.</span></span> <span data-ttu-id="0962f-106">Tím se zachová vysokou odolnost klíčů a tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="0962f-106">This maintains high durability of your keys and secrets.</span></span> <span data-ttu-id="0962f-107">Najdete v části hello [Azure spárovat oblasti](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) dokumentu podrobnosti o páry určité oblasti.</span><span class="sxs-lookup"><span data-stu-id="0962f-107">See hello [Azure paired regions](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) document for details on specific region pairs.</span></span>

<span data-ttu-id="0962f-108">V případě jednotlivých součástí v rámci hello trezor klíčů služby selžou, alternativní součásti v rámci oblasti hello krok v tooserve vaše žádost o toomake se, že žádné snížení funkčnosti.</span><span class="sxs-lookup"><span data-stu-id="0962f-108">If individual components within hello key vault service fail, alternate components within hello region step in tooserve your request toomake sure that there is no degradation of functionality.</span></span> <span data-ttu-id="0962f-109">Není nutné tootake všechny akce tootrigger to.</span><span class="sxs-lookup"><span data-stu-id="0962f-109">You do not need tootake any action tootrigger this.</span></span> <span data-ttu-id="0962f-110">To se stane automaticky a bude průhledná tooyou.</span><span class="sxs-lookup"><span data-stu-id="0962f-110">It happens automatically and will be transparent tooyou.</span></span>

<span data-ttu-id="0962f-111">V hello výjimečná událost, není k dispozici celý oblasti Azure, se automaticky směrují hello požadavků, které provedete z Azure Key Vault v daném regionu (*převzetí služeb při selhání*) tooa sekundární oblast.</span><span class="sxs-lookup"><span data-stu-id="0962f-111">In hello rare event that an entire Azure region is unavailable, hello requests that you make of Azure Key Vault in that region are automatically routed (*failed over*) tooa secondary region.</span></span> <span data-ttu-id="0962f-112">Když primární oblasti hello opět k dispozici, požadavky na směrují zpět (*zpět se nezdařilo*) toohello primární oblasti.</span><span class="sxs-lookup"><span data-stu-id="0962f-112">When hello primary region is available again, requests are routed back (*failed back*) toohello primary region.</span></span> <span data-ttu-id="0962f-113">Znovu není nutné tootake žádnou akci vzhledem k tomu, že k tomu dojde automaticky.</span><span class="sxs-lookup"><span data-stu-id="0962f-113">Again, you do not need tootake any action because this happens automatically.</span></span>

<span data-ttu-id="0962f-114">Existují vědět několik toobe upozornění:</span><span class="sxs-lookup"><span data-stu-id="0962f-114">There are a few caveats toobe aware of:</span></span>

* <span data-ttu-id="0962f-115">V události hello selhání oblast se může trvat několik minut, než hello služby toofail.</span><span class="sxs-lookup"><span data-stu-id="0962f-115">In hello event of a region failover, it may take a few minutes for hello service toofail over.</span></span> <span data-ttu-id="0962f-116">Odesílat požadavky provedené během této doby může selhat, dokud se nedokončí hello převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="0962f-116">Requests that are made during this time may fail until hello failover completes.</span></span>
* <span data-ttu-id="0962f-117">Po dokončení převzetí služeb trezoru klíčů je v režimu jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="0962f-117">After a failover is complete, your key vault is in read-only mode.</span></span> <span data-ttu-id="0962f-118">Požadavky, které jsou podporovány v tomto režimu jsou:</span><span class="sxs-lookup"><span data-stu-id="0962f-118">Requests that are supported in this mode are:</span></span>
  * <span data-ttu-id="0962f-119">Seznam trezorů klíčů</span><span class="sxs-lookup"><span data-stu-id="0962f-119">List key vaults</span></span>
  * <span data-ttu-id="0962f-120">Získat vlastnosti trezorů klíčů</span><span class="sxs-lookup"><span data-stu-id="0962f-120">Get properties of key vaults</span></span>
  * <span data-ttu-id="0962f-121">Výpis tajných klíčů</span><span class="sxs-lookup"><span data-stu-id="0962f-121">List secrets</span></span>
  * <span data-ttu-id="0962f-122">Získat tajné klíče</span><span class="sxs-lookup"><span data-stu-id="0962f-122">Get secrets</span></span>
  * <span data-ttu-id="0962f-123">Seznam klíčů</span><span class="sxs-lookup"><span data-stu-id="0962f-123">List keys</span></span>
  * <span data-ttu-id="0962f-124">Získání klíčů (Vlastnosti)</span><span class="sxs-lookup"><span data-stu-id="0962f-124">Get (properties of) keys</span></span>
  * <span data-ttu-id="0962f-125">Šifrování</span><span class="sxs-lookup"><span data-stu-id="0962f-125">Encrypt</span></span>
  * <span data-ttu-id="0962f-126">Dešifrování</span><span class="sxs-lookup"><span data-stu-id="0962f-126">Decrypt</span></span>
  * <span data-ttu-id="0962f-127">Zabalení</span><span class="sxs-lookup"><span data-stu-id="0962f-127">Wrap</span></span>
  * <span data-ttu-id="0962f-128">Rozbalení</span><span class="sxs-lookup"><span data-stu-id="0962f-128">Unwrap</span></span>
  * <span data-ttu-id="0962f-129">Ověřit</span><span class="sxs-lookup"><span data-stu-id="0962f-129">Verify</span></span>
  * <span data-ttu-id="0962f-130">Přihlášení</span><span class="sxs-lookup"><span data-stu-id="0962f-130">Sign</span></span>
  * <span data-ttu-id="0962f-131">Zálohování</span><span class="sxs-lookup"><span data-stu-id="0962f-131">Backup</span></span>
* <span data-ttu-id="0962f-132">Po převzetí služeb se nezdařilo zpět, všechny typy požadavku (včetně čtení *a* požadavků na zápis) jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0962f-132">After a failover is failed back, all request types (including read *and* write requests) are available.</span></span>

