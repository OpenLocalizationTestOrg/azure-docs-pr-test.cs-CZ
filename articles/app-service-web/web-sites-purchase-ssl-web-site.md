---
title: "Přidání certifikátu protokolu SSL do aplikace Azure App Service | Microsoft Docs"
description: "Zjistěte, jak přidat certifikát SSL do vaší aplikace služby App Service."
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
ms.openlocfilehash: 191dd7240ad15b4936a72bc27a2d0162350f3afb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a><span data-ttu-id="d1a12-103">Koupě a konfigurace certifikátu SSL pro službu Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d1a12-103">Buy and Configure an SSL Certificate for your Azure App Service</span></span>

<span data-ttu-id="d1a12-104">V tomto kurzu bude Zabezpečte svoji webovou aplikaci pomocí nákupu certifikát protokolu SSL pro vaše  **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, bezpečně uložením do [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis)a přiřadí se s vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="d1a12-104">In this tutorial, you will secure your web app by purchasing an SSL certificate for your **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, securely storing it in [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis), and associating it with a custom domain.</span></span>

## <a name="step-1---log-in-to-azure"></a><span data-ttu-id="d1a12-105">Krok 1 – přihlášení do Azure</span><span class="sxs-lookup"><span data-stu-id="d1a12-105">Step 1 - Log in to Azure</span></span>

<span data-ttu-id="d1a12-106">Přihlaste se k portálu Azure na http://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="d1a12-106">Log in to the Azure portal at http://portal.azure.com</span></span>

## <a name="step-2---place-an-ssl-certificate-order"></a><span data-ttu-id="d1a12-107">Krok 2 – umístit pořadí certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="d1a12-107">Step 2 - Place an SSL Certificate order</span></span>

<span data-ttu-id="d1a12-108">Certifikát SSL pořadí můžete umístit tak, že vytvoříte novou [certifikát služby aplikace](https://portal.azure.com/#create/Microsoft.SSL) v **portál Azure**.</span><span class="sxs-lookup"><span data-stu-id="d1a12-108">You can place an SSL Certificate order by creating a new [App Service Certificate](https://portal.azure.com/#create/Microsoft.SSL) In the **Azure portal**.</span></span>

![Vytvoření certifikátu](./media/app-service-web-purchase-ssl-web-site/createssl.png)

<span data-ttu-id="d1a12-110">Zadejte popisný **název** certifikátů pro zabezpečení SSL a zadejte **název domény**</span><span class="sxs-lookup"><span data-stu-id="d1a12-110">Enter a friendly **Name** for your SSL certificate and enter the **Domain Name**</span></span>

> [!NOTE]
> <span data-ttu-id="d1a12-111">To je jedním z nejdůležitějších části procesu nákupu.</span><span class="sxs-lookup"><span data-stu-id="d1a12-111">This is one of the most critical parts of the purchase process.</span></span> <span data-ttu-id="d1a12-112">Ujistěte se, že zadejte název hostitele správný (vlastní domény), který chcete chránit pomocí tohoto certifikátu.</span><span class="sxs-lookup"><span data-stu-id="d1a12-112">Make sure to enter correct host name (custom domain) that you want to protect with this certificate.</span></span> <span data-ttu-id="d1a12-113">**NECHCETE** připojit název hostitele s WWW.</span><span class="sxs-lookup"><span data-stu-id="d1a12-113">**DO NOT** append the Host name with WWW.</span></span> 
>

<span data-ttu-id="d1a12-114">Vyberte vaše **předplatné**, **skupiny prostředků**, a **certifikátu SKU**</span><span class="sxs-lookup"><span data-stu-id="d1a12-114">Select your **Subscription**, **Resource Group**, and **Certificate SKU**</span></span>

> [!WARNING]
> <span data-ttu-id="d1a12-115">Certifikáty App Service můžete použít pouze v případě ostatních služeb aplikace v rámci stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="d1a12-115">App Service Certificates can only be used by other App Services within the same subscription.</span></span>  
>

## <a name="step-3---store-the-certificate-in-azure-key-vault"></a><span data-ttu-id="d1a12-116">Krok 3 – uložení certifikátu v Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d1a12-116">Step 3 - Store the certificate in Azure Key Vault</span></span>

> [!NOTE]
> <span data-ttu-id="d1a12-117">[Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) je služba Azure, která pomáhá chránit kryptografické klíče a tajné klíče používané cloudovými aplikacemi a službami.</span><span class="sxs-lookup"><span data-stu-id="d1a12-117">[Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) is an Azure service that helps safeguard cryptographic keys and secrets used by cloud applications and services.</span></span>
>

<span data-ttu-id="d1a12-118">Po dokončení nákupu certifikát SSL, je nutné otevřít [služby App Service Certificate](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) okna prostředků.</span><span class="sxs-lookup"><span data-stu-id="d1a12-118">Once the SSL Certificate purchase is complete, you need to open [App Service Certificates](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) Resource blade.</span></span>

![Vložit obrázek připravena k uložení v KV](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

<span data-ttu-id="d1a12-120">Si všimnete, že je stav certifikátu **"Čekající vystavení"** jsou několik další kroky je potřeba provést před zahájením s použitím tohoto certifikátu.</span><span class="sxs-lookup"><span data-stu-id="d1a12-120">You will notice that Certificate status is **“Pending Issuance”** as there are few more steps you need to complete before you can start using this certificate.</span></span>

<span data-ttu-id="d1a12-121">Klikněte na tlačítko **konfigurace certifikátu** v okně Vlastnosti certifikátu a klikněte na **krok 1: uložení** pro uložení tohoto certifikátu v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d1a12-121">Click **Certificate Configuration** inside Certificate Properties blade and Click on **Step 1: Store** to store this certificate in Azure Key Vault.</span></span>

<span data-ttu-id="d1a12-122">Z **klíč trezoru stav** okně klikněte na tlačítko **klíč trezoru úložiště** zvolit existující trezor klíč pro uložení tohoto certifikátu **nebo vytvořit nový klíč trezoru** k vytvoření nové Key Vault ve stejném předplatném nebo skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="d1a12-122">From **Key Vault Status** Blade, click **Key Vault Repository** to choose an existing Key Vault to store this certificate **OR Create New Key Vault** to create new Key Vault inside same subscription and resource group.</span></span>

> [!NOTE]
> <span data-ttu-id="d1a12-123">Azure Key Vault má minimální poplatky za uložení tohoto certifikátu.</span><span class="sxs-lookup"><span data-stu-id="d1a12-123">Azure Key Vault has minimal charges for storing this certificate.</span></span>
> <span data-ttu-id="d1a12-124">Další informace najdete v tématu  **[klíč trezoru podrobnostmi o cenách Azure](https://azure.microsoft.com/pricing/details/key-vault/)**.</span><span class="sxs-lookup"><span data-stu-id="d1a12-124">For more information, see **[Azure Key Vault Pricing Details](https://azure.microsoft.com/pricing/details/key-vault/)**.</span></span>
>

<span data-ttu-id="d1a12-125">Jakmile vyberete klíč trezoru úložiště pro uložení tohoto certifikátu, **ukládání** možnost by se měla zobrazit úspěch.</span><span class="sxs-lookup"><span data-stu-id="d1a12-125">Once you have selected the Key Vault Repository to store this certificate in, the **Store** option should show success.</span></span>

![Vložit obrázek úspěchu úložiště v KV](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-the-domain-ownership"></a><span data-ttu-id="d1a12-127">Krok 4 - ověřit vlastnictví domény</span><span class="sxs-lookup"><span data-stu-id="d1a12-127">Step 4 - Verify the Domain Ownership</span></span>

> [!NOTE]
> <span data-ttu-id="d1a12-128">Existují tři typy ověření domény nepodporuje App service Certificate: ověření domény, e-mailu, ručně.</span><span class="sxs-lookup"><span data-stu-id="d1a12-128">There are three types of domain verification supported by App service Certificates: Domain, Mail, Manual Verification.</span></span> <span data-ttu-id="d1a12-129">Tyto mechanismy jsou popsány v další podrobnosti najdete [Advanced části](#advanced).</span><span class="sxs-lookup"><span data-stu-id="d1a12-129">These are explained in more details in the [Advanced section](#advanced).</span></span>

<span data-ttu-id="d1a12-130">Ze stejné **konfigurace certifikátu** okno, které jste použili v kroku 3, klikněte na tlačítko **krok 2: ověření**.</span><span class="sxs-lookup"><span data-stu-id="d1a12-130">From the same **Certificate Configuration** blade you used in Step 3, click **Step 2: Verify**.</span></span>

<span data-ttu-id="d1a12-131">**Ověření domény** jedná se o nejpohodlnější proces **pouze v případě** máte  **[zakoupili vaši vlastní doménu služby Azure App Service.](custom-dns-web-site-buydomains-web-app.md)**</span><span class="sxs-lookup"><span data-stu-id="d1a12-131">**Domain Verification** This is the most convenient process **ONLY IF** you have **[purchased your custom domain from Azure App Service.](custom-dns-web-site-buydomains-web-app.md)**</span></span>
<span data-ttu-id="d1a12-132">Klikněte na **ověřte** tlačítko k dokončení tohoto kroku.</span><span class="sxs-lookup"><span data-stu-id="d1a12-132">Click on **Verify** button to complete this step.</span></span>

![Vložit obrázek ověření domény](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

<span data-ttu-id="d1a12-134">Po kliknutí na **ověřte**, použijte **aktualizovat** tlačítko až **ověřte** možnost by se měla zobrazit úspěch.</span><span class="sxs-lookup"><span data-stu-id="d1a12-134">After clicking **Verify**, use the **Refresh** button until the **Verify** option should show success.</span></span>

![Vložit obrázek ověřit úspěšné provedení v KV](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-to-app-service-app"></a><span data-ttu-id="d1a12-136">Krok 5 – přiřadit certifikát k aplikaci služby App Service</span><span class="sxs-lookup"><span data-stu-id="d1a12-136">Step 5 - Assign Certificate to App Service App</span></span>

> [!NOTE]
> <span data-ttu-id="d1a12-137">Před provedením kroků v této části, musí mít přidružené vlastního názvu domény s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="d1a12-137">Before performing the steps in this section, you must have associated a custom domain name with your app.</span></span> <span data-ttu-id="d1a12-138">Další informace najdete v tématu  **[konfigurace vlastního názvu domény pro webovou aplikaci.](app-service-web-tutorial-custom-domain.md)**</span><span class="sxs-lookup"><span data-stu-id="d1a12-138">For more information, see **[Configuring a custom domain name for a web app.](app-service-web-tutorial-custom-domain.md)**</span></span>
>

<span data-ttu-id="d1a12-139">V  **[portál Azure](https://portal.azure.com/)**, klikněte **služby App Service** možnost na levé straně stránky.</span><span class="sxs-lookup"><span data-stu-id="d1a12-139">In the **[Azure portal](https://portal.azure.com/)**, click the **App Service** option on the left of the page.</span></span>

<span data-ttu-id="d1a12-140">Klikněte na název aplikace, ke které chcete přiřadit certifikát.</span><span class="sxs-lookup"><span data-stu-id="d1a12-140">Click the name of your app to which you want to assign this certificate.</span></span>

<span data-ttu-id="d1a12-141">V **nastavení**, klikněte na tlačítko **certifikáty SSL**.</span><span class="sxs-lookup"><span data-stu-id="d1a12-141">In the **Settings**, click **SSL certificates**.</span></span>

<span data-ttu-id="d1a12-142">Klikněte na tlačítko **importovat aplikaci služby certifikát** a vyberte certifikát, který jste zakoupili.</span><span class="sxs-lookup"><span data-stu-id="d1a12-142">Click **Import App Service Certificate** and select the certificate that you just purchased.</span></span>

![Vložit obrázek importovat certifikát](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

<span data-ttu-id="d1a12-144">V **vazby ssl** část kliknutím na **přidat vazby**a pomocí rozevíracích seznamů vyberte název domény pro zabezpečené pomocí protokolu SSL a certifikát používat.</span><span class="sxs-lookup"><span data-stu-id="d1a12-144">In the **ssl bindings** section Click on **Add bindings**, and use the dropdowns to select the domain name to secure with SSL, and the certificate to use.</span></span> <span data-ttu-id="d1a12-145">Může také vybrat, jestli se má používat  **[indikace názvu serveru (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  nebo IP na základě protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="d1a12-145">You may also select whether to use **[Server Name Indication (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** or IP based SSL.</span></span>

![Vložit obrázek vazby SSL](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

<span data-ttu-id="d1a12-147">Klikněte na tlačítko **přidat vazbu** uložte změny a povolit protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="d1a12-147">Click **Add Binding** to save the changes and enable SSL.</span></span>

> [!NOTE]
> <span data-ttu-id="d1a12-148">Pokud jste vybrali **IP na základě SSL** a vaše vlastní doména je nakonfigurována pomocí záznamu A, je třeba provést následující kroky.</span><span class="sxs-lookup"><span data-stu-id="d1a12-148">If you selected **IP based SSL** and your custom domain is configured using an A record, you must perform the following additional steps.</span></span> <span data-ttu-id="d1a12-149">Tyto mechanismy jsou popsány v další podrobnosti najdete [Advanced části](#Advanced).</span><span class="sxs-lookup"><span data-stu-id="d1a12-149">These are explained in more details in the [Advanced section](#Advanced).</span></span>

<span data-ttu-id="d1a12-150">V tomto okamžiku nyní byste měli mít k navštívení vaší aplikace pomocí `HTTPS://` místo `HTTP://` k ověření, že je certifikát správně nakonfigurováno.</span><span class="sxs-lookup"><span data-stu-id="d1a12-150">At this point, you should be able to visit your app using `HTTPS://` instead of `HTTP://` to verify that the certificate has been configured correctly.</span></span>

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a><span data-ttu-id="d1a12-151">Krok 6: úlohy správy</span><span class="sxs-lookup"><span data-stu-id="d1a12-151">Step 6 - Management tasks</span></span>

### <a name="azure-cli"></a><span data-ttu-id="d1a12-152">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d1a12-152">Azure CLI</span></span>

<span data-ttu-id="d1a12-153">[!code-azurecli[hlavní](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "vlastní certifikát SSL vazbu do webové aplikace")]</span><span class="sxs-lookup"><span data-stu-id="d1a12-153">[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]</span></span> 

### <a name="powershell"></a><span data-ttu-id="d1a12-154">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d1a12-154">PowerShell</span></span>

<span data-ttu-id="d1a12-155">[!code-powershell[hlavní](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "vlastní certifikát SSL vazbu do webové aplikace")]</span><span class="sxs-lookup"><span data-stu-id="d1a12-155">[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]</span></span>

## <a name="advanced"></a><span data-ttu-id="d1a12-156">Advanced</span><span class="sxs-lookup"><span data-stu-id="d1a12-156">Advanced</span></span>

### <a name="verifying-domain-ownership"></a><span data-ttu-id="d1a12-157">Ověření vlastnictví domény</span><span class="sxs-lookup"><span data-stu-id="d1a12-157">Verifying Domain Ownership</span></span>

<span data-ttu-id="d1a12-158">Existují dva další typy ověření domény nepodporuje App service Certificate: e-mailu a ruční ověření.</span><span class="sxs-lookup"><span data-stu-id="d1a12-158">There are two more types of domain verification supported by App service Certificates: Mail, and Manual Verification.</span></span>

#### <a name="mail-verification"></a><span data-ttu-id="d1a12-159">Ověření e-mailu</span><span class="sxs-lookup"><span data-stu-id="d1a12-159">Mail Verification</span></span>

<span data-ttu-id="d1a12-160">Již byl odeslán ověřovací e-mail do e-mailové adresy, přidružené k této vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="d1a12-160">Verification email has already been sent to the Email Address(es) associated with this custom domain.</span></span>
<span data-ttu-id="d1a12-161">K dokončení kroku ověření e-mailu, otevřete e-mailu a klikněte na odkaz ověření.</span><span class="sxs-lookup"><span data-stu-id="d1a12-161">To complete the Email verification step, open the email and click the verification link.</span></span>

![Vložit obrázek ověření e-mailu](./media/app-service-web-purchase-ssl-web-site/KVVerifyEmailSuccess.png)

<span data-ttu-id="d1a12-163">Pokud potřebujete znovu odeslat ověřovací e-mail, klikněte **znovu odeslat e-mailu** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d1a12-163">If you need to resend the verification email, click the **Resend Email** button.</span></span>

#### <a name="manual-verification"></a><span data-ttu-id="d1a12-164">Ruční ověření</span><span class="sxs-lookup"><span data-stu-id="d1a12-164">Manual Verification</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d1a12-165">Ověření webové stránky HTML (funguje pouze u standardní SKU certifikát)</span><span class="sxs-lookup"><span data-stu-id="d1a12-165">HTML Web Page Verification (only works with Standard Certificate SKU)</span></span>
>

1. <span data-ttu-id="d1a12-166">Vytvořte soubor HTML s názvem **"starfield.html"**</span><span class="sxs-lookup"><span data-stu-id="d1a12-166">Create an HTML file named **"starfield.html"**</span></span>

1. <span data-ttu-id="d1a12-167">Obsah tohoto souboru musí být přesný název tokenu ověření domény.</span><span class="sxs-lookup"><span data-stu-id="d1a12-167">Content of this file should be the exact name of the Domain Verification Token.</span></span> <span data-ttu-id="d1a12-168">(Můžete zkopírovat tokenu z okna Stav ověření domény)</span><span class="sxs-lookup"><span data-stu-id="d1a12-168">(You can copy the token from the Domain Verification Status Blade)</span></span>

1. <span data-ttu-id="d1a12-169">Odeslat tento soubor v kořenovém adresáři webového serveru, který je hostitelem vaší domény`/.well-known/pki-validation/starfield.html`</span><span class="sxs-lookup"><span data-stu-id="d1a12-169">Upload this file at the root of the web server hosting your domain `/.well-known/pki-validation/starfield.html`</span></span>

1. <span data-ttu-id="d1a12-170">Klikněte na tlačítko **aktualizovat** aktualizovat stav certifikátu po dokončení ověření.</span><span class="sxs-lookup"><span data-stu-id="d1a12-170">Click **Refresh** to update the certificate status after verification is completed.</span></span> <span data-ttu-id="d1a12-171">Může trvat několik minut na dokončení ověření.</span><span class="sxs-lookup"><span data-stu-id="d1a12-171">It might take few minutes for verification to complete.</span></span>

> [!TIP]
> <span data-ttu-id="d1a12-172">Ověřte v terminálu pomocí `curl -G http://<domain>/.well-known/pki-validation/starfield.html` by měl obsahovat odpověď `<verification-token>`.</span><span class="sxs-lookup"><span data-stu-id="d1a12-172">Verify in a terminal using `curl -G http://<domain>/.well-known/pki-validation/starfield.html` the response should contain the `<verification-token>`.</span></span>

#### <a name="dns-txt-record-verification"></a><span data-ttu-id="d1a12-173">Záznam ověření DNS TXT</span><span class="sxs-lookup"><span data-stu-id="d1a12-173">DNS TXT Record Verification</span></span>

1. <span data-ttu-id="d1a12-174">Pomocí Správce DNS vytvořit záznam TXT na `@` subdomény se hodnota rovná tokenu ověření domény.</span><span class="sxs-lookup"><span data-stu-id="d1a12-174">Using your DNS manager, Create a TXT record on the `@` subdomain with value equal to the Domain Verification Token.</span></span>
1. <span data-ttu-id="d1a12-175">Klikněte na tlačítko **"Aktualizovat"** aktualizovat stav certifikátu po dokončení ověření.</span><span class="sxs-lookup"><span data-stu-id="d1a12-175">Click **“Refresh”** to update the Certificate status after verification is completed.</span></span>

> [!TIP]
> <span data-ttu-id="d1a12-176">Musíte vytvořit záznam TXT na `@.<domain>` s hodnotou `<verification-token>`.</span><span class="sxs-lookup"><span data-stu-id="d1a12-176">You need to create a TXT record on `@.<domain>` with value `<verification-token>`.</span></span>

### <a name="assign-certificate-to-app-service-app"></a><span data-ttu-id="d1a12-177">Přiřadit certifikát k aplikaci služby App Service</span><span class="sxs-lookup"><span data-stu-id="d1a12-177">Assign Certificate to App Service App</span></span>

<span data-ttu-id="d1a12-178">Pokud jste vybrali **IP na základě SSL** a vaše vlastní doména je nakonfigurována pomocí záznamu A, je třeba provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d1a12-178">If you selected **IP based SSL** and your custom domain is configured using an A record, you must perform the following additional steps:</span></span>

<span data-ttu-id="d1a12-179">Po dokončení konfigurace IP adresy na základě vazby SSL, vyhrazenou IP adresu je přiřazené vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d1a12-179">After you have configured an IP based SSL binding, a dedicated IP address is assigned to your app.</span></span> <span data-ttu-id="d1a12-180">Tuto IP adresu můžete najít na **vlastní domény** stránky v části Nastavení aplikace, vpravo nahoře **názvy hostitelů** části.</span><span class="sxs-lookup"><span data-stu-id="d1a12-180">You can find this IP address on the **Custom domain** page under settings of your app, right above the **Hostnames** section.</span></span> <span data-ttu-id="d1a12-181">Je uveden jako **externí IP adresu**</span><span class="sxs-lookup"><span data-stu-id="d1a12-181">It is listed as **External IP Address**</span></span>

![Vložit obrázek IP protokolu SSL](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

<span data-ttu-id="d1a12-183">Upozorňujeme, že tato IP adresa se liší od virtuální IP adresy, které byly dříve slouží ke konfiguraci záznamu A pro doménu.</span><span class="sxs-lookup"><span data-stu-id="d1a12-183">Note that this IP address is different than the virtual IP address used previously to configure the A record for your domain.</span></span> <span data-ttu-id="d1a12-184">Pokud jsou nakonfigurovány pro použití SNI na základě protokolu SSL, nebo nejsou nakonfigurovaná pro používání protokolu SSL, je uvedena žádnou adresu pro tuto položku.</span><span class="sxs-lookup"><span data-stu-id="d1a12-184">If you are configured to use SNI based SSL, or are not configured to use SSL, no address is listed for this entry.</span></span>

<span data-ttu-id="d1a12-185">Pomocí nástrojů vaším registrátorem názvu domény, upravte záznamu A pro váš vlastní název domény tak, aby odkazoval na IP adresu z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="d1a12-185">Using the tools provided by your domain name registrar, modify the A record for your custom domain name to point to the IP address from the previous step.</span></span>

## <a name="rekey-and-sync-the-certificate"></a><span data-ttu-id="d1a12-186">Změna hodnoty klíče a synchronizovat certifikátu</span><span class="sxs-lookup"><span data-stu-id="d1a12-186">Rekey and Sync the Certificate</span></span>

<span data-ttu-id="d1a12-187">Pokud byste někdy potřebovali změna hodnoty klíče vašeho certifikátu, vyberte **změna hodnoty klíče a synchronizovat** možnost z **vlastnosti certifikátu** okno.</span><span class="sxs-lookup"><span data-stu-id="d1a12-187">If you ever need to Rekey your certificate, select **Rekey and Sync** option from **Certificate Properties** Blade.</span></span>

<span data-ttu-id="d1a12-188">Klikněte na tlačítko **změna hodnoty klíče** tlačítko na zahájení procesu.</span><span class="sxs-lookup"><span data-stu-id="d1a12-188">Click **Rekey** Button to initiate the process.</span></span> <span data-ttu-id="d1a12-189">Tento proces může trvat 1 až 10 minut na dokončení.</span><span class="sxs-lookup"><span data-stu-id="d1a12-189">This process can take 1-10 minutes to complete.</span></span>

![Vložit obrázek změna hodnoty klíče protokolu SSL](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

<span data-ttu-id="d1a12-191">Obnovení klíčů vašeho certifikátu vrátí certifikát s nový certifikát vydaný certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="d1a12-191">Rekeying your certificate rolls the certificate with a new certificate issued from the certificate authority.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d1a12-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d1a12-192">Next Steps</span></span>

* [<span data-ttu-id="d1a12-193">Přidat síti pro doručování obsahu</span><span class="sxs-lookup"><span data-stu-id="d1a12-193">Add a Content Delivery Network</span></span>](app-service-web-tutorial-content-delivery-network.md)