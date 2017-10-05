---
title: "Sestavení flexibilně škálovatelné aplikace v Azure | Microsoft Docs"
description: "Další informace o použití různých služeb Azure maximalizovat výkon aplikace ASP.NET v Azure."
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
ms.openlocfilehash: eac9c5b0d8d0f7802d88e6f4f27d9d23c406e025
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="build-a-hyper-scale-web-app-in-azure"></a><span data-ttu-id="c4f1d-103">Sestavení flexibilně škálovatelné webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="c4f1d-103">Build a hyper-scale web app in Azure</span></span>

<span data-ttu-id="c4f1d-104">V tomto kurzu se dozvíte, jak chcete škálovat webové aplikace ASP.NET v Azure a maximalizovat tak požadavků uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-104">This tutorial shows you how to scale out an ASP.NET web app in Azure to maximize user requests.</span></span>

<span data-ttu-id="c4f1d-105">Před zahájením tohoto kurzu, zajistěte, aby [je nainstalované rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-105">Before starting this tutorial, ensure that [the Azure CLI is installed](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) on your machine.</span></span> <span data-ttu-id="c4f1d-106">Kromě toho musíte [Visual Studio](https://www.visualstudio.com/vs/) na místním počítači ke spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-106">In addition, you need [Visual Studio](https://www.visualstudio.com/vs/) on your local machine to run the sample application.</span></span>

## <a name="step-1---get-sample-application"></a><span data-ttu-id="c4f1d-107">Krok 1 – Get ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="c4f1d-107">Step 1 - Get sample application</span></span>
<span data-ttu-id="c4f1d-108">V tomto kroku nastavíte místní projekt ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-108">In this step, you set up the local ASP.NET project.</span></span>

### <a name="clone-the-application-repository"></a><span data-ttu-id="c4f1d-109">Klonovat úložiště v aplikaci</span><span class="sxs-lookup"><span data-stu-id="c4f1d-109">Clone the application repository</span></span>

<span data-ttu-id="c4f1d-110">Otevřete terminál příkazového řádku podle svého výběru a `CD` do pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-110">Open the command-line terminal of your choice and `CD` to a working directory.</span></span> <span data-ttu-id="c4f1d-111">Potom spusťte následující příkazy a naklonujte ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-111">Then, run the following commands to clone the sample application.</span></span> 

```powershell
git clone https://github.com/cephalin/HighScaleApp.git
```

### <a name="run-the-sample-application-in-visual-studio"></a><span data-ttu-id="c4f1d-112">Spuštění ukázkové aplikace v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4f1d-112">Run the sample application in Visual Studio</span></span>

<span data-ttu-id="c4f1d-113">Otevřete řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-113">Open the solution in Visual Studio.</span></span>

```powershell
cd HighScaleApp
.\HighScaleApp.sln
```

<span data-ttu-id="c4f1d-114">Typ `F5` ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-114">Type `F5` to run the application.</span></span>

<span data-ttu-id="c4f1d-115">Tuto ukázkovou webovou aplikaci ASP.NET pochází z výchozí šablony a přetrvává uživatelské relace a používá výstupní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-115">This sample ASP.NET web application comes from the default template, and persists user sessions and uses the output cache.</span></span> <span data-ttu-id="c4f1d-116">Podívejte se na `HighScaleApp\Controllers\HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-116">Take a look at `HighScaleApp\Controllers\HomeController.cs`.</span></span> <span data-ttu-id="c4f1d-117">`Index()` Metoda přidává část dat do relace.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-117">The `Index()` method adds a piece of data to the session.</span></span>

```csharp
Session.Add("visited", "true"); 
```

<span data-ttu-id="c4f1d-118">A `About()` a `Contact()` metody mezipaměti jejich výstup.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-118">And the `About()` and `Contact()` methods cache their output.</span></span>

```csharp
[OutputCache(Duration = 60)]
```

## <a name="step-2---deploy-to-azure"></a><span data-ttu-id="c4f1d-119">Krok 2 – nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="c4f1d-119">Step 2 - Deploy to Azure</span></span>
<span data-ttu-id="c4f1d-120">V tomto kroku vytvoření webové aplikace Azure a nasadit ukázkovou aplikaci ASP.NET do ní.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-120">In this step, you create an Azure web app and deploy your sample ASP.NET application to it.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="c4f1d-121">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="c4f1d-121">Create a resource group</span></span>   
<span data-ttu-id="c4f1d-122">Použití [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) k vytvoření [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) v oblasti západní Evropa.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-122">Use [az group create](https://docs.microsoft.com/cli/azure/group#create) to create a [resource group](../azure-resource-manager/resource-group-overview.md) in the West Europe region.</span></span> <span data-ttu-id="c4f1d-123">Skupina prostředků je, kam jste umístili všechny prostředky Azure, které chcete spravovat společně, jako jsou webové aplikace a všechny SQL databáze back-end.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-123">A resource group is where you put all the Azure resources that you want to manage together, such as the web app and any SQL Database back end.</span></span>

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

<span data-ttu-id="c4f1d-124">Chcete-li zobrazit, jaké možné hodnoty, které můžete použít pro `---location`, použít [az služby App Service seznamu umístění](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) příkaz.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-124">To see what possible values you can use for `---location`, use the [az appservice list-locations](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) command.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="c4f1d-125">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="c4f1d-125">Create an App Service plan</span></span>
<span data-ttu-id="c4f1d-126">Použití [vytvořit plán aplikační služby az](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) vytvořit "B1" [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c4f1d-126">Use [az appservice plan create](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) to create a "B1" [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span> 

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1
```

<span data-ttu-id="c4f1d-127">Jednotka škálování, která může obsahovat libovolný počet aplikací, které chcete škálovat nahoru nebo si společně přes stejnou infrastrukturu služby App Service je plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-127">An App Service plan is a scale unit, which can include any number of apps that you want to scale up or out together over the same App Service infrastructure.</span></span> <span data-ttu-id="c4f1d-128">Každý plán je také přiřazený [cenová úroveň](https://azure.microsoft.com/en-us/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="c4f1d-128">Each plan is also assigned a [pricing tier](https://azure.microsoft.com/en-us/pricing/details/app-service/).</span></span> <span data-ttu-id="c4f1d-129">Vyšší úrovně zahrnují lepší hardwaru a další funkce, jako je více instancí Škálováním na více systémů.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-129">Higher tiers include better hardware and more features, such as more scale-out instances.</span></span>

<span data-ttu-id="c4f1d-130">V tomto kurzu je B1 minimální úroveň, která umožňuje škálování na tři instance.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-130">For this tutorial, B1 is the minimum tier that enables scale out to three instances.</span></span> <span data-ttu-id="c4f1d-131">Vždy lze přesunout aplikace nahoru nebo dolů cenové úrovně později spuštěním [aktualizace plánu služby App Service az](https://docs.microsoft.com/cli/azure/appservice/plan#update).</span><span class="sxs-lookup"><span data-stu-id="c4f1d-131">You can always move your app up or down the pricing tier later by running [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update).</span></span> 

### <a name="create-a-web-app"></a><span data-ttu-id="c4f1d-132">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c4f1d-132">Create a web app</span></span>
<span data-ttu-id="c4f1d-133">Použití [vytvořit webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) k vytvoření webové aplikace s jedinečným názvem v `$appName`.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-133">Use [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) to create a web app with a unique name in `$appName`.</span></span>

```azurecli
$appName = "<replace-with-a-unique-name>"
az appservice web create --name $appName --resource-group myResourceGroup --plan myAppServicePlan
```

### <a name="set-deployment-credentials"></a><span data-ttu-id="c4f1d-134">Nastavení přihlašovacích údajů nasazení</span><span class="sxs-lookup"><span data-stu-id="c4f1d-134">Set deployment credentials</span></span>
<span data-ttu-id="c4f1d-135">Použití [nastavený uživatel nasazení webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) a nastavte přihlašovací údaje nasazení úrovni účtu pro službu App Service.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-135">Use [az appservice web deployment user set](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) to set your account-level deployment credentials for App Service.</span></span>

```azurecli
az appservice web deployment user set --user-name <letters-numbers> --password <mininum-8-char-captital-lowercase-letters-numbers>
```

### <a name="configure-git-deployment"></a><span data-ttu-id="c4f1d-136">Konfigurace nasazení Git</span><span class="sxs-lookup"><span data-stu-id="c4f1d-136">Configure Git deployment</span></span>
<span data-ttu-id="c4f1d-137">Použití [az aplikační služby webové správy zdrojového kódu config místní git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) nakonfigurovat místní nasazení Git.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-137">Use [az appservice web source-control config-local-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) to configure local Git deployment.</span></span>

```azurecli
az appservice web source-control config-local-git --name $appName --resource-group myResourceGroup
```

<span data-ttu-id="c4f1d-138">Tento příkaz vám poskytuje výstup, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c4f1d-138">This command gives you an output that looks like the following:</span></span>

```json
{
  "url": "https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git"
}
```

<span data-ttu-id="c4f1d-139">Použijte vrácený adresu URL ke konfiguraci vaší vzdálené Git.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-139">Use the returned URL to configure your Git remote.</span></span> <span data-ttu-id="c4f1d-140">Následující příkaz použije v předchozím příkladu výstupu.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-140">The following command uses the preceding output example.</span></span>

```powershell
git remote add azure https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-the-sample-application"></a><span data-ttu-id="c4f1d-141">Nasazení ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="c4f1d-141">Deploy the sample application</span></span>
<span data-ttu-id="c4f1d-142">Teď jste připravení nasadit ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-142">You are now ready to deploy your sample application.</span></span> <span data-ttu-id="c4f1d-143">Spusťte `git push`.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-143">Run `git push`.</span></span>

```powershell
git push azure master
```

<span data-ttu-id="c4f1d-144">Po zobrazení výzvy pro heslo, používat heslo, které jste zadali, když jste spustili `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-144">When prompted for password, use the password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-to-azure-web-app"></a><span data-ttu-id="c4f1d-145">Přejděte do webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="c4f1d-145">Browse to Azure web app</span></span>
<span data-ttu-id="c4f1d-146">Použití [procházet webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) Chcete-li sledovat živý běh v Azure, spusťte tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-146">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) to see your app running live in Azure, run this command.</span></span>

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-3---connect-to-redis"></a><span data-ttu-id="c4f1d-147">Krok 3 – připojení k Redis</span><span class="sxs-lookup"><span data-stu-id="c4f1d-147">Step 3 - Connect to Redis</span></span>
<span data-ttu-id="c4f1d-148">V tomto kroku nastavíte Azure Redis Cache jako externí, společně umístěných mezipaměti do Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-148">In this step, you set up Azure Redis Cache as an external, colocated cache to your Azure web app.</span></span> <span data-ttu-id="c4f1d-149">Můžete rychle využít Redis pro ukládání do mezipaměti výstupu stránky.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-149">You can quickly utilize Redis to cache your page output.</span></span> <span data-ttu-id="c4f1d-150">Kromě toho, když jste škálovat webové aplikace později, Redis vám pomůže zachovat uživatelských relací napříč více instancí spolehlivě.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-150">In addition, when you scale out your web apps later, Redis helps you persist user sessions across multiple instances reliably.</span></span>

### <a name="create-an-azure-redis-cache"></a><span data-ttu-id="c4f1d-151">Vytvoření Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="c4f1d-151">Create an Azure Redis Cache</span></span>
<span data-ttu-id="c4f1d-152">Použití [vytvořit az redis](https://docs.microsoft.com/en-us/cli/azure/redis#create) vytvořit Azure Redis Cache a uložit ve formátu JSON výstupu.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-152">Use [az redis create](https://docs.microsoft.com/en-us/cli/azure/redis#create) to create an Azure Redis Cache and save the JSON output.</span></span> <span data-ttu-id="c4f1d-153">Použijte jedinečný název v `$cacheName`.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-153">Use a unique name in `$cacheName`.</span></span>

```powershell
$cacheName = "<replace-with-a-unique-cache-name>"
$redis = (az redis create --name $cacheName --resource-group myResourceGroup --location "West Europe" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

### <a name="configure-the-application-to-use-redis"></a><span data-ttu-id="c4f1d-154">Konfigurace aplikace pro používání Redis</span><span class="sxs-lookup"><span data-stu-id="c4f1d-154">Configure the application to use Redis</span></span>
<span data-ttu-id="c4f1d-155">Formátování řetězce připojení ke svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-155">Format the connection string for your cache.</span></span>

```powershell
$connstring = "$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False"
$connstring 
```

<span data-ttu-id="c4f1d-156">Druhý řádek měl dát výstupu, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c4f1d-156">The second line should give you an output that looks like this:</span></span>

```
mycachename.redis.cache.windows.net:6380,password=/rQP/TLz1mrEPpmh9b/gnfns/t9vBRXqXn3i1RwBjGA=,ssl=True,abortConnect=False
```

<span data-ttu-id="c4f1d-157">V sadě Visual Studio vytvořit soubor webové konfigurace v kořenového adresáře projektu názvem `redis.config` a vložte následující kód do ní.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-157">In Visual Studio, create a web configuration file in your project root called `redis.config` and paste the following code into it.</span></span> <span data-ttu-id="c4f1d-158">V `value`, použít připojovací řetězec z výstupu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-158">In `value`, use the connection string from the PowerShell output.</span></span>

```xml
<appSettings>
  <add key="RedisConnection" value="your-azure-redis-cache-connection-string"/>
</appSettings>
```

<span data-ttu-id="c4f1d-159">Pokud se podíváte `.gitignore` souboru v úložiště Git, zobrazí se, že tento soubor je vyloučen z řízení zdrojů.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-159">If you look at the `.gitignore` file in your Git repository, you'll see that this file is excluded from source control.</span></span> <span data-ttu-id="c4f1d-160">Tímto způsobem je udržováno bezpečné vaše citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-160">That way your sensitive information is kept safe.</span></span> 

<span data-ttu-id="c4f1d-161">Otevřete `Web.config`.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-161">Open `Web.config`.</span></span> <span data-ttu-id="c4f1d-162">Upozornění `<appSettings file="redis.config">` element, který získá nastavení, které jste vytvořili v `redis.config`.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-162">Notice the `<appSettings file="redis.config">` element, which gets the setting you created in `redis.config`.</span></span> 

<span data-ttu-id="c4f1d-163">Najít komentáři oddíl, který zahrnuje `<sessionState>` a `<caching>`.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-163">Find the commented section that includes `<sessionState>` and `<caching>`.</span></span> <span data-ttu-id="c4f1d-164">Zrušením komentáře u této části.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-164">Uncomment this section.</span></span>

![](./media/app-service-web-tutorial-hyper-scale-app/redisproviders.png)

<span data-ttu-id="c4f1d-165">Tento kód vyhledá Redis připojovacího řetězce je definována v `RedisConnection`.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-165">This code looks for the Redis connection string you defined in `RedisConnection`.</span></span> 

<span data-ttu-id="c4f1d-166">Teď vaše aplikace používá ke správě relací a ukládání do mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-166">Your application now uses Redis to manage sessions and caching.</span></span> <span data-ttu-id="c4f1d-167">Typ `F5` ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-167">Type `F5` to run the application.</span></span> <span data-ttu-id="c4f1d-168">Pokud chcete můžete si stáhnout klienta správy Redis k vizualizaci dat, která jsou nyní uloženy do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-168">If you like, you can download a Redis management client to visualize the data that is now saved to the cache.</span></span>

### <a name="configure-the-connection-string-in-azure"></a><span data-ttu-id="c4f1d-169">Konfigurovat připojovací řetězec v Azure</span><span class="sxs-lookup"><span data-stu-id="c4f1d-169">Configure the connection string in Azure</span></span>

<span data-ttu-id="c4f1d-170">Pro aplikace pro práci v Azure budete muset nakonfigurovat do jednoho připojovacího řetězce Redis v Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-170">For your application to work in Azure, you need to configure the same Redis connection string in your Azure web app.</span></span> <span data-ttu-id="c4f1d-171">Vzhledem k tomu `redis.config` nezachová ve správě zdrojového kódu není nasazena do Azure, když spustíte nasazení Git.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-171">Since `redis.config` is not maintained in source control, it is not deployed to Azure when you run Git deployment.</span></span>

<span data-ttu-id="c4f1d-172">Použití [aktualizovat az služby App Service web konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) přidat řetězec připojení se stejným názvem (`RedisConnection`).</span><span class="sxs-lookup"><span data-stu-id="c4f1d-172">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) to add the connection string with the same name (`RedisConnection`).</span></span>

<span data-ttu-id="c4f1d-173">AZ služby App Service web konfigurace appsettings aktualizovat – nastavení "RedisConnection = $connstring" – název $appName – myResourceGroup skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="c4f1d-173">az appservice web config appsettings update --settings "RedisConnection=$connstring" --name $appName --resource-group myResourceGroup</span></span>

<span data-ttu-id="c4f1d-174">Nezapomeňte, že `$connstring` obsahuje formátovaný připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-174">Remember that `$connstring` contains the formatted connection string.</span></span>

### <a name="redeploy-the-application-to-azure"></a><span data-ttu-id="c4f1d-175">Znovu nasaďte aplikaci do Azure</span><span class="sxs-lookup"><span data-stu-id="c4f1d-175">Redeploy the application to Azure</span></span>
<span data-ttu-id="c4f1d-176">Použití příkazu Git push změny do Azure</span><span class="sxs-lookup"><span data-stu-id="c4f1d-176">Use Git commands to push your changes to Azure</span></span>

```bash
git add .
git commit -m "now use Redis providers"
git push azure master
```

<span data-ttu-id="c4f1d-177">Po zobrazení výzvy pro heslo, používat heslo, které jste zadali, když jste spustili `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-177">When prompted for password, use the password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-to-the-azure-web-app"></a><span data-ttu-id="c4f1d-178">Přejděte do webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="c4f1d-178">Browse to the Azure web app</span></span>
<span data-ttu-id="c4f1d-179">Použití [procházet webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) a podívejte se změny za provozu v Azure.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-179">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) to see the changes live in Azure.</span></span>

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-4---scale-to-multiple-instances"></a><span data-ttu-id="c4f1d-180">Krok 4 – Škálováním na více instancí</span><span class="sxs-lookup"><span data-stu-id="c4f1d-180">Step 4 - Scale to multiple instances</span></span>
<span data-ttu-id="c4f1d-181">Plán služby App Service je jednotka škálování pro vaše webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-181">The App Service plan is the scale unit for your Azure web apps.</span></span> <span data-ttu-id="c4f1d-182">Pro rozšíření Škálováním vaší webové aplikace, můžete škálovat plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-182">To scale out your web app, you scale the App Service plan.</span></span>

<span data-ttu-id="c4f1d-183">Použití [aktualizace plánu služby App Service az](https://docs.microsoft.com/cli/azure/appservice/plan#update) škálovat plán služby App Service na tři instance, což je maximální počet povolené B1 cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-183">Use [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update) to scale out the App Service plan to three instances, which is the maximum number allowed by the B1 pricing tier.</span></span> <span data-ttu-id="c4f1d-184">Mějte na paměti, že je B1 cenovou úroveň, které jste zvolili při vytváření plánu služby App Service dříve.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-184">Remember that B1 is the pricing tier that you chose when you created the App Service plan earlier.</span></span> 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --number-of-workers 3 
```

## <a name="step-5---scale-geographically"></a><span data-ttu-id="c4f1d-185">Krok 5 – škálování geograficky</span><span class="sxs-lookup"><span data-stu-id="c4f1d-185">Step 5 - Scale geographically</span></span>
<span data-ttu-id="c4f1d-186">Při zvětšení velikosti geograficky, spusťte aplikaci v několika oblastí cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-186">When scaling geographically, you run your app in multiple regions of the Azure cloud.</span></span> <span data-ttu-id="c4f1d-187">Tento instalační program zatížení zůstatky aplikace další podle Geografie a snižuje dobu odezvy tím, že vaše aplikace blíže do klientských prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-187">This setup load-balances your app further based on geography and lowers the response time by placing your app closer to client browsers.</span></span>

<span data-ttu-id="c4f1d-188">V tomto kroku škálování webové aplikace ASP.NET v druhé oblasti s [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/).</span><span class="sxs-lookup"><span data-stu-id="c4f1d-188">In this step, you scale your ASP.NET web app to a second region with [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/).</span></span> <span data-ttu-id="c4f1d-189">Na konci tohoto kroku budete mít webová aplikace spuštěná v oblasti západní Evropa (již vytvořili) a webová aplikace spuštěná v jihovýchodní Asie (ještě nebyla vytvořena).</span><span class="sxs-lookup"><span data-stu-id="c4f1d-189">At the end of the step, you will have a web app running in West Europe (already created) and a web app running in Southeast Asia (not yet created).</span></span> <span data-ttu-id="c4f1d-190">Z TÉŽE Traffic Manager se zpracuje obě aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-190">Both apps will be served from the same Traffic Manager URL.</span></span>

### <a name="scale-up-the-europe-app-to-standard-tier"></a><span data-ttu-id="c4f1d-191">Škálování aplikace Evropa úrovně Standard</span><span class="sxs-lookup"><span data-stu-id="c4f1d-191">Scale up the Europe app to Standard tier</span></span>
<span data-ttu-id="c4f1d-192">Ve službě App Service integrace Azure Traffic Manager vyžaduje standardní cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-192">In App Service, integration with Azure Traffic Manager requires the Standard pricing tier.</span></span> <span data-ttu-id="c4f1d-193">Použití [aktualizace plánu služby App Service az](https://docs.microsoft.com/cli/azure/appservice/plan#update) škálování vašeho plánu služby App Service S1.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-193">Use [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update) to scale up your App Service plan to S1.</span></span> 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --sku S1
```
### <a name="create-a-traffic-manager-profile"></a><span data-ttu-id="c4f1d-194">Vytvoření profilu Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="c4f1d-194">Create a Traffic Manager profile</span></span> 
<span data-ttu-id="c4f1d-195">Použití [vytvořit profil správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) k vytvoření profilu Traffic Manageru a přidat jej do vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-195">Use [az network traffic-manager profile create](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) to create a Traffic Manager profile and add it to your resource group.</span></span> <span data-ttu-id="c4f1d-196">Použijte jedinečný název DNS v $dnsName.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-196">Use a unique DNS name in $dnsName.</span></span>

```azurecli
$dnsName = "<replace-with-unique-dns-name>"
az network traffic-manager profile create --name myTrafficManagerProfile --resource-group myResourceGroup --routing-method Performance --unique-dns-name $dnsName
```

> [!NOTE]
> <span data-ttu-id="c4f1d-197">`--routing-method Performance`Určuje, že tento profil [směruje provoz uživatele na nejbližší koncový bod](../traffic-manager/traffic-manager-routing-methods.md).</span><span class="sxs-lookup"><span data-stu-id="c4f1d-197">`--routing-method Performance` specifies that this profile [routes user traffic to the closest endpoint](../traffic-manager/traffic-manager-routing-methods.md).</span></span>

### <a name="get-the-resource-id-of-the-europe-app"></a><span data-ttu-id="c4f1d-198">Získání ID prostředku aplikace Evropa</span><span class="sxs-lookup"><span data-stu-id="c4f1d-198">Get the resource ID of the Europe app</span></span>
<span data-ttu-id="c4f1d-199">Použití [az služby App Service web zobrazit](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) získat ID prostředku vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-199">Use [az appservice web show](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) to get the resource ID of your web app.</span></span>

```azurecli
$appId = az appservice web show --name $appName --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-the-europe-app"></a><span data-ttu-id="c4f1d-200">Přidat koncový bod Traffic Manager pro aplikaci Evropa</span><span class="sxs-lookup"><span data-stu-id="c4f1d-200">Add a Traffic Manager endpoint for the Europe app</span></span>
<span data-ttu-id="c4f1d-201">Použít [vytvořit koncový bod správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) přidání koncového bodu pro svůj profil Traffic Manageru a pomocí ID prostředků vaší webové aplikace jako cíl.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-201">Use [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) to add an endpoint to your Traffic Manager profile and use the resource ID of your web app as the target.</span></span>

```azurecli
az network traffic-manager endpoint create --name myWestEuropeEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appId
```

### <a name="get-the-traffic-manager-endpoint-url"></a><span data-ttu-id="c4f1d-202">Získat adresu URL koncového bodu Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="c4f1d-202">Get the Traffic Manager endpoint URL</span></span>
<span data-ttu-id="c4f1d-203">Váš profil Traffic Manageru teď má koncový bod, který ukazuje na existující webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-203">Your Traffic Manager profile now has an endpoint that points to your existing web app.</span></span> <span data-ttu-id="c4f1d-204">Použití [zobrazit profil správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) získat jeho adresa URL.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-204">Use [az network traffic-manager profile show](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) to get its URL.</span></span> 

```azurecli
az network traffic-manager profile show --name myTrafficManagerProfile --resource-group myResourceGroup --query dnsConfig.fqdn --output tsv
```

<span data-ttu-id="c4f1d-205">Zkopírujte výstup do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-205">Copy the output into your browser.</span></span> <span data-ttu-id="c4f1d-206">Měli byste vidět vaší webové aplikace znovu.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-206">You should see your web app again.</span></span>

### <a name="create-an-azure-redis-cache-in-asia"></a><span data-ttu-id="c4f1d-207">Vytvoření Azure Redis Cache v Asii</span><span class="sxs-lookup"><span data-stu-id="c4f1d-207">Create an Azure Redis Cache in Asia</span></span>
<span data-ttu-id="c4f1d-208">Teď můžete replikaci vaší webové aplikace Azure oblast, jihovýchodní Asie.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-208">Now, you replicate your Azure web app to the Southeast Asia region.</span></span> <span data-ttu-id="c4f1d-209">Chcete-li začít, použijte [vytvořit az redis](https://docs.microsoft.com/en-us/cli/azure/redis#create) vytvoření druhého Azure Redis Cache v jihovýchodní Asie.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-209">To start, use [az redis create](https://docs.microsoft.com/en-us/cli/azure/redis#create) to create a second Azure Redis Cache in Southeast Asia.</span></span> <span data-ttu-id="c4f1d-210">Tato mezipaměť musí být umístěna společně s vaší aplikací v Asii.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-210">This cache needs to be colocated with your app in Asia.</span></span>

```powershell
$redis = (az redis create --name $cacheName-asia --resource-group myResourceGroup --location "Southeast Asia" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

<span data-ttu-id="c4f1d-211">`--name $cacheName-asia`poskytuje název mezipaměti západní Evropa, mezipaměti s `-asia` příponu.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-211">`--name $cacheName-asia` gives the cache the name of the West Europe cache, with the `-asia` suffix.</span></span>

### <a name="create-an-app-service-plan-in-asia"></a><span data-ttu-id="c4f1d-212">Vytvořit plán služby App Service v Asii</span><span class="sxs-lookup"><span data-stu-id="c4f1d-212">Create an App Service plan in Asia</span></span>
<span data-ttu-id="c4f1d-213">Použití [vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) vytvoření druhého plán služby App Service v oblasti jihovýchodní Asie, pomocí stejné vrstvě S1 jako plán západní Evropa.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-213">Use [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) to create a second App Service plan in the Southeast Asia region, using the same S1 tier as the West Europe plan.</span></span>

```azurecli
az appservice plan create --name myAppServicePlanAsia --resource-group myResourceGroup --location "Southeast Asia" --sku S1
```

### <a name="create-a-web-app-in-asia"></a><span data-ttu-id="c4f1d-214">Vytvoření webové aplikace v Asii</span><span class="sxs-lookup"><span data-stu-id="c4f1d-214">Create a web app in Asia</span></span>
<span data-ttu-id="c4f1d-215">Použití [vytvořit webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) k vytvoření druhého webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-215">Use [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) to create a second web app.</span></span>

```azurecli
az appservice web create --name $appName-asia --resource-group myResourceGroup --plan myAppServicePlanAsia
```

<span data-ttu-id="c4f1d-216">`--name $appName-asia`poskytuje název aplikace západní Evropa, aplikace se `-asia` příponu.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-216">`--name $appName-asia` gives the app the name of the West Europe app, with the `-asia` suffix.</span></span>

### <a name="configure-the-connection-string-for-redis"></a><span data-ttu-id="c4f1d-217">Konfigurace připojovacího řetězce pro Redis</span><span class="sxs-lookup"><span data-stu-id="c4f1d-217">Configure the connection string for Redis</span></span>
<span data-ttu-id="c4f1d-218">Použití [aktualizovat az služby App Service web konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) do webové aplikace přidat připojovací řetězec pro mezipaměť jihovýchodní Asie.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-218">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) to add to the web app the connection string for the Southeast Asia cache.</span></span>

<span data-ttu-id="c4f1d-219">AZ služby App Service web konfigurace appsettings aktualizovat – nastavení "RedisConnection = $($redis.hostname): $($redis.sslPort), heslo = $($redis.accessKeys.primaryKey), ssl = True, abortConnect = False" – název $appName-Asie – myResourceGroup skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="c4f1d-219">az appservice web config appsettings update --settings "RedisConnection=$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False" --name $appName-asia --resource-group myResourceGroup</span></span>

### <a name="configure-git-deployment-for-the-asia-app"></a><span data-ttu-id="c4f1d-220">Nakonfigurujte nasazení Git pro aplikaci Asie.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-220">Configure Git deployment for the Asia app.</span></span>
<span data-ttu-id="c4f1d-221">Použití [az aplikační služby webové správy zdrojového kódu config místní git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) nakonfigurovat místní nasazení Git pro druhý webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-221">Use [az appservice web source-control config-local-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) to configure local Git deployment for the second web app.</span></span>

```azurecli
az appservice web source-control config-local-git --name $appName-asia --resource-group myResourceGroup
```

<span data-ttu-id="c4f1d-222">Tento příkaz vám poskytuje výstup, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c4f1d-222">This command gives you an output that looks like the following:</span></span>

```json
{
  "url": "https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git"
}
```

<span data-ttu-id="c4f1d-223">Použijte vrácený adresu URL ke konfiguraci druhý vzdálené řízení Git pro místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-223">Use the returned URL to configure a second Git remote for your local repository.</span></span> <span data-ttu-id="c4f1d-224">Následující příkaz použije v předchozím příkladu výstupu.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-224">The following command uses the preceding output example.</span></span>

```bash
git remote add azure-asia https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-your-sample-application"></a><span data-ttu-id="c4f1d-225">Nasazení ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="c4f1d-225">Deploy your sample application</span></span>
<span data-ttu-id="c4f1d-226">Spustit `git push` nasadit ukázkovou aplikaci pro druhý vzdálené úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-226">Run `git push` to deploy your sample application to the second Git remote.</span></span> 

```bash
git push azure-asia master
```

<span data-ttu-id="c4f1d-227">Po zobrazení výzvy pro heslo, používat heslo, které jste zadali, když jste spustili `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-227">When prompted for password, use the password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-to-the-asia-app"></a><span data-ttu-id="c4f1d-228">Přejděte do aplikace Asie</span><span class="sxs-lookup"><span data-stu-id="c4f1d-228">Browse to the Asia app</span></span>
<span data-ttu-id="c4f1d-229">Použití [procházet webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) k ověření, že vaše aplikace běží za provozu v Azure.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-229">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) to verify that your app is running live in Azure.</span></span>

```azurecli
az appservice web browse --name $appName-asia --resource-group myResourceGroup
```

### <a name="get-the-resource-id-of-the-asia-app"></a><span data-ttu-id="c4f1d-230">Získání ID prostředku aplikace Asie</span><span class="sxs-lookup"><span data-stu-id="c4f1d-230">Get the resource ID of the Asia app</span></span>
<span data-ttu-id="c4f1d-231">Použití [az služby App Service web zobrazit](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) k získání ID prostředku vaší webové aplikace v jihovýchodní Asie.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-231">Use [az appservice web show](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) to get the resource ID of your web app in Southeast Asia.</span></span>

```azurecli
$appIdAsia = az appservice web show --name $appName-asia --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-the-asia-app"></a><span data-ttu-id="c4f1d-232">Přidat koncový bod Traffic Manager pro aplikaci Asie</span><span class="sxs-lookup"><span data-stu-id="c4f1d-232">Add a Traffic Manager endpoint for the Asia app</span></span>
<span data-ttu-id="c4f1d-233">Použití [vytvořit koncový bod správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) přidat druhý koncový bod profilu Správce provozu.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-233">Use [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) to add a second endpoint to the Traffic Manager profile.</span></span>

```azurecli
az network traffic-manager endpoint create --name myAsiaEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appIdAsia
```

### <a name="add-region-identifier-to-web-apps"></a><span data-ttu-id="c4f1d-234">Přidejte identifikátor oblasti do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c4f1d-234">Add region identifier to web apps</span></span>
<span data-ttu-id="c4f1d-235">Použití [aktualizovat az služby App Service web konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) k přidání proměnné prostředí oblast.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-235">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) to add a region-specific environment variable.</span></span>

```azurecli
az appservice web config appsettings update --settings "Region=West Europe" --name $appName --resource-group myResourceGroup
az appservice web config appsettings update --settings "Region=Southeast Asia" --name $appName-asia --resource-group myResourceGroup
```

<span data-ttu-id="c4f1d-236">Kód aplikace už používá toto nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-236">Your application code already uses this application setting.</span></span> <span data-ttu-id="c4f1d-237">Podívejte se na `HighScaleApp\Views\Home\Index.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-237">Take a look at `HighScaleApp\Views\Home\Index.cshtml`.</span></span>

### <a name="complete"></a><span data-ttu-id="c4f1d-238">Dokončení!</span><span class="sxs-lookup"><span data-stu-id="c4f1d-238">Complete!</span></span>

<span data-ttu-id="c4f1d-239">Nyní pokusu o přístup k adresu URL vašeho profilu Traffic Manageru z prohlížečů v různých zeměpisných oblastech.</span><span class="sxs-lookup"><span data-stu-id="c4f1d-239">Now, try to access the URL of your Traffic Manager profile from browsers in different geographical regions.</span></span> <span data-ttu-id="c4f1d-240">Prohlížečích klienta z Evropy by měl zobrazit "Evropa – západ ASP.NET" a prohlížeče klienta z Asie by měl zobrazit "ASP.NET jihovýchodní Asie."</span><span class="sxs-lookup"><span data-stu-id="c4f1d-240">Client browsers from Europe should show "ASP.NET West Europe", and client browser from Asia should show "ASP.NET Southeast Asia."</span></span>

## <a name="more-resources"></a><span data-ttu-id="c4f1d-241">Další zdroje informací</span><span class="sxs-lookup"><span data-stu-id="c4f1d-241">More resources</span></span>
