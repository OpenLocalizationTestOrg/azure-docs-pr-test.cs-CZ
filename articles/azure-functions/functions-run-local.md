---
title: "aaaDevelop a spuštění Azure funkce místně | Microsoft Docs"
description: "Zjistěte, jak toocode a testování Azure funkce v místním počítači před spuštěním na Azure Functions."
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
ms.openlocfilehash: 342ed4d6df41a2d2df9067948e19e347bb52c844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="code-and-test-azure-functions-locally"></a><span data-ttu-id="d9918-103">Kód a testovat místně na Azure functions</span><span class="sxs-lookup"><span data-stu-id="d9918-103">Code and test Azure functions locally</span></span>

<span data-ttu-id="d9918-104">Při hello [portál Azure] poskytuje úplnou sadu nástrojů pro vývoj a testování Azure Functions se celá řada vývojářů raději místní vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="d9918-104">While hello [Azure portal] provides a full set of tools for developing and testing Azure Functions, many developers prefer a local development experience.</span></span> <span data-ttu-id="d9918-105">Azure Functions umožňuje snadno toouse své oblíbené kódu editoru a toodevelop nástroje pro místní vývoj a testování funkcí v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="d9918-105">Azure Functions makes it easy toouse your favorite code editor and local development tools toodevelop and test your functions on your local computer.</span></span> <span data-ttu-id="d9918-106">Funkce můžete aktivovat pro události v Azure a při ladění vašich C# a funkce jazyka JavaScript v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="d9918-106">Your functions can trigger on events in Azure, and you can debug your C# and JavaScript functions on your local computer.</span></span> 

<span data-ttu-id="d9918-107">Pokud jste Azure Functions Visual Studio C# vývojář, také [se integruje s Visual Studio 2017](functions-develop-vs.md).</span><span class="sxs-lookup"><span data-stu-id="d9918-107">If you are a Visual Studio C# developer, Azure Functions also [integrates with Visual Studio 2017](functions-develop-vs.md).</span></span>

## <a name="install-hello-azure-functions-core-tools"></a><span data-ttu-id="d9918-108">Instalace nástroje základní funkce Azure hello</span><span class="sxs-lookup"><span data-stu-id="d9918-108">Install hello Azure Functions Core Tools</span></span>

<span data-ttu-id="d9918-109">Nástroje Azure základní funkce je místní verzi modulu runtime Azure Functions hello, který můžete spustit na místním počítači s Windows.</span><span class="sxs-lookup"><span data-stu-id="d9918-109">Azure Functions Core Tools is a local version of hello Azure Functions runtime that you can run on your local Windows computer.</span></span> <span data-ttu-id="d9918-110">Není emulátor ani simulátor.</span><span class="sxs-lookup"><span data-stu-id="d9918-110">It's not an emulator or simulator.</span></span> <span data-ttu-id="d9918-111">Má hello stejné runtime této zajišťuje funkce v Azure.</span><span class="sxs-lookup"><span data-stu-id="d9918-111">It's hello same runtime that powers Functions in Azure.</span></span>

<span data-ttu-id="d9918-112">Hello [nástroje základní funkce Azure] je k dispozici jako balíčku npm.</span><span class="sxs-lookup"><span data-stu-id="d9918-112">hello [Azure Functions Core Tools] is provided as an npm package.</span></span> <span data-ttu-id="d9918-113">Je nutné nejprve [nainstalovat NodeJS](https://docs.npmjs.com/getting-started/installing-node), což zahrnuje npm.</span><span class="sxs-lookup"><span data-stu-id="d9918-113">You must first [install NodeJS](https://docs.npmjs.com/getting-started/installing-node), which includes npm.</span></span>  

>[!NOTE]
><span data-ttu-id="d9918-114">V tomto okamžiku můžete balíček nástroje základní funkce Azure hello nainstalovat pouze na počítačích s Windows.</span><span class="sxs-lookup"><span data-stu-id="d9918-114">At this time, hello Azure Functions Core Tools package can only be installed on Windows computers.</span></span> <span data-ttu-id="d9918-115">Toto omezení je kvůli dočasné omezení tooa v hostiteli funkce hello.</span><span class="sxs-lookup"><span data-stu-id="d9918-115">This restriction is due tooa temporary limitation in hello Functions host.</span></span>

<span data-ttu-id="d9918-116">[nástroje základní funkce Azure] přidá hello následující aliasy příkazů:</span><span class="sxs-lookup"><span data-stu-id="d9918-116">[Azure Functions Core Tools] adds hello following command aliases:</span></span>
* <span data-ttu-id="d9918-117">**Func**</span><span class="sxs-lookup"><span data-stu-id="d9918-117">**func**</span></span>
* <span data-ttu-id="d9918-118">**azfun**</span><span class="sxs-lookup"><span data-stu-id="d9918-118">**azfun**</span></span>
* <span data-ttu-id="d9918-119">**azurefunctions**</span><span class="sxs-lookup"><span data-stu-id="d9918-119">**azurefunctions**</span></span>

<span data-ttu-id="d9918-120">Všechny tyto alias lze použít místo `func` hello příkladů v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="d9918-120">All of these alias can be used instead of `func` shown in hello examples in this topic.</span></span>

## <a name="create-a-local-functions-project"></a><span data-ttu-id="d9918-121">Vytvoření projektu místní funkce</span><span class="sxs-lookup"><span data-stu-id="d9918-121">Create a local Functions project</span></span>

<span data-ttu-id="d9918-122">Při místním spuštění projektu funkce je adresář, který má hello soubory host.json a local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="d9918-122">When running locally, a Functions project is a directory that has hello files host.json and local.settings.json.</span></span> <span data-ttu-id="d9918-123">Tento adresář je hello ekvivalentní funkce aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="d9918-123">This directory is hello equivalent of a function app in Azure.</span></span> <span data-ttu-id="d9918-124">toolearn Další informace o hello struktura složek Azure Functions, najdete v části hello [Příručka pro vývojáře Azure Functions](functions-reference.md#folder-structure).</span><span class="sxs-lookup"><span data-stu-id="d9918-124">toolearn more about hello Azure Functions folder structure, see hello [Azure Functions developers guide](functions-reference.md#folder-structure).</span></span>

<span data-ttu-id="d9918-125">Na příkazovém řádku spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d9918-125">At a command prompt, run hello following command:</span></span>

```
func init MyFunctionProj
```

<span data-ttu-id="d9918-126">výstup Hello vypadá hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="d9918-126">hello output looks like hello following example:</span></span>

```
Writing .gitignore
Writing host.json
Writing local.settings.json
Created launch.json
Initialized empty Git repository in D:/Code/Playground/MyFunctionProj/.git/
```

<span data-ttu-id="d9918-127">tooopt mimo vytvoření úložiště Git, možnost použití hello `--no-source-control [-n]`.</span><span class="sxs-lookup"><span data-stu-id="d9918-127">tooopt out of creating a Git repository, use hello option `--no-source-control [-n]`.</span></span>

<a name="local-settings"></a>

## <a name="local-settings-file"></a><span data-ttu-id="d9918-128">Nastavení místního souboru</span><span class="sxs-lookup"><span data-stu-id="d9918-128">Local settings file</span></span>

<span data-ttu-id="d9918-129">Hello souboru local.settings.json ukládá nastavení aplikace, řetězce připojení a nastavení nástroje základní funkce Azure.</span><span class="sxs-lookup"><span data-stu-id="d9918-129">hello file local.settings.json stores app settings, connection strings, and settings for Azure Functions Core Tools.</span></span> <span data-ttu-id="d9918-130">Má hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="d9918-130">It has hello following structure:</span></span>

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
| <span data-ttu-id="d9918-131">Nastavení</span><span class="sxs-lookup"><span data-stu-id="d9918-131">Setting</span></span>      | <span data-ttu-id="d9918-132">Popis</span><span class="sxs-lookup"><span data-stu-id="d9918-132">Description</span></span>                            |
| ------------ | -------------------------------------- |
| <span data-ttu-id="d9918-133">**IsEncrypted**</span><span class="sxs-lookup"><span data-stu-id="d9918-133">**IsEncrypted**</span></span> | <span data-ttu-id="d9918-134">Pokud nastavíte příliš**true**, všechny hodnoty jsou šifrované pomocí klíče místního počítače.</span><span class="sxs-lookup"><span data-stu-id="d9918-134">When set too**true**, all values are encrypted using a local machine key.</span></span> <span data-ttu-id="d9918-135">Použít s `func settings` příkazy.</span><span class="sxs-lookup"><span data-stu-id="d9918-135">Used with `func settings` commands.</span></span> <span data-ttu-id="d9918-136">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="d9918-136">Default value is **false**.</span></span> |
| <span data-ttu-id="d9918-137">**Hodnoty**</span><span class="sxs-lookup"><span data-stu-id="d9918-137">**Values**</span></span> | <span data-ttu-id="d9918-138">Kolekce nastavení aplikace používá při místním spuštění.</span><span class="sxs-lookup"><span data-stu-id="d9918-138">Collection of application settings used when running locally.</span></span> <span data-ttu-id="d9918-139">Přidejte objekt toothis nastavení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d9918-139">Add your application settings toothis object.</span></span>  |
| <span data-ttu-id="d9918-140">**AzureWebJobsStorage**</span><span class="sxs-lookup"><span data-stu-id="d9918-140">**AzureWebJobsStorage**</span></span> | <span data-ttu-id="d9918-141">Nastaví hello připojovací řetězec toohello účet úložiště Azure, který používá se interně pomocí modulu runtime Azure Functions hello.</span><span class="sxs-lookup"><span data-stu-id="d9918-141">Sets hello connection string toohello Azure Storage account that is used internally by hello Azure Functions runtime.</span></span> <span data-ttu-id="d9918-142">účet úložiště Hello podporuje vaše funkce aktivační události.</span><span class="sxs-lookup"><span data-stu-id="d9918-142">hello storage account supports your function's triggers.</span></span> <span data-ttu-id="d9918-143">Toto nastavení připojení účtu úložiště je vyžadována pro všechny funkce, s výjimkou funkce protokolu HTTP aktivované.</span><span class="sxs-lookup"><span data-stu-id="d9918-143">This storage account connection setting is required for all functions except for HTTP triggered functions.</span></span>  |
| <span data-ttu-id="d9918-144">**AzureWebJobsDashboard**</span><span class="sxs-lookup"><span data-stu-id="d9918-144">**AzureWebJobsDashboard**</span></span> | <span data-ttu-id="d9918-145">Nastaví hello připojení řetězec toohello Azure Storage účtu, který je použité toostore hello funkce protokoly.</span><span class="sxs-lookup"><span data-stu-id="d9918-145">Sets hello connection string toohello Azure Storage account that is used toostore hello function logs.</span></span> <span data-ttu-id="d9918-146">Tato volitelná hodnota zpřístupní hello protokoly portálu hello.</span><span class="sxs-lookup"><span data-stu-id="d9918-146">This optional value makes hello logs accessible in hello portal.</span></span>|
| <span data-ttu-id="d9918-147">**Hostitele**</span><span class="sxs-lookup"><span data-stu-id="d9918-147">**Host**</span></span> | <span data-ttu-id="d9918-148">Nastavení v této části přizpůsobit hello funkce hostitelský proces, při místním spuštění.</span><span class="sxs-lookup"><span data-stu-id="d9918-148">Settings in this section customize hello Functions host process when running locally.</span></span> | 
| <span data-ttu-id="d9918-149">**LocalHttpPort**</span><span class="sxs-lookup"><span data-stu-id="d9918-149">**LocalHttpPort**</span></span> | <span data-ttu-id="d9918-150">Nastaví hello výchozí port použitý při spuštění místního hostitele funkce hello (`func host start` a `func run`).</span><span class="sxs-lookup"><span data-stu-id="d9918-150">Sets hello default port used when running hello local Functions host (`func host start` and `func run`).</span></span> <span data-ttu-id="d9918-151">Hello `--port` možnost příkazového řádku má přednost před tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d9918-151">hello `--port` command-line option takes precedence over this value.</span></span> |
| <span data-ttu-id="d9918-152">**CORS**</span><span class="sxs-lookup"><span data-stu-id="d9918-152">**CORS**</span></span> | <span data-ttu-id="d9918-153">Definuje hello zdroje povolené pro [(CORS) pro sdílení prostředků různého původu](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="d9918-153">Defines hello origins allowed for [cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span> <span data-ttu-id="d9918-154">Zdroje se zadávají jako seznam oddělený čárkami bez mezer.</span><span class="sxs-lookup"><span data-stu-id="d9918-154">Origins are supplied as a comma-separated list with no spaces.</span></span> <span data-ttu-id="d9918-155">Hello hodnota zástupného znaku (**\***) je podporováno, což umožňuje požadavky od jakýkoli původ.</span><span class="sxs-lookup"><span data-stu-id="d9918-155">hello wildcard value (**\***) is supported, which allows requests from any origin.</span></span> |
| <span data-ttu-id="d9918-156">**ConnectionStrings**</span><span class="sxs-lookup"><span data-stu-id="d9918-156">**ConnectionStrings**</span></span> | <span data-ttu-id="d9918-157">Obsahuje hello databázové připojovací řetězce pro funkcí.</span><span class="sxs-lookup"><span data-stu-id="d9918-157">Contains hello database connection strings for your functions.</span></span> <span data-ttu-id="d9918-158">Připojovací řetězce v tento objekt se přidají toohello prostředí s typem zprostředkovatele hello **System.Data.SqlClient**.</span><span class="sxs-lookup"><span data-stu-id="d9918-158">Connection strings in this object are added toohello environment with hello provider type of **System.Data.SqlClient**.</span></span>  | 

<span data-ttu-id="d9918-159">Většina triggerů a vazeb mít **připojení** vlastnost, která mapuje toohello název nastavení aplikace nebo proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="d9918-159">Most triggers and bindings have a **Connection** property that maps toohello name of an environment variable or app setting.</span></span> <span data-ttu-id="d9918-160">Pro každou vlastnost připojení musí být definována v souboru local.settings.json nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="d9918-160">For each connection property, there must be app setting defined in local.settings.json file.</span></span> 

<span data-ttu-id="d9918-161">Tato nastavení lze také přečíst v kódu jako proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="d9918-161">These settings can also be read in your code as environment variables.</span></span> <span data-ttu-id="d9918-162">V jazyce C#, použijte [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) nebo [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="d9918-162">In C#, use [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) or [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx).</span></span> <span data-ttu-id="d9918-163">V jazyce JavaScript, použijte `process.env`.</span><span class="sxs-lookup"><span data-stu-id="d9918-163">In JavaScript, use `process.env`.</span></span> <span data-ttu-id="d9918-164">Bylo nastaveno jako systémové proměnné prostředí mají přednost před hodnot v souboru local.settings.json hello.</span><span class="sxs-lookup"><span data-stu-id="d9918-164">Settings specified as a system environment variable take precedence over values in hello local.settings.json file.</span></span> 

<span data-ttu-id="d9918-165">Nastavení v souboru local.settings.json hello používají funkce nástroje jenom, při místním spuštění.</span><span class="sxs-lookup"><span data-stu-id="d9918-165">Settings in hello local.settings.json file are only used by Functions tools when running locally.</span></span> <span data-ttu-id="d9918-166">Ve výchozím nastavení tato nastavení se nemigrují automaticky publikované tooAzure po hello projektu.</span><span class="sxs-lookup"><span data-stu-id="d9918-166">By default, these settings are not migrated automatically when hello project is published tooAzure.</span></span> <span data-ttu-id="d9918-167">Použití hello `--publish-local-settings` přepínač [při publikování](#publish) toomake, že tato nastavení jsou přidaný toohello funkce aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="d9918-167">Use hello `--publish-local-settings` switch [when you publish](#publish) toomake sure these settings are added toohello function app in Azure.</span></span>

<span data-ttu-id="d9918-168">Když je nastavená žádný platný úložiště připojovací řetězec pro **AzureWebJobsStorage**, se zobrazí následující chybová zpráva hello:</span><span class="sxs-lookup"><span data-stu-id="d9918-168">When no valid storage connection string is set for **AzureWebJobsStorage**, hello following error message is shown:</span></span>  

><span data-ttu-id="d9918-169">Chybí hodnota pro AzureWebJobsStorage v local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="d9918-169">Missing value for AzureWebJobsStorage in local.settings.json.</span></span> <span data-ttu-id="d9918-170">To je potřeba pro všechny aktivační události než HTTP.</span><span class="sxs-lookup"><span data-stu-id="d9918-170">This is required for all triggers other than HTTP.</span></span> <span data-ttu-id="d9918-171">Můžete spustit ' func azure functionary načítání app nastavení <functionAppName>, nebo zadejte připojovací řetězec v local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="d9918-171">You can run 'func azure functionary fetch-app-settings <functionAppName>' or specify a connection string in local.settings.json.</span></span>
  
[!INCLUDE [Note toonot use local storage](../../includes/functions-local-settings-note.md)]

### <a name="configure-app-settings"></a><span data-ttu-id="d9918-172">Konfigurace nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="d9918-172">Configure app settings</span></span>

<span data-ttu-id="d9918-173">tooset hodnotu pro připojovací řetězce, můžete provést jednu z následujících akcí hello:</span><span class="sxs-lookup"><span data-stu-id="d9918-173">tooset a value for connection strings, you can do one of hello following:</span></span>
* <span data-ttu-id="d9918-174">Zadejte připojovací řetězec hello z [Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="d9918-174">Enter hello connection string from [Azure Storage Explorer](http://storageexplorer.com/).</span></span>
* <span data-ttu-id="d9918-175">Použijte jednu z hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d9918-175">Use one of hello following commands:</span></span>

    ```
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    ```
    func azure functionapp storage fetch-connection-string <StorageAccountName>
    ```
    <span data-ttu-id="d9918-176">Oba příkazy vyžadují toofirst přihlášení je tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d9918-176">Both commands require you toofirst sign-in tooAzure.</span></span>

## <a name="create-a-function"></a><span data-ttu-id="d9918-177">Vytvoření funkce</span><span class="sxs-lookup"><span data-stu-id="d9918-177">Create a function</span></span>

<span data-ttu-id="d9918-178">toocreate funkci, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d9918-178">toocreate a function, run hello following command:</span></span>

```
func new
``` 
<span data-ttu-id="d9918-179">`func new`podporuje následující volitelné argumenty hello:</span><span class="sxs-lookup"><span data-stu-id="d9918-179">`func new` supports hello following optional arguments:</span></span>

| <span data-ttu-id="d9918-180">Argument</span><span class="sxs-lookup"><span data-stu-id="d9918-180">Argument</span></span>     | <span data-ttu-id="d9918-181">Popis</span><span class="sxs-lookup"><span data-stu-id="d9918-181">Description</span></span>                            |
| ------------ | -------------------------------------- |
| **`--language -l`** | <span data-ttu-id="d9918-182">Šablona Hello programovací jazyk, například C#, F # nebo JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d9918-182">hello template programming language, such as C#, F#, or JavaScript.</span></span> |
| **`--template -t`** | <span data-ttu-id="d9918-183">Název šablony Hello.</span><span class="sxs-lookup"><span data-stu-id="d9918-183">hello template name.</span></span> |
| **`--name -n`** | <span data-ttu-id="d9918-184">název funkce Hello.</span><span class="sxs-lookup"><span data-stu-id="d9918-184">hello function name.</span></span> |

<span data-ttu-id="d9918-185">Například toocreate aktivační událost jazyka JavaScript HTTP, spusťte:</span><span class="sxs-lookup"><span data-stu-id="d9918-185">For example, toocreate a JavaScript HTTP trigger, run:</span></span>

```
func new --language JavaScript --template HttpTrigger --name MyHttpTrigger
```

<span data-ttu-id="d9918-186">toocreate funkce aktivované fronty, spusťte:</span><span class="sxs-lookup"><span data-stu-id="d9918-186">toocreate a queue-triggered function, run:</span></span>

```
func new --language JavaScript --template QueueTrigger --name QueueTriggerJS
```

## <a name="run-functions-locally"></a><span data-ttu-id="d9918-187">Místní spuštění funkce</span><span class="sxs-lookup"><span data-stu-id="d9918-187">Run functions locally</span></span>

<span data-ttu-id="d9918-188">toorun funkce projektu, spusťte hello funkce hostitelů.</span><span class="sxs-lookup"><span data-stu-id="d9918-188">toorun a Functions project, run hello Functions host.</span></span> <span data-ttu-id="d9918-189">Hello hostitele umožňuje aktivačních událostí pro všechny funkce v projektu hello:</span><span class="sxs-lookup"><span data-stu-id="d9918-189">hello host enables triggers for all functions in hello project:</span></span>

```
func host start
```

<span data-ttu-id="d9918-190">`func host start`hello podporuje následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="d9918-190">`func host start` supports hello following options:</span></span>

| <span data-ttu-id="d9918-191">Možnost</span><span class="sxs-lookup"><span data-stu-id="d9918-191">Option</span></span>     | <span data-ttu-id="d9918-192">Popis</span><span class="sxs-lookup"><span data-stu-id="d9918-192">Description</span></span>                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | <span data-ttu-id="d9918-193">Hello toolisten místního portu na.</span><span class="sxs-lookup"><span data-stu-id="d9918-193">hello local port toolisten on.</span></span> <span data-ttu-id="d9918-194">Výchozí hodnota: 7071.</span><span class="sxs-lookup"><span data-stu-id="d9918-194">Default value: 7071.</span></span> |
| **`--debug <type>`** | <span data-ttu-id="d9918-195">Možnosti Hello jsou `VSCode` a `VS`.</span><span class="sxs-lookup"><span data-stu-id="d9918-195">hello options are `VSCode` and `VS`.</span></span> |
| **`--cors`** | <span data-ttu-id="d9918-196">Seznam původů CORS, bez mezer oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="d9918-196">A comma-separated list of CORS origins, with no spaces.</span></span> |
| **`--nodeDebugPort -n`** | <span data-ttu-id="d9918-197">Hello port pro toouse ladicího programu hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="d9918-197">hello port for hello node debugger toouse.</span></span> <span data-ttu-id="d9918-198">Výchozí hodnota: Hodnota z launch.json nebo 5858.</span><span class="sxs-lookup"><span data-stu-id="d9918-198">Default: A value from launch.json or 5858.</span></span> |
| **`--debugLevel -d`** | <span data-ttu-id="d9918-199">úroveň trasování Hello konzoly (vypnuto, podrobné nastavení, informace, varování nebo chyba).</span><span class="sxs-lookup"><span data-stu-id="d9918-199">hello console trace level (off, verbose, info, warning, or error).</span></span> <span data-ttu-id="d9918-200">Výchozí: informace.</span><span class="sxs-lookup"><span data-stu-id="d9918-200">Default: Info.</span></span>|
| **`--timeout -t`** | <span data-ttu-id="d9918-201">Hello vypršení časového limitu pro hello funkce hostitele t o spuštění, v sekundách.</span><span class="sxs-lookup"><span data-stu-id="d9918-201">hello time out for hello Functions host t     o start, in seconds.</span></span> <span data-ttu-id="d9918-202">Výchozí hodnota: 20 sekund.</span><span class="sxs-lookup"><span data-stu-id="d9918-202">Default: 20 seconds.</span></span>|
| **`--useHttps`** | <span data-ttu-id="d9918-203">Vytvoření vazby toohttps://localhost: {port} místo toohttp://localhost: {port}.</span><span class="sxs-lookup"><span data-stu-id="d9918-203">Bind toohttps://localhost:{port} rather than toohttp://localhost:{port}.</span></span> <span data-ttu-id="d9918-204">Ve výchozím nastavení tato možnost vytvoří důvěryhodný certifikát v počítači.</span><span class="sxs-lookup"><span data-stu-id="d9918-204">By default, this option creates a trusted certificate on your computer.</span></span>|
| **`--pause-on-error`** | <span data-ttu-id="d9918-205">Pozastavení další vstupní před ukončením hello procesu.</span><span class="sxs-lookup"><span data-stu-id="d9918-205">Pause for additional input before exiting hello process.</span></span> <span data-ttu-id="d9918-206">Užitečné při spuštění nástroje základní funkce Azure z integrované vývojové prostředí (IDE).</span><span class="sxs-lookup"><span data-stu-id="d9918-206">Useful when launching Azure Functions Core Tools from an integrated development environment (IDE).</span></span>|

<span data-ttu-id="d9918-207">Při spuštění hostitelů funkce hello výstupy funkce aktivované protokolem URL HTTP hello:</span><span class="sxs-lookup"><span data-stu-id="d9918-207">When hello Functions host starts, it outputs hello URL of HTTP-triggered functions:</span></span>

```
Found hello following functions:
Host.Functions.MyHttpTrigger

ob host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="debug-in-vs-code-or-visual-studio"></a><span data-ttu-id="d9918-208">Ladění ve VS Code nebo Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d9918-208">Debug in VS Code or Visual Studio</span></span>

<span data-ttu-id="d9918-209">ladicí program, tooattach předat hello `--debug` argument.</span><span class="sxs-lookup"><span data-stu-id="d9918-209">tooattach a debugger, pass hello `--debug` argument.</span></span> <span data-ttu-id="d9918-210">funkce jazyka JavaScript toodebug, použijte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d9918-210">toodebug JavaScript functions, use Visual Studio Code.</span></span> <span data-ttu-id="d9918-211">Pro C# funkce použijte sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9918-211">For C# functions, use Visual Studio.</span></span>

<span data-ttu-id="d9918-212">pomocí funkce toodebug C#, `--debug vs`.</span><span class="sxs-lookup"><span data-stu-id="d9918-212">toodebug C# functions, use `--debug vs`.</span></span> <span data-ttu-id="d9918-213">Můžete také použít [Azure funkce 2017 nástroje sady Visual Studio](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span><span class="sxs-lookup"><span data-stu-id="d9918-213">You can also use [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span></span> 

<span data-ttu-id="d9918-214">toolaunch hello hostitelů a nastavení ladění jazyka JavaScript, spusťte:</span><span class="sxs-lookup"><span data-stu-id="d9918-214">toolaunch hello host and set up JavaScript debugging, run:</span></span>

```
func host start --debug vscode
```

<span data-ttu-id="d9918-215">Potom můžete v sadě Visual Studio Code hello **ladění** zobrazit, vyberte možnost **připojte tooAzure funkce**.</span><span class="sxs-lookup"><span data-stu-id="d9918-215">Then, in Visual Studio Code, in hello **Debug** view, select **Attach tooAzure Functions**.</span></span> <span data-ttu-id="d9918-216">Můžete připojit zarážky, zkontrolujte proměnné a krok prostřednictvím kódu.</span><span class="sxs-lookup"><span data-stu-id="d9918-216">You can attach breakpoints, inspect variables, and step through code.</span></span>

![Ladění jazyka JavaScript s kódem jazyka Visual Studio](./media/functions-run-local/vscode-javascript-debugging.png)

### <a name="passing-test-data-tooa-function"></a><span data-ttu-id="d9918-218">Předávání testů data tooa funkce</span><span class="sxs-lookup"><span data-stu-id="d9918-218">Passing test data tooa function</span></span>

<span data-ttu-id="d9918-219">Můžete také vyvolat funkci přímo pomocí `func run <FunctionName>` a zadejte vstupní data pro funkce hello.</span><span class="sxs-lookup"><span data-stu-id="d9918-219">You can also invoke a function directly by using `func run <FunctionName>` and provide input data for hello function.</span></span> <span data-ttu-id="d9918-220">Tento příkaz je podobné toorunning funkce pomocí hello **Test** ve hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d9918-220">This command is similar toorunning a function using hello **Test** tab in hello Azure portal.</span></span> <span data-ttu-id="d9918-221">Tento příkaz spustí hello celý funkce hostitele.</span><span class="sxs-lookup"><span data-stu-id="d9918-221">This command launches hello entire Functions host.</span></span>

<span data-ttu-id="d9918-222">`func run`hello podporuje následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="d9918-222">`func run` supports hello following options:</span></span>

| <span data-ttu-id="d9918-223">Možnost</span><span class="sxs-lookup"><span data-stu-id="d9918-223">Option</span></span>     | <span data-ttu-id="d9918-224">Popis</span><span class="sxs-lookup"><span data-stu-id="d9918-224">Description</span></span>                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | <span data-ttu-id="d9918-225">Vložený obsah.</span><span class="sxs-lookup"><span data-stu-id="d9918-225">Inline content.</span></span> |
| **`--debug -d`** | <span data-ttu-id="d9918-226">Připojte ladicí toohello hostitelský proces před spuštěním funkce hello.</span><span class="sxs-lookup"><span data-stu-id="d9918-226">Attach a debugger toohello host process before running hello function.</span></span>|
| **`--timeout -t`** | <span data-ttu-id="d9918-227">Toowait čas (v sekundách) až do místního hostitele funkce hello je připraven.</span><span class="sxs-lookup"><span data-stu-id="d9918-227">Time toowait (in seconds) until hello local Functions host is ready.</span></span>|
| **`--file -f`** | <span data-ttu-id="d9918-228">Hello toouse název souboru jako obsah.</span><span class="sxs-lookup"><span data-stu-id="d9918-228">hello file name toouse as content.</span></span>|
| **`--no-interactive`** | <span data-ttu-id="d9918-229">Nezobrazí výzvu pro vstup.</span><span class="sxs-lookup"><span data-stu-id="d9918-229">Does not prompt for input.</span></span> <span data-ttu-id="d9918-230">Tato možnost je užitečná pro scénáře automatizace.</span><span class="sxs-lookup"><span data-stu-id="d9918-230">Useful for automation scenarios.</span></span>|

<span data-ttu-id="d9918-231">Například toocall funkce aktivované protokolem HTTP a text obsahu pass, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d9918-231">For example, toocall an HTTP-triggered function and pass content body, run hello following command:</span></span>

```
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

## <span data-ttu-id="d9918-232"><a name="publish"></a>Publikování tooAzure</span><span class="sxs-lookup"><span data-stu-id="d9918-232"><a name="publish"></a>Publish tooAzure</span></span>

<span data-ttu-id="d9918-233">toopublish aplikaci funkce projektu tooa funkce v Azure, použijte hello `publish` příkaz:</span><span class="sxs-lookup"><span data-stu-id="d9918-233">toopublish a Functions project tooa function app in Azure, use hello `publish` command:</span></span>

```
func azure functionapp publish <FunctionAppName>
```

<span data-ttu-id="d9918-234">Můžete použít hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="d9918-234">You can use hello following options:</span></span>

| <span data-ttu-id="d9918-235">Možnost</span><span class="sxs-lookup"><span data-stu-id="d9918-235">Option</span></span>     | <span data-ttu-id="d9918-236">Popis</span><span class="sxs-lookup"><span data-stu-id="d9918-236">Description</span></span>                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  <span data-ttu-id="d9918-237">Nastavení publikování v local.settings.json tooAzure, výzvy toooverwrite Pokud hello nastavení již existuje.</span><span class="sxs-lookup"><span data-stu-id="d9918-237">Publish settings in local.settings.json tooAzure, prompting toooverwrite if hello setting already exists.</span></span>|
| **`--overwrite-settings -y`** | <span data-ttu-id="d9918-238">Musí být použit s `-i`.</span><span class="sxs-lookup"><span data-stu-id="d9918-238">Must be used with `-i`.</span></span> <span data-ttu-id="d9918-239">Pokud se liší, přepíše místní hodnotou AppSettings v Azure.</span><span class="sxs-lookup"><span data-stu-id="d9918-239">Overwrites AppSettings in Azure with local value if different.</span></span> <span data-ttu-id="d9918-240">Výchozí hodnota je výzva.</span><span class="sxs-lookup"><span data-stu-id="d9918-240">Default is prompt.</span></span>|

<span data-ttu-id="d9918-241">Hello `publish` příkaz odešle hello obsah adresáře projektu funkce hello.</span><span class="sxs-lookup"><span data-stu-id="d9918-241">hello `publish` command uploads hello contents of hello Functions project directory.</span></span> <span data-ttu-id="d9918-242">Pokud odstraníte soubory lokálně, hello `publish` příkaz nedojde k jejich odstranění z Azure.</span><span class="sxs-lookup"><span data-stu-id="d9918-242">If you delete files locally, hello `publish` command does not delete them from Azure.</span></span> <span data-ttu-id="d9918-243">Můžete odstranit soubory v Azure pomocí hello [Kudu nástroj](functions-how-to-use-azure-function-app-settings.md#kudu) v hello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="d9918-243">You can delete files in Azure by using hello [Kudu tool](functions-how-to-use-azure-function-app-settings.md#kudu) in hello [Azure portal].</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9918-244">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9918-244">Next steps</span></span>

<span data-ttu-id="d9918-245">Nástroje Azure základní funkce je [otevřít zdroj a hostované na Githubu](https://github.com/azure/azure-functions-cli).</span><span class="sxs-lookup"><span data-stu-id="d9918-245">Azure Functions Core Tools is [open source and hosted on GitHub](https://github.com/azure/azure-functions-cli).</span></span>  
<span data-ttu-id="d9918-246">toofile žádost chyb nebo funkce [otevřete potíže Githubu](https://github.com/azure/azure-functions-cli/issues).</span><span class="sxs-lookup"><span data-stu-id="d9918-246">toofile a bug or feature request, [open a GitHub issue](https://github.com/azure/azure-functions-cli/issues).</span></span> 

<!-- LINKS -->

[nástroje základní funkce Azure]: https://www.npmjs.com/package/azure-functions-core-tools
[portál Azure]: https://portal.azure.com 
