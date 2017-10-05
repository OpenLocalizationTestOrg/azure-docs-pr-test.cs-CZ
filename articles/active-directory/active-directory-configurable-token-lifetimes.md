---
title: "Konfigurovat životnosti tokenu v Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak nastavit dobu platnosti pro tokeny vydané službou Azure AD."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 06f5b317-053e-44c3-aaaa-cf07d8692735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: billmath
ms.custom: aaddev
ms.reviewer: anchitn
ms.openlocfilehash: d23721eba308096a05211eb6e26e1338a69cae0c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="configurable-token-lifetimes-in-azure-active-directory-public-preview"></a><span data-ttu-id="235a5-103">Konfigurovat životnosti tokenu v Azure Active Directory (Public Preview)</span><span class="sxs-lookup"><span data-stu-id="235a5-103">Configurable token lifetimes in Azure Active Directory (Public Preview)</span></span>
<span data-ttu-id="235a5-104">Můžete zadat dobu životnosti tokenem vydaným službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="235a5-104">You can specify the lifetime of a token issued by Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="235a5-105">Můžete nastavit životnosti tokenu pro všechny aplikace ve vaší organizaci, pro aplikaci víceklientské (více organizace) nebo pro objekt určité služby ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-105">You can set token lifetimes for all apps in your organization, for a multi-tenant (multi-organization) application, or for a specific service principal in your organization.</span></span>

> [!NOTE]
> <span data-ttu-id="235a5-106">Tato funkce je aktuálně ve verzi Public Preview.</span><span class="sxs-lookup"><span data-stu-id="235a5-106">This capability currently is in Public Preview.</span></span> <span data-ttu-id="235a5-107">Připravte se na obnovit nebo odeberte všechny změny.</span><span class="sxs-lookup"><span data-stu-id="235a5-107">Be prepared to revert or remove any changes.</span></span> <span data-ttu-id="235a5-108">Tato funkce je k dispozici v libovolné předplatné služby Azure Active Directory během verzi Public Preview.</span><span class="sxs-lookup"><span data-stu-id="235a5-108">The feature is available in any Azure Active Directory subscription during Public Preview.</span></span> <span data-ttu-id="235a5-109">Ale když je tato funkce obecně dostupná, některé aspekty funkce může vyžadovat [Azure Active Directory Premium](active-directory-get-started-premium.md) předplatné.</span><span class="sxs-lookup"><span data-stu-id="235a5-109">However, when the feature becomes generally available, some aspects of the feature might require an [Azure Active Directory Premium](active-directory-get-started-premium.md) subscription.</span></span>
>
>

<span data-ttu-id="235a5-110">Objekt zásady ve službě Azure AD, představuje sadu pravidel, které vynucuje u jednotlivých aplikací, nebo na všechny aplikace v organizaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-110">In Azure AD, a policy object represents a set of rules that are enforced on individual applications or on all applications in an organization.</span></span> <span data-ttu-id="235a5-111">Každý typ zásad se strukturou jedinečnou sadu vlastností, které se použijí pro objekty, na které jsou přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="235a5-111">Each policy type has a unique structure, with a set of properties that are applied to objects to which they are assigned.</span></span>

<span data-ttu-id="235a5-112">Zásady můžete určit jako výchozí zásady pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-112">You can designate a policy as the default policy for your organization.</span></span> <span data-ttu-id="235a5-113">Zásady se použijí pro všechny aplikace v organizaci, tak dlouho, dokud není přepsána zásady s vyšší prioritou.</span><span class="sxs-lookup"><span data-stu-id="235a5-113">The policy is applied to any application in the organization, as long as it is not overridden by a policy with a higher priority.</span></span> <span data-ttu-id="235a5-114">Také můžete přiřadit zásady pro konkrétní aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a5-114">You also can assign a policy to specific applications.</span></span> <span data-ttu-id="235a5-115">Pořadí podle priority se liší podle typu zásady.</span><span class="sxs-lookup"><span data-stu-id="235a5-115">The order of priority varies by policy type.</span></span>


## <a name="token-types"></a><span data-ttu-id="235a5-116">Typy tokenů</span><span class="sxs-lookup"><span data-stu-id="235a5-116">Token types</span></span>

<span data-ttu-id="235a5-117">Můžete nastavit dobu životnosti tokenu zásady pro tokeny obnovení, tokeny přístupu, tokeny relace a tokeny typu ID.</span><span class="sxs-lookup"><span data-stu-id="235a5-117">You can set token lifetime policies for refresh tokens, access tokens, session tokens, and ID tokens.</span></span>

### <a name="access-tokens"></a><span data-ttu-id="235a5-118">Přístupové tokeny</span><span class="sxs-lookup"><span data-stu-id="235a5-118">Access tokens</span></span>
<span data-ttu-id="235a5-119">Klienti používat pro přístup k chráněnému prostředku přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="235a5-119">Clients use access tokens to access a protected resource.</span></span> <span data-ttu-id="235a5-120">Přístupový token lze použít pouze pro konkrétní kombinaci uživatele, klienta a prostředků.</span><span class="sxs-lookup"><span data-stu-id="235a5-120">An access token can be used only for a specific combination of user, client, and resource.</span></span> <span data-ttu-id="235a5-121">Přístupové tokeny nejde odvolat a jsou platné až do vypršení jejich platnosti.</span><span class="sxs-lookup"><span data-stu-id="235a5-121">Access tokens cannot be revoked and are valid until their expiry.</span></span> <span data-ttu-id="235a5-122">Škodlivý objektu actor, který má získat token přístupu můžete použít pro rozsah celé jeho životnosti.</span><span class="sxs-lookup"><span data-stu-id="235a5-122">A malicious actor that has obtained an access token can use it for extent of its lifetime.</span></span> <span data-ttu-id="235a5-123">Úprava životnost přístupový token je kompromis mezi zlepšení výkonu systému a zvýšení množství času, klient zachová přístup po uživatelský účet je zakázán.</span><span class="sxs-lookup"><span data-stu-id="235a5-123">Adjusting the lifetime of an access token is a trade-off between improving system performance and increasing the amount of time that the client retains access after the user’s account is disabled.</span></span> <span data-ttu-id="235a5-124">Vylepšené systému výkonu se dosáhne snižuje počet, kolikrát klient potřebuje získat nový přístupový token.</span><span class="sxs-lookup"><span data-stu-id="235a5-124">Improved system performance is achieved by reducing the number of times a client needs to acquire a fresh access token.</span></span>

### <a name="refresh-tokens"></a><span data-ttu-id="235a5-125">Obnovovacích tokenů</span><span class="sxs-lookup"><span data-stu-id="235a5-125">Refresh tokens</span></span>
<span data-ttu-id="235a5-126">Když klient získá přístupový token pro přístup k chráněnému prostředku, klient přijme token obnovení a přístupový token.</span><span class="sxs-lookup"><span data-stu-id="235a5-126">When a client acquires an access token to access a protected resource, the client receives both a refresh token and an access token.</span></span> <span data-ttu-id="235a5-127">Token obnovení slouží k získání nového přístupu nebo aktualizace tokenu páry když vyprší platnost aktuální přístupový token.</span><span class="sxs-lookup"><span data-stu-id="235a5-127">The refresh token is used to obtain new access/refresh token pairs when the current access token expires.</span></span> <span data-ttu-id="235a5-128">Token obnovení je vázán na kombinaci uživatele a klienta.</span><span class="sxs-lookup"><span data-stu-id="235a5-128">A refresh token is bound to a combination of user and client.</span></span> <span data-ttu-id="235a5-129">Obnovovací token se dají odvolávat a platnosti tokenu je zaškrtnuta možnost pokaždé, když se používá token.</span><span class="sxs-lookup"><span data-stu-id="235a5-129">A refresh token can be revoked, and the token's validity is checked every time the token is used.</span></span>

<span data-ttu-id="235a5-130">Je důležité rozlišovat mezi důvěrné klienty a veřejné.</span><span class="sxs-lookup"><span data-stu-id="235a5-130">It's important to make a distinction between confidential clients and public clients.</span></span> <span data-ttu-id="235a5-131">Další informace o různých typech klientů najdete v tématu [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).</span><span class="sxs-lookup"><span data-stu-id="235a5-131">For more information about different types of clients, see [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).</span></span>

#### <a name="token-lifetimes-with-confidential-client-refresh-tokens"></a><span data-ttu-id="235a5-132">Token životnosti s tokeny obnovení důvěrné klienta</span><span class="sxs-lookup"><span data-stu-id="235a5-132">Token lifetimes with confidential client refresh tokens</span></span>
<span data-ttu-id="235a5-133">Důvěrné klienti jsou aplikace, které můžete bezpečně uložit heslo klienta (tajný klíč).</span><span class="sxs-lookup"><span data-stu-id="235a5-133">Confidential clients are applications that can securely store a client password (secret).</span></span> <span data-ttu-id="235a5-134">Mohou prokázat, že požadavky přicházejí z klientské aplikace a nikoli z škodlivý objektu actor.</span><span class="sxs-lookup"><span data-stu-id="235a5-134">They can prove that requests are coming from the client application and not from a malicious actor.</span></span> <span data-ttu-id="235a5-135">Například webová aplikace je důvěrné klienta, protože tajný klíč klienta může ukládat na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="235a5-135">For example, a web app is a confidential client because it can store a client secret on the web server.</span></span> <span data-ttu-id="235a5-136">Nevystavené.</span><span class="sxs-lookup"><span data-stu-id="235a5-136">It is not exposed.</span></span> <span data-ttu-id="235a5-137">Protože tyto toky jsou bezpečnější, je výchozí doby platnosti tokenů aktualizace vydané tyto toky `until-revoked`, nelze změnit pomocí zásad a nebude na resetování hesla dobrovolná odvolán.</span><span class="sxs-lookup"><span data-stu-id="235a5-137">Because these flows are more secure, the default lifetimes of refresh tokens issued to these flows is `until-revoked`, cannot be changed by using policy, and will not be revoked on voluntary password resets.</span></span>

#### <a name="token-lifetimes-with-public-client-refresh-tokens"></a><span data-ttu-id="235a5-138">Token životnosti s tokeny obnovení veřejné klienta</span><span class="sxs-lookup"><span data-stu-id="235a5-138">Token lifetimes with public client refresh tokens</span></span>

<span data-ttu-id="235a5-139">Veřejné klientů nelze bezpečně uložit heslo klienta (tajný klíč).</span><span class="sxs-lookup"><span data-stu-id="235a5-139">Public clients cannot securely store a client password (secret).</span></span> <span data-ttu-id="235a5-140">Aplikace pro iOS nebo Android nelze například obfuskováním tajný klíč z vlastníka prostředku, bude považován za veřejné klienta.</span><span class="sxs-lookup"><span data-stu-id="235a5-140">For example, an iOS/Android app cannot obfuscate a secret from the resource owner, so it is considered a public client.</span></span> <span data-ttu-id="235a5-141">Nastavení zásad pro prostředky zabrání získat nový pár tokenu přístupu nebo aktualizace tokeny obnovení z veřejné klientů starší než během zadaného období.</span><span class="sxs-lookup"><span data-stu-id="235a5-141">You can set policies on resources to prevent refresh tokens from public clients older than a specified period from obtaining a new access/refresh token pair.</span></span> <span data-ttu-id="235a5-142">(K tomu použijte vlastnost aktualizovat Token maximální neaktivní doba.) Zásady můžete používat i nastavit dobu, jejichž překročení se už přijata obnovovacích tokenů.</span><span class="sxs-lookup"><span data-stu-id="235a5-142">(To do this, use the Refresh Token Max Inactive Time property.) You also can use policies to set a period beyond which the refresh tokens are no longer accepted.</span></span> <span data-ttu-id="235a5-143">(K tomu použijte vlastnost aktualizovat Token maximální stáří.) Můžete upravit dobu životnosti tokenu obnovení řídit, kdy a jak často je potřeba zadat přihlašovací údaje, místo se bezobslužně k novému ověření, při použití veřejných klientskou aplikaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="235a5-143">(To do this, use the Refresh Token Max Age property.) You can adjust the lifetime of a refresh token to control when and how often the user is required to reenter credentials, instead of being silently reauthenticated, when using a public client application.</span></span>

### <a name="id-tokens"></a><span data-ttu-id="235a5-144">ID tokeny</span><span class="sxs-lookup"><span data-stu-id="235a5-144">ID tokens</span></span>
<span data-ttu-id="235a5-145">ID tokeny jsou předány weby a nativních klientů.</span><span class="sxs-lookup"><span data-stu-id="235a5-145">ID tokens are passed to websites and native clients.</span></span> <span data-ttu-id="235a5-146">ID tokeny obsahovat profil informací o uživateli.</span><span class="sxs-lookup"><span data-stu-id="235a5-146">ID tokens contain profile information about a user.</span></span> <span data-ttu-id="235a5-147">ID token je vázána na konkrétní kombinaci uživatele a klienta.</span><span class="sxs-lookup"><span data-stu-id="235a5-147">An ID token is bound to a specific combination of user and client.</span></span> <span data-ttu-id="235a5-148">ID tokeny považovány za platný až do vypršení jejich platnosti.</span><span class="sxs-lookup"><span data-stu-id="235a5-148">ID tokens are considered valid until their expiry.</span></span> <span data-ttu-id="235a5-149">Obvykle aplikace webového odpovídá uživatele je vydán dobu platnosti relace v aplikaci na dobu životnosti tokenu ID pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="235a5-149">Usually, a web application matches a user’s session lifetime in the application to the lifetime of the ID token issued for the user.</span></span> <span data-ttu-id="235a5-150">Můžete upravit dobu životnosti tokenu ID řídit, jak často webové aplikace skončí relace aplikace a jak často se vyžaduje, aby uživatel nové ověření vyžadováno s Azure AD (bezobslužná nebo interaktivní).</span><span class="sxs-lookup"><span data-stu-id="235a5-150">You can adjust the lifetime of an ID token to control how often the web application expires the application session, and how often it requires the user to be reauthenticated with Azure AD (either silently or interactively).</span></span>

### <a name="single-sign-on-session-tokens"></a><span data-ttu-id="235a5-151">Tokeny jedné relace přihlášení</span><span class="sxs-lookup"><span data-stu-id="235a5-151">Single sign-on session tokens</span></span>
<span data-ttu-id="235a5-152">Když uživatel ověřuje s Azure AD a vybere **zůstat přihlášeni** políčko relaci přihlašování (SSO) se naváže prohlížeče uživatele a Azure AD.</span><span class="sxs-lookup"><span data-stu-id="235a5-152">When a user authenticates with Azure AD and selects the **Keep me signed in** check box, a single sign-on session (SSO) is established with the user’s browser and Azure AD.</span></span> <span data-ttu-id="235a5-153">Token jednotného přihlašování, ve formátu souboru cookie, představuje tuto relaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-153">The SSO token, in the form of a cookie, represents this session.</span></span> <span data-ttu-id="235a5-154">Všimněte si, že token relace jednotného přihlašování není vázána na konkrétní prostředek nebo klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-154">Note that the SSO session token is not bound to a specific resource/client application.</span></span> <span data-ttu-id="235a5-155">Tokeny relace jednotného přihlašování se dají odvolávat a jejich platnost kontroluje pokaždé, když se používají.</span><span class="sxs-lookup"><span data-stu-id="235a5-155">SSO session tokens can be revoked, and their validity is checked every time they are used.</span></span>

<span data-ttu-id="235a5-156">Azure AD používá dva typy tokenů relace jednotného přihlašování: trvalé a zajišťováno.</span><span class="sxs-lookup"><span data-stu-id="235a5-156">Azure AD uses two kinds of SSO session tokens: persistent and nonpersistent.</span></span> <span data-ttu-id="235a5-157">Trvalé relace tokeny jsou uloženy jako trvalé soubory cookie v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="235a5-157">Persistent session tokens are stored as persistent cookies by the browser.</span></span> <span data-ttu-id="235a5-158">Tokeny zajišťováno relace jsou uloženy jako soubory cookie relace.</span><span class="sxs-lookup"><span data-stu-id="235a5-158">Nonpersistent session tokens are stored as session cookies.</span></span> <span data-ttu-id="235a5-159">(Souborů cookie relací jsou zničen při zavření prohlížeče.)</span><span class="sxs-lookup"><span data-stu-id="235a5-159">(Session cookies are destroyed when the browser is closed.)</span></span>

<span data-ttu-id="235a5-160">Zajišťováno relace tokeny mají životnost 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="235a5-160">Nonpersistent session tokens have a lifetime of 24 hours.</span></span> <span data-ttu-id="235a5-161">Trvalé tokeny mají životnost 180 dní.</span><span class="sxs-lookup"><span data-stu-id="235a5-161">Persistent tokens have a lifetime of 180 days.</span></span> <span data-ttu-id="235a5-162">Token relace jednotného přihlašování se používá v rozsahu období platnosti, když je platnosti prodloužit jiný 24 hodin nebo 180 dnů, v závislosti na typ tokenu.</span><span class="sxs-lookup"><span data-stu-id="235a5-162">Any time an SSO session token is used within its validity period, the validity period is extended another 24 hours or 180 days, depending on the token type.</span></span> <span data-ttu-id="235a5-163">Pokud token relace jednotného přihlašování se nepoužívá v rozsahu období platnosti, bude považován za platnost a již byla přijata.</span><span class="sxs-lookup"><span data-stu-id="235a5-163">If an SSO session token is not used within its validity period, it is considered expired and is no longer accepted.</span></span>

<span data-ttu-id="235a5-164">Zásady můžete nastavit dobu, po první relaci token vydán nad kterým je token relace již povolena.</span><span class="sxs-lookup"><span data-stu-id="235a5-164">You can use a policy to set the time after the first session token was issued beyond which the session token is no longer accepted.</span></span> <span data-ttu-id="235a5-165">(K tomu použijte vlastnost relace tokenu maximální stáří.) Můžete upravit dobu životnosti tokenu relace řídit, kdy a jak často je uživatel muset zadat přihlašovací údaje, místo bezobslužně ověřovaného, pokud používáte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-165">(To do this, use the Session Token Max Age property.) You can adjust the lifetime of a session token to control when and how often a user is required to reenter credentials, instead of being silently authenticated, when using a web application.</span></span>

### <a name="token-lifetime-policy-properties"></a><span data-ttu-id="235a5-166">Vlastnosti zásad životnost tokenu</span><span class="sxs-lookup"><span data-stu-id="235a5-166">Token lifetime policy properties</span></span>
<span data-ttu-id="235a5-167">Životnost tokenu zásad je typ objektu zásad, který obsahuje pravidla životnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="235a5-167">A token lifetime policy is a type of policy object that contains token lifetime rules.</span></span> <span data-ttu-id="235a5-168">Pomocí vlastností zásad pro řízení zadaný token životnosti.</span><span class="sxs-lookup"><span data-stu-id="235a5-168">Use the properties of the policy to control specified token lifetimes.</span></span> <span data-ttu-id="235a5-169">Pokud je nastavené žádné zásady, systém vynucuje výchozí hodnota doby platnosti.</span><span class="sxs-lookup"><span data-stu-id="235a5-169">If no policy is set, the system enforces the default lifetime value.</span></span>

### <a name="configurable-token-lifetime-properties"></a><span data-ttu-id="235a5-170">Vlastnosti konfigurovat životnost tokenu</span><span class="sxs-lookup"><span data-stu-id="235a5-170">Configurable token lifetime properties</span></span>
| <span data-ttu-id="235a5-171">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="235a5-171">Property</span></span> | <span data-ttu-id="235a5-172">Řetězec vlastnosti zásad</span><span class="sxs-lookup"><span data-stu-id="235a5-172">Policy property string</span></span> | <span data-ttu-id="235a5-173">Ovlivňuje</span><span class="sxs-lookup"><span data-stu-id="235a5-173">Affects</span></span> | <span data-ttu-id="235a5-174">Výchozí</span><span class="sxs-lookup"><span data-stu-id="235a5-174">Default</span></span> | <span data-ttu-id="235a5-175">Minimální</span><span class="sxs-lookup"><span data-stu-id="235a5-175">Minimum</span></span> | <span data-ttu-id="235a5-176">Maximální počet</span><span class="sxs-lookup"><span data-stu-id="235a5-176">Maximum</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="235a5-177">Životnost tokenu přístupu</span><span class="sxs-lookup"><span data-stu-id="235a5-177">Access Token Lifetime</span></span> |<span data-ttu-id="235a5-178">AccessTokenLifetime</span><span class="sxs-lookup"><span data-stu-id="235a5-178">AccessTokenLifetime</span></span> |<span data-ttu-id="235a5-179">Přístupové tokeny, tokeny typu ID, typu SAML2 tokeny</span><span class="sxs-lookup"><span data-stu-id="235a5-179">Access tokens, ID tokens, SAML2 tokens</span></span> |<span data-ttu-id="235a5-180">1 hodina</span><span class="sxs-lookup"><span data-stu-id="235a5-180">1 hour</span></span> |<span data-ttu-id="235a5-181">10 minut</span><span class="sxs-lookup"><span data-stu-id="235a5-181">10 minutes</span></span> |<span data-ttu-id="235a5-182">1 den</span><span class="sxs-lookup"><span data-stu-id="235a5-182">1 day</span></span> |
| <span data-ttu-id="235a5-183">Aktualizace tokenu maximální doba neaktivní</span><span class="sxs-lookup"><span data-stu-id="235a5-183">Refresh Token Max Inactive Time</span></span> |<span data-ttu-id="235a5-184">MaxInactiveTime</span><span class="sxs-lookup"><span data-stu-id="235a5-184">MaxInactiveTime</span></span> |<span data-ttu-id="235a5-185">Obnovovacích tokenů</span><span class="sxs-lookup"><span data-stu-id="235a5-185">Refresh tokens</span></span> |<span data-ttu-id="235a5-186">14 dnů</span><span class="sxs-lookup"><span data-stu-id="235a5-186">14 days</span></span> |<span data-ttu-id="235a5-187">10 minut</span><span class="sxs-lookup"><span data-stu-id="235a5-187">10 minutes</span></span> |<span data-ttu-id="235a5-188">90 dnů</span><span class="sxs-lookup"><span data-stu-id="235a5-188">90 days</span></span> |
| <span data-ttu-id="235a5-189">Single-Factor aktualizace tokenu maximální stáří</span><span class="sxs-lookup"><span data-stu-id="235a5-189">Single-Factor Refresh Token Max Age</span></span> |<span data-ttu-id="235a5-190">MaxAgeSingleFactor</span><span class="sxs-lookup"><span data-stu-id="235a5-190">MaxAgeSingleFactor</span></span> |<span data-ttu-id="235a5-191">Obnovovacích tokenů (pro všechny uživatele)</span><span class="sxs-lookup"><span data-stu-id="235a5-191">Refresh tokens (for any users)</span></span> |<span data-ttu-id="235a5-192">Dokud odvolat</span><span class="sxs-lookup"><span data-stu-id="235a5-192">Until-revoked</span></span> |<span data-ttu-id="235a5-193">10 minut</span><span class="sxs-lookup"><span data-stu-id="235a5-193">10 minutes</span></span> |<span data-ttu-id="235a5-194">Dokud odvolat<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="235a5-194">Until-revoked<sup>1</sup></span></span> |
| <span data-ttu-id="235a5-195">Maximální stáří tokenu Multi-Factor aktualizace</span><span class="sxs-lookup"><span data-stu-id="235a5-195">Multi-Factor Refresh Token Max Age</span></span> |<span data-ttu-id="235a5-196">MaxAgeMultiFactor</span><span class="sxs-lookup"><span data-stu-id="235a5-196">MaxAgeMultiFactor</span></span> |<span data-ttu-id="235a5-197">Obnovovacích tokenů (pro všechny uživatele)</span><span class="sxs-lookup"><span data-stu-id="235a5-197">Refresh tokens (for any users)</span></span> |<span data-ttu-id="235a5-198">Dokud odvolat</span><span class="sxs-lookup"><span data-stu-id="235a5-198">Until-revoked</span></span> |<span data-ttu-id="235a5-199">10 minut</span><span class="sxs-lookup"><span data-stu-id="235a5-199">10 minutes</span></span> |<span data-ttu-id="235a5-200">Dokud odvolat<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="235a5-200">Until-revoked<sup>1</sup></span></span> |
| <span data-ttu-id="235a5-201">Maximální stáří tokenu Single-Factor relace</span><span class="sxs-lookup"><span data-stu-id="235a5-201">Single-Factor Session Token Max Age</span></span> |<span data-ttu-id="235a5-202">MaxAgeSessionSingleFactor<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="235a5-202">MaxAgeSessionSingleFactor<sup>2</sup></span></span> |<span data-ttu-id="235a5-203">Tokeny relace (trvalá nebo zajišťováno)</span><span class="sxs-lookup"><span data-stu-id="235a5-203">Session tokens (persistent and nonpersistent)</span></span> |<span data-ttu-id="235a5-204">Dokud odvolat</span><span class="sxs-lookup"><span data-stu-id="235a5-204">Until-revoked</span></span> |<span data-ttu-id="235a5-205">10 minut</span><span class="sxs-lookup"><span data-stu-id="235a5-205">10 minutes</span></span> |<span data-ttu-id="235a5-206">Dokud odvolat<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="235a5-206">Until-revoked<sup>1</sup></span></span> |
| <span data-ttu-id="235a5-207">Maximální stáří tokenu Multi-Factor relace</span><span class="sxs-lookup"><span data-stu-id="235a5-207">Multi-Factor Session Token Max Age</span></span> |<span data-ttu-id="235a5-208">MaxAgeSessionMultiFactor<sup>3</sup></span><span class="sxs-lookup"><span data-stu-id="235a5-208">MaxAgeSessionMultiFactor<sup>3</sup></span></span> |<span data-ttu-id="235a5-209">Tokeny relace (trvalá nebo zajišťováno)</span><span class="sxs-lookup"><span data-stu-id="235a5-209">Session tokens (persistent and nonpersistent)</span></span> |<span data-ttu-id="235a5-210">Dokud odvolat</span><span class="sxs-lookup"><span data-stu-id="235a5-210">Until-revoked</span></span> |<span data-ttu-id="235a5-211">10 minut</span><span class="sxs-lookup"><span data-stu-id="235a5-211">10 minutes</span></span> |<span data-ttu-id="235a5-212">Dokud odvolat<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="235a5-212">Until-revoked<sup>1</sup></span></span> |

* <span data-ttu-id="235a5-213"><sup>1</sup>365 dnů je maximální délku explicitní, které lze nastavit pro tyto atributy.</span><span class="sxs-lookup"><span data-stu-id="235a5-213"><sup>1</sup>365 days is the maximum explicit length that can be set for these attributes.</span></span>
* <span data-ttu-id="235a5-214"><sup>2</sup>Pokud **MaxAgeSessionSingleFactor** není nastaven, má tato hodnota **MaxAgeSingleFactor** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="235a5-214"><sup>2</sup>If  **MaxAgeSessionSingleFactor** is not set, this value takes the **MaxAgeSingleFactor** value.</span></span> <span data-ttu-id="235a5-215">Pokud ani parametr je nastaven, vlastnost přijímá výchozí hodnota (dokud odvolat).</span><span class="sxs-lookup"><span data-stu-id="235a5-215">If neither parameter is set, the property takes the default value (until-revoked).</span></span>
* <span data-ttu-id="235a5-216"><sup>3</sup>Pokud **MaxAgeSessionMultiFactor** není nastaven, má tato hodnota **MaxAgeMultiFactor** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="235a5-216"><sup>3</sup>If  **MaxAgeSessionMultiFactor** is not set, this value takes the **MaxAgeMultiFactor** value.</span></span> <span data-ttu-id="235a5-217">Pokud ani parametr je nastaven, vlastnost přijímá výchozí hodnota (dokud odvolat).</span><span class="sxs-lookup"><span data-stu-id="235a5-217">If neither parameter is set, the property takes the default value (until-revoked).</span></span>

### <a name="exceptions"></a><span data-ttu-id="235a5-218">Výjimky</span><span class="sxs-lookup"><span data-stu-id="235a5-218">Exceptions</span></span>
| <span data-ttu-id="235a5-219">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="235a5-219">Property</span></span> | <span data-ttu-id="235a5-220">Ovlivňuje</span><span class="sxs-lookup"><span data-stu-id="235a5-220">Affects</span></span> | <span data-ttu-id="235a5-221">Výchozí</span><span class="sxs-lookup"><span data-stu-id="235a5-221">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="235a5-222">Aktualizace tokenu maximální stáří (vydán pro federované uživatele, kteří mají dostatečná odvolání informace<sup>1</sup>)</span><span class="sxs-lookup"><span data-stu-id="235a5-222">Refresh Token Max Age (issued for federated users who have insufficient revocation information<sup>1</sup>)</span></span> |<span data-ttu-id="235a5-223">Obnovovacích tokenů (vydán pro federované uživatele, kteří mají dostatečná odvolání informace<sup>1</sup>)</span><span class="sxs-lookup"><span data-stu-id="235a5-223">Refresh tokens (issued for federated users who have insufficient revocation information<sup>1</sup>)</span></span> |<span data-ttu-id="235a5-224">12 hodin</span><span class="sxs-lookup"><span data-stu-id="235a5-224">12 hours</span></span> |
| <span data-ttu-id="235a5-225">Aktualizace tokenu neaktivní doba Max (vydán pro důvěrné klientů)</span><span class="sxs-lookup"><span data-stu-id="235a5-225">Refresh Token Max Inactive Time (issued for confidential clients)</span></span> |<span data-ttu-id="235a5-226">Obnovovacích tokenů (vydán pro důvěrné klientů)</span><span class="sxs-lookup"><span data-stu-id="235a5-226">Refresh tokens (issued for confidential clients)</span></span> |<span data-ttu-id="235a5-227">90 dnů</span><span class="sxs-lookup"><span data-stu-id="235a5-227">90 days</span></span> |
| <span data-ttu-id="235a5-228">Aktualizace tokenu maximální stáří (vydán pro důvěrné klientů)</span><span class="sxs-lookup"><span data-stu-id="235a5-228">Refresh Token Max Age (issued for confidential clients)</span></span> |<span data-ttu-id="235a5-229">Obnovovacích tokenů (vydán pro důvěrné klientů)</span><span class="sxs-lookup"><span data-stu-id="235a5-229">Refresh tokens (issued for confidential clients)</span></span> |<span data-ttu-id="235a5-230">Dokud odvolat</span><span class="sxs-lookup"><span data-stu-id="235a5-230">Until-revoked</span></span> |

* <span data-ttu-id="235a5-231"><sup>1</sup>federovaný uživatelé, kteří mají dostatečná odvolání informace zahrnují všechny uživatele, kteří nemají atribut "LastPasswordChangeTimestamp" synchronizované.</span><span class="sxs-lookup"><span data-stu-id="235a5-231"><sup>1</sup>Federated users who have insufficient revocation information include any users who do not have the "LastPasswordChangeTimestamp" attribute synced.</span></span> <span data-ttu-id="235a5-232">Tito uživatelé mají Tento krátký maximální stáří, protože AAD se nepodařilo ověřit při zrušení tokeny, které jsou svázané s původním pověření (například hesla, která byla změněna) a musí v další často se ujistit, že uživatel a přidružené tokeny stále dobrý stojící zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="235a5-232">These users are given this short Max Age because AAD is unable to verify when to revoke tokens that are tied to an old credential (such as a password that has been changed) and must check back in more frequently to ensure that the user and associated tokens are still in good standing.</span></span> <span data-ttu-id="235a5-233">Správci klientů ke zlepšení toto prostředí, musíte zajistit, že probíhá synchronizace. atribut "LastPasswordChangeTimestamp" (to je možné nastavit v objektu user pomocí prostředí Powershell nebo prostřednictvím nebude služba AADSync).</span><span class="sxs-lookup"><span data-stu-id="235a5-233">To improve this experience, tenant admins must ensure that they are syncing the “LastPasswordChangeTimestamp” attribute (this can be set on the user object using Powershell or through AADSync).</span></span>

### <a name="policy-evaluation-and-prioritization"></a><span data-ttu-id="235a5-234">Vyhodnocení zásad a stanovení priorit</span><span class="sxs-lookup"><span data-stu-id="235a5-234">Policy evaluation and prioritization</span></span>
<span data-ttu-id="235a5-235">Můžete vytvořit a pak mu přiřaďte zásadu životnost tokenu na konkrétní aplikaci, pro vaši organizaci a objekty služby.</span><span class="sxs-lookup"><span data-stu-id="235a5-235">You can create and then assign a token lifetime policy to a specific application, to your organization, and to service principals.</span></span> <span data-ttu-id="235a5-236">Víc zásad mohou vztahovat na konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-236">Multiple policies might apply to a specific application.</span></span> <span data-ttu-id="235a5-237">Životnost tokenu zásadu, projeví se řídí následujícími pravidly:</span><span class="sxs-lookup"><span data-stu-id="235a5-237">The token lifetime policy that takes effect follows these rules:</span></span>

* <span data-ttu-id="235a5-238">Pokud zásady je explicitně přiřazen k objektu služby, se vynutí.</span><span class="sxs-lookup"><span data-stu-id="235a5-238">If a policy is explicitly assigned to the service principal, it is enforced.</span></span>
* <span data-ttu-id="235a5-239">Pokud žádné zásady explicitně přiřazeny k objektu zabezpečení služby, je vyžadována zásadu explicitně přiřazená k organizaci nadřazeného objektu služby.</span><span class="sxs-lookup"><span data-stu-id="235a5-239">If no policy is explicitly assigned to the service principal, a policy explicitly assigned to the parent organization of the service principal is enforced.</span></span>
* <span data-ttu-id="235a5-240">Pokud objekt služby nebo organizace, není explicitně přiřazena žádná zásada, je vyžadována u zásady přiřazené k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-240">If no policy is explicitly assigned to the service principal or to the organization, the policy assigned to the application is enforced.</span></span>
* <span data-ttu-id="235a5-241">Pokud žádné zásady byl přiřazen objekt služby, organizace nebo objekt aplikace, je vyžadována výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="235a5-241">If no policy has been assigned to the service principal, the organization, or the application object, the default values is enforced.</span></span> <span data-ttu-id="235a5-242">(Podívejte se na tabulku v [konfigurovat životnost tokenu vlastnosti](#configurable-token-lifetime-properties).)</span><span class="sxs-lookup"><span data-stu-id="235a5-242">(See the table in [Configurable token lifetime properties](#configurable-token-lifetime-properties).)</span></span>

<span data-ttu-id="235a5-243">Další informace o vztah mezi objekty aplikací a hlavní objekty služby najdete v tématu [aplikace a služby hlavní objekty ve službě Azure Active Directory](active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="235a5-243">For more information about the relationship between application objects and service principal objects, see [Application and service principal objects in Azure Active Directory](active-directory-application-objects.md).</span></span>

<span data-ttu-id="235a5-244">V době, kdy se používá token je vyhodnocena token platnosti.</span><span class="sxs-lookup"><span data-stu-id="235a5-244">A token’s validity is evaluated at the time the token is used.</span></span> <span data-ttu-id="235a5-245">Zásada s nejvyšší prioritou na aplikaci, která je přistupuje projeví.</span><span class="sxs-lookup"><span data-stu-id="235a5-245">The policy with the highest priority on the application that is being accessed takes effect.</span></span>

> [!NOTE]
> <span data-ttu-id="235a5-246">Tady je příklad scénáře.</span><span class="sxs-lookup"><span data-stu-id="235a5-246">Here's an example scenario.</span></span>
>
> <span data-ttu-id="235a5-247">Uživatel požaduje přístup k dva webové aplikace: webové aplikace A a B. webové aplikace</span><span class="sxs-lookup"><span data-stu-id="235a5-247">A user wants to access two web applications: Web Application A and Web Application B.</span></span>
> 
> <span data-ttu-id="235a5-248">Faktory:</span><span class="sxs-lookup"><span data-stu-id="235a5-248">Factors:</span></span>
> * <span data-ttu-id="235a5-249">Obě webové aplikace jsou ve stejné organizaci nadřazené.</span><span class="sxs-lookup"><span data-stu-id="235a5-249">Both web applications are in the same parent organization.</span></span>
> * <span data-ttu-id="235a5-250">Token 1 zásad životního cyklu relace tokenu maximální stáří osm hodin je nastaven jako výchozí nadřazené organizace.</span><span class="sxs-lookup"><span data-stu-id="235a5-250">Token Lifetime Policy 1 with a Session Token Max Age of eight hours is set as the parent organization’s default.</span></span>
> * <span data-ttu-id="235a5-251">Webové aplikace A je použití regular webovou aplikaci a není propojený s žádné zásady.</span><span class="sxs-lookup"><span data-stu-id="235a5-251">Web Application A is a regular-use web application and isn’t linked to any policies.</span></span>
> * <span data-ttu-id="235a5-252">Webovou aplikaci B se používá pro vysoce citlivých procesů.</span><span class="sxs-lookup"><span data-stu-id="235a5-252">Web Application B is used for highly sensitive processes.</span></span> <span data-ttu-id="235a5-253">Jeho instanční objekt se propojí tokenu 2 zásad životního cyklu, které má relace tokenu maximální stáří 30 minut.</span><span class="sxs-lookup"><span data-stu-id="235a5-253">Its service principal is linked to Token Lifetime Policy 2, which has a Session Token Max Age of 30 minutes.</span></span>
>
> <span data-ttu-id="235a5-254">Na 12:00 PM uživatel spustí novou relaci prohlížeče a pokusí o přístup k webové aplikaci A. Uživatel se přesměruje na Azure AD a se zobrazí výzva k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="235a5-254">At 12:00 PM, the user starts a new browser session and tries to access Web Application A. The user is redirected to Azure AD and is asked to sign in.</span></span> <span data-ttu-id="235a5-255">Tím se vytvoří soubor cookie, který obsahuje token relace v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="235a5-255">This creates a cookie that has a session token in the browser.</span></span> <span data-ttu-id="235a5-256">Uživatel je přesměrován zpět na webovou aplikaci A k tokenu ID, která umožňuje uživatelům přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-256">The user is redirected back to Web Application A with an ID token that allows the user to access the application.</span></span>
>
> <span data-ttu-id="235a5-257">Ve 12:15 se uživatel pokusí o přístup k webové aplikaci B. Prohlížeč přesměruje do služby Azure AD, který zjistí soubor cookie relace.</span><span class="sxs-lookup"><span data-stu-id="235a5-257">At 12:15 PM, the user tries to access Web Application B. The browser redirects to Azure AD, which detects the session cookie.</span></span> <span data-ttu-id="235a5-258">Objekt B aplikace webové služby je propojený s tokenu 2 zásad životního cyklu, ale je také součástí nadřazené organizace s výchozí Token 1 zásad životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="235a5-258">Web Application B’s service principal is linked to Token Lifetime Policy 2, but it's also part of the parent organization, with default Token Lifetime Policy 1.</span></span> <span data-ttu-id="235a5-259">Tokenu 2 zásad životního cyklu začne platit, protože zásady připojené k objekty služby mají vyšší prioritu než výchozích zásad organizace.</span><span class="sxs-lookup"><span data-stu-id="235a5-259">Token Lifetime Policy 2 takes effect because policies linked to service principals have a higher priority than organization default policies.</span></span> <span data-ttu-id="235a5-260">Token relace původně vydán v posledních 30 minut, takže bude považován za platný.</span><span class="sxs-lookup"><span data-stu-id="235a5-260">The session token was originally issued within the last 30 minutes, so it is considered valid.</span></span> <span data-ttu-id="235a5-261">Uživatel je přesměrován zpět na webovou aplikaci B k tokenu ID, která jim udělí přístup.</span><span class="sxs-lookup"><span data-stu-id="235a5-261">The user is redirected back to Web Application B with an ID token that grants them access.</span></span>
>
> <span data-ttu-id="235a5-262">: 00: 00 uživatel pokusí o přístup k webové aplikaci A. Uživatel se přesměruje do služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="235a5-262">At 1:00 PM, the user tries to access Web Application A. The user is redirected to Azure AD.</span></span> <span data-ttu-id="235a5-263">Webové aplikace A není propojený s žádnými zásadami, ale protože je v organizaci s výchozí Token 1 zásad životního cyklu, tyto zásady projeví.</span><span class="sxs-lookup"><span data-stu-id="235a5-263">Web Application A is not linked to any policies, but because it is in an organization with default Token Lifetime Policy 1, that policy takes effect.</span></span> <span data-ttu-id="235a5-264">Je zjištěna souboru cookie relace, který byl původně vydán během posledních 8 hodin.</span><span class="sxs-lookup"><span data-stu-id="235a5-264">The session cookie that was originally issued within the last eight hours is detected.</span></span> <span data-ttu-id="235a5-265">Uživatel je bezobslužně přesměrován zpět do webové aplikace A s ID nový token.</span><span class="sxs-lookup"><span data-stu-id="235a5-265">The user is silently redirected back to Web Application A with a new ID token.</span></span> <span data-ttu-id="235a5-266">Uživatel není vyžadován pro ověření.</span><span class="sxs-lookup"><span data-stu-id="235a5-266">The user is not required to authenticate.</span></span>
>
> <span data-ttu-id="235a5-267">Okamžitě následně uživatel pokusí o přístup k webové aplikaci B. Uživatel se přesměruje do služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="235a5-267">Immediately afterward, the user tries to access Web Application B. The user is redirected to Azure AD.</span></span> <span data-ttu-id="235a5-268">Jako dříve, tokenu 2 životnost zásad se projeví.</span><span class="sxs-lookup"><span data-stu-id="235a5-268">As before, Token Lifetime Policy 2 takes effect.</span></span> <span data-ttu-id="235a5-269">Vzhledem k tomu, že byl token vydán víc než 30 minutami, bude uživatel vyzván k znovu zadejte své přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="235a5-269">Because the token was issued more than 30 minutes ago, the user is prompted to reenter their sign-in credentials.</span></span> <span data-ttu-id="235a5-270">Jsou vydávány ID token a relace zcela nový token.</span><span class="sxs-lookup"><span data-stu-id="235a5-270">A brand-new session token and ID token are issued.</span></span> <span data-ttu-id="235a5-271">Uživatele můžete poté přistoupit B. webové aplikace</span><span class="sxs-lookup"><span data-stu-id="235a5-271">The user can then access Web Application B.</span></span>
>
>

## <a name="configurable-policy-property-details"></a><span data-ttu-id="235a5-272">Podrobnosti vlastnost konfigurovat zásady</span><span class="sxs-lookup"><span data-stu-id="235a5-272">Configurable policy property details</span></span>
### <a name="access-token-lifetime"></a><span data-ttu-id="235a5-273">Životnost tokenu přístupu</span><span class="sxs-lookup"><span data-stu-id="235a5-273">Access Token Lifetime</span></span>
<span data-ttu-id="235a5-274">**Řetězec:** AccessTokenLifetime</span><span class="sxs-lookup"><span data-stu-id="235a5-274">**String:** AccessTokenLifetime</span></span>

<span data-ttu-id="235a5-275">**Ovlivňuje:** přístupové tokeny, tokeny typu ID</span><span class="sxs-lookup"><span data-stu-id="235a5-275">**Affects:** Access tokens, ID tokens</span></span>

<span data-ttu-id="235a5-276">**Souhrn:** tato zásada určuje, jak dlouho přístup a tokeny typu ID pro tento prostředek jsou považovány za platné.</span><span class="sxs-lookup"><span data-stu-id="235a5-276">**Summary:** This policy controls how long access and ID tokens for this resource are considered valid.</span></span> <span data-ttu-id="235a5-277">Snižuje dobu životnosti tokenu přístupu vlastnost snižuje riziko přístupového tokenu nebo ID token používá škodlivý objektu actor pro delší dobu.</span><span class="sxs-lookup"><span data-stu-id="235a5-277">Reducing the Access Token Lifetime property mitigates the risk of an access token or ID token being used by a malicious actor for an extended period of time.</span></span> <span data-ttu-id="235a5-278">(Tyto tokeny nelze odvolat.) O kompromisu je nepříznivě ovlivňuje výkon, protože tokeny vyměnit častěji.</span><span class="sxs-lookup"><span data-stu-id="235a5-278">(These tokens cannot be revoked.) The trade-off is that performance is adversely affected, because the tokens have to be replaced more often.</span></span>

### <a name="refresh-token-max-inactive-time"></a><span data-ttu-id="235a5-279">Aktualizace tokenu maximální doba neaktivní</span><span class="sxs-lookup"><span data-stu-id="235a5-279">Refresh Token Max Inactive Time</span></span>
<span data-ttu-id="235a5-280">**Řetězec:** MaxInactiveTime</span><span class="sxs-lookup"><span data-stu-id="235a5-280">**String:** MaxInactiveTime</span></span>

<span data-ttu-id="235a5-281">**Ovlivňuje:** obnovovacích tokenů</span><span class="sxs-lookup"><span data-stu-id="235a5-281">**Affects:** Refresh tokens</span></span>

<span data-ttu-id="235a5-282">**Souhrn:** tato zásada určuje, kolik token obnovení může být než klienta můžete už použít k načtení nový pár tokenu přístupu nebo aktualizace, při pokusu o přístup k tomuto prostředku.</span><span class="sxs-lookup"><span data-stu-id="235a5-282">**Summary:** This policy controls how old a refresh token can be before a client can no longer use it to retrieve a new access/refresh token pair when attempting to access this resource.</span></span> <span data-ttu-id="235a5-283">Vzhledem k tomu, že nový token obnovení je obvykle vrácena, pokud se používá token obnovení, zabraňuje tato zásada přístupu, pokud se klient pokusí o přístup k jakémukoli prostředku pomocí aktuální obnovovací token během zadaném časovém období.</span><span class="sxs-lookup"><span data-stu-id="235a5-283">Because a new refresh token usually is returned when a refresh token is used, this policy prevents access if the client tries to access any resource by using the current refresh token during the specified period of time.</span></span>

<span data-ttu-id="235a5-284">Tato zásada vynutí uživatelů, kteří nebyly aktivní na jejich klienta k novému ověření, k načtení nový token obnovení.</span><span class="sxs-lookup"><span data-stu-id="235a5-284">This policy forces users who have not been active on their client to reauthenticate to retrieve a new refresh token.</span></span>

<span data-ttu-id="235a5-285">Vlastnost aktualizovat Token maximální neaktivní doba musí být nastavená na hodnotu nižší než maximální stáří tokenu Single-Factor a vlastnosti Multi-Factor aktualizovat Token maximální stáří.</span><span class="sxs-lookup"><span data-stu-id="235a5-285">The Refresh Token Max Inactive Time property must be set to a lower value than the Single-Factor Token Max Age and the Multi-Factor Refresh Token Max Age properties.</span></span>

### <a name="single-factor-refresh-token-max-age"></a><span data-ttu-id="235a5-286">Single-Factor aktualizace tokenu maximální stáří</span><span class="sxs-lookup"><span data-stu-id="235a5-286">Single-Factor Refresh Token Max Age</span></span>
<span data-ttu-id="235a5-287">**Řetězec:** MaxAgeSingleFactor</span><span class="sxs-lookup"><span data-stu-id="235a5-287">**String:** MaxAgeSingleFactor</span></span>

<span data-ttu-id="235a5-288">**Ovlivňuje:** obnovovacích tokenů</span><span class="sxs-lookup"><span data-stu-id="235a5-288">**Affects:** Refresh tokens</span></span>

<span data-ttu-id="235a5-289">**Souhrn:** této zásady ovládací prvky, jak dlouho může uživatel používat token obnovení získat nový pár tokenu přístupu nebo obnovení po jejich poslední úspěšně ověření pomocí pouze jeden faktor.</span><span class="sxs-lookup"><span data-stu-id="235a5-289">**Summary:** This policy controls how long a user can use a refresh token to get a new access/refresh token pair after they last authenticated successfully by using only a single factor.</span></span> <span data-ttu-id="235a5-290">Jakmile je uživatel ověří a obdrží token nové aktualizace, uživatel může použít tok tokenu obnovení pro zadané časové období.</span><span class="sxs-lookup"><span data-stu-id="235a5-290">After a user authenticates and receives a new refresh token, the user can use the refresh token flow for the specified period of time.</span></span> <span data-ttu-id="235a5-291">(To platí, dokud, aktuální token obnovení nebude odvolaný, a není zbývající nepoužité po dobu delší dobu, než je neaktivní.) V tomto bodě že uživatel musí k novému ověření pro příjem nové obnovovací token.</span><span class="sxs-lookup"><span data-stu-id="235a5-291">(This is true as long as the current refresh token is not revoked, and it is not left unused for longer than the inactive time.) At that point, the user is forced to reauthenticate to receive a new refresh token.</span></span>

<span data-ttu-id="235a5-292">Snižuje maximální stáří vynutí se tak uživatelé mohli ověřit častěji.</span><span class="sxs-lookup"><span data-stu-id="235a5-292">Reducing the max age forces users to authenticate more often.</span></span> <span data-ttu-id="235a5-293">Protože jedním Multi-Factor authentication je považován za méně bezpečné než vícefaktorového ověřování, doporučujeme nastavit tuto vlastnost na hodnotu, která je rovna nebo menší než vlastnost Multi-Factor aktualizovat Token maximální stáří.</span><span class="sxs-lookup"><span data-stu-id="235a5-293">Because single-factor authentication is considered less secure than multi-factor authentication, we recommend that you set this property to a value that is equal to or lesser than the Multi-Factor Refresh Token Max Age property.</span></span>

### <a name="multi-factor-refresh-token-max-age"></a><span data-ttu-id="235a5-294">Maximální stáří tokenu Multi-Factor aktualizace</span><span class="sxs-lookup"><span data-stu-id="235a5-294">Multi-Factor Refresh Token Max Age</span></span>
<span data-ttu-id="235a5-295">**Řetězec:** MaxAgeMultiFactor</span><span class="sxs-lookup"><span data-stu-id="235a5-295">**String:** MaxAgeMultiFactor</span></span>

<span data-ttu-id="235a5-296">**Ovlivňuje:** obnovovacích tokenů</span><span class="sxs-lookup"><span data-stu-id="235a5-296">**Affects:** Refresh tokens</span></span>

<span data-ttu-id="235a5-297">**Souhrn:** této zásady ovládací prvky, jak dlouho může uživatel používat token obnovení získat nový pár tokenu přístupu nebo obnovení po jejich poslední úspěšně ověření pomocí několika faktory.</span><span class="sxs-lookup"><span data-stu-id="235a5-297">**Summary:** This policy controls how long a user can use a refresh token to get a new access/refresh token pair after they last authenticated successfully by using multiple factors.</span></span> <span data-ttu-id="235a5-298">Jakmile je uživatel ověří a obdrží token nové aktualizace, uživatel může použít tok tokenu obnovení pro zadané časové období.</span><span class="sxs-lookup"><span data-stu-id="235a5-298">After a user authenticates and receives a new refresh token, the user can use the refresh token flow for the specified period of time.</span></span> <span data-ttu-id="235a5-299">(To je hodnota true, dokud nebude odvolaný aktuální obnovovací token a není po dobu delší než neaktivní doba.) V tomto okamžiku jsou uživatelé přinucení k novému ověření pro příjem nové obnovovací token.</span><span class="sxs-lookup"><span data-stu-id="235a5-299">(This is true as long as the current refresh token is not revoked, and it is not unused for longer than the inactive time.) At that point, users are forced to reauthenticate to receive a new refresh token.</span></span>

<span data-ttu-id="235a5-300">Snižuje maximální stáří vynutí se tak uživatelé mohli ověřit častěji.</span><span class="sxs-lookup"><span data-stu-id="235a5-300">Reducing the max age forces users to authenticate more often.</span></span> <span data-ttu-id="235a5-301">Protože jedním Multi-Factor authentication je považován za méně bezpečné než vícefaktorového ověřování, doporučujeme nastavit tuto vlastnost na hodnotu, která je rovna nebo větší než vlastnost Single-Factor aktualizovat Token maximální stáří.</span><span class="sxs-lookup"><span data-stu-id="235a5-301">Because single-factor authentication is considered less secure than multi-factor authentication, we recommend that you set this property to a value that is equal to or greater than the Single-Factor Refresh Token Max Age property.</span></span>

### <a name="single-factor-session-token-max-age"></a><span data-ttu-id="235a5-302">Maximální stáří tokenu Single-Factor relace</span><span class="sxs-lookup"><span data-stu-id="235a5-302">Single-Factor Session Token Max Age</span></span>
<span data-ttu-id="235a5-303">**Řetězec:** MaxAgeSessionSingleFactor</span><span class="sxs-lookup"><span data-stu-id="235a5-303">**String:** MaxAgeSessionSingleFactor</span></span>

<span data-ttu-id="235a5-304">**Ovlivňuje:** tokeny relace (trvalá nebo zajišťováno)</span><span class="sxs-lookup"><span data-stu-id="235a5-304">**Affects:** Session tokens (persistent and nonpersistent)</span></span>

<span data-ttu-id="235a5-305">**Souhrn:** této zásady ovládací prvky, jak dlouho může uživatel používat token relace získat nové ID a token relace po jejich poslední úspěšně ověření pomocí pouze jeden faktor.</span><span class="sxs-lookup"><span data-stu-id="235a5-305">**Summary:** This policy controls how long a user can use a session token to get a new ID and session token after they last authenticated successfully by using only a single factor.</span></span> <span data-ttu-id="235a5-306">Jakmile je uživatel ověří a obdrží token novou relaci, uživatel může použít tok tokenu relace pro zadané časové období.</span><span class="sxs-lookup"><span data-stu-id="235a5-306">After a user authenticates and receives a new session token, the user can use the session token flow for the specified period of time.</span></span> <span data-ttu-id="235a5-307">(To platí, dokud, aktuální relace tokenu není odvolaný a jestli nevypršela platnost.) Po zadaném časovém období že uživatel musí k novému ověření, přijmout token novou relaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-307">(This is true as long as the current session token is not revoked and has not expired.) After the specified period of time, the user is forced to reauthenticate to receive a new session token.</span></span>

<span data-ttu-id="235a5-308">Snižuje maximální stáří vynutí se tak uživatelé mohli ověřit častěji.</span><span class="sxs-lookup"><span data-stu-id="235a5-308">Reducing the max age forces users to authenticate more often.</span></span> <span data-ttu-id="235a5-309">Protože jedním Multi-Factor authentication je považován za méně bezpečné než vícefaktorového ověřování, doporučujeme nastavit tuto vlastnost na hodnotu, která je rovna nebo menší než vlastnost Multi-Factor relace tokenu maximální stáří.</span><span class="sxs-lookup"><span data-stu-id="235a5-309">Because single-factor authentication is considered less secure than multi-factor authentication, we recommend that you set this property to a value that is equal to or less than the Multi-Factor Session Token Max Age property.</span></span>

### <a name="multi-factor-session-token-max-age"></a><span data-ttu-id="235a5-310">Maximální stáří tokenu Multi-Factor relace</span><span class="sxs-lookup"><span data-stu-id="235a5-310">Multi-Factor Session Token Max Age</span></span>
<span data-ttu-id="235a5-311">**Řetězec:** MaxAgeSessionMultiFactor</span><span class="sxs-lookup"><span data-stu-id="235a5-311">**String:** MaxAgeSessionMultiFactor</span></span>

<span data-ttu-id="235a5-312">**Ovlivňuje:** tokeny relace (trvalá nebo zajišťováno)</span><span class="sxs-lookup"><span data-stu-id="235a5-312">**Affects:** Session tokens (persistent and nonpersistent)</span></span>

<span data-ttu-id="235a5-313">**Souhrn:** této zásady ovládací prvky jak dlouho může uživatel používat token relace se získat od poslední úspěšně musí ověřit pomocí několika faktory nové ID a relace token.</span><span class="sxs-lookup"><span data-stu-id="235a5-313">**Summary:** This policy controls how long a user can use a session token to get a new ID and session token after the last time they authenticated successfully by using multiple factors.</span></span> <span data-ttu-id="235a5-314">Jakmile je uživatel ověří a obdrží token novou relaci, uživatel může použít tok tokenu relace pro zadané časové období.</span><span class="sxs-lookup"><span data-stu-id="235a5-314">After a user authenticates and receives a new session token, the user can use the session token flow for the specified period of time.</span></span> <span data-ttu-id="235a5-315">(To platí, dokud, aktuální relace tokenu není odvolaný a jestli nevypršela platnost.) Po zadaném časovém období že uživatel musí k novému ověření, přijmout token novou relaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-315">(This is true as long as the current session token is not revoked and has not expired.) After the specified period of time, the user is forced to reauthenticate to receive a new session token.</span></span>

<span data-ttu-id="235a5-316">Snižuje maximální stáří vynutí se tak uživatelé mohli ověřit častěji.</span><span class="sxs-lookup"><span data-stu-id="235a5-316">Reducing the max age forces users to authenticate more often.</span></span> <span data-ttu-id="235a5-317">Protože jedním Multi-Factor authentication je považován za méně bezpečné než vícefaktorového ověřování, doporučujeme nastavit tuto vlastnost na hodnotu, která je rovna nebo větší než vlastnost Single-Factor relace tokenu maximální stáří.</span><span class="sxs-lookup"><span data-stu-id="235a5-317">Because single-factor authentication is considered less secure than multi-factor authentication, we recommend that you set this property to a value that is equal to or greater than the Single-Factor Session Token Max Age property.</span></span>

## <a name="example-token-lifetime-policies"></a><span data-ttu-id="235a5-318">Příkladem zásad životnost tokenu</span><span class="sxs-lookup"><span data-stu-id="235a5-318">Example token lifetime policies</span></span>
<span data-ttu-id="235a5-319">Mnoho scénářů je možné ve službě Azure AD při vytváření a správa životnosti tokenu pro aplikace, objekty služby a celkové organizaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-319">Many scenarios are possible in Azure AD when you can create and manage token lifetimes for apps, service principals, and your overall organization.</span></span> <span data-ttu-id="235a5-320">V této části jsme provede několik běžných scénářů zásady, které můžete použít nové pravidel pro:</span><span class="sxs-lookup"><span data-stu-id="235a5-320">In this section, we walk through a few common policy scenarios that can help you impose new rules for:</span></span>

* <span data-ttu-id="235a5-321">Životnost tokenu</span><span class="sxs-lookup"><span data-stu-id="235a5-321">Token Lifetime</span></span>
* <span data-ttu-id="235a5-322">Token maximální doba neaktivní</span><span class="sxs-lookup"><span data-stu-id="235a5-322">Token Max Inactive Time</span></span>
* <span data-ttu-id="235a5-323">Maximální stáří tokenu</span><span class="sxs-lookup"><span data-stu-id="235a5-323">Token Max Age</span></span>

<span data-ttu-id="235a5-324">V příkladech dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="235a5-324">In the examples, you can learn how to:</span></span>

* <span data-ttu-id="235a5-325">Správa organizace výchozí zásady</span><span class="sxs-lookup"><span data-stu-id="235a5-325">Manage an organization's default policy</span></span>
* <span data-ttu-id="235a5-326">Vytvoření zásady pro přihlášení k webové</span><span class="sxs-lookup"><span data-stu-id="235a5-326">Create a policy for web sign-in</span></span>
* <span data-ttu-id="235a5-327">Vytvořit zásadu pro nativní aplikaci, která volá webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="235a5-327">Create a policy for a native app that calls a web API</span></span>
* <span data-ttu-id="235a5-328">Spravovat pokročilé zásady</span><span class="sxs-lookup"><span data-stu-id="235a5-328">Manage an advanced policy</span></span>

### <a name="prerequisites"></a><span data-ttu-id="235a5-329">Požadavky</span><span class="sxs-lookup"><span data-stu-id="235a5-329">Prerequisites</span></span>
<span data-ttu-id="235a5-330">V následujících příkladech vytvářet, aktualizovat, propojení a odstranit zásady pro aplikace, objekty služby a celkové organizaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-330">In the following examples, you create, update, link, and delete policies for apps, service principals, and your overall organization.</span></span> <span data-ttu-id="235a5-331">Pokud jste ještě Azure AD, doporučujeme Další informace o [jak získat klienta Azure AD](active-directory-howto-tenant.md) předtím, než budete pokračovat v těchto příkladech.</span><span class="sxs-lookup"><span data-stu-id="235a5-331">If you are new to Azure AD, we recommend that you learn about [how to get an Azure AD tenant](active-directory-howto-tenant.md) before you proceed with these examples.</span></span>  

<span data-ttu-id="235a5-332">Chcete-li začít, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="235a5-332">To get started, do the following steps:</span></span>

1. <span data-ttu-id="235a5-333">Stáhněte si nejnovější [Azure AD PowerShell modulu verze Public Preview](https://www.powershellgallery.com/packages/AzureADPreview).</span><span class="sxs-lookup"><span data-stu-id="235a5-333">Download the latest [Azure AD PowerShell Module Public Preview release](https://www.powershellgallery.com/packages/AzureADPreview).</span></span>
2. <span data-ttu-id="235a5-334">Spustit `Connect` příkaz pro přihlášení k účtu správce služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="235a5-334">Run the `Connect` command to sign in to your Azure AD admin account.</span></span> <span data-ttu-id="235a5-335">Spusťte tento příkaz pokaždé, když zahájit novou relaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-335">Run this command each time you start a new session.</span></span>

    ```PowerShell
    Connect-AzureAD -Confirm
    ```

3. <span data-ttu-id="235a5-336">Pokud chcete zobrazit všechny zásady, které byly vytvořeny ve vaší organizaci, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="235a5-336">To see all policies that have been created in your organization, run the following command.</span></span> <span data-ttu-id="235a5-337">Tento příkaz spusťte po většinu operací v následujících scénářích.</span><span class="sxs-lookup"><span data-stu-id="235a5-337">Run this command after most operations in the following scenarios.</span></span> <span data-ttu-id="235a5-338">Spuštěním příkazu také vám pomůže získat ** ** zásad.</span><span class="sxs-lookup"><span data-stu-id="235a5-338">Running the command also helps you get the ** ** of your policies.</span></span>

    ```PowerShell
    Get-AzureADPolicy
    ```

### <a name="example-manage-an-organizations-default-policy"></a><span data-ttu-id="235a5-339">Příklad: Správa organizace výchozí zásady</span><span class="sxs-lookup"><span data-stu-id="235a5-339">Example: Manage an organization's default policy</span></span>
<span data-ttu-id="235a5-340">V tomto příkladu vytvoříte zásadu, která umožňuje uživatelům přihlásit se méně často v rámci celé organizace.</span><span class="sxs-lookup"><span data-stu-id="235a5-340">In this example, you create a policy that lets your users sign in less frequently across your entire organization.</span></span> <span data-ttu-id="235a5-341">K tomuto účelu vytvořte zásady životnost tokenu pro Single-Factor aktualizovat tokeny, které se použije v organizaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-341">To do this, create a token lifetime policy for Single-Factor Refresh Tokens, which is applied across your organization.</span></span> <span data-ttu-id="235a5-342">Zásady se použijí na každou aplikaci ve vaší organizaci a pro všechny hlavní služby, který ještě nemá zásady nastavené.</span><span class="sxs-lookup"><span data-stu-id="235a5-342">The policy is applied to every application in your organization, and to each service principal that doesn’t already have a policy set.</span></span>

1. <span data-ttu-id="235a5-343">Vytvořte zásadu životnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="235a5-343">Create a token lifetime policy.</span></span>

    1.  <span data-ttu-id="235a5-344">Sada aktualizace Single-Factor Token, který má "dokud odvolaný."</span><span class="sxs-lookup"><span data-stu-id="235a5-344">Set the Single-Factor Refresh Token to "until-revoked."</span></span> <span data-ttu-id="235a5-345">Token nevyprší, dokud je odvolat přístup.</span><span class="sxs-lookup"><span data-stu-id="235a5-345">The token doesn't expire until access is revoked.</span></span> <span data-ttu-id="235a5-346">Vytvořte následující definice zásady:</span><span class="sxs-lookup"><span data-stu-id="235a5-346">Create the following policy definition:</span></span>

        ```PowerShell
        @('{
            "TokenLifetimePolicy":
            {
                "Version":1,
                "MaxAgeSingleFactor":"until-revoked"
            }
        }')
        ```

    2.  <span data-ttu-id="235a5-347">Pokud chcete vytvořit zásadu, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="235a5-347">To create the policy, run the following command:</span></span>

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    3.  <span data-ttu-id="235a5-348">Chcete zobrazit nové zásady a získat nastavení zásad **ObjectId**, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="235a5-348">To see your new policy, and to get the policy's **ObjectId**, run the following command:</span></span>

        ```PowerShell
        Get-AzureADPolicy
        ```

2. <span data-ttu-id="235a5-349">Aktualizujte zásady.</span><span class="sxs-lookup"><span data-stu-id="235a5-349">Update the policy.</span></span>

    <span data-ttu-id="235a5-350">Můžete rozhodnout, že na první zásadu, kterou můžete nastavit v tomto příkladu není striktní, protože vaše služba vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="235a5-350">You might decide that the first policy you set in this example is not as strict as your service requires.</span></span> <span data-ttu-id="235a5-351">Pokud chcete nastavit vaše Single-Factor aktualizace tokenu vyprší za dva dny, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="235a5-351">To set your Single-Factor Refresh Token to expire in two days, run the following command:</span></span>

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId FROM GET COMMAND> -DisplayName "OrganizationDefaultPolicyUpdatedScenario" -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"2.00:00:00"}}')
    ```

### <a name="example-create-a-policy-for-web-sign-in"></a><span data-ttu-id="235a5-352">Příklad: Vytvoření zásady pro přihlášení k webové</span><span class="sxs-lookup"><span data-stu-id="235a5-352">Example: Create a policy for web sign-in</span></span>

<span data-ttu-id="235a5-353">V tomto příkladu vytvoříte zásadu, která vyžaduje, aby uživatelé ověření častěji ve vaší webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-353">In this example, you create a policy that requires users to authenticate more frequently in your web app.</span></span> <span data-ttu-id="235a5-354">Tato zásada nastaví dobu platnosti tokenů přístupu nebo ID a maximální stáří token relace Multi-Factor instanční objekt vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a5-354">This policy sets the lifetime of the access/ID tokens and the max age of a multi-factor session token to the service principal of your web app.</span></span>

1. <span data-ttu-id="235a5-355">Vytvořte zásadu životnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="235a5-355">Create a token lifetime policy.</span></span>

    <span data-ttu-id="235a5-356">Tato zásada pro webové přihlášení, nastaví přístup nebo ID životnost tokenu a stáří tokenu maximální single-factor relace do dvou hodin.</span><span class="sxs-lookup"><span data-stu-id="235a5-356">This policy, for web sign-in, sets the access/ID token lifetime and the max single-factor session token age to two hours.</span></span>

    1.  <span data-ttu-id="235a5-357">Chcete-li vytvořit zásadu, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="235a5-357">To create the policy, run this command:</span></span>

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00","MaxAgeSessionSingleFactor":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  <span data-ttu-id="235a5-358">Chcete zobrazit nové zásady a získání zásady **ObjectId**, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="235a5-358">To see your new policy, and to get the policy **ObjectId**, run the following command:</span></span>

        ```PowerShell
        Get-AzureADPolicy
        ```

2.  <span data-ttu-id="235a5-359">Přiřaďte zásady instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="235a5-359">Assign the policy to your service principal.</span></span> <span data-ttu-id="235a5-360">Je také nutné získat **ObjectId** z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="235a5-360">You also need to get the **ObjectId** of your service principal.</span></span> 

    1.  <span data-ttu-id="235a5-361">Pokud chcete zobrazit všechna firemní objekty služby, se můžete dotazovat [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity).</span><span class="sxs-lookup"><span data-stu-id="235a5-361">To see all your organization's service principals, you can query [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity).</span></span> <span data-ttu-id="235a5-362">Nebo v [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), přihlaste se k účtu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="235a5-362">Or, in [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), sign in to your Azure AD account.</span></span>

    2.  <span data-ttu-id="235a5-363">Pokud máte **ObjectId** vaše služba objektu zabezpečení, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="235a5-363">When you have the **ObjectId** of your service principal, run the following command:</span></span>

        ```PowerShell
        Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
        ```


### <a name="example-create-a-policy-for-a-native-app-that-calls-a-web-api"></a><span data-ttu-id="235a5-364">Příklad: Vytvoření zásady pro nativní aplikaci, která volá webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="235a5-364">Example: Create a policy for a native app that calls a web API</span></span>
<span data-ttu-id="235a5-365">V tomto příkladu vytvoříte zásadu, která vyžaduje, aby uživatelé ověření méně často.</span><span class="sxs-lookup"><span data-stu-id="235a5-365">In this example, you create a policy that requires users to authenticate less frequently.</span></span> <span data-ttu-id="235a5-366">Zásady také prodlouží dobu, kterou uživatel může být neaktivní, než uživatel musí novému ověření.</span><span class="sxs-lookup"><span data-stu-id="235a5-366">The policy also lengthens the amount of time a user can be inactive before the user must reauthenticate.</span></span> <span data-ttu-id="235a5-367">Zásady se použijí pro webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="235a5-367">The policy is applied to the web API.</span></span> <span data-ttu-id="235a5-368">Pokud nativní aplikaci požádá o webové rozhraní API jako prostředek, je tato zásada použitá.</span><span class="sxs-lookup"><span data-stu-id="235a5-368">When the native app requests the web API as a resource, this policy is applied.</span></span>

1. <span data-ttu-id="235a5-369">Vytvořte zásadu životnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="235a5-369">Create a token lifetime policy.</span></span>

    1.  <span data-ttu-id="235a5-370">Pokud chcete vytvořit zásadu striktní pro webové rozhraní API, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="235a5-370">To create a strict policy for a web API, run the following command:</span></span>

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"30.00:00:00","MaxAgeMultiFactor":"until-revoked","MaxAgeSingleFactor":"180.00:00:00"}}') -DisplayName "WebApiDefaultPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  <span data-ttu-id="235a5-371">Chcete zobrazit nové zásady a získání zásady **ObjectId**, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="235a5-371">To see your new policy, and to get the policy **ObjectId**, run the following command:</span></span>

        ```PowerShell
        Get-AzureADPolicy
        ```

2. <span data-ttu-id="235a5-372">Přiřaďte zásady webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="235a5-372">Assign the policy to your web API.</span></span> <span data-ttu-id="235a5-373">Je také nutné získat **ObjectId** vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a5-373">You also need to get the **ObjectId** of your application.</span></span> <span data-ttu-id="235a5-374">Nejlepší způsob, jak najít vaší aplikace **ObjectId** je použití [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="235a5-374">The best way to find your app's **ObjectId** is to use the [Azure portal](https://portal.azure.com/).</span></span>

   <span data-ttu-id="235a5-375">Pokud máte **ObjectId** vaší aplikace, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="235a5-375">When you have the **ObjectId** of your app, run the following command:</span></span>

        ```PowerShell
        Add-AzureADApplicationPolicy -Id <ObjectId of the Application> -RefObjectId <ObjectId of the Policy>
        ```


### <a name="example-manage-an-advanced-policy"></a><span data-ttu-id="235a5-376">Příklad: Správa pokročilé zásady</span><span class="sxs-lookup"><span data-stu-id="235a5-376">Example: Manage an advanced policy</span></span>
<span data-ttu-id="235a5-377">V tomto příkladu můžete vytvořit několik zásady, se dozvíte, jak funguje s prioritou systému.</span><span class="sxs-lookup"><span data-stu-id="235a5-377">In this example, you create a few policies, to learn how the priority system works.</span></span> <span data-ttu-id="235a5-378">Rovněž můžete naučit ke správě víc zásad, které jsou použity na více objektů.</span><span class="sxs-lookup"><span data-stu-id="235a5-378">You also can learn how to manage multiple policies that are applied to several objects.</span></span>

1. <span data-ttu-id="235a5-379">Vytvořte zásadu životnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="235a5-379">Create a token lifetime policy.</span></span>

    1.  <span data-ttu-id="235a5-380">Pokud chcete vytvořit výchozí zásadu organizace, která nastaví dobu životnosti tokenu aktualizovat Single-Factor na 30 dní, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="235a5-380">To create an organization default policy that sets the Single-Factor Refresh Token lifetime to 30 days, run the following command:</span></span>

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"30.00:00:00"}}') -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    2.  <span data-ttu-id="235a5-381">Chcete zobrazit nové zásady a získat nastavení zásad **ObjectId**, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="235a5-381">To see your new policy, and to get the policy's **ObjectId**, run the following command:</span></span>

        ```PowerShell
        Get-AzureADPolicy
        ```

2. <span data-ttu-id="235a5-382">Přiřaďte zásady instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="235a5-382">Assign the policy to a service principal.</span></span>

    <span data-ttu-id="235a5-383">Teď máte zásadu, která platí pro celou organizaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-383">Now, you have a policy that applies to the entire organization.</span></span> <span data-ttu-id="235a5-384">Můžete chtít zachovat tuto zásadu 30denní pro objekt konkrétní služby, ale změnit výchozí zásady organizace na horní limit počtu "dokud odvolaný."</span><span class="sxs-lookup"><span data-stu-id="235a5-384">You might want to preserve this 30-day policy for a specific service principal, but change the organization default policy to the upper limit of "until-revoked."</span></span>

    1.  <span data-ttu-id="235a5-385">Pokud chcete zobrazit všechna firemní objekty služby, se můžete dotazovat [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity).</span><span class="sxs-lookup"><span data-stu-id="235a5-385">To see all your organization's service principals, you can query [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity).</span></span> <span data-ttu-id="235a5-386">Nebo v [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), přihlaste se pomocí účtu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="235a5-386">Or, in [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), sign in by using your Azure AD account.</span></span>

    2.  <span data-ttu-id="235a5-387">Pokud máte **ObjectId** vaše služba objektu zabezpečení, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="235a5-387">When you have the **ObjectId** of your service principal, run the following command:</span></span>

            ```PowerShell
            Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
            ```
        
3. <span data-ttu-id="235a5-388">Nastavte `IsOrganizationDefault` příznak na hodnotu false:</span><span class="sxs-lookup"><span data-stu-id="235a5-388">Set the `IsOrganizationDefault` flag to false:</span></span>

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $false
    ```

4. <span data-ttu-id="235a5-389">Vytvořte novou zásadu výchozí organizace:</span><span class="sxs-lookup"><span data-stu-id="235a5-389">Create a new organization default policy:</span></span>

    ```PowerShell
    New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "ComplexPolicyScenarioTwo" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
    ```

    <span data-ttu-id="235a5-390">Nyní máte původní zásadou propojené s instanční objekt a nové zásady nastaven jako výchozí zásady vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="235a5-390">You now have the original policy linked to your service principal, and the new policy is set as your organization default policy.</span></span> <span data-ttu-id="235a5-391">Je důležité si pamatovat, že zásady uplatněné na objekty služby mají přednost před výchozí zásady organizace.</span><span class="sxs-lookup"><span data-stu-id="235a5-391">It's important to remember that policies applied to service principals have priority over organization default policies.</span></span>

## <a name="cmdlet-reference"></a><span data-ttu-id="235a5-392">Reference k rutinám</span><span class="sxs-lookup"><span data-stu-id="235a5-392">Cmdlet reference</span></span>

### <a name="manage-policies"></a><span data-ttu-id="235a5-393">Správa zásad</span><span class="sxs-lookup"><span data-stu-id="235a5-393">Manage policies</span></span>

<span data-ttu-id="235a5-394">Následující rutiny můžete použít ke správě zásad.</span><span class="sxs-lookup"><span data-stu-id="235a5-394">You can use the following cmdlets to manage policies.</span></span>

#### <a name="new-azureadpolicy"></a><span data-ttu-id="235a5-395">Nové AzureADPolicy</span><span class="sxs-lookup"><span data-stu-id="235a5-395">New-AzureADPolicy</span></span>

<span data-ttu-id="235a5-396">Vytvoří novou zásadu.</span><span class="sxs-lookup"><span data-stu-id="235a5-396">Creates a new policy.</span></span>

```PowerShell
New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsOrganizationDefault <boolean> -Type <Policy Type>
```

| <span data-ttu-id="235a5-397">Parametry</span><span class="sxs-lookup"><span data-stu-id="235a5-397">Parameters</span></span> | <span data-ttu-id="235a5-398">Popis</span><span class="sxs-lookup"><span data-stu-id="235a5-398">Description</span></span> | <span data-ttu-id="235a5-399">Příklad</span><span class="sxs-lookup"><span data-stu-id="235a5-399">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Definition</code> |<span data-ttu-id="235a5-400">Pole stringified formátu JSON, který obsahuje všechny zásady pravidla.</span><span class="sxs-lookup"><span data-stu-id="235a5-400">Array of stringified JSON that contains all the policy's rules.</span></span> | `-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;DisplayName</code> |<span data-ttu-id="235a5-401">Řetězec název zásady.</span><span class="sxs-lookup"><span data-stu-id="235a5-401">String of the policy name.</span></span> |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;IsOrganizationDefault</code> |<span data-ttu-id="235a5-402">V případě hodnoty true, nastaví jako výchozí zásady organizace zásady.</span><span class="sxs-lookup"><span data-stu-id="235a5-402">If true, sets the policy as the organization's default policy.</span></span> <span data-ttu-id="235a5-403">Pokud je hodnota false, neprovede žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="235a5-403">If false, does nothing.</span></span> |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code> |<span data-ttu-id="235a5-404">Typ zásad.</span><span class="sxs-lookup"><span data-stu-id="235a5-404">Type of policy.</span></span> <span data-ttu-id="235a5-405">Pro token životnosti vždy používejte "TokenLifetimePolicy."</span><span class="sxs-lookup"><span data-stu-id="235a5-405">For token lifetimes, always use "TokenLifetimePolicy."</span></span> | `-Type "TokenLifetimePolicy"` |
| <span data-ttu-id="235a5-406"><code>&#8209;AlternativeIdentifier</code>[Nepovinné]</span><span class="sxs-lookup"><span data-stu-id="235a5-406"><code>&#8209;AlternativeIdentifier</code> [Optional]</span></span> |<span data-ttu-id="235a5-407">Nastaví alternativní ID pro tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="235a5-407">Sets an alternative ID for the policy.</span></span> |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="get-azureadpolicy"></a><span data-ttu-id="235a5-408">Get-AzureADPolicy</span><span class="sxs-lookup"><span data-stu-id="235a5-408">Get-AzureADPolicy</span></span>
<span data-ttu-id="235a5-409">Získá všechny zásady služby Azure AD nebo zadanou zásadu.</span><span class="sxs-lookup"><span data-stu-id="235a5-409">Gets all Azure AD policies or a specified policy.</span></span>

```PowerShell
Get-AzureADPolicy
```

| <span data-ttu-id="235a5-410">Parametry</span><span class="sxs-lookup"><span data-stu-id="235a5-410">Parameters</span></span> | <span data-ttu-id="235a5-411">Popis</span><span class="sxs-lookup"><span data-stu-id="235a5-411">Description</span></span> | <span data-ttu-id="235a5-412">Příklad</span><span class="sxs-lookup"><span data-stu-id="235a5-412">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="235a5-413"><code>&#8209;Id</code>[Nepovinné]</span><span class="sxs-lookup"><span data-stu-id="235a5-413"><code>&#8209;Id</code> [Optional]</span></span> |<span data-ttu-id="235a5-414">**ObjectId (Id)** chcete zásad.</span><span class="sxs-lookup"><span data-stu-id="235a5-414">**ObjectId (Id)** of the policy you want.</span></span> |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadpolicyappliedobject"></a><span data-ttu-id="235a5-415">Get-AzureADPolicyAppliedObject</span><span class="sxs-lookup"><span data-stu-id="235a5-415">Get-AzureADPolicyAppliedObject</span></span>
<span data-ttu-id="235a5-416">Získá všechny aplikace a objekty služby, které jsou propojeny s zásadu.</span><span class="sxs-lookup"><span data-stu-id="235a5-416">Gets all apps and service principals that are linked to a policy.</span></span>

```PowerShell
Get-AzureADPolicyAppliedObject -Id <ObjectId of Policy>
```

| <span data-ttu-id="235a5-417">Parametry</span><span class="sxs-lookup"><span data-stu-id="235a5-417">Parameters</span></span> | <span data-ttu-id="235a5-418">Popis</span><span class="sxs-lookup"><span data-stu-id="235a5-418">Description</span></span> | <span data-ttu-id="235a5-419">Příklad</span><span class="sxs-lookup"><span data-stu-id="235a5-419">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="235a5-420">**ObjectId (Id)** chcete zásad.</span><span class="sxs-lookup"><span data-stu-id="235a5-420">**ObjectId (Id)** of the policy you want.</span></span> |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="set-azureadpolicy"></a><span data-ttu-id="235a5-421">Set-AzureADPolicy</span><span class="sxs-lookup"><span data-stu-id="235a5-421">Set-AzureADPolicy</span></span>
<span data-ttu-id="235a5-422">Aktualizuje existující zásady.</span><span class="sxs-lookup"><span data-stu-id="235a5-422">Updates an existing policy.</span></span>

```PowerShell
Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName <string>
```

| <span data-ttu-id="235a5-423">Parametry</span><span class="sxs-lookup"><span data-stu-id="235a5-423">Parameters</span></span> | <span data-ttu-id="235a5-424">Popis</span><span class="sxs-lookup"><span data-stu-id="235a5-424">Description</span></span> | <span data-ttu-id="235a5-425">Příklad</span><span class="sxs-lookup"><span data-stu-id="235a5-425">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="235a5-426">**ObjectId (Id)** chcete zásad.</span><span class="sxs-lookup"><span data-stu-id="235a5-426">**ObjectId (Id)** of the policy you want.</span></span> |`-Id <ObjectId of Policy>` |
| <code>&#8209;DisplayName</code> |<span data-ttu-id="235a5-427">Řetězec název zásady.</span><span class="sxs-lookup"><span data-stu-id="235a5-427">String of the policy name.</span></span> |`-DisplayName "MyTokenPolicy"` |
| <span data-ttu-id="235a5-428"><code>&#8209;Definition</code>[Nepovinné]</span><span class="sxs-lookup"><span data-stu-id="235a5-428"><code>&#8209;Definition</code> [Optional]</span></span> |<span data-ttu-id="235a5-429">Pole stringified formátu JSON, který obsahuje všechny zásady pravidla.</span><span class="sxs-lookup"><span data-stu-id="235a5-429">Array of stringified JSON that contains all the policy's rules.</span></span> |`-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <span data-ttu-id="235a5-430"><code>&#8209;IsOrganizationDefault</code>[Nepovinné]</span><span class="sxs-lookup"><span data-stu-id="235a5-430"><code>&#8209;IsOrganizationDefault</code> [Optional]</span></span> |<span data-ttu-id="235a5-431">V případě hodnoty true, nastaví jako výchozí zásady organizace zásady.</span><span class="sxs-lookup"><span data-stu-id="235a5-431">If true, sets the policy as the organization's default policy.</span></span> <span data-ttu-id="235a5-432">Pokud je hodnota false, neprovede žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="235a5-432">If false, does nothing.</span></span> |`-IsOrganizationDefault $true` |
| <span data-ttu-id="235a5-433"><code>&#8209;Type</code>[Nepovinné]</span><span class="sxs-lookup"><span data-stu-id="235a5-433"><code>&#8209;Type</code> [Optional]</span></span> |<span data-ttu-id="235a5-434">Typ zásad.</span><span class="sxs-lookup"><span data-stu-id="235a5-434">Type of policy.</span></span> <span data-ttu-id="235a5-435">Pro token životnosti vždy používejte "TokenLifetimePolicy."</span><span class="sxs-lookup"><span data-stu-id="235a5-435">For token lifetimes, always use "TokenLifetimePolicy."</span></span> |`-Type "TokenLifetimePolicy"` |
| <span data-ttu-id="235a5-436"><code>&#8209;AlternativeIdentifier</code>[Nepovinné]</span><span class="sxs-lookup"><span data-stu-id="235a5-436"><code>&#8209;AlternativeIdentifier</code> [Optional]</span></span> |<span data-ttu-id="235a5-437">Nastaví alternativní ID pro tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="235a5-437">Sets an alternative ID for the policy.</span></span> |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="remove-azureadpolicy"></a><span data-ttu-id="235a5-438">Odebrat AzureADPolicy</span><span class="sxs-lookup"><span data-stu-id="235a5-438">Remove-AzureADPolicy</span></span>
<span data-ttu-id="235a5-439">Odstraní zadanou zásadu.</span><span class="sxs-lookup"><span data-stu-id="235a5-439">Deletes the specified policy.</span></span>

```PowerShell
 Remove-AzureADPolicy -Id <ObjectId of Policy>
```

| <span data-ttu-id="235a5-440">Parametry</span><span class="sxs-lookup"><span data-stu-id="235a5-440">Parameters</span></span> | <span data-ttu-id="235a5-441">Popis</span><span class="sxs-lookup"><span data-stu-id="235a5-441">Description</span></span> | <span data-ttu-id="235a5-442">Příklad</span><span class="sxs-lookup"><span data-stu-id="235a5-442">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="235a5-443">**ObjectId (Id)** chcete zásad.</span><span class="sxs-lookup"><span data-stu-id="235a5-443">**ObjectId (Id)** of the policy you want.</span></span> | `-Id <ObjectId of Policy>` |

</br></br>

### <a name="application-policies"></a><span data-ttu-id="235a5-444">Zásady aplikací</span><span class="sxs-lookup"><span data-stu-id="235a5-444">Application policies</span></span>
<span data-ttu-id="235a5-445">Následující rutiny můžete použít pro zásady aplikací.</span><span class="sxs-lookup"><span data-stu-id="235a5-445">You can use the following cmdlets for application policies.</span></span></br></br>

#### <a name="add-azureadapplicationpolicy"></a><span data-ttu-id="235a5-446">Přidat AzureADApplicationPolicy</span><span class="sxs-lookup"><span data-stu-id="235a5-446">Add-AzureADApplicationPolicy</span></span>
<span data-ttu-id="235a5-447">Odkazy zadanou zásadu k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-447">Links the specified policy to an application.</span></span>

```PowerShell
Add-AzureADApplicationPolicy -Id <ObjectId of Application> -RefObjectId <ObjectId of Policy>
```

| <span data-ttu-id="235a5-448">Parametry</span><span class="sxs-lookup"><span data-stu-id="235a5-448">Parameters</span></span> | <span data-ttu-id="235a5-449">Popis</span><span class="sxs-lookup"><span data-stu-id="235a5-449">Description</span></span> | <span data-ttu-id="235a5-450">Příklad</span><span class="sxs-lookup"><span data-stu-id="235a5-450">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="235a5-451">**ObjectId (Id)** aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a5-451">**ObjectId (Id)** of the application.</span></span> | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |<span data-ttu-id="235a5-452">**ObjectId** zásad.</span><span class="sxs-lookup"><span data-stu-id="235a5-452">**ObjectId** of the policy.</span></span> | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadapplicationpolicy"></a><span data-ttu-id="235a5-453">Get-AzureADApplicationPolicy</span><span class="sxs-lookup"><span data-stu-id="235a5-453">Get-AzureADApplicationPolicy</span></span>
<span data-ttu-id="235a5-454">Získá zásady, která je přiřazena k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="235a5-454">Gets the policy that is assigned to an application.</span></span>

```PowerShell
Get-AzureADApplicationPolicy -Id <ObjectId of Application>
```

| <span data-ttu-id="235a5-455">Parametry</span><span class="sxs-lookup"><span data-stu-id="235a5-455">Parameters</span></span> | <span data-ttu-id="235a5-456">Popis</span><span class="sxs-lookup"><span data-stu-id="235a5-456">Description</span></span> | <span data-ttu-id="235a5-457">Příklad</span><span class="sxs-lookup"><span data-stu-id="235a5-457">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="235a5-458">**ObjectId (Id)** aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a5-458">**ObjectId (Id)** of the application.</span></span> | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadapplicationpolicy"></a><span data-ttu-id="235a5-459">Odebrat AzureADApplicationPolicy</span><span class="sxs-lookup"><span data-stu-id="235a5-459">Remove-AzureADApplicationPolicy</span></span>
<span data-ttu-id="235a5-460">Odebere zásadu z aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a5-460">Removes a policy from an application.</span></span>

```PowerShell
Remove-AzureADApplicationPolicy -Id <ObjectId of Application> -PolicyId <ObjectId of Policy>
```

| <span data-ttu-id="235a5-461">Parametry</span><span class="sxs-lookup"><span data-stu-id="235a5-461">Parameters</span></span> | <span data-ttu-id="235a5-462">Popis</span><span class="sxs-lookup"><span data-stu-id="235a5-462">Description</span></span> | <span data-ttu-id="235a5-463">Příklad</span><span class="sxs-lookup"><span data-stu-id="235a5-463">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="235a5-464">**ObjectId (Id)** aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a5-464">**ObjectId (Id)** of the application.</span></span> | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |<span data-ttu-id="235a5-465">**ObjectId** zásad.</span><span class="sxs-lookup"><span data-stu-id="235a5-465">**ObjectId** of the policy.</span></span> | `-PolicyId <ObjectId of Policy>` |

</br></br>

### <a name="service-principal-policies"></a><span data-ttu-id="235a5-466">Hlavní zásady služby</span><span class="sxs-lookup"><span data-stu-id="235a5-466">Service principal policies</span></span>
<span data-ttu-id="235a5-467">Následující rutiny můžete použít pro hlavní zásady služby.</span><span class="sxs-lookup"><span data-stu-id="235a5-467">You can use the following cmdlets for service principal policies.</span></span>

#### <a name="add-azureadserviceprincipalpolicy"></a><span data-ttu-id="235a5-468">Přidat AzureADServicePrincipalPolicy</span><span class="sxs-lookup"><span data-stu-id="235a5-468">Add-AzureADServicePrincipalPolicy</span></span>
<span data-ttu-id="235a5-469">Zadaná zásada odkazuje na objekt služby.</span><span class="sxs-lookup"><span data-stu-id="235a5-469">Links the specified policy to a service principal.</span></span>

```PowerShell
Add-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal> -RefObjectId <ObjectId of Policy>
```

| <span data-ttu-id="235a5-470">Parametry</span><span class="sxs-lookup"><span data-stu-id="235a5-470">Parameters</span></span> | <span data-ttu-id="235a5-471">Popis</span><span class="sxs-lookup"><span data-stu-id="235a5-471">Description</span></span> | <span data-ttu-id="235a5-472">Příklad</span><span class="sxs-lookup"><span data-stu-id="235a5-472">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="235a5-473">**ObjectId (Id)** aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a5-473">**ObjectId (Id)** of the application.</span></span> | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |<span data-ttu-id="235a5-474">**ObjectId** zásad.</span><span class="sxs-lookup"><span data-stu-id="235a5-474">**ObjectId** of the policy.</span></span> | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a><span data-ttu-id="235a5-475">Get-AzureADServicePrincipalPolicy</span><span class="sxs-lookup"><span data-stu-id="235a5-475">Get-AzureADServicePrincipalPolicy</span></span>
<span data-ttu-id="235a5-476">Získá všechny zásady pro propojený objekt zadaná služba.</span><span class="sxs-lookup"><span data-stu-id="235a5-476">Gets any policy linked to the specified service principal.</span></span>

```PowerShell
Get-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>
```

| <span data-ttu-id="235a5-477">Parametry</span><span class="sxs-lookup"><span data-stu-id="235a5-477">Parameters</span></span> | <span data-ttu-id="235a5-478">Popis</span><span class="sxs-lookup"><span data-stu-id="235a5-478">Description</span></span> | <span data-ttu-id="235a5-479">Příklad</span><span class="sxs-lookup"><span data-stu-id="235a5-479">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="235a5-480">**ObjectId (Id)** aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a5-480">**ObjectId (Id)** of the application.</span></span> | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a><span data-ttu-id="235a5-481">Odebrat AzureADServicePrincipalPolicy</span><span class="sxs-lookup"><span data-stu-id="235a5-481">Remove-AzureADServicePrincipalPolicy</span></span>
<span data-ttu-id="235a5-482">Odebere zásadu ze zadané instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="235a5-482">Removes the policy from the specified service principal.</span></span>

```PowerShell
Remove-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>  -PolicyId <ObjectId of Policy>
```

| <span data-ttu-id="235a5-483">Parametry</span><span class="sxs-lookup"><span data-stu-id="235a5-483">Parameters</span></span> | <span data-ttu-id="235a5-484">Popis</span><span class="sxs-lookup"><span data-stu-id="235a5-484">Description</span></span> | <span data-ttu-id="235a5-485">Příklad</span><span class="sxs-lookup"><span data-stu-id="235a5-485">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="235a5-486">**ObjectId (Id)** aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a5-486">**ObjectId (Id)** of the application.</span></span> | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |<span data-ttu-id="235a5-487">**ObjectId** zásad.</span><span class="sxs-lookup"><span data-stu-id="235a5-487">**ObjectId** of the policy.</span></span> | `-PolicyId <ObjectId of Policy>` |
