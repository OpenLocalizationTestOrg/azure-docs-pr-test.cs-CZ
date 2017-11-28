---
title: "Použití Azure Key Vault z webové aplikace | Microsoft Docs"
description: "Pomocí tohoto kurzu můžete Naučte se používat Azure Key Vault z webové aplikace."
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
ms.openlocfilehash: d095bcfe37baefa90cf79bb48bff3f703ce1dad7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-key-vault-from-a-web-application"></a><span data-ttu-id="aa523-103">Použití Azure Key Vault z webové aplikace</span><span class="sxs-lookup"><span data-stu-id="aa523-103">Use Azure Key Vault from a Web Application</span></span>
## <a name="introduction"></a><span data-ttu-id="aa523-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="aa523-104">Introduction</span></span>
<span data-ttu-id="aa523-105">Pomocí tohoto kurzu můžete Naučte se používat Azure Key Vault z webové aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="aa523-105">Use this tutorial to help you learn how to use Azure Key Vault from a web application in Azure.</span></span> <span data-ttu-id="aa523-106">Provede vás procesem přístup k tajného klíče z Azure Key Vault, takže je možné ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aa523-106">It walks you through the process of accessing a secret from an Azure Key Vault so that it can be used in your web application.</span></span>

<span data-ttu-id="aa523-107">**Odhadovaný čas dokončení:** 15 minut</span><span class="sxs-lookup"><span data-stu-id="aa523-107">**Estimated time to complete:** 15 minutes</span></span>

<span data-ttu-id="aa523-108">Souhrnné informace o Azure Key Vault naleznete v tématu [Co je Azure Key Vault?](key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="aa523-108">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aa523-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="aa523-109">Prerequisites</span></span>
<span data-ttu-id="aa523-110">K dokončení tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="aa523-110">To complete this tutorial, you must have the following:</span></span>

* <span data-ttu-id="aa523-111">Identifikátor URI pro tajný klíč v Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="aa523-111">A URI to a secret in an Azure Key Vault</span></span>
* <span data-ttu-id="aa523-112">ID klienta a tajný klíč klienta pro webovou aplikaci, které jsou zaregistrované v Azure Active Directory, který má přístup k trezoru klíč</span><span class="sxs-lookup"><span data-stu-id="aa523-112">A Client ID and a Client Secret for a web application registered with Azure Active Directory that has access to your Key Vault</span></span>
* <span data-ttu-id="aa523-113">Webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa523-113">A web application.</span></span> <span data-ttu-id="aa523-114">Jsme se, že se zobrazuje kroky pro aplikaci ASP.NET MVC nasazené v Azure jako webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aa523-114">We will be showing the steps for an ASP.NET MVC application deployed in Azure as a Web App.</span></span>

> [!NOTE]
> <span data-ttu-id="aa523-115">Je nezbytné, že jste dokončili kroky uvedené v [Začínáme s Azure Key Vault](key-vault-get-started.md) pro účely tohoto kurzu tak, aby měli identifikátor URI tajného klíče a ID klienta a tajný klíč klienta pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aa523-115">It is essential that you have completed the steps listed in [Get Started with Azure Key Vault](key-vault-get-started.md) for this tutorial so that you have the URI to a secret and the Client ID and Client Secret for a web application.</span></span>
> 
> 

<span data-ttu-id="aa523-116">Webovou aplikaci, která bude mít přístup k Key Vault je ten, který je zaregistrován ve službě Azure Active Directory a byl poskytnut přístup k trezoru klíč.</span><span class="sxs-lookup"><span data-stu-id="aa523-116">The web application that will be accessing the Key Vault is the one that is registered in Azure Active Directory and has been given access to your Key Vault.</span></span> <span data-ttu-id="aa523-117">Pokud tomu tak není, přejděte zpět na zaregistrovat aplikaci v kurzu Začínáme a opakujte kroky uvedené.</span><span class="sxs-lookup"><span data-stu-id="aa523-117">If this is not the case, go back to Register an Application in the Get Started tutorial and repeat the steps listed.</span></span>

<span data-ttu-id="aa523-118">Tento kurz je určen pro vývojářům webů, které pochopit základy toho vytváření webových aplikací v Azure.</span><span class="sxs-lookup"><span data-stu-id="aa523-118">This tutorial is designed for web developers that understand the basics of creating web applications on Azure.</span></span> <span data-ttu-id="aa523-119">Další informace o službě Azure Web Apps, naleznete v části [přehled Web Apps](../app-service-web/app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="aa523-119">For more information about Azure Web Apps, see [Web Apps overview](../app-service-web/app-service-web-overview.md).</span></span>

## <span data-ttu-id="aa523-120"><a id="packages"></a>Přidání balíčků Nuget</span><span class="sxs-lookup"><span data-stu-id="aa523-120"><a id="packages"></a>Add Nuget Packages</span></span>
<span data-ttu-id="aa523-121">Jsou dva balíčky, které webové aplikace musí mít nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="aa523-121">There are two packages that your web application needs to have installed.</span></span>

* <span data-ttu-id="aa523-122">Knihovna ověřování Active Directory - obsahuje metody pro interakci s Azure Active Directory a správa identity uživatele</span><span class="sxs-lookup"><span data-stu-id="aa523-122">Active Directory Authentication Library - contains methods for interacting with Azure Active Directory and managing user identity</span></span>
* <span data-ttu-id="aa523-123">Azure Key Vault Library - obsahuje metody pro interakci s Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="aa523-123">Azure Key Vault Library - contains methods for interacting with Azure Key Vault</span></span>

<span data-ttu-id="aa523-124">Obě tyto balíčky můžete nainstalovat pomocí konzoly Správce balíčků pomocí příkazu Install-Package.</span><span class="sxs-lookup"><span data-stu-id="aa523-124">Both of these packages can be installed using the Package Manager Console using the Install-Package command.</span></span>

    // this is currently the latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <span data-ttu-id="aa523-125"><a id="webconfig"></a>Upravit soubor Web.Config</span><span class="sxs-lookup"><span data-stu-id="aa523-125"><a id="webconfig"></a>Modify Web.Config</span></span>
<span data-ttu-id="aa523-126">Existují tři nastavení aplikace, které je třeba přidat do souboru web.config následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="aa523-126">There are three application settings that need to be added to the web.config file as follows.</span></span>

    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


<span data-ttu-id="aa523-127">Pokud nechcete kvůli hostování vaší aplikace jako webové aplikace Azure, měli byste k souboru web.config přidat skutečnými hodnotami ClientId, sdílený tajný klíč klienta a identifikátor URI tajného klíče.</span><span class="sxs-lookup"><span data-stu-id="aa523-127">If you are not going to host your application as an Azure Web App, then you should add the actual ClientId, Client Secret, and Secret URI values to the web.config.</span></span> <span data-ttu-id="aa523-128">V opačném případě ponechte tyto fiktivní hodnoty, protože jsme přidáním skutečnými hodnotami na portálu Azure vytváří další úroveň zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="aa523-128">Otherwise leave these dummy values because we will be adding the actual values in the Azure Portal for an additional level of security.</span></span>

## <span data-ttu-id="aa523-129"><a id="gettoken"></a>Přidejte metodu k získání tokenu přístupu</span><span class="sxs-lookup"><span data-stu-id="aa523-129"><a id="gettoken"></a>Add Method to Get an Access Token</span></span>
<span data-ttu-id="aa523-130">Chcete-li použít rozhraní API trezoru klíč musíte přístupový token.</span><span class="sxs-lookup"><span data-stu-id="aa523-130">In order to use the Key Vault API you need an access token.</span></span> <span data-ttu-id="aa523-131">Klient klíče trezoru zpracovává volání do rozhraní API Key Vault, ale budete muset zadat pomocí funkce, který získá přístupový token.</span><span class="sxs-lookup"><span data-stu-id="aa523-131">The Key Vault Client handles calls to the Key Vault API but you need to supply it with a function that gets the access token.</span></span>  

<span data-ttu-id="aa523-132">Následuje kód slouží k získání tokenu přístupu z Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="aa523-132">Following is the code to get an access token from Azure Active Directory.</span></span> <span data-ttu-id="aa523-133">Tento kód můžete přejít kdekoli v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aa523-133">This code can go anywhere in your application.</span></span> <span data-ttu-id="aa523-134">Líbí se přidání Utils nebo EncryptionHelper třídy.</span><span class="sxs-lookup"><span data-stu-id="aa523-134">I like to add a Utils or EncryptionHelper class.</span></span>  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property to hold the secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //the method that will be provided to the KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

> [!NOTE]
> <span data-ttu-id="aa523-135">Pomocí ID klienta a tajný klíč klienta je nejjednodušší způsob, jak ověřit aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aa523-135">Using a Client ID and Client Secret is the easiest way to authenticate an Azure AD application.</span></span> <span data-ttu-id="aa523-136">A použití ve vaší webové aplikace je možné oddělení povinností a větší kontrolu nad vaší správy klíčů.</span><span class="sxs-lookup"><span data-stu-id="aa523-136">And using it in your web application allows for a separation of duties and more control over your key management.</span></span> <span data-ttu-id="aa523-137">Je však závislý na uvedení tajný klíč klienta v nastavení konfigurace, které pro některé můžou být jako rizikové jako uvedení tajný klíč, který chcete chránit v nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="aa523-137">But it does rely on putting the Client Secret in your configuration settings which for some can be as risky as putting the secret that you want to protect in your configuration settings.</span></span> <span data-ttu-id="aa523-138">Níže najdete informace o tom, jak použít ID klienta a certifikát místo ID klienta a tajný klíč klienta k ověření aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aa523-138">See below for a discussion on how to use a Client ID and Certificate instead of Client ID and Client Secret to authenticate the Azure AD application.</span></span>
> 
> 

## <span data-ttu-id="aa523-139"><a id="appstart"></a>Načtení tajný klíč na spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="aa523-139"><a id="appstart"></a>Retrieve the secret on Application Start</span></span>
<span data-ttu-id="aa523-140">Nyní potřebujeme kódu pro volání rozhraní API Key Vault a načítání tajného klíče.</span><span class="sxs-lookup"><span data-stu-id="aa523-140">Now we need code to call the Key Vault API and retrieve the secret.</span></span> <span data-ttu-id="aa523-141">Následující kód můžete umístit kdekoli, tak dlouho, dokud se označuje jako předtím, než je nutné ji použít.</span><span class="sxs-lookup"><span data-stu-id="aa523-141">The following code can be put anywhere as long as it is called before you need to use it.</span></span> <span data-ttu-id="aa523-142">Tento kód v události spustit aplikace v soubor Global.asax mít umístíte tak, aby při spuštění se spustí jednou a zpřístupní tajný klíč pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aa523-142">I have put this code in the Application Start event in the Global.asax so that it runs once on start and makes the secret available for the application.</span></span>

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

    //I put a variable in a Utils class to hold the secret for general  application use.
    Utils.EncryptSecret = sec.Value;



## <span data-ttu-id="aa523-143"><a id="portalsettings"></a>Přidat nastavení aplikace na portálu Azure (volitelné)</span><span class="sxs-lookup"><span data-stu-id="aa523-143"><a id="portalsettings"></a>Add App Settings in the Azure Portal (optional)</span></span>
<span data-ttu-id="aa523-144">Pokud máte webové aplikace Azure můžete nyní přidat skutečnými hodnotami pro AppSettings na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="aa523-144">If you have an Azure Web App you can now add the actual values for the AppSettings in the Azure Portal.</span></span> <span data-ttu-id="aa523-145">Díky tomu skutečnými hodnotami nebude v souboru web.config, ale chráněná přes portál, kde můžete dělat samostatnou přístupovou ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="aa523-145">By doing this, the actual values will not be in the web.config but protected via the Portal where you have separate access control capabilities.</span></span> <span data-ttu-id="aa523-146">Tyto hodnoty se nahradí hodnoty, které jste zadali v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="aa523-146">These values will be substituted for the values that you entered in your web.config.</span></span> <span data-ttu-id="aa523-147">Ujistěte se, že názvy jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="aa523-147">Make sure that the names are the same.</span></span>

![Nastavení aplikace se zobrazí na portálu Azure][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a><span data-ttu-id="aa523-149">Ověřování pomocí certifikátu místo tajný klíč klienta</span><span class="sxs-lookup"><span data-stu-id="aa523-149">Authenticate with a Certificate instead of a Client Secret</span></span>
<span data-ttu-id="aa523-150">Jiný způsob, jak ověřit aplikaci Azure AD je pomocí ID klienta a certifikát místo ID klienta a tajný klíč klienta.</span><span class="sxs-lookup"><span data-stu-id="aa523-150">Another way to authenticate an Azure AD application is by using a Client ID and a Certificate instead of a Client ID and Client Secret.</span></span> <span data-ttu-id="aa523-151">Toto jsou kroky pro použití certifikátu ve webové aplikaci Azure:</span><span class="sxs-lookup"><span data-stu-id="aa523-151">Following are the steps to use a Certificate in an Azure Web App:</span></span>

1. <span data-ttu-id="aa523-152">Získat nebo vytvořit certifikát</span><span class="sxs-lookup"><span data-stu-id="aa523-152">Get or Create a Certificate</span></span>
2. <span data-ttu-id="aa523-153">Certifikát přidružit aplikaci Azure AD</span><span class="sxs-lookup"><span data-stu-id="aa523-153">Associate the Certificate with an Azure AD application</span></span>
3. <span data-ttu-id="aa523-154">Přidání kódu do vaší webové aplikace na použití certifikátu</span><span class="sxs-lookup"><span data-stu-id="aa523-154">Add code to your Web App to use the Certificate</span></span>
4. <span data-ttu-id="aa523-155">Přidat certifikát do vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="aa523-155">Add a Certificate to your Web App</span></span>

<span data-ttu-id="aa523-156">**Získejte nebo vytvořte certifikát** pro naše účely budeme testovacího certifikátu.</span><span class="sxs-lookup"><span data-stu-id="aa523-156">**Get or Create a Certificate** For our purposes we will make a test certificate.</span></span> <span data-ttu-id="aa523-157">Tady je několik příkazů, které můžete použít v příkazovém řádku vývojáře vytvořit certifikát.</span><span class="sxs-lookup"><span data-stu-id="aa523-157">Here are a couple of commands that you can use in a Developer Command Prompt to create a certificate.</span></span> <span data-ttu-id="aa523-158">Změňte adresář na místo, kam chcete vytvořené soubory certifikátu.</span><span class="sxs-lookup"><span data-stu-id="aa523-158">Change directory to where you want the cert files created.</span></span>  <span data-ttu-id="aa523-159">Navíc pro počáteční a koncové datum platnosti certifikátu použijte aktuální datum plus 1 rok.</span><span class="sxs-lookup"><span data-stu-id="aa523-159">Also, for the beginning and ending date of the certificate, use the current date plus 1 year.</span></span>

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 03/07/2017 -e 03/07/2018 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

<span data-ttu-id="aa523-160">Poznamenejte si koncové datum a heslo .pfx (v tomto příkladu: 07/31. prosinci 2016 a test123).</span><span class="sxs-lookup"><span data-stu-id="aa523-160">Make note of the end date and the password for the .pfx (in this example: 07/31/2016 and test123).</span></span> <span data-ttu-id="aa523-161">Je nutné je níže.</span><span class="sxs-lookup"><span data-stu-id="aa523-161">You will need them below.</span></span>

<span data-ttu-id="aa523-162">Další informace o vytvoření testovacího certifikátu najdete v tématu [postup: vytvořit vaše vlastní testovací certifikát](https://msdn.microsoft.com/library/ff699202.aspx)</span><span class="sxs-lookup"><span data-stu-id="aa523-162">For more information on creating a test certificate, see [How to: Create Your Own Test Certificate](https://msdn.microsoft.com/library/ff699202.aspx)</span></span>

<span data-ttu-id="aa523-163">**Certifikát přidružit aplikaci Azure AD** nyní, když máte certifikát, je třeba ji přidružit k aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aa523-163">**Associate the Certificate with an Azure AD application** Now that you have a certificate, you need to associate it with an Azure AD application.</span></span> <span data-ttu-id="aa523-164">Na portálu Azure v současné době nepodporuje tento pracovní postup; To lze provést pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="aa523-164">Presently, the Azure Portal does not support this workflow; this can be completed through PowerShell.</span></span> <span data-ttu-id="aa523-165">Spusťte následující příkazy, které assoicate certifikát s aplikací Azure AD:</span><span class="sxs-lookup"><span data-stu-id="aa523-165">Run the following commands to assoicate the certificate with the Azure AD application:</span></span>

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
    $x509.Import("C:\data\KVWebApp.cer")
    $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    # If you used different dates for makecert then adjust these values
    $now = [System.DateTime]::Now
    $yearfromnow = $now.AddYears(1)

    $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -CertValue $credValue -StartDate $now -EndDate $yearfromnow

    $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get the thumbprint to use in your app settings
    $x509.Thumbprint

<span data-ttu-id="aa523-166">Po spuštění těchto příkazů se zobrazí aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aa523-166">After you have run these commands, you can see the application in Azure AD.</span></span> <span data-ttu-id="aa523-167">Při hledání, zkontrolujte, že jste vybrali "Moje společnost vlastní aplikace" místo "Aplikace společnost používá" v dialogovém okně hledání.</span><span class="sxs-lookup"><span data-stu-id="aa523-167">When searching, ensure you select "Applications my company owns" instead of "Applications my company uses" in the search dialog.</span></span>

<span data-ttu-id="aa523-168">Další informace o objektech aplikace Azure AD a ServicePrincipal objektů najdete v tématu [objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md)</span><span class="sxs-lookup"><span data-stu-id="aa523-168">To learn more about Azure AD Application Objects and ServicePrincipal Objects, see [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md)</span></span>

<span data-ttu-id="aa523-169">**Přidání kódu do vaší webové aplikace na použití certifikátu** nyní přidáme kódu do vaší webové aplikace na přístup certifikát a použít jej pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="aa523-169">**Add code to your Web App to use the Certificate** Now we will add code to your Web App to access the cert and use it for authentication.</span></span>

<span data-ttu-id="aa523-170">Nejprve je kód pro přístup k certifikát.</span><span class="sxs-lookup"><span data-stu-id="aa523-170">First there is code to access the cert.</span></span>

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since the test root isn't installed.
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


<span data-ttu-id="aa523-171">Všimněte si, že je StoreLocation CurrentUser místo LocalMachine.</span><span class="sxs-lookup"><span data-stu-id="aa523-171">Note that the StoreLocation is CurrentUser instead of LocalMachine.</span></span> <span data-ttu-id="aa523-172">A že jsme se dodává false metodu najít vzhledem k tomu, že používáme testovací certifikát.</span><span class="sxs-lookup"><span data-stu-id="aa523-172">And that we are supplying 'false' to the Find method because we are using a test cert.</span></span>

<span data-ttu-id="aa523-173">Dále je kód, který používá CertificateHelper a vytvoří ClientAssertionCertificate, která je potřebná pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="aa523-173">Next is code that uses the CertificateHelper and creates a ClientAssertionCertificate which is needed for authentication.</span></span>

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


<span data-ttu-id="aa523-174">Tady je nový kód slouží k získání tokenu přístupu.</span><span class="sxs-lookup"><span data-stu-id="aa523-174">Here is the new code to get the access token.</span></span> <span data-ttu-id="aa523-175">Tím se nahradí výše gettoken – metoda.</span><span class="sxs-lookup"><span data-stu-id="aa523-175">This replaces the GetToken method above.</span></span> <span data-ttu-id="aa523-176">Uvedené ho jiný název pro usnadnění práce.</span><span class="sxs-lookup"><span data-stu-id="aa523-176">I have given it a different name for convenience.</span></span>

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

<span data-ttu-id="aa523-177">I všechny tohoto kódu umístili do projektu webové aplikace Utils třídy pro snadné použití.</span><span class="sxs-lookup"><span data-stu-id="aa523-177">I have put all of this code into my Web App project's Utils class for ease of use.</span></span>

<span data-ttu-id="aa523-178">Do metody Application_Start je poslední změny kódu.</span><span class="sxs-lookup"><span data-stu-id="aa523-178">The last code change is in the Application_Start method.</span></span> <span data-ttu-id="aa523-179">Je potřeba nejdřív voláním metody GetCert() zatížení ClientAssertionCertificate.</span><span class="sxs-lookup"><span data-stu-id="aa523-179">First we need to call the GetCert() method to load the ClientAssertionCertificate.</span></span> <span data-ttu-id="aa523-180">A pak se nám změnit metoda zpětného volání, které jsme zadat při vytváření nové KeyVaultClient.</span><span class="sxs-lookup"><span data-stu-id="aa523-180">And then we change the callback method that we supply when creating a new KeyVaultClient.</span></span> <span data-ttu-id="aa523-181">Všimněte si, že tím se nahradí kód, který jsme měli výše.</span><span class="sxs-lookup"><span data-stu-id="aa523-181">Note that this replaces the code that we had above.</span></span>

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


<span data-ttu-id="aa523-182">**Přidat certifikát do vaší webové aplikace prostřednictvím portálu Azure** přidání certifikátu do vaší webové aplikace je jednoduchý dvoustupňový proces.</span><span class="sxs-lookup"><span data-stu-id="aa523-182">**Add a Certificate to your Web App through the Azure Portal** Adding a Certificate to your Web App is a simple two-step process.</span></span> <span data-ttu-id="aa523-183">První přejděte na portál Azure a přejděte do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa523-183">First, go to the Azure Portal and navigate to your Web App.</span></span> <span data-ttu-id="aa523-184">V okně nastavení pro webové aplikace klikněte na položku "vlastní domény a SSL".</span><span class="sxs-lookup"><span data-stu-id="aa523-184">On the Settings blade for your Web App, click on the entry for "Custom domains and SSL".</span></span> <span data-ttu-id="aa523-185">V okně, které se otevře vám bude moct nahrát certifikát, který jste vytvořili výše, KVWebApp.pfx, ujistěte se, že si pamatujete heslo pro soubor pfx.</span><span class="sxs-lookup"><span data-stu-id="aa523-185">On the blade that opens you will be able to upload the Certificate that you created above, KVWebApp.pfx, make sure that you remember the password for the pfx.</span></span>

![Přidání certifikátu do webové aplikace na portálu Azure][2]

<span data-ttu-id="aa523-187">Poslední věcí, kterou je potřeba udělat je přidat nastavení aplikace do webové aplikace, který má název webu\_zatížení\_certifikáty a hodnotu *.</span><span class="sxs-lookup"><span data-stu-id="aa523-187">The last thing that you need to do is to add an Application Setting to your Web App that has the name WEBSITE\_LOAD\_CERTIFICATES and a value of *.</span></span> <span data-ttu-id="aa523-188">Tím bude zajištěno, že se načtou všechny certifikáty.</span><span class="sxs-lookup"><span data-stu-id="aa523-188">This will ensure that all Certificates are loaded.</span></span> <span data-ttu-id="aa523-189">Pokud chcete načíst jenom certifikáty, které jste odeslali, můžete zadat seznam jejich kryptografické otisky oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="aa523-189">If you wanted to load only the Certificates that you have uploaded, then you can enter a comma-separated list of their thumbprints.</span></span>

<span data-ttu-id="aa523-190">Další informace o přidání certifikátu do webové aplikace najdete v tématu [pomocí certifikátů v aplikacích weby Azure](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)</span><span class="sxs-lookup"><span data-stu-id="aa523-190">To learn more about adding a Certificate to a Web App, see [Using Certificates in Azure Websites Applications](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)</span></span>

<span data-ttu-id="aa523-191">**Přidat certifikát do Key Vault jako tajný klíč** místo přímo nahrát certifikát služby webové aplikace, můžete ukládat v Key Vault jako tajný klíč a nasadit ho z ní.</span><span class="sxs-lookup"><span data-stu-id="aa523-191">**Add a Certificate to Key Vault as a secret** Instead of uploading your certificate to the Web App service directly, you can store it in Key Vault as a secret and deploy it from there.</span></span> <span data-ttu-id="aa523-192">Toto je popsané v tomto příspěvku blogu ve dvou krocích [nasazení Azure certifikát webové aplikace prostřednictvím Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)</span><span class="sxs-lookup"><span data-stu-id="aa523-192">This is a two-step process that is outlined in the following blog post, [Deploying Azure Web App Certificate through Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)</span></span>

## <span data-ttu-id="aa523-193"><a id="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="aa523-193"><a id="next"></a>Next steps</span></span>
<span data-ttu-id="aa523-194">Programátorské reference najdete v části [Azure Key Vault C# klienta referenční dokumentace rozhraní API](https://msdn.microsoft.com/library/azure/dn903628.aspx).</span><span class="sxs-lookup"><span data-stu-id="aa523-194">For programming references, see [Azure Key Vault C# Client API Reference](https://msdn.microsoft.com/library/azure/dn903628.aspx).</span></span>

<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
