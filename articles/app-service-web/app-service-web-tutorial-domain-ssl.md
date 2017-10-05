---
title: "Přidat vlastní domény a SSL do webové aplikace Azure | Microsoft Docs"
description: "Zjistěte, jak připravit Azure webové aplikace přejít produkční přidáním značky společnosti. Mapování vlastní název domény (jednoduché domény) na webové aplikace a zabezpečení pomocí vlastní certifikát SSL."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 03/29/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: c9d00f678b6257a8aafb35acd2d5a2292703a2dc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="add-custom-domain-and-ssl-to-an-azure-web-app"></a>Přidat vlastní domény a SSL do webové aplikace Azure

V tomto kurzu se dozvíte, jak rychle mapování vlastního názvu domény do Azure webové aplikace a zabezpečte ji pomocí vlastní certifikát SSL. 

## <a name="before-you-begin"></a>Než začnete

Před spuštěním této ukázce, nainstalujte [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) místně.

Musíte taky přístup pro správu na stránce konfigurace DNS pro váš poskytovatel příslušné domény. Chcete-li například přidat `www.contoso.com`, budete muset nakonfigurovat záznamy DNS pro `contoso.com`.

A konečně budete potřebovat platný. Soubor PFX _a_ jeho heslo pro certifikát SSL, který chcete nahrát a jejich vazby. Tento certifikát SSL musí být nakonfigurovaný pro zabezpečení vlastní název domény, které chcete. V tomto příkladu, třeba zabezpečit svůj certifikát SSL `www.contoso.com`. 

## <a name="step-1---create-an-azure-web-app"></a>Krok 1 – Vytvoření webové aplikace Azure

### <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Teď prostřednictvím rozhraní příkazového řádku Azure CLI 2.0 v okně terminálu vytvoříme prostředky potřebné pro hostování aplikace Node.js v Azure.  Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce. 

```azurecli 
az login 
``` 
   
### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků   
Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure, jako například webové aplikace, databáze a účty úložiště. 

```azurecli
az group create --name myResourceGroup --location westeurope 
```

K zobrazení možných hodnot, které se dají použít pro `---location`, použijte příkaz `az appservice list-locations` rozhraní příkazového řádku Azure CLI.

## <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

Pomocí příkazu [az appservice plan create](/cli/azure/appservice/plan#create) vytvořte plán služby App Service. 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Následující příklad vytvoří plán služby App Service s názvem `myAppServicePlan` pomocí **základní** cenová úroveň.

Vytvořit plán aplikační služby az--název myAppServicePlan--resource-group myResourceGroup – sku B1

Po vytvoření plánu služby App Service, rozhraní příkazového řádku Azure zobrazí informace o podobně jako v následujícím příkladu. 

```json 
{ 
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
    "kind": "app", 
    "location": "West Europe", 
    "sku": { 
    "capacity": 1, 
    "family": "B", 
    "name": "B1", 
    "tier": "Basic" 
    }, 
    "status": "Ready", 
    "type": "Microsoft.Web/serverfarms" 
} 
``` 

## <a name="create-a-web-app"></a>Vytvoření webové aplikace

Po vytvoření plánu služby App Service teď vytvořte v rámci plánu služby App Service `myAppServicePlan` webovou aplikaci. Poskytuje webové aplikace, můžete se hostování místa k nasazení kódu a poskytuje také adresu URL zobrazení nasazené aplikace. Pomocí příkazu [az appservice web create](/cli/azure/appservice/web#create) vytvořte webovou aplikaci. 

V tomto příkazu prosím nahraďte vlastní název jedinečný aplikace, kde uvidíte `<app_name>` zástupný symbol. Tento jedinečný název se použije jako součást výchozí název domény pro webovou aplikaci, tak název musí být jedinečný v rámci všech aplikací v Azure. Později můžete na webovou aplikaci namapovat libovolné vlastní záznamy DNS, než ji zpřístupníte uživatelům. 

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan 
```

Po vytvoření webové aplikace se v Azure CLI zobrazí podobné informace jako v následujícím příkladu. 

```json 
{ 
    "clientAffinityEnabled": true, 
    "defaultHostName": "<app_name>.azurewebsites.net", 
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<app_name>", 
    "isDefaultContainer": null, 
    "kind": "app", 
    "location": "West Europe", 
    "name": "<app_name>", 
    "repositorySiteName": "<app_name>", 
    "reserved": true, 
    "resourceGroup": "myResourceGroup", 
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
    "state": "Running", 
    "type": "Microsoft.Web/sites", 
} 
```

Z výstupu JSON `defaultHostName` ukazuje výchozí název domény vaší webové aplikace. V prohlížeči přejděte na tuto adresu.

```
http://<app_name>.azurewebsites.net 
``` 
 
![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-created.png)  

## <a name="step-2---configure-dns-mapping"></a>Krok 2: Konfigurace mapování DNS

V tomto kroku přidáte mapování z vlastní domény k vaší webové aplikace výchozí název domény, `<app_name>.azurewebsites.net`. Tento krok se obvykle, proveďte ve zprostředkovateli domény webu. Každý Registrátor web je poněkud lišit, a proto byste se měli obrátit dokumentaci svého poskytovatele. Tady jsou ale některé obecné pokyny. 

### <a name="navigate-to-to-dns-management-page"></a>Přejděte na stránce správy DNS

Nejdřív přihlaste k webu vašeho registrátora domény.  

Pro správu záznamy DNS, najdete stránce. Vyhledejte odkazy nebo oblasti webu s názvem bez přípony **název domény**, **DNS**, nebo **název serveru správy**. Často můžete najít na odkaz a zobrazení informací o vašem účtu a podívat se na odkaz, jako **mé domény**.

Jakmile zjistíte, tato stránka, vyhledejte odkaz, který umožňuje přidat nebo upravit záznamy DNS. To může být **souboru zóny** nebo **záznamy DNS** odkaz, nebo **pokročilou konfiguraci** odkaz.

### <a name="create-a-cname-record"></a>Vytvořte záznam CNAME

Přidejte záznam CNAME, který mapuje název požadované subdomény na vaší webové aplikace výchozí název domény (`<app_name>.azurewebsites.net`, kde `<app_name>` je jedinečný název vaší aplikace).

Pro `www.contoso.com` například vytvořit záznam CNAME, který se mapuje `www` název hostitele pro `<app_name>.azurewebsites.net`.

## <a name="step-3---configure-the-custom-domain-on-your-web-app"></a>Krok 3 – konfigurace vlastní domény na vaší webové aplikace

Po dokončení konfigurace mapování názvu hostitele v doméně poskytovatele, jste připravení nakonfigurovat vlastní doménu na vaší webové aplikace. Použití [přidat název hostitele konfigurace az služby App Service web](/cli/azure/appservice/web/config/hostname#add) příkazu přidejte tuto konfiguraci. 

V níže uvedeném příkazu, nahraďte `<app_name>` s jedinečným názvem aplikace a < your_custom_domain > s plně kvalifikovaný vlastní název domény (například `www.contoso.com`). 

```azurecli
az appservice web config hostname add --webapp <app_name> --resource-group myResourceGroup --name <your_custom_domain>
```

Vlastní doména nyní plně mapován na vaší webové aplikace. V prohlížeči přejděte na vlastní název domény. Například:

```
http://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-custom-domain.png)  

## <a name="step-4---bind-a-custom-ssl-certificate-to-your-web-app"></a>Krok 4 – vlastní certifikát SSL vazbu do vaší webové aplikace

Nyní máte webové aplikace Azure, názvem domény, které chcete v panelu Adresa prohlížeče. Ale pokud přejdete do `https://<your_custom_domain>` nyní se zobrazí chyba certifikátu. 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-cert-error.png)  

K této chybě dojde, protože webové aplikace ještě nemá vazbu certifikátu SSL, který odpovídá vlastního názvu domény. Ale neobdržíte chybu, pokud přejdete do `https://<app_name>.azurewebsites.net`. Důvodem je, že aplikace, stejně jako všechny aplikace služby Azure App Service, je zabezpečen pomocí certifikátu SSL `*.azurewebsites.net` zástupné domény ve výchozím nastavení. 

Chcete-li získat přístup k vaší webové aplikace pomocí vlastního názvu domény, budete muset vytvořit vazbu certifikátu SSL pro vaši vlastní doménu do webové aplikace. Provedete ho v tomto kroku. 

### <a name="upload-the-ssl-certificate"></a>Nahrát na server certifikát SSL

Nahrát na server certifikát SSL pro vaši vlastní doménu do vaší webové aplikace pomocí [az služby App Service web konfigurace ssl nahrávání](/cli/azure/appservice/web/config/ssl#upload) příkaz.

V níže uvedeném příkazu, nahraďte `<app_name>` s názvem vaší jedinečné aplikace `<path_to_ptx_file>` cestou k vaší. Soubor PFX a `<password>` heslem svůj certifikát. 

```azurecli
az appservice web config ssl upload --name <app_name> --resource-group myResourceGroup --certificate-file <path_to_pfx_file> --certificate-password <password> 
```

Při odeslání certifikátu Azure CLI uvádí informace podobně jako v následujícím příkladu:

```json
{
  "cerBlob": null,
  "expirationDate": "2018-03-29T14:12:57+00:00",
  "friendlyName": "",
  "hostNames": [
    "www.contoso.com"
  ],
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/cert
ificates/9FD1D2D06E2293673E2A8D1CA484A092BD016D00__West Europe_myResourceGroup",
  "issueDate": "2017-03-29T14:12:57+00:00",
  "issuer": "www.contoso.com",
  "keyVaultId": null,
  "keyVaultSecretName": null,
  "keyVaultSecretStatus": "Initialized",
  "kind": null,
  "location": "West Europe",
  "name": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00__West Europe_myResourceGroup",
  "password": null,
 "pfxBlob": null,
  "publicKeyHash": null,
  "resourceGroup": "myResourceGroup",
  "selfLink": null,
  "serverFarmId": null,
  "siteName": null,
  "subjectName": "www.contoso.com",
  "tags": null,
  "thumbprint": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00",
  "type": "Microsoft.Web/certificates",
  "valid": null
}
```

Z výstupu JSON `thumbprint` ukazuje odeslaný certifikát kryptografický otisk. Zkopírujte jeho hodnoty pro další krok.

### <a name="bind-the-uploaded-ssl-certificate-to-the-web-app"></a>Nahraný certifikát SSL vazbu do webové aplikace

Webové aplikace teď má vlastní název domény, který má být a má také certifikát SSL, který zabezpečuje vlastní domény. Pouze udělat zbývá pro vazbu se nahraný certifikát do webové aplikace. Můžete to provést pomocí [az služby App Service web konfigurace protokolu ssl vazby](/cli/azure/appservice/web/config/ssl#bind) příkaz.

V níže uvedeném příkazu, nahraďte `<app_name>` názvem vaší aplikace jedinečný a `<thumbprint-from-previous-output>` s kryptografickým otiskem certifikátu, kterou můžete získat z předchozí příkaz. 

AZ služby App Service web konfigurace protokolu ssl vazby – název < název_aplikace >--resource-group myResourceGroup – SNI – ssl-type-kryptografický otisk certifikátu < kryptografický otisk z – předchozí output >

Pokud certifikát je vázán na vaší webové aplikace, rozhraní příkazového řádku Azure uvádí informace podobně jako v následujícím příkladu:

{"stavu": "Normální", "clientAffinityEnabled": nastavena hodnota true, "clientCertEnabled": hodnotu false, "cloningInfo": null, "containerSize": 0, "dailyMemoryTimeQuota": 0, "defaultHostName": "< název_aplikace >. azurewebsites.net", "povoleno": true "enabledHostNames": ["www.contoso.com", "< název_aplikace >. azurewebsites.net", "< název_aplikace >. scm.azurewebsites.net"], "gatewaySiteName": null, "hostNameSslStates": [{"hostType": "Standard", "název": "< název_aplikace >. azurewebsites.net", "sslState": "Zakázáno", "kryptografický otisk": null, "toUpdate": null, "virtuální IP adresy": hodnotu null}, {"hostType": "Úložiště", "name": "< název_aplikace >. scm.azurewebsites.net", "sslState": "Zakázat" "kryptografický otisk": hodnotu null, "toUpdate": hodnotu null, "virtuální IP adresy": null}, {"hostType": "Standard", "název": "www.contoso.com", "sslState": "SniEnabled", "kryptografický otisk": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00", "toUpdate": hodnotu null, "virtuální IP adresy": hodnotu null}], "názvy hostitelů": ["www.contoso.com", "< název_aplikace >. azurewebsites.net"], "hostNamesDisabled": hodnotu false, "hostingEnvironmentProfile": hodnotu null, "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s / < název_aplikace >", "isDefaultContainer": null, "druh": "WebApp", "lastModifiedTimeUtc": "2017-03-29T14:36:18.803333", "umístění": "Západní Evropa", "maxNumberOfWorkers": hodnotu null, "Mikroslužbu": "false" a "názvem": "< app_name >" "outboundIpAddresses": "13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1", "premiumAppDeployed": null, "repositorySiteName": "< app_name >", "vyhrazené": hodnotu false, "resourceGroup": "myResourceGroup", "scmSiteAlsoStopped": hodnotu false, "serverFarmId": "/ odběry/00000000-0000-0000-0000-000000000000/Skupinyprostředků/myResourceGroup nebo poskytovatelé/Microsof t.Web/serverfarms/myAppServicePlan", "siteConfig": null, "slotSwapStatus": null, "stavu": "Systém", "suspendedTill": null, "značky": null, "targetSwapSlot": null, "trafficManagerHostNames": null, "type": "Microsoft.Web/sites", "usageState": "Normální"}

V prohlížeči přejděte ke koncovému bodu HTTPS z vlastního názvu domény. Například:

```
https://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-ssl-success.png)  

## <a name="more-resources"></a>Další zdroje informací

[Zakoupení a konfigurace vlastního názvu domény v Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[zakoupení a konfigurace certifikátu protokolu SSL pro Azure App Service](web-sites-purchase-ssl-web-site.md)
