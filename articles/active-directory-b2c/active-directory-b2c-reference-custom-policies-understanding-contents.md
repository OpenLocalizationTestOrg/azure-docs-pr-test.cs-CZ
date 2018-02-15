---
title: "Azure Active Directory B2C: Seznámení vlastní zásady sady starter | Microsoft Docs"
description: "Téma na Azure Active Directory B2C vlastní zásady"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: mtillman
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: joroja
ms.openlocfilehash: fccb6cfddc8629de7db0310340f07bffd1ff8a65
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="understanding-the-custom-policies-of-the-azure-ad-b2c-custom-policy-starter-pack"></a><span data-ttu-id="07670-103">Seznámení s vlastní zásady Startovní sady Azure AD B2C vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="07670-103">Understanding the custom policies of the Azure AD B2C Custom Policy starter pack</span></span>

<span data-ttu-id="07670-104">V této části jsou uvedeny všechny základní prvky B2C_1A_base zásad, která se dodává s **Starter Pack** a který je využít k vytváření vlastních zásad pro prostřednictvím dědičnosti třídy *B2C_1A_base_extensions zásad* .</span><span class="sxs-lookup"><span data-stu-id="07670-104">This section lists all the core elements of the B2C_1A_base policy that comes with the **Starter Pack** and that is leveraged for authoring your own policies through the inheritance of the *B2C_1A_base_extensions policy*.</span></span>

<span data-ttu-id="07670-105">Jako takový ho zejména zaměřen na typy už definované deklarací identity, transformace deklarací, obsahu definice, poskytovatelů deklarací identity s jejich technické profily a cesty uživatele jádra.</span><span class="sxs-lookup"><span data-stu-id="07670-105">As such, it more particularly focusses on the already defined claim types, claims transformations, content definitions, claims providers with their technical profile(s), and the core user journeys.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="07670-106">Společnost Microsoft neposkytuje žádné záruky, vyjádřené nebo předpokládané, s ohledem na informacích uvedených níže.</span><span class="sxs-lookup"><span data-stu-id="07670-106">Microsoft makes no warranties, express or implied, with respect to the information provided hereafter.</span></span> <span data-ttu-id="07670-107">V době GA nebo po může kdykoli před časem GA zavedeny změny.</span><span class="sxs-lookup"><span data-stu-id="07670-107">Changes may be introduced at any time, before GA time, at GA time, or after.</span></span>

<span data-ttu-id="07670-108">Zásady B2C_1A_base_extensions i svoje vlastní zásady můžete přepsat tyto definice a prodloužit tuto zásadu nadřazené tím, že poskytuje další ty, které jsou podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="07670-108">Both your own policies and the B2C_1A_base_extensions policy can override these definitions and extend this parent policy by providing additional ones as needed.</span></span>

<span data-ttu-id="07670-109">Základní prvky *B2C_1A_base zásad* jsou typy deklarací identity, transformace deklarací a definic obsahu.</span><span class="sxs-lookup"><span data-stu-id="07670-109">The core elements of the *B2C_1A_base policy* are claim types, claims transformations, and content definitions.</span></span> <span data-ttu-id="07670-110">Tyto prvky můžete odkazovat ve vlastní zásady, stejně jako v náchylné *B2C_1A_base_extensions zásad*.</span><span class="sxs-lookup"><span data-stu-id="07670-110">These elements can susceptible to be referenced in your own policies as well as in the *B2C_1A_base_extensions policy*.</span></span>

## <a name="claims-schemas"></a><span data-ttu-id="07670-111">Schémata deklarace identity</span><span class="sxs-lookup"><span data-stu-id="07670-111">Claims schemas</span></span>

<span data-ttu-id="07670-112">Tato deklarace identity, schémata je rozdělené do tří částí:</span><span class="sxs-lookup"><span data-stu-id="07670-112">This claims schemas is divided into three sections:</span></span>

1.  <span data-ttu-id="07670-113">První oddíl, který obsahuje minimální deklarace identity, které jsou požadovány pro uživatele cesty správně fungovat.</span><span class="sxs-lookup"><span data-stu-id="07670-113">A first section that lists the minimum claims that are required for the user journeys to work properly.</span></span>
2.  <span data-ttu-id="07670-114">Druhý oddíl, který obsahuje seznam deklarací potřebné pro parametrů řetězce dotazu a další speciální parametry mají být předány jiných zprostředkovatelů deklarací identity, zejména login.microsoftonline.com pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="07670-114">A second section that lists the claims required for query string parameters and other special parameters to be passed to other claims providers, especially login.microsoftonline.com for authentication.</span></span> <span data-ttu-id="07670-115">**Neměňte prosím tyto deklarace**.</span><span class="sxs-lookup"><span data-stu-id="07670-115">**Please do not modify these claims**.</span></span>
3.  <span data-ttu-id="07670-116">A nakonec třetí oddíl, který obsahuje seznam dalších, volitelných deklarace identity, které mohou být shromažďovány z uživatele, uložené v adresáři a odeslány v tokenech během přihlášení.</span><span class="sxs-lookup"><span data-stu-id="07670-116">And eventually a third section that lists any additional, optional claims that can be collected from the user, stored in the directory and sent in tokens during sign in.</span></span> <span data-ttu-id="07670-117">V této části můžete přidat nový typ deklarace identity a shromažďovat od uživatelů nebo odeslat v tokenu.</span><span class="sxs-lookup"><span data-stu-id="07670-117">New claims type to be collected from the user and/or sent in the token can be added in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="07670-118">Schéma deklarací identity obsahuje omezení na určité deklarace identity, jako jsou uživatelská jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="07670-118">The claims schema contains restrictions on certain claims such as passwords and usernames.</span></span> <span data-ttu-id="07670-119">Zásady důvěryhodnosti Framework TF zpracovává Azure AD jako ostatní poskytovatele deklarací identity a všechny její omezení jsou modelována v zásadách premium.</span><span class="sxs-lookup"><span data-stu-id="07670-119">The Trust Framework (TF) policy treats Azure AD as any other claims provider and all its restrictions are modelled in the premium policy.</span></span> <span data-ttu-id="07670-120">Zásady je možné upravovat přidat další omezení, nebo použijte jiného poskytovatele deklarací identity pro úložiště přihlašovacích údajů, který bude mít svůj vlastní omezení.</span><span class="sxs-lookup"><span data-stu-id="07670-120">A policy could be modified to add more restrictions, or use another claims provider for credential storage which will have its own restrictions.</span></span>

<span data-ttu-id="07670-121">Typy deklarací identity k dispozici jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="07670-121">The available claim types are listed below.</span></span>

### <a name="claims-that-are-required-for-the-user-journeys"></a><span data-ttu-id="07670-122">Deklarace identity, které jsou požadovány pro cesty uživatele</span><span class="sxs-lookup"><span data-stu-id="07670-122">Claims that are required for the user journeys</span></span>

<span data-ttu-id="07670-123">Následující deklarace identity jsou požadovány pro uživatele cesty ke správné funkci:</span><span class="sxs-lookup"><span data-stu-id="07670-123">The following claims are required for user journeys to work properly:</span></span>

| <span data-ttu-id="07670-124">Typ deklarace identity</span><span class="sxs-lookup"><span data-stu-id="07670-124">Claims type</span></span> | <span data-ttu-id="07670-125">Popis</span><span class="sxs-lookup"><span data-stu-id="07670-125">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="07670-126">*UserId*</span><span class="sxs-lookup"><span data-stu-id="07670-126">*UserId*</span></span> | <span data-ttu-id="07670-127">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="07670-127">Username</span></span> |
| <span data-ttu-id="07670-128">*signInName*</span><span class="sxs-lookup"><span data-stu-id="07670-128">*signInName*</span></span> | <span data-ttu-id="07670-129">Přihlaste se název</span><span class="sxs-lookup"><span data-stu-id="07670-129">Sign in name</span></span> |
| <span data-ttu-id="07670-130">*tenantId*</span><span class="sxs-lookup"><span data-stu-id="07670-130">*tenantId*</span></span> | <span data-ttu-id="07670-131">Identifikátor klienta (ID) objektu uživatele v Azure AD B2C Premium</span><span class="sxs-lookup"><span data-stu-id="07670-131">Tenant identifier (ID) of the user object in Azure AD B2C Premium</span></span> |
| <span data-ttu-id="07670-132">*objectId*</span><span class="sxs-lookup"><span data-stu-id="07670-132">*objectId*</span></span> | <span data-ttu-id="07670-133">Identifikátor objektu (ID) objektu uživatele v Azure AD B2C Premium</span><span class="sxs-lookup"><span data-stu-id="07670-133">Object identifier (ID) of the user object in Azure AD B2C Premium</span></span> |
| <span data-ttu-id="07670-134">*password*</span><span class="sxs-lookup"><span data-stu-id="07670-134">*password*</span></span> | <span data-ttu-id="07670-135">Heslo</span><span class="sxs-lookup"><span data-stu-id="07670-135">Password</span></span> |
| <span data-ttu-id="07670-136">*newPassword*</span><span class="sxs-lookup"><span data-stu-id="07670-136">*newPassword*</span></span> | |
| <span data-ttu-id="07670-137">*reenterPassword*</span><span class="sxs-lookup"><span data-stu-id="07670-137">*reenterPassword*</span></span> | |
| <span data-ttu-id="07670-138">*passwordPolicies*</span><span class="sxs-lookup"><span data-stu-id="07670-138">*passwordPolicies*</span></span> | <span data-ttu-id="07670-139">Zásady pro hesla používají Azure AD B2C Premium k určení síly hesla, vypršení platnosti, atd.</span><span class="sxs-lookup"><span data-stu-id="07670-139">Password policies used by Azure AD B2C Premium to determine password strength, expiry, etc.</span></span> |
| <span data-ttu-id="07670-140">*sub*</span><span class="sxs-lookup"><span data-stu-id="07670-140">*sub*</span></span> | |
| <span data-ttu-id="07670-141">*alternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="07670-141">*alternativeSecurityId*</span></span> | |
| <span data-ttu-id="07670-142">*identityProvider*</span><span class="sxs-lookup"><span data-stu-id="07670-142">*identityProvider*</span></span> | |
| <span data-ttu-id="07670-143">*displayName*</span><span class="sxs-lookup"><span data-stu-id="07670-143">*displayName*</span></span> | |
| <span data-ttu-id="07670-144">*strongAuthenticationPhoneNumber*</span><span class="sxs-lookup"><span data-stu-id="07670-144">*strongAuthenticationPhoneNumber*</span></span> | <span data-ttu-id="07670-145">Telefonní číslo uživatele</span><span class="sxs-lookup"><span data-stu-id="07670-145">User's telephone number</span></span> |
| <span data-ttu-id="07670-146">*Verified.strongAuthenticationPhoneNumber*</span><span class="sxs-lookup"><span data-stu-id="07670-146">*Verified.strongAuthenticationPhoneNumber*</span></span> | |
| <span data-ttu-id="07670-147">*E-mailu*</span><span class="sxs-lookup"><span data-stu-id="07670-147">*email*</span></span> | <span data-ttu-id="07670-148">E-mailovou adresu, které je možné kontaktovat uživatele</span><span class="sxs-lookup"><span data-stu-id="07670-148">Email address that can be used to contact the user</span></span> |
| <span data-ttu-id="07670-149">*signInNamesInfo.emailAddress*</span><span class="sxs-lookup"><span data-stu-id="07670-149">*signInNamesInfo.emailAddress*</span></span> | <span data-ttu-id="07670-150">E-mailovou adresu, která uživatel může použít pro přihlášení</span><span class="sxs-lookup"><span data-stu-id="07670-150">Email address that the user can use to sign in</span></span> |
| <span data-ttu-id="07670-151">*otherMails*</span><span class="sxs-lookup"><span data-stu-id="07670-151">*otherMails*</span></span> | <span data-ttu-id="07670-152">E-mailové adresy, které lze použít ke kontaktování uživatele</span><span class="sxs-lookup"><span data-stu-id="07670-152">Email addresses that can be used to contact the user</span></span> |
| <span data-ttu-id="07670-153">*userPrincipalName*</span><span class="sxs-lookup"><span data-stu-id="07670-153">*userPrincipalName*</span></span> | <span data-ttu-id="07670-154">Uživatelské jméno dle záznamu v Azure AD B2C Premium</span><span class="sxs-lookup"><span data-stu-id="07670-154">Username as stored in the Azure AD B2C Premium</span></span> |
| <span data-ttu-id="07670-155">*upnUserName*</span><span class="sxs-lookup"><span data-stu-id="07670-155">*upnUserName*</span></span> | <span data-ttu-id="07670-156">Uživatelské jméno pro vytváření hlavní název uživatele</span><span class="sxs-lookup"><span data-stu-id="07670-156">Username for creating user principal name</span></span> |
| <span data-ttu-id="07670-157">*mailNickName*</span><span class="sxs-lookup"><span data-stu-id="07670-157">*mailNickName*</span></span> | <span data-ttu-id="07670-158">Uživatelské jméno e-mailu nick uložen v Azure AD B2C Premium</span><span class="sxs-lookup"><span data-stu-id="07670-158">User's mail nick name as stored in the Azure AD B2C Premium</span></span> |
| <span data-ttu-id="07670-159">*newUser*</span><span class="sxs-lookup"><span data-stu-id="07670-159">*newUser*</span></span> | |
| <span data-ttu-id="07670-160">*executed-SelfAsserted-Input*</span><span class="sxs-lookup"><span data-stu-id="07670-160">*executed-SelfAsserted-Input*</span></span> | <span data-ttu-id="07670-161">Deklarace identity, která určuje, zda byly shromažďovány atributy od uživatele</span><span class="sxs-lookup"><span data-stu-id="07670-161">Claim that specifies whether attributes were collected from the user</span></span> |
| <span data-ttu-id="07670-162">*executed-PhoneFactor-Input*</span><span class="sxs-lookup"><span data-stu-id="07670-162">*executed-PhoneFactor-Input*</span></span> | <span data-ttu-id="07670-163">Deklarace identity, která určuje, zda nové telefonní číslo bylo odebráno od uživatele</span><span class="sxs-lookup"><span data-stu-id="07670-163">Claim that specifies whether a new phone number was collected from the user</span></span> |
| <span data-ttu-id="07670-164">*authenticationSource*</span><span class="sxs-lookup"><span data-stu-id="07670-164">*authenticationSource*</span></span> | <span data-ttu-id="07670-165">Určuje, zda byl uživatel ověřený v sociálních zprostředkovatele Identity, login.microsoftonline.com nebo místní účet</span><span class="sxs-lookup"><span data-stu-id="07670-165">Specifies whether the user was authenticated at Social Identity Provider, login.microsoftonline.com, or local account</span></span> |

### <a name="claims-required-for-query-string-parameters-and-other-special-parameters"></a><span data-ttu-id="07670-166">Deklarace identity, které jsou potřebné pro parametrů řetězce dotazu a další speciální parametry</span><span class="sxs-lookup"><span data-stu-id="07670-166">Claims required for query string parameters and other special parameters</span></span>

<span data-ttu-id="07670-167">Následující deklarace identit vyžadují speciální parametrů (včetně některých parametrů řetězce dotazu) předat do jiných zprostředkovatelů deklarací identity:</span><span class="sxs-lookup"><span data-stu-id="07670-167">The following claims are required to pass on special parameters (including some query string parameters) to other claims providers:</span></span>

| <span data-ttu-id="07670-168">Typ deklarace identity</span><span class="sxs-lookup"><span data-stu-id="07670-168">Claims type</span></span> | <span data-ttu-id="07670-169">Popis</span><span class="sxs-lookup"><span data-stu-id="07670-169">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="07670-170">*nux*</span><span class="sxs-lookup"><span data-stu-id="07670-170">*nux*</span></span> | <span data-ttu-id="07670-171">Speciální parametr předaný pro místní účet ověřování login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="07670-171">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="07670-172">*nca*</span><span class="sxs-lookup"><span data-stu-id="07670-172">*nca*</span></span> | <span data-ttu-id="07670-173">Speciální parametr předaný pro místní účet ověřování login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="07670-173">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="07670-174">*prompt*</span><span class="sxs-lookup"><span data-stu-id="07670-174">*prompt*</span></span> | <span data-ttu-id="07670-175">Speciální parametr předaný pro místní účet ověřování login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="07670-175">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="07670-176">*mkt*</span><span class="sxs-lookup"><span data-stu-id="07670-176">*mkt*</span></span> | <span data-ttu-id="07670-177">Speciální parametr předaný pro místní účet ověřování login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="07670-177">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="07670-178">*lc*</span><span class="sxs-lookup"><span data-stu-id="07670-178">*lc*</span></span> | <span data-ttu-id="07670-179">Speciální parametr předaný pro místní účet ověřování login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="07670-179">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="07670-180">*grant_type*</span><span class="sxs-lookup"><span data-stu-id="07670-180">*grant_type*</span></span> | <span data-ttu-id="07670-181">Speciální parametr předaný pro místní účet ověřování login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="07670-181">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="07670-182">*Obor*</span><span class="sxs-lookup"><span data-stu-id="07670-182">*scope*</span></span> | <span data-ttu-id="07670-183">Speciální parametr předaný pro místní účet ověřování login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="07670-183">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="07670-184">*client_id*</span><span class="sxs-lookup"><span data-stu-id="07670-184">*client_id*</span></span> | <span data-ttu-id="07670-185">Speciální parametr předaný pro místní účet ověřování login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="07670-185">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="07670-186">*objectIdFromSession*</span><span class="sxs-lookup"><span data-stu-id="07670-186">*objectIdFromSession*</span></span> | <span data-ttu-id="07670-187">Zadaný parametr výchozí zprostředkovatel správy relace k označení, že id objektu načtení z relace jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="07670-187">Parameter provided by the default session management provider to indicate that the object id has been retrieved from an SSO session</span></span> |
| <span data-ttu-id="07670-188">*isActiveMFASession*</span><span class="sxs-lookup"><span data-stu-id="07670-188">*isActiveMFASession*</span></span> | <span data-ttu-id="07670-189">Zadaný parametr správou relace MFA k označení, že uživatel má aktivní relaci vícefaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="07670-189">Parameter provided by the MFA session management to indicate that the user has an active MFA session</span></span> |

### <a name="additional-optional-claims-that-can-be-collected"></a><span data-ttu-id="07670-190">Další (volitelné) deklarace identity, které se můžou shromažďovat</span><span class="sxs-lookup"><span data-stu-id="07670-190">Additional (optional) claims that can be collected</span></span>

<span data-ttu-id="07670-191">Následující deklarace identity jsou další deklarace identity, které můžete shromažďovat od uživatelů, uložené v adresáři a odesílají v tokenu.</span><span class="sxs-lookup"><span data-stu-id="07670-191">The following claims are additional claims that can be collected from the users, stored in the directory, and sent in the token.</span></span> <span data-ttu-id="07670-192">Jak je uvedeno před, další deklarace identity lze přidat do tohoto seznamu.</span><span class="sxs-lookup"><span data-stu-id="07670-192">As outlined before, additional claims can be added to this list.</span></span>

| <span data-ttu-id="07670-193">Typ deklarace identity</span><span class="sxs-lookup"><span data-stu-id="07670-193">Claims type</span></span> | <span data-ttu-id="07670-194">Popis</span><span class="sxs-lookup"><span data-stu-id="07670-194">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="07670-195">*givenName*</span><span class="sxs-lookup"><span data-stu-id="07670-195">*givenName*</span></span> | <span data-ttu-id="07670-196">Křestní jméno uživatele (také označované jako křestní jméno)</span><span class="sxs-lookup"><span data-stu-id="07670-196">User's given name (also known as first name)</span></span> |
| <span data-ttu-id="07670-197">*surname*</span><span class="sxs-lookup"><span data-stu-id="07670-197">*surname*</span></span> | <span data-ttu-id="07670-198">Přezdívka uživatele (také označované jako název rodiny nebo příjmení)</span><span class="sxs-lookup"><span data-stu-id="07670-198">User's surname (also known as family name or last name)</span></span> |
| <span data-ttu-id="07670-199">*Extension_picture*</span><span class="sxs-lookup"><span data-stu-id="07670-199">*Extension_picture*</span></span> | <span data-ttu-id="07670-200">Obrázek uživatele ze sociálních</span><span class="sxs-lookup"><span data-stu-id="07670-200">User's picture from social</span></span> |

## <a name="claim-transformations"></a><span data-ttu-id="07670-201">Transformace deklarací identity</span><span class="sxs-lookup"><span data-stu-id="07670-201">Claim transformations</span></span>

<span data-ttu-id="07670-202">Transformace deklarace identity k dispozici jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="07670-202">The available claim transformations are listed below.</span></span>

| <span data-ttu-id="07670-203">Transformace deklarací identity</span><span class="sxs-lookup"><span data-stu-id="07670-203">Claim transformation</span></span> | <span data-ttu-id="07670-204">Popis</span><span class="sxs-lookup"><span data-stu-id="07670-204">Description</span></span> |
|----------------------|-------------|
| <span data-ttu-id="07670-205">*CreateOtherMailsFromEmail*</span><span class="sxs-lookup"><span data-stu-id="07670-205">*CreateOtherMailsFromEmail*</span></span> | |
| <span data-ttu-id="07670-206">*CreateRandomUPNUserName*</span><span class="sxs-lookup"><span data-stu-id="07670-206">*CreateRandomUPNUserName*</span></span> | |
| <span data-ttu-id="07670-207">*CreateUserPrincipalName*</span><span class="sxs-lookup"><span data-stu-id="07670-207">*CreateUserPrincipalName*</span></span> | |
| <span data-ttu-id="07670-208">*CreateSubjectClaimFromObjectID*</span><span class="sxs-lookup"><span data-stu-id="07670-208">*CreateSubjectClaimFromObjectID*</span></span> | |
| <span data-ttu-id="07670-209">*CreateSubjectClaimFromAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="07670-209">*CreateSubjectClaimFromAlternativeSecurityId*</span></span> | |
| <span data-ttu-id="07670-210">*CreateAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="07670-210">*CreateAlternativeSecurityId*</span></span> | |

## <a name="content-definitions"></a><span data-ttu-id="07670-211">Definice obsahu</span><span class="sxs-lookup"><span data-stu-id="07670-211">Content definitions</span></span>

<span data-ttu-id="07670-212">Tato část popisuje obsahu definice již byl deklarován v *B2C_1A_base* zásad.</span><span class="sxs-lookup"><span data-stu-id="07670-212">This section describes the content definitions already declared in the *B2C_1A_base* policy.</span></span> <span data-ttu-id="07670-213">Jsou náchylné k odkazuje, přepsat nebo rozšířené podle potřeby v svoje vlastní zásady, stejně jako v těchto obsahu definice *B2C_1A_base_extensions* zásad.</span><span class="sxs-lookup"><span data-stu-id="07670-213">These content definitions are susceptible to be referenced, overridden, and/or extended as needed in your own policies as well as in the *B2C_1A_base_extensions* policy.</span></span>

| <span data-ttu-id="07670-214">Poskytovatele deklarací identity</span><span class="sxs-lookup"><span data-stu-id="07670-214">Claims provider</span></span> | <span data-ttu-id="07670-215">Popis</span><span class="sxs-lookup"><span data-stu-id="07670-215">Description</span></span> |
|-----------------|-------------|
| <span data-ttu-id="07670-216">*Facebook*</span><span class="sxs-lookup"><span data-stu-id="07670-216">*Facebook*</span></span> | |
| <span data-ttu-id="07670-217">*Místní účet přihlášení*</span><span class="sxs-lookup"><span data-stu-id="07670-217">*Local Account SignIn*</span></span> | |
| <span data-ttu-id="07670-218">*PhoneFactor*</span><span class="sxs-lookup"><span data-stu-id="07670-218">*PhoneFactor*</span></span> | |
| <span data-ttu-id="07670-219">*Azure Active Directory*</span><span class="sxs-lookup"><span data-stu-id="07670-219">*Azure Active Directory*</span></span> | |
| <span data-ttu-id="07670-220">*Vlastní prohlašovanou*</span><span class="sxs-lookup"><span data-stu-id="07670-220">*Self Asserted*</span></span> | |
| <span data-ttu-id="07670-221">*Místní účet*</span><span class="sxs-lookup"><span data-stu-id="07670-221">*Local Account*</span></span> | |
| <span data-ttu-id="07670-222">Správa relací</span><span class="sxs-lookup"><span data-stu-id="07670-222">*Session Management*</span></span> | |
| <span data-ttu-id="07670-223">*Modul zásad Trustframework*</span><span class="sxs-lookup"><span data-stu-id="07670-223">*Trustframework Policy Engine*</span></span> | |
| <span data-ttu-id="07670-224">*TechnicalProfiles*</span><span class="sxs-lookup"><span data-stu-id="07670-224">*TechnicalProfiles*</span></span> | |
| <span data-ttu-id="07670-225">*Vydavatel tokenu*</span><span class="sxs-lookup"><span data-stu-id="07670-225">*Token Issuer*</span></span> | |

## <a name="technical-profiles"></a><span data-ttu-id="07670-226">Technické profily</span><span class="sxs-lookup"><span data-stu-id="07670-226">Technical profiles</span></span>

<span data-ttu-id="07670-227">Znázorňuje technické profily už deklarovaný za poskytovatele deklarací identity v této části *B2C_1A_base* zásad.</span><span class="sxs-lookup"><span data-stu-id="07670-227">This section depicts the technical profiles already declared per claim provider in the *B2C_1A_base* policy.</span></span> <span data-ttu-id="07670-228">Tyto technické profily jsou náchylné k se dál odkazovat, přepsat nebo rozšířené podle potřeby v svoje vlastní zásady, stejně jako v *B2C_1A_base_extensions* zásad.</span><span class="sxs-lookup"><span data-stu-id="07670-228">These technical profiles are susceptible to be further referenced, overridden, and/or extended as needed in your own policies as well as in the *B2C_1A_base_extensions* policy.</span></span>

### <a name="technical-profiles-for-facebook"></a><span data-ttu-id="07670-229">Technické profily pro Facebook</span><span class="sxs-lookup"><span data-stu-id="07670-229">Technical profiles for Facebook</span></span>

| <span data-ttu-id="07670-230">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="07670-230">Technical profile</span></span> | <span data-ttu-id="07670-231">Popis</span><span class="sxs-lookup"><span data-stu-id="07670-231">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="07670-232">*Facebook-OAUTH*</span><span class="sxs-lookup"><span data-stu-id="07670-232">*Facebook-OAUTH*</span></span> | |

### <a name="technical-profiles-for-local-account-signin"></a><span data-ttu-id="07670-233">Technické profily pro místní přihlášení účtu</span><span class="sxs-lookup"><span data-stu-id="07670-233">Technical profiles for Local Account Signin</span></span>

| <span data-ttu-id="07670-234">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="07670-234">Technical profile</span></span> | <span data-ttu-id="07670-235">Popis</span><span class="sxs-lookup"><span data-stu-id="07670-235">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="07670-236">*Login-NonInteractive*</span><span class="sxs-lookup"><span data-stu-id="07670-236">*Login-NonInteractive*</span></span> | |

### <a name="technical-profiles-for-phone-factor"></a><span data-ttu-id="07670-237">Technické profily pro Phone Factor</span><span class="sxs-lookup"><span data-stu-id="07670-237">Technical profiles for Phone Factor</span></span>

| <span data-ttu-id="07670-238">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="07670-238">Technical profile</span></span> | <span data-ttu-id="07670-239">Popis</span><span class="sxs-lookup"><span data-stu-id="07670-239">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="07670-240">*PhoneFactor-Input*</span><span class="sxs-lookup"><span data-stu-id="07670-240">*PhoneFactor-Input*</span></span> | |
| <span data-ttu-id="07670-241">*PhoneFactor-InputOrVerify*</span><span class="sxs-lookup"><span data-stu-id="07670-241">*PhoneFactor-InputOrVerify*</span></span> | |
| <span data-ttu-id="07670-242">*PhoneFactor-Verify*</span><span class="sxs-lookup"><span data-stu-id="07670-242">*PhoneFactor-Verify*</span></span> | |

### <a name="technical-profiles-for-azure-active-directory"></a><span data-ttu-id="07670-243">Technické profily pro Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="07670-243">Technical profiles for Azure Active Directory</span></span>

| <span data-ttu-id="07670-244">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="07670-244">Technical profile</span></span> | <span data-ttu-id="07670-245">Popis</span><span class="sxs-lookup"><span data-stu-id="07670-245">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="07670-246">*Běžné AAD*</span><span class="sxs-lookup"><span data-stu-id="07670-246">*AAD-Common*</span></span> | <span data-ttu-id="07670-247">Technické profil podle jiných AAD xxx technické profilů</span><span class="sxs-lookup"><span data-stu-id="07670-247">Technical profile included by the other AAD-xxx technical profiles</span></span> |
| <span data-ttu-id="07670-248">*AAD-UserWriteUsingAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="07670-248">*AAD-UserWriteUsingAlternativeSecurityId*</span></span> | <span data-ttu-id="07670-249">Technické profil pro sociálních přihlášení</span><span class="sxs-lookup"><span data-stu-id="07670-249">Technical profile for social logins</span></span> |
| <span data-ttu-id="07670-250">*AAD-UserReadUsingAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="07670-250">*AAD-UserReadUsingAlternativeSecurityId*</span></span> | <span data-ttu-id="07670-251">Technické profil pro sociálních přihlášení</span><span class="sxs-lookup"><span data-stu-id="07670-251">Technical profile for social logins</span></span> |
| <span data-ttu-id="07670-252">*AAD-UserReadUsingAlternativeSecurityId-NoError*</span><span class="sxs-lookup"><span data-stu-id="07670-252">*AAD-UserReadUsingAlternativeSecurityId-NoError*</span></span> | <span data-ttu-id="07670-253">Technické profil pro sociálních přihlášení</span><span class="sxs-lookup"><span data-stu-id="07670-253">Technical profile for social logins</span></span> |
| <span data-ttu-id="07670-254">*AAD-UserWritePasswordUsingLogonEmail*</span><span class="sxs-lookup"><span data-stu-id="07670-254">*AAD-UserWritePasswordUsingLogonEmail*</span></span> | <span data-ttu-id="07670-255">Technické profil pro místní účty</span><span class="sxs-lookup"><span data-stu-id="07670-255">Technical profile for local accounts</span></span> |
| <span data-ttu-id="07670-256">*AAD-UserReadUsingEmailAddress*</span><span class="sxs-lookup"><span data-stu-id="07670-256">*AAD-UserReadUsingEmailAddress*</span></span> | <span data-ttu-id="07670-257">Technické profil pro místní účty</span><span class="sxs-lookup"><span data-stu-id="07670-257">Technical profile for local accounts</span></span> |
| <span data-ttu-id="07670-258">*AAD-UserWriteProfileUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="07670-258">*AAD-UserWriteProfileUsingObjectId*</span></span> | <span data-ttu-id="07670-259">Aktualizace záznamu uživatele pomocí objectId technické profilu</span><span class="sxs-lookup"><span data-stu-id="07670-259">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="07670-260">*AAD-UserWritePhoneNumberUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="07670-260">*AAD-UserWritePhoneNumberUsingObjectId*</span></span> | <span data-ttu-id="07670-261">Aktualizace záznamu uživatele pomocí objectId technické profilu</span><span class="sxs-lookup"><span data-stu-id="07670-261">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="07670-262">*AAD-UserWritePasswordUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="07670-262">*AAD-UserWritePasswordUsingObjectId*</span></span> | <span data-ttu-id="07670-263">Aktualizace záznamu uživatele pomocí objectId technické profilu</span><span class="sxs-lookup"><span data-stu-id="07670-263">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="07670-264">*AAD-UserReadUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="07670-264">*AAD-UserReadUsingObjectId*</span></span> | <span data-ttu-id="07670-265">Technické profil slouží k načtení dat po ověření uživatele</span><span class="sxs-lookup"><span data-stu-id="07670-265">Technical profile is used to read data after user authenticates</span></span> |

### <a name="technical-profiles-for-self-asserted"></a><span data-ttu-id="07670-266">Technické profily pro samoobslužné prohlašovanou</span><span class="sxs-lookup"><span data-stu-id="07670-266">Technical profiles for Self Asserted</span></span>

| <span data-ttu-id="07670-267">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="07670-267">Technical profile</span></span> | <span data-ttu-id="07670-268">Popis</span><span class="sxs-lookup"><span data-stu-id="07670-268">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="07670-269">*SelfAsserted-Social*</span><span class="sxs-lookup"><span data-stu-id="07670-269">*SelfAsserted-Social*</span></span> | |
| <span data-ttu-id="07670-270">*SelfAsserted-ProfileUpdate*</span><span class="sxs-lookup"><span data-stu-id="07670-270">*SelfAsserted-ProfileUpdate*</span></span> | |

### <a name="technical-profiles-for-local-account"></a><span data-ttu-id="07670-271">Technické profily pro místní účet</span><span class="sxs-lookup"><span data-stu-id="07670-271">Technical profiles for Local Account</span></span>

| <span data-ttu-id="07670-272">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="07670-272">Technical profile</span></span> | <span data-ttu-id="07670-273">Popis</span><span class="sxs-lookup"><span data-stu-id="07670-273">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="07670-274">*LocalAccountSignUpWithLogonEmail*</span><span class="sxs-lookup"><span data-stu-id="07670-274">*LocalAccountSignUpWithLogonEmail*</span></span> | |

### <a name="technical-profiles-for-session-management"></a><span data-ttu-id="07670-275">Technické profilů pro správu relací</span><span class="sxs-lookup"><span data-stu-id="07670-275">Technical profiles for Session Management</span></span>

| <span data-ttu-id="07670-276">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="07670-276">Technical profile</span></span> | <span data-ttu-id="07670-277">Popis</span><span class="sxs-lookup"><span data-stu-id="07670-277">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="07670-278">*SM-nedojde k žádné akci*</span><span class="sxs-lookup"><span data-stu-id="07670-278">*SM-Noop*</span></span> | |
| <span data-ttu-id="07670-279">*SM-AAD*</span><span class="sxs-lookup"><span data-stu-id="07670-279">*SM-AAD*</span></span> | |
| <span data-ttu-id="07670-280">*SM SocialSignup*</span><span class="sxs-lookup"><span data-stu-id="07670-280">*SM-SocialSignup*</span></span> | <span data-ttu-id="07670-281">Název profilu se používá k rozlišení AAD relace mezi přihlašovací nahoru a přihlášení</span><span class="sxs-lookup"><span data-stu-id="07670-281">Profile name is being used to disambiguate AAD session between sign up and sign in</span></span> |
| <span data-ttu-id="07670-282">*SM-SocialLogin*</span><span class="sxs-lookup"><span data-stu-id="07670-282">*SM-SocialLogin*</span></span> | |
| <span data-ttu-id="07670-283">*SM-MFA*</span><span class="sxs-lookup"><span data-stu-id="07670-283">*SM-MFA*</span></span> | |

### <a name="technical-profiles-for-trustframework-policy-engine-technicalprofiles"></a><span data-ttu-id="07670-284">Technické profily pro TechnicalProfiles modul zásad Trustframework</span><span class="sxs-lookup"><span data-stu-id="07670-284">Technical profiles for Trustframework Policy Engine TechnicalProfiles</span></span>

<span data-ttu-id="07670-285">V současné době jsou definovány žádné technické profily pro **TechnicalProfiles modul zásad Trustframework** poskytovatele deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="07670-285">Currently, no technical profiles are defined for the **Trustframework Policy Engine TechnicalProfiles** claims provider.</span></span>

### <a name="technical-profiles-for-token-issuer"></a><span data-ttu-id="07670-286">Technické profily pro vydavatel tokenu</span><span class="sxs-lookup"><span data-stu-id="07670-286">Technical profiles for Token Issuer</span></span>

| <span data-ttu-id="07670-287">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="07670-287">Technical profile</span></span> | <span data-ttu-id="07670-288">Popis</span><span class="sxs-lookup"><span data-stu-id="07670-288">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="07670-289">*JwtIssuer*</span><span class="sxs-lookup"><span data-stu-id="07670-289">*JwtIssuer*</span></span> | |

## <a name="user-journeys"></a><span data-ttu-id="07670-290">Uživatel cesty</span><span class="sxs-lookup"><span data-stu-id="07670-290">User journeys</span></span>

<span data-ttu-id="07670-291">Znázorňuje cesty uživatel již byl deklarován v této části *B2C_1A_base* zásad.</span><span class="sxs-lookup"><span data-stu-id="07670-291">This section depicts the user journeys already declared in the *B2C_1A_base* policy.</span></span> <span data-ttu-id="07670-292">Tyto cesty uživatele jsou náchylné k se dál odkazovat, přepsat nebo rozšířené podle potřeby v svoje vlastní zásady, stejně jako v *B2C_1A_base_extensions* zásad.</span><span class="sxs-lookup"><span data-stu-id="07670-292">These user journeys are susceptible to be further referenced, overridden, and/or extended as needed in your own policies as well as in the *B2C_1A_base_extensions* policy.</span></span>

| <span data-ttu-id="07670-293">Uživatel cesty</span><span class="sxs-lookup"><span data-stu-id="07670-293">User journey</span></span> | <span data-ttu-id="07670-294">Popis</span><span class="sxs-lookup"><span data-stu-id="07670-294">Description</span></span> |
|--------------|-------------|
| <span data-ttu-id="07670-295">*Registrace*</span><span class="sxs-lookup"><span data-stu-id="07670-295">*SignUp*</span></span> | |
| <span data-ttu-id="07670-296">*SignIn*</span><span class="sxs-lookup"><span data-stu-id="07670-296">*SignIn*</span></span> | |
| <span data-ttu-id="07670-297">*SignUpOrSignIn*</span><span class="sxs-lookup"><span data-stu-id="07670-297">*SignUpOrSignIn*</span></span> | |
| <span data-ttu-id="07670-298">*EditProfile*</span><span class="sxs-lookup"><span data-stu-id="07670-298">*EditProfile*</span></span> | |
| <span data-ttu-id="07670-299">*PasswordReset*</span><span class="sxs-lookup"><span data-stu-id="07670-299">*PasswordReset*</span></span> | |
