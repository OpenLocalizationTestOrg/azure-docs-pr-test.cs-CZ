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
# <a name="add-custom-domain-and-ssl-to-an-azure-web-app"></a><span data-ttu-id="36970-104">Přidat vlastní domény a SSL do webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="36970-104">Add custom domain and SSL to an Azure web app</span></span>

<span data-ttu-id="36970-105">V tomto kurzu se dozvíte, jak rychle mapování vlastního názvu domény do Azure webové aplikace a zabezpečte ji pomocí vlastní certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="36970-105">This tutorial shows you how to quickly map a custom domain name to your Azure web app and then secure it with a custom SSL certificate.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="36970-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="36970-106">Before you begin</span></span>

<span data-ttu-id="36970-107">Před spuštěním této ukázce, nainstalujte [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) místně.</span><span class="sxs-lookup"><span data-stu-id="36970-107">Before running this sample, install the [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) locally.</span></span>

<span data-ttu-id="36970-108">Musíte taky přístup pro správu na stránce konfigurace DNS pro váš poskytovatel příslušné domény.</span><span class="sxs-lookup"><span data-stu-id="36970-108">You also need administrative access to the DNS configuration page for your respective domain provider.</span></span> <span data-ttu-id="36970-109">Chcete-li například přidat `www.contoso.com`, budete muset nakonfigurovat záznamy DNS pro `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="36970-109">For example, to add `www.contoso.com`, you need to be able to configure DNS entries for `contoso.com`.</span></span>

<span data-ttu-id="36970-110">A konečně budete potřebovat platný. Soubor PFX _a_ jeho heslo pro certifikát SSL, který chcete nahrát a jejich vazby.</span><span class="sxs-lookup"><span data-stu-id="36970-110">Lastly, you need a valid .PFX file _and_ its password for the SSL certificate you want to upload and bind.</span></span> <span data-ttu-id="36970-111">Tento certifikát SSL musí být nakonfigurovaný pro zabezpečení vlastní název domény, které chcete.</span><span class="sxs-lookup"><span data-stu-id="36970-111">This SSL certificate should be configured to secure the custom domain name you want.</span></span> <span data-ttu-id="36970-112">V tomto příkladu, třeba zabezpečit svůj certifikát SSL `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="36970-112">In the above example, your SSL certificate should secure `www.contoso.com`.</span></span> 

## <a name="step-1---create-an-azure-web-app"></a><span data-ttu-id="36970-113">Krok 1 – Vytvoření webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="36970-113">Step 1 - Create an Azure web app</span></span>

### <a name="log-in-to-azure"></a><span data-ttu-id="36970-114">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="36970-114">Log in to Azure</span></span>

<span data-ttu-id="36970-115">Teď prostřednictvím rozhraní příkazového řádku Azure CLI 2.0 v okně terminálu vytvoříme prostředky potřebné pro hostování aplikace Node.js v Azure.</span><span class="sxs-lookup"><span data-stu-id="36970-115">We are now going to use the Azure CLI 2.0 in a terminal window to create the resources needed to host our Node.js app in Azure.</span></span>  <span data-ttu-id="36970-116">Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="36970-116">Log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span> 

```azurecli 
az login 
``` 
   
### <a name="create-a-resource-group"></a><span data-ttu-id="36970-117">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="36970-117">Create a resource group</span></span>   
<span data-ttu-id="36970-118">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="36970-118">Create a resource group with the [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="36970-119">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure, jako například webové aplikace, databáze a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="36970-119">An Azure resource group is a logical container into which Azure resources like web apps, databases and storage accounts are deployed and managed.</span></span> 

```azurecli
az group create --name myResourceGroup --location westeurope 
```

<span data-ttu-id="36970-120">K zobrazení možných hodnot, které se dají použít pro `---location`, použijte příkaz `az appservice list-locations` rozhraní příkazového řádku Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="36970-120">To see what possible values you can use for `---location`, use the `az appservice list-locations` Azure CLI command.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="36970-121">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="36970-121">Create an App Service plan</span></span>

<span data-ttu-id="36970-122">Pomocí příkazu [az appservice plan create](/cli/azure/appservice/plan#create) vytvořte plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="36970-122">Create an App Service plan with the [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span> 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="36970-123">Následující příklad vytvoří plán služby App Service s názvem `myAppServicePlan` pomocí **základní** cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="36970-123">The following example creates an App Service plan named `myAppServicePlan` using the **Basic** pricing tier.</span></span>

<span data-ttu-id="36970-124">Vytvořit plán aplikační služby az--název myAppServicePlan--resource-group myResourceGroup – sku B1</span><span class="sxs-lookup"><span data-stu-id="36970-124">az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1</span></span>

<span data-ttu-id="36970-125">Po vytvoření plánu služby App Service, rozhraní příkazového řádku Azure zobrazí informace o podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="36970-125">When the App Service plan has been created, the Azure CLI shows information similar to the following example.</span></span> 

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

## <a name="create-a-web-app"></a><span data-ttu-id="36970-126">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="36970-126">Create a web app</span></span>

<span data-ttu-id="36970-127">Po vytvoření plánu služby App Service teď vytvořte v rámci plánu služby App Service `myAppServicePlan` webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="36970-127">Now that an App Service plan has been created, create a web app within the `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="36970-128">Poskytuje webové aplikace, můžete se hostování místa k nasazení kódu a poskytuje také adresu URL zobrazení nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="36970-128">The web app gives your a hosting space to deploy your code as well as provides a URL for you to view the deployed application.</span></span> <span data-ttu-id="36970-129">Pomocí příkazu [az appservice web create](/cli/azure/appservice/web#create) vytvořte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="36970-129">Use the [az appservice web create](/cli/azure/appservice/web#create) command to create the web app.</span></span> 

<span data-ttu-id="36970-130">V tomto příkazu prosím nahraďte vlastní název jedinečný aplikace, kde uvidíte `<app_name>` zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="36970-130">In the command below, please substitute your own unique app name where you see the `<app_name>` placeholder.</span></span> <span data-ttu-id="36970-131">Tento jedinečný název se použije jako součást výchozí název domény pro webovou aplikaci, tak název musí být jedinečný v rámci všech aplikací v Azure.</span><span class="sxs-lookup"><span data-stu-id="36970-131">This unique name will be used as the part of the default domain name for the web app, so the name needs to be unique across all apps in Azure.</span></span> <span data-ttu-id="36970-132">Později můžete na webovou aplikaci namapovat libovolné vlastní záznamy DNS, než ji zpřístupníte uživatelům.</span><span class="sxs-lookup"><span data-stu-id="36970-132">You can later map any custom DNS entry to the web app before you expose it to your users.</span></span> 

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan 
```

<span data-ttu-id="36970-133">Po vytvoření webové aplikace se v Azure CLI zobrazí podobné informace jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="36970-133">When the web app has been created, the Azure CLI shows information similar to the following example.</span></span> 

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

<span data-ttu-id="36970-134">Z výstupu JSON `defaultHostName` ukazuje výchozí název domény vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="36970-134">From the JSON output, `defaultHostName` shows your web app's default domain name.</span></span> <span data-ttu-id="36970-135">V prohlížeči přejděte na tuto adresu.</span><span class="sxs-lookup"><span data-stu-id="36970-135">In your browser, navigate to this address.</span></span>

```
http://<app_name>.azurewebsites.net 
``` 
 
![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-created.png)  

## <a name="step-2---configure-dns-mapping"></a><span data-ttu-id="36970-137">Krok 2: Konfigurace mapování DNS</span><span class="sxs-lookup"><span data-stu-id="36970-137">Step 2 - Configure DNS mapping</span></span>

<span data-ttu-id="36970-138">V tomto kroku přidáte mapování z vlastní domény k vaší webové aplikace výchozí název domény, `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="36970-138">In this step, you add a mapping from a custom domain to your web app's default domain name, `<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="36970-139">Tento krok se obvykle, proveďte ve zprostředkovateli domény webu.</span><span class="sxs-lookup"><span data-stu-id="36970-139">Typically, you perform this step in your domain provider's website.</span></span> <span data-ttu-id="36970-140">Každý Registrátor web je poněkud lišit, a proto byste se měli obrátit dokumentaci svého poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="36970-140">Every domain registrar's website is slightly different, so you should consult your provider's documentation.</span></span> <span data-ttu-id="36970-141">Tady jsou ale některé obecné pokyny.</span><span class="sxs-lookup"><span data-stu-id="36970-141">However, here are some general guidelines.</span></span> 

### <a name="navigate-to-to-dns-management-page"></a><span data-ttu-id="36970-142">Přejděte na stránce správy DNS</span><span class="sxs-lookup"><span data-stu-id="36970-142">Navigate to to DNS management page</span></span>

<span data-ttu-id="36970-143">Nejdřív přihlaste k webu vašeho registrátora domény.</span><span class="sxs-lookup"><span data-stu-id="36970-143">First, log in to your domain registrar's website.</span></span>  

<span data-ttu-id="36970-144">Pro správu záznamy DNS, najdete stránce.</span><span class="sxs-lookup"><span data-stu-id="36970-144">Then, find the page for managing DNS records.</span></span> <span data-ttu-id="36970-145">Vyhledejte odkazy nebo oblasti webu s názvem bez přípony **název domény**, **DNS**, nebo **název serveru správy**.</span><span class="sxs-lookup"><span data-stu-id="36970-145">Look for links or areas of the site labeled **Domain Name**, **DNS**, or **Name Server Management**.</span></span> <span data-ttu-id="36970-146">Často můžete najít na odkaz a zobrazení informací o vašem účtu a podívat se na odkaz, jako **mé domény**.</span><span class="sxs-lookup"><span data-stu-id="36970-146">Often, you can find the link by viewing your account information, and then looking for a link such as **My domains**.</span></span>

<span data-ttu-id="36970-147">Jakmile zjistíte, tato stránka, vyhledejte odkaz, který umožňuje přidat nebo upravit záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="36970-147">Once you find this page, look for a link that lets you add or edit DNS records.</span></span> <span data-ttu-id="36970-148">To může být **souboru zóny** nebo **záznamy DNS** odkaz, nebo **pokročilou konfiguraci** odkaz.</span><span class="sxs-lookup"><span data-stu-id="36970-148">This might be a **Zone file** or **DNS Records** link, or an **Advanced configuration** link.</span></span>

### <a name="create-a-cname-record"></a><span data-ttu-id="36970-149">Vytvořte záznam CNAME</span><span class="sxs-lookup"><span data-stu-id="36970-149">Create a CNAME record</span></span>

<span data-ttu-id="36970-150">Přidejte záznam CNAME, který mapuje název požadované subdomény na vaší webové aplikace výchozí název domény (`<app_name>.azurewebsites.net`, kde `<app_name>` je jedinečný název vaší aplikace).</span><span class="sxs-lookup"><span data-stu-id="36970-150">Add a CNAME record that maps the desired subdomain name to your web app's default domain name (`<app_name>.azurewebsites.net`, where `<app_name>` is your app's unique name).</span></span>

<span data-ttu-id="36970-151">Pro `www.contoso.com` například vytvořit záznam CNAME, který se mapuje `www` název hostitele pro `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="36970-151">For the `www.contoso.com` example, you create a CNAME that maps the `www` hostname to `<app_name>.azurewebsites.net`.</span></span>

## <a name="step-3---configure-the-custom-domain-on-your-web-app"></a><span data-ttu-id="36970-152">Krok 3 – konfigurace vlastní domény na vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="36970-152">Step 3 - Configure the custom domain on your web app</span></span>

<span data-ttu-id="36970-153">Po dokončení konfigurace mapování názvu hostitele v doméně poskytovatele, jste připravení nakonfigurovat vlastní doménu na vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="36970-153">When you finish configuring the hostname mapping in your domain provider's website, you're ready to configure the custom domain on your web app.</span></span> <span data-ttu-id="36970-154">Použití [přidat název hostitele konfigurace az služby App Service web](/cli/azure/appservice/web/config/hostname#add) příkazu přidejte tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="36970-154">Use the [az appservice web config hostname add](/cli/azure/appservice/web/config/hostname#add) command to add this configuration.</span></span> 

<span data-ttu-id="36970-155">V níže uvedeném příkazu, nahraďte `<app_name>` s jedinečným názvem aplikace a < your_custom_domain > s plně kvalifikovaný vlastní název domény (například `www.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="36970-155">In the command below, please substitute `<app_name>` with your unique app name, and <your_custom_domain> with the fully qualified custom domain name (e.g. `www.contoso.com`).</span></span> 

```azurecli
az appservice web config hostname add --webapp <app_name> --resource-group myResourceGroup --name <your_custom_domain>
```

<span data-ttu-id="36970-156">Vlastní doména nyní plně mapován na vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="36970-156">The custom domain now is fully mapped to your web app.</span></span> <span data-ttu-id="36970-157">V prohlížeči přejděte na vlastní název domény.</span><span class="sxs-lookup"><span data-stu-id="36970-157">In your browser, navigate to the custom domain name.</span></span> <span data-ttu-id="36970-158">Například:</span><span class="sxs-lookup"><span data-stu-id="36970-158">For example:</span></span>

```
http://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-custom-domain.png)  

## <a name="step-4---bind-a-custom-ssl-certificate-to-your-web-app"></a><span data-ttu-id="36970-160">Krok 4 – vlastní certifikát SSL vazbu do vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="36970-160">Step 4 - Bind a custom SSL certificate to your web app</span></span>

<span data-ttu-id="36970-161">Nyní máte webové aplikace Azure, názvem domény, které chcete v panelu Adresa prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="36970-161">You now have an Azure web app, with the domain name you want in the browser's address bar.</span></span> <span data-ttu-id="36970-162">Ale pokud přejdete do `https://<your_custom_domain>` nyní se zobrazí chyba certifikátu.</span><span class="sxs-lookup"><span data-stu-id="36970-162">However, if you navigate to the `https://<your_custom_domain>` now, you get a certificate error.</span></span> 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-cert-error.png)  

<span data-ttu-id="36970-164">K této chybě dojde, protože webové aplikace ještě nemá vazbu certifikátu SSL, který odpovídá vlastního názvu domény.</span><span class="sxs-lookup"><span data-stu-id="36970-164">This error occurs because your web app doesn't yet have an SSL certificate binding that matches your custom domain name.</span></span> <span data-ttu-id="36970-165">Ale neobdržíte chybu, pokud přejdete do `https://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="36970-165">However, you don't get an error if you navigate to `https://<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="36970-166">Důvodem je, že aplikace, stejně jako všechny aplikace služby Azure App Service, je zabezpečen pomocí certifikátu SSL `*.azurewebsites.net` zástupné domény ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="36970-166">This is because your app, as well as all Azure App Service apps, is secured with the SSL certificate for the `*.azurewebsites.net` wildcard domain by default.</span></span> 

<span data-ttu-id="36970-167">Chcete-li získat přístup k vaší webové aplikace pomocí vlastního názvu domény, budete muset vytvořit vazbu certifikátu SSL pro vaši vlastní doménu do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="36970-167">In order to access your web app by your custom domain name, you need to bind the SSL certificate for your custom domain to the web app.</span></span> <span data-ttu-id="36970-168">Provedete ho v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="36970-168">You will do it in this step.</span></span> 

### <a name="upload-the-ssl-certificate"></a><span data-ttu-id="36970-169">Nahrát na server certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="36970-169">Upload the SSL certificate</span></span>

<span data-ttu-id="36970-170">Nahrát na server certifikát SSL pro vaši vlastní doménu do vaší webové aplikace pomocí [az služby App Service web konfigurace ssl nahrávání](/cli/azure/appservice/web/config/ssl#upload) příkaz.</span><span class="sxs-lookup"><span data-stu-id="36970-170">Upload the SSL certificate for your custom domain to your web app by using the [az appservice web config ssl upload](/cli/azure/appservice/web/config/ssl#upload) command.</span></span>

<span data-ttu-id="36970-171">V níže uvedeném příkazu, nahraďte `<app_name>` s názvem vaší jedinečné aplikace `<path_to_ptx_file>` cestou k vaší. Soubor PFX a `<password>` heslem svůj certifikát.</span><span class="sxs-lookup"><span data-stu-id="36970-171">In the command below, please substitute `<app_name>` with your unique app name, `<path_to_ptx_file>` with the path to your .PFX file, and `<password>` with your certificate's password.</span></span> 

```azurecli
az appservice web config ssl upload --name <app_name> --resource-group myResourceGroup --certificate-file <path_to_pfx_file> --certificate-password <password> 
```

<span data-ttu-id="36970-172">Při odeslání certifikátu Azure CLI uvádí informace podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="36970-172">When the certificate is uploaded, the Azure CLI shows information similar to the following example:</span></span>

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

<span data-ttu-id="36970-173">Z výstupu JSON `thumbprint` ukazuje odeslaný certifikát kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="36970-173">From the JSON output, `thumbprint` shows your uploaded certificate's thumbprint.</span></span> <span data-ttu-id="36970-174">Zkopírujte jeho hodnoty pro další krok.</span><span class="sxs-lookup"><span data-stu-id="36970-174">Copy its value for the next step.</span></span>

### <a name="bind-the-uploaded-ssl-certificate-to-the-web-app"></a><span data-ttu-id="36970-175">Nahraný certifikát SSL vazbu do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="36970-175">Bind the uploaded SSL certificate to the web app</span></span>

<span data-ttu-id="36970-176">Webové aplikace teď má vlastní název domény, který má být a má také certifikát SSL, který zabezpečuje vlastní domény.</span><span class="sxs-lookup"><span data-stu-id="36970-176">Your web app now has the custom domain name you want, and it also has a SSL certificate that secures that custom domain.</span></span> <span data-ttu-id="36970-177">Pouze udělat zbývá pro vazbu se nahraný certifikát do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="36970-177">The only thing left to do is to bind the uploaded certificate to the web app.</span></span> <span data-ttu-id="36970-178">Můžete to provést pomocí [az služby App Service web konfigurace protokolu ssl vazby](/cli/azure/appservice/web/config/ssl#bind) příkaz.</span><span class="sxs-lookup"><span data-stu-id="36970-178">You do this by using the [az appservice web config ssl bind](/cli/azure/appservice/web/config/ssl#bind) command.</span></span>

<span data-ttu-id="36970-179">V níže uvedeném příkazu, nahraďte `<app_name>` názvem vaší aplikace jedinečný a `<thumbprint-from-previous-output>` s kryptografickým otiskem certifikátu, kterou můžete získat z předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="36970-179">In the command below, please substitute `<app_name>` with your unique app name and `<thumbprint-from-previous-output>` with the certificate thumbprint that you get from the previous command.</span></span> 

<span data-ttu-id="36970-180">AZ služby App Service web konfigurace protokolu ssl vazby – název < název_aplikace >--resource-group myResourceGroup – SNI – ssl-type-kryptografický otisk certifikátu < kryptografický otisk z – předchozí output ></span><span class="sxs-lookup"><span data-stu-id="36970-180">az appservice web config ssl bind --name <app_name> --resource-group myResourceGroup --certificate-thumbprint <thumbprint-from-previous-output> --ssl-type SNI</span></span>

<span data-ttu-id="36970-181">Pokud certifikát je vázán na vaší webové aplikace, rozhraní příkazového řádku Azure uvádí informace podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="36970-181">When the certificate is bound to your web app, the Azure CLI shows information similar to the following example:</span></span>

<span data-ttu-id="36970-182">{"stavu": "Normální", "clientAffinityEnabled": nastavena hodnota true, "clientCertEnabled": hodnotu false, "cloningInfo": null, "containerSize": 0, "dailyMemoryTimeQuota": 0, "defaultHostName": "< název_aplikace >. azurewebsites.net", "povoleno": true "enabledHostNames": ["www.contoso.com", "< název_aplikace >. azurewebsites.net", "< název_aplikace >. scm.azurewebsites.net"], "gatewaySiteName": null, "hostNameSslStates": [{"hostType": "Standard", "název": "< název_aplikace >. azurewebsites.net", "sslState": "Zakázáno", "kryptografický otisk": null, "toUpdate": null, "virtuální IP adresy": hodnotu null}, {"hostType": "Úložiště", "name": "< název_aplikace >. scm.azurewebsites.net", "sslState": "Zakázat" "kryptografický otisk": hodnotu null, "toUpdate": hodnotu null, "virtuální IP adresy": null}, {"hostType": "Standard", "název": "www.contoso.com", "sslState": "SniEnabled", "kryptografický otisk": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00", "toUpdate": hodnotu null, "virtuální IP adresy": hodnotu null}], "názvy hostitelů": ["www.contoso.com", "< název_aplikace >. azurewebsites.net"], "hostNamesDisabled": hodnotu false, "hostingEnvironmentProfile": hodnotu null, "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s / < název_aplikace >", "isDefaultContainer": null, "druh": "WebApp", "lastModifiedTimeUtc": "2017-03-29T14:36:18.803333", "umístění": "Západní Evropa", "maxNumberOfWorkers": hodnotu null, "Mikroslužbu": "false" a "názvem": "< app_name >" "outboundIpAddresses": "13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1", "premiumAppDeployed": null, "repositorySiteName": "< app_name >", "vyhrazené": hodnotu false, "resourceGroup": "myResourceGroup", "scmSiteAlsoStopped": hodnotu false, "serverFarmId": "/ odběry/00000000-0000-0000-0000-000000000000/Skupinyprostředků/myResourceGroup nebo poskytovatelé/Microsof t.Web/serverfarms/myAppServicePlan", "siteConfig": null, "slotSwapStatus": null, "stavu": "Systém", "suspendedTill": null, "značky": null, "targetSwapSlot": null, "trafficManagerHostNames": null, "type": "Microsoft.Web/sites", "usageState": "Normální"}</span><span class="sxs-lookup"><span data-stu-id="36970-182">{ "availabilityState": "Normal", "clientAffinityEnabled": true, "clientCertEnabled": false, "cloningInfo": null, "containerSize": 0, "dailyMemoryTimeQuota": 0, "defaultHostName": "<app_name>.azurewebsites.net", "enabled": true, "enabledHostNames": [ "www.contoso.com", "<app_name>.azurewebsites.net", "<app_name>.scm.azurewebsites.net" ], "gatewaySiteName": null, "hostNameSslStates": [ { "hostType": "Standard", "name": "<app_name>.azurewebsites.net", "sslState": "Disabled", "thumbprint": null, "toUpdate": null, "virtualIp": null }, { "hostType": "Repository", "name": "<app_name>.scm.azurewebsites.net", "sslState": "Disabled", "thumbprint": null, "toUpdate": null, "virtualIp": null }, { "hostType": "Standard", "name": "www.contoso.com", "sslState": "SniEnabled", "thumbprint": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00", "toUpdate": null, "virtualIp": null } ], "hostNames": [ "www.contoso.com", "<app_name>.azurewebsites.net" ], "hostNamesDisabled": false, "hostingEnvironmentProfile": null, "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s/<app_name>", "isDefaultContainer": null, "kind": "WebApp", "lastModifiedTimeUtc": "2017-03-29T14:36:18.803333", "location": "West Europe", "maxNumberOfWorkers": null, "microService": "false", "name": "<app_name>", "outboundIpAddresses": "13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1", "premiumAppDeployed": null, "repositorySiteName": "<app_name>", "reserved": false, "resourceGroup": "myResourceGroup", "scmSiteAlsoStopped": false, "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsof t.Web/serverfarms/myAppServicePlan", "siteConfig": null, "slotSwapStatus": null, "state": "Running", "suspendedTill": null, "tags": null, "targetSwapSlot": null, "trafficManagerHostNames": null, "type": "Microsoft.Web/sites", "usageState": "Normal" }</span></span>

<span data-ttu-id="36970-183">V prohlížeči přejděte ke koncovému bodu HTTPS z vlastního názvu domény.</span><span class="sxs-lookup"><span data-stu-id="36970-183">In your browser, navigate to HTTPS endpoint of your custom domain name.</span></span> <span data-ttu-id="36970-184">Například:</span><span class="sxs-lookup"><span data-stu-id="36970-184">For example:</span></span>

```
https://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-ssl-success.png)  

## <a name="more-resources"></a><span data-ttu-id="36970-186">Další zdroje informací</span><span class="sxs-lookup"><span data-stu-id="36970-186">More resources</span></span>

<span data-ttu-id="36970-187">[Zakoupení a konfigurace vlastního názvu domény v Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[zakoupení a konfigurace certifikátu protokolu SSL pro Azure App Service](web-sites-purchase-ssl-web-site.md)</span><span class="sxs-lookup"><span data-stu-id="36970-187">[Buy and Configure a custom domain name in Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[Buy and Configure an SSL Certificate for your Azure App Service](web-sites-purchase-ssl-web-site.md)</span></span>
