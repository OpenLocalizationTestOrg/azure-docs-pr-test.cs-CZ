---
title: "Konfigurace protokolu SSL pro cloudové služby | Microsoft Docs"
description: "Zjistěte, jak zadat koncový bod HTTPS pro webovou roli a jak nahrát certifikát SSL pro zabezpečení vaší aplikace. Tyto příklady použití portálu Azure."
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
ms.openlocfilehash: e5c8c3b098772c0586712305a577b24a6f0d924c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a><span data-ttu-id="5ac54-104">Konfigurace protokolu SSL pro všechny aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="5ac54-104">Configuring SSL for an application in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5ac54-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5ac54-105">Azure portal</span></span>](cloud-services-configure-ssl-certificate-portal.md)
> * [<span data-ttu-id="5ac54-106">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="5ac54-106">Azure classic portal</span></span>](cloud-services-configure-ssl-certificate.md)
>

<span data-ttu-id="5ac54-107">Šifrování SSL (Secure Socket Layer) je nejčastěji používanou metodou zabezpečení dat posílaných přes internet.</span><span class="sxs-lookup"><span data-stu-id="5ac54-107">Secure Socket Layer (SSL) encryption is the most commonly used method of securing data sent across the internet.</span></span> <span data-ttu-id="5ac54-108">Článek o tomto běžném postupu probírá, jak zadat koncový bod HTTPS pro webovou roli a jak nahrát certifikát SSL pro zabezpečení aplikace.</span><span class="sxs-lookup"><span data-stu-id="5ac54-108">This common task discusses how to specify an HTTPS endpoint for a web role and how to upload an SSL certificate to secure your application.</span></span>

> [!NOTE]
> <span data-ttu-id="5ac54-109">Postupy v této úloze platí pro Azure Cloud Services; Aplikační služby, najdete v části [to](../app-service-web/web-sites-configure-ssl-certificate.md).</span><span class="sxs-lookup"><span data-stu-id="5ac54-109">The procedures in this task apply to Azure Cloud Services; for App Services, see [this](../app-service-web/web-sites-configure-ssl-certificate.md).</span></span>
>

<span data-ttu-id="5ac54-110">Tato úloha používá produkčním nasazení.</span><span class="sxs-lookup"><span data-stu-id="5ac54-110">This task uses a production deployment.</span></span> <span data-ttu-id="5ac54-111">Informace o používání pracovní nasazení najdete na konci tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="5ac54-111">Information on using a staging deployment is provided at the end of this topic.</span></span>

<span data-ttu-id="5ac54-112">Čtení [to](cloud-services-how-to-create-deploy-portal.md) první, pokud jste ještě nevytvořili cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="5ac54-112">Read [this](cloud-services-how-to-create-deploy-portal.md) first if you have not yet created a cloud service.</span></span>

[!INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a><span data-ttu-id="5ac54-113">Krok 1: Získání certifikátu protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="5ac54-113">Step 1: Get an SSL certificate</span></span>
<span data-ttu-id="5ac54-114">Konfigurace protokolu SSL pro aplikaci, musíte nejdřív získat certifikát SSL, která byla podepsána pomocí certifikační autoritou (CA), důvěryhodná třetí straně, která vystavuje certifikáty pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="5ac54-114">To configure SSL for an application, you first need to get an SSL certificate that has been signed by a Certificate Authority (CA), a trusted third party who issues certificates for this purpose.</span></span> <span data-ttu-id="5ac54-115">Pokud není již nemáte, budete muset získat jeden ze společnosti, která prodává certifikáty SSL.</span><span class="sxs-lookup"><span data-stu-id="5ac54-115">If you do not already have one, you need to obtain one from a company that sells SSL certificates.</span></span>

<span data-ttu-id="5ac54-116">Certifikát musí splňovat následující požadavky na certifikáty SSL v Azure:</span><span class="sxs-lookup"><span data-stu-id="5ac54-116">The certificate must meet the following requirements for SSL certificates in Azure:</span></span>

* <span data-ttu-id="5ac54-117">Certifikát musí obsahovat privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="5ac54-117">The certificate must contain a private key.</span></span>
* <span data-ttu-id="5ac54-118">Certifikát se musí vytvořit pro výměnu klíčů, exportovat do souboru Personal Information Exchange (.pfx).</span><span class="sxs-lookup"><span data-stu-id="5ac54-118">The certificate must be created for key exchange, exportable to a Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="5ac54-119">Název subjektu certifikátu musí odpovídat domény používá pro přístup ke cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="5ac54-119">The certificate's subject name must match the domain used to access the cloud service.</span></span> <span data-ttu-id="5ac54-120">Nelze získat certifikát SSL od certifikační autority (CA) pro doménu cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="5ac54-120">You cannot obtain an SSL certificate from a certificate authority (CA) for the cloudapp.net domain.</span></span> <span data-ttu-id="5ac54-121">Musíte získat vlastní název domény má použít při přístupu ke službě.</span><span class="sxs-lookup"><span data-stu-id="5ac54-121">You must acquire a custom domain name to use when access your service.</span></span> <span data-ttu-id="5ac54-122">Pokud budete požadovat certifikát od certifikační Autority, název subjektu certifikátu musí odpovídat názvu vlastní domény, používá pro přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5ac54-122">When you request a certificate from a CA, the certificate's subject name must match the custom domain name used to access your application.</span></span> <span data-ttu-id="5ac54-123">Například, pokud je váš vlastní název domény **contoso.com** by vyžádání certifikátu z certifikační Autority pro ***. contoso.com** nebo **www.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="5ac54-123">For example, if your custom domain name is **contoso.com** you would request a certificate from your CA for ***.contoso.com** or **www.contoso.com**.</span></span>
* <span data-ttu-id="5ac54-124">Certifikát musí používat minimálně 2048bitové šifrování.</span><span class="sxs-lookup"><span data-stu-id="5ac54-124">The certificate must use a minimum of 2048-bit encryption.</span></span>

<span data-ttu-id="5ac54-125">Pro účely testování můžete [vytvořit](cloud-services-certs-create.md) a použít certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="5ac54-125">For test purposes, you can [create](cloud-services-certs-create.md) and use a self-signed certificate.</span></span> <span data-ttu-id="5ac54-126">Certifikát podepsaný svým držitelem není ověřen pomocí certifikační Autority a cloudapp.net domény můžete použít jako adresu URL webu.</span><span class="sxs-lookup"><span data-stu-id="5ac54-126">A self-signed certificate is not authenticated through a CA and can use the cloudapp.net domain as the website URL.</span></span> <span data-ttu-id="5ac54-127">Například následující úkol používá certifikát podepsaný svým držitelem, ve kterém je běžný název (CN) použitý v certifikátu **sslexample.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="5ac54-127">For example, the following task uses a self-signed certificate in which the common name (CN) used in the certificate is **sslexample.cloudapp.net**.</span></span>

<span data-ttu-id="5ac54-128">V dalším kroku musí zahrnovat informace o certifikátu v definici služby a služby konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="5ac54-128">Next, you must include information about the certificate in your service definition and service configuration files.</span></span>

<span data-ttu-id="5ac54-129"><a name="modify"> </a></span><span class="sxs-lookup"><span data-stu-id="5ac54-129"><a name="modify"> </a></span></span>

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a><span data-ttu-id="5ac54-130">Krok 2: Upravte soubory definice a konfigurace služby</span><span class="sxs-lookup"><span data-stu-id="5ac54-130">Step 2: Modify the service definition and configuration files</span></span>
<span data-ttu-id="5ac54-131">Vaše aplikace musí být nakonfigurována pro použití certifikátu a koncový bod HTTPS musí být přidán.</span><span class="sxs-lookup"><span data-stu-id="5ac54-131">Your application must be configured to use the certificate, and an HTTPS endpoint must be added.</span></span> <span data-ttu-id="5ac54-132">Definice služby a služby konfigurační soubory v důsledku toho musí aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="5ac54-132">As a result, the service definition and service configuration files need to be updated.</span></span>

1. <span data-ttu-id="5ac54-133">Ve vašem vývojovém prostředí, otevřete soubor definice služby (CSDEF), přidejte **certifikáty** části v rámci **WebRole** části a uveďte tyto informace o certifikátu (a zprostředkující certifikáty):</span><span class="sxs-lookup"><span data-stu-id="5ac54-133">In your development environment, open the service definition file (CSDEF), add a **Certificates** section within the **WebRole** section, and include the following information about the certificate (and intermediate certificates):</span></span>

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

   <span data-ttu-id="5ac54-134">**Certifikáty** oddíl definuje název naše certifikát, její umístění a název úložiště, kde se nachází.</span><span class="sxs-lookup"><span data-stu-id="5ac54-134">The **Certificates** section defines the name of our certificate, its location, and the name of the store where it is located.</span></span>

   <span data-ttu-id="5ac54-135">Oprávnění (`permisionLevel` atribut) můžete nastavit na jedno z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="5ac54-135">Permissions (`permisionLevel` attribute) can be set to one of the following values:</span></span>

   | <span data-ttu-id="5ac54-136">Hodnota oprávnění</span><span class="sxs-lookup"><span data-stu-id="5ac54-136">Permission Value</span></span> | <span data-ttu-id="5ac54-137">Popis</span><span class="sxs-lookup"><span data-stu-id="5ac54-137">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="5ac54-138">limitedOrElevated</span><span class="sxs-lookup"><span data-stu-id="5ac54-138">limitedOrElevated</span></span> |<span data-ttu-id="5ac54-139">**(Výchozí) ** Všechny procesy role můžete přístup k privátnímu klíči.</span><span class="sxs-lookup"><span data-stu-id="5ac54-139">**(Default)** All role processes can access the private key.</span></span> |
   | <span data-ttu-id="5ac54-140">zvýšené</span><span class="sxs-lookup"><span data-stu-id="5ac54-140">elevated</span></span> |<span data-ttu-id="5ac54-141">Jenom procesy se zvýšenými oprávněními, můžete přístup k privátnímu klíči.</span><span class="sxs-lookup"><span data-stu-id="5ac54-141">Only elevated processes can access the private key.</span></span> |

2. <span data-ttu-id="5ac54-142">V souboru definice služby, přidejte **InputEndpoint** v rámci **koncové body** části Povolit protokol HTTPS:</span><span class="sxs-lookup"><span data-stu-id="5ac54-142">In your service definition file, add an **InputEndpoint** element within the **Endpoints** section to enable HTTPS:</span></span>

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

3. <span data-ttu-id="5ac54-143">V souboru definice služby, přidejte **vazby** v prvku **lokality** části.</span><span class="sxs-lookup"><span data-stu-id="5ac54-143">In your service definition file, add a **Binding** element within the **Sites** section.</span></span> <span data-ttu-id="5ac54-144">Tento element přidá vazbu HTTPS k mapování koncový bod na váš web:</span><span class="sxs-lookup"><span data-stu-id="5ac54-144">This element adds an HTTPS binding to map the endpoint to your site:</span></span>

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

   <span data-ttu-id="5ac54-145">Byly dokončeny všechny požadované změny pro soubor definice služby; ale potřebujete přidat informace o certifikátu do konfiguračního souboru služby.</span><span class="sxs-lookup"><span data-stu-id="5ac54-145">All the required changes to the service definition file have been completed; but, you still need to add the certificate information to the service configuration file.</span></span>
4. <span data-ttu-id="5ac54-146">V konfiguračním souboru služby (CSCFG), ServiceConfiguration.Cloud.cscfg, přidejte **certifikáty** hodnotu s u vašeho certifikátu.</span><span class="sxs-lookup"><span data-stu-id="5ac54-146">In your service configuration file (CSCFG), ServiceConfiguration.Cloud.cscfg, add a **Certificates** value with that of your certificate.</span></span> <span data-ttu-id="5ac54-147">Následující ukázka kódu obsahuje podrobnosti o **certifikáty** části, s výjimkou hodnotu kryptografického otisku.</span><span class="sxs-lookup"><span data-stu-id="5ac54-147">The following code sample provides details of the **Certificates** section, except for the thumbprint value.</span></span>

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

<span data-ttu-id="5ac54-148">(Tento příklad používá **sha1** pro algoritmus kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="5ac54-148">(This example uses **sha1** for the thumbprint algorithm.</span></span> <span data-ttu-id="5ac54-149">Zadejte hodnotu vhodnou pro váš certifikát kryptografický algoritmus.)</span><span class="sxs-lookup"><span data-stu-id="5ac54-149">Specify the appropriate value for your certificate's thumbprint algorithm.)</span></span>

<span data-ttu-id="5ac54-150">Teď, když byly aktualizovány definice služby a služby konfigurační soubory, balíček nasazení nahrát do Azure.</span><span class="sxs-lookup"><span data-stu-id="5ac54-150">Now that the service definition and service configuration files have been updated, package your deployment for uploading to Azure.</span></span> <span data-ttu-id="5ac54-151">Pokud používáte **cspack**, nepoužívejte **/generateConfigurationFile** příznak, jako který přepíše vloženého informace o certifikátu.</span><span class="sxs-lookup"><span data-stu-id="5ac54-151">If you are using **cspack**, don't use the **/generateConfigurationFile** flag, as that will overwrite the certificate information you just inserted.</span></span>

## <a name="step-3-upload-a-certificate"></a><span data-ttu-id="5ac54-152">Krok 3: Nahrát na server certifikát</span><span class="sxs-lookup"><span data-stu-id="5ac54-152">Step 3: Upload a certificate</span></span>
<span data-ttu-id="5ac54-153">Připojení k portálu Azure a...</span><span class="sxs-lookup"><span data-stu-id="5ac54-153">Connect to the Azure portal and...</span></span>

1. <span data-ttu-id="5ac54-154">V **všechny prostředky** části portálu, vyberte cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="5ac54-154">In the **All resources** section of the Portal, select your cloud service.</span></span>

    ![Publikování cloudové služby](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. <span data-ttu-id="5ac54-156">Klikněte na tlačítko **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="5ac54-156">Click **Certificates**.</span></span>

    ![Klikněte na ikonu certifikáty](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. <span data-ttu-id="5ac54-158">Klikněte na tlačítko **nahrát** v horní části oblasti certifikáty.</span><span class="sxs-lookup"><span data-stu-id="5ac54-158">Click **Upload** at the top of the certificates area.</span></span>

    ![Klikněte na položku nabídky nahrávání](media/cloud-services-configure-ssl-certificate-portal/Upload_menu.png)

4. <span data-ttu-id="5ac54-160">Zadejte **soubor**, **heslo**, pak klikněte na tlačítko **nahrát** v dolní části oblasti vstupní data.</span><span class="sxs-lookup"><span data-stu-id="5ac54-160">Provide the **File**, **Password**, then click **Upload** at the bottom of the data entry area.</span></span>

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a><span data-ttu-id="5ac54-161">Krok 4: Připojení k instanci role pomocí protokolu HTTPS</span><span class="sxs-lookup"><span data-stu-id="5ac54-161">Step 4: Connect to the role instance by using HTTPS</span></span>
<span data-ttu-id="5ac54-162">Teď, když vaše nasazení je spuštěná v Azure, můžete připojit se pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5ac54-162">Now that your deployment is up and running in Azure, you can connect to it using HTTPS.</span></span>

1. <span data-ttu-id="5ac54-163">Klikněte **adresa URL webu** otevřete webový prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="5ac54-163">Click the **Site URL** to open up the web browser.</span></span>

   ![Klikněte na adresu URL webu](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2. <span data-ttu-id="5ac54-165">Ve webovém prohlížeči, upravte odkaz použít **https** místo **http**a potom navštivte stránku.</span><span class="sxs-lookup"><span data-stu-id="5ac54-165">In your web browser, modify the link to use **https** instead of **http**, and then visit the page.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5ac54-166">Pokud používáte certifikát podepsaný svým držitelem, když přejdete na koncový bod HTTPS, který je spojen s certifikát podepsaný svým držitelem může se zobrazit chyba certifikátu v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="5ac54-166">If you are using a self-signed certificate, when you browse to an HTTPS endpoint that's associated with the self-signed certificate you may see a certificate error in the browser.</span></span> <span data-ttu-id="5ac54-167">Používá certifikát podepsaný důvěryhodnou certifikační autoritou eliminuje tento problém; do té doby můžete chybu ignorovat.</span><span class="sxs-lookup"><span data-stu-id="5ac54-167">Using a certificate signed by a trusted certification authority eliminates this problem; in the meantime, you can ignore the error.</span></span> <span data-ttu-id="5ac54-168">(Další možností je přidání certifikátu podepsaného svým držitelem do úložiště certifikátů důvěryhodné certifikační autority uživatele.)</span><span class="sxs-lookup"><span data-stu-id="5ac54-168">(Another option is to add the self-signed certificate to the user's trusted certificate authority certificate store.)</span></span>
   >
   >

   ![Verze preview webu](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

   > [!TIP]
   > <span data-ttu-id="5ac54-170">Pokud chcete používat protokol SSL pro pracovní nasazení místo produkční nasazení, musíte nejdřív určit adresu URL pro pracovní nasazení použít.</span><span class="sxs-lookup"><span data-stu-id="5ac54-170">If you want to use SSL for a staging deployment instead of a production deployment, you'll first need to determine the URL used for the staging deployment.</span></span> <span data-ttu-id="5ac54-171">Jakmile Cloudová služba byla nasazena, je dáno adresu URL na pracovní prostředí **ID nasazení** identifikátor GUID v tomto formátu:`https://deployment-id.cloudapp.net/`</span><span class="sxs-lookup"><span data-stu-id="5ac54-171">Once your cloud service has been deployed, the URL to the staging environment is determined by the **Deployment ID** GUID in this format: `https://deployment-id.cloudapp.net/`</span></span>  
   >
   > <span data-ttu-id="5ac54-172">Vytvoření certifikátu pomocí běžný název (CN) rovnou na adresu URL, na základě GUID (například **328187776e774ceda8fc57609d404462.cloudapp.net**).</span><span class="sxs-lookup"><span data-stu-id="5ac54-172">Create a certificate with the common name (CN) equal to the GUID-based URL (for example, **328187776e774ceda8fc57609d404462.cloudapp.net**).</span></span> <span data-ttu-id="5ac54-173">Použití portálu k přidání certifikátu do dvoufázové instalace cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="5ac54-173">Use the portal to add the certificate to your staged cloud service.</span></span> <span data-ttu-id="5ac54-174">Potom CSDEF a CSCFG souborům přidat informace o certifikátu, znovu zabalte aplikaci a aktualizaci dvoufázové nasazení tak, aby pomocí nového balíčku.</span><span class="sxs-lookup"><span data-stu-id="5ac54-174">Then, add the certificate information to your CSDEF and CSCFG files, repackage your application, and update your staged deployment to use the new package.</span></span>
   >

## <a name="next-steps"></a><span data-ttu-id="5ac54-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5ac54-175">Next steps</span></span>
* <span data-ttu-id="5ac54-176">[Obecná konfigurace cloudové služby](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5ac54-176">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="5ac54-177">Zjistěte, jak [nasazení cloudové služby](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5ac54-177">Learn how to [deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="5ac54-178">Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5ac54-178">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="5ac54-179">[Správa služby cloud](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5ac54-179">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
