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
# <a name="add-custom-domain-and-ssl-tooan-azure-web-app"></a><span data-ttu-id="a1518-104">Přidat vlastní domény a SSL tooan webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="a1518-104">Add custom domain and SSL tooan Azure web app</span></span>

<span data-ttu-id="a1518-105">Tento kurz ukazuje, jak tooquickly namapovat vlastní doménu název tooyour Azure webové aplikace a zabezpečte ji pomocí vlastní certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="a1518-105">This tutorial shows you how tooquickly map a custom domain name tooyour Azure web app and then secure it with a custom SSL certificate.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="a1518-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="a1518-106">Before you begin</span></span>

<span data-ttu-id="a1518-107">Před spuštěním této ukázce, nainstalujte hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) místně.</span><span class="sxs-lookup"><span data-stu-id="a1518-107">Before running this sample, install hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) locally.</span></span>

<span data-ttu-id="a1518-108">Je také nutné stránku konfigurace DNS toohello přístup pro správu pro váš poskytovatel příslušné domény.</span><span class="sxs-lookup"><span data-stu-id="a1518-108">You also need administrative access toohello DNS configuration page for your respective domain provider.</span></span> <span data-ttu-id="a1518-109">Například tooadd `www.contoso.com`, je nutné, aby záznamy DNS schopný tooconfigure toobe pro `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="a1518-109">For example, tooadd `www.contoso.com`, you need toobe able tooconfigure DNS entries for `contoso.com`.</span></span>

<span data-ttu-id="a1518-110">A konečně budete potřebovat platný. Soubor PFX _a_ jeho heslo pro certifikát SSL hello chcete tooupload a jejich vazby.</span><span class="sxs-lookup"><span data-stu-id="a1518-110">Lastly, you need a valid .PFX file _and_ its password for hello SSL certificate you want tooupload and bind.</span></span> <span data-ttu-id="a1518-111">Tento certifikát SSL musí být nakonfigurované toosecure hello vlastní název domény, které chcete.</span><span class="sxs-lookup"><span data-stu-id="a1518-111">This SSL certificate should be configured toosecure hello custom domain name you want.</span></span> <span data-ttu-id="a1518-112">V hello výše příklad, třeba zabezpečit svůj certifikát SSL `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="a1518-112">In hello above example, your SSL certificate should secure `www.contoso.com`.</span></span> 

## <a name="step-1---create-an-azure-web-app"></a><span data-ttu-id="a1518-113">Krok 1 – Vytvoření webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="a1518-113">Step 1 - Create an Azure web app</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="a1518-114">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="a1518-114">Log in tooAzure</span></span>

<span data-ttu-id="a1518-115">Snažíme se teď má toouse hello Azure CLI 2.0 v okno terminálu toocreate hello prostředky potřebné toohost naše aplikace Node.js v Azure.</span><span class="sxs-lookup"><span data-stu-id="a1518-115">We are now going toouse hello Azure CLI 2.0 in a terminal window toocreate hello resources needed toohost our Node.js app in Azure.</span></span>  <span data-ttu-id="a1518-116">Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="a1518-116">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span> 

```azurecli 
az login 
``` 
   
### <a name="create-a-resource-group"></a><span data-ttu-id="a1518-117">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="a1518-117">Create a resource group</span></span>   
<span data-ttu-id="a1518-118">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="a1518-118">Create a resource group with hello [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="a1518-119">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure, jako například webové aplikace, databáze a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="a1518-119">An Azure resource group is a logical container into which Azure resources like web apps, databases and storage accounts are deployed and managed.</span></span> 

```azurecli
az group create --name myResourceGroup --location westeurope 
```

<span data-ttu-id="a1518-120">toosee jaké možné hodnoty, které můžete použít pro `---location`, použijte hello `az appservice list-locations` příkazu příkazového řádku Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="a1518-120">toosee what possible values you can use for `---location`, use hello `az appservice list-locations` Azure CLI command.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="a1518-121">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="a1518-121">Create an App Service plan</span></span>

<span data-ttu-id="a1518-122">Vytvořte plán služby App Service s hello [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="a1518-122">Create an App Service plan with hello [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span> 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="a1518-123">Hello následující příklad vytvoří plán služby App Service s názvem `myAppServicePlan` pomocí hello **základní** cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="a1518-123">hello following example creates an App Service plan named `myAppServicePlan` using hello **Basic** pricing tier.</span></span>

<span data-ttu-id="a1518-124">Vytvořit plán aplikační služby az--název myAppServicePlan--resource-group myResourceGroup – sku B1</span><span class="sxs-lookup"><span data-stu-id="a1518-124">az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1</span></span>

<span data-ttu-id="a1518-125">Po vytvoření hello plán služby App Service, hello rozhraní příkazového řádku Azure znázorňuje následující ukázka podobné toohello informace.</span><span class="sxs-lookup"><span data-stu-id="a1518-125">When hello App Service plan has been created, hello Azure CLI shows information similar toohello following example.</span></span> 

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

## <a name="create-a-web-app"></a><span data-ttu-id="a1518-126">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="a1518-126">Create a web app</span></span>

<span data-ttu-id="a1518-127">Teď, když byl vytvořen plán služby App Service, vytvoření webové aplikace v rámci hello `myAppServicePlan` plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="a1518-127">Now that an App Service plan has been created, create a web app within hello `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="a1518-128">Hello webové aplikace poskytuje jste jste hostování toodeploy místo kódu a poskytuje také adresu URL pro vás tooview hello nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="a1518-128">hello web app gives your a hosting space toodeploy your code as well as provides a URL for you tooview hello deployed application.</span></span> <span data-ttu-id="a1518-129">Použití hello [vytvořit webové služby App Service az](/cli/azure/appservice/web#create) příkaz toocreate hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a1518-129">Use hello [az appservice web create](/cli/azure/appservice/web#create) command toocreate hello web app.</span></span> 

<span data-ttu-id="a1518-130">V příkazu hello níže nahraďte prosím svůj vlastní název jedinečný aplikace, kde uvidíte hello `<app_name>` zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="a1518-130">In hello command below, please substitute your own unique app name where you see hello `<app_name>` placeholder.</span></span> <span data-ttu-id="a1518-131">Tento jedinečný název se použije jako součást hello hello výchozí název domény pro webovou aplikaci hello, takže název hello musí toobe jedinečný mezi všechny aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="a1518-131">This unique name will be used as hello part of hello default domain name for hello web app, so hello name needs toobe unique across all apps in Azure.</span></span> <span data-ttu-id="a1518-132">Později můžete namapovat všechny vlastní DNS záznam toohello webové aplikace v ještě před zveřejněním tooyour uživatele.</span><span class="sxs-lookup"><span data-stu-id="a1518-132">You can later map any custom DNS entry toohello web app before you expose it tooyour users.</span></span> 

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan 
```

<span data-ttu-id="a1518-133">Po vytvoření webové aplikace hello hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="a1518-133">When hello web app has been created, hello Azure CLI shows information similar toohello following example.</span></span> 

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

<span data-ttu-id="a1518-134">Z hello výstup JSON `defaultHostName` ukazuje výchozí název domény vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a1518-134">From hello JSON output, `defaultHostName` shows your web app's default domain name.</span></span> <span data-ttu-id="a1518-135">V prohlížeči přejděte toothis adresu.</span><span class="sxs-lookup"><span data-stu-id="a1518-135">In your browser, navigate toothis address.</span></span>

```
http://<app_name>.azurewebsites.net 
``` 
 
![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-created.png)  

## <a name="step-2---configure-dns-mapping"></a><span data-ttu-id="a1518-137">Krok 2: Konfigurace mapování DNS</span><span class="sxs-lookup"><span data-stu-id="a1518-137">Step 2 - Configure DNS mapping</span></span>

<span data-ttu-id="a1518-138">V tomto kroku přidáte mapování z vlastní domény tooyour webovou aplikaci na výchozí název domény, `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="a1518-138">In this step, you add a mapping from a custom domain tooyour web app's default domain name, `<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="a1518-139">Tento krok se obvykle, proveďte ve zprostředkovateli domény webu.</span><span class="sxs-lookup"><span data-stu-id="a1518-139">Typically, you perform this step in your domain provider's website.</span></span> <span data-ttu-id="a1518-140">Každý Registrátor web je poněkud lišit, a proto byste se měli obrátit dokumentaci svého poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="a1518-140">Every domain registrar's website is slightly different, so you should consult your provider's documentation.</span></span> <span data-ttu-id="a1518-141">Tady jsou ale některé obecné pokyny.</span><span class="sxs-lookup"><span data-stu-id="a1518-141">However, here are some general guidelines.</span></span> 

### <a name="navigate-tootoodns-management-page"></a><span data-ttu-id="a1518-142">Přejděte na stránku Správa tootooDNS</span><span class="sxs-lookup"><span data-stu-id="a1518-142">Navigate tootooDNS management page</span></span>

<span data-ttu-id="a1518-143">Přihlaste se nejdřív tooyour doménového registrátora webu.</span><span class="sxs-lookup"><span data-stu-id="a1518-143">First, log in tooyour domain registrar's website.</span></span>  

<span data-ttu-id="a1518-144">Vyhledejte hello stránky pro správu záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="a1518-144">Then, find hello page for managing DNS records.</span></span> <span data-ttu-id="a1518-145">Vyhledejte odkazy nebo oblasti hello webu s názvem bez přípony **název domény**, **DNS**, nebo **název serveru správy**.</span><span class="sxs-lookup"><span data-stu-id="a1518-145">Look for links or areas of hello site labeled **Domain Name**, **DNS**, or **Name Server Management**.</span></span> <span data-ttu-id="a1518-146">Často můžete najít hello odkaz a zobrazení informací o vašem účtu a podívat se na odkaz, jako **mé domény**.</span><span class="sxs-lookup"><span data-stu-id="a1518-146">Often, you can find hello link by viewing your account information, and then looking for a link such as **My domains**.</span></span>

<span data-ttu-id="a1518-147">Jakmile zjistíte, tato stránka, vyhledejte odkaz, který umožňuje přidat nebo upravit záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="a1518-147">Once you find this page, look for a link that lets you add or edit DNS records.</span></span> <span data-ttu-id="a1518-148">To může být **souboru zóny** nebo **záznamy DNS** odkaz, nebo **pokročilou konfiguraci** odkaz.</span><span class="sxs-lookup"><span data-stu-id="a1518-148">This might be a **Zone file** or **DNS Records** link, or an **Advanced configuration** link.</span></span>

### <a name="create-a-cname-record"></a><span data-ttu-id="a1518-149">Vytvořte záznam CNAME</span><span class="sxs-lookup"><span data-stu-id="a1518-149">Create a CNAME record</span></span>

<span data-ttu-id="a1518-150">Přidejte záznam CNAME, který mapuje název domény výchozí hello požadované subdomény název tooyour webové aplikace (`<app_name>.azurewebsites.net`, kde `<app_name>` je jedinečný název vaší aplikace).</span><span class="sxs-lookup"><span data-stu-id="a1518-150">Add a CNAME record that maps hello desired subdomain name tooyour web app's default domain name (`<app_name>.azurewebsites.net`, where `<app_name>` is your app's unique name).</span></span>

<span data-ttu-id="a1518-151">Pro hello `www.contoso.com` například vytvořit záznam CNAME, který se mapuje hello `www` hostname příliš`<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="a1518-151">For hello `www.contoso.com` example, you create a CNAME that maps hello `www` hostname too`<app_name>.azurewebsites.net`.</span></span>

## <a name="step-3---configure-hello-custom-domain-on-your-web-app"></a><span data-ttu-id="a1518-152">Krok 3 – konfigurace vlastní domény hello na vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="a1518-152">Step 3 - Configure hello custom domain on your web app</span></span>

<span data-ttu-id="a1518-153">Po dokončení konfigurace mapování názvu hostitele hello ve zprostředkovateli domény webu, jste připravené tooconfigure hello vlastní doménu na vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a1518-153">When you finish configuring hello hostname mapping in your domain provider's website, you're ready tooconfigure hello custom domain on your web app.</span></span> <span data-ttu-id="a1518-154">Použití hello [přidat název hostitele konfigurace az služby App Service web](/cli/azure/appservice/web/config/hostname#add) příkaz tooadd tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="a1518-154">Use hello [az appservice web config hostname add](/cli/azure/appservice/web/config/hostname#add) command tooadd this configuration.</span></span> 

<span data-ttu-id="a1518-155">V příkazu hello níže nahraďte `<app_name>` s jedinečným názvem aplikace a < your_custom_domain > s hello plně kvalifikovaný vlastní název domény (například `www.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="a1518-155">In hello command below, please substitute `<app_name>` with your unique app name, and <your_custom_domain> with hello fully qualified custom domain name (e.g. `www.contoso.com`).</span></span> 

```azurecli
az appservice web config hostname add --webapp <app_name> --resource-group myResourceGroup --name <your_custom_domain>
```

<span data-ttu-id="a1518-156">vlastní domény Hello je nyní plně namapované tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a1518-156">hello custom domain now is fully mapped tooyour web app.</span></span> <span data-ttu-id="a1518-157">V prohlížeči přejděte toohello vlastní název domény.</span><span class="sxs-lookup"><span data-stu-id="a1518-157">In your browser, navigate toohello custom domain name.</span></span> <span data-ttu-id="a1518-158">Například:</span><span class="sxs-lookup"><span data-stu-id="a1518-158">For example:</span></span>

```
http://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-custom-domain.png)  

## <a name="step-4---bind-a-custom-ssl-certificate-tooyour-web-app"></a><span data-ttu-id="a1518-160">Krok 4 – vytvoření vazby vlastní tooyour webové aplikace certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="a1518-160">Step 4 - Bind a custom SSL certificate tooyour web app</span></span>

<span data-ttu-id="a1518-161">Nyní máte webové aplikace Azure, s názvem domény hello, které chcete v adresním řádku prohlížeče hello.</span><span class="sxs-lookup"><span data-stu-id="a1518-161">You now have an Azure web app, with hello domain name you want in hello browser's address bar.</span></span> <span data-ttu-id="a1518-162">Ale pokud přejdete toohello `https://<your_custom_domain>` nyní se zobrazí chyba certifikátu.</span><span class="sxs-lookup"><span data-stu-id="a1518-162">However, if you navigate toohello `https://<your_custom_domain>` now, you get a certificate error.</span></span> 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-cert-error.png)  

<span data-ttu-id="a1518-164">K této chybě dojde, protože webové aplikace ještě nemá vazbu certifikátu SSL, který odpovídá vlastního názvu domény.</span><span class="sxs-lookup"><span data-stu-id="a1518-164">This error occurs because your web app doesn't yet have an SSL certificate binding that matches your custom domain name.</span></span> <span data-ttu-id="a1518-165">Však není dojde k chybě, pokud přejdete příliš`https://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="a1518-165">However, you don't get an error if you navigate too`https://<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="a1518-166">Důvodem je, že vaše aplikace, stejně jako všechny aplikace služby Azure App Service, je zabezpečen hello certifikát SSL pro hello `*.azurewebsites.net` zástupné domény ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="a1518-166">This is because your app, as well as all Azure App Service apps, is secured with hello SSL certificate for hello `*.azurewebsites.net` wildcard domain by default.</span></span> 

<span data-ttu-id="a1518-167">V pořadí tooaccess vaší webové aplikace pomocí vlastního názvu domény, musíte certifikát SSL hello toobind pro vaši vlastní doménu toohello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a1518-167">In order tooaccess your web app by your custom domain name, you need toobind hello SSL certificate for your custom domain toohello web app.</span></span> <span data-ttu-id="a1518-168">Provedete ho v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="a1518-168">You will do it in this step.</span></span> 

### <a name="upload-hello-ssl-certificate"></a><span data-ttu-id="a1518-169">Nahrajte certifikát SSL hello</span><span class="sxs-lookup"><span data-stu-id="a1518-169">Upload hello SSL certificate</span></span>

<span data-ttu-id="a1518-170">Nahrát hello certifikát SSL pro webovou aplikaci tooyour vlastní doménu pomocí hello [az služby App Service web konfigurace ssl nahrávání](/cli/azure/appservice/web/config/ssl#upload) příkaz.</span><span class="sxs-lookup"><span data-stu-id="a1518-170">Upload hello SSL certificate for your custom domain tooyour web app by using hello [az appservice web config ssl upload](/cli/azure/appservice/web/config/ssl#upload) command.</span></span>

<span data-ttu-id="a1518-171">V příkazu hello níže nahraďte `<app_name>` s názvem vaší jedinečné aplikace `<path_to_ptx_file>` s tooyour cesta hello. Soubor PFX a `<password>` heslem svůj certifikát.</span><span class="sxs-lookup"><span data-stu-id="a1518-171">In hello command below, please substitute `<app_name>` with your unique app name, `<path_to_ptx_file>` with hello path tooyour .PFX file, and `<password>` with your certificate's password.</span></span> 

```azurecli
az appservice web config ssl upload --name <app_name> --resource-group myResourceGroup --certificate-file <path_to_pfx_file> --certificate-password <password> 
```

<span data-ttu-id="a1518-172">Při odeslání certifikátu hello hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="a1518-172">When hello certificate is uploaded, hello Azure CLI shows information similar toohello following example:</span></span>

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

<span data-ttu-id="a1518-173">Z hello výstup JSON `thumbprint` ukazuje odeslaný certifikát kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="a1518-173">From hello JSON output, `thumbprint` shows your uploaded certificate's thumbprint.</span></span> <span data-ttu-id="a1518-174">Zkopírujte jeho hodnoty pro další krok hello.</span><span class="sxs-lookup"><span data-stu-id="a1518-174">Copy its value for hello next step.</span></span>

### <a name="bind-hello-uploaded-ssl-certificate-toohello-web-app"></a><span data-ttu-id="a1518-175">Vytvoření vazby hello nahrán SSL certifikát toohello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="a1518-175">Bind hello uploaded SSL certificate toohello web app</span></span>

<span data-ttu-id="a1518-176">Webové aplikace teď má hello vlastní název domény, který má být a má také certifikát SSL, který zabezpečuje vlastní domény.</span><span class="sxs-lookup"><span data-stu-id="a1518-176">Your web app now has hello custom domain name you want, and it also has a SSL certificate that secures that custom domain.</span></span> <span data-ttu-id="a1518-177">Hello pouze jedinou levém toodo je toobind hello odeslaný certifikát toohello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a1518-177">hello only thing left toodo is toobind hello uploaded certificate toohello web app.</span></span> <span data-ttu-id="a1518-178">Můžete to provést pomocí hello [az služby App Service web konfigurace protokolu ssl vazby](/cli/azure/appservice/web/config/ssl#bind) příkaz.</span><span class="sxs-lookup"><span data-stu-id="a1518-178">You do this by using hello [az appservice web config ssl bind](/cli/azure/appservice/web/config/ssl#bind) command.</span></span>

<span data-ttu-id="a1518-179">V příkazu hello níže nahraďte `<app_name>` názvem vaší aplikace jedinečný a `<thumbprint-from-previous-output>` s hello kryptografický otisk certifikátu, kterou můžete získat z předchozí příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="a1518-179">In hello command below, please substitute `<app_name>` with your unique app name and `<thumbprint-from-previous-output>` with hello certificate thumbprint that you get from hello previous command.</span></span> 

<span data-ttu-id="a1518-180">AZ služby App Service web konfigurace protokolu ssl vazby – název < název_aplikace >--resource-group myResourceGroup – SNI – ssl-type-kryptografický otisk certifikátu < kryptografický otisk z – předchozí output ></span><span class="sxs-lookup"><span data-stu-id="a1518-180">az appservice web config ssl bind --name <app_name> --resource-group myResourceGroup --certificate-thumbprint <thumbprint-from-previous-output> --ssl-type SNI</span></span>

<span data-ttu-id="a1518-181">Když je certifikát hello vázané tooyour webové aplikace, hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="a1518-181">When hello certificate is bound tooyour web app, hello Azure CLI shows information similar toohello following example:</span></span>

<span data-ttu-id="a1518-182">{"stavu": "Normální", "clientAffinityEnabled": nastavena hodnota true, "clientCertEnabled": hodnotu false, "cloningInfo": null, "containerSize": 0, "dailyMemoryTimeQuota": 0, "defaultHostName": "< název_aplikace >. azurewebsites.net", "povoleno": true "enabledHostNames": ["www.contoso.com", "< název_aplikace >. azurewebsites.net", "< název_aplikace >. scm.azurewebsites.net"], "gatewaySiteName": null, "hostNameSslStates": [{"hostType": "Standard", "název": "< název_aplikace >. azurewebsites.net", "sslState": "Zakázáno", "kryptografický otisk": null, "toUpdate": null, "virtuální IP adresy": hodnotu null}, {"hostType": "Úložiště", "name": "< název_aplikace >. scm.azurewebsites.net", "sslState": "Zakázat" "kryptografický otisk": hodnotu null, "toUpdate": hodnotu null, "virtuální IP adresy": null}, {"hostType": "Standard", "název": "www.contoso.com", "sslState": "SniEnabled", "kryptografický otisk": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00", "toUpdate": hodnotu null, "virtuální IP adresy": hodnotu null}], "názvy hostitelů": ["www.contoso.com", "< název_aplikace >. azurewebsites.net"], "hostNamesDisabled": hodnotu false, "hostingEnvironmentProfile": hodnotu null, "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s / < název_aplikace >", "isDefaultContainer": null, "druh": "WebApp", "lastModifiedTimeUtc": "2017-03-29T14:36:18.803333", "umístění": "Západní Evropa", "maxNumberOfWorkers": hodnotu null, "Mikroslužbu": "false" a "názvem": "< app_name >" "outboundIpAddresses": "13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1", "premiumAppDeployed": null, "repositorySiteName": "< app_name >", "vyhrazené": hodnotu false, "resourceGroup": "myResourceGroup", "scmSiteAlsoStopped": hodnotu false, "serverFarmId": "/ odběry/00000000-0000-0000-0000-000000000000/Skupinyprostředků/myResourceGroup nebo poskytovatelé/Microsof t.Web/serverfarms/myAppServicePlan", "siteConfig": null, "slotSwapStatus": null, "stavu": "Systém", "suspendedTill": null, "značky": null, "targetSwapSlot": null, "trafficManagerHostNames": null, "type": "Microsoft.Web/sites", "usageState": "Normální"}</span><span class="sxs-lookup"><span data-stu-id="a1518-182">{ "availabilityState": "Normal", "clientAffinityEnabled": true, "clientCertEnabled": false, "cloningInfo": null, "containerSize": 0, "dailyMemoryTimeQuota": 0, "defaultHostName": "<app_name>.azurewebsites.net", "enabled": true, "enabledHostNames": [ "www.contoso.com", "<app_name>.azurewebsites.net", "<app_name>.scm.azurewebsites.net" ], "gatewaySiteName": null, "hostNameSslStates": [ { "hostType": "Standard", "name": "<app_name>.azurewebsites.net", "sslState": "Disabled", "thumbprint": null, "toUpdate": null, "virtualIp": null }, { "hostType": "Repository", "name": "<app_name>.scm.azurewebsites.net", "sslState": "Disabled", "thumbprint": null, "toUpdate": null, "virtualIp": null }, { "hostType": "Standard", "name": "www.contoso.com", "sslState": "SniEnabled", "thumbprint": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00", "toUpdate": null, "virtualIp": null } ], "hostNames": [ "www.contoso.com", "<app_name>.azurewebsites.net" ], "hostNamesDisabled": false, "hostingEnvironmentProfile": null, "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s/<app_name>", "isDefaultContainer": null, "kind": "WebApp", "lastModifiedTimeUtc": "2017-03-29T14:36:18.803333", "location": "West Europe", "maxNumberOfWorkers": null, "microService": "false", "name": "<app_name>", "outboundIpAddresses": "13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1", "premiumAppDeployed": null, "repositorySiteName": "<app_name>", "reserved": false, "resourceGroup": "myResourceGroup", "scmSiteAlsoStopped": false, "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsof t.Web/serverfarms/myAppServicePlan", "siteConfig": null, "slotSwapStatus": null, "state": "Running", "suspendedTill": null, "tags": null, "targetSwapSlot": null, "trafficManagerHostNames": null, "type": "Microsoft.Web/sites", "usageState": "Normal" }</span></span>

<span data-ttu-id="a1518-183">V prohlížeči přejděte tooHTTPS koncový bod vlastního názvu domény.</span><span class="sxs-lookup"><span data-stu-id="a1518-183">In your browser, navigate tooHTTPS endpoint of your custom domain name.</span></span> <span data-ttu-id="a1518-184">Například:</span><span class="sxs-lookup"><span data-stu-id="a1518-184">For example:</span></span>

```
https://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-ssl-success.png)  

## <a name="more-resources"></a><span data-ttu-id="a1518-186">Další zdroje informací</span><span class="sxs-lookup"><span data-stu-id="a1518-186">More resources</span></span>

<span data-ttu-id="a1518-187">[Zakoupení a konfigurace vlastního názvu domény v Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[zakoupení a konfigurace certifikátu protokolu SSL pro Azure App Service](web-sites-purchase-ssl-web-site.md)</span><span class="sxs-lookup"><span data-stu-id="a1518-187">[Buy and Configure a custom domain name in Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[Buy and Configure an SSL Certificate for your Azure App Service](web-sites-purchase-ssl-web-site.md)</span></span>
