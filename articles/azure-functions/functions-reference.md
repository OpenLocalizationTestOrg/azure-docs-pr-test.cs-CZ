---
title: "Pokyny pro vývoj Azure Functions | Microsoft Docs"
description: "Další koncepty Azure Functions a techniky, které potřebujete k vývoji funkce v Azure, ve všech programovací jazyky a vazeb."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "vývojáře průvodce azure funkce, funkce, událostí zpracování, webhooků, dynamické výpočetní, bez serveru architektura"
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: chrande
ms.openlocfilehash: 879be48150cfe13e31064475aa637f13f5f5f9d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-developers-guide"></a><span data-ttu-id="a192b-104">Příručka pro vývojáře Azure funkce</span><span class="sxs-lookup"><span data-stu-id="a192b-104">Azure Functions developers guide</span></span>
<span data-ttu-id="a192b-105">V Azure Functions se konkrétní funkce sdílet několik klíčových technických konceptech a součásti, bez ohledu na jazyk nebo vazby, které používáte.</span><span class="sxs-lookup"><span data-stu-id="a192b-105">In Azure Functions, specific functions share a few core technical concepts and components, regardless of the language or binding you use.</span></span> <span data-ttu-id="a192b-106">Před přechodem do učení podrobnosti, které jsou specifické pro daný jazyk nebo vazby, nezapomeňte si přečíst tento přehled, který se vztahuje na všechny z nich.</span><span class="sxs-lookup"><span data-stu-id="a192b-106">Before you jump into learning details specific to a given language or binding, be sure to read through this overview that applies to all of them.</span></span>

<span data-ttu-id="a192b-107">Tento článek předpokládá, že jste si již přečetli [přehled Azure Functions](functions-overview.md) a jsou obeznámeni s [WebJobs SDK koncepty, jako jsou aktivační události, vazby a JobHost runtime](../app-service-web/websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="a192b-107">This article assumes that you've already read the [Azure Functions overview](functions-overview.md) and are familiar with [WebJobs SDK concepts such as triggers, bindings, and the JobHost runtime](../app-service-web/websites-dotnet-webjobs-sdk.md).</span></span> <span data-ttu-id="a192b-108">Azure Functions je založena na WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="a192b-108">Azure Functions is based on the WebJobs SDK.</span></span> 

## <a name="function-code"></a><span data-ttu-id="a192b-109">Kód – funkce</span><span class="sxs-lookup"><span data-stu-id="a192b-109">Function code</span></span>
<span data-ttu-id="a192b-110">A *funkce* je primární koncept v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="a192b-110">A *function* is the primary concept in Azure Functions.</span></span> <span data-ttu-id="a192b-111">Psaní kódu pro funkce v jazyce podle vaší volby a uložte kódu a konfiguračních souborů ve stejné složce.</span><span class="sxs-lookup"><span data-stu-id="a192b-111">You write code for a function in a language of your choice and save the code and configuration files in the same folder.</span></span> <span data-ttu-id="a192b-112">Konfigurace má název `function.json`, která obsahuje konfigurační data JSON.</span><span class="sxs-lookup"><span data-stu-id="a192b-112">The configuration is named `function.json`, which contains JSON configuration data.</span></span> <span data-ttu-id="a192b-113">Podporuje různé jazyky a každé z nich má mírně odlišné prostředí optimalizován pro daného jazyka.</span><span class="sxs-lookup"><span data-stu-id="a192b-113">Various languages are supported, and each one has a slightly different experience optimized to work best for that language.</span></span> 

<span data-ttu-id="a192b-114">Soubor function.json definuje vazby funkcí a dalších nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a192b-114">The function.json file defines the function bindings and other configuration settings.</span></span> <span data-ttu-id="a192b-115">Modul runtime používá tento soubor k určení události k monitorování a jak předat data do a ze spuštění funkce vrátit data.</span><span class="sxs-lookup"><span data-stu-id="a192b-115">The runtime uses this file to determine the events to monitor and how to pass data into and return data from function execution.</span></span> <span data-ttu-id="a192b-116">Následující příklad je function.json soubor.</span><span class="sxs-lookup"><span data-stu-id="a192b-116">The following is an example function.json file.</span></span>

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

<span data-ttu-id="a192b-117">Nastavte `disabled` vlastnost `true` zabránit funkci spouštěna.</span><span class="sxs-lookup"><span data-stu-id="a192b-117">Set the `disabled` property to `true` to prevent the function from being executed.</span></span>

<span data-ttu-id="a192b-118">`bindings` Vlastnost je, kde můžete konfigurovat triggerů a vazeb.</span><span class="sxs-lookup"><span data-stu-id="a192b-118">The `bindings` property is where you configure both triggers and bindings.</span></span> <span data-ttu-id="a192b-119">Každou vazbu sdílí několik běžných nastavení a některá nastavení, které jsou specifické pro konkrétní typ vazby.</span><span class="sxs-lookup"><span data-stu-id="a192b-119">Each binding shares a few common settings and some settings, which are specific to a particular type of binding.</span></span> <span data-ttu-id="a192b-120">Každé vazby vyžaduje následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="a192b-120">Every binding requires the following settings:</span></span>

| <span data-ttu-id="a192b-121">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="a192b-121">Property</span></span> | <span data-ttu-id="a192b-122">Hodnoty nebo typy</span><span class="sxs-lookup"><span data-stu-id="a192b-122">Values/Types</span></span> | <span data-ttu-id="a192b-123">Komentáře</span><span class="sxs-lookup"><span data-stu-id="a192b-123">Comments</span></span> |
| --- | --- | --- |
| `type` |<span data-ttu-id="a192b-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a192b-124">string</span></span> |<span data-ttu-id="a192b-125">Typ vazby.</span><span class="sxs-lookup"><span data-stu-id="a192b-125">Binding type.</span></span> <span data-ttu-id="a192b-126">Například, `queueTrigger`.</span><span class="sxs-lookup"><span data-stu-id="a192b-126">For example, `queueTrigger`.</span></span> |
| `direction` |<span data-ttu-id="a192b-127">v out</span><span class="sxs-lookup"><span data-stu-id="a192b-127">'in', 'out'</span></span> |<span data-ttu-id="a192b-128">Určuje, zda vazby pro příjem dat do funkce nebo odeslání dat z funkce.</span><span class="sxs-lookup"><span data-stu-id="a192b-128">Indicates whether the binding is for receiving data into the function or sending data from the function.</span></span> |
| `name` |<span data-ttu-id="a192b-129">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a192b-129">string</span></span> |<span data-ttu-id="a192b-130">Název, který se používá pro vázaných dat ve funkci.</span><span class="sxs-lookup"><span data-stu-id="a192b-130">The name that is used for the bound data in the function.</span></span> <span data-ttu-id="a192b-131">Pro jazyk C# to je název argumentu; pro jazyk JavaScript je klíče v seznamu klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="a192b-131">For C#, this is an argument name; for JavaScript, it's the key in a key/value list.</span></span> |

## <a name="function-app"></a><span data-ttu-id="a192b-132">Funkce aplikace</span><span class="sxs-lookup"><span data-stu-id="a192b-132">Function app</span></span>
<span data-ttu-id="a192b-133">Funkce aplikace se skládá z jedné nebo více jednotlivých funkcí, které se spravují dohromady službou Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="a192b-133">A function app is comprised of one or more individual functions that are managed together by Azure App Service.</span></span> <span data-ttu-id="a192b-134">Všechny funkce v aplikaci funkce sdílet stejnou cenový plán, průběžné nasazování a verze modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="a192b-134">All of the functions in a function app share the same pricing plan, continuous deployment and runtime version.</span></span> <span data-ttu-id="a192b-135">Všechny funkce, které jsou napsané v různých jazycích mohou sdílet stejné aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="a192b-135">Functions written in multiple languages can all share the same function app.</span></span> <span data-ttu-id="a192b-136">Funkce aplikace si můžete představit jako způsob, jak uspořádat a souhrnně spravovat funkcí.</span><span class="sxs-lookup"><span data-stu-id="a192b-136">Think of a function app as a way to organize and collectively manage your functions.</span></span> 

## <a name="runtime-script-host-and-web-host"></a><span data-ttu-id="a192b-137">Modul runtime (hostitel skriptu a webového hostitele)</span><span class="sxs-lookup"><span data-stu-id="a192b-137">Runtime (script host and web host)</span></span>
<span data-ttu-id="a192b-138">Modul runtime nebo hostitel skriptu, je základní sada WebJobs SDK hostitele, který naslouchá událostem, shromažďuje a odesílá data a nakonec spustí váš kód.</span><span class="sxs-lookup"><span data-stu-id="a192b-138">The runtime, or script host, is the underlying WebJobs SDK host that listens for events, gathers and sends data, and ultimately runs your code.</span></span> 

<span data-ttu-id="a192b-139">Pro usnadnění aktivace protokolu HTTP, je zde také webového hostitele, který je navržený pro sledovat na hostitelský modul pro skripty v produkčních scénářích.</span><span class="sxs-lookup"><span data-stu-id="a192b-139">To facilitate HTTP triggers, there is also a web host that is designed to sit in front of the script host in production scenarios.</span></span> <span data-ttu-id="a192b-140">Má dva hostitele pomáhá izolovat hostitelský modul pro skripty z před ukončení provoz spravuje webového hostitele.</span><span class="sxs-lookup"><span data-stu-id="a192b-140">Having two hosts helps to isolate the script host from the front end traffic managed by the web host.</span></span>

## <a name="folder-structure"></a><span data-ttu-id="a192b-141">Struktura složek</span><span class="sxs-lookup"><span data-stu-id="a192b-141">Folder Structure</span></span>
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

<span data-ttu-id="a192b-142">Při vytváření projektu pro nasazení funkce do aplikaci funkce ve službě Azure App Service, můžete tuto strukturu považovat kódu lokality.</span><span class="sxs-lookup"><span data-stu-id="a192b-142">When setting-up a project for deploying functions to a function app in Azure App Service, you can treat this folder structure as your site code.</span></span> <span data-ttu-id="a192b-143">Můžete použít stávající nástroje, například průběžnou integraci a nasazení, nebo vlastní nasazení skriptů pro tuto činnost čas instalace balíčku nasazení nebo code transpilation.</span><span class="sxs-lookup"><span data-stu-id="a192b-143">You can use existing tools like continuous integration and deployment, or custom deployment scripts for doing deploy time package installation or code transpilation.</span></span>

> [!NOTE]
> <span data-ttu-id="a192b-144">Zajistěte, aby k nasazení vaší `host.json` souborů a složek přímo na fungovat `wwwroot` složky.</span><span class="sxs-lookup"><span data-stu-id="a192b-144">Make sure to deploy your `host.json` file and function folders directly to the `wwwroot` folder.</span></span> <span data-ttu-id="a192b-145">Nezahrnovat `wwwroot` složky ve vašem nasazení.</span><span class="sxs-lookup"><span data-stu-id="a192b-145">Do not include the `wwwroot` folder in your deployments.</span></span> <span data-ttu-id="a192b-146">Jinak, v níž se `wwwroot\wwwroot` složek.</span><span class="sxs-lookup"><span data-stu-id="a192b-146">Otherwise, you end up with `wwwroot\wwwroot` folders.</span></span> 
> 
> 

## <span data-ttu-id="a192b-147"><a id="fileupdate"></a>Postup aktualizace soubory aplikace – funkce</span><span class="sxs-lookup"><span data-stu-id="a192b-147"><a id="fileupdate"></a> How to update function app files</span></span>
<span data-ttu-id="a192b-148">Editor funkce integrovaná v portálu Azure vám umožní aktualizovat *function.json* soubor a soubor kódu pro funkci.</span><span class="sxs-lookup"><span data-stu-id="a192b-148">The function editor built into the Azure portal lets you update the *function.json* file and the code file for a function.</span></span> <span data-ttu-id="a192b-149">Nahrát nebo aktualizovat ostatní soubory, jako *package.json* nebo *project.json* nebo závislosti, budete muset použít jiné metody nasazení.</span><span class="sxs-lookup"><span data-stu-id="a192b-149">To upload or update other files such as *package.json* or *project.json* or dependencies, you have to use other deployment methods.</span></span>

<span data-ttu-id="a192b-150">Funkce aplikace jsou postaveny na služby App Service, takže všechny [možnosti nasazení standardní webových aplikací](../app-service-web/web-sites-deploy.md) jsou také k dispozici pro funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="a192b-150">Function apps are built on App Service, so all the [deployment options available to standard web apps](../app-service-web/web-sites-deploy.md) are also available for function apps.</span></span> <span data-ttu-id="a192b-151">Tady jsou některé metody, které můžete nahrát, nebo můžete aktualizovat soubory funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="a192b-151">Here are some methods you can use to upload or update function app files.</span></span> 

#### <a name="to-use-app-service-editor"></a><span data-ttu-id="a192b-152">Použití editoru služby aplikace</span><span class="sxs-lookup"><span data-stu-id="a192b-152">To use App Service Editor</span></span>
1. <span data-ttu-id="a192b-153">Na portálu Azure Functions, klikněte na tlačítko **funkce nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a192b-153">In the Azure Functions portal, click **Function app settings**.</span></span>
2. <span data-ttu-id="a192b-154">V **Upřesnit nastavení** klikněte na tlačítko **přejděte na nastavení služby aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a192b-154">In the **Advanced Settings** section, click **Go to App Service Settings**.</span></span>
3. <span data-ttu-id="a192b-155">Klikněte na tlačítko **aplikace služby Editor** v nabídce aplikace navigaci v části **nástroje pro vývoj**.</span><span class="sxs-lookup"><span data-stu-id="a192b-155">Click **App Service Editor** in App Menu Nav under **DEVELOPMENT TOOLS**.</span></span>
4. <span data-ttu-id="a192b-156">Klikněte na tlačítko **přejděte**.</span><span class="sxs-lookup"><span data-stu-id="a192b-156">click **Go**.</span></span>
   
   <span data-ttu-id="a192b-157">Po načtení Editor služby aplikace se zobrazí *host.json* složek souborů a funkce v části *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="a192b-157">After App Service Editor loads, you'll see the *host.json* file and function folders under *wwwroot*.</span></span> 
5. <span data-ttu-id="a192b-158">Otevřít soubory, upravit nebo přetáhnout z vývojovém počítači k nahrání souborů.</span><span class="sxs-lookup"><span data-stu-id="a192b-158">Open files to edit them, or drag and drop from your development machine to upload files.</span></span>

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a><span data-ttu-id="a192b-159">Použít aplikaci funkce endpoint SCM (Kudu)</span><span class="sxs-lookup"><span data-stu-id="a192b-159">To use the function app's SCM (Kudu) endpoint</span></span>
1. <span data-ttu-id="a192b-160">Přejděte do: `https://<function_app_name>.scm.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="a192b-160">Navigate to: `https://<function_app_name>.scm.azurewebsites.net`.</span></span>
2. <span data-ttu-id="a192b-161">Klikněte na tlačítko **ladění konzoly > CMD**.</span><span class="sxs-lookup"><span data-stu-id="a192b-161">Click **Debug Console > CMD**.</span></span>
3. <span data-ttu-id="a192b-162">Přejděte na `D:\home\site\wwwroot\` aktualizace *host.json* nebo `D:\home\site\wwwroot\<function_name>` k aktualizaci souborů funkce.</span><span class="sxs-lookup"><span data-stu-id="a192b-162">Navigate to `D:\home\site\wwwroot\` to update *host.json* or `D:\home\site\wwwroot\<function_name>` to update a function's files.</span></span>
4. <span data-ttu-id="a192b-163">A přetáhněte soubor, který chcete nahrát do příslušné složky v mřížce souboru.</span><span class="sxs-lookup"><span data-stu-id="a192b-163">Drag-and-drop a file you want to upload into the appropriate folder in the file grid.</span></span> <span data-ttu-id="a192b-164">Existují dvě oblasti v mřížce souboru, kde můžete vložit do souboru.</span><span class="sxs-lookup"><span data-stu-id="a192b-164">There are two areas in the file grid where you can drop a file.</span></span> <span data-ttu-id="a192b-165">Pro *.zip* soubory, zobrazí se pole s popiskem "Přetáhněte sem odeslání a rozbalte."</span><span class="sxs-lookup"><span data-stu-id="a192b-165">For *.zip* files, a box appears with the label "Drag here to upload and unzip."</span></span> <span data-ttu-id="a192b-166">Pro ostatní typy souborů vyřaďte v mřížce souboru, ale mimo pole "rozbalte".</span><span class="sxs-lookup"><span data-stu-id="a192b-166">For other file types, drop in the file grid but outside the "unzip" box.</span></span>

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --DonnaM -->

#### <a name="to-use-continuous-deployment"></a><span data-ttu-id="a192b-167">Chcete-li použít průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="a192b-167">To use continuous deployment</span></span>
<span data-ttu-id="a192b-168">Postupujte podle pokynů v tématu [průběžné nasazování pro Azure Functions](functions-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="a192b-168">Follow the instructions in the topic [Continuous deployment for Azure Functions](functions-continuous-deployment.md).</span></span>

## <a name="parallel-execution"></a><span data-ttu-id="a192b-169">Paralelní provádění</span><span class="sxs-lookup"><span data-stu-id="a192b-169">Parallel execution</span></span>
<span data-ttu-id="a192b-170">Dojde-li více spouštěcí událostí rychleji, než funkce jednovláknové runtime dokáže zpracovat, může modul runtime vyvolají funkci vícekrát paralelně.</span><span class="sxs-lookup"><span data-stu-id="a192b-170">When multiple triggering events occur faster than a single-threaded function runtime can process them, the runtime may invoke the function multiple times in parallel.</span></span>  <span data-ttu-id="a192b-171">Pokud funkce aplikace používá [spotřeba hostování plán](functions-scale.md#how-the-consumption-plan-works), může automaticky škálovat aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="a192b-171">If a function app is using the [Consumption hosting plan](functions-scale.md#how-the-consumption-plan-works), the function app could scale out automatically.</span></span>  <span data-ttu-id="a192b-172">Každá instance funkce aplikace, jestli aplikace běží na spotřebu hostování plán nebo běžný [hostování plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), může zpracovat volání souběžných funkce paralelně pomocí více vláken.</span><span class="sxs-lookup"><span data-stu-id="a192b-172">Each instance of the function app, whether the app runs on the Consumption hosting plan or a regular [App Service hosting plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), might process concurrent function invocations in parallel using multiple threads.</span></span>  <span data-ttu-id="a192b-173">Maximální počet souběžných funkce volání v každé instanci aplikace funkce se liší podle typu používá aktivační událost, jakož i prostředky využívané třídou jiných funkcí v rámci funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="a192b-173">The maximum number of concurrent function invocations in each function app instance varies based on the type of trigger being used as well as the resources used by other functions within the function app.</span></span>

## <a name="functions-runtime-versioning"></a><span data-ttu-id="a192b-174">Verze runtime funkce</span><span class="sxs-lookup"><span data-stu-id="a192b-174">Functions runtime versioning</span></span>

<span data-ttu-id="a192b-175">Můžete nakonfigurovat verzi modulu runtime její funkce pomocí `FUNCTIONS_EXTENSION_VERSION` nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="a192b-175">You can configure the version of the Functions runtime using the `FUNCTIONS_EXTENSION_VERSION` app setting.</span></span> <span data-ttu-id="a192b-176">Například hodnota "~ 1" označuje, že funkce aplikace bude používat 1 jako jeho hlavní verzi.</span><span class="sxs-lookup"><span data-stu-id="a192b-176">For example, the value "~1" indicates that your Function App will use 1 as its major version.</span></span> <span data-ttu-id="a192b-177">Funkce aplikace upgradují na každý nový podverze při jejich vydání.</span><span class="sxs-lookup"><span data-stu-id="a192b-177">Function Apps are upgraded to each new minor version as they are released.</span></span> <span data-ttu-id="a192b-178">Můžete zobrazit přesný verze funkce aplikace v rámci **nastavení** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a192b-178">You can view the exact version of your Function App in the **Settings** tab in the Azure Portal.</span></span>

## <a name="repositories"></a><span data-ttu-id="a192b-179">Úložiště</span><span class="sxs-lookup"><span data-stu-id="a192b-179">Repositories</span></span>
<span data-ttu-id="a192b-180">Kód pro Azure Functions je open source a uložené v úložišť GitHub:</span><span class="sxs-lookup"><span data-stu-id="a192b-180">The code for Azure Functions is open source and stored in GitHub repositories:</span></span>

* [<span data-ttu-id="a192b-181">Azure Functions runtime</span><span class="sxs-lookup"><span data-stu-id="a192b-181">Azure Functions runtime</span></span>](https://github.com/Azure/azure-webjobs-sdk-script/)
* [<span data-ttu-id="a192b-182">Azure Functions na portálu</span><span class="sxs-lookup"><span data-stu-id="a192b-182">Azure Functions portal</span></span>](https://github.com/projectkudu/AzureFunctionsPortal)
* [<span data-ttu-id="a192b-183">Šablony funkcí Azure</span><span class="sxs-lookup"><span data-stu-id="a192b-183">Azure Functions templates</span></span>](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [<span data-ttu-id="a192b-184">Sada Azure WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="a192b-184">Azure WebJobs SDK</span></span>](https://github.com/Azure/azure-webjobs-sdk/)
* [<span data-ttu-id="a192b-185">Sada Azure WebJobs SDK rozšíření</span><span class="sxs-lookup"><span data-stu-id="a192b-185">Azure WebJobs SDK Extensions</span></span>](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a><span data-ttu-id="a192b-186">Vazby</span><span class="sxs-lookup"><span data-stu-id="a192b-186">Bindings</span></span>
<span data-ttu-id="a192b-187">Zde je tabulku všechny podporované vazby.</span><span class="sxs-lookup"><span data-stu-id="a192b-187">Here is a table of all supported bindings.</span></span>

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a><span data-ttu-id="a192b-188">Hlášení problémů</span><span class="sxs-lookup"><span data-stu-id="a192b-188">Reporting Issues</span></span>
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a><span data-ttu-id="a192b-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a192b-189">Next steps</span></span>
<span data-ttu-id="a192b-190">Další informace najdete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="a192b-190">For more information, see the following resources:</span></span>

* [<span data-ttu-id="a192b-191">Osvědčené postupy pro službu Azure Functions</span><span class="sxs-lookup"><span data-stu-id="a192b-191">Best Practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="a192b-192">Azure funkcí jazyka C# referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="a192b-192">Azure Functions C# developer reference</span></span>](functions-reference-csharp.md)
* [<span data-ttu-id="a192b-193">Azure funkce F # referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="a192b-193">Azure Functions F# developer reference</span></span>](functions-reference-fsharp.md)
* [<span data-ttu-id="a192b-194">Referenční informace pro vývojáře Azure funkce NodeJS</span><span class="sxs-lookup"><span data-stu-id="a192b-194">Azure Functions NodeJS developer reference</span></span>](functions-reference-node.md)
* [<span data-ttu-id="a192b-195">Azure funkce triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="a192b-195">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)
* <span data-ttu-id="a192b-196">[Azure Functions: Cesty](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) na blog týmu Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="a192b-196">[Azure Functions: The Journey](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) on the Azure App Service team blog.</span></span> <span data-ttu-id="a192b-197">Historie jak byla vyvinuta Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="a192b-197">A history of how Azure Functions was developed.</span></span>

