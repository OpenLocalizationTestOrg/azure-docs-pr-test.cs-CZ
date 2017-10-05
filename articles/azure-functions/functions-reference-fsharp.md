---
title: "Azure funkce F # referenční informace pro vývojáře | Microsoft Docs"
description: "Pochopit, jak vyvíjet Azure Functions pomocí F #."
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
ms.openlocfilehash: 1691d378263f6b4ce5072f5c621d8db02f774b5f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-f-developer-reference"></a><span data-ttu-id="72c3e-104">Azure Functions referenční informace pro vývojáře F #</span><span class="sxs-lookup"><span data-stu-id="72c3e-104">Azure Functions F# Developer Reference</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="72c3e-105">Skript jazyka C#</span><span class="sxs-lookup"><span data-stu-id="72c3e-105">C# script</span></span>](functions-reference-csharp.md)
> * [<span data-ttu-id="72c3e-106">F # skript</span><span class="sxs-lookup"><span data-stu-id="72c3e-106">F# script</span></span>](functions-reference-fsharp.md)
> * [<span data-ttu-id="72c3e-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="72c3e-107">Node.js</span></span>](functions-reference-node.md)
> 
> 

<span data-ttu-id="72c3e-108">F # pro Azure Functions je řešení umožňující snadno spouštět malé části kódu, nebo "funkce" v cloudu.</span><span class="sxs-lookup"><span data-stu-id="72c3e-108">F# for Azure Functions is a solution for easily running small pieces of code, or "functions," in the cloud.</span></span> <span data-ttu-id="72c3e-109">Toky dat do funkce F # prostřednictvím argumenty funkce.</span><span class="sxs-lookup"><span data-stu-id="72c3e-109">Data flows into your F# function via function arguments.</span></span> <span data-ttu-id="72c3e-110">Argument názvy jsou určené v `function.json`, a jsou předdefinované názvy pro přístup k takové věci, jako funkce protokolovacího nástroje a zrušení tokenů.</span><span class="sxs-lookup"><span data-stu-id="72c3e-110">Argument names are specified in `function.json`, and there are predefined names for accessing things like the function logger and cancellation tokens.</span></span>

<span data-ttu-id="72c3e-111">Tento článek předpokládá, že jste si již přečetli [referenční informace pro vývojáře Azure Functions](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="72c3e-111">This article assumes that you've already read the [Azure Functions developer reference](functions-reference.md).</span></span>

## <a name="how-fsx-works"></a><span data-ttu-id="72c3e-112">Jak funguje .fsx</span><span class="sxs-lookup"><span data-stu-id="72c3e-112">How .fsx works</span></span>
<span data-ttu-id="72c3e-113">`.fsx` Soubor je skript F #.</span><span class="sxs-lookup"><span data-stu-id="72c3e-113">An `.fsx` file is an F# script.</span></span> <span data-ttu-id="72c3e-114">Ho můžete představit jako projekt F #, který je obsažen v jednom souboru.</span><span class="sxs-lookup"><span data-stu-id="72c3e-114">It can be thought of as an F# project that's contained in a single file.</span></span> <span data-ttu-id="72c3e-115">Soubor obsahuje kód k aplikaci (v tomto případě funkce Azure) a direktivy pro správu závislosti.</span><span class="sxs-lookup"><span data-stu-id="72c3e-115">The file contains both the code for your program (in this case, your Azure Function) and directives for managing dependencies.</span></span>

<span data-ttu-id="72c3e-116">Když použijete `.fsx` pro funkce Azure, běžně vyžaduje sestavení jsou pro vás, což umožňuje zaměřit se na funkce, nikoli "standardní" kód automaticky zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="72c3e-116">When you use an `.fsx` for an Azure Function, commonly required assemblies are automatically included for you, allowing you to focus on the function rather than "boilerplate" code.</span></span>

## <a name="binding-to-arguments"></a><span data-ttu-id="72c3e-117">Vytvoření vazby na argumenty</span><span class="sxs-lookup"><span data-stu-id="72c3e-117">Binding to arguments</span></span>
<span data-ttu-id="72c3e-118">Každou vazbu podporuje někteří sadu argumentů, podle popisu v [referenční vývojáře triggerů a vazeb Azure Functions](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="72c3e-118">Each binding supports some set of arguments, as detailed in the [Azure Functions triggers and bindings developer reference](functions-triggers-bindings.md).</span></span> <span data-ttu-id="72c3e-119">Například jedna z vazeb argument, který podporuje aktivační události objektu blob je objektů POCO, které lze vyjádřit pomocí záznamu F #.</span><span class="sxs-lookup"><span data-stu-id="72c3e-119">For example, one of the argument bindings a blob trigger supports is a POCO, which can be expressed using an F# record.</span></span> <span data-ttu-id="72c3e-120">Například:</span><span class="sxs-lookup"><span data-stu-id="72c3e-120">For example:</span></span>

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

<span data-ttu-id="72c3e-121">Funkce Azure F # bude trvat jeden nebo více argumentů.</span><span class="sxs-lookup"><span data-stu-id="72c3e-121">Your F# Azure Function will take one or more arguments.</span></span> <span data-ttu-id="72c3e-122">Když mluvíme o argumenty funkce Azure, označujeme *vstupní* argumenty a *výstup* argumenty.</span><span class="sxs-lookup"><span data-stu-id="72c3e-122">When we talk about Azure Functions arguments, we refer to *input* arguments and *output* arguments.</span></span> <span data-ttu-id="72c3e-123">Vstupní argument je přesně vypadá jako: vstup do funkce Azure F #.</span><span class="sxs-lookup"><span data-stu-id="72c3e-123">An input argument is exactly what it sounds like: input to your F# Azure Function.</span></span> <span data-ttu-id="72c3e-124">*Výstup* argument je měnitelný datový nebo `byref<>` argument, který slouží jako způsob, jak předat data zpět *out* vaší funkce.</span><span class="sxs-lookup"><span data-stu-id="72c3e-124">An *output* argument is mutable data or a `byref<>` argument that serves as a way to pass data back *out* of your function.</span></span>

<span data-ttu-id="72c3e-125">V příkladu nahoře `blob` je vstupní argument, a `output` je argument výstup.</span><span class="sxs-lookup"><span data-stu-id="72c3e-125">In the example above, `blob` is an input argument, and `output` is an output argument.</span></span> <span data-ttu-id="72c3e-126">Všimněte si, že jsme použili `byref<>` pro `output` (je nutné přidat `[<Out>]` poznámky).</span><span class="sxs-lookup"><span data-stu-id="72c3e-126">Notice that we used `byref<>` for `output` (there's no need to add the `[<Out>]` annotation).</span></span> <span data-ttu-id="72c3e-127">Použití `byref<>` typ umožňuje funkce změnit které záznam nebo argument odkazuje na objekt.</span><span class="sxs-lookup"><span data-stu-id="72c3e-127">Using a `byref<>` type allows your function to change which record or object the argument refers to.</span></span>

<span data-ttu-id="72c3e-128">Pokud záznam F # je použita jako vstupní typ, musí být označen definici záznam s `[<CLIMutable>]` Chcete-li povolit rozhraní Azure Functions správně nastavit pole před předáním záznam funkce.</span><span class="sxs-lookup"><span data-stu-id="72c3e-128">When an F# record is used as an input type, the record definition must be marked with `[<CLIMutable>]` in order to allow the Azure Functions framework to set the fields appropriately before passing the record to your function.</span></span> <span data-ttu-id="72c3e-129">Pod pokličkou `[<CLIMutable>]` generuje setter vlastnosti záznamu.</span><span class="sxs-lookup"><span data-stu-id="72c3e-129">Under the hood, `[<CLIMutable>]` generates setters for the record properties.</span></span> <span data-ttu-id="72c3e-130">Například:</span><span class="sxs-lookup"><span data-stu-id="72c3e-130">For example:</span></span>

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

<span data-ttu-id="72c3e-131">Třída F # lze také pro obě vstup a výstup argumenty.</span><span class="sxs-lookup"><span data-stu-id="72c3e-131">An F# class can also be used for both in and out arguments.</span></span> <span data-ttu-id="72c3e-132">Pro třídy vlastnosti obvykle potřebovat mechanismy získání a nastavení.</span><span class="sxs-lookup"><span data-stu-id="72c3e-132">For a class, properties will usually need getters and setters.</span></span> <span data-ttu-id="72c3e-133">Například:</span><span class="sxs-lookup"><span data-stu-id="72c3e-133">For example:</span></span>

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a><span data-ttu-id="72c3e-134">Protokolování</span><span class="sxs-lookup"><span data-stu-id="72c3e-134">Logging</span></span>
<span data-ttu-id="72c3e-135">Protokolovat výstup do vaší [protokoly streamování](../app-service-web/web-sites-streaming-logs-and-console.md) v F # funkce zabere argument typu `TraceWriter`.</span><span class="sxs-lookup"><span data-stu-id="72c3e-135">To log output to your [streaming logs](../app-service-web/web-sites-streaming-logs-and-console.md) in F#, your function should take an argument of type `TraceWriter`.</span></span> <span data-ttu-id="72c3e-136">Konzistence, doporučujeme, abyste tento argument je s názvem `log`.</span><span class="sxs-lookup"><span data-stu-id="72c3e-136">For consistency, we recommend this argument is named `log`.</span></span> <span data-ttu-id="72c3e-137">Například:</span><span class="sxs-lookup"><span data-stu-id="72c3e-137">For example:</span></span>

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a><span data-ttu-id="72c3e-138">Asynchronní</span><span class="sxs-lookup"><span data-stu-id="72c3e-138">Async</span></span>
<span data-ttu-id="72c3e-139">`async` Pracovní postup můžete použít, ale musí vrátit výsledek `Task`.</span><span class="sxs-lookup"><span data-stu-id="72c3e-139">The `async` workflow can be used, but the result needs to return a `Task`.</span></span> <span data-ttu-id="72c3e-140">To lze provést pomocí `Async.StartAsTask`, například:</span><span class="sxs-lookup"><span data-stu-id="72c3e-140">This can be done with `Async.StartAsTask`, for example:</span></span>

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a><span data-ttu-id="72c3e-141">Token zrušení</span><span class="sxs-lookup"><span data-stu-id="72c3e-141">Cancellation Token</span></span>
<span data-ttu-id="72c3e-142">Pokud funkce potřebuje pro pohodlné zpracování vypnutí, můžete jí [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument.</span><span class="sxs-lookup"><span data-stu-id="72c3e-142">If your function needs to handle shutdown gracefully, you can give it a [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument.</span></span> <span data-ttu-id="72c3e-143">To je možné kombinovat s `async`, například:</span><span class="sxs-lookup"><span data-stu-id="72c3e-143">This can be combined with `async`, for example:</span></span>

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a><span data-ttu-id="72c3e-144">Import obory názvů</span><span class="sxs-lookup"><span data-stu-id="72c3e-144">Importing namespaces</span></span>
<span data-ttu-id="72c3e-145">Obory názvů lze otevřít obvyklým způsobem:</span><span class="sxs-lookup"><span data-stu-id="72c3e-145">Namespaces can be opened in the usual way:</span></span>

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

<span data-ttu-id="72c3e-146">Automaticky se otevře následujících oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="72c3e-146">The following namespaces are automatically opened:</span></span>

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* <span data-ttu-id="72c3e-147">`Microsoft.Azure.WebJobs.Host`.</span><span class="sxs-lookup"><span data-stu-id="72c3e-147">`Microsoft.Azure.WebJobs.Host`.</span></span>

## <a name="referencing-external-assemblies"></a><span data-ttu-id="72c3e-148">Odkazování na externí sestavení</span><span class="sxs-lookup"><span data-stu-id="72c3e-148">Referencing External Assemblies</span></span>
<span data-ttu-id="72c3e-149">Podobně sestavení rozhraní přidat s odkazy `#r "AssemblyName"` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="72c3e-149">Similarly, framework assembly references be added with the `#r "AssemblyName"` directive.</span></span>

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

<span data-ttu-id="72c3e-150">Následující sestavení jsou automaticky přidány Azure Functions hostování prostředí:</span><span class="sxs-lookup"><span data-stu-id="72c3e-150">The following assemblies are automatically added by the Azure Functions hosting environment:</span></span>

* <span data-ttu-id="72c3e-151">`mscorlib`,</span><span class="sxs-lookup"><span data-stu-id="72c3e-151">`mscorlib`,</span></span>
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* <span data-ttu-id="72c3e-152">`System.Net.Http.Formatting`.</span><span class="sxs-lookup"><span data-stu-id="72c3e-152">`System.Net.Http.Formatting`.</span></span>

<span data-ttu-id="72c3e-153">Kromě toho jsou speciální následující sestavení použita a může být odkazováno simplename (například `#r "AssemblyName"`):</span><span class="sxs-lookup"><span data-stu-id="72c3e-153">In addition, the following assemblies are special cased and may be referenced by simplename (e.g. `#r "AssemblyName"`):</span></span>

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* <span data-ttu-id="72c3e-154">`Microsoft.AspNEt.WebHooks.Common`.</span><span class="sxs-lookup"><span data-stu-id="72c3e-154">`Microsoft.AspNEt.WebHooks.Common`.</span></span>

<span data-ttu-id="72c3e-155">Pokud potřebujete odkazovat na privátní sestavení, můžete nahrát soubor sestavení do `bin` složky vzhledem ke vaší ho pomocí souboru (např. název funkce a referenční dokumentace  `#r "MyAssembly.dll"`).</span><span class="sxs-lookup"><span data-stu-id="72c3e-155">If you need to reference a private assembly, you can upload the assembly file into a `bin` folder relative to your function and reference it by using the file name (e.g.  `#r "MyAssembly.dll"`).</span></span> <span data-ttu-id="72c3e-156">Informace o tom, jak odeslat soubory do složky funkce najdete v následující části na správy balíčků.</span><span class="sxs-lookup"><span data-stu-id="72c3e-156">For information on how to upload files to your function folder, see the following section on package management.</span></span>

## <a name="editor-prelude"></a><span data-ttu-id="72c3e-157">Editor Prelude</span><span class="sxs-lookup"><span data-stu-id="72c3e-157">Editor Prelude</span></span>
<span data-ttu-id="72c3e-158">Editor, který podporuje služby kompilátoru F # nebude vědět, obory názvů a sestavení, které automaticky zahrne Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="72c3e-158">An editor that supports F# Compiler Services will not be aware of the namespaces and assemblies that Azure Functions automatically includes.</span></span> <span data-ttu-id="72c3e-159">Jako takový může být užitečné zahrnout prelude, která pomáhá editoru najít sestavení, který používáte a explicitně otevřete obory názvů.</span><span class="sxs-lookup"><span data-stu-id="72c3e-159">As such, it can be useful to include a prelude that helps the editor find the assemblies you are using, and to explicitly open namespaces.</span></span> <span data-ttu-id="72c3e-160">Například:</span><span class="sxs-lookup"><span data-stu-id="72c3e-160">For example:</span></span>

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

<span data-ttu-id="72c3e-161">Když Azure Functions provede kódu, zpracovává zdroji s `COMPILED` definované editor prelude bude proto ignorován.</span><span class="sxs-lookup"><span data-stu-id="72c3e-161">When Azure Functions executes your code, it processes the source with `COMPILED` defined, so the editor prelude will be ignored.</span></span>

<a name="package"></a>

## <a name="package-management"></a><span data-ttu-id="72c3e-162">Správy balíčků</span><span class="sxs-lookup"><span data-stu-id="72c3e-162">Package management</span></span>
<span data-ttu-id="72c3e-163">Použít balíčků NuGet v F # funkci, přidejte `project.json` do souboru složku funkce v systému souborů aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="72c3e-163">To use NuGet packages in an F# function, add a `project.json` file to the the function's folder in the function app's file system.</span></span> <span data-ttu-id="72c3e-164">Tady je příklad `project.json` soubor, který přidá odkaz na balíček NuGet `Microsoft.ProjectOxford.Face` verze 1.1.0:</span><span class="sxs-lookup"><span data-stu-id="72c3e-164">Here is an example `project.json` file that adds a NuGet package reference to `Microsoft.ProjectOxford.Face` version 1.1.0:</span></span>

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

<span data-ttu-id="72c3e-165">Je podporováno pouze rozhraní .NET Framework 4.6, tak zkontrolujte, zda vaše `project.json` soubor Určuje `net46` jak je vidět tady.</span><span class="sxs-lookup"><span data-stu-id="72c3e-165">Only the .NET Framework 4.6 is supported, so make sure that your `project.json` file specifies `net46` as shown here.</span></span>

<span data-ttu-id="72c3e-166">Když nahrajete `project.json` soubor modulu runtime získá balíčky a automaticky přidá reference na sestavení balíčku.</span><span class="sxs-lookup"><span data-stu-id="72c3e-166">When you upload a `project.json` file, the runtime gets the packages and automatically adds references to the package assemblies.</span></span> <span data-ttu-id="72c3e-167">Nemusíte přidávat `#r "AssemblyName"` direktivy.</span><span class="sxs-lookup"><span data-stu-id="72c3e-167">You don't need to add `#r "AssemblyName"` directives.</span></span> <span data-ttu-id="72c3e-168">Stačí přidat požadované `open` příkazů do vaší `.fsx` souboru.</span><span class="sxs-lookup"><span data-stu-id="72c3e-168">Just add the required `open` statements to your `.fsx` file.</span></span>

<span data-ttu-id="72c3e-169">Můžete uvést automaticky odkazy na sestavení do vaší prelude editor, ke zlepšení vaší editor interakci s zkompilovat služby F #.</span><span class="sxs-lookup"><span data-stu-id="72c3e-169">You may wish to put automatically references assemblies in your editor prelude, to improve your editor's interaction with F# Compile Services.</span></span>

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a><span data-ttu-id="72c3e-170">Postup přidání `project.json` soubor funkce Azure</span><span class="sxs-lookup"><span data-stu-id="72c3e-170">How to add a `project.json` file to your Azure Function</span></span>
1. <span data-ttu-id="72c3e-171">Začněte zajišťuje funkce aplikace běží, což lze provést otevřením funkce na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="72c3e-171">Begin by making sure your function app is running, which you can do by opening your function in the Azure portal.</span></span> <span data-ttu-id="72c3e-172">To také umožňuje přístup do protokolů streamování kde se zobrazí výstup instalace balíčku.</span><span class="sxs-lookup"><span data-stu-id="72c3e-172">This also gives access to the streaming logs where package installation output will be displayed.</span></span>
2. <span data-ttu-id="72c3e-173">Nahrát `project.json` souboru, použijte jednu z metod popsaných v [jak aktualizovat soubory aplikace funkce](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="72c3e-173">To upload a `project.json` file, use one of the methods described in [how to update function app files](functions-reference.md#fileupdate).</span></span> <span data-ttu-id="72c3e-174">Pokud používáte [průběžné nasazování pro Azure Functions](functions-continuous-deployment.md), můžete přidat `project.json` souboru do svojí pracovní větve, aby bylo možné experimentovat před přidáním do svojí větve nasazení.</span><span class="sxs-lookup"><span data-stu-id="72c3e-174">If you are using [Continuous Deployment for Azure Functions](functions-continuous-deployment.md), you can add a `project.json` file to your staging branch in order to experiment with it before adding it to your deployment branch.</span></span>
3. <span data-ttu-id="72c3e-175">Po `project.json` se přidá soubor, zobrazí se výstup podobný následujícímu příkladu funkce je streamování protokolu:</span><span class="sxs-lookup"><span data-stu-id="72c3e-175">After the `project.json` file is added, you will see output similar to the following example in your function's streaming log:</span></span>

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

## <a name="environment-variables"></a><span data-ttu-id="72c3e-176">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="72c3e-176">Environment variables</span></span>
<span data-ttu-id="72c3e-177">Proměnné prostředí nebo nastavení hodnoty aplikace, použijte `System.Environment.GetEnvironmentVariable`, například:</span><span class="sxs-lookup"><span data-stu-id="72c3e-177">To get an environment variable or an app setting value, use `System.Environment.GetEnvironmentVariable`, for example:</span></span>

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a><span data-ttu-id="72c3e-178">Opětovné použití kódu .fsx</span><span class="sxs-lookup"><span data-stu-id="72c3e-178">Reusing .fsx code</span></span>
<span data-ttu-id="72c3e-179">Můžete použít kód z jiných `.fsx` soubory pomocí `#load` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="72c3e-179">You can use code from other `.fsx` files by using a `#load` directive.</span></span> <span data-ttu-id="72c3e-180">Například:</span><span class="sxs-lookup"><span data-stu-id="72c3e-180">For example:</span></span>

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

<span data-ttu-id="72c3e-181">Poskytuje cesty `#load` direktivy jsou relativní vzhledem k umístění vaší `.fsx` souboru.</span><span class="sxs-lookup"><span data-stu-id="72c3e-181">Paths provides to the `#load` directive are relative to the location of your `.fsx` file.</span></span>

* <span data-ttu-id="72c3e-182">`#load "logger.fsx"`načte soubor umístěný ve složce funkce.</span><span class="sxs-lookup"><span data-stu-id="72c3e-182">`#load "logger.fsx"` loads a file located in the function folder.</span></span>
* <span data-ttu-id="72c3e-183">`#load "package\logger.fsx"`načte soubor umístěný ve `package` složky ve složce funkce.</span><span class="sxs-lookup"><span data-stu-id="72c3e-183">`#load "package\logger.fsx"` loads a file located in the `package` folder in the function folder.</span></span>
* <span data-ttu-id="72c3e-184">`#load "..\shared\mylogger.fsx"`načte soubor umístěný ve `shared` složky na stejné úrovni jako složka funkce, který je přímo pod `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="72c3e-184">`#load "..\shared\mylogger.fsx"` loads a file located in the `shared` folder at the same level as the function folder, that is, directly under `wwwroot`.</span></span>

<span data-ttu-id="72c3e-185">`#load` – Direktiva pracuje pouze s `.fsx` soubory (F # skript) a nikoli s `.fs` soubory.</span><span class="sxs-lookup"><span data-stu-id="72c3e-185">The `#load` directive only works with `.fsx` (F# script) files, and not with `.fs` files.</span></span>

## <a name="next-steps"></a><span data-ttu-id="72c3e-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="72c3e-186">Next steps</span></span>
<span data-ttu-id="72c3e-187">Další informace najdete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="72c3e-187">For more information, see the following resources:</span></span>

* [<span data-ttu-id="72c3e-188">Průvodce F #</span><span class="sxs-lookup"><span data-stu-id="72c3e-188">F# Guide</span></span>](/dotnet/articles/fsharp/index)
* [<span data-ttu-id="72c3e-189">Osvědčené postupy pro službu Azure Functions</span><span class="sxs-lookup"><span data-stu-id="72c3e-189">Best Practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="72c3e-190">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="72c3e-190">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="72c3e-191">Azure funkcí jazyka C# referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="72c3e-191">Azure Functions C# developer reference</span></span>](functions-reference-csharp.md)
* [<span data-ttu-id="72c3e-192">Referenční informace pro vývojáře Azure funkce NodeJS</span><span class="sxs-lookup"><span data-stu-id="72c3e-192">Azure Functions NodeJS developer reference</span></span>](functions-reference-node.md)
* [<span data-ttu-id="72c3e-193">Azure funkce triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="72c3e-193">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)
* [<span data-ttu-id="72c3e-194">Azure Functions testování</span><span class="sxs-lookup"><span data-stu-id="72c3e-194">Azure Functions testing</span></span>](functions-test-a-function.md)
* [<span data-ttu-id="72c3e-195">Škálování Azure Functions</span><span class="sxs-lookup"><span data-stu-id="72c3e-195">Azure Functions scaling</span></span>](functions-scale.md)

