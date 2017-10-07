---
title: "Azure Active Directory B2C: Správa jednotného přihlašování a tokenu přizpůsobení pomocí vlastních zásad | Microsoft Docs"
description: 
services: active-directory-b2c
documentationcenter: 
author: sama
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/02/2017
ms.author: sama
ms.openlocfilehash: b65271a22c77ea41eeec2126e4a3ad24364edd17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a>Azure Active Directory B2C: Správa jednotného přihlašování a tokenu přizpůsobení pomocí vlastních zásad
Pomocí vlastních zásad poskytuje že Hello stejnou kontrolu nad token, relací a jeden přihlašování konfigurace (SSO) jako prostřednictvím integrovaných zásad.  toolearn jednotlivých možností nastavení neobsahuje, naleznete v dokumentaci hello [zde](#active-directory-b2c-token-session-sso).

## <a name="token-lifetimes-and-claims-configuration"></a>Token konfigurace životnosti a deklarace identity
nastavení hello toochange na vaše životnosti tokenu, je nutné tooadd `<ClaimsProviders>` element v souboru předávající strany hello hello zásad chcete tooimpact.  Hello `<ClaimsProviders>` element je podřízená hello `<TrustFrameworkPolicy>`.  Uvnitř budete potřebovat tooput hello informace, které ovlivní vaše životnosti tokenu.  Hello XML vypadá takto:

```XML
<ClaimsProviders>
   <ClaimsProvider>
      <DisplayName>Token Issuer</DisplayName>
      <TechnicalProfiles>
         <TechnicalProfile Id="JwtIssuer">
            <Metadata>
               <Item Key="token_lifetime_secs">3600</Item>
               <Item Key="id_token_lifetime_secs">3600</Item>
               <Item Key="refresh_token_lifetime_secs">1209600</Item>
               <Item Key="rolling_refresh_token_lifetime_secs">7776000</Item>
               <Item Key="IssuanceClaimPattern">AuthorityAndTenantGuid</Item>
               <Item Key="AuthenticationContextReferenceClaimPattern">None</Item>
            </Metadata>
         </TechnicalProfile>
      </TechnicalProfiles>
   </ClaimsProvider>
</ClaimsProviders>
```

**Přístup k tokenu životnosti** hello přístup dobu životnosti tokenu lze změnit úpravou hodnoty hello uvnitř hello `<Item>` s hello Key = "token_lifetime_secs" v sekundách.  Výchozí hodnota Hello v předdefinované je 3600 sekund (60 minut).

**Životnost tokenu ID** životnost tokenu ID hello lze změnit úpravou hodnoty hello uvnitř hello `<Item>` s hello Key = "id_token_lifetime_secs" v sekundách.  Výchozí hodnota Hello v předdefinované je 3600 sekund (60 minut).

**Aktualizujte životnost tokenu** životnost tokenu hello aktualizace lze změnit úpravou hodnoty hello uvnitř hello `<Item>` s hello Key = "refresh_token_lifetime_secs" v sekundách.  Výchozí hodnota Hello v předdefinované je 1209600 sekund (14 dnů).

**Aktualizujte životnost tokenu posuvné okno** Pokud chcete tooset posuvné okno životnost tooyour obnovovací token, upravte hodnotu hello uvnitř `<Item>` s hello Key = "rolling_refresh_token_lifetime_secs" v sekundách.  Výchozí hodnota Hello v předdefinované je 7776000 (90 dnů).  Pokud nechcete, aby tooenfore posuvné okno životnost, nahraďte tento řádek:
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

**Deklarace identity vystavitele (iss)** Pokud chcete toochange hello vystavitele (iss) deklarace identity, upravte hodnotu hello uvnitř hello `<Item>` s hello Key = "IssuanceClaimPattern".  Hello použitelné hodnoty jsou `AuthorityAndTenantGuid` a `AuthorityWithTfp`.

**Nastavení deklarace představující ID zásad** hello možnosti pro nastavení této hodnoty jsou TFP (zásady důvěryhodnosti framework) a ACR (authentication kontextu reference).  
Jsme doporučujeme toto nastavení této tooTFP toodo, zkontrolujte hello `<Item>` s hello Key = "AuthenticationContextReferenceClaimPattern" existuje a je hodnota hello `None`.
Ve vašem `<OutputClaims>` položky, přidejte tento element:
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
U ACR, odebrat hello `<Item>` s hello Key = "AuthenticationContextReferenceClaimPattern".

**Deklarace identity subjektu (pod)** tato možnost je uvedena tooObjectID, pokud chcete tooswitch to příliš`Not Supported`, hello následující:

Nahraďte tento řádek 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
Tento řádek:
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a>Chování relace a jednotné přihlašování
toochange chování relace a konfigurace jednotného přihlašování, budete potřebovat tooadd `<UserJourneyBehaviors>` element uvnitř hello `<RelyingParty>` elementu.  Hello `<UserJourneyBehaviors>` element okamžitě postupujte hello `<DefaultUserJourney>`.  Hello uvnitř vaší `<UserJourneyBehavors>` element by měla vypadat takto:

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
**Konfigurace jednotného přihlašování (SSO)** toochange hello jeden přihlašování konfigurace, je třeba hodnotu hello toomodify `<SingleSignOn>`.  Hello použitelné hodnoty jsou `Tenant`, `Application`, `Policy` a `Disabled`. 

**Webovou aplikaci dobu platnosti relace (minuty)** toochange hello hello webové aplikace dobu platnosti relace, musíte hodnotu toomodify hello `<SessionExpiryInSeconds>` elementu.  Výchozí hodnota Hello v integrovaných zásad je 86400 sekund (1 440 minut).

**Časový limit relace webové aplikace** toochange hello webové aplikace časový limit relace, musíte hodnotu hello toomodify `<SessionExpiryType>`.  Hello použitelné hodnoty jsou `Absolute` a `Rolling`.
