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
# <a name="understanding-hello-custom-policies-of-hello-azure-ad-b2c-custom-policy-starter-pack"></a><span data-ttu-id="bc739-103">Principy hello vlastní zásady vlastní zásady pro spuštění sady hello Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="bc739-103">Understanding hello custom policies of hello Azure AD B2C Custom Policy starter pack</span></span>

<span data-ttu-id="bc739-104">Tato část uvádí všechny elementy základní hello hello B2C_1A_base zásad, která se dodává s hello **Starter Pack** a který je využít k vytváření vlastních zásad prostřednictvím hello dědičnosti třídy hello *B2C_1A_base_ rozšíření zásad*.</span><span class="sxs-lookup"><span data-stu-id="bc739-104">This section lists all hello core elements of hello B2C_1A_base policy that comes with hello **Starter Pack** and that is leveraged for authoring your own policies through hello inheritance of hello *B2C_1A_base_extensions policy*.</span></span>

<span data-ttu-id="bc739-105">Jako takový ho více zvlášť zaměřen na hello již definována deklarace identity typy, transformace deklarací, obsahu definice, poskytovatelů deklarací identity s jejich technické profily a hello základní uživatelské cesty.</span><span class="sxs-lookup"><span data-stu-id="bc739-105">As such, it more particularly focusses on hello already defined claim types, claims transformations, content definitions, claims providers with their technical profile(s), and hello core user journeys.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bc739-106">Společnost Microsoft neposkytuje žádné záruky, vyjádřené nebo předpokládané, s ohledem na toohello informací uvedených níže.</span><span class="sxs-lookup"><span data-stu-id="bc739-106">Microsoft makes no warranties, express or implied, with respect toohello information provided hereafter.</span></span> <span data-ttu-id="bc739-107">V době GA nebo po může kdykoli před časem GA zavedeny změny.</span><span class="sxs-lookup"><span data-stu-id="bc739-107">Changes may be introduced at any time, before GA time, at GA time, or after.</span></span>

<span data-ttu-id="bc739-108">Vlastní zásady a hello B2C_1A_base_extensions zásady můžete přepsat tyto definice a prodloužit tuto zásadu nadřazené tím, že poskytuje další ty, které jsou podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="bc739-108">Both your own policies and hello B2C_1A_base_extensions policy can override these definitions and extend this parent policy by providing additional ones as needed.</span></span>

<span data-ttu-id="bc739-109">Hello základní prvky hello *B2C_1A_base zásad* jsou typy deklarací identity, transformace deklarací a definic obsahu.</span><span class="sxs-lookup"><span data-stu-id="bc739-109">hello core elements of hello *B2C_1A_base policy* are claim types, claims transformations, and content definitions.</span></span> <span data-ttu-id="bc739-110">Tyto prvky můžete náchylný toobe odkazovaný ve vlastní zásady také jako hello *B2C_1A_base_extensions zásad*.</span><span class="sxs-lookup"><span data-stu-id="bc739-110">These elements can susceptible toobe referenced in your own policies as well as in hello *B2C_1A_base_extensions policy*.</span></span>

## <a name="claims-schemas"></a><span data-ttu-id="bc739-111">Schémata deklarace identity</span><span class="sxs-lookup"><span data-stu-id="bc739-111">Claims schemas</span></span>

<span data-ttu-id="bc739-112">Tato deklarace identity, schémata je rozdělené do tří částí:</span><span class="sxs-lookup"><span data-stu-id="bc739-112">This claims schemas is divided into three sections:</span></span>

1.  <span data-ttu-id="bc739-113">První oddíl, který obsahuje seznam hello minimální deklarací, které jsou požadovány pro hello uživatele cesty toowork správně.</span><span class="sxs-lookup"><span data-stu-id="bc739-113">A first section that lists hello minimum claims that are required for hello user journeys toowork properly.</span></span>
2.  <span data-ttu-id="bc739-114">Druhý oddíl, seznamy hello deklarace identity, které jsou potřebné pro parametrů řetězce dotazu a jiné speciální parametry toobe předána tooother zprostředkovatelů deklarací identit, zejména login.microsoftonline.com pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="bc739-114">A second section that lists hello claims required for query string parameters and other special parameters toobe passed tooother claims providers, especially login.microsoftonline.com for authentication.</span></span> <span data-ttu-id="bc739-115">**Neměňte prosím tyto deklarace**.</span><span class="sxs-lookup"><span data-stu-id="bc739-115">**Please do not modify these claims**.</span></span>
3.  <span data-ttu-id="bc739-116">A nakonec třetí oddíl, který obsahuje seznam dalších, volitelných deklarace identity, které mohou být shromažďovány z hello uživatele uložené v adresáři hello a odeslány v tokenech během přihlášení.</span><span class="sxs-lookup"><span data-stu-id="bc739-116">And eventually a third section that lists any additional, optional claims that can be collected from hello user, stored in hello directory and sent in tokens during sign in.</span></span> <span data-ttu-id="bc739-117">V této části mohou být přidány nové deklarace identity typu toobe shromážděných z hello uživatele nebo odeslat v tokenu hello.</span><span class="sxs-lookup"><span data-stu-id="bc739-117">New claims type toobe collected from hello user and/or sent in hello token can be added in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bc739-118">schéma Hello deklarací identity obsahuje omezení na určité deklarace identity, jako jsou uživatelská jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="bc739-118">hello claims schema contains restrictions on certain claims such as passwords and usernames.</span></span> <span data-ttu-id="bc739-119">Hello zásady důvěryhodnosti Framework TF zpracovává Azure AD jako ostatní poskytovatele deklarací identity a všechny její omezení jsou modelována v hello premium zásad.</span><span class="sxs-lookup"><span data-stu-id="bc739-119">hello Trust Framework (TF) policy treats Azure AD as any other claims provider and all its restrictions are modelled in hello premium policy.</span></span> <span data-ttu-id="bc739-120">Zásady může být upravený tooadd další omezení, nebo pomocí jiného poskytovatele deklarací identity pro úložiště přihlašovacích údajů, který bude mít svůj vlastní omezení.</span><span class="sxs-lookup"><span data-stu-id="bc739-120">A policy could be modified tooadd more restrictions, or use another claims provider for credential storage which will have its own restrictions.</span></span>

<span data-ttu-id="bc739-121">Níže jsou uvedeny typy deklarací identity k dispozici Hello.</span><span class="sxs-lookup"><span data-stu-id="bc739-121">hello available claim types are listed below.</span></span>

### <a name="claims-that-are-required-for-hello-user-journeys"></a><span data-ttu-id="bc739-122">Deklarace identity, které jsou požadovány pro cesty uživatele hello</span><span class="sxs-lookup"><span data-stu-id="bc739-122">Claims that are required for hello user journeys</span></span>

<span data-ttu-id="bc739-123">Hello následující deklarace identity jsou požadovány pro uživatele cesty toowork správně:</span><span class="sxs-lookup"><span data-stu-id="bc739-123">hello following claims are required for user journeys toowork properly:</span></span>

| <span data-ttu-id="bc739-124">Typ deklarace identity</span><span class="sxs-lookup"><span data-stu-id="bc739-124">Claims type</span></span> | <span data-ttu-id="bc739-125">Popis</span><span class="sxs-lookup"><span data-stu-id="bc739-125">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="bc739-126">*ID uživatele*</span><span class="sxs-lookup"><span data-stu-id="bc739-126">*UserId*</span></span> | <span data-ttu-id="bc739-127">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="bc739-127">Username</span></span> |
| <span data-ttu-id="bc739-128">*signInName*</span><span class="sxs-lookup"><span data-stu-id="bc739-128">*signInName*</span></span> | <span data-ttu-id="bc739-129">Přihlaste se název</span><span class="sxs-lookup"><span data-stu-id="bc739-129">Sign in name</span></span> |
| <span data-ttu-id="bc739-130">*tenantId*</span><span class="sxs-lookup"><span data-stu-id="bc739-130">*tenantId*</span></span> | <span data-ttu-id="bc739-131">Identifikátor klienta (ID) hello objektu uživatele v Azure AD B2C Premium</span><span class="sxs-lookup"><span data-stu-id="bc739-131">Tenant identifier (ID) of hello user object in Azure AD B2C Premium</span></span> |
| <span data-ttu-id="bc739-132">*objectId*</span><span class="sxs-lookup"><span data-stu-id="bc739-132">*objectId*</span></span> | <span data-ttu-id="bc739-133">Identifikátor objektu (ID) hello objektu uživatele v Azure AD B2C Premium</span><span class="sxs-lookup"><span data-stu-id="bc739-133">Object identifier (ID) of hello user object in Azure AD B2C Premium</span></span> |
| <span data-ttu-id="bc739-134">*heslo*</span><span class="sxs-lookup"><span data-stu-id="bc739-134">*password*</span></span> | <span data-ttu-id="bc739-135">Heslo</span><span class="sxs-lookup"><span data-stu-id="bc739-135">Password</span></span> |
| <span data-ttu-id="bc739-136">*nové_heslo*</span><span class="sxs-lookup"><span data-stu-id="bc739-136">*newPassword*</span></span> | |
| <span data-ttu-id="bc739-137">*reenterPassword*</span><span class="sxs-lookup"><span data-stu-id="bc739-137">*reenterPassword*</span></span> | |
| <span data-ttu-id="bc739-138">*passwordPolicies*</span><span class="sxs-lookup"><span data-stu-id="bc739-138">*passwordPolicies*</span></span> | <span data-ttu-id="bc739-139">Zásady pro hesla používané síly hesla toodetermine Premium Azure AD B2C, vypršení platnosti, atd.</span><span class="sxs-lookup"><span data-stu-id="bc739-139">Password policies used by Azure AD B2C Premium toodetermine password strength, expiry, etc.</span></span> |
| <span data-ttu-id="bc739-140">*Sub –*</span><span class="sxs-lookup"><span data-stu-id="bc739-140">*sub*</span></span> | |
| <span data-ttu-id="bc739-141">*alternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="bc739-141">*alternativeSecurityId*</span></span> | |
| <span data-ttu-id="bc739-142">*identityProvider*</span><span class="sxs-lookup"><span data-stu-id="bc739-142">*identityProvider*</span></span> | |
| <span data-ttu-id="bc739-143">*displayName*</span><span class="sxs-lookup"><span data-stu-id="bc739-143">*displayName*</span></span> | |
| <span data-ttu-id="bc739-144">*strongAuthenticationPhoneNumber*</span><span class="sxs-lookup"><span data-stu-id="bc739-144">*strongAuthenticationPhoneNumber*</span></span> | <span data-ttu-id="bc739-145">Telefonní číslo uživatele</span><span class="sxs-lookup"><span data-stu-id="bc739-145">User's telephone number</span></span> |
| <span data-ttu-id="bc739-146">*Verified.strongAuthenticationPhoneNumber*</span><span class="sxs-lookup"><span data-stu-id="bc739-146">*Verified.strongAuthenticationPhoneNumber*</span></span> | |
| <span data-ttu-id="bc739-147">*e-mailu*</span><span class="sxs-lookup"><span data-stu-id="bc739-147">*email*</span></span> | <span data-ttu-id="bc739-148">E-mailovou adresu, kterou lze použít toocontact hello uživatele</span><span class="sxs-lookup"><span data-stu-id="bc739-148">Email address that can be used toocontact hello user</span></span> |
| <span data-ttu-id="bc739-149">*signInNamesInfo.emailAddress*</span><span class="sxs-lookup"><span data-stu-id="bc739-149">*signInNamesInfo.emailAddress*</span></span> | <span data-ttu-id="bc739-150">E-mailovou adresu, která hello uživatele můžete použít toosign v</span><span class="sxs-lookup"><span data-stu-id="bc739-150">Email address that hello user can use toosign in</span></span> |
| <span data-ttu-id="bc739-151">*otherMails*</span><span class="sxs-lookup"><span data-stu-id="bc739-151">*otherMails*</span></span> | <span data-ttu-id="bc739-152">E-mailové adresy, které se dají použít toocontact hello uživatele</span><span class="sxs-lookup"><span data-stu-id="bc739-152">Email addresses that can be used toocontact hello user</span></span> |
| <span data-ttu-id="bc739-153">*userPrincipalName*</span><span class="sxs-lookup"><span data-stu-id="bc739-153">*userPrincipalName*</span></span> | <span data-ttu-id="bc739-154">Uživatelské jméno, jak je uložen v hello Azure AD B2C Premium</span><span class="sxs-lookup"><span data-stu-id="bc739-154">Username as stored in hello Azure AD B2C Premium</span></span> |
| <span data-ttu-id="bc739-155">*upnUserName*</span><span class="sxs-lookup"><span data-stu-id="bc739-155">*upnUserName*</span></span> | <span data-ttu-id="bc739-156">Uživatelské jméno pro vytváření hlavní název uživatele</span><span class="sxs-lookup"><span data-stu-id="bc739-156">Username for creating user principal name</span></span> |
| <span data-ttu-id="bc739-157">*mailNickName*</span><span class="sxs-lookup"><span data-stu-id="bc739-157">*mailNickName*</span></span> | <span data-ttu-id="bc739-158">Uživatelské jméno e-mailu nick uložen v hello Azure AD B2C Premium</span><span class="sxs-lookup"><span data-stu-id="bc739-158">User's mail nick name as stored in hello Azure AD B2C Premium</span></span> |
| <span data-ttu-id="bc739-159">*newUser*</span><span class="sxs-lookup"><span data-stu-id="bc739-159">*newUser*</span></span> | |
| <span data-ttu-id="bc739-160">*provést vstup SelfAsserted*</span><span class="sxs-lookup"><span data-stu-id="bc739-160">*executed-SelfAsserted-Input*</span></span> | <span data-ttu-id="bc739-161">Deklarace identity, která určuje, zda byly shromažďovány atributy od uživatele hello</span><span class="sxs-lookup"><span data-stu-id="bc739-161">Claim that specifies whether attributes were collected from hello user</span></span> |
| <span data-ttu-id="bc739-162">*provést vstup PhoneFactor*</span><span class="sxs-lookup"><span data-stu-id="bc739-162">*executed-PhoneFactor-Input*</span></span> | <span data-ttu-id="bc739-163">Deklarace identity, která určuje, zda nové telefonní číslo bylo odebráno od uživatele hello</span><span class="sxs-lookup"><span data-stu-id="bc739-163">Claim that specifies whether a new phone number was collected from hello user</span></span> |
| <span data-ttu-id="bc739-164">*authenticationSource*</span><span class="sxs-lookup"><span data-stu-id="bc739-164">*authenticationSource*</span></span> | <span data-ttu-id="bc739-165">Určuje, zda byla ověřena hello uživatele na sociálních zprostředkovatele Identity, login.microsoftonline.com nebo místní účet</span><span class="sxs-lookup"><span data-stu-id="bc739-165">Specifies whether hello user was authenticated at Social Identity Provider, login.microsoftonline.com, or local account</span></span> |

### <a name="claims-required-for-query-string-parameters-and-other-special-parameters"></a><span data-ttu-id="bc739-166">Deklarace identity, které jsou potřebné pro parametrů řetězce dotazu a další speciální parametry</span><span class="sxs-lookup"><span data-stu-id="bc739-166">Claims required for query string parameters and other special parameters</span></span>

<span data-ttu-id="bc739-167">Hello následující deklarace identity jsou požadované toopass na zprostředkovatelů deklarací identit tooother speciální parametry (včetně některých parametrů řetězce dotazu):</span><span class="sxs-lookup"><span data-stu-id="bc739-167">hello following claims are required toopass on special parameters (including some query string parameters) tooother claims providers:</span></span>

| <span data-ttu-id="bc739-168">Typ deklarace identity</span><span class="sxs-lookup"><span data-stu-id="bc739-168">Claims type</span></span> | <span data-ttu-id="bc739-169">Popis</span><span class="sxs-lookup"><span data-stu-id="bc739-169">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="bc739-170">*Nux*</span><span class="sxs-lookup"><span data-stu-id="bc739-170">*nux*</span></span> | <span data-ttu-id="bc739-171">Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr.</span><span class="sxs-lookup"><span data-stu-id="bc739-171">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="bc739-172">*NCA*</span><span class="sxs-lookup"><span data-stu-id="bc739-172">*nca*</span></span> | <span data-ttu-id="bc739-173">Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr.</span><span class="sxs-lookup"><span data-stu-id="bc739-173">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="bc739-174">*řádku*</span><span class="sxs-lookup"><span data-stu-id="bc739-174">*prompt*</span></span> | <span data-ttu-id="bc739-175">Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr.</span><span class="sxs-lookup"><span data-stu-id="bc739-175">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="bc739-176">*mkt*</span><span class="sxs-lookup"><span data-stu-id="bc739-176">*mkt*</span></span> | <span data-ttu-id="bc739-177">Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr.</span><span class="sxs-lookup"><span data-stu-id="bc739-177">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="bc739-178">*LC*</span><span class="sxs-lookup"><span data-stu-id="bc739-178">*lc*</span></span> | <span data-ttu-id="bc739-179">Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr.</span><span class="sxs-lookup"><span data-stu-id="bc739-179">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="bc739-180">*grant_type*</span><span class="sxs-lookup"><span data-stu-id="bc739-180">*grant_type*</span></span> | <span data-ttu-id="bc739-181">Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr.</span><span class="sxs-lookup"><span data-stu-id="bc739-181">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="bc739-182">*obor*</span><span class="sxs-lookup"><span data-stu-id="bc739-182">*scope*</span></span> | <span data-ttu-id="bc739-183">Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr.</span><span class="sxs-lookup"><span data-stu-id="bc739-183">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="bc739-184">*client_id*</span><span class="sxs-lookup"><span data-stu-id="bc739-184">*client_id*</span></span> | <span data-ttu-id="bc739-185">Byl předán pro místní účet ověřování toologin.microsoftonline.com speciální parametr.</span><span class="sxs-lookup"><span data-stu-id="bc739-185">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="bc739-186">*objectIdFromSession*</span><span class="sxs-lookup"><span data-stu-id="bc739-186">*objectIdFromSession*</span></span> | <span data-ttu-id="bc739-187">Parametr poskytované systém hello výchozí relace správy zprostředkovatele tooindicate hello id objektu načtení z relace jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bc739-187">Parameter provided by hello default session management provider tooindicate that hello object id has been retrieved from an SSO session</span></span> |
| <span data-ttu-id="bc739-188">*isActiveMFASession*</span><span class="sxs-lookup"><span data-stu-id="bc739-188">*isActiveMFASession*</span></span> | <span data-ttu-id="bc739-189">Parametr poskytované hello MFA relace správy tooindicate, že tento uživatel hello má aktivní relaci vícefaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="bc739-189">Parameter provided by hello MFA session management tooindicate that hello user has an active MFA session</span></span> |

### <a name="additional-optional-claims-that-can-be-collected"></a><span data-ttu-id="bc739-190">Další (volitelné) deklarace identity, které se můžou shromažďovat</span><span class="sxs-lookup"><span data-stu-id="bc739-190">Additional (optional) claims that can be collected</span></span>

<span data-ttu-id="bc739-191">Následující Hello deklarací se další deklarace identity, které mohou být shromažďovány z hello uživatele uložené v adresáři hello a odesílají v tokenu hello.</span><span class="sxs-lookup"><span data-stu-id="bc739-191">hello following claims are additional claims that can be collected from hello users, stored in hello directory, and sent in hello token.</span></span> <span data-ttu-id="bc739-192">Jak je uvedeno před, další deklarace identity se dá přidat toothis seznamu.</span><span class="sxs-lookup"><span data-stu-id="bc739-192">As outlined before, additional claims can be added toothis list.</span></span>

| <span data-ttu-id="bc739-193">Typ deklarace identity</span><span class="sxs-lookup"><span data-stu-id="bc739-193">Claims type</span></span> | <span data-ttu-id="bc739-194">Popis</span><span class="sxs-lookup"><span data-stu-id="bc739-194">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="bc739-195">*givenName*</span><span class="sxs-lookup"><span data-stu-id="bc739-195">*givenName*</span></span> | <span data-ttu-id="bc739-196">Křestní jméno uživatele (také označované jako křestní jméno)</span><span class="sxs-lookup"><span data-stu-id="bc739-196">User's given name (also known as first name)</span></span> |
| <span data-ttu-id="bc739-197">*Příjmení*</span><span class="sxs-lookup"><span data-stu-id="bc739-197">*surname*</span></span> | <span data-ttu-id="bc739-198">Přezdívka uživatele (také označované jako název rodiny nebo příjmení)</span><span class="sxs-lookup"><span data-stu-id="bc739-198">User's surname (also known as family name or last name)</span></span> |
| <span data-ttu-id="bc739-199">*Extension_picture*</span><span class="sxs-lookup"><span data-stu-id="bc739-199">*Extension_picture*</span></span> | <span data-ttu-id="bc739-200">Obrázek uživatele ze sociálních</span><span class="sxs-lookup"><span data-stu-id="bc739-200">User's picture from social</span></span> |

## <a name="claim-transformations"></a><span data-ttu-id="bc739-201">Transformace deklarací identity</span><span class="sxs-lookup"><span data-stu-id="bc739-201">Claim transformations</span></span>

<span data-ttu-id="bc739-202">transformace deklarace identity k dispozici Hello jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="bc739-202">hello available claim transformations are listed below.</span></span>

| <span data-ttu-id="bc739-203">Transformace deklarací identity</span><span class="sxs-lookup"><span data-stu-id="bc739-203">Claim transformation</span></span> | <span data-ttu-id="bc739-204">Popis</span><span class="sxs-lookup"><span data-stu-id="bc739-204">Description</span></span> |
|----------------------|-------------|
| <span data-ttu-id="bc739-205">*CreateOtherMailsFromEmail*</span><span class="sxs-lookup"><span data-stu-id="bc739-205">*CreateOtherMailsFromEmail*</span></span> | |
| <span data-ttu-id="bc739-206">*CreateRandomUPNUserName*</span><span class="sxs-lookup"><span data-stu-id="bc739-206">*CreateRandomUPNUserName*</span></span> | |
| <span data-ttu-id="bc739-207">*CreateUserPrincipalName*</span><span class="sxs-lookup"><span data-stu-id="bc739-207">*CreateUserPrincipalName*</span></span> | |
| <span data-ttu-id="bc739-208">*CreateSubjectClaimFromObjectID*</span><span class="sxs-lookup"><span data-stu-id="bc739-208">*CreateSubjectClaimFromObjectID*</span></span> | |
| <span data-ttu-id="bc739-209">*CreateSubjectClaimFromAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="bc739-209">*CreateSubjectClaimFromAlternativeSecurityId*</span></span> | |
| <span data-ttu-id="bc739-210">*CreateAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="bc739-210">*CreateAlternativeSecurityId*</span></span> | |

## <a name="content-definitions"></a><span data-ttu-id="bc739-211">Definice obsahu</span><span class="sxs-lookup"><span data-stu-id="bc739-211">Content definitions</span></span>

<span data-ttu-id="bc739-212">Tato část popisuje hello obsahu definice již byl deklarován v hello *B2C_1A_base* zásad.</span><span class="sxs-lookup"><span data-stu-id="bc739-212">This section describes hello content definitions already declared in hello *B2C_1A_base* policy.</span></span> <span data-ttu-id="bc739-213">Tyto definice obsahu jsou náchylné toobe odkazovat, přepsat nebo rozšířené podle potřeby v svoje vlastní zásady také jako hello *B2C_1A_base_extensions* zásad.</span><span class="sxs-lookup"><span data-stu-id="bc739-213">These content definitions are susceptible toobe referenced, overridden, and/or extended as needed in your own policies as well as in hello *B2C_1A_base_extensions* policy.</span></span>

| <span data-ttu-id="bc739-214">Poskytovatele deklarací identity</span><span class="sxs-lookup"><span data-stu-id="bc739-214">Claims provider</span></span> | <span data-ttu-id="bc739-215">Popis</span><span class="sxs-lookup"><span data-stu-id="bc739-215">Description</span></span> |
|-----------------|-------------|
| <span data-ttu-id="bc739-216">*Facebook*</span><span class="sxs-lookup"><span data-stu-id="bc739-216">*Facebook*</span></span> | |
| <span data-ttu-id="bc739-217">*Místní účet přihlášení*</span><span class="sxs-lookup"><span data-stu-id="bc739-217">*Local Account SignIn*</span></span> | |
| <span data-ttu-id="bc739-218">*PhoneFactor*</span><span class="sxs-lookup"><span data-stu-id="bc739-218">*PhoneFactor*</span></span> | |
| <span data-ttu-id="bc739-219">*Azure Active Directory*</span><span class="sxs-lookup"><span data-stu-id="bc739-219">*Azure Active Directory*</span></span> | |
| <span data-ttu-id="bc739-220">*Vlastní prohlašovanou*</span><span class="sxs-lookup"><span data-stu-id="bc739-220">*Self Asserted*</span></span> | |
| <span data-ttu-id="bc739-221">*Místní účet*</span><span class="sxs-lookup"><span data-stu-id="bc739-221">*Local Account*</span></span> | |
| <span data-ttu-id="bc739-222">*Správa relací*</span><span class="sxs-lookup"><span data-stu-id="bc739-222">*Session Management*</span></span> | |
| <span data-ttu-id="bc739-223">*Modul zásad Trustframework*</span><span class="sxs-lookup"><span data-stu-id="bc739-223">*Trustframework Policy Engine*</span></span> | |
| <span data-ttu-id="bc739-224">*TechnicalProfiles*</span><span class="sxs-lookup"><span data-stu-id="bc739-224">*TechnicalProfiles*</span></span> | |
| <span data-ttu-id="bc739-225">*Vydavatel tokenu*</span><span class="sxs-lookup"><span data-stu-id="bc739-225">*Token Issuer*</span></span> | |

## <a name="technical-profiles"></a><span data-ttu-id="bc739-226">Technické profily</span><span class="sxs-lookup"><span data-stu-id="bc739-226">Technical profiles</span></span>

<span data-ttu-id="bc739-227">Tato část znázorňuje hello technické profily už deklarovaný za poskytovatele deklarací identity v hello *B2C_1A_base* zásad.</span><span class="sxs-lookup"><span data-stu-id="bc739-227">This section depicts hello technical profiles already declared per claim provider in hello *B2C_1A_base* policy.</span></span> <span data-ttu-id="bc739-228">Tyto technické profily jsou náchylné toobe další odkazuje, přepsat nebo rozšířené podle potřeby v svoje vlastní zásady také jako hello *B2C_1A_base_extensions* zásad.</span><span class="sxs-lookup"><span data-stu-id="bc739-228">These technical profiles are susceptible toobe further referenced, overridden, and/or extended as needed in your own policies as well as in hello *B2C_1A_base_extensions* policy.</span></span>

### <a name="technical-profiles-for-facebook"></a><span data-ttu-id="bc739-229">Technické profily pro Facebook</span><span class="sxs-lookup"><span data-stu-id="bc739-229">Technical profiles for Facebook</span></span>

| <span data-ttu-id="bc739-230">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="bc739-230">Technical profile</span></span> | <span data-ttu-id="bc739-231">Popis</span><span class="sxs-lookup"><span data-stu-id="bc739-231">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="bc739-232">*OAUTH pro Facebook*</span><span class="sxs-lookup"><span data-stu-id="bc739-232">*Facebook-OAUTH*</span></span> | |

### <a name="technical-profiles-for-local-account-signin"></a><span data-ttu-id="bc739-233">Technické profily pro místní přihlášení účtu</span><span class="sxs-lookup"><span data-stu-id="bc739-233">Technical profiles for Local Account Signin</span></span>

| <span data-ttu-id="bc739-234">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="bc739-234">Technical profile</span></span> | <span data-ttu-id="bc739-235">Popis</span><span class="sxs-lookup"><span data-stu-id="bc739-235">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="bc739-236">*Neinteraktivní přihlášení*</span><span class="sxs-lookup"><span data-stu-id="bc739-236">*Login-NonInteractive*</span></span> | |

### <a name="technical-profiles-for-phone-factor"></a><span data-ttu-id="bc739-237">Technické profily pro Phone Factor</span><span class="sxs-lookup"><span data-stu-id="bc739-237">Technical profiles for Phone Factor</span></span>

| <span data-ttu-id="bc739-238">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="bc739-238">Technical profile</span></span> | <span data-ttu-id="bc739-239">Popis</span><span class="sxs-lookup"><span data-stu-id="bc739-239">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="bc739-240">*Vstup PhoneFactor*</span><span class="sxs-lookup"><span data-stu-id="bc739-240">*PhoneFactor-Input*</span></span> | |
| <span data-ttu-id="bc739-241">*PhoneFactor InputOrVerify*</span><span class="sxs-lookup"><span data-stu-id="bc739-241">*PhoneFactor-InputOrVerify*</span></span> | |
| <span data-ttu-id="bc739-242">*Ověřte PhoneFactor*</span><span class="sxs-lookup"><span data-stu-id="bc739-242">*PhoneFactor-Verify*</span></span> | |

### <a name="technical-profiles-for-azure-active-directory"></a><span data-ttu-id="bc739-243">Technické profily pro Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bc739-243">Technical profiles for Azure Active Directory</span></span>

| <span data-ttu-id="bc739-244">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="bc739-244">Technical profile</span></span> | <span data-ttu-id="bc739-245">Popis</span><span class="sxs-lookup"><span data-stu-id="bc739-245">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="bc739-246">*Běžné AAD*</span><span class="sxs-lookup"><span data-stu-id="bc739-246">*AAD-Common*</span></span> | <span data-ttu-id="bc739-247">Technické profil podle hello jiných profilů technické AAD xxx</span><span class="sxs-lookup"><span data-stu-id="bc739-247">Technical profile included by hello other AAD-xxx technical profiles</span></span> |
| <span data-ttu-id="bc739-248">*AAD UserWriteUsingAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="bc739-248">*AAD-UserWriteUsingAlternativeSecurityId*</span></span> | <span data-ttu-id="bc739-249">Technické profil pro sociálních přihlášení</span><span class="sxs-lookup"><span data-stu-id="bc739-249">Technical profile for social logins</span></span> |
| <span data-ttu-id="bc739-250">*AAD UserReadUsingAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="bc739-250">*AAD-UserReadUsingAlternativeSecurityId*</span></span> | <span data-ttu-id="bc739-251">Technické profil pro sociálních přihlášení</span><span class="sxs-lookup"><span data-stu-id="bc739-251">Technical profile for social logins</span></span> |
| <span data-ttu-id="bc739-252">*AAD. UserReadUsingAlternativeSecurityId NoError*</span><span class="sxs-lookup"><span data-stu-id="bc739-252">*AAD-UserReadUsingAlternativeSecurityId-NoError*</span></span> | <span data-ttu-id="bc739-253">Technické profil pro sociálních přihlášení</span><span class="sxs-lookup"><span data-stu-id="bc739-253">Technical profile for social logins</span></span> |
| <span data-ttu-id="bc739-254">*AAD UserWritePasswordUsingLogonEmail*</span><span class="sxs-lookup"><span data-stu-id="bc739-254">*AAD-UserWritePasswordUsingLogonEmail*</span></span> | <span data-ttu-id="bc739-255">Technické profil pro místní účty</span><span class="sxs-lookup"><span data-stu-id="bc739-255">Technical profile for local accounts</span></span> |
| <span data-ttu-id="bc739-256">*AAD UserReadUsingEmailAddress*</span><span class="sxs-lookup"><span data-stu-id="bc739-256">*AAD-UserReadUsingEmailAddress*</span></span> | <span data-ttu-id="bc739-257">Technické profil pro místní účty</span><span class="sxs-lookup"><span data-stu-id="bc739-257">Technical profile for local accounts</span></span> |
| <span data-ttu-id="bc739-258">*AAD UserWriteProfileUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="bc739-258">*AAD-UserWriteProfileUsingObjectId*</span></span> | <span data-ttu-id="bc739-259">Aktualizace záznamu uživatele pomocí objectId technické profilu</span><span class="sxs-lookup"><span data-stu-id="bc739-259">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="bc739-260">*AAD UserWritePhoneNumberUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="bc739-260">*AAD-UserWritePhoneNumberUsingObjectId*</span></span> | <span data-ttu-id="bc739-261">Aktualizace záznamu uživatele pomocí objectId technické profilu</span><span class="sxs-lookup"><span data-stu-id="bc739-261">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="bc739-262">*AAD UserWritePasswordUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="bc739-262">*AAD-UserWritePasswordUsingObjectId*</span></span> | <span data-ttu-id="bc739-263">Aktualizace záznamu uživatele pomocí objectId technické profilu</span><span class="sxs-lookup"><span data-stu-id="bc739-263">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="bc739-264">*AAD UserReadUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="bc739-264">*AAD-UserReadUsingObjectId*</span></span> | <span data-ttu-id="bc739-265">Technické profil je použité tooread dat po ověření uživatele</span><span class="sxs-lookup"><span data-stu-id="bc739-265">Technical profile is used tooread data after user authenticates</span></span> |

### <a name="technical-profiles-for-self-asserted"></a><span data-ttu-id="bc739-266">Technické profily pro samoobslužné prohlašovanou</span><span class="sxs-lookup"><span data-stu-id="bc739-266">Technical profiles for Self Asserted</span></span>

| <span data-ttu-id="bc739-267">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="bc739-267">Technical profile</span></span> | <span data-ttu-id="bc739-268">Popis</span><span class="sxs-lookup"><span data-stu-id="bc739-268">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="bc739-269">*Sociální SelfAsserted*</span><span class="sxs-lookup"><span data-stu-id="bc739-269">*SelfAsserted-Social*</span></span> | |
| <span data-ttu-id="bc739-270">*SelfAsserted ProfileUpdate*</span><span class="sxs-lookup"><span data-stu-id="bc739-270">*SelfAsserted-ProfileUpdate*</span></span> | |

### <a name="technical-profiles-for-local-account"></a><span data-ttu-id="bc739-271">Technické profily pro místní účet</span><span class="sxs-lookup"><span data-stu-id="bc739-271">Technical profiles for Local Account</span></span>

| <span data-ttu-id="bc739-272">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="bc739-272">Technical profile</span></span> | <span data-ttu-id="bc739-273">Popis</span><span class="sxs-lookup"><span data-stu-id="bc739-273">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="bc739-274">*LocalAccountSignUpWithLogonEmail*</span><span class="sxs-lookup"><span data-stu-id="bc739-274">*LocalAccountSignUpWithLogonEmail*</span></span> | |

### <a name="technical-profiles-for-session-management"></a><span data-ttu-id="bc739-275">Technické profilů pro správu relací</span><span class="sxs-lookup"><span data-stu-id="bc739-275">Technical profiles for Session Management</span></span>

| <span data-ttu-id="bc739-276">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="bc739-276">Technical profile</span></span> | <span data-ttu-id="bc739-277">Popis</span><span class="sxs-lookup"><span data-stu-id="bc739-277">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="bc739-278">*SM-nedojde k žádné akci*</span><span class="sxs-lookup"><span data-stu-id="bc739-278">*SM-Noop*</span></span> | |
| <span data-ttu-id="bc739-279">*SM AAD*</span><span class="sxs-lookup"><span data-stu-id="bc739-279">*SM-AAD*</span></span> | |
| <span data-ttu-id="bc739-280">*SM SocialSignup*</span><span class="sxs-lookup"><span data-stu-id="bc739-280">*SM-SocialSignup*</span></span> | <span data-ttu-id="bc739-281">Název profilu je se použité toodisambiguate AAD relace mezi Přihlaste se a přihlaste se</span><span class="sxs-lookup"><span data-stu-id="bc739-281">Profile name is being used toodisambiguate AAD session between sign up and sign in</span></span> |
| <span data-ttu-id="bc739-282">*SM SocialLogin*</span><span class="sxs-lookup"><span data-stu-id="bc739-282">*SM-SocialLogin*</span></span> | |
| <span data-ttu-id="bc739-283">*SM MFA*</span><span class="sxs-lookup"><span data-stu-id="bc739-283">*SM-MFA*</span></span> | |

### <a name="technical-profiles-for-trustframework-policy-engine-technicalprofiles"></a><span data-ttu-id="bc739-284">Technické profily pro TechnicalProfiles modul zásad Trustframework</span><span class="sxs-lookup"><span data-stu-id="bc739-284">Technical profiles for Trustframework Policy Engine TechnicalProfiles</span></span>

<span data-ttu-id="bc739-285">V současné době jsou definovány žádné technické profily pro hello **TechnicalProfiles modul zásad Trustframework** poskytovatele deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="bc739-285">Currently, no technical profiles are defined for hello **Trustframework Policy Engine TechnicalProfiles** claims provider.</span></span>

### <a name="technical-profiles-for-token-issuer"></a><span data-ttu-id="bc739-286">Technické profily pro vydavatel tokenu</span><span class="sxs-lookup"><span data-stu-id="bc739-286">Technical profiles for Token Issuer</span></span>

| <span data-ttu-id="bc739-287">Technické profilu</span><span class="sxs-lookup"><span data-stu-id="bc739-287">Technical profile</span></span> | <span data-ttu-id="bc739-288">Popis</span><span class="sxs-lookup"><span data-stu-id="bc739-288">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="bc739-289">*JwtIssuer*</span><span class="sxs-lookup"><span data-stu-id="bc739-289">*JwtIssuer*</span></span> | |

## <a name="user-journeys"></a><span data-ttu-id="bc739-290">Uživatel cesty</span><span class="sxs-lookup"><span data-stu-id="bc739-290">User journeys</span></span>

<span data-ttu-id="bc739-291">Tato část znázorňuje cesty uživatele hello již byl deklarován v hello *B2C_1A_base* zásad.</span><span class="sxs-lookup"><span data-stu-id="bc739-291">This section depicts hello user journeys already declared in hello *B2C_1A_base* policy.</span></span> <span data-ttu-id="bc739-292">Tyto cesty uživatele jsou náchylné toobe další odkazuje, přepsat nebo rozšířené podle potřeby v svoje vlastní zásady také jako hello *B2C_1A_base_extensions* zásad.</span><span class="sxs-lookup"><span data-stu-id="bc739-292">These user journeys are susceptible toobe further referenced, overridden, and/or extended as needed in your own policies as well as in hello *B2C_1A_base_extensions* policy.</span></span>

| <span data-ttu-id="bc739-293">Uživatel cesty</span><span class="sxs-lookup"><span data-stu-id="bc739-293">User journey</span></span> | <span data-ttu-id="bc739-294">Popis</span><span class="sxs-lookup"><span data-stu-id="bc739-294">Description</span></span> |
|--------------|-------------|
| <span data-ttu-id="bc739-295">*Registrace*</span><span class="sxs-lookup"><span data-stu-id="bc739-295">*SignUp*</span></span> | |
| <span data-ttu-id="bc739-296">*Přihlášení*</span><span class="sxs-lookup"><span data-stu-id="bc739-296">*SignIn*</span></span> | |
| <span data-ttu-id="bc739-297">*SignUpOrSignIn*</span><span class="sxs-lookup"><span data-stu-id="bc739-297">*SignUpOrSignIn*</span></span> | |
| <span data-ttu-id="bc739-298">*Úprava profilu*</span><span class="sxs-lookup"><span data-stu-id="bc739-298">*EditProfile*</span></span> | |
| <span data-ttu-id="bc739-299">*PasswordReset*</span><span class="sxs-lookup"><span data-stu-id="bc739-299">*PasswordReset*</span></span> | |
