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
# <a name="azure-app-service-api-app-triggers"></a>Triggery pro aplikace API Azure App Service
> [!NOTE]
> Tato verze článku hello vztahuje verzi schématu 2014-12-01-preview tooAPI aplikace.
>
>

## <a name="overview"></a>Přehled
Tento článek vysvětluje, jak aplikaci tooimplement rozhraní API se aktivuje a využívat informace z aplikace logiky.

Všechny hello fragmenty kódu v tomto tématu jsou zkopírovány ze hello [ukázka kódu aplikace API FileWatcher](http://go.microsoft.com/fwlink/?LinkId=534802).

Všimněte si, že potřebujete toodownload hello následující balíček nuget pro hello kód v toobuild Tento článek a spusťte: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).

## <a name="what-are-api-app-triggers"></a>Jaké jsou triggery pro aplikace API?
Běžný scénář pro API app toofire událost je tak, aby klienti aplikace API hello moci provést vhodnou akci hello v odpovědi toohello události. Hello REST API na základě mechanismu, který podporuje tento scénář nazývá aktivační událostí aplikace rozhraní API.

Předpokládejme například, že váš klientský kód používá hello [Twitter rozhraní API konektoru aplikace](../connectors/connectors-create-api-twitter.md) a váš kód potřebuje tooperform akce založené na nové tweetů, které obsahují určitá slova. V takovém případě může nastavíte toofacilitate aktivační události dotazování nebo nabízená tohoto požadavku.

## <a name="poll-trigger-versus-push-trigger"></a>Aktivační událost dotazování versus aktivační událost nabízené
V současné době jsou podporovány dva typy triggerů:

* Aktivační událost dotazování - klient dotazuje hello aplikace API pro oznámení o události s aktivován
* Aktivační událost nabízené - klienta je oznámeno aplikace hello API aktivuje událost

### <a name="poll-trigger"></a>Aktivační událost dotazování
Aktivační událost cyklického dotazování je implementovaný jako regulární rozhraní REST API a očekává jeho toopoll klientů (například aplikace logiky) v pořadí tooget oznámení. Hello klienta může uchování stavu, i když je bezstavové cyklického dotazování aktivační událost hello sám sebe.

Následující informace o požadavku a odpovědi pakety hello Hello ilustruje několik klíčových aspektů hello cyklického dotazování aktivační událost kontraktu:

* Žádost
  * Metoda HTTP: získání
  * Parametry
    * triggerState - tento volitelný parametr, umožňuje klientům toospecify jejich stavu, který hello cyklického dotazování aktivační událost správně můžete rozhodnout, zda tooreturn oznámení nebo není na základě hello zadat stav.
    * Parametry specifické pro rozhraní API
* Odpověď
  * Stavový kód **200** – je požadavek platný a je oznámení z hello aktivační události. obsah Hello hello oznámení bude hello odpovědi. Záhlaví "Opakování-po" v odpovědi hello označuje, že pomocí volání další požadavek musí načíst data další oznámení.
  * Stavový kód **202** – je požadavek platný, ale nejsou žádná nová oznámení z hello aktivační události.
  * Stavový kód **4xx** -žádosti není platný. Klient Hello nesmí opakovat požadavek hello.
  * Stavový kód **5xx** -požadavek má za následek vnitřní chybu serveru nebo dočasný problém. Hello klienta by měl opakovat požadavek hello.

Následující fragment kódu Hello je příkladem jak aktivovat tooimplement hlasování.

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

tootest aktivovat tuto dotazování, postupujte takto:

1. Nasazení hello rozhraní API aplikace s nastavením ověřování z **veřejné anonymní**.
2. Volání hello **touch** tootouch operace souboru. Hello následující obrázek znázorňuje ukázková žádost prostřednictvím Postman.
   ![Volání operace Touch prostřednictvím Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. Volání hello cyklického dotazování aktivační událost s hello **triggerState** nastavený parametr tooa časové razítko předchozí tooStep #. 2. Hello následující obrázek znázorňuje hello ukázková žádost prostřednictvím Postman.
   ![Volání cyklického dotazování aktivační události prostřednictvím Postman](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>Push aktivační události
Aktivační událost nabízené je implementovaný jako regulární REST API, který by vložil tooclients oznámení, kdo registrovali toobe upozorněni, když konkrétní události.

Následující informace o požadavku a odpovědi pakety hello Hello znázorňují některé klíčové aspekty hello nabízené aktivační událost kontrakt.

* Žádost
  * Metoda HTTP: PUT
  * Parametry
    * SpuštěcíID: požadované - neprůhledný řetězec (například identifikátor GUID), představuje hello registrace nabízených aktivační událost.
    * callbackUrl: požadované – adresa URL hello zpětného volání tooinvoke při aktivuje událost hello. vyvolání Hello je jednoduché volání POST HTTP.
    * Parametry specifické pro rozhraní API
* Odpověď
  * Stavový kód **200** -požadavek tooregister klienta úspěšné.
  * Stavový kód **4xx** -žádosti není platný. Klient Hello nesmí opakovat požadavek hello.
  * Stavový kód **5xx** -požadavek má za následek vnitřní chybu serveru nebo dočasný problém. Hello klienta by měl opakovat požadavek hello.
* Zpětné volání
  * Metoda HTTP: POST
  * Text žádosti: obsah oznámení.

Následující fragment kódu Hello je příkladem jak aktivovat tooimplement oznámení:

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

tootest aktivovat tuto dotazování, postupujte takto:

1. Nasazení hello rozhraní API aplikace s nastavením ověřování z **veřejné anonymní**.
2. Procházet příliš[http://requestb.in/](http://requestb.in/) toocreate RequestBin, který bude sloužit jako adresu URL zpětné volání.
3. Volání aktivační událost nabízené hello s identifikátorem GUID jako **SpuštěcíID** a hello RequestBin adresu URL jako **callbackUrl**.
   ![Aktivační událost nabízené volání prostřednictvím Postman](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. Volání hello **touch** tootouch operace souboru. Hello následující obrázek znázorňuje ukázková žádost prostřednictvím Postman.
   ![Volání operace Touch prostřednictvím Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. Zkontrolujte, zda vlastnost výstup vyvolání hello RequestBin tooconfirm, který hello nabízené aktivační událost zpětného volání.
   ![Volání cyklického dotazování aktivační události prostřednictvím Postman](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>Popisují aktivační události v definici rozhraní API
Po implementaci hello aktivační události a nasazení aplikace tooAzure vaše rozhraní API, přejděte toohello **definice rozhraní API** okno v portálu Azure preview hello a dozvíte se, že aktivační události jsou automaticky rozpozná v hello uživatelské rozhraní, které vycházejí z Hello definice rozhraní API Swaggeru 2.0 aplikace hello rozhraní API.

![Okno Definice rozhraní API](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

Pokud kliknete na tlačítko hello **stáhnout Swagger** tlačítko a soubor otevřít hello JSON, se zobrazí výsledky podobné toohello následující:

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

Vlastnost rozšíření Hello **x-ms-schedular-aktivační událost** je, jak jsou popsány v definici rozhraní API aktivační události a pokud budete požadovat definice hello rozhraní API prostřednictvím brány hello, pokud hello požadují tooone z je automaticky přidán bránou aplikace hello rozhraní API Hello následující kritéria. (Můžete také přidat tato vlastnost ručně.)

* Aktivační událost dotazování
  * Pokud je hello metoda HTTP **získat**.
  * Pokud hello **operationId** vlastnost obsahuje řetězec hello **aktivační událost**.
  * Pokud hello **parametry** vlastnost zahrnuje parametr s **název** vlastností nastavenou příliš**triggerState**.
* Push aktivační události
  * Pokud je hello metoda HTTP **PUT**.
  * Pokud hello **operationId** vlastnost obsahuje řetězec hello **aktivační událost**.
  * Pokud hello **parametry** vlastnost zahrnuje parametr s **název** vlastností nastavenou příliš**SpuštěcíID**.

## <a name="use-api-app-triggers-in-logic-apps"></a>Použití triggery pro aplikace API v Logic apps
### <a name="list-and-configure-api-app-triggers-in-hello-logic-apps-designer"></a>Zobrazit a konfigurovat triggery pro aplikace API v návrháři aplikace logiky hello
Pokud vytvoříte aplikaci logiky v hello stejné skupině prostředků jako hello aplikace API, nebudete moct tooadd ho plátna návrháře toohello jednoduše tak, že na něj kliknete. Hello následující obrázky znázorňují toto:

![Aktivační události v návrháři aplikace logiky](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Konfigurace cyklického dotazování aktivační události v návrháři aplikace logiky](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Konfigurovat nabízené aktivační události v návrháři aplikace logiky](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>Optimalizace triggery pro aplikace API pro Logic apps
Po přidání aktivační události tooan rozhraní API aplikace, existuje několik způsobů, jak tooimprove hello prostředí při použití aplikace hello rozhraní API v aplikaci logiky.

Například hello **triggerState** pro triggery nastavte parametr toohello následující výraz v aplikaci logiky hello. Tento výraz by měl vyhodnotit hello posledního vyvolání aktivační události hello z aplikace logiky hello a vrátit tuto hodnotu.  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

Poznámka: Vysvětlení funkce hello použít ve výrazu hello výše, najdete v dokumentaci toohello na [jazyk definic workflowů funkce Logic App](https://msdn.microsoft.com/library/azure/dn948512.aspx).

Uživatelé aplikace logiky potřebovat tooprovide hello výraz výše pro hello **triggerState** parametr při použití hello aktivační události. Je možné toohave tuto hodnotu předvolby hello logiku aplikace designeru prostřednictvím vlastnosti rozšíření hello **x-ms-scheduler doporučení**.  Hello **x-ms viditelnost** rozšíření vlastnost lze nastavit hodnotu tooa *interní* tak, aby hello parametr samotné není zobrazený v designeru hello.  Hello následující fragment kódu ukazuje, že.

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

Triggery nabízených oznámení hello **SpuštěcíID** parametr musí jednoznačně identifikovat aplikace logiky hello. Doporučené osvědčeným postupem je tooset název této vlastnosti toohello hello pracovního postupu pomocí hello následující výraz:

    @workflow().name

Pomocí hello **x-ms-scheduler doporučení** a **x-ms viditelnost** – vlastnosti rozšíření v jeho linkové rozhraní API, hello aplikace API můžete vyjádřit toohello logiku aplikace návrháře tooautomatically nastavit výraz pro uživatele hello.

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a>Přidání rozšíření vlastností v definice rozhraní API
Informace o dalších metadat – například vlastnosti rozšíření hello **x-ms-scheduler doporučení** a **x-ms viditelnost** -lze přidat v definice hello rozhraní API v jednom ze dvou způsobů: statické nebo dynamické.

Pro statické metadata, lze přímo upravit hello */metadata/apiDefinition.swagger.json* souborů ve vašem projektu a ručně přidejte hello vlastnosti.

Pro používání dynamických metadat aplikace API můžete upravit hello SwaggerConfig.cs souboru tooadd filtr operace, které můžete přidat tyto přípony.

    GlobalConfiguration.Configuration
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


Hello následuje příklad, jak tato třída může být implementováno toofacilitate hello dynamické metadata scénář.

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
