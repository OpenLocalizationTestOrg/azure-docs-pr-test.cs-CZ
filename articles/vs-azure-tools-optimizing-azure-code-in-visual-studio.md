---
title: "aaaOptimizing vaše Azure kódu v sadě Visual Studio | Microsoft Docs"
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
ms.openlocfilehash: 7df932def9dc16c93de29fc6a77c8fc121fda338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimizing-your-azure-code"></a><span data-ttu-id="31b02-103">Optimalizace Azure kódu</span><span class="sxs-lookup"><span data-stu-id="31b02-103">Optimizing Your Azure Code</span></span>
<span data-ttu-id="31b02-104">Pokud jste programování aplikací, které používají Microsoft Azure, nejsou některé postupy kódování, postupujte podle toohelp vyhnout problémům s aplikace škálovatelnost, chování a výkonu v cloudovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="31b02-104">When you’re programming apps that use Microsoft Azure, there are some coding practices you should follow toohelp avoid problems with app scalability, behavior and performance in a cloud environment.</span></span> <span data-ttu-id="31b02-105">Společnost Microsoft poskytuje nástroj pro analýzu kódu Azure, rozpozná a identifikuje některé z těchto problémů obvykle došlo a můžete je vyřešit.</span><span class="sxs-lookup"><span data-stu-id="31b02-105">Microsoft provides an Azure Code Analysis tool that recognizes and identifies several of these commonly-encountered issues and helps you resolve them.</span></span> <span data-ttu-id="31b02-106">Můžete si stáhnout nástroj hello v sadě Visual Studio prostřednictvím balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="31b02-106">You can download hello tool in Visual Studio via NuGet.</span></span>

## <a name="azure-code-analysis-rules"></a><span data-ttu-id="31b02-107">Azure pravidel analýzy kódu</span><span class="sxs-lookup"><span data-stu-id="31b02-107">Azure Code Analysis rules</span></span>
<span data-ttu-id="31b02-108">Nástroj pro analýzu kódu Azure Hello používá následující pravidla tooautomatically příznak Azure kódu, pokud najde známé problémy výkonových hello.</span><span class="sxs-lookup"><span data-stu-id="31b02-108">hello Azure Code Analysis tool uses hello following rules tooautomatically flag your Azure code when it finds known performance-impacting issues.</span></span> <span data-ttu-id="31b02-109">Zjistil problémy se zobrazují jako upozornění nebo chyby kompilátoru.</span><span class="sxs-lookup"><span data-stu-id="31b02-109">Detected issues appear as a warnings or compiler errors.</span></span> <span data-ttu-id="31b02-110">Kód opravy nebo návrhy tooresolve hello upozornění nebo chyby jsou často zajišťováno prostřednictvím ikonou žárovky.</span><span class="sxs-lookup"><span data-stu-id="31b02-110">Code fixes or suggestions tooresolve hello warning or error are often provided through a light bulb icon.</span></span>

## <a name="avoid-using-default-in-process-session-state-mode"></a><span data-ttu-id="31b02-111">Nepoužívejte výchozí režim stavu relace (v procesu)</span><span class="sxs-lookup"><span data-stu-id="31b02-111">Avoid using default (in-process) session state mode</span></span>
### <a name="id"></a><span data-ttu-id="31b02-112">ID</span><span class="sxs-lookup"><span data-stu-id="31b02-112">ID</span></span>
<span data-ttu-id="31b02-113">AP0000</span><span class="sxs-lookup"><span data-stu-id="31b02-113">AP0000</span></span>

### <a name="description"></a><span data-ttu-id="31b02-114">Popis</span><span class="sxs-lookup"><span data-stu-id="31b02-114">Description</span></span>
<span data-ttu-id="31b02-115">Pokud používáte hello výchozí režim stavu relace (v procesu) pro cloudové aplikace, může dojít ke ztrátě stavu relace.</span><span class="sxs-lookup"><span data-stu-id="31b02-115">If you use hello default (in-process) session state mode for cloud applications, you can lose session state.</span></span>

<span data-ttu-id="31b02-116">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="31b02-116">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="31b02-117">Důvod</span><span class="sxs-lookup"><span data-stu-id="31b02-117">Reason</span></span>
<span data-ttu-id="31b02-118">Ve výchozím nastavení je režim stavu relace hello zadaný v souboru web.config hello v procesu.</span><span class="sxs-lookup"><span data-stu-id="31b02-118">By default, hello session state mode specified in hello web.config file is in-process.</span></span> <span data-ttu-id="31b02-119">Navíc pokud žádný záznam, zadaný v konfiguračním souboru hello, výchozí režim stavu relace hello tooin procesu.</span><span class="sxs-lookup"><span data-stu-id="31b02-119">Also, if no entry specified in hello configuration file, hello Session State mode defaults tooin-process.</span></span> <span data-ttu-id="31b02-120">režim v procesu Hello ukládá stav relace v paměti na webovém serveru hello.</span><span class="sxs-lookup"><span data-stu-id="31b02-120">hello in-process mode stores session state in memory on hello web server.</span></span> <span data-ttu-id="31b02-121">Při restartování instance nebo novou instanci se používá pro vyrovnávání zatížení nebo podporu převzetí služeb při selhání, stav relace hello uložené v paměti na webovém serveru hello se neuloží.</span><span class="sxs-lookup"><span data-stu-id="31b02-121">When an instance is restarted or a new instance is used for load balancing or failover support, hello session state stored in memory on hello web server isn’t saved.</span></span> <span data-ttu-id="31b02-122">Tato situace zabraňuje aplikace hello z důvodu škálovatelné cloudové hello.</span><span class="sxs-lookup"><span data-stu-id="31b02-122">This situation prevents hello application from being scalable on hello cloud.</span></span>

<span data-ttu-id="31b02-123">Stavu relace ASP.NET podporuje několik různých možností ukládání dat stavu relace: InProc, StateServer, SQL Server, vlastní a vypnutí.</span><span class="sxs-lookup"><span data-stu-id="31b02-123">ASP.NET session state supports several different storage options for session state data: InProc, StateServer, SQLServer, Custom, and Off.</span></span> <span data-ttu-id="31b02-124">Doporučuje se použít vlastní režimu toohost data na externí úložiště stavu relace, například [poskytovatele stavu relace Azure Redis](http://go.microsoft.com/fwlink/?LinkId=401521).</span><span class="sxs-lookup"><span data-stu-id="31b02-124">It’s recommended that you use Custom mode toohost data on an external Session State store, such as [Azure Session State provider for Redis](http://go.microsoft.com/fwlink/?LinkId=401521).</span></span>

### <a name="solution"></a><span data-ttu-id="31b02-125">Řešení</span><span class="sxs-lookup"><span data-stu-id="31b02-125">Solution</span></span>
<span data-ttu-id="31b02-126">Jeden doporučené řešení je stav relace toostore mezipaměti spravované služby.</span><span class="sxs-lookup"><span data-stu-id="31b02-126">One recommended solution is toostore session state on a managed cache service.</span></span> <span data-ttu-id="31b02-127">Zjistěte, jak toouse [poskytovatele stavu relace Azure Redis](http://go.microsoft.com/fwlink/?LinkId=401521) toostore vašemu stavu relace.</span><span class="sxs-lookup"><span data-stu-id="31b02-127">Learn how toouse [Azure Session State provider for Redis](http://go.microsoft.com/fwlink/?LinkId=401521) toostore your session state.</span></span> <span data-ttu-id="31b02-128">Vám může také úložiště relace stavu v jiných místech tooensure, že je v cloudu hello škálovatelné aplikace.</span><span class="sxs-lookup"><span data-stu-id="31b02-128">You can also store session state in other places tooensure your application is scalable on hello cloud.</span></span> <span data-ttu-id="31b02-129">Další informace o alternativní řešení přečtěte si prosím toolearn [režim stavu relace](https://msdn.microsoft.com/library/ms178586).</span><span class="sxs-lookup"><span data-stu-id="31b02-129">toolearn more about alternative solutions please read [Session State Modes](https://msdn.microsoft.com/library/ms178586).</span></span>

## <a name="run-method-should-not-be-async"></a><span data-ttu-id="31b02-130">Metoda spouštění by neměl být asynchronní</span><span class="sxs-lookup"><span data-stu-id="31b02-130">Run method should not be async</span></span>
### <a name="id"></a><span data-ttu-id="31b02-131">ID</span><span class="sxs-lookup"><span data-stu-id="31b02-131">ID</span></span>
<span data-ttu-id="31b02-132">AP1000</span><span class="sxs-lookup"><span data-stu-id="31b02-132">AP1000</span></span>

### <a name="description"></a><span data-ttu-id="31b02-133">Popis</span><span class="sxs-lookup"><span data-stu-id="31b02-133">Description</span></span>
<span data-ttu-id="31b02-134">Vytváření asynchronních metod (například [await](https://msdn.microsoft.com/library/hh156528.aspx)) mimo hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda a pak volání hello asynchronní metody z [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx).</span><span class="sxs-lookup"><span data-stu-id="31b02-134">Create asynchronous methods (such as [await](https://msdn.microsoft.com/library/hh156528.aspx)) outside of hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method and then call hello async methods from [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx).</span></span> <span data-ttu-id="31b02-135">Deklarování hello [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) jako asynchronní metodu způsobí, že pracovní hello role tooenter smyčku restartování.</span><span class="sxs-lookup"><span data-stu-id="31b02-135">Declaring hello [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method as async causes hello worker role tooenter a restart loop.</span></span>

<span data-ttu-id="31b02-136">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="31b02-136">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="31b02-137">Důvod</span><span class="sxs-lookup"><span data-stu-id="31b02-137">Reason</span></span>
<span data-ttu-id="31b02-138">Volání asynchronních metod uvnitř hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda způsobí, že hello cloudové služby modulu runtime toorecycle hello role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="31b02-138">Calling async methods inside hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method causes hello cloud service runtime toorecycle hello worker role.</span></span> <span data-ttu-id="31b02-139">Když se spustí roli pracovního procesu, všechny spuštění programu probíhá uvnitř hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="31b02-139">When a worker role starts, all program execution takes place inside hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method.</span></span> <span data-ttu-id="31b02-140">Existujícímu hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda způsobí, že pracovní hello role toorestart.</span><span class="sxs-lookup"><span data-stu-id="31b02-140">Exiting hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method causes hello worker role toorestart.</span></span> <span data-ttu-id="31b02-141">Pokud se modul runtime role pracovního procesu hello dotkne hello asynchronní metody, odešle zprávu všechny operace po hello asynchronní metody a vrátí.</span><span class="sxs-lookup"><span data-stu-id="31b02-141">When hello worker role runtime hits hello async method, it dispatches all operations after hello async method and then returns.</span></span> <span data-ttu-id="31b02-142">To způsobí, že pracovní hello role tooexit z hello [ [ [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda a restartování.</span><span class="sxs-lookup"><span data-stu-id="31b02-142">This causes hello worker role tooexit from hello [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method and restart.</span></span> <span data-ttu-id="31b02-143">V hello další iterace provádění role pracovního procesu hello přístupy hello asynchronní metody znovu a restartuje, způsobuje hello pracovní role toorecycle znovu také.</span><span class="sxs-lookup"><span data-stu-id="31b02-143">In hello next iteration of execution, hello worker role hits hello async method again and restarts, causing hello worker role toorecycle again as well.</span></span>

### <a name="solution"></a><span data-ttu-id="31b02-144">Řešení</span><span class="sxs-lookup"><span data-stu-id="31b02-144">Solution</span></span>
<span data-ttu-id="31b02-145">Umístit všechny asynchronní operace mimo hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="31b02-145">Place all async operations outside of hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method.</span></span> <span data-ttu-id="31b02-146">Potom zavolejte hello rozdělili asynchronní metody z uvnitř hello [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metody, jako je .wait RunAsync ().</span><span class="sxs-lookup"><span data-stu-id="31b02-146">Then, call hello refactored async method from inside hello [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method, such as RunAsync().wait.</span></span> <span data-ttu-id="31b02-147">Nástroj pro analýzu kódu Azure Hello můžete tento problém vyřešit.</span><span class="sxs-lookup"><span data-stu-id="31b02-147">hello Azure Code Analysis tool can help you fix this issue.</span></span>

<span data-ttu-id="31b02-148">Hello následující fragment kódu ukazuje hello kód opravu tohoto problému:</span><span class="sxs-lookup"><span data-stu-id="31b02-148">hello following code snippet demonstrates hello code fix for this issue:</span></span>

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

## <a name="use-service-bus-shared-access-signature-authentication"></a><span data-ttu-id="31b02-149">Ověřování služby sběrnice sdíleného přístupového podpisu</span><span class="sxs-lookup"><span data-stu-id="31b02-149">Use Service Bus Shared Access Signature authentication</span></span>
### <a name="id"></a><span data-ttu-id="31b02-150">ID</span><span class="sxs-lookup"><span data-stu-id="31b02-150">ID</span></span>
<span data-ttu-id="31b02-151">AP2000</span><span class="sxs-lookup"><span data-stu-id="31b02-151">AP2000</span></span>

### <a name="description"></a><span data-ttu-id="31b02-152">Popis</span><span class="sxs-lookup"><span data-stu-id="31b02-152">Description</span></span>
<span data-ttu-id="31b02-153">Pomocí sdíleného přístupového podpisu (SAS) pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="31b02-153">Use Shared Access Signature (SAS) for authentication.</span></span> <span data-ttu-id="31b02-154">Služby Řízení přístupu (ACS) je zastaralá pro ověřování sběrnice služby.</span><span class="sxs-lookup"><span data-stu-id="31b02-154">Access Control Service (ACS) is being deprecated for service bus authentication.</span></span>

<span data-ttu-id="31b02-155">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="31b02-155">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="31b02-156">Důvod</span><span class="sxs-lookup"><span data-stu-id="31b02-156">Reason</span></span>
<span data-ttu-id="31b02-157">Pro zvýšení zabezpečení Azure Active Directory nahrazuje ověřování služby ACS se ověřování SAS.</span><span class="sxs-lookup"><span data-stu-id="31b02-157">For enhanced security, Azure Active Directory is replacing ACS authentication with SAS authentication.</span></span> <span data-ttu-id="31b02-158">V tématu [Azure Active Directory je hello budoucí ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) informace o plánu přechodu hello.</span><span class="sxs-lookup"><span data-stu-id="31b02-158">See [Azure Active Directory is hello future of ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) for information on hello transition plan.</span></span>

### <a name="solution"></a><span data-ttu-id="31b02-159">Řešení</span><span class="sxs-lookup"><span data-stu-id="31b02-159">Solution</span></span>
<span data-ttu-id="31b02-160">Používejte ověřování SAS ve svých aplikacích.</span><span class="sxs-lookup"><span data-stu-id="31b02-160">Use SAS authentication in your apps.</span></span> <span data-ttu-id="31b02-161">Hello následující příklad ukazuje, jak sběrnici toouse existující SAS token tooaccess služba obor názvů nebo entity.</span><span class="sxs-lookup"><span data-stu-id="31b02-161">hello following example shows how toouse an existing SAS token tooaccess a service bus namespace or entity.</span></span>

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

<span data-ttu-id="31b02-162">V tématu hello následující témata pro další informace.</span><span class="sxs-lookup"><span data-stu-id="31b02-162">See hello following topics for more information.</span></span>

* <span data-ttu-id="31b02-163">Přehled najdete v tématu [ověřování sdíleného přístupového podpisu službou Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)</span><span class="sxs-lookup"><span data-stu-id="31b02-163">For an overview, see [Shared Access Signature Authentication with Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)</span></span>
* [<span data-ttu-id="31b02-164">Jak toouse ověřování podpisu sdíleného přístupu se Service Bus</span><span class="sxs-lookup"><span data-stu-id="31b02-164">How toouse Shared Access Signature Authentication with Service Bus</span></span>](https://msdn.microsoft.com/library/dn205161.aspx)
* <span data-ttu-id="31b02-165">Ukázkový projekt, najdete v části [pomocí sdíleného přístupového podpisu (SAS) ověřování pomocí předplatných Service Bus](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)</span><span class="sxs-lookup"><span data-stu-id="31b02-165">For a sample project, see [Using Shared Access Signature (SAS) authentication with Service Bus Subscriptions](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)</span></span>

## <a name="consider-using-onmessage-method-tooavoid-receive-loop"></a><span data-ttu-id="31b02-166">Zvažte použití metody tooavoid OnMessage "přijímat smyčka"</span><span class="sxs-lookup"><span data-stu-id="31b02-166">Consider using OnMessage method tooavoid "receive loop"</span></span>
### <a name="id"></a><span data-ttu-id="31b02-167">ID</span><span class="sxs-lookup"><span data-stu-id="31b02-167">ID</span></span>
<span data-ttu-id="31b02-168">AP2002</span><span class="sxs-lookup"><span data-stu-id="31b02-168">AP2002</span></span>

### <a name="description"></a><span data-ttu-id="31b02-169">Popis</span><span class="sxs-lookup"><span data-stu-id="31b02-169">Description</span></span>
<span data-ttu-id="31b02-170">tooavoid přejdete do "přijímat smyčky," volání hello **OnMessage** metoda je lepší řešení pro příjem zprávy než volajícím hello **Receive** metoda.</span><span class="sxs-lookup"><span data-stu-id="31b02-170">tooavoid going into a "receive loop," calling hello **OnMessage** method is a better solution for receiving messages than calling hello **Receive** method.</span></span> <span data-ttu-id="31b02-171">Ale pokud je nutné použít hello **Receive** metodu a zadejte čas čekání serveru jiné než výchozí, zkontrolujte, že doba čekání serveru hello je více než jedna minuta.</span><span class="sxs-lookup"><span data-stu-id="31b02-171">However, if you must use hello **Receive** method, and you specify a non-default server wait time, make sure hello server wait time is more than one minute.</span></span>

<span data-ttu-id="31b02-172">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="31b02-172">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="31b02-173">Důvod</span><span class="sxs-lookup"><span data-stu-id="31b02-173">Reason</span></span>
<span data-ttu-id="31b02-174">Při volání metody **OnMessage**, hello klient spustí vnitřní zpráva některé čerpadlo, který neustále dotazuje hello fronty nebo předplatné.</span><span class="sxs-lookup"><span data-stu-id="31b02-174">When calling **OnMessage**, hello client starts an internal message pump that constantly polls hello queue or subscription.</span></span> <span data-ttu-id="31b02-175">Tato zpráva čerpadlo obsahuje nekonečnou smyčku, která vydává volání tooreceive zprávy.</span><span class="sxs-lookup"><span data-stu-id="31b02-175">This message pump contains an infinite loop that issues a call tooreceive messages.</span></span> <span data-ttu-id="31b02-176">Pokud vyprší časový limit volání hello, vydá nové volání.</span><span class="sxs-lookup"><span data-stu-id="31b02-176">If hello call times out, it issues a new call.</span></span> <span data-ttu-id="31b02-177">interval vypršení časového limitu Hello je dáno hello hodnotu hello [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) vlastnost hello [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)který se používá.</span><span class="sxs-lookup"><span data-stu-id="31b02-177">hello timeout interval is determined by hello value of hello [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) property of hello [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)that’s being used.</span></span>

<span data-ttu-id="31b02-178">Výhodou použití Hello **OnMessage** porovnání příliš**Receive** je, že uživatelé nemají toomanually dotazování pro zprávy, zpracování výjimek, zpracování více zpráv paralelně a dokončení hello zprávy.</span><span class="sxs-lookup"><span data-stu-id="31b02-178">hello advantage of using **OnMessage** compared too**Receive** is that users don’t have toomanually poll for messages, handle exceptions, process multiple messages in parallel, and complete hello messages.</span></span>

<span data-ttu-id="31b02-179">Když zavoláte **Receive** bez použití jeho výchozí hodnotu, se že hello *ServerWaitTime* hodnota je více než jedna minuta.</span><span class="sxs-lookup"><span data-stu-id="31b02-179">If you call **Receive** without using its default value, be sure hello *ServerWaitTime* value is more than one minute.</span></span> <span data-ttu-id="31b02-180">Nastavení *ServerWaitTime* vyprší dřív, než je plně přijat uvítací zprávu hello server zabrání toomore než jedna minuta.</span><span class="sxs-lookup"><span data-stu-id="31b02-180">Setting *ServerWaitTime* toomore than one minute prevents hello server from timing out before hello message is fully received.</span></span>

### <a name="solution"></a><span data-ttu-id="31b02-181">Řešení</span><span class="sxs-lookup"><span data-stu-id="31b02-181">Solution</span></span>
<span data-ttu-id="31b02-182">Podrobnosti viz následující příklady kódu pro použití, doporučené hello.</span><span class="sxs-lookup"><span data-stu-id="31b02-182">Please see hello following code examples for recommended usages.</span></span> <span data-ttu-id="31b02-183">Další podrobnosti najdete v tématu [QueueClient.OnMessage – metoda (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)a [QueueClient.Receive – metoda (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).</span><span class="sxs-lookup"><span data-stu-id="31b02-183">For more details, see [QueueClient.OnMessage Method (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)and [QueueClient.Receive Method (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).</span></span>

<span data-ttu-id="31b02-184">výkon hello tooimprove hello infrastrukturu zasílání zpráv Azure, najdete v části vzoru návrhu hello [asynchronní zasílání zpráv Úvod do](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="31b02-184">tooimprove hello performance of hello Azure messaging infrastructure, see hello design pattern [Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span>

<span data-ttu-id="31b02-185">Hello následuje příklad použití **OnMessage** tooreceive zprávy.</span><span class="sxs-lookup"><span data-stu-id="31b02-185">hello following is an example of using **OnMessage** tooreceive messages.</span></span>

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if hello message-pump should call complete on messages after hello callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates hello maximum number of concurrent calls toohello callback hello pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you tooget notified of any errors encountered by hello message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates hello message pump and callback is invoked for each message that is recieved, calling close on hello client will stop hello pump.
    {
        // Process hello message.
    }, options);
    Console.WriteLine("Press any key tooexit.");
    Console.ReadKey();
```

<span data-ttu-id="31b02-186">Hello následuje příklad použití **Receive** doba čekání hello výchozí server.</span><span class="sxs-lookup"><span data-stu-id="31b02-186">hello following is an example of using **Receive** with hello default server wait time.</span></span>

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

<span data-ttu-id="31b02-187">Hello následuje příklad použití **Receive** serveru jiné než výchozí doba čekání.</span><span class="sxs-lookup"><span data-stu-id="31b02-187">hello following is an example of using **Receive** with a non-default server wait time.</span></span>

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
## <a name="consider-using-asynchronous-service-bus-methods"></a><span data-ttu-id="31b02-188">Zvažte použití asynchronních metod Service Bus</span><span class="sxs-lookup"><span data-stu-id="31b02-188">Consider using asynchronous Service Bus methods</span></span>
### <a name="id"></a><span data-ttu-id="31b02-189">ID</span><span class="sxs-lookup"><span data-stu-id="31b02-189">ID</span></span>
<span data-ttu-id="31b02-190">AP2003</span><span class="sxs-lookup"><span data-stu-id="31b02-190">AP2003</span></span>

### <a name="description"></a><span data-ttu-id="31b02-191">Popis</span><span class="sxs-lookup"><span data-stu-id="31b02-191">Description</span></span>
<span data-ttu-id="31b02-192">Použijte asynchronní výkonu tooimprove metody Service Bus s zprostředkované zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="31b02-192">Use asynchronous Service Bus methods tooimprove performance with brokered messaging.</span></span>

<span data-ttu-id="31b02-193">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="31b02-193">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="31b02-194">Důvod</span><span class="sxs-lookup"><span data-stu-id="31b02-194">Reason</span></span>
<span data-ttu-id="31b02-195">Použití asynchronních metod umožňuje souběžnosti program aplikace, protože provádění jednotlivých volání neblokuje hello hlavního vlákna.</span><span class="sxs-lookup"><span data-stu-id="31b02-195">Using asynchronous methods enables application program concurrency because executing each call doesn’t block hello main thread.</span></span> <span data-ttu-id="31b02-196">Při použití služby Service Bus metody pro zasílání zpráv, provádění operace (odesílání, příjem, odstranit, apod) trvá určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="31b02-196">When using Service Bus messaging methods, performing an operation (send, receive, delete, etc.) takes time.</span></span> <span data-ttu-id="31b02-197">Tentokrát zahrnuje hello zpracování operace hello podle hello služby Service Bus v přidání toohello latenci dotazů a odpovědí hello hello.</span><span class="sxs-lookup"><span data-stu-id="31b02-197">This time includes hello processing of hello operation by hello Service Bus service in addition toohello latency of hello request and hello reply.</span></span> <span data-ttu-id="31b02-198">tooincrease hello počet operací za čas, operace musí být spuštěn současně.</span><span class="sxs-lookup"><span data-stu-id="31b02-198">tooincrease hello number of operations per time, operations must execute concurrently.</span></span> <span data-ttu-id="31b02-199">Další informace naleznete v příliš[osvědčené postupy pro výkon vylepšení pomocí Service Bus zprostředkované zasílání zpráv](https://msdn.microsoft.com/library/azure/hh528527.aspx).</span><span class="sxs-lookup"><span data-stu-id="31b02-199">For more information please refer too[Best Practices for Performance Improvements Using Service Bus Brokered Messaging](https://msdn.microsoft.com/library/azure/hh528527.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="31b02-200">Řešení</span><span class="sxs-lookup"><span data-stu-id="31b02-200">Solution</span></span>
<span data-ttu-id="31b02-201">V tématu [QueueClient – třída (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) informace o způsobu toouse hello doporučená asynchronní metody.</span><span class="sxs-lookup"><span data-stu-id="31b02-201">See [QueueClient Class (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) for information about how toouse hello recommended asynchronous method.</span></span>

<span data-ttu-id="31b02-202">výkon hello tooimprove hello infrastrukturu zasílání zpráv Azure, najdete v části vzoru návrhu hello [asynchronní zasílání zpráv Úvod do](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="31b02-202">tooimprove hello performance of hello Azure messaging infrastructure, see hello design pattern [Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span>

## <a name="consider-partitioning-service-bus-queues-and-topics"></a><span data-ttu-id="31b02-203">Vezměte v úvahu rozdělení fronty Service Bus a témat</span><span class="sxs-lookup"><span data-stu-id="31b02-203">Consider partitioning Service Bus queues and topics</span></span>
### <a name="id"></a><span data-ttu-id="31b02-204">ID</span><span class="sxs-lookup"><span data-stu-id="31b02-204">ID</span></span>
<span data-ttu-id="31b02-205">AP2004</span><span class="sxs-lookup"><span data-stu-id="31b02-205">AP2004</span></span>

### <a name="description"></a><span data-ttu-id="31b02-206">Popis</span><span class="sxs-lookup"><span data-stu-id="31b02-206">Description</span></span>
<span data-ttu-id="31b02-207">Oddíl fronty Service Bus a témat pro lepší výkon s zasílání zpráv Service Bus.</span><span class="sxs-lookup"><span data-stu-id="31b02-207">Partition Service Bus queues and topics for better performance with Service Bus messaging.</span></span>

<span data-ttu-id="31b02-208">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="31b02-208">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="31b02-209">Důvod</span><span class="sxs-lookup"><span data-stu-id="31b02-209">Reason</span></span>
<span data-ttu-id="31b02-210">Vytváření oddílů fronty sběrnice a témata zvýšíte dostupnost výkonu propustnost a služby, protože hello celkovou propustnost oddílů fronta nebo téma je již omezena hello výkonu zprostředkovatele jedné zprávy nebo úložišti pro přenos zpráv.</span><span class="sxs-lookup"><span data-stu-id="31b02-210">Partitioning Service Bus queues and topics increases performance throughput and service availability because hello overall throughput of a partitioned queue or topic is no longer limited by hello performance of a single message broker or messaging store.</span></span> <span data-ttu-id="31b02-211">Kromě toho dočasnému výpadku zasílání zpráv úložiště není znepřístupnit oddílů fronta nebo téma.</span><span class="sxs-lookup"><span data-stu-id="31b02-211">In addition, a temporary outage of a messaging store doesn’t make a partitioned queue or topic unavailable.</span></span> <span data-ttu-id="31b02-212">Další informace najdete v tématu [dělení entity zasílání zpráv](https://msdn.microsoft.com/library/azure/dn520246.aspx).</span><span class="sxs-lookup"><span data-stu-id="31b02-212">For more information, see [Partitioning Messaging Entities](https://msdn.microsoft.com/library/azure/dn520246.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="31b02-213">Řešení</span><span class="sxs-lookup"><span data-stu-id="31b02-213">Solution</span></span>
<span data-ttu-id="31b02-214">Následující kód fragment kódu ukazuje, jak Hello toopartition entity pro zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="31b02-214">hello following code snippet shows how toopartition messaging entities.</span></span>

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

<span data-ttu-id="31b02-215">Další informace najdete v tématu [rozdělena na oddíly fronty služby Service Bus a témat | Microsoft Azure Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) a podívejte se na hello [Microsoft Azure Service Bus rozdělena na oddíly fronty](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) ukázka.</span><span class="sxs-lookup"><span data-stu-id="31b02-215">For more information, see [Partitioned Service Bus Queues and Topics | Microsoft Azure Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) and check out hello [Microsoft Azure Service Bus Partitioned Queue](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) sample.</span></span>

## <a name="do-not-set-sharedaccessstarttime"></a><span data-ttu-id="31b02-216">Nenastavujte SharedAccessStartTime</span><span class="sxs-lookup"><span data-stu-id="31b02-216">Do not set SharedAccessStartTime</span></span>
### <a name="id"></a><span data-ttu-id="31b02-217">ID</span><span class="sxs-lookup"><span data-stu-id="31b02-217">ID</span></span>
<span data-ttu-id="31b02-218">AP3001</span><span class="sxs-lookup"><span data-stu-id="31b02-218">AP3001</span></span>

### <a name="description"></a><span data-ttu-id="31b02-219">Popis</span><span class="sxs-lookup"><span data-stu-id="31b02-219">Description</span></span>
<span data-ttu-id="31b02-220">Byste neměli používat SharedAccessStartTimeset toohello aktuální čas tooimmediately spustit hello zásady sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="31b02-220">You should avoid using SharedAccessStartTimeset toohello current time tooimmediately start hello Shared Access policy.</span></span> <span data-ttu-id="31b02-221">Potřebujete jenom tooset tuto vlastnost Pokud chcete zásady sdíleného přístupu toostart hello později.</span><span class="sxs-lookup"><span data-stu-id="31b02-221">You only need tooset this property if you want toostart hello Shared Access policy at a later time.</span></span>

<span data-ttu-id="31b02-222">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="31b02-222">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="31b02-223">Důvod</span><span class="sxs-lookup"><span data-stu-id="31b02-223">Reason</span></span>
<span data-ttu-id="31b02-224">Synchronizace hodin způsobí, že mírné časový rozdíl mezi datovými centry.</span><span class="sxs-lookup"><span data-stu-id="31b02-224">Clock synchronization causes a slight time difference among datacenters.</span></span> <span data-ttu-id="31b02-225">Například by logicky domníváte, že čas zahájení hello nastavení úložiště SAS zásady jako hello aktuální čas pomocí DateTime.Now nebo podobné metody způsobí, že hello SAS zásad tootake vliv okamžitě.</span><span class="sxs-lookup"><span data-stu-id="31b02-225">For example, you would logically think setting hello start time of a storage SAS policy as hello current time by using DateTime.Now or a similar method will cause hello SAS policy tootake effect immediately.</span></span> <span data-ttu-id="31b02-226">Hello mírné časové rozdíly mezi datovými centry však může způsobit problémy s tím, vzhledem k tomu, že některé datacenter časy může být mírně pozdější než čas zahájení hello, zatímco jiní jej před jeho.</span><span class="sxs-lookup"><span data-stu-id="31b02-226">However, hello slight time differences between datacenters can cause problems with this since some datacenter times might be slightly later than hello start time, while others ahead of it.</span></span> <span data-ttu-id="31b02-227">V důsledku toho můžete rychle (nebo i okamžitě) vyprší platnost hello SAS zásad nastaveného hello zásady životnosti je příliš krátké.</span><span class="sxs-lookup"><span data-stu-id="31b02-227">As a result, hello SAS policy can expire quickly (or even immediately) if hello policy lifetime is set too short.</span></span>

<span data-ttu-id="31b02-228">Další informace o používání sdíleného přístupového podpisu na úložiště Azure najdete v části [představení tabulky SAS (sdíleného přístupového podpisu), fronta SAS a aktualizace tooBlob SAS – Blog týmu pro úložiště Azure Microsoft - lokality Domů - Blogy MSDN](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span><span class="sxs-lookup"><span data-stu-id="31b02-228">For more guidance on using Shared Access Signature on Azure storage, see [Introducing Table SAS (Shared Access Signature), Queue SAS and update tooBlob SAS - Microsoft Azure Storage Team Blog - Site Home - MSDN Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="31b02-229">Řešení</span><span class="sxs-lookup"><span data-stu-id="31b02-229">Solution</span></span>
<span data-ttu-id="31b02-230">Odeberte hello příkaz, který nastaví čas zahájení hello hello sdílených zásad přístupu.</span><span class="sxs-lookup"><span data-stu-id="31b02-230">Remove hello statement that sets hello start time of hello shared access policy.</span></span> <span data-ttu-id="31b02-231">Nástroj pro analýzu kódu Azure Hello poskytuje opravu tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="31b02-231">hello Azure Code Analysis tool provides a fix for this issue.</span></span> <span data-ttu-id="31b02-232">Další informace o správu zabezpečení, najdete v tématu vzoru návrhu hello [vzor klíč osobní služby](https://msdn.microsoft.com/library/dn568102.aspx).</span><span class="sxs-lookup"><span data-stu-id="31b02-232">For more information on security management, please see hello design pattern [Valet Key Pattern](https://msdn.microsoft.com/library/dn568102.aspx).</span></span>

<span data-ttu-id="31b02-233">Hello následující fragment kódu ukazuje hello kód opravu tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="31b02-233">hello following code snippet demonstrates hello code fix for this issue.</span></span>

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a><span data-ttu-id="31b02-234">Sdílené zásady přístupu čas vypršení platnosti musí být delší než 5 minut</span><span class="sxs-lookup"><span data-stu-id="31b02-234">Shared Access Policy expiry time must be more than five minutes</span></span>
### <a name="id"></a><span data-ttu-id="31b02-235">ID</span><span class="sxs-lookup"><span data-stu-id="31b02-235">ID</span></span>
<span data-ttu-id="31b02-236">AP3002</span><span class="sxs-lookup"><span data-stu-id="31b02-236">AP3002</span></span>

### <a name="description"></a><span data-ttu-id="31b02-237">Popis</span><span class="sxs-lookup"><span data-stu-id="31b02-237">Description</span></span>
<span data-ttu-id="31b02-238">Může být co nejvíce pět minut rozdíl v hodiny mezi datovými centry v různých umístěních kvůli tooa Stav známý jako "posun hodin."</span><span class="sxs-lookup"><span data-stu-id="31b02-238">There can be as much as a five minute difference in clocks among datacenters at different locations due tooa condition known as "clock skew."</span></span> <span data-ttu-id="31b02-239">token tooprevent hello SAS zásady vypršení platnosti dříve, než plánované, nastavte toobe čas vypršení platnosti hello více než pět minut.</span><span class="sxs-lookup"><span data-stu-id="31b02-239">tooprevent hello SAS policy token from expiring earlier than planned, set hello expiry time toobe more than five minutes.</span></span>

<span data-ttu-id="31b02-240">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="31b02-240">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="31b02-241">Důvod</span><span class="sxs-lookup"><span data-stu-id="31b02-241">Reason</span></span>
<span data-ttu-id="31b02-242">Datových center v různých umístěních kolem hello, world synchronizovat signál hodiny.</span><span class="sxs-lookup"><span data-stu-id="31b02-242">Datacenters at different locations around hello world synchronize by a clock signal.</span></span> <span data-ttu-id="31b02-243">Protože trvá dobu hodiny signál tootravel toodifferent umístění, může být čas odchylka mezi datovými centry v různých geografických umístěních i když všechno, co je zřejmě synchronizován.</span><span class="sxs-lookup"><span data-stu-id="31b02-243">Because it takes time for clock signal tootravel toodifferent locations, there can be a time variance between datacenters at different geographical locations although everything is supposedly synchronized.</span></span> <span data-ttu-id="31b02-244">Tento časový rozdíl může ovlivnit hello sdíleného přístupu počáteční čas a vypršení platnosti interval zásad.</span><span class="sxs-lookup"><span data-stu-id="31b02-244">This time difference can affect hello Shared Access policy start time and expiration interval.</span></span> <span data-ttu-id="31b02-245">Proto tooensure zásady sdíleného přístupu se okamžitě projeví, nezadávejte hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="31b02-245">Therefore, tooensure Shared Access policy takes effect immediately, don’t specify hello start time.</span></span> <span data-ttu-id="31b02-246">Kromě toho zajistěte, aby byl čas vypršení platnosti hello více než 5 minut tooprevent časná vypršení časového limitu.</span><span class="sxs-lookup"><span data-stu-id="31b02-246">In addition, make sure hello expiration time is more than 5 minutes tooprevent early timeout.</span></span>

<span data-ttu-id="31b02-247">Další informace o používání sdíleného přístupového podpisu na úložiště Azure najdete v tématu [představení tabulky SAS (sdíleného přístupového podpisu), fronta SAS a aktualizace tooBlob SAS – Blog týmu pro úložiště Azure Microsoft - lokality Domů - Blogy MSDN](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span><span class="sxs-lookup"><span data-stu-id="31b02-247">For more information about using Shared Access Signature on Azure storage, see [Introducing Table SAS (Shared Access Signature), Queue SAS and update tooBlob SAS - Microsoft Azure Storage Team Blog - Site Home - MSDN Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="31b02-248">Řešení</span><span class="sxs-lookup"><span data-stu-id="31b02-248">Solution</span></span>
<span data-ttu-id="31b02-249">Další informace o správu zabezpečení, najdete v části vzoru návrhu hello [vzor klíč osobní služby](https://msdn.microsoft.com/library/dn568102.aspx).</span><span class="sxs-lookup"><span data-stu-id="31b02-249">For more information on security management, see hello design pattern [Valet Key Pattern](https://msdn.microsoft.com/library/dn568102.aspx).</span></span>

<span data-ttu-id="31b02-250">Hello tady je příklad není zadávání čas spuštění zásady sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="31b02-250">hello following is an example of not specifying a Shared Access policy start time.</span></span>

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

<span data-ttu-id="31b02-251">Hello následuje příklad zadání čas spuštění zásady sdíleného přístupu s doba vypršení platnosti zásada, která je větší než pět minut.</span><span class="sxs-lookup"><span data-stu-id="31b02-251">hello following is an example of specifying a Shared Access policy start time with a policy expiration duration greater than five minutes.</span></span>

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

<span data-ttu-id="31b02-252">Další informace najdete v tématu [vytváření a používání sdíleného přístupového podpisu](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span><span class="sxs-lookup"><span data-stu-id="31b02-252">For more information, see [Create and Use a Shared Access Signature](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span></span>

## <a name="use-cloudconfigurationmanager"></a><span data-ttu-id="31b02-253">Použití CloudConfigurationManager</span><span class="sxs-lookup"><span data-stu-id="31b02-253">Use CloudConfigurationManager</span></span>
### <a name="id"></a><span data-ttu-id="31b02-254">ID</span><span class="sxs-lookup"><span data-stu-id="31b02-254">ID</span></span>
<span data-ttu-id="31b02-255">AP4000</span><span class="sxs-lookup"><span data-stu-id="31b02-255">AP4000</span></span>

### <a name="description"></a><span data-ttu-id="31b02-256">Popis</span><span class="sxs-lookup"><span data-stu-id="31b02-256">Description</span></span>
<span data-ttu-id="31b02-257">Pomocí hello [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) třídy pro projekty, jako je Azure web a mobilní služby Azure nebude znamenat problémy modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="31b02-257">Using hello [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) class for projects such as Azure Website and Azure mobile services won't introduce runtime issues.</span></span> <span data-ttu-id="31b02-258">Jako osvědčený postup je však vhodné toouse cloudu[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) jako jednotné způsob správy konfigurace pro všechny aplikace cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="31b02-258">As a best practice, however, it's a good idea toouse Cloud[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) as a unified way of managing configurations for all Azure Cloud applications.</span></span>

<span data-ttu-id="31b02-259">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="31b02-259">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="31b02-260">Důvod</span><span class="sxs-lookup"><span data-stu-id="31b02-260">Reason</span></span>
<span data-ttu-id="31b02-261">CloudConfigurationManager přečte hello konfigurační soubor odpovídající toohello aplikace prostředí.</span><span class="sxs-lookup"><span data-stu-id="31b02-261">CloudConfigurationManager reads hello configuration file appropriate toohello application environment.</span></span>

[<span data-ttu-id="31b02-262">CloudConfigurationManager</span><span class="sxs-lookup"><span data-stu-id="31b02-262">CloudConfigurationManager</span></span>](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a><span data-ttu-id="31b02-263">Řešení</span><span class="sxs-lookup"><span data-stu-id="31b02-263">Solution</span></span>
<span data-ttu-id="31b02-264">Váš kód toouse hello Refaktorovat [třída CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="31b02-264">Refactor your code toouse hello [CloudConfigurationManager Class](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx).</span></span> <span data-ttu-id="31b02-265">Nástroj pro analýzu kódu Azure hello zajišťuje opravy kódu pro tento problém.</span><span class="sxs-lookup"><span data-stu-id="31b02-265">A code fix for this issue is provided by hello Azure Code Analysis tool.</span></span>

<span data-ttu-id="31b02-266">Hello následující fragment kódu ukazuje hello kód opravu tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="31b02-266">hello following code snippet demonstrates hello code fix for this issue.</span></span> <span data-ttu-id="31b02-267">Nahradit</span><span class="sxs-lookup"><span data-stu-id="31b02-267">Replace</span></span>

`var settings = ConfigurationManager.AppSettings["mySettings"];`

<span data-ttu-id="31b02-268">S</span><span class="sxs-lookup"><span data-stu-id="31b02-268">with</span></span>

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

<span data-ttu-id="31b02-269">Tady je příklad, jak toostore hello nastavení konfigurace v souboru App.config nebo Web.config.</span><span class="sxs-lookup"><span data-stu-id="31b02-269">Here's an example of how toostore hello configuration setting in a App.config or Web.config file.</span></span> <span data-ttu-id="31b02-270">Přidejte hello nastavení toohello oddílu appSettings hello konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="31b02-270">Add hello settings toohello appSettings section of hello configuration file.</span></span> <span data-ttu-id="31b02-271">Hello následuje hello souboru Web.config pro předchozí příklad kódu hello.</span><span class="sxs-lookup"><span data-stu-id="31b02-271">hello following is hello Web.config file for hello previous code example.</span></span>

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a><span data-ttu-id="31b02-272">Nepoužívejte pevně připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="31b02-272">Avoid using hard-coded connection strings</span></span>
### <a name="id"></a><span data-ttu-id="31b02-273">ID</span><span class="sxs-lookup"><span data-stu-id="31b02-273">ID</span></span>
<span data-ttu-id="31b02-274">AP4001</span><span class="sxs-lookup"><span data-stu-id="31b02-274">AP4001</span></span>

### <a name="description"></a><span data-ttu-id="31b02-275">Popis</span><span class="sxs-lookup"><span data-stu-id="31b02-275">Description</span></span>
<span data-ttu-id="31b02-276">Pokud používáte pevně připojovací řetězce a potřebujete tooupdate je později, budete mít toomake změny tooyour zdrojového kódu a překompilování aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="31b02-276">If you use hard-coded connection strings and you need tooupdate them later, you’ll have toomake changes tooyour source code and recompile hello application.</span></span> <span data-ttu-id="31b02-277">Ale pokud uchováváte připojovací řetězce v konfiguračním souboru, můžete je později jednoduše aktualizací hello konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="31b02-277">However, if you store your connection strings in a configuration file, you can change them later by simply updating hello configuration file.</span></span>

<span data-ttu-id="31b02-278">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="31b02-278">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="31b02-279">Důvod</span><span class="sxs-lookup"><span data-stu-id="31b02-279">Reason</span></span>
<span data-ttu-id="31b02-280">Pevně kódováno připojovací řetězce je chybný postup, protože by to zavedlo problémy, když potřebují toobe rychle změnit připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="31b02-280">Hard-coding connection strings is a bad practice because it introduces problems when connection strings need toobe changed quickly.</span></span> <span data-ttu-id="31b02-281">Kromě toho pokud hello projektu potřebuje toobe zaškrtnutí v ovládacím prvku toosource, zavést pevně připojovací řetězce ohrožení zabezpečení, vzhledem k tomu, že řetězce hello lze zobrazit v hello zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="31b02-281">In addition, if hello project needs toobe checked in toosource control, hard-coded connection strings introduce security vulnerabilities since hello strings can be viewed in hello source code.</span></span>

### <a name="solution"></a><span data-ttu-id="31b02-282">Řešení</span><span class="sxs-lookup"><span data-stu-id="31b02-282">Solution</span></span>
<span data-ttu-id="31b02-283">Ukládání připojovacích řetězců hello konfigurační soubory nebo prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="31b02-283">Store connection strings in hello configuration files or Azure environments.</span></span>

* <span data-ttu-id="31b02-284">Pro samostatné aplikace použijte nastavení app.config toostore připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="31b02-284">For standalone applications, use app.config toostore connection string settings.</span></span>
* <span data-ttu-id="31b02-285">Pro hostované službou IIS webové aplikace použijte řetězce připojení s toostore souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="31b02-285">For IIS-hosted web applications, use web.config toostore connection strings.</span></span>
* <span data-ttu-id="31b02-286">Pro aplikace ASP.NET vNext použijte configuration.json toostore připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="31b02-286">For ASP.NET vNext applications, use configuration.json toostore connection strings.</span></span>

<span data-ttu-id="31b02-287">Informace o používání soubory konfigurace, jako je soubor web.config nebo app.config najdete v tématu [pokyny pro ASP.NET Web konfigurace](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx).</span><span class="sxs-lookup"><span data-stu-id="31b02-287">For information on using configurations files such as web.config or app.config, see [ASP.NET Web Configuration Guidelines](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx).</span></span> <span data-ttu-id="31b02-288">Informace o tom, jak Azure pracovní proměnné prostředí, najdete v části [weby Azure: fungování řetězců aplikace a připojovací řetězce fungovat](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span><span class="sxs-lookup"><span data-stu-id="31b02-288">For information on how Azure environment variables work, see [Azure Web Sites: How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span></span> <span data-ttu-id="31b02-289">Informace týkající se ukládání připojovací řetězec do správy zdrojového kódu najdete v tématu [neukládejte citlivé informace, jako je například připojovací řetězce v souborech, které jsou uložené v úložiště zdrojového kódu](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).</span><span class="sxs-lookup"><span data-stu-id="31b02-289">For information on storing connection string in source control, see [avoid putting sensitive information such as connection strings in files that are stored in source code repository](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).</span></span>

## <a name="use-diagnostics-configuration-file"></a><span data-ttu-id="31b02-290">Použijte diagnostiku konfigurační soubor</span><span class="sxs-lookup"><span data-stu-id="31b02-290">Use diagnostics configuration file</span></span>
### <a name="id"></a><span data-ttu-id="31b02-291">ID</span><span class="sxs-lookup"><span data-stu-id="31b02-291">ID</span></span>
<span data-ttu-id="31b02-292">AP5000</span><span class="sxs-lookup"><span data-stu-id="31b02-292">AP5000</span></span>

### <a name="description"></a><span data-ttu-id="31b02-293">Popis</span><span class="sxs-lookup"><span data-stu-id="31b02-293">Description</span></span>
<span data-ttu-id="31b02-294">Namísto konfigurace nastavení diagnostiky ve vašem kódu, například pomocí hello Microsoft.WindowsAzure.Diagnostics programovací rozhraní API, byste měli nakonfigurovat nastavení diagnostiky v souboru diagnostics.wadcfg hello.</span><span class="sxs-lookup"><span data-stu-id="31b02-294">Instead of configuring diagnostics settings in your code such as by using hello Microsoft.WindowsAzure.Diagnostics programming API, you should configure diagnostics settings in hello diagnostics.wadcfg file.</span></span> <span data-ttu-id="31b02-295">(Nebo, pokud používáte Azure SDK 2.5 diagnostics.wadcfgx).</span><span class="sxs-lookup"><span data-stu-id="31b02-295">(Or, diagnostics.wadcfgx if you use Azure SDK 2.5).</span></span> <span data-ttu-id="31b02-296">Díky tomu můžete změnit nastavení diagnostiky bez nutnosti toorecompile vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="31b02-296">By doing this, you can change diagnostics settings without having toorecompile your code.</span></span>

<span data-ttu-id="31b02-297">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="31b02-297">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="31b02-298">Důvod</span><span class="sxs-lookup"><span data-stu-id="31b02-298">Reason</span></span>
<span data-ttu-id="31b02-299">Před 2.5 Azure SDK, (která používá Azure diagnostics 1.3), Azure Diagnostics (WAD) se daly konfigurovat pomocí několika různých metod: přidáním toohello konfigurace objektů blob v úložišti pomocí imperativní kódu, deklarativní konfigurace nebo výchozí hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="31b02-299">Before Azure SDK 2.5 (which uses Azure diagnostics 1.3), Azure Diagnostics (WAD) could be configured by using several different methods: adding it toohello configuration blob in storage, by using imperative code, declarative configuration, or hello default configuration.</span></span> <span data-ttu-id="31b02-300">Však hello upřednostňovaný způsob, jak tooconfigure diagnostiky je toouse konfiguračního souboru XML (diagnostics.wadcfg nebo diagnositcs.wadcfgx pro sadu SDK, 2.5 a novější) v projektu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="31b02-300">However, hello preferred way tooconfigure diagnostics is toouse an XML configuration file (diagnostics.wadcfg or diagnositcs.wadcfgx for SDK 2.5 and later) in hello application project.</span></span> <span data-ttu-id="31b02-301">V tomto přístupu hello diagnostics.wadcfg souboru úplně definuje konfiguraci hello a lze aktualizovat a znovu nasazena na bude.</span><span class="sxs-lookup"><span data-stu-id="31b02-301">In this approach, hello diagnostics.wadcfg file completely defines hello configuration and can be updated and redeployed at will.</span></span> <span data-ttu-id="31b02-302">Kombinování hello použití hello diagnostics.wadcfg konfigurační soubor s hello programové metody nastavení konfigurace pomocí hello [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)nebo [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx) třídy může způsobit tooconfusion.</span><span class="sxs-lookup"><span data-stu-id="31b02-302">Mixing hello use of hello diagnostics.wadcfg configuration file with hello programmatic methods of setting configurations by using hello [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)or [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)classes can lead tooconfusion.</span></span> <span data-ttu-id="31b02-303">V tématu [inicializovat nebo změna konfigurace diagnostiky Azure](https://msdn.microsoft.com/library/azure/hh411537.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="31b02-303">See [Initialize or Change Azure Diagnostics Configuration](https://msdn.microsoft.com/library/azure/hh411537.aspx) for more information.</span></span>

<span data-ttu-id="31b02-304">Počínaje WAD 1.3 (zahrnutá v Azure SDK 2.5), je již nebude možné toouse diagnostiky tooconfigure kódu.</span><span class="sxs-lookup"><span data-stu-id="31b02-304">Beginning with WAD 1.3 (included with Azure SDK 2.5), it’s no longer possible toouse code tooconfigure diagnostics.</span></span> <span data-ttu-id="31b02-305">V důsledku toho můžete zadat jenom hello konfigurace při použití nebo aktualizaci rozšíření diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="31b02-305">As a result, you can only provide hello configuration when applying or updating hello diagnostics extension.</span></span>

### <a name="solution"></a><span data-ttu-id="31b02-306">Řešení</span><span class="sxs-lookup"><span data-stu-id="31b02-306">Solution</span></span>
<span data-ttu-id="31b02-307">Použijte hello diagnostiky konfigurace návrháře toomove nastavení pro diagnostiku toohello diagnostiky konfigurační soubor (diagnositcs.wadcfg nebo diagnositcs.wadcfgx pro sadu SDK, 2.5 a novější).</span><span class="sxs-lookup"><span data-stu-id="31b02-307">Use hello diagnostics configuration designer toomove diagnostic settings toohello diagnostics configuration file (diagnositcs.wadcfg or diagnositcs.wadcfgx for SDK 2.5 and later).</span></span> <span data-ttu-id="31b02-308">Doporučujeme také nainstalovat [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) a použít nejnovější funkce diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="31b02-308">It’s also recommended that you install [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) and use hello latest diagnostics feature.</span></span>

1. <span data-ttu-id="31b02-309">V místní nabídce hello hello role, které chcete tooconfigure vyberte vlastnosti a potom vyberte kartu Konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="31b02-309">On hello shortcut menu for hello role that you want tooconfigure, choose Properties, and then choose hello Configuration tab.</span></span>
2. <span data-ttu-id="31b02-310">V hello **diagnostiky** část, ujistěte se, že hello **povolení diagnostiky** je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="31b02-310">In hello **Diagnostics** section, make sure that hello **Enable Diagnostics** check box is selected.</span></span>
3. <span data-ttu-id="31b02-311">Zvolte hello **konfigurace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="31b02-311">Choose hello **Configure** button.</span></span>

   ![Přístup k hello možnost povolení diagnostiky](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

   <span data-ttu-id="31b02-313">V tématu [konfigurace diagnostiky pro Azure Cloud Services a virtuální počítače](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="31b02-313">See [Configuring Diagnostics for Azure Cloud Services and Virtual Machines](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) for more information.</span></span>

## <a name="avoid-declaring-dbcontext-objects-as-static"></a><span data-ttu-id="31b02-314">Vyhněte se deklarace objektů DbContext jako statické</span><span class="sxs-lookup"><span data-stu-id="31b02-314">Avoid declaring DbContext objects as static</span></span>
### <a name="id"></a><span data-ttu-id="31b02-315">ID</span><span class="sxs-lookup"><span data-stu-id="31b02-315">ID</span></span>
<span data-ttu-id="31b02-316">AP6000</span><span class="sxs-lookup"><span data-stu-id="31b02-316">AP6000</span></span>

### <a name="description"></a><span data-ttu-id="31b02-317">Popis</span><span class="sxs-lookup"><span data-stu-id="31b02-317">Description</span></span>
<span data-ttu-id="31b02-318">paměť toosave, vyhněte se deklarace objektů DBContext jako statické.</span><span class="sxs-lookup"><span data-stu-id="31b02-318">toosave memory, avoid declaring DBContext objects as static.</span></span>

<span data-ttu-id="31b02-319">Prosím sdílet své myšlenky a zpětnou vazbu na [zpětnou vazbu analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="31b02-319">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="31b02-320">Důvod</span><span class="sxs-lookup"><span data-stu-id="31b02-320">Reason</span></span>
<span data-ttu-id="31b02-321">Objekty DBContext podržte hello výsledků dotazu z každé volání.</span><span class="sxs-lookup"><span data-stu-id="31b02-321">DBContext objects hold hello query results from each call.</span></span> <span data-ttu-id="31b02-322">Statické DBContext objekty nebyly použity, dokud doménu aplikace hello je odpojen.</span><span class="sxs-lookup"><span data-stu-id="31b02-322">Static DBContext objects are not disposed until hello application domain is unloaded.</span></span> <span data-ttu-id="31b02-323">Proto statický objekt DBContext spotřebovat velké objemy paměti.</span><span class="sxs-lookup"><span data-stu-id="31b02-323">Therefore, a static DBContext object can consume large amounts of memory.</span></span>

### <a name="solution"></a><span data-ttu-id="31b02-324">Řešení</span><span class="sxs-lookup"><span data-stu-id="31b02-324">Solution</span></span>
<span data-ttu-id="31b02-325">Jako místní proměnné nebo pole instance nestatické deklarovat DBContext, použijte pro úlohy a nechat ji bude zrušen z po použití.</span><span class="sxs-lookup"><span data-stu-id="31b02-325">Declare DBContext as a local variable or non-static instance field, use it for a task, and then let it be disposed of after use.</span></span>

<span data-ttu-id="31b02-326">Hello následující třídy kontroleru MVC příklad ukazuje, jak toouse hello objekt DBContext.</span><span class="sxs-lookup"><span data-stu-id="31b02-326">hello following example MVC controller class shows how toouse hello DBContext object.</span></span>

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass tooDbContext        
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

## <a name="next-steps"></a><span data-ttu-id="31b02-327">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31b02-327">Next steps</span></span>
<span data-ttu-id="31b02-328">toolearn informace o optimalizaci a řešení potíží s aplikací Azure, najdete v části [řešení potíží s webovou aplikaci v Azure App Service pomocí sady Visual Studio](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="31b02-328">toolearn more about optimizing and troubleshooting Azure apps, see [Troubleshoot a web app in Azure App Service using Visual Studio](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span></span>
