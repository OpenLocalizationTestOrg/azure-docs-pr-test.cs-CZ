---
ms.assetid: 
title: "aaaAzure omezení pokyny Key Vault | Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 06/21/2017
ms.openlocfilehash: a75cf96bc6503e51f14378bee598bad57e85be82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-throttling-guidance"></a><span data-ttu-id="866bf-102">Azure Key Vault omezení pokyny</span><span class="sxs-lookup"><span data-stu-id="866bf-102">Azure Key Vault throttling guidance</span></span>

<span data-ttu-id="866bf-103">Omezení je proces, který zahájení, který omezí hello počet souběžných volání toohello služba Azure nadměrné využití tooprevent prostředků.</span><span class="sxs-lookup"><span data-stu-id="866bf-103">Throttling is a process you initiate that limits hello number of concurrent calls toohello Azure service tooprevent overuse of resources.</span></span> <span data-ttu-id="866bf-104">Službou Azure Key Vault (AZURE) je navrženou toohandle k velkému počtu požadavků.</span><span class="sxs-lookup"><span data-stu-id="866bf-104">Azure Key Vault (AKV) is designed toohandle a high volume of requests.</span></span> <span data-ttu-id="866bf-105">Pokud dojde k čtenáře počet požadavků, omezení požadavků vašeho klienta pomáhá udržovat optimální výkon a spolehlivost hello služby službou AZURE.</span><span class="sxs-lookup"><span data-stu-id="866bf-105">If an overwhelming number of requests occurs, throttling your client's requests helps maintain optimal performance and reliability of hello AKV service.</span></span>

<span data-ttu-id="866bf-106">Omezení omezení lišit v závislosti na scénáři hello.</span><span class="sxs-lookup"><span data-stu-id="866bf-106">Throttling limits vary based on hello scenario.</span></span> <span data-ttu-id="866bf-107">Například pokud provádíte značný zápisů, možnost hello omezení je vyšší než pokud pouze provádíte čtení.</span><span class="sxs-lookup"><span data-stu-id="866bf-107">For example, if you are performing a large volume of writes, hello possibility for throttling is higher than if you are only performing reads.</span></span>

## <a name="how-does-key-vault-handle-its-limits"></a><span data-ttu-id="866bf-108">Jak Key Vault zpracovávat jeho omezení?</span><span class="sxs-lookup"><span data-stu-id="866bf-108">How does Key Vault handle its limits?</span></span>

<span data-ttu-id="866bf-109">Omezení služby v Key Vault existují tooprevent zneužití prostředků a ujistěte se kvality služeb pro všechny klienty Key Vault.</span><span class="sxs-lookup"><span data-stu-id="866bf-109">Service limits in Key Vault are there tooprevent misuse of resources and ensure quality of service for all of Key Vault’s clients.</span></span> <span data-ttu-id="866bf-110">Pokud je překročena prahová hodnota služby, limity Key Vault žádné další požadavky z tohoto klienta v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="866bf-110">When a service threshold is exceeded, Key Vault limits any further requests from that client for a period of time.</span></span> <span data-ttu-id="866bf-111">V takovém případě Key Vault vrátí stavový kód HTTP 429 (příliš mnoho požadavků), a hello požadavky služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="866bf-111">When this happens, Key Vault returns HTTP status code 429 (Too many requests), and hello requests fail.</span></span> <span data-ttu-id="866bf-112">Navíc neúspěšné požadavky, které vrátí 429 směrem omezení omezení hello sledovanými Key Vault.</span><span class="sxs-lookup"><span data-stu-id="866bf-112">Also, failed requests that return a 429 count towards hello throttle limits tracked by Key Vault.</span></span> 

<span data-ttu-id="866bf-113">Pokud máte platný obchodní případu pro vyšší šířku pásma, kontaktujte nás.</span><span class="sxs-lookup"><span data-stu-id="866bf-113">If you have a valid business case for higher throttle limits, please contact us.</span></span>


## <a name="how-toothrottle-your-app-in-response-tooservice-limits"></a><span data-ttu-id="866bf-114">Jak toothrottle aplikace v odpovědi tooservice omezení</span><span class="sxs-lookup"><span data-stu-id="866bf-114">How toothrottle your app in response tooservice limits</span></span>

<span data-ttu-id="866bf-115">Hello následují **osvědčené postupy** omezení aplikace:</span><span class="sxs-lookup"><span data-stu-id="866bf-115">hello following are **best practices** for throttling your app:</span></span>
- <span data-ttu-id="866bf-116">Snižte počet hello operací na základě požadavku.</span><span class="sxs-lookup"><span data-stu-id="866bf-116">Reduce hello number of operations per request.</span></span>
- <span data-ttu-id="866bf-117">Snižte četnost hello požadavků.</span><span class="sxs-lookup"><span data-stu-id="866bf-117">Reduce hello frequency of requests.</span></span>
- <span data-ttu-id="866bf-118">Vyhněte se okamžitě opakování.</span><span class="sxs-lookup"><span data-stu-id="866bf-118">Avoid immediate retries.</span></span> 
    - <span data-ttu-id="866bf-119">Všechny požadavky nabíhat proti vaší omezení použití.</span><span class="sxs-lookup"><span data-stu-id="866bf-119">All requests accrue against your usage limits.</span></span>

<span data-ttu-id="866bf-120">Pokud implementujete zpracování chyb vaší aplikace, použijte hello HTTP Chyba kód 429 toodetect hello nutnost omezení na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="866bf-120">When you implement your app's error handling, use hello HTTP error code 429 toodetect hello need for client-side throttling.</span></span> <span data-ttu-id="866bf-121">Pokud hello požadavek selže s kódem chyby HTTP 429 znovu, k stále dochází omezení služby Azure.</span><span class="sxs-lookup"><span data-stu-id="866bf-121">If hello request fails again with an HTTP 429 error code, you are still encountering an Azure service limit.</span></span> <span data-ttu-id="866bf-122">Pokračujte, toouse hello doporučená metoda omezení na straně klienta, opakování hello požadavku, dokud neproběhne úspěšně.</span><span class="sxs-lookup"><span data-stu-id="866bf-122">Continue toouse hello recommended client-side throttling method, retrying hello request until it succeeds.</span></span>

### <a name="recommended-client-side-throttling-method"></a><span data-ttu-id="866bf-123">Doporučený způsob omezení na straně klienta</span><span class="sxs-lookup"><span data-stu-id="866bf-123">Recommended client-side throttling method</span></span>

<span data-ttu-id="866bf-124">Na kód chyby protokolu HTTP 429 začněte omezení vašeho klienta pomocí exponenciálního omezení rychlosti přístup:</span><span class="sxs-lookup"><span data-stu-id="866bf-124">On HTTP error code 429, begin throttling your client using an exponential backoff approach:</span></span>

1. <span data-ttu-id="866bf-125">Počkejte 1 sekundu, opakujte žádost</span><span class="sxs-lookup"><span data-stu-id="866bf-125">Wait 1 second, retry request</span></span>
2. <span data-ttu-id="866bf-126">Pokud stále omezeny počkejte 2 sekundy, opakujte žádost</span><span class="sxs-lookup"><span data-stu-id="866bf-126">If still throttled wait 2 seconds, retry request</span></span>
3. <span data-ttu-id="866bf-127">Pokud stále omezeny čekání 4 a víc sekund, opakujte žádost</span><span class="sxs-lookup"><span data-stu-id="866bf-127">If still throttled wait 4 seconds, retry request</span></span>
4. <span data-ttu-id="866bf-128">Pokud stále omezeny čekání 8 sekund, opakujte žádost</span><span class="sxs-lookup"><span data-stu-id="866bf-128">If still throttled wait 8 seconds, retry request</span></span>
5. <span data-ttu-id="866bf-129">Pokud stále omezeny čekání 16 sekund, opakujte žádost</span><span class="sxs-lookup"><span data-stu-id="866bf-129">If still throttled wait 16 seconds, retry request</span></span>

<span data-ttu-id="866bf-130">Nesmí být v tomto okamžiku získávání kódy odpovědí HTTP 429.</span><span class="sxs-lookup"><span data-stu-id="866bf-130">At this point, you should not be getting HTTP 429 response codes.</span></span>

## <a name="see-also"></a><span data-ttu-id="866bf-131">Viz také</span><span class="sxs-lookup"><span data-stu-id="866bf-131">See also</span></span>

<span data-ttu-id="866bf-132">Hlubší orientaci omezení na hello cloudu Microsoftu, najdete v části [omezení vzor](https://docs.microsoft.com/azure/architecture/patterns/throttling).</span><span class="sxs-lookup"><span data-stu-id="866bf-132">For a deeper orientation of throttling on hello Microsoft Cloud, see [Throttling Pattern](https://docs.microsoft.com/azure/architecture/patterns/throttling).</span></span>

