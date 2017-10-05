---
title: "Azure Active Directory založené na certifikátech ověřování – Začínáme | Microsoft Docs"
description: "Informace o konfiguraci ověřování pomocí certifikátů ve vašem prostředí"
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
ms.openlocfilehash: 8ebc6f2dd7502fd75ffdd4d5d68338382cb1a46b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a><span data-ttu-id="4b822-103">Začínáme s ověřováním na základě certifikátů ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4b822-103">Get started with certificate-based authentication in Azure Active Directory</span></span>

<span data-ttu-id="4b822-104">Ověřování pomocí certifikátů umožňuje službou Azure Active Directory se ověřit klientský certifikát na zařízení s Windows, Android nebo iOS při připojování váš účet systému Exchange online:</span><span class="sxs-lookup"><span data-stu-id="4b822-104">Certificate-based authentication enables you to be authenticated by Azure Active Directory with a client certificate on a Windows, Android or iOS device when connecting your Exchange online account to:</span></span> 

- <span data-ttu-id="4b822-105">Mobilní aplikace Office, jako je například Microsoft Outlook a Microsoft Word</span><span class="sxs-lookup"><span data-stu-id="4b822-105">Office mobile applications such as Microsoft Outlook and Microsoft Word</span></span>   

- <span data-ttu-id="4b822-106">Klienti Exchange ActiveSync (EAS)</span><span class="sxs-lookup"><span data-stu-id="4b822-106">Exchange ActiveSync (EAS) clients</span></span> 

<span data-ttu-id="4b822-107">Konfigurace tato funkce eliminuje potřebu zadejte kombinace uživatelského jména a hesla do určité e-mailu a aplikace Microsoft Office na vašem mobilním zařízení.</span><span class="sxs-lookup"><span data-stu-id="4b822-107">Configuring this feature eliminates the need to enter a username and password combination into certain mail and Microsoft Office applications on your mobile device.</span></span> 

<span data-ttu-id="4b822-108">V tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="4b822-108">This topic:</span></span>

- <span data-ttu-id="4b822-109">Poskytuje postup pro konfiguraci a využít ověřování pomocí certifikátů pro uživatele klientů v rámci plánů Office 365 Enterprise, Business, Education a US Government.</span><span class="sxs-lookup"><span data-stu-id="4b822-109">Provides you with the steps to configure and utilize certificate-based authentication for users of tenants in Office 365 Enterprise, Business, Education, and US Government plans.</span></span> <span data-ttu-id="4b822-110">Tato funkce je dostupná ve verzi preview v Číně Office 365, US Government obrany a US Government Federal plány.</span><span class="sxs-lookup"><span data-stu-id="4b822-110">This feature is available in preview in Office 365 China, US Government Defense, and US Government Federal plans.</span></span> 

- <span data-ttu-id="4b822-111">Předpokládá, že již máte [infrastruktury veřejných klíčů (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) a [služby AD FS](connect/active-directory-aadconnectfed-whatis.md) nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="4b822-111">Assumes that you already have a [public key infrastructure (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) and [AD FS](connect/active-directory-aadconnectfed-whatis.md) configured.</span></span>    


## <a name="requirements"></a><span data-ttu-id="4b822-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4b822-112">Requirements</span></span>

<span data-ttu-id="4b822-113">Chcete-li nakonfigurovat ověřování pomocí certifikátů, musí být splněné následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="4b822-113">To configure certificate-based authentication, the following must be true:</span></span>  

- <span data-ttu-id="4b822-114">Ověřování pomocí certifikátů (CBA) je podporována pouze pro federovaném prostředí pro aplikace prohlížeče nebo nativní klienty, kteří používají moderní ověřování (ADAL).</span><span class="sxs-lookup"><span data-stu-id="4b822-114">Certificate-based authentication (CBA) is only supported for Federated environments for browser applications or native clients using modern authentication (ADAL).</span></span> <span data-ttu-id="4b822-115">Jedinou výjimkou je Exchange Active Sync (EAS) pro EXO, který můžete použít pro federované i spravované účty.</span><span class="sxs-lookup"><span data-stu-id="4b822-115">The one exception is Exchange Active Sync (EAS) for EXO which can be used for both, federated and managed accounts.</span></span> 

- <span data-ttu-id="4b822-116">Kořenové certifikační autority a jakékoliv zprostředkující certifikační autority musí být nakonfigurované v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4b822-116">The root certificate authority and any intermediate certificate authorities must be configured in Azure Active Directory.</span></span>  

- <span data-ttu-id="4b822-117">Seznam odvolaných certifikátů (CRL), může být odkazováno prostřednictvím internetové adresy URL musí mít každý certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="4b822-117">Each certificate authority must have a certificate revocation list (CRL) that can be referenced via an Internet facing URL.</span></span>  

- <span data-ttu-id="4b822-118">Musí mít alespoň jednu certifikační autoritu nakonfigurován v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4b822-118">You must have at least one certificate authority configured in Azure Active Directory.</span></span> <span data-ttu-id="4b822-119">Související kroky v najdete [konfigurace certifikačních autorit](#step-2-configure-the-certificate-authorities) části.</span><span class="sxs-lookup"><span data-stu-id="4b822-119">You can find related steps in the [Configure the certificate authorities](#step-2-configure-the-certificate-authorities) section.</span></span>  

- <span data-ttu-id="4b822-120">U klientů Exchange ActiveSync klienta musí mít certifikát směrovatelné e-mailovou adresu uživatele v systému Exchange online v hlavní název nebo název RFC822 hodnota pole alternativní název subjektu.</span><span class="sxs-lookup"><span data-stu-id="4b822-120">For Exchange ActiveSync clients, the client certificate must have the user’s routable email address in Exchange online in either the Principal Name or the RFC822 Name value of the Subject Alternative Name field.</span></span> <span data-ttu-id="4b822-121">Azure Active Directory mapuje RFC822 hodnota atributu adresu proxy serveru v adresáři.</span><span class="sxs-lookup"><span data-stu-id="4b822-121">Azure Active Directory maps the RFC822 value to the Proxy Address attribute in the directory.</span></span>  

- <span data-ttu-id="4b822-122">Klientské zařízení musí mít přístup k alespoň jednu certifikační autority, která vydává certifikáty klienta.</span><span class="sxs-lookup"><span data-stu-id="4b822-122">Your client device must have access to at least one certificate authority that issues client certificates.</span></span>  

- <span data-ttu-id="4b822-123">Klientský certifikát pro ověřování klientů musí být vydán pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="4b822-123">A client certificate for client authentication must have been issued to your client.</span></span>  




## <a name="step-1-select-your-device-platform"></a><span data-ttu-id="4b822-124">Krok 1: Vyberte platformu zařízení</span><span class="sxs-lookup"><span data-stu-id="4b822-124">Step 1: Select your device platform</span></span>

<span data-ttu-id="4b822-125">Jako první krok pro platformu zařízení, která vás budete muset zkontrolujte následující položky:</span><span class="sxs-lookup"><span data-stu-id="4b822-125">As a first step, for the device platform you care about, you need to review the following:</span></span>

- <span data-ttu-id="4b822-126">Podpora mobilních aplikacích Office</span><span class="sxs-lookup"><span data-stu-id="4b822-126">The Office mobile applications support</span></span> 
- <span data-ttu-id="4b822-127">Požadavky na konkrétní implementace</span><span class="sxs-lookup"><span data-stu-id="4b822-127">The specific implementation requirements</span></span>  

<span data-ttu-id="4b822-128">Pro tyto platformy zařízení existuje související informace:</span><span class="sxs-lookup"><span data-stu-id="4b822-128">The related information exists for the following device platforms:</span></span>

- [<span data-ttu-id="4b822-129">Android</span><span class="sxs-lookup"><span data-stu-id="4b822-129">Android</span></span>](active-directory-certificate-based-authentication-android.md)
- [<span data-ttu-id="4b822-130">iOS</span><span class="sxs-lookup"><span data-stu-id="4b822-130">iOS</span></span>](active-directory-certificate-based-authentication-ios.md)


## <a name="step-2-configure-the-certificate-authorities"></a><span data-ttu-id="4b822-131">Krok 2: Konfigurace certifikačních autorit</span><span class="sxs-lookup"><span data-stu-id="4b822-131">Step 2: Configure the certificate authorities</span></span> 

<span data-ttu-id="4b822-132">Konfigurace certifikačních autorit ve službě Azure Active Directory, pro každý certifikační autority, odešlete následující soubory:</span><span class="sxs-lookup"><span data-stu-id="4b822-132">To configure your certificate authorities in Azure Active Directory, for each certificate authority, upload the following:</span></span> 

* <span data-ttu-id="4b822-133">Na server veřejnou část certifikátu v *.cer* formátu</span><span class="sxs-lookup"><span data-stu-id="4b822-133">The public portion of the certificate, in *.cer* format</span></span> 
* <span data-ttu-id="4b822-134">Internetový bod adresy URL, kde se nacházejí seznamy odvolaných certifikátů (CRL)</span><span class="sxs-lookup"><span data-stu-id="4b822-134">The Internet facing URLs where the Certificate Revocation Lists (CRLs) reside</span></span>

<span data-ttu-id="4b822-135">Schéma pro certifikační autority vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="4b822-135">The schema for a certificate authority looks as follows:</span></span> 

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

<span data-ttu-id="4b822-136">Pro konfiguraci, můžete použít [Azure Active Directory PowerShell verze 2](/powershell/azure/install-adv2?view=azureadps-2.0):</span><span class="sxs-lookup"><span data-stu-id="4b822-136">For the configuration, you can use the [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0):</span></span>  

1. <span data-ttu-id="4b822-137">Spusťte prostředí Windows PowerShell s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="4b822-137">Start Windows PowerShell with administrator privileges.</span></span> 
2. <span data-ttu-id="4b822-138">Instalace modulu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4b822-138">Install the Azure AD module.</span></span> <span data-ttu-id="4b822-139">Je potřeba nainstalovat verzi [2.0.0.33 ](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="4b822-139">You need to install Version [2.0.0.33 ](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) or higher.</span></span>  
   
        Install-Module -Name AzureAD –RequiredVersion 2.0.0.33 

<span data-ttu-id="4b822-140">Jako první krok konfigurace budete muset navázat spojení se váš klient.</span><span class="sxs-lookup"><span data-stu-id="4b822-140">As a first configuration step, you need to establish a connection with your tenant.</span></span> <span data-ttu-id="4b822-141">Při připojení ke klientovi existuje, můžete zkontrolovat, přidat, odstranit a upravit důvěryhodných certifikačních autorit, které jsou definované ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="4b822-141">As soon as a connection to your tenant exists, you can review, add, delete and modify the trusted certificate authorities that are defined in your directory.</span></span> 

### <a name="connect"></a><span data-ttu-id="4b822-142">Připojení</span><span class="sxs-lookup"><span data-stu-id="4b822-142">Connect</span></span>

<span data-ttu-id="4b822-143">Chcete-li navázat spojení se váš klient, použijte [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) rutiny:</span><span class="sxs-lookup"><span data-stu-id="4b822-143">To establish a connection with your tenant, use the [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) cmdlet:</span></span>

    Connect-AzureAD 


### <a name="retrieve"></a><span data-ttu-id="4b822-144">Načtení</span><span class="sxs-lookup"><span data-stu-id="4b822-144">Retrieve</span></span> 

<span data-ttu-id="4b822-145">Chcete-li načíst důvěryhodných certifikačních autorit, které jsou definovány v adresáři, použijte [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) rutiny.</span><span class="sxs-lookup"><span data-stu-id="4b822-145">To retrieve the trusted certificate authorities that are defined in your directory, use the [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet.</span></span> 

    Get-AzureADTrustedCertificateAuthority 
 

### <a name="add"></a><span data-ttu-id="4b822-146">Přidat</span><span class="sxs-lookup"><span data-stu-id="4b822-146">Add</span></span>

<span data-ttu-id="4b822-147">Chcete-li vytvořit důvěryhodné certifikační autority, použijte [New-AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) rutiny a nastavte **crlDistributionPoint** správnou hodnotu atributu:</span><span class="sxs-lookup"><span data-stu-id="4b822-147">To create a trusted certificate authority, use the [New-AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet and set the **crlDistributionPoint** attribute to a correct value:</span></span> 
   
    $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
    $new_ca.AuthorityType=0 
    $new_ca.TrustedCertificate=$cert 
    $new_ca.crlDistributionPoint=”<CRL Distribution URL>”
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 


### <a name="remove"></a><span data-ttu-id="4b822-148">Odebrat</span><span class="sxs-lookup"><span data-stu-id="4b822-148">Remove</span></span>

<span data-ttu-id="4b822-149">Chcete-li odebrat důvěryhodné certifikační autority, použijte [odebrat AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) rutiny:</span><span class="sxs-lookup"><span data-stu-id="4b822-149">To remove a trusted certificate authority, use the [Remove-AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:</span></span>
   
    $c=Get-AzureADTrustedCertificateAuthority 
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiy"></a><span data-ttu-id="4b822-150">Modfiy</span><span class="sxs-lookup"><span data-stu-id="4b822-150">Modfiy</span></span>

<span data-ttu-id="4b822-151">Chcete-li upravit důvěryhodné certifikační autority, použijte [Set-AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) rutiny:</span><span class="sxs-lookup"><span data-stu-id="4b822-151">To modify a trusted certificate authority, use the [Set-AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:</span></span>

    $c=Get-AzureADTrustedCertificateAuthority 
    $c[0].AuthorityType=1 
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 


## <a name="step-3-configure-revocation"></a><span data-ttu-id="4b822-152">Krok 3: Konfigurace odvolání</span><span class="sxs-lookup"><span data-stu-id="4b822-152">Step 3: Configure revocation</span></span>

<span data-ttu-id="4b822-153">Azure Active Directory k odvolání certifikátu klienta, načte seznam odvolaných certifikátů (CRL) z adresy URL odesílané jako součást informace o certifikační autoritě a zapíše do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="4b822-153">To revoke a client certificate, Azure Active Directory fetches the certificate revocation list (CRL) from the URLs uploaded as part of certificate authority information and caches it.</span></span> <span data-ttu-id="4b822-154">Poslední publikovat časové razítko (**datum účinnosti** vlastnost) v seznamu CRL, které se používá k zajištění seznam CRL je stále platný.</span><span class="sxs-lookup"><span data-stu-id="4b822-154">The last publish timestamp (**Effective Date** property) in the CRL is used to ensure the CRL is still valid.</span></span> <span data-ttu-id="4b822-155">Seznam CRL odkazuje pravidelně odvolat přístup k certifikáty, které jsou součástí seznamu.</span><span class="sxs-lookup"><span data-stu-id="4b822-155">The CRL is periodically referenced to revoke access to certificates that are a part of the list.</span></span>

<span data-ttu-id="4b822-156">Pokud více rychlých odvolání je potřeba (například pokud uživatel ztratí zařízení), můžete zrušena autorizační token uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b822-156">If a more instant revocation is required (for example, if a user loses a device), the authorization token of the user can be invalidated.</span></span> <span data-ttu-id="4b822-157">Zneplatní autorizačním tokenem, nastavte **StsRefreshTokenValidFrom** pole pro tento konkrétní uživatel pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4b822-157">To invalidate the authorization token, set the **StsRefreshTokenValidFrom** field for this particular user using Windows PowerShell.</span></span> <span data-ttu-id="4b822-158">Je nutné aktualizovat **StsRefreshTokenValidFrom** pole pro každého uživatele, který chcete odvolat přístup.</span><span class="sxs-lookup"><span data-stu-id="4b822-158">You must update the **StsRefreshTokenValidFrom** field for each user you want to revoke access for.</span></span>

<span data-ttu-id="4b822-159">Chcete, aby odvolání potrvají, musíte nastavit **datum účinnosti** seznamu CRL na datum po hodnotu, která nastavuje **StsRefreshTokenValidFrom** a ujistěte se, daný certifikát je v seznamu CRL.</span><span class="sxs-lookup"><span data-stu-id="4b822-159">To ensure that the revocation persists, you must set the **Effective Date** of the CRL to a date after the value set by **StsRefreshTokenValidFrom** and ensure the certificate in question is in the CRL.</span></span>

<span data-ttu-id="4b822-160">Následující kroky popisují proces pro aktualizaci nebo zneplatnění autorizační token nastavením **StsRefreshTokenValidFrom** pole.</span><span class="sxs-lookup"><span data-stu-id="4b822-160">The following steps outline the process for updating and invalidating the authorization token by setting the **StsRefreshTokenValidFrom** field.</span></span> 

<span data-ttu-id="4b822-161">**Postup konfigurace odvolání:**</span><span class="sxs-lookup"><span data-stu-id="4b822-161">**To configure revocation:**</span></span> 

1. <span data-ttu-id="4b822-162">Připojte se pomocí přihlašovacích údajů správce ke službě MSOL:</span><span class="sxs-lookup"><span data-stu-id="4b822-162">Connect with admin credentials to the MSOL service:</span></span> 
   
        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

2. <span data-ttu-id="4b822-163">Načtěte aktuální hodnotu StsRefreshTokensValidFrom pro uživatele:</span><span class="sxs-lookup"><span data-stu-id="4b822-163">Retrieve the current StsRefreshTokensValidFrom value for a user:</span></span> 
   
        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 

3. <span data-ttu-id="4b822-164">Nakonfigurujte novou hodnotu StsRefreshTokensValidFrom pro uživatele, který se rovná aktuální časové razítko:</span><span class="sxs-lookup"><span data-stu-id="4b822-164">Configure a new StsRefreshTokensValidFrom value for the user equal to the current timestamp:</span></span> 
   
        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

<span data-ttu-id="4b822-165">Datum, které nastavíte, musí být v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="4b822-165">The date you set must be in the future.</span></span> <span data-ttu-id="4b822-166">Pokud není datum v budoucnosti, **StsRefreshTokensValidFrom** není nastavena vlastnost.</span><span class="sxs-lookup"><span data-stu-id="4b822-166">If the date is not in the future, the **StsRefreshTokensValidFrom** property is not set.</span></span> <span data-ttu-id="4b822-167">Pokud je datum v budoucnosti **StsRefreshTokensValidFrom** je nastaven na aktuální čas (není datum uvedené pomocí příkazu Set-MsolUser).</span><span class="sxs-lookup"><span data-stu-id="4b822-167">If the date is in the future, **StsRefreshTokensValidFrom** is set to the current time (not the date indicated by Set-MsolUser command).</span></span> 


## <a name="step-4-test-your-configuration"></a><span data-ttu-id="4b822-168">Krok 4: Testování konfigurace</span><span class="sxs-lookup"><span data-stu-id="4b822-168">Step 4: Test your configuration</span></span>

### <a name="testing-your-certificate"></a><span data-ttu-id="4b822-169">Testování vašeho certifikátu</span><span class="sxs-lookup"><span data-stu-id="4b822-169">Testing your certificate</span></span>

<span data-ttu-id="4b822-170">Jako první test konfigurace, pokuste se přihlásit k [Outlook Web Access](https://outlook.office365.com) nebo [SharePoint Online](https://microsoft.sharepoint.com) pomocí vaší **prohlížeč na zařízení**.</span><span class="sxs-lookup"><span data-stu-id="4b822-170">As a first configuration test, you should try to sign in to [Outlook Web Access](https://outlook.office365.com) or [SharePoint Online](https://microsoft.sharepoint.com) using your **on-device browser**.</span></span>

<span data-ttu-id="4b822-171">Pokud vaše přihlášení úspěšné, pak víte, že:</span><span class="sxs-lookup"><span data-stu-id="4b822-171">If your sign-in is successful, then you know that:</span></span>

- <span data-ttu-id="4b822-172">Uživatelský certifikát zřízená testovací zařízení</span><span class="sxs-lookup"><span data-stu-id="4b822-172">The user certificate has been provisioned to your test device</span></span>
- <span data-ttu-id="4b822-173">Služba AD FS je správně nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="4b822-173">AD FS is configured correctly</span></span>  


### <a name="testing-office-mobile-applications"></a><span data-ttu-id="4b822-174">Testování mobilních aplikacích Office</span><span class="sxs-lookup"><span data-stu-id="4b822-174">Testing Office mobile applications</span></span>

<span data-ttu-id="4b822-175">**K testování ověřování pomocí certifikátů na vaše mobilní aplikace Office:**</span><span class="sxs-lookup"><span data-stu-id="4b822-175">**To test certificate-based authentication on your mobile Office application:**</span></span> 

1. <span data-ttu-id="4b822-176">Na testovací zařízení nainstalujte mobilní aplikace Office (například OneDrive).</span><span class="sxs-lookup"><span data-stu-id="4b822-176">On your test device, install an Office mobile application (e.g., OneDrive).</span></span>
3. <span data-ttu-id="4b822-177">Spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="4b822-177">Launch the application.</span></span> 
4. <span data-ttu-id="4b822-178">Zadejte uživatelské jméno a potom vyberte certifikát uživatele, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="4b822-178">Enter your user name, and then select the user certificate you want to use.</span></span> 

<span data-ttu-id="4b822-179">Můžete by měla být úspěšně přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="4b822-179">You should be successfully signed in.</span></span> 

### <a name="testing-exchange-activesync-client-applications"></a><span data-ttu-id="4b822-180">Testování aplikací klienta Exchange ActiveSync</span><span class="sxs-lookup"><span data-stu-id="4b822-180">Testing Exchange ActiveSync client applications</span></span>

<span data-ttu-id="4b822-181">Pro přístup k Exchange ActiveSync (EAS) prostřednictvím ověřování pomocí certifikátů, musí být k dispozici pro aplikaci profilem EAS obsahující certifikát klienta.</span><span class="sxs-lookup"><span data-stu-id="4b822-181">To access Exchange ActiveSync (EAS) via certificate-based authentication, an EAS profile containing the client certificate must be available to the application.</span></span> 

<span data-ttu-id="4b822-182">Profilu EAS, musí obsahovat tyto informace:</span><span class="sxs-lookup"><span data-stu-id="4b822-182">The EAS profile must contain the following information:</span></span>

- <span data-ttu-id="4b822-183">Uživatelský certifikát má být použit pro ověřování</span><span class="sxs-lookup"><span data-stu-id="4b822-183">The user certificate to be used for authentication</span></span> 

- <span data-ttu-id="4b822-184">Koncový bod EAS (například outlook.office365.com)</span><span class="sxs-lookup"><span data-stu-id="4b822-184">The EAS endpoint (for example, outlook.office365.com)</span></span>

<span data-ttu-id="4b822-185">Profilu EAS můžete nakonfigurovat a umístit na zařízení prostřednictvím využití správy mobilních zařízení (MDM) jako je například Intune nebo ručně umístění certifikátu v profilu EAS na zařízení.</span><span class="sxs-lookup"><span data-stu-id="4b822-185">An EAS profile can be configured and placed on the device through the utilization of Mobile device management (MDM) such as Intune or by manually placing the certificate in the EAS profile on the device.</span></span>  

### <a name="testing-eas-client-applications-on-android"></a><span data-ttu-id="4b822-186">Testování EAS klientské aplikace v systému Android</span><span class="sxs-lookup"><span data-stu-id="4b822-186">Testing EAS client applications on Android</span></span>

<span data-ttu-id="4b822-187">**K testování ověřování pomocí certifikátu:**</span><span class="sxs-lookup"><span data-stu-id="4b822-187">**To test certificate authentication:**</span></span>  

1. <span data-ttu-id="4b822-188">Konfigurace profilu EAS v aplikaci, která splňuje požadavky na výše.</span><span class="sxs-lookup"><span data-stu-id="4b822-188">Configure an EAS profile in the application that satisfies the requirements above.</span></span>  
2. <span data-ttu-id="4b822-189">Otevřete aplikaci a ověřte, zda je synchronizace e-mailu.</span><span class="sxs-lookup"><span data-stu-id="4b822-189">Open the application, and verify that mail is synchronizing.</span></span> 

