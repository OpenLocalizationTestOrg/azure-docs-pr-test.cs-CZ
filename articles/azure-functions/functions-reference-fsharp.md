---
title: "aaaAzure funkce F # referenční informace pro vývojáře | Microsoft Docs"
description: "Pochopit, jak toodevelop Azure Functions pomocí F #."
services: functions
documentationcenter: fsharp
author: sylvanc
manager: jbronsk
editor: 
tags: 
keywords: "Azure funkce, funkce, událostí zpracování, webhooků, dynamické výpočetní, bez serveru architekturu, F #"
ms.assetid: e60226e5-2630-41d7-9e5b-9f9e5acc8e50
ms.service: functions
ms.devlang: fsharp
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/09/2016
ms.author: syclebsc
ms.openlocfilehash: 1ac366ba6f73d191c582dcd9214b688ef719617a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-f-developer-reference"></a>Azure Functions referenční informace pro vývojáře F #
> [!div class="op_single_selector"]
> * [Skript jazyka C#](functions-reference-csharp.md)
> * [F # skript](functions-reference-fsharp.md)
> * [Node.js](functions-reference-node.md)
> 
> 

F # pro Azure Functions je řešení umožňující snadno spouštět malé části kódu, nebo "funkce" v cloudu hello. Toky dat do funkce F # prostřednictvím argumenty funkce. Argument názvy jsou určené v `function.json`, a jsou předdefinované názvy pro přístup k akce, jako je hello funkce protokolovacího nástroje a zrušení tokenů.

Tento článek předpokládá, že jste si přečetli již hello [referenční informace pro vývojáře Azure Functions](functions-reference.md).

## <a name="how-fsx-works"></a>Jak funguje .fsx
`.fsx` Soubor je skript F #. Ho můžete představit jako projekt F #, který je obsažen v jednom souboru. Hello soubor obsahuje i kód hello k aplikaci (v tomto případě funkce Azure) a direktivy pro správu závislosti.

Když použijete `.fsx` pro funkce Azure, běžně vyžaduje sestavení jsou pro vás, abyste toofocus na kód funkce místo "často používaný text" hello automaticky zahrnuty.

## <a name="binding-tooarguments"></a>Vazba tooarguments
Každou vazbu podporuje někteří sadu argumentů, jako podrobné v hello [referenční vývojáře triggerů a vazeb Azure Functions](functions-triggers-bindings.md). Například jeden argument vazeb hello, které podporuje aktivační události objektu blob je objektů POCO, které lze vyjádřit pomocí záznamu F #. Například:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

Funkce Azure F # bude trvat jeden nebo více argumentů. Když mluvíme o argumenty funkce Azure, označujeme příliš*vstupní* argumenty a *výstup* argumenty. Vstupní argument je přesně vypadá jako: vstup tooyour funkce Azure F #. *Výstup* argument je měnitelný datový nebo `byref<>` argument, který slouží jako toopass data způsob zpět *out* vaší funkce.

Ve výše uvedeném příkladu hello `blob` je vstupní argument, a `output` je argument výstup. Všimněte si, že jsme použili `byref<>` pro `output` (není bez nutnosti tooadd hello `[<Out>]` poznámky). Použití `byref<>` typ umožňuje vaší toochange funkce, které argument hello záznamu nebo objektu odkazuje na.

Když je záznam F # slouží jako vstupní typ, záznamu definice hello označena `[<CLIMutable>]` v pořadí tooallow hello Azure Functions framework tooset hello pole správně před předáním hello záznamů tooyour funkce. Pod pokličkou hello `[<CLIMutable>]` generuje setter vlastnosti záznamu hello. Například:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

Třída F # lze také pro obě vstup a výstup argumenty. Pro třídy vlastnosti obvykle potřebovat mechanismy získání a nastavení. Například:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Protokolování
toolog výstup tooyour [protokoly streamování](../app-service-web/web-sites-streaming-logs-and-console.md) v F # funkce zabere argument typu `TraceWriter`. Konzistence, doporučujeme, abyste tento argument je s názvem `log`. Například:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Asynchronní
Hello `async` pracovní postup můžete použít, je však nutné hello výsledek tooreturn `Task`. To lze provést pomocí `Async.StartAsTask`, například:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>Token zrušení
Pokud funkce potřebuje řádně toohandle vypnutí, můžete jí [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument. To je možné kombinovat s `async`, například:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Import obory názvů
Obory názvů lze otevřít v hello obvyklým způsobem:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

automaticky se otevře Hello následující obory názvů:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Odkazování na externí sestavení
Podobně sestavení rozhraní přidat odkazy s hello `#r "AssemblyName"` – direktiva.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Hello následující sestavení jsou automaticky přidány hello Azure Functions hostování prostředí:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Kromě toho jsou speciální hello následující sestavení použita a může být odkazováno simplename (například `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Pokud potřebujete tooreference privátní sestavení, můžete nahrát soubor sestavení hello do `bin` referenční ho pomocí hello (např. název souboru a složky relativní tooyour funkce  `#r "MyAssembly.dll"`). Informace o způsobu tooupload tooyour funkce složku souborů najdete v části hello následující části na správy balíčků.

## <a name="editor-prelude"></a>Editor Prelude
Editor, který podporuje služby kompilátoru F # nebude vědět hello obory názvů a sestavení, které automaticky zahrne Azure Functions. Jako takový může být užitečné tooinclude prelude, která pomáhá hello editor najít hello sestavení, který používáte, a tooexplicitly otevřete obory názvů. Například:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

Když Azure Functions provede kódu, zpracovává hello zdroj s `COMPILED` definované hello editor prelude bude proto ignorován.

<a name="package"></a>

## <a name="package-management"></a>Správy balíčků
Přidání balíčků NuGet toouse v F # funkci, `project.json` složka funkce hello toohello soubory v aplikaci funkce hello systému souborů. Tady je příklad `project.json` soubor, který přidá odkaz balíčku NuGet příliš`Microsoft.ProjectOxford.Face` verze 1.1.0:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Hello rozhraní .NET Framework 4.6 je podporována pouze, tak zkontrolujte, zda vaše `project.json` soubor Určuje `net46` jak je vidět tady.

Když nahrajete `project.json` souboru, hello runtime získá hello balíčky a automaticky přidá reference toohello balíček sestavení. Nepotřebujete tooadd `#r "AssemblyName"` direktivy. Stačí přidat hello požadované `open` tooyour příkazy `.fsx` souboru.

Můžete taky, že tooput automaticky odkazuje na sestavení v editoru prelude, tooimprove jako editor interakci s zkompilovat služby F #.

### <a name="how-tooadd-a-projectjson-file-tooyour-azure-function"></a>Jak tooadd `project.json` souboru tooyour funkce Azure
1. Začněte zajišťuje funkce aplikace běží, což lze provést otevřením funkce v hello portálu Azure. To také umožňuje přístup datový proud protokolů toohello kde se zobrazí výstup instalace balíčku.
2. tooupload `project.json` souboru, použijte jednu z metod hello popsané v [jak tooupdate funkce soubory aplikace](functions-reference.md#fileupdate). Pokud používáte [průběžné nasazování pro Azure Functions](functions-continuous-deployment.md), můžete přidat `project.json` souboru tooyour pracovní větev v pořadí tooexperiment s ním před přidáním tooyour nasazení větve.
3. Po hello `project.json` se přidá soubor, zobrazí se výstup podobný toohello následující ukázka ve vaší funkci je streamování protokolu:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Proměnné prostředí
tooget proměnné prostředí nebo hodnotu nastavení aplikace, použijte `System.Environment.GetEnvironmentVariable`, například:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>Opětovné použití kódu .fsx
Můžete použít kód z jiných `.fsx` soubory pomocí `#load` – direktiva. Například:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Cesty poskytuje toohello `#load` – direktiva jsou relativní toohello umístění vaší `.fsx` souboru.

* `#load "logger.fsx"`načte soubor umístěný ve složce funkce hello.
* `#load "package\logger.fsx"`načte soubor umístěný ve hello `package` složku ve složce funkce hello.
* `#load "..\shared\mylogger.fsx"`načte soubor umístěný ve hello `shared` složky na stejné úrovni jako složka funkce hello, tedy hello přímo pod `wwwroot`.

Hello `#load` – direktiva pracuje pouze s `.fsx` soubory (F # skript) a nikoli s `.fs` soubory.

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello následující prostředky:

* [Průvodce F #](/dotnet/articles/fsharp/index)
* [Osvědčené postupy pro službu Azure Functions](functions-best-practices.md)
* [Referenční informace pro vývojáře Azure Functions](functions-reference.md)
* [Azure funkcí jazyka C# referenční informace pro vývojáře](functions-reference-csharp.md)
* [Referenční informace pro vývojáře Azure funkce NodeJS](functions-reference-node.md)
* [Azure funkce triggerů a vazeb](functions-triggers-bindings.md)
* [Azure Functions testování](functions-test-a-function.md)
* [Škálování Azure Functions](functions-scale.md)

