---
title: "Triggery pro aplikace API služby aaaApp | Microsoft Docs"
description: Jak tooimplement aktivuje v aplikace API v Azure App Service
services: logic-apps
documentationcenter: .net
author: guangyang
manager: erikre
editor: jimbe
ms.assetid: 493c3703-786d-4434-9dca-8f77744b2f5d
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 08/25/2016
ms.author: rachelap
ms.openlocfilehash: 2d6b6a942a23c0a93987e9c48b69ecc739bfd814
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-api-app-triggers"></a><span data-ttu-id="ead9a-103">Triggery pro aplikace API Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ead9a-103">Azure App Service API app triggers</span></span>
> [!NOTE]
> <span data-ttu-id="ead9a-104">Tato verze článku hello vztahuje verzi schématu 2014-12-01-preview tooAPI aplikace.</span><span class="sxs-lookup"><span data-stu-id="ead9a-104">This version of hello article applies tooAPI apps 2014-12-01-preview schema version.</span></span>
>
>

## <a name="overview"></a><span data-ttu-id="ead9a-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="ead9a-105">Overview</span></span>
<span data-ttu-id="ead9a-106">Tento článek vysvětluje, jak aplikaci tooimplement rozhraní API se aktivuje a využívat informace z aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="ead9a-106">This article explains how tooimplement API app triggers and consume them from a Logic app.</span></span>

<span data-ttu-id="ead9a-107">Všechny hello fragmenty kódu v tomto tématu jsou zkopírovány ze hello [ukázka kódu aplikace API FileWatcher](http://go.microsoft.com/fwlink/?LinkId=534802).</span><span class="sxs-lookup"><span data-stu-id="ead9a-107">All of hello code snippets in this topic are copied from hello [FileWatcher API App code sample](http://go.microsoft.com/fwlink/?LinkId=534802).</span></span>

<span data-ttu-id="ead9a-108">Všimněte si, že potřebujete toodownload hello následující balíček nuget pro hello kód v toobuild Tento článek a spusťte: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).</span><span class="sxs-lookup"><span data-stu-id="ead9a-108">Note that you'll need toodownload hello following nuget package for hello code in this article toobuild and run: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).</span></span>

## <a name="what-are-api-app-triggers"></a><span data-ttu-id="ead9a-109">Jaké jsou triggery pro aplikace API?</span><span class="sxs-lookup"><span data-stu-id="ead9a-109">What are API app triggers?</span></span>
<span data-ttu-id="ead9a-110">Běžný scénář pro API app toofire událost je tak, aby klienti aplikace API hello moci provést vhodnou akci hello v odpovědi toohello události.</span><span class="sxs-lookup"><span data-stu-id="ead9a-110">It's a common scenario for an API app toofire an event so that clients of hello API app can take hello appropriate action in response toohello event.</span></span> <span data-ttu-id="ead9a-111">Hello REST API na základě mechanismu, který podporuje tento scénář nazývá aktivační událostí aplikace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ead9a-111">hello REST API based mechanism that supports this scenario is called an API app trigger.</span></span>

<span data-ttu-id="ead9a-112">Předpokládejme například, že váš klientský kód používá hello [Twitter rozhraní API konektoru aplikace](../connectors/connectors-create-api-twitter.md) a váš kód potřebuje tooperform akce založené na nové tweetů, které obsahují určitá slova.</span><span class="sxs-lookup"><span data-stu-id="ead9a-112">For example, let's say your client code is using hello [Twitter Connector API app](../connectors/connectors-create-api-twitter.md) and your code needs tooperform an action based on new tweets that contain specific words.</span></span> <span data-ttu-id="ead9a-113">V takovém případě může nastavíte toofacilitate aktivační události dotazování nebo nabízená tohoto požadavku.</span><span class="sxs-lookup"><span data-stu-id="ead9a-113">In this case, you might set up a poll or push trigger toofacilitate this need.</span></span>

## <a name="poll-trigger-versus-push-trigger"></a><span data-ttu-id="ead9a-114">Aktivační událost dotazování versus aktivační událost nabízené</span><span class="sxs-lookup"><span data-stu-id="ead9a-114">Poll trigger versus push trigger</span></span>
<span data-ttu-id="ead9a-115">V současné době jsou podporovány dva typy triggerů:</span><span class="sxs-lookup"><span data-stu-id="ead9a-115">Currently, two types of triggers are supported:</span></span>

* <span data-ttu-id="ead9a-116">Aktivační událost dotazování - klient dotazuje hello aplikace API pro oznámení o události s aktivován</span><span class="sxs-lookup"><span data-stu-id="ead9a-116">Poll trigger - Client polls hello API app for notification of an event having been fired</span></span>
* <span data-ttu-id="ead9a-117">Aktivační událost nabízené - klienta je oznámeno aplikace hello API aktivuje událost</span><span class="sxs-lookup"><span data-stu-id="ead9a-117">Push trigger - Client is notified by hello API app when an event fires</span></span>

### <a name="poll-trigger"></a><span data-ttu-id="ead9a-118">Aktivační událost dotazování</span><span class="sxs-lookup"><span data-stu-id="ead9a-118">Poll trigger</span></span>
<span data-ttu-id="ead9a-119">Aktivační událost cyklického dotazování je implementovaný jako regulární rozhraní REST API a očekává jeho toopoll klientů (například aplikace logiky) v pořadí tooget oznámení.</span><span class="sxs-lookup"><span data-stu-id="ead9a-119">A poll trigger is implemented as a regular REST API and expects its clients (such as a Logic app) toopoll it in order tooget notification.</span></span> <span data-ttu-id="ead9a-120">Hello klienta může uchování stavu, i když je bezstavové cyklického dotazování aktivační událost hello sám sebe.</span><span class="sxs-lookup"><span data-stu-id="ead9a-120">While hello client may maintain state, hello poll trigger itself is stateless.</span></span>

<span data-ttu-id="ead9a-121">Následující informace o požadavku a odpovědi pakety hello Hello ilustruje několik klíčových aspektů hello cyklického dotazování aktivační událost kontraktu:</span><span class="sxs-lookup"><span data-stu-id="ead9a-121">hello following information regarding hello request and response packets illustrate some key aspects of hello poll trigger contract:</span></span>

* <span data-ttu-id="ead9a-122">Žádost</span><span class="sxs-lookup"><span data-stu-id="ead9a-122">Request</span></span>
  * <span data-ttu-id="ead9a-123">Metoda HTTP: získání</span><span class="sxs-lookup"><span data-stu-id="ead9a-123">HTTP method: GET</span></span>
  * <span data-ttu-id="ead9a-124">Parametry</span><span class="sxs-lookup"><span data-stu-id="ead9a-124">Parameters</span></span>
    * <span data-ttu-id="ead9a-125">triggerState - tento volitelný parametr, umožňuje klientům toospecify jejich stavu, který hello cyklického dotazování aktivační událost správně můžete rozhodnout, zda tooreturn oznámení nebo není na základě hello zadat stav.</span><span class="sxs-lookup"><span data-stu-id="ead9a-125">triggerState - This optional parameter allows clients toospecify their state so that hello poll trigger can properly decide whether tooreturn notification or not based on hello specified state.</span></span>
    * <span data-ttu-id="ead9a-126">Parametry specifické pro rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ead9a-126">API-specific parameters</span></span>
* <span data-ttu-id="ead9a-127">Odpověď</span><span class="sxs-lookup"><span data-stu-id="ead9a-127">Response</span></span>
  * <span data-ttu-id="ead9a-128">Stavový kód **200** – je požadavek platný a je oznámení z hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="ead9a-128">Status code **200** - Request is valid and there is a notification from hello trigger.</span></span> <span data-ttu-id="ead9a-129">obsah Hello hello oznámení bude hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ead9a-129">hello content of hello notification will be hello response body.</span></span> <span data-ttu-id="ead9a-130">Záhlaví "Opakování-po" v odpovědi hello označuje, že pomocí volání další požadavek musí načíst data další oznámení.</span><span class="sxs-lookup"><span data-stu-id="ead9a-130">A "Retry-After" header in hello response indicates that additional notification data must be retrieved with a subsequent request call.</span></span>
  * <span data-ttu-id="ead9a-131">Stavový kód **202** – je požadavek platný, ale nejsou žádná nová oznámení z hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="ead9a-131">Status code **202** - Request is valid, but there is no new notification from hello trigger.</span></span>
  * <span data-ttu-id="ead9a-132">Stavový kód **4xx** -žádosti není platný.</span><span class="sxs-lookup"><span data-stu-id="ead9a-132">Status code **4xx** - Request is not valid.</span></span> <span data-ttu-id="ead9a-133">Klient Hello nesmí opakovat požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="ead9a-133">hello client should not retry hello request.</span></span>
  * <span data-ttu-id="ead9a-134">Stavový kód **5xx** -požadavek má za následek vnitřní chybu serveru nebo dočasný problém.</span><span class="sxs-lookup"><span data-stu-id="ead9a-134">Status code **5xx** - Request has resulted in an internal server error and/or temporary issue.</span></span> <span data-ttu-id="ead9a-135">Hello klienta by měl opakovat požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="ead9a-135">hello client should retry hello request.</span></span>

<span data-ttu-id="ead9a-136">Následující fragment kódu Hello je příkladem jak aktivovat tooimplement hlasování.</span><span class="sxs-lookup"><span data-stu-id="ead9a-136">hello following code snippet is an example of how tooimplement a poll trigger.</span></span>

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check toosee whether there is any file touched after hello timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after hello timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by hello AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after hello timestamp, tell hello caller toopoll again after 1 mintue.
        else
        {
            // Extension method provided by hello AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

<span data-ttu-id="ead9a-137">tootest aktivovat tuto dotazování, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ead9a-137">tootest this poll trigger, follow these steps:</span></span>

1. <span data-ttu-id="ead9a-138">Nasazení hello rozhraní API aplikace s nastavením ověřování z **veřejné anonymní**.</span><span class="sxs-lookup"><span data-stu-id="ead9a-138">Deploy hello API App with an authentication setting of **public anonymous**.</span></span>
2. <span data-ttu-id="ead9a-139">Volání hello **touch** tootouch operace souboru.</span><span class="sxs-lookup"><span data-stu-id="ead9a-139">Call hello **touch** operation tootouch a file.</span></span> <span data-ttu-id="ead9a-140">Hello následující obrázek znázorňuje ukázková žádost prostřednictvím Postman.</span><span class="sxs-lookup"><span data-stu-id="ead9a-140">hello following image shows a sample request via Postman.</span></span>
   <span data-ttu-id="ead9a-141">![Volání operace Touch prostřednictvím Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="ead9a-141">![Call Touch Operation via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span></span>
3. <span data-ttu-id="ead9a-142">Volání hello cyklického dotazování aktivační událost s hello **triggerState** nastavený parametr tooa časové razítko předchozí tooStep #. 2.</span><span class="sxs-lookup"><span data-stu-id="ead9a-142">Call hello poll trigger with hello **triggerState** parameter set tooa time stamp prior tooStep #2.</span></span> <span data-ttu-id="ead9a-143">Hello následující obrázek znázorňuje hello ukázková žádost prostřednictvím Postman.</span><span class="sxs-lookup"><span data-stu-id="ead9a-143">hello following image shows hello sample request via Postman.</span></span>
   <span data-ttu-id="ead9a-144">![Volání cyklického dotazování aktivační události prostřednictvím Postman](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="ead9a-144">![Call Poll Trigger via Postman](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)</span></span>

### <a name="push-trigger"></a><span data-ttu-id="ead9a-145">Push aktivační události</span><span class="sxs-lookup"><span data-stu-id="ead9a-145">Push trigger</span></span>
<span data-ttu-id="ead9a-146">Aktivační událost nabízené je implementovaný jako regulární REST API, který by vložil tooclients oznámení, kdo registrovali toobe upozorněni, když konkrétní události.</span><span class="sxs-lookup"><span data-stu-id="ead9a-146">A push trigger is implemented as a regular REST API that pushes notifications tooclients who have registered toobe notified when specific events fire.</span></span>

<span data-ttu-id="ead9a-147">Následující informace o požadavku a odpovědi pakety hello Hello znázorňují některé klíčové aspekty hello nabízené aktivační událost kontrakt.</span><span class="sxs-lookup"><span data-stu-id="ead9a-147">hello following information regarding hello request and response packets illustrate some key aspects of hello push trigger contract.</span></span>

* <span data-ttu-id="ead9a-148">Žádost</span><span class="sxs-lookup"><span data-stu-id="ead9a-148">Request</span></span>
  * <span data-ttu-id="ead9a-149">Metoda HTTP: PUT</span><span class="sxs-lookup"><span data-stu-id="ead9a-149">HTTP method: PUT</span></span>
  * <span data-ttu-id="ead9a-150">Parametry</span><span class="sxs-lookup"><span data-stu-id="ead9a-150">Parameters</span></span>
    * <span data-ttu-id="ead9a-151">SpuštěcíID: požadované - neprůhledný řetězec (například identifikátor GUID), představuje hello registrace nabízených aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="ead9a-151">triggerId: required - Opaque string (such as a GUID) that represents hello registration of a push trigger.</span></span>
    * <span data-ttu-id="ead9a-152">callbackUrl: požadované – adresa URL hello zpětného volání tooinvoke při aktivuje událost hello.</span><span class="sxs-lookup"><span data-stu-id="ead9a-152">callbackUrl: required - URL of hello callback tooinvoke when hello event fires.</span></span> <span data-ttu-id="ead9a-153">vyvolání Hello je jednoduché volání POST HTTP.</span><span class="sxs-lookup"><span data-stu-id="ead9a-153">hello invocation is a simple POST HTTP call.</span></span>
    * <span data-ttu-id="ead9a-154">Parametry specifické pro rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ead9a-154">API-specific parameters</span></span>
* <span data-ttu-id="ead9a-155">Odpověď</span><span class="sxs-lookup"><span data-stu-id="ead9a-155">Response</span></span>
  * <span data-ttu-id="ead9a-156">Stavový kód **200** -požadavek tooregister klienta úspěšné.</span><span class="sxs-lookup"><span data-stu-id="ead9a-156">Status code **200** - Request tooregister client successful.</span></span>
  * <span data-ttu-id="ead9a-157">Stavový kód **4xx** -žádosti není platný.</span><span class="sxs-lookup"><span data-stu-id="ead9a-157">Status code **4xx** - Request is not valid.</span></span> <span data-ttu-id="ead9a-158">Klient Hello nesmí opakovat požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="ead9a-158">hello client should not retry hello request.</span></span>
  * <span data-ttu-id="ead9a-159">Stavový kód **5xx** -požadavek má za následek vnitřní chybu serveru nebo dočasný problém.</span><span class="sxs-lookup"><span data-stu-id="ead9a-159">Status code **5xx** - Request has resulted in an internal server error and/or temporary issue.</span></span> <span data-ttu-id="ead9a-160">Hello klienta by měl opakovat požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="ead9a-160">hello client should retry hello request.</span></span>
* <span data-ttu-id="ead9a-161">Zpětné volání</span><span class="sxs-lookup"><span data-stu-id="ead9a-161">Callback</span></span>
  * <span data-ttu-id="ead9a-162">Metoda HTTP: POST</span><span class="sxs-lookup"><span data-stu-id="ead9a-162">HTTP method: POST</span></span>
  * <span data-ttu-id="ead9a-163">Text žádosti: obsah oznámení.</span><span class="sxs-lookup"><span data-stu-id="ead9a-163">Request body: Notification content.</span></span>

<span data-ttu-id="ead9a-164">Následující fragment kódu Hello je příkladem jak aktivovat tooimplement oznámení:</span><span class="sxs-lookup"><span data-stu-id="ead9a-164">hello following code snippet is an example of how tooimplement a push trigger:</span></span>

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by hello AppService service SDK.
        // Here it defines hello input of hello push trigger is a string and hello output toohello callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register hello trigger toosome trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by hello AppService service SDK indicating hello registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher toolisten toofile change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire hello push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate hello FileSystemWatcher object with hello triggerId.
            _store[triggerId] = watcher;

        }

        // Fire hello assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed tooinvoke hello callback.
            Runtime runtime,
            // hello callback tooinvoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK tooinvoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

<span data-ttu-id="ead9a-165">tootest aktivovat tuto dotazování, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ead9a-165">tootest this poll trigger, follow these steps:</span></span>

1. <span data-ttu-id="ead9a-166">Nasazení hello rozhraní API aplikace s nastavením ověřování z **veřejné anonymní**.</span><span class="sxs-lookup"><span data-stu-id="ead9a-166">Deploy hello API App with an authentication setting of **public anonymous**.</span></span>
2. <span data-ttu-id="ead9a-167">Procházet příliš[http://requestb.in/](http://requestb.in/) toocreate RequestBin, který bude sloužit jako adresu URL zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="ead9a-167">Browse too[http://requestb.in/](http://requestb.in/) toocreate a RequestBin which will serve as your callback URL.</span></span>
3. <span data-ttu-id="ead9a-168">Volání aktivační událost nabízené hello s identifikátorem GUID jako **SpuštěcíID** a hello RequestBin adresu URL jako **callbackUrl**.</span><span class="sxs-lookup"><span data-stu-id="ead9a-168">Call hello push trigger with a GUID as **triggerId** and hello RequestBin URL as **callbackUrl**.</span></span>
   <span data-ttu-id="ead9a-169">![Aktivační událost nabízené volání prostřednictvím Postman](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="ead9a-169">![Call Push Trigger via Postman](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)</span></span>
4. <span data-ttu-id="ead9a-170">Volání hello **touch** tootouch operace souboru.</span><span class="sxs-lookup"><span data-stu-id="ead9a-170">Call hello **touch** operation tootouch a file.</span></span> <span data-ttu-id="ead9a-171">Hello následující obrázek znázorňuje ukázková žádost prostřednictvím Postman.</span><span class="sxs-lookup"><span data-stu-id="ead9a-171">hello following image shows a sample request via Postman.</span></span>
   <span data-ttu-id="ead9a-172">![Volání operace Touch prostřednictvím Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="ead9a-172">![Call Touch Operation via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span></span>
5. <span data-ttu-id="ead9a-173">Zkontrolujte, zda vlastnost výstup vyvolání hello RequestBin tooconfirm, který hello nabízené aktivační událost zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="ead9a-173">Check hello RequestBin tooconfirm that hello push trigger callback is invoked with property output.</span></span>
   <span data-ttu-id="ead9a-174">![Volání cyklického dotazování aktivační události prostřednictvím Postman](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)</span><span class="sxs-lookup"><span data-stu-id="ead9a-174">![Call Poll Trigger via Postman](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)</span></span>

### <a name="describe-triggers-in-api-definition"></a><span data-ttu-id="ead9a-175">Popisují aktivační události v definici rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ead9a-175">Describe triggers in API definition</span></span>
<span data-ttu-id="ead9a-176">Po implementaci hello aktivační události a nasazení aplikace tooAzure vaše rozhraní API, přejděte toohello **definice rozhraní API** okno v portálu Azure preview hello a dozvíte se, že aktivační události jsou automaticky rozpozná v hello uživatelské rozhraní, které vycházejí z Hello definice rozhraní API Swaggeru 2.0 aplikace hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ead9a-176">After implementing hello triggers and deploying your API app tooAzure, navigate toohello **API Definition** blade in hello Azure preview portal and you'll see that triggers are automatically recognized in hello UI, which is driven by hello Swagger 2.0 API definition of hello API app.</span></span>

![Okno Definice rozhraní API](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

<span data-ttu-id="ead9a-178">Pokud kliknete na tlačítko hello **stáhnout Swagger** tlačítko a soubor otevřít hello JSON, se zobrazí výsledky podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="ead9a-178">If you click hello **Download Swagger** button and open hello JSON file, you'll see results similar toohello following:</span></span>

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

<span data-ttu-id="ead9a-179">Vlastnost rozšíření Hello **x-ms-schedular-aktivační událost** je, jak jsou popsány v definici rozhraní API aktivační události a pokud budete požadovat definice hello rozhraní API prostřednictvím brány hello, pokud hello požadují tooone z je automaticky přidán bránou aplikace hello rozhraní API Hello následující kritéria.</span><span class="sxs-lookup"><span data-stu-id="ead9a-179">hello extension property **x-ms-schedular-trigger** is how triggers are described in API definition, and is automatically added by hello API app gateway when you request hello API definition via hello gateway if hello request tooone of hello following criteria.</span></span> <span data-ttu-id="ead9a-180">(Můžete také přidat tato vlastnost ručně.)</span><span class="sxs-lookup"><span data-stu-id="ead9a-180">(You can also add this property manually.)</span></span>

* <span data-ttu-id="ead9a-181">Aktivační událost dotazování</span><span class="sxs-lookup"><span data-stu-id="ead9a-181">Poll trigger</span></span>
  * <span data-ttu-id="ead9a-182">Pokud je hello metoda HTTP **získat**.</span><span class="sxs-lookup"><span data-stu-id="ead9a-182">If hello HTTP method is **GET**.</span></span>
  * <span data-ttu-id="ead9a-183">Pokud hello **operationId** vlastnost obsahuje řetězec hello **aktivační událost**.</span><span class="sxs-lookup"><span data-stu-id="ead9a-183">If hello **operationId** property contains hello string **trigger**.</span></span>
  * <span data-ttu-id="ead9a-184">Pokud hello **parametry** vlastnost zahrnuje parametr s **název** vlastností nastavenou příliš**triggerState**.</span><span class="sxs-lookup"><span data-stu-id="ead9a-184">If hello **parameters** property includes a parameter with a **name** property set too**triggerState**.</span></span>
* <span data-ttu-id="ead9a-185">Push aktivační události</span><span class="sxs-lookup"><span data-stu-id="ead9a-185">Push trigger</span></span>
  * <span data-ttu-id="ead9a-186">Pokud je hello metoda HTTP **PUT**.</span><span class="sxs-lookup"><span data-stu-id="ead9a-186">If hello HTTP method is **PUT**.</span></span>
  * <span data-ttu-id="ead9a-187">Pokud hello **operationId** vlastnost obsahuje řetězec hello **aktivační událost**.</span><span class="sxs-lookup"><span data-stu-id="ead9a-187">If hello **operationId** property contains hello string **trigger**.</span></span>
  * <span data-ttu-id="ead9a-188">Pokud hello **parametry** vlastnost zahrnuje parametr s **název** vlastností nastavenou příliš**SpuštěcíID**.</span><span class="sxs-lookup"><span data-stu-id="ead9a-188">If hello **parameters** property includes a parameter with a **name** property set too**triggerId**.</span></span>

## <a name="use-api-app-triggers-in-logic-apps"></a><span data-ttu-id="ead9a-189">Použití triggery pro aplikace API v Logic apps</span><span class="sxs-lookup"><span data-stu-id="ead9a-189">Use API app triggers in Logic apps</span></span>
### <a name="list-and-configure-api-app-triggers-in-hello-logic-apps-designer"></a><span data-ttu-id="ead9a-190">Zobrazit a konfigurovat triggery pro aplikace API v návrháři aplikace logiky hello</span><span class="sxs-lookup"><span data-stu-id="ead9a-190">List and configure API app triggers in hello Logic apps designer</span></span>
<span data-ttu-id="ead9a-191">Pokud vytvoříte aplikaci logiky v hello stejné skupině prostředků jako hello aplikace API, nebudete moct tooadd ho plátna návrháře toohello jednoduše tak, že na něj kliknete.</span><span class="sxs-lookup"><span data-stu-id="ead9a-191">If you create a Logic app in hello same resource group as hello API app, you will be able tooadd it toohello designer canvas simply by clicking it.</span></span> <span data-ttu-id="ead9a-192">Hello následující obrázky znázorňují toto:</span><span class="sxs-lookup"><span data-stu-id="ead9a-192">hello following images illustrate this:</span></span>

![Aktivační události v návrháři aplikace logiky](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Konfigurace cyklického dotazování aktivační události v návrháři aplikace logiky](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Konfigurovat nabízené aktivační události v návrháři aplikace logiky](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a><span data-ttu-id="ead9a-196">Optimalizace triggery pro aplikace API pro Logic apps</span><span class="sxs-lookup"><span data-stu-id="ead9a-196">Optimize API app triggers for Logic apps</span></span>
<span data-ttu-id="ead9a-197">Po přidání aktivační události tooan rozhraní API aplikace, existuje několik způsobů, jak tooimprove hello prostředí při použití aplikace hello rozhraní API v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="ead9a-197">After you add triggers tooan API app, there are a few things you can do tooimprove hello experience when using hello API app in a Logic app.</span></span>

<span data-ttu-id="ead9a-198">Například hello **triggerState** pro triggery nastavte parametr toohello následující výraz v aplikaci logiky hello.</span><span class="sxs-lookup"><span data-stu-id="ead9a-198">For instance, hello **triggerState** parameter for poll triggers should be set toohello following expression in hello Logic app.</span></span> <span data-ttu-id="ead9a-199">Tento výraz by měl vyhodnotit hello posledního vyvolání aktivační události hello z aplikace logiky hello a vrátit tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ead9a-199">This expression should evaluate hello last invocation of hello trigger from hello Logic app, and return that value.</span></span>  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

<span data-ttu-id="ead9a-200">Poznámka: Vysvětlení funkce hello použít ve výrazu hello výše, najdete v dokumentaci toohello na [jazyk definic workflowů funkce Logic App](https://msdn.microsoft.com/library/azure/dn948512.aspx).</span><span class="sxs-lookup"><span data-stu-id="ead9a-200">NOTE: For an explanation of hello functions used in hello expression above, refer toohello documentation on [Logic App Workflow Definition Language](https://msdn.microsoft.com/library/azure/dn948512.aspx).</span></span>

<span data-ttu-id="ead9a-201">Uživatelé aplikace logiky potřebovat tooprovide hello výraz výše pro hello **triggerState** parametr při použití hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="ead9a-201">Logic app users would need tooprovide hello expression above for hello **triggerState** parameter while using hello trigger.</span></span> <span data-ttu-id="ead9a-202">Je možné toohave tuto hodnotu předvolby hello logiku aplikace designeru prostřednictvím vlastnosti rozšíření hello **x-ms-scheduler doporučení**.</span><span class="sxs-lookup"><span data-stu-id="ead9a-202">It is possible toohave this value preset by hello Logic app designer through hello extension property **x-ms-scheduler-recommendation**.</span></span>  <span data-ttu-id="ead9a-203">Hello **x-ms viditelnost** rozšíření vlastnost lze nastavit hodnotu tooa *interní* tak, aby hello parametr samotné není zobrazený v designeru hello.</span><span class="sxs-lookup"><span data-stu-id="ead9a-203">hello **x-ms-visibility** extension property can be set tooa value of *internal* so that hello parameter itself is not shown on hello designer.</span></span>  <span data-ttu-id="ead9a-204">Hello následující fragment kódu ukazuje, že.</span><span class="sxs-lookup"><span data-stu-id="ead9a-204">hello following snippet illustrates that.</span></span>

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

<span data-ttu-id="ead9a-205">Triggery nabízených oznámení hello **SpuštěcíID** parametr musí jednoznačně identifikovat aplikace logiky hello.</span><span class="sxs-lookup"><span data-stu-id="ead9a-205">For push triggers, hello **triggerId** parameter must uniquely identify hello Logic app.</span></span> <span data-ttu-id="ead9a-206">Doporučené osvědčeným postupem je tooset název této vlastnosti toohello hello pracovního postupu pomocí hello následující výraz:</span><span class="sxs-lookup"><span data-stu-id="ead9a-206">A recommended best practice is tooset this property toohello name of hello workflow by using hello following expression:</span></span>

    @workflow().name

<span data-ttu-id="ead9a-207">Pomocí hello **x-ms-scheduler doporučení** a **x-ms viditelnost** – vlastnosti rozšíření v jeho linkové rozhraní API, hello aplikace API můžete vyjádřit toohello logiku aplikace návrháře tooautomatically nastavit výraz pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="ead9a-207">Using hello **x-ms-scheduler-recommendation** and **x-ms-visibility** extension properties in its API definiton, hello API app can convey toohello Logic app designer tooautomatically set this expression for hello user.</span></span>

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a><span data-ttu-id="ead9a-208">Přidání rozšíření vlastností v definice rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ead9a-208">Add extension properties in API defintion</span></span>
<span data-ttu-id="ead9a-209">Informace o dalších metadat – například vlastnosti rozšíření hello **x-ms-scheduler doporučení** a **x-ms viditelnost** -lze přidat v definice hello rozhraní API v jednom ze dvou způsobů: statické nebo dynamické.</span><span class="sxs-lookup"><span data-stu-id="ead9a-209">Additional metadata information - such as hello extension properties **x-ms-scheduler-recommendation** and **x-ms-visibility** - can be added in hello API defintion in one of two ways: static or dynamic.</span></span>

<span data-ttu-id="ead9a-210">Pro statické metadata, lze přímo upravit hello */metadata/apiDefinition.swagger.json* souborů ve vašem projektu a ručně přidejte hello vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="ead9a-210">For static metadata, you can directly edit hello */metadata/apiDefinition.swagger.json* file in your project and add hello properties manually.</span></span>

<span data-ttu-id="ead9a-211">Pro používání dynamických metadat aplikace API můžete upravit hello SwaggerConfig.cs souboru tooadd filtr operace, které můžete přidat tyto přípony.</span><span class="sxs-lookup"><span data-stu-id="ead9a-211">For API apps using dynamic metadata, you can edit hello SwaggerConfig.cs file tooadd an operation filter which can add these extensions.</span></span>

    GlobalConfiguration.Configuration
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


<span data-ttu-id="ead9a-212">Hello následuje příklad, jak tato třída může být implementováno toofacilitate hello dynamické metadata scénář.</span><span class="sxs-lookup"><span data-stu-id="ead9a-212">hello following is an example of how this class can be implemented toofacilitate hello dynamic metadata scenario.</span></span>

    // Add extension properties on hello triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set too'internal' toosignify this is an internal field
                    // x-ms-scheduler-recommendation: set tooa value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
