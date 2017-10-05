---
title: "Vytvoření vazby stávající vlastní certifikát SSL pro službu Azure Web Apps | Microsoft Docs"
description: "Naučte se vytvořit vazbu vlastní certifikát SSL pro webovou aplikaci, back-end mobilní aplikace nebo aplikace API v Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 5d5bf588-b0bb-4c6d-8840-1b609cfb5750
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 15c31ae5451a31dff2df08047ee43e75edacc127
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="bind-an-existing-custom-ssl-certificate-to-azure-web-apps"></a><span data-ttu-id="f48b4-103">Vytvoření vazby stávající vlastní certifikát SSL pro službu Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="f48b4-103">Bind an existing custom SSL certificate to Azure Web Apps</span></span>

<span data-ttu-id="f48b4-104">Azure Web Apps nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby.</span><span class="sxs-lookup"><span data-stu-id="f48b4-104">Azure Web Apps provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="f48b4-105">V tomto kurzu se dozvíte, jak vytvořit vazbu vlastní certifikát SSL, který zakoupenému od důvěryhodné certifikační autority do [Azure Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f48b4-105">This tutorial shows you how to bind a custom SSL certificate that you purchased from a trusted certificate authority to [Azure Web Apps](app-service-web-overview.md).</span></span> <span data-ttu-id="f48b4-106">Jakmile budete hotovi, budete mít přístup k vaší webové aplikace na koncový bod HTTPS vaše vlastní doména DNS.</span><span class="sxs-lookup"><span data-stu-id="f48b4-106">When you're finished, you'll be able to access your web app at the HTTPS endpoint of your custom DNS domain.</span></span>

![Webové aplikace s vlastní certifikát SSL](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

<span data-ttu-id="f48b4-108">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="f48b4-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f48b4-109">Upgrade cenová úroveň vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="f48b4-109">Upgrade your app's pricing tier</span></span>
> * <span data-ttu-id="f48b4-110">Vytvoření vazby svůj vlastní certifikát SSL do služby App Service</span><span class="sxs-lookup"><span data-stu-id="f48b4-110">Bind your custom SSL certificate to App Service</span></span>
> * <span data-ttu-id="f48b4-111">Vynutit HTTPS pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f48b4-111">Enforce HTTPS for your app</span></span>
> * <span data-ttu-id="f48b4-112">Automatizovat vazbu certifikátu SSL se skripty</span><span class="sxs-lookup"><span data-stu-id="f48b4-112">Automate SSL certificate binding with scripts</span></span>

> [!NOTE]
> <span data-ttu-id="f48b4-113">Pokud potřebujete získat vlastní certifikát SSL, můžete získat na portálu Azure přímo a navázat jej do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f48b4-113">If you need to get a custom SSL certificate, you can get one in the Azure portal directly and bind it to your web app.</span></span> <span data-ttu-id="f48b4-114">Postupujte podle [kurz služby App Service Certificate](web-sites-purchase-ssl-web-site.md).</span><span class="sxs-lookup"><span data-stu-id="f48b4-114">Follow the [App Service Certificates tutorial](web-sites-purchase-ssl-web-site.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f48b4-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f48b4-115">Prerequisites</span></span>

<span data-ttu-id="f48b4-116">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="f48b4-116">To complete this tutorial:</span></span>

- [<span data-ttu-id="f48b4-117">Vytvořit aplikaci služby App Service</span><span class="sxs-lookup"><span data-stu-id="f48b4-117">Create an App Service app</span></span>](/azure/app-service/)
- [<span data-ttu-id="f48b4-118">Mapování názvu DNS na vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f48b4-118">Map a custom DNS name to your web app</span></span>](app-service-web-tutorial-custom-domain.md)
- <span data-ttu-id="f48b4-119">Získat certifikát SSL od důvěryhodné certifikační autority</span><span class="sxs-lookup"><span data-stu-id="f48b4-119">Acquire an SSL certificate from a trusted certificate authority</span></span>

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a><span data-ttu-id="f48b4-120">Požadavky pro svůj certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="f48b4-120">Requirements for your SSL certificate</span></span>

<span data-ttu-id="f48b4-121">Pro použití certifikátu ve službě App Service, certifikát musí splňovat následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="f48b4-121">To use a certificate in App Service, the certificate must meet all the following requirements:</span></span>

* <span data-ttu-id="f48b4-122">Podepsaný důvěryhodnou certifikační autoritou.</span><span class="sxs-lookup"><span data-stu-id="f48b4-122">Signed by a trusted certificate authority</span></span>
* <span data-ttu-id="f48b4-123">Exportovat jako soubor PFX chráněný heslem</span><span class="sxs-lookup"><span data-stu-id="f48b4-123">Exported as a password-protected PFX file</span></span>
* <span data-ttu-id="f48b4-124">Obsahuje privátní klíč minimálně 2048 bitů dlouho</span><span class="sxs-lookup"><span data-stu-id="f48b4-124">Contains private key at least 2048 bits long</span></span>
* <span data-ttu-id="f48b4-125">Obsahuje všechny zprostředkující certifikáty v řetězu certifikátů</span><span class="sxs-lookup"><span data-stu-id="f48b4-125">Contains all intermediate certificates in the certificate chain</span></span>

> [!NOTE]
> <span data-ttu-id="f48b4-126">**Elliptic Curve Cryptography (ECC) certifikáty** můžete pracovat s App Service, ale nejsou uvedené v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="f48b4-126">**Elliptic Curve Cryptography (ECC) certificates** can work with App Service but are not covered by this article.</span></span> <span data-ttu-id="f48b4-127">Spolupracovat s vaší certifikační autority na přesné kroky k vytvoření certifikáty ECC.</span><span class="sxs-lookup"><span data-stu-id="f48b4-127">Work with your certificate authority on the exact steps to create ECC certificates.</span></span>

## <a name="prepare-your-web-app"></a><span data-ttu-id="f48b4-128">Příprava vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f48b4-128">Prepare your web app</span></span>

<span data-ttu-id="f48b4-129">K vytvoření vazby certifikát SSL a vlastní webové aplikace, vaše [plán služby App Service](https://azure.microsoft.com/pricing/details/app-service/) musí být v **základní**, **standardní**, nebo **Premium** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="f48b4-129">To bind a custom SSL certificate to your web app, your [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be in the **Basic**, **Standard**, or **Premium** tier.</span></span> <span data-ttu-id="f48b4-130">V tomto kroku je třeba zkontrolovat, že vaše webová aplikace je v podporovaném cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="f48b4-130">In this step, you make sure that your web app is in the supported pricing tier.</span></span>

### <a name="log-in-to-azure"></a><span data-ttu-id="f48b4-131">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="f48b4-131">Log in to Azure</span></span>

<span data-ttu-id="f48b4-132">Otevřete web [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f48b4-132">Open the [Azure portal](https://portal.azure.com).</span></span>

### <a name="navigate-to-your-web-app"></a><span data-ttu-id="f48b4-133">Přejděte do vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f48b4-133">Navigate to your web app</span></span>

<span data-ttu-id="f48b4-134">V levé nabídce klikněte na tlačítko **App Services**a potom klikněte na název vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f48b4-134">From the left menu, click **App Services**, and then click the name of your web app.</span></span>

![Výběr webové aplikace](./media/app-service-web-tutorial-custom-ssl/select-app.png)

<span data-ttu-id="f48b4-136">Dostali jste na stránce Správa webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f48b4-136">You have landed in the management page of your web app.</span></span>  

### <a name="check-the-pricing-tier"></a><span data-ttu-id="f48b4-137">Zkontrolujte cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="f48b4-137">Check the pricing tier</span></span>

<span data-ttu-id="f48b4-138">V levém navigačním panelu webové stránky aplikace, přejděte na **nastavení** a vyberte **škálování (plán služby App Service)**.</span><span class="sxs-lookup"><span data-stu-id="f48b4-138">In the left-hand navigation of your web app page, scroll to the **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Škálování nabídky](./media/app-service-web-tutorial-custom-ssl/scale-up-menu.png)

<span data-ttu-id="f48b4-140">Zkontrolujte, že není součástí vaší webové aplikace **volné** nebo **sdílené** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="f48b4-140">Check to make sure that your web app is not in the **Free** or **Shared** tier.</span></span> <span data-ttu-id="f48b4-141">Aktuální úroveň vaší webové aplikace se zvýrazní v tmavý blue pole.</span><span class="sxs-lookup"><span data-stu-id="f48b4-141">Your web app's current tier is highlighted by a dark blue box.</span></span>

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-ssl/check-pricing-tier.png)

<span data-ttu-id="f48b4-143">Vlastní SSL není podporována v **volné** nebo **sdílené** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="f48b4-143">Custom SSL is not supported in the **Free** or **Shared** tier.</span></span> <span data-ttu-id="f48b4-144">Pokud potřebujete škálování, postupujte podle kroků v další části.</span><span class="sxs-lookup"><span data-stu-id="f48b4-144">If you need to scale up, follow the steps in the next section.</span></span> <span data-ttu-id="f48b4-145">Jinak, zavřete **zvolte cenovou úroveň** stránky a přejít na [odesílání a vaše certifikát SSL vazbu](#upload).</span><span class="sxs-lookup"><span data-stu-id="f48b4-145">Otherwise, close the **Choose your pricing tier** page and skip to [Upload and bind your SSL certificate](#upload).</span></span>

### <a name="scale-up-your-app-service-plan"></a><span data-ttu-id="f48b4-146">Škálovat plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="f48b4-146">Scale up your App Service plan</span></span>

<span data-ttu-id="f48b4-147">Vyberte jednu z **základní**, **standardní**, nebo **Premium** vrstev.</span><span class="sxs-lookup"><span data-stu-id="f48b4-147">Select one of the **Basic**, **Standard**, or **Premium** tiers.</span></span>

<span data-ttu-id="f48b4-148">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="f48b4-148">Click **Select**.</span></span>

![Vyberte cenovou úroveň](./media/app-service-web-tutorial-custom-ssl/choose-pricing-tier.png)

<span data-ttu-id="f48b4-150">Když se zobrazí následující oznámení, je dokončena operace škálování.</span><span class="sxs-lookup"><span data-stu-id="f48b4-150">When you see the following notification, the scale operation is complete.</span></span>

![Škálování oznámení](./media/app-service-web-tutorial-custom-ssl/scale-notification.png)

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a><span data-ttu-id="f48b4-152">Vytvoření vazby certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="f48b4-152">Bind your SSL certificate</span></span>

<span data-ttu-id="f48b4-153">Jste připraveni k odeslání vašeho certifikátu SSL pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f48b4-153">You are ready to upload your SSL certificate to your web app.</span></span>

### <a name="merge-intermediate-certificates"></a><span data-ttu-id="f48b4-154">Sloučení zprostředkující certifikáty</span><span class="sxs-lookup"><span data-stu-id="f48b4-154">Merge intermediate certificates</span></span>

<span data-ttu-id="f48b4-155">Pokud certifikační autorita poskytuje několik certifikátů v řetězu certifikátů, musíte sloučit certifikáty v pořadí.</span><span class="sxs-lookup"><span data-stu-id="f48b4-155">If your certificate authority gives you multiple certificates in the certificate chain, you need to merge the certificates in order.</span></span> 

<span data-ttu-id="f48b4-156">Chcete-li to provést, otevřete každý certifikát, který jste obdrželi v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="f48b4-156">To do this, open each certificate you received in a text editor.</span></span> 

<span data-ttu-id="f48b4-157">Vytvořte soubor pro sloučené certifikát názvem _mergedcertificate.crt_.</span><span class="sxs-lookup"><span data-stu-id="f48b4-157">Create a file for the merged certificate, called _mergedcertificate.crt_.</span></span> <span data-ttu-id="f48b4-158">V textovém editoru zkopírujte obsah každý certifikát do tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="f48b4-158">In a text editor, copy the content of each certificate into this file.</span></span> <span data-ttu-id="f48b4-159">Pořadí certifikáty by měl vypadat podobně jako následující šablony:</span><span class="sxs-lookup"><span data-stu-id="f48b4-159">The order of your certificates should look like the following template:</span></span>

```
-----BEGIN CERTIFICATE-----
<your Base64 encoded SSL certificate>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded intermediate certificate 1>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded intermediate certificate 2>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded root certificate>
-----END CERTIFICATE-----
```

### <a name="export-certificate-to-pfx"></a><span data-ttu-id="f48b4-160">Export certifikátu PFX</span><span class="sxs-lookup"><span data-stu-id="f48b4-160">Export certificate to PFX</span></span>

<span data-ttu-id="f48b4-161">Exportujte sloučené certifikát SSL s privátním klíčem, který žádost o certifikát byl vytvořen s.</span><span class="sxs-lookup"><span data-stu-id="f48b4-161">Export your merged SSL certificate with the private key that your certificate request was generated with.</span></span>

<span data-ttu-id="f48b4-162">Pokud jste vygenerovali žádosti o certifikát pomocí OpenSSL, jste vytvořili soubor privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="f48b4-162">If you generated your certificate request using OpenSSL, then you have created a private key file.</span></span> <span data-ttu-id="f48b4-163">Chcete-li exportovat certifikát PFX, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="f48b4-163">To export your certificate to PFX, run the following command.</span></span> <span data-ttu-id="f48b4-164">Nahraďte zástupné symboly  _&lt;soubor privátního klíče >_ a  _&lt;sloučit soubor certifikátu >_.</span><span class="sxs-lookup"><span data-stu-id="f48b4-164">Replace the placeholders _&lt;private-key-file>_ and _&lt;merged-certificate-file>_.</span></span>

```
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

<span data-ttu-id="f48b4-165">Po zobrazení výzvy zadejte heslo k exportu.</span><span class="sxs-lookup"><span data-stu-id="f48b4-165">When prompted, define an export password.</span></span> <span data-ttu-id="f48b4-166">Toto heslo budete používat při nahrávání svůj certifikát SSL do služby App Service později.</span><span class="sxs-lookup"><span data-stu-id="f48b4-166">You'll use this password when uploading your SSL certificate to App Service later.</span></span>

<span data-ttu-id="f48b4-167">Pokud jste použili služby IIS nebo _Certreq.exe_ generovat žádosti o certifikát, nainstalujte certifikát do místního počítače a potom [exportu certifikátu do PFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).</span><span class="sxs-lookup"><span data-stu-id="f48b4-167">If you used IIS or _Certreq.exe_ to generate your certificate request, install the certificate to your local machine, and then [export the certificate to PFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).</span></span>

### <a name="upload-your-ssl-certificate"></a><span data-ttu-id="f48b4-168">Nahrajte certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="f48b4-168">Upload your SSL certificate</span></span>

<span data-ttu-id="f48b4-169">Chcete-li nahrát svůj certifikát SSL, klikněte na tlačítko **certifikáty SSL** v levé navigaci vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f48b4-169">To upload your SSL certificate, click **SSL certificates** in the left navigation of your web app.</span></span>

<span data-ttu-id="f48b4-170">Klikněte na tlačítko **nahrát certifikát**.</span><span class="sxs-lookup"><span data-stu-id="f48b4-170">Click **Upload Certificate**.</span></span>

<span data-ttu-id="f48b4-171">V **soubor certifikátu PFX**, vyberte soubor PFX.</span><span class="sxs-lookup"><span data-stu-id="f48b4-171">In **PFX Certificate File**, select your PFX file.</span></span> <span data-ttu-id="f48b4-172">V **heslo certifikátu**, zadejte heslo, které jste vytvořili při exportu souboru PFX.</span><span class="sxs-lookup"><span data-stu-id="f48b4-172">In **Certificate password**, type the password that you created when you exported the PFX file.</span></span>

<span data-ttu-id="f48b4-173">Klikněte na **Odeslat**.</span><span class="sxs-lookup"><span data-stu-id="f48b4-173">Click **Upload**.</span></span>

![Nahrání certifikátu](./media/app-service-web-tutorial-custom-ssl/upload-certificate.png)

<span data-ttu-id="f48b4-175">Po dokončení nahrávání svůj certifikát služby App Service se zobrazí v **certifikáty SSL** stránky.</span><span class="sxs-lookup"><span data-stu-id="f48b4-175">When App Service finishes uploading your certificate, it appears in the **SSL certificates** page.</span></span>

![Nahrát certifikát](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a><span data-ttu-id="f48b4-177">Vytvoření vazby certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="f48b4-177">Bind your SSL certificate</span></span>

<span data-ttu-id="f48b4-178">V **vazby SSL** klikněte na tlačítko **přidat vazbu**.</span><span class="sxs-lookup"><span data-stu-id="f48b4-178">In the **SSL bindings** section, click **Add binding**.</span></span>

<span data-ttu-id="f48b4-179">V **přidat vazbu SSL** pomocí rozevíracích seznamů a vyberte název domény pro zabezpečení a certifikát, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="f48b4-179">In the **Add SSL Binding** page, use the dropdowns to select the domain name to secure, and the certificate to use.</span></span>

> [!NOTE]
> <span data-ttu-id="f48b4-180">Pokud jste nahráli certifikát, ale nejsou zobrazeny názvy domény v **Hostname** rozevíracího seznamu, aktualizujte stránku prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="f48b4-180">If you have uploaded your certificate but don't see the domain name(s) in the **Hostname** dropdown, try refreshing the browser page.</span></span>
>
>

<span data-ttu-id="f48b4-181">V **SSL typ**, vyberte, zda chcete použít  **[indikace názvu serveru (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  nebo SSL založenou na protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="f48b4-181">In **SSL Type**, select whether to use **[Server Name Indication (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** or IP-based SSL.</span></span>

- <span data-ttu-id="f48b4-182">**Založená na SNI** -vazby SSL na základě více SNI, mohou být přidány.</span><span class="sxs-lookup"><span data-stu-id="f48b4-182">**SNI-based SSL** - Multiple SNI-based SSL bindings may be added.</span></span> <span data-ttu-id="f48b4-183">Tato možnost umožňuje víc certifikátů SSL k zabezpečení více domén na stejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f48b4-183">This option allows multiple SSL certificates to secure multiple domains on the same IP address.</span></span> <span data-ttu-id="f48b4-184">Většina moderních prohlížeče (včetně aplikace Internet Explorer, Chrome, Firefox a Opera) podporují SNI (najít komplexnější informací podpory prohlížeče na [indikace názvu serveru](http://wikipedia.org/wiki/Server_Name_Indication)).</span><span class="sxs-lookup"><span data-stu-id="f48b4-184">Most modern browsers (including Internet Explorer, Chrome, Firefox, and Opera) support SNI (find more comprehensive browser support information at [Server Name Indication](http://wikipedia.org/wiki/Server_Name_Indication)).</span></span>
- <span data-ttu-id="f48b4-185">**Založená na IP** -lze přidat pouze jednu vazbu SSL založenou na protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="f48b4-185">**IP-based SSL** - Only one IP-based SSL binding may be added.</span></span> <span data-ttu-id="f48b4-186">Tato možnost umožňuje jenom jeden certifikát SSL k zabezpečení vyhrazené veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="f48b4-186">This option allows only one SSL certificate to secure a dedicated public IP address.</span></span> <span data-ttu-id="f48b4-187">K zabezpečení více domén, je nutné zabezpečit je všechny používající stejný certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="f48b4-187">To secure multiple domains, you must secure them all using the same SSL certificate.</span></span> <span data-ttu-id="f48b4-188">Toto je tradiční možnost pro vazbu SSL.</span><span class="sxs-lookup"><span data-stu-id="f48b4-188">This is the traditional option for SSL binding.</span></span>

<span data-ttu-id="f48b4-189">Klikněte na tlačítko **přidat vazbu**.</span><span class="sxs-lookup"><span data-stu-id="f48b4-189">Click **Add Binding**.</span></span>

![Vytvoření vazby certifikátu SSL](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

<span data-ttu-id="f48b4-191">Po dokončení nahrávání svůj certifikát služby App Service se zobrazí v **vazby SSL** oddíly.</span><span class="sxs-lookup"><span data-stu-id="f48b4-191">When App Service finishes uploading your certificate, it appears in the **SSL bindings** sections.</span></span>

![Certifikát vázaný do webové aplikace](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a><span data-ttu-id="f48b4-193">Přemapování záznam pro protokol SSL IP</span><span class="sxs-lookup"><span data-stu-id="f48b4-193">Remap A record for IP SSL</span></span>

<span data-ttu-id="f48b4-194">Pokud nepoužíváte založená na IP ve vaší webové aplikaci, přejděte k [HTTPS Test pro vaše vlastní doména](#test).</span><span class="sxs-lookup"><span data-stu-id="f48b4-194">If you don't use IP-based SSL in your web app, skip to [Test HTTPS for your custom domain](#test).</span></span>

<span data-ttu-id="f48b4-195">Ve výchozím nastavení používá sdílené veřejnou IP adresu webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f48b4-195">By default, your web app uses a shared public IP address.</span></span> <span data-ttu-id="f48b4-196">Při vytvoření vazby certifikátu SSL založenou na protokolu IP, služby App Service vytvoří nový, vyhrazenou IP adresu pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f48b4-196">When you bind a certificate with IP-based SSL, App Service creates a new, dedicated IP address for your web app.</span></span>

<span data-ttu-id="f48b4-197">Pokud záznam jsou namapovány na vaší webové aplikace, aktualizace registru vaší domény pomocí této nové, vyhrazené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="f48b4-197">If you have mapped an A record to your web app, update your domain registry with this new, dedicated IP address.</span></span>

<span data-ttu-id="f48b4-198">Webové aplikace **vlastní domény** stránka se aktualizuje pomocí nové, vyhrazené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="f48b4-198">Your web app's **Custom domain** page is updated with the new, dedicated IP address.</span></span> <span data-ttu-id="f48b4-199">[Zkopírujte tuto IP adresu](app-service-web-tutorial-custom-domain.md#info), pak [přemapovat záznam A](app-service-web-tutorial-custom-domain.md#map-an-a-record) na tuto novou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f48b4-199">[Copy this IP address](app-service-web-tutorial-custom-domain.md#info), then [remap the A record](app-service-web-tutorial-custom-domain.md#map-an-a-record) to this new IP address.</span></span>

<a name="test"></a>

## <a name="test-https"></a><span data-ttu-id="f48b4-200">Test HTTPS</span><span class="sxs-lookup"><span data-stu-id="f48b4-200">Test HTTPS</span></span>

<span data-ttu-id="f48b4-201">Nyní již zbývá udělat je a ujistěte se, že funguje protokol HTTPS pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="f48b4-201">All that's left to do now is to make sure that HTTPS works for your custom domain.</span></span> <span data-ttu-id="f48b4-202">V různých prohlížečích, přejděte do `https://<your.custom.domain>` zobrazíte vykonává až vaše webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="f48b4-202">In various browsers, browse to `https://<your.custom.domain>` to see that it serves up your web app.</span></span>

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> <span data-ttu-id="f48b4-204">Pokud vaše webová aplikace obsahuje certifikát chyby ověření, pravděpodobně používáte certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="f48b4-204">If your web app gives you certificate validation errors, you're probably using a self-signed certificate.</span></span>
>
> <span data-ttu-id="f48b4-205">Pokud není tento případ, může mít vynecháno zprostředkující certifikáty při exportu certifikátu do souboru PFX.</span><span class="sxs-lookup"><span data-stu-id="f48b4-205">If that's not the case, you may have left out intermediate certificates when you export your certificate to the PFX file.</span></span>

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a><span data-ttu-id="f48b4-206">Vynucení HTTPS</span><span class="sxs-lookup"><span data-stu-id="f48b4-206">Enforce HTTPS</span></span>

<span data-ttu-id="f48b4-207">Služba aplikace nemá *není* vynutit protokolu HTTPS, takže všichni uživatelé stále přístup k vaší webové aplikace pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="f48b4-207">App Service does *not* enforce HTTPS, so anyone can still access your web app using HTTP.</span></span> <span data-ttu-id="f48b4-208">Pokud chcete vynutit HTTPS pro vaši webovou aplikaci, definovat pravidla přepsání v _web.config_ souboru pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f48b4-208">To enforce HTTPS for your web app, define a rewrite rule in the _web.config_ file for your web app.</span></span> <span data-ttu-id="f48b4-209">Služby App Service používá tento soubor, bez ohledu na jazykové rozhraní vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f48b4-209">App Service uses this file, regardless of the language framework of your web app.</span></span>

> [!NOTE]
> <span data-ttu-id="f48b4-210">Neexistuje konkrétní jazyk přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="f48b4-210">There is language-specific redirection of requests.</span></span> <span data-ttu-id="f48b4-211">Můžete použít ASP.NET MVC [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtru místo přepsání pravidla v _web.config_.</span><span class="sxs-lookup"><span data-stu-id="f48b4-211">ASP.NET MVC can use the [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter instead of the rewrite rule in _web.config_.</span></span>

<span data-ttu-id="f48b4-212">Pokud jste vývojář .NET, byste měli být relativně obeznámeni s tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="f48b4-212">If you're a .NET developer, you should be relatively familiar with this file.</span></span> <span data-ttu-id="f48b4-213">Je v kořenu vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="f48b4-213">It is in the root of your solution.</span></span>

<span data-ttu-id="f48b4-214">Případně pokud vyvíjíte pomocí PHP, Node.js, Python nebo Java, existuje pravděpodobnost, tento soubor vaším jménem jsme generované ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="f48b4-214">Alternatively, if you develop with PHP, Node.js, Python, or Java, there is a chance we generated this file on your behalf in App Service.</span></span>

<span data-ttu-id="f48b4-215">Připojení ke koncovému bodu webové aplikace FTP podle pokynů v [nasazení vaší aplikace do Azure App Service pomocí FTP nebo S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="f48b4-215">Connect to your web app's FTP endpoint by following the instructions at [Deploy your app to Azure App Service using FTP/S](app-service-deploy-ftp.md).</span></span>

<span data-ttu-id="f48b4-216">Tento soubor se musí nacházet ve _/home/site/wwwroot_.</span><span class="sxs-lookup"><span data-stu-id="f48b4-216">This file should be located in _/home/site/wwwroot_.</span></span> <span data-ttu-id="f48b4-217">Pokud ne, vytvořte _web.config_ soubor v této složce se souborem XML následující:</span><span class="sxs-lookup"><span data-stu-id="f48b4-217">If not, create a _web.config_ file in this folder with the following XML:</span></span>

```xml   
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <!-- BEGIN rule ELEMENT FOR HTTPS REDIRECT -->
        <rule name="Force HTTPS" enabled="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
            <add input="{HTTPS}" pattern="off" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
        </rule>
        <!-- END rule ELEMENT FOR HTTPS REDIRECT -->
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

<span data-ttu-id="f48b4-218">Pro existující _web.config_ souboru, zkopírujte celou `<rule>` element do vaší _web.config_na `configuration/system.webServer/rewrite/rules` elementu.</span><span class="sxs-lookup"><span data-stu-id="f48b4-218">For an existing _web.config_ file, copy the entire `<rule>` element into your _web.config_'s `configuration/system.webServer/rewrite/rules` element.</span></span> <span data-ttu-id="f48b4-219">Pokud existují další `<rule>` elementů ve vaší _web.config_, umístit zkopírovaný `<rule>` element před dalších `<rule>` elementy.</span><span class="sxs-lookup"><span data-stu-id="f48b4-219">If there are other `<rule>` elements in your _web.config_, place the copied `<rule>` element before the other `<rule>` elements.</span></span>

<span data-ttu-id="f48b4-220">Toto pravidlo vrátí HTTP 301 (trvalé přesměrování) pro protokol HTTPS, vždy, když uživatel provede požadavek HTTP do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f48b4-220">This rule returns an HTTP 301 (permanent redirect) to the HTTPS protocol whenever the user makes an HTTP request to your web app.</span></span> <span data-ttu-id="f48b4-221">Například přesměruje z `http://contoso.com` k `https://contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="f48b4-221">For example, it redirects from `http://contoso.com` to `https://contoso.com`.</span></span>

<span data-ttu-id="f48b4-222">Další informace o modul IIS URL Rewrite najdete v tématu [přepisování adres URL](http://www.iis.net/downloads/microsoft/url-rewrite) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="f48b4-222">For more information on the IIS URL Rewrite module, see the [URL Rewrite](http://www.iis.net/downloads/microsoft/url-rewrite) documentation.</span></span>

## <a name="enforce-https-for-web-apps-on-linux"></a><span data-ttu-id="f48b4-223">Vynutit HTTPS pro webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="f48b4-223">Enforce HTTPS for Web Apps on Linux</span></span>

<span data-ttu-id="f48b4-224">Aplikační služby v systému Linux nemá *není* vynutit protokolu HTTPS, takže všichni uživatelé stále přístup k vaší webové aplikace pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="f48b4-224">App Service on Linux does *not* enforce HTTPS, so anyone can still access your web app using HTTP.</span></span> <span data-ttu-id="f48b4-225">Pokud chcete vynutit HTTPS pro vaši webovou aplikaci, definovat pravidla přepsání v _.htaccess z_ souboru pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f48b4-225">To enforce HTTPS for your web app, define a rewrite rule in the _.htaccess_ file for your web app.</span></span> 

<span data-ttu-id="f48b4-226">Připojení ke koncovému bodu webové aplikace FTP podle pokynů v [nasazení vaší aplikace do Azure App Service pomocí FTP nebo S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="f48b4-226">Connect to your web app's FTP endpoint by following the instructions at [Deploy your app to Azure App Service using FTP/S](app-service-deploy-ftp.md).</span></span>

<span data-ttu-id="f48b4-227">V _/home/site/wwwroot_, vytvořit _.htaccess z_ soubor s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f48b4-227">In _/home/site/wwwroot_, create an _.htaccess_ file with the following code:</span></span>

```
RewriteEngine On
RewriteCond %{HTTP:X-ARR-SSL} ^$
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

<span data-ttu-id="f48b4-228">Toto pravidlo vrátí HTTP 301 (trvalé přesměrování) pro protokol HTTPS, vždy, když uživatel provede požadavek HTTP do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f48b4-228">This rule returns an HTTP 301 (permanent redirect) to the HTTPS protocol whenever the user makes an HTTP request to your web app.</span></span> <span data-ttu-id="f48b4-229">Například přesměruje z `http://contoso.com` k `https://contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="f48b4-229">For example, it redirects from `http://contoso.com` to `https://contoso.com`.</span></span>

## <a name="automate-with-scripts"></a><span data-ttu-id="f48b4-230">Automatizovat pomocí skriptů</span><span class="sxs-lookup"><span data-stu-id="f48b4-230">Automate with scripts</span></span>

<span data-ttu-id="f48b4-231">Je možné automatizovat vazby SSL pro webovou aplikaci pomocí skriptů, pomocí [rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli) nebo [prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f48b4-231">You can automate SSL bindings for your web app with scripts, using the [Azure CLI](/cli/azure/install-azure-cli) or [Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="azure-cli"></a><span data-ttu-id="f48b4-232">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f48b4-232">Azure CLI</span></span>

<span data-ttu-id="f48b4-233">Následující příkaz odešle exportovaný soubor PFX a získá kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="f48b4-233">The following command uploads an exported PFX file and gets the thumbprint.</span></span>

```bash
thumbprint=$(az appservice web config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

<span data-ttu-id="f48b4-234">Následující příkaz přidá vazby SSL na základě SNI, pomocí kryptografického otisku z předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="f48b4-234">The following command adds an SNI-based SSL binding, using the thumbprint from the previous command.</span></span>

```bash
az appservice web config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

### <a name="azure-powershell"></a><span data-ttu-id="f48b4-235">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="f48b4-235">Azure PowerShell</span></span>

<span data-ttu-id="f48b4-236">Následující příkaz odešle exportovaný soubor PFX a přidá vazbu na základě SNI SSL.</span><span class="sxs-lookup"><span data-stu-id="f48b4-236">The following command uploads an exported PFX file and adds an SNI-based SSL binding.</span></span>

```PowerShell
New-AzureRmWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```

## <a name="next-steps"></a><span data-ttu-id="f48b4-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f48b4-237">Next steps</span></span>

<span data-ttu-id="f48b4-238">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="f48b4-238">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f48b4-239">Upgrade cenová úroveň vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="f48b4-239">Upgrade your app's pricing tier</span></span>
> * <span data-ttu-id="f48b4-240">Vytvoření vazby svůj vlastní certifikát SSL do služby App Service</span><span class="sxs-lookup"><span data-stu-id="f48b4-240">Bind your custom SSL certificate to App Service</span></span>
> * <span data-ttu-id="f48b4-241">Vynutit HTTPS pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f48b4-241">Enforce HTTPS for your app</span></span>
> * <span data-ttu-id="f48b4-242">Automatizovat vazbu certifikátu SSL se skripty</span><span class="sxs-lookup"><span data-stu-id="f48b4-242">Automate SSL certificate binding with scripts</span></span>

<span data-ttu-id="f48b4-243">Přechodu na v dalším kurzu se dozvíte, jak používat Azure Content Delivery Network.</span><span class="sxs-lookup"><span data-stu-id="f48b4-243">Advance to the next tutorial to learn how to use Azure Content Delivery Network.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f48b4-244">Přidat síti pro doručování obsahu (CDN) do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f48b4-244">Add a Content Delivery Network (CDN) to an Azure App Service</span></span>](app-service-web-tutorial-content-delivery-network.md)
