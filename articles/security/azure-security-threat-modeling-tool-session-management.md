---
title: "aaaSession Management - Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
description: "způsoby zmírnění hrozeb v hello nástroj modelování hrozeb"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 915ffae3f775ca6902fcfb93e7e1952ce85612f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-session-management--articles"></a><span data-ttu-id="9cd9d-103">Rámce zabezpečení: Správa relací | Články</span><span class="sxs-lookup"><span data-stu-id="9cd9d-103">Security Frame: Session Management | Articles</span></span> 
| <span data-ttu-id="9cd9d-104">Produktům a službám</span><span class="sxs-lookup"><span data-stu-id="9cd9d-104">Product/Service</span></span> | <span data-ttu-id="9cd9d-105">Článek</span><span class="sxs-lookup"><span data-stu-id="9cd9d-105">Article</span></span> |
| --------------- | ------- |
| <span data-ttu-id="9cd9d-106">**Azure AD**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-106">**Azure AD**</span></span>    | <ul><li>[<span data-ttu-id="9cd9d-107">Implementace správné odhlášení pomocí ADAL metod při používání služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9cd9d-107">Implement proper logout using ADAL methods when using Azure AD</span></span>](#logout-adal)</li></ul> |
| <span data-ttu-id="9cd9d-108">Zařízení IoT</span><span class="sxs-lookup"><span data-stu-id="9cd9d-108">IoT Device</span></span> | <ul><li>[<span data-ttu-id="9cd9d-109">Použití omezené životnost generovaných tokenů SaS</span><span class="sxs-lookup"><span data-stu-id="9cd9d-109">Use finite lifetimes for generated SaS tokens</span></span>](#finite-tokens)</li></ul> |
| <span data-ttu-id="9cd9d-110">**Azure Documentdb**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-110">**Azure Document DB**</span></span> | <ul><li>[<span data-ttu-id="9cd9d-111">Pomocí minimální životnosti tokenu pro generovaných tokenů prostředků</span><span class="sxs-lookup"><span data-stu-id="9cd9d-111">Use minimum token lifetimes for generated Resource tokens</span></span>](#resource-tokens)</li></ul> |
| <span data-ttu-id="9cd9d-112">**ADFS**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-112">**ADFS**</span></span> | <ul><li>[<span data-ttu-id="9cd9d-113">Implementace správné odhlášení metodami WsFederation při použití služby AD FS</span><span class="sxs-lookup"><span data-stu-id="9cd9d-113">Implement proper logout using WsFederation methods when using ADFS</span></span>](#wsfederation-logout)</li></ul> |
| <span data-ttu-id="9cd9d-114">**Serveru identit**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-114">**Identity Server**</span></span> | <ul><li>[<span data-ttu-id="9cd9d-115">Implementace správné odhlášení při použití serveru identit</span><span class="sxs-lookup"><span data-stu-id="9cd9d-115">Implement proper logout when using Identity Server</span></span>](#proper-logout)</li></ul> |
| <span data-ttu-id="9cd9d-116">**Webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-116">**Web Application**</span></span> | <ul><li>[<span data-ttu-id="9cd9d-117">Aplikace dostupné přes protokol HTTPS, musíte použít bezpečné soubory cookie</span><span class="sxs-lookup"><span data-stu-id="9cd9d-117">Applications available over HTTPS must use secure cookies</span></span>](#https-secure-cookies)</li><li>[<span data-ttu-id="9cd9d-118">Všechny aplikace http, na základě by měl určovat pouze pro definici souboru cookie http</span><span class="sxs-lookup"><span data-stu-id="9cd9d-118">All http based application should specify http only for cookie definition</span></span>](#cookie-definition)</li><li>[<span data-ttu-id="9cd9d-119">Zmírnění před útoky webů požadavku padělání (proti útokům CSRF) na webových stránkách ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9cd9d-119">Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET web pages</span></span>](#csrf-asp)</li><li>[<span data-ttu-id="9cd9d-120">Nastavení relace dobu jeho existence nečinnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-120">Set up session for inactivity lifetime</span></span>](#inactivity-lifetime)</li><li>[<span data-ttu-id="9cd9d-121">Implementace správné odhlášení z aplikace hello</span><span class="sxs-lookup"><span data-stu-id="9cd9d-121">Implement proper logout from hello application</span></span>](#proper-app-logout)</li></ul> |
| <span data-ttu-id="9cd9d-122">**Webové rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-122">**Web API**</span></span> | <ul><li>[<span data-ttu-id="9cd9d-123">Zmírnění před útoky webů požadavku padělání (proti útokům CSRF) na rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="9cd9d-123">Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET Web APIs</span></span>](#csrf-api)</li></ul> |

## <span data-ttu-id="9cd9d-124"><a id="logout-adal"></a>Implementace správné odhlášení pomocí ADAL metod při používání služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9cd9d-124"><a id="logout-adal"></a>Implement proper logout using ADAL methods when using Azure AD</span></span>

| <span data-ttu-id="9cd9d-125">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-125">Title</span></span>                   | <span data-ttu-id="9cd9d-126">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-126">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-127">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-127">**Component**</span></span>               | <span data-ttu-id="9cd9d-128">Azure AD</span><span class="sxs-lookup"><span data-stu-id="9cd9d-128">Azure AD</span></span> | 
| <span data-ttu-id="9cd9d-129">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-129">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-130">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-130">Build</span></span> |  
| <span data-ttu-id="9cd9d-131">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-131">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-132">Obecné</span><span class="sxs-lookup"><span data-stu-id="9cd9d-132">Generic</span></span> |
| <span data-ttu-id="9cd9d-133">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-133">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-134">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-134">N/A</span></span>  |
| <span data-ttu-id="9cd9d-135">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-135">**References**</span></span>              | <span data-ttu-id="9cd9d-136">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-136">N/A</span></span>  |
| <span data-ttu-id="9cd9d-137">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-137">**Steps**</span></span> | <span data-ttu-id="9cd9d-138">Pokud aplikace hello závisí na přístupový token vydán Azure AD, by měly volat obslužné rutiny události odhlášení hello</span><span class="sxs-lookup"><span data-stu-id="9cd9d-138">If hello application relies on access token issued by Azure AD, hello logout event handler should call</span></span> |

### <a name="example"></a><span data-ttu-id="9cd9d-139">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-139">Example</span></span>
```C#
HttpContext.GetOwinContext().Authentication.SignOut(OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType)
```

### <a name="example"></a><span data-ttu-id="9cd9d-140">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-140">Example</span></span>
<span data-ttu-id="9cd9d-141">Relace uživatele ho měli zrušte také voláním metody Session.Abandon().</span><span class="sxs-lookup"><span data-stu-id="9cd9d-141">It should also destroy user's session by calling Session.Abandon() method.</span></span> <span data-ttu-id="9cd9d-142">Následující metoda ukazuje zabezpečené implementací odhlášení uživatele:</span><span class="sxs-lookup"><span data-stu-id="9cd9d-142">Following method shows secure implementation of user logout:</span></span>
```C#
    [HttpPost]
        [ValidateAntiForgeryToken]
        public void LogOff()
        {
            string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            AuthenticationContext authContext = new AuthenticationContext(Authority + TenantId, new NaiveSessionCache(userObjectID));
            authContext.TokenCache.Clear();
            Session.Clear();
            Session.Abandon();
            Response.SetCookie(new HttpCookie("ASP.NET_SessionId", string.Empty));
            HttpContext.GetOwinContext().Authentication.SignOut(
                OpenIdConnectAuthenticationDefaults.AuthenticationType,
                CookieAuthenticationDefaults.AuthenticationType);
        } 
```

## <span data-ttu-id="9cd9d-143"><a id="finite-tokens"></a>Použití omezené životnost generovaných tokenů SaS</span><span class="sxs-lookup"><span data-stu-id="9cd9d-143"><a id="finite-tokens"></a>Use finite lifetimes for generated SaS tokens</span></span>

| <span data-ttu-id="9cd9d-144">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-144">Title</span></span>                   | <span data-ttu-id="9cd9d-145">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-145">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-146">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-146">**Component**</span></span>               | <span data-ttu-id="9cd9d-147">Zařízení IoT</span><span class="sxs-lookup"><span data-stu-id="9cd9d-147">IoT Device</span></span> | 
| <span data-ttu-id="9cd9d-148">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-148">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-149">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-149">Build</span></span> |  
| <span data-ttu-id="9cd9d-150">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-150">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-151">Obecné</span><span class="sxs-lookup"><span data-stu-id="9cd9d-151">Generic</span></span> |
| <span data-ttu-id="9cd9d-152">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-152">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-153">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-153">N/A</span></span>  |
| <span data-ttu-id="9cd9d-154">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-154">**References**</span></span>              | <span data-ttu-id="9cd9d-155">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-155">N/A</span></span>  |
| <span data-ttu-id="9cd9d-156">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-156">**Steps**</span></span> | <span data-ttu-id="9cd9d-157">Tokeny SaS vygenerované ověřování tooAzure IoT Hub by měl mít dobou vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-157">SaS tokens generated for authenticating tooAzure IoT Hub should have a finite expiry period.</span></span> <span data-ttu-id="9cd9d-158">Zachovat hello SaS token životnosti tooa minimální toolimit hello časového intervalu, který může být přehrány v případě, že dojde k ohrožení hello tokeny.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-158">Keep hello SaS token lifetimes tooa minimum toolimit hello amount of time they can be replayed in case hello tokens are compromised.</span></span>|

## <span data-ttu-id="9cd9d-159"><a id="resource-tokens"></a>Pomocí minimální životnosti tokenu pro generovaných tokenů prostředků</span><span class="sxs-lookup"><span data-stu-id="9cd9d-159"><a id="resource-tokens"></a>Use minimum token lifetimes for generated Resource tokens</span></span>

| <span data-ttu-id="9cd9d-160">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-160">Title</span></span>                   | <span data-ttu-id="9cd9d-161">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-161">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-162">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-162">**Component**</span></span>               | <span data-ttu-id="9cd9d-163">Azure Documentdb</span><span class="sxs-lookup"><span data-stu-id="9cd9d-163">Azure Document DB</span></span> | 
| <span data-ttu-id="9cd9d-164">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-164">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-165">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-165">Build</span></span> |  
| <span data-ttu-id="9cd9d-166">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-166">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-167">Obecné</span><span class="sxs-lookup"><span data-stu-id="9cd9d-167">Generic</span></span> |
| <span data-ttu-id="9cd9d-168">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-168">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-169">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-169">N/A</span></span>  |
| <span data-ttu-id="9cd9d-170">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-170">**References**</span></span>              | <span data-ttu-id="9cd9d-171">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-171">N/A</span></span>  |
| <span data-ttu-id="9cd9d-172">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-172">**Steps**</span></span> | <span data-ttu-id="9cd9d-173">Snižte časový interval hello z prostředků tokenu tooa minimální požadovanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-173">Reduce hello timespan of resource token tooa minimum value required.</span></span> <span data-ttu-id="9cd9d-174">Prostředek tokeny mají platný časový interval výchozí 1 hodina.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-174">Resource tokens have a default valid timespan of 1 hour.</span></span>|

## <span data-ttu-id="9cd9d-175"><a id="wsfederation-logout"></a>Implementace správné odhlášení metodami WsFederation při použití služby AD FS</span><span class="sxs-lookup"><span data-stu-id="9cd9d-175"><a id="wsfederation-logout"></a>Implement proper logout using WsFederation methods when using ADFS</span></span>

| <span data-ttu-id="9cd9d-176">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-176">Title</span></span>                   | <span data-ttu-id="9cd9d-177">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-177">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-178">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-178">**Component**</span></span>               | <span data-ttu-id="9cd9d-179">ADFS</span><span class="sxs-lookup"><span data-stu-id="9cd9d-179">ADFS</span></span> | 
| <span data-ttu-id="9cd9d-180">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-180">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-181">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-181">Build</span></span> |  
| <span data-ttu-id="9cd9d-182">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-182">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-183">Obecné</span><span class="sxs-lookup"><span data-stu-id="9cd9d-183">Generic</span></span> |
| <span data-ttu-id="9cd9d-184">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-184">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-185">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-185">N/A</span></span>  |
| <span data-ttu-id="9cd9d-186">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-186">**References**</span></span>              | <span data-ttu-id="9cd9d-187">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-187">N/A</span></span>  |
| <span data-ttu-id="9cd9d-188">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-188">**Steps**</span></span> | <span data-ttu-id="9cd9d-189">Pokud aplikace hello spoléhá na službu tokenů zabezpečení tokenu vystavený služby AD FS, obslužné rutiny události odhlášení hello by měly volat metoda toolog WSFederationAuthenticationModule.FederatedSignOut() out hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-189">If hello application relies on STS token issued by ADFS, hello logout event handler should call WSFederationAuthenticationModule.FederatedSignOut() method toolog out hello user.</span></span> <span data-ttu-id="9cd9d-190">Také hello aktuální relace musí být zničený, a hello relace tokenu hodnota by měla být resetovat a zrušeny.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-190">Also hello current session should be destroyed, and hello session token value should be reset and nullified.</span></span>|

### <a name="example"></a><span data-ttu-id="9cd9d-191">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-191">Example</span></span>
```C#
        [HttpPost, ValidateAntiForgeryToken]
        [Authorization]
        public ActionResult SignOut(string redirectUrl)
        {
            if (!this.User.Identity.IsAuthenticated)
            {
                return this.View("LogOff", null);
            }

            // Removes hello user profile.
            this.Session.Clear();
            this.Session.Abandon();
            HttpContext.Current.Response.Cookies.Add(new System.Web.HttpCookie("ASP.NET_SessionId", string.Empty)
                {
                    Expires = DateTime.Now.AddDays(-1D),
                    Secure = true,
                    HttpOnly = true
                });

            // Signs out at hello specified security token service (STS) by using hello WS-Federation protocol.
            Uri signOutUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Issuer);
            Uri replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm);
            if (!string.IsNullOrEmpty(redirectUrl))
            {
                replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm + redirectUrl);
            }
           //     Signs out of hello current session and raises hello appropriate events.
            var authModule = FederatedAuthentication.WSFederationAuthenticationModule;
            authModule.SignOut(false);
        //     Signs out at hello specified security token service (STS) by using hello WS-Federation
        //     protocol.            
            WSFederationAuthenticationModule.FederatedSignOut(signOutUrl, replyUrl);
            return new RedirectResult(redirectUrl);
        }
```

## <span data-ttu-id="9cd9d-192"><a id="proper-logout"></a>Implementace správné odhlášení při použití serveru identit</span><span class="sxs-lookup"><span data-stu-id="9cd9d-192"><a id="proper-logout"></a>Implement proper logout when using Identity Server</span></span>

| <span data-ttu-id="9cd9d-193">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-193">Title</span></span>                   | <span data-ttu-id="9cd9d-194">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-194">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-195">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-195">**Component**</span></span>               | <span data-ttu-id="9cd9d-196">Serveru identit</span><span class="sxs-lookup"><span data-stu-id="9cd9d-196">Identity Server</span></span> | 
| <span data-ttu-id="9cd9d-197">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-197">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-198">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-198">Build</span></span> |  
| <span data-ttu-id="9cd9d-199">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-199">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-200">Obecné</span><span class="sxs-lookup"><span data-stu-id="9cd9d-200">Generic</span></span> |
| <span data-ttu-id="9cd9d-201">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-201">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-202">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-202">N/A</span></span>  |
| <span data-ttu-id="9cd9d-203">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-203">**References**</span></span>              | [<span data-ttu-id="9cd9d-204">Federované IdentityServer3 odhlášení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-204">IdentityServer3-Federated sign out</span></span>](https://identityserver.github.io/Documentation/docsv2/advanced/federated-signout.html) |
| <span data-ttu-id="9cd9d-205">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-205">**Steps**</span></span> | <span data-ttu-id="9cd9d-206">IdentityServer podporuje možnost toofederate hello zprostředkovatelům externí identity.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-206">IdentityServer supports hello ability toofederate with external identity providers.</span></span> <span data-ttu-id="9cd9d-207">Když se uživatel odhlásí od zprostředkovatele identity nadřazeného, v závislosti na protokolu hello, může to být možné tooreceive oznámení při odhlášení uživatele hello. To umožňuje toonotify IdentityServer, které jeho klienty, lze také podepsat hello uživatele se. Podívejte se do dokumentace hello v části odkazy hello podrobnosti implementace hello.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-207">When a user signs out of an upstream identity provider, depending upon hello protocol used, it might be possible tooreceive a notification when hello user signs out. It allows IdentityServer toonotify its clients so they can also sign hello user out. Check hello documentation in hello references section for hello implementation details.</span></span>|

## <span data-ttu-id="9cd9d-208"><a id="https-secure-cookies"></a>Aplikace dostupné přes protokol HTTPS, musíte použít bezpečné soubory cookie</span><span class="sxs-lookup"><span data-stu-id="9cd9d-208"><a id="https-secure-cookies"></a>Applications available over HTTPS must use secure cookies</span></span>

| <span data-ttu-id="9cd9d-209">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-209">Title</span></span>                   | <span data-ttu-id="9cd9d-210">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-210">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-211">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-211">**Component**</span></span>               | <span data-ttu-id="9cd9d-212">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="9cd9d-212">Web Application</span></span> | 
| <span data-ttu-id="9cd9d-213">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-213">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-214">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-214">Build</span></span> |  
| <span data-ttu-id="9cd9d-215">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-215">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-216">Obecné</span><span class="sxs-lookup"><span data-stu-id="9cd9d-216">Generic</span></span> |
| <span data-ttu-id="9cd9d-217">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-217">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-218">EnvironmentType - OnPrem</span><span class="sxs-lookup"><span data-stu-id="9cd9d-218">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="9cd9d-219">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-219">**References**</span></span>              | <span data-ttu-id="9cd9d-220">[Element (schéma nastavení ASP.NET) httpCookies](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure vlastnost](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx)</span><span class="sxs-lookup"><span data-stu-id="9cd9d-220">[httpCookies Element (ASP.NET Settings Schema)](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure Property](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx)</span></span> |
| <span data-ttu-id="9cd9d-221">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-221">**Steps**</span></span> | <span data-ttu-id="9cd9d-222">Soubory cookie jsou obvykle přístupné toohello domény jen pro kterou byly obor.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-222">Cookies are normally only accessible toohello domain for which they were scoped.</span></span> <span data-ttu-id="9cd9d-223">Definice hello "domény" bohužel nezahrnuje protokol hello, takže soubory cookie, které jsou vytvořené pomocí protokolu HTTPS jsou přístupné přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-223">Unfortunately, hello definition of "domain" does not include hello protocol so cookies that are created over HTTPS are accessible over HTTP.</span></span> <span data-ttu-id="9cd9d-224">Hello "zabezpečené" atribut uvádí, že toohello prohlížeč, který hello soubor cookie měl pouze být k dispozici prostřednictvím protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-224">hello "secure" attribute indicates toohello browser that hello cookie should only be made available over HTTPS.</span></span> <span data-ttu-id="9cd9d-225">Zkontrolujte, jestli všechny soubory cookie nastavte přes protokol HTTPS, používají hello **zabezpečené** atribut.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-225">Ensure that all cookies set over HTTPS use hello **secure** attribute.</span></span> <span data-ttu-id="9cd9d-226">v souboru web.config hello nastavením hello requireSSL atribut tootrue lze vynutit Hello požadavek.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-226">hello requirement can be enforced in hello web.config file by setting hello requireSSL attribute tootrue.</span></span> <span data-ttu-id="9cd9d-227">Je hello upřednostňovaný způsob, protože vynutí hello **zabezpečené** atribut pro všechny soubory cookie aktuálních a budoucích bez nutnosti toomake hello změny další kód.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-227">It is hello preferred approach because it will enforce hello **secure** attribute for all current and future cookies without hello need toomake any additional code changes.</span></span>|

### <a name="example"></a><span data-ttu-id="9cd9d-228">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-228">Example</span></span>
```C#
<configuration>
  <system.web>
    <httpCookies requireSSL="true"/>
  </system.web>
</configuration>
```
<span data-ttu-id="9cd9d-229">nastavení Hello se vynucuje, i když je aplikace hello použité tooaccess HTTP.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-229">hello setting is enforced even if HTTP is used tooaccess hello application.</span></span> <span data-ttu-id="9cd9d-230">Pokud se používá HTTP tooaccess hello aplikace, hello konců nastavení, které aplikace hello protože soubory cookie hello se nastavují s hello zabezpečené atribut a hello prohlížeče nebude jejich odeslání zpět toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-230">If HTTP is used tooaccess hello application, hello setting breaks hello application because hello cookies are set with hello secure attribute and hello browser will not send them back toohello application.</span></span>

| <span data-ttu-id="9cd9d-231">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-231">Title</span></span>                   | <span data-ttu-id="9cd9d-232">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-232">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-233">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-233">**Component**</span></span>               | <span data-ttu-id="9cd9d-234">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="9cd9d-234">Web Application</span></span> | 
| <span data-ttu-id="9cd9d-235">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-235">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-236">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-236">Build</span></span> |  
| <span data-ttu-id="9cd9d-237">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-237">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-238">Webové formuláře, MVC5</span><span class="sxs-lookup"><span data-stu-id="9cd9d-238">Web Forms, MVC5</span></span> |
| <span data-ttu-id="9cd9d-239">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-239">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-240">EnvironmentType - OnPrem</span><span class="sxs-lookup"><span data-stu-id="9cd9d-240">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="9cd9d-241">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-241">**References**</span></span>              | <span data-ttu-id="9cd9d-242">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-242">N/A</span></span>  |
| <span data-ttu-id="9cd9d-243">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-243">**Steps**</span></span> | <span data-ttu-id="9cd9d-244">Pokud hello webové aplikace je hello předávající strany a hello IdP serverem služby AD FS, tokenu FedAuth hello zabezpečené atribut je možné nakonfigurovat pomocí nastavení tooTrue requireSSL v `system.identityModel.services` části souboru Web.config:</span><span class="sxs-lookup"><span data-stu-id="9cd9d-244">When hello web application is hello Relying Party, and hello IdP is ADFS server, hello FedAuth token's secure attribute can be configured by setting requireSSL tooTrue in `system.identityModel.services` section of web.config:</span></span>|

### <a name="example"></a><span data-ttu-id="9cd9d-245">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-245">Example</span></span>
```C#
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:20:0" />
    ....  
    </federationConfiguration>
  </system.identityModel.services>
```

## <span data-ttu-id="9cd9d-246"><a id="cookie-definition"></a>Všechny aplikace http, na základě by měl určovat pouze pro definici souboru cookie http</span><span class="sxs-lookup"><span data-stu-id="9cd9d-246"><a id="cookie-definition"></a>All http based application should specify http only for cookie definition</span></span>

| <span data-ttu-id="9cd9d-247">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-247">Title</span></span>                   | <span data-ttu-id="9cd9d-248">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-248">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-249">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-249">**Component**</span></span>               | <span data-ttu-id="9cd9d-250">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="9cd9d-250">Web Application</span></span> | 
| <span data-ttu-id="9cd9d-251">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-251">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-252">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-252">Build</span></span> |  
| <span data-ttu-id="9cd9d-253">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-253">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-254">Obecné</span><span class="sxs-lookup"><span data-stu-id="9cd9d-254">Generic</span></span> |
| <span data-ttu-id="9cd9d-255">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-255">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-256">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-256">N/A</span></span>  |
| <span data-ttu-id="9cd9d-257">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-257">**References**</span></span>              | [<span data-ttu-id="9cd9d-258">Zabezpečený soubor Cookie atribut</span><span class="sxs-lookup"><span data-stu-id="9cd9d-258">Secure Cookie Attribute</span></span>](https://en.wikipedia.org/wiki/HTTP_cookie#Secure_cookie) |
| <span data-ttu-id="9cd9d-259">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-259">**Steps**</span></span> | <span data-ttu-id="9cd9d-260">toomitigate hello riziko zpřístupnění informací s útoku skriptování (XSS), nový atribut - httpOnly - byl přináší toocookies a podporuje všechny hlavní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-260">toomitigate hello risk of information disclosure with a cross-site scripting (XSS) attack, a new attribute - httpOnly - was introduced toocookies and is supported by all major browsers.</span></span> <span data-ttu-id="9cd9d-261">Hello atribut určuje, že soubor cookie není přístupná prostřednictvím skriptu.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-261">hello attribute specifies that a cookie is not accessible through script.</span></span> <span data-ttu-id="9cd9d-262">Webovou aplikaci pomocí souborů cookie HttpOnly snižuje hello možnost, můžete důvěrné informace obsažené v souboru cookie hello odcizení pomocí skriptu a odeslat tooan útočník webu.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-262">By using HttpOnly cookies, a web application reduces hello possibility that sensitive information contained in hello cookie can be stolen via script and sent tooan attacker's website.</span></span> |

### <a name="example"></a><span data-ttu-id="9cd9d-263">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-263">Example</span></span>
<span data-ttu-id="9cd9d-264">Všechny aplikace založené na protokolu HTTP, které používají soubory cookie by měl v definici hello souboru cookie, zadat HttpOnly implementací následující konfigurace v souboru web.config:</span><span class="sxs-lookup"><span data-stu-id="9cd9d-264">All HTTP-based applications that use cookies should specify HttpOnly in hello cookie definition, by implementing following configuration in web.config:</span></span>
```XML
<system.web>
.
.
   <httpCookies requireSSL="false" httpOnlyCookies="true"/>
.
.
</system.web>
```

| <span data-ttu-id="9cd9d-265">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-265">Title</span></span>                   | <span data-ttu-id="9cd9d-266">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-266">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-267">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-267">**Component**</span></span>               | <span data-ttu-id="9cd9d-268">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="9cd9d-268">Web Application</span></span> | 
| <span data-ttu-id="9cd9d-269">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-269">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-270">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-270">Build</span></span> |  
| <span data-ttu-id="9cd9d-271">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-271">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-272">Webové formuláře</span><span class="sxs-lookup"><span data-stu-id="9cd9d-272">Web Forms</span></span> |
| <span data-ttu-id="9cd9d-273">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-273">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-274">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-274">N/A</span></span>  |
| <span data-ttu-id="9cd9d-275">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-275">**References**</span></span>              | [<span data-ttu-id="9cd9d-276">Vlastnost FormsAuthentication.RequireSSL</span><span class="sxs-lookup"><span data-stu-id="9cd9d-276">FormsAuthentication.RequireSSL Property</span></span>](https://msdn.microsoft.com/library/system.web.security.formsauthentication.requiressl.aspx) |
| <span data-ttu-id="9cd9d-277">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-277">**Steps**</span></span> | <span data-ttu-id="9cd9d-278">Hello RequireSSL hodnota vlastnosti je nastavena v hello konfigurační soubor pro aplikaci ASP.NET pomocí hello requireSSL atribut elementu konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-278">hello RequireSSL property value is set in hello configuration file for an ASP.NET application by using hello requireSSL attribute of hello configuration element.</span></span> <span data-ttu-id="9cd9d-279">Zadaný v souboru Web.config hello pro vaši aplikaci ASP.NET zda SSL (Secure Sockets Layer) je požadovaná tooreturn hello ověřování založené na formulářích souboru cookie toohello server atributem requireSSL hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-279">You can specify in hello Web.config file for your ASP.NET application whether SSL (Secure Sockets Layer) is required tooreturn hello forms-authentication cookie toohello server by setting hello requireSSL attribute.</span></span>|

### <a name="example"></a><span data-ttu-id="9cd9d-280">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-280">Example</span></span> 
<span data-ttu-id="9cd9d-281">Hello následující příklad kódu nastaví atribut hello requireSSL v souboru Web.config hello.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-281">hello following code example sets hello requireSSL attribute in hello Web.config file.</span></span>
```XML
<authentication mode="Forms">
  <forms loginUrl="member_login.aspx" cookieless="UseCookies" requireSSL="true"/>
</authentication>
```

| <span data-ttu-id="9cd9d-282">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-282">Title</span></span>                   | <span data-ttu-id="9cd9d-283">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-283">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-284">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-284">**Component**</span></span>               | <span data-ttu-id="9cd9d-285">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="9cd9d-285">Web Application</span></span> | 
| <span data-ttu-id="9cd9d-286">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-286">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-287">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-287">Build</span></span> |  
| <span data-ttu-id="9cd9d-288">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-288">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-289">MVC5</span><span class="sxs-lookup"><span data-stu-id="9cd9d-289">MVC5</span></span> |
| <span data-ttu-id="9cd9d-290">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-290">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-291">EnvironmentType - OnPrem</span><span class="sxs-lookup"><span data-stu-id="9cd9d-291">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="9cd9d-292">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-292">**References**</span></span>              | [<span data-ttu-id="9cd9d-293">Windows Identity Foundation (WIF) konfigurace – část II</span><span class="sxs-lookup"><span data-stu-id="9cd9d-293">Windows Identity Foundation (WIF) Configuration – Part II</span></span>](https://blogs.msdn.microsoft.com/alikl/2011/02/01/windows-identity-foundation-wif-configuration-part-ii-cookiehandler-chunkedcookiehandler-customcookiehandler/) |
| <span data-ttu-id="9cd9d-294">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-294">**Steps**</span></span> | <span data-ttu-id="9cd9d-295">atribut httpOnly tooset pro soubory cookie FedAuth hideFromCsript hodnota atributu musí být nastavená tooTrue.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-295">tooset httpOnly attribute for FedAuth cookies, hideFromCsript attribute value should be set tooTrue.</span></span> |

### <a name="example"></a><span data-ttu-id="9cd9d-296">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-296">Example</span></span>
<span data-ttu-id="9cd9d-297">Následující konfigurace zobrazuje hello správnou konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="9cd9d-297">Following configuration shows hello correct configuration:</span></span>
```XML
<federatedAuthentication>
<cookieHandler mode="Custom"
                       hideFromScript="true"
                       name="FedAuth"
                       path="/"
                       requireSsl="true"
                       persistentSessionLifetime="25">
</cookieHandler>
</federatedAuthentication>
```

## <span data-ttu-id="9cd9d-298"><a id="csrf-asp"></a>Zmírnění před útoky webů požadavku padělání (proti útokům CSRF) na webových stránkách ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9cd9d-298"><a id="csrf-asp"></a>Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET web pages</span></span>

| <span data-ttu-id="9cd9d-299">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-299">Title</span></span>                   | <span data-ttu-id="9cd9d-300">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-300">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-301">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-301">**Component**</span></span>               | <span data-ttu-id="9cd9d-302">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="9cd9d-302">Web Application</span></span> | 
| <span data-ttu-id="9cd9d-303">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-303">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-304">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-304">Build</span></span> |  
| <span data-ttu-id="9cd9d-305">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-305">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-306">Obecné</span><span class="sxs-lookup"><span data-stu-id="9cd9d-306">Generic</span></span> |
| <span data-ttu-id="9cd9d-307">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-307">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-308">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-308">N/A</span></span>  |
| <span data-ttu-id="9cd9d-309">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-309">**References**</span></span>              | <span data-ttu-id="9cd9d-310">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-310">N/A</span></span>  |
| <span data-ttu-id="9cd9d-311">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-311">**Steps**</span></span> | <span data-ttu-id="9cd9d-312">Padělání požadavku posílaného mezi weby (proti útokům CSRF nebo XSRF) je typ útoku, ve kterém může útočník provádět akce v kontextu zabezpečení hello navázanou relaci jiného uživatele na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-312">Cross-site request forgery (CSRF or XSRF) is a type of attack in which an attacker can carry out actions in hello security context of a different user's established session on a web site.</span></span> <span data-ttu-id="9cd9d-313">cílem Hello je toomodify nebo odstraňovat obsah, pokud cílový webový server hello závisí výhradně na relaci soubory cookie tooauthenticate přijatý požadavek.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-313">hello goal is toomodify or delete content, if hello targeted web site relies exclusively on session cookies tooauthenticate received request.</span></span> <span data-ttu-id="9cd9d-314">Útočník by mohl zneužít tuto chybu zabezpečení tím, že získáme jiný uživatel prohlížeče tooload adresu URL pomocí příkazu z citlivé lokality, na kterém je hello uživatel již přihlášen.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-314">An attacker could exploit this vulnerability by getting a different user's browser tooload a URL with a command from a vulnerable site on which hello user is already logged in.</span></span> <span data-ttu-id="9cd9d-315">Existuje mnoho způsobů pro útočník toodo, že jako hostováním na jiný web, načte prostředek z citlivé server hello nebo získávání hello uživatele tooclick odkaz.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-315">There are many ways for an attacker toodo that, such as by hosting a different web site that loads a resource from hello vulnerable server, or getting hello user tooclick a link.</span></span> <span data-ttu-id="9cd9d-316">Pokud hello server odešle klientovi další token toohello, vyžaduje hello tooinclude klienta ve všech budoucích požadavků a ověřuje, že všechny budoucí požadavky patří token, který se týká toohello aktuální relaci, například pomocí se dá zabránit útokům Hello použití hello ASP.NET AntiForgeryToken nebo stavu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-316">hello attack can be prevented if hello server sends an additional token toohello client, requires hello client tooinclude that token in all future requests, and verifies that all future requests include a token that pertains toohello current session, such as by using hello ASP.NET AntiForgeryToken or ViewState.</span></span> |

| <span data-ttu-id="9cd9d-317">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-317">Title</span></span>                   | <span data-ttu-id="9cd9d-318">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-318">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-319">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-319">**Component**</span></span>               | <span data-ttu-id="9cd9d-320">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="9cd9d-320">Web Application</span></span> | 
| <span data-ttu-id="9cd9d-321">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-321">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-322">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-322">Build</span></span> |  
| <span data-ttu-id="9cd9d-323">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-323">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-324">MVC5 MVC6</span><span class="sxs-lookup"><span data-stu-id="9cd9d-324">MVC5, MVC6</span></span> |
| <span data-ttu-id="9cd9d-325">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-325">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-326">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-326">N/A</span></span>  |
| <span data-ttu-id="9cd9d-327">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-327">**References**</span></span>              | [<span data-ttu-id="9cd9d-328">Prevence XSRF/proti útokům CSRF v architektuře ASP.NET MVC a webových stránek</span><span class="sxs-lookup"><span data-stu-id="9cd9d-328">XSRF/CSRF Prevention in ASP.NET MVC and Web Pages</span></span>](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) |
| <span data-ttu-id="9cd9d-329">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-329">**Steps**</span></span> | <span data-ttu-id="9cd9d-330">Anti-proti útokům CSRF a architektura ASP.NET MVC forms - použití hello `AntiForgeryToken` pomocnou metodu pro zobrazení; put `Html.AntiForgeryToken()` do hello formuláře, například</span><span class="sxs-lookup"><span data-stu-id="9cd9d-330">Anti-CSRF and ASP.NET MVC forms - Use hello `AntiForgeryToken` helper method on Views; put an `Html.AntiForgeryToken()` into hello form, for example,</span></span>|

### <a name="example"></a><span data-ttu-id="9cd9d-331">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-331">Example</span></span>
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
```

### <a name="example"></a><span data-ttu-id="9cd9d-332">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-332">Example</span></span>
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a><span data-ttu-id="9cd9d-333">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-333">Example</span></span>
<span data-ttu-id="9cd9d-334">V hello stejný čas, Html.AntiForgeryToken() poskytuje hello návštěvníka __RequestVerificationToken s hello stejnou hodnotu jako hodnota hello náhodných skrytá výše uvedeném názvem souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-334">At hello same time, Html.AntiForgeryToken() gives hello visitor a cookie called __RequestVerificationToken, with hello same value as hello random hidden value shown above.</span></span> <span data-ttu-id="9cd9d-335">Dále toovalidate příchozí post formuláře, přidejte metodu akce cíl toohello filtru hello [ValidateAntiForgeryToken].</span><span class="sxs-lookup"><span data-stu-id="9cd9d-335">Next, toovalidate an incoming form post, add hello [ValidateAntiForgeryToken] filter toohello target action method.</span></span> <span data-ttu-id="9cd9d-336">Například:</span><span class="sxs-lookup"><span data-stu-id="9cd9d-336">For example:</span></span>
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
<span data-ttu-id="9cd9d-337">Filtr autorizace, který kontroluje, zda:</span><span class="sxs-lookup"><span data-stu-id="9cd9d-337">Authorization filter that checks that:</span></span>
* <span data-ttu-id="9cd9d-338">Hello příchozí požadavek má souboru cookie s názvem __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="9cd9d-338">hello incoming request has a cookie called __RequestVerificationToken</span></span>
* <span data-ttu-id="9cd9d-339">Hello příchozí požadavek má `Request.Form` položka názvem __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="9cd9d-339">hello incoming request has a `Request.Form` entry called __RequestVerificationToken</span></span>
* <span data-ttu-id="9cd9d-340">Tyto soubory cookie a `Request.Form` za předpokladu, že všechny shody hodnoty je dobře, hello žádost prochází jako normální.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-340">These cookie and `Request.Form` values match Assuming all is well, hello request goes through as normal.</span></span> <span data-ttu-id="9cd9d-341">Ale pokud ne, pak chybu autorizace se zprávou "požadovaný token proti padělání nebyl zadán nebo byl neplatný".</span><span class="sxs-lookup"><span data-stu-id="9cd9d-341">But if not, then an authorization failure with message “A required anti-forgery token was not supplied or was invalid”.</span></span> 

### <a name="example"></a><span data-ttu-id="9cd9d-342">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-342">Example</span></span>
<span data-ttu-id="9cd9d-343">Ochrana proti útokům CSRF a AJAX: hello tokenu formuláře může být problém pro požadavky AJAX, protože požadavek AJAX může odesílat data JSON, ne data formuláře HTML.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-343">Anti-CSRF and AJAX: hello form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="9cd9d-344">Jedno řešení je toosend hello tokeny ve vlastní hlavičku HTTP.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-344">One solution is toosend hello tokens in a custom HTTP header.</span></span> <span data-ttu-id="9cd9d-345">Hello následující kód používá tokeny hello toogenerate syntaxe Razor a potom přidá požadavek AJAX tooan tokeny hello.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-345">hello following code uses Razor syntax toogenerate hello tokens, and then adds hello tokens tooan AJAX request.</span></span> 
```C#
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }

    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a><span data-ttu-id="9cd9d-346">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-346">Example</span></span>
<span data-ttu-id="9cd9d-347">Když při zpracování požadavku hello, extrahujte hello tokeny z hlavičky požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-347">When you process hello request, extract hello tokens from hello request header.</span></span> <span data-ttu-id="9cd9d-348">Potom volejte metodu AntiForgery.Validate hello toovalidate hello tokeny.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-348">Then call hello AntiForgery.Validate method toovalidate hello tokens.</span></span> <span data-ttu-id="9cd9d-349">Hello metodu Validate vyvolá výjimku, pokud hello tokeny nejsou platné.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-349">hello Validate method throws an exception if hello tokens are not valid.</span></span>
```C#
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

| <span data-ttu-id="9cd9d-350">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-350">Title</span></span>                   | <span data-ttu-id="9cd9d-351">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-351">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-352">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-352">**Component**</span></span>               | <span data-ttu-id="9cd9d-353">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="9cd9d-353">Web Application</span></span> | 
| <span data-ttu-id="9cd9d-354">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-354">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-355">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-355">Build</span></span> |  
| <span data-ttu-id="9cd9d-356">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-356">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-357">Webové formuláře</span><span class="sxs-lookup"><span data-stu-id="9cd9d-357">Web Forms</span></span> |
| <span data-ttu-id="9cd9d-358">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-358">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-359">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-359">N/A</span></span>  |
| <span data-ttu-id="9cd9d-360">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-360">**References**</span></span>              | [<span data-ttu-id="9cd9d-361">Využít výhod z ASP.NET integrované funkce tooFend vypnout útoků Web</span><span class="sxs-lookup"><span data-stu-id="9cd9d-361">Take Advantage of ASP.NET Built-in Features tooFend Off Web Attacks</span></span>](https://msdn.microsoft.com/library/ms972969.aspx#securitybarriers_topic2) |
| <span data-ttu-id="9cd9d-362">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-362">**Steps**</span></span> | <span data-ttu-id="9cd9d-363">Můžete být omezeny útoku proti útokům CSRF v aplikacích webový formulář na základě nastavení tooa ViewStateUserKey náhodný se řetězec, který se liší pro jednotlivé uživatele – ID uživatele nebo lépe ještě ID relace.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-363">CSRF attacks in WebForm based applications can be mitigated by setting ViewStateUserKey tooa random string that varies for each user - user ID or, better yet, session ID.</span></span> <span data-ttu-id="9cd9d-364">Pro počtu technická a sociálních důvodů ID relace je mnohem lepší umístit, protože relace na ID nepředvídatelným, časový limit a liší se na jednotlivé uživatele.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-364">For a number of technical and social reasons, session ID is a much better fit because a session ID is unpredictable, times out, and varies on a per-user basis.</span></span>|

### <a name="example"></a><span data-ttu-id="9cd9d-365">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-365">Example</span></span>
<span data-ttu-id="9cd9d-366">Tady je kód hello potřebujete toohave na všechny stránky:</span><span class="sxs-lookup"><span data-stu-id="9cd9d-366">Here's hello code you need toohave in all of your pages:</span></span>
```C#
void Page_Init (object sender, EventArgs e) {
   ViewStateUserKey = Session.SessionID;
   :
}
```

## <span data-ttu-id="9cd9d-367"><a id="inactivity-lifetime"></a>Nastavení relace dobu jeho existence nečinnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-367"><a id="inactivity-lifetime"></a>Set up session for inactivity lifetime</span></span>

| <span data-ttu-id="9cd9d-368">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-368">Title</span></span>                   | <span data-ttu-id="9cd9d-369">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-369">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-370">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-370">**Component**</span></span>               | <span data-ttu-id="9cd9d-371">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="9cd9d-371">Web Application</span></span> | 
| <span data-ttu-id="9cd9d-372">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-372">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-373">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-373">Build</span></span> |  
| <span data-ttu-id="9cd9d-374">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-374">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-375">Obecné</span><span class="sxs-lookup"><span data-stu-id="9cd9d-375">Generic</span></span> |
| <span data-ttu-id="9cd9d-376">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-376">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-377">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-377">N/A</span></span>  |
| <span data-ttu-id="9cd9d-378">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-378">**References**</span></span>              | <span data-ttu-id="9cd9d-379">[Vlastnost HttpSessionState.Timeout](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="9cd9d-379">[HttpSessionState.Timeout Property](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx)</span></span> |
| <span data-ttu-id="9cd9d-380">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-380">**Steps**</span></span> | <span data-ttu-id="9cd9d-381">Časový limit relace představuje výskytu události hello, pokud uživatel neprovede žádnou akci na webovém serveru během intervalu (definovanou webový server).</span><span class="sxs-lookup"><span data-stu-id="9cd9d-381">Session timeout represents hello event occurring when a user does not perform any action on a web site during a interval (defined by web server).</span></span> <span data-ttu-id="9cd9d-382">Dobrý den událost na straně serveru, změňte stav hello hello uživatelské relace too'invalid "(například" nepoužívá už") a dostane pokyn hello webového serveru toodestroy (odstranění všech dat obsažených do něj).</span><span class="sxs-lookup"><span data-stu-id="9cd9d-382">hello event, on server side, change hello status of hello user session too'invalid' (for example  "not used anymore") and instruct hello web server toodestroy it (deleting all data contained into it).</span></span> <span data-ttu-id="9cd9d-383">Hello následující příklad kódu nastaví hello časový limit relace atributu too15 minut v souboru Web.config hello.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-383">hello following code example sets hello timeout session attribute too15 minutes in hello Web.config file.</span></span>|

### <a name="example"></a><span data-ttu-id="9cd9d-384">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-384">Example</span></span>
<span data-ttu-id="9cd9d-385">"" Kód XML <configuration> < system.web > <sessionState mode="InProc" cookieless="true" timeout="15" /> < /system.web ></configuration></span><span class="sxs-lookup"><span data-stu-id="9cd9d-385">``\`XML code <configuration> <system.web> <sessionState mode="InProc" cookieless="true" timeout="15" /> </system.web> </configuration></span></span>
```

## <a id="threat-detection"></a>Enable Threat detection on Azure SQL
```

| <span data-ttu-id="9cd9d-386">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-386">Title</span></span>                   | <span data-ttu-id="9cd9d-387">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-387">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-388">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-388">**Component**</span></span>               | <span data-ttu-id="9cd9d-389">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="9cd9d-389">Web Application</span></span> | 
| <span data-ttu-id="9cd9d-390">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-390">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-391">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-391">Build</span></span> |  
| <span data-ttu-id="9cd9d-392">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-392">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-393">Webové formuláře</span><span class="sxs-lookup"><span data-stu-id="9cd9d-393">Web Forms</span></span> |
| <span data-ttu-id="9cd9d-394">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-394">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-395">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-395">N/A</span></span>  |
| <span data-ttu-id="9cd9d-396">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-396">**References**</span></span>              | <span data-ttu-id="9cd9d-397">[forms Element pro ověřování (schéma nastavení ASP.NET)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="9cd9d-397">[forms Element for authentication (ASP.NET Settings Schema)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx)</span></span> |
| <span data-ttu-id="9cd9d-398">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-398">**Steps**</span></span> | <span data-ttu-id="9cd9d-399">Nastavení minut hello too15 časový limit souboru cookie lístek pro ověřování pomocí formulářů</span><span class="sxs-lookup"><span data-stu-id="9cd9d-399">Set hello Forms Authentication Ticket cookie timeout too15 minutes</span></span>|

### <a name="example"></a><span data-ttu-id="9cd9d-400">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-400">Example</span></span>
<span data-ttu-id="9cd9d-401">"" Kód XML<forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms></span><span class="sxs-lookup"><span data-stu-id="9cd9d-401">``\`XML code <forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms></span></span>
```

| Title                   | Details      |
| ----------------------- | ------------ |
| **Component**               | Web Application | 
| **SDL Phase**               | Build |  
| **Applicable Technologies** | Web Forms, MVC5 |
| **Attributes**              | EnvironmentType - OnPrem |
| **References**              | [asdeqa](https://skf.azurewebsites.net/Mitigations/Details/wefr) |
| **Steps** | When hello web application is Relying Party and ADFS is hello STS, hello lifetime of hello authentication cookies - FedAuth tokens - can be set by hello following configuration in web.config:|

### Example
```XML
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:15:0" />
      <!-- Set requireHttps=true; -->
      <wsFederation passiveRedirectEnabled="true" issuer="http://localhost:39529/" realm="https://localhost:44302/" reply="https://localhost:44302/" requireHttps="true"/>
      <!--
      Use hello code below tooenable encryption-decryption of claims received from ADFS. Thumbprint value varies based on hello certificate being used.
      <serviceCertificate>
        <certificateReference findValue="4FBBBA33A1D11A9022A5BF3492FF83320007686A" storeLocation="LocalMachine" storeName="My" x509FindType="FindByThumbprint" />
      </serviceCertificate>
      -->
    </federationConfiguration>
  </system.identityModel.services>
```

### <a name="example"></a><span data-ttu-id="9cd9d-402">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-402">Example</span></span>
<span data-ttu-id="9cd9d-403">Hello služby AD FS vydán doba platnosti tokenu SAML deklarace by mělo být nastavené too15 minut také tak, že spustíte následující příkaz prostředí powershell na serveru služby AD FS hello hello:</span><span class="sxs-lookup"><span data-stu-id="9cd9d-403">Also hello ADFS issued SAML claim token's lifetime should be set too15 minutes, by executing hello following powershell command on hello ADFS server:</span></span>
```C#
Set-ADFSRelyingPartyTrust -TargetName “<RelyingPartyWebApp>” -ClaimsProviderName @(“Active Directory”) -TokenLifetime 15 -AlwaysRequireAuthentication $true
```

## <span data-ttu-id="9cd9d-404"><a id="proper-app-logout"></a>Implementace správné odhlášení z aplikace hello</span><span class="sxs-lookup"><span data-stu-id="9cd9d-404"><a id="proper-app-logout"></a>Implement proper logout from hello application</span></span>

| <span data-ttu-id="9cd9d-405">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-405">Title</span></span>                   | <span data-ttu-id="9cd9d-406">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-406">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-407">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-407">**Component**</span></span>               | <span data-ttu-id="9cd9d-408">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="9cd9d-408">Web Application</span></span> | 
| <span data-ttu-id="9cd9d-409">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-409">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-410">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-410">Build</span></span> |  
| <span data-ttu-id="9cd9d-411">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-411">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-412">Obecné</span><span class="sxs-lookup"><span data-stu-id="9cd9d-412">Generic</span></span> |
| <span data-ttu-id="9cd9d-413">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-413">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-414">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-414">N/A</span></span>  |
| <span data-ttu-id="9cd9d-415">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-415">**References**</span></span>              | <span data-ttu-id="9cd9d-416">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-416">N/A</span></span>  |
| <span data-ttu-id="9cd9d-417">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-417">**Steps**</span></span> | <span data-ttu-id="9cd9d-418">Proveďte správné odhlásit z aplikace hello při stisknutí uživatele odhlášení tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-418">Perform proper Sign Out from hello application, when user presses log out button.</span></span> <span data-ttu-id="9cd9d-419">Po odhlášení aplikace by měl destroy uživatelské relace a také resetování a nezruší se tím hodnota souboru cookie relace, spolu s obnovením a anulovány hodnota souboru cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-419">Upon logout, application should destroy user's session, and also reset and nullify session cookie value, along with resetting and nullifying authentication cookie value.</span></span> <span data-ttu-id="9cd9d-420">Navíc pokud více relací jsou vázané tooa identity jednoho uživatele, se musí být souhrnně ukončena na straně serveru hello při vypršení časového limitu nebo odhlášení.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-420">Also, when multiple sessions are tied tooa single user identity, they must be collectively terminated on hello server side at timeout or logout.</span></span> <span data-ttu-id="9cd9d-421">Nakonec zkontrolujte, že odhlášení funkce je k dispozici na každé stránce.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-421">Lastly, ensure that Logout functionality is available on every page.</span></span> |

## <span data-ttu-id="9cd9d-422"><a id="csrf-api"></a>Zmírnění před útoky webů požadavku padělání (proti útokům CSRF) na rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="9cd9d-422"><a id="csrf-api"></a>Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET Web APIs</span></span>

| <span data-ttu-id="9cd9d-423">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-423">Title</span></span>                   | <span data-ttu-id="9cd9d-424">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-424">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-425">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-425">**Component**</span></span>               | <span data-ttu-id="9cd9d-426">Web API</span><span class="sxs-lookup"><span data-stu-id="9cd9d-426">Web API</span></span> | 
| <span data-ttu-id="9cd9d-427">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-427">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-428">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-428">Build</span></span> |  
| <span data-ttu-id="9cd9d-429">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-429">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-430">Obecné</span><span class="sxs-lookup"><span data-stu-id="9cd9d-430">Generic</span></span> |
| <span data-ttu-id="9cd9d-431">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-431">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-432">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-432">N/A</span></span>  |
| <span data-ttu-id="9cd9d-433">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-433">**References**</span></span>              | <span data-ttu-id="9cd9d-434">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-434">N/A</span></span>  |
| <span data-ttu-id="9cd9d-435">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-435">**Steps**</span></span> | <span data-ttu-id="9cd9d-436">Padělání požadavku posílaného mezi weby (proti útokům CSRF nebo XSRF) je typ útoku, ve kterém může útočník provádět akce v kontextu zabezpečení hello navázanou relaci jiného uživatele na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-436">Cross-site request forgery (CSRF or XSRF) is a type of attack in which an attacker can carry out actions in hello security context of a different user's established session on a web site.</span></span> <span data-ttu-id="9cd9d-437">cílem Hello je toomodify nebo odstraňovat obsah, pokud cílový webový server hello závisí výhradně na relaci soubory cookie tooauthenticate přijatý požadavek.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-437">hello goal is toomodify or delete content, if hello targeted web site relies exclusively on session cookies tooauthenticate received request.</span></span> <span data-ttu-id="9cd9d-438">Útočník by mohl zneužít tuto chybu zabezpečení tím, že získáme jiný uživatel prohlížeče tooload adresu URL pomocí příkazu z citlivé lokality, na kterém je hello uživatel již přihlášen.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-438">An attacker could exploit this vulnerability by getting a different user's browser tooload a URL with a command from a vulnerable site on which hello user is already logged in.</span></span> <span data-ttu-id="9cd9d-439">Existuje mnoho způsobů pro útočník toodo, že jako hostováním na jiný web, načte prostředek z citlivé server hello nebo získávání hello uživatele tooclick odkaz.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-439">There are many ways for an attacker toodo that, such as by hosting a different web site that loads a resource from hello vulnerable server, or getting hello user tooclick a link.</span></span> <span data-ttu-id="9cd9d-440">Pokud hello server odešle klientovi další token toohello, vyžaduje hello tooinclude klienta ve všech budoucích požadavků a ověřuje, že všechny budoucí požadavky patří token, který se týká toohello aktuální relaci, například pomocí se dá zabránit útokům Hello použití hello ASP.NET AntiForgeryToken nebo stavu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-440">hello attack can be prevented if hello server sends an additional token toohello client, requires hello client tooinclude that token in all future requests, and verifies that all future requests include a token that pertains toohello current session, such as by using hello ASP.NET AntiForgeryToken or ViewState.</span></span> |

| <span data-ttu-id="9cd9d-441">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-441">Title</span></span>                   | <span data-ttu-id="9cd9d-442">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-442">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-443">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-443">**Component**</span></span>               | <span data-ttu-id="9cd9d-444">Web API</span><span class="sxs-lookup"><span data-stu-id="9cd9d-444">Web API</span></span> | 
| <span data-ttu-id="9cd9d-445">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-445">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-446">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-446">Build</span></span> |  
| <span data-ttu-id="9cd9d-447">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-447">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-448">MVC5 MVC6</span><span class="sxs-lookup"><span data-stu-id="9cd9d-448">MVC5, MVC6</span></span> |
| <span data-ttu-id="9cd9d-449">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-449">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-450">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9cd9d-450">N/A</span></span>  |
| <span data-ttu-id="9cd9d-451">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-451">**References**</span></span>              | [<span data-ttu-id="9cd9d-452">Prevence útoků (proti útokům CSRF) padělání požadavku posílaného mezi weby v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="9cd9d-452">Preventing Cross-Site Request Forgery (CSRF) Attacks in ASP.NET Web API</span></span>](http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) |
| <span data-ttu-id="9cd9d-453">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-453">**Steps**</span></span> | <span data-ttu-id="9cd9d-454">Ochrana proti útokům CSRF a AJAX: hello tokenu formuláře může být problém pro požadavky AJAX, protože požadavek AJAX může odesílat data JSON, ne data formuláře HTML.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-454">Anti-CSRF and AJAX: hello form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="9cd9d-455">Jedno řešení je toosend hello tokeny ve vlastní hlavičku HTTP.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-455">One solution is toosend hello tokens in a custom HTTP header.</span></span> <span data-ttu-id="9cd9d-456">Hello následující kód používá tokeny hello toogenerate syntaxe Razor a potom přidá požadavek AJAX tooan tokeny hello.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-456">hello following code uses Razor syntax toogenerate hello tokens, and then adds hello tokens tooan AJAX request.</span></span> |

### <a name="example"></a><span data-ttu-id="9cd9d-457">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-457">Example</span></span>
```Javascript
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }
    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a><span data-ttu-id="9cd9d-458">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-458">Example</span></span>
<span data-ttu-id="9cd9d-459">Když při zpracování požadavku hello, extrahujte hello tokeny z hlavičky požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-459">When you process hello request, extract hello tokens from hello request header.</span></span> <span data-ttu-id="9cd9d-460">Potom volejte metodu AntiForgery.Validate hello toovalidate hello tokeny.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-460">Then call hello AntiForgery.Validate method toovalidate hello tokens.</span></span> <span data-ttu-id="9cd9d-461">Hello metodu Validate vyvolá výjimku, pokud hello tokeny nejsou platné.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-461">hello Validate method throws an exception if hello tokens are not valid.</span></span>
```C#
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

### <a name="example"></a><span data-ttu-id="9cd9d-462">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-462">Example</span></span>
<span data-ttu-id="9cd9d-463">Anti-proti útokům CSRF a formulářů rozhraní ASP.NET MVC – použití hello pomocnou metodu AntiForgeryToken na zobrazení; Html.AntiForgeryToken() vložena hello formuláře, například</span><span class="sxs-lookup"><span data-stu-id="9cd9d-463">Anti-CSRF and ASP.NET MVC forms - Use hello AntiForgeryToken helper method on Views; put an Html.AntiForgeryToken() into hello form, for example,</span></span>
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
}
```

### <a name="example"></a><span data-ttu-id="9cd9d-464">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-464">Example</span></span>
<span data-ttu-id="9cd9d-465">výše uvedený příklad Hello výstup podobný hello následující:</span><span class="sxs-lookup"><span data-stu-id="9cd9d-465">hello example above will output something like hello following:</span></span>
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a><span data-ttu-id="9cd9d-466">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-466">Example</span></span>
<span data-ttu-id="9cd9d-467">V hello stejný čas, Html.AntiForgeryToken() poskytuje hello návštěvníka __RequestVerificationToken s hello stejnou hodnotu jako hodnota hello náhodných skrytá výše uvedeném názvem souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-467">At hello same time, Html.AntiForgeryToken() gives hello visitor a cookie called __RequestVerificationToken, with hello same value as hello random hidden value shown above.</span></span> <span data-ttu-id="9cd9d-468">Dále toovalidate příchozí post formuláře, přidejte metodu akce cíl toohello filtru hello [ValidateAntiForgeryToken].</span><span class="sxs-lookup"><span data-stu-id="9cd9d-468">Next, toovalidate an incoming form post, add hello [ValidateAntiForgeryToken] filter toohello target action method.</span></span> <span data-ttu-id="9cd9d-469">Například:</span><span class="sxs-lookup"><span data-stu-id="9cd9d-469">For example:</span></span>
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
<span data-ttu-id="9cd9d-470">Filtr autorizace, který kontroluje, zda:</span><span class="sxs-lookup"><span data-stu-id="9cd9d-470">Authorization filter that checks that:</span></span>
* <span data-ttu-id="9cd9d-471">Hello příchozí požadavek má souboru cookie s názvem __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="9cd9d-471">hello incoming request has a cookie called __RequestVerificationToken</span></span>
* <span data-ttu-id="9cd9d-472">Hello příchozí požadavek má `Request.Form` položka názvem __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="9cd9d-472">hello incoming request has a `Request.Form` entry called __RequestVerificationToken</span></span>
* <span data-ttu-id="9cd9d-473">Tyto soubory cookie a `Request.Form` za předpokladu, že všechny shody hodnoty je dobře, hello žádost prochází jako normální.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-473">These cookie and `Request.Form` values match Assuming all is well, hello request goes through as normal.</span></span> <span data-ttu-id="9cd9d-474">Ale pokud ne, pak chybu autorizace se zprávou "požadovaný token proti padělání nebyl zadán nebo byl neplatný".</span><span class="sxs-lookup"><span data-stu-id="9cd9d-474">But if not, then an authorization failure with message “A required anti-forgery token was not supplied or was invalid”.</span></span>

| <span data-ttu-id="9cd9d-475">Název</span><span class="sxs-lookup"><span data-stu-id="9cd9d-475">Title</span></span>                   | <span data-ttu-id="9cd9d-476">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9cd9d-476">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="9cd9d-477">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-477">**Component**</span></span>               | <span data-ttu-id="9cd9d-478">Web API</span><span class="sxs-lookup"><span data-stu-id="9cd9d-478">Web API</span></span> | 
| <span data-ttu-id="9cd9d-479">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-479">**SDL Phase**</span></span>               | <span data-ttu-id="9cd9d-480">Sestavení</span><span class="sxs-lookup"><span data-stu-id="9cd9d-480">Build</span></span> |  
| <span data-ttu-id="9cd9d-481">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-481">**Applicable Technologies**</span></span> | <span data-ttu-id="9cd9d-482">MVC5 MVC6</span><span class="sxs-lookup"><span data-stu-id="9cd9d-482">MVC5, MVC6</span></span> |
| <span data-ttu-id="9cd9d-483">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-483">**Attributes**</span></span>              | <span data-ttu-id="9cd9d-484">Poskytovatel Identity identity zprostředkovatel – AD FS, – Azure AD</span><span class="sxs-lookup"><span data-stu-id="9cd9d-484">Identity Provider - ADFS, Identity Provider - Azure AD</span></span> |
| <span data-ttu-id="9cd9d-485">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-485">**References**</span></span>              | [<span data-ttu-id="9cd9d-486">Zabezpečení webového rozhraní API s jednotlivých účtů a místní přihlášení v rozhraní ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="9cd9d-486">Secure a Web API with Individual Accounts and Local Login in ASP.NET Web API 2.2</span></span>](http://www.asp.net/web-api/overview/security/individual-accounts-in-web-api) |
| <span data-ttu-id="9cd9d-487">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="9cd9d-487">**Steps**</span></span> | <span data-ttu-id="9cd9d-488">Hello webového rozhraní API je zabezpečena pomocí OAuth 2.0, pak se očekává, že token nosiče v požadavku hlavičku a uděluje přístup toohello požadavek ověřování jenom v případě, že hello token je platný.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-488">If hello Web API is secured using OAuth 2.0, then it expects a bearer token in Authorization request header and grants access toohello request only if hello token is valid.</span></span> <span data-ttu-id="9cd9d-489">Na rozdíl od ověřování na základě souborů cookie prohlížeče nepřipojujte toorequests tokenů nosiče hello.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-489">Unlike cookie based authentication, browsers do not attach hello bearer tokens toorequests.</span></span> <span data-ttu-id="9cd9d-490">Hello požadování klient potřebuje tooexplicitly připojit hello nosný token v hlavičce žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-490">hello requesting client needs tooexplicitly attach hello bearer token in hello request header.</span></span> <span data-ttu-id="9cd9d-491">Pro rozhraní ASP.NET Web API chráněné pomocí OAuth 2.0, proto jsou nosné tokeny za obrana proti útokům proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-491">Therefore, for ASP.NET Web APIs protected using OAuth 2.0, bearer tokens are considered as a defense against CSRF attacks.</span></span> <span data-ttu-id="9cd9d-492">Upozorňujeme, že pokud hello MVC část aplikace hello používá ověřování pomocí formulářů (tj, soubory cookie používá), tokeny proti zfalšování mají toobe používané hello MVC webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-492">Please note that if hello MVC portion of hello application uses forms authentication (i.e., uses cookies), anti-forgery tokens have toobe used by hello MVC web app.</span></span> |

### <a name="example"></a><span data-ttu-id="9cd9d-493">Příklad</span><span class="sxs-lookup"><span data-stu-id="9cd9d-493">Example</span></span>
<span data-ttu-id="9cd9d-494">Hello webového rozhraní API má toobe informován toorely pouze na nosné tokeny, nikoli na soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="9cd9d-494">hello Web API has toobe informed toorely ONLY on bearer tokens and not on cookies.</span></span> <span data-ttu-id="9cd9d-495">Je možné ji provést hello následující konfigurace v `WebApiConfig.Register` metoda: '''C ostré kód konfigurace. SuppressDefaultHostAuthentication(); konfigurace. Filters.Add (nové HostAuthenticationFilter(OAuthDefaults.AuthenticationType));</span><span class="sxs-lookup"><span data-stu-id="9cd9d-495">It can be done by hello following configuration in `WebApiConfig.Register` method: ``\`C-Sharp code config.SuppressDefaultHostAuthentication(); config.Filters.Add(new HostAuthenticationFilter(OAuthDefaults.AuthenticationType));</span></span>
```
hello SuppressDefaultHostAuthentication method tells Web API tooignore any authentication that happens before hello request reaches hello Web API pipeline, either by IIS or by OWIN middleware. That way, we can restrict Web API tooauthenticate only using bearer tokens.
