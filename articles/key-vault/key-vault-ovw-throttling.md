---
ms.assetid: 
title: "Omezení pokyny Azure Key Vault | Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 06/21/2017
ms.openlocfilehash: fe700e22c5323c2a0bdc315e349cd119798bcf40
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-key-vault-throttling-guidance"></a><span data-ttu-id="24efd-102">Azure Key Vault omezení pokyny</span><span class="sxs-lookup"><span data-stu-id="24efd-102">Azure Key Vault throttling guidance</span></span>

<span data-ttu-id="24efd-103">Omezení je proces, který zahájení, který omezuje počet souběžných volání ve službě Azure, aby se zabránilo nadměrné využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="24efd-103">Throttling is a process you initiate that limits the number of concurrent calls to the Azure service to prevent overuse of resources.</span></span> <span data-ttu-id="24efd-104">Službou Azure Key Vault (AZURE) je určený k řešení k velkému počtu požadavků.</span><span class="sxs-lookup"><span data-stu-id="24efd-104">Azure Key Vault (AKV) is designed to handle a high volume of requests.</span></span> <span data-ttu-id="24efd-105">Pokud dojde k čtenáře počet požadavků, omezení požadavků vašeho klienta pomáhá udržovat optimální výkon a spolehlivost služby službou AZURE.</span><span class="sxs-lookup"><span data-stu-id="24efd-105">If an overwhelming number of requests occurs, throttling your client's requests helps maintain optimal performance and reliability of the AKV service.</span></span>

<span data-ttu-id="24efd-106">Omezení omezení lišit v závislosti na scénáři.</span><span class="sxs-lookup"><span data-stu-id="24efd-106">Throttling limits vary based on the scenario.</span></span> <span data-ttu-id="24efd-107">Například pokud provádíte značný zápisů, možnost omezení je vyšší než pokud pouze provádíte čtení.</span><span class="sxs-lookup"><span data-stu-id="24efd-107">For example, if you are performing a large volume of writes, the possibility for throttling is higher than if you are only performing reads.</span></span>

## <a name="how-does-key-vault-handle-its-limits"></a><span data-ttu-id="24efd-108">Jak Key Vault zpracovávat jeho omezení?</span><span class="sxs-lookup"><span data-stu-id="24efd-108">How does Key Vault handle its limits?</span></span>

<span data-ttu-id="24efd-109">Omezení služby v Key Vault existují zabránili zneužití prostředky a zajistit kvality služeb pro všechny klienty Key Vault.</span><span class="sxs-lookup"><span data-stu-id="24efd-109">Service limits in Key Vault are there to prevent misuse of resources and ensure quality of service for all of Key Vault’s clients.</span></span> <span data-ttu-id="24efd-110">Pokud je překročena prahová hodnota služby, limity Key Vault žádné další požadavky z tohoto klienta v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="24efd-110">When a service threshold is exceeded, Key Vault limits any further requests from that client for a period of time.</span></span> <span data-ttu-id="24efd-111">V takovém případě Key Vault vrátí stavový kód HTTP 429 (příliš mnoho požadavků), a požadavky selžou.</span><span class="sxs-lookup"><span data-stu-id="24efd-111">When this happens, Key Vault returns HTTP status code 429 (Too many requests), and the requests fail.</span></span> <span data-ttu-id="24efd-112">Navíc neúspěšné požadavky, které vrátí 429 směrem k mezní hodnoty omezení sledovanými Key Vault.</span><span class="sxs-lookup"><span data-stu-id="24efd-112">Also, failed requests that return a 429 count towards the throttle limits tracked by Key Vault.</span></span> 

<span data-ttu-id="24efd-113">Pokud máte platný obchodní případu pro vyšší šířku pásma, kontaktujte nás.</span><span class="sxs-lookup"><span data-stu-id="24efd-113">If you have a valid business case for higher throttle limits, please contact us.</span></span>


## <a name="how-to-throttle-your-app-in-response-to-service-limits"></a><span data-ttu-id="24efd-114">Omezení aplikace v reakci na omezení služby</span><span class="sxs-lookup"><span data-stu-id="24efd-114">How to throttle your app in response to service limits</span></span>

<span data-ttu-id="24efd-115">Následují **osvědčené postupy** omezení aplikace:</span><span class="sxs-lookup"><span data-stu-id="24efd-115">The following are **best practices** for throttling your app:</span></span>
- <span data-ttu-id="24efd-116">Snižte počet operací na základě požadavku.</span><span class="sxs-lookup"><span data-stu-id="24efd-116">Reduce the number of operations per request.</span></span>
- <span data-ttu-id="24efd-117">Snižte frekvenci požadavků.</span><span class="sxs-lookup"><span data-stu-id="24efd-117">Reduce the frequency of requests.</span></span>
- <span data-ttu-id="24efd-118">Vyhněte se okamžitě opakování.</span><span class="sxs-lookup"><span data-stu-id="24efd-118">Avoid immediate retries.</span></span> 
    - <span data-ttu-id="24efd-119">Všechny požadavky nabíhat proti vaší omezení použití.</span><span class="sxs-lookup"><span data-stu-id="24efd-119">All requests accrue against your usage limits.</span></span>

<span data-ttu-id="24efd-120">Pokud implementujete zpracování chyb vaší aplikace, použijte kód chyby HTTP 429 k detekci potřebu omezení na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="24efd-120">When you implement your app's error handling, use the HTTP error code 429 to detect the need for client-side throttling.</span></span> <span data-ttu-id="24efd-121">Pokud požadavek selže s kódem chyby HTTP 429 znovu, k stále dochází omezení služby Azure.</span><span class="sxs-lookup"><span data-stu-id="24efd-121">If the request fails again with an HTTP 429 error code, you are still encountering an Azure service limit.</span></span> <span data-ttu-id="24efd-122">Nadále používat doporučené na straně klienta omezování metoda, opakování žádosti, dokud neproběhne úspěšně.</span><span class="sxs-lookup"><span data-stu-id="24efd-122">Continue to use the recommended client-side throttling method, retrying the request until it succeeds.</span></span>

### <a name="recommended-client-side-throttling-method"></a><span data-ttu-id="24efd-123">Doporučený způsob omezení na straně klienta</span><span class="sxs-lookup"><span data-stu-id="24efd-123">Recommended client-side throttling method</span></span>

<span data-ttu-id="24efd-124">Na kód chyby protokolu HTTP 429 začněte omezení vašeho klienta pomocí exponenciálního omezení rychlosti přístup:</span><span class="sxs-lookup"><span data-stu-id="24efd-124">On HTTP error code 429, begin throttling your client using an exponential backoff approach:</span></span>

1. <span data-ttu-id="24efd-125">Počkejte 1 sekundu, opakujte žádost</span><span class="sxs-lookup"><span data-stu-id="24efd-125">Wait 1 second, retry request</span></span>
2. <span data-ttu-id="24efd-126">Pokud stále omezeny počkejte 2 sekundy, opakujte žádost</span><span class="sxs-lookup"><span data-stu-id="24efd-126">If still throttled wait 2 seconds, retry request</span></span>
3. <span data-ttu-id="24efd-127">Pokud stále omezeny čekání 4 a víc sekund, opakujte žádost</span><span class="sxs-lookup"><span data-stu-id="24efd-127">If still throttled wait 4 seconds, retry request</span></span>
4. <span data-ttu-id="24efd-128">Pokud stále omezeny čekání 8 sekund, opakujte žádost</span><span class="sxs-lookup"><span data-stu-id="24efd-128">If still throttled wait 8 seconds, retry request</span></span>
5. <span data-ttu-id="24efd-129">Pokud stále omezeny čekání 16 sekund, opakujte žádost</span><span class="sxs-lookup"><span data-stu-id="24efd-129">If still throttled wait 16 seconds, retry request</span></span>

<span data-ttu-id="24efd-130">Nesmí být v tomto okamžiku získávání kódy odpovědí HTTP 429.</span><span class="sxs-lookup"><span data-stu-id="24efd-130">At this point, you should not be getting HTTP 429 response codes.</span></span>

## <a name="see-also"></a><span data-ttu-id="24efd-131">Viz také</span><span class="sxs-lookup"><span data-stu-id="24efd-131">See also</span></span>

<span data-ttu-id="24efd-132">Hlubší orientaci omezení na cloudu Microsoftu, najdete v části [omezení vzor](https://docs.microsoft.com/azure/architecture/patterns/throttling).</span><span class="sxs-lookup"><span data-stu-id="24efd-132">For a deeper orientation of throttling on the Microsoft Cloud, see [Throttling Pattern](https://docs.microsoft.com/azure/architecture/patterns/throttling).</span></span>

