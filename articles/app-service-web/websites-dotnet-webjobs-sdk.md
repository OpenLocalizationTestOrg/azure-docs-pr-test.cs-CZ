---
title: aaaWhat je hello Azure WebJobs SDK
description: "Toohello Úvod Azure WebJobs SDK. Vysvětluje, co hello SDK je, typické scénáře, které je vhodné pro a ukázky kódu."
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
ms.openlocfilehash: efac7a75c3b68a6a6601fb298f2ccac9bd71709d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hello-azure-webjobs-sdk"></a>Co je hello Azure WebJobs SDK
## <a id="overview"></a>Přehled
Tento článek vysvětluje, co hello WebJobs SDK je, zkontroluje některé běžné scénáře je užitečné pro a poskytuje přehled o tom, jak je používat v kódu.

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

[WebJobs](websites-webjobs-resources.md) je funkce služby Azure App Service, která vám umožní toorun programu nebo skriptu v hello stejné oblasti jako webovou aplikaci, aplikace API nebo mobilní aplikace. Hello účel hello [WebJobs SDK](websites-webjobs-resources.md) je toosimplify hello kód napsaný pro běžné úkoly, které může provádět webovou úlohu, například zpracování obrázků, zpracování fronty, RSS agregace, údržba souborů a odesílání e-mailů. Hello WebJobs SDK obsahuje integrované funkce pro práci s Azure Storage a Service Bus, plánování úloh a zpracování chyb a pro jiné běžné scénáře. Kromě toho je navržen toobe extensible. Hello [WebJobs SDK je open source](https://github.com/Azure/azure-webjobs-sdk/)a dojde [otevřete zdrojové úložiště pro rozšíření](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

Hello WebJobs SDK zahrnuje hello následující součásti:

* **Balíčky NuGet**. Balíčky NuGet, abyste přidali projekt Visual Studio konzolové aplikace tooa poskytují rozhraní, které používá váš kód podle architekturu vaše metody s atributy WebJobs SDK.
* **Řídicí panel**. Součást hello WebJobs SDK je obsažena v Azure App Service a poskytuje rozšířené možnosti monitorování a Diagnostika pro programy, které používají balíčky NuGet hello. Nemáte toowrite kód toouse tyto funkce monitorování a Diagnostika.

## <a id="scenarios"></a>Scénáře
Zde jsou některé typické scénáře, které lze snadno zpracovat s hello Azure WebJobs SDK:

* Obrázek zpracování nebo jiné pracovní náročná na prostředky procesoru. Běžné funkce webů, které je hello možnost tooupload obrázky nebo videa. Interval toomanipulate hello obsah po nahraje, ale nechcete, aby čekání uživatele hello toomake při, můžete udělat.
* Fronty pro zpracování. Běžný způsob, jak toocommunicate front-endové webové službě back-end je toouse fronty. Když web hello potřebuje tooget práci, doručí zprávu do fronty. Back-end službu vrátí zpráv z fronty zpráv hello a hello práci. Můžete použít fronty pro zpracování obrázků: například po hello uživatel odešle počet souborů, vložte hello názvy souborů v toobe zprávy fronty zachyceny hello back-end pro zpracování. Nebo můžete použít fronty tooimprove lokality odezvy. Místo psaní přímo tooa SQL databáze, například zápisu tooa fronty, informace uživatele hello jste hotovi a umožní hello back-end služby popisovač s vysokou latencí relační databáze pracovat. Příklad fronty pro zpracování se proces bitové kopie, naleznete v části hello [WebJobs SDK úvodní kurz](websites-dotnet-webjobs-sdk-get-started.md).
* RSS agregace. Pokud máte lokalitu, která udržuje seznam informačních kanálů RSS, může vyžádat ve všech článků hello z hello kanály v procesech na pozadí.
* Údržba souborů, například agregování nebo čištění souborů protokolu.  Můžete mít vytváří pomocí několika lokalit nebo samostatné soubory protokolu časové úseky, které chcete toocombine v pořadí toorun analysis úlohy na ně. Nebo můžete chtít tooschedule týdenní tooclean úloh toorun až staré soubory protokolu.
* Příjem příchozích dat do tabulek Azure. Může mít soubory uložené a objekty BLOB a chcete tooparse je a ukládat hello data v tabulkách. Funkce Hello příjem příchozích dat může být psaní velký počet řádků (miliony v některých případech) a hello WebJobs SDK je možné tooimplement tuto funkci snadno. Hello SDK poskytuje monitorování v reálném čase indikátory průběhu například hello počet řádků, které jsou napsané v tabulce hello.
* Jiné dlouhotrvající úlohy, které chcete toorun ve vláknu na pozadí, jako například [odesílání e-mailů](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure). 
* Všechny úlohy, které chcete toorun podle plánu, například při provádění operace zálohování každou noc.

V mnoha z těchto scénářů může být vhodné tooscale toorun webové aplikace na několika virtuálních počítačů, které by současně spustit více webové úlohy. V některých scénářích, výsledkem by mohlo hello stejné dat zpracovává více než jednou, ale to se nejedná o problém při používání předdefinovaných fronty hello, objektů blob a aktivační události služby Service Bus Dobrý den WebJobs SDK. Hello SDK zajistí, že funkce budou zpracovány pouze jednou pro každou zprávu nebo objektů blob.

Hello WebJobs SDK také umožňuje snadno toohandle běžné scénáře zpracování chyb. Nastavením výstrahy oznámení toosend funkce se nezdaří a vypršení časových limitů můžete nastavit tak, aby funkce se automaticky zruší, pokud není dokončen v určeném časovém limitu.

## <a id="code"></a>Ukázky kódu
Hello kód pro zpracování typické úlohy, které fungují s Azure Storage je jednoduché. V aplikaci konzoly `Main` metoda vytvoříte `JobHost` objekt, který koordinuje hello volá toomethods napíšete. Hello WebJobs SDK framework zná při toocall vaší metody a jaké parametr hodnoty toouse podle hello WebJobs SDK atributy, můžete použít v nich. Hello SDK poskytuje *aktivační události* která určují, jaké podmínky způsobit toobe funkce hello názvem, a *vazače* určující, jak tooget informace do a z parametry metody.

Například hello [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) atributu způsobí, že funkce toobe, volá se při příjmu zprávy ve frontě, a pokud hello zprávy ve formátu JSON pro bajtové pole nebo vlastního typu, je automaticky deserializovat uvítací zprávu. Hello [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) atribut spustí proces vždy, když se vytvoří nový objekt blob v účtu Azure Storage.

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

Hello `JobHost` objektu je kontejner pro sadu funkcí pozadí. Hello `JobHost` hello monitorování objekt funkce, sleduje události, které spouštějí je a provádí funkce hello, pokud dojde k aktivační události. Volání `JobHost` tooindicate metoda jestli chcete hello kontejneru proces toorun na hello aktuální vlákno nebo vlákna na pozadí. V příkladu hello hello `RunAndBlock` metoda spustí proces hello nepřetržitě na aktuální vlákno hello.

Protože hello `ProcessQueueMessage` metoda v tomto příkladu má `QueueTrigger` atribut, hello aktivační událost pro tuto funkci je hello příjem nové zprávy fronty. Hello `JobHost` objekt sleduje nové zprávy fronty v zadané frontě hello ("webjobsqueue" v této ukázce) a když ho najde, zavolá `ProcessQueueMessage`. 

Hello `QueueTrigger` atribut váže hello `inputText` hodnota parametru toohello zprávy fronty hello. A hello `Blob` atribut vazby `TextWriter` objekt tooa blob s názvem "blobname" v kontejner s názvem "containername".  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

Funkce Hello pak použije tyto parametry toowrite hello hodnotu hello fronty zpráv toohello blob:

        writer.WriteLine(inputText);

Hello aktivační události a vazače funkce hello WebJobs SDK výrazně zjednodušit kód hello máte toowrite. Dobrý den, vyžaduje nízké úrovně kód tooprocess front, objektů BLOB, nebo soubory nebo tooinitiate naplánované úlohy, je potřeba pro vás hello framework WebJobs SDK. Například hello framework vytvoří fronty, které ještě neexistují, otevře hello fronty, čtení zprávy ve frontě, odstranění fronty zpráv při zpracování byla dokončena, vytvoří kontejnery objektů blob, které neexistují ještě zapíše tooblobs a tak dále.

Hello následující příklad kódu ukazuje celou řadu aktivační události v jedné webové úlohy: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, a `ErrorTrigger`. 

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
            too= "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors toohello Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <a id="schedule"></a>Plánování
Hello `TimerTrigger` atribut poskytuje hello možnost tootrigger funkce toorun podle plánu. Webovou úlohu můžete naplánovat, jak celý přes Azure nebo plán jednotlivých funkcí webové úlohy pomocí hello WebJobs SDK `TimerTrigger`. Zde je ukázka kódu.

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

Další ukázkový kód, najdete v části [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) v úložišti azure webjobs sdk rozšíření hello na webu GitHub.com.

## <a name="extensibility"></a>Rozšíření
Toobuilt v nejsou omezeny funkce – hello WebJobs SDK vám umožní toowrite vlastní triggerů a vazače.  Můžete například napsat aktivačních událostí pro mezipaměť události a pravidelně plány. [Úložiště s otevřeným zdrojem](https://github.com/Azure/azure-webjobs-sdk-extensions) obsahuje [podrobné příručce na rozšiřitelnost WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) a ukázkový kód toohelp vám pomůžou začít psaní vlastní aktivační události a vazače.

## <a id="workerrole"></a>Pomocí hello WebJobs SDK mimo webové úlohy
Program, který používá hello hello WebJobs SDK je standardní konzolové aplikace a kdekoli – můžete spustit nemá toorun jako webovou úlohu. Můžete otestovat programu hello místně na vašem vývojovém počítači a v produkčním prostředí, které můžete ho spustit v roli pracovního procesu cloudové služby nebo služby systému Windows Pokud dáváte přednost jednu z těchto prostředích. 

Řídicí panel hello je však pouze k dispozici jako rozšíření pro webové aplikace služby Azure App Service. Pokud chcete toorun mimo webovou úlohu a dál používat hello řídicího panelu, můžete nakonfigurovat webové aplikace toouse hello stejné účet úložiště, který odkazuje řídicím panelu WebJobs SDK připojovací řetězec a řídicím panelu WebJobs webové aplikace se pak zobrazí data o funkce spuštění z vaší aplikace, která je spuštěná jinde. Toohello řídicí panel můžete získat pomocí adresy URL https:// hello*{webappname}*.scm.azurewebsites.net/azurejobs/#/functions. Další informace najdete v tématu [získávání řídicí panel pro místní vývoj s hello WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), ale Všimněte si, že příspěvek blogu hello ukazuje starý název připojovacího řetězce. 

## <a id="nostorage"></a>Funkce řídicího panelu
Hello WebJobs SDK poskytuje několik výhod, i když nepoužijete WebJobs SDK aktivační události nebo vazače:

* Funkce z hello řídicí panel můžete vyvolat.
* Funkce z hello řídicí panel můžete opakování.
* Protokoly můžete zobrazit v hello řídicí panel, propojené toohello konkrétní webové úlohy (protokoly aplikací, zapsány pomocí Console.Out, Console.Error, trasování, atd.) nebo propojená volání toohello určitou funkci, které byly vytvořeny (protokoly, které jsou zapsány pomocí `TextWriter` Objekt tuto hello SDK předá jako parametr funkce toohello). 

Další informace najdete v tématu [jak toomanually vyvolání funkce](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) a [jak toowrite protokoly](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a id="nextsteps"></a>Další kroky
Další informace o hello WebJobs SDK najdete v tématu [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).

Informace o hello nejnovější vylepšení toohello WebJobs SDK najdete v tématu hello [poznámky k verzi](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes).

