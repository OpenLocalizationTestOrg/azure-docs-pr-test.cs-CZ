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
# <a name="build-a-hyper-scale-web-app-in-azure"></a>Sestavení flexibilně škálovatelné webové aplikace v Azure

V tomto kurzu se dozvíte, jak chcete škálovat webové aplikace ASP.NET v Azure a maximalizovat tak požadavků uživatele.

Před zahájením tohoto kurzu, zajistěte, aby [je nainstalované rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) na váš počítač. Kromě toho musíte [Visual Studio](https://www.visualstudio.com/vs/) na místním počítači ke spuštění ukázkové aplikace.

## <a name="step-1---get-sample-application"></a>Krok 1 – Get ukázkové aplikace
V tomto kroku nastavíte místní projekt ASP.NET.

### <a name="clone-the-application-repository"></a>Klonovat úložiště v aplikaci

Otevřete terminál příkazového řádku podle svého výběru a `CD` do pracovního adresáře. Potom spusťte následující příkazy a naklonujte ukázkovou aplikaci. 

```powershell
git clone https://github.com/cephalin/HighScaleApp.git
```

### <a name="run-the-sample-application-in-visual-studio"></a>Spuštění ukázkové aplikace v sadě Visual Studio

Otevřete řešení v sadě Visual Studio.

```powershell
cd HighScaleApp
.\HighScaleApp.sln
```

Typ `F5` ke spuštění aplikace.

Tuto ukázkovou webovou aplikaci ASP.NET pochází z výchozí šablony a přetrvává uživatelské relace a používá výstupní mezipaměti. Podívejte se na `HighScaleApp\Controllers\HomeController.cs`. `Index()` Metoda přidává část dat do relace.

```csharp
Session.Add("visited", "true"); 
```

A `About()` a `Contact()` metody mezipaměti jejich výstup.

```csharp
[OutputCache(Duration = 60)]
```

## <a name="step-2---deploy-to-azure"></a>Krok 2 – nasazení do Azure
V tomto kroku vytvoření webové aplikace Azure a nasadit ukázkovou aplikaci ASP.NET do ní.

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků   
Použití [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) k vytvoření [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) v oblasti západní Evropa. Skupina prostředků je, kam jste umístili všechny prostředky Azure, které chcete spravovat společně, jako jsou webové aplikace a všechny SQL databáze back-end.

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

Chcete-li zobrazit, jaké možné hodnoty, které můžete použít pro `---location`, použít [az služby App Service seznamu umístění](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) příkaz.

### <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service
Použití [vytvořit plán aplikační služby az](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) vytvořit "B1" [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1
```

Jednotka škálování, která může obsahovat libovolný počet aplikací, které chcete škálovat nahoru nebo si společně přes stejnou infrastrukturu služby App Service je plán služby App Service. Každý plán je také přiřazený [cenová úroveň](https://azure.microsoft.com/en-us/pricing/details/app-service/). Vyšší úrovně zahrnují lepší hardwaru a další funkce, jako je více instancí Škálováním na více systémů.

V tomto kurzu je B1 minimální úroveň, která umožňuje škálování na tři instance. Vždy lze přesunout aplikace nahoru nebo dolů cenové úrovně později spuštěním [aktualizace plánu služby App Service az](https://docs.microsoft.com/cli/azure/appservice/plan#update). 

### <a name="create-a-web-app"></a>Vytvoření webové aplikace
Použití [vytvořit webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) k vytvoření webové aplikace s jedinečným názvem v `$appName`.

```azurecli
$appName = "<replace-with-a-unique-name>"
az appservice web create --name $appName --resource-group myResourceGroup --plan myAppServicePlan
```

### <a name="set-deployment-credentials"></a>Nastavení přihlašovacích údajů nasazení
Použití [nastavený uživatel nasazení webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) a nastavte přihlašovací údaje nasazení úrovni účtu pro službu App Service.

```azurecli
az appservice web deployment user set --user-name <letters-numbers> --password <mininum-8-char-captital-lowercase-letters-numbers>
```

### <a name="configure-git-deployment"></a>Konfigurace nasazení Git
Použití [az aplikační služby webové správy zdrojového kódu config místní git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) nakonfigurovat místní nasazení Git.

```azurecli
az appservice web source-control config-local-git --name $appName --resource-group myResourceGroup
```

Tento příkaz vám poskytuje výstup, který vypadá takto:

```json
{
  "url": "https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git"
}
```

Použijte vrácený adresu URL ke konfiguraci vaší vzdálené Git. Následující příkaz použije v předchozím příkladu výstupu.

```powershell
git remote add azure https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-the-sample-application"></a>Nasazení ukázkové aplikace
Teď jste připravení nasadit ukázkovou aplikaci. Spusťte `git push`.

```powershell
git push azure master
```

Po zobrazení výzvy pro heslo, používat heslo, které jste zadali, když jste spustili `az appservice web deployment user set`.

### <a name="browse-to-azure-web-app"></a>Přejděte do webové aplikace Azure
Použití [procházet webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) Chcete-li sledovat živý běh v Azure, spusťte tento příkaz.

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-3---connect-to-redis"></a>Krok 3 – připojení k Redis
V tomto kroku nastavíte Azure Redis Cache jako externí, společně umístěných mezipaměti do Azure webové aplikace. Můžete rychle využít Redis pro ukládání do mezipaměti výstupu stránky. Kromě toho, když jste škálovat webové aplikace později, Redis vám pomůže zachovat uživatelských relací napříč více instancí spolehlivě.

### <a name="create-an-azure-redis-cache"></a>Vytvoření Azure Redis Cache
Použití [vytvořit az redis](https://docs.microsoft.com/en-us/cli/azure/redis#create) vytvořit Azure Redis Cache a uložit ve formátu JSON výstupu. Použijte jedinečný název v `$cacheName`.

```powershell
$cacheName = "<replace-with-a-unique-cache-name>"
$redis = (az redis create --name $cacheName --resource-group myResourceGroup --location "West Europe" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

### <a name="configure-the-application-to-use-redis"></a>Konfigurace aplikace pro používání Redis
Formátování řetězce připojení ke svojí mezipaměti.

```powershell
$connstring = "$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False"
$connstring 
```

Druhý řádek měl dát výstupu, který vypadá takto:

```
mycachename.redis.cache.windows.net:6380,password=/rQP/TLz1mrEPpmh9b/gnfns/t9vBRXqXn3i1RwBjGA=,ssl=True,abortConnect=False
```

V sadě Visual Studio vytvořit soubor webové konfigurace v kořenového adresáře projektu názvem `redis.config` a vložte následující kód do ní. V `value`, použít připojovací řetězec z výstupu prostředí PowerShell.

```xml
<appSettings>
  <add key="RedisConnection" value="your-azure-redis-cache-connection-string"/>
</appSettings>
```

Pokud se podíváte `.gitignore` souboru v úložiště Git, zobrazí se, že tento soubor je vyloučen z řízení zdrojů. Tímto způsobem je udržováno bezpečné vaše citlivé informace. 

Otevřete `Web.config`. Upozornění `<appSettings file="redis.config">` element, který získá nastavení, které jste vytvořili v `redis.config`. 

Najít komentáři oddíl, který zahrnuje `<sessionState>` a `<caching>`. Zrušením komentáře u této části.

![](./media/app-service-web-tutorial-hyper-scale-app/redisproviders.png)

Tento kód vyhledá Redis připojovacího řetězce je definována v `RedisConnection`. 

Teď vaše aplikace používá ke správě relací a ukládání do mezipaměti Redis. Typ `F5` ke spuštění aplikace. Pokud chcete můžete si stáhnout klienta správy Redis k vizualizaci dat, která jsou nyní uloženy do mezipaměti.

### <a name="configure-the-connection-string-in-azure"></a>Konfigurovat připojovací řetězec v Azure

Pro aplikace pro práci v Azure budete muset nakonfigurovat do jednoho připojovacího řetězce Redis v Azure webové aplikace. Vzhledem k tomu `redis.config` nezachová ve správě zdrojového kódu není nasazena do Azure, když spustíte nasazení Git.

Použití [aktualizovat az služby App Service web konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) přidat řetězec připojení se stejným názvem (`RedisConnection`).

AZ služby App Service web konfigurace appsettings aktualizovat – nastavení "RedisConnection = $connstring" – název $appName – myResourceGroup skupiny prostředků

Nezapomeňte, že `$connstring` obsahuje formátovaný připojovací řetězec.

### <a name="redeploy-the-application-to-azure"></a>Znovu nasaďte aplikaci do Azure
Použití příkazu Git push změny do Azure

```bash
git add .
git commit -m "now use Redis providers"
git push azure master
```

Po zobrazení výzvy pro heslo, používat heslo, které jste zadali, když jste spustili `az appservice web deployment user set`.

### <a name="browse-to-the-azure-web-app"></a>Přejděte do webové aplikace Azure
Použití [procházet webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) a podívejte se změny za provozu v Azure.

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-4---scale-to-multiple-instances"></a>Krok 4 – Škálováním na více instancí
Plán služby App Service je jednotka škálování pro vaše webové aplikace Azure. Pro rozšíření Škálováním vaší webové aplikace, můžete škálovat plán služby App Service.

Použití [aktualizace plánu služby App Service az](https://docs.microsoft.com/cli/azure/appservice/plan#update) škálovat plán služby App Service na tři instance, což je maximální počet povolené B1 cenová úroveň. Mějte na paměti, že je B1 cenovou úroveň, které jste zvolili při vytváření plánu služby App Service dříve. 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --number-of-workers 3 
```

## <a name="step-5---scale-geographically"></a>Krok 5 – škálování geograficky
Při zvětšení velikosti geograficky, spusťte aplikaci v několika oblastí cloudu Azure. Tento instalační program zatížení zůstatky aplikace další podle Geografie a snižuje dobu odezvy tím, že vaše aplikace blíže do klientských prohlížečů.

V tomto kroku škálování webové aplikace ASP.NET v druhé oblasti s [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/). Na konci tohoto kroku budete mít webová aplikace spuštěná v oblasti západní Evropa (již vytvořili) a webová aplikace spuštěná v jihovýchodní Asie (ještě nebyla vytvořena). Z TÉŽE Traffic Manager se zpracuje obě aplikace.

### <a name="scale-up-the-europe-app-to-standard-tier"></a>Škálování aplikace Evropa úrovně Standard
Ve službě App Service integrace Azure Traffic Manager vyžaduje standardní cenovou úroveň. Použití [aktualizace plánu služby App Service az](https://docs.microsoft.com/cli/azure/appservice/plan#update) škálování vašeho plánu služby App Service S1. 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --sku S1
```
### <a name="create-a-traffic-manager-profile"></a>Vytvoření profilu Traffic Manageru 
Použití [vytvořit profil správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) k vytvoření profilu Traffic Manageru a přidat jej do vaší skupiny prostředků. Použijte jedinečný název DNS v $dnsName.

```azurecli
$dnsName = "<replace-with-unique-dns-name>"
az network traffic-manager profile create --name myTrafficManagerProfile --resource-group myResourceGroup --routing-method Performance --unique-dns-name $dnsName
```

> [!NOTE]
> `--routing-method Performance`Určuje, že tento profil [směruje provoz uživatele na nejbližší koncový bod](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="get-the-resource-id-of-the-europe-app"></a>Získání ID prostředku aplikace Evropa
Použití [az služby App Service web zobrazit](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) získat ID prostředku vaší webové aplikace.

```azurecli
$appId = az appservice web show --name $appName --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-the-europe-app"></a>Přidat koncový bod Traffic Manager pro aplikaci Evropa
Použít [vytvořit koncový bod správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) přidání koncového bodu pro svůj profil Traffic Manageru a pomocí ID prostředků vaší webové aplikace jako cíl.

```azurecli
az network traffic-manager endpoint create --name myWestEuropeEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appId
```

### <a name="get-the-traffic-manager-endpoint-url"></a>Získat adresu URL koncového bodu Traffic Manager
Váš profil Traffic Manageru teď má koncový bod, který ukazuje na existující webovou aplikaci. Použití [zobrazit profil správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) získat jeho adresa URL. 

```azurecli
az network traffic-manager profile show --name myTrafficManagerProfile --resource-group myResourceGroup --query dnsConfig.fqdn --output tsv
```

Zkopírujte výstup do prohlížeče. Měli byste vidět vaší webové aplikace znovu.

### <a name="create-an-azure-redis-cache-in-asia"></a>Vytvoření Azure Redis Cache v Asii
Teď můžete replikaci vaší webové aplikace Azure oblast, jihovýchodní Asie. Chcete-li začít, použijte [vytvořit az redis](https://docs.microsoft.com/en-us/cli/azure/redis#create) vytvoření druhého Azure Redis Cache v jihovýchodní Asie. Tato mezipaměť musí být umístěna společně s vaší aplikací v Asii.

```powershell
$redis = (az redis create --name $cacheName-asia --resource-group myResourceGroup --location "Southeast Asia" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

`--name $cacheName-asia`poskytuje název mezipaměti západní Evropa, mezipaměti s `-asia` příponu.

### <a name="create-an-app-service-plan-in-asia"></a>Vytvořit plán služby App Service v Asii
Použití [vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) vytvoření druhého plán služby App Service v oblasti jihovýchodní Asie, pomocí stejné vrstvě S1 jako plán západní Evropa.

```azurecli
az appservice plan create --name myAppServicePlanAsia --resource-group myResourceGroup --location "Southeast Asia" --sku S1
```

### <a name="create-a-web-app-in-asia"></a>Vytvoření webové aplikace v Asii
Použití [vytvořit webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) k vytvoření druhého webové aplikace.

```azurecli
az appservice web create --name $appName-asia --resource-group myResourceGroup --plan myAppServicePlanAsia
```

`--name $appName-asia`poskytuje název aplikace západní Evropa, aplikace se `-asia` příponu.

### <a name="configure-the-connection-string-for-redis"></a>Konfigurace připojovacího řetězce pro Redis
Použití [aktualizovat az služby App Service web konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) do webové aplikace přidat připojovací řetězec pro mezipaměť jihovýchodní Asie.

AZ služby App Service web konfigurace appsettings aktualizovat – nastavení "RedisConnection = $($redis.hostname): $($redis.sslPort), heslo = $($redis.accessKeys.primaryKey), ssl = True, abortConnect = False" – název $appName-Asie – myResourceGroup skupiny prostředků

### <a name="configure-git-deployment-for-the-asia-app"></a>Nakonfigurujte nasazení Git pro aplikaci Asie.
Použití [az aplikační služby webové správy zdrojového kódu config místní git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) nakonfigurovat místní nasazení Git pro druhý webovou aplikaci.

```azurecli
az appservice web source-control config-local-git --name $appName-asia --resource-group myResourceGroup
```

Tento příkaz vám poskytuje výstup, který vypadá takto:

```json
{
  "url": "https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git"
}
```

Použijte vrácený adresu URL ke konfiguraci druhý vzdálené řízení Git pro místní úložiště. Následující příkaz použije v předchozím příkladu výstupu.

```bash
git remote add azure-asia https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-your-sample-application"></a>Nasazení ukázkové aplikace
Spustit `git push` nasadit ukázkovou aplikaci pro druhý vzdálené úložiště Git. 

```bash
git push azure-asia master
```

Po zobrazení výzvy pro heslo, používat heslo, které jste zadali, když jste spustili `az appservice web deployment user set`.

### <a name="browse-to-the-asia-app"></a>Přejděte do aplikace Asie
Použití [procházet webové služby App Service az](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) k ověření, že vaše aplikace běží za provozu v Azure.

```azurecli
az appservice web browse --name $appName-asia --resource-group myResourceGroup
```

### <a name="get-the-resource-id-of-the-asia-app"></a>Získání ID prostředku aplikace Asie
Použití [az služby App Service web zobrazit](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) k získání ID prostředku vaší webové aplikace v jihovýchodní Asie.

```azurecli
$appIdAsia = az appservice web show --name $appName-asia --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-the-asia-app"></a>Přidat koncový bod Traffic Manager pro aplikaci Asie
Použití [vytvořit koncový bod správce provozu sítě az](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) přidat druhý koncový bod profilu Správce provozu.

```azurecli
az network traffic-manager endpoint create --name myAsiaEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appIdAsia
```

### <a name="add-region-identifier-to-web-apps"></a>Přidejte identifikátor oblasti do webové aplikace
Použití [aktualizovat az služby App Service web konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) k přidání proměnné prostředí oblast.

```azurecli
az appservice web config appsettings update --settings "Region=West Europe" --name $appName --resource-group myResourceGroup
az appservice web config appsettings update --settings "Region=Southeast Asia" --name $appName-asia --resource-group myResourceGroup
```

Kód aplikace už používá toto nastavení aplikace. Podívejte se na `HighScaleApp\Views\Home\Index.cshtml`.

### <a name="complete"></a>Dokončení!

Nyní pokusu o přístup k adresu URL vašeho profilu Traffic Manageru z prohlížečů v různých zeměpisných oblastech. Prohlížečích klienta z Evropy by měl zobrazit "Evropa – západ ASP.NET" a prohlížeče klienta z Asie by měl zobrazit "ASP.NET jihovýchodní Asie."

## <a name="more-resources"></a>Další zdroje informací
