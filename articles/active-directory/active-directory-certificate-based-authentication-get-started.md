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
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a>Začínáme s ověřováním na základě certifikátů ve službě Azure Active Directory

Ověřování pomocí certifikátů umožňuje toobe službou Azure Active Directory se ověřit klientský certifikát na zařízení s Windows, Android nebo iOS při připojování váš účet systému Exchange online: 

- Mobilní aplikace Office, jako je například Microsoft Outlook a Microsoft Word   

- Klienti Exchange ActiveSync (EAS) 

Konfigurace tato funkce eliminuje hello nutné tooenter uživatelské jméno a heslo kombinaci do určité e-mailu a aplikace Microsoft Office na vašem mobilním zařízení. 

V tomto tématu:

- Poskytuje vám hello kroky tooconfigure a využívat ověřování pomocí certifikátů pro uživatele klientů Office 365 Enterprise, Education a obchodní a plány vládou USA. Tato funkce je dostupná ve verzi preview v Číně Office 365, US Government obrany a US Government Federal plány. 

- Předpokládá, že již máte [infrastruktury veřejných klíčů (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) a [služby AD FS](connect/active-directory-aadconnectfed-whatis.md) nakonfigurované.    


## <a name="requirements"></a>Požadavky

ověřování pomocí certifikátů tooconfigure hello jsou splněny následující podmínky:  

- Ověřování pomocí certifikátů (CBA) je podporována pouze pro federovaném prostředí pro aplikace prohlížeče nebo nativní klienty, kteří používají moderní ověřování (ADAL). Jedinou výjimkou Hello je Exchange Active Sync (EAS) pro EXO, který můžete použít pro federované i spravované účty. 

- Hello kořenové certifikační autority a jakékoliv zprostředkující certifikační autority musí být nakonfigurované v Azure Active Directory.  

- Seznam odvolaných certifikátů (CRL), může být odkazováno prostřednictvím internetové adresy URL musí mít každý certifikační autority.  

- Musí mít alespoň jednu certifikační autoritu nakonfigurován v Azure Active Directory. Související kroky můžete najít v hello [konfigurace certifikačních autorit hello](#step-2-configure-the-certificate-authorities) části.  

- U klientů Exchange ActiveSync musí mít hello klientský certifikát uživatele hello směrovatelné e-mailová adresa v systému Exchange online v hlavní název buď hello nebo hello Název RFC822 hodnotu pole alternativní název subjektu hello. Azure Active Directory mapuje hello RFC822 hodnota toohello adresu proxy serveru atribut v adresáři hello.  

- Klientské zařízení musí mít přístup tooat alespoň jednu certifikační autority, která vydává certifikáty klienta.  

- Klientský certifikát pro ověřování klientů musí být vydán tooyour klienta.  




## <a name="step-1-select-your-device-platform"></a>Krok 1: Vyberte platformu zařízení

Jako první krok potřebujete pro hello platforma, která vás, tooreview hello následující:

- Podpora mobilních aplikacích Office Hello 
- požadavky na konkrétní implementace Hello  

pro následující platformy zařízení hello existuje informace související s Hello:

- [Android](active-directory-certificate-based-authentication-android.md)
- [iOS](active-directory-certificate-based-authentication-ios.md)


## <a name="step-2-configure-hello-certificate-authorities"></a>Krok 2: Konfigurace hello certifikačních autorit 

tooconfigure certifikačních autorit ve službě Azure Active Directory, pro každý certifikační autority, nahrajte hello následující: 

* Hello veřejnou část hello certifikátu v *.cer* formátu 
* Hello internetový bod adresy URL, kde hello seznamy odvolaných certifikátů (CRL) jsou umístěny

Hello schéma pro certifikační autority vypadá takto: 

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

Pro konfiguraci hello, můžete použít hello [Azure Active Directory PowerShell verze 2](/powershell/azure/install-adv2?view=azureadps-2.0):  

1. Spusťte prostředí Windows PowerShell s oprávněními správce. 
2. Nainstalujte modul hello Azure AD. Je třeba tooinstall verze [2.0.0.33 ](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) nebo vyšší.  
   
        Install-Module -Name AzureAD –RequiredVersion 2.0.0.33 

Jako první krok konfigurace je nutné tooestablish připojení s vašeho klienta. Při připojení klienta tooyour existuje, můžete zkontrolovat, přidat, odstranit a upravit hello důvěryhodných certifikačních autorit, které jsou definované ve vašem adresáři. 

### <a name="connect"></a>Připojení

tooestablish připojení se váš klient, použijte hello [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) rutiny:

    Connect-AzureAD 


### <a name="retrieve"></a>Načtení 

tooretrieve hello důvěryhodných certifikačních autorit, které jsou definované ve vašem adresáři používat hello [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) rutiny. 

    Get-AzureADTrustedCertificateAuthority 
 

### <a name="add"></a>Přidat

toocreate důvěryhodné certifikační autority, použití hello [New-AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) rutiny a sadu hello **crlDistributionPoint** atribut tooa správnou hodnotu: 
   
    $cert=Get-Content -Encoding byte "[LOCATION OF hello CER FILE]" 
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
    $new_ca.AuthorityType=0 
    $new_ca.TrustedCertificate=$cert 
    $new_ca.crlDistributionPoint=”<CRL Distribution URL>”
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 


### <a name="remove"></a>Odebrat

tooremove důvěryhodné certifikační autority, použití hello [odebrat AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) rutiny:
   
    $c=Get-AzureADTrustedCertificateAuthority 
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiy"></a>Modfiy

toomodify důvěryhodné certifikační autority, použití hello [Set-AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) rutiny:

    $c=Get-AzureADTrustedCertificateAuthority 
    $c[0].AuthorityType=1 
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 


## <a name="step-3-configure-revocation"></a>Krok 3: Konfigurace odvolání

toorevoke klientský certifikát, Azure Active Directory načte certifikát hello seznam odvolaných certifikátů (CRL) z adresy URL hello odesílané jako součást informace o certifikační autoritě a zapíše do mezipaměti. Hello publikování poslední časové razítko (**datum účinnosti** vlastnost) v seznamu CRL se používá hello tooensure hello CRL je stále platný. Hello CRL je pravidelně odkazované toorevoke toocertificates přístupu, které jsou součástí hello seznamu.

Pokud více rychlých odvolání je potřeba (například pokud uživatel ztratí zařízení), můžete zrušena hello autorizační token uživatele hello. tooinvalidate hello autorizační token, nastavte hello **StsRefreshTokenValidFrom** pole pro tento konkrétní uživatel pomocí prostředí Windows PowerShell. Je nutné aktualizovat hello **StsRefreshTokenValidFrom** pole pro každého uživatele chcete toorevoke přístup.

tooensure, která je uchována hello odvolání, je nutné nastavit hello **datum účinnosti** hello CRL tooa datum po hello hodnotu, která nastavuje **StsRefreshTokenValidFrom** a ověřte hello certifikátu dotyčném v Hello seznamu CRL.

Dobrý den, následující kroky outline hello proces pro aktualizaci nebo zneplatnění hello autorizační token podle nastavení hello **StsRefreshTokenValidFrom** pole. 

**odvolání tooconfigure:** 

1. Připojte službou MSOL toohello přihlašovací údaje správce: 
   
        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

2. Načtěte hello aktuální StsRefreshTokensValidFrom hodnotu pro uživatele: 
   
        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 

3. Nakonfigurujte novou hodnotu StsRefreshTokensValidFrom pro aktuální časové razítko aplikace hello uživatele stejná toohello: 
   
        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

Hello datum, které nastavíte, musí být v budoucnu hello. Pokud není datum hello v hello budoucí, hello **StsRefreshTokensValidFrom** není nastavena vlastnost. Pokud je datum hello v hello budoucí, **StsRefreshTokensValidFrom** nastavena toohello aktuální čas (ne. datum hello uvedené pomocí příkazu Set-MsolUser). 


## <a name="step-4-test-your-configuration"></a>Krok 4: Testování konfigurace

### <a name="testing-your-certificate"></a>Testování vašeho certifikátu

Jako první test konfigurace, že byste měli zkusit toosign v příliš[Outlook Web Access](https://outlook.office365.com) nebo [SharePoint Online](https://microsoft.sharepoint.com) pomocí vaší **prohlížeč na zařízení**.

Pokud vaše přihlášení úspěšné, pak víte, že:

- Hello uživatelský certifikát byl zřízené tooyour testovací zařízení
- Služba AD FS je správně nakonfigurovaná.  


### <a name="testing-office-mobile-applications"></a>Testování mobilních aplikacích Office

**tootest ověřování pomocí certifikátů na vaše mobilní aplikace Office:** 

1. Na testovací zařízení nainstalujte mobilní aplikace Office (například OneDrive).
3. Spuštění aplikace hello. 
4. Zadejte uživatelské jméno a potom vyberte hello uživatelský certifikát, že který má toouse. 

Můžete by měla být úspěšně přihlášeni. 

### <a name="testing-exchange-activesync-client-applications"></a>Testování aplikací klienta Exchange ActiveSync

tooaccess Exchange ActiveSync (EAS) prostřednictvím ověřování pomocí certifikátů, profilem EAS obsahující hello klientský certifikát musí být k dispozici toohello aplikace. 

Hello profilu EAS, musí obsahovat hello následující informace:

- Hello toobe uživatele certifikátu používaného pro ověřování 

- koncový bod EAS Hello (například outlook.office365.com)

Profilu EAS můžete nakonfigurovat a vztahujících se na zařízení hello prostřednictvím využití hello správy mobilních zařízení (MDM) jako je například Intune nebo tím, že ručně certifikát hello hello profilu EAS hello zařízení.  

### <a name="testing-eas-client-applications-on-android"></a>Testování EAS klientské aplikace v systému Android

**ověřování pomocí certifikátu tootest:**  

1. Konfigurace profilu EAS hello aplikace, který splňuje požadavky hello výše.  
2. Otevřete aplikaci hello a ověřte, zda je synchronizace e-mailu. 

