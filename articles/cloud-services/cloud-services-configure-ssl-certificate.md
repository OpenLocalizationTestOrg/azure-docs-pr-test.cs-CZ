---
title: "Konfigurace protokolu SSL pro cloudové služby (klasické) | Microsoft Docs"
description: "Zjistěte, jak zadat koncový bod HTTPS pro webovou roli a jak nahrát certifikát SSL pro zabezpečení vaší aplikace."
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
ms.openlocfilehash: edb9aaf6dae11c9b8a171b22bc8a17003f80d86b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a><span data-ttu-id="36da5-103">Konfigurace protokolu SSL pro všechny aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="36da5-103">Configuring SSL for an application in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="36da5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="36da5-104">Azure portal</span></span>](cloud-services-configure-ssl-certificate-portal.md)
> * [<span data-ttu-id="36da5-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="36da5-105">Azure classic portal</span></span>](cloud-services-configure-ssl-certificate.md)
> 
> 

<span data-ttu-id="36da5-106">Šifrování SSL (Secure Socket Layer) je nejčastěji používanou metodou zabezpečení dat posílaných přes internet.</span><span class="sxs-lookup"><span data-stu-id="36da5-106">Secure Socket Layer (SSL) encryption is the most commonly used method of securing data sent across the internet.</span></span> <span data-ttu-id="36da5-107">Článek o tomto běžném postupu probírá, jak zadat koncový bod HTTPS pro webovou roli a jak nahrát certifikát SSL pro zabezpečení aplikace.</span><span class="sxs-lookup"><span data-stu-id="36da5-107">This common task discusses how to specify an HTTPS endpoint for a web role and how to upload an SSL certificate to secure your application.</span></span>

> [!NOTE]
> <span data-ttu-id="36da5-108">Postupy v této úloze platí pro Azure Cloud Services; Aplikační služby, najdete v části [to](../app-service-web/web-sites-configure-ssl-certificate.md) článku.</span><span class="sxs-lookup"><span data-stu-id="36da5-108">The procedures in this task apply to Azure Cloud Services; for App Services, see [this](../app-service-web/web-sites-configure-ssl-certificate.md) article.</span></span>
> 
> 

<span data-ttu-id="36da5-109">Tato úloha používá produkčním nasazení.</span><span class="sxs-lookup"><span data-stu-id="36da5-109">This task uses a production deployment.</span></span> <span data-ttu-id="36da5-110">Informace o používání pracovní nasazení najdete na konci tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="36da5-110">Information on using a staging deployment is provided at the end of this topic.</span></span>

<span data-ttu-id="36da5-111">Čtení [to](cloud-services-how-to-create-deploy.md) nejprve článek, pokud jste ještě nevytvořili cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="36da5-111">Read [this](cloud-services-how-to-create-deploy.md) article first if you have not yet created a cloud service.</span></span>

[!INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a><span data-ttu-id="36da5-112">Krok 1: Získání certifikátu protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="36da5-112">Step 1: Get an SSL certificate</span></span>
<span data-ttu-id="36da5-113">Konfigurace protokolu SSL pro aplikaci, musíte nejdřív získat certifikát SSL, která byla podepsána pomocí certifikační autoritou (CA), důvěryhodná třetí straně, která vystavuje certifikáty pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="36da5-113">To configure SSL for an application, you first need to get an SSL certificate that has been signed by a Certificate Authority (CA), a trusted third party who issues certificates for this purpose.</span></span> <span data-ttu-id="36da5-114">Pokud není již nemáte, budete muset získat jeden ze společnosti, která prodává certifikáty SSL.</span><span class="sxs-lookup"><span data-stu-id="36da5-114">If you do not already have one, you need to obtain one from a company that sells SSL certificates.</span></span>

<span data-ttu-id="36da5-115">Certifikát musí splňovat následující požadavky na certifikáty SSL v Azure:</span><span class="sxs-lookup"><span data-stu-id="36da5-115">The certificate must meet the following requirements for SSL certificates in Azure:</span></span>

* <span data-ttu-id="36da5-116">Certifikát musí obsahovat privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="36da5-116">The certificate must contain a private key.</span></span>
* <span data-ttu-id="36da5-117">Certifikát se musí vytvořit pro výměnu klíčů, exportovat do souboru Personal Information Exchange (.pfx).</span><span class="sxs-lookup"><span data-stu-id="36da5-117">The certificate must be created for key exchange, exportable to a Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="36da5-118">Název subjektu certifikátu musí odpovídat domény používá pro přístup ke cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="36da5-118">The certificate's subject name must match the domain used to access the cloud service.</span></span> <span data-ttu-id="36da5-119">Nelze získat certifikát SSL od certifikační autority (CA) pro doménu cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="36da5-119">You cannot obtain an SSL certificate from a certificate authority (CA) for the cloudapp.net domain.</span></span> <span data-ttu-id="36da5-120">Musíte získat vlastní název domény má použít při přístupu ke službě.</span><span class="sxs-lookup"><span data-stu-id="36da5-120">You must acquire a custom domain name to use when access your service.</span></span> <span data-ttu-id="36da5-121">Pokud budete požadovat certifikát od certifikační Autority, název subjektu certifikátu musí odpovídat názvu vlastní domény, používá pro přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="36da5-121">When you request a certificate from a CA, the certificate's subject name must match the custom domain name used to access your application.</span></span> <span data-ttu-id="36da5-122">Například, pokud je váš vlastní název domény **contoso.com** by vyžádání certifikátu z certifikační Autority pro ***. contoso.com** nebo **www.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="36da5-122">For example, if your custom domain name is **contoso.com** you would request a certificate from your CA for ***.contoso.com** or **www.contoso.com**.</span></span>
* <span data-ttu-id="36da5-123">Certifikát musí používat minimálně 2048bitové šifrování.</span><span class="sxs-lookup"><span data-stu-id="36da5-123">The certificate must use a minimum of 2048-bit encryption.</span></span>

<span data-ttu-id="36da5-124">Pro účely testování můžete [vytvořit](cloud-services-certs-create.md) a použít certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="36da5-124">For test purposes, you can [create](cloud-services-certs-create.md) and use a self-signed certificate.</span></span> <span data-ttu-id="36da5-125">Certifikát podepsaný svým držitelem není ověřen pomocí certifikační Autority a cloudapp.net domény můžete použít jako adresu URL webu.</span><span class="sxs-lookup"><span data-stu-id="36da5-125">A self-signed certificate is not authenticated through a CA and can use the cloudapp.net domain as the website URL.</span></span> <span data-ttu-id="36da5-126">Například následující úkol používá certifikát podepsaný svým držitelem, ve kterém je běžný název (CN) použitý v certifikátu **sslexample.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="36da5-126">For example, the following task uses a self-signed certificate in which the common name (CN) used in the certificate is **sslexample.cloudapp.net**.</span></span>

<span data-ttu-id="36da5-127">V dalším kroku musí zahrnovat informace o certifikátu v definici služby a služby konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="36da5-127">Next, you must include information about the certificate in your service definition and service configuration files.</span></span>

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a><span data-ttu-id="36da5-128">Krok 2: Upravte soubory definice a konfigurace služby</span><span class="sxs-lookup"><span data-stu-id="36da5-128">Step 2: Modify the service definition and configuration files</span></span>
<span data-ttu-id="36da5-129">Vaše aplikace musí být nakonfigurována pro použití certifikátu a koncový bod HTTPS musí být přidán.</span><span class="sxs-lookup"><span data-stu-id="36da5-129">Your application must be configured to use the certificate, and an HTTPS endpoint must be added.</span></span> <span data-ttu-id="36da5-130">Definice služby a služby konfigurační soubory v důsledku toho musí aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="36da5-130">As a result, the service definition and service configuration files need to be updated.</span></span>

1. <span data-ttu-id="36da5-131">Ve vašem vývojovém prostředí, otevřete soubor definice služby (CSDEF), přidejte **certifikáty** části v rámci **WebRole** části a uveďte tyto informace o certifikátu (a zprostředkující certifikáty):</span><span class="sxs-lookup"><span data-stu-id="36da5-131">In your development environment, open the service definition file (CSDEF), add a **Certificates** section within the **WebRole** section, and include the following information about the certificate (and intermediate certificates):</span></span>
   
    ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Certificates>
            <Certificate name="SampleCertificate" 
                        storeLocation="LocalMachine" 
                        storeName="My"
                        permissionLevel="limitedOrElevated" />
            <!-- IMPORTANT! Unless your certificate is either
            self-signed or signed directly by the CA root, you
            must include all the intermediate certificates
            here. You must list them here, even if they are
            not bound to any endpoints. Failing to list any of
            the intermediate certificates may cause hard-to-reproduce
            interoperability problems on some clients.-->
            <Certificate name="CAForSampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="CA"
                        permissionLevel="limitedOrElevated" />
        </Certificates>
    ...
    </WebRole>
    ```
   
   <span data-ttu-id="36da5-132">**Certifikáty** oddíl definuje název naše certifikát, její umístění a název úložiště, kde se nachází.</span><span class="sxs-lookup"><span data-stu-id="36da5-132">The **Certificates** section defines the name of our certificate, its location, and the name of the store where it is located.</span></span>
   
   <span data-ttu-id="36da5-133">Oprávnění (`permisionLevel` atribut) můžete nastavit na jedno z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="36da5-133">Permissions (`permisionLevel` attribute) can be set to one of the following values:</span></span>
   
   | <span data-ttu-id="36da5-134">Hodnota oprávnění</span><span class="sxs-lookup"><span data-stu-id="36da5-134">Permission Value</span></span> | <span data-ttu-id="36da5-135">Popis</span><span class="sxs-lookup"><span data-stu-id="36da5-135">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="36da5-136">limitedOrElevated</span><span class="sxs-lookup"><span data-stu-id="36da5-136">limitedOrElevated</span></span> |<span data-ttu-id="36da5-137">**(Výchozí)**  Všechny procesy role můžete přístup k privátnímu klíči.</span><span class="sxs-lookup"><span data-stu-id="36da5-137">**(Default)** All role processes can access the private key.</span></span> |
   | <span data-ttu-id="36da5-138">zvýšené</span><span class="sxs-lookup"><span data-stu-id="36da5-138">elevated</span></span> |<span data-ttu-id="36da5-139">Jenom procesy se zvýšenými oprávněními, můžete přístup k privátnímu klíči.</span><span class="sxs-lookup"><span data-stu-id="36da5-139">Only elevated processes can access the private key.</span></span> |
2. <span data-ttu-id="36da5-140">V souboru definice služby, přidejte **InputEndpoint** v rámci **koncové body** části Povolit protokol HTTPS:</span><span class="sxs-lookup"><span data-stu-id="36da5-140">In your service definition file, add an **InputEndpoint** element within the **Endpoints** section to enable HTTPS:</span></span>
   
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

3. <span data-ttu-id="36da5-141">V souboru definice služby, přidejte **vazby** v prvku **lokality** části.</span><span class="sxs-lookup"><span data-stu-id="36da5-141">In your service definition file, add a **Binding** element within the **Sites** section.</span></span> <span data-ttu-id="36da5-142">Tato část přidá vazbu HTTPS k mapování koncový bod na váš web:</span><span class="sxs-lookup"><span data-stu-id="36da5-142">This section adds an HTTPS binding to map the endpoint to your site:</span></span>
   
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
   
   <span data-ttu-id="36da5-143">Byly dokončeny všechny požadované změny pro soubor definice služby, ale potřebujete přidat informace o certifikátu do konfiguračního souboru služby.</span><span class="sxs-lookup"><span data-stu-id="36da5-143">All the required changes to the service definition file have been completed, but you still need to add the certificate information to the service configuration file.</span></span>
4. <span data-ttu-id="36da5-144">V konfiguračním souboru služby (CSCFG), ServiceConfiguration.Cloud.cscfg, přidejte **certifikáty** části v rámci **Role** části nahraďte ukázkové hodnoty kryptografického otisku vidíte níže se u vašeho certifikátu:</span><span class="sxs-lookup"><span data-stu-id="36da5-144">In your service configuration file (CSCFG), ServiceConfiguration.Cloud.cscfg, add a **Certificates** section within the **Role** section, replacing the sample thumbprint value shown below with that of your certificate:</span></span>
   
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

<span data-ttu-id="36da5-145">(V předchozím příkladu používá **sha1** pro algoritmus kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="36da5-145">(The preceding example uses **sha1** for the thumbprint algorithm.</span></span> <span data-ttu-id="36da5-146">Zadejte hodnotu vhodnou pro váš certifikát kryptografický algoritmus.)</span><span class="sxs-lookup"><span data-stu-id="36da5-146">Specify the appropriate value for your certificate's thumbprint algorithm.)</span></span>

<span data-ttu-id="36da5-147">Teď, když byly aktualizovány definice služby a služby konfigurační soubory, balíček nasazení nahrát do Azure.</span><span class="sxs-lookup"><span data-stu-id="36da5-147">Now that the service definition and service configuration files have been updated, package your deployment for uploading to Azure.</span></span> <span data-ttu-id="36da5-148">Pokud používáte **cspack**, nepoužívejte **/generateConfigurationFile** příznak, jako který přepíše informací o certifikátu, můžete vložit.</span><span class="sxs-lookup"><span data-stu-id="36da5-148">If you are using **cspack**, don't use the **/generateConfigurationFile** flag, as that overwrites the certificate information you inserted.</span></span>

## <a name="step-3-upload-a-certificate"></a><span data-ttu-id="36da5-149">Krok 3: Nahrát na server certifikát</span><span class="sxs-lookup"><span data-stu-id="36da5-149">Step 3: Upload a certificate</span></span>
<span data-ttu-id="36da5-150">Balíček pro nasazení byl aktualizován na použití certifikátu a byl přidán koncový bod HTTPS.</span><span class="sxs-lookup"><span data-stu-id="36da5-150">Your deployment package has been updated to use the certificate, and an HTTPS endpoint has been added.</span></span> <span data-ttu-id="36da5-151">Nyní můžete nahrávat certifikátů a balíčků do Azure pomocí portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="36da5-151">Now you can upload the package and certificate to Azure with the Azure classic portal.</span></span>

1. <span data-ttu-id="36da5-152">Přihlaste se k [portál Azure classic][Azure classic portal].</span><span class="sxs-lookup"><span data-stu-id="36da5-152">Log in to the [Azure classic portal][Azure classic portal].</span></span> 
2. <span data-ttu-id="36da5-153">Klikněte na tlačítko **cloudové služby** v levém navigačním podokně.</span><span class="sxs-lookup"><span data-stu-id="36da5-153">Click **Cloud Services** on the left-side navigation pane.</span></span>
3. <span data-ttu-id="36da5-154">Klikněte na požadovanou cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="36da5-154">Click the desired cloud service.</span></span>
4. <span data-ttu-id="36da5-155">Klikněte **certifikáty** kartě.</span><span class="sxs-lookup"><span data-stu-id="36da5-155">Click the **Certificates** tab.</span></span>
   
    ![Klikněte na kartu certifikáty](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. <span data-ttu-id="36da5-157">Klikněte na tlačítko **Nahrát**.</span><span class="sxs-lookup"><span data-stu-id="36da5-157">Click the **Upload** button.</span></span>
   
    ![Odeslat](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. <span data-ttu-id="36da5-159">Zadejte **soubor**, **heslo**, pak klikněte na tlačítko **Complete** (na značku zaškrtnutí).</span><span class="sxs-lookup"><span data-stu-id="36da5-159">Provide the **File**, **Password**, then click **Complete** (the checkmark).</span></span>

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a><span data-ttu-id="36da5-160">Krok 4: Připojení k instanci role pomocí protokolu HTTPS</span><span class="sxs-lookup"><span data-stu-id="36da5-160">Step 4: Connect to the role instance by using HTTPS</span></span>
<span data-ttu-id="36da5-161">Teď, když vaše nasazení je spuštěná v Azure, můžete připojit se pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="36da5-161">Now that your deployment is up and running in Azure, you can connect to it using HTTPS.</span></span>

1. <span data-ttu-id="36da5-162">Na portálu Azure classic, vyberte nasazení a pak klikněte na odkaz v části **adresa URL webu**.</span><span class="sxs-lookup"><span data-stu-id="36da5-162">In the Azure classic portal, select your deployment, then click the link under **Site URL**.</span></span>
   
   ![Určení adresy URL webu][2]
2. <span data-ttu-id="36da5-164">Ve webovém prohlížeči, upravte odkaz použít **https** místo **http**a potom navštivte stránku.</span><span class="sxs-lookup"><span data-stu-id="36da5-164">In your web browser, modify the link to use **https** instead of **http**, and then visit the page.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="36da5-165">Pokud používáte certifikát podepsaný svým držitelem, když přejdete na koncový bod HTTPS, který je spojen s certifikát podepsaný svým držitelem může se zobrazit chyba certifikátu v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="36da5-165">If you are using a self-signed certificate, when you browse to an HTTPS endpoint that's associated with the self-signed certificate you may see a certificate error in the browser.</span></span> <span data-ttu-id="36da5-166">Používá certifikát podepsaný důvěryhodnou certifikační autoritou eliminuje tento problém; do té doby můžete chybu ignorovat.</span><span class="sxs-lookup"><span data-stu-id="36da5-166">Using a certificate signed by a trusted certification authority eliminates this problem; in the meantime, you can ignore the error.</span></span> <span data-ttu-id="36da5-167">(Další možností je přidání certifikátu podepsaného svým držitelem do úložiště certifikátů důvěryhodné certifikační autority uživatele.)</span><span class="sxs-lookup"><span data-stu-id="36da5-167">(Another option is to add the self-signed certificate to the user's trusted certificate authority certificate store.)</span></span>
   > 
   > 
   
   ![SSL příklad webové stránky][3]

<span data-ttu-id="36da5-169">Pokud chcete používat protokol SSL pro pracovní nasazení místo produkční nasazení, musíte nejdřív určit adresu URL pro pracovní nasazení použít.</span><span class="sxs-lookup"><span data-stu-id="36da5-169">If you want to use SSL for a staging deployment instead of a production deployment, you first need to determine the URL used for the staging deployment.</span></span> <span data-ttu-id="36da5-170">Nasazení cloudové služby na pracovní prostředí bez zahrnutí certifikát nebo všech informací o certifikátu.</span><span class="sxs-lookup"><span data-stu-id="36da5-170">Deploy your cloud service to the staging environment without including a certificate or any certificate information.</span></span> <span data-ttu-id="36da5-171">Po nasazení můžete určit adresu URL na základě GUID, který je uveden v portálu Azure classic na **adresa URL webu** pole.</span><span class="sxs-lookup"><span data-stu-id="36da5-171">Once deployed, you can determine the GUID-based URL, which is listed in the Azure classic portal's **Site URL** field.</span></span> <span data-ttu-id="36da5-172">Vytvoření certifikátu pomocí běžný název (CN) rovnou na adresu URL, na základě GUID (například **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**).</span><span class="sxs-lookup"><span data-stu-id="36da5-172">Create a certificate with the common name (CN) equal to the GUID-based URL (for example, **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**).</span></span> <span data-ttu-id="36da5-173">Pomocí portálu Azure classic se přidat certifikát do dvoufázové instalace cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="36da5-173">Use the Azure classic portal to add the certificate to your staged cloud service.</span></span> <span data-ttu-id="36da5-174">Potom CSDEF a CSCFG souborům přidat informace o certifikátu, znovu zabalte aplikaci a aktualizaci dvoufázové nasazení tak, aby pomocí nového balíčku.</span><span class="sxs-lookup"><span data-stu-id="36da5-174">Then, add the certificate information to your CSDEF and CSCFG files, repackage your application, and update your staged deployment to use the new package.</span></span>

## <a name="next-steps"></a><span data-ttu-id="36da5-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="36da5-175">Next steps</span></span>
* <span data-ttu-id="36da5-176">[Obecná konfigurace cloudové služby](cloud-services-how-to-configure.md).</span><span class="sxs-lookup"><span data-stu-id="36da5-176">[General configuration of your cloud service](cloud-services-how-to-configure.md).</span></span>
* <span data-ttu-id="36da5-177">Zjistěte, jak [nasazení cloudové služby](cloud-services-how-to-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="36da5-177">Learn how to [deploy a cloud service](cloud-services-how-to-create-deploy.md).</span></span>
* <span data-ttu-id="36da5-178">Konfigurace [vlastní název domény](cloud-services-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="36da5-178">Configure a [custom domain name](cloud-services-custom-domain-name.md).</span></span>
* <span data-ttu-id="36da5-179">[Správa služby cloud](cloud-services-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="36da5-179">[Manage your cloud service](cloud-services-how-to-manage.md).</span></span>

[Azure classic portal]: http://manage.windowsazure.com
[0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
[1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
[2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
[3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
[4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
