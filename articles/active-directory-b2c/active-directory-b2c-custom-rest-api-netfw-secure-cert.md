---
title: "Azure Active Directory B2C: Zabezpečení služby RESTful pomocí klientských certifikátů"
description: "Zabezpečit vaše vlastní výměnu REST API deklarace identity ve vašem Azure AD B2C pomocí klientských certifikátů"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 09/25/2017
ms.author: yoelh
ms.openlocfilehash: 582aadd35821779e307ac285804e3b7fe5c24abd
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="secure-your-restful-service-by-using-client-certificates"></a><span data-ttu-id="1c151-103">Zabezpečení služby RESTful pomocí klientských certifikátů</span><span class="sxs-lookup"><span data-stu-id="1c151-103">Secure your RESTful service by using client certificates</span></span>
<span data-ttu-id="1c151-104">V souvisejícím článku jste [vytvoření RESTful služby](active-directory-b2c-custom-rest-api-netfw.md) který komunikuje se službou Azure Active Directory B2C (Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="1c151-104">In a related article, you [create a RESTful service](active-directory-b2c-custom-rest-api-netfw.md) that interacts with Azure Active Directory B2C (Azure AD B2C).</span></span>

<span data-ttu-id="1c151-105">V tomto článku zjistěte, jak omezit přístup k vaší webové aplikace Azure (RESTful API) pomocí certifikátu klienta.</span><span class="sxs-lookup"><span data-stu-id="1c151-105">In this article, you learn how to restrict access to your Azure web app (RESTful API) by using a client certificate.</span></span> <span data-ttu-id="1c151-106">Tento mechanismus se nazývá vzájemné ověřování TLS, nebo *ověřování certifikátu klienta*.</span><span class="sxs-lookup"><span data-stu-id="1c151-106">This mechanism is called TLS mutual authentication, or *client certificate authentication*.</span></span> <span data-ttu-id="1c151-107">Pouze služby, které mají správnou certifikáty, jako je například Azure AD B2C, můžete přístup ke službě.</span><span class="sxs-lookup"><span data-stu-id="1c151-107">Only services that have proper certificates, such as Azure AD B2C, can access your service.</span></span>

>[!NOTE]
><span data-ttu-id="1c151-108">Můžete také zabezpečení služby RESTful pomocí [základní ověřování protokolu HTTP](active-directory-b2c-custom-rest-api-netfw-secure-basic.md).</span><span class="sxs-lookup"><span data-stu-id="1c151-108">You can also secure your RESTful service by using [HTTP basic authentication](active-directory-b2c-custom-rest-api-netfw-secure-basic.md).</span></span> <span data-ttu-id="1c151-109">Základní ověřování protokolu HTTP se považuje však méně bezpečná přes klientský certifikát.</span><span class="sxs-lookup"><span data-stu-id="1c151-109">However, HTTP basic authentication is considered less secure over a client certificate.</span></span> <span data-ttu-id="1c151-110">Naše doporučení je služba RESTful zabezpečit pomocí ověření klientského certifikátu, jak je popsáno v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="1c151-110">Our recommendation is to secure the RESTful service by using client certificate authentication as described in this article.</span></span>

<span data-ttu-id="1c151-111">Tento článek podrobnosti o tom, jak:</span><span class="sxs-lookup"><span data-stu-id="1c151-111">This article details how to:</span></span>
* <span data-ttu-id="1c151-112">Nastavení webové aplikace k použití ověřování pomocí certifikátu klienta.</span><span class="sxs-lookup"><span data-stu-id="1c151-112">Set up your web app to use client certificate authentication.</span></span>
* <span data-ttu-id="1c151-113">Nahrajte certifikát do Azure AD B2C zásad klíče.</span><span class="sxs-lookup"><span data-stu-id="1c151-113">Upload the certificate to Azure AD B2C policy keys.</span></span>
* <span data-ttu-id="1c151-114">Nakonfigurujte vaše vlastní zásady pro použití certifikátu klienta.</span><span class="sxs-lookup"><span data-stu-id="1c151-114">Configure your custom policy to use the client certificate.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1c151-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1c151-115">Prerequisites</span></span>
* <span data-ttu-id="1c151-116">Proveďte kroky v [integrovat REST API deklarací výměnu](active-directory-b2c-custom-rest-api-netfw.md) článku.</span><span class="sxs-lookup"><span data-stu-id="1c151-116">Complete the steps in the [Integrate REST API claims exchanges](active-directory-b2c-custom-rest-api-netfw.md) article.</span></span>
* <span data-ttu-id="1c151-117">Získáte platný certifikát (soubor .pfx s privátní klíč).</span><span class="sxs-lookup"><span data-stu-id="1c151-117">Get a valid certificate (a .pfx file with a private key).</span></span>

## <a name="step-1-configure-a-web-app-for-client-certificate-authentication"></a><span data-ttu-id="1c151-118">Krok 1: Konfigurace webové aplikace pro ověření certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="1c151-118">Step 1: Configure a web app for client certificate authentication</span></span>
<span data-ttu-id="1c151-119">Nastavit **Azure App Service** vyžadování klientských certifikátů, nastavení webové aplikace `clientCertEnabled` lokality nastavení *true*.</span><span class="sxs-lookup"><span data-stu-id="1c151-119">To set up **Azure App Service** to require client certificates, set the web app `clientCertEnabled` site setting to *true*.</span></span> <span data-ttu-id="1c151-120">Chcete-li tuto změnu, musíte použít rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="1c151-120">To make this change, you must use the REST API.</span></span> <span data-ttu-id="1c151-121">Toto nastavení je k dispozici prostřednictvím prostředí pro správu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1c151-121">The setting is available through the management experience in the Azure portal.</span></span> <span data-ttu-id="1c151-122">Najít nastavení, v aplikaci RESTful **nastavení** nabídce v části **nástroje pro vývoj**, vyberte **Průzkumníka prostředků**.</span><span class="sxs-lookup"><span data-stu-id="1c151-122">To locate the setting, on your RESTful application's **Settings** menu, under **Development tools**, select **Resource Explorer**.</span></span>

>[!NOTE]
><span data-ttu-id="1c151-123">Ujistěte se, že plán služby Azure App Service je standardní nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="1c151-123">Make sure that your Azure App Service plan is Standard or greater.</span></span> <span data-ttu-id="1c151-124">Další informace najdete v tématu [podrobný přehled plánů služby Azure App Service](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="1c151-124">For more information, see [Azure App Service plans in-depth overview](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>


<span data-ttu-id="1c151-125">Použití [Průzkumníka prostředků Azure (Preview)](https://resources.azure.com) nastavit **clientCertEnabled** vlastnost *true*, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="1c151-125">Use [Azure Resource Explorer (Preview)](https://resources.azure.com) to set the **clientCertEnabled** property to *true*, as shown in the following image:</span></span>

![Nastavení clientCertEnabled pomocí Průzkumníka prostředků Azure](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-cert-resource-explorer.png)

>[!NOTE]
><span data-ttu-id="1c151-127">Další informace o nastavení **clientCertEnabled** vlastnost, najdete v části [konfigurace TLS vzájemné ověřování pro webové aplikace](https://docs.microsoft.com/azure/app-service-web/app-service-web-configure-tls-mutual-auth).</span><span class="sxs-lookup"><span data-stu-id="1c151-127">For more information about setting the **clientCertEnabled** property, see [Configure TLS mutual authentication for web apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-configure-tls-mutual-auth).</span></span>

>[!TIP]
><span data-ttu-id="1c151-128">Aby bylo snazší vytvořit volání rozhraní REST API služby, případně můžete použít [ARMClient](https://github.com/projectkudu/ARMClient) nástroj.</span><span class="sxs-lookup"><span data-stu-id="1c151-128">Alternatively, to make it easier to craft the REST API call, you can use the [ARMClient](https://github.com/projectkudu/ARMClient) tool.</span></span>

## <a name="step-2-upload-your-certificate-to-azure-ad-b2c-policy-keys"></a><span data-ttu-id="1c151-129">Krok 2: Nahrajte certifikát do Azure AD B2C zásad klíče</span><span class="sxs-lookup"><span data-stu-id="1c151-129">Step 2: Upload your certificate to Azure AD B2C policy keys</span></span>
<span data-ttu-id="1c151-130">Po nastavení `clientCertEnabled` k *true*, komunikaci se službou rozhraní RESTful API, vyžaduje klientský certifikát.</span><span class="sxs-lookup"><span data-stu-id="1c151-130">After you set `clientCertEnabled` to *true*, the communication with your RESTful API requires a client certificate.</span></span> <span data-ttu-id="1c151-131">Získat, odeslání a uložte certifikát klienta v klientovi služby Azure AD B2C, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="1c151-131">To obtain, upload, and store the client certificate in your Azure AD B2C tenant, do the following:</span></span> 
1. <span data-ttu-id="1c151-132">Ve vašem klientu Azure AD B2C, vyberte **nastavení B2C** > **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="1c151-132">In your Azure AD B2C tenant, select **B2C Settings** > **Identity Experience Framework**.</span></span>

2. <span data-ttu-id="1c151-133">Chcete-li zobrazit klíčů, které jsou k dispozici ve vašem klientovi, vyberte **zásad klíče**.</span><span class="sxs-lookup"><span data-stu-id="1c151-133">To view the keys that are available in your tenant, select **Policy Keys**.</span></span>

3. <span data-ttu-id="1c151-134">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="1c151-134">Select **Add**.</span></span>  
    <span data-ttu-id="1c151-135">**Vytvořte klíč** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="1c151-135">The **Create a key** window opens.</span></span>

4. <span data-ttu-id="1c151-136">V **možnosti** vyberte **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="1c151-136">In the **Options** box, select **Upload**.</span></span>

5. <span data-ttu-id="1c151-137">V **název** zadejte **B2cRestClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="1c151-137">In the **Name** box, type **B2cRestClientCertificate**.</span></span>  
    <span data-ttu-id="1c151-138">Předpona *B2C_1A_* je automaticky přidán.</span><span class="sxs-lookup"><span data-stu-id="1c151-138">The prefix *B2C_1A_* is added automatically.</span></span>

6. <span data-ttu-id="1c151-139">V **nahrávání souborů** , vyberte soubor .pfx vašeho certifikátu s privátním klíčem.</span><span class="sxs-lookup"><span data-stu-id="1c151-139">In the **File upload** box, select your certificate's .pfx file with a private key.</span></span>

7. <span data-ttu-id="1c151-140">V **heslo** zadejte heslo k certifikátu.</span><span class="sxs-lookup"><span data-stu-id="1c151-140">In the **Password** box, type the certificate's password.</span></span>

    ![Odešlete klíč zásad](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-cert-upload.png)

7. <span data-ttu-id="1c151-142">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1c151-142">Select **Create**.</span></span>

8. <span data-ttu-id="1c151-143">K zobrazení klíčů, které jsou k dispozici ve vašem klientovi a ověřte, že jste vytvořili `B2C_1A_B2cRestClientCertificate` klíč, vyberte **zásad klíče**.</span><span class="sxs-lookup"><span data-stu-id="1c151-143">To view the keys that are available in your tenant and confirm that you've created the `B2C_1A_B2cRestClientCertificate` key, select **Policy Keys**.</span></span>

## <a name="step-3-change-the-technical-profile"></a><span data-ttu-id="1c151-144">Krok 3: Změna technické profilu</span><span class="sxs-lookup"><span data-stu-id="1c151-144">Step 3: Change the technical profile</span></span>
<span data-ttu-id="1c151-145">Pro podporu ověřování certifikátu klienta ve vlastních zásadách, změna technické profil následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1c151-145">To support client certificate authentication in your custom policy, change the technical profile by doing the following:</span></span>

1. <span data-ttu-id="1c151-146">V pracovním adresáři, otevřete *TrustFrameworkExtensions.xml* soubor rozšíření zásad.</span><span class="sxs-lookup"><span data-stu-id="1c151-146">In your working directory, open the *TrustFrameworkExtensions.xml* extension policy file.</span></span>

2. <span data-ttu-id="1c151-147">Vyhledejte `<TechnicalProfile>` uzlu, který zahrnuje `Id="REST-API-SignUp"`.</span><span class="sxs-lookup"><span data-stu-id="1c151-147">Search for the `<TechnicalProfile>` node that includes `Id="REST-API-SignUp"`.</span></span>

3. <span data-ttu-id="1c151-148">Vyhledejte `<Metadata>` elementu.</span><span class="sxs-lookup"><span data-stu-id="1c151-148">Locate the `<Metadata>` element.</span></span>

4. <span data-ttu-id="1c151-149">Změna *AuthenticationType* k *ClientCertificate*, a to takto:</span><span class="sxs-lookup"><span data-stu-id="1c151-149">Change the *AuthenticationType* to *ClientCertificate*, as follows:</span></span>

    ```xml
    <Item Key="AuthenticationType">ClientCertificate</Item>
    ```

5. <span data-ttu-id="1c151-150">Ihned po zavření `<Metadata>` elementu, přidejte následující fragment kódu XML:</span><span class="sxs-lookup"><span data-stu-id="1c151-150">Immediately after the closing `<Metadata>` element, add the following XML snippet:</span></span> 

    ```xml
    <CryptographicKeys>
        <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_B2cRestClientCertificate" />
    </CryptographicKeys>
    ```

    <span data-ttu-id="1c151-151">Po přidání fragmentu váš technické profil by měl vypadat podobně jako následující kód XML:</span><span class="sxs-lookup"><span data-stu-id="1c151-151">After you add the snippet, your technical profile should look like the following XML code:</span></span>

    ![Nastavit ClientCertificate ověřování XML elementy](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-cert-tech-profile.png)

## <a name="step-4-upload-the-policy-to-your-tenant"></a><span data-ttu-id="1c151-153">Krok 4: Nahrání zásady klienta</span><span class="sxs-lookup"><span data-stu-id="1c151-153">Step 4: Upload the policy to your tenant</span></span>

1. <span data-ttu-id="1c151-154">V [portál Azure](https://portal.azure.com), přepnout [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a potom vyberte **Azure AD B2C**.</span><span class="sxs-lookup"><span data-stu-id="1c151-154">In the [Azure portal](https://portal.azure.com), switch to the [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and then select **Azure AD B2C**.</span></span>

2. <span data-ttu-id="1c151-155">Vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="1c151-155">Select **Identity Experience Framework**.</span></span>

3. <span data-ttu-id="1c151-156">Vyberte **všechny zásady**.</span><span class="sxs-lookup"><span data-stu-id="1c151-156">Select **All Policies**.</span></span>

4. <span data-ttu-id="1c151-157">Vyberte **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="1c151-157">Select **Upload Policy**.</span></span>

5. <span data-ttu-id="1c151-158">Vyberte **přepsat zásady, pokud existuje** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="1c151-158">Select the **Overwrite the policy if it exists** check box.</span></span>

6. <span data-ttu-id="1c151-159">Nahrát *TrustFrameworkExtensions.xml* souboru a pak se ujistěte, že předává ověření.</span><span class="sxs-lookup"><span data-stu-id="1c151-159">Upload the *TrustFrameworkExtensions.xml* file, and then ensure that it passes validation.</span></span>

## <a name="step-5-test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="1c151-160">Krok 5: Testování spustit pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="1c151-160">Step 5: Test the custom policy by using Run Now</span></span>
1. <span data-ttu-id="1c151-161">Otevřete **nastavení Azure AD B2C**a potom vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="1c151-161">Open **Azure AD B2C Settings**, and then select **Identity Experience Framework**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="1c151-162">Spustit nyní vyžaduje alespoň jedné aplikace do být preregistered u klienta.</span><span class="sxs-lookup"><span data-stu-id="1c151-162">Run Now requires at least one application to be preregistered on the tenant.</span></span> <span data-ttu-id="1c151-163">Další postup registrace aplikace najdete v tématu Azure AD B2C [Začínáme](active-directory-b2c-get-started.md) článek nebo [registrace aplikace](active-directory-b2c-app-registration.md) článku.</span><span class="sxs-lookup"><span data-stu-id="1c151-163">To learn how to register applications, see the Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or the [Application registration](active-directory-b2c-app-registration.md) article.</span></span>

2. <span data-ttu-id="1c151-164">Otevřete **B2C_1A_signup_signin**, předávající stranu vlastních zásad, které jste nahráli a potom vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="1c151-164">Open **B2C_1A_signup_signin**, the relying party (RP) custom policy that you uploaded, and then select **Run now**.</span></span>

3. <span data-ttu-id="1c151-165">Otestujte proces zadáním **Test** v **křestní jméno** pole.</span><span class="sxs-lookup"><span data-stu-id="1c151-165">Test the process by typing **Test** in the **Given Name** box.</span></span>  
    <span data-ttu-id="1c151-166">Azure AD B2C zobrazí chybovou zprávu v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="1c151-166">Azure AD B2C displays an error message at the top of the window.</span></span>    

    ![Testování vaší identity rozhraní API](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-test.png)

4. <span data-ttu-id="1c151-168">V **křestní jméno** zadejte název (jiné než "Test").</span><span class="sxs-lookup"><span data-stu-id="1c151-168">In the **Given Name** box, type a name (other than "Test").</span></span>  
    <span data-ttu-id="1c151-169">Azure AD B2C zaregistruje uživatele a pak odešle číslo věrného do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c151-169">Azure AD B2C signs up the user and then sends a loyalty number to your application.</span></span> <span data-ttu-id="1c151-170">Poznamenejte si číslo v tomto příkladu JWT:</span><span class="sxs-lookup"><span data-stu-id="1c151-170">Note the number in this JWT example:</span></span>

   ```
   {
     "typ": "JWT",
     "alg": "RS256",
     "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
   }.{
     "exp": 1507125903,
     "nbf": 1507122303,
     "ver": "1.0",
     "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
     "aud": "e1d2612f-c2bc-4599-8e7b-d874eaca1ee1",
     "acr": "b2c_1a_signup_signin",
     "nonce": "defaultNonce",
     "iat": 1507122303,
     "auth_time": 1507122303,
     "loyaltyNumber": "290",
     "given_name": "Emily",
     "emails": ["B2cdemo@outlook.com"]
   }
   ```

   >[!NOTE]
   ><span data-ttu-id="1c151-171">Pokud se zobrazí chybová zpráva *název není platný, zadejte platný název*, znamená to, že Azure AD B2C úspěšně volat služby RESTful, dokud se zobrazí certifikát klienta.</span><span class="sxs-lookup"><span data-stu-id="1c151-171">If you receive the error message, *The name is not valid, please provide a valid name*, it means that Azure AD B2C successfully called your RESTful service while it presented the client certificate.</span></span> <span data-ttu-id="1c151-172">Dalším krokem je ověřit certifikát.</span><span class="sxs-lookup"><span data-stu-id="1c151-172">The next step is to validate the certificate.</span></span>

## <a name="step-6-add-certificate-validation"></a><span data-ttu-id="1c151-173">Krok 6: Přidání ověření certifikátu</span><span class="sxs-lookup"><span data-stu-id="1c151-173">Step 6: Add certificate validation</span></span>
<span data-ttu-id="1c151-174">Certifikát klienta, který Azure AD B2C odešle do služby RESTful nepodléhají ověření platformou Azure Web Apps, s výjimkou a zkontrolujte, zda certifikát existuje.</span><span class="sxs-lookup"><span data-stu-id="1c151-174">The client certificate that Azure AD B2C sends to your RESTful service does not undergo validation by the Azure Web Apps platform, except to check whether the certificate exists.</span></span> <span data-ttu-id="1c151-175">Ověřování certifikátu odpovídá webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c151-175">Validating the certificate is the responsibility of the web app.</span></span> 

<span data-ttu-id="1c151-176">V této části přidejte ukázkový kód ASP.NET, která ověřuje vlastnosti certifikátu pro účely ověření.</span><span class="sxs-lookup"><span data-stu-id="1c151-176">In this section, you add sample ASP.NET code that validates the certificate properties for authentication purposes.</span></span>

> [!NOTE]
><span data-ttu-id="1c151-177">Další informace o konfiguraci Azure App Service pro ověření certifikátu klienta najdete v tématu [konfigurace TLS vzájemné ověřování pro webové aplikace](https://docs.microsoft.com/azure/app-service-web/app-service-web-configure-tls-mutual-auth).</span><span class="sxs-lookup"><span data-stu-id="1c151-177">For more information about configuring Azure App Service for client certificate authentication, see [Configure TLS mutual authentication for web apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-configure-tls-mutual-auth).</span></span>

### <a name="61-add-application-settings-to-your-projects-webconfig-file"></a><span data-ttu-id="1c151-178">6.1 přidáte nastavení aplikace do souboru web.config vašeho projektu</span><span class="sxs-lookup"><span data-stu-id="1c151-178">6.1 Add application settings to your project's web.config file</span></span>
<span data-ttu-id="1c151-179">V sadě Visual Studio projekt, který jste vytvořili dříve, přidejte následující nastavení aplikace, které *web.config* souboru po `appSettings` element:</span><span class="sxs-lookup"><span data-stu-id="1c151-179">In the Visual Studio project that you created earlier, add the following application settings to the *web.config* file after the `appSettings` element:</span></span>

```XML
<add key="ClientCertificate:Subject" value="CN=Subject name" />
<add key="ClientCertificate:Issuer" value="CN=Issuer name" />
<add key="ClientCertificate:Thumbprint" value="Certificate thumbprint" />
```

<span data-ttu-id="1c151-180">Nahraďte tento certifikát **název subjektu**, **název vystavitele**, a **kryptografický otisk certifikátu** hodnoty hodnotami vašeho certifikátu.</span><span class="sxs-lookup"><span data-stu-id="1c151-180">Replace the certificate's **Subject name**, **Issuer name**, and **Certificate thumbprint** values with your certificate values.</span></span>

### <a name="62-add-the-isvalidclientcertificate-function"></a><span data-ttu-id="1c151-181">6.2 přidat funkci IsValidClientCertificate</span><span class="sxs-lookup"><span data-stu-id="1c151-181">6.2 Add the IsValidClientCertificate function</span></span>
<span data-ttu-id="1c151-182">Otevřete *Controllers\IdentityController.cs* souboru a pak přidejte do `Identity` řadiče třídy následující funkce:</span><span class="sxs-lookup"><span data-stu-id="1c151-182">Open the *Controllers\IdentityController.cs* file, and then add to the `Identity` controller class the following function:</span></span> 

```csharp
private bool IsValidClientCertificate()
{
    string ClientCertificateSubject = ConfigurationManager.AppSettings["ClientCertificate:Subject"];
    string ClientCertificateIssuer = ConfigurationManager.AppSettings["ClientCertificate:Issuer"];
    string ClientCertificateThumbprint = ConfigurationManager.AppSettings["ClientCertificate:Thumbprint"];

    X509Certificate2 clientCertInRequest = RequestContext.ClientCertificate;
    if (clientCertInRequest == null)
    {
        Trace.WriteLine("Certificate is NULL");
        return false;
    }

    // Basic verification
    if (clientCertInRequest.Verify() == false)
    {
        Trace.TraceError("Basic verification failed");
        return false;
    }

    // 1. Check the time validity of the certificate
    if (DateTime.Compare(DateTime.Now, clientCertInRequest.NotBefore) < 0 ||
        DateTime.Compare(DateTime.Now, clientCertInRequest.NotAfter) > 0)
    {
        Trace.TraceError($"NotBefore '{clientCertInRequest.NotBefore}' or NotAfter '{clientCertInRequest.NotAfter}' not valid");
        return false;
    }

    // 2. Check the subject name of the certificate
    bool foundSubject = false;
    string[] certSubjectData = clientCertInRequest.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
    foreach (string s in certSubjectData)
    {
        if (String.Compare(s.Trim(), ClientCertificateSubject) == 0)
        {
            foundSubject = true;
            break;
        }
    }

    if (!foundSubject)
    {
        Trace.TraceError($"Subject name '{clientCertInRequest.Subject}' is not valid");
        return false;
    }
    
    // 3. Check the issuer name of the certificate
    bool foundIssuerCN = false;
    string[] certIssuerData = clientCertInRequest.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
    foreach (string s in certIssuerData)
    {
        if (String.Compare(s.Trim(), ClientCertificateIssuer) == 0)
        {
            foundIssuerCN = true;
            break;
        }
    }

    if (!foundIssuerCN)
    {
        Trace.TraceError($"Issuer '{clientCertInRequest.Issuer}' is not valid");
        return false;
    }

    // 4. Check the thumbprint of the certificate
    if (String.Compare(clientCertInRequest.Thumbprint.Trim().ToUpper(), ClientCertificateThumbprint) != 0)
    {
        Trace.TraceError($"Thumbprint '{clientCertInRequest.Thumbprint.Trim().ToUpper()}' is not valid");
        return false;
    }

    // 5. If you also want to test whether the certificate chains to a trusted root authority, you can uncomment the following code:
    //
    //X509Chain certChain = new X509Chain();
    //certChain.Build(certificate);
    //bool isValidCertChain = true;
    //foreach (X509ChainElement chElement in certChain.ChainElements)
    //{
    //    if (!chElement.Certificate.Verify())
    //    {
    //        isValidCertChain = false;
    //        break;
    //    }
    //}
    //if (!isValidCertChain) return false;
    return true;
}
```

<span data-ttu-id="1c151-183">V předchozím ukázkovém kódu můžeme přijmout si certifikát platný, pouze pokud jsou splněny všechny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="1c151-183">In the preceding sample code, we accept the certificate as valid only if all the following conditions are met:</span></span>
* <span data-ttu-id="1c151-184">Certifikát není vypršela a je aktivní pro aktuální čas na serveru.</span><span class="sxs-lookup"><span data-stu-id="1c151-184">The certificate is not expired and is active for the current time on server.</span></span>
* <span data-ttu-id="1c151-185">`Subject` Název certifikátu je rovno `ClientCertificate:Subject` hodnota nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c151-185">The `Subject` name of the certificate is equal to the `ClientCertificate:Subject` application setting value.</span></span>
* <span data-ttu-id="1c151-186">`Issuer` Název certifikátu je rovno `ClientCertificate:Issuer` hodnota nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c151-186">The `Issuer` name of the certificate is equal to the `ClientCertificate:Issuer` application setting value.</span></span>
* <span data-ttu-id="1c151-187">`thumbprint` Certifikátu je rovno `ClientCertificate:Thumbprint` hodnota nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c151-187">The `thumbprint` of the certificate is equal to the `ClientCertificate:Thumbprint` application setting value.</span></span>

>[!IMPORTANT]
><span data-ttu-id="1c151-188">V závislosti na velkých a malých písmen služby může být nutné přidat další ověření.</span><span class="sxs-lookup"><span data-stu-id="1c151-188">Depending on the sensitivity of your service, you might need to add more validations.</span></span> <span data-ttu-id="1c151-189">Například může být potřeba otestovat, zda certifikát je zřetězený k důvěryhodné kořenové autority, Vystavitel ověření názvu organizace a tak dále.</span><span class="sxs-lookup"><span data-stu-id="1c151-189">For example, you might need to test whether the certificate chains to a trusted root authority, issuer organization name validation, and so on.</span></span>

### <a name="63-call-the-isvalidclientcertificate-function"></a><span data-ttu-id="1c151-190">6.3 volání funkce IsValidClientCertificate</span><span class="sxs-lookup"><span data-stu-id="1c151-190">6.3 Call the IsValidClientCertificate function</span></span>
<span data-ttu-id="1c151-191">Otevřete *Controllers\IdentityController.cs* souboru a potom na začátku `SignUp()` fungovat, přidejte následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="1c151-191">Open the *Controllers\IdentityController.cs* file and then, at the beginning of the `SignUp()` function, add the following code snippet:</span></span> 

```csharp
if (IsValidClientCertificate() == false)
{
    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Your client certificate is not valid", HttpStatusCode.Conflict));
}
```

<span data-ttu-id="1c151-192">Po přidání fragmentu, vaše `Identity` řadič by měl vypadat jako následující kód:</span><span class="sxs-lookup"><span data-stu-id="1c151-192">After you add the snippet, your `Identity` controller should look like the following code:</span></span>

![Přidat kód pro ověření certifikátu](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-code.png)

## <a name="step-7-publish-your-project-to-azure-and-test-it"></a><span data-ttu-id="1c151-194">Krok 7: Publikování projektu v Azure a otestovat ji</span><span class="sxs-lookup"><span data-stu-id="1c151-194">Step 7: Publish your project to Azure and test it</span></span>
1. <span data-ttu-id="1c151-195">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **Contoso.AADB2C.API** projektu a potom vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="1c151-195">In **Solution Explorer**, right-click the **Contoso.AADB2C.API** project, and then select **Publish**.</span></span>

2. <span data-ttu-id="1c151-196">Opakujte "Krok 6" a znovu otestovat vaše vlastní zásada s ověření certifikátu.</span><span class="sxs-lookup"><span data-stu-id="1c151-196">Repeat "Step 6," and re-test your custom policy with the certificate validation.</span></span> <span data-ttu-id="1c151-197">Pokuste se spustit zásady a ujistěte se, že vše funguje po přidání ověření.</span><span class="sxs-lookup"><span data-stu-id="1c151-197">Try to run the policy, and make sure that everything works after you add the validation.</span></span>

3. <span data-ttu-id="1c151-198">Ve vaší *web.config* souboru, změňte hodnotu `ClientCertificate:Subject` k **neplatný**.</span><span class="sxs-lookup"><span data-stu-id="1c151-198">In your *web.config* file, change the value of `ClientCertificate:Subject` to **invalid**.</span></span> <span data-ttu-id="1c151-199">Znovu spusťte zásady a měli byste vidět chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="1c151-199">Run the policy again and you should see an error message.</span></span>

4. <span data-ttu-id="1c151-200">Změnit hodnotu zpět do **platný**a ujistěte se, že zásady můžete volat rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="1c151-200">Change the value back to **valid**, and make sure that the policy can call your REST API.</span></span>

<span data-ttu-id="1c151-201">Pokud potřebujete, chcete-li vyřešit tento krok, přečtěte si [shromažďování protokolů pomocí Application Insights](active-directory-b2c-troubleshoot-custom.md).</span><span class="sxs-lookup"><span data-stu-id="1c151-201">If you need to troubleshoot this step, see [Collecting logs by using Application Insights](active-directory-b2c-troubleshoot-custom.md).</span></span>

## <a name="optional-download-the-complete-policy-files-and-code"></a><span data-ttu-id="1c151-202">(Volitelné) Stáhnout soubory dokončení zásad a kódu</span><span class="sxs-lookup"><span data-stu-id="1c151-202">(Optional) Download the complete policy files and code</span></span>
* <span data-ttu-id="1c151-203">Po dokončení [začít pracovat s vlastními zásadami](active-directory-b2c-get-started-custom.md) návod, doporučujeme vám vytvořit váš scénář pomocí vlastních zásad pro soubory.</span><span class="sxs-lookup"><span data-stu-id="1c151-203">After you complete the [Get started with custom policies](active-directory-b2c-get-started-custom.md) walkthrough, we recommend that you build your scenario by using your own custom policy files.</span></span> <span data-ttu-id="1c151-204">Pro vaši informaci uvádíme [ukázkové soubory zásad](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw-secure-cert).</span><span class="sxs-lookup"><span data-stu-id="1c151-204">For your reference, we have provided [Sample policy files](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw-secure-cert).</span></span>
* <span data-ttu-id="1c151-205">Si můžete stáhnout kompletní kód z [řešení sady Visual Studio ukázkový pro referenci](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/Contoso.AADB2C.API).</span><span class="sxs-lookup"><span data-stu-id="1c151-205">You can download the complete code from [Sample Visual Studio solution for reference](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/Contoso.AADB2C.API).</span></span> 
