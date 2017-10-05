---
title: Co je Azure WebJobs SDK
description: "Úvod do Azure WebJobs SDK. Vysvětluje, co sady SDK, typické scénáře jsou užitečné pro a ukázky kódu."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 8281267b-572b-4b14-a328-6704493ea682
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: 8eb05b7cbfb4505f2e94c5b8e6d367ec63a2f033
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-the-azure-webjobs-sdk"></a><span data-ttu-id="6fd2d-104">Co je Azure WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="6fd2d-104">What is the Azure WebJobs SDK</span></span>
## <span data-ttu-id="6fd2d-105"><a id="overview"></a>Přehled</span><span class="sxs-lookup"><span data-stu-id="6fd2d-105"><a id="overview"></a>Overview</span></span>
<span data-ttu-id="6fd2d-106">Tento článek vysvětluje, co WebJobs SDK, zkontroluje některé běžné scénáře je užitečné pro a poskytuje přehled o tom, jak je používat v kódu.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-106">This article explains what the WebJobs SDK is, reviews some common scenarios it is useful for, and gives an overview of how you use it in your code.</span></span>

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

<span data-ttu-id="6fd2d-107">[WebJobs](websites-webjobs-resources.md) je funkce služby Azure App Service, která umožňuje spustit program nebo skript v rámci stejné jako webové aplikace, aplikace API nebo mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-107">[WebJobs](websites-webjobs-resources.md) is a feature of Azure App Service that enables you to run a program or script in the same context as a web app, API app, or mobile app.</span></span> <span data-ttu-id="6fd2d-108">Účelem [WebJobs SDK](websites-webjobs-resources.md) je můžete zjednodušit kód napsaný pro běžné úkoly, které může provádět webovou úlohu, například zpracování obrázků, zpracování fronty, RSS agregace, údržba souborů a odesílání e-mailů.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-108">The purpose of the [WebJobs SDK](websites-webjobs-resources.md) is to simplify the code you write for common tasks that a WebJob can perform, such as image processing, queue processing, RSS aggregation, file maintenance, and sending emails.</span></span> <span data-ttu-id="6fd2d-109">Sada SDK webové úlohy obsahuje integrované funkce pro práci s Azure Storage a Service Bus, plánování úloh a zpracování chyb a pro jiné běžné scénáře.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-109">The WebJobs SDK has built-in features for working with Azure Storage and Service Bus, for scheduling tasks and handling errors, and for many other common scenarios.</span></span> <span data-ttu-id="6fd2d-110">Kromě toho je určený pro rozšiřitelnost.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-110">In addition, it's designed to be extensible.</span></span> <span data-ttu-id="6fd2d-111">[WebJobs SDK je open source](https://github.com/Azure/azure-webjobs-sdk/)a dojde [otevřete zdrojové úložiště pro rozšíření](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).</span><span class="sxs-lookup"><span data-stu-id="6fd2d-111">The [WebJobs SDK is open source](https://github.com/Azure/azure-webjobs-sdk/), and there's an [open source repository for extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).</span></span>

<span data-ttu-id="6fd2d-112">Sada WebJobs SDK zahrnuje následující součásti:</span><span class="sxs-lookup"><span data-stu-id="6fd2d-112">The WebJobs SDK includes the following components:</span></span>

* <span data-ttu-id="6fd2d-113">**Balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-113">**NuGet packages**.</span></span> <span data-ttu-id="6fd2d-114">Balíčky NuGet, které přidáte do projektu Visual Studio konzolové aplikace poskytují rozhraní, které používá váš kód podle architekturu vaše metody s atributy WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-114">NuGet packages that you add to a Visual Studio Console Application project provide a framework that your code uses by decorating your methods with WebJobs SDK attributes.</span></span>
* <span data-ttu-id="6fd2d-115">**Řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-115">**Dashboard**.</span></span> <span data-ttu-id="6fd2d-116">Součást WebJobs SDK je obsažena v Azure App Service a poskytuje rozšířené možnosti monitorování a Diagnostika pro programy, které používají balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-116">Part of the WebJobs SDK is included in Azure App Service and provides rich monitoring and diagnostics for programs that use the NuGet packages.</span></span> <span data-ttu-id="6fd2d-117">Nemáte k zápisu kódu pro použití těchto funkcí monitorování a Diagnostika.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-117">You don't have to write code to use these monitoring and diagnostics features.</span></span>

## <span data-ttu-id="6fd2d-118"><a id="scenarios"></a>Scénáře</span><span class="sxs-lookup"><span data-stu-id="6fd2d-118"><a id="scenarios"></a>Scenarios</span></span>
<span data-ttu-id="6fd2d-119">Zde jsou některé typické scénáře, které lze snadno zpracovat s Azure WebJobs SDK:</span><span class="sxs-lookup"><span data-stu-id="6fd2d-119">Here are some typical scenarios you can handle more easily with the Azure WebJobs SDK:</span></span>

* <span data-ttu-id="6fd2d-120">Obrázek zpracování nebo jiné pracovní náročná na prostředky procesoru.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-120">Image processing or other CPU-intensive work.</span></span> <span data-ttu-id="6fd2d-121">Běžné funkce webů, které je možnost odesílat obrázky nebo videa.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-121">A common feature of websites is the ability to upload images or videos.</span></span> <span data-ttu-id="6fd2d-122">Často budete chtít pracovat s obsahem po nahraje, ale nechcete, aby se uživatel, počkejte, můžete udělat.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-122">Often you want to manipulate the content after it's uploaded, but you don't want to make the user wait while you do that.</span></span>
* <span data-ttu-id="6fd2d-123">Fronty pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-123">Queue processing.</span></span> <span data-ttu-id="6fd2d-124">Běžný způsob pro front-end webové ke komunikaci s back-end službu je používat fronty.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-124">A common way for a web frontend to communicate with a backend service is to use queues.</span></span> <span data-ttu-id="6fd2d-125">Když web potřebuje ke své práci, doručí zprávu do fronty.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-125">When the website needs to get work done, it pushes a message onto a queue.</span></span> <span data-ttu-id="6fd2d-126">Back-end službu vrátí zprávy z fronty a provádí práce.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-126">A backend service pulls messages from the queue and does the work.</span></span> <span data-ttu-id="6fd2d-127">Můžete použít fronty pro zpracování obrázků: například po uživatel odešle počet souborů, vložte názvy souborů ve zprávě fronty být zachyceny back-end pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-127">You could use queues for image processing: for example, after the user uploads a number of files, put the file names in a queue message to be picked up by the backend for processing.</span></span> <span data-ttu-id="6fd2d-128">Nebo můžete použít front ke zvýšení rychlosti odezvy lokality.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-128">Or you could use queues to improve site responsiveness.</span></span> <span data-ttu-id="6fd2d-129">Například místo zápis přímo do databáze SQL, zapisovat do fronty, řekněte uživatele, kterého jste hotovi, a umožní back-end služby popisovač s vysokou latencí relační databáze pracovat.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-129">For example, instead of writing directly to a SQL database, write to a queue, tell the user you're done, and let the backend service handle high-latency relational database work.</span></span> <span data-ttu-id="6fd2d-130">Příklad fronty pro zpracování se proces bitové kopie, naleznete v části [WebJobs SDK úvodní kurz](websites-dotnet-webjobs-sdk-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6fd2d-130">For an example of queue processing with image process, see the [WebJobs SDK Get Started tutorial](websites-dotnet-webjobs-sdk-get-started.md).</span></span>
* <span data-ttu-id="6fd2d-131">RSS agregace.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-131">RSS aggregation.</span></span> <span data-ttu-id="6fd2d-132">Pokud máte lokalitu, která udržuje seznam informačních kanálů RSS, může vyžádat ve všech článků z informační kanály v procesech na pozadí.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-132">If you have a site that maintains a list of RSS feeds, you could pull in all of the articles from the feeds in a background process.</span></span>
* <span data-ttu-id="6fd2d-133">Údržba souborů, například agregování nebo čištění souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-133">File maintenance, such as aggregating or cleaning up log files.</span></span>  <span data-ttu-id="6fd2d-134">Můžete mít soubory protokolů, které vytváří pomocí několika lokalit nebo samostatné časové úseky, které chcete kombinovat ke spuštění úlohy analýzy na nich.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-134">You might have log files being created by several sites or for separate time spans which you want to combine in order to run analysis jobs on them.</span></span> <span data-ttu-id="6fd2d-135">Nebo můžete chtít naplánovat úlohu spustit týdenní vyčištění staré soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-135">Or you might want to schedule a task to run weekly to clean up old log files.</span></span>
* <span data-ttu-id="6fd2d-136">Příjem příchozích dat do tabulek Azure.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-136">Ingress into Azure Tables.</span></span> <span data-ttu-id="6fd2d-137">Můžete mít uložené soubory a objekty BLOB a chcete analyzovat jejich a uložení dat v tabulkách.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-137">You might have files stored and blobs and want to parse them and store the data in tables.</span></span> <span data-ttu-id="6fd2d-138">Funkce příjem příchozích dat může být psaní velký počet řádků (miliony v některých případech) a WebJobs SDK umožňuje snadno implementovat tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-138">The ingress function could be writing lots of rows (millions in some cases), and the WebJobs SDK makes it possible to implement this functionality easily.</span></span> <span data-ttu-id="6fd2d-139">Sada SDK také poskytuje monitorování v reálném čase indikátory průběhu například počet řádků, které jsou napsané v tabulce.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-139">The SDK also provides real-time monitoring of progress indicators such as the number of rows written in the table.</span></span>
* <span data-ttu-id="6fd2d-140">Jiné dlouhotrvající úlohy, které chcete spustit ve vláknu na pozadí, jako například [odesílání e-mailů](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure).</span><span class="sxs-lookup"><span data-stu-id="6fd2d-140">Other long-running tasks that you want to run in a background thread, such as [sending emails](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure).</span></span> 
* <span data-ttu-id="6fd2d-141">Všechny úlohy, které chcete spustit podle plánu, například při provádění operace zálohování každou noc.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-141">Any tasks that you want to run on a schedule, such as performing a back-up operation every night.</span></span>

<span data-ttu-id="6fd2d-142">V mnoha z těchto scénářů můžete škálovat webové aplikace na několika virtuálních počítačů, které by současně spustit více webové úlohy spouštět.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-142">In many of these scenarios you may want to scale a web app to run on multiple VMs, which would run multiple WebJobs simultaneously.</span></span> <span data-ttu-id="6fd2d-143">V některých scénářích výsledkem by mohlo stejná data zpracovává více než jednou, ale to není problém při používání předdefinovaných fronty, objektů blob a aktivační události služby Service Bus WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-143">In some scenarios this could result in the same data getting processed multiple times, but this is not a problem when you use the built-in queue, blob, and Service Bus triggers of the WebJobs SDK.</span></span> <span data-ttu-id="6fd2d-144">Sada SDK zajistí, že funkce budou zpracovány pouze jednou pro každou zprávu nebo objektů blob.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-144">The SDK ensures that your functions will be processed only once for each message or blob.</span></span>

<span data-ttu-id="6fd2d-145">Sada WebJobs SDK také usnadňuje zpracování běžné scénáře pro zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-145">The WebJobs SDK also makes it easy to handle common error handling scenarios.</span></span> <span data-ttu-id="6fd2d-146">Můžete nastavit výstrahy k odesílání oznámení, pokud funkci selže a vypršení časových limitů můžete nastavit tak, aby funkce se automaticky zruší, pokud není dokončen v určeném časovém limitu.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-146">You can set up alerts to send notifications when a function fails, and you can set timeouts so that a function is automatically canceled if it doesn't complete within a specified time limit.</span></span>

## <span data-ttu-id="6fd2d-147"><a id="code"></a>Ukázky kódu</span><span class="sxs-lookup"><span data-stu-id="6fd2d-147"><a id="code"></a> Code samples</span></span>
<span data-ttu-id="6fd2d-148">Kód pro zpracování typické úlohy, které fungují s Azure Storage je jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-148">The code for handling typical tasks that work with Azure Storage is simple.</span></span> <span data-ttu-id="6fd2d-149">V aplikaci konzoly `Main` metoda vytvoříte `JobHost` objekt, který koordinuje volání metody napíšete.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-149">In your Console Application's `Main` method you create a `JobHost` object that coordinates the calls to methods you write.</span></span> <span data-ttu-id="6fd2d-150">Rozhraní WebJobs SDK ví, kdy se má volat vaší metody a co hodnot parametru pro použití na základě WebJobs SDK atributů, které můžete použít v nich.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-150">The WebJobs SDK framework knows when to call your methods and what parameter values to use based on the WebJobs SDK attributes you use in them.</span></span> <span data-ttu-id="6fd2d-151">Sada SDK poskytuje *aktivační události* která určují, jaké podmínky způsobit funkce, která se má volat, a *vazače* , určete, jak získat informace o do a z parametry metody.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-151">The SDK provides *triggers* that specify what conditions cause the function to be called, and *binders* that specify how to get information into and out of method parameters.</span></span>

<span data-ttu-id="6fd2d-152">Například [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) atributu způsobí, že funkce, která se má volat při příjmu zprávy ve frontě, a pokud je zpráva formátu JSON pro bajtové pole nebo vlastního typu, zpráva je automaticky deserializovat.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-152">For example, the [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) attribute causes a function to be called when a message is received on a queue, and if the message format is JSON for a byte array or a custom type, the message is automatically deserialized.</span></span> <span data-ttu-id="6fd2d-153">[BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) atribut spustí proces vždy, když se vytvoří nový objekt blob v účtu Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-153">The [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) attribute triggers a process whenever a new blob is created in an Azure Storage account.</span></span>

<span data-ttu-id="6fd2d-154">Zde je jednoduchý program, který posuzuje fronty a vytvoří objekt blob pro každou zprávu fronty přijal:</span><span class="sxs-lookup"><span data-stu-id="6fd2d-154">Here is a simple program that polls a queue and creates a blob for each queue message received:</span></span>

        public static void Main()
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)
        {
            writer.WriteLine(inputText);
        }

<span data-ttu-id="6fd2d-155">`JobHost` Objektu je kontejner pro sadu funkcí pozadí.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-155">The `JobHost` object is a container for a set of background functions.</span></span> <span data-ttu-id="6fd2d-156">`JobHost` Objekt monitoruje funkce sleduje události, které spouštějí a provádí funkce, když dojde k aktivační události.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-156">The `JobHost` object monitors the functions, watches for events that trigger them, and executes the functions when trigger events occur.</span></span> <span data-ttu-id="6fd2d-157">Volání `JobHost` metodu za účelem určení toho, zda kontejner proces spuštěn na aktuální vlákno nebo vlákna na pozadí.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-157">You call a `JobHost` method to indicate whether you want the container process to run on the current thread or a background thread.</span></span> <span data-ttu-id="6fd2d-158">V příkladu `RunAndBlock` proces metoda nepřetržitě běží na aktuální vlákno.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-158">In the example, the `RunAndBlock` method runs the process continuously on the current thread.</span></span>

<span data-ttu-id="6fd2d-159">Protože `ProcessQueueMessage` metoda v tomto příkladu má `QueueTrigger` atribut, aktivační událost pro příjem nové zprávy fronty je této funkce.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-159">Because the `ProcessQueueMessage` method in this example has a `QueueTrigger` attribute, the trigger for that function is the reception of a new queue message.</span></span> <span data-ttu-id="6fd2d-160">`JobHost` Objekt sleduje nové zprávy fronty v zadané frontě ("webjobsqueue" v této ukázce) a když ho najde, zavolá `ProcessQueueMessage`.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-160">The `JobHost` object watches for new queue messages on the specified queue ("webjobsqueue" in this sample) and when one is found, it calls `ProcessQueueMessage`.</span></span> 

<span data-ttu-id="6fd2d-161">`QueueTrigger` Atribut vazby `inputText` parametr na hodnotu zprávy ve frontě.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-161">The `QueueTrigger` attribute binds the `inputText` parameter to the value of the queue message.</span></span> <span data-ttu-id="6fd2d-162">A `Blob` atribut vazby `TextWriter` objektu do objektu blob s názvem "blobname" v kontejner s názvem "containername".</span><span class="sxs-lookup"><span data-stu-id="6fd2d-162">And the `Blob` attribute binds a `TextWriter` object to a blob named "blobname" in a container named "containername".</span></span>  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

<span data-ttu-id="6fd2d-163">Funkce pro zápis hodnoty zprávy fronty na objekt blob použije tyto parametry:</span><span class="sxs-lookup"><span data-stu-id="6fd2d-163">The function then uses these parameters to write the value of the queue message to the blob:</span></span>

        writer.WriteLine(inputText);

<span data-ttu-id="6fd2d-164">Aktivační události a vazače funkce sady SDK webové úlohy výrazně zjednodušit kód, který máte k zápisu.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-164">The trigger and binder features of the WebJobs SDK greatly simplify the code you have to write.</span></span> <span data-ttu-id="6fd2d-165">Kód nižší úrovně, které jsou potřebné ke zpracování front, objektů BLOB nebo souborů nebo k zahájení naplánovaných úloh, je potřeba pro vás rozhraní WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-165">The low-level code required to process queues, blobs, or files, or to initiate scheduled tasks, is done for you by the WebJobs SDK framework.</span></span> <span data-ttu-id="6fd2d-166">Například rozhraní framework vytvoří fronty, které ještě nejsou otevře frontu, čtení zprávy ve frontě, odstranění fronty zpráv při zpracování byla dokončena, vytvoří kontejnery objektů blob, které neexistují ještě zapisuje do objektů BLOB a tak dále.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-166">For example, the framework creates queues that don't exist yet, opens the queue, reads queue messages, deletes queue messages when processing is completed, creates blob containers that don't exist yet, writes to blobs, and so on.</span></span>

<span data-ttu-id="6fd2d-167">Následující příklad kódu ukazuje celou řadu aktivační události v jedné webové úlohy: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, a `ErrorTrigger`.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-167">The following code example shows a variety of triggers in one WebJob: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, and `ErrorTrigger`.</span></span> 

```
    public class Functions
    {
        public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        TextWriter log)
        {
            log.WriteLine(message);
        }

        public static void ProcessFileAndUploadToBlob(
            [FileTrigger(@"import\{name}", "*.*", autoDelete: true)] Stream file,
            [Blob(@"processed/{name}", FileAccess.Write)] Stream output,
            string name,
            TextWriter log)
        {
            output = file;
            file.Close();
            log.WriteLine(string.Format("Processed input file '{0}'!", name));
        }

        [Singleton]
        public static void ProcessWebHookA([WebHookTrigger] string body, TextWriter log)
        {
            log.WriteLine(string.Format("WebHookA invoked! Body: {0}", body));
        }

        public static void ProcessGitHubWebHook([WebHookTrigger] string body, TextWriter log)
        {
            dynamic issueEvent = JObject.Parse(body);
            log.WriteLine(string.Format("GitHub WebHook invoked! ('{0}', '{1}')",
                issueEvent.issue.title, issueEvent.action));
        }

        public static void ErrorMonitor(
        [ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
        [SendGrid(
            To = "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors to the Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <span data-ttu-id="6fd2d-168"><a id="schedule"></a>Plánování</span><span class="sxs-lookup"><span data-stu-id="6fd2d-168"><a id="schedule"></a> Scheduling</span></span>
<span data-ttu-id="6fd2d-169">`TimerTrigger` Atribut vám dává možnost k funkcím aktivační událost spouštět podle plánu.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-169">The `TimerTrigger` attribute gives you the ability to trigger functions to run on a schedule.</span></span> <span data-ttu-id="6fd2d-170">Webovou úlohu můžete naplánovat jako celek prostřednictvím Azure nebo plán jednotlivých funkcí webovou úlohu pomocí WebJobs SDK `TimerTrigger`.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-170">You can schedule a WebJob as a whole through Azure or schedule individual functions of a WebJob using the WebJobs SDK `TimerTrigger`.</span></span> <span data-ttu-id="6fd2d-171">Zde je ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-171">Here's a code sample.</span></span>

```
public class Functions
{
    public static void ProcessTimer([TimerTrigger("*/15 * * * * *", RunOnStartup = true)]
    TimerInfo info, [Queue("queue")] out string message)
    {
        message = info.FormatNextOccurrences(1);
    }
}
```

<span data-ttu-id="6fd2d-172">Další ukázkový kód, najdete v části [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) v úložišti azure webjobs sdk rozšíření na webu GitHub.com.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-172">For more sample code, see [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) in the azure-webjobs-sdk-extensions repository on GitHub.com.</span></span>

## <a name="extensibility"></a><span data-ttu-id="6fd2d-173">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="6fd2d-173">Extensibility</span></span>
<span data-ttu-id="6fd2d-174">Nejste omezena na předdefinované funkci--WebJobs SDK umožňuje psát vlastní aktivační události a vazače.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-174">You're not limited to built-in functionality -- the WebJobs SDK allows you to write custom triggers and binders.</span></span>  <span data-ttu-id="6fd2d-175">Můžete například napsat aktivačních událostí pro mezipaměť události a pravidelně plány.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-175">For example, you can write triggers for cache events and periodic schedules.</span></span> <span data-ttu-id="6fd2d-176">[Úložiště s otevřeným zdrojem](https://github.com/Azure/azure-webjobs-sdk-extensions) obsahuje [podrobné příručce na rozšiřitelnost WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) a spustit ukázkový kód, které vám pomůžou psaní vlastní aktivační události a vazače.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-176">An [open source repository](https://github.com/Azure/azure-webjobs-sdk-extensions) contains a [detailed guide on WebJobs SDK extensibility](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) and sample code to help get you started writing your own triggers and binders.</span></span>

## <span data-ttu-id="6fd2d-177"><a id="workerrole"></a>Pomocí sady SDK webové úlohy mimo webové úlohy</span><span class="sxs-lookup"><span data-stu-id="6fd2d-177"><a id="workerrole"></a>Using the WebJobs SDK outside of WebJobs</span></span>
<span data-ttu-id="6fd2d-178">Program, který používá WebJobs SDK je standardní konzolové aplikace a můžou běžet kdekoli – nemá spustit jako webovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-178">A program that uses the the WebJobs SDK is a standard Console Application and can run anywhere -- it doesn't have to run as a WebJob.</span></span> <span data-ttu-id="6fd2d-179">Program můžete otestovat místně na vašem vývojovém počítači, a v produkčním prostředí můžete ji spustit v roli pracovního procesu cloudové služby nebo služby systému Windows Pokud dáváte přednost jednu z těchto prostředích.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-179">You can test the program locally on your development computer, and in production you can run it in a Cloud Service worker role or a Windows service if you prefer one of those environments.</span></span> 

<span data-ttu-id="6fd2d-180">Řídicí panel je však pouze k dispozici jako rozšíření pro webové aplikace služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-180">However, the dashboard is only available as an extension for an Azure App Service web app.</span></span> <span data-ttu-id="6fd2d-181">Pokud chcete spustit mimo webovou úlohu a stále pomocí řídicího panelu, můžete nakonfigurovat webovou aplikaci k používání stejný účet úložiště odkazující na řídicím panelu WebJobs SDK připojovací řetězec a který řídicím panelu WebJobs webové aplikace se pak zobrazí data o spuštění funkce z vaší aplikace, která je spuštěná jinde.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-181">If you want to run outside of a WebJob and still use the Dashboard, you can configure a web app to use the same storage account that your WebJobs SDK Dashboard connection string refers to, and that web app's WebJobs Dashboard will then show data about function execution from your program that is running somewhere else.</span></span> <span data-ttu-id="6fd2d-182">Na řídicím panelu můžete získat pomocí adresy URL https://*{webappname}*.scm.azurewebsites.net/azurejobs/#/functions.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-182">You can get to the Dashboard by using the URL https://*{webappname}*.scm.azurewebsites.net/azurejobs/#/functions.</span></span> <span data-ttu-id="6fd2d-183">Další informace najdete v tématu [získávání řídicí panel pro místní vývoj pomocí WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), ale Všimněte si, že v příspěvku blogu ukazuje starý název připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-183">For more information, see [Getting a dashboard for local development with the WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), but note that the blog post shows an old connection string name.</span></span> 

## <span data-ttu-id="6fd2d-184"><a id="nostorage"></a>Funkce řídicího panelu</span><span class="sxs-lookup"><span data-stu-id="6fd2d-184"><a id="nostorage"></a>Dashboard features</span></span>
<span data-ttu-id="6fd2d-185">Sada WebJobs SDK poskytuje několik výhod, i když nepoužijete WebJobs SDK aktivační události nebo vazače:</span><span class="sxs-lookup"><span data-stu-id="6fd2d-185">The WebJobs SDK provides several advantages even if you don't use WebJobs SDK triggers or binders:</span></span>

* <span data-ttu-id="6fd2d-186">Funkce z řídicího panelu můžete vyvolat.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-186">You can invoke functions from the Dashboard.</span></span>
* <span data-ttu-id="6fd2d-187">Funkce z řídicího panelu můžete opakování.</span><span class="sxs-lookup"><span data-stu-id="6fd2d-187">You can replay functions from the Dashboard.</span></span>
* <span data-ttu-id="6fd2d-188">Protokoly můžete zobrazit v řídicím panelu, propojený na konkrétní webové úlohy (protokoly aplikací, zapsány pomocí Console.Out, Console.Error, trasování, atd.) nebo propojený na volání určité funkce, které byly vytvořeny (protokoly, které jsou zapsány pomocí `TextWriter` objektu aby sada SDK předá funkce jako parametr).</span><span class="sxs-lookup"><span data-stu-id="6fd2d-188">You can view logs in the Dashboard, linked to the particular WebJob (application logs, written by using Console.Out, Console.Error, Trace, etc.) or linked to the particular function invocation that generated them (logs written by using a `TextWriter` object that the SDK passes to the function as a parameter).</span></span> 

<span data-ttu-id="6fd2d-189">Další informace najdete v tématu [postup ručně vyvolání funkce](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) a [jak napsat protokoly](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)</span><span class="sxs-lookup"><span data-stu-id="6fd2d-189">For more information, see [How to manually invoke a function](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) and [How to write logs](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)</span></span> 

## <span data-ttu-id="6fd2d-190"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="6fd2d-190"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="6fd2d-191">Další informace o WebJobs SDK najdete v tématu [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="6fd2d-191">For more information about the WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

<span data-ttu-id="6fd2d-192">Informace o nejnovějších vylepšení WebJobs SDK najdete v tématu [poznámky k verzi](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes).</span><span class="sxs-lookup"><span data-stu-id="6fd2d-192">For information about the latest enhancements to the WebJobs SDK, see the [Release Notes](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes).</span></span>

