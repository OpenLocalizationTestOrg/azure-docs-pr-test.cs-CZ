---
title: "aaaConfigure SSL pro cloudové služby (klasické) | Microsoft Docs"
description: "Zjistěte, jak toospecify koncový bod HTTPS pro webovou roli a jak tooupload protokolem SSL certifikát toosecure vaší aplikace."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 4cbb7f38-7994-454d-b4f0-7259b558c766
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: adegeo
ms.openlocfilehash: a1ca031b98af49d371977a208ed24f6dc8ea2ac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a><span data-ttu-id="353c0-103">Konfigurace protokolu SSL pro všechny aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="353c0-103">Configuring SSL for an application in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="353c0-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="353c0-104">Azure portal</span></span>](cloud-services-configure-ssl-certificate-portal.md)
> * [<span data-ttu-id="353c0-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="353c0-105">Azure classic portal</span></span>](cloud-services-configure-ssl-certificate.md)
> 
> 

<span data-ttu-id="353c0-106">Šifrování Secure Socket Layer (SSL) je metoda hello nejčastěji používaná zabezpečení dat posílaných v hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="353c0-106">Secure Socket Layer (SSL) encryption is hello most commonly used method of securing data sent across hello internet.</span></span> <span data-ttu-id="353c0-107">Tato běžných úkolů popisuje jak toospecify koncový bod HTTPS pro webovou roli a jak tooupload protokolem SSL certifikát toosecure vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="353c0-107">This common task discusses how toospecify an HTTPS endpoint for a web role and how tooupload an SSL certificate toosecure your application.</span></span>

> [!NOTE]
> <span data-ttu-id="353c0-108">Hello postupy v této úloze platí tooAzure cloudové služby; Aplikační služby, najdete v části [to](../app-service-web/web-sites-configure-ssl-certificate.md) článku.</span><span class="sxs-lookup"><span data-stu-id="353c0-108">hello procedures in this task apply tooAzure Cloud Services; for App Services, see [this](../app-service-web/web-sites-configure-ssl-certificate.md) article.</span></span>
> 
> 

<span data-ttu-id="353c0-109">Tato úloha používá produkčním nasazení.</span><span class="sxs-lookup"><span data-stu-id="353c0-109">This task uses a production deployment.</span></span> <span data-ttu-id="353c0-110">Informace o používání pracovní nasazení se poskytuje na konci hello v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="353c0-110">Information on using a staging deployment is provided at hello end of this topic.</span></span>

<span data-ttu-id="353c0-111">Čtení [to](cloud-services-how-to-create-deploy.md) nejprve článek, pokud jste ještě nevytvořili cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="353c0-111">Read [this](cloud-services-how-to-create-deploy.md) article first if you have not yet created a cloud service.</span></span>

[!INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a><span data-ttu-id="353c0-112">Krok 1: Získání certifikátu protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="353c0-112">Step 1: Get an SSL certificate</span></span>
<span data-ttu-id="353c0-113">tooconfigure SSL pro aplikaci, musíte nejdřív tooget certifikát SSL, která byla podepsána pomocí certifikační autoritou (CA), důvěryhodná třetí straně, která vystavuje certifikáty pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="353c0-113">tooconfigure SSL for an application, you first need tooget an SSL certificate that has been signed by a Certificate Authority (CA), a trusted third party who issues certificates for this purpose.</span></span> <span data-ttu-id="353c0-114">Pokud není již nemáte, je třeba tooobtain jeden ze společnosti, která prodává certifikáty SSL.</span><span class="sxs-lookup"><span data-stu-id="353c0-114">If you do not already have one, you need tooobtain one from a company that sells SSL certificates.</span></span>

<span data-ttu-id="353c0-115">certifikát Hello musí splňovat následující požadavky na certifikáty SSL v Azure hello:</span><span class="sxs-lookup"><span data-stu-id="353c0-115">hello certificate must meet hello following requirements for SSL certificates in Azure:</span></span>

* <span data-ttu-id="353c0-116">Hello certifikát musí obsahovat privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="353c0-116">hello certificate must contain a private key.</span></span>
* <span data-ttu-id="353c0-117">Hello certifikátu musí být vytvořeny pro výměnu klíčů, exportovatelný tooa soubor Personal Information Exchange (.pfx).</span><span class="sxs-lookup"><span data-stu-id="353c0-117">hello certificate must be created for key exchange, exportable tooa Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="353c0-118">Hello názvu subjektu certifikátu musí odpovídat hello domény použít tooaccess hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="353c0-118">hello certificate's subject name must match hello domain used tooaccess hello cloud service.</span></span> <span data-ttu-id="353c0-119">Nelze získat certifikát SSL od certifikační autority (CA) pro doménu cloudapp.net hello.</span><span class="sxs-lookup"><span data-stu-id="353c0-119">You cannot obtain an SSL certificate from a certificate authority (CA) for hello cloudapp.net domain.</span></span> <span data-ttu-id="353c0-120">Musíte získat toouse název vlastní domény při přístupu ke službě.</span><span class="sxs-lookup"><span data-stu-id="353c0-120">You must acquire a custom domain name toouse when access your service.</span></span> <span data-ttu-id="353c0-121">Pokud budete požadovat certifikát od certifikační Autority, musí se název subjektu certifikátu hello odpovídat tooaccess název používaný hello vlastní domény aplikace.</span><span class="sxs-lookup"><span data-stu-id="353c0-121">When you request a certificate from a CA, hello certificate's subject name must match hello custom domain name used tooaccess your application.</span></span> <span data-ttu-id="353c0-122">Například, pokud je váš vlastní název domény **contoso.com** by vyžádání certifikátu z certifikační Autority pro ***. contoso.com** nebo **www.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="353c0-122">For example, if your custom domain name is **contoso.com** you would request a certificate from your CA for ***.contoso.com** or **www.contoso.com**.</span></span>
* <span data-ttu-id="353c0-123">Hello certifikát musí používat minimálně 2048bitové šifrování.</span><span class="sxs-lookup"><span data-stu-id="353c0-123">hello certificate must use a minimum of 2048-bit encryption.</span></span>

<span data-ttu-id="353c0-124">Pro účely testování můžete [vytvořit](cloud-services-certs-create.md) a použít certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="353c0-124">For test purposes, you can [create](cloud-services-certs-create.md) and use a self-signed certificate.</span></span> <span data-ttu-id="353c0-125">Certifikát podepsaný svým držitelem není ověřen pomocí certifikační Autority a hello cloudapp.net domény můžete použít jako adresu URL webu hello.</span><span class="sxs-lookup"><span data-stu-id="353c0-125">A self-signed certificate is not authenticated through a CA and can use hello cloudapp.net domain as hello website URL.</span></span> <span data-ttu-id="353c0-126">Například hello následující úkol používá certifikát podepsaný svým držitelem, ve které hello je běžný název (CN) použitý v certifikátu hello **sslexample.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="353c0-126">For example, hello following task uses a self-signed certificate in which hello common name (CN) used in hello certificate is **sslexample.cloudapp.net**.</span></span>

<span data-ttu-id="353c0-127">V dalším kroku musí zahrnovat informace o certifikátu hello v definici služby a služby konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="353c0-127">Next, you must include information about hello certificate in your service definition and service configuration files.</span></span>

## <a name="step-2-modify-hello-service-definition-and-configuration-files"></a><span data-ttu-id="353c0-128">Krok 2: Upravte soubory pro definice a konfigurace služby hello</span><span class="sxs-lookup"><span data-stu-id="353c0-128">Step 2: Modify hello service definition and configuration files</span></span>
<span data-ttu-id="353c0-129">Aplikace musí být nakonfigurované toouse hello certifikátu a koncový bod HTTPS musí být přidán.</span><span class="sxs-lookup"><span data-stu-id="353c0-129">Your application must be configured toouse hello certificate, and an HTTPS endpoint must be added.</span></span> <span data-ttu-id="353c0-130">V důsledku toho hello definice služby a soubory konfigurace služby musí toobe aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="353c0-130">As a result, hello service definition and service configuration files need toobe updated.</span></span>

1. <span data-ttu-id="353c0-131">Ve vašem vývojovém prostředí, otevřete soubor definice služby hello (CSDEF), přidejte **certifikáty** části v rámci hello **WebRole** části a zahrnovat hello následující informace certifikát (a zprostředkující certifikáty):</span><span class="sxs-lookup"><span data-stu-id="353c0-131">In your development environment, open hello service definition file (CSDEF), add a **Certificates** section within hello **WebRole** section, and include hello following information about the certificate (and intermediate certificates):</span></span>
   
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
   
   <span data-ttu-id="353c0-132">Hello **certifikáty** oddíl definuje název hello naše certifikát, její umístění a název hello hello úložiště, kde se nachází.</span><span class="sxs-lookup"><span data-stu-id="353c0-132">hello **Certificates** section defines hello name of our certificate, its location, and hello name of hello store where it is located.</span></span>
   
   <span data-ttu-id="353c0-133">Oprávnění (`permisionLevel` atribut) může být sada tooone Dobrý den následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="353c0-133">Permissions (`permisionLevel` attribute) can be set tooone of hello following values:</span></span>
   
   | <span data-ttu-id="353c0-134">Hodnota oprávnění</span><span class="sxs-lookup"><span data-stu-id="353c0-134">Permission Value</span></span> | <span data-ttu-id="353c0-135">Popis</span><span class="sxs-lookup"><span data-stu-id="353c0-135">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="353c0-136">limitedOrElevated</span><span class="sxs-lookup"><span data-stu-id="353c0-136">limitedOrElevated</span></span> |<span data-ttu-id="353c0-137">**(Výchozí)**  Všechny procesy role můžete přístup hello privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="353c0-137">**(Default)** All role processes can access hello private key.</span></span> |
   | <span data-ttu-id="353c0-138">zvýšené</span><span class="sxs-lookup"><span data-stu-id="353c0-138">elevated</span></span> |<span data-ttu-id="353c0-139">K privátní klíč hello přístup jenom procesy se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="353c0-139">Only elevated processes can access hello private key.</span></span> |
2. <span data-ttu-id="353c0-140">V souboru definice služby, přidejte **InputEndpoint** v rámci hello **koncové body** části tooenable HTTPS:</span><span class="sxs-lookup"><span data-stu-id="353c0-140">In your service definition file, add an **InputEndpoint** element within hello **Endpoints** section tooenable HTTPS:</span></span>
   
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

3. <span data-ttu-id="353c0-141">V souboru definice služby, přidejte **vazby** v rámci hello **lokality** části.</span><span class="sxs-lookup"><span data-stu-id="353c0-141">In your service definition file, add a **Binding** element within hello **Sites** section.</span></span> <span data-ttu-id="353c0-142">V této části se přidá toomap vazba HTTPS webu tooyour koncový bod:</span><span class="sxs-lookup"><span data-stu-id="353c0-142">This section adds an HTTPS binding toomap the endpoint tooyour site:</span></span>
   
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
   
   <span data-ttu-id="353c0-143">Byly dokončeny všechny hello požadované změny toohello souboru definice služby, ale stále potřebujete informace o certifikátu hello tooadd hello služby konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="353c0-143">All hello required changes toohello service definition file have been completed, but you still need tooadd hello certificate information to hello service configuration file.</span></span>
4. <span data-ttu-id="353c0-144">V konfiguračním souboru služby (CSCFG), ServiceConfiguration.Cloud.cscfg, přidejte **certifikáty** části v rámci hello **Role** části nahraďte hello ukázka kryptografický otisk hodnoty zobrazené níže s který vašeho certifikátu:</span><span class="sxs-lookup"><span data-stu-id="353c0-144">In your service configuration file (CSCFG), ServiceConfiguration.Cloud.cscfg, add a **Certificates** section within hello **Role** section, replacing hello sample thumbprint value shown below with that of your certificate:</span></span>
   
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

<span data-ttu-id="353c0-145">(hello předcházející příklad používá **sha1** pro hello kryptografický algoritmus.</span><span class="sxs-lookup"><span data-stu-id="353c0-145">(hello preceding example uses **sha1** for hello thumbprint algorithm.</span></span> <span data-ttu-id="353c0-146">Zadejte hodnotu hello vhodnou pro váš certifikát kryptografický algoritmus.)</span><span class="sxs-lookup"><span data-stu-id="353c0-146">Specify hello appropriate value for your certificate's thumbprint algorithm.)</span></span>

<span data-ttu-id="353c0-147">Teď, když hello služby definice a služby konfigurační soubory byly aktualizovány, balíček nasazení pro nahrávání tooAzure.</span><span class="sxs-lookup"><span data-stu-id="353c0-147">Now that hello service definition and service configuration files have been updated, package your deployment for uploading tooAzure.</span></span> <span data-ttu-id="353c0-148">Pokud používáte **cspack**, nepoužívejte **/generateConfigurationFile** příznak, jako který přepíše informací o certifikátu, můžete vložit.</span><span class="sxs-lookup"><span data-stu-id="353c0-148">If you are using **cspack**, don't use the **/generateConfigurationFile** flag, as that overwrites the certificate information you inserted.</span></span>

## <a name="step-3-upload-a-certificate"></a><span data-ttu-id="353c0-149">Krok 3: Nahrát na server certifikát</span><span class="sxs-lookup"><span data-stu-id="353c0-149">Step 3: Upload a certificate</span></span>
<span data-ttu-id="353c0-150">Balíček pro nasazení, musí být aktualizované toouse hello certifikátu a byl přidán koncový bod HTTPS.</span><span class="sxs-lookup"><span data-stu-id="353c0-150">Your deployment package has been updated toouse hello certificate, and an HTTPS endpoint has been added.</span></span> <span data-ttu-id="353c0-151">Nyní můžete nahrát hello certifikátů a balíčků tooAzure s hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="353c0-151">Now you can upload hello package and certificate tooAzure with hello Azure classic portal.</span></span>

1. <span data-ttu-id="353c0-152">Přihlaste se toohello [portál Azure classic][Azure classic portal].</span><span class="sxs-lookup"><span data-stu-id="353c0-152">Log in toohello [Azure classic portal][Azure classic portal].</span></span> 
2. <span data-ttu-id="353c0-153">Klikněte na tlačítko **cloudové služby** na levém navigačním podokně hello.</span><span class="sxs-lookup"><span data-stu-id="353c0-153">Click **Cloud Services** on hello left-side navigation pane.</span></span>
3. <span data-ttu-id="353c0-154">Klikněte na tlačítko hello potřeby cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="353c0-154">Click hello desired cloud service.</span></span>
4. <span data-ttu-id="353c0-155">Klikněte na tlačítko hello **certifikáty** kartě.</span><span class="sxs-lookup"><span data-stu-id="353c0-155">Click hello **Certificates** tab.</span></span>
   
    ![Klikněte na kartu certifikáty hello](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. <span data-ttu-id="353c0-157">Klikněte na tlačítko hello **nahrát** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="353c0-157">Click hello **Upload** button.</span></span>
   
    ![Odeslat](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. <span data-ttu-id="353c0-159">Zadejte hello **soubor**, **heslo**, pak klikněte na tlačítko **Complete** (hello zaškrtnutí).</span><span class="sxs-lookup"><span data-stu-id="353c0-159">Provide hello **File**, **Password**, then click **Complete** (hello checkmark).</span></span>

## <a name="step-4-connect-toohello-role-instance-by-using-https"></a><span data-ttu-id="353c0-160">Krok 4: Připojte instanci role toohello pomocí protokolu HTTPS</span><span class="sxs-lookup"><span data-stu-id="353c0-160">Step 4: Connect toohello role instance by using HTTPS</span></span>
<span data-ttu-id="353c0-161">Teď, když vaše nasazení je spuštěná v Azure, můžete připojit tooit pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="353c0-161">Now that your deployment is up and running in Azure, you can connect tooit using HTTPS.</span></span>

1. <span data-ttu-id="353c0-162">V hello portál Azure classic, vyberte nasazení a pak klikněte na odkaz hello pod **adresa URL webu**.</span><span class="sxs-lookup"><span data-stu-id="353c0-162">In hello Azure classic portal, select your deployment, then click hello link under **Site URL**.</span></span>
   
   ![Určení adresy URL webu][2]
2. <span data-ttu-id="353c0-164">Ve webovém prohlížeči, upravte hello odkaz toouse **https** místo **http**a potom navštivte stránku hello.</span><span class="sxs-lookup"><span data-stu-id="353c0-164">In your web browser, modify hello link toouse **https** instead of **http**, and then visit hello page.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="353c0-165">Pokud používáte certifikát podepsaný svým držitelem, když procházíte tooan koncový bod HTTPS, který je spojen s certifikát podepsaný svým držitelem hello může se zobrazit chyba certifikátu v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="353c0-165">If you are using a self-signed certificate, when you browse tooan HTTPS endpoint that's associated with hello self-signed certificate you may see a certificate error in hello browser.</span></span> <span data-ttu-id="353c0-166">Používá certifikát podepsaný důvěryhodnou certifikační autoritou eliminuje tento problém; v hello té doby chybu můžete ignorovat hello.</span><span class="sxs-lookup"><span data-stu-id="353c0-166">Using a certificate signed by a trusted certification authority eliminates this problem; in hello meantime, you can ignore hello error.</span></span> <span data-ttu-id="353c0-167">(Další možností je tooadd hello certifikát podepsaný svým držitelem toohello důvěryhodné certifikační autority úložišti certifikátů uživatele.)</span><span class="sxs-lookup"><span data-stu-id="353c0-167">(Another option is tooadd hello self-signed certificate toohello user's trusted certificate authority certificate store.)</span></span>
   > 
   > 
   
   ![SSL příklad webové stránky][3]

<span data-ttu-id="353c0-169">Pokud chcete toouse SSL pro nasazení pracovní místo produkční nasazení, je nutné nejprve toodetermine hello adresa URL použitá pro hello pracovní nasazení.</span><span class="sxs-lookup"><span data-stu-id="353c0-169">If you want toouse SSL for a staging deployment instead of a production deployment, you first need toodetermine hello URL used for hello staging deployment.</span></span> <span data-ttu-id="353c0-170">Nasazení pracovní prostředí cloudové služby toohello bez zahrnutí certifikát nebo všech informací o certifikátu.</span><span class="sxs-lookup"><span data-stu-id="353c0-170">Deploy your cloud service toohello staging environment without including a certificate or any certificate information.</span></span> <span data-ttu-id="353c0-171">Po nasazení můžete určit hello na základě GUID adresu URL, která je uvedena v hello portál Azure classic na **adresa URL webu** pole.</span><span class="sxs-lookup"><span data-stu-id="353c0-171">Once deployed, you can determine hello GUID-based URL, which is listed in hello Azure classic portal's **Site URL** field.</span></span> <span data-ttu-id="353c0-172">Vytvoření certifikátu s hello běžný název (CN) rovna toohello na základě GUID adresou URL (například **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**).</span><span class="sxs-lookup"><span data-stu-id="353c0-172">Create a certificate with hello common name (CN) equal toohello GUID-based URL (for example, **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**).</span></span> <span data-ttu-id="353c0-173">Použití hello Azure classic portálu tooadd hello certifikát tooyour připravený cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="353c0-173">Use hello Azure classic portal tooadd hello certificate tooyour staged cloud service.</span></span> <span data-ttu-id="353c0-174">Pak přidejte hello informace tooyour CSDEF a CSCFG soubory certifikátů, znovu zabalte aplikaci a aktualizovat vaše dvoufázové instalace toouse hello nový balíček pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="353c0-174">Then, add hello certificate information tooyour CSDEF and CSCFG files, repackage your application, and update your staged deployment toouse hello new package.</span></span>

## <a name="next-steps"></a><span data-ttu-id="353c0-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="353c0-175">Next steps</span></span>
* <span data-ttu-id="353c0-176">[Obecná konfigurace cloudové služby](cloud-services-how-to-configure.md).</span><span class="sxs-lookup"><span data-stu-id="353c0-176">[General configuration of your cloud service](cloud-services-how-to-configure.md).</span></span>
* <span data-ttu-id="353c0-177">Zjistěte, jak příliš[nasazení cloudové služby](cloud-services-how-to-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="353c0-177">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy.md).</span></span>
* <span data-ttu-id="353c0-178">Konfigurace [vlastní název domény](cloud-services-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="353c0-178">Configure a [custom domain name](cloud-services-custom-domain-name.md).</span></span>
* <span data-ttu-id="353c0-179">[Správa služby cloud](cloud-services-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="353c0-179">[Manage your cloud service](cloud-services-how-to-manage.md).</span></span>

[Azure classic portal]: http://manage.windowsazure.com
[0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
[1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
[2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
[3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
[4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
