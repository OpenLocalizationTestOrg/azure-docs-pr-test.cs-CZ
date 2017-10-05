---
title: "Optimalizace Azure kódu v sadě Visual Studio | Microsoft Docs"
description: "Další informace o tom, jak Azure kódu optimalizace nástroje v sadě Visual Studio pomáhat kódu robustnější a lépe provádění."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: ed48ee06-e2d2-4322-af22-07200fb16987
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 8f145502a856798d6e69ac11f324c72fa23f938e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="optimizing-your-azure-code"></a><span data-ttu-id="6026f-103">Optimalizace Azure kódu</span><span class="sxs-lookup"><span data-stu-id="6026f-103">Optimizing Your Azure Code</span></span>
<span data-ttu-id="6026f-104">Pokud jste programování aplikací, které používají Microsoft Azure, nejsou některé osvědčených kódovacích postupů, které byste měli postupovat, abyste se vyhnuli problémy s aplikací, škálovatelnost, chování a výkonu v cloudovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="6026f-104">When you’re programming apps that use Microsoft Azure, there are some coding practices you should follow to help avoid problems with app scalability, behavior and performance in a cloud environment.</span></span> <span data-ttu-id="6026f-105">Společnost Microsoft poskytuje nástroj pro analýzu kódu Azure, rozpozná a identifikuje některé z těchto problémů obvykle došlo a můžete je vyřešit.</span><span class="sxs-lookup"><span data-stu-id="6026f-105">Microsoft provides an Azure Code Analysis tool that recognizes and identifies several of these commonly-encountered issues and helps you resolve them.</span></span> <span data-ttu-id="6026f-106">Můžete stáhnout nástroj v sadě Visual Studio prostřednictvím balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="6026f-106">You can download the tool in Visual Studio via NuGet.</span></span>

## <a name="azure-code-analysis-rules"></a><span data-ttu-id="6026f-107">Azure pravidel analýzy kódu</span><span class="sxs-lookup"><span data-stu-id="6026f-107">Azure Code Analysis rules</span></span>
<span data-ttu-id="6026f-108">Nástroj pro analýzu kódu Azure automaticky příznak Azure kódu, pokud najde známé problémy výkonových používá následující pravidla.</span><span class="sxs-lookup"><span data-stu-id="6026f-108">The Azure Code Analysis tool uses the following rules to automatically flag your Azure code when it finds known performance-impacting issues.</span></span> <span data-ttu-id="6026f-109">Zjistil problémy se zobrazují jako upozornění nebo chyby kompilátoru.</span><span class="sxs-lookup"><span data-stu-id="6026f-109">Detected issues appear as a warnings or compiler errors.</span></span> <span data-ttu-id="6026f-110">Prostřednictvím ikonou žárovky často zadat kód opravy nebo návrhy k vyřešení upozornění nebo chyby.</span><span class="sxs-lookup"><span data-stu-id="6026f-110">Code fixes or suggestions to resolve the warning or error are often provided through a light bulb icon.</span></span>

## <a name="avoid-using-default-in-process-session-state-mode"></a><span data-ttu-id="6026f-111">Nepoužívejte výchozí režim stavu relace (v procesu)</span><span class="sxs-lookup"><span data-stu-id="6026f-111">Avoid using default (in-process) session state mode</span></span>
### <a name="id"></a><span data-ttu-id="6026f-112">ID</span><span class="sxs-lookup"><span data-stu-id="6026f-112">ID</span></span>
<span data-ttu-id="6026f-113">AP0000</span><span class="sxs-lookup"><span data-stu-id="6026f-113">AP0000</span></span>

### <a name="description"></a><span data-ttu-id="6026f-114">Popis</span><span class="sxs-lookup"><span data-stu-id="6026f-114">Description</span></span>
<span data-ttu-id="6026f-115">Pokud používáte výchozí režim stavu relace (v rámci procesu) pro cloudové aplikace, může dojít ke ztrátě stavu relace.</span><span class="sxs-lookup"><span data-stu-id="6026f-115">If you use the default (in-process) session state mode for cloud applications, you can lose session state.</span></span>

<span data-ttu-id="6026f-116">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="6026f-116">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="6026f-117">Důvod</span><span class="sxs-lookup"><span data-stu-id="6026f-117">Reason</span></span>
<span data-ttu-id="6026f-118">Ve výchozím nastavení je zadaný v souboru web.config režim stavu relace v procesu.</span><span class="sxs-lookup"><span data-stu-id="6026f-118">By default, the session state mode specified in the web.config file is in-process.</span></span> <span data-ttu-id="6026f-119">Navíc pokud žádný záznam, zadaný v konfiguračním souboru, režim stavu relace výchozí v procesu.</span><span class="sxs-lookup"><span data-stu-id="6026f-119">Also, if no entry specified in the configuration file, the Session State mode defaults to in-process.</span></span> <span data-ttu-id="6026f-120">Režim v procesu ukládá stav relace v paměti na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="6026f-120">The in-process mode stores session state in memory on the web server.</span></span> <span data-ttu-id="6026f-121">Při restartování instance nebo novou instanci se používá pro vyrovnávání zatížení nebo podporu převzetí služeb při selhání, není uložen stav relace, které jsou uložené v paměti na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="6026f-121">When an instance is restarted or a new instance is used for load balancing or failover support, the session state stored in memory on the web server isn’t saved.</span></span> <span data-ttu-id="6026f-122">Tato situace brání aplikaci v právě škálovatelné v cloudu.</span><span class="sxs-lookup"><span data-stu-id="6026f-122">This situation prevents the application from being scalable on the cloud.</span></span>

<span data-ttu-id="6026f-123">Stavu relace ASP.NET podporuje několik různých možností ukládání dat stavu relace: InProc, StateServer, SQL Server, vlastní a vypnutí.</span><span class="sxs-lookup"><span data-stu-id="6026f-123">ASP.NET session state supports several different storage options for session state data: InProc, StateServer, SQLServer, Custom, and Off.</span></span> <span data-ttu-id="6026f-124">Doporučujeme používat vlastní režim k hostování dat. na externí úložiště stavu relace, například [poskytovatele stavu relace Azure Redis](http://go.microsoft.com/fwlink/?LinkId=401521).</span><span class="sxs-lookup"><span data-stu-id="6026f-124">It’s recommended that you use Custom mode to host data on an external Session State store, such as [Azure Session State provider for Redis](http://go.microsoft.com/fwlink/?LinkId=401521).</span></span>

### <a name="solution"></a><span data-ttu-id="6026f-125">Řešení</span><span class="sxs-lookup"><span data-stu-id="6026f-125">Solution</span></span>
<span data-ttu-id="6026f-126">Jeden doporučené řešení je k ukládání stavu relace v mezipaměti spravované služby.</span><span class="sxs-lookup"><span data-stu-id="6026f-126">One recommended solution is to store session state on a managed cache service.</span></span> <span data-ttu-id="6026f-127">Další informace o použití [poskytovatele stavu relace Azure Redis](http://go.microsoft.com/fwlink/?LinkId=401521) k uložení stavu vaší relace.</span><span class="sxs-lookup"><span data-stu-id="6026f-127">Learn how to use [Azure Session State provider for Redis](http://go.microsoft.com/fwlink/?LinkId=401521) to store your session state.</span></span> <span data-ttu-id="6026f-128">Také můžete uložit stav relace v jiná místa a ujistěte se, že aplikace je škálovatelná v cloudu.</span><span class="sxs-lookup"><span data-stu-id="6026f-128">You can also store session state in other places to ensure your application is scalable on the cloud.</span></span> <span data-ttu-id="6026f-129">Další informace o alternativní řešení přečtěte si prosím [režim stavu relace](https://msdn.microsoft.com/library/ms178586).</span><span class="sxs-lookup"><span data-stu-id="6026f-129">To learn more about alternative solutions please read [Session State Modes](https://msdn.microsoft.com/library/ms178586).</span></span>

## <a name="run-method-should-not-be-async"></a><span data-ttu-id="6026f-130">Metoda spouštění by neměl být asynchronní</span><span class="sxs-lookup"><span data-stu-id="6026f-130">Run method should not be async</span></span>
### <a name="id"></a><span data-ttu-id="6026f-131">ID</span><span class="sxs-lookup"><span data-stu-id="6026f-131">ID</span></span>
<span data-ttu-id="6026f-132">AP1000</span><span class="sxs-lookup"><span data-stu-id="6026f-132">AP1000</span></span>

### <a name="description"></a><span data-ttu-id="6026f-133">Popis</span><span class="sxs-lookup"><span data-stu-id="6026f-133">Description</span></span>
<span data-ttu-id="6026f-134">Vytváření asynchronních metod (například [await](https://msdn.microsoft.com/library/hh156528.aspx)) mimo [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda a pak zavolají asynchronní metody z [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx).</span><span class="sxs-lookup"><span data-stu-id="6026f-134">Create asynchronous methods (such as [await](https://msdn.microsoft.com/library/hh156528.aspx)) outside of the [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method and then call the async methods from [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx).</span></span> <span data-ttu-id="6026f-135">Deklarování [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) jako asynchronní metodu způsobí, že role pracovního procesu zadat smyčku restartování.</span><span class="sxs-lookup"><span data-stu-id="6026f-135">Declaring the [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method as async causes the worker role to enter a restart loop.</span></span>

<span data-ttu-id="6026f-136">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="6026f-136">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="6026f-137">Důvod</span><span class="sxs-lookup"><span data-stu-id="6026f-137">Reason</span></span>
<span data-ttu-id="6026f-138">Volání asynchronních metod uvnitř [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda způsobí, že běh služby cloud recyklace role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="6026f-138">Calling async methods inside the [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method causes the cloud service runtime to recycle the worker role.</span></span> <span data-ttu-id="6026f-139">Když se spustí roli pracovního procesu, všechny spuštění programu probíhá uvnitř [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="6026f-139">When a worker role starts, all program execution takes place inside the [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method.</span></span> <span data-ttu-id="6026f-140">Ukončení [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda způsobí, že role pracovního procesu restartování.</span><span class="sxs-lookup"><span data-stu-id="6026f-140">Exiting the [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method causes the worker role to restart.</span></span> <span data-ttu-id="6026f-141">Pokud se modul runtime role pracovního procesu dotkne asynchronní metody, odešle zprávu všechny operace po asynchronní metody a vrátí.</span><span class="sxs-lookup"><span data-stu-id="6026f-141">When the worker role runtime hits the async method, it dispatches all operations after the async method and then returns.</span></span> <span data-ttu-id="6026f-142">To způsobí, že ukončíte z role pracovního procesu [ [ [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda a restartování.</span><span class="sxs-lookup"><span data-stu-id="6026f-142">This causes the worker role to exit from the [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method and restart.</span></span> <span data-ttu-id="6026f-143">V další iterace provádění role pracovního procesu dotkne asynchronní metody znovu a restartuje, způsobuje recyklace znovu i role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="6026f-143">In the next iteration of execution, the worker role hits the async method again and restarts, causing the worker role to recycle again as well.</span></span>

### <a name="solution"></a><span data-ttu-id="6026f-144">Řešení</span><span class="sxs-lookup"><span data-stu-id="6026f-144">Solution</span></span>
<span data-ttu-id="6026f-145">Umístit všechny asynchronní operace mimo [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="6026f-145">Place all async operations outside of the [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method.</span></span> <span data-ttu-id="6026f-146">Potom zavolejte rozdělili asynchronní metody z uvnitř [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metody, jako je .wait RunAsync ().</span><span class="sxs-lookup"><span data-stu-id="6026f-146">Then, call the refactored async method from inside the [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method, such as RunAsync().wait.</span></span> <span data-ttu-id="6026f-147">Nástroj pro analýzu kódu Azure můžete tento problém vyřešit.</span><span class="sxs-lookup"><span data-stu-id="6026f-147">The Azure Code Analysis tool can help you fix this issue.</span></span>

<span data-ttu-id="6026f-148">Následující fragment kódu ukazuje kód opravu tohoto problému:</span><span class="sxs-lookup"><span data-stu-id="6026f-148">The following code snippet demonstrates the code fix for this issue:</span></span>

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a><span data-ttu-id="6026f-149">Ověřování služby sběrnice sdíleného přístupového podpisu</span><span class="sxs-lookup"><span data-stu-id="6026f-149">Use Service Bus Shared Access Signature authentication</span></span>
### <a name="id"></a><span data-ttu-id="6026f-150">ID</span><span class="sxs-lookup"><span data-stu-id="6026f-150">ID</span></span>
<span data-ttu-id="6026f-151">AP2000</span><span class="sxs-lookup"><span data-stu-id="6026f-151">AP2000</span></span>

### <a name="description"></a><span data-ttu-id="6026f-152">Popis</span><span class="sxs-lookup"><span data-stu-id="6026f-152">Description</span></span>
<span data-ttu-id="6026f-153">Pomocí sdíleného přístupového podpisu (SAS) pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="6026f-153">Use Shared Access Signature (SAS) for authentication.</span></span> <span data-ttu-id="6026f-154">Služby Řízení přístupu (ACS) je zastaralá pro ověřování sběrnice služby.</span><span class="sxs-lookup"><span data-stu-id="6026f-154">Access Control Service (ACS) is being deprecated for service bus authentication.</span></span>

<span data-ttu-id="6026f-155">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="6026f-155">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="6026f-156">Důvod</span><span class="sxs-lookup"><span data-stu-id="6026f-156">Reason</span></span>
<span data-ttu-id="6026f-157">Pro zvýšení zabezpečení Azure Active Directory nahrazuje ověřování služby ACS se ověřování SAS.</span><span class="sxs-lookup"><span data-stu-id="6026f-157">For enhanced security, Azure Active Directory is replacing ACS authentication with SAS authentication.</span></span> <span data-ttu-id="6026f-158">V tématu [Azure Active Directory je budoucí ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) informace o plánu přechodu.</span><span class="sxs-lookup"><span data-stu-id="6026f-158">See [Azure Active Directory is the future of ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) for information on the transition plan.</span></span>

### <a name="solution"></a><span data-ttu-id="6026f-159">Řešení</span><span class="sxs-lookup"><span data-stu-id="6026f-159">Solution</span></span>
<span data-ttu-id="6026f-160">Používejte ověřování SAS ve svých aplikacích.</span><span class="sxs-lookup"><span data-stu-id="6026f-160">Use SAS authentication in your apps.</span></span> <span data-ttu-id="6026f-161">Následující příklad ukazuje, jak používat existující tokenu SAS pro přístup k oboru názvů service bus nebo entity.</span><span class="sxs-lookup"><span data-stu-id="6026f-161">The following example shows how to use an existing SAS token to access a service bus namespace or entity.</span></span>

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

<span data-ttu-id="6026f-162">Najdete v následujících tématech pro další informace.</span><span class="sxs-lookup"><span data-stu-id="6026f-162">See the following topics for more information.</span></span>

* <span data-ttu-id="6026f-163">Přehled najdete v tématu [ověřování sdíleného přístupového podpisu službou Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)</span><span class="sxs-lookup"><span data-stu-id="6026f-163">For an overview, see [Shared Access Signature Authentication with Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)</span></span>
* [<span data-ttu-id="6026f-164">Jak používat ověřování sdíleného přístupového podpisu službou Service Bus</span><span class="sxs-lookup"><span data-stu-id="6026f-164">How to use Shared Access Signature Authentication with Service Bus</span></span>](https://msdn.microsoft.com/library/dn205161.aspx)
* <span data-ttu-id="6026f-165">Ukázkový projekt, najdete v části [pomocí sdíleného přístupového podpisu (SAS) ověřování pomocí předplatných Service Bus](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)</span><span class="sxs-lookup"><span data-stu-id="6026f-165">For a sample project, see [Using Shared Access Signature (SAS) authentication with Service Bus Subscriptions](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)</span></span>

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a><span data-ttu-id="6026f-166">Zvažte použití metody OnMessage předejdete "přijímat smyčka"</span><span class="sxs-lookup"><span data-stu-id="6026f-166">Consider using OnMessage method to avoid "receive loop"</span></span>
### <a name="id"></a><span data-ttu-id="6026f-167">ID</span><span class="sxs-lookup"><span data-stu-id="6026f-167">ID</span></span>
<span data-ttu-id="6026f-168">AP2002</span><span class="sxs-lookup"><span data-stu-id="6026f-168">AP2002</span></span>

### <a name="description"></a><span data-ttu-id="6026f-169">Popis</span><span class="sxs-lookup"><span data-stu-id="6026f-169">Description</span></span>
<span data-ttu-id="6026f-170">Aby se zabránilo probíhající do "přijímat smyčky," volání **OnMessage** metoda je lepší řešení pro příjem zprávy než volání **Receive** metoda.</span><span class="sxs-lookup"><span data-stu-id="6026f-170">To avoid going into a "receive loop," calling the **OnMessage** method is a better solution for receiving messages than calling the **Receive** method.</span></span> <span data-ttu-id="6026f-171">Ale pokud je nutné použít **Receive** metodu a zadejte čas čekání serveru jiné než výchozí, zkontrolujte, že doba čekání serveru je více než jedna minuta.</span><span class="sxs-lookup"><span data-stu-id="6026f-171">However, if you must use the **Receive** method, and you specify a non-default server wait time, make sure the server wait time is more than one minute.</span></span>

<span data-ttu-id="6026f-172">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="6026f-172">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="6026f-173">Důvod</span><span class="sxs-lookup"><span data-stu-id="6026f-173">Reason</span></span>
<span data-ttu-id="6026f-174">Při volání metody **OnMessage**, klient spustí vnitřní zpráva některé čerpadlo, který neustále dotazuje fronty nebo předplatné.</span><span class="sxs-lookup"><span data-stu-id="6026f-174">When calling **OnMessage**, the client starts an internal message pump that constantly polls the queue or subscription.</span></span> <span data-ttu-id="6026f-175">Tato zpráva čerpadlo obsahuje nekonečnou smyčku, která vydává volání přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="6026f-175">This message pump contains an infinite loop that issues a call to receive messages.</span></span> <span data-ttu-id="6026f-176">Pokud vyprší časový limit volání, vydá nové volání.</span><span class="sxs-lookup"><span data-stu-id="6026f-176">If the call times out, it issues a new call.</span></span> <span data-ttu-id="6026f-177">Interval vypršení časového limitu je dáno hodnotu [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) vlastnost [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)který se používá.</span><span class="sxs-lookup"><span data-stu-id="6026f-177">The timeout interval is determined by the value of the [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) property of the [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)that’s being used.</span></span>

<span data-ttu-id="6026f-178">Výhodou použití **OnMessage** ve srovnání s **Receive** je, že uživatelé nemají k ručnímu dotazování na zprávy, zpracování výjimek, zpracovat více zpráv paralelně a dokončení zprávy.</span><span class="sxs-lookup"><span data-stu-id="6026f-178">The advantage of using **OnMessage** compared to **Receive** is that users don’t have to manually poll for messages, handle exceptions, process multiple messages in parallel, and complete the messages.</span></span>

<span data-ttu-id="6026f-179">Při volání **Receive** bez použití nastavení výchozí hodnoty, nezapomeňte *ServerWaitTime* hodnota je více než jedna minuta.</span><span class="sxs-lookup"><span data-stu-id="6026f-179">If you call **Receive** without using its default value, be sure the *ServerWaitTime* value is more than one minute.</span></span> <span data-ttu-id="6026f-180">Nastavení *ServerWaitTime* na více než jednu minutu zabrání serveru vypršel časový limit před plně doručení zprávy.</span><span class="sxs-lookup"><span data-stu-id="6026f-180">Setting *ServerWaitTime* to more than one minute prevents the server from timing out before the message is fully received.</span></span>

### <a name="solution"></a><span data-ttu-id="6026f-181">Řešení</span><span class="sxs-lookup"><span data-stu-id="6026f-181">Solution</span></span>
<span data-ttu-id="6026f-182">Podrobnosti viz následující příklady kódu pro použití, doporučené.</span><span class="sxs-lookup"><span data-stu-id="6026f-182">Please see the following code examples for recommended usages.</span></span> <span data-ttu-id="6026f-183">Další podrobnosti najdete v tématu [QueueClient.OnMessage – metoda (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)a [QueueClient.Receive – metoda (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).</span><span class="sxs-lookup"><span data-stu-id="6026f-183">For more details, see [QueueClient.OnMessage Method (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)and [QueueClient.Receive Method (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).</span></span>

<span data-ttu-id="6026f-184">Pro zlepšení výkonu infrastruktury zasílání zpráv Azure, najdete v části vzor návrhu [asynchronní zasílání zpráv Úvod do](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="6026f-184">To improve the performance of the Azure messaging infrastructure, see the design pattern [Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span>

<span data-ttu-id="6026f-185">Následuje příklad použití **OnMessage** přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="6026f-185">The following is an example of using **OnMessage** to receive messages.</span></span>

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

<span data-ttu-id="6026f-186">Následuje příklad použití **Receive** se serverem výchozí doba čekání.</span><span class="sxs-lookup"><span data-stu-id="6026f-186">The following is an example of using **Receive** with the default server wait time.</span></span>

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

<span data-ttu-id="6026f-187">Následuje příklad použití **Receive** serveru jiné než výchozí doba čekání.</span><span class="sxs-lookup"><span data-stu-id="6026f-187">The following is an example of using **Receive** with a non-default server wait time.</span></span>

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a><span data-ttu-id="6026f-188">Zvažte použití asynchronních metod Service Bus</span><span class="sxs-lookup"><span data-stu-id="6026f-188">Consider using asynchronous Service Bus methods</span></span>
### <a name="id"></a><span data-ttu-id="6026f-189">ID</span><span class="sxs-lookup"><span data-stu-id="6026f-189">ID</span></span>
<span data-ttu-id="6026f-190">AP2003</span><span class="sxs-lookup"><span data-stu-id="6026f-190">AP2003</span></span>

### <a name="description"></a><span data-ttu-id="6026f-191">Popis</span><span class="sxs-lookup"><span data-stu-id="6026f-191">Description</span></span>
<span data-ttu-id="6026f-192">Asynchronní metody Service Bus používejte ke zlepšení výkonu pomocí zprostředkované zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="6026f-192">Use asynchronous Service Bus methods to improve performance with brokered messaging.</span></span>

<span data-ttu-id="6026f-193">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="6026f-193">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="6026f-194">Důvod</span><span class="sxs-lookup"><span data-stu-id="6026f-194">Reason</span></span>
<span data-ttu-id="6026f-195">Použití asynchronních metod umožňuje souběžnosti program aplikace, protože provádění jednotlivých volání neblokuje hlavního vlákna.</span><span class="sxs-lookup"><span data-stu-id="6026f-195">Using asynchronous methods enables application program concurrency because executing each call doesn’t block the main thread.</span></span> <span data-ttu-id="6026f-196">Při použití služby Service Bus metody pro zasílání zpráv, provádění operace (odesílání, příjem, odstranit, apod) trvá určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="6026f-196">When using Service Bus messaging methods, performing an operation (send, receive, delete, etc.) takes time.</span></span> <span data-ttu-id="6026f-197">Tentokrát zahrnuje zpracování operace služby Service Bus kromě latence požadavku a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="6026f-197">This time includes the processing of the operation by the Service Bus service in addition to the latency of the request and the reply.</span></span> <span data-ttu-id="6026f-198">Pokud chcete zvýšit počet operací za čas, musí současně provést operace.</span><span class="sxs-lookup"><span data-stu-id="6026f-198">To increase the number of operations per time, operations must execute concurrently.</span></span> <span data-ttu-id="6026f-199">Další informace naleznete v [osvědčené postupy pro výkon vylepšení pomocí Service Bus zprostředkované zasílání zpráv](https://msdn.microsoft.com/library/azure/hh528527.aspx).</span><span class="sxs-lookup"><span data-stu-id="6026f-199">For more information please refer to [Best Practices for Performance Improvements Using Service Bus Brokered Messaging](https://msdn.microsoft.com/library/azure/hh528527.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="6026f-200">Řešení</span><span class="sxs-lookup"><span data-stu-id="6026f-200">Solution</span></span>
<span data-ttu-id="6026f-201">V tématu [QueueClient – třída (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) informace o tom, jak používat doporučené asynchronní metody.</span><span class="sxs-lookup"><span data-stu-id="6026f-201">See [QueueClient Class (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) for information about how to use the recommended asynchronous method.</span></span>

<span data-ttu-id="6026f-202">Pro zlepšení výkonu infrastruktury zasílání zpráv Azure, najdete v části vzor návrhu [asynchronní zasílání zpráv Úvod do](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="6026f-202">To improve the performance of the Azure messaging infrastructure, see the design pattern [Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span>

## <a name="consider-partitioning-service-bus-queues-and-topics"></a><span data-ttu-id="6026f-203">Vezměte v úvahu rozdělení fronty Service Bus a témat</span><span class="sxs-lookup"><span data-stu-id="6026f-203">Consider partitioning Service Bus queues and topics</span></span>
### <a name="id"></a><span data-ttu-id="6026f-204">ID</span><span class="sxs-lookup"><span data-stu-id="6026f-204">ID</span></span>
<span data-ttu-id="6026f-205">AP2004</span><span class="sxs-lookup"><span data-stu-id="6026f-205">AP2004</span></span>

### <a name="description"></a><span data-ttu-id="6026f-206">Popis</span><span class="sxs-lookup"><span data-stu-id="6026f-206">Description</span></span>
<span data-ttu-id="6026f-207">Oddíl fronty Service Bus a témat pro lepší výkon s zasílání zpráv Service Bus.</span><span class="sxs-lookup"><span data-stu-id="6026f-207">Partition Service Bus queues and topics for better performance with Service Bus messaging.</span></span>

<span data-ttu-id="6026f-208">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="6026f-208">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="6026f-209">Důvod</span><span class="sxs-lookup"><span data-stu-id="6026f-209">Reason</span></span>
<span data-ttu-id="6026f-210">Vytváření oddílů fronty sběrnice a témata zvýšíte dostupnost výkonu propustnost a služby, protože celkovou propustnost oddílů fronta nebo téma už není omezené podle výkonu zprostředkovatele jedné zprávy nebo úložišti pro přenos zpráv.</span><span class="sxs-lookup"><span data-stu-id="6026f-210">Partitioning Service Bus queues and topics increases performance throughput and service availability because the overall throughput of a partitioned queue or topic is no longer limited by the performance of a single message broker or messaging store.</span></span> <span data-ttu-id="6026f-211">Kromě toho dočasnému výpadku zasílání zpráv úložiště není znepřístupnit oddílů fronta nebo téma.</span><span class="sxs-lookup"><span data-stu-id="6026f-211">In addition, a temporary outage of a messaging store doesn’t make a partitioned queue or topic unavailable.</span></span> <span data-ttu-id="6026f-212">Další informace najdete v tématu [dělení entity zasílání zpráv](https://msdn.microsoft.com/library/azure/dn520246.aspx).</span><span class="sxs-lookup"><span data-stu-id="6026f-212">For more information, see [Partitioning Messaging Entities](https://msdn.microsoft.com/library/azure/dn520246.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="6026f-213">Řešení</span><span class="sxs-lookup"><span data-stu-id="6026f-213">Solution</span></span>
<span data-ttu-id="6026f-214">Následující fragment kódu ukazuje, jak oddílu entit pro zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="6026f-214">The following code snippet shows how to partition messaging entities.</span></span>

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

<span data-ttu-id="6026f-215">Další informace najdete v tématu [rozdělena na oddíly fronty služby Service Bus a témat | Microsoft Azure Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) a podívejte se [Microsoft Azure Service Bus rozdělena na oddíly fronty](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) ukázka.</span><span class="sxs-lookup"><span data-stu-id="6026f-215">For more information, see [Partitioned Service Bus Queues and Topics | Microsoft Azure Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) and check out the [Microsoft Azure Service Bus Partitioned Queue](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) sample.</span></span>

## <a name="do-not-set-sharedaccessstarttime"></a><span data-ttu-id="6026f-216">Nenastavujte SharedAccessStartTime</span><span class="sxs-lookup"><span data-stu-id="6026f-216">Do not set SharedAccessStartTime</span></span>
### <a name="id"></a><span data-ttu-id="6026f-217">ID</span><span class="sxs-lookup"><span data-stu-id="6026f-217">ID</span></span>
<span data-ttu-id="6026f-218">AP3001</span><span class="sxs-lookup"><span data-stu-id="6026f-218">AP3001</span></span>

### <a name="description"></a><span data-ttu-id="6026f-219">Popis</span><span class="sxs-lookup"><span data-stu-id="6026f-219">Description</span></span>
<span data-ttu-id="6026f-220">Byste neměli používat SharedAccessStartTimeset na aktuální čas k okamžitému spuštění zásady sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="6026f-220">You should avoid using SharedAccessStartTimeset to the current time to immediately start the Shared Access policy.</span></span> <span data-ttu-id="6026f-221">Stačí tuto vlastnost nastavit, pokud chcete spustit později zásady sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="6026f-221">You only need to set this property if you want to start the Shared Access policy at a later time.</span></span>

<span data-ttu-id="6026f-222">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="6026f-222">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="6026f-223">Důvod</span><span class="sxs-lookup"><span data-stu-id="6026f-223">Reason</span></span>
<span data-ttu-id="6026f-224">Synchronizace hodin způsobí, že mírné časový rozdíl mezi datovými centry.</span><span class="sxs-lookup"><span data-stu-id="6026f-224">Clock synchronization causes a slight time difference among datacenters.</span></span> <span data-ttu-id="6026f-225">Například by logicky domníváte, že nastavení času spuštění úložiště SAS zásady jako aktuální čas pomocí DateTime.Now nebo podobné metody způsobí, že zásady SAS tak, aby se projeví okamžitě.</span><span class="sxs-lookup"><span data-stu-id="6026f-225">For example, you would logically think setting the start time of a storage SAS policy as the current time by using DateTime.Now or a similar method will cause the SAS policy to take effect immediately.</span></span> <span data-ttu-id="6026f-226">Mírné časové rozdíly mezi datovými centry však může způsobit problémy s tím, vzhledem k tomu, že některé datacenter časy může být mírně pozdější než čas zahájení, zatímco jiní jej před jeho.</span><span class="sxs-lookup"><span data-stu-id="6026f-226">However, the slight time differences between datacenters can cause problems with this since some datacenter times might be slightly later than the start time, while others ahead of it.</span></span> <span data-ttu-id="6026f-227">V důsledku toho můžete zásady SAS vyprší rychle (nebo i okamžitě) Pokud je nastavená příliš krátká doba platnosti zásad.</span><span class="sxs-lookup"><span data-stu-id="6026f-227">As a result, the SAS policy can expire quickly (or even immediately) if the policy lifetime is set too short.</span></span>

<span data-ttu-id="6026f-228">Další informace o používání sdíleného přístupového podpisu na úložiště Azure najdete v části [představení tabulky SAS (sdíleného přístupového podpisu), fronta SAS a proveďte aktualizaci objektu Blob SAS – Blog týmu pro úložiště Azure Microsoft - lokality Domů - Blogy MSDN](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span><span class="sxs-lookup"><span data-stu-id="6026f-228">For more guidance on using Shared Access Signature on Azure storage, see [Introducing Table SAS (Shared Access Signature), Queue SAS and update to Blob SAS - Microsoft Azure Storage Team Blog - Site Home - MSDN Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="6026f-229">Řešení</span><span class="sxs-lookup"><span data-stu-id="6026f-229">Solution</span></span>
<span data-ttu-id="6026f-230">Odeberte příkaz, který nastaví čas zahájení zásady sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="6026f-230">Remove the statement that sets the start time of the shared access policy.</span></span> <span data-ttu-id="6026f-231">Nástroj pro analýzu kódu Azure poskytuje opravu tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="6026f-231">The Azure Code Analysis tool provides a fix for this issue.</span></span> <span data-ttu-id="6026f-232">Další informace o správu zabezpečení, najdete v tématu vzor návrhu [vzor klíč osobní služby](https://msdn.microsoft.com/library/dn568102.aspx).</span><span class="sxs-lookup"><span data-stu-id="6026f-232">For more information on security management, please see the design pattern [Valet Key Pattern](https://msdn.microsoft.com/library/dn568102.aspx).</span></span>

<span data-ttu-id="6026f-233">Následující fragment kódu ukazuje opravu kód pro tento problém.</span><span class="sxs-lookup"><span data-stu-id="6026f-233">The following code snippet demonstrates the code fix for this issue.</span></span>

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a><span data-ttu-id="6026f-234">Sdílené zásady přístupu čas vypršení platnosti musí být delší než 5 minut</span><span class="sxs-lookup"><span data-stu-id="6026f-234">Shared Access Policy expiry time must be more than five minutes</span></span>
### <a name="id"></a><span data-ttu-id="6026f-235">ID</span><span class="sxs-lookup"><span data-stu-id="6026f-235">ID</span></span>
<span data-ttu-id="6026f-236">AP3002</span><span class="sxs-lookup"><span data-stu-id="6026f-236">AP3002</span></span>

### <a name="description"></a><span data-ttu-id="6026f-237">Popis</span><span class="sxs-lookup"><span data-stu-id="6026f-237">Description</span></span>
<span data-ttu-id="6026f-238">Může být co nejvíce pět minut rozdíl v hodiny mezi datovými centry v různých umístěních kvůli Stav známý jako "posun hodin."</span><span class="sxs-lookup"><span data-stu-id="6026f-238">There can be as much as a five minute difference in clocks among datacenters at different locations due to a condition known as "clock skew."</span></span> <span data-ttu-id="6026f-239">Abyste zabránili SAS token zásady vypršení platnosti dříve, než plánované, nastavte čas vypršení platnosti být více než pět minut.</span><span class="sxs-lookup"><span data-stu-id="6026f-239">To prevent the SAS policy token from expiring earlier than planned, set the expiry time to be more than five minutes.</span></span>

<span data-ttu-id="6026f-240">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="6026f-240">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="6026f-241">Důvod</span><span class="sxs-lookup"><span data-stu-id="6026f-241">Reason</span></span>
<span data-ttu-id="6026f-242">Signál hodiny synchronizovat datových center v různých umístěních po celém světě.</span><span class="sxs-lookup"><span data-stu-id="6026f-242">Datacenters at different locations around the world synchronize by a clock signal.</span></span> <span data-ttu-id="6026f-243">Protože trvá dobu hodiny signál k dostavit do různých umístění, může být čas odchylka mezi datovými centry v různých geografických umístěních i když všechno, co je zřejmě synchronizován.</span><span class="sxs-lookup"><span data-stu-id="6026f-243">Because it takes time for clock signal to travel to different locations, there can be a time variance between datacenters at different geographical locations although everything is supposedly synchronized.</span></span> <span data-ttu-id="6026f-244">Tento časový rozdíl může ovlivnit sdíleného přístupu zásad počáteční čas a vypršení platnosti interval.</span><span class="sxs-lookup"><span data-stu-id="6026f-244">This time difference can affect the Shared Access policy start time and expiration interval.</span></span> <span data-ttu-id="6026f-245">Proto aby zásady sdíleného přístupu se okamžitě projeví, nezadávejte čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="6026f-245">Therefore, to ensure Shared Access policy takes effect immediately, don’t specify the start time.</span></span> <span data-ttu-id="6026f-246">Kromě toho zajistěte, aby byl čas vypršení platnosti více než 5 minut, aby se zabránilo časná časový limit.</span><span class="sxs-lookup"><span data-stu-id="6026f-246">In addition, make sure the expiration time is more than 5 minutes to prevent early timeout.</span></span>

<span data-ttu-id="6026f-247">Další informace o používání sdíleného přístupového podpisu na úložiště Azure najdete v tématu [představení tabulky SAS (sdíleného přístupového podpisu), fronta SAS a proveďte aktualizaci objektu Blob SAS – Blog týmu pro úložiště Azure Microsoft - lokality Domů - Blogy MSDN](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span><span class="sxs-lookup"><span data-stu-id="6026f-247">For more information about using Shared Access Signature on Azure storage, see [Introducing Table SAS (Shared Access Signature), Queue SAS and update to Blob SAS - Microsoft Azure Storage Team Blog - Site Home - MSDN Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="6026f-248">Řešení</span><span class="sxs-lookup"><span data-stu-id="6026f-248">Solution</span></span>
<span data-ttu-id="6026f-249">Další informace o správu zabezpečení, najdete v části vzor návrhu [vzor klíč osobní služby](https://msdn.microsoft.com/library/dn568102.aspx).</span><span class="sxs-lookup"><span data-stu-id="6026f-249">For more information on security management, see the design pattern [Valet Key Pattern](https://msdn.microsoft.com/library/dn568102.aspx).</span></span>

<span data-ttu-id="6026f-250">Následuje příklad není zadávání čas spuštění zásady sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="6026f-250">The following is an example of not specifying a Shared Access policy start time.</span></span>

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

<span data-ttu-id="6026f-251">Následuje příklad zadání čas spuštění zásady sdíleného přístupu s doba vypršení platnosti zásada, která je větší než pět minut.</span><span class="sxs-lookup"><span data-stu-id="6026f-251">The following is an example of specifying a Shared Access policy start time with a policy expiration duration greater than five minutes.</span></span>

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

<span data-ttu-id="6026f-252">Další informace najdete v tématu [vytváření a používání sdíleného přístupového podpisu](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span><span class="sxs-lookup"><span data-stu-id="6026f-252">For more information, see [Create and Use a Shared Access Signature](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span></span>

## <a name="use-cloudconfigurationmanager"></a><span data-ttu-id="6026f-253">Použití CloudConfigurationManager</span><span class="sxs-lookup"><span data-stu-id="6026f-253">Use CloudConfigurationManager</span></span>
### <a name="id"></a><span data-ttu-id="6026f-254">ID</span><span class="sxs-lookup"><span data-stu-id="6026f-254">ID</span></span>
<span data-ttu-id="6026f-255">AP4000</span><span class="sxs-lookup"><span data-stu-id="6026f-255">AP4000</span></span>

### <a name="description"></a><span data-ttu-id="6026f-256">Popis</span><span class="sxs-lookup"><span data-stu-id="6026f-256">Description</span></span>
<span data-ttu-id="6026f-257">Pomocí [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) třídy pro projekty, jako je Azure web a mobilní služby Azure nebude znamenat problémy modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="6026f-257">Using the [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) class for projects such as Azure Website and Azure mobile services won't introduce runtime issues.</span></span> <span data-ttu-id="6026f-258">Jako osvědčený postup je však vhodné použít cloudové[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) jako jednotné způsob správy konfigurace pro všechny aplikace cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="6026f-258">As a best practice, however, it's a good idea to use Cloud[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) as a unified way of managing configurations for all Azure Cloud applications.</span></span>

<span data-ttu-id="6026f-259">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="6026f-259">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="6026f-260">Důvod</span><span class="sxs-lookup"><span data-stu-id="6026f-260">Reason</span></span>
<span data-ttu-id="6026f-261">CloudConfigurationManager přečte vhodné prostředí aplikace konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="6026f-261">CloudConfigurationManager reads the configuration file appropriate to the application environment.</span></span>

[<span data-ttu-id="6026f-262">CloudConfigurationManager</span><span class="sxs-lookup"><span data-stu-id="6026f-262">CloudConfigurationManager</span></span>](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a><span data-ttu-id="6026f-263">Řešení</span><span class="sxs-lookup"><span data-stu-id="6026f-263">Solution</span></span>
<span data-ttu-id="6026f-264">Refaktorovat kód pro použití [třída CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="6026f-264">Refactor your code to use the [CloudConfigurationManager Class](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx).</span></span> <span data-ttu-id="6026f-265">Nástroj pro analýzu kódu Azure zajišťuje opravy kódu pro tento problém.</span><span class="sxs-lookup"><span data-stu-id="6026f-265">A code fix for this issue is provided by the Azure Code Analysis tool.</span></span>

<span data-ttu-id="6026f-266">Následující fragment kódu ukazuje opravu kód pro tento problém.</span><span class="sxs-lookup"><span data-stu-id="6026f-266">The following code snippet demonstrates the code fix for this issue.</span></span> <span data-ttu-id="6026f-267">Nahradit</span><span class="sxs-lookup"><span data-stu-id="6026f-267">Replace</span></span>

`var settings = ConfigurationManager.AppSettings["mySettings"];`

<span data-ttu-id="6026f-268">S</span><span class="sxs-lookup"><span data-stu-id="6026f-268">with</span></span>

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

<span data-ttu-id="6026f-269">Tady je příklad toho, jak uložit nastavení konfigurace do souboru App.config nebo Web.config.</span><span class="sxs-lookup"><span data-stu-id="6026f-269">Here's an example of how to store the configuration setting in a App.config or Web.config file.</span></span> <span data-ttu-id="6026f-270">Přidejte nastavení do oddílu appSettings konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="6026f-270">Add the settings to the appSettings section of the configuration file.</span></span> <span data-ttu-id="6026f-271">Zde je soubor Web.config pro předchozí příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="6026f-271">The following is the Web.config file for the previous code example.</span></span>

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a><span data-ttu-id="6026f-272">Nepoužívejte pevně připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="6026f-272">Avoid using hard-coded connection strings</span></span>
### <a name="id"></a><span data-ttu-id="6026f-273">ID</span><span class="sxs-lookup"><span data-stu-id="6026f-273">ID</span></span>
<span data-ttu-id="6026f-274">AP4001</span><span class="sxs-lookup"><span data-stu-id="6026f-274">AP4001</span></span>

### <a name="description"></a><span data-ttu-id="6026f-275">Popis</span><span class="sxs-lookup"><span data-stu-id="6026f-275">Description</span></span>
<span data-ttu-id="6026f-276">Pokud používáte pevně připojovací řetězce a je potřeba je aktualizovat později, budete muset provést změny vašeho zdrojového kódu a překompilování aplikace.</span><span class="sxs-lookup"><span data-stu-id="6026f-276">If you use hard-coded connection strings and you need to update them later, you’ll have to make changes to your source code and recompile the application.</span></span> <span data-ttu-id="6026f-277">Ale pokud uchováváte připojovací řetězce v konfiguračním souboru, můžete je později změníte jednoduše aktualizací konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="6026f-277">However, if you store your connection strings in a configuration file, you can change them later by simply updating the configuration file.</span></span>

<span data-ttu-id="6026f-278">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="6026f-278">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="6026f-279">Důvod</span><span class="sxs-lookup"><span data-stu-id="6026f-279">Reason</span></span>
<span data-ttu-id="6026f-280">Pevně kódováno připojovací řetězce je chybný postup, protože by to zavedlo problémy, když je potřeba rychle změnit připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="6026f-280">Hard-coding connection strings is a bad practice because it introduces problems when connection strings need to be changed quickly.</span></span> <span data-ttu-id="6026f-281">Kromě toho pokud projektu musí být vráceny se změnami do správy zdrojového kódu, zavést pevně připojovací řetězce ohrožení zabezpečení, protože řetězce lze zobrazit ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="6026f-281">In addition, if the project needs to be checked in to source control, hard-coded connection strings introduce security vulnerabilities since the strings can be viewed in the source code.</span></span>

### <a name="solution"></a><span data-ttu-id="6026f-282">Řešení</span><span class="sxs-lookup"><span data-stu-id="6026f-282">Solution</span></span>
<span data-ttu-id="6026f-283">Ukládání připojovacích řetězců v prostředí Azure na konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="6026f-283">Store connection strings in the configuration files or Azure environments.</span></span>

* <span data-ttu-id="6026f-284">Pro samostatné aplikace pomocí souboru app.config uložit nastavení připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="6026f-284">For standalone applications, use app.config to store connection string settings.</span></span>
* <span data-ttu-id="6026f-285">Pro hostované službou IIS webových aplikací použijte soubor web.config k ukládání připojovacích řetězců.</span><span class="sxs-lookup"><span data-stu-id="6026f-285">For IIS-hosted web applications, use web.config to store connection strings.</span></span>
* <span data-ttu-id="6026f-286">Pro aplikace ASP.NET vNext použijte k ukládání připojovacích řetězců configuration.json.</span><span class="sxs-lookup"><span data-stu-id="6026f-286">For ASP.NET vNext applications, use configuration.json to store connection strings.</span></span>

<span data-ttu-id="6026f-287">Informace o používání soubory konfigurace, jako je soubor web.config nebo app.config najdete v tématu [pokyny pro ASP.NET Web konfigurace](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx).</span><span class="sxs-lookup"><span data-stu-id="6026f-287">For information on using configurations files such as web.config or app.config, see [ASP.NET Web Configuration Guidelines](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx).</span></span> <span data-ttu-id="6026f-288">Informace o tom, jak Azure pracovní proměnné prostředí, najdete v části [weby Azure: fungování řetězců aplikace a připojovací řetězce fungovat](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span><span class="sxs-lookup"><span data-stu-id="6026f-288">For information on how Azure environment variables work, see [Azure Web Sites: How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span></span> <span data-ttu-id="6026f-289">Informace týkající se ukládání připojovací řetězec do správy zdrojového kódu najdete v tématu [neukládejte citlivé informace, jako je například připojovací řetězce v souborech, které jsou uložené v úložiště zdrojového kódu](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).</span><span class="sxs-lookup"><span data-stu-id="6026f-289">For information on storing connection string in source control, see [avoid putting sensitive information such as connection strings in files that are stored in source code repository](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).</span></span>

## <a name="use-diagnostics-configuration-file"></a><span data-ttu-id="6026f-290">Použijte diagnostiku konfigurační soubor</span><span class="sxs-lookup"><span data-stu-id="6026f-290">Use diagnostics configuration file</span></span>
### <a name="id"></a><span data-ttu-id="6026f-291">ID</span><span class="sxs-lookup"><span data-stu-id="6026f-291">ID</span></span>
<span data-ttu-id="6026f-292">AP5000</span><span class="sxs-lookup"><span data-stu-id="6026f-292">AP5000</span></span>

### <a name="description"></a><span data-ttu-id="6026f-293">Popis</span><span class="sxs-lookup"><span data-stu-id="6026f-293">Description</span></span>
<span data-ttu-id="6026f-294">Namísto konfigurace nastavení diagnostiky ve vašem kódu, například pomocí Microsoft.WindowsAzure.Diagnostics programovací rozhraní API, měli byste nakonfigurovat nastavení diagnostiky v souboru diagnostics.wadcfg.</span><span class="sxs-lookup"><span data-stu-id="6026f-294">Instead of configuring diagnostics settings in your code such as by using the Microsoft.WindowsAzure.Diagnostics programming API, you should configure diagnostics settings in the diagnostics.wadcfg file.</span></span> <span data-ttu-id="6026f-295">(Nebo, pokud používáte Azure SDK 2.5 diagnostics.wadcfgx).</span><span class="sxs-lookup"><span data-stu-id="6026f-295">(Or, diagnostics.wadcfgx if you use Azure SDK 2.5).</span></span> <span data-ttu-id="6026f-296">Díky tomu můžete změnit nastavení diagnostiky bez nutnosti její kompilace kódu.</span><span class="sxs-lookup"><span data-stu-id="6026f-296">By doing this, you can change diagnostics settings without having to recompile your code.</span></span>

<span data-ttu-id="6026f-297">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="6026f-297">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="6026f-298">Důvod</span><span class="sxs-lookup"><span data-stu-id="6026f-298">Reason</span></span>
<span data-ttu-id="6026f-299">Předtím, než pomocí několika různých metod se daly konfigurovat Azure SDK 2.5, (která používá Azure diagnostics 1.3), Azure Diagnostics (WAD): přidání do objektu blob konfigurace v úložišti pomocí imperativní kódu, deklarativní konfigurace nebo výchozí konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6026f-299">Before Azure SDK 2.5 (which uses Azure diagnostics 1.3), Azure Diagnostics (WAD) could be configured by using several different methods: adding it to the configuration blob in storage, by using imperative code, declarative configuration, or the default configuration.</span></span> <span data-ttu-id="6026f-300">Upřednostňovaný způsob konfigurace diagnostiky je však můžete použít soubor XML konfigurace (diagnostics.wadcfg nebo diagnositcs.wadcfgx pro sadu SDK, 2.5 a novější) v projektu aplikace.</span><span class="sxs-lookup"><span data-stu-id="6026f-300">However, the preferred way to configure diagnostics is to use an XML configuration file (diagnostics.wadcfg or diagnositcs.wadcfgx for SDK 2.5 and later) in the application project.</span></span> <span data-ttu-id="6026f-301">V tomto přístupu soubor diagnostics.wadcfg úplně definuje konfiguraci a lze aktualizovat a znovu nasazena na bude.</span><span class="sxs-lookup"><span data-stu-id="6026f-301">In this approach, the diagnostics.wadcfg file completely defines the configuration and can be updated and redeployed at will.</span></span> <span data-ttu-id="6026f-302">Kombinování použití konfiguračního souboru diagnostics.wadcfg pomocí programový metod nastavení konfigurace pomocí [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)nebo [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)třídy může vést k nejasnostem.</span><span class="sxs-lookup"><span data-stu-id="6026f-302">Mixing the use of the diagnostics.wadcfg configuration file with the programmatic methods of setting configurations by using the [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)or [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)classes can lead to confusion.</span></span> <span data-ttu-id="6026f-303">V tématu [inicializovat nebo změna konfigurace diagnostiky Azure](https://msdn.microsoft.com/library/azure/hh411537.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="6026f-303">See [Initialize or Change Azure Diagnostics Configuration](https://msdn.microsoft.com/library/azure/hh411537.aspx) for more information.</span></span>

<span data-ttu-id="6026f-304">Počínaje WAD 1.3 (zahrnutá v Azure SDK 2.5), je již nebude možné použít ke konfiguraci diagnostiky kódu.</span><span class="sxs-lookup"><span data-stu-id="6026f-304">Beginning with WAD 1.3 (included with Azure SDK 2.5), it’s no longer possible to use code to configure diagnostics.</span></span> <span data-ttu-id="6026f-305">V důsledku toho můžete zadat pouze při použití nebo aktualizaci rozšíření diagnostiky konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6026f-305">As a result, you can only provide the configuration when applying or updating the diagnostics extension.</span></span>

### <a name="solution"></a><span data-ttu-id="6026f-306">Řešení</span><span class="sxs-lookup"><span data-stu-id="6026f-306">Solution</span></span>
<span data-ttu-id="6026f-307">Pomocí návrháře konfigurace diagnostiky přesunutí nastavení pro diagnostiku do konfiguračního souboru diagnostiky (diagnositcs.wadcfg nebo diagnositcs.wadcfgx pro sadu SDK, 2.5 a novější).</span><span class="sxs-lookup"><span data-stu-id="6026f-307">Use the diagnostics configuration designer to move diagnostic settings to the diagnostics configuration file (diagnositcs.wadcfg or diagnositcs.wadcfgx for SDK 2.5 and later).</span></span> <span data-ttu-id="6026f-308">Doporučujeme také nainstalovat [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) a použít nejnovější funkce diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="6026f-308">It’s also recommended that you install [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) and use the latest diagnostics feature.</span></span>

1. <span data-ttu-id="6026f-309">V místní nabídce pro roli, kterou chcete nakonfigurovat, vyberte vlastnosti a potom vyberte na kartě konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6026f-309">On the shortcut menu for the role that you want to configure, choose Properties, and then choose the Configuration tab.</span></span>
2. <span data-ttu-id="6026f-310">V **diagnostiky** část, ujistěte se, že **povolení diagnostiky** je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="6026f-310">In the **Diagnostics** section, make sure that the **Enable Diagnostics** check box is selected.</span></span>
3. <span data-ttu-id="6026f-311">Vyberte **konfigurace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6026f-311">Choose the **Configure** button.</span></span>

   ![Přístup k možnost povolení diagnostiky](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

   <span data-ttu-id="6026f-313">V tématu [konfigurace diagnostiky pro Azure Cloud Services a virtuální počítače](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="6026f-313">See [Configuring Diagnostics for Azure Cloud Services and Virtual Machines](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) for more information.</span></span>

## <a name="avoid-declaring-dbcontext-objects-as-static"></a><span data-ttu-id="6026f-314">Vyhněte se deklarace objektů DbContext jako statické</span><span class="sxs-lookup"><span data-stu-id="6026f-314">Avoid declaring DbContext objects as static</span></span>
### <a name="id"></a><span data-ttu-id="6026f-315">ID</span><span class="sxs-lookup"><span data-stu-id="6026f-315">ID</span></span>
<span data-ttu-id="6026f-316">AP6000</span><span class="sxs-lookup"><span data-stu-id="6026f-316">AP6000</span></span>

### <a name="description"></a><span data-ttu-id="6026f-317">Popis</span><span class="sxs-lookup"><span data-stu-id="6026f-317">Description</span></span>
<span data-ttu-id="6026f-318">Pokud chcete uložit paměti, vyhněte se deklarace objektů DBContext jako statické.</span><span class="sxs-lookup"><span data-stu-id="6026f-318">To save memory, avoid declaring DBContext objects as static.</span></span>

<span data-ttu-id="6026f-319">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="6026f-319">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="6026f-320">Důvod</span><span class="sxs-lookup"><span data-stu-id="6026f-320">Reason</span></span>
<span data-ttu-id="6026f-321">Objekty DBContext ukládání výsledků dotazu z každé volání.</span><span class="sxs-lookup"><span data-stu-id="6026f-321">DBContext objects hold the query results from each call.</span></span> <span data-ttu-id="6026f-322">Statické DBContext objekty nebyly použity, dokud doménu aplikace je odpojen.</span><span class="sxs-lookup"><span data-stu-id="6026f-322">Static DBContext objects are not disposed until the application domain is unloaded.</span></span> <span data-ttu-id="6026f-323">Proto statický objekt DBContext spotřebovat velké objemy paměti.</span><span class="sxs-lookup"><span data-stu-id="6026f-323">Therefore, a static DBContext object can consume large amounts of memory.</span></span>

### <a name="solution"></a><span data-ttu-id="6026f-324">Řešení</span><span class="sxs-lookup"><span data-stu-id="6026f-324">Solution</span></span>
<span data-ttu-id="6026f-325">Jako místní proměnné nebo pole instance nestatické deklarovat DBContext, použijte pro úlohy a nechat ji bude zrušen z po použití.</span><span class="sxs-lookup"><span data-stu-id="6026f-325">Declare DBContext as a local variable or non-static instance field, use it for a task, and then let it be disposed of after use.</span></span>

<span data-ttu-id="6026f-326">V následujícím příkladu třída controller MVC ukazuje, jak použít objekt DBContext.</span><span class="sxs-lookup"><span data-stu-id="6026f-326">The following example MVC controller class shows how to use the DBContext object.</span></span>

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a><span data-ttu-id="6026f-327">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6026f-327">Next steps</span></span>
<span data-ttu-id="6026f-328">Další informace o optimalizaci a řešení potíží s aplikací Azure najdete v tématu [řešení potíží s webovou aplikaci v Azure App Service pomocí sady Visual Studio](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="6026f-328">To learn more about optimizing and troubleshooting Azure apps, see [Troubleshoot a web app in Azure App Service using Visual Studio](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span></span>
