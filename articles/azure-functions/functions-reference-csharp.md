---
title: "Referenční informace pro vývojáře Azure funkcí jazyka C# skript | Microsoft Docs"
description: "Pochopit, jak vyvíjet Azure Functions pomocí jazyka C#."
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
ms.openlocfilehash: 83a351ce0279ada8ce7fe0513497349471334a86
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-c-script-developer-reference"></a><span data-ttu-id="53d43-104">Azure funkcí jazyka C# skript referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="53d43-104">Azure Functions C# script developer reference</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="53d43-105">Skript jazyka C#</span><span class="sxs-lookup"><span data-stu-id="53d43-105">C# script</span></span>](functions-reference-csharp.md)
> * [<span data-ttu-id="53d43-106">F # skript</span><span class="sxs-lookup"><span data-stu-id="53d43-106">F# script</span></span>](functions-reference-fsharp.md)
> * [<span data-ttu-id="53d43-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="53d43-107">Node.js</span></span>](functions-reference-node.md)
>
>

<span data-ttu-id="53d43-108">Prostředí skriptu jazyka C# pro Azure Functions je založena na Azure WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="53d43-108">The C# script experience for Azure Functions is based on the Azure WebJobs SDK.</span></span> <span data-ttu-id="53d43-109">Toky dat do funkce jazyka C# prostřednictvím metoda argumenty.</span><span class="sxs-lookup"><span data-stu-id="53d43-109">Data flows into your C# function via method arguments.</span></span> <span data-ttu-id="53d43-110">Argument názvy jsou určené v `function.json`, a jsou předdefinované názvy pro přístup k takové věci, jako funkce protokolovacího nástroje a zrušení tokenů.</span><span class="sxs-lookup"><span data-stu-id="53d43-110">Argument names are specified in `function.json`, and there are predefined names for accessing things like the function logger and cancellation tokens.</span></span>

<span data-ttu-id="53d43-111">Tento článek předpokládá, že jste si již přečetli [referenční informace pro vývojáře Azure Functions](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="53d43-111">This article assumes that you've already read the [Azure Functions developer reference](functions-reference.md).</span></span>

<span data-ttu-id="53d43-112">Informace o použití jazyka C# třídy knihovny najdete v tématu [knihovny tříd pomocí rozhraní .NET s Azure Functions](functions-dotnet-class-library.md).</span><span class="sxs-lookup"><span data-stu-id="53d43-112">For information on using C# class libraries, see [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md).</span></span>

## <a name="how-csx-works"></a><span data-ttu-id="53d43-113">Jak funguje .csx</span><span class="sxs-lookup"><span data-stu-id="53d43-113">How .csx works</span></span>
<span data-ttu-id="53d43-114">`.csx` Formátu umožňuje zapsat menší "standardní" a zaměřit se na psaní právě C# funkce.</span><span class="sxs-lookup"><span data-stu-id="53d43-114">The `.csx` format allows you to write less "boilerplate" and focus on writing just a C# function.</span></span> <span data-ttu-id="53d43-115">Zahrnout všechny odkazy na sestavení a obory názvů na začátku souboru jako obvykle.</span><span class="sxs-lookup"><span data-stu-id="53d43-115">Include any assembly references and namespaces at the beginning of the file as usual.</span></span> <span data-ttu-id="53d43-116">Místo zabalení všechno, co v oboru názvů a třídy, pouze definuje `Run` metoda.</span><span class="sxs-lookup"><span data-stu-id="53d43-116">Instead of wrapping everything in a namespace and class, just define a `Run` method.</span></span> <span data-ttu-id="53d43-117">Pokud je nutné zahrnout všechny třídy pro instanci Pokud chcete definovat objekty prostý staré CLR objektů POCO (), můžete zahrnout třídu ve stejném souboru.</span><span class="sxs-lookup"><span data-stu-id="53d43-117">If you need to include any classes, for instance to define Plain Old CLR Object (POCO) objects, you can include a class inside the same file.</span></span>   

## <a name="binding-to-arguments"></a><span data-ttu-id="53d43-118">Vytvoření vazby na argumenty</span><span class="sxs-lookup"><span data-stu-id="53d43-118">Binding to arguments</span></span>
<span data-ttu-id="53d43-119">Různé vazby je vázána na funkce jazyka C# pomocí `name` vlastnost *function.json* konfigurace.</span><span class="sxs-lookup"><span data-stu-id="53d43-119">The various bindings are bound to a C# function via the `name` property in the *function.json* configuration.</span></span> <span data-ttu-id="53d43-120">Každé vazby má svou vlastní podporované typy; řetězec, objektů POCO nebo CloudBlockBlob může například podporovat aktivační události objektu blob.</span><span class="sxs-lookup"><span data-stu-id="53d43-120">Each binding has its own supported types; for instance, a blob trigger can support a string, a POCO, or a CloudBlockBlob.</span></span> <span data-ttu-id="53d43-121">Podporované typy jsou popsané v dokumentaci pro každou vazbu.</span><span class="sxs-lookup"><span data-stu-id="53d43-121">The supported types are documented in the reference for each binding.</span></span> <span data-ttu-id="53d43-122">Objekt objektů POCO musí mít metody getter a setter definované pro každou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="53d43-122">A POCO object must have a getter and setter defined for each property.</span></span>

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

## <a name="using-method-return-value-for-output-binding"></a><span data-ttu-id="53d43-123">Pomocí návratovou hodnotu metoda pro vazbu výstup</span><span class="sxs-lookup"><span data-stu-id="53d43-123">Using method return value for output binding</span></span>

<span data-ttu-id="53d43-124">Můžete použít návratovou hodnotu metoda pro vazbu výstup pomocí názvu `$return` v *function.json*:</span><span class="sxs-lookup"><span data-stu-id="53d43-124">You can use a method return value for an output binding, by using the name `$return` in *function.json*:</span></span>

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

## <a name="writing-multiple-output-values"></a><span data-ttu-id="53d43-125">Zápis více hodnot výstup</span><span class="sxs-lookup"><span data-stu-id="53d43-125">Writing multiple output values</span></span>

<span data-ttu-id="53d43-126">K vytvoření více hodnot pro vazbu výstup, použijte [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) nebo [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) typy.</span><span class="sxs-lookup"><span data-stu-id="53d43-126">To write multiple values to an output binding, use the [`ICollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) or [`IAsyncCollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) types.</span></span> <span data-ttu-id="53d43-127">Tyto typy jsou jen pro zápis kolekce, které se zapisují do výstupu vazby po dokončení metody.</span><span class="sxs-lookup"><span data-stu-id="53d43-127">These types are write-only collections that are written to the output binding when the method completes.</span></span>

<span data-ttu-id="53d43-128">Tento příklad zapíše více fronty zpráv pomocí `ICollector`:</span><span class="sxs-lookup"><span data-stu-id="53d43-128">This example writes multiple queue messages using `ICollector`:</span></span>

```csharp
public static void Run(ICollector<string> myQueueItem, TraceWriter log)
{
    myQueueItem.Add("Hello");
    myQueueItem.Add("World!");
}
```

## <a name="logging"></a><span data-ttu-id="53d43-129">Protokolování</span><span class="sxs-lookup"><span data-stu-id="53d43-129">Logging</span></span>
<span data-ttu-id="53d43-130">Do protokolu výstup váš datový proud protokolů v jazyce C#, zahrnují argument typu `TraceWriter`.</span><span class="sxs-lookup"><span data-stu-id="53d43-130">To log output to your streaming logs in C#, include an argument of type `TraceWriter`.</span></span> <span data-ttu-id="53d43-131">Doporučujeme, abyste pojmenujte ji `log`.</span><span class="sxs-lookup"><span data-stu-id="53d43-131">We recommend that you name it `log`.</span></span> <span data-ttu-id="53d43-132">Nepoužívejte `Console.Write` v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="53d43-132">Avoid using `Console.Write` in Azure Functions.</span></span> 

<span data-ttu-id="53d43-133">`TraceWriter`je definována v [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs).</span><span class="sxs-lookup"><span data-stu-id="53d43-133">`TraceWriter` is defined in the [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs).</span></span> <span data-ttu-id="53d43-134">Úroveň protokolu pro `TraceWriter` se dá nakonfigurovat v [hostitele\.json].</span><span class="sxs-lookup"><span data-stu-id="53d43-134">The log level for `TraceWriter` can be configured in [host\.json].</span></span>

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a><span data-ttu-id="53d43-135">Asynchronní</span><span class="sxs-lookup"><span data-stu-id="53d43-135">Async</span></span>
<span data-ttu-id="53d43-136">Chcete-li funkci asynchronní, použijte `async` – klíčové slovo a vraťte se `Task` objektu.</span><span class="sxs-lookup"><span data-stu-id="53d43-136">To make a function asynchronous, use the `async` keyword and return a `Task` object.</span></span>

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName,
        Stream blobInput,
        Stream blobOutput)
{
    await blobInput.CopyToAsync(blobOutput, 4096, token);
}
```

## <a name="cancellation-token"></a><span data-ttu-id="53d43-137">Token zrušení</span><span class="sxs-lookup"><span data-stu-id="53d43-137">Cancellation Token</span></span>
<span data-ttu-id="53d43-138">Některé operace vyžadují řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="53d43-138">Some operations require graceful shutdown.</span></span> <span data-ttu-id="53d43-139">I když je vždy nejlepší napsat kód, který dokáže zpracovat chybám, v případech, kde se má zpracovat řádné vypnutí požadavky, můžete definovat [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) zadali argument.</span><span class="sxs-lookup"><span data-stu-id="53d43-139">While it's always best to write code that can handle crashing,  in cases where you want to handle graceful shutdown requests, you define a [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) typed argument.</span></span>  <span data-ttu-id="53d43-140">A `CancellationToken` je k dispozici signál, že se aktivuje vypnutí hostitele.</span><span class="sxs-lookup"><span data-stu-id="53d43-140">A `CancellationToken` is provided to signal that a host shutdown is triggered.</span></span>

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

## <a name="importing-namespaces"></a><span data-ttu-id="53d43-141">Import obory názvů</span><span class="sxs-lookup"><span data-stu-id="53d43-141">Importing namespaces</span></span>
<span data-ttu-id="53d43-142">Pokud potřebujete importovat obory názvů, můžete to udělat tak jako obvyklé, se `using` klauzule.</span><span class="sxs-lookup"><span data-stu-id="53d43-142">If you need to import namespaces, you can do so as usual, with the `using` clause.</span></span>

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

<span data-ttu-id="53d43-143">Následující obory názvů jsou automaticky importuje a jsou proto volitelné:</span><span class="sxs-lookup"><span data-stu-id="53d43-143">The following namespaces are automatically imported and are therefore optional:</span></span>

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`

## <a name="referencing-external-assemblies"></a><span data-ttu-id="53d43-144">Odkazování na externí sestavení</span><span class="sxs-lookup"><span data-stu-id="53d43-144">Referencing External Assemblies</span></span>
<span data-ttu-id="53d43-145">Pro sestavení architektury, přidání odkazů pomocí `#r "AssemblyName"` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="53d43-145">For framework assemblies, add references by using the `#r "AssemblyName"` directive.</span></span>

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

<span data-ttu-id="53d43-146">Následující sestavení jsou automaticky přidány Azure Functions hostování prostředí:</span><span class="sxs-lookup"><span data-stu-id="53d43-146">The following assemblies are automatically added by the Azure Functions hosting environment:</span></span>

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

<span data-ttu-id="53d43-147">Následující sestavení může být odkazován jednoduchý název (například `#r "AssemblyName"`):</span><span class="sxs-lookup"><span data-stu-id="53d43-147">The following assemblies may be referenced by simple-name (for example, `#r "AssemblyName"`):</span></span>

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNet.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

## <a name="referencing-custom-assemblies"></a><span data-ttu-id="53d43-148">Odkazování na vlastní sestavení</span><span class="sxs-lookup"><span data-stu-id="53d43-148">Referencing custom assemblies</span></span>

<span data-ttu-id="53d43-149">Chcete-li vlastního sestavení, můžete použít buď *sdílené* sestavení nebo *privátní* sestavení:</span><span class="sxs-lookup"><span data-stu-id="53d43-149">To reference a custom assembly, you can use either a *shared* assembly or a *private* assembly:</span></span>
- <span data-ttu-id="53d43-150">Sdílená sestavení jsou sdíleny ve všech funkcí v rámci funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="53d43-150">Shared assemblies are shared across all functions within a function app.</span></span> <span data-ttu-id="53d43-151">Chcete-li vlastního sestavení, nahrajte sestavení do funkce aplikace, například v `bin` složku v kořenu aplikace funkce.</span><span class="sxs-lookup"><span data-stu-id="53d43-151">To reference a custom assembly, upload the assembly to your function app, such as in a `bin` folder in the function app root.</span></span> 
- <span data-ttu-id="53d43-152">Soukromá sestavení jsou součástí kontextu danou funkci a podporovat zkušební načtení různých verzí.</span><span class="sxs-lookup"><span data-stu-id="53d43-152">Private assemblies are part of a given function's context, and support side-loading of different versions.</span></span> <span data-ttu-id="53d43-153">Soukromá sestavení musí být nahrán v `bin` složky v adresáři funkce.</span><span class="sxs-lookup"><span data-stu-id="53d43-153">Private assemblies should be uploaded in a `bin` folder in the function directory.</span></span> <span data-ttu-id="53d43-154">Odkazovat pomocí názvu souboru, jako třeba `#r "MyAssembly.dll"`.</span><span class="sxs-lookup"><span data-stu-id="53d43-154">Reference using the file name, such as  `#r "MyAssembly.dll"`.</span></span> 

<span data-ttu-id="53d43-155">Informace o tom, jak odeslat soubory do složky funkce najdete v následující části na správy balíčků.</span><span class="sxs-lookup"><span data-stu-id="53d43-155">For information on how to upload files to your function folder, see the following section on package management.</span></span>

### <a name="watched-directories"></a><span data-ttu-id="53d43-156">Sledované adresáře</span><span class="sxs-lookup"><span data-stu-id="53d43-156">Watched directories</span></span>

<span data-ttu-id="53d43-157">Adresář, který obsahuje soubor skriptu funkce je automaticky sledovaná změny sestavení.</span><span class="sxs-lookup"><span data-stu-id="53d43-157">The directory that contains the function script file is automatically watched for changes to assemblies.</span></span> <span data-ttu-id="53d43-158">Podívejte se na sestavení změny v ostatních adresářů, přidejte je do `watchDirectories` seznamu v [hostitele\.json].</span><span class="sxs-lookup"><span data-stu-id="53d43-158">To watch for assembly changes in other directories, add them to the `watchDirectories` list in [host\.json].</span></span>

## <a name="using-nuget-packages"></a><span data-ttu-id="53d43-159">Pomocí balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="53d43-159">Using NuGet packages</span></span>
<span data-ttu-id="53d43-160">Použití balíčků NuGet ve funkci jazyka C#, nahrajte *project.json* soubor do složky funkce v systému souborů aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="53d43-160">To use NuGet packages in a C# function, upload a *project.json* file to the function's folder in the function app's file system.</span></span> <span data-ttu-id="53d43-161">Tady je příklad *project.json* soubor, který přidá odkaz na Microsoft.ProjectOxford.Face verze 1.1.0:</span><span class="sxs-lookup"><span data-stu-id="53d43-161">Here is an example *project.json* file that adds a reference to Microsoft.ProjectOxford.Face version 1.1.0:</span></span>

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

<span data-ttu-id="53d43-162">Je podporováno pouze rozhraní .NET Framework 4.6, tak zkontrolujte, zda vaše *project.json* soubor Určuje `net46` jak je vidět tady.</span><span class="sxs-lookup"><span data-stu-id="53d43-162">Only the .NET Framework 4.6 is supported, so make sure that your *project.json* file specifies `net46` as shown here.</span></span>

<span data-ttu-id="53d43-163">Když nahrajete *project.json* soubor modulu runtime získá balíčky a automaticky přidá reference na sestavení balíčku.</span><span class="sxs-lookup"><span data-stu-id="53d43-163">When you upload a *project.json* file, the runtime gets the packages and automatically adds references to the package assemblies.</span></span> <span data-ttu-id="53d43-164">Nemusíte přidávat `#r "AssemblyName"` direktivy.</span><span class="sxs-lookup"><span data-stu-id="53d43-164">You don't need to add `#r "AssemblyName"` directives.</span></span> <span data-ttu-id="53d43-165">Chcete-li používat typy definované v balíčky NuGet, přidejte požadované `using` příkazů do vaší *run.csx* souboru</span><span class="sxs-lookup"><span data-stu-id="53d43-165">To use the types defined in the NuGet packages, add the required `using` statements to your *run.csx* file</span></span> 

<span data-ttu-id="53d43-166">V modulu runtime funkce obnovení NuGet funguje tak, že porovnáte `project.json` a `project.lock.json`.</span><span class="sxs-lookup"><span data-stu-id="53d43-166">In the Functions runtime, NuGet restore works by comparing `project.json` and `project.lock.json`.</span></span> <span data-ttu-id="53d43-167">Pokud data a časových razítek souborů **nepodporují** běží shodu, obnovení NuGet a aktualizovat balíčky NuGet stahování.</span><span class="sxs-lookup"><span data-stu-id="53d43-167">If the date and time stamps of the files **do not** match, a NuGet restore runs and NuGet downloads updated packages.</span></span> <span data-ttu-id="53d43-168">Ale pokud razítka data a času souborů **provést** shodu, NuGet nebude provádět obnovení.</span><span class="sxs-lookup"><span data-stu-id="53d43-168">However, if the date and time stamps of the files **do** match, NuGet does not perform a restore.</span></span> <span data-ttu-id="53d43-169">Proto `project.lock.json` by neměl být nasadit jako způsobí, že chcete přeskočit obnovení balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="53d43-169">Therefore, `project.lock.json` should not be deployed as it causes NuGet to skip package restore.</span></span> <span data-ttu-id="53d43-170">Abyste se vyhnuli nasazení souboru zámku, přidejte `project.lock.json` k `.gitignore` souboru.</span><span class="sxs-lookup"><span data-stu-id="53d43-170">To avoid deploying the lock file, add the `project.lock.json` to the `.gitignore` file.</span></span>

<span data-ttu-id="53d43-171">Pokud chcete používat vlastní NuGet kanálu, zadejte datového kanálu v *Nuget.Config* soubor v kořenu aplikace funkce.</span><span class="sxs-lookup"><span data-stu-id="53d43-171">To use a custom NuGet feed, specify the feed in a *Nuget.Config* file in the Function App root.</span></span> <span data-ttu-id="53d43-172">Další informace najdete v tématu [konfigurace NuGet chování](/nuget/consume-packages/configuring-nuget-behavior).</span><span class="sxs-lookup"><span data-stu-id="53d43-172">For more information, see [Configuring NuGet behavior](/nuget/consume-packages/configuring-nuget-behavior).</span></span>

### <a name="using-a-projectjson-file"></a><span data-ttu-id="53d43-173">Pomocí souboru project.json.</span><span class="sxs-lookup"><span data-stu-id="53d43-173">Using a project.json file</span></span>
1. <span data-ttu-id="53d43-174">Otevřete funkce na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="53d43-174">Open the function in the Azure portal.</span></span> <span data-ttu-id="53d43-175">Na kartě protokoly zobrazí výstup instalace balíčku.</span><span class="sxs-lookup"><span data-stu-id="53d43-175">The logs tab displays the package installation output.</span></span>
2. <span data-ttu-id="53d43-176">Nahrát soubor project.json, použijte jednu z metod popsaných v [jak aktualizovat soubory aplikace funkce](functions-reference.md#fileupdate) v referenčním tématu pro vývojáře Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="53d43-176">To upload a project.json file, use one of the methods described in the [How to update function app files](functions-reference.md#fileupdate) in the Azure Functions developer reference topic.</span></span>
3. <span data-ttu-id="53d43-177">Po *project.json* soubor odešle, uvidíte výstup podobný následujícímu příkladu funkce je streamování protokolu:</span><span class="sxs-lookup"><span data-stu-id="53d43-177">After the *project.json* file is uploaded, you see output like the following example in your function's streaming log:</span></span>

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

## <a name="environment-variables"></a><span data-ttu-id="53d43-178">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="53d43-178">Environment variables</span></span>
<span data-ttu-id="53d43-179">Proměnné prostředí nebo nastavení hodnoty aplikace, použijte `System.Environment.GetEnvironmentVariable`, jak ukazuje následující příklad kódu:</span><span class="sxs-lookup"><span data-stu-id="53d43-179">To get an environment variable or an app setting value, use `System.Environment.GetEnvironmentVariable`, as shown in the following code example:</span></span>

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

## <a name="reusing-csx-code"></a><span data-ttu-id="53d43-180">Opětovné použití kódu .csx</span><span class="sxs-lookup"><span data-stu-id="53d43-180">Reusing .csx code</span></span>
<span data-ttu-id="53d43-181">Můžete použít třídy a metody definované v jiných *.csx* soubory ve vaší *run.csx* souboru.</span><span class="sxs-lookup"><span data-stu-id="53d43-181">You can use classes and methods defined in other *.csx* files in your *run.csx* file.</span></span> <span data-ttu-id="53d43-182">Chcete-li provést, použijte `#load` direktivy ve vaší *run.csx* souboru.</span><span class="sxs-lookup"><span data-stu-id="53d43-182">To do that, use `#load` directives in your *run.csx* file.</span></span> <span data-ttu-id="53d43-183">V následujícím příkladu rutiny protokolování s názvem `MyLogger` je sdílena v *myLogger.csx* a načtena do *run.csx* pomocí `#load` – direktiva:</span><span class="sxs-lookup"><span data-stu-id="53d43-183">In the following example, a logging routine named `MyLogger` is shared in *myLogger.csx* and loaded into *run.csx* using the `#load` directive:</span></span>

<span data-ttu-id="53d43-184">Příklad *run.csx*:</span><span class="sxs-lookup"><span data-stu-id="53d43-184">Example *run.csx*:</span></span>

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}");
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

<span data-ttu-id="53d43-185">Příklad *mylogger.csx*:</span><span class="sxs-lookup"><span data-stu-id="53d43-185">Example *mylogger.csx*:</span></span>

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext);
}
```

<span data-ttu-id="53d43-186">Pomocí sdílenou *.csx* je běžný vzor, když chcete vaší argumentů mezi funkce pomocí objektu objektů POCO silného typu.</span><span class="sxs-lookup"><span data-stu-id="53d43-186">Using a shared *.csx* is a common pattern when you want to strongly type your arguments between functions using a POCO object.</span></span> <span data-ttu-id="53d43-187">V následujícím příkladu zjednodušené služby triggeru protokolu HTTP a aktivační události fronty sdílet objektů POCO objekt s názvem `Order` na typově pořadí dat:</span><span class="sxs-lookup"><span data-stu-id="53d43-187">In the following simplified example, an HTTP trigger and queue trigger share a POCO object named `Order` to strongly type the order data:</span></span>

<span data-ttu-id="53d43-188">Příklad *run.csx* pro triggeru protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="53d43-188">Example *run.csx* for HTTP trigger:</span></span>

```cs
#load "..\shared\order.csx"

using System.Net;

public static async Task<HttpResponseMessage> Run(Order req, IAsyncCollector<Order> outputQueueItem, TraceWriter log)
{
    log.Info("C# HTTP trigger function received an order.");
    log.Info(req.ToString());
    log.Info("Submitting to processing queue.");

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

<span data-ttu-id="53d43-189">Příklad *run.csx* frontě aktivační události:</span><span class="sxs-lookup"><span data-stu-id="53d43-189">Example *run.csx* for queue trigger:</span></span>

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

<span data-ttu-id="53d43-190">Příklad *order.csx*:</span><span class="sxs-lookup"><span data-stu-id="53d43-190">Example *order.csx*:</span></span>

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

<span data-ttu-id="53d43-191">Můžete použít relativní cestu `#load` – direktiva:</span><span class="sxs-lookup"><span data-stu-id="53d43-191">You can use a relative path with the `#load` directive:</span></span>

* <span data-ttu-id="53d43-192">`#load "mylogger.csx"`načte soubor umístěný ve složce funkce.</span><span class="sxs-lookup"><span data-stu-id="53d43-192">`#load "mylogger.csx"` loads a file located in the function folder.</span></span>
* <span data-ttu-id="53d43-193">`#load "loadedfiles\mylogger.csx"`načte soubor umístěný ve složce ve složce funkce.</span><span class="sxs-lookup"><span data-stu-id="53d43-193">`#load "loadedfiles\mylogger.csx"` loads a file located in a folder in the function folder.</span></span>
* <span data-ttu-id="53d43-194">`#load "..\shared\mylogger.csx"`načte soubor umístěný ve složce na stejné úrovni jako složka funkce, který je přímo pod *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="53d43-194">`#load "..\shared\mylogger.csx"` loads a file located in a folder at the same level as the function folder, that is, directly under *wwwroot*.</span></span>

<span data-ttu-id="53d43-195">`#load` – Direktiva pracuje pouze s *.csx* soubory (C# skript), ne při *.cs* soubory.</span><span class="sxs-lookup"><span data-stu-id="53d43-195">The `#load` directive works only with *.csx* (C# script) files, not with *.cs* files.</span></span>

<a name="imperative-bindings"></a> 

## <a name="binding-at-runtime-via-imperative-bindings"></a><span data-ttu-id="53d43-196">Vazba za běhu prostřednictvím imperativní vazby</span><span class="sxs-lookup"><span data-stu-id="53d43-196">Binding at runtime via imperative bindings</span></span>

<span data-ttu-id="53d43-197">V jazyce C# a jinými jazyky rozhraní .NET, můžete použít [imperativní](https://en.wikipedia.org/wiki/Imperative_programming) vazby vzor, nikoli [ *deklarativní* ](https://en.wikipedia.org/wiki/Declarative_programming) vazeb v *function.json*.</span><span class="sxs-lookup"><span data-stu-id="53d43-197">In C# and other .NET languages, you can use an [imperative](https://en.wikipedia.org/wiki/Imperative_programming) binding pattern, as opposed to the [*declarative*](https://en.wikipedia.org/wiki/Declarative_programming) bindings in *function.json*.</span></span> <span data-ttu-id="53d43-198">Imperativní vazba je užitečné, když vázané parametry muset počítaný v době běhu spíše než návrhu.</span><span class="sxs-lookup"><span data-stu-id="53d43-198">Imperative binding is useful when binding parameters need to be computed at runtime rather than design time.</span></span> <span data-ttu-id="53d43-199">S tento vzor můžete vytvořit vazbu na podporované vstup a výstup vazbu na průběžně ve vašem kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="53d43-199">With this pattern, you can bind to supported input and output binding on-the-fly in your function code.</span></span>

<span data-ttu-id="53d43-200">Definujte imperativní vazby následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="53d43-200">Define an imperative binding as follows:</span></span>

- <span data-ttu-id="53d43-201">**Nechcete** zahrnují položku v *function.json* pro vaše požadované imperativní vazby.</span><span class="sxs-lookup"><span data-stu-id="53d43-201">**Do not** include an entry in *function.json* for your desired imperative bindings.</span></span>
- <span data-ttu-id="53d43-202">Předejte jí vstupní parametr [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) nebo [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).</span><span class="sxs-lookup"><span data-stu-id="53d43-202">Pass in an input parameter [`Binder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) or [`IBinder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).</span></span>
- <span data-ttu-id="53d43-203">K provedení vazby dat použijte následující vzor C#.</span><span class="sxs-lookup"><span data-stu-id="53d43-203">Use the following C# pattern to perform the data binding.</span></span>

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

<span data-ttu-id="53d43-204">kde `BindingTypeAttribute` je atribut rozhraní .NET, který definuje váš vazby a `T` je vstupní nebo výstupní typ, který podporuje tento typ vazby.</span><span class="sxs-lookup"><span data-stu-id="53d43-204">where `BindingTypeAttribute` is the .NET attribute that defines your binding and `T` is the input or output type that's supported by that binding type.</span></span> <span data-ttu-id="53d43-205">`T`také nelze `out` typ parametru (například `out JObject`).</span><span class="sxs-lookup"><span data-stu-id="53d43-205">`T` also cannot be an `out` parameter type (such as `out JObject`).</span></span> <span data-ttu-id="53d43-206">Například v tabulce Mobile Apps výstupu vazba podporuje [šest výstup typy](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), ale můžete použít pouze [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) nebo [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs)pro `T`.</span><span class="sxs-lookup"><span data-stu-id="53d43-206">For example, the Mobile Apps table output binding supports [six output types](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), but you can only use [ICollector<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) or [IAsyncCollector<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) for `T`.</span></span>

<span data-ttu-id="53d43-207">Následující příklad kódu vytvoří [objektu blob Storage výstup vazby](functions-bindings-storage-blob.md#using-a-blob-output-binding) s objektem blob cestu, která je definována v době běhu, pak zapíše řetězec do objektu blob.</span><span class="sxs-lookup"><span data-stu-id="53d43-207">The following example code creates a [Storage blob output binding](functions-bindings-storage-blob.md#using-a-blob-output-binding) with blob path that's defined at run time, then writes a string to the blob.</span></span>

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

<span data-ttu-id="53d43-208">[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) definuje [objektu blob Storage](functions-bindings-storage-blob.md) vstupem nebo výstupem vazby a [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) je typ vazby podporované výstup.</span><span class="sxs-lookup"><span data-stu-id="53d43-208">[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) defines the [Storage blob](functions-bindings-storage-blob.md) input or output binding, and [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) is a supported output binding type.</span></span>
<span data-ttu-id="53d43-209">Kód získá výchozí nastavení aplikace pro připojovací řetězec pro účet úložiště (což je `AzureWebJobsStorage`).</span><span class="sxs-lookup"><span data-stu-id="53d43-209">As is, the code gets the default app setting for the Storage account connection string (which is `AzureWebJobsStorage`).</span></span> <span data-ttu-id="53d43-210">Můžete zadat nastavení vlastní aplikace používat tak, že přidáte [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) a předání atribut pole do `BindAsync<T>()`.</span><span class="sxs-lookup"><span data-stu-id="53d43-210">You can specify a custom app setting to use by adding the [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) and passing the attribute array into `BindAsync<T>()`.</span></span> <span data-ttu-id="53d43-211">Například:</span><span class="sxs-lookup"><span data-stu-id="53d43-211">For example,</span></span>

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

<span data-ttu-id="53d43-212">Následující tabulka obsahuje seznam atributy .NET pro každý typ vazby a balíčky, ve kterých jsou definovány.</span><span class="sxs-lookup"><span data-stu-id="53d43-212">The following table lists the .NET attributes for each binding type and the packages in which they are defined.</span></span>

> [!div class="mx-codeBreakAll"]
| <span data-ttu-id="53d43-213">Vazba</span><span class="sxs-lookup"><span data-stu-id="53d43-213">Binding</span></span> | <span data-ttu-id="53d43-214">Atribut</span><span class="sxs-lookup"><span data-stu-id="53d43-214">Attribute</span></span> | <span data-ttu-id="53d43-215">Přidání odkazu</span><span class="sxs-lookup"><span data-stu-id="53d43-215">Add reference</span></span> |
|------|------|------|
| <span data-ttu-id="53d43-216">Databáze Cosmos</span><span class="sxs-lookup"><span data-stu-id="53d43-216">Cosmos DB</span></span> | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.DocumentDB"` |
| <span data-ttu-id="53d43-217">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="53d43-217">Event Hubs</span></span> | <span data-ttu-id="53d43-218">[`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="53d43-218">[`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span></span> | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
| <span data-ttu-id="53d43-219">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="53d43-219">Mobile Apps</span></span> | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
| <span data-ttu-id="53d43-220">Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="53d43-220">Notification Hubs</span></span> | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
| <span data-ttu-id="53d43-221">Service Bus</span><span class="sxs-lookup"><span data-stu-id="53d43-221">Service Bus</span></span> | <span data-ttu-id="53d43-222">[`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="53d43-222">[`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span></span> | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
| <span data-ttu-id="53d43-223">Fronty úložiště</span><span class="sxs-lookup"><span data-stu-id="53d43-223">Storage queue</span></span> | <span data-ttu-id="53d43-224">[`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="53d43-224">[`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span></span> | |
| <span data-ttu-id="53d43-225">Objekt blob úložiště</span><span class="sxs-lookup"><span data-stu-id="53d43-225">Storage blob</span></span> | <span data-ttu-id="53d43-226">[`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="53d43-226">[`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span></span> | |
| <span data-ttu-id="53d43-227">Tabulka úložiště</span><span class="sxs-lookup"><span data-stu-id="53d43-227">Storage table</span></span> | <span data-ttu-id="53d43-228">[`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="53d43-228">[`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span></span> | |
| <span data-ttu-id="53d43-229">Twilio</span><span class="sxs-lookup"><span data-stu-id="53d43-229">Twilio</span></span> | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |



## <a name="next-steps"></a><span data-ttu-id="53d43-230">Další kroky</span><span class="sxs-lookup"><span data-stu-id="53d43-230">Next steps</span></span>
<span data-ttu-id="53d43-231">Další informace najdete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="53d43-231">For more information, see the following resources:</span></span>

* [<span data-ttu-id="53d43-232">Osvědčené postupy pro službu Azure Functions</span><span class="sxs-lookup"><span data-stu-id="53d43-232">Best Practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="53d43-233">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="53d43-233">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="53d43-234">Azure funkce F # referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="53d43-234">Azure Functions F# developer reference</span></span>](functions-reference-fsharp.md)
* [<span data-ttu-id="53d43-235">Referenční informace pro vývojáře Azure funkce NodeJS</span><span class="sxs-lookup"><span data-stu-id="53d43-235">Azure Functions NodeJS developer reference</span></span>](functions-reference-node.md)
* [<span data-ttu-id="53d43-236">Azure funkce triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="53d43-236">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)

[hostitele\.json]: https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json