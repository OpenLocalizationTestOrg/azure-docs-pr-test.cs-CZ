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
# <a name="build-a-hyper-scale-web-app-in-azure"></a>Sestavení flexibilně škálovatelné webové aplikace v Azure

Tento kurz ukazuje, jak požaduje tooscale se webové aplikace ASP.NET v Azure toomaximize uživatele.

Před zahájením tohoto kurzu, zajistěte, aby [hello je nainstalované rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) na váš počítač. Kromě toho musíte [Visual Studio](https://www.visualstudio.com/vs/) na místním počítači toorun hello ukázkovou aplikaci.

## <a name="step-1---get-sample-application"></a>Krok 1 – Get ukázkové aplikace
V tomto kroku nastavíte místní projekt ASP.NET hello.

### <a name="clone-hello-application-repository"></a>Klon hello aplikace úložiště

Otevřete hello terminál příkazového řádku podle svého výběru a `CD` tooa pracovní adresář. Potom spusťte hello následující příkazy tooclone hello ukázkovou aplikaci. 

```powershell
git clone https://github.com/cephalin/HighScaleApp.git
```

### <a name="run-hello-sample-application-in-visual-studio"></a>Spustit hello ukázkovou aplikaci v sadě Visual Studio

Otevřete hello řešení v sadě Visual Studio.

```powershell
cd HighScaleApp
.\HighScaleApp.sln
```

Typ `F5` toorun hello aplikace.

Tato ukázka webové aplikace ASP.NET pochází z hello výchozí šablonu a trvá uživatelské relace a používá hello výstupní mezipaměti. Podívejte se na `HighScaleApp\Controllers\HomeController.cs`. Hello `Index()` metoda přidá část dat toohello relace.

```csharp
Session.Add("visited", "true"); 
```

A hello `About()` a `Contact()` metody mezipaměti jejich výstup.

```csharp
[OutputCache(Duration = 60)]
```

## <a name="step-2---deploy-tooazure"></a>Krok 2 – nasazení tooAzure
V tomto kroku vytvoření webové aplikace Azure a nasazení vaší tooit aplikace ASP.NET ukázka.

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků   
Použití [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) toocreate [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) v oblasti západní Evropa hello. Skupina prostředků je, kam jste umístili všechny hello prostředky Azure, které chcete toomanage společně, například hello webovou aplikaci a všechny SQL databáze back-end.

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

toosee jaké možné hodnoty, které můžete použít pro `---location`, použijte hello [míst seznamu služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) příkaz.

### <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service
Použití [vytvořit plán aplikační služby az](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) toocreate "B1" [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1
```

Plán služby App Service je jednotka škálování, která může obsahovat libovolný počet aplikací, které chcete tooscale si nebo si společně over hello stejné infrastruktury služby App Service. Každý plán je také přiřazený [cenová úroveň](https://azure.microsoft.com/en-us/pricing/details/app-service/). Vyšší úrovně zahrnují lepší hardwaru a další funkce, jako je více instancí Škálováním na více systémů.

V tomto kurzu je B1 hello minimální vrstvy, která umožňuje škálování toothree instance. Aplikace může vždy přesunout nahoru nebo dolů hello později cenová úroveň spuštěním [aktualizace plánu služby App Service az](https://docs.microsoft.com/cli/azure/appservice/plan#update). 

### <a name="create-a-web-app"></a>Vytvoření webové aplikace
Použití [vytvořit webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate webovou aplikaci s jedinečným názvem v `$appName`.

```azurecli
$appName = "<replace-with-a-unique-name>"
az appservice web create --name $appName --resource-group myResourceGroup --plan myAppServicePlan
```

### <a name="set-deployment-credentials"></a>Nastavení přihlašovacích údajů nasazení
Použití [nastavený uživatel nasazení webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) tooset nasazením úrovni účtu přihlašovací údaje pro službu App Service.

```azurecli
az appservice web deployment user set --user-name <letters-numbers> --password <mininum-8-char-captital-lowercase-letters-numbers>
```

### <a name="configure-git-deployment"></a>Konfigurace nasazení Git
Použití [az aplikační služby webové správy zdrojového kódu config místní git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure místní nasazení Git.

```azurecli
az appservice web source-control config-local-git --name $appName --resource-group myResourceGroup
```

Tento příkaz vám poskytuje výstup bude vypadat takto hello následující:

```json
{
  "url": "https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git"
}
```

Použití hello vrátil tooconfigure adresu URL vašeho Git vzdálené. Hello následující příkaz použije hello předcházející příklad výstupu.

```powershell
git remote add azure https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-hello-sample-application"></a>Nasazení ukázkové aplikace hello
Můžete je nyní připraven toodeploy ukázkovou aplikaci. Spusťte `git push`.

```powershell
git push azure master
```

Po zobrazení výzvy pro heslo pomocí hello heslo, které jste zadali, když jste spustili `az appservice web deployment user set`.

### <a name="browse-tooazure-web-app"></a>Procházet tooAzure webové aplikace
Použití [procházet webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee aplikace živý běh v Azure, spusťte tento příkaz.

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-3---connect-tooredis"></a>Krok 3 – připojit tooRedis
V tomto kroku nastavíte Azure Redis Cache jako externí, společně umístěných mezipaměti tooyour webové aplikace Azure. Můžete rychle využít Redis toocache výstupu stránky. Kromě toho, když jste škálovat webové aplikace později, Redis vám pomůže zachovat uživatelských relací napříč více instancí spolehlivě.

### <a name="create-an-azure-redis-cache"></a>Vytvoření Azure Redis Cache
Použití [vytvořit az redis](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate mezipaměti Azure Redis a uložte hello výstup JSON. Použijte jedinečný název v `$cacheName`.

```powershell
$cacheName = "<replace-with-a-unique-cache-name>"
$redis = (az redis create --name $cacheName --resource-group myResourceGroup --location "West Europe" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

### <a name="configure-hello-application-toouse-redis"></a>Konfigurace aplikace toouse hello Redis
Formát hello připojovací řetězec pro mezipaměť.

```powershell
$connstring = "$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False"
$connstring 
```

druhý řádek Hello měl dát výstupu, který vypadá takto:

```
mycachename.redis.cache.windows.net:6380,password=/rQP/TLz1mrEPpmh9b/gnfns/t9vBRXqXn3i1RwBjGA=,ssl=True,abortConnect=False
```

V sadě Visual Studio vytvořit soubor webové konfigurace v kořenového adresáře projektu názvem `redis.config` a hello vložte následující kód do ní. V `value`, použít hello připojovací řetězec z hello výstup prostředí PowerShell.

```xml
<appSettings>
  <add key="RedisConnection" value="your-azure-redis-cache-connection-string"/>
</appSettings>
```

Pokud si prohlédnete hello `.gitignore` souboru v úložiště Git, zobrazí se, že tento soubor je vyloučen z řízení zdrojů. Tímto způsobem je udržováno bezpečné vaše citlivé informace. 

Otevřete `Web.config`. Všimněte si hello `<appSettings file="redis.config">` element, který získá hello nastavení, které jste vytvořili v `redis.config`. 

Najít označeno jako komentář hello oddíl, který zahrnuje `<sessionState>` a `<caching>`. Zrušením komentáře u této části.

![](./media/app-service-web-tutorial-hyper-scale-app/redisproviders.png)

Tento kód vyhledá hello Redis připojovacího řetězce je definována v `RedisConnection`. 

Teď vaše aplikace používá Redis toomanage relace a ukládání do mezipaměti. Typ `F5` toorun hello aplikace. Pokud chcete můžete si stáhnout Redis správy klienta toovisualize hello data, která jsou nyní uloženy toohello mezipaměti.

### <a name="configure-hello-connection-string-in-azure"></a>Konfigurace hello připojovací řetězec v Azure

Pro vaše aplikace toowork v Azure, budete potřebovat tooconfigure hello stejný připojovací řetězec Redis v Azure webové aplikace. Vzhledem k tomu `redis.config` nezachová ve správě zdrojového kódu není nasazený tooAzure při spuštění nasazení Git.

Použití [aktualizovat az služby App Service web konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd hello připojovací řetězec s hello stejný název (`RedisConnection`).

AZ služby App Service web konfigurace appsettings aktualizovat – nastavení "RedisConnection = $connstring" – název $appName – myResourceGroup skupiny prostředků

Nezapomeňte, že `$connstring` obsahuje hello formátu připojovací řetězec.

### <a name="redeploy-hello-application-tooazure"></a>Znovu nasaďte tooAzure aplikace hello
Použít toopush příkazy Git tooAzure vaše změny

```bash
git add .
git commit -m "now use Redis providers"
git push azure master
```

Po zobrazení výzvy pro heslo pomocí hello heslo, které jste zadali, když jste spustili `az appservice web deployment user set`.

### <a name="browse-toohello-azure-web-app"></a>Procházet toohello webové aplikace Azure
Použití [procházet webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee hello změny za provozu v Azure.

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-4---scale-toomultiple-instances"></a>Krok 4 – škálování toomultiple instancí
Hello plán služby App Service je hello jednotky škálování pro vaše webové aplikace Azure. tooscale na vaší webové aplikace, můžete škálovat hello plán služby App Service.

Použití [aktualizace plánu služby App Service az](https://docs.microsoft.com/cli/azure/appservice/plan#update) hello tooscale out hello instancích toothree plánu služby App Service, což je maximální počet povolený hello B1 cenová úroveň. Mějte na paměti, že je B1 hello cenové úrovně, které jste zvolili při vytváření hello plán služby App Service dříve. 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --number-of-workers 3 
```

## <a name="step-5---scale-geographically"></a>Krok 5 – škálování geograficky
Při příjmu geograficky, spusťte aplikaci v několika oblastech hello cloudu Azure. Tento instalační program zatížení zůstatky aplikace další podle Geografie a snižuje dobu odezvy hello tím, že vaše aplikace blíže tooclient prohlížeče.

V tomto kroku škálování vaší ASP.NET webové aplikace tooa druhý oblasti s [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/). Na konci hello hello kroku budete mít webová aplikace spuštěná v oblasti západní Evropa (již vytvořili) a webová aplikace spuštěná v jihovýchodní Asie (ještě nebyla vytvořena). Obě aplikace se zpracuje z hello stejná adresa URL správce provozu.

### <a name="scale-up-hello-europe-app-toostandard-tier"></a>Škálování hello Evropa aplikace tooStandard vrstvy
Ve službě App Service integrace Azure Traffic Manager vyžaduje hello standardní cenovou úroveň. Použití [aktualizace plánu služby App Service az](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale si vaše tooS1 plán služby App Service. 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --sku S1
```
### <a name="create-a-traffic-manager-profile"></a>Vytvoření profilu Traffic Manageru 
Použití [vytvořit profil správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) toocreate Traffic Manager profilu a přidejte ji tooyour skupinu prostředků. Použijte jedinečný název DNS v $dnsName.

```azurecli
$dnsName = "<replace-with-unique-dns-name>"
az network traffic-manager profile create --name myTrafficManagerProfile --resource-group myResourceGroup --routing-method Performance --unique-dns-name $dnsName
```

> [!NOTE]
> `--routing-method Performance`Určuje, že tento profil [směruje uživatele provoz toohello nejbližší endpoint](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="get-hello-resource-id-of-hello-europe-app"></a>Získání ID prostředku hello hello Evropa aplikace
Použití [az služby App Service web zobrazit](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) ID prostředku hello tooget vaší webové aplikace.

```azurecli
$appId = az appservice web show --name $appName --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-hello-europe-app"></a>Přidat koncový bod Traffic Manager pro aplikaci Evropa hello
Použít [vytvořit koncový bod správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd tooyour koncový bod profilu služby Traffic Manager a pomocí ID prostředku hello vaší webové aplikace jako hello cíl.

```azurecli
az network traffic-manager endpoint create --name myWestEuropeEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appId
```

### <a name="get-hello-traffic-manager-endpoint-url"></a>Získat adresu URL koncového bodu hello Traffic Manager
Váš profil Traffic Manageru teď má koncový bod této body tooyour existující webovou aplikaci. Použití [zobrazit profil správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) tooget jeho adresa URL. 

```azurecli
az network traffic-manager profile show --name myTrafficManagerProfile --resource-group myResourceGroup --query dnsConfig.fqdn --output tsv
```

Kopírovat výstup hello do prohlížeče. Měli byste vidět vaší webové aplikace znovu.

### <a name="create-an-azure-redis-cache-in-asia"></a>Vytvoření Azure Redis Cache v Asii
Teď můžete replikaci vaší službě Azure web app toohello jihovýchodní Asie. toostart, použijte [vytvořit az redis](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate v jihovýchodní Asie druhý účet Azure Redis Cache. Tato mezipaměť musí toobe společně umístěné s vaší aplikací v Asii.

```powershell
$redis = (az redis create --name $cacheName-asia --resource-group myResourceGroup --location "Southeast Asia" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

`--name $cacheName-asia`poskytuje hello název mezipaměti hello hello západní Evropa mezipaměti s hello `-asia` příponu.

### <a name="create-an-app-service-plan-in-asia"></a>Vytvořit plán služby App Service v Asii
Použití [vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) toocreate druhý služby App Service plánování v oblasti hello jihovýchodní Asie, pomocí hello stejné S1 vrstvy jako plán hello západní Evropa.

```azurecli
az appservice plan create --name myAppServicePlanAsia --resource-group myResourceGroup --location "Southeast Asia" --sku S1
```

### <a name="create-a-web-app-in-asia"></a>Vytvoření webové aplikace v Asii
Použití [vytvořit webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate druhý webové aplikace.

```azurecli
az appservice web create --name $appName-asia --resource-group myResourceGroup --plan myAppServicePlanAsia
```

`--name $appName-asia`poskytuje hello aplikace hello název aplikace hello západní Evropa, s hello `-asia` příponu.

### <a name="configure-hello-connection-string-for-redis"></a>Konfigurace hello připojovací řetězec pro Redis
Použití [aktualizovat az služby App Service web konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd toohello webové aplikace hello připojovací řetězec pro hello mezipaměti jihovýchodní Asie.

AZ služby App Service web konfigurace appsettings aktualizovat – nastavení "RedisConnection = $($redis.hostname): $($redis.sslPort), heslo = $($redis.accessKeys.primaryKey), ssl = True, abortConnect = False" – název $appName-Asie – myResourceGroup skupiny prostředků

### <a name="configure-git-deployment-for-hello-asia-app"></a>Nakonfigurujte nasazení Git pro aplikace hello Asie.
Použití [az aplikační služby webové správy zdrojového kódu config místní git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure místní nasazení Git pro hello druhý webovou aplikaci.

```azurecli
az appservice web source-control config-local-git --name $appName-asia --resource-group myResourceGroup
```

Tento příkaz vám poskytuje výstup bude vypadat takto hello následující:

```json
{
  "url": "https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git"
}
```

Použití hello vrátila adresa URL tooconfigure druhý Git vzdálené pro místní úložiště. Hello následující příkaz použije hello předcházející příklad výstupu.

```bash
git remote add azure-asia https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-your-sample-application"></a>Nasazení ukázkové aplikace
Spustit `git push` toodeploy vaše ukázková aplikace toohello druhý Git vzdálené. 

```bash
git push azure-asia master
```

Po zobrazení výzvy pro heslo pomocí hello heslo, které jste zadali, když jste spustili `az appservice web deployment user set`.

### <a name="browse-toohello-asia-app"></a>Procházet toohello Asie aplikace
Použití [procházet webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) tooverify, který vaše aplikace běží za provozu v Azure.

```azurecli
az appservice web browse --name $appName-asia --resource-group myResourceGroup
```

### <a name="get-hello-resource-id-of-hello-asia-app"></a>Získání ID prostředku hello hello Asie aplikace
Použití [az služby App Service web zobrazit](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) ID prostředku hello tooget vaší webové aplikace v jihovýchodní Asie.

```azurecli
$appIdAsia = az appservice web show --name $appName-asia --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-hello-asia-app"></a>Přidat koncový bod Traffic Manager pro aplikaci Asie hello
Použití [vytvořit koncový bod správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd druhý toohello koncový bod profilu služby Traffic Manager.

```azurecli
az network traffic-manager endpoint create --name myAsiaEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appIdAsia
```

### <a name="add-region-identifier-tooweb-apps"></a>Přidání aplikací tooweb identifikátor oblasti
Použití [aktualizovat az služby App Service web konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd proměnnou prostředí oblast.

```azurecli
az appservice web config appsettings update --settings "Region=West Europe" --name $appName --resource-group myResourceGroup
az appservice web config appsettings update --settings "Region=Southeast Asia" --name $appName-asia --resource-group myResourceGroup
```

Kód aplikace už používá toto nastavení aplikace. Podívejte se na `HighScaleApp\Views\Home\Index.cshtml`.

### <a name="complete"></a>Dokončení!

Nyní zkuste tooaccess hello adresu URL vašeho profilu Traffic Manageru z prohlížečů v různých zeměpisných oblastech. Prohlížečích klienta z Evropy by měl zobrazit "Evropa – západ ASP.NET" a prohlížeče klienta z Asie by měl zobrazit "ASP.NET jihovýchodní Asie."

## <a name="more-resources"></a>Další zdroje informací
