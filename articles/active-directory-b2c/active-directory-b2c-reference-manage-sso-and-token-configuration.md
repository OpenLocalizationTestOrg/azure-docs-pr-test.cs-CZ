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
ms.openlocfilehash: 8f5703d15766f221517cd89352d41685652d32d6
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a><span data-ttu-id="2f1fc-102">Azure Active Directory B2C: Správa jednotného přihlašování a tokenu přizpůsobení pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="2f1fc-102">Azure Active Directory B2C: Manage SSO and token customization with custom policies</span></span>
<span data-ttu-id="2f1fc-103">Pomocí vlastních zásad poskytuje stejné kontrolu nad token, relací a jeden přihlašování konfigurace (SSO) jako prostřednictvím integrovaných zásad.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-103">Using custom policies provides you the same control over your token, session and single sign-on (SSO) configurations as through built-in policies.</span></span>  <span data-ttu-id="2f1fc-104">Další informace o jaké každé nastavení, najdete v dokumentaci [zde](#active-directory-b2c-token-session-sso).</span><span class="sxs-lookup"><span data-stu-id="2f1fc-104">To learn what each setting does, please see the documentation [here](#active-directory-b2c-token-session-sso).</span></span>

## <a name="token-lifetimes-and-claims-configuration"></a><span data-ttu-id="2f1fc-105">Token konfigurace životnosti a deklarace identity</span><span class="sxs-lookup"><span data-stu-id="2f1fc-105">Token lifetimes and claims configuration</span></span>
<span data-ttu-id="2f1fc-106">Chcete-li změnit nastavení na vaše životnosti tokenu, je nutné přidat `<ClaimsProviders>` element v souboru předávající strany zásady, které chcete mít vliv.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-106">To change the settings on your token lifetimes, you need to add a `<ClaimsProviders>` element in the relying party file of the policy you want to impact.</span></span>  <span data-ttu-id="2f1fc-107">`<ClaimsProviders>` Element je podřízená `<TrustFrameworkPolicy>`.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-107">The `<ClaimsProviders>` element is a child of the `<TrustFrameworkPolicy>`.</span></span>  <span data-ttu-id="2f1fc-108">Uvnitř budete muset uvést informace, které ovlivní vaše životnosti tokenu.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-108">Inside you'll need to put the information that affects your token lifetimes.</span></span>  <span data-ttu-id="2f1fc-109">Soubor XML vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2f1fc-109">The XML looks like this:</span></span>

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

<span data-ttu-id="2f1fc-110">**Přístup k tokenu životnosti** dobu životnosti tokenu přístupu lze změnit úpravou hodnoty uvnitř `<Item>` klíčem = "token_lifetime_secs" v sekundách.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-110">**Access token lifetimes** The access token lifetime can be changed by modifying the value inside the `<Item>` with the Key="token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="2f1fc-111">Výchozí hodnota v předdefinované je 3600 sekund (60 minut).</span><span class="sxs-lookup"><span data-stu-id="2f1fc-111">The default value in built-in is 3600 seconds (60 minutes).</span></span>

<span data-ttu-id="2f1fc-112">**Životnost tokenu ID** ID dobu životnosti tokenu lze změnit úpravou hodnoty uvnitř `<Item>` klíčem = "id_token_lifetime_secs" v sekundách.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-112">**ID token lifetime** The ID token lifetime can be changed by modifying the value inside the `<Item>` with the Key="id_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="2f1fc-113">Výchozí hodnota v předdefinované je 3600 sekund (60 minut).</span><span class="sxs-lookup"><span data-stu-id="2f1fc-113">The default value in built-in is 3600 seconds (60 minutes).</span></span>

<span data-ttu-id="2f1fc-114">**Aktualizujte životnost tokenu** aktualizace dobu životnosti tokenu lze změnit úpravou hodnoty uvnitř `<Item>` klíčem = "refresh_token_lifetime_secs" v sekundách.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-114">**Refresh token lifetime** The refresh token lifetime can be changed by modifying the value inside the `<Item>` with the Key="refresh_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="2f1fc-115">Výchozí hodnota v předdefinované je 1209600 sekund (14 dnů).</span><span class="sxs-lookup"><span data-stu-id="2f1fc-115">The default value in built-in is 1209600 seconds (14 days).</span></span>

<span data-ttu-id="2f1fc-116">**Aktualizujte životnost tokenu posuvné okno** Pokud chcete nastavit životnost posuvné okno na obnovovací token, upravte hodnotu uvnitř `<Item>` klíčem = "rolling_refresh_token_lifetime_secs" v sekundách.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-116">**Refresh token sliding window lifetime** If you would like to set a sliding window lifetime to your refresh token, modify the value inside `<Item>` with the Key="rolling_refresh_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="2f1fc-117">Výchozí hodnota v předdefinované je 7776000 (90 dnů).</span><span class="sxs-lookup"><span data-stu-id="2f1fc-117">The default value in built-in is 7776000 (90 days).</span></span>  <span data-ttu-id="2f1fc-118">Pokud nechcete enfore klouzavé období platnosti, nahraďte tento řádek:</span><span class="sxs-lookup"><span data-stu-id="2f1fc-118">If you don't want to enfore a sliding window lifetime, replace this line with:</span></span>
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

<span data-ttu-id="2f1fc-119">**Deklarace identity vystavitele (iss)** Pokud chcete změnit deklarace vystavitele (iss), upravte hodnotu uvnitř `<Item>` klíčem = "IssuanceClaimPattern".</span><span class="sxs-lookup"><span data-stu-id="2f1fc-119">**Issuer (iss) claim** If you want to change the Issuer (iss) claim, modify the value inside the `<Item>` with the Key="IssuanceClaimPattern".</span></span>  <span data-ttu-id="2f1fc-120">Použitelné hodnoty jsou `AuthorityAndTenantGuid` a `AuthorityWithTfp`.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-120">The applicable values are `AuthorityAndTenantGuid` and `AuthorityWithTfp`.</span></span>

<span data-ttu-id="2f1fc-121">**Nastavení deklarace představující ID zásad** možnosti pro nastavení této hodnoty jsou TFP (zásady důvěryhodnosti framework) a ACR (authentication kontextu reference).</span><span class="sxs-lookup"><span data-stu-id="2f1fc-121">**Setting claim representing policy ID** The options for setting this value are TFP (trust framework policy) and ACR (authentication context reference).</span></span>  
<span data-ttu-id="2f1fc-122">Doporučujeme, abyste tato nastavení na TFP, chcete-li to provést, zkontrolujte `<Item>` se klíč = "AuthenticationContextReferenceClaimPattern" existuje a je hodnota `None`.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-122">We recommend setting this to TFP, to do this, ensure the `<Item>` with the Key="AuthenticationContextReferenceClaimPattern" exists and the value is `None`.</span></span>
<span data-ttu-id="2f1fc-123">Ve vašem `<OutputClaims>` položky, přidejte tento element:</span><span class="sxs-lookup"><span data-stu-id="2f1fc-123">In your `<OutputClaims>` item, add this element:</span></span>
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
<span data-ttu-id="2f1fc-124">ACR, odeberte `<Item>` klíčem = "AuthenticationContextReferenceClaimPattern".</span><span class="sxs-lookup"><span data-stu-id="2f1fc-124">For ACR, remove the `<Item>` with the Key="AuthenticationContextReferenceClaimPattern".</span></span>

<span data-ttu-id="2f1fc-125">**Deklarace identity subjektu (pod)** tato možnost je uvedena do výchozího stavu ObjectID, pokud chcete přepnout na `Not Supported`, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="2f1fc-125">**Subject (sub) claim** This option is defaulted to ObjectID, if you would like to switch this to `Not Supported`, do the following:</span></span>

<span data-ttu-id="2f1fc-126">Nahraďte tento řádek</span><span class="sxs-lookup"><span data-stu-id="2f1fc-126">Replace this line</span></span> 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
<span data-ttu-id="2f1fc-127">Tento řádek:</span><span class="sxs-lookup"><span data-stu-id="2f1fc-127">with this line:</span></span>
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a><span data-ttu-id="2f1fc-128">Chování relace a jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2f1fc-128">Session behavior and SSO</span></span>
<span data-ttu-id="2f1fc-129">Pokud chcete změnit chování relace a konfigurace jednotného přihlašování, je nutné přidat `<UserJourneyBehaviors>` elementu `<RelyingParty>` elementu.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-129">To change your session behavior and SSO configurations, you need to add a `<UserJourneyBehaviors>` element inside of the `<RelyingParty>` element.</span></span>  <span data-ttu-id="2f1fc-130">`<UserJourneyBehaviors>` Element okamžitě postupujte `<DefaultUserJourney>`.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-130">The `<UserJourneyBehaviors>` element must immediately follow the `<DefaultUserJourney>`.</span></span>  <span data-ttu-id="2f1fc-131">Uvnitř vaší `<UserJourneyBehavors>` element by měla vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="2f1fc-131">The inside of your `<UserJourneyBehavors>` element should look like this:</span></span>

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
<span data-ttu-id="2f1fc-132">**Konfigurace jednotného přihlašování (SSO)** Pokud chcete změnit konfiguraci přihlášení, budete muset upravit hodnotu `<SingleSignOn>`.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-132">**Single sign-on (SSO) configuration** To change the single sign-on configuration, you need to modify the value of `<SingleSignOn>`.</span></span>  <span data-ttu-id="2f1fc-133">Použitelné hodnoty jsou `Tenant`, `Application`, `Policy` a `Disabled`.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-133">The applicable values are `Tenant`, `Application`, `Policy` and `Disabled`.</span></span> 

<span data-ttu-id="2f1fc-134">**Webovou aplikaci dobu platnosti relace (minuty)** změnit webovou aplikaci dobu platnosti relace, budete muset upravit hodnotu `<SessionExpiryInSeconds>` elementu.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-134">**Web app session lifetime (minutes)** To change the the web app session lifetime, you need to modify value of the `<SessionExpiryInSeconds>` element.</span></span>  <span data-ttu-id="2f1fc-135">Výchozí hodnota v integrovaných zásad je 86400 sekund (1 440 minut).</span><span class="sxs-lookup"><span data-stu-id="2f1fc-135">The default value in built-in policies is 86400 seconds (1440 minutes).</span></span>

<span data-ttu-id="2f1fc-136">**Časový limit relace webové aplikace** změnit časový limit relace webové aplikace, budete muset upravit hodnotu `<SessionExpiryType>`.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-136">**Web app session timeout** To change the web app session timeout, you need to modify the value of `<SessionExpiryType>`.</span></span>  <span data-ttu-id="2f1fc-137">Použitelné hodnoty jsou `Absolute` a `Rolling`.</span><span class="sxs-lookup"><span data-stu-id="2f1fc-137">The applicable values are `Absolute` and `Rolling`.</span></span>
