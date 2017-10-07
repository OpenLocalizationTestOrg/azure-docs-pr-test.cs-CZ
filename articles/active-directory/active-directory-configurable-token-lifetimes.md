---
title: "životnosti tokenu aaaConfigurable v Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooset životnosti pro tokeny vydané službou Azure AD."
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
ms.openlocfilehash: 0d4c8545981c5463cc7c95f669167bbc38230123
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configurable-token-lifetimes-in-azure-active-directory-public-preview"></a><span data-ttu-id="36ee0-103">Konfigurovat životnosti tokenu v Azure Active Directory (Public Preview)</span><span class="sxs-lookup"><span data-stu-id="36ee0-103">Configurable token lifetimes in Azure Active Directory (Public Preview)</span></span>
<span data-ttu-id="36ee0-104">Můžete zadat hello životnost tokenem vydaným službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="36ee0-104">You can specify hello lifetime of a token issued by Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="36ee0-105">Můžete nastavit životnosti tokenu pro všechny aplikace ve vaší organizaci, pro aplikaci víceklientské (více organizace) nebo pro objekt určité služby ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="36ee0-105">You can set token lifetimes for all apps in your organization, for a multi-tenant (multi-organization) application, or for a specific service principal in your organization.</span></span>

> [!NOTE]
> <span data-ttu-id="36ee0-106">Tato funkce je aktuálně ve verzi Public Preview.</span><span class="sxs-lookup"><span data-stu-id="36ee0-106">This capability currently is in Public Preview.</span></span> <span data-ttu-id="36ee0-107">Připravit toorevert nebo odeberte všechny změny.</span><span class="sxs-lookup"><span data-stu-id="36ee0-107">Be prepared toorevert or remove any changes.</span></span> <span data-ttu-id="36ee0-108">Hello funkce je dostupná v libovolné předplatné služby Azure Active Directory během verzi Public Preview.</span><span class="sxs-lookup"><span data-stu-id="36ee0-108">hello feature is available in any Azure Active Directory subscription during Public Preview.</span></span> <span data-ttu-id="36ee0-109">Ale když funkce hello je obecně dostupná, některé aspekty funkcí hello může vyžadovat [Azure Active Directory Premium](active-directory-get-started-premium.md) předplatné.</span><span class="sxs-lookup"><span data-stu-id="36ee0-109">However, when hello feature becomes generally available, some aspects of hello feature might require an [Azure Active Directory Premium](active-directory-get-started-premium.md) subscription.</span></span>
>
>

<span data-ttu-id="36ee0-110">Objekt zásady ve službě Azure AD, představuje sadu pravidel, které vynucuje u jednotlivých aplikací, nebo na všechny aplikace v organizaci.</span><span class="sxs-lookup"><span data-stu-id="36ee0-110">In Azure AD, a policy object represents a set of rules that are enforced on individual applications or on all applications in an organization.</span></span> <span data-ttu-id="36ee0-111">Každý typ zásad se strukturou jedinečnou sadu vlastností, které jsou použité tooobjects toowhich, které jsou přiřazeni.</span><span class="sxs-lookup"><span data-stu-id="36ee0-111">Each policy type has a unique structure, with a set of properties that are applied tooobjects toowhich they are assigned.</span></span>

<span data-ttu-id="36ee0-112">Zásady můžete určit jako hello výchozí zásady pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="36ee0-112">You can designate a policy as hello default policy for your organization.</span></span> <span data-ttu-id="36ee0-113">zásady Hello je použité tooany aplikace v organizaci hello, tak dlouho, dokud není přepsána zásady s vyšší prioritou.</span><span class="sxs-lookup"><span data-stu-id="36ee0-113">hello policy is applied tooany application in hello organization, as long as it is not overridden by a policy with a higher priority.</span></span> <span data-ttu-id="36ee0-114">Také můžete přiřadit zásady toospecific aplikace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-114">You also can assign a policy toospecific applications.</span></span> <span data-ttu-id="36ee0-115">Hello pořadí podle priority se liší podle typu zásady.</span><span class="sxs-lookup"><span data-stu-id="36ee0-115">hello order of priority varies by policy type.</span></span>


## <a name="token-types"></a><span data-ttu-id="36ee0-116">Typy tokenů</span><span class="sxs-lookup"><span data-stu-id="36ee0-116">Token types</span></span>

<span data-ttu-id="36ee0-117">Můžete nastavit dobu životnosti tokenu zásady pro tokeny obnovení, tokeny přístupu, tokeny relace a tokeny typu ID.</span><span class="sxs-lookup"><span data-stu-id="36ee0-117">You can set token lifetime policies for refresh tokens, access tokens, session tokens, and ID tokens.</span></span>

### <a name="access-tokens"></a><span data-ttu-id="36ee0-118">Přístupové tokeny</span><span class="sxs-lookup"><span data-stu-id="36ee0-118">Access tokens</span></span>
<span data-ttu-id="36ee0-119">Použití přístupu klientů tokeny tooaccess k chráněnému prostředku.</span><span class="sxs-lookup"><span data-stu-id="36ee0-119">Clients use access tokens tooaccess a protected resource.</span></span> <span data-ttu-id="36ee0-120">Přístupový token lze použít pouze pro konkrétní kombinaci uživatele, klienta a prostředků.</span><span class="sxs-lookup"><span data-stu-id="36ee0-120">An access token can be used only for a specific combination of user, client, and resource.</span></span> <span data-ttu-id="36ee0-121">Přístupové tokeny nejde odvolat a jsou platné až do vypršení jejich platnosti.</span><span class="sxs-lookup"><span data-stu-id="36ee0-121">Access tokens cannot be revoked and are valid until their expiry.</span></span> <span data-ttu-id="36ee0-122">Škodlivý objektu actor, který má získat token přístupu můžete použít pro rozsah celé jeho životnosti.</span><span class="sxs-lookup"><span data-stu-id="36ee0-122">A malicious actor that has obtained an access token can use it for extent of its lifetime.</span></span> <span data-ttu-id="36ee0-123">Nastavení doby platnosti hello přístupový token je kompromis mezi zlepšení výkonu systému a roste hello množství času, který hello klienta bude mít přístup po hello uživatelský účet je zakázán.</span><span class="sxs-lookup"><span data-stu-id="36ee0-123">Adjusting hello lifetime of an access token is a trade-off between improving system performance and increasing hello amount of time that hello client retains access after hello user’s account is disabled.</span></span> <span data-ttu-id="36ee0-124">Vylepšené systému výkonu se dosáhne snížením hello počet klient potřebuje tooacquire nový přístupový token.</span><span class="sxs-lookup"><span data-stu-id="36ee0-124">Improved system performance is achieved by reducing hello number of times a client needs tooacquire a fresh access token.</span></span>

### <a name="refresh-tokens"></a><span data-ttu-id="36ee0-125">Obnovovacích tokenů</span><span class="sxs-lookup"><span data-stu-id="36ee0-125">Refresh tokens</span></span>
<span data-ttu-id="36ee0-126">Když klient získá token tooaccess k přístupu k chráněnému prostředku, obdrží klient hello obnovovací token a přístupový token.</span><span class="sxs-lookup"><span data-stu-id="36ee0-126">When a client acquires an access token tooaccess a protected resource, hello client receives both a refresh token and an access token.</span></span> <span data-ttu-id="36ee0-127">token obnovení Hello je použité tooobtain nové přístupu nebo aktualizace tokenu páry když vyprší platnost hello aktuální přístupový token.</span><span class="sxs-lookup"><span data-stu-id="36ee0-127">hello refresh token is used tooobtain new access/refresh token pairs when hello current access token expires.</span></span> <span data-ttu-id="36ee0-128">Token obnovení je kombinací vázané tooa uživatele a klienta.</span><span class="sxs-lookup"><span data-stu-id="36ee0-128">A refresh token is bound tooa combination of user and client.</span></span> <span data-ttu-id="36ee0-129">Obnovovací token se dají odvolávat a kontroluje platnost tokenu hello pokaždé, když se používá hello token.</span><span class="sxs-lookup"><span data-stu-id="36ee0-129">A refresh token can be revoked, and hello token's validity is checked every time hello token is used.</span></span>

<span data-ttu-id="36ee0-130">Je důležité toomake rozdíl mezi důvěrné klienty a veřejné.</span><span class="sxs-lookup"><span data-stu-id="36ee0-130">It's important toomake a distinction between confidential clients and public clients.</span></span> <span data-ttu-id="36ee0-131">Další informace o různých typech klientů najdete v tématu [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).</span><span class="sxs-lookup"><span data-stu-id="36ee0-131">For more information about different types of clients, see [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).</span></span>

#### <a name="token-lifetimes-with-confidential-client-refresh-tokens"></a><span data-ttu-id="36ee0-132">Token životnosti s tokeny obnovení důvěrné klienta</span><span class="sxs-lookup"><span data-stu-id="36ee0-132">Token lifetimes with confidential client refresh tokens</span></span>
<span data-ttu-id="36ee0-133">Důvěrné klienti jsou aplikace, které můžete bezpečně uložit heslo klienta (tajný klíč).</span><span class="sxs-lookup"><span data-stu-id="36ee0-133">Confidential clients are applications that can securely store a client password (secret).</span></span> <span data-ttu-id="36ee0-134">Mohou prokázat, že požadavky přicházejí od aplikace hello klienta a nikoli z škodlivý objektu actor.</span><span class="sxs-lookup"><span data-stu-id="36ee0-134">They can prove that requests are coming from hello client application and not from a malicious actor.</span></span> <span data-ttu-id="36ee0-135">Například webová aplikace je důvěrné klienta, protože tajný klíč klienta může ukládat na webovém serveru hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-135">For example, a web app is a confidential client because it can store a client secret on hello web server.</span></span> <span data-ttu-id="36ee0-136">Nevystavené.</span><span class="sxs-lookup"><span data-stu-id="36ee0-136">It is not exposed.</span></span> <span data-ttu-id="36ee0-137">Protože tyto toky jsou bezpečnější, hello výchozí životnosti tokenů aktualizace vydané toothese toky je `until-revoked`, nelze změnit pomocí zásad a nebude na resetování hesla dobrovolná odvolán.</span><span class="sxs-lookup"><span data-stu-id="36ee0-137">Because these flows are more secure, hello default lifetimes of refresh tokens issued toothese flows is `until-revoked`, cannot be changed by using policy, and will not be revoked on voluntary password resets.</span></span>

#### <a name="token-lifetimes-with-public-client-refresh-tokens"></a><span data-ttu-id="36ee0-138">Token životnosti s tokeny obnovení veřejné klienta</span><span class="sxs-lookup"><span data-stu-id="36ee0-138">Token lifetimes with public client refresh tokens</span></span>

<span data-ttu-id="36ee0-139">Veřejné klientů nelze bezpečně uložit heslo klienta (tajný klíč).</span><span class="sxs-lookup"><span data-stu-id="36ee0-139">Public clients cannot securely store a client password (secret).</span></span> <span data-ttu-id="36ee0-140">Aplikace pro iOS nebo Android nelze například obfuskováním tajný klíč z hello vlastníka prostředku, tak bude považován za veřejné klienta.</span><span class="sxs-lookup"><span data-stu-id="36ee0-140">For example, an iOS/Android app cannot obfuscate a secret from hello resource owner, so it is considered a public client.</span></span> <span data-ttu-id="36ee0-141">Můžete nastavit zásady na prostředcích tooprevent tokeny obnovení z veřejné klientů starší než získat nový pár tokenu přístupu nebo aktualizaci během zadaného období.</span><span class="sxs-lookup"><span data-stu-id="36ee0-141">You can set policies on resources tooprevent refresh tokens from public clients older than a specified period from obtaining a new access/refresh token pair.</span></span> <span data-ttu-id="36ee0-142">(toodo hello tento, použijte vlastnost aktualizovat Token maximální neaktivní doba.) Můžete taky použít zásady tooset už jsou podmínky přijaty lhůtu, po které hello obnovovacích tokenů.</span><span class="sxs-lookup"><span data-stu-id="36ee0-142">(toodo this, use hello Refresh Token Max Inactive Time property.) You also can use policies tooset a period beyond which hello refresh tokens are no longer accepted.</span></span> <span data-ttu-id="36ee0-143">(toodo hello tento, použijte vlastnost aktualizovat Token maximální stáří.) Můžete upravit hello životnost aktualizace tokenu toocontrol, kdy a jak často hello je uživatel přihlašovací údaje požadované tooreenter místo se bezobslužně k novému ověření, při použití veřejných klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-143">(toodo this, use hello Refresh Token Max Age property.) You can adjust hello lifetime of a refresh token toocontrol when and how often hello user is required tooreenter credentials, instead of being silently reauthenticated, when using a public client application.</span></span>

### <a name="id-tokens"></a><span data-ttu-id="36ee0-144">ID tokeny</span><span class="sxs-lookup"><span data-stu-id="36ee0-144">ID tokens</span></span>
<span data-ttu-id="36ee0-145">ID tokeny jsou předávány toowebsites a nativních klientů.</span><span class="sxs-lookup"><span data-stu-id="36ee0-145">ID tokens are passed toowebsites and native clients.</span></span> <span data-ttu-id="36ee0-146">ID tokeny obsahovat profil informací o uživateli.</span><span class="sxs-lookup"><span data-stu-id="36ee0-146">ID tokens contain profile information about a user.</span></span> <span data-ttu-id="36ee0-147">ID token je vázané tooa konkrétní kombinaci uživatele a klienta.</span><span class="sxs-lookup"><span data-stu-id="36ee0-147">An ID token is bound tooa specific combination of user and client.</span></span> <span data-ttu-id="36ee0-148">ID tokeny považovány za platný až do vypršení jejich platnosti.</span><span class="sxs-lookup"><span data-stu-id="36ee0-148">ID tokens are considered valid until their expiry.</span></span> <span data-ttu-id="36ee0-149">Obvykle aplikace webového odpovídá uživatele pro uživatele hello vydané dobu platnosti relace v průběhu životnosti toohello aplikace hello hello ID tokenu.</span><span class="sxs-lookup"><span data-stu-id="36ee0-149">Usually, a web application matches a user’s session lifetime in hello application toohello lifetime of hello ID token issued for hello user.</span></span> <span data-ttu-id="36ee0-150">Můžete upravit hello životnost ID tokenu toocontrol jak často webové aplikace hello vyprší platnost relace hello aplikace a jak často vyžaduje toobe uživatele hello k novému ověření službou Azure AD (bezobslužná nebo interaktivní).</span><span class="sxs-lookup"><span data-stu-id="36ee0-150">You can adjust hello lifetime of an ID token toocontrol how often hello web application expires hello application session, and how often it requires hello user toobe reauthenticated with Azure AD (either silently or interactively).</span></span>

### <a name="single-sign-on-session-tokens"></a><span data-ttu-id="36ee0-151">Tokeny jedné relace přihlášení</span><span class="sxs-lookup"><span data-stu-id="36ee0-151">Single sign-on session tokens</span></span>
<span data-ttu-id="36ee0-152">Když uživatel ověřuje s Azure AD a vybere hello **zůstat přihlášeni** políčko relaci přihlašování (SSO) se naváže hello uživatele prohlížeče a Azure AD.</span><span class="sxs-lookup"><span data-stu-id="36ee0-152">When a user authenticates with Azure AD and selects hello **Keep me signed in** check box, a single sign-on session (SSO) is established with hello user’s browser and Azure AD.</span></span> <span data-ttu-id="36ee0-153">token Hello jednotné přihlašování, v hello formu souboru cookie, představuje tuto relaci.</span><span class="sxs-lookup"><span data-stu-id="36ee0-153">hello SSO token, in hello form of a cookie, represents this session.</span></span> <span data-ttu-id="36ee0-154">Poznámka: Tento token relace jednotného přihlašování k hello není vázané tooa konkrétní prostředků nebo klientská aplikace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-154">Note that hello SSO session token is not bound tooa specific resource/client application.</span></span> <span data-ttu-id="36ee0-155">Tokeny relace jednotného přihlašování se dají odvolávat a jejich platnost kontroluje pokaždé, když se používají.</span><span class="sxs-lookup"><span data-stu-id="36ee0-155">SSO session tokens can be revoked, and their validity is checked every time they are used.</span></span>

<span data-ttu-id="36ee0-156">Azure AD používá dva typy tokenů relace jednotného přihlašování: trvalé a zajišťováno.</span><span class="sxs-lookup"><span data-stu-id="36ee0-156">Azure AD uses two kinds of SSO session tokens: persistent and nonpersistent.</span></span> <span data-ttu-id="36ee0-157">Trvalé relace tokeny jsou uloženy jako trvalé soubory cookie prohlížeče hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-157">Persistent session tokens are stored as persistent cookies by hello browser.</span></span> <span data-ttu-id="36ee0-158">Tokeny zajišťováno relace jsou uloženy jako soubory cookie relace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-158">Nonpersistent session tokens are stored as session cookies.</span></span> <span data-ttu-id="36ee0-159">(Souborů cookie relací jsou při zavření prohlížeče hello zničen.)</span><span class="sxs-lookup"><span data-stu-id="36ee0-159">(Session cookies are destroyed when hello browser is closed.)</span></span>

<span data-ttu-id="36ee0-160">Zajišťováno relace tokeny mají životnost 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="36ee0-160">Nonpersistent session tokens have a lifetime of 24 hours.</span></span> <span data-ttu-id="36ee0-161">Trvalé tokeny mají životnost 180 dní.</span><span class="sxs-lookup"><span data-stu-id="36ee0-161">Persistent tokens have a lifetime of 180 days.</span></span> <span data-ttu-id="36ee0-162">Kdykoli tokenu jednotného přihlašování k relace se používá v rozsahu období platnosti, doba platnosti hello je rozšířeno jiný 24 hodin nebo 180 dnů, v závislosti na typu tokenu hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-162">Any time an SSO session token is used within its validity period, hello validity period is extended another 24 hours or 180 days, depending on hello token type.</span></span> <span data-ttu-id="36ee0-163">Pokud token relace jednotného přihlašování se nepoužívá v rozsahu období platnosti, bude považován za platnost a již byla přijata.</span><span class="sxs-lookup"><span data-stu-id="36ee0-163">If an SSO session token is not used within its validity period, it is considered expired and is no longer accepted.</span></span>

<span data-ttu-id="36ee0-164">Můžete použít zásady tooset hello čas po hello první relace token vydán nad rámec které hello token relace už přijata.</span><span class="sxs-lookup"><span data-stu-id="36ee0-164">You can use a policy tooset hello time after hello first session token was issued beyond which hello session token is no longer accepted.</span></span> <span data-ttu-id="36ee0-165">(toodo hello tento, použijte vlastnost relace tokenu maximální stáří.) Můžete upravit hello životnost relace tokenu toocontrol, kdy a jak často je uživatel přihlašovací údaje požadované tooreenter místo bezobslužně ověřovaného, pokud používáte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="36ee0-165">(toodo this, use hello Session Token Max Age property.) You can adjust hello lifetime of a session token toocontrol when and how often a user is required tooreenter credentials, instead of being silently authenticated, when using a web application.</span></span>

### <a name="token-lifetime-policy-properties"></a><span data-ttu-id="36ee0-166">Vlastnosti zásad životnost tokenu</span><span class="sxs-lookup"><span data-stu-id="36ee0-166">Token lifetime policy properties</span></span>
<span data-ttu-id="36ee0-167">Životnost tokenu zásad je typ objektu zásad, který obsahuje pravidla životnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="36ee0-167">A token lifetime policy is a type of policy object that contains token lifetime rules.</span></span> <span data-ttu-id="36ee0-168">Použití vlastnosti hello hello zásad toocontrol zadaný token životnosti.</span><span class="sxs-lookup"><span data-stu-id="36ee0-168">Use hello properties of hello policy toocontrol specified token lifetimes.</span></span> <span data-ttu-id="36ee0-169">Pokud je nastavené žádné zásady, systém hello vynucuje hodnotu doby života výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-169">If no policy is set, hello system enforces hello default lifetime value.</span></span>

### <a name="configurable-token-lifetime-properties"></a><span data-ttu-id="36ee0-170">Vlastnosti konfigurovat životnost tokenu</span><span class="sxs-lookup"><span data-stu-id="36ee0-170">Configurable token lifetime properties</span></span>
| <span data-ttu-id="36ee0-171">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="36ee0-171">Property</span></span> | <span data-ttu-id="36ee0-172">Řetězec vlastnosti zásad</span><span class="sxs-lookup"><span data-stu-id="36ee0-172">Policy property string</span></span> | <span data-ttu-id="36ee0-173">Ovlivňuje</span><span class="sxs-lookup"><span data-stu-id="36ee0-173">Affects</span></span> | <span data-ttu-id="36ee0-174">Výchozí</span><span class="sxs-lookup"><span data-stu-id="36ee0-174">Default</span></span> | <span data-ttu-id="36ee0-175">Minimální</span><span class="sxs-lookup"><span data-stu-id="36ee0-175">Minimum</span></span> | <span data-ttu-id="36ee0-176">Maximální počet</span><span class="sxs-lookup"><span data-stu-id="36ee0-176">Maximum</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="36ee0-177">Životnost tokenu přístupu</span><span class="sxs-lookup"><span data-stu-id="36ee0-177">Access Token Lifetime</span></span> |<span data-ttu-id="36ee0-178">AccessTokenLifetime</span><span class="sxs-lookup"><span data-stu-id="36ee0-178">AccessTokenLifetime</span></span> |<span data-ttu-id="36ee0-179">Přístupové tokeny, tokeny typu ID, typu SAML2 tokeny</span><span class="sxs-lookup"><span data-stu-id="36ee0-179">Access tokens, ID tokens, SAML2 tokens</span></span> |<span data-ttu-id="36ee0-180">1 hodina</span><span class="sxs-lookup"><span data-stu-id="36ee0-180">1 hour</span></span> |<span data-ttu-id="36ee0-181">10 minut</span><span class="sxs-lookup"><span data-stu-id="36ee0-181">10 minutes</span></span> |<span data-ttu-id="36ee0-182">1 den</span><span class="sxs-lookup"><span data-stu-id="36ee0-182">1 day</span></span> |
| <span data-ttu-id="36ee0-183">Aktualizace tokenu maximální doba neaktivní</span><span class="sxs-lookup"><span data-stu-id="36ee0-183">Refresh Token Max Inactive Time</span></span> |<span data-ttu-id="36ee0-184">MaxInactiveTime</span><span class="sxs-lookup"><span data-stu-id="36ee0-184">MaxInactiveTime</span></span> |<span data-ttu-id="36ee0-185">Obnovovacích tokenů</span><span class="sxs-lookup"><span data-stu-id="36ee0-185">Refresh tokens</span></span> |<span data-ttu-id="36ee0-186">14 dnů</span><span class="sxs-lookup"><span data-stu-id="36ee0-186">14 days</span></span> |<span data-ttu-id="36ee0-187">10 minut</span><span class="sxs-lookup"><span data-stu-id="36ee0-187">10 minutes</span></span> |<span data-ttu-id="36ee0-188">90 dnů</span><span class="sxs-lookup"><span data-stu-id="36ee0-188">90 days</span></span> |
| <span data-ttu-id="36ee0-189">Single-Factor aktualizace tokenu maximální stáří</span><span class="sxs-lookup"><span data-stu-id="36ee0-189">Single-Factor Refresh Token Max Age</span></span> |<span data-ttu-id="36ee0-190">MaxAgeSingleFactor</span><span class="sxs-lookup"><span data-stu-id="36ee0-190">MaxAgeSingleFactor</span></span> |<span data-ttu-id="36ee0-191">Obnovovacích tokenů (pro všechny uživatele)</span><span class="sxs-lookup"><span data-stu-id="36ee0-191">Refresh tokens (for any users)</span></span> |<span data-ttu-id="36ee0-192">Dokud odvolat</span><span class="sxs-lookup"><span data-stu-id="36ee0-192">Until-revoked</span></span> |<span data-ttu-id="36ee0-193">10 minut</span><span class="sxs-lookup"><span data-stu-id="36ee0-193">10 minutes</span></span> |<span data-ttu-id="36ee0-194">Dokud odvolat<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="36ee0-194">Until-revoked<sup>1</sup></span></span> |
| <span data-ttu-id="36ee0-195">Maximální stáří tokenu Multi-Factor aktualizace</span><span class="sxs-lookup"><span data-stu-id="36ee0-195">Multi-Factor Refresh Token Max Age</span></span> |<span data-ttu-id="36ee0-196">MaxAgeMultiFactor</span><span class="sxs-lookup"><span data-stu-id="36ee0-196">MaxAgeMultiFactor</span></span> |<span data-ttu-id="36ee0-197">Obnovovacích tokenů (pro všechny uživatele)</span><span class="sxs-lookup"><span data-stu-id="36ee0-197">Refresh tokens (for any users)</span></span> |<span data-ttu-id="36ee0-198">Dokud odvolat</span><span class="sxs-lookup"><span data-stu-id="36ee0-198">Until-revoked</span></span> |<span data-ttu-id="36ee0-199">10 minut</span><span class="sxs-lookup"><span data-stu-id="36ee0-199">10 minutes</span></span> |<span data-ttu-id="36ee0-200">Dokud odvolat<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="36ee0-200">Until-revoked<sup>1</sup></span></span> |
| <span data-ttu-id="36ee0-201">Maximální stáří tokenu Single-Factor relace</span><span class="sxs-lookup"><span data-stu-id="36ee0-201">Single-Factor Session Token Max Age</span></span> |<span data-ttu-id="36ee0-202">MaxAgeSessionSingleFactor<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="36ee0-202">MaxAgeSessionSingleFactor<sup>2</sup></span></span> |<span data-ttu-id="36ee0-203">Tokeny relace (trvalá nebo zajišťováno)</span><span class="sxs-lookup"><span data-stu-id="36ee0-203">Session tokens (persistent and nonpersistent)</span></span> |<span data-ttu-id="36ee0-204">Dokud odvolat</span><span class="sxs-lookup"><span data-stu-id="36ee0-204">Until-revoked</span></span> |<span data-ttu-id="36ee0-205">10 minut</span><span class="sxs-lookup"><span data-stu-id="36ee0-205">10 minutes</span></span> |<span data-ttu-id="36ee0-206">Dokud odvolat<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="36ee0-206">Until-revoked<sup>1</sup></span></span> |
| <span data-ttu-id="36ee0-207">Maximální stáří tokenu Multi-Factor relace</span><span class="sxs-lookup"><span data-stu-id="36ee0-207">Multi-Factor Session Token Max Age</span></span> |<span data-ttu-id="36ee0-208">MaxAgeSessionMultiFactor<sup>3</sup></span><span class="sxs-lookup"><span data-stu-id="36ee0-208">MaxAgeSessionMultiFactor<sup>3</sup></span></span> |<span data-ttu-id="36ee0-209">Tokeny relace (trvalá nebo zajišťováno)</span><span class="sxs-lookup"><span data-stu-id="36ee0-209">Session tokens (persistent and nonpersistent)</span></span> |<span data-ttu-id="36ee0-210">Dokud odvolat</span><span class="sxs-lookup"><span data-stu-id="36ee0-210">Until-revoked</span></span> |<span data-ttu-id="36ee0-211">10 minut</span><span class="sxs-lookup"><span data-stu-id="36ee0-211">10 minutes</span></span> |<span data-ttu-id="36ee0-212">Dokud odvolat<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="36ee0-212">Until-revoked<sup>1</sup></span></span> |

* <span data-ttu-id="36ee0-213"><sup>1</sup>365 dnů je hello maximální explicitní délku, která se dá nastavit pro tyto atributy.</span><span class="sxs-lookup"><span data-stu-id="36ee0-213"><sup>1</sup>365 days is hello maximum explicit length that can be set for these attributes.</span></span>
* <span data-ttu-id="36ee0-214"><sup>2</sup>Pokud **MaxAgeSessionSingleFactor** není nastaven, tato hodnota trvá hello **MaxAgeSingleFactor** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="36ee0-214"><sup>2</sup>If  **MaxAgeSessionSingleFactor** is not set, this value takes hello **MaxAgeSingleFactor** value.</span></span> <span data-ttu-id="36ee0-215">Pokud ani parametr je nastaven, vlastnost hello trvá hello výchozí hodnota (dokud odvolat).</span><span class="sxs-lookup"><span data-stu-id="36ee0-215">If neither parameter is set, hello property takes hello default value (until-revoked).</span></span>
* <span data-ttu-id="36ee0-216"><sup>3</sup>Pokud **MaxAgeSessionMultiFactor** není nastaven, tato hodnota trvá hello **MaxAgeMultiFactor** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="36ee0-216"><sup>3</sup>If  **MaxAgeSessionMultiFactor** is not set, this value takes hello **MaxAgeMultiFactor** value.</span></span> <span data-ttu-id="36ee0-217">Pokud ani parametr je nastaven, vlastnost hello trvá hello výchozí hodnota (dokud odvolat).</span><span class="sxs-lookup"><span data-stu-id="36ee0-217">If neither parameter is set, hello property takes hello default value (until-revoked).</span></span>

### <a name="exceptions"></a><span data-ttu-id="36ee0-218">Výjimky</span><span class="sxs-lookup"><span data-stu-id="36ee0-218">Exceptions</span></span>
| <span data-ttu-id="36ee0-219">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="36ee0-219">Property</span></span> | <span data-ttu-id="36ee0-220">Ovlivňuje</span><span class="sxs-lookup"><span data-stu-id="36ee0-220">Affects</span></span> | <span data-ttu-id="36ee0-221">Výchozí</span><span class="sxs-lookup"><span data-stu-id="36ee0-221">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="36ee0-222">Aktualizace tokenu maximální stáří (vydán pro federované uživatele, kteří mají dostatečná odvolání informace<sup>1</sup>)</span><span class="sxs-lookup"><span data-stu-id="36ee0-222">Refresh Token Max Age (issued for federated users who have insufficient revocation information<sup>1</sup>)</span></span> |<span data-ttu-id="36ee0-223">Obnovovacích tokenů (vydán pro federované uživatele, kteří mají dostatečná odvolání informace<sup>1</sup>)</span><span class="sxs-lookup"><span data-stu-id="36ee0-223">Refresh tokens (issued for federated users who have insufficient revocation information<sup>1</sup>)</span></span> |<span data-ttu-id="36ee0-224">12 hodin</span><span class="sxs-lookup"><span data-stu-id="36ee0-224">12 hours</span></span> |
| <span data-ttu-id="36ee0-225">Aktualizace tokenu neaktivní doba Max (vydán pro důvěrné klientů)</span><span class="sxs-lookup"><span data-stu-id="36ee0-225">Refresh Token Max Inactive Time (issued for confidential clients)</span></span> |<span data-ttu-id="36ee0-226">Obnovovacích tokenů (vydán pro důvěrné klientů)</span><span class="sxs-lookup"><span data-stu-id="36ee0-226">Refresh tokens (issued for confidential clients)</span></span> |<span data-ttu-id="36ee0-227">90 dnů</span><span class="sxs-lookup"><span data-stu-id="36ee0-227">90 days</span></span> |
| <span data-ttu-id="36ee0-228">Aktualizace tokenu maximální stáří (vydán pro důvěrné klientů)</span><span class="sxs-lookup"><span data-stu-id="36ee0-228">Refresh Token Max Age (issued for confidential clients)</span></span> |<span data-ttu-id="36ee0-229">Obnovovacích tokenů (vydán pro důvěrné klientů)</span><span class="sxs-lookup"><span data-stu-id="36ee0-229">Refresh tokens (issued for confidential clients)</span></span> |<span data-ttu-id="36ee0-230">Dokud odvolat</span><span class="sxs-lookup"><span data-stu-id="36ee0-230">Until-revoked</span></span> |

* <span data-ttu-id="36ee0-231"><sup>1</sup>federovaný uživatelé, kteří mají dostatečná odvolání informace zahrnují všechny uživatele, kteří nemají hello "LastPasswordChangeTimestamp" atribut synchronizované.</span><span class="sxs-lookup"><span data-stu-id="36ee0-231"><sup>1</sup>Federated users who have insufficient revocation information include any users who do not have hello "LastPasswordChangeTimestamp" attribute synced.</span></span> <span data-ttu-id="36ee0-232">Tito uživatelé mají tato krátké maximální stáří AAD je proto nelze tooverify Pokud toorevoke tokeny, které jsou svázané tooan starý přihlašovací údaje (například hesla, která byla změněna) a musí se změnami zpět častěji tooensure tento uživatel hello a přidružené tokeny jsou stále v dobré stojící.</span><span class="sxs-lookup"><span data-stu-id="36ee0-232">These users are given this short Max Age because AAD is unable tooverify when toorevoke tokens that are tied tooan old credential (such as a password that has been changed) and must check back in more frequently tooensure that hello user and associated tokens are still in good standing.</span></span> <span data-ttu-id="36ee0-233">tooimprove toto prostředí klienta, které admins musíte zajistit, že se synchronizuje hello "LastPasswordChangeTimestamp" atribut (to je možné nastavit u objektu uživatele hello pomocí prostředí Powershell nebo prostřednictvím nebude služba AADSync).</span><span class="sxs-lookup"><span data-stu-id="36ee0-233">tooimprove this experience, tenant admins must ensure that they are syncing hello “LastPasswordChangeTimestamp” attribute (this can be set on hello user object using Powershell or through AADSync).</span></span>

### <a name="policy-evaluation-and-prioritization"></a><span data-ttu-id="36ee0-234">Vyhodnocení zásad a stanovení priorit</span><span class="sxs-lookup"><span data-stu-id="36ee0-234">Policy evaluation and prioritization</span></span>
<span data-ttu-id="36ee0-235">Můžete vytvořit a potom přiřadit konkrétní aplikaci životnost tokenu zásad tooa, tooyour organizace a tooservice objekty.</span><span class="sxs-lookup"><span data-stu-id="36ee0-235">You can create and then assign a token lifetime policy tooa specific application, tooyour organization, and tooservice principals.</span></span> <span data-ttu-id="36ee0-236">Víc zásad může použít tooa konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="36ee0-236">Multiple policies might apply tooa specific application.</span></span> <span data-ttu-id="36ee0-237">Hello životnost tokenu zásady, které se projeví řídí následujícími pravidly:</span><span class="sxs-lookup"><span data-stu-id="36ee0-237">hello token lifetime policy that takes effect follows these rules:</span></span>

* <span data-ttu-id="36ee0-238">Pokud zásadu explicitně přiřazený toohello instančního objektu, se vynutí.</span><span class="sxs-lookup"><span data-stu-id="36ee0-238">If a policy is explicitly assigned toohello service principal, it is enforced.</span></span>
* <span data-ttu-id="36ee0-239">Pokud žádné zásady jsou explicitně přiřazené toohello instanční objekt, je vyžadována zásadu explicitně přiřazeny toohello nadřazené organizace hello instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="36ee0-239">If no policy is explicitly assigned toohello service principal, a policy explicitly assigned toohello parent organization of hello service principal is enforced.</span></span>
* <span data-ttu-id="36ee0-240">Pokud žádné zásady explicitně přiřazeny toohello instanční objekt nebo toohello organizace, je vyžadována hello zásady přiřazené toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-240">If no policy is explicitly assigned toohello service principal or toohello organization, hello policy assigned toohello application is enforced.</span></span>
* <span data-ttu-id="36ee0-241">Pokud žádná zásada byla přiřazena toohello služby objekt zabezpečení, hello organizaci nebo objekt aplikace hello, hello výchozí hodnoty je vyžadována.</span><span class="sxs-lookup"><span data-stu-id="36ee0-241">If no policy has been assigned toohello service principal, hello organization, or hello application object, hello default values is enforced.</span></span> <span data-ttu-id="36ee0-242">(Viz tabulka hello v [konfigurovat životnost tokenu vlastnosti](#configurable-token-lifetime-properties).)</span><span class="sxs-lookup"><span data-stu-id="36ee0-242">(See hello table in [Configurable token lifetime properties](#configurable-token-lifetime-properties).)</span></span>

<span data-ttu-id="36ee0-243">Další informace o hello vztah mezi objekty aplikací a hlavní objekty služby najdete v tématu [aplikace a služby hlavní objekty ve službě Azure Active Directory](active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="36ee0-243">For more information about hello relationship between application objects and service principal objects, see [Application and service principal objects in Azure Active Directory](active-directory-application-objects.md).</span></span>

<span data-ttu-id="36ee0-244">Token platnosti vyhodnotí ve hello dobu, kdy se používá hello token.</span><span class="sxs-lookup"><span data-stu-id="36ee0-244">A token’s validity is evaluated at hello time hello token is used.</span></span> <span data-ttu-id="36ee0-245">Hello zásada s nejvyšší prioritou hello na hello aplikace, která je přistupuje projeví.</span><span class="sxs-lookup"><span data-stu-id="36ee0-245">hello policy with hello highest priority on hello application that is being accessed takes effect.</span></span>

> [!NOTE]
> <span data-ttu-id="36ee0-246">Tady je příklad scénáře.</span><span class="sxs-lookup"><span data-stu-id="36ee0-246">Here's an example scenario.</span></span>
>
> <span data-ttu-id="36ee0-247">Uživatel chce tooaccess dva webové aplikace: webové aplikace A a B. webové aplikace</span><span class="sxs-lookup"><span data-stu-id="36ee0-247">A user wants tooaccess two web applications: Web Application A and Web Application B.</span></span>
> 
> <span data-ttu-id="36ee0-248">Faktory:</span><span class="sxs-lookup"><span data-stu-id="36ee0-248">Factors:</span></span>
> * <span data-ttu-id="36ee0-249">Obě webové aplikace jsou v hello stejné nadřazené organizace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-249">Both web applications are in hello same parent organization.</span></span>
> * <span data-ttu-id="36ee0-250">Token 1 zásad životního cyklu relace tokenu maximální stáří osm hodin je nastaven jako výchozí hello nadřazené organizace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-250">Token Lifetime Policy 1 with a Session Token Max Age of eight hours is set as hello parent organization’s default.</span></span>
> * <span data-ttu-id="36ee0-251">Webové aplikace A je použití regular webovou aplikaci a není propojené tooany zásady.</span><span class="sxs-lookup"><span data-stu-id="36ee0-251">Web Application A is a regular-use web application and isn’t linked tooany policies.</span></span>
> * <span data-ttu-id="36ee0-252">Webovou aplikaci B se používá pro vysoce citlivých procesů.</span><span class="sxs-lookup"><span data-stu-id="36ee0-252">Web Application B is used for highly sensitive processes.</span></span> <span data-ttu-id="36ee0-253">Jeho instančního objektu je propojené tooToken 2 zásad životního cyklu, který má relace tokenu maximální stáří 30 minut.</span><span class="sxs-lookup"><span data-stu-id="36ee0-253">Its service principal is linked tooToken Lifetime Policy 2, which has a Session Token Max Age of 30 minutes.</span></span>
>
> <span data-ttu-id="36ee0-254">Na 12:00 PM hello uživatel spustí novou relaci prohlížeče a pokusů tooaccess A. webové aplikace hello uživatel přesměrovaného tooAzure AD a se zobrazí výzva, toosign v.</span><span class="sxs-lookup"><span data-stu-id="36ee0-254">At 12:00 PM, hello user starts a new browser session and tries tooaccess Web Application A. hello user is redirected tooAzure AD and is asked toosign in.</span></span> <span data-ttu-id="36ee0-255">Tím se vytvoří soubor cookie, který obsahuje token relace v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-255">This creates a cookie that has a session token in hello browser.</span></span> <span data-ttu-id="36ee0-256">uživatel Hello je back tooWeb přesměrovaného aplikace A k tokenu ID, který hello uživatele tooaccess hello aplikacím.</span><span class="sxs-lookup"><span data-stu-id="36ee0-256">hello user is redirected back tooWeb Application A with an ID token that allows hello user tooaccess hello application.</span></span>
>
> <span data-ttu-id="36ee0-257">Ve 12:15 hello uživatel pokusí tooaccess B. webové aplikace hello prohlížeče přesměrování tooAzure AD, který zjistí hello souboru cookie relace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-257">At 12:15 PM, hello user tries tooaccess Web Application B. hello browser redirects tooAzure AD, which detects hello session cookie.</span></span> <span data-ttu-id="36ee0-258">Objekt B aplikace webové služby je propojené tooToken 2 zásad životního cyklu, ale je také součástí hello nadřazené organizace, výchozí Token 1 zásad životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="36ee0-258">Web Application B’s service principal is linked tooToken Lifetime Policy 2, but it's also part of hello parent organization, with default Token Lifetime Policy 1.</span></span> <span data-ttu-id="36ee0-259">Token 2 zásad životního cyklu využívá vliv, protože zásady tooservice propojené objekty mají vyšší prioritu než výchozí zásady organizace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-259">Token Lifetime Policy 2 takes effect because policies linked tooservice principals have a higher priority than organization default policies.</span></span> <span data-ttu-id="36ee0-260">Hello relace token původně vydán v rámci hello posledních 30 minut, takže bude považován za platný.</span><span class="sxs-lookup"><span data-stu-id="36ee0-260">hello session token was originally issued within hello last 30 minutes, so it is considered valid.</span></span> <span data-ttu-id="36ee0-261">uživatel Hello je back tooWeb přesměrovaného aplikaci B k tokenu ID, která jim udělí přístup.</span><span class="sxs-lookup"><span data-stu-id="36ee0-261">hello user is redirected back tooWeb Application B with an ID token that grants them access.</span></span>
>
> <span data-ttu-id="36ee0-262">: 00: 00 je hello pokusů tooaccess A. webové aplikace hello uživatel přesměrovaného tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="36ee0-262">At 1:00 PM, hello user tries tooaccess Web Application A. hello user is redirected tooAzure AD.</span></span> <span data-ttu-id="36ee0-263">Webové aplikace A není propojené tooany zásady, ale protože je v organizaci s výchozí Token 1 zásad životního cyklu, tyto zásady projeví.</span><span class="sxs-lookup"><span data-stu-id="36ee0-263">Web Application A is not linked tooany policies, but because it is in an organization with default Token Lifetime Policy 1, that policy takes effect.</span></span> <span data-ttu-id="36ee0-264">je zjištěna Hello souboru cookie relace, který byl původně vystavil v rámci hello posledních 8 hodin.</span><span class="sxs-lookup"><span data-stu-id="36ee0-264">hello session cookie that was originally issued within hello last eight hours is detected.</span></span> <span data-ttu-id="36ee0-265">uživatel Hello je back tooWeb bezobslužně přesměrovaného aplikace A s ID nový token.</span><span class="sxs-lookup"><span data-stu-id="36ee0-265">hello user is silently redirected back tooWeb Application A with a new ID token.</span></span> <span data-ttu-id="36ee0-266">uživatel Hello není požadovaná tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="36ee0-266">hello user is not required tooauthenticate.</span></span>
>
> <span data-ttu-id="36ee0-267">Hned potom hello uživatel pokusí tooaccess B. webové aplikace hello uživatel je přesměrovaného tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="36ee0-267">Immediately afterward, hello user tries tooaccess Web Application B. hello user is redirected tooAzure AD.</span></span> <span data-ttu-id="36ee0-268">Jako dříve, tokenu 2 životnost zásad se projeví.</span><span class="sxs-lookup"><span data-stu-id="36ee0-268">As before, Token Lifetime Policy 2 takes effect.</span></span> <span data-ttu-id="36ee0-269">Vzhledem k tomu hello token vydán víc než 30 minutami, hello uživatele je výzvami tooreenter jejich přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="36ee0-269">Because hello token was issued more than 30 minutes ago, hello user is prompted tooreenter their sign-in credentials.</span></span> <span data-ttu-id="36ee0-270">Jsou vydávány ID token a relace zcela nový token.</span><span class="sxs-lookup"><span data-stu-id="36ee0-270">A brand-new session token and ID token are issued.</span></span> <span data-ttu-id="36ee0-271">uživatel Hello přístup B. webové aplikace</span><span class="sxs-lookup"><span data-stu-id="36ee0-271">hello user can then access Web Application B.</span></span>
>
>

## <a name="configurable-policy-property-details"></a><span data-ttu-id="36ee0-272">Podrobnosti vlastnost konfigurovat zásady</span><span class="sxs-lookup"><span data-stu-id="36ee0-272">Configurable policy property details</span></span>
### <a name="access-token-lifetime"></a><span data-ttu-id="36ee0-273">Životnost tokenu přístupu</span><span class="sxs-lookup"><span data-stu-id="36ee0-273">Access Token Lifetime</span></span>
<span data-ttu-id="36ee0-274">**Řetězec:** AccessTokenLifetime</span><span class="sxs-lookup"><span data-stu-id="36ee0-274">**String:** AccessTokenLifetime</span></span>

<span data-ttu-id="36ee0-275">**Ovlivňuje:** přístupové tokeny, tokeny typu ID</span><span class="sxs-lookup"><span data-stu-id="36ee0-275">**Affects:** Access tokens, ID tokens</span></span>

<span data-ttu-id="36ee0-276">**Souhrn:** tato zásada určuje, jak dlouho přístup a tokeny typu ID pro tento prostředek jsou považovány za platné.</span><span class="sxs-lookup"><span data-stu-id="36ee0-276">**Summary:** This policy controls how long access and ID tokens for this resource are considered valid.</span></span> <span data-ttu-id="36ee0-277">Snižuje dobu životnosti tokenu přístupu vlastnost hello snižuje riziko hello přístupového tokenu nebo ID token používá škodlivý objektu actor pro delší dobu.</span><span class="sxs-lookup"><span data-stu-id="36ee0-277">Reducing hello Access Token Lifetime property mitigates hello risk of an access token or ID token being used by a malicious actor for an extended period of time.</span></span> <span data-ttu-id="36ee0-278">(Tyto tokeny nelze odvolat.) kompromis Hello je nepříznivě ovlivňuje výkon, protože hello tokeny toobe nahradit častěji.</span><span class="sxs-lookup"><span data-stu-id="36ee0-278">(These tokens cannot be revoked.) hello trade-off is that performance is adversely affected, because hello tokens have toobe replaced more often.</span></span>

### <a name="refresh-token-max-inactive-time"></a><span data-ttu-id="36ee0-279">Aktualizace tokenu maximální doba neaktivní</span><span class="sxs-lookup"><span data-stu-id="36ee0-279">Refresh Token Max Inactive Time</span></span>
<span data-ttu-id="36ee0-280">**Řetězec:** MaxInactiveTime</span><span class="sxs-lookup"><span data-stu-id="36ee0-280">**String:** MaxInactiveTime</span></span>

<span data-ttu-id="36ee0-281">**Ovlivňuje:** obnovovacích tokenů</span><span class="sxs-lookup"><span data-stu-id="36ee0-281">**Affects:** Refresh tokens</span></span>

<span data-ttu-id="36ee0-282">**Souhrn:** tato zásada určuje, kolik token obnovení může být, než klient již nelze používat ji tooretrieve nový pár tokenu přístupu nebo aktualizace při pokusu o tooaccess tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="36ee0-282">**Summary:** This policy controls how old a refresh token can be before a client can no longer use it tooretrieve a new access/refresh token pair when attempting tooaccess this resource.</span></span> <span data-ttu-id="36ee0-283">Vzhledem k tomu, že nový token obnovení je obvykle vrácena, pokud se používá token obnovení, zabraňuje tato zásada přístupu, pokud klient hello pokusí tooaccess jakémukoli prostředku, pomocí tokenu obnovení aktuální hello během hello zadané časové období.</span><span class="sxs-lookup"><span data-stu-id="36ee0-283">Because a new refresh token usually is returned when a refresh token is used, this policy prevents access if hello client tries tooaccess any resource by using hello current refresh token during hello specified period of time.</span></span>

<span data-ttu-id="36ee0-284">Tato zásada vynutí uživatelů, kteří nebyly aktivní na jejich klienta tooreauthenticate tooretrieve nový token obnovení.</span><span class="sxs-lookup"><span data-stu-id="36ee0-284">This policy forces users who have not been active on their client tooreauthenticate tooretrieve a new refresh token.</span></span>

<span data-ttu-id="36ee0-285">Hello vlastnost aktualizovat Token maximální neaktivní doba musí být nastavena tooa nižší hodnotu než hello Single-Factor tokenu maximální stáří a vlastnosti Multi-Factor aktualizovat Token maximální stáří hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-285">hello Refresh Token Max Inactive Time property must be set tooa lower value than hello Single-Factor Token Max Age and hello Multi-Factor Refresh Token Max Age properties.</span></span>

### <a name="single-factor-refresh-token-max-age"></a><span data-ttu-id="36ee0-286">Single-Factor aktualizace tokenu maximální stáří</span><span class="sxs-lookup"><span data-stu-id="36ee0-286">Single-Factor Refresh Token Max Age</span></span>
<span data-ttu-id="36ee0-287">**Řetězec:** MaxAgeSingleFactor</span><span class="sxs-lookup"><span data-stu-id="36ee0-287">**String:** MaxAgeSingleFactor</span></span>

<span data-ttu-id="36ee0-288">**Ovlivňuje:** obnovovacích tokenů</span><span class="sxs-lookup"><span data-stu-id="36ee0-288">**Affects:** Refresh tokens</span></span>

<span data-ttu-id="36ee0-289">**Souhrn:** tento ovládací prvky zásad jak dlouho uživatele můžete použít obnovení tokenu tooget a nové přístupu nebo aktualizace tokenu pár po jejich poslední úspěšně ověření pomocí pouze jeden faktor.</span><span class="sxs-lookup"><span data-stu-id="36ee0-289">**Summary:** This policy controls how long a user can use a refresh token tooget a new access/refresh token pair after they last authenticated successfully by using only a single factor.</span></span> <span data-ttu-id="36ee0-290">Poté, co uživatel ověří a obdrží token nové aktualizace, hello uživatel může použít tok tokenu hello aktualizace pro hello zadané období času.</span><span class="sxs-lookup"><span data-stu-id="36ee0-290">After a user authenticates and receives a new refresh token, hello user can use hello refresh token flow for hello specified period of time.</span></span> <span data-ttu-id="36ee0-291">(To je hodnota true, dokud nebude odvolaný hello aktuální obnovovací token a není zbývající nepoužité po dobu delší než hello neaktivní doba.) V tomto okamžiku je uživatel hello vynucené tooreauthenticate tooreceive nový token obnovení.</span><span class="sxs-lookup"><span data-stu-id="36ee0-291">(This is true as long as hello current refresh token is not revoked, and it is not left unused for longer than hello inactive time.) At that point, hello user is forced tooreauthenticate tooreceive a new refresh token.</span></span>

<span data-ttu-id="36ee0-292">Uživatelé tooauthenticate snížit maximální stáří hello vynutí častěji.</span><span class="sxs-lookup"><span data-stu-id="36ee0-292">Reducing hello max age forces users tooauthenticate more often.</span></span> <span data-ttu-id="36ee0-293">Protože jedním Multi-Factor authentication je považován za méně bezpečné než vícefaktorového ověřování, doporučujeme nastavit tuto vlastnost tooa hodnotu menší než hello Multi-Factor aktualizovat Token maximální stáří vlastnost tedy rovna tooor.</span><span class="sxs-lookup"><span data-stu-id="36ee0-293">Because single-factor authentication is considered less secure than multi-factor authentication, we recommend that you set this property tooa value that is equal tooor lesser than hello Multi-Factor Refresh Token Max Age property.</span></span>

### <a name="multi-factor-refresh-token-max-age"></a><span data-ttu-id="36ee0-294">Maximální stáří tokenu Multi-Factor aktualizace</span><span class="sxs-lookup"><span data-stu-id="36ee0-294">Multi-Factor Refresh Token Max Age</span></span>
<span data-ttu-id="36ee0-295">**Řetězec:** MaxAgeMultiFactor</span><span class="sxs-lookup"><span data-stu-id="36ee0-295">**String:** MaxAgeMultiFactor</span></span>

<span data-ttu-id="36ee0-296">**Ovlivňuje:** obnovovacích tokenů</span><span class="sxs-lookup"><span data-stu-id="36ee0-296">**Affects:** Refresh tokens</span></span>

<span data-ttu-id="36ee0-297">**Souhrn:** tento ovládací prvky zásad jak dlouho uživatele můžete použít obnovení tokenu tooget a nové přístupu nebo aktualizace tokenu pár po jejich poslední úspěšně ověření pomocí několika faktory.</span><span class="sxs-lookup"><span data-stu-id="36ee0-297">**Summary:** This policy controls how long a user can use a refresh token tooget a new access/refresh token pair after they last authenticated successfully by using multiple factors.</span></span> <span data-ttu-id="36ee0-298">Poté, co uživatel ověří a obdrží token nové aktualizace, hello uživatel může použít tok tokenu hello aktualizace pro hello zadané období času.</span><span class="sxs-lookup"><span data-stu-id="36ee0-298">After a user authenticates and receives a new refresh token, hello user can use hello refresh token flow for hello specified period of time.</span></span> <span data-ttu-id="36ee0-299">(To je hodnota true, dokud nebude odvolaný hello aktuální obnovovací token a není po dobu delší než hello neaktivní doba.) V tomto okamžiku jsou uživatelé přinucení tooreauthenticate tooreceive nový token obnovení.</span><span class="sxs-lookup"><span data-stu-id="36ee0-299">(This is true as long as hello current refresh token is not revoked, and it is not unused for longer than hello inactive time.) At that point, users are forced tooreauthenticate tooreceive a new refresh token.</span></span>

<span data-ttu-id="36ee0-300">Uživatelé tooauthenticate snížit maximální stáří hello vynutí častěji.</span><span class="sxs-lookup"><span data-stu-id="36ee0-300">Reducing hello max age forces users tooauthenticate more often.</span></span> <span data-ttu-id="36ee0-301">Protože jedním Multi-Factor authentication je považován za méně bezpečné než vícefaktorového ověřování, doporučujeme nastavit tuto vlastnost tooa hodnotu, která je rovna tooor větší než hello Single-Factor aktualizovat Token maximální stáří vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="36ee0-301">Because single-factor authentication is considered less secure than multi-factor authentication, we recommend that you set this property tooa value that is equal tooor greater than hello Single-Factor Refresh Token Max Age property.</span></span>

### <a name="single-factor-session-token-max-age"></a><span data-ttu-id="36ee0-302">Maximální stáří tokenu Single-Factor relace</span><span class="sxs-lookup"><span data-stu-id="36ee0-302">Single-Factor Session Token Max Age</span></span>
<span data-ttu-id="36ee0-303">**Řetězec:** MaxAgeSessionSingleFactor</span><span class="sxs-lookup"><span data-stu-id="36ee0-303">**String:** MaxAgeSessionSingleFactor</span></span>

<span data-ttu-id="36ee0-304">**Ovlivňuje:** tokeny relace (trvalá nebo zajišťováno)</span><span class="sxs-lookup"><span data-stu-id="36ee0-304">**Affects:** Session tokens (persistent and nonpersistent)</span></span>

<span data-ttu-id="36ee0-305">**Souhrn:** této zásady ovládací prvky jak dlouho může uživatel používat relace tokenu tooget nové ID a relace token po jejich poslední úspěšně ověření pomocí pouze jeden faktor.</span><span class="sxs-lookup"><span data-stu-id="36ee0-305">**Summary:** This policy controls how long a user can use a session token tooget a new ID and session token after they last authenticated successfully by using only a single factor.</span></span> <span data-ttu-id="36ee0-306">Poté, co uživatel ověří a obdrží token novou relaci, hello uživatel může použít tok tokenu hello relace pro hello zadané období času.</span><span class="sxs-lookup"><span data-stu-id="36ee0-306">After a user authenticates and receives a new session token, hello user can use hello session token flow for hello specified period of time.</span></span> <span data-ttu-id="36ee0-307">(To platí, dokud, hello aktuální relace token není odvolaný a jestli nevypršela platnost.) Po hello zadané časové období, uživatel hello je tooreceive vynucené tooreauthenticate token novou relaci.</span><span class="sxs-lookup"><span data-stu-id="36ee0-307">(This is true as long as hello current session token is not revoked and has not expired.) After hello specified period of time, hello user is forced tooreauthenticate tooreceive a new session token.</span></span>

<span data-ttu-id="36ee0-308">Uživatelé tooauthenticate snížit maximální stáří hello vynutí častěji.</span><span class="sxs-lookup"><span data-stu-id="36ee0-308">Reducing hello max age forces users tooauthenticate more often.</span></span> <span data-ttu-id="36ee0-309">Protože jedním Multi-Factor authentication je považován za méně bezpečné než vícefaktorového ověřování, doporučujeme nastavit tuto vlastnost tooa hodnotu, která je rovna tooor menší než hello Multi-Factor relace tokenu maximální stáří vlastnost.</span><span class="sxs-lookup"><span data-stu-id="36ee0-309">Because single-factor authentication is considered less secure than multi-factor authentication, we recommend that you set this property tooa value that is equal tooor less than hello Multi-Factor Session Token Max Age property.</span></span>

### <a name="multi-factor-session-token-max-age"></a><span data-ttu-id="36ee0-310">Maximální stáří tokenu Multi-Factor relace</span><span class="sxs-lookup"><span data-stu-id="36ee0-310">Multi-Factor Session Token Max Age</span></span>
<span data-ttu-id="36ee0-311">**Řetězec:** MaxAgeSessionMultiFactor</span><span class="sxs-lookup"><span data-stu-id="36ee0-311">**String:** MaxAgeSessionMultiFactor</span></span>

<span data-ttu-id="36ee0-312">**Ovlivňuje:** tokeny relace (trvalá nebo zajišťováno)</span><span class="sxs-lookup"><span data-stu-id="36ee0-312">**Affects:** Session tokens (persistent and nonpersistent)</span></span>

<span data-ttu-id="36ee0-313">**Souhrn:** této zásady ovládací prvky jak dlouho může uživatel používat relace tokenu tooget nové ID a relace token po hello naposledy musí úspěšně ověřit pomocí několika faktory.</span><span class="sxs-lookup"><span data-stu-id="36ee0-313">**Summary:** This policy controls how long a user can use a session token tooget a new ID and session token after hello last time they authenticated successfully by using multiple factors.</span></span> <span data-ttu-id="36ee0-314">Poté, co uživatel ověří a obdrží token novou relaci, hello uživatel může použít tok tokenu hello relace pro hello zadané období času.</span><span class="sxs-lookup"><span data-stu-id="36ee0-314">After a user authenticates and receives a new session token, hello user can use hello session token flow for hello specified period of time.</span></span> <span data-ttu-id="36ee0-315">(To platí, dokud, hello aktuální relace token není odvolaný a jestli nevypršela platnost.) Po hello zadané časové období, uživatel hello je tooreceive vynucené tooreauthenticate token novou relaci.</span><span class="sxs-lookup"><span data-stu-id="36ee0-315">(This is true as long as hello current session token is not revoked and has not expired.) After hello specified period of time, hello user is forced tooreauthenticate tooreceive a new session token.</span></span>

<span data-ttu-id="36ee0-316">Uživatelé tooauthenticate snížit maximální stáří hello vynutí častěji.</span><span class="sxs-lookup"><span data-stu-id="36ee0-316">Reducing hello max age forces users tooauthenticate more often.</span></span> <span data-ttu-id="36ee0-317">Protože jedním Multi-Factor authentication je považován za méně bezpečné než vícefaktorového ověřování, doporučujeme nastavit tuto vlastnost tooa hodnotu, která je rovna tooor větší než hello Single-Factor relace tokenu maximální stáří vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="36ee0-317">Because single-factor authentication is considered less secure than multi-factor authentication, we recommend that you set this property tooa value that is equal tooor greater than hello Single-Factor Session Token Max Age property.</span></span>

## <a name="example-token-lifetime-policies"></a><span data-ttu-id="36ee0-318">Příkladem zásad životnost tokenu</span><span class="sxs-lookup"><span data-stu-id="36ee0-318">Example token lifetime policies</span></span>
<span data-ttu-id="36ee0-319">Mnoho scénářů je možné ve službě Azure AD při vytváření a správa životnosti tokenu pro aplikace, objekty služby a celkové organizaci.</span><span class="sxs-lookup"><span data-stu-id="36ee0-319">Many scenarios are possible in Azure AD when you can create and manage token lifetimes for apps, service principals, and your overall organization.</span></span> <span data-ttu-id="36ee0-320">V této části jsme provede několik běžných scénářů zásady, které můžete použít nové pravidel pro:</span><span class="sxs-lookup"><span data-stu-id="36ee0-320">In this section, we walk through a few common policy scenarios that can help you impose new rules for:</span></span>

* <span data-ttu-id="36ee0-321">Životnost tokenu</span><span class="sxs-lookup"><span data-stu-id="36ee0-321">Token Lifetime</span></span>
* <span data-ttu-id="36ee0-322">Token maximální doba neaktivní</span><span class="sxs-lookup"><span data-stu-id="36ee0-322">Token Max Inactive Time</span></span>
* <span data-ttu-id="36ee0-323">Maximální stáří tokenu</span><span class="sxs-lookup"><span data-stu-id="36ee0-323">Token Max Age</span></span>

<span data-ttu-id="36ee0-324">V příkladech hello dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="36ee0-324">In hello examples, you can learn how to:</span></span>

* <span data-ttu-id="36ee0-325">Správa organizace výchozí zásady</span><span class="sxs-lookup"><span data-stu-id="36ee0-325">Manage an organization's default policy</span></span>
* <span data-ttu-id="36ee0-326">Vytvoření zásady pro přihlášení k webové</span><span class="sxs-lookup"><span data-stu-id="36ee0-326">Create a policy for web sign-in</span></span>
* <span data-ttu-id="36ee0-327">Vytvořit zásadu pro nativní aplikaci, která volá webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="36ee0-327">Create a policy for a native app that calls a web API</span></span>
* <span data-ttu-id="36ee0-328">Spravovat pokročilé zásady</span><span class="sxs-lookup"><span data-stu-id="36ee0-328">Manage an advanced policy</span></span>

### <a name="prerequisites"></a><span data-ttu-id="36ee0-329">Požadavky</span><span class="sxs-lookup"><span data-stu-id="36ee0-329">Prerequisites</span></span>
<span data-ttu-id="36ee0-330">V hello následující příklady vytvářet, aktualizovat, odkaz a odstranit zásady pro aplikace, objekty služby a celkové organizaci.</span><span class="sxs-lookup"><span data-stu-id="36ee0-330">In hello following examples, you create, update, link, and delete policies for apps, service principals, and your overall organization.</span></span> <span data-ttu-id="36ee0-331">Pokud jste nový tooAzure AD, doporučujeme Další informace o [jak tooget na Azure AD klienta](active-directory-howto-tenant.md) předtím, než budete pokračovat v těchto příkladech.</span><span class="sxs-lookup"><span data-stu-id="36ee0-331">If you are new tooAzure AD, we recommend that you learn about [how tooget an Azure AD tenant](active-directory-howto-tenant.md) before you proceed with these examples.</span></span>  

<span data-ttu-id="36ee0-332">tooget spuštění hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="36ee0-332">tooget started, do hello following steps:</span></span>

1. <span data-ttu-id="36ee0-333">Stáhněte si nejnovější hello [Azure AD PowerShell modulu verze Public Preview](https://www.powershellgallery.com/packages/AzureADPreview).</span><span class="sxs-lookup"><span data-stu-id="36ee0-333">Download hello latest [Azure AD PowerShell Module Public Preview release](https://www.powershellgallery.com/packages/AzureADPreview).</span></span>
2. <span data-ttu-id="36ee0-334">Spustit hello `Connect` příkaz toosign v tooyour účet správce Azure AD.</span><span class="sxs-lookup"><span data-stu-id="36ee0-334">Run hello `Connect` command toosign in tooyour Azure AD admin account.</span></span> <span data-ttu-id="36ee0-335">Spusťte tento příkaz pokaždé, když zahájit novou relaci.</span><span class="sxs-lookup"><span data-stu-id="36ee0-335">Run this command each time you start a new session.</span></span>

    ```PowerShell
    Connect-AzureAD -Confirm
    ```

3. <span data-ttu-id="36ee0-336">toosee všechny zásady, které byly vytvořeny ve vaší organizaci, spusťte hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="36ee0-336">toosee all policies that have been created in your organization, run hello following command.</span></span> <span data-ttu-id="36ee0-337">Tento příkaz spusťte po většinu operací v hello následující scénáře.</span><span class="sxs-lookup"><span data-stu-id="36ee0-337">Run this command after most operations in hello following scenarios.</span></span> <span data-ttu-id="36ee0-338">Spuštění příkazu hello také pomáhá získat hello ** ** zásad.</span><span class="sxs-lookup"><span data-stu-id="36ee0-338">Running hello command also helps you get hello ** ** of your policies.</span></span>

    ```PowerShell
    Get-AzureADPolicy
    ```

### <a name="example-manage-an-organizations-default-policy"></a><span data-ttu-id="36ee0-339">Příklad: Správa organizace výchozí zásady</span><span class="sxs-lookup"><span data-stu-id="36ee0-339">Example: Manage an organization's default policy</span></span>
<span data-ttu-id="36ee0-340">V tomto příkladu vytvoříte zásadu, která umožňuje uživatelům přihlásit se méně často v rámci celé organizace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-340">In this example, you create a policy that lets your users sign in less frequently across your entire organization.</span></span> <span data-ttu-id="36ee0-341">toodo, vytvořte zásadu životnost tokenu pro Single-Factor aktualizovat tokeny, které se použije v organizaci.</span><span class="sxs-lookup"><span data-stu-id="36ee0-341">toodo this, create a token lifetime policy for Single-Factor Refresh Tokens, which is applied across your organization.</span></span> <span data-ttu-id="36ee0-342">zásady Hello je použité tooevery aplikace ve vaší organizaci a tooeach instanční objekt, který ještě nemá zásady nastavené.</span><span class="sxs-lookup"><span data-stu-id="36ee0-342">hello policy is applied tooevery application in your organization, and tooeach service principal that doesn’t already have a policy set.</span></span>

1. <span data-ttu-id="36ee0-343">Vytvořte zásadu životnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="36ee0-343">Create a token lifetime policy.</span></span>

    1.  <span data-ttu-id="36ee0-344">Nastavit hello Single-Factor aktualizovat Token příliš "dokud odvolaný."</span><span class="sxs-lookup"><span data-stu-id="36ee0-344">Set hello Single-Factor Refresh Token too"until-revoked."</span></span> <span data-ttu-id="36ee0-345">Hello token nevyprší, dokud je odvolat přístup.</span><span class="sxs-lookup"><span data-stu-id="36ee0-345">hello token doesn't expire until access is revoked.</span></span> <span data-ttu-id="36ee0-346">Vytvořte hello následující definice zásady:</span><span class="sxs-lookup"><span data-stu-id="36ee0-346">Create hello following policy definition:</span></span>

        ```PowerShell
        @('{
            "TokenLifetimePolicy":
            {
                "Version":1,
                "MaxAgeSingleFactor":"until-revoked"
            }
        }')
        ```

    2.  <span data-ttu-id="36ee0-347">toocreate hello zásady, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="36ee0-347">toocreate hello policy, run hello following command:</span></span>

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    3.  <span data-ttu-id="36ee0-348">toosee nové zásady a zásady hello tooget **ObjectId**spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="36ee0-348">toosee your new policy, and tooget hello policy's **ObjectId**, run hello following command:</span></span>

        ```PowerShell
        Get-AzureADPolicy
        ```

2. <span data-ttu-id="36ee0-349">Aktualizujte zásady hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-349">Update hello policy.</span></span>

    <span data-ttu-id="36ee0-350">Můžete rozhodnout, že hello první zásad nastavených v tomto příkladu není striktní, protože vaše služba vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="36ee0-350">You might decide that hello first policy you set in this example is not as strict as your service requires.</span></span> <span data-ttu-id="36ee0-351">spuštění vaší Single-Factor aktualizovat Token tooexpire dvou dní tooset hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="36ee0-351">tooset your Single-Factor Refresh Token tooexpire in two days, run hello following command:</span></span>

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId FROM GET COMMAND> -DisplayName "OrganizationDefaultPolicyUpdatedScenario" -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"2.00:00:00"}}')
    ```

### <a name="example-create-a-policy-for-web-sign-in"></a><span data-ttu-id="36ee0-352">Příklad: Vytvoření zásady pro přihlášení k webové</span><span class="sxs-lookup"><span data-stu-id="36ee0-352">Example: Create a policy for web sign-in</span></span>

<span data-ttu-id="36ee0-353">V tomto příkladu vytvoříte zásadu, která vyžaduje tooauthenticate uživatelé častěji ve vaší webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="36ee0-353">In this example, you create a policy that requires users tooauthenticate more frequently in your web app.</span></span> <span data-ttu-id="36ee0-354">Tato zásada nastaví životnost hello hello tokenů přístupu nebo ID a hello maximální stáří objekt relace Multi-Factor tokenu toohello služby vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-354">This policy sets hello lifetime of hello access/ID tokens and hello max age of a multi-factor session token toohello service principal of your web app.</span></span>

1. <span data-ttu-id="36ee0-355">Vytvořte zásadu životnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="36ee0-355">Create a token lifetime policy.</span></span>

    <span data-ttu-id="36ee0-356">Tato zásada pro webové přihlášení, nastaví životnost tokenu přístupu nebo ID hello a hello maximální relace single-factor tokenu stáří tootwo hodin.</span><span class="sxs-lookup"><span data-stu-id="36ee0-356">This policy, for web sign-in, sets hello access/ID token lifetime and hello max single-factor session token age tootwo hours.</span></span>

    1.  <span data-ttu-id="36ee0-357">toocreate hello zásady, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="36ee0-357">toocreate hello policy, run this command:</span></span>

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00","MaxAgeSessionSingleFactor":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  <span data-ttu-id="36ee0-358">toosee nové zásady a zásady hello tooget **ObjectId**spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="36ee0-358">toosee your new policy, and tooget hello policy **ObjectId**, run hello following command:</span></span>

        ```PowerShell
        Get-AzureADPolicy
        ```

2.  <span data-ttu-id="36ee0-359">Přiřaďte hello zásad tooyour instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="36ee0-359">Assign hello policy tooyour service principal.</span></span> <span data-ttu-id="36ee0-360">Musíte taky tooget hello **ObjectId** z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="36ee0-360">You also need tooget hello **ObjectId** of your service principal.</span></span> 

    1.  <span data-ttu-id="36ee0-361">toosee objekty všechna firemní služby, můžete dotazovat [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity).</span><span class="sxs-lookup"><span data-stu-id="36ee0-361">toosee all your organization's service principals, you can query [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity).</span></span> <span data-ttu-id="36ee0-362">Nebo v [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), přihlaste se tooyour účet Azure AD.</span><span class="sxs-lookup"><span data-stu-id="36ee0-362">Or, in [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), sign in tooyour Azure AD account.</span></span>

    2.  <span data-ttu-id="36ee0-363">Pokud máte hello **ObjectId** objektu služby spustit následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="36ee0-363">When you have hello **ObjectId** of your service principal, run hello following command:</span></span>

        ```PowerShell
        Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
        ```


### <a name="example-create-a-policy-for-a-native-app-that-calls-a-web-api"></a><span data-ttu-id="36ee0-364">Příklad: Vytvoření zásady pro nativní aplikaci, která volá webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="36ee0-364">Example: Create a policy for a native app that calls a web API</span></span>
<span data-ttu-id="36ee0-365">V tomto příkladu vytvoříte zásadu, která vyžaduje uživatelé tooauthenticate méně často.</span><span class="sxs-lookup"><span data-stu-id="36ee0-365">In this example, you create a policy that requires users tooauthenticate less frequently.</span></span> <span data-ttu-id="36ee0-366">zásady Hello také prodlouží hello množství času, který uživatel může být neaktivní, než uživatele hello musí novému ověření.</span><span class="sxs-lookup"><span data-stu-id="36ee0-366">hello policy also lengthens hello amount of time a user can be inactive before hello user must reauthenticate.</span></span> <span data-ttu-id="36ee0-367">zásady Hello je použité toohello webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="36ee0-367">hello policy is applied toohello web API.</span></span> <span data-ttu-id="36ee0-368">Když nativní aplikaci hello vyžádá hello webového rozhraní API jako prostředek, je tato zásada použitá.</span><span class="sxs-lookup"><span data-stu-id="36ee0-368">When hello native app requests hello web API as a resource, this policy is applied.</span></span>

1. <span data-ttu-id="36ee0-369">Vytvořte zásadu životnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="36ee0-369">Create a token lifetime policy.</span></span>

    1.  <span data-ttu-id="36ee0-370">toocreate striktní zásady pro webové rozhraní API, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="36ee0-370">toocreate a strict policy for a web API, run hello following command:</span></span>

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"30.00:00:00","MaxAgeMultiFactor":"until-revoked","MaxAgeSingleFactor":"180.00:00:00"}}') -DisplayName "WebApiDefaultPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  <span data-ttu-id="36ee0-371">toosee nové zásady a zásady hello tooget **ObjectId**spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="36ee0-371">toosee your new policy, and tooget hello policy **ObjectId**, run hello following command:</span></span>

        ```PowerShell
        Get-AzureADPolicy
        ```

2. <span data-ttu-id="36ee0-372">Přiřaďte hello zásad tooyour webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="36ee0-372">Assign hello policy tooyour web API.</span></span> <span data-ttu-id="36ee0-373">Musíte taky tooget hello **ObjectId** vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-373">You also need tooget hello **ObjectId** of your application.</span></span> <span data-ttu-id="36ee0-374">Hello toofind nejlepší způsob, jak vaše aplikace **ObjectId** je toouse hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="36ee0-374">hello best way toofind your app's **ObjectId** is toouse hello [Azure portal](https://portal.azure.com/).</span></span>

   <span data-ttu-id="36ee0-375">Pokud máte hello **ObjectId** vaší aplikace, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="36ee0-375">When you have hello **ObjectId** of your app, run hello following command:</span></span>

        ```PowerShell
        Add-AzureADApplicationPolicy -Id <ObjectId of hello Application> -RefObjectId <ObjectId of hello Policy>
        ```


### <a name="example-manage-an-advanced-policy"></a><span data-ttu-id="36ee0-376">Příklad: Správa pokročilé zásady</span><span class="sxs-lookup"><span data-stu-id="36ee0-376">Example: Manage an advanced policy</span></span>
<span data-ttu-id="36ee0-377">V tomto příkladu vytvoříte několik zásad, toolearn fungování hello priority systému.</span><span class="sxs-lookup"><span data-stu-id="36ee0-377">In this example, you create a few policies, toolearn how hello priority system works.</span></span> <span data-ttu-id="36ee0-378">Také dozvíte, jak toomanage víc zásad, které jsou použité tooseveral objekty.</span><span class="sxs-lookup"><span data-stu-id="36ee0-378">You also can learn how toomanage multiple policies that are applied tooseveral objects.</span></span>

1. <span data-ttu-id="36ee0-379">Vytvořte zásadu životnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="36ee0-379">Create a token lifetime policy.</span></span>

    1.  <span data-ttu-id="36ee0-380">toocreate výchozí zásadu organizace, která nastaví dny hello Single-Factor aktualizovat Token too30 doba platnosti, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="36ee0-380">toocreate an organization default policy that sets hello Single-Factor Refresh Token lifetime too30 days, run hello following command:</span></span>

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"30.00:00:00"}}') -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    2.  <span data-ttu-id="36ee0-381">toosee nové zásady a zásady hello tooget **ObjectId**spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="36ee0-381">toosee your new policy, and tooget hello policy's **ObjectId**, run hello following command:</span></span>

        ```PowerShell
        Get-AzureADPolicy
        ```

2. <span data-ttu-id="36ee0-382">Přiřaďte hello zásad tooa instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="36ee0-382">Assign hello policy tooa service principal.</span></span>

    <span data-ttu-id="36ee0-383">Teď máte zásadu, která se použije toohello celé organizace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-383">Now, you have a policy that applies toohello entire organization.</span></span> <span data-ttu-id="36ee0-384">Možná chcete tuto zásadu 30denní toopreserve pro objekt konkrétní služby, ale změnit hello organizace výchozí zásady toohello horní limit počtu "dokud odvolaný."</span><span class="sxs-lookup"><span data-stu-id="36ee0-384">You might want toopreserve this 30-day policy for a specific service principal, but change hello organization default policy toohello upper limit of "until-revoked."</span></span>

    1.  <span data-ttu-id="36ee0-385">toosee objekty všechna firemní služby, můžete dotazovat [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity).</span><span class="sxs-lookup"><span data-stu-id="36ee0-385">toosee all your organization's service principals, you can query [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity).</span></span> <span data-ttu-id="36ee0-386">Nebo v [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), přihlaste se pomocí účtu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="36ee0-386">Or, in [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), sign in by using your Azure AD account.</span></span>

    2.  <span data-ttu-id="36ee0-387">Pokud máte hello **ObjectId** objektu služby spustit následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="36ee0-387">When you have hello **ObjectId** of your service principal, run hello following command:</span></span>

            ```PowerShell
            Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
            ```
        
3. <span data-ttu-id="36ee0-388">Sada hello `IsOrganizationDefault` příznak toofalse:</span><span class="sxs-lookup"><span data-stu-id="36ee0-388">Set hello `IsOrganizationDefault` flag toofalse:</span></span>

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $false
    ```

4. <span data-ttu-id="36ee0-389">Vytvořte novou zásadu výchozí organizace:</span><span class="sxs-lookup"><span data-stu-id="36ee0-389">Create a new organization default policy:</span></span>

    ```PowerShell
    New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "ComplexPolicyScenarioTwo" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
    ```

    <span data-ttu-id="36ee0-390">Nyní máte hello původní zásady propojené tooyour instanční objekt a nové zásady hello je nastaven jako výchozí zásady vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-390">You now have hello original policy linked tooyour service principal, and hello new policy is set as your organization default policy.</span></span> <span data-ttu-id="36ee0-391">Je důležité tooremember, že zásady tooservice objekty mají přednost před výchozí zásady organizace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-391">It's important tooremember that policies applied tooservice principals have priority over organization default policies.</span></span>

## <a name="cmdlet-reference"></a><span data-ttu-id="36ee0-392">Reference k rutinám</span><span class="sxs-lookup"><span data-stu-id="36ee0-392">Cmdlet reference</span></span>

### <a name="manage-policies"></a><span data-ttu-id="36ee0-393">Správa zásad</span><span class="sxs-lookup"><span data-stu-id="36ee0-393">Manage policies</span></span>

<span data-ttu-id="36ee0-394">Můžete použít následující rutiny toomanage zásady hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-394">You can use hello following cmdlets toomanage policies.</span></span>

#### <a name="new-azureadpolicy"></a><span data-ttu-id="36ee0-395">Nové AzureADPolicy</span><span class="sxs-lookup"><span data-stu-id="36ee0-395">New-AzureADPolicy</span></span>

<span data-ttu-id="36ee0-396">Vytvoří novou zásadu.</span><span class="sxs-lookup"><span data-stu-id="36ee0-396">Creates a new policy.</span></span>

```PowerShell
New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsOrganizationDefault <boolean> -Type <Policy Type>
```

| <span data-ttu-id="36ee0-397">Parametry</span><span class="sxs-lookup"><span data-stu-id="36ee0-397">Parameters</span></span> | <span data-ttu-id="36ee0-398">Popis</span><span class="sxs-lookup"><span data-stu-id="36ee0-398">Description</span></span> | <span data-ttu-id="36ee0-399">Příklad</span><span class="sxs-lookup"><span data-stu-id="36ee0-399">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Definition</code> |<span data-ttu-id="36ee0-400">Pole stringified formátu JSON, který obsahuje všechny zásady hello pravidla.</span><span class="sxs-lookup"><span data-stu-id="36ee0-400">Array of stringified JSON that contains all hello policy's rules.</span></span> | `-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;DisplayName</code> |<span data-ttu-id="36ee0-401">Řetězec název zásady hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-401">String of hello policy name.</span></span> |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;IsOrganizationDefault</code> |<span data-ttu-id="36ee0-402">V případě hodnoty true, nastaví jako výchozí zásady organizace hello hello zásad.</span><span class="sxs-lookup"><span data-stu-id="36ee0-402">If true, sets hello policy as hello organization's default policy.</span></span> <span data-ttu-id="36ee0-403">Pokud je hodnota false, neprovede žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="36ee0-403">If false, does nothing.</span></span> |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code> |<span data-ttu-id="36ee0-404">Typ zásad.</span><span class="sxs-lookup"><span data-stu-id="36ee0-404">Type of policy.</span></span> <span data-ttu-id="36ee0-405">Pro token životnosti vždy používejte "TokenLifetimePolicy."</span><span class="sxs-lookup"><span data-stu-id="36ee0-405">For token lifetimes, always use "TokenLifetimePolicy."</span></span> | `-Type "TokenLifetimePolicy"` |
| <span data-ttu-id="36ee0-406"><code>&#8209;AlternativeIdentifier</code>[Nepovinné]</span><span class="sxs-lookup"><span data-stu-id="36ee0-406"><code>&#8209;AlternativeIdentifier</code> [Optional]</span></span> |<span data-ttu-id="36ee0-407">Nastaví alternativní ID pro zásadu hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-407">Sets an alternative ID for hello policy.</span></span> |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="get-azureadpolicy"></a><span data-ttu-id="36ee0-408">Get-AzureADPolicy</span><span class="sxs-lookup"><span data-stu-id="36ee0-408">Get-AzureADPolicy</span></span>
<span data-ttu-id="36ee0-409">Získá všechny zásady služby Azure AD nebo zadanou zásadu.</span><span class="sxs-lookup"><span data-stu-id="36ee0-409">Gets all Azure AD policies or a specified policy.</span></span>

```PowerShell
Get-AzureADPolicy
```

| <span data-ttu-id="36ee0-410">Parametry</span><span class="sxs-lookup"><span data-stu-id="36ee0-410">Parameters</span></span> | <span data-ttu-id="36ee0-411">Popis</span><span class="sxs-lookup"><span data-stu-id="36ee0-411">Description</span></span> | <span data-ttu-id="36ee0-412">Příklad</span><span class="sxs-lookup"><span data-stu-id="36ee0-412">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="36ee0-413"><code>&#8209;Id</code>[Nepovinné]</span><span class="sxs-lookup"><span data-stu-id="36ee0-413"><code>&#8209;Id</code> [Optional]</span></span> |<span data-ttu-id="36ee0-414">**ObjectId (Id)** hello zásady, které chcete.</span><span class="sxs-lookup"><span data-stu-id="36ee0-414">**ObjectId (Id)** of hello policy you want.</span></span> |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadpolicyappliedobject"></a><span data-ttu-id="36ee0-415">Get-AzureADPolicyAppliedObject</span><span class="sxs-lookup"><span data-stu-id="36ee0-415">Get-AzureADPolicyAppliedObject</span></span>
<span data-ttu-id="36ee0-416">Získá všechny aplikace a objekty služby, které jsou propojené tooa zásad.</span><span class="sxs-lookup"><span data-stu-id="36ee0-416">Gets all apps and service principals that are linked tooa policy.</span></span>

```PowerShell
Get-AzureADPolicyAppliedObject -Id <ObjectId of Policy>
```

| <span data-ttu-id="36ee0-417">Parametry</span><span class="sxs-lookup"><span data-stu-id="36ee0-417">Parameters</span></span> | <span data-ttu-id="36ee0-418">Popis</span><span class="sxs-lookup"><span data-stu-id="36ee0-418">Description</span></span> | <span data-ttu-id="36ee0-419">Příklad</span><span class="sxs-lookup"><span data-stu-id="36ee0-419">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="36ee0-420">**ObjectId (Id)** hello zásady, které chcete.</span><span class="sxs-lookup"><span data-stu-id="36ee0-420">**ObjectId (Id)** of hello policy you want.</span></span> |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="set-azureadpolicy"></a><span data-ttu-id="36ee0-421">Set-AzureADPolicy</span><span class="sxs-lookup"><span data-stu-id="36ee0-421">Set-AzureADPolicy</span></span>
<span data-ttu-id="36ee0-422">Aktualizuje existující zásady.</span><span class="sxs-lookup"><span data-stu-id="36ee0-422">Updates an existing policy.</span></span>

```PowerShell
Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName <string>
```

| <span data-ttu-id="36ee0-423">Parametry</span><span class="sxs-lookup"><span data-stu-id="36ee0-423">Parameters</span></span> | <span data-ttu-id="36ee0-424">Popis</span><span class="sxs-lookup"><span data-stu-id="36ee0-424">Description</span></span> | <span data-ttu-id="36ee0-425">Příklad</span><span class="sxs-lookup"><span data-stu-id="36ee0-425">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="36ee0-426">**ObjectId (Id)** hello zásady, které chcete.</span><span class="sxs-lookup"><span data-stu-id="36ee0-426">**ObjectId (Id)** of hello policy you want.</span></span> |`-Id <ObjectId of Policy>` |
| <code>&#8209;DisplayName</code> |<span data-ttu-id="36ee0-427">Řetězec název zásady hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-427">String of hello policy name.</span></span> |`-DisplayName "MyTokenPolicy"` |
| <span data-ttu-id="36ee0-428"><code>&#8209;Definition</code>[Nepovinné]</span><span class="sxs-lookup"><span data-stu-id="36ee0-428"><code>&#8209;Definition</code> [Optional]</span></span> |<span data-ttu-id="36ee0-429">Pole stringified formátu JSON, který obsahuje všechny zásady hello pravidla.</span><span class="sxs-lookup"><span data-stu-id="36ee0-429">Array of stringified JSON that contains all hello policy's rules.</span></span> |`-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <span data-ttu-id="36ee0-430"><code>&#8209;IsOrganizationDefault</code>[Nepovinné]</span><span class="sxs-lookup"><span data-stu-id="36ee0-430"><code>&#8209;IsOrganizationDefault</code> [Optional]</span></span> |<span data-ttu-id="36ee0-431">V případě hodnoty true, nastaví jako výchozí zásady organizace hello hello zásad.</span><span class="sxs-lookup"><span data-stu-id="36ee0-431">If true, sets hello policy as hello organization's default policy.</span></span> <span data-ttu-id="36ee0-432">Pokud je hodnota false, neprovede žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="36ee0-432">If false, does nothing.</span></span> |`-IsOrganizationDefault $true` |
| <span data-ttu-id="36ee0-433"><code>&#8209;Type</code>[Nepovinné]</span><span class="sxs-lookup"><span data-stu-id="36ee0-433"><code>&#8209;Type</code> [Optional]</span></span> |<span data-ttu-id="36ee0-434">Typ zásad.</span><span class="sxs-lookup"><span data-stu-id="36ee0-434">Type of policy.</span></span> <span data-ttu-id="36ee0-435">Pro token životnosti vždy používejte "TokenLifetimePolicy."</span><span class="sxs-lookup"><span data-stu-id="36ee0-435">For token lifetimes, always use "TokenLifetimePolicy."</span></span> |`-Type "TokenLifetimePolicy"` |
| <span data-ttu-id="36ee0-436"><code>&#8209;AlternativeIdentifier</code>[Nepovinné]</span><span class="sxs-lookup"><span data-stu-id="36ee0-436"><code>&#8209;AlternativeIdentifier</code> [Optional]</span></span> |<span data-ttu-id="36ee0-437">Nastaví alternativní ID pro zásadu hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-437">Sets an alternative ID for hello policy.</span></span> |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="remove-azureadpolicy"></a><span data-ttu-id="36ee0-438">Odebrat AzureADPolicy</span><span class="sxs-lookup"><span data-stu-id="36ee0-438">Remove-AzureADPolicy</span></span>
<span data-ttu-id="36ee0-439">Odstranění hello zadat zásady.</span><span class="sxs-lookup"><span data-stu-id="36ee0-439">Deletes hello specified policy.</span></span>

```PowerShell
 Remove-AzureADPolicy -Id <ObjectId of Policy>
```

| <span data-ttu-id="36ee0-440">Parametry</span><span class="sxs-lookup"><span data-stu-id="36ee0-440">Parameters</span></span> | <span data-ttu-id="36ee0-441">Popis</span><span class="sxs-lookup"><span data-stu-id="36ee0-441">Description</span></span> | <span data-ttu-id="36ee0-442">Příklad</span><span class="sxs-lookup"><span data-stu-id="36ee0-442">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="36ee0-443">**ObjectId (Id)** hello zásady, které chcete.</span><span class="sxs-lookup"><span data-stu-id="36ee0-443">**ObjectId (Id)** of hello policy you want.</span></span> | `-Id <ObjectId of Policy>` |

</br></br>

### <a name="application-policies"></a><span data-ttu-id="36ee0-444">Zásady aplikací</span><span class="sxs-lookup"><span data-stu-id="36ee0-444">Application policies</span></span>
<span data-ttu-id="36ee0-445">Můžete použít následující rutiny pro zásady aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-445">You can use hello following cmdlets for application policies.</span></span></br></br>

#### <a name="add-azureadapplicationpolicy"></a><span data-ttu-id="36ee0-446">Přidat AzureADApplicationPolicy</span><span class="sxs-lookup"><span data-stu-id="36ee0-446">Add-AzureADApplicationPolicy</span></span>
<span data-ttu-id="36ee0-447">Odkazy hello zadat zásady tooan aplikace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-447">Links hello specified policy tooan application.</span></span>

```PowerShell
Add-AzureADApplicationPolicy -Id <ObjectId of Application> -RefObjectId <ObjectId of Policy>
```

| <span data-ttu-id="36ee0-448">Parametry</span><span class="sxs-lookup"><span data-stu-id="36ee0-448">Parameters</span></span> | <span data-ttu-id="36ee0-449">Popis</span><span class="sxs-lookup"><span data-stu-id="36ee0-449">Description</span></span> | <span data-ttu-id="36ee0-450">Příklad</span><span class="sxs-lookup"><span data-stu-id="36ee0-450">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="36ee0-451">**ObjectId (Id)** aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-451">**ObjectId (Id)** of hello application.</span></span> | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |<span data-ttu-id="36ee0-452">**ObjectId** hello zásad.</span><span class="sxs-lookup"><span data-stu-id="36ee0-452">**ObjectId** of hello policy.</span></span> | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadapplicationpolicy"></a><span data-ttu-id="36ee0-453">Get-AzureADApplicationPolicy</span><span class="sxs-lookup"><span data-stu-id="36ee0-453">Get-AzureADApplicationPolicy</span></span>
<span data-ttu-id="36ee0-454">Získá hello zásady, která je přiřazena tooan aplikace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-454">Gets hello policy that is assigned tooan application.</span></span>

```PowerShell
Get-AzureADApplicationPolicy -Id <ObjectId of Application>
```

| <span data-ttu-id="36ee0-455">Parametry</span><span class="sxs-lookup"><span data-stu-id="36ee0-455">Parameters</span></span> | <span data-ttu-id="36ee0-456">Popis</span><span class="sxs-lookup"><span data-stu-id="36ee0-456">Description</span></span> | <span data-ttu-id="36ee0-457">Příklad</span><span class="sxs-lookup"><span data-stu-id="36ee0-457">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="36ee0-458">**ObjectId (Id)** aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-458">**ObjectId (Id)** of hello application.</span></span> | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadapplicationpolicy"></a><span data-ttu-id="36ee0-459">Odebrat AzureADApplicationPolicy</span><span class="sxs-lookup"><span data-stu-id="36ee0-459">Remove-AzureADApplicationPolicy</span></span>
<span data-ttu-id="36ee0-460">Odebere zásadu z aplikace.</span><span class="sxs-lookup"><span data-stu-id="36ee0-460">Removes a policy from an application.</span></span>

```PowerShell
Remove-AzureADApplicationPolicy -Id <ObjectId of Application> -PolicyId <ObjectId of Policy>
```

| <span data-ttu-id="36ee0-461">Parametry</span><span class="sxs-lookup"><span data-stu-id="36ee0-461">Parameters</span></span> | <span data-ttu-id="36ee0-462">Popis</span><span class="sxs-lookup"><span data-stu-id="36ee0-462">Description</span></span> | <span data-ttu-id="36ee0-463">Příklad</span><span class="sxs-lookup"><span data-stu-id="36ee0-463">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="36ee0-464">**ObjectId (Id)** aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-464">**ObjectId (Id)** of hello application.</span></span> | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |<span data-ttu-id="36ee0-465">**ObjectId** hello zásad.</span><span class="sxs-lookup"><span data-stu-id="36ee0-465">**ObjectId** of hello policy.</span></span> | `-PolicyId <ObjectId of Policy>` |

</br></br>

### <a name="service-principal-policies"></a><span data-ttu-id="36ee0-466">Hlavní zásady služby</span><span class="sxs-lookup"><span data-stu-id="36ee0-466">Service principal policies</span></span>
<span data-ttu-id="36ee0-467">Můžete použít následující rutiny pro hlavní zásady služby hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-467">You can use hello following cmdlets for service principal policies.</span></span>

#### <a name="add-azureadserviceprincipalpolicy"></a><span data-ttu-id="36ee0-468">Přidat AzureADServicePrincipalPolicy</span><span class="sxs-lookup"><span data-stu-id="36ee0-468">Add-AzureADServicePrincipalPolicy</span></span>
<span data-ttu-id="36ee0-469">Odkazy hello určené zásadě tooa instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="36ee0-469">Links hello specified policy tooa service principal.</span></span>

```PowerShell
Add-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal> -RefObjectId <ObjectId of Policy>
```

| <span data-ttu-id="36ee0-470">Parametry</span><span class="sxs-lookup"><span data-stu-id="36ee0-470">Parameters</span></span> | <span data-ttu-id="36ee0-471">Popis</span><span class="sxs-lookup"><span data-stu-id="36ee0-471">Description</span></span> | <span data-ttu-id="36ee0-472">Příklad</span><span class="sxs-lookup"><span data-stu-id="36ee0-472">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="36ee0-473">**ObjectId (Id)** aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-473">**ObjectId (Id)** of hello application.</span></span> | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |<span data-ttu-id="36ee0-474">**ObjectId** hello zásad.</span><span class="sxs-lookup"><span data-stu-id="36ee0-474">**ObjectId** of hello policy.</span></span> | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a><span data-ttu-id="36ee0-475">Get-AzureADServicePrincipalPolicy</span><span class="sxs-lookup"><span data-stu-id="36ee0-475">Get-AzureADServicePrincipalPolicy</span></span>
<span data-ttu-id="36ee0-476">Získá všechny zásady propojené toohello zadaný instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="36ee0-476">Gets any policy linked toohello specified service principal.</span></span>

```PowerShell
Get-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>
```

| <span data-ttu-id="36ee0-477">Parametry</span><span class="sxs-lookup"><span data-stu-id="36ee0-477">Parameters</span></span> | <span data-ttu-id="36ee0-478">Popis</span><span class="sxs-lookup"><span data-stu-id="36ee0-478">Description</span></span> | <span data-ttu-id="36ee0-479">Příklad</span><span class="sxs-lookup"><span data-stu-id="36ee0-479">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="36ee0-480">**ObjectId (Id)** aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-480">**ObjectId (Id)** of hello application.</span></span> | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a><span data-ttu-id="36ee0-481">Odebrat AzureADServicePrincipalPolicy</span><span class="sxs-lookup"><span data-stu-id="36ee0-481">Remove-AzureADServicePrincipalPolicy</span></span>
<span data-ttu-id="36ee0-482">Odebere hello zásad hello zadaný instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="36ee0-482">Removes hello policy from hello specified service principal.</span></span>

```PowerShell
Remove-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>  -PolicyId <ObjectId of Policy>
```

| <span data-ttu-id="36ee0-483">Parametry</span><span class="sxs-lookup"><span data-stu-id="36ee0-483">Parameters</span></span> | <span data-ttu-id="36ee0-484">Popis</span><span class="sxs-lookup"><span data-stu-id="36ee0-484">Description</span></span> | <span data-ttu-id="36ee0-485">Příklad</span><span class="sxs-lookup"><span data-stu-id="36ee0-485">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="36ee0-486">**ObjectId (Id)** aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="36ee0-486">**ObjectId (Id)** of hello application.</span></span> | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |<span data-ttu-id="36ee0-487">**ObjectId** hello zásad.</span><span class="sxs-lookup"><span data-stu-id="36ee0-487">**ObjectId** of hello policy.</span></span> | `-PolicyId <ObjectId of Policy>` |
