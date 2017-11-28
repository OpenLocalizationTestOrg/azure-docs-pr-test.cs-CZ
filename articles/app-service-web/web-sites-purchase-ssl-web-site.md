---
title: "aaaAdd protokolem SSL certifikát tooyour aplikace Azure App Service | Microsoft Docs"
description: "Zjistěte, jak tooadd protokolem SSL certifikát tooyour aplikaci služby App Service."
services: app-service
documentationcenter: .net
author: ahmedelnably
manager: stefsch
editor: cephalin
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2016
ms.author: apurvajo
ms.openlocfilehash: f4652794ba745790a073264f6a102c64c73e8db0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a><span data-ttu-id="97053-103">Koupě a konfigurace certifikátu SSL pro službu Azure App Service</span><span class="sxs-lookup"><span data-stu-id="97053-103">Buy and Configure an SSL Certificate for your Azure App Service</span></span>

<span data-ttu-id="97053-104">V tomto kurzu bude Zabezpečte svoji webovou aplikaci pomocí nákupu certifikát protokolu SSL pro vaše  **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, bezpečně uložením do [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis)a přiřadí se s vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="97053-104">In this tutorial, you will secure your web app by purchasing an SSL certificate for your **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, securely storing it in [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis), and associating it with a custom domain.</span></span>

## <a name="step-1---log-in-tooazure"></a><span data-ttu-id="97053-105">Krok 1 – přihlášení tooAzure</span><span class="sxs-lookup"><span data-stu-id="97053-105">Step 1 - Log in tooAzure</span></span>

<span data-ttu-id="97053-106">Přihlaste se toohello portál Azure na http://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="97053-106">Log in toohello Azure portal at http://portal.azure.com</span></span>

## <a name="step-2---place-an-ssl-certificate-order"></a><span data-ttu-id="97053-107">Krok 2 – umístit pořadí certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="97053-107">Step 2 - Place an SSL Certificate order</span></span>

<span data-ttu-id="97053-108">Certifikát SSL pořadí můžete umístit tak, že vytvoříte novou [certifikát služby aplikace](https://portal.azure.com/#create/Microsoft.SSL) v hello **portál Azure**.</span><span class="sxs-lookup"><span data-stu-id="97053-108">You can place an SSL Certificate order by creating a new [App Service Certificate](https://portal.azure.com/#create/Microsoft.SSL) In hello **Azure portal**.</span></span>

![Vytvoření certifikátu](./media/app-service-web-purchase-ssl-web-site/createssl.png)

<span data-ttu-id="97053-110">Zadejte popisný **název** certifikátů pro zabezpečení SSL a zadejte hello **název domény**</span><span class="sxs-lookup"><span data-stu-id="97053-110">Enter a friendly **Name** for your SSL certificate and enter hello **Domain Name**</span></span>

> [!NOTE]
> <span data-ttu-id="97053-111">To je jedním z nejdůležitějších části hello procesu nákupu hello.</span><span class="sxs-lookup"><span data-stu-id="97053-111">This is one of hello most critical parts of hello purchase process.</span></span> <span data-ttu-id="97053-112">Ujistěte se, že tooenter Opravte název hostitele (vlastní domény), které chcete tooprotect s tímto certifikátem.</span><span class="sxs-lookup"><span data-stu-id="97053-112">Make sure tooenter correct host name (custom domain) that you want tooprotect with this certificate.</span></span> <span data-ttu-id="97053-113">**NECHCETE** připojit hello název hostitele s WWW.</span><span class="sxs-lookup"><span data-stu-id="97053-113">**DO NOT** append hello Host name with WWW.</span></span> 
>

<span data-ttu-id="97053-114">Vyberte vaše **předplatné**, **skupiny prostředků**, a **certifikátu SKU**</span><span class="sxs-lookup"><span data-stu-id="97053-114">Select your **Subscription**, **Resource Group**, and **Certificate SKU**</span></span>

> [!WARNING]
> <span data-ttu-id="97053-115">Aplikace služby certifikáty lze použít pouze v případě ostatních služeb aplikace v rámci hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="97053-115">App Service Certificates can only be used by other App Services within hello same subscription.</span></span>  
>

## <a name="step-3---store-hello-certificate-in-azure-key-vault"></a><span data-ttu-id="97053-116">Krok 3 – certifikát hello úložiště v Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="97053-116">Step 3 - Store hello certificate in Azure Key Vault</span></span>

> [!NOTE]
> <span data-ttu-id="97053-117">[Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) je služba Azure, která pomáhá chránit kryptografické klíče a tajné klíče používané cloudovými aplikacemi a službami.</span><span class="sxs-lookup"><span data-stu-id="97053-117">[Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) is an Azure service that helps safeguard cryptographic keys and secrets used by cloud applications and services.</span></span>
>

<span data-ttu-id="97053-118">Po dokončení nákupu certifikát SSL hello potřebujete tooopen [služby App Service Certificate](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) okna prostředků.</span><span class="sxs-lookup"><span data-stu-id="97053-118">Once hello SSL Certificate purchase is complete, you need tooopen [App Service Certificates](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) Resource blade.</span></span>

![Vložit obrázek připravené toostore v KV](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

<span data-ttu-id="97053-120">Si všimnete, že je stav certifikátu **"Čekající vystavení"** jak několika více kroků toocomplete musíte před zahájením s použitím tohoto certifikátu.</span><span class="sxs-lookup"><span data-stu-id="97053-120">You will notice that Certificate status is **“Pending Issuance”** as there are few more steps you need toocomplete before you can start using this certificate.</span></span>

<span data-ttu-id="97053-121">Klikněte na tlačítko **konfigurace certifikátu** v okně Vlastnosti certifikátu a klikněte na **krok 1: úložiště** toostore tento certifikát v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="97053-121">Click **Certificate Configuration** inside Certificate Properties blade and Click on **Step 1: Store** toostore this certificate in Azure Key Vault.</span></span>

<span data-ttu-id="97053-122">Z **klíč trezoru stav** okně klikněte na tlačítko **klíč trezoru úložiště** toochoose existující toostore Key Vault tento certifikát **nebo vytvořit nový klíč trezoru** toocreate nový klíč trezoru ve stejném předplatném nebo skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="97053-122">From **Key Vault Status** Blade, click **Key Vault Repository** toochoose an existing Key Vault toostore this certificate **OR Create New Key Vault** toocreate new Key Vault inside same subscription and resource group.</span></span>

> [!NOTE]
> <span data-ttu-id="97053-123">Azure Key Vault má minimální poplatky za uložení tohoto certifikátu.</span><span class="sxs-lookup"><span data-stu-id="97053-123">Azure Key Vault has minimal charges for storing this certificate.</span></span>
> <span data-ttu-id="97053-124">Další informace najdete v tématu  **[klíč trezoru podrobnostmi o cenách Azure](https://azure.microsoft.com/pricing/details/key-vault/)**.</span><span class="sxs-lookup"><span data-stu-id="97053-124">For more information, see **[Azure Key Vault Pricing Details](https://azure.microsoft.com/pricing/details/key-vault/)**.</span></span>
>

<span data-ttu-id="97053-125">Jakmile vyberete hello klíč trezoru úložiště toostore tento certifikát, hello **úložiště** možnost by se měla zobrazit úspěch.</span><span class="sxs-lookup"><span data-stu-id="97053-125">Once you have selected hello Key Vault Repository toostore this certificate in, hello **Store** option should show success.</span></span>

![Vložit obrázek úspěchu úložiště v KV](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-hello-domain-ownership"></a><span data-ttu-id="97053-127">Krok 4 – ověření hello vlastnictví domény</span><span class="sxs-lookup"><span data-stu-id="97053-127">Step 4 - Verify hello Domain Ownership</span></span>

> [!NOTE]
> <span data-ttu-id="97053-128">Existují tři typy ověření domény nepodporuje App service Certificate: ověření domény, e-mailu, ručně.</span><span class="sxs-lookup"><span data-stu-id="97053-128">There are three types of domain verification supported by App service Certificates: Domain, Mail, Manual Verification.</span></span> <span data-ttu-id="97053-129">Tyto mechanismy jsou popsány v další podrobnosti v hello [Advanced části](#advanced).</span><span class="sxs-lookup"><span data-stu-id="97053-129">These are explained in more details in hello [Advanced section](#advanced).</span></span>

<span data-ttu-id="97053-130">Z hello stejné **konfigurace certifikátu** okno, které jste použili v kroku 3, klikněte na tlačítko **krok 2: ověření**.</span><span class="sxs-lookup"><span data-stu-id="97053-130">From hello same **Certificate Configuration** blade you used in Step 3, click **Step 2: Verify**.</span></span>

<span data-ttu-id="97053-131">**Ověření domény** je nejvhodnější proces hello **pouze v případě** máte  **[zakoupili vaši vlastní doménu služby Azure App Service.](custom-dns-web-site-buydomains-web-app.md)**</span><span class="sxs-lookup"><span data-stu-id="97053-131">**Domain Verification** This is hello most convenient process **ONLY IF** you have **[purchased your custom domain from Azure App Service.](custom-dns-web-site-buydomains-web-app.md)**</span></span>
<span data-ttu-id="97053-132">Klikněte na **ověřte** tlačítko toocomplete tento krok.</span><span class="sxs-lookup"><span data-stu-id="97053-132">Click on **Verify** button toocomplete this step.</span></span>

![Vložit obrázek ověření domény](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

<span data-ttu-id="97053-134">Po kliknutí na **ověřte**, použijte hello **aktualizovat** tlačítko dokud hello **ověřte** možnost by se měla zobrazit úspěch.</span><span class="sxs-lookup"><span data-stu-id="97053-134">After clicking **Verify**, use hello **Refresh** button until hello **Verify** option should show success.</span></span>

![Vložit obrázek ověřit úspěšné provedení v KV](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-tooapp-service-app"></a><span data-ttu-id="97053-136">Krok 5 – přiřadit certifikát tooApp služby App Service</span><span class="sxs-lookup"><span data-stu-id="97053-136">Step 5 - Assign Certificate tooApp Service App</span></span>

> [!NOTE]
> <span data-ttu-id="97053-137">Před provedením kroků hello v této části, musí mít přidružené vlastního názvu domény s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="97053-137">Before performing hello steps in this section, you must have associated a custom domain name with your app.</span></span> <span data-ttu-id="97053-138">Další informace najdete v tématu  **[konfigurace vlastního názvu domény pro webovou aplikaci.](app-service-web-tutorial-custom-domain.md)**</span><span class="sxs-lookup"><span data-stu-id="97053-138">For more information, see **[Configuring a custom domain name for a web app.](app-service-web-tutorial-custom-domain.md)**</span></span>
>

<span data-ttu-id="97053-139">V hello  **[portál Azure](https://portal.azure.com/)**, klikněte na tlačítko hello **služby App Service** možnost na levé straně hello stránku hello.</span><span class="sxs-lookup"><span data-stu-id="97053-139">In hello **[Azure portal](https://portal.azure.com/)**, click hello **App Service** option on hello left of hello page.</span></span>

<span data-ttu-id="97053-140">Klikněte na název hello toowhich vaší aplikace, kterou chcete tooassign tento certifikát.</span><span class="sxs-lookup"><span data-stu-id="97053-140">Click hello name of your app toowhich you want tooassign this certificate.</span></span>

<span data-ttu-id="97053-141">V hello **nastavení**, klikněte na tlačítko **certifikáty SSL**.</span><span class="sxs-lookup"><span data-stu-id="97053-141">In hello **Settings**, click **SSL certificates**.</span></span>

<span data-ttu-id="97053-142">Klikněte na tlačítko **importovat aplikaci služby certifikát** a vyberte hello certifikátů, které jste zakoupili.</span><span class="sxs-lookup"><span data-stu-id="97053-142">Click **Import App Service Certificate** and select hello certificate that you just purchased.</span></span>

![Vložit obrázek importovat certifikát](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

<span data-ttu-id="97053-144">V hello **vazby ssl** část kliknutím na **přidat vazby**, použijte hello rozevíracích seznamů tooselect hello domény název toosecure pomocí protokolu SSL a hello toouse certifikátu.</span><span class="sxs-lookup"><span data-stu-id="97053-144">In hello **ssl bindings** section Click on **Add bindings**, and use hello dropdowns tooselect hello domain name toosecure with SSL, and hello certificate toouse.</span></span> <span data-ttu-id="97053-145">Můžete také vybrat zda toouse  **[indikace názvu serveru (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  nebo IP na základě protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="97053-145">You may also select whether toouse **[Server Name Indication (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** or IP based SSL.</span></span>

![Vložit obrázek vazby SSL](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

<span data-ttu-id="97053-147">Klikněte na tlačítko **přidat vazbu** toosave hello změny a povolit protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="97053-147">Click **Add Binding** toosave hello changes and enable SSL.</span></span>

> [!NOTE]
> <span data-ttu-id="97053-148">Pokud jste vybrali **IP na základě SSL** a vaše vlastní doména je nakonfigurována pomocí záznamu A, musíte provést následující další kroky hello.</span><span class="sxs-lookup"><span data-stu-id="97053-148">If you selected **IP based SSL** and your custom domain is configured using an A record, you must perform hello following additional steps.</span></span> <span data-ttu-id="97053-149">Tyto mechanismy jsou popsány v další podrobnosti v hello [Advanced části](#Advanced).</span><span class="sxs-lookup"><span data-stu-id="97053-149">These are explained in more details in hello [Advanced section](#Advanced).</span></span>

<span data-ttu-id="97053-150">V tomto okamžiku jste by měl být schopný toovisit vaší aplikace pomocí `HTTPS://` místo `HTTP://` tooverify, který hello certifikát nebyl nakonfigurován správně.</span><span class="sxs-lookup"><span data-stu-id="97053-150">At this point, you should be able toovisit your app using `HTTPS://` instead of `HTTP://` tooverify that hello certificate has been configured correctly.</span></span>

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a><span data-ttu-id="97053-151">Krok 6: úlohy správy</span><span class="sxs-lookup"><span data-stu-id="97053-151">Step 6 - Management tasks</span></span>

### <a name="azure-cli"></a><span data-ttu-id="97053-152">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="97053-152">Azure CLI</span></span>

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a><span data-ttu-id="97053-153">PowerShell</span><span class="sxs-lookup"><span data-stu-id="97053-153">PowerShell</span></span>

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="advanced"></a><span data-ttu-id="97053-154">Advanced</span><span class="sxs-lookup"><span data-stu-id="97053-154">Advanced</span></span>

### <a name="verifying-domain-ownership"></a><span data-ttu-id="97053-155">Ověření vlastnictví domény</span><span class="sxs-lookup"><span data-stu-id="97053-155">Verifying Domain Ownership</span></span>

<span data-ttu-id="97053-156">Existují dva další typy ověření domény nepodporuje App service Certificate: e-mailu a ruční ověření.</span><span class="sxs-lookup"><span data-stu-id="97053-156">There are two more types of domain verification supported by App service Certificates: Mail, and Manual Verification.</span></span>

#### <a name="mail-verification"></a><span data-ttu-id="97053-157">Ověření e-mailu</span><span class="sxs-lookup"><span data-stu-id="97053-157">Mail Verification</span></span>

<span data-ttu-id="97053-158">Ověření e-mailu už jsou poslané toohello e-mailové adresy přidruženého tuto vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="97053-158">Verification email has already been sent toohello Email Address(es) associated with this custom domain.</span></span>
<span data-ttu-id="97053-159">krok ověření e-mailu toocomplete hello, otevřete hello e-mailu a klikněte na odkaz ověření hello.</span><span class="sxs-lookup"><span data-stu-id="97053-159">toocomplete hello Email verification step, open hello email and click hello verification link.</span></span>

![Vložit obrázek ověření e-mailu](./media/app-service-web-purchase-ssl-web-site/KVVerifyEmailSuccess.png)

<span data-ttu-id="97053-161">Pokud potřebujete tooresend hello ověření e-mailu, klikněte na tlačítko hello **znovu odeslat E-mail** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="97053-161">If you need tooresend hello verification email, click hello **Resend Email** button.</span></span>

#### <a name="manual-verification"></a><span data-ttu-id="97053-162">Ruční ověření</span><span class="sxs-lookup"><span data-stu-id="97053-162">Manual Verification</span></span>

> [!IMPORTANT]
> <span data-ttu-id="97053-163">Ověření webové stránky HTML (funguje pouze u standardní SKU certifikát)</span><span class="sxs-lookup"><span data-stu-id="97053-163">HTML Web Page Verification (only works with Standard Certificate SKU)</span></span>
>

1. <span data-ttu-id="97053-164">Vytvořte soubor HTML s názvem **"starfield.html"**</span><span class="sxs-lookup"><span data-stu-id="97053-164">Create an HTML file named **"starfield.html"**</span></span>

1. <span data-ttu-id="97053-165">Obsah tohoto souboru musí být hello přesný název hello domény ověření tokenu.</span><span class="sxs-lookup"><span data-stu-id="97053-165">Content of this file should be hello exact name of hello Domain Verification Token.</span></span> <span data-ttu-id="97053-166">(Hello token můžete zkopírovat z hello okno Stav ověření domény)</span><span class="sxs-lookup"><span data-stu-id="97053-166">(You can copy hello token from hello Domain Verification Status Blade)</span></span>

1. <span data-ttu-id="97053-167">Odeslat tento soubor v kořenovém hello hello webového serveru, který je hostitelem vaší domény`/.well-known/pki-validation/starfield.html`</span><span class="sxs-lookup"><span data-stu-id="97053-167">Upload this file at hello root of hello web server hosting your domain `/.well-known/pki-validation/starfield.html`</span></span>

1. <span data-ttu-id="97053-168">Klikněte na tlačítko **aktualizovat** tooupdate hello certifikát stav po dokončení ověření.</span><span class="sxs-lookup"><span data-stu-id="97053-168">Click **Refresh** tooupdate hello certificate status after verification is completed.</span></span> <span data-ttu-id="97053-169">Může trvat několik minut toocomplete ověření.</span><span class="sxs-lookup"><span data-stu-id="97053-169">It might take few minutes for verification toocomplete.</span></span>

> [!TIP]
> <span data-ttu-id="97053-170">Ověřte v terminálu pomocí `curl -G http://<domain>/.well-known/pki-validation/starfield.html` hello odpověď by měla obsahovat hello `<verification-token>`.</span><span class="sxs-lookup"><span data-stu-id="97053-170">Verify in a terminal using `curl -G http://<domain>/.well-known/pki-validation/starfield.html` hello response should contain hello `<verification-token>`.</span></span>

#### <a name="dns-txt-record-verification"></a><span data-ttu-id="97053-171">Záznam ověření DNS TXT</span><span class="sxs-lookup"><span data-stu-id="97053-171">DNS TXT Record Verification</span></span>

1. <span data-ttu-id="97053-172">Pomocí Správce DNS vytvořit záznam TXT na hello `@` subdomény s hodnota rovna toohello domény ověření tokenu.</span><span class="sxs-lookup"><span data-stu-id="97053-172">Using your DNS manager, Create a TXT record on hello `@` subdomain with value equal toohello Domain Verification Token.</span></span>
1. <span data-ttu-id="97053-173">Klikněte na tlačítko **"Aktualizovat"** tooupdate hello stav certifikátu po dokončení ověření.</span><span class="sxs-lookup"><span data-stu-id="97053-173">Click **“Refresh”** tooupdate hello Certificate status after verification is completed.</span></span>

> [!TIP]
> <span data-ttu-id="97053-174">Musíte toocreate záznam TXT na `@.<domain>` s hodnotou `<verification-token>`.</span><span class="sxs-lookup"><span data-stu-id="97053-174">You need toocreate a TXT record on `@.<domain>` with value `<verification-token>`.</span></span>

### <a name="assign-certificate-tooapp-service-app"></a><span data-ttu-id="97053-175">Přiřadit certifikát tooApp služby App Service</span><span class="sxs-lookup"><span data-stu-id="97053-175">Assign Certificate tooApp Service App</span></span>

<span data-ttu-id="97053-176">Pokud jste vybrali **IP na základě SSL** a vaše vlastní doména je nakonfigurována pomocí záznamu A, musíte provést následující další kroky hello:</span><span class="sxs-lookup"><span data-stu-id="97053-176">If you selected **IP based SSL** and your custom domain is configured using an A record, you must perform hello following additional steps:</span></span>

<span data-ttu-id="97053-177">Po dokončení konfigurace IP adresy na základě vazby SSL, vyhrazenou IP adresu nebo je přiřazen tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="97053-177">After you have configured an IP based SSL binding, a dedicated IP address is assigned tooyour app.</span></span> <span data-ttu-id="97053-178">Tuto IP adresu můžete najít na hello **vlastní domény** stránky v části Nastavení aplikace, vpravo nahoře hello **názvy hostitelů** části.</span><span class="sxs-lookup"><span data-stu-id="97053-178">You can find this IP address on hello **Custom domain** page under settings of your app, right above hello **Hostnames** section.</span></span> <span data-ttu-id="97053-179">Je uveden jako **externí IP adresu**</span><span class="sxs-lookup"><span data-stu-id="97053-179">It is listed as **External IP Address**</span></span>

![Vložit obrázek IP protokolu SSL](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

<span data-ttu-id="97053-181">Všimněte si, že tato IP adresa je jiné než hello virtuální IP adresy použít dříve tooconfigure hello A záznam pro vaši doménu.</span><span class="sxs-lookup"><span data-stu-id="97053-181">Note that this IP address is different than hello virtual IP address used previously tooconfigure hello A record for your domain.</span></span> <span data-ttu-id="97053-182">Pokud jste nakonfigurovali toouse SNI na základě protokolu SSL, nebo nejsou nakonfigurovány toouse SSL, je uvedena žádnou adresu pro tuto položku.</span><span class="sxs-lookup"><span data-stu-id="97053-182">If you are configured toouse SNI based SSL, or are not configured toouse SSL, no address is listed for this entry.</span></span>

<span data-ttu-id="97053-183">Pomocí nástrojů hello vaším registrátorem názvu domény, upravte hello záznam pro vaše vlastní doména název toopoint toohello IP adresu z předchozího kroku hello.</span><span class="sxs-lookup"><span data-stu-id="97053-183">Using hello tools provided by your domain name registrar, modify hello A record for your custom domain name toopoint toohello IP address from hello previous step.</span></span>

## <a name="rekey-and-sync-hello-certificate"></a><span data-ttu-id="97053-184">Změna hodnoty klíče a synchronizovat hello certifikátu</span><span class="sxs-lookup"><span data-stu-id="97053-184">Rekey and Sync hello Certificate</span></span>

<span data-ttu-id="97053-185">Pokud byste někdy potřebovali tooRekey svůj certifikát, vyberte **změna hodnoty klíče a synchronizovat** možnost z **vlastnosti certifikátu** okno.</span><span class="sxs-lookup"><span data-stu-id="97053-185">If you ever need tooRekey your certificate, select **Rekey and Sync** option from **Certificate Properties** Blade.</span></span>

<span data-ttu-id="97053-186">Klikněte na tlačítko **změna hodnoty klíče** tlačítko tooinitiate hello procesu.</span><span class="sxs-lookup"><span data-stu-id="97053-186">Click **Rekey** Button tooinitiate hello process.</span></span> <span data-ttu-id="97053-187">Tento proces může trvat 1 až 10 minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="97053-187">This process can take 1-10 minutes toocomplete.</span></span>

![Vložit obrázek změna hodnoty klíče protokolu SSL](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

<span data-ttu-id="97053-189">Obnovení klíčů vašeho certifikátu vrátí hello certifikát s nový certifikát vydaný certifikační autority hello.</span><span class="sxs-lookup"><span data-stu-id="97053-189">Rekeying your certificate rolls hello certificate with a new certificate issued from hello certificate authority.</span></span>

## <a name="next-steps"></a><span data-ttu-id="97053-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="97053-190">Next Steps</span></span>

* [<span data-ttu-id="97053-191">Přidat síti pro doručování obsahu</span><span class="sxs-lookup"><span data-stu-id="97053-191">Add a Content Delivery Network</span></span>](app-service-web-tutorial-content-delivery-network.md)