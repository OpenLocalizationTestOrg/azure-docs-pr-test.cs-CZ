---
title: "aaaGuidance pro vývoj Azure Functions | Microsoft Docs"
description: "Další koncepty Azure Functions hello a techniky, je nutné, aby toodevelop funkcí v Azure, ve všech programovací jazyky a vazeb."
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
ms.openlocfilehash: 86d37dae5333f615faafc966e9da6e08e0a6354e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-developers-guide"></a><span data-ttu-id="b550c-104">Příručka pro vývojáře Azure funkce</span><span class="sxs-lookup"><span data-stu-id="b550c-104">Azure Functions developers guide</span></span>
<span data-ttu-id="b550c-105">V Azure Functions se konkrétní funkce sdílet několik klíčových technických konceptech a součásti, bez ohledu na jazyk hello nebo vazby, které používáte.</span><span class="sxs-lookup"><span data-stu-id="b550c-105">In Azure Functions, specific functions share a few core technical concepts and components, regardless of hello language or binding you use.</span></span> <span data-ttu-id="b550c-106">Před přechodem do učení podrobnosti o konkrétní tooa danou jazyk nebo vazba být jisti tooread prostřednictvím tento přehled, která se použije tooall z nich.</span><span class="sxs-lookup"><span data-stu-id="b550c-106">Before you jump into learning details specific tooa given language or binding, be sure tooread through this overview that applies tooall of them.</span></span>

<span data-ttu-id="b550c-107">Tento článek předpokládá, že jste si přečetli již hello [přehled Azure Functions](functions-overview.md) a jsou obeznámeni s [WebJobs SDK koncepty, jako jsou aktivační události, vazby a hello JobHost runtime](../app-service-web/websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="b550c-107">This article assumes that you've already read hello [Azure Functions overview](functions-overview.md) and are familiar with [WebJobs SDK concepts such as triggers, bindings, and hello JobHost runtime](../app-service-web/websites-dotnet-webjobs-sdk.md).</span></span> <span data-ttu-id="b550c-108">Azure Functions je založena na hello WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="b550c-108">Azure Functions is based on hello WebJobs SDK.</span></span> 

## <a name="function-code"></a><span data-ttu-id="b550c-109">Kód – funkce</span><span class="sxs-lookup"><span data-stu-id="b550c-109">Function code</span></span>
<span data-ttu-id="b550c-110">A *funkce* je primární koncept hello v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="b550c-110">A *function* is hello primary concept in Azure Functions.</span></span> <span data-ttu-id="b550c-111">Psaní kódu pro funkce v jazyce podle vaší volby a uložte hello kódu a konfigurační soubory v hello stejné složce.</span><span class="sxs-lookup"><span data-stu-id="b550c-111">You write code for a function in a language of your choice and save hello code and configuration files in hello same folder.</span></span> <span data-ttu-id="b550c-112">Konfigurace Hello jmenuje `function.json`, která obsahuje konfigurační data JSON.</span><span class="sxs-lookup"><span data-stu-id="b550c-112">hello configuration is named `function.json`, which contains JSON configuration data.</span></span> <span data-ttu-id="b550c-113">Podporuje různé jazyky a každé z nich má mírně odlišné prostředí optimalizované toowork nejvhodnější pro tento jazyk.</span><span class="sxs-lookup"><span data-stu-id="b550c-113">Various languages are supported, and each one has a slightly different experience optimized toowork best for that language.</span></span> 

<span data-ttu-id="b550c-114">soubor function.json Hello definuje hello vazby funkcí a dalších nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b550c-114">hello function.json file defines hello function bindings and other configuration settings.</span></span> <span data-ttu-id="b550c-115">modul runtime Hello používá tento soubor toodetermine hello události toomonitor a jak toopass dat do a vrátit data z funkce provádění.</span><span class="sxs-lookup"><span data-stu-id="b550c-115">hello runtime uses this file toodetermine hello events toomonitor and how toopass data into and return data from function execution.</span></span> <span data-ttu-id="b550c-116">Hello následuje příklad souboru function.json.</span><span class="sxs-lookup"><span data-stu-id="b550c-116">hello following is an example function.json file.</span></span>

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

<span data-ttu-id="b550c-117">Sada hello `disabled` vlastnost příliš`true` tooprevent hello funkce z spouštěna.</span><span class="sxs-lookup"><span data-stu-id="b550c-117">Set hello `disabled` property too`true` tooprevent hello function from being executed.</span></span>

<span data-ttu-id="b550c-118">Hello `bindings` vlastnost je, kde můžete konfigurovat triggerů a vazeb.</span><span class="sxs-lookup"><span data-stu-id="b550c-118">hello `bindings` property is where you configure both triggers and bindings.</span></span> <span data-ttu-id="b550c-119">Každou vazbu sdílí několik běžných nastavení a některá nastavení, které jsou specifické tooa konkrétní typ vazby.</span><span class="sxs-lookup"><span data-stu-id="b550c-119">Each binding shares a few common settings and some settings, which are specific tooa particular type of binding.</span></span> <span data-ttu-id="b550c-120">Každé vazby vyžaduje hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="b550c-120">Every binding requires hello following settings:</span></span>

| <span data-ttu-id="b550c-121">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b550c-121">Property</span></span> | <span data-ttu-id="b550c-122">Hodnoty nebo typy</span><span class="sxs-lookup"><span data-stu-id="b550c-122">Values/Types</span></span> | <span data-ttu-id="b550c-123">Komentáře</span><span class="sxs-lookup"><span data-stu-id="b550c-123">Comments</span></span> |
| --- | --- | --- |
| `type` |<span data-ttu-id="b550c-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b550c-124">string</span></span> |<span data-ttu-id="b550c-125">Typ vazby.</span><span class="sxs-lookup"><span data-stu-id="b550c-125">Binding type.</span></span> <span data-ttu-id="b550c-126">Například, `queueTrigger`.</span><span class="sxs-lookup"><span data-stu-id="b550c-126">For example, `queueTrigger`.</span></span> |
| `direction` |<span data-ttu-id="b550c-127">v out</span><span class="sxs-lookup"><span data-stu-id="b550c-127">'in', 'out'</span></span> |<span data-ttu-id="b550c-128">Určuje, zda hello vazby pro příjem dat do funkce hello nebo odeslání dat z funkce hello.</span><span class="sxs-lookup"><span data-stu-id="b550c-128">Indicates whether hello binding is for receiving data into hello function or sending data from hello function.</span></span> |
| `name` |<span data-ttu-id="b550c-129">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b550c-129">string</span></span> |<span data-ttu-id="b550c-130">Hello název, který se používá pro hello vázaných dat v hello funkce.</span><span class="sxs-lookup"><span data-stu-id="b550c-130">hello name that is used for hello bound data in hello function.</span></span> <span data-ttu-id="b550c-131">Pro jazyk C# to je název argumentu; pro jazyk JavaScript to je hello klíče v seznamu klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="b550c-131">For C#, this is an argument name; for JavaScript, it's hello key in a key/value list.</span></span> |

## <a name="function-app"></a><span data-ttu-id="b550c-132">Funkce aplikace</span><span class="sxs-lookup"><span data-stu-id="b550c-132">Function app</span></span>
<span data-ttu-id="b550c-133">Funkce aplikace se skládá z jedné nebo více jednotlivých funkcí, které se spravují dohromady službou Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b550c-133">A function app is comprised of one or more individual functions that are managed together by Azure App Service.</span></span> <span data-ttu-id="b550c-134">Všechny funkce hello ve sdílené složce, funkce aplikace hello stejné ceny plán, průběžné nasazování a verze modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="b550c-134">All of hello functions in a function app share hello same pricing plan, continuous deployment and runtime version.</span></span> <span data-ttu-id="b550c-135">Funkce, které jsou napsané v může více jazyků, že všechny sdílené složky hello stejné funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="b550c-135">Functions written in multiple languages can all share hello same function app.</span></span> <span data-ttu-id="b550c-136">Vezměte v úvahu funkce aplikace jako způsob tooorganize a souhrnně spravovat funkcí.</span><span class="sxs-lookup"><span data-stu-id="b550c-136">Think of a function app as a way tooorganize and collectively manage your functions.</span></span> 

## <a name="runtime-script-host-and-web-host"></a><span data-ttu-id="b550c-137">Modul runtime (hostitel skriptu a webového hostitele)</span><span class="sxs-lookup"><span data-stu-id="b550c-137">Runtime (script host and web host)</span></span>
<span data-ttu-id="b550c-138">Hello runtime nebo skriptu hostitele, je hello základní sada WebJobs SDK hostitele, který naslouchá událostem, shromažďuje a odesílá data a nakonec spustí váš kód.</span><span class="sxs-lookup"><span data-stu-id="b550c-138">hello runtime, or script host, is hello underlying WebJobs SDK host that listens for events, gathers and sends data, and ultimately runs your code.</span></span> 

<span data-ttu-id="b550c-139">aktivační události toofacilitate HTTP, je také webového hostitele, který je navrženou toosit před hello skriptu hostitele v produkčních scénářích.</span><span class="sxs-lookup"><span data-stu-id="b550c-139">toofacilitate HTTP triggers, there is also a web host that is designed toosit in front of hello script host in production scenarios.</span></span> <span data-ttu-id="b550c-140">Má dva hostitele pomáhá tooisolate hello skriptu hostitele od provozu front-endu hello spravuje hello webového hostitele.</span><span class="sxs-lookup"><span data-stu-id="b550c-140">Having two hosts helps tooisolate hello script host from hello front end traffic managed by hello web host.</span></span>

## <a name="folder-structure"></a><span data-ttu-id="b550c-141">Struktura složek</span><span class="sxs-lookup"><span data-stu-id="b550c-141">Folder Structure</span></span>
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

<span data-ttu-id="b550c-142">Při vytváření projektu pro nasazení funkce tooa funkce aplikace v Azure App Service, můžete tuto strukturu považovat kódu lokality.</span><span class="sxs-lookup"><span data-stu-id="b550c-142">When setting-up a project for deploying functions tooa function app in Azure App Service, you can treat this folder structure as your site code.</span></span> <span data-ttu-id="b550c-143">Můžete použít stávající nástroje, například průběžnou integraci a nasazení, nebo vlastní nasazení skriptů pro tuto činnost čas instalace balíčku nasazení nebo code transpilation.</span><span class="sxs-lookup"><span data-stu-id="b550c-143">You can use existing tools like continuous integration and deployment, or custom deployment scripts for doing deploy time package installation or code transpilation.</span></span>

> [!NOTE]
> <span data-ttu-id="b550c-144">Ujistěte se, že toodeploy vaše `host.json` složek souborů a funkce přímo toohello `wwwroot` složky.</span><span class="sxs-lookup"><span data-stu-id="b550c-144">Make sure toodeploy your `host.json` file and function folders directly toohello `wwwroot` folder.</span></span> <span data-ttu-id="b550c-145">Nezahrnovat hello `wwwroot` složky ve vašem nasazení.</span><span class="sxs-lookup"><span data-stu-id="b550c-145">Do not include hello `wwwroot` folder in your deployments.</span></span> <span data-ttu-id="b550c-146">Jinak, v níž se `wwwroot\wwwroot` složek.</span><span class="sxs-lookup"><span data-stu-id="b550c-146">Otherwise, you end up with `wwwroot\wwwroot` folders.</span></span> 
> 
> 

## <span data-ttu-id="b550c-147"><a id="fileupdate"></a>Jak tooupdate funkce soubory aplikace</span><span class="sxs-lookup"><span data-stu-id="b550c-147"><a id="fileupdate"></a> How tooupdate function app files</span></span>
<span data-ttu-id="b550c-148">editor Funkce Hello součástí hello portál Azure vám umožní aktualizovat hello *function.json* soubor a soubor hello kód pro funkci.</span><span class="sxs-lookup"><span data-stu-id="b550c-148">hello function editor built into hello Azure portal lets you update hello *function.json* file and hello code file for a function.</span></span> <span data-ttu-id="b550c-149">tooupload nebo aktualizace jiných souborů, jako *package.json* nebo *project.json* nebo závislosti, máte toouse jiné metody nasazení.</span><span class="sxs-lookup"><span data-stu-id="b550c-149">tooupload or update other files such as *package.json* or *project.json* or dependencies, you have toouse other deployment methods.</span></span>

<span data-ttu-id="b550c-150">Funkce aplikace jsou postaveny na aplikační služby, takže všechny hello [možnosti nasazení webové aplikace k dispozici toostandard](../app-service-web/web-sites-deploy.md) jsou také k dispozici pro funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="b550c-150">Function apps are built on App Service, so all hello [deployment options available toostandard web apps](../app-service-web/web-sites-deploy.md) are also available for function apps.</span></span> <span data-ttu-id="b550c-151">Tady jsou některé metody, můžete použít tooupload nebo funkce aplikace soubory aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b550c-151">Here are some methods you can use tooupload or update function app files.</span></span> 

#### <a name="toouse-app-service-editor"></a><span data-ttu-id="b550c-152">toouse Editor služby aplikace</span><span class="sxs-lookup"><span data-stu-id="b550c-152">toouse App Service Editor</span></span>
1. <span data-ttu-id="b550c-153">Na portálu Azure Functions hello, klikněte na tlačítko **funkce nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b550c-153">In hello Azure Functions portal, click **Function app settings**.</span></span>
2. <span data-ttu-id="b550c-154">V hello **Upřesnit nastavení** klikněte na tlačítko **přejít nastavení služby tooApp**.</span><span class="sxs-lookup"><span data-stu-id="b550c-154">In hello **Advanced Settings** section, click **Go tooApp Service Settings**.</span></span>
3. <span data-ttu-id="b550c-155">Klikněte na tlačítko **aplikace služby Editor** v nabídce aplikace navigaci v části **nástroje pro vývoj**.</span><span class="sxs-lookup"><span data-stu-id="b550c-155">Click **App Service Editor** in App Menu Nav under **DEVELOPMENT TOOLS**.</span></span>
4. <span data-ttu-id="b550c-156">Klikněte na tlačítko **přejděte**.</span><span class="sxs-lookup"><span data-stu-id="b550c-156">click **Go**.</span></span>
   
   <span data-ttu-id="b550c-157">Po načtení Editor služby aplikace se zobrazí hello *host.json* složek souborů a funkce v části *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="b550c-157">After App Service Editor loads, you'll see hello *host.json* file and function folders under *wwwroot*.</span></span> 
5. <span data-ttu-id="b550c-158">Tooedit otevřených souborů, nebo přetažení z vaší vývoj počítač tooupload souborů.</span><span class="sxs-lookup"><span data-stu-id="b550c-158">Open files tooedit them, or drag and drop from your development machine tooupload files.</span></span>

#### <a name="toouse-hello-function-apps-scm-kudu-endpoint"></a><span data-ttu-id="b550c-159">koncový bod toouse hello funkce aplikace SCM (Kudu)</span><span class="sxs-lookup"><span data-stu-id="b550c-159">toouse hello function app's SCM (Kudu) endpoint</span></span>
1. <span data-ttu-id="b550c-160">Přejděte do: `https://<function_app_name>.scm.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="b550c-160">Navigate to: `https://<function_app_name>.scm.azurewebsites.net`.</span></span>
2. <span data-ttu-id="b550c-161">Klikněte na tlačítko **ladění konzoly > CMD**.</span><span class="sxs-lookup"><span data-stu-id="b550c-161">Click **Debug Console > CMD**.</span></span>
3. <span data-ttu-id="b550c-162">Přejděte příliš`D:\home\site\wwwroot\` tooupdate *host.json* nebo `D:\home\site\wwwroot\<function_name>` tooupdate soubory funkce.</span><span class="sxs-lookup"><span data-stu-id="b550c-162">Navigate too`D:\home\site\wwwroot\` tooupdate *host.json* or `D:\home\site\wwwroot\<function_name>` tooupdate a function's files.</span></span>
4. <span data-ttu-id="b550c-163">Přetažení myší souboru chcete tooupload do příslušné složky hello v mřížce souboru hello.</span><span class="sxs-lookup"><span data-stu-id="b550c-163">Drag-and-drop a file you want tooupload into hello appropriate folder in hello file grid.</span></span> <span data-ttu-id="b550c-164">Existují dvě oblasti v mřížce hello souboru, kde můžete vložit do souboru.</span><span class="sxs-lookup"><span data-stu-id="b550c-164">There are two areas in hello file grid where you can drop a file.</span></span> <span data-ttu-id="b550c-165">Pro *.zip* soubory, zobrazí se pole s popiskem hello "Přetáhněte sem tooupload a rozbalte."</span><span class="sxs-lookup"><span data-stu-id="b550c-165">For *.zip* files, a box appears with hello label "Drag here tooupload and unzip."</span></span> <span data-ttu-id="b550c-166">Pro ostatní typy souborů vyřaďte hello soubor mřížky, ale mimo pole "rozbalte" hello.</span><span class="sxs-lookup"><span data-stu-id="b550c-166">For other file types, drop in hello file grid but outside hello "unzip" box.</span></span>

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on hello consumption plan --DonnaM -->

#### <a name="toouse-continuous-deployment"></a><span data-ttu-id="b550c-167">průběžné nasazování toouse</span><span class="sxs-lookup"><span data-stu-id="b550c-167">toouse continuous deployment</span></span>
<span data-ttu-id="b550c-168">Postupujte podle pokynů hello v tématu hello [průběžné nasazování pro Azure Functions](functions-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="b550c-168">Follow hello instructions in hello topic [Continuous deployment for Azure Functions](functions-continuous-deployment.md).</span></span>

## <a name="parallel-execution"></a><span data-ttu-id="b550c-169">Paralelní provádění</span><span class="sxs-lookup"><span data-stu-id="b550c-169">Parallel execution</span></span>
<span data-ttu-id="b550c-170">Dojde-li více spouštěcí událostí rychleji, než funkce jednovláknové runtime dokáže zpracovat, může hello runtime vyvolají funkci hello vícekrát paralelně.</span><span class="sxs-lookup"><span data-stu-id="b550c-170">When multiple triggering events occur faster than a single-threaded function runtime can process them, hello runtime may invoke hello function multiple times in parallel.</span></span>  <span data-ttu-id="b550c-171">Pokud funkce aplikace používá hello [spotřeba hostování plán](functions-scale.md#how-the-consumption-plan-works), může automaticky škálovat aplikaci funkce hello.</span><span class="sxs-lookup"><span data-stu-id="b550c-171">If a function app is using hello [Consumption hosting plan](functions-scale.md#how-the-consumption-plan-works), hello function app could scale out automatically.</span></span>  <span data-ttu-id="b550c-172">Každou instanci hello funkce aplikace, zda text hello aplikace běží hello spotřeba hostování plán nebo běžný [hostování plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), může zpracovat volání souběžných funkce paralelně pomocí více vláken.</span><span class="sxs-lookup"><span data-stu-id="b550c-172">Each instance of hello function app, whether hello app runs on hello Consumption hosting plan or a regular [App Service hosting plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), might process concurrent function invocations in parallel using multiple threads.</span></span>  <span data-ttu-id="b550c-173">maximální počet souběžných funkce volání v každé instanci funkce aplikace Hello se liší podle typu hello používá i hello prostředky využívané třídou jiných funkcí v rámci aplikace hello funkce aktivační události.</span><span class="sxs-lookup"><span data-stu-id="b550c-173">hello maximum number of concurrent function invocations in each function app instance varies based on hello type of trigger being used as well as hello resources used by other functions within hello function app.</span></span>

## <a name="functions-runtime-versioning"></a><span data-ttu-id="b550c-174">Verze runtime funkce</span><span class="sxs-lookup"><span data-stu-id="b550c-174">Functions runtime versioning</span></span>

<span data-ttu-id="b550c-175">Můžete nakonfigurovat hello verzi modulu runtime funkce hello použijte hello `FUNCTIONS_EXTENSION_VERSION` nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="b550c-175">You can configure hello version of hello Functions runtime using hello `FUNCTIONS_EXTENSION_VERSION` app setting.</span></span> <span data-ttu-id="b550c-176">Například hello hodnotu "~ 1" označuje, že funkce aplikace bude používat 1 jako jeho hlavní verzi.</span><span class="sxs-lookup"><span data-stu-id="b550c-176">For example, hello value "~1" indicates that your Function App will use 1 as its major version.</span></span> <span data-ttu-id="b550c-177">Funkce aplikace jsou upgradovaný tooeach nové podverze při jejich vydání.</span><span class="sxs-lookup"><span data-stu-id="b550c-177">Function Apps are upgraded tooeach new minor version as they are released.</span></span> <span data-ttu-id="b550c-178">Hello přesnou verzi vaší aplikace funkce si můžete prohlédnout v hello **nastavení** ve hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b550c-178">You can view hello exact version of your Function App in hello **Settings** tab in hello Azure Portal.</span></span>

## <a name="repositories"></a><span data-ttu-id="b550c-179">Úložiště</span><span class="sxs-lookup"><span data-stu-id="b550c-179">Repositories</span></span>
<span data-ttu-id="b550c-180">Hello kód pro Azure Functions je open source a uložené v úložišť GitHub:</span><span class="sxs-lookup"><span data-stu-id="b550c-180">hello code for Azure Functions is open source and stored in GitHub repositories:</span></span>

* [<span data-ttu-id="b550c-181">Azure Functions runtime</span><span class="sxs-lookup"><span data-stu-id="b550c-181">Azure Functions runtime</span></span>](https://github.com/Azure/azure-webjobs-sdk-script/)
* [<span data-ttu-id="b550c-182">Azure Functions na portálu</span><span class="sxs-lookup"><span data-stu-id="b550c-182">Azure Functions portal</span></span>](https://github.com/projectkudu/AzureFunctionsPortal)
* [<span data-ttu-id="b550c-183">Šablony funkcí Azure</span><span class="sxs-lookup"><span data-stu-id="b550c-183">Azure Functions templates</span></span>](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [<span data-ttu-id="b550c-184">Sada Azure WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="b550c-184">Azure WebJobs SDK</span></span>](https://github.com/Azure/azure-webjobs-sdk/)
* [<span data-ttu-id="b550c-185">Sada Azure WebJobs SDK rozšíření</span><span class="sxs-lookup"><span data-stu-id="b550c-185">Azure WebJobs SDK Extensions</span></span>](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a><span data-ttu-id="b550c-186">Vazby</span><span class="sxs-lookup"><span data-stu-id="b550c-186">Bindings</span></span>
<span data-ttu-id="b550c-187">Zde je tabulku všechny podporované vazby.</span><span class="sxs-lookup"><span data-stu-id="b550c-187">Here is a table of all supported bindings.</span></span>

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a><span data-ttu-id="b550c-188">Hlášení problémů</span><span class="sxs-lookup"><span data-stu-id="b550c-188">Reporting Issues</span></span>
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a><span data-ttu-id="b550c-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b550c-189">Next steps</span></span>
<span data-ttu-id="b550c-190">Další informace najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="b550c-190">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="b550c-191">Osvědčené postupy pro službu Azure Functions</span><span class="sxs-lookup"><span data-stu-id="b550c-191">Best Practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="b550c-192">Azure funkcí jazyka C# referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="b550c-192">Azure Functions C# developer reference</span></span>](functions-reference-csharp.md)
* [<span data-ttu-id="b550c-193">Azure funkce F # referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="b550c-193">Azure Functions F# developer reference</span></span>](functions-reference-fsharp.md)
* [<span data-ttu-id="b550c-194">Referenční informace pro vývojáře Azure funkce NodeJS</span><span class="sxs-lookup"><span data-stu-id="b550c-194">Azure Functions NodeJS developer reference</span></span>](functions-reference-node.md)
* [<span data-ttu-id="b550c-195">Azure funkce triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="b550c-195">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)
* <span data-ttu-id="b550c-196">[Azure Functions: hello cesty](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) na blog týmu Azure App Service hello.</span><span class="sxs-lookup"><span data-stu-id="b550c-196">[Azure Functions: hello Journey](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) on hello Azure App Service team blog.</span></span> <span data-ttu-id="b550c-197">Historie jak byla vyvinuta Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="b550c-197">A history of how Azure Functions was developed.</span></span>

