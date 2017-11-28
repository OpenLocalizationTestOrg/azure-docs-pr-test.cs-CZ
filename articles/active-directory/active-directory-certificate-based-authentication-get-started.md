---
title: "aaaAzure na základě certifikátu ověřování služby Active Directory – Začínáme | Microsoft Docs"
description: "Zjistěte, jak ověřování pomocí certifikátů tooconfigure ve vašem prostředí"
author: MarkusVi
documentationcenter: na
manager: femila
ms.assetid: c6ad7640-8172-4541-9255-770f39ecce0e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 3c73bdf56018c0716085c923a61e9560dbe4004c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a><span data-ttu-id="d3af3-103">Začínáme s ověřováním na základě certifikátů ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d3af3-103">Get started with certificate-based authentication in Azure Active Directory</span></span>

<span data-ttu-id="d3af3-104">Ověřování pomocí certifikátů umožňuje toobe službou Azure Active Directory se ověřit klientský certifikát na zařízení s Windows, Android nebo iOS při připojování váš účet systému Exchange online:</span><span class="sxs-lookup"><span data-stu-id="d3af3-104">Certificate-based authentication enables you toobe authenticated by Azure Active Directory with a client certificate on a Windows, Android or iOS device when connecting your Exchange online account to:</span></span> 

- <span data-ttu-id="d3af3-105">Mobilní aplikace Office, jako je například Microsoft Outlook a Microsoft Word</span><span class="sxs-lookup"><span data-stu-id="d3af3-105">Office mobile applications such as Microsoft Outlook and Microsoft Word</span></span>   

- <span data-ttu-id="d3af3-106">Klienti Exchange ActiveSync (EAS)</span><span class="sxs-lookup"><span data-stu-id="d3af3-106">Exchange ActiveSync (EAS) clients</span></span> 

<span data-ttu-id="d3af3-107">Konfigurace tato funkce eliminuje hello nutné tooenter uživatelské jméno a heslo kombinaci do určité e-mailu a aplikace Microsoft Office na vašem mobilním zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3af3-107">Configuring this feature eliminates hello need tooenter a username and password combination into certain mail and Microsoft Office applications on your mobile device.</span></span> 

<span data-ttu-id="d3af3-108">V tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="d3af3-108">This topic:</span></span>

- <span data-ttu-id="d3af3-109">Poskytuje vám hello kroky tooconfigure a využívat ověřování pomocí certifikátů pro uživatele klientů Office 365 Enterprise, Education a obchodní a plány vládou USA.</span><span class="sxs-lookup"><span data-stu-id="d3af3-109">Provides you with hello steps tooconfigure and utilize certificate-based authentication for users of tenants in Office 365 Enterprise, Business, Education, and US Government plans.</span></span> <span data-ttu-id="d3af3-110">Tato funkce je dostupná ve verzi preview v Číně Office 365, US Government obrany a US Government Federal plány.</span><span class="sxs-lookup"><span data-stu-id="d3af3-110">This feature is available in preview in Office 365 China, US Government Defense, and US Government Federal plans.</span></span> 

- <span data-ttu-id="d3af3-111">Předpokládá, že již máte [infrastruktury veřejných klíčů (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) a [služby AD FS](connect/active-directory-aadconnectfed-whatis.md) nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="d3af3-111">Assumes that you already have a [public key infrastructure (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) and [AD FS](connect/active-directory-aadconnectfed-whatis.md) configured.</span></span>    


## <a name="requirements"></a><span data-ttu-id="d3af3-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d3af3-112">Requirements</span></span>

<span data-ttu-id="d3af3-113">ověřování pomocí certifikátů tooconfigure hello jsou splněny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="d3af3-113">tooconfigure certificate-based authentication, hello following must be true:</span></span>  

- <span data-ttu-id="d3af3-114">Ověřování pomocí certifikátů (CBA) je podporována pouze pro federovaném prostředí pro aplikace prohlížeče nebo nativní klienty, kteří používají moderní ověřování (ADAL).</span><span class="sxs-lookup"><span data-stu-id="d3af3-114">Certificate-based authentication (CBA) is only supported for Federated environments for browser applications or native clients using modern authentication (ADAL).</span></span> <span data-ttu-id="d3af3-115">Jedinou výjimkou Hello je Exchange Active Sync (EAS) pro EXO, který můžete použít pro federované i spravované účty.</span><span class="sxs-lookup"><span data-stu-id="d3af3-115">hello one exception is Exchange Active Sync (EAS) for EXO which can be used for both, federated and managed accounts.</span></span> 

- <span data-ttu-id="d3af3-116">Hello kořenové certifikační autority a jakékoliv zprostředkující certifikační autority musí být nakonfigurované v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d3af3-116">hello root certificate authority and any intermediate certificate authorities must be configured in Azure Active Directory.</span></span>  

- <span data-ttu-id="d3af3-117">Seznam odvolaných certifikátů (CRL), může být odkazováno prostřednictvím internetové adresy URL musí mít každý certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="d3af3-117">Each certificate authority must have a certificate revocation list (CRL) that can be referenced via an Internet facing URL.</span></span>  

- <span data-ttu-id="d3af3-118">Musí mít alespoň jednu certifikační autoritu nakonfigurován v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d3af3-118">You must have at least one certificate authority configured in Azure Active Directory.</span></span> <span data-ttu-id="d3af3-119">Související kroky můžete najít v hello [konfigurace certifikačních autorit hello](#step-2-configure-the-certificate-authorities) části.</span><span class="sxs-lookup"><span data-stu-id="d3af3-119">You can find related steps in hello [Configure hello certificate authorities](#step-2-configure-the-certificate-authorities) section.</span></span>  

- <span data-ttu-id="d3af3-120">U klientů Exchange ActiveSync musí mít hello klientský certifikát uživatele hello směrovatelné e-mailová adresa v systému Exchange online v hlavní název buď hello nebo hello Název RFC822 hodnotu pole alternativní název subjektu hello.</span><span class="sxs-lookup"><span data-stu-id="d3af3-120">For Exchange ActiveSync clients, hello client certificate must have hello user’s routable email address in Exchange online in either hello Principal Name or hello RFC822 Name value of hello Subject Alternative Name field.</span></span> <span data-ttu-id="d3af3-121">Azure Active Directory mapuje hello RFC822 hodnota toohello adresu proxy serveru atribut v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="d3af3-121">Azure Active Directory maps hello RFC822 value toohello Proxy Address attribute in hello directory.</span></span>  

- <span data-ttu-id="d3af3-122">Klientské zařízení musí mít přístup tooat alespoň jednu certifikační autority, která vydává certifikáty klienta.</span><span class="sxs-lookup"><span data-stu-id="d3af3-122">Your client device must have access tooat least one certificate authority that issues client certificates.</span></span>  

- <span data-ttu-id="d3af3-123">Klientský certifikát pro ověřování klientů musí být vydán tooyour klienta.</span><span class="sxs-lookup"><span data-stu-id="d3af3-123">A client certificate for client authentication must have been issued tooyour client.</span></span>  




## <a name="step-1-select-your-device-platform"></a><span data-ttu-id="d3af3-124">Krok 1: Vyberte platformu zařízení</span><span class="sxs-lookup"><span data-stu-id="d3af3-124">Step 1: Select your device platform</span></span>

<span data-ttu-id="d3af3-125">Jako první krok potřebujete pro hello platforma, která vás, tooreview hello následující:</span><span class="sxs-lookup"><span data-stu-id="d3af3-125">As a first step, for hello device platform you care about, you need tooreview hello following:</span></span>

- <span data-ttu-id="d3af3-126">Podpora mobilních aplikacích Office Hello</span><span class="sxs-lookup"><span data-stu-id="d3af3-126">hello Office mobile applications support</span></span> 
- <span data-ttu-id="d3af3-127">požadavky na konkrétní implementace Hello</span><span class="sxs-lookup"><span data-stu-id="d3af3-127">hello specific implementation requirements</span></span>  

<span data-ttu-id="d3af3-128">pro následující platformy zařízení hello existuje informace související s Hello:</span><span class="sxs-lookup"><span data-stu-id="d3af3-128">hello related information exists for hello following device platforms:</span></span>

- [<span data-ttu-id="d3af3-129">Android</span><span class="sxs-lookup"><span data-stu-id="d3af3-129">Android</span></span>](active-directory-certificate-based-authentication-android.md)
- [<span data-ttu-id="d3af3-130">iOS</span><span class="sxs-lookup"><span data-stu-id="d3af3-130">iOS</span></span>](active-directory-certificate-based-authentication-ios.md)


## <a name="step-2-configure-hello-certificate-authorities"></a><span data-ttu-id="d3af3-131">Krok 2: Konfigurace hello certifikačních autorit</span><span class="sxs-lookup"><span data-stu-id="d3af3-131">Step 2: Configure hello certificate authorities</span></span> 

<span data-ttu-id="d3af3-132">tooconfigure certifikačních autorit ve službě Azure Active Directory, pro každý certifikační autority, nahrajte hello následující:</span><span class="sxs-lookup"><span data-stu-id="d3af3-132">tooconfigure your certificate authorities in Azure Active Directory, for each certificate authority, upload hello following:</span></span> 

* <span data-ttu-id="d3af3-133">Hello veřejnou část hello certifikátu v *.cer* formátu</span><span class="sxs-lookup"><span data-stu-id="d3af3-133">hello public portion of hello certificate, in *.cer* format</span></span> 
* <span data-ttu-id="d3af3-134">Hello internetový bod adresy URL, kde hello seznamy odvolaných certifikátů (CRL) jsou umístěny</span><span class="sxs-lookup"><span data-stu-id="d3af3-134">hello Internet facing URLs where hello Certificate Revocation Lists (CRLs) reside</span></span>

<span data-ttu-id="d3af3-135">Hello schéma pro certifikační autority vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="d3af3-135">hello schema for a certificate authority looks as follows:</span></span> 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 

<span data-ttu-id="d3af3-136">Pro konfiguraci hello, můžete použít hello [Azure Active Directory PowerShell verze 2](/powershell/azure/install-adv2?view=azureadps-2.0):</span><span class="sxs-lookup"><span data-stu-id="d3af3-136">For hello configuration, you can use hello [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0):</span></span>  

1. <span data-ttu-id="d3af3-137">Spusťte prostředí Windows PowerShell s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="d3af3-137">Start Windows PowerShell with administrator privileges.</span></span> 
2. <span data-ttu-id="d3af3-138">Nainstalujte modul hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d3af3-138">Install hello Azure AD module.</span></span> <span data-ttu-id="d3af3-139">Je třeba tooinstall verze [2.0.0.33 ](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="d3af3-139">You need tooinstall Version [2.0.0.33 ](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) or higher.</span></span>  
   
        Install-Module -Name AzureAD –RequiredVersion 2.0.0.33 

<span data-ttu-id="d3af3-140">Jako první krok konfigurace je nutné tooestablish připojení s vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="d3af3-140">As a first configuration step, you need tooestablish a connection with your tenant.</span></span> <span data-ttu-id="d3af3-141">Při připojení klienta tooyour existuje, můžete zkontrolovat, přidat, odstranit a upravit hello důvěryhodných certifikačních autorit, které jsou definované ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="d3af3-141">As soon as a connection tooyour tenant exists, you can review, add, delete and modify hello trusted certificate authorities that are defined in your directory.</span></span> 

### <a name="connect"></a><span data-ttu-id="d3af3-142">Připojení</span><span class="sxs-lookup"><span data-stu-id="d3af3-142">Connect</span></span>

<span data-ttu-id="d3af3-143">tooestablish připojení se váš klient, použijte hello [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) rutiny:</span><span class="sxs-lookup"><span data-stu-id="d3af3-143">tooestablish a connection with your tenant, use hello [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) cmdlet:</span></span>

    Connect-AzureAD 


### <a name="retrieve"></a><span data-ttu-id="d3af3-144">Načtení</span><span class="sxs-lookup"><span data-stu-id="d3af3-144">Retrieve</span></span> 

<span data-ttu-id="d3af3-145">tooretrieve hello důvěryhodných certifikačních autorit, které jsou definované ve vašem adresáři používat hello [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) rutiny.</span><span class="sxs-lookup"><span data-stu-id="d3af3-145">tooretrieve hello trusted certificate authorities that are defined in your directory, use hello [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet.</span></span> 

    Get-AzureADTrustedCertificateAuthority 
 

### <a name="add"></a><span data-ttu-id="d3af3-146">Přidat</span><span class="sxs-lookup"><span data-stu-id="d3af3-146">Add</span></span>

<span data-ttu-id="d3af3-147">toocreate důvěryhodné certifikační autority, použití hello [New-AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) rutiny a sadu hello **crlDistributionPoint** atribut tooa správnou hodnotu:</span><span class="sxs-lookup"><span data-stu-id="d3af3-147">toocreate a trusted certificate authority, use hello [New-AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet and set hello **crlDistributionPoint** attribute tooa correct value:</span></span> 
   
    $cert=Get-Content -Encoding byte "[LOCATION OF hello CER FILE]" 
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
    $new_ca.AuthorityType=0 
    $new_ca.TrustedCertificate=$cert 
    $new_ca.crlDistributionPoint=”<CRL Distribution URL>”
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 


### <a name="remove"></a><span data-ttu-id="d3af3-148">Odebrat</span><span class="sxs-lookup"><span data-stu-id="d3af3-148">Remove</span></span>

<span data-ttu-id="d3af3-149">tooremove důvěryhodné certifikační autority, použití hello [odebrat AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) rutiny:</span><span class="sxs-lookup"><span data-stu-id="d3af3-149">tooremove a trusted certificate authority, use hello [Remove-AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:</span></span>
   
    $c=Get-AzureADTrustedCertificateAuthority 
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiy"></a><span data-ttu-id="d3af3-150">Modfiy</span><span class="sxs-lookup"><span data-stu-id="d3af3-150">Modfiy</span></span>

<span data-ttu-id="d3af3-151">toomodify důvěryhodné certifikační autority, použití hello [Set-AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) rutiny:</span><span class="sxs-lookup"><span data-stu-id="d3af3-151">toomodify a trusted certificate authority, use hello [Set-AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:</span></span>

    $c=Get-AzureADTrustedCertificateAuthority 
    $c[0].AuthorityType=1 
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 


## <a name="step-3-configure-revocation"></a><span data-ttu-id="d3af3-152">Krok 3: Konfigurace odvolání</span><span class="sxs-lookup"><span data-stu-id="d3af3-152">Step 3: Configure revocation</span></span>

<span data-ttu-id="d3af3-153">toorevoke klientský certifikát, Azure Active Directory načte certifikát hello seznam odvolaných certifikátů (CRL) z adresy URL hello odesílané jako součást informace o certifikační autoritě a zapíše do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d3af3-153">toorevoke a client certificate, Azure Active Directory fetches hello certificate revocation list (CRL) from hello URLs uploaded as part of certificate authority information and caches it.</span></span> <span data-ttu-id="d3af3-154">Hello publikování poslední časové razítko (**datum účinnosti** vlastnost) v seznamu CRL se používá hello tooensure hello CRL je stále platný.</span><span class="sxs-lookup"><span data-stu-id="d3af3-154">hello last publish timestamp (**Effective Date** property) in hello CRL is used tooensure hello CRL is still valid.</span></span> <span data-ttu-id="d3af3-155">Hello CRL je pravidelně odkazované toorevoke toocertificates přístupu, které jsou součástí hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="d3af3-155">hello CRL is periodically referenced toorevoke access toocertificates that are a part of hello list.</span></span>

<span data-ttu-id="d3af3-156">Pokud více rychlých odvolání je potřeba (například pokud uživatel ztratí zařízení), můžete zrušena hello autorizační token uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="d3af3-156">If a more instant revocation is required (for example, if a user loses a device), hello authorization token of hello user can be invalidated.</span></span> <span data-ttu-id="d3af3-157">tooinvalidate hello autorizační token, nastavte hello **StsRefreshTokenValidFrom** pole pro tento konkrétní uživatel pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d3af3-157">tooinvalidate hello authorization token, set hello **StsRefreshTokenValidFrom** field for this particular user using Windows PowerShell.</span></span> <span data-ttu-id="d3af3-158">Je nutné aktualizovat hello **StsRefreshTokenValidFrom** pole pro každého uživatele chcete toorevoke přístup.</span><span class="sxs-lookup"><span data-stu-id="d3af3-158">You must update hello **StsRefreshTokenValidFrom** field for each user you want toorevoke access for.</span></span>

<span data-ttu-id="d3af3-159">tooensure, která je uchována hello odvolání, je nutné nastavit hello **datum účinnosti** hello CRL tooa datum po hello hodnotu, která nastavuje **StsRefreshTokenValidFrom** a ověřte hello certifikátu dotyčném v Hello seznamu CRL.</span><span class="sxs-lookup"><span data-stu-id="d3af3-159">tooensure that hello revocation persists, you must set hello **Effective Date** of hello CRL tooa date after hello value set by **StsRefreshTokenValidFrom** and ensure hello certificate in question is in hello CRL.</span></span>

<span data-ttu-id="d3af3-160">Dobrý den, následující kroky outline hello proces pro aktualizaci nebo zneplatnění hello autorizační token podle nastavení hello **StsRefreshTokenValidFrom** pole.</span><span class="sxs-lookup"><span data-stu-id="d3af3-160">hello following steps outline hello process for updating and invalidating hello authorization token by setting hello **StsRefreshTokenValidFrom** field.</span></span> 

<span data-ttu-id="d3af3-161">**odvolání tooconfigure:**</span><span class="sxs-lookup"><span data-stu-id="d3af3-161">**tooconfigure revocation:**</span></span> 

1. <span data-ttu-id="d3af3-162">Připojte službou MSOL toohello přihlašovací údaje správce:</span><span class="sxs-lookup"><span data-stu-id="d3af3-162">Connect with admin credentials toohello MSOL service:</span></span> 
   
        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

2. <span data-ttu-id="d3af3-163">Načtěte hello aktuální StsRefreshTokensValidFrom hodnotu pro uživatele:</span><span class="sxs-lookup"><span data-stu-id="d3af3-163">Retrieve hello current StsRefreshTokensValidFrom value for a user:</span></span> 
   
        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 

3. <span data-ttu-id="d3af3-164">Nakonfigurujte novou hodnotu StsRefreshTokensValidFrom pro aktuální časové razítko aplikace hello uživatele stejná toohello:</span><span class="sxs-lookup"><span data-stu-id="d3af3-164">Configure a new StsRefreshTokensValidFrom value for hello user equal toohello current timestamp:</span></span> 
   
        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

<span data-ttu-id="d3af3-165">Hello datum, které nastavíte, musí být v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="d3af3-165">hello date you set must be in hello future.</span></span> <span data-ttu-id="d3af3-166">Pokud není datum hello v hello budoucí, hello **StsRefreshTokensValidFrom** není nastavena vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d3af3-166">If hello date is not in hello future, hello **StsRefreshTokensValidFrom** property is not set.</span></span> <span data-ttu-id="d3af3-167">Pokud je datum hello v hello budoucí, **StsRefreshTokensValidFrom** nastavena toohello aktuální čas (ne. datum hello uvedené pomocí příkazu Set-MsolUser).</span><span class="sxs-lookup"><span data-stu-id="d3af3-167">If hello date is in hello future, **StsRefreshTokensValidFrom** is set toohello current time (not hello date indicated by Set-MsolUser command).</span></span> 


## <a name="step-4-test-your-configuration"></a><span data-ttu-id="d3af3-168">Krok 4: Testování konfigurace</span><span class="sxs-lookup"><span data-stu-id="d3af3-168">Step 4: Test your configuration</span></span>

### <a name="testing-your-certificate"></a><span data-ttu-id="d3af3-169">Testování vašeho certifikátu</span><span class="sxs-lookup"><span data-stu-id="d3af3-169">Testing your certificate</span></span>

<span data-ttu-id="d3af3-170">Jako první test konfigurace, že byste měli zkusit toosign v příliš[Outlook Web Access](https://outlook.office365.com) nebo [SharePoint Online](https://microsoft.sharepoint.com) pomocí vaší **prohlížeč na zařízení**.</span><span class="sxs-lookup"><span data-stu-id="d3af3-170">As a first configuration test, you should try toosign in too[Outlook Web Access](https://outlook.office365.com) or [SharePoint Online](https://microsoft.sharepoint.com) using your **on-device browser**.</span></span>

<span data-ttu-id="d3af3-171">Pokud vaše přihlášení úspěšné, pak víte, že:</span><span class="sxs-lookup"><span data-stu-id="d3af3-171">If your sign-in is successful, then you know that:</span></span>

- <span data-ttu-id="d3af3-172">Hello uživatelský certifikát byl zřízené tooyour testovací zařízení</span><span class="sxs-lookup"><span data-stu-id="d3af3-172">hello user certificate has been provisioned tooyour test device</span></span>
- <span data-ttu-id="d3af3-173">Služba AD FS je správně nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="d3af3-173">AD FS is configured correctly</span></span>  


### <a name="testing-office-mobile-applications"></a><span data-ttu-id="d3af3-174">Testování mobilních aplikacích Office</span><span class="sxs-lookup"><span data-stu-id="d3af3-174">Testing Office mobile applications</span></span>

<span data-ttu-id="d3af3-175">**tootest ověřování pomocí certifikátů na vaše mobilní aplikace Office:**</span><span class="sxs-lookup"><span data-stu-id="d3af3-175">**tootest certificate-based authentication on your mobile Office application:**</span></span> 

1. <span data-ttu-id="d3af3-176">Na testovací zařízení nainstalujte mobilní aplikace Office (například OneDrive).</span><span class="sxs-lookup"><span data-stu-id="d3af3-176">On your test device, install an Office mobile application (e.g., OneDrive).</span></span>
3. <span data-ttu-id="d3af3-177">Spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="d3af3-177">Launch hello application.</span></span> 
4. <span data-ttu-id="d3af3-178">Zadejte uživatelské jméno a potom vyberte hello uživatelský certifikát, že který má toouse.</span><span class="sxs-lookup"><span data-stu-id="d3af3-178">Enter your user name, and then select hello user certificate you want toouse.</span></span> 

<span data-ttu-id="d3af3-179">Můžete by měla být úspěšně přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="d3af3-179">You should be successfully signed in.</span></span> 

### <a name="testing-exchange-activesync-client-applications"></a><span data-ttu-id="d3af3-180">Testování aplikací klienta Exchange ActiveSync</span><span class="sxs-lookup"><span data-stu-id="d3af3-180">Testing Exchange ActiveSync client applications</span></span>

<span data-ttu-id="d3af3-181">tooaccess Exchange ActiveSync (EAS) prostřednictvím ověřování pomocí certifikátů, profilem EAS obsahující hello klientský certifikát musí být k dispozici toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3af3-181">tooaccess Exchange ActiveSync (EAS) via certificate-based authentication, an EAS profile containing hello client certificate must be available toohello application.</span></span> 

<span data-ttu-id="d3af3-182">Hello profilu EAS, musí obsahovat hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="d3af3-182">hello EAS profile must contain hello following information:</span></span>

- <span data-ttu-id="d3af3-183">Hello toobe uživatele certifikátu používaného pro ověřování</span><span class="sxs-lookup"><span data-stu-id="d3af3-183">hello user certificate toobe used for authentication</span></span> 

- <span data-ttu-id="d3af3-184">koncový bod EAS Hello (například outlook.office365.com)</span><span class="sxs-lookup"><span data-stu-id="d3af3-184">hello EAS endpoint (for example, outlook.office365.com)</span></span>

<span data-ttu-id="d3af3-185">Profilu EAS můžete nakonfigurovat a vztahujících se na zařízení hello prostřednictvím využití hello správy mobilních zařízení (MDM) jako je například Intune nebo tím, že ručně certifikát hello hello profilu EAS hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3af3-185">An EAS profile can be configured and placed on hello device through hello utilization of Mobile device management (MDM) such as Intune or by manually placing hello certificate in hello EAS profile on hello device.</span></span>  

### <a name="testing-eas-client-applications-on-android"></a><span data-ttu-id="d3af3-186">Testování EAS klientské aplikace v systému Android</span><span class="sxs-lookup"><span data-stu-id="d3af3-186">Testing EAS client applications on Android</span></span>

<span data-ttu-id="d3af3-187">**ověřování pomocí certifikátu tootest:**</span><span class="sxs-lookup"><span data-stu-id="d3af3-187">**tootest certificate authentication:**</span></span>  

1. <span data-ttu-id="d3af3-188">Konfigurace profilu EAS hello aplikace, který splňuje požadavky hello výše.</span><span class="sxs-lookup"><span data-stu-id="d3af3-188">Configure an EAS profile in hello application that satisfies hello requirements above.</span></span>  
2. <span data-ttu-id="d3af3-189">Otevřete aplikaci hello a ověřte, zda je synchronizace e-mailu.</span><span class="sxs-lookup"><span data-stu-id="d3af3-189">Open hello application, and verify that mail is synchronizing.</span></span> 

