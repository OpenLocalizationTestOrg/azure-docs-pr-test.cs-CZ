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
# <a name="azure-functions-f-developer-reference"></a><span data-ttu-id="e0990-104">Azure Functions referenční informace pro vývojáře F #</span><span class="sxs-lookup"><span data-stu-id="e0990-104">Azure Functions F# Developer Reference</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e0990-105">Skript jazyka C#</span><span class="sxs-lookup"><span data-stu-id="e0990-105">C# script</span></span>](functions-reference-csharp.md)
> * [<span data-ttu-id="e0990-106">F # skript</span><span class="sxs-lookup"><span data-stu-id="e0990-106">F# script</span></span>](functions-reference-fsharp.md)
> * [<span data-ttu-id="e0990-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="e0990-107">Node.js</span></span>](functions-reference-node.md)
> 
> 

<span data-ttu-id="e0990-108">F # pro Azure Functions je řešení umožňující snadno spouštět malé části kódu, nebo "funkce" v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="e0990-108">F# for Azure Functions is a solution for easily running small pieces of code, or "functions," in hello cloud.</span></span> <span data-ttu-id="e0990-109">Toky dat do funkce F # prostřednictvím argumenty funkce.</span><span class="sxs-lookup"><span data-stu-id="e0990-109">Data flows into your F# function via function arguments.</span></span> <span data-ttu-id="e0990-110">Argument názvy jsou určené v `function.json`, a jsou předdefinované názvy pro přístup k akce, jako je hello funkce protokolovacího nástroje a zrušení tokenů.</span><span class="sxs-lookup"><span data-stu-id="e0990-110">Argument names are specified in `function.json`, and there are predefined names for accessing things like hello function logger and cancellation tokens.</span></span>

<span data-ttu-id="e0990-111">Tento článek předpokládá, že jste si přečetli již hello [referenční informace pro vývojáře Azure Functions](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="e0990-111">This article assumes that you've already read hello [Azure Functions developer reference](functions-reference.md).</span></span>

## <a name="how-fsx-works"></a><span data-ttu-id="e0990-112">Jak funguje .fsx</span><span class="sxs-lookup"><span data-stu-id="e0990-112">How .fsx works</span></span>
<span data-ttu-id="e0990-113">`.fsx` Soubor je skript F #.</span><span class="sxs-lookup"><span data-stu-id="e0990-113">An `.fsx` file is an F# script.</span></span> <span data-ttu-id="e0990-114">Ho můžete představit jako projekt F #, který je obsažen v jednom souboru.</span><span class="sxs-lookup"><span data-stu-id="e0990-114">It can be thought of as an F# project that's contained in a single file.</span></span> <span data-ttu-id="e0990-115">Hello soubor obsahuje i kód hello k aplikaci (v tomto případě funkce Azure) a direktivy pro správu závislosti.</span><span class="sxs-lookup"><span data-stu-id="e0990-115">hello file contains both hello code for your program (in this case, your Azure Function) and directives for managing dependencies.</span></span>

<span data-ttu-id="e0990-116">Když použijete `.fsx` pro funkce Azure, běžně vyžaduje sestavení jsou pro vás, abyste toofocus na kód funkce místo "často používaný text" hello automaticky zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="e0990-116">When you use an `.fsx` for an Azure Function, commonly required assemblies are automatically included for you, allowing you toofocus on hello function rather than "boilerplate" code.</span></span>

## <a name="binding-tooarguments"></a><span data-ttu-id="e0990-117">Vazba tooarguments</span><span class="sxs-lookup"><span data-stu-id="e0990-117">Binding tooarguments</span></span>
<span data-ttu-id="e0990-118">Každou vazbu podporuje někteří sadu argumentů, jako podrobné v hello [referenční vývojáře triggerů a vazeb Azure Functions](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="e0990-118">Each binding supports some set of arguments, as detailed in hello [Azure Functions triggers and bindings developer reference](functions-triggers-bindings.md).</span></span> <span data-ttu-id="e0990-119">Například jeden argument vazeb hello, které podporuje aktivační události objektu blob je objektů POCO, které lze vyjádřit pomocí záznamu F #.</span><span class="sxs-lookup"><span data-stu-id="e0990-119">For example, one of hello argument bindings a blob trigger supports is a POCO, which can be expressed using an F# record.</span></span> <span data-ttu-id="e0990-120">Například:</span><span class="sxs-lookup"><span data-stu-id="e0990-120">For example:</span></span>

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

<span data-ttu-id="e0990-121">Funkce Azure F # bude trvat jeden nebo více argumentů.</span><span class="sxs-lookup"><span data-stu-id="e0990-121">Your F# Azure Function will take one or more arguments.</span></span> <span data-ttu-id="e0990-122">Když mluvíme o argumenty funkce Azure, označujeme příliš*vstupní* argumenty a *výstup* argumenty.</span><span class="sxs-lookup"><span data-stu-id="e0990-122">When we talk about Azure Functions arguments, we refer too*input* arguments and *output* arguments.</span></span> <span data-ttu-id="e0990-123">Vstupní argument je přesně vypadá jako: vstup tooyour funkce Azure F #.</span><span class="sxs-lookup"><span data-stu-id="e0990-123">An input argument is exactly what it sounds like: input tooyour F# Azure Function.</span></span> <span data-ttu-id="e0990-124">*Výstup* argument je měnitelný datový nebo `byref<>` argument, který slouží jako toopass data způsob zpět *out* vaší funkce.</span><span class="sxs-lookup"><span data-stu-id="e0990-124">An *output* argument is mutable data or a `byref<>` argument that serves as a way toopass data back *out* of your function.</span></span>

<span data-ttu-id="e0990-125">Ve výše uvedeném příkladu hello `blob` je vstupní argument, a `output` je argument výstup.</span><span class="sxs-lookup"><span data-stu-id="e0990-125">In hello example above, `blob` is an input argument, and `output` is an output argument.</span></span> <span data-ttu-id="e0990-126">Všimněte si, že jsme použili `byref<>` pro `output` (není bez nutnosti tooadd hello `[<Out>]` poznámky).</span><span class="sxs-lookup"><span data-stu-id="e0990-126">Notice that we used `byref<>` for `output` (there's no need tooadd hello `[<Out>]` annotation).</span></span> <span data-ttu-id="e0990-127">Použití `byref<>` typ umožňuje vaší toochange funkce, které argument hello záznamu nebo objektu odkazuje na.</span><span class="sxs-lookup"><span data-stu-id="e0990-127">Using a `byref<>` type allows your function toochange which record or object hello argument refers to.</span></span>

<span data-ttu-id="e0990-128">Když je záznam F # slouží jako vstupní typ, záznamu definice hello označena `[<CLIMutable>]` v pořadí tooallow hello Azure Functions framework tooset hello pole správně před předáním hello záznamů tooyour funkce.</span><span class="sxs-lookup"><span data-stu-id="e0990-128">When an F# record is used as an input type, hello record definition must be marked with `[<CLIMutable>]` in order tooallow hello Azure Functions framework tooset hello fields appropriately before passing hello record tooyour function.</span></span> <span data-ttu-id="e0990-129">Pod pokličkou hello `[<CLIMutable>]` generuje setter vlastnosti záznamu hello.</span><span class="sxs-lookup"><span data-stu-id="e0990-129">Under hello hood, `[<CLIMutable>]` generates setters for hello record properties.</span></span> <span data-ttu-id="e0990-130">Například:</span><span class="sxs-lookup"><span data-stu-id="e0990-130">For example:</span></span>

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

<span data-ttu-id="e0990-131">Třída F # lze také pro obě vstup a výstup argumenty.</span><span class="sxs-lookup"><span data-stu-id="e0990-131">An F# class can also be used for both in and out arguments.</span></span> <span data-ttu-id="e0990-132">Pro třídy vlastnosti obvykle potřebovat mechanismy získání a nastavení.</span><span class="sxs-lookup"><span data-stu-id="e0990-132">For a class, properties will usually need getters and setters.</span></span> <span data-ttu-id="e0990-133">Například:</span><span class="sxs-lookup"><span data-stu-id="e0990-133">For example:</span></span>

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a><span data-ttu-id="e0990-134">Protokolování</span><span class="sxs-lookup"><span data-stu-id="e0990-134">Logging</span></span>
<span data-ttu-id="e0990-135">toolog výstup tooyour [protokoly streamování](../app-service-web/web-sites-streaming-logs-and-console.md) v F # funkce zabere argument typu `TraceWriter`.</span><span class="sxs-lookup"><span data-stu-id="e0990-135">toolog output tooyour [streaming logs](../app-service-web/web-sites-streaming-logs-and-console.md) in F#, your function should take an argument of type `TraceWriter`.</span></span> <span data-ttu-id="e0990-136">Konzistence, doporučujeme, abyste tento argument je s názvem `log`.</span><span class="sxs-lookup"><span data-stu-id="e0990-136">For consistency, we recommend this argument is named `log`.</span></span> <span data-ttu-id="e0990-137">Například:</span><span class="sxs-lookup"><span data-stu-id="e0990-137">For example:</span></span>

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a><span data-ttu-id="e0990-138">Asynchronní</span><span class="sxs-lookup"><span data-stu-id="e0990-138">Async</span></span>
<span data-ttu-id="e0990-139">Hello `async` pracovní postup můžete použít, je však nutné hello výsledek tooreturn `Task`.</span><span class="sxs-lookup"><span data-stu-id="e0990-139">hello `async` workflow can be used, but hello result needs tooreturn a `Task`.</span></span> <span data-ttu-id="e0990-140">To lze provést pomocí `Async.StartAsTask`, například:</span><span class="sxs-lookup"><span data-stu-id="e0990-140">This can be done with `Async.StartAsTask`, for example:</span></span>

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a><span data-ttu-id="e0990-141">Token zrušení</span><span class="sxs-lookup"><span data-stu-id="e0990-141">Cancellation Token</span></span>
<span data-ttu-id="e0990-142">Pokud funkce potřebuje řádně toohandle vypnutí, můžete jí [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument.</span><span class="sxs-lookup"><span data-stu-id="e0990-142">If your function needs toohandle shutdown gracefully, you can give it a [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument.</span></span> <span data-ttu-id="e0990-143">To je možné kombinovat s `async`, například:</span><span class="sxs-lookup"><span data-stu-id="e0990-143">This can be combined with `async`, for example:</span></span>

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a><span data-ttu-id="e0990-144">Import obory názvů</span><span class="sxs-lookup"><span data-stu-id="e0990-144">Importing namespaces</span></span>
<span data-ttu-id="e0990-145">Obory názvů lze otevřít v hello obvyklým způsobem:</span><span class="sxs-lookup"><span data-stu-id="e0990-145">Namespaces can be opened in hello usual way:</span></span>

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

<span data-ttu-id="e0990-146">automaticky se otevře Hello následující obory názvů:</span><span class="sxs-lookup"><span data-stu-id="e0990-146">hello following namespaces are automatically opened:</span></span>

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* <span data-ttu-id="e0990-147">`Microsoft.Azure.WebJobs.Host`.</span><span class="sxs-lookup"><span data-stu-id="e0990-147">`Microsoft.Azure.WebJobs.Host`.</span></span>

## <a name="referencing-external-assemblies"></a><span data-ttu-id="e0990-148">Odkazování na externí sestavení</span><span class="sxs-lookup"><span data-stu-id="e0990-148">Referencing External Assemblies</span></span>
<span data-ttu-id="e0990-149">Podobně sestavení rozhraní přidat odkazy s hello `#r "AssemblyName"` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="e0990-149">Similarly, framework assembly references be added with hello `#r "AssemblyName"` directive.</span></span>

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

<span data-ttu-id="e0990-150">Hello následující sestavení jsou automaticky přidány hello Azure Functions hostování prostředí:</span><span class="sxs-lookup"><span data-stu-id="e0990-150">hello following assemblies are automatically added by hello Azure Functions hosting environment:</span></span>

* <span data-ttu-id="e0990-151">`mscorlib`,</span><span class="sxs-lookup"><span data-stu-id="e0990-151">`mscorlib`,</span></span>
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* <span data-ttu-id="e0990-152">`System.Net.Http.Formatting`.</span><span class="sxs-lookup"><span data-stu-id="e0990-152">`System.Net.Http.Formatting`.</span></span>

<span data-ttu-id="e0990-153">Kromě toho jsou speciální hello následující sestavení použita a může být odkazováno simplename (například `#r "AssemblyName"`):</span><span class="sxs-lookup"><span data-stu-id="e0990-153">In addition, hello following assemblies are special cased and may be referenced by simplename (e.g. `#r "AssemblyName"`):</span></span>

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* <span data-ttu-id="e0990-154">`Microsoft.AspNEt.WebHooks.Common`.</span><span class="sxs-lookup"><span data-stu-id="e0990-154">`Microsoft.AspNEt.WebHooks.Common`.</span></span>

<span data-ttu-id="e0990-155">Pokud potřebujete tooreference privátní sestavení, můžete nahrát soubor sestavení hello do `bin` referenční ho pomocí hello (např. název souboru a složky relativní tooyour funkce  `#r "MyAssembly.dll"`).</span><span class="sxs-lookup"><span data-stu-id="e0990-155">If you need tooreference a private assembly, you can upload hello assembly file into a `bin` folder relative tooyour function and reference it by using hello file name (e.g.  `#r "MyAssembly.dll"`).</span></span> <span data-ttu-id="e0990-156">Informace o způsobu tooupload tooyour funkce složku souborů najdete v části hello následující části na správy balíčků.</span><span class="sxs-lookup"><span data-stu-id="e0990-156">For information on how tooupload files tooyour function folder, see hello following section on package management.</span></span>

## <a name="editor-prelude"></a><span data-ttu-id="e0990-157">Editor Prelude</span><span class="sxs-lookup"><span data-stu-id="e0990-157">Editor Prelude</span></span>
<span data-ttu-id="e0990-158">Editor, který podporuje služby kompilátoru F # nebude vědět hello obory názvů a sestavení, které automaticky zahrne Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="e0990-158">An editor that supports F# Compiler Services will not be aware of hello namespaces and assemblies that Azure Functions automatically includes.</span></span> <span data-ttu-id="e0990-159">Jako takový může být užitečné tooinclude prelude, která pomáhá hello editor najít hello sestavení, který používáte, a tooexplicitly otevřete obory názvů.</span><span class="sxs-lookup"><span data-stu-id="e0990-159">As such, it can be useful tooinclude a prelude that helps hello editor find hello assemblies you are using, and tooexplicitly open namespaces.</span></span> <span data-ttu-id="e0990-160">Například:</span><span class="sxs-lookup"><span data-stu-id="e0990-160">For example:</span></span>

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

<span data-ttu-id="e0990-161">Když Azure Functions provede kódu, zpracovává hello zdroj s `COMPILED` definované hello editor prelude bude proto ignorován.</span><span class="sxs-lookup"><span data-stu-id="e0990-161">When Azure Functions executes your code, it processes hello source with `COMPILED` defined, so hello editor prelude will be ignored.</span></span>

<a name="package"></a>

## <a name="package-management"></a><span data-ttu-id="e0990-162">Správy balíčků</span><span class="sxs-lookup"><span data-stu-id="e0990-162">Package management</span></span>
<span data-ttu-id="e0990-163">Přidání balíčků NuGet toouse v F # funkci, `project.json` složka funkce hello toohello soubory v aplikaci funkce hello systému souborů.</span><span class="sxs-lookup"><span data-stu-id="e0990-163">toouse NuGet packages in an F# function, add a `project.json` file toohello hello function's folder in hello function app's file system.</span></span> <span data-ttu-id="e0990-164">Tady je příklad `project.json` soubor, který přidá odkaz balíčku NuGet příliš`Microsoft.ProjectOxford.Face` verze 1.1.0:</span><span class="sxs-lookup"><span data-stu-id="e0990-164">Here is an example `project.json` file that adds a NuGet package reference too`Microsoft.ProjectOxford.Face` version 1.1.0:</span></span>

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

<span data-ttu-id="e0990-165">Hello rozhraní .NET Framework 4.6 je podporována pouze, tak zkontrolujte, zda vaše `project.json` soubor Určuje `net46` jak je vidět tady.</span><span class="sxs-lookup"><span data-stu-id="e0990-165">Only hello .NET Framework 4.6 is supported, so make sure that your `project.json` file specifies `net46` as shown here.</span></span>

<span data-ttu-id="e0990-166">Když nahrajete `project.json` souboru, hello runtime získá hello balíčky a automaticky přidá reference toohello balíček sestavení.</span><span class="sxs-lookup"><span data-stu-id="e0990-166">When you upload a `project.json` file, hello runtime gets hello packages and automatically adds references toohello package assemblies.</span></span> <span data-ttu-id="e0990-167">Nepotřebujete tooadd `#r "AssemblyName"` direktivy.</span><span class="sxs-lookup"><span data-stu-id="e0990-167">You don't need tooadd `#r "AssemblyName"` directives.</span></span> <span data-ttu-id="e0990-168">Stačí přidat hello požadované `open` tooyour příkazy `.fsx` souboru.</span><span class="sxs-lookup"><span data-stu-id="e0990-168">Just add hello required `open` statements tooyour `.fsx` file.</span></span>

<span data-ttu-id="e0990-169">Můžete taky, že tooput automaticky odkazuje na sestavení v editoru prelude, tooimprove jako editor interakci s zkompilovat služby F #.</span><span class="sxs-lookup"><span data-stu-id="e0990-169">You may wish tooput automatically references assemblies in your editor prelude, tooimprove your editor's interaction with F# Compile Services.</span></span>

### <a name="how-tooadd-a-projectjson-file-tooyour-azure-function"></a><span data-ttu-id="e0990-170">Jak tooadd `project.json` souboru tooyour funkce Azure</span><span class="sxs-lookup"><span data-stu-id="e0990-170">How tooadd a `project.json` file tooyour Azure Function</span></span>
1. <span data-ttu-id="e0990-171">Začněte zajišťuje funkce aplikace běží, což lze provést otevřením funkce v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e0990-171">Begin by making sure your function app is running, which you can do by opening your function in hello Azure portal.</span></span> <span data-ttu-id="e0990-172">To také umožňuje přístup datový proud protokolů toohello kde se zobrazí výstup instalace balíčku.</span><span class="sxs-lookup"><span data-stu-id="e0990-172">This also gives access toohello streaming logs where package installation output will be displayed.</span></span>
2. <span data-ttu-id="e0990-173">tooupload `project.json` souboru, použijte jednu z metod hello popsané v [jak tooupdate funkce soubory aplikace](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="e0990-173">tooupload a `project.json` file, use one of hello methods described in [how tooupdate function app files](functions-reference.md#fileupdate).</span></span> <span data-ttu-id="e0990-174">Pokud používáte [průběžné nasazování pro Azure Functions](functions-continuous-deployment.md), můžete přidat `project.json` souboru tooyour pracovní větev v pořadí tooexperiment s ním před přidáním tooyour nasazení větve.</span><span class="sxs-lookup"><span data-stu-id="e0990-174">If you are using [Continuous Deployment for Azure Functions](functions-continuous-deployment.md), you can add a `project.json` file tooyour staging branch in order tooexperiment with it before adding it tooyour deployment branch.</span></span>
3. <span data-ttu-id="e0990-175">Po hello `project.json` se přidá soubor, zobrazí se výstup podobný toohello následující ukázka ve vaší funkci je streamování protokolu:</span><span class="sxs-lookup"><span data-stu-id="e0990-175">After hello `project.json` file is added, you will see output similar toohello following example in your function's streaming log:</span></span>

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

## <a name="environment-variables"></a><span data-ttu-id="e0990-176">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="e0990-176">Environment variables</span></span>
<span data-ttu-id="e0990-177">tooget proměnné prostředí nebo hodnotu nastavení aplikace, použijte `System.Environment.GetEnvironmentVariable`, například:</span><span class="sxs-lookup"><span data-stu-id="e0990-177">tooget an environment variable or an app setting value, use `System.Environment.GetEnvironmentVariable`, for example:</span></span>

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a><span data-ttu-id="e0990-178">Opětovné použití kódu .fsx</span><span class="sxs-lookup"><span data-stu-id="e0990-178">Reusing .fsx code</span></span>
<span data-ttu-id="e0990-179">Můžete použít kód z jiných `.fsx` soubory pomocí `#load` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="e0990-179">You can use code from other `.fsx` files by using a `#load` directive.</span></span> <span data-ttu-id="e0990-180">Například:</span><span class="sxs-lookup"><span data-stu-id="e0990-180">For example:</span></span>

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

<span data-ttu-id="e0990-181">Cesty poskytuje toohello `#load` – direktiva jsou relativní toohello umístění vaší `.fsx` souboru.</span><span class="sxs-lookup"><span data-stu-id="e0990-181">Paths provides toohello `#load` directive are relative toohello location of your `.fsx` file.</span></span>

* <span data-ttu-id="e0990-182">`#load "logger.fsx"`načte soubor umístěný ve složce funkce hello.</span><span class="sxs-lookup"><span data-stu-id="e0990-182">`#load "logger.fsx"` loads a file located in hello function folder.</span></span>
* <span data-ttu-id="e0990-183">`#load "package\logger.fsx"`načte soubor umístěný ve hello `package` složku ve složce funkce hello.</span><span class="sxs-lookup"><span data-stu-id="e0990-183">`#load "package\logger.fsx"` loads a file located in hello `package` folder in hello function folder.</span></span>
* <span data-ttu-id="e0990-184">`#load "..\shared\mylogger.fsx"`načte soubor umístěný ve hello `shared` složky na stejné úrovni jako složka funkce hello, tedy hello přímo pod `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="e0990-184">`#load "..\shared\mylogger.fsx"` loads a file located in hello `shared` folder at hello same level as hello function folder, that is, directly under `wwwroot`.</span></span>

<span data-ttu-id="e0990-185">Hello `#load` – direktiva pracuje pouze s `.fsx` soubory (F # skript) a nikoli s `.fs` soubory.</span><span class="sxs-lookup"><span data-stu-id="e0990-185">hello `#load` directive only works with `.fsx` (F# script) files, and not with `.fs` files.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0990-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0990-186">Next steps</span></span>
<span data-ttu-id="e0990-187">Další informace najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="e0990-187">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="e0990-188">Průvodce F #</span><span class="sxs-lookup"><span data-stu-id="e0990-188">F# Guide</span></span>](/dotnet/articles/fsharp/index)
* [<span data-ttu-id="e0990-189">Osvědčené postupy pro službu Azure Functions</span><span class="sxs-lookup"><span data-stu-id="e0990-189">Best Practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="e0990-190">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="e0990-190">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="e0990-191">Azure funkcí jazyka C# referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="e0990-191">Azure Functions C# developer reference</span></span>](functions-reference-csharp.md)
* [<span data-ttu-id="e0990-192">Referenční informace pro vývojáře Azure funkce NodeJS</span><span class="sxs-lookup"><span data-stu-id="e0990-192">Azure Functions NodeJS developer reference</span></span>](functions-reference-node.md)
* [<span data-ttu-id="e0990-193">Azure funkce triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="e0990-193">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)
* [<span data-ttu-id="e0990-194">Azure Functions testování</span><span class="sxs-lookup"><span data-stu-id="e0990-194">Azure Functions testing</span></span>](functions-test-a-function.md)
* [<span data-ttu-id="e0990-195">Škálování Azure Functions</span><span class="sxs-lookup"><span data-stu-id="e0990-195">Azure Functions scaling</span></span>](functions-scale.md)

