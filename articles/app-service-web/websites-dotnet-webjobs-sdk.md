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
# <a name="what-is-the-azure-webjobs-sdk"></a>Co je Azure WebJobs SDK
## <a id="overview"></a>Přehled
Tento článek vysvětluje, co WebJobs SDK, zkontroluje některé běžné scénáře je užitečné pro a poskytuje přehled o tom, jak je používat v kódu.

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

[WebJobs](websites-webjobs-resources.md) je funkce služby Azure App Service, která umožňuje spustit program nebo skript v rámci stejné jako webové aplikace, aplikace API nebo mobilní aplikace. Účelem [WebJobs SDK](websites-webjobs-resources.md) je můžete zjednodušit kód napsaný pro běžné úkoly, které může provádět webovou úlohu, například zpracování obrázků, zpracování fronty, RSS agregace, údržba souborů a odesílání e-mailů. Sada SDK webové úlohy obsahuje integrované funkce pro práci s Azure Storage a Service Bus, plánování úloh a zpracování chyb a pro jiné běžné scénáře. Kromě toho je určený pro rozšiřitelnost. [WebJobs SDK je open source](https://github.com/Azure/azure-webjobs-sdk/)a dojde [otevřete zdrojové úložiště pro rozšíření](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

Sada WebJobs SDK zahrnuje následující součásti:

* **Balíčky NuGet**. Balíčky NuGet, které přidáte do projektu Visual Studio konzolové aplikace poskytují rozhraní, které používá váš kód podle architekturu vaše metody s atributy WebJobs SDK.
* **Řídicí panel**. Součást WebJobs SDK je obsažena v Azure App Service a poskytuje rozšířené možnosti monitorování a Diagnostika pro programy, které používají balíčky NuGet. Nemáte k zápisu kódu pro použití těchto funkcí monitorování a Diagnostika.

## <a id="scenarios"></a>Scénáře
Zde jsou některé typické scénáře, které lze snadno zpracovat s Azure WebJobs SDK:

* Obrázek zpracování nebo jiné pracovní náročná na prostředky procesoru. Běžné funkce webů, které je možnost odesílat obrázky nebo videa. Často budete chtít pracovat s obsahem po nahraje, ale nechcete, aby se uživatel, počkejte, můžete udělat.
* Fronty pro zpracování. Běžný způsob pro front-end webové ke komunikaci s back-end službu je používat fronty. Když web potřebuje ke své práci, doručí zprávu do fronty. Back-end službu vrátí zprávy z fronty a provádí práce. Můžete použít fronty pro zpracování obrázků: například po uživatel odešle počet souborů, vložte názvy souborů ve zprávě fronty být zachyceny back-end pro zpracování. Nebo můžete použít front ke zvýšení rychlosti odezvy lokality. Například místo zápis přímo do databáze SQL, zapisovat do fronty, řekněte uživatele, kterého jste hotovi, a umožní back-end služby popisovač s vysokou latencí relační databáze pracovat. Příklad fronty pro zpracování se proces bitové kopie, naleznete v části [WebJobs SDK úvodní kurz](websites-dotnet-webjobs-sdk-get-started.md).
* RSS agregace. Pokud máte lokalitu, která udržuje seznam informačních kanálů RSS, může vyžádat ve všech článků z informační kanály v procesech na pozadí.
* Údržba souborů, například agregování nebo čištění souborů protokolu.  Můžete mít soubory protokolů, které vytváří pomocí několika lokalit nebo samostatné časové úseky, které chcete kombinovat ke spuštění úlohy analýzy na nich. Nebo můžete chtít naplánovat úlohu spustit týdenní vyčištění staré soubory protokolu.
* Příjem příchozích dat do tabulek Azure. Můžete mít uložené soubory a objekty BLOB a chcete analyzovat jejich a uložení dat v tabulkách. Funkce příjem příchozích dat může být psaní velký počet řádků (miliony v některých případech) a WebJobs SDK umožňuje snadno implementovat tuto funkci. Sada SDK také poskytuje monitorování v reálném čase indikátory průběhu například počet řádků, které jsou napsané v tabulce.
* Jiné dlouhotrvající úlohy, které chcete spustit ve vláknu na pozadí, jako například [odesílání e-mailů](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure). 
* Všechny úlohy, které chcete spustit podle plánu, například při provádění operace zálohování každou noc.

V mnoha z těchto scénářů můžete škálovat webové aplikace na několika virtuálních počítačů, které by současně spustit více webové úlohy spouštět. V některých scénářích výsledkem by mohlo stejná data zpracovává více než jednou, ale to není problém při používání předdefinovaných fronty, objektů blob a aktivační události služby Service Bus WebJobs SDK. Sada SDK zajistí, že funkce budou zpracovány pouze jednou pro každou zprávu nebo objektů blob.

Sada WebJobs SDK také usnadňuje zpracování běžné scénáře pro zpracování chyb. Můžete nastavit výstrahy k odesílání oznámení, pokud funkci selže a vypršení časových limitů můžete nastavit tak, aby funkce se automaticky zruší, pokud není dokončen v určeném časovém limitu.

## <a id="code"></a>Ukázky kódu
Kód pro zpracování typické úlohy, které fungují s Azure Storage je jednoduchá. V aplikaci konzoly `Main` metoda vytvoříte `JobHost` objekt, který koordinuje volání metody napíšete. Rozhraní WebJobs SDK ví, kdy se má volat vaší metody a co hodnot parametru pro použití na základě WebJobs SDK atributů, které můžete použít v nich. Sada SDK poskytuje *aktivační události* která určují, jaké podmínky způsobit funkce, která se má volat, a *vazače* , určete, jak získat informace o do a z parametry metody.

Například [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) atributu způsobí, že funkce, která se má volat při příjmu zprávy ve frontě, a pokud je zpráva formátu JSON pro bajtové pole nebo vlastního typu, zpráva je automaticky deserializovat. [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) atribut spustí proces vždy, když se vytvoří nový objekt blob v účtu Azure Storage.

Zde je jednoduchý program, který posuzuje fronty a vytvoří objekt blob pro každou zprávu fronty přijal:

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

`JobHost` Objektu je kontejner pro sadu funkcí pozadí. `JobHost` Objekt monitoruje funkce sleduje události, které spouštějí a provádí funkce, když dojde k aktivační události. Volání `JobHost` metodu za účelem určení toho, zda kontejner proces spuštěn na aktuální vlákno nebo vlákna na pozadí. V příkladu `RunAndBlock` proces metoda nepřetržitě běží na aktuální vlákno.

Protože `ProcessQueueMessage` metoda v tomto příkladu má `QueueTrigger` atribut, aktivační událost pro příjem nové zprávy fronty je této funkce. `JobHost` Objekt sleduje nové zprávy fronty v zadané frontě ("webjobsqueue" v této ukázce) a když ho najde, zavolá `ProcessQueueMessage`. 

`QueueTrigger` Atribut vazby `inputText` parametr na hodnotu zprávy ve frontě. A `Blob` atribut vazby `TextWriter` objektu do objektu blob s názvem "blobname" v kontejner s názvem "containername".  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

Funkce pro zápis hodnoty zprávy fronty na objekt blob použije tyto parametry:

        writer.WriteLine(inputText);

Aktivační události a vazače funkce sady SDK webové úlohy výrazně zjednodušit kód, který máte k zápisu. Kód nižší úrovně, které jsou potřebné ke zpracování front, objektů BLOB nebo souborů nebo k zahájení naplánovaných úloh, je potřeba pro vás rozhraní WebJobs SDK. Například rozhraní framework vytvoří fronty, které ještě nejsou otevře frontu, čtení zprávy ve frontě, odstranění fronty zpráv při zpracování byla dokončena, vytvoří kontejnery objektů blob, které neexistují ještě zapisuje do objektů BLOB a tak dále.

Následující příklad kódu ukazuje celou řadu aktivační události v jedné webové úlohy: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, a `ErrorTrigger`. 

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

## <a id="schedule"></a>Plánování
`TimerTrigger` Atribut vám dává možnost k funkcím aktivační událost spouštět podle plánu. Webovou úlohu můžete naplánovat jako celek prostřednictvím Azure nebo plán jednotlivých funkcí webovou úlohu pomocí WebJobs SDK `TimerTrigger`. Zde je ukázka kódu.

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

Další ukázkový kód, najdete v části [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) v úložišti azure webjobs sdk rozšíření na webu GitHub.com.

## <a name="extensibility"></a>Rozšíření
Nejste omezena na předdefinované funkci--WebJobs SDK umožňuje psát vlastní aktivační události a vazače.  Můžete například napsat aktivačních událostí pro mezipaměť události a pravidelně plány. [Úložiště s otevřeným zdrojem](https://github.com/Azure/azure-webjobs-sdk-extensions) obsahuje [podrobné příručce na rozšiřitelnost WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) a spustit ukázkový kód, které vám pomůžou psaní vlastní aktivační události a vazače.

## <a id="workerrole"></a>Pomocí sady SDK webové úlohy mimo webové úlohy
Program, který používá WebJobs SDK je standardní konzolové aplikace a můžou běžet kdekoli – nemá spustit jako webovou úlohu. Program můžete otestovat místně na vašem vývojovém počítači, a v produkčním prostředí můžete ji spustit v roli pracovního procesu cloudové služby nebo služby systému Windows Pokud dáváte přednost jednu z těchto prostředích. 

Řídicí panel je však pouze k dispozici jako rozšíření pro webové aplikace služby Azure App Service. Pokud chcete spustit mimo webovou úlohu a stále pomocí řídicího panelu, můžete nakonfigurovat webovou aplikaci k používání stejný účet úložiště odkazující na řídicím panelu WebJobs SDK připojovací řetězec a který řídicím panelu WebJobs webové aplikace se pak zobrazí data o spuštění funkce z vaší aplikace, která je spuštěná jinde. Na řídicím panelu můžete získat pomocí adresy URL https://*{webappname}*.scm.azurewebsites.net/azurejobs/#/functions. Další informace najdete v tématu [získávání řídicí panel pro místní vývoj pomocí WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), ale Všimněte si, že v příspěvku blogu ukazuje starý název připojovacího řetězce. 

## <a id="nostorage"></a>Funkce řídicího panelu
Sada WebJobs SDK poskytuje několik výhod, i když nepoužijete WebJobs SDK aktivační události nebo vazače:

* Funkce z řídicího panelu můžete vyvolat.
* Funkce z řídicího panelu můžete opakování.
* Protokoly můžete zobrazit v řídicím panelu, propojený na konkrétní webové úlohy (protokoly aplikací, zapsány pomocí Console.Out, Console.Error, trasování, atd.) nebo propojený na volání určité funkce, které byly vytvořeny (protokoly, které jsou zapsány pomocí `TextWriter` objektu aby sada SDK předá funkce jako parametr). 

Další informace najdete v tématu [postup ručně vyvolání funkce](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) a [jak napsat protokoly](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a id="nextsteps"></a>Další kroky
Další informace o WebJobs SDK najdete v tématu [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).

Informace o nejnovějších vylepšení WebJobs SDK najdete v tématu [poznámky k verzi](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes).

