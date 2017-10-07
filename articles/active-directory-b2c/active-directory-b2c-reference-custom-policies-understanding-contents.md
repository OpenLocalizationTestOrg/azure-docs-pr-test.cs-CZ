---
title: "Azure Active Directory B2C: Seznámení vlastní zásady sady starter hello | Microsoft Docs"
description: "Téma na Azure Active Directory B2C vlastní zásady"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: joroja
ms.openlocfilehash: 3484e8cc6fa6a9d57c2aa14c0cc9616065892d10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-hello-custom-policies-of-hello-azure-ad-b2c-custom-policy-starter-pack"></a>Principy hello vlastní zásady vlastní zásady pro spuštění sady hello Azure AD B2C

Tato část uvádí všechny elementy základní hello hello B2C_1A_base zásad, která se dodává s hello **Starter Pack** a který je využít k vytváření vlastních zásad prostřednictvím hello dědičnosti třídy hello *B2C_1A_base_ rozšíření zásad*.

Jako takový ho více zvlášť zaměřen na hello již definována deklarace identity typy, transformace deklarací, obsahu definice, poskytovatelů deklarací identity s jejich technické profily a hello základní uživatelské cesty.

> [!IMPORTANT]
> Společnost Microsoft neposkytuje žádné záruky, vyjádřené nebo předpokládané, s ohledem na toohello informací uvedených níže. V době GA nebo po může kdykoli před časem GA zavedeny změny.

Vlastní zásady a hello B2C_1A_base_extensions zásady můžete přepsat tyto definice a prodloužit tuto zásadu nadřazené tím, že poskytuje další ty, které jsou podle potřeby.

Hello základní prvky hello *B2C_1A_base zásad* jsou typy deklarací identity, transformace deklarací a definic obsahu. Tyto prvky můžete náchylný toobe odkazovaný ve vlastní zásady také jako hello *B2C_1A_base_extensions zásad*.

## <a name="claims-schemas"></a>Schémata deklarace identity

Tato deklarace identity, schémata je rozdělené do tří částí:

1.  První oddíl, který obsahuje seznam hello minimální deklarací, které jsou požadovány pro hello uživatele cesty toowork správně.
2.  Druhý oddíl, seznamy hello deklarace identity, které jsou potřebné pro parametrů řetězce dotazu a jiné speciální parametry toobe předána tooother zprostředkovatelů deklarací identit, zejména login.microsoftonline.com pro ověřování. **Neměňte prosím tyto deklarace**.
3.  A nakonec třetí oddíl, který obsahuje seznam dalších, volitelných deklarace identity, které mohou být shromažďovány z hello uživatele uložené v adresáři hello a odeslány v tokenech během přihlášení. V této části mohou být přidány nové deklarace identity typu toobe shromážděných z hello uživatele nebo odeslat v tokenu hello.

> [!IMPORTANT]
> schéma Hello deklarací identity obsahuje omezení na určité deklarace identity, jako jsou uživatelská jména a hesla. Hello zásady důvěryhodnosti Framework TF zpracovává Azure AD jako ostatní poskytovatele deklarací identity a všechny její omezení jsou modelována v hello premium zásad. Zásady může být upravený tooadd další omezení, nebo pomocí jiného poskytovatele deklarací identity pro úložiště přihlašovacích údajů, který bude mít svůj vlastní omezení.

Níže jsou uvedeny typy deklarací identity k dispozici Hello.

### <a name="claims-that-are-required-for-hello-user-journeys"></a>Deklarace identity, které jsou požadovány pro cesty uživatele hello

Hello následující deklarace identity jsou požadovány pro uživatele cesty toowork správně:

| Typ deklarace identity | Popis |
|-------------|-------------|
| *ID uživatele* | Uživatelské jméno |
| *signInName* | Přihlaste se název |
| *tenantId* | Identifikátor klienta (ID) hello objektu uživatele v Azure AD B2C Premium |
| *objectId* | Identifikátor objektu (ID) hello objektu uživatele v Azure AD B2C Premium |
| *heslo* | Heslo |
| *nové_heslo* | |
| *reenterPassword* | |
| *passwordPolicies* | Zásady pro hesla používané síly hesla toodetermine Premium Azure AD B2C, vypršení platnosti, atd. |
| *Sub –* | |
| *alternativeSecurityId* | |
| *identityProvider* | |
| *displayName* | |
| *strongAuthenticationPhoneNumber* | Telefonní číslo uživatele |
| *Verified.strongAuthenticationPhoneNumber* | |
| *e-mailu* | E-mailovou adresu, kterou lze použít toocontact hello uživatele |
| *signInNamesInfo.emailAddress* | E-mailovou adresu, která hello uživatele můžete použít toosign v |
| *otherMails* | E-mailové adresy, které se dají použít toocontact hello uživatele |
| *userPrincipalName* | Uživatelské jméno, jak je uložen v hello Azure AD B2C Premium |
| *upnUserName* | Uživatelské jméno pro vytváření hlavní název uživatele |
| *mailNickName* | Uživatelské jméno e-mailu nick uložen v hello Azure AD B2C Premium |
| *newUser* | |
| *provést vstup SelfAsserted* | Deklarace identity, která určuje, zda byly shromažďovány atributy od uživatele hello |
| *provést vstup PhoneFactor* | Deklarace identity, která určuje, zda nové telefonní číslo bylo odebráno od uživatele hello |
| *authenticationSource* | Určuje, zda byla ověřena hello uživatele na sociálních zprostředkovatele Identity, login.microsoftonline.com nebo místní účet |

### <a name="claims-required-for-query-string-parameters-and-other-special-parameters"></a>Deklarace identity, které jsou potřebné pro parametrů řetězce dotazu a další speciální parametry

Hello následující deklarace identity jsou požadované toopass na zprostředkovatelů deklarací identit tooother speciální parametry (včetně některých parametrů řetězce dotazu):

| Typ deklarace identity | Popis |
|-------------|-------------|
| *Nux* | Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr. |
| *NCA* | Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr. |
| *řádku* | Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr. |
| *mkt* | Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr. |
| *LC* | Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr. |
| *grant_type* | Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr. |
| *obor* | Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr. |
| *client_id* | Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr. |
| *objectIdFromSession* | Parametr poskytované systém hello výchozí relace správy zprostředkovatele tooindicate hello id objektu načtení z relace jednotného přihlašování |
| *isActiveMFASession* | Parametr poskytované hello MFA relace správy tooindicate, že tento uživatel hello má aktivní relaci vícefaktorového ověřování |

### <a name="additional-optional-claims-that-can-be-collected"></a>Další (volitelné) deklarace identity, které se můžou shromažďovat

Následující Hello deklarací se další deklarace identity, které mohou být shromažďovány z hello uživatele uložené v adresáři hello a odesílají v tokenu hello. Jak je uvedeno před, další deklarace identity se dá přidat toothis seznamu.

| Typ deklarace identity | Popis |
|-------------|-------------|
| *givenName* | Křestní jméno uživatele (také označované jako křestní jméno) |
| *Příjmení* | Přezdívka uživatele (také označované jako název rodiny nebo příjmení) |
| *Extension_picture* | Obrázek uživatele ze sociálních |

## <a name="claim-transformations"></a>Transformace deklarací identity

transformace deklarace identity k dispozici Hello jsou uvedeny níže.

| Transformace deklarací identity | Popis |
|----------------------|-------------|
| *CreateOtherMailsFromEmail* | |
| *CreateRandomUPNUserName* | |
| *CreateUserPrincipalName* | |
| *CreateSubjectClaimFromObjectID* | |
| *CreateSubjectClaimFromAlternativeSecurityId* | |
| *CreateAlternativeSecurityId* | |

## <a name="content-definitions"></a>Definice obsahu

Tato část popisuje hello obsahu definice již byl deklarován v hello *B2C_1A_base* zásad. Tyto definice obsahu jsou náchylné toobe odkazovat, přepsat nebo rozšířené podle potřeby v svoje vlastní zásady také jako hello *B2C_1A_base_extensions* zásad.

| Poskytovatele deklarací identity | Popis |
|-----------------|-------------|
| *Facebook* | |
| *Místní účet přihlášení* | |
| *PhoneFactor* | |
| *Azure Active Directory* | |
| *Vlastní prohlašovanou* | |
| *Místní účet* | |
| *Správa relací* | |
| *Modul zásad Trustframework* | |
| *TechnicalProfiles* | |
| *Vydavatel tokenu* | |

## <a name="technical-profiles"></a>Technické profily

Tato část znázorňuje hello technické profily už deklarovaný za poskytovatele deklarací identity v hello *B2C_1A_base* zásad. Tyto technické profily jsou náchylné toobe další odkazuje, přepsat nebo rozšířené podle potřeby v svoje vlastní zásady také jako hello *B2C_1A_base_extensions* zásad.

### <a name="technical-profiles-for-facebook"></a>Technické profily pro Facebook

| Technické profilu | Popis |
|-------------------|-------------|
| *OAUTH pro Facebook* | |

### <a name="technical-profiles-for-local-account-signin"></a>Technické profily pro místní přihlášení účtu

| Technické profilu | Popis |
|-------------------|-------------|
| *Neinteraktivní přihlášení* | |

### <a name="technical-profiles-for-phone-factor"></a>Technické profily pro Phone Factor

| Technické profilu | Popis |
|-------------------|-------------|
| *Vstup PhoneFactor* | |
| *PhoneFactor InputOrVerify* | |
| *Ověřte PhoneFactor* | |

### <a name="technical-profiles-for-azure-active-directory"></a>Technické profily pro Azure Active Directory

| Technické profilu | Popis |
|-------------------|-------------|
| *Běžné AAD* | Technické profil podle hello jiných profilů technické AAD xxx |
| *AAD UserWriteUsingAlternativeSecurityId* | Technické profil pro sociálních přihlášení |
| *AAD UserReadUsingAlternativeSecurityId* | Technické profil pro sociálních přihlášení |
| *AAD. UserReadUsingAlternativeSecurityId NoError* | Technické profil pro sociálních přihlášení |
| *AAD UserWritePasswordUsingLogonEmail* | Technické profil pro místní účty |
| *AAD UserReadUsingEmailAddress* | Technické profil pro místní účty |
| *AAD UserWriteProfileUsingObjectId* | Aktualizace záznamu uživatele pomocí objectId technické profilu |
| *AAD UserWritePhoneNumberUsingObjectId* | Aktualizace záznamu uživatele pomocí objectId technické profilu |
| *AAD UserWritePasswordUsingObjectId* | Aktualizace záznamu uživatele pomocí objectId technické profilu |
| *AAD UserReadUsingObjectId* | Technické profil je použité tooread dat po ověření uživatele |

### <a name="technical-profiles-for-self-asserted"></a>Technické profily pro samoobslužné prohlašovanou

| Technické profilu | Popis |
|-------------------|-------------|
| *Sociální SelfAsserted* | |
| *SelfAsserted ProfileUpdate* | |

### <a name="technical-profiles-for-local-account"></a>Technické profily pro místní účet

| Technické profilu | Popis |
|-------------------|-------------|
| *LocalAccountSignUpWithLogonEmail* | |

### <a name="technical-profiles-for-session-management"></a>Technické profilů pro správu relací

| Technické profilu | Popis |
|-------------------|-------------|
| *SM-nedojde k žádné akci* | |
| *SM AAD* | |
| *SM SocialSignup* | Název profilu je se použité toodisambiguate AAD relace mezi Přihlaste se a přihlaste se |
| *SM SocialLogin* | |
| *SM MFA* | |

### <a name="technical-profiles-for-trustframework-policy-engine-technicalprofiles"></a>Technické profily pro TechnicalProfiles modul zásad Trustframework

V současné době jsou definovány žádné technické profily pro hello **TechnicalProfiles modul zásad Trustframework** poskytovatele deklarací identity.

### <a name="technical-profiles-for-token-issuer"></a>Technické profily pro vydavatel tokenu

| Technické profilu | Popis |
|-------------------|-------------|
| *JwtIssuer* | |

## <a name="user-journeys"></a>Uživatel cesty

Tato část znázorňuje cesty uživatele hello již byl deklarován v hello *B2C_1A_base* zásad. Tyto cesty uživatele jsou náchylné toobe další odkazuje, přepsat nebo rozšířené podle potřeby v svoje vlastní zásady také jako hello *B2C_1A_base_extensions* zásad.

| Uživatel cesty | Popis |
|--------------|-------------|
| *Registrace* | |
| *Přihlášení* | |
| *SignUpOrSignIn* | |
| *Úprava profilu* | |
| *PasswordReset* | |
