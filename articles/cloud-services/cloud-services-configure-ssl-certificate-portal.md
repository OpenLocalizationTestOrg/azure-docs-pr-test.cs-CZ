---
title: "aaaConfigure SSL pro cloudové služby | Microsoft Docs"
description: "Zjistěte, jak toospecify koncový bod HTTPS pro webovou roli a jak tooupload protokolem SSL certifikát toosecure vaší aplikace. Tyto příklady použití hello portálu Azure."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 371ba204-48b6-41af-ab9f-ed1d64efe704
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: adegeo
ms.openlocfilehash: b19283bb7b0e95374f2ae9c3532eb1effc7d6a9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a><span data-ttu-id="6d332-104">Konfigurace protokolu SSL pro všechny aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="6d332-104">Configuring SSL for an application in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6d332-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6d332-105">Azure portal</span></span>](cloud-services-configure-ssl-certificate-portal.md)
> * [<span data-ttu-id="6d332-106">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="6d332-106">Azure classic portal</span></span>](cloud-services-configure-ssl-certificate.md)
>

<span data-ttu-id="6d332-107">Šifrování Secure Socket Layer (SSL) je metoda hello nejčastěji používaná zabezpečení dat posílaných v hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="6d332-107">Secure Socket Layer (SSL) encryption is hello most commonly used method of securing data sent across hello internet.</span></span> <span data-ttu-id="6d332-108">Tato běžných úkolů popisuje jak toospecify koncový bod HTTPS pro webovou roli a jak tooupload protokolem SSL certifikát toosecure vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="6d332-108">This common task discusses how toospecify an HTTPS endpoint for a web role and how tooupload an SSL certificate toosecure your application.</span></span>

> [!NOTE]
> <span data-ttu-id="6d332-109">Hello postupy v této úloze platí tooAzure cloudové služby; Aplikační služby, najdete v části [to](../app-service-web/web-sites-configure-ssl-certificate.md).</span><span class="sxs-lookup"><span data-stu-id="6d332-109">hello procedures in this task apply tooAzure Cloud Services; for App Services, see [this](../app-service-web/web-sites-configure-ssl-certificate.md).</span></span>
>

<span data-ttu-id="6d332-110">Tato úloha používá produkčním nasazení.</span><span class="sxs-lookup"><span data-stu-id="6d332-110">This task uses a production deployment.</span></span> <span data-ttu-id="6d332-111">Informace o používání pracovní nasazení se poskytuje na konci hello v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="6d332-111">Information on using a staging deployment is provided at hello end of this topic.</span></span>

<span data-ttu-id="6d332-112">Čtení [to](cloud-services-how-to-create-deploy-portal.md) první, pokud jste ještě nevytvořili cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6d332-112">Read [this](cloud-services-how-to-create-deploy-portal.md) first if you have not yet created a cloud service.</span></span>

[!INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a><span data-ttu-id="6d332-113">Krok 1: Získání certifikátu protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="6d332-113">Step 1: Get an SSL certificate</span></span>
<span data-ttu-id="6d332-114">tooconfigure SSL pro aplikaci, musíte nejdřív tooget certifikát SSL, která byla podepsána pomocí certifikační autoritou (CA), důvěryhodná třetí straně, která vystavuje certifikáty pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="6d332-114">tooconfigure SSL for an application, you first need tooget an SSL certificate that has been signed by a Certificate Authority (CA), a trusted third party who issues certificates for this purpose.</span></span> <span data-ttu-id="6d332-115">Pokud není již nemáte, je třeba tooobtain jeden ze společnosti, která prodává certifikáty SSL.</span><span class="sxs-lookup"><span data-stu-id="6d332-115">If you do not already have one, you need tooobtain one from a company that sells SSL certificates.</span></span>

<span data-ttu-id="6d332-116">certifikát Hello musí splňovat následující požadavky na certifikáty SSL v Azure hello:</span><span class="sxs-lookup"><span data-stu-id="6d332-116">hello certificate must meet hello following requirements for SSL certificates in Azure:</span></span>

* <span data-ttu-id="6d332-117">Hello certifikát musí obsahovat privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="6d332-117">hello certificate must contain a private key.</span></span>
* <span data-ttu-id="6d332-118">Hello certifikátu musí být vytvořeny pro výměnu klíčů, exportovatelný tooa soubor Personal Information Exchange (.pfx).</span><span class="sxs-lookup"><span data-stu-id="6d332-118">hello certificate must be created for key exchange, exportable tooa Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="6d332-119">Hello názvu subjektu certifikátu musí odpovídat hello domény použít tooaccess hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6d332-119">hello certificate's subject name must match hello domain used tooaccess hello cloud service.</span></span> <span data-ttu-id="6d332-120">Nelze získat certifikát SSL od certifikační autority (CA) pro doménu cloudapp.net hello.</span><span class="sxs-lookup"><span data-stu-id="6d332-120">You cannot obtain an SSL certificate from a certificate authority (CA) for hello cloudapp.net domain.</span></span> <span data-ttu-id="6d332-121">Musíte získat toouse název vlastní domény při přístupu ke službě.</span><span class="sxs-lookup"><span data-stu-id="6d332-121">You must acquire a custom domain name toouse when access your service.</span></span> <span data-ttu-id="6d332-122">Pokud budete požadovat certifikát od certifikační Autority, musí se název subjektu certifikátu hello odpovídat tooaccess název používaný hello vlastní domény aplikace.</span><span class="sxs-lookup"><span data-stu-id="6d332-122">When you request a certificate from a CA, hello certificate's subject name must match hello custom domain name used tooaccess your application.</span></span> <span data-ttu-id="6d332-123">Například, pokud je váš vlastní název domény **contoso.com** by vyžádání certifikátu z certifikační Autority pro ***. contoso.com** nebo **www.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="6d332-123">For example, if your custom domain name is **contoso.com** you would request a certificate from your CA for ***.contoso.com** or **www.contoso.com**.</span></span>
* <span data-ttu-id="6d332-124">Hello certifikát musí používat minimálně 2048bitové šifrování.</span><span class="sxs-lookup"><span data-stu-id="6d332-124">hello certificate must use a minimum of 2048-bit encryption.</span></span>

<span data-ttu-id="6d332-125">Pro účely testování můžete [vytvořit](cloud-services-certs-create.md) a použít certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="6d332-125">For test purposes, you can [create](cloud-services-certs-create.md) and use a self-signed certificate.</span></span> <span data-ttu-id="6d332-126">Certifikát podepsaný svým držitelem není ověřen pomocí certifikační Autority a hello cloudapp.net domény můžete použít jako adresu URL webu hello.</span><span class="sxs-lookup"><span data-stu-id="6d332-126">A self-signed certificate is not authenticated through a CA and can use hello cloudapp.net domain as hello website URL.</span></span> <span data-ttu-id="6d332-127">Například hello následující úkol používá certifikát podepsaný svým držitelem, ve které hello je běžný název (CN) použitý v certifikátu hello **sslexample.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="6d332-127">For example, hello following task uses a self-signed certificate in which hello common name (CN) used in hello certificate is **sslexample.cloudapp.net**.</span></span>

<span data-ttu-id="6d332-128">V dalším kroku musí zahrnovat informace o certifikátu hello v definici služby a služby konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="6d332-128">Next, you must include information about hello certificate in your service definition and service configuration files.</span></span>

<span data-ttu-id="6d332-129"><a name="modify"> </a></span><span class="sxs-lookup"><span data-stu-id="6d332-129"><a name="modify"> </a></span></span>

## <a name="step-2-modify-hello-service-definition-and-configuration-files"></a><span data-ttu-id="6d332-130">Krok 2: Upravte soubory pro definice a konfigurace služby hello</span><span class="sxs-lookup"><span data-stu-id="6d332-130">Step 2: Modify hello service definition and configuration files</span></span>
<span data-ttu-id="6d332-131">Aplikace musí být nakonfigurované toouse hello certifikátu a koncový bod HTTPS musí být přidán.</span><span class="sxs-lookup"><span data-stu-id="6d332-131">Your application must be configured toouse hello certificate, and an HTTPS endpoint must be added.</span></span> <span data-ttu-id="6d332-132">V důsledku toho hello definice služby a soubory konfigurace služby musí toobe aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="6d332-132">As a result, hello service definition and service configuration files need toobe updated.</span></span>

1. <span data-ttu-id="6d332-133">Ve vašem vývojovém prostředí, otevřete soubor definice služby hello (CSDEF), přidejte **certifikáty** části v rámci hello **WebRole** části a zahrnovat hello následující informace certifikát (a zprostředkující certifikáty):</span><span class="sxs-lookup"><span data-stu-id="6d332-133">In your development environment, open hello service definition file (CSDEF), add a **Certificates** section within hello **WebRole** section, and include hello following information about the certificate (and intermediate certificates):</span></span>

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Certificates>
            <Certificate name="SampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="My"
                        permissionLevel="limitedOrElevated" />
            <!-- IMPORTANT! Unless your certificate is either
            self-signed or signed directly by hello CA root, you
            must include all hello intermediate certificates
            here. You must list them here, even if they are
            not bound tooany endpoints. Failing toolist any of
            hello intermediate certificates may cause hard-to-reproduce
            interoperability problems on some clients.-->
            <Certificate name="CAForSampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="CA"
                        permissionLevel="limitedOrElevated" />
        </Certificates>
    ...
    </WebRole>
    ```

   <span data-ttu-id="6d332-134">Hello **certifikáty** oddíl definuje název hello naše certifikát, její umístění a název hello hello úložiště, kde se nachází.</span><span class="sxs-lookup"><span data-stu-id="6d332-134">hello **Certificates** section defines hello name of our certificate, its location, and hello name of hello store where it is located.</span></span>

   <span data-ttu-id="6d332-135">Oprávnění (`permisionLevel` atribut) může být sada tooone Dobrý den následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="6d332-135">Permissions (`permisionLevel` attribute) can be set tooone of hello following values:</span></span>

   | <span data-ttu-id="6d332-136">Hodnota oprávnění</span><span class="sxs-lookup"><span data-stu-id="6d332-136">Permission Value</span></span> | <span data-ttu-id="6d332-137">Popis</span><span class="sxs-lookup"><span data-stu-id="6d332-137">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="6d332-138">limitedOrElevated</span><span class="sxs-lookup"><span data-stu-id="6d332-138">limitedOrElevated</span></span> |<span data-ttu-id="6d332-139">**(Výchozí)**  Všechny procesy role můžete přístup hello privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="6d332-139">**(Default)** All role processes can access hello private key.</span></span> |
   | <span data-ttu-id="6d332-140">zvýšené</span><span class="sxs-lookup"><span data-stu-id="6d332-140">elevated</span></span> |<span data-ttu-id="6d332-141">K privátní klíč hello přístup jenom procesy se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="6d332-141">Only elevated processes can access hello private key.</span></span> |

2. <span data-ttu-id="6d332-142">V souboru definice služby, přidejte **InputEndpoint** v rámci hello **koncové body** části tooenable HTTPS:</span><span class="sxs-lookup"><span data-stu-id="6d332-142">In your service definition file, add an **InputEndpoint** element within hello **Endpoints** section tooenable HTTPS:</span></span>

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Endpoints>
            <InputEndpoint name="HttpsIn" protocol="https" port="443"
                certificate="SampleCertificate" />
        </Endpoints>
    ...
    </WebRole>
    ```

3. <span data-ttu-id="6d332-143">V souboru definice služby, přidejte **vazby** v rámci hello **lokality** části.</span><span class="sxs-lookup"><span data-stu-id="6d332-143">In your service definition file, add a **Binding** element within hello **Sites** section.</span></span> <span data-ttu-id="6d332-144">Tento element přidá toomap vazba HTTPS webu tooyour koncový bod:</span><span class="sxs-lookup"><span data-stu-id="6d332-144">This element adds an HTTPS binding toomap the endpoint tooyour site:</span></span>

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="HttpsIn" endpointName="HttpsIn" />
                </Bindings>
            </Site>
        </Sites>
    ...
    </WebRole>
    ```

   <span data-ttu-id="6d332-145">Byly dokončeny všechny hello požadované změny toohello souboru definice služby; ale stále potřebujete informace o certifikátu hello tooadd hello služby konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="6d332-145">All hello required changes toohello service definition file have been completed; but, you still need tooadd hello certificate information to hello service configuration file.</span></span>
4. <span data-ttu-id="6d332-146">V konfiguračním souboru služby (CSCFG), ServiceConfiguration.Cloud.cscfg, přidejte **certifikáty** hodnotu s u vašeho certifikátu.</span><span class="sxs-lookup"><span data-stu-id="6d332-146">In your service configuration file (CSCFG), ServiceConfiguration.Cloud.cscfg, add a **Certificates** value with that of your certificate.</span></span> <span data-ttu-id="6d332-147">Hello následující ukázka kódu obsahuje podrobnosti o hello **certifikáty** části, s výjimkou hodnota kryptografického otisku hello.</span><span class="sxs-lookup"><span data-stu-id="6d332-147">hello following code sample provides details of hello **Certificates** section, except for hello thumbprint value.</span></span>

   ```xml
    <Role name="Deployment">
    ...
        <Certificates>
            <Certificate name="SampleCertificate"
                thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff"
                thumbprintAlgorithm="sha1" />
            <Certificate name="CAForSampleCertificate"
                thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc"
                thumbprintAlgorithm="sha1" />
        </Certificates>
    ...
    </Role>
    ```

<span data-ttu-id="6d332-148">(Tento příklad používá **sha1** pro hello kryptografický algoritmus.</span><span class="sxs-lookup"><span data-stu-id="6d332-148">(This example uses **sha1** for hello thumbprint algorithm.</span></span> <span data-ttu-id="6d332-149">Zadejte hodnotu hello vhodnou pro váš certifikát kryptografický algoritmus.)</span><span class="sxs-lookup"><span data-stu-id="6d332-149">Specify hello appropriate value for your certificate's thumbprint algorithm.)</span></span>

<span data-ttu-id="6d332-150">Teď, když hello služby definice a služby konfigurační soubory byly aktualizovány, balíček nasazení pro nahrávání tooAzure.</span><span class="sxs-lookup"><span data-stu-id="6d332-150">Now that hello service definition and service configuration files have been updated, package your deployment for uploading tooAzure.</span></span> <span data-ttu-id="6d332-151">Pokud používáte **cspack**, nepoužívejte **/generateConfigurationFile** příznak, jako který přepíše vloženého informace o certifikátu.</span><span class="sxs-lookup"><span data-stu-id="6d332-151">If you are using **cspack**, don't use the **/generateConfigurationFile** flag, as that will overwrite the certificate information you just inserted.</span></span>

## <a name="step-3-upload-a-certificate"></a><span data-ttu-id="6d332-152">Krok 3: Nahrát na server certifikát</span><span class="sxs-lookup"><span data-stu-id="6d332-152">Step 3: Upload a certificate</span></span>
<span data-ttu-id="6d332-153">Připojit toohello portál Azure a...</span><span class="sxs-lookup"><span data-stu-id="6d332-153">Connect toohello Azure portal and...</span></span>

1. <span data-ttu-id="6d332-154">V hello **všechny prostředky** části hello portál, vyberte cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6d332-154">In hello **All resources** section of hello Portal, select your cloud service.</span></span>

    ![Publikování cloudové služby](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. <span data-ttu-id="6d332-156">Klikněte na tlačítko **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="6d332-156">Click **Certificates**.</span></span>

    ![Klikněte na ikonu certifikáty hello](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. <span data-ttu-id="6d332-158">Klikněte na tlačítko **nahrát** hello horní části oblasti certifikáty hello.</span><span class="sxs-lookup"><span data-stu-id="6d332-158">Click **Upload** at hello top of hello certificates area.</span></span>

    ![Klikněte na položku nabídky nahrávání hello](media/cloud-services-configure-ssl-certificate-portal/Upload_menu.png)

4. <span data-ttu-id="6d332-160">Zadejte hello **soubor**, **heslo**, pak klikněte na tlačítko **nahrát** dole hello hello dat vstupní oblast.</span><span class="sxs-lookup"><span data-stu-id="6d332-160">Provide hello **File**, **Password**, then click **Upload** at hello bottom of hello data entry area.</span></span>

## <a name="step-4-connect-toohello-role-instance-by-using-https"></a><span data-ttu-id="6d332-161">Krok 4: Připojte instanci role toohello pomocí protokolu HTTPS</span><span class="sxs-lookup"><span data-stu-id="6d332-161">Step 4: Connect toohello role instance by using HTTPS</span></span>
<span data-ttu-id="6d332-162">Teď, když vaše nasazení je spuštěná v Azure, můžete připojit tooit pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6d332-162">Now that your deployment is up and running in Azure, you can connect tooit using HTTPS.</span></span>

1. <span data-ttu-id="6d332-163">Klikněte na tlačítko hello **adresa URL webu** tooopen až hello webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6d332-163">Click hello **Site URL** tooopen up hello web browser.</span></span>

   ![Klikněte na tlačítko hello adresa URL webu](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2. <span data-ttu-id="6d332-165">Ve webovém prohlížeči, upravte hello odkaz toouse **https** místo **http**a potom navštivte stránku hello.</span><span class="sxs-lookup"><span data-stu-id="6d332-165">In your web browser, modify hello link toouse **https** instead of **http**, and then visit hello page.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6d332-166">Pokud používáte certifikát podepsaný svým držitelem, když procházíte tooan koncový bod HTTPS, který je spojen s certifikát podepsaný svým držitelem hello může se zobrazit chyba certifikátu v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="6d332-166">If you are using a self-signed certificate, when you browse tooan HTTPS endpoint that's associated with hello self-signed certificate you may see a certificate error in hello browser.</span></span> <span data-ttu-id="6d332-167">Používá certifikát podepsaný důvěryhodnou certifikační autoritou eliminuje tento problém; v hello té doby chybu můžete ignorovat hello.</span><span class="sxs-lookup"><span data-stu-id="6d332-167">Using a certificate signed by a trusted certification authority eliminates this problem; in hello meantime, you can ignore hello error.</span></span> <span data-ttu-id="6d332-168">(Další možností je tooadd hello certifikát podepsaný svým držitelem toohello důvěryhodné certifikační autority úložišti certifikátů uživatele.)</span><span class="sxs-lookup"><span data-stu-id="6d332-168">(Another option is tooadd hello self-signed certificate toohello user's trusted certificate authority certificate store.)</span></span>
   >
   >

   ![Verze preview webu](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

   > [!TIP]
   > <span data-ttu-id="6d332-170">Pokud chcete toouse SSL pro nasazení pracovní místo produkční nasazení, je nutné nejdříve toodetermine hello adresa URL použitá pro hello pracovní nasazení.</span><span class="sxs-lookup"><span data-stu-id="6d332-170">If you want toouse SSL for a staging deployment instead of a production deployment, you'll first need toodetermine hello URL used for hello staging deployment.</span></span> <span data-ttu-id="6d332-171">Jakmile nasadila cloudové služby toohello URL hello pracovní prostředí je určen podle hello **ID nasazení** identifikátor GUID v tomto formátu:`https://deployment-id.cloudapp.net/`</span><span class="sxs-lookup"><span data-stu-id="6d332-171">Once your cloud service has been deployed, hello URL toohello staging environment is determined by hello **Deployment ID** GUID in this format: `https://deployment-id.cloudapp.net/`</span></span>  
   >
   > <span data-ttu-id="6d332-172">Vytvoření certifikátu s hello běžný název (CN) rovna toohello na základě GUID adresou URL (například **328187776e774ceda8fc57609d404462.cloudapp.net**).</span><span class="sxs-lookup"><span data-stu-id="6d332-172">Create a certificate with hello common name (CN) equal toohello GUID-based URL (for example, **328187776e774ceda8fc57609d404462.cloudapp.net**).</span></span> <span data-ttu-id="6d332-173">Použití hello portálu tooadd hello certifikát tooyour připravený cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6d332-173">Use hello portal tooadd hello certificate tooyour staged cloud service.</span></span> <span data-ttu-id="6d332-174">Pak přidejte hello informace tooyour CSDEF a CSCFG soubory certifikátů, znovu zabalte aplikaci a aktualizovat vaše dvoufázové instalace toouse hello nový balíček pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="6d332-174">Then, add hello certificate information tooyour CSDEF and CSCFG files, repackage your application, and update your staged deployment toouse hello new package.</span></span>
   >

## <a name="next-steps"></a><span data-ttu-id="6d332-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6d332-175">Next steps</span></span>
* <span data-ttu-id="6d332-176">[Obecná konfigurace cloudové služby](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6d332-176">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="6d332-177">Zjistěte, jak příliš[nasazení cloudové služby](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6d332-177">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="6d332-178">Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6d332-178">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="6d332-179">[Správa služby cloud](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6d332-179">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
