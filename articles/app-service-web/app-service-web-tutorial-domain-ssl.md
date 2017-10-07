---
title: "aaaAdd vlastní domény a SSL tooan Azure webová aplikace | Microsoft Docs"
description: "Zjistěte, jak tooprepare vaše Azure webové aplikace toogo produkční přidáním značky společnosti. Namapujte hello vlastní domény název (jednoduché doména) tooyour webové aplikace a zabezpečení pomocí vlastní certifikát SSL."
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
ms.openlocfilehash: 2679ed8b2dbbeba0b128c1a3ec01148f97c35342
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-custom-domain-and-ssl-tooan-azure-web-app"></a>Přidat vlastní domény a SSL tooan webové aplikace Azure

Tento kurz ukazuje, jak tooquickly namapovat vlastní doménu název tooyour Azure webové aplikace a zabezpečte ji pomocí vlastní certifikát SSL. 

## <a name="before-you-begin"></a>Než začnete

Před spuštěním této ukázce, nainstalujte hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) místně.

Je také nutné stránku konfigurace DNS toohello přístup pro správu pro váš poskytovatel příslušné domény. Například tooadd `www.contoso.com`, je nutné, aby záznamy DNS schopný tooconfigure toobe pro `contoso.com`.

A konečně budete potřebovat platný. Soubor PFX _a_ jeho heslo pro certifikát SSL hello chcete tooupload a jejich vazby. Tento certifikát SSL musí být nakonfigurované toosecure hello vlastní název domény, které chcete. V hello výše příklad, třeba zabezpečit svůj certifikát SSL `www.contoso.com`. 

## <a name="step-1---create-an-azure-web-app"></a>Krok 1 – Vytvoření webové aplikace Azure

### <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Snažíme se teď má toouse hello Azure CLI 2.0 v okno terminálu toocreate hello prostředky potřebné toohost naše aplikace Node.js v Azure.  Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů. 

```azurecli 
az login 
``` 
   
### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků   
Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure, jako například webové aplikace, databáze a účty úložiště. 

```azurecli
az group create --name myResourceGroup --location westeurope 
```

toosee jaké možné hodnoty, které můžete použít pro `---location`, použijte hello `az appservice list-locations` příkazu příkazového řádku Azure CLI.

## <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

Vytvořte plán služby App Service s hello [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) příkaz. 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Hello následující příklad vytvoří plán služby App Service s názvem `myAppServicePlan` pomocí hello **základní** cenová úroveň.

Vytvořit plán aplikační služby az--název myAppServicePlan--resource-group myResourceGroup – sku B1

Po vytvoření hello plán služby App Service, hello rozhraní příkazového řádku Azure znázorňuje následující ukázka podobné toohello informace. 

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

Teď, když byl vytvořen plán služby App Service, vytvoření webové aplikace v rámci hello `myAppServicePlan` plán služby App Service. Hello webové aplikace poskytuje jste jste hostování toodeploy místo kódu a poskytuje také adresu URL pro vás tooview hello nasazené aplikace. Použití hello [vytvořit webové služby App Service az](/cli/azure/appservice/web#create) příkaz toocreate hello webové aplikace. 

V příkazu hello níže nahraďte prosím svůj vlastní název jedinečný aplikace, kde uvidíte hello `<app_name>` zástupný symbol. Tento jedinečný název se použije jako součást hello hello výchozí název domény pro webovou aplikaci hello, takže název hello musí toobe jedinečný mezi všechny aplikace v Azure. Později můžete namapovat všechny vlastní DNS záznam toohello webové aplikace v ještě před zveřejněním tooyour uživatele. 

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan 
```

Po vytvoření webové aplikace hello hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka. 

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

Z hello výstup JSON `defaultHostName` ukazuje výchozí název domény vaší webové aplikace. V prohlížeči přejděte toothis adresu.

```
http://<app_name>.azurewebsites.net 
``` 
 
![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-created.png)  

## <a name="step-2---configure-dns-mapping"></a>Krok 2: Konfigurace mapování DNS

V tomto kroku přidáte mapování z vlastní domény tooyour webovou aplikaci na výchozí název domény, `<app_name>.azurewebsites.net`. Tento krok se obvykle, proveďte ve zprostředkovateli domény webu. Každý Registrátor web je poněkud lišit, a proto byste se měli obrátit dokumentaci svého poskytovatele. Tady jsou ale některé obecné pokyny. 

### <a name="navigate-tootoodns-management-page"></a>Přejděte na stránku Správa tootooDNS

Přihlaste se nejdřív tooyour doménového registrátora webu.  

Vyhledejte hello stránky pro správu záznamy DNS. Vyhledejte odkazy nebo oblasti hello webu s názvem bez přípony **název domény**, **DNS**, nebo **název serveru správy**. Často můžete najít hello odkaz a zobrazení informací o vašem účtu a podívat se na odkaz, jako **mé domény**.

Jakmile zjistíte, tato stránka, vyhledejte odkaz, který umožňuje přidat nebo upravit záznamy DNS. To může být **souboru zóny** nebo **záznamy DNS** odkaz, nebo **pokročilou konfiguraci** odkaz.

### <a name="create-a-cname-record"></a>Vytvořte záznam CNAME

Přidejte záznam CNAME, který mapuje název domény výchozí hello požadované subdomény název tooyour webové aplikace (`<app_name>.azurewebsites.net`, kde `<app_name>` je jedinečný název vaší aplikace).

Pro hello `www.contoso.com` například vytvořit záznam CNAME, který se mapuje hello `www` hostname příliš`<app_name>.azurewebsites.net`.

## <a name="step-3---configure-hello-custom-domain-on-your-web-app"></a>Krok 3 – konfigurace vlastní domény hello na vaší webové aplikace

Po dokončení konfigurace mapování názvu hostitele hello ve zprostředkovateli domény webu, jste připravené tooconfigure hello vlastní doménu na vaší webové aplikace. Použití hello [přidat název hostitele konfigurace az služby App Service web](/cli/azure/appservice/web/config/hostname#add) příkaz tooadd tuto konfiguraci. 

V příkazu hello níže nahraďte `<app_name>` s jedinečným názvem aplikace a < your_custom_domain > s hello plně kvalifikovaný vlastní název domény (například `www.contoso.com`). 

```azurecli
az appservice web config hostname add --webapp <app_name> --resource-group myResourceGroup --name <your_custom_domain>
```

vlastní domény Hello je nyní plně namapované tooyour webové aplikace. V prohlížeči přejděte toohello vlastní název domény. Například:

```
http://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-custom-domain.png)  

## <a name="step-4---bind-a-custom-ssl-certificate-tooyour-web-app"></a>Krok 4 – vytvoření vazby vlastní tooyour webové aplikace certifikát SSL

Nyní máte webové aplikace Azure, s názvem domény hello, které chcete v adresním řádku prohlížeče hello. Ale pokud přejdete toohello `https://<your_custom_domain>` nyní se zobrazí chyba certifikátu. 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-cert-error.png)  

K této chybě dojde, protože webové aplikace ještě nemá vazbu certifikátu SSL, který odpovídá vlastního názvu domény. Však není dojde k chybě, pokud přejdete příliš`https://<app_name>.azurewebsites.net`. Důvodem je, že vaše aplikace, stejně jako všechny aplikace služby Azure App Service, je zabezpečen hello certifikát SSL pro hello `*.azurewebsites.net` zástupné domény ve výchozím nastavení. 

V pořadí tooaccess vaší webové aplikace pomocí vlastního názvu domény, musíte certifikát SSL hello toobind pro vaši vlastní doménu toohello webovou aplikaci. Provedete ho v tomto kroku. 

### <a name="upload-hello-ssl-certificate"></a>Nahrajte certifikát SSL hello

Nahrát hello certifikát SSL pro webovou aplikaci tooyour vlastní doménu pomocí hello [az služby App Service web konfigurace ssl nahrávání](/cli/azure/appservice/web/config/ssl#upload) příkaz.

V příkazu hello níže nahraďte `<app_name>` s názvem vaší jedinečné aplikace `<path_to_ptx_file>` s tooyour cesta hello. Soubor PFX a `<password>` heslem svůj certifikát. 

```azurecli
az appservice web config ssl upload --name <app_name> --resource-group myResourceGroup --certificate-file <path_to_pfx_file> --certificate-password <password> 
```

Při odeslání certifikátu hello hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka:

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

Z hello výstup JSON `thumbprint` ukazuje odeslaný certifikát kryptografický otisk. Zkopírujte jeho hodnoty pro další krok hello.

### <a name="bind-hello-uploaded-ssl-certificate-toohello-web-app"></a>Vytvoření vazby hello nahrán SSL certifikát toohello webové aplikace

Webové aplikace teď má hello vlastní název domény, který má být a má také certifikát SSL, který zabezpečuje vlastní domény. Hello pouze jedinou levém toodo je toobind hello odeslaný certifikát toohello webové aplikace. Můžete to provést pomocí hello [az služby App Service web konfigurace protokolu ssl vazby](/cli/azure/appservice/web/config/ssl#bind) příkaz.

V příkazu hello níže nahraďte `<app_name>` názvem vaší aplikace jedinečný a `<thumbprint-from-previous-output>` s hello kryptografický otisk certifikátu, kterou můžete získat z předchozí příkaz hello. 

AZ služby App Service web konfigurace protokolu ssl vazby – název < název_aplikace >--resource-group myResourceGroup – SNI – ssl-type-kryptografický otisk certifikátu < kryptografický otisk z – předchozí output >

Když je certifikát hello vázané tooyour webové aplikace, hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka:

{"stavu": "Normální", "clientAffinityEnabled": nastavena hodnota true, "clientCertEnabled": hodnotu false, "cloningInfo": null, "containerSize": 0, "dailyMemoryTimeQuota": 0, "defaultHostName": "< název_aplikace >. azurewebsites.net", "povoleno": true "enabledHostNames": ["www.contoso.com", "< název_aplikace >. azurewebsites.net", "< název_aplikace >. scm.azurewebsites.net"], "gatewaySiteName": null, "hostNameSslStates": [{"hostType": "Standard", "název": "< název_aplikace >. azurewebsites.net", "sslState": "Zakázáno", "kryptografický otisk": null, "toUpdate": null, "virtuální IP adresy": hodnotu null}, {"hostType": "Úložiště", "name": "< název_aplikace >. scm.azurewebsites.net", "sslState": "Zakázat" "kryptografický otisk": hodnotu null, "toUpdate": hodnotu null, "virtuální IP adresy": null}, {"hostType": "Standard", "název": "www.contoso.com", "sslState": "SniEnabled", "kryptografický otisk": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00", "toUpdate": hodnotu null, "virtuální IP adresy": hodnotu null}], "názvy hostitelů": ["www.contoso.com", "< název_aplikace >. azurewebsites.net"], "hostNamesDisabled": hodnotu false, "hostingEnvironmentProfile": hodnotu null, "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s / < název_aplikace >", "isDefaultContainer": null, "druh": "WebApp", "lastModifiedTimeUtc": "2017-03-29T14:36:18.803333", "umístění": "Západní Evropa", "maxNumberOfWorkers": hodnotu null, "Mikroslužbu": "false" a "názvem": "< app_name >" "outboundIpAddresses": "13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1", "premiumAppDeployed": null, "repositorySiteName": "< app_name >", "vyhrazené": hodnotu false, "resourceGroup": "myResourceGroup", "scmSiteAlsoStopped": hodnotu false, "serverFarmId": "/ odběry/00000000-0000-0000-0000-000000000000/Skupinyprostředků/myResourceGroup nebo poskytovatelé/Microsof t.Web/serverfarms/myAppServicePlan", "siteConfig": null, "slotSwapStatus": null, "stavu": "Systém", "suspendedTill": null, "značky": null, "targetSwapSlot": null, "trafficManagerHostNames": null, "type": "Microsoft.Web/sites", "usageState": "Normální"}

V prohlížeči přejděte tooHTTPS koncový bod vlastního názvu domény. Například:

```
https://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-ssl-success.png)  

## <a name="more-resources"></a>Další zdroje informací

[Zakoupení a konfigurace vlastního názvu domény v Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[zakoupení a konfigurace certifikátu protokolu SSL pro Azure App Service](web-sites-purchase-ssl-web-site.md)
