---
title: "Azure AD Connect: Použijte poskytovatele SAML 2.0 Identity pro jednotné přihlašování na | Microsoft Docs"
description: "Toto téma popisuje pomocí vyhovující Idp SAML 2.0 pro jednotné přihlašování na."
services: active-directory
author: billmath
manager: femila
ms.custom: it-pro
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: f9653dc44fb284a9b3c1988f623c33f27ae148cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#  <a name="use-a-saml-20-identity-provider-idp-for-single-sign-on"></a>Použít poskytovatele 2.0 Identity SAML (IdP) pro jednotné přihlašování na

Toto téma obsahuje informace o používání SAML 2.0 kompatibilní s aktualizací SP Lite profil na základě zprostředkovatele Identity jako hello preferované tokenu služby zabezpečení (STS) nebo zprostředkovatele identity. To je užitečné, pokud už máte adresáře uživatele a hesla ukládat v místě, které lze přistupovat pomocí SAML 2.0. Tento adresář existující uživatele lze použít pro přihlášení tooOffice 365 a jiným Azure AD zabezpečeným prostředkům. Hello SAML 2.0 SP-Lite profil je založen na hello široce používá zabezpečení kontrolního výrazu SAML (Markup Language) federované identity standardní tooprovide přihlašování a atribut exchange framework.

>[!NOTE]
>Seznam 3. stran Idps, které byly testovány pro použití s Azure AD najdete v části hello [seznam kompatibility federace Azure AD](active-directory-aadconnect-federation-compatibility.md)

Společnost Microsoft podporuje toto přihlášení jako hello integrace cloudové služby společnosti Microsoft, např. Office 365, s vaší správně nakonfigurované SAML 2.0 profil na základě deklarací identity. Zprostředkovatelé identity SAML 2.0 jsou produkty třetích stran, a proto společnost Microsoft neposkytuje podporu pro hello nasazení, konfiguraci, řešení potíží s osvědčené postupy týkající se jim. Jednou správně nakonfigurované hello integrace s hello SAML 2.0 poskytovatele identit může být testována pro správnou konfiguraci pomocí hello nástroj Analyzátor připojení společnosti Microsoft, který je podrobněji popsané v následující. Další informace o SAML 2.0 SP-Lite zprostředkovatele identity na základě profilu požádejte hello organizace, která je zadána.

>[!IMPORTANT]
>Jen omezenou sadou klientů jsou k dispozici v tomto scénáři přihlášení pomocí poskytovatelů identit SAML 2.0, patří mezi ně:

>- Webových klientů, jako například Outlook Web Access a služby SharePoint Online
- Bohaté e-mailové klienty, kteří používají základní ověřování a podporovaná metoda přístup Exchange například IMAP, POP, Active Sync, MAPI, atd. (hello rozšířené klientský protokol koncový bod je požadovaná toobe nasazení), včetně:
    - Aplikace Microsoft Outlook 2010, Outlook 2013/Outlook 2016, Apple iPhone (různé verze iOS)
    - Různá zařízení Google Android
    - Windows Phone 7, Windows Phone 7,8 a Windows Phone 8.0
    - E-mailového klienta systému Windows 8 a Windows 8.1 e-mailového klienta
    - E-mailového klienta Windows 10

Všechny ostatní klienty nejsou k dispozici v tomto scénáři přihlášení u svého poskytovatele Identity SAML 2.0. Například klient plochy hello Lync 2010 není možné toologin do provozu hello u svého poskytovatele Identity SAML 2.0 nakonfigurována pro jednotné přihlašování.

## <a name="azure-ad-saml-20-protocol-requirements"></a>Protokol požadavky pro Azure AD SAML 2.0
Toto téma obsahuje podrobné požadavky na protokol hello a zpráva formátování, že poskytovatel identity SAML 2.0 musí implementovat toofederate s Azure AD tooenable přihlašování tooone nebo více cloudovým službám Microsoftu (např. Office 365). předávající strana Hello SAML 2.0 (SP služby tokenů zabezpečení) pro cloudové služby Microsoftu používá v tomto scénáři je Azure AD.

Doporučuje se, zajistíte, aby vaše SAML 2.0 zprávy výstup zprostředkovatele identity se jako podobné toohello jako možné zadat ukázka trasování. Také použijte hodnoty konkrétní atribut z metadat hello zadaný Azure AD, kde je to možné. Jakmile vyhovují zprávy si výstup, můžete otestovat s hello Microsoft připojení Analyzer jak je popsáno níže.

metadata Hello Azure AD můžete stáhnout z této adresy URL: [https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml](http://https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml).
Pro zákazníkům v Číně pomocí hello Číně konkrétní instanci Office 365, hello následující koncového bodu federačního slouží: [https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml](https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml).

## <a name="saml-protocol-requirements"></a>Požadavky protokolu SAML
Tato část podrobně popisuje, jak jsou páry zprávu žádosti a odpovědi hello připravili v pořadí toohelp jste tooformat zprávy si správně.

Azure AD může být nakonfigurované toowork pomocí poskytovatelů identit, které používají hello SAML 2.0 SP Lite profil s některé specifické požadavky, jak je uvedeno dále. Pomocí hello ukázka SAML žádosti a odpovědi zprávy společně s automatizované a ruční testování, můžete pracovat tooachieve interoperability s Azure AD.

## <a name="signature-block-requirements"></a>Požadavky na podpis bloku
V rámci hello odpověď SAML uzlu zprávy hello podpis obsahuje informace o hello digitální podpis pro samotnou zprávu hello. blok signatury Hello má hello následující požadavky:

1. musí být podepsané Hello assertion uzel
2.  Hello RSA sha1 algoritmus musí být použita jako hello DigestMethod. Jiné algoritmy digitální podpis nebyly přijaty.
   `<ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>`
3.  Může také podepsat hello dokumentu XML. 
4.  Hello transformace algoritmus musí shodovat s hello hodnoty v hello následující ukázka:`<ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
       <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>`
9.  Hello SignatureMethod algoritmus musí odpovídat hello následující ukázka:`<ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>`

## <a name="supported-bindings"></a>Podporované vazby
Vazby jsou hello přenosu souvisejících parametrů komunikace, které jsou požadovány. Hello následující požadavky na použití toohello vazby

1. HTTPS je hello vyžaduje přenos.
2.  Azure AD bude vyžadovat HTTP POST na token odesílání během přihlášení
3.  Azure AD použije HTTP POST pro zprostředkovatele identity toohello požadavek ověřování hello a PŘESMĚROVÁNÍ pro zprostředkovatele identity toohello zpráva odhlášení hello.

## <a name="required-attributes"></a>Povinné atributy
Tato tabulka uvádí požadavky pro konkrétní atributy ve zprávě hello SAML 2.0.
 
|Atribut|Popis|
| ----- | ----- |
|NameID|stejné jako ImmutableID hello Azure AD uživatele musí hello Hello hodnotu Tento kontrolní výraz součásti. Může být až too64 alfanumerickými znaky. Musí být kódovaný bezpečné znaky bez HTML, například je "+" znak zobrazí jako ".2B".|
|IDPEmail|Hello hlavní název uživatele (UPN), je uvedena v hello SAML odpovědi jako element s hello název IDPEmail to je UserPrincipalName hello uživatele (UPN) v Azure AD nebo Office 365. Hello UPN je ve formátu e-mailovou adresu. Hodnota UPN v systému Windows Office 365 (Azure Active Directory).|
|Vystavitel|Toto je požadovaná toobe identifikátoru URI hello zprostředkovatele identity. Neměli byste znovu používat hello vystavitele z hello ukázkové zprávy. Pokud máte víc domén nejvyšší úrovně ve vaší hello klienty Azure AD musí odpovídat vystavitele hello zadaný identifikátor URI nastavení nakonfigurovaná v každé doméně.|

>[!IMPORTANT]
>Azure AD aktuálně podporuje hello následujících NameID formát URI SAML 2.0:urn:oasis:names:tc:SAML:2.0:nameid-formátu: trvalé.

## <a name="sample-saml-request-and-response-messages"></a>Ukázka zprávy požadavku a odpovědi SAML
Pár zprávu žádosti a odpovědi je zobrazen pro výměnu zpráv přihlašování hello.
Toto je ukázkovou zprávu požadavku, který se odesílá z poskytovatele identity SAML 2.0 ukázka tooa Azure AD. poskytovatele identity SAML 2.0 ukázka Hello je že nakonfigurovaný protokol SAML-P toouse služby Active Directory Federation Services (AD FS). Vzájemná funkční spolupráce testování také bylo dokončeno s jiných zprostředkovatelů identity SAML 2.0.

    `<samlp:AuthnRequest xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion" ID="_7171b0b2-19f2-4ba2-8f94-24b5e56b7f1e" IssueInstant="2014-01-30T16:18:35Z" Version="2.0" AssertionConsumerServiceIndex="0" >
    <saml:Issuer>urn:federation:MicrosoftOnline</saml:Issuer>
    <samlp:NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
    </samlp:AuthnRequest>`

Toto je ukázkovou zprávu odpovědi, která se odesílá z hello ukázka SAML 2.0 kompatibilní identity zprostředkovatele tooAzure AD nebo Office 365.

    `<samlp:Response ID="_592c022f-e85e-4d23-b55b-9141c95cd2a5" Version="2.0" IssueInstant="2014-01-31T15:36:31.357Z" Destination="https://login.microsoftonline.com/login.srf" Consent="urn:oasis:names:tc:SAML:2.0:consent:unspecified" InResponseTo="_049917a6-1183-42fd-a190-1d2cbaf9b144" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
    </samlp:Status>
    <Assertion ID="_7e3c1bcd-f180-4f78-83e1-7680920793aa" IssueInstant="2014-01-31T15:36:31.279Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      <ds:SignedInfo>
        <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
        <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1" />
        <ds:Reference URI="#_7e3c1bcd-f180-4f78-83e1-7680920793aa">
          <ds:Transforms>
            <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
            <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
          </ds:Transforms>
          <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1" />
          <ds:DigestValue>CBn/5YqbheaJP425c0pHva9PhNY=</ds:DigestValue>
        </ds:Reference>
      </ds:SignedInfo>
      <ds:SignatureValue>TciWMyHW2ZODrh/2xrvp5ggmcHBFEd9vrp6DYXp+hZWJzmXMmzwmwS8KNRJKy8H7XqBsdELA1Msqi8I3TmWdnoIRfM/ZAyUppo8suMu6Zw+boE32hoQRnX9EWN/f0vH6zA/YKTzrjca6JQ8gAV1ErwvRWDpyMcwdYCiWALv9ScbkAcebOE1s1JctZ5RBXggdZWrYi72X+I4i6WgyZcIGai/rZ4v2otoWAEHS0y1yh1qT7NDPpl/McDaTGkNU6C+8VfjD78DrUXEcAfKvPgKlKrOMZnD1lCGsViimGY+LSuIdY45MLmyaa5UT4KWph6dA==</ds:SignatureValue>
      <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
        <ds:X509Data>
          <ds:X509Certificate>MIIC7jCCAdagAwIBAgIQRrjsbFPaXIlOG3GTv50fkjANBgkqhkiG9w0BAQsFADAzMTEwLwYDVQQDEyhBREZTIFNpZ25pbmcgLSBXUzIwMTJSMi0wLnN3aW5mb3JtZXIuY29tMB4XDTE0MDEyMDE1MTY0MFoXDTE1MDEyMDE1MTY0MFowMzExMC8GA1UEAxMoQURGUyBTaWduaW5nIC0gV1MyMDEyUjItMC5zd2luZm9ybWVyLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKe+rLVmXy1QwCwZwqgbbp1/+3ZWxd9T/jV0hpLIIWr+LCOHqq8n8beJvlivgLmDJo8f+EITnAxWcsJUvVai/35AhHCUq9tc9sqMp5PWtabAEMb2AU72/QlX/72D2/NbGQq1BWYbqUpgpCZ2nSgvlWDHlCiUo//UGsvfox01kjTFlmqQInsJVfRxF5AcCAwEAATANBgkqhkiG9w0BAQsFAAOCAQEAi8c6C4zaTEc7aQiUgvnGQgCbMZbhUXXLGRpjvFLKaQzkwa9eq7WLJibcSNyGXBa/SfT5wJgsm3TPKgSehGAOTirhcqHheZyvBObAScY7GOT+u9pVYp6raFrc7ez3c+CGHeV/tNvy1hJNs12FYH4X+ZCNFIT9tprieR25NCdi5SWUbPZL0tVzJsHc1y92b2M2FxqRDohxQgJvyJOpcg2mSBzZZIkvDg7gfPSUXHVS1MQs0RHSbwq/XdQocUUhl9/e/YWCbNNxlM84BxFsBUok1dH/gzBySx+Fc8zYi7cOq9yaBT3RLT6cGmFGVYZJW4FyhPZOCLVNsLlnPQcX3dDg9A==</ds:X509Certificate>
        </ds:X509Data>
      </KeyInfo>
    </ds:Signature>
    <Subject>
      <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">ABCDEG1234567890</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="_049917a6-1183-42fd-a190-1d2cbaf9b144" NotOnOrAfter="2014-01-31T15:41:31.357Z" Recipient="https://login.microsoftonline.com/login.srf" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2014-01-31T15:36:31.263Z" NotOnOrAfter="2014-01-31T16:36:31.263Z">
      <AudienceRestriction>
        <Audience>urn:federation:MicrosoftOnline</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="IDPEmail">
        <AttributeValue>administrator@contoso.com</AttributeValue>
      </Attribute>
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2014-01-31T15:36:30.200Z" SessionIndex="_7e3c1bcd-f180-4f78-83e1-7680920793aa">
      <AuthnContext>
        <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
    </Assertion>
    </samlp:Response>`

## <a name="configure-your-saml-20-compliant-identity-provider"></a>Konfigurace zprostředkovatele identity kompatibilní SAML 2.0
Toto téma obsahuje pokyny o tom, jak tooconfigure vaše SAML 2.0 identity zprostředkovatele toofederate s Azure AD tooenable přístup přihlášení tooone nebo další Microsoft cloud services (např. Office 365) pomocí protokolu hello SAML 2.0. Hello SAML 2.0 předávající strany pro cloudové služby Microsoftu používá v tomto scénáři je Azure AD.

## <a name="add-azure-ad-metadata"></a>Přidat metadata Azure AD
Zprostředkovatele identity SAML 2.0 musí tooadhere tooinformation o předávající straně hello Azure AD. Azure AD publikuje metadata na https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml.

Doporučuje se při konfiguraci svého poskytovatele identity SAML 2.0 vždy importovat metadata hello nejnovější Azure AD. Všimněte si, že Azure AD není číst metadata z hello zprostředkovatele identity.

## <a name="add-azure-ad-as-a-relying-party"></a>Přidat Azure AD jako předávající strany
Máte tooenable komunikací mezi vašimi poskytovatele identity SAML 2.0 a Azure AD. Tato konfigurace budou závislé na zprostředkovatele konkrétní identity a toodocumentation by měla odkazovat pro ni. Obvykle byste měli nastavit hello předávající strany ID toohello stejné jako hello entityID z metadat hello Azure AD.

>[!NOTE]
>Ověřte hello hodiny na serveru poskytovatele identity SAML 2.0 je synchronizované tooan přesný čas zdroje. Čas nesprávné hodin může způsobit toofail federovaných přihlášení.

## <a name="install-windows-powershell-for-sign-on-with-saml-20-identity-provider"></a>Instalace prostředí Windows PowerShell pro přihlašování pomocí zprostředkovatele identity SAML 2.0
Po dokončení konfigurace zprostředkovatele identity SAML 2.0 pro použití s Azure AD přihlášení hello dalším krokem je toodownload a hello instalace Azure Active Directory modul pro prostředí Windows PowerShell. Po instalaci budete používat tyto rutiny tooconfigure domény Azure AD jako federované domény.

Hello Azure Active Directory modul pro prostředí Windows PowerShell je ke stažení pro správu ve službě Azure AD data vaší organizace. Tento modul nainstaluje soubor rutin tooWindows prostředí PowerShell; Spusťte tyto rutiny tooset až tooAzure přístup přihlašování AD a naopak tooall hello cloudových služeb, které odebíráte. Pokyny o tom, jak toodownload a nainstalujte hello rutin najdete v tématu [http://technet.microsoft.com/library/jj151815.aspx](http://technet.microsoft.com/library/jj151815.aspx)

## <a name="set-up-a-trust-between-your-saml-identity-provider-and-azure-ad"></a>Nastavení vztahu důvěryhodnosti mezi poskytovatele identity SAML a Azure AD
Před provedením konfigurace federace v doméně služby Azure AD, musí mít nakonfigurované vlastní doménu. Nelze vytvořit federaci hello výchozí doménu, která zajišťuje Microsoft. Hello výchozí doménu od společnosti Microsoft končí textem "onmicrosoft.com".
Budete spustit sérii rutiny v tooadd rozhraní příkazového řádku prostředí Windows PowerShell hello nebo převést domén pro jednotné přihlašování.

Každá doména služby Azure Active Directory, která chcete toofederate pomocí zprostředkovatele identity SAML 2.0 musí buď přidat jako jedinou doménu přihlášení nebo převedený toobe jedné doméně přihlášení ze standardní domény. Přidáním nebo převedením domény nastaví vztah důvěryhodnosti mezi poskytovatele identity SAML 2.0 a Azure AD.

Hello následující postup vás provede převod existující standardní domény tooa federované domény pomocí SAML 2.0 SP-Lite. Všimněte si, že k výpadku, který má dopad na uživatele v provozu too2 hodin po provedení tohoto kroku může dojít k vaší doméně.

## <a name="configuring-a-domain-in-your-azure-ad-directory-for-federation"></a>Konfigurace domény v adresáři Azure AD pro federaci


1. Připojit tooyour adresář Azure AD jako správce klienta: připojení MsolService.
2.  Nakonfigurujte požadované federační toouse domény Office 365 pomocí SAML 2.0:`$dom = "contoso.com" $BrandName - "Sample SAML 2.0 IDP" $LogOnUrl = "https://WS2012R2-0.contoso.com/passiveLogon" $LogOffUrl = "https://WS2012R2-0.contoso.com/passiveLogOff" $ecpUrl = "https://WS2012R2-0.contoso.com/PAOS" $MyURI = "urn:uri:MySamlp2IDP" $MySigningCert = @" MIIC7jCCAdagAwIBAgIQRrjsbFPaXIlOG3GTv50fkjANBgkqhkiG9w0BAQsFADAzMTEwLwYDVQQDEyh BREZTIFNpZ25pbmcgLSBXUzIwMTJSMi0wLnN3aW5mb3JtZXIuY29tMB4XDTE0MDEyMDE1MTY0MFoXDT E1MDEyMDE1MTY0MFowMzExMC8GA1UEAxMoQURGUyBTaWduaW5nIC0gV1MyMDEyUjItMC5zd2luZm9yb WVyLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKe+rLVmXy1QwCwZwqgbbp1/kupQ VcjKuKLitVDbssFyqbDTjP7WRjlVMWAHBI3kgNT7oE362Gf2WMJFf1b0HcrsgLin7daRXpq4Qi6OA57 sW1YFMj3sqyuTP0eZV3S4+ZbDVob6amsZIdIwxaLP9Zfywg2bLsGnVldB0+XKedZwDbCLCVg+3ZWxd9 T/jV0hpLIIWr+LCOHqq8n8beJvlivgLmDJo8f+EITnAxWcsJUvVai/35AhHCUq9tc9sqMp5PWtabAEM b2AU72/QlX/72D2/NbGQq1BWYbqUpgpCZ2nSgvlWDHlCiUo//UGsvfox01kjTFlmqQInsJVfRxF5AcC AwEAATANBgkqhkiG9w0BAQsFAAOCAQEAi8c6C4zaTEc7aQiUgvnGQgCbMZbhUXXLGRpjvFLKaQzkwa9 eq7WLJibcSNyGXBa/SfT5wJgsm3TPKgSehGAOTirhcqHheZyvBObAScY7GOT+u9pVYp6raFrc7ez3c+ CGHeV/tNvy1hJNs12FYH4X+ZCNFIT9tprieR25NCdi5SWUbPZL0tVzJsHc1y92b2M2FxqRDohxQgJvy JOpcg2mSBzZZIkvDg7gfPSUXHVS1MQs0RHSbwq/XdQocUUhl9/e/YWCbNNxlM84BxFsBUok1dH/gzBy Sx+Fc8zYi7cOq9yaBT3RLT6cGmFGVYZJW4FyhPZOCLVNsLlnPQcX3dDg9A==" "@ $uri = "http://WS2012R2-0.contoso.com/adfs/services/trust" $Protocol = "SAMLP" Set-MsolDomainAuthentication -DomainName $dom -FederationBrandName $dom -Authentication Federated -PassiveLogOnUri $MyURI -ActiveLogOnUri $ecpUrl -SigningCertificate $MySigningCert -IssuerUri $uri -LogOffUri $url -PreferredAuthenticationProtocol $Protocol` 

3.  Můžete získat hello podpisový certifikát řetězec s kódováním base64 z IDP metadata souboru. Příkladem tohoto umístění bylo zadáno, ale může mírně lišit v závislosti na vaší implementace.

    `<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol"> <KeyDescriptor use="signing"> <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#"> <X509Data> <X509Certificate>MIIC5jCCAc6gAwIBAgIQLnaxUPzay6ZJsC8HVv/QfTANBgkqhkiG9w0BAQsFADAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwHhcNMTMxMTA0MTgxMzMyWhcNMTQxMTA0MTgxMzMyWjAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCwMdVLTr5YTSRp+ccbSpuuFeXMfABD9mVCi2wtkRwC30TIyPdORz642MkurdxdPCWjwgJ0HW6TvXwcO9afH3OC5V//wEGDoNcI8PV4enCzTYFe/h//w51uqyv48Fbb3lEXs+aVl8155OAj2sO9IX64OJWKey82GQWK3g7LfhWWpp17j5bKpSd9DBH5pvrV+Q1ESU3mx71TEOvikHGCZYitEPywNeVMLRKrevdWI3FAhFjcCSO6nWDiMqCqiTDYOURXIcHVYTSof1YotkJ4tG6mP5Kpjzd4VQvnR7Pjb47nhIYG6iZ3mR1F85Ns9+hBWukQWNN2hcD/uGdPXhpdMVpBAgMBAAEwDQYJKoZIhvcNAQELBQADggEBAK7h7jF7wPzhZ1dPl4e+XMAr8I7TNbhgEU3+oxKyW/IioQbvZVw1mYVCbGq9Rsw4KE06eSMybqHln3w5EeBbLS0MEkApqHY+p68iRpguqa+W7UHKXXQVgPMCpqxMFKonX6VlSQOR64FgpBme2uG+LJ8reTgypEKspQIN0WvtPWmiq4zAwBp08hAacgv868c0MM4WbOYU0rzMIR6Q+ceGVRImlCwZ5b7XKp4mJZ9hlaRjeuyVrDuzBkzROSurX1OXoci08yJvhbtiBJLf3uPOJHrhjKRwIt2TnzS9ElgFZlJiDIA26Athe73n43CT0af2IG6yC7e6sK4L3NEXJrwwUZk=</X509Certificate> </X509Data> </KeyInfo> </KeyDescriptor>` 

Další informace o "Set-MsolDomainAuthentication" v tématu: [http://technet.microsoft.com/library/dn194112.aspx](http://technet.microsoft.com/library/dn194112.aspx).

>[!NOTE]
>Je nutné spustit pomocí "$ecpUrl ="https://WS2012R2-0.contoso.com/PAOS"" pouze v případě, že nastavíte ECP rozšíření pro zprostředkovatele identity. Klienti Exchange Online, s výjimkou Outlook webové aplikace (OWA), závisí na příspěvku na základě active koncového bodu. Pokud vaše SAML 2.0 služby tokenů zabezpečení implementuje implementace ECP aktivní koncový bod podobné tooShibboleth na aktivní koncový bod je možné, že pro tyto toointeract bohatých klientů s hello služby Exchange Online.

Po nakonfiguroval federační můžete přepnout zpět příliš "nefederovaných" (nebo "spravovaný"), ale tato změna zabírají toocomplete tootwo hodin a vyžaduje, že přiřazení nové náhodné hesla pro cloud na základě tooeach přihlášení uživatele. Přepnout zpět příliš "spravovaný" může být potřeba v některých scénářích tooreset Chyba v nastavení. Další informace o převodu domény najdete v tématu: [http://msdn.microsoft.com/library/windowsazure/dn194122.aspx](http://msdn.microsoft.com/library/windowsazure/dn194122.aspx).

## <a name="provision-user-principals-tooazure-ad--office-365"></a>Zřídit tooAzure objekty uživatele AD nebo Office 365
Předtím, než můžete ověřovat vaši uživatelé tooOffice 365, je třeba zřídit služby Azure AD s uživatelem, které objekty, které odpovídají toohello assertion v hello SAML 2.0 deklarací identity. Pokud tyto objekty uživatele nejsou známá tooAzure AD předem nelze použít pro federované přihlašování. Azure AD Connect nebo prostředí Windows PowerShell lze použít tooprovision uživatelské objekty.

Azure AD Connect se dá použít tooprovision objekty tooyour domén v adresáři Azure AD z hello místní služby Active Directory. Podrobnější informace najdete v části [integraci místních adresářů se službou Azure Active Directory](active-directory-aadconnect.md).

Prostředí Windows PowerShell lze také použít tooautomate přidání nového uživatele tooAzure AD a toosynchronize změní z místního adresáře hello. toouse hello rutiny prostředí Windows PowerShell, je nutné stáhnout hello [Azure Active Directory moduly](https://docs.microsoft.com/powershell/azure/install-adv2?view=azureadps-2.0).

Tento postup ukazuje, jak tooadd tooAzure jednoho uživatele AD.


1. Připojit tooyour adresář Azure AD jako správce klienta: připojení MsolService.
2.  Vytvořte nový hlavní název uživatele:` New-MsolUser
        -UserPrincipalName elwoodf1@contoso.com
        -ImmutableId ABCDEFG1234567890
        -DisplayName "Elwood Folk"
        -FirstName Elwood 
        -LastName Folk 
        -AlternateEmailAddresses "Elwood.Folk@contoso.com" 
        -LicenseAssignment "samlp2test:ENTERPRISEPACK" 
        -UsageLocation "US" ` 

Další informace o "New-MsolUser" rezervaci [http://technet.microsoft.com/library/dn194096.aspx](http://technet.microsoft.com/library/dn194096.aspx)

>[!NOTE]
>Hello "UserPrinciplName" hodnota musí odpovídat hello hodnotu, která chcete odeslat pro "IDPEmail" v deklaraci identity SAML 2.0 a hello "ImmutableID" hodnota musí odpovídat hello hodnota odeslaná v vaší kontrolní výraz "NameID".

## <a name="verify-single-sign-on-with-your-saml-20-idp"></a>Ověřte, jednotné přihlašování s vaší IDP SAML 2.0
Jako správce hello před ověření a Správa jednotného přihlašování (také federace identit názvem), zkontrolujte informace hello a kroků hello hello následující články tooset si jednotné přihlašování s poskytovatelem SAML 2.0 SP-Lite na základě identity:


1.  Jste si přečetli hello požadavky protokolu Azure AD SAML 2.0
2.  Nakonfigurování zprostředkovatele identity SAML 2.0
3.  Instalace prostředí Windows PowerShell pro jednotné přihlašování pomocí zprostředkovatele identity SAML 2.0
4.  Nastavení vztahu důvěryhodnosti mezi poskytovatele identity SAML 2.0 a Azure AD
5.  Buď pomocí prostředí Windows PowerShell nebo Azure AD Connect, zřídí hlavní tooAzure známé testovací uživatele služby Active Directory (Office 365).
6.  Konfigurovat pomocí synchronizace adresáře [Azure AD Connect](active-directory-aadconnect.md).

Po nastavení jednotného přihlašování s vaší SAML 2.0 SP-Lite na základě identity poskytovatele, ověřte, zda pracuje správně.

>[!NOTE]
>Pokud jste převedli domény, nikoli přidávání jednoho, může trvat hodiny tooset too24 si jednotné přihlašování.
Před ověření jednotného přihlašování, by měl dokončit nastavení synchronizace služby Active Directory, synchronizace vašich adresářů a aktivujte synchronizované uživatele.

### <a name="use-hello-tool-tooverify-that-single-sign-on-has-been-set-up-correctly"></a>Použití hello nástroj tooverify, který jednotné přihlášení byla nastavena správně
tooverify této jednotné přihlášení byla nastavena správně, můžete provést následující postup tooconfirm jsou možné toosign v toohello cloudové služby s svoje podnikové přihlašovací údaje hello.

Společnost Microsoft poskytuje nástroje, které můžete použít tootest SAML 2.0 na základě zprostředkovatele identity. Před spuštěním hello testovací nástroj musíte nakonfigurovat toofederate klienta služby Azure AD pomocí zprostředkovatele identity.

>[!NOTE]
>Hello analyzátor připojení vyžaduje Internet Explorer 10 nebo novější.



1. Stažení hello analyzátor připojení, [https://testconnectivity.microsoft.com/?tabid=Client](https://testconnectivity.microsoft.com/?tabid=Client).
2.  Klikněte na tlačítko Instalovat nyní toobegin stahuje a instaluje nástroj hello.
3.  Vyberte "I nemůže vytvořit federaci se Office 365, Azure nebo jiné služby, které používají Azure Active Directory".
4.  Jakmile nástroj hello stažené a spuštěná, zobrazí se okno diagnostiky připojení hello. testování připojení federační bude projděte nástroj Hello.
5.  Hello připojení analyzátor bude otevřít vaše IDP SAML 2.0 pro toologon můžete, zadejte přihlašovací údaje hello testujete hello hlavní název uživatele: ![SAML](media/active-directory-aadconnect-federation-saml-idp/saml1.png)
6.  V hello federační otestujte okno přihlášení by měl zadejte název účtu a heslo pro hello klienta Azure AD, který je nakonfigurovaný toobe Federovaná pomocí zprostředkovatele identity SAML 2.0. Nástroj Hello se pokusí toosign v pomocí těchto přihlašovacích údajů a podrobné výsledky testů prováděné během hello přihlášení pokus bude třeba zadat jako výstup.
![SAML](media/active-directory-aadconnect-federation-saml-idp/saml2.png)
7. Toto okno zobrazuje selhání výsledek testování. Kliknutím na zkontrolujte podrobné výsledky se zobrazí informace o hello výsledky pro všechny testy, které bylo provedeno. Můžete také ukládat výsledky toodisk hello v pořadí tooshare je.
 
>[!NOTE]
>Hello připojení analyzátor také testy Active federace pomocí hello WS *-ECP/PAOS a na základě protokolů. Pokud nepoužíváte tyto můžete ignorovat hello následující chyba: testování hello Active přihlášení toku pomocí zprostředkovatele identity Active federační koncový bod.

### <a name="manually-verify-that-single-sign-on-has-been-set-up-correctly"></a>Ručně ověřte, zda že tento jednotné přihlášení byla nastavena správně
Ruční ověření poskytuje další kroky, které můžete provést tooensure, který vaší identity SAML 2.0 zprostředkovatele v řadě scénářů pracuje správně.
tooverify, který jednotné přihlášení byla nastavena správně, dokončete hello následující kroky:


1. V počítači připojeném k doméně Přihlaste se pomocí hello stejné přihlašovací jméno, který používáte pro svoje podnikové přihlašovací údaje tooyour cloudové služby.
2.  Klikněte do pole pro heslo hello. Pokud jednotné přihlašování je nastavený, pole pro heslo hello bude nastaveno a zobrazí se následující zpráva hello: "teď jste požadované toosign v na <your company>."
3.  Klikněte na tlačítko hello Přihlaste se na <your company> odkaz. Pokud jste možnost toosign v, potom jednotné přihlašování je nastaven.

## <a name="next-steps"></a>Další kroky


- [Přizpůsobení službou Azure AD Connect a správy služby Active Directory Federation Services](active-directory-aadconnect-federation-management.md)
- [Seznam kompatibilit pro federaci Azure AD](active-directory-aadconnect-federation-compatibility.md)
- [Vlastní instalace Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
