---
title: "aaaBuild flexibilně škálovatelné aplikace v Azure | Microsoft Docs"
description: "Zjistěte, jak toouse různé služby Azure toomaximize hello výkonu aplikace ASP.NET v Azure."
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: erikre
editor: 
ms.assetid: a4d49ac7-0f97-4997-84c5-cdb9c4465757
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 03/23/2017
ms.author: cephalin
ms.openlocfilehash: 7952647b49a82c286c6a737eb41a7f23a13fd75e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-hyper-scale-web-app-in-azure"></a><span data-ttu-id="0b13d-103">Sestavení flexibilně škálovatelné webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="0b13d-103">Build a hyper-scale web app in Azure</span></span>

<span data-ttu-id="0b13d-104">Tento kurz ukazuje, jak požaduje tooscale se webové aplikace ASP.NET v Azure toomaximize uživatele.</span><span class="sxs-lookup"><span data-stu-id="0b13d-104">This tutorial shows you how tooscale out an ASP.NET web app in Azure toomaximize user requests.</span></span>

<span data-ttu-id="0b13d-105">Před zahájením tohoto kurzu, zajistěte, aby [hello je nainstalované rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="0b13d-105">Before starting this tutorial, ensure that [hello Azure CLI is installed](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) on your machine.</span></span> <span data-ttu-id="0b13d-106">Kromě toho musíte [Visual Studio](https://www.visualstudio.com/vs/) na místním počítači toorun hello ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0b13d-106">In addition, you need [Visual Studio](https://www.visualstudio.com/vs/) on your local machine toorun hello sample application.</span></span>

## <a name="step-1---get-sample-application"></a><span data-ttu-id="0b13d-107">Krok 1 – Get ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="0b13d-107">Step 1 - Get sample application</span></span>
<span data-ttu-id="0b13d-108">V tomto kroku nastavíte místní projekt ASP.NET hello.</span><span class="sxs-lookup"><span data-stu-id="0b13d-108">In this step, you set up hello local ASP.NET project.</span></span>

### <a name="clone-hello-application-repository"></a><span data-ttu-id="0b13d-109">Klon hello aplikace úložiště</span><span class="sxs-lookup"><span data-stu-id="0b13d-109">Clone hello application repository</span></span>

<span data-ttu-id="0b13d-110">Otevřete hello terminál příkazového řádku podle svého výběru a `CD` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="0b13d-110">Open hello command-line terminal of your choice and `CD` tooa working directory.</span></span> <span data-ttu-id="0b13d-111">Potom spusťte hello následující příkazy tooclone hello ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0b13d-111">Then, run hello following commands tooclone hello sample application.</span></span> 

```powershell
git clone https://github.com/cephalin/HighScaleApp.git
```

### <a name="run-hello-sample-application-in-visual-studio"></a><span data-ttu-id="0b13d-112">Spustit hello ukázkovou aplikaci v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b13d-112">Run hello sample application in Visual Studio</span></span>

<span data-ttu-id="0b13d-113">Otevřete hello řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0b13d-113">Open hello solution in Visual Studio.</span></span>

```powershell
cd HighScaleApp
.\HighScaleApp.sln
```

<span data-ttu-id="0b13d-114">Typ `F5` toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b13d-114">Type `F5` toorun hello application.</span></span>

<span data-ttu-id="0b13d-115">Tato ukázka webové aplikace ASP.NET pochází z hello výchozí šablonu a trvá uživatelské relace a používá hello výstupní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="0b13d-115">This sample ASP.NET web application comes from hello default template, and persists user sessions and uses hello output cache.</span></span> <span data-ttu-id="0b13d-116">Podívejte se na `HighScaleApp\Controllers\HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="0b13d-116">Take a look at `HighScaleApp\Controllers\HomeController.cs`.</span></span> <span data-ttu-id="0b13d-117">Hello `Index()` metoda přidá část dat toohello relace.</span><span class="sxs-lookup"><span data-stu-id="0b13d-117">hello `Index()` method adds a piece of data toohello session.</span></span>

```csharp
Session.Add("visited", "true"); 
```

<span data-ttu-id="0b13d-118">A hello `About()` a `Contact()` metody mezipaměti jejich výstup.</span><span class="sxs-lookup"><span data-stu-id="0b13d-118">And hello `About()` and `Contact()` methods cache their output.</span></span>

```csharp
[OutputCache(Duration = 60)]
```

## <a name="step-2---deploy-tooazure"></a><span data-ttu-id="0b13d-119">Krok 2 – nasazení tooAzure</span><span class="sxs-lookup"><span data-stu-id="0b13d-119">Step 2 - Deploy tooAzure</span></span>
<span data-ttu-id="0b13d-120">V tomto kroku vytvoření webové aplikace Azure a nasazení vaší tooit aplikace ASP.NET ukázka.</span><span class="sxs-lookup"><span data-stu-id="0b13d-120">In this step, you create an Azure web app and deploy your sample ASP.NET application tooit.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="0b13d-121">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="0b13d-121">Create a resource group</span></span>   
<span data-ttu-id="0b13d-122">Použití [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) toocreate [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) v oblasti západní Evropa hello.</span><span class="sxs-lookup"><span data-stu-id="0b13d-122">Use [az group create](https://docs.microsoft.com/cli/azure/group#create) toocreate a [resource group](../azure-resource-manager/resource-group-overview.md) in hello West Europe region.</span></span> <span data-ttu-id="0b13d-123">Skupina prostředků je, kam jste umístili všechny hello prostředky Azure, které chcete toomanage společně, například hello webovou aplikaci a všechny SQL databáze back-end.</span><span class="sxs-lookup"><span data-stu-id="0b13d-123">A resource group is where you put all hello Azure resources that you want toomanage together, such as hello web app and any SQL Database back end.</span></span>

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

<span data-ttu-id="0b13d-124">toosee jaké možné hodnoty, které můžete použít pro `---location`, použijte hello [míst seznamu služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) příkaz.</span><span class="sxs-lookup"><span data-stu-id="0b13d-124">toosee what possible values you can use for `---location`, use hello [az appservice list-locations](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) command.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="0b13d-125">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="0b13d-125">Create an App Service plan</span></span>
<span data-ttu-id="0b13d-126">Použití [vytvořit plán aplikační služby az](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) toocreate "B1" [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0b13d-126">Use [az appservice plan create](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) toocreate a "B1" [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span> 

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1
```

<span data-ttu-id="0b13d-127">Plán služby App Service je jednotka škálování, která může obsahovat libovolný počet aplikací, které chcete tooscale si nebo si společně over hello stejné infrastruktury služby App Service.</span><span class="sxs-lookup"><span data-stu-id="0b13d-127">An App Service plan is a scale unit, which can include any number of apps that you want tooscale up or out together over hello same App Service infrastructure.</span></span> <span data-ttu-id="0b13d-128">Každý plán je také přiřazený [cenová úroveň](https://azure.microsoft.com/en-us/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="0b13d-128">Each plan is also assigned a [pricing tier](https://azure.microsoft.com/en-us/pricing/details/app-service/).</span></span> <span data-ttu-id="0b13d-129">Vyšší úrovně zahrnují lepší hardwaru a další funkce, jako je více instancí Škálováním na více systémů.</span><span class="sxs-lookup"><span data-stu-id="0b13d-129">Higher tiers include better hardware and more features, such as more scale-out instances.</span></span>

<span data-ttu-id="0b13d-130">V tomto kurzu je B1 hello minimální vrstvy, která umožňuje škálování toothree instance.</span><span class="sxs-lookup"><span data-stu-id="0b13d-130">For this tutorial, B1 is hello minimum tier that enables scale out toothree instances.</span></span> <span data-ttu-id="0b13d-131">Aplikace může vždy přesunout nahoru nebo dolů hello později cenová úroveň spuštěním [aktualizace plánu služby App Service az](https://docs.microsoft.com/cli/azure/appservice/plan#update).</span><span class="sxs-lookup"><span data-stu-id="0b13d-131">You can always move your app up or down hello pricing tier later by running [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update).</span></span> 

### <a name="create-a-web-app"></a><span data-ttu-id="0b13d-132">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="0b13d-132">Create a web app</span></span>
<span data-ttu-id="0b13d-133">Použití [vytvořit webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate webovou aplikaci s jedinečným názvem v `$appName`.</span><span class="sxs-lookup"><span data-stu-id="0b13d-133">Use [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate a web app with a unique name in `$appName`.</span></span>

```azurecli
$appName = "<replace-with-a-unique-name>"
az appservice web create --name $appName --resource-group myResourceGroup --plan myAppServicePlan
```

### <a name="set-deployment-credentials"></a><span data-ttu-id="0b13d-134">Nastavení přihlašovacích údajů nasazení</span><span class="sxs-lookup"><span data-stu-id="0b13d-134">Set deployment credentials</span></span>
<span data-ttu-id="0b13d-135">Použití [nastavený uživatel nasazení webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) tooset nasazením úrovni účtu přihlašovací údaje pro službu App Service.</span><span class="sxs-lookup"><span data-stu-id="0b13d-135">Use [az appservice web deployment user set](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) tooset your account-level deployment credentials for App Service.</span></span>

```azurecli
az appservice web deployment user set --user-name <letters-numbers> --password <mininum-8-char-captital-lowercase-letters-numbers>
```

### <a name="configure-git-deployment"></a><span data-ttu-id="0b13d-136">Konfigurace nasazení Git</span><span class="sxs-lookup"><span data-stu-id="0b13d-136">Configure Git deployment</span></span>
<span data-ttu-id="0b13d-137">Použití [az aplikační služby webové správy zdrojového kódu config místní git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure místní nasazení Git.</span><span class="sxs-lookup"><span data-stu-id="0b13d-137">Use [az appservice web source-control config-local-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure local Git deployment.</span></span>

```azurecli
az appservice web source-control config-local-git --name $appName --resource-group myResourceGroup
```

<span data-ttu-id="0b13d-138">Tento příkaz vám poskytuje výstup bude vypadat takto hello následující:</span><span class="sxs-lookup"><span data-stu-id="0b13d-138">This command gives you an output that looks like hello following:</span></span>

```json
{
  "url": "https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git"
}
```

<span data-ttu-id="0b13d-139">Použití hello vrátil tooconfigure adresu URL vašeho Git vzdálené.</span><span class="sxs-lookup"><span data-stu-id="0b13d-139">Use hello returned URL tooconfigure your Git remote.</span></span> <span data-ttu-id="0b13d-140">Hello následující příkaz použije hello předcházející příklad výstupu.</span><span class="sxs-lookup"><span data-stu-id="0b13d-140">hello following command uses hello preceding output example.</span></span>

```powershell
git remote add azure https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-hello-sample-application"></a><span data-ttu-id="0b13d-141">Nasazení ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="0b13d-141">Deploy hello sample application</span></span>
<span data-ttu-id="0b13d-142">Můžete je nyní připraven toodeploy ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0b13d-142">You are now ready toodeploy your sample application.</span></span> <span data-ttu-id="0b13d-143">Spusťte `git push`.</span><span class="sxs-lookup"><span data-stu-id="0b13d-143">Run `git push`.</span></span>

```powershell
git push azure master
```

<span data-ttu-id="0b13d-144">Po zobrazení výzvy pro heslo pomocí hello heslo, které jste zadali, když jste spustili `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="0b13d-144">When prompted for password, use hello password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-tooazure-web-app"></a><span data-ttu-id="0b13d-145">Procházet tooAzure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="0b13d-145">Browse tooAzure web app</span></span>
<span data-ttu-id="0b13d-146">Použití [procházet webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee aplikace živý běh v Azure, spusťte tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="0b13d-146">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee your app running live in Azure, run this command.</span></span>

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-3---connect-tooredis"></a><span data-ttu-id="0b13d-147">Krok 3 – připojit tooRedis</span><span class="sxs-lookup"><span data-stu-id="0b13d-147">Step 3 - Connect tooRedis</span></span>
<span data-ttu-id="0b13d-148">V tomto kroku nastavíte Azure Redis Cache jako externí, společně umístěných mezipaměti tooyour webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="0b13d-148">In this step, you set up Azure Redis Cache as an external, colocated cache tooyour Azure web app.</span></span> <span data-ttu-id="0b13d-149">Můžete rychle využít Redis toocache výstupu stránky.</span><span class="sxs-lookup"><span data-stu-id="0b13d-149">You can quickly utilize Redis toocache your page output.</span></span> <span data-ttu-id="0b13d-150">Kromě toho, když jste škálovat webové aplikace později, Redis vám pomůže zachovat uživatelských relací napříč více instancí spolehlivě.</span><span class="sxs-lookup"><span data-stu-id="0b13d-150">In addition, when you scale out your web apps later, Redis helps you persist user sessions across multiple instances reliably.</span></span>

### <a name="create-an-azure-redis-cache"></a><span data-ttu-id="0b13d-151">Vytvoření Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="0b13d-151">Create an Azure Redis Cache</span></span>
<span data-ttu-id="0b13d-152">Použití [vytvořit az redis](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate mezipaměti Azure Redis a uložte hello výstup JSON.</span><span class="sxs-lookup"><span data-stu-id="0b13d-152">Use [az redis create](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate an Azure Redis Cache and save hello JSON output.</span></span> <span data-ttu-id="0b13d-153">Použijte jedinečný název v `$cacheName`.</span><span class="sxs-lookup"><span data-stu-id="0b13d-153">Use a unique name in `$cacheName`.</span></span>

```powershell
$cacheName = "<replace-with-a-unique-cache-name>"
$redis = (az redis create --name $cacheName --resource-group myResourceGroup --location "West Europe" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

### <a name="configure-hello-application-toouse-redis"></a><span data-ttu-id="0b13d-154">Konfigurace aplikace toouse hello Redis</span><span class="sxs-lookup"><span data-stu-id="0b13d-154">Configure hello application toouse Redis</span></span>
<span data-ttu-id="0b13d-155">Formát hello připojovací řetězec pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="0b13d-155">Format hello connection string for your cache.</span></span>

```powershell
$connstring = "$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False"
$connstring 
```

<span data-ttu-id="0b13d-156">druhý řádek Hello měl dát výstupu, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="0b13d-156">hello second line should give you an output that looks like this:</span></span>

```
mycachename.redis.cache.windows.net:6380,password=/rQP/TLz1mrEPpmh9b/gnfns/t9vBRXqXn3i1RwBjGA=,ssl=True,abortConnect=False
```

<span data-ttu-id="0b13d-157">V sadě Visual Studio vytvořit soubor webové konfigurace v kořenového adresáře projektu názvem `redis.config` a hello vložte následující kód do ní.</span><span class="sxs-lookup"><span data-stu-id="0b13d-157">In Visual Studio, create a web configuration file in your project root called `redis.config` and paste hello following code into it.</span></span> <span data-ttu-id="0b13d-158">V `value`, použít hello připojovací řetězec z hello výstup prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0b13d-158">In `value`, use hello connection string from hello PowerShell output.</span></span>

```xml
<appSettings>
  <add key="RedisConnection" value="your-azure-redis-cache-connection-string"/>
</appSettings>
```

<span data-ttu-id="0b13d-159">Pokud si prohlédnete hello `.gitignore` souboru v úložiště Git, zobrazí se, že tento soubor je vyloučen z řízení zdrojů.</span><span class="sxs-lookup"><span data-stu-id="0b13d-159">If you look at hello `.gitignore` file in your Git repository, you'll see that this file is excluded from source control.</span></span> <span data-ttu-id="0b13d-160">Tímto způsobem je udržováno bezpečné vaše citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="0b13d-160">That way your sensitive information is kept safe.</span></span> 

<span data-ttu-id="0b13d-161">Otevřete `Web.config`.</span><span class="sxs-lookup"><span data-stu-id="0b13d-161">Open `Web.config`.</span></span> <span data-ttu-id="0b13d-162">Všimněte si hello `<appSettings file="redis.config">` element, který získá hello nastavení, které jste vytvořili v `redis.config`.</span><span class="sxs-lookup"><span data-stu-id="0b13d-162">Notice hello `<appSettings file="redis.config">` element, which gets hello setting you created in `redis.config`.</span></span> 

<span data-ttu-id="0b13d-163">Najít označeno jako komentář hello oddíl, který zahrnuje `<sessionState>` a `<caching>`.</span><span class="sxs-lookup"><span data-stu-id="0b13d-163">Find hello commented section that includes `<sessionState>` and `<caching>`.</span></span> <span data-ttu-id="0b13d-164">Zrušením komentáře u této části.</span><span class="sxs-lookup"><span data-stu-id="0b13d-164">Uncomment this section.</span></span>

![](./media/app-service-web-tutorial-hyper-scale-app/redisproviders.png)

<span data-ttu-id="0b13d-165">Tento kód vyhledá hello Redis připojovacího řetězce je definována v `RedisConnection`.</span><span class="sxs-lookup"><span data-stu-id="0b13d-165">This code looks for hello Redis connection string you defined in `RedisConnection`.</span></span> 

<span data-ttu-id="0b13d-166">Teď vaše aplikace používá Redis toomanage relace a ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="0b13d-166">Your application now uses Redis toomanage sessions and caching.</span></span> <span data-ttu-id="0b13d-167">Typ `F5` toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b13d-167">Type `F5` toorun hello application.</span></span> <span data-ttu-id="0b13d-168">Pokud chcete můžete si stáhnout Redis správy klienta toovisualize hello data, která jsou nyní uloženy toohello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="0b13d-168">If you like, you can download a Redis management client toovisualize hello data that is now saved toohello cache.</span></span>

### <a name="configure-hello-connection-string-in-azure"></a><span data-ttu-id="0b13d-169">Konfigurace hello připojovací řetězec v Azure</span><span class="sxs-lookup"><span data-stu-id="0b13d-169">Configure hello connection string in Azure</span></span>

<span data-ttu-id="0b13d-170">Pro vaše aplikace toowork v Azure, budete potřebovat tooconfigure hello stejný připojovací řetězec Redis v Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b13d-170">For your application toowork in Azure, you need tooconfigure hello same Redis connection string in your Azure web app.</span></span> <span data-ttu-id="0b13d-171">Vzhledem k tomu `redis.config` nezachová ve správě zdrojového kódu není nasazený tooAzure při spuštění nasazení Git.</span><span class="sxs-lookup"><span data-stu-id="0b13d-171">Since `redis.config` is not maintained in source control, it is not deployed tooAzure when you run Git deployment.</span></span>

<span data-ttu-id="0b13d-172">Použití [aktualizovat az služby App Service web konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd hello připojovací řetězec s hello stejný název (`RedisConnection`).</span><span class="sxs-lookup"><span data-stu-id="0b13d-172">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd hello connection string with hello same name (`RedisConnection`).</span></span>

<span data-ttu-id="0b13d-173">AZ služby App Service web konfigurace appsettings aktualizovat – nastavení "RedisConnection = $connstring" – název $appName – myResourceGroup skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="0b13d-173">az appservice web config appsettings update --settings "RedisConnection=$connstring" --name $appName --resource-group myResourceGroup</span></span>

<span data-ttu-id="0b13d-174">Nezapomeňte, že `$connstring` obsahuje hello formátu připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="0b13d-174">Remember that `$connstring` contains hello formatted connection string.</span></span>

### <a name="redeploy-hello-application-tooazure"></a><span data-ttu-id="0b13d-175">Znovu nasaďte tooAzure aplikace hello</span><span class="sxs-lookup"><span data-stu-id="0b13d-175">Redeploy hello application tooAzure</span></span>
<span data-ttu-id="0b13d-176">Použít toopush příkazy Git tooAzure vaše změny</span><span class="sxs-lookup"><span data-stu-id="0b13d-176">Use Git commands toopush your changes tooAzure</span></span>

```bash
git add .
git commit -m "now use Redis providers"
git push azure master
```

<span data-ttu-id="0b13d-177">Po zobrazení výzvy pro heslo pomocí hello heslo, které jste zadali, když jste spustili `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="0b13d-177">When prompted for password, use hello password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-toohello-azure-web-app"></a><span data-ttu-id="0b13d-178">Procházet toohello webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="0b13d-178">Browse toohello Azure web app</span></span>
<span data-ttu-id="0b13d-179">Použití [procházet webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee hello změny za provozu v Azure.</span><span class="sxs-lookup"><span data-stu-id="0b13d-179">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee hello changes live in Azure.</span></span>

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-4---scale-toomultiple-instances"></a><span data-ttu-id="0b13d-180">Krok 4 – škálování toomultiple instancí</span><span class="sxs-lookup"><span data-stu-id="0b13d-180">Step 4 - Scale toomultiple instances</span></span>
<span data-ttu-id="0b13d-181">Hello plán služby App Service je hello jednotky škálování pro vaše webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="0b13d-181">hello App Service plan is hello scale unit for your Azure web apps.</span></span> <span data-ttu-id="0b13d-182">tooscale na vaší webové aplikace, můžete škálovat hello plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="0b13d-182">tooscale out your web app, you scale hello App Service plan.</span></span>

<span data-ttu-id="0b13d-183">Použití [aktualizace plánu služby App Service az](https://docs.microsoft.com/cli/azure/appservice/plan#update) hello tooscale out hello instancích toothree plánu služby App Service, což je maximální počet povolený hello B1 cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="0b13d-183">Use [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale out hello App Service plan toothree instances, which is hello maximum number allowed by hello B1 pricing tier.</span></span> <span data-ttu-id="0b13d-184">Mějte na paměti, že je B1 hello cenové úrovně, které jste zvolili při vytváření hello plán služby App Service dříve.</span><span class="sxs-lookup"><span data-stu-id="0b13d-184">Remember that B1 is hello pricing tier that you chose when you created hello App Service plan earlier.</span></span> 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --number-of-workers 3 
```

## <a name="step-5---scale-geographically"></a><span data-ttu-id="0b13d-185">Krok 5 – škálování geograficky</span><span class="sxs-lookup"><span data-stu-id="0b13d-185">Step 5 - Scale geographically</span></span>
<span data-ttu-id="0b13d-186">Při příjmu geograficky, spusťte aplikaci v několika oblastech hello cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="0b13d-186">When scaling geographically, you run your app in multiple regions of hello Azure cloud.</span></span> <span data-ttu-id="0b13d-187">Tento instalační program zatížení zůstatky aplikace další podle Geografie a snižuje dobu odezvy hello tím, že vaše aplikace blíže tooclient prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0b13d-187">This setup load-balances your app further based on geography and lowers hello response time by placing your app closer tooclient browsers.</span></span>

<span data-ttu-id="0b13d-188">V tomto kroku škálování vaší ASP.NET webové aplikace tooa druhý oblasti s [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/).</span><span class="sxs-lookup"><span data-stu-id="0b13d-188">In this step, you scale your ASP.NET web app tooa second region with [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/).</span></span> <span data-ttu-id="0b13d-189">Na konci hello hello kroku budete mít webová aplikace spuštěná v oblasti západní Evropa (již vytvořili) a webová aplikace spuštěná v jihovýchodní Asie (ještě nebyla vytvořena).</span><span class="sxs-lookup"><span data-stu-id="0b13d-189">At hello end of hello step, you will have a web app running in West Europe (already created) and a web app running in Southeast Asia (not yet created).</span></span> <span data-ttu-id="0b13d-190">Obě aplikace se zpracuje z hello stejná adresa URL správce provozu.</span><span class="sxs-lookup"><span data-stu-id="0b13d-190">Both apps will be served from hello same Traffic Manager URL.</span></span>

### <a name="scale-up-hello-europe-app-toostandard-tier"></a><span data-ttu-id="0b13d-191">Škálování hello Evropa aplikace tooStandard vrstvy</span><span class="sxs-lookup"><span data-stu-id="0b13d-191">Scale up hello Europe app tooStandard tier</span></span>
<span data-ttu-id="0b13d-192">Ve službě App Service integrace Azure Traffic Manager vyžaduje hello standardní cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="0b13d-192">In App Service, integration with Azure Traffic Manager requires hello Standard pricing tier.</span></span> <span data-ttu-id="0b13d-193">Použití [aktualizace plánu služby App Service az](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale si vaše tooS1 plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="0b13d-193">Use [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale up your App Service plan tooS1.</span></span> 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --sku S1
```
### <a name="create-a-traffic-manager-profile"></a><span data-ttu-id="0b13d-194">Vytvoření profilu Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="0b13d-194">Create a Traffic Manager profile</span></span> 
<span data-ttu-id="0b13d-195">Použití [vytvořit profil správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) toocreate Traffic Manager profilu a přidejte ji tooyour skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="0b13d-195">Use [az network traffic-manager profile create](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) toocreate a Traffic Manager profile and add it tooyour resource group.</span></span> <span data-ttu-id="0b13d-196">Použijte jedinečný název DNS v $dnsName.</span><span class="sxs-lookup"><span data-stu-id="0b13d-196">Use a unique DNS name in $dnsName.</span></span>

```azurecli
$dnsName = "<replace-with-unique-dns-name>"
az network traffic-manager profile create --name myTrafficManagerProfile --resource-group myResourceGroup --routing-method Performance --unique-dns-name $dnsName
```

> [!NOTE]
> <span data-ttu-id="0b13d-197">`--routing-method Performance`Určuje, že tento profil [směruje uživatele provoz toohello nejbližší endpoint](../traffic-manager/traffic-manager-routing-methods.md).</span><span class="sxs-lookup"><span data-stu-id="0b13d-197">`--routing-method Performance` specifies that this profile [routes user traffic toohello closest endpoint](../traffic-manager/traffic-manager-routing-methods.md).</span></span>

### <a name="get-hello-resource-id-of-hello-europe-app"></a><span data-ttu-id="0b13d-198">Získání ID prostředku hello hello Evropa aplikace</span><span class="sxs-lookup"><span data-stu-id="0b13d-198">Get hello resource ID of hello Europe app</span></span>
<span data-ttu-id="0b13d-199">Použití [az služby App Service web zobrazit](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) ID prostředku hello tooget vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b13d-199">Use [az appservice web show](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) tooget hello resource ID of your web app.</span></span>

```azurecli
$appId = az appservice web show --name $appName --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-hello-europe-app"></a><span data-ttu-id="0b13d-200">Přidat koncový bod Traffic Manager pro aplikaci Evropa hello</span><span class="sxs-lookup"><span data-stu-id="0b13d-200">Add a Traffic Manager endpoint for hello Europe app</span></span>
<span data-ttu-id="0b13d-201">Použít [vytvořit koncový bod správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd tooyour koncový bod profilu služby Traffic Manager a pomocí ID prostředku hello vaší webové aplikace jako hello cíl.</span><span class="sxs-lookup"><span data-stu-id="0b13d-201">Use [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd an endpoint tooyour Traffic Manager profile and use hello resource ID of your web app as hello target.</span></span>

```azurecli
az network traffic-manager endpoint create --name myWestEuropeEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appId
```

### <a name="get-hello-traffic-manager-endpoint-url"></a><span data-ttu-id="0b13d-202">Získat adresu URL koncového bodu hello Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="0b13d-202">Get hello Traffic Manager endpoint URL</span></span>
<span data-ttu-id="0b13d-203">Váš profil Traffic Manageru teď má koncový bod této body tooyour existující webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0b13d-203">Your Traffic Manager profile now has an endpoint that points tooyour existing web app.</span></span> <span data-ttu-id="0b13d-204">Použití [zobrazit profil správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) tooget jeho adresa URL.</span><span class="sxs-lookup"><span data-stu-id="0b13d-204">Use [az network traffic-manager profile show](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) tooget its URL.</span></span> 

```azurecli
az network traffic-manager profile show --name myTrafficManagerProfile --resource-group myResourceGroup --query dnsConfig.fqdn --output tsv
```

<span data-ttu-id="0b13d-205">Kopírovat výstup hello do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0b13d-205">Copy hello output into your browser.</span></span> <span data-ttu-id="0b13d-206">Měli byste vidět vaší webové aplikace znovu.</span><span class="sxs-lookup"><span data-stu-id="0b13d-206">You should see your web app again.</span></span>

### <a name="create-an-azure-redis-cache-in-asia"></a><span data-ttu-id="0b13d-207">Vytvoření Azure Redis Cache v Asii</span><span class="sxs-lookup"><span data-stu-id="0b13d-207">Create an Azure Redis Cache in Asia</span></span>
<span data-ttu-id="0b13d-208">Teď můžete replikaci vaší službě Azure web app toohello jihovýchodní Asie.</span><span class="sxs-lookup"><span data-stu-id="0b13d-208">Now, you replicate your Azure web app toohello Southeast Asia region.</span></span> <span data-ttu-id="0b13d-209">toostart, použijte [vytvořit az redis](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate v jihovýchodní Asie druhý účet Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="0b13d-209">toostart, use [az redis create](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate a second Azure Redis Cache in Southeast Asia.</span></span> <span data-ttu-id="0b13d-210">Tato mezipaměť musí toobe společně umístěné s vaší aplikací v Asii.</span><span class="sxs-lookup"><span data-stu-id="0b13d-210">This cache needs toobe colocated with your app in Asia.</span></span>

```powershell
$redis = (az redis create --name $cacheName-asia --resource-group myResourceGroup --location "Southeast Asia" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

<span data-ttu-id="0b13d-211">`--name $cacheName-asia`poskytuje hello název mezipaměti hello hello západní Evropa mezipaměti s hello `-asia` příponu.</span><span class="sxs-lookup"><span data-stu-id="0b13d-211">`--name $cacheName-asia` gives hello cache hello name of hello West Europe cache, with hello `-asia` suffix.</span></span>

### <a name="create-an-app-service-plan-in-asia"></a><span data-ttu-id="0b13d-212">Vytvořit plán služby App Service v Asii</span><span class="sxs-lookup"><span data-stu-id="0b13d-212">Create an App Service plan in Asia</span></span>
<span data-ttu-id="0b13d-213">Použití [vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) toocreate druhý služby App Service plánování v oblasti hello jihovýchodní Asie, pomocí hello stejné S1 vrstvy jako plán hello západní Evropa.</span><span class="sxs-lookup"><span data-stu-id="0b13d-213">Use [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) toocreate a second App Service plan in hello Southeast Asia region, using hello same S1 tier as hello West Europe plan.</span></span>

```azurecli
az appservice plan create --name myAppServicePlanAsia --resource-group myResourceGroup --location "Southeast Asia" --sku S1
```

### <a name="create-a-web-app-in-asia"></a><span data-ttu-id="0b13d-214">Vytvoření webové aplikace v Asii</span><span class="sxs-lookup"><span data-stu-id="0b13d-214">Create a web app in Asia</span></span>
<span data-ttu-id="0b13d-215">Použití [vytvořit webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate druhý webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b13d-215">Use [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate a second web app.</span></span>

```azurecli
az appservice web create --name $appName-asia --resource-group myResourceGroup --plan myAppServicePlanAsia
```

<span data-ttu-id="0b13d-216">`--name $appName-asia`poskytuje hello aplikace hello název aplikace hello západní Evropa, s hello `-asia` příponu.</span><span class="sxs-lookup"><span data-stu-id="0b13d-216">`--name $appName-asia` gives hello app hello name of hello West Europe app, with hello `-asia` suffix.</span></span>

### <a name="configure-hello-connection-string-for-redis"></a><span data-ttu-id="0b13d-217">Konfigurace hello připojovací řetězec pro Redis</span><span class="sxs-lookup"><span data-stu-id="0b13d-217">Configure hello connection string for Redis</span></span>
<span data-ttu-id="0b13d-218">Použití [aktualizovat az služby App Service web konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd toohello webové aplikace hello připojovací řetězec pro hello mezipaměti jihovýchodní Asie.</span><span class="sxs-lookup"><span data-stu-id="0b13d-218">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd toohello web app hello connection string for hello Southeast Asia cache.</span></span>

<span data-ttu-id="0b13d-219">AZ služby App Service web konfigurace appsettings aktualizovat – nastavení "RedisConnection = $($redis.hostname): $($redis.sslPort), heslo = $($redis.accessKeys.primaryKey), ssl = True, abortConnect = False" – název $appName-Asie – myResourceGroup skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="0b13d-219">az appservice web config appsettings update --settings "RedisConnection=$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False" --name $appName-asia --resource-group myResourceGroup</span></span>

### <a name="configure-git-deployment-for-hello-asia-app"></a><span data-ttu-id="0b13d-220">Nakonfigurujte nasazení Git pro aplikace hello Asie.</span><span class="sxs-lookup"><span data-stu-id="0b13d-220">Configure Git deployment for hello Asia app.</span></span>
<span data-ttu-id="0b13d-221">Použití [az aplikační služby webové správy zdrojového kódu config místní git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure místní nasazení Git pro hello druhý webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0b13d-221">Use [az appservice web source-control config-local-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure local Git deployment for hello second web app.</span></span>

```azurecli
az appservice web source-control config-local-git --name $appName-asia --resource-group myResourceGroup
```

<span data-ttu-id="0b13d-222">Tento příkaz vám poskytuje výstup bude vypadat takto hello následující:</span><span class="sxs-lookup"><span data-stu-id="0b13d-222">This command gives you an output that looks like hello following:</span></span>

```json
{
  "url": "https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git"
}
```

<span data-ttu-id="0b13d-223">Použití hello vrátila adresa URL tooconfigure druhý Git vzdálené pro místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="0b13d-223">Use hello returned URL tooconfigure a second Git remote for your local repository.</span></span> <span data-ttu-id="0b13d-224">Hello následující příkaz použije hello předcházející příklad výstupu.</span><span class="sxs-lookup"><span data-stu-id="0b13d-224">hello following command uses hello preceding output example.</span></span>

```bash
git remote add azure-asia https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-your-sample-application"></a><span data-ttu-id="0b13d-225">Nasazení ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="0b13d-225">Deploy your sample application</span></span>
<span data-ttu-id="0b13d-226">Spustit `git push` toodeploy vaše ukázková aplikace toohello druhý Git vzdálené.</span><span class="sxs-lookup"><span data-stu-id="0b13d-226">Run `git push` toodeploy your sample application toohello second Git remote.</span></span> 

```bash
git push azure-asia master
```

<span data-ttu-id="0b13d-227">Po zobrazení výzvy pro heslo pomocí hello heslo, které jste zadali, když jste spustili `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="0b13d-227">When prompted for password, use hello password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-toohello-asia-app"></a><span data-ttu-id="0b13d-228">Procházet toohello Asie aplikace</span><span class="sxs-lookup"><span data-stu-id="0b13d-228">Browse toohello Asia app</span></span>
<span data-ttu-id="0b13d-229">Použití [procházet webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) tooverify, který vaše aplikace běží za provozu v Azure.</span><span class="sxs-lookup"><span data-stu-id="0b13d-229">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) tooverify that your app is running live in Azure.</span></span>

```azurecli
az appservice web browse --name $appName-asia --resource-group myResourceGroup
```

### <a name="get-hello-resource-id-of-hello-asia-app"></a><span data-ttu-id="0b13d-230">Získání ID prostředku hello hello Asie aplikace</span><span class="sxs-lookup"><span data-stu-id="0b13d-230">Get hello resource ID of hello Asia app</span></span>
<span data-ttu-id="0b13d-231">Použití [az služby App Service web zobrazit](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) ID prostředku hello tooget vaší webové aplikace v jihovýchodní Asie.</span><span class="sxs-lookup"><span data-stu-id="0b13d-231">Use [az appservice web show](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) tooget hello resource ID of your web app in Southeast Asia.</span></span>

```azurecli
$appIdAsia = az appservice web show --name $appName-asia --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-hello-asia-app"></a><span data-ttu-id="0b13d-232">Přidat koncový bod Traffic Manager pro aplikaci Asie hello</span><span class="sxs-lookup"><span data-stu-id="0b13d-232">Add a Traffic Manager endpoint for hello Asia app</span></span>
<span data-ttu-id="0b13d-233">Použití [vytvořit koncový bod správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd druhý toohello koncový bod profilu služby Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="0b13d-233">Use [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd a second endpoint toohello Traffic Manager profile.</span></span>

```azurecli
az network traffic-manager endpoint create --name myAsiaEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appIdAsia
```

### <a name="add-region-identifier-tooweb-apps"></a><span data-ttu-id="0b13d-234">Přidání aplikací tooweb identifikátor oblasti</span><span class="sxs-lookup"><span data-stu-id="0b13d-234">Add region identifier tooweb apps</span></span>
<span data-ttu-id="0b13d-235">Použití [aktualizovat az služby App Service web konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd proměnnou prostředí oblast.</span><span class="sxs-lookup"><span data-stu-id="0b13d-235">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd a region-specific environment variable.</span></span>

```azurecli
az appservice web config appsettings update --settings "Region=West Europe" --name $appName --resource-group myResourceGroup
az appservice web config appsettings update --settings "Region=Southeast Asia" --name $appName-asia --resource-group myResourceGroup
```

<span data-ttu-id="0b13d-236">Kód aplikace už používá toto nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b13d-236">Your application code already uses this application setting.</span></span> <span data-ttu-id="0b13d-237">Podívejte se na `HighScaleApp\Views\Home\Index.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="0b13d-237">Take a look at `HighScaleApp\Views\Home\Index.cshtml`.</span></span>

### <a name="complete"></a><span data-ttu-id="0b13d-238">Dokončení!</span><span class="sxs-lookup"><span data-stu-id="0b13d-238">Complete!</span></span>

<span data-ttu-id="0b13d-239">Nyní zkuste tooaccess hello adresu URL vašeho profilu Traffic Manageru z prohlížečů v různých zeměpisných oblastech.</span><span class="sxs-lookup"><span data-stu-id="0b13d-239">Now, try tooaccess hello URL of your Traffic Manager profile from browsers in different geographical regions.</span></span> <span data-ttu-id="0b13d-240">Prohlížečích klienta z Evropy by měl zobrazit "Evropa – západ ASP.NET" a prohlížeče klienta z Asie by měl zobrazit "ASP.NET jihovýchodní Asie."</span><span class="sxs-lookup"><span data-stu-id="0b13d-240">Client browsers from Europe should show "ASP.NET West Europe", and client browser from Asia should show "ASP.NET Southeast Asia."</span></span>

## <a name="more-resources"></a><span data-ttu-id="0b13d-241">Další zdroje informací</span><span class="sxs-lookup"><span data-stu-id="0b13d-241">More resources</span></span>
