---
title: "Osvědčené postupy pro řešení Azure Functions | Microsoft Docs"
description: "Přečtěte si informace osvědčené postupy a vzory pro Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "řešení Azure functions, vzory, osvědčeným postupem, funkce, událostí zpracování, webhooků, dynamické výpočetní, bez serveru architektura"
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/13/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 645a5dd16e72619e7c2470ab8f03098f0fa6c7f8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="optimize-the-performance-and-reliability-of-azure-functions"></a><span data-ttu-id="79044-104">Optimalizace výkonu a spolehlivosti Azure functions</span><span class="sxs-lookup"><span data-stu-id="79044-104">Optimize the performance and reliability of Azure Functions</span></span>

<span data-ttu-id="79044-105">Tento článek obsahuje pokyny ke zlepšení výkonu a spolehlivosti aplikací pro funkce.</span><span class="sxs-lookup"><span data-stu-id="79044-105">This article provides guidance to improve the performance and reliability of your function apps.</span></span> 


## <a name="avoid-long-running-functions"></a><span data-ttu-id="79044-106">Vyhněte se dlouho běžící funkce</span><span class="sxs-lookup"><span data-stu-id="79044-106">Avoid long running functions</span></span>

<span data-ttu-id="79044-107">Velké, dlouhotrvajících funkce může způsobit neočekávané vypršení časového limitu problémy.</span><span class="sxs-lookup"><span data-stu-id="79044-107">Large, long-running functions can cause unexpected timeout issues.</span></span> <span data-ttu-id="79044-108">Funkce se může stát velké z důvodu velký počet závislostí Node.js.</span><span class="sxs-lookup"><span data-stu-id="79044-108">A function can become large due to many Node.js dependencies.</span></span> <span data-ttu-id="79044-109">Import závislosti může také způsobit způsobit neočekávané vypršení časových limitů opakování zvýšené zátěže.</span><span class="sxs-lookup"><span data-stu-id="79044-109">Importing dependencies can also cause increased load times that result in unexpected timeouts.</span></span> <span data-ttu-id="79044-110">Závislosti jsou načíst jak explicitně a implicitně.</span><span class="sxs-lookup"><span data-stu-id="79044-110">Dependencies are loaded both explicitly and implicitly.</span></span> <span data-ttu-id="79044-111">Jeden modul načteny kódu může načíst vlastní dalších modulů.</span><span class="sxs-lookup"><span data-stu-id="79044-111">A single module loaded by your code may load its own additional modules.</span></span>  

<span data-ttu-id="79044-112">Kdykoli je to možné, refactor velké funkce do menší funkce nastaví které pracují společně a rychle vrátit odpovědi.</span><span class="sxs-lookup"><span data-stu-id="79044-112">Whenever possible, refactor large functions into smaller function sets that work together and return responses fast.</span></span> <span data-ttu-id="79044-113">Například webhooku nebo funkce aktivace protokolu HTTP může vyžadovat odpověď potvrzení v určitých časovém limitu; je běžné, že webhooky vyžadují okamžitou reakci.</span><span class="sxs-lookup"><span data-stu-id="79044-113">For example, a webhook or HTTP trigger function might require an acknowledgment response within a certain time limit; it is common for webhooks to require an immediate response.</span></span> <span data-ttu-id="79044-114">Datová část aktivace protokolu HTTP můžete předat do fronty ke zpracování funkcí frontě aktivační události.</span><span class="sxs-lookup"><span data-stu-id="79044-114">You can pass the HTTP trigger payload into a queue to be processed by a queue trigger function.</span></span> <span data-ttu-id="79044-115">Tento přístup umožňuje odložení samotnou práci a vrátí okamžitou reakci.</span><span class="sxs-lookup"><span data-stu-id="79044-115">This approach allows you to defer the actual work and return an immediate response.</span></span>


## <a name="cross-function-communication"></a><span data-ttu-id="79044-116">Mezi funkce komunikace</span><span class="sxs-lookup"><span data-stu-id="79044-116">Cross function communication</span></span>

<span data-ttu-id="79044-117">Při integraci víc funkcí, je obecně doporučujeme používat fronty úložiště pro různé funkce komunikace.</span><span class="sxs-lookup"><span data-stu-id="79044-117">When integrating multiple functions, it is generally a best practice to use storage queues for cross function communication.</span></span>  <span data-ttu-id="79044-118">Hlavním důvodem je, že jsou fronty úložiště levnější a výrazně usnadňují zřizování.</span><span class="sxs-lookup"><span data-stu-id="79044-118">The main reason is storage queues are cheaper and much easier to provision.</span></span> 

<span data-ttu-id="79044-119">Jednotlivé zprávy ve frontě úložiště mají omezenou velikost na 64 KB.</span><span class="sxs-lookup"><span data-stu-id="79044-119">Individual messages in a storage queue are limited in size to 64 KB.</span></span> <span data-ttu-id="79044-120">Pokud potřebujete předat větší zprávy mezi funkce, Azure Service Bus fronty může podporovat zpráva velikostí až 256 KB.</span><span class="sxs-lookup"><span data-stu-id="79044-120">If you need to pass larger messages between functions, an Azure Service Bus queue could be used to support message sizes up to 256 KB.</span></span>

<span data-ttu-id="79044-121">Témata Service Bus jsou užitečné, pokud budete potřebovat filtrování před zpracováním zpráv.</span><span class="sxs-lookup"><span data-stu-id="79044-121">Service Bus topics are useful if you require message filtering before processing.</span></span>

<span data-ttu-id="79044-122">Centra událostí jsou užitečné pro podporu velkému komunikace.</span><span class="sxs-lookup"><span data-stu-id="79044-122">Event hubs are useful to support high volume communications.</span></span>


## <a name="write-functions-to-be-stateless"></a><span data-ttu-id="79044-123">Zápis funkce bez zadání stavu</span><span class="sxs-lookup"><span data-stu-id="79044-123">Write functions to be stateless</span></span> 

<span data-ttu-id="79044-124">Funkce by měla být bezstavové a idempotent, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="79044-124">Functions should be stateless and idempotent if possible.</span></span> <span data-ttu-id="79044-125">Přidružte žádné informace o stavu požadovaná data.</span><span class="sxs-lookup"><span data-stu-id="79044-125">Associate any required state information with your data.</span></span> <span data-ttu-id="79044-126">Například pořadí zpracování by měla mít pravděpodobně přiřazený `state` člen.</span><span class="sxs-lookup"><span data-stu-id="79044-126">For example, an order being processed would likely have an associated `state` member.</span></span> <span data-ttu-id="79044-127">Funkce může zpracovat pořadí na základě tohoto stavu při současném zachování bezstavové funkce sám sebe.</span><span class="sxs-lookup"><span data-stu-id="79044-127">A function could process an order based on that state while the function itself remains stateless.</span></span> 

<span data-ttu-id="79044-128">Funkce Idempotent doporučují zejména s aktivační události časovače.</span><span class="sxs-lookup"><span data-stu-id="79044-128">Idempotent functions are especially recommended with timer triggers.</span></span> <span data-ttu-id="79044-129">Například pokud máte něco, co nezbytně nutné spouštět jednou denně, zapisovat tak může probíhat kdykoli během dne se stejné výsledky.</span><span class="sxs-lookup"><span data-stu-id="79044-129">For example, if you have something that absolutely must run once a day, write it so it can run any time during the day with the same results.</span></span> <span data-ttu-id="79044-130">Funkce můžete ukončit, i když není žádná práce pro určitý den.</span><span class="sxs-lookup"><span data-stu-id="79044-130">The function can exit when there is no work for a particular day.</span></span> <span data-ttu-id="79044-131">Také pokud předchozím spuštění se nepovedlo dokončit, příštím spuštění by měl vyzvedávat kde bylo přerušeno.</span><span class="sxs-lookup"><span data-stu-id="79044-131">Also if a previous run failed to complete, the next run should pick up where it left off.</span></span>


## <a name="write-defensive-functions"></a><span data-ttu-id="79044-132">Napsat Obranným funkce</span><span class="sxs-lookup"><span data-stu-id="79044-132">Write defensive functions</span></span>

<span data-ttu-id="79044-133">Předpokládejme, že funkce může dojít k výjimce kdykoli.</span><span class="sxs-lookup"><span data-stu-id="79044-133">Assume your function could encounter an exception at any time.</span></span> <span data-ttu-id="79044-134">Návrh funkcí s možností pokračovat z předchozího bodu selhání při dalším spuštění.</span><span class="sxs-lookup"><span data-stu-id="79044-134">Design your functions with the ability to continue from a previous fail point during the next execution.</span></span> <span data-ttu-id="79044-135">Vezměte v úvahu scénář, který vyžaduje následující akce:</span><span class="sxs-lookup"><span data-stu-id="79044-135">Consider a scenario that requires the following actions:</span></span>

1. <span data-ttu-id="79044-136">Dotaz na 10 000 řádků v databáze.</span><span class="sxs-lookup"><span data-stu-id="79044-136">Query for 10,000 rows in a db.</span></span>
2. <span data-ttu-id="79044-137">Vytvořit zprávu fronty pro každou z těchto řádků ke zpracování další dolů na řádku.</span><span class="sxs-lookup"><span data-stu-id="79044-137">Create a queue message for each of those rows to process further down the line.</span></span>
 
<span data-ttu-id="79044-138">V závislosti na tom, jak složitá je systém, můžete mít: související se situací službami pro příjem dat chovají chybně výpadky sítě nebo kvóty omezuje dosáhlo maximálního atd. Všechny tyto může ovlivnit funkce kdykoli.</span><span class="sxs-lookup"><span data-stu-id="79044-138">Depending on how complex your system is, you may have: involved downstream services behaving badly, networking outages, or quota limits reached, etc. All of these can affect your function at any time.</span></span> <span data-ttu-id="79044-139">Je třeba navrhnout funkcí abyste byli připraveni pro ni.</span><span class="sxs-lookup"><span data-stu-id="79044-139">You need to design your functions to be prepared for it.</span></span>

<span data-ttu-id="79044-140">Jak kódu reagovat Pokud dojde k selhání po vložení 5 000 těchto položek do fronty pro zpracování?</span><span class="sxs-lookup"><span data-stu-id="79044-140">How does your code react if a failure occurs after inserting 5,000 of those items into a queue for processing?</span></span> <span data-ttu-id="79044-141">Sledování položek v sadě, která po dokončení.</span><span class="sxs-lookup"><span data-stu-id="79044-141">Track items in a set that you’ve completed.</span></span> <span data-ttu-id="79044-142">Jinak může vložte je dalším.</span><span class="sxs-lookup"><span data-stu-id="79044-142">Otherwise, you might insert them again next time.</span></span> <span data-ttu-id="79044-143">To může mít vážný dopad na pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="79044-143">This can have a serious impact on your work flow.</span></span> 

<span data-ttu-id="79044-144">Pokud již byla zpracována položka fronty, povolte funkce jako no-op.</span><span class="sxs-lookup"><span data-stu-id="79044-144">If a queue item was already processed, allow your function to be a no-op.</span></span>

<span data-ttu-id="79044-145">Výhodou Obranným opatření již součástí, které použijete v platformě Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="79044-145">Take advantage of defensive measures already provided for components you use in the Azure Functions platform.</span></span> <span data-ttu-id="79044-146">Například v tématu **zpracování poškozených fronty zpráv** v dokumentaci k [fronty Azure Storage aktivuje](functions-bindings-storage-queue.md#trigger).</span><span class="sxs-lookup"><span data-stu-id="79044-146">For example, see **Handling poison queue messages** in the documentation for [Azure Storage Queue triggers](functions-bindings-storage-queue.md#trigger).</span></span>
 

## <a name="dont-mix-test-and-production-code-in-the-same-function-app"></a><span data-ttu-id="79044-147">Nekombinujte testovací a produkční kódu ve stejné aplikaci – funkce</span><span class="sxs-lookup"><span data-stu-id="79044-147">Don't mix test and production code in the same function app</span></span>

<span data-ttu-id="79044-148">Funkce v rámci funkce aplikace sdílet prostředky.</span><span class="sxs-lookup"><span data-stu-id="79044-148">Functions within a function app share resources.</span></span> <span data-ttu-id="79044-149">Například je sdílené paměti.</span><span class="sxs-lookup"><span data-stu-id="79044-149">For example, memory is shared.</span></span> <span data-ttu-id="79044-150">Pokud používáte aplikaci funkce v produkčním prostředí, není funkcí spojených se testovací a prostředky do něj přidat.</span><span class="sxs-lookup"><span data-stu-id="79044-150">If you're using a function app in production, don't add test-related functions and resources to it.</span></span> <span data-ttu-id="79044-151">Může to způsobit neočekávané režii během provádění kódu produkční.</span><span class="sxs-lookup"><span data-stu-id="79044-151">It can cause unexpected overhead during production code execution.</span></span>

<span data-ttu-id="79044-152">Dávejte pozor, zatížení ve svých aplikacích funkce produkční.</span><span class="sxs-lookup"><span data-stu-id="79044-152">Be careful what you load in your production function apps.</span></span> <span data-ttu-id="79044-153">Paměť je průměrem každou funkci v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="79044-153">Memory is averaged across each function in the app.</span></span>

<span data-ttu-id="79044-154">Pokud máte sdílené sestavení odkazuje více funkcí rozhraní .net, můžete ji umístěte do běžné sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="79044-154">If you have a shared assembly referenced in multiple .Net functions, put it in a common shared folder.</span></span> <span data-ttu-id="79044-155">Odkaz sestavení s příkazem podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="79044-155">Reference the assembly with a statement similar to the following example:</span></span> 

    #r "..\Shared\MyAssembly.dll". 

<span data-ttu-id="79044-156">Jinak je snadno omylem nasadit více testovací verzí binární stejné s odlišným mezi funkce.</span><span class="sxs-lookup"><span data-stu-id="79044-156">Otherwise, it is easy to accidentally deploy multiple test versions of the same binary that behave differently between functions.</span></span>

<span data-ttu-id="79044-157">Nepoužívejte podrobné protokolování v produkčním kódu.</span><span class="sxs-lookup"><span data-stu-id="79044-157">Don't use verbose logging in production code.</span></span> <span data-ttu-id="79044-158">Má negativním dopadem na výkon.</span><span class="sxs-lookup"><span data-stu-id="79044-158">It has a negative performance impact.</span></span>



## <a name="use-async-code-but-avoid-blocking-calls"></a><span data-ttu-id="79044-159">Použít asynchronní kód, ale zabránění blokování volání</span><span class="sxs-lookup"><span data-stu-id="79044-159">Use async code but avoid blocking calls</span></span>

<span data-ttu-id="79044-160">Asynchronní programování je součástí osvědčeného postupu.</span><span class="sxs-lookup"><span data-stu-id="79044-160">Asynchronous programming is a recommended best practice.</span></span> <span data-ttu-id="79044-161">Vždy-li však odkazující na `Result` vlastnost nebo volání `Wait` metodu `Task` instance.</span><span class="sxs-lookup"><span data-stu-id="79044-161">However, always avoid referencing the `Result` property or calling `Wait` method on a `Task` instance.</span></span> <span data-ttu-id="79044-162">Tento přístup může vést k vyčerpání přístup z více vláken.</span><span class="sxs-lookup"><span data-stu-id="79044-162">This approach can lead to thread exhaustion.</span></span>


[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="next-steps"></a><span data-ttu-id="79044-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79044-163">Next steps</span></span>
<span data-ttu-id="79044-164">Další informace najdete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="79044-164">For more information, see the following resources:</span></span>

<span data-ttu-id="79044-165">Protože Azure Functions využívá Azure App Service také měli být vědomi pokyny služby App Service.</span><span class="sxs-lookup"><span data-stu-id="79044-165">Because Azure Functions uses Azure App Service, you should also be aware of  App Service guidelines.</span></span>
* [<span data-ttu-id="79044-166">Patterns and Practices optimalizací výkonu protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="79044-166">Patterns and Practices HTTP Performance Optimizations</span></span>](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)
