---
title: "aaaUse Azure Key Vault z webové aplikace | Microsoft Docs"
description: "Použijte tento kurz toohelp zjistíte, jak toouse Azure Key Vault z webové aplikace."
services: key-vault
documentationcenter: 
author: adhurwit
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: adhurwit
ms.openlocfilehash: d5e2299e60b379c4e234d5cd6be03411c5a5c958
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-key-vault-from-a-web-application"></a><span data-ttu-id="779d1-103">Použití Azure Key Vault z webové aplikace</span><span class="sxs-lookup"><span data-stu-id="779d1-103">Use Azure Key Vault from a Web Application</span></span>
## <a name="introduction"></a><span data-ttu-id="779d1-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="779d1-104">Introduction</span></span>
<span data-ttu-id="779d1-105">Použijte tento kurz toohelp zjistíte, jak toouse Azure Key Vault z webové aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="779d1-105">Use this tutorial toohelp you learn how toouse Azure Key Vault from a web application in Azure.</span></span> <span data-ttu-id="779d1-106">Provede vás procesem hello přístup k tajného klíče z Azure Key Vault, takže je možné ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="779d1-106">It walks you through hello process of accessing a secret from an Azure Key Vault so that it can be used in your web application.</span></span>

<span data-ttu-id="779d1-107">**Odhadovaný čas toocomplete:** 15 minut</span><span class="sxs-lookup"><span data-stu-id="779d1-107">**Estimated time toocomplete:** 15 minutes</span></span>

<span data-ttu-id="779d1-108">Souhrnné informace o Azure Key Vault naleznete v tématu [Co je Azure Key Vault?](key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="779d1-108">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="779d1-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="779d1-109">Prerequisites</span></span>
<span data-ttu-id="779d1-110">toocomplete tento kurz, musíte mít hello následující:</span><span class="sxs-lookup"><span data-stu-id="779d1-110">toocomplete this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="779d1-111">Identifikátor URI tajného klíče tooa v Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="779d1-111">A URI tooa secret in an Azure Key Vault</span></span>
* <span data-ttu-id="779d1-112">ID klienta a tajný klíč klienta pro webovou aplikaci, které jsou zaregistrované v Azure Active Directory, který má přístup tooyour Key Vault</span><span class="sxs-lookup"><span data-stu-id="779d1-112">A Client ID and a Client Secret for a web application registered with Azure Active Directory that has access tooyour Key Vault</span></span>
* <span data-ttu-id="779d1-113">Webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="779d1-113">A web application.</span></span> <span data-ttu-id="779d1-114">Jsme se, že se zobrazuje hello kroky pro aplikaci ASP.NET MVC nasazené v Azure jako webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="779d1-114">We will be showing hello steps for an ASP.NET MVC application deployed in Azure as a Web App.</span></span>

> [!NOTE]
> <span data-ttu-id="779d1-115">Je nezbytné, že jste dokončili hello kroků uvedených v [Začínáme s Azure Key Vault](key-vault-get-started.md) pro tento kurz, který vám hello tooa identifikátor URI tajného klíče a hello ID klienta a tajný klíč klienta pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="779d1-115">It is essential that you have completed hello steps listed in [Get Started with Azure Key Vault](key-vault-get-started.md) for this tutorial so that you have hello URI tooa secret and hello Client ID and Client Secret for a web application.</span></span>
> 
> 

<span data-ttu-id="779d1-116">Hello webové aplikace, která bude mít přístup k hello Key Vault je hello jeden, který je zaregistrován ve službě Azure Active Directory a udělil přístup tooyour Key Vault.</span><span class="sxs-lookup"><span data-stu-id="779d1-116">hello web application that will be accessing hello Key Vault is hello one that is registered in Azure Active Directory and has been given access tooyour Key Vault.</span></span> <span data-ttu-id="779d1-117">Pokud to není hello případ, přejděte zpět tooRegister aplikace v kurzu Začínáme hello a opakujte kroky hello zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="779d1-117">If this is not hello case, go back tooRegister an Application in hello Get Started tutorial and repeat hello steps listed.</span></span>

<span data-ttu-id="779d1-118">Tento kurz je určen pro vývojářům webů, které pochopit základy hello vytváření webových aplikací v Azure.</span><span class="sxs-lookup"><span data-stu-id="779d1-118">This tutorial is designed for web developers that understand hello basics of creating web applications on Azure.</span></span> <span data-ttu-id="779d1-119">Další informace o službě Azure Web Apps, naleznete v části [přehled Web Apps](../app-service-web/app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="779d1-119">For more information about Azure Web Apps, see [Web Apps overview](../app-service-web/app-service-web-overview.md).</span></span>

## <span data-ttu-id="779d1-120"><a id="packages"></a>Přidání balíčků Nuget</span><span class="sxs-lookup"><span data-stu-id="779d1-120"><a id="packages"></a>Add Nuget Packages</span></span>
<span data-ttu-id="779d1-121">Jsou dva balíčky, které webové aplikace potřebuje toohave nainstalována.</span><span class="sxs-lookup"><span data-stu-id="779d1-121">There are two packages that your web application needs toohave installed.</span></span>

* <span data-ttu-id="779d1-122">Knihovna ověřování Active Directory - obsahuje metody pro interakci s Azure Active Directory a správa identity uživatele</span><span class="sxs-lookup"><span data-stu-id="779d1-122">Active Directory Authentication Library - contains methods for interacting with Azure Active Directory and managing user identity</span></span>
* <span data-ttu-id="779d1-123">Azure Key Vault Library - obsahuje metody pro interakci s Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="779d1-123">Azure Key Vault Library - contains methods for interacting with Azure Key Vault</span></span>

<span data-ttu-id="779d1-124">Obě tyto balíčky můžete nainstalovat pomocí konzoly Správce balíčků pomocí příkazu hello Install-Package hello.</span><span class="sxs-lookup"><span data-stu-id="779d1-124">Both of these packages can be installed using hello Package Manager Console using hello Install-Package command.</span></span>

    // this is currently hello latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <span data-ttu-id="779d1-125"><a id="webconfig"></a>Upravit soubor Web.Config</span><span class="sxs-lookup"><span data-stu-id="779d1-125"><a id="webconfig"></a>Modify Web.Config</span></span>
<span data-ttu-id="779d1-126">Existují tři nastavení aplikace, které je třeba soubor web.config přidané toohello toobe následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="779d1-126">There are three application settings that need toobe added toohello web.config file as follows.</span></span>

    <!-- ClientId and ClientSecret refer toohello web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is hello URI for hello secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


<span data-ttu-id="779d1-127">Pokud ale nebudete toohost aplikace jako webové aplikace Azure, měli byste přidat hello skutečné ClientId, sdílený tajný klíč klienta a identifikátor URI tajného klíče hodnoty toohello web.config. V opačném případě ponechte tyto fiktivní hodnoty, protože jsme přidáním hello skutečnými hodnotami v hello portál Azure pro další úroveň zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="779d1-127">If you are not going toohost your application as an Azure Web App, then you should add hello actual ClientId, Client Secret, and Secret URI values toohello web.config. Otherwise leave these dummy values because we will be adding hello actual values in hello Azure Portal for an additional level of security.</span></span>

## <span data-ttu-id="779d1-128"><a id="gettoken"></a>Přidat metoda tooGet tokenu přístupu</span><span class="sxs-lookup"><span data-stu-id="779d1-128"><a id="gettoken"></a>Add Method tooGet an Access Token</span></span>
<span data-ttu-id="779d1-129">V pořadí toouse hello API trezoru klíč musíte přístupový token.</span><span class="sxs-lookup"><span data-stu-id="779d1-129">In order toouse hello Key Vault API you need an access token.</span></span> <span data-ttu-id="779d1-130">Hello klíč trezoru klienta zpracovává volání toohello trezoru klíč rozhraní API, ale můžete potřebovat toosupply její funkci, která se získá hello přístupový token.</span><span class="sxs-lookup"><span data-stu-id="779d1-130">hello Key Vault Client handles calls toohello Key Vault API but you need toosupply it with a function that gets hello access token.</span></span>  

<span data-ttu-id="779d1-131">Následuje hello kód tooget přístupový token ze služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="779d1-131">Following is hello code tooget an access token from Azure Active Directory.</span></span> <span data-ttu-id="779d1-132">Tento kód můžete přejít kdekoli v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="779d1-132">This code can go anywhere in your application.</span></span> <span data-ttu-id="779d1-133">I jako tooadd Utils nebo EncryptionHelper třídy.</span><span class="sxs-lookup"><span data-stu-id="779d1-133">I like tooadd a Utils or EncryptionHelper class.</span></span>  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property toohold hello secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //hello method that will be provided toohello KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed tooobtain hello JWT token");

        return result.AccessToken;
    }

> [!NOTE]
> <span data-ttu-id="779d1-134">Pomocí ID klienta a tajný klíč klienta je hello nejjednodušší způsob, jak tooauthenticate aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="779d1-134">Using a Client ID and Client Secret is hello easiest way tooauthenticate an Azure AD application.</span></span> <span data-ttu-id="779d1-135">A použití ve vaší webové aplikace je možné oddělení povinností a větší kontrolu nad vaší správy klíčů.</span><span class="sxs-lookup"><span data-stu-id="779d1-135">And using it in your web application allows for a separation of duties and more control over your key management.</span></span> <span data-ttu-id="779d1-136">Je však závislý na uvedení hello tajný klíč klienta v nastavení konfigurace, které pro některé můžou být jako rizikové jako uvedení hello tajný klíč, který má tooprotect v nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="779d1-136">But it does rely on putting hello Client Secret in your configuration settings which for some can be as risky as putting hello secret that you want tooprotect in your configuration settings.</span></span> <span data-ttu-id="779d1-137">Níže najdete podrobné informace o tom, jak toouse ID klienta a certifikát místo ID klienta a tajný klíč klienta tooauthenticate hello aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="779d1-137">See below for a discussion on how toouse a Client ID and Certificate instead of Client ID and Client Secret tooauthenticate hello Azure AD application.</span></span>
> 
> 

## <span data-ttu-id="779d1-138"><a id="appstart"></a>Načtení hello tajný klíč na spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="779d1-138"><a id="appstart"></a>Retrieve hello secret on Application Start</span></span>
<span data-ttu-id="779d1-139">Nyní jsme potřebovat code toocall hello trezoru klíč rozhraní API a načíst hello tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="779d1-139">Now we need code toocall hello Key Vault API and retrieve hello secret.</span></span> <span data-ttu-id="779d1-140">Hello následující kód můžou být přepnuté odkudkoli, dokud se označuje jako předtím, než je nutné toouse ho.</span><span class="sxs-lookup"><span data-stu-id="779d1-140">hello following code can be put anywhere as long as it is called before you need toouse it.</span></span> <span data-ttu-id="779d1-141">I tak, aby spustí jednou při spuštění a díky hello tajný klíč, které jsou k dispozici pro aplikace hello zavedla tento kód hello událost spustit aplikace v souboru Global.asax hello.</span><span class="sxs-lookup"><span data-stu-id="779d1-141">I have put this code in hello Application Start event in hello Global.asax so that it runs once on start and makes hello secret available for hello application.</span></span>

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

    //I put a variable in a Utils class toohold hello secret for general  application use.
    Utils.EncryptSecret = sec.Value;



## <span data-ttu-id="779d1-142"><a id="portalsettings"></a>Přidání nastavení aplikace v hello portálu Azure (volitelné)</span><span class="sxs-lookup"><span data-stu-id="779d1-142"><a id="portalsettings"></a>Add App Settings in hello Azure Portal (optional)</span></span>
<span data-ttu-id="779d1-143">Pokud máte webové aplikace Azure nyní můžete přidat hello skutečnými hodnotami pro hello AppSettings v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="779d1-143">If you have an Azure Web App you can now add hello actual values for hello AppSettings in hello Azure Portal.</span></span> <span data-ttu-id="779d1-144">Díky tomu skutečnými hodnotami hello nebude v souboru web.config hello ale chráněné prostřednictvím hello portál, kde můžete dělat samostatnou přístupovou ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="779d1-144">By doing this, hello actual values will not be in hello web.config but protected via hello Portal where you have separate access control capabilities.</span></span> <span data-ttu-id="779d1-145">Tyto hodnoty se nahradí hello hodnoty, které jste zadali v souboru web.config. Ujistěte se, že hello názvy jsou hello stejné.</span><span class="sxs-lookup"><span data-stu-id="779d1-145">These values will be substituted for hello values that you entered in your web.config. Make sure that hello names are hello same.</span></span>

![Nastavení aplikace se zobrazí na portálu Azure][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a><span data-ttu-id="779d1-147">Ověřování pomocí certifikátu místo tajný klíč klienta</span><span class="sxs-lookup"><span data-stu-id="779d1-147">Authenticate with a Certificate instead of a Client Secret</span></span>
<span data-ttu-id="779d1-148">Jiný způsob tooauthenticate aplikaci Azure AD je pomocí ID klienta a certifikát místo ID klienta a tajný klíč klienta.</span><span class="sxs-lookup"><span data-stu-id="779d1-148">Another way tooauthenticate an Azure AD application is by using a Client ID and a Certificate instead of a Client ID and Client Secret.</span></span> <span data-ttu-id="779d1-149">Následující jsou hello kroky toouse certifikát ve webové aplikaci Azure:</span><span class="sxs-lookup"><span data-stu-id="779d1-149">Following are hello steps toouse a Certificate in an Azure Web App:</span></span>

1. <span data-ttu-id="779d1-150">Získat nebo vytvořit certifikát</span><span class="sxs-lookup"><span data-stu-id="779d1-150">Get or Create a Certificate</span></span>
2. <span data-ttu-id="779d1-151">Hello certifikát přidružit aplikaci Azure AD</span><span class="sxs-lookup"><span data-stu-id="779d1-151">Associate hello Certificate with an Azure AD application</span></span>
3. <span data-ttu-id="779d1-152">Přidat kód tooyour webové aplikace toouse hello certifikátu</span><span class="sxs-lookup"><span data-stu-id="779d1-152">Add code tooyour Web App toouse hello Certificate</span></span>
4. <span data-ttu-id="779d1-153">Přidat certifikát tooyour webové aplikace</span><span class="sxs-lookup"><span data-stu-id="779d1-153">Add a Certificate tooyour Web App</span></span>

<span data-ttu-id="779d1-154">**Získejte nebo vytvořte certifikát** pro naše účely budeme testovacího certifikátu.</span><span class="sxs-lookup"><span data-stu-id="779d1-154">**Get or Create a Certificate** For our purposes we will make a test certificate.</span></span> <span data-ttu-id="779d1-155">Tady je několik příkazů, můžete použít v toocreate příkazový řádek vývojáře certifikát.</span><span class="sxs-lookup"><span data-stu-id="779d1-155">Here are a couple of commands that you can use in a Developer Command Prompt toocreate a certificate.</span></span> <span data-ttu-id="779d1-156">Změňte adresář toowhere chcete hello cert soubory vytvořené.</span><span class="sxs-lookup"><span data-stu-id="779d1-156">Change directory toowhere you want hello cert files created.</span></span>  <span data-ttu-id="779d1-157">Navíc pro hello počáteční a koncové datum hello certifikát, použijte hello aktuální datum plus 1 rok.</span><span class="sxs-lookup"><span data-stu-id="779d1-157">Also, for hello beginning and ending date of hello certificate, use hello current date plus 1 year.</span></span>

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 03/07/2017 -e 03/07/2018 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

<span data-ttu-id="779d1-158">Poznamenejte si hello koncové datum a hello hesla pro hello .pfx (v tomto příkladu: 07/31. prosinci 2016 a test123).</span><span class="sxs-lookup"><span data-stu-id="779d1-158">Make note of hello end date and hello password for hello .pfx (in this example: 07/31/2016 and test123).</span></span> <span data-ttu-id="779d1-159">Je nutné je níže.</span><span class="sxs-lookup"><span data-stu-id="779d1-159">You will need them below.</span></span>

<span data-ttu-id="779d1-160">Další informace o vytvoření testovacího certifikátu najdete v tématu [postup: vytvořit vaše vlastní testovací certifikát](https://msdn.microsoft.com/library/ff699202.aspx)</span><span class="sxs-lookup"><span data-stu-id="779d1-160">For more information on creating a test certificate, see [How to: Create Your Own Test Certificate](https://msdn.microsoft.com/library/ff699202.aspx)</span></span>

<span data-ttu-id="779d1-161">**Přidružení hello certifikát s aplikací Azure AD** nyní, když máte certifikát, je nutné tooassociate ji pomocí aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="779d1-161">**Associate hello Certificate with an Azure AD application** Now that you have a certificate, you need tooassociate it with an Azure AD application.</span></span> <span data-ttu-id="779d1-162">Hello portálu Azure v současné době nepodporuje tento pracovní postup; To lze provést pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="779d1-162">Presently, hello Azure Portal does not support this workflow; this can be completed through PowerShell.</span></span> <span data-ttu-id="779d1-163">Spusťte následující příkazy tooassoicate hello certifikát s aplikací Azure AD hello hello:</span><span class="sxs-lookup"><span data-stu-id="779d1-163">Run hello following commands tooassoicate hello certificate with hello Azure AD application:</span></span>

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
    $x509.Import("C:\data\KVWebApp.cer")
    $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    # If you used different dates for makecert then adjust these values
    $now = [System.DateTime]::Now
    $yearfromnow = $now.AddYears(1)

    $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -CertValue $credValue -StartDate $now -EndDate $yearfromnow

    $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get hello thumbprint toouse in your app settings
    $x509.Thumbprint

<span data-ttu-id="779d1-164">Po spuštění těchto příkazů, uvidíte hello aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="779d1-164">After you have run these commands, you can see hello application in Azure AD.</span></span> <span data-ttu-id="779d1-165">Při hledání, ujistěte se, že vyberete "Moje společnost vlastní aplikace" místo "Aplikace společnost používá" v dialogovém okně hledání hello.</span><span class="sxs-lookup"><span data-stu-id="779d1-165">When searching, ensure you select "Applications my company owns" instead of "Applications my company uses" in hello search dialog.</span></span>

<span data-ttu-id="779d1-166">toolearn informace o objekty aplikací Azure AD a ServicePrincipal objekty, najdete v části [objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md)</span><span class="sxs-lookup"><span data-stu-id="779d1-166">toolearn more about Azure AD Application Objects and ServicePrincipal Objects, see [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md)</span></span>

<span data-ttu-id="779d1-167">**Přidat kód tooyour webové aplikace toouse hello certifikát** nyní přidáme kód tooyour webové aplikace tooaccess hello certifikátu a použít jej pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="779d1-167">**Add code tooyour Web App toouse hello Certificate** Now we will add code tooyour Web App tooaccess hello cert and use it for authentication.</span></span>

<span data-ttu-id="779d1-168">Nejprve je kód tooaccess hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="779d1-168">First there is code tooaccess hello cert.</span></span>

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since hello test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


<span data-ttu-id="779d1-169">Všimněte si, že hello StoreLocation je CurrentUser místo LocalMachine.</span><span class="sxs-lookup"><span data-stu-id="779d1-169">Note that hello StoreLocation is CurrentUser instead of LocalMachine.</span></span> <span data-ttu-id="779d1-170">A že jsme se poskytuje "Nepravda" toohello nalezena metoda, protože se používá testovací certifikát.</span><span class="sxs-lookup"><span data-stu-id="779d1-170">And that we are supplying 'false' toohello Find method because we are using a test cert.</span></span>

<span data-ttu-id="779d1-171">Dále je kód, který používá hello CertificateHelper a vytvoří ClientAssertionCertificate, která je potřebná pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="779d1-171">Next is code that uses hello CertificateHelper and creates a ClientAssertionCertificate which is needed for authentication.</span></span>

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


<span data-ttu-id="779d1-172">Zde je hello nový kód tooget hello přístupový token.</span><span class="sxs-lookup"><span data-stu-id="779d1-172">Here is hello new code tooget hello access token.</span></span> <span data-ttu-id="779d1-173">Tím se nahradí hello gettoken – metoda výše.</span><span class="sxs-lookup"><span data-stu-id="779d1-173">This replaces hello GetToken method above.</span></span> <span data-ttu-id="779d1-174">Uvedené ho jiný název pro usnadnění práce.</span><span class="sxs-lookup"><span data-stu-id="779d1-174">I have given it a different name for convenience.</span></span>

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

<span data-ttu-id="779d1-175">I všechny tohoto kódu umístili do projektu webové aplikace Utils třídy pro snadné použití.</span><span class="sxs-lookup"><span data-stu-id="779d1-175">I have put all of this code into my Web App project's Utils class for ease of use.</span></span>

<span data-ttu-id="779d1-176">Poslední změna kódu Hello se hello metody Application_Start.</span><span class="sxs-lookup"><span data-stu-id="779d1-176">hello last code change is in hello Application_Start method.</span></span> <span data-ttu-id="779d1-177">Nejdřív potřebujeme toocall hello GetCert() metoda tooload hello ClientAssertionCertificate.</span><span class="sxs-lookup"><span data-stu-id="779d1-177">First we need toocall hello GetCert() method tooload hello ClientAssertionCertificate.</span></span> <span data-ttu-id="779d1-178">A pak se nám změnit hello metoda zpětného volání, které jsme zadat při vytváření nové KeyVaultClient.</span><span class="sxs-lookup"><span data-stu-id="779d1-178">And then we change hello callback method that we supply when creating a new KeyVaultClient.</span></span> <span data-ttu-id="779d1-179">Všimněte si, že tím se nahradí hello kód, který jsme měli výše.</span><span class="sxs-lookup"><span data-stu-id="779d1-179">Note that this replaces hello code that we had above.</span></span>

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


<span data-ttu-id="779d1-180">**Přidat certifikát tooyour webové aplikace prostřednictvím portálu Azure hello** přidání certifikátu tooyour webové aplikace je jednoduchý dvoustupňový proces.</span><span class="sxs-lookup"><span data-stu-id="779d1-180">**Add a Certificate tooyour Web App through hello Azure Portal** Adding a Certificate tooyour Web App is a simple two-step process.</span></span> <span data-ttu-id="779d1-181">Nejprve přejděte toohello portálu Azure a přejděte tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="779d1-181">First, go toohello Azure Portal and navigate tooyour Web App.</span></span> <span data-ttu-id="779d1-182">V okně Nastavení hello pro vaši webovou aplikaci klikněte na položku hello "vlastní domény a SSL".</span><span class="sxs-lookup"><span data-stu-id="779d1-182">On hello Settings blade for your Web App, click on hello entry for "Custom domains and SSL".</span></span> <span data-ttu-id="779d1-183">Na hello budou okno, které se otevře, budete moct tooupload hello certifikát, který jste vytvořili výše, KVWebApp.pfx, ujistěte se, že jste si heslo hello pro hello pfx.</span><span class="sxs-lookup"><span data-stu-id="779d1-183">On hello blade that opens you will be able tooupload hello Certificate that you created above, KVWebApp.pfx, make sure that you remember hello password for hello pfx.</span></span>

![Přidání certifikátu tooa webové aplikace v hello portálu Azure][2]

<span data-ttu-id="779d1-185">Hello poslední věcí, je nutné, aby toodo je tooadd nastavení aplikace tooyour webovou aplikaci, která má název webu hello\_zatížení\_certifikáty a hodnotu *.</span><span class="sxs-lookup"><span data-stu-id="779d1-185">hello last thing that you need toodo is tooadd an Application Setting tooyour Web App that has hello name WEBSITE\_LOAD\_CERTIFICATES and a value of *.</span></span> <span data-ttu-id="779d1-186">Tím bude zajištěno, že se načtou všechny certifikáty.</span><span class="sxs-lookup"><span data-stu-id="779d1-186">This will ensure that all Certificates are loaded.</span></span> <span data-ttu-id="779d1-187">Pokud byste chtěli, že pouze hello tooload certifikáty, které jste odeslali a potom můžete zadat seznam jejich kryptografické otisky oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="779d1-187">If you wanted tooload only hello Certificates that you have uploaded, then you can enter a comma-separated list of their thumbprints.</span></span>

<span data-ttu-id="779d1-188">toolearn Další informace o přidání certifikátu tooa webové aplikace, najdete v části [pomocí certifikátů v aplikacích weby Azure](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)</span><span class="sxs-lookup"><span data-stu-id="779d1-188">toolearn more about adding a Certificate tooa Web App, see [Using Certificates in Azure Websites Applications](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)</span></span>

<span data-ttu-id="779d1-189">**Přidat certifikát tooKey trezoru jako tajný klíč** místo odeslání vašeho certifikátu toohello webové aplikace služby přímo, můžete ukládat v Key Vault jako tajný klíč a nasadit ho z ní.</span><span class="sxs-lookup"><span data-stu-id="779d1-189">**Add a Certificate tooKey Vault as a secret** Instead of uploading your certificate toohello Web App service directly, you can store it in Key Vault as a secret and deploy it from there.</span></span> <span data-ttu-id="779d1-190">Toto je popsané v následujícím příspěvku na blogu hello ve dvou krocích [nasazení Azure certifikát webové aplikace prostřednictvím Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)</span><span class="sxs-lookup"><span data-stu-id="779d1-190">This is a two-step process that is outlined in hello following blog post, [Deploying Azure Web App Certificate through Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)</span></span>

## <span data-ttu-id="779d1-191"><a id="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="779d1-191"><a id="next"></a>Next steps</span></span>
<span data-ttu-id="779d1-192">Programátorské reference najdete v části [Azure Key Vault C# klienta referenční dokumentace rozhraní API](https://msdn.microsoft.com/library/azure/dn903628.aspx).</span><span class="sxs-lookup"><span data-stu-id="779d1-192">For programming references, see [Azure Key Vault C# Client API Reference](https://msdn.microsoft.com/library/azure/dn903628.aspx).</span></span>

<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
