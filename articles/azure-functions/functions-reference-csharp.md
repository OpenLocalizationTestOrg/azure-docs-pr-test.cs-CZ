---
title: "aaaAzure funkce jazyka C# skript referenční informace pro vývojáře | Microsoft Docs"
description: "Pochopit, jak toodevelop Azure Functions pomocí jazyka C#."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "funkce azure, funkce, zpracování událostí, webhook, dynamické výpočty, architektura bez serverů"
ms.assetid: f28cda01-15f3-4047-83f3-e89d5728301c
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/07/2017
ms.author: donnam
ms.openlocfilehash: 27a8f4eb77497a373ff4031539e2e930585e48e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-c-script-developer-reference"></a><span data-ttu-id="71f54-104">Azure funkcí jazyka C# skript referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="71f54-104">Azure Functions C# script developer reference</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="71f54-105">Skript jazyka C#</span><span class="sxs-lookup"><span data-stu-id="71f54-105">C# script</span></span>](functions-reference-csharp.md)
> * [<span data-ttu-id="71f54-106">F # skript</span><span class="sxs-lookup"><span data-stu-id="71f54-106">F# script</span></span>](functions-reference-fsharp.md)
> * [<span data-ttu-id="71f54-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="71f54-107">Node.js</span></span>](functions-reference-node.md)
>
>

<span data-ttu-id="71f54-108">Hello C# skript prostředí pro Azure Functions je založena na hello Azure WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="71f54-108">hello C# script experience for Azure Functions is based on hello Azure WebJobs SDK.</span></span> <span data-ttu-id="71f54-109">Toky dat do funkce jazyka C# prostřednictvím metoda argumenty.</span><span class="sxs-lookup"><span data-stu-id="71f54-109">Data flows into your C# function via method arguments.</span></span> <span data-ttu-id="71f54-110">Argument názvy jsou určené v `function.json`, a jsou předdefinované názvy pro přístup k akce, jako je hello funkce protokolovacího nástroje a zrušení tokenů.</span><span class="sxs-lookup"><span data-stu-id="71f54-110">Argument names are specified in `function.json`, and there are predefined names for accessing things like hello function logger and cancellation tokens.</span></span>

<span data-ttu-id="71f54-111">Tento článek předpokládá, že jste si přečetli již hello [referenční informace pro vývojáře Azure Functions](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="71f54-111">This article assumes that you've already read hello [Azure Functions developer reference](functions-reference.md).</span></span>

<span data-ttu-id="71f54-112">Informace o použití jazyka C# třídy knihovny najdete v tématu [knihovny tříd pomocí rozhraní .NET s Azure Functions](functions-dotnet-class-library.md).</span><span class="sxs-lookup"><span data-stu-id="71f54-112">For information on using C# class libraries, see [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md).</span></span>

## <a name="how-csx-works"></a><span data-ttu-id="71f54-113">Jak funguje .csx</span><span class="sxs-lookup"><span data-stu-id="71f54-113">How .csx works</span></span>
<span data-ttu-id="71f54-114">Hello `.csx` formátu vám umožní toowrite méně "standardní" a zaměřit na zápis právě C# funkce.</span><span class="sxs-lookup"><span data-stu-id="71f54-114">hello `.csx` format allows you toowrite less "boilerplate" and focus on writing just a C# function.</span></span> <span data-ttu-id="71f54-115">Zahrnout všechny odkazy na sestavení a obory názvů na začátku hello hello soubor jako obvykle.</span><span class="sxs-lookup"><span data-stu-id="71f54-115">Include any assembly references and namespaces at hello beginning of hello file as usual.</span></span> <span data-ttu-id="71f54-116">Místo zabalení všechno, co v oboru názvů a třídy, pouze definuje `Run` metoda.</span><span class="sxs-lookup"><span data-stu-id="71f54-116">Instead of wrapping everything in a namespace and class, just define a `Run` method.</span></span> <span data-ttu-id="71f54-117">Pokud potřebujete tooinclude všechny třídy pro instanci toodefine prostý objekty původního objektu CLR (objektů POCO), můžete použít třídu uvnitř hello stejného souboru.</span><span class="sxs-lookup"><span data-stu-id="71f54-117">If you need tooinclude any classes, for instance toodefine Plain Old CLR Object (POCO) objects, you can include a class inside hello same file.</span></span>   

## <a name="binding-tooarguments"></a><span data-ttu-id="71f54-118">Vazba tooarguments</span><span class="sxs-lookup"><span data-stu-id="71f54-118">Binding tooarguments</span></span>
<span data-ttu-id="71f54-119">Hello různé vazby jsou vázané tooa C# funkce prostřednictvím hello `name` vlastnost hello *function.json* konfigurace.</span><span class="sxs-lookup"><span data-stu-id="71f54-119">hello various bindings are bound tooa C# function via hello `name` property in hello *function.json* configuration.</span></span> <span data-ttu-id="71f54-120">Každé vazby má svou vlastní podporované typy; řetězec, objektů POCO nebo CloudBlockBlob může například podporovat aktivační události objektu blob.</span><span class="sxs-lookup"><span data-stu-id="71f54-120">Each binding has its own supported types; for instance, a blob trigger can support a string, a POCO, or a CloudBlockBlob.</span></span> <span data-ttu-id="71f54-121">Hello podporované typy jsou popsány v hello odkaz pro každou vazbu.</span><span class="sxs-lookup"><span data-stu-id="71f54-121">hello supported types are documented in hello reference for each binding.</span></span> <span data-ttu-id="71f54-122">Objekt objektů POCO musí mít metody getter a setter definované pro každou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="71f54-122">A POCO object must have a getter and setter defined for each property.</span></span>

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="using-method-return-value-for-output-binding"></a><span data-ttu-id="71f54-123">Pomocí návratovou hodnotu metoda pro vazbu výstup</span><span class="sxs-lookup"><span data-stu-id="71f54-123">Using method return value for output binding</span></span>

<span data-ttu-id="71f54-124">Můžete použít návratovou hodnotu metoda pro vazbu výstup pomocí názvu hello `$return` v *function.json*:</span><span class="sxs-lookup"><span data-stu-id="71f54-124">You can use a method return value for an output binding, by using hello name `$return` in *function.json*:</span></span>

```json
{
    "type": "queue",
    "direction": "out",
    "name": "$return",
    "queueName": "outqueue",
    "connection": "MyStorageConnectionString",
}
```

```csharp
public static string Run(string input, TraceWriter log)
{
    return input;
}
```

## <a name="writing-multiple-output-values"></a><span data-ttu-id="71f54-125">Zápis více hodnot výstup</span><span class="sxs-lookup"><span data-stu-id="71f54-125">Writing multiple output values</span></span>

<span data-ttu-id="71f54-126">toowrite více hodnot tooan výstup vazbu, použijte hello [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) nebo [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) typy.</span><span class="sxs-lookup"><span data-stu-id="71f54-126">toowrite multiple values tooan output binding, use hello [`ICollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) or [`IAsyncCollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) types.</span></span> <span data-ttu-id="71f54-127">Tyto typy jsou jen pro zápis kolekce, které jsou napsané toohello výstup vazby po dokončení metoda hello.</span><span class="sxs-lookup"><span data-stu-id="71f54-127">These types are write-only collections that are written toohello output binding when hello method completes.</span></span>

<span data-ttu-id="71f54-128">Tento příklad zapíše více fronty zpráv pomocí `ICollector`:</span><span class="sxs-lookup"><span data-stu-id="71f54-128">This example writes multiple queue messages using `ICollector`:</span></span>

```csharp
public static void Run(ICollector<string> myQueueItem, TraceWriter log)
{
    myQueueItem.Add("Hello");
    myQueueItem.Add("World!");
}
```

## <a name="logging"></a><span data-ttu-id="71f54-129">Protokolování</span><span class="sxs-lookup"><span data-stu-id="71f54-129">Logging</span></span>
<span data-ttu-id="71f54-130">výstupní datový proud protokolů tooyour v jazyce C# toolog, zahrnout argument typu `TraceWriter`.</span><span class="sxs-lookup"><span data-stu-id="71f54-130">toolog output tooyour streaming logs in C#, include an argument of type `TraceWriter`.</span></span> <span data-ttu-id="71f54-131">Doporučujeme, abyste pojmenujte ji `log`.</span><span class="sxs-lookup"><span data-stu-id="71f54-131">We recommend that you name it `log`.</span></span> <span data-ttu-id="71f54-132">Nepoužívejte `Console.Write` v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="71f54-132">Avoid using `Console.Write` in Azure Functions.</span></span> 

<span data-ttu-id="71f54-133">`TraceWriter`je definována v hello [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs).</span><span class="sxs-lookup"><span data-stu-id="71f54-133">`TraceWriter` is defined in hello [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs).</span></span> <span data-ttu-id="71f54-134">Hello úroveň protokolu pro `TraceWriter` se dá nakonfigurovat v [hostitele\.json].</span><span class="sxs-lookup"><span data-stu-id="71f54-134">hello log level for `TraceWriter` can be configured in [host\.json].</span></span>

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a><span data-ttu-id="71f54-135">Asynchronní</span><span class="sxs-lookup"><span data-stu-id="71f54-135">Async</span></span>
<span data-ttu-id="71f54-136">toomake funkci asynchronní, použijte hello `async` – klíčové slovo a vraťte se `Task` objektu.</span><span class="sxs-lookup"><span data-stu-id="71f54-136">toomake a function asynchronous, use hello `async` keyword and return a `Task` object.</span></span>

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName,
        Stream blobInput,
        Stream blobOutput)
{
    await blobInput.CopyToAsync(blobOutput, 4096, token);
}
```

## <a name="cancellation-token"></a><span data-ttu-id="71f54-137">Token zrušení</span><span class="sxs-lookup"><span data-stu-id="71f54-137">Cancellation Token</span></span>
<span data-ttu-id="71f54-138">Některé operace vyžadují řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="71f54-138">Some operations require graceful shutdown.</span></span> <span data-ttu-id="71f54-139">I když je vždy nejlepší toowrite kód, který dokáže zpracovat chybám v případech, kde chcete toohandle řádné vypnutí požadavků, můžete definovat [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) zadali argument.</span><span class="sxs-lookup"><span data-stu-id="71f54-139">While it's always best toowrite code that can handle crashing,  in cases where you want toohandle graceful shutdown requests, you define a [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) typed argument.</span></span>  <span data-ttu-id="71f54-140">A `CancellationToken` je k dispozici toosignal, že se aktivuje vypnutí hostitele.</span><span class="sxs-lookup"><span data-stu-id="71f54-140">A `CancellationToken` is provided toosignal that a host shutdown is triggered.</span></span>

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName,
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a><span data-ttu-id="71f54-141">Import obory názvů</span><span class="sxs-lookup"><span data-stu-id="71f54-141">Importing namespaces</span></span>
<span data-ttu-id="71f54-142">Pokud potřebujete tooimport obory názvů, můžete tak učinit běžným způsobem s hello `using` klauzule.</span><span class="sxs-lookup"><span data-stu-id="71f54-142">If you need tooimport namespaces, you can do so as usual, with hello `using` clause.</span></span>

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

<span data-ttu-id="71f54-143">Hello následující obory názvů jsou automaticky importuje a jsou proto volitelné:</span><span class="sxs-lookup"><span data-stu-id="71f54-143">hello following namespaces are automatically imported and are therefore optional:</span></span>

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`

## <a name="referencing-external-assemblies"></a><span data-ttu-id="71f54-144">Odkazování na externí sestavení</span><span class="sxs-lookup"><span data-stu-id="71f54-144">Referencing External Assemblies</span></span>
<span data-ttu-id="71f54-145">Pro sestavení architektury, přidejte odkazy na pomocí hello `#r "AssemblyName"` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="71f54-145">For framework assemblies, add references by using hello `#r "AssemblyName"` directive.</span></span>

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

<span data-ttu-id="71f54-146">Hello následující sestavení jsou automaticky přidány hello Azure Functions hostování prostředí:</span><span class="sxs-lookup"><span data-stu-id="71f54-146">hello following assemblies are automatically added by hello Azure Functions hosting environment:</span></span>

* `mscorlib`
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`

<span data-ttu-id="71f54-147">Hello následující sestavení může být odkazován jednoduchý název (například `#r "AssemblyName"`):</span><span class="sxs-lookup"><span data-stu-id="71f54-147">hello following assemblies may be referenced by simple-name (for example, `#r "AssemblyName"`):</span></span>

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNet.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

## <a name="referencing-custom-assemblies"></a><span data-ttu-id="71f54-148">Odkazování na vlastní sestavení</span><span class="sxs-lookup"><span data-stu-id="71f54-148">Referencing custom assemblies</span></span>

<span data-ttu-id="71f54-149">tooreference vlastního sestavení, můžete použít buď *sdílené* sestavení nebo *privátní* sestavení:</span><span class="sxs-lookup"><span data-stu-id="71f54-149">tooreference a custom assembly, you can use either a *shared* assembly or a *private* assembly:</span></span>
- <span data-ttu-id="71f54-150">Sdílená sestavení jsou sdíleny ve všech funkcí v rámci funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="71f54-150">Shared assemblies are shared across all functions within a function app.</span></span> <span data-ttu-id="71f54-151">tooreference vlastního sestavení, nahrání hello sestavení tooyour funkce aplikace, například v `bin` složky v kořenové aplikace hello funkce.</span><span class="sxs-lookup"><span data-stu-id="71f54-151">tooreference a custom assembly, upload hello assembly tooyour function app, such as in a `bin` folder in hello function app root.</span></span> 
- <span data-ttu-id="71f54-152">Soukromá sestavení jsou součástí kontextu danou funkci a podporovat zkušební načtení různých verzí.</span><span class="sxs-lookup"><span data-stu-id="71f54-152">Private assemblies are part of a given function's context, and support side-loading of different versions.</span></span> <span data-ttu-id="71f54-153">Soukromá sestavení musí být nahrán v `bin` složky v adresáři funkce hello.</span><span class="sxs-lookup"><span data-stu-id="71f54-153">Private assemblies should be uploaded in a `bin` folder in hello function directory.</span></span> <span data-ttu-id="71f54-154">Referenční dokumentace pomocí hello název souboru, například `#r "MyAssembly.dll"`.</span><span class="sxs-lookup"><span data-stu-id="71f54-154">Reference using hello file name, such as  `#r "MyAssembly.dll"`.</span></span> 

<span data-ttu-id="71f54-155">Informace o způsobu tooupload tooyour funkce složku souborů najdete v části hello následující části na správy balíčků.</span><span class="sxs-lookup"><span data-stu-id="71f54-155">For information on how tooupload files tooyour function folder, see hello following section on package management.</span></span>

### <a name="watched-directories"></a><span data-ttu-id="71f54-156">Sledované adresáře</span><span class="sxs-lookup"><span data-stu-id="71f54-156">Watched directories</span></span>

<span data-ttu-id="71f54-157">Hello adresář, který obsahuje soubor skriptu hello funkce je automaticky sledovaných pro tooassemblies změny.</span><span class="sxs-lookup"><span data-stu-id="71f54-157">hello directory that contains hello function script file is automatically watched for changes tooassemblies.</span></span> <span data-ttu-id="71f54-158">je toowatch sestavení změny v ostatních adresářů, přidejte toohello `watchDirectories` seznamu v [hostitele\.json].</span><span class="sxs-lookup"><span data-stu-id="71f54-158">toowatch for assembly changes in other directories, add them toohello `watchDirectories` list in [host\.json].</span></span>

## <a name="using-nuget-packages"></a><span data-ttu-id="71f54-159">Pomocí balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="71f54-159">Using NuGet packages</span></span>
<span data-ttu-id="71f54-160">Nahrát toouse balíčků NuGet v C# funkce, *project.json* toohello funkce složka soubory v aplikaci funkce hello systému souborů.</span><span class="sxs-lookup"><span data-stu-id="71f54-160">toouse NuGet packages in a C# function, upload a *project.json* file toohello function's folder in hello function app's file system.</span></span> <span data-ttu-id="71f54-161">Tady je příklad *project.json* soubor, který přidá odkaz tooMicrosoft.ProjectOxford.Face verze 1.1.0:</span><span class="sxs-lookup"><span data-stu-id="71f54-161">Here is an example *project.json* file that adds a reference tooMicrosoft.ProjectOxford.Face version 1.1.0:</span></span>

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

<span data-ttu-id="71f54-162">Hello rozhraní .NET Framework 4.6 je podporována pouze, tak zkontrolujte, zda vaše *project.json* soubor Určuje `net46` jak je vidět tady.</span><span class="sxs-lookup"><span data-stu-id="71f54-162">Only hello .NET Framework 4.6 is supported, so make sure that your *project.json* file specifies `net46` as shown here.</span></span>

<span data-ttu-id="71f54-163">Když nahrajete *project.json* souboru, hello runtime získá hello balíčky a automaticky přidá reference toohello balíček sestavení.</span><span class="sxs-lookup"><span data-stu-id="71f54-163">When you upload a *project.json* file, hello runtime gets hello packages and automatically adds references toohello package assemblies.</span></span> <span data-ttu-id="71f54-164">Nepotřebujete tooadd `#r "AssemblyName"` direktivy.</span><span class="sxs-lookup"><span data-stu-id="71f54-164">You don't need tooadd `#r "AssemblyName"` directives.</span></span> <span data-ttu-id="71f54-165">toouse hello typy definované v hello balíčky NuGet, přidejte požadované hello `using` příkazy tooyour *run.csx* souboru</span><span class="sxs-lookup"><span data-stu-id="71f54-165">toouse hello types defined in hello NuGet packages, add hello required `using` statements tooyour *run.csx* file</span></span> 

<span data-ttu-id="71f54-166">V modulu runtime hello funkce obnovení NuGet funguje tak, že porovnáte `project.json` a `project.lock.json`.</span><span class="sxs-lookup"><span data-stu-id="71f54-166">In hello Functions runtime, NuGet restore works by comparing `project.json` and `project.lock.json`.</span></span> <span data-ttu-id="71f54-167">Pokud hello data a časových razítek souborů hello **nepodporují** běží shodu, obnovení NuGet a aktualizovat balíčky NuGet stahování.</span><span class="sxs-lookup"><span data-stu-id="71f54-167">If hello date and time stamps of hello files **do not** match, a NuGet restore runs and NuGet downloads updated packages.</span></span> <span data-ttu-id="71f54-168">Nicméně, pokud hello data a časových razítek souborů hello **provést** shodu, NuGet nebude provádět obnovení.</span><span class="sxs-lookup"><span data-stu-id="71f54-168">However, if hello date and time stamps of hello files **do** match, NuGet does not perform a restore.</span></span> <span data-ttu-id="71f54-169">Proto `project.lock.json` by neměl být nasadit jako způsobí, že obnovení balíčků NuGet tooskip.</span><span class="sxs-lookup"><span data-stu-id="71f54-169">Therefore, `project.lock.json` should not be deployed as it causes NuGet tooskip package restore.</span></span> <span data-ttu-id="71f54-170">tooavoid nasazení hello zámek souboru, přidejte hello `project.lock.json` toohello `.gitignore` souboru.</span><span class="sxs-lookup"><span data-stu-id="71f54-170">tooavoid deploying hello lock file, add hello `project.lock.json` toohello `.gitignore` file.</span></span>

<span data-ttu-id="71f54-171">toouse vlastní informační kanál NuGet, zadejte hello kanálu v *Nuget.Config* soubor v kořenovém funkce aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="71f54-171">toouse a custom NuGet feed, specify hello feed in a *Nuget.Config* file in hello Function App root.</span></span> <span data-ttu-id="71f54-172">Další informace najdete v tématu [konfigurace NuGet chování](/nuget/consume-packages/configuring-nuget-behavior).</span><span class="sxs-lookup"><span data-stu-id="71f54-172">For more information, see [Configuring NuGet behavior](/nuget/consume-packages/configuring-nuget-behavior).</span></span>

### <a name="using-a-projectjson-file"></a><span data-ttu-id="71f54-173">Pomocí souboru project.json.</span><span class="sxs-lookup"><span data-stu-id="71f54-173">Using a project.json file</span></span>
1. <span data-ttu-id="71f54-174">Otevřete hello funkce v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="71f54-174">Open hello function in hello Azure portal.</span></span> <span data-ttu-id="71f54-175">Hello zaznamená karta zobrazí hello balíček instalace výstup.</span><span class="sxs-lookup"><span data-stu-id="71f54-175">hello logs tab displays hello package installation output.</span></span>
2. <span data-ttu-id="71f54-176">tooupload soubor project.json, použijte jednu z metod hello popsaných v hello [jak tooupdate funkce soubory aplikace](functions-reference.md#fileupdate) v tématu referenční vývojáře Azure Functions hello.</span><span class="sxs-lookup"><span data-stu-id="71f54-176">tooupload a project.json file, use one of hello methods described in hello [How tooupdate function app files](functions-reference.md#fileupdate) in hello Azure Functions developer reference topic.</span></span>
3. <span data-ttu-id="71f54-177">Po hello *project.json* soubor odešle, uvidíte výstup jako následující příklad ve vaší funkci hello je streamování protokolu:</span><span class="sxs-lookup"><span data-stu-id="71f54-177">After hello *project.json* file is uploaded, you see output like hello following example in your function's streaming log:</span></span>

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

## <a name="environment-variables"></a><span data-ttu-id="71f54-178">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="71f54-178">Environment variables</span></span>
<span data-ttu-id="71f54-179">tooget proměnné prostředí nebo hodnotu nastavení aplikace, použijte `System.Environment.GetEnvironmentVariable`, jak ukazuje následující příklad kódu hello:</span><span class="sxs-lookup"><span data-stu-id="71f54-179">tooget an environment variable or an app setting value, use `System.Environment.GetEnvironmentVariable`, as shown in hello following code example:</span></span>

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " +
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a><span data-ttu-id="71f54-180">Opětovné použití kódu .csx</span><span class="sxs-lookup"><span data-stu-id="71f54-180">Reusing .csx code</span></span>
<span data-ttu-id="71f54-181">Můžete použít třídy a metody definované v jiných *.csx* soubory ve vaší *run.csx* souboru.</span><span class="sxs-lookup"><span data-stu-id="71f54-181">You can use classes and methods defined in other *.csx* files in your *run.csx* file.</span></span> <span data-ttu-id="71f54-182">toodo, který použít `#load` direktivy ve vaší *run.csx* souboru.</span><span class="sxs-lookup"><span data-stu-id="71f54-182">toodo that, use `#load` directives in your *run.csx* file.</span></span> <span data-ttu-id="71f54-183">V následujícím příkladu hello, s názvem rutiny protokolování `MyLogger` je sdílena v *myLogger.csx* a načtena do *run.csx* pomocí hello `#load` – direktiva:</span><span class="sxs-lookup"><span data-stu-id="71f54-183">In hello following example, a logging routine named `MyLogger` is shared in *myLogger.csx* and loaded into *run.csx* using hello `#load` directive:</span></span>

<span data-ttu-id="71f54-184">Příklad *run.csx*:</span><span class="sxs-lookup"><span data-stu-id="71f54-184">Example *run.csx*:</span></span>

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}");
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

<span data-ttu-id="71f54-185">Příklad *mylogger.csx*:</span><span class="sxs-lookup"><span data-stu-id="71f54-185">Example *mylogger.csx*:</span></span>

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext);
}
```

<span data-ttu-id="71f54-186">Pomocí sdílenou *.csx* je běžný vzor, když chcete, aby toostrongly vaše argumenty mezi funkce pomocí objektu objektů POCO typů.</span><span class="sxs-lookup"><span data-stu-id="71f54-186">Using a shared *.csx* is a common pattern when you want toostrongly type your arguments between functions using a POCO object.</span></span> <span data-ttu-id="71f54-187">V následující ukázka zjednodušené hello, triggeru protokolu HTTP a aktivační události fronty sdílet objektů POCO objekt s názvem `Order` toostrongly typ hello pořadí dat:</span><span class="sxs-lookup"><span data-stu-id="71f54-187">In hello following simplified example, an HTTP trigger and queue trigger share a POCO object named `Order` toostrongly type hello order data:</span></span>

<span data-ttu-id="71f54-188">Příklad *run.csx* pro triggeru protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="71f54-188">Example *run.csx* for HTTP trigger:</span></span>

```cs
#load "..\shared\order.csx"

using System.Net;

public static async Task<HttpResponseMessage> Run(Order req, IAsyncCollector<Order> outputQueueItem, TraceWriter log)
{
    log.Info("C# HTTP trigger function received an order.");
    log.Info(req.ToString());
    log.Info("Submitting tooprocessing queue.");

    if (req.orderId == null)
    {
        return new HttpResponseMessage(HttpStatusCode.BadRequest);
    }
    else
    {
        await outputQueueItem.AddAsync(req);
        return new HttpResponseMessage(HttpStatusCode.OK);
    }
}
```

<span data-ttu-id="71f54-189">Příklad *run.csx* frontě aktivační události:</span><span class="sxs-lookup"><span data-stu-id="71f54-189">Example *run.csx* for queue trigger:</span></span>

```cs
#load "..\shared\order.csx"

using System;

public static void Run(Order myQueueItem, out Order outputQueueItem,TraceWriter log)
{
    log.Info($"C# Queue trigger function processed order...");
    log.Info(myQueueItem.ToString());

    outputQueueItem = myQueueItem;
}
```

<span data-ttu-id="71f54-190">Příklad *order.csx*:</span><span class="sxs-lookup"><span data-stu-id="71f54-190">Example *order.csx*:</span></span>

```cs
public class Order
{
    public string orderId {get; set; }
    public string custName {get; set;}
    public string custAddress {get; set;}
    public string custEmail {get; set;}
    public string cartId {get; set; }

    public override String ToString()
    {
        return "\n{\n\torderId : " + orderId +
                  "\n\tcustName : " + custName +             
                  "\n\tcustAddress : " + custAddress +             
                  "\n\tcustEmail : " + custEmail +             
                  "\n\tcartId : " + cartId + "\n}";             
    }
}
```

<span data-ttu-id="71f54-191">Můžete použít relativní cesty s hello `#load` – direktiva:</span><span class="sxs-lookup"><span data-stu-id="71f54-191">You can use a relative path with hello `#load` directive:</span></span>

* <span data-ttu-id="71f54-192">`#load "mylogger.csx"`načte soubor umístěný ve složce funkce hello.</span><span class="sxs-lookup"><span data-stu-id="71f54-192">`#load "mylogger.csx"` loads a file located in hello function folder.</span></span>
* <span data-ttu-id="71f54-193">`#load "loadedfiles\mylogger.csx"`načte soubor umístěný ve složce ve složce funkce hello.</span><span class="sxs-lookup"><span data-stu-id="71f54-193">`#load "loadedfiles\mylogger.csx"` loads a file located in a folder in hello function folder.</span></span>
* <span data-ttu-id="71f54-194">`#load "..\shared\mylogger.csx"`načte soubor umístěný ve složce na stejné úrovni jako složka funkce hello, tedy hello přímo pod *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="71f54-194">`#load "..\shared\mylogger.csx"` loads a file located in a folder at hello same level as hello function folder, that is, directly under *wwwroot*.</span></span>

<span data-ttu-id="71f54-195">Hello `#load` – direktiva pracuje pouze s *.csx* soubory (C# skript), ne při *.cs* soubory.</span><span class="sxs-lookup"><span data-stu-id="71f54-195">hello `#load` directive works only with *.csx* (C# script) files, not with *.cs* files.</span></span>

<a name="imperative-bindings"></a> 

## <a name="binding-at-runtime-via-imperative-bindings"></a><span data-ttu-id="71f54-196">Vazba za běhu prostřednictvím imperativní vazby</span><span class="sxs-lookup"><span data-stu-id="71f54-196">Binding at runtime via imperative bindings</span></span>

<span data-ttu-id="71f54-197">V jazyce C# a jinými jazyky rozhraní .NET, můžete použít [imperativní](https://en.wikipedia.org/wiki/Imperative_programming) vazby vzor jako názvem na rozdíl od toohello [ *deklarativní* ](https://en.wikipedia.org/wiki/Declarative_programming) vazeb v *function.json*.</span><span class="sxs-lookup"><span data-stu-id="71f54-197">In C# and other .NET languages, you can use an [imperative](https://en.wikipedia.org/wiki/Imperative_programming) binding pattern, as opposed toohello [*declarative*](https://en.wikipedia.org/wiki/Declarative_programming) bindings in *function.json*.</span></span> <span data-ttu-id="71f54-198">Imperativní vazba je užitečné, když vázané parametry potřebovat toobe počítaný v době běhu spíše než návrhu.</span><span class="sxs-lookup"><span data-stu-id="71f54-198">Imperative binding is useful when binding parameters need toobe computed at runtime rather than design time.</span></span> <span data-ttu-id="71f54-199">S tento vzor můžete vytvořit vazbu toosupported vstup a výstup vazbu na průběžně ve vašem kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="71f54-199">With this pattern, you can bind toosupported input and output binding on-the-fly in your function code.</span></span>

<span data-ttu-id="71f54-200">Definujte imperativní vazby následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="71f54-200">Define an imperative binding as follows:</span></span>

- <span data-ttu-id="71f54-201">**Nechcete** zahrnují položku v *function.json* pro vaše požadované imperativní vazby.</span><span class="sxs-lookup"><span data-stu-id="71f54-201">**Do not** include an entry in *function.json* for your desired imperative bindings.</span></span>
- <span data-ttu-id="71f54-202">Předejte jí vstupní parametr [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) nebo [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).</span><span class="sxs-lookup"><span data-stu-id="71f54-202">Pass in an input parameter [`Binder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) or [`IBinder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).</span></span>
- <span data-ttu-id="71f54-203">Použijte následující C# vzor tooperform hello datová vazba hello.</span><span class="sxs-lookup"><span data-stu-id="71f54-203">Use hello following C# pattern tooperform hello data binding.</span></span>

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

<span data-ttu-id="71f54-204">kde `BindingTypeAttribute` je hello .NET atribut, který definuje váš vazby a `T` je hello vstupních nebo výstupních typ, který podporuje tento typ vazby.</span><span class="sxs-lookup"><span data-stu-id="71f54-204">where `BindingTypeAttribute` is hello .NET attribute that defines your binding and `T` is hello input or output type that's supported by that binding type.</span></span> <span data-ttu-id="71f54-205">`T`také nelze `out` typ parametru (například `out JObject`).</span><span class="sxs-lookup"><span data-stu-id="71f54-205">`T` also cannot be an `out` parameter type (such as `out JObject`).</span></span> <span data-ttu-id="71f54-206">Například v tabulce Mobile Apps výstupu vazba podporuje [šest výstup typy](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), ale můžete použít pouze [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) nebo [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs)pro `T`.</span><span class="sxs-lookup"><span data-stu-id="71f54-206">For example, the Mobile Apps table output binding supports [six output types](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), but you can only use [ICollector<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) or [IAsyncCollector<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) for `T`.</span></span>

<span data-ttu-id="71f54-207">Hello následující příklad kódu vytvoří [objektu blob Storage výstup vazby](functions-bindings-storage-blob.md#using-a-blob-output-binding) s objektem blob cestu, která je definována v době běhu, pak zapíše objekt blob toohello řetězec.</span><span class="sxs-lookup"><span data-stu-id="71f54-207">hello following example code creates a [Storage blob output binding](functions-bindings-storage-blob.md#using-a-blob-output-binding) with blob path that's defined at run time, then writes a string toohello blob.</span></span>

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    using (var writer = await binder.BindAsync<TextWriter>(new BlobAttribute("samples-output/path")))
    {
        writer.Write("Hello World!!");
    }
}
```

<span data-ttu-id="71f54-208">[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) definuje hello [objektu blob Storage](functions-bindings-storage-blob.md) vstupem nebo výstupem vazby a [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) je typ vazby podporované výstup.</span><span class="sxs-lookup"><span data-stu-id="71f54-208">[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) defines hello [Storage blob](functions-bindings-storage-blob.md) input or output binding, and [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) is a supported output binding type.</span></span>
<span data-ttu-id="71f54-209">Hello kód získá hello výchozí nastavení aplikace hello připojovací řetězce k účtu úložiště (což je `AzureWebJobsStorage`).</span><span class="sxs-lookup"><span data-stu-id="71f54-209">As is, hello code gets hello default app setting for hello Storage account connection string (which is `AzureWebJobsStorage`).</span></span> <span data-ttu-id="71f54-210">Můžete zadat nastavení toouse vlastní aplikaci přidáním [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) a předání hello atribut pole do `BindAsync<T>()`.</span><span class="sxs-lookup"><span data-stu-id="71f54-210">You can specify a custom app setting toouse by adding the [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) and passing hello attribute array into `BindAsync<T>()`.</span></span> <span data-ttu-id="71f54-211">Například:</span><span class="sxs-lookup"><span data-stu-id="71f54-211">For example,</span></span>

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    var attributes = new Attribute[]
    {    
        new BlobAttribute("samples-output/path"),
        new StorageAccountAttribute("MyStorageAccount")
    };

    using (var writer = await binder.BindAsync<TextWriter>(attributes))
    {
        writer.Write("Hello World!");
    }
}
```

<span data-ttu-id="71f54-212">Hello následující tabulka uvádí hello .NET atributy pro každou vazbu typu hello balíčků a ve kterých jsou definovány.</span><span class="sxs-lookup"><span data-stu-id="71f54-212">hello following table lists hello .NET attributes for each binding type and hello packages in which they are defined.</span></span>

> [!div class="mx-codeBreakAll"]
| <span data-ttu-id="71f54-213">Vazba</span><span class="sxs-lookup"><span data-stu-id="71f54-213">Binding</span></span> | <span data-ttu-id="71f54-214">Atribut</span><span class="sxs-lookup"><span data-stu-id="71f54-214">Attribute</span></span> | <span data-ttu-id="71f54-215">Přidání odkazu</span><span class="sxs-lookup"><span data-stu-id="71f54-215">Add reference</span></span> |
|------|------|------|
| <span data-ttu-id="71f54-216">Databáze Cosmos</span><span class="sxs-lookup"><span data-stu-id="71f54-216">Cosmos DB</span></span> | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.DocumentDB"` |
| <span data-ttu-id="71f54-217">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="71f54-217">Event Hubs</span></span> | <span data-ttu-id="71f54-218">[`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="71f54-218">[`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span></span> | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
| <span data-ttu-id="71f54-219">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="71f54-219">Mobile Apps</span></span> | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
| <span data-ttu-id="71f54-220">Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="71f54-220">Notification Hubs</span></span> | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
| <span data-ttu-id="71f54-221">Service Bus</span><span class="sxs-lookup"><span data-stu-id="71f54-221">Service Bus</span></span> | <span data-ttu-id="71f54-222">[`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="71f54-222">[`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span></span> | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
| <span data-ttu-id="71f54-223">Fronty úložiště</span><span class="sxs-lookup"><span data-stu-id="71f54-223">Storage queue</span></span> | <span data-ttu-id="71f54-224">[`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="71f54-224">[`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span></span> | |
| <span data-ttu-id="71f54-225">Objekt blob úložiště</span><span class="sxs-lookup"><span data-stu-id="71f54-225">Storage blob</span></span> | <span data-ttu-id="71f54-226">[`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="71f54-226">[`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span></span> | |
| <span data-ttu-id="71f54-227">Tabulka úložiště</span><span class="sxs-lookup"><span data-stu-id="71f54-227">Storage table</span></span> | <span data-ttu-id="71f54-228">[`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="71f54-228">[`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span></span> | |
| <span data-ttu-id="71f54-229">Twilio</span><span class="sxs-lookup"><span data-stu-id="71f54-229">Twilio</span></span> | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |



## <a name="next-steps"></a><span data-ttu-id="71f54-230">Další kroky</span><span class="sxs-lookup"><span data-stu-id="71f54-230">Next steps</span></span>
<span data-ttu-id="71f54-231">Další informace najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="71f54-231">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="71f54-232">Osvědčené postupy pro službu Azure Functions</span><span class="sxs-lookup"><span data-stu-id="71f54-232">Best Practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="71f54-233">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="71f54-233">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="71f54-234">Azure funkce F # referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="71f54-234">Azure Functions F# developer reference</span></span>](functions-reference-fsharp.md)
* [<span data-ttu-id="71f54-235">Referenční informace pro vývojáře Azure funkce NodeJS</span><span class="sxs-lookup"><span data-stu-id="71f54-235">Azure Functions NodeJS developer reference</span></span>](functions-reference-node.md)
* [<span data-ttu-id="71f54-236">Azure funkce triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="71f54-236">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)

[hostitele\.json]: https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json
