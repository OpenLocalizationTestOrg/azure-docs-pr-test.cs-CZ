---
title: aaaBest postupy pro Azure Functions | Microsoft Docs
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
ms.openlocfilehash: bd3d826b75267a740e355b008943a48f71ebd339
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-hello-performance-and-reliability-of-azure-functions"></a><span data-ttu-id="8c277-104">Optimalizace hello výkonu a spolehlivosti Azure functions</span><span class="sxs-lookup"><span data-stu-id="8c277-104">Optimize hello performance and reliability of Azure Functions</span></span>

<span data-ttu-id="8c277-105">Tento článek obsahuje pokyny tooimprove hello výkonu a spolehlivosti aplikací pro funkce.</span><span class="sxs-lookup"><span data-stu-id="8c277-105">This article provides guidance tooimprove hello performance and reliability of your function apps.</span></span> 


## <a name="avoid-long-running-functions"></a><span data-ttu-id="8c277-106">Vyhněte se dlouho běžící funkce</span><span class="sxs-lookup"><span data-stu-id="8c277-106">Avoid long running functions</span></span>

<span data-ttu-id="8c277-107">Velké, dlouhotrvajících funkce může způsobit neočekávané vypršení časového limitu problémy.</span><span class="sxs-lookup"><span data-stu-id="8c277-107">Large, long-running functions can cause unexpected timeout issues.</span></span> <span data-ttu-id="8c277-108">Funkce se může stát velké z důvodu závislosti toomany Node.js.</span><span class="sxs-lookup"><span data-stu-id="8c277-108">A function can become large due toomany Node.js dependencies.</span></span> <span data-ttu-id="8c277-109">Import závislosti může také způsobit způsobit neočekávané vypršení časových limitů opakování zvýšené zátěže.</span><span class="sxs-lookup"><span data-stu-id="8c277-109">Importing dependencies can also cause increased load times that result in unexpected timeouts.</span></span> <span data-ttu-id="8c277-110">Závislosti jsou načíst jak explicitně a implicitně.</span><span class="sxs-lookup"><span data-stu-id="8c277-110">Dependencies are loaded both explicitly and implicitly.</span></span> <span data-ttu-id="8c277-111">Jeden modul načteny kódu může načíst vlastní dalších modulů.</span><span class="sxs-lookup"><span data-stu-id="8c277-111">A single module loaded by your code may load its own additional modules.</span></span>  

<span data-ttu-id="8c277-112">Kdykoli je to možné, refactor velké funkce do menší funkce nastaví které pracují společně a rychle vrátit odpovědi.</span><span class="sxs-lookup"><span data-stu-id="8c277-112">Whenever possible, refactor large functions into smaller function sets that work together and return responses fast.</span></span> <span data-ttu-id="8c277-113">Například webhooku nebo funkce aktivace protokolu HTTP může vyžadovat odpověď potvrzení v určitých časovém limitu; je obvyklé, webhooky toorequire okamžitou reakci.</span><span class="sxs-lookup"><span data-stu-id="8c277-113">For example, a webhook or HTTP trigger function might require an acknowledgment response within a certain time limit; it is common for webhooks toorequire an immediate response.</span></span> <span data-ttu-id="8c277-114">Datová část aktivační událost hello HTTP můžete předat do fronty toobe, zpracovává aktivační funkce fronty.</span><span class="sxs-lookup"><span data-stu-id="8c277-114">You can pass hello HTTP trigger payload into a queue toobe processed by a queue trigger function.</span></span> <span data-ttu-id="8c277-115">Tento přístup vám umožní toodefer hello samotnou práci a vrátí okamžitou reakci.</span><span class="sxs-lookup"><span data-stu-id="8c277-115">This approach allows you toodefer hello actual work and return an immediate response.</span></span>


## <a name="cross-function-communication"></a><span data-ttu-id="8c277-116">Mezi funkce komunikace</span><span class="sxs-lookup"><span data-stu-id="8c277-116">Cross function communication</span></span>

<span data-ttu-id="8c277-117">Při integraci víc funkcí, je obecně nejlepší postup toouse úložiště fronty pro různé funkce komunikace.</span><span class="sxs-lookup"><span data-stu-id="8c277-117">When integrating multiple functions, it is generally a best practice toouse storage queues for cross function communication.</span></span>  <span data-ttu-id="8c277-118">Hlavním důvodem Hello je fronty úložiště jsou levnější a mnohem snazší tooprovision.</span><span class="sxs-lookup"><span data-stu-id="8c277-118">hello main reason is storage queues are cheaper and much easier tooprovision.</span></span> 

<span data-ttu-id="8c277-119">Jednotlivé zprávy ve frontě úložiště mají omezenou velikost too64 KB.</span><span class="sxs-lookup"><span data-stu-id="8c277-119">Individual messages in a storage queue are limited in size too64 KB.</span></span> <span data-ttu-id="8c277-120">Pokud potřebujete větší zprávy toopass mezi funkce, frontu zpráv Azure Service Bus může být použité toosupport zpráva velikosti až too256 KB.</span><span class="sxs-lookup"><span data-stu-id="8c277-120">If you need toopass larger messages between functions, an Azure Service Bus queue could be used toosupport message sizes up too256 KB.</span></span>

<span data-ttu-id="8c277-121">Témata Service Bus jsou užitečné, pokud budete potřebovat filtrování před zpracováním zpráv.</span><span class="sxs-lookup"><span data-stu-id="8c277-121">Service Bus topics are useful if you require message filtering before processing.</span></span>

<span data-ttu-id="8c277-122">Centra událostí jsou užitečné toosupport velkému komunikace.</span><span class="sxs-lookup"><span data-stu-id="8c277-122">Event hubs are useful toosupport high volume communications.</span></span>


## <a name="write-functions-toobe-stateless"></a><span data-ttu-id="8c277-123">Zápis funkce toobe bezstavové</span><span class="sxs-lookup"><span data-stu-id="8c277-123">Write functions toobe stateless</span></span> 

<span data-ttu-id="8c277-124">Funkce by měla být bezstavové a idempotent, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="8c277-124">Functions should be stateless and idempotent if possible.</span></span> <span data-ttu-id="8c277-125">Přidružte žádné informace o stavu požadovaná data.</span><span class="sxs-lookup"><span data-stu-id="8c277-125">Associate any required state information with your data.</span></span> <span data-ttu-id="8c277-126">Například pořadí zpracování by měla mít pravděpodobně přiřazený `state` člen.</span><span class="sxs-lookup"><span data-stu-id="8c277-126">For example, an order being processed would likely have an associated `state` member.</span></span> <span data-ttu-id="8c277-127">Funkce může zpracovat pořadí na základě tohoto stavu při současném zachování bezstavové hello funkce sám sebe.</span><span class="sxs-lookup"><span data-stu-id="8c277-127">A function could process an order based on that state while hello function itself remains stateless.</span></span> 

<span data-ttu-id="8c277-128">Funkce Idempotent doporučují zejména s aktivační události časovače.</span><span class="sxs-lookup"><span data-stu-id="8c277-128">Idempotent functions are especially recommended with timer triggers.</span></span> <span data-ttu-id="8c277-129">Například pokud máte něco, co nezbytně nutné spouštět jednou denně, zapisovat tak může probíhat kdykoli během dne hello s hello stejné výsledky.</span><span class="sxs-lookup"><span data-stu-id="8c277-129">For example, if you have something that absolutely must run once a day, write it so it can run any time during hello day with hello same results.</span></span> <span data-ttu-id="8c277-130">Funkce Hello můžete ukončit, i když není žádná práce pro určitý den.</span><span class="sxs-lookup"><span data-stu-id="8c277-130">hello function can exit when there is no work for a particular day.</span></span> <span data-ttu-id="8c277-131">Také pokud předchozího spuštění nezdařilo toocomplete, hello příštím spuštění by měl vyzvedávat kde bylo přerušeno.</span><span class="sxs-lookup"><span data-stu-id="8c277-131">Also if a previous run failed toocomplete, hello next run should pick up where it left off.</span></span>


## <a name="write-defensive-functions"></a><span data-ttu-id="8c277-132">Napsat Obranným funkce</span><span class="sxs-lookup"><span data-stu-id="8c277-132">Write defensive functions</span></span>

<span data-ttu-id="8c277-133">Předpokládejme, že funkce může dojít k výjimce kdykoli.</span><span class="sxs-lookup"><span data-stu-id="8c277-133">Assume your function could encounter an exception at any time.</span></span> <span data-ttu-id="8c277-134">Návrh funkce s hello možnost toocontinue z předchozího bodu selhání při dalším spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="8c277-134">Design your functions with hello ability toocontinue from a previous fail point during hello next execution.</span></span> <span data-ttu-id="8c277-135">Vezměte v úvahu scénář, který vyžaduje hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="8c277-135">Consider a scenario that requires hello following actions:</span></span>

1. <span data-ttu-id="8c277-136">Dotaz na 10 000 řádků v databáze.</span><span class="sxs-lookup"><span data-stu-id="8c277-136">Query for 10,000 rows in a db.</span></span>
2. <span data-ttu-id="8c277-137">Vytvořte zprávu fronty pro každou z těchto řádků tooprocess další dolů hello řádku.</span><span class="sxs-lookup"><span data-stu-id="8c277-137">Create a queue message for each of those rows tooprocess further down hello line.</span></span>
 
<span data-ttu-id="8c277-138">V závislosti na tom, jak složitá je systém, můžete mít: související se situací službami pro příjem dat chovají chybně výpadky sítě nebo kvóty omezuje dosáhlo maximálního atd. Všechny tyto může ovlivnit funkce kdykoli.</span><span class="sxs-lookup"><span data-stu-id="8c277-138">Depending on how complex your system is, you may have: involved downstream services behaving badly, networking outages, or quota limits reached, etc. All of these can affect your function at any time.</span></span> <span data-ttu-id="8c277-139">Je nutné toodesign vaše toobe funkce připravené pro ni.</span><span class="sxs-lookup"><span data-stu-id="8c277-139">You need toodesign your functions toobe prepared for it.</span></span>

<span data-ttu-id="8c277-140">Jak kódu reagovat Pokud dojde k selhání po vložení 5 000 těchto položek do fronty pro zpracování?</span><span class="sxs-lookup"><span data-stu-id="8c277-140">How does your code react if a failure occurs after inserting 5,000 of those items into a queue for processing?</span></span> <span data-ttu-id="8c277-141">Sledování položek v sadě, která po dokončení.</span><span class="sxs-lookup"><span data-stu-id="8c277-141">Track items in a set that you’ve completed.</span></span> <span data-ttu-id="8c277-142">Jinak může vložte je dalším.</span><span class="sxs-lookup"><span data-stu-id="8c277-142">Otherwise, you might insert them again next time.</span></span> <span data-ttu-id="8c277-143">To může mít vážný dopad na pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="8c277-143">This can have a serious impact on your work flow.</span></span> 

<span data-ttu-id="8c277-144">Pokud již byla zpracována položka fronty, povolte operaci-vaší toobe funkce.</span><span class="sxs-lookup"><span data-stu-id="8c277-144">If a queue item was already processed, allow your function toobe a no-op.</span></span>

<span data-ttu-id="8c277-145">Výhodou Obranným opatření již součástí, které použijete v platformě Azure Functions hello.</span><span class="sxs-lookup"><span data-stu-id="8c277-145">Take advantage of defensive measures already provided for components you use in hello Azure Functions platform.</span></span> <span data-ttu-id="8c277-146">Například v tématu **zpracování poškozených fronty zpráv** v dokumentaci hello [fronty Azure Storage aktivuje](functions-bindings-storage-queue.md#trigger).</span><span class="sxs-lookup"><span data-stu-id="8c277-146">For example, see **Handling poison queue messages** in hello documentation for [Azure Storage Queue triggers](functions-bindings-storage-queue.md#trigger).</span></span>
 

## <a name="dont-mix-test-and-production-code-in-hello-same-function-app"></a><span data-ttu-id="8c277-147">Nemáte kombinace testovací a produkční kód na hello stejné funkce aplikace</span><span class="sxs-lookup"><span data-stu-id="8c277-147">Don't mix test and production code in hello same function app</span></span>

<span data-ttu-id="8c277-148">Funkce v rámci funkce aplikace sdílet prostředky.</span><span class="sxs-lookup"><span data-stu-id="8c277-148">Functions within a function app share resources.</span></span> <span data-ttu-id="8c277-149">Například je sdílené paměti.</span><span class="sxs-lookup"><span data-stu-id="8c277-149">For example, memory is shared.</span></span> <span data-ttu-id="8c277-150">Pokud používáte aplikaci funkce v produkčním prostředí, nepřidáte funkcí spojených se testovací a tooit prostředky.</span><span class="sxs-lookup"><span data-stu-id="8c277-150">If you're using a function app in production, don't add test-related functions and resources tooit.</span></span> <span data-ttu-id="8c277-151">Může to způsobit neočekávané režii během provádění kódu produkční.</span><span class="sxs-lookup"><span data-stu-id="8c277-151">It can cause unexpected overhead during production code execution.</span></span>

<span data-ttu-id="8c277-152">Dávejte pozor, zatížení ve svých aplikacích funkce produkční.</span><span class="sxs-lookup"><span data-stu-id="8c277-152">Be careful what you load in your production function apps.</span></span> <span data-ttu-id="8c277-153">Paměť je průměrem každou funkci v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="8c277-153">Memory is averaged across each function in hello app.</span></span>

<span data-ttu-id="8c277-154">Pokud máte sdílené sestavení odkazuje více funkcí rozhraní .net, můžete ji umístěte do běžné sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="8c277-154">If you have a shared assembly referenced in multiple .Net functions, put it in a common shared folder.</span></span> <span data-ttu-id="8c277-155">Odkaz na sestavení hello s příkaz podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c277-155">Reference hello assembly with a statement similar toohello following example:</span></span> 

    #r "..\Shared\MyAssembly.dll". 

<span data-ttu-id="8c277-156">Jinak, je snadné tooaccidentally nasadit více verzí testovací hello stejné binární soubor, který se chovat jinak mezi funkce.</span><span class="sxs-lookup"><span data-stu-id="8c277-156">Otherwise, it is easy tooaccidentally deploy multiple test versions of hello same binary that behave differently between functions.</span></span>

<span data-ttu-id="8c277-157">Nepoužívejte podrobné protokolování v produkčním kódu.</span><span class="sxs-lookup"><span data-stu-id="8c277-157">Don't use verbose logging in production code.</span></span> <span data-ttu-id="8c277-158">Má negativním dopadem na výkon.</span><span class="sxs-lookup"><span data-stu-id="8c277-158">It has a negative performance impact.</span></span>



## <a name="use-async-code-but-avoid-blocking-calls"></a><span data-ttu-id="8c277-159">Použít asynchronní kód, ale zabránění blokování volání</span><span class="sxs-lookup"><span data-stu-id="8c277-159">Use async code but avoid blocking calls</span></span>

<span data-ttu-id="8c277-160">Asynchronní programování je součástí osvědčeného postupu.</span><span class="sxs-lookup"><span data-stu-id="8c277-160">Asynchronous programming is a recommended best practice.</span></span> <span data-ttu-id="8c277-161">Vždy-li však odkazující na hello `Result` vlastnost nebo volání `Wait` metodu `Task` instance.</span><span class="sxs-lookup"><span data-stu-id="8c277-161">However, always avoid referencing hello `Result` property or calling `Wait` method on a `Task` instance.</span></span> <span data-ttu-id="8c277-162">Tento přístup může vést toothread vyčerpání.</span><span class="sxs-lookup"><span data-stu-id="8c277-162">This approach can lead toothread exhaustion.</span></span>


[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="next-steps"></a><span data-ttu-id="8c277-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c277-163">Next steps</span></span>
<span data-ttu-id="8c277-164">Další informace najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="8c277-164">For more information, see hello following resources:</span></span>

<span data-ttu-id="8c277-165">Protože Azure Functions využívá Azure App Service také měli být vědomi pokyny služby App Service.</span><span class="sxs-lookup"><span data-stu-id="8c277-165">Because Azure Functions uses Azure App Service, you should also be aware of  App Service guidelines.</span></span>
* [<span data-ttu-id="8c277-166">Patterns and Practices optimalizací výkonu protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="8c277-166">Patterns and Practices HTTP Performance Optimizations</span></span>](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)

