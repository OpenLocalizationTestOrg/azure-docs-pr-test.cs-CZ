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
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a><span data-ttu-id="2f42f-102">Azure Active Directory B2C: Správa jednotného přihlašování a tokenu přizpůsobení pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="2f42f-102">Azure Active Directory B2C: Manage SSO and token customization with custom policies</span></span>
<span data-ttu-id="2f42f-103">Pomocí vlastních zásad poskytuje že Hello stejnou kontrolu nad token, relací a jeden přihlašování konfigurace (SSO) jako prostřednictvím integrovaných zásad.</span><span class="sxs-lookup"><span data-stu-id="2f42f-103">Using custom policies provides you hello same control over your token, session and single sign-on (SSO) configurations as through built-in policies.</span></span>  <span data-ttu-id="2f42f-104">toolearn jednotlivých možností nastavení neobsahuje, naleznete v dokumentaci hello [zde](#active-directory-b2c-token-session-sso).</span><span class="sxs-lookup"><span data-stu-id="2f42f-104">toolearn what each setting does, please see hello documentation [here](#active-directory-b2c-token-session-sso).</span></span>

## <a name="token-lifetimes-and-claims-configuration"></a><span data-ttu-id="2f42f-105">Token konfigurace životnosti a deklarace identity</span><span class="sxs-lookup"><span data-stu-id="2f42f-105">Token lifetimes and claims configuration</span></span>
<span data-ttu-id="2f42f-106">nastavení hello toochange na vaše životnosti tokenu, je nutné tooadd `<ClaimsProviders>` element v souboru předávající strany hello hello zásad chcete tooimpact.</span><span class="sxs-lookup"><span data-stu-id="2f42f-106">toochange hello settings on your token lifetimes, you need tooadd a `<ClaimsProviders>` element in hello relying party file of hello policy you want tooimpact.</span></span>  <span data-ttu-id="2f42f-107">Hello `<ClaimsProviders>` element je podřízená hello `<TrustFrameworkPolicy>`.</span><span class="sxs-lookup"><span data-stu-id="2f42f-107">hello `<ClaimsProviders>` element is a child of hello `<TrustFrameworkPolicy>`.</span></span>  <span data-ttu-id="2f42f-108">Uvnitř budete potřebovat tooput hello informace, které ovlivní vaše životnosti tokenu.</span><span class="sxs-lookup"><span data-stu-id="2f42f-108">Inside you'll need tooput hello information that affects your token lifetimes.</span></span>  <span data-ttu-id="2f42f-109">Hello XML vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2f42f-109">hello XML looks like this:</span></span>

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

<span data-ttu-id="2f42f-110">**Přístup k tokenu životnosti** hello přístup dobu životnosti tokenu lze změnit úpravou hodnoty hello uvnitř hello `<Item>` s hello Key = "token_lifetime_secs" v sekundách.</span><span class="sxs-lookup"><span data-stu-id="2f42f-110">**Access token lifetimes** hello access token lifetime can be changed by modifying hello value inside hello `<Item>` with hello Key="token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="2f42f-111">Výchozí hodnota Hello v předdefinované je 3600 sekund (60 minut).</span><span class="sxs-lookup"><span data-stu-id="2f42f-111">hello default value in built-in is 3600 seconds (60 minutes).</span></span>

<span data-ttu-id="2f42f-112">**Životnost tokenu ID** životnost tokenu ID hello lze změnit úpravou hodnoty hello uvnitř hello `<Item>` s hello Key = "id_token_lifetime_secs" v sekundách.</span><span class="sxs-lookup"><span data-stu-id="2f42f-112">**ID token lifetime** hello ID token lifetime can be changed by modifying hello value inside hello `<Item>` with hello Key="id_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="2f42f-113">Výchozí hodnota Hello v předdefinované je 3600 sekund (60 minut).</span><span class="sxs-lookup"><span data-stu-id="2f42f-113">hello default value in built-in is 3600 seconds (60 minutes).</span></span>

<span data-ttu-id="2f42f-114">**Aktualizujte životnost tokenu** životnost tokenu hello aktualizace lze změnit úpravou hodnoty hello uvnitř hello `<Item>` s hello Key = "refresh_token_lifetime_secs" v sekundách.</span><span class="sxs-lookup"><span data-stu-id="2f42f-114">**Refresh token lifetime** hello refresh token lifetime can be changed by modifying hello value inside hello `<Item>` with hello Key="refresh_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="2f42f-115">Výchozí hodnota Hello v předdefinované je 1209600 sekund (14 dnů).</span><span class="sxs-lookup"><span data-stu-id="2f42f-115">hello default value in built-in is 1209600 seconds (14 days).</span></span>

<span data-ttu-id="2f42f-116">**Aktualizujte životnost tokenu posuvné okno** Pokud chcete tooset posuvné okno životnost tooyour obnovovací token, upravte hodnotu hello uvnitř `<Item>` s hello Key = "rolling_refresh_token_lifetime_secs" v sekundách.</span><span class="sxs-lookup"><span data-stu-id="2f42f-116">**Refresh token sliding window lifetime** If you would like tooset a sliding window lifetime tooyour refresh token, modify hello value inside `<Item>` with hello Key="rolling_refresh_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="2f42f-117">Výchozí hodnota Hello v předdefinované je 7776000 (90 dnů).</span><span class="sxs-lookup"><span data-stu-id="2f42f-117">hello default value in built-in is 7776000 (90 days).</span></span>  <span data-ttu-id="2f42f-118">Pokud nechcete, aby tooenfore posuvné okno životnost, nahraďte tento řádek:</span><span class="sxs-lookup"><span data-stu-id="2f42f-118">If you don't want tooenfore a sliding window lifetime, replace this line with:</span></span>
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

<span data-ttu-id="2f42f-119">**Deklarace identity vystavitele (iss)** Pokud chcete toochange hello vystavitele (iss) deklarace identity, upravte hodnotu hello uvnitř hello `<Item>` s hello Key = "IssuanceClaimPattern".</span><span class="sxs-lookup"><span data-stu-id="2f42f-119">**Issuer (iss) claim** If you want toochange hello Issuer (iss) claim, modify hello value inside hello `<Item>` with hello Key="IssuanceClaimPattern".</span></span>  <span data-ttu-id="2f42f-120">Hello použitelné hodnoty jsou `AuthorityAndTenantGuid` a `AuthorityWithTfp`.</span><span class="sxs-lookup"><span data-stu-id="2f42f-120">hello applicable values are `AuthorityAndTenantGuid` and `AuthorityWithTfp`.</span></span>

<span data-ttu-id="2f42f-121">**Nastavení deklarace představující ID zásad** hello možnosti pro nastavení této hodnoty jsou TFP (zásady důvěryhodnosti framework) a ACR (authentication kontextu reference).</span><span class="sxs-lookup"><span data-stu-id="2f42f-121">**Setting claim representing policy ID** hello options for setting this value are TFP (trust framework policy) and ACR (authentication context reference).</span></span>  
<span data-ttu-id="2f42f-122">Jsme doporučujeme toto nastavení této tooTFP toodo, zkontrolujte hello `<Item>` s hello Key = "AuthenticationContextReferenceClaimPattern" existuje a je hodnota hello `None`.</span><span class="sxs-lookup"><span data-stu-id="2f42f-122">We recommend setting this tooTFP, toodo this, ensure hello `<Item>` with hello Key="AuthenticationContextReferenceClaimPattern" exists and hello value is `None`.</span></span>
<span data-ttu-id="2f42f-123">Ve vašem `<OutputClaims>` položky, přidejte tento element:</span><span class="sxs-lookup"><span data-stu-id="2f42f-123">In your `<OutputClaims>` item, add this element:</span></span>
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
<span data-ttu-id="2f42f-124">U ACR, odebrat hello `<Item>` s hello Key = "AuthenticationContextReferenceClaimPattern".</span><span class="sxs-lookup"><span data-stu-id="2f42f-124">For ACR, remove hello `<Item>` with hello Key="AuthenticationContextReferenceClaimPattern".</span></span>

<span data-ttu-id="2f42f-125">**Deklarace identity subjektu (pod)** tato možnost je uvedena tooObjectID, pokud chcete tooswitch to příliš`Not Supported`, hello následující:</span><span class="sxs-lookup"><span data-stu-id="2f42f-125">**Subject (sub) claim** This option is defaulted tooObjectID, if you would like tooswitch this too`Not Supported`, do hello following:</span></span>

<span data-ttu-id="2f42f-126">Nahraďte tento řádek</span><span class="sxs-lookup"><span data-stu-id="2f42f-126">Replace this line</span></span> 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
<span data-ttu-id="2f42f-127">Tento řádek:</span><span class="sxs-lookup"><span data-stu-id="2f42f-127">with this line:</span></span>
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a><span data-ttu-id="2f42f-128">Chování relace a jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2f42f-128">Session behavior and SSO</span></span>
<span data-ttu-id="2f42f-129">toochange chování relace a konfigurace jednotného přihlašování, budete potřebovat tooadd `<UserJourneyBehaviors>` element uvnitř hello `<RelyingParty>` elementu.</span><span class="sxs-lookup"><span data-stu-id="2f42f-129">toochange your session behavior and SSO configurations, you need tooadd a `<UserJourneyBehaviors>` element inside of hello `<RelyingParty>` element.</span></span>  <span data-ttu-id="2f42f-130">Hello `<UserJourneyBehaviors>` element okamžitě postupujte hello `<DefaultUserJourney>`.</span><span class="sxs-lookup"><span data-stu-id="2f42f-130">hello `<UserJourneyBehaviors>` element must immediately follow hello `<DefaultUserJourney>`.</span></span>  <span data-ttu-id="2f42f-131">Hello uvnitř vaší `<UserJourneyBehavors>` element by měla vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="2f42f-131">hello inside of your `<UserJourneyBehavors>` element should look like this:</span></span>

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
<span data-ttu-id="2f42f-132">**Konfigurace jednotného přihlašování (SSO)** toochange hello jeden přihlašování konfigurace, je třeba hodnotu hello toomodify `<SingleSignOn>`.</span><span class="sxs-lookup"><span data-stu-id="2f42f-132">**Single sign-on (SSO) configuration** toochange hello single sign-on configuration, you need toomodify hello value of `<SingleSignOn>`.</span></span>  <span data-ttu-id="2f42f-133">Hello použitelné hodnoty jsou `Tenant`, `Application`, `Policy` a `Disabled`.</span><span class="sxs-lookup"><span data-stu-id="2f42f-133">hello applicable values are `Tenant`, `Application`, `Policy` and `Disabled`.</span></span> 

<span data-ttu-id="2f42f-134">**Webovou aplikaci dobu platnosti relace (minuty)** toochange hello hello webové aplikace dobu platnosti relace, musíte hodnotu toomodify hello `<SessionExpiryInSeconds>` elementu.</span><span class="sxs-lookup"><span data-stu-id="2f42f-134">**Web app session lifetime (minutes)** toochange hello hello web app session lifetime, you need toomodify value of hello `<SessionExpiryInSeconds>` element.</span></span>  <span data-ttu-id="2f42f-135">Výchozí hodnota Hello v integrovaných zásad je 86400 sekund (1 440 minut).</span><span class="sxs-lookup"><span data-stu-id="2f42f-135">hello default value in built-in policies is 86400 seconds (1440 minutes).</span></span>

<span data-ttu-id="2f42f-136">**Časový limit relace webové aplikace** toochange hello webové aplikace časový limit relace, musíte hodnotu hello toomodify `<SessionExpiryType>`.</span><span class="sxs-lookup"><span data-stu-id="2f42f-136">**Web app session timeout** toochange hello web app session timeout, you need toomodify hello value of `<SessionExpiryType>`.</span></span>  <span data-ttu-id="2f42f-137">Hello použitelné hodnoty jsou `Absolute` a `Rolling`.</span><span class="sxs-lookup"><span data-stu-id="2f42f-137">hello applicable values are `Absolute` and `Rolling`.</span></span>
