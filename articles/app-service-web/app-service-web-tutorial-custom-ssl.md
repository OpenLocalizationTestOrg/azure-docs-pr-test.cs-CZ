---
title: "aaaBind existující vlastní SSL certifikát tooAzure Web Apps | Microsoft Docs"
description: "Přečtěte si tootoobind vlastní SSL certifikát tooyour webové aplikace, back-end mobilní aplikace nebo aplikace API v Azure App Service."
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
ms.openlocfilehash: 3503ba9f96c8ea8d18451e8bf9a9b441797ef44d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="bind-an-existing-custom-ssl-certificate-tooazure-web-apps"></a><span data-ttu-id="41795-103">Vytvořit vazbu existující vlastní SSL certifikát tooAzure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="41795-103">Bind an existing custom SSL certificate tooAzure Web Apps</span></span>

<span data-ttu-id="41795-104">Azure Web Apps nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby.</span><span class="sxs-lookup"><span data-stu-id="41795-104">Azure Web Apps provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="41795-105">Tento kurz ukazuje, jak toobind vlastní SSL certifikát, který jste zakoupenému od důvěryhodné certifikační autority příliš[Azure Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="41795-105">This tutorial shows you how toobind a custom SSL certificate that you purchased from a trusted certificate authority too[Azure Web Apps](app-service-web-overview.md).</span></span> <span data-ttu-id="41795-106">Jakmile budete hotovi, budete moct tooaccess vaší webové aplikace na koncový bod HTTPS hello vlastní domény DNS.</span><span class="sxs-lookup"><span data-stu-id="41795-106">When you're finished, you'll be able tooaccess your web app at hello HTTPS endpoint of your custom DNS domain.</span></span>

![Webové aplikace s vlastní certifikát SSL](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

<span data-ttu-id="41795-108">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="41795-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="41795-109">Upgrade cenová úroveň vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="41795-109">Upgrade your app's pricing tier</span></span>
> * <span data-ttu-id="41795-110">Vytvoření vazby vaše vlastní tooApp certifikát SSL služby</span><span class="sxs-lookup"><span data-stu-id="41795-110">Bind your custom SSL certificate tooApp Service</span></span>
> * <span data-ttu-id="41795-111">Vynutit HTTPS pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="41795-111">Enforce HTTPS for your app</span></span>
> * <span data-ttu-id="41795-112">Automatizovat vazbu certifikátu SSL se skripty</span><span class="sxs-lookup"><span data-stu-id="41795-112">Automate SSL certificate binding with scripts</span></span>

> [!NOTE]
> <span data-ttu-id="41795-113">Pokud potřebujete tooget vlastní certifikát SSL, můžete získat v hello portál Azure přímo a navázat jej tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="41795-113">If you need tooget a custom SSL certificate, you can get one in hello Azure portal directly and bind it tooyour web app.</span></span> <span data-ttu-id="41795-114">Postupujte podle hello [kurz služby App Service Certificate](web-sites-purchase-ssl-web-site.md).</span><span class="sxs-lookup"><span data-stu-id="41795-114">Follow hello [App Service Certificates tutorial](web-sites-purchase-ssl-web-site.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="41795-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="41795-115">Prerequisites</span></span>

<span data-ttu-id="41795-116">toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="41795-116">toocomplete this tutorial:</span></span>

- [<span data-ttu-id="41795-117">Vytvořit aplikaci služby App Service</span><span class="sxs-lookup"><span data-stu-id="41795-117">Create an App Service app</span></span>](/azure/app-service/)
- [<span data-ttu-id="41795-118">Mapa vlastní DNS název tooyour webové aplikace</span><span class="sxs-lookup"><span data-stu-id="41795-118">Map a custom DNS name tooyour web app</span></span>](app-service-web-tutorial-custom-domain.md)
- <span data-ttu-id="41795-119">Získat certifikát SSL od důvěryhodné certifikační autority</span><span class="sxs-lookup"><span data-stu-id="41795-119">Acquire an SSL certificate from a trusted certificate authority</span></span>

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a><span data-ttu-id="41795-120">Požadavky pro svůj certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="41795-120">Requirements for your SSL certificate</span></span>

<span data-ttu-id="41795-121">toouse certifikát ve službě App Service, hello certifikátu musí splňovat všechny hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="41795-121">toouse a certificate in App Service, hello certificate must meet all hello following requirements:</span></span>

* <span data-ttu-id="41795-122">Podepsaný důvěryhodnou certifikační autoritou.</span><span class="sxs-lookup"><span data-stu-id="41795-122">Signed by a trusted certificate authority</span></span>
* <span data-ttu-id="41795-123">Exportovat jako soubor PFX chráněný heslem</span><span class="sxs-lookup"><span data-stu-id="41795-123">Exported as a password-protected PFX file</span></span>
* <span data-ttu-id="41795-124">Obsahuje privátní klíč minimálně 2048 bitů dlouho</span><span class="sxs-lookup"><span data-stu-id="41795-124">Contains private key at least 2048 bits long</span></span>
* <span data-ttu-id="41795-125">Obsahuje všechny zprostředkující certifikáty v řetězu certifikátů hello</span><span class="sxs-lookup"><span data-stu-id="41795-125">Contains all intermediate certificates in hello certificate chain</span></span>

> [!NOTE]
> <span data-ttu-id="41795-126">**Elliptic Curve Cryptography (ECC) certifikáty** můžete pracovat s App Service, ale nejsou uvedené v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="41795-126">**Elliptic Curve Cryptography (ECC) certificates** can work with App Service but are not covered by this article.</span></span> <span data-ttu-id="41795-127">Spolupracovat s vaší certifikační autority na certifikáty ECC toocreate přesný postup hello.</span><span class="sxs-lookup"><span data-stu-id="41795-127">Work with your certificate authority on hello exact steps toocreate ECC certificates.</span></span>

## <a name="prepare-your-web-app"></a><span data-ttu-id="41795-128">Příprava vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="41795-128">Prepare your web app</span></span>

<span data-ttu-id="41795-129">toobind vlastní SSL certifikát tooyour webové aplikace, vaše [plán služby App Service](https://azure.microsoft.com/pricing/details/app-service/) musí být v hello **základní**, **standardní**, nebo **Premium** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="41795-129">toobind a custom SSL certificate tooyour web app, your [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be in hello **Basic**, **Standard**, or **Premium** tier.</span></span> <span data-ttu-id="41795-130">V tomto kroku budete zajistěte, aby že vaší webové aplikace v hello podporován cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="41795-130">In this step, you make sure that your web app is in hello supported pricing tier.</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="41795-131">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="41795-131">Log in tooAzure</span></span>

<span data-ttu-id="41795-132">Otevřete hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="41795-132">Open hello [Azure portal](https://portal.azure.com).</span></span>

### <a name="navigate-tooyour-web-app"></a><span data-ttu-id="41795-133">Přejděte tooyour webové aplikace</span><span class="sxs-lookup"><span data-stu-id="41795-133">Navigate tooyour web app</span></span>

<span data-ttu-id="41795-134">V levé nabídce hello, klikněte na **App Services**a potom klikněte na název hello vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="41795-134">From hello left menu, click **App Services**, and then click hello name of your web app.</span></span>

![Výběr webové aplikace](./media/app-service-web-tutorial-custom-ssl/select-app.png)

<span data-ttu-id="41795-136">Dostali jste na stránce Správa hello vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="41795-136">You have landed in hello management page of your web app.</span></span>  

### <a name="check-hello-pricing-tier"></a><span data-ttu-id="41795-137">Zkontrolujte hello cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="41795-137">Check hello pricing tier</span></span>

<span data-ttu-id="41795-138">V levém navigačním hello webové stránky aplikace, posuňte toohello **nastavení** a vyberte **škálování (plán služby App Service)**.</span><span class="sxs-lookup"><span data-stu-id="41795-138">In hello left-hand navigation of your web app page, scroll toohello **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Škálování nabídky](./media/app-service-web-tutorial-custom-ssl/scale-up-menu.png)

<span data-ttu-id="41795-140">Zkontrolujte toomake se, že vaše webová aplikace není hello **volné** nebo **sdílené** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="41795-140">Check toomake sure that your web app is not in hello **Free** or **Shared** tier.</span></span> <span data-ttu-id="41795-141">Aktuální úroveň vaší webové aplikace se zvýrazní v tmavý blue pole.</span><span class="sxs-lookup"><span data-stu-id="41795-141">Your web app's current tier is highlighted by a dark blue box.</span></span>

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-ssl/check-pricing-tier.png)

<span data-ttu-id="41795-143">Vlastní SSL není podporována v hello **volné** nebo **sdílené** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="41795-143">Custom SSL is not supported in hello **Free** or **Shared** tier.</span></span> <span data-ttu-id="41795-144">Pokud potřebujete tooscale nahoru, postupujte podle kroků hello v další části hello.</span><span class="sxs-lookup"><span data-stu-id="41795-144">If you need tooscale up, follow hello steps in hello next section.</span></span> <span data-ttu-id="41795-145">Jinak, zavřete hello **zvolte cenovou úroveň** stránky a přeskočit příliš[odesílání a vaše certifikát SSL vazbu](#upload).</span><span class="sxs-lookup"><span data-stu-id="41795-145">Otherwise, close hello **Choose your pricing tier** page and skip too[Upload and bind your SSL certificate](#upload).</span></span>

### <a name="scale-up-your-app-service-plan"></a><span data-ttu-id="41795-146">Škálovat plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="41795-146">Scale up your App Service plan</span></span>

<span data-ttu-id="41795-147">Vyberte jednu z hello **základní**, **standardní**, nebo **Premium** vrstev.</span><span class="sxs-lookup"><span data-stu-id="41795-147">Select one of hello **Basic**, **Standard**, or **Premium** tiers.</span></span>

<span data-ttu-id="41795-148">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="41795-148">Click **Select**.</span></span>

![Vyberte cenovou úroveň](./media/app-service-web-tutorial-custom-ssl/choose-pricing-tier.png)

<span data-ttu-id="41795-150">Až uvidíte hello následující oznámení, je dokončena operace škálování hello.</span><span class="sxs-lookup"><span data-stu-id="41795-150">When you see hello following notification, hello scale operation is complete.</span></span>

![Škálování oznámení](./media/app-service-web-tutorial-custom-ssl/scale-notification.png)

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a><span data-ttu-id="41795-152">Vytvoření vazby certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="41795-152">Bind your SSL certificate</span></span>

<span data-ttu-id="41795-153">Můžete jsou připravené tooupload vaší webové aplikace tooyour certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="41795-153">You are ready tooupload your SSL certificate tooyour web app.</span></span>

### <a name="merge-intermediate-certificates"></a><span data-ttu-id="41795-154">Sloučení zprostředkující certifikáty</span><span class="sxs-lookup"><span data-stu-id="41795-154">Merge intermediate certificates</span></span>

<span data-ttu-id="41795-155">Pokud certifikační autorita poskytuje několik certifikátů v řetězu certifikátů hello, je třeba toomerge hello certifikáty v pořadí.</span><span class="sxs-lookup"><span data-stu-id="41795-155">If your certificate authority gives you multiple certificates in hello certificate chain, you need toomerge hello certificates in order.</span></span> 

<span data-ttu-id="41795-156">toodo to otevřete každý certifikát vám zobrazily v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="41795-156">toodo this, open each certificate you received in a text editor.</span></span> 

<span data-ttu-id="41795-157">Vytvořte soubor pro hello sloučené certifikát nazvaný _mergedcertificate.crt_.</span><span class="sxs-lookup"><span data-stu-id="41795-157">Create a file for hello merged certificate, called _mergedcertificate.crt_.</span></span> <span data-ttu-id="41795-158">V textovém editoru zkopírujte obsah hello jednotlivých certifikátů do tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="41795-158">In a text editor, copy hello content of each certificate into this file.</span></span> <span data-ttu-id="41795-159">pořadí Hello certifikáty by měl vypadat podobně jako hello následující šablony:</span><span class="sxs-lookup"><span data-stu-id="41795-159">hello order of your certificates should look like hello following template:</span></span>

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

### <a name="export-certificate-toopfx"></a><span data-ttu-id="41795-160">Export certifikátu tooPFX</span><span class="sxs-lookup"><span data-stu-id="41795-160">Export certificate tooPFX</span></span>

<span data-ttu-id="41795-161">Exportujte sloučené certifikát SSL s hello privátní klíč, který žádost o certifikát se vygeneroval s.</span><span class="sxs-lookup"><span data-stu-id="41795-161">Export your merged SSL certificate with hello private key that your certificate request was generated with.</span></span>

<span data-ttu-id="41795-162">Pokud jste vygenerovali žádosti o certifikát pomocí OpenSSL, jste vytvořili soubor privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="41795-162">If you generated your certificate request using OpenSSL, then you have created a private key file.</span></span> <span data-ttu-id="41795-163">tooexport váš certifikát tooPFX, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="41795-163">tooexport your certificate tooPFX, run hello following command.</span></span> <span data-ttu-id="41795-164">Nahraďte zástupné symboly hello  _&lt;soubor privátního klíče >_ a  _&lt;sloučit soubor certifikátu >_.</span><span class="sxs-lookup"><span data-stu-id="41795-164">Replace hello placeholders _&lt;private-key-file>_ and _&lt;merged-certificate-file>_.</span></span>

```
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

<span data-ttu-id="41795-165">Po zobrazení výzvy zadejte heslo k exportu.</span><span class="sxs-lookup"><span data-stu-id="41795-165">When prompted, define an export password.</span></span> <span data-ttu-id="41795-166">Toto heslo budete používat při nahrávání vaší SSL certifikát tooApp služby později.</span><span class="sxs-lookup"><span data-stu-id="41795-166">You'll use this password when uploading your SSL certificate tooApp Service later.</span></span>

<span data-ttu-id="41795-167">Pokud jste použili služby IIS nebo _Certreq.exe_ toogenerate žádost o certifikát, nainstalujte hello certifikát tooyour místního počítače a potom [exportovat certifikát tooPFX hello](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).</span><span class="sxs-lookup"><span data-stu-id="41795-167">If you used IIS or _Certreq.exe_ toogenerate your certificate request, install hello certificate tooyour local machine, and then [export hello certificate tooPFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).</span></span>

### <a name="upload-your-ssl-certificate"></a><span data-ttu-id="41795-168">Nahrajte certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="41795-168">Upload your SSL certificate</span></span>

<span data-ttu-id="41795-169">tooupload svůj certifikát SSL, klikněte na tlačítko **certifikáty SSL** v hello levé navigační vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="41795-169">tooupload your SSL certificate, click **SSL certificates** in hello left navigation of your web app.</span></span>

<span data-ttu-id="41795-170">Klikněte na tlačítko **nahrát certifikát**.</span><span class="sxs-lookup"><span data-stu-id="41795-170">Click **Upload Certificate**.</span></span>

<span data-ttu-id="41795-171">V **soubor certifikátu PFX**, vyberte soubor PFX.</span><span class="sxs-lookup"><span data-stu-id="41795-171">In **PFX Certificate File**, select your PFX file.</span></span> <span data-ttu-id="41795-172">V **heslo certifikátu**, zadejte heslo hello, kterou jste vytvořili při exportu souboru PFX hello.</span><span class="sxs-lookup"><span data-stu-id="41795-172">In **Certificate password**, type hello password that you created when you exported hello PFX file.</span></span>

<span data-ttu-id="41795-173">Klikněte na **Odeslat**.</span><span class="sxs-lookup"><span data-stu-id="41795-173">Click **Upload**.</span></span>

![Nahrání certifikátu](./media/app-service-web-tutorial-custom-ssl/upload-certificate.png)

<span data-ttu-id="41795-175">Po dokončení nahrávání svůj certifikát služby App Service se zobrazí v hello **certifikáty SSL** stránky.</span><span class="sxs-lookup"><span data-stu-id="41795-175">When App Service finishes uploading your certificate, it appears in hello **SSL certificates** page.</span></span>

![Nahrát certifikát](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a><span data-ttu-id="41795-177">Vytvoření vazby certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="41795-177">Bind your SSL certificate</span></span>

<span data-ttu-id="41795-178">V hello **vazby SSL** klikněte na tlačítko **přidat vazbu**.</span><span class="sxs-lookup"><span data-stu-id="41795-178">In hello **SSL bindings** section, click **Add binding**.</span></span>

<span data-ttu-id="41795-179">V hello **přidat vazbu SSL** stránky, použijte hello rozevíracích seznamů tooselect hello domény název toosecure a toouse certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="41795-179">In hello **Add SSL Binding** page, use hello dropdowns tooselect hello domain name toosecure, and hello certificate toouse.</span></span>

> [!NOTE]
> <span data-ttu-id="41795-180">Pokud jste nahráli certifikát, ale nejsou zobrazeny názvy domény hello v hello **Hostname** rozevírací seznam, zkuste aktualizovat stránku hello prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="41795-180">If you have uploaded your certificate but don't see hello domain name(s) in hello **Hostname** dropdown, try refreshing hello browser page.</span></span>
>
>

<span data-ttu-id="41795-181">V **SSL typ**, vyberte zda toouse  **[indikace názvu serveru (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  nebo SSL založenou na protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="41795-181">In **SSL Type**, select whether toouse **[Server Name Indication (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** or IP-based SSL.</span></span>

- <span data-ttu-id="41795-182">**Založená na SNI** -vazby SSL na základě více SNI, mohou být přidány.</span><span class="sxs-lookup"><span data-stu-id="41795-182">**SNI-based SSL** - Multiple SNI-based SSL bindings may be added.</span></span> <span data-ttu-id="41795-183">Tato možnost umožňuje více toosecure certifikáty SSL více domén na hello stejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="41795-183">This option allows multiple SSL certificates toosecure multiple domains on hello same IP address.</span></span> <span data-ttu-id="41795-184">Většina moderních prohlížeče (včetně aplikace Internet Explorer, Chrome, Firefox a Opera) podporují SNI (najít komplexnější informací podpory prohlížeče na [indikace názvu serveru](http://wikipedia.org/wiki/Server_Name_Indication)).</span><span class="sxs-lookup"><span data-stu-id="41795-184">Most modern browsers (including Internet Explorer, Chrome, Firefox, and Opera) support SNI (find more comprehensive browser support information at [Server Name Indication](http://wikipedia.org/wiki/Server_Name_Indication)).</span></span>
- <span data-ttu-id="41795-185">**Založená na IP** -lze přidat pouze jednu vazbu SSL založenou na protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="41795-185">**IP-based SSL** - Only one IP-based SSL binding may be added.</span></span> <span data-ttu-id="41795-186">Tato možnost umožňuje jenom jednu toosecure certifikát SSL vyhrazený veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="41795-186">This option allows only one SSL certificate toosecure a dedicated public IP address.</span></span> <span data-ttu-id="41795-187">toosecure několik domén, musíte zabezpečit je všechny pomocí hello stejný certifikát protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="41795-187">toosecure multiple domains, you must secure them all using hello same SSL certificate.</span></span> <span data-ttu-id="41795-188">Toto je tradiční možnost hello pro vazbu SSL.</span><span class="sxs-lookup"><span data-stu-id="41795-188">This is hello traditional option for SSL binding.</span></span>

<span data-ttu-id="41795-189">Klikněte na tlačítko **přidat vazbu**.</span><span class="sxs-lookup"><span data-stu-id="41795-189">Click **Add Binding**.</span></span>

![Vytvoření vazby certifikátu SSL](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

<span data-ttu-id="41795-191">Po dokončení nahrávání svůj certifikát služby App Service se zobrazí v hello **vazby SSL** oddíly.</span><span class="sxs-lookup"><span data-stu-id="41795-191">When App Service finishes uploading your certificate, it appears in hello **SSL bindings** sections.</span></span>

![Certifikát vázaný tooweb aplikace](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a><span data-ttu-id="41795-193">Přemapování záznam pro protokol SSL IP</span><span class="sxs-lookup"><span data-stu-id="41795-193">Remap A record for IP SSL</span></span>

<span data-ttu-id="41795-194">Pokud nepoužíváte založená na IP ve vaší webové aplikaci, přeskočte příliš[HTTPS Test pro vaše vlastní doména](#test).</span><span class="sxs-lookup"><span data-stu-id="41795-194">If you don't use IP-based SSL in your web app, skip too[Test HTTPS for your custom domain](#test).</span></span>

<span data-ttu-id="41795-195">Ve výchozím nastavení používá sdílené veřejnou IP adresu webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="41795-195">By default, your web app uses a shared public IP address.</span></span> <span data-ttu-id="41795-196">Při vytvoření vazby certifikátu SSL založenou na protokolu IP, služby App Service vytvoří nový, vyhrazenou IP adresu pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="41795-196">When you bind a certificate with IP-based SSL, App Service creates a new, dedicated IP address for your web app.</span></span>

<span data-ttu-id="41795-197">Pokud jsou namapovány webové aplikace A záznamů tooyour, aktualizujte registr domény pomocí této nové, vyhrazené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="41795-197">If you have mapped an A record tooyour web app, update your domain registry with this new, dedicated IP address.</span></span>

<span data-ttu-id="41795-198">Webové aplikace **vlastní domény** stránka je aktualizována hello nové, vyhrazenou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="41795-198">Your web app's **Custom domain** page is updated with hello new, dedicated IP address.</span></span> <span data-ttu-id="41795-199">[Zkopírujte tuto IP adresu](app-service-web-tutorial-custom-domain.md#info), pak [přemapování hello záznam](app-service-web-tutorial-custom-domain.md#map-an-a-record) toothis novou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="41795-199">[Copy this IP address](app-service-web-tutorial-custom-domain.md#info), then [remap hello A record](app-service-web-tutorial-custom-domain.md#map-an-a-record) toothis new IP address.</span></span>

<a name="test"></a>

## <a name="test-https"></a><span data-ttu-id="41795-200">Test HTTPS</span><span class="sxs-lookup"><span data-stu-id="41795-200">Test HTTPS</span></span>

<span data-ttu-id="41795-201">Všechno, co teď opustil toodo je toomake se, že pracuje HTTPS pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="41795-201">All that's left toodo now is toomake sure that HTTPS works for your custom domain.</span></span> <span data-ttu-id="41795-202">V různých prohlížečích, procházet příliš`https://<your.custom.domain>` toosee, který ji obsluhuje až vaše webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="41795-202">In various browsers, browse too`https://<your.custom.domain>` toosee that it serves up your web app.</span></span>

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> <span data-ttu-id="41795-204">Pokud vaše webová aplikace obsahuje certifikát chyby ověření, pravděpodobně používáte certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="41795-204">If your web app gives you certificate validation errors, you're probably using a self-signed certificate.</span></span>
>
> <span data-ttu-id="41795-205">Pokud to není hello případ, může mít vynecháno zprostředkující certifikáty při exportu souboru PFX toohello vašeho certifikátu.</span><span class="sxs-lookup"><span data-stu-id="41795-205">If that's not hello case, you may have left out intermediate certificates when you export your certificate toohello PFX file.</span></span>

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a><span data-ttu-id="41795-206">Vynucení HTTPS</span><span class="sxs-lookup"><span data-stu-id="41795-206">Enforce HTTPS</span></span>

<span data-ttu-id="41795-207">Služba aplikace nemá *není* vynutit protokolu HTTPS, takže všichni uživatelé stále přístup k vaší webové aplikace pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="41795-207">App Service does *not* enforce HTTPS, so anyone can still access your web app using HTTP.</span></span> <span data-ttu-id="41795-208">definování tooenforce HTTPS pro webovou aplikaci, přepište pravidlo v hello _web.config_ souboru pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="41795-208">tooenforce HTTPS for your web app, define a rewrite rule in hello _web.config_ file for your web app.</span></span> <span data-ttu-id="41795-209">Služby App Service používá tento soubor, bez ohledu na jazykové rozhraní hello vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="41795-209">App Service uses this file, regardless of hello language framework of your web app.</span></span>

> [!NOTE]
> <span data-ttu-id="41795-210">Neexistuje konkrétní jazyk přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="41795-210">There is language-specific redirection of requests.</span></span> <span data-ttu-id="41795-211">ASP.NET MVC můžete použít hello [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtru místo hello přepište pravidlo v _web.config_.</span><span class="sxs-lookup"><span data-stu-id="41795-211">ASP.NET MVC can use hello [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter instead of hello rewrite rule in _web.config_.</span></span>

<span data-ttu-id="41795-212">Pokud jste vývojář .NET, byste měli být relativně obeznámeni s tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="41795-212">If you're a .NET developer, you should be relatively familiar with this file.</span></span> <span data-ttu-id="41795-213">Je v kořenovém hello vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="41795-213">It is in hello root of your solution.</span></span>

<span data-ttu-id="41795-214">Případně pokud vyvíjíte pomocí PHP, Node.js, Python nebo Java, existuje pravděpodobnost, tento soubor vaším jménem jsme generované ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="41795-214">Alternatively, if you develop with PHP, Node.js, Python, or Java, there is a chance we generated this file on your behalf in App Service.</span></span>

<span data-ttu-id="41795-215">Koncový bod FTP tooyour webovou aplikaci připojit podle následujících pokynů hello [nasazení vaší aplikace tooAzure služby App Service pomocí FTP nebo S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="41795-215">Connect tooyour web app's FTP endpoint by following hello instructions at [Deploy your app tooAzure App Service using FTP/S](app-service-deploy-ftp.md).</span></span>

<span data-ttu-id="41795-216">Tento soubor se musí nacházet ve _/home/site/wwwroot_.</span><span class="sxs-lookup"><span data-stu-id="41795-216">This file should be located in _/home/site/wwwroot_.</span></span> <span data-ttu-id="41795-217">Pokud ne, vytvořte _web.config_ soubor v této složce s hello následující XML:</span><span class="sxs-lookup"><span data-stu-id="41795-217">If not, create a _web.config_ file in this folder with hello following XML:</span></span>

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

<span data-ttu-id="41795-218">Pro existující _web.config_ souboru, zkopírujte celou hello `<rule>` element do vaší _web.config_na `configuration/system.webServer/rewrite/rules` elementu.</span><span class="sxs-lookup"><span data-stu-id="41795-218">For an existing _web.config_ file, copy hello entire `<rule>` element into your _web.config_'s `configuration/system.webServer/rewrite/rules` element.</span></span> <span data-ttu-id="41795-219">Pokud existují další `<rule>` elementů ve vaší _web.config_, místní hello zkopírovat `<rule>` element před hello jiných `<rule>` elementy.</span><span class="sxs-lookup"><span data-stu-id="41795-219">If there are other `<rule>` elements in your _web.config_, place hello copied `<rule>` element before hello other `<rule>` elements.</span></span>

<span data-ttu-id="41795-220">Toto pravidlo vrátí protokol HTTP 301 (trvalé přesměrování) toohello HTTPS, vždy, když uživatel hello provede webové aplikace HTTP požadavku tooyour.</span><span class="sxs-lookup"><span data-stu-id="41795-220">This rule returns an HTTP 301 (permanent redirect) toohello HTTPS protocol whenever hello user makes an HTTP request tooyour web app.</span></span> <span data-ttu-id="41795-221">Například přesměruje z `http://contoso.com` příliš`https://contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="41795-221">For example, it redirects from `http://contoso.com` too`https://contoso.com`.</span></span>

<span data-ttu-id="41795-222">Další informace o modul IIS URL Rewrite hello najdete v tématu hello [přepisování adres URL](http://www.iis.net/downloads/microsoft/url-rewrite) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="41795-222">For more information on hello IIS URL Rewrite module, see hello [URL Rewrite](http://www.iis.net/downloads/microsoft/url-rewrite) documentation.</span></span>

## <a name="enforce-https-for-web-apps-on-linux"></a><span data-ttu-id="41795-223">Vynutit HTTPS pro webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="41795-223">Enforce HTTPS for Web Apps on Linux</span></span>

<span data-ttu-id="41795-224">Aplikační služby v systému Linux nemá *není* vynutit protokolu HTTPS, takže všichni uživatelé stále přístup k vaší webové aplikace pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="41795-224">App Service on Linux does *not* enforce HTTPS, so anyone can still access your web app using HTTP.</span></span> <span data-ttu-id="41795-225">definování tooenforce HTTPS pro webovou aplikaci, přepište pravidlo v hello _.htaccess z_ souboru pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="41795-225">tooenforce HTTPS for your web app, define a rewrite rule in hello _.htaccess_ file for your web app.</span></span> 

<span data-ttu-id="41795-226">Koncový bod FTP tooyour webovou aplikaci připojit podle následujících pokynů hello [nasazení vaší aplikace tooAzure služby App Service pomocí FTP nebo S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="41795-226">Connect tooyour web app's FTP endpoint by following hello instructions at [Deploy your app tooAzure App Service using FTP/S](app-service-deploy-ftp.md).</span></span>

<span data-ttu-id="41795-227">V _/home/site/wwwroot_, vytvořit _.htaccess z_ soubor s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="41795-227">In _/home/site/wwwroot_, create an _.htaccess_ file with hello following code:</span></span>

```
RewriteEngine On
RewriteCond %{HTTP:X-ARR-SSL} ^$
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

<span data-ttu-id="41795-228">Toto pravidlo vrátí protokol HTTP 301 (trvalé přesměrování) toohello HTTPS, vždy, když uživatel hello provede webové aplikace HTTP požadavku tooyour.</span><span class="sxs-lookup"><span data-stu-id="41795-228">This rule returns an HTTP 301 (permanent redirect) toohello HTTPS protocol whenever hello user makes an HTTP request tooyour web app.</span></span> <span data-ttu-id="41795-229">Například přesměruje z `http://contoso.com` příliš`https://contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="41795-229">For example, it redirects from `http://contoso.com` too`https://contoso.com`.</span></span>

## <a name="automate-with-scripts"></a><span data-ttu-id="41795-230">Automatizovat pomocí skriptů</span><span class="sxs-lookup"><span data-stu-id="41795-230">Automate with scripts</span></span>

<span data-ttu-id="41795-231">Je možné automatizovat vazby SSL pro webovou aplikaci pomocí skriptů, pomocí hello [rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli) nebo [prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="41795-231">You can automate SSL bindings for your web app with scripts, using hello [Azure CLI](/cli/azure/install-azure-cli) or [Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="azure-cli"></a><span data-ttu-id="41795-232">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="41795-232">Azure CLI</span></span>

<span data-ttu-id="41795-233">Následující příkaz Hello odešle exportovaný soubor PFX a získá hello kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="41795-233">hello following command uploads an exported PFX file and gets hello thumbprint.</span></span>

```bash
thumbprint=$(az appservice web config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

<span data-ttu-id="41795-234">Hello následující příkaz přidá vazby SSL na základě SNI, pomocí kryptografického otisku hello z předchozí příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="41795-234">hello following command adds an SNI-based SSL binding, using hello thumbprint from hello previous command.</span></span>

```bash
az appservice web config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

### <a name="azure-powershell"></a><span data-ttu-id="41795-235">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="41795-235">Azure PowerShell</span></span>

<span data-ttu-id="41795-236">Hello následující příkaz odešle exportovaný soubor PFX a přidá vazbu na základě SNI SSL.</span><span class="sxs-lookup"><span data-stu-id="41795-236">hello following command uploads an exported PFX file and adds an SNI-based SSL binding.</span></span>

```PowerShell
New-AzureRmWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```

## <a name="next-steps"></a><span data-ttu-id="41795-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="41795-237">Next steps</span></span>

<span data-ttu-id="41795-238">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="41795-238">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="41795-239">Upgrade cenová úroveň vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="41795-239">Upgrade your app's pricing tier</span></span>
> * <span data-ttu-id="41795-240">Vytvoření vazby vaše vlastní tooApp certifikát SSL služby</span><span class="sxs-lookup"><span data-stu-id="41795-240">Bind your custom SSL certificate tooApp Service</span></span>
> * <span data-ttu-id="41795-241">Vynutit HTTPS pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="41795-241">Enforce HTTPS for your app</span></span>
> * <span data-ttu-id="41795-242">Automatizovat vazbu certifikátu SSL se skripty</span><span class="sxs-lookup"><span data-stu-id="41795-242">Automate SSL certificate binding with scripts</span></span>

<span data-ttu-id="41795-243">Jak zálohy další kurz toolearn toohello toouse Azure Content Delivery Network.</span><span class="sxs-lookup"><span data-stu-id="41795-243">Advance toohello next tutorial toolearn how toouse Azure Content Delivery Network.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="41795-244">Přidat tooan Content Delivery Network (CDN) Azure App Service</span><span class="sxs-lookup"><span data-stu-id="41795-244">Add a Content Delivery Network (CDN) tooan Azure App Service</span></span>](app-service-web-tutorial-content-delivery-network.md)
