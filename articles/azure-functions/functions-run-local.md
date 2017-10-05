---
title: "Vývoj a spustit místně na Azure functions | Microsoft Docs"
description: "Zjistěte, jak kód a testovat Azure functions v místním počítači před spuštěním na Azure Functions."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 07/12/2017
ms.author: glenga
ms.openlocfilehash: bbe03973dbd7c70463caa6d1a45efae2ec722c05
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="code-and-test-azure-functions-locally"></a><span data-ttu-id="29351-103">Kód a testovat místně na Azure functions</span><span class="sxs-lookup"><span data-stu-id="29351-103">Code and test Azure functions locally</span></span>

<span data-ttu-id="29351-104">Když [portál Azure] poskytuje úplnou sadu nástrojů pro vývoj a testování Azure Functions se celá řada vývojářů raději místní vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="29351-104">While the [Azure portal] provides a full set of tools for developing and testing Azure Functions, many developers prefer a local development experience.</span></span> <span data-ttu-id="29351-105">Azure Functions umožňuje snadno použít editor Oblíbené kódu a místní vývojové nástroje pro vývoj a testování funkcí v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="29351-105">Azure Functions makes it easy to use your favorite code editor and local development tools to develop and test your functions on your local computer.</span></span> <span data-ttu-id="29351-106">Funkce můžete aktivovat pro události v Azure a při ladění vašich C# a funkce jazyka JavaScript v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="29351-106">Your functions can trigger on events in Azure, and you can debug your C# and JavaScript functions on your local computer.</span></span> 

<span data-ttu-id="29351-107">Pokud jste Azure Functions Visual Studio C# vývojář, také [se integruje s Visual Studio 2017](functions-develop-vs.md).</span><span class="sxs-lookup"><span data-stu-id="29351-107">If you are a Visual Studio C# developer, Azure Functions also [integrates with Visual Studio 2017](functions-develop-vs.md).</span></span>

## <a name="install-the-azure-functions-core-tools"></a><span data-ttu-id="29351-108">Instalace nástroje Azure Functions jádra</span><span class="sxs-lookup"><span data-stu-id="29351-108">Install the Azure Functions Core Tools</span></span>

<span data-ttu-id="29351-109">Nástroje Azure základní funkce je místní verzi modulu runtime Azure Functions, který můžete spustit na místním počítači s Windows.</span><span class="sxs-lookup"><span data-stu-id="29351-109">Azure Functions Core Tools is a local version of the Azure Functions runtime that you can run on your local Windows computer.</span></span> <span data-ttu-id="29351-110">Není emulátor ani simulátor.</span><span class="sxs-lookup"><span data-stu-id="29351-110">It's not an emulator or simulator.</span></span> <span data-ttu-id="29351-111">Je stejný modul runtime, který zajišťuje funkce v Azure.</span><span class="sxs-lookup"><span data-stu-id="29351-111">It's the same runtime that powers Functions in Azure.</span></span>

<span data-ttu-id="29351-112">[Nástroje základní funkce Azure] je k dispozici jako balíčku npm.</span><span class="sxs-lookup"><span data-stu-id="29351-112">The [Azure Functions Core Tools] is provided as an npm package.</span></span> <span data-ttu-id="29351-113">Je nutné nejprve [nainstalovat NodeJS](https://docs.npmjs.com/getting-started/installing-node), což zahrnuje npm.</span><span class="sxs-lookup"><span data-stu-id="29351-113">You must first [install NodeJS](https://docs.npmjs.com/getting-started/installing-node), which includes npm.</span></span>  

>[!NOTE]
><span data-ttu-id="29351-114">V tomto okamžiku můžete balíček nástroje základní funkce Azure nainstalovat pouze na počítačích s Windows.</span><span class="sxs-lookup"><span data-stu-id="29351-114">At this time, the Azure Functions Core Tools package can only be installed on Windows computers.</span></span> <span data-ttu-id="29351-115">Toto omezení je kvůli dočasné omezení na hostiteli funkce.</span><span class="sxs-lookup"><span data-stu-id="29351-115">This restriction is due to a temporary limitation in the Functions host.</span></span>

<span data-ttu-id="29351-116">[Nástroje základní funkce Azure] přidá následující aliasy příkazů:</span><span class="sxs-lookup"><span data-stu-id="29351-116">[Azure Functions Core Tools] adds the following command aliases:</span></span>
* <span data-ttu-id="29351-117">**Func**</span><span class="sxs-lookup"><span data-stu-id="29351-117">**func**</span></span>
* <span data-ttu-id="29351-118">**azfun**</span><span class="sxs-lookup"><span data-stu-id="29351-118">**azfun**</span></span>
* <span data-ttu-id="29351-119">**azurefunctions**</span><span class="sxs-lookup"><span data-stu-id="29351-119">**azurefunctions**</span></span>

<span data-ttu-id="29351-120">Všechny tyto alias lze použít místo `func` zobrazeno v příkladech v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="29351-120">All of these alias can be used instead of `func` shown in the examples in this topic.</span></span>

## <a name="create-a-local-functions-project"></a><span data-ttu-id="29351-121">Vytvoření projektu místní funkce</span><span class="sxs-lookup"><span data-stu-id="29351-121">Create a local Functions project</span></span>

<span data-ttu-id="29351-122">Při místním spuštění projektu funkce je adresář, který má soubory host.json a local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="29351-122">When running locally, a Functions project is a directory that has the files host.json and local.settings.json.</span></span> <span data-ttu-id="29351-123">Tento adresář je ekvivalentem funkce aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="29351-123">This directory is the equivalent of a function app in Azure.</span></span> <span data-ttu-id="29351-124">Další informace o struktuře složky Azure Functions najdete v tématu [Příručka pro vývojáře Azure Functions](functions-reference.md#folder-structure).</span><span class="sxs-lookup"><span data-stu-id="29351-124">To learn more about the Azure Functions folder structure, see the [Azure Functions developers guide](functions-reference.md#folder-structure).</span></span>

<span data-ttu-id="29351-125">Na příkazovém řádku spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="29351-125">At a command prompt, run the following command:</span></span>

```
func init MyFunctionProj
```

<span data-ttu-id="29351-126">Výstup bude vypadat jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="29351-126">The output looks like the following example:</span></span>

```
Writing .gitignore
Writing host.json
Writing local.settings.json
Created launch.json
Initialized empty Git repository in D:/Code/Playground/MyFunctionProj/.git/
```

<span data-ttu-id="29351-127">Odhlásit se od vytvoření úložiště Git, použijte možnost `--no-source-control [-n]`.</span><span class="sxs-lookup"><span data-stu-id="29351-127">To opt out of creating a Git repository, use the option `--no-source-control [-n]`.</span></span>

<a name="local-settings"></a>

## <a name="local-settings-file"></a><span data-ttu-id="29351-128">Nastavení místního souboru</span><span class="sxs-lookup"><span data-stu-id="29351-128">Local settings file</span></span>

<span data-ttu-id="29351-129">Soubor local.settings.json ukládá nastavení aplikace, řetězce připojení a nastavení nástroje základní funkce Azure.</span><span class="sxs-lookup"><span data-stu-id="29351-129">The file local.settings.json stores app settings, connection strings, and settings for Azure Functions Core Tools.</span></span> <span data-ttu-id="29351-130">Má následující strukturu:</span><span class="sxs-lookup"><span data-stu-id="29351-130">It has the following structure:</span></span>

```json
{
  "IsEncrypted": false,   
  "Values": {
    "AzureWebJobsStorage": "<connection string>", 
    "AzureWebJobsDashboard": "<connection string>", 
  },
  "Host": {
    "LocalHttpPort": 7071, 
    "CORS": "*" 
  },
  "ConnectionStrings": {
    "SQLConnectionString": "Value"
  }
}
```
| <span data-ttu-id="29351-131">Nastavení</span><span class="sxs-lookup"><span data-stu-id="29351-131">Setting</span></span>      | <span data-ttu-id="29351-132">Popis</span><span class="sxs-lookup"><span data-stu-id="29351-132">Description</span></span>                            |
| ------------ | -------------------------------------- |
| <span data-ttu-id="29351-133">**IsEncrypted**</span><span class="sxs-lookup"><span data-stu-id="29351-133">**IsEncrypted**</span></span> | <span data-ttu-id="29351-134">Pokud nastavíte hodnotu **true**, všechny hodnoty jsou šifrované pomocí klíče místního počítače.</span><span class="sxs-lookup"><span data-stu-id="29351-134">When set to **true**, all values are encrypted using a local machine key.</span></span> <span data-ttu-id="29351-135">Použít s `func settings` příkazy.</span><span class="sxs-lookup"><span data-stu-id="29351-135">Used with `func settings` commands.</span></span> <span data-ttu-id="29351-136">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="29351-136">Default value is **false**.</span></span> |
| <span data-ttu-id="29351-137">**Hodnoty**</span><span class="sxs-lookup"><span data-stu-id="29351-137">**Values**</span></span> | <span data-ttu-id="29351-138">Kolekce nastavení aplikace používá při místním spuštění.</span><span class="sxs-lookup"><span data-stu-id="29351-138">Collection of application settings used when running locally.</span></span> <span data-ttu-id="29351-139">Přidáte nastavení aplikace s tímto objektem.</span><span class="sxs-lookup"><span data-stu-id="29351-139">Add your application settings to this object.</span></span>  |
| <span data-ttu-id="29351-140">**AzureWebJobsStorage**</span><span class="sxs-lookup"><span data-stu-id="29351-140">**AzureWebJobsStorage**</span></span> | <span data-ttu-id="29351-141">Nastaví připojovací řetězec k účtu Azure Storage, který se používá interně modulem runtime Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="29351-141">Sets the connection string to the Azure Storage account that is used internally by the Azure Functions runtime.</span></span> <span data-ttu-id="29351-142">Účet úložiště podporuje vaše funkce aktivační události.</span><span class="sxs-lookup"><span data-stu-id="29351-142">The storage account supports your function's triggers.</span></span> <span data-ttu-id="29351-143">Toto nastavení připojení účtu úložiště je vyžadována pro všechny funkce, s výjimkou funkce protokolu HTTP aktivované.</span><span class="sxs-lookup"><span data-stu-id="29351-143">This storage account connection setting is required for all functions except for HTTP triggered functions.</span></span>  |
| <span data-ttu-id="29351-144">**AzureWebJobsDashboard**</span><span class="sxs-lookup"><span data-stu-id="29351-144">**AzureWebJobsDashboard**</span></span> | <span data-ttu-id="29351-145">Nastaví připojovací řetězec k účtu úložiště Azure, který se používá k ukládání protokolů funkce.</span><span class="sxs-lookup"><span data-stu-id="29351-145">Sets the connection string to the Azure Storage account that is used to store the function logs.</span></span> <span data-ttu-id="29351-146">Tato volitelná hodnota zpřístupní protokoly na portálu.</span><span class="sxs-lookup"><span data-stu-id="29351-146">This optional value makes the logs accessible in the portal.</span></span>|
| <span data-ttu-id="29351-147">**Hostitele**</span><span class="sxs-lookup"><span data-stu-id="29351-147">**Host**</span></span> | <span data-ttu-id="29351-148">Nastavení v této části přizpůsobit funkce hostitelský proces, při místním spuštění.</span><span class="sxs-lookup"><span data-stu-id="29351-148">Settings in this section customize the Functions host process when running locally.</span></span> | 
| <span data-ttu-id="29351-149">**LocalHttpPort**</span><span class="sxs-lookup"><span data-stu-id="29351-149">**LocalHttpPort**</span></span> | <span data-ttu-id="29351-150">Nastaví výchozí port použitý při spuštění místního hostitele funkce (`func host start` a `func run`).</span><span class="sxs-lookup"><span data-stu-id="29351-150">Sets the default port used when running the local Functions host (`func host start` and `func run`).</span></span> <span data-ttu-id="29351-151">`--port` Možnost příkazového řádku má přednost před tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="29351-151">The `--port` command-line option takes precedence over this value.</span></span> |
| <span data-ttu-id="29351-152">**CORS**</span><span class="sxs-lookup"><span data-stu-id="29351-152">**CORS**</span></span> | <span data-ttu-id="29351-153">Definuje zdroje povolené pro [(CORS) pro sdílení prostředků různého původu](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="29351-153">Defines the origins allowed for [cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span> <span data-ttu-id="29351-154">Zdroje se zadávají jako seznam oddělený čárkami bez mezer.</span><span class="sxs-lookup"><span data-stu-id="29351-154">Origins are supplied as a comma-separated list with no spaces.</span></span> <span data-ttu-id="29351-155">Hodnota zástupného znaku (**\***) je podporováno, což umožňuje požadavky od jakýkoli původ.</span><span class="sxs-lookup"><span data-stu-id="29351-155">The wildcard value (**\***) is supported, which allows requests from any origin.</span></span> |
| <span data-ttu-id="29351-156">**ConnectionStrings**</span><span class="sxs-lookup"><span data-stu-id="29351-156">**ConnectionStrings**</span></span> | <span data-ttu-id="29351-157">Obsahuje databázové připojovací řetězce pro funkcí.</span><span class="sxs-lookup"><span data-stu-id="29351-157">Contains the database connection strings for your functions.</span></span> <span data-ttu-id="29351-158">Připojovací řetězce v tento objekt se přidají do prostředí s typem zprostředkovatele **System.Data.SqlClient**.</span><span class="sxs-lookup"><span data-stu-id="29351-158">Connection strings in this object are added to the environment with the provider type of **System.Data.SqlClient**.</span></span>  | 

<span data-ttu-id="29351-159">Většina triggerů a vazeb mít **připojení** vlastnost, která se mapuje na název nastavení aplikace nebo proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="29351-159">Most triggers and bindings have a **Connection** property that maps to the name of an environment variable or app setting.</span></span> <span data-ttu-id="29351-160">Pro každou vlastnost připojení musí být definována v souboru local.settings.json nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="29351-160">For each connection property, there must be app setting defined in local.settings.json file.</span></span> 

<span data-ttu-id="29351-161">Tato nastavení lze také přečíst v kódu jako proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="29351-161">These settings can also be read in your code as environment variables.</span></span> <span data-ttu-id="29351-162">V jazyce C#, použijte [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) nebo [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="29351-162">In C#, use [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) or [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx).</span></span> <span data-ttu-id="29351-163">V jazyce JavaScript, použijte `process.env`.</span><span class="sxs-lookup"><span data-stu-id="29351-163">In JavaScript, use `process.env`.</span></span> <span data-ttu-id="29351-164">Bylo nastaveno jako systémové proměnné prostředí mají přednost před hodnot v souboru local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="29351-164">Settings specified as a system environment variable take precedence over values in the local.settings.json file.</span></span> 

<span data-ttu-id="29351-165">Nastavení v souboru local.settings.json používají funkce nástroje jenom, při místním spuštění.</span><span class="sxs-lookup"><span data-stu-id="29351-165">Settings in the local.settings.json file are only used by Functions tools when running locally.</span></span> <span data-ttu-id="29351-166">Ve výchozím nastavení tato nastavení se nemigrují automaticky při publikování projektu do Azure.</span><span class="sxs-lookup"><span data-stu-id="29351-166">By default, these settings are not migrated automatically when the project is published to Azure.</span></span> <span data-ttu-id="29351-167">Použití `--publish-local-settings` přepínač [při publikování](#publish) a ujistěte se, tato nastavení jsou přidány do aplikaci funkce v Azure.</span><span class="sxs-lookup"><span data-stu-id="29351-167">Use the `--publish-local-settings` switch [when you publish](#publish) to make sure these settings are added to the function app in Azure.</span></span>

<span data-ttu-id="29351-168">Když je nastavená žádný platný úložiště připojovací řetězec pro **AzureWebJobsStorage**, se zobrazí následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="29351-168">When no valid storage connection string is set for **AzureWebJobsStorage**, the following error message is shown:</span></span>  

><span data-ttu-id="29351-169">Chybí hodnota pro AzureWebJobsStorage v local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="29351-169">Missing value for AzureWebJobsStorage in local.settings.json.</span></span> <span data-ttu-id="29351-170">To je potřeba pro všechny aktivační události než HTTP.</span><span class="sxs-lookup"><span data-stu-id="29351-170">This is required for all triggers other than HTTP.</span></span> <span data-ttu-id="29351-171">Můžete spustit ' func azure functionary načítání app nastavení <functionAppName>, nebo zadejte připojovací řetězec v local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="29351-171">You can run 'func azure functionary fetch-app-settings <functionAppName>' or specify a connection string in local.settings.json.</span></span>
  
[!INCLUDE [Note to not use local storage](../../includes/functions-local-settings-note.md)]

### <a name="configure-app-settings"></a><span data-ttu-id="29351-172">Konfigurace nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="29351-172">Configure app settings</span></span>

<span data-ttu-id="29351-173">Pokud chcete nastavit hodnotu pro připojovací řetězce, můžete provést jednu z těchto možností:</span><span class="sxs-lookup"><span data-stu-id="29351-173">To set a value for connection strings, you can do one of the following:</span></span>
* <span data-ttu-id="29351-174">Zadejte připojovací řetězec z [Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="29351-174">Enter the connection string from [Azure Storage Explorer](http://storageexplorer.com/).</span></span>
* <span data-ttu-id="29351-175">Použijte jednu z následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="29351-175">Use one of the following commands:</span></span>

    ```
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    ```
    func azure functionapp storage fetch-connection-string <StorageAccountName>
    ```
    <span data-ttu-id="29351-176">Oba příkazy vyžadovat, abyste první přihlášení do Azure.</span><span class="sxs-lookup"><span data-stu-id="29351-176">Both commands require you to first sign-in to Azure.</span></span>

## <a name="create-a-function"></a><span data-ttu-id="29351-177">Vytvoření funkce</span><span class="sxs-lookup"><span data-stu-id="29351-177">Create a function</span></span>

<span data-ttu-id="29351-178">Pokud chcete vytvořit funkci, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="29351-178">To create a function, run the following command:</span></span>

```
func new
``` 
<span data-ttu-id="29351-179">`func new`podporuje následující volitelné argumenty:</span><span class="sxs-lookup"><span data-stu-id="29351-179">`func new` supports the following optional arguments:</span></span>

| <span data-ttu-id="29351-180">Argument</span><span class="sxs-lookup"><span data-stu-id="29351-180">Argument</span></span>     | <span data-ttu-id="29351-181">Popis</span><span class="sxs-lookup"><span data-stu-id="29351-181">Description</span></span>                            |
| ------------ | -------------------------------------- |
| **`--language -l`** | <span data-ttu-id="29351-182">Šablona programovací jazyk, například C#, F # nebo JavaScript.</span><span class="sxs-lookup"><span data-stu-id="29351-182">The template programming language, such as C#, F#, or JavaScript.</span></span> |
| **`--template -t`** | <span data-ttu-id="29351-183">Název šablony.</span><span class="sxs-lookup"><span data-stu-id="29351-183">The template name.</span></span> |
| **`--name -n`** | <span data-ttu-id="29351-184">Název funkce.</span><span class="sxs-lookup"><span data-stu-id="29351-184">The function name.</span></span> |

<span data-ttu-id="29351-185">Například pokud chcete vytvořit aktivační událost jazyka JavaScript HTTP, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="29351-185">For example, to create a JavaScript HTTP trigger, run:</span></span>

```
func new --language JavaScript --template HttpTrigger --name MyHttpTrigger
```

<span data-ttu-id="29351-186">Chcete-li vytvořit funkci aktivovaného fronty, spusťte:</span><span class="sxs-lookup"><span data-stu-id="29351-186">To create a queue-triggered function, run:</span></span>

```
func new --language JavaScript --template QueueTrigger --name QueueTriggerJS
```

## <a name="run-functions-locally"></a><span data-ttu-id="29351-187">Místní spuštění funkce</span><span class="sxs-lookup"><span data-stu-id="29351-187">Run functions locally</span></span>

<span data-ttu-id="29351-188">Pokud chcete spustit funkce projektu, spusťte hostiteli funkce.</span><span class="sxs-lookup"><span data-stu-id="29351-188">To run a Functions project, run the Functions host.</span></span> <span data-ttu-id="29351-189">Hostitel umožňuje aktivačních událostí pro všechny funkce v projektu:</span><span class="sxs-lookup"><span data-stu-id="29351-189">The host enables triggers for all functions in the project:</span></span>

```
func host start
```

<span data-ttu-id="29351-190">`func host start`podporuje následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="29351-190">`func host start` supports the following options:</span></span>

| <span data-ttu-id="29351-191">Možnost</span><span class="sxs-lookup"><span data-stu-id="29351-191">Option</span></span>     | <span data-ttu-id="29351-192">Popis</span><span class="sxs-lookup"><span data-stu-id="29351-192">Description</span></span>                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | <span data-ttu-id="29351-193">Místní port pro naslouchání.</span><span class="sxs-lookup"><span data-stu-id="29351-193">The local port to listen on.</span></span> <span data-ttu-id="29351-194">Výchozí hodnota: 7071.</span><span class="sxs-lookup"><span data-stu-id="29351-194">Default value: 7071.</span></span> |
| **`--debug <type>`** | <span data-ttu-id="29351-195">Možnosti jsou `VSCode` a `VS`.</span><span class="sxs-lookup"><span data-stu-id="29351-195">The options are `VSCode` and `VS`.</span></span> |
| **`--cors`** | <span data-ttu-id="29351-196">Seznam původů CORS, bez mezer oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="29351-196">A comma-separated list of CORS origins, with no spaces.</span></span> |
| **`--nodeDebugPort -n`** | <span data-ttu-id="29351-197">Port pro uzel ladicí program používat.</span><span class="sxs-lookup"><span data-stu-id="29351-197">The port for the node debugger to use.</span></span> <span data-ttu-id="29351-198">Výchozí hodnota: Hodnota z launch.json nebo 5858.</span><span class="sxs-lookup"><span data-stu-id="29351-198">Default: A value from launch.json or 5858.</span></span> |
| **`--debugLevel -d`** | <span data-ttu-id="29351-199">Úroveň trasování konzoly (vypnuto, podrobné nastavení, informace, varování nebo chyba).</span><span class="sxs-lookup"><span data-stu-id="29351-199">The console trace level (off, verbose, info, warning, or error).</span></span> <span data-ttu-id="29351-200">Výchozí: informace.</span><span class="sxs-lookup"><span data-stu-id="29351-200">Default: Info.</span></span>|
| **`--timeout -t`** | <span data-ttu-id="29351-201">Časový limit pro o spuštění t hostitele na funkce v sekundách.</span><span class="sxs-lookup"><span data-stu-id="29351-201">The time out for the Functions host t     o start, in seconds.</span></span> <span data-ttu-id="29351-202">Výchozí hodnota: 20 sekund.</span><span class="sxs-lookup"><span data-stu-id="29351-202">Default: 20 seconds.</span></span>|
| **`--useHttps`** | <span data-ttu-id="29351-203">Vytvořit vazbu na https://localhost: {port} místo na http://localhost: {port}.</span><span class="sxs-lookup"><span data-stu-id="29351-203">Bind to https://localhost:{port} rather than to http://localhost:{port}.</span></span> <span data-ttu-id="29351-204">Ve výchozím nastavení tato možnost vytvoří důvěryhodný certifikát v počítači.</span><span class="sxs-lookup"><span data-stu-id="29351-204">By default, this option creates a trusted certificate on your computer.</span></span>|
| **`--pause-on-error`** | <span data-ttu-id="29351-205">Pozastavení další vstupní před ukončení procesu.</span><span class="sxs-lookup"><span data-stu-id="29351-205">Pause for additional input before exiting the process.</span></span> <span data-ttu-id="29351-206">Užitečné při spuštění nástroje základní funkce Azure z integrované vývojové prostředí (IDE).</span><span class="sxs-lookup"><span data-stu-id="29351-206">Useful when launching Azure Functions Core Tools from an integrated development environment (IDE).</span></span>|

<span data-ttu-id="29351-207">Při spuštění funkce hostitele výstupy funkce aktivované protokolem URL HTTP:</span><span class="sxs-lookup"><span data-stu-id="29351-207">When the Functions host starts, it outputs the URL of HTTP-triggered functions:</span></span>

```
Found the following functions:
Host.Functions.MyHttpTrigger

ob host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="debug-in-vs-code-or-visual-studio"></a><span data-ttu-id="29351-208">Ladění ve VS Code nebo Visual Studio</span><span class="sxs-lookup"><span data-stu-id="29351-208">Debug in VS Code or Visual Studio</span></span>

<span data-ttu-id="29351-209">Chcete-li připojit ladicí program, předat `--debug` argument.</span><span class="sxs-lookup"><span data-stu-id="29351-209">To attach a debugger, pass the `--debug` argument.</span></span> <span data-ttu-id="29351-210">Chcete-li ladit funkce jazyka JavaScript, použijte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="29351-210">To debug JavaScript functions, use Visual Studio Code.</span></span> <span data-ttu-id="29351-211">Pro C# funkce použijte sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="29351-211">For C# functions, use Visual Studio.</span></span>

<span data-ttu-id="29351-212">Chcete-li ladit funkce jazyka C#, použijte `--debug vs`.</span><span class="sxs-lookup"><span data-stu-id="29351-212">To debug C# functions, use `--debug vs`.</span></span> <span data-ttu-id="29351-213">Můžete také použít [Azure funkce 2017 nástroje sady Visual Studio](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span><span class="sxs-lookup"><span data-stu-id="29351-213">You can also use [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span></span> 

<span data-ttu-id="29351-214">Spusťte hostiteli a nastavte ladění jazyka JavaScript, spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="29351-214">To launch the host and set up JavaScript debugging, run:</span></span>

```
func host start --debug vscode
```

<span data-ttu-id="29351-215">Potom v sadě Visual Studio Code v **ladění** zobrazit, vyberte možnost **připojit k Azure Functions**.</span><span class="sxs-lookup"><span data-stu-id="29351-215">Then, in Visual Studio Code, in the **Debug** view, select **Attach to Azure Functions**.</span></span> <span data-ttu-id="29351-216">Můžete připojit zarážky, zkontrolujte proměnné a krok prostřednictvím kódu.</span><span class="sxs-lookup"><span data-stu-id="29351-216">You can attach breakpoints, inspect variables, and step through code.</span></span>

![Ladění jazyka JavaScript s kódem jazyka Visual Studio](./media/functions-run-local/vscode-javascript-debugging.png)

### <a name="passing-test-data-to-a-function"></a><span data-ttu-id="29351-218">Předávání testů dat na funkci</span><span class="sxs-lookup"><span data-stu-id="29351-218">Passing test data to a function</span></span>

<span data-ttu-id="29351-219">Můžete také vyvolat funkci přímo pomocí `func run <FunctionName>` a zadejte vstupní data pro funkce.</span><span class="sxs-lookup"><span data-stu-id="29351-219">You can also invoke a function directly by using `func run <FunctionName>` and provide input data for the function.</span></span> <span data-ttu-id="29351-220">Tento příkaz je podobná spuštění funkce pomocí **Test** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="29351-220">This command is similar to running a function using the **Test** tab in the Azure portal.</span></span> <span data-ttu-id="29351-221">Tento příkaz spustí celý funkce hostitele.</span><span class="sxs-lookup"><span data-stu-id="29351-221">This command launches the entire Functions host.</span></span>

<span data-ttu-id="29351-222">`func run`podporuje následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="29351-222">`func run` supports the following options:</span></span>

| <span data-ttu-id="29351-223">Možnost</span><span class="sxs-lookup"><span data-stu-id="29351-223">Option</span></span>     | <span data-ttu-id="29351-224">Popis</span><span class="sxs-lookup"><span data-stu-id="29351-224">Description</span></span>                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | <span data-ttu-id="29351-225">Vložený obsah.</span><span class="sxs-lookup"><span data-stu-id="29351-225">Inline content.</span></span> |
| **`--debug -d`** | <span data-ttu-id="29351-226">Připojte ladicí program k hostitelskému procesu před spuštěním funkce.</span><span class="sxs-lookup"><span data-stu-id="29351-226">Attach a debugger to the host process before running the function.</span></span>|
| **`--timeout -t`** | <span data-ttu-id="29351-227">Doba čekání (v sekundách), dokud místního hostitele funkce není připravena.</span><span class="sxs-lookup"><span data-stu-id="29351-227">Time to wait (in seconds) until the local Functions host is ready.</span></span>|
| **`--file -f`** | <span data-ttu-id="29351-228">Název souboru, který chcete použít jako obsah.</span><span class="sxs-lookup"><span data-stu-id="29351-228">The file name to use as content.</span></span>|
| **`--no-interactive`** | <span data-ttu-id="29351-229">Nezobrazí výzvu pro vstup.</span><span class="sxs-lookup"><span data-stu-id="29351-229">Does not prompt for input.</span></span> <span data-ttu-id="29351-230">Tato možnost je užitečná pro scénáře automatizace.</span><span class="sxs-lookup"><span data-stu-id="29351-230">Useful for automation scenarios.</span></span>|

<span data-ttu-id="29351-231">Například volání funkce aktivované protokolem HTTP a předat obsahu, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="29351-231">For example, to call an HTTP-triggered function and pass content body, run the following command:</span></span>

```
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

## <span data-ttu-id="29351-232"><a name="publish"></a>Publikování v Azure</span><span class="sxs-lookup"><span data-stu-id="29351-232"><a name="publish"></a>Publish to Azure</span></span>

<span data-ttu-id="29351-233">K publikování funkce projektu do aplikace pro funkce v Azure, použijte `publish` příkaz:</span><span class="sxs-lookup"><span data-stu-id="29351-233">To publish a Functions project to a function app in Azure, use the `publish` command:</span></span>

```
func azure functionapp publish <FunctionAppName>
```

<span data-ttu-id="29351-234">Můžete použít následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="29351-234">You can use the following options:</span></span>

| <span data-ttu-id="29351-235">Možnost</span><span class="sxs-lookup"><span data-stu-id="29351-235">Option</span></span>     | <span data-ttu-id="29351-236">Popis</span><span class="sxs-lookup"><span data-stu-id="29351-236">Description</span></span>                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  <span data-ttu-id="29351-237">Nastavení publikování v local.settings.json do Azure, výzvy přepsat, pokud nastavení již existuje.</span><span class="sxs-lookup"><span data-stu-id="29351-237">Publish settings in local.settings.json to Azure, prompting to overwrite if the setting already exists.</span></span>|
| **`--overwrite-settings -y`** | <span data-ttu-id="29351-238">Musí být použit s `-i`.</span><span class="sxs-lookup"><span data-stu-id="29351-238">Must be used with `-i`.</span></span> <span data-ttu-id="29351-239">Pokud se liší, přepíše místní hodnotou AppSettings v Azure.</span><span class="sxs-lookup"><span data-stu-id="29351-239">Overwrites AppSettings in Azure with local value if different.</span></span> <span data-ttu-id="29351-240">Výchozí hodnota je výzva.</span><span class="sxs-lookup"><span data-stu-id="29351-240">Default is prompt.</span></span>|

<span data-ttu-id="29351-241">`publish` Příkaz odešle obsah adresáře projektu funkce.</span><span class="sxs-lookup"><span data-stu-id="29351-241">The `publish` command uploads the contents of the Functions project directory.</span></span> <span data-ttu-id="29351-242">Pokud odstraníte soubory místně, `publish` příkaz nedojde k jejich odstranění z Azure.</span><span class="sxs-lookup"><span data-stu-id="29351-242">If you delete files locally, the `publish` command does not delete them from Azure.</span></span> <span data-ttu-id="29351-243">Můžete odstranit soubory v Azure pomocí [Kudu nástroj](functions-how-to-use-azure-function-app-settings.md#kudu) v [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="29351-243">You can delete files in Azure by using the [Kudu tool](functions-how-to-use-azure-function-app-settings.md#kudu) in the [Azure portal].</span></span>

## <a name="next-steps"></a><span data-ttu-id="29351-244">Další kroky</span><span class="sxs-lookup"><span data-stu-id="29351-244">Next steps</span></span>

<span data-ttu-id="29351-245">Nástroje Azure základní funkce je [otevřít zdroj a hostované na Githubu](https://github.com/azure/azure-functions-cli).</span><span class="sxs-lookup"><span data-stu-id="29351-245">Azure Functions Core Tools is [open source and hosted on GitHub](https://github.com/azure/azure-functions-cli).</span></span>  
<span data-ttu-id="29351-246">Do souboru žádost chyb nebo funkce [otevřete potíže Githubu](https://github.com/azure/azure-functions-cli/issues).</span><span class="sxs-lookup"><span data-stu-id="29351-246">To file a bug or feature request, [open a GitHub issue](https://github.com/azure/azure-functions-cli/issues).</span></span> 

<!-- LINKS -->

[Nástroje základní funkce Azure]: https://www.npmjs.com/package/azure-functions-core-tools
[portál Azure]: https://portal.azure.com 
